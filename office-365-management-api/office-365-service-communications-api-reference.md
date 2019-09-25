---
ms.TocTitle: Office 365 Service Communications API reference
title: Office 365 服务通信 API 参考
description: 此 API 可用于访问以下数据：“获取服务”、“获取当前状态”、“获取历史状态”和“获取消息”。
ms.ContentId: d0b9341a-b205-5442-1c20-8fb56407351d
ms.topic: reference (API)
ms.date: ''
localization_priority: Priority
ms.openlocfilehash: 427c741f623f46962c3cd6818adbb64bfc0f1e7a
ms.sourcegitcommit: 0e6d71ad2375e5d1fc72d6893724511c08840c27
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2019
ms.locfileid: "37131422"
---
# <a name="office-365-service-communications-api-reference"></a><span data-ttu-id="f25c6-103">Office 365 服务通信 API 参考</span><span class="sxs-lookup"><span data-stu-id="f25c6-103">Office 365 Service Communications API reference (preview)</span></span>

<span data-ttu-id="f25c6-104">Office 365 服务通信 API V2 可用于访问以下数据：</span><span class="sxs-lookup"><span data-stu-id="f25c6-104">You can use the Office 365 Service Communications API V2 to access the following data:</span></span>

- <span data-ttu-id="f25c6-105">**获取服务**：获取已订阅服务的列表。</span><span class="sxs-lookup"><span data-stu-id="f25c6-105">**Get Services**: Get the list of subscribed services.</span></span>
    
- <span data-ttu-id="f25c6-106">**获取当前状态**：获取当前正在进行的服务事件的实时视图。</span><span class="sxs-lookup"><span data-stu-id="f25c6-106">**Get Current Status**: Get a real-time view of current and ongoing service incidents and maintenance events</span></span>
    
- <span data-ttu-id="f25c6-107">**获取历史记录状态**：获取服务事件的历史视图。</span><span class="sxs-lookup"><span data-stu-id="f25c6-107">**Get Historical Status**: Get a historical view of service health, including service incidents and maintenance events.</span></span>
    
- <span data-ttu-id="f25c6-108">**获取消息**：查找事件和消息中心通信。</span><span class="sxs-lookup"><span data-stu-id="f25c6-108">**Get Messages**: Find Incident, Planned Maintenance, and Message Center communications.</span></span>
    
<span data-ttu-id="f25c6-109">目前，Office 365 服务通信 API 包含 Office 365、Yammer、Dynamics CRM 和 Microsoft Intune 云服务的数据。</span><span class="sxs-lookup"><span data-stu-id="f25c6-109">Currently, the Office 365 Service Communications API contains data for Office 365, Yammer, Dynamics CRM and Microsoft Intune cloud services.</span></span>

## <a name="the-fundamentals"></a><span data-ttu-id="f25c6-110">基础知识</span><span class="sxs-lookup"><span data-stu-id="f25c6-110">The fundamentals</span></span>

<span data-ttu-id="f25c6-111">此 API 的根 URL 包含将操作范围限定为一个租户的租户标识符：</span><span class="sxs-lookup"><span data-stu-id="f25c6-111">The root URL of the API includes a tenant identifier that scopes the operations to a single tenant:</span></span>

```http
https://manage.office.com/api/v1.0/{tenant_identifier}/ServiceComms/{operation}
```

<span data-ttu-id="f25c6-112">借助 **Office 365 服务通信 API** 这项 REST 服务，可开发使用任何 Web 语言和宿主环境（支持 HTTPS 和 X.509 证书）的解决方案。</span><span class="sxs-lookup"><span data-stu-id="f25c6-112">The **Office 365 Service Communications API** is a REST service that allows you to develop solutions using any web language and hosting environment that supports HTTPS and X.509 certificates.</span></span> <span data-ttu-id="f25c6-113">此 API 依赖 **Microsoft Azure Active Directory** 和 **OAuth2** 协议进行身份验证和授权。</span><span class="sxs-lookup"><span data-stu-id="f25c6-113">The API relies on **Microsoft Azure Active Directory** and the **OAuth2** protocol for authentication and authorization.</span></span> <span data-ttu-id="f25c6-114">若要在应用程序中访问此 API，必须先在 Azure AD 中注册应用程序，并为它配置适当范围的权限。</span><span class="sxs-lookup"><span data-stu-id="f25c6-114">To access the API from your application, you'll need to first register it in Azure AD and configure it with permissions at the appropriate scope.</span></span> <span data-ttu-id="f25c6-115">这样，应用程序便能请求获取调用此 API 所需的 OAuth2 访问令牌。</span><span class="sxs-lookup"><span data-stu-id="f25c6-115">This will enable your application to request OAuth2 access tokens necessary for calling the API.</span></span> <span data-ttu-id="f25c6-116">若要详细了解如何在 Azure AD 中注册和配置应用程序，请参阅 [Office 365 管理 API 入门](get-started-with-office-365-management-apis.md)。</span><span class="sxs-lookup"><span data-stu-id="f25c6-116">You can find more information about registering and configuring an application in Azure AD at [Office 365 Management APIs getting started](get-started-with-office-365-management-apis.md).</span></span>

