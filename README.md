1、
安装完成后，在开始菜单里找到“Git”->“Git Bash”,进一步设置，在命令行输入：

$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"

注：
git config命令的--global参数是全局参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。

查看系统config
git config --system --list　　

查看当前用户（global）配置
git config --global  --list

查看当前仓库配置信息
git config --local  --list

2、创建空仓库  
$ mkdir 文件夹名  //注：自己创建文件夹目录（文件夹名尽量不要用中文）
$ cd 文件夹名    //注：进入进创建的文件夹
$ pwd
/Users/michael/文件夹名
$ git init     //通过git init命令把这个目录变成Git可以管理的仓库
​     在本地新建一个repo,进入一个项目目录,执行git init,会初始化一个repo,并在当前文件夹下创建一个.git文件夹.
Initialized empty Git repository in /Users/michael/文件夹名/.git/

3 文件操作
【工作区，版本库（暂存区（stage），master）】//创建Git版本库时，Git自动为我们创建了唯一一个master分支，所以，现在，git commit就是往master分支上提交更改

把文件往Git版本库里添加的时候，是分两步执行的：
第一步是用git add把文件添加进去，实际上就是把文件修改添加到暂存区；
第二步是用git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支。

$ git add <file> 文件名   // 添加文件
$ git commit -m <message>  //提交文件，-m后面输入的是本次提交的说明，可以输入任意内容 （-m 后面可不要）
$ git status  //命令查看仓库当前的状态
​     git status -s: -s表示short, -s的输出标记会有两列,第一列是对staging区域而言,第二列是对working目录而言.
$ git diff <file> //查看修改的内容
  //git diff HEAD -- <file>//命令可以查看工作区和版本库里面最新版本的区别(HEAD表示最新的版本)

     不加参数的git diff:
     show diff of unstaged changes.
     此命令比较的是工作目录中当前文件和暂存区域快照之间的差异,也就是修改之后还没有暂存起来的变化内容.
     
     若要看已经暂存起来的文件和上次提交时的快照之间的差异,可以用:
     git diff --cached 命令.
     show diff of staged changes.
     (Git 1.6.1 及更高版本还允许使用 git diff --staged，效果是相同的).
     
     git diff HEAD
     show diff of all staged or unstated changes.
     也即比较woking directory和上次提交之间所有的改动.
     
     如果想看自从某个版本之后都改动了什么,可以用:
     git diff [version tag]
     跟log命令一样,diff也可以加上--stat参数来简化输出.
     
     git diff [branchA] [branchB]可以用来比较两个分支.
     它实际上会返回一个由A到B的patch,不是我们想要的结果.
     一般我们想要的结果是两个分支分开以后各自的改动都是什么,是由命令:
     git diff [branchA]…[branchB]给出的.
     实际上它是:git diff $(git merge-base [branchA] [branchB]) [branchB]的结果.

$ git log --pretty=oneline  //可以查看提交历史，以便确定要回退到哪个版本  【--pretty=oneline参数可选，显示最近一个log】

$ git reflog查看命令历史，以便确定要回到未来的哪个版本

$ git checkout -- <file> //命令中的--很重要，没有--，就变成了“切换到另一个分支”的命令

命令git checkout -- <file>意思就是，把<file>文件在工作区的修改全部撤销，这里有两种情况：

一种是file自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
一种是file已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。
总之，就是让这个文件回到最近一次git commit或git add时的状态

$ git pull

​     fetch from a remote repo and try to merge into the current branch.
​     pull == fetch + merge FETCH_HEAD

​     git pull会首先执行git fetch,然后执行git merge,把取来的分支的head merge到当前分支.这个merge操作会产生一个新的commit.    

​     如果使用--rebase参数,它会执行git rebase来取代原来的git merge.
​	 
​	 
$ git reset HEAD <file>//commit之前 可以把暂存区的修改撤销掉（unstage），重新放回工作区
$ git reset --hard commit_id  //切换版本 HEAD指向的版本就是当前版本 commit_id输入版本的前几个字符就可以
git reset

