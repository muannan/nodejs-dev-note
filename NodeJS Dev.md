# 概述

node.js 是一个开源和跨平台的 JavaScript 运行环境。

与浏览器端运行 JS 环境类似，浏览器端是 JS + web API，而 node.js 则是 JS + node API。 



安装方法: 直接在 [Node.js 官网](https://nodejs.org/en) 下载最新 LTC 版本安装包，傻瓜式安装即可

运行JS文件: 使用 `node` 命令可以直接运行 JS 文件(文件中可使用 node API)，例如 `node <file-address>`



参考的文档和视频：

node.js 官方文档: https://nodejs.org/en/learn/getting-started/introduction-to-nodejs

尚硅谷Node.js零基础视频教程（BiliBili）: https://www.bilibili.com/video/BV1gM411W7ex?p=181&spm_id_from=pageDriver&vd_source=9ac83b3ca683e3dcf6e29152632646e8



windows 安装完 node 后，输入 npm 会被禁用

输入指令 `set-ExecutionPolicy RemoteSigned` 就好了









# 使用 npm 管理项目的依赖

`npm` (Node Package Manager) 是 Node.js 的标准包管理器，用来管理项目的依赖。

安装好 Node.js 后就可以直接使用 npm，命令 `npm -v` 可以查看 npm 的版本号：

```bash
npm -v
```



参考的文档：

node.js 官方文档（An introduction to the npm package manager）: https://nodejs.org/en/learn/getting-started/an-introduction-to-the-npm-package-manager



## 初始化一个项目的依赖

在该项目文件夹中，使用以下命令：

```bash
npm init
```

其作用是交互式的创建 package.json 文件。

该文件是依赖的配置文件，里面会写明项目依赖的包和版本号。



## 安装依赖包

向项目中安装依赖包的命令如下：

```bash
npm install <package-name>
```

该命令会将对应的包下载到 `node_moudile` 文件夹中。

自 npm5 以来，这个命令也会将 `<package-name>` 添加到 `package.json` 文件中的 `dependencies` 中。而在 npm5 之前到版本，需要在后面额外标记 `--save` 才能做到。



当接手一个别人的项目时，通常项目中会有一个 `package.json` 文件，此时要运行下述命令:

```bash
npm install
```

这样就可以将项目所依赖的包（package.json 文件下的 `dependencies` 和 `devDependenies`）都下载到 `node_modules` 文件夹里

通常向别人发送整个项目，或者将项目提交到 github，会忽略 `node_modules` 文件夹发送或上传，因为该文件夹通常体积很大且重复，不便于传输。



## 使用 npm 运行项目

可以直接用 npm 来运行整个项目，命令如下：

```bash
npm run <task-name>
```

`<task-name>` 对应 `package.json` 文件夹下的 `scripts` 子项，例如：

```json
{
  "scripts": {
    "start": "node ./bin/www",
    "start-dev": "node lib/server-production"
  }
}
```

按照上面的示例，可以直接运行如下命令：

```bash
npm run start
```

也可以简写为：

```bash
npm start
```



## npx

npx 是 npm 的一部分。

一些依赖包全局安装才能生效。使用 npx，无需全局安装某个 npm 包就可以直接运行它。







# 使用 node:fs 模块操作文件

`node:fs` 是 Node.js 提供的文件系统（File System）模块，用于文件和目录进行交互，比如读取文件、写入文件、修改权限、创建目录等操作。

之前该功能由 `fs` 模块负责，现在更新为了 `node:fs` 模块。

并且 Node.js 还提供了基于 Promise 的文件系统模块 `node:fs/promise` ，这是更加现代的方式。

这里对 `node:fs/promise` 模块的使用方式进行讲解。

```js
import { readFile, writeFile } from "node:fs/promises"

const run = async () => {
  const data = await readFile('example.txt', 'utf8')
  console.log(data)
  
  await writeFile('example.txt', 'New content')
}
run()
```



`filehandle` 是 Node.js fs 中提供的一种用于操作文件的抽象对象。

使用 fsPromise 的 `open()` 方法来打开一个文件，并获得该文件的 `filehandle` 对象。

通过 `filehandle` 提供了一些方法，比如 `read()` `write()` `close()` 等，用于读取、写入或关闭文件。也可以使用它来查询文件的属性（如文件大小、创建时间）或设置文件属性（如修改文件的读写权限等） 







# Buffer

在 Node.js 中，Buffer 是一个用于处理**二进制数据**的全局类，特别是在处理文件、网络数据流等场景中非常常用。JavaScript 原生只支持字符串处理 Unicode 数据，不擅长处理二进制数据，因此 Node.js 提供了 Buffer 来补这个短板。

Buffer 数据 转换为 JS 字符串

```js
let buf = Buffer.from([105, 108, 111, 118, 101, 121, 111, 117])
console.log( buf.toString() )  // 默认以 UTF-8 的编码方法进行转换
```







# nodejs http API

```js
const http = require('http')

// 创建服务对象
const server = http.createServier((request, response) => {
  response.end('hello')  // 设置响应体，并结束响应
})

// 监听端口，启动服务
server.listen(9000, () => {
  console.log('服务已经启动。。。')
})
```



server.listen 的回调函数在启动成功时调用



本地IP: 127.0.0.1



## 设置HTTP响应报文

response.statusCode 设置响应状态码

response.setHeader 设置响应头信息

response.write('xx') 设置响应体

response.end('') 每一个请求，在处理的时候必须要执行 end 方法





## HTTP响应文件

```js
const http = require('http')
const fs = require('fs')

// 创建服务对象
const server = http.createServier((request, response) => {
  let html = fs.readFileSync(__dirname + '/index.html')
  response.end(html)  // 设置响应体，并结束响应
})

// 监听端口，启动服务
server.listen(9000, () => {
  console.log('服务已经启动。。。')
})
```





## HTTP 实现网页引入外部资源

```js
const http = require('http')
const fs = require('fs')

// 创建服务对象
const server = http.createServier((request, response) => {
  
  // 获取请求 url 的路径
  let {pathname} = new URL(request.url, 'http://127.0.0.1')
  if(pathname === '/'){
  	let html = fs.readFileSync(__dirname + '/index.html')
  	response.end(html)  // 设置响应体，并结束响应	
  }else if(pathname === '/style.css'){
    let css = fs.readFileSync(__dirname + '/style.css')
  	response.end(html)	
  }else if(pathname === '/JavaScript.js'){
    let js = fs.readFileSync(__dirname + '/JavaScript.js')
  	response.end(js)
  }else{
    response.statusCode = 404
    response.end('<h1>404 Not Found<h1>')
  }
})

// 监听端口，启动服务
server.listen(9000, () => {
  console.log('服务已经启动。。。')
})
```



## HTTP搭建静态资源服务并简化引用代码

```js
const http = require('http')
const fs = require('fs')

// 创建服务对象
const server = http.createServier((request, response) => {
  
  // 获取请求 url 的路径
  let {pathname} = new URL(request.url, 'http://127.0.0.1')
  let filePath = __direname + '/page' + pathname
  fs.readFile(filePath, (error, data) => {
    if(error){
      response.statusCode = 500
      response.end('文件读取失败')
      return
    }
    response.end(data)
  })
})

// 监听端口，启动服务
server.listen(9000, () => {
  console.log('服务已经启动。。。')
})
```



## 绝对路径

/web 与页面URL的协议、主机名、端口拼接形成完成的URL再发送请求 





## mime 类型

mime类型是一种标准，用来表示文档、文件或字节的性质和格式

例如：text/html  text/css  image/jpeg  application/json

HTTP服务可以设置响应头 Content-Type 来表明响应体的 MIME 类型，浏览器会根据该类型决定如何处理资源

```js
const http = require('http')
const fs = require('fs')
const path = require('path')
let mimes = {
  html: 'text/html',
  css: 'text/css',
  js: 'text/javascript',
  png: 'image/png'
  ...
}

// 创建服务对象
const server = http.createServier((request, response) => {
  
  // 获取请求 url 的路径
  let {pathname} = new URL(request.url, 'http://127.0.0.1')
  let filePath = __direname + '/page' + pathname
  fs.readFile(filePath, (error, data) => {
    if(error){
      response.statusCode = 500
      response.end('文件读取失败')
      return
    }
    // 获取文件的后缀名
    let ext = path.extname(filePath).slice(1)
    // 获取对应的MIME类型
    let type = mimes[ext]
    if(type){
      if(ext === 'html'){
        response.setHeader('content-type', type + ';charset = utf-8')
      }else{
        response.setHeader('content-type', type)
      }
    }else{
      response.setHeader('content-type', 'application/octet-stream')
    }
    response.end(data)
  })
})

// 监听端口，启动服务
server.listen(9000, () => {
  console.log('服务已经启动。。。')
})
```















# 模块化

## 函数暴露调用

模块文件 me.js

```js
function tiemo(){
  console.log("贴膜")
}

module.exports = tiemo
```

or

```js
module.exports = () => {
  console.log("贴膜")
}
```







主文件

```js
const tiemo = require('./me.js')

tiemo()
```



## 暴露多个函数

模块文件 me.js

```js
function tiemo(){
  console.log("贴膜")
}

function niejiao(){
  console.log('捏脚')
}

module.exports = {
  tiemo: tiemo,
  niejiao: niejiao
}
```

or

```js
function tiemo(){
  console.log("贴膜")
}

function niejiao(){
  console.log('捏脚')
}

exports.niejiao = niejiao
exports.tiemo = tiemo
```

常用 module.exports





主文件

```js
const me = require('./me.js')

me.tiemo()
me.niejiao()
```





## 注意事项

对于自己创建的模块，导入时路径建议写相对路径，且不能省略 ./ 和 ../

js 和 json 文件导入时可以不写后缀（先检测js的后缀）

如果导入的路径是个文件夹，则会首先检测该文件夹下的 package.json 文件中的 main 属性对应的文件，如果存在则导入，如果不存在则报错

​	如果 main 属性找不到，或 package.json文件不存在，则会尝试导入文件夹下的 index.js 和 index.json，如果还没找到则报错

导入 nodejs 内置模块时，直接 require 模块的名字即可，无需加 ./ 或 ../











# express 框架

express 是一个基于 node.js 平台的极简、灵活的 web 应用开发框架，官方网址：expressjs.com.cn

简单来说，express是一个封装好的工具包，封装了很多功能，便于我们开发 web 应用（HTTP服务）



## express 下载 & 初始化

先初始化 `npm init`

`npm i express`

```js
const express = require('express')

const app = express()

// 创建路由
app.get('/home', (request, response) => {  // 如果浏览器发送了 get 请求，且请求路径是 '/home'，则调用回调函数，为浏览器响应回调结果
  response.end('hello express')
})

// 监听端口，启动服务
app.listen(3000, () => {
  console.log('[info] 服务已经启动，端口3000正在监听中...')
})
```





## express 路由

路由确定了应用程序如何响应客户端对特定端点的请求

使用格式

`app.<method>(path, callback)`





## express 获取请求报文

express 框架封装了一些API来方便获取请求报文中的数据，并且兼容原生HTTP模块的获取方式

```js
const express = require('express')
const app = express()

app.get('/home', (request, response) => {
  // 原生操作
  console.log(req.method)
  console.log(req.url)
  console.log(req.httpVersion)
  console.log(req.headers)
  
  // express操作
  console.log(req.path)
  console.log(req.query)
  
  // 获取来访问的IP
  console.log(req.ip)
  
  // 获取请求头
  console.log(req.get('host'))
  
  response.end('hello express')
})

app.listen(3000, () => {
  console.log('[info] 服务已经启动，端口3000正在监听中...')
})
```





## express 获取路由参数

```js
const express = require('express')
const app = express()

app.get('/:id.html', (request, response) => {
  console.log(request.params.id) // 获取访问时的id，id可以是任意形参
  res.end('商品详情')
})

app.listen(3000, () => {
  console.log('[info] 服务已经启动，端口3000正在监听中...')
})
```







## express 全局中间件

```js
const express = require('express')
const fs = require('fs')
const app = express()

// 声明中间件函数
function recordMiddleware(req, res, next){
  let {url, ip} = req
  fs.appendFileSync(path.resolve(__direname, './access.log'), `${url} ${ip}\r\n`)
  next()
}

// 使用中间件函数
app.use(recordMiddleware)

app.get('/:id.html', (request, response) => {
  console.log(req.params.id) // 获取访问时的id，id可以是任意形参
  res.end('商品详情')
})

app.listen(3000, () => {
  console.log('[info] 服务已经启动，端口3000正在监听中...')
})
```





## express 静态资源中间件

设置了静态资源中间件，浏览器可以通过get url 访问 public 文件夹下的任意资源文件

```js
const express = require('express')
const app = express()

app.use(express.static(__dirname + '/public'))

app.listen(3000, () => {
  console.log('[info] sever is runing...')
})
```



注意：

index.html 是默认访问的文件

一般路由响应动态资源，静态资源中间件响应静态资源







# express-generator 应用生成器

## 安装

首先全局安装

```shell
npm install -g express-generator
```

在当前文件夹下创建骨架

```shell
express -e 15-generator
```

后续

在新创建的文件夹内打开终端，安装依赖

```shell
npm i
```

启动项目！

```js
npm start
```

打开浏览器访问 `127.0.0.1:3000` 











# express 路由

## 基本概念

“路由” 确定了应用程序如何响应客户端对特定端点的请求



## express中的路由

```js
const express = require('express')
const app = express()

// 创建 get 路由
app.get('/home', (req, res) => {
  res.send('网页首页')
})

// 创建 post 路由
app.post('/login', (req, res) => {
  console.log(req.body)
})
```







# RESTful API

RESTful API 是一种特殊风格的接口：

- URL 中的路径表示 资源，路径中不能有动词，例如 create、delete 等这些都不能有
- 操作资源要与 HTTP 请求方法 对应
- 操作结果要与 HTTP 响应状态码 对应

例如

GET /collection: 获取服务器资源

GET /collection/resource: 获取服务器内的单个资源

POST /collection: 上传新的资源至服务器

PUT /collection/resource: 更新服务器单个资源全部信息

PATCH /collection/resource: 更新服务器单个资源部分信息

DELETE /collection/resource: 删除服务器内的单个资源



 





API接口 示例

```js
const db = require('../../public/javascripts/db/db')
const BookModel = require('../../public/javascripts/models/BookModel')

router.get('/pictures', (requests, response, next) => {
  db().then(
    BookModel.find().then((data) => {
      response.json({
        code: '0000',
        message: '读取成功',
        data: data
      })
    })
  )
})
```





```json
{
  "status": "success",  // 描述 HTTP 响应结果，500-599 之间为 fail，在 400-499 之间为 error，其它均为 success
  "code": 200,
  "message": "操作成功",
  "data": : {
  	"nickname": "Joaquin Ondricka",
  	"email": "lowe.chaim@example.org"
	},
	"error": {}
}
```







# 会话控制 cookie session

常见的会话控制技术有 cookie session token



cookie 是 HTTP服务器 发送到用户浏览器并保存在本地的一小块数据

浏览器向服务器发送请求时，会自动将 当前域名下 可用的 cookie 设置在请求头中，传递给服务器



session 是保存在 服务器端的一块数据，保存当前访问用户的相关信息

填写账号和密码校验身份，校验通过后创建session信息，然后将session_id的值通过响应头返回给浏览器

有了cookie，下次发送请求时会自动携带cookie，服务器通过 cookie 中的 session_id 的值确定用户的身份





安装包

```shell
npm i express-session connect-mongo
```

调用

```js
const session = require('express-session')
const MongoStore = require('connect-mongo')
```

设置中间件

```js
app.use(session({
  name: 'sid',
  secret: 'atguigu',
  saveUninitialized: false,
  resave: true,
  store: MongoStore.create({
    mongoUrl: 'mongodb://127.0.0.1:27017/project'
  }),
  cookie: {
    httpOnly: true,
    maxAge: 1000 * 300
  }
}))
```











# 更改对外端口

在 app.js 文件的 module.export = app 前 加 `process.env.PORT = 9000`，这还与 bin 文件夹下的 www 文件中 `var port = normalizePort` 有关系













# 添加 cors 支持前后端分离



安装 cors: `npm install --save cors`

在 app.js 文件中添加：

```js
var cors = require('cors')
```



reference

https://www.freecodecamp.org/chinese/news/create-a-react-frontend-a-node-express-backend-and-connect-them-together/















# uuid

Reference: https://www.npmjs.com/package/uuid



安装命令如下：

```shell
npm install uuid
```

基础使用方法如下：

```js
import { v4 as uuidv4 } from 'uuid'
uuidv4(); // ⇨ '9b1deb4d-3b7d-4bad-9bdd-2b0d7b3dcb6d'
```

















# npm 换源

Reference：

- npm 淘宝镜像源: https://npmmirror.com/





国内配置镜像源会更快一些，命令如下

```shell
npm config set registry https://registry.npmmirror.com
```









