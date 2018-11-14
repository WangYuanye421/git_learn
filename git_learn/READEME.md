[TOC]
# 1. 概述
> **基本的 Git 工作流程如下：**
> - 在工作目录中修改文件。
> - 暂存文件，将文件的快照放入暂存区域。
> - 提交更新，找到暂存区域的文件，将快照永久性存储到 Git 仓库目录

> **SVN 与Git比较(面试常问)**

<html>
<table>
    <tr>
        <th></th>
        <th>SVN</th>
        <th>Git</th>
    </tr>
    <tr>
        <td>优点</td>
        <td>
            1.管理方便，逻辑明确，符合一般人思维习惯;</br>
            2.易于管理，集中式服务器更能保证安全性;</br>
            3.代码一致性非常高;</br>
            4.适合开发人数不多的项目开发
        </td>
        <td>
            1.适合分布式开发,强调个体;</br> 
            2.公共服务器压力和数据量都不会太大;</br> 
            3.速度快、灵活; </br>
            4.任意两个开发者之间可以很容易的解决冲突;</br>
            5.离线工作,每个人都是独立完整版本
        </td>
    </tr>
    <tr>
        <td>缺点</td>
        <td>
            1.集中式管理,联网才能更新、提交,中央处理器影响所有人;</br>
            2.服务器压力太大,数据库容量暴增;</br>
            3.不适合开源开发（开发人数非常非常多,但是Google app engine就是用SVN的.</br>
            但是一般集中式管理的有非常明确的权限管理机制（例如分支访问限制,可以实现分层管理,从而很好的解决开发人数众多的问题
        </td>
        <td>
            1.学习周期相对而言比较长;</br>
            2.不符合常规思维;</br>
            3.代码保密性差,一旦开发者把整个库克隆下来就可以完全公开所有代码和版本信息
        </td>
    </tr>
</table>
</html>

---
# 2. 核心概念
## 2.1 文件的生命周期
![git管理文件时文件的生命周期](C:/Users/wyy/Desktop/life_cycle.png)  

## 2.2 分支仓库关系

## 2.3 分支模型
仓库
- 远程
- 本地
分支
- 工作区
- 暂存区
- 本地分支

---
# 3. 常用命令
## 3.1 Git 配置
### 3.1.1 三个作用域
> Git 内置config工具，可控制Git外观及行为。git config --[scope] 需指定作用域参数，默认为当前仓库。配置前，应该了解参数涉及的三个作用层面：
```txt
1. system   配置将作用于整台计算机      /etc/gitconfig
2. global   作用于当前用户              ~/.gitconfig 或 ~/.config/git/config
3. local    作用于当前仓库              当前仓库 .git/config目录
```
**作用域越小，优先级越高;同时配置时，覆盖上层，即：local 覆盖其他**

### 3.1.2 配置查询与参考
> `git config --list` 查看Git所有配置项  
> list 以键值对形式罗列出Git当前的配置项，可通过指定key来**查看**或**修改**相应value：如  
```
git config user.name                查看用户名称信息
git config gui.encoding             查看用户界面编码
git config user.email [new_email]   更新用户邮件信息
```
> Git基本配置(以当前用户为例)

```
git config --global user.name  your_name
git config --global user.email your_email

git config --global i18n.logoutputencoding=utf-8        #日志输入编码
git config --global i18n.commitencoding=utf-8           #设置commit编码（输入中文不乱码）
git config --global gui.encoding=utf-8                  #用户图形界面编码

git config --global alias.[alias] [order/'String']      #自定义指令简称
    eg: git config --global alias.cm commit
        git config --global alias.refd 'branch --set-upstream-to=origin/develop'
        git refd develop   等价于   git branch --set-upstream-to=origin/develop develop
        

```
**删除别名的命令目前不清楚，但根据最初设置别名的作用域级别，到相应的config文件中删除即可**
### 3.1.3 设置忽略文件

## 3.2 初始化
> 根据工作中遇到的情况，我将Git的初始化分为两种，一种是从已知远程库中初始化本地代码；另一种则是先初始化本地仓库，然后关联空的远程仓库

