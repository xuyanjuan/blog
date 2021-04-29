---
title: Vue路由钩子

date: 2018/2/24 20:46:25

updated: 2018/2/24 20:46:25

categories:
 - Vue
 
tags:
 - 路由
---
![20200826102636292.jpg](https://i.loli.net/2021/02/01/mGWJefdCR9EaIlk.jpg)

# 什么是路由钩子（路由守卫）：
**路由守卫有点类似于ajax的请求拦截器，就是请求发送之前先给你拦截住做一些事情之后再去发送请求**

# 路由钩子分类：
**全局钩子函数**
~~~
router.beforeEach((to, from, next) => {
    // to router即将进入的路由对象
    // from:当前导航即将离开的路由
    // next:一个必须要调用的方法
    let token = router.app.$storage.fetch("token");
    let needAuth = to.matched.some(item => item.meta.login);
    if(!token && needAuth) return next({path: "/login"});
    next();
});
~~~
**路由独享的钩子函数**
~~~
cont router = new VueRouter({
    routes: [
        {
            path: '/file',
            component: File,
            beforeEnter: (to, from ,next) => {
                // do someting
            }
        }
    ]
});
~~~
**组件内钩子函数**
~~~
const File = {
    template: `<div>This is file</div>`,
    beforeRouteEnter(to, from, next) {
        // do someting
        // 在渲染该组件的对应路由被 confirm 前调用
    },
    beforeRouteUpdate(to, from, next) {
        // do someting
        // 在当前路由改变，但是依然渲染该组件是调用
    },
    beforeRouteLeave(to, from ,next) {
        // do someting
        // 导航离开该组件的对应路由时被调用
    }
}
~~~