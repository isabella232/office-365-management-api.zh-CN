---
ms.TocTitle: Office 365 Service Communications API reference
title: Office 365 服务通信 API 参考
description: 此 API 可用于访问以下数据：“获取服务”、“获取当前状态”、“获取历史状态”和“获取消息”。
ms.ContentId: d0b9341a-b205-5442-1c20-8fb56407351d
ms.topic: reference (API)
ms.date: ''
localization_priority: Priority
ms.openlocfilehash: e6024c19457796fb6f3fb94a62a013cc86a95072
ms.sourcegitcommit: a4ba198b7417e49880905e49a38d0bd1f4ad8802
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/02/2020
ms.locfileid: "47334886"
---
# <a name="office-365-service-communications-api-reference"></a>Office 365 服务通信 API 参考

Office 365 服务通信 API V2 可用于访问以下数据：

- **获取服务**：获取已订阅服务的列表。

- **获取当前状态**：获取当前正在进行的服务事件的实时视图。

- **获取历史记录状态**：获取服务事件的历史视图。

- **获取消息**：查找事件和消息中心通信。

目前，Office 365 服务通信 API 包含 Office 365、Yammer、Dynamics CRM 和 Microsoft Intune 云服务的数据。

## <a name="the-fundamentals"></a>基础知识

此 API 的根 URL 包含将操作范围限定为一个租户的租户标识符：

```http
https://manage.office.com/api/v1.0/{tenant_identifier}/ServiceComms/{operation}
```

借助 **Office 365 服务通信 API** 这项 REST 服务，可开发使用任何 Web 语言和宿主环境（支持 HTTPS 和 X.509 证书）的解决方案。 此 API 依赖 **Microsoft Azure Active Directory** 和 **OAuth2** 协议进行身份验证和授权。 若要在应用程序中访问此 API，必须先在 Azure AD 中注册应用程序，并为它配置适当范围的权限。 这样，应用程序便能请求获取调用此 API 所需的 OAuth2 访问令牌。 若要详细了解如何在 Azure AD 中注册和配置应用程序，请参阅 [Office 365 管理 API 入门](get-started-with-office-365-management-apis.md)。

所有 API 请求都要求，授权 HTTP 头中必须有从 Azure AD 中获取的包含 **ServiceHealth.Read** 声明的有效 OAuth2 JWT 访问令牌；且租户标识符必须与根 URL 中的租户标识符一致。

```json
Authorization: Bearer {OAuth2 token}
```

### <a name="request-headers"></a>请求标头

以下是所有 Office 365 服务通信 API 操作支持的请求头。

|头|说明|
|:-----|:-----|
|**Accept（可选）**|以下是可接受的响应表示形式：<br/>**application/json;odata.metadata=full**<br/>**application/json;odata.metadata=minimal**<br/>[The default if header not specified] **application/json;odata.metadata=none**|
|**Authorization（必需）**|请求的授权令牌（持有者 JWT Azure AD 令牌）。|

<br/>

### <a name="response-headers"></a>响应标头

以下是所有 Office 365 服务通信 API 操作返回的响应头：

|头|说明|
|:-----|:-----|
|**Content-Length**|响应正文长度。|
|**Content-Type**|响应表示形式：<br/>**application/json**<br/>**application/json;odata.metadata=full**<br/>**application/json;odata.metadata=minimal**<br/>**application/json;odata.metadata=none**<br/>**odata.streaming=true**|
|**Cache-Control**|用于指定请求/响应链上的所有缓存机制都必须遵守的指令。|
|**Pragma**|实现专属行为。|
|**Expires**|客户端何时应让资源到期。|
|**X-Activity-Id**|服务器生成的活动 ID。|
|**OData-Version**|受支持的 OData 版本 (4.0)。|
|**Date**|服务器发送响应的日期 (UTC)。|
|**X-Time-Taken**|响应生成耗时（以毫秒为单位）。|
|**X-Instance-Name**|用于生成响应的 Azure 实例的标识符（用于调试目的）。|
|**Server**|用于生成响应的服务器（用于调试目的）。|
|**X-ASPNET-Version**|生成响应的服务器使用的 ASP.Net 版本（用于调试目的）。|
|**X-Powered-By**|生成响应的服务器使用的技术（用于调试目的）。|
|||

