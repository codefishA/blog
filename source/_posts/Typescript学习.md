---
title: typeScript入门

author: codefish
date: 2022-5-20 10:34:07
categories: js
tags: [ts,js]
top_img: /img/justin.jpg
cover: /img/justin.jpg
---
## ---

typeScript作为js的超集，主要是加了类型限制，对于面向对象的使用多于js,学习ts是后面学习node框架koa,express,nest等的基础

[toc]

### 1.类型

```js
let a:number
// a为number，以后的使用a的值只能为数字

```

**ts-可以编译为任何js版本 tsc**

```js
let c:boolean = false
```

如果变量声明时，直接赋值（同时进行），它可以自动判断

```js
function sum(a,b) {
	return a+b
}
//sum(123,456)
//sum('123',456)

//改善
function sum(a:number,b:number) {
	return a+b
}
sum(123,'456') //提示错误

//函数返回值的类型
function sum(a:number,b:number):number {
	return a+b
}
```

类型的都为小写 number,boolean,string

- １．number 任意数字
- ２．string 任意字符串
- ３．boolean 任意布尔值
- ４.字面量类型说明

​	

```js
let a:10
//赋值一次不能修改

//可以使用｜来连接多个类型
let b:boolean|string

```

- ５．any

  ```js
  //表示可以为任意类型－
  let　ｄ　//－－变量只声明不赋值，自动判断为ａｎｙ
  //设置ａｎｙ后，相当于关闭类型检测
  let d:any
  
  let s:string
  s = d  -- s:string
  s = d  -- s将变为any
  
  ```

  

- ６．unknown -- 效果和any一致

  ```js
  //any 类型的变量可以赋值给任意类型的变量
  let s:string
  let d:any
  s = d -- s变为any
  
  let n:unkonwn
  n = 'hello'
  s = n //将一个unkonwn赋给string--报错
  
  //解决
  if(typeof s==="string"){
  	s = n
  }
  ```

  **为一个安全的ａｎｙ，不能赋值给其他变量**

- 7.类型断言

  ```js
  //告诉解析器变量的实际类型
  s = e as string
  s = <string>e;
  ```

  

- 8.void-函数返回值

  ```js
  function fn(num):number{}
  function fn(nunm):number|boolean{} 
  function fn(num):void {}   //没有返回值
  ```

  

- 9.never--永远不会返回结果

  ```js
  function fn():never{} 
  //甚至不需要return
  function fn():void{
      return
  }
  ```

  

- 10.object - js一切皆对象

  ```js
  let a:object;
  a = {}
  
  //限制对象里面的属性
  let b:{name:string};
  //对象b必须有一个属性name-类型为string
  b = {name:''}     //结构和上面一致
  
  //加上一个？表示为可选属性
  let b:{name:string,age?:number}
  b = {name:'',age:18}
  b = {name:''}
         
  //一个必须属性，其他根据需要来
  let c = {name:string,[propName:string]:any};
  //propName可以为任意字段
  let c = {name:string,[name:string]:any};
         
  c = {name:'',age:''}
  c = {name:'',age:''，email:''}
         
  //限制函数类型
   let d: (a:number, b:number)=>number
   //两参数一返回值
   d = function (n1:string,n2:string):number{}
  ```

  

- 11.array

  限定数组里面值的类型

  ```js
  let e:string[];
  e = ['a','s']
  
  let f:number[]
  let g:array<number>
  ```

  

- 12.tuple 元组

  固定长度的数组

  ```js
  let h:[string,string]
  h = ['hello',123]
  
  let h:[number,number]
  h = [1,2]
  ```

  

- 13.enum 枚举

  ```js
  let i :{name:string,gender:string}
  i = {
      name:'孙悟空',
      gender:'男' //固定的male
  }
  
  //let i :{name:string,gender:1|0}
  =>{
      gender:1
  }
  enum Gender = {
      Male = 0,
      Female = 1
  }
  let i:{naem:string,gender:Gender}
  i = {
  	name:'孙悟空'，
      gender:Gender.Male
  }
  ```

  

