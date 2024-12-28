---

title: react
author: codefish
date: 2022-8-10 22:08:07
categories: react
tags: [react redux]
top_img: /img/basketball.jpg
cover: /img/basketball.jpg

---
hexo对于{}处理会有问题

### 1.基础语法

组件必须有一个根节点，组件名字必须大写

**内联样式**

```js

    <div className="arr" style={{ color: "#F40",fontSize:20 }}>
      {arr}
    </div>


```

style如果是对象，要两个{}内嵌，写属性要加引号，fontSize小驼峰

1. 定义虚拟dom时候，不写引号

2. 标签中混入js要用{}

3. jsx即是js和html混用，{}匹配js,<>匹配html

4. 内联用{}内嵌写法

5. 只有一个根标签

6. 标签需要闭合

7. 写一个不存在的标签

   ```js
   写的小写标签当作浏览器标签识别 <good>
   写的大写标签(首字母)当作component组件,如果不存在,报错com is not undefinded
   ```

   如果{}里面是一个数组，可以自动遍历，如果是对象，则出错

   **表达式和代码**

   jsx里面{}只能是一个表达式，不能是js代码

   - arr.map
   - demo(1)
   - const a = function(){}
   - 
   - if
   - for
   - switch



dom.render()

- react解析组件标签，找到了Mycomponent
- 发现组件时使用函数定义的，随后调用改函数，将返回的虚拟DOM转换为真实DOM,然后呈现在页面当中

### 2.两种组件写法

1. 函数式写法

   ```js
   import ARRCOM from "./arr.js";
   import MyClass from "./myclass.js";
   function MyApp() {
     const arr = ["angular", "react", "vue"];
     return (
       <div className="myapp">
         <header className="App-logo">
           {arr.map((item, index) => {
             return (
                 <li key={index}>
                   {index}:{item}
                 </li>
             );
           })}
         </header>
         <ARRCOM></ARRCOM><MyClass></MyClass>
       </div>
     );
   }
   
   export default MyApp;
   
   ```

   

2. 类写法

   ```js
   import React, { Component } from "react";
   
   class MyClass extends Component {
     render() {
       console.log(this);
       return (
         <div>
           <h1>当前求和为：</h1>
           <button>点我+1</button>
         </div>
       );
     }
   }
   export default MyClass;
   
   ```

   

   ### 3.简单/复杂组件

   D:组件是否有状态 - 状态驱动页面

   ps:需要注意

   ```js
   State 的更新可能是异步的
   出于性能考虑，React 可能会把多个 setState() 调用合并成一个调用。
   
   因为 this.props 和 this.state 可能会异步更新，所以你不要依赖他们的值来更新下一个状态。
   ```

   **父子组件传参数和生命周期**

   子组件

   ```js
   import React, { Component } from "react";
   class MyClass extends Component {
     render() {
       console.log(this);
       return (
         <div>
           <h1>Hello, world!</h1>
           <h2>It is {this.props.date.toLocaleTimeString()}</h2>
         </div>
       );
     }
   }
   export default MyClass;
   ```

   

父组件

```js
import { Component } from "react";
import MyClass from "./myclass.js";
class MyApp extends Component {
  constructor(props) {
    super(props);
    this.state = {
      date: new Date(),
    };
  }
  componentDidMount() {
    this.timer = setInterval(() => {
      this.setState({ date: new Date() });
    }, 1000);
  }
  componentWillUnmount() {
    clearInterval(this.timer);
  }
  render() {
    return (
      <div className="myapp">
        <MyClass date={this.state.date}></MyClass>
      </div>
    );
  }
}

export default MyApp;
```

**componentWillUnmount组件销毁时候触发，componentDidMount组件挂载的时候触发**



ps（注意）:

1. state和props时预先定义的，你也可以定义别的
2. 修改state需要使用到setSate构造函数的方法，别的方法修改报错
3. 直接在子组件通过this.props.xxx使用父组件传递的参数
4. 这里的合并是浅合并，所以 `this.setState({comments})` 完整保留了 `this.state.posts`， 但是完全替换了 `this.state.comments`。



Official 阻止默认事件

