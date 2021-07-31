#  一、Mock的入门和安装

## 1.1.为什么用Mock.js

最近学习vue编写项目由于没有Api接口，在使axios中不能模拟接收自己到想要的数据（我主要不能快速模拟接收图片数据，其他的数据类型可以用springmvc快速简单模拟一个数据）。

在网上查阅了很多的资料，在神器的网站（B站）听说Mockjs可以在后端还没创建好接口的情况下拦截浏览器的请求。最重要还可以模拟生成图片，美滋滋。

## 1.2.Mock.js介绍

1. Mock.js 是一款模拟 JSON 数据的前端技术，为什么要产生这种技术？ 
2.  对于前后端分离的项目，后端工程师的 API 数据迟迟没有上线； 
3. 而前端工程师却没有 JSON 数据进行数据填充，自己写后端模拟又太繁重； 
4. 这个时候，Mock.js 就能解决这个问题，让前端工程师更加独立做自己； 
5. 官方网站为：mockjs.com；学前基础：Vue2.x 基础完结后方可学习；或一定前端基础。

## 1.3.安装

### 1.3.1.方式1：Node (CommonJS)

在选定文件目录下的终端输入（前提电脑安装了node，具体安装方式上网一大堆

## 1.4.Mock语法规范

1.  Mock.js 的语法规范包括两个部分：数据模板定义规范和数据占位定义规范； 

2. 数据模板定义的规范包含 3 个部分：属性名、生成规则和属性值； 

   '属性名|生成规则' : 属性值    //    'name|rule' : value 

3. 其中，字符串、数值有 7 种生成规则，具体如表说明： 

   ![mock语法规范1](E:\笔记\Mockjs\图片\mock语法规范1.png)

4. 除了以上几种规则格式，还有布尔值、对象和数组等规则； 

   ![mock语法规范2](E:\笔记\Mockjs\图片\mock语法规范2.png)

5. 也支持函数和正则表达式； 

   ![mock语法规范3](E:\笔记\Mockjs\图片\mock语法规范3.png)

6. 数据定义的占位符@，比较好理解，占领属性值的位置； 

   ```js
   'list|5' : [{
     cname : '@cname', 
     city : '@city', 
     full : '@cname - @city' 
   	}]
   ```

   



## 1.5.Mock随机占位符

### 1.5.1.通过 '@占位符' 这种方式来随机产生各种不同的数据；

```js
//第一种输入占位符的方式 
console.log(Mock.Random.cname()); 
//第二种输入占位符的方式 
console.log(Mock.mock('@cname'));
```

PS:第二种方式听说更香哦

<hr>

### **1.5.2.常用占位符**

![mock占位符1](E:\笔记\Mockjs\图片\mock占位符1.png)

具体用法可以常考官网：

[官网示例 ]: http://mockjs.com/examples.html

```js
//随机中文人名，不带 c 就是英文
console.log(Mock.mock('@cname'));
//随机 ID 
console.log(Mock.mock('@id'));
//随机中文标题，不带 c 就是英文
console.log(Mock.mock('@ctitle'));
//随机颜色，十六进制 
console.log(Mock.mock('@color')); 
//随机图片，给你一个图片地址 
console.log(Mock.mock('@image')); 
//随机 ip 地址 
console.log(Mock.mock('@ip')); 
//随机 url 地址 
console.log(Mock.mock('@url')); 
//随机字符串 
console.log(Mock.mock('@string')); 
//随机数值 
console.log(Mock.mock('@integer')); 
//随机日期 
console.log(Mock.mock('@datetime'));
```



### 1.5.3.如果没有我们想要的数据格式进行填充，可以使用扩展功能自己扩展

```js
//自行扩展，各种商店名称 
        Mock.Random.extend({
            cstore() {
                return this.pick([
                    '宠物店',
                    '美容店',
                    '小吃店',
                    '数码店',
                    '快餐店'
                ]);
            }
        });
        //扩展调用 
        console.log(Mock.mock('@cstore'));
```



# 二、在Axios库的运用

## 2.1.在VueCli（vue脚手架）安装Axios

方式：Node (CommonJS)

```cmd
npm install axios
```

## 2.2.CDN使用Axios

```js
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
```

