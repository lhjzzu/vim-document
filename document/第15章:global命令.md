## 技巧97 结识global命令

 :global命令允许我们再某个指定模式的所有匹配行上运行Ex命令。
 
* 语法`:[range] global[!] /{pattern}/ [cmd]`
    1. 缺省情况下，:global的作用范围是整个文件(%)
    2. `{pattern}`域与查找历史相互关联。这意味着如果该域留空，则会自动使用当前的查找模式。
    3. `[cmd]`可以是除了:global命令之外的任何Ex命令。缺省时使用`:print`。
    4. `:global!`或者`:vglobal`反转:global的行为。即为在没有匹配到模式的行执行[cmd]命令
    5. global命令执行时，第一轮 `[range]`对范围内的行进行标记，第二轮对每个标记的行执行`[cmd]`

    
    
## 技巧98 删除所有包含模式的文本行

`:g/{pattern}/d` 在所有匹配`{pattern}`的行执行`d`命令
`:v/{pattern}/d` 在所有不匹配`{pattern}`的行执行`d`命令

## 技巧99 将TODO项收集至寄存器

1. `qaq` 清空寄存器a
2. `:g/{pattern}/yank A` 把所有匹配`{pattern}`的行，存储到寄存器a中. (A表示追加到寄存器a的末尾)


## 技巧100 将CSS文件中所有规则的属性按照字母排序


### 对单条规则的属性进行排序


```

按键操作                        缓冲区内容
{start}                       html { 
                                margin: 0;                                padding: 0;                                border: 0;                                font-size: 100%;                                font: inherit;                                vertical-align: baseline;
                               }
                               
                               
                               
vi{                           html { 
                                margin: 0;                                padding: 0;                                border: 0;                                font-size: 100%;                                font: inherit;                                vertical-align: baseline;
                               }
                               
                               
                               
                               
:'<,'>sort                    html { 
                                border: 0;
                                font-size: 100%;
                                font: inherit;
                                margin: 0;                                padding: 0;                                vertical-align: baseline;
                               }


```

1. `vi{` 在可视模式下，高亮选中{内的所有内容
2. `:'<,'>sort` 对高亮选区进行排序

### 对所有规则的属性进行排序

```
按键操作                        缓冲区内容
{start}                       html { 
                                margin: 0;                                padding: 0;                                border: 0;                                font-size: 100%;                                font: inherit;                                vertical-align: baseline;
                               }
                               
                               body {
                                  color: black; 
                                  background: white;                                 
                                  line-height: 1.5;
                                  }
                               
                           
                               
                               
                               
                               
:g/{/ .+1,/}/-1 sort         html { 
                                border: 0;
                                font-size: 100%;
                                font: inherit;
                                margin: 0;                                padding: 0;                                vertical-align: baseline;
                               }
                               
                               body {
                                  background: white;
                                  color: black; 
                                  line-height: 1.5;
                                  }


```

1. `g/{pattern}/[range][cmd]` 指定cmd执行的范围
2. `.+1,/}/-1` 从{所在当前行往下偏移一行，直到}所在行往上偏移一行。 `.`指代当前行, `+1` 向下偏移一行，`-1`向上偏移一行。