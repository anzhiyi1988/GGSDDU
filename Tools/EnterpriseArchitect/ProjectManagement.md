[TOC]

# Project Management Tools

（项目管理工具）

EA是在项目经理的基础上建立起来的，组织知识库是有价值的，必须管理和维护。

风险（Risk ）可以在任何位置建模和管理，并通过内置的度量和估计（Metrics and Estimation）确定项目工作。

任务（Task）可以被创建并分配给特定的角色资源，还可以在甘特图中方便的查看。

审计功能（Audit function）支持细粒度级别跟踪更改。

Team Review facility 和 Element Discussion 允许用户在模型上协作工作。

EA中的每个元素都有许多对项目管理有用的默认属性，如阶段、版本、状态、作者和创建以及修改日期。

本模块提供的如下功能：

**Metrics and Estimation（度量和估算）**

估算（Estimation）项目的时长和工作内容，EA提供Use Case metrics facility作为工具使用：测量系统复杂性；评估项目工作；评估项目时间。

参考： [Use Case Estimation](#use-case-estimation)

**Resources Management（资源管理）**

管理项目工作人员，可以把给某人指定角色，把任务分配给特定模型，这样可以跟踪工作和评估完成时间

参考： [Project Resources](#project-resources)

**Project Maintenance（项目维护）**

监控和管理单个模型的进展，记录在个别元素出现时影响它们的问题、更改、问题和任务

Maintenance Changes,Defects and Issues

**Project Tasks and Issues（任务和问题）**

项目过程中，有许多非技术性任务，他对项目的成功至关重要，比如例会，EA提供记录和监控这些问题的工具。

Project Task Allocation

Project Issues

Project Tasks

# Task Management

任务管理，分配资源给各类任务，有以下几部分内容：

**Manage Project Resources**

资源指项目中的工作人员，可以分配角色或者任务给他们，通过EA跟踪进度和时间。也可以定义工作、风险和统计来支持资源管理。资源的增删改查使用Resource Allocation。

参考： [Project Resources](#project-resources)

**The Gantt View**

EA通过甘特图可视化元素和分配的资源， The Project Gantt View

**Project Task Allocation**

甘特图的特定功能，主要从工作维度查看资源分配情况。

**Review Personal Tasks**

个人视图，每个成员可以记录、审查、管理项目中的个人工作。

**Project Management Windows**

EA提供四个窗口来定义工作的分配

- Resources - the people who work on a project, who can be assigned roles and allocated tasks
- Effort - the effort expended in work on the element
- Risks - the risks associated with the element
- Metrics - the metrics measured for an element

## Project Resources

资源指项目中的工作人员，可以分配角色或者任务给他们，通过EA跟踪进度和时间。也可以定义工作、风险和统计来支持资源管理。资源的增删改查使用Resource Allocation。

访问路径：Construct > Task Management > Resource Allocation

用途：

- 为一个element分配一个资源，定义开始和结束时间
  - Construct > Task Management > Resource Allocation >new

- 为一个element记录其他管理信息，如effort、risk、metrics
  - Construct > Change Management > Effort : New
  - Construct > Change Management > Risk : New
  - Construct > Change Management > Metrics : New

- 获取资源分配细节
  - Construct > Task Management > Gantt : Select the 'Report View' tab
- 通过项目管理窗口配置项目管理数据并填充下拉列表
  - 添加风险类型列表Configure > Reference Data > Project Types > Project Indicators : Risk
  - 配置项目相关人Configure > Reference Data > Project Types > People

## The Project Gantt View

甘特图的展示，主要从三方面：

- 资源维度展示
- element维度展示
- report

## Progress Bars

建立如下进度条：

![1540807766978](image\1540807766978.png)

在每个element的属性页签打开tags选项卡，添加tag，然后配置如下属性

- Type=ProgressBar; - 必须配置，进度条类型
- Syntax:MaxVal=200; - 最大值
- Syntax:Text=#value#anzhy; - 进度条后面的显示内容
- Syntax:Compartment=1; - 同一个元素的进度条分类

# Use Case Estimation
估算项目的任务和时间

过程：

**Calibrating（校准）**

为了更好的估算，必须仔细校准如下因子：

- Technical Complexity Factors,技术复杂性因子，试图量化技术工作的难度和复杂性。
- Environment Complexity Factors,环境复杂性因子， 视图量化非技术工作的难度和复杂性，如团队经验和知识。
- Default Hour Rate,默认小时费率，设置每个用例的小时数。

**Estimating（估算）**

输入校准后的值，可以通过“QA ReportsView”的“Use Case Metrics”选项卡来估算项目的时间。

## Technical Complexity Factors

访问路径：Configure > Reference Data > Project Types > Estimation Factors > Environment Complexity Factors

## Environment Complexity Factors

访问路径：

## Default Hours

访问路径：

## Estimating Project Size

估算项目时间

EA采用的是已建立的简单的估算技术：

- 建立的用例数
- 用例的难度等级
- 项目环境因素
- 建立的参数

只有当有真实的项目作为基线时，此技术才有价值

访问路径：Construct > Project > Status > QA Reports and Use Case Metrics > Use Case Metrics

![Use Case Metrics](image\UseCaseMetrics.png)

Root Package - 

Reload - 

Phase like - 

Keyword like - 

Use Cases - 用例数

Include Actors - 选择参与者

Technical Complexity Factor - 

Environment Complexity Factor - 

Unadjusted Use Case Points(UUCP) - 检查用例复杂度之和

Ave Hours per Use Case - 分配给易、中、难用例的小时数

Total Estimate - 再次确认评估参数细节

Default Rate - 修改默认小时率

Re-Calculate - 修改用例后，从新计算

Report - 生成当前估算的富文本

View Report - 展示最后的报告