```js
function ActionLink() {
  function handleClick(e) {
    e.preventDefault();
    console.log('The link was clicked.');
  }

  return (
    <a href="#" onClick={handleClick}>
      Click me
    </a>
  );
}
//e.preventDefault();不需要担心兼容性
```

### 3.state

**存疑**

此处this.handleClick = this.handleClick.bind(this)为es6的public class field，handleClick的this执行什么呢？

不写下面那句话的话，this指向underfinded

```js
constructor(props) {
      super(props);
      this.state = {isToggleOn: true};
  
      // 为了在回调中使用 `this`，这个绑定是必不可少的
      this.handleClick = this.handleClick.bind(this);
    }
  
    handleClick() {
      this.setState(state => ({
        isToggleOn: !state.isToggleOn
      }));
    }
```



如果你没有使用public class fields 语法，你可以在回调中使用[箭头函数](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions)：

```
class LoggingButton extends React.Component {
  handleClick() {
    console.log('this is:', this);
  }

  render() {
    // 此语法确保 `handleClick` 内的 `this` 已被绑定。    return (      <button onClick={() => this.handleClick()}>        Click me
      </button>
    );
  }
}
```

根据逻辑判断进行条件渲染

```js
{unreadMessages.length > 0 &&
        <h2>
          You have {unreadMessages.length} unread messages.
        </h2>
      }
```

之所以能这样做，是因为在 JavaScript 中，`true && expression` 总是会返回 `expression`, 而 `false && expression` 总是会返回 `false`。

类中的方法默认开启严格模式

setState执行的是合并操作，不会把其他属性删除

super(props)的操作，必须在this.xx = xx前面

条件渲染中，当状态改变之后组件重新渲染

##### state的简写方式

```js
this.demo1 =this.demo1.bind(this) 
...
class Person{
    demo1 = demo1.bind(this)
}
## 相当于在person添加一个属性
```

在优化函数写法

```js
class Person {
    //初始化状态
    state = {value:'value'}
    //自定义方法 -- 赋值语句和箭头函数相结合
    changeColor =()=>{
        console.log(this)
    }
}
// 如果写普通函数还是不行，箭头函数没有自己的this,往外找，找到了当前函数的外层，即是Person的实例化对象
```

更新组件的状态 - > 组件重新渲染

### 4.props

**1.props语法糖**

```js
let obj = {name:"a", age:23}
<person age={19}/>
<person {...obj}/>
//只在标签传递有效
```

array.reduce() 求和

```js
function sum(..nums){
	return nums.reduce((preVal,curVal)=>{
		return preVal + curVal
    })
}
```

--标签属性类型的限制和非必须限制--

指定默认值--

poops是只读的，单向数据流

对props进行限制 -**Person.propsTypes**

```js
Person.propTypes = {
    name:React.PropTypes.string
}
//上述被启用 在15版本以后
//16之后单独抽出porp-types
Person.propTypes = {
    name:React.PropTypes.string.isRequired,
}
Person.defaultProps = {
    name:'zhangsan'
}
//isRequired-是否非必须
//defaultProps-默认值
```

**传递方法**

```js
Person.propTypes = {
    speak:React.PropTypes.func.isRequired,
}
//string 区别于类型的String
function speak(){
    console.log('speak')
}
<person speak={speak}/>
// ->类型限制 function - func
```

此种写法写在class外部 - 移除到class内部

```js
class Person extends Component {
    static propTypes = {
        
    }
    static defaultProps = {
        
    }
    render(){
		return ()
    }
}
```

**constructor和super**

完全可以省略，constructor主要是给state初始化，给实例绑定方法，完全可以使用public class field写法替代。如果写了构造器constructor，一起把super加上，否则会出现问题，

```js
constructor(){
	super()
    console.log(this.porps)  // 结果是underfinded
}
```

构造器是否接受props,传递给super,取决于是否需要在constructor通过this拿到props

##### 函数式组件使用props(只能使用props)

```js
function Person(props){
    const {age,name,sex} = props
    return (
    <div>apple</div>
    )
}
Person.propTypes = {}
Person.defaultProps = {}
//props接受所有的传递参数，Object
```

