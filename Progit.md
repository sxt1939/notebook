安装与版本

```shell
#安装
sudo yum install git
sudo apt-get install git 

#查看版本
git --version

#生成公钥
ssh-keygen -t rsa -b 2048 -C "email"
ssh-keygen -t rsa -C "email"
```



配置config

```shell
#config
vim ~/.gitconfig
	git config --list --show-origin 查看用户信息以及所在的文件位置
	git config user.name
	git config user.email

	#全局配置用户信息；或者在特定项目目录下不加--global单独配置此项目的用户信息
	git config --global user.name "xxx"
	git config --global user.email "xxx"

	git config --gloabl alias.st "status -s" #设置别名
	git config --global color.ui true
```

初始化本地仓库init

```shell
git init --initial-branch=main
```



克隆仓库clone

```shell
git clone -b dev 项目地址	#dev为远端分支名  -b指定仓库分支
git clone -b dev 项目地址 repname	#dev为远端分支名  -b指定仓库分支 repname为自定义名字	
git clone --recursive url #将子目录也克隆下来
```

拉取远端代码fetch

```shell
git fetch --all
```



拉取并合并远端代码pull

```shell
git pull -r #拉取公共代码时rebase一下
git pull origin master:dev #拉取远程master分支与本地dev分支合并
git pull <远程主机名> <远程分支名>:<本地分支名>
git pull <remote> <branch>
```

合并分支merge

```sh
git merge dev	#将dev分支代码合并到当前分支
git mergetool #使用工具解决冲突
```

变基rebase

```shell
git checkout feature
git rebase master
#这两条命令等价于git rebase master feature
```



推送代码push

```shell
git push -u origin master #-u是告诉git去跟踪推送的分支，git会把本地和远端的master分支关联起来 之后推送就可以直接git push
git push origin local_branch:remote_branch #将本地的分支推送到远端分支
git push -u origin --all
git push -u origin --tags
```

删除远程分支

```shell
git push origin --delete dev #dev为远程分支
```



检出分支checkout/switch

```shell
git checkout -b dev origin/dev #检出一个分支且本地dev会跟踪远程dev 本地dev名字可以更改
git checkout --track origin/dev #同样也会检出一个分支并跟踪远端分支
git switch -c main
```



添加与提交commit

```shell
git commit -m "remove test.txt"
git commit -am # -a跳过add 直接添加和提交--前提是这个文件已经被跟踪
git commit --amend
```

删除文件rm

```shell
#删除文件 rm
git rm xxx	#从版本库中删除文件
git rm xx.file #删除文件
git rm --cached xx.file #删除文件 --cached从仓库中删除但是保留到工作区，如提交了日志文件
```

删除本地分支

```shell
git branch -d xxx #删除本地某个分支 分支未合并时会提示删除失败，-D可强制删除
```



更改文件名mv

```shell
#更改文件名
git mv a.txt b.txt #等价与 1: mv a.txt b.txt 2:git rm a.txt 3:git add b.txt
```



分支branch

```sh
git branch	#查看当前分支
git branch -v #查看所有分支的最后一次提交
git branch -vv #查看所有分支的跟踪关系
git branch -a	#查看分支本地和远程
git branch --merged #过滤出当前已合并的分支 分支前不带*的说明已合并
git branch --no-merged

git branch -u origin/dev #设置当前分支跟踪远端的dev分支  或者--set-upstream-to
git branch --set-upstream-to=origin/<branch> local_branch
```



查看远程信息remote

```shell
git remote add mypro url#添加一个远程仓库
git remote update origin --prune #同步远端所有分支
git remote -v #查看当前仓库地址
git remote show origin #查看当前仓库地址

```



修改差异diff

```shell
git diff [xxx] #查看[xxx]文件的差异修改
git diff #查看工作目录中当前文件和暂存区域快照之间的差异，也就是修改之后还没有暂存起来的变化内容
git diff --cached #查看已暂存的将要添加到下次提交里的内容
git diff --staged #同上 Git 1.6.1支持
git difftool --tool-help#查看系统支持的Git diff插件
```



查看文件状态status

```shell
#查看文件状态
git status -s
```

合并多次提交message为一条

```shell
git rebase -i SHA1
```

储藏工作stash

```shell
git stash [save]
git stash list
git stash apply #默认应用最新的一个
git stash apply stash@{2} 

git stash drop stash@{0}
git stash pop
```



打标签tag

```shell
git tag
git tag -l 'V1.5.7.*'
git tag -a V1.0 -m "this is v1.0" #最新的一次提交打标签
git tag -a v1.0 SHA1 -m 'xxx' #给指定的提交打标签
git push origin --tags #推送所有标签
git push origin v1.0 #推送指定标签
git checkout -b dev_ver1.0 v1.0 #检出一个分支
git checkout v1.0 #切换分支
```



别名alias

```sh
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
```



查看日志log

