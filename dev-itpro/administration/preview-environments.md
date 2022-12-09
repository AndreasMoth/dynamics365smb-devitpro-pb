---
title: Prepare for major updates with preview environments
description: Learn how to use preview sandboxes to help prepare for the next major update of Business Central online.
author: edupont04

ms.topic: conceptual
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.search.keywords: administration, tenant, admin, environment, sandbox, update
ms.date: 01/06/2022
ms.author: edupont
---

# Prepare for major updates with preview environments

[!INCLUDE [admin-previews](../developer/includes/admin-previews.md)]

The following figure illustrates the suggested steps for getting a preview of a major update with example dates for the two release waves in any given calendar year.  

![Generic timeline for steps to get a preview of a major update with sample dates for the two release waves each year.](../media/update-rollout-timeline-preview.png)

For more information, see [Major Updates and Minor Updates for Business Central Online](update-rollout-timeline.md).  

## Practice and test

Once you have the preview, start using it:

1. Review the new functionality. Try it out, and begin training employees on the new features that are coming.

2. Validate your extensions.

    Upload and install your extensions into the sandbox environments created on the preview version and run through the functionality. Verify that the customization continues to work and is compatible with the new version.  

    In rare cases, if you discover any changes required for your per-tenant extension to become compatible with the next release, apply the changes to your app, test it again on a sandbox environment running on a preview version. Then, if tests complete successfully, upload the app into your production environment, setting the **Deploy to** field to *Next major version*. This way, the compatible version of your app will be used when you schedule the upgrade of your production environment to the new major update once it becomes available.

3. Test the quality. If you run into issues related to the preview, please provide [feedback on Collaborate](#provide-feedback-on-the-preview).

Finally, as always, if you have ideas for features you would like to see in future releases of [!INCLUDE [prod_short](../developer/includes/prod_short.md)], let us know at [https://aka.ms/bcideas](https://aka.ms/bcideas).

## Provide feedback on the preview

We need your feedback on the preview! Let us know about your experiences with the new version. You can provide feedback on Microsoft Collaborate:

1. Browse to [aka.ms/collaborate](https://aka.ms/collaborate)

2. If you have not registered before, complete the registration form to access Collaborate

3. Choose **Engagements**, and then choose the **Join** action

4. Under the **Ready! for Dynamics 365 Business Central** program, find the latest preview version in the list of available engagements, and then choose the **Join** action

Once you have joined the preview engagement, you can submit your feedback by filling in the form. You must specify if you are reporting a technical issue, a translation issue, or a documentation issue. The default severity is set to 3, which is the normal severity for most defects and means that the system is not crashing and users are not blocked from doing their work, but you can change that. When we have triaged your issue, you can see changes in Collaborate.

> [!TIP]
> If you get stuck trying to join Collaborate, please contact [dyn365bep@microsoft.com](mailto:dyn365bep@microsoft.com) with as many details about the error that you got and other circumstances.

## See also

[Major Updates of Business Central Online](update-rollout-timeline.md)  
[Managing Major and Minor Updates of Business Central Online](tenant-admin-center-update-management.md)  
[Working with Administration Tools](administration.md)  
[The Business Central Administration Center](tenant-admin-center.md)  
[Managing Environments](tenant-admin-center-environments.md)  
[Managing Tenant Notifications](tenant-admin-center-notifications.md)  
[Step 4: Getting access to preview bits](../developer/readiness/get-started.md#step-4-getting-access-to-preview-bits)  
