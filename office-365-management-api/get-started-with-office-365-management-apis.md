---
ms.TocTitle: Get started with Office 365 Management APIs
title: Office 365 管理 API 入门
description: API 使用 Azure AD 提供身份验证服务，这样你便可使用这些服务向应用程序授予对 API 的访问权限。
ms.ContentId: 74137c9a-29e0-b588-6122-26f4d2c5e3fc
ms.topic: reference (API)
ms.date: 09/05/2018
ms.openlocfilehash: 87abfbe89f60710dc6c9981fa57bde4238790b1f
ms.sourcegitcommit: 525c0d0e78cc44ea8cb6a4bdce1858cb4ef91d57
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2018
ms.locfileid: "25834794"
---
# <a name="get-started-with-office-365-management-apis"></a>Office 365 管理 API 入门

如果创建需要访问 Office 365 管理 API 等安全服务的应用程序，必须设法让安全服务知道应用程序是否有权访问它。 Office 365 管理 API 使用 Azure AD 提供身份验证服务，这样你便可使用这些服务向应用程序授予对 API 的访问权限。 

有四个关键步骤：

1. **在 Azure AD 中注册应用程序**。 为了让应用程序有权访问 Office 365 管理 API，必须在 Azure AD 中注册应用程序。 这样，便可为应用程序建立标识，并指定它访问 API 所需的权限级别。
    
2. **获取 Office 365 租户管理员同意**。 Office 365 租户管理员必须明确同意允许应用程序通过 Office 365 管理 API 访问管理员租户数据。 同意过程是一种基于浏览器的体验，需要租户管理员先登录 **Azure AD 同意 UI**，并检查应用程序请求获取的访问权限，再同意或拒绝请求。 在同意授予后，UI 便会将用户重定向回应用程序，此时 URL 中包含授权代码。 应用程序对 Azure AD 执行服务到服务调用，以用此授权代码交换包含租户管理员和应用程序相关信息的访问令牌。 必须从访问令牌中提取并存储租户 ID，以供日后使用。
    
3. **从 Azure AD 请求获取访问令牌**。 应用程序使用在 Azure AD 中配置的应用程序凭据，持续不断地请求获取适用于已同意租户的其他访问令牌，而无需与租户管理员有进一步交互。 这些访问令牌称为“仅应用程序令牌”，因为它们不包含租户管理员的相关信息。
    
4. **调用 Office 365 管理 API**。 仅应用程序访问令牌被传递到 Office 365 管理 API，以验证和授权应用程序。
    
下图展示了获取同意和请求获取访问令牌的顺序。


![管理 API 入门 - 授权流](images/authorization-flow.png)


## <a name="register-your-application-in-azure-ad"></a>在 Azure AD 中注册应用程序

Office 365 管理 API 使用 Azure AD 提供对 Office 365 租户数据的安全身份验证。 若要访问 Office 365 管理 API，必须在 Azure AD 中注册应用程序，并在配置过程中指定应用程序访问 API 所需的权限级别。


### <a name="prerequisites"></a>先决条件

