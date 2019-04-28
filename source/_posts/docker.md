---
title: Docker实践
date: 2018/07/05
categories: Docker
---
# docker实践
本篇是根据阮一峰的这篇博客来进行学习实践的[Docker 入门教程](http://www.ruanyifeng.com/blog/2018/02/docker-tutorial.html)，记录一下学习过程和踩过的坑。
## 安装docker
下载地址：[Docker for Windows](https://docs.docker.com/docker-for-windows/install/)
安装完成之后会重启，重启之后突然给我来个报错，windows果然不能乱用
![](http://dry-image.test.upcdn.net/error.png)
还好点了链接进去，就能看到解决方法，需要进入bios修改配置,将virtualization technology 打开。

可以参考这篇：[bios打开VTx](https://zhidao.baidu.com/question/240877150936295084.html)

操作完成之后重启电脑，docker会自动弹出以下窗口：

![](http://dry-image.test.upcdn.net/done.png)

可以通过以下命令查看是否安装成功
```javascript
docker version
或者
docker info
```

## HELLO WORLD
> Docker 把应用程序及其依赖，打包在 image 文件里面。只有通过这个文件，才能生成 Docker 容器。image 文件可以看作是容器的模板。Docker 根据 image 文件生成容器的实例。同一个 image 文件，可以生成多个同时运行的容器实例。image 是二进制文件。实际开发中，一个 image 文件往往通过继承另一个 image 文件，加上一些个性化设置而生成。

现在从官方仓库拉一个image文件下来：

`docker image pull library/hello-world`

获取成功后，可以在本机看到这个文件

`docker images ls`

![](http://dry-image.test.upcdn.net/docker-hw.png)

运行这个image文件

`docker container run hello-world`

![](http://dry-image.test.upcdn.net/docker-hello.png)

输出完成之后，这个容器会自动终止，有些容器提供的是服务，不会自动终止，可以通过命令终止 `docker container kill [containID]`


## 制作自己的Docker容器

### 编写Docker文件
新建`.dockerignore`文件，写入以下内容：
```javascript
.git
node_modules
npm-debug.log
```
和 .gitignore一个作用，不打包进image文件。
再在项目根目录下新建`Dockerfile`，写入以下内容：
```docker
FROM node:8.9-alpine
ENV NODE_ENV production
WORKDIR /usr/src/app
COPY ["package.json", "package-lock.json*", "npm-shrinkwrap.json*", "./"]
RUN npm install --production --silent && mv node_modules ../
COPY . .
EXPOSE 3000
CMD npm start
```
```
FROM node:8.9-alpine：FROM表示image文件继承官方的 node image，冒号表示标签，8.9表示node版本， -alpine表示基于轻量级的Linux系统

ENV NODE_ENV production： 生产环境

WORKDIR /app：指定接下来的工作路径为/usr/src/app

COPY ：将这些文件拷贝进入 image 文件的"./"目录。

RUN ：执行node命令

EXPOSE 3000：将容器 3000 端口暴露出来， 允许外部连接这个端口。
```

这里可以用VSCODE的一个插件，`Docker`,可以自动生成这些内容，方便。

### 创建image文件
有了`Dockerfile`文件后，可以用`docker image build -t [镜像名]] .`命令创建image文件
执行后，如下图所示
![](http://dry-image.test.upcdn.net/docker-bd.png)

### 生成容器
执行`docker container run -p 8000:3000 -it [镜像名] /bin/bash`

```
-p : 端口映射
-it ：容器的shell映射
```

可以通过`docker ps`查看正在运行的容器

![](http://dry-image.test.upcdn.net/docker-ps.png)


结束。