```js
<script src="https://cdn.bootcss.com/Mock.js/1.0.0/mock-min.js"></script>
```

```js
const axios = require('axios');
        //使用 get 请求获取数据 
        axios.get('填写要请求的api')
            .then(res => {
                console.log(res.data);
            })
            .catch(err => {
                console.log('错误：' + err);
            });
```

PS：注意可能有跨域问题。下面在本机测试，不会跨域。

## 2.2.vue中使用Mock

在vue项目安装mock: `npm install mockjs`  和axios：`npm install axios`

2.2.1.在项目的根目录新建`vue.config.js`文件

```js
module.exports = {
    devServer: {
        before: require('./mock/index')//引入mock/index.js
    }
}
```

mock/index.js

```js
//测试项目笔记最好会放上
const fs = require('fs')
 const path = require('path')
 const Mock = require('mockjs')
 const JSON5 = require('json5')

 //读取json文件
 function getJsonFile(filePath) {
     //读取指定json文件
     var json = fs.readFileSync(path.resolve(__dirname, filePath), 'utf-8');
    //  fs.readFileSync(path.join(__dirname,"./userInfo.json5"),'utf-8')

     //  解析并返回
     return JSON5.parse(json)
 }

 // 返回一个函数
 module.exports = function (app) {
     if (process.env.MOCK == 'true') {
         //监听http请求
         app.get('/user/userInfo', function (rep, res) {
             // 每次响应请求时候读取mock data的json文件
             // getJsonFile方法定义了如何读取json文件并解析成数据对象
             var json = getJsonFile('./userInfo.json5')
             // 将json传入Mock.mock 方法中，生成的数据返回给浏览器
             console.log("haah")
             res.json(Mock.mock(json));
         });
     }
 }
```

**./userInfo.json5**

```json
{
    id: '@id',
    cname: '@cname',
    avatar: "@image('200×200','red','#fff','avatar')"
}
```

**测试实例HelloWorld.vue**

```js
<script>
import axios from 'axios'
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  mounted() {
    axios.get('/user/userInfo')
    .then(res=>{
      console.log(res)
    })
    .catch(err =>{
      console.error(err);
    })
  }
}
</script>
```

.env.development

```js
//true为开启mock；false为关闭
MOCK = true
```



# 三、在Ajax库的运用

## 3.1.在引用JQuery的项目中引用Mock.js

```js
 <script src="https://cdn.bootcss.com/jquery/2.2.4/jquery.min.js"></script>
 <script src="https://cdn.bootcss.com/Mock.js/1.0.0/mock-min.js"></script>
```



## 3.2.JQuery的项目使用Mock

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="https://cdn.bootcss.com/jquery/2.2.4/jquery.min.js"></script>
    <script src="https://cdn.bootcss.com/Mock.js/1.0.0/mock-min.js"></script>
</head>
<body>
   <h1>testMockAjax</h1> 
     <!-- 第二种 -->
   <script>MOCK = 'true'</script>
   <!-- 第一种删除Mock。注释掉 -->
   <script src="./mock/testMock.js"></script>
 
   <script>
       $.ajax({
           url:'/testmcokajax2',
           type: 'get',
           dataType: 'json',
           success:(data)=>{
               console.log(data)
           }
       })
   </script>
</body>
</html>
```

**./mock/testMock.js**

```js
if(MOCK = 'true'){
  Mock.mock('/testmcokajax', 'get', {
    id: '@id',
    cname: '@cname',
    avatar: "@image('200×200','red','#fff','avatar')"
})
}
```



## 3.3.项目移除mock

方式1：直接删除``<script src="./mock/testMock.js"></script>``

方式2：在`script`标签定义 ``<script>MOCK = 'true'</script>``,移除改为`false`



# 四、笔记定位

学习mockjs参考了

[B站连接]: https://www.bilibili.com/video/BV1PE411A7U6?p=1

[官网github文档]: https://github.com/nuysoft/Mock/wiki/Getting-Started
[学习项目代码：码云]:https://gitee.com/lhj548362/mockLearn.git

这个笔记只是简单拦截请求并作出简单的响应。响应的数据格式还需结合自己的项目确定数据结构。其中的mock在项目中移除需要事先考虑使用哪种方式，避免影响项目的正常网络请求。