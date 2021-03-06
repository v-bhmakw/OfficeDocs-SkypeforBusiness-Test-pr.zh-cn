﻿---
title: Lync Server 2013：设置边缘服务器的网络接口
TOCTitle: 设置边缘服务器的网络接口
ms:assetid: b0aecdf6-4ae2-46f6-b9b6-948bfc3df11e
ms:mtpsurl: https://technet.microsoft.com/zh-cn/library/Gg412847(v=OCS.15)
ms:contentKeyID: 49313971
ms.date: 05/19/2016
mtps_version: v=OCS.15
ms.translationtype: HT
---

# 在 Lync Server 2013 中设置边缘服务器的网络接口

 

_**上一次修改主题：** 2012-09-08_

每台边缘服务器均是具有面向外部和内部的接口的多宿主计算机。适配器域名系统 (DNS) 设置取决于外围网络中是否有 DNS 服务器。如果外围中存在 DNS 服务器，则这些服务器必须有一个包含有关下一个跃点服务器或池（即，控制器或指定的前端池）的一个或多个 DNS A 记录的区域，而对于外部查询，则会对其他公共 DNS 服务器进行名称查找。如果外围中不存在 DNS 服务器，则边缘服务器将使用外部 DNS 服务器解析 Internet 名称查找，并且每个边缘服务器会使用 HOST 将下一个跃点服务器名称解析为 IP 地址。

<table>
<thead>
<tr class="header">
<th><img src="images/Gg399038.security(OCS.15).gif" title="security" alt="security" />安全性 注意：</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>为安全起见，建议不要让边缘服务器访问位于内部网络的 DNS 服务器。</td>
</tr>
</tbody>
</table>


## 配置接口（外围网络中存在 DNS 服务器）

1.  为每台边缘服务器安装两个网络适配器，一个用于面向内部的接口，另一个用于面向外部的接口。
    
    <table>
    <thead>
    <tr class="header">
    <th><img src="images/Gg398794.important(OCS.15).gif" title="important" alt="important" />重要提示：</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>内部子网和外部子网不得相互路由。</td>
    </tr>
    </tbody>
    </table>


2.  在外部接口上，在外部外围网络（也称为 DMZ、外围安全区域或屏蔽子网）子网上配置三个静态 IP 地址，并将默认网关指向外部防火墙的内部接口。配置适配器 DNS 设置以指向一对外围 DNS 服务器。
    
    > [!NOTE]  
	> 可以只为此接口分配一个 IP 地址，但是这样做的话，您需要将端口分配更改为非标准值。您可在 拓扑生成器中创建拓扑时对此做出决定。
    


3.  在内部接口上，在内部外围网络子网上配置一个静态 IP 地址，但不要设置默认网关。配置适配器 DNS 设置，以指向至少一台 DNS 服务器（最好是一对外围 DNS 服务器）。

4.  在内部接口上创建到客户端、 Lync Server 2013 和 Exchange 统一消息 (UM) 服务器所在的所有内部网络的永久静态路由。

## 配置接口（外围网络中不存在 DNS 服务器）

1.  为每台边缘服务器安装两个网络适配器，一个用于面向内部的接口，另一个用于面向外部的接口。
    
    <table>
    <thead>
    <tr class="header">
    <th><img src="images/Gg398794.important(OCS.15).gif" title="important" alt="important" />重要提示：</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>内部子网和外部子网不得相互路由。</td>
    </tr>
    </tbody>
    </table>


2.  在外部接口上，在外部外围网络子网上配置三个静态 IP 地址。还可在外部接口上配置默认网关。例如，定义面向 Internet 的路由器或外部防火墙作为默认网关。将 DNS 设置配置为指向一台 DNS 服务器（最好是一对外部 DNS 服务器）。
    
    > [!NOTE]  
	> 可以只对外部接口使用一个 IP 地址，但不建议这样做。若要达到此目的，您需要将端口分配更改为非标准值，并且这个值不是通常对客户端通信“防火墙友好”的默认端口 443。在 拓扑生成器中创建拓扑时可确定 IP 地址设置和端口设置。
    


3.  在内部接口上，在内部外围网络子网上配置一个静态 IP 地址，但不要设置默认网关。将适配器 DNS 设置留空。

4.  在内部接口上创建连接到运行 Lync Server 2013 的 Lync 客户端或服务器所在的所有内部网络的永久静态路由。

5.  在每台边缘服务器上编辑 HOST 文件，以包含下一个跃点服务器或虚拟 IP (VIP) 的记录（记录为控制器、Standard Edition Server 或在 拓扑生成器中配置为边缘服务器下一个跃点地址的前端池）。如果正在使用 DNS 负载平衡，请包含下一个跃点池中所有成员的行。

