---
ms.TocTitle: Office 365 Management Activity API schema
title: Office 365 管理活动 API 架构
description: Office 365 管理活动 API 架构作为两层数据服务提供 - 常见架构和特定于产品的架构。
ms.ContentId: 1c2bf08c-4f3b-26c0-e1b2-90b190f641f5
ms.topic: reference (API)
ms.date: ''
localization_priority: Priority
ms.openlocfilehash: a8e8fdab103bcee6a5ea8de56dc91c45c1c20b43
ms.sourcegitcommit: 358bfe9553eabbe837fda1d73cd1d1a83bcb427e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/15/2019
ms.locfileid: "28014334"
---
# <a name="office-365-management-activity-api-schema"></a>Office 365 管理活动 API 架构
 
Office 365 管理活动 API 架构作为两层数据服务提供：

- **常见架构**。 用于访问核心 Office 365 审核概念（如 Record Type、Creation Time、User Type 和 Action），以及提供核心维度（如 User ID）、具体位置细节（如 Client IP address）和特定于产品的属性（如 Object ID）的接口。 它建立一致且统一的视图，以便用户使用适当参数在少数顶级视图中提取所有 Office 365 审核数据，并为所有数据源提供固定架构，从而极大地降低了学习成本。 常见架构源自于归每个产品团队（如 Exchange、SharePoint、Azure Active Directory、Yammer 和 OneDrive for Business）所有的产品数据。 Object ID 字段可由产品团队扩展，添加特定于产品的属性。
    
- **特定于产品的架构**。 基于常见架构，提供一组特定于产品的属性；例如，Sway 架构、SharePoint 架构、OneDrive for Business 架构以及 Exchange 管理员架构。
    
**对于你自身的情况应使用哪个层？**
一般情况下，如果数据在较高层中可用，则不要回到较低层。 换言之，如果可在特定于产品的架构中满足数据要求，则不需要返回到常见架构。 

## <a name="office-365-management-api-schemas"></a>Office 365 管理 API 架构

本文详细介绍了常见架构以及每个特定于产品的架构。 下表描述了可用的架构。 

