---
ms.TocTitle: Office 365 Management Activity API reference
title: Office 365 管理活动 API 参考
description: 使用 Office 365 管理活动 API，可从 Office 365 和 Azure AD 活动日志中获取用户、管理员、系统和策略的操作和事件相关信息。
ms.ContentId: 52749845-37f8-6076-7ea5-49d9a4055445
ms.topic: reference (API)
ms.date: 01/10/2018
localization_priority: Priority
ms.openlocfilehash: 76907cf9f22078a232cc20e65ba5fdc12c7f5d7e
ms.sourcegitcommit: 55264094d1ebc2f9968b2d29d5982b1ba4e29118
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/04/2019
ms.locfileid: "29735227"
---
# <a name="office-365-management-activity-api-reference"></a><span data-ttu-id="10b92-103">Office 365 管理活动 API 参考</span><span class="sxs-lookup"><span data-stu-id="10b92-103">Office 365 Management Activity API reference</span></span>

<span data-ttu-id="10b92-104">使用 Office 365 管理活动 API，可从 Office 365 和 Azure AD 活动日志中获取用户、管理员、系统和策略的操作和事件相关信息。</span><span class="sxs-lookup"><span data-stu-id="10b92-104">Use the Office 365 Management Activity API to retrieve information about user, admin, system, and policy actions and events from Office 365 and Azure AD activity logs.</span></span> 

