﻿---
title: Lync Server 2013：为 HTTP 反向代理请求和配置证书
TOCTitle: 为 HTTP 反向代理请求和配置证书
ms:assetid: 4b70991e-5f10-40a3-b069-0b227c3a3a0a
ms:mtpsurl: https://technet.microsoft.com/zh-cn/library/Gg429704(v=OCS.15)
ms:contentKeyID: 49312774
ms.date: 12/10/2016
mtps_version: v=OCS.15
ms.translationtype: HT
---

# 在 Lync Server 2013 中为 HTTP 反向代理请求和配置证书

 

_**上一次修改主题：** 2016-12-08_

如果证书颁发机构 (CA) 基础结构向运行 Microsoft Lync Server 2013 的内部服务器颁发过服务器证书，则需要在运行 Microsoft Forefront Threat Management Gateway 2010 或 IIS ARR 的服务器上安装该 CA 基础结构的根 CA 证书。

还必须在反向代理服务器上安装公共 Web 服务器证书。此证书的使用者替代名称应包含启用远程访问的用户驻留的各个池的已发布外部完全限定的域名 (FQDN)，以及将在边缘基础结构内使用的所有控制器或控制器池的外部 FQDN。使用者替代名称还必须包含会议简单 URL、电话拨入式简单 URL 和外部自动发现服务 URL（如果您正在部署移动应用程序并计划使用自动发现功能），如下表所示。


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th></th>
<th>值</th>
<th>示例</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>使用者名称</p></td>
<td><p>池 FQDN</p></td>
<td><p>webext.contoso.com</p></td>
</tr>
<tr class="even">
<td><p>使用者替代名称</p></td>
<td><p>池 FQDN</p></td>
<td><p>webext.contoso.com</p>
<div class="alert">
<table>
<thead>
<tr class="header">
<th><img src="images/Gg398794.important(OCS.15).gif" title="important" alt="important" />重要提示：</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>使用者名称还必须存在于使用者替代名称中。</td>
</tr>
</tbody>
</table>

</div></td>
</tr>
<tr class="odd">
<td><p>使用者替代名称</p></td>
<td><p>可选 控制器 Web 服务（如果部署了 控制器）</p></td>
<td><p>webdirext.contoso.com</p></td>
</tr>
<tr class="even">
<td><p>使用者替代名称</p></td>
<td><p>会议简单 URL</p>
<div class="alert">
> [!NOTE]  
> 所有会议简单 URL 都必须位于使用者替代名称中。每个 SIP 域必须至少有一个活动会议简单 URL。


</div></td>
<td><p>meet.contoso.com</p></td>
</tr>
<tr class="odd">
<td><p>使用者替代名称</p></td>
<td><p>拨入简单 URL</p></td>
<td><p>dialin.contoso.com</p></td>
</tr>
<tr class="even">
<td><p>使用者替代名称</p></td>
<td><p>Office Web Apps 服务器</p></td>
<td><p>officewebapps01.contoso.com</p></td>
</tr>
<tr class="odd">
<td><p>使用者替代名称</p></td>
<td><p>外部自动发现服务 URL</p></td>
<td><p>lyncdiscover.contoso.com</p>
<div class="alert">
> [!NOTE]  
> 如果您在使用 Microsoft Exchange Server，则也需要为 Exchange 自动发现和 Web 服务 URL 配置反向代理规则。


</div></td>
</tr>
</tbody>
</table>


> [!NOTE]  
> 如果内部部署包含多个 Standard Edition Server 或前端池，则必须为每个外部 Web 场 FQDN 配置 Web 发布规则，并且还需为其配置证书和 Web 侦听器，或者获取使用者替代名称包含所有池使用的名称的证书，将其分配给 Web 侦听器，并在多个 Web 发布规则中共享。



## 创建证书请求

请在反向代理上创建证书请求。您可以在另一台计算机上创建请求，但是在从公共证书颁发机构收到证书之后，您必须导出证书及私钥，并将其导入到反向代理中。

> [!NOTE]  
> 证书请求或证书签名请求 (CSR) 是指向受信任的公共证书颁发机构 (CA) 发出的请求以便验证请求计算机的公钥并签名。当生成证书时，将创建一个公钥和一个私钥。仅公钥会进行共享和签名。顾名思义，公钥对任何公共请求可用。公钥供需要安全地交换信息并验证计算机的身份的客户端、服务器和其他请求使用。私钥受到安全保护，仅由创建密钥对以解密通过公钥进行加密的消息的计算机使用。私钥可以用于其他用途。对于反向代理用途，主要用途是数据译码。其次，另一用途是证书密钥级别的证书身份验证，仅限于验证请求者是否拥有计算机的公钥或者您用于其公钥的计算机是否是所声称的计算机。



<table>
<thead>
<tr class="header">
<th><img src="images/Gg398094.tip(OCS.15).gif" title="tip" alt="tip" />提示：</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>如果您同时规划 边缘服务器 证书和反向代理证书，则应该会注意到，两个证书的诸多要求非常相似。当您配置并请求 边缘服务器 证书时，请组合使用 边缘服务器 和反向代理使用者替代名称。如果您导出证书和私钥并将导出的文件复制到反向代理，然后导入证书/密钥对，并在将来的过程中根据需要进行分配，那么您可以对反向代理使用相同的证书。请参阅边缘服务器<a href="lync-server-2013-plan-for-edge-server-certificates.md">在 Lync Server 2013 中规划边缘服务器证书</a>和反向代理<a href="lync-server-2013-certificate-summary-reverse-proxy.md">Lync Server 2013 中的证书摘要 - 反向代理</a>的证书要求。 确保使用可导入的私钥创建证书。对于池化的边缘服务器，需要使用可导入的私钥创建证书和证书请求，因此这是正常做法，边缘服务器 的 Lync Server 部署向导 中的证书向导将允许您设置“使私钥可以导出”标志。从公共证书颁发机构收到证书请求之后，您将导出证书和私钥。有关如何创建和导出证书及私钥的详细信息，请参阅主题<a href="lync-server-2013-set-up-certificates-for-the-external-edge-interface.md">为 Lync Server 2013 的外部边缘接口设置证书</a>中的部分“为池中的边缘服务器导出包含私钥的证书”。证书扩展名的类型应为 <strong>.pfx</strong>。</td>
</tr>
</tbody>
</table>