​     undo changes and commits.
​     这里的HEAD关键字指的是当前分支最末梢最新的一个提交.也就是版本库中该分支上的最新版本.
​     git reset HEAD: unstage files from index and reset pointer to HEAD
​     这个命令用来把不小心add进去的文件从staged状态取出来,可以单独针对某一个文件操作: git reset HEAD - - filename, 这个- - 也可以不加.
​     git reset --soft
​     move HEAD to specific commit reference, index and staging are untouched.
​     git reset --hard
​     unstage files AND undo any changes in the working directory since last commit.
​     使用git reset —hard HEAD进行reset,即上次提交之后,所有staged的改动和工作目录的改动都会消失,还原到上次提交的状态.
​     这里的HEAD可以被写成任何一次提交的SHA-1.
​     不带soft和hard参数的git reset,实际上带的是默认参数mixed.

     总结:
     git reset --mixed id,是将git的HEAD变了(也就是提交记录变了),但文件并没有改变，(也就是working tree并没有改变). 取消了commit和add的内容.
     git reset --soft id. 实际上，是git reset –mixed id 后,又做了一次git add.即取消了commit的内容.
     git reset --hard id.是将git的HEAD变了,文件也变了.
     按改动范围排序如下:
     soft (commit) < mixed (commit + add) < hard (commit + add + local working)

$ git rm  -- <file>   //删除文件   注：删掉后必须Git commit
​     git rm file: 从staging区移除文件,同时也移除出工作目录.
​     git rm --cached: 从staging区移除文件,但留在工作目录中.
​     git rm --cached从功能上等同于git reset HEAD,清除了缓存区,但不动工作目录树
​	 
4 GitHub上建立远程库

$ git remote add 远程库名 git@github.com:用户名/仓库名.git   //建立远程库
$ git remote   //查看远程库信息   后面加参数-v查看更详细信息
$ git remote show [remote-name] 查看某个远程仓库的详细信息

git push命令用于将本地分支的更新，推送到远程主机。它的格式与git pull命令相似。
$ git push <远程主机名> <本地分支名>:<远程分支名>

要关联一个远程库，使用命令git remote add origin git@server-name:path/repo-name.git；
关联后，使用命令git push -u origin master第一次推送master分支的所有内容；
此后，每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改

git pull 取回远程主机某个分支的更新，再与本地的指定分支合并
$ git pull <远程主机名> <远程分支名>:<本地分支名>

如果远程分支要与当前分支合并，则冒号后面的部分可以省略
$ git pull <远程主机名> <远程分支名>

git fetch命令用于从另一个存储库下载对象和引用


git fetch和git pull的区别
git fetch：相当于是从远程获取最新版本到本地，不会自动合并。
$ git fetch origin master
$ git log -p master..origin/master
$ git merge origin/master

以上命令的含义：
首先从远程的origin的master主分支下载最新的版本到origin/master分支上
然后比较本地的master分支和origin/master分支的差别
最后进行合并
上述过程其实可以用以下更清晰的方式来进行：

$ git fetch origin master:tmp
$ git diff tmp 
$ git merge tmp

2. git pull：相当于是从远程获取最新版本并merge到本地

git pull origin master
上述命令其实相当于git fetch 和 git merge
在实际使用中，git fetch更安全一些，因为在merge前，我们可以查看更新情况，然后再决定是否合并。





5 从远程库克隆
git clone git@github.com:用户名/远程库名.git


要克隆一个仓库，首先必须知道仓库的地址，然后使用git clone命令克隆。
Git支持多种协议，包括https，但通过ssh支持的原生git协议速度最快

6创建与合并分支
查看分支：git branch
创建分支：git branch <name>
切换分支：git checkout <name>
创建+切换分支：git checkout -b <name>
合并某分支到当前分支：git merge <name>  //合并分支时；提示Already up-to-date，但是代码没合并时，切回主分支，执行命令git reset --hard 分支名;
删除分支：git branch -d <name>   //当d为大写D时，表示强制删除

git branch -l :查看本地分支
git branch -r :查看远程分支
git branch -a :查看全部分支（远程的和本地的）

解决冲突
当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。
解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交（add，commit）。
用git log --graph命令可以看到分支合并图。
git log

​     show commit history of a branch.

​     git log --oneline --number: 每条log只显示一行,显示number条.

​     git log --oneline --graph:可以图形化地表示出分支合并历史.

​     git log branchname可以显示特定分支的log.

​     git log --oneline branch1 ^branch2,可以查看在分支1,却不在分支2中的提交.^表示排除这个分支(Window下可能要给^branch2加上引号).

​     git log --decorate会显示出tag信息.

​     git log --author=[author name] 可以指定作者的提交历史.

​     git log --since --before --until --after 根据提交时间筛选log.

​     --no-merges可以将merge的commits排除在外.

