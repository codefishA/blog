---



title: vben-全局设定&class解读

tags: [vben-admin, vue, typescript]

author: codefish

date: 2025-01-13 22:12:34

categories: vben-admin

top_img: /img/7.jpg

cover: /img/7.jpg



---





学习项目 [vue-vben-admin](https://github.com/vbenjs/vue-vben-admin )

### 1.preferencesManager

外观管理器

### 2.StorageManager

缓存管理器, 状态管理器

### 3.GlobalShareState

全局共享状态

```js
/**
 * 全局复用的变量、组件、配置，各个模块之间共享
 * 通过单例模式实现,单例必须注意不受请求影响，例如用户信息这些需要根据请求获取的。后续如果有ssr需求，也不会影响
 */
```



### 4.StateHandler

异步等待异步, 状态处理器

#### Eg1: 

```typescript
private async getForm() {
    if (!this.isMounted) {
      // 等待form挂载
      await this.stateHandler.waitForCondition();
    }
    if (!this.form?.meta) {
      throw new Error('<VbenForm /> is not mounted');
    }
    return this.form;
  }
```

`此处用于在form组件未挂载前不返回form属性`

**何时可以返回form属性**

```typescript
mount(formActions: FormActions) {
    if (!this.isMounted) {
      Object.assign(this.form, formActions);
      this.stateHandler.setConditionTrue();
      this.setLatestSubmissionValues({
        ...toRaw(this.handleRangeTimeValue(this.form.values)),
      });
      this.isMounted = true;
    }
  }
```

总的来说就是, 在组件没有被手动mount时, 调用getForm不会被返回



#### Eg2:

**举个例子**

我现在封装了一个echarts类, 这里面图表的渲染依赖于异步的接口请求,正常的实现思路, `const myPie = echrts.init(dom)`, 在数据获取之后, 使用myPie.setOptions(), 但是现在我想要我的代码简介一些, 那么我就可以使用`await this.stateHandler.waitForCondition();`

如下: 

```typesctipt
import { StateHandler } from './state-handler'; // 假设 StateHandler 在这个路径

class EChartsHandler {
  private chart: any; // 你的 echarts 实例
  private stateHandler: StateHandler;
	private data;
  constructor() {
    this.stateHandler = new StateHandler();
  }

  async loadData() {
    try {
      const data = await fetchDataFromServer(); // 异步获取数据
      this.setData(data);
      this.stateHandler.setConditionTrue(); // 数据加载完成，设置条件为 true
    } catch (error) {
      console.error('数据加载失败', error);
      this.stateHandler.setConditionFalse(); // 数据加载失败，设置条件为 false
    }
  }

  async renderChart() {
    await this.stateHandler.waitForCondition(); // 等待数据加载完成
    this.chart = echarts.init(document.getElementById('chart'));
    this.chart.setOption(this.getChartOptions());
  }

  setData(data: any) {
    // 设置数据
  }

  getChartOptions() {
    // 返回图表配置
    return {
    	...,
    	data: this.data
    }
  }
}
```

**这么一看, 这东西和promise.all 或者 promise.then()的差异点在哪里呢**



#### Eg3: 

看下面一个例子

```typescript
import { StateHandler } from './state-handler'; // 假设 StateHandler 在这个路径

class ComplexDataHandler {
  private stateHandler: StateHandler;
  private dataSources: Array<Promise<any>>;
  private chart: any;

  constructor() {
    this.stateHandler = new StateHandler();
    this.dataSources = [];
  }

  async loadData() {
    try {
      // 模拟从多个数据源异步获取数据
      const dataSource1 = fetchDataFromSource1();
      const dataSource2 = fetchDataFromSource2();
      const dataSource3 = fetchDataFromSource3();

      this.dataSources = [dataSource1, dataSource2, dataSource3];

      // 等待所有数据源完成
      const results = await Promise.all(this.dataSources);

      // 设置数据并标记状态为已准备好
      this.setData(results);
      this.stateHandler.setConditionTrue();
    } catch (error) {
      console.error('数据加载失败', error);
      this.stateHandler.setConditionFalse();
    }
  }

  async renderChart() {
    // 等待数据加载完成
    await this.stateHandler.waitForCondition();

    if (this.stateHandler.isConditionTrue()) {
      this.chart = echarts.init(document.getElementById('chart'));
      this.chart.setOption(this.getChartOptions());
    } else {
      console.error('无法渲染图表，因为数据加载失败');
    }
  }

  setData(data: any[]) {
    // 处理并设置数据
  }

  getChartOptions() {
    // 返回图表配置
  }

  cancelLoading() {
    // 用户取消操作时，清理状态
    this.stateHandler.reset();
    console.log('数据加载已取消');
  }
}

// 使用示例
const dataHandler = new ComplexDataHandler();
dataHandler.loadData();
dataHandler.renderChart();

// 用户取消操作
dataHandler.cancelLoading();
```

看完这个例子, 我还是觉得和promise无区别, 用promist.all()套一下三个请求, 还是一模一样



#### eg4:

在许多情况下，Promise.all() 确实可以实现类似的功能，特别是在等待多个异步操作完成时。然而，StateHandler 提供了一些额外的功能和灵活性，适用于更复杂的状态管理场景。以下是一些可能的区别和 StateHandler 的优势：

##### 1. 状态管理的灵活性

状态变化的控制：StateHandler 提供了对状态变化的更细粒度控制。你可以在状态变化时执行特定的逻辑，而不仅仅是等待所有异步操作完成。例如，你可以在某个条件满足时触发特定的回调。

状态重置和清理：StateHandler 提供了重置和清理状态的功能，这在需要多次重复使用状态管理逻辑时非常有用。

##### 2. 处理复杂的状态依赖

条件等待：StateHandler 可以用于等待特定的状态条件，而不仅仅是等待异步操作完成。这在需要根据不同条件执行不同逻辑的场景中非常有用。

异步操作的取消和恢复：在某些情况下，你可能需要在异步操作进行中取消或恢复操作。StateHandler 可以帮助管理这些状态变化。

##### 3. 错误处理和恢复

更好的错误处理：虽然 Promise.all() 会在任何一个 Promise 被 reject 时立即 reject，但 StateHandler 可以提供更细致的错误处理和恢复机制。

##### 示例场景

假设你有一个应用，需要从多个数据源获取数据，并在数据加载过程中允许**用户取消操作或在失败时重试。**在这种情况下，StateHandler 可以帮助你管理这些复杂的状态变化和用户交互。**并且他节约了服务器资源, 避免了后面无用的请求**

```typescript
class DataLoader {
  private stateHandler: StateHandler;

  constructor() {
    this.stateHandler = new StateHandler();
  }

  async loadData() {
    try {
      const dataSources = [fetchData1(), fetchData2(), fetchData3()];
      const results = await Promise.all(dataSources);
      this.stateHandler.setConditionTrue(); // 数据加载成功
      return results;
    } catch (error) {
      this.stateHandler.setConditionFalse(); // 数据加载失败
      throw error;
    }
  }

  async render() {
    await this.stateHandler.waitForCondition();
    if (this.stateHandler.isConditionTrue()) {
      // 渲染逻辑
    } else {
      // 错误处理逻辑
    }
  }

  cancel() {
    this.stateHandler.reset(); // 用户取消操作
  }
}
```

##### 可以获得的信息

1. 数据加载状态：

如果 isConditionTrue() 返回 true，你可以确定数据已经成功加载并准备好用于渲染。

2. 错误或取消状态：

如果 isConditionTrue() 返回 false，你可以推断出数据加载过程中出现了问题，或者用户取消了加载操作。

3. 下一步操作：

根据状态的真假，你可以决定是继续进行渲染，还是执行错误处理或其他补救措施。

通过这种方式，StateHandler 提供了一种简单而有效的机制来管理异步操作的状态和条件，帮助你在复杂的应用场景中做出正确的逻辑决策。



#### 总结: 

​		就我个人理解, 此处主要是封装了一个状态管理工具, 功能和promise.then类似, 但是多了取消操作 , 在面对复杂场景可能有用武之地(目前还在探究中), 此处我目前把他理解为**手搓promise, 增加取消功能**