要在将分配证书和私钥的计算机上生成证书签名请求，请执行以下操作：

**创建证书签名请求**

1.  打开 Microsoft 管理控制台 (MMC) 并添加“证书”管理单元，选择“计算机”，然后展开“个人”。有关如何在 Microsoft 管理控制台 (MMC) 中创建证书控制台的详细信息，请参阅 [http://go.microsoft.com/fwlink/?LinkId=282616](http://go.microsoft.com/fwlink/?linkid=282616)。

2.  右键单击“证书”，单击“所有任务”，单击“高级操作”，然后单击“创建自定义请求”。

3.  在“证书注册”页面上，单击“下一步”。

4.  在“选择证书注册策略”页面上，在“自定义请求”下选择“不使用注册策略继续”。单击“下一步”。

5.  在“自定义请求”页面上，对于“模板”，请选择“（无模板）旧密钥”。除非证书提供商另有指示，否则请取消选中“取消默认扩展”，并在“请求格式”上选择“PKCS \#10”。单击“下一步”。

6.  在“证书信息”页面上，单击“详细信息”，然后单击“属性”。

7.  在“证书属性”页面上，在“常规”选项卡上的“友好名称”字段中，键入此证书的名称。或者，在“说明”字段中键入说明。友好名称和说明通常由管理员用于识别证书的用途，例如“Lync Server 的反向代理侦听器”。

8.  选择“使用者”选项卡。在“使用者名称”下，对于“类型”，对于使用者名称类型请选择“公用名”。对于“值”，请键入将用于反向代理的使用者名称，然后单击“添加”。在本主题的表中提供的示例中，使用者名称是 webext.contoso.com，将在“使用者名称”的“值”字段中键入。

9.  在“使用者”选项卡上，在“备用名称”下方，从“类型”的下拉列表中选择“DNS”。对于定义的、证书上需要的每个使用者替代名称，请键入完全限定的域名，然后单击“添加”。例如，表中有三个使用者替代名称：meet.contoso.com、dialin.contoso.com 和 lyncdiscover.contoso.com。在“值”字段中，键入 meet.contoso.com，然后单击“添加”。对您需要定义的每个使用者替代名称重复。

10. 在“证书属性”页面上，单击“扩展”选项卡。在此页面上，您将在“密钥用法”中定义加密密钥用途，而在“扩展的密钥用法（应用程序策略）”中定义扩展密钥用法。

11. 单击“密钥用法”箭头以显示“可用选项”。在“可用选项”下方，单击“数字签名”，然后单击“添加”。单击“密钥加密”，然后单击“添加”。如果“将这些密钥用法标成关键”的复选框取消选中，请选中该复选框。

12. 单击“扩展的密钥用法（应用程序策略）”箭头以显示“可用选项”。在“可用选项”下方，单击“服务器身份验证”，然后单击“添加”。单击“客户端身份验证”，然后单击“添加”。如果“将扩展密钥用法标成关键”的复选框选中，请取消选中该复选框。与“密钥用法”复选框（必须选中）不同，您必须确保“扩展密钥用法”复选框未选中。

13. 在“证书属性”页面上，单击“私钥”选项卡。单击“密钥选项”箭头。对于“密钥大小”，请从下拉列表中选择“2048”。如果您正在与此证书面向的反向代理不同的计算机上生成此密钥对和 CSR，请选择“使私钥可以导出”。
    
    <table>
    <thead>
    <tr class="header">
    <th><img src="images/Gg399038.security(OCS.15).gif" title="security" alt="security" />安全性 注意：</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>当您在场中拥有多个反向代理时，通常建议选择“使私钥可以导出”，因为您会将证书和私钥复制到场中的每台计算机。如果您允许使用可导出的私钥，则必须额外注意证书和在上面生成证书的计算机。如果私钥受到破坏，则将指明证书无用，并且可能会将计算机暴露给外部访问和其他安全漏洞。</td>
    </tr>
    </tbody>
    </table>


14. 在“私钥”选项卡上，单击“密钥类型”箭头。选择“交换”选项卡。

15. 单击“确定”以保存您设置的“证书属性”。

16. 在“证书注册”页面上，单击“下一步”。

17. 在“你想将脱机请求保存到何处？”页面上，系统将提示您输入“文件名”和保存证书签名请求的“文件格式”。

18. 在“文件名”输入字段中，键入请求的路径和文件名，或者单击“浏览”以选择文件的位置并键入请求的文件名。

19. 对于“文件格式”，单击“Base 64”或“二进制”。除非供应商另有指示，否则对于您的证书请选择“Base 64”。

20. 找到您在上一个步骤中保存的请求文件。向公共证书颁发机构提交。
    
    <table>
    <thead>
    <tr class="header">
    <th><img src="images/Gg398794.important(OCS.15).gif" title="important" alt="important" />重要提示：</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>Microsoft 已识别了符合统一通信用途要求的公共 CA。以下知识库文件中维护了一个列表。<a href="http://go.microsoft.com/fwlink/?linkid=282625">http://go.microsoft.com/fwlink/?LinkId=282625</a></td>
    </tr>
    </tbody>
    </table>

