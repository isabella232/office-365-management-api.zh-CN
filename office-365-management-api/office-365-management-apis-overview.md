---
ms.TocTitle: Office 365 Management APIs overview
title: Office 365 管理 API 概述
description: 提供了一个用于执行所有 Office 365 客户和合作伙伴的管理任务的扩展性平台，包括服务通信、安全性、合规性、报告和审核等方面的任务。
ms.ContentId: 4fca85f9-efa6-3b4b-b10d-a07cb31f803f
ms.topic: reference (API)
ms.date: 09/28/2016
ms.openlocfilehash: b49bbbcaaefa1d33014919eaa5f0da8fd54c0889
ms.sourcegitcommit: 525c0d0e78cc44ea8cb6a4bdce1858cb4ef91d57
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2018
ms.locfileid: "25834790"
---
# <a name="office-365-management-apis-overview"></a><span data-ttu-id="302a1-103">Office 365 管理 API 概述</span><span class="sxs-lookup"><span data-stu-id="302a1-103">See Office 365 Management APIs overview.</span></span>

<span data-ttu-id="302a1-104">Office 365 管理 API 提供了一个用于执行所有 Office 365 客户和合作伙伴的管理任务的扩展性平台，包括服务通信、安全性、合规性、报告和审核等方面的任务。</span><span class="sxs-lookup"><span data-stu-id="302a1-104">Tenant administrators and application developers can use the Office 365 Management APIs, a single extensibility platform for all Office 365 customers' and partners' management tasks, including service communications, security, compliance, reporting, auditing, user management, and reseller management.</span></span> <span data-ttu-id="302a1-105">在设计和实现方面，所有 Office 365 管理 API 都与当前的一套 Office 365 REST API 保持一致，采用常见行业标准方法，包括 OAuth v2、OData v4 和 JSON。</span><span class="sxs-lookup"><span data-stu-id="302a1-105">All of the Office 365 Management APIs are consistent in design and implementation with the current suite of Office 365 REST APIs, using common industry-standard approaches, including OAuth v2, OData v4, and JSON.</span></span> <span data-ttu-id="302a1-106">与其他 Office 365 API 一样，应用程序是在 Azure Active Directory 中进行注册，便于开发人员一致地验证和授权应用程序。</span><span class="sxs-lookup"><span data-stu-id="302a1-106">Like the other Office 365 APIs, applications are registered in Azure Active Directory, giving developers a consistent way to authenticate and authorize their apps.</span></span>

<span data-ttu-id="302a1-107">若对 Office 365 管理 API 有疑问，请在 [StackOverflow](http://stackoverflow.com/tags/office365) 上使用 [office365] 标签发布问题。</span><span class="sxs-lookup"><span data-stu-id="302a1-107">If you have questions about the Office 365 Management APIs, post your question on [Stack Overflow](http://stackoverflow.com/tags/office365), using the [office365] tag.</span></span>

## <a name="office-365-service-communications-api-preview"></a><span data-ttu-id="302a1-108">Office 365 服务通信 API（预览版）</span><span class="sxs-lookup"><span data-stu-id="302a1-108">Office 365 Service Communications API</span></span>

<span data-ttu-id="302a1-109">Office 365 服务通信 API 已在预览模式下发布。</span><span class="sxs-lookup"><span data-stu-id="302a1-109">The Office 365 Service Communications API has been released in preview mode.</span></span> <span data-ttu-id="302a1-110">它取代了旧版 Office 365 服务通信 API，用于向租户管理员和合作伙伴提供服务运行状况信息。</span><span class="sxs-lookup"><span data-stu-id="302a1-110">It replaces the Office 365 Service Communications API to provide service health information to tenant administrators and partners.</span></span> <span data-ttu-id="302a1-111">与旧版不同的是，新版服务通信 API 提供有凝聚力的平台体验，并以一致的方式（包括 URL 命名、数据格式和身份验证）内置 REST API。</span><span class="sxs-lookup"><span data-stu-id="302a1-111">Unlike the previous version, the new Service Communications API delivers a cohesive platform experience, with REST APIs built in a consistent fashion including URL naming, data format, and authentication.</span></span>

<span data-ttu-id="302a1-112">只有新版 API 中才有新功能，这样是为了鼓励旧版客户尽早采用新版。</span><span class="sxs-lookup"><span data-stu-id="302a1-112">New features are only added to the new version of the API, encouraging early adoption by legacy customers.</span></span> <span data-ttu-id="302a1-113">在 Office 365 服务通信 API 正式发布后，我们便会开始弃用旧版服务通信 API。</span><span class="sxs-lookup"><span data-stu-id="302a1-113">When the General Announcement of Office 365 Service Communications API was made, the older version of the Service Communications API began a period of deprecation.</span></span> 

<span data-ttu-id="302a1-114">有关操作参考，请参阅 [Office 365 服务通信 API（预览版）参考](office-365-service-communications-api-reference.md)。</span><span class="sxs-lookup"><span data-stu-id="302a1-114">For operations reference, see [Office 365 Service Communications API reference (preview)](office-365-service-communications-api-reference.md).</span></span>


## <a name="office-365-management-activity-api"></a><span data-ttu-id="302a1-115">Office 365 管理活动 API</span><span class="sxs-lookup"><span data-stu-id="302a1-115">Office 365 Management Activity API</span></span>

<span data-ttu-id="302a1-116">使用 Office 365 管理活动 API，可从 Office 365 和 Azure Active Directory 活动日志中获取各个用户、管理员、系统以及策略操作和事件的相关信息。</span><span class="sxs-lookup"><span data-stu-id="302a1-116">The Office 365 Management Activity API provides information about user, admin, system, and policy actions and events from Office 365 and Azure Active Directory activity logs to Office 365 tenant administrators and partners via REST APIs.</span></span> <span data-ttu-id="302a1-117">客户和合作伙伴可根据此类信息，新建企业运营、安全性和合规性监视解决方案或增强现有解决方案。</span><span class="sxs-lookup"><span data-stu-id="302a1-117">Customers and partners can use this information to create new or enhance existing operations, security, and compliance-monitoring solutions for the enterprise.</span></span> 

<span data-ttu-id="302a1-118">有关操作参考，请参阅 [Office 365 管理活动 API 参考](office-365-management-activity-api-reference.md)。</span><span class="sxs-lookup"><span data-stu-id="302a1-118">For operations reference, see [Office 365 Management Activity API reference (preview)](office-365-management-activity-api-reference.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="302a1-119">另请参阅</span><span class="sxs-lookup"><span data-stu-id="302a1-119">See also</span></span>

- [<span data-ttu-id="302a1-120">Office 365 管理 API 入门</span><span class="sxs-lookup"><span data-stu-id="302a1-120">Get started with Office 365 APIs powered by Microsoft Graph</span></span>](get-started-with-office-365-management-apis.md)
- [<span data-ttu-id="302a1-121">Office 365 管理活动 API 架构</span><span class="sxs-lookup"><span data-stu-id="302a1-121">Office 365 Management Activity API</span></span>](office-365-management-activity-api-schema.md)
- [<span data-ttu-id="302a1-122">Office 365 管理活动 API 疑难解答</span><span class="sxs-lookup"><span data-stu-id="302a1-122">Office 365 Management Activity API</span></span>](troubleshooting-the-office-365-management-activity-api.md)
- [<span data-ttu-id="302a1-123">Office 365 REST API</span><span class="sxs-lookup"><span data-stu-id="302a1-123">Office 365 REST APIs reference</span></span>](https://docs.microsoft.com/zh-CN/previous-versions/office/office-365-api/how-to/platform-development-overview)

