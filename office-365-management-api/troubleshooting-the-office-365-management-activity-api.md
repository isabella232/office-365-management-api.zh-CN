---
ms.technology: o365-service-communications
ms.TocTitle: Troubleshooting the Office 365 Management Activity API
title: Office 365 管理活动 API 疑难解答
description: 汇总 Microsoft 支持部门在提供此 API 支持方面所收到的最常见问题。
ms.ContentId: 50822603-a1ec-a754-e7dc-67afe36bb1b0
ms.topic: reference (API)
ms.date: ''
localization_priority: Priority
ms.openlocfilehash: 323407799cefe74b331dec01bb746971c41b5adf
ms.sourcegitcommit: ec60dbd5990cfc61b8c000b423e7ade25fa613a8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2020
ms.locfileid: "48397431"
---
# <a name="office-365-management-activity-api-faqs-and-troubleshooting"></a><span data-ttu-id="b3eab-103">Office 365 管理活动 API 常见问题和疑难解答</span><span class="sxs-lookup"><span data-stu-id="b3eab-103">Office 365 Management Activity API FAQs and troubleshooting</span></span>

<span data-ttu-id="b3eab-104">Office 365 管理活动 API（也称为\*统一审核 API \*）是 Office 365 安全与合规性产品的一部分，它：</span><span class="sxs-lookup"><span data-stu-id="b3eab-104">The Office 365 Management Activity API (also known as the *Unified Auditing API*) is a part of Office 365 security and compliance offerings, that:</span></span>

- <span data-ttu-id="b3eab-105">允许以编程方式访问多个审核管道工作负载（例如 SharePoint 和 Exchange）</span><span class="sxs-lookup"><span data-stu-id="b3eab-105">Allows programmatic access to multiple auditing pipeline workloads (such as SharePoint and Exchange)</span></span>

- <span data-ttu-id="b3eab-106">是各种第三方供应商产品使用的主要接口，用于聚合审核数据并对其编制索引</span><span class="sxs-lookup"><span data-stu-id="b3eab-106">Is the main interface used by a variety of third-party vendor products to aggregate and index auditing data</span></span>

<span data-ttu-id="b3eab-107">不应将管理活动 API 与 Office 365 服务通信 API 混淆。</span><span class="sxs-lookup"><span data-stu-id="b3eab-107">The Management Activity API shouldn't be confused with the Office 365 Service Communications API.</span></span> <span data-ttu-id="b3eab-108">管理活动 API 用于审核各种工作负载中的最终用户活动。</span><span class="sxs-lookup"><span data-stu-id="b3eab-108">The Management Activity API is for auditing end user activities in the various workloads.</span></span> <span data-ttu-id="b3eab-109">服务通信 API 用于审核由 Office 365 中的可用服务（例如 Dynamics CRM 或标识服务）发送的状态和消息。</span><span class="sxs-lookup"><span data-stu-id="b3eab-109">The Service Communications API is for auditing status and messages that are sent by the services that are available in Office 365 (such as Dynamics CRM or Identity Service).</span></span>

## <a name="frequently-asked-questions-about-the-office-365-management-activity-api"></a><span data-ttu-id="b3eab-110">有关 Office 365 管理活动 API 的常见问题解答</span><span class="sxs-lookup"><span data-stu-id="b3eab-110">Frequently asked questions about the Office 365 Management Activity API</span></span>

<span data-ttu-id="b3eab-111">**在发送有关给定 Office 365 事件的通知之前，我必须等待的最长时间是多长？**</span><span class="sxs-lookup"><span data-stu-id="b3eab-111">**What is the maximum time I will have to wait before a notification is sent about a given Office 365 event?**</span></span>

<span data-ttu-id="b3eab-112">不存在通知发送的最长保证延迟（换句话说即没有 SLA）。</span><span class="sxs-lookup"><span data-stu-id="b3eab-112">There is no guaranteed maximum latency for notification delivery (in other words, no SLA).</span></span> <span data-ttu-id="b3eab-113">Microsoft 支持部门的经验是，大多数通知都是在事件发生后一小时内发送的。</span><span class="sxs-lookup"><span data-stu-id="b3eab-113">Microsoft Support's experience has been that most notifications are sent within one hour of the event.</span></span> <span data-ttu-id="b3eab-114">延迟通常要短得多，但也经常会比一小时更长。</span><span class="sxs-lookup"><span data-stu-id="b3eab-114">Often the latency is much shorter, but often it's longer as well.</span></span> <span data-ttu-id="b3eab-115">不同工作负载的延迟在一定程度上有所不同，但一般规则是大多数通知将在原始事件发生后的 24 小时内发送。</span><span class="sxs-lookup"><span data-stu-id="b3eab-115">This varies somewhat from workload to workload, but a general rule is that most notifications will be delivered within 24 hours of the originating event.</span></span>

<span data-ttu-id="b3eab-116">**无法再立即获得 Webhook 通知？毕竟，它们不是由事件驱动的吗？**</span><span class="sxs-lookup"><span data-stu-id="b3eab-116">**Aren't webhook notifications more immediate? After all, aren't they are event-driven?**</span></span>

<span data-ttu-id="b3eab-117">否。</span><span class="sxs-lookup"><span data-stu-id="b3eab-117">No.</span></span><span data-ttu-id="b3eab-118">Webhook 通知并不是事件触发通知意义上的事件驱动通知。</span><span class="sxs-lookup"><span data-stu-id="b3eab-118"> Webhook notifications aren't event-driven in the sense that the event triggers the notification.</span></span> <span data-ttu-id="b3eab-119">仍须创建内容 blob，且创建内容 blob 会触发通知发送。</span><span class="sxs-lookup"><span data-stu-id="b3eab-119">The content blob must still be created and creating the content blob is what triggers the notification delivery.</span></span> <span data-ttu-id="b3eab-120">最近，与直接使用 /content 操作查询 API 相比，使用 Webhook 等待通知的时间更长。</span><span class="sxs-lookup"><span data-stu-id="b3eab-120">Recently, there have been longer wait times for notifications when using a webhook compared to querying the API directly with the /content operation.</span></span> <span data-ttu-id="b3eab-121">因此，不应将管理活动 API 视为实时安全警报系统。</span><span class="sxs-lookup"><span data-stu-id="b3eab-121">Therefore, the Management Activity API shouldn't be thought of as a real-time security alert system.</span></span> <span data-ttu-id="b3eab-122">Microsoft 有针对该系统的其他产品。</span><span class="sxs-lookup"><span data-stu-id="b3eab-122">Microsoft has other products for that.</span></span> <span data-ttu-id="b3eab-123">就安全性而言，管理活动 API 事件通知可能更适用于在长时间段内确定使用模式。</span><span class="sxs-lookup"><span data-stu-id="b3eab-123">As far as security is concerned, Management Activity API event notifications can more appropriately be used to determine use patterns over extended periods of time.</span></span>

<span data-ttu-id="b3eab-124">**我可以在管理活动 API 中查询内容 blob 中的特定事件 ID、RecordType 或其他属性吗？**</span><span class="sxs-lookup"><span data-stu-id="b3eab-124">**Can I query the Management Activity API for a particular event ID or RecordType or other properties in the content blob?**</span></span>

<span data-ttu-id="b3eab-125">否。</span><span class="sxs-lookup"><span data-stu-id="b3eab-125">No.</span></span><span data-ttu-id="b3eab-126">不要将通过管理活动 API 提供的数据视为传统意义上的“日志”。</span><span class="sxs-lookup"><span data-stu-id="b3eab-126"> Don't think of the data available through the Management Activity API as being a “log” in the traditional sense.</span></span> <span data-ttu-id="b3eab-127">相反，将其视为事件详细信息的转储。</span><span class="sxs-lookup"><span data-stu-id="b3eab-127">Rather, think of it as a dump of event details.</span></span> <span data-ttu-id="b3eab-128">你可以通过使用自定义应用程序或第三方工具来收集所有这些事件详细信息，在本地存储并对它们编制索引，然后实现自己的查询逻辑。</span><span class="sxs-lookup"><span data-stu-id="b3eab-128">It's up to you to gather all those event details, store and index them locally, and then implement your own query logic, by using either a custom application or a third-party tool.</span></span>

