# Spring security

创建`/config/MyWebConfig.java`

```java
@Configuration
public class MyWebConfig implements WebMvcConfigurer {

    /**
     * UserDetailsService 需要这个类进行密码校验。
     */
    @Bean
    public PasswordEncoder passwordEncoder() {
        //return new BCryptPasswordEncoder();
        // 密码加密方式，这里选择不加密
        return NoOpPasswordEncoder.getInstance();
    }

    @Bean
    public UserDetailsService userDetailsService() {
        return new InMemoryUserDetailsManager(
                User.withUsername("admin").password("admin").authorities("admin").build(),
                User.withUsername("user").password("user").authorities("user").build()
        );
    }
}

```

创建`/config/MyWebSecurityConfig.java`

```java
@Configuration
public class MyWebSecurityConfig extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        // 关闭跨域请求伪造(Cross-Site Request Forger)
        http.csrf().disable();
        // 自定义登陆表单
        http.formLogin()
                // 修改表单处理的链接
                .loginProcessingUrl("/login");
        // 授权行为
        http.authorizeRequests()
                // 使某人有权利访问以ant路径匹配模式的链接
                //.antMatchers("/admin/**").hasAuthority("admin")
                // 所有请求都需要授权
                //.anyRequest().authenticated()
                // 允许所有人的任何请求
                // anyRequest()放在最后，应为拦截自上向下执行
                .anyRequest().permitAll();

    }
}
```

url的匹配规则：

- ant：ant-style匹配
- regex：正则匹配