# websocket

## 背景

**websocket 解决 HTTP 协议的缺陷：通信只能由客服端发起**

在没有 websocket 之前：采用轮询，效率低，浪费资源（因为必须不停连接，或者 HTTP 连接始终打开）

## WebSocket 协议在 2008 年诞生，2011 年成为国际标准。所有浏览器都已经支持了。

## 特点

1. 建立在 TCP 协议之上，服务器端的实现比较容易。
2. 与 HTTP 协议有着良好的兼容性。默认端口也是 80 和 443，并且握手阶段采用 HTTP 协议，因此在握手时不容易屏蔽，能通过各种 HTTP 代理服务器。
3. 数据格式比较轻量，性能开销小，通信高效。
4. 可以发送文本，也可以发送二进制数据。
5. 没有同源限制，客户端可以与任意服务器通信。
6. 协议标识符是`ws`（如果加密，则为`wss`），服务器网址就是`URL`。

## websocket 用法

```javascript
var ws = new WebSocket('wss://echo.websocket.org')

ws.onopen = function (evt) {
	console.log('Connection open ...')
	ws.send('Hello WebSockets!')
}

ws.onmessage = function (evt) {
	console.log('Received Message: ' + evt.data)
	ws.close()
}

ws.onclose = function (evt) {
	console.log('Connection closed.')
}
```

## 客户端的 API

### WebSocket 构造函数

WebSocket 对象作为一个构造函数，用于新建 WebSocket 实例。

```javascript
var ws = new WebSocket('wss://echo.websocket.org')
```

### webSocket.readyState

`readyState`属性返回实例对象的当前状态，共有四种:

- `CONNECTING`：值为 0，表示正在连接。
- `OPEN`：值为 1，表示连接成功，可以通信了。
- `CLOSING`：值为 2，表示连接正在关闭。
- `CLOSED`：值为 3，表示连接已经关闭，或者打开连接失败。

```javascript
switch (ws.readyState) {
	case WebSocket.CONNECTING:
		// do something
		break
	case WebSocket.OPEN:
		// do something
		break
	case WebSocket.CLOSING:
		// do something
		break
	case WebSocket.CLOSED:
		// do something
		break
	default:
		// this never happens
		break
}
```

### webSocket.onopen

实例对象的`onopen`属性，用于指定连接成功后的回调函数。

### webSocket.onclose

实例对象的`onclose`属性，用于指定连接关闭后的回调函数。

### webSocket.onmessage

实例对象的`onmessage`属性，用于指定收到服务器数据后的回调函数。
需要注意，服务器数据可能是文本，也可能是二进制数据（`blob`对象或`Arraybuffer`对象）。

### webSocket.send()

实例对象的`send()`方法用于向服务器发送数据。

### webSocket.bufferedAmount

实例对象的`bufferedAmount`属性，表示还有多少字节的二进制数据没有发送出去。它可以用来判断发送是否结束。

### webSocket.onerror

实例对象的`onerror`属性，用于指定报错时的回调函数。
