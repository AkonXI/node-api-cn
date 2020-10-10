在给定的套接字上启动一个连接。

可能的签名：

* [`socket.connect(options[, connectListener])`][`socket.connect(options)`]
* [`socket.connect(path[, connectListener])`][`socket.connect(path)`] 用于 [IPC][] 连接。
* [`socket.connect(port[, host][, connectListener])`][`socket.connect(port)`] 用于 TCP 连接。
* 返回: {net.Socket} socket 自身。

该方法是异步的。当连接建立了的时候，[`'connect'`][] 事件将会被触发。如果连接过程中有问题，[`'error'`][] 事件将会代替 [`'connect'`][] 事件被触发，并将错误信息传递给 [`'error'`][] 监听器。
最后一个参数 `connectListener`，如果指定了，将会被添加为 [`'connect'`][] 事件的监听器。

This function should only be used for reconnecting a socket after
`'close'` has been emitted or otherwise it may lead to undefined
behavior.