​     git log --grep 根据commit信息过滤log: git log --grep=keywords

​     默认情况下, git log --grep --author是OR的关系,即满足一条即被返回,如果你想让它们是AND的关系,可以加上--all-match的option.

​     git log -S: filter by introduced diff.

​     比如: git log -SmethodName (注意S和后面的词之间没有等号分隔).

​     git log -p: show patch introduced at each commit.

​     每一个提交都是一个快照(snapshot),Git会把每次提交的diff计算出来,作为一个patch显示给你看.

​     另一种方法是git show [SHA].
​     git log --stat: show diffstat of changes introduced at each commit.
​     同样是用来看改动的相对信息的,--stat比-p的输出更简单一些.

bug分支

修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；

当手头工作没有完成时，先把工作现场git stash一下，然后去修复bug，修复后，再git stash pop，回到工作现场



本地新建的分支如果不推送到远程，对其他人就是不可见的；

从本地推送分支，使用git push origin branch-name，如果推送失败，先用git pull抓取远程的新提交；

在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致；

建立本地分支和远程分支的关联，使用git branch --set-upstream branch-name origin/branch-name；

从远程抓取分支，使用git pull，如果有冲突，要先处理冲突。



$ git stash  把当前的改动压入一个栈.
​     git stash将会把当前目录和index中的所有改动(但不包括未track的文件)压入一个栈,然后留给你一个clean的工作状态,即处于上一次最新提交处.
​     git stash list会显示这个栈的list.
​     git stash apply:取出stash中的上一个项目(stash@{0}),并且应用于当前的工作目录.
​     也可以指定别的项目,比如git stash apply stash@{1}.
​     如果你在应用stash中项目的同时想要删除它,可以用git stash pop

     删除stash中的项目:
     git stash drop: 删除上一个,也可指定参数删除指定的一个项目.
     git stash clear: 删除所有项目.


7创建标签
命令git tag <tagname> <commit_id>用于新建一个标签，默认为HEAD，也可以指定一个commit id；
命令git tag -a <tagname> -m "XXXX..."可以指定标签信息；
命令git tag可以查看所有标签。

命令git push origin <tagname>可以推送一个本地标签；
命令git push origin --tags可以推送全部未推送过的本地标签；
命令git tag -d <tagname>可以删除一个本地标签；
命令git push origin :refs/tags/<tagname>可以删除一个远程标签

8配置别名
$ git config --global alias.st status                 git status ==git st
$ git config --global alias.co checkout               git checkout==git co  
$ git config --global alias.ci commit                 git commit==git ci  
$ git config --global alias.br branch                 git branch==git br  



9常用命令
  mkdir：         XX (创建一个空目录XX指目录名)
   pwd：          显示当前目录的路径。
   git init          把当前的目录变成可以管理的git仓库，生成隐藏.git文件。
   touch           xx文件或者新建文件
   git add XX       把xx文件添加到暂存区去。
   git commit –m “XX”  提交文件 –m后面的是注释。
   git status        查看仓库状态
   git diff  XX     查看XX文件修改了那些内容
   git log          查看历史记录
   git reset  --hard HEAD^
   cat XX         查看XX文件内容
   git reflog       查看历史记录的版本号id
   git checkout -- XX  把XX文件在工作区的修改全部撤销。
   git rm XX          删除XX文件
   git remote add originhttps://github.com/sgl/testgit 关联一个远程库
   git push –u(第一次要用-u 以后不需要) origin master 把当前master分支推送到远程库
   git clonehttps://github.com/sgl/testgit  从远程库中克隆
   git checkout –b dev  创建dev分支 并切换到dev分支上
   git branch  查看当前所有的分支
   git checkout master 切换回master分支
   git merge dev    在当前的分支上合并dev分支
   git branch –d dev 删除dev分支
   git branch name  创建分支
   git remote 查看远程库的信息
   git remote –v 查看远程库的详细信息
   git push originmaster  Git会把master分支推送到远程库对应的远程分支上 

   

 10  SSH key（配置好服务器后，提交或者读取需要密码时，此方法也可解决）

  ① $ ssh-keygen -t rsa -C "youremail@example.com"  \\生成SSH Key。在windows下查看[c盘->用户->自己的用户名->.ssh]，id_rsa私钥、id_rsa.pub公钥
  ②登录github。打开setting->SSH keys，点击右上角 New SSH key，把生成好的公钥id_rsa.pub放进 key输入框中，再为当前的key起一个title来区分每个key。
