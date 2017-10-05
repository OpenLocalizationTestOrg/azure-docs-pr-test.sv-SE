---
title: "Självstudiekurs: Skapa en pipeline för att flytta data med hjälp av Azure PowerShell | Microsoft-dokument"
description: "I den här självstudiekursen kommer du att skapa en Azure Data Factory-pipeline med en kopieringsaktivitet genom att använda Azure PowerShell."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 71087349-9365-4e95-9847-170658216ed8
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 81efe7c6af29af778686e1f6bcf62fedc9711052
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-create-a-data-factory-pipeline-that-moves-data-by-using-azure-powershell"></a><span data-ttu-id="c03af-103">Självstudiekurs: Skapa en Data Factory-pipeline som flyttar data med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="c03af-103">Tutorial: Create a Data Factory pipeline that moves data by using Azure PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c03af-104">Översikt och förutsättningar</span><span class="sxs-lookup"><span data-stu-id="c03af-104">Overview and prerequisites</span></span>](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [<span data-ttu-id="c03af-105">Guiden Kopiera</span><span class="sxs-lookup"><span data-stu-id="c03af-105">Copy Wizard</span></span>](data-factory-copy-data-wizard-tutorial.md)
> * [<span data-ttu-id="c03af-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c03af-106">Azure portal</span></span>](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [<span data-ttu-id="c03af-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c03af-107">Visual Studio</span></span>](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [<span data-ttu-id="c03af-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="c03af-108">PowerShell</span></span>](data-factory-copy-activity-tutorial-using-powershell.md)
> * [<span data-ttu-id="c03af-109">Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="c03af-109">Azure Resource Manager template</span></span>](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [<span data-ttu-id="c03af-110">REST API</span><span class="sxs-lookup"><span data-stu-id="c03af-110">REST API</span></span>](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [<span data-ttu-id="c03af-111">.NET-API</span><span class="sxs-lookup"><span data-stu-id="c03af-111">.NET API</span></span>](data-factory-copy-activity-tutorial-using-dotnet-api.md)
>
>

<span data-ttu-id="c03af-112">I den här artikeln får du lära dig hur du använder PowerShell för att skapa en datafabrik med en pipeline som kopierar data från en Azure-bloblagring till en Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="c03af-112">In this article, you learn how to use PowerShell to create a data factory with a pipeline that copies data from an Azure blob storage to an Azure SQL database.</span></span> <span data-ttu-id="c03af-113">Om du inte har använt Azure Data Factory, bör du läsa igenom artikeln [Introduktion till Azure Data Factory](data-factory-introduction.md) innan du genomför den här självstudien.</span><span class="sxs-lookup"><span data-stu-id="c03af-113">If you are new to Azure Data Factory, read through the [Introduction to Azure Data Factory](data-factory-introduction.md) article before doing this tutorial.</span></span>   

<span data-ttu-id="c03af-114">I den här självstudien får du skapa en pipeline i en aktivitet: kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="c03af-114">In this tutorial, you create a pipeline with one activity in it: Copy Activity.</span></span> <span data-ttu-id="c03af-115">Kopieringsaktiviteten kopierar data från källans datalager till mottagarens datalager.</span><span class="sxs-lookup"><span data-stu-id="c03af-115">The copy activity copies data from a supported data store to a supported sink data store.</span></span> <span data-ttu-id="c03af-116">En lista över datakällor som stöds som källor och mottagare finns i [datalager som stöds](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="c03af-116">For a list of data stores supported as sources and sinks, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="c03af-117">Aktiviteten drivs av en globalt tillgänglig tjänst som kan kopiera data mellan olika datalager på ett säkert, tillförlitligt och skalbart sätt.</span><span class="sxs-lookup"><span data-stu-id="c03af-117">The activity is powered by a globally available service that can copy data between various data stores in a secure, reliable, and scalable way.</span></span> <span data-ttu-id="c03af-118">Se artikeln [Dataförflyttningsaktiviteter](data-factory-data-movement-activities.md) för information om kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="c03af-118">For more information about the Copy Activity, see [Data Movement Activities](data-factory-data-movement-activities.md).</span></span>

<span data-ttu-id="c03af-119">En pipeline kan ha fler än en aktivitet.</span><span class="sxs-lookup"><span data-stu-id="c03af-119">A pipeline can have more than one activity.</span></span> <span data-ttu-id="c03af-120">Du kan länka två aktiviteter (köra en aktivitet efter en annan) genom att ställa in datauppsättningen för utdata för en aktivitet som den inkommande datauppsättningen för den andra aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="c03af-120">And, you can chain two activities (run one activity after another) by setting the output dataset of one activity as the input dataset of the other activity.</span></span> <span data-ttu-id="c03af-121">Mer information finns i [flera aktiviteter i en pipeline](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span><span class="sxs-lookup"><span data-stu-id="c03af-121">For more information, see [multiple activities in a pipeline](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span></span>

> [!NOTE]
> <span data-ttu-id="c03af-122">Den här artikeln beskriver inte alla Data Factory-cmdletar.</span><span class="sxs-lookup"><span data-stu-id="c03af-122">This article does not cover all the Data Factory cmdlets.</span></span> <span data-ttu-id="c03af-123">Se [Cmdlet-referens för Data Factory](/powershell/module/azurerm.datafactories) för omfattande dokumentation om dessa cmdletar.</span><span class="sxs-lookup"><span data-stu-id="c03af-123">See [Data Factory Cmdlet Reference](/powershell/module/azurerm.datafactories) for comprehensive documentation on these cmdlets.</span></span>
> 
> <span data-ttu-id="c03af-124">Datapipelinen i den här självstudien kopierar data från ett källdatalager till ett måldatalager.</span><span class="sxs-lookup"><span data-stu-id="c03af-124">The data pipeline in this tutorial copies data from a source data store to a destination data store.</span></span> <span data-ttu-id="c03af-125">Om du vill se en självstudie som visar hur du omvandlar data med Azure Data Factory går du till [Tutorial: Build a pipeline to transform data using Hadoop cluster](data-factory-build-your-first-pipeline.md) (Självstudie: Bygg en pipeline för att omvandla data med Hadoop-kluster).</span><span class="sxs-lookup"><span data-stu-id="c03af-125">For a tutorial on how to transform data using Azure Data Factory, see [Tutorial: Build a pipeline to transform data using Hadoop cluster](data-factory-build-your-first-pipeline.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c03af-126">Krav</span><span class="sxs-lookup"><span data-stu-id="c03af-126">Prerequisites</span></span>
- <span data-ttu-id="c03af-127">Slutför stegen i artikeln [Självstudier – förhandskrav](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="c03af-127">Complete prerequisites listed in the [tutorial prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) article.</span></span>
- <span data-ttu-id="c03af-128">**Installera Azure PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="c03af-128">Install **Azure PowerShell**.</span></span> <span data-ttu-id="c03af-129">Följ instruktionerna i [Så här installerar och konfigurerar du Azure PowerShell](../powershell-install-configure.md).</span><span class="sxs-lookup"><span data-stu-id="c03af-129">Follow the instructions in [How to install and configure Azure PowerShell](../powershell-install-configure.md).</span></span>

## <a name="steps"></a><span data-ttu-id="c03af-130">Steg</span><span class="sxs-lookup"><span data-stu-id="c03af-130">Steps</span></span>
<span data-ttu-id="c03af-131">Här är de steg du utför som en del av de här självstudierna:</span><span class="sxs-lookup"><span data-stu-id="c03af-131">Here are the steps you perform as part of this tutorial:</span></span>

1. <span data-ttu-id="c03af-132">Skapa en Azure-**datafabrik**.</span><span class="sxs-lookup"><span data-stu-id="c03af-132">Create an Azure **data factory**.</span></span> <span data-ttu-id="c03af-133">I det här steget skapar du en datafabrik med namnet ADFTutorialDataFactoryPSH.</span><span class="sxs-lookup"><span data-stu-id="c03af-133">In this step, you create a data factory named ADFTutorialDataFactoryPSH.</span></span> 
2. <span data-ttu-id="c03af-134">Skapa **länkade tjänster** i den här datafabriken.</span><span class="sxs-lookup"><span data-stu-id="c03af-134">Create **linked services** in the data factory.</span></span> <span data-ttu-id="c03af-135">I det här steget kan du skapa två länkade tjänster: Azure Storage och Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="c03af-135">In this step, you create two linked services of types: Azure Storage and Azure SQL Database.</span></span> 
    
    <span data-ttu-id="c03af-136">AzureStorageLinkedService länkar ditt Azure Storage-konto till datafabriken.</span><span class="sxs-lookup"><span data-stu-id="c03af-136">The AzureStorageLinkedService links your Azure storage account to the data factory.</span></span> <span data-ttu-id="c03af-137">Du har skapat en behållare och överfört data till det här lagringskontot som en del av [förhandskraven](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="c03af-137">You created a container and uploaded data to this storage account as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>   

    <span data-ttu-id="c03af-138">AzureSqlLinkedService länkar din Azure SQL-databas till datafabriken.</span><span class="sxs-lookup"><span data-stu-id="c03af-138">AzureSqlLinkedService links your Azure SQL database to the data factory.</span></span> <span data-ttu-id="c03af-139">Data som kopieras från blob-lagringen sparas i den här databasen.</span><span class="sxs-lookup"><span data-stu-id="c03af-139">The data that is copied from the blob storage is stored in this database.</span></span> <span data-ttu-id="c03af-140">Du har skapat den SQL-tabellen i den här databasen som en del av [förhandskraven](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="c03af-140">You created a SQL table in this database as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>   
3. <span data-ttu-id="c03af-141">Skapa **datauppsättningar** för indata och utdata i datafabriken.</span><span class="sxs-lookup"><span data-stu-id="c03af-141">Create input and output **datasets** in the data factory.</span></span>  
    
    <span data-ttu-id="c03af-142">Den länkade Azure storage-tjänsten anger anslutningssträngen som Data Factory-tjänsten använder vid körning för att ansluta till ditt Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="c03af-142">The Azure storage linked service specifies the connection string that Data Factory service uses at run time to connect to your Azure storage account.</span></span> <span data-ttu-id="c03af-143">Och en Azure Blob-datauppsättning anger vilken blobbehållare och mapp som innehåller data.</span><span class="sxs-lookup"><span data-stu-id="c03af-143">And, the input blob dataset specifies the container and the folder that contains the input data.</span></span>  

    <span data-ttu-id="c03af-144">Den länkade Azure SQL-databasen anger anslutningssträngen som Data Factory-tjänsten använder vid körning för att ansluta till ditt Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="c03af-144">Similarly, the Azure SQL Database linked service specifies the connection string that Data Factory service uses at run time to connect to your Azure SQL database.</span></span> <span data-ttu-id="c03af-145">Och utdatauppsättningen för SQL-tabellen anger tabellen i databasen som data kopieras till från blob-lagringen.</span><span class="sxs-lookup"><span data-stu-id="c03af-145">And, the output SQL table dataset specifies the table in the database to which the data from the blob storage is copied.</span></span>
4. <span data-ttu-id="c03af-146">Skapa en **pipeline** i datafabriken.</span><span class="sxs-lookup"><span data-stu-id="c03af-146">Create a **pipeline** in the data factory.</span></span> <span data-ttu-id="c03af-147">I det här steget kan du skapa en pipeline med en kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="c03af-147">In this step, you create a pipeline with a copy activity.</span></span>   
    
    <span data-ttu-id="c03af-148">Kopieringsaktiviteten kopierar data från en Azure-blob till en tabell i Azure SQL-databasen.</span><span class="sxs-lookup"><span data-stu-id="c03af-148">The copy activity copies data from a blob in the Azure blob storage to a table in the Azure SQL database.</span></span> <span data-ttu-id="c03af-149">Du kan använda en kopieringsaktivitet i en pipeline för att kopiera data från alla datakällor som stöds till ett mål som stöds.</span><span class="sxs-lookup"><span data-stu-id="c03af-149">You can use a copy activity in a pipeline to copy data from any supported source to any supported destination.</span></span> <span data-ttu-id="c03af-150">I avsnittet [Dataförflyttningsaktiviteter](data-factory-data-movement-activities.md#supported-data-stores-and-formats) finns en lista över datalager som stöds.</span><span class="sxs-lookup"><span data-stu-id="c03af-150">For a list of supported data stores, see [data movement activities](data-factory-data-movement-activities.md#supported-data-stores-and-formats) article.</span></span> 
5. <span data-ttu-id="c03af-151">Övervaka pipeline.</span><span class="sxs-lookup"><span data-stu-id="c03af-151">Monitor the pipeline.</span></span> <span data-ttu-id="c03af-152">I det här steget ska du **övervaka** sektorer från indata- och utdatauppsättningar med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c03af-152">In this step, you **monitor** the slices of input and output datasets by using PowerShell.</span></span>

## <a name="create-a-data-factory"></a><span data-ttu-id="c03af-153">Skapa en datafabrik</span><span class="sxs-lookup"><span data-stu-id="c03af-153">Create a data factory</span></span>
> [!IMPORTANT]
> <span data-ttu-id="c03af-154">Slutför [förutsättningarna för självstudiekursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) om du inte redan har utfört dessa.</span><span class="sxs-lookup"><span data-stu-id="c03af-154">Complete [prerequisites for the tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) if you haven't already done so.</span></span>   

<span data-ttu-id="c03af-155">En datafabrik kan ha en eller flera pipelines.</span><span class="sxs-lookup"><span data-stu-id="c03af-155">A data factory can have one or more pipelines.</span></span> <span data-ttu-id="c03af-156">En pipeline kan innehålla en eller flera aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="c03af-156">A pipeline can have one or more activities in it.</span></span> <span data-ttu-id="c03af-157">Det kan exempelvis vara en kopieringsaktivitet som kopierar data från en källa till ett måldataarkiv och en HDInsight Hive-aktivitet som kör Hive-skript för att transformera indata till produktutdata.</span><span class="sxs-lookup"><span data-stu-id="c03af-157">For example, a Copy Activity to copy data from a source to a destination data store and a HDInsight Hive activity to run a Hive script to transform input data to product output data.</span></span> <span data-ttu-id="c03af-158">Låt oss börja med att skapa datafabriken i det här steget.</span><span class="sxs-lookup"><span data-stu-id="c03af-158">Let's start with creating the data factory in this step.</span></span>

1. <span data-ttu-id="c03af-159">Starta **PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="c03af-159">Launch **PowerShell**.</span></span> <span data-ttu-id="c03af-160">Låt Azure PowerShell vara öppet tills du är klar med självstudien.</span><span class="sxs-lookup"><span data-stu-id="c03af-160">Keep Azure PowerShell open until the end of this tutorial.</span></span> <span data-ttu-id="c03af-161">Om du stänger och öppnar det igen måste du köra kommandona en gång till.</span><span class="sxs-lookup"><span data-stu-id="c03af-161">If you close and reopen, you need to run the commands again.</span></span>

    <span data-ttu-id="c03af-162">Kör följande kommando och ange det användarnamn och lösenord som du använder för att logga in i Azure Portal:</span><span class="sxs-lookup"><span data-stu-id="c03af-162">Run the following command, and enter the user name and password that you use to sign in to the Azure portal:</span></span>

    ```PowerShell
    Login-AzureRmAccount
    ```   
   
    <span data-ttu-id="c03af-163">Kör följande kommando för att visa alla prenumerationer för det här kontot:</span><span class="sxs-lookup"><span data-stu-id="c03af-163">Run the following command to view all the subscriptions for this account:</span></span>

    ```PowerShell
    Get-AzureRmSubscription
    ```

    <span data-ttu-id="c03af-164">Kör följande kommando för att välja den prenumeration som du vill arbeta med.</span><span class="sxs-lookup"><span data-stu-id="c03af-164">Run the following command to select the subscription that you want to work with.</span></span> <span data-ttu-id="c03af-165">Ersätt **&lt;NameOfAzureSubscription**&gt; med namnet på din Azure-prenumeration:</span><span class="sxs-lookup"><span data-stu-id="c03af-165">Replace **&lt;NameOfAzureSubscription**&gt; with the name of your Azure subscription:</span></span>

    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext
    ```
2. <span data-ttu-id="c03af-166">Skapa en Azure-resursgrupp med namnet **ADFTutorialResourceGroup** genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="c03af-166">Create an Azure resource group named **ADFTutorialResourceGroup** by running the following command:</span></span>

    ```PowerShell
    New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```
    
    <span data-ttu-id="c03af-167">Vissa av stegen i den här självstudien förutsätter att du använder resursgruppen med namnet **ADFTutorialResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="c03af-167">Some of the steps in this tutorial assume that you use the resource group named **ADFTutorialResourceGroup**.</span></span> <span data-ttu-id="c03af-168">Om du använder en annan resursgrupp måste du använda den i stället för ADFTutorialResourceGroup i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="c03af-168">If you use a different resource group, you need to use it in place of ADFTutorialResourceGroup in this tutorial.</span></span>
3. <span data-ttu-id="c03af-169">Kör cmdleten **New-AzureRmDataFactory** och skapa en datafabrik med namnet: **ADFTutorialDataFactoryPSH**:</span><span class="sxs-lookup"><span data-stu-id="c03af-169">Run the **New-AzureRmDataFactory** cmdlet to create a data factory named **ADFTutorialDataFactoryPSH**:</span></span>  

    ```PowerShell
    $df=New-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name ADFTutorialDataFactoryPSH –Location "West US"
    ```
    <span data-ttu-id="c03af-170">Det här namnet har kanske redan tagits.</span><span class="sxs-lookup"><span data-stu-id="c03af-170">This name may already have been taken.</span></span> <span data-ttu-id="c03af-171">Därför bör namnet på datafabriken göras unikt genom att lägga till ett prefix eller suffix (till exempel: ADFTutorialDataFactoryPSH05152017) och köra kommandot igen.</span><span class="sxs-lookup"><span data-stu-id="c03af-171">Therefore, make the name of the data factory unique by adding a prefix or suffix (for example: ADFTutorialDataFactoryPSH05152017) and run the command again.</span></span>  

<span data-ttu-id="c03af-172">Observera följande punkter:</span><span class="sxs-lookup"><span data-stu-id="c03af-172">Note the following points:</span></span>

* <span data-ttu-id="c03af-173">Namnet på Azure Data Factory måste vara globalt unikt.</span><span class="sxs-lookup"><span data-stu-id="c03af-173">The name of the Azure data factory must be globally unique.</span></span> <span data-ttu-id="c03af-174">Om du får följande fel ändrar du namnet (till exempel dittnamnADFTutorialDataFactoryPSH).</span><span class="sxs-lookup"><span data-stu-id="c03af-174">If you receive the following error, change the name (for example, yournameADFTutorialDataFactoryPSH).</span></span> <span data-ttu-id="c03af-175">Använd det här namnet i stället för ADFTutorialFactoryPSH när du utför stegen i självstudien.</span><span class="sxs-lookup"><span data-stu-id="c03af-175">Use this name in place of ADFTutorialFactoryPSH while performing steps in this tutorial.</span></span> <span data-ttu-id="c03af-176">Se artikeln [Data Factory – namnregler](data-factory-naming-rules.md) för information om Data Factory-artefakter.</span><span class="sxs-lookup"><span data-stu-id="c03af-176">See [Data Factory - Naming Rules](data-factory-naming-rules.md) for Data Factory artifacts.</span></span>

    ```
    Data factory name “ADFTutorialDataFactoryPSH” is not available
    ```
* <span data-ttu-id="c03af-177">Om du vill skapa Data Factory-instanser måste du vara deltagare/administratör för Azure-prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="c03af-177">To create Data Factory instances, you must be a contributor or administrator of the Azure subscription.</span></span>
* <span data-ttu-id="c03af-178">Namnet på datafabriken kan registreras som ett DNS-namn i framtiden och blir då synligt offentligt.</span><span class="sxs-lookup"><span data-stu-id="c03af-178">The name of the data factory may be registered as a DNS name in the future, and hence become publicly visible.</span></span>
* <span data-ttu-id="c03af-179">Följande fel kan visas: ”**Prenumerationen har inte registrerats för användning av namnområdet Microsoft.DataFactory.**”</span><span class="sxs-lookup"><span data-stu-id="c03af-179">You may receive the following error: "**This subscription is not registered to use namespace Microsoft.DataFactory.**"</span></span> <span data-ttu-id="c03af-180">Gör något av följande och försök publicera igen:</span><span class="sxs-lookup"><span data-stu-id="c03af-180">Do one of the following, and try publishing again:</span></span>

  * <span data-ttu-id="c03af-181">I Azure PowerShell kör du följande kommando för att registrera Data Factory-providern:</span><span class="sxs-lookup"><span data-stu-id="c03af-181">In Azure PowerShell, run the following command to register the Data Factory provider:</span></span>

    ```PowerShell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```

    <span data-ttu-id="c03af-182">Du kan köra följande kommando om du vill kontrollera att Data Factory-providern är registrerad:</span><span class="sxs-lookup"><span data-stu-id="c03af-182">Run the following command to confirm that the Data Factory provider is registered:</span></span>

    ```PowerShell
    Get-AzureRmResourceProvider
    ```
  * <span data-ttu-id="c03af-183">Logga in i [Azure Portal](https://portal.azure.com) via Azure-prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="c03af-183">Sign in by using the Azure subscription to the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="c03af-184">Gå till ett Data Factory-blad, eller skapa en datafabrik i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="c03af-184">Go to a Data Factory blade, or create a data factory in the Azure portal.</span></span> <span data-ttu-id="c03af-185">Med den här åtgärden registreras providern automatiskt.</span><span class="sxs-lookup"><span data-stu-id="c03af-185">This action automatically registers the provider for you.</span></span>

## <a name="create-linked-services"></a><span data-ttu-id="c03af-186">Skapa länkade tjänster</span><span class="sxs-lookup"><span data-stu-id="c03af-186">Create linked services</span></span>
<span data-ttu-id="c03af-187">Du kan skapa länkade tjänster i en datafabrik för att länka ditt datalager och beräkna datafabrik-tjänster.</span><span class="sxs-lookup"><span data-stu-id="c03af-187">You create linked services in a data factory to link your data stores and compute services to the data factory.</span></span> <span data-ttu-id="c03af-188">I den här självstudiekursen kommer använder du inte någon beräkningstjänst, till exempel Azure HDInsight eller Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="c03af-188">In this tutorial, you don't use any compute service such as Azure HDInsight or Azure Data Lake Analytics.</span></span> <span data-ttu-id="c03af-189">Du använder två datalager av typen Azure Storage (källa) och Azure SQL Database (mål).</span><span class="sxs-lookup"><span data-stu-id="c03af-189">You use two data stores of type Azure Storage (source) and Azure SQL Database (destination).</span></span> 

<span data-ttu-id="c03af-190">Därför kan du skapa två länkade tjänster som heter AzureStorageLinkedService och AzureSqlLinkedService av typerna: AzureStorage och AzureSqlDatabase.</span><span class="sxs-lookup"><span data-stu-id="c03af-190">Therefore, you create two linked services named AzureStorageLinkedService and AzureSqlLinkedService of types: AzureStorage and AzureSqlDatabase.</span></span>  

<span data-ttu-id="c03af-191">AzureStorageLinkedService länkar ditt Azure Storage-konto till datafabriken.</span><span class="sxs-lookup"><span data-stu-id="c03af-191">The AzureStorageLinkedService links your Azure storage account to the data factory.</span></span> <span data-ttu-id="c03af-192">Använd det lagringskonto i vilket du skapade en behållare och laddade upp data under [förberedelsestegen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="c03af-192">This storage account is the one in which you created a container and uploaded the data as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>   

<span data-ttu-id="c03af-193">AzureSqlLinkedService länkar din Azure SQL-databas till datafabriken.</span><span class="sxs-lookup"><span data-stu-id="c03af-193">AzureSqlLinkedService links your Azure SQL database to the data factory.</span></span> <span data-ttu-id="c03af-194">Data som kopieras från blob-lagringen sparas i den här databasen.</span><span class="sxs-lookup"><span data-stu-id="c03af-194">The data that is copied from the blob storage is stored in this database.</span></span> <span data-ttu-id="c03af-195">Du har skapat den tomma tabellen i den här databasen som en del av [förhandskraven](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="c03af-195">You created the emp table in this database as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span> 

### <a name="create-a-linked-service-for-an-azure-storage-account"></a><span data-ttu-id="c03af-196">Skapa en länkad tjänst för ett Azure-lagringskonto</span><span class="sxs-lookup"><span data-stu-id="c03af-196">Create a linked service for an Azure storage account</span></span>
<span data-ttu-id="c03af-197">I det här steget länkar du ditt Azure-lagringskonto till datafabriken.</span><span class="sxs-lookup"><span data-stu-id="c03af-197">In this step, you link your Azure storage account to your data factory.</span></span>

1. <span data-ttu-id="c03af-198">Skapa en JSON-fil med namnet **AzureStorageLinkedService.json** i mappen **C:\ADFGetStartedPSH** med följande innehåll: (skapa mappen ADFGetStartedPSH om den inte redan finns.)</span><span class="sxs-lookup"><span data-stu-id="c03af-198">Create a JSON file named **AzureStorageLinkedService.json** in **C:\ADFGetStartedPSH** folder with the following content: (Create the folder ADFGetStartedPSH if it does not already exist.)</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="c03af-199">Ersätt &lt;accountname&gt; och &lt;accountkey&gt; med namnet och nyckeln för ditt Azure-lagringskonto innan du sparar filen.</span><span class="sxs-lookup"><span data-stu-id="c03af-199">Replace &lt;accountname&gt; and &lt;accountkey&gt; with name and key of your Azure storage account before saving the file.</span></span> 

    ```json
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
2. <span data-ttu-id="c03af-200">I **Azure PowerShell** växlar du till appen **ADFGetStartedPSH**.</span><span class="sxs-lookup"><span data-stu-id="c03af-200">In **Azure PowerShell**, switch to the **ADFGetStartedPSH** folder.</span></span>
4. <span data-ttu-id="c03af-201">Kör cmdleten **New-AzureRmDataFactoryLinkedService** för att skapa den länkade tjänsten: **AzureStorageLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="c03af-201">Run the **New-AzureRmDataFactoryLinkedService** cmdlet to create the linked service: **AzureStorageLinkedService**.</span></span> <span data-ttu-id="c03af-202">Med den här cmdleten och andra Data Factory-cmdlets som du använder i den här självstudien måste du ange värden för parametrarna **ResourceGroupName** och **DataFactoryName**.</span><span class="sxs-lookup"><span data-stu-id="c03af-202">This cmdlet, and other Data Factory cmdlets you use in this tutorial requires you to pass values for the **ResourceGroupName** and **DataFactoryName** parameters.</span></span> <span data-ttu-id="c03af-203">Du kan också skicka DataFactory-objektet som returnerades av cmdlet:en New-AzureRmDataFactory utan att ange ResourceGroupName och DataFactoryName varje gång du kör en cmdlet.</span><span class="sxs-lookup"><span data-stu-id="c03af-203">Alternatively, you can pass the DataFactory object returned by the New-AzureRmDataFactory cmdlet without typing ResourceGroupName and DataFactoryName each time you run a cmdlet.</span></span> 

    ```PowerShell
    New-AzureRmDataFactoryLinkedService $df -File .\AzureStorageLinkedService.json
    ```
    <span data-ttu-id="c03af-204">Här är exempel på utdata:</span><span class="sxs-lookup"><span data-stu-id="c03af-204">Here is the sample output:</span></span>

    ```
    LinkedServiceName : AzureStorageLinkedService
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    Properties        : Microsoft.Azure.Management.DataFactories.Models.LinkedServiceProperties
    ProvisioningState : Succeeded
    ``` 

    <span data-ttu-id="c03af-205">Andra sätt att skapa den här länkade tjänsten är att ange resursgruppens namn och datafabriksnamnet istället för att ange DataFactory-objektet.</span><span class="sxs-lookup"><span data-stu-id="c03af-205">Other way of creating this linked service is to specify resource group name and data factory name instead of specifying the DataFactory object.</span></span>  

    ```PowerShell
    New-AzureRmDataFactoryLinkedService -ResourceGroupName ADFTutorialResourceGroup -DataFactoryName <Name of your data factory> -File .\AzureStorageLinkedService.json
    ```

### <a name="create-a-linked-service-for-an-azure-sql-database"></a><span data-ttu-id="c03af-206">Skapa en länkad tjänst för en Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="c03af-206">Create a linked service for an Azure SQL database</span></span>
<span data-ttu-id="c03af-207">I det här steget länkar du Azure SQL-databasen till din datafabrik.</span><span class="sxs-lookup"><span data-stu-id="c03af-207">In this step, you link your Azure SQL database to your data factory.</span></span>

1. <span data-ttu-id="c03af-208">Skapa en JSON-fil med namnet AzureSqlLinkedService.json i mappen C:\ADFGetStartedPSH med följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="c03af-208">Create a JSON file named AzureSqlLinkedService.json in C:\ADFGetStartedPSH folder with the following content:</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="c03af-209">Ersätt &lt;servername&gt;, &lt;databasename&gt;, &lt;username@servername&gt; och &lt;password&gt; med namnen för Azure SQL-servern, databasen, användarkontot och lösenordet.</span><span class="sxs-lookup"><span data-stu-id="c03af-209">Replace &lt;servername&gt;, &lt;databasename&gt;, &lt;username@servername&gt;, and &lt;password&gt; with names of your Azure SQL server, database, user account, and password.</span></span>
    
    ```json
    {
        "name": "AzureSqlLinkedService",
        "properties": {
            "type": "AzureSqlDatabase",
            "typeProperties": {
                "connectionString": "Server=tcp:<server>.database.windows.net,1433;Database=<databasename>;User ID=<user>@<server>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
            }
        }
     }
    ```
2. <span data-ttu-id="c03af-210">Kör följande kommando för att skapa en länkad tjänst:</span><span class="sxs-lookup"><span data-stu-id="c03af-210">Run the following command to create a linked service:</span></span>

    ```PowerShell
    New-AzureRmDataFactoryLinkedService $df -File .\AzureSqlLinkedService.json
    ```
    
    <span data-ttu-id="c03af-211">Här är exempel på utdata:</span><span class="sxs-lookup"><span data-stu-id="c03af-211">Here is the sample output:</span></span>

    ```
    LinkedServiceName : AzureSqlLinkedService
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    Properties        : Microsoft.Azure.Management.DataFactories.Models.LinkedServiceProperties
    ProvisioningState : Succeeded
    ```

   <span data-ttu-id="c03af-212">Bekräfta att inställningen **Tillåt åtkomst till Azure-tjänster** är aktiverad för SQL-databasservern.</span><span class="sxs-lookup"><span data-stu-id="c03af-212">Confirm that **Allow access to Azure services** setting is turned on for your SQL database server.</span></span> <span data-ttu-id="c03af-213">Gör så här för att kontrollera och aktivera den:</span><span class="sxs-lookup"><span data-stu-id="c03af-213">To verify and turn it on, do the following steps:</span></span>

    1. <span data-ttu-id="c03af-214">Logga in på [Azure-portalen](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="c03af-214">Log in to the [Azure portal](https://portal.azure.com)</span></span>
    2. <span data-ttu-id="c03af-215">Klicka på **fler tjänster >** till vänster och klicka på **SQL-servrar** i kategorin **DATABASER**.</span><span class="sxs-lookup"><span data-stu-id="c03af-215">Click **More services >** on the left, and click **SQL servers** in the **DATABASES** category.</span></span>
    3. <span data-ttu-id="c03af-216">Markera din server i listan över SQL-servrar.</span><span class="sxs-lookup"><span data-stu-id="c03af-216">Select your server in the list of SQL servers.</span></span>
    4. <span data-ttu-id="c03af-217">Klicka på länken **Visa brandväggsinställningar** på SQL server-bladet.</span><span class="sxs-lookup"><span data-stu-id="c03af-217">On the SQL server blade, click **Show firewall settings** link.</span></span>
    5. <span data-ttu-id="c03af-218">På bladet **Brandväggsinställningar** klickar du på **På** för **Tillåt åtkomst till Azure-tjänster**.</span><span class="sxs-lookup"><span data-stu-id="c03af-218">In the **Firewall settings** blade, click **ON** for **Allow access to Azure services**.</span></span>
    6. <span data-ttu-id="c03af-219">Klicka på **Spara** i verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="c03af-219">Click **Save** on the toolbar.</span></span> 

## <a name="create-datasets"></a><span data-ttu-id="c03af-220">Skapa datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="c03af-220">Create datasets</span></span>
<span data-ttu-id="c03af-221">I det föregående steget skapade du kopplade tjänster för att länka ett Azure-lagringskonto och en Azure SQL-databas till datafabriken.</span><span class="sxs-lookup"><span data-stu-id="c03af-221">In the previous step, you created linked services to link your Azure Storage account and Azure SQL database to your data factory.</span></span> <span data-ttu-id="c03af-222">I det här steget definierar du två datauppsättningar – InputDataset och OutputDataset – som visar in- och utdata som lagras i de datalager som refereras till av AzureStorageLinkedService och AzureSqlLinkedService.</span><span class="sxs-lookup"><span data-stu-id="c03af-222">In this step, you define two datasets named InputDataset and OutputDataset that represent input and output data that is stored in the data stores referred by AzureStorageLinkedService and AzureSqlLinkedService respectively.</span></span>

<span data-ttu-id="c03af-223">Den länkade Azure storage-tjänsten anger anslutningssträngen som Data Factory-tjänsten använder vid körning för att ansluta till ditt Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="c03af-223">The Azure storage linked service specifies the connection string that Data Factory service uses at run time to connect to your Azure storage account.</span></span> <span data-ttu-id="c03af-224">Och en indatauppsättning anger vilken blobbehållare och mapp som innehåller indata.</span><span class="sxs-lookup"><span data-stu-id="c03af-224">And, the input blob dataset (InputDataset) specifies the container and the folder that contains the input data.</span></span>  

<span data-ttu-id="c03af-225">Den länkade Azure SQL-databasen anger anslutningssträngen som Data Factory-tjänsten använder vid körning för att ansluta till ditt Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="c03af-225">Similarly, the Azure SQL Database linked service specifies the connection string that Data Factory service uses at run time to connect to your Azure SQL database.</span></span> <span data-ttu-id="c03af-226">Och utdatauppsättningen (OutputDataset) för SQL-tabellen anger tabellen i databasen som data kopieras till från blob-lagringen.</span><span class="sxs-lookup"><span data-stu-id="c03af-226">And, the output SQL table dataset (OututDataset) specifies the table in the database to which the data from the blob storage is copied.</span></span> 

### <a name="create-an-input-dataset"></a><span data-ttu-id="c03af-227">Skapa en indatauppsättning</span><span class="sxs-lookup"><span data-stu-id="c03af-227">Create an input dataset</span></span>
<span data-ttu-id="c03af-228">I det här steget skapar du en datauppsättning med namnet InputDataset som pekar på en blobfil (emp.ext) i rotmappen i en blobbehållare (adftutorial) i Azure Storage som representeras av den länkade tjänsten AzureStorageLinkedService.</span><span class="sxs-lookup"><span data-stu-id="c03af-228">In this step, you create a dataset named InputDataset that points to a blob file (emp.txt) in the root folder of a blob container (adftutorial) in the Azure Storage represented by the AzureStorageLinkedService linked service.</span></span> <span data-ttu-id="c03af-229">Om du inte anger ett värde för filnamnet (eller hoppar över det), kommer data från alla blobbar i indatamappen att kopieras till målet.</span><span class="sxs-lookup"><span data-stu-id="c03af-229">If you don't specify a value for the fileName (or skip it), data from all blobs in the input folder are copied to the destination.</span></span> <span data-ttu-id="c03af-230">I den här kursen anger du ett värde för filnamnet.</span><span class="sxs-lookup"><span data-stu-id="c03af-230">In this tutorial, you specify a value for the fileName.</span></span>  

1. <span data-ttu-id="c03af-231">Skapa en JSON-fil med namnet **InputDataset.json** i mappen **C:\ADFGetStartedPSH** med följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="c03af-231">Create a JSON file named **InputDataset.json** in the **C:\ADFGetStartedPSH** folder, with the following content:</span></span>

    ```json
    {
        "name": "InputDataset",
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
                "fileName": "emp.txt",
                "folderPath": "adftutorial/",
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

    <span data-ttu-id="c03af-232">Följande tabell innehåller beskrivningar av de JSON-egenskaper som användes i kodfragmentet:</span><span class="sxs-lookup"><span data-stu-id="c03af-232">The following table provides descriptions for the JSON properties used in the snippet:</span></span>

    | <span data-ttu-id="c03af-233">Egenskap</span><span class="sxs-lookup"><span data-stu-id="c03af-233">Property</span></span> | <span data-ttu-id="c03af-234">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c03af-234">Description</span></span> |
    |:--- |:--- |
    | <span data-ttu-id="c03af-235">typ</span><span class="sxs-lookup"><span data-stu-id="c03af-235">type</span></span> | <span data-ttu-id="c03af-236">Typegenskapen har angetts till **AzureBlob** eftersom det finns data i Azure Blob-lagringen.</span><span class="sxs-lookup"><span data-stu-id="c03af-236">The type property is set to **AzureBlob** because data resides in an Azure blob storage.</span></span> |
    | <span data-ttu-id="c03af-237">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="c03af-237">linkedServiceName</span></span> | <span data-ttu-id="c03af-238">Refererar till **AzureStorageLinkedService** som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="c03af-238">Refers to the **AzureStorageLinkedService** that you created earlier.</span></span> |
    | <span data-ttu-id="c03af-239">folderPath</span><span class="sxs-lookup"><span data-stu-id="c03af-239">folderPath</span></span> | <span data-ttu-id="c03af-240">Anger vilken **blobbehållare** och **mapp** som innehåller indatablobbar.</span><span class="sxs-lookup"><span data-stu-id="c03af-240">Specifies the blob **container** and the **folder** that contains input blobs.</span></span> <span data-ttu-id="c03af-241">I den här självstudiekursen adftutorial är blob-behållaren och -mappen rotmappen.</span><span class="sxs-lookup"><span data-stu-id="c03af-241">In this tutorial, adftutorial is the blob container and folder is the root folder.</span></span> | 
    | <span data-ttu-id="c03af-242">fileName</span><span class="sxs-lookup"><span data-stu-id="c03af-242">fileName</span></span> | <span data-ttu-id="c03af-243">Den här egenskapen är valfri.</span><span class="sxs-lookup"><span data-stu-id="c03af-243">This property is optional.</span></span> <span data-ttu-id="c03af-244">Om du tar bort egenskapen kommer alla filer från folderPath hämtas.</span><span class="sxs-lookup"><span data-stu-id="c03af-244">If you omit this property, all files from the folderPath are picked.</span></span> <span data-ttu-id="c03af-245">I den här självstudiekursen har angetts **emp.txt** som filnamn så att endast den filen hämtas för bearbetning.</span><span class="sxs-lookup"><span data-stu-id="c03af-245">In this tutorial, **emp.txt** is specified for the fileName, so only that file is picked up for processing.</span></span> |
    | <span data-ttu-id="c03af-246">format -> typ</span><span class="sxs-lookup"><span data-stu-id="c03af-246">format -> type</span></span> |<span data-ttu-id="c03af-247">Indatafilen är i textformat, så vi använder **TextFormat**.</span><span class="sxs-lookup"><span data-stu-id="c03af-247">The input file is in the text format, so we use **TextFormat**.</span></span> |
    | <span data-ttu-id="c03af-248">columnDelimiter</span><span class="sxs-lookup"><span data-stu-id="c03af-248">columnDelimiter</span></span> | <span data-ttu-id="c03af-249">Kolumner i loggfilerna avgränsas med **kommatecken (`,`)**.</span><span class="sxs-lookup"><span data-stu-id="c03af-249">The columns in the input file are delimited by **comma character (`,`)**.</span></span> |
    | <span data-ttu-id="c03af-250">frekvens/intervall</span><span class="sxs-lookup"><span data-stu-id="c03af-250">frequency/interval</span></span> | <span data-ttu-id="c03af-251">Frekvensen är **timme** och intervallet är **1**, vilket innebär att indatasektorerna är tillgängliga en gång i **timmen**.</span><span class="sxs-lookup"><span data-stu-id="c03af-251">The frequency is set to **Hour** and interval is  set to **1**, which means that the input slices are available **hourly**.</span></span> <span data-ttu-id="c03af-252">Det betyder att tjänsten Data Factory söker efter indata varje timme i rotmappen för den angivna blobbehållaren (**adftutorial**).</span><span class="sxs-lookup"><span data-stu-id="c03af-252">In other words, the Data Factory service looks for input data every hour in the root folder of blob container (**adftutorial**) you specified.</span></span> <span data-ttu-id="c03af-253">Den söker data i pipelinens start- och sluttider och inte före eller efter dessa tider.</span><span class="sxs-lookup"><span data-stu-id="c03af-253">It looks for the data within the pipeline start and end times, not before or after these times.</span></span>  |
    | <span data-ttu-id="c03af-254">extern</span><span class="sxs-lookup"><span data-stu-id="c03af-254">external</span></span> | <span data-ttu-id="c03af-255">Den här egenskapen anges som **true** om indatan inte skapades av denna pipeline.</span><span class="sxs-lookup"><span data-stu-id="c03af-255">This property is set to **true** if the data is not generated by this pipeline.</span></span> <span data-ttu-id="c03af-256">Inkommande data i den här självstudien finns i filen emp.txt som genereras av denna pipeline, så vi ställer in den här egenskapen på true.</span><span class="sxs-lookup"><span data-stu-id="c03af-256">The input data in this tutorial is in the emp.txt file, which is not generated by this pipeline, so we set this property to true.</span></span> |

    <span data-ttu-id="c03af-257">Mer information om de här JSON-egenskaperna finns i artikeln [Azure Blob-anslutningsapp](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="c03af-257">For more information about these JSON properties, see [Azure Blob connector article](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
2. <span data-ttu-id="c03af-258">Kör följande kommando för att skapa Data Factory-datauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="c03af-258">Run the following command to create the Data Factory dataset.</span></span>

    ```PowerShell  
    New-AzureRmDataFactoryDataset $df -File .\InputDataset.json
    ```
    <span data-ttu-id="c03af-259">Här är exempel på utdata:</span><span class="sxs-lookup"><span data-stu-id="c03af-259">Here is the sample output:</span></span>

    ```
    DatasetName       : InputDataset
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    Availability      : Microsoft.Azure.Management.DataFactories.Common.Models.Availability
    Location          : Microsoft.Azure.Management.DataFactories.Models.AzureBlobDataset
    Policy            : Microsoft.Azure.Management.DataFactories.Common.Models.Policy
    Structure         : {FirstName, LastName}
    Properties        : Microsoft.Azure.Management.DataFactories.Models.DatasetProperties
    ProvisioningState : Succeeded
    ```

### <a name="create-an-output-dataset"></a><span data-ttu-id="c03af-260">Skapa en datauppsättning för utdata</span><span class="sxs-lookup"><span data-stu-id="c03af-260">Create an output dataset</span></span>
<span data-ttu-id="c03af-261">I den här delen av steget ska du skapa en utdatauppsättning med namnet **OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="c03af-261">In this part of the step, you create an output dataset named **OutputDataset**.</span></span> <span data-ttu-id="c03af-262">Den här datauppsättningen pekar på en SQL-tabell i Azure SQL-databasen som representeras av **AzureSqlLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="c03af-262">This dataset points to a SQL table in the Azure SQL database represented by **AzureSqlLinkedService**.</span></span> 

1. <span data-ttu-id="c03af-263">Skapa en JSON-fil med namnet **OutputDataset.json** i mappen **C:\ADFGetStartedPSH** med följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="c03af-263">Create a JSON file named **OutputDataset.json** in the **C:\ADFGetStartedPSH** folder with the following content:</span></span>

    ```json
    {
        "name": "OutputDataset",
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

    <span data-ttu-id="c03af-264">Följande tabell innehåller beskrivningar av de JSON-egenskaper som användes i kodfragmentet:</span><span class="sxs-lookup"><span data-stu-id="c03af-264">The following table provides descriptions for the JSON properties used in the snippet:</span></span>

    | <span data-ttu-id="c03af-265">Egenskap</span><span class="sxs-lookup"><span data-stu-id="c03af-265">Property</span></span> | <span data-ttu-id="c03af-266">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c03af-266">Description</span></span> |
    |:--- |:--- |
    | <span data-ttu-id="c03af-267">typ</span><span class="sxs-lookup"><span data-stu-id="c03af-267">type</span></span> | <span data-ttu-id="c03af-268">Typegenskapen är **AzureSqlTable** eftersom data kopieras till en tabell i en Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="c03af-268">The type property is set to **AzureSqlTable** because data is copied to a table in an Azure SQL database.</span></span> |
    | <span data-ttu-id="c03af-269">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="c03af-269">linkedServiceName</span></span> | <span data-ttu-id="c03af-270">Refererar till **AzureSqlLinkedService** som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="c03af-270">Refers to the **AzureSqlLinkedService** that you created earlier.</span></span> |
    | <span data-ttu-id="c03af-271">tableName</span><span class="sxs-lookup"><span data-stu-id="c03af-271">tableName</span></span> | <span data-ttu-id="c03af-272">Ange **tabellen** dit data kopieras.</span><span class="sxs-lookup"><span data-stu-id="c03af-272">Specified the **table** to which the data is copied.</span></span> | 
    | <span data-ttu-id="c03af-273">frekvens/intervall</span><span class="sxs-lookup"><span data-stu-id="c03af-273">frequency/interval</span></span> | <span data-ttu-id="c03af-274">Frekvensen är inställd på **timme** och intervallet är **1**, vilket innebär att utdatasegment produceras **varje timme** mellan pipelinens start- och sluttider, inte före eller efter dessa tider.</span><span class="sxs-lookup"><span data-stu-id="c03af-274">The frequency is set to **Hour** and interval is **1**, which means that the output slices are produced **hourly** between the pipeline start and end times, not before or after these times.</span></span>  |

    <span data-ttu-id="c03af-275">Det finns tre kolumner – **ID**, **FirstName** och **LastName** – i emp-tabellen i databasen.</span><span class="sxs-lookup"><span data-stu-id="c03af-275">There are three columns – **ID**, **FirstName**, and **LastName** – in the emp table in the database.</span></span> <span data-ttu-id="c03af-276">ID är en identitetskolumn, så du anger bara **FirstName** och **LastName** här.</span><span class="sxs-lookup"><span data-stu-id="c03af-276">ID is an identity column, so you need to specify only **FirstName** and **LastName** here.</span></span>

    <span data-ttu-id="c03af-277">Mer information om de här JSON-egenskaperna finns i artikeln [Azure SQL-anslutningsapp](data-factory-azure-sql-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="c03af-277">For more information about these JSON properties, see [Azure SQL connector article](data-factory-azure-sql-connector.md#dataset-properties).</span></span>
2. <span data-ttu-id="c03af-278">Skapa datafabriksdatauppsättningen genom att köra följande kommando.</span><span class="sxs-lookup"><span data-stu-id="c03af-278">Run the following command to create the data factory dataset.</span></span>

    ```PowerShell   
    New-AzureRmDataFactoryDataset $df -File .\OutputDataset.json
    ```

    <span data-ttu-id="c03af-279">Här är exempel på utdata:</span><span class="sxs-lookup"><span data-stu-id="c03af-279">Here is the sample output:</span></span>

    ```
    DatasetName       : OutputDataset
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    Availability      : Microsoft.Azure.Management.DataFactories.Common.Models.Availability
    Location          : Microsoft.Azure.Management.DataFactories.Models.AzureSqlTableDataset
    Policy            :
    Structure         : {FirstName, LastName}
    Properties        : Microsoft.Azure.Management.DataFactories.Models.DatasetProperties
    ProvisioningState : Succeeded
    ```

## <a name="create-a-pipeline"></a><span data-ttu-id="c03af-280">Skapa en pipeline</span><span class="sxs-lookup"><span data-stu-id="c03af-280">Create a pipeline</span></span>
<span data-ttu-id="c03af-281">I det här steget ska du skapa en pipeline med en **kopieringsaktivitet** som använder **InputDataset** som indata och **OutputDataset** som utdata.</span><span class="sxs-lookup"><span data-stu-id="c03af-281">In this step, you create a pipeline with a **copy activity** that uses **InputDataset** as an input and **OutputDataset** as an output.</span></span>

<span data-ttu-id="c03af-282">Schemat styrs för närvarande av utdatamängd.</span><span class="sxs-lookup"><span data-stu-id="c03af-282">Currently, output dataset is what drives the schedule.</span></span> <span data-ttu-id="c03af-283">I den här självstudiekursen är datamängden för utdata konfigurerad för att skapa ett segment en gång i timmen.</span><span class="sxs-lookup"><span data-stu-id="c03af-283">In this tutorial, output dataset is configured to produce a slice once an hour.</span></span> <span data-ttu-id="c03af-284">Pipelinen har en starttid och sluttid som är en dag från varandra, vilket är 24 timmar.</span><span class="sxs-lookup"><span data-stu-id="c03af-284">The pipeline has a start time and end time that are one day apart, which is 24 hours.</span></span> <span data-ttu-id="c03af-285">Därför produceras 24 segment för utdatauppsättningen av pipeline.</span><span class="sxs-lookup"><span data-stu-id="c03af-285">Therefore, 24 slices of output dataset are produced by the pipeline.</span></span> 


1. <span data-ttu-id="c03af-286">Skapa en JSON-fil med namnet **ADFTutorialPipeline.json** i mappen **C:\ADFGetStartedPSH** med följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="c03af-286">Create a JSON file named **ADFTutorialPipeline.json** in the **C:\ADFGetStartedPSH** folder, with the following content:</span></span>

    ```json   
    {
      "name": "ADFTutorialPipeline",
      "properties": {
        "description": "Copy data from a blob to Azure SQL table",
        "activities": [
          {
            "name": "CopyFromBlobToSQL",
            "type": "Copy",
            "inputs": [
              {
                "name": "InputDataset"
              }
            ],
            "outputs": [
              {
                "name": "OutputDataset"
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
    <span data-ttu-id="c03af-287">Observera följande punkter:</span><span class="sxs-lookup"><span data-stu-id="c03af-287">Note the following points:</span></span>
   
    - <span data-ttu-id="c03af-288">I avsnittet Aktiviteter finns det bara en aktivitet vars **typ** anges till **Kopia**.</span><span class="sxs-lookup"><span data-stu-id="c03af-288">In the activities section, there is only one activity whose **type** is set to **Copy**.</span></span> <span data-ttu-id="c03af-289">Se artikeln [Dataförflyttningsaktiviteter](data-factory-data-movement-activities.md) för information om kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="c03af-289">For more information about the copy activity, see [data movement activities](data-factory-data-movement-activities.md).</span></span> <span data-ttu-id="c03af-290">I Data Factory-lösningar, kan du också använda [datatransformeringsaktiviteter](data-factory-data-transformation-activities.md).</span><span class="sxs-lookup"><span data-stu-id="c03af-290">In Data Factory solutions, you can also use [data transformation activities](data-factory-data-transformation-activities.md).</span></span>
    - <span data-ttu-id="c03af-291">Indata för aktiviteten är inställd på **InputDataset** och utdata för aktiviteten är inställd på **OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="c03af-291">Input for the activity is set to **InputDataset** and output for the activity is set to **OutputDataset**.</span></span> 
    - <span data-ttu-id="c03af-292">I avsnittet för **typeProperties** har **BlobSource** angetts som källtyp och **SqlSink** har angetts som mottagartyp.</span><span class="sxs-lookup"><span data-stu-id="c03af-292">In the **typeProperties** section, **BlobSource** is specified as the source type and **SqlSink** is specified as the sink type.</span></span> <span data-ttu-id="c03af-293">En fullständig lista över datakällor som stöds av kopieringsaktiviteten som källor och mottagare finns i [Datalager som stöds](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="c03af-293">For a complete list of data stores supported by the copy activity as sources and sinks, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="c03af-294">Klicka på länken i tabellen om du vill veta hur du använder ett visst datalager som stöds som källa/mottagare.</span><span class="sxs-lookup"><span data-stu-id="c03af-294">To learn how to use a specific supported data store as a source/sink, click the link in the table.</span></span>  
     
    <span data-ttu-id="c03af-295">Ersätt värdet i **start**egenskapen med den aktuella dagen och **slut**värdet med nästa dag.</span><span class="sxs-lookup"><span data-stu-id="c03af-295">Replace the value of the **start** property with the current day and **end** value with the next day.</span></span> <span data-ttu-id="c03af-296">Du kan ange endast datumdelen och hoppa över tidsvärdet.</span><span class="sxs-lookup"><span data-stu-id="c03af-296">You can specify only the date part and skip the time part of the date time.</span></span> <span data-ttu-id="c03af-297">Till exempel ”2016-02-03” som motsvarar ”2016-02-03T00:00:00Z”</span><span class="sxs-lookup"><span data-stu-id="c03af-297">For example, "2016-02-03", which is equivalent to "2016-02-03T00:00:00Z"</span></span>
     
    <span data-ttu-id="c03af-298">Både start- och slutdatum måste vara i [ISO-format](http://en.wikipedia.org/wiki/ISO_8601).</span><span class="sxs-lookup"><span data-stu-id="c03af-298">Both start and end datetimes must be in [ISO format](http://en.wikipedia.org/wiki/ISO_8601).</span></span> <span data-ttu-id="c03af-299">Exempel: 2016-10-14T16:32:41Z.</span><span class="sxs-lookup"><span data-stu-id="c03af-299">For example: 2016-10-14T16:32:41Z.</span></span> <span data-ttu-id="c03af-300">**Sluttiden** är valfri, men vi använder den i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="c03af-300">The **end** time is optional, but we use it in this tutorial.</span></span> 
     
    <span data-ttu-id="c03af-301">Om du inte anger värdet för **slut**egenskapen, beräknas det som ”**start + 48 timmar**”.</span><span class="sxs-lookup"><span data-stu-id="c03af-301">If you do not specify value for the **end** property, it is calculated as "**start + 48 hours**".</span></span> <span data-ttu-id="c03af-302">Om du vill köra pipelinen på obestämd tid, anger du **9999-09-09** som värde för **slut**egenskapen.</span><span class="sxs-lookup"><span data-stu-id="c03af-302">To run the pipeline indefinitely, specify **9999-09-09** as the value for the **end** property.</span></span>
     
    <span data-ttu-id="c03af-303">I det föregående exemplet finns det 24 datasektorer eftersom varje datasektor skapas varje timme.</span><span class="sxs-lookup"><span data-stu-id="c03af-303">In the preceding example, there are 24 data slices as each data slice is produced hourly.</span></span>

    <span data-ttu-id="c03af-304">Beskrivningar av JSON-egenskaper i en pipeline-definition finns i artikeln [skapa pipelines](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="c03af-304">For descriptions of JSON properties in a pipeline definition, see [create pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="c03af-305">Beskrivningar av JSON-egenskaper i en kopieringsaktivitet-definition finns i artikeln [aktiviteter för dataflyttning](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="c03af-305">For descriptions of JSON properties in a copy activity definition, see [data movement activities](data-factory-data-movement-activities.md).</span></span> <span data-ttu-id="c03af-306">Beskrivningar av JSON-egenskaper som stöds av BlobSource finns i artikeln [Azure Blob-anslutningsapp](data-factory-azure-blob-connector.md).</span><span class="sxs-lookup"><span data-stu-id="c03af-306">For descriptions of JSON properties supported by BlobSource, see [Azure Blob connector article](data-factory-azure-blob-connector.md).</span></span> <span data-ttu-id="c03af-307">Beskrivningar av JSON-egenskaper som stöds av SqlSink finns i artikeln [Azure SQL Database-anslutningsapp](data-factory-azure-sql-connector.md).</span><span class="sxs-lookup"><span data-stu-id="c03af-307">For descriptions of JSON properties supported by SqlSink, see [Azure SQL Database connector article](data-factory-azure-sql-connector.md).</span></span>
2. <span data-ttu-id="c03af-308">Kör följande kommando för att skapa datafabrikstabellen.</span><span class="sxs-lookup"><span data-stu-id="c03af-308">Run the following command to create the data factory table.</span></span>

    ```PowerShell   
    New-AzureRmDataFactoryPipeline $df -File .\ADFTutorialPipeline.json
    ```

    <span data-ttu-id="c03af-309">Här är exempel på utdata:</span><span class="sxs-lookup"><span data-stu-id="c03af-309">Here is the sample output:</span></span> 

    ```
    PipelineName      : ADFTutorialPipeline
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    Properties        : Microsoft.Azure.Management.DataFactories.Models.PipelinePropertie
    ProvisioningState : Succeeded
    ```

<span data-ttu-id="c03af-310">**Grattis!**</span><span class="sxs-lookup"><span data-stu-id="c03af-310">**Congratulations!**</span></span> <span data-ttu-id="c03af-311">Du har skapat en Azure-datafabrik med en pipeline för att kopiera data från en Azure blob-lagring till en Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="c03af-311">You have successfully created an Azure data factory with a pipeline to copy data from an Azure blob storage to an Azure SQL database.</span></span> 

## <a name="monitor-the-pipeline"></a><span data-ttu-id="c03af-312">Övervaka pipeline</span><span class="sxs-lookup"><span data-stu-id="c03af-312">Monitor the pipeline</span></span>
<span data-ttu-id="c03af-313">I det här steget använder du Azure PowerShell till att övervaka vad som händer i en Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="c03af-313">In this step, you use Azure PowerShell to monitor what’s going on in an Azure data factory.</span></span>

1. <span data-ttu-id="c03af-314">Ersätt &lt;DataFactoryName&gt; med namnet på din datafabrik och kör **Get-AzureRmDataFactory**. Tilldela en utdatan till en $df-variabel.</span><span class="sxs-lookup"><span data-stu-id="c03af-314">Replace &lt;DataFactoryName&gt; with the name of your data factory and run **Get-AzureRmDataFactory**, and assign the output to a variable $df.</span></span>

    ```PowerShell  
    $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name <DataFactoryName>
    ```

    <span data-ttu-id="c03af-315">Exempel:</span><span class="sxs-lookup"><span data-stu-id="c03af-315">For example:</span></span>
    ```PowerShell
    $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name ADFTutorialDataFactoryPSH0516
    ```
    
    <span data-ttu-id="c03af-316">Skriv sedan ut innehållet i $df för att se följande utdata:</span><span class="sxs-lookup"><span data-stu-id="c03af-316">Then, run print the contents of $df to see the following output:</span></span> 
    
    ```
    PS C:\ADFGetStartedPSH> $df
    
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    DataFactoryId     : 6f194b34-03b3-49ab-8f03-9f8a7b9d3e30
    ResourceGroupName : ADFTutorialResourceGroup
    Location          : West US
    Tags              : {}
    Properties        : Microsoft.Azure.Management.DataFactories.Models.DataFactoryProperties
    ProvisioningState : Succeeded
    ```
2. <span data-ttu-id="c03af-317">Kör **Get-AzureRmDataFactorySlice** att få information om alla sektorer av **OutputDataset**, vilket är utdatan för pipelinen.</span><span class="sxs-lookup"><span data-stu-id="c03af-317">Run **Get-AzureRmDataFactorySlice** to get details about all slices of the **OutputDataset**, which is the output dataset of the pipeline.</span></span>  

    ```PowerShell   
    Get-AzureRmDataFactorySlice $df -DatasetName OutputDataset -StartDateTime 2017-05-11T00:00:00Z
    ```

   <span data-ttu-id="c03af-318">Den här inställningen måste matcha **Start**-värdet i pipelinens JSON.</span><span class="sxs-lookup"><span data-stu-id="c03af-318">This setting should match the **Start** value in the pipeline JSON.</span></span> <span data-ttu-id="c03af-319">Du bör se 24 sektorer, en för varje timme från kl. 12:00 den aktuella dagen till 12:00 nästa dag.</span><span class="sxs-lookup"><span data-stu-id="c03af-319">You should see 24 slices, one for each hour from 12 AM of the current day to 12 AM of the next day.</span></span>

   <span data-ttu-id="c03af-320">Här följer tre exempelsegment från utdata:</span><span class="sxs-lookup"><span data-stu-id="c03af-320">Here are three sample slices from the output:</span></span> 

    ``` 
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    DatasetName       : OutputDataset
    Start             : 5/11/2017 11:00:00 PM
    End               : 5/12/2017 12:00:00 AM
    RetryCount        : 0
    State             : Ready
    SubState          :
    LatencyStatus     :
    LongRetryCount    : 0

    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    DatasetName       : OutputDataset
    Start             : 5/11/2017 9:00:00 PM
    End               : 5/11/2017 10:00:00 PM
    RetryCount        : 0
    State             : InProgress
    SubState          :
    LatencyStatus     :
    LongRetryCount    : 0   

    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    DatasetName       : OutputDataset
    Start             : 5/11/2017 8:00:00 PM
    End               : 5/11/2017 9:00:00 PM
    RetryCount        : 0
    State             : Waiting
    SubState          : ConcurrencyLimit
    LatencyStatus     :
    LongRetryCount    : 0
    ```
3. <span data-ttu-id="c03af-321">Kör **Get-AzureRmDataFactoryRun** för att hämta information om aktiviteten som körs för en **viss** sektor.</span><span class="sxs-lookup"><span data-stu-id="c03af-321">Run **Get-AzureRmDataFactoryRun** to get the details of activity runs for a **specific** slice.</span></span> <span data-ttu-id="c03af-322">Kopiera datum-/tid-värde från utdata från det föregående kommandot för att ange värdet för parametern StartDateTime.</span><span class="sxs-lookup"><span data-stu-id="c03af-322">Copy the date-time value from the output of the previous command to specify the value for the StartDateTime parameter.</span></span> 

    ```PowerShell  
    Get-AzureRmDataFactoryRun $df -DatasetName OutputDataset -StartDateTime "5/11/2017 09:00:00 PM"
    ```

   <span data-ttu-id="c03af-323">Här är exempel på utdata:</span><span class="sxs-lookup"><span data-stu-id="c03af-323">Here is the sample output:</span></span> 

    ```
    Id                  : c0ddbd75-d0c7-4816-a775-704bbd7c7eab_636301332000000000_636301368000000000_OutputDataset
    ResourceGroupName   : ADFTutorialResourceGroup
    DataFactoryName     : ADFTutorialDataFactoryPSH0516
    DatasetName         : OutputDataset
    ProcessingStartTime : 5/16/2017 8:00:33 PM
    ProcessingEndTime   : 5/16/2017 8:01:36 PM
    PercentComplete     : 100
    DataSliceStart      : 5/11/2017 9:00:00 PM
    DataSliceEnd        : 5/11/2017 10:00:00 PM
    Status              : Succeeded
    Timestamp           : 5/16/2017 8:00:33 PM
    RetryAttempt        : 0
    Properties          : {}
    ErrorMessage        :
    ActivityName        : CopyFromBlobToSQL
    PipelineName        : ADFTutorialPipeline
    Type                : Copy  
    ```

<span data-ttu-id="c03af-324">Se [Cmdlet-referens för Data Factory](/powershell/module/azurerm.datafactories) för omfattande dokumentation om Data Factory-cmdletar.</span><span class="sxs-lookup"><span data-stu-id="c03af-324">For comprehensive documentation on Data Factory cmdlets, see [Data Factory Cmdlet Reference](/powershell/module/azurerm.datafactories).</span></span>

## <a name="summary"></a><span data-ttu-id="c03af-325">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="c03af-325">Summary</span></span>
<span data-ttu-id="c03af-326">I den här självstudien har du skapat en Azure-datafabrik som kopierar data från en Azure-blobb till en Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="c03af-326">In this tutorial, you created an Azure data factory to copy data from an Azure blob to an Azure SQL database.</span></span> <span data-ttu-id="c03af-327">Du använde PowerShell till att skapa datafabriken, länkade tjänster, datauppsättningar och en pipeline.</span><span class="sxs-lookup"><span data-stu-id="c03af-327">You used PowerShell to create the data factory, linked services, datasets, and a pipeline.</span></span> <span data-ttu-id="c03af-328">Här är de avancerade steg som du utförde i självstudien:</span><span class="sxs-lookup"><span data-stu-id="c03af-328">Here are the high-level steps you performed in this tutorial:</span></span>  

1. <span data-ttu-id="c03af-329">Du skapade en Azure **Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="c03af-329">Created an Azure **data factory**.</span></span>
2. <span data-ttu-id="c03af-330">Du skapade **länkade tjänster**:</span><span class="sxs-lookup"><span data-stu-id="c03af-330">Created **linked services**:</span></span>

   <span data-ttu-id="c03af-331">a.</span><span class="sxs-lookup"><span data-stu-id="c03af-331">a.</span></span> <span data-ttu-id="c03af-332">En länkad **Azure Storage**-tjänst som länkar Azure-lagringskontot som innehåller indata.</span><span class="sxs-lookup"><span data-stu-id="c03af-332">An **Azure Storage** linked service to link your Azure storage account that holds input data.</span></span>     
   <span data-ttu-id="c03af-333">b.</span><span class="sxs-lookup"><span data-stu-id="c03af-333">b.</span></span> <span data-ttu-id="c03af-334">En länkad **Azure SQL**-tjänst som länkar den SQL-databas som innehåller utdata.</span><span class="sxs-lookup"><span data-stu-id="c03af-334">An **Azure SQL** linked service to link your SQL database that holds the output data.</span></span>
3. <span data-ttu-id="c03af-335">Du skapade **datauppsättningar** som beskriver indata och utdata för pipelines.</span><span class="sxs-lookup"><span data-stu-id="c03af-335">Created **datasets** that describe input data and output data for pipelines.</span></span>
4. <span data-ttu-id="c03af-336">Du skapade en **pipeline** med **Kopiera aktivitet**, med **BlobSource** som källa och **SqlSink** som mottagare.</span><span class="sxs-lookup"><span data-stu-id="c03af-336">Created a **pipeline** with **Copy Activity**, with **BlobSource** as the source and **SqlSink** as the sink.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c03af-337">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c03af-337">Next steps</span></span>
<span data-ttu-id="c03af-338">I den här kursen används Azure blob storage som ett datalager för källa och en Azure SQL-databas som ett dataarkiv som mål i en kopieringsåtgärd.</span><span class="sxs-lookup"><span data-stu-id="c03af-338">In this tutorial, you used Azure blob storage as a source data store and an Azure SQL database as a destination data store in a copy operation.</span></span> <span data-ttu-id="c03af-339">Följande tabell innehåller en lista över datalager som stöds som källor och mål av kopieringsaktiviteten:</span><span class="sxs-lookup"><span data-stu-id="c03af-339">The following table provides a list of data stores supported as sources and destinations by the copy activity:</span></span> 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

<span data-ttu-id="c03af-340">För mer information om hur du kopierar data till/från ett datalager klickar du på länken för datalagret i tabellen.</span><span class="sxs-lookup"><span data-stu-id="c03af-340">To learn about how to copy data to/from a data store, click the link for the data store in the table.</span></span> 

