---
title: "CodeCop Warning AA0074"
description: "TextConst and Label variable names should have a suffix (an approved three-letter suffix: Msg, Tok, Err, Qst, Lbl, Txt) describing usage."
ms.author: solsen
ms.custom: na
ms.date: 12/07/2021
ms.reviewer: na
ms.topic: reference
author: SusanneWindfeldPedersen
---
[//]: # (START>DO_NOT_EDIT)
[//]: # (IMPORTANT:Do not edit any of the content between here and the END>DO_NOT_EDIT.)
[//]: # (Any modifications should be made in the .xml files in the ModernDev repo.)
# CodeCop Warning AA0074
TextConst and Label variable names should have an approved suffix.

## Description
TextConst and Label variable names should have a suffix (an approved three-letter suffix: Msg, Tok, Err, Qst, Lbl, Txt) describing usage.

[//]: # (IMPORTANT: END>DO_NOT_EDIT)

## Remarks

|Three-letter suffix  |Meaning  |
|---------------------|---------|
|Msg                  |Message  |
|Tok                  |Token   |
|Err                  |Error  |
|Qst                  |StrMenu or Confirm |
|Lbl                  |Label, Caption |
|Txt                  |Text |

## Example

```AL
// TextConst suffixed with Tok for Token
MethodGetTok : TextConst ENU = 'GET';
```

## See Also  
[CodeCop Analyzer](codecop.md)  
[Get Started with AL](../devenv-get-started.md)  
[Developing Extensions](../devenv-dev-overview.md)  