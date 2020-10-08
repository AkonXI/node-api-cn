<!-- YAML
added: v0.1.0
-->

* `socket` {stream.Duplex}

建立新的 TCP 流时会触发此事件。 
`socket` 通常是 [`net.Socket`] 类型的对象。 
通常用户无需访问此事件。 
特别是，由于协议解析器附加到套接字的方式，套接字将不会触发 `'readable'` 事件。 
也可以通过 `request.socket` 访问 `socket`。

用户也可以显式触发此事件，以将连接注入 HTTP 服务器。 
在这种情况下，可以传入任何 [`Duplex`] 流。

如果在此处调用 `socket.setTimeout()`，则当套接字已提供请求时（如果 `server.keepAliveTimeout` 为非零），超时将会被 `server.keepAliveTimeout` 替换。

此事件保证传入 {net.Socket} 类（{stream.Duplex} 的子类）的实例，除非用户指定了 {net.Socket} 以外的套接字类型。

