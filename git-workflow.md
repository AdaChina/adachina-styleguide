# Git工作流及操作规范

## 前言

阅读之前，你需要对这些有所了解：

* Git（推荐[经典入门教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/)和[猴子都能懂的GIT入门](http://backlogtool.com/git-guide/cn/)，也可以看[官方文档](https://git-scm.com/documentation)，玩玩[Githug](https://github.com/Gazler/githug)实践一下）
* Git workflow (可以在[这里](http://www.ruanyifeng.com/blog/2015/12/git-workflow.html)简单了解，如果想详细了解可以看[bitbucket workflow](https://www.atlassian.com/git/workflows))


## 我们使用的工作流

我们使用的工作流基于[gitlab workflow](https://about.gitlab.com/2014/09/29/gitlab-flow/)，根据我们开发现状调整而来。

### 持续发布的项目
------

在这些项目有两个长期存在的分支：

* master
* release

#### 日常开发

开始开发新功能时，开发者新建一个功能分支`feature-a`，跟这个功能有关的开发都在此分支上进行。当功能开发完成后，由开发者向项目提交[`Pull Request`](https://www.atlassian.com/git/tutorials/making-a-pull-request)(简称`PR`)。代码审阅通过，项目负责人负责把`PR`merge到master分支，并关闭该`PR`，代码审阅不通过，负责人关闭该`PR`，等功能修改完成后，由开发者提交新的`PR`。merge完成后将自动触发CI，运行测试并检测代码质量，若通过则自动部署到staging服务器；若不通过，开发者在功能分支上继续进行修改并提交新的`PR`。部署完成后，在测试服务器上运行正常且无反馈，则由项目负责人把master分支merge到release分支进行生产环境部署，并删除功能分支。

#### Bug修复

发现bug后，开发者新建bug修复分支`fix-a`，修复后提交`PR`，发布流程与日常开发相同，不可跳过master分支直接并入release分支。bug修复后关闭bug修复分支。

### 代码审查标准
-------

项目中的所有成员都可以对代码进行code review，并且可以发commets提出问题或建议。

代码审查的关键点为：

* 功能是否完全实现
* 实现方法是否合适
* 代码是否清晰易懂


## Git操作规范

开发过程中使用Git必须按步骤并遵守以下规范。

### 一、新建分支
------

新功能开发或者修复bug都应该建立新的分支，命名规范为：功能分支以`feature`开头，如`feature-download-page`；修复bug分支以`fix`开头，如`fix-login-exception`。

```bash
# 拉取master分支最新状态, origin是你指定的远程仓库
$ git checkout master
$ git pull origin master

# 新建本地分支
$ git checkout -b feature-download-page

# 在远程仓库新建分支
$ git push origin feature-download-page
```

### 二、在分支提交commit
------

开发过程中提交commit，commit信息规范为：以`[backend]`/`[frontend]`开头以说明修改属于后端/前端，并附上该commit的改动内容

```bash
$ git add .
$ git status
$ git commit -m "[backend] add download controller"
```

### 三、保持与master分支同步
------

开发期间要保持与master分支的同步

```bash
# 如果你使用rebase，将会得到更干净的提交记录，详细看后面rebase的介绍
# on branch feature-download-page
$ git fetch origin master
$ git merge origin/master
```

### 四、功能完成后，发出Pull Request
------

`Pull Request`的格式规范如下：

* 功能分支

	标题：[FEATURE] 功能的描述
	内容：功能实现的说明，可以包括功能设计的要点，实现的方式
	
	例：![feature example](https://raw.githubusercontent.com/AdaChina/adachina-styleguide/master/images/feature_branch.png)

* bug修复分支
	
	标题：[FIX] 修复问题的描述
	内容：问题的根本原因，问题的解决方法
	
	例：![bug example](https://raw.githubusercontent.com/AdaChina/adachina-styleguide/master/images/fix_branch.png)

### 多人协作
------

当多人同时在一个功能分支工作时，提交自己的修改前要注意与远程分支同步，若遇到冲突，应该与其它开发者沟通解决，禁止使用`-f`/`--force`选项

```bash
# 如果你使用rebase，将会得到更干净的提交记录，详细看后面rebase的介绍
# on branch feature-download-page
$ git fetch origin feature-download-page
$ git merge origin/feature-download-page

# 或者
$ git pull origin feature-download-page
```

### 关于rebase
------

尽管[rebase](https://git-scm.com/book/zh/v1/Git-%E5%88%86%E6%94%AF-%E5%88%86%E6%94%AF%E7%9A%84%E8%A1%8D%E5%90%88)很好很强大，但不推荐使用。

#### 好处
rebase可以修改提交记录，同步分支时使用rebase能得到更加干净的提交历史，例子如下：

* merge会把你和远程分支的修改按时间排序并自动添加一次提交
	![merge example](https://raw.githubusercontent.com/AdaChina/adachina-styleguide/master/images/merge.png)

* 而rebase会把你的提交放到远程分支之后
	![rebase example](https://raw.githubusercontent.com/AdaChina/adachina-styleguide/master/images/rebase.png)

#### 风险
千万不要对你已发布到远程仓库的提交记录做rebase操作(即修改已提交的记录)，你只可以对自己未提交commit做rebase。[否则，人民群众会仇恨你，你的朋友和家人也会嘲笑你，唾弃你。](https://git-scm.com/book/zh/v1/Git-%E5%88%86%E6%94%AF-%E5%88%86%E6%94%AF%E7%9A%84%E8%A1%8D%E5%90%88#衍合的风险)总之，rebase有可能发生难以预料的修改结果。


