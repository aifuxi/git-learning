### git基础概念
>  客户端并不只是提取最新版本的文件快照，而是把代码仓库完整的镜像下来
>  git核心本质上是一个键值对数据库。可以向该数据库插入任意类型的内容，
>  他会返回一个键值，通过该值可以在任意时刻再次检索该内容。

### 区域

| 区域   | 介绍                                        |
| ------ | ------------------------------------------- |
| 工作区 | 沙箱环境 git不会管理 随便更改操作           |
| 暂存区 | 记录文件的操作                              |
| 版本库 | 最终的代码实现提交到这里 .git目录就是版本库 |



### .git目录下文件的介绍

| 目录名      | 介绍                                     |
| ----------- | ---------------------------------------- |
| hooks       | 钩子函数的一个库 类似于回调函数          |
| info        | 包含一个全局性的排除文件                 |
| objects     | 目录存储所有数据内容                     |
| refs        | 目录存储指向数据（分支）的提交对象的指针 |
| config      | 文件包含项目特有的配置选项               |
| description | 显示对仓库的描述信息                     |
| HEAD        | 文件目前被检出的分支                     |
| logs        | 日志信息                                 |
| index       | 文件保存暂存区的信息                     |



### 基础Linux命令

```shell
# 清除屏幕
$ clear
# 将内容写入到test.txt中(添加)
$ echo 'hello word '>test.txt
# 将当前目录下的子目录以下面这种方式展现出来
$ ll
total 16
-rw-r--r-- 1 Administrator 197121 13540  5月 15 08:40 git.md
# 将当前目录下的子目录以及文件也展现出来
$ find ./ 
# 只讲文件展现出来
$ find ./ -type f 
# 删除文件
$ rm text.txt
# 删除目录及目录下的文件
$ rm -rf dirname/filename
# 在同一目录下是表示重命名文件,不在同一目录下表示移动文件
$ mv a.txt b.txt # 重命名文件
$ mv a.txt dirname/filename # 移动文件
# 查看文件内容
$ cat a.txt 
```



### 初始化git

```shell
# 初始化仓库 生成.git文件
$ git init
# 配置git中的user.name
$ git config --global user.name "xxx"
# 配置git中的user.email
$ git config --global user.email "xxx@xxx.xxx"
# 查看配置信息
$ git config --list
```



### 添加到暂存区

```shell
# 首先将工作区（文件）做成git对象放到版本库,然后再放到暂存区 但是这里没    有生成树对象
$ git add . # 添加当前目录所有修改到暂存区
$ git add filename # 添加单个文件到暂存区
$ git ls-files -s  # 查看暂存区的当前状态
```



### 添加到版本库

```shell
# 比较少的注释,或单行注释
$ git commit -m '提交的信息'
# 多行注释,或很长的注释
$ git commit -m 回车 # 然后会进入vim编辑器,可以编辑很多注释了
```



### 高级命令（crud）

```shell
$ git init               # 初始化
$ git add ./             # 添加暂存区   
$ git commit -m "注释"    # 提交项目
$ git status             # 查看当前状态
$ git diff               # 查看当先做的哪些更新没有暂存
$ git diff -cached       # 查看哪些已经更新好了准备下次提交
$ git commit -a -m       # git自动将已经跟踪过的文件暂存起来一并提交
$ rm yd.txt              # 删除文件 暂存区里没有文集 版本库多了一个提交对象不过没有内容
$ mv z.txt zx.txt        # 重新起名字跟已修改一样的操作
$ git add ./
$ git commit -m "rename zx"
$ git log 				 # 普通方式查看提交历史记录
$ git log --oneline      # 用一行查看提交历史记录
$ git relog              # 查看所有的提交历史记录
```



### 分支

> - 每一个功能都可以开一个分支，不影响主线的分支
>
> - 分支就是一个活动的指针就在提交对象的前面指向最新提交
>
> - master默认是主分支
> - 每次**切换**分支前当前分支一定要是**已提交状态** 否则会**污染**主分支 如果第一次提交了再修改的时候没有提交他就不让切换分支了

```shell
$ git branch test    # 会在当前的提交对象上创建一个分支
$ git checkout test  # 将分支转到test上面来
$ git branch -D test  # 删除分支 不能自己删自己 
$ git log --oneline --decorate --graph --all  # 查看完整的分支图(没删除前)
# 配置别名, 以后可以使用 git lol 替代git log --oneline --decorate --graph --all
$ git config --global alias.lol 'log --oneline --decorate --graph --all'
$ git branch -v  # 查看分支的最后一个提交
$ git branch test hash  # 新建一个分支到hash所对应的提交对象上去 很重要
$ git checkout -b test  # 创建分支并且切换过去
$ git checkout name     # 切换分支的时候一定要提交完的时候再切否则会出现问题
$ git merge 分支名      # 合并分支
```

