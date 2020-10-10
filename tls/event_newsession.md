<!-- YAML
added: v0.9.2
changes:
  - version: v0.11.12
    pr-url: https://github.com/nodejs/node-v0.x-archive/pull/7118
    description: The `callback` argument is now supported.
-->

`'newSession'` 事件在创建一个新的 TLS 会话时触发。
这可能用于在外部存储保存会话。
数据会被提供给 [`'resumeSession'`] 回调。

监听器回调被调用时传入三个参数：

* `sessionId` {Buffer} TLS 会话识别符。
* `sessionData` {Buffer} TLS 会话数据。
* `callback` {Function} 在安全连接时为了发送或者接收数据，无参的回调函数必须被调用。

添加监听器后，监听器只在连接建立后生效。

