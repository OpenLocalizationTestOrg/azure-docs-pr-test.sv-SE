---
title: "Självstudier: Använd REST API toocreate ett Azure Data Factory-pipelinen | Microsoft Docs"
description: "I den här kursen använder du REST API toocreate ett Azure Data Factory-pipelinen med en Kopieringsaktiviteten toocopy data från ett Azure blob storage en Azure SQL database."
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
ms.openlocfilehash: aa6c9b035101c4ff9acff90117ca6e3e7067f418
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-use-rest-api-toocreate-an-azure-data-factory-pipeline-toocopy-data"></a><span data-ttu-id="906e7-103">Självstudier: Använd REST API toocreate ett Azure Data Factory pipeline toocopy data</span><span class="sxs-lookup"><span data-stu-id="906e7-103">Tutorial: Use REST API toocreate an Azure Data Factory pipeline toocopy data</span></span> 
> [!div class="op_single_selector"]
> * [<span data-ttu-id="906e7-104">Översikt och förutsättningar</span><span class="sxs-lookup"><span data-stu-id="906e7-104">Overview and prerequisites</span></span>](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [<span data-ttu-id="906e7-105">Guiden Kopiera</span><span class="sxs-lookup"><span data-stu-id="906e7-105">Copy Wizard</span></span>](data-factory-copy-data-wizard-tutorial.md)
> * [<span data-ttu-id="906e7-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="906e7-106">Azure portal</span></span>](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [<span data-ttu-id="906e7-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="906e7-107">Visual Studio</span></span>](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [<span data-ttu-id="906e7-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="906e7-108">PowerShell</span></span>](data-factory-copy-activity-tutorial-using-powershell.md)
> * [<span data-ttu-id="906e7-109">Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="906e7-109">Azure Resource Manager template</span></span>](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [<span data-ttu-id="906e7-110">REST API</span><span class="sxs-lookup"><span data-stu-id="906e7-110">REST API</span></span>](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [<span data-ttu-id="906e7-111">.NET-API</span><span class="sxs-lookup"><span data-stu-id="906e7-111">.NET API</span></span>](data-factory-copy-activity-tutorial-using-dotnet-api.md)
> 
> 

<span data-ttu-id="906e7-112">I den här artikeln lär du dig hur toouse REST-API toocreate en datafabrik med en rörledning som kopierar data från Azure blob storage tooan Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="906e7-112">In this article, you learn how toouse REST API toocreate a data factory with a pipeline that copies data from an Azure blob storage tooan Azure SQL database.</span></span> <span data-ttu-id="906e7-113">Om du är ny tooAzure Data Factory, Läs igenom hello [introduktion tooAzure Data Factory](data-factory-introduction.md) artikel innan du utför den här kursen.</span><span class="sxs-lookup"><span data-stu-id="906e7-113">If you are new tooAzure Data Factory, read through hello [Introduction tooAzure Data Factory](data-factory-introduction.md) article before doing this tutorial.</span></span>   

<span data-ttu-id="906e7-114">I den här självstudien får du skapa en pipeline i en aktivitet: kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="906e7-114">In this tutorial, you create a pipeline with one activity in it: Copy Activity.</span></span> <span data-ttu-id="906e7-115">Hej kopieringsaktiviteten kopierar data från ett datalager för data som stöds store tooa stöds mottagare.</span><span class="sxs-lookup"><span data-stu-id="906e7-115">hello copy activity copies data from a supported data store tooa supported sink data store.</span></span> <span data-ttu-id="906e7-116">En lista över datakällor som stöds som källor och mottagare finns i [datalager som stöds](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="906e7-116">For a list of data stores supported as sources and sinks, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="906e7-117">hello aktiviteten drivs av en globalt tillgänglig tjänst som kan kopiera data mellan olika datalager på ett säkert, tillförlitligt och skalbar sätt.</span><span class="sxs-lookup"><span data-stu-id="906e7-117">hello activity is powered by a globally available service that can copy data between various data stores in a secure, reliable, and scalable way.</span></span> <span data-ttu-id="906e7-118">Läs mer om hello Kopieringsaktiviteten [Data Movement aktiviteter](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="906e7-118">For more information about hello Copy Activity, see [Data Movement Activities](data-factory-data-movement-activities.md).</span></span>

<span data-ttu-id="906e7-119">En pipeline kan ha fler än en aktivitet.</span><span class="sxs-lookup"><span data-stu-id="906e7-119">A pipeline can have more than one activity.</span></span> <span data-ttu-id="906e7-120">Och du kan länka två aktiviteter (köra en aktivitet efter ett annat) genom att ange hello datamängd för utdata för en aktivitet som hello indatauppsättning av hello andra aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="906e7-120">And, you can chain two activities (run one activity after another) by setting hello output dataset of one activity as hello input dataset of hello other activity.</span></span> <span data-ttu-id="906e7-121">Mer information finns i [flera aktiviteter i en pipeline](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span><span class="sxs-lookup"><span data-stu-id="906e7-121">For more information, see [multiple activities in a pipeline](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span></span>

> [!NOTE]
> <span data-ttu-id="906e7-122">Den här artikeln täcker inte alla hello Data Factory REST API.</span><span class="sxs-lookup"><span data-stu-id="906e7-122">This article does not cover all hello Data Factory REST API.</span></span> <span data-ttu-id="906e7-123">Omfattande dokumentation om Data Factory-cmdlets finns i [referensen för REST-API:et för Data Factory](/rest/api/datafactory/).</span><span class="sxs-lookup"><span data-stu-id="906e7-123">See [Data Factory REST API Reference](/rest/api/datafactory/) for comprehensive documentation on Data Factory cmdlets.</span></span>
>  
> <span data-ttu-id="906e7-124">hello data pipeline i den här kursen kopierar data från en källa data store tooa målarkiv data.</span><span class="sxs-lookup"><span data-stu-id="906e7-124">hello data pipeline in this tutorial copies data from a source data store tooa destination data store.</span></span> <span data-ttu-id="906e7-125">En självstudiekurs om hur tootransform data med hjälp av Azure Data Factory finns [Självstudier: skapa en pipeline tootransform data med Hadoop-kluster](data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="906e7-125">For a tutorial on how tootransform data using Azure Data Factory, see [Tutorial: Build a pipeline tootransform data using Hadoop cluster](data-factory-build-your-first-pipeline.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="906e7-126">Krav</span><span class="sxs-lookup"><span data-stu-id="906e7-126">Prerequisites</span></span>
* <span data-ttu-id="906e7-127">Gå igenom [kursen översikt](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) och fullständig hello **nödvändiga** steg.</span><span class="sxs-lookup"><span data-stu-id="906e7-127">Go through [Tutorial Overview](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) and complete hello **prerequisite** steps.</span></span>
* <span data-ttu-id="906e7-128">Installera [Curl](https://curl.haxx.se/dlwiz/) på din dator.</span><span class="sxs-lookup"><span data-stu-id="906e7-128">Install [Curl](https://curl.haxx.se/dlwiz/) on your machine.</span></span> <span data-ttu-id="906e7-129">Verktyget hello Curl med övriga kommandon toocreate en datafabrik.</span><span class="sxs-lookup"><span data-stu-id="906e7-129">You use hello Curl tool with REST commands toocreate a data factory.</span></span> 
* <span data-ttu-id="906e7-130">Gör följande genom att följa anvisningarna i [den här artikeln](../azure-resource-manager/resource-group-create-service-principal-portal.md):</span><span class="sxs-lookup"><span data-stu-id="906e7-130">Follow instructions from [this article](../azure-resource-manager/resource-group-create-service-principal-portal.md) to:</span></span> 
  1. <span data-ttu-id="906e7-131">Skapa en webbapp med namnet **ADFCopyTutorialApp** i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="906e7-131">Create a Web application named **ADFCopyTutorialApp** in Azure Active Directory.</span></span>
  2. <span data-ttu-id="906e7-132">Hämta ett **klient-ID** och en **hemlig nyckel**.</span><span class="sxs-lookup"><span data-stu-id="906e7-132">Get **client ID** and **secret key**.</span></span> 
  3. <span data-ttu-id="906e7-133">Hämta ett **klientorganisations-ID**.</span><span class="sxs-lookup"><span data-stu-id="906e7-133">Get **tenant ID**.</span></span> 
  4. <span data-ttu-id="906e7-134">Tilldela hello **ADFCopyTutorialApp** programmet toohello **Data Factory deltagare** roll.</span><span class="sxs-lookup"><span data-stu-id="906e7-134">Assign hello **ADFCopyTutorialApp** application toohello **Data Factory Contributor** role.</span></span>  
* <span data-ttu-id="906e7-135">[Installera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="906e7-135">Install [Azure PowerShell](/powershell/azure/overview).</span></span>  
* <span data-ttu-id="906e7-136">Starta **PowerShell** och hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="906e7-136">Launch **PowerShell** and do hello following steps.</span></span> <span data-ttu-id="906e7-137">Behåll Azure PowerShell öppen tills hello slutet av den här kursen.</span><span class="sxs-lookup"><span data-stu-id="906e7-137">Keep Azure PowerShell open until hello end of this tutorial.</span></span> <span data-ttu-id="906e7-138">Om du stänga och öppna måste toorun hello kommandon igen.</span><span class="sxs-lookup"><span data-stu-id="906e7-138">If you close and reopen, you need toorun hello commands again.</span></span>
  
  1. <span data-ttu-id="906e7-139">Kör följande kommando hello och ange hello användarnamn och lösenord som du använder toosign i toohello Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="906e7-139">Run hello following command and enter hello user name and password that you use toosign in toohello Azure portal:</span></span>
    
    ```PowerShell 
    Login-AzureRmAccount
    ```   
  2. <span data-ttu-id="906e7-140">Kör följande kommando tooview hello alla hello prenumerationer för kontot:</span><span class="sxs-lookup"><span data-stu-id="906e7-140">Run hello following command tooview all hello subscriptions for this account:</span></span>

    ```PowerShell     
    Get-AzureRmSubscription
    ``` 
  3. <span data-ttu-id="906e7-141">Hello kör följande kommando tooselect hello prenumeration som du vill toowork med.</span><span class="sxs-lookup"><span data-stu-id="906e7-141">Run hello following command tooselect hello subscription that you want toowork with.</span></span> <span data-ttu-id="906e7-142">Ersätt  **&lt;NameOfAzureSubscription** &gt; med hello namnet på din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="906e7-142">Replace **&lt;NameOfAzureSubscription**&gt; with hello name of your Azure subscription.</span></span> 
     
    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext
    ```
  4. <span data-ttu-id="906e7-143">Skapa en Azure-resursgrupp med namnet **ADFTutorialResourceGroup** genom att köra följande kommando i hello PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="906e7-143">Create an Azure resource group named **ADFTutorialResourceGroup** by running hello following command in hello PowerShell:</span></span>  

    ```PowerShell     
      New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```
     
      <span data-ttu-id="906e7-144">Om hello resursgruppen finns redan, anger du om tooupdate den (Y) eller låta (n).</span><span class="sxs-lookup"><span data-stu-id="906e7-144">If hello resource group already exists, you specify whether tooupdate it (Y) or keep it as (N).</span></span> 
     
      <span data-ttu-id="906e7-145">Några av hello stegen i den här självstudiekursen förutsätts att du använder hello resursgrupp med namnet ADFTutorialResourceGroup.</span><span class="sxs-lookup"><span data-stu-id="906e7-145">Some of hello steps in this tutorial assume that you use hello resource group named ADFTutorialResourceGroup.</span></span> <span data-ttu-id="906e7-146">Om du använder en annan resursgrupp måste toouse hello namnet på resursgruppen i stället för ADFTutorialResourceGroup i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="906e7-146">If you use a different resource group, you need toouse hello name of your resource group in place of ADFTutorialResourceGroup in this tutorial.</span></span>

## <a name="create-json-definitions"></a><span data-ttu-id="906e7-147">Skapa JSON-definitioner</span><span class="sxs-lookup"><span data-stu-id="906e7-147">Create JSON definitions</span></span>
<span data-ttu-id="906e7-148">Skapa följande JSON-filer i hello mapp där curl.exe finns.</span><span class="sxs-lookup"><span data-stu-id="906e7-148">Create following JSON files in hello folder where curl.exe is located.</span></span> 

### <a name="datafactoryjson"></a><span data-ttu-id="906e7-149">datafactory.json</span><span class="sxs-lookup"><span data-stu-id="906e7-149">datafactory.json</span></span>
> [!IMPORTANT]
> <span data-ttu-id="906e7-150">Namnet måste vara globalt unika, så du kanske vill tooprefix-suffix ADFCopyTutorialDF toomake den ett unikt namn.</span><span class="sxs-lookup"><span data-stu-id="906e7-150">Name must be globally unique, so you may want tooprefix/suffix ADFCopyTutorialDF toomake it a unique name.</span></span> 
> 
> 

```JSON
{  
    "name": "ADFCopyTutorialDF",  
    "location": "WestUS"
}  
```

### <a name="azurestoragelinkedservicejson"></a><span data-ttu-id="906e7-151">azurestoragelinkedservice.json</span><span class="sxs-lookup"><span data-stu-id="906e7-151">azurestoragelinkedservice.json</span></span>
> [!IMPORTANT]
> <span data-ttu-id="906e7-152">Ersätt **accountname** och **accountkey** med namnet och nyckeln för ditt Azure-lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="906e7-152">Replace **accountname** and **accountkey** with name and key of your Azure storage account.</span></span> <span data-ttu-id="906e7-153">toolearn hur tooget lagringen åt nyckeln finns [visa, kopiera och generera lagring åtkomstnycklar](../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys).</span><span class="sxs-lookup"><span data-stu-id="906e7-153">toolearn how tooget your storage access key, see [View, copy and regenerate storage access keys](../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys).</span></span>

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

<span data-ttu-id="906e7-154">Mer information om egenskaper som JSON finns i [Azure Storage länkade tjänster](data-factory-azure-blob-connector.md#azure-storage-linked-service).</span><span class="sxs-lookup"><span data-stu-id="906e7-154">For details about JSON properties, see [Azure Storage linked service](data-factory-azure-blob-connector.md#azure-storage-linked-service).</span></span>

### <a name="azuersqllinkedservicejson"></a><span data-ttu-id="906e7-155">azuersqllinkedservice.json</span><span class="sxs-lookup"><span data-stu-id="906e7-155">azuersqllinkedservice.json</span></span>
> [!IMPORTANT]
> <span data-ttu-id="906e7-156">Ersätt **servername**, **databasename**, **användarnamn**, och **lösenord** med namnet på din Azure SQL-servern, namnet på SQL-databas användare konto och lösenord för hello-kontot.</span><span class="sxs-lookup"><span data-stu-id="906e7-156">Replace **servername**, **databasename**, **username**, and **password** with name of your Azure SQL server, name of SQL database, user account, and password for hello account.</span></span>  
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

<span data-ttu-id="906e7-157">Mer information om egenskaper som JSON finns i [Azure SQL länkade tjänster](data-factory-azure-sql-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="906e7-157">For details about JSON properties, see [Azure SQL linked service](data-factory-azure-sql-connector.md#linked-service-properties).</span></span>

### <a name="inputdatasetjson"></a><span data-ttu-id="906e7-158">inputdataset.json</span><span class="sxs-lookup"><span data-stu-id="906e7-158">inputdataset.json</span></span>

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

<span data-ttu-id="906e7-159">hello innehåller följande tabell beskrivningar för hello JSON egenskaper som används i hello fragment:</span><span class="sxs-lookup"><span data-stu-id="906e7-159">hello following table provides descriptions for hello JSON properties used in hello snippet:</span></span>

| <span data-ttu-id="906e7-160">Egenskap</span><span class="sxs-lookup"><span data-stu-id="906e7-160">Property</span></span> | <span data-ttu-id="906e7-161">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="906e7-161">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="906e7-162">typ</span><span class="sxs-lookup"><span data-stu-id="906e7-162">type</span></span> | <span data-ttu-id="906e7-163">hello typegenskapen har ställts in för**AzureBlob** eftersom data finns i ett Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="906e7-163">hello type property is set too**AzureBlob** because data resides in an Azure blob storage.</span></span> |
| <span data-ttu-id="906e7-164">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="906e7-164">linkedServiceName</span></span> | <span data-ttu-id="906e7-165">Refererar toohello **AzureStorageLinkedService** som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="906e7-165">Refers toohello **AzureStorageLinkedService** that you created earlier.</span></span> |
| <span data-ttu-id="906e7-166">folderPath</span><span class="sxs-lookup"><span data-stu-id="906e7-166">folderPath</span></span> | <span data-ttu-id="906e7-167">Anger hello blob **behållare** och hello **mappen** som innehåller inkommande blobbar.</span><span class="sxs-lookup"><span data-stu-id="906e7-167">Specifies hello blob **container** and hello **folder** that contains input blobs.</span></span> <span data-ttu-id="906e7-168">I den här självstudiekursen adftutorial är hello blob-behållaren och mappen är hello rotmappen.</span><span class="sxs-lookup"><span data-stu-id="906e7-168">In this tutorial, adftutorial is hello blob container and folder is hello root folder.</span></span> | 
| <span data-ttu-id="906e7-169">fileName</span><span class="sxs-lookup"><span data-stu-id="906e7-169">fileName</span></span> | <span data-ttu-id="906e7-170">Den här egenskapen är valfri.</span><span class="sxs-lookup"><span data-stu-id="906e7-170">This property is optional.</span></span> <span data-ttu-id="906e7-171">Om du utesluter den här egenskapen har alla filer från hello folderPath plockats.</span><span class="sxs-lookup"><span data-stu-id="906e7-171">If you omit this property, all files from hello folderPath are picked.</span></span> <span data-ttu-id="906e7-172">I den här självstudiekursen **emp.txt** har angetts för hello filnamn, så att endast filen hämtas för bearbetning.</span><span class="sxs-lookup"><span data-stu-id="906e7-172">In this tutorial, **emp.txt** is specified for hello fileName, so only that file is picked up for processing.</span></span> |
| <span data-ttu-id="906e7-173">format -> typ</span><span class="sxs-lookup"><span data-stu-id="906e7-173">format -> type</span></span> |<span data-ttu-id="906e7-174">hello indatafilen är i hello-format, så vi använder **TextFormat**.</span><span class="sxs-lookup"><span data-stu-id="906e7-174">hello input file is in hello text format, so we use **TextFormat**.</span></span> |
| <span data-ttu-id="906e7-175">columnDelimiter</span><span class="sxs-lookup"><span data-stu-id="906e7-175">columnDelimiter</span></span> | <span data-ttu-id="906e7-176">hello kolumner i hello indatafilen avgränsas med **kommatecken tecken (`,`)**.</span><span class="sxs-lookup"><span data-stu-id="906e7-176">hello columns in hello input file are delimited by **comma character (`,`)**.</span></span> |
| <span data-ttu-id="906e7-177">frekvens/intervall</span><span class="sxs-lookup"><span data-stu-id="906e7-177">frequency/interval</span></span> | <span data-ttu-id="906e7-178">hello frekvens har angetts för**timme** och intervallet anges för**1**, vilket innebär att hello indata segment är tillgängliga **varje timme**.</span><span class="sxs-lookup"><span data-stu-id="906e7-178">hello frequency is set too**Hour** and interval is  set too**1**, which means that hello input slices are available **hourly**.</span></span> <span data-ttu-id="906e7-179">Med andra ord hello Data Factory-tjänsten söker efter indata varje timme i hello rotmapp blob-behållaren (**adftutorial**) du har angett.</span><span class="sxs-lookup"><span data-stu-id="906e7-179">In other words, hello Data Factory service looks for input data every hour in hello root folder of blob container (**adftutorial**) you specified.</span></span> <span data-ttu-id="906e7-180">Det ser ut för hello data inom hello pipeline start- och tider, inte före eller efter dessa tider.</span><span class="sxs-lookup"><span data-stu-id="906e7-180">It looks for hello data within hello pipeline start and end times, not before or after these times.</span></span>  |
| <span data-ttu-id="906e7-181">extern</span><span class="sxs-lookup"><span data-stu-id="906e7-181">external</span></span> | <span data-ttu-id="906e7-182">Den här egenskapen anges för**SANT** om hello data inte genereras av denna pipeline.</span><span class="sxs-lookup"><span data-stu-id="906e7-182">This property is set too**true** if hello data is not generated by this pipeline.</span></span> <span data-ttu-id="906e7-183">hello inkommande data i den här självstudiekursen har hello emp.txt fil, som genereras av denna pipeline, så vi ställa in den här egenskapen tootrue.</span><span class="sxs-lookup"><span data-stu-id="906e7-183">hello input data in this tutorial is in hello emp.txt file, which is not generated by this pipeline, so we set this property tootrue.</span></span> |

<span data-ttu-id="906e7-184">Mer information om de här JSON-egenskaperna finns i artikeln [Azure Blob-anslutningsapp](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="906e7-184">For more information about these JSON properties, see [Azure Blob connector article](data-factory-azure-blob-connector.md#dataset-properties).</span></span>

### <a name="outputdatasetjson"></a><span data-ttu-id="906e7-185">outputdataset.json</span><span class="sxs-lookup"><span data-stu-id="906e7-185">outputdataset.json</span></span>

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
<span data-ttu-id="906e7-186">hello innehåller följande tabell beskrivningar för hello JSON egenskaper som används i hello fragment:</span><span class="sxs-lookup"><span data-stu-id="906e7-186">hello following table provides descriptions for hello JSON properties used in hello snippet:</span></span>

| <span data-ttu-id="906e7-187">Egenskap</span><span class="sxs-lookup"><span data-stu-id="906e7-187">Property</span></span> | <span data-ttu-id="906e7-188">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="906e7-188">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="906e7-189">typ</span><span class="sxs-lookup"><span data-stu-id="906e7-189">type</span></span> | <span data-ttu-id="906e7-190">hello typegenskapen har ställts in för**AzureSqlTable** eftersom data är kopierade tooa tabell i Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="906e7-190">hello type property is set too**AzureSqlTable** because data is copied tooa table in an Azure SQL database.</span></span> |
| <span data-ttu-id="906e7-191">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="906e7-191">linkedServiceName</span></span> | <span data-ttu-id="906e7-192">Refererar toohello **AzureSqlLinkedService** som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="906e7-192">Refers toohello **AzureSqlLinkedService** that you created earlier.</span></span> |
| <span data-ttu-id="906e7-193">tableName</span><span class="sxs-lookup"><span data-stu-id="906e7-193">tableName</span></span> | <span data-ttu-id="906e7-194">Angivna hello **tabell** toowhich hello data kopieras.</span><span class="sxs-lookup"><span data-stu-id="906e7-194">Specified hello **table** toowhich hello data is copied.</span></span> | 
| <span data-ttu-id="906e7-195">frekvens/intervall</span><span class="sxs-lookup"><span data-stu-id="906e7-195">frequency/interval</span></span> | <span data-ttu-id="906e7-196">hello frekvens har angetts för**timme** och är **1**, vilket innebär att hello utdata segment produceras **varje timme** mellan hello pipeline start och slut tider inte före eller När dessa tider.</span><span class="sxs-lookup"><span data-stu-id="906e7-196">hello frequency is set too**Hour** and interval is **1**, which means that hello output slices are produced **hourly** between hello pipeline start and end times, not before or after these times.</span></span>  |

<span data-ttu-id="906e7-197">Det finns tre kolumner – **ID**, **Förnamn**, och **efternamn** – i hello tomma tabellen i hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="906e7-197">There are three columns – **ID**, **FirstName**, and **LastName** – in hello emp table in hello database.</span></span> <span data-ttu-id="906e7-198">ID: T är en identitetskolumn, så du behöver bara toospecify **Förnamn** och **efternamn** här.</span><span class="sxs-lookup"><span data-stu-id="906e7-198">ID is an identity column, so you need toospecify only **FirstName** and **LastName** here.</span></span>

<span data-ttu-id="906e7-199">Mer information om de här JSON-egenskaperna finns i artikeln [Azure SQL-anslutningsapp](data-factory-azure-sql-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="906e7-199">For more information about these JSON properties, see [Azure SQL connector article](data-factory-azure-sql-connector.md#dataset-properties).</span></span>

### <a name="pipelinejson"></a><span data-ttu-id="906e7-200">pipeline.json</span><span class="sxs-lookup"><span data-stu-id="906e7-200">pipeline.json</span></span>

```JSON
{
  "name": "ADFTutorialPipeline",
  "properties": {
    "description": "Copy data from a blob tooAzure SQL table",
    "activities": [
      {
        "name": "CopyFromBlobToSQL",
        "description": "Push Regional Effectiveness Campaign data tooAzure SQL database",
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

<span data-ttu-id="906e7-201">Observera följande punkter hello:</span><span class="sxs-lookup"><span data-stu-id="906e7-201">Note hello following points:</span></span>

- <span data-ttu-id="906e7-202">Under hello aktiviteter är bara en aktivitet vars **typen** har angetts för**kopiera**.</span><span class="sxs-lookup"><span data-stu-id="906e7-202">In hello activities section, there is only one activity whose **type** is set too**Copy**.</span></span> <span data-ttu-id="906e7-203">Läs mer om hello kopieringsaktiviteten [data movement aktiviteter](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="906e7-203">For more information about hello copy activity, see [data movement activities](data-factory-data-movement-activities.md).</span></span> <span data-ttu-id="906e7-204">I Data Factory-lösningar, kan du också använda [datatransformeringsaktiviteter](data-factory-data-transformation-activities.md).</span><span class="sxs-lookup"><span data-stu-id="906e7-204">In Data Factory solutions, you can also use [data transformation activities](data-factory-data-transformation-activities.md).</span></span>
- <span data-ttu-id="906e7-205">Indata för hello aktiviteten är inställd för**AzureBlobInput** och utdata för hello aktiviteten är inställd för**AzureSqlOutput**.</span><span class="sxs-lookup"><span data-stu-id="906e7-205">Input for hello activity is set too**AzureBlobInput** and output for hello activity is set too**AzureSqlOutput**.</span></span> 
- <span data-ttu-id="906e7-206">I hello **typeProperties** avsnittet **BlobSource** har angetts som hello källtypen och **SqlSink** har angetts som hello Mottagartypen.</span><span class="sxs-lookup"><span data-stu-id="906e7-206">In hello **typeProperties** section, **BlobSource** is specified as hello source type and **SqlSink** is specified as hello sink type.</span></span> <span data-ttu-id="906e7-207">En fullständig lista över datakällor som stöds av hello kopieringsaktiviteten som källor och sänkor finns [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="906e7-207">For a complete list of data stores supported by hello copy activity as sources and sinks, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="906e7-208">toolearn hur toouse en specifik stöds data lagras som en källor/mottagare, klicka länken hello i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="906e7-208">toolearn how toouse a specific supported data store as a source/sink, click hello link in hello table.</span></span>  
 
<span data-ttu-id="906e7-209">Ersätt hello värdet för hello **starta** egenskap med hello aktuell dag och **end** värdet med hello nästa dag.</span><span class="sxs-lookup"><span data-stu-id="906e7-209">Replace hello value of hello **start** property with hello current day and **end** value with hello next day.</span></span> <span data-ttu-id="906e7-210">Du kan ange endast hello DatumDel och hoppa över hello time-delen av hello datum och tid.</span><span class="sxs-lookup"><span data-stu-id="906e7-210">You can specify only hello date part and skip hello time part of hello date time.</span></span> <span data-ttu-id="906e7-211">Till exempel ”2017-02-03”, vilket motsvarar för ”2017-02-03T00:00:00Z”</span><span class="sxs-lookup"><span data-stu-id="906e7-211">For example, "2017-02-03", which is equivalent too"2017-02-03T00:00:00Z"</span></span>
 
<span data-ttu-id="906e7-212">Både start- och slutdatum måste vara i [ISO-format](http://en.wikipedia.org/wiki/ISO_8601).</span><span class="sxs-lookup"><span data-stu-id="906e7-212">Both start and end datetimes must be in [ISO format](http://en.wikipedia.org/wiki/ISO_8601).</span></span> <span data-ttu-id="906e7-213">Exempel: 2016-10-14T16:32:41Z.</span><span class="sxs-lookup"><span data-stu-id="906e7-213">For example: 2016-10-14T16:32:41Z.</span></span> <span data-ttu-id="906e7-214">Hej **end** tid är valfritt, men vi använda den i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="906e7-214">hello **end** time is optional, but we use it in this tutorial.</span></span> 
 
<span data-ttu-id="906e7-215">Om du inte anger värdet för hello **end** egenskapen, det beräknas som ”**start + 48 timmar**”.</span><span class="sxs-lookup"><span data-stu-id="906e7-215">If you do not specify value for hello **end** property, it is calculated as "**start + 48 hours**".</span></span> <span data-ttu-id="906e7-216">toorun hello pipeline på obestämd tid, ange **9999-09-09** som hello värde hello **end** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="906e7-216">toorun hello pipeline indefinitely, specify **9999-09-09** as hello value for hello **end** property.</span></span>
 
<span data-ttu-id="906e7-217">I föregående exempel hello, finns det 24 datasektorer som varje datasektorn produceras per timma.</span><span class="sxs-lookup"><span data-stu-id="906e7-217">In hello preceding example, there are 24 data slices as each data slice is produced hourly.</span></span>

<span data-ttu-id="906e7-218">Beskrivningar av JSON-egenskaper i en pipeline-definition finns i artikeln [skapa pipelines](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="906e7-218">For descriptions of JSON properties in a pipeline definition, see [create pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="906e7-219">Beskrivningar av JSON-egenskaper i en kopieringsaktivitet-definition finns i artikeln [aktiviteter för dataflyttning](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="906e7-219">For descriptions of JSON properties in a copy activity definition, see [data movement activities](data-factory-data-movement-activities.md).</span></span> <span data-ttu-id="906e7-220">Beskrivningar av JSON-egenskaper som stöds av BlobSource finns i artikeln [Azure Blob-anslutningsapp](data-factory-azure-blob-connector.md).</span><span class="sxs-lookup"><span data-stu-id="906e7-220">For descriptions of JSON properties supported by BlobSource, see [Azure Blob connector article](data-factory-azure-blob-connector.md).</span></span> <span data-ttu-id="906e7-221">Beskrivningar av JSON-egenskaper som stöds av SqlSink finns i artikeln [Azure SQL Database-anslutningsapp](data-factory-azure-sql-connector.md).</span><span class="sxs-lookup"><span data-stu-id="906e7-221">For descriptions of JSON properties supported by SqlSink, see [Azure SQL Database connector article](data-factory-azure-sql-connector.md).</span></span>

## <a name="set-global-variables"></a><span data-ttu-id="906e7-222">Ange globala variabler</span><span class="sxs-lookup"><span data-stu-id="906e7-222">Set global variables</span></span>
<span data-ttu-id="906e7-223">Kör följande kommandon när du ersätter hello värdena med dina egna hello i Azure PowerShell:</span><span class="sxs-lookup"><span data-stu-id="906e7-223">In Azure PowerShell, execute hello following commands after replacing hello values with your own:</span></span>

> [!IMPORTANT]
> <span data-ttu-id="906e7-224">Anvisningar för hur du hämtar klient-ID, klienthemlighet, klientorganisations-ID och prenumerations-ID finns i [kravavsnittet](#prerequisites).</span><span class="sxs-lookup"><span data-stu-id="906e7-224">See [Prerequisites](#prerequisites) section for instructions on getting client ID, client secret, tenant ID, and subscription ID.</span></span>   
> 
> 

```JSON
$client_id = "<client ID of application in AAD>"
$client_secret = "<client key of application in AAD>"
$tenant = "<Azure tenant ID>";
$subscription_id="<Azure subscription ID>";

$rg = "ADFTutorialResourceGroup"
```

<span data-ttu-id="906e7-225">Kör följande kommando när du har uppdaterat hello namn i hello data factory du använder hello:</span><span class="sxs-lookup"><span data-stu-id="906e7-225">Run hello following command after updating hello name of hello data factory you are using:</span></span> 

```
$adf = "ADFCopyTutorialDF"
```

## <a name="authenticate-with-aad"></a><span data-ttu-id="906e7-226">Autentisera med AAD</span><span class="sxs-lookup"><span data-stu-id="906e7-226">Authenticate with AAD</span></span>
<span data-ttu-id="906e7-227">Kör följande kommando tooauthenticate med Azure Active Directory (AAD) hello:</span><span class="sxs-lookup"><span data-stu-id="906e7-227">Run hello following command tooauthenticate with Azure Active Directory (AAD):</span></span> 

```PowerShell
$cmd = { .\curl.exe -X POST https://login.microsoftonline.com/$tenant/oauth2/token  -F grant_type=client_credentials  -F resource=https://management.core.windows.net/ -F client_id=$client_id -F client_secret=$client_secret };
$responseToken = Invoke-Command -scriptblock $cmd;
$accessToken = (ConvertFrom-Json $responseToken).access_token;

(ConvertFrom-Json $responseToken) 
```

## <a name="create-data-factory"></a><span data-ttu-id="906e7-228">Skapa en datafabrik</span><span class="sxs-lookup"><span data-stu-id="906e7-228">Create data factory</span></span>
<span data-ttu-id="906e7-229">I det här steget ska du skapa en Azure Data Factory med namnet **ADFCopyTutorialDF**.</span><span class="sxs-lookup"><span data-stu-id="906e7-229">In this step, you create an Azure Data Factory named **ADFCopyTutorialDF**.</span></span> <span data-ttu-id="906e7-230">En datafabrik kan ha en eller flera pipelines.</span><span class="sxs-lookup"><span data-stu-id="906e7-230">A data factory can have one or more pipelines.</span></span> <span data-ttu-id="906e7-231">En pipeline kan innehålla en eller flera aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="906e7-231">A pipeline can have one or more activities in it.</span></span> <span data-ttu-id="906e7-232">Till exempel lagra en Kopieringsaktiviteten toocopy data från en källa tooa mål.</span><span class="sxs-lookup"><span data-stu-id="906e7-232">For example, a Copy Activity toocopy data from a source tooa destination data store.</span></span> <span data-ttu-id="906e7-233">En HDInsight Hive aktiviteten toorun en Hive-skript tootransform inkommande data tooproduct utdata.</span><span class="sxs-lookup"><span data-stu-id="906e7-233">A HDInsight Hive activity toorun a Hive script tootransform input data tooproduct output data.</span></span> <span data-ttu-id="906e7-234">Kör följande kommandon toocreate hello data factory hello:</span><span class="sxs-lookup"><span data-stu-id="906e7-234">Run hello following commands toocreate hello data factory:</span></span> 

1. <span data-ttu-id="906e7-235">Tilldela hello kommandot toovariable med namnet **cmd**.</span><span class="sxs-lookup"><span data-stu-id="906e7-235">Assign hello command toovariable named **cmd**.</span></span> 
   
    > [!IMPORTANT]
    > <span data-ttu-id="906e7-236">Bekräfta det hello-namnet i hello data factory som du anger här (ADFCopyTutorialDF) matchar hello namnet som angetts i hello **datafactory.json**.</span><span class="sxs-lookup"><span data-stu-id="906e7-236">Confirm that hello name of hello data factory you specify here (ADFCopyTutorialDF) matches hello name specified in hello **datafactory.json**.</span></span> 
   
    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@datafactory.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/ADFCopyTutorialDF0411?api-version=2015-10-01};
    ```
2. <span data-ttu-id="906e7-237">Kör hello kommando med hjälp av **Invoke-Command**.</span><span class="sxs-lookup"><span data-stu-id="906e7-237">Run hello command by using **Invoke-Command**.</span></span>
   
    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. <span data-ttu-id="906e7-238">Visa hello resultat.</span><span class="sxs-lookup"><span data-stu-id="906e7-238">View hello results.</span></span> <span data-ttu-id="906e7-239">Om hello datafabriken har skapats visas hello JSON för hello data factory i hello **resultat**, annars visas ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="906e7-239">If hello data factory has been successfully created, you see hello JSON for hello data factory in hello **results**; otherwise, you see an error message.</span></span>  
   
    ```
    Write-Host $results
    ```

<span data-ttu-id="906e7-240">Observera följande punkter hello:</span><span class="sxs-lookup"><span data-stu-id="906e7-240">Note hello following points:</span></span>

* <span data-ttu-id="906e7-241">hello namnet på hello Azure Data Factory måste vara globalt unika.</span><span class="sxs-lookup"><span data-stu-id="906e7-241">hello name of hello Azure Data Factory must be globally unique.</span></span> <span data-ttu-id="906e7-242">Om du ser hello fel i resultat: **datafabriksnamnet ”ADFCopyTutorialDF” är inte tillgänglig**, hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="906e7-242">If you see hello error in results: **Data factory name “ADFCopyTutorialDF” is not available**, do hello following steps:</span></span>  
  
  1. <span data-ttu-id="906e7-243">Ändra hello namn (till exempel yournameADFCopyTutorialDF) i hello **datafactory.json** fil.</span><span class="sxs-lookup"><span data-stu-id="906e7-243">Change hello name (for example, yournameADFCopyTutorialDF) in hello **datafactory.json** file.</span></span>
  2. <span data-ttu-id="906e7-244">I hello första kommandot där hello **$cmd** variabeln har tilldelats ett värde, Ersätt ADFCopyTutorialDF med hello nytt namn och kör hello-kommando.</span><span class="sxs-lookup"><span data-stu-id="906e7-244">In hello first command where hello **$cmd** variable is assigned a value, replace ADFCopyTutorialDF with hello new name and run hello command.</span></span> 
  3. <span data-ttu-id="906e7-245">Kör hello följande två kommandon tooinvoke hello REST API toocreate hello data factory och Skriv ut hello resultaten av hello igen.</span><span class="sxs-lookup"><span data-stu-id="906e7-245">Run hello next two commands tooinvoke hello REST API toocreate hello data factory and print hello results of hello operation.</span></span> 
     
     <span data-ttu-id="906e7-246">Se artikeln [Data Factory – namnregler](data-factory-naming-rules.md) för namnregler för Data Factory-artefakter.</span><span class="sxs-lookup"><span data-stu-id="906e7-246">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>
* <span data-ttu-id="906e7-247">toocreate Data Factory instanser måste toobe deltagare/administratören av hello Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="906e7-247">toocreate Data Factory instances, you need toobe a contributor/administrator of hello Azure subscription</span></span>
* <span data-ttu-id="906e7-248">hello namnet på hello data factory kan registreras som en DNS-namnet i hello framtida och därför bli synligt offentligt.</span><span class="sxs-lookup"><span data-stu-id="906e7-248">hello name of hello data factory may be registered as a DNS name in hello future and hence become publicly visible.</span></span>
* <span data-ttu-id="906e7-249">Om felmeddelandet hello ”:**den här prenumerationen är inte registrerad toouse namnområde Microsoft.DataFactory**”, gör du något av följande hello och försök att publicera igen:</span><span class="sxs-lookup"><span data-stu-id="906e7-249">If you receive hello error: "**This subscription is not registered toouse namespace Microsoft.DataFactory**", do one of hello following and try publishing again:</span></span> 
  
  * <span data-ttu-id="906e7-250">Kör hello efter kommandot tooregister hello Data Factory-providern i Azure PowerShell:</span><span class="sxs-lookup"><span data-stu-id="906e7-250">In Azure PowerShell, run hello following command tooregister hello Data Factory provider:</span></span> 

    ```PowerShell    
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```
    <span data-ttu-id="906e7-251">Du kan köra följande kommando tooconfirm hello som hello Data Factory providern är registrerad.</span><span class="sxs-lookup"><span data-stu-id="906e7-251">You can run hello following command tooconfirm that hello Data Factory provider is registered.</span></span> 
    
    ```PowerShell
    Get-AzureRmResourceProvider
    ```
  * <span data-ttu-id="906e7-252">Logga in med hjälp av hello Azure-prenumeration i hello [Azure-portalen](https://portal.azure.com) och navigera tooa Data Factory bladet (eller) skapa en datafabrik i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="906e7-252">Login using hello Azure subscription into hello [Azure portal](https://portal.azure.com) and navigate tooa Data Factory blade (or) create a data factory in hello Azure portal.</span></span> <span data-ttu-id="906e7-253">Den här åtgärden registrerar automatiskt hello-providern för dig.</span><span class="sxs-lookup"><span data-stu-id="906e7-253">This action automatically registers hello provider for you.</span></span>

<span data-ttu-id="906e7-254">Innan du skapar en pipeline, måste toocreate några Data Factory-entiteter först.</span><span class="sxs-lookup"><span data-stu-id="906e7-254">Before creating a pipeline, you need toocreate a few Data Factory entities first.</span></span> <span data-ttu-id="906e7-255">Du först skapa länkade tjänster toolink källa och mål data lagras tooyour data lagras.</span><span class="sxs-lookup"><span data-stu-id="906e7-255">You first create linked services toolink source and destination data stores tooyour data store.</span></span> <span data-ttu-id="906e7-256">Definiera inkommande och utgående datauppsättningar toorepresent data i länkade datalager.</span><span class="sxs-lookup"><span data-stu-id="906e7-256">Then, define input and output datasets toorepresent data in linked data stores.</span></span> <span data-ttu-id="906e7-257">Skapa slutligen hello pipeline med en aktivitet som använder dessa data.</span><span class="sxs-lookup"><span data-stu-id="906e7-257">Finally, create hello pipeline with an activity that uses these datasets.</span></span>

## <a name="create-linked-services"></a><span data-ttu-id="906e7-258">Skapa länkade tjänster</span><span class="sxs-lookup"><span data-stu-id="906e7-258">Create linked services</span></span>
<span data-ttu-id="906e7-259">Du kan skapa länkade tjänster i en data factory toolink dina data lagras och beräkna services toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="906e7-259">You create linked services in a data factory toolink your data stores and compute services toohello data factory.</span></span> <span data-ttu-id="906e7-260">I den här självstudiekursen kommer använder du inte någon beräkningstjänst, till exempel Azure HDInsight eller Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="906e7-260">In this tutorial, you don't use any compute service such as Azure HDInsight or Azure Data Lake Analytics.</span></span> <span data-ttu-id="906e7-261">Du använder två datalager av typen Azure Storage (källa) och Azure SQL Database (mål).</span><span class="sxs-lookup"><span data-stu-id="906e7-261">You use two data stores of type Azure Storage (source) and Azure SQL Database (destination).</span></span> <span data-ttu-id="906e7-262">Därför kan du skapa två länkade tjänster som heter AzureStorageLinkedService och AzureSqlLinkedService av typerna: AzureStorage och AzureSqlDatabase.</span><span class="sxs-lookup"><span data-stu-id="906e7-262">Therefore, you create two linked services named AzureStorageLinkedService and AzureSqlLinkedService of types: AzureStorage and AzureSqlDatabase.</span></span>  

<span data-ttu-id="906e7-263">Hej AzureStorageLinkedService länkar din Azure storage-konto toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="906e7-263">hello AzureStorageLinkedService links your Azure storage account toohello data factory.</span></span> <span data-ttu-id="906e7-264">Det här lagringskontot är hello något som du skapade en behållare och överföra hello data som en del av [krav](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="906e7-264">This storage account is hello one in which you created a container and uploaded hello data as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>   

<span data-ttu-id="906e7-265">AzureSqlLinkedService länkar din Azure SQL database toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="906e7-265">AzureSqlLinkedService links your Azure SQL database toohello data factory.</span></span> <span data-ttu-id="906e7-266">hello-data som kopieras från hello blob storage lagras i den här databasen.</span><span class="sxs-lookup"><span data-stu-id="906e7-266">hello data that is copied from hello blob storage is stored in this database.</span></span> <span data-ttu-id="906e7-267">Du har skapat hello tomma tabellen i den här databasen som en del av [krav](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="906e7-267">You created hello emp table in this database as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>  

### <a name="create-azure-storage-linked-service"></a><span data-ttu-id="906e7-268">Skapa en länkad Azure-lagringstjänst</span><span class="sxs-lookup"><span data-stu-id="906e7-268">Create Azure Storage linked service</span></span>
<span data-ttu-id="906e7-269">I det här steget kan länka du din Azure storage-konto tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="906e7-269">In this step, you link your Azure storage account tooyour data factory.</span></span> <span data-ttu-id="906e7-270">Du kan ange hello namn och nyckel för Azure storage-konto i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="906e7-270">You specify hello name and key of your Azure storage account in this section.</span></span> <span data-ttu-id="906e7-271">Se [Azure länkade lagringstjänsten](data-factory-azure-blob-connector.md#azure-storage-linked-service) mer information om toodefine för JSON-egenskaper som används för ett Azure Storage länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="906e7-271">See [Azure Storage linked service](data-factory-azure-blob-connector.md#azure-storage-linked-service) for details about JSON properties used toodefine an Azure Storage linked service.</span></span>  

1. <span data-ttu-id="906e7-272">Tilldela hello kommandot toovariable med namnet **cmd**.</span><span class="sxs-lookup"><span data-stu-id="906e7-272">Assign hello command toovariable named **cmd**.</span></span> 

    ```PowerShell   
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@azurestoragelinkedservice.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/AzureStorageLinkedService?api-version=2015-10-01};
    ```
2. <span data-ttu-id="906e7-273">Kör hello kommando med hjälp av **Invoke-Command**.</span><span class="sxs-lookup"><span data-stu-id="906e7-273">Run hello command by using **Invoke-Command**.</span></span>

    ```PowerShell   
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. <span data-ttu-id="906e7-274">Visa hello resultat.</span><span class="sxs-lookup"><span data-stu-id="906e7-274">View hello results.</span></span> <span data-ttu-id="906e7-275">Om hello länkad tjänst har skapats, så se hello JSON hello länkad tjänst i hello **resultat**, annars visas ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="906e7-275">If hello linked service has been successfully created, you see hello JSON for hello linked service in hello **results**; otherwise, you see an error message.</span></span>

    ```PowerShell   
    Write-Host $results
    ```

### <a name="create-azure-sql-linked-service"></a><span data-ttu-id="906e7-276">Skapa en länkad Azure SQL-tjänst</span><span class="sxs-lookup"><span data-stu-id="906e7-276">Create Azure SQL linked service</span></span>
<span data-ttu-id="906e7-277">I det här steget kan länka du din Azure SQL database tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="906e7-277">In this step, you link your Azure SQL database tooyour data factory.</span></span> <span data-ttu-id="906e7-278">Du kan ange hello Azure SQL-servernamnet, databasnamnet, användarnamn och lösenord i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="906e7-278">You specify hello Azure SQL server name, database name, user name, and user password in this section.</span></span> <span data-ttu-id="906e7-279">Se [Azure SQL länkade tjänsten](data-factory-azure-sql-connector.md#linked-service-properties) mer information om toodefine för JSON-egenskaper som används för en Azure SQL länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="906e7-279">See [Azure SQL linked service](data-factory-azure-sql-connector.md#linked-service-properties) for details about JSON properties used toodefine an Azure SQL linked service.</span></span>

1. <span data-ttu-id="906e7-280">Tilldela hello kommandot toovariable med namnet **cmd**.</span><span class="sxs-lookup"><span data-stu-id="906e7-280">Assign hello command toovariable named **cmd**.</span></span> 
   
    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@azuresqllinkedservice.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/AzureSqlLinkedService?api-version=2015-10-01};
    ```
2. <span data-ttu-id="906e7-281">Kör hello kommando med hjälp av **Invoke-Command**.</span><span class="sxs-lookup"><span data-stu-id="906e7-281">Run hello command by using **Invoke-Command**.</span></span>
   
    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. <span data-ttu-id="906e7-282">Visa hello resultat.</span><span class="sxs-lookup"><span data-stu-id="906e7-282">View hello results.</span></span> <span data-ttu-id="906e7-283">Om hello länkad tjänst har skapats, så se hello JSON hello länkad tjänst i hello **resultat**, annars visas ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="906e7-283">If hello linked service has been successfully created, you see hello JSON for hello linked service in hello **results**; otherwise, you see an error message.</span></span>
   
    ```PowerShell
    Write-Host $results
    ```

## <a name="create-datasets"></a><span data-ttu-id="906e7-284">Skapa datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="906e7-284">Create datasets</span></span>
<span data-ttu-id="906e7-285">I föregående steg hello Skapa länkade tjänster toolink dina Azure Storage-konto och Azure SQL database tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="906e7-285">In hello previous step, you created linked services toolink your Azure Storage account and Azure SQL database tooyour data factory.</span></span> <span data-ttu-id="906e7-286">I det här steget definierar du två datamängder som heter AzureBlobInput och AzureSqlOutput som representerar indata och utdata som är lagrad i hello datalager som anges av AzureStorageLinkedService och AzureSqlLinkedService respektive.</span><span class="sxs-lookup"><span data-stu-id="906e7-286">In this step, you define two datasets named AzureBlobInput and AzureSqlOutput that represent input and output data that is stored in hello data stores referred by AzureStorageLinkedService and AzureSqlLinkedService respectively.</span></span>

<span data-ttu-id="906e7-287">hello länkad Azure storage-tjänst anger hello anslutningssträngen som Data Factory-tjänsten använder vid körning tooconnect tooyour Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="906e7-287">hello Azure storage linked service specifies hello connection string that Data Factory service uses at run time tooconnect tooyour Azure storage account.</span></span> <span data-ttu-id="906e7-288">Och hello inkommande blobbdatauppsättning (AzureBlobInput) anger hello-behållaren och hello-mapp som innehåller hello indata.</span><span class="sxs-lookup"><span data-stu-id="906e7-288">And, hello input blob dataset (AzureBlobInput) specifies hello container and hello folder that contains hello input data.</span></span>  

<span data-ttu-id="906e7-289">På samma sätt anger hello Azure SQL Database länkad tjänst hello anslutningssträngen som Data Factory-tjänsten använder vid körning tooconnect tooyour Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="906e7-289">Similarly, hello Azure SQL Database linked service specifies hello connection string that Data Factory service uses at run time tooconnect tooyour Azure SQL database.</span></span> <span data-ttu-id="906e7-290">Och hello utgående SQL tabell datauppsättningen (OututDataset) anger hello tabellen i hello databasen toowhich hello kopieras data från hello blob storage.</span><span class="sxs-lookup"><span data-stu-id="906e7-290">And, hello output SQL table dataset (OututDataset) specifies hello table in hello database toowhich hello data from hello blob storage is copied.</span></span> 

### <a name="create-input-dataset"></a><span data-ttu-id="906e7-291">Skapa indatauppsättning</span><span class="sxs-lookup"><span data-stu-id="906e7-291">Create input dataset</span></span>
<span data-ttu-id="906e7-292">I det här steget skapar du en datauppsättning med namnet AzureBlobInput som pekar tooa blob-fil (emp.txt) i blob-behållaren (adftutorial) hello rotmapp i hello Azure Storage som representeras av hello AzureStorageLinkedService länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="906e7-292">In this step, you create a dataset named AzureBlobInput that points tooa blob file (emp.txt) in hello root folder of a blob container (adftutorial) in hello Azure Storage represented by hello AzureStorageLinkedService linked service.</span></span> <span data-ttu-id="906e7-293">Om du inte ange ett värde för hello filnamn (eller hoppa över den), är data från alla blobbar i hello inkommande mappen kopierade toohello mål.</span><span class="sxs-lookup"><span data-stu-id="906e7-293">If you don't specify a value for hello fileName (or skip it), data from all blobs in hello input folder are copied toohello destination.</span></span> <span data-ttu-id="906e7-294">I kursen får ange du ett värde för hello filnamnet.</span><span class="sxs-lookup"><span data-stu-id="906e7-294">In this tutorial, you specify a value for hello fileName.</span></span> 

1. <span data-ttu-id="906e7-295">Tilldela hello kommandot toovariable med namnet **cmd**.</span><span class="sxs-lookup"><span data-stu-id="906e7-295">Assign hello command toovariable named **cmd**.</span></span> 

    ```PowerSHell   
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@inputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobInput?api-version=2015-10-01};
    ```
2. <span data-ttu-id="906e7-296">Kör hello kommando med hjälp av **Invoke-Command**.</span><span class="sxs-lookup"><span data-stu-id="906e7-296">Run hello command by using **Invoke-Command**.</span></span>
   
    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. <span data-ttu-id="906e7-297">Visa hello resultat.</span><span class="sxs-lookup"><span data-stu-id="906e7-297">View hello results.</span></span> <span data-ttu-id="906e7-298">Om hello datauppsättningen har skapats visas hello JSON för hello datauppsättningen i hello **resultat**, annars visas ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="906e7-298">If hello dataset has been successfully created, you see hello JSON for hello dataset in hello **results**; otherwise, you see an error message.</span></span>
   
    ```PowerShell
    Write-Host $results
    ```

### <a name="create-output-dataset"></a><span data-ttu-id="906e7-299">Skapa datauppsättning för utdata</span><span class="sxs-lookup"><span data-stu-id="906e7-299">Create output dataset</span></span>
<span data-ttu-id="906e7-300">hello Azure SQL Database länkade tjänsten anger hello anslutningssträngen som Data Factory-tjänsten använder vid körning tooconnect tooyour Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="906e7-300">hello Azure SQL Database linked service specifies hello connection string that Data Factory service uses at run time tooconnect tooyour Azure SQL database.</span></span> <span data-ttu-id="906e7-301">hello SQL tabell utdatauppsättningen (OututDataset) du skapar i det här steget anger hello tabellen i hello toowhich hello databasdata från hello blob storage kopieras.</span><span class="sxs-lookup"><span data-stu-id="906e7-301">hello output SQL table dataset (OututDataset) you create in this step specifies hello table in hello database toowhich hello data from hello blob storage is copied.</span></span>

1. <span data-ttu-id="906e7-302">Tilldela hello kommandot toovariable med namnet **cmd**.</span><span class="sxs-lookup"><span data-stu-id="906e7-302">Assign hello command toovariable named **cmd**.</span></span>

    ```PowerShell   
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@outputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureSqlOutput?api-version=2015-10-01};
    ```
2. <span data-ttu-id="906e7-303">Kör hello kommando med hjälp av **Invoke-Command**.</span><span class="sxs-lookup"><span data-stu-id="906e7-303">Run hello command by using **Invoke-Command**.</span></span>
    
    ```PowerShell   
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. <span data-ttu-id="906e7-304">Visa hello resultat.</span><span class="sxs-lookup"><span data-stu-id="906e7-304">View hello results.</span></span> <span data-ttu-id="906e7-305">Om hello datauppsättningen har skapats visas hello JSON för hello datauppsättningen i hello **resultat**, annars visas ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="906e7-305">If hello dataset has been successfully created, you see hello JSON for hello dataset in hello **results**; otherwise, you see an error message.</span></span>
   
    ```PowerShell
    Write-Host $results
    ``` 

## <a name="create-pipeline"></a><span data-ttu-id="906e7-306">Skapa pipeline</span><span class="sxs-lookup"><span data-stu-id="906e7-306">Create pipeline</span></span>
<span data-ttu-id="906e7-307">I det här steget ska du skapa en pipeline med en **kopieringsaktivitet** som använder **AzureBlobInput** som indata och **AzureSqlOutput** som utdata.</span><span class="sxs-lookup"><span data-stu-id="906e7-307">In this step, you create a pipeline with a **copy activity** that uses **AzureBlobInput** as an input and **AzureSqlOutput** as an output.</span></span>

<span data-ttu-id="906e7-308">Datamängd för utdata är för närvarande vilka enheter hello schema.</span><span class="sxs-lookup"><span data-stu-id="906e7-308">Currently, output dataset is what drives hello schedule.</span></span> <span data-ttu-id="906e7-309">I den här kursen är utdatauppsättningen konfigurerade tooproduce ett segment en gång i timmen.</span><span class="sxs-lookup"><span data-stu-id="906e7-309">In this tutorial, output dataset is configured tooproduce a slice once an hour.</span></span> <span data-ttu-id="906e7-310">hello pipeline har en starttid och sluttid som är en dag från varandra, vilket är 24 timmar.</span><span class="sxs-lookup"><span data-stu-id="906e7-310">hello pipeline has a start time and end time that are one day apart, which is 24 hours.</span></span> <span data-ttu-id="906e7-311">Därför produceras 24 segment för utdatauppsättningen av hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="906e7-311">Therefore, 24 slices of output dataset are produced by hello pipeline.</span></span> 

1. <span data-ttu-id="906e7-312">Tilldela hello kommandot toovariable med namnet **cmd**.</span><span class="sxs-lookup"><span data-stu-id="906e7-312">Assign hello command toovariable named **cmd**.</span></span>

    ```PowerShell   
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@pipeline.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datapipelines/MyFirstPipeline?api-version=2015-10-01};
    ```
2. <span data-ttu-id="906e7-313">Kör hello kommando med hjälp av **Invoke-Command**.</span><span class="sxs-lookup"><span data-stu-id="906e7-313">Run hello command by using **Invoke-Command**.</span></span>

    ```PowerShell   
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. <span data-ttu-id="906e7-314">Visa hello resultat.</span><span class="sxs-lookup"><span data-stu-id="906e7-314">View hello results.</span></span> <span data-ttu-id="906e7-315">Om hello datauppsättningen har skapats visas hello JSON för hello datauppsättningen i hello **resultat**, annars visas ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="906e7-315">If hello dataset has been successfully created, you see hello JSON for hello dataset in hello **results**; otherwise, you see an error message.</span></span>  

    ```PowerShell   
    Write-Host $results
    ```

<span data-ttu-id="906e7-316">**Grattis!**</span><span class="sxs-lookup"><span data-stu-id="906e7-316">**Congratulations!**</span></span> <span data-ttu-id="906e7-317">Du har skapat ett Azure data factory med en rörledning som kopierar data från Azure Blob Storage tooAzure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="906e7-317">You have successfully created an Azure data factory, with a pipeline that copies data from Azure Blob Storage tooAzure SQL database.</span></span>

## <a name="monitor-pipeline"></a><span data-ttu-id="906e7-318">Övervaka pipeline</span><span class="sxs-lookup"><span data-stu-id="906e7-318">Monitor pipeline</span></span>
<span data-ttu-id="906e7-319">I det här steget kan använda du Data Factory REST API: et toomonitor segment som produceras av hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="906e7-319">In this step, you use Data Factory REST API toomonitor slices being produced by hello pipeline.</span></span>

```PowerShell
$ds ="AzureSqlOutput"
```

> [!IMPORTANT] 
> <span data-ttu-id="906e7-320">Kontrollera att hello start- och tider anges i hello efter kommandot Matcha hello start- och sluttider för hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="906e7-320">Make sure that hello start and end times specified in hello following command match hello start and end times of hello pipeline.</span></span> 

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

<span data-ttu-id="906e7-321">Kör hello Invoke-Command och hello nästa tills du ser en sektor i **klar** tillstånd eller **misslyckades** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="906e7-321">Run hello Invoke-Command and hello next one until you see a slice in **Ready** state or **Failed** state.</span></span> <span data-ttu-id="906e7-322">När hello sektorn är i tillståndet Ready, kontrollera hello **tomma** tabell i Azure SQL database för hello utdata.</span><span class="sxs-lookup"><span data-stu-id="906e7-322">When hello slice is in Ready state, check hello **emp** table in your Azure SQL database for hello output data.</span></span> 

<span data-ttu-id="906e7-323">Två rader med data från hello källfilen finns kopierade toohello tomma tabellen i hello Azure SQL-databas för varje segment.</span><span class="sxs-lookup"><span data-stu-id="906e7-323">For each slice, two rows of data from hello source file are copied toohello emp table in hello Azure SQL database.</span></span> <span data-ttu-id="906e7-324">Därför se 24 nya poster i hello tomma tabellen när alla hello segment har bearbetat (statusen Ready).</span><span class="sxs-lookup"><span data-stu-id="906e7-324">Therefore, you see 24 new records in hello emp table when all hello slices are successfully processed (in Ready state).</span></span> 

## <a name="summary"></a><span data-ttu-id="906e7-325">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="906e7-325">Summary</span></span>
<span data-ttu-id="906e7-326">I den här kursen används REST API toocreate ett Azure data factory toocopy data från Azure blob tooan Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="906e7-326">In this tutorial, you used REST API toocreate an Azure data factory toocopy data from an Azure blob tooan Azure SQL database.</span></span> <span data-ttu-id="906e7-327">Här följer hello anvisningar som du utförde i den här kursen:</span><span class="sxs-lookup"><span data-stu-id="906e7-327">Here are hello high-level steps you performed in this tutorial:</span></span>  

1. <span data-ttu-id="906e7-328">Du skapade en Azure **Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="906e7-328">Created an Azure **data factory**.</span></span>
2. <span data-ttu-id="906e7-329">Du skapade **länkade tjänster**:</span><span class="sxs-lookup"><span data-stu-id="906e7-329">Created **linked services**:</span></span>
   1. <span data-ttu-id="906e7-330">Ett Azure Storage länkade tjänsten toolink ditt Azure Storage-konto som innehåller data som indata.</span><span class="sxs-lookup"><span data-stu-id="906e7-330">An Azure Storage linked service toolink your Azure Storage account that holds input data.</span></span>     
   2. <span data-ttu-id="906e7-331">En Azure SQL länkade tjänsten toolink din Azure SQL-databas som innehåller hello utdata.</span><span class="sxs-lookup"><span data-stu-id="906e7-331">An Azure SQL linked service toolink your Azure SQL database that holds hello output data.</span></span> 
3. <span data-ttu-id="906e7-332">Du skapade **datauppsättningar** som beskriver indata och utdata för pipelines.</span><span class="sxs-lookup"><span data-stu-id="906e7-332">Created **datasets**, which describe input data and output data for pipelines.</span></span>
4. <span data-ttu-id="906e7-333">Du skapade en **pipeline** med en kopieringsaktivitet med BlobSource som källa och SqlSink som mottagare.</span><span class="sxs-lookup"><span data-stu-id="906e7-333">Created a **pipeline** with a Copy Activity with BlobSource as source and SqlSink as sink.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="906e7-334">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="906e7-334">Next steps</span></span>
<span data-ttu-id="906e7-335">I den här kursen används Azure blob storage som ett datalager för källa och en Azure SQL-databas som ett dataarkiv som mål i en kopieringsåtgärd.</span><span class="sxs-lookup"><span data-stu-id="906e7-335">In this tutorial, you used Azure blob storage as a source data store and an Azure SQL database as a destination data store in a copy operation.</span></span> <span data-ttu-id="906e7-336">hello innehåller följande tabell en lista över datakällor som stöds som källor och mål av hello kopieringsaktiviteten:</span><span class="sxs-lookup"><span data-stu-id="906e7-336">hello following table provides a list of data stores supported as sources and destinations by hello copy activity:</span></span> 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

<span data-ttu-id="906e7-337">toolearn om hur toocopy till eller från en databas, klicka länken hello för hello datalager i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="906e7-337">toolearn about how toocopy data to/from a data store, click hello link for hello data store in hello table.</span></span>
