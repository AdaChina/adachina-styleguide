## Git工作流程和规范

### Table of Content

- branch 原则和命名规则
- 代码提交原则
- 代码审核原则
- Owner的额外职责

****

#### Branch 原则
以下两种情况需要建立分 (Branch) 支来处理：

- 搭建主要（新）功能（Feature）
- 严重问题（Critical Bug）的修正，

待功能**全部**完成和问题修复以后再 Merge 回主分支（一般是 Master 分支）。

**Branch 命名规则**

- 功能实现分支：[feature-name]。如 user-management
- 问题修复分支：bug-fix-[bug-name]。如 bug-fix-cannot-login

****

#### 代码提交原则
代码的提交（Commit）的描述需要清晰明确，提交原则需要符合以下：

- 一个 Commit 需要提交完整的功能(算法)或者问题修复，别人拉取 (pull) 代码后要可用。
- 不能携带不必要的注释和 debug 代码，如 Rails 开发下的 binding.pry。
- 每次提交需携带清晰的提交描述

**代码提交的描述规则**

######区分前后端提交

前后端代码的提交需要标注，每次提交需要携带 prefix。如：

- 前端提交：[frontend] 完成用户注册页面的样式和集成后台数据。
- 后端提交：[backend] 完成用户的登录登出后台接口。

针对不同类型的提交，可参考如下描述方式：

######功能实现

- 内容：功能实现的说明，可以包括功能设计的要点，实现的方式
- 示例：[backend][feature]用户登录和注册。使用devise完成用户的登录和注册。用户注册需要邮件激活。

######问题修复

- 内容：CAUSE: 问题的根本原因。SOLUTION: 问题的解决方法
- 示例：[backend][bugfix-175] 新注册用户接收到得注册邮件，没有激活链接。CAUSE: 注册邮件的template中的user没有关联上新注册的用户，导致激活链接为nil。SOLUTION: 传递新注册的用户id到mailer中，在mailer中使用id找到新注册的用户。然后使用用户的激活链接。

######文档更新

- 内容：文档更新的详细内容，如内容不多可以不写
- 示例：[docs] 更新API文档说明。添加新的API：ADA_ChangeName, ADA_SendData.


######Misc（其他）

- 内容：提交的详细描述
- 示例：[misc] 修复typo

****
### 代码审核原则

任何需要 merge 进入主分支的代码，都需要进行代码审核。代码审核以功能为基本单元进行审核，项目中的所有成员都有责任和义务进行代码审核。原则上代码审核需要面对面进行（主开发和项目成员）。

审核内容：

- 功能是否实现完成（对照需求书）
- 问题是否修复
- 代码的实现是否清晰易懂
- 代码是否会对系统其他部分造成影响
- 代码是否有相应地测试

****

### Owner 的额外职责

Repo 的 Owner 需要完成项目的集成整合，完成部署和上线管理。

****

### 参考链接

- [Bitbucket workflow](https://www.atlassian.com/git/workflows) 介绍 git 的开发流程，英文 [必读]
- [fork-compare-pull request](https://confluence.atlassian.com/display/BITBUCKET/Fork+a+Repo,+Compare+Code,+and+Create+a+Pull+Request) 
