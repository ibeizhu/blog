---
title: git-diff
date: 2017-05-09 15:35:02
tags: git diff
---

### git 有四种状态，三个区域

#### 状态：untracked,unmodified,modified,staged

#### 区域：工作区域，暂存区域，本地仓库


[edit files actions] // 工作区域文件修改了

git diff //是查看 工作区域 与 暂存区域 的文件差别

git add .   // 文件修改从 工作区域=>暂存区域

git diff --cached //是查看 暂存区域 与 本地仓库 的文件差别

git commit -m ''   // 文件修改从 索引区=>本地仓库

git diff HEAD  //是查看 本地仓库 与 远程仓库 的文件差别