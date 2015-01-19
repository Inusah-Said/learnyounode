编写一个 HTTP **服务器**，它可以对每个收到的请求响应相应的文本文件。

你的服务器需要监听由第一个命令行参数所制定的端口。

同时，第二个会提供给你的程序的参数则是所需要响应的文本文件的位置。你**必须**使用 `fs.createReadStream()` 方法以 stream 的形式作出请求相应。

----------------------------------------------------------------------
## 提示

因为我们需要创建一个 HTTP 服务而不是普通的 TCP 服务来完成这个练习，因此，你应该使用 `http` 这个 Node 核心模块。和 `net` 模块类似，`http` 模块拥有一个叫做 `http.createServer()` 的方法，不过它所创建的是一个可以用 HTTP 通信的服务器。

`http.createServer()` 接收一个回调函数作为参数，回调函数会在你的服务器每一次进行连接的时候执行，这个回调函数有以下的特征：

```js
function callback (request, response) { /* ... */ }
```

在这里，这两个参数是代表一个 HTTP 请求以及相应的响应的两个对象。`request` 用来从请求中获取一些的属性，例如请求头和查询字符（query-string)，而 `response` 会发送数据给客户端，包括响应头部和响应主体。

`request` 和 `response` 也都是 Node stream！这意味着，如果需要的话，你可以使用流式处理（streaming）的那些抽象方法来实现发送和接收数据。

`http.createServer()` 会返回一个 HTTP 服务器的实例。你必须调用 `server.listen(portNumber)` 方法去监听一个特定的端口。

一个典型的 Node HTTP 服务器将会是这个样子：

```js
var http = require('http')
var server = http.createServer(function (req, res) {
  // 处理请求的逻辑...
})
server.listen(8000)
```

`http` 模块的文档你可以使用浏览器访问如下路径来查看：
  {rootdir:/node_apidoc/http.html}

`fs` 这个核心模块也含有一些 stream API 用来处理文件。你需要使用 `fs.createReadStream()` 方法来为命令行参数指定的文件创建一个 stream。这个方法会返回一个 stream 对象，这个对象可以使用类似 `src.pipe(dst)` 的方法把数据从 `src`流传输(pipe) 到 `dst`流中。通过这种形式，你可以轻松地把一个文件系统的 stream 和一个 HTTP 响应的 stream 连接起来。

----------------------------------------------------------------------