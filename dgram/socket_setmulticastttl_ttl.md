<!-- YAML
added: v0.3.8
-->

* `ttl` {integer}

设置 `IP_MULTICAST_TTL` 套接字选项。
一般来说，TTL 表示"生存时间"。
这里特指一个 IP 数据包传输时允许的最大跳步数，尤其是对多播传输。
当 IP 数据包每向前经过一个路由或网关时，TTL 值减 1，若经过某个路由时，TTL 值被减至 0，便不再继续向前传输。

`ttl` 参数可以是 `0` 到 `255` 之间。
在大多数系统上，默认值是 `1`。

This method throws `EBADF` if called on an unbound socket.
