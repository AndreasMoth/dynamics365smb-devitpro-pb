---
title: "BlankNumbers Property"
description: "Indicates whether the system will clear a range of numbers as it formats them."
ms.author: solsen
ms.custom: na
ms.date: 06/15/2022
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: reference
author: SusanneWindfeldPedersen
---
[//]: # (START>DO_NOT_EDIT)
[//]: # (IMPORTANT:Do not edit any of the content between here and the END>DO_NOT_EDIT.)
[//]: # (Any modifications should be made in the .xml files in the ModernDev repo.)
# BlankNumbers Property
> **Version**: _Available or changed with runtime version 1.0._

Indicates whether the system will clear a range of numbers as it formats them.

## Applies to
-   Table Field
-   Page Field

## Property Value

|Value|Description|
|-----------|---------------------------------------|
|**DontBlank**|Not clear any numbers. This is the default value.|
|**BlankNeg**|Clear negative numbers.|
|**BlankNegAndZero**|Clear negative numbers and zero.|
|**BlankZero**|Clear numbers equal to zero.|
|**BlankZeroAndPos**|Clear positive numbers and zero.|
|**BlankPos**|Clear positive numbers.|

[//]: # (IMPORTANT: END>DO_NOT_EDIT)

## Syntax  
```AL
BlankNumbers = BlankNegAndZero;
```

## See Also  
 [BlankZero Property](devenv-blankzero-property.md)