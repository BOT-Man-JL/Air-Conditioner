﻿# 分布式温控系统用例模型说明书

> 2014211306 班 F 组
>
> 李俊宁 董星彤 张有杰 赵亮 左旭彤
>
> 2017 年 04 月 18 日

## Table of Contents

[TOC]

[page-break]

> 迭代历史：
>
> - v0.1 | 2017/4/17 | John Lee | 创建文档
> - v0.2 | 2017/4/17 | 张有杰 | 完成主控机用例描述部分
> - v0.3 | 2017/4/18 | 赵亮 | 完成文档介绍、项目背景
> - v0.4 | 2017/4/18 | 左旭彤 董星彤 | 完成从控机用例描述部分
> - v0.5 | 2017/4/18 | John Lee | 添加用例图
> - v0.6 | 2017/4/18 | 张有杰   | 完成主控机用例描述部分编号修正
> - v1.0 | 2017/4/18 | John Lee | 发布文档

## 1. 文档介绍

### 1.1 文档目的

该文档主要的目的建立用例模型。
根据已经确定的需求分析来描述用户以及各个子功能系统之间的交互场景，
对每一个场景建立相应的用例模型，
软件开发人员根据此模型来进行软件的详细设计与开发。

### 1.2 文档范围

围绕分布式温控系统展开，
详细地描述分布式温控系统的用例图、用例说明、领域模型以及系统操作契约等。
通过用例图构建出所有可能用例与各个参与者之间的框架结构关系，
然后对每一个交互场景进行详细描述和分析。

### 1.3 读者对象

用户、设计人员、编码人员和测试人员等

### 1.4 参考文献

- 《分布式温控系统用户需求说明书》
- 《用例模型说明书模板》
- 《分布式温控系统详细要求》

### 1.5 术语与缩写解释

| 术语 | 解释 |
|--|--|
| UC_M | 主控机的用例模型 |
| UC_S | 从控机的用例模型 |

## 2. 项目背景

更具如下需求，设计中央空调系统：

1. 空调系统由中央空调和房间空调（从控机）两部分构成。中央空调由特定的管理人员操控，房间空调由用户来操控。
2. 中央空调具备开关按钮，只可人工开启和关闭，正常开启后处于待机状态。有冷暖两种工作模式，每种模式有特定的缺省温度以及温度区间，可根据季节进行工作模式的调整。中央空调能够实时监测个房间的温度和状态，并且能够根据实时刷新的频率进行配置。
3. 从控机只能人工方式开闭，可以通过控制面板设置目标温度，目标温度有上下限制。控制面板的温度调节可以连续变化也可以断续变化。每个从控机内有一个温度传感器，可以实时监测房间的温度。
4. 从控机的控制面板能够发送高、中、低风速的请求，各小组可以自定义高、中、低三种风速下的温度变化值。中央空调可以根据从控机的请求时长及高中低风速的供风量计算每个房间所消耗的能量以及所需支付的金额，并将对应信息发送给每个从控机进行在线显示，以便客户查询。
5. 中央空调监控具备统计功能，可以根据需要给出日报表、周报表和月报表；报表内容如下：房间号、从控机开关机的次数、温控请求起止时间（列出所有记录）、温控请求的起止温度及风量大小（列出所有记录）、每次温控请求所需费用、每日（周、月）所需总费用。
6. 中央空调同时只能处理三台从控机的请求，为此主机要有负载均衡的能力。如果有超过三台从控机请求，则需要对所有请求机器进行调度，调度算法自行定义。

## 3. 用例图

### 3.1 从控机用例图

### 3.2 从控机用例图

![Slave-User-Cases](diagrams/user-cases-slave.svg)

```
[a1 最终用户]-(c1 控制空调)
(c1 控制空调)>(i1 启动从控机)
(c1 控制空调)>(i2 用户登录系统)
(c1 控制空调)<(x1 改变温度)
(c1 控制空调)<(x2 调节风速)
[a2 时钟]-(触发从控机子系统的自动更新功能)
```

## 4. 用例说明

### 4.1 主控机用例描述

#### 4.1.1 主控机 001 用例描述

