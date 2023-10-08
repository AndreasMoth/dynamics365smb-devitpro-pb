---
title: "HttpClient.AddCertificate(Text [, Text]) Method"
description: "Adds a certificate to the HttpClient class."
ms.author: solsen
ms.custom: na
ms.date: 03/24/2022
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: reference
author: SusanneWindfeldPedersen
---
[//]: # (START>DO_NOT_EDIT)
[//]: # (IMPORTANT:Do not edit any of the content between here and the END>DO_NOT_EDIT.)
[//]: # (Any modifications should be made in the .xml files in the ModernDev repo.)
# HttpClient.AddCertificate(Text [, Text]) Method
> **Version**: _Available or changed with runtime version 1.0._

Adds a certificate to the HttpClient class.


## Syntax
```AL
 HttpClient.AddCertificate(Certificate: Text [, Password: Text])
```
## Parameters
*HttpClient*  
&emsp;Type: [HttpClient](httpclient-data-type.md)  
An instance of the [HttpClient](httpclient-data-type.md) data type.  

*Certificate*  
&emsp;Type: [Text](../text/text-data-type.md)  
The Base64 encoded certificate.  

*[Optional] Password*  
&emsp;Type: [Text](../text/text-data-type.md)  
The certificate password.  



[//]: # (IMPORTANT: END>DO_NOT_EDIT)

## Example
[!INCLUDE[httpclient_cert_example](../../includes/include-http-cert-example.md)]

## Remarks
The certificate must be in base 64 format.

With the **AddCertificate** method you set the certificates that you want to be associated to the request of the http client connection. These have the only purpose of authenticating the client.

The system caches SSL sessions as they are created and attempts to reuse a cached session for a new request, if possible. When attempting to reuse an SSL session, it uses the first certificate that was added or tries to reuse an anonymous session if no certificates have been specified.

> [!IMPORTANT]
> The certificate must be use one of the supported cipher suites; otherwise, outgoing web service calls will fail with security exception. For more information, see [Supported Cipher Suites](../../devenv-supported-cipher-suites.md).

[!INCLUDE[httpclient_cert_note](includes/include-http-cert-note.md)]

## See Also

[HttpClient Data Type](httpclient-data-type.md)  
[Get Started with AL](../../devenv-get-started.md)  
[Developing Extensions](../../devenv-dev-overview.md)