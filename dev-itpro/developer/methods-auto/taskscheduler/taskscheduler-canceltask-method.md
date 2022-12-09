---
title: "TaskScheduler.CancelTask(Guid) Method"
description: "Cancels and deletes a scheduled task that runs a specific codeunit."
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
# TaskScheduler.CancelTask(Guid) Method
> **Version**: _Available or changed with runtime version 1.0._

Cancels and deletes a scheduled task that runs a specific codeunit.


## Syntax
```AL
[Ok := ]  TaskScheduler.CancelTask(Task: Guid)
```
## Parameters
*Task*  
&emsp;Type: [Guid](../guid/guid-data-type.md)  
Specifies the unique identifier of the task. The unique identifier is returned by the CREATETASK method.
          


## Return Value
*[Optional] Ok*  
&emsp;Type: [Boolean](../boolean/boolean-data-type.md)  



[//]: # (IMPORTANT: END>DO_NOT_EDIT)

## Remarks  
 Scheduled tasks are recorded in table **2000000175 Scheduled Task**. CancelTask removes the task entry from the table.  

 CancelTask can only cancel pending tasks. It cannot cancel a task that is in process. To see an example of CancelTask in use, refer to AL code of table **472 Job Queue Entry**.  

 For more information about tasks and TaskScheduler data type methods, see managing tasks [Task Scheduler](../../devenv-task-scheduler.md).  

## See Also
[TaskScheduler Data Type](taskscheduler-data-type.md)  
[Get Started with AL](../../devenv-get-started.md)  
[Developing Extensions](../../devenv-dev-overview.md)