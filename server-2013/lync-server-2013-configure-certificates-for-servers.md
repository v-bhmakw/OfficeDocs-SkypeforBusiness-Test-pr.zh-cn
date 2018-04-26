﻿---
title: Lync Server 2013：为服务器配置证书
TOCTitle: 为服务器配置证书
ms:assetid: e12e59b5-a146-4859-86ec-cabfc198c7b5
ms:mtpsurl: https://technet.microsoft.com/zh-cn/library/Gg398995(v=OCS.15)
ms:contentKeyID: 49314523
ms.date: 12/10/2016
mtps_version: v=OCS.15
ms.translationtype: HT
---

# 在 Lync Server 2013 中为服务器配置证书

 

_**上一次修改主题：** 2016-12-08_

要成功完成此过程，应以 RTCUniversalServerAdmins 组成员或委派了相应权限的用户身份登录。有关委派权限的详细信息，请参阅 [Lync Server 2013 中的委派安装权限](lync-server-2013-delegate-setup-permissions.md)。根据组织和请求证书的要求，可能还需要其他组成员身份。请咨询管理公钥基础结构 (PKI) 证书颁发机构 (CA) 的组。

<table>
<thead>
<tr class="header">
<th><img src="images/Dn783119.note(OCS.15).gif" title="note" alt="note" />注意：</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Lync Server 2013 除了支持对运行 Lync Phone Edition 的客户端发出的连接使用 SHA-2 套件（SHA-2 使用摘要长度 224、256、384 或 512 位）以外，还支持对运行 Windows 7、Windows Server 2008 R2、Windows Server 2008、Windows Vista 或 Windows XP 操作系统的客户端发出的连接使用 SHA-2 套件。要支持使用 SHA-2 套件进行外部访问，外部证书必须由公共 CA 颁发，它也可以颁发具有相同位长度摘要的证书。</td>
</tr>
</tbody>
</table>


<table>
<thead>
<tr class="header">
<th><img src="images/JJ205186.Caution(OCS.15).gif" title="Caution" alt="Caution" />警告：</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>选择哪个哈希摘要和签名算法取决于将使用证书的客户端和服务器，客户端和服务器将与其通信的其他计算机和设备也必须知道如何使用证书中使用的算法。有关操作系统和一些客户端应用程序中支持的摘要长度的信息，请参阅 <a href="http://go.microsoft.com/fwlink/?linkid=287002">http://go.microsoft.com/fwlink/?LinkId=287002</a>。</td>
</tr>
</tbody>
</table>


每台 Standard Edition Server 或 前端服务器最多需要四个证书：oAuthTokenIssuer 证书、默认证书、Web 内部证书和 Web 外部证书。但是，也可以请求和分配单个具有相应使用者替代名称条目的默认证书以及 oAuthTokenIssuer 证书。有关证书要求的详细信息，请参阅 [Lync Server 2013 中内部服务器的证书要求](lync-server-2013-certificate-requirements-for-internal-servers.md)。有关请求、分配和安装 oAuthTokenIssuer 证书的详细信息，请参阅 [在 Lync Server 2013 中管理服务器到服务器身份验证 (Oauth) 和合作伙伴应用程序](lync-server-2013-managing-server-to-server-authentication-oauth-and-partner-applications.md)

使用以下过程请求、分配和安装 Standard Edition Server 或 前端服务器证书。对每台 前端服务器重复此过程。

<table>
<thead>
<tr class="header">
<th><img src="images/Gg398794.important(OCS.15).gif" title="important" alt="important" />重要提示：</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>以下过程说明如何配置由组织部署的内部企业 PKI 颁发的证书以及脱机请求处理。有关从公共 CA 获取证书的信息，请参阅规划文档中的 <a href="lync-server-2013-certificate-requirements-for-internal-servers.md">Lync Server 2013 中内部服务器的证书要求</a>。此外，此过程还说明如何在设置 前端服务器的过程中请求、分配和安装证书。如果提前请求证书（如该部署文档中的 <a href="lync-server-2013-request-certificates-in-advance-optional.md">为 Lync Server 2013 提前请求证书（可选）</a>一节所述），或者不使用组织中部署的内部企业 PKI 获取证书，则必须相应地修改此过程。</td>
</tr>
</tbody>
</table>


## 为前端服务器配置证书

1.  在 Lync Server 部署向导中，单击“步骤 3: 请求、安装或分配证书”旁边的“运行”。

2.  在“证书向导”页上，单击“请求”。

3.  在“证书请求”页上，单击“下一步”。

4.  在“延迟的请求或即时请求”页上，可通过单击“下一步”接受默认的“立即将请求发送给联机证书颁发机构”选项。如果选择此选项，具有自动联机注册的内部 CA 必须可用。如果选择了延迟请求的选项，系统将提示您输入用于保存证书请求文件的名称和位置。证书请求必须由组织内部的 CA 或公共 CA 提出和处理，然后您需要导入证书响应并将其分配给适当的证书角色。

