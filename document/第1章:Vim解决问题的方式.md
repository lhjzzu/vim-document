显示特殊按键

```
标记          含义
<Esc>        按退出键
<CR>         按回车键,也写作<Enter>
<Ctrl>       按控制键
<Tab>        按制表键
<S-Tab>      同时按<Shift>和<Tab>
<Up>         按上光标键
<Down>       按下光标键
```

## 技巧 1 结识`.`命令

### 重复删除
```
按键操作  缓冲区内容
{start}  Line one  
         Line two
         Line three
         Line four
         
  x      ine one  
         Line two
         Line three
         Line four
         
  .      ne one  
         Line two
         Line three
         Line four
       
  ..      one  
         Line two
         Line three
         Line four
```

1. 光标最初位于第一行L处
2. 	`x`命令会删除光标下的字符(依然处于普通模式)
3. `.` 重复 `x`命令
4. `..` 重复2次 `x`命令

```
按键操作  缓冲区内容
{start}  Line one  
         Line two
         Line three
         Line four
         
  dd     Line two
         Line three
         Line four
         
  .      Line three
         Line four
```

1. 光标最初位于第一行L处
2. 	`dd`命令会删除当前行(依然处于普通模式)
3. `.` 重复 `dd`命令



```
按键操作  缓冲区内容
{start}  Line one  
         Line two
         Line three
         Line four
         
  >G     Line one
           Line two
           Line three
           Line four
         
  j      Line one
           Line two
           Line three
           Line four
  .      Line one
           Line two
             Line three
             Line four
  j.     Line one
           Line two
             Line three
              Line four
```

1. 光标从第二行L处开始
2. `>G` 增加从当前行到文档末尾的所有行的缩进层级
3. `j`移动到下一行 (`h`=> 左, `j` => 下, `k`=> 上, `l`=> 右 )
4. `.` 重复`>G`操作
5. `j.` 下移一行并重复`>G`操作

其他命令:

1. `u`撤销操作

## 技巧 2 不要自我重复

在所有行后添加分号

```
按键操作     缓冲区内容
{start}     var foo = 1
            var bar = 'a'
            var foobar = foo + bar

A;<Esc>     var foo = 1;
            var bar = 'a'
            var foobar = foo + bar
            
j           var foo = 1;
            var bar = 'a'
            var foobar = foo + bar

.           var foo = 1;
            var bar = 'a';
            var foobar = foo + bar

j.          var foo = 1;
            var bar = 'a';
            var foobar = foo + bar;         
            
```

1. 光标最初位于第一行v处
2. `A;<Esc>`: `A`移动到行末尾并进入插入模式,然后输入`; `,再按`<Esc>`回到普通模式
3. `j`移动到下一行
4. `.` 重复 `A;<Esc>`操作
5. `j.` 移动到下一行并重复 `A;<Esc>`操作


其他命令

1. `$`: 移动光标到行尾
2. `a`: 在光标所在位置进入插入模式



## 技巧 3 以退为进

```
按键操作     缓冲区内容

{start}     var foo = "method("+argument1+","+argument2+")";
f+          var foo = "method("+argument1+","+argument2+")";
s + <Esc>   var foo = "method(" + argument1+","+argument2+")";
;           var foo = "method(" + argument1+","+argument2+")";
.           var foo = "method(" + argument1 + ","+argument2+")";
;.          var foo = "method(" + argument1 + "," + argument2+")";
;.          var foo = "method(" + argument1 + "," + argument2 + ")";

```

1. 光标位于v处
2. `f+` 在行内查找下一处+的位置
3. `s + <Esc>`：`s`删除光标处的字符，并进入插入模式,并输入`空格+空格`,再按`<Esc>`回到普通模式
4. `;` 重复查找上次 `f{char}` 命令所查找的字符
5. `.` 重复`s + <Esc>`
6. `;.` 查找下个加号，然后重复`s + <Esc>`
7. `;.` 查找下个加号，然后重复`s + <Esc>`


<!--1. `f{char}`在行内查找下一处该字符的位置
2. `;` 重复查找上次 f 命令所查找的字符
3. `s` 先删除光标下的字符，然后进入插入模式-->

## 技巧 4 执行、重复、回退

```
目的                    操作                  重复     回退
做出一个修改             {edit}                 .      u
在行内查找下一指定字符     f{char}                ;      ,
在行内查找上一指定字符     F{char}                ;      ,
在文档中查找下一处匹配项   /pattern<CR>           n      N在文档中查找上一处匹配项   ?pattern<CR>           n      N
执行替换                :s/target/replacement  &      u
执行一系列修改           qx{changes}q           @x     u  (暂时不理解这个命令)
```

## 技巧 5 查找并手动替换
```
将下列文档中所有的content替换成copy
...We're waiting for content before the site can go live... 
...If you are content with this, let's go ahead with it... 
...We'll launch as soon as we have the content...
```

1. 此时光标位于content第一个字母
2. `*` :会将当前光标所在位置的单词content，在整个文档中高亮。
3. `cwcopy<Esc>`: `cw`会删除从光标位置到单词结尾间的字符，并进入插入模式
4. `n` 移动到下个高亮的content
5. `.` 重复`cwcopy<Esc>`的操作

其他命令

1. :%s/content/copy/g 直接把所有的content的替换成copy

## 技巧6 结识 `.`范式

理想模式:用一键移动，一键执行。

从上面的例子我们可以看出,用相应的键进行移动，并且我们把执行做成`.`范式。



