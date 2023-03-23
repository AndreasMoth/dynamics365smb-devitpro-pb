---
title: Analyzing Email Trace Telemetry 
description: Learn about the email telemetry in Business Central  
author: jswymer
ms.topic: conceptual
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.search.keywords: administration, tenant, admin, environment, sandbox, telemetry
ms.date: 04/01/2021
ms.author: jswymer
---

# Analyzing Email Trace Telemetry

**APPLIES TO:** [!INCLUDE[prod_short](../includes/prod_short.md)] 2020 release wave 2, update 17.2, and later

Email telemetry gathers data about the following operations: 

- An email was sent successfully
- An attempt to send an email failed 


<!--Today, partners can test email setup during setup (click a test button). If settings change on SMTP side later, they don't get notified before something stops working. -->

Before you can collect this data, you'll have to set up email. For more information, see [Set Up Email](/dynamics365/business-central/admin-how-setup-email) in the [!INCLUDE[prod_short](../includes/prod_short.md)] application help.

## <a name="success"></a>Email sent successfully

Occurs when an email was successfully sent from the client.

### General dimensions

|Dimension|Description or value|
|---------|-----|
|message|**Email sent successfully**|
|severityLevel|**1**|
|user_Id|[!INCLUDE[user_Id](../includes/include-telemetry-user-id.md)] |

### Custom dimensions

|Dimension|Description or value|
|---------|-----|
|aadTenantId|Specifies the Azure Active Directory (Azure AD) tenant ID used for Azure AD authentication. For on-premises, if you aren't using Azure AD authentication, this value is **common**. |
|alCategory|**Email**|
|alConnector|Specifies the email-provider connector used to send the email. Possible values include: <ul><li>Current User</li><li>Microsoft 365</li><li>SMTP</li><li>Other custom connectors installed by extensions.</li></ul> The connector is specified on the email accounts that are set up in [!INCLUDE[prod_short](../developer/includes/prod_short.md)]. For more information, see [Adding Email Accounts](/dynamics365/business-central/admin-how-setup-email#adding-email-accounts).|
|alDataClassification|**SystemMetadata**|
|alEmailMessageID|Specifies the GUID assigned to email, like C7A56676-9F3F-4044-90F0-D7F3196AC366.|
|alObjectId|**8888**, which is the ID of the system application codeunit that sends emails.|
|alObjectName|**Email Dispatcher**, which is the name of the system application codeunit that sends the emails.|
|alObjectType|**CodeUnit**|
|component|**Dynamics 365 Business Central Server**.|
|componentVersion|Specifies the version number of the component that emits telemetry (see the component dimension.)|
|environmentName|Specifies the name of the tenant environment. See [Managing Environments](tenant-admin-center-environments.md). This dimension isn't included for [!INCLUDE[prod_short](../includes/prod_short.md)] on-premises environments.|
|environmentType|Specifies the environment type for the tenant, such as **Production**, **Sandbox**, **Trial**. See [Environment Types](tenant-admin-center-environments.md#types-of-environments)|
|eventId|**AL0000CTV**|
|telemetrySchemaVersion|Specifies the version of the [!INCLUDE[prod_short](../developer/includes/prod_short.md)] telemetry schema.|

## <a name="failed"></a>Failed to send email

Occurs when an email failed to be sent from the client.

### General dimensions

|Dimension|Description or value|
|---------|-----|
|message|**Failed to send email.**|
|severityLevel|**1**|
|user_Id|[!INCLUDE[user_Id](../includes/include-telemetry-user-id.md)] |

### Custom dimensions

|Dimension|Description or value|
|---------|-----|
|aadTenantId|Specifies the Azure Active Directory (Azure AD) tenant ID used for Azure AD authentication. For on-premises, if you aren't using Azure AD authentication, this value is **common**. |
|alCategory|**Email**|
|alConnector|Specifies the email-provider connector used to send the email. Possible values include: <ul><li>Current User</li><li>Microsoft 365</li><li>SMTP</li><li>Other custom connectors installed by extensions.</li></ul> The connector is specified on the email accounts that are set up in [!INCLUDE[prod_short](../developer/includes/prod_short.md)]. For more information, see [Adding Email Accounts](/dynamics365/business-central/admin-how-setup-email#adding-email-accounts).|
|alDataClassification|**SystemMetadata**|
|alEmailMessageID|Specifies the GUID assigned to email, like C7A56676-9F3F-4044-90F0-D7F3196AC366.|
|alErrorCallStack| Specifies the AL callstack when the error occurred. This dimension was added in version 19.0.|
|alErrorText| Specifies the AL error message. This dimension was added in version 19.0. |
|alObjectId|**8888**, which is the ID of the system application codeunit that sends emails.|
|alObjectName|**Email Dispatcher**, which is the name of the system application codeunit that sends the emails.|
|alObjectType|**CodeUnit**|
|component|**Dynamics 365 Business Central Server**.|
|componentVersion|Specifies the version number of the component that emits telemetry (see the component dimension.)|
|environmentName|Specifies the name of the tenant environment. See [Managing Environments](tenant-admin-center-environments.md). This dimension isn't included for [!INCLUDE[prod_short](../includes/prod_short.md)] on-premises environments.|
|environmentType|Specifies the environment type for the tenant, such as **Production**, **Sandbox**, **Trial**. See [Environment Types](tenant-admin-center-environments.md#types-of-environments)|
|eventId|**AL0000CTP**|
|telemetrySchemaVersion|Specifies the version of the [!INCLUDE[prod_short](../developer/includes/prod_short.md)] telemetry schema.|

> [!TIP]
> You can also view failed emails in the **Email Outbox** page in the [!INCLUDE[prod_short](../developer/includes/prod_short.md)] client.


## See also

[Monitoring and Analyzing Telemetry](telemetry-overview.md)  
[Enable Sending Telemetry to Application Insights](telemetry-enable-application-insights.md)  
