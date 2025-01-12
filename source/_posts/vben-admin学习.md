---



title: vben-admin学习1

tags: [vben-admin, vue, typescript]

author: codefish

date: 2025-01-09 23:20:34

categories: vben-admin

top_img: /img/9.jpg

cover: /img/9.jpg



---

学习项目 [vue-vben-admin](https://github.com/vbenjs/vue-vben-admin )


___



## 1.初始化

```js
//(package.json/.node-version有写)
nvm use 20 

pnpm i 

pnpm run dev
```

疑问点: 选择ui框架是怎么实现的 ?

## 2. 选择了ant-design

###### 

### 1.main.ts

```typescript
import { initPreferences } from '@vben/preferences';
import { unmountGlobalLoading } from '@vben/utils';

import { overridesPreferences } from './preferences';

/**
 * 应用初始化完成之后再进行页面加载渲染
 */
async function initApplication() {
  // name用于指定项目唯一标识
  // 用于区分不同项目的偏好设置以及存储数据的key前缀以及其他一些需要隔离的数据
  const env = import.meta.env.PROD ? 'prod' : 'dev';
  const appVersion = import.meta.env.VITE_APP_VERSION;
  const namespace = `${import.meta.env.VITE_APP_NAMESPACE}-${appVersion}-${env}`;

  // app偏好设置初始化
  await initPreferences({
    namespace,
    overrides: overridesPreferences,
  });

  // 启动应用并挂载
  // vue应用主要逻辑及视图
  const { bootstrap } = await import('./bootstrap');
  await bootstrap(namespace);

  // 移除并销毁loading
  unmountGlobalLoading();
}

initApplication();

```

#### 简单解读

1. 根据不同环境, 项目nameSpace, 项目版本, 三个参数拼接了项目唯一编码
2. 等待外观初始化,  传人第一步拼接的标识, 和覆盖的外观参数(覆盖默认外观参数)
3. 导入bootstrap, 并等待初始化
4. 销毁loading



#### 疑问点

1. loading在哪初始化
2. bootstrap做了哪些工作



#### 继续探究

##### 1. 外观加载

- initPreferences
- overridesPreferences



###### 1.initPreferences 

声明

```typescript
(alias) const initPreferences: ({ namespace, overrides }: 
```

文件路径

<u>packages/@core/preferences/src/index.ts</u>

```typescript
// 初始化偏好设置
const initPreferences =
  preferencesManager.initPreferences.bind(preferencesManager);
```

即这里调用了preferencesManager.initPreferences



**回头来看初始化外观配置**

```js
public async initPreferences({ namespace, overrides }: InitialOptions) {
    // 是否初始化过
    if (this.isInitialized) {
      return;
    }
    // 初始化存储管理器
    this.cache = new StorageManager({ prefix: namespace });
    // 合并初始偏好设置
    this.initialPreferences = merge({}, overrides, defaultPreferences);

    // 加载并合并当前存储的偏好设置
    const mergedPreference = merge(
      {},
      // overrides,
      this.loadCachedPreferences() || {},
      this.initialPreferences,
    );

    // 更新偏好设置
    this.updatePreferences(mergedPreference);

    this.setupWatcher();

    this.initPlatform();
    // 标记为已初始化
    this.isInitialized = true;
  }
```

**解读**

注释已经很清楚,  唯一疑惑的地方是initialPreferences存储之后 , 先获取了一遍存储器的配置, 再将初始化配置覆盖其他配置



- 此处的merge函数问了下老g: 解读如下

- this.initialPreferences = merge({}, overrides, defaultPreferences); 时，合并的结果将遵循以下逻辑：

  合并顺序：

  从一个空对象 {} 开始。

  依次合并 overrides 和 defaultPreferences。

  2. 数组覆盖逻辑：

  如果 overrides 和 defaultPreferences 中的某个键对应的值都是数组，那么 overrides 中的数组将直接覆盖 defaultPreferences 中的数组。

  结果：

  this.initialPreferences 将是一个合并后的对象。

  对于非数组的键，overrides 中的值将覆盖 defaultPreferences 中的值。

  对于数组的键，overrides 中的数组将直接替换 defaultPreferences 中的数组。

  因此，this.initialPreferences 的结果是一个合并后的对象，优先使用 overrides 中的值，特别是数组类型的值会直接覆盖。







###### 2.preferencesManager类

接下来找到preferencesManager这个类, 部分声明如下

```js
class PreferenceManager {
  private cache: null | StorageManager = null;
  // private flattenedState: Flatten<Preferences>;
  private initialPreferences: Preferences = defaultPreferences;
  private isInitialized: boolean = false;
  private savePreferences: (preference: Preferences) => void;
  private state: Preferences = reactive<Preferences>({
    ...this.loadPreferences(),
  });
  constructor() {
    this.cache = new StorageManager();

    // 避免频繁的操作缓存
    this.savePreferences = useDebounceFn(
      (preference: Preferences) => this._savePreferences(preference),
      150,
    );
  }

  clearCache() {
    [STORAGE_KEY, STORAGE_KEY_LOCALE, STORAGE_KEY_THEME].forEach((key) => {
      this.cache?.removeItem(key);
    });
  }
  ...
}
```





不难猜出上述几个属性的意义

- private cache: null | StorageManager = null;   **自身缓存**
- private initialPreferences: Preferences = defaultPreferences; **默认外观配置**
- private isInitialized: boolean = false;  **是否已经初始化**
- private savePreferences: (preference: Preferences) => void; **报错外观方法**
- private state: Preferences = reactive<Preferences>({ 
      ...this.loadPreferences(),
    });  **外观配置, 并且初始化配置**



- loadPreferences

```typescript
private loadPreferences(): Preferences {
    return this.loadCachedPreferences() || { ...defaultPreferences };
}
```

优先获取缓存的配置, 如果缓存配置没有则初始化默认配置



- constructor

  - 初始化了一个缓存类

  - 给savePreferences增加防抖

- clearCache 

  一个非公有或者私有方法, 用于清除缓存



##### 2.bootstrap初始化

挂载vue在节点上, 注册需要用的实例对象

###### 1.initComponentAdapter

注册全局组件

- 声明要注册的组件list

```js
const components: Partial<Record<ComponentType, Component>> ={}
```

- *将组件注册到全局共享状态中*

  globalShareState**.**setComponents(components)**;**

  ```js
  globalShareState.defineMessage({
      // 复制成功消息提示
      copyPreferencesSuccess: (title, content) => {
        notification.success({
          description: content,
          message: title,
          placement: 'bottomRight',
        });
      },
    });
  ```

  

- 1





##### 3.createApp - setupI18n

加载多语言

```typescript
async function setupI18n(app: App, options: LocaleSetupOptions = {}) {
  await coreSetup(app, {
    defaultLocale: preferences.app.locale,
    loadMessages,
    missingWarn: !import.meta.env.PROD,
    ...options,
  });
}
```

从preferences里面读取语言en/cn,  loadMessage加载语言, missingWarn线上版本不警告, 注意逻辑就是**loadMessages**



###### 1. loadMessages

- 加载系统反应文件
- 加载第三方文件

```typescript
async function loadMessages(lang: SupportedLanguagesType) {
  const [appLocaleMessages] = await Promise.all([
    localesMap[lang]?.(),
    loadThirdPartyMessage(lang),
  ]);
  return appLocaleMessages?.default;
}
```



加载系统文件,  加载`/\.\/langs\/([**^**/]**+**)\/(.*****)\.json**$**/`下的所有语言文件, 此处使用了一个方法**loadLocalesMapFromDir**

加载第三方文件, 使用loadThirdPartyMessage,  里面分别加载antD和day.js的locales文件



##### 4. initStores 初始化pinia



##### 5. 其他

###### 1.安装权限指令

###### 2.配置路由及路由守卫

###### 3.配置@tanstack/vue-query

`@tanstack/vue-query` 非常适合以下场景：

- 需要频繁与后端 API 交互的应用。
- 需要缓存数据以减少重复请求的应用。
- 需要实时更新数据的应用（如仪表盘、实时通知等）。
- 需要处理复杂数据加载逻辑的应用（如分页、无限加载等）。

###### 4.动态更新标题

```js
// 配置 pinia-tore
  await initStores(app, { namespace });

  // 安装权限指令
  registerAccessDirective(app);

  // 配置路由及路由守卫
  app.use(router);

  // 配置@tanstack/vue-query
  app.use(VueQueryPlugin);

  // 动态更新标题
  watchEffect(() => {
    if (preferences.app.dynamicTitle) {
      const routeTitle = router.currentRoute.value.meta?.title;
      const pageTitle =
        (routeTitle ? `${$t(routeTitle)} - ` : '') + preferences.app.name;
      useTitle(pageTitle);
    }
  });
```



