- 用例编号:UC_M_001
- 用例名称:运行状态切换
- 范围:主控机
- 级别:用户目标级别
- 主要参与者:管理员
- 项目相关人员及其兴趣:管理员:完成开、关机状态之间的切换
- 前置条件:主控机系统正常无故障
- 后置条件:主控机系统切换状态
- 主要成功场景
  1. 管理员选择目标切换状态
  2. 系统成功完成想目标状态的切换，用例结束。
- 扩展（或替代流程)
  - \*a 任何时刻，系统停电或硬件不正常
    - 记录日志，系统关闭
    - 管理人员重启系统
  - 1a. 管理员开启主控机系统
    - 转入扩展用例: 主控机开机 UC_M_001_1
    - 进入成功场景 2
  - 1b. 管理员关闭主控机系统
    - 转入扩展用例: 主控机关机 UC_M_001_2
    - 进入成功场景 2
- 特殊需求
  - 无
- 发生频率
  - 管理员切换主控机运行状态时

##### 4.1.1.1 主控机 001_1 用例描述

- 用例编号:UC_M_001_1
- 用例名称:主控机开机
- 范围:主控机
- 级别:子功能级别
- 主要参与者:管理员
- 项目相关人员及其兴趣:管理员:成功打开主控机，使其保持待机状态。
- 前置条件:主控机系统处于关机状态
- 后置条件:主控机系统成功开机，进入待机状态
- 主要成功场景
  1. 管理员手动按下开机按钮
  2. 管理员成功验证身份
  3. 主控机成功开机，进入管理界面，用例结束
- 扩展（或替代流程)
  - \*a 任何时刻，系统停电或硬件不正常
    - 记录日志，系统关闭
    - 管理人员重启系统，系统重新进入成功场景 1
  - 2a. 管理员验证身份失败，用户名或密码错误
    - 重新验证身份，系统重新进入成功场景 1
  - 2b. 失败次数超过三次
    - 主控机系统锁定。
    - 管理员重启系统，系统重新进入成功场景 1
  - 2c. 主机系统锁定
    - 使用预设PIN码解锁系统，重新设置管理员账户密码，返回成功场景 1
- 特殊需求
  - 无
- 发生频率
  - 管理员打开主控机时使用

##### 4.1.1.2 主控机 001_2 用例描述

- 用例编号:UC_M_001_2
- 用例名称:主控机关机
- 范围:主控机
- 级别:子功能级别
- 主要参与者:管理员
- 项目相关人员及其兴趣:管理员:关闭主控机。
- 前置条件:主控机系统处于开机状态
- 后置条件:主控机系统成功关机。
- 主要成功场景
  1. 管理员点击关机按钮
  2. 系统停止所有监控及温控服务，记录日志
  3. 系统向所有从机发送关机信息
  2. 主控机系统成功关机，用例结束。
- 扩展（或替代流程)
  - 无
- 特殊需求
  - 无
- 发生频率
  - 管理员关闭主控机时使用

#### 4.1.2 主控机 002 用例描述

- 用例编号:UC_M_002
- 用例名称:选项配置更改
- 范围:主控机
- 级别:用户目标级别
- 主要参与者:管理员
- 项目相关人员及其兴趣:管理员:对系统选项及参数进行配置
- 前置条件:主控机系统处于开机状态，正常运行
- 后置条件:主控机配置修改成功
- 主要成功场景
  1. 管理员打开配置管理界面
  2. 管理员选择想要更改的配置选项
  3. 管理员输入新的参数选项并提交
  4. 系统完成新参数选项的配置，用例结束
- 扩展（或替代流程)
  - 2a: 管理员选择工作模式的配置
    - 切换主控机供暖/制冷的工作模式
    - 返回成功场景 3
  - 2b. 管理员调整计费标准
    - 调节不同风速的收费标准
    - 返回成功场景 3
  - 2c. 负载均衡策略配置
    - 调整使用不同的负载均衡策略
    - 返回成功场景 3
  - 2d. 刷新频率设置
    - 从控机心跳频率设置
    - 返回成功场景 3
  - 2e. 系统配置按默认值初始化
    - 进入成功场景 4
- 特殊需求
  - 无
- 发生频率
  - 管理员更改系统配置时使用

#### 4.1.3 主控机 003 用例描述

