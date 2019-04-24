---
ms.TocTitle: Get started with Office 365 Management APIs
title: Office 365 管理 API 入门
description: API 使用 Azure AD 提供身份验证服务，这样你便可使用这些服务向应用程序授予对 API 的访问权限。
ms.ContentId: 74137c9a-29e0-b588-6122-26f4d2c5e3fc
ms.topic: reference (API)
ms.date: 09/05/2018
localization_priority: Priority
ms.openlocfilehash: 9732b5a838bdf4c14a6a13af8196704c89dec63d
ms.sourcegitcommit: 5b1eaeb7f262b7b9f7ab30ccb9f10878814153ac
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "32224019"
---
# <a name="get-started-with-office-365-management-apis"></a><span data-ttu-id="fa1ee-103">Office 365 管理 API 入门</span><span class="sxs-lookup"><span data-stu-id="fa1ee-103">Get started with Office 365 Management APIs</span></span>

<span data-ttu-id="fa1ee-104">如果创建需要访问 Office 365 管理 API 等安全服务的应用程序，必须设法让安全服务知道应用程序是否有权访问它。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-104">When you create an application that needs access to secured services like the Office 365 Management APIs, you need to provide a way to let the service know if your application has rights to access it.</span></span> <span data-ttu-id="fa1ee-105">Office 365 管理 API 使用 Azure AD 提供身份验证服务，这样你便可使用这些服务向应用程序授予对 API 的访问权限。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-105">The Office 365 Management APIs use Azure AD to provide authentication services that you can use to grant rights for your application to access them.</span></span> 

<span data-ttu-id="fa1ee-106">有四个关键步骤：</span><span class="sxs-lookup"><span data-stu-id="fa1ee-106">There are four key steps:</span></span>

1. <span data-ttu-id="fa1ee-107">**在 Azure AD 中注册应用程序**。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-107">**Register your application in Azure AD**.</span></span> <span data-ttu-id="fa1ee-108">为了让应用程序有权访问 Office 365 管理 API，必须在 Azure AD 中注册应用程序。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-108">To allow your application access to the Office 365 Management APIs, you need to register your application in Azure AD.</span></span> <span data-ttu-id="fa1ee-109">这样，便可为应用程序建立标识，并指定它访问 API 所需的权限级别。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-109">This allows you to establish an identity for your application and specify the permission levels it needs to access the APIs.</span></span>
    
2. <span data-ttu-id="fa1ee-110">**获取 Office 365 租户管理员同意**。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-110">**Get Office 365 tenant admin consent**.</span></span> <span data-ttu-id="fa1ee-111">Office 365 租户管理员必须明确同意允许应用程序通过 Office 365 管理 API 访问管理员租户数据。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-111">An Office 365 tenant admin must explicitly grant consent to allow your application to access their tenant data by means of the Office 365 Management APIs.</span></span> <span data-ttu-id="fa1ee-112">同意过程是一种基于浏览器的体验，需要租户管理员先登录 **Azure AD 同意 UI**，并检查应用程序请求获取的访问权限，再同意或拒绝请求。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-112">The consent process is a browser-based experience that requires the tenant admin to sign in to the **Azure AD consent UI** and review the access permissions that your application is requesting, and then either grant or deny the request.</span></span> <span data-ttu-id="fa1ee-113">在同意授予后，UI 便会将用户重定向回应用程序，此时 URL 中包含授权代码。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-113">After consent is granted, the UI redirects the user back to your application with an authorization code in the URL.</span></span> <span data-ttu-id="fa1ee-114">应用程序对 Azure AD 执行服务到服务调用，以用此授权代码交换包含租户管理员和应用程序相关信息的访问令牌。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-114">Your application makes a service-to-service call to Azure AD to exchange this authorization code for an access token, which contains information about both the tenant admin and your application.</span></span> <span data-ttu-id="fa1ee-115">必须从访问令牌中提取并存储租户 ID，以供日后使用。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-115">The tenant ID must be extracted from the access token and stored for future use.</span></span>
    
3. <span data-ttu-id="fa1ee-116">**从 Azure AD 请求获取访问令牌**。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-116">**Request access tokens from Azure AD**.</span></span> <span data-ttu-id="fa1ee-117">应用程序使用在 Azure AD 中配置的应用程序凭据，持续不断地请求获取适用于已同意租户的其他访问令牌，而无需与租户管理员有进一步交互。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-117">Using your application's credentials as configured in Azure AD, your application requests additional access tokens for a consented tenant on an ongoing basis, without the need for further tenant admin interaction.</span></span> <span data-ttu-id="fa1ee-118">这些访问令牌称为“仅应用程序令牌”，因为它们不包含租户管理员的相关信息。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-118">These access tokens are called app-only tokens because they do not include information about the tenant admin.</span></span>
    
4. <span data-ttu-id="fa1ee-119">**调用 Office 365 管理 API**。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-119">**Call the Office 365 Management APIs**.</span></span> <span data-ttu-id="fa1ee-120">仅应用程序访问令牌被传递到 Office 365 管理 API，以验证和授权应用程序。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-120">The app-only access tokens are passed to the Office 365 Management APIs to authenticate and authorize your application.</span></span>
    
<span data-ttu-id="fa1ee-121">下图展示了获取同意和请求获取访问令牌的顺序。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-121">The following diagram shows the sequence of consent and access token requests.</span></span>


![管理 API 入门 - 授权流](images/authorization-flow.png)


## <a name="register-your-application-in-azure-ad"></a><span data-ttu-id="fa1ee-123">在 Azure AD 中注册应用程序</span><span class="sxs-lookup"><span data-stu-id="fa1ee-123">Register your application in Azure AD</span></span>

<span data-ttu-id="fa1ee-124">Office 365 管理 API 使用 Azure AD 提供对 Office 365 租户数据的安全身份验证。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-124">The Office 365 Management APIs use Azure AD to provide secure authentication to Office 365 tenant data.</span></span> <span data-ttu-id="fa1ee-125">若要访问 Office 365 管理 API，必须在 Azure AD 中注册应用程序，并在配置过程中指定应用程序访问 API 所需的权限级别。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-125">To access the Office 365 Management APIs, you need to register your app in Azure AD, and as part of the configuration, you will specify the permission levels your app needs to access the APIs.</span></span>


### <a name="prerequisites"></a><span data-ttu-id="fa1ee-126">先决条件</span><span class="sxs-lookup"><span data-stu-id="fa1ee-126">Prerequisites</span></span>

