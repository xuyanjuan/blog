---
title: axios封装

date: 2018/2/20 23:02:20

updated: 2018/2/20 23:02:20

categories:
 - Vue
 
tags:
 - axios
---
![20200826102636292.jpg](https://i.loli.net/2021/02/01/mGWJefdCR9EaIlk.jpg)


## 安装 
```npm install axios```

官方文档https://www.npmjs.com/package/axios

## 新建文件夹
**index.js文件用来封装我们的axios，api.js用来统一管理我们的接口**
![vIponP4Z8OJykmS.png](https://i.loli.net/2021/03/19/U6ctj4G5WdIlHPf.png)

## 配置axios的基本属性
```javascript
// index.js

const axios = require('axios')

axios.defaults.baseURL = 'http://127.0.0.1:8000' // 请求的接口主域名
axios.defaults.timeout = 10000 // 请求超时的设置 10s
```
## 设置请求拦截器

就是在发起请求前进行操作，比如需求是在用户登录以后才能访问，那么就可以使用请求拦截器
```javascript
// 请求拦截器(在请求前会自动去做的操作)，如果登陆了，那么自动带token发送
axios.interceptors.request.use(config => {
  let token = localStorage.getItem('token');
  console.log(token)
  if (token) {
    config.headers.common['Authorization'] = "JWT " + token // 头部设置
  }
  return config;
}, error => {
  console.log(error)
  return Promise.reject(error);
});
```

## 设置响应拦截器
就是在服务器返回给我们数据之前做一些额外的操作
```javascript
// 响应拦截器(在响应时自动会做的操作)，比如状态码返回为401时，要进入login界面
axios.interceptors.response.use(response => {
  return response;
}, error => {
  console.log(error.request.response)
  if (error.request.status == 401) {
    window.location.href = "/"
  }
  return error
});
```
## 封装 get、post、put、delete四种请求
```css
/**
 * post方法，对应post请求
 * @param {String} url [请求的url地址]
 * @param {Object} params [请求时携带的参数]
 * @param {Object} headers [请求时的头部]
 **/

```
**get请求**
```javascript
// index.js
export function get(url, params, headers) {
  return new Promise((resolve, reject) => {
    axios.get(url, {params, headers}).then(res => {
      resolve(res)
    }).catch(err => {
      reject(err)
    })
  })
}
```   
**post请求**
 ```javascript
// index.js
export function post(url, params, headers) {
  return new Promise((resolve, reject) => {
    axios.post(url, params, headers).then((res) => {
      resolve(res)
    }).catch((err) => {
      // debugger
      reject(err)
    })
  })
}
```
**put请求**
```javascript
// index.js
export function put(url, params, headers) {
  return new Promise((resolve, reject) => {
    axios.put(url, params, headers).then((res) => {
      resolve(res)
    }).catch((err) => {
      // debugger
      reject(err)
    })
  })
}
```
**delete请求**
```javascript
export function del(url, params, headers) {
  return new Promise((resolve, reject) => {
    axios.delete(url, {data: params, headers}).then((res) => {
      resolve(res)
    }).catch((err) => {
      // debugger
      reject(err)
    })
  })
}
```
## api管理接口
**在api.js里面导入我们封装的方法**
```javascript
import {get, post, put, del} from './index'
```
```js
// signUp是我们定义的方法
export const signUp = parameter => {
  return post(
    'signup/',  //baseURL后拼接的路由 
    parameter, //接口请求时携带的参数对象
  )
```
**页面内调用**
```js
import {signUp,signIn} from "@/http/api";

function signUp(){
      let params = {
        "username": this.signup_username,
        'password': this.signup_password,
      }
      signUp(params).then(res => {
        if (res.code == 200) {
          console.log('成功')
        }
        if (res.code == 400) {
         console.log('失败')
        }
      }).catch(error => {
        console.log(error)
      })
    }
```