- 数组可以为-any任意类型

  ```js
  let arr:ant = [1,2,true,'a']
  ```

### 函数

```js
//可选参数
function myone(age:string='zhangsan',x?:string,):string{
    return age+x;
}
//可选参数需要写在必选参数之后

//默认值
console.log(myone());
console.log(myone('wangwu','heihei'))
```



**shift + space 切换全半角**

![image-20220506112117432](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220506112117432.png)



### 1.ts编译事项

1. ts是不不可以直接使用的，需要编译为js才能使用

2. ts文件里面的形参，如果使用了某个类型进行修饰，那么最终在编译后的js文件里面，是没有这个类型的。

3. ts文件中的变量使用的let修饰，编译的js文件中的修饰符就变成了var

4. vs-code 自动编译

5. **接口的定义与使用**

   ```js
   //接口-一种能力一种约束
   (function () {
     interface Person {
       name: string;
       age: number;
     }
     console.log("这里是2.ts");
     function a(person: Person): void {
       console.log(person.name + " " + person.age, "-n-a-m-e");
     }
     const person = {
       name: "wangwu",
       age: 23,
     };
     //将接口接口作为函数参数
     a(person);
   })();
   ```

   

6. ts里面使用一个类

   ```js
   (function () {
     console.log("class-ts");
     interface Person {
       firstName: string;
       lastName: string;
     }
     //定义一个类
     class PersonOne {
       //定义公共的字段
       public firstname: string;
       lastname: string;
       fullname: string;
       constructor(firstname: string, lastname: string) {
         this.firstname = firstname;
         this.lastname = lastname;
         this.fullname = firstname + lastname;
       }
     }
   
     //实例化对象
     const person = new PersonOne("诸葛", "孔明");
   })();
   
   ```

### 2.webpack 打包ts

1. 初始化npm init -y

2. 初始化tsc

3. 命令

   ```js
   npm i webpack-cli
   npm i webpack-dev-server
   npm i html-webpack-plugin clean-webpack-plugin
   npm i ts-loader
   npm i cross-env
   ```

   

**报错1 **

webpack报错 TypeError: Cannot read property ‘tap‘ of undefined

原因：全局和局部版本不一样



---

webpack 配置

```js
const HtmlWebpackPlugin = require('html-webpack-plugin');
const CleanWebpackPlugin = require('clean-webpack-plugin')
const path = require('path');

module.exports = {
  entry: './1.ts', // 注意这里原来是 './src/index.js'，需要改成 ts 结尾！
  module: {
    rules: [
      {
        test: /\.tsx?$/,
        use: 'ts-loader',
        exclude: /node_modules/,
      },
    ],
  },
  resolve: {
    extensions: ['.tsx', '.ts', '.js'],
  },
  output: {
    filename: '[contenthash:8].js',
    path: path.resolve(__dirname, 'dist'),
  },
  plugins:[
    new HtmlWebpackPlugin({
      template:'./a.html'
    }),
    // new CleanWebpackPlugin()
  ]
};

```



clean-webpack-plugin写法

```js

const { CleanWebpackPlugin } = require("clean-webpack-plugin");
plugins: [
    new CleanWebpackPlugin()
]
 

//错误写法
const CleanWebpackPlugin = require("clean-webpack-plugin");
plugins: [
    new CleanWebpackPlugin(['dist'])
]


//
const CleanWebpackPlugin = require("clean-webpack-plugin");
plugins: [
    new CleanWebpackPlugin(['dist'], {
        root: path.resolve(__dirname, '../'),   //根目录
    })

```



**" --config build/webpack.config.js"配置**

配置配置文件的路径

```txt
webpack默认使用同目录下的webpack.config.js文件,也可以自定义指定文件位置
在package.json文件中配置:

```



### 3.ts接口

/*是对对象的状态（属性）和行为（方法）的抽象（描述）*/

