ctags是一个外部程序，它通过扫描代码库，生成关键字。

## 技巧101 结识ctags

为了能够使用vim的标签跳转功能，首先，必须安装ctags程序。

### 安装Ctags

1. $ brew install ctags
2. $ ctags --help
    - Exuberant Ctags 5.8, Copyright (C) 1996-2009 Darren Hiebert Compiled: Dec 18 2010, 22:44:26
 
3. 如果没出出现上述信息，需要对$PATH进行修改
   + vi ~/.bash_profile
   + export PATH="/usr/local/bin:/usr/local/sbin:$PATH"
   + source ~/.bash_profile

### 详解标签文件

```
!_TAG_FILE_FORMAT	2	/extended format; --format=1 will not append ;" to lines/
!_TAG_FILE_SORTED	1	/0=unsorted, 1=sorted, 2=foldcase/
!_TAG_PROGRAM_AUTHOR	Darren Hiebert	/dhiebert@users.sourceforge.net/
!_TAG_PROGRAM_NAME	Exuberant Ctags	//
!_TAG_PROGRAM_URL	http://ctags.sourceforge.net	/official site/
!_TAG_PROGRAM_VERSION	5.8	//

Anglophone anglophone.rb  /^class Anglophone < Speaker$/;" c 
Francophone francophone.rb  /^class Francophone < Speaker$/;" c 
Speaker speaker.rb  /^class Speaker$/;" c initialize speaker.rb   /^ def initialize(name)$/;" f 
speak anglophone.rb  /^ def speak$/;" f  class:Anglophone
speak francophone .rb  /^ def speak$/;" f  class:Francophone 
speak speaker.rb  /^ def speak$/;" f  class:Speaker 

```

* 标签文件的前几行由元数据组成
* 此后的每一行文本均由关键字，文件名，以及关键字在源代码的位置这3项内容构成.
* 关键字是按字母顺序排列，vim采用折半查找法快速定位某个关键字。
 
### 用ctags建立代码库索引

 `$ ctags *.rb` ,ctags创建了一个名为tags的纯文本文件，其内容是根据3个rb文件的分析而生成的关键字索引.
 
 
### 用模式定位关键字，而不是用行号
ctags不采用绝对行号，而是`用查找命令定位`每一处关键字

### 用元数据标记关键字

* 传统上标签文件由制表符分割的3组字段构成:`关键字`,`文件名`,以及`定位符`
* 目前使用扩展格式，允许在末尾添加额外字段，为关键字提供元数据
   + Anglophone 后面有`c`标签，用来表示`类`
   + speak 后面有`f`标签，用来表示`函数`

## 技巧102 配置Vim使用ctags

要使用基于ctag的跳转命令

1. 要确保标签文件上是最新的
2. 让vim知道到哪里去找标签文件

### 告诉Vim标签文件在哪里

* `tags`选项指定了vim应该到玛丽去找标签文件
????没有理解


### 手动执行ctags

1. `$ !ctags -R` 该命令将从vim当前的工作目录开始，遍历其所有的子目录，并未其中的文件建立索引。最终，再把这个标签问价保存到当前工作目录中。
2. 如果加入--exclude=.git 或 --languages=-sql等类似的参数，那么手动比较麻烦，我们可以用`:nnoremap <f5> :!ctags -R<CR>`映射为<F5>，这样就可以完成索引的更新工作了。

### 在每次保存问价时自动执行ctags

1. `:autocmd BufWritePost * call system("ctags -R")` 在每次保存文件时自动调用ctags
2. `自动命令功能`允许我们在某个事件发生时调用一个命令，这些事件包括`缓冲区`的`创建`，`打开`或者`保存`等。


### 通过版本控制工具的回调机制自动执行ctags

1. 大多数版本控制工具都支持回调机制，允许我们执行一个脚本来响应代码仓库事件
2. 因此，我们可以将源代码控制工具配置成每次提交代码改动时，自动为代码仓库更新索引。


### 结论

1. 第一种，手动更新索引的方式最简单，但是我们要定期更新索引，否则索引会过时。
2. 第二种，每当缓冲区被保存时，自动调用ctags进行更新。频繁更新，特别对于规模较大的工程，耗时较长，总的开销较大。
3. 第三种，每当提交代码改动时，自动更新代码库的索引。虽然索引文件不是最新的（不包含新开发的代码的索引）,但代价较小，也基本满足需求。

## 技巧103 使用Vim的标签跳转命令，浏览关键字的定义

vim与ctags的集成，使得代码中的关键字变成了某种形式的超链接，使得我们可以快速的跳转到关键字的定义处。

### 跳转到关键字的定义处

1. `<C-]>` 光标将会从当前所在的关键字跳转到它的定义处。
2. `<C-t>` 光标将会回到跳转前光标所在的位置。

### 当关键字存在多处匹配时，可指定跳转的位置

1. 如果有多处匹配，`g<C-]>`会把所有匹配项列举出来供我们选择
2. 如果有多处匹配,`<C-]>`所到之处并非我们想要的，则可以通过`:tselect`命令，调出匹配列表进行回溯.也可以用`:tprev`、`:tfirst`、`:tlast`。

###  使用Ex命令

 * `:tag {keyword}` 相当于 `<C-]>`.
 * `:tjump {keyword}`相当于 `g<C-]>`的功能

 也可以使用正则表达式
 
 * `:tag /{pattern}`
 * `: tjump /{pattern}`

 
在使用标签进行代码浏览时的可用命令

```
命令                             用途
<C-]>                           跳转匹配当前光标所在关键字的第一处标签
g<C-]>                          如果有多处标签可以匹配当前光标所在的关键字，
                                提示用户指定一处进行跳转，如果只有一处匹配
                                则不会提示，直接进行跳转
:tag {keyword}                  相当于<C-]>
:tjump {keyword}                相当于g<C-]>
:pop 或 <C-t>                   反向遍历标签历史
:tag                            正向遍历标签历史
:tnext                          跳转到下一处匹配的标签
:tprev                          跳转到上一处匹配的标签
:tfirst                         跳转到第一处匹配的标签
:tlast                          跳转到最后一处匹配的标签
:tselect                        提示用户从标签匹配列表中选一项进行跳转
```
