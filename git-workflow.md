## Git工作流程和规范

开发团队基于Owner，forker的基本协作模式。通过pull request进行代码提交和代码审查。

[Bitbucket workflow](https://www.atlassian.com/git/workflows)

[fork-compare-pull request](https://confluence.atlassian.com/display/BITBUCKET/Fork+a+Repo,+Compare+Code,+and+Create+a+Pull+Request)

### Branch原则

对于主要功能（feature）或严重问题（critical bug）的修正，建议针对相应的功能和问题建立独立分支进行管理。待功能**全部**完成和问题修复以后在Merge回主分支（一般是master分支）。

**Branch命名规则**

- 功能实现分支：features/[feature-name]
- 问题修复分支：bugs/[bug-id]

### 代码提交原则
代码的提交（commit）的描述需要清晰明确。一个pull request需要提交完整的功能或者问题修复（一般会包括多个commit）。不允许提交不完整的pull request：例如功能没有实现完全，或问题只修正了部分情况。

**代码提交的描述规则**

######功能实现

标题：[FEATURE] 功能的描述

内容：功能实现的说明，可以包括功能设计的要点，实现的方式

示例：

[FEATURE]用户登录和注册

使用devise完成用户的登录和注册。用户注册需要邮件激活。


######问题修复

标题：[BUGFIX-[bug_id]] 问题的描述

内容：

CAUSE: 问题的根本原因

SOLUTION: 问题的解决方法

示例：

[BUGFIX-175] 新注册用户接收到得注册邮件，没有激活链接

CAUSE: 注册邮件的template中的user没有关联上新注册的用户，导致激活链接为nil。

SOLUTION: 传递新注册的用户id到mailer中，在mailer中使用id找到新注册的用户。然后使用用户的激活链接。

######文档更新

标题：[DOCS] 更新内容

内容：文档更新的详细内容，如内容不多可以不写
 
示例：

[DOCS] 更新API文档说明

添加新的API：ADA_ChangeName, ADA_SendData.


######Misc

标题：[MISC] 提交描述

内容：提交的详细描述

示例：

[MISC] 修复typo

### 代码审核原则

任何需要merge进入主分支的代码，都需要进行代码审核。代码审核以pull request为基本单元进行审核，项目中的所有成员都有责任和义务进行代码审核。原则上代码审核需要面对面进行（pull requester and reviewer）。

审核内容：

- 功能是否实现完成，问题是否修复
- 代码的实现是否清晰易懂
- 代码是否会对系统其他部分造成影响
- 代码是否有相应地测试


### Owner的额外职责

Repo的Owner需要完成项目的集成整合。处理pull request。
