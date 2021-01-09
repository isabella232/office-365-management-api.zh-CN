---
ms.technology: o365-service-communications
ms.TocTitle: Troubleshooting the Office 365 Management Activity API
title: Office 365 管理活动 API 疑难解答
description: 汇总 Microsoft 支持部门在提供此 API 支持方面所收到的最常见问题。
ms.ContentId: 50822603-a1ec-a754-e7dc-67afe36bb1b0
ms.topic: reference (API)
ms.date: ''
localization_priority: Priority
ms.openlocfilehash: 9c909220d660e0202c3ebda2777b2d8922da45a3
ms.sourcegitcommit: c3bb30b86a4569e9f18891f1cdc30cbffc8c8db4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2021
ms.locfileid: "49784205"
---
# <a name="office-365-management-activity-api-faqs-and-troubleshooting"></a>Office 365 管理活动 API 常见问题和疑难解答

Office 365 管理活动 API（也称为 *统一审核 API*）是 Office 365 安全与合规性产品的一部分，它：

- 允许以编程方式访问多个审核管道工作负载（例如 SharePoint 和 Exchange）

- 是各种第三方供应商产品使用的主要接口，用于聚合审核数据并对其编制索引

不应将管理活动 API 与 Office 365 服务通信 API 混淆。 管理活动 API 用于审核各种工作负载中的最终用户活动。 服务通信 API 用于审核由 Office 365 中的可用服务（例如 Dynamics CRM 或标识服务）发送的状态和消息。
 
> [!NOTE]
> 属于审核的事件存在问题。无法获取 2020 年 10 月 22 日至 11 月 6 日通过 Office 365 管理活动 API 提供的 AzureActiveDirectory 内容类型。 Azure AD 登录事件不会受此问题影响。 将在未来几天内提供影响期限的缺失事件，预计将于 2020 年 11 月 20 日前完成。 在某些情况下，客户可能会发现在 2020 年 10 月 26 日至 11月 5 日之间生成的事件的重复事件数据。

## <a name="frequently-asked-questions-about-the-office-365-management-activity-api"></a>有关 Office 365 管理活动 API 的常见问题解答

**如何上手使用管理活动 API？**

若要开始使用 Office 365 管理活动 API，请参阅 [Office 365 管理 API 入门](get-started-with-office-365-management-apis.md)。

**如果我对 Office 365 组织禁用审核，会发生什么情况？我是否仍然可以通过管理活动 API 获取事件？**

