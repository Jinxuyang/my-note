[TOC]

## Spring Security原理

![](https://gitee.com/vergeee/static-repo/raw/master//img/20210407183614.png)

绿：检查请求中是否包含这些信息

蓝：处理异常

橙：决定该请求是否能访问到服务

具体看别人的博客

## 自定义登录

### 配置

继承`WebSecurityConfigurerAdapter`

以下是我的配置

```java
@Component
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
    @Autowired
    private ValidateCodeFilter validateCodeFilter;

    @Autowired
    private RestfulAuthenticationFailureHandler restfulAuthenticationFailureHandler;

    @Autowired
    private RestfulAuthenticationSuccessHandler restfulAuthenticationSuccessHandler;

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        // 为validateCodeFilter设置失败处理类，下面会讲到
        validateCodeFilter.setAuthenticationFailureHandler(restfulAuthenticationFailureHandler);
        // 添加validateCodeFilter在UsernamePasswordAuthenticationFilter之前
        http.addFilterBefore(validateCodeFilter, UsernamePasswordAuthenticationFilter.class);
        // 表单登录设置
        http.formLogin()
                .loginProcessingUrl("/login")
                .successHandler(restfulAuthenticationSuccessHandler)
                .failureHandler(restfulAuthenticationFailureHandler);
        // 放行
        http.authorizeRequests()
                .antMatchers("/code/image","/login")
                .permitAll();
        http.csrf().disable();
    }
}
```

### 加载用户信息逻辑

实现UserDetailService

```java
public interface UserDetailsService {
	UserDetails loadUserByUsername(String username) throws UsernameNotFoundException;
}
```

实现loadUserByUsername()方法，

一般逻辑为：

1. 从数据库加载用户信息

2. 构造一个UserDetails，这个UserDetails是一个接口你可以自己实现也可以用`org.springframework.security.core.userdetails`包下的User

   ```java
   public interface UserDetails extends Serializable {
   	
   	Collection<? extends GrantedAuthority> getAuthorities();
   
   	String getPassword();
   	
   	String getUsername();
   	
   	boolean isAccountNonExpired();
   
   	boolean isAccountNonLocked();
   	
   	boolean isCredentialsNonExpired();
   
   	boolean isEnabled();
   }
   ```

   其中包含权限，用户名，密码和四个boolean，四个boolean有一个不是true就会抛出异常

3. 将你自己实现的`UserDetailsService`配到配置文件中

   ```java
   // 写在上面的配置文件之后即可
   http.userDetailsService(yourUserDetailsService)
   ```

4. 完成

### 密码加密

一般情况下数据库内不会存放明文的密码，Spring中一般使用`PasswordEncoder`对密码进行加密和比对，Spring也提供了各种实现类采用不同算法



### 自定义登录页面

在配置中配置 

```java
http.formLogin()
    .loginPage() //登录页面
    .loginProcessingUrl() //处理登录的接口
```

### 自定义登录成功和失败处理

 实现`AuthenticationSuccessHandler`接口并在配置文件中修改

```java
http.formLogin()
                .successHandler(yourhandler)
                .failureHandler(yourhandler);
```



```java
public interface AuthenticationSuccessHandler {
   default void onAuthenticationSuccess(HttpServletRequest request,
         HttpServletResponse response, FilterChain chain, Authentication authentication)
         throws IOException, ServletException{
      onAuthenticationSuccess(request, response, authentication);
      chain.doFilter(request, response);
   }

   void onAuthenticationSuccess(HttpServletRequest request,
         HttpServletResponse response, Authentication authentication)
         throws IOException, ServletException;

}
```

authentication内包含了验证的各种信息

实现`AuthenticationFailureHandler`接口

```java
public interface AuthenticationFailureHandler {
   void onAuthenticationFailure(HttpServletRequest request,
         HttpServletResponse response, AuthenticationException exception)
         throws IOException, ServletException;
}
```

## 图片验证码功能



![](https://gitee.com/vergeee/static-repo/raw/master//img/20210407183614.png)

在UserPasswordAuthenticationFilter 加上一个校验 验证码的过滤器即可，验证通过继续进入下一个过滤器，失败则调用AuthenticationFailureHandler

### 写一个接口用于获取验证码图片

```java
@RestController
public class ValidateCodeController {

    @Autowired
    private ImageCodeService imageCodeService;

    /**
     * 获取图形验证码的图
     * @param uuid 用户提供一个uuid，用于绑定正确的验证码，登陆时需要提交这个uuid
     */
    @GetMapping("/code/image")
    public void createCode(HttpServletResponse response,String uuid) throws IOException {
        response.setHeader("Cache-Control", "no-store, no-cache");
        response.setContentType("image/jpeg");

        // 生成图片，逻辑在下面
        BufferedImage image = imageCodeService.generateImageCode(uuid);

        ServletOutputStream out = response.getOutputStream();
        ImageIO.write(image, "jpg", out);
        out.close();
    }
}
```

### 根据随机数生成验证码

```java
@Autowired
private RedisTemplate<String,Object> redisTemplate

public BufferedImage generateImageCode(String uuid) {
    
    // 生成验证码
    int width = 60;
    int height = 20;
    BufferedImage image = new BufferedImage(width,height,BufferedImage.TYPE_INT_RGB);
    Graphics g = image.getGraphics();
    String chars="0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ";
    char[] rands=new char[4];
    for(int i=0;i<4;i++)
    {
        int rand=(int)(Math.random()*36);
        rands[i]=chars.charAt(rand);
    }
    g.setColor(new Color(0xDCDCDC));
    g.fillRect(0, 0, width, height);
    for(int i=0;i<120;i++)
    {
        int x=(int)(Math.random()*width);
        int y=(int)(Math.random()*height);
        int red=(int)(Math.random()*255);
        int green=(int)(Math.random()*255);
        int blue=(int)(Math.random()*255);
        g.setColor(new Color(red,green,blue));
        g.drawOval(x, y, 1, 0);
    }
    g.setColor(Color.BLACK);
    g.setFont(new Font(null, Font.ITALIC|Font.BOLD,18));
    
    g.drawString(""+rands[0], 1, 17);
    g.drawString(""+rands[1], 16, 15);
    g.drawString(""+rands[2], 31, 18);
    g.drawString(""+rands[3], 46, 16);

    // 将验证码信息存入redis
    String code = String.copyValueOf(rands);
    redisTemplate.opsForValue().set(RedisConstant.UUID_CODE+uuid, code, 6000, TimeUnit.SECONDS);

    return image;
}
```

### 通过过滤器将验证码与用户提交的进行对比

在校验过程中出现错误时需要抛出异常，自定义`ValidateCodeException`

```java
public class ValidateCodeException extends AuthenticationException {
    public ValidateCodeException(String msg) {
        super(msg);
    }
}
```

`AuthenticationException`为所有异常的的父类

```java
@Component
public class ValidateCodeFilter extends OncePerRequestFilter {
    @Autowired
    private RedisTemplate<String,Object> redisTemplate;

    @Autowired
    private AuthenticationFailureHandler authenticationFailureHandler;

    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain) throws ServletException, IOException {
        // uri和请求方法是否符合要求
        if ("/login".equals(request.getRequestURI()) && "POST".equals(request.getMethod())){
            try {
                // 校验验证码
                validate(request);
            } catch (ValidateCodeException e){
                authenticationFailureHandler.onAuthenticationFailure(request,response,e);
                return;
            }
        }
        filterChain.doFilter(request,response);
    }

    private void validate(HttpServletRequest request) throws ServletRequestBindingException {
        // 从请求中获取uuid 和 用户提交的验证码
        String uuid = ServletRequestUtils.getStringParameter(request,"uuid");
        String userCode = ServletRequestUtils.getStringParameter(request,"code");
        if (userCode == null) {
            throw new ValidateCodeException("验证码不能为空");
        }

        // 从redis中取出正确的验证码
        String code = (String) redisTemplate.opsForValue().get(RedisConstant.UUID_CODE+uuid);
        if (code == null){
            throw new ValidateCodeException("验证码已过期，请刷新");
        }

        if (!code.equalsIgnoreCase(userCode)){
            throw new ValidateCodeException("验证码不匹配");
        }
		
        // 校验完成删除该验证码
        redisTemplate.delete(RedisConstant.UUID_CODE+uuid);
    }

    public AuthenticationFailureHandler getAuthenticationFailureHandler() {
        return authenticationFailureHandler;
    }

    public void setAuthenticationFailureHandler(AuthenticationFailureHandler authenticationFailureHandler) {
        this.authenticationFailureHandler = authenticationFailureHandler;
    }
}
```

### 在过滤器链上加入校验验证码的过滤器

放在`UsernamePasswordAuthenticationFilter`前

配置

```java
@Component
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
    @Autowired
    private ValidateCodeFilter validateCodeFilter;
    
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.addFilterBefore(validateCodeFilter, UsernamePasswordAuthenticationFilter.class);
    }
}
```





## 短信验证码登录

### 生成随机数并发送给用户

### 存储手机号和验证码

### 通过验证码登录

![img](https://gitee.com/vergeee/static-repo/raw/master//img/20210408202501.png)

#### SmsAuthenticationToken

```java
/**
 * 模仿UsernamePasswordAuthenticationToken实现SmsAuthenticationToken
 * @Author Verge
 * @Date 2021/4/8 20:28
 * @Version 1.0
 */
public class SmsAuthenticationToken extends AbstractAuthenticationToken {

    private final Object principal;

    /**
     * 构造一个未经验证的SmsAuthenticationToken
     * @param mobile 手机号
     */
    public SmsAuthenticationToken(String mobile) {
        super(null);
        this.principal = mobile;
        setAuthenticated(false);
    }

    /**
     * 构造一个经过验证的SmsAuthenticationToken
     * @param principal 用户信息
     * @param authorities 用户权限
     */
    public SmsAuthenticationToken(Object principal, Collection<? extends GrantedAuthority> authorities) {
        super(authorities);
        this.principal = principal;
        super.setAuthenticated(true);
    }

    @Override
    public Object getCredentials() {
        return null;
    }

    @Override
    public Object getPrincipal() {
        return this.principal;
    }
}
```

#### SmsAuthenticationFilter

拦截短信验证码登录请求组装SmsAuthenticationToken

```java
/**
 * 模仿UsernamePasswordAuthenticationFilter
 * @Author Verge
 * @Date 2021/4/8 20:38
 * @Version 1.0
 */
public class SmsAuthenticationFilter extends AbstractAuthenticationProcessingFilter {
    public static final String SPRING_SECURITY_FORM_MOBILE_KEY = "username";

    private static final AntPathRequestMatcher DEFAULT_ANT_PATH_REQUEST_MATCHER = new AntPathRequestMatcher("/login/mobile", "POST");

    private String mobileParameter = SPRING_SECURITY_FORM_MOBILE_KEY;

    private boolean postOnly = true;

    public SmsAuthenticationFilter() {
        super(DEFAULT_ANT_PATH_REQUEST_MATCHER);
    }

    public SmsAuthenticationFilter(AuthenticationManager authenticationManager) {
        super(DEFAULT_ANT_PATH_REQUEST_MATCHER, authenticationManager);
    }

    @Override
    public Authentication attemptAuthentication(HttpServletRequest request, HttpServletResponse response)
            throws AuthenticationException {
        // 校验请求是否为POST
        if (this.postOnly && !request.getMethod().equals("POST")) {
            throw new AuthenticationServiceException("Authentication method not supported: " + request.getMethod());
        }

        String mobile = obtainMobile(request);
        mobile = (mobile != null) ? mobile : "";
        mobile = mobile.trim();

        SmsAuthenticationToken authRequest = new SmsAuthenticationToken(mobile);

        setDetails(request, authRequest);
        return this.getAuthenticationManager().authenticate(authRequest);
    }

    /**
     * 从request中获取手机号
     * @param request 请求信息
     * @return
     */
    @Nullable
    protected String obtainMobile(HttpServletRequest request) {
        return request.getParameter(this.mobileParameter);
    }

    /**
     * 将请求信息存入SmsAuthenticationToken
     * @param request 请求信息
     * @param authRequest SmsAuthenticationToken
     */
    protected void setDetails(HttpServletRequest request, SmsAuthenticationToken authRequest) {
        authRequest.setDetails(this.authenticationDetailsSource.buildDetails(request));
    }

    public void setPostOnly(boolean postOnly) {
        this.postOnly = postOnly;
    }

    public void setMobileParameter(String mobileParameter) {
        this.mobileParameter = mobileParameter;
    }
}
```

#### SmsAuthenticationProvider

```java
/**
 * @Author Verge
 * @Date 2021/4/8 20:57
 * @Version 1.0
 */
public class SmsAuthenticationProvider implements AuthenticationProvider {

    private UserDetailsService userDetailsService;
    
    /**
     * 身份认证逻辑
     * @param authentication 未经校验的authentication
     * @return 通过校验的authentication
     * @throws AuthenticationException 校验失败抛出异常
     */
    @Override
    public Authentication authenticate(Authentication authentication) throws AuthenticationException {
        SmsAuthenticationToken authenticationToken = (SmsAuthenticationToken) authentication;

        // 根据手机号读取用户信息
        UserDetails user = userDetailsService.loadUserByUsername((String)authenticationToken.getPrincipal());
        /*
        *  校验逻辑：
        *   成功返回经过校验的Authentication,并复制原authentication的Details信息
        *   否则抛出AuthenticationException
        * 
        * */
        return null;
    }

    /**
     * 判断该provider是否支持校验此authentication
     * @param authentication 传进来的authentication
     * @return 该provider是否支持校验此authentication
     */
    @Override
    public boolean supports(Class<?> authentication) {
        //判断authentication是不是SmsAuthenticationToken这个类型
        return SmsAuthenticationToken.class.isAssignableFrom(authentication);
    }
}
```

