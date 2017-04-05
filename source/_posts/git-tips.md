---
title: git-tips
date: 2016-08-16 19:00:00
tags:
---
## git一些常用的操作记录

### 一、git checkout

#### 恢复某个已修改的文件（撤销未提交的修改）：
```
git checkout file-name
```
例如：git checkout src/abc.js

#### 比如修改的都是js文件，不必一个个撤销，可以使用
```
git checkout *.js
```
#### 撤销所有修改
```
git checkout .
```
###  二、删除 git add 文件
```
git add abc.js

git rm --cached abc.js

git add .

git rm --cached .
```
### 三、解决本地多个ssh-key问题

#### 为github配置新的key ，取名为github
```
~/.ssh$ ssh-keygen -t rsa -C "xxx@gmail.com" -f ~/.ssh/github

~/.ssh$ ls

github github.pub id_rsa id_rsa.pub known_hosts
```
#### 其中默认的是id_rsa

### 四、Existing folder or Git repository
```
cd existing_folder
git init
git remote add origin git@xxxxxx:xxx/xxxx.git
git add .
git commit
git push -u origin master
```


最后推荐大家使用一个更好的提交工具
[gitmoji](https://github.com/carloscuesta/gitmoji) 和
[git cz](https://github.com/commitizen/cz-cli)
安装方法参考这个地址
git cz 和 gitmoji规范了提交文件的流程,填写更友好的commit信息,方便做code review