[TOC]
### 编辑类
```
i: insert，进入插入模式，新字符插入在光标前
a: append，进入编辑模式，新字符插入在光标后
c: change，修改
d: delete，删除
p: put，放置，可以将d删除的内容，放置在光标后面
y: yank，拷贝
r: replace，替换，和c不同在于，不必进入编辑模式即可替换
s: substitute，替代，和c不同在于，可以只修改一个字符而非整个字
x: x，和d不同在于，可以只删除一个字符而非整个字
~: change case，替换大小写
.: repeat，重复上一条命令
u: undo，撤销上一条命令
J: join，将两行合并为一行
```


### 移动光标类
```
h: left，向左移动光标
j: down，向下移动光标
k: up，向下移动光标
l: right，向由移动光标
0: digit zero, move to beginning of line，移动到行首
$: move to end of line，移动到行尾
w: move by word，按字向后移动光标(包括标点)
W: move by large word，按字向后移动光标(忽略标点)
b: move backward by word，按字向前移动光标(包括标点)
B: move backward by large word，按字向前移动光标(忽略标点)
e: move to end of word，移动到字尾(包括标点)
E: move to large end of word，移动到字尾(忽略标点)
G: go to end of the file，移动到文件末尾最后一行
```


#### c命令详解
c 命令所删除的数据都存在缓冲区, 可以结合p/P命令构成剪切粘贴操作, 方法是:先进行 c 命令, 再按 Esc 键返回命令模式, 最后才进行 p/P 命令.
删除--->剪切---->进入插入模式
```
C or c$
表示修改当前行上光标后面的部分. 进入编辑状态.

c0 or c^
表示从光标处到当前行行首的部分进行修改，^代表首个非空格处。

cc OR S
修改当前行. 进入编辑状态.

cw
从光标所在的位置开始到该单词结束进行修改. 进入编辑状态

cfx AND cFx
这里的 x 为一任意字符, cfx 表示修改从光标到下一个字符 x 之间的文本;
cFx 表示修改从光标到上一个字符 x 之间的文本.

cn|
修改从光标到当前行的第 n 个字符间的所有字符, n 正整数.

cnG and cG
这里的 n 为一任意自然数, cnG 表示修改当前行到第 n 行之间的所有行;
cG 表示修改当前行直至末行.
```

## 使用shell命令
1. `:!command`
不退出vim，并执行shell命令command，将命令输出显示在vim的命令区域，不会改变当前编辑的文件的内容

## 参考文章
1. [提升效率的若干vim技巧](http://www.dutor.net/index.php/2011/09/efficient-vim-tips/)
