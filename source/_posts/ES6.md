---
title: ES6概述

author: codefish
date: 2022-3-7 10:16:41
categories: js
tags: [箭头函数,解构赋值,promise]
top_img: /img/5.jpg
cover: /img/5.jpg
---
### ES6

ES6 内容补充，当看到一个没见过或者不熟悉的语法时，无法拒绝对于它的学习

#### 1.箭头函数

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

**ps:注意**

​	若箭头函数只有一个return语句，可以省略return 和 {};如果想加上{}的话，必须也加上return语句

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



#### 2. let 和 const 命令

let 声明的变量只在**块级作用域**有效。

- 只能声明一次
- 无变量声明提升
- 块级作用域

会形成暂时性死区  --->  **声明之前无法使用，声明之后不能重新定义**



const 声明的变量 为一个常量变量

- 声明之后只读不可修改
  - 只能声明一次
  - 无变量声明提升
  - 块级作用域
    - 相当于在栈区上锁，当其为引用数据时候，只要不改地址就不会报错（即赋值）



- **在ES5 中未经声明的变量会挂载到window ，归 window 所有。而let 和 const 则不会自动挂载到 window 上**



#### 3.重绘和重排

重绘（repaint 或 redraw）：**当盒子的位置、大小以及其他属性，例如颜色、字体大小**等 都确定下来之后，浏览器便把这些原色都按照各自的特性绘制一遍，将内容呈现在页面 上。重绘是指一个元素外观的改变所触发的浏览器行为，浏览器会根**据元素的新属性重 新绘制，使元素呈现新的外观。** 触发重绘的条件：改变元素外观属性。如：color，background-color 等。 注意：table 及其内部元素可能需要多次计算才能确定好其在渲染树中节点的属性值，**比 同等元素要多花两倍时间，这就是我们尽量避免使用 table 布局页面的原因之一。** 

重排（重构/回流/reflow）：当渲染树中的一部分(或全部)因为元素的规模尺寸，布局， 隐藏等改变而需要重新构建, 这就称为回流(reflow)。每个页面至少需要一次回流，就是 在页面第一次加载的时候。 **重绘和重排的关系：在回流的时候，浏览器会使渲染树中受到影响的部分失效，并重新 构造这部分渲染树，完成回流后，浏览器会重新绘制受影响的部分到屏幕中，该过程称 为重绘。所以，重排必定会引发重绘，但重绘不一定会引发重排。**



#### 4.rest 参数

概述：

   Rest就是为解决传入的参数数量不一定， rest parameter(Rest 参数) 本身就是数组，数组的相关的方法都可以用。

```javascript
function f(a, b, ...theArgs) {
  // ...
}
```



描述

- 区分rest参数和 参数对象
  rest参数不会为每个变量给一个单独的名称，参数对象包含所有参数传递给函数
  参数对象不是真正的数组，rest参数是真实的数组实例。例如数组sort、map、forEach、pop的方法都可以直接使用
  参数对象有他自己额外的特性（例如callee 属性）

- ```js
  
  //以前函数
  function f(a, b) {
    var args = Array.prototype.slice.call(arguments, f.length);
   
    // …
  }
  // 等效于现在
  function f(a, b, ...args) {}
  ```

- ```js
  
  function fun1(...theArgs) {
    console.log(theArgs.length);
  }
   
  fun1();  // 0
  fun1(5); // 1
  fun1(5, 6, 7); // 3
  
  ```

- ````js
  排序
  function sortRestArgs(...theArgs) {
    var sortedArgs = theArgs.sort();
    return sortedArgs;
  }
  //好像一位和两位混合不能进行排序，这需要看一下为甚？主要第一个为主
  console.log(sortRestArgs(5, 3, 7, 1)); // shows 1, 3, 5, 7
  //参数对象arguments不能排序
  ````

- 箭头函数通过rest查看参数



#### 5.严格模式

严格模式主要有以下限制。

- 变量必须声明后再使用
- 函数的参数不能有同名属性，否则报错
- 不能使用with语句
- 不能对只读属性赋值，否则报错
- 不能使用前缀0表示八进制数，否则报错
- 不能删除不可删除的属性，否则报错
- 不能删除变量delete prop，会报错，只能删除属性delete global[prop]
- eval不会在它的外层作用域引入变量
- eval和arguments不能被重新赋值
- arguments不会自动反映函数参数的变化
- 不能使用arguments.callee
- 不能使用arguments.caller
- 禁止this指向全局对象
- 不能使用fn.caller和fn.arguments获取函数调用的堆栈
- 增加了保留字（比如protected、static和interface）

