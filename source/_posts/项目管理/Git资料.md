title: Git资料
date: 2014-12-03 14:22:52
tags:
    - Git
---

##一、推荐学习网站

[廖雪峰 Git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
[Pro Git（中文版）](http://git.oschina.net/progit/)

##二、日常git命令总结

**回退版本**

    git log --pretty=oneline
    git reset --hard 3628164

**撤销修改**

    git checkout -- readme.txt

**添加远程仓库**

    git remote add origin git_url
    git push -u origin master

**分支管理**

    git checkout -b dev
    git merge dev
    git branch -d dev

**临时Bug处理**

    git stash
    git checkout master
    git checkout -b issue-101
    ....处理....
    git checkout master
    git merge --no-ff -m "merged bug fix 101" issue-101
    git branch -d issue-101
    git checkout dev
    git stash list
    git stash pop  //git stash apply stash@{0}

**标签处理**

    git tag v1.0
    git tag -d v0.1
    git push origin v1.0
    删除远程标签
    先删本地
    git tag -d v1.0
    git push origin :refs/tags/v1.0

