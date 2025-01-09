---



title: vben-admin学习1

tags: [vben-admin, vue, typescript]

author: codefish

date: 2025-01-09 23:20:34

categories: vben-admin

top_img: /img/9.jpg

cover: /img/9.jpg



---



开发中用到最多的场景，父子组件通信，分发事件，组件传值。



### 1.generate 迭代器

可以暂停和开始

- *function
- yield
- yield*



#### 1.funtion* 定义一个迭代器函数

1. generator.prototype.next()
2. generator.prototype.return()
3. generator.prototype.throw() 抛出错误

```js
function* generator(i) {
  yield i;
  yield i + 10;
}
const gen = generator(10);
```

**生成器函数有一个next（）方法**

`next()`方法返回一个对象，这个对象包含两个属性：value 和 done，value 属性表示本次 `yield` 表达式的返回值，done 属性为布尔类型，表示生成器后续是否还有 `yield` 语句，即生成器函数是否已经执行完毕并返回。



调用 `next()`方法时，如果传入了参数，那么这个参数会传给**上一条执行的 yield 语句左边的变量**，例如下面例子中的`x`：

```js
function *gen(){
    yield 10;
    x=yield 'foo';
    yield x;
}

var gen_obj=gen();
console.log(gen_obj.next());// 执行 yield 10，返回 10
console.log(gen_obj.next());// 执行 yield 'foo'，返回 'foo'
console.log(gen_obj.next(100));// 将 100 赋给上一条 yield 'foo' 的左值，即执行 x=100，返回 100
console.log(gen_obj.next());// 执行完毕，value 为 undefined，done 为 true
```

**x=yield 'foo' 不同于 let x=yield 'foo'**

后者不会将等号右边的赋值给左边

```
function *createIterator() {
    let first = yield 1;
    let second = yield first + 2; // 4 + 2
                                  // first =4 是 next(4) 将参数赋给上一条的
    yield second + 3;             // 5 + 3
}

let iterator = createIterator();

console.log(iterator.next());    // "{ value: 1, done: false }"
console.log(iterator.next(4));   // "{ value: 6, done: false }"
console.log(iterator.next(5));   // "{ value: 8, done: false }"
console.log(iterator.next());    // "{ value: undefined, done: true }"
```



**接收参数（可以多个**

```js
function* idMaker(){
    console.log(arguments) //{'1':5,'2':7}
    var index = arguments[0] || 0;
    while(true)
        yield index++;
}

var gen = idMaker(5,7);
console.log(gen.next().value); // 5
console.log(gen.next().value); // 6
```



**显示返回**

next()的value变为返回的值，done的状态变为true

**注意**

1. 不能当构造器使用

**应用**

1. 使用迭代器遍历二维数组为一维数组、也可以多维转为一维数组



#### 2.yield* 

**用于委托给另一个**generate或者可迭代对象

`yield*` 表达式本身的值是当迭代器关闭时返回的值（即`done`为`true`时）。

```js
function* g1() {
  yield 2;
  yield 3;
  yield 4;
}

function* g2() {
  yield 1;
  yield* g1();
  yield 5;
}

var iterator = g2();

console.log(iterator.next()); // { value: 1, done: false }
console.log(iterator.next()); // { value: 2, done: false }
console.log(iterator.next()); // { value: 3, done: false }
console.log(iterator.next()); // { value: 4, done: false }
console.log(iterator.next()); // { value: 5, done: false }
console.log(iterator.next()); // { value: undefined, done: true }
```



除了生成器对象这一种可迭代对象，`yield*` 还可以 `yield` 其它任意的可迭代对象，比如说数组、字符串、`arguments` 对象等等。

```js
function* g3() {
  yield* [1, 2];
  yield* "34";
  yield* arguments;
}

var iterator = g3(5, 6);

console.log(iterator.next()); // { value: 1, done: false }
console.log(iterator.next()); // { value: 2, done: false }
console.log(iterator.next()); // { value: "3", done: false }
console.log(iterator.next()); // { value: "4", done: false }
console.log(iterator.next()); // { value: 5, done: false }
console.log(iterator.next()); // { value: 6, done: false }
console.log(iterator.next()); // { value: undefined, done: true }
```

#### 3.迭代器Iterator

ES6 规定，默认的 Iterator 接口部署在数据结构的`Symbol.iterator`属性，Symbol.iterator属性本身是一个函数，就是当前数据结构默认的遍历器生成函数，其实就是我们上面写的myiteration函数，执行这个函数，就会返回一个带有next方法的遍历器对象。至于属性名Symbol.iterator，它是一个表达式，返回`Symbol`对象的`iterator`属性，这是一个预定义好的、类型为 Symbol 的特殊值，所以要放在方括号内。

在js里原生具备 Iterator 接口的数据结构如下：

