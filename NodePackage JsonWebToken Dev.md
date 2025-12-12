# 概述

JWT (JSON Web Token) 



参考:

npm (jsonwebtoken): https://www.npmjs.com/package/jsonwebtoken

尚硅谷Node.js零基础视频教程（jwt介绍与使用）

https://www.bilibili.com/video/BV1gM411W7ex?spm_id_from=333.788.videopod.episodes&vd_source=9ac83b3ca683e3dcf6e29152632646e8&p=188

# JWT Token

安装指令:

```bash
npm install jsonwebtoken
npm install --save @types/jsonwebtoken
```



创建 token

```js
import * as jwt from "jsonwebtoken"

const token = jwt.sign()  // 参数一是用户数据，参数二是加密字符串，参数三是配置对象 可以设置生命周期
const token = jwt.sign(
	{username: "aikn"},
  "secret",
  {expiresIn: 30} // 30秒
)
```



校验 token

```tsx
const response = jwt.verift(token, "secret")
```

如果校验失败，会报错 `JsonWebTokenError`

如果校验成功，返回信息内容:

```
{
  secretId: 'cd52764d-0d4d-4e4a-b9cb-e14bf527ad50',  // 这里是 token 创建时加入的信息
  iat: 1747275288,
  exp: 1747275318
}
```



超时报的错误是

```
TokenExpiredError: jwt expired
    at /Users/annanmu/workspaces/北建有余微信小程序/program/withNestJS/server/node_modules/jsonwebtoken/verify.js:190:21
    at getSecret (/Users/annanmu/workspaces/北建有余微信小程序/program/withNestJS/server/node_modules/jsonwebtoken/verify.js:97:14)
    at module.exports [as verify] (/Users/annanmu/workspaces/北建有余微信小程序/program/withNestJS/server/node_modules/jsonwebtoken/verify.js:101:10)
    at LoginService.login (/Users/annanmu/workspaces/北建有余微信小程序/program/withNestJS/server/src/modules/login/login.service.ts:43:35)
    at process.processTicksAndRejections (node:internal/process/task_queues:105:5) {
  expiredAt: 2025-05-15T02:19:07.000Z
}
```





```tsx
import * as jwt from "jsonwebtoken"
import {JsonWebTokenError, TokenExpiredError} from "jsonwebtoken"

try {
  const payload = jwt.verify(token, secret);
  console.log("验证成功:", payload);
} catch (err: unknown) {
  if (err instanceof TokenExpiredError) {
    console.log("Token 已过期");
  } else if (err instanceof JsonWebTokenError) {
    console.log("无效的 Token");
  } else {
    console.log("其他错误:", err);
  }
}
```

或者通过 err.name 判断错误类型 (推荐，兼容性更强)

```tsx
import * as jwt from "jsonwebtoken"

try {
  jwt.verify(token, secret);
} catch (err: any) {
  switch(err.name) {
    case "TokenExpiredError": console.log('Token 过期'); break;
    case "JsonWebTokenError": console.log('JWT 格式或签名错误'); break;
    default: console.log('未知 JWT 错误:', err)
  }
}
```