```shell
#查看日志
git log --graph --pretty=oneline --abbrev-commit
git log --pretty=oneline	#显示一行日志
git log --oneline --decorate #查看分支所指对象
git log --oneline --decorate --graph --all #查看分叉历史
git log -1
git log -p -1 #-p显示提交的差异
git log -Sfunction_name #-S 找出改动过function_name函数的提交
git log -L :git_deflate_bound:zlib.c#查看 zlib.c 文件中`git_deflate_bound`函数的每一次变更
git log --stat #统计信息
git log --since=2.weeks
git log --pretty=format:"%p %h - %an, %ar : %s" #订制格式
git log --pretty=format:"%h %s" --graph
#format常用选项
	%H : 提交对象（commit）的完整哈希字串
    %h 提交对象的简短哈希字串
    %T 树对象（tree）的完整哈希字串
    %t 树对象的简短哈希字串
    %P 父对象（parent）的完整哈希字串
    %p 父对象的简短哈希字串
    %an 作者（author）的名字
    %ae 作者的电子邮件地址
    %ad 作者修订日期（可以用 --date= 选项定制格式）
    %ar 作者修订日期，按多久以前的方式显示
    %cn 提交者（committer）的名字
    %ce 提交者的电子邮件地址
    %cd 提交日期
    %cr 提交日期，按多久以前的方式显示
    %s 提交说明
    
git log常用选项
	-p 按补丁格式显示每个更新之间的差异。
	--stat 显示每次更新的文件修改统计信息。
	--shortstat 只显示 --stat 中最后的行数修改添加移除统计。
	--name-only 仅在提交信息后显示已修改的文件清单。
	--name-status 显示新增、修改、删除的文件清单。
	--abbrev-commit 仅显示 SHA-1 的前几个字符，而非所有的 40 个字符。
	--relative-date 使用较短的相对时间显示（比如，“2 weeks ago”）。
	--graph 显示 ASCII 图形表示的分支合并历史。
	--pretty 使用其他格式显示历史提交信息。可用的选项包括 oneline，short，full，fuller 和 format（后跟指定格式）。
	
	-(n)仅显示最近的 n 条提交
	--since, --after仅显示指定时间之后的提交。
	--until, --before仅显示指定时间之前的提交。
	--author仅显示指定作者相关的提交。
	--committer仅显示指定提交者相关的提交。
	--grep仅显示含指定关键字的提交
	-S仅显示添加或移除了某个关键字的提交
```





子模块submodule

```shell
git submodule init #拉取一个带有子目录的仓库时是空的，需要在子目录初始化
git submodule update
git diff --submodule
git submodule add <url> <path>    #url为子模块的路径，path为该子模块存储的目录路径
git submodule update --init	#更新子模块
git submodule update --remote [xx]	#更新子模块
git submodule update --remote --merge
git submodule update --remote --rebase
git submodule update --init --recursive
git log -p --submodule
git submodule foreach 'git checkout -b featureA'
#删除子模块
有时子模块的项目维护地址发生了变化，或者需要替换子模块，就需要删除原有的子模块。

删除子模块较复杂，步骤如下：

rm -rf 子模块目录 删除子模块目录及源码
vi .gitmodules 删除项目目录下.gitmodules文件中子模块相关条目
vi .git/config 删除配置项中子模块相关条目
rm .git/module/* 删除模块下的子模块目录，每个子模块对应一个目录，注意只删除对应的子模块目录即可
commit 这些修改–这一步很关键，否则会报错：already exists in the index
执行完成后，再执行添加子模块命令即可，如果仍然报错，执行如下：

git rm --cached 子模块名称

完成删除后，提交到仓库即可。

#修改子模块到指定标签
进入到子模块目录下
git tag #查看下当前有哪些tag，没有的话git pull一下从远端拉取
git checkout xxx #切换到指定tag
```



查找命令 grep

```shell
git grep -n "hello"
git grep --count ""hello #统计hello在每个文件中出现的次数
```



```shell
git reset HEAD^ #回退到上一个版本，但修改内容保留
git reset --hard HEAD^ #回退到上一个版本，修改内容不保留
git reset --hard HEAD^	#回退到版本库中的上一个版本^  ^^两个 ^^^三个
git reflog	#记录之前回退的命令

https://blog.csdn.net/qq_48424581/article/details/123548537 #错误提交回退

git clean -df #返回到某个节点，（未跟踪文件的删除）
git clean 参数
    -n 不实际删除，只是进行演练，展示将要进行的操作，有哪些文件将要被删除。（可先使用该命令参数，然后再决定是否执行）
    -f 删除文件
    -i 显示将要删除的文件
    -d 递归删除目录及文件（未跟踪的）
    -q 仅显示错误，成功删除的文件不显示

#撤销单个文件的修改
git checkout -- [filepath]//单个文件
git checkout .//所有文件


git cat-file -p sha1
git ls-files -s
git write-tree
git show --pretty=fuller
git rev-parse V1.0 #查看V1.0版本对应的SHA1值
```

