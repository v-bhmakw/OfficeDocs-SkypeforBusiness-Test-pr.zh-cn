﻿---
title: 分配每用户存档策略
TOCTitle: 分配每用户存档策略
ms:assetid: a12ca483-b235-460f-b3fe-130fb3087264
ms:mtpsurl: https://technet.microsoft.com/zh-cn/library/Gg182560(v=OCS.15)
ms:contentKeyID: 49313779
ms.date: 05/19/2016
mtps_version: v=OCS.15
ms.translationtype: HT
---

# 分配每用户存档策略

 

_**上一次修改主题：** 2013-02-22_

存档策略是可以在 Lync Server 2013 控制面板中配置的用户帐户的各项设置之一。

部署一个或多个每用户存档策略是可选的。还可以只部署一个全局级别的存档策略或站点级别的存档策略。如果要部署每用户策略，则必须将其明确分配给用户、组或联系人对象。未分配特定的站点级别或每用户策略时，会自动默认使用全局级别会议策略中定义的存档要求。

创建至少一个每用户存档策略之后，请使用本主题中的过程分配策略，该策略相应地指定服务器将存档特定用户的内部通信、外部通信还是两者都存档。

有关创建每用户存档策略的详细信息，请参阅[创建存档策略以启用或禁用特定站点或用户的内部或外部通信的存档](lync-server-2013-creating-an-archiving-policy-to-enable-or-disable-archiving-of-internal-or-external-communications-for-specific-sites-or-users.md)。

## 分配每用户存档策略

1.  使用分配给 CsUserAdministrator 或 CsAdministrator 角色的用户帐户，登录到内部部署中的任何计算机。

2.  打开浏览器窗口，然后输入管理 URL 以打开 Lync Server 控制面板。有关可以用于启动 Lync Server 控制面板的不同方法的详细信息，请参阅[打开 Lync Server 管理工具](lync-server-2013-open-lync-server-administrative-tools.md)。

3.  在左侧导航栏中，单击“用户”。

4.  使用下列方法之一查找用户：
    
      - 在“搜索用户”框中，键入用户帐户的显示名称、名字、姓氏、安全帐户管理器 (SAM) 帐户名、SIP 地址或线路统一资源标识符 (URI) 的全部或第一部分，然后单击“查找”。
    
      - 如果具有保存的查询，请单击“打开查询”图标，使用“打开”对话框来检索该查询（.usf 文件），然后单击“查找”。

5.  （可选）指定附加搜索条件以缩小结果的范围：
    
    1.  单击“添加筛选器”。
    
    2.  通过键入用户属性，或单击下拉列表中的箭头选择属性来输入用户属性。
    
    3.  在“等于”下拉列表中，单击运算符（例如“等于”或“不等于”）。
    
    4.  根据所选的用户属性，通过键入条件或单击下拉列表中的箭头输入用于筛选搜索结果的条件。
        
        <table>
        <thead>
        <tr class="header">
        <th><img src="images/Gg398094.tip(OCS.15).gif" title="tip" alt="tip" />提示：</th>
        </tr>
        </thead>
        <tbody>
        <tr class="odd">
        <td>要向查询中添加附加搜索子句，请单击“添加筛选器”。</td>
        </tr>
        </tbody>
        </table>
    
    5.  单击“查找”。

6.  在搜索结果中单击某个用户，再单击“操作”，然后单击“分配策略”。
    
    <table>
    <thead>
    <tr class="header">
    <th><img src="images/Gg398094.tip(OCS.15).gif" title="tip" alt="tip" />提示：</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>如果您希望将相同的每用户存档策略应用于多个用户，请在搜索结果中选择多个用户，单击“操作”，然后单击“分配策略”。</td>
    </tr>
    </tbody>
    </table>


7.  在“分配策略”中的“存档策略”下，执行下列操作之一：
    
    > [!NOTE]  
    > 有多个策略可以使用“分配策略”对话框进行配置，因此默认情况下，将为对话框中的每个策略选中“&lt;保留原样&gt;”。如果不对此设置进行任何更改，则将继续使用先前分配给用户的策略。
    
    
      - 允许 Lync Server 2013 自动选择全局级别的策略或站点级别的策略（如果已定义）。
    
      - 单击之前在“存档策略”页中定义的每用户存档策略的名称。
        
        <table>
        <thead>
        <tr class="header">
        <th><img src="images/Gg398094.tip(OCS.15).gif" title="tip" alt="tip" />提示：</th>
        </tr>
        </thead>
        <tbody>
        <tr class="odd">
        <td>为帮助您确定要分配的策略，请在单击策略名称后单击“查看”以查看策略中定义的用户权限。</td>
        </tr>
        </tbody>
        </table>


8.  完成后，单击“确定”。

## 使用 Windows PowerShell cmdlet 分配每用户存档策略

还可以使用 Windows PowerShell 和 **Grant-CsArchivingPolicy** cmdlet 分配每用户存档策略。此 cmdlet 可从 Lync Server 2013 命令行管理程序或从 Windows PowerShell 的远程会话中运行。有关使用远程 Windows PowerShell 连接到 Lync Server 的详细信息，请参阅 Lync Server Windows PowerShell 博客文章“快速入门：使用远程 PowerShell 管理 Microsoft Lync Server 2010”，网址为 [http://go.microsoft.com/fwlink/p/?linkId=255876](http://go.microsoft.com/fwlink/p/?linkid=255876)。

## 向单个用户分配每用户存档策略

  - 以下命令向用户 Ken Myer 分配每用户存档策略 RedmondArchivingPolicy。
    
        Grant-CsArchivingPolicy -Identity "Ken Myer" -PolicyName "RedmondArchivingPolicy"

## 向多个用户分配每用户存档策略

  - 此命令将每用户存档策略 RedmondArchivingPolicy 分配给具有驻留在注册器池 atl-cs-001.litwareinc.com 中的帐户的所有用户。有关此命令中使用的 Filter 参数的详细信息，请参阅 [Get-CsUser](https://docs.microsoft.com/en-us/powershell/module/skype/Get-CsUser) cmdlet 的文档。
    
        Get-CsUser -Filter {RegistrarPool -eq "atl-cs-001.litwareinc.com"} | Grant-CsArchivingPolicy -PolicyName "RedmondArchivingPolicy"

## 取消分配每用户存档策略

  - 以下命令取消之前已分配给 Ken Myer 的所有每用户存档策略的分配。在取消分配每用户策略后，将自动使用全局策略或本地站点策略（如果有）管理 Ken Myer。站点策略优先于全局策略。
    
        Grant-CsarchivingPolicy -Identity "Ken Myer" -PolicyName $Null

有关详细信息，请参阅 [Grant-CsArchivingPolicy](https://docs.microsoft.com/en-us/powershell/module/skype/Grant-CsArchivingPolicy) cmdlet 的帮助主题。

## 另请参阅

#### 任务

[创建存档策略以启用或禁用特定站点或用户的内部或外部通信的存档](lync-server-2013-creating-an-archiving-policy-to-enable-or-disable-archiving-of-internal-or-external-communications-for-specific-sites-or-users.md)  

#### 其他资源

[分配每用户策略](lync-server-2013-assigning-per-user-policies.md)

