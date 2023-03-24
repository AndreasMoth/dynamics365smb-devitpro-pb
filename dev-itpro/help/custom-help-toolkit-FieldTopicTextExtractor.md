---
title: The FieldTopicTextExtractor tool 
description: Learn about the FieldTopicTextExtractor tool in the custom Help toolkit for Business Central and how it can help you convert field-level Help from Dynamics NAV to the Business Central format. 
author: jowilco

ms.reviewer: edupont
ms.topic: conceptual
ms.date: 10/25/2021
ms.author: jowilco
---

# Custom Help Toolkit: The FieldTopicTextExtractor tool

If you're migrating a solution from Dynamics NAV to [!INCLUDE [prod_short](../developer/includes/prod_short.md)], you can use this tool to export the text contained in the first paragraph of field-level Help articles for Microsoft Dynamics NAV 2018 or earlier versions. You can then use these paragraphs as tooltip text in your [!INCLUDE [prod_short](../developer/includes/prod_short.md)] solution.  

> [!NOTE]
> The tool does not import the tooltip text directly into your [!INCLUDE [prod_short](../developer/includes/prod_short.md)] page objects. It creates a spreadsheet that will make it easier to reuse the text from field-level Help topics in your [!INCLUDE [prod_short](../developer/includes/prod_short.md)] solution.

## Use the FieldTopicTextExtractor tool to extract content to Excel

Use the **FieldTopicTextExtractor** tool to populate an Excel workbook with the field IDs and the corresponding first paragraph from a set of HTML articles from the [!INCLUDE [navnow_md](../developer/includes/navnow_md.md)] field-level Help. The Dynamics NAV field articles have a name in the format `T_10_20.html` for field 20 on table 10. The name of the resulting Excel spreadsheet will be in the format Field_topic_summaries\*.xlsx.

Here's the syntax for FieldTopicTextExtractor.exe:  

```cmd
FieldTopicTextExtractor.exe --n --o
```

Here's an explanation of the parameters:

|Parameter   |Description  |
|------------|-------------|
|n|Specifies the path to the HTML files with Dynamics NAV field articles. |
|o|Specifies the path where the Excel spreadsheet must be created.|

## Examples

The following example extracts the introductory paragraph from T_\*.html files in D:\NAV to an Excel spreadsheet with the name Field_topic_summaries\*.xlsx in D:\BC:

```cmd
FieldTopicTextExtractor.exe --n D:\NAV --o D:\BC
```

## After extracting the Dynamics NAV field article text

Excel makes it easy to bulk-apply and bulk-edit text strings because you can sort and filter data. If you work with dedicated technical writers, then they're probably more efficient if they can work with tooltip text in Excel prior to adding tooltip text to your [!INCLUDE [prod_short](../developer/includes/prod_short.md)] solution. You can also take the opportunity to revise the text. For more information, see [Guidelines for tooltip text](../user-assistance.md#guidelines-for-tooltip-text).  

Before you import the tooltips into your solution, map the original field IDs to page control IDs in your [!INCLUDE [prod_short](../developer/includes/prod_short.md)] solution. It requires knowledge of the actual solution to determine which tooltips are relevant for which pages, and Excel is handy to help you in this process.  

For example, the **No** field from the **Customer** table appears in several page objects. You can filter by the field name, copy or tweak the text, and then map the relevant strings to the relevant page controls.  

## See also

[Custom Help Toolkit](custom-help-toolkit.md)  
[Migrate Legacy Help to the Business Central Format](../upgrade/migrate-help.md)  

[!INCLUDE [footer-banner](../includes/footer-banner.md)]
