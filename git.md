[TOC]
## git比较远程分支和本地分支
假定远端库名为 origin, 你要比较的本地分支为 test, 远端分支为 xxx
```
# 获取远端库最新信息
$ git fetch origin

# 做diff
$ git diff test origin/xxx
```
## git撤销后再次撤销
第一次撤销后使用`git log`会无法看到最后一次提交的log，这是需要使用`git reflog show`或git log -g命令来查看所有的提交日志。再使用`git reset --hard id`
