---
title: Vue拦截器

date: 2018/3/2 13:43:24

updated: 2018/3/2 13:43:24

categories:
 - Vue拦截器
 
tags:
 - Vue
---
![20200826102636292.jpg](https://i.loli.net/2021/02/01/mGWJefdCR9EaIlk.jpg)

# 什么是拦截器:
**拦截器可以说相当于是个过滤器：就是把你不想要的或不想显示的内容给过滤掉。 也可以检查请求信息，也可以处理响应信息**

# 拦截器分类:
**在请求或响应被 then 或 catch 处理前拦截他们，分为请求拦截器和响应拦截器；**

# 拦截器的使用和配置
**使用**
~~~
import axios from 'axios'
 

 
// 请求拦截器(在请求前会自动去做的操作)，如果登陆自动带token发送
axios.interceptors.request.use(config => {
  let token = localStorage.getItem('token');
  console.log(token)
  if (token) {
    config.headers.common['Authorization'] = "JWT " + token // 头部设置
  }
  return config;
}, error => {
  console.log(error)
  // 对请求错误做点什么
  return Promise.reject(error);
});
 
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
~~~
**在main.js加入配置:**
~~~
import axios from 'axios';
import '../config/axios'
Vue.prototype.$ajax = axios
~~~

