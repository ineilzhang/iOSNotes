### Git学习

1. 能够列入代码版本控制的文件是有规定的，不能是编写的二进制文件，临时文件和用户特有的文件。
2. 在Xcode下创建的代码库就是工程目录，是一对一的关系；而在命令行下创建的目录是根目录，可以包含多个工程目录。
3. git log 常用命令
  - **git log -p -2**   显示每次提交的内容差异，用 -2 则仅显示最近的两次更新
  - **git log -stat**  显示简要的增改行数统计
  - **git log -pretty=oneline**  每个提交放在一行显示
  - **git log -pretty=full**  每个提交的全面显示
4. git show 常用命令
	- git show <git提交版本号> <文件名>   查看某次提交文件改动，文件名如果省略则查看所有文件
5. git 删除本地分支缓存列表
	- git remote prune origin
	- git fetch -p
6. git 重命名分支
	- git branch -m old new
7. git 查看分支信息(包含远程追踪分支)
	- git branch -avv
8. 