<span data-ttu-id="f25c6-117">所有 API 请求都要求，授权 HTTP 头中必须有从 Azure AD 中获取的包含 **ServiceHealth.Read** 声明的有效 OAuth2 JWT 访问令牌；且租户标识符必须与根 URL 中的租户标识符一致。</span><span class="sxs-lookup"><span data-stu-id="f25c6-117">All API requests require an Authorization HTTP header that has a valid OAuth2 JWT access token obtained from Azure AD that contains the **ServiceHealth.Read** claim; and the tenant identifier must match the tenant identifier in the root URL.</span></span>

```json
Authorization: Bearer {OAuth2 token}
```

### <a name="request-headers"></a><span data-ttu-id="f25c6-118">请求标头</span><span class="sxs-lookup"><span data-stu-id="f25c6-118">Request headers</span></span>

<span data-ttu-id="f25c6-119">以下是所有 Office 365 服务通信 API 操作支持的请求头。</span><span class="sxs-lookup"><span data-stu-id="f25c6-119">These are the supported request headers for all Office 365 Service Communications API operations.</span></span>

|<span data-ttu-id="f25c6-120">头</span><span class="sxs-lookup"><span data-stu-id="f25c6-120">Header</span></span>|<span data-ttu-id="f25c6-121">说明</span><span class="sxs-lookup"><span data-stu-id="f25c6-121">Description</span></span>|
|:-----|:-----|
|<span data-ttu-id="f25c6-122">**Accept（可选）**</span><span class="sxs-lookup"><span data-stu-id="f25c6-122">**Accept (Optional)**</span></span>|<span data-ttu-id="f25c6-123">以下是可接受的响应表示形式：</span><span class="sxs-lookup"><span data-stu-id="f25c6-123">The following are acceptable representations for the response:</span></span><br/><span data-ttu-id="f25c6-124">**application/json;odata.metadata=full**</span><span class="sxs-lookup"><span data-stu-id="f25c6-124">**application/json;odata.metadata=full**</span></span><br/><span data-ttu-id="f25c6-125">**application/json;odata.metadata=minimal**</span><span class="sxs-lookup"><span data-stu-id="f25c6-125">**application/json;odata.metadata=minimal**</span></span><br/><span data-ttu-id="f25c6-126">[The default if header not specified] **application/json;odata.metadata=none**</span><span class="sxs-lookup"><span data-stu-id="f25c6-126">[The default if header not specified] **application/json;odata.metadata=none**</span></span>|
|<span data-ttu-id="f25c6-127">**Authorization（必需）**</span><span class="sxs-lookup"><span data-stu-id="f25c6-127">**Authorization (Required)**</span></span>|<span data-ttu-id="f25c6-128">请求的授权令牌（持有者 JWT Azure AD 令牌）。</span><span class="sxs-lookup"><span data-stu-id="f25c6-128">Authorization token (Bearer JWT Azure AD Token) for the request.</span></span>|


<br/>

### <a name="response-headers"></a><span data-ttu-id="f25c6-129">响应标头</span><span class="sxs-lookup"><span data-stu-id="f25c6-129">Response headers</span></span>

<span data-ttu-id="f25c6-130">以下是所有 Office 365 服务通信 API 操作返回的响应头：</span><span class="sxs-lookup"><span data-stu-id="f25c6-130">These are the response headers returned for all Office 365 Service Communications API operations:</span></span>