### 3.2.1 克隆远程仓库(多数情况)
```
$ git clone [URL] [repo_name]          克隆远程仓库，项目目录命名为repo_name
```

### 3.2.2 初始化本地仓库
> 用Git对已有的项目进行管理，并推至远程仓库
```
    $ git init                      //对项目目录进行初始化，将生成 .git 文件夹
    $ git remote add [URL]          //为当前项目指定远程仓库（该仓库应该是一个空仓库）
        ......                      //将所有文件添加至暂存区并提交
```

## 3.3 管理远程仓库

> `git remote add [URL] {repo_name}`    添加远程仓库  
> `git remote rm [repo_name]`           删除远程仓库  
> `git remote -v`                       查看当前仓库关联的远程仓库（可以关联多个远程仓库,场景是代码托管在多个远程仓库中）  
> `git remote show [repo_name]`         显示远程仓库详情  
> `git remote rename A B`               重命名远程仓库

> ` git fetch `                                         获取所有远程仓库中所有分支中提交  
> `git fetch remote_repo`                               获取指定远程仓库的所有提交  
> `git fetch remote_repo branch_name`                   获取指定远程仓库中指定分支的所有更新  
> `git fetch remote_repo branch_name:new_local_branch`  获取指定远程仓库中指定分支的所有更新，并创建本地分支保存远端

## 3.4 分支模型

### 3.4.1 查看
> `git branch`          查看本地分支  
> `git branch -r`       查看远程分支  
> `git branch -a`       查看所有分支（本地+远程）

### 3.4.2 新建
> `git branch [local_branch]`             新建本地分支：local_branch             
> `git push origin [remote_branch]`       新建远程分支：remote_branch  
> `git branch -m old_name new_name`       分支改名

### 3.4.3 删除
> `git branch -d local_branch`              删除本地分支  
> `git push origin --delete remote_branch`  删除远程分支

### 3.4.4 切换
> `git checkout branch_name`                由当前分支切换至branch_name分支  
> `git checkout -b branch_name`             检出branch_name并切换至该分支，同样适用于远程分支检出  
**等价于 `git branch branch_name` + `git checkout branch_name`**  
> `git checkout -b branch_name tag_name`    也可以从指定标签中检出分支

### <a name="guanlian">3.4.1 关联</a>
> `git branch --set-upstream-to=origin/<remote_branch> local_branch`    本地 local_branch 分支关联远程 remote_branch 分支  
> `git branch -u origin/<remote_branch>  local_branch`                  同上，等价  
> `git branch --unset-upstream local_branch`                            取消local_branch关联的远程分支

### 3.4.5 添加/删除文件
> `git add file_name/dir`       添加文件至暂存区  
> `git mv A B`                  修改文件名  
> `git rm file_name/dir`        从暂存区删除  
> `git rm -f file`              若文件已加入暂存，需强制删除（force）  
> `git rm -cached file`         只删除暂存区的文件，本地磁盘保留  
> 注：添加至暂存区意味着文件将被Git追踪记录(track),所以`git add file_name` 的作用可理解为:  
```
 1. 开始追踪新的文件  
 2. 将文件放入暂存区，即下次要提交的版本。(若暂存的文件发生变动，需重新add)  
 3. 发生冲突的文件不会放入暂存区，解决冲突后，使用`git add`可标记冲突已解决
```
### 3.4.6 拉/推代码
> 当本地分支未与远程分支关联，**每次**pull/push时需要显示声明远程分支，即：
```
$ git pull origin develop
$ git push origin develop
```
> 若 <a href="#guanlian">3.4.1 关联</a>了远程分支，可直接`git pull/push`

### 3.4.7 查看/对比/合并
> `git status -s`           查看，  -s 简短输出(显示文件新增或修改，是否已被暂存)  
![简短输出样式图](C:/Users/wyy/Desktop/status-s.png)  
> `git diff`                        对比工作区和暂存区差异，即修改之后未暂存的内容  
> `git diff --cached`               查看暂存区文件较上次提交的差异  
> `git merge branch_name`           合并branch_name到当前所在分支  
> `git merge branch_name --no-ff`   合并时强制取消fast-forward合并模式，使用递归策略保留分支历史(见下图对比)  
> `git merge --abort`             发生冲突时，终止合并，试图重建当前分支合并前的状态，冲突文件需重新add到暂存区，
![--no-ff区别](./pic/diff_no_ff.png)

