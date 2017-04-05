---
title: cheat
date: 2016-09-25 18:48:36
tags:
---

### 更好用的shell查询命令工具,cheat

安装cheat(需要python环境)

```
pip install cheat
```

使用方式：
```
cheat ls
```

其他帮助命令：
```
man ls
```

### 为什么要使用cheat?
相信每个人都用过 man 来查看某个命令的使用方法,
这个确实很全面,但是说明文字很长,而且看着很累,解释不言简意赅,
对于英语能力一般的人确实是个体力活
cheat使用更简短的语言描述,和实例来展示该命令的使用方式
比如:
```
cheat ls
```

下面是ls命令的常用方式说明
```
# Displays everything in the target directory
ls path/to/the/target/directory

# Displays everything including hidden files
ls -a

# Displays all files, along with the size (with unit suffixes) and timestamp
ls -lh

# Display files, sorted by size
ls -S

# Display directories only
ls -d */
```