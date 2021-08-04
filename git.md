初次安装git,配置提交人信息

```git
git config --global user.name xxx
git config --global user.email xxx
git config --list   //查看配置信息
```

git基本提交步骤

```git
git init                         //第一次配置：初始化本地仓库

git pull 远程url   //每次提交前先拉取一下
git status         //查看文件状态
git add .          //添加所有文件到暂存区  add 文件名    添加指定文件
git commit -m xxx  //提交当前分支     -o 文件名 -m xxx  提交指定文件

git remote add origin 远程url   //第一次配置：远程仓库别名
git push -u origin             //第一次配置：下次提交可以直接给push

git push           //提交

git log            //查看提交记录
```

第一次拉取克隆项目

```git
git clone 远程url
```

开发到一半需要切换分支，先复制，回到分支在粘贴即可

```git
git stash       //复制
git stash pop   //粘贴
```

分支命令

```git
git branch        //查看所有分支
git branch        //查看远程分支
git branch xxx    //创建xxx分支   -b xxx 创建并切换到xxx分支
git checkout xxx  //切换到xxx分支
git merge xxx     //把xxx分支和当前所在分支合并
git branch -d xxx //删除xxx分支

```

配置ssh

```git
ssh-keygen
cd ~/.ssh 
cat id_rsa.pub                     //复制输出的一串密钥
头像--settings--SSH and GPG keys   //new 粘贴
```