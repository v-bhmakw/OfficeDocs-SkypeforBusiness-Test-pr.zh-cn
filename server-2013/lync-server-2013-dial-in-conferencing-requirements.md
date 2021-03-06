﻿---
title: 电话拨入式会议要求
TOCTitle: 电话拨入式会议要求
ms:assetid: 9aff949e-3dac-481a-be46-a180c72e8066
ms:mtpsurl: https://technet.microsoft.com/zh-cn/library/Gg398802(v=OCS.15)
ms:contentKeyID: 49313723
ms.date: 05/19/2016
mtps_version: v=OCS.15
ms.translationtype: HT
---

# 电话拨入式会议要求

 

_**上一次修改主题：** 2012-09-30_

启动 Lync Server 2013 部署过程之前，您需要规划下列内容：

  - 连接到公用电话交换网 (PSTN) 时使用的配置

  - 为拨入访问号码分配电话拨入式会议区域的策略

  - 创建会议目录的策略

## 规划拨入 PSTN 连接

电话拨入式会议至少需要一台中介服务器和一个 PSTN 网关。

您可以在中央站点或分支站点部署中介服务器。在中央站点，您可以在前端池或 Standard Edition Server 上并置中介服务器，或者将其部署在独立服务器或池中。在分支站点，您可以将中介服务器部署在独立服务器中或部署为 Survivable Branch Appliance 的组件。

您可以在中央站点或分支站点部署 PSTN 网关。在分支站点，PSTN 网关可以是独立的，也可以作为 Survivable Branch Appliance 的组件。

> [!NOTE]  
> 电话拨入式会议不使用媒体旁路，因为 A/V 会议服务器不支持媒体旁路。



有关针对电话拨入式会议规划中介服务器和 PSTN 网关配置的详细信息，请参阅规划文档中的[Lync Server 2013 中中介服务器的组件和拓扑](lync-server-2013-components-and-topologies-for-mediation-server.md)。

## 规划电话拨入式会议区域

在拨入配置过程中，创建拨号计划和电话拨入式会议访问号码。拨号计划是几组规范化规则，用来指定电话号码的数字和数字模式，并将电话号码转换为标准 E.164 格式以进行呼叫路由。电话拨入式会议访问号码是参与者为加入会议而呼叫的号码。

每个电话拨入式会议访问号码必须至少与一个拨号计划关联。电话拨入式会议区域会将电话拨入式会议访问号码与其拨号计划相关联。设置拨号计划时，指定应用于该拨号计划的电话拨入式会议区域。在创建拨入访问号码时，选择将该访问号码与相应拨号计划关联的区域。

创建拨号计划时，指定拨号计划的作用域：用户作用域、池作用域或站点作用域。会按照各用户适用的最小作用域为其分配拨号计划。例如，如果适用，则为用户分配用户级别的拨号计划。如果用户级别的拨号计划不适用，则为用户分配池级别的拨号计划。如果池级别的拨号计划不适用，则为用户分配站点级别的拨号计划。如果站点级别的拨号计划不适用，则为用户分配全局拨号计划。

配置拨号计划之前，规划命名和使用区域的方式非常重要。对于电话拨入式会议区域，请注意下列事项：

  - 区域通常是与某个办公室或一组办公室相关联的地理区域。

  - 语言与拨入访问号码相关联。如果支持具有多种语言的地理区域，应该决定如何定义区域以便支持多种语言。例如，可以基于地理位置和语言的组合定义多个区域，或者基于地理位置定义一个区域，且每种语言拥有不同的拨入访问号码。

  - 用户安排会议时，默认情况下会议将使用用户的拨号计划所指定的区域。

  - 默认情况下，区域的所有拨入访问号码都会包含在会议邀请中。

  - 命名区域时应确保可清楚识别，这一点非常重要。用户可以使用区域的名称来更改会议的区域，以便在邀请中包含不同的访问号码。（当用户使用 Outlook 安排会议时，可利用 Lync 2013 联机会议外接程序来更改区域。）

  - 应该对区域加以设计，以便想要拨入会议的被邀请者可以在会议邀请中看到本地访问号码。

  - 您可以使用 Lync Server 命令行管理程序 cmdlet 配置区域内的访问号码在电话拨入式会议设置页上显示的顺序（也是在会议邀请中显示的顺序）。

  - 任何位置的任何用户均可拨打拨入访问号码来加入会议。

## 规划会议目录

会议目录将维护使用 Lync 2013 时参与者加入会议所使用的会议 ID（包含字母数字）与电话拨入式会议参与者加入会议所使用的会议 ID（仅包含数字）之间的映射。会议 ID 的格式如下：

    <housekeeping digit (1 digit)><conference directory (usually 1-2 digits)><conference number (variable number of digits><check digit (1 digit)>

创建多个会议目录将确保会议 ID 将短暂停留，直到创建了大量会议。在每个用户需要参与典型数量会议的组织中，建议您为池中的每 999 个用户创建一个会议目录。通过使用此指南，通常可使会议 ID 保持较小数目。但是，一旦会议目录的数目（在池中）超过 9，则会议 ID 号将增大以支持其他会议。

## 另请参阅

#### 概念

[Lync Server 2013 中的中介服务器组件](lync-server-2013-mediation-server-component.md)  
[Lync Server 2013 中的拨号计划和规范化规则](lync-server-2013-dial-plans-and-normalization-rules.md)

