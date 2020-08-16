# Git

- svn：增量

- git：全量
  
  - 每一个版本都包含全部的文件，时刻保持数据的完整性
  
- git 三种状态【已管理】
  - 已修改：modified     【工作区：增删改查】

  - 已暂存：staged         【暂存区：add】

  - 已提交：commited   【本地仓：commit】

  - checkout

    - 放弃工作区的修改，相对于暂存区或本地仓

    - 版本穿梭（游离状态）：git checkout sha1值

      - 修改后，必须提交

        - git add .

          git commit -m ''

      - 创建分支的好时机

        - git branch new-branch-name sha1值

  - reset

    - 将之前增加到暂存区中的内容，回退到工作区

- 将目录纳入管理（初始化）：git init 

- 查看日志
  - git log
    - git log -2 # 查看最近两次历史记录
  
- 设置用户名和邮箱
  - git config --global   （基本不用，给整个计算机一次性设置）
  - git config --system （推荐，给当前用户一次性设置）
  - git config --local      （默认，给当前项目一次性设置）
    - git config --local --unset user.name username
    - git config --local --unset user.email email

- git rm 恢复
  ```sh
  git rm test.txt            # 删除放到暂存区
  git reset HEAD test.txt    # 将暂存区恢复工作区
  git checkout -- test.txt   # 撤销删除动作
  ```

- 操作系统rm

  ```sh
  rm test.txt                # 删除回到工作区
  git restore test.txt       # 撤销删除动作
  ```

- 注释重写

  ```
  git commit --amend -m '修正上一次的注释'
  ```

- 忽略文件/目录（编辑.gitignore文件）

  ```sh
  *              # 任意字符
  !              # 非
  dir/:          # 忽略dir目录中的所有文件
  dir/*.txt      # 忽略dir目录下的所有txt文件
  dir/*/*.txt
  空目录          # 默认忽略      
  ```

- 分支

  > 分支是一个commit链，一条工作记录线
  >
  > 快照version：每次提交后的所有文件
>
  > 分支名（master）：指向当前的提交（commit）
  >
  > HEAD：指向当前分支（HEAD->分支名）

  ```sh
  git branch -v       # 查看分支
  git branch name     # 创建分支
  git checkout name   # 切换分支
  git branch -d name  # 删除分支，不能删除当前分支；存在未合并的分支也不能删除
  git branch -D name  # 强制删除，即使存在未合并分支
  git branch -b name  # 创建新分支并切换
  git branch -m master master1 # 重命名分支
  ```
  - 细节：
    - 如果在分支A中进行了写操作，但此操作局限在工作区中进行（没有add commit），在master中能够看到该操作
    - 如果分支A中进行了写操作，进行了commit（本地库），则master中无法观察到此文件
    - 如果在分支A中进行了写操作，但此操作局限在工作区中进行（没有add commit），可以成功删除分支A
  
- 合并（merge）

  - 如果一个分支靠前（dev），另一个分支落后（master）。如果不冲突，master可以通过merge直接赶上dev，此称为fast forwar。
  - fast forward 本质是分支指针的移动
    - 注意：跳过的中间commit，仍然会保存。
  - fast formard：
    1. 两个分支fas forward 归于一点commit
    2. 没有分支信息（丢失分支信息）
  - git 在merge时，默认使用fast forward，可以通过`git merge --no-ff`禁用
    1. 两个分支fas forward 不归于一点commit，主动发起合并的分支会前进一步
    2. 分支信息完成（不丢失分支信息）

- 冲突

  - 合并果如冲突，需要解决冲突
  - 解决冲突后
    - git add xxx         告知冲突已解决
    - git commit -m 
  - 查看日志
    - git log --graph --pretty=oneline --abbrev-commit

- 版本穿梭

  - 在多个commit之间进行穿梭
    - 回退
      - gti reset --hard HEAD^^
      - git reset --hard HEAD~2
      - git reset --hard sha1值
    - 前进
      - git reflog  查看所有记录
      - git reset --hard sha1值
  - 合并add并commit
    - git commit -am '注释'

- stash: 保存现场

  1. 建议（规范）：在功能没有开发完前，不要commit

  2. 规定（必须）：在没有commit之前，不能checkout切换分支（不在同一个commit阶段）

  3. 如果还没有将某一个功能开发完毕，就要切换分支，建议

     保存现场（临时保存，stash）

     - git stash   # 保存现场
       - git stash save name
     - git stash list # 查看临时保存列表
     - git stash pop # 还原现场(默认还原最近一次)，并删除原保存内容
     - git stash apply # 还原内容，不删除原保存的内容
       - git stash apply stash@{n} # 还原任意现场
     - git stash drop stash@{n} # 删除任意现场内容

  4.  如果不同分支在同一个commit阶段，在commit之前，可以checkout切换分支

  5. stash保存现场后，工作区没有stash的现场内容

     - stash还原与commit后的内容冲突处理

