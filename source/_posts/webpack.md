---
title: webpack从零到一搭建vue项目
date: 2022-04-18 19:27:06
tags: [webpack5,vue]
categories: webpack
top_img: /img/9.jpg
cover: /img/9.jpg
---

webpack是当下应用最大的构建工具，从0到1构建项目，是一个值得学习课题。不管是vue项目还是其他项目都离不开构建工具（可能是其他构建工具）。

#### webpack打包工具

##### 1.安装webpack

##### webpack webpack-cli -D(4x,5X)

- ​	安装低版本，指定  

  - webpack@4.43.0 webpack-cli@3.3.12 -D

  - ```js
    @3 安装3版本的最新版本
    ```

- node-modules  -> bin -> 软链接目录（-D）

  - ```js
    //锁死安装路径
    npx webpack -V  //npx 找项目下的软连接目录，有的话直接启动，如果没有则安装webpack到本地，并启动他
    webpack -V //全局环境
    ```

- 定义名称

  - ```js
    script{
        
    }
    
    
    ```

- webpack 0配置启动

- 配置webpack文件

  - ```js
    //根目录下新建 webpack.conifg.js
    mudule export ={
    	//打包入口 默认       ./src/index.js 
        entry:'./sec.index.js'
        //打包出口，文件路径   
        //打包模式
        outPut:{
        //为打包资源文件存在位置，必须为绝对路径
    	path:
        filename:
        //产出的资源文件名
    	}
    //默认生产模式，默认启动js压缩优化
    	mode:'production'，
        
    //不开启模式匹配，不压缩	
    mode:none
        
    }
    
    //const {resolve,join}  = require('path')
    const path = require('path');
    产出子文件的存放位置 
    path:path.resolve(_dirnamr,'./dist')
    
    mode:producton
    mode:development 开发模式。不会压缩代码，会暴露更多信息，方便调整。
    mode:none
    //模式警告
    ```

  - ```js
    --config 告诉启动哪一个配置权限
    ```

    - webpack 支持import，不支持css，只支持js

      ```js 
      引入css，出错。 
      less pug vue jpe jpeg svg
      ```

  - ```js
    module :{
        rules:[
            {
                test:/|.css/,
                use:'',
            }
            {
            	test:/\.less$/,
            	use:['style-loader','css-loader','less-loader']
            }
            ...
        ]
    }
    ```

    ##### loader 模编译器

    css-loader  css 挂载，

    ```js
    默认装...5
    降级 @..
    ```

    

  - 打包的css怎么用

  - css-loader 支持模块。style-loader触发效果

  - ```js
    style-loader:  //操作dom动态创建
    npm install style-loader@zm 
    当多个load作用于一个模块的时候，有执行顺序。从后往前
    ```

  - .less  采用less模块

- plugin插件

  - mini-css-extract

  - ```js
    //引入插件
    const minicss from 
    
    [name].[ext] 占位符
    /\.(png|jpg|jpe?g|webp)$/ 
    ? 有或没有
        
    
    ```

以前写的工作室官网项目，本来能运行起来的，隔了很久之后，中间还重做了一次系统，今天重新运行竟然运行不起来了。

报错：

Matrix-official@1.0.0 dev: `webpack-dev-server --inline --progress --config

```js
从网上找了一个解决方案：

卸载webpack-dev-server
npm uninstall webpack-dev-server
重新安装webpack-dev-server
npm install webpack-dev-server@2.9.7
```



loader 模板转译 编译器



webpack 打包工具

1.安装

​	npm i webpack@4.43.0 webpack-cli@3.3.12 -D 

​	-D    <=>   --save -dev

​	ps：webpack webpack-cli loader 非常重视版本兼容，版本不符合会导致打包失败

2.两种打包方式

1. 在package.json的script中写

   - ```js
     "dev" : "webpack",
      //npm run dev 启动打包工具
     ```

2. 直接npx webpack

3.默认打包

​		直接运行打包命令

​		默认会在根目录新建一个dist文件夹，把其他文件打包到里面的main.js文件里。默认能正确打包，js和json文件

4.打包less,css,sass,sass,png等等文件

​	需要安装loader工具，

```js
eg:
npm i less-loader -D
npm i css-loader -D
npm i mini-css-extract-plugin -D
npm i sass-loader -D

插件1//mini-css-extract-plugin 可以生成一个css文件，将css样式预编译并且存在一个指定的位置。
const minicss = require("mini-css-extract-plugin");
 plugins: [
        new minicss({
          filename: "./my/b.css",
        }),
      ],

     //loader 模板转译 编译器
