---
title: cordova+vue混合开发
date: 2019/02/01 15:15:39
categories: [vue,cordova]
---
# cordova+vue混合开发
第一次用cordova混合开发，前端用Vue开发，demo里调用cordova的相机插件，最后用cordova打包成安卓apk。

## 一、混合开发初体验
### 创建cordova项目
1. 安装
`npm i -g cordova`
1. 创建demo项目
`cordova create demo`
我电脑上这一步就创建失败，提示：
`npmnode_modulescordovanode_modules_cordova-app-hello-world@3.12.0@cordova-app-hello-worldindex.jspackage.json`
但具体是什么原因造成的，没找到。于是我去[apache/cordova-app-hello-world](https://github.com/apache/cordova-app-hello-world)找到那个模版文件夹，把 `template_src`目录下的内容复制到你自己的目录下，改名为demo 就相当于完成了create 这一步操作了。
1. 添加安卓平台
`cd demo`
`cordova platform add android`
项目结构如图:
![](http://dry-image.test.upcdn.net/cordova1.png)


检查平台状况
`cordova platform ls`
 编译安卓
`cordova build android`

### 创建vue项目
webpack配置需要改成相对路径，vue-cli3搭建的项目，不再赘述，需要修改`vue.config.js`文件，将路径改为相对路径：
```
module.exports = {
	baseUrl: './'
}
```
`index.html`中需要加入`<script type="text/javascript" src="cordova.js"></script>`,
因为cordova会把www内的资源打包到`.\platforms\android\app\src\main\assets\www`,可以直接修改webpack的配置文件，将打包路径改到www里。因为是demo，我弄成了两个项目，没有合到一起，打包完vue项目，手动把`dist`文件夹内容拷进去。


### 安装Android Studio

网上很多，我看得这篇[安装Android Studio](https://www.cnblogs.com/xiadewang/p/7820377.html)

### Android Studio运行cordova项目
将`platforms\android`文件夹导入到`Android Studio`,
点开这个文件`app/src/main/java/io/cordova/demo/MainActivity.java`，右键点击`Run MainActivity`,等它运行完，就会出现下图模拟器：
![](http://dry-image.test.upcdn.net/cordova2.png)
混合开发的基本流程就是这样了。

## 二、调用cordova相机插件
1. 安装插件
`cordova plugin add cordova-plugin-camera`

1. 在前端调用相机
在按钮单击事件中调用`navigator.camera.getPicture`方法
	```
	clickHandler() {
		let vm = this
		navigator.camera.getPicture(vm.success, vm.fail, {
			quality: 50,
			destinationType: Camera.DestinationType.DATA_URL
		})
	}
	```

其中参数为成功、失败的回调，和相机的配置参数
[官网API](https://cordova.apache.org/docs/en/latest/reference/cordova-plugin-camera/index.html#camera)

在电脑的模拟器上打开是这种效果，连接手机的话会看到手机原生的相机：

![](http://dry-image.test.upcdn.net/cordova3.png)



混合开发调用原生设备接口的基本流程就是这样。



