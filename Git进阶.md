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