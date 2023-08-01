---
title: "HttpClient.Send(HttpRequestMessage, var HttpResponseMessage) Method"
description: "Sends an HTTP request as an asynchronous operation."
ms.author: solsen
ms.custom: na
ms.date: 03/02/2023
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: reference
author: SusanneWindfeldPedersen
---
[//]: # (START>DO_NOT_EDIT)
[//]: # (IMPORTANT:Do not edit any of the content between here and the END>DO_NOT_EDIT.)
[//]: # (Any modifications should be made in the .xml files in the ModernDev repo.)
# HttpClient.Send(HttpRequestMessage, var HttpResponseMessage) Method
> **Version**: _Available or changed with runtime version 1.0._

Sends an HTTP request as an asynchronous operation.


## Syntax
```AL
[Ok := ]  HttpClient.Send(Request: HttpRequestMessage, var Response: HttpResponseMessage)
```
## Parameters
*HttpClient*  
&emsp;Type: [HttpClient](httpclient-data-type.md)  
An instance of the [HttpClient](httpclient-data-type.md) data type.  

*Request*  
&emsp;Type: [HttpRequestMessage](../httprequestmessage/httprequestmessage-data-type.md)  
The HTTP request message to send.  

*Response*  
&emsp;Type: [HttpResponseMessage](../httpresponsemessage/httpresponsemessage-data-type.md)  
The response received from the remote endpoint.  


## Return Value
*[Optional] Ok*  
&emsp;Type: [Boolean](../boolean/boolean-data-type.md)  
Accessing the HttpContent property of HttpResponseMessage in a case when the request fails will result in an error. If you omit this optional return value and the operation does not execute successfully, a runtime error will occur.  


[//]: # (IMPORTANT: END>DO_NOT_EDIT)
## Supported HTTP methods

[!INCLUDE[SupportedHTTPmethods](../../../includes/include-http-methods.md )]

## See Also
[HttpClient Data Type](httpclient-data-type.md)  
[Get Started with AL](../../devenv-get-started.md)  
[Developing Extensions](../../devenv-dev-overview.md)