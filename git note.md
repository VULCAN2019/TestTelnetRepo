# Git Note

## 版本控制工具应该具备的功能

#### 协同修改

+ 多人并行不悖的修改服务器端的同一个文件

#### 数据备份

- 不仅是保~~~~存当前状态，还能够保存每一个提交过的历史状态

#### 版本管理

1. SVN采用的是增量式管理的方式
- Git采取了文件系统快照的方式

#### 权限控制

- 对团队开发人员进行权限控制

- 对团队外开发者贡献的代码进行审核——Git独有

#### 历史纪录

- 查看修改人/修改时间/修改内容/日志信息

- 将本地文件恢复到某一个历史状态

#### 分支管理

- 允许开发团队在工作过程红多条生产线同时推进任务，进一步提高效率

---

## linux系统版本控制历史

![5cc01c903e707](https://i.loli.net/2019/04/24/5cc01c903e707.png)

---

## Git结构

![5cc01c47eec6f](https://i.loli.net/2019/04/24/5cc01c47eec6f.png)

---

## Git和代码托管中心

#### 代码托管中心的任务：维护远程库

#### 局域网环境下

+ GitLab服务器

#### 外网环境下

+ GitHub

+ 码云

---

## 本地库和远程库

- 团队内部协作

![5cc02263099dd](https://i.loli.net/2019/04/24/5cc02263099dd.png)

- 跨团队协作

![5cc0227fdec0c](https://i.loli.net/2019/04/24/5cc0227fdec0c.png)

---

## 本地库初始化

- 命令：git init

***注意：.git目录中存放的是本地库相关的子目录和文件，不要删除，也不要随便修改***

#### 设置签名

- 形式
  
  - 用户名：june
  
  - email地址： hello@gg.com

- 作用：区分不同开发人员的身份

- 辨析：这里设置的签名和登陆远程库的账号、密码没有任何关系

- 命令
  
  - 项目级别/仓库级别：仅在当前本地库范围内有效
    
    - git config user.name june_pro
    
    - git config uger.email hello@gg.com
    
    - ***信息保存位置：./.git/config文件***
  
  - 系统用户级别：登陆当前操作系统的用户范围
    
    - git config --global user.name june_pro
    
    - git config --global uger.email hello@gg.com
    
    - ***信息保存位置:~/.gitconfig***
  
  - 级别优先级
    
    + 就近原则：项目级别优先于系统用户级别，二者都有时采用项目级别的签名
    
    + 如果只有系统用户级别的签名，就以系统用户级别的签名为准
    
    + 不允许二者都没有

---

## 添加提交以及查看状态操作

> git status        **查看当前状态**
> 
> > On branch master        **目前在主干分支**
> > 
> > No commits yet            **本地库没有提交过任何东西**
> > 
> > nothing to commit        **暂存区没有任何内容可提交**

git add <file>                        **提交变化到暂存区**

git rm --cached <file>         **删除暂存区的内容【也就是将要提交的内容】**

git commit <file>                **上传到本地库** ***（随后会进入vim编辑器）***

git commit -m "my first.new file" <file>        **上传到本地库**  ***（不会进入vim）***

---

## 版本的前进和后退

> #### 查看历史纪录
> 
> > git log
> > 
> > ![5cc54f8885b4d](https://i.loli.net/2019/04/28/5cc54f8885b4d.png)
> > 
> > ***HEAD是指针的名字***
> > 
> > - ***多屏显示控制方式***
> >   
> >   - 空格向下翻页
> >   
> >   - b向上翻页
> >   
> >   - q退出
> > 
> > git log --pretty=oneline **以一行的格式显示出信息**
> > 
> > ![5cc54fec634dc](https://i.loli.net/2019/04/28/5cc54fec634dc.png)
> > 
> > git log --oneline
> > 
> > ![5cc5501b0ae8b](https://i.loli.net/2019/04/28/5cc5501b0ae8b.png)
> > 
> > git reflog            **显示了到某一个版本需要移动几步（HEAD@{步数}）**
> > 
> > ![5cc55053dc2bc](https://i.loli.net/2019/04/28/5cc55053dc2bc.png)
> 
> #### 基于索引值前进后退
> 
> > git reset --hard 索引值        ***`推荐使用`***
> > 
> > git reset --hard d1740cd
> 
> ###### `使用^符号`    **只能后退**
> 
> > git reset --hard HEAD^    **一个^表示后退一步**
> 
> ###### 使用~符号    **只能后退**
> 
> > git reset --hard HEAD~n    **表示后退n步**

## reset命令的三个参数对比

- soft参数
  
  - 仅仅在本地库移动HEAD指针
  
  - ![5cc55bb6dcd88](https://i.loli.net/2019/04/28/5cc55bb6dcd88.png)

- mixed参数
  
  - 在本地库移动HEAD指针
  
  - 重置暂存区
  
  - ![5cc55be4854b6](https://i.loli.net/2019/04/28/5cc55be4854b6.png)

- hard参数
  
  - 在本地库移动HEAD指针
  
  - 重置暂存区                                 ***暂存区   index***
  
  - 重置工作区                                ***工作区   working tree***
  
  - ![5cc55c40019d7](https://i.loli.net/2019/04/28/5cc55c40019d7.png)
    
    # 

## 永久删除文件后找回

> <a style="color:yellow">与创建文件、添加暂存区、提交本地库一样</a>

> > vim delete.txt **创建文件并编辑**
> > 
> > git add delete.txt **添加到暂存区**
> > 
> > git commit -m 'new delete.txt' delete.txt **提交到本地库**
> > 
> > rm delete.txt **删除文件**
> > 
> > git add delete.txt **添加到暂存区**
> > 
> > git commit -m 'delete dekete.txt' delete.txt **提交到本地库**
> > 
> > git reset --hard **移动到文件存在时的版本**

> <a>**一种情况    要删除的文件添加到暂存区后不想删除了**</a>
> 
> > rm delete.txt        **删除文件**
> > 
> > git add delete.txt        **添加到暂存区**
> > 
> > git reset --hard HEAD        **移动指针到当前指针处**
> 
> <a style="color:green;font-weight:bold;">原理：当前版本状态并不知道当前暂存区的操作状态   不知道为什么请回去看看*《Git结构》*</a>

## 比较文件差异

* git fiff <file>        **将工作区中的文件和暂存区进行比较**

* git diff <本地库中历史版本> <file>   **将工作区中的文件和本地库历史纪录比较**

* git diff <本地库中历史版本>          **比较当前工作区的所有文件*

## 分支概述

#### <a style="color:yellow">在版本控制过程中，使用多条线同时推进多个任务</a>

![5cc7d1da10b88](https://i.loli.net/2019/04/30/5cc7d1da10b88.png)

> <a style="color:yellow">分支的好处</a>
> 
> > 同时并行推进多个功能开发，提高开发效率
> > 
> > 各个分支在开发过程中，如果某一个分支开发失败，不会对其他分支有任何影响。失败的分支删除重新开始即可。

## 分支操作

* 创建分支        git branch <分支名>

* 查看分支        git branch -v

* 切换分支        git checkout <分支名>

* 合并分支        git checkout [被合并分支名 master]
  
                         git merge [有新内容分支名]

## 解决合并冲突

<a style="color:yellow">冲突的表现</a>

![5cc7e307b0a65](https://i.loli.net/2019/04/30/5cc7e307b0a65.png)

![5cc7e3285ea0c](https://i.loli.net/2019/04/30/5cc7e3285ea0c.png)

<a style="color:yellow">冲突的解决</a>

1. 编辑文件，删除特殊符号

2. 把文件修改到满意的程度

3. git add [文件名]

4. git commit -m '日志信息'
   
   **`注意   此时commit一定不能带具体文件名`**

## Git 基本原理

#### 1. 哈希

哈希时一个系列的加密算法`平时所说的哈希算法不止一个算法`，各个不同的哈希算法虽然加密成都不同，但是有以下几个共同点：

1. 不管输入数据的数据量有多大，输入同一个哈希算法，得到的加密结果长度固定

2. 哈希算法确定，输入数据确定，输出数据能够保持不变

3. 哈希算法确定，输入数据有变化，输入数据一定有变化，而且变化很大

4. 哈希算法不可逆
   
   <a style="color:red">Git底层采用的时SHA-1算法</a>

![5cc7e919cb506](https://i.loli.net/2019/04/30/5cc7e919cb506.png)

<a style="color:red">    Git就是靠这种机制来从根本上保证数据完整性的</a>

#### 2.保存版本的机制

1. <a style="color:yellow">集中式版本控制工具的文件管理机制</a>
   
   * 以文件变更列表的方式存储信息。这类系统将它们保存的信息卡座是彝族基本文件和每个文件随时间逐步累积的差异。
     
     ![5cc7ed1f5c82c](https://i.loli.net/2019/04/30/5cc7ed1f5c82c.png)
     
     <a style="color:yellow">Git的文件管理机制</a>
   
   * Git把数据看作是小型文件系统的一组快照。每次提交更新时Git都会对当前的全部文件制作一个快照并保存这个快照的索引。为了搞笑，如果文件没有修改，Git不再重新储存该文件，而是只保存一个链接指向之前存储的文件。所以Git的工作方式可以称之为快照流。
     
     ![5cc7ee47e0265](https://i.loli.net/2019/04/30/5cc7ee47e0265.png)

2. <a style="color:yellow">Git文件管理机制细节</a>
   
   * Git的“提交的对象”
     
     ![5cc7ee8804462](https://i.loli.net/2019/04/30/5cc7ee8804462.png)
   
   * 提交对象及其父对象形成的链条
     
     ![5cc7efc1acfd3](https://i.loli.net/2019/04/30/5cc7efc1acfd3.png)

3. <a style="color:yellow">Git分支管理机制</a>
   
   1. 分支的创建
   
   ![5cc7f3c94d98b](https://i.loli.net/2019/04/30/5cc7f3c94d98b.png)
   
   2. 分支的切换
   
   ![5cc7f4296f0a3](https://i.loli.net/2019/04/30/5cc7f4296f0a3.png)
   
   ![5cc7f45a750c8](https://i.loli.net/2019/04/30/5cc7f45a750c8.png)
   
   ![5cc7f47564a92](https://i.loli.net/2019/04/30/5cc7f47564a92.png)
   
   ![5cc7f49617a53](https://i.loli.net/2019/04/30/5cc7f49617a53.png)

## 在本地创建远程库地址别名

git remote add origin https://....../name.git    把后面一长串链接改个别名为 <a style="color:yellow;">origin</a>

## 推送操作

git push origin master    <a style="color:yellow;">origin </a>为之前的别名

## 克隆操作

git clone https://....../name.git    将远程库克隆到本地

#### 效果

* 完整的把远程库下载到本地

* 创建origin远程地址别名

* 初始化本地库

## 远程库拉取

#### 拉取

> pull = fetch + merge   `抓取合并`
> 
> git pull [远程地址别名] [远程分支名]      <a style="color:yellow;"> 这一句等于下面两句</a>
> 
> > git fetch [远程库地址别名`origin`] [远程分支名`master`]
> > 
> > git checkout [远程地址别名/远程分支名]    切换过去看一哈 保证安全
> > 
> > git checkout [远程分支名`master`]
> > 
> > git merge [远程地址别名/远程分支名]
