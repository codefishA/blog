---



title: 开发代码调试, 问题定位, h5调试总结

tags: [map,set,generate,iterator]

author: codefish

date: 2024-06-07 20:20:34

categories: js

top_img: /img/7.jpg

cover: /img/7.jpg



---

开发中问题调试, 一直是一个业务开发痛点, 根据前端的应用载体可分为, 

1. web应用
2. 小程序
3. h5
   1. app内嵌h5
   2. 微信h5 | 微信公众号

### web调试

1. 一般在开发过程中, 大多采用**console, debugger断点方式**
2. 线上环境调试
   1. 本地连接线上数据进行调试
   2. 线上查看控制台是否有报错(一般是使用了某些没有的定义的变量或者方法)
   3. 线上调试正在运行的源代码, 进行debugger调试
   4. 各种浏览器tools: Vue devtools , React developer tools, react redux 



### 小程序调试

基本和上述一致,  只是载体从浏览器变为了开发者工具或者真机, 需要注意的是,  如果需要测试唤起调试, 需要编译好小程序后, 手机扫描, 设置唤起扫码的版本



### h5调试

1. #### 内嵌h5

   如果有对应程序的debug包, 可以直接抓包, 如果不能抓包, 网页检查器也是不能使用的

   - 接口调试 / 网页错误

     - 抓包工具charles fiddler

     - 网页调试功能 

       - safari可以使用网页检查器(和web应用一样全可以调试)

       - chorme devices可以调试任何网页, 前提是app没有限制 

         Eg: [chorme devices](chrome://inspect/#devices) : chrome://inspect/#devices

2. #### 微信h5, 微信公众号

   1. 主要使用微信开发者工具

3. 





###### 关于工具的使用不赘述, 可以自行搜索

注意

- charles证书有效期为一年, 网页检查器部分机型不装证书可能没有效果
- 线上源代码调试的时候, 搜索关键字建议用中文, 函数名都会被转为别的名字, 会搜索不到