<span data-ttu-id="b3eab-129">**我如何知道来自我现有审核解决方案（该解决方案从管理活动 API 收集数据）的数据是否准确完整？**</span><span class="sxs-lookup"><span data-stu-id="b3eab-129">**How do I know the data coming from my existing auditing solution, which collects data from the Management Activity API, is accurate and complete?**</span></span>

<span data-ttu-id="b3eab-130">简单说，Microsoft 不提供任何类型的日志，允许你交叉检查任何给定的应用程序或第三方 (ISV) 应用程序。</span><span class="sxs-lookup"><span data-stu-id="b3eab-130">The short answer is that Microsoft doesn't provide any kind of a log that will allow you to cross-check any given application or third-party (ISV) application.</span></span> <span data-ttu-id="b3eab-131">还存在其他从相同管道提取数据的 Microsoft 安全产品，但这些产品不属于本次讨论的范围，不能用于直接交叉检查管理活动 API。</span><span class="sxs-lookup"><span data-stu-id="b3eab-131">There are other Microsoft security products that draw their data from the same pipeline, but those products fall outside the scope of this discussion and can't be used to directly cross-check the Management Activity API.</span></span> <span data-ttu-id="b3eab-132">如果担心现有解决方案提供的内容与预期内容之间存在差异，则应实施上述操作。</span><span class="sxs-lookup"><span data-stu-id="b3eab-132">If you're concerned about discrepancies between what your existing solution is providing and what you expect, you should implement the operations illustrated above.</span></span> <span data-ttu-id="b3eab-133">但这可能很困难，具体取决于你现有的工具或解决方案如何列出数据并对其编制索引。</span><span class="sxs-lookup"><span data-stu-id="b3eab-133">But this can be difficult, depending on how your existing tool or solution lists and indexes data.</span></span> <span data-ttu-id="b3eab-134">如果现有解决方案仅显示按实际事件的创建时间排序的数据，则无法按事件创建时间查询 API，进而你可以比较结果集。</span><span class="sxs-lookup"><span data-stu-id="b3eab-134">If your existing solution only presents data sorted by the creation time of the actual event, there's no way to query the API by event creation time so that you could compare result sets.</span></span> <span data-ttu-id="b3eab-135">在这种情况下，你必须连续数天收集通知的内容 blob，手动对它们编制索引或排序，然后进行粗略比较。</span><span class="sxs-lookup"><span data-stu-id="b3eab-135">In this scenario, you'd have to collect the notified content blobs for several days, index or sort them manually, and then do a rough comparison.</span></span>

<span data-ttu-id="b3eab-136">**内容 blob 可以使用多长时间？**</span><span class="sxs-lookup"><span data-stu-id="b3eab-136">**How long will the content blobs remain available?**</span></span>

<span data-ttu-id="b3eab-137">在通知内容 blob 可用后的 7 天内，内容 blob 都可用。</span><span class="sxs-lookup"><span data-stu-id="b3eab-137">Content blobs are available 7 days after the notification of the content blob's availability.</span></span> <span data-ttu-id="b3eab-138">这意味着如果内容 blob 的创建存在明显延迟，则在内容 blob 不再可用之前，你会在实际事件创建日期之后有更多时间（延迟时间加 7 天）。</span><span class="sxs-lookup"><span data-stu-id="b3eab-138">This means that if there is a significant delay in the creation of the content blob, you will have more time (the delay plus 7 days) after the actual event creation date before the content blob is no longer available.</span></span>

<span data-ttu-id="b3eab-139">**如果通知延迟 24 小时才收到，这是不是意味着我只有 6 天的时间来检索内容 blob？**</span><span class="sxs-lookup"><span data-stu-id="b3eab-139">**If there is a 24-hour delay in getting a notification, doesn't that mean I will have only 6 days to retrieve the content blob?**</span></span>

<span data-ttu-id="b3eab-140">否。</span><span class="sxs-lookup"><span data-stu-id="b3eab-140">No.</span></span><span data-ttu-id="b3eab-141">即使通知延迟了很长的时间（例如服务中断的情况），你仍然可以在第一次收到通知后的 7 天内下载与原始事件相关的内容 blob。</span><span class="sxs-lookup"><span data-stu-id="b3eab-141"> Even if the notification is delayed for an unusually long period (for example, in the case of a service interruption), you would still have 7 days after the first availability of the notification to download the content blob related to the originating event.</span></span>

<span data-ttu-id="b3eab-142">**要对特定 Office 365 服务审核什么事件？**</span><span class="sxs-lookup"><span data-stu-id="b3eab-142">**What events are audited for a specific Office 365 service?**</span></span>

<span data-ttu-id="b3eab-143">“Office 365 管理活动 API 架构”一文列出了全部事件。</span><span class="sxs-lookup"><span data-stu-id="b3eab-143">Office 365 Management Activity API schema documentation has a comprehensive list of events.</span></span> <span data-ttu-id="b3eab-144">有关详细信息，请参阅 Office 365 管理活动 API 架构。</span><span class="sxs-lookup"><span data-stu-id="b3eab-144">For details, see Office 365 Management Activity API schema.</span></span> <span data-ttu-id="b3eab-145">另请参阅在安全与合规中心内搜索审核日志中的“已审核的活动”部分，以获取大多数已审核的 Office 365 服务的事件列表。</span><span class="sxs-lookup"><span data-stu-id="b3eab-145">Also see the “Audited activities” section in Search the audit log in the Security & Compliance Center for a list of events for most of the Office 365 services that are audited.</span></span>

<span data-ttu-id="b3eab-146">**如何上手使用管理活动 API？**</span><span class="sxs-lookup"><span data-stu-id="b3eab-146">**How do I onboard to the Management Activity API?**</span></span>

<span data-ttu-id="b3eab-147">若要开始使用 Office 365 管理活动 API，请参阅 Office 365 管理 API 入门。</span><span class="sxs-lookup"><span data-stu-id="b3eab-147">To get started with the Office 365 Management Activity API, see Get started with Office 365 Management APIs.</span></span>

<span data-ttu-id="b3eab-148">**我们希望以编程的方式捕获所有工作负荷中的全部事件。执行此操作的最可靠方法是什么？**</span><span class="sxs-lookup"><span data-stu-id="b3eab-148">**We want to programmatically capture all events in all workloads. What is the most reliable way to do this?**</span></span>

<span data-ttu-id="b3eab-149">为此，可使用 Office 365 管理活动 API。</span><span class="sxs-lookup"><span data-stu-id="b3eab-149">You can do this by using the Office 365 Management Activity API.</span></span> <span data-ttu-id="b3eab-150">此外，由于无法使用 Webhook，因此还建议使用**请求模型**。</span><span class="sxs-lookup"><span data-stu-id="b3eab-150">Also, we recommend that you use the **pull model** due to issues with using webhooks.</span></span> <span data-ttu-id="b3eab-151">有关详细信息，请参阅 Office 365 管理活动 API 疑难解答中的“使用 Webhook”部分。</span><span class="sxs-lookup"><span data-stu-id="b3eab-151">For more information, see the “Using webhooks” section in Troubleshooting the Office 365 Management Activity API.</span></span>