|<span data-ttu-id="f25c6-131">头</span><span class="sxs-lookup"><span data-stu-id="f25c6-131">Header</span></span>|<span data-ttu-id="f25c6-132">说明</span><span class="sxs-lookup"><span data-stu-id="f25c6-132">Description</span></span>|
|:-----|:-----|
|<span data-ttu-id="f25c6-133">**Content-Length**</span><span class="sxs-lookup"><span data-stu-id="f25c6-133">**Content-Length**</span></span>|<span data-ttu-id="f25c6-134">响应正文长度。</span><span class="sxs-lookup"><span data-stu-id="f25c6-134">The length of the response body.</span></span>|
|<span data-ttu-id="f25c6-135">**Content-Type**</span><span class="sxs-lookup"><span data-stu-id="f25c6-135">**Content-Type**</span></span>|<span data-ttu-id="f25c6-136">响应表示形式：</span><span class="sxs-lookup"><span data-stu-id="f25c6-136">Representation of the response:</span></span><br/><span data-ttu-id="f25c6-137">**application/json**</span><span class="sxs-lookup"><span data-stu-id="f25c6-137">**application/json**</span></span><br/><span data-ttu-id="f25c6-138">**application/json;odata.metadata=full**</span><span class="sxs-lookup"><span data-stu-id="f25c6-138">**application/json;odata.metadata=full**</span></span><br/><span data-ttu-id="f25c6-139">**application/json;odata.metadata=minimal**</span><span class="sxs-lookup"><span data-stu-id="f25c6-139">**application/json;odata.metadata=minimal**</span></span><br/><span data-ttu-id="f25c6-140">**application/json;odata.metadata=none**</span><span class="sxs-lookup"><span data-stu-id="f25c6-140">**application/json;odata.metadata=none**</span></span><br/><span data-ttu-id="f25c6-141">**odata.streaming=true**</span><span class="sxs-lookup"><span data-stu-id="f25c6-141">**odata.streaming=true**</span></span>|
|<span data-ttu-id="f25c6-142">**Cache-Control**</span><span class="sxs-lookup"><span data-stu-id="f25c6-142">**Cache-Control**</span></span>|<span data-ttu-id="f25c6-143">用于指定请求/响应链上的所有缓存机制都必须遵守的指令。</span><span class="sxs-lookup"><span data-stu-id="f25c6-143">Used to specify directives that all caching mechanisms along the request/response chain must obey.</span></span>|
|<span data-ttu-id="f25c6-144">**Pragma**</span><span class="sxs-lookup"><span data-stu-id="f25c6-144">**Pragma**</span></span>|<span data-ttu-id="f25c6-145">实现专属行为。</span><span class="sxs-lookup"><span data-stu-id="f25c6-145">Implementation-specific behaviors.</span></span>|
|<span data-ttu-id="f25c6-146">**Expires**</span><span class="sxs-lookup"><span data-stu-id="f25c6-146">**Expires**</span></span>|<span data-ttu-id="f25c6-147">客户端何时应让资源到期。</span><span class="sxs-lookup"><span data-stu-id="f25c6-147">When the client should make the resource expire.</span></span>|
|<span data-ttu-id="f25c6-148">**X-Activity-Id**</span><span class="sxs-lookup"><span data-stu-id="f25c6-148">**X-Activity-Id**</span></span>|<span data-ttu-id="f25c6-149">服务器生成的活动 ID。</span><span class="sxs-lookup"><span data-stu-id="f25c6-149">The server-generated activity Id.</span></span>|
|<span data-ttu-id="f25c6-150">**OData-Version**</span><span class="sxs-lookup"><span data-stu-id="f25c6-150">**OData-Version**</span></span>|<span data-ttu-id="f25c6-151">受支持的 OData 版本 (4.0)。</span><span class="sxs-lookup"><span data-stu-id="f25c6-151">The supported OData Version (4.0).</span></span>|
|<span data-ttu-id="f25c6-152">**Date**</span><span class="sxs-lookup"><span data-stu-id="f25c6-152">**Date**</span></span>|<span data-ttu-id="f25c6-153">服务器发送响应的日期 (UTC)。</span><span class="sxs-lookup"><span data-stu-id="f25c6-153">The date in UTC when the response was sent from the server.</span></span>|
|<span data-ttu-id="f25c6-154">**X-Time-Taken**</span><span class="sxs-lookup"><span data-stu-id="f25c6-154">**X-Time-Taken**</span></span>|<span data-ttu-id="f25c6-155">响应生成耗时（以毫秒为单位）。</span><span class="sxs-lookup"><span data-stu-id="f25c6-155">The time it took to generate the response (ms).</span></span>|
|<span data-ttu-id="f25c6-156">**X-Instance-Name**</span><span class="sxs-lookup"><span data-stu-id="f25c6-156">**X-Instance-Name**</span></span>|<span data-ttu-id="f25c6-157">用于生成响应的 Azure 实例的标识符（用于调试目的）。</span><span class="sxs-lookup"><span data-stu-id="f25c6-157">The identifier of the Azure instance used to generate the response (for debugging purposes).</span></span>|
|<span data-ttu-id="f25c6-158">**Server**</span><span class="sxs-lookup"><span data-stu-id="f25c6-158">**Server**</span></span>|<span data-ttu-id="f25c6-159">用于生成响应的服务器（用于调试目的）。</span><span class="sxs-lookup"><span data-stu-id="f25c6-159">The server used to generate the response (for debugging purposes).</span></span>|
|<span data-ttu-id="f25c6-160">**X-ASPNET-Version**</span><span class="sxs-lookup"><span data-stu-id="f25c6-160">**X-ASPNET-Version**</span></span>|<span data-ttu-id="f25c6-161">生成响应的服务器使用的 ASP.Net 版本（用于调试目的）。</span><span class="sxs-lookup"><span data-stu-id="f25c6-161">The version of ASP.Net used by the server that generated the response (for debugging purposes).</span></span>|
|<span data-ttu-id="f25c6-162">**X-Powered-By**</span><span class="sxs-lookup"><span data-stu-id="f25c6-162">**X-Powered-By**</span></span>|<span data-ttu-id="f25c6-163">生成响应的服务器使用的技术（用于调试目的）。</span><span class="sxs-lookup"><span data-stu-id="f25c6-163">The technologies used in the server that generated the response (for debugging purposes).</span></span>|
|||

