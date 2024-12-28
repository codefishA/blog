---

title: vue3 初体验
tags: [ref,reactive,reandonly,vite,Suspence,provide,Teleport]
author: codefish
date: 2022-2-15 14:13:36
categories: vue
top_img: /img/12.jpg
cover: /img/12.jpg

---
vue3的更新是颠覆式的，最大的改变就是组合式api,以及shake tree优化代码运行速度，新的响应式原理，封装更多内置方法。

## vue 3.0

---

- Vue -cli
- Vue -router
- VueX



Vue3 不需要根节点(在模板template)

## vue setup 

//新的optioon 所有的api都在次使用，只在初始化执行一次
//组合APi中第一个要使用的函数

Vue3 支持 vue2 大多数语法

```js
data(){
    return {
	count++;
    }
}

methods:{
	numPut(){
        this.count++
    }
}
```

## 1）setup 是组合 api 的入口函数

Vue3 的写法

```js
setup(){
    let count = 0
    function updateCount(){
		count++
    }
}
//此时的数据不是响应的数据
```

引入ref解决

```js
import { ref } from 'Vue'
```

## 2）ref  定义一个响应式的数据

```js
//count未ref类型
const  count = ref(0)
function(){
	count.value++;
}

//返回一个ref对象
//对象中如果要对value操作，需要进行xxx.value
//模板中，直接使用xxx

//创建一个包含响应式数据的引用（refrence)对象
//js 中操作 aa.value
//模板中直接操作 aa
//---一般用来定义一个基本类型的响应式数据


```

## 3）reactive

```JS
//

setup(){
    //对象
    //此时user对象的类型是Proxy
    const user = recative({
	name:"lihua",
        age:19,
    })
}

<h2>名字：{{user.name}}</h2>

//方法两种写法
function changename(){
}

const updateUser = ()=>{
    user.name = 'wang' + user.name
    return {
	user,
    }
}


const obj = {
	name:'hello',
    age:10
}
obj.name = obj.name + 'obj'
//此方式修改对象的值不能响应，只能采用代理对象的方式来更新数据

```

reactive 

```js
//作用：定义多个数据的响应式
const proxy = reactive(obj)
 //接收一个普通对象然后返回该普通对象的响应式代理器对象
//响应式转换是深层的，会影响内部所有嵌套的属性
//内部基于Es6 的Proxy实现，通过代理对象操作源对象内部数据都是响应式的

//***在vue2.0 中，数组内部数据的变化不会引起响应

//把对象变为响应式的数据
//返回的是一个Proxy的代理对象，被代理者就是obj中的传入的对象
const user = reactive({
    name:'张三',
    age:20,
})
```

```js
//对比两种情况下，数据的更新
//对象obj 和 对象user 的变换情况 
const obj = {
    name;'小周',
    age:'20'
}
const user = reactive(obj)

obj.gender = '男'
//不更新

user.gender = '男'
//这种方式界面更新渲染，并且也添加到了obj对象中去

delete user.age  //界面更新渲染，属性被删除
delete obj.age //界面不渲染，属性被删除
引起ts类型报错 加上 const obj:any = {}..

```

补充：

需要注意的是，`reactive`中传递的参数必须是`json`对象或者数组，如果传递了其他对象（比如`new Date()`），在默认情况下修改对象，界面不会自动更新，如果也需要具有响应式，可以通过重新赋值的方式实现

```js
//msg 的数据是存在变化的，但界面并未发生变化，同时我们发现在控制台里，msg 并未被识别成 proxy。
<template>
<div>
  <p>{{msg}}</p>
  <button @click="c">button</button>
</div>
</template>

<script>
import { reactive } from 'vue'
export default {
  name: 'App',
  setup() {
    let msg = reactive(new Date())
    function c() {
      console.log(msg);
      msg.setDate(msg.getDate() + 1);
      console.log(msg);
    }
    return {
      msg,
      c
    };
  }
}
</script>


//显然，对于这种数据类型，我们需要做特殊处理。
//这个特殊处理就是重新赋值（，而不是直接修改原来的值）。
```



基本数据传递给reactive，reactive并不会将它包装成porxy对象，并且当数据变化时，界面也不会变化



## 4）比较Vue2与Vue3的响应式

vue2 

- 核心

  对象：通过defineProperty对对象的已有属性值的读取和修改进行劫持（监视/拦截）

  数组:通过重写数组更新数组一系列更新元素的方法来实现元素修改的劫持

vue 3.0

通过proxy代理，reflect（反射对象），不是函数对象，不能构造 

