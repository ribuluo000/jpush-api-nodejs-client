# JPush API client library for Node.js


im:
[https://docs.jiguang.cn/jpush/client/iOS/ios_faq/](https://docs.jiguang.cn/jpush/client/iOS/ios_faq/)

Jpush推送Notification iOS在appstore 版本收不到（客户端配置正确--jpush后台可以推过来，但是自己的后台推不过来）的解决方法：

```js
client.push().setPlatform('ios', 'android')
    .setAudience(JPush.tag('555', '666'), JPush.alias('666,777'))
    .setNotification('Hi, JPush', JPush.ios('ios alert','tishiyin.mp3',), JPush.android('android alert', null, 1))
    .setMessage('msg content')
    .setOptions(null, 60)   //todo important 
    .send(function(err, res) {
        if (err) {
            console.log(err.message)
        } else {
            console.log('Sendno: ' + res.sendno)
            console.log('Msg_id: ' + res.msg_id)
        }
    });
```

[https://github.com/jpush/jpush-api-nodejs-client/blob/master/doc/api.md](https://github.com/jpush/jpush-api-nodejs-client/blob/master/doc/api.md)

setOptions 
image:
![image](https://github.com/ribuluo000/jpush-api-nodejs-client/blob/master/WX20171122-184957%402x.png)



re.
[https://github.com/jpush/jpush-react-native](https://github.com/jpush/jpush-react-native)

[https://github.com/jpush/jcore-react-native](https://github.com/jpush/jcore-react-native)

[http://www.jianshu.com/p/e7f81b5e1807](http://www.jianshu.com/p/e7f81b5e1807)

[http://www.jianshu.com/p/c2592540a335](http://www.jianshu.com/p/c2592540a335)

[http://www.open-open.com/lib/view/open1481162364413.html](http://www.open-open.com/lib/view/open1481162364413.html)



[![Build Status](https://travis-ci.org/jpush/jpush-api-nodejs-client.svg?branch=master)](https://travis-ci.org/jpush/jpush-api-nodejs-client)

本 SDK 提供 JPush 服务端接口的 Node 封装，与 JPush Rest API 组件通信。使用时引用该模块即可，可参考附带 Demo 学习使用方法。

[REST API 文档](http://docs.jiguang.cn/jpush/server/push/server_overview/)

[NodeJS API 文档](https://github.com/jpush/jpush-api-nodejs-client/blob/master/doc/api.md)


## Install
```
npm install jpush-sdk
#or
{
    "dependencies": {
        "jpush-sdk": "*"
    }
}
```

## Example
### Quick start
此 Demo 展示如何使用 Node lib 向所有用户推送通知。
``` js
var JPush = require("../lib/JPush/JPush.js")
var client = JPush.buildClient('your appKey', 'your masterSecret')

//easy push
client.push().setPlatform(JPush.ALL)
    .setAudience(JPush.ALL)
    .setNotification('Hi, JPush', JPush.ios('ios alert', 'happy', 5))
    .send(function(err, res) {
        if (err) {
            console.log(err.message)
        } else {
            console.log('Sendno: ' + res.sendno)
            console.log('Msg_id: ' + res.msg_id)
        }
    });
```

### Expert mode（高级版）

```js
client.push().setPlatform('ios', 'android')
    .setAudience(JPush.tag('555', '666'), JPush.alias('666,777'))
    .setNotification('Hi, JPush', JPush.ios('ios alert'), JPush.android('android alert', null, 1))
    .setMessage('msg content')
    .setOptions(null, 60)
    .send(function(err, res) {
        if (err) {
            console.log(err.message)
        } else {
            console.log('Sendno: ' + res.sendno)
            console.log('Msg_id: ' + res.msg_id)
        }
    });
```

关于 Payload 对象的方法，参考[详细 API 文档](https://github.com/jpush/jpush-api-nodejs-client/blob/master/doc/api.md)。

### 获取统计信息
本 Node lib 简易封装获取统计信息的接口，传入推送 API 返回的 msg_id 列表，多个 msg_id 用逗号隔开，最多支持 100 个 msg_id。
更多详细要求，请参考 [Report API 文档](https://docs.jiguang.cn/jpush/server/push/rest_api_v3_report/)。

```js
var JPush = require("../lib/JPush/JPush.js");
var client = JPush.buildClient('your appKey', 'your masterSecret');

client.getReportReceiveds('746522674,344076897', function(err, res) {
    if (err) {
        console.log(err.message)
    } else {
        for (var i = 0; i < res.length; i++) {
            console.log(res[i].android_received)
            console.log(res[i].ios_apns_sent)
            console.log(res[i].msg_id)
        }
    }
});
```

### 关闭 Log

```js
// 在构建 JPushClient 对象的时候, 指定 isDebug 参数。
var client = JPush.buildClient({
    appKey:'47a3ddda34b2602fa9e17c01',
    masterSecret:'d94f733358cca97b18b2cb98',
    isDebug:false
});
// or
var client = JPush.buildClient('47a3ddda34b2602fa9e17c01', 'd94f733358cca97b18b2cb98', null, false);
```

> 目前使用了 debug 模块来控制日志输出，若要查看 JPush 的相关日志信息，请先配置 DEBUG 环境变量 'jpush'。
