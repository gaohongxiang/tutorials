## 命令面板

命令面板是 VS Code 快捷键的主要交互界面。

通过命令面板可以快速搜索命令并且执行。可以摆脱鼠标，完全通过键盘操作来完成全部编码工作

打开命令面板快捷键： `F1` 或者 `Ctrl + Shift + P`

## VS Code快捷键

#### VS Code部件快捷键

命令面板 `F1` 或者 `Ctrl + Shift + P`

文件资源管理器（File explorer） `ctrl + shift + E`

跨文件搜索（Search across files）`ctrl + shift + F`

源代码管理（Source code management）`ctrl + shift + G`

启动和调试（Launch and debug）`ctrl + shift + D`

管理扩展（Manage extensions）`ctrl + shift + X`

#### 光标移动快捷键

光标在单词中间，跳转到单词的`最前面` | `最后面`: `ctrl + ←` | `ctrl + →`

光标以单词为单位横向不停移动：一直按 `ctrl + ←` | `ctrl + →`

光标移动到行首|行末：`Home` | `End`

光标在代码块的花括号之间移动：`ctrl + shift + \`

#### 文本选择快捷键

选中光标到单词`最前面` | `最后面`的文本: `ctrl + shift + ←` | `ctrl + shift + →`

#### 删除文本快捷键

删除一个单词内光标的前面|后面的文本：`ctrl + Backspace` | `ctrl + delete`

删除光标所在行文本：`shift + delete`

## 命令行

#### 打开文件或目录

在新窗口中打开文件或目录
```
code 文件名或目录
```
例：`code ~/test`

在现窗口中打开文件或目录
```
code -r 文件名或目录
```
例：`code -r ~/test`

在现窗口中打开文件或目录并滚动到指定行
```
code -r -g 文件名或目录:行数
```
例：`code -r -g ~/test:12`

**使用场景：**一个特别常见的例子就是当我们使用脚本执行某个命令，这个命令告诉我们某个文件的某一行出现了错误，我们就能够快速定位了。

#### 比较两个文件的内容

```
code -r 文件1 文件2
```
例：`code -r ~/test1.php ~/test2.php`

**使用场景：**1、使用命令行运行脚本 2、借助 VS Code 的图形化界面进行文件内容的对比。

#### 将在命令行中展示的内容实时的展示在VS Code里
```
ls | code -r -
```
**使用场景：**可以将展示的内容在编辑器中搜索和修改