---
title: "Självstudier: Använd REST API för att skapa ett Azure Data Factory-pipeline | Microsoft Docs"
description: "I den här självstudiekursen kommer du att använda REST API för att skapa en Azure-datafabrik och kopiera data från ett Azure-blob till en Azure SQL-databas."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 1704cdf8-30ad-49bc-a71c-4057e26e7350
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 6663774497aa18aa98e7e8c5aed6183c599b2172
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="tutorial-use-rest-api-to-create-an-azure-data-factory-pipeline-to-copy-data"></a><span data-ttu-id="0a320-103">Självstudier: Använd REST API för att skapa ett Azure Data Factory-pipeline för att kopiera data</span><span class="sxs-lookup"><span data-stu-id="0a320-103">Tutorial: Use REST API to create an Azure Data Factory pipeline to copy data</span></span> 
> [!div class="op_single_selector"]
> * [<span data-ttu-id="0a320-104">Översikt och förutsättningar</span><span class="sxs-lookup"><span data-stu-id="0a320-104">Overview and prerequisites</span></span>](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [<span data-ttu-id="0a320-105">Guiden Kopiera</span><span class="sxs-lookup"><span data-stu-id="0a320-105">Copy Wizard</span></span>](data-factory-copy-data-wizard-tutorial.md)
> * [<span data-ttu-id="0a320-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="0a320-106">Azure portal</span></span>](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [<span data-ttu-id="0a320-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0a320-107">Visual Studio</span></span>](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [<span data-ttu-id="0a320-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="0a320-108">PowerShell</span></span>](data-factory-copy-activity-tutorial-using-powershell.md)
> * [<span data-ttu-id="0a320-109">Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="0a320-109">Azure Resource Manager template</span></span>](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [<span data-ttu-id="0a320-110">REST API</span><span class="sxs-lookup"><span data-stu-id="0a320-110">REST API</span></span>](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [<span data-ttu-id="0a320-111">.NET-API</span><span class="sxs-lookup"><span data-stu-id="0a320-111">.NET API</span></span>](data-factory-copy-activity-tutorial-using-dotnet-api.md)
> 
> 

<span data-ttu-id="0a320-112">I den här artikeln får du lära dig hur du använder REST API för att skapa en datafabrik med en pipeline som kopierar data från en Azure-bloblagring till en Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="0a320-112">In this article, you learn how to use REST API to create a data factory with a pipeline that copies data from an Azure blob storage to an Azure SQL database.</span></span> <span data-ttu-id="0a320-113">Om du inte har använt Azure Data Factory, bör du läsa igenom artikeln [Introduktion till Azure Data Factory](data-factory-introduction.md) innan du genomför den här självstudien.</span><span class="sxs-lookup"><span data-stu-id="0a320-113">If you are new to Azure Data Factory, read through the [Introduction to Azure Data Factory](data-factory-introduction.md) article before doing this tutorial.</span></span>   

<span data-ttu-id="0a320-114">I den här självstudien får du skapa en pipeline i en aktivitet: kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="0a320-114">In this tutorial, you create a pipeline with one activity in it: Copy Activity.</span></span> <span data-ttu-id="0a320-115">Kopieringsaktiviteten kopierar data från källans datalager till mottagarens datalager.</span><span class="sxs-lookup"><span data-stu-id="0a320-115">The copy activity copies data from a supported data store to a supported sink data store.</span></span> <span data-ttu-id="0a320-116">En lista över datakällor som stöds som källor och mottagare finns i [datalager som stöds](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="0a320-116">For a list of data stores supported as sources and sinks, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="0a320-117">Aktiviteten drivs av en globalt tillgänglig tjänst som kan kopiera data mellan olika datalager på ett säkert, tillförlitligt och skalbart sätt.</span><span class="sxs-lookup"><span data-stu-id="0a320-117">The activity is powered by a globally available service that can copy data between various data stores in a secure, reliable, and scalable way.</span></span> <span data-ttu-id="0a320-118">Se artikeln [Dataförflyttningsaktiviteter](data-factory-data-movement-activities.md) för information om kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="0a320-118">For more information about the Copy Activity, see [Data Movement Activities](data-factory-data-movement-activities.md).</span></span>

<span data-ttu-id="0a320-119">En pipeline kan ha fler än en aktivitet.</span><span class="sxs-lookup"><span data-stu-id="0a320-119">A pipeline can have more than one activity.</span></span> <span data-ttu-id="0a320-120">Du kan länka två aktiviteter (köra en aktivitet efter en annan) genom att ställa in datauppsättningen för utdata för en aktivitet som den inkommande datauppsättningen för den andra aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="0a320-120">And, you can chain two activities (run one activity after another) by setting the output dataset of one activity as the input dataset of the other activity.</span></span> <span data-ttu-id="0a320-121">Mer information finns i [flera aktiviteter i en pipeline](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span><span class="sxs-lookup"><span data-stu-id="0a320-121">For more information, see [multiple activities in a pipeline](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span></span>

> [!NOTE]
> <span data-ttu-id="0a320-122">Den här artikeln beskriver inte hela REST-API:et för Data Factory.</span><span class="sxs-lookup"><span data-stu-id="0a320-122">This article does not cover all the Data Factory REST API.</span></span> <span data-ttu-id="0a320-123">Omfattande dokumentation om Data Factory-cmdlets finns i [referensen för REST-API:et för Data Factory](/rest/api/datafactory/).</span><span class="sxs-lookup"><span data-stu-id="0a320-123">See [Data Factory REST API Reference](/rest/api/datafactory/) for comprehensive documentation on Data Factory cmdlets.</span></span>
>  
> <span data-ttu-id="0a320-124">Datapipelinen i den här självstudien kopierar data från ett källdatalager till ett måldatalager.</span><span class="sxs-lookup"><span data-stu-id="0a320-124">The data pipeline in this tutorial copies data from a source data store to a destination data store.</span></span> <span data-ttu-id="0a320-125">Om du vill se en självstudie som visar hur du omvandlar data med Azure Data Factory går du till [Tutorial: Build a pipeline to transform data using Hadoop cluster](data-factory-build-your-first-pipeline.md) (Självstudie: Bygg en pipeline för att omvandla data med Hadoop-kluster).</span><span class="sxs-lookup"><span data-stu-id="0a320-125">For a tutorial on how to transform data using Azure Data Factory, see [Tutorial: Build a pipeline to transform data using Hadoop cluster](data-factory-build-your-first-pipeline.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0a320-126">Krav</span><span class="sxs-lookup"><span data-stu-id="0a320-126">Prerequisites</span></span>
* <span data-ttu-id="0a320-127">Gå igenom [Självstudier – översikt](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) och slutför de **nödvändiga** stegen.</span><span class="sxs-lookup"><span data-stu-id="0a320-127">Go through [Tutorial Overview](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) and complete the **prerequisite** steps.</span></span>
* <span data-ttu-id="0a320-128">Installera [Curl](https://curl.haxx.se/dlwiz/) på din dator.</span><span class="sxs-lookup"><span data-stu-id="0a320-128">Install [Curl](https://curl.haxx.se/dlwiz/) on your machine.</span></span> <span data-ttu-id="0a320-129">Du kan använda verktyget Curl med REST-kommandon för att skapa en datafabrik.</span><span class="sxs-lookup"><span data-stu-id="0a320-129">You use the Curl tool with REST commands to create a data factory.</span></span> 
* <span data-ttu-id="0a320-130">Gör följande genom att följa anvisningarna i [den här artikeln](../azure-resource-manager/resource-group-create-service-principal-portal.md):</span><span class="sxs-lookup"><span data-stu-id="0a320-130">Follow instructions from [this article](../azure-resource-manager/resource-group-create-service-principal-portal.md) to:</span></span> 
  1. <span data-ttu-id="0a320-131">Skapa en webbapp med namnet **ADFCopyTutorialApp** i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="0a320-131">Create a Web application named **ADFCopyTutorialApp** in Azure Active Directory.</span></span>
  2. <span data-ttu-id="0a320-132">Hämta ett **klient-ID** och en **hemlig nyckel**.</span><span class="sxs-lookup"><span data-stu-id="0a320-132">Get **client ID** and **secret key**.</span></span> 
  3. <span data-ttu-id="0a320-133">Hämta ett **klientorganisations-ID**.</span><span class="sxs-lookup"><span data-stu-id="0a320-133">Get **tenant ID**.</span></span> 
  4. <span data-ttu-id="0a320-134">Tilldela **ADFCopyTutorialApp**-programmet rollen som **Data Factory-deltagare**.</span><span class="sxs-lookup"><span data-stu-id="0a320-134">Assign the **ADFCopyTutorialApp** application to the **Data Factory Contributor** role.</span></span>  
* <span data-ttu-id="0a320-135">[Installera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="0a320-135">Install [Azure PowerShell](/powershell/azure/overview).</span></span>  
* <span data-ttu-id="0a320-136">Starta **PowerShell** och kör följande kommando.</span><span class="sxs-lookup"><span data-stu-id="0a320-136">Launch **PowerShell** and do the following steps.</span></span> <span data-ttu-id="0a320-137">Låt Azure PowerShell vara öppet tills du är klar med självstudien.</span><span class="sxs-lookup"><span data-stu-id="0a320-137">Keep Azure PowerShell open until the end of this tutorial.</span></span> <span data-ttu-id="0a320-138">Om du stänger och öppnar det igen måste du köra kommandona en gång till.</span><span class="sxs-lookup"><span data-stu-id="0a320-138">If you close and reopen, you need to run the commands again.</span></span>
  
  1. <span data-ttu-id="0a320-139">Kör följande kommando och ange det användarnamn och lösenord som du använder för att logga in på Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="0a320-139">Run the following command and enter the user name and password that you use to sign in to the Azure portal:</span></span>
    
    ```PowerShell 
    Login-AzureRmAccount
    ```   
  2. <span data-ttu-id="0a320-140">Kör följande kommando för att visa alla prenumerationer för det här kontot:</span><span class="sxs-lookup"><span data-stu-id="0a320-140">Run the following command to view all the subscriptions for this account:</span></span>

    ```PowerShell     
    Get-AzureRmSubscription
    ``` 
  3. <span data-ttu-id="0a320-141">Kör följande kommando för att välja den prenumeration som du vill arbeta med.</span><span class="sxs-lookup"><span data-stu-id="0a320-141">Run the following command to select the subscription that you want to work with.</span></span> <span data-ttu-id="0a320-142">Ersätt **&lt;NameOfAzureSubscription**&gt; med namnet på din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="0a320-142">Replace **&lt;NameOfAzureSubscription**&gt; with the name of your Azure subscription.</span></span> 
     
    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext
    ```
  4. <span data-ttu-id="0a320-143">Skapa en Azure-resursgrupp med namnet **ADFTutorialResourceGroup** genom att köra följande kommando i PowerShell:</span><span class="sxs-lookup"><span data-stu-id="0a320-143">Create an Azure resource group named **ADFTutorialResourceGroup** by running the following command in the PowerShell:</span></span>  

    ```PowerShell     
      New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```
     
      <span data-ttu-id="0a320-144">Om resursgruppen redan finns anger du om du vill uppdatera den (Y) eller lämna den som den är (N).</span><span class="sxs-lookup"><span data-stu-id="0a320-144">If the resource group already exists, you specify whether to update it (Y) or keep it as (N).</span></span> 
     
      <span data-ttu-id="0a320-145">Vissa av stegen i den här självstudien förutsätter att du använder resursgruppen med namnet ADFTutorialResourceGroup.</span><span class="sxs-lookup"><span data-stu-id="0a320-145">Some of the steps in this tutorial assume that you use the resource group named ADFTutorialResourceGroup.</span></span> <span data-ttu-id="0a320-146">Om du använder en annan resursgrupp måste du använda namnet på din resursgrupp i stället för ADFTutorialResourceGroup i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="0a320-146">If you use a different resource group, you need to use the name of your resource group in place of ADFTutorialResourceGroup in this tutorial.</span></span>

## <a name="create-json-definitions"></a><span data-ttu-id="0a320-147">Skapa JSON-definitioner</span><span class="sxs-lookup"><span data-stu-id="0a320-147">Create JSON definitions</span></span>
<span data-ttu-id="0a320-148">Skapa följande JSON-filer i mappen som curl.exe finns i.</span><span class="sxs-lookup"><span data-stu-id="0a320-148">Create following JSON files in the folder where curl.exe is located.</span></span> 

### <a name="datafactoryjson"></a><span data-ttu-id="0a320-149">datafactory.json</span><span class="sxs-lookup"><span data-stu-id="0a320-149">datafactory.json</span></span>
> [!IMPORTANT]
> <span data-ttu-id="0a320-150">Namnet måste vara globalt unikt, så du kan behöva lägga till ett prefix eller suffix till ADFCopyTutorialDF för att göra det unikt.</span><span class="sxs-lookup"><span data-stu-id="0a320-150">Name must be globally unique, so you may want to prefix/suffix ADFCopyTutorialDF to make it a unique name.</span></span> 
> 
> 

```JSON
{  
    "name": "ADFCopyTutorialDF",  
    "location": "WestUS"
}  
```

### <a name="azurestoragelinkedservicejson"></a><span data-ttu-id="0a320-151">azurestoragelinkedservice.json</span><span class="sxs-lookup"><span data-stu-id="0a320-151">azurestoragelinkedservice.json</span></span>
> [!IMPORTANT]
> <span data-ttu-id="0a320-152">Ersätt **accountname** och **accountkey** med namnet och nyckeln för ditt Azure-lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="0a320-152">Replace **accountname** and **accountkey** with name and key of your Azure storage account.</span></span> <span data-ttu-id="0a320-153">Information om hur du hämtar din lagringsåtkomstnyckel finns i [Visa, kopiera och återskapa lagringsåtkomstnycklar](../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys).</span><span class="sxs-lookup"><span data-stu-id="0a320-153">To learn how to get your storage access key, see [View, copy and regenerate storage access keys](../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys).</span></span>

```JSON
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
```

<span data-ttu-id="0a320-154">Mer information om egenskaper som JSON finns i [Azure Storage länkade tjänster](data-factory-azure-blob-connector.md#azure-storage-linked-service).</span><span class="sxs-lookup"><span data-stu-id="0a320-154">For details about JSON properties, see [Azure Storage linked service](data-factory-azure-blob-connector.md#azure-storage-linked-service).</span></span>

### <a name="azuersqllinkedservicejson"></a><span data-ttu-id="0a320-155">azuersqllinkedservice.json</span><span class="sxs-lookup"><span data-stu-id="0a320-155">azuersqllinkedservice.json</span></span>
> [!IMPORTANT]
> <span data-ttu-id="0a320-156">Ersätt **servername**, **databasename**, **username** och **password** med namnet på din Azure SQL-server, namnet på SQL-databasen, användarkontot och lösenordet för kontot.</span><span class="sxs-lookup"><span data-stu-id="0a320-156">Replace **servername**, **databasename**, **username**, and **password** with name of your Azure SQL server, name of SQL database, user account, and password for the account.</span></span>  
> 
>

```JSON
{
    "name": "AzureSqlLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "description": "",
        "typeProperties": {
            "connectionString": "Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30"
        }
    }
}
```

<span data-ttu-id="0a320-157">Mer information om egenskaper som JSON finns i [Azure SQL länkade tjänster](data-factory-azure-sql-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="0a320-157">For details about JSON properties, see [Azure SQL linked service](data-factory-azure-sql-connector.md#linked-service-properties).</span></span>

### <a name="inputdatasetjson"></a><span data-ttu-id="0a320-158">inputdataset.json</span><span class="sxs-lookup"><span data-stu-id="0a320-158">inputdataset.json</span></span>

```JSON
{
  "name": "AzureBlobInput",
  "properties": {
    "structure": [
      {
        "name": "FirstName",
        "type": "String"
      },
      {
        "name": "LastName",
        "type": "String"
      }
    ],
    "type": "AzureBlob",
    "linkedServiceName": "AzureStorageLinkedService",
    "typeProperties": {
      "folderPath": "adftutorial/",
      "fileName": "emp.txt",
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ","
      }
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

<span data-ttu-id="0a320-159">Följande tabell innehåller beskrivningar av de JSON-egenskaper som användes i kodfragmentet:</span><span class="sxs-lookup"><span data-stu-id="0a320-159">The following table provides descriptions for the JSON properties used in the snippet:</span></span>

| <span data-ttu-id="0a320-160">Egenskap</span><span class="sxs-lookup"><span data-stu-id="0a320-160">Property</span></span> | <span data-ttu-id="0a320-161">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0a320-161">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="0a320-162">typ</span><span class="sxs-lookup"><span data-stu-id="0a320-162">type</span></span> | <span data-ttu-id="0a320-163">Typegenskapen har angetts till **AzureBlob** eftersom det finns data i Azure Blob-lagringen.</span><span class="sxs-lookup"><span data-stu-id="0a320-163">The type property is set to **AzureBlob** because data resides in an Azure blob storage.</span></span> |
| <span data-ttu-id="0a320-164">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="0a320-164">linkedServiceName</span></span> | <span data-ttu-id="0a320-165">Refererar till **AzureStorageLinkedService** som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="0a320-165">Refers to the **AzureStorageLinkedService** that you created earlier.</span></span> |
| <span data-ttu-id="0a320-166">folderPath</span><span class="sxs-lookup"><span data-stu-id="0a320-166">folderPath</span></span> | <span data-ttu-id="0a320-167">Anger vilken **blobbehållare** och **mapp** som innehåller indatablobbar.</span><span class="sxs-lookup"><span data-stu-id="0a320-167">Specifies the blob **container** and the **folder** that contains input blobs.</span></span> <span data-ttu-id="0a320-168">I den här självstudiekursen adftutorial är blob-behållaren och -mappen rotmappen.</span><span class="sxs-lookup"><span data-stu-id="0a320-168">In this tutorial, adftutorial is the blob container and folder is the root folder.</span></span> | 
| <span data-ttu-id="0a320-169">fileName</span><span class="sxs-lookup"><span data-stu-id="0a320-169">fileName</span></span> | <span data-ttu-id="0a320-170">Den här egenskapen är valfri.</span><span class="sxs-lookup"><span data-stu-id="0a320-170">This property is optional.</span></span> <span data-ttu-id="0a320-171">Om du tar bort egenskapen kommer alla filer från folderPath hämtas.</span><span class="sxs-lookup"><span data-stu-id="0a320-171">If you omit this property, all files from the folderPath are picked.</span></span> <span data-ttu-id="0a320-172">I den här självstudiekursen har angetts **emp.txt** som filnamn så att endast den filen hämtas för bearbetning.</span><span class="sxs-lookup"><span data-stu-id="0a320-172">In this tutorial, **emp.txt** is specified for the fileName, so only that file is picked up for processing.</span></span> |
| <span data-ttu-id="0a320-173">format -> typ</span><span class="sxs-lookup"><span data-stu-id="0a320-173">format -> type</span></span> |<span data-ttu-id="0a320-174">Indatafilen är i textformat, så vi använder **TextFormat**.</span><span class="sxs-lookup"><span data-stu-id="0a320-174">The input file is in the text format, so we use **TextFormat**.</span></span> |
| <span data-ttu-id="0a320-175">columnDelimiter</span><span class="sxs-lookup"><span data-stu-id="0a320-175">columnDelimiter</span></span> | <span data-ttu-id="0a320-176">Kolumner i loggfilerna avgränsas med **kommatecken (`,`)**.</span><span class="sxs-lookup"><span data-stu-id="0a320-176">The columns in the input file are delimited by **comma character (`,`)**.</span></span> |
| <span data-ttu-id="0a320-177">frekvens/intervall</span><span class="sxs-lookup"><span data-stu-id="0a320-177">frequency/interval</span></span> | <span data-ttu-id="0a320-178">Frekvensen är **timme** och intervallet är **1**, vilket innebär att indatasektorerna är tillgängliga en gång i **timmen**.</span><span class="sxs-lookup"><span data-stu-id="0a320-178">The frequency is set to **Hour** and interval is  set to **1**, which means that the input slices are available **hourly**.</span></span> <span data-ttu-id="0a320-179">Det betyder att tjänsten Data Factory söker efter indata varje timme i rotmappen för den angivna blobbehållaren (**adftutorial**).</span><span class="sxs-lookup"><span data-stu-id="0a320-179">In other words, the Data Factory service looks for input data every hour in the root folder of blob container (**adftutorial**) you specified.</span></span> <span data-ttu-id="0a320-180">Den söker data i pipelinens start- och sluttider och inte före eller efter dessa tider.</span><span class="sxs-lookup"><span data-stu-id="0a320-180">It looks for the data within the pipeline start and end times, not before or after these times.</span></span>  |
| <span data-ttu-id="0a320-181">extern</span><span class="sxs-lookup"><span data-stu-id="0a320-181">external</span></span> | <span data-ttu-id="0a320-182">Den här egenskapen anges som **true** om indatan inte skapades av denna pipeline.</span><span class="sxs-lookup"><span data-stu-id="0a320-182">This property is set to **true** if the data is not generated by this pipeline.</span></span> <span data-ttu-id="0a320-183">Inkommande data i den här självstudien finns i filen emp.txt som genereras av denna pipeline, så vi ställer in den här egenskapen på true.</span><span class="sxs-lookup"><span data-stu-id="0a320-183">The input data in this tutorial is in the emp.txt file, which is not generated by this pipeline, so we set this property to true.</span></span> |

<span data-ttu-id="0a320-184">Mer information om de här JSON-egenskaperna finns i artikeln [Azure Blob-anslutningsapp](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="0a320-184">For more information about these JSON properties, see [Azure Blob connector article](data-factory-azure-blob-connector.md#dataset-properties).</span></span>

### <a name="outputdatasetjson"></a><span data-ttu-id="0a320-185">outputdataset.json</span><span class="sxs-lookup"><span data-stu-id="0a320-185">outputdataset.json</span></span>

```JSON
{
  "name": "AzureSqlOutput",
  "properties": {
    "structure": [
      {
        "name": "FirstName",
        "type": "String"
      },
      {
        "name": "LastName",
        "type": "String"
      }
    ],
    "type": "AzureSqlTable",
    "linkedServiceName": "AzureSqlLinkedService",
    "typeProperties": {
      "tableName": "emp"
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```
<span data-ttu-id="0a320-186">Följande tabell innehåller beskrivningar av de JSON-egenskaper som användes i kodfragmentet:</span><span class="sxs-lookup"><span data-stu-id="0a320-186">The following table provides descriptions for the JSON properties used in the snippet:</span></span>

| <span data-ttu-id="0a320-187">Egenskap</span><span class="sxs-lookup"><span data-stu-id="0a320-187">Property</span></span> | <span data-ttu-id="0a320-188">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0a320-188">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="0a320-189">typ</span><span class="sxs-lookup"><span data-stu-id="0a320-189">type</span></span> | <span data-ttu-id="0a320-190">Typegenskapen är **AzureSqlTable** eftersom data kopieras till en tabell i en Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="0a320-190">The type property is set to **AzureSqlTable** because data is copied to a table in an Azure SQL database.</span></span> |
| <span data-ttu-id="0a320-191">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="0a320-191">linkedServiceName</span></span> | <span data-ttu-id="0a320-192">Refererar till **AzureSqlLinkedService** som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="0a320-192">Refers to the **AzureSqlLinkedService** that you created earlier.</span></span> |
| <span data-ttu-id="0a320-193">tableName</span><span class="sxs-lookup"><span data-stu-id="0a320-193">tableName</span></span> | <span data-ttu-id="0a320-194">Ange **tabellen** dit data kopieras.</span><span class="sxs-lookup"><span data-stu-id="0a320-194">Specified the **table** to which the data is copied.</span></span> | 
| <span data-ttu-id="0a320-195">frekvens/intervall</span><span class="sxs-lookup"><span data-stu-id="0a320-195">frequency/interval</span></span> | <span data-ttu-id="0a320-196">Frekvensen är inställd på **timme** och intervallet är **1**, vilket innebär att utdatasegment produceras **varje timme** mellan pipelinens start- och sluttider, inte före eller efter dessa tider.</span><span class="sxs-lookup"><span data-stu-id="0a320-196">The frequency is set to **Hour** and interval is **1**, which means that the output slices are produced **hourly** between the pipeline start and end times, not before or after these times.</span></span>  |

<span data-ttu-id="0a320-197">Det finns tre kolumner – **ID**, **FirstName** och **LastName** – i emp-tabellen i databasen.</span><span class="sxs-lookup"><span data-stu-id="0a320-197">There are three columns – **ID**, **FirstName**, and **LastName** – in the emp table in the database.</span></span> <span data-ttu-id="0a320-198">ID är en identitetskolumn, så du anger bara **FirstName** och **LastName** här.</span><span class="sxs-lookup"><span data-stu-id="0a320-198">ID is an identity column, so you need to specify only **FirstName** and **LastName** here.</span></span>

<span data-ttu-id="0a320-199">Mer information om de här JSON-egenskaperna finns i artikeln [Azure SQL-anslutningsapp](data-factory-azure-sql-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="0a320-199">For more information about these JSON properties, see [Azure SQL connector article](data-factory-azure-sql-connector.md#dataset-properties).</span></span>

### <a name="pipelinejson"></a><span data-ttu-id="0a320-200">pipeline.json</span><span class="sxs-lookup"><span data-stu-id="0a320-200">pipeline.json</span></span>

```JSON
{
  "name": "ADFTutorialPipeline",
  "properties": {
    "description": "Copy data from a blob to Azure SQL table",
    "activities": [
      {
        "name": "CopyFromBlobToSQL",
        "description": "Push Regional Effectiveness Campaign data to Azure SQL database",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureSqlOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "SqlSink",
            "writeBatchSize": 10000,
            "writeBatchTimeout": "60:00:00"
          }
        },
        "Policy": {
          "concurrency": 1,
          "executionPriorityOrder": "NewestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
    ],
    "start": "2017-05-11T00:00:00Z",
    "end": "2017-05-12T00:00:00Z"
  }
}
```

<span data-ttu-id="0a320-201">Observera följande punkter:</span><span class="sxs-lookup"><span data-stu-id="0a320-201">Note the following points:</span></span>

- <span data-ttu-id="0a320-202">I avsnittet Aktiviteter finns det bara en aktivitet vars **typ** anges till **Kopia**.</span><span class="sxs-lookup"><span data-stu-id="0a320-202">In the activities section, there is only one activity whose **type** is set to **Copy**.</span></span> <span data-ttu-id="0a320-203">Se artikeln [Dataförflyttningsaktiviteter](data-factory-data-movement-activities.md) för information om kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="0a320-203">For more information about the copy activity, see [data movement activities](data-factory-data-movement-activities.md).</span></span> <span data-ttu-id="0a320-204">I Data Factory-lösningar, kan du också använda [datatransformeringsaktiviteter](data-factory-data-transformation-activities.md).</span><span class="sxs-lookup"><span data-stu-id="0a320-204">In Data Factory solutions, you can also use [data transformation activities](data-factory-data-transformation-activities.md).</span></span>
- <span data-ttu-id="0a320-205">Indata för aktiviteten är inställd på **AzureBlobInput** och utdata för aktiviteten är inställd på **AzureSqlOutput**.</span><span class="sxs-lookup"><span data-stu-id="0a320-205">Input for the activity is set to **AzureBlobInput** and output for the activity is set to **AzureSqlOutput**.</span></span> 
- <span data-ttu-id="0a320-206">I avsnittet för **typeProperties** har **BlobSource** angetts som källtyp och **SqlSink** har angetts som mottagartyp.</span><span class="sxs-lookup"><span data-stu-id="0a320-206">In the **typeProperties** section, **BlobSource** is specified as the source type and **SqlSink** is specified as the sink type.</span></span> <span data-ttu-id="0a320-207">En fullständig lista över datakällor som stöds av kopieringsaktiviteten som källor och mottagare finns i [Datalager som stöds](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="0a320-207">For a complete list of data stores supported by the copy activity as sources and sinks, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="0a320-208">Klicka på länken i tabellen om du vill veta hur du använder ett visst datalager som stöds som källa/mottagare.</span><span class="sxs-lookup"><span data-stu-id="0a320-208">To learn how to use a specific supported data store as a source/sink, click the link in the table.</span></span>  
 
<span data-ttu-id="0a320-209">Ersätt värdet i **start**egenskapen med den aktuella dagen och **slut**värdet med nästa dag.</span><span class="sxs-lookup"><span data-stu-id="0a320-209">Replace the value of the **start** property with the current day and **end** value with the next day.</span></span> <span data-ttu-id="0a320-210">Du kan ange endast datumdelen och hoppa över tidsvärdet.</span><span class="sxs-lookup"><span data-stu-id="0a320-210">You can specify only the date part and skip the time part of the date time.</span></span> <span data-ttu-id="0a320-211">Till exempel ”2017-02-03” som motsvarar ”2017-02-03T00:00:00Z”</span><span class="sxs-lookup"><span data-stu-id="0a320-211">For example, "2017-02-03", which is equivalent to "2017-02-03T00:00:00Z"</span></span>
 
<span data-ttu-id="0a320-212">Både start- och slutdatum måste vara i [ISO-format](http://en.wikipedia.org/wiki/ISO_8601).</span><span class="sxs-lookup"><span data-stu-id="0a320-212">Both start and end datetimes must be in [ISO format](http://en.wikipedia.org/wiki/ISO_8601).</span></span> <span data-ttu-id="0a320-213">Exempel: 2016-10-14T16:32:41Z.</span><span class="sxs-lookup"><span data-stu-id="0a320-213">For example: 2016-10-14T16:32:41Z.</span></span> <span data-ttu-id="0a320-214">**Sluttiden** är valfri, men vi använder den i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="0a320-214">The **end** time is optional, but we use it in this tutorial.</span></span> 
 
<span data-ttu-id="0a320-215">Om du inte anger värdet för **slut**egenskapen, beräknas det som ”**start + 48 timmar**”.</span><span class="sxs-lookup"><span data-stu-id="0a320-215">If you do not specify value for the **end** property, it is calculated as "**start + 48 hours**".</span></span> <span data-ttu-id="0a320-216">Om du vill köra pipelinen på obestämd tid, anger du **9999-09-09** som värde för **slut**egenskapen.</span><span class="sxs-lookup"><span data-stu-id="0a320-216">To run the pipeline indefinitely, specify **9999-09-09** as the value for the **end** property.</span></span>
 
<span data-ttu-id="0a320-217">I det föregående exemplet finns det 24 datasektorer eftersom varje datasektor skapas varje timme.</span><span class="sxs-lookup"><span data-stu-id="0a320-217">In the preceding example, there are 24 data slices as each data slice is produced hourly.</span></span>

<span data-ttu-id="0a320-218">Beskrivningar av JSON-egenskaper i en pipeline-definition finns i artikeln [skapa pipelines](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="0a320-218">For descriptions of JSON properties in a pipeline definition, see [create pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="0a320-219">Beskrivningar av JSON-egenskaper i en kopieringsaktivitet-definition finns i artikeln [aktiviteter för dataflyttning](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="0a320-219">For descriptions of JSON properties in a copy activity definition, see [data movement activities](data-factory-data-movement-activities.md).</span></span> <span data-ttu-id="0a320-220">Beskrivningar av JSON-egenskaper som stöds av BlobSource finns i artikeln [Azure Blob-anslutningsapp](data-factory-azure-blob-connector.md).</span><span class="sxs-lookup"><span data-stu-id="0a320-220">For descriptions of JSON properties supported by BlobSource, see [Azure Blob connector article](data-factory-azure-blob-connector.md).</span></span> <span data-ttu-id="0a320-221">Beskrivningar av JSON-egenskaper som stöds av SqlSink finns i artikeln [Azure SQL Database-anslutningsapp](data-factory-azure-sql-connector.md).</span><span class="sxs-lookup"><span data-stu-id="0a320-221">For descriptions of JSON properties supported by SqlSink, see [Azure SQL Database connector article](data-factory-azure-sql-connector.md).</span></span>

## <a name="set-global-variables"></a><span data-ttu-id="0a320-222">Ange globala variabler</span><span class="sxs-lookup"><span data-stu-id="0a320-222">Set global variables</span></span>
<span data-ttu-id="0a320-223">När du har ersatt värdena med dina egna kör du följande kommandon i Azure PowerShell:</span><span class="sxs-lookup"><span data-stu-id="0a320-223">In Azure PowerShell, execute the following commands after replacing the values with your own:</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0a320-224">Anvisningar för hur du hämtar klient-ID, klienthemlighet, klientorganisations-ID och prenumerations-ID finns i [kravavsnittet](#prerequisites).</span><span class="sxs-lookup"><span data-stu-id="0a320-224">See [Prerequisites](#prerequisites) section for instructions on getting client ID, client secret, tenant ID, and subscription ID.</span></span>   
> 
> 

```JSON
$client_id = "<client ID of application in AAD>"
$client_secret = "<client key of application in AAD>"
$tenant = "<Azure tenant ID>";
$subscription_id="<Azure subscription ID>";

$rg = "ADFTutorialResourceGroup"
```

<span data-ttu-id="0a320-225">Kör följande kommando efter att du har uppdaterat namnet på datafabriken som du använder:</span><span class="sxs-lookup"><span data-stu-id="0a320-225">Run the following command after updating the name of the data factory you are using:</span></span> 

```
$adf = "ADFCopyTutorialDF"
```

## <a name="authenticate-with-aad"></a><span data-ttu-id="0a320-226">Autentisera med AAD</span><span class="sxs-lookup"><span data-stu-id="0a320-226">Authenticate with AAD</span></span>
<span data-ttu-id="0a320-227">Kör följande kommando för att autentisera med Azure Active Directory (AAD):</span><span class="sxs-lookup"><span data-stu-id="0a320-227">Run the following command to authenticate with Azure Active Directory (AAD):</span></span> 

```PowerShell
$cmd = { .\curl.exe -X POST https://login.microsoftonline.com/$tenant/oauth2/token  -F grant_type=client_credentials  -F resource=https://management.core.windows.net/ -F client_id=$client_id -F client_secret=$client_secret };
$responseToken = Invoke-Command -scriptblock $cmd;
$accessToken = (ConvertFrom-Json $responseToken).access_token;

(ConvertFrom-Json $responseToken) 
```

## <a name="create-data-factory"></a><span data-ttu-id="0a320-228">Skapa en datafabrik</span><span class="sxs-lookup"><span data-stu-id="0a320-228">Create data factory</span></span>
<span data-ttu-id="0a320-229">I det här steget ska du skapa en Azure Data Factory med namnet **ADFCopyTutorialDF**.</span><span class="sxs-lookup"><span data-stu-id="0a320-229">In this step, you create an Azure Data Factory named **ADFCopyTutorialDF**.</span></span> <span data-ttu-id="0a320-230">En datafabrik kan ha en eller flera pipelines.</span><span class="sxs-lookup"><span data-stu-id="0a320-230">A data factory can have one or more pipelines.</span></span> <span data-ttu-id="0a320-231">En pipeline kan innehålla en eller flera aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="0a320-231">A pipeline can have one or more activities in it.</span></span> <span data-ttu-id="0a320-232">Till exempel en kopieringsaktivitet som kopierar data från ett källdatalager till ett måldatalager.</span><span class="sxs-lookup"><span data-stu-id="0a320-232">For example, a Copy Activity to copy data from a source to a destination data store.</span></span> <span data-ttu-id="0a320-233">En Hive-aktivitet för HDInsight för att köra Hive-skript som transformerar indata till produktutdata.</span><span class="sxs-lookup"><span data-stu-id="0a320-233">A HDInsight Hive activity to run a Hive script to transform input data to product output data.</span></span> <span data-ttu-id="0a320-234">Skapa datafabriken genom att köra följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="0a320-234">Run the following commands to create the data factory:</span></span> 

1. <span data-ttu-id="0a320-235">Tilldela kommandot till variabeln med namnet **cmd**.</span><span class="sxs-lookup"><span data-stu-id="0a320-235">Assign the command to variable named **cmd**.</span></span> 
   
    > [!IMPORTANT]
    > <span data-ttu-id="0a320-236">Bekräfta att namnet på datafabriken som du anger här (ADFCopyTutorialDF) matchar namnet i **datafactory.json**.</span><span class="sxs-lookup"><span data-stu-id="0a320-236">Confirm that the name of the data factory you specify here (ADFCopyTutorialDF) matches the name specified in the **datafactory.json**.</span></span> 
   
    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@datafactory.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/ADFCopyTutorialDF0411?api-version=2015-10-01};
    ```
2. <span data-ttu-id="0a320-237">Kör kommandot med **Invoke-Command**.</span><span class="sxs-lookup"><span data-stu-id="0a320-237">Run the command by using **Invoke-Command**.</span></span>
   
    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. <span data-ttu-id="0a320-238">Granska resultaten.</span><span class="sxs-lookup"><span data-stu-id="0a320-238">View the results.</span></span> <span data-ttu-id="0a320-239">Om datafabriken har skapats korrekt visas JSON för datafabriken i **resultatet**. Annars visas ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="0a320-239">If the data factory has been successfully created, you see the JSON for the data factory in the **results**; otherwise, you see an error message.</span></span>  
   
    ```
    Write-Host $results
    ```

<span data-ttu-id="0a320-240">Observera följande punkter:</span><span class="sxs-lookup"><span data-stu-id="0a320-240">Note the following points:</span></span>

* <span data-ttu-id="0a320-241">Namnet på Azure Data Factory måste vara globalt unikt.</span><span class="sxs-lookup"><span data-stu-id="0a320-241">The name of the Azure Data Factory must be globally unique.</span></span> <span data-ttu-id="0a320-242">Om felet **Datafabriksnamnet ”ADFCopyTutorialDF” är inte tillgängligt** returneras går du igenom följande steg:</span><span class="sxs-lookup"><span data-stu-id="0a320-242">If you see the error in results: **Data factory name “ADFCopyTutorialDF” is not available**, do the following steps:</span></span>  
  
  1. <span data-ttu-id="0a320-243">Ändra namnet (till exempel dittnamnADFCopyTutorialDF) i filen **datafactory.json**.</span><span class="sxs-lookup"><span data-stu-id="0a320-243">Change the name (for example, yournameADFCopyTutorialDF) in the **datafactory.json** file.</span></span>
  2. <span data-ttu-id="0a320-244">I det första kommandot där variabeln **$cmd** tilldelas ett värde ersätter du ADFCopyTutorialDF med det nya namnet och kör kommandot.</span><span class="sxs-lookup"><span data-stu-id="0a320-244">In the first command where the **$cmd** variable is assigned a value, replace ADFCopyTutorialDF with the new name and run the command.</span></span> 
  3. <span data-ttu-id="0a320-245">Anropa REST-API:et genom att köra de följande två kommandona för att skapa datafabriken och skriva ut resultatet av åtgärden.</span><span class="sxs-lookup"><span data-stu-id="0a320-245">Run the next two commands to invoke the REST API to create the data factory and print the results of the operation.</span></span> 
     
     <span data-ttu-id="0a320-246">Se artikeln [Data Factory – namnregler](data-factory-naming-rules.md) för namnregler för Data Factory-artefakter.</span><span class="sxs-lookup"><span data-stu-id="0a320-246">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>
* <span data-ttu-id="0a320-247">Om du vill skapa Data Factory-instanser måste du vara deltagare/administratör för Azure-prenumerationen</span><span class="sxs-lookup"><span data-stu-id="0a320-247">To create Data Factory instances, you need to be a contributor/administrator of the Azure subscription</span></span>
* <span data-ttu-id="0a320-248">Namnet på datafabriken kan registreras som ett DNS-namn i framtiden och blir då synligt offentligt.</span><span class="sxs-lookup"><span data-stu-id="0a320-248">The name of the data factory may be registered as a DNS name in the future and hence become publicly visible.</span></span>
* <span data-ttu-id="0a320-249">Om du får felet: ”**Den här prenumerationen har inte registrerats för användning av namnområdet Microsoft.DataFactory**” gör du något av följande och försöker att publicera igen:</span><span class="sxs-lookup"><span data-stu-id="0a320-249">If you receive the error: "**This subscription is not registered to use namespace Microsoft.DataFactory**", do one of the following and try publishing again:</span></span> 
  
  * <span data-ttu-id="0a320-250">I Azure PowerShell kör du följande kommando för att registrera Data Factory-providern:</span><span class="sxs-lookup"><span data-stu-id="0a320-250">In Azure PowerShell, run the following command to register the Data Factory provider:</span></span> 

    ```PowerShell    
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```
    <span data-ttu-id="0a320-251">Du kan köra följande kommando om du vill kontrollera att Data Factory-providern är registrerad.</span><span class="sxs-lookup"><span data-stu-id="0a320-251">You can run the following command to confirm that the Data Factory provider is registered.</span></span> 
    
    ```PowerShell
    Get-AzureRmResourceProvider
    ```
  * <span data-ttu-id="0a320-252">Logga in med Azure-prenumerationen i [Azure Portal](https://portal.azure.com) och navigera till ett Data Factory-blad (eller) skapa en datafabrik i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="0a320-252">Login using the Azure subscription into the [Azure portal](https://portal.azure.com) and navigate to a Data Factory blade (or) create a data factory in the Azure portal.</span></span> <span data-ttu-id="0a320-253">Med den här åtgärden registreras providern automatiskt.</span><span class="sxs-lookup"><span data-stu-id="0a320-253">This action automatically registers the provider for you.</span></span>

<span data-ttu-id="0a320-254">Du måste först skapa några Data Factory-entiteter innan du skapar en pipeline.</span><span class="sxs-lookup"><span data-stu-id="0a320-254">Before creating a pipeline, you need to create a few Data Factory entities first.</span></span> <span data-ttu-id="0a320-255">Först skapar du länkade tjänster för att länka käll- och måldatalager till ditt datalager.</span><span class="sxs-lookup"><span data-stu-id="0a320-255">You first create linked services to link source and destination data stores to your data store.</span></span> <span data-ttu-id="0a320-256">Sedan definierar du in- och utdatauppsättningar för att representera data i länkade datalager.</span><span class="sxs-lookup"><span data-stu-id="0a320-256">Then, define input and output datasets to represent data in linked data stores.</span></span> <span data-ttu-id="0a320-257">Slutligen skapar du pipelinen med en aktivitet som använder dessa datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="0a320-257">Finally, create the pipeline with an activity that uses these datasets.</span></span>

## <a name="create-linked-services"></a><span data-ttu-id="0a320-258">Skapa länkade tjänster</span><span class="sxs-lookup"><span data-stu-id="0a320-258">Create linked services</span></span>
<span data-ttu-id="0a320-259">Du kan skapa länkade tjänster i en datafabrik för att länka ditt datalager och beräkna datafabrik-tjänster.</span><span class="sxs-lookup"><span data-stu-id="0a320-259">You create linked services in a data factory to link your data stores and compute services to the data factory.</span></span> <span data-ttu-id="0a320-260">I den här självstudiekursen kommer använder du inte någon beräkningstjänst, till exempel Azure HDInsight eller Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="0a320-260">In this tutorial, you don't use any compute service such as Azure HDInsight or Azure Data Lake Analytics.</span></span> <span data-ttu-id="0a320-261">Du använder två datalager av typen Azure Storage (källa) och Azure SQL Database (mål).</span><span class="sxs-lookup"><span data-stu-id="0a320-261">You use two data stores of type Azure Storage (source) and Azure SQL Database (destination).</span></span> <span data-ttu-id="0a320-262">Därför kan du skapa två länkade tjänster som heter AzureStorageLinkedService och AzureSqlLinkedService av typerna: AzureStorage och AzureSqlDatabase.</span><span class="sxs-lookup"><span data-stu-id="0a320-262">Therefore, you create two linked services named AzureStorageLinkedService and AzureSqlLinkedService of types: AzureStorage and AzureSqlDatabase.</span></span>  

<span data-ttu-id="0a320-263">AzureStorageLinkedService länkar ditt Azure Storage-konto till datafabriken.</span><span class="sxs-lookup"><span data-stu-id="0a320-263">The AzureStorageLinkedService links your Azure storage account to the data factory.</span></span> <span data-ttu-id="0a320-264">Använd det lagringskonto i vilket du skapade en behållare och laddade upp data under [förberedelsestegen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="0a320-264">This storage account is the one in which you created a container and uploaded the data as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>   

<span data-ttu-id="0a320-265">AzureSqlLinkedService länkar din Azure SQL-databas till datafabriken.</span><span class="sxs-lookup"><span data-stu-id="0a320-265">AzureSqlLinkedService links your Azure SQL database to the data factory.</span></span> <span data-ttu-id="0a320-266">Data som kopieras från blob-lagringen sparas i den här databasen.</span><span class="sxs-lookup"><span data-stu-id="0a320-266">The data that is copied from the blob storage is stored in this database.</span></span> <span data-ttu-id="0a320-267">Du har skapat den tomma tabellen i den här databasen som en del av [förhandskraven](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="0a320-267">You created the emp table in this database as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>  

### <a name="create-azure-storage-linked-service"></a><span data-ttu-id="0a320-268">Skapa en länkad Azure-lagringstjänst</span><span class="sxs-lookup"><span data-stu-id="0a320-268">Create Azure Storage linked service</span></span>
<span data-ttu-id="0a320-269">I det här steget länkar du ditt Azure-lagringskonto till datafabriken.</span><span class="sxs-lookup"><span data-stu-id="0a320-269">In this step, you link your Azure storage account to your data factory.</span></span> <span data-ttu-id="0a320-270">Du anger namnet och nyckeln för Azure Storage-kontot i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="0a320-270">You specify the name and key of your Azure storage account in this section.</span></span> <span data-ttu-id="0a320-271">Se [Länkad Azure Storage-tjänst](data-factory-azure-blob-connector.md#azure-storage-linked-service) om du vill ha information om JSON-egenskaper som används för att definiera en länkad Azure Storage-tjänst.</span><span class="sxs-lookup"><span data-stu-id="0a320-271">See [Azure Storage linked service](data-factory-azure-blob-connector.md#azure-storage-linked-service) for details about JSON properties used to define an Azure Storage linked service.</span></span>  

1. <span data-ttu-id="0a320-272">Tilldela kommandot till variabeln med namnet **cmd**.</span><span class="sxs-lookup"><span data-stu-id="0a320-272">Assign the command to variable named **cmd**.</span></span> 

    ```PowerShell   
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@azurestoragelinkedservice.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/AzureStorageLinkedService?api-version=2015-10-01};
    ```
2. <span data-ttu-id="0a320-273">Kör kommandot med **Invoke-Command**.</span><span class="sxs-lookup"><span data-stu-id="0a320-273">Run the command by using **Invoke-Command**.</span></span>

    ```PowerShell   
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. <span data-ttu-id="0a320-274">Granska resultaten.</span><span class="sxs-lookup"><span data-stu-id="0a320-274">View the results.</span></span> <span data-ttu-id="0a320-275">Om den länkade tjänsten har skapats korrekt visas JSON för den länkade tjänsten i **resultatet**. Annars visas ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="0a320-275">If the linked service has been successfully created, you see the JSON for the linked service in the **results**; otherwise, you see an error message.</span></span>

    ```PowerShell   
    Write-Host $results
    ```

### <a name="create-azure-sql-linked-service"></a><span data-ttu-id="0a320-276">Skapa en länkad Azure SQL-tjänst</span><span class="sxs-lookup"><span data-stu-id="0a320-276">Create Azure SQL linked service</span></span>
<span data-ttu-id="0a320-277">I det här steget länkar du Azure SQL-databasen till din datafabrik.</span><span class="sxs-lookup"><span data-stu-id="0a320-277">In this step, you link your Azure SQL database to your data factory.</span></span> <span data-ttu-id="0a320-278">Du anger Azure SQL-servernamnet, databasnamnet, användarnamnet och lösenordet i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="0a320-278">You specify the Azure SQL server name, database name, user name, and user password in this section.</span></span> <span data-ttu-id="0a320-279">Se [Länkad Azure SQL-tjänst](data-factory-azure-sql-connector.md#linked-service-properties) om du vill ha information om JSON-egenskaper som används för att definiera en länkad Azure SQL-tjänst.</span><span class="sxs-lookup"><span data-stu-id="0a320-279">See [Azure SQL linked service](data-factory-azure-sql-connector.md#linked-service-properties) for details about JSON properties used to define an Azure SQL linked service.</span></span>

1. <span data-ttu-id="0a320-280">Tilldela kommandot till variabeln med namnet **cmd**.</span><span class="sxs-lookup"><span data-stu-id="0a320-280">Assign the command to variable named **cmd**.</span></span> 
   
    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@azuresqllinkedservice.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/AzureSqlLinkedService?api-version=2015-10-01};
    ```
2. <span data-ttu-id="0a320-281">Kör kommandot med **Invoke-Command**.</span><span class="sxs-lookup"><span data-stu-id="0a320-281">Run the command by using **Invoke-Command**.</span></span>
   
    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. <span data-ttu-id="0a320-282">Granska resultaten.</span><span class="sxs-lookup"><span data-stu-id="0a320-282">View the results.</span></span> <span data-ttu-id="0a320-283">Om den länkade tjänsten har skapats korrekt visas JSON för den länkade tjänsten i **resultatet**. Annars visas ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="0a320-283">If the linked service has been successfully created, you see the JSON for the linked service in the **results**; otherwise, you see an error message.</span></span>
   
    ```PowerShell
    Write-Host $results
    ```

## <a name="create-datasets"></a><span data-ttu-id="0a320-284">Skapa datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="0a320-284">Create datasets</span></span>
<span data-ttu-id="0a320-285">I det föregående steget skapade du kopplade tjänster för att länka ett Azure-lagringskonto och en Azure SQL-databas till datafabriken.</span><span class="sxs-lookup"><span data-stu-id="0a320-285">In the previous step, you created linked services to link your Azure Storage account and Azure SQL database to your data factory.</span></span> <span data-ttu-id="0a320-286">I det här steget definierar du två datauppsättningar, AzureBlobInput och AzureSqlOutput, som visar in- och utdata som lagras i de datalager som refereras till av AzureStorageLinkedService och AzureSqlLinkedService.</span><span class="sxs-lookup"><span data-stu-id="0a320-286">In this step, you define two datasets named AzureBlobInput and AzureSqlOutput that represent input and output data that is stored in the data stores referred by AzureStorageLinkedService and AzureSqlLinkedService respectively.</span></span>

<span data-ttu-id="0a320-287">Den länkade Azure storage-tjänsten anger anslutningssträngen som Data Factory-tjänsten använder vid körning för att ansluta till ditt Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="0a320-287">The Azure storage linked service specifies the connection string that Data Factory service uses at run time to connect to your Azure storage account.</span></span> <span data-ttu-id="0a320-288">Och en Blob-datauppsättning (AzureBlobInput) anger vilken blobbehållare och mapp som innehåller data.</span><span class="sxs-lookup"><span data-stu-id="0a320-288">And, the input blob dataset (AzureBlobInput) specifies the container and the folder that contains the input data.</span></span>  

<span data-ttu-id="0a320-289">Den länkade Azure SQL-databasen anger anslutningssträngen som Data Factory-tjänsten använder vid körning för att ansluta till ditt Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="0a320-289">Similarly, the Azure SQL Database linked service specifies the connection string that Data Factory service uses at run time to connect to your Azure SQL database.</span></span> <span data-ttu-id="0a320-290">Och utdatauppsättningen (OutputDataset) för SQL-tabellen anger tabellen i databasen som data kopieras till från blob-lagringen.</span><span class="sxs-lookup"><span data-stu-id="0a320-290">And, the output SQL table dataset (OututDataset) specifies the table in the database to which the data from the blob storage is copied.</span></span> 

### <a name="create-input-dataset"></a><span data-ttu-id="0a320-291">Skapa indatauppsättning</span><span class="sxs-lookup"><span data-stu-id="0a320-291">Create input dataset</span></span>
<span data-ttu-id="0a320-292">I det här steget skapar du en datauppsättning med namnet AzureBlobInput som pekar på en blobfil (emp.ext) i rotmappen i en blobbehållare (adftutorial) i Azure Storage som representeras av den länkade tjänsten AzureStorageLinkedService.</span><span class="sxs-lookup"><span data-stu-id="0a320-292">In this step, you create a dataset named AzureBlobInput that points to a blob file (emp.txt) in the root folder of a blob container (adftutorial) in the Azure Storage represented by the AzureStorageLinkedService linked service.</span></span> <span data-ttu-id="0a320-293">Om du inte anger ett värde för filnamnet (eller hoppar över det), kommer data från alla blobbar i indatamappen att kopieras till målet.</span><span class="sxs-lookup"><span data-stu-id="0a320-293">If you don't specify a value for the fileName (or skip it), data from all blobs in the input folder are copied to the destination.</span></span> <span data-ttu-id="0a320-294">I den här kursen anger du ett värde för filnamnet.</span><span class="sxs-lookup"><span data-stu-id="0a320-294">In this tutorial, you specify a value for the fileName.</span></span> 

1. <span data-ttu-id="0a320-295">Tilldela kommandot till variabeln med namnet **cmd**.</span><span class="sxs-lookup"><span data-stu-id="0a320-295">Assign the command to variable named **cmd**.</span></span> 

    ```PowerSHell   
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@inputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobInput?api-version=2015-10-01};
    ```
2. <span data-ttu-id="0a320-296">Kör kommandot med **Invoke-Command**.</span><span class="sxs-lookup"><span data-stu-id="0a320-296">Run the command by using **Invoke-Command**.</span></span>
   
    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. <span data-ttu-id="0a320-297">Granska resultaten.</span><span class="sxs-lookup"><span data-stu-id="0a320-297">View the results.</span></span> <span data-ttu-id="0a320-298">Om datauppsättningen har skapats korrekt visas JSON för datauppsättningen i **resultatet**. Annars visas ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="0a320-298">If the dataset has been successfully created, you see the JSON for the dataset in the **results**; otherwise, you see an error message.</span></span>
   
    ```PowerShell
    Write-Host $results
    ```

### <a name="create-output-dataset"></a><span data-ttu-id="0a320-299">Skapa datauppsättning för utdata</span><span class="sxs-lookup"><span data-stu-id="0a320-299">Create output dataset</span></span>
<span data-ttu-id="0a320-300">Den länkade tjänsten Azure SQL Database anger anslutningssträngen som används av tjänsten Data Factory vid körningstiden för att ansluta till din Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="0a320-300">The Azure SQL Database linked service specifies the connection string that Data Factory service uses at run time to connect to your Azure SQL database.</span></span> <span data-ttu-id="0a320-301">Och utdatauppsättningen (OutputDataset) för SQL-tabellen som du skapade i det här steget anger tabellen i databasen som data kopieras till från blob-lagringen.</span><span class="sxs-lookup"><span data-stu-id="0a320-301">The output SQL table dataset (OututDataset) you create in this step specifies the table in the database to which the data from the blob storage is copied.</span></span>

1. <span data-ttu-id="0a320-302">Tilldela kommandot till variabeln med namnet **cmd**.</span><span class="sxs-lookup"><span data-stu-id="0a320-302">Assign the command to variable named **cmd**.</span></span>

    ```PowerShell   
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@outputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureSqlOutput?api-version=2015-10-01};
    ```
2. <span data-ttu-id="0a320-303">Kör kommandot med **Invoke-Command**.</span><span class="sxs-lookup"><span data-stu-id="0a320-303">Run the command by using **Invoke-Command**.</span></span>
    
    ```PowerShell   
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. <span data-ttu-id="0a320-304">Granska resultaten.</span><span class="sxs-lookup"><span data-stu-id="0a320-304">View the results.</span></span> <span data-ttu-id="0a320-305">Om datauppsättningen har skapats korrekt visas JSON för datauppsättningen i **resultatet**. Annars visas ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="0a320-305">If the dataset has been successfully created, you see the JSON for the dataset in the **results**; otherwise, you see an error message.</span></span>
   
    ```PowerShell
    Write-Host $results
    ``` 

## <a name="create-pipeline"></a><span data-ttu-id="0a320-306">Skapa pipeline</span><span class="sxs-lookup"><span data-stu-id="0a320-306">Create pipeline</span></span>
<span data-ttu-id="0a320-307">I det här steget ska du skapa en pipeline med en **kopieringsaktivitet** som använder **AzureBlobInput** som indata och **AzureSqlOutput** som utdata.</span><span class="sxs-lookup"><span data-stu-id="0a320-307">In this step, you create a pipeline with a **copy activity** that uses **AzureBlobInput** as an input and **AzureSqlOutput** as an output.</span></span>

<span data-ttu-id="0a320-308">Schemat styrs för närvarande av utdatamängd.</span><span class="sxs-lookup"><span data-stu-id="0a320-308">Currently, output dataset is what drives the schedule.</span></span> <span data-ttu-id="0a320-309">I den här självstudiekursen är datamängden för utdata konfigurerad för att skapa ett segment en gång i timmen.</span><span class="sxs-lookup"><span data-stu-id="0a320-309">In this tutorial, output dataset is configured to produce a slice once an hour.</span></span> <span data-ttu-id="0a320-310">Pipelinen har en starttid och sluttid som är en dag från varandra, vilket är 24 timmar.</span><span class="sxs-lookup"><span data-stu-id="0a320-310">The pipeline has a start time and end time that are one day apart, which is 24 hours.</span></span> <span data-ttu-id="0a320-311">Därför produceras 24 segment för utdatauppsättningen av pipeline.</span><span class="sxs-lookup"><span data-stu-id="0a320-311">Therefore, 24 slices of output dataset are produced by the pipeline.</span></span> 

1. <span data-ttu-id="0a320-312">Tilldela kommandot till variabeln med namnet **cmd**.</span><span class="sxs-lookup"><span data-stu-id="0a320-312">Assign the command to variable named **cmd**.</span></span>

    ```PowerShell   
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@pipeline.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datapipelines/MyFirstPipeline?api-version=2015-10-01};
    ```
2. <span data-ttu-id="0a320-313">Kör kommandot med **Invoke-Command**.</span><span class="sxs-lookup"><span data-stu-id="0a320-313">Run the command by using **Invoke-Command**.</span></span>

    ```PowerShell   
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. <span data-ttu-id="0a320-314">Granska resultaten.</span><span class="sxs-lookup"><span data-stu-id="0a320-314">View the results.</span></span> <span data-ttu-id="0a320-315">Om datauppsättningen har skapats korrekt visas JSON för datauppsättningen i **resultatet**. Annars visas ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="0a320-315">If the dataset has been successfully created, you see the JSON for the dataset in the **results**; otherwise, you see an error message.</span></span>  

    ```PowerShell   
    Write-Host $results
    ```

<span data-ttu-id="0a320-316">**Grattis!**</span><span class="sxs-lookup"><span data-stu-id="0a320-316">**Congratulations!**</span></span> <span data-ttu-id="0a320-317">Du har skapat en Azure-datafabrik, med en pipeline som kopierar data från Azure Blob Storage till en Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="0a320-317">You have successfully created an Azure data factory, with a pipeline that copies data from Azure Blob Storage to Azure SQL database.</span></span>

## <a name="monitor-pipeline"></a><span data-ttu-id="0a320-318">Övervaka pipeline</span><span class="sxs-lookup"><span data-stu-id="0a320-318">Monitor pipeline</span></span>
<span data-ttu-id="0a320-319">I det här steget ska du använda REST-API:et för Data Factory för att övervaka sektorer som genereras av pipelinen.</span><span class="sxs-lookup"><span data-stu-id="0a320-319">In this step, you use Data Factory REST API to monitor slices being produced by the pipeline.</span></span>

```PowerShell
$ds ="AzureSqlOutput"
```

> [!IMPORTANT] 
> <span data-ttu-id="0a320-320">Kontrollera att start- och sluttider som anges i följande kommando matchar start- och sluttider för pipeline.</span><span class="sxs-lookup"><span data-stu-id="0a320-320">Make sure that the start and end times specified in the following command match the start and end times of the pipeline.</span></span> 

```PowerShell
$cmd = {.\curl.exe -X GET -H "Authorization: Bearer $accessToken" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/$ds/slices?start=2017-05-11T00%3a00%3a00.0000000Z"&"end=2017-05-12T00%3a00%3a00.0000000Z"&"api-version=2015-10-01};
```

```PowerShell
$results2 = Invoke-Command -scriptblock $cmd;
```

```PowerShell
IF ((ConvertFrom-Json $results2).value -ne $NULL) {
    ConvertFrom-Json $results2 | Select-Object -Expand value | Format-Table
} else {
        (convertFrom-Json $results2).RemoteException
}
```

<span data-ttu-id="0a320-321">Kör Invoke-Command och nästa kommando tills du ser sektorn med tillståndet **Ready** eller **Failed**.</span><span class="sxs-lookup"><span data-stu-id="0a320-321">Run the Invoke-Command and the next one until you see a slice in **Ready** state or **Failed** state.</span></span> <span data-ttu-id="0a320-322">När sektorn har statusen Ready tittar du efter utdata i tabellen **emp** i Azure SQL-databasen.</span><span class="sxs-lookup"><span data-stu-id="0a320-322">When the slice is in Ready state, check the **emp** table in your Azure SQL database for the output data.</span></span> 

<span data-ttu-id="0a320-323">För varje sektor kopieras två rader med data från källfilen till emp-tabellen i Azure SQL-databasen.</span><span class="sxs-lookup"><span data-stu-id="0a320-323">For each slice, two rows of data from the source file are copied to the emp table in the Azure SQL database.</span></span> <span data-ttu-id="0a320-324">Därför finns det 24 nya poster i emp-tabellen när alla sektorer har bearbetats (statusen Ready).</span><span class="sxs-lookup"><span data-stu-id="0a320-324">Therefore, you see 24 new records in the emp table when all the slices are successfully processed (in Ready state).</span></span> 

## <a name="summary"></a><span data-ttu-id="0a320-325">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="0a320-325">Summary</span></span>
<span data-ttu-id="0a320-326">I den här självstudiekursen använde du REST-API:et för att skapa en Azure-datafabrik och kopiera data från ett Azure-blobb till en Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="0a320-326">In this tutorial, you used REST API to create an Azure data factory to copy data from an Azure blob to an Azure SQL database.</span></span> <span data-ttu-id="0a320-327">Här är de avancerade steg som du utförde i självstudien:</span><span class="sxs-lookup"><span data-stu-id="0a320-327">Here are the high-level steps you performed in this tutorial:</span></span>  

1. <span data-ttu-id="0a320-328">Du skapade en Azure **Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="0a320-328">Created an Azure **data factory**.</span></span>
2. <span data-ttu-id="0a320-329">Du skapade **länkade tjänster**:</span><span class="sxs-lookup"><span data-stu-id="0a320-329">Created **linked services**:</span></span>
   1. <span data-ttu-id="0a320-330">En länkad Azure Storage-tjänst för att länka Azure Storage-kontot som innehåller indata.</span><span class="sxs-lookup"><span data-stu-id="0a320-330">An Azure Storage linked service to link your Azure Storage account that holds input data.</span></span>     
   2. <span data-ttu-id="0a320-331">En länkad Azure SQL-tjänst för att länka Azure SQL-databasen som innehåller utdata.</span><span class="sxs-lookup"><span data-stu-id="0a320-331">An Azure SQL linked service to link your Azure SQL database that holds the output data.</span></span> 
3. <span data-ttu-id="0a320-332">Du skapade **datauppsättningar** som beskriver indata och utdata för pipelines.</span><span class="sxs-lookup"><span data-stu-id="0a320-332">Created **datasets**, which describe input data and output data for pipelines.</span></span>
4. <span data-ttu-id="0a320-333">Du skapade en **pipeline** med en kopieringsaktivitet med BlobSource som källa och SqlSink som mottagare.</span><span class="sxs-lookup"><span data-stu-id="0a320-333">Created a **pipeline** with a Copy Activity with BlobSource as source and SqlSink as sink.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="0a320-334">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0a320-334">Next steps</span></span>
<span data-ttu-id="0a320-335">I den här kursen används Azure blob storage som ett datalager för källa och en Azure SQL-databas som ett dataarkiv som mål i en kopieringsåtgärd.</span><span class="sxs-lookup"><span data-stu-id="0a320-335">In this tutorial, you used Azure blob storage as a source data store and an Azure SQL database as a destination data store in a copy operation.</span></span> <span data-ttu-id="0a320-336">Följande tabell innehåller en lista över datalager som stöds som källor och mål av kopieringsaktiviteten:</span><span class="sxs-lookup"><span data-stu-id="0a320-336">The following table provides a list of data stores supported as sources and destinations by the copy activity:</span></span> 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

<span data-ttu-id="0a320-337">För mer information om hur du kopierar data till/från ett datalager klickar du på länken för datalagret i tabellen.</span><span class="sxs-lookup"><span data-stu-id="0a320-337">To learn about how to copy data to/from a data store, click the link for the data store in the table.</span></span>