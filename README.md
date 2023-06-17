# 两种方式使用 whatsapp GCM 服务
# 1) 可以直接使用demo(WhatsappGCMDemo.html)
   ## 把 WhatsappGCMDemo.html 下载到本地，双击打开
# 2) WhatsappGCM
### whatsapp 用来注册，接收服务器的消息推送
#  websocket 连接
````js
const WebSocket = require('ws');

const url = 'ws://47.243.17.172:8080/websocket';
//const url = 'ws://127.0.0.1:8080/websocket';
const socket = new WebSocket(url);

socket.on('open', () => {
  console.log('connect to server');
});

socket.on('message', (msg) => {
  console.log('receive msg:\n', msg.toString('utf-8'));

});

socket.on('close', () => {
  console.log('disconnect');
});

socket.on('error', (err) => {
  console.error('error:', err);
});

````
#  GCM 注册
````js
//websocket 连接 成功之后，发送注册命令
//代理仅支持 http 代理
socket.send('{"command" : "register", "proxy_server" : "127.0.0.1", "proxy_port" : 1080}');

//发送完成之后，会收到注册成功回调
{
	"code": 0,
	"command": "register",
	"desc": "success",
	"token": "xxxxxxx"
}
````

#  发送XMPP GCM 包
````xml
//token 字段 设置为 GCM 注册返回的token
<iq id='1' xmlns='urn:xmpp:whatsapp:push' type='set' to='s.whatsapp.net'>
	<config id="token" platform="gcm" />
</iq>

````

#  获取 pn
````js
//发送 websocket 登录命令， 等待接收 服务器推送的pn
socket.send('{"command" : "login", "token" : "token"}');


//接收到的pn
{
	"code": 0,
	"command": "pn",
	"pn": "xxxxxxx"
}
````

# 发送XMPP Pn 命令

````xml
<iq id='1' xmlns='urn:xmpp:whatsapp:push' type='get' to='s.whatsapp.net'>
	<pn/>
        xxxxxxxx
    </pn>
</iq>

````