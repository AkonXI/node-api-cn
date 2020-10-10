<!-- YAML
added: v12.12.0
changes:
  - version:
     - v13.1.0
     - v12.16.0
    pr-url: https://github.com/nodejs/node/pull/30114
    description: The `bufferSize` option was introduced.
-->

* `path` {string|Buffer|URL}
* `options` {Object}
  * `encoding` {string|null} **默认值:** `'utf8'`。
  * `bufferSize` {number} 当从目录读取时在内部缓冲的目录项的数量。值越高，则性能越好，但内存占用更高。**默认值:** `32`。
* 返回: {fs.Dir}

同步地打开目录。 
参见 opendir(3)。

创建一个 [`fs.Dir`]，其中包含所有用于更进一步读取和清理目录的函数。

`encoding` 选项用于在打开目录和后续的读取操作时设置 `path` 的字符编码。


