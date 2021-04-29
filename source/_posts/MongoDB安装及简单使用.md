---
title: MongoDB安装及简单使用

date: 2018/9/2 20:46:25

updated: 2018/9/2 20:46:25

categories:
 - 数据库
 
tags:
 - MongoDB
---
![20200826102636292.jpg](https://i.loli.net/2021/02/01/mGWJefdCR9EaIlk.jpg)
# 优点
**高性能、可扩展、易部署、易使用，存储数据非常方便 更高的写入负载，表结构不明确，且数据在不断变大**

# 缺点

**不能保证事务安全性 消耗内存大**

# 安装
下载地址：https://www.mongodb.com/download-center/community
![B370EBF1-A8F8-483A-A05C-F0DE0A903A96.jpg](https://i.loli.net/2021/03/18/cnDqQYE932aRfHk.jpg)
**点击 "Custom(自定义)" 按钮来设置你的安装目录**
![win-install1.jpg](https://i.loli.net/2021/03/18/JWuhSb3UQVlNqnB.jpg)
![win-install2.jpg](https://i.loli.net/2021/03/18/qjKORnLG6hzgA9c.jpg)
**下一步安装 "install mongoDB compass" 可选可不选（安装时间长短的问题）**
![8F7AF133-BE49-4BAB-9F93-88A9D666F6C0.jpg](https://i.loli.net/2021/03/18/jaUqSMXWZNtTfil.jpg)
**创建数据目录**

MongoDB 将数据目录存储在 db 目录下。但是这个数据目录不会主动创建，我们在安装完成后需要创建它（数据目录应该放在根目录下）
~~~
mkdir "\data\db"
~~~

**命令行下运行 MongoDB 服务器**
从 MongoDB 目录的 bin 目录中执行 mongod.exe 文件
~~~
C:\mongodb\bin\mongod --dbpath c:\data\db
~~~
如果执行成功，会输出如下信息
![6866eda7d9972ec6df48d4989c7b9fa.png](https://i.loli.net/2021/03/18/rvfcnidTF8EQxBa.png)

**连接MongoDB**
~~~
C:\mongodb\bin\mongo.exe
~~~
连接成功
![1578d37a80abe1b2ba4845596aabd9e.png](https://i.loli.net/2021/03/18/gEal1pviqDsfO5G.png)
# python操作MongoDB
>pip install pymongo
```py
from pymongo import MongoClient

# 连接MongoDB
client = MongoClient(host='localhost', port=27017)

# 获取MongoDB数据

class GetMongo(APIView):
    def get(self,request):
        # 进入数据库
        db = client.WorkOrder
        # 集合
        table = db.CateTemplate
        # 查找数据
        res = table.find_one({'wid':1)

        return Response(res)

```

# 简单使用
~~~
> db.stats() 查看状态

> db.version() 查看版本

> db.help() 显示数据库操作命令，里面有很多的命令 

> use runoob  创建数据库 如果已经存在则切换

> db 查看当前的数据库

> show dbs  查看所有的数据库

> db.createCollection(name, options)
    # 先创建集合，类似数据库中的表 name: 要创建的集合名称  options: 可选参数, 指定有关内存大小及索引的选项

> db.runoob.insert({"name":"hello"}) 插入数据

> db.dropDatabase() 删除数据库

> db.collection.drop() 
删除集合  collection某一个集合名  如果成功删除选定集合，则 drop() 方法返回 true，否则返回 false

> show tables 查看集合

> show collections 也是查看集合 命令会更加准确点

> db.userInfo.findOne() 查询第一条数据

> db.userInfo.find({"name": xucongming} 查找名字是xucongming的数据

> db.userInfo.find({age: {$gt: 22}}) 查询age > 22的记录

> db.userInfo.find({age: {$lt: 22}}) 查询age < 22的记录

> db.userInfo.find({age: {$gte: 25}}) 查询age >= 25的记录

> db.userInfo.find({age: {$lte: 25}}) 查询age <= 25的记录

> db.userInfo.find({age: {$gte: 23, $lte: 26}} 查询age >= 23 并且 age <= 26

> db.userInfo.find({name: /mongo/}) 查询name中包含 mongo的数据

> db.userInfo.find().limit(5) 查询前5条数据

> db.userInfo.find().limit(10).skip(5) 查询在5-10之间的数据
~~~