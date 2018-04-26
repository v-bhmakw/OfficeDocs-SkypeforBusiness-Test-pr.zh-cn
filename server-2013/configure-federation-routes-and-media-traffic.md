﻿---
title: 配置联合路由和媒体流量
TOCTitle: 配置联合路由和媒体流量
ms:assetid: 8b2f5f81-a955-4ad1-ad74-397322ff9521
ms:mtpsurl: https://technet.microsoft.com/zh-cn/library/JJ688121(v=OCS.15)
ms:contentKeyID: 49888498
ms.date: 05/19/2016
mtps_version: v=OCS.15
ms.translationtype: HT
---

# 配置联合路由和媒体流量

 

_**上一次修改主题：** 2012-10-15_

联盟是两个或更多 SIP 域之间的一种信任关系，它允许不同组织中的用户跨越网络边界进行通信。迁移到 Lync Server 2013 试点池后，需要从 Lync Server 2010 边缘服务器的联盟路由转换到 Lync Server 2013 边缘服务器的联盟路由。

使用下列过程可将单站点部署的联盟路由和媒体流量路由从 Lync Server 2010 边缘服务器和控制器转换到 Lync Server 2013 边缘服务器。

<table>
<thead>
<tr class="header">
<th><img src="images/Gg398794.important(OCS.15).gif" title="important" alt="important" />重要提示：</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>更改联盟路由和媒体流量需要安排 Lync Server 2013 和 Lync Server 2010 边缘服务器的维护停机时间。整个转换过程还意味着服务中断期间无法进行联盟访问。应将停机时间安排在预计用户活动最少的时间段内。还应向最终用户提供包含丰富信息的通知。相应地规划这次服务中断并正确设置组织的期望。</td>
</tr>
</tbody>
</table>


<table>
<thead>
<tr class="header">
<th><img src="images/Gg398794.important(OCS.15).gif" title="important" alt="important" />重要提示：</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>如果旧 Lync Server 2010 边缘服务器配置为对访问边缘服务、Web 会议边缘服务和 A/V 边缘服务使用相同的 FQDN，则本节中的过程不受支持。如果旧边缘服务配置为使用相同的 FQDN，则必须首先将所有用户从 Lync Server 2010 迁移到 Lync Server 2013，接着停用 Lync Server 2010 边缘服务器，然后在 Lync Server 2013 边缘服务器上启用联盟。</td>
</tr>
</tbody>
</table>


<table>
<thead>
<tr class="header">
<th><img src="images/Gg398794.important(OCS.15).gif" title="important" alt="important" />重要提示：</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>如果通过 Lync Server 2013 边缘服务器路由您的 XMPP 联盟，则旧 Lync Server 2010 用户将无法与 XMPP 联盟伙伴进行通信，直到将所有用户移动到 Lync Server 2013，已配置 XMPP 策略和证书，已在 Lync Server 2013 上配置 XMPP 联盟伙伴，及已更新最后的 DNS 条目为止。</td>
</tr>
</tbody>
</table>


## 从 Lync Server 2013 站点中删除旧联盟关联

1.  在 Lync Server 2013 前端服务器上，打开拓扑生成器中的现有拓扑。

2.  在左侧窗格中，导航到位于 **Lync Server** 正下方的站点节点。

3.  右键单击该站点，然后单击“编辑属性”。

4.  在左侧窗格中，选择“联盟路由”。

5.  在“站点联盟路由分配”下，清除“启用 SIP 联盟”复选框以禁止联盟路由通过旧 Lync Server 2010 环境。
    
    ![“编辑属性”对话框 -“联盟路由”页](images/JJ688121.8d755ae0-fc7d-4253-b0db-0cf31b863c55(OCS.15).jpg "“编辑属性”对话框 -“联盟路由”页")

6.  单击“确定”以关闭“编辑属性”页。

7.  从“拓扑生成器”中，选择顶层节点“Lync Server”。

8.  从“操作”菜单上，单击“发布拓扑”。

9.  单击“下一步”以完成发布过程，并在完成发布过程后单击“完成”。

## 将旧边缘服务器配置为非联盟边缘服务器