/*是一种类型，是一种规范，是一种规则，是一个能力，是一种约束*/

//*定义一个接口，该接口作为person对象的限制使用，限制或者约束该对象中的属性数据*//

```js
interface Dog {
      readonly id:number,
      name:string,
      age:number,
      sex?:string
   }
      
 const kite:Dog = {
      id:1,
      name:'hurry',
      age:3,
      sex:'公'
   }
    
    //readonly:只读属性
    //  ?  非必要/可选
    
```



### 4.函数类型

*为了使用接口表示函数类型，我们需要给接口定义一个调用签名*

*它就是一个只有参数列表和返回值类型的函数定义。参数列表里的每个参数都需要名字和类型*

```js
//函数类型-通过接口的方式作为函数的类型来使用
//定义一个接口，用来作为某个函数使用

interface fnsign{
      //签名
      //定义一个调用签名
      (source:string,subString:string):boolean
   }
   const fn:fnsign = function(source:string,subString:string):boolean{
      return source.search(subString)> -1
   }
```



### 5.类-class



*定义一个类，这个类的类型就是上面定义的接口，实际上也可以理解为，ifly接口约束了当前的这个person类*

```ts
console.log("04-class-ts");
    interface IFLY{
        fly()
    }

    class Person implements IFLY{
        fly() {
            console.log('super cam flu ,OMG!')
        }
    }

    const one = new Person()
    one.fly()
```







**编译打包问题**

```txt
新增文件在服务器不会在页面实现
需要重新打包--npm run dev
```

因为使用`webpack-dev-server`是webpack5以前的方式了，如果使用，就会在`npm run start`运行时发生报错，错误信息为： `Error: Cannot find module 'webpack-cli/bin/config-yargs'



#### 打包ts原理

```js
配置一个tsconfig.js
当ts文件改变时，就会自动编译ts到一个指定目录
在index.js中引入ts打包后的路径

当ts文件修改时，会自动对其编译，造成打包后的js文件改变，js文件一旦改变，webpack就会立即打包

```





ps:**类可以通过当前接口的方式，定义当前这个类的类型**

*类可以实现一个接口，类可以实现多个接口，要注意，接口中的内容都要真正实现*

那么可以定义一个类，继承其他需要的接口，然后再继承这个接口。

**接口和接口之间叫做继承**

**类和接口之间叫实现**





//类：可以理解为模板。通过模板可以实例化对象

```js
class Fish{
        name:string
        age:number
        size:string
        type:string
        constructor(name:string='汗汗',age:number,size:string,type:string){
            this.name = name
            this.age = age
            this.size = size
            this.type = type
        }
        say(){
            console.log(this.name+' '+this.age+' '+ this.size+' '+ this.type+' ')
        }
    }
    const big = new Fish('',2,'big','鲫鱼')
    console.log(big.say())
```





### 6.类的继承

*继承：类与类之间的关系*

*继承后类与类的叫法*

A类继承了B这个类，那么此时A类叫子类，B叫基类

子类--- > 派生类

基类 ---> 超类（父类



### 7.存取器

```js
(() => {
  //存取器-有效的控制对对象中成员的访问
  //外部可以传入姓氏和名字，同时使用set,get控制姓名的属性，外部也可以进行修改
  class Person {
    private _name: string;
    _lastName: string;
    constructor(name: string, lastName: string) {
      this._name = name;
      this._lastName = lastName;
    }
    public get name() {
      return this._name;
    }
    public set name(val){
        this._name = val
     }
  }
  const peter = new Person("东方", "不败");
  console.log(peter, "111");
  console.log(peter.name, "peter-name");
  peter.name = "西方";
  console.log(peter.name, "peter-name");
})();

```



### 8.静态成员

```js
//通过static修饰的属性或者方法，那么就是静态的属性及静态的方法，也称作未静态成员
//静态成员在使用的时候是使用类目的这种语法来调用的