<span data-ttu-id="b3eab-152">**管理活动 API 提取的记录与使用 Microsoft 365 合规中心内的审核日志搜索工具返回的记录是否有何不同？**</span><span class="sxs-lookup"><span data-stu-id="b3eab-152">**Are there any differences in the records that are fetched by the Management Activity API versus the records that are returned by using the audit log search tool in the Microsoft 365 compliance center?**</span></span>

<span data-ttu-id="b3eab-153">这两种方法返回的数据是相同的。</span><span class="sxs-lookup"><span data-stu-id="b3eab-153">The data that is returned by both methods is the same.</span></span> <span data-ttu-id="b3eab-154">数据并未经过筛选。</span><span class="sxs-lookup"><span data-stu-id="b3eab-154">There is no filtering that happens.</span></span> <span data-ttu-id="b3eab-155">唯一区别是：使用 API 可一次获取过去 7 天的数据；</span><span class="sxs-lookup"><span data-stu-id="b3eab-155">The only difference is that with the API, you can get data for the last 7 days at a time.</span></span> <span data-ttu-id="b3eab-156">在安全与合规中心内搜索审核日志（或在 Exchange Online 中使用相应的 Search-unifiedauditlog cmdlet），可获取过去 90 天的数据。</span><span class="sxs-lookup"><span data-stu-id="b3eab-156">When searching the audit log in the Security & Compliance Center (or by using the corresponding Search-UnifiedAuditLog cmdlet in Exchange Online), you can get data for the last 90 days.</span></span>

<span data-ttu-id="b3eab-157">**如果我对 Office 365 组织禁用审核，会发生什么情况？我是否仍然可以通过管理活动 API 获取事件？**</span><span class="sxs-lookup"><span data-stu-id="b3eab-157">**What happens if I disable auditing for my Office 365 organization? Will I still get events via the Management Activity API?**</span></span>

<span data-ttu-id="b3eab-158">否。</span><span class="sxs-lookup"><span data-stu-id="b3eab-158">No.</span></span><span data-ttu-id="b3eab-159">必须为组织启用 Office 365 统一审核，才能通过管理活动 API 拉取记录。</span><span class="sxs-lookup"><span data-stu-id="b3eab-159"> Office 365 unified auditing must be enabled for your organization to pull records via the Management Activity API.</span></span> <span data-ttu-id="b3eab-160">有关说明，请参阅启用或禁用 Office 365 审核日志搜索。</span><span class="sxs-lookup"><span data-stu-id="b3eab-160">For instructions, see Turn Office 365 audit log search on or off.</span></span>

<span data-ttu-id="b3eab-161">**管理活动 API 的限制是什么？**</span><span class="sxs-lookup"><span data-stu-id="b3eab-161">**What is the throttling limit for the Management Activity API?**</span></span>

<span data-ttu-id="b3eab-162">所有组织最初每分钟分配 2000 个请求基线。</span><span class="sxs-lookup"><span data-stu-id="b3eab-162">All organizations are initially allocated a baseline of 2,000 requests per minute.</span></span> <span data-ttu-id="b3eab-163">这不是一个静态的预定义的限制，但根据组合因素进行建模，包括组织中的席位数，以及 Office 365 E5 和 Microsoft 365 E5 组织的带宽大约为非 E5 组织的两倍的事实。</span><span class="sxs-lookup"><span data-stu-id="b3eab-163">This is not a static, predefined limit but is modeled on a combination of factors including the number of seats in the organization and the fact that Office 365 E5 and Microsoft 365 E5 organizations will get approximately twice as much bandwidth as non-E5 organizations.</span></span> <span data-ttu-id="b3eab-164">这也是最大宽带的上限，以保护服务的健康。</span><span class="sxs-lookup"><span data-stu-id="b3eab-164">There will also be cap on the maximum bandwidth to protect the health of the service.</span></span>

> [!NOTE]
> <span data-ttu-id="b3eab-165">尽管每个租户最初每分钟最多可提交 2000 个请求，Microsoft 也无法保证响应速率。</span><span class="sxs-lookup"><span data-stu-id="b3eab-165">Even though each tenant can initially submit up to 2,000 requests per minute, Microsoft cannot guarantee a response rate.</span></span> <span data-ttu-id="b3eab-166">响应速率取决于各种因素，如客户端系统性能、网络容量和网络速度。</span><span class="sxs-lookup"><span data-stu-id="b3eab-166">The response rate depends on various factors, such as client system performance, network capacity, and network speed.</span></span>

<span data-ttu-id="b3eab-167">**我在使用管理活动 API 时遇到了限制错误。该怎么操作？**</span><span class="sxs-lookup"><span data-stu-id="b3eab-167">**I'm encountering a throttling error in the Management Activity API. What should I do?**</span></span>

<span data-ttu-id="b3eab-168">开启 Microsoft 支持票证，申请新限制，并添加提高限制的正当业务理由。</span><span class="sxs-lookup"><span data-stu-id="b3eab-168">Open a ticket with Microsoft Support and request a new throttling limit, and include a business justification for increasing the limit.</span></span> <span data-ttu-id="b3eab-169">我们将会评估申请，如果接受，便会提高限制。</span><span class="sxs-lookup"><span data-stu-id="b3eab-169">We will evaluate the request, and if accepted, we will increase the throttling limit.</span></span>

<span data-ttu-id="b3eab-170">**TargetUpdatedProperties 为何不再位于 Azure Active Directory 活动审核日志的 ExtendedProperties 中？**</span><span class="sxs-lookup"><span data-stu-id="b3eab-170">**Why are TargetUpdatedProperties no longer in ExtendedProperties in the audit logs for Azure Active Directory activities?**</span></span>

<span data-ttu-id="b3eab-171">TargetUpdatedProperties 显示在 ExtendedProperties 中。</span><span class="sxs-lookup"><span data-stu-id="b3eab-171">TargetUpdatedProperties were appearing in ExtendedProperties.</span></span> <span data-ttu-id="b3eab-172">但是，它们将从 ExtendedProperties 中删除，并且现在将显示在 ModifiedProperties 中。</span><span class="sxs-lookup"><span data-stu-id="b3eab-172">However, they have been removed from ExtendedProperties and now appear in ModifiedProperties.</span></span>

## <a name="troubleshooting-the-office-365-management-activity-api"></a><span data-ttu-id="b3eab-173">Office 365 管理活动 API 疑难解答</span><span class="sxs-lookup"><span data-stu-id="b3eab-173">Troubleshooting the Office 365 Management Activity API</span></span>

<span data-ttu-id="b3eab-174">对于任何开始使用 Office 365 管理活动 API 的人来说，应该明确的一点是，没有按事件细节（例如事件发生的日期、可能触发事件的网站集或者事件类型）进行查询的概念。</span><span class="sxs-lookup"><span data-stu-id="b3eab-174">One thing that should be made clear for anyone who's getting started with the Office 365 Management Activity API is that there is no concept of querying by event specifics, such as date that the event occurred, which site collection an event might have been fired from, or the type of event.</span></span> <span data-ttu-id="b3eab-175">相反，而是创建对特定工作负载（例如，SharePoint 或 Azure AD）的订阅，并且每个租户对应一个订阅。</span><span class="sxs-lookup"><span data-stu-id="b3eab-175">Instead, you create subscriptions to specific workloads (for example, SharePoint or Azure AD) and each subscription is per tenant.</span></span>

<span data-ttu-id="b3eab-176">以下各节概述了使用 Office 365 管理活动 API 的客户最常见问题：</span><span class="sxs-lookup"><span data-stu-id="b3eab-176">The following sections summarizes the most common questions that customers have in using the Office 365 Management Activity API:</span></span>