<br/>

<span data-ttu-id="f25c6-164">下文介绍了 Office 365 服务通信 API 操作。</span><span class="sxs-lookup"><span data-stu-id="f25c6-164">Following are the Office 365 Service Communications API operations.</span></span>


## <a name="get-services"></a><span data-ttu-id="f25c6-165">获取服务</span><span class="sxs-lookup"><span data-stu-id="f25c6-165">Get Services</span></span>

<span data-ttu-id="f25c6-166">返回已订阅服务的列表。</span><span class="sxs-lookup"><span data-stu-id="f25c6-166">Returns the list of subscribed services.</span></span>

||<span data-ttu-id="f25c6-167">服务</span><span class="sxs-lookup"><span data-stu-id="f25c6-167">Service</span></span>|<span data-ttu-id="f25c6-168">说明</span><span class="sxs-lookup"><span data-stu-id="f25c6-168">Description</span></span>|
|:-----|:-----|:-----|
|<span data-ttu-id="f25c6-169">**路径**</span><span class="sxs-lookup"><span data-stu-id="f25c6-169">**Path**</span></span>| `/Services`||
|<span data-ttu-id="f25c6-170">**查询选项**</span><span class="sxs-lookup"><span data-stu-id="f25c6-170">**Query-option**</span></span>|<span data-ttu-id="f25c6-171">$select</span><span class="sxs-lookup"><span data-stu-id="f25c6-171">$select</span></span>|<span data-ttu-id="f25c6-172">选择一部分属性。</span><span class="sxs-lookup"><span data-stu-id="f25c6-172">Pick a subset of properties.</span></span>|
|<span data-ttu-id="f25c6-173">**响应**</span><span class="sxs-lookup"><span data-stu-id="f25c6-173">**Response**</span></span>|<span data-ttu-id="f25c6-174">“Service”实体列表</span><span class="sxs-lookup"><span data-stu-id="f25c6-174">List of "Service" entities</span></span>|<span data-ttu-id="f25c6-175">“Service”实体包含“Id”(String)、“DisplayName”(String) 和“FeatureNames”（String 列表）。</span><span class="sxs-lookup"><span data-stu-id="f25c6-175">"Service" entity contains "Id" (String), "DisplayName" (String), and "FeatureNames" (list of Strings).</span></span>|
||||

### <a name="sample-request"></a><span data-ttu-id="f25c6-176">示例请求</span><span class="sxs-lookup"><span data-stu-id="f25c6-176">Sample request</span></span>

```json
GET https://manage.office.com/api/v1.0/contoso.com/ServiceComms/Services 
Authorization: Bearer {AAD_Bearer_JWT_Token}

```

### <a name="sample-response"></a><span data-ttu-id="f25c6-177">示例响应</span><span class="sxs-lookup"><span data-stu-id="f25c6-177">Sample response</span></span>

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


## <a name="get-current-status"></a><span data-ttu-id="f25c6-178">获取当前状态</span><span class="sxs-lookup"><span data-stu-id="f25c6-178">Get Current Status</span></span>

<span data-ttu-id="f25c6-179">返回过去 24 小时的服务状态。</span><span class="sxs-lookup"><span data-stu-id="f25c6-179">Returns the status of the service over the previous 24 hours.</span></span>

> [!NOTE] 
> <span data-ttu-id="f25c6-180">服务响应将包含过去 24 小时内的状态和所有事件。</span><span class="sxs-lookup"><span data-stu-id="f25c6-180">The service response will contain the status and any incidents within the previous 24 hours.</span></span> <span data-ttu-id="f25c6-181">返回的 StatusDate 或 StatusTime 值将正好是过去 24 小时。</span><span class="sxs-lookup"><span data-stu-id="f25c6-181">The StatusDate or StatusTime value returned will be exactly 24 hours in the past.</span></span> <span data-ttu-id="f25c6-182">若要获取特定事件的最近一次更新信息, 请使用 "获取消息" 功能并从与事件 ID 匹配的响应记录中读取 LastUpdatedTime 值。</span><span class="sxs-lookup"><span data-stu-id="f25c6-182">To get the last update for a particular incident, use the Get Messages functionality and read the LastUpdatedTime value from the response record that matches your incident ID.</span></span> <br/>

