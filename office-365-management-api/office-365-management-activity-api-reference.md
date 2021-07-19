---
ms.technology: o365-service-communications
ms.TocTitle: Office 365 Management Activity API reference
title: Office 365 管理活动 API 参考
description: 使用 Office 365 管理活动 API，可从 Office 365 和 Azure AD 活动日志中获取用户、管理员、系统和策略的操作和事件相关信息。
ms.ContentId: 52749845-37f8-6076-7ea5-49d9a4055445
ms.topic: reference (API)
ms.date: ''
localization_priority: Priority
ms.openlocfilehash: c8eb59433b49c9735ddfefea0d1e6804e8937439
ms.sourcegitcommit: f08ff7cfd17aedd9d2ca85b5df0666ca986c9aed
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2021
ms.locfileid: "53447896"
---
# <a name="office-365-management-activity-api-reference"></a>Office 365 管理活动 API 参考

使用 Office 365 管理活动 API，可从 Office 365 和 Azure AD 活动日志中获取用户、管理员、系统和策略的操作和事件相关信息。 

可使用 Office 365 和 Microsoft Azure Active Directory 审核和活动日志中的操作和事件，创建可提供监视、分析和数据可视化的解决方案。 借助这些解决方案，组织可以深入了解对自己内容执行的操作。 Office 365 活动报告中也包含这些操作和事件。 有关详细信息，请参阅[在 Office 365 安全与合规中心内搜索审核日志](https://support.office.com/article/Search-the-audit-log-in-the-Office-365-Security-Compliance-Center-0d4d0f35-390b-4518-800e-0c7ec95e946c)。

借助 Office 365 服务活动 API 这项 REST Web 服务，可开发使用任何语言和宿主环境（支持 HTTPS 和 X.509 证书）的解决方案。 此 API 依赖 Azure AD 和 OAuth2 协议进行身份验证和授权。 若要在应用程序中访问此 API，必须先在 Azure AD 中注册应用程序，并为它配置适当的权限。 这样，应用程序便能请求获取调用此 API 所需的 OAuth2 访问令牌。 有关详细信息，请参阅 [Office 365 管理 API 入门](get-started-with-office-365-management-apis.md)。

有关 Office 365 管理活动 API 返回的数据的信息，请参阅 [Office 365 管理活动 API 架构](office-365-management-activity-api-schema.md)。

> [!IMPORTANT]
> 必须为你的 Office 365 组织启用统一审核日志记录，然后才能通过 Office 365 管理活动 API 访问数据。 可通过启用 Office 365 审核日志来实现此操作。 有关说明，请参阅 [ 启用或禁用 Office 365 审核日志搜索 ](https://docs.microsoft.com/office365/securitycompliance/turn-audit-log-search-on-or-off)。


## <a name="working-with-the-office-365-management-activity-api"></a>使用 Office 365 管理活动 API

Office 365 管理活动 API 将操作和事件聚合到特定于租户的内容 Blob 中，这些内容 Blob 按它们包含的内容的类型和源进行分类。目前，支持以下内容类型：

- Audit.AzureActiveDirectory
    
- Audit.Exchange
    
- Audit.SharePoint
    
- Audit.General（包括前面内容类型不包含的其他所有工作负载）

- DLP.All（仅包括 DLP 事件的所有工作负载）
    
若要详细了解与这些内容类型关联的事件和属性，请参阅 [Office 365 管理活动 API 架构](office-365-management-activity-api-schema.md)。

若要开始检索某租户的内容 blob，请先创建对相应内容类型的订阅。 若要检索多个租户的内容 blob，请创建对每个相应内容类型的多个订阅（每个租户一个订阅）。

创建订阅后，可以通过定期轮询来发现可供下载的新内容 blob，也可以向订阅注册 Webhook 终结点，这样我们便会在有新内容 blob 时向此终结点发送通知。


> [!NOTE] 
> 创建订阅后，第一批内容 blob 最多可能需要 12 个小时才可用于订阅。 内容 blob 的创建方式为，跨多个服务器和数据中心收集并聚合操作和事件。 鉴于此分布式流程，内容 blob 中的操作和事件不一定按发生时间顺序显示。 可能会出现下面这种情况：一个内容 blob 中的操作和事件早于前面内容 blob 中的操作和事件发生。 我们正在努力缩小操作和事件的发生时间与其在内容 blob 中的可用时间之间的延迟，但无法确保它们按时间顺序依序显示。


> [!NOTE] 
> 只有已拥有“读取 DLP 敏感数据”权限的用户，才能通过活动源 API 访问 DLP 敏感数据。 若要详细了解数据丢失防护 (DLP)，请参阅[数据丢失防护策略概述](https://support.office.com/article/Overview-of-data-loss-prevention-policies-1966b2a7-d1e2-4d92-ab61-42efbb137f5e)。

## <a name="activity-api-operations"></a>活动 API 操作

所有 API 操作的范围都限定为一个租户，此 API 的根 URL 包含指定租户上下文的租户 ID。 租户 ID 为 GUID。 若要详细了解如何获取 GUID，请参阅 [Office 365 管理 API 入门](get-started-with-office-365-management-apis.md)。 

由于我们发送到 Webhook 的通知包含租户 ID，因此你可使用相同的 Webhook 来接收所有租户的通知。

你所使用的 API 终结点的 URL 基于贵公司 Microsoft 365 或 Office 365 订阅计划的类型。

**企业版计划**

```http
https://manage.office.com/api/v1.0/{tenant_id}/activity/feed/{operation}
```

**GCC 政府版计划**

```http
https://manage-gcc.office.com/api/v1.0/{tenant_id}/activity/feed/{operation}
```

**GCC （政府）高级版计划**

```http
https://manage.office365.us/api/v1.0/{tenant_id}/activity/feed/{operation}
```

**DoD 政府计划**

```http
https://manage.protection.apps.mil/api/v1.0/{tenant_id}/activity/feed/{operation}
```

所有 API 操作都需要一个授权 HTTP 标头，其中包含从Azure AD获取的访问令牌。访问令牌中的租户 ID 必须与 API 根 URL 中的租户 ID 匹配，访问令牌必须包含 ActivityFeed.Read 声明（这与在 Azure AD 中为应用程序配置的权限 [读取组织的活动数据] 相对应）。

```json
Authorization: Bearer eyJ0e...Qa6wg
```

活动 API 支持以下操作：

- **启动订阅**：以便开始接收通知，并检索租户的活动数据。
    
- **停止订阅**：以便停止检索租户数据。
    
- **列出当前订阅**
    
- **列出可用内容** 及相应的内容 URL。
    
- **接收通知**：通知是在有新内容时由 Webhook 发送。
    
- **检索内容**：使用内容 URL 进行检索。
    
- **列出通知**：通知是由 Webhook 发送。

- **检索资源易记名称**：数据源中由 GUID 标识的对象的名称。
    

## <a name="start-a-subscription"></a>启动订阅

此操作会启动对指定内容类型的订阅。 如果已有对指定内容类型的订阅，此操作用于：

- 更新有效 Webhook 的属性。
    
- 启用因通知发送失败次数过多而被禁用的 Webhook。
    
- 通过指定更晚或为空的到期日期，重新启用已到期的 Webhook。
    
- 删除 Webhook。
    
||订阅|说明|
|:-----|:-----|:-----|
|**路径**| `/subscriptions/start?contentType={ContentType}`||
|**参数**|contentType|必须为有效内容类型。|
||PublisherIdentifier|对 API 编码的供应商的租户 GUID。 这 **不是** 应用程序 GUID 或使用应用程序的客户的 GUID，而是编写代码的公司的 GUID。 此参数用于限制请求速率。 请务必在发出的所有请求中指定此参数，以获取专用配额。 收到的所有不包含此参数的请求都会共用同一配额。|
|**正文**|webhook|可选 JSON 对象，包含以下三个属性：<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><b>address</b>：可接收通知的必需 HTTPS 终结点。  在订阅创建前，Webhook 会收到用于验证自身的测试消息。</p></li><li><p><b>authId</b>：可选字符串，作为 WebHook-AuthID 头包含在向 Webhook 发送的通知中，用于向 Webhook 标识和授权请求来源。</p></li><li><p><b>expiration</b>：可选日期/时间，指明在此日期/时间后便不得再向 Webhook 发送通知。</p></li></ul>|
|**响应**|contentType|在调用中指定的内容类型。|
||status|订阅的状态。如果禁用订阅，将无法列出或检索内容。|
||webhook|在调用中指定的 Webhook 属性，以及 Webhook 的状态。 如果 Webhook 已遭禁用，便无法收到通知，但仍可以列出和检索内容，只要订阅处于启用状态即可。|

#### <a name="sample-request"></a>示例请求

```json
POST {root}/subscriptions/start?contentType=Audit.SharePoint&PublisherIdentifier=46b472a7-c68e-4adf-8ade-3db49497518e
Content-Type: application/json; utf-8
Authorization: Bearer eyJ0e...Qa6wg

{
    "webhook" : {
        "address": "https://webhook.myapp.com/o365/",
        "authId": "o365activityapinotification",
        "expiration": ""
    }
}

```


#### <a name="sample-response"></a>示例响应

```json
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
    "contentType": "Audit.SharePoint",
    "status": "enabled",
    "webhook": {
        "status": "enabled",
        "address":  "https://webhook.myapp.com/o365/",
        "authId": "o365activityapinotification",
        "expiration": null
    }
}

```


## <a name="webhook-validation"></a>Webhook 验证

在 /start 操作已调用且 Webhook 已指定后，我们会向指定 Webhook 地址发送验证通知，以验证有效侦听器能否接受并处理通知。 如果我们没有收到“HTTP 200 正常”响应，便不会创建订阅。 或者，如果 /start 正获调用以向现有订阅添加 Webhook，但我们没有收到“HTTP 200 正常”响应，便不会添加此 Webhook，但订阅仍保持不变。

#### <a name="sample-request"></a>示例请求

```json
POST {webhook address}
Content-Type: application/json; charset=utf-8
Webhook-AuthID: (webhook authId, if provided)
Webhook-ValidationCode: (random opaque string)

{
    "validationCode": (random opaque string, same as header)
}

```


#### <a name="sample-response"></a>示例响应

```json

HTTP/1.1 200 OK

```


## <a name="stop-a-subscription"></a>停止订阅

此操作会停止对指定内容类型的订阅。 

在订阅停止后，便不再会收到通知，也无法再检索可用内容。 如果今后重启订阅，将有权访问重启时间点后的新内容， 但无法检索从订阅停止时间到重启时间之间的可用内容。


||订阅|说明|
|:-----|:-----|:-----|
|**路径**| `/subscriptions/stop?contentType={ContentType}`||
|**参数**|contentType|必须为有效内容类型。|
||PublisherIdentifier|对 API 编码的供应商的租户 GUID。 这 **不是** 应用程序 GUID 或使用应用程序的客户的 GUID，而是编写代码的公司的 GUID。 此参数用于限制请求速率。 请务必在发出的所有请求中指定此参数，以获取专用配额。 收到的所有不包含此参数的请求都会共用同一配额。|
|**正文**|（空）||
|**响应**|（空）|||

#### <a name="sample-request"></a>示例请求

```json
POST {root}/subscriptions/stop?contentType=Audit.SharePoint&PublisherIdentifier=46b472a7-c68e-4adf-8ade-3db49497518e
Authorization: Bearer eyJ0e...Qa6wg

```

#### <a name="sample-response"></a>示例响应

```json
HTTP/1.1 200 OK
```


## <a name="list-current-subscriptions"></a>列出当前订阅

此操作会返回包含当前订阅及相关 Webhook 的集合。

||订阅|说明|
|:-----|:-----|:-----|
|**路径**| `/subscriptions/list`||
|**参数**|PublisherIdentifier|对 API 编码的供应商的租户 GUID。 这 **不是** 应用程序 GUID 或使用应用程序的客户的 GUID，而是编写代码的公司的 GUID。 此参数用于限制请求速率。 请务必在发出的所有请求中指定此参数，以获取专用配额。 收到的所有不包含此参数的请求都会共用同一配额。|
|**正文**|（空）||
|**响应**|JSON 数组|每个订阅都由包含以下三个属性的 JSON 对象表示：<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><b>contentType</b>：指明内容类型。</p></li><li><p><b>status</b>：指明订阅的状态。</p></li><li><p><b>webhook</b>：指明已配置的 Webhook 及其状态（已启用、已禁用或已到期）。  如果订阅没有 Webhook，虽然 Webhook 属性会显示，但值为 null。</p></li></ul>|


#### <a name="sample-request"></a>示例请求

```json
GET {root}/subscriptions/list?PublisherIdentifier=46b472a7-c68e-4adf-8ade-3db49497518e
Authorization: Bearer eyJ0e...Qa6wg

```

#### <a name="sample-response"></a>示例响应

```json
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

[
    {
        "contentType" : "Audit.SharePoint",
        "status": "enabled",
        "webhook": {
            "status": "enabled",
            "address": "https://webhook.myapp.com/o365/",
            "authId": "o365activityapinotification",
            "expiration": null
        }
    },

    ...

    {
        "contentType": "Audit.Exchange",
        "webhook": null
    }
]

```


## <a name="list-available-content"></a>列出可用内容

此操作会列出当前可供检索的指定内容类型内容。 内容中聚合了从跨多个数据中心的多个服务器中捕获的操作和事件。 内容按聚合可用时间顺序列出，但无法保证聚合中的事件和操作按时间顺序依序列出。 如果订阅处于已禁用状态，返回的便是错误。


||订阅|说明|
|:-----|:-----|:-----|
|**路径**| `/subscriptions/content?contentType={ContentType}&amp;startTime={0}&amp;endTime={1}`||
|**参数**|contentType|必须为有效内容类型。|
||PublisherIdentifier|对 API 编码的供应商的租户 GUID。 这 **不是** 应用程序 GUID 或使用应用程序的客户的 GUID，而是编写代码的公司的 GUID。 此参数用于限制请求速率。 请务必在发出的所有请求中指定此参数，以获取专用配额。 收到的所有不包含此参数的请求都会共用同一配额。|
||startTime endTime|依据为内容可用时间的可选日期/时间 (UTC)，指明要返回的内容的时间范围。 此时间范围包含 startTime (startTime <= contentCreated)，但不包含 endTime (contentCreated < endTime)，这样便能使用不重叠的增量时间间隔翻阅可用内容。<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>YYYY-MM-DD</p></li><li><p>YYYY-MM-DDTHH:MM</p></li><li><p>YYYY-MM-DDTHH:MM:SS</p></li></ul>必须同时指定（或同时省略）开始时间和结束时间，两者间隔不得超过 24 小时，且开始时间与当前时间的间隔不得超过 7 天。 默认情况下，如果 startTime 和 endTime 遭省略，返回的便是过去 24 小时内的可用内容。<p>**注意**：即使可以指定间隔超过 24 小时的 startTime 和 endTime，也不建议这样做。 此外，如果确实返回了开始时间和结束时间间隔超过 24 小时的请求的任何响应结果，它们可能只是部分结果，不得纳入考虑范围。 应发出 startTime 和 endTime 间隔不超过 24 小时的请求。</p>|
|**响应**|JSON 数组|可用内容由包含以下属性的 JSON 对象表示：<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><b>contentType</b>：指明内容类型。</p></li><li><p><b>contentId</b>：用于唯一标识内容的不透明字符串。</p></li><li><p><b>contentUri</b>：供检索内容时使用的 URL。</p></li><li><p><b>contentCreated</b>：内容的可用日期/时间。</p></li><li><p><b>contentExpiration</b>：内容不再可供检索的日期/时间。</p></li></ul>|


#### <a name="sample-request"></a>示例请求

```json
GET {root}/subscriptions/content?contentType=Audit.SharePoint&PublisherIdentifier=46b472a7-c68e-4adf-8ade-3db49497518e
Authorization: Bearer eyJ0e...Qa6wg

```

#### <a name="sample-response"></a>示例响应

```json
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

[
    {
        "contentType": "Audit.SharePoint",
        "contentId": "492638008028$492638008028$f28ab78ad40140608012736e373933ebspo2015043022$4a81a7c326fc4aed89c62e6039ab833b$04",
        "contentUri": "https://manage.office.com/api/v1.0/f28ab78a-d401-4060-8012-736e373933eb/activity/feed/audit/492638008028$492638008028$f28ab78ad40140608012736e373933ebspo2015043022$4a81a7c326fc4aed89c62e6039ab833b$04",
        "contentCreated": "2015-05-23T17:35:00.000Z",
        "contentExpiration": "2015-05-30T17:35:00.000Z"
    },
    ...
]

```


### <a name="pagination"></a>分页

列出某个时间范围的可用内容时，返回的结果数会受到限制，以防止响应超时。 如果指定时间范围的结果数超出一个响应中可返回的结果数，结果便会被截断，并向响应添加头，以指明用于检索下页结果的 URL。 此 URL 包含在初始请求中指定的相同 _startTime_ 和 _endTime_ 参数，以及用于指明下页的内部 ID 的参数。 如果初始请求中未指定 _startTime_ 和 _endTime_，便会设置为反映初始请求前 24 小时间隔内的结果。


```json
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
NextPageUri: https://manage.office.com/api/v1/{tenant_id}/activity/feed/subscriptions/content?contentType=Audit.SharePoint&amp;startTime=2015-10-01&amp;endTime=2015-10-02&amp;nextPage=2015101900R022885001761

```

若要列出指定时间范围的所有可用内容，可能需要检索多页，直到收到的响应不含 **NextPageUri** 头为止。


## <a name="receiving-notifications"></a>接收通知

当有新内容可用时，我们便会向订阅的已配置 Webhook 发送通知。 由于通知包含租户标识符，因此可使用同一个 Webhook 接收已订阅的所有租户的通知。

通知是以 HTTP POST 形式通过 TLS（TLS 1.0 及更高版本）发送到指定 Webhook 地址。 如果 Webhook 配置包括验证 ID，我们便会以 HTTP 头 (Webhook-AuthID) 的形式发送它。 除“HTTP 200 正常”之外的其他任何响都被视为失败，我们会重试发送通知。 还可以将 Webhook 配置为要求基于客户端证书的身份验证，我们将使用 manage.office.com 证书进行身份验证。

请求正文包含一个数组，其中有一个或多个表示可用内容 blob 的 JSON 对象。 为了保持通知相对较小，各个通知中的内容 blob 数受到限制。 由于此限制可能会更改，因此实现应查询数组长度，而不要以为它是固定大小。 每个对象包含 /content 操作返回的相同属性，以及数据所属租户的 GUID 和创建订阅的应用程序的 GUID。 这样，Webhook 便可以在用于多个租户和应用程序时创建上下文。

- **tenantId**：内容所属租户的 GUID。
    
- **clientId**：创建订阅的应用程序的 GUID。
    
- **contentType**：指明内容类型。
    
- **contentId**：用于唯一标识内容的不透明字符串。
    
- **contentUri**：供检索内容时使用的 URL。
    
- **contentCreated**：内容的可用日期/时间。
    
- **contentExpiration**：内容不再可供检索的日期/时间。
    
下面是一个通知示例。

```json
POST https://webhook.myapp.com/o365/ 
Content-Type: application/json; utf-8
Webhook-AuthID: o365activityapinotification

[
    {
        "tenantId": "{GUID}",
        "clientId": "{GUID}",
        "contentType": "Audit.SharePoint",
        "contentId": "492638008028$492638008028$f28ab78ad40140608012736e373933ebspo2015043022$4a81a7c326fc4aed89c62e6039ab833b$04",
        "contentUri": "https://manage.office.com/api/v1.0/f28ab78a-d401-4060-8012-736e373933eb/activity/feed/audit/492638008028$492638008028$f28ab78ad40140608012736e373933ebspo2015043022$4a81a7c326fc4aed89c62e6039ab833b$04",
        "contentCreated": "2015-05-23T17:35:00.000Z",
        "contentExpiration": "2015-05-30T17:35:00.000Z"
    },
    ...
]

```


## <a name="notification-failure-and-retry"></a>通知失败和重试

通知系统在有新内容可用时发送通知。 如果通知发送失败次数过多，重试机制便会以指数方式增加两次重试间隔。 如果一再失败，我们有权禁用 Webhook 并彻底停止向它发送通知。 可使用 /start 操作重新启用已禁用的 Webhook。


## <a name="retrieving-content"></a>检索内容

若要检索内容 blob，请对可用内容列表和已发送到 Webhook 的通知中包含的相应内容 URI 发出 GET 请求。 返回的内容为一个集合，其中包含一个或多个 JSON 格式的操作和事件。

#### <a name="sample-request"></a>示例请求

```json
GET https://manage.office.com/api/v1.0/41463f53-8812-40f4-890f-865bf6e35190/activity/feed/audit/301299007231$301299007231$41463f53881240f4890f865bf6e35190aad2015062920$e1c2ab19858a469fb1f1fd097effffc9$04 HTTP/1.1
Authorization: Bearer eyJ0e...Qa6wg
```

#### <a name="sample-response"></a>示例响应

```json
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

[
    {
        "CreationTime": "2015-06-29T20:03:19",
        "Id": "80c76bd2-9d81-4c57-a97a-accfc3443dca",
        "Operation": "PasswordLogonInitialAuthUsingPassword",
        "OrganizationId": "41463f53-8812-40f4-890f-865bf6e35190",
        "RecordType": 9,
        "ResultStatus": "failed",
        "UserKey": "1153977025279851686@contoso.onmicrosoft.com",
        "UserType": 0,
        "Workload": "AzureActiveDirectory",
        "ClientIP": "134.170.188.221",
        "ObjectId": "admin@contoso.onmicrosoft.com",
        "UserId": "admin@contoso.onmicrosoft.com",
        "AzureActiveDirectoryEventType": 0,
        "ExtendedProperties": [
            {
                "Name": "LoginError",
                "Value": "-2147217390;PP_E_BAD_PASSWORD;The entered and stored passwords do not match."
            }
        ],
        "Client": "Exchange",
        "LoginStatus": -2147217390,
        "UserDomain": "contoso.onmicrosoft.com"
    },
    {
        "CreationTime": "2015-06-29T20:03:34",
        "Id": "4e655d3f-35fa-42e0-b050-264b2d255c7a",
        "Operation": "PasswordLogonInitialAuthUsingPassword",
        "OrganizationId": "41463f53-8812-40f4-890f-865bf6e35190",
        "RecordType": 9,
        "ResultStatus": "success",
        "UserKey": "1153977025279851686@contoso.onmicrosoft.com",
        "UserType": 0,
        "Workload": "AzureActiveDirectory",
        "ClientIP": "134.170.188.221",
        "ObjectId": "admin@contoso.onmicrosoft.com",
        "UserId": "admin@contoso.onmicrosoft.com",
        "AzureActiveDirectoryEventType": 0,
        "Client": "Exchange",
        "LoginStatus": 0,
        "UserDomain": "contoso.onmicrosoft.com"
    },
    {
        "CreationTime": "2015-06-29T20:04:55",
        "Id": "b567caf0-088e-4c1c-a4ea-633a1e3d66c8",
        "Operation": "Add User.",
        "OrganizationId": "41463f53-8812-40f4-890f-865bf6e35190",
        "RecordType": 8,
        "ResultStatus": "success",
        "UserKey": "1003BFFD8EC47CA6@contoso.onmicrosoft.com",
        "UserType": 0,
        "Workload": "AzureActiveDirectory",
        "ObjectId": "user001@contoso.onmicrosoft.com",
        "UserId": "admin@contoso.onmicrosoft.com",
        "AzureActiveDirectoryEventType": 0,
        "Actor": [
            {
                "ID": "1cef1fdb-ff52-48c4-8e4e-dfb5ea83d357",
                "Type": 2
            },
            {
                "ID": "admin@contoso.onmicrosoft.com",
                "Type": 5
            },
            {
                "ID": "1003BFFD8EC47CA6",
                "Type": 3
            }
        ],
        "ActorContextId": "41463f53-8812-40f4-890f-865bf6e35190",
        "InterSystemsId": "c2ced078-ad57-4079-a743-5c37f5284790",
        "IntraSystemId": "d1497f7e-15b4-49aa-83ad-11a17ca4a2f4",
        "Target": [
            {
                "ID": "user001@contoso.onmicrosoft.com",
                "Type": 5
            },
            {
                "ID": "10037FFE91510806",
                "Type": 3
            }
        ],
        "TargetContextId": "41463f53-8812-40f4-890f-865bf6e35190"
    }
]

```


## <a name="list-notifications"></a>列出通知

此操作会列出尝试对指定内容类型发送的所有通知。 如果在启动对内容类型的订阅时未添加 Webhook，便无通知可供检索。 因为我们会在失败时重试发送通知，所以此操作可能会返回有关同一内容的多个通知，并且通知发送顺序不一定与内容可用时间顺序一致（尤其是在失败后重试发送通知时）。 

此操作可有助于调查与 Webhook 和通知相关的问题，但不得用它来确定当前什么内容可供检索。 若要确定，请改用 /content 操作。 如果订阅处于已禁用状态，返回的便是错误。


||订阅|说明|
|:-----|:-----|:-----|
|**路径**| `/subscriptions/notifications?contentType={ContentType}&amp;startTime={0}&amp;endTime={1}`||
|**参数**|contentType|必须为有效内容类型。|
||PublisherIdentifier|对 API 编码的供应商的租户 GUID。 这 **不是** 应用程序 GUID 或使用应用程序的客户的 GUID，而是编写代码的公司的 GUID。 此参数用于限制请求速率。 请务必在发出的所有请求中指定此参数，以获取专用配额。 收到的所有不包含此参数的请求都会共用同一配额。|
||startTime endTime|依据为内容可用时间的可选日期/时间 (UTC)，指明要返回的内容的时间范围。 此时间范围包含 _startTime_ ( _startTime_ <= contentCreated)，但不包含 _endTime_ (_contentCreated_ < endTime)，这样便能使用不重叠的增量时间间隔翻阅可用内容。<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>YYYY-MM-DD</p></li><li><p>YYYY-MM-DDTHH:MM</p></li><li><p>YYYY-MM-DDTHH:MM:SS</p></li></ul>必须同时指定（或同时省略）开始时间和结束时间，两者间隔不得超过 24 小时，且开始时间与当前时间的间隔不得超过 7 天。 默认情况下，如果 _startTime_ 和 _endTime_ 遭省略，返回的便是过去 24 小时内的可用内容。|
|**响应**|JSON 数组|通知由包含以下属性的 JSON 对象表示： <ul><li>**contentType**：指明内容类型。</li><li>**contentId**：用于唯一标识内容的不透明字符串。</li><li>**contentUri**：供检索内容时使用的 URL。 </li><li>**contentCreated**：内容的可用日期/时间。</li><li>**contentExpiration**：内容不再可供检索的日期/时间。</li><li>**notificationSent**：通知发送日期/时间。</li><li>**notificationStatus**：指明通知尝试发送是成功还是失败。</li></ul>|

#### <a name="sample-request"></a>示例请求

```json
GET {root}/subscriptions/notifications?contentType=Audit.SharePoint&PublisherIdentifier=46b472a7-c68e-4adf-8ade-3db49497518e
Authorization: Bearer eyJ0e...Qa6wg

```

#### <a name="sample-response"></a>示例响应

```json
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

[
    {
        "contentType": "Audit.SharePoint",
        "contentId": "492638008028$492638008028$f28ab78ad40140608012736e373933ebspo2015043022$4a81a7c326fc4aed89c62e6039ab833b$04",
        "contentUri": "https://manage.office.com/api/v1.0/f28ab78a-d401-4060-8012-736e373933eb/activity/feed/audit/492638008028$492638008028$f28ab78ad40140608012736e373933ebspo2015043022$4a81a7c326fc4aed89c62e6039ab833b$04",
        "contentCreated": "2015-05-23T17:35:00.000Z",
        "contentExpiration": "2015-05-30T17:35:00.000Z",
        "notificationSent": "2015-05-23T17:36:00.000Z",
        "notificationStatus": "success"

    },
    ...
]

```


### <a name="pagination"></a>分页

列出某个时间范围的通知历史记录时，返回的结果数会受到限制，以防止响应超时。 如果指定时间范围的结果数超出一个响应中可返回的结果数，结果便会被截断，并向响应添加头，以指明用于检索下页结果的 URL。 此 URL 包含在初始请求中指定的相同 _startTime_ 和 _endTime_ 参数，以及用于指明下页的内部 ID 的参数。 如果初始请求中未指定 _startTime_ 和 _endTime_，便会设置为反映初始请求前 24 小时间隔内的结果。

```json
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
NextPageUri: https://manage.office.com/api/v1/{tenant_id}/activity/feed/subscriptions/content?contentType=Audit.SharePoint&amp;startTime=2015-10-01&amp;endTime=2015-10-02&amp;nextPage=2015101900R022885001761

```

若要列出指定时间范围的所有可用内容，可能需要检索多页，直到收到的响应不含 **NextPageUri** 头为止。

## <a name="retrieve-resource-friendly-names"></a>检索资源易记名称

此操作会检索数据源中由 GUID 标识的对象的易记名称。 目前，仅支持“DlpSensitiveType”对象。 


||订阅|说明|
|:-----|:-----|:-----|
|**路径**| `/resources/dlpSensitiveTypes`||
|**参数**|PublisherIdentifier|对 API 编码的供应商的租户 GUID。 这 **不是** 应用程序 GUID 或使用应用程序的客户的 GUID，而是编写代码的公司的 GUID。 此参数用于限制请求速率。 请务必在发出的所有请求中指定此参数，以获取专用配额。 收到的所有不包含此参数的请求都会共用同一配额。|
|**头**|Accept-Language|用于指定本地化名称的目标语言的头。 例如，使用“en-US”表示英语，使用“es”表示西班牙语。 如果没有此头，返回的便是默认语言 (en-US)。|
|**正文**|（空）||
|**响应**|JSON 数组|可用内容由包含以下属性的 JSON 对象表示：<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><b>id</b>：指明敏感信息类型的 GUID。</p></li><li><p><b>name</b>：指明敏感信息类型的易记名称。</p></li></ul>|

#### <a name="sample-request"></a>示例请求

```json
GET {root}/resources/dlpSensitiveTypes?PublisherIdentifier=46b472a7-c68e-4adf-8ade-3db49497518e
Authorization: Bearer eyJ0e...Qa6wg
Accept-Language: {language code} 

```

#### <a name="sample-response"></a>示例响应

```json
HTTP/1.1 200 OK

[
    {
        "id": "50842eb7-edc8-4019-85dd-5a5c1f2bb085",
        "name": "CreditCardNumber"
    }, 
    {
        "id": "0e9b3178-9678-47dd-a509-37222ca96b42",
        "name": "EUDebitCardNumber"
    }, 
    ...
    {
    }
]

```

## <a name="api-throttling"></a>API 限制

通过 Office 365 管理活动 API 访问审核日志的组织，会因发布者级别限制而受限。 这意味着，对于代表多个客户请求数据的发布者而言，限制由所有这些客户共享。

我们正在从发布者级别限制迁移至租户级别限制。 结果是每个组织都会获得自己完全分配的带宽配额，以访问其审核数据。 所有组织最初每分钟分配 2000 个请求基线。 这不是一个静态的预定义的限制，但根据组合因素进行建模，包括组织中的席位数， Office 365 和 Microsoft 365 E5 组织的带宽大约为非 E5 组织的两倍。 这也是最大宽带的上限，以保护服务的健康。

有关详细信息，参见 “[Microsoft 365 高级审核](https://docs.microsoft.com/microsoft-365/compliance/advanced-audit#high-bandwidth-access-to-the-office-365-management-activity-api)”中的“Office 365 管理活动 API”。

> [!NOTE] 
> 尽管每个租户最初每分钟最多可提交 2000 个请求，Microsoft 也无法保证响应速率。 响应速率取决于各种因素，如客户端系统性能、网络容量和网络速度。 

## <a name="errors"></a>错误

如果遇到错误，服务会使用标准 HTTP 错误代码语法，向调用方报告错误响应代码。 . 失败调用的正文中包含其他信息（作为一个 JSON 对象）。 下面的示例展示了完整的 JSON 错误正文： 

```json

{ 
    "error":{ 
        "code":"AF50000",
        "message": "An internal server error occurred. Retry the request."
    } 
}

```


|||
|:-----|:-----|
|代码|消息|
|AF10001|在请求中发送的权限集 ({0}) 不含所需的权限 **ActivityFeed.Read**。<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>{0} = 访问令牌中的权限集。</p></li></ul>|
|AF20001|缺少参数 {0}。 <ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>{0} = 缺少的参数的名称。</p></li></ul>|
|AF20002|参数类型 {0} 无效。 类型应为 {1}<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>{0} = 无效参数的名称。</p></li><li><p>{1} = 应采用的类型（int、datetime、guid）</p></li></ul>|
|AF20003|提供的到期时间 {0} 设置为过去的日期和时间。<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>{0} = 在 API 调用中传递的到期时间。</p></li></ul>|
|AF20010|在 URL 中传递的租户 ID ({0}) 与访问令牌中传递的租户 ID ({1}) 不一致。<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>{0} = 在 URL 中传递的租户 ID</p></li><li><p>{1} = 在访问令牌中传递的租户 ID</p></li></ul>|
|AF20011|指定的租户 ID ({0}) 在系统中不存在或已遭删除。 <ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>   {0} = 在 URL 中传递的租户 ID</p></li></ul>|
|AF20012|指定的租户 ID ({0}) 未在系统中正确配置。 <ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>    {0} = 在 URL 中传递的租户 ID</p></li></ul>|
|AF20013|在 URL 中传递的租户 ID ({0}) 不是有效的 GUID。<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p> {0} = 在 URL 中传递的租户 ID</p></li></ul>|
|AF20020|指定的内容类型无效。|
|AF20021|无法验证 webhook 终结点 {{0}）。 {1}<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>{0} = Webhook 地址。</p></li><li><p>{1} =“终结点未返回 HTTP 200” 或“地址必须以 HTTPS 开头”。</p></li></ul>|
|AF20022|找不到对指定内容类型的订阅。|
|AF20023|{0} 已禁用订阅。<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>{0} =“租户管理员”或“服务管理员”</p></li></ul>|
|AF20030|必须同时指定（或同时省略）开始时间和结束时间，两者间隔不得超过 24 小时，且开始时间与当前时间的间隔不得超过 7 天。|
|AF20031|nextPage 输入 {0} 无效<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>{0} = 在 URL 中传递的下页指示符</p></li></ul>|
|AF20050|指定的内容 ({0}) 不存在。<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>{0} = 资源 ID 或资源 URL</p></li></ul>|
|AF20051|使用密钥 {0} 请求获取的内容已到期。 无法检索 7 天前的内容。<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>{0} = 资源 ID 或资源 URL</p></li></ul>|
|AF20052|URL 中的内容 ID {0} 无效。<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>{0} = 资源 ID 或资源 URL</p></li></ul>|
|AF20053|Accept-Language 头中只能显示一种语言。|
|AF20054|Accept-Language 头中的语法无效。|
|AF429|请求次数过多。 方法 = {0}，PublisherId = {1}<ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>{0} = HTTP 方法</p></li><li><p>{1} = 用作 PublisherIdentifier 的租户 GUID</p></li></ul>|
|AF50000|发生了内部错误。 请重试请求。|