<span data-ttu-id="10b92-105">可使用 Office 365 和 Microsoft Azure Active Directory 审核和活动日志中的操作和事件，创建可提供监视、分析和数据可视化的解决方案。</span><span class="sxs-lookup"><span data-stu-id="10b92-105">You can use the actions and events from the Office 365 and Microsoft Azure Active Directory audit and activity logs to create solutions that provide monitoring, analysis, and data visualization.</span></span> <span data-ttu-id="10b92-106">借助这些解决方案，组织可以深入了解对自己内容执行的操作。</span><span class="sxs-lookup"><span data-stu-id="10b92-106">These solutions give organizations greater visibility into actions taken on their content.</span></span> <span data-ttu-id="10b92-107">Office 365 活动报告中也包含这些操作和事件。</span><span class="sxs-lookup"><span data-stu-id="10b92-107">These actions and events are also available in the Office 365 Activity Reports.</span></span> <span data-ttu-id="10b92-108">有关详细信息，请参阅[在 Office 365 安全与合规中心内搜索审核日志](https://support.office.com/zh-CN/article/Search-the-audit-log-in-the-Office-365-Security-Compliance-Center-0d4d0f35-390b-4518-800e-0c7ec95e946c)。</span><span class="sxs-lookup"><span data-stu-id="10b92-108">For more information, see [Search the audit log in the Office 365 Security & Compliance Center](https://support.office.com/zh-CN/article/Search-the-audit-log-in-the-Office-365-Security-Compliance-Center-0d4d0f35-390b-4518-800e-0c7ec95e946c).</span></span>

<span data-ttu-id="10b92-109">借助 Office 365 服务活动 API 这项 REST Web 服务，可开发使用任何语言和宿主环境（支持 HTTPS 和 X.509 证书）的解决方案。</span><span class="sxs-lookup"><span data-stu-id="10b92-109">The Office 365 Management Activity API is a REST web service that you can use to develop solutions using any language and hosting environment that supports HTTPS and X.509 certificates.</span></span> <span data-ttu-id="10b92-110">此 API 依赖 Azure AD 和 OAuth2 协议进行身份验证和授权。</span><span class="sxs-lookup"><span data-stu-id="10b92-110">The API relies on Azure AD and the OAuth2 protocol for authentication and authorization.</span></span> <span data-ttu-id="10b92-111">若要在应用程序中访问此 API，必须先在 Azure AD 中注册应用程序，并为它配置适当的权限。</span><span class="sxs-lookup"><span data-stu-id="10b92-111">To access the API from your application, you'll need to first register it in Azure AD and configure it with appropriate permissions.</span></span> <span data-ttu-id="10b92-112">这样，应用程序便能请求获取调用此 API 所需的 OAuth2 访问令牌。</span><span class="sxs-lookup"><span data-stu-id="10b92-112">This will enable your application to request the OAuth2 access tokens it needs to call the API.</span></span> <span data-ttu-id="10b92-113">有关详细信息，请参阅 [Office 365 管理 API 入门](get-started-with-office-365-management-apis.md)。</span><span class="sxs-lookup"><span data-stu-id="10b92-113">For more information, see [Get started with Office 365 Management APIs](get-started-with-office-365-management-apis.md).</span></span>

> [!NOTE] 
> <span data-ttu-id="10b92-114">若要了解 Office 365 管理活动 API 返回的数据的架构（包括可能会影响实现的已知问题和即将发生的更改），请参阅 [Office 365 管理活动 API 架构](office-365-management-activity-api-schema.md)。</span><span class="sxs-lookup"><span data-stu-id="10b92-114">For information about the schema of the data that the Office 365 Management Activity API returns, including known issues and upcoming changes, that might affect your implementation, see [Office 365 Management Activity API schema](office-365-management-activity-api-schema.md).</span></span>


## <a name="working-with-the-office-365-management-activity-api"></a><span data-ttu-id="10b92-115">使用 Office 365 管理活动 API</span><span class="sxs-lookup"><span data-stu-id="10b92-115">Working with the Office 365 Management Activity API</span></span>

<span data-ttu-id="10b92-116">Office 365 管理活动 API 将操作和事件聚合到租户专用内容 blob 中，这些 blob 按其中所含内容的类型和来源进行分类。</span><span class="sxs-lookup"><span data-stu-id="10b92-116">The Office 365 Management Activity API aggregates actions and events into tenant-specific content blobs, which are classified by the type and source of the content they contain.</span></span> <span data-ttu-id="10b92-117">目前支持以下内容类型：</span><span class="sxs-lookup"><span data-stu-id="10b92-117">Currently, these content types are supported:</span></span>

- <span data-ttu-id="10b92-118">Audit.AzureActiveDirectory</span><span class="sxs-lookup"><span data-stu-id="10b92-118">Audit.AzureActiveDirectory</span></span>
    
- <span data-ttu-id="10b92-119">Audit.Exchange</span><span class="sxs-lookup"><span data-stu-id="10b92-119">Audit.Exchange</span></span>
    
- <span data-ttu-id="10b92-120">Audit.SharePoint</span><span class="sxs-lookup"><span data-stu-id="10b92-120">Audit.SharePoint</span></span>
    
- <span data-ttu-id="10b92-121">Audit.General（包括前面内容类型不包含的其他所有工作负载）</span><span class="sxs-lookup"><span data-stu-id="10b92-121">Audit.General (includes all other workloads not included in the previous content types)</span></span>

- <span data-ttu-id="10b92-122">DLP.All（仅包括 DLP 事件的所有工作负载）</span><span class="sxs-lookup"><span data-stu-id="10b92-122">DLP.All (DLP events only for all workloads)</span></span>
    
<span data-ttu-id="10b92-123">若要详细了解与这些内容类型关联的事件和属性，请参阅 [Office 365 管理活动 API 架构](office-365-management-activity-api-schema.md)。</span><span class="sxs-lookup"><span data-stu-id="10b92-123">For details about the events and properties associated with these content types, see [Office 365 Management Activity API schema](office-365-management-activity-api-schema.md).</span></span>

<span data-ttu-id="10b92-124">若要开始检索某租户的内容 blob，请先创建对相应内容类型的订阅。</span><span class="sxs-lookup"><span data-stu-id="10b92-124">To begin retrieving content blobs for a tenant, you first a create subscription to the desired content types.</span></span> <span data-ttu-id="10b92-125">若要检索多个租户的内容 blob，请创建对每个相应内容类型的多个订阅（每个租户一个订阅）。</span><span class="sxs-lookup"><span data-stu-id="10b92-125">If you are retrieving content blobs for multiple tenants, you create multiple subscriptions to each of the desired content types, one for each tenant.</span></span>

<span data-ttu-id="10b92-126">创建订阅后，可以通过定期轮询来发现可供下载的新内容 blob，也可以向订阅注册 Webhook 终结点，这样我们便会在有新内容 blob 时向此终结点发送通知。</span><span class="sxs-lookup"><span data-stu-id="10b92-126">After you create a subscription, you can poll regularly to discover new content blobs that are available for download, or you can register a webhook endpoint with the subscription and we will send notifications to this endpoint as new content blobs are available.</span></span>


> [!NOTE] 
> <span data-ttu-id="10b92-127">创建订阅后，第一批内容 blob 最多可能需要 12 个小时才可用于订阅。</span><span class="sxs-lookup"><span data-stu-id="10b92-127">When a subscription is created, it can take up to 12 hours for the first content blobs to become available for that subscription.</span></span> <span data-ttu-id="10b92-128">内容 blob 的创建方式为，跨多个服务器和数据中心收集并聚合操作和事件。</span><span class="sxs-lookup"><span data-stu-id="10b92-128">The content blobs are creating by collecting and aggregating actions and events across multiple servers and datacenters.</span></span> <span data-ttu-id="10b92-129">鉴于此分布式流程，内容 blob 中的操作和事件不一定按发生时间顺序显示。</span><span class="sxs-lookup"><span data-stu-id="10b92-129">As a result of this distributed process, the actions and events contained in the content blobs will not necessarily appear in the order in which they occurred.</span></span> <span data-ttu-id="10b92-130">可能会出现下面这种情况：一个内容 blob 中的操作和事件早于前面内容 blob 中的操作和事件发生。</span><span class="sxs-lookup"><span data-stu-id="10b92-130">One content blob can contain actions and events that occurred prior to the actions and events contained in an earlier content blob.</span></span> <span data-ttu-id="10b92-131">我们正在努力缩小操作和事件的发生时间与其在内容 blob 中的可用时间之间的延迟，但无法确保它们按时间顺序依序显示。</span><span class="sxs-lookup"><span data-stu-id="10b92-131">We are working to decrease the latency between the occurrence of actions and events and their availability within a content blob, but we cannot guarantee that they appear sequentially.</span></span>


> [!NOTE] 
> <span data-ttu-id="10b92-132">只有已拥有“读取 DLP 敏感数据”权限的用户，才能通过活动源 API 访问 DLP 敏感数据。</span><span class="sxs-lookup"><span data-stu-id="10b92-132">DLP sensitive data is only available in the activity feed API to users that have been granted “Read DLP sensitive data” permissions.</span></span> <span data-ttu-id="10b92-133">若要详细了解数据丢失防护 (DLP)，请参阅[数据丢失防护策略概述](https://support.office.com/zh-CN/article/Overview-of-data-loss-prevention-policies-1966b2a7-d1e2-4d92-ab61-42efbb137f5e)。</span><span class="sxs-lookup"><span data-stu-id="10b92-133">For more on Data Loss Prevention (DLP) see [Overview of Data Loss Prevention Policies](https://support.office.com/zh-CN/article/Overview-of-data-loss-prevention-policies-1966b2a7-d1e2-4d92-ab61-42efbb137f5e)</span></span>

## <a name="activity-api-operations"></a><span data-ttu-id="10b92-134">活动 API 操作</span><span class="sxs-lookup"><span data-stu-id="10b92-134">Activity API operations</span></span>

<span data-ttu-id="10b92-135">所有 API 操作的范围都限定为一个租户，此 API 的根 URL 包含指定租户上下文的租户 ID。</span><span class="sxs-lookup"><span data-stu-id="10b92-135">All API operations are scoped to a single tenant and the root URL of the API includes a tenant ID that specifies the tenant context.</span></span> <span data-ttu-id="10b92-136">租户 ID 为 GUID。</span><span class="sxs-lookup"><span data-stu-id="10b92-136">The tenant ID is a GUID.</span></span> <span data-ttu-id="10b92-137">若要详细了解如何获取 GUID，请参阅 [Office 365 管理 API 入门](get-started-with-office-365-management-apis.md)。</span><span class="sxs-lookup"><span data-stu-id="10b92-137">For information about how to get the GUID, see [Get started with Office 365 Management APIs](get-started-with-office-365-management-apis.md).</span></span>


```http
https://manage.office.com/api/v1.0/{tenant_id}/activity/feed/{operation}
```

<span data-ttu-id="10b92-138">由于我们发送到 Webhook 的通知包含**租户 ID**，因此你可使用相同的 Webhook 来接收所有租户的通知。</span><span class="sxs-lookup"><span data-stu-id="10b92-138">Because the notifications we send to your webhook include the **tenant ID**, you can use the same webhook to receive notifications for all tenants.</span></span>

<span data-ttu-id="10b92-139">所有 API 操作都要求，必须有包含从 Azure AD 获取的访问令牌的授权 HTTP 头。</span><span class="sxs-lookup"><span data-stu-id="10b92-139">All API operations require an Authorization HTTP header with an access token obtained from Azure AD.</span></span> <span data-ttu-id="10b92-140">访问令牌中的租户 ID 必须与 API 根 URL 中的租户 ID 一致，且访问令牌必须包含 ActivityFeed.Read 声明（此声明对应于在 Azure AD 中为应用程序配置的[读取组织的活动数据]权限）。</span><span class="sxs-lookup"><span data-stu-id="10b92-140">The tenant ID in the access token must match the tenant ID in the root URL of the API and the access token must contain the ActivityFeed.Read claim (this corresponds to the permission [Read activity data for an organization] that you configured for you application in Azure AD).</span></span>

```json
Authorization: Bearer eyJ0e...Qa6wg
```

<span data-ttu-id="10b92-141">活动 API 支持以下操作：</span><span class="sxs-lookup"><span data-stu-id="10b92-141">The Activity API supports the following operations:</span></span>

- <span data-ttu-id="10b92-142">**启动订阅**：以便开始接收通知，并检索租户的活动数据。</span><span class="sxs-lookup"><span data-stu-id="10b92-142">**Start a subscription** to begin receiving notifications and retrieving activity data for a tenant.</span></span>
    
- <span data-ttu-id="10b92-143">**停止订阅**：以便停止检索租户数据。</span><span class="sxs-lookup"><span data-stu-id="10b92-143">**Stop a subscription** to discontinue retrieving data for a tenant.</span></span>
    
- <span data-ttu-id="10b92-144">**列出当前订阅**</span><span class="sxs-lookup"><span data-stu-id="10b92-144">**List current subscriptions**</span></span>
    
- <span data-ttu-id="10b92-145">**列出可用内容**及相应的内容 URL。</span><span class="sxs-lookup"><span data-stu-id="10b92-145">**List available content** and the corresponding content URLs.</span></span>
    
- <span data-ttu-id="10b92-146">**接收通知**：通知是在有新内容时由 Webhook 发送。</span><span class="sxs-lookup"><span data-stu-id="10b92-146">**Receiving notifications** sent by a webhook when new content is available.</span></span>
    
- <span data-ttu-id="10b92-147">**检索内容**：使用内容 URL 进行检索。</span><span class="sxs-lookup"><span data-stu-id="10b92-147">**Retrieving content** by using the content URL.</span></span>
    
- <span data-ttu-id="10b92-148">**列出通知**：通知是由 Webhook 发送。</span><span class="sxs-lookup"><span data-stu-id="10b92-148">**List notifications** sent by a webhook.</span></span>

- <span data-ttu-id="10b92-149">**检索资源易记名称**：数据源中由 GUID 标识的对象的名称。</span><span class="sxs-lookup"><span data-stu-id="10b92-149">**Retrieve resource friendly names** for objects in the data feed identified by guids.</span></span>
    

## <a name="start-a-subscription"></a><span data-ttu-id="10b92-150">启动订阅</span><span class="sxs-lookup"><span data-stu-id="10b92-150">Start a subscription</span></span>

<span data-ttu-id="10b92-151">此操作会启动对指定内容类型的订阅。</span><span class="sxs-lookup"><span data-stu-id="10b92-151">This operation starts a subscription to the specified content type.</span></span> <span data-ttu-id="10b92-152">如果已有对指定内容类型的订阅，此操作用于：</span><span class="sxs-lookup"><span data-stu-id="10b92-152">If a subscription to the specified content type already exists, this operation is used to:</span></span>

- <span data-ttu-id="10b92-153">更新有效 Webhook 的属性。</span><span class="sxs-lookup"><span data-stu-id="10b92-153">Update the properties of an active webhook.</span></span>
    
- <span data-ttu-id="10b92-154">启用因通知发送失败次数过多而被禁用的 Webhook。</span><span class="sxs-lookup"><span data-stu-id="10b92-154">Enable a webhook that was disabled because of excessive failed notifications.</span></span>
    
- <span data-ttu-id="10b92-155">通过指定更晚或为空的到期日期，重新启用已到期的 Webhook。</span><span class="sxs-lookup"><span data-stu-id="10b92-155">Re-enable an expired webhook by specifying a later or null expiration date.</span></span>
    
- <span data-ttu-id="10b92-156">删除 Webhook。</span><span class="sxs-lookup"><span data-stu-id="10b92-156">Remove a webhook.</span></span>
    
||<span data-ttu-id="10b92-157">订阅</span><span class="sxs-lookup"><span data-stu-id="10b92-157">Subscription</span></span>|<span data-ttu-id="10b92-158">说明</span><span class="sxs-lookup"><span data-stu-id="10b92-158">Description</span></span>|
|:-----|:-----|:-----|
|<span data-ttu-id="10b92-159">**路径**</span><span class="sxs-lookup"><span data-stu-id="10b92-159">**Path**</span></span>| `/subscriptions/start?contentType={ContentType}`||
|<span data-ttu-id="10b92-160">**参数**</span><span class="sxs-lookup"><span data-stu-id="10b92-160">**Parameters**</span></span>|<span data-ttu-id="10b92-161">contentType</span><span class="sxs-lookup"><span data-stu-id="10b92-161">contentType</span></span>|<span data-ttu-id="10b92-162">必须为有效内容类型。</span><span class="sxs-lookup"><span data-stu-id="10b92-162">Must be a valid content type.</span></span>|
||<span data-ttu-id="10b92-163">PublisherIdentifier</span><span class="sxs-lookup"><span data-stu-id="10b92-163">PublisherIdentifier</span></span>|<span data-ttu-id="10b92-164">对 API 编码的供应商的租户 GUID。</span><span class="sxs-lookup"><span data-stu-id="10b92-164">The tenant GUID of the vendor coding against the API.</span></span> <span data-ttu-id="10b92-165">这**不是**应用程序 GUID 或使用应用程序的客户的 GUID，而是编写代码的公司的 GUID。</span><span class="sxs-lookup"><span data-stu-id="10b92-165">This is **not** the application GUID or the GUID of the customer using the application, but the GUID of the company writing the code.</span></span> <span data-ttu-id="10b92-166">此参数用于限制请求速率。</span><span class="sxs-lookup"><span data-stu-id="10b92-166">This parameter is used for throttling the request rate.</span></span> <span data-ttu-id="10b92-167">请务必在发出的所有请求中指定此参数，以获取专用配额。</span><span class="sxs-lookup"><span data-stu-id="10b92-167">Make sure this parameter is specified in all issued requests to get a dedicated quota.</span></span> <span data-ttu-id="10b92-168">收到的所有不包含此参数的请求都会共用同一配额。</span><span class="sxs-lookup"><span data-stu-id="10b92-168">All requests received without this parameter will share the same quota.</span></span>|
|<span data-ttu-id="10b92-169">**正文**</span><span class="sxs-lookup"><span data-stu-id="10b92-169">**Body**</span></span>|<span data-ttu-id="10b92-170">webhook</span><span class="sxs-lookup"><span data-stu-id="10b92-170">webhook</span></span>|<span data-ttu-id="10b92-171">可选 JSON 对象，包含以下三个属性：</span><span class="sxs-lookup"><span data-stu-id="10b92-171">Optional JSON object with three properties:</span></span><ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span data-ttu-id="10b92-172"><b>address</b>：可接收通知的必需 HTTPS 终结点。</span><span class="sxs-lookup"><span data-stu-id="10b92-172"><b>address</b>: Required HTTPS endpoint that can receive notifications.</span></span>  <span data-ttu-id="10b92-173">在订阅创建前，Webhook 会收到用于验证自身的测试消息。</span><span class="sxs-lookup"><span data-stu-id="10b92-173">A test message will be sent to the webhook to validate the webhook before creating the subscription.</span></span></p></li><li><p><span data-ttu-id="10b92-174"><b>authId</b>：可选字符串，作为 WebHook-AuthID 头包含在向 Webhook 发送的通知中，用于向 Webhook 标识和授权请求来源。</span><span class="sxs-lookup"><span data-stu-id="10b92-174"><b>authId</b>: Optional string that will be included as the WebHook-AuthID header in notifications sent to the webhook as a means of identifying and authorizing the source of the request to the webhook.</span></span></p></li><li><p><span data-ttu-id="10b92-175"><b>expiration</b>：可选日期/时间，指明在此日期/时间后便不得再向 Webhook 发送通知。</span><span class="sxs-lookup"><span data-stu-id="10b92-175"><b>expiration</b>: Optional datetime that indicates a datetime after which notifications should no longer be sent to the webhook.</span></span></p></li></ul>|
|<span data-ttu-id="10b92-176">**响应**</span><span class="sxs-lookup"><span data-stu-id="10b92-176">**Response**</span></span>|<span data-ttu-id="10b92-177">contentType</span><span class="sxs-lookup"><span data-stu-id="10b92-177">contentType</span></span>|<span data-ttu-id="10b92-178">在调用中指定的内容类型。</span><span class="sxs-lookup"><span data-stu-id="10b92-178">The content type specified in the call.</span></span>|
||<span data-ttu-id="10b92-179">status</span><span class="sxs-lookup"><span data-stu-id="10b92-179">status</span></span>|<span data-ttu-id="10b92-180">订阅的状态。</span><span class="sxs-lookup"><span data-stu-id="10b92-180">The status of the subscription.</span></span> <span data-ttu-id="10b92-181">如果订阅已遭禁用，便无法列出或检索内容。</span><span class="sxs-lookup"><span data-stu-id="10b92-181">If a subscription is disabled, you will not be able to list or retrieve content.</span></span>|
||<span data-ttu-id="10b92-182">webhook</span><span class="sxs-lookup"><span data-stu-id="10b92-182">webhook</span></span>|<span data-ttu-id="10b92-183">在调用中指定的 Webhook 属性，以及 Webhook 的状态。</span><span class="sxs-lookup"><span data-stu-id="10b92-183">The webhook properties specified in the call together with the status of the webhook.</span></span> <span data-ttu-id="10b92-184">如果 Webhook 已遭禁用，便无法收到通知，但仍可以列出和检索内容，只要订阅处于启用状态即可。</span><span class="sxs-lookup"><span data-stu-id="10b92-184">If the webhook is disabled, you will not receive notification, but you will still be able to list and retrieve content, provided the subscription is enabled.</span></span>|

#### <a name="sample-request"></a><span data-ttu-id="10b92-185">示例请求</span><span class="sxs-lookup"><span data-stu-id="10b92-185">Sample request</span></span>

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


#### <a name="sample-response"></a><span data-ttu-id="10b92-186">示例响应</span><span class="sxs-lookup"><span data-stu-id="10b92-186">Sample response</span></span>

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


## <a name="webhook-validation"></a><span data-ttu-id="10b92-187">Webhook 验证</span><span class="sxs-lookup"><span data-stu-id="10b92-187">Webhook validation</span></span>

<span data-ttu-id="10b92-188">在 /start 操作已调用且 Webhook 已指定后，我们会向指定 Webhook 地址发送验证通知，以验证有效侦听器能否接受并处理通知。</span><span class="sxs-lookup"><span data-stu-id="10b92-188">When the /start operation is called and a webhook is specified, we will send a validation notification to the specified webhook address to validate that an active listener can accept and process notifications.</span></span> <span data-ttu-id="10b92-189">如果我们没有收到“HTTP 200 正常”响应，便不会创建订阅。</span><span class="sxs-lookup"><span data-stu-id="10b92-189">If we do not receive an HTTP 200 OK response, the subscription will not be created.</span></span> <span data-ttu-id="10b92-190">或者，如果 /start 正获调用以向现有订阅添加 Webhook，但我们没有收到“HTTP 200 正常”响应，便不会添加此 Webhook，但订阅仍保持不变。</span><span class="sxs-lookup"><span data-stu-id="10b92-190">Or, if /start is being called to add a webhook to an existing subscription and a response of HTTP 200 OK is not received, the webhook will not be added and the subscription will remain unchanged.</span></span>

#### <a name="sample-request"></a><span data-ttu-id="10b92-191">示例请求</span><span class="sxs-lookup"><span data-stu-id="10b92-191">Sample request</span></span>

```json
POST {webhook address}
Content-Type: application/json; charset=utf-8
Webhook-AuthID: (webhook authId) if provided)
Webhook-ValidationCode: (random opaque string)

{
    "validationCode": (random opaque string, same as header)
}

```


#### <a name="sample-response"></a><span data-ttu-id="10b92-192">示例响应</span><span class="sxs-lookup"><span data-stu-id="10b92-192">Sample response</span></span>

```json

HTTP/1.1 200 OK

```


## <a name="stop-a-subscription"></a><span data-ttu-id="10b92-193">停止订阅</span><span class="sxs-lookup"><span data-stu-id="10b92-193">Stop a subscription</span></span>

<span data-ttu-id="10b92-194">此操作会停止对指定内容类型的订阅。</span><span class="sxs-lookup"><span data-stu-id="10b92-194">This operation stops a subscription to the specified content type.</span></span> 

<span data-ttu-id="10b92-195">在订阅停止后，便不再会收到通知，也无法再检索可用内容。</span><span class="sxs-lookup"><span data-stu-id="10b92-195">When a subscription is stopped, you will no longer receive notifications and you will not be able to retrieve available content.</span></span> <span data-ttu-id="10b92-196">如果今后重启订阅，将有权访问重启时间点后的新内容，</span><span class="sxs-lookup"><span data-stu-id="10b92-196">If the subscription is later restarted, you will have access to new content from that point forward.</span></span> <span data-ttu-id="10b92-197">但无法检索从订阅停止时间到重启时间之间的可用内容。</span><span class="sxs-lookup"><span data-stu-id="10b92-197">You will not be able to retrieve content that was available between the time the subscription was stopped and restarted.</span></span>


||<span data-ttu-id="10b92-198">订阅</span><span class="sxs-lookup"><span data-stu-id="10b92-198">Subscription</span></span>|<span data-ttu-id="10b92-199">说明</span><span class="sxs-lookup"><span data-stu-id="10b92-199">Description</span></span>|
|:-----|:-----|:-----|
|<span data-ttu-id="10b92-200">**路径**</span><span class="sxs-lookup"><span data-stu-id="10b92-200">**Path**</span></span>| `/subscriptions/stop?contentType={ContentType}`||
|<span data-ttu-id="10b92-201">**参数**</span><span class="sxs-lookup"><span data-stu-id="10b92-201">**Parameters**</span></span>|<span data-ttu-id="10b92-202">contentType</span><span class="sxs-lookup"><span data-stu-id="10b92-202">contentType</span></span>|<span data-ttu-id="10b92-203">必须为有效内容类型。</span><span class="sxs-lookup"><span data-stu-id="10b92-203">Must be a valid content type.</span></span>|
||<span data-ttu-id="10b92-204">PublisherIdentifier</span><span class="sxs-lookup"><span data-stu-id="10b92-204">PublisherIdentifier</span></span>|<span data-ttu-id="10b92-205">对 API 编码的供应商的租户 GUID。</span><span class="sxs-lookup"><span data-stu-id="10b92-205">The tenant GUID of the vendor coding against the API.</span></span> <span data-ttu-id="10b92-206">这**不是**应用程序 GUID 或使用应用程序的客户的 GUID，而是编写代码的公司的 GUID。</span><span class="sxs-lookup"><span data-stu-id="10b92-206">This is **not** the application GUID or the GUID of the customer using the application, but the GUID of the company writing the code.</span></span> <span data-ttu-id="10b92-207">此参数用于限制请求速率。</span><span class="sxs-lookup"><span data-stu-id="10b92-207">This parameter is used for throttling the request rate.</span></span> <span data-ttu-id="10b92-208">请务必在发出的所有请求中指定此参数，以获取专用配额。</span><span class="sxs-lookup"><span data-stu-id="10b92-208">Make sure this parameter is specified in all issued requests to get a dedicated quota.</span></span> <span data-ttu-id="10b92-209">收到的所有不包含此参数的请求都会共用同一配额。</span><span class="sxs-lookup"><span data-stu-id="10b92-209">All requests received without this parameter will share the same quota.</span></span>|
|<span data-ttu-id="10b92-210">**正文**</span><span class="sxs-lookup"><span data-stu-id="10b92-210">**Body**</span></span>|<span data-ttu-id="10b92-211">（空）</span><span class="sxs-lookup"><span data-stu-id="10b92-211">(empty)</span></span>||
|<span data-ttu-id="10b92-212">**响应**</span><span class="sxs-lookup"><span data-stu-id="10b92-212">**Response**</span></span>|<span data-ttu-id="10b92-213">（空）</span><span class="sxs-lookup"><span data-stu-id="10b92-213">(empty)</span></span>|||

#### <a name="sample-request"></a><span data-ttu-id="10b92-214">示例请求</span><span class="sxs-lookup"><span data-stu-id="10b92-214">Sample request</span></span>

```json
POST {root}/subscriptions/stop?contentType=Audit.SharePoint&PublisherIdentifier=46b472a7-c68e-4adf-8ade-3db49497518e
Authorization: Bearer eyJ0e...Qa6wg

```

#### <a name="sample-response"></a><span data-ttu-id="10b92-215">示例响应</span><span class="sxs-lookup"><span data-stu-id="10b92-215">Sample response</span></span>

```json
HTTP/1.1 200 OK
```


## <a name="list-current-subscriptions"></a><span data-ttu-id="10b92-216">列出当前订阅</span><span class="sxs-lookup"><span data-stu-id="10b92-216">List current subscriptions</span></span>

<span data-ttu-id="10b92-217">此操作会返回包含当前订阅及相关 Webhook 的集合。</span><span class="sxs-lookup"><span data-stu-id="10b92-217">This operation returns a collection of the current subscriptions together with the associated webhooks.</span></span>

||<span data-ttu-id="10b92-218">订阅</span><span class="sxs-lookup"><span data-stu-id="10b92-218">Subscription</span></span>|<span data-ttu-id="10b92-219">说明</span><span class="sxs-lookup"><span data-stu-id="10b92-219">Description</span></span>|
|:-----|:-----|:-----|
|<span data-ttu-id="10b92-220">**路径**</span><span class="sxs-lookup"><span data-stu-id="10b92-220">**Path**</span></span>| `/subscriptions/list`||
|<span data-ttu-id="10b92-221">**参数**</span><span class="sxs-lookup"><span data-stu-id="10b92-221">**Parameters**</span></span>|<span data-ttu-id="10b92-222">PublisherIdentifier</span><span class="sxs-lookup"><span data-stu-id="10b92-222">PublisherIdentifier</span></span>|<span data-ttu-id="10b92-223">对 API 编码的供应商的租户 GUID。</span><span class="sxs-lookup"><span data-stu-id="10b92-223">The tenant GUID of the vendor coding against the API.</span></span> <span data-ttu-id="10b92-224">这**不是**应用程序 GUID 或使用应用程序的客户的 GUID，而是编写代码的公司的 GUID。</span><span class="sxs-lookup"><span data-stu-id="10b92-224">This is **not** the application GUID or the GUID of the customer using the application, but the GUID of the company writing the code.</span></span> <span data-ttu-id="10b92-225">此参数用于限制请求速率。</span><span class="sxs-lookup"><span data-stu-id="10b92-225">This parameter is used for throttling the request rate.</span></span> <span data-ttu-id="10b92-226">请务必在发出的所有请求中指定此参数，以获取专用配额。</span><span class="sxs-lookup"><span data-stu-id="10b92-226">Make sure this parameter is specified in all issued requests to get a dedicated quota.</span></span> <span data-ttu-id="10b92-227">收到的所有不包含此参数的请求都会共用同一配额。</span><span class="sxs-lookup"><span data-stu-id="10b92-227">All requests received without this parameter will share the same quota.</span></span>|
|<span data-ttu-id="10b92-228">**正文**</span><span class="sxs-lookup"><span data-stu-id="10b92-228">**Body**</span></span>|<span data-ttu-id="10b92-229">（空）</span><span class="sxs-lookup"><span data-stu-id="10b92-229">(empty)</span></span>||
|<span data-ttu-id="10b92-230">**响应**</span><span class="sxs-lookup"><span data-stu-id="10b92-230">**Response**</span></span>|<span data-ttu-id="10b92-231">JSON 数组</span><span class="sxs-lookup"><span data-stu-id="10b92-231">JSON array</span></span>|<span data-ttu-id="10b92-232">每个订阅都由包含以下三个属性的 JSON 对象表示：</span><span class="sxs-lookup"><span data-stu-id="10b92-232">Each subscription will be represented by a JSON object with three properties:</span></span><ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span data-ttu-id="10b92-233"><b>contentType</b>：指明内容类型。</span><span class="sxs-lookup"><span data-stu-id="10b92-233"><b>contentType</b>: Indicates the content type.</span></span></p></li><li><p><span data-ttu-id="10b92-234"><b>status</b>：指明订阅的状态。</span><span class="sxs-lookup"><span data-stu-id="10b92-234"><b>status</b>: Indicates the status of the subscription.</span></span></p></li><li><p><span data-ttu-id="10b92-235"><b>webhook</b>：指明已配置的 Webhook 及其状态（已启用、已禁用或已到期）。</span><span class="sxs-lookup"><span data-stu-id="10b92-235"><b>webhook</b>: Indicates the configured webhook, together with the status (enabled, disabled, expired) of the webhook.</span></span>  <span data-ttu-id="10b92-236">如果订阅没有 Webhook，虽然 Webhook 属性会显示，但值为 null。</span><span class="sxs-lookup"><span data-stu-id="10b92-236">If a subscription does not have a webhook, the webhook property will be present but with null value.</span></span></p></li></ul>|


#### <a name="sample-request"></a><span data-ttu-id="10b92-237">示例请求</span><span class="sxs-lookup"><span data-stu-id="10b92-237">Sample request</span></span>

```json
GET {root}/subscriptions/list?PublisherIdentifier=46b472a7-c68e-4adf-8ade-3db49497518e
Authorization: Bearer eyJ0e...Qa6wg

```

#### <a name="sample-response"></a><span data-ttu-id="10b92-238">示例响应</span><span class="sxs-lookup"><span data-stu-id="10b92-238">Sample response</span></span>

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


## <a name="list-available-content"></a><span data-ttu-id="10b92-239">列出可用内容</span><span class="sxs-lookup"><span data-stu-id="10b92-239">List available content</span></span>

<span data-ttu-id="10b92-240">此操作会列出当前可供检索的指定内容类型内容。</span><span class="sxs-lookup"><span data-stu-id="10b92-240">This operation lists the content currently available for retrieval for the specified content type.</span></span> <span data-ttu-id="10b92-241">内容中聚合了从跨多个数据中心的多个服务器中捕获的操作和事件。</span><span class="sxs-lookup"><span data-stu-id="10b92-241">The content is an aggregation of actions and events harvested from multiple servers across multiple datacenters.</span></span> <span data-ttu-id="10b92-242">内容按聚合可用时间顺序列出，但无法保证聚合中的事件和操作按时间顺序依序列出。</span><span class="sxs-lookup"><span data-stu-id="10b92-242">The content will be listed in the order in which the aggregations become available, but the events and actions within the aggregations are not guaranteed to be sequential.</span></span> <span data-ttu-id="10b92-243">如果订阅处于已禁用状态，返回的便是错误。</span><span class="sxs-lookup"><span data-stu-id="10b92-243">An error is returned if the subscription status is disabled.</span></span>


||<span data-ttu-id="10b92-244">订阅</span><span class="sxs-lookup"><span data-stu-id="10b92-244">Subscription</span></span>|<span data-ttu-id="10b92-245">说明</span><span class="sxs-lookup"><span data-stu-id="10b92-245">Description</span></span>|
|:-----|:-----|:-----|
|<span data-ttu-id="10b92-246">**路径**</span><span class="sxs-lookup"><span data-stu-id="10b92-246">**Path**</span></span>| `/subscriptions/content?contentType={ContentType}&amp;startTime={0}&amp;endTime={1}`||
|<span data-ttu-id="10b92-247">**参数**</span><span class="sxs-lookup"><span data-stu-id="10b92-247">**Parameters**</span></span>|<span data-ttu-id="10b92-248">contentType</span><span class="sxs-lookup"><span data-stu-id="10b92-248">contentType</span></span>|<span data-ttu-id="10b92-249">必须为有效内容类型。</span><span class="sxs-lookup"><span data-stu-id="10b92-249">Must be a valid content type.</span></span>|
||<span data-ttu-id="10b92-250">PublisherIdentifier</span><span class="sxs-lookup"><span data-stu-id="10b92-250">PublisherIdentifier</span></span>|<span data-ttu-id="10b92-251">对 API 编码的供应商的租户 GUID。</span><span class="sxs-lookup"><span data-stu-id="10b92-251">The tenant GUID of the vendor coding against the API.</span></span> <span data-ttu-id="10b92-252">这**不是**应用程序 GUID 或使用应用程序的客户的 GUID，而是编写代码的公司的 GUID。</span><span class="sxs-lookup"><span data-stu-id="10b92-252">This is **not** the application GUID or the GUID of the customer using the application, but the GUID of the company writing the code.</span></span> <span data-ttu-id="10b92-253">此参数用于限制请求速率。</span><span class="sxs-lookup"><span data-stu-id="10b92-253">This parameter is used for throttling the request rate.</span></span> <span data-ttu-id="10b92-254">请务必在发出的所有请求中指定此参数，以获取专用配额。</span><span class="sxs-lookup"><span data-stu-id="10b92-254">Make sure this parameter is specified in all issued requests to get a dedicated quota.</span></span> <span data-ttu-id="10b92-255">收到的所有不包含此参数的请求都会共用同一配额。</span><span class="sxs-lookup"><span data-stu-id="10b92-255">All requests received without this parameter will share the same quota.</span></span>|
||<span data-ttu-id="10b92-256">startTime endTime</span><span class="sxs-lookup"><span data-stu-id="10b92-256">startTime endTime</span></span>|<span data-ttu-id="10b92-257">依据为内容可用时间的可选日期/时间 (UTC)，指明要返回的内容的时间范围。</span><span class="sxs-lookup"><span data-stu-id="10b92-257">Optional datetimes (UTC) indicating the time range of content to return, based on when the content became available.</span></span> <span data-ttu-id="10b92-258">此时间范围包含 startTime (startTime <= contentCreated)，但不包含 endTime (contentCreated < endTime)，这样便能使用不重叠的增量时间间隔翻阅可用内容。</span><span class="sxs-lookup"><span data-stu-id="10b92-258">The time range is inclusive with respect to startTime (startTime <= contentCreated) and exclusive with respect to endTime (contentCreated < endTime), so that non-overlapping, incrementing time intervals can used to page through available content.</span></span><ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span data-ttu-id="10b92-259">YYYY-MM-DD</span><span class="sxs-lookup"><span data-stu-id="10b92-259">YYYY-MM-DD</span></span></p></li><li><p><span data-ttu-id="10b92-260">YYYY-MM-DDTHH:MM</span><span class="sxs-lookup"><span data-stu-id="10b92-260">YYYY-MM-DDTHH:MM</span></span></p></li><li><p><span data-ttu-id="10b92-261">YYYY-MM-DDTHH:MM:SS</span><span class="sxs-lookup"><span data-stu-id="10b92-261">YYYY-MM-DDTHH:MM:SS</span></span></p></li></ul><span data-ttu-id="10b92-262">必须同时指定（或同时省略）开始时间和结束时间，两者间隔不得超过 24 小时，且开始时间与当前时间的间隔不得超过 7 天。</span><span class="sxs-lookup"><span data-stu-id="10b92-262">Both must be specified (or both omitted) and they must be no more than 24 hours apart, with the start time no more than 7 days in the past.</span></span> <span data-ttu-id="10b92-263">默认情况下，如果 startTime 和 endTime 遭省略，返回的便是过去 24 小时内的可用内容。</span><span class="sxs-lookup"><span data-stu-id="10b92-263">By default, if startTime and endTime are omitted, then the content available in the last 24 hours is returned.</span></span><p><span data-ttu-id="10b92-264">**注意**：即使可以指定间隔超过 24 小时的 startTime 和 endTime，也不建议这样做。</span><span class="sxs-lookup"><span data-stu-id="10b92-264">**NOTE**: Even though it is possible to specify a startTime and endTime more than 24 hours apart, this is not recommended.</span></span> <span data-ttu-id="10b92-265">此外，如果确实返回了开始时间和结束时间间隔超过 24 小时的请求的任何响应结果，它们可能只是部分结果，不得纳入考虑范围。</span><span class="sxs-lookup"><span data-stu-id="10b92-265">Furthermore, if you do get any results in response to a request for more than 24 hours, these could be partial results and should not be taken into account.</span></span> <span data-ttu-id="10b92-266">应发出 startTime 和 endTime 间隔不超过 24 小时的请求。</span><span class="sxs-lookup"><span data-stu-id="10b92-266">The request should be issued with an interval of no more than 24 hours between the startTime and endTime.</span></span></p>|
|<span data-ttu-id="10b92-267">**响应**</span><span class="sxs-lookup"><span data-stu-id="10b92-267">**Response**</span></span>|<span data-ttu-id="10b92-268">JSON 数组</span><span class="sxs-lookup"><span data-stu-id="10b92-268">JSON array</span></span>|<span data-ttu-id="10b92-269">可用内容由包含以下属性的 JSON 对象表示：</span><span class="sxs-lookup"><span data-stu-id="10b92-269">The available content will be represented by JSON objects with the following properties:</span></span><ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span data-ttu-id="10b92-270"><b>contentType</b>：指明内容类型。</span><span class="sxs-lookup"><span data-stu-id="10b92-270"><b>contentType</b>: Indicates the content type.</span></span></p></li><li><p><span data-ttu-id="10b92-271"><b>contentId</b>：用于唯一标识内容的不透明字符串。</span><span class="sxs-lookup"><span data-stu-id="10b92-271"><b>contentId</b>: An opaque string that uniquely identifies the content.</span></span></p></li><li><p><span data-ttu-id="10b92-272"><b>contentUri</b>：供检索内容时使用的 URL。</span><span class="sxs-lookup"><span data-stu-id="10b92-272"><b>contentUri</b>: The URL to use when retrieving the content.</span></span></p></li><li><p><span data-ttu-id="10b92-273"><b>contentCreated</b>：内容的可用日期/时间。</span><span class="sxs-lookup"><span data-stu-id="10b92-273"><b>contentCreated</b>: The datetime when the content was made available.</span></span></p></li><li><p><span data-ttu-id="10b92-274"><b>contentExpiration</b>：内容不再可供检索的日期/时间。</span><span class="sxs-lookup"><span data-stu-id="10b92-274"><b>contentExpiration</b>: The datetime after which the content will no longer be available for retrieval.</span></span></p></li></ul>|


#### <a name="sample-request"></a><span data-ttu-id="10b92-275">示例请求</span><span class="sxs-lookup"><span data-stu-id="10b92-275">Sample request</span></span>

```json
GET {root}/subscriptions/content?contentType=Audit.SharePoint&PublisherIdentifier=46b472a7-c68e-4adf-8ade-3db49497518e
Authorization: Bearer eyJ0e...Qa6wg

```

#### <a name="sample-response"></a><span data-ttu-id="10b92-276">示例响应</span><span class="sxs-lookup"><span data-stu-id="10b92-276">Sample response</span></span>

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


### <a name="pagination"></a><span data-ttu-id="10b92-277">分页</span><span class="sxs-lookup"><span data-stu-id="10b92-277">Pagination</span></span>

<span data-ttu-id="10b92-278">列出某个时间范围的可用内容时，返回的结果数会受到限制，以防止响应超时。</span><span class="sxs-lookup"><span data-stu-id="10b92-278">When listing available content for a time range, the number of results returned is limited to prevent response timeouts.</span></span> <span data-ttu-id="10b92-279">如果指定时间范围的结果数超出一个响应中可返回的结果数，结果便会被截断，并向响应添加头，以指明用于检索下页结果的 URL。</span><span class="sxs-lookup"><span data-stu-id="10b92-279">If there are more results in the specified time range than can be returned in single response, the results will be truncated and a header will be added to the response indicating the URL to use to retrieve the next page of results.</span></span> <span data-ttu-id="10b92-280">此 URL 包含在初始请求中指定的相同 _startTime_ 和 _endTime_ 参数，以及用于指明下页的内部 ID 的参数。</span><span class="sxs-lookup"><span data-stu-id="10b92-280">The URL will contain the same  _startTime_ and _endTime_ parameters that were specified in the original request, together with a parameter indicating the internal ID of the next page.</span></span> <span data-ttu-id="10b92-281">如果初始请求中未指定 _startTime_ 和 _endTime_，便会设置为反映初始请求前 24 小时间隔内的结果。</span><span class="sxs-lookup"><span data-stu-id="10b92-281">If _startTime_ and _endTime_ were not specified in the original request, they will be set to reflect the 24-hour interval that preceded the original request.</span></span>


```json
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
NextPageUri: https://manage.office.com/api/v1/{tenant_id}/activity/feed/subscriptions/content?contentType=Audit.SharePoint&amp;startTime=2015-10-01&amp;endTime=2015-10-02&amp;nextPage=2015101900R022885001761

```

<span data-ttu-id="10b92-282">若要列出指定时间范围的所有可用内容，可能需要检索多页，直到收到的响应不含 **NextPageUri** 头为止。</span><span class="sxs-lookup"><span data-stu-id="10b92-282">To list all available content for a specified time range, you might need to retrieve multiple pages until a response without the **NextPageUri** header is received.</span></span>


## <a name="receiving-notifications"></a><span data-ttu-id="10b92-283">接收通知</span><span class="sxs-lookup"><span data-stu-id="10b92-283">Receiving notifications</span></span>

<span data-ttu-id="10b92-284">当有新内容可用时，我们便会向订阅的已配置 Webhook 发送通知。</span><span class="sxs-lookup"><span data-stu-id="10b92-284">Notifications are sent to the configured webhook for a subscription as new content becomes available.</span></span> <span data-ttu-id="10b92-285">由于通知包含租户标识符，因此可使用同一个 Webhook 接收已订阅的所有租户的通知。</span><span class="sxs-lookup"><span data-stu-id="10b92-285">Because the notification includes the tenant identifier, you can use the same webhook to receive notifications for all tenants for which you have subscriptions.</span></span>

<span data-ttu-id="10b92-286">通知是以 HTTP POST 形式通过 TLS（TLS 1.0 及更高版本）发送到指定 Webhook 地址。</span><span class="sxs-lookup"><span data-stu-id="10b92-286">The notification is made as an HTTP POST over TLS (TLS 1.0 and later versions) to the specified webhook address.</span></span> <span data-ttu-id="10b92-287">如果 Webhook 配置包括验证 ID，我们便会以 HTTP 头 (Webhook-AuthID) 的形式发送它。</span><span class="sxs-lookup"><span data-stu-id="10b92-287">If the webhook configuration includes an auth ID, we will send it as an HTTP header: Webhook-AuthID.</span></span> <span data-ttu-id="10b92-288">除“HTTP 200 正常”之外的其他任何响都被视为失败，我们会重试发送通知。</span><span class="sxs-lookup"><span data-stu-id="10b92-288">Any response other than HTTP 200 OK will be considered a failure and the notification will be retried.</span></span> <span data-ttu-id="10b92-289">还可以将 Webhook 配置为要求基于客户端证书的身份验证，我们将使用 manage.office.com 证书进行身份验证。</span><span class="sxs-lookup"><span data-stu-id="10b92-289">You can also configure your webhook to require client certificate-based authentication and we will authenticate using the manage.office.com certificate.</span></span>

<span data-ttu-id="10b92-290">请求正文包含一个数组，其中有一个或多个表示可用内容 blob 的 JSON 对象。</span><span class="sxs-lookup"><span data-stu-id="10b92-290">The body of the request will contain an array of one or more JSON objects that represent the available content blobs.</span></span> <span data-ttu-id="10b92-291">为了保持通知相对较小，各个通知中的内容 blob 数受到限制。</span><span class="sxs-lookup"><span data-stu-id="10b92-291">The number of content blobs in each notification is limited to keep the size of the notification relatively small.</span></span> <span data-ttu-id="10b92-292">由于此限制可能会更改，因此实现应查询数组长度，而不要以为它是固定大小。</span><span class="sxs-lookup"><span data-stu-id="10b92-292">Because this limit might change, your implementation should query for the length of the array instead of expecting a fixed size.</span></span> <span data-ttu-id="10b92-293">每个对象包含 /content 操作返回的相同属性，以及数据所属租户的 GUID 和创建订阅的应用程序的 GUID。</span><span class="sxs-lookup"><span data-stu-id="10b92-293">Each object will include the same properties returned by the /content operation, together with the GUID of the tenant to which the data belongs and the GUID of your application that created the subscriptions.</span></span> <span data-ttu-id="10b92-294">这样，Webhook 便可以在用于多个租户和应用程序时创建上下文。</span><span class="sxs-lookup"><span data-stu-id="10b92-294">This allows the webhook to establish context when it is being used with multiple tenants and applications.</span></span>

- <span data-ttu-id="10b92-295">**tenantId**：内容所属租户的 GUID。</span><span class="sxs-lookup"><span data-stu-id="10b92-295">**tenantId**: The GUID of the tenant to which the content belongs.</span></span>
    
- <span data-ttu-id="10b92-296">**clientId**：创建订阅的应用程序的 GUID。</span><span class="sxs-lookup"><span data-stu-id="10b92-296">**clientId**: The GUID of your application that created the subscription.</span></span>
    
- <span data-ttu-id="10b92-297">**contentType**：指明内容类型。</span><span class="sxs-lookup"><span data-stu-id="10b92-297">**contentType**: Indicates the content type.</span></span>
    
- <span data-ttu-id="10b92-298">**contentId**：用于唯一标识内容的不透明字符串。</span><span class="sxs-lookup"><span data-stu-id="10b92-298">**contentId**: An opaque string that uniquely identifies the content.</span></span>
    
- <span data-ttu-id="10b92-299">**contentUri**：供检索内容时使用的 URL。</span><span class="sxs-lookup"><span data-stu-id="10b92-299">**contentUri**: The URL to use when retrieving the content.</span></span>
    
- <span data-ttu-id="10b92-300">**contentCreated**：内容的可用日期/时间。</span><span class="sxs-lookup"><span data-stu-id="10b92-300">**contentCreated**: The datetime when the content was made available.</span></span>
    
- <span data-ttu-id="10b92-301">**contentExpiration**：内容不再可供检索的日期/时间。</span><span class="sxs-lookup"><span data-stu-id="10b92-301">**contentExpiration**: The datetime after which the content will no longer be available for retrieval.</span></span>
    
<span data-ttu-id="10b92-302">下面是一个通知示例。</span><span class="sxs-lookup"><span data-stu-id="10b92-302">The following is an example of a notification.</span></span>

```json
POST https://webhook.myapp.com/o365/ 
Content-Type: application/json; utf-8
Webhook-AuthID: o365activityapinotification

[
    {
        "tenantId": "{GUID}"
        "clientId": "{GUID}"
        "contentType": "Audit.SharePoint",
        "contentId": "492638008028$492638008028$f28ab78ad40140608012736e373933ebspo2015043022$4a81a7c326fc4aed89c62e6039ab833b$04",
        "contentUri": "https://manage.office.com/api/v1.0/f28ab78a-d401-4060-8012-736e373933eb/activity/feed/audit/492638008028$492638008028$f28ab78ad40140608012736e373933ebspo2015043022$4a81a7c326fc4aed89c62e6039ab833b$04",
        "contentCreated": "2015-05-23T17:35:00.000Z",
        "contentExpiration": "2015-05-30T17:35:00.000Z"
    },
    ...
]

```


## <a name="notification-failure-and-retry"></a><span data-ttu-id="10b92-303">通知失败和重试</span><span class="sxs-lookup"><span data-stu-id="10b92-303">Notification failure and retry</span></span>

<span data-ttu-id="10b92-304">通知系统在有新内容可用时发送通知。</span><span class="sxs-lookup"><span data-stu-id="10b92-304">The notification system sends notifications as new content becomes available.</span></span> <span data-ttu-id="10b92-305">如果通知发送失败次数过多，重试机制便会以指数方式增加两次重试间隔。</span><span class="sxs-lookup"><span data-stu-id="10b92-305">If we encounter excessive failures when sending notifications, our retry mechanism will exponentially increase the time between retries.</span></span> <span data-ttu-id="10b92-306">如果一再失败，我们有权禁用 Webhook 并彻底停止向它发送通知。</span><span class="sxs-lookup"><span data-stu-id="10b92-306">If we continue to encounter failures, we reserve the right to disable the webhook and stop sending notifications to it altogether.</span></span> <span data-ttu-id="10b92-307">可使用 /start 操作重新启用已禁用的 Webhook。</span><span class="sxs-lookup"><span data-stu-id="10b92-307">The /start operation can be used to re-enable a disabled webhook.</span></span>


## <a name="retrieving-content"></a><span data-ttu-id="10b92-308">检索内容</span><span class="sxs-lookup"><span data-stu-id="10b92-308">Retrieving content</span></span>

<span data-ttu-id="10b92-309">若要检索内容 blob，请对可用内容列表和已发送到 Webhook 的通知中包含的相应内容 URI 发出 GET 请求。</span><span class="sxs-lookup"><span data-stu-id="10b92-309">To retrieve a content blob, make a GET request against the corresponding content URI that is included in the list of available content and in the notifications sent to a webhook.</span></span> <span data-ttu-id="10b92-310">返回的内容为一个集合，其中包含一个或多个 JSON 格式的操作和事件。</span><span class="sxs-lookup"><span data-stu-id="10b92-310">The returned content will be a collection of one more actions or events in JSON format.</span></span>

#### <a name="sample-request"></a><span data-ttu-id="10b92-311">示例请求</span><span class="sxs-lookup"><span data-stu-id="10b92-311">Sample request</span></span>

```json
GET https://manage.office.com/api/v1.0/41463f53-8812-40f4-890f-865bf6e35190/activity/feed/audit/301299007231$301299007231$41463f53881240f4890f865bf6e35190aad2015062920$e1c2ab19858a469fb1f1fd097effffc9$04 HTTP/1.1
Authorization: Bearer eyJ0e...Qa6wg
```

#### <a name="sample-response"></a><span data-ttu-id="10b92-312">示例响应</span><span class="sxs-lookup"><span data-stu-id="10b92-312">Sample response</span></span>

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


## <a name="list-notifications"></a><span data-ttu-id="10b92-313">列出通知</span><span class="sxs-lookup"><span data-stu-id="10b92-313">List notifications</span></span>

<span data-ttu-id="10b92-314">此操作会列出尝试对指定内容类型发送的所有通知。</span><span class="sxs-lookup"><span data-stu-id="10b92-314">This operation lists all notification attempts for the specified content type.</span></span> <span data-ttu-id="10b92-315">如果在启动对内容类型的订阅时未添加 Webhook，便无通知可供检索。</span><span class="sxs-lookup"><span data-stu-id="10b92-315">If you did not include a webhook when starting the subscription to the content type, there will be no notifications to retrieve.</span></span> <span data-ttu-id="10b92-316">因为我们会在失败时重试发送通知，所以此操作可能会返回有关同一内容的多个通知，并且通知发送顺序不一定与内容可用时间顺序一致（尤其是在失败后重试发送通知时）。</span><span class="sxs-lookup"><span data-stu-id="10b92-316">Because we retry notifications in the event of failure, this operation can return multiple notifications for the same content, and the order in which the notifications are sent will not necessarily match the order in which the content became available (especially when there are failures and retries).</span></span> 

<span data-ttu-id="10b92-317">此操作可有助于调查与 Webhook 和通知相关的问题，但不得用它来确定当前什么内容可供检索。</span><span class="sxs-lookup"><span data-stu-id="10b92-317">You can use this operation to help investigate issues related to webhooks and notifications, but you should not use it to determine what content is currently available for retrieval.</span></span> <span data-ttu-id="10b92-318">若要确定，请改用 /content 操作。</span><span class="sxs-lookup"><span data-stu-id="10b92-318">Use the /content operation instead.</span></span> <span data-ttu-id="10b92-319">如果订阅处于已禁用状态，返回的便是错误。</span><span class="sxs-lookup"><span data-stu-id="10b92-319">We return an error if the subscription status is disabled.</span></span>


||<span data-ttu-id="10b92-320">订阅</span><span class="sxs-lookup"><span data-stu-id="10b92-320">Subscription</span></span>|<span data-ttu-id="10b92-321">说明</span><span class="sxs-lookup"><span data-stu-id="10b92-321">Description</span></span>|
|:-----|:-----|:-----|
|<span data-ttu-id="10b92-322">**路径**</span><span class="sxs-lookup"><span data-stu-id="10b92-322">**Path**</span></span>| `/subscriptions/notifications?contentType={ContentType}&amp;startTime={0}&amp;endTime={1}`||
|<span data-ttu-id="10b92-323">**参数**</span><span class="sxs-lookup"><span data-stu-id="10b92-323">**Parameters**</span></span>|<span data-ttu-id="10b92-324">contentType</span><span class="sxs-lookup"><span data-stu-id="10b92-324">contentType</span></span>|<span data-ttu-id="10b92-325">必须为有效内容类型。</span><span class="sxs-lookup"><span data-stu-id="10b92-325">Must be a valid content type.</span></span>|
||<span data-ttu-id="10b92-326">PublisherIdentifier</span><span class="sxs-lookup"><span data-stu-id="10b92-326">PublisherIdentifier</span></span>|<span data-ttu-id="10b92-327">对 API 编码的供应商的租户 GUID。</span><span class="sxs-lookup"><span data-stu-id="10b92-327">The tenant GUID of the vendor coding against the API.</span></span> <span data-ttu-id="10b92-328">这**不是**应用程序 GUID 或使用应用程序的客户的 GUID，而是编写代码的公司的 GUID。</span><span class="sxs-lookup"><span data-stu-id="10b92-328">This is **not** the application GUID or the GUID of the customer using the application, but the GUID of the company writing the code.</span></span> <span data-ttu-id="10b92-329">此参数用于限制请求速率。</span><span class="sxs-lookup"><span data-stu-id="10b92-329">This parameter is used for throttling the request rate.</span></span> <span data-ttu-id="10b92-330">请务必在发出的所有请求中指定此参数，以获取专用配额。</span><span class="sxs-lookup"><span data-stu-id="10b92-330">Make sure this parameter is specified in all issued requests to get a dedicated quota.</span></span> <span data-ttu-id="10b92-331">收到的所有不包含此参数的请求都会共用同一配额。</span><span class="sxs-lookup"><span data-stu-id="10b92-331">All requests received without this parameter will share the same quota.</span></span>|
||<span data-ttu-id="10b92-332">startTime endTime</span><span class="sxs-lookup"><span data-stu-id="10b92-332">startTime endTime</span></span>|<span data-ttu-id="10b92-333">依据为内容可用时间的可选日期/时间 (UTC)，指明要返回的内容的时间范围。</span><span class="sxs-lookup"><span data-stu-id="10b92-333">Optional datetimes (UTC) that indicate the time range of content to return, based on when the content became available.</span></span> <span data-ttu-id="10b92-334">此时间范围包含 _startTime_ ( _startTime_ <= contentCreated)，但不包含 _endTime_ (_contentCreated_ < endTime)，这样便能使用不重叠的增量时间间隔翻阅可用内容。</span><span class="sxs-lookup"><span data-stu-id="10b92-334">The time range is inclusive with respect to  _startTime_ ( _startTime_ <= contentCreated) and exclusive with respect to _endTime_ (_contentCreated_ < endTime), so that non-overlapping, incrementing time intervals can used to page through available content.</span></span><ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span data-ttu-id="10b92-335">YYYY-MM-DD</span><span class="sxs-lookup"><span data-stu-id="10b92-335">YYYY-MM-DD</span></span></p></li><li><p><span data-ttu-id="10b92-336">YYYY-MM-DDTHH:MM</span><span class="sxs-lookup"><span data-stu-id="10b92-336">YYYY-MM-DDTHH:MM</span></span></p></li><li><p><span data-ttu-id="10b92-337">YYYY-MM-DDTHH:MM:SS</span><span class="sxs-lookup"><span data-stu-id="10b92-337">YYYY-MM-DDTHH:MM:SS</span></span></p></li></ul><span data-ttu-id="10b92-338">必须同时指定（或同时省略）开始时间和结束时间，两者间隔不得超过 24 小时，且开始时间与当前时间的间隔不得超过 7 天。</span><span class="sxs-lookup"><span data-stu-id="10b92-338">Both must be specified (or both omitted) and they must be no more than 24 hours apart, with the start time no more than 7 days in the past.</span></span> <span data-ttu-id="10b92-339">默认情况下，如果 _startTime_ 和 _endTime_ 遭省略，返回的便是过去 24 小时内的可用内容。</span><span class="sxs-lookup"><span data-stu-id="10b92-339">By default, if  _startTime_ and _endTime_ are omitted, the content available in the last 24 hours is returned.</span></span>|
|<span data-ttu-id="10b92-340">**响应**</span><span class="sxs-lookup"><span data-stu-id="10b92-340">**Response**</span></span>|<span data-ttu-id="10b92-341">JSON 数组</span><span class="sxs-lookup"><span data-stu-id="10b92-341">JSON array</span></span>|<span data-ttu-id="10b92-342">通知由包含以下属性的 JSON 对象表示：</span><span class="sxs-lookup"><span data-stu-id="10b92-342">The notifications will be represented by JSON objects with the following properties:</span></span> <ul><li><span data-ttu-id="10b92-343">**contentType**：指明内容类型。</span><span class="sxs-lookup"><span data-stu-id="10b92-343">**contentType**: indicates the content type.</span></span></li><li><span data-ttu-id="10b92-344">**contentId**：用于唯一标识内容的不透明字符串。</span><span class="sxs-lookup"><span data-stu-id="10b92-344">**contentId**: an opaque string that uniquely identifies the content.</span></span></li><li><span data-ttu-id="10b92-345">**contentUri**：供检索内容时使用的 URL。</span><span class="sxs-lookup"><span data-stu-id="10b92-345">**contentUri**: the URL to use when retrieving the content.</span></span> </li><li><span data-ttu-id="10b92-346">**contentCreated**：内容的可用日期/时间。</span><span class="sxs-lookup"><span data-stu-id="10b92-346">**contentCreated**: the datetime when the content was made available.</span></span></li><li><span data-ttu-id="10b92-347">**contentExpiration**：内容不再可供检索的日期/时间。</span><span class="sxs-lookup"><span data-stu-id="10b92-347">**contentExpiration**: the datetime after which the content will no longer be available for retrieval.</span></span></li><li><span data-ttu-id="10b92-348">**notificationSent**：通知发送日期/时间。</span><span class="sxs-lookup"><span data-stu-id="10b92-348">**notificationSent**: the datetime when the notification was sent.</span></span></li><li><span data-ttu-id="10b92-349">**notificationStatus**：指明通知尝试发送是成功还是失败。</span><span class="sxs-lookup"><span data-stu-id="10b92-349">**notificationStatus**: indicates the success or failure of the notification attempt.</span></span></li></ul>|

#### <a name="sample-request"></a><span data-ttu-id="10b92-350">示例请求</span><span class="sxs-lookup"><span data-stu-id="10b92-350">Sample request</span></span>

```json
GET {root}/subscriptions/notifications?contentType=Audit.SharePoint&PublisherIdentifier=46b472a7-c68e-4adf-8ade-3db49497518e
Authorization: Bearer eyJ0e...Qa6wg

```

#### <a name="sample-response"></a><span data-ttu-id="10b92-351">示例响应</span><span class="sxs-lookup"><span data-stu-id="10b92-351">Sample response</span></span>

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


### <a name="pagination"></a><span data-ttu-id="10b92-352">分页</span><span class="sxs-lookup"><span data-stu-id="10b92-352">Pagination</span></span>

<span data-ttu-id="10b92-353">列出某个时间范围的通知历史记录时，返回的结果数会受到限制，以防止响应超时。</span><span class="sxs-lookup"><span data-stu-id="10b92-353">When listing notification history for a time range, the number of results returned is limited to prevent response timeouts.</span></span> <span data-ttu-id="10b92-354">如果指定时间范围的结果数超出一个响应中可返回的结果数，结果便会被截断，并向响应添加头，以指明用于检索下页结果的 URL。</span><span class="sxs-lookup"><span data-stu-id="10b92-354">If there are more results in the specified time range than can be returned in a single response, the results are truncated and a header is added to the response indicating the URL to use to retrieve the next page of results.</span></span> <span data-ttu-id="10b92-355">此 URL 包含在初始请求中指定的相同 _startTime_ 和 _endTime_ 参数，以及用于指明下页的内部 ID 的参数。</span><span class="sxs-lookup"><span data-stu-id="10b92-355">The URL will contain the same  _startTime_ and _endTime_ parameters that were specified in the original request, together with a parameter indicating the internal ID of the next page.</span></span> <span data-ttu-id="10b92-356">如果初始请求中未指定 _startTime_ 和 _endTime_，便会设置为反映初始请求前 24 小时间隔内的结果。</span><span class="sxs-lookup"><span data-stu-id="10b92-356">If _startTime_ and _endTime_ were not specified in the original request, they will be set to reflect the 24-hour interval that preceded the original request.</span></span>

```json
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
NextPageUrl: https://manage.office.com/api/v1/{tenant_id}/activity/feed/subscriptions/content?contentType=Audit.SharePoint&amp;startTime=2015-10-01&amp;endTime=2015-10-02&amp;nextPage=2015101900R022885001761

```

<span data-ttu-id="10b92-357">若要列出指定时间范围的所有可用内容，可能需要检索多页，直到收到的响应不含 **NextPageUrl** 头为止。</span><span class="sxs-lookup"><span data-stu-id="10b92-357">To list all available content for a specified time range, you might need to retrieve multiple pages until a response without the **NextPageUrl** header is received.</span></span>

## <a name="retrieve-resource-friendly-names"></a><span data-ttu-id="10b92-358">检索资源易记名称</span><span class="sxs-lookup"><span data-stu-id="10b92-358">Retrieve resource friendly names</span></span>

<span data-ttu-id="10b92-359">此操作会检索数据源中由 GUID 标识的对象的易记名称。</span><span class="sxs-lookup"><span data-stu-id="10b92-359">This operation retrieves friendly names for objects in the data feed identified by guids.</span></span> <span data-ttu-id="10b92-360">目前，仅支持“DlpSensitiveType”对象。</span><span class="sxs-lookup"><span data-stu-id="10b92-360">Currently "DlpSensitiveType" is the only supported object.</span></span> 


||<span data-ttu-id="10b92-361">订阅</span><span class="sxs-lookup"><span data-stu-id="10b92-361">Subscription</span></span>|<span data-ttu-id="10b92-362">说明</span><span class="sxs-lookup"><span data-stu-id="10b92-362">Description</span></span>|
|:-----|:-----|:-----|
|<span data-ttu-id="10b92-363">**路径**</span><span class="sxs-lookup"><span data-stu-id="10b92-363">**Path**</span></span>| `/resources/dlpSensitiveTypes`||
|<span data-ttu-id="10b92-364">**参数**</span><span class="sxs-lookup"><span data-stu-id="10b92-364">**Parameters**</span></span>|<span data-ttu-id="10b92-365">PublisherIdentifier</span><span class="sxs-lookup"><span data-stu-id="10b92-365">PublisherIdentifier</span></span>|<span data-ttu-id="10b92-366">对 API 编码的供应商的租户 GUID。</span><span class="sxs-lookup"><span data-stu-id="10b92-366">The tenant GUID of the vendor coding against the API.</span></span> <span data-ttu-id="10b92-367">这**不是**应用程序 GUID 或使用应用程序的客户的 GUID，而是编写代码的公司的 GUID。</span><span class="sxs-lookup"><span data-stu-id="10b92-367">This is **not** the application GUID or the GUID of the customer using the application, but the GUID of the company writing the code.</span></span> <span data-ttu-id="10b92-368">此参数用于限制请求速率。</span><span class="sxs-lookup"><span data-stu-id="10b92-368">This parameter is used for throttling the request rate.</span></span> <span data-ttu-id="10b92-369">请务必在发出的所有请求中指定此参数，以获取专用配额。</span><span class="sxs-lookup"><span data-stu-id="10b92-369">Make sure this parameter is specified in all issued requests to get a dedicated quota.</span></span> <span data-ttu-id="10b92-370">收到的所有不包含此参数的请求都会共用同一配额。</span><span class="sxs-lookup"><span data-stu-id="10b92-370">All requests received without this parameter will share the same quota.</span></span>|
|<span data-ttu-id="10b92-371">**头**</span><span class="sxs-lookup"><span data-stu-id="10b92-371">**Headers**</span></span>|<span data-ttu-id="10b92-372">Accept-Language</span><span class="sxs-lookup"><span data-stu-id="10b92-372">Accept-Language</span></span>|<span data-ttu-id="10b92-373">用于指定本地化名称的目标语言的头。</span><span class="sxs-lookup"><span data-stu-id="10b92-373">Header to specify the desired language for localized names.</span></span> <span data-ttu-id="10b92-374">例如，使用“en-US”表示英语，使用“es”表示西班牙语。</span><span class="sxs-lookup"><span data-stu-id="10b92-374">For example, use "en-US" for English or "es" for Spanish.</span></span> <span data-ttu-id="10b92-375">如果没有此头，返回的便是默认语言 (en-US)。</span><span class="sxs-lookup"><span data-stu-id="10b92-375">The default language (en-US) will be returned if this header is not present.</span></span>|
|<span data-ttu-id="10b92-376">**正文**</span><span class="sxs-lookup"><span data-stu-id="10b92-376">**Body**</span></span>|<span data-ttu-id="10b92-377">（空）</span><span class="sxs-lookup"><span data-stu-id="10b92-377">(empty)</span></span>||
|<span data-ttu-id="10b92-378">**响应**</span><span class="sxs-lookup"><span data-stu-id="10b92-378">**Response**</span></span>|<span data-ttu-id="10b92-379">JSON 数组</span><span class="sxs-lookup"><span data-stu-id="10b92-379">JSON array</span></span>|<span data-ttu-id="10b92-380">可用内容由包含以下属性的 JSON 对象表示：</span><span class="sxs-lookup"><span data-stu-id="10b92-380">The available content will be represented by JSON objects with the following properties:</span></span><ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span data-ttu-id="10b92-381"><b>id</b>：指明敏感信息类型的 GUID。</span><span class="sxs-lookup"><span data-stu-id="10b92-381"><b>id</b>: Indicates the guid of the sensitive information type.</span></span></p></li><li><p><span data-ttu-id="10b92-382"><b>name</b>：指明敏感信息类型的易记名称。</span><span class="sxs-lookup"><span data-stu-id="10b92-382"><b>name</b>: The friendly name of the sensitive information type.</span></span></p></li></ul>|

#### <a name="sample-request"></a><span data-ttu-id="10b92-383">示例请求</span><span class="sxs-lookup"><span data-stu-id="10b92-383">Sample request</span></span>

```json
GET {root}/resources/dlpSensitiveTypes?PublisherIdentifier=46b472a7-c68e-4adf-8ade-3db49497518e
Authorization: Bearer eyJ0e...Qa6wg
Accept-Language: {language code} 

```

#### <a name="sample-response"></a><span data-ttu-id="10b92-384">示例响应</span><span class="sxs-lookup"><span data-stu-id="10b92-384">Sample response</span></span>

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

## <a name="api-throttling"></a><span data-ttu-id="10b92-385">API 限制</span><span class="sxs-lookup"><span data-stu-id="10b92-385">API throttling</span></span>

<span data-ttu-id="10b92-386">每个对 API 编码的供应商都有专用的请求限制配额，即每分钟 6 万个。</span><span class="sxs-lookup"><span data-stu-id="10b92-386">Each vendor coding against the API has a dedicated quota for request throttling at 60K per minute.</span></span> <span data-ttu-id="10b92-387">若要获取专用配额，请在所有请求中指定 PublisherIdentifier 参数。</span><span class="sxs-lookup"><span data-stu-id="10b92-387">To get the dedicated quota, specify the parameter PublisherIdentifier in all your requests.</span></span> <span data-ttu-id="10b92-388">包含相同 PublisherIdentifier 的请求会共用同一配额。</span><span class="sxs-lookup"><span data-stu-id="10b92-388">Requests with the same PublisherIdentifier will share the same quota.</span></span> <span data-ttu-id="10b92-389">所有未指定 PublisherIdentifier 的请求与 GUID 00000000-0000-0000-0000-000000000000 共用同一配额。</span><span class="sxs-lookup"><span data-stu-id="10b92-389">All requests without the PublisherIdentifier specified will share the same quota as GUID 00000000-0000-0000-0000-000000000000.</span></span>

<span data-ttu-id="10b92-390">如果 Office 365 需要对某些问题向你反向上报，请确保 GUID 已用作 PublisherIdentifier 的租户的订阅是最新的，且已使用正确的联系信息进行更新。</span><span class="sxs-lookup"><span data-stu-id="10b92-390">If Office 365 needs to reverse escalate to you in certain issues, make sure the subscription for the tenant whose GUID is used as your PublisherIdentifier is current and updated with the correct contact information.</span></span> <span data-ttu-id="10b92-391">对于此租户，没有任何订阅要求。</span><span class="sxs-lookup"><span data-stu-id="10b92-391">There is no subscription requirement for this tenant.</span></span>

<span data-ttu-id="10b92-392">对于使用此 API 开发自己的解决方案的客户，建议使用自己的租户 GUID，以免因有限共享配额导致竞争。</span><span class="sxs-lookup"><span data-stu-id="10b92-392">For customers who are developing their own solutions using this API, we recommend using your own tenant GUID to avoid competition caused by a limited shared quota.</span></span>

> [!NOTE] 
> <span data-ttu-id="10b92-393">尽管每个发布者每分钟最多可提交 6 万个请求，Microsoft 也无法保证响应速率。</span><span class="sxs-lookup"><span data-stu-id="10b92-393">Even though each publisher can submit up to 60K requests per minute, Microsoft cannot guarantee a response rate.</span></span> <span data-ttu-id="10b92-394">响应速率取决于各种因素，如客户端系统性能、网络容量和网络速度。</span><span class="sxs-lookup"><span data-stu-id="10b92-394">The response rate depends on various factors, such as client system performance, network capacity, and network speed.</span></span>  <span data-ttu-id="10b92-395">发布者每分钟最多可提交 6 万个请求，但无法在这同一分钟内收到所有 6 万个请求的响应。</span><span class="sxs-lookup"><span data-stu-id="10b92-395">A publisher can submit up to 60K requests per minute, but should not expect to receive responses for all 60K requests within that same minute.</span></span> <span data-ttu-id="10b92-396">如果发布者要对客户端应用程序进行基准校验，他们应在计划运行客户端应用程序的各个不同环境中这样做，因为结果因环境而异。</span><span class="sxs-lookup"><span data-stu-id="10b92-396">If anything, should a publisher want to benchmark a client application, they should do so in each different environment that they are planning on running the client application in because results will vary from environment to environment.</span></span>

## <a name="errors"></a><span data-ttu-id="10b92-397">错误</span><span class="sxs-lookup"><span data-stu-id="10b92-397">Errors</span></span>

<span data-ttu-id="10b92-398">如果遇到错误，服务会使用标准 HTTP 错误代码语法，向调用方报告错误响应代码。</span><span class="sxs-lookup"><span data-stu-id="10b92-398">When the service encounters an error, it will report the error response code to the caller, using standard HTTP error-code syntax.</span></span> <span data-ttu-id="10b92-399">.</span><span class="sxs-lookup"><span data-stu-id="10b92-399">.</span></span> <span data-ttu-id="10b92-400">失败调用的正文中包含其他信息（作为一个 JSON 对象）。</span><span class="sxs-lookup"><span data-stu-id="10b92-400">Additional information is included in the body of the failed call as a single JSON object.</span></span> <span data-ttu-id="10b92-401">下面的示例展示了完整的 JSON 错误正文：</span><span class="sxs-lookup"><span data-stu-id="10b92-401">An example of a full JSON error body is shown below:</span></span> 

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
|<span data-ttu-id="10b92-402">代码</span><span class="sxs-lookup"><span data-stu-id="10b92-402">Code</span></span>|<span data-ttu-id="10b92-403">消息</span><span class="sxs-lookup"><span data-stu-id="10b92-403">Message</span></span>|
|<span data-ttu-id="10b92-404">AF10001</span><span class="sxs-lookup"><span data-stu-id="10b92-404">AF10001</span></span>|<span data-ttu-id="10b92-405">在请求中发送的权限集 ({0}) 不含所需的权限 **ActivityFeed.Read**。</span><span class="sxs-lookup"><span data-stu-id="10b92-405">The permission set ({0}) sent in the request did not include the expected permission **ActivityFeed.Read**.</span></span><ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span data-ttu-id="10b92-406">{0} = 访问令牌中的权限集。</span><span class="sxs-lookup"><span data-stu-id="10b92-406">{0} = the permission set in the access token.</span></span></p></li></ul>|
|<span data-ttu-id="10b92-407">AF20001</span><span class="sxs-lookup"><span data-stu-id="10b92-407">AF20001</span></span>|<span data-ttu-id="10b92-408">缺少参数 {0}。</span><span class="sxs-lookup"><span data-stu-id="10b92-408">Missing parameter: {0}.</span></span> <ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span data-ttu-id="10b92-409">{0} = 缺少的参数的名称。</span><span class="sxs-lookup"><span data-stu-id="10b92-409">{0} = the name of the missing parameter.</span></span></p></li></ul>|
|<span data-ttu-id="10b92-410">AF20002</span><span class="sxs-lookup"><span data-stu-id="10b92-410">AF20002</span></span>|<span data-ttu-id="10b92-411">参数类型 {0} 无效。</span><span class="sxs-lookup"><span data-stu-id="10b92-411">Invalid parameter type: {0}.</span></span> <span data-ttu-id="10b92-412">类型应为 {1}</span><span class="sxs-lookup"><span data-stu-id="10b92-412">Expected type: {1}</span></span><ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span data-ttu-id="10b92-413">{0} = 无效参数的名称。</span><span class="sxs-lookup"><span data-stu-id="10b92-413">{0} = the name of the invalid parameter.</span></span></p></li><li><p><span data-ttu-id="10b92-414">{1} = 应采用的类型（int、datetime、guid）</span><span class="sxs-lookup"><span data-stu-id="10b92-414">{1} = the expected type (int, datetime, guid).</span></span></p></li></ul>|
|<span data-ttu-id="10b92-415">AF20003</span><span class="sxs-lookup"><span data-stu-id="10b92-415">AF20003</span></span>|<span data-ttu-id="10b92-416">提供的到期时间 {0} 设置为过去的日期和时间。</span><span class="sxs-lookup"><span data-stu-id="10b92-416">Expiration {0} provided is set to past date and time.</span></span><ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span data-ttu-id="10b92-417">{0} = 在 API 调用中传递的到期时间。</span><span class="sxs-lookup"><span data-stu-id="10b92-417">{0} = the expiration passed in the API call.</span></span></p></li></ul>|
|<span data-ttu-id="10b92-418">AF20010</span><span class="sxs-lookup"><span data-stu-id="10b92-418">AF20010</span></span>|<span data-ttu-id="10b92-419">在 URL 中传递的租户 ID ({0}) 与访问令牌中传递的租户 ID ({1}) 不一致。</span><span class="sxs-lookup"><span data-stu-id="10b92-419">The tenant ID passed in the URL ({0}) does not match the tenant ID passed in the access token ({1}).</span></span><ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span data-ttu-id="10b92-420">{0} = 在 URL 中传递的租户 ID</span><span class="sxs-lookup"><span data-stu-id="10b92-420">{0} = tenant ID passed in the URL</span></span></p></li><li><p><span data-ttu-id="10b92-421">{1} = 在访问令牌中传递的租户 ID</span><span class="sxs-lookup"><span data-stu-id="10b92-421">{1} = tenant ID passed in the access token</span></span></p></li></ul>|
|<span data-ttu-id="10b92-422">AF20011</span><span class="sxs-lookup"><span data-stu-id="10b92-422">AF20011</span></span>|<span data-ttu-id="10b92-423">指定的租户 ID ({0}) 在系统中不存在或已遭删除。</span><span class="sxs-lookup"><span data-stu-id="10b92-423">Specified tenant ID ({0}) does not exist in the system or has been deleted.</span></span> <ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>   <span data-ttu-id="10b92-424">{0} = 在 URL 中传递的租户 ID</span><span class="sxs-lookup"><span data-stu-id="10b92-424">{0} = tenant ID passed in the URL</span></span></p></li></ul>|
|<span data-ttu-id="10b92-425">AF20012</span><span class="sxs-lookup"><span data-stu-id="10b92-425">AF20012</span></span>|<span data-ttu-id="10b92-426">指定的租户 ID ({0}) 未在系统中正确配置。</span><span class="sxs-lookup"><span data-stu-id="10b92-426">Specified tenant ID ({0}) is incorrectly configured in the system.</span></span> <ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>    <span data-ttu-id="10b92-427">{0} = 在 URL 中传递的租户 ID</span><span class="sxs-lookup"><span data-stu-id="10b92-427">{0} = tenant ID passed in the URL</span></span></p></li></ul>|
|<span data-ttu-id="10b92-428">AF20013</span><span class="sxs-lookup"><span data-stu-id="10b92-428">AF20013</span></span>|<span data-ttu-id="10b92-429">在 URL 中传递的租户 ID ({0}) 不是有效的 GUID。</span><span class="sxs-lookup"><span data-stu-id="10b92-429">The tenant ID passed in the URL ({0}) is not a valid GUID.</span></span><ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p> <span data-ttu-id="10b92-430">{0} = 在 URL 中传递的租户 ID</span><span class="sxs-lookup"><span data-stu-id="10b92-430">{0} = tenant ID passed in the URL</span></span></p></li></ul>|
|<span data-ttu-id="10b92-431">AF20020</span><span class="sxs-lookup"><span data-stu-id="10b92-431">AF20020</span></span>|<span data-ttu-id="10b92-432">指定的内容类型无效。</span><span class="sxs-lookup"><span data-stu-id="10b92-432">The specified content type is not valid.</span></span>|
|<span data-ttu-id="10b92-433">AF20021</span><span class="sxs-lookup"><span data-stu-id="10b92-433">AF20021</span></span>|<span data-ttu-id="10b92-434">无法验证 Webhook 终结点{{0})。</span><span class="sxs-lookup"><span data-stu-id="10b92-434">The webhook endpoint {{0}) could not be validated.</span></span> <span data-ttu-id="10b92-435">{1}</span><span class="sxs-lookup"><span data-stu-id="10b92-435">{1}</span></span><ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span data-ttu-id="10b92-436">{0} = Webhook 地址。</span><span class="sxs-lookup"><span data-stu-id="10b92-436">{0} = webhook address.</span></span></p></li><li><p><span data-ttu-id="10b92-437">{1} =“终结点未返回 HTTP 200”</span><span class="sxs-lookup"><span data-stu-id="10b92-437">{1} = "The endpoint did not return HTTP 200."</span></span> <span data-ttu-id="10b92-438">或“地址必须以 HTTPS 开头”。</span><span class="sxs-lookup"><span data-stu-id="10b92-438">or "The address must begin with HTTPS."</span></span></p></li></ul>|
|<span data-ttu-id="10b92-439">AF20022</span><span class="sxs-lookup"><span data-stu-id="10b92-439">AF20022</span></span>|<span data-ttu-id="10b92-440">找不到对指定内容类型的订阅。</span><span class="sxs-lookup"><span data-stu-id="10b92-440">No subscription found for the specified content type.</span></span>|
|<span data-ttu-id="10b92-441">AF20023</span><span class="sxs-lookup"><span data-stu-id="10b92-441">AF20023</span></span>|<span data-ttu-id="10b92-442">{0} 已禁用订阅。</span><span class="sxs-lookup"><span data-stu-id="10b92-442">The subscription was disabled by {0}.</span></span><ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span data-ttu-id="10b92-443">{0} =“租户管理员”或“服务管理员”</span><span class="sxs-lookup"><span data-stu-id="10b92-443">{0} = "a tenant admin" or "a service admin"</span></span></p></li></ul>|
|<span data-ttu-id="10b92-444">AF20030</span><span class="sxs-lookup"><span data-stu-id="10b92-444">AF20030</span></span>|<span data-ttu-id="10b92-445">必须同时指定（或同时省略）开始时间和结束时间，两者间隔不得超过 24 小时，且开始时间与当前时间的间隔不得超过 7 天。</span><span class="sxs-lookup"><span data-stu-id="10b92-445">Start time and end time must both be specified (or both omitted) and must be less than or equal to 24 hours apart, with the start time no more than 7 days in the past.</span></span>|
|<span data-ttu-id="10b92-446">AF20031</span><span class="sxs-lookup"><span data-stu-id="10b92-446">AF20031</span></span>|<span data-ttu-id="10b92-447">nextPage 输入 {0} 无效</span><span class="sxs-lookup"><span data-stu-id="10b92-447">Invalid nextPage Input: {0}.</span></span><ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span data-ttu-id="10b92-448">{0} = 在 URL 中传递的下页指示符</span><span class="sxs-lookup"><span data-stu-id="10b92-448">{0} = the next page indicator passed in the URL</span></span></p></li></ul>|
|<span data-ttu-id="10b92-449">AF20050</span><span class="sxs-lookup"><span data-stu-id="10b92-449">AF20050</span></span>|<span data-ttu-id="10b92-450">指定的内容 ({0}) 不存在。</span><span class="sxs-lookup"><span data-stu-id="10b92-450">The specified content ({0}) does not exist.</span></span><ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span data-ttu-id="10b92-451">{0} = 资源 ID 或资源 URL</span><span class="sxs-lookup"><span data-stu-id="10b92-451">{0} = resource id or resource URL</span></span></p></li></ul>|
|<span data-ttu-id="10b92-452">AF20051</span><span class="sxs-lookup"><span data-stu-id="10b92-452">AF20051</span></span>|<span data-ttu-id="10b92-453">使用密钥 {0} 请求获取的内容已到期。</span><span class="sxs-lookup"><span data-stu-id="10b92-453">Content requested with the key {0} has already expired.</span></span> <span data-ttu-id="10b92-454">无法检索 7 天前的内容。</span><span class="sxs-lookup"><span data-stu-id="10b92-454">Content older than 7 days cannot be retrieved.</span></span><ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span data-ttu-id="10b92-455">{0} = 资源 ID 或资源 URL</span><span class="sxs-lookup"><span data-stu-id="10b92-455">•    {0} = resource id or resource URL</span></span></p></li></ul>|
|<span data-ttu-id="10b92-456">AF20052</span><span class="sxs-lookup"><span data-stu-id="10b92-456">AF20052</span></span>|<span data-ttu-id="10b92-457">URL 中的内容 ID {0} 无效。</span><span class="sxs-lookup"><span data-stu-id="10b92-457">Content ID {0} in the URL is invalid.</span></span><ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span data-ttu-id="10b92-458">{0} = 资源 ID 或资源 URL</span><span class="sxs-lookup"><span data-stu-id="10b92-458">{0} = resource id or resource URL</span></span></p></li></ul>|
|<span data-ttu-id="10b92-459">AF20053</span><span class="sxs-lookup"><span data-stu-id="10b92-459">AF20053</span></span>|<span data-ttu-id="10b92-460">Accept-Language 头中只能显示一种语言。</span><span class="sxs-lookup"><span data-stu-id="10b92-460">Only one language may be present in the Accept-Language header.</span></span>|
|<span data-ttu-id="10b92-461">AF20054</span><span class="sxs-lookup"><span data-stu-id="10b92-461">AF20054</span></span>|<span data-ttu-id="10b92-462">Accept-Language 头中的语法无效。</span><span class="sxs-lookup"><span data-stu-id="10b92-462">Invalid syntax in Accept-Language header.</span></span>|
|<span data-ttu-id="10b92-463">AF429</span><span class="sxs-lookup"><span data-stu-id="10b92-463">AF429</span></span>|<span data-ttu-id="10b92-464">请求次数过多。</span><span class="sxs-lookup"><span data-stu-id="10b92-464">Too many requests.</span></span> <span data-ttu-id="10b92-465">方法 = {0}，PublisherId = {1}</span><span class="sxs-lookup"><span data-stu-id="10b92-465">Method={0}, PublisherId={1}</span></span><ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span data-ttu-id="10b92-466">{0} = HTTP 方法</span><span class="sxs-lookup"><span data-stu-id="10b92-466">{0} = HTTP Method</span></span></p></li><li><p><span data-ttu-id="10b92-467">{1} = 用作 PublisherIdentifier 的租户 GUID</span><span class="sxs-lookup"><span data-stu-id="10b92-467">{1} = Tenant GUID used as PublisherIdentifier</span></span></p></li></ul>|
|<span data-ttu-id="10b92-468">AF50000</span><span class="sxs-lookup"><span data-stu-id="10b92-468">AF50000</span></span>|<span data-ttu-id="10b92-469">发生了内部错误。</span><span class="sxs-lookup"><span data-stu-id="10b92-469">An internal error occurred.</span></span> <span data-ttu-id="10b92-470">请重试请求。</span><span class="sxs-lookup"><span data-stu-id="10b92-470">Retry the request.</span></span>|
