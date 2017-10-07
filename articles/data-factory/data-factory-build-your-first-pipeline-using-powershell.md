---
title: "aaaBuild din första data factory (PowerShell) | Microsoft Docs"
description: "I den här självstudien skapar du ett exempel på en Azure Data Factory-pipeline med hjälp av Azure PowerShell."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 22ec1236-ea86-4eb7-b903-0e79a58b90c7
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 626260798b56d590577b3c4b24f7cf52873c9f80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-build-your-first-azure-data-factory-using-azure-powershell"></a><span data-ttu-id="299bc-103">Självstudier: Skapa din första Azure-datafabrik med Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="299bc-103">Tutorial: Build your first Azure data factory using Azure PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="299bc-104">Översikt och förutsättningar</span><span class="sxs-lookup"><span data-stu-id="299bc-104">Overview and prerequisites</span></span>](data-factory-build-your-first-pipeline.md)
> * [<span data-ttu-id="299bc-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="299bc-105">Azure portal</span></span>](data-factory-build-your-first-pipeline-using-editor.md)
> * [<span data-ttu-id="299bc-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="299bc-106">Visual Studio</span></span>](data-factory-build-your-first-pipeline-using-vs.md)
> * [<span data-ttu-id="299bc-107">PowerShell</span><span class="sxs-lookup"><span data-stu-id="299bc-107">PowerShell</span></span>](data-factory-build-your-first-pipeline-using-powershell.md)
> * [<span data-ttu-id="299bc-108">Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="299bc-108">Resource Manager Template</span></span>](data-factory-build-your-first-pipeline-using-arm.md)
> * [<span data-ttu-id="299bc-109">REST-API</span><span class="sxs-lookup"><span data-stu-id="299bc-109">REST API</span></span>](data-factory-build-your-first-pipeline-using-rest-api.md)
>
>

<span data-ttu-id="299bc-110">I den här artikeln använder du Azure PowerShell toocreate din första Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="299bc-110">In this article, you use Azure PowerShell toocreate your first Azure data factory.</span></span> <span data-ttu-id="299bc-111">toodo hello självstudier med andra verktyg/SDK: er, Välj ett alternativ för hello hello listrutan.</span><span class="sxs-lookup"><span data-stu-id="299bc-111">toodo hello tutorial using other tools/SDKs, select one of hello options from hello drop-down list.</span></span>

<span data-ttu-id="299bc-112">hello pipeline i den här självstudiekursen har en aktivitet: **HDInsight Hive aktiviteten**.</span><span class="sxs-lookup"><span data-stu-id="299bc-112">hello pipeline in this tutorial has one activity: **HDInsight Hive activity**.</span></span> <span data-ttu-id="299bc-113">Den här aktiviteten körs en hive-skript på ett Azure HDInsight-kluster att transformeringar inkommande data tooproduce utdata.</span><span class="sxs-lookup"><span data-stu-id="299bc-113">This activity runs a hive script on an Azure HDInsight cluster that transforms input data tooproduce output data.</span></span> <span data-ttu-id="299bc-114">hello pipeline är schemalagda toorun när en månad mellan hello angivna start- och sluttider.</span><span class="sxs-lookup"><span data-stu-id="299bc-114">hello pipeline is scheduled toorun once a month between hello specified start and end times.</span></span> 

> [!NOTE]
> <span data-ttu-id="299bc-115">hello data pipeline i den här handledningen omvandlar indata tooproduce utdata.</span><span class="sxs-lookup"><span data-stu-id="299bc-115">hello data pipeline in this tutorial transforms input data tooproduce output data.</span></span> <span data-ttu-id="299bc-116">Den kopierar inte data från en källa data store tooa målarkiv data.</span><span class="sxs-lookup"><span data-stu-id="299bc-116">It does not copy data from a source data store tooa destination data store.</span></span> <span data-ttu-id="299bc-117">En självstudiekurs om hur toocopy data med hjälp av Azure Data Factory finns [Självstudier: kopiera data från Blob Storage tooSQL databasen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="299bc-117">For a tutorial on how toocopy data using Azure Data Factory, see [Tutorial: Copy data from Blob Storage tooSQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>
> 
> <span data-ttu-id="299bc-118">En pipeline kan ha fler än en aktivitet.</span><span class="sxs-lookup"><span data-stu-id="299bc-118">A pipeline can have more than one activity.</span></span> <span data-ttu-id="299bc-119">Och du kan länka två aktiviteter (köra en aktivitet efter ett annat) genom att ange hello datamängd för utdata för en aktivitet som hello indatauppsättning av hello andra aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="299bc-119">And, you can chain two activities (run one activity after another) by setting hello output dataset of one activity as hello input dataset of hello other activity.</span></span> <span data-ttu-id="299bc-120">Mer detaljerad information finns i [Scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) (Schemaläggning och utförande i Data Factory).</span><span class="sxs-lookup"><span data-stu-id="299bc-120">For more information, see [scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="299bc-121">Krav</span><span class="sxs-lookup"><span data-stu-id="299bc-121">Prerequisites</span></span>
* <span data-ttu-id="299bc-122">Läs igenom [kursen översikt](data-factory-build-your-first-pipeline.md) artikeln och fullständig hello **nödvändiga** steg.</span><span class="sxs-lookup"><span data-stu-id="299bc-122">Read through [Tutorial Overview](data-factory-build-your-first-pipeline.md) article and complete hello **prerequisite** steps.</span></span>
* <span data-ttu-id="299bc-123">Följ instruktionerna i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) artikel tooinstall senaste versionen av Azure PowerShell på datorn.</span><span class="sxs-lookup"><span data-stu-id="299bc-123">Follow instructions in [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) article tooinstall latest version of Azure PowerShell on your computer.</span></span>
* <span data-ttu-id="299bc-124">(valfritt) Den här artikeln täcker inte alla hello Data Factory-cmdletar.</span><span class="sxs-lookup"><span data-stu-id="299bc-124">(optional) This article does not cover all hello Data Factory cmdlets.</span></span> <span data-ttu-id="299bc-125">Se [Cmdlet-referens för Data Factory](/powershell/module/azurerm.datafactories) för omfattande dokumentation om Data Factory-cmdletar.</span><span class="sxs-lookup"><span data-stu-id="299bc-125">See [Data Factory Cmdlet Reference](/powershell/module/azurerm.datafactories) for comprehensive documentation on Data Factory cmdlets.</span></span>

## <a name="create-data-factory"></a><span data-ttu-id="299bc-126">Skapa en datafabrik</span><span class="sxs-lookup"><span data-stu-id="299bc-126">Create data factory</span></span>
<span data-ttu-id="299bc-127">I det här steget kan du använda Azure PowerShell toocreate ett Azure Data Factory med namnet **FirstDataFactoryPSH**.</span><span class="sxs-lookup"><span data-stu-id="299bc-127">In this step, you use Azure PowerShell toocreate an Azure Data Factory named **FirstDataFactoryPSH**.</span></span> <span data-ttu-id="299bc-128">En datafabrik kan ha en eller flera pipelines.</span><span class="sxs-lookup"><span data-stu-id="299bc-128">A data factory can have one or more pipelines.</span></span> <span data-ttu-id="299bc-129">En pipeline kan innehålla en eller flera aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="299bc-129">A pipeline can have one or more activities in it.</span></span> <span data-ttu-id="299bc-130">Till exempel indata en Kopieringsaktiviteten toocopy data från ett dataarkiv som källa tooa mål och en HDInsight Hive aktiviteten toorun en Hive-skript tootransform.</span><span class="sxs-lookup"><span data-stu-id="299bc-130">For example, a Copy Activity toocopy data from a source tooa destination data store and a HDInsight Hive activity toorun a Hive script tootransform input data.</span></span> <span data-ttu-id="299bc-131">Låt oss börja med att skapa hello data factory i det här steget.</span><span class="sxs-lookup"><span data-stu-id="299bc-131">Let's start with creating hello data factory in this step.</span></span>

1. <span data-ttu-id="299bc-132">Starta Azure PowerShell och kör följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="299bc-132">Start Azure PowerShell and run hello following command.</span></span> <span data-ttu-id="299bc-133">Behåll Azure PowerShell öppen tills hello slutet av den här kursen.</span><span class="sxs-lookup"><span data-stu-id="299bc-133">Keep Azure PowerShell open until hello end of this tutorial.</span></span> <span data-ttu-id="299bc-134">Om du stänga och öppna måste toorun kommandona igen.</span><span class="sxs-lookup"><span data-stu-id="299bc-134">If you close and reopen, you need toorun these commands again.</span></span>
   * <span data-ttu-id="299bc-135">Kör följande kommando hello och ange hello användarnamn och lösenord som du använder toosign i toohello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="299bc-135">Run hello following command and enter hello user name and password that you use toosign in toohello Azure portal.</span></span>
    ```PowerShell
    Login-AzureRmAccount
    ```    
   * <span data-ttu-id="299bc-136">Kör följande kommando tooview hello alla hello prenumerationer för det här kontot.</span><span class="sxs-lookup"><span data-stu-id="299bc-136">Run hello following command tooview all hello subscriptions for this account.</span></span>
    ```PowerShell
    Get-AzureRmSubscription 
    ```
   * <span data-ttu-id="299bc-137">Hello kör följande kommando tooselect hello prenumeration som du vill toowork med.</span><span class="sxs-lookup"><span data-stu-id="299bc-137">Run hello following command tooselect hello subscription that you want toowork with.</span></span> <span data-ttu-id="299bc-138">Den här prenumerationen bör hello samtidigt som hello något du använde i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="299bc-138">This subscription should be hello same as hello one you used in hello Azure portal.</span></span>
    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext
    ```     
2. <span data-ttu-id="299bc-139">Skapa en Azure-resursgrupp med namnet **ADFTutorialResourceGroup** genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="299bc-139">Create an Azure resource group named **ADFTutorialResourceGroup** by running hello following command:</span></span>
    
    ```PowerShell
    New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```
    <span data-ttu-id="299bc-140">Några av hello stegen i den här självstudiekursen förutsätts att du använder hello resursgrupp med namnet ADFTutorialResourceGroup.</span><span class="sxs-lookup"><span data-stu-id="299bc-140">Some of hello steps in this tutorial assume that you use hello resource group named ADFTutorialResourceGroup.</span></span> <span data-ttu-id="299bc-141">Om du använder en annan resursgrupp måste toouse den i stället för ADFTutorialResourceGroup i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="299bc-141">If you use a different resource group, you need toouse it in place of ADFTutorialResourceGroup in this tutorial.</span></span>
3. <span data-ttu-id="299bc-142">Kör hello **ny AzureRmDataFactory** cmdlet som skapar en datafabrik som heter **FirstDataFactoryPSH**.</span><span class="sxs-lookup"><span data-stu-id="299bc-142">Run hello **New-AzureRmDataFactory** cmdlet that creates a data factory named **FirstDataFactoryPSH**.</span></span>

    ```PowerShell
    New-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name FirstDataFactoryPSH –Location "West US"
    ```
<span data-ttu-id="299bc-143">Observera följande punkter hello:</span><span class="sxs-lookup"><span data-stu-id="299bc-143">Note hello following points:</span></span>

* <span data-ttu-id="299bc-144">hello namnet på hello Azure Data Factory måste vara globalt unika.</span><span class="sxs-lookup"><span data-stu-id="299bc-144">hello name of hello Azure Data Factory must be globally unique.</span></span> <span data-ttu-id="299bc-145">Om felmeddelandet hello **datafabriksnamnet ”FirstDataFactoryPSH” är inte tillgänglig**, ändra hello namn (till exempel yournameFirstDataFactoryPSH).</span><span class="sxs-lookup"><span data-stu-id="299bc-145">If you receive hello error **Data factory name “FirstDataFactoryPSH” is not available**, change hello name (for example, yournameFirstDataFactoryPSH).</span></span> <span data-ttu-id="299bc-146">Använd det här namnet i stället för ADFTutorialFactoryPSH när du utför stegen i självstudien.</span><span class="sxs-lookup"><span data-stu-id="299bc-146">Use this name in place of ADFTutorialFactoryPSH while performing steps in this tutorial.</span></span> <span data-ttu-id="299bc-147">Se artikeln [Data Factory – namnregler](data-factory-naming-rules.md) för namnregler för Data Factory-artefakter.</span><span class="sxs-lookup"><span data-stu-id="299bc-147">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>
* <span data-ttu-id="299bc-148">toocreate Data Factory instanser måste toobe deltagare/administratören av hello Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="299bc-148">toocreate Data Factory instances, you need toobe a contributor/administrator of hello Azure subscription</span></span>
* <span data-ttu-id="299bc-149">hello namnet på hello data factory kan registreras som en DNS-namnet i hello framtida och därför bli synligt offentligt.</span><span class="sxs-lookup"><span data-stu-id="299bc-149">hello name of hello data factory may be registered as a DNS name in hello future and hence become publically visible.</span></span>
* <span data-ttu-id="299bc-150">Om felmeddelandet hello ”:**den här prenumerationen är inte registrerad toouse namnområde Microsoft.DataFactory**”, gör du något av följande hello och försök att publicera igen:</span><span class="sxs-lookup"><span data-stu-id="299bc-150">If you receive hello error: "**This subscription is not registered toouse namespace Microsoft.DataFactory**", do one of hello following and try publishing again:</span></span>

  * <span data-ttu-id="299bc-151">Kör hello efter kommandot tooregister hello Data Factory-providern i Azure PowerShell:</span><span class="sxs-lookup"><span data-stu-id="299bc-151">In Azure PowerShell, run hello following command tooregister hello Data Factory provider:</span></span>

    ```PowerShell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```
      <span data-ttu-id="299bc-152">Du kan köra följande kommando tooconfirm hello som hello Data Factory providern är registrerad:</span><span class="sxs-lookup"><span data-stu-id="299bc-152">You can run hello following command tooconfirm that hello Data Factory provider is registered:</span></span>

    ```PowerShell
    Get-AzureRmResourceProvider
    ```
  * <span data-ttu-id="299bc-153">Logga in med hjälp av hello Azure-prenumeration i hello [Azure-portalen](https://portal.azure.com) och navigera tooa Data Factory bladet (eller) skapa en datafabrik i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="299bc-153">Login using hello Azure subscription into hello [Azure portal](https://portal.azure.com) and navigate tooa Data Factory blade (or) create a data factory in hello Azure portal.</span></span> <span data-ttu-id="299bc-154">Den här åtgärden registrerar automatiskt hello-providern för dig.</span><span class="sxs-lookup"><span data-stu-id="299bc-154">This action automatically registers hello provider for you.</span></span>

<span data-ttu-id="299bc-155">Innan du skapar en pipeline, måste toocreate några Data Factory-entiteter först.</span><span class="sxs-lookup"><span data-stu-id="299bc-155">Before creating a pipeline, you need toocreate a few Data Factory entities first.</span></span> <span data-ttu-id="299bc-156">Du måste först skapa länkade tjänster toolink data lagrar/beräknar tooyour data lagras, definiera indata och utdata datauppsättningar toorepresent in-/ utdata i länkade datalager och sedan skapa hello pipeline med en aktivitet som använder dessa data.</span><span class="sxs-lookup"><span data-stu-id="299bc-156">You first create linked services toolink data stores/computes tooyour data store, define input and output datasets toorepresent input/output data in linked data stores, and then create hello pipeline with an activity that uses these datasets.</span></span>

## <a name="create-linked-services"></a><span data-ttu-id="299bc-157">Skapa länkade tjänster</span><span class="sxs-lookup"><span data-stu-id="299bc-157">Create linked services</span></span>
<span data-ttu-id="299bc-158">I det här steget kan länka du ditt Azure Storage-konto och en på begäran Azure HDInsight-kluster tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="299bc-158">In this step, you link your Azure Storage account and an on-demand Azure HDInsight cluster tooyour data factory.</span></span> <span data-ttu-id="299bc-159">hello Azure Storage-konto innehåller hello inkommande och utgående data för hello pipeline i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="299bc-159">hello Azure Storage account holds hello input and output data for hello pipeline in this sample.</span></span> <span data-ttu-id="299bc-160">hello länkad HDInsight-tjänst är används toorun en Hive-skript som angetts i hello aktivitet för hello pipeline i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="299bc-160">hello HDInsight linked service is used toorun a Hive script specified in hello activity of hello pipeline in this sample.</span></span> <span data-ttu-id="299bc-161">Identifiera vilka data store/beräkning tjänster används i din situation och länka dessa tjänster toohello data factory genom att skapa länkade tjänster.</span><span class="sxs-lookup"><span data-stu-id="299bc-161">Identify what data store/compute services are used in your scenario and link those services toohello data factory by creating linked services.</span></span>

### <a name="create-azure-storage-linked-service"></a><span data-ttu-id="299bc-162">Skapa en länkad Azure-lagringstjänst</span><span class="sxs-lookup"><span data-stu-id="299bc-162">Create Azure Storage linked service</span></span>
<span data-ttu-id="299bc-163">I det här steget kan länka du din Azure Storage-konto tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="299bc-163">In this step, you link your Azure Storage account tooyour data factory.</span></span> <span data-ttu-id="299bc-164">Du använder hello samma Azure Storage-konto toostore i/o-data och hello HQL skriptfil.</span><span class="sxs-lookup"><span data-stu-id="299bc-164">You use hello same Azure Storage account toostore input/output data and hello HQL script file.</span></span>

1. <span data-ttu-id="299bc-165">Skapa en JSON-fil som heter StorageLinkedService.json i hello C:\ADFGetStarted mappen med hello följande innehåll.</span><span class="sxs-lookup"><span data-stu-id="299bc-165">Create a JSON file named StorageLinkedService.json in hello C:\ADFGetStarted folder with hello following content.</span></span> <span data-ttu-id="299bc-166">Skapa hello mapp ADFGetStarted om den inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="299bc-166">Create hello folder ADFGetStarted if it does not already exist.</span></span>

    ```json
    {
        "name": "StorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "description": "",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        }
    }
    ```
    <span data-ttu-id="299bc-167">Ersätt **kontonamn** med hello namnet på ditt Azure storage-konto och **kontonyckel** med hello snabbtangent av hello Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="299bc-167">Replace **account name** with hello name of your Azure storage account and **account key** with hello access key of hello Azure storage account.</span></span> <span data-ttu-id="299bc-168">toolearn hur tooget lagringen åtkomst till nyckeln, se hello information om hur tooview, kopiera och generera lagring åtkomstnycklar i [hantera ditt lagringskonto](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="299bc-168">toolearn how tooget your storage access key, see hello information about how tooview, copy, and regenerate storage access keys in [Manage your storage account](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span>
2. <span data-ttu-id="299bc-169">Växla toohello ADFGetStarted mapp i Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="299bc-169">In Azure PowerShell, switch toohello ADFGetStarted folder.</span></span>
3. <span data-ttu-id="299bc-170">Du kan använda hello **ny AzureRmDataFactoryLinkedService** cmdlet som skapar en länkad tjänst.</span><span class="sxs-lookup"><span data-stu-id="299bc-170">You can use hello **New-AzureRmDataFactoryLinkedService** cmdlet that creates a linked service.</span></span> <span data-ttu-id="299bc-171">Denna cmdlet och andra Data Factory-cmdletar som du använder i den här kursen kräver att du toopass värden för hello *ResourceGroupName* och *DataFactoryName* parametrar.</span><span class="sxs-lookup"><span data-stu-id="299bc-171">This cmdlet and other Data Factory cmdlets you use in this tutorial requires you toopass values for hello *ResourceGroupName* and *DataFactoryName* parameters.</span></span> <span data-ttu-id="299bc-172">Du kan också använda **Get-AzureRmDataFactory** tooget en **DataFactory** objekt och skickar hello objekt utan att ange *ResourceGroupName* och  *DataFactoryName* varje gång du kör en cmdlet.</span><span class="sxs-lookup"><span data-stu-id="299bc-172">Alternatively, you can use **Get-AzureRmDataFactory** tooget a **DataFactory** object and pass hello object without typing *ResourceGroupName* and *DataFactoryName* each time you run a cmdlet.</span></span> <span data-ttu-id="299bc-173">Hello kör följande kommando tooassign hello utdata från hello **Get-AzureRmDataFactory** cmdlet tooa **$df** variabeln.</span><span class="sxs-lookup"><span data-stu-id="299bc-173">Run hello following command tooassign hello output of hello **Get-AzureRmDataFactory** cmdlet tooa **$df** variable.</span></span>

    ```PowerShell
    $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name FirstDataFactoryPSH
    ```
4. <span data-ttu-id="299bc-174">Kör nu hello **ny AzureRmDataFactoryLinkedService** cmdlet som skapar hello länkade **StorageLinkedService** service.</span><span class="sxs-lookup"><span data-stu-id="299bc-174">Now, run hello **New-AzureRmDataFactoryLinkedService** cmdlet that creates hello linked **StorageLinkedService** service.</span></span>

    ```PowerShell
    New-AzureRmDataFactoryLinkedService $df -File .\StorageLinkedService.json
    ```
    <span data-ttu-id="299bc-175">Om du inte kör hello **Get-AzureRmDataFactory** cmdlet och tilldelade hello utdata toohello **$df** variabel, skulle det ha toospecify värden för hello *ResourceGroupName*och *DataFactoryName* parametrar på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="299bc-175">If you hadn't run hello **Get-AzureRmDataFactory** cmdlet and assigned hello output toohello **$df** variable, you would have toospecify values for hello *ResourceGroupName* and *DataFactoryName* parameters as follows.</span></span>

    ```PowerShell
    New-AzureRmDataFactoryLinkedService -ResourceGroupName ADFTutorialResourceGroup -DataFactoryName FirstDataFactoryPSH -File .\StorageLinkedService.json
    ```
    <span data-ttu-id="299bc-176">Om du stänger Azure PowerShell hello mitten av hello kursen har du toorun hello **Get-AzureRmDataFactory** cmdlet nästa gång du startar Azure PowerShell toocomplete hello kursen.</span><span class="sxs-lookup"><span data-stu-id="299bc-176">If you close Azure PowerShell in hello middle of hello tutorial, you have toorun hello **Get-AzureRmDataFactory** cmdlet next time you start Azure PowerShell toocomplete hello tutorial.</span></span>

### <a name="create-azure-hdinsight-linked-service"></a><span data-ttu-id="299bc-177">Skapa en Azure HDInsight-länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="299bc-177">Create Azure HDInsight linked service</span></span>
<span data-ttu-id="299bc-178">I det här steget kan länka du ett på begäran HDInsight-kluster tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="299bc-178">In this step, you link an on-demand HDInsight cluster tooyour data factory.</span></span> <span data-ttu-id="299bc-179">Hej HDInsight-kluster skapas under körning och tas bort när den är klar bearbetning och inaktiv för hello angiven tidsperiod automatiskt.</span><span class="sxs-lookup"><span data-stu-id="299bc-179">hello HDInsight cluster is automatically created at runtime and deleted after it is done processing and idle for hello specified amount of time.</span></span> <span data-ttu-id="299bc-180">Du kan använda ditt eget HDInsight-kluster i stället för att använda ett HDInsight-kluster på begäran.</span><span class="sxs-lookup"><span data-stu-id="299bc-180">You could use your own HDInsight cluster instead of using an on-demand HDInsight cluster.</span></span> <span data-ttu-id="299bc-181">Se [Beräkna länkade tjänster](data-factory-compute-linked-services.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="299bc-181">See [Compute Linked Services](data-factory-compute-linked-services.md) for details.</span></span>

1. <span data-ttu-id="299bc-182">Skapa en JSON-fil med namnet **HDInsightOnDemandLinkedService**JSON i hello **C:\ADFGetStarted** mapp med hello följande innehåll.</span><span class="sxs-lookup"><span data-stu-id="299bc-182">Create a JSON file named **HDInsightOnDemandLinkedService**.json in hello **C:\ADFGetStarted** folder with hello following content.</span></span>

    ```json
    {
        "name": "HDInsightOnDemandLinkedService",
        "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
                "version": "3.5",
                "clusterSize": 1,
                "timeToLive": "00:05:00",
                "osType": "Linux",
                "linkedServiceName": "StorageLinkedService"
            }
        }
    }
    ```
    <span data-ttu-id="299bc-183">hello innehåller följande tabell beskrivningar för hello JSON egenskaper som används i hello fragment:</span><span class="sxs-lookup"><span data-stu-id="299bc-183">hello following table provides descriptions for hello JSON properties used in hello snippet:</span></span>

   | <span data-ttu-id="299bc-184">Egenskap</span><span class="sxs-lookup"><span data-stu-id="299bc-184">Property</span></span> | <span data-ttu-id="299bc-185">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="299bc-185">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="299bc-186">ClusterSize</span><span class="sxs-lookup"><span data-stu-id="299bc-186">ClusterSize</span></span> |<span data-ttu-id="299bc-187">Anger hello storleken på hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="299bc-187">Specifies hello size of hello HDInsight cluster.</span></span> |
   | <span data-ttu-id="299bc-188">TimeToLive</span><span class="sxs-lookup"><span data-stu-id="299bc-188">TimeToLive</span></span> |<span data-ttu-id="299bc-189">Anger den inaktiva tiden hello för hello HDInsight-kluster, innan den tas bort.</span><span class="sxs-lookup"><span data-stu-id="299bc-189">Specifies that hello idle time for hello HDInsight cluster, before it is deleted.</span></span> |
   | <span data-ttu-id="299bc-190">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="299bc-190">linkedServiceName</span></span> |<span data-ttu-id="299bc-191">Anger hello storage-konto som används toostore hello loggar som genereras av HDInsight</span><span class="sxs-lookup"><span data-stu-id="299bc-191">Specifies hello storage account that is used toostore hello logs that are generated by HDInsight</span></span> |

    <span data-ttu-id="299bc-192">Observera följande punkter hello:</span><span class="sxs-lookup"><span data-stu-id="299bc-192">Note hello following points:</span></span>

   * <span data-ttu-id="299bc-193">hello Data Factory skapar en **Linux-baserade** HDInsight-kluster du hello JSON.</span><span class="sxs-lookup"><span data-stu-id="299bc-193">hello Data Factory creates a **Linux-based** HDInsight cluster for you with hello JSON.</span></span> <span data-ttu-id="299bc-194">Se [HDInsight-länkad tjänst på begäran](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) för mer information.</span><span class="sxs-lookup"><span data-stu-id="299bc-194">See [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) for details.</span></span>
   * <span data-ttu-id="299bc-195">Du kan använda **ditt eget HDInsight-kluster** i stället för att använda ett HDInsight-kluster på begäran.</span><span class="sxs-lookup"><span data-stu-id="299bc-195">You could use **your own HDInsight cluster** instead of using an on-demand HDInsight cluster.</span></span> <span data-ttu-id="299bc-196">Se [HDInsight-länkad tjänst](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) för mer information.</span><span class="sxs-lookup"><span data-stu-id="299bc-196">See [HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) for details.</span></span>
   * <span data-ttu-id="299bc-197">Hej HDInsight-kluster skapas en **standardbehållaren** i hello blob storage som du angav i hello JSON (**linkedServiceName**).</span><span class="sxs-lookup"><span data-stu-id="299bc-197">hello HDInsight cluster creates a **default container** in hello blob storage you specified in hello JSON (**linkedServiceName**).</span></span> <span data-ttu-id="299bc-198">HDInsight tar inte bort den här behållaren när hello kluster har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="299bc-198">HDInsight does not delete this container when hello cluster is deleted.</span></span> <span data-ttu-id="299bc-199">Det här beteendet är avsiktligt.</span><span class="sxs-lookup"><span data-stu-id="299bc-199">This behavior is by design.</span></span> <span data-ttu-id="299bc-200">Med en HDInsight-länkad tjänst på begäran skapas ett HDInsight-kluster varje gång en sektor bearbetas, såvida det inte finns ett befintligt live-kluster (**timeToLive**).</span><span class="sxs-lookup"><span data-stu-id="299bc-200">With on-demand HDInsight linked service, a HDInsight cluster is created every time a slice is processed unless there is an existing live cluster (**timeToLive**).</span></span> <span data-ttu-id="299bc-201">hello klustret tas bort automatiskt när hello bearbetningen är klar.</span><span class="sxs-lookup"><span data-stu-id="299bc-201">hello cluster is automatically deleted when hello processing is done.</span></span>

       <span data-ttu-id="299bc-202">Allteftersom fler sektorer bearbetas kan du se många behållare i ditt Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="299bc-202">As more slices are processed, you see many containers in your Azure blob storage.</span></span> <span data-ttu-id="299bc-203">Om du inte behöver dem för felsökning av hello jobb, kanske du vill toodelete dem tooreduce hello lagring kostnad.</span><span class="sxs-lookup"><span data-stu-id="299bc-203">If you do not need them for troubleshooting of hello jobs, you may want toodelete them tooreduce hello storage cost.</span></span> <span data-ttu-id="299bc-204">hello namnen på de här behållarna följer ett mönster ”: adf**yourdatafactoryname**-**linkedservicename**- datetimestamp”.</span><span class="sxs-lookup"><span data-stu-id="299bc-204">hello names of these containers follow a pattern: "adf**yourdatafactoryname**-**linkedservicename**-datetimestamp".</span></span> <span data-ttu-id="299bc-205">Använd verktyg som [Microsoft Lagringsutforskaren](http://storageexplorer.com/) toodelete behållare i din Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="299bc-205">Use tools such as [Microsoft Storage Explorer](http://storageexplorer.com/) toodelete containers in your Azure blob storage.</span></span>

     <span data-ttu-id="299bc-206">Se [HDInsight-länkad tjänst på begäran](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) för mer information.</span><span class="sxs-lookup"><span data-stu-id="299bc-206">See [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) for details.</span></span>
2. <span data-ttu-id="299bc-207">Kör hello **ny AzureRmDataFactoryLinkedService** cmdlet som skapar hello länkade tjänsten som kallas HDInsightOnDemandLinkedService.</span><span class="sxs-lookup"><span data-stu-id="299bc-207">Run hello **New-AzureRmDataFactoryLinkedService** cmdlet that creates hello linked service called HDInsightOnDemandLinkedService.</span></span>
    
    ```PowerShell
    New-AzureRmDataFactoryLinkedService $df -File .\HDInsightOnDemandLinkedService.json
    ```

## <a name="create-datasets"></a><span data-ttu-id="299bc-208">Skapa datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="299bc-208">Create datasets</span></span>
<span data-ttu-id="299bc-209">I det här steget Skapa datauppsättningar toorepresent hello indata och utdata för Hive-bearbetning.</span><span class="sxs-lookup"><span data-stu-id="299bc-209">In this step, you create datasets toorepresent hello input and output data for Hive processing.</span></span> <span data-ttu-id="299bc-210">De här datauppsättningarna finns toohello **StorageLinkedService** du har skapat tidigare i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="299bc-210">These datasets refer toohello **StorageLinkedService** you have created earlier in this tutorial.</span></span> <span data-ttu-id="299bc-211">Hej länkade tjänsten punkter tooan Azure Storage-konto och datauppsättningar anger behållare, mapp, filnamn i hello lagring som innehåller indata och utdata.</span><span class="sxs-lookup"><span data-stu-id="299bc-211">hello linked service points tooan Azure Storage account and datasets specify container, folder, file name in hello storage that holds input and output data.</span></span>

### <a name="create-input-dataset"></a><span data-ttu-id="299bc-212">Skapa indatauppsättning</span><span class="sxs-lookup"><span data-stu-id="299bc-212">Create input dataset</span></span>
1. <span data-ttu-id="299bc-213">Skapa en JSON-fil med namnet **InputTable.json** i hello **C:\ADFGetStarted** mapp med hello följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="299bc-213">Create a JSON file named **InputTable.json** in hello **C:\ADFGetStarted** folder with hello following content:</span></span>

    ```json
    {
        "name": "AzureBlobInput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "fileName": "input.log",
                "folderPath": "adfgetstarted/inputdata",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Month",
                "interval": 1
            },
            "external": true,
            "policy": {}
        }
    }
    ```
    <span data-ttu-id="299bc-214">hello JSON definierar en datauppsättning med namnet **AzureBlobInput**, som representerar indata för en aktivitet i hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="299bc-214">hello JSON defines a dataset named **AzureBlobInput**, which represents input data for an activity in hello pipeline.</span></span> <span data-ttu-id="299bc-215">Dessutom det anger att hello indata finns i hello blob-behållaren som kallas **adfgetstarted** och hello mapp som heter **inputdata**.</span><span class="sxs-lookup"><span data-stu-id="299bc-215">In addition, it specifies that hello input data is located in hello blob container called **adfgetstarted** and hello folder called **inputdata**.</span></span>

    <span data-ttu-id="299bc-216">hello innehåller följande tabell beskrivningar för hello JSON egenskaper som används i hello fragment:</span><span class="sxs-lookup"><span data-stu-id="299bc-216">hello following table provides descriptions for hello JSON properties used in hello snippet:</span></span>

   | <span data-ttu-id="299bc-217">Egenskap</span><span class="sxs-lookup"><span data-stu-id="299bc-217">Property</span></span> | <span data-ttu-id="299bc-218">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="299bc-218">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="299bc-219">typ</span><span class="sxs-lookup"><span data-stu-id="299bc-219">type</span></span> |<span data-ttu-id="299bc-220">hello anges typ egenskapen tooAzureBlob eftersom data finns i Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="299bc-220">hello type property is set tooAzureBlob because data resides in Azure blob storage.</span></span> |
   | <span data-ttu-id="299bc-221">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="299bc-221">linkedServiceName</span></span> |<span data-ttu-id="299bc-222">refererar toohello StorageLinkedService som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="299bc-222">refers toohello StorageLinkedService you created earlier.</span></span> |
   | <span data-ttu-id="299bc-223">fileName</span><span class="sxs-lookup"><span data-stu-id="299bc-223">fileName</span></span> |<span data-ttu-id="299bc-224">Den här egenskapen är valfri.</span><span class="sxs-lookup"><span data-stu-id="299bc-224">This property is optional.</span></span> <span data-ttu-id="299bc-225">Om du utesluter den här egenskapen har alla hello-filer från hello folderPath plockats.</span><span class="sxs-lookup"><span data-stu-id="299bc-225">If you omit this property, all hello files from hello folderPath are picked.</span></span> <span data-ttu-id="299bc-226">I det här fallet bearbetas endast hello input.log.</span><span class="sxs-lookup"><span data-stu-id="299bc-226">In this case, only hello input.log is processed.</span></span> |
   | <span data-ttu-id="299bc-227">typ</span><span class="sxs-lookup"><span data-stu-id="299bc-227">type</span></span> |<span data-ttu-id="299bc-228">hello-loggfiler finns i textformat, så vi använder TextFormat.</span><span class="sxs-lookup"><span data-stu-id="299bc-228">hello log files are in text format, so we use TextFormat.</span></span> |
   | <span data-ttu-id="299bc-229">columnDelimiter</span><span class="sxs-lookup"><span data-stu-id="299bc-229">columnDelimiter</span></span> |<span data-ttu-id="299bc-230">kolumner i hello loggfiler är avgränsade med hello kommatecken (,).</span><span class="sxs-lookup"><span data-stu-id="299bc-230">columns in hello log files are delimited by hello comma character (,).</span></span> |
   | <span data-ttu-id="299bc-231">frekvens/intervall</span><span class="sxs-lookup"><span data-stu-id="299bc-231">frequency/interval</span></span> |<span data-ttu-id="299bc-232">frekvens ange tooMonth intervall är 1, vilket innebär att hello inkommande segment är tillgängliga varje månad.</span><span class="sxs-lookup"><span data-stu-id="299bc-232">frequency set tooMonth and interval is 1, which means that hello input slices are available monthly.</span></span> |
   | <span data-ttu-id="299bc-233">extern</span><span class="sxs-lookup"><span data-stu-id="299bc-233">external</span></span> |<span data-ttu-id="299bc-234">den här egenskapen anges tootrue om hello indata inte genereras av hello Data Factory-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="299bc-234">this property is set tootrue if hello input data is not generated by hello Data Factory service.</span></span> |
2. <span data-ttu-id="299bc-235">Kör följande kommando i Azure PowerShell toocreate hello Data Factory dataset hello:</span><span class="sxs-lookup"><span data-stu-id="299bc-235">Run hello following command in Azure PowerShell toocreate hello Data Factory dataset:</span></span>

    ```PowerShell
    New-AzureRmDataFactoryDataset $df -File .\InputTable.json
    ```

### <a name="create-output-dataset"></a><span data-ttu-id="299bc-236">Skapa datauppsättning för utdata</span><span class="sxs-lookup"><span data-stu-id="299bc-236">Create output dataset</span></span>
<span data-ttu-id="299bc-237">Nu kan skapa du hello utdata dataset toorepresent hello utgående data som lagras i hello Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="299bc-237">Now, you create hello output dataset toorepresent hello output data stored in hello Azure Blob storage.</span></span>

1. <span data-ttu-id="299bc-238">Skapa en JSON-fil med namnet **OutputTable.json** i hello **C:\ADFGetStarted** mapp med hello följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="299bc-238">Create a JSON file named **OutputTable.json** in hello **C:\ADFGetStarted** folder with hello following content:</span></span>

    ```json
    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "adfgetstarted/partitioneddata",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ","
          }
        },
        "availability": {
          "frequency": "Month",
          "interval": 1
        }
      }
    }
    ```
    <span data-ttu-id="299bc-239">hello JSON definierar en datauppsättning med namnet **AzureBlobOutput**, som representerar utdata för en aktivitet i hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="299bc-239">hello JSON defines a dataset named **AzureBlobOutput**, which represents output data for an activity in hello pipeline.</span></span> <span data-ttu-id="299bc-240">Dessutom kan det anger att hello resultat lagras i hello blob-behållaren som kallas **adfgetstarted** och hello mapp som heter **partitioneddata**.</span><span class="sxs-lookup"><span data-stu-id="299bc-240">In addition, it specifies that hello results are stored in hello blob container called **adfgetstarted** and hello folder called **partitioneddata**.</span></span> <span data-ttu-id="299bc-241">Hej **tillgänglighet** avsnittet anger att hello utdatauppsättningen skapas varje månad.</span><span class="sxs-lookup"><span data-stu-id="299bc-241">hello **availability** section specifies that hello output dataset is produced on a monthly basis.</span></span>
2. <span data-ttu-id="299bc-242">Kör följande kommando i Azure PowerShell toocreate hello Data Factory dataset hello:</span><span class="sxs-lookup"><span data-stu-id="299bc-242">Run hello following command in Azure PowerShell toocreate hello Data Factory dataset:</span></span>

    ```PowerShell
    New-AzureRmDataFactoryDataset $df -File .\OutputTable.json
    ```

## <a name="create-pipeline"></a><span data-ttu-id="299bc-243">Skapa pipeline</span><span class="sxs-lookup"><span data-stu-id="299bc-243">Create pipeline</span></span>
<span data-ttu-id="299bc-244">I det här steget ska du skapa din första pipeline med en **HDInsightHive**-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="299bc-244">In this step, you create your first pipeline with a **HDInsightHive** activity.</span></span> <span data-ttu-id="299bc-245">Inkommande segmentet är tillgängliga månad (frekvens: månad, intervall: 1), utdata segment skapas varje månad och hello scheduler för hello aktiviteten också egenskapen toomonthly.</span><span class="sxs-lookup"><span data-stu-id="299bc-245">Input slice is available monthly (frequency: Month, interval: 1), output slice is produced monthly, and hello scheduler property for hello activity is also set toomonthly.</span></span> <span data-ttu-id="299bc-246">hello inställningar för hello utdatauppsättningen och hello aktivitet Schemaläggaren måste matcha.</span><span class="sxs-lookup"><span data-stu-id="299bc-246">hello settings for hello output dataset and hello activity scheduler must match.</span></span> <span data-ttu-id="299bc-247">Datamängd för utdata är för närvarande vilka enheter hello schema, så du måste skapa en datamängd för utdata även om hello aktiviteten inte producerar några utdata.</span><span class="sxs-lookup"><span data-stu-id="299bc-247">Currently, output dataset is what drives hello schedule, so you must create an output dataset even if hello activity does not produce any output.</span></span> <span data-ttu-id="299bc-248">Om hello aktiviteten inte tar några indata, kan du hoppa över skapar hello inkommande dataset.</span><span class="sxs-lookup"><span data-stu-id="299bc-248">If hello activity doesn't take any input, you can skip creating hello input dataset.</span></span> <span data-ttu-id="299bc-249">hello-egenskaper som används i följande JSON hello beskrivs hello slutet av det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="299bc-249">hello properties used in hello following JSON are explained at hello end of this section.</span></span>

1. <span data-ttu-id="299bc-250">Skapa en JSON-fil som heter MyFirstPipelinePSH.json i hello C:\ADFGetStarted mappen med hello följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="299bc-250">Create a JSON file named MyFirstPipelinePSH.json in hello C:\ADFGetStarted folder with hello following content:</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="299bc-251">Ersätt **storageaccountname** med hello namnet på ditt lagringskonto i hello JSON.</span><span class="sxs-lookup"><span data-stu-id="299bc-251">Replace **storageaccountname** with hello name of your storage account in hello JSON.</span></span>
   >
   >

    ```json
    {
        "name": "MyFirstPipeline",
        "properties": {
            "description": "My first Azure Data Factory pipeline",
            "activities": [
                {
                    "type": "HDInsightHive",
                    "typeProperties": {
                        "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                        "scriptLinkedService": "StorageLinkedService",
                        "defines": {
                            "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
                            "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"
                        }
                    },
                    "inputs": [
                        {
                            "name": "AzureBlobInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
                        }
                    ],
                    "policy": {
                        "concurrency": 1,
                        "retry": 3
                    },
                    "scheduler": {
                        "frequency": "Month",
                        "interval": 1
                    },
                    "name": "RunSampleHiveActivity",
                    "linkedServiceName": "HDInsightOnDemandLinkedService"
                }
            ],
            "start": "2017-07-01T00:00:00Z",
            "end": "2017-07-02T00:00:00Z",
            "isPaused": false
        }
    }
    ```
    <span data-ttu-id="299bc-252">Hello JSON kodutdrag skapar du en pipeline som består av en enskild aktivitet som använder Hive tooprocess Data på ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="299bc-252">In hello JSON snippet, you are creating a pipeline that consists of a single activity that uses Hive tooprocess Data on an HDInsight cluster.</span></span>

    <span data-ttu-id="299bc-253">hello Hive-skriptfil, **partitionweblogs.hql**, lagras i hello Azure storage-konto (anges av hello scriptLinkedService, kallas **StorageLinkedService**), och i **skript**  mapp i hello behållaren **adfgetstarted**.</span><span class="sxs-lookup"><span data-stu-id="299bc-253">hello Hive script file, **partitionweblogs.hql**, is stored in hello Azure storage account (specified by hello scriptLinkedService, called **StorageLinkedService**), and in **script** folder in hello container **adfgetstarted**.</span></span>

    <span data-ttu-id="299bc-254">Hej **definierar** avsnittet är används toospecify hello runtime inställningar som ska skickas toohello hive-skript som Hive konfigurationsvärden (t.ex. ${hiveconf: inputtable}, ${hiveconf:partitionedtable}).</span><span class="sxs-lookup"><span data-stu-id="299bc-254">hello **defines** section is used toospecify hello runtime settings that be passed toohello hive script as Hive configuration values (e.g ${hiveconf:inputtable}, ${hiveconf:partitionedtable}).</span></span>

    <span data-ttu-id="299bc-255">Hej **starta** och **end** egenskaper för hello pipeline anger hello aktiva perioden för hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="299bc-255">hello **start** and **end** properties of hello pipeline specifies hello active period of hello pipeline.</span></span>

    <span data-ttu-id="299bc-256">I hello aktivitets-JSON, anger du den hello Hive-skript som körs på hello beräkning som anges av hello **linkedServiceName** – **HDInsightOnDemandLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="299bc-256">In hello activity JSON, you specify that hello Hive script runs on hello compute specified by hello **linkedServiceName** – **HDInsightOnDemandLinkedService**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="299bc-257">Se ”Pipeline-JSON” i [Pipelines och aktiviteter i Azure Data Factory](data-factory-create-pipelines.md) mer information om JSON-egenskaper som används i hello exempel.</span><span class="sxs-lookup"><span data-stu-id="299bc-257">See "Pipeline JSON" in [Pipelines and activities in Azure Data Factory](data-factory-create-pipelines.md) for details about JSON properties that are used in hello example.</span></span>

2. <span data-ttu-id="299bc-258">Bekräfta att du ser hello **input.log** filen i hello **adfgetstarted/inputdata** mapp i hello Azure blob storage och kör hello efter kommandot toodeploy hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="299bc-258">Confirm that you see hello **input.log** file in hello **adfgetstarted/inputdata** folder in hello Azure blob storage, and run hello following command toodeploy hello pipeline.</span></span> <span data-ttu-id="299bc-259">Eftersom hello **starta** och **end** gånger ställs i hello senaste och **isPaused** är uppsättningen toofalse, hello pipeline (aktivitet i pipelinen hello) körs omedelbart när du har distribuerat.</span><span class="sxs-lookup"><span data-stu-id="299bc-259">Since hello **start** and **end** times are set in hello past and **isPaused** is set toofalse, hello pipeline (activity in hello pipeline) runs immediately after you deploy.</span></span>

    ```PowerShell
    New-AzureRmDataFactoryPipeline $df -File .\MyFirstPipelinePSH.json
    ```
3. <span data-ttu-id="299bc-260">Grattis, du har skapat din första pipeline med Azure PowerShell!</span><span class="sxs-lookup"><span data-stu-id="299bc-260">Congratulations, you have successfully created your first pipeline using Azure PowerShell!</span></span>

## <a name="monitor-pipeline"></a><span data-ttu-id="299bc-261">Övervaka pipeline</span><span class="sxs-lookup"><span data-stu-id="299bc-261">Monitor pipeline</span></span>
<span data-ttu-id="299bc-262">I det här steget använder du Azure PowerShell toomonitor vad som händer i ett Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="299bc-262">In this step, you use Azure PowerShell toomonitor what’s going on in an Azure data factory.</span></span>

1. <span data-ttu-id="299bc-263">Kör **Get-AzureRmDataFactory** och tilldela hello utdata tooa **$df** variabeln.</span><span class="sxs-lookup"><span data-stu-id="299bc-263">Run **Get-AzureRmDataFactory** and assign hello output tooa **$df** variable.</span></span>

    ```PowerShell
    $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name FirstDataFactoryPSH
    ```
2. <span data-ttu-id="299bc-264">Kör **Get-AzureRmDataFactorySlice** tooget information om alla segment av hello **EmpSQLTable**, vilket är hello utdatatabellen för hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="299bc-264">Run **Get-AzureRmDataFactorySlice** tooget details about all slices of hello **EmpSQLTable**, which is hello output table of hello pipeline.</span></span>

    ```PowerShell
    Get-AzureRmDataFactorySlice $df -DatasetName AzureBlobOutput -StartDateTime 2017-07-01
    ```
    <span data-ttu-id="299bc-265">Observera att hello StartDateTime som du anger här hello samma starttid har angetts i hello pipeline-JSON.</span><span class="sxs-lookup"><span data-stu-id="299bc-265">Notice that hello StartDateTime you specify here is hello same start time specified in hello pipeline JSON.</span></span> <span data-ttu-id="299bc-266">Här är exempel på utdata från hello:</span><span class="sxs-lookup"><span data-stu-id="299bc-266">Here is hello sample output:</span></span>

    ```PowerShell
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : FirstDataFactoryPSH
    DatasetName       : AzureBlobOutput
    Start             : 7/1/2017 12:00:00 AM
    End               : 7/2/2017 12:00:00 AM
    RetryCount        : 0
    State             : InProgress
    SubState          :
    LatencyStatus     :
    LongRetryCount    : 0
    ```
3. <span data-ttu-id="299bc-267">Kör **Get-AzureRmDataFactoryRun** tooget hello information om aktivitet körs för ett visst segment.</span><span class="sxs-lookup"><span data-stu-id="299bc-267">Run **Get-AzureRmDataFactoryRun** tooget hello details of activity runs for a specific slice.</span></span>

    ```PowerShell
    Get-AzureRmDataFactoryRun $df -DatasetName AzureBlobOutput -StartDateTime 2017-07-01
    ```

    <span data-ttu-id="299bc-268">Här är exempel på utdata från hello:</span><span class="sxs-lookup"><span data-stu-id="299bc-268">Here is hello sample output:</span></span> 

    ```PowerShell
    Id                  : 0f6334f2-d56c-4d48-b427-d4f0fb4ef883_635268096000000000_635292288000000000_AzureBlobOutput
    ResourceGroupName   : ADFTutorialResourceGroup
    DataFactoryName     : FirstDataFactoryPSH
    DatasetName         : AzureBlobOutput
    ProcessingStartTime : 12/18/2015 4:50:33 AM
    ProcessingEndTime   : 12/31/9999 11:59:59 PM
    PercentComplete     : 0
    DataSliceStart      : 7/1/2017 12:00:00 AM
    DataSliceEnd        : 7/2/2017 12:00:00 AM
    Status              : AllocatingResources
    Timestamp           : 12/18/2015 4:50:33 AM
    RetryAttempt        : 0
    Properties          : {}
    ErrorMessage        :
    ActivityName        : RunSampleHiveActivity
    PipelineName        : MyFirstPipeline
    Type                : Script
    ```
    <span data-ttu-id="299bc-269">Du kan hålla köra denna cmdlet tills du ser hello sektor i **klar** tillstånd eller **misslyckades** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="299bc-269">You can keep running this cmdlet until you see hello slice in **Ready** state or **Failed** state.</span></span> <span data-ttu-id="299bc-270">När hello sektorn är i tillståndet Ready, kontrollera hello **partitioneddata** mapp i hello **adfgetstarted** behållare i blobblagring för hello utdata.</span><span class="sxs-lookup"><span data-stu-id="299bc-270">When hello slice is in Ready state, check hello **partitioneddata** folder in hello **adfgetstarted** container in your blob storage for hello output data.</span></span>  <span data-ttu-id="299bc-271">Det kan ta lite längre tid att skapa ett HDInsight-kluster på begäran.</span><span class="sxs-lookup"><span data-stu-id="299bc-271">Creation of an on-demand HDInsight cluster usually takes some time.</span></span>

    ![utdata](./media/data-factory-build-your-first-pipeline-using-powershell/three-ouptut-files.png)

> [!IMPORTANT]
> <span data-ttu-id="299bc-273">Att skapa ett HDInsight-kluster på begäran kan ta lite längre tid (cirka 20 minuter).</span><span class="sxs-lookup"><span data-stu-id="299bc-273">Creation of an on-demand HDInsight cluster usually takes sometime (approximately 20 minutes).</span></span> <span data-ttu-id="299bc-274">Därför förvänta sig hello pipeline tootake **cirka 30 minuter** tooprocess hello sektorn.</span><span class="sxs-lookup"><span data-stu-id="299bc-274">Therefore, expect hello pipeline tootake **approximately 30 minutes** tooprocess hello slice.</span></span>
>
> <span data-ttu-id="299bc-275">hello indatafilen hämtar bort när hello segment har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="299bc-275">hello input file gets deleted when hello slice is processed successfully.</span></span> <span data-ttu-id="299bc-276">Om du vill toorerun hello segment eller hello kursen igen överför därför hello indatafilen (input.log) toohello inputdata mapp för hello adfgetstarted behållare.</span><span class="sxs-lookup"><span data-stu-id="299bc-276">Therefore, if you want toorerun hello slice or do hello tutorial again, upload hello input file (input.log) toohello inputdata folder of hello adfgetstarted container.</span></span>
>
>

## <a name="summary"></a><span data-ttu-id="299bc-277">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="299bc-277">Summary</span></span>
<span data-ttu-id="299bc-278">I kursen får skapat du ett Azure data factory tooprocess data genom att köra Hive-skript på ett HDInsight hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="299bc-278">In this tutorial, you created an Azure data factory tooprocess data by running Hive script on a HDInsight hadoop cluster.</span></span> <span data-ttu-id="299bc-279">Du har använt hello Data Factory-redigeraren i hello Azure portal toodo hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="299bc-279">You used hello Data Factory Editor in hello Azure portal toodo hello following steps:</span></span>

1. <span data-ttu-id="299bc-280">Du skapade en Azure **Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="299bc-280">Created an Azure **data factory**.</span></span>
2. <span data-ttu-id="299bc-281">Du skapade två **länkade tjänster**:</span><span class="sxs-lookup"><span data-stu-id="299bc-281">Created two **linked services**:</span></span>
   1. <span data-ttu-id="299bc-282">**Azure Storage** länkade tjänsten toolink din Azure blob-lagring som innehåller in-/ utdata filer toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="299bc-282">**Azure Storage** linked service toolink your Azure blob storage that holds input/output files toohello data factory.</span></span>
   2. <span data-ttu-id="299bc-283">**Azure HDInsight** på begäran länkade tjänsten toolink en på begäran HDInsight Hadoop-kluster toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="299bc-283">**Azure HDInsight** on-demand linked service toolink an on-demand HDInsight Hadoop cluster toohello data factory.</span></span> <span data-ttu-id="299bc-284">Azure Data Factory skapar ett HDInsight Hadoop klustret just-in-time tooprocess indata och utdata för produkten.</span><span class="sxs-lookup"><span data-stu-id="299bc-284">Azure Data Factory creates a HDInsight Hadoop cluster just-in-time tooprocess input data and produce output data.</span></span>
3. <span data-ttu-id="299bc-285">Skapa två **datauppsättningar**, som beskriver inkommande och utgående data för HDInsight Hive aktivitet i hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="299bc-285">Created two **datasets**, which describe input and output data for HDInsight Hive activity in hello pipeline.</span></span>
4. <span data-ttu-id="299bc-286">Du skapade en **pipeline** med en **HDInsight Hive**-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="299bc-286">Created a **pipeline** with a **HDInsight Hive** activity.</span></span>

## <a name="next-steps"></a><span data-ttu-id="299bc-287">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="299bc-287">Next steps</span></span>
<span data-ttu-id="299bc-288">I den här artikeln har du skapat en pipeline med en transformeringsaktivitet (HDInsight-aktivitet) som kör ett Hive-skript på ett Azure HDInsight-kluster på begäran.</span><span class="sxs-lookup"><span data-stu-id="299bc-288">In this article, you have created a pipeline with a transformation activity (HDInsight Activity) that runs a Hive script on an on-demand Azure HDInsight cluster.</span></span> <span data-ttu-id="299bc-289">hur toouse en Kopieringsaktiviteten toocopy data från ett Azure Blob-tooAzure SQL, se toosee [Självstudier: kopiera data från ett Azure Blob-tooAzure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="299bc-289">toosee how toouse a Copy Activity toocopy data from an Azure Blob tooAzure SQL, see [Tutorial: Copy data from an Azure Blob tooAzure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="299bc-290">Se även</span><span class="sxs-lookup"><span data-stu-id="299bc-290">See Also</span></span>
| <span data-ttu-id="299bc-291">Avsnitt</span><span class="sxs-lookup"><span data-stu-id="299bc-291">Topic</span></span> | <span data-ttu-id="299bc-292">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="299bc-292">Description</span></span> |
|:--- |:--- |
| [<span data-ttu-id="299bc-293">Cmdlet-referens för Data Factory</span><span class="sxs-lookup"><span data-stu-id="299bc-293">Data Factory Cmdlet Reference</span></span>](/powershell/module/azurerm.datafactories) |<span data-ttu-id="299bc-294">Se den omfattande dokumentationen för Data Factory-cmdletar</span><span class="sxs-lookup"><span data-stu-id="299bc-294">See comprehensive documentation on Data Factory cmdlets</span></span> |
| [<span data-ttu-id="299bc-295">Pipelines</span><span class="sxs-lookup"><span data-stu-id="299bc-295">Pipelines</span></span>](data-factory-create-pipelines.md) |<span data-ttu-id="299bc-296">Den här artikeln hjälper dig att förstå pipelines och aktiviteter i Azure Data Factory och hur toouse dem tooconstruct slutpunkt till slutpunkt datadrivna arbetsflöden för din scenario eller ditt företag.</span><span class="sxs-lookup"><span data-stu-id="299bc-296">This article helps you understand pipelines and activities in Azure Data Factory and how toouse them tooconstruct end-to-end data-driven workflows for your scenario or business.</span></span> |
| [<span data-ttu-id="299bc-297">Datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="299bc-297">Datasets</span></span>](data-factory-create-datasets.md) |<span data-ttu-id="299bc-298">I den här artikeln förklaras hur datauppsättningar fungerar i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="299bc-298">This article helps you understand datasets in Azure Data Factory.</span></span> |
| [<span data-ttu-id="299bc-299">Schemaläggning och körning</span><span class="sxs-lookup"><span data-stu-id="299bc-299">Scheduling and Execution</span></span>](data-factory-scheduling-and-execution.md) |<span data-ttu-id="299bc-300">Den här artikeln förklarar hello schemaläggning och körning av aspekter av Azure Data Factory programmodell.</span><span class="sxs-lookup"><span data-stu-id="299bc-300">This article explains hello scheduling and execution aspects of Azure Data Factory application model.</span></span> |
| [<span data-ttu-id="299bc-301">Övervaka och hantera pipelines med övervakningsappen</span><span class="sxs-lookup"><span data-stu-id="299bc-301">Monitor and manage pipelines using Monitoring App</span></span>](data-factory-monitor-manage-app.md) |<span data-ttu-id="299bc-302">Den här artikeln beskriver hur toomonitor, hantera och felsöka pipelines med hello övervakning & Management-appen.</span><span class="sxs-lookup"><span data-stu-id="299bc-302">This article describes how toomonitor, manage, and debug pipelines using hello Monitoring & Management App.</span></span> |
