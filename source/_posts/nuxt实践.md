---



title: nuxt实践

tags: [seo,ssg,vue,nuxt]

author: codefish

date: 2024-08-11 21:55:34

categories: nuxt

top_img: /img/.jpg

cover: /img/12.jpg



---

从使用nuxt开发web门户, 到使用nuxt开发h5页面

###### 1. 为什么使用

​	ssr, ssg , seo 是我看中的地方

###### 2. 为什么h5使用

1. 不用定义路由
2. 加载较快
3. seo较好



### 开发注意

1. **列表渲染涉及到本地图片的地方一定要注意, 否则线上500**

   ```js
   new URL(`~/assets/image/1/a.png`, import.meta.url).href
   ```

2. 打包产物dist里面是软链接, 实践内容在public里面



___







#### nuxt.config.ts配置



##### 配置iconfont

```js
css:[
    "~/assets/iconfont/iconfont.css"
  ],
```



##### env配置

```js
import { loadEnv } from 'vite'
const envName = process.env.npm_lifecycle_script.match(/--mode\s(.*)/)?.[1]
const envData = loadEnv(envName as string, process.cwd()) // 获取.env文件中的配置
Object.assign(process.env,envData)

env: {
    ...Object.assign(process.env,envData)
  },
```

使用: 

```js
const baseURL = import.meta.env.VITE_APP_BASE_URL as string
```



##### hash路由配置

```js
router: {
    mode: 'hash',
    base: '/',
    options: {
      hashMode: true,
    },
  },
```



##### 打包配置

```js
app: {
    baseURL: '/name/',
    pageTransition: { name: 'page' },
    buildAssetsDir: '/dist',
    env: {}
}
```





##### 请求封装

```js
class HttpRequestClass {
  request<T = any>(url: string, method: Methods, data: any, options?: UseFetchOptions<T>, userOptions?:userOptions) {
    return new Promise((resolve, reject) => {
      const newOptions: UseFetchOptions<T> = {
        baseURL: userOptions?.baseUrl ? userOptions?.baseUrl : baseURL,
        method: method,
        ...options,
      }

      if (method === 'GET' || method === 'DELETE') {
        newOptions.params = data?.params
        newOptions.query = Object.assign(newOptions.query as Object, data?.query)
      }
      if (method === 'POST' || method === 'PUT') {
        newOptions.body = data?.body
        newOptions.query = Object.assign(newOptions.query as Object, data?.query)
      }
      useFetch(url, newOptions)
        .then((res) => {
          if (res.error.value) {
            throw new Error(res.error.value.message)
          }
          resolve(res)
        })
        .catch((error) => {
          reject(error)
        })
    })
  }

  // 封装常用方法

  get<T = any>(url: string, params?: any, options?: UseFetchOptions<T>, option?:userOptions) {
    return this.request(url, 'GET', params, options,option)
  }

  post<T = any>(url: string, data: any, options?: UseFetchOptions<T>, option?:userOptions) {
    return this.request(url, 'POST', data, options,option)
  }

  Put<T = any>(url: string, data: any, options?: UseFetchOptions<T>, option?:userOptions) {
    return this.request(url, 'PUT', data, options,option)
  }

  Delete<T = any>(url: string, params: any, options?: UseFetchOptions<T>, option?:userOptions) {
    return this.request(url, 'DELETE', params, options,option)
  }
}
```

此处的参数需要带上上一层

eg: 此处的params: {body: null}, 可以根据需要自行封装

```typescript
export const FriendIndexApi = (params: any) => {
  return httpRequest.get(URL.index, params)
}
```





