---
title: js异步编程的那些事

author: codefish
date: 2021-12-24 18:33:07
categories: js
tags: [宏任务,微任务,promise]
top_img: /img/9.jpg
cover: /img/9.jpg
---

主要是关于js运行顺序，宏任务，微任务，任务队列，Event loop，同步事件异步事件

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

##### async 和 await 执行顺序*

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

### 宏任务微任务

#### 1.任务分类

- 同步任务
- 异步任务
  - 宏任务
  - 微任务

#### 2.任务详情

1. 同步任务

   定义：立即执行的任务

2. 异步任务

   定义：调用后不会立即执行的任务，例如定时器，promise,requestAnimationFrame

   - 宏任务
     1. 定时器
     2. requestAnimationFrame
   - 微任务
     1. promise

   

#### 3.任务执行顺序

##### 1..执行顺序

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

##### 2.注意：

- 当执行promise内的代码时，语句遵循同步执行的规则



#### 4.任务执行的原理*

---

​		众所周知，js语言在刚研发出来的时候，请求量很少，不像想在有那么多进程，即被设计为一个单线程语言。

​		当浏览器在执行一个js代码的时候，首先他会从上往下执行代码，当碰到同步任务的时候，它就会让他进入到主线程之中立即执行。**但是**,当它解析到异步代码的时候，它就会将该任务放到一个子进程中去跑，然后继续向下执行，当再遇到同步代码时依旧立即执行，再遇到异步任务同样将其挂载到子进程，直到执行到最后一句代码。

​		此时，主进程空了以后，浏览器就会做一件事情--Event-loop--,什么是eventloop呢，可以理解为，浏览器问任务队列，你的异步任务有执行完成的吗？如果没有我等一下再来问。好，如果此时有异步任务那么就让这个异步事件立即执行。这时就得讲一讲**异步任务从子进程到任务队列的过程**，当异步任务在子进程运行（跑）的时候，....然后有一个运行结束，就比如一个定时器，定时3000，3秒后他跑完，然后就将他的回调加载到任务队列里面去。在任务队列里任务执行的先后，遵循先进先出。

​		到这同步异步就这些，然后宏任务和微任务是怎么执行的呢？这时候就把任务队列划分宏任务队列和微任务队列，宏任务执行完了进宏任务队列，微任务执行完了进微任务队列，当执行Event-loop的时候，先执行宏任务队列里面的任务，再执行微任务队列里面的任务。

#### 4..**requestAnimationFrame	请求动画帧**

requestAnimationFrame最大的优势是**由系统来决定回调函数的执行时机**

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