- 其中，尤其需要注意this的限制。ES6 模块之中，顶层的this指向undefined，即不应该在顶层代码使用this。



```txt
 1. 块级函数
 es5中严格模式下禁止声明块级函数，而在es6的严格模式中可以声明块级函数，并可以在块级作用
 2. 箭头函数

箭头函数是es6中新增的一种特殊函数，不管是否处于严格模式下，箭头函数的参数都不能有同名的。而ES5中，只有在严格模式下函数的参数才不能有同名

3.字面量对象重复属性问题

es5的严格模式中，如果对象属性重名则会报错，es6则不管是不是严格模式，如果有同名属性，则后面的属性覆盖前面的

4.模块模式

采用es6的模块化开发时，代码自动运行在严格模式下且没有任何办法跳出严格模式

5.类模式

在es6中不管是类声明还是类表达式，其内部使用的都是严格模式。
```



#### 6.Symbol

​	ES6 新增 一种数据类型Symbol，专门为了给对象的key赋值，这样对象的key不相同，则值不会被覆盖。

​	它的功能类似于一种标识唯一性的ID。通常情况下，我们可以通过调用Symbol()函数来创建一个Symbol实例：

```js
let s1 = Symbol()
	
	//或者，你也可以在调用Symbol()函数时传入一个可选的字符串参数，相当于给你创建的Symbol实例一个描述信息：
	let s2 = Symbol('another symbol')
```

**Symbol 方法接收一个参数，表示对生成的 symbol 值的一种描述**

Object.keys( )  不能拿到symbol

Reflect.ownkeys( ) 可以拿到symbol

Symbol.for 方法可以检测上下文中是否已经存在使用该方法且相同参数创建的 symbol 值，如果存在则返回已经存在的值，如果不存在则新建。

```js
const s1 = Symbol.for('foo');
const s2 = Symbol.for('foo');

console.log(s1 === s2); // true
复制代码
```

Symbol.keyFor 方法返回一个使用 Symbol.for 方法创建的 symbol 值的 key

```js
const foo = Symbol.for("foo");
const key = Symbol.keyFor(foo);

console.log(key) // "foo"
```

`Symbol.for()`与`Symbol()`这两种写法，都会生成新的 Symbol。它们的区别是，前者会被登记在全局环境中供搜索，后者不会。`Symbol.for()`不会每次调用就返回一个新的 Symbol 类型的值，而是会先检查给定的`key`是否已经存在，如果不存在才会新建一个值。比如，如果你调用`Symbol.for("cat")`30 次，每次都会返回同一个 Symbol 值，但是调用`Symbol("cat")`30 次，会返回 30 个不同的 Symbol 值。



#### 7.模板字符串

在ES6之前，我们只能用加号连接变量表达式，代码如下：

```javascript
var a = 20;
var b = 10;
var c = "JavaScript";
var str = "My age is " + (a + b) + " and I love " + c;
console.log(str);
```

使用模板字符串嵌入表达式，代码如下：

```javascript
const a = 20;
const b = 10;
const c = "JavaScript";
const str = `My age is ${a+b} and I love ${c}`;
console.log(str);
```

多行字符串

Es6的模板字符串给我们提供了一种创建多行文本字符串的新方法。在ES6之前我们只能使用"\n“在字符串中进行换行，代码如下：

```javascript
console.log("1\n2\n3");
//output
//1
//2
//3
```

在es6中，我们可以直接输入回车进行换行，如下段代码所示：

```javascript
console.log(`1
2
3`);
```

原始字符串(Raw strings)

原始字符串是一个普通字符串，其中不会解释转义字符。我们可以使用模板字符串创建原始字符串。我们可以使用String.raw来获取原始字符串，代码如下：

```javascript
let s = String.raw `xy\n${ 1 + 1 }z`;
console.log(s); 
//output "xy\n2z"
```

- 任何以\u开头的内容都将被视为Unicode转义
- 以\x开头的任何内容都将被视为十六进制转义
- 任何以\开头然后跟着一个数字将被视为八进制转义

JavaScript 规定有5个字符，不能在字符串里面直接使用，只能使用转义形式。

- U+005C：反斜杠（reverse solidus)
- U+000D：回车（carriage return）
- U+2028：行分隔符（line separator）
- U+2029：段分隔符（paragraph separator）
- U+000A：换行符（line feed）

#### 8.运算符

拓展运算符

