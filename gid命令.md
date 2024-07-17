**git基本配置**
1. 安装Git 后, 首先要设置你的用户名称和e-mail 地址, 因为每次Git 提交都会使用该信息
```
git config --global user.name ""
gir config --global user.email""
```

**git工作原理**
四个工作区域
Git 本地有三个工作区域：工作目录（Working Directory）、暂存区(Stage/Index)、资源库
(Repository 或Git Directory)。如果再加上远程的git 仓库(Remote Directory)就可以分为四个
工作区域。文件在这四个区域之间的转换关系如下
1. Workspace：工作区，就是你平时存放项目代码的地方
2. Index / Stage：暂存区，用于临时存放你的改动，事实上它只是一个文件，保存即将提交
到文件列表信息
3. Repository：仓库区（或本地仓库），就是安全存放数据的位置，这里面有你提交到所有
版本的数据。其中HEAD 指向最新放入仓库的版本
4. Remote：远程仓库，托管代码的服务器(比如Github/Gitee)，可以简单的认为是你项目组
中的一台电脑用于远程数据交换
5. Directory：使用Git 管理的一个目录，也就是一个仓库，包含我们的工作空间和Git 的管
理空间。
6. WorkSpace：需要通过Git 进行版本控制的目录和文件，这些目录和文件组成了工作空间。
7. .git：存放Git 管理信息的目录，初始化仓库的时候自动创建

**Git创建**
创建全新的仓库
说明: 创建全新的仓库，需要用GIT 管理的项目的根目录执行
3. 执行git init
4. 4. 执行后可以看到，仅仅在项目目录多出了一个.git 目录，关于版本等的所有信息都在
这个目录里面

**远程克隆仓库**
 克隆一个代码仓库和它的整个代码历史(版本信息)
$ git clone [url] # url 就是远程git 项目的地址

1. 创建目录d:\hspgit2 作为本地git 仓库
2. 在github 或者gitee 找一个项目的地址url, 比如
3. 执行克隆指令
git clone https://gitee.com/6tail/lunar-javascript.git


**Git文件管理**
文件四种状态
● 版本控制就是对文件的版本控制，在Git 管理中，文件被统一管理，有四个状态
1. Untracked: 未跟踪, 此文件在文件夹中, 但并没有加入到git 库, 不参与版本控制. 通过
git add 状态变为Staged
2. Unmodify: 文件已经入库, 未修改, 即版本库中的文件快照内容与文件夹中完全一致. 这
种类型的文件有两种处理方式, 如果它被修改, 变为Modified. 如果使用git rm 移出版本库,
则成为Untracked
3. Modified: 文件已修改, 仅仅是修改, 并没有进行其他的操作. 这种文件有两个去处, 通过git add 可进入暂存staged 状态, 使用git checkout 则丢弃修改过, 返回到unmodify 状态,
这个git checkout 即从库中取出文件, 覆盖当前修改
4. Staged: 暂存状态. 执行git commit 则将修改同步到库中, 这时库中的文件和本地文件又
变为一致, 文件为Unmodify 状态. 执行git reset HEAD filename 取消暂存, 文件状态为
Modified

**###忽略文件###**

忽略文件处理方式
● 不想把某些文件纳入版本控制中，比如数据库文件，临时文件，设计文件等, 在主目录
下建立".gitignore"文件(默认就有)，此文件有如下规则：
1. 忽略文件中的空行或以井号（#）开始的行
2. 支持Linux 通配符。例如：星号（*）代表任意多个字符，问号（？）代表一个字符，
方括号（[abc]）代表可选字符范围，大括号（{string1,string2,...}）代表可选的字符串等。
3. 如果名称的最前面有一个感叹号（!），表示例外规则，将不被忽略。
4. 如果名称的最前面是一个路径分隔符（/），表示忽略.gitignore 文件所在的目录，不包
括其任何子目录中的dir 目录
5. 如果名称的最后面是一个路径分隔符（/），忽略.gitignore 文件所在的目录和所有子目
录的dir 目录


**使用IDEA管理git操作**
1. 将Gitee 初始化hspjava，拉取到IDEA
2. 创建一个crm 模块，进行测试
3. 创建HI.java, 进行测试
4. 将HI.java add 到git, 说明：将HI.java 加入到暂存区
5. 将HI.java commit 到git commit 只是将HI.java 提交到本地仓库 并没有push 到远程仓库(即GitHub/Gitee) 
6. 将HI.java push 到Gitee , 会输入用户名密码验证(是Gitee 的账号).
7. 观察Gitee 上项目的变化

**GIT 分支管理**
1. 分支可以有多个(根据业务需求)
2. 如果各分支没有交集，始终平行发展，则不需要合并(merge)
3. 如果两个分支，需要合并，则执行merge 操作.

创建IDEA Maven 项目, 和Gitee 的hsp-erp 代码仓库关联
1. 先在Gitee 创建仓库hsp-erp, 并设置成开源
2. 在新的目录比如d:/idea_projects 使用idea 创建hsp-erp maven, 并和Gitee 仓库hsp-erp
关联, 前面老师已经讲过了, 自己回顾一下
3. 创建文件HspErpApplication.java , 写入一些内容
4. 将HspErpApplication.java push 到Gitee 远程仓库, 执行add->commit->push
5. 观察Gitee 远程仓库是否已经push 成功