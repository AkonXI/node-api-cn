<!-- YAML
added:
 - v11.13.0
 - v10.16.0
-->

* `count` {integer} 要求的颜色数（最小为 2）。**默认值:** 16。
* `env` {Object} 包含要检查的环境变量的对象。这使得能够模拟特定终端的使用。**默认值:** `process.env`。
* 返回: {boolean}

如果 `writeStream` 支持至少与 `count` 中提供的颜色数一样多，则返回 `true`。 
最小的支持为 2（黑色和白色）。

这也存在与 [`writeStream.getColorDepth()`] 中描述的相同的假正数或假负数。

```js
process.stdout.hasColors();
// 返回 true 或 false，取决于 `stdout` 是否支持至少 16 种颜色。
process.stdout.hasColors(256);
// 返回 true 或 false，取决于 `stdout` 是否支持至少 256 种颜色。
process.stdout.hasColors({ TMUX: '1' });
// 返回 true。
process.stdout.hasColors(2 ** 24, { TMUX: '1' });
// 返回 false (环境设置假设支持 2 ** 8 种颜色)。
```

