---

title: javascript入门
tags: [javaScript]
author: codefish
date: 2021-12-11 16:13:52
categories: js
top_img: /img/2.jpg
cover: /img/2.jpg

---
撸js的第一遍，持续不断深入的学习一门语言应当坚持下去，每一回学习都会有不一样的体会。

# javascript介绍

## JavaScript 是脚本语言（作为web的

JavaScript 是一种函数优先轻量级的编程语言。

JavaScript 是可插入 HTML 页面的编程代码。

JavaScript 插入 HTML 页面后，可由所有的现代浏览器执行。

JavaScript 基于原型编程，多范式的动态脚本语言，支持面向对象，命令式，声明式，函数式编程范式，解释性或即时编译型的编程语言

网景 -->欧洲计算机协会 -->ECMAScript(ES)

Javascipt 核心语法

核心语法（ESMAscript） DOM（W3C) BOM(网景)

ES1           ...         ES3       ...          ES5

ES4（去掉改动大的语法,和谐会议）

ES6 2015年   当年争议的话题

ES7 -ES11 一年一更新 2016-2020 ES6之后统称ES6

语法糖 ：ES6 写代码运行更快

## JavaScript作用

网页开发

JS包含三个部分的内容（在浏览器端

- ​		核心语法
  - 变量
  - 流程控制
  - 循环
  - 数据类型
- ​        BOM (网景)
  - 浏览器对象模型 borwer object model
- ​        DOM （W3C)
  - 文档对象模型，和网页相关  document object model



## 宿主环境

定义：JS运行的环境就叫宿主环境

- ​	node
- 浏览器



作用：

1. 交互效果

2. 收集数据

   

##  JS 的准备语法

- 如何编写JS文件

  1.  将JS嵌入到网页当中
  2.  将JS文件引入到文档中

- Script标签的位置

  1. 可以放在网页任何位置（放在html标签外面，页面最上面，浏览器自动把他放在head中，但是文档声明头消失）--> 解析时，浏览器会采用降价处理。h5-h4  【如果文档没有写文档声明头】 理论上可以放任何位置，唯独不建议放在文档声明头上面

  2. 建议的位置 </html> 之后，浏览器会默认将其移动到<body>之中。放在<body/>和</html>之间

     - js操作标签，页面元素先渲染出页面，即该标签先加载成功，script才开始获取元素。
     - 标准文档流的解析顺序
     - **操作input标签 首要前提是 input要加载成功**

  3. 独立的Js文件引入

     - <script src=''></script>

     - 通过script标签配合src配合

- 打印

  - 在JS代码调试的时候，经常需要使用到console.log()

    1. console 是一个对象，里面有很多函数可以来帮助我们来开发，最常用的是console.log

    2. 弹窗

       1. alert
       2. prompt 比confirm多一个input框框
       3. confirm 比alert多一个取消

       - ​     alert（） 加括号为一个函数

       

    3. 语句和表达式

       1. 语句
          - 有多个部分组成，不一定有结果
          - eg：console.log()
          - **语句都是从右向左看**
       2. 表达式
          - 凡是强调有结果

       

    4. 语句结束符

       1. 在语句的最后，最后写上一个语句的结束符号

    ### 文档入口函数

    window.onload = function(){

    ​	在函数当中的代码会在页面的最后执行

    }

    放在head中不会出错

    **如果页面有部分图片或资源加载不出来，则js会一直等待起加载，导致关键js交互无效果。**

    window > document

    #### 如果页面有多个入口函数

    **后面加载的覆盖前面的**

    #### onclick同理

    **外部js也需要文本入口函数**,位置放在</html>之后

    如果是工具类的js文件，（不会主动的获取js中的某个），则可以写在head文件中，如果是自己需要操作标签元素的，最好写在body之后

    PS：script  和   script src 不能混用

## 变量

​	定义：临时存储数据的容器

1. ​    如何创建一个变量

   ​	var 变量名 = 值

   - 小驼峰命名
   - console.log() 如果打印的是一个变量，不需要加引号
   - underfined当变量声明未赋值的时候，则为未定义

2. 变量名的命名规则

   - 字母数字下划线，**字符**，命名

   - 数字不能是首字母  -> **可以下划线开头**

     - 尽量词能表达意

   - 不能使用关键字

     **上面也是标识符命名规则**

   - 声明和初始化

     - 通过关键字创建一个变量即为变量声明，给变量**第一次赋值**既是初始化
     - 控制台凡是出现**蓝色**的都是数字
     - 控制台凡是出现**黑色**的都是文字
     - **程序从右向左看**
     - 修改值的过程，叫**重新赋值** x = a
     - 重新声明赋值初始化 var x = 'hello,x'  旧的值被回收

3. 变量提升

   - 浏览器的js解析引擎，在真正执行代码的时候，会进行预解析
   - 变量声明提升 声明不赋值 underfinded
   - 把所以的声明语句提升到作用域的最顶端
     - 如若未声明直接使用，则报错
   - 原因：在正式执行之前，会对其进行预解析
   - **提升到当前作用域的最顶端**

   ps：使用js属性对应的方法**length属性**，获取字符串的长度

   ​	获取字符串长度

   

   #### js是一门弱类型语言

   var a (string int)  强类型

   变量可以根据数据的需要随时更换类型（变量本身没有类型，赋的值是上面类型就是上面类型）

   

## 数据类型

**基本数据类型**

- string
- number
- Boolean
- underfinded
- null

**引用数据类型**

细化：object 

- ​	array
- ​    function
- ​     object



**栈** **堆**

基本（基础）数据类型，主要存储在栈

引用数据在栈区存放内存地址，堆里存放数据的值

**PS：******当一个引用数据类型赋值给一个变量时，这个变量存储的是简单的内存地址****



## String字符串

定义:在代码中如果想要存储文字

'' "" 没有区别，**推荐使用单引号**

- html标签属性都是双引号，为了区分。一致性不混用

- 结合实际情况，**单引号里嵌套双引号**   ...说:"".

  - 存在问题 ...说:''
  - 单引号里使用双引号，**双引号里面使用单引号**

  

#### 字符串拼接

var s1 = '鲁迅先生说:' + ''

相当于拼接字符串

#### 创建字符串   --   通过构造函数

var s = new String('')

var s1 = ''    直接量，相当于js给我们提供的一种用来创建字符串类型的快捷方式

在Js里，除了null和underfinded都可以用构造函数创建出来

不管里面是什么类型，只要外面加了引号，就会变成字符串类型

**创建字符串的方式**

- ''
- ""
- 构造函数

通过'+'拼接字符串



**注意：******

- 保证代码中使用统一的符号
- 单双不能单独嵌套使用



可以通过length属性来获取字符串的长度

**ps 除了null和underfinded其他都是对象**

**PS1 任何数据类型都可以通过构造函数的形式创造出来**

**构造函数创建出来的数据，是一个object**

//对象 console是一个对象，log是console身上的方法

length(在字符串中) 。2+



## 数字

 在js中，number远远比其他语言当中的number简单

无整数，本质上都是小数 0.2+0.3 ！== 0.5

**toFixed** //去除小数，返回整数位置，数字类型变为string（文本



## 布尔 boolean

##### 真和假 false 和true

ps：表示一个判断的结果



## null

ps：一般用来表示一个对象的空值



## Underfinded

ps：一般表示变量没有初始化

- 先声明一个变量，在某一行未来要使用某个变量，**建议给声明的变量设置一个初始值**



## **基础数据类型之间的类型转换**

ps：把一个类型的数据转换为另一个数据类型

- 自动类型数据转换
- 强制数据类型转换

#### 强制数据类型转换

NaN是JS里一个特殊值，和任何都不相等，连同自己

​		typeof（）**帮助查看数据类型**

- ​	String（）

  - ps：相当于将原数据外面加一个引号

  - 负值转换也是负值的字符串

- ​    Number（）

  - **ps ** 当其他数据类型的数据不能转换为有效的数字类型时，则被转换为NaN
  - Number('')   或空串  ->  0
  - 'hello world' -> **NaN**
  - '100 ' -> 100
  - 'a1' -> **NaN**
  - boolean true -> 1
  - boolean false -> 0

- ​    Boolean（）

  - ps:把其他数据类型转换为boolean
  - **'' 空串 -> false**
  - **0 -> false**
  - 1 -1 -> true
  - **null -> false**
  - **underfinded -> false**
  - **NaN -> false**
  - **false -> false**
  - boolean(null underfinded) -> false

**适用于number**

- parseInt（）
  - ps：强制转换为整数
  - 还适用于将其他数据类型转换为整型
  - s1 = 'abc123' -> NaN
  - s2 ='123abc' -> 123
- parseFloat() 将其他数据类型转换成float
  - 1.32a -> 1.32
  - '1a' -> 1

**typeof**

1. null object
2. underfnded underfinded

- underfinded派生于null

#### 自动类型转换

- 数字 + 字符串 --> 转换为字符串
- 数字 - '数字字符串'  -> 字符串转换成数字
- 数字 *  '数字字符串' -> 字符串转换成数字
- 数字 /    '数字字符串' -> 字符串转化成数字
- Boolean和数字计算时（+-*/），Boolean转换为数字
- 字符串数字 + 布尔 ->二者变换成字符串
- 字符串数字 -*/ 布尔  -> 都会变成数字
- 字符串 + 布尔 -> 二者都变成字符串
- 字符串 -*/ 布尔 二者都变成Number
- 任何值和NaN计算，结果都是NaN
- null 和 underfinded 都是 NaN
- **尽可能写程序时，把不可控的因素变成可控的，例如类型转换****
- 自动类型转换调用的方法就是上面提到的三个方法，Number，String，Boolean



ps：将数字以最快转换为空串 +''







### 计算

#### 除数不能为0 infinity



#### JS操作html和css修改标签样式、

1. 找到当前元素btna =  getElementById('btn')

