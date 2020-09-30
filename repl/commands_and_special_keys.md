
所有 REPL 的实例都支持下列特殊命令：

* `.break` - 在输入一个多行表达式的过程中，输入 `.break` 命令（或按下 **Ctrl+C**）将终止表达式的继续输入。
* `.clear` - 重置 REPL 的 `context` 为一个空对象，并清除正在输入中的所有多行表达式。
* `.exit` - 关闭输入输出流，退出 REPL。
* `.help` - 显示特定命令的帮助列表。
* `.save` - 保存当前 REPL 会话到一个文件：
  `> .save ./file/to/save.js`
* `.load` - 读取一个文件到当前 REPL 会话。
  `> .load ./file/to/load.js`
* `.editor` 进入编辑模式（**Ctrl+D** 完成，**Ctrl+C** 取消）

```console
> .editor
// 进入编辑模式（^D 完成，^C 取消）
function welcome(name) {
  return `你好 ${name}！`;
}

welcome('Node.js 用户');

// ^D
'你好 Node.js 用户！'
>
```

REPL 中下列按键组合有特殊作用：

* **Ctrl+C**: 当按下一次时，与 `.break` 命令的效果一样。当在空白行按下两次时，与 `.exit` 命令的效果一样。
* **Ctrl+D**: 与 `.exit` 命令的效果一样。
* `<tab>`: 当在空白行按下时，显示全局和本地作用域内的变量。当在输入时按下，显示相关的自动补全选项。

有关与反向i搜索相关的快捷键，请参见[反向i搜索][`reverse-i-search`]。 
有关所有的其他快捷键，请参见 [TTY 快捷键][TTY keybindings]。

