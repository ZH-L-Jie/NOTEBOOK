springboot-02-config

### 1、yaml和properties的差别

yaml:

```yaml
#普通的key-value
name: dalong

#对象
student:
  name: dalong
  age: 3

#行内写法
student2: {name: dalong,age: 3}

#数组
pets:
  - cat
  - dog
  - pig

pets2: [cat,dog,pig]
```

properties:

```properties
#properties只能保存键值对
name=dalong

student.name = dalong
student.age = 3

```

### 2、启动springbootTest测试时，会存在启动失败的情况，Maven依赖导入失败，窗口会卡住

导入下面依赖：

```xml
<dependency>
    <groupId>org.junit.platform</groupId>
    <artifactId>junit-platform-launcher</artifactId>
    <scope>test</scope>
</dependency>
```

### 3、lombok依赖

```xml
<!--lombok-->
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.16.10</version>
    <scope>provided</scope>
</dependency>

```

### 4、yaml文件接入对象的属性值

需要在依赖导入，防止idea上方报红（报红不会影响编译）

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
    <optional>true</optional>
</dependency>
```

再，再实体类上加入注解

```java
@Data
@Component
@AllArgsConstructor
@NoArgsConstructor
@ConfigurationProperties(prefix = "person")
public class Person {
	private String name;
	private Integer age;
	private Boolean happy;
	private Date birth;
	private Map<String,Object> maps;
	private List<Object> lists;
	private Dog dog;

}
```

和yaml中的person对象绑定，属性有不对应的会编译报错

```yaml
person:
  name: dalong
  age: 15
  happy: false
  birth: 2020/04/10
  maps: {k1: v1,k2: v2}
  lists:
    - code
    - music
    - girl
  dog:
    name: 网吃啊
    age: 3
```

用处：比如我们配置Mybatis时，可以用Java配置方式，再在yaml中绑定注入属性

当然也可以用properties文件配置，但配置起来会繁琐很多

1. yaml文件的注入，如上

2. properties文件注入，如上所说，繁琐，用@PropertiesSource（value=“classpath：xxxx.properties”）绑定文件位置，在实体类属性上用@Value("${属性名}")el表达式绑定属性

3. 使用springIOC@compend和@Value在实体类的属性上直接注入

   **方式1的升级用法：**

   ```yaml
   person:
     name: dalong${random.uuid}
     age: ${random.int}
     happy: false
     birth: 2019/11/02
     maps: {k1: v1,k2: v2}
     hello: 大  #真的定义值
     lists:
       - code
       - music
       - gir1
     dog:
       name: ${person.hello:hello}_旺财   #这里hello.hello是默认值
       age: 3
   ```

   

### 5、yaml的**松散绑定**：实体类驼峰命名------yaml带-命名

```yaml
dog:
  first-name: 好狗
  age: 3
```

```java
@Component
@Data
@AllArgsConstructor
@NoArgsConstructor
@ConfigurationProperties(prefix = "dog")
public class Dog {
	private String firstName;
	private Integer age;
}
```

**@Repository**:加在Dao层的注解注入

### 6、开启数据检验 (jsr303)

1. 实体类加上@Validated

2. 属性加上

   ```java
   @Validated
   public class Person {
   
   	@Email(message = "邮箱格式错误！")
   	private String name;
   
   	private Integer age;....
   ```

   [jsr303使用](https://www.ibm.com/developerworks/cn/java/j-lo-jsr303/)

   

### 7、项目执行yaml文件的顺序

1. file:./config
2. file:./
3. classpath:/config/
4. classpath:/

**在项目看就是从外向内权限降低**

用于多环境开发，如：测试、生产......运用的配置环境也是不尽相同

但，这种方式有点不方便，可以**选定配置文件**用于不同环境的开发



**多环境多配置**

![多环境多配置](E:\笔记\md文件\springboot\springboot-02-config\多环境多配置.png)

```yaml
server:
  port: 8082


server:
  port: 8082

spring:
  profiles:
    active: test
```

properties文件也是可以用类似的方式选用环境（记得utf-8编码）

yaml文件**升级**

```yml
server:
  port: 8082
spring:
  profiles: dev
---
server:
  port: 8083
spring:
  profiles: test
---
spring:
  profiles:
    active: test

server:
  port: 8081

```

### 8、自动装配-在yaml中

yaml中手动配置自动导入的配置，我们怎么操作那些自动导入的配置属性呢？

![自动装配的步骤](E:\笔记\md文件\springboot\springboot-02-config\自动装配的原理-yaml.png)

1. 在AutoConfiguration.jar中打开找到spring.properties

2. 找到要手动配置的配置类

3. ![xxxproperties](E:\笔记\md文件\springboot\springboot-02-config\XXXproperties.png)

4. 点进去，

   ![](E:\笔记\md文件\springboot\springboot-02-config\prefix.png)

   ![](E:\笔记\md文件\springboot\springboot-02-config\查找配置属性.png)

 #xxXAutoConfiguration：默认值xxxProperties和配置文件绑定，我们就可以使用自定义的配置了！

**其他的自动配置类可能和上述步骤不同；可以点击@XXXConfigurationXXX的注解看后面是否有导入类，进入类就可找到，否则继续上述点到找到为止！**

### 9、查看配置成功和失败的类(默认为false)

```yaml
debug: true
```