### 5.refs

通过refs拿到dom

```js
class Person extends Component{
    showData = () => {} //fn
    render(){
        return (
            <div></div>
        ...
        )
    }
}
```

**三种写法**

**1.string写法（后续可能被移除**

```js
<input ref="getEle"></input>
this.refs.getEle.focus()
```

tips:提示使用creatRef写法，warning

**2.回调写法**

```js
<input ref={(dom)=>{this.domNode = dom }}></input>
this.domNode.focus()
this.domNode.style.color = "#f40"
//此处的dom即当前节点 <span>ele</span>
```

ps:回调函数的内联函数写法在组件更新时候(state/条件渲染)，会触发两次，第一次获取的domNode为null

--> **改进**class的绑定写法

```js
outFn = e => e //错误写法
outFn = e => { this.input1 = c //domnode绑定给input1}

<input ref={this.outFn}>class绑定语法</div>

this.input1.style.color = "#f40"
```

outFn = e => e

拿到的只有当前这个函数，拿不到dom



**3.createRef**写法

creatRef调用后返回一个容器，改容器可以储存被ref所标识的节点

```js
getEle = React.createRef()
<input ref={getEle}></input>
this.getEle.current.focus()
{current: span}


    alert(this.try.current.value); 			//有效
    this.check.current.color = "#f40"		//无效
    this.check.current.innerHtml = "#f40";  //无效
```

该容器专人专用（相当于原生获取dom)，再次被赋值会被覆盖

**关于onClick和onclick**

```js
所有的原生事件都被二次封装
/1）通过onXXX属性指定事件处理函数（主要大小写
	a>react使用的是自定义（合成）事件，而不是使用的原生的DOM事件， --为了更好的兼容性
    b>react中的事件委托方式处理的（委托给组件最外层的元素）--高效
/2）通过event.target得到发送事件的发生源
```



**jsx的注释不同于其他注释**

```js
{/*...js代码*/}
```



### 6.**高阶函数**

定义：如果一个函数符号下面2个规范中的任何一个，那该函数就是高阶函数

- 若A函数，接受的是一个函数，那么A就可以称之为高阶函数

- 若A函数，调用的返回值依然是一个函数，那么A就可以称之为高阶函数

  

  **函数的柯里化**：通过函数调用继续返回函数的方式，实现多次接收参数最后统一处理的函数编码形式

```js
折叠技巧
//#region endregion#//
```

常见的高阶函数：Promise,  setTimeout , array.map

```js
saveFormData = (dataType)=>{
				return (event)=>{
					this.setState({[dataType]:event.target.value})
				}
			}

			render(){
				return(
					<form onSubmit={this.handleSubmit}>
						用户名：<input onChange={this.saveFormData('username')} type="text" name="username"/>
						密码：<input onChange={this.saveFormData('password')} type="password" name="password"/>
						<button>登录</button>
					</form>
				)
			}
```

**非柯里化写法**

```JS
saveFormData = (dataType,event)=>{
				this.setState({[dataType]:event.target.value})
			}
			render(){
				return(
					<form onSubmit={this.handleSubmit}>
						用户名：<input onChange={event => this.saveFormData('username',event) } type="text" name="username"/>
						密码：<input onChange={event => this.saveFormData('password',event) } type="password" name="password"/>
						<button>登录</button>
					</form>
				)
			}
```

原因：onchange函数是被自动调用的，需要的是一个函数，如果写成this.fn()表示调用的是当前函数的返回值

注意：只要组件state修改，组件就会重新挂载-setState

定时器的使用时机：挂载的时候，开启定时器

