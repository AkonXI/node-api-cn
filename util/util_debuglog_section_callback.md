<!-- YAML
added: v0.11.3
-->

* `section` {string} 一个字符串，指定要为应用的哪些部分创建 `debuglog` 函数。
* `callback` {Function} A callback invoked the first time the logging function
is called with a function argument that is a more optimized logging function.
* 返回: {Function} 日志函数。

`util.debuglog()` 方法用于创建一个函数，基于 `NODE_DEBUG` 环境变量的存在与否有条件地写入调试信息到 `stderr`。
如果 `section` 名称在环境变量的值中，则返回的函数类似于 [`console.error()`]。
否则，返回的函数是一个空操作。

```js
const util = require('util');
const debuglog = util.debuglog('foo');

debuglog('hello from foo [%d]', 123);
```

如果程序在环境中运行时带上 `NODE_DEBUG=foo`，则输出类似如下： 

```console
FOO 3245: hello from foo [123]
```

其中 `3245` 是进程 id。
如果运行时没带上环境变量集合，则不会打印任何东西。

`section` 还支持通配符：

```js
const util = require('util');
const debuglog = util.debuglog('foo-bar');

debuglog('hi there, it\'s foo-bar [%d]', 2333);
```

如果在环境中使用 `NODE_DEBUG=foo*` 运行，那么它将输出如下内容：

```console
FOO-BAR 3257: hi there, it's foo-bar [2333]
```

`NODE_DEBUG` 环境变量中可指定多个由逗号分隔的 `section` 名称。
例如：`NODE_DEBUG=fs,net,tls`。

The optional `callback` argument can be used to replace the logging function
with a different function that doesn't have any initialization or
unnecessary wrapping.

```js
const util = require('util');
let debuglog = util.debuglog('internals', (debug) => {
  // Replace with a logging function that optimizes out
  // testing if the section is enabled
  debuglog = debug;
});
```



