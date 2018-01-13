## 技巧118 对你的工作进行拼写检查

* 当拼写检查器被启用后，Vim会对所有未在拼写文件中出现过的单词进行标记。
    + `:set spell` 进行拼写检查，拼写错误的单词会被标记出来

    
### 操作Vim的拼写检查器

```
命令         用途
]s          跳到下一处拼写错误
[s          跳到上一处拼写错误
z=          为当前单词提供更正建议
zg          把当前单词添加到拼写文件中
zw          把当前单词从拼写文件中删除
zug         测下哦针对当前单词的zg,zw命令
```
操作命令是按命令字符的先后顺序键入。例如`]s`不是同时按，是先按`]`在按`s`。


## 技巧119 使用其他拼写字典

Vim的拼写检查器本身就支持英语的区域性变体。我们可以指定区域或其他语言

### 指定某个语言的区域性变体

* vim缺省设置`spelllang=en`，意味着所有被英语为母语的地区所认可的单词都是合法的. 例如`moustache`(英式)和`mustache`(美式)
* `spelllang `选项并不是全的，它永远只在本地缓冲区中有效。
*  指定Vim只接受美式拼法:
 + `:set spell`
 + `:set spelllang=en_us`

### 获取其他语言的拼写文件

实现比较简单，暂时用不到，需要时再了解.


## 技巧120 将单词添加到拼写文件中

* vim的品系字典并非完美，但我们可以通过把单词添加到拼写文件的方式进一步完善它。

  1. 如果某个单词并没有出现在拼写字典中，vim会误把它标记为错误，这个时候我们可以用 `zg`命令将这个单词添加到拼写文件中
  2. 相反的，我们可以用`zw`把某个单词标记为错误，将该词从拼写文件中删除
  3. `zug` 撤销对光标下单词所执行的`zg`,`zw`操作

### 为专业术语创建拼写文件

具体用到时，具体了解


## 技巧121 在插入模式下更正拼写错误

### 通常做法: 切换到普通模式

* `[s`, `]s`进行跳转, `z=`为单词更正提出建议

### 快捷方式:利用拼写自动补全功能

* 在插入模式下,光标位于除了单词首字母外的位置末尾,`<C-x>s`为单词提供建议


## 附录A

### A.1 动态改变Vim的设置项

* 用`:set`设置选项,下面以`ignorecase`为例，这个选型是布尔型
    + `:set ignorecase` 打开这个设置
    + `:set noignorecase` 关闭这个设置
    + `:set ignorecase` 反转这个设置
    + `:set ignorecase?` 获取这个设置的当前状态
    + `:set ignorecase&` 用&号将任意选项重置为默认值
    + `:set ts=2 sts=2 sw=2 et` 设置`tabstop`(制表符)，`softtabstop`，`shifwidth`，`exandtab`所占的列数.
    + `ignorecase`可被简写为`ic` 即为 `:set ic`,`:set noic`
    
    
### A.2 将配置信息存至vimrc文件

* 我们可以将定制好的选项写入文件，然后通过`:source {file}`将指定的`{file}`中的设置项应用于当前的缓冲区.

* Vim启动时，会默认载入vimrc文件中的配置
   + `:edit $MYVIMRC`可以直接打开vimrc文件进行改动，`$MYVIMRC`是vimrc的路径。
   + `:source $MYVIMRC` 改动完成之后，加载新的配置。
   + 如果当前文件恰好是vimrc,则`:source $MYVIMRC`可简化为`:so %`
   
### A.3 为特定类型的文件应用个性化设置

* 在vimrc中，用`autocmd` 使Vim监听某一类事件，一旦该事件发生，则执行指定的命令
* 如果针对特定类型文件需要配置多个选项，则需要使用文件类型插件(ftplugin)，为不同文件类型进行定制.

```
1. 例如: 排版时，ruby需要2个空格缩进，javascript文件需要4列宽度, 用nodelint检查javascript类型文件

在vimrc中设置
if has("autocmd")
   filetype on
   autocmd FileType ruby setlocal ts=2 sts=2 sw=2 et
   autocmd FileType javascript setlocal ts=4 sts=4 sw=4 noet
   autocmd FileType javascript compiler nodelint
endif

当是对应类型时，自动触发对应的命令去执行.

2. 如果配置的选项过多时，用文件类型(ftplugin)插件

例如， 对javascript而言

* 可以将其配置移动到~/.vim/after/ftplugin/javascript.vim中去。
* 为了使ftplugin机制能够使用,vimrc中需要包含 filetype plugin on 



```