**forceUpdate(强制更新**

触发两个钩子

- componentWillUpdate - 
- render - ..
- 不更新任何state强制更新

**componentWillReceiveProps**生命周期

```js
并不会在第一次传递props调用，只会在porps更新时调用
```

1.初始化阶段

```js
1.constructor
2.conponentWillMount()
3.render()
4.componentDidMount()
```

2.更新阶段

```js
1.shouldComponentUpdate() //有一个阀门，必须返回一个boolean，是否继续执行
2.componentWillUpdate()
3.remder() 
4.conponentDidMount()
```

3.销毁阶段

ReactDom.unmountComponentAtNode()触发 --销毁节点

```js
componentWillUnmount()
```

常用钩子：

a.componentDidMount()

```
1.初始化事件
2.网络请求
```

b.componentWillUnmount()

```js
清理定时器
```

c.render()

*bootCDN*



### 7.**新版生命周期**

*新版本可以使用旧的生命周期* - waring-不被推荐使用

UNSAFE_componentWillMount  加入UNSAFE

```js
需要加UNSAFE
UNSAFE_componentWillMount()
UNSAFE_componentWillReceiveProps()
UNSAFE_componentWillUpdate()
```

新版废弃上述三个钩子，新增两个钩子

#### **1.getDerivedStateFromPorps (derived派生)**

```js
getDerivefStateFromPorps() 
返回状态对象，或者null
```

1.返回null

2.返回object

````js
return {count:108}
如果当前state有count,不能修改
````

使用场景：即state取决于props

缺点：代码冗余，定时器难以维护,组件难以维护

#### **2.getSnapshotBeforeUpdate**

--在最近一次渲染输出（提交到 DOM 节点）之前调用。

（快照）*更新之前获取快照* -  任何值都可以为一个快照值

必须返回一个快照，或者null

component

#### **3.diff算法**

遍历列表的时候不要使用inde作为key

PS:

简单：key时虚拟Dom对象的标识

详细：当状态中的数据发生改变时，rract会根据新数据生成新的虚拟都没,随后react进行，新老dom进行diff比较

**比较规则：**

​	a.旧虚拟dom找到了禹新dom相同的key

​		1)如果内容没变，那么直接使用之前的dom,

​		2)如果不同的话，则生成新的dom,替换旧的dom

  b.index作为key,可能会引发的问题、

​		1.索引值被打乱，造成无用的dom更新（效率低，无用的资源浪费

​		2.需求：

#### **4.样式冲突（模块化解决**

后面引入的覆盖之前引用的

less 嵌套

```js
import hello from './a.module.css'
```

这样之后当前css的样式都以对象的形式，包含在hello里面

```jsx
<div class = {hello.title}></div>
```

此时title为类选择器，如果为其他选择器怎么办？

```css
.title{
    background-color:"#fff"
}
```

#### **5.插件使用**

代码块

#### **6.受控组件和非受控组件浅析**

checkout 可以写一个defaultValue，后续defaultChecked的值可以改，如果使用checked,则必须使用onChange来改变checkout选中状态，此处defaultChecked的写法就是非受控组件，而使用onChange函数自己改变状态的，则是受控组件

*解构技巧*

const {...} = this.props

const {...} = this

#### **7.组件传参**

- 父子组件传参

  ```js
  <item todos={todos}> 
  //父组件将其内部的变量 todos传给子组件items,子组件接受到的值为todos
  <item {...todos}>
  //解构写法 - 将todos这个对象的值逐一的传递到子组件
      
  ```

  

- 子传父 

  ```js
  原理：父组件向子组件传递一个函数（通过props)，然后子组件调用当前函数
  ```

  id库

  ```js
  uuid - 唯一id
  nanoid - 比uuid库小，功能一样
  ```

*浅拷贝小tips*

```js
let obj = {a:1,b:2}
let obj1 = {..obj,b:3}
```

todolist案例中更改状态

```js
updateTodo = (id,done) => {
    const {todos} = this.state
    todos.map((todoObj)=>{
        if(todoObj.id === id) return {...todoObj,done}
        else return todoObj
    })
}
```

array.reduce()

#### **8.消息订阅与发布**

订阅与取消订阅

```js
componentDidMount() {
    // publish 发布消息 消息名为：publish_one 内容为：This is publish
    PubSub.subscribe("publish_one", (msg, data) => {
      this.setState({ userData: data });
    });
  }
  //取消消息订阅
  componentWillUnmount(){
    PubSub.unsubscribe("publish_one")
  }
```

发布消息

```js
PubSub.publish("publish_one", data.data.items);
```

