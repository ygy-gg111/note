git操作本地仓库

初始化 git config --global user.name ""

​              git congif --global user.email ""

建立仓库

git init 

git status 查看仓库状态

提交文件到暂缓区

git add [文件名 文件2 文件3]  把文件提交到

git add . 把当前目录下所有文件提交到暂缓区

git commit -m "注释"

版本回退

查看之前当前的操作 得到 head码

git log 

git log --pretty--=oneline  推荐

查看未来的操作 如果我们一不小心关掉了git 可以通过这个查看

git reflog  



回到当前的版本

git reset --hard  head码



github线上仓库

1使用clone指令克隆

https协议

git clone 线上仓库地址 https :





每天下班 git push 把本地修改的上传到github, 

每天上班git pull 把线上的更新下来







github 分支控制

git branch 分支名  新建分支

git checkout 分支名 切换到分支

分支合并

git  merge  要合并的分支



删除分支

git branch -d 分支名

