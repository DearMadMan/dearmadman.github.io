title: git 请求合并 冲突解决方案
date: 2015-09-01 15:31:20
categories: other
tags: git
---
![](/images/s08.jpg)
# git 请求合并 冲突解决方案

## 场景
发布在gitcafe的项目被人fork，并且贡献了代码，进行请求合并

## 合并
这种合并一般是以打补丁的方式进行请求合并，操作方式：
```
git checkout -b patch master

git am -3 -k some.patch

git status  # 这里会列举冲突的文件 

# 进行冲突处理 处理完所有冲突后

git add .

git am --continue

git checkout master

git merge patch

git push 

```