- Tag标签

  > 适用于整个项目，和具体的分支没有关系

  ```sh
  git tag xxx                  # 添加标签xxx
  git tag -a xxx -m '注释'     # 添加标签名字和注释
  git tag -d v1.0              # 删除标签
  git tag                      # 查看标签
  git tag -l 'v2.0'            # 查询标签
  git tag -d 'v2.0'            # 删除标签
  ```

- blame责任

  ```sh
  git blame a.txt  # 查看a.txt的所有提交commit sha1值，以及每一行的作者
  ```

- diff比较

  ```sh
  # diff比较的是文件本身
  @@ -4,3 +4,6 @@
  4：从第4行开始
  3：比较3行
  6：比较6行
  -：原文件
  +：对比的文件
  
  # git diff 比较的是区中的文件
  # 暂存区和工作区的差异
  touch a.txt
  echo 'aa' >> a.txt
  git init
  git add .
  git commit -m "init"
  
  git diff commit的sha1值 # 本地库与工作区的差异
  git diff HEAD           # 最新本地库与工作区的差异
  git diff --cached commit的sha1值 # 本地库与暂存区的差异
  git diff --cached HEAD # 最新本地库与暂存区的差异
  ```

  

# Github

![image-20200815205844857](C:\Users\wyp\AppData\Roaming\Typora\typora-user-images\image-20200815205844857.png)

- 操作：
  - pull: GitHub -> 本地（拉取[fetch]+合并[merge]）
  - push: 本地 -> GitHub
- 创建远程GitHub
  - 登录GitHub.com
  - HTTPS: git remote add origin https://github.com/githubaccount/project.git
    - 将remote端的https://github.com/githubaccount/project.git用origin 表示
  - SSH: git remote add origin git@github.com:githubaccount/project.git
    - ssh配置
      - 本地：私钥
      - 远程github：存放公钥
      - ssh-keygen
        - 生成私钥（本机），公钥（Github）
      - 公钥的存放地方
        1. 项目的setting中，只有当前项目可以和本机免密登录
        2. 账号的setting中，账户的所有项目都可以和本机免密登录
  - git push -u origin master
    - 将本地的master分支推送到远程origin中
    - 第一次推送需要指定origin和master，以后可以直接使用git push命令
  - git remote show
    - 查看远程库列表
    - git remote show origin 查看origin远程库
  - git clone  git@github.com:githubaccount/project.git
    - 项目名为project
  - git clone  git@github.com:githubaccount/project.git project2
    - 修改项目名为project2

- 开发
  - dev：开发分支，频繁改变
  - test：基本开发完毕后，交给测试实施人员的分支
  - master：生产阶段，最后合并，很少变化
  - bugfix：临时修复bug分支
- 查看所有分支（本地+远程）
  - git branch -a 
  - git branch -av
- 有冲突
  - pull = fetch + merge
  - merge
    - vi 解决冲突-> git add. -> commit
  - pull -> vi -> add -> commit -> push

# 图形工具

git gui、gitk、github desktop



- 查看Github分支的日志
  - git log refs/remotes/origin/master
  - 分支就是一个指针，为commit的sha1值

- 分支
  - 本地分支
    - 本地->远程
      - git push
        - 方法1
          - git push -u origin dev
        - 方法2
          - git push --set-upstream origin dev
    
  - 远程分支
    - 远程->本地
      - pull : 远程->追踪
      - 追踪-本地
        - 方法1：
          - git checkout -b dev origin/dev
        - 方法2：
          - git checkout -b dev --track origin/dev
          - git checkout --track origin/dev  (简写)
    
  - 本地的远程分支
    
    - 追踪分支（origin/分支），默认：origin/master
    
  - 删除远端分支

    - 删除本地分支

      - git branch -b 分支名

    - 删除远端分支

      ```sh
      # git push origin src:dst
      git push origin :test  # src为空，可删除远端test分支
      
      # 等价于如下命令：
      git push origin --delete test
      ```

    - 推送本地分支到远程分支的其他方法

      ```sh
      # 将本地的dev推送到远端dev2分支，并自动关联感知
      git push origin dev:dev2
      
      # HEAD表示当前分支
      git push origin HEAD:dev2
      
      # 等价于如下命令：
      git push
      git checkout -b dev --track origin/dev2
      ```

    - 本地没有test分支，但本地却感知远端test分支，清理无效分支

      > 无效分支：追踪分支（本地中感知的远程分支，如origin/dev）

      ```sh
      # 检测无效分支
      git remote prune origin --dry-run
      
      # 删除无效分支
      git remote prune origin
      ```

    - 拉取远程分支到本地新分支的其他方法

      ```sh
      # 将远端的master分支拉取到origin/test，并自动关联感知
      git fetch origin master:refs/remotes/origin/test
      ```

