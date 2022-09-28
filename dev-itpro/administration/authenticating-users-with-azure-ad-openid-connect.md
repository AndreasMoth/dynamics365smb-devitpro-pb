---
title: Configure Azure Active Directory Authentication with OpenID Connect
description: Learn how to authentication Business Central users by using Azure Active Directory with OpenID Connect.
ms.custom: na
ms.date: 01/26/2022
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.service: "dynamics365-business-central"
author: jswymer
---
# Configure Azure Active Directory Authentication with OpenID Connect

[!INCLUDE[2022_releasewave1](../includes/2022_releasewave1.md)]

This article explains how to configure Business Central to use Azure Active Directory to authenticate users. This setup configures Azure AD authentication to use [OpenID connect](). 

## Preparation

- Obtain and set up security certificates on the [!INCLUDE[prod_short](../developer/includes/prod_short.md)] deployment

    Azure AD authentication requires the use of service certificates to help secure client connections over a wide area network (WAN). In a production environment, you should obtain a certificate from a certification authority or trusted provider. In a test environment, if you don't have a certificate, then you can create your own self-signed certificate. The implementation of certificates involves installation and configuration of the certificates on the [!INCLUDE[server](../developer/includes/server.md)] server and client computers.

    Follow the instructions for obtaining and installing the certificates [Using Certificates to Secure Connections](../deployment/implement-security-certificates-production-environment.md). However, for now, **don't** change the credential type used by [!INCLUDE[server](../developer/includes/server.md)] and [!INCLUDE[webserver](../developer/includes/webserver.md)] yet. You'll change it later in this article.

## Task 1: Create an Azure AD Tenant

To get started, you need an Azure AD tenant. The Azure AD tenant is where you manage user accounts and register apps, like [!INCLUDE[prod_short](../developer/includes/prod_short.md)].

There are a couple ways to get an Azure AD tenant:

- Sign up a Microsoft 365 plan

    If you have a Microsoft 365 subscription that is based on a domain such as *solutions\.onmicrosoft\.com*, then you're already using Azure AD. The Microsoft 365 user accounts are based on Azure AD. So, there's nothing more to do in this task.

    If you want to sign up for a Microsoft 365 plan, you can use a plan such as Microsoft 365 Enterprise E1 as your test site, or sign up for a trial developer plan. A trial plan includes an administrative account that you'll use to access the Azure management portal. If your Microsoft 365 site is *solutions.onmicrosoft.com*, for example, your administrative account can be *admin\@solutions\.onmicrosoft\.com*. For more information, see [Select a Microsoft 365 plan for business](https://go.microsoft.com/fwlink/?LinkId=309050).

- Sign up for an Azure subscription that isn't associated with a Microsoft 365 subscription and create your own Azure AD tenant. For more information, see the next section.

### Create your own Azure AD tenants

If you have a [!INCLUDE[prod_short](../developer/includes/prod_short.md)] on-premises deployment configured for multitenancy, you can choose to use the same Azure AD tenant for all [!INCLUDE[prod_short](../developer/includes/prod_short.md)] tenants. Or you can create a separate Azure AD tenant for each [!INCLUDE[prod_short](../developer/includes/prod_short.md)] tenant.

