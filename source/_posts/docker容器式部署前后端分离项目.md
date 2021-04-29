---
title: docker容器式部署前后端分离项目

date: 2019/1/15 10:46:25

updated: 2019/1/15 10:46:25

categories:
 - docker
 
tags:
 - django
---
![20200826102636292.jpg](https://i.loli.net/2021/02/01/mGWJefdCR9EaIlk.jpg)

# 配置django
- **在宿主机安装gunicorn，容器内我们用异步的方式来启动Django**
>pip install gunicorn gevent


- **Django项目配置settings.py对应的应用**
 ```
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'corsheaders',
    'rest_framework',
    'myapp',
    'dwebsocket',
    'gunicorn'
]
```
- **在Django项目的根目录编写gunicorn的配置文件：gunicorn.conf.py**
```
import multiprocessing

bind = "0.0.0.0:8000"   #绑定的ip与端口
workers = 1                #进程数
```
- **在项目的根目录编写好依赖列表：requirements.txt**
```
Django==2.0.4
django-cors-headers==2.5.3
djangorestframework==3.9.3
celery==4.4.2
dwebsocket==0.5.12
redis==3.3.11
pymongo==3.8.0
PyMySQL
Pillow
pyjwt
pycryptodome
selenium
qiniu
gunicorn
gevent
```
- **在项目根目录编写Dockerfile文件**
```
FROM python:3.7
# 定制镜像，需要先有一个基础镜像，在这个基础镜像上进行定制。FROM 就是指定基础镜像，此指令必需放在有效指令的第一行

WORKDIR /Project/mydjango
# 为后续的 RUN、CMD、ENTRYPOINT 指令配置工作目录，当容器运行后，进入容器内的默认目录

COPY requirements.txt ./
# 复制本机的文件（以Dockfile所在目录相对路径）到容器中

RUN pip install -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple
# 用来执行命令、RUN 指令经常用来调用shell指令

COPY . .
ENV LANG C.UTF-8
# 指定一个环境变量，会被后续 RUN 指令使用，并在容器运行时保持

CMD ["gunicorn", "mydjango.wsgi:application","-c","./gunicorn.conf.py"]
# 如果docker run没有指定任何的执行命令或者dockerfile里面也没有ENTRYPOINT，那么就会使用执行CMD指定的默认的命令
```
# 打包django镜像并启动
![2f4ceb9da8f95ad228f22794bd29647.png](https://i.loli.net/2021/03/29/RoWnUMBehKgxctJ.png)

- **cd进入到项目的目录下执行**
>docker build -t 'mydjango' .
- **查看打包好的镜像**
>docker images
- **启动镜像**
>docker run -it --rm -p 5000:8000 mydjango
- **打开阿里云的防火墙**
![ee76d54426a7ed603cb59960fdd3e70.png](https://i.loli.net/2021/03/29/GcimzSsQDKBwEX6.png)

利用端口映射技术将宿主机的5000端口映射到容器内的8000端口
- **访问Django服务**
>容器ip:5000

# 配置Vue
- **首先打开vue项目的打包配置文件config/index.js**
```
build: {
    // Template for index.html
    index: path.resolve(__dirname, '../dist/index.html'),

    // Paths
    assetsRoot: path.resolve(__dirname, '../dist'),
    assetsSubDirectory: 'static',
    assetsPublicPath: './',

    /**
     * Source Maps
     */

    productionSourceMap: true,
    // https://webpack.js.org/configuration/devtool/#production
    devtool: '#source-map',

    // Gzip off by default as many popular static hosts such as
    // Surge or Netlify already gzip all static assets for you.
    // Before setting to `true`, make sure to:
    // npm install --save-dev compression-webpack-plugin
    productionGzip: false,
    productionGzipExtensions: ['js', 'css'],

    // Run the build command with an extra argument to
    // View the bundle analyzer report after build finishes:
    // `npm run build --report`
    // Set to `true` or `false` to always turn it on or off
    bundleAnalyzerReport: process.env.npm_config_report
  }
}
```
- **如果曾经修改为history模式记得改回hash**
```js
export default new Router({
  //mode:'history'   /*hash*/
})
```
- **在vue项目根目录下编写Dockerfile**
```
FROM node:lts-alpine

# install simple http server for serving static content
RUN npm install -g http-server

# make the 'app' folder the current working directory
WORKDIR /app

# copy both 'package.json' and 'package-lock.json' (if available)
COPY package*.json ./

# install project dependencies
RUN npm install

# copy project files and folders to the current working directory (i.e. 'app' folder)
COPY . .

# build app for production with minification
RUN npm run build

EXPOSE 8080
CMD [ "http-server", "dist" ]
```
# 打包Vue镜像并启动
![2f4ceb9da8f95ad228f22794bd29647.png](https://i.loli.net/2021/03/29/RoWnUMBehKgxctJ.png)
- **项目的根目录，执行打包命令**
>docker build -t myvue .
- **启动镜像**
>docker run -it --rm -p 8081:8080 myvue

同样打开阿里云的防火墙
- **访问Vue.js服务**
>容器ip:8081
