<!-- YAML
added: v0.1.91
changes:
  - version: v8.8.0
    pr-url: https://github.com/nodejs/node/pull/15380
    description: 支持 `windowsHide` 选项。
-->

* `file` {string} 要运行的可执行文件的名称或路径。
* `args` {string[]} 字符串参数的列表。
* `options` {Object}
  * `cwd` {string} 子进程的当前工作目录。
  * `env` {Object} 环境变量的键值对。
    **默认值:** `process.env`。
  * `encoding` {string} **默认值:** `'utf8'`。
  * `timeout` {number} **默认值:** `0`。
  * `maxBuffer` {number} stdout 或 stderr 上允许的最大数据量（以字节为单位）。
    如果超过限制，则子进程会被终止，并且输出会被截断。
    参见 [maxBuffer 和 Unicode][`maxBuffer` and Unicode] 的注意事项。
    **默认值:** `1024 * 1024`。
  * `killSignal` {string|integer} **默认值:** `'SIGTERM'`。
  * `uid` {number} 设置进程的用户标识，参见 setuid(2)。
  * `gid` {number} 设置进程的群组标识，参见 setgid(2)。
  * `windowsHide` {boolean} 隐藏子进程的控制台窗口（在 Windows 系统上通常会创建）。
    **默认值:** `false`。
  * `windowsVerbatimArguments` {boolean} 在 Windows 上不为参数加上引号或转义。
    在 Unix 上会被忽略。
    **默认值:** `false`。
  * `shell` {boolean|string} 如果为 `true`，则在 shell 中运行 `command`。
     在 Unix 上使用 `'/bin/sh'`，在 Windows 上使用 `process.env.ComSpec`。
     可以将不同的 shell 指定为字符串。
     参见 [shell 的要求][Shell requirements]和[默认的 Windows shell][Default Windows shell]。
     **默认值:** `false`（没有 shell）。
* `callback` {Function} 当进程终止时调用并传入输出。
  * `error` {Error}
  * `stdout` {string|Buffer}
  * `stderr` {string|Buffer}
* 返回: {ChildProcess}

`child_process.execFile()` 函数类似于 [`child_process.exec()`]，但默认情况下不会衍生 shell。
指定的可执行文件 `file` 会被直接衍生作为新的进程，使其比 [`child_process.exec()`] 稍微更高效。

支持与 [`child_process.exec()`] 相同的选项。
由于没有衍生 shell，因此不支持 I/O 重定向和文件通配等行为。


```js
const { execFile } = require('child_process');
const child = execFile('node', ['--version'], (error, stdout, stderr) => {
  if (error) {
    throw error;
  }
  console.log(stdout);
});
```

传给回调的 `stdout` 和 `stderr` 参数会包含子进程的 stdout 和 stderr 输出。
默认情况下，Node.js 会将输出解码为 UTF-8 并将字符串传给回调。
`encoding` 选项可用于指定字符编码（用于解码 stdout 和 stderr 输出）。
如果 `encoding` 是 `'buffer'` 或无法识别的字符编码，则传给回调的会是 `Buffer` 对象。

如果调用此方法的 [`util.promisify()`] 版本，则返回 `Promise`（会传入具有 `stdout` 和 `stderr` 属性的 `Object`）。
返回的 `ChildProcess` 实例会作为 `child` 属性附加到 `Promise`。
如果出现错误（包括导致退出码不为 0 的任何错误），则返回 reject 的 promise，并传入与回调中相同的 `error` 对象，但是还有两个额外的属性 `stdout` 和 `stderr`。

```js
const util = require('util');
const execFile = util.promisify(require('child_process').execFile);
async function getVersion() {
  const { stdout } = await execFile('node', ['--version']);
  console.log(stdout);
}
getVersion();
```

**如果启用了 `shell` 选项，则不要将未经过处理的用户输入传给此函数。
包含 shell 元字符的任何输入都可用于触发任意命令的执行。**

