# cordova实战

## 安装
`cnpm i -g cordova`

//不知什么原因，我的电脑上这一步就创建失败，提示：

..npmnode_modulescordovanode_modules_cordova-app-hello-world@3.12.0@cordova-app-hello-worldindex.jspackage.json' 这个文件不存在，但index.js应该是一个文件，下面怎么可能有文件呢？

但具体是什么原因造成的，我不知道。

于是我自己去_cordova-app-hello-world@3.12.0@cordova-app-hello-world 找到那个模版文件夹，

把 template_src 复制到你喜欢的目录下，改名为MyApp 就相当于完成了create 这一步操作了。

cd MyApp

`cordova platform add android`

检查平台状况
`cordova platform ls`

## 安装Android Studio

[](https://www.cnblogs.com/xiadewang/p/7820377.html)

build时报错
`You have not accepted the license agreements of the following SDK components:`
错误原因：
从Android Gradle plugin 2.2.0开始，gradle会自动加载需要的SDK, build-tools，但是因为没有接受license，导致加载依赖终止。
解决方法：
1. 在android SDK位置，进入Tools/bin目录下
1. 打开cmd（不要用其他的），进入上面的目录
1. 输入命令: `sdkmanager.bat --licenses`
1. 输入之后，会跳出来很多东西，我们只需要全部写上y即可

## 创建cordova项目
`cordova create demo`

## 前端资源处理
webpack配置需要改成相对路径，vue-cli3搭建的项目，需要在根目录下新建`vue.config.js`文件，配置如下：
```
module.exports = {
	baseUrl: './'
}
```
`index.html`中需要加入`<script type="text/javascript" src="cordova.js"></script>`,
因为cordova会把www内的资源打包到`.\platforms\android\app\src\main\assets\www`,




[签名打包](https://segmentfault.com/a/1190000005177715)






