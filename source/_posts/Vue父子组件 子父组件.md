---
title: Vue父子组件 子父组件

date: 2018/4/2 20:46:25

updated: 2018/4/2 20:46:25

categories:
 - Vue
 
tags:
 - 组件传参
---
![20200826102636292.jpg](https://i.loli.net/2021/02/01/mGWJefdCR9EaIlk.jpg)

# 父向子传, 是用属性传递
**父组件：**
~~~
<template>
<!-- 通过props向子组件传递name值 -->
  <child name="小明">
</template>

<script>
  // 引入子组件
  import child from "./child-component"
  export default {
    name: "parent-component",
    components:{
      child
    }
  }
</script>

<style>

</style>
~~~
**子组件：**
~~~
<template>

  <div>
    通过props从父组件传递的值：{{name}}
  </div>

</template>

<script>
  export default {
    name: "child-component",
    props: {
      name: {
        type: String,
        default: "xiaoming"
      }
    }
  }
</script>

<style scoped>

</style>
~~~
# 子向父传, 是用事件传递
**子组件：**
~~~
<template>

  <div style="background-color: #0a9dc7 ;height:200px">
    通过props从父组件传递的值：{{name}}

    <button @click="btnClick">向父组件传<button>
  </div>

</template>

<script>
  export default {
    name: "child-component",
    props: {
      name: {
        type: String,
        default: "xiaoming"
      }
    },

    data() {
      return {
        user: {
          name: "xieluoxixi",
          password: "123456"
        },
        other: "other data"
      }
    },
    methods: {
      btnClick() {
        //通过$emit向父组件传递值，其中可以一次性传递多个
        this.$emit("sendEventToParent", this.user, this.other)
      }
    }
  }
</script>

<style>

</style>
~~~
**父组件：**
~~~
<template>
  <div>
    <p>
      父组件从子组件上获取的值：
      {{user.name+user.password}}
      {{other}}
    </p>
    <!--通过props向子组件传递name值-->
    <p>
      下方为子组件区域
    </p>
    <child name="小明" @sendEventToParent="getValueFromChild"/>
  </div>
</template>

<script>
  // 引入子组件
  import child from "./child-component"

  export default {
    name: "parent-component",
    components: {
      child
    },
    data() {
      return {
        //用于接收从子组件传递回来的值
        user: {},
        other: ""
      }
    },
    methods: {
      getValueFromChild(user, other) {
        // console.log(user)
        // console.log(other)

        this.user = user
        this.other = other
      }
    }
  }
</script>

<style scoped>

</style>
~~~