<!-- YAML
added: v8.2.0
-->

* `object` {Object} 支持 `Symbol.toPrimitive` 或 `valueOf()` 的对象。
* `offsetOrEncoding` {integer|string} 字节偏移量或字符编码。
* `length` {integer} 长度。

对于 `valueOf()` 返回值不严格等于 `object` 的对象，返回 `Buffer.from(object.valueOf(), offsetOrEncoding, length)`。

```js
const buf = Buffer.from(new String('this is a test'));
// 打印: <Buffer 74 68 69 73 20 69 73 20 61 20 74 65 73 74>
```

对于支持 `Symbol.toPrimitive` 的对象，会返回 `Buffer.from(object[Symbol.toPrimitive]('string'), offsetOrEncoding)`。

```js
class Foo {
  [Symbol.toPrimitive]() {
    return 'this is a test';
  }
}

const buf = Buffer.from(new Foo(), 'utf8');
// 打印: <Buffer 74 68 69 73 20 69 73 20 61 20 74 65 73 74>
```

如果 `object` 没有提及的方法、或适用于 `Buffer.from()` 变量的其他类型，则抛出 `TypeError`。

