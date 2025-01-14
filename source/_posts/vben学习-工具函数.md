---



title: vben-admin学习1

tags: [vben-admin, vue, typescript]

author: codefish

date: 2025-01-14 23:20:34

categories: vben-admin

top_img: /img/2.jpg

cover: /img/2.jpg



---

学习项目 [vue-vben-admin](https://github.com/vbenjs/vue-vben-admin )


___

### 1. bindMethods

```typescript
export function bindMethods<T extends object>(instance: T): void {
    const prototype = Object.getPrototypeOf(instance); //获取属性 返回对象
    const propertyNames = Object.getOwnPropertyNames(prototype); //获取属性名 返回数组
  
    propertyNames.forEach((propertyName) => {
      const descriptor = Object.getOwnPropertyDescriptor(prototype, propertyName); //获取属性描述符
      const propertyValue = instance[propertyName as keyof T]; //获取属性值
  
      if (
        typeof propertyValue === 'function' &&
        propertyName !== 'constructor' &&
        descriptor &&
        !descriptor.get &&
        !descriptor.set
      ) {
        instance[propertyName as keyof T] = propertyValue.bind(instance);
      }
    });
  }
```

这个函数 bindMethods 的作用是将一个对象实例的方法绑定到该实例上。这样做的目的是确保在调用这些方法时，this 始终指向该实例，而不是其他上下文。

以下是这个函数的工作原理的详细解释：

1. 获取原型：Object.getPrototypeOf(instance) 获取传入对象实例的原型。这是因为方法通常定义在类的原型上。
2. 获取属性名：Object.getOwnPropertyNames(prototype) 获取原型上的所有属性名（包括方法名）。

遍历属性名：对每个属性名进行遍历。

4. 获取属性描述符和属性值：

Object.getOwnPropertyDescriptor(prototype, propertyName) 获取属性的描述符。

instance[propertyName as keyof T] 获取属性值。

5. 绑定方法：

**目的是确保只对普通方法进行绑定。普通方法是指那些定义在类的原型上、没有 getter 和 setter 的函数。**

检查属性值是否为函数。

确保属性名不是 constructor。

确保属性没有 getter 和 setter。

如果满足以上条件，则使用 Function.prototype.bind 将方法绑定到实例上。

通过这种方式，确保了在调用这些方法时，this 始终指向该实例，而不是其他上下文。这在事件处理或回调函数中非常有用，因为在这些情况下，this 的指向可能会发生变化。



**什么时候this 不指向该实例, 是其他上下文**

在 JavaScript 中，this 的指向取决于函数的调用方式，而不是函数定义的位置。以下是一些常见的情况下，this 可能不指向预期的实例：

1. 全局调用：

如果一个函数在全局作用域中被调用，this 通常指向全局对象（在浏览器中是 window，在 Node.js 中是 global）。在严格模式下，this 会是 undefined。

事件处理函数：

当一个方法作为事件处理函数时，this 通常指向触发事件的 DOM 元素，而不是定义该方法的对象。

3. 回调函数：

当一个方法作为回调函数传递时，this 的指向可能会丢失。例如，在数组的 forEach 方法中传递一个方法作为回调，this 不会自动指向原始对象。

4. 定时器函数：

在 setTimeout 或 setInterval 中调用的函数，this 默认指向全局对象。

5. 方法赋值给变量：

如果将一个对象的方法赋值给一个变量，然后通过该变量调用方法，this 不会指向原始对象。

箭头函数：

箭头函数不绑定自己的 this，它会捕获其所在上下文的 this 值。因此，在箭头函数中，this 的指向是由其外层作用域决定的。