5.  在“选择证书颁发机构(CA)”页上，选择“从在您的环境中检测到的列表中选择一个 CA”选项，然后从列表中选择一个已知（已在 Active Directory 域服务 中注册）的 CA。或者，选择“指定其他证书颁发机构”选项，在框中输入其他 CA 的名称，然后单击“下一步”。

6.  在“证书颁发机构帐户”页上，将提示您提供凭据，以在 CA 中请求证书和处理证书请求。您应该已经确定提前请求证书是否需要用户名和密码。CA 管理员将拥有所需的信息，并且可能需要协助您完成此步骤。如果需要提供备用凭据，请选中相应的复选框，在文本框中输入用户名和密码，然后单击“下一步”。

7.  在“指定替代证书模板”页上，要使用默认的 Web 服务器模板，请单击“下一步”。
    
    <table>
    <thead>
    <tr class="header">
    <th><img src="images/Dn783119.note(OCS.15).gif" title="note" alt="note" />注意：</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>如果组织已经创建了模板，以用作默认 Web 服务器 CA 模板的备用模板，请选中相应的复选框，然后输入备用模板的名称。您将需要 CA 管理员定义的模板名称。</td>
    </tr>
    </tbody>
    </table>


8.  在“名称和安全设置”页上，指定一个“友好名称”，该名称应能标识证书和目的。如果将此处留空，则系统会自动生成一个名称。设置密钥的“位长度”，或接受默认值 2048 位。如果您确定需要将证书和私钥移动或复制到其他系统中，请选择“将证书的私钥标记为可导出”，然后单击“下一步”。
    
    <table>
    <thead>
    <tr class="header">
    <th><img src="images/Dn783119.note(OCS.15).gif" title="note" alt="note" />注意：</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>Lync Server 2013 对可导出的私钥具有最低要求。其中一个可导出的位置是池中的边缘服务器，在这里媒体中继身份验证服务使用证书副本，而不是对池中的每个实例都分别使用单个证书。</td>
    </tr>
    </tbody>
    </table>


9.  在“组织信息”页上，可选择提供组织信息，然后单击“下一步”。

10. 在“地理信息”页上，可选择提供地理信息，然后单击“下一步”。

11. 在“使用者名称/使用者替代名称”页上，检查将添加的使用者替代名称，然后单击“下一步”。

12. 在“SIP 域设置”页上，选择“SIP 域”，然后单击“下一步”。

13. 在“配置其他使用者替代名称”页上，添加其他任何需要的使用者替代名称，包括将来其他 SIP 域可能需要的使用者替代名称，然后单击“下一步”。

14. 在“证书请求摘要”页上，检查摘要中的信息。如果信息正确，请单击“下一步”。如果需要更正或修改设置，请单击“上一步”返回相应的页面进行更正或修改。

15. 在“正在执行命令”页上，单击“下一步”。

16. 在“联机证书请求状态”页上，检查返回的信息。请注意，证书已发布并安装到本地证书存储中。如果报告证书已发布且已安装，但是无效，请确保 CA 根证书已安装在服务器的受信任根 CA 存储中。有关如何检索受信任根 CA 证书的信息，请参阅 CA 文档。如果需要查看检索的证书，请单击“查看证书详细信息”。默认情况下，将选中“向 Lync Server 证书使用分配此证书”复选框。如果要手动分配证书，请清除此复选框，然后单击“完成”。

17. 如果在上一页中清除了“向 Lync Server 证书使用分配此证书”复选框，将显示“证书分配”页。单击“下一步”。

18. 在“证书存储”页上，选择已请求的证书。如果要查看证书，请单击“查看证书详细信息”，然后单击“下一步”继续。
    
    <table>
    <thead>
    <tr class="header">
    <th><img src="images/Dn783119.note(OCS.15).gif" title="note" alt="note" />注意：</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>如果“联机证书请求状态”页报告证书存在问题（例如证书无效），查看实际证书可协助解决问题。可能导致证书无效的两个具体问题为：先前提到的受信任根 CA 证书缺失，与证书相关联的私钥缺失。有关如何解决这两个问题的信息，请参阅 CA 文档。</td>
    </tr>
    </tbody>
    </table>


19. 在“证书分配摘要”页上，检查显示的信息以确保此证书是应分配的证书，然后单击“下一步”。

20. 在“正在执行命令”页上，检查命令的输出。如果要检查分配过程或查看是否出现错误或警告，请单击“查看日志”。检查完成后，单击“完成”。

21. 在“证书向导”页上，确保证书的“状态”为“已分配”，然后单击“关闭”。

## 另请参阅

#### 其他资源

[Lync Server 2013 的证书基础结构要求](lync-server-2013-certificate-infrastructure-requirements.md)