### 3.4.8 提交/回退
> `git commit -m 'msg about this commit' `      提交  
> `git commit -a -m 'msg' `                     自动add到暂存区并提交(前提：文件已被Git追踪管理，新增文件不适用)  
> `git checkout file_name`                      回退文件版本至本地仓库  
> `git reset --hard [commit_id]`                回退到指定提交版本

### 3.4.9 其他
> `git log -p`              查看日志  

> `git commit --amend`      修订提交，可以多次提交，只会有一次提交记录  
> **使用场景**：某次提交遗忘了几个文件或提交信息写错

## 3.5 标签
> 标签分为`轻量标签`和`附注标签 -a`，`附注标签`因其携带信息丰富被广泛使用  
> `git tag -l`                          显示标签  
> `git tag -l 'filter' `                过滤显示标签  
> `git show tag_name`                   通过标签显示详情  
> `git tag -a tag_name -m 'msg' `       新建附注标签  
> `git push origin tag_name/--tags`     推送指定标签(所有标签)到远程  
> `git checkout -b branch_name tag_name`从指定标签上检出分支

---
# 4. Git使用总结
> 1. 使用 `git help [command]`查找相关命令  
> 2. Git 鼓励频繁的使用分支与合并  
> 3. 项目分支命名规范及作用

<html>
    <table>
        <tr>
            <th>分支命名</th>
            <th>存活周期</th>
            <th>存在意义</th>
        </tr>
        <tr>
            <td>master</td>
            <td>长期分支,固定分支</td>
            <td>主分支，可用的稳定版本</td>
        </tr>
        <tr>
            <td>release_1.0</td>
            <td>临时分支，版本记录</td>
            <td>
                一个项目会分多个模块或阶段上线,<br>
                当开发进度达到某一上线要求时(即需要发布一版上线),<br>
                此时在master或develop分支上派生release分支,<br>
                该分支允许做小的修正及发布所需的各项说明.<br>
                使得develop分支空闲下来以进行下一版本的开发.<br>
                注:该分支可以删除,打上tag,合并到master后就可以删除.<br>
                下次发布时,重新派生release分支生成新版本.<br>
                删除后,版本的迭代记录可以在git中使用tag查找.
            </td>
        </tr>
        <tr>
            <td>develop</td>
            <td>长期分支</td>
            <td>未上线功能的测试</td>
        </tr>
        <tr>
            <td>topic/issue/feature</td>
            <td>临时分支，上线后删除</td>
            <td>新功能特性的开发</td>
        </tr>
        <tr>
            <td>hotfix</td>
            <td>临时分支，修复后删除</td>
            <td>线上紧急bug修复</td>
        </tr>
        <tr>
            <td colspan="3">
                <p>使用说明:</p>
                <ol>
                    <li>feature分支向develop分支进行合并，进行测试</li>
                    <li>develop分支上测试通过的功能，可以通过feature分支向master合并，删除对应feature分支（部分功能上线</li>
                    <li>线上紧急bug，在master分支上新开hotfix分支，修复完成，打tag后合并到master分支,删除hotfix分支</li>
                    <li>feature分支可根据功能或模块进行命名，如feature_customer</li>
                    <li>所有的新需求和bug修复，都不应该在develop或master分支上修改，应该根据需求派生新的本地分支来开发</li>
                    <li>对于没有权限将新代码合并至master分支的情况，可以将本地分支推到远程，由相关有权限的人员进行合并</li>
                </ol>
            </td>
        </tr>
    </table>
</html>

---
# 5. 参考与引用
- [廖雪峰-Git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)  
- [pro git](https://progit.bootcss.com/#_pro_git)  
- [高质量GIT中文教程](https://github.com/geeeeeeeeek/git-recipes)  
- [git flow流程介绍](https://danielkummer.github.io/git-flow-cheatsheet/index.zh_CN.html)  
- [git-简易指南](http://www.bootcss.com/p/git-guide/)

