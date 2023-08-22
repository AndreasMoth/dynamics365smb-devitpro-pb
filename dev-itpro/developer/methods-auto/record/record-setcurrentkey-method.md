---
title: "Record.SetCurrentKey(Any [, Any,...]) Method"
description: "Selects a key for a table."
ms.author: solsen
ms.custom: na
ms.date: 07/07/2021
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: reference
author: SusanneWindfeldPedersen
---
[//]: # (START>DO_NOT_EDIT)
[//]: # (IMPORTANT:Do not edit any of the content between here and the END>DO_NOT_EDIT.)
[//]: # (Any modifications should be made in the .xml files in the ModernDev repo.)
# Record.SetCurrentKey(Any [, Any,...]) Method
> **Version**: _Available or changed with runtime version 1.0._

Selects a key for a table.


## Syntax
```AL
[Ok := ]  Record.SetCurrentKey(Field1: Any [, Field2: Any,...])
```
## Parameters
*Record*  
&emsp;Type: [Record](record-data-type.md)  
An instance of the [Record](record-data-type.md) data type.  

*Field1*  
&emsp;Type: [Any](../any/any-data-type.md)  
  
*[Optional] Field2*  
&emsp;Type: [Any](../any/any-data-type.md)  
  


## Return Value
*[Optional] Ok*  
&emsp;Type: [Boolean](../boolean/boolean-data-type.md)  
**true** if the operation was successful; otherwise **false**.   If you omit this optional return value and the operation does not execute successfully, a runtime error will occur.  


[//]: # (IMPORTANT: END>DO_NOT_EDIT)

## Remarks 

You can use SetCurrentKey for selecting a key and sorting. When you use SetCurrentKey the following rules apply:  

- Inactive fields are ignored. Only active keys are scanned.  

- When searching for a key, the first occurrence of the specified fields is selected. This means the following:  

  - If you specify only one field as a parameter when you call SetCurrentKey, the key that is actually selected may consist of more than one field.  

  - If the field that you specify is the first component of several keys, the key that is selected may not be the key that you expect.  

  - If no keys can be found that include the fields that you specify, the return value is **false**.
  
## See Also
[Record Data Type](record-data-type.md)  
[Get Started with AL](../../devenv-get-started.md)  
[Developing Extensions](../../devenv-dev-overview.md)  
[SetCurrentKey, SetRange, SetFilter, GetRangeMin, and GetRangeMax Methods](../../devenv-setcurrentkey-setrange-setfilter-getrangemin-and-getrangemax-methods.md)  
