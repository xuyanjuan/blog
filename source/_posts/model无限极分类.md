---
title: model无限极分类

date: 2018/2/25 22:46:25

updated: 2018/2/25 22:46:25

categories:
 - django
 
tags:
 - model
---
![20200826102636292.jpg](https://i.loli.net/2021/02/01/mGWJefdCR9EaIlk.jpg)

### model.py
```
from django.db import models

class Parent_level(models.Model):
    # 分类名称
    name = models.CharField(max_length=100)
    # 父级id
    pid = models.ForeignKey('self', on_delete=models.SET_NULL, null=True, blank=True)

    # 表名
    def __str__(self):
        return self.name
```
### serializers.py
```
from rest_framework import serializers
from .models import *


class Pser(serializers.ModelSerializer):
    class Meta:
        model = Parent_level
        fields = "__all__"
```
### view.py
```
from django.shortcuts import render, HttpResponse
from rest_framework.views import APIView
from rest_framework.views import Response
from .serializer import *
from .models import *


def Tree(datas):
    lists = [] #最后的结果存放在这里
    tree = {} #相当于是承接部分
    parent_id = ''
    for i in datas:
        #将datas循环，然后加入一个dict中
        item = i
        tree[item['id']] = item
        # {  1:{id:1},  2:{id:2},  3:{id:3}  }
    root = None
    for i in datas:
        obj = i
        if not obj['pid']: #判断不在obj中的父级pid
            root = tree[obj['id']]
            lists.append(root)
        else:
            parent_id = obj['pid']
            # 判断父级是否有孩子字段（childlist），如果有将当前数据加入，如果没有添加（childlist）后再加入
            if 'childlist' not in tree[parent_id]:
                tree[parent_id]['childlist'] = []
            # 加入父级用户的记录
            tree[parent_id]['childlist'].append(tree[obj['id']])
    return lists


class MyTree(APIView):
    def get(self, request):
        parent_level = Parent_level.objects.filter()
        # 序列化
        p_ser = Pser(parent_level, many=True)

        mylist = Tree(p_ser.data)

        return Response(mylist)
```
### Vue
**在vue中封装一个递归组件**
```
<template>

  <div>

    <ul>
      <div :class="[data.id==0 ? 'root': '', 'hello']">{{ data.name }}</div>

      <ul v-if="data.childlist && data.childlist.length>0">
        <hello v-for="child in data.childlist" :key="child.id" :data="child"/>
      </ul>
    </ul>

  </div>
</template>

<script>
export default {
  name: 'hello', // 递归组件需要设置 name 属性，才能在模板中调用自己
  props:['data'],
};
</script>

<style >
.wxjcate {
  padding-left: 8px;
  border-left: 1px solid gray;
}
ul {
  padding-left: 20px;
  list-style: none;
}
.root { display: none; }

</style>

```
**调用组件**
```
<template>
  <div>

    <hello :data="data" />

  </div>
</template>

<script>


import hello from './hello.vue';



export default {
  data () {
    return {
      data:{},
      online: 0
    }
  },
  components: {
    hello
  },

  //钩子方法
  mounted:function(){

    this.get_token();
  },
  //绑定事件
  methods:{
    get_token(){
      this.$axios.get('mytree/').then((result) =>{
        // console.log(result);
        var mytree = {'id':0,name:'123'};
        mytree['childlist'] = result.data;
        this.data = mytree;
      });
    },
  }
}
</script>

<style>

.on {
  background: #cdcbff;
}
.off {
  background: #fefdff;

}
</style>
```


