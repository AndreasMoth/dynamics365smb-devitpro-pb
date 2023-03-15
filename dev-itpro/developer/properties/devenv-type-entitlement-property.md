---
title: "Type (Entitlement) Property"
description: "The type of entitlement."
ms.author: solsen
ms.custom: na
ms.date: 03/15/2023
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: reference
author: SusanneWindfeldPedersen
---
[//]: # (START>DO_NOT_EDIT)
[//]: # (IMPORTANT:Do not edit any of the content between here and the END>DO_NOT_EDIT.)
[//]: # (Any modifications should be made in the .xml files in the ModernDev repo.)
# Type (Entitlement) Property
> **Version**: _Available or changed with runtime version 7.0._

The type of entitlement. When a user logs into Business Central, it is checked if the user is assigned the given AAD service plan, the given AAD role etc., and if that is the case, the user will be entitled to use the objects covered by this entitlement. The same applies if an application logs into Business Central.

## Applies to
-   Entitlement

## Property Value

|Value|Description|
|-----------|---------------------------------------|
|**PerUserServicePlan**|The entitlement is associated with an AAD service plan which is licensed to specific users.|
|**PerUserOfferPlan**|The entitlement is associated with an offer which is licensed to specific users.|
|**FlatRateServicePlan**|The entitlement is associated with an AAD service plan which is licensed to an AAD tenant.|
|**Role**|The entitlement is associated with an AAD role.|
|**ConcurrentUserServicePlan**|The entitlement is associated with a named AAD group.|
|**Application**|The entitlement is associated with an AAD application.|
|**ApplicationScope**|The entitlement is associated with an AAD application scope.|
|**Implicit**|Everyone has this license.|
|**Unlicensed**|Entitlement applied when no other entitlements from an app has been applied.|

[//]: # (IMPORTANT: END>DO_NOT_EDIT)

## Remarks

> [!NOTE]  
> In the current version of [!INCLUDE [prod_short](../../includes/prod_short.md)] entitlements can only be included with Microsoft apps (enforced by the AppSource cop rules and the technical validation checks that we run for the apps submitted to AppSource). These objects will become available for the ISV apps when we introduce ability to monetize AppSource apps in one of our future releases. For more information, see [Entitlement Object](../devenv-entitlement-object.md).

When `Type` is set to `Role`, the [RoleType Property](devenv-roletype-property.md) is used to further define whether the `RoleType` is `Local` or `Delegated`.


## Syntax

```al
entitlement MyEntitlement
{
    ...
    Type = Role;                    // Entitlement type, if Role, then specify RoleType property
    RoleType = Delegated;
    ObjectEntitlements = 
        ”D365 BUS PREMIUM - BaseApp”;​
}
```

## See Also

[Get Started with AL](../devenv-get-started.md)  
[Type (Report) Property](devenv-type-report-property.md)  