- ES2016 新增了一个指数运算符（`**`）。

  ```javascript
  2 ** 2 // 4
  2 ** 3 // 8
  ```

逻辑赋值运算符

```javascript
// 或赋值运算符
x ||= y
// 等同于
x || (x = y)

// 与赋值运算符
x &&= y
// 等同于
x && (x = y)

// Null 赋值运算符
x ??= y
// 等同于
x ?? (x = y)
```

**但是只有运算符左侧的值为`null`或`undefined`时，才会返回右侧的值。**



#### 9.解构赋值

##### 1.数组的解构赋值

- ```javascript
  let [a, b, c] = [1, 2, 3];
  ```

本质上，这种写法属于“模式匹配”，只要等号两边的模式相同，左边的变量就会被赋予对应的值。下面是一些使用嵌套数组进行解构的例子。

```js
let [foo, [[bar], baz]] = [1, [[2], 3]];
foo // 1
bar // 2
baz // 3

let [ , , third] = ["foo", "bar", "baz"];
third // "baz"

let [x, , y] = [1, 2, 3];
x // 1
y // 3

let [head, ...tail] = [1, 2, 3, 4];
head // 1
tail // [2, 3, 4]

let [x, y, ...z] = ['a'];
x // "a"
y // undefined
z // []

```

如果解构不成功，变量的值就等于`undefined`。

```javascript
let [foo] = [];
let [bar, foo] = [1];
```

以上两种情况都属于解构不成功，`foo`的值都会等于`undefined`。

不完全解构，即等号左边的模式，只匹配一部分的等号右边的数组。这种情况下，解构依然可以成功。

```javascript
let [x, y] = [1, 2, 3];
x // 1
y // 2

let [a, [b], d] = [1, [2, 3], 4];
a // 1
b // 2
d // 4
```

如果等号的右边不是数组（或者严格地说，不是可遍历的结构，参见《Iterator》一章），那么将会报错。

```javascript
// 报错
let [foo] = 1;
let [foo] = false;
let [foo] = NaN;
let [foo] = undefined;
let [foo] = null;
let [foo] = {};
```

上面的语句都会报错，因为等号右边的值，要么转为对象以后不具备 Iterator 接口（前五个表达式），要么本身就不具备 Iterator 接口（最后一个表达式）。

对于 Set 结构，也可以使用数组的解构赋值。

```javascript
let [x, y, z] = new Set(['a', 'b', 'c']);
x // "a"
```

默认值

解构赋值允许指定默认值。

```javascript
let [foo = true] = [];
foo // true

let [x, y = 'b'] = ['a']; // x='a', y='b'
let [x, y = 'b'] = ['a', undefined]; // x='a', y='b'
```

注意，ES6 内部使用严格相等运算符（`===`），判断一个位置是否有值。所以，只有当一个数组成员严格等于`undefined`，默认值才会生效。

```javascript
let [x = 1] = [undefined];
x // 1

let [x = 1] = [null];
x // null
```

上面代码中，如果一个数组成员是`null`，默认值就不会生效，因为`null`不严格等于`undefined`。

##### 2.对象的解构赋值

- ​	基本用法

  - ```js
    let { foo, bar } = { foo: 'aaa', bar: 'bbb' };
    foo // "aaa"
    bar // "bbb"
    ```

- 解构不仅可以用于数组，还可以用于对象。

  ```javascript
  let { foo, bar } = { foo: 'aaa', bar: 'bbb' };
  foo // "aaa"
  bar // "bbb"
  ```

  对象的解构与数组有一个重要的不同。数组的元素是按次序排列的，变量的取值由它的位置决定；而对象的属性没有次序，变量必须与属性同名，才能取到正确的值。

  ```javascript
  let { bar, foo } = { foo: 'aaa', bar: 'bbb' };
  foo // "aaa"
  bar // "bbb"
  
  let { baz } = { foo: 'aaa', bar: 'bbb' };
  baz // undefined
  ```

- 如果变量名与属性名不一致，必须写成下面这样。

  ```javascript
  let { foo: baz } = { foo: 'aaa', bar: 'bbb' };
  baz // "aaa"
  
  let obj = { first: 'hello', last: 'world' };
  let { first: f, last: l } = obj;
  f // 'hello'
  l // 'world'
  ```

  ```javascript
  let { foo: baz } = { foo: 'aaa', bar: 'bbb' };
  baz // "aaa"
  foo // error: foo is not defined
  ```

  上面代码中，`foo`是匹配的模式，`baz`才是变量。真正被赋值的是变量`baz`，而不是模式`foo`。

