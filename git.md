# 杂记

## github使用
-------------
    ### 步骤总览
    
    ```
        - git init
        - git config
        - git remote add
        - git clone
        - git branch
        - git checkout                        #切换分支,提取分支的文件版本
        - git pull                            #拉去最新状态并合并,相当于fetch+merge
        - git status
        - git update-index
        - git stash
        - git add
        - git rm                              #取消add操作,不会放弃修改
        - git diff
        - git fetch                           #获取远程对应仓库的最新的内容,并不会更新,只会修改远程分支HEAD的指向
        - git merge                           #合并分支,可能会遇到冲突,并且合并后的分支会有两个父指向
        - git commit
        - git rebase                          #另一种分支合并的方法,复制分支的一系列提交记录,并将文件在另一个分支重现,合并后的记录像是顺序开发的一样,只有一个父指向,呈现出提交记录线性.就像是没有使用过合并一样.
        - git reset                           #非协作分支回滚,放弃修改,回滚到某个版本
        - git revert                          #协作分支回滚
        - git cherry-pick                     #调整并合并分支,创建一个新的分支
        - git push
        - git log
        - git show
    ```




* 基础功能

```
    git init         #初始化
    git add file/*   #添加文件

    git status                 #查看状态
    git status  -v             #查看状态以及以及提交的文件差异


    git commit -m "msg"     #提交
    git commit -am          #git add和git commit合并

    git commit -amend       #可以修改之前提交时输入的信息参数,会产生一个新提交,来替换老的提交记录
    
    git clone -b master url #克隆指定分支的文件
    git clone url           #克隆主分支的文件
    
    
    git rm file --cached    #删除某个文件,但在本地保留
    git rm file -f          #强制删除
    
    
    
    git config --list       #查看配置列表
    git config --global user.name "name"     #设置用户名
    git config --global user.email "email"   #设置email
    git config --global core.editor          #设置编译器
    git config --global http/https.proxy 127.0.0.1:1081                 #如果挂了梯子git拉取还是太慢可以设置代理,只有http/https生效
    git config --global --unset http/https.proxy                                     #取消设置的项
    git config --global core.autocrlf input/true                                          #处理换行,提交时转换成LF,检出时不转换/提交时转换LF,检出时转换CRLF
    git config --global core.safecrlf true                                                #禁止提交两种换行




    
    git branch -M ***main***                        #修改当前分支的名字
    git branch -d/-D name                           #删除分支/强制删除分支
    git branch [-l]                                 #查看本地分支
    git branch -r                                   #查看分支
    git branch -v[v]                                #查看各分支最后一次的提交id值 -vv代表查看分支关联的远程分支
    git branch --merged/--no-merged                 #查看已经合并/未合并到本地的分支
    git branch --set-upstream-to/--track ***master origin/master***                  #设置本地仓库与远程仓库间的映射


    git tag v1.0 HEAD                                                                #为指定提交点建立tag,tag不支持移动,可以切换,但会使HEAD游离,不能commit提交,不像分支一样易变,如果不指定提交hash会默认为HEAD
    git describe ***ref**                                                            #查询指定分支的描述,<tag><num><ref>,为tag,间隔了几次提交,当前的commit hash,用于tag

    git push -u origin master                                                        #将当前分支上传至服务器,没有则创建.
    git push origin main                                                             #将本地分支推送到服务器的指定分支上
    git push origin local^:origin                                                     #将指定本地分支推送到服务器上,^可选,表示当前分支的上一次提交
    git push -u ***localbranch-name***  ***origin  branch-name***    [-f]            #将指定分支上传到服务器的对应分支 加上-f选项可以强制上传,忽略冲突
    git push ***origin :remote-branch***               #推送一个空分支到远程仓库,相当于删除远程分支
    git push origin --delete branch                    #删除远程分支
    git push origin --delete tag tagname               #删除远程tag 
    
    
    git pull --all                                  #拉取所有分支
    git pull origin ***master***                    #拉取指定分支 
    git pull --rebase origin master                 #相当于fetch,rebase
    git pull                                        #相当于fetch,merge
    
    git fetch remote-branchname(origin)                    #获取远程仓库相应分支的最新提交.从远程仓库下载本地仓库缺失的提交记录,并更新本地远程仓库的指针.只是下载最新的数据,并不会自动合并本地仓库的内容.不带参数,会下载所有提交记录到所对应的分支
    git fetch origin remote:local                   #将指定远程分支下载到指定的本地分支下，没有则创建
    git fetch -p                                    #fetch补丁之后删除与远程分支没有对应的本地分支
    
    
    git merge ***origin/master***                  #将远程分支合并到当前分支,这种合并是逻辑合并,只是指向合并的父节点
    git merge ***branch***                         #将指定分支合并到当前分支
    git merge ***origin/master*** --no-ff          #不要Fast-Foward合并,可以生成merge提交记录,自己修改一些提交信息
    
    git rebase ***branch***                        #以指定分支为基复制当前的分支链在构建在其下面,合并后只留下一个父指向.此合并是复制合并,相当于每次子提交都像是在本分支发生的. 并且当前分支向前移动.
    git rebase ***branch*** ***branch***           #以前者为基复制添加后者的记录,rebase会使当前分支向前移动,也就是比原分支领先一个分支.原分支不应该再使用了,或者原分支也rebase一下
    

    
    git reset HEAD~2            #回滚到上两个版本.就像是改写历史一样,并且不会生成提交记录,那些被撤销的分支就行重来没有提交过,回滚后那些版本所做的修改还在,但是未加入暂存.对于多人协作的分支,请时使用revert
    git reset file              #回滚指定文件
    git reset HEAD file         #回滚文件到指定分支时的状态


    git revert HEAD^            #这种方式回滚,会增加一个提交,用来表示回滚到某个提交.^是可选的表示相对位置,也可指定精确位置


    git update-index --assume-unchanged filename    #不跟踪文件状态
    git update-index --no-assume-unchanged filename #开始跟踪文件状态
    
    
```

