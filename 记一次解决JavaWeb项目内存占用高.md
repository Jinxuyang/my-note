### 起因

无意中发现服务器内存几乎没有了

![image-20211114144255580](%E8%AE%B0%E4%B8%80%E6%AC%A1%E8%A7%A3%E5%86%B3JavaWeb%E9%A1%B9%E7%9B%AE%E5%86%85%E5%AD%98%E5%8D%A0%E7%94%A8%E9%AB%98.assets/image-20211114144255580.png)

## 排除

使用top命令查看内存情况

默认显示单位是KB可以按E进行切换

https://cloud.tencent.com/developer/article/1704223

![image-20211114144729411](%E8%AE%B0%E4%B8%80%E6%AC%A1%E8%A7%A3%E5%86%B3JavaWeb%E9%A1%B9%E7%9B%AE%E5%86%85%E5%AD%98%E5%8D%A0%E7%94%A8%E9%AB%98.assets/image-20211114144729411.png)



由于我的服务器上只起了一个Java项目可以直接定位到问题