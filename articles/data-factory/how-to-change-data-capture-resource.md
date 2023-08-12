---
title: Capture changed data with a change data capture resource
description: This tutorial provides step-by-step instructions on how to capture changed data from ADLS Gen2 to Azure SQL DB using a Change data capture resource.
author: n0elleli
ms.author: noelleli
ms.reviewer: 
ms.service: data-factory
ms.subservice:
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 06/06/2023
---

# How to capture changed data from ADLS Gen2 to Azure SQL DB using a Change Data Capture (CDC) resource
[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

In this tutorial, you use the Azure Data Factory user interface (UI) to create a new Change Data Capture (CDC) resource that picks up changed data from an Azure Data Lake Storage (ADLS) Gen2 source to an Azure SQL Database in real-time. The configuration pattern in this tutorial can be modified and expanded upon. 

In this tutorial, you follow these steps:
* Create a Change Data Capture resource.
* Monitor Change Data Capture activity.

## Prerequisites

* **Azure subscription.** If you don't have an Azure subscription, create a free Azure account before you begin.
* **Azure storage account.** You use ADLS storage as a source data store. If you don't have a storage account, see Create an Azure storage account for steps to create one.
* **Azure SQL Database.** You use Azure SQL DB as a target data store. If you don’t have an Azure SQL DB, create one in the Azure portal first before continuing the tutorial. 


## Create a change data capture artifact

1.	Navigate to the **Author** blade in your data factory. You see a new top-level artifact below **Pipelines** called **Change Data Capture (preview)**.
  
  :::image type="content" source="media/adf-cdc/change-data-capture-resource-61.png" alt-text="Screenshot of new top level artifact shown under Factory resources panel." lightbox="media/adf-cdc/change-data-capture-resource-61.png":::
  
2.	To create a new **Change Data Capture**, hover over **Change Data Capture (preview)** until you see 3 dots appear. Select on the **Change Data Capture (preview) Actions**.

  :::image type="content" source="media/adf-cdc/change-data-capture-resource-62.png" alt-text="Screenshot of Change Data Capture (preview) Actions after hovering on the new top-level artifact." lightbox="media/adf-cdc/change-data-capture-resource-62.png":::

3.	Select **New CDC (preview)**. This opens a flyout to begin the guided process. 

  :::image type="content" source="media/adf-cdc/change-data-capture-resource-63.png" alt-text="Screenshot of a list of Change Data Capture actions." lightbox="media/adf-cdc/change-data-capture-resource-63.png":::
  
4.	You are prompted to name your CDC resource. By default, the name is set to “adfcdc” and continue to increment up by 1. You can replace this default name with your own. 

  :::image type="content" source="media/adf-cdc/change-data-capture-resource-64.png" alt-text="Screenshot of the text box to update the name of the resource.":::

5.	 Use the drop-down selection list to choose your data source. For this tutorial, we use **DelimitedText**. 

  :::image type="content" source="media/adf-cdc/change-data-capture-resource-65.png" alt-text="Screenshot of the guided process flyout with source options in a drop-down selection menu."::: 

6.	You will then be prompted to select a linked service. Create a new linked service or select an existing one. 

  :::image type="content" source="media/adf-cdc/change-data-capture-resource-93.png" alt-text="Screenshot of the selection box to choose or create a new linked service.":::
  
7.	Use the **Source settings** to optionally set advanced source configurations which includes selection of column or row delimiter and many more source settings.

  :::image type="content" source="media/adf-cdc/change-data-capture-resource-94.png" alt-text="Screenshot of Advanced Source settings to set delimiters":::

> [!NOTE]
> You can choose to manually edit these source settings, but if you don’t, they will be set to the defaults.

8.	Use the **Browse** button to select your source data folder. 

  :::image type="content" source="media/adf-cdc/change-data-capture-resource-95.png" alt-text="Screenshot of a folder icon to browse for a folder path.":::

9.	Once you’ve selected a folder path, select **Continue** to set your data target. 

  :::image type="content" source="media/adf-cdc/change-data-capture-resource-96.png" alt-text="Screenshot of the continue button in the guided process to proceed to select data targets.":::

> [!NOTE]
> You can choose to add multiple source folders with the **+** button. The other sources must also use the same linked service that you’ve already selected. 

10.	Then, select a **Target type** using the drop-down selection. For this tutorial, we select **Azure SQL Database**. 

  :::image type="content" source="media/adf-cdc/change-data-capture-resource-69.png" alt-text="Screenshot of a drop-down selection menu of all data target types.":::

11.	You are prompted to select a linked service. Create a new linked service or select an existing one. 
 
  :::image type="content" source="media/adf-cdc/change-data-capture-resource-70.png" alt-text="Screenshot of the selection box to choose or create a new linked service to your data target.":::
 
12.	Create new **Target table(s)** or select an existing **Target table(s)**. Under **Existing entities** use the checkbox to select an existing Target table(s) or Under **New entities** select **Edit new tables** to create new Target table(s). The **Preview** button allows you to view your table data.

  :::image type="content" source="media/adf-cdc/change-data-capture-resource-71.png" alt-text="Screenshot of the existing entities to choose tables for your target.":::

  :::image type="content" source="media/adf-cdc/change-data-capture-resource-72.png" alt-text="Screenshot of the new entities tab to create new tables for your target.":::
  
> [!NOTE]
> If there are existing table(s) at the Target with matching name(s), they will be selected by default under **Existing entities**. If not, new tables with matching name(s) are created under **New entities**. Additionally, you can edit new tables with **Edit new tables** button.

13.	Select **Continue** when you have finalized your selection(s). 

  :::image type="content" source="media/adf-cdc/change-data-capture-resource-73.png" alt-text="Screenshot of the continue button in the guided process to proceed to the next step.":::

> [!NOTE]
> You can choose multiple target tables from your Azure SQL DB. Use the check boxes to select all targets. 

14.	You automatically land in a new change data capture tab, where you can configure your new resource. 

  :::image type="content" source="media/adf-cdc/change-data-capture-resource-74.png" alt-text="Screenshot of the change data capture studio." lightbox="media/adf-cdc/change-data-capture-resource-74.png":::
 
15.	A new mapping will automatically be created for you. You can update the **Source** and **Target** selections for your mapping by using the drop-down selection lists. 

  :::image type="content" source="media/adf-cdc/change-data-capture-resource-75.png" alt-text="Screenshot of the source to target mapping in the change data capture studio." lightbox="media/adf-cdc/change-data-capture-resource-75.png":::

16.	Once you’ve selected your tables, you should see that their columns are auto mapped by default with the **Auto map** toggle on. Auto map automatically maps the columns by name in the sink, picks up new column changes when source schema evolves and flows this to the supported sink types. If you would want to retain Auto map and not change any column mappings, proceed to **Step 19** directly. 

  :::image type="content" source="media/adf-cdc/change-data-capture-resource-76.png" alt-text="Screenshot of default Auto map toggle set to on." lightbox="media/adf-cdc/change-data-capture-resource-76.png":::

17. If you would want to enable the column mapping(s), select the mapping(s) and switch the Auto map toggle off, and then select the Column mappings button to view the column mappings.

  :::image type="content" source="media/adf-cdc/change-data-capture-resource-77.png" alt-text="Screenshot of mapping selection, Auto map toggle set to off and column mapping button." lightbox="media/adf-cdc/change-data-capture-resource-77.png":::
  
> [!NOTE]
> You can switch back to the default Auto mapping anytime by switching the **Auto map** toggle on.
  
18.	Here you can view your column mappings. Use the drop-down lists to edit your column mappings for Mapping method, Source column, and Target column.

   :::image type="content" source="media/adf-cdc/change-data-capture-resource-78.png" alt-text="Screenshot of the column mapping page to allow users to editing column mappings." lightbox="media/adf-cdc/change-data-capture-resource-78.png":::

You can add more column mappings using the **New mapping** button. Use the drop-down lists to select the **Mapping method**, **Source column**, and **Target** column. Also, if you want to track the delete operation for supported sink types, you can select the **Keys** column. You can select **Data Preview - Refresh** button to visualize how the data looks at the target.
  
  :::image type="content" source="media/adf-cdc/change-data-capture-resource-79.png" alt-text="Screenshot of the Add new mapping icon to add new column mappings, drop down with mapping methods, select Keys column and Data preview refresh button for allowing users to visualize data at target." lightbox="media/adf-cdc/change-data-capture-resource-79.png":::

19.	When your mapping is complete, select the back arrow to return to the main CDC canvas.

  :::image type="content" source="media/adf-cdc/change-data-capture-resource-80.png" alt-text="Screenshot of back button to go back to table mapping page." lightbox="media/adf-cdc/change-data-capture-resource-80.png":::

20.	You can add more source to target mappings in one CDC artifact. Use the Edit button to add more data sources and targets. Then, select **New mapping** and use the drop-down lists to set a new source and target mapping. Also Auto map can be set on or off for each of these mappings independently.

   :::image type="content" source="media/adf-cdc/change-data-capture-resource-81.png" alt-text="Screenshot of the edit button to add new sources and new mapping button to set a new source to target mapping." lightbox="media/adf-cdc/change-data-capture-resource-81.png":::

21.	Once your mappings are complete, set your CDC latency using the **Set Latency** button. 

   :::image type="content" source="media/adf-cdc/change-data-capture-resource-82.png" alt-text="Screenshot of the set frequency button at the top of the canvas." lightbox="media/adf-cdc/change-data-capture-resource-82.png":::
   
22.	Select the latency of your CDC and select **Apply** to make the changes. By default, it is set to **15 minutes**. For this tutorial, we select the **Real-time** latency. Real-time latency will continuously keep picking up changes in your source data in a less than 1-minute interval.

    For other latencies, say if you select 15 minutes, every 15 minutes, your change data capture will process your source data and pick up any changed data since the last processed time.

:::image type="content" source="media/adf-cdc/change-data-capture-resource-83.png" alt-text="Screenshot of the set frequency selection menu.":::

> [!NOTE] 
> Support for **streaming data integration** (EventHub & Kafka data sources) is coming soon. When available the latency will be set to Real-time by default.

23.	Once everything has been finalized, select the **Publish All** to publish your changes. 

:::image type="content" source="media/adf-cdc/change-data-capture-resource-84.png" alt-text="Screenshot of the publish button at the top of the canvas." lightbox="media/adf-cdc/change-data-capture-resource-84.png":::

> [!NOTE] 
> If you do not publish your changes, you will not be able to start your CDC resource. The start button will be greyed out. 

24.	Select **Start** to start running your **Change Data Capture**. 

  :::image type="content" source="media/adf-cdc/change-data-capture-resource-85.png" alt-text="Screenshot of the start button at the top of the canvas." lightbox="media/adf-cdc/change-data-capture-resource-85.png":::
 

## Monitor your Change data capture

1.	To monitor your change data capture, navigate to the **Monitor** blade or select the monitoring icon from the CDC designer. 

  :::image type="content" source="media/adf-cdc/change-data-capture-resource-86.png" alt-text="Screenshot of the monitoring blade.":::
 
  :::image type="content" source="media/adf-cdc/change-data-capture-resource-87.png" alt-text="Screenshot of the monitoring button at the top of the CDC canvas." lightbox="media/adf-cdc/change-data-capture-resource-87.png":::

2.	Select **Change Data Capture (preview)** to view your CDC resources. 

  :::image type="content" source="media/adf-cdc/change-data-capture-resource-88.png" alt-text="Screenshot of the Change Data Capture monitoring section.":::
 
3.	Here you can see the **Source**, **Target**, **Status**, and **Last processed** time of your change data capture. 

  :::image type="content" source="media/adf-cdc/change-data-capture-resource-89.png" alt-text="Screenshot of an overview of the change data capture monitoring page." lightbox="media/adf-cdc/change-data-capture-resource-89.png":::

4.	Select the name of your CDC to see more details. You can see how many changes (insert/update/delete) were read and written and other diagnostic information. 

  :::image type="content" source="media/adf-cdc/change-data-capture-resource-90.png" alt-text="Screenshot of the detailed monitoring of a selected change data capture." lightbox="media/adf-cdc/change-data-capture-resource-90.png":::

> [!NOTE] 
> If you have multiple mappings set up in your Change data capture, each mapping will show as a different color. Click on the bar to see specific details for each mapping or use the Diagnostics at the bottom of the screen. 

  :::image type="content" source="media/adf-cdc/change-data-capture-resource-91.png" alt-text="Screenshot of the detailed monitoring page of a change data capture with multiple sources to target mappings." lightbox="media/adf-cdc/change-data-capture-resource-91.png":::
  
  :::image type="content" source="media/adf-cdc/change-data-capture-resource-92.png" alt-text="Screenshot of a detailed breakdown of each mapping in the change data capture artifact." lightbox="media/adf-cdc/change-data-capture-resource-92.png":::
  
  
## Next steps
- [Learn more about the change data capture resource](concepts-change-data-capture-resource.md)
