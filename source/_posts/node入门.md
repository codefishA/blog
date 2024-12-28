---
title: node基础

author: codefish
date: 2022-6-10 20:12:10
categories: extension
tags: [node]
top_img: /img/Craig Adderley.jpg
cover: /img/Craig Adderley.jpg
---

node主要是利用ECMAScript的核心语法封装的js库，你可以用它构建一个简单的服务器，或者读取excel,修改本地文件等

## 1.fs模块

*fs.readFile()  用来读取指定文件中的内容*

*fs.writeFile()  写入文件*

```js
//1.存放路径
//2.编码格式
//3.回调函数
const fs = require('fs')
fs.readFile('./assets/read.txt','utf-8',function(err,dataStr){
    console.log(err+'-失败')
    console.log(dataStr+'成功读取的数据')
})

-> 结果
null-失败
读取文件
成功读取的数据

-> 读取失败
err的值为错误对象
dataStr 为 underfinded
    
```



### 1.**判断是否读取成功**

判断err是否为空

### 2，向指定文件写入内容

````js
//1.文件路径
//2.写入内容
//3.编码格式
//4.回调函数-
const fs = require('fs')
fs.writeFile('./assets/a.txt','111','utf-8',function(err){
    if(err){
        return console.log('写入失败',err.message)
    }
    console.log('文件写入成功')
})
// fs.writeFile('G:/assets/a.txt','111','utf-8',function(err,a){
//     console.log(err,a)
// })

//结果
null

如果写入失败-err的值为一个错位对象

````

### 3.整理文件

**问题**

*1.replace不写正则为单个匹配*

*2.写入时候replace替代的是%n无效果*

*3.\n 与 ，/n*

```js
const fs = require("fs");
fs.readFile("./assets/read.txt", "utf-8", (err, res) => {
  if (err) {
    return console.log("读取失败" + err.message);
  }
  let data = fn(res);
  write(data);
});
function fn(res) {
  // return res.replace('-',':').replace(' ','%n')
  let data = res.split(" ");
  let newstr = "";
  for (var i = 0; i < data.length; i++) {
    newstr = data[i].replace("-", ":") + " \n" + newstr;
  }
  return newstr;
}
function write(data) {
  fs.writeFile("./assets/write.txt", data, "utf-8", (err) => {
    if (err) {
      return console.log("写入失败" + err.message);
    }
    console.log("写入成功");
  });
}

```

#### ，路径动态拼接的问题

*在使用fs模块操作文件时，如果提供的操作路径是以./或者../开头的相对路径时，很容易出现路径动态拼接错误的问题*

*原因：代码在执行的时候，会以执行node命令时所处的目录，动态拼接出被操作文件的完整路径*

**即在当前路径下执行代码不会有问题，如果在其他路径下执行就会出现问题**

---

**解决：**

```js
//使用绝对路径去替换相对路径
__dirname 代表当前文件所在路径

//使用当前文件路径拼接
readFile(__dirname+"/assets/read.txt", "utf-8", ()=>{})
```



，11



## 2.path模块

***path.join()***:用来将多个路径片段拼接为一个完整的路径字符串

***path.basename()***：用来从路径字符串中，将文件名解析出来



### 1.*path.join([...args])*

*../ 可以抵消路径 抵消上一层路径*

**eg:**

['/a/b','../'] 

**此处可以抵消/b**



凡是涉及路径拼接的操作之后都使用**path.join()**来处理



### 2.path.basename()

```js
path.basename(filespath[,ext])
//1.filespath一个路径的字符串
//2.ext 可选参数 ，表示文件扩展名
//3.返回：<string> 表示路径的最后一部分
```



eg：

```js
const fpath = '/b/c/index.html'
var fullName = path.basename(fpath)
console.log(fullName)  //  index.html

var nameWithoutExt = path.basename('path','.html')
console.log(nameWidthExt) // -> 输出 index
```

### 3.path.extname()

打印拓展名

```js
const fext = path.extname(fpath)
console.log(fext,'-fext')
```



## 3.分离css,html,js案例

注意点；

- 重复调用fs.writeFile() 后写入的会覆盖之前写入的