```js
//将目标对象变成代理对象
//参数1：user-->target目标对象
//参数2：handler -->处理器对象，用来监视数据，及数据的操作
//目标对象
const user = {
    name:"佐助",
    age:20,
    wife:{
	name:'xiaoyin'
    }
}
const proxyUser = new Proxy(user,{
    get(target,prop){},
    set(){},
    delectProperty(){}
})
//通过代理对象获取目标对象中的某个属性值
//reflect反射对象
```

![image-20220215193447751](E:/%E6%A1%8C%E9%9D%A2/%E5%AD%A6%E4%B9%A0/mdimages/image-20220215193447751.png)

通过代理对象修改目标对象的值

通过代理对象向目标对象中添加一个新的属性

![image-20220215193649429](E:/%E6%A1%8C%E9%9D%A2/%E5%AD%A6%E4%B9%A0/mdimages/image-20220215193649429.png)

vue3的深度响应可以深度响应

## 5）setup 细节

```js
//父子组件通信
//父组件

//子组件中
props:['msg']

msg:{
	name:"aq",
    sex:'male'
}
```

setup一般都是返回一个对象，对象中的属性和方法都可以在html模板中直接使用

```js
setup是在beforeCreate生命周期之前就执行了，而且就执行一次
//beforeCreate 此时组件还没创建，组件实例对象this根本不能用

```

setup运行的时机

setup执行的时机

- 在beforeCreate之前执行（一次），此时组件对象还没有创建,不能使用data和methods
- this是underfinded,不能通过this来访问data/computed/methods/props
- 其实所有的composition API 相关回调函数中也都不可以

setup的返回值

- ​	一般都返回一个对象，为模板提供数据，也就是模板中可以直接使用次对象中的所有属性方法
- 返回对象中的属性会与data函数返回对象的属性合并并成为组件对象的属性
- 返回对象中的方法会与methods中的方法合并并成功组件对象的方法
- 如果有重名，setup优先
- 注意
- 一般不要混合使用，methods和data都可以写在setup
- setup不能是async函数，因为返回值不再是一个对象，而是一个promise，模板看不到对象中的属性数据
- 数据初始化的生命周期回调

setup()的参数 

两种写法

1.setup(props,contect)

2.setup(props,attrs,slots,emit)

- props 参数，是一个对象，里面有父级组件向子级组件传递的数据

- contect参数(是一个对象)

- ```js
  1.attrs	
  	//获取当前组件标签的属性，但是该数据是在props中，没有接收的所有的属性的对象
  2.emit
  	//分发事件
  @xxx = 'xxx'
  function xxx(txt){
  	msg.value += txt;
  }
  <button @click='emitXXX'>分发事件</button>
  function emitXXX(){
      context.emit('xxx','+')
  }
  3.expose
  4.slots
  	//插槽
  ```



## 6）ref和reactive细节(响应式API)

- ref也可以传递对象吗
  1. 如果ref里面传递的是对象/数组，那么是经过了reactivity处理，形成了Proxy类型的对象，
  2. ref内部：通过给value属性添加getter/setter来实现对数据的劫持
  3. reactivity内部：通过使用Proxy来实现对对象内部所有的数据的劫持,并通过Reflect操作对象内部数据
  4. ref的数据操作，在js中要.value,在模板中不需要，(内部解析模块会自动添加.value)



**递归监听和非递归监听**

`ref`和`reactive`都属于递归监听，也就是数据的每一层都是响应式的，如果数据量比较大，非常消耗性能，非递归监听只会监听数据的第一层。

***  数组也使用**reactive**



##  7）计算属性与监视

1. ​	计算属性

   ```js
   
   import { ref } from "@vue/reactivity";
   import { computed } from '@vue/runtime-core';
   export default {
     name: "detailforref",
     setup() {
       const m1 =ref("东方");
      const m2 = ref('不败')
      const all = computed(()=>{
          return m1.value + m2.value
      })
      const updataname = () =>{
          m1.value = '西方'
          m2.value = '求败'
      }
       return {
         m1,
         m2,
         all,
         updataname,
       }
   
     },
   };
   ```

   - 计算属性中如果只传入一个回调函数，表示的是get
   - 计算属性返回的是一个ref类型的对象

2. 监视属性

   ```js
   import { ref } from "@vue/reactivity";
   import { watch } from '@vue/runtime-core';
   export default {
     name: "detailforref",
     setup() {
       const m1 =ref("东方");
      const m2 = ref('不败')
      const user = [m1,m2]
      const all = ref('')
   //    const all = computed(()=>{
   //        return m1.value + '_' + m2.value
   //    })
      watch(user,(val)=>{
          return all.value = val[0]+'__'+val[1]
      })
      const updataname = () =>{
          m1.value = '西方'
          m2.value = '求败'
      }
       return {
         m1,
         m2,
         all,
         updataname,
         user
       }
   
     },
   };
   ```

