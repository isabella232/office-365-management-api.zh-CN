---
ms.technology: o365-service-communications
ms.TocTitle: Office 365 Management APIs overview
title: Office 365 管理 API 概述
description: 提供了一个用于执行所有 Office 365 客户和合作伙伴的管理任务的扩展性平台，包括服务通信、安全性、合规性、报告和审核等方面的任务。
ms.ContentId: 4fca85f9-efa6-3b4b-b10d-a07cb31f803f
ms.topic: reference (API)
ms.date: 09/28/2016
ms.localizationpriority: high
ms.openlocfilehash: 52924d518dd74f822a61977d96c5edbb7fc1e888
ms.sourcegitcommit: 13b50617b1a73f5890414087d8eabe6b2240cfb4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/25/2021
ms.locfileid: "58510109"
---
# <a name="office-365-management-apis-overview"></a>Office 365 管理 API 概述

Office 365 管理 API 提供了一个用于执行所有 Office 365 客户和合作伙伴的管理任务的扩展性平台，包括服务通信、安全性、合规性、报告和审核等方面的任务。 在设计和实现方面，所有 Office 365 管理 API 都与当前的一套 Office 365 REST API 保持一致，采用常见行业标准方法，包括 OAuth v2、OData v4 和 JSON。 与其他 Office 365 API 一样，应用程序是在 Azure Active Directory 中进行注册，便于开发人员一致地验证和授权应用程序。

若对 Office 365 管理 API 有疑问，请在 [StackOverflow](http://stackoverflow.com/tags/office365) 上使用 [office365] 标签发布问题。

## <a name="office-365-service-communications-api-preview"></a>Office 365 服务通信 API（预览版）

Office 365 服务通信 API 已在预览模式下发布。 它取代了旧版 Office 365 服务通信 API，用于向租户管理员和合作伙伴提供服务运行状况信息。 与旧版不同的是，新版服务通信 API 提供有凝聚力的平台体验，并以一致的方式（包括 URL 命名、数据格式和身份验证）内置 REST API。

只有新版 API 中才有新功能，这样是为了鼓励旧版客户尽早采用新版。 在 Office 365 服务通信 API 正式发布后，我们便会开始弃用旧版服务通信 API。 

有关操作参考，请参阅 [Office 365 服务通信 API（预览版）参考](office-365-service-communications-api-reference.md)。


## <a name="office-365-management-activity-api"></a>Office 365 管理活动 API

使用 Office 365 管理活动 API，可从 Office 365 和 Azure Active Directory 活动日志中获取各个用户、管理员、系统以及策略操作和事件的相关信息。 客户和合作伙伴可根据此类信息，新建企业运营、安全性和合规性监视解决方案或增强现有解决方案。 

有关操作参考，请参阅 [Office 365 管理活动 API 参考](office-365-management-activity-api-reference.md)。

## <a name="see-also"></a>另请参阅

- [Office 365 管理 API 入门](get-started-with-office-365-management-apis.md)
- [Office 365 管理活动 API 架构](office-365-management-activity-api-schema.md)
- [Office 365 管理活动 API 疑难解答](troubleshooting-the-office-365-management-activity-api.md)
- [Office 365 REST API](/previous-versions/office/office-365-api/how-to/platform-development-overview)
