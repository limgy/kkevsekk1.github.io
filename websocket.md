# websocket
` 4.2.8 新增 `
> 稳定性: 稳定

websocket模块，采用okhttp3 实现，本模块中包含了okhttp3 核心所有的类，下面讲解其基本的使用方法，更多的使用规则,可参考：(https://square.github.io/okhttp/)，这里涉及一些线程安全问题，请学习多线程，生命周期等内容。

## 创建websocket客户端
+ 创建一个http client，可以设定client是否重连，心跳等功能
+ 创建一个request 请求对象，采用什么协议ws 或wss 、服务器、端口都能内容
+ 设置监听，当websocket 生命周期内的一些事情。 
+ 设置上面的操作以后，打开链接，创建webSocket 客户端。
+ 用webSocket 客户端 发送消息 `webSocket.send("你好服务器")`;

``` js
importPackage(Packages["okhttp3"]); //导入包
var client = new OkHttpClient.Builder().retryOnConnectionFailure(true).build();
var request = new Request.Builder().url("ws://192.168.31.164:9317").build(); //vscode  插件的ip地址，
client.dispatcher().cancelAll();//清理一次
myListener = {
    onOpen: function (webSocket, response) {
        print("onOpen");
		//打开链接后，想服务器端发送一条消息
        var json = {};
        json.type="hello";
        json.data= {device_name:"模拟设备",client_version:123,app_version:123,app_version_code:"233"};
        var hello=JSON.stringify(json);
        webSocket.send(hello);
    },
    onMessage: function (webSocket, msg) { //msg可能是字符串，也可能是byte数组，取决于服务器送的内容
        print("msg");
        print(msg);
    },
    onClosing: function (webSocket, code, response) {
        print("正在关闭");
    },
    onClosed: function (webSocket, code, response) {
        print("已关闭");
    },
    onFailure: function (webSocket, t, response) {
        print("错误");
        print( t);
    }
}

var webSocket= client.newWebSocket(request, new WebSocketListener(myListener)); //创建链接

setInterval(() => { // 防止主线程退出
    
}, 1000);

```

创建 websocket 服务器端，一样支持，可以参考 okhttp 官网。

## 实例
```js
// 新建一个WebSocket
// 指定web socket的事件回调在当前线程（好处是没有多线程问题要处理，坏处是不能阻塞当前线程，包括死循环）
// 不加后面的参数则回调在IO线程
let ws = web.newWebSocket("wss://echo.websocket.org", {
    eventThread: 'this'
});
console.show();
// 监听他的各种事件
ws.on("open", (res, ws) => {
    log("WebSocket已连接");
}).on("failure", (err, res, ws) => {
    log("WebSocket连接失败");
    console.error(err);
}).on("closing", (code, reason, ws) => {
    log("WebSocket关闭中");
}).on("text", (text, ws) => {
    console.info("收到文本消息: ", text);
}).on("binary", (bytes, ws) => {
    console.info("收到二进制消息:");
    console.info("hex: ", bytes.hex());
    console.info("base64: ", bytes.base64());
    console.info("md5: ", bytes.md5());
    console.info("size: ", bytes.size());
    console.info("bytes: ", bytes.toByteArray());
}).on("closed", (code, reason, ws) => {
    log("WebSocket已关闭: code = %d, reason = %s", code, reason);
});

// 发送文本消息
log("发送消息: Hello, WebSocket!");
ws.send("Hello, WebSocket!");
setTimeout(() => {
    // 两秒后发送二进制消息
   log("发送二进制消息: 5piO5aSp5L2g6IO96ICDMTAw5YiG44CC");
   ws.send(web.ByteString.decodeBase64("5piO5aSp5L2g6IO96ICDMTAw5YiG44CC"));
}, 2000);
setTimeout(() => {
    // 8秒后断开WebSocket
    log("断开WebSocket");
    // 1000表示正常关闭
    ws.close(1000, null);
}, 8000);
setTimeout(() => {
    log("退出程序");
}, 12000)

```