<span data-ttu-id="fa1ee-127">必须有 Office 365 订阅以及与 Office 365 订阅关联的 Azure 订阅，才能在 Azure AD 中注册应用程序。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-127">To register your app in Azure AD, you need a subscription to Office 365 and a subscription to Azure that has been associated with your Office 365 subscription.</span></span> <span data-ttu-id="fa1ee-128">可以从 Office 365 和 Azure 这两款产品的试用订阅入手。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-128">You can use trial subscriptions to both Office 365 and Azure to get started.</span></span> <span data-ttu-id="fa1ee-129">有关详细信息，请参阅[欢迎加入 Office 365 开发人员计划](https://docs.microsoft.com/zh-CN/office/developer-program/office-365-developer-program)。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-129">For more details, see [Welcome to the Office 365 Developer Program](https://docs.microsoft.com/zh-CN/office/developer-program/office-365-developer-program).</span></span>


### <a name="use-the-azure-management-portal-to-register-your-application-in-azure-ad"></a><span data-ttu-id="fa1ee-130">使用 Azure 管理门户在 Azure AD 中注册应用程序</span><span class="sxs-lookup"><span data-stu-id="fa1ee-130">Use the Azure Management Portal to register your application in Azure AD</span></span>

<span data-ttu-id="fa1ee-131">拥有包含正确订阅的 Microsoft 租户后，便可以在 Azure AD 中注册应用程序了。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-131">After you have a Microsoft tenant with the proper subscriptions, you can register your application in Azure AD.</span></span>

1. <span data-ttu-id="fa1ee-132">使用包含要使用的 Office 365 订阅的 Microsoft 租户的凭据，登录 [Azure 管理门户](https://manage.windowsazure.com/)。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-132">Sign into the [Azure management portal](https://manage.windowsazure.com/), using the credential of your Microsoft tenant that has the subscription to Office 365 you wish to use.</span></span> <span data-ttu-id="fa1ee-133">也可以单击 [Office 管理门户](https://portal.office.com/)左侧导航窗格中的链接，从而访问 Azure 管理门户。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-133">You can also access the Azure Management Portal via a link that appears in the left navigation pane in the [Office admin portal](https://portal.office.com/).</span></span>
    
2. <span data-ttu-id="fa1ee-134">在左侧导航窗格中，选择“Active Directory”（图中的“1”）。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-134">In the left navigation panel, choose Active Directory (1).</span></span> <span data-ttu-id="fa1ee-135">请确保“目录”选项卡（图中的“2”）处于选中状态，再选择目录名称（图中的“3”）。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-135">Make sure the Directory tab (2) is selected, and then select the directory name (3).</span></span>
    
   ![O365 注册页](images/o365-sign-up-page.png)
    
    
3. <span data-ttu-id="fa1ee-137">在目录页上，选择“应用程序”\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-137">On the directory page, select **Applications**.</span></span> <span data-ttu-id="fa1ee-138">此时，Azure AD 会列出当前在租赁中安装的应用程序。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-138">Azure AD displays a list of the applications currently installed in your tenancy.</span></span>
    
4. <span data-ttu-id="fa1ee-139">选择“添加”\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-139">Choose **Add**.</span></span>
    
   ![Office 365 管理页](images/o365-admin-page.png)
    
    
5. <span data-ttu-id="fa1ee-141">选择“添加我的组织正在开发的应用程序”\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-141">Select **Add an application my organization is developing**.</span></span>
    
6. <span data-ttu-id="fa1ee-142">在“名称”\*\*\*\* 字段中输入应用程序名称，并将“类型”\*\*\*\* 指定为“WEB 应用程序和/或 WEB API”。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-142">Enter the **NAME** of your application and specify the **Type** as WEB APPLICATION AND/OR WEB API.</span></span>
    
7. <span data-ttu-id="fa1ee-143">输入相应的应用程序属性：</span><span class="sxs-lookup"><span data-stu-id="fa1ee-143">Enter the appropriate App properties:</span></span>
    
   - <span data-ttu-id="fa1ee-144">**登录 URL**。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-144">**SIGN-ON URL**.</span></span> <span data-ttu-id="fa1ee-145">可供用户登录并使用应用程序的网址。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-145">The URL where users can sign in and use your app.</span></span> <span data-ttu-id="fa1ee-146">稍后可根据需要更改此设置。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-146">You can change this later as needed.</span></span>
    
   - <span data-ttu-id="fa1ee-147">**应用程序 ID URI**。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-147">**APP ID URI**.</span></span> <span data-ttu-id="fa1ee-148">用作应用程序的唯一逻辑标识符的 URI。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-148">The URI used as a unique logical identifier for your app.</span></span> <span data-ttu-id="fa1ee-149">URI 必须位于已验证自定义域中，这样外部用户才能向应用程序授予权限，允许应用程序访问外部用户在 Windows Azure AD 中的数据。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-149">The URI must be in a verified custom domain for an external user to grant your app access to their data in Windows Azure AD.</span></span> <span data-ttu-id="fa1ee-150">例如，如果 Microsoft 租户为 **contoso.onmicrosoft.com**，那么应用程序 ID URI 可以是 **https://app.contoso.onmicrosoft.com**。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-150">For example, if your Microsoft tenant is **contoso.onmicrosoft.com**, the APP ID URI could be **https://app.contoso.onmicrosoft.com**.</span></span>
    
8. <span data-ttu-id="fa1ee-151">此时，应用程序已向 Azure AD 进行注册，并已分配有客户端 ID。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-151">Your app is now registered with Azure AD, and has been assigned a client ID.</span></span> <span data-ttu-id="fa1ee-152">不过，还剩几个重要的应用程序方面需要配置。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-152">However, there are several important aspects of your app left to configure.</span></span>
    

### <a name="configure-your-application-properties-in-azure-ad"></a><span data-ttu-id="fa1ee-153">在 Azure AD 中配置应用程序属性</span><span class="sxs-lookup"><span data-stu-id="fa1ee-153">Configure your application properties in Azure AD</span></span>

<span data-ttu-id="fa1ee-154">至此，应用程序已注册，必须指定几个重要属性，以确定应用程序如何在 Azure AD 中运行，以及租户管理员如何同意允许应用程序使用 Office 365 管理 API 访问管理员租户数据。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-154">Now that your application is registered, there are several important properties you must specify that determine how your application functions within Azure AD and how tenant admins will grant consent to allow your application to access their data by using the Office 365 Management APIs.</span></span>

<span data-ttu-id="fa1ee-155">若要详细了解 Azure AD 应用程序常规配置，请参阅[应用程序对象属性](https://docs.microsoft.com/zh-CN/azure/active-directory/develop/active-directory-application-objects)。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-155">For more information about Azure AD application configuration in general, see [Application Object Properties](https://docs.microsoft.com/zh-CN/azure/active-directory/develop/active-directory-application-objects).</span></span>


1. <span data-ttu-id="fa1ee-156">**客户端 ID**。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-156">**CLIENT ID**.</span></span> <span data-ttu-id="fa1ee-157">此值由 Azure AD 自动生成。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-157">This value is automatically generated by Azure AD.</span></span> <span data-ttu-id="fa1ee-158">应用程序在请求获取租户管理员同意时，以及在从 Azure AD 请求获取仅应用程序令牌时使用此值。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-158">Your application will use this value when requesting consent from tenant admins and when requesting app-only tokens from Azure AD.</span></span>
    
2. <span data-ttu-id="fa1ee-159">**应用程序是多租户的**。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-159">**APPLICATION IS MULTI-TENANT**.</span></span> <span data-ttu-id="fa1ee-160">此属性必须设置为“是”\*\*\*\*，这样租户管理员才能同意允许应用程序使用 Office 365 管理 API 访问管理员租户数据。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-160">This property must be set to **YES** to allow tenant admins to grant consent to your app to access their data by using the Office 365 Management APIs.</span></span> <span data-ttu-id="fa1ee-161">如果此属性设置为“否”\*\*\*\*，应用程序就只能访问你自己租户的数据。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-161">If this property is set to **NO**, your application will only be able to access your own tenant's data.</span></span>
    
3. <span data-ttu-id="fa1ee-162">**回复 URL**。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-162">**REPLY URL**.</span></span> <span data-ttu-id="fa1ee-163">这是租户管理员在同意允许应用程序使用 Office 365 管理 API 访问管理员租户数据后重定向到的 URL。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-163">This is the URL that a tenant admin will be redirected to after granting consent to allow your application to access their data by using the Office 365 Management APIs.</span></span> <span data-ttu-id="fa1ee-164">可根据需要配置多个回复 URL。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-164">You can configure multiple reply URLs as needed.</span></span> <span data-ttu-id="fa1ee-165">Azure 会自动设置第一个回复 URL，以匹配你在创建应用程序时指定的登录 URL，但你可根据需要更改此值。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-165">Azure automatically sets the first one to match the sign-on URL you specified when you created the application, but you can change this value as needed.</span></span>
    
<span data-ttu-id="fa1ee-166">更改这些属性后，请务必选择“保存”\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-166">Be sure to choose **Save** after making any changes to these properties.</span></span>


### <a name="generate-a-new-key-for-your-application"></a><span data-ttu-id="fa1ee-167">生成应用程序的新密钥</span><span class="sxs-lookup"><span data-stu-id="fa1ee-167">Generate a new key for your application</span></span>

<span data-ttu-id="fa1ee-168">用授权代码交换访问令牌时会用到密钥（亦称为“客户端密码”）。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-168">Keys, also known as client secrets, are used when exchanging an authorization code for an access token.</span></span>


1. <span data-ttu-id="fa1ee-169">在 Azure 管理门户中，选择应用程序并在顶部菜单中选择**配置**。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-169">In the Azure Management Portal, select your application and choose **Configure** in the top menu.</span></span> <span data-ttu-id="fa1ee-170">向下滚动到**密钥**。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-170">Scroll down to **keys**.</span></span>
    
2. <span data-ttu-id="fa1ee-171">依次选择密钥期限和“保存”\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-171">Select the duration for your key, and choose **Save**.</span></span>
    
   ![Azure 订阅页](images/azure-subscription-page.png)
    
    
3. <span data-ttu-id="fa1ee-173">Azure 仅显示保存后的应用程序密码。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-173">Azure displays the app secret only after saving it.</span></span> <span data-ttu-id="fa1ee-174">选择“剪贴板”图标，将客户端密码复制到剪贴板中。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-174">Select the Clipboard icon to copy the client secret to the Clipboard.</span></span>
    
   ![Azure 门户页](images/azure-portal-page.png)

   > [!IMPORTANT] 
   > <span data-ttu-id="fa1ee-176">Azure 仅在客户端密码最初生成时才显示它。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-176">Azure only displays the client secret at the time you initially generate it.</span></span> <span data-ttu-id="fa1ee-177">无法返回到此页，因此稍后也无法检索客户端密码。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-177">You cannot navigate back to this page and retrieve the client secret later.</span></span>

### <a name="configure-an-x509-certificate-to-enable-service-to-service-calls"></a><span data-ttu-id="fa1ee-178">将 X.509 证书配置为启用服务到服务调用</span><span class="sxs-lookup"><span data-stu-id="fa1ee-178">Configure an X.509 certificate to enable service-to-service calls</span></span>

<span data-ttu-id="fa1ee-179">在后台运行的应用程序（如守护程序或服务）可使用客户端凭据，以请求获取仅应用程序访问令牌，而无需在获取初始同意后重复请求获取租户管理员同意。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-179">An application that is running in the background, such as a daemon or service, can use client credentials to request app-only access tokens without repeatedly requesting consent from the tenant admin after initial consent is granted.</span></span> 

<span data-ttu-id="fa1ee-180">有关详细信息，请参阅[使用客户端凭据执行服务到服务调用](https://msdn.microsoft.com/zh-CN/library/azure/dn645543.aspx)。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-180">For more information, see [Service to Service Calls Using Client Credentials](https://msdn.microsoft.com/zh-CN/library/azure/dn645543.aspx).</span></span>

<span data-ttu-id="fa1ee-181">必须为应用程序配置 X.509 证书，以用作在从 Azure AD 请求获取仅应用程序访问令牌时所需的客户端凭据。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-181">You must configure an X.509 certificate with your application to be used as client credentials when requesting app-only access tokens from Azure AD.</span></span> <span data-ttu-id="fa1ee-182">此过程分为两步：</span><span class="sxs-lookup"><span data-stu-id="fa1ee-182">There are two steps to the process:</span></span>

- <span data-ttu-id="fa1ee-183">获取 X.509 证书。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-183">Obtain an X.509 certificate.</span></span> <span data-ttu-id="fa1ee-184">可使用自签名证书或由公共可信的证书颁发机构颁发的证书。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-184">You can use a self-signed certificate or a certificate issued by publicly trusted certificate authority.</span></span>
    
- <span data-ttu-id="fa1ee-185">将应用程序清单修改为，包含证书的指纹和公钥。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-185">Modify your application manifest to include the thumbprint and public key of your certificate.</span></span>
    
<span data-ttu-id="fa1ee-186">下面介绍了如何使用 Visual Studio 或 Windows SDK _makecert_ 工具生成自签名证书，并将公钥导出到经过 base64 编码的文件中。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-186">The following instructions show you how to use the Visual Studio or Windows SDK _makecert_ tool to generate a self-signed certificate and export the public key to a base64-encoded file.</span></span>


1. <span data-ttu-id="fa1ee-187">在命令行中运行下面的命令：</span><span class="sxs-lookup"><span data-stu-id="fa1ee-187">From the command line, run the following:</span></span>
    
   ```
    makecert -r -pe -n "CN=MyCompanyName MyAppName Cert" -b 03/15/2015 -e 03/15/2017 -ss my -len 2048
   ```

   > [!NOTE] 
   > <span data-ttu-id="fa1ee-188">生成 X.509 证书时，请确保密钥长度至少为 2048。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-188">When you are generating the X.509 certificate, make sure the key length is at least 2048.</span></span> <span data-ttu-id="fa1ee-189">无法将长度较短的密钥接受为有效密钥。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-189">Shorter key lengths are not accepted as valid keys.</span></span>

2. <span data-ttu-id="fa1ee-190">打开证书 MMC 管理单元，并连接到用户帐户。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-190">Open the Certificates MMC snap-in and connect to your user account.</span></span> 
    
3. <span data-ttu-id="fa1ee-191">在个人文件夹中查找新证书，并将公钥导出到经过 base64 编码的文件（例如，mycompanyname.cer）中。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-191">Find the new certificate in the Personal folder and export the public key to a base64-encoded file (for example, mycompanyname.cer).</span></span> <span data-ttu-id="fa1ee-192">由于应用程序将使用此证书与 Azure AD 进行通信，因此请确保你也保留对私钥的访问权限。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-192">Your application will use this certificate to communicate with Azure AD, so make sure you retain access to the private key as well.</span></span>
    
   > [!NOTE] 
   > <span data-ttu-id="fa1ee-193">可使用 Windows PowerShell 提取指纹和经过 base64 编码的公钥。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-193">You can use Windows PowerShell to extract the thumbprint and base64-encoded public key.</span></span> <span data-ttu-id="fa1ee-194">其他平台提供了用于检索证书属性的类似工具。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-194">Other platforms provide similar tools to retrieve properties of certificates.</span></span>

4. <span data-ttu-id="fa1ee-195">在 Windows PowerShell 提示符中键入并运行下面的命令：</span><span class="sxs-lookup"><span data-stu-id="fa1ee-195">From the Windows PowerShell prompt, type and run the following:</span></span>
    
   ```powershell
    $cer = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2
    $cer.Import("mycer.cer")
    $bin = $cer.GetRawCertData()
    $base64Value = [System.Convert]::ToBase64String($bin)
    $bin = $cer.GetCertHash()
    $base64Thumbprint = [System.Convert]::ToBase64String($bin)
    $keyid = [System.Guid]::NewGuid().ToString()
   ```

5. <span data-ttu-id="fa1ee-196">存储 `$base64Thumbprint`、`$base64Value` 和 `$keyid` 的值，以供在下一组步骤中更新应用程序清单时使用。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-196">Store the values for `$base64Thumbprint`, `$base64Value`, and `$keyid` to be used when you update your application manifest in the next set of steps.</span></span>
    
   <span data-ttu-id="fa1ee-197">使用从证书中提取的值和生成的密钥 ID，现在必须在 Azure AD 中更新应用程序清单。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-197">Using the values extracted from the certificate and the generated key ID, you must now update your application manifest in Azure AD.</span></span>
    
6. <span data-ttu-id="fa1ee-198">在 Azure 管理门户中，依次选择应用程序和顶部菜单中的“配置”\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-198">In the Azure Management Portal, select your application and choose **Configure** in the top menu.</span></span>
    
7. <span data-ttu-id="fa1ee-199">在命令栏中，依次选择“管理清单”\*\*\*\* 和“下载清单”\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-199">In the command bar, choose **Manage manifest**, and then choose **Download Manifest**.</span></span>
    
   ![命令行证书显示](images/command-line-certificate-display.png)
    
    
8. <span data-ttu-id="fa1ee-201">打开下载的清单进行编辑，并将空的 KeyCredentials 属性替换为以下 JSON：</span><span class="sxs-lookup"><span data-stu-id="fa1ee-201">Open the downloaded manifest for editing and replace the empty KeyCredentials property with the following JSON:</span></span>
    
   ```json
      "keyCredentials": [
        {
            "customKeyIdentifier" : "$base64Thumbprint_from_above",
            "keyId": "$keyid_from_above",
            "type": "AsymmetricX509Cert",
            "usage": "Verify",
            "value": "$base64Value_from_above"
        }
    ],
   ```


   > [!NOTE] 
   > <span data-ttu-id="fa1ee-202">[KeyCredentials](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#KeyCredentialType) 属性是一个集合，让为滚动更新方案上传多个 X.509 证书，或者为泄漏方案删除证书成为可能。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-202">The [KeyCredentials](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#KeyCredentialType) property is a collection, making it possible to upload multiple X.509 certificates for rollover scenarios or delete certificates for compromise scenarios.</span></span>

9. <span data-ttu-id="fa1ee-203">保存更改，并依次选择命令栏中的“管理清单”\*\*\*\* 和“上传清单”\*\*\*\*，再浏览并选择更新后的清单文件以上传。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-203">Save your changes and upload the updated manifest by choosing **Manage manifest** in the command bar, choosing **Upload manifest**, browsing to your updated manifest file, and then selecting it.</span></span>
    

### <a name="specify-the-permissions-your-app-requires-to-access-the-office-365-management-apis"></a><span data-ttu-id="fa1ee-204">指定应用程序访问 Office 365 管理 API 所需的权限</span><span class="sxs-lookup"><span data-stu-id="fa1ee-204">Specify the permissions your app requires to access the Office 365 Management APIs</span></span>

<span data-ttu-id="fa1ee-205">最后，需要指定应用程序访问 Office 365 管理 API 所需的确切权限。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-205">Finally, you need to specify exactly what permissions your app requires of the Office 365 Management APIs.</span></span> <span data-ttu-id="fa1ee-206">为此，请先向应用程序添加对 Office 365 管理 API 的访问权限，再指定所需的一项或多项权限。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-206">To do so, you add access to the Office 365 Management APIs to your app, and then you specify the permission(s) you need.</span></span>


1. <span data-ttu-id="fa1ee-207">在 Azure 管理门户中，依次选择应用程序和顶部菜单中的“配置”\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-207">In the Azure Management Portal, select your application, and choose **Configure** in the top menu.</span></span> <span data-ttu-id="fa1ee-208">向下滚动到“对其他应用程序的权限”\*\*\*\*，并选择“添加应用程序”\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-208">Scroll down to **permissions to other applications**, and choose **Add application**.</span></span>
    
   ![Azure AD 页](images/azure-ad-page.png)
    
    
2. <span data-ttu-id="fa1ee-210">选择“Office 365 管理 API”\*\*\*\*（图中的“1”），让它显示在“已选择”\*\*\*\* 列（图中的“2”）中，再选择右下角的复选标记（图中的“3”），以保存选择并返回到应用程序的主配置页。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-210">Select the **Office 365 Management APIs** (1) so that it appears in the **Selected** column (2), and then select the check mark in the lower right (3) to save your selection and return to the main configuration page for your application.</span></span>
    
   ![Azure AD 应用程序页](images/azure-ad-apps-page.png)
    
    
3. <span data-ttu-id="fa1ee-212">此时，Office 管理 API 显示在应用程序需要有权访问的应用程序列表中。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-212">The Office Management APIs now appear in the list of applications to which your application requires permissions.</span></span> <span data-ttu-id="fa1ee-213">在“应用程序权限”\*\*\*\* 和“委托权限”\*\*\*\* 下，选择应用程序需要的权限。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-213">Under both **Application Permissions** and **Delegated Permissions**, select the permissions your application requires.</span></span> <span data-ttu-id="fa1ee-214">若要详细了解每项权限，请参阅具体的 API 参考。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-214">Refer to the specific API reference for more details about each permission.</span></span>  

   > [!NOTE] 
   > <span data-ttu-id="fa1ee-215">目前有四项未用权限与今后即将弃用的活动报告和威胁智能相关。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-215">There are currently four unused permissions related to activity reports and threat intelligence that will be removed in the future.</span></span> <span data-ttu-id="fa1ee-216">请勿选择其中任何权限，因为它们是不必要的。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-216">Do not select any of these permissions because they are unnecessary.</span></span>
    
   ![“添加应用程序”对话框](images/add-an-application-dialog.png)
    
    
4. <span data-ttu-id="fa1ee-218">选择“保存”\*\*\*\*，以保存配置。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-218">Choose **Save** to save the configuration.</span></span>
    

## <a name="get-office-365-tenant-admin-consent"></a><span data-ttu-id="fa1ee-219">获取 Office 365 租户管理员同意</span><span class="sxs-lookup"><span data-stu-id="fa1ee-219">Get Office 365 tenant admin consent</span></span>

<span data-ttu-id="fa1ee-220">至此，应用程序已配置有使用 Office 365 管理 API 所需的权限。租户管理员必须显式授予应用程序这些权限，应用程序才能使用 API 访问管理员租户数据。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-220">Now that your application is configured with the permissions it needs to use the Office 365 Management APIs, a tenant admin must explicitly grant your application these permissions in order to access their tenant's data by using the APIs.</span></span> <span data-ttu-id="fa1ee-221">若要授予同意，租户管理员必须使用以下专门构造的 URL 登录 Azure AD，以查看应用程序请求获取的权限。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-221">To grant consent, the tenant admin must sign in to Azure AD by using the following specially constructed URL, where they can review your application's requested permissions.</span></span> <span data-ttu-id="fa1ee-222">如果使用 API 访问你自己租户的数据，便无需执行这一步。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-222">This step is not required when using the APIs to access data from your own tenant.</span></span>


```http
https://login.windows.net/common/oauth2/authorize?response_type=code&resource=https%3A%2F%2Fmanage.office.com&client_id={your_client_id}&redirect_uri={your_redirect_url }
```

<span data-ttu-id="fa1ee-223">重定向 URL 必须与在 Azure AD 中为应用程序配置的回复 URL 之一一致，或是其子路径。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-223">The redirect URL must match or be a sub-path under one of the Reply URLs configured for your application in Azure AD.</span></span>

<span data-ttu-id="fa1ee-224">例如：</span><span class="sxs-lookup"><span data-stu-id="fa1ee-224">For example:</span></span>

```http
https://login.windows.net/common/oauth2/authorize?response_type=code&resource=https%3A%2F%2Fmanage.office.com&client_id=2d4d11a2-f814-46a7-890a-274a72a7309e&redirect_uri=http%3A%2F%2Fwww.mycompany.com%2Fmyapp%2F
```

<span data-ttu-id="fa1ee-225">可测试同意 URL，具体方法为将同意 URL 粘贴到浏览器中，并使用除用于注册应用程序的租户以外的租户的 Office 365 管理员凭据登录。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-225">You can test the consent URL by pasting it into a browser and signing in using the credentials of an Office 365 admin for a tenant other than the tenant that you used to register the application.</span></span> <span data-ttu-id="fa1ee-226">此时，可看到向应用程序授予 Office 管理 API 使用权限的请求。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-226">You will see the request to grant your application permission to use the Office Management APIs.</span></span>


![Azure AD 已添加应用程序页](images/azure-ad-app-added-page.png)

<span data-ttu-id="fa1ee-228">选择“接受”\*\*\*\* 后，便会重定向到指定页，并且查询字符串中会有授权代码。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-228">After choosing **Accept**, you are redirected to the specified page, and there will be a code in the query string.</span></span> 

<span data-ttu-id="fa1ee-229">例如：</span><span class="sxs-lookup"><span data-stu-id="fa1ee-229">For example:</span></span>

```http
http://www.mycompany.com/myapp/?code=AAABAAAAvPM1KaPlrEqdFSB...
```

<span data-ttu-id="fa1ee-230">应用程序使用此授权代码，从 Azure AD 获取可从中提取租户 ID 的访问令牌。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-230">Your application uses this authorization code to obtain an access token from Azure AD, from which the tenant ID can be extracted.</span></span> <span data-ttu-id="fa1ee-231">提取并存储租户 ID 后，可获取后续访问令牌，而无需请求租户管理员登录。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-231">After you have extracted and stored the tenant ID, you can obtain subsequent access tokens without requiring the tenant admin to sign in.</span></span>


## <a name="request-access-tokens-from-azure-ad"></a><span data-ttu-id="fa1ee-232">从 Azure AD 请求获取访问令牌</span><span class="sxs-lookup"><span data-stu-id="fa1ee-232">Request access tokens from Azure AD</span></span>

<span data-ttu-id="fa1ee-233">从 Azure AD 请求获取访问令牌的方法有以下两种：</span><span class="sxs-lookup"><span data-stu-id="fa1ee-233">There are two methods for requesting access tokens from Azure AD:</span></span>

- <span data-ttu-id="fa1ee-234">
  [授权代码授予流](https://msdn.microsoft.com/en-us/library/azure/dn645542.aspx)：让租户管理员明确授予同意，以向应用程序返回授权代码。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-234">The [Authorization Code Grant Flow](https://msdn.microsoft.com/en-us/library/azure/dn645542.aspx) involves a tenant admin granting explicit consent, which returns an authorization code to your application.</span></span> <span data-ttu-id="fa1ee-235">然后，应用程序用授权代码交换访问令牌。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-235">Your application then exchanges the authorization code for an access token.</span></span> <span data-ttu-id="fa1ee-236">必须使用这种方法，才能获取应用程序使用 API 访问租户数据所需的初始同意；并且必须有第一个访问令牌，才能获取并存储租户 ID。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-236">This method is required to obtain the initial consent that your application needs to access the tenant data by using the API, and this first access token is needed in order to obtain and store the tenant ID.</span></span>
    
- <span data-ttu-id="fa1ee-237">[客户端凭据授予流](https://msdn.microsoft.com/zh-CN/library/azure/dn645543.aspx)：允许应用程序在旧版访问令牌到期时请求获取后续访问令牌，而无需请求租户管理员登录并明确授予同意。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-237">The [Client Credentials Grant Flow](https://msdn.microsoft.com/zh-CN/library/azure/dn645543.aspx) allows your application to request subsequent access tokens as old ones expire, without requiring the tenant admin to sign in and explicitly grant consent.</span></span> <span data-ttu-id="fa1ee-238">必须对在初始租户管理员同意授予后在后台连续运行并调用 API 的应用程序使用这种方法。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-238">This method must be used for applications that run continuously in the background calling the APIs once the initial tenant admin consent has been granted.</span></span>
    

### <a name="request-an-access-token-using-the-authorization-code"></a><span data-ttu-id="fa1ee-239">使用授权代码请求获取访问令牌</span><span class="sxs-lookup"><span data-stu-id="fa1ee-239">Request an access token using the authorization code</span></span>

<span data-ttu-id="fa1ee-240">在租户管理员同意后，应用程序会收到授权代码，它是以 Azure AD 将租户管理员重定向到指定 URL 时显示的查询字符串参数出现。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-240">After a tenant admin grants consent, your application receives an authorization code as a query string parameter when Azure AD redirects the tenant admin to your designated URL.</span></span>

```http
http://www.mycompany.com/myapp/?code=AAABAAAAvPM1KaPlrEqdFSB...
```

<span data-ttu-id="fa1ee-241">应用程序向 Azure AD 发出 HTTP REST POST，以用授权代码交换访问令牌。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-241">Your application makes an HTTP REST POST to Azure AD to exchange the authorization code for an access token.</span></span> <span data-ttu-id="fa1ee-242">由于租户 ID 尚未知，因此 POST 会被发送到“常用”终结点，即 URL 中没有嵌入租户 ID：</span><span class="sxs-lookup"><span data-stu-id="fa1ee-242">Because the tenant ID is not yet known, the POST will be to the "common" endpoint, which does not have the tenant ID embedded in the URL:</span></span>

```http
https://login.windows.net/common/oauth2/token
```

<span data-ttu-id="fa1ee-243">POST 正文包含以下内容：</span><span class="sxs-lookup"><span data-stu-id="fa1ee-243">The body of the POST contains the following:</span></span>

```json
resource=https%3A%2F%2Fmanage.office.com&amp;client_id=a6099727-6b7b-482c-b509-1df309acc563 &amp;redirect_uri= http%3A%2F%2Fwww.mycompany.com%2Fmyapp%2F &amp;client_secret={your_client_key}&amp;grant_type=authorization_code&amp;code= AAABAAAAvPM1KaPlrEqdFSB...
```

#### <a name="sample-request"></a><span data-ttu-id="fa1ee-244">示例请求</span><span class="sxs-lookup"><span data-stu-id="fa1ee-244">Sample request</span></span>

```json
POST https://login.windows.net/common/oauth2/token HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Host: login.windows.net
Content-Length: 944

resource=https%3A%2F%2Fmanage.office.com&amp;client_id=a6099727-6b7b-482c-b509-1df309acc563 &amp;redirect_uri= http%3A%2F%2Fwww.mycompany.com%2Fmyapp%2F &amp;client_secret={your_client_key}&amp;grant_type=authorization_code&amp;code=AAABAAAAvPM1KaPlrEqdFSB...
```

<br/>

<span data-ttu-id="fa1ee-245">响应正文包含多个属性，包括访问令牌。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-245">The body of the response will include several properties, including the access token.</span></span> 

#### <a name="sample-response"></a><span data-ttu-id="fa1ee-246">示例响应</span><span class="sxs-lookup"><span data-stu-id="fa1ee-246">Sample response</span></span>

```json
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Content-Length: 3265

{"expires_in":"3599","token_type":"Bearer","scope":"ActivityFeed.Read ActivityReports.Read ServiceHealth.Read","expires_on":"1438290275","not_before":"1438286375","resource":"https://manage.office.com","access_token":"eyJ0eX...","refresh_token":"AAABAAA...","id_token":"eyJ0eXAi..."}
```

<span data-ttu-id="fa1ee-247">返回的访问令牌是 JWT 令牌，其中既包含授予同意的管理员的相关信息，也包含请求获取访问权限的应用程序的相关信息。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-247">The access token that is returned is a JWT token that includes information about both the admin that granted consent and the application requesting access.</span></span> <span data-ttu-id="fa1ee-248">下面的示例展示了未编码的令牌。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-248">The following shows an example of an un-encoded token.</span></span> <span data-ttu-id="fa1ee-249">应用程序必须从此令牌中提取并存储租户 ID “tid”，这样才能在访问令牌到期时用它请求获取其他访问令牌，而无需与管理员有进一步交互。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-249">Your application must extract the tenant ID "tid" from this token and store it so that it can be used to request additional access tokens as they expire, without further admin interaction.</span></span>

#### <a name="sample-token"></a><span data-ttu-id="fa1ee-250">示例令牌</span><span class="sxs-lookup"><span data-stu-id="fa1ee-250">Sample token</span></span>

```json
{
  "aud": "https://manage.office.com",
  "iss": "https://sts.windows.net/41463f53-8812-40f4-890f-865bf6e35190/",
  "iat": 1427246416,
  "nbf": 1427246416,
  "exp": 1427250316,
  "ver": "1.0",
  "tid": "41463f53-8812-40f4-890f-865bf6e35190",
  "amr": [
    "pwd"
  ],
  "oid": "1cef1fdb-ff52-48c4-8e4e-dfb5ea83d357",
  "upn": "admin@contoso.onmicrosoft.com",
  "puid": "1003BFFD8EC47CA6",
  "sub": "7XpD5OWAXM1OWmKiVKh1FOkKXV4N3OSRol6mz1pxxhU",
  "given_name": "John",
  "family_name": "Doe",
  "name": "Contoso, Inc.",
  "unique_name": "admin@contoso.onmicrosoft.com",
  "appid": "a6099727-6b7b-482c-b509-1df309acc563",
  "appidacr": "1",
  "scp": "ActivityFeed.Read ServiceHealth.Read",
  "acr": "1"
}
```


### <a name="request-an-access-token-by-using-client-credentials"></a><span data-ttu-id="fa1ee-251">使用客户端凭据请求获取访问令牌</span><span class="sxs-lookup"><span data-stu-id="fa1ee-251">Request an access token by using client credentials</span></span>

<span data-ttu-id="fa1ee-252">在租户 ID 已知后，应用程序便可以对 Azure AD 执行服务到服务调用，以在访问令牌到期时请求获取其他访问令牌。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-252">After the tenant ID is known, your application can make service-to-service calls to Azure AD to request additional access tokens as they expire.</span></span> <span data-ttu-id="fa1ee-253">这些令牌仅包含发出请求的应用程序的相关信息，并不包含最初授予同意的管理员的相关信息。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-253">These tokens include information only about the requesting application and not about the admin that originally granted consent.</span></span> <span data-ttu-id="fa1ee-254">若要执行服务到服务调用，应用程序必须使用 X.509 证书创建客户端断言（形式为经过 base64 编码的 SHA256 签名 JWT 持有者令牌）。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-254">Service-to-service calls require that your application use an X.509 certificate to create client assertion in the form of a base64-encoded, SHA256 signed JWT bearer token.</span></span>

<span data-ttu-id="fa1ee-255">在 .NET 中开发应用程序时，可使用 [Azure AD 身份验证库 (ADAL)](https://docs.microsoft.com/zh-CN/azure/active-directory/develop/active-directory-authentication-libraries) 创建客户端断言。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-255">When you are developing your application in .NET, you can use the [Azure AD Authentication Library (ADAL)](https://docs.microsoft.com/zh-CN/azure/active-directory/develop/active-directory-authentication-libraries) to create client assertions.</span></span> <span data-ttu-id="fa1ee-256">其他开发平台应该有类似的库。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-256">Other development platforms should have similar libraries.</span></span>

<span data-ttu-id="fa1ee-257">未编码的 JWT 令牌由具有以下属性的头和有效负载组成。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-257">An un-encoded JWT token consists of a header and payload that have the following properties.</span></span>

```json
HEADER:

{
  "alg": "RS256",
  "x5t": "{thumbprint of your X.509 certificate used to sign the token",
}

PAYLOAD:

{
  "aud": "https://login.windows.net/{tenantid}/oauth2/token",
  "iss": "{your app client ID}",
  "sub": "{your app client ID}"
  "jti": "{random GUID}",
  "nbf": {epoch time, before which the token is not valid},
  "exp": {epoch time, after which the token is not valid},
}

```

#### <a name="sample-jwt-token"></a><span data-ttu-id="fa1ee-258">示例 JWT 令牌</span><span class="sxs-lookup"><span data-stu-id="fa1ee-258">Sample JWT token</span></span>


```json
HEADER:

{
  "alg": "RS256",
  "x5t": "YyfshJC3rPQ-kpGo5dUaiY5t3iU",
}

PAYLOAD:

{
  "aud": "https://login.windows.net/41463f53-8812-40f4-890f-865bf6e35190/oauth2/token",
  "iss": "a6099727-6b7b-482c-b509-1df309acc563",
  "sub": "a6099727-6b7b-482c-b509-1df309acc563"
  "jti": "0ce254c4-81b1-4a2e-8436-9a8c3b49dfb9",
  "nbf": 1427248048,
  "exp": 1427248648,
}
```

<span data-ttu-id="fa1ee-259">然后，在执行服务到服务调用以请求获取访问令牌的过程中，将客户端断言传递到 Azure AD。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-259">The client assertion is then passed to Azure AD as part of a service-to-service call to request an access token.</span></span> <span data-ttu-id="fa1ee-260">使用客户端凭据请求获取访问令牌时，请将 HTTP POST 发送到租户专用终结点，即 URL 中已嵌入先前提取和存储的租户 ID。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-260">When using client credentials to request an access token, use an HTTP POST to a tenant-specific endpoint, where the previously extracted and stored tenant ID is embedded in the URL.</span></span>


```http
https://login.windows.net/{tenantid}/oauth2/token
```

<span data-ttu-id="fa1ee-261">POST 正文包含以下内容：</span><span class="sxs-lookup"><span data-stu-id="fa1ee-261">The body of the POST contains the following:</span></span>


```json
resource=https%3A%2F%2Fmanage.office.com&amp;client_id={your_app_client_id}&amp;grant_type=client_credentials&amp;client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer&amp;client_assertion={encoded_signed_JWT_token}
```

#### <a name="sample-request"></a><span data-ttu-id="fa1ee-262">示例请求</span><span class="sxs-lookup"><span data-stu-id="fa1ee-262">Sample request</span></span>

```json
POST https://login.windows.net/41463f53-8812-40f4-890f-865bf6e35190/oauth2/token HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Host: login.windows.net
Content-Length: 994

resource=https%3A%2F%2Fmanage.office.com&amp;client_id= a6099727-6b7b-482c-b509-1df309acc563&amp;grant_type=client_credentials &amp;client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer&amp;client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Ill5ZnNoSkMzclBRLWtwR281ZFVhaVk1dDNpVSJ9.eyJhdWQiOiJodHRwczpcL1wvbG9naW4ud2luZG93cy5uZXRcLzQxNDYzZjUzLTg4MTItNDBmNC04OTBmLTg2NWJmNmUzNTE5MFwvb2F1dGgyXC90b2tlbiIsImV4cCI6MTQyNzI0ODY0OCwiaXNzIjoiYTYwOTk3MjctNmI3Yi00ODJjLWI1MDktMWRmMzA5YWNjNTYzIiwianRpIjoiMGNlMjU0YzQtODFiMS00YTJlLTg0MzYtOWE4YzNiNDlkZmI5IiwibmJmIjoxNDI3MjQ4MDQ4LCJzdWIiOiJhNjA5OTcyNy02YjdiLTQ4MmMtYjUwOS0xZGYzMDlhY2M1NjMifQ.vfDrmCjiXgoj2JrTkwyOpr-NOeQTzlXQcGlKGNpLLe0oh4Zvjdcim5C7E0UbI3Z2yb9uKQdx9G7GeqS-gVc9kNV_XSSNP4wEQj3iYNKpf_JD2ikUVIWBkOg41BiTuknRJAYOMjiuBE2a6Wyk-vPCs_JMd7Sr-N3LiNZ-TjluuVzWHfok_HWz_wH8AzdoMF3S0HtrjNd9Ld5eI7MVMt4OTpRfh-Syofi7Ow0HN07nKT5FYeC_ThBpGiIoODnMQQtDA2tM7D3D6OlLQRgLfI8ir73PVXWL7V7Zj2RcOiooIeXx38dvuSwYreJYtdphmrDBZ2ehqtduzUZhaHL1iDvLlw
```

<span data-ttu-id="fa1ee-263">响应与之前几乎相同，区别在于令牌不包含相同的属性，因为它不包含授予同意的管理员的属性。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-263">The response will be the same as before, but the token will not have the same properties, because it does not contain properties of the admin that granted consent.</span></span> 

#### <a name="sample-response"></a><span data-ttu-id="fa1ee-264">示例响应</span><span class="sxs-lookup"><span data-stu-id="fa1ee-264">Sample response</span></span>

```json
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Content-Length: 1276

{"token_type":"Bearer","expires_in":"3599","expires_on":"1431659094","not_before":"1431655194","resource":"https://manage.office.com","access_token":"eyJ0eXAiOiJKV1QiL..."}
```

#### <a name="sample-access-token"></a><span data-ttu-id="fa1ee-265">示例访问令牌</span><span class="sxs-lookup"><span data-stu-id="fa1ee-265">Sample access token</span></span>

```json
{
  "aud": "https://manage.office.com",
  "iss": "https://sts.windows.net/41463f53-8812-40f4-890f-865bf6e35190/",
  "iat": 1431655194,
  "nbf": 1431655194,
  "exp": 1431659094,
  "ver": "1.0",
  "tid": "41463f53-8812-40f4-890f-865bf6e35190",
  "roles": [
    "ServiceHealth.Read",
    "ActivityFeed.Read"
  ],
  "oid": "67cb0334-e242-4783-8028-0f39132fb5ad",
  "sub": "67cb0334-e242-4783-8028-0f39132fb5ad",
  "idp": "https://sts.windows.net/41463f53-8812-40f4-890f-865bf6e35190/",
  "appid": "a6099727-6b7b-482c-b509-1df309acc563",
  "appidacr": "1"
}
```


## <a name="build-your-app"></a><span data-ttu-id="fa1ee-266">生成应用程序</span><span class="sxs-lookup"><span data-stu-id="fa1ee-266">Build your app</span></span>

<span data-ttu-id="fa1ee-267">至此，已在 Azure AD 中注册了应用程序，并为应用程序配置了必要的权限，可以生成应用程序了。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-267">Now that you have registered your app in Azure AD and configured it with the necessary permissions, you're ready to build your app.</span></span> <span data-ttu-id="fa1ee-268">以下是设计和生成应用程序时需要注意的一些关键方面：</span><span class="sxs-lookup"><span data-stu-id="fa1ee-268">The following are some of the key aspects to consider when designing and building your app:</span></span>

- <span data-ttu-id="fa1ee-269">**同意体验**。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-269">**The consent experience**.</span></span> <span data-ttu-id="fa1ee-270">若要获取客户同意，必须在浏览器中使用前面所述的专门构造 URL 将客户定向到 Azure AD 网站，并且必须有 Azure AD 在管理员授予同意后将他们重定向到的网站。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-270">To obtain consent from your customers, you must direct them in a browser to the Azure AD website, using the specially constructed URL described previously, and you must have a website to which Azure AD will redirect the admin once they grant consent.</span></span> <span data-ttu-id="fa1ee-271">此网站必须从 URL 中提取授权代码，并用它来请求获取可从中获取租户 ID 的访问令牌。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-271">This website must extract the authorization code from the URL and use it to request an access token from which it can obtain the tenant ID.</span></span>
    
- <span data-ttu-id="fa1ee-272">**将租户 ID 存储在系统中**。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-272">**Store the tenant ID in your system**.</span></span> <span data-ttu-id="fa1ee-273">从 Azure AD 请求获取访问令牌和调用 Office 管理 API 时，都必须这样做。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-273">This will be needed when requesting access tokens from Azure AD and when calling the Office Management APIs.</span></span>
    
- <span data-ttu-id="fa1ee-274">**管理访问令牌**。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-274">**Managing access tokens**.</span></span> <span data-ttu-id="fa1ee-275">需要有在必要时请求获取和管理访问令牌的组件。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-275">You will need a component that requests and manages access tokens as needed.</span></span> <span data-ttu-id="fa1ee-276">如果应用程序定期调用 API，可按需请求获取令牌；或如果应用程序连续调用 API 来检索数据，可定期（例如，每 45 分钟）请求获取一次令牌。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-276">If your app calls the APIs periodically, it can request tokens on demand, or if it calls the APIs continuously to retrieve data, it can request tokens at regular intervals (for example, every 45 minutes).</span></span>
    
- <span data-ttu-id="fa1ee-277">根据要使用的特定 API 的需要，**实现 Webhook 侦听器**。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-277">**Implement a webhook listener** as needed by the particular API you are using.</span></span>
    
- <span data-ttu-id="fa1ee-278">**数据检索和存储**。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-278">**Data retrieval and storage**.</span></span> <span data-ttu-id="fa1ee-279">需要有检索每个租户的数据的组件，检索方式可以是使用连续轮询，也可以是响应 Webhook 通知，具体视要使用的特定 API 而定。</span><span class="sxs-lookup"><span data-stu-id="fa1ee-279">You'll need a component that retrieves data for each tenant, either by using continuous polling or in response to webhook notifications, depending on the particular API you are using.</span></span>
    
