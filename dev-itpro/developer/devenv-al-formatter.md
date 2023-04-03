---
title: "AL Formatter"
description: "The AL Formatter can help you insert and remove space from AL code."
author: SusanneWindfeldPedersen
ms.custom: na
ms.date: 04/01/2021
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.author: solsen
---

# AL Formatter

The [!INCLUDE[d365al_ext_md](../includes/d365al_ext_md.md)] offers users the option to automatically format their source code. This capability increases the usability of the editor by allowing developers to instantly fix the indentation and formatting of their code. 
The auto-formatter analyzes the syntax tree of the AL code that you're formatting. By using rules that are based on the coding and style guidelines for AL, the auto-formatter then inserts and removes whitespace from key points in the document to make it more readable.

> [!NOTE]  
> The rules used by the auto-formatter cannot be configured by the user. This limitation is present to allow for a uniform style to be used throughout the community of AL developers.

## Invoking AL formatter
The auto-formatter can be invoked to format an entire AL document or a pre-selected range. In an existing project, open the document that you want to format, right-click inside the document, and select **Format Document**. In the default configuration for Visual Studio Code, the command can be run using the shortcut **Alt+Shift+F**.

![Format Document.](media/format-document.gif)  

To format a range, in an already opened project, open the document that you want to modify, select the specific range to format, right-click, and select **Format Selection**. In the default configuration for Visual Studio Code, the command can be run using the shortcut **Ctrl+K**, **Ctrl+F**.

![Format Selection.](media/format-selection.gif)

## See Also

[AL Development Environment](devenv-reference-overview.md)  
[AL Outline View](devenv-al-outline-view.md)  
[AL Code Actions](devenv-code-actions.md)  

