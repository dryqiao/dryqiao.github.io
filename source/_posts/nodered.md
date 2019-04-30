---
title: Node-RED初探
date: 2019/01/06 21:04:37
categories: [iot,node]
---
# Node-RED初探

## 1. 介绍

Node-RED是一个基于流程的IOT工具。提供了设备、PC和云的一种方便的交互方式。

1.  node-red采用了工作流的方式，使用户拖拽工作节点(node)，就可以配置复杂的任务。目前已经有1976个node，1245个flow
2.  node-red是基于node.js来编写的，有丰富的想象力。node.js已经有大量的开源包可以使用，社区十分活跃。如果有现有节点实现不了的任务，那就自定义一个node吧。
3.  如同nodejs ,node-red单线程事件触发，避免进程切换开销 ，占用资源少，不会出现加锁解锁的问题。


## 2. 安装

node-red是依赖nodejs的跨平台能力，它可以在如下平台安装。
![image](http://dry-image.test.upcdn.net/nodered1.png)

### 2.1 在ubuntu上安装

**step1:安装nodejs**

```bash
//install on ubunut
curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
sudo apt-get install -y nodejs

```

**step2:安装node-red**

```bash
sudo npm install -g --unsafe-perm node-red
```

**step3:运行**

```bash
sudo node-red
```

### 2.2 在raspberry pi上安装

[install on raspberry-pi](https://nodered.org/docs/hardware/raspberrypi)

```bash
bash <(curl -sL https://raw.githubusercontent.com/node-red/raspbian-deb-package/master/resources/update-nodejs-and-nodered)
```

### 2.3 在docker上安装

[install on docker](https://nodered.org/docs/platforms/docker)

```bash
docker run -it -p 1880:1880 --name mynodered nodered/node-red-docker
```


## 3. 使用

### 3.1 node说明

按照节点行为：
1.  头节点
2.  尾节点
3.  过程节点

按照功能划分：

![image](http://dry-image.test.upcdn.net/nodered2.png)

node-red现有的节点已经可以完成很多任务了，比如:
*  我们可以通过`http request`,`function`,`html`和`debug`节点做一个爬虫程序；
*  通过`exec`节点向本机发送shell命令完成部署工作；
*  通过`modbus`节点采集支持modbus的设备，通过`function`编写规则来进行预警，通过`mail`进行预警消息的发送，用`mqtt`发送采集消息到指定topic，另一端使用`mqtt`接收，并在`dashboard`上呈现相应逻辑。
*  ...需要充分发挥想象力

在使用某一个node时，可以在右侧的info面板中，查看其说明。也可以在[Node-RED Library](https://flows.nodered.org/?type=all&num_pages=1)中查询该节点的使用方式，并粘贴它的demo flow。

### 3.2 功能介绍

主版面

![image](http://dry-image.test.upcdn.net/nodered5.png)

配置导入

![image](http://dry-image.test.upcdn.net/nodered3.png)

节点管理

![image](http://dry-image.test.upcdn.net/nodered4.png)

### 3.3 自定义节点

node节点实际上就是一个moudle。如果需要自定义node，那必须得有一点nodejs的基础。

首先需要创建`package.json`，进行依赖包的管理。依赖包中“node-red”属性供node-red服务找到入口js文件，并创建节点:

```json
{
  "name": "demo-node",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "dry",
  "license": "ISC",
  "node-red" : {
        "nodes": {
            "lower-case": "lower-case.js"
        }
    }

}
```

然后创建`lower-case.js`和`lower-case.html`:

```javascript
module.exports = function(RED) {
    function LowerCaseNode(config) {
        RED.nodes.createNode(this,config);
        var node = this;
        node.on('input', function(msg) {
            msg.payload = msg.payload.toLowerCase();
            node.send(msg);
        });
    }
    RED.nodes.registerType("lower-case",LowerCaseNode);
}

```

```html
<script type="text/javascript">
    RED.nodes.registerType('lower-case',{
        category: 'function',
        color: '#a6bbcf',
        defaults: {
            name: {value:""}
        },
        inputs:1,
        outputs:1,
        icon: "file.png",
        label: function() {
            return this.name||"lower-case";
        }
    });
</script>

<script type="text/x-red" data-template-name="lower-case">
    <div class="form-row">
        <label for="node-input-name"><i class="icon-tag"></i> Name</label>
        <input type="text" id="node-input-name" placeholder="Name">
    </div>
</script>

<script type="text/x-red" data-help-name="lower-case">
    <p>A simple node that converts the message payloads into all lower-case characters</p>
</script>

```

最终，只需cd到~/.node-red，并npm install刚才的模块，node-red就动态加载这个节点了。

```
cd ~/.node-red
npm install /Users/victang/Desktop/demo-node
```


## 4. 对node-red的总结

### 4.1 node-red性能

对于node-red的性能，可以直接比较nodejs的性能。
这里有一篇比较服务端I/O性能的文章：[Server-side I/O Performance: Node vs. PHP vs. Java vs. Go](https://www.toptal.com/back-end/server-side-io-performance-node-php-java-go)
结论是node.js和java差不多，弱于go。



### 4.2 node-red的优点

1.  将逻辑封装成node, 进行拖拽式配置——>对比向导式配置, 更加直观
2.  可以导出json, 并通过json导入, 并且有demo库可供导入
3.  node可以在线安装, 并且可以在线升级
4.  node的说明文档, 配置信息整体呈现
5.  逻辑更简单, 只有【拖拽配置】, 【点击部署】两个概念
6.  在线调试功能强大
7.  每个节点的状态, 实时显示