- 给命令去别名

  ```sh
  # 设置checkout 的别名为ch
  git config --global alias.ch checkout
  
  # 可直接修改配置文件
  cat ~/.gitconfig
  [core]
          editor = \"D:\\Program Files\\EditPlus\\EditPlus.exe\" -w
  [user]
          email = xxxxxx@qq.com
          name = Mr.Dragon
  [filter "lfs"]
          clean = git-lfs clean -- %f
          smudge = git-lfs smudge -- %f
          process = git-lfs filter-process
          required = true
  [gui]
          recentrepo = C:/Users/wyp/Desktop/project2
          recentrepo = C:/Users/wyp/Desktop/project1
  [alias]
          ch = checkout
  ```

  

- 远端标签

  - 标签：git tag

    ```sh
    # 简单标签，只存储当前的commit的sha1值
    git tag v1.0
    
    # （创建一个新对象，产生一个新commit/sha1）注释标签，存储包含了当前的commit的sha1值
    git tag -a v2.0 -m 'test_release2.0'
    
    # 查看标签
    git tag
    
    # 查看标签详细信息
    git show v1.0
    ```

  - 本地标签推送到远端

    ```sh
    git push origin v1.0            # 推送v1.0标签
    git push origin v1.0 v2.0 v3.0  # 批量推送标签v1.0 v2.0 v2.0
    git push origin --tags          # 推送所有标签
    
    # 完整命令
    git push origin refs/tags/v1.0:ref/tags/v1.0
    ```

  - 获取远程标签

    ```sh
    # 如果远端新增标签，则pull可以将新增的标签拉取到本地
    # 如果远程是删除标签，则pull无法感知，需要在本地手动删除
    git pull 
    
    # 只获取标签v4.0
    git fetch origin tag v4.0    
    ```

  - 删除远程标签

    ```sh
    # 删除远端标签v1.0
    git push origin :v1.0
    
    # 完整命令
    git push origin :refs/tags/v2.0
    
    # 如果将远程标签删除，其他用户无法直接感知
    ```

- 压缩：git gc

  > object 、refs中记录了很多commit的sha1值，如果执行gc则会将这么多sha1值存放到一个压缩文件中packagd-refs
  >
  > refs：标签、head、remote
  >
  > objects：对象，git每一次version的全量内容

- Git裸库

  > 没有工作区的工作仓库，存在于服务端

  ```sh
  # 初始化裸库
  git init --bare
  
  # 只能push推到远程服务器
  git push 。。。
  # 由于没有工作区，所以不能进行git add 操作
  ```

- 子模块：submodule

  > 应用场景：
  >
  > 在一个仓库中引用另一个仓库的代码。
  >
  > A库中有B库，B是A的子模块，但B修改 push之后 A无法直接感知，需要主动操作，更新submodule
  ```sh
  # 在github.com创建A、B两个项目，并配置ssh连接
  git@github.com:account/A.git
  git@github.com:account/B.git
  
  # 第一次必须关联远端github地址，之后才能直接通过git push推送
  git remote add origin git@github.com:account/A.git
  git push -u origin master
  
  git remote add origin git@github.com:account/B.git
  git push -u origin master
  
  # 修改B库本地内容-> add-> commit-> push：A （本地+远程）无法感知
  # 进入A/B中
  git pull
  # 或者遍历A项目中的所有子模块：
  git submodule foreach git pull # A本地已有感知，但远程还没有
  # 在A库本地->add ->commit ->push
  git add .
  git commit -m 'xxxx'
  git push
  
  # 如果克隆的项目包含submodule，需要加参数--recursive递归遍历子模块
  git clone git@github.com:account/A.git --recursive
  
  # 删除子模块submodule B
  git rm --cached B   # 删除B暂存区
  rm -rf B            # 删除B目录(工作区)
  rm -rf .gitmodules
  git add .
  git commit -m '删除子模块B'
  git push
  
  ```

