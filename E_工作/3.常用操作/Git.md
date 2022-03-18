#### **设置用户名和邮箱**

大公司都有自己的提交规范，要注明用户名和邮箱否则无法提交，有的公司（比如我现在在的），还会用husky自动检查commit信息是否规范

```bash
git config --global user.name "你的名字" //让你全部的Git仓库绑定你的名字
git config --global user.email "你的邮箱" //让你全部的Git仓库绑定你的邮箱
```

#### **连接远程仓库**

```bash
git remote add origin https://github.com/name/name_cangku.git //把本地仓库与远程仓库连接起来
```

#### **增加push的时候推送的源**

```bash
git remote set-url --add origin xxxxxxx(仓库地址)
```

<u>常见于我的个人项目，需要同时推送到github和gitee</u>

#### **查看当前远程仓库的信息**

```bash
git remote //查看远程库的信息，会显示origin，远程仓库默认名称为origin
git remote -v //显示更详细的信息
```

#### **将整个远程仓库克隆到本地**

```bash
git clone 仓库地址
```

#### **查看当前仓库的状态**

```bash
git status
```

#### **当一个项目有多个特性分支，不同小组的开发怎么创建分支？**

> 1. 先在gitlab上新建一个分支，选定从你小组所在的特性分支分支上新建
>
> 2. 然后在本地终端拉取所有远程分支：`git fetch --all`
>
> 3. 然后再创建本地分支：
>
>    ```bash
>    git checkout -b LocalDev origin/Remotedev (LocalDev 为本地分支名，Remotedev 为远程分支名)
>    ```
>
> 到这就大功告成了，在自己的分支愉快地玩耍吧

#### **分支的创建和切换**

```bash
git branch  //查看当前所有分支
git branch <分支名字>  //创建分支
git checkout <分支名字>  //切换到分支
git checkout -b <分支名字>  //创建并切换到分支
```

#### **将工作区的文件放到暂存区**

```bash
git add .     //把工作区的文件全部提交到暂存区
git add ./<file>/    //把工作区的<file>文件提交到暂存区
```

#### **拉取远程最新的代码**

```bash
git pull  //把最新的提交从远程仓库中抓取下来，并在本地合并,和git push相反
git fetch //把最新的提交从远程仓库中抓取下来，不合并
pull相当于fetch+merge
```

#### **合并分支**

```bash
git merge <分支名字>  //合并分支
```

#### **查看文件修改的具体内容**

```bash
git diff
```

#### **将暂存区的文件提交**

```bash
git commit -m "xxx" //把暂存区的所有文件提交到仓库区，暂存区空空荡荡
```

#### **查看提交历史**

```bash
git log   //显示从最近到最远的提交历史  
git log --pretty=oneline   (更简洁的方式显示)
```

#### **将当前暂存区的文件追加到上次的commit**

```bash 
git commit --amend   //然后:wq退出编辑
或修改上次的commit内容，则使用
git commit --amend -m "修改commit内容"
```

使用amend追加的commit，无法直接push，需要用到下面这条指令

#### **回退到当前分支的某个版本 硬重置**

```bash
git reflog //查看所有分支的所有操作记录
git reset --hard 版本号    //版本号就是asd1e345dsffsas之类的乱码  硬重置会丢弃暂存区和工作区
```

#### **回退到最近一次的提交之前 软重置**

```bash
git reset —soft HEAD~1    //暂存区和工作区都还在
```

#### **查看所有分支的所有操作记录**

```bash
git reflog   //能看到commit和reset 包括被删除的commit
```

#### **强制推送到远程分支**

```bash
git push origin xxx(远程分支) -f
```

#### **删除远程分支**

```bash
git push origin --delete <分支名字>
```

最好不要这么玩，一般代码提交合并请求的时候，可以勾选**合并后删除来源分支**

#### **删除本地分支**

```bash
git branch -d <分支名字>  //删除分支,有可能会删除失败，因为Git会保护没有被合并的分支
git branch -D <分支名字>  //强行删除，丢弃没被合并的分支
```

#### **储藏变更 还原到分支被创建时候的样子**

```bash
git stash  //当有其他任务插进来时，把当前工作现场“存储”起来,以后恢复后继续工作
```

#### **储藏时记录消息**

```bash
git stash save <message>
或
git stash push -m <message>
```

#### **查看暂存列表**

```bash
git stash list  //查看你刚刚“存放”起来的工作去哪里了
```

#### **取出储藏**

```bash
git stash pop  //取出最近的一次储藏，同时把stash记录也删了
git stash drop stash@{x}   //删除stash内容
git stash apply stash@{x}   //恢复却不删除stash记录
```
