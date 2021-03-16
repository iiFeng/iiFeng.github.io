# Git 是什么 <!-- {docsify-ignore} -->
代码版本管理工具

## Git 原理
文件通过 `git add .` 提交后，生成 hash 值，文件被保存 Git 二进制文件对象

## Git 步骤
暂存区:所有变动的文件，Git 都记录在一个区域。对变动文件保存对象和更新暂存区使用命令：`git add --all`    

工作区生成快照:将暂存区的文件提交目录结果和说明。当前分支指针移向新创建的快照：`git commit -m`
- 记录快照：指向某个快照的指针
- 指针 HEAD， 总是指向当前分支的最近一次快照。HEAD^ 指向 HEAD 的前一个快照（父节点），HEAD~6 则是 HEAD 之前的第 6 个快照。      

推送当前分支到远程仓库： `git push [remote]`

## Git 撤销
暂存区撤销文件：`git rm --cached [filename]`        
工作区撤销文件：`git checkout  -- [filename]` //找回本次修改之前的文件      
修改工作区上一次的提交信息：`git commit --amend -m`       
丢弃提交，当前分支指针重置为指定快照： `git reset [commit_sha]`
- --soft 参数可以撤回 commit 操作，文件仍然保留：`git reset --soft HEAD^`
- --hard 参数可以让工作区里面的文件也回到以前的状态：`git reset -hard [commit_sha]`       
撤销之前的提交：`git revert`

## Git 查看信息
显示有变更的文件：`git status`        
显示暂存区和工作区的差异：`git diff`      
