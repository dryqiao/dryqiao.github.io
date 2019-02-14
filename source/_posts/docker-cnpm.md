---
title: 用Docker搭建cnpm私有仓库以及私有仓库的使用
date: 2018/09/21
categories: [Docker,cnpm]
---
# 用Docker搭建cnpm私有仓库以及私有仓库的使用

## 搭建cnpm私有仓库

这是官方文档：
[Deploy a private npm registry in 5 minutes](https://github.com/cnpm/cnpmjs.org/wiki/Deploy-a-private-npm-registry-in-5-minutes)
但是按照这个跑不起来，一直报环境的环境，所以我稍微修改了下代码，下面是我的Dockerfile：
```
FROM node:10-alpine
MAINTAINER qiao <445271466@qq.com>

RUN npm install -g cnpm && cnpm i -g cnpmjs.org sqlite3

COPY config.json /root/.cnpmjs.org/

VOLUME /root/.cnpmjs.org

EXPOSE 7001 7002
CMD ["cnpmjs.org", "start"]
```
cnpmjs.org修改的部分配置，
[cnpmjs.org默认配置](https://github.com/cnpm/cnpmjs.org/blob/master/config/index.js)
```
{
    "admins": {
      "dev": "dev"
    },
    "scopes": [
      "@md"
    ],
    "enablePrivate": true,
    "bindingHost": "0.0.0.0",
    "registryHost": "localhost:17001"
  }
```
`admins`是仓库用户
`scopes`是域，私有包都要加上这个前缀
`registryHost`是一个坑，默认是`r.cnpmjs.org`，
若不修改，安装私有包的时候会从这个地址的仓库获取，会报404错误，因为这是公有仓库地址，私有包并没有传到上面，
我们需要将他改成容器映射出来的地址，我容器映射出来的地址是`localhost：17001`，就将它改成这个。
若你需要修改这个端口，两个方法：
1. 克隆我的项目[github地址](https://github.com/dryqiao/cnpmjs.org),修改配置文件，安装下面的方法自己创建镜像，生成容器。
2. 用我的镜像生成容器，进入容器修改配置文件，然后重启。

#### 制作Docker镜像
`docker image build -t cnpmjs.org:1.0 .`

#### 生成容器
`docker run -it -p 17001:7001 -p 17002:7002 --name mdnpm.org cnpmjs.org:1.0`

#### 推送镜像到dockerhub
先登录自己的docker账号：`docker login`
推送镜像的规范是：`docker push 注册用户名/镜像名`
本地的镜像需要先打个tag，标上自己的用户名和版本号
`docker tag cnpmjs.org:1.0 dryqiao/cnpmjs.org:1.0`
`docker push dryqiao/cnpmjs.org:1.0`
然后如下图，推送成功!
![](http://dry-image.test.upcdn.net/docker-push.png)

这里我将容器的7001,7002映射到17001和17002上，因为7001容易被占用

#### 通过dockerhub一步生成容器
我把镜像传到dockerhuub上了，可以跳过以上步骤直接使用，
`docker run -it --name cnpmjs.org -p 17001:7001 -p 17002:7002 dryqiao/cnpmjs.org`

现在在浏览器打开`localhost:17002`，能看到下图表示仓库搭建成功！
![](http://dry-image.test.upcdn.net/docker-7002.png)

## 私有仓库的使用

#### 修改cnpm的源：
`cnpm set registry http://localhost:17001`

ok，现在下载包的时候，会优先去私有仓库获取，若找不到，cnpm会再去公共仓库获取。

#### 发布私有包
```
cnpm init
cnpm publish
```
出现下图表示发布成功！
![](http://dry-image.test.upcdn.net/cnpm-publish.png)

可以通过命令查看这个私有包的信息
`cnpm view @md/hhh`
![](http://dry-image.test.upcdn.net/cnpm-view.png)

#### 下载包

`cnpm i [packge name]`

END