- 用例编号:UC_M_003
- 用例名称:报表管理
- 范围:主控机
- 级别:用户目标级用例
- 主要参与者:管理员
- 前置条件:系统正常运行
- 后置条件:管理员成功完成报表管理
- 主要成功场景
  1. 管理员打开报表管理界面
  2. 选择具体管理项，进行报表管理
  3. 完成报表管理，用例结束
- 扩展（或替代流程)
  - 2a. 选择报表导入导出选项，进入扩展用例: 报表导入导出 UC_M_003_1
  - 2b. 选择报表查询选项，进入扩展用例: 报表查询 UC_M_003_2
- 特殊需求
  - 无
- 发生频率
  - 管理员管理报表时

##### 4.1.3.1 主控机 003_1 用例描述

- 用例编号:UC_M_003_1
- 用例名称:报表导入导出
- 范围:主控机
- 级别:子用例级别
- 主要参与者:管理员
- 项目相关人员及其兴趣:管理员:对主控机报表进行导入导出
- 前置条件:系统进入报表管理界面
- 后置条件:主控机成功管理报表
- 主要成功场景
  1. 管理员选择导入导出选项
  2. 进行报表管理，用例结束。
- 扩展（或替代流程)
  - 2a. 选择打印报表
    - 系统连接外设，打印报表数据
  - 2b. 选择导入报表
    - 选择数据库文件，进行报表数据导入
  - 2c. 选择导出报表
    - 选择报表导出位置及文件类型，完成报表数据导出
- 特殊需求
  - 系统在导入及导出报表时对不同的数据库文件应具有兼容性
  - 系统打印报表时应可以选择具体的打印选项，如选择纸张大小等。
- 发生频率
  - 管理员需要导入及导出报表内容时

##### 4.1.3.2 主控机 003_2 用例描述

- 用例编号:UC_M_003_2
- 用例名称:报表查询
- 范围:主控机
- 级别:子用例级别
- 主要参与者:管理员
- 项目相关人员及其兴趣:管理员:查询报表内容
- 前置条件:系统进入报表管理界面
- 后置条件:管理员成功查询报表，系统显示报表内容
- 主要成功场景
  1. 管理员选择查询的报表类型
  2. 管理员选择目标报表选项，如日期、时间段、用户、房间号等
  3. 验证报表信息合法，成功查询
  4. 系统显示相应的报表报表，用例结束
- 扩展（或替代流程)
  - 1a. 管理员选择日报表，进入成功场景 2
  - 1b. 管理员选择月报表，进入成功场景 2
  - 1c. 管理员选择年报表，进入成功场景 2
  - 3a. 报表选项不合法，重新输入
    - 返回成功场景 2
  - 3b. 报表选项合法
    - 进入成功场景 3
- 特殊需求
  - 系统可以以图表等方式灵活显示报表统计信息
- 发生频率
  - 管理员查询报表时

#### 4.1.4 主控机 004 用例描述

- 用例编号:UC_M_004
- 用例名称:从控机状态监测
- 范围:主控机
- 级别:用户目标级别
- 主要参与者:管理员
- 项目相关人员及其兴趣:管理员:主控机能够对从控机目标温度、当前温度及风速进行统计
- 前置条件:系统正常运行
- 后置条件:主控机成功监控从控机状态
- 主要成功场景
  1. 管理员点开从控机信息监控界面
  2. 主控机监控从控机信息
  3. 主控机正常完成监控功能，用例结束
- 扩展（或替代流程)
  - 2a. 主控机统计从控机费用及耗能情况，转入扩展用例: 费用统计 UC_M_004_1
  - 2b. 主控机监控从控机目标温度、当前温度、风速信息
- 特殊需求
  - 主控机应能够监测出从控机的异常情况，比如长时间低温、高温运转等。
- 发生频率
    - 管理员需要导入及导出报表内容时

##### 4.1.4.1 主控机 004_1 用例描述

- 用例编号:UC_M_004_1
- 用例名称:费用统计
- 范围:主控机
- 级别:子功能级别
- 主要参与者:管理员
- 项目相关人员及其兴趣:管理员:主控机能够正确统计各从控机的费用信息
- 前置条件:系统正常运行，监测各房间状态
- 后置条件:主控机实时统计费用
- 主要成功场景
  1. 从控机费用正确统计并显示。
  2. 从控机能耗正确统计并显示。
  3. 用例结束。
