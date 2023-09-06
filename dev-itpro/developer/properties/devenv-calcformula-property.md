---
title: "CalcFormula Property"
description: "Sets the Calculation formula for a FlowField."
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
# CalcFormula Property
> **Version**: _Available or changed with runtime version 1.0._

Sets the Calculation formula for a FlowField.
The following syntax is valid for the CalcFormula property:

```
CalcFormula =
[-]Exist(<DestinationTable> [WHERE (<TableFilters>)]) |
Count(<DestinationTable> [WHERE (<TableFilters>)]) |
[-]Sum(<DestinationTable>.<DestinationFieldName> [WHERE(<TableFilters>)])|
[-]Average(<DestinationTable>.<DestinationFieldName> [WHERE(<TableFilters>)]) |
Min(<DestinationTable>.<DestinationFieldName> [WHERE(<TableFilters>)]) |
Max(<DestinationTable>.<DestinationFieldName> [WHERE(<TableFilters>)]) |
Lookup(<DestinationTable>.<DestinationFieldName> [WHERE(<TableFilters>)])
<TableFilters> ::= <TableFilter> {,<TableFilter>}
<TableFilter> ::=
<DestinationFieldName> = CONST(<FieldConst>) | FILTER(<Filter>) | FIELD(<SourceFieldName>) | FIELD(UPPERLIMIT(<SourceFieldName>)) |
FIELD(FILTER(<SourceFieldName>)) | FIELD(UPPERLIMIT(FILTER(<SourceFieldName>)))
```


## Applies to
-   Table Field

[//]: # (IMPORTANT: END>DO_NOT_EDIT)

## Syntax

```AL
CalcFormula = Count(RecordCalcFields);
```
  
## Remarks

Parts of the formula are described in the following table.  

|Symbol|Description|  
|------|-----------|  
|`<TableNo>`|Specifies the table holding the information to be used in the FlowField.|  
|`<FieldNo>`|Specifies the column from which you want to compute values.|  
|`<TableFilters>`|A list of filters to be used in the computation of the FlowField.|  
|`<TableFilter>`|A table filter can be one of the following: a constant expression, a filter expression, a value from ordinary fields, or a FlowFilter field. A key for the other table must exist and include the fields used in the filters.|  
|`<DstFieldNo>`|Specifies the destination field number.|  
|`<SrcFieldNo>`|Specifies the source field number.|  
|`<Filter>`|A filter expression such as 10&#124;20..30.|  

You can choose from several methods of calculations including sum (total), average, maximum value, minimum value, record count, lookup, and whether a record exists.  

## See also

[Get Started with AL](../devenv-get-started.md)  
[Developing Extensions](../devenv-dev-overview.md)  