---
ms.TocTitle: Office 365 Management Activity API frequently asked questions
title: Office 365 管理活动 API 常见问题解答
description: 有关使用 Office 365 管理活动 API 的常见问题解答
ms.ContentId: ''
ms.topic: reference (API)
ms.date: ''
localization_priority: Priority
ms.openlocfilehash: 9083127d1fd3ecf82e5fe778ba1727d22d91017c
ms.sourcegitcommit: 784b581a699c6d0ab7939ea621d5ecbea71925ea
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/29/2019
ms.locfileid: "35924775"
---
# <a name="office-365-management-activity-api-frequently-asked-questions"></a>Office 365 管理活动 API 常见问题解答

#### <a name="what-events-are-audited-for-a-specific-office-365-service"></a>要对特定 Office 365 服务审核什么事件？

“Office 365 管理活动 API 架构”一文列出了全部事件。 有关详细信息，请参阅 [Office 365 管理活动 API 架构](office-365-management-activity-api-schema.md)。 另请参阅[在安全与合规中心内搜索审核日志](https://docs.microsoft.com/en-us/office365/securitycompliance/search-the-audit-log-in-security-and-compliance#audited-activities)中的“已审核的活动”部分，以获取大多数已审核的 Office 365 服务的事件列表。

#### <a name="how-do-i-onboard-to-the-management-activity-api"></a>如何上手使用管理活动 API？

若要开始使用 Office 365 管理活动 API，请参阅 [Office 365 管理 API 入门](get-started-with-office-365-management-apis.md)。
 
#### <a name="what-is-the-throttling-limit-for-the--management-activity-api"></a>管理活动 API 的限制是什么？

目前，每个发布者 ID 每分钟的请求数不得超过 6 万个。 

#### <a name="we-want-to-programmatically-capture-all-events-in-all-workloads-what-is-the-most-reliable-way-to-do-this"></a>我们希望以编程方式捕获所有工作负载中的全部事件。 实现此目标的最可靠方法是什么？

为此，可使用 Office 365 管理活动 API。 此外，由于无法使用 Webhook，因此还建议使用**请求模型**。 有关详细信息，请参阅 [Office 365 管理活动 API 疑难解答](troubleshooting-the-office-365-management-activity-api.md#using-webhooks)中的“使用 Webhook”部分。

#### <a name="is-it-true-that-mailbox-auditing-in-exchange-online-can-only-be-enabled-by-using-powershell"></a>Exchange Online 中的邮箱审核是否只能使用 PowerShell 启用？

过去是这样，但自 2019 年 1 月起，所有 Office 365 组织都会默认启用邮箱审核。 有关详细信息，请参阅[管理邮箱审核](https://docs.microsoft.com/office365/securitycompliance/enable-mailbox-auditing)。

#### <a name="are-there-any-differences-in-the-records-that-are-fetched-by-the-management-activity-api-versus-the-records-that-are-returned-by-using-the-audit-log-search-tool-in-the-office-365-security--compliance-center"></a>管理活动 API 提取的记录与使用 Office 365 安全与合规中心内的审核日志搜索工具返回的记录是否有何不同？

这两种方法返回的数据是相同的。 数据并未经过筛选。 唯一区别是：使用 API 可一次获取过去 7 天的数据； 在安全与合规中心内搜索审核日志（或在 Exchange Online 中使用相应的 [Search-unifiedauditlog](https://docs.microsoft.com/powershell/module/exchange/policy-and-compliance-audit/search-unifiedauditlog) cmdlet），可获取过去 90 天的数据。 

#### <a name="what-happens-if-i-disable-auditing-for-my-office-365-organization-will-i-still-get-events-via-the-management-activity-api"></a>如果禁用 Office 365 组织审核，会出现什么情况？ 是否仍可通过管理活动 API 获得事件？

否。 必须为组织启用 Office 365 统一审核，才能通过管理活动 API 拉取记录。 有关说明，请参阅 [ 启用或禁用 Office 365 审核日志搜索 ](https://docs.microsoft.com/office365/securitycompliance/turn-audit-log-search-on-or-off)。

#### <a name="arent-webhook-notifications-more-immediate-after-all-arent-they-event-driven"></a>Webhook 通知不是更即时吗？ 毕竟它们是事件驱动通知，不是吗？

不是。 Webhook 通知并不是事件触发通知意义上的事件驱动通知。 仍必须创建内容 blob，而创建内容 blob 则会触发通知传递。 最近，与直接使用 */content* 操作查询 API 相比，使用 Webhook 的通知等待时间更长。 因此，不得将管理活动 API 视为实时安全警报系统。 对于这一领域，Microsoft 有其他产品。 就安全性而言，管理活动 API 事件通知可能更适用于确定长时间的使用模式。

#### <a name="when-pulling-the-data-from-the-management-activity-api-there-is-sometimes-a-delay-of-more-than-3-to-5-days-why-is-this"></a>使用管理活动 API 拉取数据时，有时会有超过 3 到 5 天的延迟。 这是什么原因？

有时，Office 365 服务会出现暂时中断或其他问题。 在这种情况下，一些审核记录会遭删除，而服务则会尝试回填这些记录。 虽然只有大约 5% 到 10% 的记录会出现这种情况，但正是这些记录导致了在某些情况下可能会发生的延迟。 如果延迟超过 5 天，请检查 Office 365 管理中心内的服务运行状况仪表板。 如果需要，还可以开启 [Microsoft 支持](https://support.office.com/article/contact-support-for-business-products-admin-help-32a17ca7-6fa0-4870-8a8d-e25ba4ccfd4b#ID0EAADAAA=online)票证。

#### <a name="im-encountering-a-throttling-error-in-the-management-activity-api-what-should-i-do"></a>我在使用管理活动 API 时遇到了限制错误。 该怎么办？

开启 [Microsoft 支持](https://support.office.com/article/contact-support-for-business-products-admin-help-32a17ca7-6fa0-4870-8a8d-e25ba4ccfd4b#ID0EAADAAA=online)票证，申请新限制，并添加提高限制的正当业务理由。 我们将会评估申请，如果接受，便会提高限制。

#### <a name="why-are-targetupdatedproperties-no-longer-in-extendedproperties-in-the-audit-logs-for-azure-active-directory-activities"></a>TargetUpdatedProperties 为何不再位于 Azure Active Directory 活动审核日志的 ExtendedProperties 中？

TargetUpdatedProperties 显示在 ExtendedProperties 中。 但是，它们将从 ExtendedProperties 中删除，并且现在将显示在 ModifiedProperties 中。