- 与数组一样，解构也可以用于嵌套结构的对象。

  ```javascript
  let obj = {
    p: [
      'Hello',
      { y: 'World' }
    ]
  };
  
  let { p: [x, { y }] } = obj;
  x // "Hello"
  y // "World"
  ```

- 默认值

对象的解构也可以指定默认值。

```javascript
var {x = 3} = {};
x // 3

var {x, y = 5} = {x: 1};
x // 1
y // 5

var {x: y = 3} = {};
y // 3

var {x: y = 3} = {x: 5};
y // 5

var { message: msg = 'Something went wrong' } = {};
msg // "Something went wrong"
```

默认值生效的条件是，对象的属性值严格等于`undefined`

```javascript
var {x = 3} = {x: undefined};
x // 3

var {x = 3} = {x: null};
x // null
```

上面代码中，属性`x`等于`null`，因为`null`与`undefined`不严格相等，所以是个有效的赋值，导致默认值`3`不会生效。

注意

（1）如果要将一个已经声明的变量用于解构赋值，必须非常小心。

```javascript
// 错误的写法
let x;
{x} = {x: 1};
// SyntaxError: syntax error
```

上面代码的写法会报错，因为 JavaScript 引擎会将`{x}`理解成一个代码块，从而发生语法错误。只有不将大括号写在行首，避免 JavaScript 将其解释为代码块，才能解决这个问题。

```javascript
// 正确的写法
let x;
({x} = {x: 1});
```

（2）解构赋值允许等号左边的模式之中，不放置任何变量名。因此，可以写出非常古怪的赋值表达式。

```javascript
({} = [true, false]);
({} = 'abc');
({} = []);
```

上面的表达式虽然毫无意义，但是语法是合法的，可以执行。

（3）由于数组本质是特殊的对象，因此可以对数组进行对象属性的解构。

```javascript
let arr = [1, 2, 3];
let {0 : first, [arr.length - 1] : last} = arr;
first // 1
last // 3
```

上面代码对数组进行对象解构。数组`arr`的`0`键对应的值是`1`，`[arr.length - 1]`就是`2`键，对应的值是`3`。方括号这种写法，属于“属性名表达式”（参见《对象的扩展》一章）。

##### 3.字符串的解构赋值

字符串也可以解构赋值。这是因为此时，字符串被转换成了一个类似数组的对象。

```javascript
const [a, b, c, d, e] = 'hello';
a // "h"
b // "e"
c // "l"
d // "l"
e // "o"
```

类似数组的对象都有一个`length`属性，因此还可以对这个属性解构赋值。

```javascript
let {length : len} = 'hello';
len // 5
```

##### 4.数值和布尔值的解构赋值

解构赋值时，如果等号右边是数值和布尔值，则会先转为对象。

```javascript
let {toString: s} = 123;
s === Number.prototype.toString // true

let {toString: s} = true;
s === Boolean.prototype.toString // true
```

上面代码中，数值和布尔值的包装对象都有`toString`属性，因此变量`s`都能取到值。

解构赋值的规则是，只要等号右边的值不是对象或数组，就先将其转为对象。由于`undefined`和`null`无法转为对象，所以对它们进行解构赋值，都会报错。

```javascript
let { prop: x } = undefined; // TypeError
let { prop: y } = null; // TypeError
```

##### 5.函数参数的解构赋值

函数的参数也可以使用解构赋值。

```javascript
function add([x, y]){
  return x + y;
}

add([1, 2]); // 3
```

上面代码中，函数`add`的参数表面上是一个数组，但在传入参数的那一刻，数组参数就被解构成变量`x`和`y`。对于函数内部的代码来说，它们能感受到的参数就是`x`和`y`。

函数参数的解构也可以使用默认值。

```javascript
function move({x = 0, y = 0} = {}) {
  return [x, y];
}

move({x: 3, y: 8}); // [3, 8]
move({x: 3}); // [3, 0]
move({}); // [0, 0]
move(); // [0, 0]
```

上面代码中，函数`move`的参数是一个对象，通过对这个对象进行解构，得到变量`x`和`y`的值。如果解构失败，`x`和`y`等于默认值。

##### 6.圆括号问题

解构赋值虽然很方便，但是解析起来并不容易。对于编译器来说，一个式子到底是模式，还是表达式，没有办法从一开始就知道，必须解析到（或解析不到）等号才能知道。

由此带来的问题是，如果模式中出现圆括号怎么处理。ES6 的规则是，只要有可能导致解构的歧义，就不得使用圆括号。

