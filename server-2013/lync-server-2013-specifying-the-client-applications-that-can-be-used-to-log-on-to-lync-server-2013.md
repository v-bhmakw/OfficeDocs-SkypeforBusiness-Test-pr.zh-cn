﻿---
title: 指定要用于登录到 Lync Server 2013 的客户端应用程序
TOCTitle: 指定要用于登录到 Lync Server 2013 的客户端应用程序
ms:assetid: d256a581-9a48-4d1a-82cc-2e1f520d7d2e
ms:mtpsurl: https://technet.microsoft.com/zh-cn/library/Gg182591(v=OCS.15)
ms:contentKeyID: 49314334
ms.date: 05/19/2016
mtps_version: v=OCS.15
ms.translationtype: HT
---

# 指定要用于登录到 Lync Server 2013 的客户端应用程序

 

_**上一次修改主题：** 2012-12-11_

通过 Lync Server 2013，可以指定环境中支持的客户端版本。当两个不同版本的客户端交互时，其中一个客户端的可用功能会受到另一个客户端的功能的限制。为了充分利用 Lync Server 2013 中包含的各种功能并改善整体用户体验，可以使用客户端版本筛选器来限制 Lync Server 2013 环境中使用的客户端版本。使用客户端版本筛选器还有助于降低支持多个客户端版本的相关成本。

除了创建全局策略外，还可以为特定服务或站点创建客户端版本策略，或创建可分配给单个用户的用户作用域策略。可以在 Lync Server 控制面板中将用户作用域客户端版本策略分配给“用户”组中的各个用户。

> [!NOTE]  
> 由于匿名用户未与用户、站点或服务关联，因此匿名用户仅受全局级别策略的影响。



<table>
<thead>
<tr class="header">
<th><img src="images/Gg398794.important(OCS.15).gif" title="important" alt="important" />重要提示：</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>按优先顺序列出筛选器。例如，如果您具有一个筛选器可允许运行版本 1.5 的客户端进行连接，还有另一个筛选器可阻止运行低于 2.0 版本的客户端，则第一个筛选器优先，允许运行版本 1.5 的客户端进行连接。</td>
</tr>
</tbody>
</table>


## 编辑默认客户端版本策略

1.  使用分配给 CsUserAdministrator 或 CsAdministrator 角色的用户帐户，登录到内部部署中的任何计算机。

2.  打开浏览器窗口，然后输入管理 URL 以打开 Lync Server 控制面板。有关可以用于启动 Lync Server 控制面板的不同方法的详细信息，请参阅[打开 Lync Server 管理工具](lync-server-2013-open-lync-server-administrative-tools.md)。

3.  在左侧导航栏中，单击“客户端”。
    
    > [!NOTE]  
	> 默认情况下，选中“客户端版本策略”选项卡。
    


4.  在“客户端版本策略”页上，双击列表中的“全局”策略。

5.  在“编辑客户端版本策略”中，执行下列操作之一：
    
      - 单击“新建”创建新的客户端版本规则。
    
      - 单击列表中已定义的客户端类型之一，然后单击“显示详细信息”。
    
    <table>
    <thead>
    <tr class="header">
    <th><img src="images/Dn783119.note(OCS.15).gif" title="note" alt="note" />注意：</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>可以使用通配符来指示客户端类型。</td>
    </tr>
    </tbody>
    </table>


6.  在“用户代理”中，选择客户端类型。

7.  在“版本号”下，执行下列操作：
    
      - 在“主版本”中，键入与客户端的主版本相对应的版本号。
    
      - 在“次版本”中，键入与客户端的次版本相对应的版本号。
    
      - 在“内部版本”中，键入与客户端的主版本和次版本相对应的版本号。
    
      - 在“更新”中，键入与客户端的更新版本相对应的版本号。
    
    > [!NOTE]  
	> 可以使用通配符来指示客户端版本号。
    


8.  要为上述步骤中指定的客户端版本指定匹配操作，请在“比较操作”中，单击下列选项之一：
    
      - **相同**
    
      - **不是**
    
      - **更高**
    
      - **更高或相同**
    
      - **更低**
    
      - **更低或相同**

9.  要指定在满足上述步骤中的条件时执行的操作，请在“操作”中，单击下列选项之一：
    
      - 要允许客户端登录，请单击“允许”。
    
      - 要允许客户端登录并接收来自 Windows Server 更新服务或 Microsoft Update 的更新，请单击“允许并升级”。仅当选中用户代理“OC”时，才能进行此操作。
        
        > [!NOTE]  
		> 选择此操作会使得在用户下次登录到 Lync 2013 时显示一个通知。该通知指出有可用更新，即使更新尚未发布到 Windows Server Update Service 或 Microsoft Update。为了避免混淆，您只应在更新可用后选择此操作。
        
    
      - 要允许客户端登录并显示有关下载其他客户端版本的位置的消息，请单击“使用 URL 允许”。需要在此过程的后面阶段指定 URL。
    
      - 要阻止客户端登录，请单击“阻止”。
    
      - 要阻止客户端登录并允许客户端接收来自 Windows Server 更新服务或 Microsoft Update 的更新，请单击“阻止并升级”。仅当选中用户代理“OC”时，才能进行此操作。
    
      - 要阻止客户端登录并显示有关下载其他客户端版本的位置的消息，请单击“使用 URL 阻止”。需要在此过程的后面阶段指定 URL。

10. （可选）如果在上一步中单击“使用 URL 允许”或“使用 URL 阻止”，则在“URL”中键入要包含在消息中的客户端下载 URL。

11. 单击“确定”，然后单击“提交”。

## 另请参阅

#### 其他资源

[在 Lync Server 2013 中管理设备、电话和客户端应用程序](lync-server-2013-managing-devices-phones-and-client-applications.md)

