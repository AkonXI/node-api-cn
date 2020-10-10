<!-- YAML
added: v0.5.0
changes:
  - version: v14.9.0
    pr-url: https://github.com/nodejs/node/pull/34729
    description: This function is also available as `buf.readUint8()`.
  - version: v10.0.0
    pr-url: https://github.com/nodejs/node/pull/18395
    description: Removed `noAssert` and no implicit coercion of the offset
                 to `uint32` anymore.
-->

* `offset` {integer} 开始读取之前要跳过的字节数。必须满足：`0 <= offset <= buf.length - 1`。**默认值:** `0`。
* 返回: {integer}

从 `buf` 中指定的 `offset` 读取一个无符号的 8 位整数值。

```js
const buf = Buffer.from([1, -2]);

console.log(buf.readUInt8(0));
// 打印: 1
console.log(buf.readUInt8(1));
// 打印: 254
console.log(buf.readUInt8(2));
// 抛出异常 ERR_OUT_OF_RANGE。
```

