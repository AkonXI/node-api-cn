
`path` 模块的默认操作会因 Node.js 应用程序运行所在的操作系统而异。
具体来说，当在 Windows 操作系统上运行时，`path` 模块会假定正被使用的是 Windows 风格的路径。

因此，使用 `path.basename()` 可能会在 POSIX 和 Windows 上产生不同的结果：

在 POSIX 上:

```js
path.basename('C:\\temp\\myfile.html');
// 返回: 'C:\\temp\\myfile.html'
```

在 Windows 上:

```js
path.basename('C:\\temp\\myfile.html');
// 返回: 'myfile.html'
```

如果要在任意操作系统上使用 Windows 文件路径时获得一致的结果，则使用 [`path.win32`]：

在 POSIX 和 Windows 上:

```js
path.win32.basename('C:\\temp\\myfile.html');
// 返回: 'myfile.html'
```

如果要在任意操作系统上使用 POSIX 文件路径时获得一致的结果，则使用 [`path.posix`]：

在 POSIX 和 Windows 上:

```js
path.posix.basename('/tmp/myfile.html');
// 返回: 'myfile.html'
```

在 Windows 上，Node.js 遵循独立驱动器工作目录的概念。
当使用没有反斜杠的驱动器路径时，可以观察到此行为。
例如，`path.resolve('C:\\')` 可能会返回与 `path.resolve('C:')` 不同的结果。
详见[此 MSDN 页面][MSDN-Rel-Path]。