<br/>

下文介绍了 Office 365 服务通信 API 操作。

## <a name="get-services"></a>获取服务

返回已订阅服务的列表。

||服务|说明|
|:-----|:-----|:-----|
|**路径**| `/Services`||
|**查询选项**|$select|选择一部分属性。|
|**响应**|“Service”实体列表|“Service”实体包含“Id”(String)、“DisplayName”(String) 和“FeatureNames”（String 列表）。|
||||

### <a name="sample-request"></a>示例请求

```json
GET https://manage.office.com/api/v1.0/contoso.com/ServiceComms/Services 
Authorization: Bearer {AAD_Bearer_JWT_Token}

```

### <a name="sample-response"></a>示例响应

```json
{
    "value": [
        {
            "Id": "Exchange",
            "DisplayName": "Exchange Online",
            "FeatureNames": [
                "Sign-in",
                "E-Mail and calendar access",
                "E-Mail timely delivery",
                "Management and Provisioning",
                "Voice mail"
            ]
        },
        {
            "Id": "Lync",
            "DisplayName": "Lync Online",
            "FeatureNames": [
                "Audio and Video",
                "Federation",
                "Management and Provisioning",
                "Sign-In",
                "All Features",
                "Dial-In Conferencing",
                "Online Meetings",
                "Instant Messaging",
                "Presence",
                "Mobility"
            ]
        }
    ]
}
```

## <a name="get-current-status"></a>获取当前状态

返回过去 24 小时的服务状态。

> [!NOTE] 
> 服务响应将包含过去 24 小时内的状态和所有事件。 返回的 StatusDate 或 StatusTime 值将正好是过去 24 小时。 若要获取特定事件的最近一次更新信息, 请使用 "获取消息" 功能并从与事件 ID 匹配的响应记录中读取 LastUpdatedTime 值。 <br/>

<br/>

||服务|说明|
|:-----|:-----|:-----|
|**路径**| `/CurrentStatus`||
|**筛选器**|Workload|按 Workload 筛选（String，默认值：all）。|
|**查询选项**|$select|选择一部分属性。|
|**响应**|“WorkloadStatus”实体列表。|“WorkloadStatus”实体包含“Id”(String)、“Workload”(String)、“StatusTime”(DateTimeOffset)、“WorkloadDisplayName”(String)、“Status”(String)、“IncidentIds”（String 列表）和“FeatureGroupStatusCollection”（“FeatureStatus”列表）。<br/><br/>“FeatureStatus”实体包含“Feature”(String)、“FeatureGroupDisplayName”(String) 和“FeatureStatus”(String)。|
||||

### <a name="sample-request"></a>示例请求

```json
GET https://manage.office.com/api/v1.0/contoso.com/ServiceComms/CurrentStatus
Authorization: Bearer {AAD_Bearer_JWT_Token}
```

### <a name="sample-response"></a>示例响应

```json
{
    "value": [
        {
            "Id": "Exchange",
            "Workload": "Exchange",
            "StatusDate": "2015-04-11T00:00:00Z",
            "WorkloadDisplayName": "Exchange Online",
            "Status": "ServiceOperational",
            "IncidentIds": [],
            "FeatureGroupStatusCollection": [
                {
                    "Feature": "Signin",
                    "FeatureGroupDisplayName": "Sign-in",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "Access",
                    "FeatureGroupDisplayName": "E-Mail and calendar access",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "Delivery",
                    "FeatureGroupDisplayName": "E-Mail timely delivery",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "Provisioning",
                    "FeatureGroupDisplayName": "Management and Provisioning",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "UnifiedMessaging",
                    "FeatureGroupDisplayName": "Voice mail",
                    "FeatureStatus": "ServiceOperational"
                }
            ]
        },
        {
            "Id": "Lync",
            "Workload": "Lync",
            "StatusDate": "2015-04-11T00:00:00Z",
            "WorkloadDisplayName": "Lync Online",
            "Status": "ServiceOperational",
            "IncidentIds": [],
            "FeatureGroupStatusCollection": [
                {
                    "Feature": "AudioVideo",
                    "FeatureGroupDisplayName": "Audio and Video",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "Federation",
                    "FeatureGroupDisplayName": "Federation",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "ManagementandProvisioning",
                    "FeatureGroupDisplayName": "Management and Provisioning",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "Sign-In",
                    "FeatureGroupDisplayName": "Sign-In",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "All",
                    "FeatureGroupDisplayName": "All Features",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "DialInConferencing",
                    "FeatureGroupDisplayName": "Dial-In Conferencing",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "OnlineMeetings",
                    "FeatureGroupDisplayName": "Online Meetings",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "InstantMessaging",
                    "FeatureGroupDisplayName": "Instant Messaging",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "Presence",
                    "FeatureGroupDisplayName": "Presence",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "Mobility",
                    "FeatureGroupDisplayName": "Mobility",
                    "FeatureStatus": "ServiceOperational"
                }
            ]
        }
    ]
}
```

