# 说明

因为时间原因，这里就不把教程笔记写进这个笔记里了，就写一些教程笔记里出现的问题和没记录的要点，如果没写说明没什么问题，照着视频学就好了，笔记顺序按照课程顺序。

就麻烦读者两个笔记互相切换看着了

这次使用SpringBoot2.6.7，和视频版本有较大不同。

# 链接

Spring Security 中文文档： https://www.springcloud.cc/spring-security.html#overall-architecture



# 设置账号密码



## 1.设置配置文件

测试后发现只能在 application.properties配置，无法在yaml配置使用

## 2.设置配置类

注意：使用http://localhost:8080/login 后，会出现登陆界面，登陆成功会出现404，这不是你的出错了，只是登录成功，没有设置跳到哪里，就自动跳到404而已（我在这卡了，一度以为自己错了...）

这部分弹幕有争议，我测试了一下

```java
//失败
@Configuration
public class SecurtyConfig extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        BCryptPasswordEncoder passwordEncoder = new BCryptPasswordEncoder();
        String password = passwordEncoder.encode("123");
        auth.inMemoryAuthentication().withUser("123").password(password).roles("admin");
    }
}
```

```java
@Configuration
public class SecurtyConfig extends WebSecurityConfigurerAdapter {
//成功 
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        BCryptPasswordEncoder passwordEncoder = new BCryptPasswordEncoder();
        //加了.passwordEncoder(passwordEncoder) 
        String password = passwordEncoder.encode("123");
		auth.inMemoryAuthentication().passwordEncoder(passwordEncoder)
                                      .withUser("123").password(password).roles("admin");
    }

}
```

```java
//成功
@Configuration
public class SecurtyConfig extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        BCryptPasswordEncoder passwordEncoder = new BCryptPasswordEncoder();
        String password = passwordEncoder.encode("123");
        auth.inMemoryAuthentication().withUser("123").password(password).roles("admin");
    }
//只加了下面的代码
    @Bean
    PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
```

```java
//成功
@Configuration
public class SecurtyConfig extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        BCryptPasswordEncoder passwordEncoder = new BCryptPasswordEncoder();
        String password = passwordEncoder.encode("123");
        //加了.passwordEncoder(passwordEncoder) 
		auth.inMemoryAuthentication().passwordEncoder(passwordEncoder)
                                      .withUser("123").password(password).roles("admin");
    }
//和加了下面的代码
    @Bean
    PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
```

猜测结论：底层有个待实现的加密方法，通过@Bean或者.passwordEncoder()方法，重写这个加密方法，之后就可以正常使用了。没有查资料，只是通过测试猜测是这样的...

## 3.自定义配置实现类

任意用户名+正确的密码=可以登录

要注意，设置的密码是"加密"后的密码，而不是原密码



# 自定义登录页

```java
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        //退出
        http.logout().logoutUrl("/logout").//点击这个链接执行退出
            logoutSuccessUrl("/test/hello").permitAll();//之后跳转到这里

        //配置没有权限访问跳转到页面
        http.exceptionHandling().accessDeniedPage("/unauth.html");
        
        http.formLogin()   //自定义自己编写的登录页面
            .loginPage("/on.html")  //登录页面设置
            .loginProcessingUrl("/user/login")   //登录访问路径
            .defaultSuccessUrl("/success.html").permitAll()  //登录成功之后，跳转路径
                .failureUrl("/unauth.html")
            .and().authorizeRequests()
                .antMatchers("/","/test/hello","/user/login").permitAll() //设置哪些路径可以直接访问，不需要认证
                //当前登录用户，只有具有admins权限才可以访问这个路径
                //1 hasAuthority方法,单权限
               // .antMatchers("/test/index").hasAuthority("admins")
                //2 hasAnyAuthority方法，多权限	
               // .antMatchers("/test/index").hasAnyAuthority("admins,manager")
                //3 hasRole方法  角色的访问控制，单角色
               //在AuthorityUtils.commaSeparatedStringToAuthorityList("ROLE_sale");里写ROLE_sale
                .antMatchers("/test/index").hasRole("sale")
				
                //4.hasAnyRole ([ole1，role2])   用户拥有任意一个指定的角色权限时返回true

                .anyRequest().authenticated()//.anyRequest()创建RequestMatcher后链接的对象。.authenticated()认证，跟anyRequest组合使用，不然报错
            
                .and().rememberMe().tokenRepository(persistentTokenRepository())//记住密码，参数是自定义方法
                .tokenValiditySeconds(60)//设置有效时长，单位秒
                .userDetailsService(userDetailsService);//userDetailsService 自动注入的c
               // .and().csrf().csrfTokenRepository(CookieCsrfTokenRepository.withHttpOnlyFalse());
          // .and().csrf().disable();  //关闭csrf防护
    }
```





# 注解

@Scured(  )主要用来控制角色和权限，简单且常用,要加前缀

@PreAuthorize( "hasAnyAuthority('xxx')" )里面是填access表达式（可在官网文档找到），可以控制的范围更广，hasAnyAuthority可以更换



# 基于数据库的记住我











