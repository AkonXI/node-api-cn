<!-- YAML
added: v0.6.9
-->

* `flag` {boolean}

设置或清除 `SO_BROADCAST` socket 选项。
当设置为 `true`, UDP 包可能会被发送到一个本地接口的广播地址。

This method throws `EBADF` if called on an unbound socket.