- substree

  > 建议：
  >
  > - submodule：单向操作，复杂
  > - substree：双向操作，简单

  ```sh
  # GitHub中创建项目parent、substree
  
  # 创建本地项目parent、substree
  mkdir parent substree
  cd parent
  git init
  cd ../substree
  git init
  
  # ssh
  # 先指定仓库地址
  cd parent/
  git remote add origin git@github.com:Dragon631/parent.git
  
  cd ../substree/
  git remote add origin git@github.com:Dragon631/substree.git
  
  # 再指定分支
  git add .
  git commit -m 'xxx'
  git push -u origin master
  
  # 在parent父项目中执行
  # 在parent中加入substree，别名为substree-origin
  git remote add substree-origin git@github.com:Dragon631/substree.git
  git substree add -P substree substree-origin master
  # 等价：git substree add --prefix substree substree-origin master
  
  # --squash，合并commit，为了防止子工程干扰父工程
  # 如果子工程进行了a,b,c,d,e操作，而a，c增加,b，d删除，互相抵消，则最终将所有操作合并为1次f操作的commit，最后在执行1次新的commit提交
  # 即加了squash之后：会产生新的提交（容易导致冲突）；并往前走两步commit
  # 建议：
  # 在做substree时，如果加squash，以后每次都加；如果不加，则都不要加
  # 即git substree 命令，要么都加squash，要么都不加
  git substree add -P substree substree-origin master
  
  # 演示修改substree内容，看parent如何感知substree的变化
  # 在substree修改内容：add -> commit -> push
  # 在parent中将substree pull（到本地）：
  git substree pull -P substree substree-origin master
  
  # 将本地的推送到服务端
  git push
  ```

  - 冲突

    > 修改同一文件的同一行、不是同一祖先、不规范
    >
    > - 如果是同一个祖先，则可能不会冲突
    > - 如果不是同一个祖先，则很可能会冲突
    > - 在substree、submodule容易冲突（有两个根）
    >   - 解决：vi -> add -> commint -> push

  - 核心流程：

    - substree -> parent 

      1. 修改substree子工程，并push到服务端

         ```sh
         # modify
         git add .
         git push
         ```

      2. （本地）将GitHub中的子工程更新到父工程的子模块中

         ```sh
         git substree pull -P substree substree-origin master
         ```

      3. parent父工程中子模块的更新情况推送到对应的GitHub上（父->子）

         ```sh
         git push
         ```

    - parent中的子模块 -> substree 

      1. 修改父工程的substree子模块，并push到parent服务端

         ```sh
         # modify
         git add .
         # 推送parent本身
         git push 
         ```

      2. 将父中子模块推送值substree工程

         ```sh
         git substree push -P substree substree-origin master
         ```

    - 操作真实的子模块

      ```sh
      git substree pull/push -P
      ```

- cherry-pick
  > 如果写了一半（已经提交），发现写错分支，需要将已提交的commit转移到别的分支
  >
  > 每次只能转移（复制）一个commit，内容会被复制，但是sha1值会改变（sha1值是唯一的）
  >
  > 思路：
  >
  > - cherry-pick 复制到应该编写的分支上
  >   - git cherry-pick commit/sha1
  > - 把写错的分支删除
  >   - git checkout 旧节点
  >   - 删除分支
  > - 新建分支
  >
  > 注意：
  >
  > - cherry-pick在复制的时候，不要跨commit节点复制，会报冲突

- 变基/衍合：rebase

  - 编写代码的地方

  - 在要改变的分支中操作

    - git rebase [要转移到的分支名]
    - 如： git rebase master

  - 改变分支的根基

  - 会改变提交历史

  - rebase之后提交的线路，是一条直线

  - rebase也会冲突

    - 解决冲突：vi -> add -> git rebase --continue

      ```sh
      # branch dev
      # modify dev
      git add .
      git commit -m 'xxx'
      
      # 变基
      git rebase master
      
      # 解决冲突
      # vi -> add
      git rebase --continue
      # 最后dev指向最新的commit
      # 使master也指向最新的commit
      git checkout master
      git merge dev
      
      ```

      

    - 忽略冲突：git rebase --skip

      - 放弃rebase分支的修改，直接使用其他分支

    - 终止rebase：git rebase --abort

      - 还原rebase之前的场景

  - 建议

    - rebase分支只在本地操作，不要推送到Github
    - 不要再master上直接执行