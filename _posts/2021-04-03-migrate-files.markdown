---
layout: post
title:  "Welcome to Jekyll!"
categories: [ salesforce ]
---
# Migrate Attachments from One Org to another Using Data Loader

One might wonder how to migrate attachments from one Salesforce org to another Salesforce org using Data Loader. Here is a practical guide on how to do that.
Let us consider 2 orgs, one is source org where all attachments are present and the other is destination org where attachments have to migrate.

```mermaid
graph LR
A[Source] ----> B[Destination]
```

## Steps To Be Executed

 1. Export Attachments using Salesforce standard Data Export feature.
 2. Create Attachments CSV.
 3. Map attachment file path in Attachment CSV body column.
 4. Import attachments to destination org using Data Loader.

## Export Attachments using Salesforce standard "Data Export" feature
Go to Salesforce setup, in Quick find box search for Export and select Data Export under Data. Click on "Export Now" or if you want to schedule and export you can do that using "Schedule Export".

### Export Configuration

under exported data select `include all data` for all attachments or select individual objects to get related attachments.

Salesforce adds your export to the queue and sends you a confirmation email with a link after the export is complete.
Open the link provided in the email and download exported zip folders.
The first zip folder contains all CSV files along with attachments, downloaded attachments are named after their ID with extension type file, which we can ignore as we are only interested in the path and not the actual file. If you want to see the actual file with the proper extension you can refer to [Rename Attachments with Proper Extension](https://www.salesforcelwc.in/rename-attachments). Move all attachments into one folder.

## Modify Attachments CSV
Make sure that the CSV file you want to use for attachment importing contains these required columns. Each column represents a Salesforce field.

-   ParentId—Salesforce ID of the parent record
-   Name—Name of the attachment file, such as  myattachment.jpg
-   Body—Absolute path to the attachment on your local drive
    
Make sure that the values in the Body column contain the full path of the attachments on your computer. For example, if an attachment named  attachment.jpg  is the folder  C:\Export\Attachments, the Body must specify  C:\Export\Attachments\attachment.jpg. Your CSV file looks like this example:
 
| OLD_Attachment_Id | Name | Body | ParentId |    
|--|--|--|--|
|  |Product1.png|C:\Export\Attachments\0017F00000iIxwXQAS|0017F00000iIxwXQAS
|  |Product2.png|C:\Export\Attachments\0017F00000iIxwXQAS|0017F00000iIxwYQAS
|  |Product3.png|C:\Export\Attachments\0017F00000iIxwXQAS|0017F00000iIxwZQAS
|  |Product4.png|C:\Export\Attachments\0017F00000iIxwXQAS|0017F00000iIxwaQAC
|  |Product5.png|C:\Export\Attachments\0017F00000iIxwXQAS|0017F00000iIxwbQAC

Now you are just one step away!

# Import Attachments Using Data Loader
In Data Loader, under settings keep the batch size as 1 or calculate batch size for loading attachments. Select insert operation and check `Show all Salesforce objects` option and select `Attachment` object. Map ParentId, Body, and Name Salesforce fields and perform the Insert operation. If you are new to data loader and don't know how to use it then please refer [Learn how to use salesforce data loader](https://developer.salesforce.com/docs/atlas.en-us.dataLoader.meta/dataLoader/inserting_updating_or_deleting_data.htm).