```

#### 插件1

mini-css-extract-plugin 可以生成一个css文件，将css样式预编译并且存在一个指定的位置。

loader使用

```js
module.exports = {
    entry:'./src/index.js',

    output:{
        path:resolve(__dirname,"./my"),
        filename:'my.js',
    },

    mode:'development',

    module:{
        rules:[
            {
                test:/\.css$/,
                use:["style-loader","css-loader"],
            },
            {
                test:/\.less$/,
                use:[minicss.loader,"css-loader","less-loader"],
            },
            {
                test:/\.(png|jpg|webp|gif|jpeg)$/,
                use:{
                    loader:'file-loader',

                    options:{
                        name:"[name].[ext]",
                        outputPath:"./my/images",
                        publicPath:'../images'
                    },
                },
            },
        ]
    },
    
    // e? 表示e有或者没有
   // name:"[name].[ext]",   占位符
  //publicPath:'../images'   解决css中引入图片的路径问题
 //outputPath:"./my/images",   打包文件存放地址
    
    
 //打包入口 ./src/index.js 支持相对，也支持绝对路径
  entry: "./src/index.js",
  //出口
  output: {
    //   产出的资源文件的存放位置 ./dist 必须是绝对路径
    path: resolve(__dirname, "./build"),
    //  产出的资源文件名 main.js
    filename: "index.js",
  },
  //模式 production 生产模式 会默认开启JS压缩优化
  //模式 development 开发模式 不会压缩代码，还会有注释，会暴露更多的信息，方便调试
  // none 不开启模式，也不会启动各种优化插件
  mode: "development",
    
    //多个入口
     entry:{
        index:'./src/index.js',
        es6:"./src/es6.js",
    },
        

    output:{
        path:resolve(__dirname,"./my"),
        filename:'[name].js',
    },

```

##### 插件2

打包后的文件，自动生成一个html文件。并且可以设置模板

##### Webpack 如何将打包后的文件，自动生成一个html文件

...html-webpack-plugin

...安装4-x版本

...1.npm 安装插件

...引入插件

...实例化对象



- 创建html模板

  - ```js
    template:"./src/index/.html"
    //模板克隆目录
    filename:"index.html"
    ```

##### 保持文件夹清空（打包之前

##### 插件三.保持文件夹初始化。

- clean-webpack-plugin

- 引用

  - ```js
    const {} from {''}...
    ```



##### 解决js的兼容问题

语法（箭头函数）和特性（promise）

多入口应用

```js
entry:{
	index:"./",
    ecma:"./",
}
    
 output:{
     占位符
 }
```



##### 处理js 的打包，Babel-loader

安装依赖

babel-loader @babel/core -D

兼容性：知道目标浏览器集合

有一套工具链：主要将es6

##### ps:只能转语法,新特性只能profill

- 将其在js文件最上方引入

babel 预设 -> 做语法转换的part

- env 原生
- react react转换
- ts转换
- flow转换

```js
@babel/preset -env
```

写babel-loader配置

```js
option:{
    presets:["@babel/preset-env"],
    //存在默认浏览器降级版本
}
```

###### 设置目标浏览器集合

package.json 中

```js
"browerslist":[
	//几个常用集合
    "last 2 versions",
    ">1%"
]
//1.所有浏览器最近两个版本
//2.兼容所有浏览器市场份额大于1%（全球）
//last 1 version 兼容当前最新版本

```



查看符合的版本(终端)

```js
npx browerslist "last 2 versions"

npx browerlist "last 2 versions",">1%"
```



##### profill 

一个js库，内有es6新增所有的特性

在顶部安装polyfill

**生产依赖** 默认为生产模式安装

npm i @babel/polyfill

在js文件的第一行引入profill

```JS
import "@babel/polyfill"
```

按需导入 ->	



##### webpack-dev-server

1.推荐3x版本

2.定义脚本

````js
"serve":"webpack-dev-server"

//拦截打包的文件，在硬件的内存中
````

3.配置在webpack.config.js

```js
devServer:{

​	port:8081,//端口

​	open:true,//打开一个新标签页

}
```



##### 定位bug位置

```js
devtool:"none"
//开发模式下默认开启 devtool:'source-map',
	多出一个.map文件，是一个影射地图
//关闭后指向打包后的js
```



### Vue

1.安装vue-loader vue-template-compiler

​			实例化： vue-loader-plugin

2.webpack....

3.vue-style-loader

 vue 生产依赖



​			