### 存储

> 这个分支的功能做到一半,并没有做完,需要紧急切换到其他分支,可以使用git stash存储起来,不用硬去提交了

```shell
$ git stash list # 查看存储
$ git stash apply  # 拿出栈顶的元素(存储) 但是不会删除
$ git stash drop 存储的名字 # 删除存储
$ git stash pop 存储的名字 # 拿出栈顶的元素(存储) 并且删除
```



### 后悔药
```shell
# 工作区撤回在工作目录中的修改
$ git checkout -- filename  本质是相当于重置
# 暂存区撤回自己的暂存
$ git reset HEAD filename
# 提交区注释写错了修改注释
$ git commit --amend
# 回到某个版本
$ git reset --hard 版本的hash值
```
### 项目经理远程操作github仓库步骤

1. 先在github上创建一个空的仓库new repository 注意不要有readme.md文件

2. 创建本地仓库然后基础设置   `git init`

3. 然后给github上面的地址起别名和用户别名
   
  ```shell
  $ git remote add use https://github.com/zhaoyuanmeng/git_to_use.git
  $ git config --list
  ```

  

4. 注意如果是复制别人的github 要把.git删掉只复制项目部分代码到自己的本地仓库

5. 注意凭据 本人是可以直接上传的

6. 检查完毕后推送到远地仓库  `git push use(别名) master（分支）`

7. 给员工开放权限通过github里面的manage access contributor

8. 获取员工上传的代码 `git fetch use(别名)`

9. 切换成远程跟踪分支 `git checkout use/master`

10. 合并远程跟踪分支 `git merge use/master` 

11. `git pull`  获取数据并合并

### 员工拉取项目经理的仓库代码
1. 本地不用创建仓库 直接克隆下来
   
    `git clone https://github.com/zhaoyuanmeng/git_to_use.git`
    
2. 它自动创建一个别名; 查看别名`git remote -v`       

3. 创建新的文件 `echo "hello world">test.txt`

4. `git add ./`   

5. `git commit -m "tijiao`"

6. `git push origin master`

### 本地分支 远程分支 远程跟踪分支

1. 本地分支是本机电脑的
2. 远程分支是github上对应的分支
3. 远程跟踪分支是本地与远程分支的一个映射
4. 成员克隆远程仓库以后默认本地分支和对应的远程跟踪分支有同步关系
5. 在push的时候会生成对应的远程跟踪分支
6. 在fetch的时候把数据下载到远程跟踪分支里面
7. 注意成员开辟新分支提交的时候 经理在fetch的时候要创建对应的分支不用加别名
8. 主分支和远程跟踪分支自动绑定的功能(默认情况下push的时候)
9. 建立同步关系 `git branch -u (远程跟踪分支)` 注意要在那个分支里面输入这个命令

```shell
$ git checkout --track remote别名/分支名  # 最自动创建本地分支并且与远程跟踪分支绑定
$ git checkout -b 分支名 remote别名/分支名 # 效果与上面一样
```

### 删除远程分支
```shell
$ git push use(别名) --delete (分支名)  # 删除远程分支
$ git remote prune use --dry-run  # 列出仍在远程跟踪但是远程分支已经被删除的无用分支
$ git remote prune use    # 清除上面的命令列出来的远程跟宗
```
### 冲突

- git本地操作的冲突
      + 典型合并的时候
- git远程协作的时候
	+ push, 两个人同时推（更改同一个文件）解决办法只能先把远程仓库拉下来 然后再更改那个, 文件然后再add commit push  
	+ pull, 更改完以后不push 直接pull会报错 远程仓库会覆盖更改的内容建议push 不过push还会出错就是上面那个错误

### SSH

> github特有的一种协议 走的不是验证密码的道路而是密匙

```shell
# 配置步骤
$ ssh -keygen -t rsa -C 邮箱名
$ # 在c:\users\Adminstor\.ssh下生成公私密匙
$ ssh -T git@github.com # 测试一下生成的密匙
# 查看生成的密匙
$ cat ~/./.ssh/id_rsa.pub
$ # 把公共的密匙复制到github账户里面setting下
$ # 好处是不用每次输密码 而且项目经理不用设置贡献者 直接把他们的公共密匙复制到自己账户    就可以了
```

### .gitnore文件

> 可以设置不要git管理的文件或目录