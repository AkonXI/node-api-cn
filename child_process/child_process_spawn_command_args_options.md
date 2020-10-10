<!-- YAML
added: v0.1.90
changes:
  - version:
      - v13.2.0
      - v12.16.0
    pr-url: https://github.com/nodejs/node/pull/30162
    description: 支持 `serialization` 选项。
  - version: v8.8.0
    pr-url: https://github.com/nodejs/node/pull/15380
    description: 支持 `windowsHide` 选项。
  - version: v6.4.0
    pr-url: https://github.com/nodejs/node/pull/7696
    description: 支持 `argv0` 选项。
  - version: v5.7.0
    pr-url: https://github.com/nodejs/node/pull/4598
    description: 支持 `shell` 选项。
-->

* `command` {string} 要运行的命令。
* `args` {string[]} 字符串参数的列表。
* `options` {Object}
  * `cwd` {string} 子进程的当前工作目录。
  * `env` {Object} 环境变量的键值对。
    **默认值:** `process.env`。
  * `argv0` {string} 显式地设置发送给子进程的 `argv[0]` 的值。
    如果没有指定，则会被设置为 `command` 的值。
  * `stdio` {Array|string} 子进程的 stdio 配置，参见 [`options.stdio`][`stdio`]。
  * `detached` {boolean} 使子进程独立于其父进程运行。
    具体行为取决于平台，参见 [`options.detached`]。
  * `uid` {number} 设置进程的用户标识，参见 setuid(2)。
  * `gid` {number} 设置进程的群组标识，参见 setgid(2)。
  * `serialization` {string} 指定用于在进程之间发送消息的序列化类型。
    可能的值为 `'json'` 和 `'advanced'`。
    详见[高级序列化][Advanced serialization]。
    **默认值:** `'json'`。
  * `shell` {boolean|string} 如果为 `true`，则在 shell 中运行 `command`。
    在 Unix 上使用 `'/bin/sh'`，在 Windows 上使用 `process.env.ComSpec`。
    可以将不同的 shell 指定为字符串。
    参见 [shell 的要求][Shell requirements]和[默认的 Windows shell][Default Windows shell]。
    **默认值:** `false`（没有 shell）。
  * `windowsVerbatimArguments` {boolean} 在 Windows 上不为参数加上引号或转义。
    在 Unix 上会被忽略。
    如果指定了 `shell` 并且是 CMD，则自动设为 `true`。
    **默认值:** `false`。
  * `windowsHide` {boolean} 隐藏子进程的控制台窗口（在 Windows 系统上通常会创建）。
    **默认值:** `false`。
* 返回: {ChildProcess}

`child_process.spawn()` 方法使用给定的 `command` 衍生新的进程，并传入 `args` 中的命令行参数。
如果省略 `args`，则其默认为空数组。

**如果启用了 `shell` 选项，则不要将未经过处理的用户输入传给此函数。
包含 shell 元字符的任何输入都可用于触发任意命令的执行。**

第三个参数可用于指定额外的选项，具有以下默认值：

```js
const defaults = {
  cwd: undefined,
  env: process.env
};
```

使用 `cwd` 指定衍生进程的工作目录。
如果没有给定，则默认为继承当前工作目录。

使用 `env` 指定新进程可见的环境变量，默认为 [`process.env`]。

`env` 中的 `undefined` 值会被忽略。

示例，运行 `ls -lh /usr`，并捕获 `stdout`、`stderr`、以及退出码：

```js
const { spawn } = require('child_process');
const ls = spawn('ls', ['-lh', '/usr']);

ls.stdout.on('data', (data) => {
  console.log(`stdout: ${data}`);
});

ls.stderr.on('data', (data) => {
  console.error(`stderr: ${data}`);
});

ls.on('close', (code) => {
  console.log(`子进程退出，退出码 ${code}`);
});
```

示例，以非常精细的方式运行 `ps ax | grep ssh`：

```js
const { spawn } = require('child_process');
const ps = spawn('ps', ['ax']);
const grep = spawn('grep', ['ssh']);

ps.stdout.on('data', (data) => {
  grep.stdin.write(data);
});

ps.stderr.on('data', (data) => {
  console.error(`ps 的 stderr: ${data}`);
});

ps.on('close', (code) => {
  if (code !== 0) {
    console.log(`ps 进程退出，退出码 ${code}`);
  }
  grep.stdin.end();
});

grep.stdout.on('data', (data) => {
  console.log(data.toString());
});

grep.stderr.on('data', (data) => {
  console.error(`grep 的 stderr: ${data}`);
});

grep.on('close', (code) => {
  if (code !== 0) {
    console.log(`grep 进程退出，退出码 ${code}`);
  }
});
```

示例，检查失败的 `spawn`：

```js
const { spawn } = require('child_process');
const subprocess = spawn('错误的命令');

subprocess.on('error', (err) => {
  console.error('启动子进程失败');
});
```

一些平台（macOS、Linux）会使用 `argv[0]` 的值作为进程的标题，而其他平台（Windows、SunOS）则使用 `command`。

Node.js 在启动时会使用 `process.execPath` 重写 `argv[0]`，因此 Node.js 子进程的 `process.argv[0]` 不会匹配从父进程传给 `spawn` 的 `argv0` 参数，可以使用 `process.argv0` 属性获取。


