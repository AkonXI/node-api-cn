<!-- YAML
added: v10.0.0
changes:
  - version: v14.12.0
    pr-url: https://github.com/nodejs/node/pull/34993
    description: The `string` parameter will stringify an object with an
                 explicit `toString` function.
  - version: v14.0.0
    pr-url: https://github.com/nodejs/node/pull/31030
    description: The `string` parameter won't coerce unsupported input to
                 strings anymore.
-->

* `string` {string|Object}
* `position` {integer}
* `encoding` {string} **默认值:** `'utf8'`。
* 返回: {Promise}

将 `string` 写入到文件。
如果 `string` 不是字符串或具有自有 `toString` 函数属性的对象，则抛出异常。


`Promise` 会被解决并带上一个对象，对象包含一个 `bytesWritten` 属性（指定写入的字节数）和一个 `buffer` 属性（指向写入的 `string`）。

`position` 指定文件开头的偏移量（数据要被写入的位置）。
如果 `position` 的类型不是一个 `number`，则数据会被写入当前的位置。
参见 pwrite(2)。

`encoding` 是期望的字符串编码。

在同一个文件上多次使用 `filehandle.write()` 且不等待 `Promise` 被解决（或被拒绝）是不安全的。
对于这种情况，建议使用 [`fs.createWriteStream()`]。

在 Linux 上，当以追加模式打开文件时，则写入时无法指定位置。
内核会忽略位置参数，并始终追加数据到文件的末尾。

