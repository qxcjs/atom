## git比较远程分支和本地分支
假定远端库名为 origin, 你要比较的本地分支为 test, 远端分支为 xxx
```
# 获取远端库最新信息
$ git fetch origin

# 做diff
$ git diff test origin/xxx
```
