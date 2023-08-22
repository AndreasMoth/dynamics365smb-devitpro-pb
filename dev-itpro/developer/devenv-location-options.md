---
title: "LocationOptions Overview"
ms.custom: na
ms.date: 04/01/2021
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: overview
author: blrobl
description: Discover Dynamics365 Business Central Location Options - EnableHighAccuracy, Timeout, MaximumAge & more. Enhance your AL implementation.
---
# LocationOptions Overview
When implementing location from AL, there are some options that you can optionally pass to `LocationProvider.RequestLocationAsync(options)`. The options can be accessed by using `Microsoft.Dynamics.Nav.Client.Capabilities.LocationOptions` found in the `Microsoft.Dynamics.Nav.ClientExtensions` dll.  

 For most scenarios it is not necessary to specify options.  

## Location Options List  

`bool EnableHighAccuracy`

A value to provide a hint to the device that this request must have the best possible location accuracy.

`int Timeout`

The maximum length of time \(milliseconds\) that is allowed to pass to a location request.

`int MaximumAge`
The maximum length of time \(milliseconds\) of a cached location. 

## See Also  
[Implementing Location in AL](devenv-implement-location-al.md)  
  
