<!-- YAML
added: v0.5.0
changes:
  - version: v10.0.0
    pr-url: https://github.com/nodejs/node/pull/18395
    description: Removed `noAssert` and no implicit coercion of the offset
                 to `uint32` anymore.
-->

* `value` {integer} 要写入 `buf` 的数值。
* `offset` {integer} 开始写入之前要跳过的字节数。必须满足：`0 <= offset <= buf.length - 1`。**默认值:** `0`。
* 返回: {integer} `offset` 加上已写入的字节数。

将 `value` 写入到 `buf` 中指定的 `offset` 位置。
`value` 必须是有符号的 8 位整数。
当 `value` 不是有符号的 8 位整数时，行为是未定义的。

`value` 会被解析并写入为二进制补码的有符号整数。

```js
const buf = Buffer.allocUnsafe(2);

buf.writeInt8(2, 0);
buf.writeInt8(-2, 1);

console.log(buf);
// 打印: <Buffer 02 fe>
```