- [<span data-ttu-id="b3eab-177">关于第三方工具和客户端的问题</span><span class="sxs-lookup"><span data-stu-id="b3eab-177">Questions about third-party tools and clients</span></span>](#questions-about-third-party-tools-and-clients)

- [<span data-ttu-id="b3eab-178">在 Office 365 中启用统一审核日志记录</span><span class="sxs-lookup"><span data-stu-id="b3eab-178">Enabling unified audit logging in Office 365</span></span>](#enabling-unified-audit-logging-in-office-365)

- [<span data-ttu-id="b3eab-179">连接到 API</span><span class="sxs-lookup"><span data-stu-id="b3eab-179">Connecting to the API</span></span>](#connecting-to-the-api)

- [<span data-ttu-id="b3eab-180">检查订阅</span><span class="sxs-lookup"><span data-stu-id="b3eab-180">Checking your subscriptions</span></span>](#checking-your-subscriptions)

- [<span data-ttu-id="b3eab-181">创建新订阅</span><span class="sxs-lookup"><span data-stu-id="b3eab-181">Creating a new subscription</span></span>](#creating-a-new-subscription)

- [<span data-ttu-id="b3eab-182">使用 webhook</span><span class="sxs-lookup"><span data-stu-id="b3eab-182">Using webhooks</span></span>](#using-webhooks)

- [<span data-ttu-id="b3eab-183">请求内容 blob 和限制</span><span class="sxs-lookup"><span data-stu-id="b3eab-183">Requesting content blobs and throttling</span></span>](#requesting-content-blobs-and-throttling)

<span data-ttu-id="b3eab-184">我们将展示一系列简单的 PowerShell 脚本，可以帮助你回答客户提出的最常见问题，或者通过演示主要操作开始实施自定义解决方案。</span><span class="sxs-lookup"><span data-stu-id="b3eab-184">We'll show a selection of simple PowerShell scripts that can help you answer the most common questions asked by customers or get you started implementing a custom solution by demonstrating the main operations.</span></span> <span data-ttu-id="b3eab-185">并非所有操作都在这些章节中做了相关解释，但它们在 Office 365 管理活动 API 参考中均有列出。</span><span class="sxs-lookup"><span data-stu-id="b3eab-185">Not all the operations are explained in these sections, but they are all listed in Office 365 Management Activity API reference.</span></span>

### <a name="questions-about-third-party-tools-and-clients"></a><span data-ttu-id="b3eab-186">关于第三方工具和客户端的问题</span><span class="sxs-lookup"><span data-stu-id="b3eab-186">Questions about third-party tools and clients</span></span>

<span data-ttu-id="b3eab-187">最常见问题类别来自使用第三方产品下载和汇总审核数据的客户。</span><span class="sxs-lookup"><span data-stu-id="b3eab-187">The most common category of questions come from customers using third-party products to download and aggregate auditing data.</span></span> <span data-ttu-id="b3eab-188">根据第三方产品，客户可能会遇到设置问题或这些产品中暴露的数据出现中断或不一致的情况。</span><span class="sxs-lookup"><span data-stu-id="b3eab-188">Depending on the third-party product, customers may encounter difficulty with the setup or experience an interruption or an inconsistency in the data exposed in those products.</span></span> <span data-ttu-id="b3eab-189">这里应该指出，此类客户首先应采取的措施是联系其供应商的支持部门。</span><span class="sxs-lookup"><span data-stu-id="b3eab-189">Here it should be stated that the first action such customers should take is to contact their vendor's support organization.</span></span> <span data-ttu-id="b3eab-190">通常情况下，特定租户的供应商配置或服务问题就是客户投诉的原因。</span><span class="sxs-lookup"><span data-stu-id="b3eab-190">Typically, a tenant-specific vendor configuration or service problem is the cause of customer complaints.</span></span>

### <a name="enabling-unified-audit-logging-in-office-365"></a><span data-ttu-id="b3eab-191">在 Office 365 中启用统一审核日志记录</span><span class="sxs-lookup"><span data-stu-id="b3eab-191">Enabling unified audit logging in Office 365</span></span>

<span data-ttu-id="b3eab-192">如果你刚刚设置了一个正在尝试使用管理活动 API 的应用程序，但它无法正常工作，请确保已针对你的 Office 365 组织启用统一审核日志记录。</span><span class="sxs-lookup"><span data-stu-id="b3eab-192">If you've just set up an app that's trying to use the Management Activity API and it's not working, be sure that you've enabled unified audit logging for your Office 365 organization.</span></span> <span data-ttu-id="b3eab-193">可通过启用 Office 365 审核日志来实现此操作。</span><span class="sxs-lookup"><span data-stu-id="b3eab-193">You do this by turning on the Office 365 audit log.</span></span> <span data-ttu-id="b3eab-194">有关说明，请参阅[打开或关闭 Office 365 审核日志搜索](https://docs.microsoft.com/office365/securitycompliance/turn-audit-log-search-on-or-off)。</span><span class="sxs-lookup"><span data-stu-id="b3eab-194">For instructions, see [Turn Office 365 audit log search on or off](https://docs.microsoft.com/office365/securitycompliance/turn-audit-log-search-on-or-off).</span></span>

<span data-ttu-id="b3eab-195">如果未启用统一审核，则通常会收到包含以下字符串的错误：`Microsoft.Office.Compliance.Audit``.DataServiceException: Tenant <tenantID> does not exist.`</span><span class="sxs-lookup"><span data-stu-id="b3eab-195">If unified auditing isn't enabled, you will typically receive an error that contains the following string: `Microsoft.Office.Compliance.Audit``.DataServiceException: Tenant <tenantID> does not exist.`</span></span>

### <a name="connecting-to-the-api"></a><span data-ttu-id="b3eab-196">连接到 API</span><span class="sxs-lookup"><span data-stu-id="b3eab-196">Connecting to the API</span></span>

<span data-ttu-id="b3eab-197">大多数应用程序使用简单直观的客户端凭据 OAuth2 流连接到 API。</span><span class="sxs-lookup"><span data-stu-id="b3eab-197">Most applications connect to the API using a straightforward Client Credentials OAuth2 flow.</span></span> <span data-ttu-id="b3eab-198">因此，第一步是创建一个 Azure AD 应用，该应用具有访问管理活动 API 数据所需的权限。</span><span class="sxs-lookup"><span data-stu-id="b3eab-198">Therefore, the first step is to create an Azure AD application that has the permissions needed to access the Management Activity API data.</span></span> <span data-ttu-id="b3eab-199">介绍进行 Azure AD 应用注册的步骤不在本文的探讨范围内。</span><span class="sxs-lookup"><span data-stu-id="b3eab-199">It's outside the scope of this article to explain the steps to create an Azure AD App registration.</span></span> <span data-ttu-id="b3eab-200">有关详细信息，请参阅[通过 Azure Active Directory 租户注册应用程序](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)。</span><span class="sxs-lookup"><span data-stu-id="b3eab-200">For more information, see [Register your application with your Azure Active Directory tenant](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications).</span></span>

#### <a name="azure-application-permissions"></a><span data-ttu-id="b3eab-201">Azure 应用程序权限</span><span class="sxs-lookup"><span data-stu-id="b3eab-201">Azure application permissions</span></span>

<span data-ttu-id="b3eab-202">当前用于 Office 365 管理活动 API 的三个权限是：</span><span class="sxs-lookup"><span data-stu-id="b3eab-202">The three permissions currently used for the Office 365 Management Activity API are:</span></span>

1. <span data-ttu-id="b3eab-203">为组织读取活动数据</span><span class="sxs-lookup"><span data-stu-id="b3eab-203">Read activity data for your organization</span></span>

2. <span data-ttu-id="b3eab-204">为组织读取服务运行状况信息</span><span class="sxs-lookup"><span data-stu-id="b3eab-204">Read service health information for your organization</span></span>

3. <span data-ttu-id="b3eab-205">读取数据丢失防护 (DLP) 策略事件，包括检测到的敏感信息</span><span class="sxs-lookup"><span data-stu-id="b3eab-205">Read Data Loss Prevention (DLP) policy events, including detected sensitive information</span></span>

> [!NOTE]
> <span data-ttu-id="b3eab-206">你应至少为上述权限集的前两个启用“应用程序权限”和“委派权限”。</span><span class="sxs-lookup"><span data-stu-id="b3eab-206">You should enable both the Application Permissions and the Delegated Permissions for at least the first two of the above permission sets.</span></span> <span data-ttu-id="b3eab-207">仅在你对 DLP 工作负载感兴趣时才需要读取 DLP 策略事件。</span><span class="sxs-lookup"><span data-stu-id="b3eab-207">Read DLP policy events will only be necessary if you are interested in the DLP workloads.</span></span>

#### <a name="getting-an-access-token"></a><span data-ttu-id="b3eab-208">获取访问令牌</span><span class="sxs-lookup"><span data-stu-id="b3eab-208">Getting an access token</span></span>

<span data-ttu-id="b3eab-209">以下 PowerShell 脚本使用应用 ID 和客户端密码从管理活动 API 身份验证端点获取 OAuth2 令牌。</span><span class="sxs-lookup"><span data-stu-id="b3eab-209">The following PowerShell script uses the App ID and a Client Secret to obtain the OAuth2 token from the Management Activity API authentication endpoint.</span></span> <span data-ttu-id="b3eab-210">然后，它将访问令牌放置到 `$headerParams` 数组变量，该变量将附加到 HTTP 请求中。</span><span class="sxs-lookup"><span data-stu-id="b3eab-210">It then places the access token into the `$headerParams` array variable, which you'll attach to your HTTP request.</span></span> <span data-ttu-id="b3eab-211">对于 API 终结点的值（在 $resource 变量中），请使用基于组织的 Microsoft 365 或 Office 365 订阅计划的以下任一值:</span><span class="sxs-lookup"><span data-stu-id="b3eab-211">For the value for the API endpoint (in the $resource variable) use one of the following values based on your organization's Microsoft 365 or Office 365 subscription plan:</span></span>

- <span data-ttu-id="b3eab-212">企业版计划：`manage.office.com`</span><span class="sxs-lookup"><span data-stu-id="b3eab-212">Enterprise plan: `manage.office.com`</span></span>

- <span data-ttu-id="b3eab-213">GCC 政府版计划：`manage-gcc.office.com`</span><span class="sxs-lookup"><span data-stu-id="b3eab-213">GCC government plan: `manage-gcc.office.com`</span></span>

- <span data-ttu-id="b3eab-214">GCC 高级政府版计划: `manage.office365.us`</span><span class="sxs-lookup"><span data-stu-id="b3eab-214">GCC High government plan: `manage.office365.us`</span></span>

- <span data-ttu-id="b3eab-215">DoD 政府版计划: `manage.protection.apps.mil`</span><span class="sxs-lookup"><span data-stu-id="b3eab-215">DoD government plan: `manage.protection.apps.mil`</span></span>

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

<span data-ttu-id="b3eab-216">`$oauth` 变量将包含响应对象，该对象具有多个属性，包括访问令牌。</span><span class="sxs-lookup"><span data-stu-id="b3eab-216">The `$oauth` variable will contain the response object, which has several properties including the access token.</span></span> <span data-ttu-id="b3eab-217">响应示例如下：</span><span class="sxs-lookup"><span data-stu-id="b3eab-217">Here's an example of a response:</span></span>

```json
token_type     : Bearer
expires_in     : 3599
ext_expires_in : 0
expires_on     : 1508285860
not_before     : 1508281960
resource       : https://manage.office.com
access_token   : eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6IjJLVmN1enFBaWRPTHFXU2FvbDd3Z0ZSR0NZbyIsImtpZCI6IjJLVmN1enFBaWRPTHFXU2FvbDd3Z0ZSR0NZbyJ9.eyJhdWQiOiJodHRwczov…
```

### <a name="checking-your-subscriptions"></a><span data-ttu-id="b3eab-218">检查订阅</span><span class="sxs-lookup"><span data-stu-id="b3eab-218">Checking your subscriptions</span></span>

<span data-ttu-id="b3eab-219">如果出现到现有管理活动 API 客户端或解决方案的数据流中断的问题，你可能想知道订阅是否出现某些问题。</span><span class="sxs-lookup"><span data-stu-id="b3eab-219">If you've experienced an interruption in data flowing to an existing Management Activity API client or solution, you might wonder if something happened to your subscription.</span></span> <span data-ttu-id="b3eab-220">要检查活动的订阅，请将以下内容添加到上一个脚本：</span><span class="sxs-lookup"><span data-stu-id="b3eab-220">To check your active subscriptions, add the following to the previous script:</span></span>

```powershell
Invoke-WebRequest -Headers $headerParams -Uri "$resource/api/v1.0/$tenantGUID/activity/feed/subscriptions/list" 
```

#### <a name="sample-response"></a><span data-ttu-id="b3eab-221">示例响应</span><span class="sxs-lookup"><span data-stu-id="b3eab-221">Sample response</span></span>

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

<span data-ttu-id="b3eab-222">这表示租户已启用 Audit.Exchange 和 Audit.SharePoint 订阅。</span><span class="sxs-lookup"><span data-stu-id="b3eab-222">This says that the tenant has both Audit.Exchange and Audit.SharePoint subscriptions enabled.</span></span> <span data-ttu-id="b3eab-223">Exchange 订阅未启用 webhook (null)，而 SharePoint 订阅已启用 webhook，并显示已注册端点的地址。</span><span class="sxs-lookup"><span data-stu-id="b3eab-223">The Exchange subscription has no webhook enabled (null) and the SharePoint subscription has a webhook enabled with the address of the registered endpoint shown.</span></span>

### <a name="creating-a-new-subscription"></a><span data-ttu-id="b3eab-224">创建新订阅</span><span class="sxs-lookup"><span data-stu-id="b3eab-224">Creating a new subscription</span></span>

<span data-ttu-id="b3eab-225">要创建新订阅，请使用 /start 操作。</span><span class="sxs-lookup"><span data-stu-id="b3eab-225">To create a new subscription, you use the /start operation.</span></span> <span data-ttu-id="b3eab-226">对于 API 终结点，请使用基于订阅计划的以下任一值:</span><span class="sxs-lookup"><span data-stu-id="b3eab-226">For the API endpoint, use one of these values base on your subscription plan:</span></span>

- <span data-ttu-id="b3eab-227">企业版计划：`manage.office.com`</span><span class="sxs-lookup"><span data-stu-id="b3eab-227">Enterprise plan: `manage.office.com`</span></span>

- <span data-ttu-id="b3eab-228">GCC 政府版计划：`manage-gcc.office.com`</span><span class="sxs-lookup"><span data-stu-id="b3eab-228">GCC government plan: `manage-gcc.office.com`</span></span>

- <span data-ttu-id="b3eab-229">GCC 高级政府版计划: `manage.office365.us`</span><span class="sxs-lookup"><span data-stu-id="b3eab-229">GCC High government plan: `manage.office365.us`</span></span>

- <span data-ttu-id="b3eab-230">DoD 政府版计划: `manage.protection.apps.mil`</span><span class="sxs-lookup"><span data-stu-id="b3eab-230">DoD government plan: `manage.protection.apps.mil`</span></span>

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

<span data-ttu-id="b3eab-231">前面的示例将获取今天可用的所有内容通知，即从 UTC 时间中午 12:00 到当前时间。</span><span class="sxs-lookup"><span data-stu-id="b3eab-231">The previous example will get all the content notifications that became available today, which means from 12:00 AM UTC to the current time.</span></span> <span data-ttu-id="b3eab-232">如果要指定不同的时间段（请记住，可以查询的最长时间段为 24 小时），请将 *starttime* 和 *endtime* 参数添加到 URI 中，例如：</span><span class="sxs-lookup"><span data-stu-id="b3eab-232">If you want to specify a different time period (keeping in mind that the maximum period for which you can query is 24 hours), add the *starttime* and *endtime* parameters to the URI; for example:</span></span>

```powershell
Invoke-WebRequest -Method GET -Headers $headerParams -Uri "$resource/api/v1.0/$tenantGUID/activity/feed/subscriptions/content?contentType=Audit.SharePoint&startTime=2017-10-13T00:00&endTime=2017-10-13T11:59"
```

> [!NOTE]
> <span data-ttu-id="b3eab-233">要么必须同时使用 *starttime* 和 *endtime* 参数，要么两个参数都不使用。</span><span class="sxs-lookup"><span data-stu-id="b3eab-233">You must use either both *starttime* and *endtime* parameters or neither.</span></span>

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
> - <span data-ttu-id="b3eab-234">*contentUri* 属性是你可以从中检索内容 blob 的 URI。</span><span class="sxs-lookup"><span data-stu-id="b3eab-234">The *contentUri* property is the URI from which you can retrieve the content blob.</span></span> <span data-ttu-id="b3eab-235">blob 本身包含事件详细信息，它将包含有关 1 - N 事件的详细信息。</span><span class="sxs-lookup"><span data-stu-id="b3eab-235">The blob itself is what contains the event details; it will contain details about 1 – N events.</span></span> <span data-ttu-id="b3eab-236">虽然集合中可能有 30 个 JSON 对象，但在这 30 个内容 URI 中可能详述了更多事件。</span><span class="sxs-lookup"><span data-stu-id="b3eab-236">While there may be 30 JSON objects in the collection, there may be many more events detailed in those 30 content URIs.</span></span>
>
> - <span data-ttu-id="b3eab-237">*contentCreated* 属性不是创建所通知的事件的日期，</span><span class="sxs-lookup"><span data-stu-id="b3eab-237">The *contentCreated* property is not the date that the event being notified was created.</span></span> <span data-ttu-id="b3eab-238">而是创建通知的日期。</span><span class="sxs-lookup"><span data-stu-id="b3eab-238">This is the date the notification was created.</span></span> <span data-ttu-id="b3eab-239">可以在创建内容 blob 之前创建该 blob 中详述的事件。</span><span class="sxs-lookup"><span data-stu-id="b3eab-239">The events detailed in that blob may have been created well before the content blob was created.</span></span> <span data-ttu-id="b3eab-240">因此，永远不能直接查询任何给定时间段内发生的事件的 API。</span><span class="sxs-lookup"><span data-stu-id="b3eab-240">Therefore, you can never query the API directly for events that occurred within any given period.</span></span>

#### <a name="paging-contents-for-busy-tenants"></a><span data-ttu-id="b3eab-241">繁忙租户的分页内容</span><span class="sxs-lookup"><span data-stu-id="b3eab-241">Paging contents for busy tenants</span></span>

<span data-ttu-id="b3eab-242">许多较大的 Office 365 租户每小时都会生成数千个事件。</span><span class="sxs-lookup"><span data-stu-id="b3eab-242">Many larger Office 365 tenants have thousands of events being generated every hour.</span></span> <span data-ttu-id="b3eab-243">如果你的组织就是这种情况，并且你尝试执行上述示例中的 24 小时周期查询，则可能需要检索比一个响应中返回的通知更多的通知。</span><span class="sxs-lookup"><span data-stu-id="b3eab-243">If this is the case with your organization, and you try to execute a query for a 24-hour period like in the example above, you may need to retrieve more notifications than can be returned in one response.</span></span> <span data-ttu-id="b3eab-244">在这种情况下，需要实现某种逻辑循环，每次都检查 **NextPageUrl:** 标头值的响应头。</span><span class="sxs-lookup"><span data-stu-id="b3eab-244">In this case, you'll need to implement a logical loop of some kind, each time checking the response headers for the **NextPageUrl:** header value.</span></span> <span data-ttu-id="b3eab-245">有关详细信息，请参阅 Office 365 管理活动 API 参考中的分页部分。</span><span class="sxs-lookup"><span data-stu-id="b3eab-245">See the Pagination section in the Office 365 Management Activity API reference for more details.</span></span>

```powershell
IF the NextPageUrl header is present in the response... 

THEN issue a new request substituting the Uri parameter in the above Invoke-WebRequest example for the value of the NextPageUrl header...
 
ELSE exit the loop
```

<span data-ttu-id="b3eab-246">除非你有一个非常活跃的租户，否则很难测试此循环代码。</span><span class="sxs-lookup"><span data-stu-id="b3eab-246">It will be difficult to test this looping code unless you have a very active tenant.</span></span> <span data-ttu-id="b3eab-247">在测试中，我们尝试在脚本中执行数千次更新操作，并且无法生成足够多数量的通知以要求发送 **NextPageUrl** 标头。</span><span class="sxs-lookup"><span data-stu-id="b3eab-247">In our testing, we tried to execute several thousand update operations in a script and was unable to generate a large enough number of notifications to require the **NextPageUrl** header to be sent.</span></span>

### <a name="using-webhooks"></a><span data-ttu-id="b3eab-248">使用 webhook</span><span class="sxs-lookup"><span data-stu-id="b3eab-248">Using webhooks</span></span>

<span data-ttu-id="b3eab-249">有两种方法可以获得已创建内容 blob 的通知。</span><span class="sxs-lookup"><span data-stu-id="b3eab-249">There two ways to get a notification that content blobs have been created.</span></span> <span data-ttu-id="b3eab-250">*推送*方法是使用 webhook 端点实现的，该端点是你自己创建和托管的 Web 应用程序或云平台上的 Web 应用程序。</span><span class="sxs-lookup"><span data-stu-id="b3eab-250">The *push* approach is implemented with a webhook endpoint, which is a web application that you create and host yourself or on a cloud platform.</span></span> <span data-ttu-id="b3eab-251">在创建针对已审核内容类型的订阅时注册 webhook。</span><span class="sxs-lookup"><span data-stu-id="b3eab-251">You register the webhook at the time you create a subscription to an audited content type.</span></span> <span data-ttu-id="b3eab-252">还可以使用下面显示的方法向现有订阅添加 webhook 注册。</span><span class="sxs-lookup"><span data-stu-id="b3eab-252">You may also add a webhook registration to an existing subscription using the approach shown below.</span></span> <span data-ttu-id="b3eab-253">*拉取*方法要求使用 /content 操作查询特定时间段（不超过 24 小时）。</span><span class="sxs-lookup"><span data-stu-id="b3eab-253">The *pull* approach requires you to query for a particular timespan (no more than 24 hours) using the /content operation.</span></span> <span data-ttu-id="b3eab-254">该响应将告诉你在指定时间段内创建了哪些内容 blob。</span><span class="sxs-lookup"><span data-stu-id="b3eab-254">The response will tell you which content blobs were created during the period specified.</span></span>

<span data-ttu-id="b3eab-255">在添加 webhook 之前，请注意以下两个问题：</span><span class="sxs-lookup"><span data-stu-id="b3eab-255">Before you add a webhook, be aware of the following two issues:</span></span>

1. <span data-ttu-id="b3eab-256">由于调试和疑难解答过程存在困难，因此 Microsoft 不再强调 Webhook。</span><span class="sxs-lookup"><span data-stu-id="b3eab-256">Webhooks are being de-emphasized by Microsoft because of the difficulty in debugging and troubleshooting.</span></span> <span data-ttu-id="b3eab-257">通常，你应该托管响应传入请求的 Web API 端点。</span><span class="sxs-lookup"><span data-stu-id="b3eab-257">Typically, you would host a WebApi endpoint that responds to incoming requests.</span></span> <span data-ttu-id="b3eab-258">许多客户使用托管环境（他们没有完全控制权限）或本地环境（难以允许传入的 HTTP 请求）。</span><span class="sxs-lookup"><span data-stu-id="b3eab-258">Many customers either use hosting environments (which they don't have full control over) or on-premises environments (that have difficulty allowing incoming HTTP requests).</span></span> <span data-ttu-id="b3eab-259">支持部门已经发现许多问题，其中来自 Office 365 审核管道的传入请求已被防火墙或路由器阻止。</span><span class="sxs-lookup"><span data-stu-id="b3eab-259">Support has seen many problems where the incoming requests from the Office 365 auditing pipeline have been blocked by a firewall or router.</span></span> <span data-ttu-id="b3eab-260">在这种情况下，API 将实现退避算法，这可能会令人困惑，并可能导致禁用订阅中的 webhook。</span><span class="sxs-lookup"><span data-stu-id="b3eab-260">In this case, the API will implement a back-off algorithm, which can be confusing and may result in disabling the webhook in the subscription.</span></span>

2. <span data-ttu-id="b3eab-261">执行启动操作后，webhook 必须准备好立即响应验证请求。</span><span class="sxs-lookup"><span data-stu-id="b3eab-261">The webhook must be ready to immediately respond to a validation request after the start operation is executed.</span></span> <span data-ttu-id="b3eab-262">如果 webhook 应用程序中存在错误，则验证将失败，并且不会启用 webhook。</span><span class="sxs-lookup"><span data-stu-id="b3eab-262">If there is a bug in the webhook application, the validation will fail, and the webhook will not be enabled.</span></span> <span data-ttu-id="b3eab-263">有关验证请求架构的信息，请参阅 Office 365 管理活动 API 参考中的 Webhook 验证部分。</span><span class="sxs-lookup"><span data-stu-id="b3eab-263">For information about the validation request schema, see the Webhook validation section in the Office 365 Management Activity API reference.</span></span> <span data-ttu-id="b3eab-264">你应该认真考虑生成生产就绪型 webhook 以响应管理活动 API 通知所面临的挑战。</span><span class="sxs-lookup"><span data-stu-id="b3eab-264">You should seriously consider the challenges in producing a production-ready webhook to respond to Management Activity API notifications.</span></span>

<span data-ttu-id="b3eab-265">如果你决定实现 webhook，则应对其进行测试，以验证在首次调用指定 webhook 端点的任何 /start 操作之前，端点是否已准备好响应针对验证请求和正常通知请求的 HTTP 200 响应。</span><span class="sxs-lookup"><span data-stu-id="b3eab-265">If you decide to implement a webhook, it should be tested to verify that the endpoint is prepared to respond with an HTTP 200 response to both the validation request and the normal notification requests before any /start operation that specifies a webhook endpoint is called for the first time.</span></span> <span data-ttu-id="b3eab-266">通常，你将使用来自 API 的模拟请求来测试此准备情况。</span><span class="sxs-lookup"><span data-stu-id="b3eab-266">Typically, you would use a mocked request from the API to test this readiness.</span></span> <span data-ttu-id="b3eab-267">仔细阅读并遵守 Office 365 管理活动 API 参考中的 Webhook 验证和检索内容部分，以了解这两种请求类型的架构。</span><span class="sxs-lookup"><span data-stu-id="b3eab-267">Carefully read and observe both the Webhook validation and Retrieving content sections in the Office 365 Management Activity API reference to understand the schema of these two types of requests.</span></span>

<span data-ttu-id="b3eab-268">要使用 webhook 端点添加订阅，请将 POST 的正文参数添加到 /start 端点，例如：</span><span class="sxs-lookup"><span data-stu-id="b3eab-268">To add a subscription with a webhook endpoint, add a body parameter to the POST to the /start endpoint; for example:</span></span>

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

<span data-ttu-id="b3eab-269">在此调用之后，将立即向 `https://webhook.myapp.com/o365/ …` 发送验证请求，并且应根据 Office 365 管理活动 API 参考中的 Webhook 验证部分中的说明准备好用于响应的侦听器。</span><span class="sxs-lookup"><span data-stu-id="b3eab-269">Immediately following this call, a validation request will be sent out to `https://webhook.myapp.com/o365/ …` and there should be a listener ready to respond, as per the description in the Webhook validation section in the Office 365 Management Activity API reference.</span></span> <span data-ttu-id="b3eab-270">侦听器必须通过 HTTP 200 进行响应。</span><span class="sxs-lookup"><span data-stu-id="b3eab-270">Your listener must respond with HTTP 200.</span></span> <span data-ttu-id="b3eab-271">如果此时立即运行 /list 操作，则在验证成功返回状态之前，只能看到显示为 null 的 webhook。</span><span class="sxs-lookup"><span data-stu-id="b3eab-271">If you immediately run the /list operation at this point, you will not see the webhook shown as other than null until the validation has returned successfully.</span></span>

#### <a name="checking-notifications-to-webhooks"></a><span data-ttu-id="b3eab-272">检查 webhook 通知</span><span class="sxs-lookup"><span data-stu-id="b3eab-272">Checking notifications to webhooks</span></span>

<span data-ttu-id="b3eab-273">能够区分 /notifications 操作和 /content 操作很重要。</span><span class="sxs-lookup"><span data-stu-id="b3eab-273">It's important to distinguish between the /notifications operation and the /content operation.</span></span> <span data-ttu-id="b3eab-274">仅当通过 webhook 端点进行订阅时才需要检查通知。</span><span class="sxs-lookup"><span data-stu-id="b3eab-274">Checking notifications is only relevant if you have subscribed with a webhook endpoint.</span></span> <span data-ttu-id="b3eab-275">此操作让你了解尝试向 webhook 发送的通知是否成功。</span><span class="sxs-lookup"><span data-stu-id="b3eab-275">This operation will let you know if attempts to send notifications to your webhook have been successful or not.</span></span> <span data-ttu-id="b3eab-276">此操作不应用于列出可用内容。</span><span class="sxs-lookup"><span data-stu-id="b3eab-276">This operation should not be used to list available content.</span></span> <span data-ttu-id="b3eab-277">要根据 webhook 中已收到内容交叉检查内容的可用性，你可以使用 /content 操作。</span><span class="sxs-lookup"><span data-stu-id="b3eab-277">To cross-check the availability of content against what you may have already received in your webhook, you would use the /content operation.</span></span> <span data-ttu-id="b3eab-278">但请务必先使用 /notifications 操作检查失败的通知。</span><span class="sxs-lookup"><span data-stu-id="b3eab-278">But be sure to first check for failed notifications using the /notifications operation.</span></span>

### <a name="requesting-content-blobs-and-throttling"></a><span data-ttu-id="b3eab-279">请求内容 blob 和限制</span><span class="sxs-lookup"><span data-stu-id="b3eab-279">Requesting content blobs and throttling</span></span>

<span data-ttu-id="b3eab-280">获取内容 URI 列表后，必须请求 URI 指定的 blob。</span><span class="sxs-lookup"><span data-stu-id="b3eab-280">After you've obtained a list of content URIs, you must request the blobs specified by the URIs.</span></span> <span data-ttu-id="b3eab-281">下面是使用 PowerShell 请求内容 Blob（使用适用于企业组织的 manage.office.com API 终结点）的示例。</span><span class="sxs-lookup"><span data-stu-id="b3eab-281">The following is an example of requesting a content blob (using the manage.office.com API endpoint for Enterprise organizations) using PowerShell.</span></span> <span data-ttu-id="b3eab-282">此示例假定你已使用本文[获取访问令牌](#getting-an-access-token)部分中的上一个示例获取访问令牌并已正确填充 `$headerParams` 变量。</span><span class="sxs-lookup"><span data-stu-id="b3eab-282">This example assumes you have already used the previous example in the [Getting an access token](#getting-an-access-token) section in this article to get an access token and have populated the `$headerParams` variable appropriately.</span></span>

```powershell
# Get a content blob
$uri = 'https://manage.office.com/api/v1.0/<<your-tenant-guid>>/activity/feed/audit/<<ContentID>$audit_sharepoint$Audit_SharePoint'
$contents = Invoke-WebRequest -Method GET -Headers $headerParams -Uri $uri
```

<span data-ttu-id="b3eab-283">请注意上一个示例中有关 *$uri* 变量的以下内容：</span><span class="sxs-lookup"><span data-stu-id="b3eab-283">Note the following things about the *$uri* variable in the previous example:</span></span>

- <span data-ttu-id="b3eab-284">我们使用单引号，以便 *$* 符号不会被解译为 PowerShell 中的变量</span><span class="sxs-lookup"><span data-stu-id="b3eab-284">We used single quotes so that the *$* symbols aren't interpreted as variables in PowerShell</span></span>

- <span data-ttu-id="b3eab-285">整个 URI 将在针对你上次调用 /content 端点的响应的 *contentUri* 属性中返回。</span><span class="sxs-lookup"><span data-stu-id="b3eab-285">The entire URI will be returned in the *contentUri* property of the response to your previous call to the /content endpoint.</span></span> <span data-ttu-id="b3eab-286">显示的占位符令牌仅用于说明。</span><span class="sxs-lookup"><span data-stu-id="b3eab-286">The placeholder tokens shown are just for illustration.</span></span>

<span data-ttu-id="b3eab-287">在尝试检索可用内容 blob 时，许多客户（大多数与繁忙租户合作）遇到如下错误：</span><span class="sxs-lookup"><span data-stu-id="b3eab-287">When attempting to retrieve the available content blobs, many customers (mostly working with busy tenants) have experienced errors that look like this:</span></span>

```json
Response Code 403: {'error':{'code':'AF429','message':'Too many requests. Method=GetBlob, PublisherId=00000000-0000-0000-0000-000000000000'}}
```

<span data-ttu-id="b3eab-288">这可能因限制所致。</span><span class="sxs-lookup"><span data-stu-id="b3eab-288">This is likely due to throttling.</span></span> <span data-ttu-id="b3eab-289">请注意，Publisherid 参数的值可能表示客户端未在请求中指定 *PublisherIdentifier*。</span><span class="sxs-lookup"><span data-stu-id="b3eab-289">Note that the value of the PublisherId parameter likely indicates that the client didn't specify the *PublisherIdentifier* in the request.</span></span> <span data-ttu-id="b3eab-290">此外，请记住，正确的参数名称是 *PublisherIdentifier*，即使你在 403 错误响应中看到列出 *PublisherId* 也是如此。</span><span class="sxs-lookup"><span data-stu-id="b3eab-290">Also, keep in mind that the correct parameter name is *PublisherIdentifier* even though you see *PublisherId* listed in the 403 error responses.</span></span>

> [!NOTE]
> <span data-ttu-id="b3eab-291">在 API 参考中，在 API 的每个操作中均会列出 *PublisherIdentifier* 参数，但在检索内容 blob 时，它也应包含在针对 contentUri URL 的 GET 请求中。</span><span class="sxs-lookup"><span data-stu-id="b3eab-291">In the API reference, the *PublisherIdentifier* parameter is listed in every operation of the API, but it should also be included in the GET request to the contentUri URL when retrieving the content blob.</span></span>

<span data-ttu-id="b3eab-292">如果你正在进行简单的 API 调用来解决问题（例如，检查给定订阅是否处于活动状态），则可以安全地省略 *PublisherIdentifier* 参数，但在每次调用时，任何最终用于生产用途的代码绝对都应该包括 *PublisherIdentifier* 参数。</span><span class="sxs-lookup"><span data-stu-id="b3eab-292">If you're doing simple API calls to troubleshoot problems (for example, checking if a given subscription is active) you can safely omit the *PublisherIdentifier* parameter, but absolutely any code that is eventually meant for production use should include the *PublisherIdentifier* parameter on every call.</span></span>

<span data-ttu-id="b3eab-293">如果你正在为公司的租户实现客户端，则 *PublisherIdentifier* 是租户 GUID。</span><span class="sxs-lookup"><span data-stu-id="b3eab-293">If you're implementing a client for your company's tenant, the *PublisherIdentifier* is the Tenant GUID.</span></span> <span data-ttu-id="b3eab-294">如果要为多个客户创建 ISV 应用程序或外接程序，则 *PublisherIdentifier* 应该是 ISV 的租户 GUID，而不是最终用户公司的租户 GUID。</span><span class="sxs-lookup"><span data-stu-id="b3eab-294">If you are creating an ISV application or add-in for multiple customers, the *PublisherIdentifier* should be the ISV's Tenant GUID, and not the tenant GUID of end user's company.</span></span>

<span data-ttu-id="b3eab-295">如果包含有效的 *PublisherIdentifier*，那么你将进入一个池，其中每分钟为每个租户分配 6 万个请求。</span><span class="sxs-lookup"><span data-stu-id="b3eab-295">If you include the valid *PublisherIdentifier*, then you will be in a pool that is allotted 60K requests per minute per tenant.</span></span> <span data-ttu-id="b3eab-296">请求的量是极大的。</span><span class="sxs-lookup"><span data-stu-id="b3eab-296">This is an exceptionally large number of requests.</span></span> <span data-ttu-id="b3eab-297">但是，如果不包含 *PublisherIdentifier* 参数，则你将进入每分钟为所有租户分配 6 万个请求的常规池。</span><span class="sxs-lookup"><span data-stu-id="b3eab-297">However, if you don't include the *PublisherIdentifier* parameter, you will be in the general pool allotted 60K requests per minute for all tenants.</span></span> <span data-ttu-id="b3eab-298">在这种情况下，你很可能会发现调用受到限制。</span><span class="sxs-lookup"><span data-stu-id="b3eab-298">In this case, you will more than likely find that your calls are getting throttled.</span></span> <span data-ttu-id="b3eab-299">为了防止出现这种情况，以下是使用 *PublisherIdentifier* 请求内容 blob 的方法：</span><span class="sxs-lookup"><span data-stu-id="b3eab-299">To prevent this, here's how you would request a content blob using the *PublisherIdentifier*:</span></span>

```json
$contentUri = ($response.Content | ConvertFrom-Json).contentUri[0]
$uri = $contentUri + '?PublisherIdentifier=82b24b6d-0591-4604-827b-705d55d0992f'
$contents = Invoke-WebRequest -Method GET -Headers $headerParams -Uri $uri
```

<span data-ttu-id="b3eab-300">上一个示例假定 *$response* 变量填充了请求 /content 端点的响应，并且 *$headerParams* 变量包含有效的访问令牌。</span><span class="sxs-lookup"><span data-stu-id="b3eab-300">The previous example assumes that the *$response* variable was populated with the response to a request to the /content endpoint and that the *$headerParams* variable includes a valid access token.</span></span> <span data-ttu-id="b3eab-301">该脚本从响应中捕捉内容 URI 数组中的第一项，然后调用 GET 下载该 blob，并将其放入 *$contents* 变量中。</span><span class="sxs-lookup"><span data-stu-id="b3eab-301">The script grabs the first item in the array of content URIs from the response and then invokes the GET to download that blob and put it in the *$contents* variable.</span></span> <span data-ttu-id="b3eab-302">代码可能会遍历 contentUri 集合，针对每个 *contentUri* 发出 GET。</span><span class="sxs-lookup"><span data-stu-id="b3eab-302">Your code will likely loop through the contentUri collection, issuing the GET for each *contentUri*.</span></span>
