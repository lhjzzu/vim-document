## 技巧104 不用离开Vim也能编译代码

vim调用外部编译器，从而省去了离开编辑器的麻烦

### 准备工作

编译wakup.c文件

```
~/code/quickfix/wakeup
Makefile wakeup.c wakeup.h

```

1. `vim -u NONE -N wakeup.c`
2. `:make`

* vim除了会显示make命令的输出结果外，还会即系结果中的每一行内容，并把文件名，行号以及错误信息提取出来。
* 对于每一条出错信息，vim都会在quickfix列表中为其创建一项记录。
* `:make`,vim会自动跳转到第一处错误处。`:make!`，可使vim的光标位置保持不变。

## 技巧105 浏览Quickfix列表

### 浏览Quickfix列表的命令

```
命令               用途
:cnext            跳转到下一项
:cprev            跳转到上一项
:cfirst           跳转到第一项
:clast            跳转到最后一项
:cnfile           跳转到下一个文件中的第一项
:cpfile           跳转到下一个文件中的最后一项
:cc N             跳转到第n项
:copen            打开quickfix窗口
:cclose           关闭quickfix窗口

```

`:make`，`:grep`以及`:vimgrep`都会使用`quickfix`列表  

### Quickfix的快速前进/后退命令

1. 在跳转命令之前加上数字。例如`:{count}cnext`跳转`count`项
2. `cnfile`直接跳转到下一个文件

### 使用Quickfix窗口

1. `:copen` 打开quickfix窗口
2. `cclose`关闭quickfix窗口

## 技巧106 回溯以前的Quickfix列表

当我们更新quickfix列表时，vim并不会覆盖之前的内容，而好似将使用过的quickfix列表结果保存起来，方便我们回溯。

* `:colder`命令可以让我们回溯到quickfix列表之前的某个版本
* `:cnewer`命令可以让quickfix列表从旧版回到较新的版本
* `:colder`和`:cnewer`都支持次数

## 技巧107 定制外部编译器

Vim的`:make`命令不仅限于调用外部的make程序，也可以调用任何安装在你机器上的编译器。

???????????具体用到时，详细了解 


