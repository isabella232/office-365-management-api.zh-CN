---
ms.TocTitle: Office 365 Management Activity API reference
title: Office 365 管理活动 API 参考
description: 使用 Office 365 管理活动 API，可从 Office 365 和 Azure AD 活动日志中获取用户、管理员、系统和策略的操作和事件相关信息。
ms.ContentId: 52749845-37f8-6076-7ea5-49d9a4055445
ms.topic: reference (API)
ms.date: ''
localization_priority: Priority
ms.openlocfilehash: 48065e1770e485ffa04778d662a170ae14916354
ms.sourcegitcommit: d55928a0d535090fa2dbe94f38c7316d0e52e9a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/09/2020
ms.locfileid: "44173139"
---
# <a name="office-365-management-activity-api-reference"></a><span data-ttu-id="eff5a-103">Office 365 管理活动 API 参考</span><span class="sxs-lookup"><span data-stu-id="eff5a-103">Office 365 Management Activity API reference</span></span>

<span data-ttu-id="eff5a-104">使用 Office 365 管理活动 API，可从 Office 365 和 Azure AD 活动日志中获取用户、管理员、系统和策略的操作和事件相关信息。</span><span class="sxs-lookup"><span data-stu-id="eff5a-104">Use the Office 365 Management Activity API to retrieve information about user, admin, system, and policy actions and events from Office 365 and Azure AD activity logs.</span></span> 