- fs.writeFile()只能用来创建文件，不能用来创建路径

  ```js
  const fs = require('fs')
  const { resolve } = require('path')
  const path = require('path')
  fs.readFile('./eg1/eg.html','utf-8',(err,sucmsg)=>{
      if(err){
          console.log('读取失败',err.message)
          return
      }
      handle(sucmsg)
  })
  function handle(data){
      //匹配三个模块的正则
      let cssexg = /<style>[\s\S]*<\/style>/ ///<style>[\s\S]*</style>/g
      let jsexg = /<script>[\s\S]*<\/script>/
      let htmlmsg = data.match()
      //定义处理css样式的方法
      //使用正则提取内容
      var cssPart = cssexg.exec(data) + ''
      var jsPart = jsexg.exec(data) + ''
      var htmlPart = data.replace(cssPart,'<link rel="stylesheet" href="./index.css">').replace(jsPart,'<script src="./index.js"></script>')
      //去掉首尾的style和script标签
      cssPart = cssPart.replace('<style>','').replace('</style>','')
      jsPart = jsPart.replace('<script>','').replace('<script>','')
      //存储在当前的文件中
      fs.writeFile(path.join(__dirname,'./index.css'),cssPart,(err)=>{
          if(err){
              return console.log("存储css文件失败",err.message)
          }
      })
      fs.writeFile(path.join(__dirname,'./index.js'),jsPart,(err)=>{
          if(err){
             return console.log("存储js文件失败",err.message)
          }
      })
      fs.writeFile(path.join(__dirname,'./index.html'),htmlPart,(err)=>{
          if(err){
              console.log("存储html文件失败",err.message)
          }
      })
      //开始写为fs.writeFile(path.join(__dirname,'./clock/index.js'),由于clock文件夹不存在导致存储失败
  }
  ```

  

## 4.http模块

**1.初步接触**

```js
//web服务器
//1.导入http模块
//2.创建web服务器实例
//3.绑定requise事件
//4.启动服务器

const http =  require('http')
const server = http.createServer()
// 3.绑定requise事件
server.on('request',function(req,res){
    console.log('Someone visit our web server');
})
//4.启动服务器
server.listen(8080,function(){
    console.log('server listenging 8080')
    //有人访问-就会一直响应
})
```



**2.返回中文会出现乱码问题**-res

**解决**：res.setHeader('Content-Type','text/html;charset=utf-8')

中间为分号（非冒号)

```js
//req请求对象
//只要服务器接收到了客户端的请求，就会通过调用server.on()为服务器绑定的request事件处理函数，
//如果想在事件处理函数中，访问与客户端相关的数据或属性，可以使用如下的方式
const http = require('http')
const server = http.createServer()
server.on('request',(req,res)=>{
    console.log('server is running ',req.url,req.method)
    const url = req.url
    const method = req.method
    const str = `Your request url is ${url},and request method id ${method},'中文'`
    res.setHeader('Content-Type','text/html;charset=utf-8')
    res.end(str)
})
server.listen(8080,function(){
    return console.log('serve running at http://localhost:8080/')
})
```

**3.根据不同url响应不同的页面内容**

1.核心实现步骤

- 获取请求的url
- 设置默认的响应内容为404 Not found
- 判断用户的请求是否为/或index.html页面
- 判断用户请求是否为/about.html关于页面
- 设置Content-Type响应头，防止中文乱码
- 使用res.end()把内容响应给客户端

```js
const http = require('http')
const server = http.createServer()
server.on('request',(req,res)=>{
    console.log('server is running ',req.url,req.method)
    const url = req.url
    const method = req.method
    res.setHeader('Content-Type','text/html;charset=utf-8')
    let content = `<h3>404 not found</h3> </br>Your request url is ${url},and request method id ${method},'中文'`
    if(url==='/'||url=='/index.html'){
        content = `<h3>首页</h3>`
    }else if(url==='/about.html'){
        content = `<h1>关于页面</h1>`
    }
    res.end(content)
})
server.listen(8080,function(){
    return console.log('serve running at http://localhost:8080/')
})
```

## 5.http案例

**将时钟案例的三个文件，根据url地址的不同，返回给客户端**

实现思路

**将文件的实际路径，作为每个资源的url地址**