- 扩展（或替代流程)
  - 无
- 特殊需求
  - 主控机应能够监测出从控机的异常情况，比如费用异常增多，能耗过大等。
- 发生频率
    - 从控机开启，并发出温控请求后。

#### 4.1.5 主控机 005 用例描述

- 用例编号:UC_M_005
- 用例名称:从控机获取信息
- 范围:主控机
- 级别:用户目标级别
- 主要参与者:从控机
- 项目相关人员及其兴趣:从控机:获取从控机的耗能和计费情况及主控机状态
- 前置条件:主控机正常运行并成功统计并显示从控机的耗能和计费情况
- 后置条件:从控机成功获取从控机耗能及计费情况、主控机状态
- 主要成功场景
  1. 从控机向主控机周期性发出查询请求
  2. 从控机成功收到查询结果
  3. 从控机显示耗能和统计情况及主控机工作状态
  4. 信息获取成功，用例结束
- 扩展（或替代流程)
  - 无
- 特殊需求
  - 从控机可以采用图表、数字等方式结合，清晰显示查询结果。
- 发生频率
  - 从控机周期性获取信息

#### 4.1.6 主控机 006 用例描述

- 用例编号:UC_M_006
- 用例名称:从控机调控
- 范围:主控机
- 级别:用户目标级别
- 主要参与者:从控机
- 项目相关人员及其兴趣:从控机:向主控机发出调控请求
- 前置条件:主控机正常送风
- 后置条件:主控机响应请求，进行温控
- 主要成功场景
  1. 从控机向主控机发出温控请求
  2. 主控机收到温控请求，进行调控
  3. 主控机响应温控请求，用例结束
- 扩展（或替代流程)
  - 1a 从控机发出目标温度调节请求，转入扩展用例: 调节温度请求 UC_M_006_1
  - 1b 从控机发出风速调节请求，转入扩展用例: 调节风速请求 UC_M_006_2
  - 1c 从控机发出停止送风请求，转入扩展用例: 停止送风请求 UC_M_006_3
- 特殊需求
  - 面对时间间隔过低的来自同一从控机的请求，主控机只响应最后一次请求。
  - 主控机对从控机请求的接受时间应在可接受的范围内，如1s。
- 发生频率
  - 用户使用从控机发出调控请求

##### 4.1.6.1 主控机 006_1 用例描述

- 用例编号:UC_M_006_1
- 用例名称:调节温度请求
- 范围:主控机
- 级别:子功能级别
- 主要参与者:从控机
- 项目相关人员及其兴趣:从控机:向主控机发出调节目标温度的请求 主控机:接受从机调节请求，并返回确认信息
- 前置条件:主控机正常送风，从机正常运行
- 后置条件:主控机响应请求，成功调整从控机目标温度，并向从机发出确认信息
- 主要成功场景
  1. 从控机向主控机发出调节温度的请求
  2. 主控机收到温控请求，进行目标温度的调整
  3. 主控机成功响应温度调整请求，用例结束
- 扩展（或替代流程)
  - 2a. 若目标温度与主控机工作模式对应的温度范围矛盾
     - 报告错误信息，响应从控机
  - 2b. 目标温度处于主控机工作模式对应的温度范围内
     - 转入成功场景 3
  - 3a. 若当前从控机数目超过主控机负载能力
     - 从控机请求服从主控机负载均衡策略
  - 3b. 主控机立刻相应从控机请求
- 特殊需求
  - 主控机对从控机请求的接受时间应在可接受的范围内，如1s。
- 发生频率
  - 用户使用从控机调节温度

##### 4.1.6.2 主控机 006_2 用例描述

- 用例编号:UC_M_006_2
- 用例名称:调节风速请求
- 范围:主控机
- 级别:子功能级别
- 主要参与者:从控机
- 项目相关人员及其兴趣:从控机:向主控机发出调整风速的请求 主控机:接受从机调节请求，并返回确认信息
- 前置条件:主控机正常送风，从机正常运行
- 后置条件:主控机响应请求，调整分配给从控机的风速，并向从机发出确认信息
- 主要成功场景
  1. 从控机向主控机发出风速调整请求
  2. 主控机收到风控请求，进行风速调整
  3. 主控机成功响应风控请求，向从控机返回确认信息，用例结束