<br/>

||<span data-ttu-id="f25c6-183">服务</span><span class="sxs-lookup"><span data-stu-id="f25c6-183">Service</span></span>|<span data-ttu-id="f25c6-184">说明</span><span class="sxs-lookup"><span data-stu-id="f25c6-184">Description</span></span>|
|:-----|:-----|:-----|
|<span data-ttu-id="f25c6-185">**路径**</span><span class="sxs-lookup"><span data-stu-id="f25c6-185">**Path**</span></span>| `/CurrentStatus`||
|<span data-ttu-id="f25c6-186">**筛选器**</span><span class="sxs-lookup"><span data-stu-id="f25c6-186">**Filter**</span></span>|<span data-ttu-id="f25c6-187">Workload</span><span class="sxs-lookup"><span data-stu-id="f25c6-187">Workload</span></span>|<span data-ttu-id="f25c6-188">按 Workload 筛选（String，默认值：all）。</span><span class="sxs-lookup"><span data-stu-id="f25c6-188">Filter by workload (String, default: all).</span></span>|
|<span data-ttu-id="f25c6-189">**查询选项**</span><span class="sxs-lookup"><span data-stu-id="f25c6-189">**Query-option**</span></span>|<span data-ttu-id="f25c6-190">$select</span><span class="sxs-lookup"><span data-stu-id="f25c6-190">$select</span></span>|<span data-ttu-id="f25c6-191">选择一部分属性。</span><span class="sxs-lookup"><span data-stu-id="f25c6-191">Pick a subset of properties.</span></span>|
|<span data-ttu-id="f25c6-192">**响应**</span><span class="sxs-lookup"><span data-stu-id="f25c6-192">**Response**</span></span>|<span data-ttu-id="f25c6-193">“WorkloadStatus”实体列表。</span><span class="sxs-lookup"><span data-stu-id="f25c6-193">List of "WorkloadStatus" entities.</span></span>|<span data-ttu-id="f25c6-194">“WorkloadStatus”实体包含“Id”(String)、“Workload”(String)、“StatusTime”(DateTimeOffset)、“WorkloadDisplayName”(String)、“Status”(String)、“IncidentIds”（String 列表）和“FeatureGroupStatusCollection”（“FeatureStatus”列表）。</span><span class="sxs-lookup"><span data-stu-id="f25c6-194">"WorkloadStatus" entity contains "Id" (String), "Workload" (String), "StatusTime"(DateTimeOffset), "WorkloadDisplayName" (String), "Status" (String), "IncidentIds" (list of Strings), and FeatureGroupStatusCollection (list of "FeatureStatus").</span></span><br/><br/><span data-ttu-id="f25c6-195">“FeatureStatus”实体包含“Feature”(String)、“FeatureGroupDisplayName”(String) 和“FeatureStatus”(String)。</span><span class="sxs-lookup"><span data-stu-id="f25c6-195">"FeatureStatus" entity contains "Feature" (String), "FeatureGroupDisplayName" (String), and "FeatureStatus" (String).</span></span>|
||||

### <a name="sample-request"></a><span data-ttu-id="f25c6-196">示例请求</span><span class="sxs-lookup"><span data-stu-id="f25c6-196">Sample request</span></span>

```json
GET https://manage.office.com/api/v1.0/contoso.com/ServiceComms/CurrentStatus
Authorization: Bearer {AAD_Bearer_JWT_Token}
```

### <a name="sample-response"></a><span data-ttu-id="f25c6-197">示例响应</span><span class="sxs-lookup"><span data-stu-id="f25c6-197">Sample response</span></span>

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

### <a name="status-definitions"></a><span data-ttu-id="f25c6-198">状态定义</span><span class="sxs-lookup"><span data-stu-id="f25c6-198">Status definitions</span></span>

<span data-ttu-id="f25c6-199">状态定义包括以下值:</span><span class="sxs-lookup"><span data-stu-id="f25c6-199">The status definitions include the following values:</span></span> 