但是，这条规则实际上不那么容易辨别，处理起来相当麻烦。因此，建议只要有可能，就不要在模式中放置圆括号。

###### 1.不能使用圆括号的情况

以下三种解构赋值不得使用圆括号。

（1）变量声明语句

```javascript
// 全部报错
let [(a)] = [1];

let {x: (c)} = {};
let ({x: c}) = {};
let {(x: c)} = {};
let {(x): c} = {};

let { o: ({ p: p }) } = { o: { p: 2 } };
```

上面 6 个语句都会报错，因为它们都是变量声明语句，模式不能使用圆括号。

（2）函数参数

函数参数也属于变量声明，因此不能带有圆括号。

```javascript
// 报错
function f([(z)]) { return z; }
// 报错
function f([z,(x)]) { return x; }
```

（3）赋值语句的模式

```javascript
// 全部报错
({ p: a }) = { p: 42 };
([a]) = [5];
```

上面代码将整个模式放在圆括号之中，导致报错。

```javascript
// 报错
[({ p: a }), { x: c }] = [{}, {}];
```

上面代码将一部分模式放在圆括号之中，导致报错。

###### 2.可以使用圆括号的情况

可以使用圆括号的情况只有一种：赋值语句的非模式部分，可以使用圆括号。

```javascript
[(b)] = [3]; // 正确
({ p: (d) } = {}); // 正确
[(parseInt.prop)] = [3]; // 正确
```

上面三行语句都可以正确执行，因为首先它们都是赋值语句，而不是声明语句；其次它们的圆括号都不属于模式的一部分。第一行语句中，模式是取数组的第一个成员，跟圆括号无关；第二行语句中，模式是`p`，而不是`d`；第三行语句与第一行语句的性质一致。

##### 7.用途

变量的解构赋值用途很多。

**（1）交换变量的值**

```javascript
let x = 1;
let y = 2;

[x, y] = [y, x];
```

上面代码交换变量`x`和`y`的值，这样的写法不仅简洁，而且易读，语义非常清晰。

**（2）从函数返回多个值**

函数只能返回一个值，如果要返回多个值，只能将它们放在数组或对象里返回。有了解构赋值，取出这些值就非常方便。

```javascript
// 返回一个数组

function example() {
  return [1, 2, 3];
}
let [a, b, c] = example();

// 返回一个对象

function example() {
  return {
    foo: 1,
    bar: 2
  };
}
let { foo, bar } = example();
```

**（3）函数参数的定义**

解构赋值可以方便地将一组参数与变量名对应起来。

```javascript
// 参数是一组有次序的值
function f([x, y, z]) { ... }
f([1, 2, 3]);

// 参数是一组无次序的值
function f({x, y, z}) { ... }
f({z: 3, y: 2, x: 1});
```

**（4）提取 JSON 数据**

解构赋值对提取 JSON 对象中的数据，尤其有用。

```javascript
let jsonData = {
  id: 42,
  status: "OK",
  data: [867, 5309]
};

let { id, status, number } = jsonData;

console.log(id, status, number);
// 42, "OK", [867, 5309]
```

上面代码可以快速提取 JSON 数据的值。

**（5）函数参数的默认值**

```javascript
jQuery.ajax = function (url, {
  async = true,
  beforeSend = function () {},
  cache = true,
  complete = function () {},
  crossDomain = false,
  global = true,
  // ... more config
} = {}) {
  // ... do stuff
};
```

指定参数的默认值，就避免了在函数体内部再写`var foo = config.foo || 'default foo';`这样的语句。

**（6）遍历 Map 结构**

任何部署了 Iterator 接口的对象，都可以用`for...of`循环遍历。Map 结构原生支持 Iterator 接口，配合变量的解构赋值，获取键名和键值就非常方便。

```javascript
const map = new Map();
map.set('first', 'hello');
map.set('second', 'world');

for (let [key, value] of map) {
  console.log(key + " is " + value);
}
// first is hello
// second is world
```

如果只想获取键名，或者只想获取键值，可以写成下面这样。

```javascript
// 获取键名
for (let [key] of map) {
  // ...
}

// 获取键值
for (let [,value] of map) {
  // ...
}
```

**（7）输入模块的指定方法**

加载模块时，往往需要指定输入哪些方法。解构赋值使得输入语句非常清晰。

```javascript
const { SourceMapConsumer, SourceNode } = require("source-map");
```

## 

#### 10.面向对象