否。 必须为组织启用 Office 365 统一审核，才能通过管理活动 API 拉取记录。 有关说明，请参阅[启用或禁用审核日志搜索](https://docs.microsoft.com/microsoft-365/compliance/turn-audit-log-search-on-or-off)。

**要对特定 Office 365 服务审核什么事件？**

“Office 365 管理活动 API 架构”一文列出了全部事件。 有关详细信息，请参阅 Office 365 管理活动 API 架构。 另请参阅[在安全与合规中心内搜索审核日志](https://docs.microsoft.com/microsoft-365/compliance/search-the-audit-log-in-security-and-compliance#audited-activities)中的“已审核的活动”部分，以获取大多数已审核的 Office 365 服务的事件列表。

**管理活动 API 提取的记录与使用 Microsoft 365 合规中心内的审核日志搜索工具返回的记录是否有何不同？**

这两种方法返回的数据是相同的。 唯一的区别在于使用 API 只能获取过去 7 天的数据（下面的问题中提供了更多详细信息）。 在安全与合规中心内搜索审核日志（或在 Exchange Online PowerShell 中使用相应的 **Search-unifiedauditlog** cmdlet），可以获取生成数据时生效的保持期（对于例如 90 天或一年）的数据。

**在发送有关给定 Office 365 事件的通知之前，我必须等待的最长时间是多长？**

不存在通知发送的最长保证延迟（换句话说，没有 SLA）。 通常情况下，大多数通知在事件发生后的一个小时内发送。 通常，延迟要短得多，但可能比一小时更长，因为时间因工作负载而异。

**Webhook 通知不是更直接吗？**

不是。 最近，与直接使用 `/content` 操作查询 API 相比，使用 Webhook 等待通知的时间更长。

**可通过 API 获取内容的时长：**

在通知内容可用后的 7 天内，都可以通过 API 获取内容。 即使通知延迟了很长的时间（例如服务中断的情况），你仍然可以在第一次收到通知后的 7 天内下载与原始事件相关的内容 blob。

**我可以在管理活动 API 中查询内容 blob 中的特定事件 ID、RecordType 或其他属性吗？**

否。 管理活动 API 提供特定日志的所有事件详细信息。 可以使用它来下载所有详细信息，然后你可以对下载的数据实施自己的查询逻辑；例如，通过使用自定义应用程序或第三方工具。

**我如何知道来自我现有审核解决方案（该解决方案从管理活动 API 收集数据）的数据是否准确完整？**

简单说，Microsoft 不提供允许你交叉检查任何给定应用程序或第三方 (ISV) 应用程序的任何类型的日志。 还有一些其他 Microsoft 安全产品可以从相同管道获取数据，但这些产品不属于本次讨论的范围，不能用于直接交叉检查管理活动 API。 如果担心现有解决方案提供的内容与预期内容之间存在差异，则应实施上述操作。 但这可能很困难，具体取决于你现有的工具或解决方案如何列出数据并对其编制索引。 如果现有解决方案仅显示按实际事件的创建时间排序的数据，则无法按事件创建时间查询 API，进而比较结果集。 在这种情况下，你必须连续数天收集通知的内容 blob，手动对它们编制索引或排序，然后进行手动比较。

**管理活动 API 的限制是什么？**

所有组织最初每分钟分配 2000 个请求基线。 然后，根据各种因素（包括组织中的席位数量）组合来调整限流。 此外，Office 365 E5 和 Microsoft 365 E5 组织获得的带宽是非 E5 组织的约两倍。 这也是最大宽带的上限，以保护服务的健康。

> [!NOTE]
> 尽管每个租户最初每分钟最多可提交 2000 个请求，Microsoft 也无法保证响应速率。 响应速率取决于各种因素，如客户端系统性能、网络容量和网络速度。

**我在使用管理活动 API 时遇到了限制错误。该怎么操作？**

开启 Microsoft 支持票证，申请新限制，并添加提高限制的正当业务理由。 我们将会评估申请，如果接受，便会提高限制。

**TargetUpdatedProperties 为何不再位于 Azure Active Directory 活动审核日志的 ExtendedProperties 中？**

TargetUpdatedProperties 显示在 ExtendedProperties 中。 但是，它们将从 ExtendedProperties 中删除，并且现在将显示在 ModifiedProperties 中。

**为什么 Azure Active Directory (Azure AD) 登录活动的 UserAccountNotFound 错误的审核日志不能通过管理活动 API 使用？**

从 2020 年 11 月开始，Azure AD 登录活动的审核日志将从 Azure AD 事件中心引入到统一审核日志中。 由于 UserAccountNotFound 登录错误在事件集线器中不可用，因此管理活动 API 不再返回 UserAccountNotFound 错误的审核日志。

## <a name="troubleshooting-the-office-365-management-activity-api"></a>Office 365 管理活动 API 疑难解答

对于任何开始使用 Office 365 管理活动 API 的人来说，应该明确的一点是，没有按事件细节（例如事件发生的日期、可能触发事件的网站集或者事件类型）进行查询的概念。 相反，而是创建对特定工作负载（例如，SharePoint 或 Azure AD）的订阅，并且每个租户对应一个订阅。

以下各节概述了使用 Office 365 管理活动 API 的客户最常见问题：

- [关于第三方工具和客户端的问题](#questions-about-third-party-tools-and-clients)

- [在 Office 365 中启用统一审核日志记录](#enabling-unified-audit-logging-in-office-365)

- [连接到 API](#connecting-to-the-api)

- [检查订阅](#checking-your-subscriptions)

- [创建新订阅](#creating-a-new-subscription)

- [使用 webhook](#using-webhooks)

- [请求内容 blob 和限制](#requesting-content-blobs-and-throttling)

我们将展示一系列简单的 PowerShell 脚本，可以帮助你回答客户提出的最常见问题，或者通过演示主要操作开始实施自定义解决方案。 并非所有操作都在这些章节中做了相关解释，但它们在 Office 365 管理活动 API 参考中均有列出。

### <a name="questions-about-third-party-tools-and-clients"></a>关于第三方工具和客户端的问题

最常见问题类别来自使用第三方产品下载和汇总审核数据的客户。 根据第三方产品，客户可能会遇到设置问题或这些产品中暴露的数据出现中断或不一致的情况。 这里应该指出，此类客户首先应采取的措施是联系其供应商的支持部门。 通常情况下，特定租户的供应商配置或服务问题就是客户投诉的原因。

### <a name="enabling-unified-audit-logging-in-office-365"></a>在 Office 365 中启用统一审核日志记录

如果你刚刚设置了一个正在尝试使用管理活动 API 的应用程序，但它无法正常工作，请确保已针对你的 Office 365 组织启用统一审核日志记录。 可通过启用 Office 365 审核日志来实现此操作。 有关说明，请参阅[打开或关闭 Office 365 审核日志搜索](https://docs.microsoft.com/office365/securitycompliance/turn-audit-log-search-on-or-off)。

如果未启用统一审核，则通常会收到包含以下字符串的错误：`Microsoft.Office.Compliance.Audit``.DataServiceException: Tenant <tenantID> does not exist.`

### <a name="connecting-to-the-api"></a>连接到 API

大多数应用程序使用简单直观的客户端凭据 OAuth2 流连接到 API。 因此，第一步是创建一个 Azure AD 应用，该应用具有访问管理活动 API 数据所需的权限。 介绍进行 Azure AD 应用注册的步骤不在本文的探讨范围内。 有关详细信息，请参阅[通过 Azure Active Directory 租户注册应用程序](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)。

#### <a name="azure-application-permissions"></a>Azure 应用程序权限

当前用于 Office 365 管理活动 API 的三个权限是：

1. 为组织读取活动数据

2. 为组织读取服务运行状况信息

3. 读取数据丢失防护 (DLP) 策略事件，包括检测到的敏感信息

> [!NOTE]
> 你应至少为上述权限集的前两个启用“应用程序权限”和“委派权限”。 仅在你对 DLP 工作负载感兴趣时才需要读取 DLP 策略事件。

#### <a name="getting-an-access-token"></a>获取访问令牌

以下 PowerShell 脚本使用应用 ID 和客户端密码从管理活动 API 身份验证端点获取 OAuth2 令牌。 然后，它将访问令牌放置到 `$headerParams` 数组变量，该变量将附加到 HTTP 请求中。 对于 API 终结点的值（在 $resource 变量中），请使用基于组织的 Microsoft 365 或 Office 365 订阅计划的以下任一值:

- 企业版计划：`manage.office.com`

- GCC 政府版计划：`manage-gcc.office.com`

- GCC 高级政府版计划: `manage.office365.us`

- DoD 政府版计划: `manage.protection.apps.mil`

```powershell
# Create app of type Web app / API in Azure AD, generate a Client Secret, and update the client id and client secret here
$ClientID = "<YOUR_APPLICATION_ID"
$ClientSecret = "<YOUR_CLIENT_SECRET>"
$loginURL = "https://login.microsoftonline.com/"
$tenantdomain = "<YOUR_DOMAIN>.onmicrosoft.com"
# Get the tenant GUID from Properties | Directory ID under the Azure Active Directory section. For $resource, use one of these endpoint values based on your subscription plan: Enterprise - manage.office.com; GCC - manage-gcc.office.com; GCC High: manage.office365.us; DoD: manage.protection.apps.mil
$TenantGUID = "<YOUR_TENANT_GUID>"
$resource = "https://<YOUR_API_ENDPOINT>"
# auth
$body = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
$oauth = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body
$headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"} 
```

`$oauth` 变量将包含响应对象，该对象具有多个属性，包括访问令牌。 响应示例如下：

```json
token_type     : Bearer
expires_in     : 3599
ext_expires_in : 0
expires_on     : 1508285860
not_before     : 1508281960
resource       : https://manage.office.com
access_token   : eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6IjJLVmN1enFBaWRPTHFXU2FvbDd3Z0ZSR0NZbyIsImtpZCI6IjJLVmN1enFBaWRPTHFXU2FvbDd3Z0ZSR0NZbyJ9.eyJhdWQiOiJodHRwczov…
```

### <a name="checking-your-subscriptions"></a>检查订阅

如果出现到现有管理活动 API 客户端或解决方案的数据流中断的问题，你可能想知道订阅是否出现某些问题。 要检查活动的订阅，请将以下内容添加到上一个脚本：

```powershell
Invoke-WebRequest -Headers $headerParams -Uri "$resource/api/v1.0/$tenantGUID/activity/feed/subscriptions/list" 
```

#### <a name="sample-response"></a>示例响应

```json
StatusCode        : 200
Status Description : OK
Content           : [{"contentType":"Audit.Exchange","status":"enabled","webhook":null},{"contentType":"Audit.SharePoint","status":"enabled","webhook":{"authId":"","address":"https://mvcwebapiwebhookreceiver.azurewebsite...
RawContent        : HTTP/1.1 200 OK
                    Pragma: no-cache
                    Content-Length: 266
                    Cache-Control: no-cache
                    Content-Type: application/json; charset=utf-8
                    Expires: -1
                    Server: Microsoft-IIS/8.5,Microsoft-IIS/8.5
                    X-AspNet-Versi...
Forms             : {}
Headers           : {[Pragma, no-cache], [Content-Length, 266], [Cache-Control, no-cache], [Content-Type, application/json; charset=utf-8]...}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 266
```

这表示租户已启用 Audit.Exchange 和 Audit.SharePoint 订阅。 Exchange 订阅未启用 webhook (null)，而 SharePoint 订阅已启用 webhook，并显示已注册端点的地址。

### <a name="creating-a-new-subscription"></a>创建新订阅

要创建新订阅，请使用 /start 操作。 对于 API 终结点，请使用基于订阅计划的以下任一值:

- 企业版计划：`manage.office.com`

- GCC 政府版计划：`manage-gcc.office.com`

- GCC 高级政府版计划: `manage.office365.us`

- DoD 政府版计划: `manage.protection.apps.mil`

```powershell
Invoke-WebRequest -Method Post -Headers $headerParams -Uri "https://<YOUR_API_ENDPOINT>/api/v1.0/$tenantGUID/activity/feed/subscriptions/start?contentType=Audit.AzureActiveDirectory"

> [!NOTE]
> Remember that `$headerParams` was populated in the first part of the script listed in the [Connecting to the API](#connecting-to-the-api) section in this article.

The previous code will create a new subscription to the Audit.AzureActiveDirectory content type, with a webhook that is null. You can then check your subscriptions using the code in the [Checking your subscriptions](#checking-your-subscriptions) section in this article.

## Checking content availability

To check what content blobs were created during a certain period, you can add the following line to the script in the “Connecting to the API” section.

```powershell
Invoke-WebRequest -Method GET -Headers $headerParams -Uri "$resource/api/v1.0/$tenantGUID/activity/feed/subscriptions/content?contentType=Audit.SharePoint"
```

前面的示例将获取今天可用的所有内容通知，即从 UTC 时间中午 12:00 到当前时间。 如果要指定不同的时间段（请记住，可以查询的最长时间段为 24 小时），请将 *starttime* 和 *endtime* 参数添加到 URI 中，例如：

```powershell
Invoke-WebRequest -Method GET -Headers $headerParams -Uri "$resource/api/v1.0/$tenantGUID/activity/feed/subscriptions/content?contentType=Audit.SharePoint&startTime=2017-10-13T00:00&endTime=2017-10-13T11:59"
```

> [!NOTE]
> 要么必须同时使用 *starttime* 和 *endtime* 参数，要么两个参数都不使用。

```json
[{      "contentUri" : "https://<your_API_endpoint>/api/v1.0/<your_tenant_guid>/activity/feed/audit/20171014180051748005825$20171014180051748005825$audit_sharepoint$Audit_SharePoint",
        "contentId" : "20171014180051748005825$20171014180051748005825$audit_sharepoint$Audit_SharePoint",
        "contentType" : "Audit.SharePoint",
        "contentCreated" : "2017-10-13T18:00:51.748Z",
        "contentExpiration" : "2017-10-20T18:00:51.748Z",
}]
```

> [!IMPORTANT]
>
> - *contentUri* 属性是你可以从中检索内容 blob 的 URI。 blob 本身包含事件详细信息，它将包含有关 1 - N 事件的详细信息。 虽然集合中可能有 30 个 JSON 对象，但在这 30 个内容 URI 中可能详述了更多事件。
>
> - *contentCreated* 属性不是创建所通知的事件的日期， 而是创建通知的日期。 可以在创建内容 blob 之前创建该 blob 中详述的事件。 因此，永远不能直接查询任何给定时间段内发生的事件的 API。

#### <a name="paging-contents-for-busy-tenants"></a>繁忙租户的分页内容

许多较大的 Office 365 租户每小时都会生成数千个事件。 如果你的组织就是这种情况，并且你尝试执行上述示例中的 24 小时周期查询，则可能需要检索比一个响应中返回的通知更多的通知。 在这种情况下，需要实现某种逻辑循环，每次都检查 **NextPageUrl:** 标头值的响应头。 有关详细信息，请参阅 Office 365 管理活动 API 参考中的分页部分。

```powershell
IF the NextPageUrl header is present in the response... 

THEN issue a new request substituting the Uri parameter in the above Invoke-WebRequest example for the value of the NextPageUrl header...
 
ELSE exit the loop
```

除非你有一个非常活跃的租户，否则很难测试此循环代码。 在测试中，我们尝试在脚本中执行数千次更新操作，并且无法生成足够多数量的通知以要求发送 **NextPageUrl** 标头。

### <a name="using-webhooks"></a>使用 webhook

有两种方法可以获得已创建内容 blob 的通知。 *推送* 方法是使用 webhook 端点实现的，该端点是你自己创建和托管的 Web 应用程序或云平台上的 Web 应用程序。 在创建针对已审核内容类型的订阅时注册 webhook。 还可以使用下面显示的方法向现有订阅添加 webhook 注册。 *拉取* 方法要求使用 /content 操作查询特定时间段（不超过 24 小时）。 该响应将告诉你在指定时间段内创建了哪些内容 blob。

在添加 webhook 之前，请注意以下两个问题：

1. 由于调试和疑难解答过程存在困难，因此 Microsoft 不再强调 Webhook。 通常，你应该托管响应传入请求的 Web API 端点。 许多客户使用托管环境（他们没有完全控制权限）或本地环境（难以允许传入的 HTTP 请求）。 支持部门已经发现许多问题，其中来自 Office 365 审核管道的传入请求已被防火墙或路由器阻止。 在这种情况下，API 将实现退避算法，这可能会令人困惑，并可能导致禁用订阅中的 webhook。

2. 执行启动操作后，webhook 必须准备好立即响应验证请求。 如果 webhook 应用程序中存在错误，则验证将失败，并且不会启用 webhook。 有关验证请求架构的信息，请参阅 Office 365 管理活动 API 参考中的 Webhook 验证部分。 你应该认真考虑生成生产就绪型 webhook 以响应管理活动 API 通知所面临的挑战。

如果你决定实现 webhook，则应对其进行测试，以验证在首次调用指定 webhook 端点的任何 /start 操作之前，端点是否已准备好响应针对验证请求和正常通知请求的 HTTP 200 响应。 通常，你将使用来自 API 的模拟请求来测试此准备情况。 仔细阅读并遵守 Office 365 管理活动 API 参考中的 Webhook 验证和检索内容部分，以了解这两种请求类型的架构。

要使用 webhook 端点添加订阅，请将 POST 的正文参数添加到 /start 端点，例如：

```json
$body = '{
    "webhook" : {
        "address": "https://webhook.myapp.com/o365/",
        "authId": "o365activityapinotification",
        "expiration": ""
    }}'
$uri = "https://manage.office.com/api/v1.0/$tenantGUID/activity/feed//subscriptions/start?contentType=Audit.SharePoint"
Invoke-RestMethod -Method Post -uri $uri -Headers $headerParams -Body $body
```

在此调用之后，将立即向 `https://webhook.myapp.com/o365/ …` 发送验证请求，并且应根据 Office 365 管理活动 API 参考中的 Webhook 验证部分中的说明准备好用于响应的侦听器。 侦听器必须通过 HTTP 200 进行响应。 如果此时立即运行 /list 操作，则在验证成功返回状态之前，只能看到显示为 null 的 webhook。

#### <a name="checking-notifications-to-webhooks"></a>检查 webhook 通知

能够区分 /notifications 操作和 /content 操作很重要。 仅当通过 webhook 端点进行订阅时才需要检查通知。 此操作让你了解尝试向 webhook 发送的通知是否成功。 此操作不应用于列出可用内容。 要根据 webhook 中已收到内容交叉检查内容的可用性，你可以使用 /content 操作。 但请务必先使用 /notifications 操作检查失败的通知。

### <a name="requesting-content-blobs-and-throttling"></a>请求内容 blob 和限制

获取内容 URI 列表后，必须请求 URI 指定的 blob。 下面是使用 PowerShell 请求内容 Blob（使用适用于企业组织的 manage.office.com API 终结点）的示例。 此示例假定你已使用本文[获取访问令牌](#getting-an-access-token)部分中的上一个示例获取访问令牌并已正确填充 `$headerParams` 变量。

```powershell
# Get a content blob
$uri = 'https://manage.office.com/api/v1.0/<<your-tenant-guid>>/activity/feed/audit/<<ContentID>$audit_sharepoint$Audit_SharePoint'
$contents = Invoke-WebRequest -Method GET -Headers $headerParams -Uri $uri
```

请注意上一个示例中有关 *$uri* 变量的以下内容：

- 我们使用单引号，以便 *$* 符号不会被解译为 PowerShell 中的变量

- 整个 URI 将在针对你上次调用 /content 端点的响应的 *contentUri* 属性中返回。 显示的占位符令牌仅用于说明。

在尝试检索可用内容 blob 时，许多客户（大多数与繁忙租户合作）遇到如下错误：

```json
Response Code 403: {'error':{'code':'AF429','message':'Too many requests. Method=GetBlob, PublisherId=00000000-0000-0000-0000-000000000000'}}
```

这可能因限制所致。 请注意，Publisherid 参数的值可能表示客户端未在请求中指定 *PublisherIdentifier*。 此外，请记住，正确的参数名称是 *PublisherIdentifier*，即使你在 403 错误响应中看到列出 *PublisherId* 也是如此。

> [!NOTE]
> 在 API 参考中，在 API 的每个操作中均会列出 *PublisherIdentifier* 参数，但在检索内容 blob 时，它也应包含在针对 contentUri URL 的 GET 请求中。

如果你正在进行简单的 API 调用来解决问题（例如，检查给定订阅是否处于活动状态），则可以安全地省略 *PublisherIdentifier* 参数，但在每次调用时，任何最终用于生产用途的代码绝对都应该包括 *PublisherIdentifier* 参数。

如果你正在为公司的租户实现客户端，则 *PublisherIdentifier* 是租户 GUID。 如果要为多个客户创建 ISV 应用程序或外接程序，则 *PublisherIdentifier* 应该是 ISV 的租户 GUID，而不是最终用户公司的租户 GUID。

如果包含有效的 *PublisherIdentifier*，那么你将进入一个池，其中每分钟为每个租户分配 6 万个请求。 请求的量是极大的。 但是，如果不包含 *PublisherIdentifier* 参数，则你将进入每分钟为所有租户分配 6 万个请求的常规池。 在这种情况下，你很可能会发现调用受到限制。 为了防止出现这种情况，以下是使用 *PublisherIdentifier* 请求内容 blob 的方法：

```json
$contentUri = ($response.Content | ConvertFrom-Json).contentUri[0]
$uri = $contentUri + '?PublisherIdentifier=82b24b6d-0591-4604-827b-705d55d0992f'
$contents = Invoke-WebRequest -Method GET -Headers $headerParams -Uri $uri
```

上一个示例假定 *$response* 变量填充了请求 /content 端点的响应，并且 *$headerParams* 变量包含有效的访问令牌。 该脚本从响应中捕捉内容 URI 数组中的第一项，然后调用 GET 下载该 blob，并将其放入 *$contents* 变量中。 代码可能会遍历 contentUri 集合，针对每个 *contentUri* 发出 GET。
