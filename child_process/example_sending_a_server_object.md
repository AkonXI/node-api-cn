
`sendHandle` 参数可用于将一个 TCP server 对象的句柄传给子进程，如以下示例所示：

```js
const subprocess = require('child_process').fork('subprocess.js');

// 打开 server 对象，并发送该句柄。
const server = require('net').createServer();
server.on('connection', (socket) => {
  socket.end('由父进程处理');
});
server.listen(1337, () => {
  subprocess.send('server', server);
});
```

子进程接收 server 对象如下：

```js
process.on('message', (m, server) => {
  if (m === 'server') {
    server.on('connection', (socket) => {
      socket.end('由子进程处理');
    });
  }
});
```

一旦服务器在父进程和子进程之间是共享的，则一些连接可被父进程处理，另一些可被子进程处理。

上面的示例使用了一个由 `net` 模块创建的服务器，虽然 `dgram` 模块的服务器使用完全相同的工作流程，但它监听 `'message'` 事件而不是 `'connection'` 事件，且使用 `server.bind()` 而不是 `server.listen()`。
目前仅在 Unix 平台上支持这一点。