​		将所有的物体抽象为一个类，每一个具体的个体是类的实例化对象。构造函数是创建类的方法，可以无限制创建类，相当于工厂。

​		数据类型中的类是一种数据类型，而面向对象是一种编程思想，更高级的还有函数式编程。面向对象里面也有属性和方法。有构造函数创建。为了避免资源的浪费，再构造函数里面创建实例化对象的属性，再构造函数的原型上创建对象的方法。

- ​	模板： ——> 构造函数是用来生成对象的，
- ​    php python 是一种面向对象的编程语言     ->    以类为创建对象的模板
- ​     JavaScript 是基于对象的编程语言   ->    以构造函数为模板 ->动态语言：没有定义数据类型，需要时才会去处理



##### 1.构造函数

创建构造函数

- 不能使用箭头函数
- 首字符大写的函数
- 建议不使用函数表达式，如果以函数表达式创建箭头函数，后面进行继承操作的时候，由于需要给子的构造函数还原，若没有名字，则还原之后他的constructor 不具名。

```js
const Animal = function(){}
const Cat = function(){}
```



- 在构造函数中，一般只写实例化对象的属性，将实例化对象的方法写在构造函数的原型上，形如

- ```js
  function Animal(name,sex){
      this.name = name;
      this.sex = sex;
  }
  
  Animal.prototype.play = function(){
      console.log('play');
  }
  ```



new 关键字

- ​		原理：

  - 创建一个空对象 
  - 空的原型
  - this 的指向变为空对象

- ```js 
  const cat = new Cat('实际参数')
  //若不需要传递参数，则后面的（）可以省略
  //形如
  const cat = new Car
  ```

  在构造函数中使用this 关键字，this 指向未来即将生成的实例化对象

  ```js
  //为了保证实例化对象的时候，使用new 关键字，可以在构造函数内部使用，严格模式
  //如果在实例化对象的时候，没有使用new 关键字系统就会报错
  'use strict'    ....
  若实例化时没有加new，实例化对象为未定义，而对象的属性直接报错
  ```

  ```js
  function Car(){
      this.name = 'nem';
      this.sex = 'cute';
  }
  
  const car =Car();
  console.log(car);
  // underfinded
  ```



- 构造函数返回值
  - 构造函数的返回值如果时基础数据类型，对实例化对象没有影响。
  - 如果构造函数的返回值是引用数据类型，则会覆盖掉构造函数内部的属性。
    - 故构造函数不能有返回值

new.target命令

函数内部可以使用`new.target`属性。如果当前函数是`new`命令调用，`new.target`指向当前函数，否则为`undefined`。

```js
function f() {
  console.log(new.target === f);
}
f() // false
new f() // true
```

使用这个属性，可以判断函数调用的时候，是否使用`new`命令。

```js
function f() {
  if (!new.target) {
    throw new Error('请使用 new 命令调用！');
  }
  // ...
}
f() // Uncaught Error: 请使用 new 命令调用！
```

上面代码中，构造函数`f`调用时，没有使用`new`命令，就抛出一个错误。



Object.create( ) 创建实例对象

通过 Object.create 创建的对象原型为空。

```js
//用于继承
Fn1.prototype = Oject.create( Fn2.prototype )
```

- 如果需要一个构造函数的内容的部分包含另一个构造函数的内容，并且又有自己的

  属性，该怎么写

  ```js
  function Animal(name,sex){
      this.name = name;
      this.sex = sex;
  }
  
  function Cat(name,sex,scorlls){
      Animal.call(this,name,sex);
      this.scorlls = scorlls;
  }
  
  //此处的call也可替换为apply，但后面参数要写成数组的形式
  Animal.apply( this,[name,sex])
  ```



##### 2.原型和原型链

一、prototype

在JavaScript中，每个函数都有一个prototype属性，这个属性指向函数的原型对象。并且这个属性是一个对象数据类型的值.

除了underfinded 和 null 以外，所有的数据类型的原型都是Oject



`Person           ------     prototype ---					Perosn.prototype`

`(构造函数)								           --->   						(实现原理)`



二、__proto__

这是每个对象(除null外)都会有的属性，叫做__proto__，这个属性会指向该对象的原型。



三、constructor

每个原型都有一个constructor属性，指向该关联的构造函数。

```js
function Person() {

}
console.log(Person===Person.prototype.constructor)  //true

//现在基本已不被使用
obj.__proto__ = obj.construcror.prototype
被右边的写法替代
```



![image-20211222192440398](%E5%AD%A6%E4%B9%A0/mdimages/image-20211222192440398.png)

