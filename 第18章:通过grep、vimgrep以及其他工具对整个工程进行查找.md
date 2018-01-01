## 技巧108 不必离开Vim也能调用grep

### 在系统命令行中执行grep

1. `$grep -n Waldo *`
    
    ```
    department-store.txt:1:Waldo is beside the boot
    counter. 
    goldrush.txt:6:Waldo is studying his clipboard.
    goldrush.txt:9:The penny farthing is 10 paces ahead of Waldo.
    
    ```
     * 缺省情况下，grep会为每个匹配项打印一行，显示匹配行的内容以及所在的文件名
     * -n参数用来显示行号信息

2. `$vim goldrush.txt +9` 打开文件并跳转到指定的行

### 在vim内部调用grep

* `:grep -i Waldo *`
   + Vim在后台在shell中执行`$grep -n Waldo *`
   + Vim会解析grep的输出，并创建一个quickfix列表
   + Vim默认使用参数 `-n`
   + `-i` 控制grep不区分大小写
  

## 技巧109 定制grep程序

??? 具体用到时，再了解



## 技巧110 使用Vim内部的Grep

:vimgrep命令允许我们使用Vim自带的正则表达式引擎，实现跨文件的查找功能.

`:vim[grep][!] /{pattern}/[g][j] {file} ...` 

??? 具体用时，再了解.









