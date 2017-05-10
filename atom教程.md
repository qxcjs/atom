[TOC]
## atom教程

### atom插件
atom在安装时会将插件管理器一同安装，在命令行可以直接使用`apm`命令安装

```
apm install markdown-toc

apm install markdown-scroll-sync

apm install vim-mode

apm install vim-mode-plus

apm install markdown-img-paste # 安装后使用快捷键ctrl+shift+v就可以将复制到系统剪切板的图片粘贴到 markdown

apm install markdown-preview-plus

apm install markdown-preview-enhanced

apm install sync-settings
```

### 常用快捷键
```
command + \ 隐藏/显示文件目录
command + o 打开文件/文件夹
command +w 关闭文件
command + L 选择一整行
command + enter 光标任意位置也可以直接创建下一行
command + shift + [ 向左切换文件
command + shift + ] 向右切换文件
command + 'number' 比如command + 1切换到对应文件
command + shift +p 弹出包搜索框
command + k + ↑ 向上分屏
command + k + ↓ 向下分屏
command + k + ← 向左分屏
command + k + → 向右分屏
command + b 在打开的文件中查找文件
command + p 在项目中查找文件
command + f 在当前文件中查找
command + g 查找下一个
command + shift + g 查找上一个
command + shift + f 在整个项目中查找
esc 退出查找界面
command + d 选择词
command + ↑ 跳到文件顶部
command + ↓ 跳到文件底部
command + ← 跳到行首
command + → 跳到行尾
command + shift + ↑ 向上选择所有
shift + ↑ 向上选择一行
option/alt + → 光标按词向后跳 加上shift就是选择
```
> 这里要注意一下，先按下command + k 之后，再按箭头才会分屏成功。

## 参考文章
1. [Atom同步配置](http://www.cnblogs.com/hooray/p/5885211.html)