- <span data-ttu-id="f25c6-200">正在调查</span><span class="sxs-lookup"><span data-stu-id="f25c6-200">Investigating</span></span>
- <span data-ttu-id="f25c6-201">ServiceDegradation</span><span class="sxs-lookup"><span data-stu-id="f25c6-201">ServiceDegradation</span></span>
- <span data-ttu-id="f25c6-202">ServiceInterruption</span><span class="sxs-lookup"><span data-stu-id="f25c6-202">ServiceInterruption</span></span> 
- <span data-ttu-id="f25c6-203">RestoringService</span><span class="sxs-lookup"><span data-stu-id="f25c6-203">RestoringService</span></span>
- <span data-ttu-id="f25c6-204">ExtendedRecovery</span><span class="sxs-lookup"><span data-stu-id="f25c6-204">ExtendedRecovery</span></span>
- <span data-ttu-id="f25c6-205">ServiceRestored</span><span class="sxs-lookup"><span data-stu-id="f25c6-205">ServiceRestored</span></span>
- <span data-ttu-id="f25c6-206">PostIncidentReportPublished</span><span class="sxs-lookup"><span data-stu-id="f25c6-206">PostIncidentReportPublished</span></span> 
- <span data-ttu-id="f25c6-207">VerifyingService</span><span class="sxs-lookup"><span data-stu-id="f25c6-207">VerifyingService</span></span>
- <span data-ttu-id="f25c6-208">ServiceOperational</span><span class="sxs-lookup"><span data-stu-id="f25c6-208">ServiceOperational</span></span>

