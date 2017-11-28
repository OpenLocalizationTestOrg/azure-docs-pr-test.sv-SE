---
title: "Självstudier: Skapa en pipeline toomove data med hjälp av Azure PowerShell | Microsoft Docs"
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
ms.openlocfilehash: 21406d7dfaa0c555b2538fbb52839d761c140fc5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-create-a-data-factory-pipeline-that-moves-data-by-using-azure-powershell"></a><span data-ttu-id="a12ea-103">Självstudiekurs: Skapa en Data Factory-pipeline som flyttar data med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="a12ea-103">Tutorial: Create a Data Factory pipeline that moves data by using Azure PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="a12ea-104">Översikt och förutsättningar</span><span class="sxs-lookup"><span data-stu-id="a12ea-104">Overview and prerequisites</span></span>](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [<span data-ttu-id="a12ea-105">Guiden Kopiera</span><span class="sxs-lookup"><span data-stu-id="a12ea-105">Copy Wizard</span></span>](data-factory-copy-data-wizard-tutorial.md)
> * [<span data-ttu-id="a12ea-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="a12ea-106">Azure portal</span></span>](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [<span data-ttu-id="a12ea-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a12ea-107">Visual Studio</span></span>](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [<span data-ttu-id="a12ea-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="a12ea-108">PowerShell</span></span>](data-factory-copy-activity-tutorial-using-powershell.md)
> * [<span data-ttu-id="a12ea-109">Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="a12ea-109">Azure Resource Manager template</span></span>](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [<span data-ttu-id="a12ea-110">REST API</span><span class="sxs-lookup"><span data-stu-id="a12ea-110">REST API</span></span>](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [<span data-ttu-id="a12ea-111">.NET-API</span><span class="sxs-lookup"><span data-stu-id="a12ea-111">.NET API</span></span>](data-factory-copy-activity-tutorial-using-dotnet-api.md)
>
>

<span data-ttu-id="a12ea-112">I den här artikeln får du lära dig hur toouse PowerShell toocreate en datafabrik med en rörledning som kopierar data från Azure blob storage tooan Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="a12ea-112">In this article, you learn how toouse PowerShell toocreate a data factory with a pipeline that copies data from an Azure blob storage tooan Azure SQL database.</span></span> <span data-ttu-id="a12ea-113">Om du är ny tooAzure Data Factory, Läs igenom hello [introduktion tooAzure Data Factory](data-factory-introduction.md) artikel innan du utför den här kursen.</span><span class="sxs-lookup"><span data-stu-id="a12ea-113">If you are new tooAzure Data Factory, read through hello [Introduction tooAzure Data Factory](data-factory-introduction.md) article before doing this tutorial.</span></span>   

<span data-ttu-id="a12ea-114">I den här självstudien får du skapa en pipeline i en aktivitet: kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="a12ea-114">In this tutorial, you create a pipeline with one activity in it: Copy Activity.</span></span> <span data-ttu-id="a12ea-115">Hej kopieringsaktiviteten kopierar data från ett datalager för data som stöds store tooa stöds mottagare.</span><span class="sxs-lookup"><span data-stu-id="a12ea-115">hello copy activity copies data from a supported data store tooa supported sink data store.</span></span> <span data-ttu-id="a12ea-116">En lista över datakällor som stöds som källor och mottagare finns i [datalager som stöds](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="a12ea-116">For a list of data stores supported as sources and sinks, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="a12ea-117">hello aktiviteten drivs av en globalt tillgänglig tjänst som kan kopiera data mellan olika datalager på ett säkert, tillförlitligt och skalbar sätt.</span><span class="sxs-lookup"><span data-stu-id="a12ea-117">hello activity is powered by a globally available service that can copy data between various data stores in a secure, reliable, and scalable way.</span></span> <span data-ttu-id="a12ea-118">Läs mer om hello Kopieringsaktiviteten [Data Movement aktiviteter](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="a12ea-118">For more information about hello Copy Activity, see [Data Movement Activities](data-factory-data-movement-activities.md).</span></span>

<span data-ttu-id="a12ea-119">En pipeline kan ha fler än en aktivitet.</span><span class="sxs-lookup"><span data-stu-id="a12ea-119">A pipeline can have more than one activity.</span></span> <span data-ttu-id="a12ea-120">Och du kan länka två aktiviteter (köra en aktivitet efter ett annat) genom att ange hello datamängd för utdata för en aktivitet som hello indatauppsättning av hello andra aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="a12ea-120">And, you can chain two activities (run one activity after another) by setting hello output dataset of one activity as hello input dataset of hello other activity.</span></span> <span data-ttu-id="a12ea-121">Mer information finns i [flera aktiviteter i en pipeline](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span><span class="sxs-lookup"><span data-stu-id="a12ea-121">For more information, see [multiple activities in a pipeline](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span></span>

> [!NOTE]
> <span data-ttu-id="a12ea-122">Den här artikeln täcker inte alla hello Data Factory-cmdletar.</span><span class="sxs-lookup"><span data-stu-id="a12ea-122">This article does not cover all hello Data Factory cmdlets.</span></span> <span data-ttu-id="a12ea-123">Se [Cmdlet-referens för Data Factory](/powershell/module/azurerm.datafactories) för omfattande dokumentation om dessa cmdletar.</span><span class="sxs-lookup"><span data-stu-id="a12ea-123">See [Data Factory Cmdlet Reference](/powershell/module/azurerm.datafactories) for comprehensive documentation on these cmdlets.</span></span>
> 
> <span data-ttu-id="a12ea-124">hello data pipeline i den här kursen kopierar data från en källa data store tooa målarkiv data.</span><span class="sxs-lookup"><span data-stu-id="a12ea-124">hello data pipeline in this tutorial copies data from a source data store tooa destination data store.</span></span> <span data-ttu-id="a12ea-125">En självstudiekurs om hur tootransform data med hjälp av Azure Data Factory finns [Självstudier: skapa en pipeline tootransform data med Hadoop-kluster](data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="a12ea-125">For a tutorial on how tootransform data using Azure Data Factory, see [Tutorial: Build a pipeline tootransform data using Hadoop cluster](data-factory-build-your-first-pipeline.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a12ea-126">Krav</span><span class="sxs-lookup"><span data-stu-id="a12ea-126">Prerequisites</span></span>
- <span data-ttu-id="a12ea-127">Slutföra förutsättningar som anges i hello [förutsättningar för självstudien](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="a12ea-127">Complete prerequisites listed in hello [tutorial prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) article.</span></span>
- <span data-ttu-id="a12ea-128">**Installera Azure PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="a12ea-128">Install **Azure PowerShell**.</span></span> <span data-ttu-id="a12ea-129">Följ anvisningarna för hello i [hur tooinstall och konfigurera Azure PowerShell](../powershell-install-configure.md).</span><span class="sxs-lookup"><span data-stu-id="a12ea-129">Follow hello instructions in [How tooinstall and configure Azure PowerShell](../powershell-install-configure.md).</span></span>

## <a name="steps"></a><span data-ttu-id="a12ea-130">Steg</span><span class="sxs-lookup"><span data-stu-id="a12ea-130">Steps</span></span>
<span data-ttu-id="a12ea-131">Här är hello steg du utför som en del av den här kursen:</span><span class="sxs-lookup"><span data-stu-id="a12ea-131">Here are hello steps you perform as part of this tutorial:</span></span>

1. <span data-ttu-id="a12ea-132">Skapa en Azure-**datafabrik**.</span><span class="sxs-lookup"><span data-stu-id="a12ea-132">Create an Azure **data factory**.</span></span> <span data-ttu-id="a12ea-133">I det här steget skapar du en datafabrik med namnet ADFTutorialDataFactoryPSH.</span><span class="sxs-lookup"><span data-stu-id="a12ea-133">In this step, you create a data factory named ADFTutorialDataFactoryPSH.</span></span> 
2. <span data-ttu-id="a12ea-134">Skapa **länkade tjänster** i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="a12ea-134">Create **linked services** in hello data factory.</span></span> <span data-ttu-id="a12ea-135">I det här steget kan du skapa två länkade tjänster: Azure Storage och Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="a12ea-135">In this step, you create two linked services of types: Azure Storage and Azure SQL Database.</span></span> 
    
    <span data-ttu-id="a12ea-136">Hej AzureStorageLinkedService länkar din Azure storage-konto toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="a12ea-136">hello AzureStorageLinkedService links your Azure storage account toohello data factory.</span></span> <span data-ttu-id="a12ea-137">Du har skapat en behållare och överföra data toothis storage-konto som en del av [krav](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="a12ea-137">You created a container and uploaded data toothis storage account as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>   

    <span data-ttu-id="a12ea-138">AzureSqlLinkedService länkar din Azure SQL database toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="a12ea-138">AzureSqlLinkedService links your Azure SQL database toohello data factory.</span></span> <span data-ttu-id="a12ea-139">hello-data som kopieras från hello blob storage lagras i den här databasen.</span><span class="sxs-lookup"><span data-stu-id="a12ea-139">hello data that is copied from hello blob storage is stored in this database.</span></span> <span data-ttu-id="a12ea-140">Du har skapat den SQL-tabellen i den här databasen som en del av [förhandskraven](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="a12ea-140">You created a SQL table in this database as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>   
3. <span data-ttu-id="a12ea-141">Skapa ingående och utgående **datauppsättningar** i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="a12ea-141">Create input and output **datasets** in hello data factory.</span></span>  
    
    <span data-ttu-id="a12ea-142">hello länkad Azure storage-tjänst anger hello anslutningssträngen som Data Factory-tjänsten använder vid körning tooconnect tooyour Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="a12ea-142">hello Azure storage linked service specifies hello connection string that Data Factory service uses at run time tooconnect tooyour Azure storage account.</span></span> <span data-ttu-id="a12ea-143">Och hello inkommande blobbdatauppsättning anger hello-behållaren och hello-mapp som innehåller hello indata.</span><span class="sxs-lookup"><span data-stu-id="a12ea-143">And, hello input blob dataset specifies hello container and hello folder that contains hello input data.</span></span>  

    <span data-ttu-id="a12ea-144">På samma sätt anger hello Azure SQL Database länkad tjänst hello anslutningssträngen som Data Factory-tjänsten använder vid körning tooconnect tooyour Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="a12ea-144">Similarly, hello Azure SQL Database linked service specifies hello connection string that Data Factory service uses at run time tooconnect tooyour Azure SQL database.</span></span> <span data-ttu-id="a12ea-145">Och hello utdatauppsättningen SQL tabellen anger hello tabellen i hello toowhich hello databasdata från hello blob storage kopieras.</span><span class="sxs-lookup"><span data-stu-id="a12ea-145">And, hello output SQL table dataset specifies hello table in hello database toowhich hello data from hello blob storage is copied.</span></span>
4. <span data-ttu-id="a12ea-146">Skapa en **pipeline** i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="a12ea-146">Create a **pipeline** in hello data factory.</span></span> <span data-ttu-id="a12ea-147">I det här steget kan du skapa en pipeline med en kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="a12ea-147">In this step, you create a pipeline with a copy activity.</span></span>   
    
    <span data-ttu-id="a12ea-148">Hej kopieringsaktiviteten kopierar data från en blobb i hello Azure blob storage tooa tabellen i hello Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="a12ea-148">hello copy activity copies data from a blob in hello Azure blob storage tooa table in hello Azure SQL database.</span></span> <span data-ttu-id="a12ea-149">Du kan använda en kopia aktivitet i en pipeline toocopy data från alla stöds källan tooany stöds målet.</span><span class="sxs-lookup"><span data-stu-id="a12ea-149">You can use a copy activity in a pipeline toocopy data from any supported source tooany supported destination.</span></span> <span data-ttu-id="a12ea-150">I avsnittet [Dataförflyttningsaktiviteter](data-factory-data-movement-activities.md#supported-data-stores-and-formats) finns en lista över datalager som stöds.</span><span class="sxs-lookup"><span data-stu-id="a12ea-150">For a list of supported data stores, see [data movement activities](data-factory-data-movement-activities.md#supported-data-stores-and-formats) article.</span></span> 
5. <span data-ttu-id="a12ea-151">Övervakaren hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="a12ea-151">Monitor hello pipeline.</span></span> <span data-ttu-id="a12ea-152">I det här steget du **övervakaren** hello segment av inkommande och utgående datamängder med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a12ea-152">In this step, you **monitor** hello slices of input and output datasets by using PowerShell.</span></span>

## <a name="create-a-data-factory"></a><span data-ttu-id="a12ea-153">Skapa en datafabrik</span><span class="sxs-lookup"><span data-stu-id="a12ea-153">Create a data factory</span></span>
> [!IMPORTANT]
> <span data-ttu-id="a12ea-154">Fullständig [förutsättningar för självstudiekursen hello](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) om du inte redan gjort det..</span><span class="sxs-lookup"><span data-stu-id="a12ea-154">Complete [prerequisites for hello tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) if you haven't already done so.</span></span>   

<span data-ttu-id="a12ea-155">En datafabrik kan ha en eller flera pipelines.</span><span class="sxs-lookup"><span data-stu-id="a12ea-155">A data factory can have one or more pipelines.</span></span> <span data-ttu-id="a12ea-156">En pipeline kan innehålla en eller flera aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="a12ea-156">A pipeline can have one or more activities in it.</span></span> <span data-ttu-id="a12ea-157">Till exempel ange en Kopieringsaktiviteten toocopy data från ett dataarkiv som källa tooa mål och en HDInsight Hive aktiviteten toorun tootransform en Hive-skript data tooproduct utdata.</span><span class="sxs-lookup"><span data-stu-id="a12ea-157">For example, a Copy Activity toocopy data from a source tooa destination data store and a HDInsight Hive activity toorun a Hive script tootransform input data tooproduct output data.</span></span> <span data-ttu-id="a12ea-158">Låt oss börja med att skapa hello data factory i det här steget.</span><span class="sxs-lookup"><span data-stu-id="a12ea-158">Let's start with creating hello data factory in this step.</span></span>

1. <span data-ttu-id="a12ea-159">Starta **PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="a12ea-159">Launch **PowerShell**.</span></span> <span data-ttu-id="a12ea-160">Behåll Azure PowerShell öppen tills hello slutet av den här kursen.</span><span class="sxs-lookup"><span data-stu-id="a12ea-160">Keep Azure PowerShell open until hello end of this tutorial.</span></span> <span data-ttu-id="a12ea-161">Om du stänga och öppna måste toorun hello kommandon igen.</span><span class="sxs-lookup"><span data-stu-id="a12ea-161">If you close and reopen, you need toorun hello commands again.</span></span>

    <span data-ttu-id="a12ea-162">Kör följande kommando hello och ange hello användarnamn och lösenord som du använder toosign i toohello Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="a12ea-162">Run hello following command, and enter hello user name and password that you use toosign in toohello Azure portal:</span></span>

    ```PowerShell
    Login-AzureRmAccount
    ```   
   
    <span data-ttu-id="a12ea-163">Kör följande kommando tooview hello alla hello prenumerationer för kontot:</span><span class="sxs-lookup"><span data-stu-id="a12ea-163">Run hello following command tooview all hello subscriptions for this account:</span></span>

    ```PowerShell
    Get-AzureRmSubscription
    ```

    <span data-ttu-id="a12ea-164">Hello kör följande kommando tooselect hello prenumeration som du vill toowork med.</span><span class="sxs-lookup"><span data-stu-id="a12ea-164">Run hello following command tooselect hello subscription that you want toowork with.</span></span> <span data-ttu-id="a12ea-165">Ersätt  **&lt;NameOfAzureSubscription** &gt; med hello namnet på din Azure-prenumeration:</span><span class="sxs-lookup"><span data-stu-id="a12ea-165">Replace **&lt;NameOfAzureSubscription**&gt; with hello name of your Azure subscription:</span></span>

    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext
    ```
2. <span data-ttu-id="a12ea-166">Skapa en Azure-resursgrupp med namnet **ADFTutorialResourceGroup** genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="a12ea-166">Create an Azure resource group named **ADFTutorialResourceGroup** by running hello following command:</span></span>

    ```PowerShell
    New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```
    
    <span data-ttu-id="a12ea-167">Några av hello stegen i den här självstudiekursen förutsätter att du använder hello resursgrupp med namnet **ADFTutorialResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="a12ea-167">Some of hello steps in this tutorial assume that you use hello resource group named **ADFTutorialResourceGroup**.</span></span> <span data-ttu-id="a12ea-168">Om du använder en annan resursgrupp måste toouse den i stället för ADFTutorialResourceGroup i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="a12ea-168">If you use a different resource group, you need toouse it in place of ADFTutorialResourceGroup in this tutorial.</span></span>
3. <span data-ttu-id="a12ea-169">Kör hello **ny AzureRmDataFactory** cmdlet toocreate en datafabrik som heter **ADFTutorialDataFactoryPSH**:</span><span class="sxs-lookup"><span data-stu-id="a12ea-169">Run hello **New-AzureRmDataFactory** cmdlet toocreate a data factory named **ADFTutorialDataFactoryPSH**:</span></span>  

    ```PowerShell
    $df=New-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name ADFTutorialDataFactoryPSH –Location "West US"
    ```
    <span data-ttu-id="a12ea-170">Det här namnet har kanske redan tagits.</span><span class="sxs-lookup"><span data-stu-id="a12ea-170">This name may already have been taken.</span></span> <span data-ttu-id="a12ea-171">Därför att hello namn i hello data factory unika genom att lägga till ett prefix eller suffix (till exempel: ADFTutorialDataFactoryPSH05152017) och kör hello kommandot igen.</span><span class="sxs-lookup"><span data-stu-id="a12ea-171">Therefore, make hello name of hello data factory unique by adding a prefix or suffix (for example: ADFTutorialDataFactoryPSH05152017) and run hello command again.</span></span>  

<span data-ttu-id="a12ea-172">Observera följande punkter hello:</span><span class="sxs-lookup"><span data-stu-id="a12ea-172">Note hello following points:</span></span>

* <span data-ttu-id="a12ea-173">hello namn i hello Azure data factory måste vara globalt unika.</span><span class="sxs-lookup"><span data-stu-id="a12ea-173">hello name of hello Azure data factory must be globally unique.</span></span> <span data-ttu-id="a12ea-174">Om du får följande fel hello ändra hello namn (till exempel yournameADFTutorialDataFactoryPSH).</span><span class="sxs-lookup"><span data-stu-id="a12ea-174">If you receive hello following error, change hello name (for example, yournameADFTutorialDataFactoryPSH).</span></span> <span data-ttu-id="a12ea-175">Använd det här namnet i stället för ADFTutorialFactoryPSH när du utför stegen i självstudien.</span><span class="sxs-lookup"><span data-stu-id="a12ea-175">Use this name in place of ADFTutorialFactoryPSH while performing steps in this tutorial.</span></span> <span data-ttu-id="a12ea-176">Se artikeln [Data Factory – namnregler](data-factory-naming-rules.md) för information om Data Factory-artefakter.</span><span class="sxs-lookup"><span data-stu-id="a12ea-176">See [Data Factory - Naming Rules](data-factory-naming-rules.md) for Data Factory artifacts.</span></span>

    ```
    Data factory name “ADFTutorialDataFactoryPSH” is not available
    ```
* <span data-ttu-id="a12ea-177">toocreate Data Factory instanser måste du vara en deltagare eller en administratör för hello Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="a12ea-177">toocreate Data Factory instances, you must be a contributor or administrator of hello Azure subscription.</span></span>
* <span data-ttu-id="a12ea-178">hello namnet på hello data factory kan registreras som en DNS-namnet i hello framtida och därför bli synligt offentligt.</span><span class="sxs-lookup"><span data-stu-id="a12ea-178">hello name of hello data factory may be registered as a DNS name in hello future, and hence become publicly visible.</span></span>
* <span data-ttu-id="a12ea-179">Du kan få hello följande fel ”:**den här prenumerationen är inte registrerad toouse namnområde Microsoft.DataFactory.**”</span><span class="sxs-lookup"><span data-stu-id="a12ea-179">You may receive hello following error: "**This subscription is not registered toouse namespace Microsoft.DataFactory.**"</span></span> <span data-ttu-id="a12ea-180">Gör något av följande hello och försök att publicera igen:</span><span class="sxs-lookup"><span data-stu-id="a12ea-180">Do one of hello following, and try publishing again:</span></span>

  * <span data-ttu-id="a12ea-181">Kör hello efter kommandot tooregister hello Data Factory-providern i Azure PowerShell:</span><span class="sxs-lookup"><span data-stu-id="a12ea-181">In Azure PowerShell, run hello following command tooregister hello Data Factory provider:</span></span>

    ```PowerShell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```

    <span data-ttu-id="a12ea-182">Kör följande kommando tooconfirm hello som hello Data Factory providern är registrerad:</span><span class="sxs-lookup"><span data-stu-id="a12ea-182">Run hello following command tooconfirm that hello Data Factory provider is registered:</span></span>

    ```PowerShell
    Get-AzureRmResourceProvider
    ```
  * <span data-ttu-id="a12ea-183">Logga in med hjälp av hello Azure-prenumeration toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="a12ea-183">Sign in by using hello Azure subscription toohello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="a12ea-184">Gå tooa Data Factory-bladet, eller skapa en datafabrik i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a12ea-184">Go tooa Data Factory blade, or create a data factory in hello Azure portal.</span></span> <span data-ttu-id="a12ea-185">Den här åtgärden registrerar automatiskt hello-providern för dig.</span><span class="sxs-lookup"><span data-stu-id="a12ea-185">This action automatically registers hello provider for you.</span></span>

## <a name="create-linked-services"></a><span data-ttu-id="a12ea-186">Skapa länkade tjänster</span><span class="sxs-lookup"><span data-stu-id="a12ea-186">Create linked services</span></span>
<span data-ttu-id="a12ea-187">Du kan skapa länkade tjänster i en data factory toolink dina data lagras och beräkna services toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="a12ea-187">You create linked services in a data factory toolink your data stores and compute services toohello data factory.</span></span> <span data-ttu-id="a12ea-188">I den här självstudiekursen kommer använder du inte någon beräkningstjänst, till exempel Azure HDInsight eller Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="a12ea-188">In this tutorial, you don't use any compute service such as Azure HDInsight or Azure Data Lake Analytics.</span></span> <span data-ttu-id="a12ea-189">Du använder två datalager av typen Azure Storage (källa) och Azure SQL Database (mål).</span><span class="sxs-lookup"><span data-stu-id="a12ea-189">You use two data stores of type Azure Storage (source) and Azure SQL Database (destination).</span></span> 

<span data-ttu-id="a12ea-190">Därför kan du skapa två länkade tjänster som heter AzureStorageLinkedService och AzureSqlLinkedService av typerna: AzureStorage och AzureSqlDatabase.</span><span class="sxs-lookup"><span data-stu-id="a12ea-190">Therefore, you create two linked services named AzureStorageLinkedService and AzureSqlLinkedService of types: AzureStorage and AzureSqlDatabase.</span></span>  

<span data-ttu-id="a12ea-191">Hej AzureStorageLinkedService länkar din Azure storage-konto toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="a12ea-191">hello AzureStorageLinkedService links your Azure storage account toohello data factory.</span></span> <span data-ttu-id="a12ea-192">Det här lagringskontot är hello något som du skapade en behållare och överföra hello data som en del av [krav](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="a12ea-192">This storage account is hello one in which you created a container and uploaded hello data as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>   

<span data-ttu-id="a12ea-193">AzureSqlLinkedService länkar din Azure SQL database toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="a12ea-193">AzureSqlLinkedService links your Azure SQL database toohello data factory.</span></span> <span data-ttu-id="a12ea-194">hello-data som kopieras från hello blob storage lagras i den här databasen.</span><span class="sxs-lookup"><span data-stu-id="a12ea-194">hello data that is copied from hello blob storage is stored in this database.</span></span> <span data-ttu-id="a12ea-195">Du har skapat hello tomma tabellen i den här databasen som en del av [krav](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="a12ea-195">You created hello emp table in this database as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span> 

### <a name="create-a-linked-service-for-an-azure-storage-account"></a><span data-ttu-id="a12ea-196">Skapa en länkad tjänst för ett Azure-lagringskonto</span><span class="sxs-lookup"><span data-stu-id="a12ea-196">Create a linked service for an Azure storage account</span></span>
<span data-ttu-id="a12ea-197">I det här steget kan länka du din Azure storage-konto tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="a12ea-197">In this step, you link your Azure storage account tooyour data factory.</span></span>

1. <span data-ttu-id="a12ea-198">Skapa en JSON-fil med namnet **AzureStorageLinkedService.json** i **C:\ADFGetStartedPSH** mapp med hello följande innehåll: (skapa hello mapp ADFGetStartedPSH om den inte redan finns).</span><span class="sxs-lookup"><span data-stu-id="a12ea-198">Create a JSON file named **AzureStorageLinkedService.json** in **C:\ADFGetStartedPSH** folder with hello following content: (Create hello folder ADFGetStartedPSH if it does not already exist.)</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="a12ea-199">Ersätt &lt;accountname&gt; och &lt;accountkey&gt; med namnet och nyckeln för ditt Azure storage-konto innan du sparar hello-filen.</span><span class="sxs-lookup"><span data-stu-id="a12ea-199">Replace &lt;accountname&gt; and &lt;accountkey&gt; with name and key of your Azure storage account before saving hello file.</span></span> 

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
2. <span data-ttu-id="a12ea-200">I **Azure PowerShell**, växla toohello **ADFGetStartedPSH** mapp.</span><span class="sxs-lookup"><span data-stu-id="a12ea-200">In **Azure PowerShell**, switch toohello **ADFGetStartedPSH** folder.</span></span>
4. <span data-ttu-id="a12ea-201">Kör hello **ny AzureRmDataFactoryLinkedService** cmdlet toocreate hello länkade tjänsten: **AzureStorageLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="a12ea-201">Run hello **New-AzureRmDataFactoryLinkedService** cmdlet toocreate hello linked service: **AzureStorageLinkedService**.</span></span> <span data-ttu-id="a12ea-202">Denna cmdlet och andra Data Factory-cmdletar som du använder i den här kursen kräver att du toopass värden för hello **ResourceGroupName** och **DataFactoryName** parametrar.</span><span class="sxs-lookup"><span data-stu-id="a12ea-202">This cmdlet, and other Data Factory cmdlets you use in this tutorial requires you toopass values for hello **ResourceGroupName** and **DataFactoryName** parameters.</span></span> <span data-ttu-id="a12ea-203">Du kan också skicka hello DataFactory objektet som returnerades av hello ny AzureRmDataFactory cmdlet utan att ange ResourceGroupName och DataFactoryName varje gång du kör en cmdlet.</span><span class="sxs-lookup"><span data-stu-id="a12ea-203">Alternatively, you can pass hello DataFactory object returned by hello New-AzureRmDataFactory cmdlet without typing ResourceGroupName and DataFactoryName each time you run a cmdlet.</span></span> 

    ```PowerShell
    New-AzureRmDataFactoryLinkedService $df -File .\AzureStorageLinkedService.json
    ```
    <span data-ttu-id="a12ea-204">Här är exempel på utdata från hello:</span><span class="sxs-lookup"><span data-stu-id="a12ea-204">Here is hello sample output:</span></span>

    ```
    LinkedServiceName : AzureStorageLinkedService
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    Properties        : Microsoft.Azure.Management.DataFactories.Models.LinkedServiceProperties
    ProvisioningState : Succeeded
    ``` 

    <span data-ttu-id="a12ea-205">Andra sätt att skapa den här länkade tjänsten är toospecify resursgruppens namn och datafabriksnamnet istället för att ange hello DataFactory objekt.</span><span class="sxs-lookup"><span data-stu-id="a12ea-205">Other way of creating this linked service is toospecify resource group name and data factory name instead of specifying hello DataFactory object.</span></span>  

    ```PowerShell
    New-AzureRmDataFactoryLinkedService -ResourceGroupName ADFTutorialResourceGroup -DataFactoryName <Name of your data factory> -File .\AzureStorageLinkedService.json
    ```

### <a name="create-a-linked-service-for-an-azure-sql-database"></a><span data-ttu-id="a12ea-206">Skapa en länkad tjänst för en Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="a12ea-206">Create a linked service for an Azure SQL database</span></span>
<span data-ttu-id="a12ea-207">I det här steget kan länka du din Azure SQL database tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="a12ea-207">In this step, you link your Azure SQL database tooyour data factory.</span></span>

1. <span data-ttu-id="a12ea-208">Skapa en JSON-fil som heter AzureSqlLinkedService.json i C:\ADFGetStartedPSH mappen med hello följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="a12ea-208">Create a JSON file named AzureSqlLinkedService.json in C:\ADFGetStartedPSH folder with hello following content:</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="a12ea-209">Ersätt &lt;servername&gt;, &lt;databasename&gt;, &lt;username@servername&gt; och &lt;password&gt; med namnen för Azure SQL-servern, databasen, användarkontot och lösenordet.</span><span class="sxs-lookup"><span data-stu-id="a12ea-209">Replace &lt;servername&gt;, &lt;databasename&gt;, &lt;username@servername&gt;, and &lt;password&gt; with names of your Azure SQL server, database, user account, and password.</span></span>
    
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
2. <span data-ttu-id="a12ea-210">Kör följande kommando toocreate hello en länkad tjänst:</span><span class="sxs-lookup"><span data-stu-id="a12ea-210">Run hello following command toocreate a linked service:</span></span>

    ```PowerShell
    New-AzureRmDataFactoryLinkedService $df -File .\AzureSqlLinkedService.json
    ```
    
    <span data-ttu-id="a12ea-211">Här är exempel på utdata från hello:</span><span class="sxs-lookup"><span data-stu-id="a12ea-211">Here is hello sample output:</span></span>

    ```
    LinkedServiceName : AzureSqlLinkedService
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    Properties        : Microsoft.Azure.Management.DataFactories.Models.LinkedServiceProperties
    ProvisioningState : Succeeded
    ```

   <span data-ttu-id="a12ea-212">Bekräfta att **och ge åtkomst tooAzure tjänster** inställningen är aktiverad för SQL-databasservern.</span><span class="sxs-lookup"><span data-stu-id="a12ea-212">Confirm that **Allow access tooAzure services** setting is turned on for your SQL database server.</span></span> <span data-ttu-id="a12ea-213">tooverify och den aktiveras, hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a12ea-213">tooverify and turn it on, do hello following steps:</span></span>

    1. <span data-ttu-id="a12ea-214">Logga in toohello [Azure-portalen](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="a12ea-214">Log in toohello [Azure portal](https://portal.azure.com)</span></span>
    2. <span data-ttu-id="a12ea-215">Klicka på **fler tjänster >** hello vänster och klicka på **SQL-servrar** i hello **databaser** kategori.</span><span class="sxs-lookup"><span data-stu-id="a12ea-215">Click **More services >** on hello left, and click **SQL servers** in hello **DATABASES** category.</span></span>
    3. <span data-ttu-id="a12ea-216">Markera din server i hello lista över SQL-servrar.</span><span class="sxs-lookup"><span data-stu-id="a12ea-216">Select your server in hello list of SQL servers.</span></span>
    4. <span data-ttu-id="a12ea-217">Klicka på hello SQL server-bladet **visa brandväggsinställningar** länk.</span><span class="sxs-lookup"><span data-stu-id="a12ea-217">On hello SQL server blade, click **Show firewall settings** link.</span></span>
    5. <span data-ttu-id="a12ea-218">I hello **brandväggsinställningar** bladet, klickar du på **ON** för **och ge åtkomst tooAzure tjänster**.</span><span class="sxs-lookup"><span data-stu-id="a12ea-218">In hello **Firewall settings** blade, click **ON** for **Allow access tooAzure services**.</span></span>
    6. <span data-ttu-id="a12ea-219">Klicka på **spara** hello i verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="a12ea-219">Click **Save** on hello toolbar.</span></span> 

## <a name="create-datasets"></a><span data-ttu-id="a12ea-220">Skapa datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="a12ea-220">Create datasets</span></span>
<span data-ttu-id="a12ea-221">I föregående steg hello Skapa länkade tjänster toolink dina Azure Storage-konto och Azure SQL database tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="a12ea-221">In hello previous step, you created linked services toolink your Azure Storage account and Azure SQL database tooyour data factory.</span></span> <span data-ttu-id="a12ea-222">I det här steget definierar du två datamängder som heter InputDataset och OutputDataset som representerar indata och utdata som är lagrad i hello datalager som anges av AzureStorageLinkedService och AzureSqlLinkedService respektive.</span><span class="sxs-lookup"><span data-stu-id="a12ea-222">In this step, you define two datasets named InputDataset and OutputDataset that represent input and output data that is stored in hello data stores referred by AzureStorageLinkedService and AzureSqlLinkedService respectively.</span></span>

<span data-ttu-id="a12ea-223">hello länkad Azure storage-tjänst anger hello anslutningssträngen som Data Factory-tjänsten använder vid körning tooconnect tooyour Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="a12ea-223">hello Azure storage linked service specifies hello connection string that Data Factory service uses at run time tooconnect tooyour Azure storage account.</span></span> <span data-ttu-id="a12ea-224">Och hello inkommande blobbdatauppsättning (InputDataset) anger hello-behållaren och hello-mapp som innehåller hello indata.</span><span class="sxs-lookup"><span data-stu-id="a12ea-224">And, hello input blob dataset (InputDataset) specifies hello container and hello folder that contains hello input data.</span></span>  

<span data-ttu-id="a12ea-225">På samma sätt anger hello Azure SQL Database länkad tjänst hello anslutningssträngen som Data Factory-tjänsten använder vid körning tooconnect tooyour Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="a12ea-225">Similarly, hello Azure SQL Database linked service specifies hello connection string that Data Factory service uses at run time tooconnect tooyour Azure SQL database.</span></span> <span data-ttu-id="a12ea-226">Och hello utgående SQL tabell datauppsättningen (OututDataset) anger hello tabellen i hello databasen toowhich hello kopieras data från hello blob storage.</span><span class="sxs-lookup"><span data-stu-id="a12ea-226">And, hello output SQL table dataset (OututDataset) specifies hello table in hello database toowhich hello data from hello blob storage is copied.</span></span> 

### <a name="create-an-input-dataset"></a><span data-ttu-id="a12ea-227">Skapa en indatauppsättning</span><span class="sxs-lookup"><span data-stu-id="a12ea-227">Create an input dataset</span></span>
<span data-ttu-id="a12ea-228">I det här steget skapar du en datauppsättning med namnet InputDataset som pekar tooa blob-fil (emp.txt) i blob-behållaren (adftutorial) hello rotmapp i hello Azure Storage som representeras av hello AzureStorageLinkedService länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a12ea-228">In this step, you create a dataset named InputDataset that points tooa blob file (emp.txt) in hello root folder of a blob container (adftutorial) in hello Azure Storage represented by hello AzureStorageLinkedService linked service.</span></span> <span data-ttu-id="a12ea-229">Om du inte ange ett värde för hello filnamn (eller hoppa över den), är data från alla blobbar i hello inkommande mappen kopierade toohello mål.</span><span class="sxs-lookup"><span data-stu-id="a12ea-229">If you don't specify a value for hello fileName (or skip it), data from all blobs in hello input folder are copied toohello destination.</span></span> <span data-ttu-id="a12ea-230">I kursen får ange du ett värde för hello filnamnet.</span><span class="sxs-lookup"><span data-stu-id="a12ea-230">In this tutorial, you specify a value for hello fileName.</span></span>  

1. <span data-ttu-id="a12ea-231">Skapa en JSON-fil med namnet **InputDataset.json** i hello **C:\ADFGetStartedPSH** mapp med hello följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="a12ea-231">Create a JSON file named **InputDataset.json** in hello **C:\ADFGetStartedPSH** folder, with hello following content:</span></span>

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

    <span data-ttu-id="a12ea-232">hello innehåller följande tabell beskrivningar för hello JSON egenskaper som används i hello fragment:</span><span class="sxs-lookup"><span data-stu-id="a12ea-232">hello following table provides descriptions for hello JSON properties used in hello snippet:</span></span>

    | <span data-ttu-id="a12ea-233">Egenskap</span><span class="sxs-lookup"><span data-stu-id="a12ea-233">Property</span></span> | <span data-ttu-id="a12ea-234">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a12ea-234">Description</span></span> |
    |:--- |:--- |
    | <span data-ttu-id="a12ea-235">typ</span><span class="sxs-lookup"><span data-stu-id="a12ea-235">type</span></span> | <span data-ttu-id="a12ea-236">hello typegenskapen har ställts in för**AzureBlob** eftersom data finns i ett Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="a12ea-236">hello type property is set too**AzureBlob** because data resides in an Azure blob storage.</span></span> |
    | <span data-ttu-id="a12ea-237">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="a12ea-237">linkedServiceName</span></span> | <span data-ttu-id="a12ea-238">Refererar toohello **AzureStorageLinkedService** som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="a12ea-238">Refers toohello **AzureStorageLinkedService** that you created earlier.</span></span> |
    | <span data-ttu-id="a12ea-239">folderPath</span><span class="sxs-lookup"><span data-stu-id="a12ea-239">folderPath</span></span> | <span data-ttu-id="a12ea-240">Anger hello blob **behållare** och hello **mappen** som innehåller inkommande blobbar.</span><span class="sxs-lookup"><span data-stu-id="a12ea-240">Specifies hello blob **container** and hello **folder** that contains input blobs.</span></span> <span data-ttu-id="a12ea-241">I den här självstudiekursen adftutorial är hello blob-behållaren och mappen är hello rotmappen.</span><span class="sxs-lookup"><span data-stu-id="a12ea-241">In this tutorial, adftutorial is hello blob container and folder is hello root folder.</span></span> | 
    | <span data-ttu-id="a12ea-242">fileName</span><span class="sxs-lookup"><span data-stu-id="a12ea-242">fileName</span></span> | <span data-ttu-id="a12ea-243">Den här egenskapen är valfri.</span><span class="sxs-lookup"><span data-stu-id="a12ea-243">This property is optional.</span></span> <span data-ttu-id="a12ea-244">Om du utesluter den här egenskapen har alla filer från hello folderPath plockats.</span><span class="sxs-lookup"><span data-stu-id="a12ea-244">If you omit this property, all files from hello folderPath are picked.</span></span> <span data-ttu-id="a12ea-245">I den här självstudiekursen **emp.txt** har angetts för hello filnamn, så att endast filen hämtas för bearbetning.</span><span class="sxs-lookup"><span data-stu-id="a12ea-245">In this tutorial, **emp.txt** is specified for hello fileName, so only that file is picked up for processing.</span></span> |
    | <span data-ttu-id="a12ea-246">format -> typ</span><span class="sxs-lookup"><span data-stu-id="a12ea-246">format -> type</span></span> |<span data-ttu-id="a12ea-247">hello indatafilen är i hello-format, så vi använder **TextFormat**.</span><span class="sxs-lookup"><span data-stu-id="a12ea-247">hello input file is in hello text format, so we use **TextFormat**.</span></span> |
    | <span data-ttu-id="a12ea-248">columnDelimiter</span><span class="sxs-lookup"><span data-stu-id="a12ea-248">columnDelimiter</span></span> | <span data-ttu-id="a12ea-249">hello kolumner i hello indatafilen avgränsas med **kommatecken tecken (`,`)**.</span><span class="sxs-lookup"><span data-stu-id="a12ea-249">hello columns in hello input file are delimited by **comma character (`,`)**.</span></span> |
    | <span data-ttu-id="a12ea-250">frekvens/intervall</span><span class="sxs-lookup"><span data-stu-id="a12ea-250">frequency/interval</span></span> | <span data-ttu-id="a12ea-251">hello frekvens har angetts för**timme** och intervallet anges för**1**, vilket innebär att hello indata segment är tillgängliga **varje timme**.</span><span class="sxs-lookup"><span data-stu-id="a12ea-251">hello frequency is set too**Hour** and interval is  set too**1**, which means that hello input slices are available **hourly**.</span></span> <span data-ttu-id="a12ea-252">Med andra ord hello Data Factory-tjänsten söker efter indata varje timme i hello rotmapp blob-behållaren (**adftutorial**) du har angett.</span><span class="sxs-lookup"><span data-stu-id="a12ea-252">In other words, hello Data Factory service looks for input data every hour in hello root folder of blob container (**adftutorial**) you specified.</span></span> <span data-ttu-id="a12ea-253">Det ser ut för hello data inom hello pipeline start- och tider, inte före eller efter dessa tider.</span><span class="sxs-lookup"><span data-stu-id="a12ea-253">It looks for hello data within hello pipeline start and end times, not before or after these times.</span></span>  |
    | <span data-ttu-id="a12ea-254">extern</span><span class="sxs-lookup"><span data-stu-id="a12ea-254">external</span></span> | <span data-ttu-id="a12ea-255">Den här egenskapen anges för**SANT** om hello data inte genereras av denna pipeline.</span><span class="sxs-lookup"><span data-stu-id="a12ea-255">This property is set too**true** if hello data is not generated by this pipeline.</span></span> <span data-ttu-id="a12ea-256">hello inkommande data i den här självstudiekursen har hello emp.txt fil, som genereras av denna pipeline, så vi ställa in den här egenskapen tootrue.</span><span class="sxs-lookup"><span data-stu-id="a12ea-256">hello input data in this tutorial is in hello emp.txt file, which is not generated by this pipeline, so we set this property tootrue.</span></span> |

    <span data-ttu-id="a12ea-257">Mer information om de här JSON-egenskaperna finns i artikeln [Azure Blob-anslutningsapp](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="a12ea-257">For more information about these JSON properties, see [Azure Blob connector article](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
2. <span data-ttu-id="a12ea-258">Kör följande kommando toocreate hello Data Factory dataset hello.</span><span class="sxs-lookup"><span data-stu-id="a12ea-258">Run hello following command toocreate hello Data Factory dataset.</span></span>

    ```PowerShell  
    New-AzureRmDataFactoryDataset $df -File .\InputDataset.json
    ```
    <span data-ttu-id="a12ea-259">Här är exempel på utdata från hello:</span><span class="sxs-lookup"><span data-stu-id="a12ea-259">Here is hello sample output:</span></span>

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

### <a name="create-an-output-dataset"></a><span data-ttu-id="a12ea-260">Skapa en datauppsättning för utdata</span><span class="sxs-lookup"><span data-stu-id="a12ea-260">Create an output dataset</span></span>
<span data-ttu-id="a12ea-261">I den här delen av hello steget skapar du en datamängd för utdata som heter **OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="a12ea-261">In this part of hello step, you create an output dataset named **OutputDataset**.</span></span> <span data-ttu-id="a12ea-262">Denna dataset pekar tooa SQL-tabellen i hello Azure SQL-databas som representeras av **AzureSqlLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="a12ea-262">This dataset points tooa SQL table in hello Azure SQL database represented by **AzureSqlLinkedService**.</span></span> 

1. <span data-ttu-id="a12ea-263">Skapa en JSON-fil med namnet **OutputDataset.json** i hello **C:\ADFGetStartedPSH** mapp med hello följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="a12ea-263">Create a JSON file named **OutputDataset.json** in hello **C:\ADFGetStartedPSH** folder with hello following content:</span></span>

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

    <span data-ttu-id="a12ea-264">hello innehåller följande tabell beskrivningar för hello JSON egenskaper som används i hello fragment:</span><span class="sxs-lookup"><span data-stu-id="a12ea-264">hello following table provides descriptions for hello JSON properties used in hello snippet:</span></span>

    | <span data-ttu-id="a12ea-265">Egenskap</span><span class="sxs-lookup"><span data-stu-id="a12ea-265">Property</span></span> | <span data-ttu-id="a12ea-266">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a12ea-266">Description</span></span> |
    |:--- |:--- |
    | <span data-ttu-id="a12ea-267">typ</span><span class="sxs-lookup"><span data-stu-id="a12ea-267">type</span></span> | <span data-ttu-id="a12ea-268">hello typegenskapen har ställts in för**AzureSqlTable** eftersom data är kopierade tooa tabell i Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="a12ea-268">hello type property is set too**AzureSqlTable** because data is copied tooa table in an Azure SQL database.</span></span> |
    | <span data-ttu-id="a12ea-269">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="a12ea-269">linkedServiceName</span></span> | <span data-ttu-id="a12ea-270">Refererar toohello **AzureSqlLinkedService** som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="a12ea-270">Refers toohello **AzureSqlLinkedService** that you created earlier.</span></span> |
    | <span data-ttu-id="a12ea-271">tableName</span><span class="sxs-lookup"><span data-stu-id="a12ea-271">tableName</span></span> | <span data-ttu-id="a12ea-272">Angivna hello **tabell** toowhich hello data kopieras.</span><span class="sxs-lookup"><span data-stu-id="a12ea-272">Specified hello **table** toowhich hello data is copied.</span></span> | 
    | <span data-ttu-id="a12ea-273">frekvens/intervall</span><span class="sxs-lookup"><span data-stu-id="a12ea-273">frequency/interval</span></span> | <span data-ttu-id="a12ea-274">hello frekvens har angetts för**timme** och är **1**, vilket innebär att hello utdata segment produceras **varje timme** mellan hello pipeline start och slut tider inte före eller När dessa tider.</span><span class="sxs-lookup"><span data-stu-id="a12ea-274">hello frequency is set too**Hour** and interval is **1**, which means that hello output slices are produced **hourly** between hello pipeline start and end times, not before or after these times.</span></span>  |

    <span data-ttu-id="a12ea-275">Det finns tre kolumner – **ID**, **Förnamn**, och **efternamn** – i hello tomma tabellen i hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="a12ea-275">There are three columns – **ID**, **FirstName**, and **LastName** – in hello emp table in hello database.</span></span> <span data-ttu-id="a12ea-276">ID: T är en identitetskolumn, så du behöver bara toospecify **Förnamn** och **efternamn** här.</span><span class="sxs-lookup"><span data-stu-id="a12ea-276">ID is an identity column, so you need toospecify only **FirstName** and **LastName** here.</span></span>

    <span data-ttu-id="a12ea-277">Mer information om de här JSON-egenskaperna finns i artikeln [Azure SQL-anslutningsapp](data-factory-azure-sql-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="a12ea-277">For more information about these JSON properties, see [Azure SQL connector article](data-factory-azure-sql-connector.md#dataset-properties).</span></span>
2. <span data-ttu-id="a12ea-278">Kör följande kommando toocreate hello data factory dataset hello.</span><span class="sxs-lookup"><span data-stu-id="a12ea-278">Run hello following command toocreate hello data factory dataset.</span></span>

    ```PowerShell   
    New-AzureRmDataFactoryDataset $df -File .\OutputDataset.json
    ```

    <span data-ttu-id="a12ea-279">Här är exempel på utdata från hello:</span><span class="sxs-lookup"><span data-stu-id="a12ea-279">Here is hello sample output:</span></span>

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

## <a name="create-a-pipeline"></a><span data-ttu-id="a12ea-280">Skapa en pipeline</span><span class="sxs-lookup"><span data-stu-id="a12ea-280">Create a pipeline</span></span>
<span data-ttu-id="a12ea-281">I det här steget ska du skapa en pipeline med en **kopieringsaktivitet** som använder **InputDataset** som indata och **OutputDataset** som utdata.</span><span class="sxs-lookup"><span data-stu-id="a12ea-281">In this step, you create a pipeline with a **copy activity** that uses **InputDataset** as an input and **OutputDataset** as an output.</span></span>

<span data-ttu-id="a12ea-282">Datamängd för utdata är för närvarande vilka enheter hello schema.</span><span class="sxs-lookup"><span data-stu-id="a12ea-282">Currently, output dataset is what drives hello schedule.</span></span> <span data-ttu-id="a12ea-283">I den här kursen är utdatauppsättningen konfigurerade tooproduce ett segment en gång i timmen.</span><span class="sxs-lookup"><span data-stu-id="a12ea-283">In this tutorial, output dataset is configured tooproduce a slice once an hour.</span></span> <span data-ttu-id="a12ea-284">hello pipeline har en starttid och sluttid som är en dag från varandra, vilket är 24 timmar.</span><span class="sxs-lookup"><span data-stu-id="a12ea-284">hello pipeline has a start time and end time that are one day apart, which is 24 hours.</span></span> <span data-ttu-id="a12ea-285">Därför produceras 24 segment för utdatauppsättningen av hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="a12ea-285">Therefore, 24 slices of output dataset are produced by hello pipeline.</span></span> 


1. <span data-ttu-id="a12ea-286">Skapa en JSON-fil med namnet **ADFTutorialPipeline.json** i hello **C:\ADFGetStartedPSH** mapp med hello följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="a12ea-286">Create a JSON file named **ADFTutorialPipeline.json** in hello **C:\ADFGetStartedPSH** folder, with hello following content:</span></span>

    ```json   
    {
      "name": "ADFTutorialPipeline",
      "properties": {
        "description": "Copy data from a blob tooAzure SQL table",
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
    <span data-ttu-id="a12ea-287">Observera följande punkter hello:</span><span class="sxs-lookup"><span data-stu-id="a12ea-287">Note hello following points:</span></span>
   
    - <span data-ttu-id="a12ea-288">Under hello aktiviteter är bara en aktivitet vars **typen** har angetts för**kopiera**.</span><span class="sxs-lookup"><span data-stu-id="a12ea-288">In hello activities section, there is only one activity whose **type** is set too**Copy**.</span></span> <span data-ttu-id="a12ea-289">Läs mer om hello kopieringsaktiviteten [data movement aktiviteter](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="a12ea-289">For more information about hello copy activity, see [data movement activities](data-factory-data-movement-activities.md).</span></span> <span data-ttu-id="a12ea-290">I Data Factory-lösningar, kan du också använda [datatransformeringsaktiviteter](data-factory-data-transformation-activities.md).</span><span class="sxs-lookup"><span data-stu-id="a12ea-290">In Data Factory solutions, you can also use [data transformation activities](data-factory-data-transformation-activities.md).</span></span>
    - <span data-ttu-id="a12ea-291">Indata för hello aktiviteten är inställd för**InputDataset** och utdata för hello aktiviteten är inställd för**OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="a12ea-291">Input for hello activity is set too**InputDataset** and output for hello activity is set too**OutputDataset**.</span></span> 
    - <span data-ttu-id="a12ea-292">I hello **typeProperties** avsnittet **BlobSource** har angetts som hello källtypen och **SqlSink** har angetts som hello Mottagartypen.</span><span class="sxs-lookup"><span data-stu-id="a12ea-292">In hello **typeProperties** section, **BlobSource** is specified as hello source type and **SqlSink** is specified as hello sink type.</span></span> <span data-ttu-id="a12ea-293">En fullständig lista över datakällor som stöds av hello kopieringsaktiviteten som källor och sänkor finns [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="a12ea-293">For a complete list of data stores supported by hello copy activity as sources and sinks, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="a12ea-294">toolearn hur toouse en specifik stöds data lagras som en källor/mottagare, klicka länken hello i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="a12ea-294">toolearn how toouse a specific supported data store as a source/sink, click hello link in hello table.</span></span>  
     
    <span data-ttu-id="a12ea-295">Ersätt hello värdet för hello **starta** egenskap med hello aktuell dag och **end** värdet med hello nästa dag.</span><span class="sxs-lookup"><span data-stu-id="a12ea-295">Replace hello value of hello **start** property with hello current day and **end** value with hello next day.</span></span> <span data-ttu-id="a12ea-296">Du kan ange endast hello DatumDel och hoppa över hello time-delen av hello datum och tid.</span><span class="sxs-lookup"><span data-stu-id="a12ea-296">You can specify only hello date part and skip hello time part of hello date time.</span></span> <span data-ttu-id="a12ea-297">Till exempel ”2016-02-03”, vilket motsvarar för ”2016-02-03T00:00:00Z”</span><span class="sxs-lookup"><span data-stu-id="a12ea-297">For example, "2016-02-03", which is equivalent too"2016-02-03T00:00:00Z"</span></span>
     
    <span data-ttu-id="a12ea-298">Både start- och slutdatum måste vara i [ISO-format](http://en.wikipedia.org/wiki/ISO_8601).</span><span class="sxs-lookup"><span data-stu-id="a12ea-298">Both start and end datetimes must be in [ISO format](http://en.wikipedia.org/wiki/ISO_8601).</span></span> <span data-ttu-id="a12ea-299">Exempel: 2016-10-14T16:32:41Z.</span><span class="sxs-lookup"><span data-stu-id="a12ea-299">For example: 2016-10-14T16:32:41Z.</span></span> <span data-ttu-id="a12ea-300">Hej **end** tid är valfritt, men vi använda den i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="a12ea-300">hello **end** time is optional, but we use it in this tutorial.</span></span> 
     
    <span data-ttu-id="a12ea-301">Om du inte anger värdet för hello **end** egenskapen, det beräknas som ”**start + 48 timmar**”.</span><span class="sxs-lookup"><span data-stu-id="a12ea-301">If you do not specify value for hello **end** property, it is calculated as "**start + 48 hours**".</span></span> <span data-ttu-id="a12ea-302">toorun hello pipeline på obestämd tid, ange **9999-09-09** som hello värde hello **end** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="a12ea-302">toorun hello pipeline indefinitely, specify **9999-09-09** as hello value for hello **end** property.</span></span>
     
    <span data-ttu-id="a12ea-303">I föregående exempel hello, finns det 24 datasektorer som varje datasektorn produceras per timma.</span><span class="sxs-lookup"><span data-stu-id="a12ea-303">In hello preceding example, there are 24 data slices as each data slice is produced hourly.</span></span>

    <span data-ttu-id="a12ea-304">Beskrivningar av JSON-egenskaper i en pipeline-definition finns i artikeln [skapa pipelines](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="a12ea-304">For descriptions of JSON properties in a pipeline definition, see [create pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="a12ea-305">Beskrivningar av JSON-egenskaper i en kopieringsaktivitet-definition finns i artikeln [aktiviteter för dataflyttning](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="a12ea-305">For descriptions of JSON properties in a copy activity definition, see [data movement activities](data-factory-data-movement-activities.md).</span></span> <span data-ttu-id="a12ea-306">Beskrivningar av JSON-egenskaper som stöds av BlobSource finns i artikeln [Azure Blob-anslutningsapp](data-factory-azure-blob-connector.md).</span><span class="sxs-lookup"><span data-stu-id="a12ea-306">For descriptions of JSON properties supported by BlobSource, see [Azure Blob connector article](data-factory-azure-blob-connector.md).</span></span> <span data-ttu-id="a12ea-307">Beskrivningar av JSON-egenskaper som stöds av SqlSink finns i artikeln [Azure SQL Database-anslutningsapp](data-factory-azure-sql-connector.md).</span><span class="sxs-lookup"><span data-stu-id="a12ea-307">For descriptions of JSON properties supported by SqlSink, see [Azure SQL Database connector article](data-factory-azure-sql-connector.md).</span></span>
2. <span data-ttu-id="a12ea-308">Kör följande kommando toocreate hello data factory-tabell hello.</span><span class="sxs-lookup"><span data-stu-id="a12ea-308">Run hello following command toocreate hello data factory table.</span></span>

    ```PowerShell   
    New-AzureRmDataFactoryPipeline $df -File .\ADFTutorialPipeline.json
    ```

    <span data-ttu-id="a12ea-309">Här är exempel på utdata från hello:</span><span class="sxs-lookup"><span data-stu-id="a12ea-309">Here is hello sample output:</span></span> 

    ```
    PipelineName      : ADFTutorialPipeline
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFTutorialDataFactoryPSH0516
    Properties        : Microsoft.Azure.Management.DataFactories.Models.PipelinePropertie
    ProvisioningState : Succeeded
    ```

<span data-ttu-id="a12ea-310">**Grattis!**</span><span class="sxs-lookup"><span data-stu-id="a12ea-310">**Congratulations!**</span></span> <span data-ttu-id="a12ea-311">Du har skapat ett Azure data factory med en rörledning toocopy data från Azure blob storage tooan Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="a12ea-311">You have successfully created an Azure data factory with a pipeline toocopy data from an Azure blob storage tooan Azure SQL database.</span></span> 

## <a name="monitor-hello-pipeline"></a><span data-ttu-id="a12ea-312">Övervakaren hello pipeline</span><span class="sxs-lookup"><span data-stu-id="a12ea-312">Monitor hello pipeline</span></span>
<span data-ttu-id="a12ea-313">I det här steget använder du Azure PowerShell toomonitor vad som händer i ett Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="a12ea-313">In this step, you use Azure PowerShell toomonitor what’s going on in an Azure data factory.</span></span>

1. <span data-ttu-id="a12ea-314">Ersätt &lt;DataFactoryName&gt; med hello namn för din data factory och kör **Get-AzureRmDataFactory**, och tilldela hello utdata tooa variabeln $df.</span><span class="sxs-lookup"><span data-stu-id="a12ea-314">Replace &lt;DataFactoryName&gt; with hello name of your data factory and run **Get-AzureRmDataFactory**, and assign hello output tooa variable $df.</span></span>

    ```PowerShell  
    $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name <DataFactoryName>
    ```

    <span data-ttu-id="a12ea-315">Exempel:</span><span class="sxs-lookup"><span data-stu-id="a12ea-315">For example:</span></span>
    ```PowerShell
    $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name ADFTutorialDataFactoryPSH0516
    ```
    
    <span data-ttu-id="a12ea-316">Kör sedan skriva ut hello innehållet i $df toosee hello följande utdata:</span><span class="sxs-lookup"><span data-stu-id="a12ea-316">Then, run print hello contents of $df toosee hello following output:</span></span> 
    
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
2. <span data-ttu-id="a12ea-317">Kör **Get-AzureRmDataFactorySlice** tooget information om alla segment av hello **OutputDataset**, vilket är hello utdatauppsättningen för hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="a12ea-317">Run **Get-AzureRmDataFactorySlice** tooget details about all slices of hello **OutputDataset**, which is hello output dataset of hello pipeline.</span></span>  

    ```PowerShell   
    Get-AzureRmDataFactorySlice $df -DatasetName OutputDataset -StartDateTime 2017-05-11T00:00:00Z
    ```

   <span data-ttu-id="a12ea-318">Den här inställningen ska matcha hello **starta** värdet i hello pipeline-JSON.</span><span class="sxs-lookup"><span data-stu-id="a12ea-318">This setting should match hello **Start** value in hello pipeline JSON.</span></span> <span data-ttu-id="a12ea-319">Du bör se 24 segment, en för varje timme från 12: 00 av hello dagens too12 är av hello nästa dag.</span><span class="sxs-lookup"><span data-stu-id="a12ea-319">You should see 24 slices, one for each hour from 12 AM of hello current day too12 AM of hello next day.</span></span>

   <span data-ttu-id="a12ea-320">Här följer tre exempel segment från hello utdata:</span><span class="sxs-lookup"><span data-stu-id="a12ea-320">Here are three sample slices from hello output:</span></span> 

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
3. <span data-ttu-id="a12ea-321">Kör **Get-AzureRmDataFactoryRun** tooget hello information om aktivitet körs för en **specifika** sektorn.</span><span class="sxs-lookup"><span data-stu-id="a12ea-321">Run **Get-AzureRmDataFactoryRun** tooget hello details of activity runs for a **specific** slice.</span></span> <span data-ttu-id="a12ea-322">Kopiera hello datetime-värde från hello utdata från hello föregående kommando toospecify hello värde för hello StartDateTime parameter.</span><span class="sxs-lookup"><span data-stu-id="a12ea-322">Copy hello date-time value from hello output of hello previous command toospecify hello value for hello StartDateTime parameter.</span></span> 

    ```PowerShell  
    Get-AzureRmDataFactoryRun $df -DatasetName OutputDataset -StartDateTime "5/11/2017 09:00:00 PM"
    ```

   <span data-ttu-id="a12ea-323">Här är exempel på utdata från hello:</span><span class="sxs-lookup"><span data-stu-id="a12ea-323">Here is hello sample output:</span></span> 

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

<span data-ttu-id="a12ea-324">Se [Cmdlet-referens för Data Factory](/powershell/module/azurerm.datafactories) för omfattande dokumentation om Data Factory-cmdletar.</span><span class="sxs-lookup"><span data-stu-id="a12ea-324">For comprehensive documentation on Data Factory cmdlets, see [Data Factory Cmdlet Reference](/powershell/module/azurerm.datafactories).</span></span>

## <a name="summary"></a><span data-ttu-id="a12ea-325">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="a12ea-325">Summary</span></span>
<span data-ttu-id="a12ea-326">I kursen får skapat du ett Azure data factory toocopy data från Azure blob tooan Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="a12ea-326">In this tutorial, you created an Azure data factory toocopy data from an Azure blob tooan Azure SQL database.</span></span> <span data-ttu-id="a12ea-327">Du kan använda PowerShell toocreate hello data factory, länkade tjänster, datauppsättningar och en pipeline.</span><span class="sxs-lookup"><span data-stu-id="a12ea-327">You used PowerShell toocreate hello data factory, linked services, datasets, and a pipeline.</span></span> <span data-ttu-id="a12ea-328">Här följer hello anvisningar som du utförde i den här kursen:</span><span class="sxs-lookup"><span data-stu-id="a12ea-328">Here are hello high-level steps you performed in this tutorial:</span></span>  

1. <span data-ttu-id="a12ea-329">Du skapade en Azure **Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="a12ea-329">Created an Azure **data factory**.</span></span>
2. <span data-ttu-id="a12ea-330">Du skapade **länkade tjänster**:</span><span class="sxs-lookup"><span data-stu-id="a12ea-330">Created **linked services**:</span></span>

   <span data-ttu-id="a12ea-331">a.</span><span class="sxs-lookup"><span data-stu-id="a12ea-331">a.</span></span> <span data-ttu-id="a12ea-332">En **Azure Storage** länkade tjänsten toolink ditt Azure storage-konto som innehåller data som indata.</span><span class="sxs-lookup"><span data-stu-id="a12ea-332">An **Azure Storage** linked service toolink your Azure storage account that holds input data.</span></span>     
   <span data-ttu-id="a12ea-333">b.</span><span class="sxs-lookup"><span data-stu-id="a12ea-333">b.</span></span> <span data-ttu-id="a12ea-334">En **Azure SQL** länkade tjänsten toolink din SQL-databas som innehåller hello utdata.</span><span class="sxs-lookup"><span data-stu-id="a12ea-334">An **Azure SQL** linked service toolink your SQL database that holds hello output data.</span></span>
3. <span data-ttu-id="a12ea-335">Du skapade **datauppsättningar** som beskriver indata och utdata för pipelines.</span><span class="sxs-lookup"><span data-stu-id="a12ea-335">Created **datasets** that describe input data and output data for pipelines.</span></span>
4. <span data-ttu-id="a12ea-336">Skapa en **pipeline** med **Kopieringsaktiviteten**, med **BlobSource** som hello källa och **SqlSink** som hello mottagare.</span><span class="sxs-lookup"><span data-stu-id="a12ea-336">Created a **pipeline** with **Copy Activity**, with **BlobSource** as hello source and **SqlSink** as hello sink.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a12ea-337">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a12ea-337">Next steps</span></span>
<span data-ttu-id="a12ea-338">I den här kursen används Azure blob storage som ett datalager för källa och en Azure SQL-databas som ett dataarkiv som mål i en kopieringsåtgärd.</span><span class="sxs-lookup"><span data-stu-id="a12ea-338">In this tutorial, you used Azure blob storage as a source data store and an Azure SQL database as a destination data store in a copy operation.</span></span> <span data-ttu-id="a12ea-339">hello innehåller följande tabell en lista över datakällor som stöds som källor och mål av hello kopieringsaktiviteten:</span><span class="sxs-lookup"><span data-stu-id="a12ea-339">hello following table provides a list of data stores supported as sources and destinations by hello copy activity:</span></span> 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

<span data-ttu-id="a12ea-340">toolearn om hur toocopy till eller från en databas, klicka länken hello för hello datalager i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="a12ea-340">toolearn about how toocopy data to/from a data store, click hello link for hello data store in hello table.</span></span> 

