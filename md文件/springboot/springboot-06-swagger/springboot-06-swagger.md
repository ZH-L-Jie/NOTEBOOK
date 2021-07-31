Swagger



- 号称世界上最流行的Api框架；

- RestFul Api文档在线自动生成工具=>==Api文档与APl定义同步更新==

- 直接运行，可以在线测试API接口；

- 支持多种语言：Java，Php..…

  官网：https://swagger.io/

  

在项目使用Swagger需要springbox

- swagger2
- ui

### 1.SpringBoot 集成Swagger

1. 新建一个SpringBoot=web项目

2. 导入相关依赖

   ```xml
   <!--swagger-->
           <dependency>
               <groupId>io.springfox</groupId>
               <artifactId>springfox-swagger-ui</artifactId>
               <version>2.9.2</version>
           </dependency>
           <dependency>
               <groupId>io.springfox</groupId>
               <artifactId>springfox-swagger2</artifactId>
               <version>2.9.2</version>
           </dependency>
   ```

3. 编写一个Hello工程

4. 配置Swagger

5. 测试运行：[ http://localhost:8080/swagger-ui.html ](http://localhost:8080/swagger-ui.html)

   ![](E:\笔记\md文件\springboot\springboot-06-swagger\swagger页面.png)

6. ```xml
   <!--swagger-->
           <dependency>
               <groupId>io.springfox</groupId>
               <artifactId>springfox-swagger-ui</artifactId>
               <version>2.9.2</version>
           </dependency>
           <dependency>
               <groupId>io.springfox</groupId>
               <artifactId>springfox-swagger2</artifactId>
               <version>2.9.2</version>
           </dependency>
   ```

   

**总结：**

1. 我们可以通过Swagger给一些比较难理解的属性或者接口，增加注释信息
2. 接口文档实时更新
3. 可以在线测试试