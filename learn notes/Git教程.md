# git 入门介绍

git以快照的方式对所以文件进行存储，如果当前文件版本与上一个版本没有改变则不存储。

git文件的三种状态：modified,staged, 和committed。

Modified状态意味你改变了文件但是没有提交到本地的数据库。

Staged状态意味你当前版本标记了修改文件可以进入后面的提交快照。

Committed状态意味数据已经安全存到本地数据库。

下面是三种状态转换的流程图：

# Git基础使用

初始化仓库

```
git init
```

指定文件去监听

```
git add *.c
```

克隆仓库

```
git clone https://github.com/libgit2/libgit2 newname
```

记录仓库的改变

每个文件有两种状态：tracked 和 untracked。

你可以暂存这些文件和提交所以暂存的更改，循环往复：

查看文件状态：

```
git status
```

查看短文件状态：

```
git status -s
```

忽略文件

```
$ cat .gitignore
*.[oa]
*~
```

- \# 行注释
- /  开头避免递归
- /  结尾指定路径
- ！ 否定模式

```
# 忽略所以.a的文件
# ignore all .a files
*.a

# 但是仅监听lib.a文件，尽管你在上面忽略了.a文件
# but do track lib.a, even though you're ignoring .a files above
!lib.a

# 仅忽略当前的文件夹的TODO文件，不忽略子文件夹的TODO文件
# only ignore the TODO file in the current directory, not subdir/TODO
/TODO

# 忽略在任意文件夹的所以文件名build
# ignore all files in any directory named build
build/

# 忽略doc/notes.txt，但是不忽略doc/server/arch.txt
# ignore doc/notes.txt, but not doc/server/arch.txt
doc/*.txt

#忽略所有.pdf文件在doc/文件夹和它的子文件夹
# ignore all .pdf files in the doc/ directory and any of its subdirectories
doc/**/*.pdf
```

查看已更改但没有暂存的内容用git diff不带参数

```
git diff
```

查看当前目录与暂存区的不同

```
git diff --staged
```

查看已经暂存的内容（--staged和--cached是相似的）

```
git diff --cached
```

提交你的改变

```
git commit
```

另外，可以用-m提交改变附带信息

```
git commit -m "Story 182: fix benchmarks for speed"
```

跳过暂存区，提交改变

```
git commit -a -m 'Add new benchmarks'
```

移除文件从监听中

```
# 删除文件
rm PROJECTS.md

# 从git中删除
git rm PROJECTS.md
```

想移除文件从暂存区，但不从工作目录

```
git rm --cached README
```

移除log文件夹中所有.log的文件

```
git rm log/\*.log
```

移除所以文件名以~结尾的文件

```
git rm \*~
```

重命名文件

```
git mv file_from file_to
```

查看提交历史记录

```
git log
```

查看每次提交的不同，-p或--patch；也可以限制最近输出数量-2

```
git log -p -2
```

 查看提交的统计

```
git log --stat
```

改变log输出的格式，oneline打印每次提交在一行

```
git log --pretty=oneline
```

改变log输出的格式，format格式化打印

```
git log --pretty=format:"%h - %an, %ar : %s"
```

| Specifier | Description of Output                           |
| --------- | ----------------------------------------------- |
| %H        | Commit hash                                     |
| %h        | Abbreviated commit hash                         |
| %T        | Tree hash                                       |
| %t        | Abbreviated tree hash                           |
| %P        | Parent hashes                                   |
| %p        | Abbreviated parent hashes                       |
| %an       | Author name                                     |
| %ae       | Author email                                    |
| %ad       | Author date (format respects the --date=option) |
| %ar       | Author date, relative                           |
| %cn       | Committer name                                  |
| %ce       | Committer email                                 |
| %cd       | Committer date                                  |
| %cr       | Committer date, relative                        |
| %s        | Subject                                         |

改变log输出的格式，--graph可以打印分支合并的历史记录

```
git log --pretty=format:"%h %s" --graph
```

以下是git log常用的选项：

| Option          | Description                                                  |
| --------------- | ------------------------------------------------------------ |
| -p              | Show the patch introduced with each commit.                  |
| --stat          | Show statistics for files modified in each commit.           |
| --shortstat     | Display only the changed/insertions/deletions line from the --stat command. |
| --name-only     | Show the list of files modified after the commit information. |
| --name-status   | Show the list of files affected with added/modified/deleted information as well. |
| --abbrev-commit | Show only the first few characters of the SHA-1 checksum instead of all 40. |
| --relative-date | Display the date in a relative format (for example, “2 weeks ago”) instead of using the full date format. |
| --graph         | Display an ASCII graph of the branch and merge history beside the log output. |
| --pretty        | Show commits in an alternate format. Option values include oneline, short, full, fuller, and format (where you specify your own format). |
| --oneline       | Shorthand for --pretty=oneline --abbrev-commit used together. |

