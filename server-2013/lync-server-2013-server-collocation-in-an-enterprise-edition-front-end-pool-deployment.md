﻿---
title: Enterprise Edition 前端池部署中的 Lync Server 2013 服务器并置
TOCTitle: Enterprise Edition 前端池部署中的服务器并置
ms:assetid: 0516b18d-14c0-4237-9279-0f92e341b1bd
ms:mtpsurl: https://technet.microsoft.com/zh-cn/library/Gg398102(v=OCS.15)
ms:contentKeyID: 49311863
ms.date: 05/19/2016
mtps_version: v=OCS.15
ms.translationtype: HT
---

# Lync Server 2013 的 Enterprise Edition 前端池部署中的服务器并置

 

_**上一次修改主题：** 2013-11-11_

本节描述可在 Lync Server 2013 前端池部署中并置的服务器角色、数据库和文件共享。

## 服务器角色

在 Lync Server 2013 中，A/V 会议服务、中介服务、监控和存档并置在前端服务器上，但需要进行额外配置才能启用它们。如果不想将中介服务器与前端服务器并置，则可以在单独的计算机上将其部署为独立中介服务器。

可以将受信任应用程序服务器与前端服务器并置。

以下服务器角色必须分别部署在不同的计算机上：

  - 控制器

  - 边缘服务器

  - 中介服务器（若未与前端服务器并置）

  - Office Web Apps Server

不能将 持久聊天 服务器角色与前端服务器并置。

## 数据库

可以将以下每一种数据库并置到相同的数据库服务器：

  - 后端数据库

  - 监控数据库

  - 存档数据库

  - 持久聊天 数据库

  - 持久聊天合规性数据库

可以在单个 SQL Server 实例中并置任何数据库或所有这些数据库，或对每一个数据库使用不同的 SQL Server 实例，但具有以下限制：

  - 每个 SQL Server 实例只能包含一个后端数据库、一个监控数据库、一个存档数据库、一个 持久聊天数据库以及一个 持久聊天合规性数据库。

  - 无论这些数据库使用同一个 SQL Server 实例还是使用单独的 SQL Server 实例，数据库服务器仅支持一个前端池、一个存档部署以及一个监控部署。

可以并置文件共享与数据库，如本节后面所述。

> [!NOTE]  
> 在 Lync Server 2013 中，可以选择为您的部署中的部分或所有用户将存档存储与 Exchange 2013 存储集成。但您无法将任何运行 Lync Server 的服务器或组件部署到与 Exchange 存储相同的服务器上。



<table>
<thead>
<tr class="header">
<th><img src="images/Gg398794.important(OCS.15).gif" title="important" alt="important" />重要提示：</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>尽管支持并置数据库，但数据库的大小增长非常快。例如，当您考虑将存档数据库与其他数据库并置时，您应了解，如果您要对多个用户的消息进行存档，则存档数据库所需的磁盘空间会变得很大。因此，不建议并置多个数据库，特别是将存档数据库、 持久聊天数据库或 持久聊天合规性数据库与后端数据库并置。</td>
</tr>
</tbody>
</table>


## 文件共享

文件共享可以是独立的服务器，也可以并置到与以下任何或所有项相同的服务器：

  - 数据库服务器，包括企业版前端池的后端服务器

  - 存档数据库

  - 监控数据库

  - 持久聊天 数据库

  - 持久聊天合规性数据库

单个文件共享可用于多个前端池、Standard Edition 服务器（全部位于同一站点）。

> [!NOTE]  
> 在 Lync Server 2013 中，监控和存档功能使用 Lync Server 文件共享作为前端服务器。



## 其他组件

反向代理服务器并不是 Lync Server 2013 组件，它只是您想支持联盟用户共享 Web 内容时需要在部署中使用的服务器。因此，您无法将它与任何 Lync Server 2013 服务器角色一起并置。但是，您可以通过在组织中用于支持其他应用程序的现有反向代理服务器上配置反向代理支持来为 Lync Server 2013 部署实现该支持。

不能将任何 Exchange 统一消息 (UM) 组件或 SharePoint 组件与任何 SharePoint Server 角色并置。

