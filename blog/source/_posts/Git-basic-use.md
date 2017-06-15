---
title: Git basic use
subtitle: VIM's basic usage
date: 2017-06-15 16:46:19
author: Ricann
head-img:
categories:  tools
tags:
 - Git
 - tools
keywords:  Git, tools
comments: true
---

## clone
指定用户名和密码：
 + git clone http://name:pwd@xxx.git

## diff
获取两次提交之间的文件变化列表：
 + git diff SHA1_1 SHA1_2 --stat
 + git diff SHA1_1 SHA1_2 --numstat
 + git diff SHA1_1 SHA1_2 --shortstat

<!--more-->

## branch
### 删除远程分支
git branch -r -d origin/branch-name

git push origin :branch-name

## patch
 + 生成某次提交以后的所有patch：
  - git format-patch -s sha1
 + 检查patch文件：
  - git apply --stat newpatch.patch
 + 检查能否应用成功：
  - git apply --check newpatch.patch
 + 打补丁：
  - git am --signoff < newpatch.patch

## submodule
 + 添加：
  - git submodule add repo path
 + 使用：
  - git submodule update --init --recursive
 + 删除：
  - 先在.gitmodules中删除相关配置，再执行*git rm –cached*