1.  在左窗格中，导航到“Lync Server 2010”节点，然后导航到“边缘池”节点。

2.  右键单击该边缘服务器，然后单击“编辑属性”。

3.  在左窗格中，选择“常规”。

4.  清除“为此边缘池启用联盟(端口 5061)”复选框条目，并选择“确定”关闭页面。
    
    ![编辑属性 - 常规 - 取消选中“启用联盟”](images/JJ688121.3be2c8c0-9ed9-4544-bafd-b7694271fafc(OCS.15).jpg "编辑属性 - 常规 - 取消选中“启用联盟”")

5.  从“操作”菜单中，选择“发布部署”，然后单击“下一步”。

6.  “发布向导”完成时，单击“完成”关闭向导。

7.  确认已禁用旧边缘服务器的联盟。
    
    ![拓扑生成器 - 边缘池 - 禁用联盟](images/JJ688121.a2948438-d51a-4aeb-9eaa-d899ca950758(OCS.15).jpg "拓扑生成器 - 边缘池 - 禁用联盟")

## 在 Lync Server 2010 边缘服务器上配置证书

1.  从旧 Lync Server 2010 边缘服务器导出外部访问代理证书及私钥。

2.  在 Lync Server 2013 边缘服务器上，导入上一步骤中的访问代理外部证书。

3.  将访问代理外部证书分配给边缘服务器的 Lync Server 2013 外部接口。

4.  应从受信任的 CA 请求和分配 Lync Server 2013 边缘服务器的内部接口证书。

## 将 Lync Server 2010 联盟路由更改为使用 Lync Server 2013 边缘服务器

1.  从拓扑生成器中的左窗格中，依次导航到“Lync Server 2013”节点和“边缘池”节点。

2.  右键单击该边缘服务器，然后单击“编辑属性”。

3.  在左窗格中，选择“常规”。

4.  选中“为此边缘池启用联盟(端口 5061)”复选框条目，然后单击“确定”关闭页面。
    
    ![“编辑属性”对话框 -“常规”页](images/JJ688121.cc79a88c-cce4-4cab-80ad-4f70325dc7c4(OCS.15).jpg "“编辑属性”对话框 -“常规”页")

5.  从“操作”菜单中，选择“发布部署”，然后单击“下一步”。

6.  “发布向导”完成时，单击“完成”关闭向导。

7.  确认“联盟(端口 5061)”设置为“启用”。
    
    ![拓扑生成器 - 边缘池 - 启用联盟](images/JJ688121.e8ccdada-23f4-47e5-a99d-5bf795fefc48(OCS.15).jpg "拓扑生成器 - 边缘池 - 启用联盟")

## 更新 Lync Server 2013 边缘服务器联盟下一跃点

1.  从拓扑生成器中的左窗格中，依次导航到“Lync Server 2013”节点和“边缘池”节点。

2.  展开节点，右键单击列出的边缘服务器，然后单击“编辑属性”。

3.  在“常规”页上的“下一个跃点选择”下，从下拉列表中选择 Lync Server 2013 池。
    
    ![“编辑属性”对话框 - “下一个跃点”页](images/JJ688121.5741b9a8-e729-4457-9f62-38f08a2c5b02(OCS.15).jpg "“编辑属性”对话框 - “下一个跃点”页")

4.  单击“确定”以关闭“编辑属性”页。

5.  从“拓扑生成器”中，选择顶层节点“Lync Server”。

6.  从“操作”菜单中，单击“发布拓扑”并完成向导。

## 配置 Lync Server 2013 边缘服务器出站媒体路径

1.  从拓扑生成器中的左窗格中，导航到“Lync Server 2013”节点，然后导航到“Standard Edition 前端服务器”或“Enterprise Edition 前端池”下方的池。

2.  右键单击该池，然后单击“编辑属性”。

3.  在“关联”部分，选中“关联边缘池(用于媒体组件)”复选框。
    
    ![编辑属性 - 常规 - 关联边缘池](images/JJ688121.fd9b18ca-fda2-4764-9bf0-726bf39f6a12(OCS.15).jpg "编辑属性 - 常规 - 关联边缘池")

4.  从下拉框中，选择“Lync Server 2013 边缘服务器”。