### <a name="status-definitions"></a>状态定义

状态定义包括以下值:

- 正在调查
- ServiceDegradation
- ServiceInterruption
- RestoringService
- ExtendedRecovery
- InvestigationSuspended
- ServiceRestored
- FalsePositive
- PostIncidentReportPublished
- VerifyingService
- ServiceOperational

有关最新列表及这些状态定义的说明, 请参阅[如何检查 Office 365 服务运行状况](https://docs.microsoft.com/office365/enterprise/view-service-health#status-definitions)。

## <a name="get-historical-status"></a>获取历史状态

按天返回服务在特定时间范围内的历史状态。

||服务|说明|
|:-----|:-----|:-----|
|**路径**| `/HistoricalStatus`||
|**筛选器**|Workload|按 Workload 筛选（String，默认值：all）。|
||StatusTime|按晚于 StatusTime 的天数进行筛选（DateTimeOffset，默认值：ge CurrentTime - 7 days）。|
|**查询选项**|$select|选择一部分属性。|
|**响应**|“WorkloadStatus”实体列表。|“WorkloadStatus”实体包含“Id”(String)、“Workload”(String)、“StatusTime”(DateTimeOffset)、“WorkloadDisplayName”(String)、“Status”(String)、“IncidentIds”（String 列表）和“FeatureGroupStatusCollection”（“FeatureStatus”列表）。<br/><br/>“FeatureStatus”实体包含“Feature”(String)、“FeatureGroupDisplayName”(String) 和“FeatureStatus”(String)。|
||||

### <a name="sample-request"></a>示例请求

```json
GET https://manage.office.com/api/v1.0/contoso.com/ServiceComms/HistoricalStatus
Authorization: Bearer {AAD_Bearer_JWT_Token}
```

### <a name="sample-response"></a>示例响应

```json
{
    "value": [
        {
            "Id": "Exchange",
            "Workload": "Exchange",
            "StatusDate": "2015-04-11T00:00:00Z",
            "WorkloadDisplayName": "Exchange Online",
            "Status": "ServiceOperational",
            "IncidentIds": [],
            "FeatureGroupStatusCollection": [
                {
                    "Feature": "Signin",
                    "FeatureGroupDisplayName": "Sign-in",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "Access",
                    "FeatureGroupDisplayName": "E-Mail and calendar access",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "Delivery",
                    "FeatureGroupDisplayName": "E-Mail timely delivery",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "Provisioning",
                    "FeatureGroupDisplayName": "Management and Provisioning",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "UnifiedMessaging",
                    "FeatureGroupDisplayName": "Voice mail",
                    "FeatureStatus": "ServiceOperational"
                }
            ]
        },
        {
            "Id": "Exchange",
            "Workload": "Exchange",
            "StatusDate": "2015-04-10T00:00:00Z",
            "WorkloadDisplayName": "Exchange Online",
            "Status": "InformationAvailable",
            "IncidentIds": [
                "EX20870"
            ],
            "FeatureGroupStatusCollection": [
                {
                    "Feature": "Signin",
                    "FeatureGroupDisplayName": "Sign-in",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "Access",
                    "FeatureGroupDisplayName": "E-Mail and calendar access",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "Delivery",
                    "FeatureGroupDisplayName": "E-Mail timely delivery",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "Provisioning",
                    "FeatureGroupDisplayName": "Management and Provisioning",
                    "FeatureStatus": "ServiceOperational"
                },
                {
                    "Feature": "UnifiedMessaging",
                    "FeatureGroupDisplayName": "Voice mail",
                    "FeatureStatus": "InformationAvailable"
                }
            ]
        }
    ]
}
```

## <a name="get-messages"></a>获取消息

返回特定时间范围内关于服务的消息。 使用类型筛选器，以筛选出“服务事件”、“计划内维护”或“消息中心”消息。

||服务|说明|
|:-----|:-----|:-----|
|**路径**| `/Messages`||
|**筛选器**|Workload|按 Workload 筛选（String，默认值：all）。|
||StartTime|按 StartTime 筛选（DateTimeOffset，默认值：ge CurrentTime - 7 days）。|
||EndTime|按 EndTime 筛选（DateTimeOffset，默认值：le CurrentTime）。|
||MessageType|按 MessageType 筛选（String，默认值：all）。|
||ID|按 ID 筛选（String，默认值：all）。|
|**查询选项**|$select|选择一部分属性。|
||$top|选择前多少个结果（默认值和最大值：$top = 100）。|
||$skip|跳过的结果数（默认值：$skip = 0）。|
|**响应**|“Message”实体列表。|“Message”实体包含“Id”(String)、“StartTime”(DateTimeOffset)、“EndTime”(DateTimeOffset)、“Status”(String)、“Messages”（“MessageHistory”实体列表）、“LastUpdatedTime”(DateTimeOffset)、“Workload”(String)、“WorkloadDisplayName”(String)、“Feature”(String)、“FeatureDisplayName”(String) 和“MessageType”（Enum，默认值：all）。<br/><br/>“MessageHistory”实体包含“PublishedTime”(DateTimeOffset) 和“MessageText”(String)。|
||||

### <a name="sample-request"></a>示例请求

```json
GET https://manage.office.com/api/v1.0/contoso.com/ServiceComms/Messages
Authorization: Bearer {AAD_Bearer_JWT_Token}
```

### <a name="sample-response"></a>示例响应

```json
{
 {
    "value": [
        {
            "Id": "EX20119",
            "Name": "EX20119",
            "Title": null,
            "StartTime": "2015-04-01T22:25:00Z",
            "EndTime": "2015-04-07T21:48:00Z",
            "Status": "Service restored",
            "Messages": [
                {
                    "PublishedTime": "2015-04-01T22:34:28.76Z",
                    "MessageText": "Current Status: Engineers are investigating an issue in which some customers may be experiencing problems accessing or using Exchange Online services or features. This event is actively being investigated. More information will be provided shortly."
                },
                {
                    "PublishedTime": "2015-04-03T18:45:32.56Z",
                    "MessageText": "Current Status: Engineers are implementing a fix within the affected infrastructure to remediate Exchange Online archive access issues. \n\nUser Experience: Affected users are unable to access online archives when using Outlook Web App (OWA). As a workaround, users may be able to access online archives via the Outlook client.\n\nCustomer Impact: Customer impact appears to be limited at this time. This issue only affects hybrid customers.\n\nPreliminary Root Cause: As part of our ongoing work, an updated certificate was deployed to the infrastructure; however, a caching issue has caused the infrastructure to use an expired certificate which is causing archive access issues.  \n\nNext Update by: Monday, April 6, 2015, at 9:00 PM UTC\n"
                },
                {
                    "PublishedTime": "2015-04-06T21:17:34.007Z",
                    "MessageText": "Current Status: Deployment of the fix is almost complete. Engineers are monitoring service health to ensure the issue has been remediated. \n\nUser Experience: Affected users are unable to access online archives when using Outlook Web App (OWA). As a workaround, users may be able to access online archives via the Outlook client. \n\nCustomer Impact: Customer impact appears to be limited at this time. This issue only affects hybrid customers.\n\nPreliminary Root Cause: As part of our ongoing work, an updated certificate was deployed to the infrastructure; however, a caching issue has caused the infrastructure to use an expired certificate which is causing archive access issues. \n\nNext Update by: Tuesday, April 7, 2015, at 10:00 PM UTC "
                },
                {
                    "PublishedTime": "2015-04-08T21:54:49.06Z",
                    "MessageText": "Final Status: Engineers have implemented a fix which remediated end-user impact. \r\n\r\nUser Experience: Affected users were unable to access online archives when using Outlook Web App (OWA).\r\n\r\nCustomer Impact: Customer impact appears to have been limited. This issue only affected hybrid customers.\r\n\r\nIncident Start Time: Monday, March 30, 2015, at 9:28 AM UTC\r\n\r\nIncident End Time: Tuesday, April 7, 2015, at 8:00 PM UTC\r\n\r\nPreliminary Root Cause: As part of our ongoing work, an updated certificate was deployed to the infrastructure; however, a caching issue has caused the infrastructure to use an expired certificate which is causing archive access issues.  \r\n\r\nNext Steps: The following is a list of known action item(s) associated with this incident. As part of the Office 365 problem management process, additional engineering actions may be identified to improve the overall service.\r\n- Action: Review the monitoring infrastructure for improvements as this event was reported by customers before an alert prompted a high priority investigation. \r\n\r\nA post-incident report will be available on the Service Health Dashboard within five business days."
                }
            ],
            "LastUpdatedTime": "2015-04-08T21:54:49.55Z",
            "Workload": "Exchange Online",
            "WorkloadDisplayName": "Exchange",
            "Feature": "Access",
            "FeatureDisplayName": "E-Mail and calendar access"
        },
        {
            "Id": "EX20789",
            "Name": "EX20789",
            "Title": null,
            "StartTime": "2015-04-06T20:00:00Z",
            "EndTime": "2015-04-07T23:00:00Z",
            "Status": "Service restored",
            "Messages": [
                {
                    "PublishedTime": "2015-04-07T23:26:44.643Z",
                    "MessageText": "Final Status: Engineers have validated that the deployed fix has resolved the issue and that service is restored.\r\n\r\nUser Experience: Affected users were unable to send or receive email while using Exchange Web Services (EWS) on their mobile devices.\r\n\r\nCustomer Impact: Customer impact appeared to be limited during this event. This issue was only affecting customers that use third-party Mobile Device Management software from Good Technology. As a workaround to provide immediate relief from impact, engineers implemented a configuration change for customers who reported this issue to Microsoft Support.\r\n\r\nIncident Start Time: Wednesday, April 1, 2015, at 2:00 PM UTC\r\n\r\nIncident End Time: Tuesday, April 7, 2015, at 10:00 PM UTC\r\n\r\nPreliminary Root Cause: As part of our ongoing efforts to improve service resiliency, an update was deployed to the infrastructure that facilitates connections from multiple Exchange Online protocols to mailbox databases. The update, however, contained a code issue that caused connectivity issues in some scenarios. \r\n\r\nNext Steps: The following is a list of known action item(s) associated with this incident. As part of the Office 365 problem management process, additional engineering actions may be identified to improve the overall service.\r\n- Action: Review the infrastructure update process to prevent reoccurrences of this scenario.\r\n\r\nPlease consider this service notification the final update on the event."
               }
            ],
            "LastUpdatedTime": "2015-04-07T23:26:45.08Z",
            "Workload": "Exchange Online",
            "WorkloadDisplayName": "Exchange",
            "Feature": "Access",
            "FeatureDisplayName": "E-Mail and calendar access"
        }
    ]
}
```


## <a name="errors"></a>错误

如果遇到错误，服务会使用标准 HTTP 错误代码语法，向调用方报告错误响应代码。 根据 [OData V4 规范](http://docs.oasis-open.org/odata/odata/v4.0/odata-v4.0-part1-protocol.html)，失败调用的正文中包含其他信息（作为一个 JSON 对象）。 下面的示例展示了完整的 JSON 错误正文：


```json
{ 
    "error":{ 
        "code":"AF5000.  An internal server error occurred.",
        "message": "Retry the request." 
    } 
}
```