- 扩展（或替代流程)
  - 1a. 从控机发送请求时已达到目标温度并停止送风
    - 主控机发送“等待”响应
    - 从控机实际温度与目标温度相差1℃时，再次响应从控机请求，开始送风。
  - 1b. 从控机温度未达到目标温度
    - 主控机确认从控机请求
- 特殊需求
  - 主控机对从控机请求的接受时间应在可接受的范围内，如1s。
- 发生频率
  - 用户使用从控机调节风速
  
##### 4.1.6.3 主控机 006_3 用例描述

- 用例编号:UC_M_006_3
- 用例名称:停止送风请求
- 范围:主控机
- 级别:子功能级别
- 主要参与者:从控机
- 项目相关人员及其兴趣:从控机:向主控机发出停止送风请求 主控机:接受从机请求，并返回确认信息
- 前置条件:主控机正常送风，从机正常运行
- 后置条件:主控机响应请求，停止送风，并向从机发出确认信息
- 主要成功场景
  1. 室内温度达到预期设定温度，从控机向主控机发出停止送风请求
  2. 主控机收到请求，停止送风
  3. 主控机向从控机返回确认信息，用例结束
- 扩展（或替代流程)
  - 无
- 特殊需求
  - 无
- 发生频率
  - 室内温度达到预期设定温度

### 4.2 从控机用例描述

#### 4.2.1 从控机001用例描述

- 用例编号:UC_S_001
- 用例名称:控制空调
- 范围:分布式中央空调控制系统从控机子系统
- 级别:用户目标级别
- 主要参与者:最终用户
- 项目相关人员及其兴趣:最终用户：调节从控机子系统，从控机子系统：接受或拒绝请求
- 前置条件:主控机已经被人工开启，用户去操作控制面板
- 后置条件:用户完成对温度的调节控制
- 主要成功场景
  1. 用户请求使用控制面板控制温度
  2. 如果之前此房间用户未有请求，转扩展用例：启动从控机 US_S_001_1
  3. 包含用例：登录系统 US_S_001_2
  4. 如果用户点击温度调节按钮，转扩展用例：改变温度 US_S_001_3
  5. 如果用户点击风速调节按钮，转扩展用例：改变风速 US_S_001_4
  6. 温度控制完成，用例结束
- 扩展（或替代流程)
  - *a 停电，等待来电
  - *b 系统崩溃，等待系统管理员处理突发事件
- 特殊需求
  - 无
- 发生频率
  - 用户每次使用从控机时启动

##### 4.2.1.1 从控机001_1用例描述

- 用例编号:UC_S_001_1
- 用例名称:启动从控机
- 范围:分布式中央空调控制系统从控机子系统
- 级别:子系统目标级别
- 主要参与者:最终用户
- 项目相关人员及其兴趣:最终用户：启动从控机子系统，从控机子系统：接受或拒绝请求
- 前置条件:从控机未被打开，用户去操作控制面板
- 后置条件:用户启动从控机
- 主要成功场景
  1. 用户要求启动从控机并点击开机键
  2. 开机完成，用例结束
- 扩展（或替代流程)
  - *a 停电，等待来电
  - *b 系统崩溃，等待系统管理员处理突发事件
- 特殊需求
  - 无
- 发生频率
  - 用户开启从控机

##### 4.2.1.2 从控机001_2用例描述

- 用例编号:UC_S_001_2
- 用例名称:用户登录系统
- 范围:分布式中央空调控制系统从控机子系统
- 级别:子系统目标级别
- 主要参与者:最终用户
- 项目相关人员及其兴趣:最终用户：登录子系统，从控机子系统：接受或拒绝请求
- 前置条件:主控机已经被人工开启并开始工作，从控机被打开，用户去操作控制面板
- 后置条件:用户成功登录系统
- 主要成功场景
  1. 从控机开机，与中央空调完成连接认证
  2. 用户输入房间号+身份证号
  3. 用户登录成功，扩展用例结束