* git的进阶功能

```
        
    git checkout branchname                        #切换分支
    git checkout -b name                           #创建并切换分支
    git checkout  -b  newname oldname              #基于老分支创建新分支
    git checkout -b ***local***   ***origin/branch***  #根据远程分支创建本地分支
    git checkout $id                               #把某次历史提交checkout出来查看,不会创建分支,切换到其他分支会自动删除
    git checkout $id -b newname                    #把某次历史提交checkout出来查看,并创建一个分支保存
    git checkout --track ***origin/branch***       #将分支检索出来并设置HEAD跟踪
    git switch ***branch***                        #仅仅切换分支
    
```


* git 分支管理

```
    git remote 
    git remote -v                       #查看远程仓库对应的url
    git remote show                     #显示远程仓库
    git remote add ***shortname*** ***url***           #添加一个远程仓库,并设置缩写
    git remote set-head origin master                  #设置远程仓库的HEAD指向master分支
    git remote set-url ***origin  url***               #设置远程仓库地址,可以修改已存在的
    git remote rename oldname new name                 #重命名远程仓库的简写 
    git remote rm remotename                           #移除一个远程仓库的引用
    git remote prune origin                            #清理无效的远程分支
    git remote prune origin --dry-run                  #查看哪些分支需要清理


    git stash                                      #暂存状态
    git stash apply                                #回复暂存
    git stash list                                 #查看暂存列表
    git stash drop                                 #删除暂存区
    


    HEAD                                           #HEAD代表了当前正在操作的是哪个分支,默认指向正在工作的分支,但可以手动分离.分支可以看作是一个状态点,而HEAD才是代表着正在活跃的状态
    git symbolic-ref HEAD                          #查看HEAD当前的状态
    git checkout 提交记录(哈希值)                     #切换到某次提交 
    git checkout main^/main~                       #直接使用hash值来切换到某次提交不方便,所以可以使用相对定位,^main表示main分支的上一次提交,同理^^表示上两次的那次提交.如果有多个父记录,默认第一个
    git checkout main^2                            #当有多个父记录时,这条命令选择第二个父记录
    git checkout main~3                            #表示多次之前的
    git checkout HEAD^~3~6                         #链式操作
    git checkout HEAD^/HEAD~4                      #也可以以当前HEAD作为参照物进行移动
    git branch -f main HEAD~3                      #强制main分支指向相对于HEAD的上三级分支,原main位置新提交的分支处于游离状态






    git --bare init                                    #创建纯仓库
    git clone --bare                                   #克隆带版本的项目,但不checkout
    scp -r my.git git@server.com                       #将仓库上传至服务器

    
```


* 其他

```

    git cherry-pick c4 c2                          #将c4 c2分支顺序取出来复制到当前分支,适用于多分支挑选合并.与rebase差不多,但是rebase不能挑选指定分支.这些分支的父分支.可以当作反撤销使用
    git rebase --interactive/-i                    #交互式rebase,显示一个列表文件,供你挑选要合并的分支
    git commit --amend                             #使用修改之前的提交,和上面配合使用





    .gitattributes                                  #用于忽略一些文件的转换,比如批处理文件,图像数据文件,应用文件等二进制文件被当成文本被意外转换
    .gitignore                                      #忽略追踪配置文件
    git help command    #显示命令的帮助
    git log                                         #查询提交日志 -p -2可以查询最近两次的详细修改
    git log -20 --stat                              #查询提交记录,并显示提交的状态信息在每次提交的后面有一个总结
    
    git show            #显示最近一次的提交信息
    
    git diff            #查看未暂存的改动  可加文件名查看指定文件
    git diff --cached   #查看暂存区内的改动,对比暂存区与仓库


    ssh-keygen -t rsb -b 4096 -C "email"            #生成密钥对 id_rsa
    ssh -T git@github.com                           #测试ssh可访问性
    ssh -i keyfile root@host                        #ssh登录远程计算机
    
    
    
    
    匹配规则:a/**/z                                  #可以匹配a/b/z,a/b/c/z这类目录,


```



## 遇到ssh下载慢问题

在.ssh目录下建立config文件即可


Host ssh.github.com
  User git
  Port 443
  Hostname ssh.github.com
  IdentityFile "D:\ide\home\.ssh\id_rsa"
  TCPKeepAlive yes
  ProxyCommand "D:\ide\other\Git\mingw64\bin\connect.exe" -S 127.0.0.1:1080 -a none %h %p

```
