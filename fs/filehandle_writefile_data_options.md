<!-- YAML
added: v10.0.0
changes:
  - version: v14.12.0
    pr-url: https://github.com/nodejs/node/pull/34993
    description: The `data` parameter will stringify an object with an
                 explicit `toString` function.
  - version: v14.0.0
    pr-url: https://github.com/nodejs/node/pull/31030
    description: 参数 `data` 不再强制转换不支持的输入为字符串。
-->

* `data` {string|Buffer|Uint8Array|Object}
* `options` {Object|string}
  * `encoding` {string|null} **默认值:** `'utf8'`。
* 返回: {Promise}


异步地将数据写入到一个文件，如果文件已存在则覆盖该文件。
`data` 可以是字符串、buffer、或具有自有 `toString` 函数属性的对象。
`Promise` 会在成功时被解决，且不带参数。

如果 `data` 是 buffer，则 `encoding` 选项会被忽略。

如果 `options` 是字符串，则它指定字符编码。

`FileHandle` 必须支持写入。

在同一个文件上多次使用 `filehandle.writeFile()` 且不等待 `Promise` 被解决（或被拒绝）是不安全的。

如果对文件句柄进行了一次或多次 `filehandle.write()` 调用，然后再调用 `filehandle.writeFile()`，则将从当前位置写入数据，直到文件结束。 
它并不总是从文件的开头写入。

