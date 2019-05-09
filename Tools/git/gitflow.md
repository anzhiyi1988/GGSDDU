# 基于gitflow的开发过程说明

## gitflow介绍
- Gitflow工作流定义了一个围绕项目发布的严格分支模型。提供了一个健壮的用于管理大型项目的框架。
- Gitflow工作流为不同的分支分配一个很明确的角色，并定义分支之间如何和什么时候进行交互。除了使用功能分支，在做准备、维护和记录发布也使用各自的分支。

### gitflow流程图
![img](../../image/git-workflow-release-cycle-4maintenance.png)

### 角色分支说明
- master：存储了正式发布的历史，所有的发布都在主分支上进行。发布完成后，在master分支上创建tag。只有主程序员可以将其他分支（release和hotfix）代码合并到master分支
  - 开发：无
  - 测试：无 
- develop：功能集成分支，从master分支上创建，与master并行，所有的功能开发完成后都要合并到develop分支，只有主程序员可以将其他分支（feature和release）代码合并到develop分支
  - 开发：无
  - 测试：针对集成后功能的回归测试
- feature：功能开发分支，每个feature分支包含一个或多个相互独立的功能需求。feature由主程序员或程序员从develop分支中创建而来，新功能完成后，合并回develop分支。新功能提交不直接与master分支交互。feature分支开发人员可以自由推送代码，但当需要将功能合并到develop分支时，需要通过pull request模式交互通过后，由主程序员进行合并。合并完成后删除对应feature分支
  - 开发：功能需求开发、修改测试缺陷
  - 测试：功能需求测试 
  - **feature分支命名规则：feature/日期_需求描述**，如：feature/20180725_问题线索登记
- release：发布分支（稳定版分支），由develop上汇集的已完成功能创建的分支，release分支创建之后，再汇集到develop分支的功能不能在合并到此release分支。release分支完成后，由主程序员将代码分办合并到master和develop分支，合并完成后删除对应release分支
  - 开发：无
  - 测试：稳定版回归测试 
  - **release分支命名规则：release/版本号**，如：release/v1.0.1
- bugfix：缺陷修改分支，所有develop、release分支上涉及需求、缺陷修改， 都需要创建bugfix分支进行修改。之后通过pull request模式交互通过后，由主程序员进行合并到develop、release分支
- hotfix：补丁分支，所有master分支上涉及的临时需求、缺陷修改，都需要创建hotfix分支进行修改。之后通过pull request模式交互通过后，由主程序员进行合并到master、develop、release分支
  - 开发：测试bug修改
  - 测试：bug复测 

## pull request说明
- pull request是在将feature分支向develop分支发起合并时进行的一种交互形式。开发完成一个功能，不是立即合并到develop，而是推送到远程仓库的功能分支上，之后发起一个 Pull Request 请求去合并修改到develop 。 在修改成为主干代码前，这让其它的开发者有机会先去 code Review 变更。
- https://www.jianshu.com/p/6bcd082101c1

## 开发职责说明
- 主程序员：创建develop、feature、hotfix分支，处理pull request请求，合并分支代码到develop和master分支，创建tag
- 程序员：创建feature、bugfix、hotfix分支，完成自己负责分支功能开发、bug修改。提交推送代码，发起pull request。
- 测试人员：确认pull request请求是否可以通过

## 开发过程说明
1. 在gitlab上创建项目、添加成员，分配角色
2. 从master创建develop分支，设置develop分支为默认分支，设置master和develop分支被保护分支
2. 拆分需求，从develop创建feature分支，分配给开发人员
3. feature分支上进行功能开发，推送代码到远程仓库，测试，修改缺陷
4. 发起pull reqeust请求
5. 测试人员确认是否可以合并，同时code review
6. 合并代码到develop分支
7. 从develop创建release分支，设置release分支被保护分支
8. 创建bugfix修改稳定版问题
9. 测试结束将release分支合并到master和develop分支
10. 在master进行发布，创建tag
11. 发布后反馈缺陷，创建hotfix进行解决，之后合并到master、develop、release分支
12. 在master进行补丁发布，创建tag

## 主要事项
- 由于gitflow工作模式下会存在大量的并行分支，为了保证分支间代码的一致性，已通过测试的feature、bugfix、hotfix要及时发起pull request，通过后合并到develop分支。开发人员每天至少从develop同步一次代码到自己的分支。项目可以根据情况约定统一的时间
- **feature之前没有特殊情况不予许直接进行合并**
- 冲突处理，一定要在本地解决后，再推送到远端

## gitflow工作流程实例
http://blog.jobbole.com/76867/

## sourcetree 使用说明
https://www.cnblogs.com/tian-xie/p/6264104.html

## 测试项目
http://open.thunisoft.com/bjjcw/gitflowtest