//static namea 爆红
//类中默认有一个内置的namea属性
(()=>{
    class Person7 {
        static namea:string;
        constructor(namea:string){
            //当namea被static修饰之后。namea是静态属性，不能通过实例对象直接调用静态属性来使用
            //通过类名.静态属性的方式来访问该成员变量
            //通过类目.静态属性的方式来修改成员变量
            //通过类名.静态方法的方式调用内部的属性和方法
            //构造函数不能使用static-- 构造函数不能使用static来修饰
            //优点：可以不实例化对象来调用类的属性和方法


            // this.namea = namea;
        }
        sayhi(){
            // console.log(this.namea,'-t-h-i-s-namea---:')
        }
    }
    const sweet = new Person7('霄汉')
    console.log(Person7.namea,'n-amea')   //underfinded
    sweet.sayhi()
})()
```



### 9.抽象类

```js
//抽象类和抽象方法
//抽象类包含抽象方法和实例方法，抽象类不可以被实例化
//作用化-为了让子类进行实例化以及实现内部的抽象方法
//抽象方法不能具体实现
//抽象类中的属性和方法都是为子类服务
(() => {
  abstract class Animal {
      abstract name:string;
    // abstract eat(): void {
    //     console.log('all-animals-eat')
    // }
    abstract eat()
    run(){
        console.log('非抽象方法，run')
    }
    sayhi(){
        console.log('hi-evnery')
    }
  }
//   const ani:Animal = new Animal()  //
//   抽象类并不能被直接实例化
class dog extends Animal{
    //重新实现抽象类中的方法
    //此时这个方法是当前dog的实例方法
    name:string = "pet"
    eat() {
        console.log(this.name+'-dog-eat')
    }
}
const erdog = new dog();
erdog.eat()
})();

```



### 10.函数

```js
(() => {
  //函数
  //封装一些重复使用的代码，在需要时候直接调用既可
  //函数声明-表达式-返回值
  //ts中的书写可以定义参数类型
  //ts支持函数重载

  function add(x: number, y: number): number {
    return x + y;
  }
  // function add(x:number):number{}
  console.log(add(3, 4), "add-result");
  //函数的完整写法
  const addone: (x: number, y: number) => number = function (
    x: number,
    y: number
  ): number {
    return x + y;
  };
})();

```





### 11.可选参数和默认参数

1. webpack 入口文件超过大小限制

```js
performance: {
    hints: "warning", // 枚举
    maxAssetSize: 30000000, // 整数类型（以字节为单位）
    maxEntrypointSize: 50000000, // 整数类型（以字节为单位）
    assetFilter: function (assetFilename) {
      // 提供资源文件名的断言函数
      return assetFilename.endsWith(".css") || assetFilename.endsWith(".js");
    },
  },
```



### 12.剩余参数

```js
(()=>{
    //函数剩余参数
    //剩余参数是放在函数声明的时候所有的参数的最后
    function showMsg(str:string,...args:string[]){
        console.log(str)  //a
        console.log(args) //剩余参数
    }   
    showMsg('a','b','c')
})()
```



### 13.函数重载

```js
(()=>{
    //ts的函数重载不同于其他语言
    //函数重载声明
    function add(x:number,y:number):number
    function add(x:string,y:string):string
    //未被声明的则会报错

    function add(x: number|string, y: number|string): number|string {
        if(typeof(x)=='number'&&typeof(y)=='number')
        {return x + y;}
        else if(typeof(x)=='string'&&typeof(y)=='string'){
            return +x+y;
        }
      }
      console.log(add(3, 4), "add-result");
      //7
      console.log(add('诸葛', '亮'), "add-result");
      //34
      console.log(add('3', 4), "add-result");
      //underfinded
})()
```



### 14.多个泛型参数的函数

```js
(() => {
  function getMsg<k, v>(value1: k, value2: v): [ k, v ] {
    return [value1, value2];
  }
  console.log(getMsg('a',1))
  console.log(getMsg(1,true))
})();

```



### 15.泛型接口



# 

















