<span data-ttu-id="eff5a-105">可使用 Office 365 和 Microsoft Azure Active Directory 审核和活动日志中的操作和事件，创建可提供监视、分析和数据可视化的解决方案。</span><span class="sxs-lookup"><span data-stu-id="eff5a-105">You can use the actions and events from the Office 365 and Microsoft Azure Active Directory audit and activity logs to create solutions that provide monitoring, analysis, and data visualization.</span></span> <span data-ttu-id="eff5a-106">借助这些解决方案，组织可以深入了解对自己内容执行的操作。</span><span class="sxs-lookup"><span data-stu-id="eff5a-106">These solutions give organizations greater visibility into actions taken on their content.</span></span> <span data-ttu-id="eff5a-107">Office 365 活动报告中也包含这些操作和事件。</span><span class="sxs-lookup"><span data-stu-id="eff5a-107">These actions and events are also available in the Office 365 Activity Reports.</span></span> <span data-ttu-id="eff5a-108">有关详细信息，请参阅[在 Office 365 安全与合规中心内搜索审核日志](https://support.office.com/article/Search-the-audit-log-in-the-Office-365-Security-Compliance-Center-0d4d0f35-390b-4518-800e-0c7ec95e946c)。</span><span class="sxs-lookup"><span data-stu-id="eff5a-108">For more information, see [Search the audit log in the Office 365 Security & Compliance Center](https://support.office.com/article/Search-the-audit-log-in-the-Office-365-Security-Compliance-Center-0d4d0f35-390b-4518-800e-0c7ec95e946c).</span></span>

<span data-ttu-id="eff5a-109">借助 Office 365 服务活动 API 这项 REST Web 服务，可开发使用任何语言和宿主环境（支持 HTTPS 和 X.509 证书）的解决方案。</span><span class="sxs-lookup"><span data-stu-id="eff5a-109">The Office 365 Management Activity API is a REST web service that you can use to develop solutions using any language and hosting environment that supports HTTPS and X.509 certificates.</span></span> <span data-ttu-id="eff5a-110">此 API 依赖 Azure AD 和 OAuth2 协议进行身份验证和授权。</span><span class="sxs-lookup"><span data-stu-id="eff5a-110">The API relies on Azure AD and the OAuth2 protocol for authentication and authorization.</span></span> <span data-ttu-id="eff5a-111">若要在应用程序中访问此 API，必须先在 Azure AD 中注册应用程序，并为它配置适当的权限。</span><span class="sxs-lookup"><span data-stu-id="eff5a-111">To access the API from your application, you'll need to first register it in Azure AD and configure it with appropriate permissions.</span></span> <span data-ttu-id="eff5a-112">这样，应用程序便能请求获取调用此 API 所需的 OAuth2 访问令牌。</span><span class="sxs-lookup"><span data-stu-id="eff5a-112">This will enable your application to request the OAuth2 access tokens it needs to call the API.</span></span> <span data-ttu-id="eff5a-113">有关详细信息，请参阅 [Office 365 管理 API 入门](get-started-with-office-365-management-apis.md)。</span><span class="sxs-lookup"><span data-stu-id="eff5a-113">For more information, see [Get started with Office 365 Management APIs](get-started-with-office-365-management-apis.md).</span></span>

<span data-ttu-id="eff5a-114">有关 Office 365 管理活动 API 返回的数据的信息，请参阅 [Office 365 管理活动 API 架构](office-365-management-activity-api-schema.md)。</span><span class="sxs-lookup"><span data-stu-id="eff5a-114">For information about the data that the Office 365 Management Activity API returns, see [Office 365 Management Activity API schema](office-365-management-activity-api-schema.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="eff5a-115">必须为你的 Office 365 组织启用统一审核日志记录，然后才能通过 Office 365 管理活动 API 访问数据。</span><span class="sxs-lookup"><span data-stu-id="eff5a-115">Before you can access data through the Office 365 Management Activity API, you must enable unified audit logging for your Office 365 organization.</span></span> <span data-ttu-id="eff5a-116">可通过启用 Office 365 审核日志来实现此操作。</span><span class="sxs-lookup"><span data-stu-id="eff5a-116">You do this by turning on the Office 365 audit log.</span></span> <span data-ttu-id="eff5a-117">有关说明，请参阅 [ 启用或禁用 Office 365 审核日志搜索 ](https://docs.microsoft.com/office365/securitycompliance/turn-audit-log-search-on-or-off)。</span><span class="sxs-lookup"><span data-stu-id="eff5a-117">For instructions, see [Turn Office 365 audit log search on or off](https://docs.microsoft.com/office365/securitycompliance/turn-audit-log-search-on-or-off).</span></span>


## <a name="working-with-the-office-365-management-activity-api"></a><span data-ttu-id="eff5a-118">使用 Office 365 管理活动 API</span><span class="sxs-lookup"><span data-stu-id="eff5a-118">Working with the Office 365 Management Activity API</span></span>

<span data-ttu-id="eff5a-119">Office 365 管理活动 API 将操作和事件聚合到租户专用内容 blob 中，这些 blob 按其中所含内容的类型和来源进行分类。</span><span class="sxs-lookup"><span data-stu-id="eff5a-119">The Office 365 Management Activity API aggregates actions and events into tenant-specific content blobs, which are classified by the type and source of the content they contain.</span></span> <span data-ttu-id="eff5a-120">目前支持以下内容类型：</span><span class="sxs-lookup"><span data-stu-id="eff5a-120">Currently, these content types are supported:</span></span>

- <span data-ttu-id="eff5a-121">Audit.AzureActiveDirectory</span><span class="sxs-lookup"><span data-stu-id="eff5a-121">Audit.AzureActiveDirectory</span></span>
    
- <span data-ttu-id="eff5a-122">Audit.Exchange</span><span class="sxs-lookup"><span data-stu-id="eff5a-122">Audit.Exchange</span></span>
    
- <span data-ttu-id="eff5a-123">Audit.SharePoint</span><span class="sxs-lookup"><span data-stu-id="eff5a-123">Audit.SharePoint</span></span>
    
- <span data-ttu-id="eff5a-124">Audit.General（包括前面内容类型不包含的其他所有工作负载）</span><span class="sxs-lookup"><span data-stu-id="eff5a-124">Audit.General (includes all other workloads not included in the previous content types)</span></span>

- <span data-ttu-id="eff5a-125">DLP.All（仅包括 DLP 事件的所有工作负载）</span><span class="sxs-lookup"><span data-stu-id="eff5a-125">DLP.All (DLP events only for all workloads)</span></span>
    
<span data-ttu-id="eff5a-126">若要详细了解与这些内容类型关联的事件和属性，请参阅 [Office 365 管理活动 API 架构](office-365-management-activity-api-schema.md)。</span><span class="sxs-lookup"><span data-stu-id="eff5a-126">For details about the events and properties associated with these content types, see [Office 365 Management Activity API schema](office-365-management-activity-api-schema.md).</span></span>

<span data-ttu-id="eff5a-127">若要开始检索某租户的内容 blob，请先创建对相应内容类型的订阅。</span><span class="sxs-lookup"><span data-stu-id="eff5a-127">To begin retrieving content blobs for a tenant, you first a create subscription to the desired content types.</span></span> <span data-ttu-id="eff5a-128">若要检索多个租户的内容 blob，请创建对每个相应内容类型的多个订阅（每个租户一个订阅）。</span><span class="sxs-lookup"><span data-stu-id="eff5a-128">If you are retrieving content blobs for multiple tenants, you create multiple subscriptions to each of the desired content types, one for each tenant.</span></span>

<span data-ttu-id="eff5a-129">创建订阅后，可以通过定期轮询来发现可供下载的新内容 blob，也可以向订阅注册 Webhook 终结点，这样我们便会在有新内容 blob 时向此终结点发送通知。</span><span class="sxs-lookup"><span data-stu-id="eff5a-129">After you create a subscription, you can poll regularly to discover new content blobs that are available for download, or you can register a webhook endpoint with the subscription and we will send notifications to this endpoint as new content blobs are available.</span></span>


> [!NOTE] 
> <span data-ttu-id="eff5a-130">创建订阅后，第一批内容 blob 最多可能需要 12 个小时才可用于订阅。</span><span class="sxs-lookup"><span data-stu-id="eff5a-130">When a subscription is created, it can take up to 12 hours for the first content blobs to become available for that subscription.</span></span> <span data-ttu-id="eff5a-131">内容 blob 的创建方式为，跨多个服务器和数据中心收集并聚合操作和事件。</span><span class="sxs-lookup"><span data-stu-id="eff5a-131">The content blobs are created by collecting and aggregating actions and events across multiple servers and datacenters.</span></span> <span data-ttu-id="eff5a-132">鉴于此分布式流程，内容 blob 中的操作和事件不一定按发生时间顺序显示。</span><span class="sxs-lookup"><span data-stu-id="eff5a-132">As a result of this distributed process, the actions and events contained in the content blobs will not necessarily appear in the order in which they occurred.</span></span> <span data-ttu-id="eff5a-133">可能会出现下面这种情况：一个内容 blob 中的操作和事件早于前面内容 blob 中的操作和事件发生。</span><span class="sxs-lookup"><span data-stu-id="eff5a-133">One content blob can contain actions and events that occurred prior to the actions and events contained in an earlier content blob.</span></span> <span data-ttu-id="eff5a-134">我们正在努力缩小操作和事件的发生时间与其在内容 blob 中的可用时间之间的延迟，但无法确保它们按时间顺序依序显示。</span><span class="sxs-lookup"><span data-stu-id="eff5a-134">We are working to decrease the latency between the occurrence of actions and events and their availability within a content blob, but we can't guarantee that they appear sequentially.</span></span>


> [!NOTE] 
> <span data-ttu-id="eff5a-135">只有已拥有“读取 DLP 敏感数据”权限的用户，才能通过活动源 API 访问 DLP 敏感数据。</span><span class="sxs-lookup"><span data-stu-id="eff5a-135">DLP sensitive data is only available in the activity feed API to users that have been granted “Read DLP sensitive data” permissions.</span></span> <span data-ttu-id="eff5a-136">若要详细了解数据丢失防护 (DLP)，请参阅[数据丢失防护策略概述](https://support.office.com/article/Overview-of-data-loss-prevention-policies-1966b2a7-d1e2-4d92-ab61-42efbb137f5e)。</span><span class="sxs-lookup"><span data-stu-id="eff5a-136">For more on Data Loss Prevention (DLP) see [Overview of Data Loss Prevention Policies](https://support.office.com/article/Overview-of-data-loss-prevention-policies-1966b2a7-d1e2-4d92-ab61-42efbb137f5e)</span></span>

## <a name="activity-api-operations"></a><span data-ttu-id="eff5a-137">活动 API 操作</span><span class="sxs-lookup"><span data-stu-id="eff5a-137">Activity API operations</span></span>

<span data-ttu-id="eff5a-138">所有 API 操作的范围都限定为一个租户，此 API 的根 URL 包含指定租户上下文的租户 ID。</span><span class="sxs-lookup"><span data-stu-id="eff5a-138">All API operations are scoped to a single tenant and the root URL of the API includes a tenant ID that specifies the tenant context.</span></span> <span data-ttu-id="eff5a-139">租户 ID 为 GUID。</span><span class="sxs-lookup"><span data-stu-id="eff5a-139">The tenant ID is a GUID.</span></span> <span data-ttu-id="eff5a-140">若要详细了解如何获取 GUID，请参阅 [Office 365 管理 API 入门](get-started-with-office-365-management-apis.md)。</span><span class="sxs-lookup"><span data-stu-id="eff5a-140">For information about how to get the GUID, see [Get started with Office 365 Management APIs](get-started-with-office-365-management-apis.md).</span></span> 

<span data-ttu-id="eff5a-141">由于我们发送到 Webhook 的通知包含租户 ID，因此你可使用相同的 Webhook 来接收所有租户的通知。</span><span class="sxs-lookup"><span data-stu-id="eff5a-141">Because the notifications we send to your webhook include the tenant ID, you can use the same webhook to receive notifications for all tenants.</span></span>

<span data-ttu-id="eff5a-142">你所使用的 API 终结点的 URL 基于贵公司 Microsoft 365 或 Office 365 订阅计划的类型。</span><span class="sxs-lookup"><span data-stu-id="eff5a-142">The URL for the API endpoint that you use is based on the type of Microsoft 365 or Office 365 subscription plan for your organization.</span></span>

<span data-ttu-id="eff5a-143">**企业版计划和 GCC 政府版计划**</span><span class="sxs-lookup"><span data-stu-id="eff5a-143">**Enterprise plan and GCC government plan**</span></span>

```http
https://manage.office.com/api/v1.0/{tenant_id}/activity/feed/{operation}
```

<span data-ttu-id="eff5a-144">**GCC （政府）高级版计划**</span><span class="sxs-lookup"><span data-stu-id="eff5a-144">**GCC High government plan**</span></span>

```http
https://manage.office365.us/api/v1.0/{tenant_id}/activity/feed/{operation}
```

<span data-ttu-id="eff5a-145">**DoD 政府计划**</span><span class="sxs-lookup"><span data-stu-id="eff5a-145">**DoD government plan**</span></span>

```http
https://manage.protection.apps.mil/api/v1.0/{tenant_id}/activity/feed/{operation}
```

<span data-ttu-id="eff5a-146">所有 API 操作都要求，必须有包含从 Azure AD 获取的访问令牌的授权 HTTP 头。</span><span class="sxs-lookup"><span data-stu-id="eff5a-146">All API operations require an Authorization HTTP header with an access token obtained from Azure AD.</span></span> <span data-ttu-id="eff5a-147">访问令牌中的租户 ID 必须与 API 根 URL 中的租户 ID 一致，且访问令牌必须包含 ActivityFeed.Read 声明（此声明对应于在 Azure AD 中为应用程序配置的[读取组织的活动数据]权限）。</span><span class="sxs-lookup"><span data-stu-id="eff5a-147">The tenant ID in the access token must match the tenant ID in the root URL of the API and the access token must contain the ActivityFeed.Read claim (this corresponds to the permission [Read activity data for an organization] that you configured for you application in Azure AD).</span></span>

```json
Authorization: Bearer eyJ0e...Qa6wg
```

<span data-ttu-id="eff5a-148">活动 API 支持以下操作：</span><span class="sxs-lookup"><span data-stu-id="eff5a-148">The Activity API supports the following operations:</span></span>

- <span data-ttu-id="eff5a-149">**启动订阅**：以便开始接收通知，并检索租户的活动数据。</span><span class="sxs-lookup"><span data-stu-id="eff5a-149">**Start a subscription** to begin receiving notifications and retrieving activity data for a tenant.</span></span>
    
- <span data-ttu-id="eff5a-150">**停止订阅**：以便停止检索租户数据。</span><span class="sxs-lookup"><span data-stu-id="eff5a-150">**Stop a subscription** to discontinue retrieving data for a tenant.</span></span>
    
- <span data-ttu-id="eff5a-151">**列出当前订阅**</span><span class="sxs-lookup"><span data-stu-id="eff5a-151">**List current subscriptions**</span></span>
    
- <span data-ttu-id="eff5a-152">**列出可用内容**及相应的内容 URL。</span><span class="sxs-lookup"><span data-stu-id="eff5a-152">**List available content** and the corresponding content URLs.</span></span>
    
- <span data-ttu-id="eff5a-153">**接收通知**：通知是在有新内容时由 Webhook 发送。</span><span class="sxs-lookup"><span data-stu-id="eff5a-153">**Receiving notifications** sent by a webhook when new content is available.</span></span>
    
- <span data-ttu-id="eff5a-154">**检索内容**：使用内容 URL 进行检索。</span><span class="sxs-lookup"><span data-stu-id="eff5a-154">**Retrieving content** by using the content URL.</span></span>
    
- <span data-ttu-id="eff5a-155">**列出通知**：通知是由 Webhook 发送。</span><span class="sxs-lookup"><span data-stu-id="eff5a-155">**List notifications** sent by a webhook.</span></span>

- <span data-ttu-id="eff5a-156">**检索资源易记名称**：数据源中由 GUID 标识的对象的名称。</span><span class="sxs-lookup"><span data-stu-id="eff5a-156">**Retrieve resource friendly names** for objects in the data feed identified by guids.</span></span>
    

## <a name="start-a-subscription"></a><span data-ttu-id="eff5a-157">启动订阅</span><span class="sxs-lookup"><span data-stu-id="eff5a-157">Start a subscription</span></span>

<span data-ttu-id="eff5a-158">此操作会启动对指定内容类型的订阅。</span><span class="sxs-lookup"><span data-stu-id="eff5a-158">This operation starts a subscription to the specified content type.</span></span> <span data-ttu-id="eff5a-159">如果已有对指定内容类型的订阅，此操作用于：</span><span class="sxs-lookup"><span data-stu-id="eff5a-159">If a subscription to the specified content type already exists, this operation is used to:</span></span>

- <span data-ttu-id="eff5a-160">更新有效 Webhook 的属性。</span><span class="sxs-lookup"><span data-stu-id="eff5a-160">Update the properties of an active webhook.</span></span>
    
- <span data-ttu-id="eff5a-161">启用因通知发送失败次数过多而被禁用的 Webhook。</span><span class="sxs-lookup"><span data-stu-id="eff5a-161">Enable a webhook that was disabled because of excessive failed notifications.</span></span>
    
- <span data-ttu-id="eff5a-162">通过指定更晚或为空的到期日期，重新启用已到期的 Webhook。</span><span class="sxs-lookup"><span data-stu-id="eff5a-162">Re-enable an expired webhook by specifying a later or null expiration date.</span></span>
    
- <span data-ttu-id="eff5a-163">删除 Webhook。</span><span class="sxs-lookup"><span data-stu-id="eff5a-163">Remove a webhook.</span></span>
    
||<span data-ttu-id="eff5a-164">订阅</span><span class="sxs-lookup"><span data-stu-id="eff5a-164">Subscription</span></span>|<span data-ttu-id="eff5a-165">说明</span><span class="sxs-lookup"><span data-stu-id="eff5a-165">Description</span></span>|
|:-----|:-----|:-----|
|<span data-ttu-id="eff5a-166">**路径**</span><span class="sxs-lookup"><span data-stu-id="eff5a-166">**Path**</span></span>| `/subscriptions/start?contentType={ContentType}`||
|<span data-ttu-id="eff5a-167">**参数**</span><span class="sxs-lookup"><span data-stu-id="eff5a-167">**Parameters**</span></span>|<span data-ttu-id="eff5a-168">contentType</span><span class="sxs-lookup"><span data-stu-id="eff5a-168">contentType</span></span>|<span data-ttu-id="eff5a-169">必须为有效内容类型。</span><span class="sxs-lookup"><span data-stu-id="eff5a-169">Must be a valid content type.</span></span>|
||<span data-ttu-id="eff5a-170">PublisherIdentifier</span><span class="sxs-lookup"><span data-stu-id="eff5a-170">PublisherIdentifier</span></span>|<span data-ttu-id="eff5a-171">对 API 编码的供应商的租户 GUID。</span><span class="sxs-lookup"><span data-stu-id="eff5a-171">The tenant GUID of the vendor coding against the API.</span></span> <span data-ttu-id="eff5a-172">这**不是**应用程序 GUID 或使用应用程序的客户的 GUID，而是编写代码的公司的 GUID。</span><span class="sxs-lookup"><span data-stu-id="eff5a-172">This is **not** the application GUID or the GUID of the customer using the application, but the GUID of the company writing the code.</span></span> <span data-ttu-id="eff5a-173">此参数用于限制请求速率。</span><span class="sxs-lookup"><span data-stu-id="eff5a-173">This parameter is used for throttling the request rate.</span></span> <span data-ttu-id="eff5a-174">请务必在发出的所有请求中指定此参数，以获取专用配额。</span><span class="sxs-lookup"><span data-stu-id="eff5a-174">Make sure this parameter is specified in all issued requests to get a dedicated quota.</span></span> <span data-ttu-id="eff5a-175">收到的所有不包含此参数的请求都会共用同一配额。</span><span class="sxs-lookup"><span data-stu-id="eff5a-175">All requests received without this parameter will share the same quota.</span></span>|
|<span data-ttu-id="eff5a-176">**正文**</span><span class="sxs-lookup"><span data-stu-id="eff5a-176">**Body**</span></span>|<span data-ttu-id="eff5a-177">webhook</span><span class="sxs-lookup"><span data-stu-id="eff5a-177">webhook</span></span>|<span data-ttu-id="eff5a-178">可选 JSON 对象，包含以下三个属性：</span><span class="sxs-lookup"><span data-stu-id="eff5a-178">Optional JSON object with three properties:</span></span><ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span data-ttu-id="eff5a-179"><b>address</b>：可接收通知的必需 HTTPS 终结点。</span><span class="sxs-lookup"><span data-stu-id="eff5a-179"><b>address</b>: Required HTTPS endpoint that can receive notifications.</span></span>  <span data-ttu-id="eff5a-180">在订阅创建前，Webhook 会收到用于验证自身的测试消息。</span><span class="sxs-lookup"><span data-stu-id="eff5a-180">A test message will be sent to the webhook to validate the webhook before creating the subscription.</span></span></p></li><li><p><span data-ttu-id="eff5a-181"><b>authId</b>：可选字符串，作为 WebHook-AuthID 头包含在向 Webhook 发送的通知中，用于向 Webhook 标识和授权请求来源。</span><span class="sxs-lookup"><span data-stu-id="eff5a-181"><b>authId</b>: Optional string that will be included as the WebHook-AuthID header in notifications sent to the webhook as a means of identifying and authorizing the source of the request to the webhook.</span></span></p></li><li><p><span data-ttu-id="eff5a-182"><b>expiration</b>：可选日期/时间，指明在此日期/时间后便不得再向 Webhook 发送通知。</span><span class="sxs-lookup"><span data-stu-id="eff5a-182"><b>expiration</b>: Optional datetime that indicates a datetime after which notifications should no longer be sent to the webhook.</span></span></p></li></ul>|
|<span data-ttu-id="eff5a-183">**响应**</span><span class="sxs-lookup"><span data-stu-id="eff5a-183">**Response**</span></span>|<span data-ttu-id="eff5a-184">contentType</span><span class="sxs-lookup"><span data-stu-id="eff5a-184">contentType</span></span>|<span data-ttu-id="eff5a-185">在调用中指定的内容类型。</span><span class="sxs-lookup"><span data-stu-id="eff5a-185">The content type specified in the call.</span></span>|
||<span data-ttu-id="eff5a-186">status</span><span class="sxs-lookup"><span data-stu-id="eff5a-186">status</span></span>|<span data-ttu-id="eff5a-187">订阅的状态。</span><span class="sxs-lookup"><span data-stu-id="eff5a-187">The status of the subscription.</span></span> <span data-ttu-id="eff5a-188">如果订阅已遭禁用，便无法列出或检索内容。</span><span class="sxs-lookup"><span data-stu-id="eff5a-188">If a subscription is disabled, you will not be able to list or retrieve content.</span></span>|
||<span data-ttu-id="eff5a-189">webhook</span><span class="sxs-lookup"><span data-stu-id="eff5a-189">webhook</span></span>|<span data-ttu-id="eff5a-190">在调用中指定的 Webhook 属性，以及 Webhook 的状态。</span><span class="sxs-lookup"><span data-stu-id="eff5a-190">The webhook properties specified in the call together with the status of the webhook.</span></span> <span data-ttu-id="eff5a-191">如果 Webhook 已遭禁用，便无法收到通知，但仍可以列出和检索内容，只要订阅处于启用状态即可。</span><span class="sxs-lookup"><span data-stu-id="eff5a-191">If the webhook is disabled, you will not receive notification, but you will still be able to list and retrieve content, provided the subscription is enabled.</span></span>|

#### <a name="sample-request"></a><span data-ttu-id="eff5a-192">示例请求</span><span class="sxs-lookup"><span data-stu-id="eff5a-192">Sample request</span></span>

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


#### <a name="sample-response"></a><span data-ttu-id="eff5a-193">示例响应</span><span class="sxs-lookup"><span data-stu-id="eff5a-193">Sample response</span></span>

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


## <a name="webhook-validation"></a><span data-ttu-id="eff5a-194">Webhook 验证</span><span class="sxs-lookup"><span data-stu-id="eff5a-194">Webhook validation</span></span>

<span data-ttu-id="eff5a-195">在 /start 操作已调用且 Webhook 已指定后，我们会向指定 Webhook 地址发送验证通知，以验证有效侦听器能否接受并处理通知。</span><span class="sxs-lookup"><span data-stu-id="eff5a-195">When the /start operation is called and a webhook is specified, we will send a validation notification to the specified webhook address to validate that an active listener can accept and process notifications.</span></span> <span data-ttu-id="eff5a-196">如果我们没有收到“HTTP 200 正常”响应，便不会创建订阅。</span><span class="sxs-lookup"><span data-stu-id="eff5a-196">If we do not receive an HTTP 200 OK response, the subscription will not be created.</span></span> <span data-ttu-id="eff5a-197">或者，如果 /start 正获调用以向现有订阅添加 Webhook，但我们没有收到“HTTP 200 正常”响应，便不会添加此 Webhook，但订阅仍保持不变。</span><span class="sxs-lookup"><span data-stu-id="eff5a-197">Or, if /start is being called to add a webhook to an existing subscription and a response of HTTP 200 OK is not received, the webhook will not be added and the subscription will remain unchanged.</span></span>

#### <a name="sample-request"></a><span data-ttu-id="eff5a-198">示例请求</span><span class="sxs-lookup"><span data-stu-id="eff5a-198">Sample request</span></span>

```json
POST {webhook address}
Content-Type: application/json; charset=utf-8
Webhook-AuthID: (webhook authId, if provided)
Webhook-ValidationCode: (random opaque string)

{
    "validationCode": (random opaque string, same as header)
}

```


#### <a name="sample-response"></a><span data-ttu-id="eff5a-199">示例响应</span><span class="sxs-lookup"><span data-stu-id="eff5a-199">Sample response</span></span>

```json

HTTP/1.1 200 OK

```


## <a name="stop-a-subscription"></a><span data-ttu-id="eff5a-200">停止订阅</span><span class="sxs-lookup"><span data-stu-id="eff5a-200">Stop a subscription</span></span>

<span data-ttu-id="eff5a-201">此操作会停止对指定内容类型的订阅。</span><span class="sxs-lookup"><span data-stu-id="eff5a-201">This operation stops a subscription to the specified content type.</span></span> 

<span data-ttu-id="eff5a-202">在订阅停止后，便不再会收到通知，也无法再检索可用内容。</span><span class="sxs-lookup"><span data-stu-id="eff5a-202">When a subscription is stopped, you will no longer receive notifications and you will not be able to retrieve available content.</span></span> <span data-ttu-id="eff5a-203">如果今后重启订阅，将有权访问重启时间点后的新内容，</span><span class="sxs-lookup"><span data-stu-id="eff5a-203">If the subscription is later restarted, you will have access to new content from that point forward.</span></span> <span data-ttu-id="eff5a-204">但无法检索从订阅停止时间到重启时间之间的可用内容。</span><span class="sxs-lookup"><span data-stu-id="eff5a-204">You will not be able to retrieve content that was available between the time the subscription was stopped and restarted.</span></span>


||<span data-ttu-id="eff5a-205">订阅</span><span class="sxs-lookup"><span data-stu-id="eff5a-205">Subscription</span></span>|<span data-ttu-id="eff5a-206">说明</span><span class="sxs-lookup"><span data-stu-id="eff5a-206">Description</span></span>|
|:-----|:-----|:-----|
|<span data-ttu-id="eff5a-207">**路径**</span><span class="sxs-lookup"><span data-stu-id="eff5a-207">**Path**</span></span>| `/subscriptions/stop?contentType={ContentType}`||
|<span data-ttu-id="eff5a-208">**参数**</span><span class="sxs-lookup"><span data-stu-id="eff5a-208">**Parameters**</span></span>|<span data-ttu-id="eff5a-209">contentType</span><span class="sxs-lookup"><span data-stu-id="eff5a-209">contentType</span></span>|<span data-ttu-id="eff5a-210">必须为有效内容类型。</span><span class="sxs-lookup"><span data-stu-id="eff5a-210">Must be a valid content type.</span></span>|
||<span data-ttu-id="eff5a-211">PublisherIdentifier</span><span class="sxs-lookup"><span data-stu-id="eff5a-211">PublisherIdentifier</span></span>|<span data-ttu-id="eff5a-212">对 API 编码的供应商的租户 GUID。</span><span class="sxs-lookup"><span data-stu-id="eff5a-212">The tenant GUID of the vendor coding against the API.</span></span> <span data-ttu-id="eff5a-213">这**不是**应用程序 GUID 或使用应用程序的客户的 GUID，而是编写代码的公司的 GUID。</span><span class="sxs-lookup"><span data-stu-id="eff5a-213">This is **not** the application GUID or the GUID of the customer using the application, but the GUID of the company writing the code.</span></span> <span data-ttu-id="eff5a-214">此参数用于限制请求速率。</span><span class="sxs-lookup"><span data-stu-id="eff5a-214">This parameter is used for throttling the request rate.</span></span> <span data-ttu-id="eff5a-215">请务必在发出的所有请求中指定此参数，以获取专用配额。</span><span class="sxs-lookup"><span data-stu-id="eff5a-215">Make sure this parameter is specified in all issued requests to get a dedicated quota.</span></span> <span data-ttu-id="eff5a-216">收到的所有不包含此参数的请求都会共用同一配额。</span><span class="sxs-lookup"><span data-stu-id="eff5a-216">All requests received without this parameter will share the same quota.</span></span>|
|<span data-ttu-id="eff5a-217">**正文**</span><span class="sxs-lookup"><span data-stu-id="eff5a-217">**Body**</span></span>|<span data-ttu-id="eff5a-218">（空）</span><span class="sxs-lookup"><span data-stu-id="eff5a-218">(empty)</span></span>||
|<span data-ttu-id="eff5a-219">**响应**</span><span class="sxs-lookup"><span data-stu-id="eff5a-219">**Response**</span></span>|<span data-ttu-id="eff5a-220">（空）</span><span class="sxs-lookup"><span data-stu-id="eff5a-220">(empty)</span></span>|||

#### <a name="sample-request"></a><span data-ttu-id="eff5a-221">示例请求</span><span class="sxs-lookup"><span data-stu-id="eff5a-221">Sample request</span></span>

```json
POST {root}/subscriptions/stop?contentType=Audit.SharePoint&PublisherIdentifier=46b472a7-c68e-4adf-8ade-3db49497518e
Authorization: Bearer eyJ0e...Qa6wg

```

#### <a name="sample-response"></a><span data-ttu-id="eff5a-222">示例响应</span><span class="sxs-lookup"><span data-stu-id="eff5a-222">Sample response</span></span>

```json
HTTP/1.1 200 OK
```


## <a name="list-current-subscriptions"></a><span data-ttu-id="eff5a-223">列出当前订阅</span><span class="sxs-lookup"><span data-stu-id="eff5a-223">List current subscriptions</span></span>

<span data-ttu-id="eff5a-224">此操作会返回包含当前订阅及相关 Webhook 的集合。</span><span class="sxs-lookup"><span data-stu-id="eff5a-224">This operation returns a collection of the current subscriptions together with the associated webhooks.</span></span>

||<span data-ttu-id="eff5a-225">订阅</span><span class="sxs-lookup"><span data-stu-id="eff5a-225">Subscription</span></span>|<span data-ttu-id="eff5a-226">说明</span><span class="sxs-lookup"><span data-stu-id="eff5a-226">Description</span></span>|
|:-----|:-----|:-----|
|<span data-ttu-id="eff5a-227">**路径**</span><span class="sxs-lookup"><span data-stu-id="eff5a-227">**Path**</span></span>| `/subscriptions/list`||
|<span data-ttu-id="eff5a-228">**参数**</span><span class="sxs-lookup"><span data-stu-id="eff5a-228">**Parameters**</span></span>|<span data-ttu-id="eff5a-229">PublisherIdentifier</span><span class="sxs-lookup"><span data-stu-id="eff5a-229">PublisherIdentifier</span></span>|<span data-ttu-id="eff5a-230">对 API 编码的供应商的租户 GUID。</span><span class="sxs-lookup"><span data-stu-id="eff5a-230">The tenant GUID of the vendor coding against the API.</span></span> <span data-ttu-id="eff5a-231">这**不是**应用程序 GUID 或使用应用程序的客户的 GUID，而是编写代码的公司的 GUID。</span><span class="sxs-lookup"><span data-stu-id="eff5a-231">This is **not** the application GUID or the GUID of the customer using the application, but the GUID of the company writing the code.</span></span> <span data-ttu-id="eff5a-232">此参数用于限制请求速率。</span><span class="sxs-lookup"><span data-stu-id="eff5a-232">This parameter is used for throttling the request rate.</span></span> <span data-ttu-id="eff5a-233">请务必在发出的所有请求中指定此参数，以获取专用配额。</span><span class="sxs-lookup"><span data-stu-id="eff5a-233">Make sure this parameter is specified in all issued requests to get a dedicated quota.</span></span> <span data-ttu-id="eff5a-234">收到的所有不包含此参数的请求都会共用同一配额。</span><span class="sxs-lookup"><span data-stu-id="eff5a-234">All requests received without this parameter will share the same quota.</span></span>|
|<span data-ttu-id="eff5a-235">**正文**</span><span class="sxs-lookup"><span data-stu-id="eff5a-235">**Body**</span></span>|<span data-ttu-id="eff5a-236">（空）</span><span class="sxs-lookup"><span data-stu-id="eff5a-236">(empty)</span></span>||
|<span data-ttu-id="eff5a-237">**响应**</span><span class="sxs-lookup"><span data-stu-id="eff5a-237">**Response**</span></span>|<span data-ttu-id="eff5a-238">JSON 数组</span><span class="sxs-lookup"><span data-stu-id="eff5a-238">JSON array</span></span>|<span data-ttu-id="eff5a-239">每个订阅都由包含以下三个属性的 JSON 对象表示：</span><span class="sxs-lookup"><span data-stu-id="eff5a-239">Each subscription will be represented by a JSON object with three properties:</span></span><ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span data-ttu-id="eff5a-240"><b>contentType</b>：指明内容类型。</span><span class="sxs-lookup"><span data-stu-id="eff5a-240"><b>contentType</b>: Indicates the content type.</span></span></p></li><li><p><span data-ttu-id="eff5a-241"><b>status</b>：指明订阅的状态。</span><span class="sxs-lookup"><span data-stu-id="eff5a-241"><b>status</b>: Indicates the status of the subscription.</span></span></p></li><li><p><span data-ttu-id="eff5a-242"><b>webhook</b>：指明已配置的 Webhook 及其状态（已启用、已禁用或已到期）。</span><span class="sxs-lookup"><span data-stu-id="eff5a-242"><b>webhook</b>: Indicates the configured webhook, together with the status (enabled, disabled, expired) of the webhook.</span></span>  <span data-ttu-id="eff5a-243">如果订阅没有 Webhook，虽然 Webhook 属性会显示，但值为 null。</span><span class="sxs-lookup"><span data-stu-id="eff5a-243">If a subscription does not have a webhook, the webhook property will be present but with null value.</span></span></p></li></ul>|


#### <a name="sample-request"></a><span data-ttu-id="eff5a-244">示例请求</span><span class="sxs-lookup"><span data-stu-id="eff5a-244">Sample request</span></span>

```json
GET {root}/subscriptions/list?PublisherIdentifier=46b472a7-c68e-4adf-8ade-3db49497518e
Authorization: Bearer eyJ0e...Qa6wg

```

#### <a name="sample-response"></a><span data-ttu-id="eff5a-245">示例响应</span><span class="sxs-lookup"><span data-stu-id="eff5a-245">Sample response</span></span>

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


## <a name="list-available-content"></a><span data-ttu-id="eff5a-246">列出可用内容</span><span class="sxs-lookup"><span data-stu-id="eff5a-246">List available content</span></span>

<span data-ttu-id="eff5a-247">此操作会列出当前可供检索的指定内容类型内容。</span><span class="sxs-lookup"><span data-stu-id="eff5a-247">This operation lists the content currently available for retrieval for the specified content type.</span></span> <span data-ttu-id="eff5a-248">内容中聚合了从跨多个数据中心的多个服务器中捕获的操作和事件。</span><span class="sxs-lookup"><span data-stu-id="eff5a-248">The content is an aggregation of actions and events harvested from multiple servers across multiple datacenters.</span></span> <span data-ttu-id="eff5a-249">内容按聚合可用时间顺序列出，但无法保证聚合中的事件和操作按时间顺序依序列出。</span><span class="sxs-lookup"><span data-stu-id="eff5a-249">The content will be listed in the order in which the aggregations become available, but the events and actions within the aggregations are not guaranteed to be sequential.</span></span> <span data-ttu-id="eff5a-250">如果订阅处于已禁用状态，返回的便是错误。</span><span class="sxs-lookup"><span data-stu-id="eff5a-250">An error is returned if the subscription status is disabled.</span></span>


||<span data-ttu-id="eff5a-251">订阅</span><span class="sxs-lookup"><span data-stu-id="eff5a-251">Subscription</span></span>|<span data-ttu-id="eff5a-252">说明</span><span class="sxs-lookup"><span data-stu-id="eff5a-252">Description</span></span>|
|:-----|:-----|:-----|
|<span data-ttu-id="eff5a-253">**路径**</span><span class="sxs-lookup"><span data-stu-id="eff5a-253">**Path**</span></span>| `/subscriptions/content?contentType={ContentType}&amp;startTime={0}&amp;endTime={1}`||
|<span data-ttu-id="eff5a-254">**参数**</span><span class="sxs-lookup"><span data-stu-id="eff5a-254">**Parameters**</span></span>|<span data-ttu-id="eff5a-255">contentType</span><span class="sxs-lookup"><span data-stu-id="eff5a-255">contentType</span></span>|<span data-ttu-id="eff5a-256">必须为有效内容类型。</span><span class="sxs-lookup"><span data-stu-id="eff5a-256">Must be a valid content type.</span></span>|
||<span data-ttu-id="eff5a-257">PublisherIdentifier</span><span class="sxs-lookup"><span data-stu-id="eff5a-257">PublisherIdentifier</span></span>|<span data-ttu-id="eff5a-258">对 API 编码的供应商的租户 GUID。</span><span class="sxs-lookup"><span data-stu-id="eff5a-258">The tenant GUID of the vendor coding against the API.</span></span> <span data-ttu-id="eff5a-259">这**不是**应用程序 GUID 或使用应用程序的客户的 GUID，而是编写代码的公司的 GUID。</span><span class="sxs-lookup"><span data-stu-id="eff5a-259">This is **not** the application GUID or the GUID of the customer using the application, but the GUID of the company writing the code.</span></span> <span data-ttu-id="eff5a-260">此参数用于限制请求速率。</span><span class="sxs-lookup"><span data-stu-id="eff5a-260">This parameter is used for throttling the request rate.</span></span> <span data-ttu-id="eff5a-261">请务必在发出的所有请求中指定此参数，以获取专用配额。</span><span class="sxs-lookup"><span data-stu-id="eff5a-261">Make sure this parameter is specified in all issued requests to get a dedicated quota.</span></span> <span data-ttu-id="eff5a-262">收到的所有不包含此参数的请求都会共用同一配额。</span><span class="sxs-lookup"><span data-stu-id="eff5a-262">All requests received without this parameter will share the same quota.</span></span>|
||<span data-ttu-id="eff5a-263">startTime endTime</span><span class="sxs-lookup"><span data-stu-id="eff5a-263">startTime endTime</span></span>|<span data-ttu-id="eff5a-264">依据为内容可用时间的可选日期/时间 (UTC)，指明要返回的内容的时间范围。</span><span class="sxs-lookup"><span data-stu-id="eff5a-264">Optional datetimes (UTC) indicating the time range of content to return, based on when the content became available.</span></span> <span data-ttu-id="eff5a-265">此时间范围包含 startTime (startTime <= contentCreated)，但不包含 endTime (contentCreated < endTime)，这样便能使用不重叠的增量时间间隔翻阅可用内容。</span><span class="sxs-lookup"><span data-stu-id="eff5a-265">The time range is inclusive with respect to startTime (startTime <= contentCreated) and exclusive with respect to endTime (contentCreated < endTime), so that non-overlapping, incrementing time intervals can used to page through available content.</span></span><ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span data-ttu-id="eff5a-266">YYYY-MM-DD</span><span class="sxs-lookup"><span data-stu-id="eff5a-266">YYYY-MM-DD</span></span></p></li><li><p><span data-ttu-id="eff5a-267">YYYY-MM-DDTHH:MM</span><span class="sxs-lookup"><span data-stu-id="eff5a-267">YYYY-MM-DDTHH:MM</span></span></p></li><li><p><span data-ttu-id="eff5a-268">YYYY-MM-DDTHH:MM:SS</span><span class="sxs-lookup"><span data-stu-id="eff5a-268">YYYY-MM-DDTHH:MM:SS</span></span></p></li></ul><span data-ttu-id="eff5a-269">必须同时指定（或同时省略）开始时间和结束时间，两者间隔不得超过 24 小时，且开始时间与当前时间的间隔不得超过 7 天。</span><span class="sxs-lookup"><span data-stu-id="eff5a-269">Both must be specified (or both omitted) and they must be no more than 24 hours apart, with the start time no more than 7 days in the past.</span></span> <span data-ttu-id="eff5a-270">默认情况下，如果 startTime 和 endTime 遭省略，返回的便是过去 24 小时内的可用内容。</span><span class="sxs-lookup"><span data-stu-id="eff5a-270">By default, if startTime and endTime are omitted, then the content available in the last 24 hours is returned.</span></span><p><span data-ttu-id="eff5a-271">**注意**：即使可以指定间隔超过 24 小时的 startTime 和 endTime，也不建议这样做。</span><span class="sxs-lookup"><span data-stu-id="eff5a-271">**NOTE**: Even though it is possible to specify a startTime and endTime more than 24 hours apart, this is not recommended.</span></span> <span data-ttu-id="eff5a-272">此外，如果确实返回了开始时间和结束时间间隔超过 24 小时的请求的任何响应结果，它们可能只是部分结果，不得纳入考虑范围。</span><span class="sxs-lookup"><span data-stu-id="eff5a-272">Furthermore, if you do get any results in response to a request for more than 24 hours, these could be partial results and should not be taken into account.</span></span> <span data-ttu-id="eff5a-273">应发出 startTime 和 endTime 间隔不超过 24 小时的请求。</span><span class="sxs-lookup"><span data-stu-id="eff5a-273">The request should be issued with an interval of no more than 24 hours between the startTime and endTime.</span></span></p>|
|<span data-ttu-id="eff5a-274">**响应**</span><span class="sxs-lookup"><span data-stu-id="eff5a-274">**Response**</span></span>|<span data-ttu-id="eff5a-275">JSON 数组</span><span class="sxs-lookup"><span data-stu-id="eff5a-275">JSON array</span></span>|<span data-ttu-id="eff5a-276">可用内容由包含以下属性的 JSON 对象表示：</span><span class="sxs-lookup"><span data-stu-id="eff5a-276">The available content will be represented by JSON objects with the following properties:</span></span><ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span data-ttu-id="eff5a-277"><b>contentType</b>：指明内容类型。</span><span class="sxs-lookup"><span data-stu-id="eff5a-277"><b>contentType</b>: Indicates the content type.</span></span></p></li><li><p><span data-ttu-id="eff5a-278"><b>contentId</b>：用于唯一标识内容的不透明字符串。</span><span class="sxs-lookup"><span data-stu-id="eff5a-278"><b>contentId</b>: An opaque string that uniquely identifies the content.</span></span></p></li><li><p><span data-ttu-id="eff5a-279"><b>contentUri</b>：供检索内容时使用的 URL。</span><span class="sxs-lookup"><span data-stu-id="eff5a-279"><b>contentUri</b>: The URL to use when retrieving the content.</span></span></p></li><li><p><span data-ttu-id="eff5a-280"><b>contentCreated</b>：内容的可用日期/时间。</span><span class="sxs-lookup"><span data-stu-id="eff5a-280"><b>contentCreated</b>: The datetime when the content was made available.</span></span></p></li><li><p><span data-ttu-id="eff5a-281"><b>contentExpiration</b>：内容不再可供检索的日期/时间。</span><span class="sxs-lookup"><span data-stu-id="eff5a-281"><b>contentExpiration</b>: The datetime after which the content will no longer be available for retrieval.</span></span></p></li></ul>|


#### <a name="sample-request"></a><span data-ttu-id="eff5a-282">示例请求</span><span class="sxs-lookup"><span data-stu-id="eff5a-282">Sample request</span></span>

```json
GET {root}/subscriptions/content?contentType=Audit.SharePoint&PublisherIdentifier=46b472a7-c68e-4adf-8ade-3db49497518e
Authorization: Bearer eyJ0e...Qa6wg

```

#### <a name="sample-response"></a><span data-ttu-id="eff5a-283">示例响应</span><span class="sxs-lookup"><span data-stu-id="eff5a-283">Sample response</span></span>

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


### <a name="pagination"></a><span data-ttu-id="eff5a-284">分页</span><span class="sxs-lookup"><span data-stu-id="eff5a-284">Pagination</span></span>

<span data-ttu-id="eff5a-285">列出某个时间范围的可用内容时，返回的结果数会受到限制，以防止响应超时。</span><span class="sxs-lookup"><span data-stu-id="eff5a-285">When listing available content for a time range, the number of results returned is limited to prevent response timeouts.</span></span> <span data-ttu-id="eff5a-286">如果指定时间范围的结果数超出一个响应中可返回的结果数，结果便会被截断，并向响应添加头，以指明用于检索下页结果的 URL。</span><span class="sxs-lookup"><span data-stu-id="eff5a-286">If there are more results in the specified time range than can be returned in single response, the results will be truncated and a header will be added to the response indicating the URL to use to retrieve the next page of results.</span></span> <span data-ttu-id="eff5a-287">此 URL 包含在初始请求中指定的相同 _startTime_ 和 _endTime_ 参数，以及用于指明下页的内部 ID 的参数。</span><span class="sxs-lookup"><span data-stu-id="eff5a-287">The URL will contain the same  _startTime_ and _endTime_ parameters that were specified in the original request, together with a parameter indicating the internal ID of the next page.</span></span> <span data-ttu-id="eff5a-288">如果初始请求中未指定 _startTime_ 和 _endTime_，便会设置为反映初始请求前 24 小时间隔内的结果。</span><span class="sxs-lookup"><span data-stu-id="eff5a-288">If _startTime_ and _endTime_ were not specified in the original request, they will be set to reflect the 24-hour interval that preceded the original request.</span></span>


```json
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
NextPageUri: https://manage.office.com/api/v1/{tenant_id}/activity/feed/subscriptions/content?contentType=Audit.SharePoint&amp;startTime=2015-10-01&amp;endTime=2015-10-02&amp;nextPage=2015101900R022885001761

```

<span data-ttu-id="eff5a-289">若要列出指定时间范围的所有可用内容，可能需要检索多页，直到收到的响应不含 **NextPageUri** 头为止。</span><span class="sxs-lookup"><span data-stu-id="eff5a-289">To list all available content for a specified time range, you might need to retrieve multiple pages until a response without the **NextPageUri** header is received.</span></span>


## <a name="receiving-notifications"></a><span data-ttu-id="eff5a-290">接收通知</span><span class="sxs-lookup"><span data-stu-id="eff5a-290">Receiving notifications</span></span>

<span data-ttu-id="eff5a-291">当有新内容可用时，我们便会向订阅的已配置 Webhook 发送通知。</span><span class="sxs-lookup"><span data-stu-id="eff5a-291">Notifications are sent to the configured webhook for a subscription as new content becomes available.</span></span> <span data-ttu-id="eff5a-292">由于通知包含租户标识符，因此可使用同一个 Webhook 接收已订阅的所有租户的通知。</span><span class="sxs-lookup"><span data-stu-id="eff5a-292">Because the notification includes the tenant identifier, you can use the same webhook to receive notifications for all tenants for which you have subscriptions.</span></span>

<span data-ttu-id="eff5a-293">通知是以 HTTP POST 形式通过 TLS（TLS 1.0 及更高版本）发送到指定 Webhook 地址。</span><span class="sxs-lookup"><span data-stu-id="eff5a-293">The notification is made as an HTTP POST over TLS (TLS 1.0 and later versions) to the specified webhook address.</span></span> <span data-ttu-id="eff5a-294">如果 Webhook 配置包括验证 ID，我们便会以 HTTP 头 (Webhook-AuthID) 的形式发送它。</span><span class="sxs-lookup"><span data-stu-id="eff5a-294">If the webhook configuration includes an auth ID, we will send it as an HTTP header: Webhook-AuthID.</span></span> <span data-ttu-id="eff5a-295">除“HTTP 200 正常”之外的其他任何响都被视为失败，我们会重试发送通知。</span><span class="sxs-lookup"><span data-stu-id="eff5a-295">Any response other than HTTP 200 OK will be considered a failure and the notification will be retried.</span></span> <span data-ttu-id="eff5a-296">还可以将 Webhook 配置为要求基于客户端证书的身份验证，我们将使用 manage.office.com 证书进行身份验证。</span><span class="sxs-lookup"><span data-stu-id="eff5a-296">You can also configure your webhook to require client certificate-based authentication and we will authenticate using the manage.office.com certificate.</span></span>

<span data-ttu-id="eff5a-297">请求正文包含一个数组，其中有一个或多个表示可用内容 blob 的 JSON 对象。</span><span class="sxs-lookup"><span data-stu-id="eff5a-297">The body of the request will contain an array of one or more JSON objects that represent the available content blobs.</span></span> <span data-ttu-id="eff5a-298">为了保持通知相对较小，各个通知中的内容 blob 数受到限制。</span><span class="sxs-lookup"><span data-stu-id="eff5a-298">The number of content blobs in each notification is limited to keep the size of the notification relatively small.</span></span> <span data-ttu-id="eff5a-299">由于此限制可能会更改，因此实现应查询数组长度，而不要以为它是固定大小。</span><span class="sxs-lookup"><span data-stu-id="eff5a-299">Because this limit might change, your implementation should query for the length of the array instead of expecting a fixed size.</span></span> <span data-ttu-id="eff5a-300">每个对象包含 /content 操作返回的相同属性，以及数据所属租户的 GUID 和创建订阅的应用程序的 GUID。</span><span class="sxs-lookup"><span data-stu-id="eff5a-300">Each object will include the same properties returned by the /content operation, together with the GUID of the tenant to which the data belongs and the GUID of your application that created the subscriptions.</span></span> <span data-ttu-id="eff5a-301">这样，Webhook 便可以在用于多个租户和应用程序时创建上下文。</span><span class="sxs-lookup"><span data-stu-id="eff5a-301">This allows the webhook to establish context when it is being used with multiple tenants and applications.</span></span>

- <span data-ttu-id="eff5a-302">**tenantId**：内容所属租户的 GUID。</span><span class="sxs-lookup"><span data-stu-id="eff5a-302">**tenantId**: The GUID of the tenant to which the content belongs.</span></span>
    
- <span data-ttu-id="eff5a-303">**clientId**：创建订阅的应用程序的 GUID。</span><span class="sxs-lookup"><span data-stu-id="eff5a-303">**clientId**: The GUID of your application that created the subscription.</span></span>
    
- <span data-ttu-id="eff5a-304">**contentType**：指明内容类型。</span><span class="sxs-lookup"><span data-stu-id="eff5a-304">**contentType**: Indicates the content type.</span></span>
    
- <span data-ttu-id="eff5a-305">**contentId**：用于唯一标识内容的不透明字符串。</span><span class="sxs-lookup"><span data-stu-id="eff5a-305">**contentId**: An opaque string that uniquely identifies the content.</span></span>
    
- <span data-ttu-id="eff5a-306">**contentUri**：供检索内容时使用的 URL。</span><span class="sxs-lookup"><span data-stu-id="eff5a-306">**contentUri**: The URL to use when retrieving the content.</span></span>
    
- <span data-ttu-id="eff5a-307">**contentCreated**：内容的可用日期/时间。</span><span class="sxs-lookup"><span data-stu-id="eff5a-307">**contentCreated**: The datetime when the content was made available.</span></span>
    
- <span data-ttu-id="eff5a-308">**contentExpiration**：内容不再可供检索的日期/时间。</span><span class="sxs-lookup"><span data-stu-id="eff5a-308">**contentExpiration**: The datetime after which the content will no longer be available for retrieval.</span></span>
    
<span data-ttu-id="eff5a-309">下面是一个通知示例。</span><span class="sxs-lookup"><span data-stu-id="eff5a-309">The following is an example of a notification.</span></span>

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


## <a name="notification-failure-and-retry"></a><span data-ttu-id="eff5a-310">通知失败和重试</span><span class="sxs-lookup"><span data-stu-id="eff5a-310">Notification failure and retry</span></span>

<span data-ttu-id="eff5a-311">通知系统在有新内容可用时发送通知。</span><span class="sxs-lookup"><span data-stu-id="eff5a-311">The notification system sends notifications as new content becomes available.</span></span> <span data-ttu-id="eff5a-312">如果通知发送失败次数过多，重试机制便会以指数方式增加两次重试间隔。</span><span class="sxs-lookup"><span data-stu-id="eff5a-312">If we encounter excessive failures when sending notifications, our retry mechanism will exponentially increase the time between retries.</span></span> <span data-ttu-id="eff5a-313">如果一再失败，我们有权禁用 Webhook 并彻底停止向它发送通知。</span><span class="sxs-lookup"><span data-stu-id="eff5a-313">If we continue to encounter failures, we reserve the right to disable the webhook and stop sending notifications to it altogether.</span></span> <span data-ttu-id="eff5a-314">可使用 /start 操作重新启用已禁用的 Webhook。</span><span class="sxs-lookup"><span data-stu-id="eff5a-314">The /start operation can be used to re-enable a disabled webhook.</span></span>


## <a name="retrieving-content"></a><span data-ttu-id="eff5a-315">检索内容</span><span class="sxs-lookup"><span data-stu-id="eff5a-315">Retrieving content</span></span>

<span data-ttu-id="eff5a-316">若要检索内容 blob，请对可用内容列表和已发送到 Webhook 的通知中包含的相应内容 URI 发出 GET 请求。</span><span class="sxs-lookup"><span data-stu-id="eff5a-316">To retrieve a content blob, make a GET request against the corresponding content URI that is included in the list of available content and in the notifications sent to a webhook.</span></span> <span data-ttu-id="eff5a-317">返回的内容为一个集合，其中包含一个或多个 JSON 格式的操作和事件。</span><span class="sxs-lookup"><span data-stu-id="eff5a-317">The returned content will be a collection of one more actions or events in JSON format.</span></span>

#### <a name="sample-request"></a><span data-ttu-id="eff5a-318">示例请求</span><span class="sxs-lookup"><span data-stu-id="eff5a-318">Sample request</span></span>

```json
GET https://manage.office.com/api/v1.0/41463f53-8812-40f4-890f-865bf6e35190/activity/feed/audit/301299007231$301299007231$41463f53881240f4890f865bf6e35190aad2015062920$e1c2ab19858a469fb1f1fd097effffc9$04 HTTP/1.1
Authorization: Bearer eyJ0e...Qa6wg
```

#### <a name="sample-response"></a><span data-ttu-id="eff5a-319">示例响应</span><span class="sxs-lookup"><span data-stu-id="eff5a-319">Sample response</span></span>

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


## <a name="list-notifications"></a><span data-ttu-id="eff5a-320">列出通知</span><span class="sxs-lookup"><span data-stu-id="eff5a-320">List notifications</span></span>

<span data-ttu-id="eff5a-321">此操作会列出尝试对指定内容类型发送的所有通知。</span><span class="sxs-lookup"><span data-stu-id="eff5a-321">This operation lists all notification attempts for the specified content type.</span></span> <span data-ttu-id="eff5a-322">如果在启动对内容类型的订阅时未添加 Webhook，便无通知可供检索。</span><span class="sxs-lookup"><span data-stu-id="eff5a-322">If you did not include a webhook when starting the subscription to the content type, there will be no notifications to retrieve.</span></span> <span data-ttu-id="eff5a-323">因为我们会在失败时重试发送通知，所以此操作可能会返回有关同一内容的多个通知，并且通知发送顺序不一定与内容可用时间顺序一致（尤其是在失败后重试发送通知时）。</span><span class="sxs-lookup"><span data-stu-id="eff5a-323">Because we retry notifications in the event of failure, this operation can return multiple notifications for the same content, and the order in which the notifications are sent will not necessarily match the order in which the content became available (especially when there are failures and retries).</span></span> 

<span data-ttu-id="eff5a-324">此操作可有助于调查与 Webhook 和通知相关的问题，但不得用它来确定当前什么内容可供检索。</span><span class="sxs-lookup"><span data-stu-id="eff5a-324">You can use this operation to help investigate issues related to webhooks and notifications, but you should not use it to determine what content is currently available for retrieval.</span></span> <span data-ttu-id="eff5a-325">若要确定，请改用 /content 操作。</span><span class="sxs-lookup"><span data-stu-id="eff5a-325">Use the /content operation instead.</span></span> <span data-ttu-id="eff5a-326">如果订阅处于已禁用状态，返回的便是错误。</span><span class="sxs-lookup"><span data-stu-id="eff5a-326">We return an error if the subscription status is disabled.</span></span>


||<span data-ttu-id="eff5a-327">订阅</span><span class="sxs-lookup"><span data-stu-id="eff5a-327">Subscription</span></span>|<span data-ttu-id="eff5a-328">说明</span><span class="sxs-lookup"><span data-stu-id="eff5a-328">Description</span></span>|
|:-----|:-----|:-----|
|<span data-ttu-id="eff5a-329">**路径**</span><span class="sxs-lookup"><span data-stu-id="eff5a-329">**Path**</span></span>| `/subscriptions/notifications?contentType={ContentType}&amp;startTime={0}&amp;endTime={1}`||
|<span data-ttu-id="eff5a-330">**参数**</span><span class="sxs-lookup"><span data-stu-id="eff5a-330">**Parameters**</span></span>|<span data-ttu-id="eff5a-331">contentType</span><span class="sxs-lookup"><span data-stu-id="eff5a-331">contentType</span></span>|<span data-ttu-id="eff5a-332">必须为有效内容类型。</span><span class="sxs-lookup"><span data-stu-id="eff5a-332">Must be a valid content type.</span></span>|
||<span data-ttu-id="eff5a-333">PublisherIdentifier</span><span class="sxs-lookup"><span data-stu-id="eff5a-333">PublisherIdentifier</span></span>|<span data-ttu-id="eff5a-334">对 API 编码的供应商的租户 GUID。</span><span class="sxs-lookup"><span data-stu-id="eff5a-334">The tenant GUID of the vendor coding against the API.</span></span> <span data-ttu-id="eff5a-335">这**不是**应用程序 GUID 或使用应用程序的客户的 GUID，而是编写代码的公司的 GUID。</span><span class="sxs-lookup"><span data-stu-id="eff5a-335">This is **not** the application GUID or the GUID of the customer using the application, but the GUID of the company writing the code.</span></span> <span data-ttu-id="eff5a-336">此参数用于限制请求速率。</span><span class="sxs-lookup"><span data-stu-id="eff5a-336">This parameter is used for throttling the request rate.</span></span> <span data-ttu-id="eff5a-337">请务必在发出的所有请求中指定此参数，以获取专用配额。</span><span class="sxs-lookup"><span data-stu-id="eff5a-337">Make sure this parameter is specified in all issued requests to get a dedicated quota.</span></span> <span data-ttu-id="eff5a-338">收到的所有不包含此参数的请求都会共用同一配额。</span><span class="sxs-lookup"><span data-stu-id="eff5a-338">All requests received without this parameter will share the same quota.</span></span>|
||<span data-ttu-id="eff5a-339">startTime endTime</span><span class="sxs-lookup"><span data-stu-id="eff5a-339">startTime endTime</span></span>|<span data-ttu-id="eff5a-340">依据为内容可用时间的可选日期/时间 (UTC)，指明要返回的内容的时间范围。</span><span class="sxs-lookup"><span data-stu-id="eff5a-340">Optional datetimes (UTC) that indicate the time range of content to return, based on when the content became available.</span></span> <span data-ttu-id="eff5a-341">此时间范围包含 _startTime_ ( _startTime_ <= contentCreated)，但不包含 _endTime_ (_contentCreated_ < endTime)，这样便能使用不重叠的增量时间间隔翻阅可用内容。</span><span class="sxs-lookup"><span data-stu-id="eff5a-341">The time range is inclusive with respect to  _startTime_ ( _startTime_ <= contentCreated) and exclusive with respect to _endTime_ (_contentCreated_ < endTime), so that non-overlapping, incrementing time intervals can used to page through available content.</span></span><ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span data-ttu-id="eff5a-342">YYYY-MM-DD</span><span class="sxs-lookup"><span data-stu-id="eff5a-342">YYYY-MM-DD</span></span></p></li><li><p><span data-ttu-id="eff5a-343">YYYY-MM-DDTHH:MM</span><span class="sxs-lookup"><span data-stu-id="eff5a-343">YYYY-MM-DDTHH:MM</span></span></p></li><li><p><span data-ttu-id="eff5a-344">YYYY-MM-DDTHH:MM:SS</span><span class="sxs-lookup"><span data-stu-id="eff5a-344">YYYY-MM-DDTHH:MM:SS</span></span></p></li></ul><span data-ttu-id="eff5a-345">必须同时指定（或同时省略）开始时间和结束时间，两者间隔不得超过 24 小时，且开始时间与当前时间的间隔不得超过 7 天。</span><span class="sxs-lookup"><span data-stu-id="eff5a-345">Both must be specified (or both omitted) and they must be no more than 24 hours apart, with the start time no more than 7 days in the past.</span></span> <span data-ttu-id="eff5a-346">默认情况下，如果 _startTime_ 和 _endTime_ 遭省略，返回的便是过去 24 小时内的可用内容。</span><span class="sxs-lookup"><span data-stu-id="eff5a-346">By default, if  _startTime_ and _endTime_ are omitted, the content available in the last 24 hours is returned.</span></span>|
|<span data-ttu-id="eff5a-347">**响应**</span><span class="sxs-lookup"><span data-stu-id="eff5a-347">**Response**</span></span>|<span data-ttu-id="eff5a-348">JSON 数组</span><span class="sxs-lookup"><span data-stu-id="eff5a-348">JSON array</span></span>|<span data-ttu-id="eff5a-349">通知由包含以下属性的 JSON 对象表示：</span><span class="sxs-lookup"><span data-stu-id="eff5a-349">The notifications will be represented by JSON objects with the following properties:</span></span> <ul><li><span data-ttu-id="eff5a-350">**contentType**：指明内容类型。</span><span class="sxs-lookup"><span data-stu-id="eff5a-350">**contentType**: indicates the content type.</span></span></li><li><span data-ttu-id="eff5a-351">**contentId**：用于唯一标识内容的不透明字符串。</span><span class="sxs-lookup"><span data-stu-id="eff5a-351">**contentId**: an opaque string that uniquely identifies the content.</span></span></li><li><span data-ttu-id="eff5a-352">**contentUri**：供检索内容时使用的 URL。</span><span class="sxs-lookup"><span data-stu-id="eff5a-352">**contentUri**: the URL to use when retrieving the content.</span></span> </li><li><span data-ttu-id="eff5a-353">**contentCreated**：内容的可用日期/时间。</span><span class="sxs-lookup"><span data-stu-id="eff5a-353">**contentCreated**: the datetime when the content was made available.</span></span></li><li><span data-ttu-id="eff5a-354">**contentExpiration**：内容不再可供检索的日期/时间。</span><span class="sxs-lookup"><span data-stu-id="eff5a-354">**contentExpiration**: the datetime after which the content will no longer be available for retrieval.</span></span></li><li><span data-ttu-id="eff5a-355">**notificationSent**：通知发送日期/时间。</span><span class="sxs-lookup"><span data-stu-id="eff5a-355">**notificationSent**: the datetime when the notification was sent.</span></span></li><li><span data-ttu-id="eff5a-356">**notificationStatus**：指明通知尝试发送是成功还是失败。</span><span class="sxs-lookup"><span data-stu-id="eff5a-356">**notificationStatus**: indicates the success or failure of the notification attempt.</span></span></li></ul>|

#### <a name="sample-request"></a><span data-ttu-id="eff5a-357">示例请求</span><span class="sxs-lookup"><span data-stu-id="eff5a-357">Sample request</span></span>

```json
GET {root}/subscriptions/notifications?contentType=Audit.SharePoint&PublisherIdentifier=46b472a7-c68e-4adf-8ade-3db49497518e
Authorization: Bearer eyJ0e...Qa6wg

```

#### <a name="sample-response"></a><span data-ttu-id="eff5a-358">示例响应</span><span class="sxs-lookup"><span data-stu-id="eff5a-358">Sample response</span></span>

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


### <a name="pagination"></a><span data-ttu-id="eff5a-359">分页</span><span class="sxs-lookup"><span data-stu-id="eff5a-359">Pagination</span></span>

<span data-ttu-id="eff5a-360">列出某个时间范围的通知历史记录时，返回的结果数会受到限制，以防止响应超时。</span><span class="sxs-lookup"><span data-stu-id="eff5a-360">When listing notification history for a time range, the number of results returned is limited to prevent response timeouts.</span></span> <span data-ttu-id="eff5a-361">如果指定时间范围的结果数超出一个响应中可返回的结果数，结果便会被截断，并向响应添加头，以指明用于检索下页结果的 URL。</span><span class="sxs-lookup"><span data-stu-id="eff5a-361">If there are more results in the specified time range than can be returned in a single response, the results are truncated and a header is added to the response indicating the URL to use to retrieve the next page of results.</span></span> <span data-ttu-id="eff5a-362">此 URL 包含在初始请求中指定的相同 _startTime_ 和 _endTime_ 参数，以及用于指明下页的内部 ID 的参数。</span><span class="sxs-lookup"><span data-stu-id="eff5a-362">The URL will contain the same  _startTime_ and _endTime_ parameters that were specified in the original request, together with a parameter indicating the internal ID of the next page.</span></span> <span data-ttu-id="eff5a-363">如果初始请求中未指定 _startTime_ 和 _endTime_，便会设置为反映初始请求前 24 小时间隔内的结果。</span><span class="sxs-lookup"><span data-stu-id="eff5a-363">If _startTime_ and _endTime_ were not specified in the original request, they will be set to reflect the 24-hour interval that preceded the original request.</span></span>

```json
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
NextPageUrl: https://manage.office.com/api/v1/{tenant_id}/activity/feed/subscriptions/content?contentType=Audit.SharePoint&amp;startTime=2015-10-01&amp;endTime=2015-10-02&amp;nextPage=2015101900R022885001761

```

<span data-ttu-id="eff5a-364">若要列出指定时间范围的所有可用内容，可能需要检索多页，直到收到的响应不含 **NextPageUrl** 头为止。</span><span class="sxs-lookup"><span data-stu-id="eff5a-364">To list all available content for a specified time range, you might need to retrieve multiple pages until a response without the **NextPageUrl** header is received.</span></span>

## <a name="retrieve-resource-friendly-names"></a><span data-ttu-id="eff5a-365">检索资源易记名称</span><span class="sxs-lookup"><span data-stu-id="eff5a-365">Retrieve resource friendly names</span></span>

<span data-ttu-id="eff5a-366">此操作会检索数据源中由 GUID 标识的对象的易记名称。</span><span class="sxs-lookup"><span data-stu-id="eff5a-366">This operation retrieves friendly names for objects in the data feed identified by guids.</span></span> <span data-ttu-id="eff5a-367">目前，仅支持“DlpSensitiveType”对象。</span><span class="sxs-lookup"><span data-stu-id="eff5a-367">Currently "DlpSensitiveType" is the only supported object.</span></span> 


||<span data-ttu-id="eff5a-368">订阅</span><span class="sxs-lookup"><span data-stu-id="eff5a-368">Subscription</span></span>|<span data-ttu-id="eff5a-369">说明</span><span class="sxs-lookup"><span data-stu-id="eff5a-369">Description</span></span>|
|:-----|:-----|:-----|
|<span data-ttu-id="eff5a-370">**路径**</span><span class="sxs-lookup"><span data-stu-id="eff5a-370">**Path**</span></span>| `/resources/dlpSensitiveTypes`||
|<span data-ttu-id="eff5a-371">**参数**</span><span class="sxs-lookup"><span data-stu-id="eff5a-371">**Parameters**</span></span>|<span data-ttu-id="eff5a-372">PublisherIdentifier</span><span class="sxs-lookup"><span data-stu-id="eff5a-372">PublisherIdentifier</span></span>|<span data-ttu-id="eff5a-373">对 API 编码的供应商的租户 GUID。</span><span class="sxs-lookup"><span data-stu-id="eff5a-373">The tenant GUID of the vendor coding against the API.</span></span> <span data-ttu-id="eff5a-374">这**不是**应用程序 GUID 或使用应用程序的客户的 GUID，而是编写代码的公司的 GUID。</span><span class="sxs-lookup"><span data-stu-id="eff5a-374">This is **not** the application GUID or the GUID of the customer using the application, but the GUID of the company writing the code.</span></span> <span data-ttu-id="eff5a-375">此参数用于限制请求速率。</span><span class="sxs-lookup"><span data-stu-id="eff5a-375">This parameter is used for throttling the request rate.</span></span> <span data-ttu-id="eff5a-376">请务必在发出的所有请求中指定此参数，以获取专用配额。</span><span class="sxs-lookup"><span data-stu-id="eff5a-376">Make sure this parameter is specified in all issued requests to get a dedicated quota.</span></span> <span data-ttu-id="eff5a-377">收到的所有不包含此参数的请求都会共用同一配额。</span><span class="sxs-lookup"><span data-stu-id="eff5a-377">All requests received without this parameter will share the same quota.</span></span>|
|<span data-ttu-id="eff5a-378">**头**</span><span class="sxs-lookup"><span data-stu-id="eff5a-378">**Headers**</span></span>|<span data-ttu-id="eff5a-379">Accept-Language</span><span class="sxs-lookup"><span data-stu-id="eff5a-379">Accept-Language</span></span>|<span data-ttu-id="eff5a-380">用于指定本地化名称的目标语言的头。</span><span class="sxs-lookup"><span data-stu-id="eff5a-380">Header to specify the desired language for localized names.</span></span> <span data-ttu-id="eff5a-381">例如，使用“en-US”表示英语，使用“es”表示西班牙语。</span><span class="sxs-lookup"><span data-stu-id="eff5a-381">For example, use "en-US" for English or "es" for Spanish.</span></span> <span data-ttu-id="eff5a-382">如果没有此头，返回的便是默认语言 (en-US)。</span><span class="sxs-lookup"><span data-stu-id="eff5a-382">The default language (en-US) will be returned if this header is not present.</span></span>|
|<span data-ttu-id="eff5a-383">**正文**</span><span class="sxs-lookup"><span data-stu-id="eff5a-383">**Body**</span></span>|<span data-ttu-id="eff5a-384">（空）</span><span class="sxs-lookup"><span data-stu-id="eff5a-384">(empty)</span></span>||
|<span data-ttu-id="eff5a-385">**响应**</span><span class="sxs-lookup"><span data-stu-id="eff5a-385">**Response**</span></span>|<span data-ttu-id="eff5a-386">JSON 数组</span><span class="sxs-lookup"><span data-stu-id="eff5a-386">JSON array</span></span>|<span data-ttu-id="eff5a-387">可用内容由包含以下属性的 JSON 对象表示：</span><span class="sxs-lookup"><span data-stu-id="eff5a-387">The available content will be represented by JSON objects with the following properties:</span></span><ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span data-ttu-id="eff5a-388"><b>id</b>：指明敏感信息类型的 GUID。</span><span class="sxs-lookup"><span data-stu-id="eff5a-388"><b>id</b>: Indicates the guid of the sensitive information type.</span></span></p></li><li><p><span data-ttu-id="eff5a-389"><b>name</b>：指明敏感信息类型的易记名称。</span><span class="sxs-lookup"><span data-stu-id="eff5a-389"><b>name</b>: The friendly name of the sensitive information type.</span></span></p></li></ul>|

#### <a name="sample-request"></a><span data-ttu-id="eff5a-390">示例请求</span><span class="sxs-lookup"><span data-stu-id="eff5a-390">Sample request</span></span>

```json
GET {root}/resources/dlpSensitiveTypes?PublisherIdentifier=46b472a7-c68e-4adf-8ade-3db49497518e
Authorization: Bearer eyJ0e...Qa6wg
Accept-Language: {language code} 

```

#### <a name="sample-response"></a><span data-ttu-id="eff5a-391">示例响应</span><span class="sxs-lookup"><span data-stu-id="eff5a-391">Sample response</span></span>

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

## <a name="api-throttling"></a><span data-ttu-id="eff5a-392">API 限制</span><span class="sxs-lookup"><span data-stu-id="eff5a-392">API throttling</span></span>

<span data-ttu-id="eff5a-393">通过 Office 365 管理活动 API 访问审核日志的组织，会因发布者级别限制而受限。</span><span class="sxs-lookup"><span data-stu-id="eff5a-393">Organizations that access auditing logs through the Office 365 Management Activity API were restricted by throttling limits at the publisher level.</span></span> <span data-ttu-id="eff5a-394">这意味着，对于代表多个客户请求数据的发布者而言，限制由所有这些客户共享。</span><span class="sxs-lookup"><span data-stu-id="eff5a-394">This means that for a publisher pulling data on behalf of multiple customers, the limit was shared by all those customers.</span></span>

<span data-ttu-id="eff5a-395">我们正在从发布者级别限制迁移至租户级别限制。</span><span class="sxs-lookup"><span data-stu-id="eff5a-395">We're moving from a publisher-level limit to a tenant-level limit.</span></span> <span data-ttu-id="eff5a-396">结果是每个组织都会获得自己完全分配的带宽配额，以访问其审核数据。</span><span class="sxs-lookup"><span data-stu-id="eff5a-396">The result is that each organization will get their own fully allocated bandwidth quota to access their auditing data.</span></span> <span data-ttu-id="eff5a-397">所有组织最初每分钟分配 2000 个请求基线。</span><span class="sxs-lookup"><span data-stu-id="eff5a-397">All organizations are initially allocated a baseline of 2,000 requests per minute.</span></span> <span data-ttu-id="eff5a-398">这不是一个静态的预定义的限制，但根据组合因素进行建模，包括组织中的席位数， Office 365 和 Microsoft 365 E5 组织的带宽大约为非 E5 组织的两倍。</span><span class="sxs-lookup"><span data-stu-id="eff5a-398">This is not a static, predefined limit but is modeled on a combination of factors including the number of seats in the organization and that Office 365 and Microsoft 365 E5 organizations will get approximately twice as much bandwidth as non-E5 organizations.</span></span> <span data-ttu-id="eff5a-399">这也是最大宽带的上限，以保护服务的健康。</span><span class="sxs-lookup"><span data-stu-id="eff5a-399">There will also be cap on the maximum bandwidth to protect the health of the service.</span></span>

<span data-ttu-id="eff5a-400">有关详细信息，参见 “[Microsoft 365 高级审核](https://docs.microsoft.com/microsoft-365/compliance/advanced-audit#high-bandwidth-access-to-the-office-365-management-activity-api)”中的“Office 365 管理活动 API”。</span><span class="sxs-lookup"><span data-stu-id="eff5a-400">For more information, see the "High-bandwidth access to the Office 365 Management Activity API" section in [Advanced audit in Microsoft 365](https://docs.microsoft.com/microsoft-365/compliance/advanced-audit#high-bandwidth-access-to-the-office-365-management-activity-api).</span></span>

> [!NOTE] 
> <span data-ttu-id="eff5a-401">尽管每个租户最初每分钟最多可提交 2000 个请求，Microsoft 也无法保证响应速率。</span><span class="sxs-lookup"><span data-stu-id="eff5a-401">Even though each tenant can initially submit up to 2,000 requests per minute, Microsoft cannot guarantee a response rate.</span></span> <span data-ttu-id="eff5a-402">响应速率取决于各种因素，如客户端系统性能、网络容量和网络速度。</span><span class="sxs-lookup"><span data-stu-id="eff5a-402">The response rate depends on various factors, such as client system performance, network capacity, and network speed.</span></span> 

## <a name="errors"></a><span data-ttu-id="eff5a-403">错误</span><span class="sxs-lookup"><span data-stu-id="eff5a-403">Errors</span></span>

<span data-ttu-id="eff5a-404">如果遇到错误，服务会使用标准 HTTP 错误代码语法，向调用方报告错误响应代码。</span><span class="sxs-lookup"><span data-stu-id="eff5a-404">When the service encounters an error, it will report the error response code to the caller, using standard HTTP error-code syntax.</span></span> <span data-ttu-id="eff5a-405">.</span><span class="sxs-lookup"><span data-stu-id="eff5a-405">.</span></span> <span data-ttu-id="eff5a-406">失败调用的正文中包含其他信息（作为一个 JSON 对象）。</span><span class="sxs-lookup"><span data-stu-id="eff5a-406">Additional information is included in the body of the failed call as a single JSON object.</span></span> <span data-ttu-id="eff5a-407">下面的示例展示了完整的 JSON 错误正文：</span><span class="sxs-lookup"><span data-stu-id="eff5a-407">An example of a full JSON error body is shown below:</span></span> 

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
|<span data-ttu-id="eff5a-408">代码</span><span class="sxs-lookup"><span data-stu-id="eff5a-408">Code</span></span>|<span data-ttu-id="eff5a-409">消息</span><span class="sxs-lookup"><span data-stu-id="eff5a-409">Message</span></span>|
|<span data-ttu-id="eff5a-410">AF10001</span><span class="sxs-lookup"><span data-stu-id="eff5a-410">AF10001</span></span>|<span data-ttu-id="eff5a-411">在请求中发送的权限集 ({0}) 不含所需的权限 **ActivityFeed.Read**。</span><span class="sxs-lookup"><span data-stu-id="eff5a-411">The permission set ({0}) sent in the request did not include the expected permission **ActivityFeed.Read**.</span></span><ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span data-ttu-id="eff5a-412">{0} = 访问令牌中的权限集。</span><span class="sxs-lookup"><span data-stu-id="eff5a-412">{0} = the permission set in the access token.</span></span></p></li></ul>|
|<span data-ttu-id="eff5a-413">AF20001</span><span class="sxs-lookup"><span data-stu-id="eff5a-413">AF20001</span></span>|<span data-ttu-id="eff5a-414">缺少参数 {0}。</span><span class="sxs-lookup"><span data-stu-id="eff5a-414">Missing parameter: {0}.</span></span> <ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span data-ttu-id="eff5a-415">{0} = 缺少的参数的名称。</span><span class="sxs-lookup"><span data-stu-id="eff5a-415">{0} = the name of the missing parameter.</span></span></p></li></ul>|
|<span data-ttu-id="eff5a-416">AF20002</span><span class="sxs-lookup"><span data-stu-id="eff5a-416">AF20002</span></span>|<span data-ttu-id="eff5a-417">参数类型 {0} 无效。</span><span class="sxs-lookup"><span data-stu-id="eff5a-417">Invalid parameter type: {0}.</span></span> <span data-ttu-id="eff5a-418">类型应为 {1}</span><span class="sxs-lookup"><span data-stu-id="eff5a-418">Expected type: {1}</span></span><ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span data-ttu-id="eff5a-419">{0} = 无效参数的名称。</span><span class="sxs-lookup"><span data-stu-id="eff5a-419">{0} = the name of the invalid parameter.</span></span></p></li><li><p><span data-ttu-id="eff5a-420">{1} = 应采用的类型（int、datetime、guid）</span><span class="sxs-lookup"><span data-stu-id="eff5a-420">{1} = the expected type (int, datetime, guid).</span></span></p></li></ul>|
|<span data-ttu-id="eff5a-421">AF20003</span><span class="sxs-lookup"><span data-stu-id="eff5a-421">AF20003</span></span>|<span data-ttu-id="eff5a-422">提供的到期时间 {0} 设置为过去的日期和时间。</span><span class="sxs-lookup"><span data-stu-id="eff5a-422">Expiration {0} provided is set to past date and time.</span></span><ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span data-ttu-id="eff5a-423">{0} = 在 API 调用中传递的到期时间。</span><span class="sxs-lookup"><span data-stu-id="eff5a-423">{0} = the expiration passed in the API call.</span></span></p></li></ul>|
|<span data-ttu-id="eff5a-424">AF20010</span><span class="sxs-lookup"><span data-stu-id="eff5a-424">AF20010</span></span>|<span data-ttu-id="eff5a-425">在 URL 中传递的租户 ID ({0}) 与访问令牌中传递的租户 ID ({1}) 不一致。</span><span class="sxs-lookup"><span data-stu-id="eff5a-425">The tenant ID passed in the URL ({0}) does not match the tenant ID passed in the access token ({1}).</span></span><ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span data-ttu-id="eff5a-426">{0} = 在 URL 中传递的租户 ID</span><span class="sxs-lookup"><span data-stu-id="eff5a-426">{0} = tenant ID passed in the URL</span></span></p></li><li><p><span data-ttu-id="eff5a-427">{1} = 在访问令牌中传递的租户 ID</span><span class="sxs-lookup"><span data-stu-id="eff5a-427">{1} = tenant ID passed in the access token</span></span></p></li></ul>|
|<span data-ttu-id="eff5a-428">AF20011</span><span class="sxs-lookup"><span data-stu-id="eff5a-428">AF20011</span></span>|<span data-ttu-id="eff5a-429">指定的租户 ID ({0}) 在系统中不存在或已遭删除。</span><span class="sxs-lookup"><span data-stu-id="eff5a-429">Specified tenant ID ({0}) does not exist in the system or has been deleted.</span></span> <ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>   <span data-ttu-id="eff5a-430">{0} = 在 URL 中传递的租户 ID</span><span class="sxs-lookup"><span data-stu-id="eff5a-430">{0} = tenant ID passed in the URL</span></span></p></li></ul>|
|<span data-ttu-id="eff5a-431">AF20012</span><span class="sxs-lookup"><span data-stu-id="eff5a-431">AF20012</span></span>|<span data-ttu-id="eff5a-432">指定的租户 ID ({0}) 未在系统中正确配置。</span><span class="sxs-lookup"><span data-stu-id="eff5a-432">Specified tenant ID ({0}) is incorrectly configured in the system.</span></span> <ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p>    <span data-ttu-id="eff5a-433">{0} = 在 URL 中传递的租户 ID</span><span class="sxs-lookup"><span data-stu-id="eff5a-433">{0} = tenant ID passed in the URL</span></span></p></li></ul>|
|<span data-ttu-id="eff5a-434">AF20013</span><span class="sxs-lookup"><span data-stu-id="eff5a-434">AF20013</span></span>|<span data-ttu-id="eff5a-435">在 URL 中传递的租户 ID ({0}) 不是有效的 GUID。</span><span class="sxs-lookup"><span data-stu-id="eff5a-435">The tenant ID passed in the URL ({0}) is not a valid GUID.</span></span><ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p> <span data-ttu-id="eff5a-436">{0} = 在 URL 中传递的租户 ID</span><span class="sxs-lookup"><span data-stu-id="eff5a-436">{0} = tenant ID passed in the URL</span></span></p></li></ul>|
|<span data-ttu-id="eff5a-437">AF20020</span><span class="sxs-lookup"><span data-stu-id="eff5a-437">AF20020</span></span>|<span data-ttu-id="eff5a-438">指定的内容类型无效。</span><span class="sxs-lookup"><span data-stu-id="eff5a-438">The specified content type is not valid.</span></span>|
|<span data-ttu-id="eff5a-439">AF20021</span><span class="sxs-lookup"><span data-stu-id="eff5a-439">AF20021</span></span>|<span data-ttu-id="eff5a-440">无法验证 Webhook 终结点{{0})。</span><span class="sxs-lookup"><span data-stu-id="eff5a-440">The webhook endpoint {{0}) could not be validated.</span></span> <span data-ttu-id="eff5a-441">{1}</span><span class="sxs-lookup"><span data-stu-id="eff5a-441">{1}</span></span><ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span data-ttu-id="eff5a-442">{0} = Webhook 地址。</span><span class="sxs-lookup"><span data-stu-id="eff5a-442">{0} = webhook address.</span></span></p></li><li><p><span data-ttu-id="eff5a-443">{1} =“终结点未返回 HTTP 200”</span><span class="sxs-lookup"><span data-stu-id="eff5a-443">{1} = "The endpoint did not return HTTP 200."</span></span> <span data-ttu-id="eff5a-444">或“地址必须以 HTTPS 开头”。</span><span class="sxs-lookup"><span data-stu-id="eff5a-444">or "The address must begin with HTTPS."</span></span></p></li></ul>|
|<span data-ttu-id="eff5a-445">AF20022</span><span class="sxs-lookup"><span data-stu-id="eff5a-445">AF20022</span></span>|<span data-ttu-id="eff5a-446">找不到对指定内容类型的订阅。</span><span class="sxs-lookup"><span data-stu-id="eff5a-446">No subscription found for the specified content type.</span></span>|
|<span data-ttu-id="eff5a-447">AF20023</span><span class="sxs-lookup"><span data-stu-id="eff5a-447">AF20023</span></span>|<span data-ttu-id="eff5a-448">{0} 已禁用订阅。</span><span class="sxs-lookup"><span data-stu-id="eff5a-448">The subscription was disabled by {0}.</span></span><ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span data-ttu-id="eff5a-449">{0} =“租户管理员”或“服务管理员”</span><span class="sxs-lookup"><span data-stu-id="eff5a-449">{0} = "a tenant admin" or "a service admin"</span></span></p></li></ul>|
|<span data-ttu-id="eff5a-450">AF20030</span><span class="sxs-lookup"><span data-stu-id="eff5a-450">AF20030</span></span>|<span data-ttu-id="eff5a-451">必须同时指定（或同时省略）开始时间和结束时间，两者间隔不得超过 24 小时，且开始时间与当前时间的间隔不得超过 7 天。</span><span class="sxs-lookup"><span data-stu-id="eff5a-451">Start time and end time must both be specified (or both omitted) and must be less than or equal to 24 hours apart, with the start time no more than 7 days in the past.</span></span>|
|<span data-ttu-id="eff5a-452">AF20031</span><span class="sxs-lookup"><span data-stu-id="eff5a-452">AF20031</span></span>|<span data-ttu-id="eff5a-453">nextPage 输入 {0} 无效</span><span class="sxs-lookup"><span data-stu-id="eff5a-453">Invalid nextPage Input: {0}.</span></span><ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span data-ttu-id="eff5a-454">{0} = 在 URL 中传递的下页指示符</span><span class="sxs-lookup"><span data-stu-id="eff5a-454">{0} = the next page indicator passed in the URL</span></span></p></li></ul>|
|<span data-ttu-id="eff5a-455">AF20050</span><span class="sxs-lookup"><span data-stu-id="eff5a-455">AF20050</span></span>|<span data-ttu-id="eff5a-456">指定的内容 ({0}) 不存在。</span><span class="sxs-lookup"><span data-stu-id="eff5a-456">The specified content ({0}) does not exist.</span></span><ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span data-ttu-id="eff5a-457">{0} = 资源 ID 或资源 URL</span><span class="sxs-lookup"><span data-stu-id="eff5a-457">{0} = resource id or resource URL</span></span></p></li></ul>|
|<span data-ttu-id="eff5a-458">AF20051</span><span class="sxs-lookup"><span data-stu-id="eff5a-458">AF20051</span></span>|<span data-ttu-id="eff5a-459">使用密钥 {0} 请求获取的内容已到期。</span><span class="sxs-lookup"><span data-stu-id="eff5a-459">Content requested with the key {0} has already expired.</span></span> <span data-ttu-id="eff5a-460">无法检索 7 天前的内容。</span><span class="sxs-lookup"><span data-stu-id="eff5a-460">Content older than 7 days cannot be retrieved.</span></span><ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span data-ttu-id="eff5a-461">{0} = 资源 ID 或资源 URL</span><span class="sxs-lookup"><span data-stu-id="eff5a-461">•    {0} = resource id or resource URL</span></span></p></li></ul>|
|<span data-ttu-id="eff5a-462">AF20052</span><span class="sxs-lookup"><span data-stu-id="eff5a-462">AF20052</span></span>|<span data-ttu-id="eff5a-463">URL 中的内容 ID {0} 无效。</span><span class="sxs-lookup"><span data-stu-id="eff5a-463">Content ID {0} in the URL is invalid.</span></span><ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span data-ttu-id="eff5a-464">{0} = 资源 ID 或资源 URL</span><span class="sxs-lookup"><span data-stu-id="eff5a-464">{0} = resource id or resource URL</span></span></p></li></ul>|
|<span data-ttu-id="eff5a-465">AF20053</span><span class="sxs-lookup"><span data-stu-id="eff5a-465">AF20053</span></span>|<span data-ttu-id="eff5a-466">Accept-Language 头中只能显示一种语言。</span><span class="sxs-lookup"><span data-stu-id="eff5a-466">Only one language may be present in the Accept-Language header.</span></span>|
|<span data-ttu-id="eff5a-467">AF20054</span><span class="sxs-lookup"><span data-stu-id="eff5a-467">AF20054</span></span>|<span data-ttu-id="eff5a-468">Accept-Language 头中的语法无效。</span><span class="sxs-lookup"><span data-stu-id="eff5a-468">Invalid syntax in Accept-Language header.</span></span>|
|<span data-ttu-id="eff5a-469">AF429</span><span class="sxs-lookup"><span data-stu-id="eff5a-469">AF429</span></span>|<span data-ttu-id="eff5a-470">请求次数过多。</span><span class="sxs-lookup"><span data-stu-id="eff5a-470">Too many requests.</span></span> <span data-ttu-id="eff5a-471">方法 = {0}，PublisherId = {1}</span><span class="sxs-lookup"><span data-stu-id="eff5a-471">Method={0}, PublisherId={1}</span></span><ul xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:mtps="http://msdn2.microsoft.com/mtps" xmlns:mshelp="http://msdn.microsoft.com/mshelp" xmlns:ddue="http://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:msxsl="urn:schemas-microsoft-com:xslt"><li><p><span data-ttu-id="eff5a-472">{0} = HTTP 方法</span><span class="sxs-lookup"><span data-stu-id="eff5a-472">{0} = HTTP Method</span></span></p></li><li><p><span data-ttu-id="eff5a-473">{1} = 用作 PublisherIdentifier 的租户 GUID</span><span class="sxs-lookup"><span data-stu-id="eff5a-473">{1} = Tenant GUID used as PublisherIdentifier</span></span></p></li></ul>|
|<span data-ttu-id="eff5a-474">AF50000</span><span class="sxs-lookup"><span data-stu-id="eff5a-474">AF50000</span></span>|<span data-ttu-id="eff5a-475">发生了内部错误。</span><span class="sxs-lookup"><span data-stu-id="eff5a-475">An internal error occurred.</span></span> <span data-ttu-id="eff5a-476">请重试请求。</span><span class="sxs-lookup"><span data-stu-id="eff5a-476">Retry the request.</span></span>|