- 扩展（或替代流程)
  - *a 停电，等待来电
  - *b 系统崩溃，等待系统管理员处理突发事件
  - 1a  用户启动从控机，但是主控机没有被启动：等待系统管理员处理
  - 2a  用户输入的房间号或者身份证号不正确，无法完成认证
- 特殊需求
  - 无
- 发生频率
  - 用户开启从控机

##### 4.2.1.3 从控机001_3用例描述

- 用例编号:UC_S_001_3
- 用例名称:改变温度
- 范围:分布式中央空调控制系统从控机子系统
- 级别:子系统目标级别
- 主要参与者:最终用户
- 项目相关人员及其兴趣:最终用户：调节空调温度，从控机子系统：接受或拒绝请求
- 前置条件:主控机已经被人工开启并开始工作，从控机被打开，用户去操作控制面板
- 后置条件:温度改变设置成功，从控机面板显示当前设置温度
- 主要成功场景1
  1. 用户请求上下调节温度
  2. 用户按动温度调节按钮连续两次或多次指令的时间间隔小于1s
  3. 系统接收最后一次调节的指令，调节成功，用例结束
- 主要成功场景2
  1. 用户请求上下调节温度
  2. 用户按动温度调节按钮连续两次或多次指令的时间间隔大于1s
  3. 系统接收所有调节的指令，调节成功，用例结束
- 扩展（或替代流程)
  - *a 停电，等待来电
  - *b 系统崩溃，等待系统管理员处理突发事件
  - 1a  用户请求调节温度，但是从控机没有响应：等待系统管理员处理
  - 2a  用户调节温度时，请求的温度超过了合理范围（供暖温度25°C～30°C，制冷温度18°C～25°C）
- 特殊需求
  - 无
- 发生频率
  - 用户调节温度时开启

##### 4.2.1.4 从控机001_4用例描述

- 用例编号:UC_S_001_4
- 用例名称:调节风速
- 范围:分布式中央空调控制系统从控机子系统
- 级别:子系统目标级别
- 主要参与者:最终用户
- 项目相关人员及其兴趣:最终用户：调节空调风速，从控机子系统：接受或拒绝请求
- 前置条件:中央空调主控机处于开启状态，中央空调从控机处于开启状态，用户登陆成功
- 后置条件:用户完成对风速的调节，从控机面板显示当前风速
- 主要成功场景1
  1. 用户请求使用控制面板调节风速
  2. 用户按动温度调节按钮连续两次或多次指令的时间间隔小于1s
  3. 系统接收最后一次调节的指令，调节成功，用例结束
- 扩展（或替代流程)
  - *a 停电，等待来电
  - *b 系统崩溃，等待系统管理员处理突发事件
  - 3a 用户选择了与当前风速相同的风速，子系统检测到请求风速与当前风速相同，不予理会，风速调节失败，用例结束
  - 3b 用户选择了与当前风速不同的风速，主控机系统检测到房间温度与设定目标温度相同，不给该房间送风，风速调节失败，用例结束
  - 3c 用户选择了与当前风速不同的风速，主控机系统目前处于负载均衡状态，不给该房间送风或固定送某种风，风速调节失败，用例结束
- 特殊需求
  - 无
- 发生频率
  - 用户调节风速时开启

#### 4.2.2 从控机002用例描述

- 用例编号:UC_S_002
- 用例名称:触发从控机子系统的自动更新功能
- 范围:分布式中央空调控制系统从控机子系统
- 级别:用户目标级别
- 主要参与者:计时器（时钟）
- 项目相关人员及其兴趣：时钟：周期性发送脉冲出发从控机子系统自动更新
- 前置条件:中央空调主控机处于开启状态，中央空调从控机处于开启状态，用户登陆成功
- 后置条件:从控机更新显示的中央空调工作模式，工作温度，能量消耗和金额等信息
- 主要成功场景
  1. 计时器周期性地向从控机系统发送脉冲，触发从控机子系统的自动更新功能
- 扩展（或替代流程)
  - *a 停电，等待来电
  - *b 系统崩溃，等待系统管理员处理突发事件
  - *c 时钟在任意时刻发送脉冲失败，等待系统重启
- 特殊需求
  - 无
- 发生频率
  - 每隔周期T发生