PS：注意在需要消息的地方订阅，在传递数据的地方发布消息，记得在组件销毁之前取消订阅

**fetch**

- xhr(传统ajax)
  - jquery
  - axios
- fetch
  - 关注分离(复杂的事情简单化



关注分离具体到发送请求：将请求服务器和拿到数据分开处理

```js
fetch(`url`).then(response=>{
    return response.josn()
}),err=>{
    console.log('connect failed')
    return new Promise()
    //返回一个初始化的promise,获取数据失败，直接返回，不走下一步回调
}.then(()=>{
    console.log('get data success')
},()=>{})
//此处返回的response.json()的为一个promise,then链式调用的为返回的promise,只走成功的或者失败的回调，不会同时走，即下面的一个.then为上面成功或者失败的回调
//使用catch代替失败的回调
fetch(`url`).then(response=>{
    return response.josn()
}).then(()=>{
    console.log('get data success')
},()=>{}).catch((err)=>{
    console.log(err) //统一处理错误
})
//await
try{
    const res = await fetch(`url`)
    const data = await res.json()
}catch(err){
    console.log('请求出错',err)
}
```

fetch使用较少，老版本浏览器不支持fetch请求

### 8.路由（旧版

SPA:单页面应用

- 只有一个页面

- 无刷更新数据

- 数据请求，异步展示

#### 1.react-router-dom 5

```js
//#1
  <React.StrictMode>
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </React.StrictMode>
//#2
<NavLink
          activeClassName="ative"
          className="list-group-item"
          to="/http"
        >
          http
</NavLink>
//#3
<Route path="/http" component={Basic} />
```

路由器分类

- BrowserRouter
- HashRouter   

#### 1.link

```js
<BrowserRouter>
    <linK to='/home'>home</link>
</BrowserRouter>
```

link都需要被router包裹，故在最大的app外面包裹browserRouter

link被转化为A标签，

#### 2.navlink

当前被点击的内容加一个navlink类名（默认，可以加一个activeClassName绑定一个类名

#### 3.switch

```js
<switch>
    <Route path="/about" component={About}></Route>
    </switch>
```

用switch包裹的组件集合，匹配到了第一个就不会往下匹配，

Switch可以提高路由匹配效率(单一匹配)。

#### 4.多级路由的样式更新问题

​        1.public/index.html 中 引入样式时不写 ./ 写 / （常用）

​        2.public/index.html 中 引入样式时不写 ./ 写 %PUBLIC_URL% （常用）

​		3.如果是hasuRouter则没有问题

#### 5.模糊匹配

```js
<NavLink
          activeClassName="ative"
          className="list-group-item"
          to="/http/aa"
        >
          http
</NavLink>
<NavLink
          activeClassName="ative"
          className="list-group-item"
          to="/a/http/aa"
        >
          http
</NavLink>
//#3
<Route path="/http" component={Basic} />
```

当前情况下跳转至/http/aa可以匹配到Basic组件（模糊匹配）,而不能匹配到/a/http/aa

严格匹配不要随便开启，需要再开，有些时候开启会导致无法继续匹配二级路由

**精准匹配**

```js
<Route extra={true} path="/http" component={Basic} />
```

**重定向Redirect**

```js
<switch>
<Route path="/http" component={Basic} />
<Route path="/input" component={Basic} />
<redirect to="/a"/>
    </switch>
```

当http,input都无法匹配的时候，重定向到..

一般把redirect写在所有的路由下方，当所有的都无法匹配时候，redirct---

#### 6.嵌套路由

PS:不能使用严格匹配，会导致路由丢失

- 注册子路由时候，加上父路由的path值
- 路由的匹配是按照注册路由的顺序进行的

#### 7.向路由组件传参

```jsx
<Link to={`/home/message/detail/${msgObj.id}/${msgObj.title}`}>{msgObj.title}</Link>
<Route path="/home/message/detail/:id/:title" component={Detail}/>
				
//detail组件
const {id,title} = this.props.match.params
```

- params传参

  ```js
  <link to='demo/18/zhangsan'/>
  <Route path='demo/:id/name'
  ```

  接受参数: **this.props.match.params**

- search传参

  ```js
  <link to='demo?id=18&zhangsan'/>
  <Route path='demo'/>
  ```

  接受参数：**this.props.location.search**

- state传参

  ```js
  <link to='demo' state={name:"tom"}/>
  <Route path='demo'/>
  ```

  接受参数：**this.props.location.state**

#### 8.编程式导航

```js
this.prosp.history.push()							this.prosp.history.replace()					this.prosp.history.goBack()
this.prosp.history.goForward()						this.prosp.history.go()
//prosP,location...等等只有路由组件才有
```

**BrowserRouter与HashRouter的区别**

- 兼容性
  - 不支持IE9及下
  - 使用的 URL的哈希值
- path
  - 路径没有#
- state的影响
  - browserRouter无影响
  - HashRouter刷新后导致路由state参数的丢失
- 可以解决路径错误问题

**withRouter**

一个函数

```js
export default withRouter(myApp)
```

使一般组件具有路由路由组件特有的api，返回值是一个新组件

#### 9.使用antd组件库

```js
// npm i antd
import { Button } from "antd";
import 'antd/dist/antd.min.css';
```

*注意有的版本导入antd.css会有问题，此处改为antd.min.css*

### 10.redux

[参考阮一峰](https://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_one_basic_usages.html)

*:只要调用setSate，就能更新状态，更新dom

**redux**：只能管理状态，但不会触发更新

```js
componentDidMount(){
		//检测redux中状态的变化，只要变化，就调用render
		store.subscribe(()=>{
			this.setState({}) //幌一下
		})
}
```

setState存在效率问题，此处是一个**小技巧**

#### 1.监听单个组件

redux 文件夹下

*1.store.js*

```js
import {createStore} from 'redux'
import countReducer from './count_reducer'
export default createStore(countReducer)
```

2.count_reducer

```js
const initState = 0 //初始化状态
export default function countReducer(preState=initState,action){
	//从action对象中获取：type、data
	const {type,data} = action
	//根据type决定如何加工数据
	switch (type) {
		case 'increment': //如果是加
			return preState + data
		case 'decrement': //若果是减
			return preState - data
		default:
			return preState
	}
}
```

第一次调用的时候，preState为underfinded,因此需要赋一个默认值

3.use.js 使用store

```js
componentDidMount(){
		//检测redux中状态的变化，只要变化，就调用render
		store.subscribe(()=>{
			this.setState({})
		})
}
<select ref={c => this.selectNumber = c}>
					<option value="1">1</option>
					<option value="2">2</option>
					<option value="3">3</option>
</select>
import store from '../../redux/store'
fn = ()=>{
	const {value} = this.selectNumber
    store.dispatch({type:'increment',data:value*1})
}
```

**此处一个重要的点就是监听store状态，只要改变了，就触发setState更新视图**，可以将监听放在index.js里面，进行全局监听

```js
store.subscribe(()=>{
ReactDom.render(<APP/>,document.getElementById('root'))
})
```

此处有diff算法兜底，不会造成大未改变页面更新

**pnpm使用**

使用 `pnpm install` 安装项目依赖时出现：`ERR_PNPM_PEER_DEP_ISSUES Unmet peer dependencies`

问题原因：在 npm 3 中，不会再强制安装 peerDependencies （对等依赖）中所指定的包，而是通过警告的方式来提示我们。pnpm 会在全局缓存已经下载过的依赖包，如果全局缓存的依赖版本与项目 package.json 中指定的版本不一致，就会出现这种 hint 警告。

**处理方案**

1. 在项目的 `package.json` 中配置 `peerDependencyRules` 忽略对应的警告提示：

   ```js
   "pnpm": {
       "peerDependencyRules": {
         "ignoreMissing": [
           "react"
         ]
       }
     }
   ```

   

2. 在 `.npmrc` 配置文件中添加 `strict-peer-dependencies=false` ，这意味着将关闭严格的对等依赖模式。操作命令如下：

   ```js
   npm config set strict-peer-dependencies=false
   ```

   

**箭头函数补充**

```js
data => 1   //返回1
data => {} //不会返回空对象，会把里面当作函数体
-解决
data => ({})
```

