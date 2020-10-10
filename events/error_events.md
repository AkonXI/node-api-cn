
当 `EventEmitter` 实例出错时，应该触发 `'error'` 事件。
这些在 Node.js 中被视为特殊情况。

如果没有为 `'error'` 事件注册监听器，则当 `'error'` 事件触发时，会抛出错误、打印堆栈跟踪、并退出 Node.js 进程。

```js
const myEmitter = new MyEmitter();
myEmitter.emit('error', new Error('错误信息'));
// 抛出错误并使 Node.js 崩溃。
```

为了防止崩溃 Node.js 进程，可以使用 [`domain`] 模块。
（但请注意，不推荐使用 `domain` 模块。）

作为最佳实践，应该始终为 `'error'` 事件注册监听器。

```js
const myEmitter = new MyEmitter();
myEmitter.on('error', (err) => {
  console.error('错误信息');
});
myEmitter.emit('error', new Error('错误'));
// 打印: 错误信息
```

通过使用符号 `errorMonitor` 安装监听器，可以监视 `'error'` 事件但不消耗触发的错误。

```js
const myEmitter = new MyEmitter();
myEmitter.on(EventEmitter.errorMonitor, (err) => {
  MyMonitoringTool.log(err);
});
myEmitter.emit('error', new Error('错误'));
// 仍然抛出错误并使 Node.js 崩溃。
```
