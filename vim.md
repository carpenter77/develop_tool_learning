#### git 配置

```
git config --list --global //
git config --local user.name 'xxx'//配置本地用户名
git config --local user.email 'example@gmail.com'//配置本地仓库的email
//在做code review的时候若是代码有问题，git系统也能够自动发邮件告知信息
```

#### git工作区和暂存区

add放到暂存区，修改会将git暂存区的内容覆盖掉。

```
git mv oldname newname
git log --oneline -n4// 简洁方式查看过去的提交历史 -n 指定过去的4个。
git log --all --graph //图形化，查看所有的分支
gitk //启动图形化界面
git cat-file -p xx xx哈希值，可以查看文件。
git cat-file -t xx 查看文件类型
git commit -am ""// add or/and commit 对应modify的文件可以直接提交
```

**介绍.git文件夹**

* 工作区：电脑能够看到的目录 
* 暂存区： add执行之后保持追踪

* 当切换分支的时候HEAD文件内容会变化，变成对应的分支。
* config文件中记录了email和用户
* refs文件夹：包含了tag、heads 。 仓库的许多个标签(tags)和多个分支(heads)。heads 中存放的commit的对象，这也是master指针指向的对象。tags里存放也是哈希的commit的对象的哈希指针。
* objects文件夹： pack和一些以16进制位的文件中存放了，obeject就是blob类型文件，存放修改的内容。

一次commit应该都会对应着一棵树tree，tree存放着git取出来的示图（展示给用户看到的文件），是对本参考下的所有文件的一个快照（以树形式呈现出来的）。tree中数据项： tree表示是文件夹（再指向一个tree），blob代表一个文件，文件对应目录下的文件。blob中不以文件名做区分，只要是文件的内容是相同的，则认为同是同一个blob。

```
git reset --hard 将工作区强制恢复到某次commit
```

**分离头指针**

```
git checkout commitid//进入分离头指针，可以继续进行开发，不影响分支。 本质上就是在工作在一个没有分支的情况下。经常用在尝试的时候，尝试不太好，再扔掉（也不会被显示在git branch 中）。不错就新建一个分支
git branch branchName commitid //建立一个分支出来
```

分离头指针以后，再切换回主分支以后，不会立即就清零零时分支，会过段时间清理，同时git 也会提示你。

#####  分支

```
git branch -v //查看分支的数量
git checkout -b newbranch oldbrach  //基于旧分支创建新分支，同时切换到新分支上
//Head指向不同的分支，但是Head其实指向还是某个commit
git diff HEAD HEAD~2 //HEAD^^ HEAD的gradaparent做对比。HEAD相当于一个变量。
git branch -d branchname //删除已经被merge的分支
git branch -D branchname //删除不需要merge的分支
git diff branch1 branch2 -- filename //对比两个不同分支下文件的差异，也可以直接对比不同分支的commit id 
```

##### Git commit信息修改

```
git commit --amend 对最新一次commit的修改信息做变更
git rebase -i commitid// 注意的这里的commitid应该是我们需要变更的commit的基commit。
//进入编辑界面按照提示进行某次历史的git commit进行变更。他也是通过分离头指针的方式，但是又会切换回最新的commit上
git rebase -i commitid//合并多个commit的场景，压缩commit。对应的 commit为修改squash操作，它就会被合并父基commitid上(也是commitid) (rebase中显示越往上的commit时间越早）
git rebase -i //将几个间隔的commit合并到一起，就是修改为squash操作，将你希望合并的commit和对应的基commit写到相连的位置就行。
git rebase --continue //继续刚才的操作
```

##### 暂存区的内容和Head做比较

```
git diff --cached //和暂存区做比较，我们的变更没有的问题的时候，就会放心的提交	
git diff 默认比较也是工作区和Head的内容
git diff -- filename 指定对指定的文件进行对比
git reset HEAD //删除暂存区内追踪的内容，但是本地修改还在。暂存区的目录树会被重写，被 master 分支指向的目录树所替换，但是工作区不受影响。
git rm --cached <file> 命令时，会直接从暂存区删除文件，工作区则不做出改变。
git checkout . 或者 git checkout -- <file> 命令时，会用暂存区全部或指定的文件替换工作区的文件。这个操作很危险，会清除工作区中未添加到暂存区中的改动。
git checkout HEAD . 或者 git checkout HEAD <file> 命令时，会用 HEAD 指向的 master 分支中的全部或者部分文件替换暂存区和以及工作区中的文件。这个命令也是极具危险性的，因为不但会清除工作区中未提交的改动，也会清除暂存区中未提交的改动。
```

##### 删除最近的几次commit

```
git reset --hard commitid// 工作区和暂存区都要恢复到和这次commitid一模一样的状态。
git rm -- filename //将commit文件放到暂存区
```