- 不会默认调用，需要手动配置默认调用

```js
immediate:true
// 当前watch默认执行一次
deep:true
//深度监视
watch(user,(val)=>{
       return all.value = val[0]+'__'+val[1]
   },{immediate:true,deep:true})
```

- ​		watchEffect()

```js
//监视，不需要配置immediate，本身默认就会进行监视，(默认执行一次)
watchEffect(()=>{
	fullName3.value = user.firstName + '-' +user.laastName
})
```

- watch可以监视多个数据

```js
watch([user.firstName,user.lastName,Fullnsmes3],()=>{

})
//Fullnames 是响应式数据
//非响应式数据时，需要进行改动

watch([()=>user.firstName,()=>user.lastName],=>{
      
      })
```



## 8）生命周期对比

```js
beforeCreate()
created()
beforemount
mounted
beforeUpdate
updated
beforedestory
destory

*** deforeUnmount
*** unmounted

vue2所有的周期都可以在Vue3中使用
但Vue3中是组合Api

beforeCreated -> 使用 setup()
created -> 使用setup()
beforeMount -> onBeforeMount
mounted -> onMounted
beforeUpdate -> onBeforeUpdate
updated -> onUpdated
beforeDestory -> onBeforeUnmount
destoryed -> onUnmounted
errorCaptured -> onErrorCaptured

新增调试周期函数
onRenderTracked
onReaderTriggered
两个钩子函数都接受一个DebuggerEvent,与watchEffect 参数选项中的onTrack和onTrigger类似
```

3.0里面的生命周期钩子比2.0生命周期快



## 9）自定义hook函数

类似于mixin

//公共接口是否可以混入组件或者临时存储

```js
import {ref} from "@vue/reactivity";
import { onBeforeUnmount, onMounted} from 'vue'
export default function(){
//   name: "detailforref",
 
    const x = ref(-1);
    const y = ref(-1);
    const clickHandler = (event) =>{
      x.value = event.pageX
      console.log(event.pageX)
      y.value = event.pageY
    }
    //页面加载完毕，再进行点击操作
     onMounted(()=>{
       window.addEventListener('click',clickHandler)
    // console.log("page reload success");
  })
  onBeforeUnmount(()=>{
    window.removeEventListener('click',clickHandler)
  })
    return {
      x,
      y,
    }
  
 
}
```

```js
import {defineComponent} from 'vue'
import hook from './hook'
export default defineComponent({
  name:'detailforhook',
  setup(){
    const {x,y} =hook()
    return {
      x,y
    }
  }

})
```







https://juejin.cn/post/6977929393511514148

---

#### 1. ref和shallowRef

- `ref`定义的数据每一层都是响应式数据
- `shallowRef`定义的数据，只有第一层是响应式的，即只有在更改`.value`的时候才能实现响应式

#### 2.toRaw

**toRaw的出现是解决什么问题呢？**

有些时候我们不希望数据进行响应式实时更新，可以通过`toRaw`获取`ref`或`reactive`引用的原始数据，通过修改原始数据，不会造成界面的更新，只有通过修改`ref`和`reactive`包装后的数据时才会发生界面响应式变化。

```js
let obj1 = {...};
//state和obj1是引用关系，state的本质是一个Proxy对象，其中引用了obj1
let state = reactive(obj1);
//通过toRaw方法获取到原始数据，其实是获取到obj1的内存地址，obj2和obj1是完全相等的
let obj2 = toRaw(state)
console.log(obj1 === obj2);//true

```

#### 3.markRow

> **与toRaw不同，markRaw包装后的数据永远不会被追踪！**
>
> 暂时没发现有什么用处(手动狗头)

```typescript
let obj1 = {name: "lijing", age: 18}
let obj2 = markRaw(obj1);
//此时reactive包装的数据虽然是响应式对象，但是不会被跟踪，也不会产生效应式效果
let state1 = reactive(obj2)

console.log(obj1 === obj2);//true
```

#### toRef和toRefs

##### 1)ref

```js
使用ref对一个对象的某个简单数据类型属性进行响应式改造后，通过修改响应式数据不会影响到原始数据，如上图中，通过state1修改值后，obj1中的a属性值没有发生变化。这里有个注意点：修改的这个属性必须是简单数据类型，一个具体的值，不能是引用，如果该属性也是一个对象，则会影响，因为对象--->引用！

```

##### 2）ref的另一个作用