1. Sign up for an Azure subscription at [https://azure.microsoft.com](https://azure.microsoft.com).
2. Sign in to [Azure portal](https://portal.azure.com).
3. Create a directory by following the instructions at [How to get an Azure Active Directory tenant](/azure/active-directory/develop/active-directory-howto-tenant).

    This step will create an Azure AD tenant. When you create an Azure Active Directory in the Azure portal, you specify an initial domain name that identifies your Azure AD tenant, such as *solutions.onmicrosoft.com* or *cronusinternationltd.onmicrosoft.com*. You'll use the domain name when you add users to your Azure AD and when you configure the [!INCLUDE[server](../developer/includes/server.md)] instance. In the tasks that follow, this value is referred to as the Azure AD Tenant ID. 

4. After you've created the Azure AD tenant, add users.

    For more information, see [Quickstart: Add new users to Azure Active Directory](/azure/active-directory/fundamentals/add-users-azure-active-directory). Later, you'll have to map the users in Azure AD to your users in [!INCLUDE[prod_short](../developer/includes/prod_short.md)].

## Task 2: Register an application in the Azure AD tenant  

In this task, you register your [!INCLUDE[prod_short](../developer/includes/prod_short.md)] solution as an application in the Azure AD tenant.

> [!NOTE]
> If you're configuring a multitenant deployment, where each tenant will use a different Azure Tenant, you only register an application on one of the Azure AD tenants. Then, you'll make the application available to the other Azure AD tenants by making it a *Multitenant* application.

1. Sign in to [Azure portal](https://portal.azure.com) and open the Active Directory tenant.

2. To register the application, follow the guidelines at [Registering Business Central On-Premises in Azure AD for Integrating with Other Services](/dynamics365/business-central/dev-itpro/administration/register-app-azure).

    When you add an application to an Azure AD tenant, you specify the following information. The configuration is slightly different in for [!INCLUDE[prod_short](../developer/includes/prod_short.md)] single-tenant and multitenant deployment.

    # [Single-tenant](#tab/singletenant)

    | Setting | Description |
    |--|--|
    |Name|Specifies the name of your application as it will display to your users, such as **Business Central App by My Solutions**.|
    |Supported account types|Select Accounts in any organizational directory (Any Azure AD directory - Multitenant)|
    |Redirect URI|Specifies the type of application that you're registering and the redirect URI (or reply URL) for your application. Set the type to **Web**, and in the redirect URL box, enter URL for signing in to the [!INCLUDE[webclient](../developer/includes/webclient.md)], for example `https://localhost:443/BC200/SignIn`.<br /><br />The URI has the format `https://<domain or computer name>/<webserver-instance>/SignIn`, such as `https://cronusinternationltd.onmicrosoft.com/BC200/SignIn` or `https://MyBcWebServer/BC200/SignIn`. <br /> <br />**Important** The portion of the reply URL after the domain name (in this case `BC200/SignIn`) is case-sensitive, so make sure that the web server instance name matches the case of the web server instance name as it is defined on IIS for your [!INCLUDE[webserver](../developer/includes/webserver.md)] installation.|

    # [Multitenant-tenant](#tab/multitenant)
    
    | Setting | Description |
    |--|--|
    |Name|Specifies the name of your application as it will display to your users, such as **Business Central App by My Solutions**.|
    |Supported account types|Specifies the accounts that you would like your application to support. If you're going to use different Azure AD tenants for different [!INCLUDE[prod_short](../developer/includes/prod_short.md)] tenants, then select **Accounts in any organizational directory (Any Azure AD directory - Multitenant)**. Otherwise, you can choose **Accounts in this organizational directory only (Single tenant)**.|
    |Redirect URI|Specifies the type of application that you're registering and the redirect URI (or reply URL) for your application. Set the type to **Web**, and in the redirect URL box, enter URL for signing in to the [!INCLUDE[webclient](../developer/includes/webclient.md)], for example `https://localhost:443/BC200/SignIn`.<br /><br />The URI has the format `https://<domain or computer name>/<webserver-instance>/SignIn`, such as `https://cronusinternationltd.onmicrosoft.com/BC200/SignIn` or `https://MyBcWebServer/BC200/SignIn`.<br /> <br />**Important** The portion of the reply URL after the domain name (in this case `BC200/SignIn`) is case-sensitive, so make sure that the web server instance name matches the case of the web server instance name as it is defined on IIS for your [!INCLUDE[webserver](../developer/includes/webserver.md)] installation.|

    ---

3. Set up ID tokens for implicit grant and hybrid flows:

   1. Select **Authentication**.
   2. Under **Implicit grant and hybrid flows**, select **ID tokens (used for implicit and hybrid flows)**.
   3. Select **Save**.

4. The [!INCLUDE[prod_short](../developer/includes/prod_short.md)] solution is now registered in your Azure AD tenant. To complete the steps that follow, you'll need the following information:

   - Tenant's domain (**Directory (tenant) ID**)
   - **Redirect URI**
   - **Application (client) ID**

    You can view the settings in the Azure portal by selecting **Overview** for the registered application. So, make a note of or copy the values for these settings for later use.

> [!NOTE]
> The guidelines for the Azure Portal in this article might not reflect the current user interface of the Azure Portal. Please refer to the Azure Portal documentation for the latest instructions.

## <a name="task3"></a>Task 3: Associate Azure AD Users with [!INCLUDE[prod_short](../developer/includes/prod_short.md)] Users
  
Each user in your Azure AD tenant that will access [!INCLUDE[prod_short](../developer/includes/prod_short.md)] must be set up with an account in [!INCLUDE[prod_short](../developer/includes/prod_short.md)].

You can postpone this task for most users and do it after you complete Azure AD setup. But you must do this task now for your [!INCLUDE[prod_short](../developer/includes/prod_short.md)] user account. If you don't, you won't be able to sign in to [!INCLUDE[prod_short](../developer/includes/prod_short.md)] after you switch to Azure AD.

To associate a [!INCLUDE[prod_short](../developer/includes/prod_short.md)] user with Azure AD, set the user's authentication email in [!INCLUDE[prod_short](../developer/includes/prod_short.md)] to the user's principal name in Azure AD, like *chris@contoso.com*. <!-- When you combine this setting with the relevant configuration of the [!INCLUDE[server](../developer/includes/server.md)] instance, users achieve single sign-on when they access [!INCLUDE[nav_web](../developer/includes/nav_web_md.md)].-->

1. In the [Azure portal](https://portal.azure.com), get the Azure AD user's principle name.

    For more information, see [Users in Azure Active Directory](/azure/active-directory/fundamentals/add-users-azure-active-directory) in the Azure help.

2. Assign the Azure user principle name to the Business Central user. You can use the web client or [!INCLUDE[adminshell](../developer/includes/adminshell.md)].

  # [Business Central Web client](#tab/admintool)

  1. Start the [!INCLUDE[prod_short](../developer/includes/prod_short.md)], and open the **Users** page.
  2. Open the user that you want to modify.
  3. Under **Microsoft 365 (Authentication)**, set the **Authentication Email** to the user principle name  in Azure AD.

  # [Administration Shell](#tab/adminshell)

  1. Run [!INCLUDE[adminshell](../developer/includes/adminshell.md)] as an administrator.
  2. Run the [Set-NAVServerUser cmdlet](/powershell/module/microsoft.dynamics.nav.management/set-navserveruser) to set the authentication email. For example:

     ```powershell
     Set-NAVServerUser -WindowsAccount yourdomain\username -AuthenticationEmail "AzureAD_principal_name"
     ```
---

For more information, see [Create Users According to Licenses](/dynamics365/business-central/ui-how-users-permissions).

## Task 4: Configure [!INCLUDE[server](../developer/includes/server.md)]

Once you have the Azure AD tenant and a registered application for [!INCLUDE[prod_short](../developer/includes/prod_short.md)], you configure the [!INCLUDE[server](../developer/includes/server.md)] instance for Azure AD authentication.

<!-- You can configure the [!INCLUDE[server](../developer/includes/server.md)] by using the [!INCLUDE[admintool](../developer/includes/admintool.md)] or [!INCLUDE[adminshell](../developer/includes/adminshell.md)].

# [Administration Tool](#tab/admintool)

1. Open the [!INCLUDE[admintool](../developer/includes/admintool.md)].

2. In the **General** tab, set the **Credential Type** to **AccessControlService**.

3. Configure the Azure Active Directory settings.

    This step is different for a single-tenant and multitenant [!INCLUDE[server](../developer/includes/server.md)] deployments. The settings you set in this step are under the **Azure Active Directory** tab.

    # [Single-tenant](#tab/singletenant)


    1. Set the **Valid Audiences** parameter to the **Application (client) ID** of the registered application in Azure AD.

    2. Set the **WS-Federation Metadata Location** parameter.

        The WS-federation metadata location establishes a trust relationship between [!INCLUDE[prod_short](../developer/includes/prod_short.md)] and Azure AD. The parameter value has the following format:

        ```
        https://login.microsoftonline.com/<AAD TENANT ID>/FederationMetadata/2007-06/FederationMetadata.xml
        ```

        |Parameter|Description|
        |-|-|-|
        |`<AAD TENANT ID>`|The ID of the Azure AD tenant ID or its domain, for example, `11111111-aaaa-2222-bbbb-333333333333` or `CRONUSInternationLtd.onmicrosoft.com`. The value is the same as what you used in the WS-federation login endpoint in the previous step.|

        **Example**

        ```
        https://login.microsoftonline.com/cronusinternationltd.onmicrosoft.com/FederationMetadata/2007-06/FederationMetadata.xml
        ```  
    
    # [Multitenant-tenant](#tab/multitenant)

    1. Set the **Valid Audiences** parameter to the **Application (client) ID** of the registered application in Azure AD.

    2. Set the **WS-Federation Metadata Location** parameter.

        The WS-federation metadata location establishes a trust relationship between [!INCLUDE[prod_short](../developer/includes/prod_short.md)] and Azure AD. The parameter value has the following format:

        ```
        https://login.microsoftonline.com/<AADTENANTID>/FederationMetadata/2007-06/FederationMetadata.xml
        ```

        |Parameter|Description|
        |-|-|-|
        |`<AADTENANTID>`|Set this parameter to one of the following values:<ul><li>`{AADTENANTID}`- Use this value if each [!INCLUDE[prod_short](../developer/includes/prod_short.md)] tenant corresponds to an Azure AD tenant that has a service principal. The [!INCLUDE[server](../developer/includes/server.md)] instance will automatically replace `{AADTENANTID}` with the correct Azure AD tenant. The Azure AD Tenant ID is specified when you mount the tenant. ID parameter that is specified when mounting a tenant replaces the placeholder.</li><li>`common`- Use this value if the corresponding [!INCLUDE[prod_short](../developer/includes/prod_short.md)] application in Azure AD configured as a multitenant application, but tenants will use the same Azure AD tenant. </li></ul>|

    ---

    > [!IMPORTANT]
    > The **Application Client Certificate Thumbprint**, **Application Client ID**, and **Application Client Secret** parameters aren't used. The **Application Client ID** must be empty or set to `00000000-0000-0000-0000-000000000000`. If not, you'll get the following error when you try to sign in to Business Central: `The value for the WSFederationLoginEndpoint configuration settings cannot be empty`.
4. To configure SOAP and OData web services for Azure AD authentication, specify the App ID URI that is registered for [!INCLUDE[prod_short](../developer/includes/prod_short.md)] in the Azure AD.

    On the **Azure Active Directory** tab, set the **Azure AD App URI**. The App ID URI is typically the same as the *wtrealm* parameter value of the **WS-Federation Endpoint** setting.

5. Increase the `ExtendedSecurityTokenLifetime` parameter value.

    This parameter defines the interval of time that a client session can remain inactive before the session is dropped. If the value is too low, users may experience the error **Connection is not longer available or was lost**. The event log will include the error **The SAML2 token is not valid because its validity period has ended.** for the server instance. Increasing this value will resolve this issue. We recommend that you set it to a value greater than 8 hours.

6. Disable token-signing certificate validation by selecting the **Disable Token-Signing Certificate Validation** check box.

7. Restart the server instance.

# [Administration Shell](#tab/adminshell)-->

1. Run [!INCLUDE[adminshell](../developer/includes/adminshell.md)] as an administrator.
 
    To configure the server instance in the next steps, you'll use the [Set-NAVServerConfiguration cmdlet](/powershell/module/microsoft.dynamics.nav.management/set-navserverconfiguration).

2. Set the `ClientServicesCredentialType` to `AccessControlService`.

   ```powershell
   Set-NAVServerConfiguration -ServerInstance $BCServerInstanceName  -KeyName ClientServicesCredentialType -KeyValue AccessControlService
   ```

3. Configure the Azure Active Directory settings.

    This step is different for a single-tenant and multitenant [!INCLUDE[server](../developer/includes/server.md)] deployments.

    1. Set the `ValidAudiences` parameter to the **Application (client) ID** of the registered application in Azure AD.

        The value has the following format:

        ```powershell
        Set-NAVServerConfiguration -ServerInstance $BCServerInstanceName  -KeyName ValidAudiences -KeyValue "<application ID>"
        ```

        **Example**

        ```powershell
        Set-NAVServerConfiguration -ServerInstance BC200 -KeyName ValidAudiences -KeyValue "44444444-cccc-5555-dddd-666666666666"
        ```

    2. Set the `ClientServicesFederationMetadataLocation` parameter.

        The value has the following format:

        ```powershell
        Set-NAVServerConfiguration -ServerInstance $BCServerInstanceName  -KeyName ClientServicesFederationMetadataLocation -KeyValue "https://login.microsoftonline.com/<AAD TENANT ID>/FederationMetadata/2007-06/FederationMetadata.xml"
        ```

        # [Single-tenant](#tab/singletenant)

        Replace `<AAD TENANT ID>` with the ID of the Azure AD tenant ID or its domain, for example, `11111111-aaaa-2222-bbbb-333333333333` or `CRONUSInternationLtd.onmicrosoft.com`.

        For example:

        ```powershell
        Set-NAVServerConfiguration -ServerInstance BC200 -KeyName ClientServicesFederationMetadataLocation -KeyValue "https://login.microsoftonline.com/cronusinternationltd.onmicrosoft.com/FederationMetadata/2007-06/FederationMetadata.xml"
        ```  

        # [Multitenant-tenant](#tab/multitenant)

        Replace `<AAD TENANT ID>` with `common`.

        <!--
        |Value|Description|
        |-|-|
        |`{AADTENANTID}`|Use this value if each [!INCLUDE[prod_short](../developer/includes/prod_short.md)] tenant corresponds to an Azure AD tenant that has a service principal. The [!INCLUDE[server](../developer/includes/server.md)] instance will automatically replace `{AADTENANTID}` with the correct Azure AD tenant. The Azure AD Tenant ID is specified when you mount the tenant. ID parameter that is specified when mounting a tenant replaces the placeholder.
        |`common`|Use this value if the corresponding [!INCLUDE[prod_short](../developer/includes/prod_short.md)] application in Azure AD configured as a multitenant application, but tenants will use the same Azure AD tenant|
        -->

        For example:

        ```powershell
        Set-NAVServerConfiguration -ServerInstance BC200 -KeyName ClientServicesFederationMetadataLocation -KeyValue "https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml"
        ```

        ---

    > [!IMPORTANT]
    > The `AzureActiveDirectoryClientSecret`, `AzureActiveDirectoryClientId`, and `AzureActiveDirectoryClientSecret` parameters aren't used. The `AzureActiveDirectoryClientId` must be empty or set to `00000000-0000-0000-0000-000000000000`. If not, you'll get the following error when you try to sign in to Business Central: `The value for the WSFederationLoginEndpoint configuration settings cannot be empty`.

<!--
4. Disable token-signing certificate validation by setting `DisableTokenSigningCertificateValidation` to `true`.

    ```powershell
    Set-NAVServerConfiguration -ServerInstance $BCServerInstanceName  -KeyName DisableTokenSigningCertificateValidation -KeyValue true
    ```
-->
4. To configure SOAP and OData web services for Azure AD authentication, specify the App ID URI that is registered for [!INCLUDE[prod_short](../developer/includes/prod_short.md)] in the Azure AD.

    ```powershell
    Set-NAVServerConfiguration -ServerInstance $BCServerInstanceName  -KeyName AppIdUri -KeyValue "<Application ID URI>"
    ```

    The value is typically the same as the *wtrealm* parameter value of the WSFederationLoginEndpoint parameter. 

    **Example**

    ```powershell
    Set-NAVServerConfiguration -ServerInstance BC200  -KeyName AppIdUri -KeyValue "https://cronusinternationltd.onmicrosoft.com"
    ```

5. Restart the server instance. For example:

    ```powershell
    Restart-NAVServerInstance -ServerInstance BC200
    ```
<!--
---
-->

## Task 5: Configure [!INCLUDE[webservercomponents](../developer/includes/webservercomponents.md)]

<!--

# [navsettings.config](#tab/navsettings)

1. Open the navsettings.json for the [!INCLUDE[webserver](../developer/includes/webserver.md)] in any text or code editor, such as Notepad or Visual Studio Code.

2. Set the `ClientServicesCredentialType` key value to `AccessControlService`.

    ```
    "ClientServicesCredentialType":  "AccessControlService",
    ```

3. Add the `AadApplicationId` key value and set it to **Application (client) ID**. `AccessControlService`.

    ```
    "AadApplicationId": "b90cd8cd-0361-4dba-b080-8a10c78fe1d9,
    ```

4. Add the `AadAuthorityUri` key value and set the value `https://login.microsoftonline.com/<AADTenentID>`, where `<AADTenentID>` is the Azure AD tenant ID.

    ```
    "AadAuthorityUri": "https://login.microsoftonline.com/8455892e-0f26-4c36-a48d-37278a0ae37d"
    ```

5. Save the navsettings.json file

# [Administration Sehll](#tab/adminshell) -->

1. Set the `ClientServicesCredentialType` key to `AccessControlService`.

    ```powershell
    Set-NAVWebServerInstanceConfiguration -WebServerInstance BC200 -KeyName ClientServicesCredentialType -KeyValue "AccessControlService"
    ```

2. Set the `AadApplicationId` key to the application (client) ID of the registered application for Business Central in Azure AD.

    ```powershell
    Set-NAVWebServerInstanceConfiguration -WebServerInstance $BCServerInstanceName -KeyName AadApplicationId -KeyValue $AADApplicationId
    ```

    For example:

    ```powershell
    Set-NAVWebServerInstanceConfiguration -WebServerInstance $BCServerInstanceName -KeyName AadApplicationId "44444444-cccc-5555-dddd-666666666666"
    ```

3. Set the `AadAuthorityUri` key.

    ```powershell
    Set-NAVWebServerInstanceConfiguration -WebServerInstance $BCServerInstanceName -KeyName AadAuthorityUri -KeyValue "https://login.microsoftonline.com/<AzureADTenantID>"
    ```

    # [Single-tenant](#tab/singletenant)

    Replace `<AAD TENANT ID>` with the ID of the Azure AD tenant ID or its domain, for example, `11111111-aaaa-2222-bbbb-333333333333` or `CRONUSInternationLtd.onmicrosoft.com`.

    For example:

    ```powershell
    Set-NAVWebServerInstanceConfiguration -WebServerInstance $BCServerInstanceName -KeyName AadAuthorityUri -KeyValue "https://login.microsoftonline.com/11111111-aaaa-2222-bbbb-333333333333"
    ```

    # [Multitenant-tenant](#tab/multitenant)

    Replace `<AAD TENANT ID>` with `common`.

    <!--

    |Value|Description|
    |-|-|
    |`{AADTENANTID}`|Use this value if each [!INCLUDE[prod_short](../developer/includes/prod_short.md)] tenant corresponds to an Azure AD tenant that has a service principal. The [!INCLUDE[server](../developer/includes/server.md)] instance will automatically replace `{AADTENANTID}` with the correct Azure AD tenant. The Azure AD Tenant ID is specified when you mount the tenant. ID parameter that is specified when mounting a tenant replaces the placeholder.
    |`common`|Use this value if the corresponding [!INCLUDE[prod_short](../developer/includes/prod_short.md)] application in Azure AD configured as a multitenant application, but tenants will use the same Azure AD tenant|

    -->
    For example:

    <!--
    ```powershell
    Set-NAVServerConfiguration -ServerInstance BC200 -KeyName ClientServicesFederationMetadataLocation -KeyValue "https://login.microsoftonline.com/{AADTENANTID}"
    ```
    -->
    or

    ```powershell
    Set-NAVServerConfiguration -ServerInstance BC200 -KeyName ClientServicesFederationMetadataLocation -KeyValue "https://login.microsoftonline.com/common"
    ```

    ---
<!--
---
-->
For more information, see [Configure Configuring [!INCLUDE[webserver](../developer/includes/webserver.md)] Instances](configure-web-server.md) and [Configure Authentication](users-credential-types.md).

## Task 7: Mount tenants (multitenant only)

If you have a multitenant [!INCLUDE[prod_short](../developer/includes/prod_short.md)] environment, mount the tenant databases on the [!INCLUDE[server](../developer/includes/server.md)].

<!-- Use either the [!INCLUDE[admintool](../developer/includes/admintool.md)] or [!INCLUDE[adminshell](../developer/includes/adminshell.md)]. For more information about mounting tenants, see [Mount or Dismount a Tenant on a Business Central Server Instance](mount-dismount-tenant.md).

# [Administration Tool](#tab/admintool)

In the navigation pane, use the **Tenants** node  to mount the tenants.

- If you'll be using the same Azure AD tenant for all [!INCLUDE[prod_short](../developer/includes/prod_short.md)] tenants, you can leave **Azure AD Tenant ID** blank.
- If you'll be using different Azure AD tenants, set the **Azure AD Tenant ID** to the ID or domain name of the Azure AD tenant that you want to use for the tenant. For example, **11111111-aaaa-2222-bbbb-333333333333** or **CRONUSInternationLtd.onmicrosoft.com**.
- Also, if you've set up different host names that you want to use accessing the tenant, set the **Alternate ID**. See [Using alternate tenant IDs](#altid).

# [Administration Shell](#tab/adminshell)-->

Mount your tenants by using the [Mount-NAVTenant cmdlet](/powershell/module/microsoft.dynamics.nav.management/mount-navtenant).

- If you'll be using the same Azure AD tenant for all [!INCLUDE[prod_short](../developer/includes/prod_short.md)] tenants, you can omit the `-AadTenantId` parameter.
- If you'll be using different Azure AD tenants, set the `-AadTenantId` parameter to the ID or domain name of the Azure AD tenant that you want to use for the tenant.
- Also, if you've set up different host names that you want to use accessing the tenant, use the `-AlternateId` parameter. See [Using alternate tenant IDs](#altid).

For example:

```powershell
Mount-NAVTenant -ServerInstance BC200  -Tenant Tenant1 –DatabaseServer ..\BCDEMO -DatabaseName "BC Demo Database" -AadTenantId 1111-aaaa-2222-bbbb-333333333333
```
<!--
--- 
-->

## Task 8: Set up other users and web service accounts
  
### User accounts

Set up remaining [!INCLUDE[prod_short](../developer/includes/prod_short.md)] user accounts, which you didn't cover in [Task 3](#task3), with Azure AD authentication email accounts.

### Web service accounts

With Azure AD authentication, [!INCLUDE[prod_short](../developer/includes/prod_short.md)]  supports two different methods for authenticating users trying to access data through OData and SOAP web services: web service access keys (or Basic authentication) and OAuth.

With Basic authentication, the [!INCLUDE[prod_short](../developer/includes/prod_short.md)] user account must have web service access key. To sign in, instead using their Azure AD password, users provide the [!INCLUDE[prod_short](../developer/includes/prod_short.md)] the web service access key. For information about setting up web service access keys, see [How to use an Access Key for SOAP and OData Web Service Authentication](../webservices/web-services-authentication.md#accesskey).

With OAuth, users are authenticated based on their Azure AD credentials, providing a more direct and single sign-on experience. See [Using OAuth to Authorize Business Central Web Services (OData and SOAP)](../webservices/authenticate-web-services-using-oauth.md).

## More Security and configuration tips

### Set the access token lifetime

> [!IMPORTANT]  
> For security reasons, we recommend that you limit the lifetime of the access token to 10 minutes as described in this section.

Follow the steps outlined below.

1. Download the latest [Azure AD PowerShell Module Public Preview release](https://www.powershellgallery.com/packages/AzureADPreview/2.0.1.11).
2. Run the following command to sign in to your Azure AD admin account: `Connect-AzureAD -Confirm`
3. Sign in as the tenant admin. 
4. Run the `Get-AzureADPolicy` command. 
5. For each `Id` that is the result of above command, run `Remove-AzureADPolicy -Id {Guid}`. 
6. Set the token lifetime to 10 minutes by running the following command: `New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "AccessTokenLifetime":"0.00:10:00"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"`.

For reference, see the prerequisites section in the following article: [Configurable token lifetimes in Azure Active Directory](/azure/active-directory/active-directory-configurable-token-lifetimes#prerequisites).

### Increase the `ExtendedSecurityTokenLifetime` parameter value on Business Central Server

This parameter defines the interval of time that a client session can remain inactive before the session is dropped. If the value is too low, users may experience the error: **Connection is not longer available or was lost**. The event log will include the error: **The SAML2 token is not valid because its validity period has ended.** for the server instance. Increasing this value will resolve this issue. We recommend that you set it to a value greater than 8 hours.

```powershell
Set-NAVServerConfiguration -ServerInstance $BCServerInstanceName  -KeyName ExtendedSecurityTokenLifetime -KeyValue "<hours>"
```

**Example**

```powershell
Set-NAVServerConfiguration -ServerInstance BC200  -KeyName ExtendedSecurityTokenLifetime -KeyValue "20"
```

### Using host names for tenants

You can configure host name tenant resolution, where each tenant is assigned a unique domain, like customer1.cronusinternational.com. Customers would then access their tenant by using `https://customer1.cronusinternational.com/BC200`.

This setup implies that the public URL is different for each tenant. To support this scenario, you set [!INCLUDE[server](../developer/includes/server.md)] to calculate the host dynamically. In the `WSFederationLoginEndpoint` parameter, use the `{HOSTNAME}` placeholder in the `wreply`, for example:

```http
https://login.microsoftonline.com/<AAD TENANT ID>/wsfed?wa=wsignin1.0%26wtrealm=<APP ID URI>%26wreply=https://{HOSTNAME}/BC200/SignIn

```

### <a name="altid"></a>Using alternate tenant IDs

When you mount a tenant, you can give the tenant an additional ID by setting the `-AlternateId` parameter. Users can then access the tenant using this ID as a well as the original. The alternate ID is useful if you have different host names for tenants. In this case, you have to set up URL rewriting in the web.config file for the [!INCLUDE[webserver](../developer/includes/webserver.md)]. For more information, see [Configuring [!INCLUDE[webserver](../developer/includes/webserver.md)] to Accept Host Names for Tenants](configure-web-server-to-accept-host-names-for-tenants.md).

### Using Visual Studio Code

If you're connecting to your solution from Visual Studio Code, then you must also specify the [!INCLUDE[prod_short](../developer/includes/prod_short.md)] server config parameter `ValidAudiences` and set it to `https://api.businesscentral.dynamics.com`. If you don't do this step, you'll get the error `securitytokeninvalidaudienceexception` in the application log when trying to download symbols.


### Give users guidance about single sign-on

Single sign-on means that users are still signed in to Azure AD when they sign out from [!INCLUDE[prod_short](../developer/includes/prod_short.md)], unless they close all browser windows. However, if a user selected the **Keep me signed in** field when they signed in, they're still signed in when they close the browser window. To fully sign out from Azure AD, the user must sign out from each application that uses Azure AD, including [!INCLUDE[prod_short](../developer/includes/prod_short.md)] and SharePoint.  

We recommend that you provide guidance to your users for signing out of their account when they're done. Doing so will help can keep your [!INCLUDE[prod_short](../developer/includes/prod_short.md)] deployment more secure.  

## See Also  

[Authentication and Credential Types](Users-Credential-Types.md)  
[Troubleshooting: SAML2 token errors with Azure Active Directory/Office 365 Authentication](troubleshooting-SAML2-token-not-valid-because-validity-period-ended.md)  
[Migrating to Multitenancy](../deployment/migrating-to-multitenancy.md)
