### 1、Data

### 2、Mybatis

**整合包**
		mybatis-spring-boot-starter



1. 导入包
2. 配置文件
3. mybatis配置
4. 编写sql
5. service层调用dao层
6. controller 调用 service层

### 3、SpringSecurity（安全）

**简介**

Spring Security是针对Spring项目的安全框架，也是Spring Boot底层安全模块默认的技术选型，他可以实现强大的Web安全控制，对于安全控制，我们仅需要引入spring-boot-starter-security模块，进行少量的配置，即可实现强大的安全管理！

记住几个类：

- WebSecurityConfigurerAdapter：自定义Security策略
- AuthenticationManagerBuilder：自定义认证策略
- @EnableWebSecurity：开启WebSecurity模式

Spring Security的两个主要目标是“认证”和“授权”（访问控制）。

“认证"（Authentication）

“授权”（Authorization）

这个概念是通用的，而不是只在Spring Security中存在。
参考官网：https://spring.io/projects/spring-security，查看我们自己项目中的版本，找到对应的帮助文档：
https://docs.spring.io/spring-security/site/docs/5.2.0.RELEASE/reference/htmlsingle