|架构名称|说明|
|:-----|:-----|
|[常见架构](#common-schema)|用于提取 Record Type、User ID、Client IP、User Type 和 Action 以及核心维度，如用户属性（如 UserID）、位置属性（如 Client IP）和特定于产品的属性（如 Object ID）的视图。|
|[SharePoint 基本架构](#sharepoint-base-schema)|使用特定于所有 SharePoint 审核数据的属性扩展常见架构。|
|[SharePoint 文件操作](#sharepoint-file-operations)|使用特定于 SharePoint 中的文件访问和操作的属性扩展 SharePoint 基本架构。|
|[SharePoint 共享架构](#sharepoint-sharing-schema)|使用特定于文件共享的属性扩展 SharePoint 基本架构。|
|[SharePoint 架构](#sharepoint-schema)|使用特定于 SharePoint 但与文件访问和操作无关的属性扩展 SharePoint基本架构。|
|[项目架构](#project-schema)|使用特定于 Project 的属性扩展 SharePoint 基本架构。|
|[Exchange 管理员架构](#exchange-admin-schema)|使用特定于所有 Exchange 管理员审核数据的属性扩展常见架构。|
|[Exchange 邮箱架构](#exchange-mailbox-schema)|使用特定于所有 Exchange 邮箱审核数据的属性扩展常见架构。|
|[Azure Active Directory 基本架构](#azure-active-directory-base-schema)|使用特定于所有 Azure Active Directory 审核数据的属性扩展常见架构。|
|[Azure Active Directory 帐户登录架构](#azure-active-directory-account-logon-schema)|使用特定于所有 Azure Active Directory 登录事件的属性扩展 Azure Active Directory 基本架构。|
|[Azure Active Directory STS 登录架构](#azure-active-directory-sts-logon-schema)|使用特定于所有 Azure Active Directory STS 登录事件的属性扩展 Azure Active Directory 基本架构。|
|[Azure Active Directory 架构](#azure-active-directory-schema)|使用特定于所有 Azure Active Directory 审核数据的属性扩展常见架构。|
|[DLP 架构](#dlp-schema)|使用特定于数据丢失防护事件的属性扩展常见架构。|
|[安全与合规中心架构](#security-and-compliance-center-schema)|使用特定于所有安全与合规中心事件的属性扩展常见架构。|
|[安全与合规警报中心](#security-and-compliance-alerts-schema)|使用特定于所有 Office 365 安全与合规警报的属性扩展常见架构。|
|[Yammer 架构](#yammer-schema)|使用特定于所有 Yammer 事件的属性扩展常见架构。|
|[Sway 架构](#sway-schema)|使用特定于所有 Sway 事件的属性扩展常见架构。|
|[数据中心安全基本架构](#data-center-security-base-schema)|使用特定于所有数据中心安全审核数据的属性扩展常见架构。|
|[数据中心安全 Cmdlet 架构](#data-center-security-cmdlet-schema)|使用特定于所有数据中心安全 cmdlet 审核数据的属性扩展数据中心安全基本架构。|
|[Microsoft Teams 架构](#microsoft-teams-schema)|使用特定于所有 Microsoft Teams 事件的属性扩展常见架构。|
|[Microsoft Teams 加载项架构](#microsoft-teams-add-ons-schema)|使用特定于 Microsoft Teams 加载项的属性扩展 Microsoft Teams 架构。|
|[Microsoft Teams 设置架构](#microsoft-teams-settings-schema)|使用特定于 Microsoft Teams 设置更改事件的属性扩展 Microsoft Teams 架构。|
|[Office 365 高级威胁防护和威胁智能架构](#office-365-advanced-threat-protection-and-threat-intelligence-schema)|使用特定于 Office 365 高级威胁防护和威胁智能数据的属性扩展常见架构。|
|[Power BI 架构](#power-bi-schema)|使用特定于所有 Power BI 事件的属性扩展常见架构。|

## <a name="common-schema"></a>常见架构

**EntityType 名称**：AuditRecord

|参数|类型|强制？|说明|
|:-----|:-----|:-----|:-----|
|Id|Combination GUIDEdm.Guid|是|审核记录的唯一标识符。|
|RecordType|Self.[AuditLogRecordType](#auditlogrecordtype)|是|记录指示的操作类型。 有关审核日志记录类型的详细信息表，请参阅 [AuditLogRecordType](#auditlogrecordtype)。|
|CreationTime|Edm.Date|是|用户执行活动时的协调世界时 (UTC) 日期和时间。|
|Operation|Edm.String|是|用户或管理员活动的名称。 有关最常见操作/活动的说明，请参阅[在 Office 365 保护中心搜索审核日志](http://go.microsoft.com/fwlink/p/?LinkId=708432)。 对于 Exchange 管理员活动，此属性标识已运行的 cmdlet 名称。 对于 DLp 事件，这可以是“DLP 架构”下描述的“DlpRuleMatch”、“DlpRuleUndo”或“DlpInfo”。|
|OrganizationId|Edm.Guid|是|组织 Office 365 租户的 GUID。 对于组织而言，该值始终相同，而不管它是在哪个 Office 365 服务中出现。|
|UserType|Self.[UserType](#user-type)|是|执行操作的用户类型。 有关用户类型的详细信息，请参阅 [UserType](#user-type) 表。|
|UserKey|Edm.String|是|UserID 属性中标识的用户的备选 ID。 例如，此属性使用 passport 唯一 ID (PUID) 填充，用于 SharePoint、OneDrive for Business 和 Exchange 中用户执行的事件。 此属性还可以为系统帐户执行的其他服务和事件中发生的事件指定与 UserID 属性相同的值。|
|Workload|Edm.String|否|Office 365 服务，其中活动发生在 Workload 字符串中。 此属性的可能值为：<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>Exchange</p></li><li><p>SharePoint</p></li><li><p>OneDrive</p></li><li><p>Azure Active Directory</p></li><li><p>SecurityComplianceCenter</p></li><li><p>Sway</p></li><li><p>ThreatIntelligence</p></li></ul>|
|ResultStatus|Edm.String|否|指示操作（在 Operation 属性中指定）成功还是失败。 可能的值为：**Succeeded**、**PartiallySucceeded** 或 **Failed**。 对于 Exchange 管理员活动，值为 **True** 或 **False**。|
|ObjectId|Edm.string|否|对于 SharePoint 和 OneDrive for Business 活动，用户访问的文件或文件夹的完整路径名称。 对于 Exchange 管理员审核日志，通过 cmdlet 修改的对象的名称。|
|UserID|Edm.string|是|执行导致记录被记录的操作（在 Operation 属性中指定）的用户的 UPN（用户主体名称）；例如 `my_name@my_domain_name`。 注意，系统帐户执行的活动记录（例如 SHAREPOINT\system 或 NT AUTHORITY\SYSTEM）也包括在内。|
|ClientIP|Edm.String|是|记录活动时使用的设备的 IP 地址。 IP 地址显示为 IPv4 或 IPv6 地址格式。|
|Scope|Self.[AuditLogScope](#auditlogscope)|否|此事件是由托管的 O365 服务还是本地服务器创建的？ 可能的值为 **online** 和 **onprem**。 请注意，SharePoint 是当前将事件从本地发送到 O365 的唯一工作负载。|

### <a name="enum-auditlogrecordtype---type-edmint32"></a>枚举：AuditLogRecordType - 类型：Edm.Int32

#### <a name="auditlogrecordtype"></a>AuditLogRecordType

|值|成员名称|说明|
|:-----|:-----|:-----|
|1|ExchangeAdmin|来自 Exchange 管理员审核日志的事件。|
|2|ExchangeItem|来自 Exchange 邮箱审核日志的事件，用于对单个项执行的操作，例如创建或接收电子邮件。|
|3|ExchangeItemGroup|来自 Exchange 邮箱审核日志的事件，用于可对多个项执行的操作，例如移动或删除一个或多个电子邮件。|
|4|SharePoint|SharePoint 事件。|
|6|SharePointFileOperation|SharePoint 文件操作事件。|
|8|AzureActiveDirectory|Azure Active Directory 事件。|
|9|AzureActiveDirectoryAccountLogon|Azure Active Directory OrgId 徽标事件（弃用）。|
|10|DataCenterSecurityCmdlet|数据中心安全 cmdlet 事件。|
|11|ComplianceDLPSharePoint|SharePoint 和 OneDrive for Business 中的数据丢失保护 (DLP) 事件。|
|12|Sway|来自 Sway 服务和客户端的事件。|
|13|ComplianceDLPExchange|通过统一 DLP 策略配置时，Exchange 中的数据丢失保护 (DLP) 事件。 不支持基于 Exchange 传输规则的 DLP 事件。|
|14|SharePointSharingOperation|SharePoint 共享事件。|
|15|AzureActiveDirectoryStsLogon|Azure Active Directory 中安全令牌服务 (STS) 登录事件。|
|18|SecurityComplianceCenterEOPCmdlet|来自安全与合规中心的 Admin 操作。|
|20|PowerBIAudit|Power BI 事件。|
|21|CRM|Microsoft CRM 事件。|
|22|Yammer|Yammer 事件。|
|23|SkypeForBusinessCmdlets|Skype for Business 事件。|
|24|Discovery|通过在安全与合规中心中运行内容搜索和管理电子数据展示案例执行的电子数据展示活动事件。|
|25|MicrosoftTeams|Microsoft Teams 中的事件。|
|26|MicrosoftTeamsAddOns|Microsoft Teams 加载项中的事件。|
|27|MicrosoftTeamsSettingsOperation|Microsoft Teams 中的设置更改。|
|28|ThreatIntelligence|Exchange Online Protection 和 Office 365 高级威胁防护中的网络钓鱼和恶意软件事件。|
|30|MicrosoftFlow|Microsoft Flow 事件。|
|32|MicrosoftStream|Microsoft Stream 事件。|
|35|Project|Microsoft Project 事件。|
|36|SharepointListOperation|Sharepoint List 事件。|
|40|SecurityComplianceAlerts|安全与合规警报信号。|
|41|ThreatIntelligenceUrl|Office 365 高级威胁防护中的安全链接信息块时间和信息块覆盖事件。|
|47|ThreatIntelligenceAtpContent|在 Office 365 高级威胁防护中，SharePoint、OneDrive for Business 和 Microsoft Teams 中的文件的网络钓鱼和恶意软件事件。|

### <a name="enum-user-type---type-edmint32"></a>枚举：User Type - 类型：Edm.Int32

#### <a name="user-type"></a>User Type

|值|成员名称|说明|
|:-----|:-----|:-----|
|0|Regular|常规用户。|
|1|Reserved|保留的用户。|
|2|Admin|管理员。|
|3|DcAdmin|Microsoft 数据中心操作员。|
|4|System|系统帐户。|
|5|Application|应用程序。|
|6|ServicePrincipal|服务主体。|

> [!NOTE] 
> 仅 Exchange 操作包括用户类型。 SharePoint 操作不指定用户类型。 

### <a name="enum-auditlogscope---type-edmint32"></a>枚举：AuditLogScope - 类型：Edm.Int32

#### <a name="auditlogscope"></a>AuditLogScope

|值|成员名称|说明|
|:-----|:-----|:-----|
|0|Online|此事件由托管的 O365 服务创建。|
|1|Onprem|此事件由本地服务器创建。|


## <a name="sharepoint-base-schema"></a>SharePoint 基本架构

|**参数**|**类型**|**强制？**|**说明**|
|:-----|:-----|:-----|:-----|
|Site|Edm.Guid|否|用户访问的文件或文件夹所在网站的 GUID。|
|ItemType|Edm.String String="Microsoft.Office.Audit.Schema.SharePoint.[ItemType](#itemtype)"|否|访问或修改的对象类型。 有关对象类型的详细信息，请参阅 [ItemType](#itemtype) 表。|
|EventSource|Edm.String String="Microsoft.Office.Audit.Schema.SharePoint.[EventSource](#eventsource)"|否|识别在 SharePoint 中发生的事件。 可能的值为 **SharePoint** 或 **ObjectModel**。|
|SourceName|Edm.String|否|触发已审核操作的实体。 可能的值为 SharePoint 或 **ObjectModel**。|
|UserAgent|Edm.String|否|有关用户客户端或浏览器的信息。 此信息由客户端或浏览器提供。|
|MachineDomainInfo|Edm.String Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true"|否|有关设备同步操作的信息。 只有在请求中存在该信息时才会报告该信息。|
|MachineId|Edm.String Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true"|否|有关设备同步操作的信息。 只有在请求中存在该信息时才会报告该信息。|


### <a name="enum-itemtype---type-edmint32"></a>枚举：ItemType - 类型：Edm.Int32

#### <a name="itemtype"></a>ItemType

|**值**|**成员名称**|**说明**|
|:-----|:-----|:-----|
|0|Invalid|项目不是其他项目类型（在此表中列出）。|
|1|File|项目为文件。|
|5|Folder|项目为文件夹。|
|6|Web|项目为 Web。|
|7|Site|项目为网站。|
|8|Tenant|项目为租户。|
|9|DocumentLibrary|项目为文档库。|
|11|Page|项目为页面。|

### <a name="enum-eventsource---type-edmint32"></a>枚举：EventSource - 类型：Edm.Int32

#### <a name="eventsource"></a>EventSource

|**值**|**成员名称**|**说明**|
|:-----|:-----|:-----|
|0|SharePoint|事件源是 SharePoint。|
|1|ObjectModel|事件源是 ObjectModel。|


### <a name="enum-sharepointauditoperation---type-edmint32"></a>枚举：SharePointAuditOperation - 类型：Edm.Int32

|**成员名称**|**说明**|
|:-----|:-----|
|AccessInvitationAccepted*|邀请查看或编辑共享文件（或文件夹）的收件人已通过单击邀请中的链接访问共享文件。|
|AccessInvitationCreated*|用户向其他人发出邀请（在其组织内部或外部）来查看或编辑 SharePoint 或 OneDrive for Business 网站上的共享文件或文件夹。 事件条目的详细信息标识共享文件的名称、接收邀请的用户以及发送邀请的人员所选择的共享权限的类型。|
|AccessInvitationExpired*|向外部用户发送的邀请过期。 默认情况下，如果在 7 天内未接受邀请，那么发送给组织外部用户的邀请将过期。|
|AccessInvitationRevoked*|SharePoint 或 OneDrive for Business 中的网站管理员或网站或文档所有者撤回发送给组织外部用户的邀请。 邀请只有在被接受之前才能撤回。|
|AccessInvitationUpdated*|在 SharePoint 或 OneDrive for Business 网站上创建并向其他人发送邀请以查看或编辑共享文件（或文件夹）的用户可以重新发送邀请。|
|AccessRequestApproved|SharePoint 或 OneDrive for Business 中的网站管理员或网站或文档所有者批准用户访问网站或文档的请求。|
|AccessRequestCreated|用户请求访问 SharePoint 或 OneDrive for Business 中无权限访问的网站或文档。 |
|AccessRequestRejected*|SharePoint 中的网站管理员或网站或文档所有者拒绝用户访问网站或文档的请求。|
|ActivationEnabled*|用户可以对以下表单模板启用浏览器功能：不包含表单代码、要求完全信任、启用移动设备上的呈现功能或使用由服务器管理员管理的数据连接。|
|AdministratorAddedToTermStore*|添加术语库管理员。|
|AdministratorDeletedFromTermStore*|删除术语库管理员。|
|AllowGroupCreationSet*|网站管理员或所有者向 SharePoint 或 OneDrive for Business 网站添加权限级别，允许分配该权限的用户为该网站创建组。|
|AppCatalogCreated*|创建的应用程序目录为 SharePoint 环境提供自定义业务应用。|
|AuditPolicyRemoved*|文档生命周期策略已在网站集中删除。|
|AuditPolicyUpdate*|文档生命周期策略已在网站集中更新。|
|AzureStreamingEnabledSet*|视频门户所有者允许来自 Azure 的视频流。|
|CollaborationTypeModified*|网站（例如 intranet、extranet 或公共网站）上允许的协作类型已被修改。|
|ConnectedSiteSettingModified|用户创建、修改或删除项目与项目网站之间的链接，或者修改 Project Web App 中链接的同步设置。|
|CreateSSOApplication*|在 Secure Store Service 中创建的目标应用程序。|
|CustomFieldOrLookupTableCreated|用户在 Project Web App 中创建自定义字段或查找表/项。|
|CustomFieldOrLookupTableDeleted|用户在 Project Web App 中删除自定义字段或查找表/项。|
|CustomFieldOrLookupTableModified|用户在 Project Web App 中修改自定义字段或查找表/项。|
|CustomizeExemptUsers*|全局管理员自定义 SharePoint 管理中心的豁免用户代理列表。 可以指定哪些用户代理可以免于接收用于编制索引的整个 Web页面。 这意味着，当指定为豁免的用户代理遇到 InfoPath 表单时，表单将作为 XML 文件而不是整个 Web 页面返回。 这可以加速对 InfoPath 表单编制索引。|
|DefaultLanguageChangedInTermStore*|术语库中的语言设置发生更改。|
|DelegateModified|用户创建或修改 Project Web App 中的安全代理。|
|DelegateRemoved|用户删除 Project Web App 中的安全代理。|
|DeleteSSOApplication*|删除 SSO 应用程序。|
|eDiscoveryHoldApplied*|就地保留置于内容源上。 通过使用 SharePoint 中的电子数据展示网站集（比如电子数据展示中心）来管理就地保存。|
|eDiscoveryHoldRemoved*|从内容源中删除就地保留。 通过使用 SharePoint 中的电子数据展示网站集（比如电子数据展示中心）来管理就地保存。|
|eDiscoverySearchPerformed*|在 SharePoint 中使用电子数据展示网站集执行电子数据展示搜索。|
|EngagementAccepted|用户接受 Project Web App 中的资源预订。|
|EngagementModified|用户修改 Project Web App 中的资源预订。|
|EngagementRejected|用户拒绝 Project Web App 中的资源预订。|
|EnterpriseCalendarModified|用户复制、修改或删除 Project Web App 中的企业日历。|
|EntityDeleted|用户删除 Project Web App 中的时间表。|
|EntityForceCheckedIn|用户在 Project Web App 中的日历、自定义字段或查找表上强制签入。|
|ExemptUserAgentSet*|全局管理员向 SharePoint 管理中心的豁免用户代理列表添加用户代理。|
|FileAccessed|用户或系统帐户访问 SharePoint 或 OneDrive for Business 网站上的文件。 系统帐户还可以生成 FileAccessed 事件。|
|FileCheckOutDiscarded*|用户放弃（或撤销）签出的文件。 这意味着他们在签出文件时对文件所做的任何更改都将被放弃，而不会保存到文档库中的文档版本中。|
|FileCheckedIn*|用户签入在 SharePoint 或 OneDrive for Business 文档库中签出的文档。|
|FileCheckedOut*|用户签出位于 SharePoint 或 OneDrive for Business 文档库的文档。 用户可以对与其共享的文档执行签出和更改操作。|
|FileCopied|用户从 SharePoint 或 OneDrive for Business 网站复制文档。 可以将复制的文件保存到网站上的另一个文件夹。|
|FileDeleted|用户从 SharePoint 或 OneDrive for Business 网站删除文档。|
|FileDeletedFirstStageRecycleBin|用户从 SharePoint 或 OneDrive for Business 网站上的回收站删除文件。|
|FileDeletedSecondStageRecycleBin|用户从 SharePoint 或 OneDrive for Business 网站上的第二阶段回收站删除文件。|
|FileDownloaded|用户从 SharePoint 或 OneDrive for Business 网站下载文档。|
|FileFetched|此事件已被 FileAccessed 事件取代，并且已弃用。|
|FileModified|用户或系统帐户修改位于 SharePoint 或 OneDrive for Business 网站上的文档内容或属性。|
|FileMoved|用户将文档从 SharePoint 或 OneDrive for Business 的当前位置移动到新位置。|
|FilePreviewed|用户在 SharePoint 或 OneDrive for Business 网站上预览文档。|
|FileRenamed|用户在 SharePoint 或 OneDrive for Business 网站上重命名文档。|
|FileRestored|用户从 SharePoint 或 OneDrive for Business 网站的回收站中恢复文档。 |
|FileSyncDownloadedFull|用户建立同步关系，并首次成功从 SharePoint 或 OneDrive Business 文档库将文件下载到他们的计算机。|
|FileSyncDownloadedPartial|用户成功下载对 SharePoint 或 OneDrive for Business 文档库中的文件所做的任何更改。 此事件表明对文档库中的文件所做的任何更改已下载到用户计算机。 仅下载了更改，因为用户先前已下载文档库（如 FileSyncDownloadedFull 事件所示）。|
|FileSyncUploadedFull*|用户建立同步关系，并首次成功从他们的计算机将文件上传到 SharePoint 或 OneDrive Business 文档库。|
|FileSyncUploadedPartial*|用户成功上传对 SharePoint 或 OneDrive for Business 文档库上的文件所做的更改。 此事件表明对文档库文件的本地版本所做的任何更改都已成功上传到文档库。 仅上传更改，因为用户先前已上传这些文件（如 FileSyncUploadedFull 事件所示）。|
|FileUploaded|用户将文档上传到 SharePoint 或 OneDrive for Business 网站上的文件夹。 |
|FileViewed|此事件已被 FileAccessed 事件取代，并且已弃用。|
|FolderCopied|用户将文件夹从 SharePoint 或 OneDrive for Business 网站复制到 SharePoint 或 OneDrive for Business 的其他位置。|
|FolderCreated|用户在 SharePoint 或 OneDrive for Business 网站上创建文件夹。|
|FolderDeleted|用户从 SharePoint 或 OneDrive for Business 网站删除文件夹。|
|FolderDeletedFirstStageRecycleBin|用户从 SharePoint 或 OneDrive for Business 网站上的回收站删除文件夹。|
|FolderDeletedSecondStageRecycleBin|用户从 SharePoint 或 OneDrive for Business 网站上的第二阶段回收站删除文件夹。|
|FolderModified|用户在 SharePoint 或 OneDrive for Business 网站上修改文件夹。 此事件包含文件夹元数据更改，如标签和属性。|
|FolderMoved|用户从 SharePoint 或 OneDrive for Business 网站移动文件夹。|
|FolderRenamed|用户在 SharePoint 或 OneDrive for Business 网站上重命名文件夹。|
|FolderRestored|用户从 SharePoint 或 OneDrive for Business 网站上的回收站恢复文件夹。|
|GroupAdded*|网站管理员或所有者为 SharePoint 或 OneDrive for Business 网站创建组，或者执行导致创建组的任务。 例如，当用户首次创建共享文件的链接时，系统组会被添加到用户的 OneDrive for Business 网站中。 此事件也可以是用户使用编辑权限创建共享文件链接的结果。|
|GroupRemoved*|用户从 SharePoint 或 OneDrive for Business 网站删除组。 |
|GroupUpdated*|网站管理员或所有者更改 SharePoint 或 OneDrive for Business 网站的组设置。 这可能包括更改组名、可以查看或编辑组成员身份的人员，以及成员身份请求的处理方式。|
|LanguageAddedToTermStore*|向术语库中添加语言。|
|LanguageRemovedFromTermStore*|从术语库中删除语言。|
|LegacyWorkflowEnabledSet*|网站管理员或所有者向网站添加 SharePoint Workflow Task 内容类型。 全局管理员还可以在 SharePoint 管理中心中对整个组织启用工作流。|
|LookAndFeelModified|用户修改快速启动、甘特图格式或组格式。 或者用户在 Project Web App 中创建、修改或删除视图。|
|ManagedSyncClientAllowed|用户成功建立与 SharePoint 或 OneDrive for Business 网站的同步关系。 同步关系之所以成功，是因为用户计算机是添加到域列表（称为“安全收件人列表”）的域成员，可以访问组织中的文档库。 有关此功能的详细信息，请参阅[使用 Windows PowerShell cmdlet 为安全收件人列表中的域启用 OneDrive 同步](http://go.microsoft.com/fwlink/p/?LinkID=534609)。|
|MaxQuotaModified*|修改了网站的最大限额。|
|MaxResourceUsageModified*|修改了网站所允许的最大资源使用量。|
|MySitePublicEnabledSet*|SharePoint 管理员设置了使用户拥有公共 MySites 的标志。|
|NewsFeedEnabledSet*|网站管理员或所有者启用 SharePoint 或 OneDrive for Business 网站的 RSS 源。 全局管理员可以在 SharePoint 管理中心中对整个组织启用 RSS 源。|
|ODBNextUXSettings*|已启用 OneDrive for Business 的新 UI。|
|OfficeOnDemandSet*|网站管理员启用 Office on Demand，允许用户访问最新版本的 Office 桌面应用程序。 SharePoint 管理中心启用了 Office on Demand，并需要包括全套已安装的 Office 应用程序的 Office 365 订阅。|
|PageViewed|用户在 SharePoint 网站或 OneDrive for Business 网站上查看页面。 这不包括在浏览器上从 SharePoint 网站或 One Drive for Business 网站查看文档库文件。|
|PeopleResultsScopeSet*|网站管理员创建或更改 SharePoint 网站人员搜索的结果来源。|
|PermissionSyncSettingModified|用户修改 Project Web App 中的项目权限同步设置。|
|PermissionTemplateModified|用户在 Project Web App 中创建、修改或删除权限模板。|
|PortfolioDataAccessed|用户访问 Project Web App 中的项目组合内容（驱动程序库、驱动程序优先顺序、项目组合分析）。|
|PortfolioDataModified|用户在 Project Web App 中创建、修改或删除项目组合数据（驱动程序库、驱动程序优先顺序、项目组合分析）。|
|PreviewModeEnabledSet*|网站管理员启用 SharePoint 网站的文档预览。|
|ProjectAccessed|用户访问 Project Web App 中的项目内容。|
|ProjectCheckedIn|用户签入他们从 Project Web App 中签出的项目。|
|ProjectCheckedOut|用户签出位于 Project Web App 的项目。 用户可以签出他们有权打开的项目并对其进行更改。|
|ProjectCreated|用户在 Project Web App 中创建项目。|
|ProjectDeleted|用户在 Project Web App 中删除项目。|
|ProjectForceCheckedIn|用户强制在 Project Web App 中签入项目。|
|ProjectModified|用户在 Project Web App 中修改项目。|
|ProjectPublished|用户在 Project Web App 中发布项目。|
|ProjectWorkflowRestarted|用户在 Project Web App 中重新启动工作流。|
|PWASettingsAccessed|用户通过 CSOM 访问 Project Web App 设置。|
|PWASettingsModified|用户修改 Project Web App 配置。|
|QueueJobStateModified|用户取消或重启 Project Web App 中的队列作业。|
|QuotaWarningEnabledModified*|修改了存储配额警告。|
|RenderingEnabled*|启用浏览器功能的表单模板将由 InfoPath Forms Services 呈现。|
|ReportingAccessed|用户访问 Project Web App 中的报告终结点。|
|ReportingSettingModified|用户修改 Project Web App 中的报告配置。|
|ResourceAccessed|用户访问 Project Web App 中的企业资源内容。|
|ResourceCheckedIn|用户签入他们从 Project Web App 中签出的企业资源。|
|ResourceCheckedOut|用户签出位于 Project Web App 的企业资源。|
|ResourceCreated|用户在 Project Web App 中创建企业资源。|
|ResourceDeleted|用户在 Project Web App 中删除企业资源。|
|ResourceForceCheckedIn|用户在 Project Web App 中强制签入企业资源。|
|ResourceModified|用户在 Project Web App 中修改企业资源。|
|ResourcePlanCheckedInOrOut|用户签入或签出 Project Web App 中的资源计划。|
|ResourcePlanModified|用户在 Project Web App 中修改资源计划。|
|ResourcePlanPublished|用户在 Project Web App 中发布资源计划。|
|ResourceRedacted|用户在 Project Web App 中编辑企业资源，删除所有个人信息。|
|ResourceWarningEnabledModified*|修改了资源配额警告。|
|SSOGroupCredentialsSet*|在 Secure Store Service 中设置了组凭据。|
|SSOUserCredentialsSet*|在 Secure Store Service 中设置了用户凭据。|
|SearchCenterUrlSet*|设置了搜索中心 URL。|
|SecondaryMySiteOwnerSet*|用户向其 MySite 添加了第二所有者。|
|SecurityCategoryModified|用户在 Project Web App 中创建、修改或删除安全类别。|
|SecurityGroupModified|用户在 Project Web App 中创建、修改或删除安全组。|
|SendToConnectionAdded*|全局管理员在 SharePoint 管理中心中的“记录管理”页上创建新“发送至”连接。 “发送至”连接指定文档存储库或记录中心设置。 创建“发送至”连接时，内容管理器可以将文档提交到指定位置。|
|SendToConnectionRemoved*|全局管理员在 SharePoint 管理中心的“记录管理”页上删除“发送至”连接。|
|SharedLinkCreated|用户在 SharePoint 或 OneDrive for Business 中创建共享文件的链接。 此链接可以发送给其他人，以便授予其对文件的访问权限。 用户可创建两个类型的链接：允许用户查看和编辑共享文件的链接，或仅允许用户查看文件的链接。|
|SharedLinkDisabled*|用户禁用（永久）为共享文件而创建的链接。|
|SharingInvitationAccepted*|用户接受共享文件或文件夹的邀请。 当用户与其他用户共享文件时，将记录此事件。|
|SharingRevoked*|用户取消共享之前与其他用户共享的文件或文件夹。 当用户停止与其他用户共享文件时，将记录此事件。|
|SharingSet|用户与组织内的其他用户共享位于 SharePoint 或 OneDrive for Business 中的文件或文件夹。|
|SiteAdminChangeRequest*|用户请求添加为 SharePoint 网站集的网站集管理员。 网站集管理员具有网站集和所有子网站的完全控制权限。|
|SiteCollectionAdminAdded*|网站集管理员或所有者添加人员，作为 SharePoint 或 OneDrive for Business 网站的网站集管理员。 网站集管理员具有网站集和所有子网站的完全控制权限。|
|SiteCollectionCreated*| 全局管理员在 SharePoint 组织中创建新的网站集。|
|SiteRenamed*|网站管理员或所有者重命名 SharePoint 或 OneDrive for Business 网站|
|StatusReportModified|用户在 Project Web App 中创建、修改或删除状态报告。|
|SyncGetChanges*|用户在 SharePoint 或 OneDrive for Business 的操作任务栏中单击“同步”****，以便将对文档库中的文件所做的任何更改都同步到他们的计算机。|
|TaskStatusAccessed|用户在 Project Web App 中访问一个或多个任务的状态。|
|TaskStatusApproved|用户在 Project Web App 中批准一个或多个任务的状态更新。|
|TaskStatusRejected|用户在 Project Web App 中拒绝一个或多个任务的状态更新。|
|TaskStatusSaved|用户在 Project Web App 中保存一个或多个任务的状态更新。|
|TaskStatusSubmitted|用户在 Project Web App 中提交一个或多个任务的状态更新。|
|TimesheetAccessed|用户访问 Project Web App 中的时间表。|
|TimesheetApproved|用户审批 Project Web App 中的时间表。|
|TimesheetRejected|用户拒绝 Project Web App 中的时间表。|
|TimesheetSaved|用户保存 Project Web App 中的时间表。|
|TimesheetSubmitted|用户在 Project Web App 中提交状态时间表。|
|UnmanagedSyncClientBlocked|用户尝试从不是组织域成员或者是尚未添加到可访问组织文档库的域列表（称为“安全收件人列表”）的域成员的计算机与 SharePoint 或 OneDrive for Business 网站建立同步关系。 不允许同步关系，并阻止用户计算机在文档库上同步、下载或上传文件。 有关此功能的信息，请参阅[使用 Windows PowerShell cmdlet 为安全收件人列表中的域启用 OneDrive 同步](https://docs.microsoft.com/en-us/powershell/module/sharepoint-online/index?view=sharepoint-ps)。|
|UpdateSSOApplication*|Secure Store Service 中更新目标应用程序。|
|UserAddedToGroup*|网站管理员或所有者向 SharePoint 或 OneDrive for Business 网站上的组添加人员。 向组添加人员授予用户已分配给组的权限。 |
|UserRemovedFromGroup*|网站管理员或所有者从 SharePoint 或 OneDrive for Business 网站上的组删除人员。 删除该人员后，不再向其授予已分配给组的权限。 |
|WorkflowModified|用户在 Project Web App 中创建、修改或删除 Enterprise Project 类型或 Workflow 阶段。|


> [!NOTE] 
> *此操作处于预览状态。



## <a name="sharepoint-file-operations"></a>SharePoint 文件操作

在[在 Office 365 保护中心搜索审核日志](https://support.office.com/en-us/article/Search-the-audit-log-in-the-Office-365-Security-Compliance-Center-0d4d0f35-390b-4518-800e-0c7ec95e946c?ui=en-US&amp;rs=en-US&amp;ad=US)的“文件和文件夹活动”部分列出的与文件相关的 SharePoint 事件使用此架构。



|**参数**|**类型**|**强制？**|**说明**|
|:-----|:-----|:-----|:-----|
|SiteUrl|Edm.String|是|用户访问的文件或文件夹所在网站的 URL。|
|SourceRelativeUrl|Edm.String|否|包含用户访问文件的文件夹的 URL。 _SiteURL_、_SourceRelativeURL_ 和 _SourceFileName_ 参数的值组合与 **ObjectID** 属性的值相同，它是用户访问的文件的完整路径名称。|
|SourceFileName|Edm.String|是|用户访问的文件或文件夹名称。|
|SourceFileExtension|Edm.String|否|用户访问的文件的文件扩展名。 如果访问对象是一个文件夹，则此属性为空。|
|DestinationRelativeUrl|Edm.String|否|在其中复制或移动文件的目标文件夹的 URL。 _SiteURL_、_DestinationRelativeURL_ 和 _DestinationFileName_ 参数的值组合与 **ObjectID** 属性的值相同，它是复制的文件的完整路径名称。 此属性仅对 FileCopied 和 FileMoved 事件显示。|
|DestinationFileName|Edm.String|否|复制或移动的文件的名称。 此属性仅对 FileCopied 和 FileMoved 事件显示。|
|DestinationFileExtension|Edm.String|否|复制或移动的文件的文件扩展名。 此属性仅对 FileCopied 和 FileMoved 事件显示。|
|UserSharedWith|Edm.String|否|与之共享资源的用户。|
|SharingType|Edm.String|否|分配给与之共享资源的用户的共享权限的类型。 通过 _UserSharedWith_ 参数标识此用户。|



## <a name="sharepoint-sharing-schema"></a>SharePoint 共享架构

 与文件共享相关的 SharePoint 事件。 它们不同于与文件和文件夹相关的事件，因为用户正在执行对另一个用户有一定影响的操作。 有关 SharePoint 共享架构信息，请参阅[在 Office 365 审核日志中使用共享审核](https://support.office.com/en-us/article/Use-sharing-auditing-in-the-Office-365-audit-log-50bbf89f-7870-4c2a-ae14-42635e0cfc01?ui=en-US&amp;rs=en-US&amp;ad=US)。



|**参数**|**类型**|**强制？**|**说明**|
|:-----|:-----|:-----|:-----|
|TargetUserOrGroupName |Edm.String|否|存储与之共享资源的目标用户或组的 UPN 或名称。|
|TargetUserOrGroupType|Edm.String|否|标识目标用户或组是成员、来宾、组还是合作伙伴。 |
|EventData|XML 代码|否|传达有关已发生的共享操作的后续信息，例如向组中添加用户或授予编辑权限。|


## <a name="sharepoint-schema"></a>SharePoint 架构

在[在 Office 365 保护中心搜索审核日志](https://support.office.com/en-us/article/Search-the-audit-log-in-the-Office-365-Security-Compliance-Center-0d4d0f35-390b-4518-800e-0c7ec95e946c?ui=en-US&amp;rs=en-US&amp;ad=US)中列出的 SharePoint 事件（除了文件和文件夹事件）使用此架构。



|**参数**|**类型**|**强制？**|**说明**|
|:-----|:-----|:-----|:-----|
|CustomEvent|Edm.String|否|自定义事件的可选字符串。|
|EventData|Edm.String|否|自定义事件的可选负载。|
|ModifiedProperties|Collection(ModifiedProperty)|否|属性包含在管理员事件中，例如将用户添加为网站或网站集管理组的成员。 该属性包括已修改属性的名称（例如，Site Admin 组）、已修改属性的新值（例如添加为网站管理员的用户）和已修改对象的先前值。|


## <a name="project-schema"></a>Project 架构

|**参数**|**类型**|**强制？**|**说明**|
|:-----|:-----|:-----|:-----|
|Entity|Edm.String|是| 审核针对的 [ProjectEntity](#project-entity)。|
|Action|Edm.String|是|采取的 [ProjectAction](#project-action)。|
|OnBehalfOfResId|Edm.Guid|否|代表其执行操作的资源 ID。|

<a name="project-action"></a>

### <a name="enum-project-action---type-edmint32"></a>枚举：Project Action - 类型：Edm.Int32


|**成员名称**|**说明**|
|:-----|:-----|
|Accepted|用户接受事件或工作流。|
|Accessed|用户访问实体。|
|Activated|用户激活实体、事件或工作流。|
|Cancelled|用户取消事件或工作流。|
|CheckedIn|用户签入实体。|
|CheckedOut|用户签出实体。|
|Copied|用户复制实体。|
|Created|用户创建实体。|
|Deactivated|用户停用实体。|
|Deleted|用户删除实体。|
|Exported|用户导出实体。|
|ForceCheckedIn|用户强制签入实体。|
|Modified|用户修改实体。|
|Published|用户发布实体。|
|Redacted|用户编辑实体。|
|Rejected|用户拒绝实体。|
|Restarted|用户重启事件或工作流。|
|Saved|用户保留实体。|
|Sent|用户发送实体。|
|Submitted|用户提交进行审阅的实体或工作流。|

<a name="project-entity"></a>
### <a name="enum-project-entity---type-edmint32"></a>枚举：Project Entity - 类型：Edm.Int32

|**成员名称**|**说明**|
|:-----|:-----|
|CustomField|表示企业自定义域。|
|Driver|表示项目组合驱动程序。|
|DriverPrioritization|表示项目组合优先顺序。|
|Engagement|表示资源预订。|
|EnterpriseCalendar|表示企业资源日历。|
|EnterpriseProjectType|表示企业项目类型。|
|FiscalPeriod|表示会计期间。|
|GanttChartFormat|表示甘特图格式。|
|GroupingFormat|表示视图分组格式。|
|LineClassification|表示时间表行分类。|
|LookupTable|表示企业版查找表。|
|PermissionTemplate|表示安全权限模板。|
|PortfolioAnalysis|表示项目组合分析。|
|Project|表示一个项目。|
|QueueJob|表示队列作业。|
|QuickLaunch|表示快速启动项。|
|Reporting|表示报告终结点。|
|Resource|表示企业资源。|
|ResourcePlan|表示与项目关联的资源计划。|
|SecurityCategory|表示安全类别。|
|SecurityGroup|表示安全组。|
|Setting|表示 Project Web App 设置|
|Statusing|表示任务状态更新。|
|StatusReport|表示状态报告。|
|TimeReportingPeriod|表示时间表的一段时间|
|Timesheet|表示时间表实体。|
|TimesheetAuditLog|表示时间表审核日志。|
|TimesheetManager|表示时间表管理员。|
|UserDelegate|表示其他用户的用户委派。|
|View|表示视图定义。|
|WorkflowPhase|表示工作流中的阶段。|
|WorkflowStage|表示工作流中的阶段。|

## <a name="exchange-admin-schema"></a>Exchange 管理员架构



|**参数**|**类型**|**强制**|**说明**|
|:-----|:-----|:-----|:-----|
|ModifiedObjectResolvedName|Edm.String|否|这是通过 cmdlet 修改的对象的用户友好名称。 只有当 cmdlet 修改对象时才会记录此参数。|
|Parameters|Collection(Common.NameValuePair)|否|与 Operations 属性中标识的 cmdlet 结合使用的所有参数的名称和值。|
|ModifiedProperties|Collection(Common.ModifiedProperty)|否|该属性包含在管理员事件中。 该属性包括已修改属性的名称、已修改属性的新值和已修改对象的先前值。|
|ExternalAccess|Edm.Boolean|是|指定 cmdlet 是由组织中的用户、Microsoft 数据中心人员或数据中心服务帐户，还是由委托的管理员运行。 值 **False** 表示 cmdlet 由组织中的某人运行。 值 **True** 表示 cmdlet 由数据中心人员、数据中心服务帐户或委托的管理员运行。|
|OriginatingServer|Edm.String|否|从中执行 cmdlet 的服务器的名称。|
|OrganizationName|Edm.String|否|租户名称。|


## <a name="exchange-mailbox-schema"></a>Exchange 邮箱架构



|**参数**|**类型**|**强制**|**说明**|
|:-----|:-----|:-----|:-----|
|LogonType|Self.[LogonType](#logontype)|否| 指示访问邮箱并执行已记录操作的用户类型。|
|InternalLogonType|Self.[LogonType](#logontype)|否|仅供内部使用。|
|MailboxGuid|Edm.String|否|访问邮箱的 Exchange GUID。|
|MailboxOwnerUPN|Edm.String|否|拥有已访问邮箱的人员的电子邮件地址。|
|MailboxOwnerSid|Edm.String|否|邮箱所有者的 SID。|
|MailboxOwnerMasterAccountSid|Edm.String|否|邮箱所有者帐户的主帐户 SID。|
|LogonUserSid|Edm.String|否|执行操作的用户的 SID。|
|LogonUserDisplayName|Edm.String|否|执行操作的用户的用户友好名称。|
|ExternalAccess|Edm.Boolean|是|如果登录用户的域与邮箱所有者的域不同，则为 true。|
|OriginatingServer |Edm.String|否|这是操作的源位置。|
|OrganizationName|Edm.String|否|租户名称。|
|ClientInfoString|Edm.String|否|有关用于执行此操作的电子邮件客户端的信息，如浏览器版本、Outlook 版本和移动设备信息。|
|ClientIPAddress|Edm.String|否|记录操作时使用的设备的 IP 地址。 IP 地址显示为 IPv4 或 IPv6 地址格式。|
|ClientMachineName|Edm.String|否|托管 Outlook 客户端的计算机名称。|
|ClientProcessName|Edm.String|否|用于访问邮箱的电子邮件客户端。 |
|ClientVersion|Edm.String|否|电子邮件客户端版本。|


### <a name="enum-logontype---type-edmint32"></a>枚举：LogonType - 类型：Edm.Int32

#### <a name="logontype"></a>LogonType


|**值**|**成员名称**|**说明**|
|:-----|:-----|:-----|
|0|Owner|邮箱所有者。|
|1|Admin|对某人的邮箱具有管理权限的人员。|
|2|Delegated|对某人的邮箱具有委派权限的人员。|
|3|Transport|Microsoft 数据中心的传输服务。|
|4|SystemService|中Microsoft 数据中心的服务帐户|
|5|BestAccess|仅供内部使用。|
|6|DelegatedAdmin|委派的管理员。|


### <a name="exchangemailboxauditgrouprecord-schema"></a>ExchangeMailboxAuditGroupRecord 架构



|**参数**|**类型**|**强制？**|**说明**|
|:-----|:-----|:-----|:-----|
|Folder|Self.[ExchangeFolder](#exchangefolder-complex-type)|否|一组项目的所在文件夹。|
|CrossMailboxOperations|Edm.Boolean|否|指示操作是否涉及多个邮箱。|
|DestMailboxId|Edm.Guid|否|仅当 CrossMailboxOperations 参数为 **True** 时设置。 指定目标邮箱的 GUID。|
|DestMailboxOwnerUPN|Edm.String|否|仅当 CrossMailboxOperations 参数为 **True** 时设置。 指定目标邮箱所有者的 UPN。|
|DestMailboxOwnerSid|Edm.String|否|仅当 CrossMailboxOperations 参数为 **True** 时设置。 指定目标邮箱的 SID。|
|DestMailboxOwnerMasterAccountSid|Edm.String|否|仅当 CrossMailboxOperations 参数为 **True** 时设置。 指定对目标邮箱所有者主帐户 SID 的 SID。|
|DestFolder|Self.[ExchangeFolder](#exchangefolder-complex-type)|否|用于移动等操作的目标文件夹。|
|Folders|Collection(Self.[ExchangeFolder](#exchangefolder-complex-type))|否|涉及操作的源文件夹信息；例如，如果选择文件夹然后删除。|
|AffectedItems|Collection(Self.[ExchangeItem](#exchangeitem-complex-type))|否|有关组中每个项目的信息。|



### <a name="exchangemailboxauditrecord-schema"></a>ExchangeMailboxAuditRecord 架构



|**参数**|**类型**|**强制？**|**说明**|
|:-----|:-----|:-----|:-----|
|Item|Self.[ExchangeItem](#exchangeitem-complex-type)|否|表示在对其执行操作的项|
|ModifiedProperties|Collection(Edm.String)|否|待定|
|SendAsUserSmtp|Edm.String|否|所模拟的用户的 SMTP 地址。|
|SendAsUserMailboxGuid|Edm.Guid|否|所访问的用于发送邮件的邮箱的 Exchange GUID。|
|SendOnBehalfOfUserSmtp|Edm.String|否|代表其发送电子邮件的用户的 SMTP 地址。|
|SendOnBehalfOfUserMailboxGuid|Edm.Guid|否|所访问的代表发送邮件的邮箱的 Exchange GUID。|


### <a name="exchangeitem-complex-type"></a>ExchangeItem 复杂类型


|**参数**|**类型**|**强制？**|**说明**|
|:-----|:-----|:-----|:-----|
|Id|Edm.String|是|存储 ID。|
|Subject|Edm.String|否|访问的邮件的主题行。|
|ParentFolder|Edm.ExchangeFolder|否|项目所在的文件夹名称。|
|Attachments|Edm.String|否|附加到邮件中的所有项的名称和文件大小列表。|

### <a name="exchangefolder-complex-type"></a>ExchangeFolder 复杂类型



|**参数**|**类型**|**强制？**|**说明**|
|:-----|:-----|:-----|:-----|
|Id|Edm.String|是|文件夹对象的存储 ID。|
|Path|Edm.String|否|访问的邮件所在的邮箱文件夹的名称。|



## <a name="azure-active-directory-base-schema"></a>Azure Active Directory 基本架构



|**参数**|**类型**|**强制？**|**说明**|
|:-----|:-----|:-----|:-----|
|AzureActiveDirectoryEventType|Self.[AzureActiveDirectoryEventType](#azureactivedirectoryeventtype)|是|Azure AD 事件的类型。 |
|ExtendedProperties|Collection(Common.NameValuePair)|否|Azure AD 事件的扩展属性。|
|ModifiedProperties|Collection(Common.ModifiedProperty)|否|该属性包含在管理员事件中。 该属性包括已修改属性的名称、已修改属性的新值和已修改属性的先前值。|

### <a name="enum-azureactivedirectoryeventtype---type--edmint32"></a>枚举：AzureActiveDirectoryEventType - 类型 - Edm.Int32

#### <a name="azureactivedirectoryeventtype"></a>AzureActiveDirectoryEventType

|**成员名称**|**说明**|
|:-----|:-----|
|AccountLogon|帐户登录事件。|
|AzureApplicationAuditEvent|Azure 应用程序安全事件。|

## <a name="azure-active-directory-account-logon-schema"></a>Azure Active Directory 帐户登录架构



|**参数**|**类型**|**强制？**|**说明**|
|:-----|:-----|:-----|:-----|
|Application|Edm.String|否|触发帐户登录活动的应用程序，如 Office 15。|
|Client|Edm.String|否|有关客户端设备、设备 OS 和用于帐户登录事件的设备浏览器的详细信息。|
|LoginStatus|Edm.Int32|是|此属性直接来自 OrgIdLogon.LoginStatus。 各种有趣的登录失败映射可以通过警报算法来完成。|
|UserDomain|Edm.String|是|租户标识信息 (TII)。|



### <a name="enum-credentialtype---type-edmint32"></a>枚举：CredentialType - 类型：Edm.Int32



|**值**|**成员名称**|**说明**|
|:-----|:-----|:-----|
|-1|Other|其他身份验证。|
|0|Password|用户凭据是用户名和密码。|
|1|MobilePhone|用户凭据是移动电话。|
|2|SecretQuestion|用户凭据是机密问题。|
|3|SecurePin|用户凭据是安全 PIN。|
|4|SecurePinReset|用户凭据是安全 PIN 重置。|
|11|EasyID|用户凭据是 EasyID。|
|14|PasswordIndexCredentialType|用户凭据是 PasswordIndexCredentialType。|
|16|Device|用户凭据是设备。|
|17|ForeignRealmIndex|用户凭据是 ForeignRealmIndex。|

### <a name="enum-logintype---type-edmint32"></a>枚举：LoginType - 类型：Edm.Int32



|**值**|**成员名称**|**说明**|
|:-----|:-----|:-----|
|-1|Other|其他 i 类型。|
|1|InitialAuth|使用初始身份验证登录|
|2|CookieCopy|使用 cookie 登录。|
|3|SilentReAuth|通过无提示重新身份验证登录。|


### <a name="enum-authenticationmethod---type-edmint32"></a>枚举：AuthenticationMethod - 类型：Edm.Int32



|**值**|**成员名称**|**说明**|
|:-----|:-----|:-----|
|0|Min|身份验证方法是 Min|
|1|Password|身份验证方法是密码。|
|2|Digest|身份验证方法是摘要。|
|3|ProxyAuth|身份验证方法是 ProxyAuth。|
|4|InfoCard|身份验证方法是 InfoCard。|
|5|DAToken|身份验证方法是 DAToken。|
|6|Sha1RememberMyPassword|身份验证方法是 Sha1RememberMyPassword。|
|7|LMPasswordHash|身份验证方法是 LMPasswordHash。|
|8|ADFSFederatedToken|身份验证方法是 ADFSFederatedToken。|
|9|EID|身份验证方法是 EID。|
|10|DeviceID|身份验证方法是 DeviceID。 |
|11|MD5|身份验证方法是 MD5。|
|12|EncProxyPasswordHash|身份验证方法是 EncProxyPasswordHash。|
|13|LWAFederation|身份验证方法是 LWAFederation。|
|14|Sha1HashedPassword|身份验证方法是 Sha1HashedPassword。|
|15|SecurePin|身份验证方法是安全 Pin。|
|16|SecurePinReset|身份验证方法是安全 PIN 重置。|
|17|SAML20PostSimpleSign|身份验证方法是 SAML20PostSimpleSign。|
|18|SAML20Post|身份验证方法是 SAML20Post。|
|19|OneTimeCode|身份验证方法是一次性代码。|



## <a name="azure-active-directory-schema"></a>Azure Active Directory 架构



|**参数**|**类型**|**强制？**|**说明**|
|:-----|:-----|:-----|:-----|
|Actor|Collection(Self.[IdentityTypeValuePair](#complex-type-identitytypevaluepair))|否|执行操作的用户或服务主体。|
|ActorContextId|Edm.String|否|参与者所属组织的 GUID。|
|ActorIpAddress|Edm.String|否|IPV4 或 IPV6 地址格式的参与者 IP 地址。|
|InterSystemsId|Edm.String|否|在 Office 365 服务的组件之间跟踪操作的 GUID。|
|IntraSystemsId|Edm.String|否|Azure Active Directory 生成的用来跟踪操作的 GUID。|
|SupportTicketId|Edm.String|否|在“代表操作”情况下针对操作的客户支持票证 ID。|
|Target|Collection(Self.[IdentityTypeValuePair](#complex-type-identitytypevaluepair))|否|对其执行操作（由 Operation 属性标识）的用户。|
|TargetContextId|Edm.String|否|目标用户所属组织的 GUID。|



### <a name="complex-type-identitytypevaluepair"></a>复杂类型 IdentityTypeValuePair



|**参数**|**类型**|**强制？**|**说明**|
|:-----|:-----|:-----|:-----|
|ID|Edm.String|是|给定类型的标识值。|
|Type|Self.IdentityType|是|标识类型。|

### <a name="enum-identitytype---type-edmint32"></a>枚举：IdentityType - 类型：Edm.Int32

#### <a name="identitytype"></a>IdentityType

|**成员名称**|**说明**|
|:-----|:-----|
|Claim|标识是用于授权目的的声明。|
|Name|审核操作参与者或目标标识显示名称。|
|Other|参与者标识是其他类型，例如由 Office 365 服务生成的 GUID 的 ObjectId。|
|PUID|审核操作参与者或目标 passport 唯一 ID (PUID)。|
|SPN|如果操作是由 Office 365 服务执行的，服务主体的身份。|
|UPN|用户主体名称。|


## <a name="azure-active-directory-sts-logon-schema"></a>Azure Active Directory STS 登录架构



|**参数**|**类型**|**强制？**|**说明**|
|:-----|:-----|:-----|:-----|
|ApplicationId|Edm.String|否|表示正在请求登录的应用程序的 GUID。 可以通过 Azure Active Directory Graph API 查找显示名称。|
|Client|Edm.String|否|客户端设备信息，由执行登录的浏览器提供。|
|LogonError|Edm.String|否|用于失败登录，包含登录失败的原因。|

## <a name="dlp-schema"></a>DLP 架构

DLP 事件可用于 Exchange Online、SharePoint Online 和 OneDrive For Business。 请注意，Exchange 中的 DLP 事件仅适用于基于统一 DLP 策略的事件（例如通过安全与合规中心配置的事件）。 不支持基于 Exchange 传输规则的 DLP 事件。

在常见架构中，DLP（数据丢失防护）事件始终具有 UserKey=“DlpAgent”。 有三种类型的 DlpEvents，它们被存储为常见架构的 Operation 属性值：

- DlpRuleMatch：表示匹配某个规则。 Exchange 和 SharePoint Online 和 OneDrive for Business 中存在这些事件。 对于 Exchange，它包含误报和重写信息。 对于 SharePoint Online 和 OneDrive for Business，误报和重写会生成单独事件。

- DlpRuleUndo：仅存在于 SharePoint Online 和 OneDrive for Business 中，指示以前应用的策略操作已“撤销”，因为用户指定误报/重写，或者因为文件不再受策略的制约（由于策略更改或文档内容的更改）。

- DlpInfo：仅存在于 SharePoint Online 和 OneDrive for Business 中，指示指定的误报，但没有“撤销”任何操作。



|**参数**|**类型**|**强制**|**说明**|
|:-----|:-----|:-----|:-----|
|SharePointMetaData|Self.[SharePointMetadata](#sharepointmetadata-complex-type)|否|说明 SharePoint 或 OneDrive for Business 中有关包含敏感信息的文档的元数据。|
|ExchangeMetaData|Self.[ExchangeMetadata](#exchangemetadata-complex-type)|否|介绍有关包含敏感信息的电子邮件的元数据。|
|ExceptionInfo|Edm.String|否|确定策略不再适用的原因和/或最终用户指出的有关误报和/或重写的任何信息。|
|PolicyDetails|Collection(Self.[PolicyDetails](#policydetails-complex-type))|是|有关触发 DLP 事件的一个或多个策略的信息。|
|SensitiveInfoDetectionIsIncluded|Boolean|是|指示事件是否包含来自源内容的敏感数据类型和相关上下文的值。 访问敏感数据需要 Azure Active Directory 中的“读取包括敏感详细信息的 DLP 策略事件”权限。|




### <a name="sharepointmetadata-complex-type"></a>SharePointMetadata 复杂类型

|**参数**|**类型**|**强制？**|**说明**|
|:-----|:-----|:-----|:-----|
|From|Edm.String|是|触发事件的用户。 这将是 FileOwner、LastModifier 或 LastSharer。|
|itemCreationTime|Edm.Date|是|记录事件时的 UTC 日期时间戳。|
|SiteCollectionGuid|Edm.Guid|是|网站集的 GUID。|
|SiteCollectionUrl|Edm.String|是|SharePoint 网站名称。|
|FileName|Edm.String|是|路径名称。|
|FileOwner|Edm.String|是|文档所有者。|
|FilePathUrl|Edm.String|是|文档的 URL|
|DocumentLastModifier|Edm.String|是|上次修改文档的用户。|
|DocumentSharer|Edm.String|是|上次修改共享文档的用户。|
|UniqueId|Edm.String|是|标识文件的 guid。|
|LastModifiedTime|Edm.DateTime|是|上次修改文档时的 UTC 时间戳。|


### <a name="exchangemetadata-complex-type"></a>ExchangeMetadata 复杂类型

|**参数**|**类型**|**强制？**|**说明**|
|:-----|:-----|:-----|:-----|
|MessageID|Edm.String|是|触发事件的电子邮件消息 ID。|
|From|Edm.String|是|发送电子邮件的用户。|
|To|Collection(Edm.String)|否|邮件“收件人”行上的电子邮件地址集合。|
|CC|Collection(Edm.String)|否|邮件“抄送”行上的电子邮件地址集合。|
|BCC|Collection(Edm.String)|否|邮件“密件抄送”行上的电子邮件地址集合。|
|Subject|Edm.String|是|电子邮件主题。|
|发件箱|Edm.DateTime|是|发送电子邮件时的 UTC 时间。|
|RecipientCount|Edm.Int32|是|邮件“收件人”、“抄送”和“密件抄送”行上的所有收件人总数。|


### <a name="policydetails-complex-type"></a>PolicyDetails 复杂类型

|**参数**|**类型**|**强制？**|**说明**|
|:-----|:-----|:-----|:-----|
|PolicyId|Edm.Guid|是|此事件 DLP 策略的 guid。|
|PolicyName|Edm.String|是|此事件 DLP 策略的友好名称。|
|Rules|Collection(Self.[Rules](#rules-complex-type))|是|关于策略中匹配此事件的规则的信息。|

### <a name="rules-complex-type"></a>Rules 复杂类型

|**参数**|**类型**|**强制？**|**说明**|
|:-----|:-----|:-----|:-----|
|RuleId|Edm.Guid|是|此事件 DLP 规则的 guid。|
|RuleName|Edm.String|是|此事件 DLP 规则的友好名称。|
|Actions|Collection(Edm.String)|否|由于 DLP RuleMatch 事件而采取的操作列表。|
|OverriddenActions|Collection(Edm.String)|否|先前采取的由于 DLPRuleUndo 事件而撤销的操作列表。 |
|Severity|Edm.String|否|规则匹配的严重性（低、中和高）。|
|RuleMode|Edm.String|是|指示 DLP 规则是否设置为“强制使用”、“在通知情况下审核”或“仅审核”。|
|ConditionsMatched|Self.[ConditionsMatched](#conditionsmatched-complex-type)|否|关于此事件匹配规则条件的详细信息。|

### <a name="conditionsmatched-complex-type"></a>ConditionsMatched 复杂类型

|**参数**|**类型**|**强制？**|**说明**|
|:-----|:-----|:-----|:-----|
|SensitiveInformation|Collection(Self.[SensitiveInformation](#sensitiveinformation-complex-type))|否|有关检测到的敏感信息类型的信息。|
|DocumentProperties|Collection(NameValuePair)|否|有关触发规则匹配的文档属性的信息。|
|OtherConditions|Collection(NameValuePair)|否|描述匹配的任何其他条件的键值对列表。|

### <a name="sensitiveinformation-complex-type"></a>SensitiveInformation 复杂类型

|**参数**|**类型**|**强制？**|**说明**|
|:-----|:-----|:-----|:-----|
|Confidence|Edm.Int|是|匹配检测的模式的置信度。|
|Count|Edm.Int|是|检测到的敏感实例数。|
|SensitiveType|Edm.Guid|是|识别检测到的敏感数据类型的 guid。|
|SensitiveInformationDetections|Self.SensitiveInformationDetections|否|包含具有以下详细信息的敏感信息数据的对象数组 - 匹配值和匹配值上下文。|

### <a name="sensitiveinformationdetections-complex-type"></a>SensitiveInformationDetections 复杂类型 
DLP 敏感数据仅可在已获得“读取 DLP 敏感数据”权限的用户的活动源 API 中获得。 

|**参数**|**类型**|**强制？**|**说明**|
|:-----|:-----|:-----|:-----|
|Detections|Collection(Self.Detections)|是|检测到的敏感信息数组。 信息包含键值对，其中值 = 匹配值（如 SSN 的信用卡值），上下文 = 来自包含匹配值的源内容的摘要。 |
|ResultsTruncated|Edm.Boolean|是|指示日志是否由于大量结果而被截断。 |

### <a name="exceptioninfo-complex-type"></a>ExceptionInfo 复杂类型

|**参数**|**类型**|**强制？**|**说明**|
|:-----|:-----|:-----|:-----|
|Reason|Edm.String|否|对于 DLPRuleUndo 事件，这说明了为什么规则不再适用，原因可能是以下 3 个中的一个：重写、文档更改或策略更改|
|FalsePositive|Edm.Boolean|否|指示用户是否将此事件指定为误报。|
|Justification|Edm.String|否|如果用户选择重写策略，则可以在此捕获任何用户指定的理由。|
|Rules|Collection(Edm.Guid)|否|每个指定为误报或重写的规则或未撤销操作的规则的 guid 集合。|

## <a name="security-and-compliance-center-schema"></a>安全与合规中心架构

|**参数**|**类型**|**强制**|**说明**|
|:-----|:-----|:-----|:-----|
|StartTime|Edm.Date|否|执行 cmdlet 时的日期和时间。|
|ClientRequestId|Edm.String|否|可用于将此 cmdlet 与安全与合规中心 UX 操作关联起来的 GUID。 此信息仅供 Microsoft 支持部门使用。|
|CmdletVersion|Edm.String|否|执行 cmdlet 时的内部版本号。|
|EffectiveOrganization|Edm.String|否|受 cmdlet 影响的组织 GUID。 （弃用：此参数以后将停止显示。）|
|UserServicePlan|Edm.String|否|分配给执行 cmdlet 的用户的 Exchange Online Protection 服务计划。|
|ClientApplication|Edm.String|否|如果 cmdlet 由应用程序执行，而不是由远程 powershell 执行，则此字段包含该应用程序的名称。|
|Parameters|Edm.String|否|与不包含个人身份信息的 cmdlet 结合使用的参数的名称和值。|
|NonPiiParameters|Edm.String|否|与包含个人身份信息的 cmdlet 结合使用的参数的名称和值。 （弃用：此字段将在以后停止显示，其内容将与 Parameters 字段合并。）|

## <a name="security-and-compliance-alerts-schema"></a>安全与合规警报架构

警报信号包括：

- 基于[安全与合规中心的警报策略](https://docs.microsoft.com/office365/securitycompliance/alert-policies#default-alert-policies)生成的所有警报。
- 在 [Office 365 云应用安全](https://docs.microsoft.com/office365/securitycompliance/office-365-cas-overview)和 [Microsoft Cloud App Security](https://docs.microsoft.com/zh-CN/cloud-app-security/what-is-cloud-app-security) 中生成的 Office 365 相关警报。

这些事件的 UserId 和 UserKey 始终为 SecurityComplianceAlerts。 有两种类型的警报信号，它们被存储为常见架构的 Operation 属性值：

- AlertTriggered：由于策略匹配而生成新警报。

- AlertEntityGenerated：向警报添加新实体。 此事件仅适用于基于 Office 365 安全与合规中心的警报策略生成的警报。 每个生成的警报都可与一个或多个上述事件相关联。 例如，如果任何用户在 5 分钟内删除了超过 100 个文件，则定义警报策略以触发警报。 如果两个用户几乎同时超过阈值，则会有两个 AlertEntityGenerated 事件，但只有一个 AlertTriggered 事件。

|**参数**|**类型**|**强制**|**说明**|
|:-----|:-----|:-----|:-----|
|AlertId|Edm.Guid|是|警报的 GUID。|
|AlertType|Self.String|是|警报的类型。 警报类型包括： <ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>系统警报</p></li><li><p>自定义警报</p></li>|
|Name|Edm.String|是|警报的名称。|
|PolicyId|Edm.Guid|否|触发了警报的策略的 GUID。|
|Status|Edm.String|否|警报的状态。 状态包括： <ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>活动</p></li><li><p>正在调查</p></li><li><p>已解决</p></li><li><p>已解除</p></li></ul>|
|Severity|Edm.String|否|警报的严重性。 严重性级别包括： <ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>低</p></li><li><p>中</p></li><li><p>高</p></li></ul>|
|类别|Edm.String|否|警报的类别。 类别包括： <ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>DataLossPrevention</p></li><li><p>ThreatManagement</p></li><li><p>DataGovernance</p></li><li><p>AccessGovernance</p></li><li><p>MailFlow</p></li><li><p>Other</p></li></ul>|
|Source|Edm.String|否|警报的来源。 来源包括： <ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>Office 365 安全与合规中心</p></li><li><p>云应用安全</p></li></ul>|
|Comments|Edm.String|否|查看过警报的用户留下的注释。 默认情况下为“新警报”。|
|Data|Edm.String|否|警报或警报实体的详细数据 blob。|
|AlertEntityId|Edm.String|否|警报实体的标识符。 此参数仅适用于 AlertEntityGenerated 事件。|
|EntityType|Edm.String|否|警报或警报实体的类型。 实体类型包括： <ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>User</p></li><li><p>Recipients</p></li><li><p>Sender</p></li><li><p>MalwareFamily</p></li></ul>此参数仅适用于 AlertEntityGenerated 事件。|

## <a name="yammer-schema"></a>Yammer 架构

在[在 Office 365 保护中心搜索审核日志](https://support.office.com/en-us/article/Search-the-audit-log-in-the-Office-365-Security-Compliance-Center-0d4d0f35-390b-4518-800e-0c7ec95e946c?ui=en-US&amp;rs=en-US&amp;ad=US)中列出的 Yammer 事件将使用此架构。

|**参数**|**类型**|**强制**|**说明**|
|:-----|:-----|:-----|:-----|
|ActorUserId|Edm.String|否|执行操作的用户的电子邮件。|
|ActorYammerUserId|Edm.Int32|否|执行操作的用户的 ID。|
|DataExportType|Edm.String|否|如果数据导出包括邮件、备注、文件、主题、用户和组，则返回“data”；如果数据导出仅包括用户，则返回“user”。|
|FileId|Edm.Int32|否|操作中的文件 ID。 |
|FileName|Edm.String|否|操作中的文件名称。 如果与操作不相关，将显示空白。|
|GroupName|Edm.String|否|操作中的组名称。 如果与操作不相关，将显示空白。|
|IsSoftDelete|Edm.Boolean|否|如果网络数据保留策略设置为“软删除”，返回“true”；如果网络数据保留策略设置为“硬删除”，则返回“false”。|
|MessageId|Edm.Int32|否|操作中的消息 ID。|
|YammerNetworkId|Edm.Int32|否|执行操作的用户的网络 ID。|
|TargetUserId|Edm.String|否|操作中的目标用户的电子邮件。 如果与操作不相关，将显示空白。|
|TargetYammerUserId|Edm.Int32|否|操作中的目标用户的 ID。|
|VersionId|Edm.Int32|否|操作中文件的版本 ID。|

## <a name="sway-schema"></a>Sway 架构

在[在 Office 365 保护中心搜索审核日志](https://support.office.com/en-us/article/Search-the-audit-log-in-the-Office-365-Security-Compliance-Center-0d4d0f35-390b-4518-800e-0c7ec95e946c?ui=en-US&amp;rs=en-US&amp;ad=US)中列出的 Sway 事件（除了文件和文件夹事件）将使用此架构。

|**参数**|**类型**|**强制？**|**说明**|
|:-----|:-----|:-----|:-----|
|ObjectType|Self.[ObjectType](#objecttype)|否|触发事件的访问点。 访问点包括： <ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>Sway</p></li><li><p>在主机中嵌入的 Sway</p></li><li><p>Office 365 管理门户中的 Sway 设置</p></li></ul>|
|Endpoint|Self.[Endpoint](#endpoint)|否|触发事件的 Sway 客户端终结点。 Sway 客户端终结点可以是 web、iOS、Windows 或 Android。 |
|BrowserName|Edm.String|否|用于访问触发事件的 Sway 的浏览器。 |
|DeviceType|Self.[DeviceType](#devicetype)|否|用于访问触发事件的 Sway 的设备类型。 设备类型可以是台式机、移动设备或平板电脑。|
|SwayLookupId|Edm.String|否|Sway ID。 |
|SiteUrl|Edm.String|否|Sway 的 URL。|
|OperationResult|Self.[OperationResult](#operationresult)|否|成功或失败。|


### <a name="enum-objecttype---type-edmint32"></a>枚举：ObjectType - 类型 Edm.Int32

#### <a name="objecttype"></a>ObjectType

|**值**|**成员名称**|**说明**|
|:-----|:-----|:-----|
|0|Sway|从 Sway 触发事件。|
|1|SwayEmbedded|从嵌入主机的 Sway 触发事件。|
|2|SwayAdminPortal|从 Office 365 管理门户的 Sway 服务设置触发事件。|


### <a name="enum-operationresult---type-edmint32"></a>枚举：OperationResult - 类型 Edm.Int32

#### <a name="operationresult"></a>OperationResult

|**值**|**成员名称**|**说明**|
|:-----|:-----|:-----|
|0|Succeeded|事件成功。|
|1|Failed|事件失败。|


### <a name="enum-endpoint---type-edmint32"></a>枚举：Endpoint - 类型 Edm.Int32

#### <a name="endpoint"></a>Endpoint

|**值**|**成员名称**|**说明**|
|:-----|:-----|:-----|
|0|SwayWeb|使用 Sway Web 客户端触发事件。|
|1|SwayIOS|使用 Sway iOS 客户端触发事件。|
|2|SwayWindows|使用 Sway Windows 客户端触发事件。|
|3|SwayAndroid|使用 Sway Android 客户端触发事件。|



### <a name="enum-devicetype---type-edmint32"></a>枚举：DeviceType - 类型 Edm.Int32

#### <a name="devicetype"></a>DeviceType

|**值**|**成员名称**|**说明**|
|:-----|:-----|:-----|
|0|桌面|使用桌面触发事件。|
|1|移动设备|使用移动设备触发事件。|
|2|平板电脑|使用平板电脑设备触发事件。|



### <a name="enum-swayauditoperation---type-edmint32"></a>枚举：SwayAuditOperation - 类型：Edm.Int32

#### <a name="swayauditoperation"></a>SwayAuditOperation

|**值**|**成员名称**|**说明**|
|:-----|:-----|:-----|
|1|Create|用户创建 Sway。|
|2|Delete|用户删除 Sway。|
|3|View|用户查看 Sway。|
|4|Edit|用户编辑 Sway。|
|5|Duplicate|用户复制 Sway。|
|7|Share|用户启动共享 Sway。 此事件捕获在 Sway 共享菜单中单击特定共享目标的用户操作。 该事件并不表示用户是否真正执行并完成共享操作。|
|8|ChangeShareLevel|用户更改 Sway 的共享级别。 此事件捕获用户更改与 Sway 关联的共享范围。 例如，从组织内更改为公开。|
|9|RevokeShare|用户通过撤销访问来停止共享 Sway。 撤销访问更改与 Sway 关联的链接。|
|10|EnableDuplication|用户启用 Sway 的复制功能（默认启用）。|
|11|DisableDuplication|用户禁用 Sway 的复制功能（默认禁用）。|
|12|ServiceOn|用户通过 Office 365 管理中心对整个组织启用 Sway（默认启用）。|
|13|ServiceOff|用户通过 Office 365 管理中心对整个组织禁用 Sway（默认禁用）。|
|14|ExternalSharingOn|用户通过 Office 365 管理中心对整个组织启用外部共享。|
|15|ExternalSharingOff|用户通过 Office 365 管理中心对整个组织禁用外部共享。|

## <a name="data-center-security-base-schema"></a>数据中心安全基本架构



|**参数**|**类型**|**强制？**|**说明**|
|:-----|:-----|:-----|:-----|
|DataCenterSecurityEventType|Self.[DataCenterSecurityEventType](#datacentersecurityeventtype)|是|锁定框中的 dmdlet 事件的类型。|

### <a name="enum-datacentersecurityeventtype---type-edmint32"></a>枚举：DataCenterSecurityEventType - 类型：Edm.Int32

#### <a name="datacentersecurityeventtype"></a>DataCenterSecurityEventType


|**成员名称**|**说明**|
|:-----|:-----|
|DataCenterSecurityCmdletAuditEvent|这是 cmdlet 审核类型事件的枚举值。|



## <a name="data-center-security-cmdlet-schema"></a>数据中心安全 Cmdlet 架构



|**参数**|**类型**|**强制？**|**说明**|
|:-----|:-----|:-----|:-----|
|StartTime|Edm.Date|是|Cmdlet 执行的开始时间。|
|EffectiveOrganization|Edm.String|是|提升/cmdlet 面向的租户的名称。|
|ElevationTime|Edm.Date|是|提升的开始时间。|
|ElevationApprover|Edm.String|是|Microsoft 管理员名称。|
|ElevationApprovedTime|Edm.Date|否|批准提升的时间戳。|
|ElevationRequestId|Edm.Guid|是|提升请求的唯一标识符。|
|ElevationRole|Edm.String|否|为其请求提升的角色。|
|ElevationDuration|Edm.Int32|是|提升处于活动状态的持续时间。|
|GenericInfo|Edm.String|否|用于注释和其他通用信息。|


## <a name="microsoft-teams-schema"></a>Microsoft Teams 架构

|**参数**|**类型**|**强制？**|**说明**|
|:-----|:-----|:-----|:-----|
|MessageId|Edm.String|否|聊天或频道消息的标识符。|
|MeetupId|Edm.String|否|计划或临时会议的标识符。|
|Members|Collection(Self.[MicrosoftTeamsMember](#MicrosoftTeamsMember-complex-type))|否|团队中的用户列表。|
|TeamName|Edm.String|否|审核中的团队名称。|
|TeamGuid|Edm.Guid|否|审核中团队的唯一标识符。|
|ChannelName|Edm.String|否|审核中的频道名称。|
|ChannelGuid|Edm.Guid|否|审核中的频道的唯一标识符。|

### <a name="microsoftteamsmember-complex-type"></a>MicrosoftTeamsMember 复杂类型

|**参数**|**类型**|**强制？**|**说明**|
|:-----|:-----|:-----|:-----|
|UPN|Edm.String|否|用户的用户主体名称。|
|Role|Self.[MemberRoleType](#memberroletype)|否|团队中用户的角色。|
|DisplayName|Edm.String|否|用户的显示名称。|

### <a name="enum-memberroletype---type-edmint32"></a>枚举：MemberRoleType - 类型：Edm.Int32

#### <a name="memberroletype"></a>MemberRoleType

|**值**|**成员名称**|**说明**|
|:-----|:-----|:-----|
|0|Member|属于团队成员的用户。|
|1|Owner|担任团队所有者的用户。|
|2|Guest|不属于团队成员的用户。|


## <a name="microsoft-teams-add-ons-schema"></a>Microsoft Teams 加载项架构

|**参数**|**类型**|**强制？**|**说明**|
|:-----|:-----|:-----|:-----|
|AddOnType|Self.[AddOnType](#addontype)|否|生成此事件的加载项类型。|
|AddonName|Edm.String|否|生成此事件的加载项名称。|
|AddOnGuid|Edm.Guid|否|生成此事件的加载项的唯一标识符。|
|TabType|Edm.String|否|生成此事件的选项卡类型。|

### <a name="enum-addontype---type-edmint32"></a>枚举：AddOnType - 类型：Edm.Int32

#### <a name="addontype"></a>AddOnType

|**值**|**成员名称**|**说明**|
|:-----|:-----|:-----|
|1|Bot|Microsoft Teams 机器人。|
|2|Connector|Microsoft Teams 连接器。|
|3|Tab|Microsoft Teams 选项卡。|


## <a name="microsoft-teams-settings-schema"></a>Microsoft Teams 设置架构

|**参数**|**类型**|**强制？**|**说明**|
|:-----|:-----|:-----|:-----|
|ModifiedProperty|Common.ModifiedProperty|否|修改的属性。 它将包含属性的 **Name**、**OldValue** 和 **NewValue**。|
|ExtendedProperties|Collection(Common.NameValuePair)|否|被更改的设置的扩展属性列表。 每个属性都将具有 **Name** 和 **Value**。|

## <a name="office-365-advanced-threat-protection-and-threat-intelligence-schema"></a>Office 365 高级威胁防护和威胁智能架构

Office 365 高级威胁防护 (ATP) 和威胁智能事件适用于具有 ATP、威胁智能或 E5 订阅的 Office 365 客户。 ATP 和威胁智能源中的每个事件对应以下确定为包含威胁的事件：

- 由组织中的用户发送或接收电子邮件，同时对送达的邮件进行检测，并从[零时差自动清除](https://support.office.com/zh-CN/article/Zero-hour-auto-purge-protection-against-spam-and-malware-96deb75f-64e8-4c10-b570-84c99c674e15)检测邮件。 

- 组织中的用户单击 URL，基于 [Office 365 ATP 安全链接](https://docs.microsoft.com/office365/securitycompliance/atp-safe-links)保护被检测为恶意。  

### <a name="email-message-events"></a>电子邮件事件

|**参数**|**类型**|**强制？**|**说明**|
|:-----|:-----|:-----|:-----|
|AttachmentData|Collection(Self.[AttachmentData](#AttachmentData))|否|有关触发事件的电子邮件中附件的数据。|
|DetectionType|Self.[DetectionType](#DetectionType)|是|检测类型。|
|DetectionMethod|Edm.String|是|Office 365 ATP 用于检测的方法或技术。|
|InternetMessageId|Edm.String|是|Internet 邮件 ID。|
|NetworkMessageId|Edm.String|是|Exchange Online 网络消息 ID。|
|P1Sender|Edm.String|是|电子邮件发件人的返回路径。|
|P2Sender|Edm.String|是|电子邮件的发件人。|
|Recipients|Collection(Edm.String)|是|电子邮件的收件人数组。|
|SenderIp|Edm.String|是|提交 Office 365 电子邮件的 IP 地址。 IP 地址显示为 IPv4 或 IPv6 地址格式。|
|Subject|Edm.String|是|邮件的主题行。|
|Verdict|Edm.String|是|邮件裁定。|

### <a name="enum-detectiontype---type-edmint32"></a>枚举：DetectionType - 类型：Edm.Int32

#### <a name="detectiontype"></a>DetectionType

|**值**|**成员名称**|**说明**|
|:-----|:-----|:-----|
|0|Inline|在送达时检测到威胁。|
|1|Delayed|在送达后检测到威胁。|
|2|ZAP|通过[零时差自动清除](https://support.office.com/zh-CN/article/Zero-hour-auto-purge-protection-against-spam-and-malware-96deb75f-64e8-4c10-b570-84c99c674e15)删除邮件。 使用此检测类型的事件通常前面是“Delayed”检测类型的邮件。|


### <a name="attachmentdata-complex-type"></a>AttachmentData 复杂类型

#### <a name="attachmentdata"></a>AttachmentData

|**参数**|**类型**|**强制？**|**说明**|
|:-----|:-----|:-----|:-----|
|FileName|Edm.String|是|附件的文件名。|
|FileType|Edm.String|是|附件的文件类型。|
|FileVerdict|Self.[FileVerdict](#FileVerdict)|是|文件恶意软件裁定。|
|MalwareFamily|Edm.String|否|文件恶意软件系列。|
|SHA256|Edm.String|是|文件 SHA256 哈希。|

### <a name="enum-fileverdict---type-edmint32"></a>枚举：FileVerdict - 类型：Edm.Int32

#### <a name="fileverdict"></a>FileVerdict

|**值**|**成员名称**|**说明**|
|:-----|:-----|:-----|
|0|Good|未检测到任何威胁。|
|1|Bad|在附件中发现恶意软件。|
|-1|Error|扫描/分析错误。|
|-2|Timeout|扫描/分析超时。|
|-3|Pending|扫描/分析未完成。|

### <a name="url-time-of-click-events"></a>URL 单击时事件

|**参数**|**类型**|**强制？**|**说明**|
|:-----|:-----|:-----|:-----|
|UserID|Edm.String|是|单击 URL 的用户的标识符（例如电子邮件地址）。|
|AppName|Edm.String|是|从中单击 URL 的 Office 365 服务（例如邮件）。|
|Blocked|Edm.Boolean|是|如果单击 URL 被 [Office 365 ATP安全链接](https://docs.microsoft.com/office365/securitycompliance/atp-safe-links)保护阻止，则为 true。|
|ClickedThrough|Edm.Boolean|是|如果用户根据组织的 [Office 365 ATP 安全链接](https://docs.microsoft.com/office365/securitycompliance/atp-safe-links)保护策略通过单击（覆盖）解除了 URL 阻止，则为 true。|
|SourceId|Edm.String|是|从中单击 URL 的 Office 365 服务的标识符（例如，对于邮件而言，这是 Exchange Online 网络消息 ID）。|
|TimeOfClick|Edm.Date|是|用户单击 URL 时的协调世界时 (UTC) 日期和时间。|
|URL|Edm.String|是|用户单击 URL。|
|UserIp|Edm.String|是|单击 URL 的用户的 IP 地址。 IP 地址显示为 IPv4 或 IPv6 地址格式。|

## <a name="power-bi-schema"></a>Power BI 架构

在[在 Office 365 保护中心搜索审核日志](/power-bi/service-admin-auditing#activities-audited-by-power-bi)中列出的 Power BI 事件将使用此架构。

|**参数**|**类型**|**强制？**|**说明**|
|:-----|:-----|:-----|:-----|
| AppName               | Edm.String   Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true"                            |  否  | 发生事件的应用名称。 |
| DashboardName         | Edm.String   Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true"                            |  否  | 发生事件的仪表板名称。 |
| DataClassification    | Edm.String   Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true"                            |  否  | [数据分类](/power-bi/service-data-classification)（如果有），针对发生事件的仪表板。 |
| DatasetName           | Edm.String   Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true"                            |  否  | 发生事件的数据集名称。 |
| MembershipInformation | Collection([MembershipInformationType](#MembershipInformationType))   Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true" |  否  | 与组相关的成员身份信息。 |
| OrgAppPermission      | Edm.String   Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true"                            |  否  | 组织应用（整个组织、特定用户或特定组）的权限列表。 |
| ReportName            | Edm.String   Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true"                            |  否  | 发生事件的报表名称。 |
| SharingInformation    | Collection([SharingInformationType](#SharingInformationType))   Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true"    |  否  | 与向其发送共享邀请的人员相关的信息。 |
| SwitchState           | Edm.String   Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true"                            |  否  | 与不同租户级开关的状态相关的信息。 |
| WorkSpaceName         | Edm.String   Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true"                            |  否  | 发生事件的工作区名称。 |

### <a name="membershipinformationtype-complex-type"></a>MembershipInformationType 复杂类型

|**参数**|**类型**|**强制？**|**说明**|
|:-----|:-----|:-----|:-----|
| MemberEmail | Edm.String   Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true" |  否  | 组的电子邮件地址。 |
| 状态      | Edm.String   Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true" |  否  | 目前尚未填充。 |

### <a name="sharinginformationtype-complex-type"></a>SharingInformationType 复杂类型

|**参数**|**类型**|**强制？**|**说明**|
|:-----|:-----|:-----|:-----|
| RecipientEmail    | Edm.String   Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true" |  否  | 共享邀请的收件人的电子邮件地址。 |
| RecipientName    | Edm.String   Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true" |  否  | 共享邀请的收件人的名称。 |
| ResharePermission | Edm.String   Term="Microsoft.Office.Audit.Schema.PIIFlag" Bool="true" |  否  | 授予此收件人的权限。 |