- Array
- Map
- Set
- String
- TypedArray
- 函数的 arguments 对象
- NodeList 对象 因此，上面的任意一个数据类型都支持for...of遍历，也可以调用自身的



### 2.判断类型

**[参考](https://www.cnblogs.com/onepixel/p/5126046.html#:~:text=%E5%88%A4%E6%96%ADJS%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B%E7%9A%84%E5%9B%9B%E7%A7%8D%E6%96%B9%E6%B3%95%201%201%E3%80%81typeof%20typeof%20%E6%98%AF%E4%B8%80%E4%B8%AA%E6%93%8D%E4%BD%9C%E7%AC%A6%EF%BC%8C%E5%85%B6%E5%8F%B3%E4%BE%A7%E8%B7%9F%E4%B8%80%E4%B8%AA%E4%B8%80%E5%85%83%E8%A1%A8%E8%BE%BE%E5%BC%8F%EF%BC%8C%E5%B9%B6%E8%BF%94%E5%9B%9E%E8%BF%99%E4%B8%AA%E8%A1%A8%E8%BE%BE%E5%BC%8F%E7%9A%84%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B%E3%80%82%20...%202%202%E3%80%81instanceof,%28%29%20%E6%98%AF%20Object%20%E7%9A%84%E5%8E%9F%E5%9E%8B%E6%96%B9%E6%B3%95%EF%BC%8C%E8%B0%83%E7%94%A8%E8%AF%A5%E6%96%B9%E6%B3%95%EF%BC%8C%E9%BB%98%E8%AE%A4%E8%BF%94%E5%9B%9E%E5%BD%93%E5%89%8D%E5%AF%B9%E8%B1%A1%E7%9A%84%20%5B%20%5BClass%5D%5D%20%E3%80%82%20)**

- Object.prototype.toString.call(args)
- instanceof
- typeof
- constructor



工具函数判断 isEmpty

#### 1.Object.prototype.toString.call(args)

```js
Object.prototype.toString.call('') ;   // [object String]
Object.prototype.toString.call(1) ;    // [object Number]
Object.prototype.toString.call(true) ; // [object Boolean]
Object.prototype.toString.call(Symbol()); //[object Symbol]
Object.prototype.toString.call(undefined) ; // [object Undefined]
Object.prototype.toString.call(null) ; // [object Null]
Object.prototype.toString.call(new Function()) ; // [object Function]
Object.prototype.toString.call(new Date()) ; // [object Date]
Object.prototype.toString.call([]) ; // [object Array]
Object.prototype.toString.call(new RegExp()) ; // [object RegExp]
Object.prototype.toString.call(new Error()) ; // [object Error]
Object.prototype.toString.call(document) ; // [object HTMLDocument]
Object.prototype.toString.call(window) ; //[object global] window 是全局对象 global 的引用
```

#### 2.instanceof

instanceof 是用来判断 A 是否为 B 的实例，表达式为：A instanceof B，如果 A 是 B 的实例，则返回 true,否则返回 false。 在这里需要特别注意的是：**instanceof 检测的是原型**，我们用一段伪代码来模拟其内部执行过程：

**instanceof 只能用来判断两个对象是否属于实例关系****， 而不能判断一个对象实例具体属于哪种类型。**

#### 3.typeof

**缺点：对象之间无法细分**

```
typeof` `''``; ``// string 有效
typeof` `1; ``// number 有效
typeof` `Symbol(); ``// symbol 有效
typeof` `true``; ``//boolean 有效
typeof` `undefined; ``//undefined 有效
typeof` `null``; ``//object 无效
typeof` `[] ; ``//object 无效
typeof` `new` `Function(); ``// function 有效
typeof` `new` `Date(); ``//object 无效
typeof` `new` `RegExp(); ``//object 无效
```

- 对于基本类型，除 null 以外，均可以返回正确的结果。
- 对于引用类型，除 function 以外，一律返回 object 类型。
- 对于 null ，返回 object 类型。
- 对于 function 返回  function 类型。

#### 4.constructor

定义变量的时候都隐式的调用了new关键字

1. null 和 undefined 是无效的对象，因此是不会有 constructor 存在的，这两种类型的数据需要通过其他方式来判断。

2. 函数的 constructor 是不稳定的，这个主要体现在自定义对象上，当开发者重写 prototype 后，原有的 constructor 引用会丢失，constructor 会默认为 Object

因此，**为了规范开发**，在重写对象原型时一般都需要重新给 constructor 赋值，以保证对象实例的类型不被篡改。

#### 特殊判定

1. 判定数组 Array.isArray()
2. 判断 null 和 underfinded isNull ， isUnderfinded
3. 判断空数组 JSON.stringify(obj) === '[]'
4. 

Array.isArray() 



### 3.map

**`Map`** 对象保存键值对，并且能够记住键的原始插入顺序。任何值（对象或者[基本类型](https://developer.mozilla.org/zh-CN/docs/Glossary/Primitive)）都可以作为一个键或一个值。

属性

- size （无length属性，容易踩坑)

方法

1. clear()
2. delete()
3. entries()
4. forEach
5. get()
6. has()
7. keys()
8. set()
9. values()





**set与object对比**

[参考mdn](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Map)

一个 `Object` 的键必须是一个 [`String`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String) 或是 [`Symbol`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol)。

一个 `Map` 的键可以是**任意值**，包括函数、对象或任意基本类型。

一个 `Object` 有一个原型，原型链上的键名有可能和你自己在对象上的设置的键名产生冲突。

`Map` 默认情况不包含任何键。只包含显式插入的键。

虽然 `Object` 的键目前是有序的，但并不总是这样，而且这个顺序是复杂的。因此，最好不要依赖属性的顺序。

`Map` 中的键是有序的。

`Object` 的键值对个数只能手动计算。

`Map` 的键值对个数可以轻易地通过 [`size`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Map/size) 属性获取。

JavaSctipt 的 [for...of](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for...of) 表达式并不能直接迭代对象。for of 配合 Object.keys()

`Map` 是 [可迭代的](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Iteration_protocols) 的，所以可以直接被迭代。

Set在频繁增删键值对的场景下表现更好。



#### 1.values()

**`values()`** 方法返回一个新的[*迭代器*](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Iterators_and_Generators)对象。它包含按顺序插入 `Map` 对象中每个元素的 `value` 值

#### 2.keys

**`keys()`** 返回一个引用的[*迭代器*](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Iterators_and_Generators)对象。它包含按照顺序插入 `Map` 对象中每个元素的 key 值。

```js
const map1 = new Map();

map1.set('0', 'foo');
map1.set(1, 'bar');

const iterator1 = map1.keys();

console.log(iterator1.next().value);
// expected output: "0"

console.log(iterator1.next().value);
// expected output: 1

```

理解：作用类似于对象的Object.keys() 但它的实现方式（返回的是迭代器），对象返回的是数组

#### 3.entries

**`entries()`** 方法返回一个新的[*迭代器*](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Iterators_and_Generators)对象，其中包含 `Map` 对象中按插入顺序排列的每个元素的 `[key, value]` 对。在这种情况下，这个迭代器对象也是一个可迭代对象，因此可以使用 for-of 循环。当使用 `[Symbol.iterator]` 时，它返回一个函数，该函数在调用时返回迭代器本身。

```js
const map1 = new Map();

map1.set('0', 'foo');
map1.set(1, 'bar');

const iterator1 = map1.entries();

console.log(iterator1.next().value);
// expected output: Array ["0", "foo"]

console.log(iterator1.next().value);
// expected output: Array [1, "bar"]

```

#### 4.set()

设置值，如果存在值将被覆盖



### 4.set

属性

- size （无length属性，容易踩坑)

