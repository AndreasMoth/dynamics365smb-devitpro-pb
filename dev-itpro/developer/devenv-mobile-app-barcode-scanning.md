---
title: Barcode scanning in mobile app
description: Learn how to add bar code scanning capability to the Business Central mobile app 
author: jswymer
ms.author: jswymer
ms.reviewer: jswymer
ms.topic: conceptual
ms.collection: get-started
ms.date: 10/05/2023
ms.custom: bap-template 
ms-service: dynamics365-business-central
---

<!--Remove all the comments in this template before you sign-off or merge to the main branch.-->

<!--This template provides the basic structure of a concept article. See [Write a concept article](write-a-concept-article.md) in the contributor guide. To provide feedback on this template contact [bace feedback team](mailto:templateswg@microsoft.com).-->

<!--H1 - Required. This should match the title you entered in the metadata. Set expectations for what the content covers, so customers know the content meets their needs. Should NOT begin with a verb.-->

# Adding barcode scanning to the mobile app 

The Business Central mobile app supports a native barcode scanner control that enables you to add a  increases warehouse users' productivity as they can scan barcodes using the device camera or even the dedicated barcode scanner. This feature also opens scenarios for partners to create more advanced experiences using a barcode scanner.
The new barcode scanning feature supports three different scenarios, each with varying levels of complexity. The scenarios range from simple user interface (UI) features to more advanced approaches that cater to ISVs.



Add a barcode scanning button on a field

The simplest way to provide barcode scanning capability in the mobile app is by adding a button on a field that starts the barcode scanner built in to the device's camera. you To enable the barcode scanning action on a field, the ExtendedDatatype property in AL code must be set to Barcode. Pages with such fields, which are only supported for text and code data types, will automatically display a barcode scanning button in the UI, enabling scanning via the device camera. This scanning is highly efficient and responsive, featuring mobile OS level processing and supporting the most well-known 1D and 2D barcode formats. Once a barcode is scanned, its value is entered in the field on the page, and the focus moves to the next quick-entry field on the page. This feature is supported on both iOS and Android platforms.

Scenario 2: AL action

AL developers are also able to trigger the barcode scanning UI via an AL-based action, so the barcode scanning can be started via a button, link, or some other semiautomated logic (for instance, when a page is opened). Also supported on iOS and Android platforms, this scenario uses the same camera-based scanning technology as scenario 1 and returns the scanned barcode value to AL code for further processing.

Scenario 3: Barcode event

This scenario targets professional hardware devices, typically with laser-based barcode scanners, offering greater flexibility to developers. It is only supported by hardware barcode scanners, such as Zebra or Datalogic, running Android 11 and above (there’s no support for iOS). With this scenario, developers register a barcode subscriber that listens for subsequent barcode events on the AL side. When the hardware scans a barcode, its value is sent to the Business Central mobile app and then to AL code. In other words, AL code can intercept an event from an Android device and process the decoded barcode further. Additionally, this scenario supports scanning barcodes and building up a document without interacting with any UI.
## Section heading

<!--add your content here-->

<!--Next steps - Required. Provide at least one next step and no more than three. Include some context so the customer can determine why they would click the link.-->
## Next steps

<!--Remove all the comments in this template before you sign-off or merge to the main branch.-->