磁盘文件-由服务端读取-返回给客户端（文件流，数据的形式

1. 导入需要的模块
2. 创建基本的web服务器
3. 将资源的请求url地址映射为文件的存放路径
4. 读取文件内容并响应给客户端
5. 优化资源的请求路径



**问题**：..path 无法读取文件

fpath = '../eg1' = url

解决path.join(__dirname,fpath)





## 6.Node模块化

### 1.模块分类

1. 内置模块

   ---

   由Node.js官方提供的，例如fs,path,http

2. 自定义模块

   ---

   用户创建的每个js文件都是自定义模块

3. 第三方模块

   ---

   由第三方开发的模块，并非官方提供的内置模块，也不是用户创建的自定义模块，试用前需要先下载

4. 1

   

### 2.加载模块

使用强大的require()方法，可以加载需要的内置模块，用户自定义模块，第三方模块使用

- const fs = require('fs')  加载内置的fs模块
- const custom = require('./custom.js') 加载用户的自定义模块
- const moment = require('moment') 加载第三方模块（关于第三方模块的下载和使用）
- 

**注意**：使用require()方法加载其他模块时，会执行加载模块中的代码



### 3.对象和属性

#### 1.module.exports对象

在自定义对象中，可以使用module.exports对象，将模块内的成员共享出去，供外界使用，外界使用require() 导入自定义模块时，得到的就是module.exports所指向的对象。

**使用require()时导入的以**module.export为准

**exports和module.exports**-挂载对象结果一致

**最终共享的结果，以module.exports以准**



**1.案例1**

```js
//当exports 和 module.exports 同时存在的时候，module会覆盖之前export挂载的内容

export.username = "张三"

module.exports = {
    name:"里斯",
    age:23
}
//最终结果为： Object
{name:"里斯",age:23}

```

**2.案例2**

````js
module.exports.username = "张三" //object 1

exports = {  //object 2
    name:"里斯",
    age:23
}
//最终结果为： Object
{username:"张三"}

//开始时候，export和module.export指向object1 
//exports = {}  让export指向对象2
````

**3.案例3**

```js
module.exports.name = "zhang"
export.age = 23

//结果为
{name:"zhang",age: 23}
```

**案例4**

```js
export = {  //object 2
    name:"里斯",
    age:23
}
module.exports = exports
module.exports.sex = "female"

//结果-具备所有的属性
```





#### 2.common.js规范

1. 每个模块内部，module()变量代表当前模块
2. module变量是一个对象，他的exports属性（module.exports)是对外的接口，
3. 加载某个模块，其实是加载改模块的module.exports属性，require()用于加载模块



#### 3.npm 与 包

--(node Package Manager)--

包： 第三方模块

包的来源： 第三方个人或者团队开发出来的，免费提供所有人使用

注意：Node.js中的包都是免费开源的 ，不需要付费既可以免费下载使用

需要包： 基于内置 模块开发时，效率很低-包是基于内置模块封装出来的

包的下载： npm,inc公司下的npm.js

##### 1.格式化时间

```js
1.创建格式化时间的函数
2.创建补零函数
3.导出格式化时间的函数
```

```js
function dataForm(dtStr){
	const dt = new Date(daStr)
    const y = padZero(dt.getFullYear()) //所有的都需要补0
    const m = dt.getMonth() + 1
    const d = dt.getDate()
    const h = dt.getHours()
    const mins = dt.getMinutes()
    const s = dt.getSeconds()
    //return 'YYYY-MM-DD HH:mm:ss'
    return `${y}-${m}-${d} ${h}:${m}:${s}`
}

function padZero(n){
    return n>9 ? n : '0' +n
}

module.exports = {
    dataForm()
}
```



##### 2.使用包

```js
const moment = require('moment')
const dt = moment().format("YYYY-MM-DD HH-mm-ss")

moment() //获取当前时间
//   一个m不补零 - 其他相同
```

### 4.模块加载机制

**1.优先从缓存加载**

模块在第一次加载后会被缓存

require 同时三次 - 后面的不会执行

**2,内置模块的加载机制**

内置模块的优先级最高

### 5.自定义模块的加载机制