方法

1. clear()
2. delete()
3. entries()
4. forEach
5. values()
6. has()
7. keys()

#### 1.keys()

返回迭代器对象，迭代的为key

#### 2.values()

返回迭代器对象，迭代的为value

#### 3.entries()

**`entries()`** 方法返回一个新的[迭代器](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Iterators_and_Generators)对象，这个对象包含的元素是类似 **`[value, value]` 形式的数组**，`value` 是集合对象中的每个元素，迭代器对象元素的顺序即集合对象中元素插入的顺序。由于集合对象不像 `Map` 对象那样拥有 `key`，然而，为了与 `Map` 对象的 API 形式保持一致，故使得每一个*条目*的 *key* 和 *value* 都拥有相同的值，因而最终返回一个 `[value, value]` 形式的数组。

```js
const set1 = new Set();
set1.add(42);
set1.add('forty two');

const iterator1 = set1.entries();

console.log(iterator1.next()) // { value: [ 42, 42 ], done: false } -- 结果无法理解
for (const entry of iterator1) {
  console.log(entry);
  // expected output: Array [42, 42]
  // expected output: Array ["forty two", "forty two"]
}

```

**console.log(iterator1.next()) **

此处返回的迭代器对象，使用gen.next()打印，值会打印出两次



#### 4.add()

```js
const set1 = new Set();

set1.add(42);
set1.add(42);
set1.add(13);

for (const item of set1) {
  console.log(item);
  // expected output: 42
  // expected output: 13
}
```





### 参考

1. [js中的迭代器(Iterator)](https://juejin.cn/post/7018850645226569758)
2. [mdn](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Iterators_and_Generators)



### 思考

js中的接口指的是什么

接口：抽象对象，也具有方法

此处实现功能：生成一个数组之后，定义一个迭代器函数，当调用for of的时候，调用之前定义的迭代器函数