2. 触发事件 (确定事件三要素)

   - 事件：用户在浏览器中的一切动作
     - 鼠标事件，键盘事件，文档事件
   - 事件三要素 
     - 事件源
     - 事件
     - 事件处理函数

   btna.onclick = function(){

   `	//处理的事情

   ​	alert('...')

   }

   - **事件处理函数有无不影响事件的发生**

**eg**

- 获取输入框中的内容
  - element.value



### 常用运算符

- 加
- 减
- 乘
- 除
- 求模/取余 %
- ++ -- %= 
- = != 值不等
- == 值相等  !== 值和类型其一(或）不等
- === 值和类型都相等
- < > <= >=
- && || !
  - 短路问题

console.log(new Boolean(false))
Boolean {false}

typeof(new Boolean(false))

object



**`typeof(null)`**
**`'object'`**
**`typeof(underfinded)`**
**`'undefined'`**
**`typeof(NaN)`**
**'number'**



**取反 先转换成布尔值再取反**

**取反 取反 !!x  = Boolean(x)**



### 逻辑运算符

ps:&& 和 || 存在短路问题

​	惰性面

1. &&
2. ||

**()改变运算符的优先级，里面存放都是表达式。**



### 栈和堆

栈区大小确定  -> 基本数据类型不变 var b = true

堆区随需要可以扩容



### JS 严格区分大小写



### Null 

​	是一个数据类型，在Null这个类型只有一个值，就是null.

- ​	**表示一个空值，给空对象赋值**

### Underfinded

​	是一个数据类型，在underfinded这个类型只有一个值，就是underfinded

​	派生于Null

- ​		**null == underfinded**



## 流程控制和循环

- if
  - 如果条件不是Boolean，则会强制转换为Boolean
- if else
  - else if
  - ps:嵌套过多降低代码可读性
- switch
- 三元

**Dom操作** -> 看重执行顺序

#### swith

switch(条件)

{

case'张三':;

case'张四':;

}

**多个case共用一个代码块**

case:    case 2:   ... case n: 语句 ;  break

break continue

#### switch展示结果不确定，switch保存数据

![image-20211130135105212](%E5%AD%A6%E4%B9%A0/mdimages/image-20211130135105212.png)



``` js
btn.onclick = fuction(){
    var input_value = input.value
    if(input_value.length === 0){
        alert('输入不能为空')
        return ;
    }
}
```

**trim() 去除两端的空格 -- > 字符串方法**

**如果是一个长空串，则处理为空串**

**isNaN  判断是否是NaN**

**math.random()**



### 循环

- while
- do..while
- for



random() 随机数函数

return 函数使用

continue

break

**var lis = list.children**

**获取list的children**



## 流程控制

流程控制就是来控制我们的代码按照什么结构顺序来执行。

流程控制主要有以下三种结构，分别是：

（1）顺序结构

（2）分支结构

（3）循环结构

顺序流程控制
顺序结构是程序中最简单、最基本的流程控制，它没有特定的语法结构，程序会按照代码的先后顺序，依次执行。
分支流程控制
由上到下执行代码的过程中，根据不同的条件，执行不同的路径代码，从而得到不同的结果。

### 1.if分支语句

//if分支语句的语法结构如下：
//条件成立执行代码，否则什么也不做
if(条件表达式){
	//条件成立执行的代码语句
}

### 2.if else双分支语句

//if else双分支语句的语法格式如下：
//条件成立执行if里面的代码，否则执行else里面的代码
if(条件表达式){
  //条件成立执行的代码
}else{
  //条件不成立执行的代码
}

### 3.if else if多分支语句

//if else if多分支语句的语法格式如下：
//适合检查多重条件
if(条件表达式1){
  语句1;
}else if(条件表达式2){
  语句2;
}else if(条件表达式3){
  语句3;
  ...
}else{
  //上述条件都不成立时执行此代码
}

### 4.三元表达式

由三元运算符组成的式子就称为三元表达式（因为三元表达式也是表达式，所以会有返回结果）。
//三元不等式的语法机构如下：
条件表达式 ？ 表达式1 ： 表达式2;
//如果条件表达式的结果为真，则返回表达式1点值；而如果条件表达式的结果为假，则返回表达式2的值。

### 5.switch语句

switch语句也是多分支语句，它用于基于不同的条件来执行不同的代码。当要针对变量设置一系列的特定值的选项时，就可以使用switch。
//switch语句的语法格式如下：
switch(表达式){
  case value1:
    //表达式等于value1时要执行的代码
    break;
  case value2:
    //表达式等于value2时要执行的代码
    break;
  default:
    //表达式不等于任何一个value值时要执行的代码
}

switch语句中的条件表达式我们经常写成变量的形式。
表达式的值与value的值相匹配时，要求值和数据类型一致才算匹配成功。
如果当前case里面没有break，则不回退出switch语句，而是继续执行下一个case，直到遇到break或执行default语句才退出switch语句。

### 6.switch语句和if else if语句的区别

一般情况下，它们两个语句可以相互替换。
switch…case语句通常处理case为比较确定值的情况，而if…else…语句更加灵活，常用于范围判断（大于、等于某个范围）。
switch语句进行条件判断后直接执行到程序的条件语句，而if…else…语句有几种条件，就得判断几次（从上到下依次判断）。
当分支比较少时，if…else…语句的执行效率比switch语句更高。
当分支比较多时，switch语句的执行效率比if…else…语句更高，结构也更清晰。

## 循环流程控制

目的：重复执行某些代码。

在JS中，主要有以下三种类型的循环语句：

（1）for循环

（2）while循环

（3）do…while循环

### 1.for循环

在程序中，一组被重复执行的语句被称为循环体，而能否继续重复执行，则取决于循环的终止条件。
循环语句是指由循环体和循环的终止条件造成的句子。
for循环主要用于把某些代码循环若干次，通常跟计数有关系。
//for循环的语法结构如下：
for(初始化变量;条件表达式;操作表达式){
  //循环体
}
//初始化变量：其实就是用var声明一个普通的变量，并且这个变量通常是作为计数器使用。
//条件表达式：用来决定每一次循环是否继续执行，也就是循环的终止条件。
//操作表达式：每次循环最后执行的代码，经常用于更新计数器（递增或递减）。

for循环可以重复执行相同的代码。
因为有计数器的存在，使用for循环可以重复执行不同代码。
for(var i = 1; i <= 31; i++){
  console.log('今天是5月'+i+'日了！');

因为有计数器的存在，使用for循环可以重复执行某些操作，比如：算数运算。
var sum = 0;
for(var i = 1; i <= 10; i++){
  sum+=i;
}
console.log(sum);

循环嵌套是指在一个循环语句中再定义一个循环语句的语法结构。像在一个for循环语句中， 再嵌套一个for循环，我们就称之为双重for循环。
for(var i = 1; i <= 3; i++){
  console.log('这是第'+i+'次外层循环');
  for(var j = 1; j <= 3; j++){
    console.log('这是第'+i+'次里层循环');
  }
}

### 2.while循环

while语句可以在条件表达式为真的前提下，循环执行指定的一段代码，直到表达式不为真时结束循环。
//while语句的语法结构如下：
while(条件表达式){
  //循环体代码
}

### 3.do while循环

do…while语句其实是while语句的一个变体。该循环会先执行一次代码块，然后对条件表达式进行判断，如果条件为真，就会重复执行循环体，否则退出循环。（do while循环语句至少执行一次循环体代码）
//do while语句的语法结构如下：
do{
  //循环体代码
}while(条件表达式);

### 4.小结

三种循环在很多情况下都是可以相互替代使用的。
对于计数，三者的使用基本相同，但是更常用的还是for循环。
对于更复杂的判断条件，可以使用while和do while语句。

**while语句是先判断后执行；而do while是先执行一次后判断执行。**

##### do while 至少执行一次，while，for最少执行0次

continue和break

#### 1.continue关键字

continue关键字用于立即跳出本次循环，继续下一次循环（本次循环体中continue之后的代码就会少执行一次）。
var sum = 0;
for(var i = 1; i <= 100; i++){
  if(i % 7 == 0){
    continue;
  }
  sum+=i;
}
console.log(sum);

#### 2.break关键字

break关键字用于立即跳出整个循环。
for(var i = 1; i <=5; i++){
  if(i == 3){
    break;
  }
  console.log('这是第'+i+'个数！');
}



### ...className = '' 去掉类名

data-XXX =''  html5自定义类型存数据

getAttribute





### 数组

#### 使用情况

1. 一系列的值
2. 希望快速存储和取数据的时候



- 在向数据中插入元素的时候，如果索引值不连续，就会形成空位

- 对于索引值不连续的数组，称之为稀疏数组
- length 获取数组元素
- length比索引值大1
- PS ：如果length设置比其大，则后都是空位
  - 如果length设置比索引值小，则从后面清楚
  - 快速清空数组，arr.length = 0



1. **length失效****一**

   - 数组添加元素key不为数组，则不会被计算

   - arr['name'] = '哈哈哈' 计算数组长度的时候，当前元素失效
   - 长度 == 任然是最大索引值加1  只计算数字索引

2. **length失效****一**
   - arr[100] = 'hhh'/
   - console.log(arr.length) //101



for可以以索引取字符串的值



**字符串后面用方括号**

**对象.属性（方法**

数组. 后面只能是一个符合

**对象[属性]  允许使用方括号解析属性**

**obj 对象 有一个 hello 属性**

**var a = 'hello'**

**obj[a] --> obj['hello]   js引擎将a解析为一个变量**

**--数组是一种特殊的对象 key-value**

**--特殊类型（凡是对象key都是字符串，）**

- ​	**数组的key看上去是一个number，实际上会给他转换为string**
- **一切皆对象，除了null和underfinded很多数据类型都对象的特点，有属性和方法，以key-value为结构，key的类型永远是字符串，调用属性的时候可以用点可以用方括号**
- 数组的key可以为字符串



### 空位

```jsx
// 产生5个数组空位的数组
let arr1 = Array(5)

//同上3
let arr2 = [,2,,3,,]

//产生10个数组空位的数组
let arr3 = []
arr3.length = 10

//数组a[3]-a[9]都是数组空位
let arr4 = [1,2,3]
arr4[10] = 5

//数组arr[2]产生数组空位
let arr5 = [1,2,3,4,5]
delete arr[2]
```



#### 数组的增删改查

- delect[n] 通过delect删除元素会形成稀疏数组，形成空位（存在空位，索引值不连续
- arr[0] ='xxx'
- console.log(arr[0])

数组的空位可以访问，值为**underfinded**

**当数组为一个正常数组时，可以通过length来清楚数组（不存在索引值不为非数字的值**

数组遍历优化或提高效率

var len = arr.length

for(;i<len)



## api (接口方法)

- **push向数组的最后插入一个元素** push(a,b)一次可以添加多个值，unshift同
- **arr.shift() 删除第一个数据元素，并返回给我们删除的元素**
- **pop() 删除数组的最后一个元素，并且返回删除的最后一个元素**
- **unshift() 向数组的最前方插入一个元素**

- splice(索引值，删除数组元素的个数，插入的元素) 删除元素 替换元素 插入元素
  - 删除
    - arr.splice(1,1) 从索引一开始，删除一元素
    - arr.splice(1,1,'xxx') 。。。 替换一元素 值为xxx
    - arr.splice(2,0,'xxx') 在第三个位置插入xxx
  - 

所有的表单控件必须用value，初此之外必须使用，innerHtml,innerText

resize:none  ->删除textarea的可变  不让其影响其他元素

dotted 虚线效果（solid）

 

## 数组方法 

- find 

- findIndex

- forEach

forEach(item,index)

find可以返回当前元素所在的数组

## 



### 对象（object）



#### 对象包含两种 **属性和方法**

#### 创建对象的方法

var obj = {}

- 添加元素

  obj['username'] = '张三'   数组的key可以是数字或者字符串

  var str = ’like‘

  obj[str] = '电影'

  - ​	str 不加引号，可以作为变量被解析
  - obj.age === obj['age']
    - **区别**：方括号中可以解析变量，**如上**
  - **对象.一定是字符串**
    - obj.str str默认为字符串

- 方法的写法

  - obj['say'] = function(){

    ....}

  - 对象中的方法需要手动调用

- 添加属性和方法

  - ```js
    //创建对象时添加内容，一定采用key-value的形式
    - - var obj = {
        }
    
    
    ```

  - 变量名和对象名相同，需要在变量的外部加一个方括号

    - [str]:'值'，

- **key值的设定需要符合标识符命名规则**

- 对象的方法里面如何使用对象的属性和方法 （this代表事件源）

  - this放在对象的方法中，即指向该对象

- 向对象中添加属性和方法 

  - obj.a = '';
  - obj.c = function(){}

- 删除数组 中的属性和方法

  - delece obj.a;
  - object obj[''];

- 取或调用数组中的属性和方法

- 遍历对象

  - ​	`for(var key in obj){`

    ​	`console.log(obj[key])`

    }

- 1

### 对象和数组进行比较

1. //数组时有序的，对象是无序的
2. 数组的key必须是number 对象的key是字符串，数组是一种特殊的对象，虽然数组的key是number，但在解析时，会被自动转换成字符串
3. 数组操作元素只能[]对象，对象可以通过  .  和 []
4. 数组的遍历可以使用foreach，for ，for in，对象只能使用for in
5. 当key值重复时，value会发生覆盖



#### 对象方法Object.keys(object)





### 函数

##### 定义 ：一段代码的集合体

特点：不调用时为静止片段，不调用不执行。减少工作量

#### 创建函数的方式

1. function 函数名(){

   语句；

   }

2. 函数表达式

   - var 。。。 = function    在js中，函数地位和基础数据类型相同，数据能做什么，函数就能做什么

   var 变量 = function(){

   ​	语句；

   }

   - ​	将函数放在函数的右边，js引擎就会把函数理解为一个表达式
   - 一般来说说，表达式都放在等号的右边

3. var fn = new function(){

   }



### 函数名

理解为变量名  -> 符合标识符命名规则

- ​	给函数起名字时，不要首字母大小，
  - 凡是大写字母开头都是构造函数
- 形参和实参
  - 形参：理解为只能用在内部的变量
    - 调用函数的时候给形参赋值，即实参赋值给形参
  - 实参：
  - ps：



### 返回值

需求:通过add函数计算出100+200的值，然后通过msg函数结果以弹窗将结果以弹窗的形式弹出

- 将函数处理后的结果在函数外拿到



### callback回调函数

把函数当参数传递给另一个函数

##### 没有名字的函数叫匿名函数



1. 函数创建方式：
   1. `function`关键字 
   2. 函数表达式
   3. 构造函数

> 通过函数表达式创建的函数不能提前使用。因为函数表达式的创建形式相当于是把一个匿名函数赋值给了变量，虽然变量和函数都会提升，但是赋值的操作不会提升。所以也就不能提前使用函数。



> 不管是变量也好还是函数也好，提升终归不是一个好的事情，所以对于程序员来说，知道这个规则但是我们尽量不去使用。



```js
// 尽量不在函数体之前使用函数
fn1();
function fn1() {}
```

2. 函数名
   1. 函数的命名遵守标识符的规则
   2. 尽量不要首字母大写，因为在js中首字母大写是属于构造函数(class类)
3. 函数的参数
   1. 形参(创建函数时，函数名之后的括号里放的就是形参列表)
   2. 实参(调用函数时，函数名后面的括号放的就是实参列表)



​	关于形参和实参：

  1. 形参的个数多于实参的个数

     ```js
     function fn1(a,b,c) {}
     
     fn1(10,20); 
     ```

     多余的形参值是undefined。

     > 完全可以把形参理解为js中的变量，只不过这个变量只能用在函数中，而实参其实就是给这个变量赋值。
     >
     > 如果实参的个数少于形参，那就会出现多余的形参没有被赋值，自然结果就和未赋值的变量相同。

  2. 实参的个数多余形参的个数

     ```js
     function fn1(a,b) {}
     fn1(10,20,30,40);
     ```

     当实参的个数多于形参的个数时，想要找到多余的实参，可以利用arguments(js提供的专门在函数当中使用的类数组对象)。

     可以通过arguments.length获取实参的个数。

     可以通过arguments.callee.length 获取形参的个数。



4. 函数返回值

   js的函数一旦创建完毕，那么就会在内存中停留但是不会被执行，只有函数调用的时候，函数代码才会执行。在执行的过程中如果碰到了return，那么函数就会立刻停止执行。

   在一个函数中，同一时刻有且仅有一个return可以生效。

   return只能返回一个值。





------

1. 作用域和变量

   1. 作用域：代码生效的范围。

   在es5当中，作用域只有两个：

   1. 全局作用域：整个script存在的空间
   2. 函数作用域：函数内部的空间

既然存在两个作用域，那么对应的在不同作用域下声明的变量，也就有了不同的称呼：

* 全局变量(在全局作用域下声明的变量，生效范围全局)
* 局部变量(在函数内部声明的变量，生效范围仅限于函数的内部)



2. 作用域链

   *由小范围的作用域不断向上延申扩展最终形成的一系列作用域，称之为作用域链。*

3. 生命周期和垃圾回收机制

4. 闭包函数

   1. 在函数内部将一个函数当作返回值返回，这样的函数结构称之为闭包函数。
   2. 特点：返回的函数可以任意的通过作用域链拿到上一层函数的值。
      1. 会将状态保存，并不会在函数执行完毕之后销毁函数内部的变量。
   3. 缺点：不会及时的被销毁。

5. 回调函数

   函数在js中是一等公民，所以基础数据类型能干的事，函数也可以干。

   所以就可以拿函数当作一个参数传递给另外一个函数。

   ```js
   function jsq(num1, num2, cb) {
       return cb(num1, num2);
   }
   ```

   

6. 递归函数

   所谓的递归函数就是在函数体内调用本函数。

7. IIFE



### 闭包函数

``` js
function test(){
	var temp = 100;
    function a() {
		console.log(temp);
    }
    return a;
}
var demo = test();
demo();
// 凡是多个函数嵌套，里面的函数都保存到了外部函数的外部（即外部)里面函数在执行时，可以继续访问外部函数未销毁之前外部变量函数的变量，且不会报错
```



``` js
//经典闭包案例
//闭包里面的匿名函数，先不去找i的值，仅仅先创建了10个闭包函数，最后执行完循环，i=10不满足条件，然后执行这些闭包函数
//    此时再来找i的值，i的值未10，最终结果为10个10
function test(){
	var arr = [];
    for(var i = 0;i<10;i++)
        {
			arr[i] = function(){
				documen.write(i+' ');	
            }
        }
    return arr;
 }
var myArr = test();
for(var j = 0;j < 10;j++){
	myArr[j]();
}
```

``` js
 for (let i = 0; i < arr.length; i++) {            var divall = document.createElement('div');            divall.setAttribute('class', 'noteSome');            divall.innerHTML = getModule(arr[i].ipt_value, arr[i].text_value, arr[i].time, arr[i].note_id)            artile.append(divall);
```



### 字符串方法

#### 1. 获取字符串长度

JavaScript中的字符串有一个length属性，该属性可以用来获取字符串的长度：

```javascript
const str = 'hello';str.length   // 输出结果：5复制代码
```

#### 2. 获取字符串指定位置的值

charAt()和charCodeAt()方法都可以通过索引来获取指定位置的值：

- charAt() 方法获取到的是指定位置的字符；
- charCodeAt()方法获取的是指定位置字符的Unicode值。

#### （1）charAt()

charAt() 方法可以返回指定位置的字符。其语法如下：

```javascript
string.charAt(index)复制代码
```

index表示字符在字符串中的索引值：

```javascript
const str = 'hello';
str.charAt(1)  // 输出结果：e 
复制代码
```

我们知道，字符串也可以通过索引值来直接获取对应字符，那它和charAt()有什么区别呢？来看例子：

```javascript
const str = 'hello';
str.charAt(1)  // 输出结果：e 
str[1]         // 输出结果：e 
str.charAt(5)  // 输出结果：'' 
str[5]         // 输出结果：undefined
复制代码
```

可以看到，当index的取值不在str的长度范围内时，str[index]会返回undefined，而charAt(index)会返回空字符串；除此之外，str[index]不兼容ie6-ie8，charAt(index)可以兼容。

#### （2）charCodeAt()

`charCodeAt()`：该方法会返回指定索引位置字符的 Unicode 值，返回值是 0 - 65535 之间的整数，表示给定索引处的 UTF-16 代码单元，如果指定位置没有字符，将返回 **NaN**：

```javascript
let str = "abcdefg";
console.log(str.charCodeAt(1)); // "b" --> 98
复制代码
```

通过这个方法，可以获取字符串中指定Unicode编码值范围的字符。比如，数字0～9的Unicode编码范围是: 48～57，可以通过这个方法来筛选字符串中的数字，当然如果你更熟悉正则表达式，会更方便。

#### 3. 检索字符串是否包含特定序列

这5个方法都可以用来检索一个字符串中是否包含特定的序列。其中前两个方法得到的指定元素的索引值，并且只会返回第一次匹配到的值的位置。后三个方法返回的是布尔值，表示是否匹配到指定的值。

注意：这5个方法都对大小写敏感！

#### （1）indexOf()

`indexOf()`：查找某个字符，**有则返回第一次匹配到的位置**，否则返回-1，其语法如下：

```javascript
string.indexOf(searchvalue,fromindex)
复制代码
```

该方法有两个参数：

- searchvalue：必需，规定需检索的字符串值；
- fromindex：可选的整数参数，规定在字符串中开始检索的位置。它的合法取值是 0 到 string.length - 1。如省略该，则从字符串的首字符开始检索。

```javascript
let str = "abcdefgabc";console.log(str.indexOf("a"));   // 输出结果：0console.log(str.indexOf("z"));   // 输出结果：-1console.log(str.indexOf("c", 4)) // 输出结果：9复制代码
```

#### （2）lastIndexOf()

`lastIndexOf()`：查找某个字符，有则返回最后一次匹配到的位置，否则返回-1

```javascript
let str = "abcabc";console.log(str.lastIndexOf("a"));  // 输出结果：3console.log(str.lastIndexOf("z"));  // 输出结果：-1复制代码
```

该方法和indexOf()类似，只是查找的顺序不一样，indexOf()是正序查找，lastIndexOf()是逆序查找。

#### （3）includes()

`includes()`：该方法用于判断字符串是否包含指定的子字符串。如果找到匹配的字符串则返回 true，否则返回 false。该方法的语法如下：

```javascript
string.includes(searchvalue, start)复制代码
```

该方法有两个参数：

- searchvalue：必需，要查找的字符串；
- start：可选，设置从那个位置开始查找，默认为 0。

```javascript
let str = 'Hello world!';str.includes('o')  // 输出结果：truestr.includes('z')  // 输出结果：falsestr.includes('e', 2)  // 输出结果：false复制代码
```

#### （4）startsWith()

`startsWith()`：该方法用于检测字符串**是否以指定的子字符串开始**。如果是以指定的子字符串开头返回 true，否则 false。其语法和上面的includes()方法一样。

```javascript
let str = 'Hello world!';str.startsWith('Hello') // 输出结果：truestr.startsWith('Helle') // 输出结果：falsestr.startsWith('wo', 6) // 输出结果：true复制代码
```

#### （5）endsWith()

`endsWith()`：该方法用来判断当前字符串**是否是以指定的子字符串结尾**。如果传入的子字符串在搜索字符串的末尾则返回 true，否则将返回 false。其语法如下：

```javascript
string.endsWith(searchvalue, length)复制代码
```

该方法有两个参数：

- searchvalue：必需，要搜索的子字符串；
- length： 设置字符串的长度，默认值为原始字符串长度 string.length。

```javascript
let str = 'Hello world!';str.endsWith('!')       // 输出结果：truestr.endsWith('llo')     // 输出结果：falsestr.endsWith('llo', 5)  // 输出结果：true复制代码
```

可以看到，**当第二个参数设置为5时，就会从字符串的前5个字符中进行检索，所以会返回true。**

#### 4. 连接多个字符串

concat() 方法用于连接两个或多个字符串。该方法不会改变原有字符串，会返回连接两个或多个字符串的新字符串。其语法如下：

```javascript
string.concat(string1, string2, ..., stringX)复制代码
```

其中参数 string1, string2, ..., stringX 是必须的，他们将被连接为一个字符串的一个或多个字符串对象。

```javascript
let str = "abc";console.log(str.concat("efg"));          //输出结果："abcefg"console.log(str.concat("efg","hijk")); //输出结果："abcefghijk"复制代码
```

虽然concat()方法是专门用来拼接字符串的，但是在开发中使用最多的还是加操作符+，因为其更加简单。

#### 5. 字符串分割成数组

split() 方法用于把一个字符串分割成字符串数组。该方法不会改变原始字符串。其语法如下：

```javascript
string.split(separator,limit)复制代码
```

该方法有两个参数：

- separator：必需。字符串或正则表达式，从该参数指定的地方分割 string。
- limit：可选。该参数可指定返回的数组的最大长度。如果设置了该参数，返回的子串不会多于这个参数指定的数组。如果没有设置该参数，整个字符串都会被分割，不考虑它的长度。

```javascript
let str = "abcdef";
str.split("c");    // 输出结果：["ab", "def"]
str.split("", 4)   // 输出结果：['a', 'b', 'c', 'd'] 
复制代码
```

如果把空字符串用作 separator，那么字符串中的每个字符之间都会被分割。

```javascript
str.split("");     // 输出结果：["a", "b", "c", "d", "e", "f"]
复制代码
```

其实在将字符串分割成数组时，可以同时拆分多个分割符，使用正则表达式即可实现：

```javascript
const list = "apples,bananas;cherries"
const fruits = list.split(/[,;]/)
console.log(fruits);  // 输出结果：["apples", "bananas", "cherries"]
复制代码
```

#### 6. 截取字符串

substr()、substring()和 slice() 方法都可以用来截取字符串。

#### （1） slice()

slice() 方法用于提取字符串的某个部分，并以新的字符串返回被提取的部分。其语法如下：

```javascript
string.slice(start,end)
复制代码
```

该方法有两个参数：

- start：必须。 要截取的片断的起始下标，第一个字符位置为 0。如果为负数，则从尾部开始截取。
- end：可选。 要截取的片段结尾的下标。若未指定此参数，则要提取的子串包括 start 到原字符串结尾的字符串。如果该参数是负数，那么它规定的是从字符串的尾部开始算起的位置。

上面说了，如果start是负数，则该参数规定的是从字符串的尾部开始算起的位置。也就是说，-1 指字符串的最后一个字符，-2 指倒数第二个字符，以此类推：

```javascript
let str = "abcdefg";
str.slice(1,6);   // 输出结果："bcdef" 
str.slice(1);     // 输出结果："bcdefg" 
str.slice();      // 输出结果："abcdefg" 
str.slice(-2);    // 输出结果："fg"
str.slice(6, 1);  // 输出结果：""
复制代码
```

注意，该方法返回的子串**包括开始处的字符**，但**不包括结束处的字符**。

#### （2） substr()

substr() 方法用于在字符串中抽取从开始下标开始的指定数目的字符。其语法如下：

```javascript
string.substr(start,length)复制代码
```

该方法有两个参数：

- start	必需。要抽取的子串的起始下标。必须是数值。如果是负数，那么该参数声明从字符串的尾部开始算起的位置。也就是说，-1 指字符串中最后一个字符，-2 指倒数第二个字符，以此类推。
- length：可选。子串中的字符数。必须是数值。如果省略了该参数，那么返回从 stringObject 的开始位置到结尾的字串。

```javascript
let str = "abcdefg";str.substr(1,6); // 输出结果："bcdefg"
str.substr(1);   // 输出结果："bcdefg" 相当于截取[1,str.length-1]
str.substr();    // 输出结果："abcdefg" 相当于截取[0,str.length-1]
str.substr(-1);  // 输出结果："g"复制代码
```

#### （3） substring()

substring() 方法用于提取字符串中介于两个指定下标之间的字符。其语法如下：

```javascript
string.substring(from, to)复制代码
```

该方法有两个参数：

- from：必需。一个非负的整数，规定要提取的子串的第一个字符在 string 中的位置。
- to：可选。一个非负的整数，比要提取的子串的最后一个字符在 string 中的位置多 1。如果省略该参数，那么返回的子串会一直到字符串的结尾。

**注意：** 如果参数 from 和 to 相等，那么该方法返回的就是一个空串（即长度为 0 的字符串）。如果 from 比 to 大，那么该方法在提取子串之前会先交换这两个参数。并且该方法不接受负的参数，如果参数是个负数，就会返回这个字符串。

```javascript
let str = "abcdefg";str.substring(1,6); // 输出结果："bcdef" [1,6)
 str.substring(1);   // 输出结果："bcdefg"[1,str.length-1]
str.substring();    // 输出结果："abcdefg" [0,str.length-1]
str.substring(6,1); // 输出结果 "bcdef" [1,6)
str.substring(-1);  // 输出结果："abcdefg"复制代码
```

注意，该方法返回的子串**包括开始处的字符**，但**不包括结束处的字符**。

#### 7. 字符串大小写转换

toLowerCase() 和 toUpperCase()方法可以用于字符串的大小写转换。

#### （1）toLowerCase()

`toLowerCase()`：该方法用于把字符串转换为小写。

```javascript
let str = "adABDndj";str.toLowerCase(); // 输出结果："adabdndj"复制代码
```

#### （2）toUpperCase()

`toUpperCase()`：该方法用于把字符串转换为大写。

```javascript
let str = "adABDndj";str.toUpperCase(); // 输出结果："ADABDNDJ"复制代码
```

我们可以用这个方法来将字符串中第一个字母变成大写：

```javascript
let word = 'apple'
word = word[0].toUpperCase() + word.substr(1)
console.log(word) // 输出结果："Apple"复制代码
```

#### 8. 字符串模式匹配

replace()、match()和search()方法可以用来匹配或者替换字符。

#### （1）replace()

`replace()`：该方法用于在字符串中用一些字符替换另一些字符，或替换一个与正则表达式匹配的子串。其语法如下：

```javascript
string.replace(searchvalue, newvalue)复制代码
```

该方法有两个参数：

- searchvalue：必需。规定子字符串或要替换的模式的 RegExp 对象。如果该值是一个字符串，则将它作为要检索的直接量文本模式，而不是首先被转换为 RegExp 对象。
- newvalue：必需。一个字符串值。规定了替换文本或生成替换文本的函数。

```javascript
let str = "abcdef";
str.replace("c", "z") // 输出结果：abzdef
复制代码
```

执行一个全局替换, 忽略大小写:

```javascript
let str="Mr Blue has a blue house and a blue car";
str.replace(/blue/gi, "red");    // 输出结果：'Mr red has a red house and a red car'
复制代码
```

**注意：** 如果 regexp 具有全局标志 g，那么 replace() 方法将替换所有匹配的子串。否则，它只替换第一个匹配子串。

#### （2）match()

`match()`：该方法用于在字符串内检索指定的值，或找到一个或多个正则表达式的匹配。该方法类似 indexOf() 和 lastIndexOf()，但是它返回指定的值，而不是字符串的位置。其语法如下：

```javascript
string.match(regexp)
复制代码
```

该方法的参数 regexp 是必需的，规定要匹配的模式的 RegExp 对象。如果该参数不是 RegExp 对象，则需要首先把它传递给 RegExp 构造函数，将其转换为 RegExp 对象。

**注意：** 该方法返回存放匹配结果的数组。该数组的内容依赖于 regexp 是否具有全局标志 g。

```javascript
let str = "abcdef";
console.log(str.match("c")) // ["c", index: 2, input: "abcdef", groups: undefined]
复制代码
```

#### （3）search()

`search()`方法用于检索字符串中指定的子字符串，或检索与正则表达式相匹配的子字符串。其语法如下：

```javascript
string.search(searchvalue)
复制代码
```

该方法的参数 regex 可以是需要在 string 中检索的子串，也可以是需要检索的 RegExp 对象。

**注意：** 要执行忽略大小写的检索，请追加标志 i。该方法不执行全局匹配，它将忽略标志 g，也就是只会返回第一次匹配成功的结果。如果没有找到任何匹配的子串，则返回 -1。

**返回值：** 返回 str 中第一个与 regexp 相匹配的子串的起始位置。

```javascript
let str = "abcdef";str.search(/bcd/)   // 输出结果：1复制代码
```

#### 9. 移除字符串收尾空白符

trim()、trimStart()和trimEnd()这三个方法可以用于移除字符串首尾的头尾空白符，空白符包括：空格、制表符 tab、换行符等其他空白符等。

#### （1）trim()

trim() 方法用于移除字符串首尾空白符，该方法不会改变原始字符串：

```javascript
let str = "  abcdef  "str.trim()    // 输出结果："abcdef"复制代码
```

注意，该方法不适用于null、undefined、Number类型。

#### （2）trimStart()

trimStart() 方法的的行为与`trim()`一致，不过会返回一个**从原始字符串的开头删除了空白的新字符串**，不会修改原始字符串：

```javascript
const s = '  abc  ';s.trimStart()   // "abc  "复制代码
```

#### （3）trimEnd()

trimEnd() 方法的的行为与`trim()`一致，不过会返回一个**从原始字符串的结尾删除了空白的新字符串**，不会修改原始字符串：

```javascript
const s = '  abc  ';s.trimEnd()   // "  abc"复制代码
```

#### 10. 获取字符串本身

valueOf()和toString()方法都会返回字符串本身的值，感觉用处不大。

#### （1）valueOf()

`valueOf()`：返回某个字符串对象的原始值，该方法通常由 JavaScript 自动进行调用，而不是显式地处于代码中。

```javascript
let str = "abcdef"
console.log(str.valueOf()) // "abcdef"
复制代码
```

#### （2）toString()

`toString()`：返回字符串对象本身

```javascript
let str = "abcdef"
console.log(str.toString()) // "abcdef"
复制代码
```

#### 11. 重复一个字符串

repeat() 方法返回一个新字符串，表示将原字符串重复n次：

```javascript
'x'.repeat(3)     // 输出结果："xxx"
'hello'.repeat(2) // 输出结果："hellohello"
'na'.repeat(0)    // 输出结果：""
复制代码
```

如果参数是小数，会向下取整：

```javascript
'na'.repeat(2.9) // 输出结果："nana"
复制代码
```

如果参数是负数或者Infinity，会报错：

```javascript
'na'.repeat(Infinity)   // RangeError
'na'.repeat(-1)         // RangeError
复制代码
```

如果参数是 0 到-1 之间的小数，则等同于 0，这是因为会先进行取整运算。0 到-1 之间的小数，取整以后等于-0，repeat视同为 0。

```javascript
'na'.repeat(-0.9)   // 输出结果：""
复制代码
```

如果参数是NaN，就等同于 0：

```javascript
'na'.repeat(NaN)    // 输出结果：""
复制代码
```

如果repeat的参数是字符串，则会先转换成数字。

```javascript
'na'.repeat('na')   // 输出结果：""
'na'.repeat('3')    // 输出结果："nanana"
复制代码
```

#### 12. 补齐字符串长度

padStart()和padEnd()方法用于补齐字符串的长度。如果某个字符串不够指定长度，会在头部或尾部补全。

#### （1）padStart()

`padStart()`用于头部补全。该方法有两个参数，其中第一个参数是一个数字，表示字符串补齐之后的长度；第二个参数是用来补全的字符串。 

如果原字符串的长度，等于或大于指定的最小长度，则返回原字符串：

```javascript
'x'.padStart(1, 'ab') // 'x'复制代码
```

如果用来补全的字符串与原字符串，两者的长度之和超过了指定的最小长度，则会截去超出位数的补全字符串：

```javascript
'x'.padStart(5, 'ab') // 'ababx''x'.padStart(4, 'ab') // 'abax'复制代码
```

如果省略第二个参数，默认使用空格补全长度：

```javascript
'x'.padStart(4) // '   x'复制代码
```

padStart()的常见用途是为数值补全指定位数，笔者最近做的一个需求就是将返回的页数补齐为三位，比如第1页就显示为001，就可以使用该方法来操作：

```javascript
"1".padStart(3, '0')   // 输出结果： '001'
"15".padStart(3, '0')  // 输出结果： '015'
复制代码
```

#### （2）padEnd()

`padEnd()`用于尾部补全。该方法也是接收两个参数，第一个参数是字符串补全生效的最大长度，第二个参数是用来补全的字符串：

```javascript
'x'.padEnd(5, 'ab') // 'xabab'
'x'.padEnd(4, 'ab') // 'xaba'
复制代码
```

#### 13. 字符串转为数字

parseInt()和parseFloat()方法都用于将字符串转为数字。

#### （1）parseInt()

parseInt() 方法用于可解析一个字符串，并返回一个整数。其语法如下：

```javascript
parseInt(string, radix)
复制代码
```

该方法有两个参数：

- string：必需。要被解析的字符串。
- radix：可选。表示要解析的数字的基数。该值介于 2 ~ 36 之间。



当参数 radix 的值为 0，或没有设置该参数时，parseInt() 会根据 string 来判断数字的基数。

```javascript
parseInt("10");			  // 输出结果：10
parseInt("17",8);		  // 输出结果：15 (8+7)
parseInt("010");		  // 输出结果：10 或 8
复制代码
```

当参数 radix 的值以 “0x” 或 “0X” 开头，将以 16 为基数：

```javascript
parseInt("0x10")      // 输出结果：16
复制代码
```

如果该参数小于 2 或者大于 36，则 parseInt() 将返回 NaN：

```javascript
parseInt("50", 1)      // 输出结果：NaN
parseInt("50", 40)     // 输出结果：NaN
复制代码
```

只有字符串中的第一个数字会被返回，当遇到第一个不是数字的字符为止:

```javascript
parseInt("40 4years")   // 输出结果：40
复制代码
```

如果字符串的第一个字符不能被转换为数字，就会返回 NaN：

```javascript
parseInt("new100")     // 输出结果：NaN
复制代码
```

字符串开头和结尾的空格是允许的：

```javascript
parseInt("  60  ")    // 输出结果： 60
复制代码
```

#### （2）parseFloat()

parseFloat() 方法可解析一个字符串，并返回一个浮点数。该方法指定字符串中的首个字符是否是数字。如果是，则对字符串进行解析，直到到达数字的末端为止，然后以数字返回该数字，而不是作为字符串。其语法如下：

```javascript
parseFloat(string)
复制代码
```

parseFloat 将它的字符串参数解析成为浮点数并返回。如果在解析过程中遇到了正负号（+ 或 -）、数字 (0-9)、小数点，或者科学记数法中的指数（e 或 E）以外的字符，则它会忽略该字符以及之后的所有字符，返回当前已经解析到的浮点数。同时参数字符串首位的空白符会被忽略。

```javascript
parseFloat("10.00")      // 输出结果：10.00
parseFloat("10.01")      // 输出结果：10.01
parseFloat("-10.01")     // 输出结果：-10.01
parseFloat("40.5 years") // 输出结果：40.5

复制代码
```

如果参数字符串的第一个字符不能被解析成为数字，则 parseFloat 返回 NaN。

```javascript
parseFloat("new40.5")    // 输出结果：NaN
```



编码和解码 encode URI() 和 encodeURICompoent()

![image-20211213203747451](%E5%AD%A6%E4%B9%A0/mdimages/image-20211213203747451.png)









### 数组方法



#### 1.测试所有元素

- #### every（）

  - 测试一个数组内的所有元素是否都能通过某个指定函数的测试。它返回一个布尔值。
  - 里面时一个回调函数 留可以传index item array(原数组)

#### 2.填充一个数组中从起始索引到终止索引内的全部元素。(fill)

- #### fill()

- 前提，数组已经存在位置，如果数组为空或未初始化则无效

  ``` js
  const array1 = [1, 2, 3, 4];
  
  // fill with 0 from position 2 until position 4
  console.log(array1.fill(0, 2, 4));
  // expected output: [1, 2, 0, 0]
  
  // fill with 5 from position 1
  console.log(array1.fill(5, 1));
  // expected output: [1, 5, 5, 5]
  
  console.log(array1.fill(6));
  // expected output: [6, 6, 6, 6]
  
  item start conut
  第一个参数：用item填充 必须
  第二个参数：填充开始的位置 省略则为0
  第三分参数：填充多少个  默认为length
  ```

  

#### 3.合并两个或多个数组。

- #### `**concat()**` 方法

- ``` js
  //合并多个数组
  var num1 = [1, 2, 3],
      num2 = [4, 5, 6],
      num3 = [7, 8, 9];
  var nums = num1.concat(num2, num3);
  console.log(nums);
  // results in [1, 2, 3, 4, 5, 6, 7, 8, 9]
  
  //将值连接到数组 
  // 和下面合并多维很相似
  var alpha = ['a', 'b', 'c'];
  var alphaNumeric = alpha.concat(1, [2, 3]);
  console.log(alphaNumeric);
  // results in ['a', 'b', 'c', 1, 2, 3]
  
  //合并多维数组
  //会把第一层[]拆开拿到里面的元素
  var num1 = [[1]];
  var num2 = [2, [3]];
  var num3=[5,[6]];
  var nums = num1.concat(num2);
  console.log(nums);
  // results is [[1], 2, [3]]
  
  //混合合并
  var nums2=num1.concat(4,num3);
  console.log(nums2)
  // results is [[1], 4, 5,[6]]
  ```

#### 4.过滤所有元素

	##### filter()

- ##### 将满足条件的放入一个新数组

  - 里面为一个回调函数 

  - element  数组中当前正在处理的元素。

  - ndex 数组中当前正在处理的元素的索引

  - array（调用了 `filter` 的数组本身。）

  - ```js
    var arr = [
      { id: 15 },
      { id: -1 },
      { id: 0 },
      { id: 3 },
      { id: 12.2 },
      { },
      { id: null },
      { id: NaN },
      { id: 'undefined' }
    ];
    
    var invalidEntries = 0;
    
    function isNumber(obj) {
      return obj !== undefined && typeof(obj) === 'number' && !isNaN(obj);
    }
    
    function filterByID(item) {
      if (isNumber(item.id) && item.id !== 0) {
        return true;
      }
      invalidEntries++;
      return false;
    }
    
    var arrByID = arr.filter(filterByID);
    
    console.log('Filtered Array\n', arrByID);
    // Filtered Array
    // [{ id: 15 }, { id: -1 }, { id: 3 }, { id: 12.2 }]
    
    console.log('Number of Invalid Entries = ', invalidEntries);
    // Number of Invalid Entries = 5
    ```

  ### 5.遍历数组

  - （1）forEach

    1. 可以改变数组自身，没有返回值；

    2. 中途不能用常规操作跳出循环，可以用抛出异常（try/catch）的方式，但不推荐这样做；

       ``` js
       var arr1 = [
          {name:'鸣人',age:16},
          {name:'佐助',age:17}
       ];
       var arr2 = [1,2,3];
       
       arr1.forEach(item => { 
         item.age = item.age + 1}
       );
       ```

       **当数组中元素是值类型，forEach绝对不会改变数组；当是引用类型，则可以改变数组**

       （2）map() 映射

    原数组被"映射"成对应新数组。

    - 新建一个数组，需要有承载对象，也意味着原始数组在调用它后不会发生变化;
    - 该数组中的每个元素都调用一个提供的函数后返回结果。

    创建新数组不代表不能用它改变原有数组，你用原有数组去承载就可以：

    ```js
    let arr = [1,2,3]; 
    arr = arr.map(item => { return item * 2 })
    ```

    - map()中每个元素都要执行相应的回调函数，所以必须要有return
    - 如果你想给数组做一定的过滤处理，那map()基本上行不通：

    ```js
    let newArr = [1,2,3,4,5].map(item => { if(item > 3) return item })
    // => [undefined, undefined, undefined, 4, 5]
    ```

    - 不能凑合用， 人生不能凑合，代码也是！

      （3)fliter() 过滤

      -  	创建一个新数组，新数组中的元素是筛选出来的符合条件的所有对象。

    ```js
    let newArr = [1,2,3,4,5].filter(item =>{
       if(item > 3) return item 
    })
    //  => [4,5]
    ```

    ​			(4)sort()排序

    - sort()用于对数组的元素进行排序。排序顺序可以是字母或数字，并按升序或降序。

    - ​	sort()与map()、filter()等不同，它直接改变原始数组（很重要！）；

    - 如果想按照其他标准进行排序，就需提供比较函数compareFunction(a,b)，数组会按照调用该函数的返回值排序，即a和b是两个将要比较的元素：

      - 如果compareFunction(a,b)小于0，则a排列到b之前；
      - 如果 compareFunction(a, b)等于0，a和b的相对位置不变（并不保证）;
      - 如果 compareFunction(a, b)大于0，b排列到a之前； 直接上例子：

      ```js
      let Users = [
        {name:'鸣人',age:16},
        {name:'卡卡西',age:28},
        {name:'自来也',age:50},
        {name:'佐助',age:17}
      ];
      Users.sort((a,b)=> {
         return a.age - b.age
      })
      
      // => 鸣人、佐助、卡卡西、自来也
      ```

      

      (5)检索存在

      - ​	some()

        ```js
        var result = [
           {name:'鸣人',age:16},
           {name:'佐助',age:17}
        ].some(item => {
        	return item.age > 16 
        });
        => true
        ```

      - every()

      ```js
      var result = [
         {name:'鸣人',age:16},
         {name:'佐助',age:17}
      ].every(item => {
      	return item.age > 16 
      });
      => false
      ```

      - some()和every()返回的都是`Boolean`值，仅此而此

#### 6.数组去重

- ​	new Set( ) 	经典方法

```js
let tempArr = new Set([1,2,3,3,4,4,5])
// => {1,2,3,4,5} 

//并且已有元素是添加不进去的：
tempArr.add(3) 
// => {1,2,3,4,5}

tempArr.add(6)
// => {1,2,3,4,5,6}
```

​                     **Set()没法去重元素是引用对象的数组。**

- #### _.uniqWith()     js库

  ```js
  import _ from 'lodash';
  <script>
  var objects = [{ 'x': 1, 'y': 2 }, { 'x': 2, 'y': 1 }, { 'x': 1, 'y': 2 }];
  _.uniqWith(objects, _.isEqual);
  </script>
  
  //=> [{ 'x': 1, 'y': 2 }, { 'x': 2, 'y': 1 }]
  
  
  //_.isEqual(value,other)用于执行深比较来确定两者的值是否相等。 _.uniqWith()做去重处理。
  ```



#### 7.查找元素

- findIndex（）

通过对象属性值直接获取对应索引： `**findIndex()**` 方法返回数组中满足提供的测试函数的第一个元素的值。否则返回 [`undefined`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/undefined)。

- find（）

`find()`顾名思义，就是用来在数组中找到我们所需要的元素



#### 8.回调地狱

```js
// 回调地狱
    ajax('get', './a.json', function (info) {
      if (info.addr != 'ok') {
        ajax('get', info.addr, function (info) {
          if (info.addr != 'ok') {
            ajax('get', info.addr, function (info) {
              if (info.addr == 'ok') {
                alert(info.msg);
              }
            })
          }
        })
      }
    });
```



#### 9.eval

它的功能是将对应的字符串解析成 JS 并执行，应该避免使用 JS，因为非常消耗性能（2 次，一次解析成 JS，一次执行）

#### 10.copyWithin

```js
//copyWithin() 方法浅复制数组的一部分到同一数组中的另一个位置，并返回它，不会改变原数组的长度。
```

- 他会影响原数组，返回的数据和原数据为同一引用

- ```js
  [1, 2, 3, 4, 5].copyWithin(-2)
  // [1, 2, 3, 1, 2]
  
  [1, 2, 3, 4, 5].copyWithin(0, 3)
  // [4, 5, 3, 4, 5]
  
  [1, 2, 3, 4, 5].copyWithin(0, 3, 4)
  // [4, 2, 3, 4, 5]
  
  [1, 2, 3, 4, 5].copyWithin(-2, -3, -1)
  // [1, 2, 3, 3, 4]
  ```

#### 11.call bind apply

```js
//第一个参数 都是this 的指向，后面的参数则都是传递的参数
// call 和 bind 传递参数都是： a,b,c...的形式
  //而apply后面传递的参数应该为数组形式，就算是单个数据也要写成数组的形式 ：['a']
//bind和call存在的区别在于，call和apply都会立即执行函数，而bind则不会。因此可以用bind写事件处理函数
```

```js
 // Array.prototype ---》数组的实例化对象的原型 ---》this 指向数组的实例化对象
    function fn1(a,b,c) {
      // arguments  foreach
      // es5中非常重要的写法
      Array.prototype.forEach.call(arguments,function(item) {
        console.log(item);
      })
    }
    fn1('哈哈哈', '嘿嘿嘿', '呵呵呵');
```

```js
const btn = document.querySelector('#btn');
    // call apply bind 改变this指向
    // call(this,值1,值2....) apply(this, [值1, 值2...]) bind(this, 值1, 值2....)
    // 从表面上看bind方法和call方法完全一致，但是
    // bind方法可以再调用之后不执行函数，而call和apply只要调用就立刻执行
    const obj = {
      a: 100,
    }
    function fn1(user) {
      console.log('你好', user);
      console.log(this.a); // window.a
    }
    // 让fn1的this指向obj 使用call方法，只要运行代码，程序就会立刻执行
    // fn1.call(obj, '张三');
    // fn1.apply(obj, ['张三']);
    // 我的需要: fn1作为btn的事件处理函数，点击按钮之后再执行
    // fn1需要改变this指向，同时需要向fn1中传值
    btn.onclick = fn1.bind(obj, '张三');
```

![image-20211223194857541](%E5%AD%A6%E4%B9%A0/mdimages/image-20211223194857541.png)

```js
function Person(name,age){
    //this == obj
    this.name = name;
    this.age = age;
}
ver person = new Person('deng',100)
var obj = {}
Person.call(obj,'cheng',300)
//test  ---> test.call()

//默认函数运行，相当于隐式运行.call( )
//把Person时传入obj，相当于this == obj this.name = name ,,this.age = age
```

```js
function Person(name,age){
    //this == obj
    this.name = name;
    this.age = age;
}
function Student(){
}
```







#### 12.reduce

`**reduce()**` 方法对数组中的每个元素执行一个由您提供的**reducer**函数(升序执行)，将其结果汇总为单个返回值。

```js
const array1 = [1, 2, 3, 4];
const reducer = (previousValue, currentValue) => previousValue + currentValue;

// 1 + 2 + 3 + 4
console.log(array1.reduce(reducer));
//reduce 里面为一个回调函数
```

```php
array.reduce(function(prev, current, currentIndex, arr), initialValue)
```

1. prev：函数传进来的初始值或上一次回调的返回值
2. current：数组中当前处理的元素值
3. currentIndex：当前元素索引
4. arr：当前元素所属的数组本身
5. initialValue：传给函数的初始值

#### 13.Map

**加工数据**

`**map()**` 方法创建一个新数组，其结果是该数组中的每个元素是调用一次提供的函数后的返回值。

```js
const array1 = [1, 4, 9, 16];

// pass a function to map
const map1 = array1.map(x => x * 2);

console.log(map1);
// expected output: Array [2, 8, 18, 32]
```

#### 14.splice

- 删除 修改 清空

- ```js
  //splice() 方法通过删除或替换现有元素或者原地添加新的元素来修改数组,并以数组形式返回被修改的内容。此方法会改变原数组。
  ```

- ```js
  const months = ['Jan', 'March', 'April', 'June'];
  months.splice(1, 0, 'Feb');
  // inserts at index 1
  console.log(months);
  // expected output: Array ["Jan", "Feb", "March", "April", "June"]
  
  months.splice(4, 1, 'May');
  // replaces 1 element at index 4
  console.log(months);
  
  //1.要修改的元素索引
  //2.要删除的元素个数
  //3.要添加的元素
  
  //清空 arr.splice(0,arr.length)
  ```

- 

#### 15.URI 和 URL

##### 一.统一资源标识符（URI）

<1>什么是URI

URI，统一资源标志符(Uniform Resource Identifier， URI)，表示的是web上每一种可用的资源，如 HTML文档、图像、视频片段、程序等都由一个URI进行标识的。

<2>URI的结构组成

URI通常由三部分组成：

①资源的命名机制；

②存放资源的主机名；

③资源自身的名称。

（注意：这只是一般URI资源的命名方式，只要是可以唯一标识资源的都被称为URI，上面三条合在一起是URI的充分不必要条件）

<3>URI举例

如：https://blog.csdn.net/qq_32595453/article/details/79516787

我们可以这样解释它：

①这是一个可以通过https协议访问的资源，

②位于主机 blog.csdn.net上，

③通过“/qq_32595453/article/details/79516787”可以对该资源进行唯一标识（注意，这个不一定是完整的路径）

注意：以上三点只不过是对实例的解释，以上三点并不是URI的必要条件，URI只是一种概念，怎样实现无所谓，只要它唯一标识一个资源就可以了。

##### 二、URL
URL是URI的一个子集。它是Uniform Resource Locator的缩写，译为“统一资源定位 符”。

通俗地说，URL是Internet上描述信息资源的字符串，主要用在各种WWW客户程序和服务器程序上。

采用URL可以用一种统一的格式来描述各种信息资源，包括文件、服务器的地址和目录等。URL是URI概念的一种实现方式。

URL的一般格式为(带方括号[]的为可选项)：

protocol :// hostname[:port] / path / [;parameters][?query]#fragment

URL的格式由三部分组成： 

①第一部分是协议(或称为服务方式)。

②第二部分是存有该资源的主机IP地址(有时也包括端口号)。

③第三部分是主机资源的具体地址，如目录和文件名等。

第一部分和第二部分用“://”符号隔开，

第二部分和第三部分用“/”符号隔开。

第一部分和第二部分是不可缺少的，第三部分有时可以省略。

##### 三、URI和URL之间的区别
从上面的例子来看，你可能觉得URI和URL可能是相同的概念，其实并不是，URI和URL都定义了资源是什么，但URL还定义了该如何访问资源。URL是一种具体的URI，它是URI的一个子集，它不仅唯一标识资源，而且还提供了定位该资源的信息。URI 是一种语义上的抽象概念，可以是绝对的，也可以是相对的，而URL则必须提供足够的信息来定位，是绝对的。

##### 1.decodeURI( )

`**decodeURI()**` 函数能解码由[`encodeURI`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/encodeURI) 创建或其它流程得到的统一资源标识符（URI）。

##### 2.encodeURI( )

`**encodeURI()**` 函数通过将特定字符的每个实例替换为一个、两个、三或四转义序列来对统一资源标识符 (URI) 进行编码 (该字符的 UTF-8 编码仅为四转义序列)由两个 "代理" 字符组成)。

#### 

### 对象方法

#### 1.Object.defineProperty()

`**Object.defineProperty()**` 方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性，并返回此对象。

```js
const object1 = {};

Object.defineProperty(object1, 'property1', {
  value: 42,
  writable: true
});

object1.property1 = 77;
// throws an error in strict mode

console.log(object1.property1);
// expected output: 42
```

|            | `configurable` | `enumerable` | `value` | `writable` | `get`  | `set`  |
| ---------- | -------------- | ------------ | ------- | ---------- | ------ | ------ |
| 数据描述符 | 可以           | 可以         | 可以    | 可以       | 不可以 | 不可以 |
| 存取描述符 | 可以           | 可以         | 不可以  | 不可以     | 可以   | 可以   |

- `configurable`

  当且仅当该属性的 `configurable` 键值为 `true` 时，该属性的描述符才能够被改变，同时该属性也能从对应的对象上被删除。 **默认为** **`false`**。

- `enumerable`

  当且仅当该属性的 `enumerable` 键值为 `true` 时，该属性才会出现在对象的枚举属性中。 **默认为 `false`**。

数据描述符还具有以下可选键值：

- `value`

  该属性对应的值。可以是任何有效的 JavaScript 值（数值，对象，函数等）。 **默认为 [`undefined`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/undefined)**。

- `writable`

  当且仅当该属性的 `writable` 键值为 `true` 时，属性的值，也就是上面的 `value`，才能被[`赋值运算符` (en-US)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators#assignment_operators)改变。 **默认为 `false`。**

存取描述符还具有以下可选键值：

- `get`

  属性的 getter 函数，如果没有 getter，则为 `undefined`。当访问该属性时，会调用此函数。执行时不传入任何参数，但是会传入 `this` 对象（由于继承关系，这里的`this`并不一定是定义该属性的对象）。该函数的返回值会被用作属性的值。 **默认为 [`undefined`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/undefined)**。

- `set`

  属性的 setter 函数，如果没有 setter，则为 `undefined`。当属性值被修改时，会调用此函数。该方法接受一个参数（也就是被赋予的新值），会传入赋值时的 `this` 对象。 **默认为 [`undefined`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/undefined)**。



### Error

[Error 类型](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Error#error_types)

除了通用的Error构造函数外，JavaScript还有6个其他类型的错误构造函数。更多客户端异常,详见 [Exception Handling Statements](https://developer.mozilla.org/en-US/JavaScript/Guide/Statements#exception_handling_statements)。

- **[`EvalError`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/EvalError)**

  创建一个error实例，表示错误的原因：与 [`eval()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/eval) 有关。

- **[`InternalError`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/InternalError)** 

  创建一个代表Javascript引擎内部错误的异常抛出的实例。 如: "递归太多".

- **[`RangeError`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RangeError)**

  创建一个error实例，表示错误的原因：数值变量或参数超出其有效范围。

- **[`ReferenceError`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/ReferenceError)**

  创建一个error实例，表示错误的原因：无效引用。

- **[`SyntaxError`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/SyntaxError)**

  创建一个error实例，表示错误的原因：[`eval()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/eval)在解析代码的过程中发生的语法错误。

- **[`TypeError`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/TypeError)**

  创建一个error实例，表示错误的原因：变量或参数不属于有效类型。

- **[`URIError`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/URIError)**

  创建一个error实例，表示错误的原因：给 [`encodeURI()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/encodeURI)或 [`decodeURI()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/decodeURI)传递的参数无效。



### Date 

```js
const data = new Date();
//得到data为 ...
//data = xxxxxxx

//将data放入new Date(  ..) 可以得到曾经保存的时间
const data1 = new Date(data)

//可以根据时间戳的差值计算出时间间隔
```





### 异步编程

背景：js诞生之际是一门单线程语言，功能较少。后随着计算机的发展，ajax等的兴起。当计算机执行一个需要较长时间的进程时候，为了提高计算机效率，不让其空闲，就有了异步。

#### 什么是异步js

- ​	在js中，任务分为两种，同步任务和异步任务。同步任务在主线程中，一开始就执行，而异步任务在需要时或者特点条件下才会执行。	
  - 异步js主要有：
    1. DOM操作
    2. 定时器
    3. AJAX请求
  - 在程序执行过程中，所以的同步任务都会在主线程上执行，形成一个执行栈

  - 主线程之外还存在一个任务队列，只要异步任务有了结果，就会在任务队列中放置一个事件，当同步任务执行完，或者说执行栈一被清空。主线程就开始执行任务队列里面的异步事件，即当前异步事件的回调函数。再任务队列中，事件遵循先进后出规则。

#### 异步事件注意点

- 异步事件的发生先后顺序是不确定的，这取决于当前事件在子进程中完成的先后顺序，先完成的先进入任务队列，先执行。
- 定时器由于任务队列的先进后出，可能会延时发生。当定时器时间到达时，才会在任务队列中添加事件（在这之前，挂载在子进程上），而此时定时器需要等到先排队的事件执行完才能执行。（前提是执行栈也已经被清空）
- Event loop 主线程在任务队列中执行事件是循环不断的，整个这个的运行机制叫做Event loop。
- html5 规定定时器最短时间间隔为4ms，足的会变为4ms。老版本的规定为10ms，DOM时间触发后不会立即执行，会延误16ms。



### Promise

- Promise是异步编程的一种解决方案。比传统的解决方案 ——回调函数和事件——更合理强大。

- 可以说是一个容器，里面保存着某个未来才会结束的事件，（通常是一个异步操作）的结果。可以获取异步操作的消息

- 特点

  - 对象的状态不受外界影响，~代表一个异步操作，有三种状态

    - pending
    - fulfilled
    - rejected

    只有异步的操作结果，可以决定当前是哪一种状态，任何其他的操作都无法改变这个状态。

  - 一旦状态改变就不会再变，只有两种可能

    - pending -> fulfilled
    - pending -> rejected

    只要是事件发生了，就不会再改变，叫做定型resolved。

    而事件的特点是，如果你错过了他，再去监听，是得不到结果的。

```js
//实例化Promise对象
let p = new Promise((resolve,reject) => {
                    if() resolve(info);
                    else reject(err);
                    })
p.then((info) => {} , (err) ={} )

.catch((err)={
    
})
```

- 如果后面嵌套then。里面返回promise对象
- **.then 可以链式不断调用**

```js
p.then((info) => {
   return new Promise((resolve,reject) => {
                    if() resolve(info);
                    else reject(err);
                    })
} , (err) ={} )

.then((info) => {
    return new Promise((resolve,reject) => {
                    if() resolve(info);
                    else reject(err);
                    })
}, (err) ={})

.then ...

//.catch 每一个状态为reject都会执行catch()
```

##### async await

- 用await替代then，**await**和**async**需要配合使用

```js
async function move(){
            const p1 = await ajax('get','./a.json')
            // console.log(p1)
            const data1 = p1.src
            // console.log(data1)
            const p2 = await ajax('get',data1);

            const data2 = p2.src;

            const p3 = await ajax('get',data2)

            console.log(p3.msg)
        }
        move()
```

async 和 await 执行顺序

```js
async function async1() {
    console.log('async1 start') //执行3
    await async2()              //执行函数 4
    console.log('async1 end')   //挂载等待1
}

async function async2() {
    console.log('async2') //执行5
}
console.log('script start')   //顺序执行1

setTimeout(function () {
    console.log('setTimeout')  //等待微任务1
}, 0)
async1();   //执行函数 2

new Promise(function (resolve) {
    console.log('promise1') //立即执行 6 
    resolve();
}).then(function () {       //挂载等待2
    console.log('promise2')
})
console.log('script end')   //立即执行7

结果
script start
async1 start
async2
promise1
script end
async1 end
promise2
setTimeout

知识点
显然，这考察的是js中的事件循环和回调队列。注意以下几点：
     *   Promise优先于setTimeout宏任务。所以，setTimeout回调会在最后执行。
     *   Promise一旦被定义，就会立即执行。
     *   Promise的reject和resolve是异步执行的回调。所以，resolve()会被放到回调队列中，在主函数执行完和setTimeout前调用。
     *   await执行完后，会让出线程。async标记的函数会返回一个Promise对象
await后面的语句会在await执行完之后再执行。定时器为宏任务，会在微任务执行完之后再执行。
```

```js
  async function queryData() {
      # 2.1  添加await之后 当前的await 返回结果之后才会执行后面的代码   
      
      var info = await axios.get('async1');
      #2.2  让异步代码看起来、表现起来更像同步代码
      var ret = await axios.get('async2?info=' + info.data);
      return ret.data;
    }
————————————————
```

##### ajax 配合async



##### Promise.prototype.finally()

用于指定不管 Promise 对象最后状态如何，都会执行的操作。

不管`promise`最后的状态，在执行完`then`或`catch`指定的回调函数以后，都会执行`finally`方法指定的回调函数。

```js
promise
.then(result => {···})
.catch(error => {···})
.finally(() => {···});
```

##### Promise.all()

`Promise.all()`方法用于将多个 Promise 实例，包装成一个新的 Promise 实例

```javascript
const p = Promise.all([p1, p2, p3]);
```

上面代码中，`Promise.all()`方法接受一个数组作为参数，`p1`、`p2`、`p3`都是 Promise 实例，如果不是，就会先调用下面讲到的`Promise.resolve`方法，将参数转为 Promise 实例，再进一步处理。另外，`Promise.all()`方法的参数可以不是数组，但必须具有 Iterator 接口，且返回的每个成员都是 Promise 实例。

（1）只有`p1`、`p2`、`p3`的状态都变成`fulfilled`，`p`的状态才会变成`fulfilled`，此时`p1`、`p2`、`p3`的返回值组成一个数组，传递给`p`的回调函数。

（2）只要`p1`、`p2`、`p3`之中有一个被`rejected`，`p`的状态就变成`rejected`，**此时第一个被`reject`的实例的返回值**，会传递给`p`的回调函数。

##### Promise.race()

`Promise.race()`方法同样是将多个 Promise 实例，包装成一个新的 Promise 实例。

```javascript
const p = Promise.race([p1, p2, p3]);
```

只要`p1`、`p2`、`p3`之中有一个实例率先改变状态，`p`的状态就跟着改变。那**个率先改变的 Promise 实例的返回值，**就传递给`p`的回调函数。

`Promise.race()`方法的参数与`Promise.all()`方法一样，如果不是 Promise 实例，就会先调用下面讲到的`Promise.resolve()`方法，将参数转为 Promise 实例，再进一步处理。



**`Promise.any()`跟`Promise.race()`方法很像，只有一点不同，就是`Promise.any()`不会因为某个 Promise 变成`rejected`状态而结束，必须等到所有参数 Promise 变成`rejected`状态才会结束**



#### 宏任务和微任务

在异步任务中，将任务分为两种，分别是：

-  				宏任务

  - 定时器
  - requestAnimationFrame

- ​                  微任务

  - promise

  

- 在任务队列之外，将宏任务和微任务分别挂载在宏任务队列和微任务。当同步任务执行完之后，会进行Event loop 执行任务队列。

  - 执行顺序是：

    - 先执行同步任务，再执行异步任务
    - 先执行微任务，再执行宏任务

    ```js
    console.log(1); //同步执行1
    
    new Promise(resolve=> {
      setTimeout(()=> {
        console.log(2);
      }, 4); //宏任务1  定时器1
    
      resolve();
    
      console.log(3);	//同步执行2
    })
    
    .then(_ => {
      console.log(4);  //微任务1
    })
    
    setTimeout(()=> { //宏任务2 定时器2
      new Promise(resolve => {
        console.log(5);
    
        resolve();
      })
      .then(_ => {
        console.log(6);
      })
    },4)
    
    //当执行promise内的代码时，语句遵循同步执行的规则
    //此处有一个简写 _ => 表示舍弃参数，不进行传值操作
    
    ```

    - **requestAnimationFrame	请求动画帧**

- 60HZ 当你什么都不做的时候，电脑也会以每秒60次的刷新速率更新，

- 动画原理

  - 而动画的本质就是人眼看到图像被属性变化而引起的视觉效果，这个效果要以平滑连贯的方式进行过渡

- setTimeout ( 存在丢帧现象 )

  - setTimeout的执行时间并不是确定的。在Javascript中， setTimeout 任务被放进了异步队列中，只有当主线程上的任务执行完以后，才会去检查该队列里的任务是否需要开始执行，因此 **setTimeout 的实际执行时间一般要比其设定的时间晚一些。**
  - 刷新频率受**屏幕分辨率**和**屏幕尺寸**的影响，因此不同设备的屏幕刷新频率可能会不同，而 setTimeout只能设置一个固定的时间间隔，这个时间不一定和屏幕的刷新时间相同。

- #### requestAnimationFrame

  - requestAnimationFrame最大的优势是**由系统来决定回调函数的执行时机**

    - 具体一点讲，如果屏幕刷新率是60Hz,那么回调函数就每16.7ms被执行一次，如果刷新率是75Hz，那么这个时间间隔就变成了1000/75=13.3ms，换句话说就是，requestAnimationFrame的步伐跟着系统的刷新步伐走。**它能保证回调函数在屏幕每一次的刷新间隔中只被执行一次**，这样就不会引起丢帧现象，也不会导致动画出现卡顿的问题。

    - ```js
              let box = document.querySelector('.box');
              let len = 0;
              function fn() {
                  len += 5;
                  box.style.left = len + 'px';
                  if (len < 1280) {
                      window.requestAnimationFrame(fn);
                  }
              }
              window.requestAnimationFrame(fn);
      ```

      （1）requestAnimationFrame 会把每一帧中的所有 DOM 操作集中起来，在一次重绘或回 流中就完成，并且重绘或回流的时间间隔紧紧跟随浏览器的刷新频率。 

      （2）在隐藏或不可见的元素中，requestAnimationFrame 将不会进行重绘或回流，这当然 就意味着更少的 CPU、GPU 和内存使用量 

      （3）requestAnimationFrame 是由浏览器专门为动画提供的 API，在运行时浏览器会自动 优化方法的调用，**并且如果页面不是激活状态下的话，动画会自动暂停，有效节省了 CPU 开销**

### ES6

#### 箭头函数

​	本质是一个语法糖

```js
function() {}

() => {}

//1.如果箭头函数只有一个参数，则括号可以省略
item => {}   //形如
//2如果函数体只有一条语句，则可以省略{}括号
() => return 'hello'

(a, b) => a + b

(a) => {
  a = a + 1
  return a
}



//如果要返回一个对象，就要注意，如果是单表达式，这么写的话会报错：
// SyntaxError:
x => { foo: x }
//因为和函数体的{ ... }有语法冲突，所以要改为：

// ok:
x => ({ foo: x })





```

(1). 箭头函数this为父作用域的this，不是调用时的this
箭头函数的this永远指向其父作用域，任何方法都改变不了，包括call，apply，bind。
普通函数的this指向调用它的那个对象。

(2). 箭头函数不能作为构造函数，不能使用new

(3). 箭头函数没有arguments，caller，callee

箭头函数本身没有arguments，如果箭头函数在一个function内部，它会将外部函数的arguments拿过来使用。

箭头函数中要想接收不定参数，应该使用rest参数...解决。

 (4). 箭头函数通过call和apply调用，不会改变this指向，只会传入参数

(5). 箭头函数没有原型属性

(6). 箭头函数不能作为Generator函数，不能使用yield关键字

(7). 箭头函数返回对象时，要加一个小括号

(8). 箭头函数在ES6 class中声明的方法为实例方法，不是原型方法

(9). 多重箭头函数就是一个高阶函数，相当于内嵌函数



```js
(10). 箭头函数常见错误
let a = {
  foo: 1,
  bar: () => console.log(this.foo)
}

a.bar()  //undefined
bar函数中的this指向父作用域，而a对象没有作用域，因此this不是a，打印结果为undefined

function A() {
  this.foo = 1
}

A.prototype.bar = () => console.log(this.foo)
let a = new A()
a.bar()  //undefined
原型上使用箭头函数，this指向是其父作用域，并不是对象a，因此得不到预期结果

3.一些this的指向实例
var obj = {
  birth: 1990,
  getAge: function(){
    console.log(new Date().getFullYear() - this.birth); //this指向obj对象
  },
}
obj.getAge(); // 29

var obj = {
    birth: 1990,
    getAge: function () {
        var b = this.birth; // 1990
        var fn = function () {
            return new Date().getFullYear() - this.birth; // this指向window或undefined
        };
        return fn();
    }
};
obj.getAge();// NaN

var obj = {
    birth: 1990,
    getAge: function () {
        var b = this.birth; // 1990
        var fn = () => new Date().getFullYear() - this.birth; // this指向obj对象
        return fn();
    }
};
obj.getAge(); // 29

var obj = {
       getAge: () => {
            console.log(this); // window
        }
    };

obj.getAge(); //window
作为对象的属性时，this的指向则不再是对象本身了，箭头函数捕获的是obj{}这个对象的环境，然后这个环境的this指向的是window
```

