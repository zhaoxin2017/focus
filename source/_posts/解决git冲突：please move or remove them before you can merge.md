---
title: 解决git冲突：please move or remove them before you can merge
---

**问题：** 使用 git，pull 代码时报错：please move or remove them before you can merge

**意思：**请在合并之前移动或删除它们

**造成的原因：**本地修改时与远端提交的代码冲突而又没有 merge 合并

**解决：**

```shell script
git clean -d -fx
```

**参数解释：**

```shell script
d ：删除未被添加到git的路径中的文件

f ：强制运行

x ：删除忽略文件已经对git来说不识别的文件
```

注意：但是这样是有风险的，会删除本地的修改，也就是选择与远端同步，就是你写的、修改的代码统统会被移除！好多人直接这么做，几天的代码就没了，所以执行之前把自己冲突的代码先备份一下，解决冲突后再还原，然后再继续 pull 代码，切记一定要注意。
————————————————
版权声明：本文为 CSDN 博主「harry5508」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/harry5508/article/details/87784171
