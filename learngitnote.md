## 廖雪峰Git教程学习笔记

### 系统
  * Windows7

### 创建版本库
 版本库又名仓库，英文名repository，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。

* 新建一个文件夹作为git仓库，或者使用原来的文件夹也行，这里为了学习方便，新建了一个名为learngit的文件夹
  ```
  $ mkdir learngit
  $ cd learngit
  $ pwd
  /Users/(username)/learngit
  ```
* 通过`git init`命令把这个目录变成Git可以管理的仓库
  ```
  $ git init
  Initialized empty Git repository in /Users/(username)/learngit/.git/
  ```
* 把文件添加到版本库
  第一步，在learngit文件夹或者其子目录下新建一个如README.md的文件，添加如下内容：
  ```
  ## Git is a version control system.
  ### Git is free software.
  ```
  第二步，用命令git add告诉Git，把文件添加到仓库：
  ```
  $ git add README.md
  ```
  第三步，用命令git commit告诉Git，把文件提交到仓库：
  ```
  $ git commit -m "wrote a readme file"
  [master (root-commit) cb926e7] wrote a readme file
  1 file changed, 2 insertions(+)
  create mode 100644 README.md
 ```
  `git commit -m`命令，`-m`后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义，如果需要添加多个添加多个文件，则可以多次`add`不同文件，然后再一次性`commit`上去。
  ```
  $ git add file1.md
  $ git add file2.md file3.md
  $ git commit -m "add 3 files."
  ```

### 时光机穿梭
  * 修改README.md里面的内容
    ```
    ## Git is a distributed version control system.
    ### Git is free software.
    ```
  * 使用`git status`命令查看结果，`git status`命令可以让我们时刻掌握仓库当前的状态
    ```
    On branch master
    Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)

            modified:   README.md

    no changes added to commit (use "git add" and/or "git commit -a")
    ```
  * `git status`可以告诉我们那个文件被修改了，但是如果需要知道具 体修改了什么内容，则需要借助`git diff`命令
    ```
    $ git diff README.md
    diff --git a/README.md b/README.md
    index f75f0ff..800cf28 100644
    --- a/README.md
    +++ b/README.md
    @@ -1,2 +1,2 @@
    -## Git is a version control system.
    +## Git is a distributed version control system.
     ### Git is free software.
    ```
    通过比对`-## Git is a version control system.`和`+## Git is a distributed version control system.`就知道我们在md文件的第一行增加了“distributed”单词
  * 提交修改后的版本到仓库
    ```
    $ git add README.md
    $ git commit -m "add distributed"
    [master 10cd03d] add distributed
    1 file changed, 1 insertion(+), 1 deletion(-)
    $ git status
    On branch master
    nothing to commit, working tree clean
    ```

### 版本回退
 * 再修改一下README.md的内容并提交一个新的版本到仓库
   ```
   ## Git is a distributed version control system.
   ### Git is free software distributed under the GPL.
   ```
   ```
   $ git add README.md
   $ git commit -m "append GPL"
   [master 4ed8895] append GPL
   1 file changed, 1 insertion(+), 1 deletion(-)
   ```
 * 通过`git log`查看由近及远的历史提交日志
   ```
   $ git log
   commit 4ed88950820409e5816e544258a5258b9d6d5fe1 (HEAD -> master)
   Author: Veeson <veesonliu@gmail.com>
   Date:   Wed Mar 28 21:17:07 2018 +0800

       append GPL

   commit 10cd03d8d1d1eb7b334a0d9b242adf15f933c234
   Author: Veeson <veesonliu@gmail.com>
   Date:   Wed Mar 28 21:10:37 2018 +0800

       add distributed

   commit 0faa019d9188e6db3de6cac48260ebbbf62095af
   Author: Veeson <veesonliu@gmail.com>
   Date:   Wed Mar 28 20:05:27 2018 +0800

       wrote a readme file
   ```
   还可以在`git log`后面加上`--pretty=oneline`参数，减少输出
   ```
   $ git log --pretty=oneline
   4ed88950820409e5816e544258a5258b9d6d5fe1 (HEAD -> master) append GPL
   10cd03d8d1d1eb7b334a0d9b242adf15f933c234 add distributed
   0faa019d9188e6db3de6cac48260ebbbf62095af wrote a readme file
   ```
 * 回滚到"add distributed"的版本
  ```
  git reset --hard HEAD^
  ```  
  Git必须要知道当前版本是哪个版本，在Git中，用HEAD表示当前版本，也就是最新的提交fa3926a1b6db3586030a978d4415be006ea01484，上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。
  ```
  $ cat README.md
  ## Git is a distributed version control system.
  ### Git is free software.
  ```
  输入`cat README.md`就可以看到确实回退到了`add distributed`的版本

 * git总有后悔药可以吃

 如果想恢复到回滚之前的版本，也就是回到`append GPL`的版本，只需找到`append GPL`的`commit id`，通过`git log`可以找到`commit id`，甚至只需要之前前面几位`commit id`即可。
 ```
 $ git reset --hard fa3926
 HEAD is now at fa3926a append GPL
 ```
 当然通过`commit id`找回版本的方法受制于需要知道`commit id`，如果不小心关掉了命令行窗口而且没有备份的话这个方法就不灵了，Git提供了一个命令`git reflog`用来记录你的每一次命令
 ```
 $ git reflog
 d29fc4a (HEAD -> master) HEAD@{0}: reset: moving to HEAD^
 fa3926a HEAD@{1}: reset: moving to fa3926
 d29fc4a (HEAD -> master) HEAD@{2}: reset: moving to HEAD^
 fa3926a HEAD@{3}: commit: append GPL
 d29fc4a (HEAD -> master) HEAD@{4}: commit: add distributed
 d8aaf77 HEAD@{5}: commit (initial): wrote a readme file
 ```
 这样我们就又能知道`append GPL`的的`commit id`是`fa3926`了，从而也就实现了版本回滚了。