必须有 Office 365 订阅以及与 Office 365 订阅关联的 Azure 订阅，才能在 Azure AD 中注册应用程序。 可以从 Office 365 和 Azure 这两款产品的试用订阅入手。 有关详细信息，请参阅[欢迎加入 Office 365 开发人员计划](https://docs.microsoft.com/zh-CN/office/developer-program/office-365-developer-program)。


### <a name="use-the-azure-management-portal-to-register-your-application-in-azure-ad"></a>使用 Azure 管理门户在 Azure AD 中注册应用程序

拥有包含正确订阅的 Microsoft 租户后，便可以在 Azure AD 中注册应用程序了。

1. 使用包含要使用的 Office 365 订阅的 Microsoft 租户的凭据，登录 [Azure 管理门户](https://manage.windowsazure.com/)。 也可以单击 [Office 管理门户](https://portal.office.com/)左侧导航窗格中的链接，从而访问 Azure 管理门户。
    
2. 在左侧导航窗格中，选择“Active Directory”（图中的“1”）。 请确保“目录”选项卡（图中的“2”）处于选中状态，再选择目录名称（图中的“3”）。
    
   ![O365 注册页](images/o365-sign-up-page.png)
    
    
3. 在目录页上，选择“应用程序”****。 此时，Azure AD 会列出当前在租赁中安装的应用程序。
    
4. 选择“添加”****。
    
   ![Office 365 管理页](images/o365-admin-page.png)
    
    
5. 选择“添加我的组织正在开发的应用程序”****。
    
6. 在“名称”**** 字段中输入应用程序名称，并将“类型”**** 指定为“WEB 应用程序和/或 WEB API”。
    
7. 输入相应的应用程序属性：
    
   - **登录 URL**。 可供用户登录并使用应用程序的网址。 稍后可根据需要更改此设置。
    
   - **应用程序 ID URI**。 用作应用程序的唯一逻辑标识符的 URI。 URI 必须位于已验证自定义域中，这样外部用户才能向应用程序授予权限，允许应用程序访问外部用户在 Windows Azure AD 中的数据。 例如，如果 Microsoft 租户为 **contoso.onmicrosoft.com**，那么应用程序 ID URI 可以是 **https://app.contoso.onmicrosoft.com**。
    
8. 此时，应用程序已向 Azure AD 进行注册，并已分配有客户端 ID。 不过，还剩几个重要的应用程序方面需要配置。
    

### <a name="configure-your-application-properties-in-azure-ad"></a>在 Azure AD 中配置应用程序属性

至此，应用程序已注册，必须指定几个重要属性，以确定应用程序如何在 Azure AD 中运行，以及租户管理员如何同意允许应用程序使用 Office 365 管理 API 访问管理员租户数据。

若要详细了解 Azure AD 应用程序常规配置，请参阅[应用程序对象属性](https://docs.microsoft.com/zh-CN/azure/active-directory/develop/active-directory-application-objects)。


1. **客户端 ID**。 此值由 Azure AD 自动生成。 应用程序在请求获取租户管理员同意时，以及在从 Azure AD 请求获取仅应用程序令牌时使用此值。
    
2. **应用程序是多租户的**。 此属性必须设置为“是”****，这样租户管理员才能同意允许应用程序使用 Office 365 管理 API 访问管理员租户数据。 如果此属性设置为“否”****，应用程序就只能访问你自己租户的数据。
    
3. **回复 URL**。 这是租户管理员在同意允许应用程序使用 Office 365 管理 API 访问管理员租户数据后重定向到的 URL。 可根据需要配置多个回复 URL。 Azure 会自动设置第一个回复 URL，以匹配你在创建应用程序时指定的登录 URL，但你可根据需要更改此值。
    
更改这些属性后，请务必选择“保存”****。


### <a name="generate-a-new-key-for-your-application"></a>生成应用程序的新密钥

用授权代码交换访问令牌时会用到密钥（亦称为“客户端密码”）。


1. 在 Azure 管理门户中，选择应用程序并在顶部菜单中选择**配置**。 向下滚动到**密钥**。
    
2. 依次选择密钥期限和“保存”****。
    
   ![Azure 订阅页](images/azure-subscription-page.png)
    
    
3. Azure 仅显示保存后的应用程序密码。 选择“剪贴板”图标，将客户端密码复制到剪贴板中。
    
   ![Azure 门户页](images/azure-portal-page.png)

   > [!IMPORTANT] 
   > Azure 仅在客户端密码最初生成时才显示它。 无法返回到此页，因此稍后也无法检索客户端密码。

### <a name="configure-an-x509-certificate-to-enable-service-to-service-calls"></a>将 X.509 证书配置为启用服务到服务调用

在后台运行的应用程序（如守护程序或服务）可使用客户端凭据，以请求获取仅应用程序访问令牌，而无需在获取初始同意后重复请求获取租户管理员同意。 

有关详细信息，请参阅[使用客户端凭据执行服务到服务调用](https://msdn.microsoft.com/zh-CN/library/azure/dn645543.aspx)。

必须为应用程序配置 X.509 证书，以用作在从 Azure AD 请求获取仅应用程序访问令牌时所需的客户端凭据。 此过程分为两步：

- 获取 X.509 证书。 可使用自签名证书或由公共可信的证书颁发机构颁发的证书。
    
- 将应用程序清单修改为，包含证书的指纹和公钥。
    
下面介绍了如何使用 Visual Studio 或 Windows SDK _makecert_ 工具生成自签名证书，并将公钥导出到经过 base64 编码的文件中。


1. 在命令行中运行下面的命令：
    
   ```
    makecert -r -pe -n "CN=MyCompanyName MyAppName Cert" -b 03/15/2015 -e 03/15/2017 -ss my -len 2048
   ```

   > [!NOTE] 
   > 生成 X.509 证书时，请确保密钥长度至少为 2048。 无法将长度较短的密钥接受为有效密钥。

2. 打开证书 MMC 管理单元，并连接到用户帐户。 
    
3. 在个人文件夹中查找新证书，并将公钥导出到经过 base64 编码的文件（例如，mycompanyname.cer）中。 由于应用程序将使用此证书与 Azure AD 进行通信，因此请确保你也保留对私钥的访问权限。
    
   > [!NOTE] 
   > 可使用 Windows PowerShell 提取指纹和经过 base64 编码的公钥。 其他平台提供了用于检索证书属性的类似工具。

4. 在 Windows PowerShell 提示符中键入并运行下面的命令：
    
   ```powershell
    $cer = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2
    $cer.Import("mycer.cer")
    $bin = $cer.GetRawCertData()
    $base64Value = [System.Convert]::ToBase64String($bin)
    $bin = $cer.GetCertHash()
    $base64Thumbprint = [System.Convert]::ToBase64String($bin)
    $keyid = [System.Guid]::NewGuid().ToString()
   ```

5. 存储 `$base64Thumbprint`、`$base64Value` 和 `$keyid` 的值，以供在下一组步骤中更新应用程序清单时使用。
    
   使用从证书中提取的值和生成的密钥 ID，现在必须在 Azure AD 中更新应用程序清单。
    
6. 在 Azure 管理门户中，依次选择应用程序和顶部菜单中的“配置”****。
    
7. 在命令栏中，依次选择“管理清单”**** 和“下载清单”****。
    
   ![命令行证书显示](images/command-line-certificate-display.png)
    
    
8. 打开下载的清单进行编辑，并将空的 KeyCredentials 属性替换为以下 JSON：
    
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
   > [KeyCredentials](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#KeyCredentialType) 属性是一个集合，让为滚动更新方案上传多个 X.509 证书，或者为泄漏方案删除证书成为可能。

9. 保存更改，并依次选择命令栏中的“管理清单”**** 和“上传清单”****，再浏览并选择更新后的清单文件以上传。
    

### <a name="specify-the-permissions-your-app-requires-to-access-the-office-365-management-apis"></a>指定应用程序访问 Office 365 管理 API 所需的权限

最后，需要指定应用程序访问 Office 365 管理 API 所需的确切权限。 为此，请先向应用程序添加对 Office 365 管理 API 的访问权限，再指定所需的一项或多项权限。


1. 在 Azure 管理门户中，依次选择应用程序和顶部菜单中的“配置”****。 向下滚动到“对其他应用程序的权限”****，并选择“添加应用程序”****。
    
   ![Azure AD 页](images/azure-ad-page.png)
    
    
2. 选择“Office 365 管理 API”****（图中的“1”），让它显示在“已选择”**** 列（图中的“2”）中，再选择右下角的复选标记（图中的“3”），以保存选择并返回到应用程序的主配置页。
    
   ![Azure AD 应用程序页](images/azure-ad-apps-page.png)
    
    
3. 此时，Office 管理 API 显示在应用程序需要有权访问的应用程序列表中。 在“应用程序权限”**** 和“委托权限”**** 下，选择应用程序需要的权限。 若要详细了解每项权限，请参阅具体的 API 参考。  

   > [!NOTE] 
   > 目前有四项未用权限与今后即将弃用的活动报告和威胁智能相关。 请勿选择其中任何权限，因为它们是不必要的。
    
   ![“添加应用程序”对话框](images/add-an-application-dialog.png)
    
    
4. 选择“保存”****，以保存配置。
    

## <a name="get-office-365-tenant-admin-consent"></a>获取 Office 365 租户管理员同意

至此，应用程序已配置有使用 Office 365 管理 API 所需的权限。租户管理员必须显式授予应用程序这些权限，应用程序才能使用 API 访问管理员租户数据。 若要授予同意，租户管理员必须使用以下专门构造的 URL 登录 Azure AD，以查看应用程序请求获取的权限。 如果使用 API 访问你自己租户的数据，便无需执行这一步。


```http
https://login.windows.net/common/oauth2/authorize?response_type=code&resource=https%3A%2F%2Fmanage.office.com&client_id={your_client_id}&redirect_uri={your_redirect_url }
```

重定向 URL 必须与在 Azure AD 中为应用程序配置的回复 URL 之一一致，或是其子路径。

例如：

```http
https://login.windows.net/common/oauth2/authorize?response_type=code&resource=https%3A%2F%2Fmanage.office.com&client_id=2d4d11a2-f814-46a7-890a-274a72a7309e&redirect_uri=http%3A%2F%2Fwww.mycompany.com%2Fmyapp%2F
```

可测试同意 URL，具体方法为将同意 URL 粘贴到浏览器中，并使用除用于注册应用程序的租户以外的租户的 Office 365 管理员凭据登录。 此时，可看到向应用程序授予 Office 管理 API 使用权限的请求。


![Azure AD 已添加应用程序页](images/azure-ad-app-added-page.png)

选择“接受”**** 后，便会重定向到指定页，并且查询字符串中会有授权代码。 

例如：

```http
http://www.mycompany.com/myapp/?code=AAABAAAAvPM1KaPlrEqdFSB...
```

应用程序使用此授权代码，从 Azure AD 获取可从中提取租户 ID 的访问令牌。 提取并存储租户 ID 后，可获取后续访问令牌，而无需请求租户管理员登录。


## <a name="request-access-tokens-from-azure-ad"></a>从 Azure AD 请求获取访问令牌

从 Azure AD 请求获取访问令牌的方法有以下两种：

- 
  [授权代码授予流](https://msdn.microsoft.com/en-us/library/azure/dn645542.aspx)：让租户管理员明确授予同意，以向应用程序返回授权代码。 然后，应用程序用授权代码交换访问令牌。 必须使用这种方法，才能获取应用程序使用 API 访问租户数据所需的初始同意；并且必须有第一个访问令牌，才能获取并存储租户 ID。
    
- [客户端凭据授予流](https://msdn.microsoft.com/zh-CN/library/azure/dn645543.aspx)：允许应用程序在旧版访问令牌到期时请求获取后续访问令牌，而无需请求租户管理员登录并明确授予同意。 必须对在初始租户管理员同意授予后在后台连续运行并调用 API 的应用程序使用这种方法。
    

### <a name="request-an-access-token-using-the-authorization-code"></a>使用授权代码请求获取访问令牌

在租户管理员同意后，应用程序会收到授权代码，它是以 Azure AD 将租户管理员重定向到指定 URL 时显示的查询字符串参数出现。

```http
http://www.mycompany.com/myapp/?code=AAABAAAAvPM1KaPlrEqdFSB...
```

应用程序向 Azure AD 发出 HTTP REST POST，以用授权代码交换访问令牌。 由于租户 ID 尚未知，因此 POST 会被发送到“常用”终结点，即 URL 中没有嵌入租户 ID：

```http
https://login.windows.net/common/oauth2/token
```

POST 正文包含以下内容：

```json
resource=https%3A%2F%2Fmanage.office.com&amp;client_id=a6099727-6b7b-482c-b509-1df309acc563 &amp;redirect_uri= http%3A%2F%2Fwww.mycompany.com%2Fmyapp%2F &amp;client_secret={your_client_key}&amp;grant_type=authorization_code&amp;code= AAABAAAAvPM1KaPlrEqdFSB...
```

#### <a name="sample-request"></a>示例请求

```json
POST https://login.windows.net/common/oauth2/token HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Host: login.windows.net
Content-Length: 944

resource=https%3A%2F%2Fmanage.office.com&amp;client_id=a6099727-6b7b-482c-b509-1df309acc563 &amp;redirect_uri= http%3A%2F%2Fwww.mycompany.com%2Fmyapp%2F &amp;client_secret={your_client_key}&amp;grant_type=authorization_code&amp;code=AAABAAAAvPM1KaPlrEqdFSB...
```

<br/>

响应正文包含多个属性，包括访问令牌。 

#### <a name="sample-response"></a>示例响应

```json
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Content-Length: 3265

{"expires_in":"3599","token_type":"Bearer","scope":"ActivityFeed.Read ActivityReports.Read ServiceHealth.Read","expires_on":"1438290275","not_before":"1438286375","resource":"https://manage.office.com","access_token":"eyJ0eX...","refresh_token":"AAABAAA...","id_token":"eyJ0eXAi..."}
```

返回的访问令牌是 JWT 令牌，其中既包含授予同意的管理员的相关信息，也包含请求获取访问权限的应用程序的相关信息。 下面的示例展示了未编码的令牌。 应用程序必须从此令牌中提取并存储租户 ID “tid”，这样才能在访问令牌到期时用它请求获取其他访问令牌，而无需与管理员有进一步交互。

#### <a name="sample-token"></a>示例令牌

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


### <a name="request-an-access-token-by-using-client-credentials"></a>使用客户端凭据请求获取访问令牌

在租户 ID 已知后，应用程序便可以对 Azure AD 执行服务到服务调用，以在访问令牌到期时请求获取其他访问令牌。 这些令牌仅包含发出请求的应用程序的相关信息，并不包含最初授予同意的管理员的相关信息。 若要执行服务到服务调用，应用程序必须使用 X.509 证书创建客户端断言（形式为经过 base64 编码的 SHA256 签名 JWT 持有者令牌）。

在 .NET 中开发应用程序时，可使用 [Azure AD 身份验证库 (ADAL)](https://docs.microsoft.com/zh-CN/azure/active-directory/develop/active-directory-authentication-libraries) 创建客户端断言。 其他开发平台应该有类似的库。

未编码的 JWT 令牌由具有以下属性的头和有效负载组成。

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

#### <a name="sample-jwt-token"></a>示例 JWT 令牌


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

然后，在执行服务到服务调用以请求获取访问令牌的过程中，将客户端断言传递到 Azure AD。 使用客户端凭据请求获取访问令牌时，请将 HTTP POST 发送到租户专用终结点，即 URL 中已嵌入先前提取和存储的租户 ID。


```http
https://login.windows.net/{tenantid}/oauth2/token
```

POST 正文包含以下内容：


```json
resource=https%3A%2F%2Fmanage.office.com&amp;client_id={your_app_client_id}&amp;grant_type=client_credentials&amp;client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer&amp;client_assertion={encoded_signed_JWT_token}
```

#### <a name="sample-request"></a>示例请求

```json
POST https://login.windows.net/41463f53-8812-40f4-890f-865bf6e35190/oauth2/token HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Host: login.windows.net
Content-Length: 994

resource=https%3A%2F%2Fmanage.office.com&amp;client_id= a6099727-6b7b-482c-b509-1df309acc563&amp;grant_type=client_credentials &amp;client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer&amp;client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Ill5ZnNoSkMzclBRLWtwR281ZFVhaVk1dDNpVSJ9.eyJhdWQiOiJodHRwczpcL1wvbG9naW4ud2luZG93cy5uZXRcLzQxNDYzZjUzLTg4MTItNDBmNC04OTBmLTg2NWJmNmUzNTE5MFwvb2F1dGgyXC90b2tlbiIsImV4cCI6MTQyNzI0ODY0OCwiaXNzIjoiYTYwOTk3MjctNmI3Yi00ODJjLWI1MDktMWRmMzA5YWNjNTYzIiwianRpIjoiMGNlMjU0YzQtODFiMS00YTJlLTg0MzYtOWE4YzNiNDlkZmI5IiwibmJmIjoxNDI3MjQ4MDQ4LCJzdWIiOiJhNjA5OTcyNy02YjdiLTQ4MmMtYjUwOS0xZGYzMDlhY2M1NjMifQ.vfDrmCjiXgoj2JrTkwyOpr-NOeQTzlXQcGlKGNpLLe0oh4Zvjdcim5C7E0UbI3Z2yb9uKQdx9G7GeqS-gVc9kNV_XSSNP4wEQj3iYNKpf_JD2ikUVIWBkOg41BiTuknRJAYOMjiuBE2a6Wyk-vPCs_JMd7Sr-N3LiNZ-TjluuVzWHfok_HWz_wH8AzdoMF3S0HtrjNd9Ld5eI7MVMt4OTpRfh-Syofi7Ow0HN07nKT5FYeC_ThBpGiIoODnMQQtDA2tM7D3D6OlLQRgLfI8ir73PVXWL7V7Zj2RcOiooIeXx38dvuSwYreJYtdphmrDBZ2ehqtduzUZhaHL1iDvLlw
```

响应与之前几乎相同，区别在于令牌不包含相同的属性，因为它不包含授予同意的管理员的属性。 

#### <a name="sample-response"></a>示例响应

```json
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Content-Length: 1276

{"token_type":"Bearer","expires_in":"3599","expires_on":"1431659094","not_before":"1431655194","resource":"https://manage.office.com","access_token":"eyJ0eXAiOiJKV1QiL..."}
```

#### <a name="sample-access-token"></a>示例访问令牌

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


## <a name="build-your-app"></a>生成应用程序

至此，已在 Azure AD 中注册了应用程序，并为应用程序配置了必要的权限，可以生成应用程序了。 以下是设计和生成应用程序时需要注意的一些关键方面：

- **同意体验**。 若要获取客户同意，必须在浏览器中使用前面所述的专门构造 URL 将客户定向到 Azure AD 网站，并且必须有 Azure AD 在管理员授予同意后将他们重定向到的网站。 此网站必须从 URL 中提取授权代码，并用它来请求获取可从中获取租户 ID 的访问令牌。
    
- **将租户 ID 存储在系统中**。 从 Azure AD 请求获取访问令牌和调用 Office 管理 API 时，都必须这样做。
    
- **管理访问令牌**。 需要有在必要时请求获取和管理访问令牌的组件。 如果应用程序定期调用 API，可按需请求获取令牌；或如果应用程序连续调用 API 来检索数据，可定期（例如，每 45 分钟）请求获取一次令牌。
    
- 根据要使用的特定 API 的需要，**实现 Webhook 侦听器**。
    
- **数据检索和存储**。 需要有检索每个租户的数据的组件，检索方式可以是使用连续轮询，也可以是响应 Webhook 通知，具体视要使用的特定 API 而定。
    