<span data-ttu-id="f25c6-209">有关最新列表及这些状态定义的说明, 请参阅[如何检查 Office 365 服务运行状况](https://docs.microsoft.com/office365/enterprise/view-service-health#status-definitions)。</span><span class="sxs-lookup"><span data-stu-id="f25c6-209">For an up-to-date list and description of these status definitions, see [How to check Office 365 service health](https://docs.microsoft.com/office365/enterprise/view-service-health#status-definitions).</span></span>


## <a name="get-historical-status"></a><span data-ttu-id="f25c6-210">获取历史状态</span><span class="sxs-lookup"><span data-stu-id="f25c6-210">Get Historical Status</span></span>

<span data-ttu-id="f25c6-211">按天返回服务在特定时间范围内的历史状态。</span><span class="sxs-lookup"><span data-stu-id="f25c6-211">Returns the historical status of the service, by day, over a certain time range.</span></span>

||<span data-ttu-id="f25c6-212">服务</span><span class="sxs-lookup"><span data-stu-id="f25c6-212">Service</span></span>|<span data-ttu-id="f25c6-213">说明</span><span class="sxs-lookup"><span data-stu-id="f25c6-213">Description</span></span>|
|:-----|:-----|:-----|
|<span data-ttu-id="f25c6-214">**路径**</span><span class="sxs-lookup"><span data-stu-id="f25c6-214">**Path**</span></span>| `/HistoricalStatus`||
|<span data-ttu-id="f25c6-215">**筛选器**</span><span class="sxs-lookup"><span data-stu-id="f25c6-215">**Filters**</span></span>|<span data-ttu-id="f25c6-216">Workload</span><span class="sxs-lookup"><span data-stu-id="f25c6-216">Workload</span></span>|<span data-ttu-id="f25c6-217">按 Workload 筛选（String，默认值：all）。</span><span class="sxs-lookup"><span data-stu-id="f25c6-217">Filter by workload (String, default: all).</span></span>|
||<span data-ttu-id="f25c6-218">StatusTime</span><span class="sxs-lookup"><span data-stu-id="f25c6-218">StatusTime</span></span>|<span data-ttu-id="f25c6-219">按晚于 StatusTime 的天数进行筛选（DateTimeOffset，默认值：ge CurrentTime - 7 days）。</span><span class="sxs-lookup"><span data-stu-id="f25c6-219">Filter by days greater than StatusTime (DateTimeOffset, default: ge CurrentTime - 7 days).</span></span>|
|<span data-ttu-id="f25c6-220">**查询选项**</span><span class="sxs-lookup"><span data-stu-id="f25c6-220">**Query-option**</span></span>|<span data-ttu-id="f25c6-221">$select</span><span class="sxs-lookup"><span data-stu-id="f25c6-221">$select</span></span>|<span data-ttu-id="f25c6-222">选择一部分属性。</span><span class="sxs-lookup"><span data-stu-id="f25c6-222">Pick a subset of properties.</span></span>|
|<span data-ttu-id="f25c6-223">**响应**</span><span class="sxs-lookup"><span data-stu-id="f25c6-223">**Response**</span></span>|<span data-ttu-id="f25c6-224">“WorkloadStatus”实体列表。</span><span class="sxs-lookup"><span data-stu-id="f25c6-224">List of "WorkloadStatus" entities.</span></span>|<span data-ttu-id="f25c6-225">“WorkloadStatus”实体包含“Id”(String)、“Workload”(String)、“StatusTime”(DateTimeOffset)、“WorkloadDisplayName”(String)、“Status”(String)、“IncidentIds”（String 列表）和“FeatureGroupStatusCollection”（“FeatureStatus”列表）。</span><span class="sxs-lookup"><span data-stu-id="f25c6-225">"WorkloadStatus" entity contains "Id" (String), "Workload" (String), "StatusTime"(DateTimeOffset), "WorkloadDisplayName" (String), "Status" (String), "IncidentIds" (list of Strings), and FeatureGroupStatusCollection (list of "FeatureStatus").</span></span><br/><br/><span data-ttu-id="f25c6-226">“FeatureStatus”实体包含“Feature”(String)、“FeatureGroupDisplayName”(String) 和“FeatureStatus”(String)。</span><span class="sxs-lookup"><span data-stu-id="f25c6-226">"FeatureStatus" entity contains "Feature" (String), "FeatureGroupDisplayName" (String), and "FeatureStatus" (String).</span></span>|
||||

### <a name="sample-request"></a><span data-ttu-id="f25c6-227">示例请求</span><span class="sxs-lookup"><span data-stu-id="f25c6-227">Sample request</span></span>

```json
GET https://manage.office.com/api/v1.0/contoso.com/ServiceComms/HistoricalStatus
Authorization: Bearer {AAD_Bearer_JWT_Token}
```

### <a name="sample-response"></a><span data-ttu-id="f25c6-228">示例响应</span><span class="sxs-lookup"><span data-stu-id="f25c6-228">Sample response</span></span>

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

## <a name="get-messages"></a><span data-ttu-id="f25c6-229">获取消息</span><span class="sxs-lookup"><span data-stu-id="f25c6-229">Get Messages</span></span>

<span data-ttu-id="f25c6-230">返回特定时间范围内关于服务的消息。</span><span class="sxs-lookup"><span data-stu-id="f25c6-230">Returns the messages about the service over a certain time range.</span></span> <span data-ttu-id="f25c6-231">使用类型筛选器，以筛选出“服务事件”、“计划内维护”或“消息中心”消息。</span><span class="sxs-lookup"><span data-stu-id="f25c6-231">Use the type filter to filter for "Service Incident", "Planned Maintenance", or "Message Center" messages.</span></span>

||<span data-ttu-id="f25c6-232">服务</span><span class="sxs-lookup"><span data-stu-id="f25c6-232">Service</span></span>|<span data-ttu-id="f25c6-233">说明</span><span class="sxs-lookup"><span data-stu-id="f25c6-233">Description</span></span>|
|:-----|:-----|:-----|
|<span data-ttu-id="f25c6-234">**路径**</span><span class="sxs-lookup"><span data-stu-id="f25c6-234">**Path**</span></span>| `/Messages`||
|<span data-ttu-id="f25c6-235">**筛选器**</span><span class="sxs-lookup"><span data-stu-id="f25c6-235">**Filters**</span></span>|<span data-ttu-id="f25c6-236">Workload</span><span class="sxs-lookup"><span data-stu-id="f25c6-236">Workload</span></span>|<span data-ttu-id="f25c6-237">按 Workload 筛选（String，默认值：all）。</span><span class="sxs-lookup"><span data-stu-id="f25c6-237">Filter by workload (String, default: all).</span></span>|
||<span data-ttu-id="f25c6-238">StartTime</span><span class="sxs-lookup"><span data-stu-id="f25c6-238">StartTime</span></span>|<span data-ttu-id="f25c6-239">按 StartTime 筛选（DateTimeOffset，默认值：ge CurrentTime - 7 days）。</span><span class="sxs-lookup"><span data-stu-id="f25c6-239">Filter by Start Time (DateTimeOffset, default: ge CurrentTime - 7 days).</span></span>|
||<span data-ttu-id="f25c6-240">EndTime</span><span class="sxs-lookup"><span data-stu-id="f25c6-240">EndTime</span></span>|<span data-ttu-id="f25c6-241">按 EndTime 筛选（DateTimeOffset，默认值：le CurrentTime）。</span><span class="sxs-lookup"><span data-stu-id="f25c6-241">Filter by End Time (DateTimeOffset, default: le CurrentTime).</span></span>|
||<span data-ttu-id="f25c6-242">MessageType</span><span class="sxs-lookup"><span data-stu-id="f25c6-242">MessageType</span></span>|<span data-ttu-id="f25c6-243">按 MessageType 筛选（String，默认值：all）。</span><span class="sxs-lookup"><span data-stu-id="f25c6-243">Filter by MessageType (String, default: all).</span></span>|
||<span data-ttu-id="f25c6-244">ID</span><span class="sxs-lookup"><span data-stu-id="f25c6-244">ID</span></span>|<span data-ttu-id="f25c6-245">按 ID 筛选（String，默认值：all）。</span><span class="sxs-lookup"><span data-stu-id="f25c6-245">Filter by ID (String, default: all).</span></span>|
|<span data-ttu-id="f25c6-246">**查询选项**</span><span class="sxs-lookup"><span data-stu-id="f25c6-246">**Query-option**</span></span>|<span data-ttu-id="f25c6-247">$select</span><span class="sxs-lookup"><span data-stu-id="f25c6-247">$select</span></span>|<span data-ttu-id="f25c6-248">选择一部分属性。</span><span class="sxs-lookup"><span data-stu-id="f25c6-248">Pick a subset of properties.</span></span>|
||<span data-ttu-id="f25c6-249">$top</span><span class="sxs-lookup"><span data-stu-id="f25c6-249">$top</span></span>|<span data-ttu-id="f25c6-250">选择前多少个结果（默认值和最大值：$top = 100）。</span><span class="sxs-lookup"><span data-stu-id="f25c6-250">Pick the top number of results (default and max $top=100).</span></span>|
||<span data-ttu-id="f25c6-251">$skip</span><span class="sxs-lookup"><span data-stu-id="f25c6-251">$skip</span></span>|<span data-ttu-id="f25c6-252">跳过的结果数（默认值：$skip = 0）。</span><span class="sxs-lookup"><span data-stu-id="f25c6-252">Skip number of results (default: $skip=0).</span></span>|
|<span data-ttu-id="f25c6-253">**响应**</span><span class="sxs-lookup"><span data-stu-id="f25c6-253">**Response**</span></span>|<span data-ttu-id="f25c6-254">“Message”实体列表。</span><span class="sxs-lookup"><span data-stu-id="f25c6-254">List of "Message" entities.</span></span>|<span data-ttu-id="f25c6-255">“Message”实体包含“Id”(String)、“StartTime”(DateTimeOffset)、“EndTime”(DateTimeOffset)、“Status”(String)、“Messages”（“MessageHistory”实体列表）、“LastUpdatedTime”(DateTimeOffset)、“Workload”(String)、“WorkloadDisplayName”(String)、“Feature”(String)、“FeatureDisplayName”(String) 和“MessageType”（Enum，默认值：all）。</span><span class="sxs-lookup"><span data-stu-id="f25c6-255">"Message" entity contains "Id" (String), "StartTime" (DateTimeOffset), "EndTime" (DateTimeOffset), "Status" (String), "Messages" (list of "MessageHistory" entity), "LastUpdatedTime" (DateTimeOffset), "Workload" (String), "WorkloadDisplayName" (String), "Feature" (String), "FeatureDisplayName" (String), "MessageType" (Enum, default: all).</span></span><br/><br/><span data-ttu-id="f25c6-256">“MessageHistory”实体包含“PublishedTime”(DateTimeOffset) 和“MessageText”(String)。</span><span class="sxs-lookup"><span data-stu-id="f25c6-256">"MessageHistory" entity contains "PublishedTime" (DateTimeOffset), "MessageText" (String).</span></span>|
||||

### <a name="sample-request"></a><span data-ttu-id="f25c6-257">示例请求</span><span class="sxs-lookup"><span data-stu-id="f25c6-257">Sample request</span></span>

```json
GET https://manage.office.com/api/v1.0/contoso.com/ServiceComms/Messages
Authorization: Bearer {AAD_Bearer_JWT_Token}
```

### <a name="sample-response"></a><span data-ttu-id="f25c6-258">示例响应</span><span class="sxs-lookup"><span data-stu-id="f25c6-258">Sample response</span></span>

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


## <a name="errors"></a><span data-ttu-id="f25c6-259">错误</span><span class="sxs-lookup"><span data-stu-id="f25c6-259">Errors</span></span>

<span data-ttu-id="f25c6-260">如果遇到错误，服务会使用标准 HTTP 错误代码语法，向调用方报告错误响应代码。</span><span class="sxs-lookup"><span data-stu-id="f25c6-260">When the service encounters an error, it reports the error response code to the caller, using standard HTTP error-code syntax.</span></span> <span data-ttu-id="f25c6-261">根据 [OData V4 规范](http://docs.oasis-open.org/odata/odata/v4.0/odata-v4.0-part1-protocol.html)，失败调用的正文中包含其他信息（作为一个 JSON 对象）。</span><span class="sxs-lookup"><span data-stu-id="f25c6-261">As per [OData V4 specification](http://docs.oasis-open.org/odata/odata/v4.0/odata-v4.0-part1-protocol.html), additional information is included in the body of the failed call as a single JSON object.</span></span> <span data-ttu-id="f25c6-262">下面的示例展示了完整的 JSON 错误正文：</span><span class="sxs-lookup"><span data-stu-id="f25c6-262">The following is an example of a full JSON error body:</span></span>


```json
{ 
    "error":{ 
        "code":"AF5000.  An internal server error occurred.",
        "message": "Retry the request." 
    } 
}
```

