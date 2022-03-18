**Git**

git是分布式

svn是集中式  依赖于中央服务器

------

git是一款工具

gitee和github分别是国内和国外的代码托管平台

------

**Git支持linux指令**

1. pwd //查看当前路径

2. mkdir 文件夹名 //创建文件夹

3. cd 文件夹名 //进入指定的文件夹

4. touch index.css //创建文件

5. touch base.css //创建文件

6. ls  //查看当前文件夹下所有文件和文件夹

7. vi index.css //编辑指定的文件

8. 按下"i"  进入插入模式  ==> 编写内容

9. 按下esc退出插入模式  ===>  再按住shift+冒号  ==>  输入wq回车(退出并保存)  

10. 1. 输入 q! 回车 强制退出 不保存

11. cat 文件名  //查看文件内容

12. cd .. //回到上层目录

13. mv  文件名  新的路径  //移动指定文件到指定的路径

14. mv 文件名  新文件名   //重命名

15. mv  路径/文件名  新路径/新文件名  //移动文件并重命名

16. rm 文件名 //删除指定的文件

17. rm -rf  文件夹   //强制递归删除子文件后再删除该文件夹  注：因为文件夹不能直接通过rm删除

------

**创建Git项目**

1. 创建文件夹
2. 第2步会生成一个隐藏的.git文件夹 不要删
3. 创建dist src 
4. 进入src目录  创建css js img等文件夹  html文件
5. 输入初始化指令   git init
6. 输入git status查看状态
7. (遇到红色敲)输入git add . （两个空格一个点）将项目放到暂存区
8. (遇到绿色敲)输入git commit -m "做了什么事情"  讲项目放到本地仓库

------

**回滚**

1. 疯狂添加多次提交

2. git reflog

3. 选择版本   git reset --hard 版本号

4. 1. git reset --hard head^  //一个^ 上一个版本   ^^上2个版本

------

**查看当前的用户配置**

git config --list

**设置用户邮箱**

git config --global user.email ""

**设置用户名**

git config --global user.name ""

**如果编写错误 编辑**

git config --global -e

------

**分支**

1. git branch  //查看所有分支
2. git branch dev //创建一个dev的分支
3. git branch //创建分支后再查看分支
4. git checkout master //若要合并分支先切换到主分支
5. git merge dev   //从主分支合并dev分支 
6. 处理冲突后 git add . / git commit -m ""

------

**配置远程仓库**

1. 注册登录gitee

2. 配置SSH公钥 一台电脑只配置一次

3. 生成公钥

4. 1.  ssh-keygen -t ed25519 -C "你的邮箱@×xxxx.com"

5. 得到公钥

6. 1. cat ~/ .ssh/id_ed25519.pub

7. 复制公钥 添加  https://gitee.com/profile/sshkeys

8. 创建新的仓库

9. 1. 选择开源MIT协议 -->初始化仓库

10. 到电脑上找个文件夹 git clone 远程仓库地址

11. 打开文件夹写代码

12. git add .

13. git commit -m 'xxx'

14. git push origin master 提交到远程仓库

------

**.gitignore**

配置忽略提交到本地仓库的文件或者文件夹

直接输入路径/文件名或文件夹名

如果需要提交则在前面加#号

这个文件后期会用脚手架来生成

------

git remote add 别名 远程仓库地址//添加远程仓库地址

git remote remove 别名 //移除远程仓库的连接

git  push origin  master//推送到远程仓库 第一次有可能提交失败 

git  push origin  master --force强制提交