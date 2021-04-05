---
layout: post
title: Migrate Attachments from One Org to another Using Salesforce Data Loader
date: '2021-04-03T17:25:00.000+05:30'
categories: [ salesforce ]
permalink: /migrate-attachments-salesforce-data-loader.html
description: Wondering how to migrate attachments from one Salesforce org to another Salesforce org using Data Loader. Here is a practical guide on how to do that.
image: assets/images/attachments/header.png
beforetoc: "Wondering how to migrate attachments from one Salesforce org to another Salesforce org using Data Loader. Here is a practical guide on how to do that."
author: kishore
---
Wondering how to migrate attachments from one Salesforce org to another Salesforce org using Data Loader. Here is a practical guide on how to do that.

Let us consider 2 orgs, one is source org where all attachments are present and the other is destination org where attachments have to migrate.

`[Source] ------> [Destination]`

* Do not remove this line (it will not be displayed)
{:toc}

## Export Attachments using Salesforce standard Data Export feature
Go to Salesforce setup, in Quick find box search for Export and select Data Export under Data. Click on `Export Now` or if you want to schedule and export you can do that using `Schedule Export`.

![Export Button]({{ site.baseurl }}/assets/images/attachments/data-export-button.png)

### Export Configuration

![ Configuration ]({{ site.baseurl }}/assets/images/attachments/configuration.png)

under exported data select `include all data` for all attachments or select individual objects to get related attachments.

![Selected sbjects for export]({{ site.baseurl }}/assets/images/attachments/selected-objects.png)

Salesforce adds your export to the queue and sends you a confirmation email with a link after the export is complete.
Open the link provided in the email and download exported zip folders.
The first zip folder contains all CSV files along with attachments.

> Downloaded attachments are named after their ID with extension type file, you don't need to panic.

![ Attachments ]({{ site.baseurl }}/assets/images/attachments/attachments-noext.png)

If you want to see the actual file with the proper extension you can refer to [Rename Attachments with Proper Extension](https://www.salesforcelwc.in/rename-attachments)<i class="fas fa-external-link-alt"></i>. Move all attachments into one folder.

![Exported Files]({{ site.baseurl }}/assets/images/attachments/exported-files.png)

## Modify Attachments CSV
Make sure that the CSV file you want to use for attachment importing contains these required columns. Each column represents a Salesforce field.

-   ParentId—Salesforce ID of the parent record
-   Name—Name of the attachment file, such as  myattachment.jpg
-   Body—Absolute path to the attachment on your local drive
    
Make sure that the values in the Body column contain the full path of the attachments on your computer. For example, if an attachment named  attachment.jpg  is the folder  `C:\Export\Attachments`, the Body must specify  `C:\Export\Attachments\{Source org Salesforce ID}`. Your CSV file looks like this example:

![ csv ]({{ site.baseurl }}/assets/images/attachments/csv.png)

Below image shoes how attachments are linked to CSV

![ Relation between CSV and Attachment folder]({{ site.baseurl }}/assets/images/attachments/compare-id.png)

Now you are just one step away!

# Import Attachments Using Data Loader

![ dataloader ]({{ site.baseurl }}/assets/images/attachments/dataloader-allfiles.png)

In Data Loader, under settings keep the batch size as 1 or calculate batch size for loading attachments. Select insert operation and check `Show all Salesforce objects` option and select `Attachment` object. Map ParentId, Body, and Name Salesforce fields and perform the Insert operation. If you are new to data loader and don't know how to use it then please refer [Learn how to use salesforce data loader](https://developer.salesforce.com/docs/atlas.en-us.dataLoader.meta/dataLoader/inserting_updating_or_deleting_data.htm){:rel="nofollow"}{:target="_blank"}.
