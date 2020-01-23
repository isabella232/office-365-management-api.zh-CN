---
ms.TocTitle: Troubleshooting the Office 365 Management Activity API
title: Office 365 管理活动 API 疑难解答
description: 汇总 Microsoft 支持部门在提供此 API 支持方面所收到的最常见问题。
ms.ContentId: 50822603-a1ec-a754-e7dc-67afe36bb1b0
ms.topic: reference (API)
ms.date: ''
localization_priority: Priority
ms.openlocfilehash: b751c89194407e57c8654a9317b8070ab2918b03
ms.sourcegitcommit: 36d0167805d24bbb3e2cf1a02d0f011270cc31cb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2020
ms.locfileid: "41263287"
---
# <a name="troubleshooting-the-office-365-management-activity-api"></a>Office 365 管理活动 API 疑难解答

Office 365 管理活动 API（也称为*统一审核 API *）只是 Office 365 安全与合规性产品的一部分，但它却备受关注，原因是它：

- 允许以编程方式访问多个审核管道工作负载（例如 SharePoint 和 Exchange）
- 是各种第三方供应商产品使用的主要接口，用于聚合审核数据并对其编制索引

不应将管理活动 API 与 Office 365 服务通信 API 混淆。 管理活动 API 用于审核各种工作负载中的最终用户活动。  服务通信 API 用于审核由 Office 365 中的可用服务（例如 Dynamics CRM 或标识服务）发送的状态和消息。

尽管相对而言操作较少且 REST 接口简单，但是对关于如何使用管理活动 API 以及如何检索数据的确切详细信息存在很多困惑。  对于任何开始使用管理活动 API 的人来说，应该明确的一点是，没有按事件细节（例如事件发生的日期、可能触发事件的网站集或者事件类型）进行查询的概念。  相反，而是创建对特定工作负载（例如，SharePoint 或 Azure AD）的订阅，并且每个租户对应一个订阅。

本文汇总了 Microsoft 支持部门在提供此 API 支持方面所收到的最常见问题。  我们将展示一系列简单的 PowerShell 脚本，可以帮助你回答客户提出的最常见问题，或者通过演示主要操作开始实施自定义解决方案。  并非所有操作都在本文中做了相关解释，但它们在 [Office 365 管理活动 API 参考](office-365-management-activity-api-reference.md)中均有列出。

## <a name="questions-about-third-party-tools-and-clients"></a>关于第三方工具和客户端的问题

我们目前支持的最常见问题类别来自使用第三方产品下载和汇总审核数据的客户。 根据第三方产品，客户可能会遇到设置问题或这些产品中暴露的数据出现中断或不一致的情况。 这里应该指出，此类客户首先应采取的措施是联系其供应商的支持部门。 在支持的所有服务请求中，工程师只看到一个案例，其中特定于租户的服务问题是原因。

然而，这些客户可能仍有一些未得到解答的问题。 他们的供应商可能会坚持认为这是服务问题，或者他们可能只是想在联系供应商之前进行一些初步检查。 

## <a name="enabling-unified-audit-logging-in-office-365"></a>在 Office 365 中启用统一审核日志记录

