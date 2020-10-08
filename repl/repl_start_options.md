<!-- YAML
added: v0.1.91
changes:
  - version: v13.4.0
    pr-url: https://github.com/nodejs/node/pull/30811
    description: The `preview` option is now available.
  - version: v12.0.0
    pr-url: https://github.com/nodejs/node/pull/26518
    description: The `terminal` option now follows the default description in
                 all cases and `useColors` checks `hasColors()` if available.
  - version: v10.0.0
    pr-url: https://github.com/nodejs/node/pull/19187
    description: The `REPL_MAGIC_MODE` `replMode` was removed.
  - version: v6.3.0
    pr-url: https://github.com/nodejs/node/pull/6635
    description: The `breakEvalOnSigint` option is supported now.
  - version: v5.8.0
    pr-url: https://github.com/nodejs/node/pull/5388
    description: The `options` parameter is optional now.
-->

* `options` {Object|string}
  * `prompt` {string} 要显示的输入提示符。**默认值:** `'> '`（末尾有一个空格）。
  * `input` {stream.Readable} REPL 输入要被读取的可读流。**默认值:** `process.stdin`。
  * `output` {stream.Writable} REPL 输出要被写入的可写流。**默认值:** `process.stdout`。
  * `terminal` {boolean} 如果为 `true`，则指定 `output` 应被当作一个 TTY 终端。
    **默认值:** 初始化时检查 `output` 流的 `isTTY` 属性的值。
  * `eval` {Function} 当解释每行输入时使用的函数。**默认值:** JavaScript `eval()` 函数的异步封装。
    `eval` 函数出错时会返回 `repl.Recoverable`，表明输入不完整并提示用户完成输入。
  * `useColors` {boolean} 如果为 `true`，则指定默认的 `writer` 函数可以在 REPL 输出中包含 ANSI 颜色风格。
    如果提供了自定义的 `writer` 函数，则该参数无效。
    **默认值:** 如果 REPL 实例的 `terminal` 值为 `true`，则检查 `output` 流上的颜色支持。
  * `useGlobal` {boolean} 如果为 `true`，则指定默认的解释函数使用 JavaScript `global` 作为上下文，而不是为 REPL 实例创建一个新的独立的上下文。
    在node命令行(node CLI)交互解释器中，这个值为 `true`。
    **默认值:** `false`。
  * `ignoreUndefined` {boolean} 如果为 `true`，则指定默认的输出器不会输出命令返回的 `undefined` 值。
     **默认值:** `false`。
  * `writer` {Function} 在写入到 `output` 之前，该函数被调用用来格式化每个命令的输出。
    **默认值:** [`util.inspect()`]。
  * `completer` {Function} 可选的函数，用来自定义 Tab 键的自动补全。
    详见 [`readline.InterfaceCompleter`]。
  * `replMode` {symbol} 一个标志位，指定默认的解释器使用严格模式或默认（sloppy）模式来执行 JavaScript 命令。
    可选的值有：
    * `repl.REPL_MODE_SLOPPY` 要使用默认模式解释表达式。
    * `repl.REPL_MODE_STRICT` 要使用严格模式解释表达式。该模式等同于在每个 repl 声明前加上 `'use strict'`。
  * `breakEvalOnSigint` {boolean} 当接收到 `SIGINT` 时停止解释当前代码，比如当按下 <kbd>Ctrl</kbd>+<kbd>C</kbd>。
    不能与自定义的 `eval` 函数同时使用。
    **默认值:** `false`。
  * `preview` {boolean} 定义 repl 是否打印自动补全并输出预览。
    **默认值**: 如果使用默认的 eval 函数，则为 `true`,如果使用自定义的 eval 函数，则为 `false`。 
    如果 `terminal` 为假，则没有预览，并且 `preview` 的值无效。
* 返回: {repl.REPLServer}

`repl.start()` 方法创建并启动一个 [`repl.REPLServer`] 实例。

如果 `options` 是一个字符串，则它指定了输入提示符：

```js
const repl = require('repl');

// 一个 Unix 风格的提示符。
repl.start('$ ');
```