```js
<button ref='inoutref'></input>
//获取input框的焦点
const inputRef = ref(null)
onMounted(()=>{
      if(inputRef.value){
        console.log(inputRef)
        inputRef.value.focus()
      }
    })
```

#### 2. toRef

为源响应式对象上的某个属性创建一个ref对象，二者内部操作的是同一个数据值，**更新时二者的是同一个数据值，更新时二者是同步的**

- 区别ref：拷贝了一份新的数值单独操作，更新时互不影响，toRef两个数据之间有关联
- 应用：当要将某个prop的ref传递给复合函数时，toRef很有用
  - 原本时传递普通数据，若使用toRef则传递的是ref对象

如果使用toRef来转换，则修改响应式数据会影响到原始数据，数据发生改变，但是界面不会自动更新

转换后的是一个`ObjectRefImpl`类型

#### 3. toRefs

将一个响应式对象转换为普通对象，该普通对象 的每一个property都是一个ref

```js
const state1 =toRefs(state)
//让state里面的每一个属性都变成ref类型
```



遍历对象中的所有属性，将其变为响应式数据，这是因为`toRef`只能传一个`key`，`toRefs`所达到的效果与`toRef`一样

## 10）shallowReactive 和 shallowRef



##### 1.reactive 为深度响应式

##### 2.shallowReactive 浅响应式



##### 3.ref为深度响应式

##### 4.shallowRef为浅响应式



#### shallowRef 只进行value的响应式，不进行对象的reactive处理

```js
//什么时候用浅响应式
1.一般情况下使用ref和reactive即可
2.如果有一个对象数据，结构比较深，但变化只是比较外	层属性变换====> shalllowReactive
3.如果一个对象数据，后面会产生新的对象来替换 ===> shallowRef
```



## 11)readonly和shallowReadonly

深度可读和浅度可读



## 12）toRow 和 markRow

##### toRow ->将代理对象变为普通对象

##### markRow ->标记的对象数据从此以后不能再成为代理对象





## 13）customRef 自定义ref

​			可以用于自定义一个ref,可以显式的控制依赖追踪和触发响应，接受一个工厂函数，两个参数分别是用于追踪track 与用于触发响应的trigger，并返回一个带有get 和 set 属性的对象，

- 使用自定义ref实现带防抖功能的v-model



track trigger 延申知识



## 14）inject和provide

依赖注入，实现跨层级，祖孙之间数据传递

```js
provide('color',color)
/***
第一个数据为标识，
第二个参数为数据本身
*/
const color = inject('color')
//接收数据
//直接从祖组件传递到孙子组件
```

## 15） 判断数据

- isRef:检查一个值是否为一个ref对象
- isReactive:检查一个对象是否有reactive创建的响应式数据
- isReadonly:检查一个对象是否由Readonly创建的只读代理
- isProxy:检查一个对象是否由reactive或者Readonly创建的代理

## 16）新组件

1. Fragment（片段）
   - 在vue2中必须有一个根标签
   - 不需要根标签，减少标签层级，减小内存占用
2. Teleport（瞬移）

```js
<Teleport to=''><Teleport>
```

将该标签内的。。。

待补充

### Suspence(不确定的)

运行在渲染异步组件时渲染一些后背内容，可以让我们创建一个平滑的用户体验

vue2动态引用组件

```js
const component =()=>import './...'
//在vue3 中这种写法不行
```

vue3 动态引入组件

```js
import {defineAsyncComponent} from 'vue'
const component =defineAsyncComponent(()=>import './...')
```

### Suspence使用

```js
<Suspence>
    	<template #default>
    	<--异步组件-->
    	</template>

	 	<template v-slot:fallback>
    	<--准备的内容-->
    	</template>
    </Suspence>

// #default 是简写 # = 'v-slot'
// v-slot:fallback 固定写法

```





Vue2 给原型挂载方法

vue3 给原型挂载方法



## Vue3 面试

抱歉:研究没有那么深

2020.9正式版->2022.2.7默认版本->Vue 3 支持vue2大多数特性 - > 新一代构建工具，按需编译，热加载更快 ->  不需要根节点 ，减少内存占用 ->  组合式api ，代替了Vue2,option api,复用性更好，更好的支持Ts -> 深度监视，之前需要set监视数组内部数据变换，现在直接就可以深度监视 -> suspence 更加注重用户体验 ->hook函数 -> 跨级组件通信 ，依赖注入 -> ref实现防抖 -> 可以使用2的生命周期函数，vue3的周期函数更快 -> 响应式数据原理Proxy -> 重写虚拟dom,速度更快 ->新组件瞬移，片段(不需要根节点)