输出最近两周的log

```
git log --since=2.weeks
```

-S接受一个字符串仅显示更改了该字符串出现次数的提交

```
git log -S function_name
```

--接受一个路径，筛选出指定路径或文件名的提交更改

```
git log -- path/to/file
```

| Option            | Description                                                  |
| ----------------- | ------------------------------------------------------------ |
| -<n>              | Show only the last n commits                                 |
| --since, --after  | Limit the commits to those made after the specified date.    |
| --until, --before | Limit the commits to those made before the specified date.   |
| --author          | Only show commits in which the author entry matches the specified string. |
| --committer       | Only show commits in which the committer entry matches the specified string. |
| --grep            | Only show commits with a commit message containing the string |
| -S                | Only show commits adding or removing code matching the string |

撤销提交

```
git commit --amend
```

撤销暂存区文件

```
git reset HEAD CONTRIBUTING.md
```

撤销修改的文件

```
git checkout -- CONTRIBUTING.md
```

撤销暂存区文件--git restore

```
git restore --staged CONTRIBUTING.md
```

显示你的远程

```
$ git remote -v
origin https://github.com/schacon/ticgit (fetch)
origin https://github.com/schacon/ticgit (push)
```

添加远程仓库

```
# git remote add <shortname> <url>:
git remote add pb https://github.com/paulboone/ticgit
```

用pb代替整个URL，获取本地仓库还没有的信息

```
git fetch pb
```

拉取远程仓库

```
git fetch <remote>
```

推送仓库到远程

```
git push origin master
```

查看远程信息

```
# git remote show <remote>
git remote show origin
```

重命名和移除远程

```
# 重命名远程
git remote rename pb paul

# 移除远程, git remote 或 git remote rm
git remote remove paul
```

查看标记

```
git tag

# 搜索特定的标记（tag）
git tag -l "V1.8.5*"
```

创建注释标签

```
# 通过-a指定标记，-m附带注释
git tag -a v1.4 -m "my version 1.4"
```

创建轻量标签

轻量标签不支持-a, -s, 或-m, 仅支持标记名字

```
git tag v1.4-lw
```

分享标记（Tags）

仅在分支支持

```
# git push origin <tagname>
git push origin v1.5
```

推送所以标记（Tags）到远程服务器

```
git push origin --tags
```

删除标记

```
# git tag -d <tagname>
git tag -d v1.4-lw
```

# Git 分支

创建一个新的分支

```
git branch testing
```

切换分支

```
git checkout testing
```

创建一个分支并且切换到该分支

```
git checkout -b testing
```

## 基本的分支和合并

基本步骤：

1. 切换到你的生产分支
2. 创建一个分支添加hotfix
3. 在测试之后，合并hotfix分支，推送到生产环境
4. 切换回你的original用户和继续工作

切换到生产分支

```
git checkout -b iss53
```

```
$ vim index.html
$ git commit -a -m 'Create new footer [issue 53]'
```

切换到master分支

```
git checkout master
```

创建hotfix分支去工作

```
$ git checkout -b hotfix
Switched to a new branch 'hotfix'
$ vim index.html
$ git commit -a -m 'Fix broken email address'
```

运行测试，确定hotfix分支是正确的，合并hotfix分支到master分支部署到生产环境。

```
git checkout master
git merge hotfix
```

切换回master，删除hotfix分支

```
git branch -d hotfix
```

切换到工作区

```
$ git checkout iss53
Switched to branch "iss53"
$ vim index.html
$ git commit -a -m 'Finish the new footer [issue 53]'
```

## 基本合并

```
git checkout master
git merge iss53
```

现在可以不用iss53分支

```
git branch -d iss53
```

## 基本的合并冲突

## 分支管理

查看创建的分支

```
git branch
```

查看分支的commit信息

```
git branch -v
```

--merged查看哪些分支已经合并到当前分支

```
git branch --merged
```

--no-merged查看哪些分支没有合并到当前分支

```
git branch --no-merged
```

如果git branch -d删除分支失败，可以用-D强制删除

重命名分支

```
git branch --move bad-branch-name corrected-branch-name
```

查看我们的位置

```
git branch --all
```

更改命名后的分支任然在远程仓库，可以删除它

```
git push origin --delete bad-branch-name
```

回滚某个版本

```
git reset --hard commit_id
```

