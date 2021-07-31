# springstudy

### 1、自定义banner——启动的图样

网址：

 https://www.bootschool.net/ascii-art/comic?pageNO=1 

直接复制txt文件到resource即可

### 2、在 properties 文件里面写下面即可

```properties
server.port=8081
```

### 3、pom.xml

- 在父工程的spring-boot-dependencies：核心依赖

- 在导入springboot的其他依赖时，不需要指定版本，有版本仓库

  #### 启动器

  ```xml
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter</artifactId>
  </dependency>
  ```

  - springboot的启动场景
  
  - 比如<artifactId>spring-boot-starter-web</artifactId>就会导入web环境所有的依赖！
  
  - springboot会将所有的功能场景，都变成一个个启动器（需要什么功能直接导入启动器相应的依赖即可） 
  
    https://docs.spring.io/spring-boot/docs/2.2.6.RELEASE/reference/html/using-spring-boot.html#using-boot-starter 

### 4、主程序

```java
@SpringBootApplication
public class Springboot01HelloworldApplication {

	public static void main(String[] args) {
		SpringApplication.run(Springboot01HelloworldApplication.class, args);
	}

}
```

注解

```java
@SpringBootconfiguration:springboot的配置
	@configuration:spring配置类
	@component：说明这也是一个spring的组件
        
@EnableAutoconfiguration：自动配置
	@AutoConfigurationPackage：自动配置包
		@Import（AutoconfigurationPackages.Registrar.class）：自动配置，包注册
	@Import（AutoconfigurationImportselector.class）：自动配置导入选择
        
//获取所有的配置
List<String> configurations=getCandidateConfigurations（annotationMetadata，attributes）；
        
```

获取候选的配置

```java
protected List<string> getCandidateConfigurations(AnnotationMetadata metadata, 			AnnotationAttributes attributes){
List<String> configurations=
	SpringFactoriesLoader.1oadFactoryNames(getSpringFactoriesLoaderFactoryclass(), getBeanClassLoader()); Assert. notEmpty(configurations,"No auto configuration classes found in META-INF/spring. factories. If you"
+"are using a custom packaging, make sure that file is correct.");
    return configurations;
}
```



@SpringBootAppLication：标注这个类是一个springboot的应用：启动类下的所有资源被导入



META-INF/spring.factories：自动配置的核心文件     (自动导入的包，没有到入就没有导入)

![](E:\笔记\md文件\springboot\springboot-01\自动导入的配置环境.png)

```java
Properties properties=PropertiesLoaderutils.loadProperties（resource）;
    //所有资源加载到配置类中！
```

结论：springboot所有自动配置都是在启动的时候扫描并加载：spring.factories所有的自动配置类都在这里面，但是不一定生效，要判断条件是否成立，只要导入了对应的start，就有对应的启动器了，有了启动器，我们自动装配就会生效，然后就配置成功！

1. springboot在启动的时候，从类路径下/META-INF/spring.factories获取指定的值；

2. 将这些自动配置的类导入容器，自动配置就会生效，帮我进行自动配置！

3. 以前我们需要自动配置的东西，现在springboot帮我们做了！

4. 整合javaEE，解决方案和自动配置的东西都在spring-boot-a 下utoconfigure-2.2.0.RELEASE.jar 这个包下

5. 它会把所有需要导入的组件，以类名的方式返回，这些组件就会被添加到容器；

6. 容器中也会存在非常多的xxxAutoConfiguration的文件（@Bean），就是这些类给容器中导入了这个场景需要的所有组件；并自动配置，@Configuration，JavaConfig！

7. 有了自动配置类，免去了我们手动编写配置文件的工作！

   ```per