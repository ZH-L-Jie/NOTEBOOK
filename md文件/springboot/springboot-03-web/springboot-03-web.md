springboot里面有web的文件，需知道之前放在web文件夹的文件放在哪里。

### 1、放置静态资源

在WebMvcAutoConfiguration类的addResourceHandlers（）方法

知：

1.在springboot，我们可以使用以下方式处理静态资源

-  webjars 	localhost:8080/webjars/

- public，static，/**，resources      localhost:8080/

  resources >static>public

```java
public void addResourceHandlers(ResourceHandlerRegistry registry) {
    if (!this.resourceProperties.isAddMappings()) {
        logger.debug(Default resource handling disabled);
        return;
    }
    Duration cachePeriod = this.resourceProperties.getCache().getPeriod();
    CacheControl cacheControl = this.resourceProperties.getCache().getCachecontrol().toHttpCacheControl();
    if (!registry.hasMappingForPattern(webjars)) {
        customizeResourceHandlerRegistration(registry.addResourceHandler(webjars)
                .addResourceLocations(classpathMETA-INFresourceswebjars)
                .setCachePeriod(getSeconds(cachePeriod)).setCacheControl(cacheControl));
    }
    String staticPathPattern = this.mvcProperties.getStaticPathPattern();
    if (!registry.hasMappingForPattern(staticPathPattern)) {
        customizeResourceHandlerRegistration(registry.addResourceHandler(staticPathPattern)
                .addResourceLocations(getResourceLocations(this.resourceProperties.getStaticLocations()))
                .setCachePeriod(getSeconds(cachePeriod)).setCacheControl(cacheControl));
    }
}
```

![静态资源放置路径](E:\笔记\md文件\springboot\springboot-03-web\静态资源放置路径.png)

![符合静态资源路径](E:\笔记\md文件\springboot\springboot-03-web\符合静态资源路径.png)

在配置中加入下面，访问静态资源路径为localhost：8080/aaa/1.js

```properties
spring.mvc.static-path-pattern=/aaa/**
```

### 2、首页放置位置

templates目录下的所有页面，只能通过controller来跳转！（相当于之前的WEB_INF）

==有模板引擎才行==

### 3、修改页面的小图标

直接把放进静态扫描文件夹就可以修改网页的小图标（原来是个小叶子）

配置文件写下，只是表明不是用系统默认小叶子的图标

```properties
spring.mvc.favicon.enabled=false
```

==PS:在2.2.xx版本设置小图标的函数已经移除，直接加入favicon.ico在静态资源文件夹即可，2.2.xx之前的版本，也可直接加入favicon.ico即可，也可以关闭原来的小叶子==

### 4、thymeleaf

springboot的[thymeleaf](https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#using-texts)

**后来加上**

注意：thymeleaf 2.x的版本可能报错  与springboot版本不匹配   3.x就好了。新版的springboot直接合适的thymeleaf

```xml
<!--thymeleaf-->
        <dependency>
            <groupId>org.thymeleaf</groupId>
            <artifactId>thymeleaf-spring5</artifactId>
        </dependency>
        <dependency>
            <groupId>org.thymeleaf.extras</groupId>
            <artifactId>thymeleaf-extras-java8time</artifactId>
        </dependency>
```

**模板引擎**

结论：只要需要使用thymeleaf，只需要导入对应的依赖就可以了！我们将html放在我们的templates目录下

```java
@ConfigurationProperties(prefix = "spring.thymeleaf")
public class ThymeleafProperties {

	private static final Charset DEFAULT_ENCODING = StandardCharsets.UTF_8;

	public static final String DEFAULT_PREFIX = "classpath:/templates/";

	public static final String DEFAULT_SUFFIX = ".html";
}
```



### 5、扩展Springmvc

[参考官方文档](https://docs.spring.io/spring-boot/docs/2.2.6.RELEASE/reference/html/spring-boot-features.html#boot-features-developing-web-applications)

![自动配置mvc步骤](E:\笔记\md文件\springboot\springboot-03-web\自动配置mvc步骤.png)

在原有的mvc加功能

![原来的mvc加功能](E:\笔记\md文件\springboot\springboot-03-web\原来的mvc加功能.png)

**原有的功能上不可加@EnableWebMvc**

![](E:\笔记\md文件\springboot\springboot-03-web\WebMvcAutoConfiguration.png)

**@ConditionOnBean**在判断list的时候，如果list没有值，返回false，否则返回true

**@ConditionOnMissingBean**在判断list的时候，如果list没有值，返回true，否则返回false，其他逻辑都一样

![](E:\笔记\md文件\springboot\springboot-03-web\@EnableWebMvc.png)



![](E:\笔记\md文件\springboot\springboot-03-web\DelegatingWebMvcConfiguration.png)

加入会导致WebMVCAutoConfiguration类失效

在springboot中，有非常多的xxxxConfiguration帮助我们进行扩展配置，只要看见了这个东西，我们就要注意了！

### 6、页面国际化

**1.首页配置：**

1. 注意点，所有页面的静态资源都需要使用thymeleaf接管；
2. url:l@{}

**2.页面国际化：**

1.我们需要配置i18n文件（ internationalization，中间18个字母 ）

2.我们如果需要在项目中进行按钮自动切换，我们需要自定义一个组件LocaleResolver

3.记得将自己写的组件配置到spring容器@Bean

4.#{}

PS：**每个请求都会进入组件LocaleResolver**

### 7、自我开发步骤

1. 参考官方文档，一般会有一些具体的步骤或一些思路
2. 看源码，查看配置文件（xxxProperties），和准备实现功能的重发方法的思路
3. 寻找博客，看一般的设想思路



### 8、 lang="en"

可能会发现编辑器默认生成的格式是<html lang="en"></html>,其实这也是有原因的，lang是language的缩写，代表该网页属于哪一个国家或是地区的网页。下面列举几个常用lang：

- **en-US 英国(美国)**

- **zh-CN 中文(简体，中国大陆) 国内版** 

- **zh-SG 中文(简体，新加坡)**

- **zh-HK 中文(繁体，香港)**

- **zh-MO 中文(繁体，澳门)**

- **zh-TW 中文(繁体，台湾)**

  ```html
  <html lang="zh-CN">
  <!--存放网页上所有内容和信息-->
  </html>
  ```

  

### 9、java配置注入

- 注入普通的java类，@Bean注入方法的方法名可以随便取（方法名对应Bean.xml的id）
- 扩展springboot功能，@bean方法名必须为引入接口的接口名（首字母小写）

![](E:\笔记\md文件\springboot\springboot-03-web\java注入.png)

### 10、添加拦截器LoginHandlerInterceptor

1. 继承HandlerInterceptor接口	
2. 重写preHandle（）方法
3. 在mvc配置类中加入拦截器

![](E:\笔记\md文件\springboot\springboot-03-web\添加拦截器配置.png)

### 11、员工列表展示

1.提取公共页面

1. th:fragment="sidebar"	
2. th:replace="~{commons/commons:：topbar}"
3. 如果要传递参数，可以直接使用（）传参，接收判断即可！

2.列表循环展示

### 12.添加员工

1. 按钮提交
2. 跳转到添加页面
3. 添加员工成功
4. 返回首页

### 13、404

直接新建一个error文件夹，放进404.html



> - 前端：
> - 模板：别人写好的，我们拿来改成自己需要的
> - 框架：组件：自己手动组合拼接！Bootstrap，Layui，semantic-ui
> - 栅格系统
> - 导航栏。侧边栏
> - 表单



### 14、回顾：

**上周回顾**

1.  SpringBoot是什么？
2.  微服务
3.  HelloWorld~
4.  探究源码~自动装配原理~
5.  配置 yaml 多文档环境切换
6.  静态资源映射
7.  Thymeleaf th:xxx
8.  SpringBoot 如何扩展MVC javaconfig-
9.  如何修改SpringBoot的默认配置~
10.  CRUD
11.  国际化 拦截器
12.  定制首页，错误页~



**这周：**

1.  JDBC
2.  Mybatis：重点
3.  Druid：重点
4.  Shiro：安全：重点
5.  Spring Security：安全：重点
6.  异步任务~，邮件发送，定时任务（）
7.  Swagger
8.  Dubbo+Zookeeper