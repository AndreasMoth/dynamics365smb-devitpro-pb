---
title: "AppSourceCop Error AS0080"
description: "Decreasing the length of a field is not allowed."
ms.author: solsen
ms.custom: na
ms.date: 12/22/2023
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: reference
author: SusanneWindfeldPedersen
---
[//]: # (START>DO_NOT_EDIT)
[//]: # (IMPORTANT:Do not edit any of the content between here and the END>DO_NOT_EDIT.)
[//]: # (Any modifications should be made in the .xml files in the ModernDev repo.)
# AppSourceCop Error AS0080
Fields must not decrease in length

## Description
Decreasing the length of a field is not allowed. This might break the upgrade of existing installations and dependent extensions.

[//]: # (IMPORTANT: END>DO_NOT_EDIT)

## Remarks

The changes validated by this rule are not allowed because they are destructive changes for the sync engine.
These changes might also break the runtime of dependent extensions.

The validation of the length of table fields was previously done with [AS0004](appsourcecop-as0004.md) and has now been split into two different rules:
- [AS0080](appsourcecop-as0080.md) - which validates against decreasing the length of fields
- [AS0086](appsourcecop-as0086.md) - which validates against increasing the length of fields

## How to fix this diagnostic?

Reverting the change will fix this diagnostic. If decreasing the length of the field is required, the recommended approach is to mark the field as [Obsolete Pending](../properties/devenv-obsoletestate-property.md) and introduce a new field with the desired length.
Once all dependent extensions have uptaken the new field, you can mark the the original one as [Obsolete Removed](../properties/devenv-obsoletestate-property.md).


## Code examples triggering the rule

### Example 1 - Decreasing the length of a field

Version 1.0 of the extension:
```AL
table 50100 MyTable
{
    fields
    {
        field(50100; MyField; Text[50]) { }
    }
}
```

Version 2.0 of the extension:
```AL
table 50100 MyTable
{
    fields
    {
        field(50100; MyField; Text[25]) { }
    }
}
```

In version 2.0, the type of the field `MyField` has changed from `Text[50]` to `Text[25]`. 
As this is a breaking change, if version 1.0 was installed on a tenant, it won't be possible to synchronize and upgrade the version 2.0 of the exten
Moreover, if a dependent extension uses this field, this change of length can lead to runtime exceptions.

### Example 2 - Modifying the length of a field which is part of the primary key

Version 1.0 of the extension:
```AL
table 50100 MyTable
{
    fields
    {
        field(50100; MyField; Text[50]) { }
    }
    keys
    {
        field(MyKey; MyField) { }
    }
}
```

Version 2.0 of the extension:
```AL
table 50100 MyTable
{
    fields
    {
        field(50100; MyField; Text[100]) { }
    }
    keys
    {
        field(MyKey; MyField) { }
    }
}
```

In version 2.0, the type of the field `MyField` has changed from `Text[50]` to `Text[100]`. Even if the length has increased, this change is not supported because `MyField` is part of the primary key `MyKey`. 
As this is a breaking change, if version 1.0 was installed on a tenant, it won't be possible to synchronize and upgrade the version 2.0 of the extension. 
Moreover, if a dependent extension uses this field, this change of length can lead to runtime exceptions.

> [!NOTE]  
> When no primary key is explictly defined in the table definition, the first field is used as primary key.

## See Also  
[AppSourceCop Analyzer](appsourcecop.md)  
[Get Started with AL](../devenv-get-started.md)  
[Developing Extensions](../devenv-dev-overview.md)  