1. 没有指定./ -- 把其当作内置模块或者第三方模块

2. 省略了文件名，

   ```js
   1.安装切确的文件名进行加载 
   2.补全.js进行加载
   3.补全.json扩展名进行加载
   4.补全.node拓展名进行加载
   5.加载失败提示
   ```

   ### 



### 6.加载第三方模块

```js
//如果不是一个内置模块，也没有以../或者./开通，则会在node_modules文件夹中加载第三方模块

//如果没有找到对应的第三方模块，则移动到再上一级父目录中，进行加载，直到文件系统的根目录


```



### 7.目录作为模块

当把目录当作模块标识符，传递给require()进行加载的时候，有三种加载方式。

1. 被加载的目录下查找一个叫做package.json的文件，并寻找main属性，作为require() 加载的入口
2. 如果入口没有package.json文件，或者入口不存在或无法解析，则node.js会试图加载目录下的index.js文件
3. 如果以上两步都失败了，则node.js会在终端打印错误信息，报告模块的缺失。Error:cannot find module "xxx"



## 7.express模块

1. 托管静态资源
2. 使用路由精简项目结构
3. 能够使用常见的中间键
4. 能够使用express创建API接口
5. 能够在express中启用CORS跨域资源共享

express: 快速，开放，极简的web开发框架，进一步开发，极大的提示开发效率

- web网站服务器：提供web网页资源的服务器
- API接口服务器：专门对外提供api接口的服务器



#### 1.基本使用

- 创建基本web服务器

  ```js
  //1.导入express
  const express = require('express')
  //2.创建web服务器
  const app = express()
  //4.监听get和post请求-并响应
  app.get('/user',(req,res)=>{
      //调用express提供的res.send()方法，向客户端响应一个json对象
      res.send({name:"zs",age:20,gender:"男"})
  })
  app.post('/user',(req.res)=>{
      app.send({name:"ls",age:20,gender:"女"})
  })
  //启动服务器
  app.listen(80,()=>{
  console.log("express server running at http://127.0.0.1")
  })
  ```

  

- 监听Get请求

  ```js
  ///通过app.get()方法 ，可以监听客户端的GET请求，具体的语法格式如下：
  /*
  *params
  *1.url地址
  *2.处理函数-
  *   1.req请求对象 (包含了与请i去对象相关的属性和方法)
  *   2.res响应对象（包含了与响应相关的属性和方法
  *
  app.get('url',function(req,res){ 
  //处理函数
  })
  ```

  

- app.post监听post请求

- send **把内容响应给客户端**

  ```js
  app.post('/user',(req,res)=>{
      //向客户端发送文本内容
      res.send("请求成功")
  })
  ```

  

- **报错**express : 无法将“express”项识别为 cmdlet、函数、脚本文件或可运行程序的名称

如上我们使用 node 的 npm 安装了 express 框架后，并不会顺带着把 express 脚手架安装。

此时，如果直接输入 express 会进行报错，即关键字不识别（环境变量中没有express.exe）。

需要输入如下命令，安装“脚手架”。

```js
npm install express express-generator -g
```

#### 2.中间件加载静态资源

要提供静态文件（如图像，CSS文件和JavaScript文件），请使用`express.static`Express中的内置中间件功能。

函数签名是：

```javascript
express.static(root, [options])
```

复制

`root`参数指定要从中为静态资产提供服务的根目录。有关`options`参数的更多信息，请参阅express.static。

例如，使用以下代码在名为`public`的目录中提供图像，CSS文件和JavaScript文件：

```javascript
app.use(express.static('public'))
```

复制

现在，您可以加载`public`目录中的文件：

```javascript
http://localhost:3000/images/kitten.jpg
http://localhost:3000/css/style.css
http://localhost:3000/js/app.js
http://localhost:3000/images/bg.png
http://localhost:3000/hello.html
```

复制

 Express查找相对于静态目录的文件，因此静态目录的名称不是URL的一部分。 

要使用多个静态资产目录，请`express.static`多次调用中间件函数：

```javascript
app.use(express.static('public'))
app.use(express.static('files'))
```

复制

Express按照您使用`express.static`中间件功能设置静态目录的顺序查找文件。