四、实例与原型

 当读取实例的属性时，如果找不到，就会查找与对象关联的原型中的属性，如果还查不到，就去找原型的原型，一直找到最顶层为止。

五、原型的原型

 在前面，我们已经讲了原型也是一个对象，既然是对象，我们就可以用最原始的方式创建它，那就是：

```
var obj = new Object();
obj.name = 'Kevin'
console.log(obj.name) // Kevin
```

其实原型对象就是通过 Object 构造函数生成的，结合之前所讲，实例的 __proto__ 指向构造函数的 prototype ，所以我们再更新下关系图：

```js
//原型的最上面是object
//object 的上面是 null
```



六、原型链

 简单的回顾一下构造函数、原型和实例的关系：每个构造函数都有一个原型对象，原型对象都包含一个指向构造函数的指针，而实例都包含一个指向原型对象的内部指针。那么假如我们让原型对象等于另一个类型的实例，结果会怎样？显然，此时的原型对象将包含一个指向另一个原型的指针，相应地，另一个原型中也包含着一个指向另一个构造函数的指针。假如另一个原型又是另一个类型的实例，那么上述关系依然成立。如此层层递进，就构成了实例与原型的链条。这就是所谓的原型链的基本概念。——摘自《javascript高级程序设计》

```js
//而原型链中就是实例对象和原型对象之间的链接。每个函数都有一个prototype属性，这个prototype属性就是我们的原型对象，我们拿这个函数通过new构造函数创建出来的实例对象，这个实例对象自己会有一个指针(_proto_)指向他的构造函数的原型对象！这样构造函数和实例对象之间就通过( _proto_ )连接在一起形成了一条链子。
```

在JavaScript中 实例化对象与原型之间的链接，叫做原型链，其基本思想是利用原型让一个引用类型继承另一个引用类型的属性和方法，然后层层递进，就构成了实例与原型的链条，这就是所谓的原型链

###### 为什么要使用原型链呢？

1.为了实现继承，简化代码，实现代码重用！
2.只要是这个链条上的内容，都可以被访问和使用到！

###### 使用原型链有什么作用？
继承
prototype用来实现基于原型的继承与属性的共享
避免了代码冗余，公用的属性和方法，可以放到原型对象中，这样，通过该构造函数实例化的所有对象都可以使用该对象的构造函数中的属性和方法！
减少了内存占用

###### 原型链的特点
**就近原则**，当我们要使用一个值时，程序会优先查找离自己最近的，也就是本身有没有，如果自己没有，他就会沿着原型链向上查找，如果还没有找到，它还会沿着原型链继续向上查找，找到到达Object
**引用类型，**当我们使用或者修改原型链上的值时，其实使用的是同一个值！
JS中每个函数都存在原型对象属性prototype。并且所有函数的默认原型都是Object的实例。
每个继承父函数的实例对象都包含一个内部属性_proto_。该属性包含一个指针，指向父函数的prototype。若父函数的原型对象的_proto_属性为再上一层函数。在此过程中就形成了原型链。

###### ES5继承

```js
//Es5 继承时，函数尽量不使用函数声明语法
function Animal(name,sex){
    this.name = name;
    this.sex = sex;
}

Animal.prototype.play = function(){
    console.log('play');
}

function Cat(name,sex,scorlls){
    Animal.call(this,name,sex);
    this.scorlls = scorlls;
}

Cat.prototype = Object.create(Animal.prototype);
// Cat.prototype.constructor = Cat;
Cat.prototype.constructor = Cat;

const cat = new Cat('cat','like','ball');
const ani = new Animal('any','dislike')


// Cat.prototype.constructor = Cat;
cat.play();
// Animal.play()
ani.play()
console.log(cat,ani.constructor.prototype)

//如果子对象存在自己的方法，则应该写在
Cat.prototype = Object.create(Animal.prototype)
//写在前面的话一旦赋值给空白对象，方法就会丢失
```

###### ES6继承（语法糖）

```js
class Animal{
    constructor(name,sex){
		this.name = name;
        this.sex = sex;
    }
    eat(){
	  console.log('吃东西');
    }
}

class cat extends Animal {
	constructor(pz,name,sex){
	super(name,sex);
    this.pz = pz;
    }
    miao(){
		console.log('喵...');
    }
}

constructor(pz,name,sex){
	super(name,sex);
  //  里面的参数不需要对应，可以是任意顺序
```



##### static 

