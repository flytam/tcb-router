# tcb-router

[![NPM version][npm-image]][npm-url]
[![Build Status](https://travis-ci.org/flytam/tcb-router.svg?branch=master)](https://travis-ci.org/flytam/tcb-router)
[![npm download][download-image]][download-url]
[![npm license][license-image]][download-url]
[![Coverage Status](https://coveralls.io/repos/github/flytam/tcb-router/badge.svg?branch=master)](https://coveralls.io/github/flytam/tcb-router?branch=master)

[npm-image]: https://img.shields.io/npm/v/tcb-router.svg?style=flat-square
[npm-url]: https://npmjs.org/package/tcb-router
[david-image]: https://img.shields.io/david/673800357/tcb-router.svg?style=flat-square
[download-image]: https://img.shields.io/npm/dm/tcb-router.svg?style=flat-square
[download-url]: https://npmjs.org/package/tcb-router
[license-image]: https://img.shields.io/npm/l/tcb-router.svg

> 该库可用于云函数路由处理以及即将上线的小程序云函数路由处理。目前该库仍处于不稳定开发阶段

小程序云函数类路由，包含小程序端和云函数端

服务端库初步完成。文档等需求肝完慢慢更...

优化服务端函数处理逻辑

## 测试

```bash
yarn run test
```

## API 设计阶段

## server 基础部分完成，客户端尚未开工

## 云函数端

基于 express 风格

```bash
npm install tcb-router
```

```javascript
const { TcbRouter } = require("tcb-router");

exports.main = async (event, context, callback) => {
    const app = new TcbRouter({ event, context, defaultRes: true, callback });
    app.use((req, res, next) => {
        // 可以像在express中那样使用中间件机制，不调用next的话就不会走到下一个路由了
        next();
    });
    app.use((req, res, next) => {
        if (req.xxx) {
            res.callback(null, "xxxx");
        } else {
            next();
        }
    });
    app.receive("login", async (req, res) => {
        const result = await getFromDatabase();
        res.callback(null, { result });
    });
    await app.apply(); // 应用
};
```

### TcbRouter 初始化

参数

-   event
-   context
-   defaultRes : Boolean 表示是否使用默认回参， 默认是否，和云函数提供的 callback 相同

```javascript
// 默认回参
{
    code: 0 | other， // 0 表示正常
    data: null | {}, //返回的数据
    message: '' | 'error'//出错情况下的报错信息
}
```

### app.use

1、use(string,function | array[function])

### req

event 相当于 event 的值

data 小程序端的 data 字段

url 需要匹配的路径，通过云函数入口的 event.url 字段传入。\*表示匹配任意路径，传入其它值是精准匹配

### res

res.callback

返回内容

## 小程序端 该部分尚未编写...

```javascript
const {TcbRouterClient} = require("tcb-router");
const client = new TcbRouterClient({
  functionName: "test", //调用的云函数名字
});
async login(){
const {data,code,message} = client.send("login",{
username:"flytam",
password:"password"
})
}
```
