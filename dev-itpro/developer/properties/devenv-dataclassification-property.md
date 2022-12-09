---
title: "DataClassification Property"
description: "Specifies the classification to be applied on the data contained in the table."
ms.author: solsen
ms.custom: na
ms.date: 12/08/2022
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: reference
author: SusanneWindfeldPedersen
---
[//]: # (START>DO_NOT_EDIT)
[//]: # (IMPORTANT:Do not edit any of the content between here and the END>DO_NOT_EDIT.)
[//]: # (Any modifications should be made in the .xml files in the ModernDev repo.)
# DataClassification Property
> **Version**: _Available or changed with runtime version 1.0._

Specifies the classification to be applied on the data contained in the table.

## Applies to
-   Table
-   Table Field

## Property Value

|Value|Available or changed with|Description|
|-----------|-----------|---------------------------------------|
|**CustomerContent**|runtime version 1.0|Content directly provided/created by admins and users. CustomerContent is the default value.|
|**EndUserIdentifiableInformation**|runtime version 1.0|(EUII) Data that identifies or could be used to identify the user of a Microsoft service. EUII does not contain Customer content.|
|**AccountData**|runtime version 1.0|Customer billing information and payment instrument information, including administrator contact information, such as tenant administrator’s name, address, or phone number.|
|**EndUserPseudonymousIdentifiers**|runtime version 1.0|(EUPI) An identifier created by Microsoft tied to the user of a Microsoft service. When EUPI is combined with other information, such as a mapping table, it identifies the end user. EUPI does not contain information uploaded or created by the customer (Customer content or EUII)|
|**OrganizationIdentifiableInformation**|runtime version 1.0|(OII) Data that can be used to identify a tenant, generally config or usage data. This data is not linkable to a user and does not contain Customer content.|
|**SystemMetadata**|runtime version 1.0|Data generated while running the service or program that is not linkable to a user or tenant.|
|**ToBeClassified**|runtime version 1.0|Content that has not yet been given a classification. This is the initial value when table or field is created.|

[//]: # (IMPORTANT: END>DO_NOT_EDIT)

## Remarks  

The data classification can be used to adhere to security, compliance, and privacy requirements and processes for collecting, storing, and using user personal information.

FlowField and FlowFilter fields are automatically set to the **SystemMetadata** data classification.  

When the **DataClassification** property is set on a table level, the fields on the table will inherit the property value. If the **DataClassification** property is set on both a table and field level, then the data classification specified on a field level will override the one from the table.
  
## See Also
[Table Properties](devenv-table-properties.md) 