```js
//凡是被static修饰的属性和方法都是静态方法和属性,只能被类名调用,不能被实例化对象调用.同时也不能被子类继承,换句话说它属于当前这个类的.
//若是被实例化对象调用结果为underfinded

//使用static修饰属性和方法

    class A {
      // 静态属性
      static info = '见过你的美,还能爱上谁?';
      // 静态方法
      static love() {
        console.log('小姐姐,看见你就犯困,为情所困,为你所困!');
​
      }
    }
    // 直接使用类名调用
    console.log(A.info);
    A.love();

```

能被继承

extends的主要用于子类继承父类,继承之后子类拥有父类的的所有方法包括,静态方法和属性

```js
 class A {
      // 静态属性
      static info = '见过你的美,还能爱上谁?';
      // 静态方法
      static love() {
        console.log('小姐姐,看见你就犯困,为情所困,为你所困!');
      }
      // 普通方法,调用静态属性
      say() {
        console.log('小姐姐,' + A.info);
      }
    }
    class B extends A {
      run() {
        console.log('类B的方法..');
      }
    }
    //使用类名 父类中普通方法调用
    B.love();
```

##### 原型链覆盖

```js
fn.prototype = { ...}
//这样写原来对象上的方法和属性都被覆盖掉
```



#### 11.字符串方法

##### 1.ES6 String.raw()

ES6 还为原生的 String 对象，提供了一个`raw()`方法。该方法返回一个斜杠都被转义（即斜杠前面再加一个斜杠）的字符串，往往用于模板字符串的处理方法。

```javascript
String.raw`Hi\n${2+3}!`
// 实际返回 "Hi\\n5!"，显示的是转义后的结果 "Hi\n5!"

String.raw`Hi\u000A!`;
// 实际返回 "Hi\\u000A!"，显示的是转义后的结果 "Hi\u000A!"
```

如果原字符串的斜杠已经转义，那么`String.raw()`会进行再次转义。

```javascript
String.raw`Hi\\n`
// 返回 "Hi\\\\n"

String.raw`Hi\\n` === "Hi\\\\n" // true
```

`String.raw()`方法可以**作为处理模板字符串的基本方法，它会将所有变量替换，而且对斜杠进行转义，方便下一步作为字符串来使用。**

##### 2.String.fromCodePoint()

ES6 提供了`String.fromCodePoint()`方法，可以识别大于`0xFFFF`的字符，弥补了`String.fromCharCode()`方法的不足。在作用上，**正好与下面的`codePointAt()`方法相反**。

```javascript
String.fromCodePoint(0x20BB7)
// "𠮷"
String.fromCodePoint(0x78, 0x1f680, 0x79) === 'x\uD83D\uDE80y'
// true
```

上面代码中，如果`String.fromCodePoint`方法有多个参数，则它们会被合并成一个字符串返回。

注意，`fromCodePoint`方法定义在`String`对象上，而`codePointAt`方法定义在字符串的实例对象上。

##### 3.实例方法：codePointAt() 

JavaScript 内部，字符以 UTF-16 的格式储存，每个字符固定为`2`个字节。对于那些需要`4`个字节储存的字符（Unicode 码点大于`0xFFFF`的字符），JavaScript 会认为它们是两个字符。

```javascript
var s = "𠮷";

s.length // 2
s.charAt(0) // ''
s.charAt(1) // ''
s.charCodeAt(0) // 55362
s.charCodeAt(1) // 57271
```

上面代码中，汉字“𠮷”（注意，这个字不是“吉祥”的“吉”）的码点是`0x20BB7`，UTF-16 编码为`0xD842 0xDFB7`（十进制为`55362 57271`），需要`4`个字节储存。对于这种`4`个字节的字符，JavaScript 不能正确处理，字符串长度会误判为`2`，而且`charAt()`方法无法读取整个字符，`charCodeAt()`方法只能分别返回前两个字节和后两个字节的值。

ES6 提供了`codePointAt()`方法，能够正确处理 4 个字节储存的字符，返回一个字符的码点。

```javascript
let s = '𠮷a';

s.codePointAt(0) // 134071
s.codePointAt(1) // 57271

s.codePointAt(2) // 97
```

`codePointAt()`方法的参数，是字符在字符串中的位置（从 0 开始）。上面代码中，JavaScript 将“𠮷a”视为三个字符，codePointAt 方法在第一个字符上，正确地识别了“𠮷”，返回了它的十进制码点 134071（即十六进制的`20BB7`）。在第二个字符（即“𠮷”的后两个字节）和第三个字符“a”上，`codePointAt()`方法的结果与`charCodeAt()`方法相同。