注：为获得最佳结果，请使用反向代理缓存来提高服务静态资产的性能。

要为`express.static`函数提供的文件创建虚拟路径前缀（路径实际上不存在于文件系统中），请为静态目录指定一个装载路径，如下所示：

```javascript
app.use('/static', express.static('public'))
```

复制

现在，您可以从`/static`路径前缀中加载目录`public`中的文件。

```javascript
http://localhost:3000/static/images/kitten.jpg
http://localhost:3000/static/css/style.css
http://localhost:3000/static/js/app.js
http://localhost:3000/static/images/bg.png
http://localhost:3000/static/hello.html
```

复制

但是，您提供给`express.static`函数的路径是相对于启动`node`过程的目录。如果您从其他目录运行快速应用程序，则使用您想要提供的目录的绝对路径更安全：

```javascript
app.use('/static', express.static(path.join(__dirname, 'public')))
```

复制

有关该`serve-static`函数及其选项的更多详细信息，请参阅[serve-static](http://expressjs.com/en/resources/middleware/serve-static.html)。

**注意：**

1. public 目录默认找的是当前运行的目录-即如果你在a目录下运行./src/express/index.js，他最终会在当前目录下寻找public文件夹



#### 3.路由

Express支持对应于HTTP方法的下列路由方法：`get`，`post`，`put`，`head`，`delete`，`options`，`trace`，`copy`，`lock`，`mkcol`，`move`，`purge`，`unlock`，`report`，`mkactivity`，`checkout`，`merge`，`m-search`，`notify`，`subscribe`，`unsubscribe`，`patch`和`search`。

路由转换为无效JavaScript变量名称的方法，请使用括号表示法。例如，`app['m-search']('/', function ...`

有一种特殊的路由方法，`app.all()`它不是从任何HTTP方法派生的。此方法用于为所有请求方法的路径提供加载中间件功能。

app.all('/secret', function (req, res, next) {  console.log('Accessing the secret section ...')  next() // pass control to the next handler })

**路由路径**

路由路径与请求方法一起定义了可以进行请求的端点。路径路径可以是字符串，字符串模式或正则表达式。

字符`?`，`+`，`*`，和`()`是他们的正则表达式的对应的子集。字符串路径会逐字解释连字符（`-`）和点（`.`）。

如果您需要在路径字符串中使用字符（`$`），请将其包含在`([])`之内。例如，“ `/data/$book`” 处的请求的路径字符串将是“ `/data/([\$])book`”。

**以下是一些基于字符串模式的路由路径示例。**

此路由路径将匹配`acd`并`abcd`。

```javascript
app.get('/ab?cd', function (req, res) {
  res.send('ab?cd')
})
```

复制

这条路由的路径将会匹配`abcd`，`abbcd`，`abbbcd`等等。

```javascript
app.get('/ab+cd', function (req, res) {
  res.send('ab+cd')
})
```

复制

这条路线的路径匹配`abcd`，`abxcd`，`abRANDOMcd`，`ab123cd`等。

```javascript
app.get('/ab*cd', function (req, res) {
  res.send('ab*cd')
})
```

复制

此路由路径将匹配`/abe`并`/abcde`。

```javascript
app.get('/ab(cd)?e', function (req, res) {
  res.send('ab(cd)?e')
})
```

**在Express 4.x中，[正则表达式中](https://github.com/expressjs/express/issues/2495)[的`*`字符不以通常的方式解释](https://github.com/expressjs/express/issues/2495)。作为解决方法，请使用`{0,}`而不是`*`。Express 5可能会解决这个问题。**

##### 1.响应方法

`res`下表中响应对象（）的方法可以向客户端发送响应，并终止请求 - 响应循环。如果这些方法都不是从路由处理程序调用的，则客户端请求将被挂起。

| 方法               | 描述                                                   |
| :----------------- | :----------------------------------------------------- |
| res.download()     | 提示要下载的文件。                                     |
| res.end()          | 结束响应过程。                                         |
| res.json（）       | 发送JSON响应。                                         |
| res.jsonp（）      | 用JSONP支持发送JSON响应。                              |
| res.redirect（）   | 重定向请求。                                           |
| res.render（）     | 呈现视图模板。                                         |
| res.send（）       | 发送各种类型的响应。                                   |
| res.sendFile（）   | 以八位字节流的形式发送文件。                           |
| res.sendStatus（） | 设置响应状态代码并将其字符串表示形式作为响应主体发送。 |

##### 2.app.route()

通过使用可以为路由路径创建可链接的路由处理程序`app.route()`。由于路径是在单个位置指定的，所以创建模块化路由很有帮助，因为这可以减少冗余和拼写错误。有关路由的更多信息，请参阅：Router（）文档。

以下是使用定义的链接路由处理程序`app.route()`的示例。

```javascript
app.route('/book')
  .get(function (req, res) {
    res.send('Get a random book')
  })
  .post(function (req, res) {
    res.send('Add a book')
  })
  .put(function (req, res) {
    res.send('Update the book')
  })
```

##### 3.express.Router

使用`express.Router`类创建模块化，可挂载的路由处理程序。一个`Router`实例是一个完整的中间件和路由系统; 出于这个原因，它通常被称为“迷你应用程序”。

以下示例将路由器创建为模块，在其中加载中间件功能，定义一些路由并将路由器模块安装在主应用程序的路径中。

在app目录中创建一个`birds.js，`

##### *4.可配置的中间件

如果您需要中间件可配置，则导出一个接受选项对象或其他参数的函数，然后根据输入参数返回中间件实现。

File: `my-middleware.js`

```javascript
module.exports = function(options) {
  return function(req, res, next) {
    // Implement the middleware function based on the options object
    next()
  }
}
```

复制

现在可以使用中间件，如下所示。

```javascript
var mw = require('./my-middleware.js')

app.use(mw({ option1: '1', option2: '2' }))
```

#### 4.使用中间件

- 执行任何代码。

- 对请求和响应对象进行更改。

- 结束请求 - 响应循环。

- 调用堆栈中的下一个中间件功能。next()

如果当前的中间件功能没有结束请求 - 响应周期，则它必须调用`next()`以将控制传递给下一个中间件功能。否则，请求将被挂起。

Express应用程序可以使用以下类型的中间件：

- Application-level middleware

- 路由器级中间件

- 错误处理中间件

- 内置中间件

- 第三方中间件



---

下面是一个在装载点加载一系列中间件功能的例子，带有装载路径。它演示了一个中间件子堆栈，用于打印任何类型的HTTP请求到`/user/:id`路径的请求信息。

```javascript
app.use('/user/:id', function (req, res, next) {
  console.log('Request URL:', req.originalUrl)
  next()
}, function (req, res, next) {
  console.log('Request Type:', req.method)
  next()
})
```

## 路由器级中间件

路由器级中间件的工作方式与应用级中间件的工作方式相同，只是它绑定到一个实例`express.Router()`。

```javascript
var router = express.Router()
```

复制

使用`router.use()`和`router.METHOD()`函数加载路由器级中间件。

以下示例代码通过使用路由器级中间件来复制上面显示的用于应用程序级中间件的中间件系统：

```javascript
var app = express()
var router = express.Router()

// a middleware function with no mount path. This code is executed for every request to the router
router.use(function (req, res, next) {
  console.log('Time:', Date.now())
  next()
})

// a middleware sub-stack shows request info for any type of HTTP request to the /user/:id path
router.use('/user/:id', function (req, res, next) {
  console.log('Request URL:', req.originalUrl)
  next()
}, function (req, res, next) {
  console.log('Request Type:', req.method)
  next()
})

// a middleware sub-stack that handles GET requests to the /user/:id path
router.get('/user/:id', function (req, res, next) {
  // if the user ID is 0, skip to the next router
  if (req.params.id === '0') next('route')
  // otherwise pass control to the next middleware function in this stack
  else next()
}, function (req, res, next) {
  // render a regular page
  res.render('regular')
})

// handler for the /user/:id path, which renders a special page
router.get('/user/:id', function (req, res, next) {
  console.log(req.params.id)
  res.render('special')
})

// mount the router on the app
app.use('/', router)
```

***next('router')** ?

跳过路由器的其他中间件功能，请调用`next('router')`以将控制权从路由器实例中退出
