<!-- YAML
added: v5.10.0
changes:
  - version: v10.0.0
    pr-url: https://github.com/nodejs/node/pull/12562
    description: 参数 `callback` 不再是可选的。 
      如果不传入，则在运行时会抛出 `TypeError`。
  - version: v7.0.0
    pr-url: https://github.com/nodejs/node/pull/7897
    description: 参数 `callback` 不再是可选的。 
      如果不传入，则会触发弃用警告（id 为 DEP0013）。
  - version: v6.2.1
    pr-url: https://github.com/nodejs/node/pull/6828
    description: The `callback` parameter is optional now.
-->

* `prefix` {string}
* `options` {string|Object}
  * `encoding` {string} **默认值:** `'utf8'`。
* `callback` {Function}
  * `err` {Error}
  * `directory` {string}

创建一个唯一的临时目录。

生成要附加在必需的 `prefix` 后面的六位随机字符，以创建唯一的临时目录。
由于平台的不一致性，请避免在 `prefix` 中以 `X` 字符结尾。 
在某些平台上，特别是 BSD，可以返回六个以上的随机字符，并用随机字符替换 `prefix` 中结尾的 `X` 字符。

创建的目录路径作为字符串传给回调的第二个参数。

可选的 `options` 参数可以是指定字符编码的字符串，也可以是具有指定要使用的字符编码的 `encoding` 属性的对象。


```js
fs.mkdtemp(path.join(os.tmpdir(), '目录-'), (err, directory) => {
  if (err) throw err;
  console.log(directory);
  // 打印: /tmp/目录-itXde2 或 C:\Users\...\AppData\Local\Temp\目录-itXde2
});
```

`fs.mkdtemp()` 方法将六位随机选择的字符直接附加到 `prefix` 字符串。
例如，给定目录 `/tmp`，如果打算在 `/tmp` 中创建临时目录，则 `prefix` 必须在尾部加上特定平台的路径分隔符（`require('path').sep`）。

```js
// 新的临时目录的父目录。
const tmpDir = os.tmpdir();

// 此用法是错误的：
fs.mkdtemp(tmpDir, (err, directory) => {
  if (err) throw err;
  console.log(directory);
  // 输出类似 `/tmpabc123`。
  // 新的临时目录会被创建在文件系统根目录，而不是在 /tmp 目录中。
});

// 此用法是正确的：
const { sep } = require('path');
fs.mkdtemp(`${tmpDir}${sep}`, (err, directory) => {
  if (err) throw err;
  console.log(directory);
  // 输出类似 `/tmp/abc123`。
  // 新的临时目录会被创建在 /tmp 目录中。
});
```