### 工作区和暂存区
 * 工作区(Working Directory)，就是电脑能看到的目录，比如`learngit`文件夹
   ![liaoxuefeng](https://cdn.liaoxuefeng.com/cdn/files/attachments/0013849082162373cc083b22a2049c4a47408722a61a770000/0)
 * 版本库(Repository)

  ![liaoxuefeng](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384907702917346729e9afbf4127b6dfbae9207af016000/0)

 工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。

 Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。

 `git add`把文件添加进去，实际上就是把文件修改添加到暂存区，`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支，`git master`表示往`master`分支上提交更改。总而言是就是需要提交的文件全部放到暂存区，然后一次性提交暂存区的所有更改。

 ![liaoxuefeng](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384907720458e56751df1c474485b697575073c40ae9000/0)

 ![liaoxuefeng](https://cdn.liaoxuefeng.com/cdn/files/attachments/0013849077337835a877df2d26742b88dd7f56a6ace3ecf000/0)

### 管理修改
  git跟踪管理的是`修改`而不是`文件`，比如新增了一行，删除了一行，更改了某些字符，删了一些又加了一些，甚至创建一个新文件等等，都算一个`修改`。

  当我们修改了README.md文件，通过然后通过`git add`添加到暂存区，这时候先别想分支提交更改，我们再一次修改README.md文件，然后执行`git commit`提交更改，这时候第二次修改的文件并没有被提交，因为git管理的是`修改`，我们只添加了第一次修改后README.md文件到暂存区，而第二次并没有，所以`git commit`只负责提交暂存区的修改，第二次的修改就当然没办法提交上去。解决这个问题的方法就是可以继续`git add`再`git commit`，也可以别着急提交第一次修改，先`git add`第二次修改，再`git commit`，就相当于把两次修改合并后一块提交了。

### 撤销修改
 如果还没`git add`到暂存区，通过`git checkout -- README.md`即可撤销工作区的修改，如果已经添加到暂存区了，需要先执行`git reset HEAD README.md`撤销(unstage)暂存区的修改，重新放回工作区，再执行`git checkout -- README.md`撤销(unstage)工作区的修改，如果已经`git commit`提交到了版本库，就要参考前面`版本回退`一节，前提是还没通过网络推送到远程库。

### 删除文件
 `rm README.md`命令可以删除工作区的文件，这时候版本库里面还存在README.md文件，如果确实像删掉，执行`git rm README.md`即可，如果是误删除，因为版本库里面还存着，通过`git checkout -- README.md`即可将误删的文件恢复到最新版本。

 `git checkout`其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。但是要小心，你只能恢复文件到最新版本，你会丢失最近一次提交后你修改的内容。

### 远程仓库
 * GitHub远程仓库

   Git是分布式版本控制系统，同一个Git仓库，可以分布到不同的机器上。一台电脑上也是可以克隆多个版本库的，只要不在同一个目录下。因为一台电脑上搞几个远程库完全没有意义，而且硬盘挂了会导致所有库都挂掉。找一台电脑充当服务器的角色，每天24小时开机，其他每个人都从这个“服务器”仓库克隆一份到自己的电脑上，并且各自把各自的提交推送到服务器仓库里，也从服务器仓库中拉取别人的提交。完全可以自己搭建一台运行Git的服务器，不过现阶段，为了学Git先搭个服务器绝对是小题大作。好在这个世界上有个叫GitHub的神奇的网站，从名字就可以看出，这个网站就是提供Git仓库托管服务的，所以，只要注册一个GitHub账号，就可以免费获得Git远程仓库。

   第1步：创建SSH Key。
   ```
   $ ssh-keygen -t rsa -C "youremail@example.com"
   ```
   然后一路回车，使用默认值即可。

   如果一切顺利的话，可以在用户主目录里找到.ssh目录，里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人。

   第2步：登陆GitHub，打开“Account settings”，“SSH Keys”页面：

   然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容：

   ![liaoxuefeng](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384908342205cc1234dfe1b541ff88b90b44b30360da000/0)

   点“Add Key”，你就应该看到已经添加的Key：

   ![liaoxuefeng](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384908342205cc1234dfe1b541ff88b90b44b30360da000/0)

   GitHub需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而Git支持SSH协议，所以，GitHub只要知道了你的公钥，就可以确认只有你自己才能推送。GitHub允许你添加多个Key。假定你有若干电脑，你一会儿在公司提交，一会儿在家里提交，只要把每台电脑的Key都添加到GitHub，就可以在每台电脑上往GitHub推送了。

 * 添加远程库
   在GitHub上创建一个新的仓库，步骤略。

   在本地的learngit仓库下运行命令：
   ```
   git remote add origin https://github.com/veeson/learngit.git
   ```
   把本地库的所有内容推送到远程库上：
   ```
   $ git push -u origin master
   ```
   由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。

   从现在起，只要本地作了提交，就可以通过命令：
   ```
   $ git push origin master
   ```
 * 从远程库克隆

   在GitHub上创建一个新的仓库，过程略。

   下一步是用命令git clone克隆一个本地库：
   ```
   git clone git@github.com:veeson/learngit.git
   ```
