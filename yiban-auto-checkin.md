---
title: 我是如何实现易班自动打卡的？
date: 2020-07-28 14:01:36
tags:
	- spring
	- java
categories: 
	- 项目
---

需要用到的工具

HttpCanary

## 抓包

1. 先进入易班，然后退出登录，方便我们一会记录整个登录和打卡的过程

2. 打开HttpCanary将易班添加为目标程

3. 开始抓包

4. 完成从登录易班到打卡完成的整个过程的抓包

   <!--more-->

## 分析

我们倒着来分析

1. 抓到了很多数据包，但是音频和图片js...文件是我们不需要的，因此进行过滤保留这三种数据包即可

2. 开始一个一个分析，发现一个数据包很可疑，打开一看这不就是我们提交的表单吗

3. 验证猜想，重发这个请求，返回消息如下，可以确定这就是我们要找的数据包

4. 猜想是否可以依靠每天自动重发这个请求实现签到，答案是否定的，是什么原因呢

   1. 分析该请求的请求头，发现这里有一个Cookie，有两个字段

      ```
      client=android
      PHPSESSID=96098a2d53fdfef072b13f7113da5e17
      ```

   2. 第一个不用说，第二个就是罪魁祸首，一看这名字就知道肯定和SESSION有关，百度一下

5. 好了这下目标明确，寻找是谁第一次Set了这个Cookie，筛选，好了就是他，理论上我们只要，发送一下这个请求就可以拿到Cookie

6. 问题又来了，这些又是啥？？？继续往下看

7. 首先猜测一下这个act=iappxxxxx应该是易班小程序的ID，那么OK，他是不会变的，记住就行了，那么这个v是什么？看了看他的前一个请求发现就是access_token

8. 好了继续寻找access_token

9. 拉到最下面，第一个请求应该就是登录的请求，查看他的响应信息果然access_token就在里面

10. 至此，抓包数据分析完成

**总结：**

1. 登录拿到access_token
2. 拿着access_token去获得Cookie
3. 拿着Cookie提交表单
4. 完成

## 代码

**以下使用java，使用Hutool进行Http请求、json解析等等等**

获取Access_token

```java
public static CommonResult getAccess_token(String username, String password){
        //设置请求参数
        HashMap<String,Object> map = new HashMap<>();
        map.put("account",username);
        map.put("passwd",password);
        map.put("ct",1);
        map.put("identify",0);
        map.put("v","4.7.4");

        //获取返回的字符串
        String jsonStr = HttpUtil.get("https://mobile.yiban.cn/api/v2/passport/login",map);

        //把获取到的字符串转换为JSONObject对象
        JSONObject jsonObject = JSONUtil.parseObj(jsonStr);

        //try catch一下，以防获取到一些奇奇怪怪的东西
        try {
            //比较状态码，是否登陆成功
            if (jsonObject.get("response").equals("100")){
                String res = ((JSONObject) jsonObject.get("data")).getStr("access_token");
                return CommonResult.success(res,"拿到数据了");
            } else {
                return CommonResult.failed("登录失败");
            }
        } catch (Exception e){
            e.printStackTrace();
            return CommonResult.failed("易班返回的Json数据可能有变化");
        }
    }
```

获取Cookie

```java
public static CommonResult getPHPSESSID(String access_token){
        //请求数据
        HashMap<String,Object> map = new HashMap<>();
        map.put("act","iapp610661");
        map.put("access_token",access_token);

        //发送请求，从cookie中获取PHPSESSIONID
        HttpResponse response = HttpRequest
                .get("http://f.yiban.cn/iapp/index")
                .form(map)
                .setFollowRedirects(true)
                .execute();
        try {
            String res = response.header("Set-Cookie");
            return CommonResult.success(res,"成功获取SESSID");
        } catch (NullPointerException e) {
            System.out.println(response.toString());
            return CommonResult.failed("没获取到SESSID");
        }
    }
```

提交表单

```java
public static CommonResult submit(String cookie, String content){
        HttpResponse response = HttpRequest.post("http://yiban.sust.edu.cn/v4/public/index.php/Index/form/add.html?id=9")
                .contentType("application/x-www-form-urlencoded")
                .body(content)
                .cookie(cookie)
                .execute();
        JSONObject jsonObject = JSONUtil.parseObj(response.body());
        try {
            if (jsonObject.get("code").equals("1")){
                return CommonResult.success(jsonObject.getStr("msg"));
            } else {
                return CommonResult.failed(jsonObject.getStr("msg"));
            }
        } catch (NullPointerException e){
            return CommonResult.failed(e.getMessage()+  "没找到返回的code，奇了怪了");
        }
    }
```

发送邮件(不得不说Hutool就是好用)

```java
MailUtil.send("你的邮箱",标题,内容,是否为html(boolean));
```