如果你刚刚设置了一个正在尝试使用管理活动 API 的应用程序，但它无法正常工作，请确保已针对你的 Office 365 组织启用统一审核日志记录。 可通过启用 Office 365 审核日志来实现此操作。 有关说明，请参阅[打开或关闭 Office 365 审核日志搜索](https://docs.microsoft.com/office365/securitycompliance/turn-audit-log-search-on-or-off)。

如果未启用统一审核，则通常会收到包含以下字符串的错误：`Microsoft.Office.Compliance.Audit.DataServiceException: Tenant <tenantID> does not exist.`

## <a name="connecting-to-the-api"></a>连接到 API

大多数应用程序使用简单直观的客户端凭据 OAuth2 流连接到 API。 因此，第一步是创建一个 Azure AD 应用，该应用具有访问管理活动 API 数据所需的权限。 介绍进行 Azure AD 应用注册的步骤不在本文的探讨范围内。 有关详细信息，请参阅[通过 Azure Active Directory 租户注册应用程序](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)。

### <a name="azure-application-permissions"></a>Azure 应用程序权限

当前用于 Office 365 管理活动 API 的三个权限是：

1. 为组织读取活动数据
2. 为组织读取服务运行状况信息
3. 读取数据丢失防护 (DLP) 策略事件，包括检测到的敏感信息 

> [!NOTE] 
> 你应至少为上述权限集的前两个启用“应用程序权限”和“委派权限”。 仅在你对 DLP 工作负载感兴趣时才需要读取 DLP 策略事件。

### <a name="getting-an-access-token"></a>获取访问令牌

以下 PowerShell 脚本使用应用 ID 和客户端密码从管理活动 API 身份验证端点获取 OAuth2 令牌。 然后，它将访问令牌放置到 `$headerParams` 数组变量，该变量将附加到 HTTP 请求中： 

```powershell
# Create app of type Web app / API in Azure AD, generate a Client Secret, and update the client id and client secret here
$ClientID = "<YOUR_APPLICATION_ID"
$ClientSecret = "<YOUR_CLIENT_SECRET>"
$loginURL = "https://login.microsoftonline.com/"
$tenantdomain = "<YOUR_DOMAIN>.onmicrosoft.com"
# Get the tenant GUID from Properties | Directory ID under the Azure Active Directory section
$TenantGUID = "<YOUR_TENANT_GUID>"
$resource = "https://manage.office.com"
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

## <a name="checking-your-subscriptions"></a>检查订阅

如果出现到现有管理活动 API 客户端或解决方案的数据流中断的问题，你可能想知道订阅是否出现某些问题。 要检查活动的订阅，请将以下内容添加到上一个脚本：

```powershell
Invoke-WebRequest -Headers $headerParams -Uri "https://manage.office.com/api/v1.0/$tenantGUID/activity/feed/subscriptions/list" 
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

## <a name="creating-a-new-subscription"></a>创建新订阅

要创建新订阅，请使用 /start 操作：

```powershell
Invoke-WebRequest -Method Post -Headers $headerParams -Uri "https://manage.office.com/api/v1.0/$tenantGUID/activity/feed/subscriptions/start?contentType=Audit.AzureActiveDirectory"
```

> [!NOTE] 
> 请记住，在本文的[连接到 API](#connecting-to-the-api)部分列出的脚本的第一部分中填充了 `$headerParams`。

前面的代码将创建一个针对 Audit.AzureActiveDirectory 内容类型的新订阅，其中 webhook 为 null。 然后，你可以使用本文[检查订阅](#checking-your-subscriptions)部分中的代码检查你的订阅。

## <a name="checking-content-availability"></a>检查内容可用性

要检查在特定时间段内创建了哪些内容 blob，可以在“连接到 API”部分中将以下行添加到脚本中：

```powershell
Invoke-WebRequest -Method GET -Headers $headerParams -Uri "https://manage.office.com/api/v1.0/$tenantGUID/activity/feed/subscriptions/content?contentType=Audit.SharePoint"
```

前面的示例将获取今天可用的所有内容通知，即从 UTC 时间中午 12:00 到当前时间。 如果要指定不同的时间段（请记住，可以查询的最长时间段为 24 小时），请将 *starttime* 和 *endtime* 参数添加到 URI 中，例如：

```powershell
Invoke-WebRequest -Method GET -Headers $headerParams -Uri "https://manage.office.com/api/v1.0/$tenantGUID/activity/feed/subscriptions/content?contentType=Audit.SharePoint&startTime=2017-10-13T00:00&endTime=2017-10-13T11:59"
```

> [!NOTE] 
> 要么必须同时使用 *starttime* 和 *endtime* 参数，要么两个参数都不使用。

上一个请求将返回一个 JSON 对象，其中包含在你所指定的时间段内可用的通知集合。 响应如下所示：

```json
[{      "contentUri" : "https://manage.office.com/api/v1.0/<<your_tenant_guid>>/activity/feed/audit/20171014180051748005825$20171014180051748005825$audit_sharepoint$Audit_SharePoint",
        "contentId" : "20171014180051748005825$20171014180051748005825$audit_sharepoint$Audit_SharePoint",
        "contentType" : "Audit.SharePoint",
        "contentCreated" : "2017-10-13T18:00:51.748Z",
        "contentExpiration" : "2017-10-20T18:00:51.748Z",
}]
```

> [!IMPORTANT] 
> - *contentUri* 属性是你可以从中检索内容 blob 的 URI。 blob 本身包含事件详细信息，它将包含有关 1 - N 事件的详细信息。 虽然集合中可能有 30 个 JSON 对象，但在这 30 个内容 URI 中可能详述了更多事件。
> - *contentCreated* 属性不是创建所通知的事件的日期， 而是创建通知的日期。 可以在创建内容 blob 之前创建该 blob 中详述的事件。 因此，永远不能直接查询任何给定时间段内发生的事件的 API。

### <a name="paging-contents-for-busy-tenants"></a>繁忙租户的分页内容

许多较大的 Office 365 租户每小时都会生成数千个事件。 如果你的组织就是这种情况，并且你尝试执行上述示例中的 24 小时周期查询，则可能需要检索比一个响应中返回的通知更多的通知。 在这种情况下，需要实现某种逻辑循环，每次都检查 **NextPageUrl:** 标头值的响应头。 有关详细信息，请参阅 Office 365 管理活动 API 参考中的[分页](office-365-management-activity-api-reference.md#pagination)部分。 

此循环的逻辑将如下所示：

```powershell
IF the NextPageUrl header is present in the response... 

THEN issue a new request substituting the Uri parameter in the above Invoke-WebRequest example for the value of the NextPageUrl header...
 
ELSE exit the loop
```

除非你有一个非常活跃的租户，否则很难测试此循环代码。 在测试中，我们尝试在脚本中执行数千次更新操作，并且无法生成足够多数量的通知以要求发送 **NextPageUrl** 标头。

## <a name="using-webhooks"></a>使用 webhook

有两种方法可以获得已创建内容 blob 的通知。 *推送*方法是使用 webhook 端点实现的，该端点是你自己创建和托管的 Web 应用程序或云平台上的 Web 应用程序。 在创建针对已审核内容类型的订阅时注册 webhook。 还可以使用下面显示的方法向现有订阅添加 webhook 注册。 *拉取*方法要求使用 [/content 操作](office-365-management-activity-api-reference.md#list-available-content)查询特定时间段（不超过 24 小时）。 该响应将告诉你在指定时间段内创建了哪些内容 blob。

在添加 webhook 之前，请注意以下两个问题：

1. 由于调试和疑难解答过程存在困难，因此 Microsoft 不再强调 Webhook。 通常，你应该托管响应传入请求的 Web API 端点。 许多客户使用托管环境（他们没有完全控制权限）或本地环境（难以允许传入的 HTTP 请求）。 支持部门已经发现许多问题，其中来自 Office 365 审核管道的传入请求已被防火墙或路由器阻止。 在这种情况下，API 将实现退避算法，这可能会令人困惑，并可能导致禁用订阅中的 webhook。

2. 执行启动操作后，webhook 必须准备好立即响应验证请求。 如果 webhook 应用程序中存在错误，则验证将失败，并且不会启用 webhook。 有关验证请求架构的信息，请参阅 Office 365 管理活动 API 参考中的 [Webhook 验证](office-365-management-activity-api-reference.md#webhook-validation)部分。 你应该认真考虑生成生产就绪型 webhook 以响应管理活动 API 通知所面临的挑战。

如果你决定实现 webhook，则应对其进行测试，以验证在首次调用指定 webhook 端点的任何 /start 操作之前，端点是否已准备好响应针对验证请求和正常通知请求的 HTTP 200 响应。 通常，你将使用来自 API 的模拟请求来测试此准备情况。 仔细阅读并遵守 Office 365 管理活动 API 参考中的 [Webhook 验证](office-365-management-activity-api-reference.md#webhook-validation)和[检索内容](office-365-management-activity-api-reference.md#retrieving-content)部分，以了解这两种请求类型的架构。

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

在此调用之后，将立即向 `https://webhook.myapp.com/o365/ …` 发送验证请求，并且应根据 Office 365 管理活动 API 参考中的 [Webhook 验证](office-365-management-activity-api-reference.md#webhook-validation)部分中的说明准备好用于响应的侦听器。 侦听器必须通过 HTTP 200 进行响应。 如果此时立即运行 /list 操作，则在验证成功返回状态之前，只能看到显示为 null 的 webhook。

### <a name="checking-notifications-to-webhooks"></a>检查 webhook 通知

能够区分 /notifications 操作和 /content 操作很重要。 仅当通过 webhook 端点进行订阅时才需要检查通知。 此操作让你了解尝试向 webhook 发送的通知是否成功。 此操作不应用于列出可用内容。 要根据 webhook 中已收到内容交叉检查内容的可用性，你可以使用 /content 操作。 但请务必先使用 [/notifications 操作](office-365-management-activity-api-reference.md#list-notifications)检查失败的通知。

## <a name="requesting-content-blobs-and-throttling"></a>请求内容 blob 和限制

获取内容 URI 列表后，必须请求 URI 指定的 blob。 以下是使用 PowerShell 请求内容 blob 的示例。 此示例假定你已使用本文[获取访问令牌](#getting-an-access-token)部分中的上一个示例获取访问令牌并已正确填充 `$headerParams` 变量。

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

```powershell
$contentUri = ($response.Content | ConvertFrom-Json).contentUri[0]
$uri = $contentUri + '?PublisherIdentifier=82b24b6d-0591-4604-827b-705d55d0992f'
$contents = Invoke-WebRequest -Method GET -Headers $headerParams -Uri $uri
```

上一个示例假定 *$response* 变量填充了请求 /content 端点的响应，并且 *$headerParams* 变量包含有效的访问令牌。 该脚本从响应中捕捉内容 URI 数组中的第一项，然后调用 GET 下载该 blob，并将其放入 *$contents* 变量中。 代码可能会遍历 contentUri 集合，针对每个 *contentUri* 发出 GET。

## <a name="frequently-asked-questions-about-the-office-365-management-api"></a>有关 Office 365 管理 API 的常见问题解答

#### <a name="what-is-the-maximum-time-i-will-have-to-wait-before-a-notification-is-sent-about-a-given-office-365-event"></a>在发送有关给定 Office 365 事件的通知之前，我必须等待的最长时间是多长？

不存在通知发送的最长保证延迟（换句话说即没有 SLA）。 Microsoft 支持部门的经验是，大多数通知都是在事件发生后一小时内发送的。 延迟通常要短得多，但也经常会比一小时更长。 不同工作负载的延迟在一定程度上有所不同，但一般规则是大多数通知将在原始事件发生后的 24 小时内发送。

#### <a name="arent-webhook-notifications-more-immediate-after-all-arent-they-are-event-driven"></a>Webhook 通知不是更直接吗？ 毕竟，它们不是事件驱动的吗？

不是。 就事件触发通知层面而言，Webhook 通知并不是事件驱动的。 仍须创建内容 blob，且创建内容 blob 会触发通知发送。 最近，与直接使用 /content 操作查询 API 相比，使用 Webhook 等待通知的时间更长。 因此，不应将管理活动 API 视为实时安全警报系统。 Microsoft 有针对该系统的其他产品。 就安全性而言，管理活动 API 事件通知可能更适用于在长时间段内确定使用模式。 

#### <a name="can-i-query-the-management-activity-api-for-a-particular-event-id-or-recordtype-or-other-properties-in-the-content-blob"></a>我可以在管理活动 API 中查询内容 blob 中的特定事件 ID、RecordType 或其他属性吗？

不能。 不要将通过管理活动 API 提供的数据视为传统意义上的“日志”。 相反，将其视为事件详细信息的转储。 你可以通过使用自定义应用程序或第三方工具来收集所有这些事件详细信息，在本地存储并对它们编制索引，然后实现自己的查询逻辑。

#### <a name="how-do-i-know-the-data-coming-from-my-existing-auditing-solution-which-collects-data-from-the-management-activity-api-is-accurate-and-complete"></a>我如何知道来自我现有审核解决方案（该解决方案从管理活动 API 收集数据）的数据是否准确完整？

简单说，Microsoft 不提供任何类型的日志，允许你交叉检查任何给定的应用程序或第三方 (ISV) 应用程序。 还存在其他从相同管道提取数据的 Microsoft 安全产品，但这些产品不属于本次讨论的范围，不能用于直接交叉检查管理活动 API。 如果担心现有解决方案提供的内容与预期内容之间存在差异，则应实施上述操作。 但这可能很困难，具体取决于你现有的工具或解决方案如何列出数据并对其编制索引。 如果现有解决方案仅显示按实际事件的创建时间排序的数据，则无法按事件创建时间查询 API，进而你可以比较结果集。 在这种情况下，你必须连续数天收集通知的内容 blob，手动对它们编制索引或排序，然后进行粗略比较。

#### <a name="how-long-will-the-content-blobs-remain-available"></a>内容 blob 可以使用多长时间？

在通知内容 blob 可用后的 7 天内，内容 blob 都可用。 这意味着如果内容 blob 的创建存在明显延迟，则在内容 blob 不再可用之前，你会在实际事件创建日期之后有更多时间（延迟时间加 7 天）。

#### <a name="if-there-is-a-24-hour-delay-in-getting-a-notification-doesnt-that-mean-i-will-have-only-6-days-to-retrieve-the-content-blob"></a>如果通知延迟 24 小时才收到，这是不是意味着我只有 6 天的时间来检索内容 blob？

不是。 即使通知延迟了很长的时间（例如服务中断的情况），你仍然可以在第一次收到通知后的 7 天内下载与原始事件相关的内容 blob。












