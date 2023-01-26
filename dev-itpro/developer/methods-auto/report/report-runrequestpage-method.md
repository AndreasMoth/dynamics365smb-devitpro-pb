---
title: "Report.RunRequestPage(Integer [, Text]) Method"
description: "Runs the request page for a report without running the report."
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
# Report.RunRequestPage(Integer [, Text]) Method
> **Version**: _Available or changed with runtime version 1.0._

Runs the request page for a report without running the report. Returns an XML string that contains the request page parameters that are entered on the request page.


## Syntax
```AL
Parameters :=   Report.RunRequestPage(Number: Integer [, Parameters: Text])
```
## Parameters
*Number*  
&emsp;Type: [Integer](../integer/integer-data-type.md)  
The ID of the report for which you want to run the request page. If the report that you specify does not exist, then a run-time error occurs.
          

*[Optional] Parameters*  
&emsp;Type: [Text](../text/text-data-type.md)  
A string of request page parameters as XML to use to run the report.
          


## Return Value
*Parameters*  
&emsp;Type: [Text](../text/text-data-type.md)  
XML string that contains the request page parameters that are entered on the request page


[//]: # (IMPORTANT: END>DO_NOT_EDIT)

## Remarks  

This method opens the request page for the specified report to allow the user to provide parameters for the report. When the user closes the request page by choosing the **OK** button, a string that contains the parameter values that were set by the user is returned. When the user chooses the **Cancel** button, an empty string will be returned. The returned parameter string can be picked up by calling one of the following methods:  

- [Execute Method](../report/report-execute-method.md)  

- [Print Method](../report/report-print-method.md)  

- [SaveAs Method](../report/report-saveas-method.md)  

> [!NOTE]  
> You can use these methods to schedule reports in the job queue.  

Because the request page runs in the context of where it was invoked from, users cannot bookmark a link to this page from the user interface.  

## Example

This example illustrates how to use the RunRequestPage method to run the request page for report ID 206 Sales Invoice. The request page parameters are saved to a table, and then uses the parameters with the Execute, SaveAs, and Print methods to preview the report, save it as a PDF file, and print it.  

This example requires that you create a table for holding parameters that are entered on the report request page and a codeunit that runs the report methods.  

Create a table called **Request Parameters** that has the following fields.  

```al
...
fields
    {
        field(1; ReportId; Integer)
        {
            DataClassification = ToBeClassified;

        }
        field(2; UserId; Code[100])
        {
            DataClassification = ToBeClassified;
        }
        field(3; Parameters; Blob)
        {
            DataClassification = ToBeClassified;
        }
    }
...
``` 

Create a codeunit and add the following code to the *OnRun* trigger of the codeunit.  

```al
var
    ReportParameters: Record "Report Parameters";
    XmlParameters: Text;
    OStream: OutStream;
    IStream: InStream;
    CurrentUser: Code[100];
    Content: File;
    TempFileName: Text;

begin
    // Use the Report.RunRequestPage method to run the request page to get report parameters  
    XmlParameters := Report.RunRequestPage(206);  
    CurrentUser := UserId;  
    
    // Save the request page parameters to the database table  
    with ReportParameters do begin  
        // Cleanup  
        if Get(206,CurrentUser) then  
        Delete;  
    
        SetAutoCalcFields(Parameters);  
        ReportId := 206;  
        UserId := CurrentUser;  
        Parameters.CreateOutStream(OStream,TextEncoding::UTF8);  
        Message(XmlParameters);  
        OStream.WriteText(XmlParameters);  
    
        Insert;  
    end;  
    
    Clear(ReportParameters);  
    XmlParameters := '';  
    
    // Read the request page parameters from the database table  
    with ReportParameters do begin  
        SetAutoCalcFields(Parameters);  
        Get(206,CurrentUser);  
        Parameters.CreateInStream(IStream,TextEncoding::UTF8);  
        IStream.ReadText(XmlParameters);  
    end;  
    
    // Use the Report.SaveAs method to save the report as a PDF file  
    Content.Create('TestFile.pdf');  
    Content.CreateOutStream(OStream);  
    Report.SaveAs(206,XmlParameters,ReportFormat::Pdf,OStream);  
    Content.Close;  
    
    // Use the Report.Execute method to preview the report  
    Report.Execute(206,XmlParameters);  
    
    // Use the Report.Print method to print the report  
    Report.Print(206,XmlParameters);  

```  


## See Also
[Report Data Type](report-data-type.md)  
[Get Started with AL](../../devenv-get-started.md)  
[Developing Extensions](../../devenv-dev-overview.md)