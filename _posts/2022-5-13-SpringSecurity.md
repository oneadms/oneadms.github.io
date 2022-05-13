---
layout: post
title: Spring
---

# SpringSecurity使用总结

## WebSecurityConfigurerAdapter配置

```java
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {

//  @Bean
//  PasswordEncoder passwordEncoder() {
//    return NoOpPasswordEncoder.getInstance();
//  }
  @Bean
  public AuthenticationManager authenticationManager() throws Exception {
    return super.authenticationManager();
  }
  @Override
  protected void configure(HttpSecurity http) throws Exception {
    http.authorizeRequests().antMatchers("/login").permitAll().antMatchers("/admin/**")
        .hasRole("admin")
        .antMatchers("/user/**").hasRole("user")
        .anyRequest().authenticated()
        .and()
        .csrf()
        .disable();
  }
  @Bean
  RoleHierarchy roleHierarchy() {
    RoleHierarchyImpl roleHierarchy = new RoleHierarchyImpl();
    roleHierarchy.setHierarchy("ROLE_admin > ROLE_user");
    return roleHierarchy;
  }
}
```

## UserDetailsService配置loadUserByUsername，UserDetailsPasswordService配置updatePassword 

updatePassword 可以根据将旧密码的加密规则修改成新密码的加密规则 如果是同一个加密规则 可以升级不可以降级 如 bcrypt

配置



```java

@Service
public class UserService implements UserDetailsService , UserDetailsPasswordService {
@Autowired
  private UserMapper userMapper;
  @Override
  public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
    User user = userMapper.getUserByUsername(username);
    if (user == null) {
      throw new UsernameNotFoundException("该账号不存在");
    }
    user.setRoles(userMapper.getUserRolesByUid(user.getId()));
    return user;
  }

  @Override
  public UserDetails updatePassword(UserDetails user, String newPassword)  {
    Integer res = userMapper.updatePasswordByUsername(user.getUsername(), newPassword);
    User resUser = (User) user;
    if (res > 0) {
      resUser.setPassword(newPassword);
    }

    return resUser;
  }
}
```

# UserService配置

```java
public class User implements UserDetails {
  private Integer id;
  private String username;
  private List<Role> roles;

  public Integer getId() {
    return id;
  }

  public void setId(Integer id) {
    this.id = id;
  }

  public void setUsername(String username) {
    this.username = username;
  }

  public String getNickname() {
    return nickname;
  }

  public void setNickname(String nickname) {
    this.nickname = nickname;
  }

  public void setPassword(String password) {
    this.password = password;
  }

  public Boolean getEnabled() {
    return enabled;
  }

  public void setEnabled(Boolean enabled) {
    this.enabled = enabled;
  }

  public Integer getRole() {
    return role;
  }

  public void setRole(Integer role) {
    this.role = role;
  }

  private String nickname;
  private String password;
  private Boolean enabled;
  private Integer role;

  /**
   * 获取权限
   * @return
   */
  @Override
  public Collection<? extends GrantedAuthority> getAuthorities() {
    return roles.stream().map(r -> new SimpleGrantedAuthority(r.getName())).collect(Collectors.toList());
  }

  @Override
  public String getPassword() {
    return password;
  }

  @Override
  public String getUsername() {
    return username;
  }

  @Override
  public boolean isAccountNonExpired() {
    return true;
  }

  @Override
  public boolean isAccountNonLocked() {
    return true;
  }

  @Override
  public boolean isCredentialsNonExpired() {
    return true;
  }

  @Override
  public boolean isEnabled() {
    return enabled;
  }

  public void setRoles(List<Role> roles) {
    this.roles = roles;
  }
}
```

## 自定义登录

```java
 @RequestMapping("/login")
  public Map<String, Object> login(@RequestBody User user) {
    Map<String, Object> map = new HashMap<>();
    UsernamePasswordAuthenticationToken token = new UsernamePasswordAuthenticationToken(
        user.getUsername(), user.getPassword());
    try {
      Authentication authenticate = authenticationManager.authenticate(token);
      SecurityContextHolder.getContext().setAuthentication(authenticate);
      map.put("code", 200);
      map.put("msg", "登录成功");


    }
    catch (AuthenticationException e) {
      map.put("code", -1);
      if (e instanceof UsernameNotFoundException) {
//出于安全考虑，Spring Security 内部将 UsernameNotFoundException 隐藏起来了，转而抛出了 BadCredentialsException
      } else if (e instanceof BadCredentialsException) {
        map.put("message", "用户名或者密码写错，登录失败");
      } else if (e instanceof AccountExpiredException) {

        map.put("message", "账户过期，登录失败");
      } else if (e instanceof CredentialsExpiredException) {
        map.put("message", "密码过期，登录失败");
      } else if (e instanceof DisabledException) {
        map.put("message", "账户被禁用，登录失败");
      } else if (e instanceof LockedException) {
        map.put("message", "账户被锁定，登录失败");
      }
      }


    return map;
  }
}
```

# 注意点

数据库表设计 name 前面必须要 以：

>  ROLE_  开头为角色字段

![image-20220513205350287](https://raw.githubusercontent.com/oneadms/blog_picture/main/image-20220513205350287.png)



同时配置角色继承关系也是如此

![image-20220513205449183](https://raw.githubusercontent.com/oneadms/blog_picture/main/image-20220513205449183.png)

拦截规则 除外

![image-20220513205531445](https://raw.githubusercontent.com/oneadms/blog_picture/main/image-20220513205531445.png)