5.  单击“确定”关闭“编辑属性”页。

## 启用 Lync Server 2013 边缘服务器联盟

1.  从拓扑生成器中的左窗格中，依次导航到“Lync Server 2013”节点和“边缘池”节点。

2.  展开节点，右键单击列出的 边缘服务器，然后单击“编辑属性”。
    
    <table>
    <thead>
    <tr class="header">
    <th><img src="images/Dn783119.note(OCS.15).gif" title="note" alt="note" />注意：</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>只能对单个 边缘池启用联盟。如果具有多个边缘池，请选择一个用作联盟 边缘池。</td>
    </tr>
    </tbody>
    </table>


3.  在“常规”页上，确认已选中“为此边缘池启用联盟(端口 5061)”设置。
    
    ![“编辑属性”对话框 -“常规”页](images/JJ688121.cc79a88c-cce4-4cab-80ad-4f70325dc7c4(OCS.15).jpg "“编辑属性”对话框 -“常规”页")

4.  单击“确定”以关闭“编辑属性”页。

5.  接下来，导航到站点节点。

6.  右键单击站点，然后单击“编辑属性”。

7.  在左窗格中，单击“联盟路由”。

8.  在“站点联盟路由分配”下，选择“启用 SIP 联盟”，然后从列表中选择列出的 Lync Server 2013  边缘服务器。
    
    ![编辑属性 -“联盟路由”页](images/JJ688121.c50c13b8-0859-4e3e-8793-45c431a5b4b5(OCS.15).jpg "编辑属性 -“联盟路由”页")

9.  单击“确定”关闭“编辑属性”页。
    
    对于多站点部署，在每个站点上完成该过程。

## 发布边缘服务器配置更改

1.  从“拓扑生成器”中，选择顶层节点“Lync Server”。

2.  从“操作”菜单中，选择“发布拓扑”并完成向导。

3.  稍待片刻以便在部署中的所有池间执行 Active Directory 复制。
    
    <table>
    <thead>
    <tr class="header">
    <th><img src="images/Dn783119.note(OCS.15).gif" title="note" alt="note" />注意：</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>您可能会看到以下消息：<br />
    <strong>警告: 拓扑包含多个联盟边缘服务器。在迁移到产品的更高版本时，可能会出现这种情况。在这种情况下，只有一个边缘服务器处于活动状态以用于联盟。请确保外部 DNS SRV 记录指向正确的边缘服务器。如果要将多个联盟边缘服务器部署为同时处于活动状态(非迁移情况下)，请确保所有联盟伙伴都在使用 Lync Server，并确保外部 DNS SRV 记录列出了所有已启用联盟的边缘服务器。</strong><br />
    该警告是预期警告，可以放心地忽略。</td>
    </tr>
    </tbody>
    </table>


## 配置 Lync Server 2013 边缘服务器

1.  使所有 Lync Server 2013 边缘服务器联机。

2.  更新外部防火墙路由规则或硬件负载平衡器设置以将外部访问（通常端口为 443）和联盟（通常端口为 5061）的 SIP 流量发送到 Lync Server 2013边缘服务器，而不是旧 边缘服务器。
    
    <table>
    <thead>
    <tr class="header">
    <th><img src="images/Dn783119.note(OCS.15).gif" title="note" alt="note" />注意：</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>如果您没有硬件负载平衡器，则需要更新联盟的 DNS A 记录，以将其解析为新的 Lync Server 访问边缘服务器。若要在中断最少的情况下完成此操作，请减小外部 Lync Server 访问边缘 FQDN 的 TLL 值，以便在更新 DNS 以指向新的 Lync Server 访问边缘时，将快速更新联盟和远程访问。</td>
    </tr>
    </tbody>
    </table>


3.  接下来，从每台 边缘服务器计算机停止“Lync Server 访问边缘”。

4.  在每台旧 边缘服务器计算机上，从“管理工具”中打开“服务”小程序。

5.  在服务列表中，找到“Lync Server 访问边缘”。

6.  右键单击服务名称，然后选择“停止”以停止该服务。

7.  将“启动”类型设置为“已禁用”。

8.  单击“确定”关闭“属性”窗口。
