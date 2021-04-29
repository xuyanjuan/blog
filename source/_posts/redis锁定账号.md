---
title: redis锁定账号

date: 2018/10/25 22:46:25

updated: 2018/10/25 22:46:25

categories:
 - django
 
tags:
 - redis
---
![20200826102636292.jpg](https://i.loli.net/2021/02/01/mGWJefdCR9EaIlk.jpg)


# redis锁定账号

**在五分钟以内输错5次用户名和密码进行锁定**

```py
class LoginView(APIView):
    def post(self, request):
        # 首先获取到用户名和密码
        username = request.data.get("username")
        password = request.data.get("password")

        r = redis.Redis()

        # 判断该用户是否已存在黑名单
        if r.get(str(username) + "black"):
            return Response({"msg": "抱歉,该账户已被封禁,请等待30分钟", 'code': 400})

        # 如果没有被封禁,那么就进行登录
        user_li = UserModel.objects.filter(username=username, password=password)
        # 判断密码是否正确
        if user_li:
            return Response({"msg": "登陆成功", 'code': 200, 'uid': user_li[0].id, 'username': user_li[0].username})

        # 判断redis中是否存在该键值对,如果不存在,则进行创建
        if r.get(str(username)) == None:
            r.set(str(username), 0)

        # 判断是否是第一次输错,如果是那么就开始计时
        if int(r.get(str(username))) == 0:
            # 存在时间
            r.expire(str(username), 200)
            # 进行累加
            r.incrby(str(username), 1)
            return Response({"msg": "第一次错误", 'code': 400})
        # 如果不是第一次错误,那么就开始进行累加
        else:
            # 如果在五分钟内错误了五次后,那么就进行封禁
            if int(r.get(str(username))) == 5:
                r.set(str(username) + "black", str(password))
                # 封禁时间
                r.expire(str(username) + "black", 1800)
                return Response({"msg": "封禁", 'code': 400})
            # 进行累加
            r.incrby(str(username), 1)
            return Response({"msg": "错误", 'code': 400, 'count': r.get(str(username))})

```

