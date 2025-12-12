# 概述

Axios 是一个网络请求库。

Axios 的安装命令为:

```bash
npm install axios
```



参考的文档:

Axios 官方中文中文文档: https://axios-http.com/zh/docs/intro



# 发送简单的 GET 和 POST 请求

示例如下:

```tsx
import axios from 'axios'

const getUser = async () => {
  try {
    const response1 = await axios.get('http://localhost:3000/user?ID=12345')
    const response2 = await axios.get('http:////localhost:3000/user', {  // 这与上面的请求方法一致
      params: { ID: 12345 }
    })
    const response3 = await axios.post('http://localhost:3000/user', {
      firstName: 'Mu',
      lastName: 'Annan'
    })
  } catch(error) {
   	console.error(error) 
  }
}
```

实际上的请求方式为

```tsx
axios({
  method: 'post',
  url: '/user/12345',
  data: {
    firstName: 'Fred',
    lastName: 'Flintstone'
  }
})
```

而 `axios.get` 等只是一个别名（为了方便）





# 创建实例

```tsx
const instance = axios.create({
  baseURL: 'https://some-domain.com/api/',
  timeout: 1000,  // 等待1秒
  headers: {'X-Custom-Header': 'foobar'}
})
```

创建完该实例后，实例方法具有 `get` `post` `request` 等

创建实例，一些东西就不用重复配置了

request 可以配置很多包括 method 的项，而 get post使得可以省略包括 method 的一些项



创建实例后还可以继续添加和修改值

即使使用该实例时 get post 时仍然可以重写相关的东西





# 响应结构

```json
{
  // `data` 由服务器提供的响应
  data: {},

  // `status` 来自服务器响应的 HTTP 状态码
  status: 200,

  // `statusText` 来自服务器响应的 HTTP 状态信息
  statusText: 'OK',

  // `headers` 是服务器响应头
  // 所有的 header 名称都是小写，而且可以使用方括号语法访问
  // 例如: `response.headers['content-type']`
  headers: {},

  // `config` 是 `axios` 请求的配置信息
  config: {},

  // `request` 是生成此响应的请求
  // 在node.js中它是最后一个ClientRequest实例 (in redirects)，
  // 在浏览器中则是 XMLHttpRequest 实例
  request: {}
}
```















# 先前的笔记

Axios 是一款基于 promise 的 HTTP 客户端，用于浏览器和 Node.js 环境中，其本身是用 XMLHttpRequests 写的。

与 fetch 相比，Axios 兼容性好，拥有内置的请求和响应拦截器，处理复杂请求时代码更加简洁。

npm 安装指令为：

```shell
npm install axios
```



```js
import axios from 'axios'

axios.get('/user?ID=12345')
  .then(response => {
    // handle success
  })
  .catch(error => {
    //handle error
  })
  .finally(() => {
    // always executed
  })

axios.post('https://api.example.com/data', {
    key1: 'value1',
    key2: 'value2'
  })
	.then(response => {})
	.catch(error => {})
```







# 拦截器

拦截器在请求发出前，和响应处理前做的事项

```tsx
// 添加请求拦截器
axios.interceptors.request.use(config => {
    // 在发送请求之前做些什么
    return config;
  }, function (error) {
    // 对请求错误做些什么
    return Promise.reject(error);
  });

// 添加响应拦截器
axios.interceptors.response.use(response => {
    // 2xx 范围内的状态码都会触发该函数。
    // 对响应数据做点什么
    return response;
  }, error => {
    // 超出 2xx 范围的状态码都会触发该函数。
    // 对响应错误做点什么
    return Promise.reject(error);
  });
```

自定义的 axios 实例也可以添加拦截器