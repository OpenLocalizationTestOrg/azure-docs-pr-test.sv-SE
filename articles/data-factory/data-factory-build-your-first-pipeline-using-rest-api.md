---
title: "aaaBuild din första data factory (REST) | Microsoft Docs"
description: "I den här självstudiekursen ska du skapa en Azure Data Factory-exempelpipeline med hjälp av REST-API:et för Data Factory."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 7e0a2465-2d85-4143-a4bb-42e03c273097
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 5d8e39bd598cca35f91d501bad74e8a8436f8f89
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-build-your-first-azure-data-factory-using-data-factory-rest-api"></a><span data-ttu-id="979b5-103">Självstudier: Skapa din första Azure-datafabrik med hjälp av REST-API:et för Data Factory</span><span class="sxs-lookup"><span data-stu-id="979b5-103">Tutorial: Build your first Azure data factory using Data Factory REST API</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="979b5-104">Översikt och förutsättningar</span><span class="sxs-lookup"><span data-stu-id="979b5-104">Overview and prerequisites</span></span>](data-factory-build-your-first-pipeline.md)
> * [<span data-ttu-id="979b5-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="979b5-105">Azure portal</span></span>](data-factory-build-your-first-pipeline-using-editor.md)
> * [<span data-ttu-id="979b5-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="979b5-106">Visual Studio</span></span>](data-factory-build-your-first-pipeline-using-vs.md)
> * [<span data-ttu-id="979b5-107">PowerShell</span><span class="sxs-lookup"><span data-stu-id="979b5-107">PowerShell</span></span>](data-factory-build-your-first-pipeline-using-powershell.md)
> * [<span data-ttu-id="979b5-108">Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="979b5-108">Resource Manager Template</span></span>](data-factory-build-your-first-pipeline-using-arm.md)
> * [<span data-ttu-id="979b5-109">REST-API</span><span class="sxs-lookup"><span data-stu-id="979b5-109">REST API</span></span>](data-factory-build-your-first-pipeline-using-rest-api.md)
>
>


<span data-ttu-id="979b5-110">I den här artikeln använder du Data Factory REST API: et toocreate din första Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="979b5-110">In this article, you use Data Factory REST API toocreate your first Azure data factory.</span></span> <span data-ttu-id="979b5-111">toodo hello självstudier med andra verktyg/SDK: er, Välj ett alternativ för hello hello listrutan.</span><span class="sxs-lookup"><span data-stu-id="979b5-111">toodo hello tutorial using other tools/SDKs, select one of hello options from hello drop-down list.</span></span>

<span data-ttu-id="979b5-112">hello pipeline i den här självstudiekursen har en aktivitet: **HDInsight Hive aktiviteten**.</span><span class="sxs-lookup"><span data-stu-id="979b5-112">hello pipeline in this tutorial has one activity: **HDInsight Hive activity**.</span></span> <span data-ttu-id="979b5-113">Den här aktiviteten körs en hive-skript på ett Azure HDInsight-kluster att transformeringar inkommande data tooproduce utdata.</span><span class="sxs-lookup"><span data-stu-id="979b5-113">This activity runs a hive script on an Azure HDInsight cluster that transforms input data tooproduce output data.</span></span> <span data-ttu-id="979b5-114">hello pipeline är schemalagda toorun när en månad mellan hello angivna start- och sluttider.</span><span class="sxs-lookup"><span data-stu-id="979b5-114">hello pipeline is scheduled toorun once a month between hello specified start and end times.</span></span>

> [!NOTE]
> <span data-ttu-id="979b5-115">Den här artikeln täcker inte alla hello REST API.</span><span class="sxs-lookup"><span data-stu-id="979b5-115">This article does not cover all hello REST API.</span></span> <span data-ttu-id="979b5-116">Omfattande dokumentation om REST API finns i [referensen för REST-API för Data Factory](/rest/api/datafactory/).</span><span class="sxs-lookup"><span data-stu-id="979b5-116">For comprehensive documentation on REST API, see [Data Factory REST API Reference](/rest/api/datafactory/).</span></span>
> 
> <span data-ttu-id="979b5-117">En pipeline kan ha fler än en aktivitet.</span><span class="sxs-lookup"><span data-stu-id="979b5-117">A pipeline can have more than one activity.</span></span> <span data-ttu-id="979b5-118">Och du kan länka två aktiviteter (köra en aktivitet efter ett annat) genom att ange hello datamängd för utdata för en aktivitet som hello indatauppsättning av hello andra aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="979b5-118">And, you can chain two activities (run one activity after another) by setting hello output dataset of one activity as hello input dataset of hello other activity.</span></span> <span data-ttu-id="979b5-119">Mer detaljerad information finns i [Scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) (Schemaläggning och utförande i Data Factory).</span><span class="sxs-lookup"><span data-stu-id="979b5-119">For more information, see [scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="979b5-120">Krav</span><span class="sxs-lookup"><span data-stu-id="979b5-120">Prerequisites</span></span>
* <span data-ttu-id="979b5-121">Läs igenom [kursen översikt](data-factory-build-your-first-pipeline.md) artikeln och fullständig hello **nödvändiga** steg.</span><span class="sxs-lookup"><span data-stu-id="979b5-121">Read through [Tutorial Overview](data-factory-build-your-first-pipeline.md) article and complete hello **prerequisite** steps.</span></span>
* <span data-ttu-id="979b5-122">Installera [Curl](https://curl.haxx.se/dlwiz/) på din dator.</span><span class="sxs-lookup"><span data-stu-id="979b5-122">Install [Curl](https://curl.haxx.se/dlwiz/) on your machine.</span></span> <span data-ttu-id="979b5-123">Verktyget hello CURL med övriga kommandon toocreate en datafabrik.</span><span class="sxs-lookup"><span data-stu-id="979b5-123">You use hello CURL tool with REST commands toocreate a data factory.</span></span>
* <span data-ttu-id="979b5-124">Gör följande genom att följa anvisningarna i [den här artikeln](../azure-resource-manager/resource-group-create-service-principal-portal.md):</span><span class="sxs-lookup"><span data-stu-id="979b5-124">Follow instructions from [this article](../azure-resource-manager/resource-group-create-service-principal-portal.md) to:</span></span>
  1. <span data-ttu-id="979b5-125">Skapa en webbapp med namnet **ADFGetStartedApp** i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="979b5-125">Create a Web application named **ADFGetStartedApp** in Azure Active Directory.</span></span>
  2. <span data-ttu-id="979b5-126">Hämta ett **klient-ID** och en **hemlig nyckel**.</span><span class="sxs-lookup"><span data-stu-id="979b5-126">Get **client ID** and **secret key**.</span></span>
  3. <span data-ttu-id="979b5-127">Hämta ett **klientorganisations-ID**.</span><span class="sxs-lookup"><span data-stu-id="979b5-127">Get **tenant ID**.</span></span>
  4. <span data-ttu-id="979b5-128">Tilldela hello **ADFGetStartedApp** programmet toohello **Data Factory deltagare** roll.</span><span class="sxs-lookup"><span data-stu-id="979b5-128">Assign hello **ADFGetStartedApp** application toohello **Data Factory Contributor** role.</span></span>
* <span data-ttu-id="979b5-129">[Installera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="979b5-129">Install [Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="979b5-130">Starta **PowerShell** och hello kör följande kommando.</span><span class="sxs-lookup"><span data-stu-id="979b5-130">Launch **PowerShell** and run hello following command.</span></span> <span data-ttu-id="979b5-131">Behåll Azure PowerShell öppen tills hello slutet av den här kursen.</span><span class="sxs-lookup"><span data-stu-id="979b5-131">Keep Azure PowerShell open until hello end of this tutorial.</span></span> <span data-ttu-id="979b5-132">Om du stänga och öppna måste toorun hello kommandon igen.</span><span class="sxs-lookup"><span data-stu-id="979b5-132">If you close and reopen, you need toorun hello commands again.</span></span>
  1. <span data-ttu-id="979b5-133">Kör **Login-AzureRmAccount** och ange hello användarnamn och lösenord som du använder toosign i toohello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="979b5-133">Run **Login-AzureRmAccount** and enter hello user name and password that you use toosign in toohello Azure portal.</span></span>
  2. <span data-ttu-id="979b5-134">Kör **Get-AzureRmSubscription** tooview alla hello prenumerationer för det här kontot.</span><span class="sxs-lookup"><span data-stu-id="979b5-134">Run **Get-AzureRmSubscription** tooview all hello subscriptions for this account.</span></span>
  3. <span data-ttu-id="979b5-135">Kör **Get-AzureRmSubscription - SubscriptionName NameOfAzureSubscription | Set-AzureRmContext** tooselect hello prenumeration som du vill toowork med.</span><span class="sxs-lookup"><span data-stu-id="979b5-135">Run **Get-AzureRmSubscription -SubscriptionName NameOfAzureSubscription | Set-AzureRmContext** tooselect hello subscription that you want toowork with.</span></span> <span data-ttu-id="979b5-136">Ersätt **NameOfAzureSubscription** med hello namnet på din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="979b5-136">Replace **NameOfAzureSubscription** with hello name of your Azure subscription.</span></span>
* <span data-ttu-id="979b5-137">Skapa en Azure-resursgrupp med namnet **ADFTutorialResourceGroup** genom att köra följande kommando i hello PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="979b5-137">Create an Azure resource group named **ADFTutorialResourceGroup** by running hello following command in hello PowerShell:</span></span>

    ```PowerShell
    New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```

   <span data-ttu-id="979b5-138">Några av hello stegen i den här självstudiekursen förutsätts att du använder hello resursgrupp med namnet ADFTutorialResourceGroup.</span><span class="sxs-lookup"><span data-stu-id="979b5-138">Some of hello steps in this tutorial assume that you use hello resource group named ADFTutorialResourceGroup.</span></span> <span data-ttu-id="979b5-139">Om du använder en annan resursgrupp måste toouse hello namnet på resursgruppen i stället för ADFTutorialResourceGroup i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="979b5-139">If you use a different resource group, you need toouse hello name of your resource group in place of ADFTutorialResourceGroup in this tutorial.</span></span>

## <a name="create-json-definitions"></a><span data-ttu-id="979b5-140">Skapa JSON-definitioner</span><span class="sxs-lookup"><span data-stu-id="979b5-140">Create JSON definitions</span></span>
<span data-ttu-id="979b5-141">Skapa följande JSON-filer i hello mapp där curl.exe finns.</span><span class="sxs-lookup"><span data-stu-id="979b5-141">Create following JSON files in hello folder where curl.exe is located.</span></span>

### <a name="datafactoryjson"></a><span data-ttu-id="979b5-142">datafactory.json</span><span class="sxs-lookup"><span data-stu-id="979b5-142">datafactory.json</span></span>
> [!IMPORTANT]
> <span data-ttu-id="979b5-143">Namnet måste vara globalt unika, så du kanske vill tooprefix-suffix ADFCopyTutorialDF toomake den ett unikt namn.</span><span class="sxs-lookup"><span data-stu-id="979b5-143">Name must be globally unique, so you may want tooprefix/suffix ADFCopyTutorialDF toomake it a unique name.</span></span>
>
>

```JSON
{
    "name": "FirstDataFactoryREST",
    "location": "WestUS"
}
```

### <a name="azurestoragelinkedservicejson"></a><span data-ttu-id="979b5-144">azurestoragelinkedservice.json</span><span class="sxs-lookup"><span data-stu-id="979b5-144">azurestoragelinkedservice.json</span></span>
> [!IMPORTANT]
> <span data-ttu-id="979b5-145">Ersätt **accountname** och **accountkey** med namnet och nyckeln för ditt Azure-lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="979b5-145">Replace **accountname** and **accountkey** with name and key of your Azure storage account.</span></span> <span data-ttu-id="979b5-146">toolearn hur tooget lagringen åtkomst till nyckeln, se hello information om hur tooview, kopiera och generera lagring åtkomstnycklar i [hantera ditt lagringskonto](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="979b5-146">toolearn how tooget your storage access key, see hello information about how tooview, copy, and regenerate storage access keys in [Manage your storage account](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span>
>
>

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

### <a name="hdinsightondemandlinkedservicejson"></a><span data-ttu-id="979b5-147">hdinsightondemandlinkedservice.json</span><span class="sxs-lookup"><span data-stu-id="979b5-147">hdinsightondemandlinkedservice.json</span></span>

```JSON
{
    "name": "HDInsightOnDemandLinkedService",
    "properties": {
        "type": "HDInsightOnDemand",
        "typeProperties": {
            "version": "3.5",
            "clusterSize": 1,
            "timeToLive": "00:05:00",
            "osType": "Linux",
            "linkedServiceName": "AzureStorageLinkedService"
        }
    }
}
```

<span data-ttu-id="979b5-148">hello innehåller följande tabell beskrivningar för hello JSON egenskaper som används i hello fragment:</span><span class="sxs-lookup"><span data-stu-id="979b5-148">hello following table provides descriptions for hello JSON properties used in hello snippet:</span></span>

| <span data-ttu-id="979b5-149">Egenskap</span><span class="sxs-lookup"><span data-stu-id="979b5-149">Property</span></span> | <span data-ttu-id="979b5-150">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="979b5-150">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="979b5-151">ClusterSize</span><span class="sxs-lookup"><span data-stu-id="979b5-151">ClusterSize</span></span> |<span data-ttu-id="979b5-152">Storleken på hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="979b5-152">Size of hello HDInsight cluster.</span></span> |
| <span data-ttu-id="979b5-153">TimeToLive</span><span class="sxs-lookup"><span data-stu-id="979b5-153">TimeToLive</span></span> |<span data-ttu-id="979b5-154">Anger den inaktiva tiden hello för hello HDInsight-kluster, innan den tas bort.</span><span class="sxs-lookup"><span data-stu-id="979b5-154">Specifies that hello idle time for hello HDInsight cluster, before it is deleted.</span></span> |
| <span data-ttu-id="979b5-155">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="979b5-155">linkedServiceName</span></span> |<span data-ttu-id="979b5-156">Anger hello storage-konto som används toostore hello loggar som genereras av HDInsight</span><span class="sxs-lookup"><span data-stu-id="979b5-156">Specifies hello storage account that is used toostore hello logs that are generated by HDInsight</span></span> |

<span data-ttu-id="979b5-157">Observera följande punkter hello:</span><span class="sxs-lookup"><span data-stu-id="979b5-157">Note hello following points:</span></span>

* <span data-ttu-id="979b5-158">hello Data Factory skapar en **Linux-baserade** HDInsight-kluster du hello ovan JSON.</span><span class="sxs-lookup"><span data-stu-id="979b5-158">hello Data Factory creates a **Linux-based** HDInsight cluster for you with hello above JSON.</span></span> <span data-ttu-id="979b5-159">Se [HDInsight-länkad tjänst på begäran](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) för mer information.</span><span class="sxs-lookup"><span data-stu-id="979b5-159">See [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) for details.</span></span>
* <span data-ttu-id="979b5-160">Du kan använda **ditt eget HDInsight-kluster** i stället för att använda ett HDInsight-kluster på begäran.</span><span class="sxs-lookup"><span data-stu-id="979b5-160">You could use **your own HDInsight cluster** instead of using an on-demand HDInsight cluster.</span></span> <span data-ttu-id="979b5-161">Se [HDInsight-länkad tjänst](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) för mer information.</span><span class="sxs-lookup"><span data-stu-id="979b5-161">See [HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) for details.</span></span>
* <span data-ttu-id="979b5-162">Hej HDInsight-kluster skapas en **standardbehållaren** i hello blob storage som du angav i hello JSON (**linkedServiceName**).</span><span class="sxs-lookup"><span data-stu-id="979b5-162">hello HDInsight cluster creates a **default container** in hello blob storage you specified in hello JSON (**linkedServiceName**).</span></span> <span data-ttu-id="979b5-163">HDInsight tar inte bort den här behållaren när hello kluster har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="979b5-163">HDInsight does not delete this container when hello cluster is deleted.</span></span> <span data-ttu-id="979b5-164">Det här beteendet är avsiktligt.</span><span class="sxs-lookup"><span data-stu-id="979b5-164">This behavior is by design.</span></span> <span data-ttu-id="979b5-165">Med länkad HDInsight-tjänsten på begäran, ett HDInsight-kluster skapas varje gång en sektor bearbetas såvida det inte finns ett befintligt live kluster (**timeToLive**) och tas bort när hello bearbetningen är klar.</span><span class="sxs-lookup"><span data-stu-id="979b5-165">With on-demand HDInsight linked service, a HDInsight cluster is created every time a slice is processed unless there is an existing live cluster (**timeToLive**) and is deleted when hello processing is done.</span></span>

    <span data-ttu-id="979b5-166">Allteftersom fler sektorer bearbetas kan du se många behållare i ditt Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="979b5-166">As more slices are processed, you see many containers in your Azure blob storage.</span></span> <span data-ttu-id="979b5-167">Om du inte behöver dem för felsökning av hello jobb, kanske du vill toodelete dem tooreduce hello lagring kostnad.</span><span class="sxs-lookup"><span data-stu-id="979b5-167">If you do not need them for troubleshooting of hello jobs, you may want toodelete them tooreduce hello storage cost.</span></span> <span data-ttu-id="979b5-168">hello namnen på de här behållarna följer ett mönster ”: adf**yourdatafactoryname**-**linkedservicename**- datetimestamp”.</span><span class="sxs-lookup"><span data-stu-id="979b5-168">hello names of these containers follow a pattern: "adf**yourdatafactoryname**-**linkedservicename**-datetimestamp".</span></span> <span data-ttu-id="979b5-169">Använd verktyg som [Microsoft Lagringsutforskaren](http://storageexplorer.com/) toodelete behållare i din Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="979b5-169">Use tools such as [Microsoft Storage Explorer](http://storageexplorer.com/) toodelete containers in your Azure blob storage.</span></span>

<span data-ttu-id="979b5-170">Se [HDInsight-länkad tjänst på begäran](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) för mer information.</span><span class="sxs-lookup"><span data-stu-id="979b5-170">See [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) for details.</span></span>

### <a name="inputdatasetjson"></a><span data-ttu-id="979b5-171">inputdataset.json</span><span class="sxs-lookup"><span data-stu-id="979b5-171">inputdataset.json</span></span>

```JSON
{
    "name": "AzureBlobInput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
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

<span data-ttu-id="979b5-172">hello JSON definierar en datauppsättning med namnet **AzureBlobInput**, som representerar indata för en aktivitet i hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="979b5-172">hello JSON defines a dataset named **AzureBlobInput**, which represents input data for an activity in hello pipeline.</span></span> <span data-ttu-id="979b5-173">Dessutom det anger att hello indata finns i hello blob-behållaren som kallas **adfgetstarted** och hello mapp som heter **inputdata**.</span><span class="sxs-lookup"><span data-stu-id="979b5-173">In addition, it specifies that hello input data is located in hello blob container called **adfgetstarted** and hello folder called **inputdata**.</span></span>

<span data-ttu-id="979b5-174">hello innehåller följande tabell beskrivningar för hello JSON egenskaper som används i hello fragment:</span><span class="sxs-lookup"><span data-stu-id="979b5-174">hello following table provides descriptions for hello JSON properties used in hello snippet:</span></span>

| <span data-ttu-id="979b5-175">Egenskap</span><span class="sxs-lookup"><span data-stu-id="979b5-175">Property</span></span> | <span data-ttu-id="979b5-176">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="979b5-176">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="979b5-177">typ</span><span class="sxs-lookup"><span data-stu-id="979b5-177">type</span></span> |<span data-ttu-id="979b5-178">hello anges typ egenskapen tooAzureBlob eftersom data finns i Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="979b5-178">hello type property is set tooAzureBlob because data resides in Azure blob storage.</span></span> |
| <span data-ttu-id="979b5-179">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="979b5-179">linkedServiceName</span></span> |<span data-ttu-id="979b5-180">refererar toohello StorageLinkedService som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="979b5-180">refers toohello StorageLinkedService you created earlier.</span></span> |
| <span data-ttu-id="979b5-181">fileName</span><span class="sxs-lookup"><span data-stu-id="979b5-181">fileName</span></span> |<span data-ttu-id="979b5-182">Den här egenskapen är valfri.</span><span class="sxs-lookup"><span data-stu-id="979b5-182">This property is optional.</span></span> <span data-ttu-id="979b5-183">Om du utesluter den här egenskapen har alla hello-filer från hello folderPath plockats.</span><span class="sxs-lookup"><span data-stu-id="979b5-183">If you omit this property, all hello files from hello folderPath are picked.</span></span> <span data-ttu-id="979b5-184">I det här fallet bearbetas endast hello input.log.</span><span class="sxs-lookup"><span data-stu-id="979b5-184">In this case, only hello input.log is processed.</span></span> |
| <span data-ttu-id="979b5-185">typ</span><span class="sxs-lookup"><span data-stu-id="979b5-185">type</span></span> |<span data-ttu-id="979b5-186">hello-loggfiler finns i textformat, så vi använder TextFormat.</span><span class="sxs-lookup"><span data-stu-id="979b5-186">hello log files are in text format, so we use TextFormat.</span></span> |
| <span data-ttu-id="979b5-187">columnDelimiter</span><span class="sxs-lookup"><span data-stu-id="979b5-187">columnDelimiter</span></span> |<span data-ttu-id="979b5-188">kolumner i hello loggfiler är avgränsade med semikolon kommatecken ()</span><span class="sxs-lookup"><span data-stu-id="979b5-188">columns in hello log files are delimited by a comma character (,)</span></span> |
| <span data-ttu-id="979b5-189">frekvens/intervall</span><span class="sxs-lookup"><span data-stu-id="979b5-189">frequency/interval</span></span> |<span data-ttu-id="979b5-190">frekvens ange tooMonth intervall är 1, vilket innebär att hello inkommande segment är tillgängliga varje månad.</span><span class="sxs-lookup"><span data-stu-id="979b5-190">frequency set tooMonth and interval is 1, which means that hello input slices are available monthly.</span></span> |
| <span data-ttu-id="979b5-191">extern</span><span class="sxs-lookup"><span data-stu-id="979b5-191">external</span></span> |<span data-ttu-id="979b5-192">den här egenskapen anges tootrue om hello indata inte genereras av hello Data Factory-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="979b5-192">this property is set tootrue if hello input data is not generated by hello Data Factory service.</span></span> |

### <a name="outputdatasetjson"></a><span data-ttu-id="979b5-193">outputdataset.json</span><span class="sxs-lookup"><span data-stu-id="979b5-193">outputdataset.json</span></span>

```JSON
{
    "name": "AzureBlobOutput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
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

<span data-ttu-id="979b5-194">hello JSON definierar en datauppsättning med namnet **AzureBlobOutput**, som representerar utdata för en aktivitet i hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="979b5-194">hello JSON defines a dataset named **AzureBlobOutput**, which represents output data for an activity in hello pipeline.</span></span> <span data-ttu-id="979b5-195">Dessutom kan det anger att hello resultat lagras i hello blob-behållaren som kallas **adfgetstarted** och hello mapp som heter **partitioneddata**.</span><span class="sxs-lookup"><span data-stu-id="979b5-195">In addition, it specifies that hello results are stored in hello blob container called **adfgetstarted** and hello folder called **partitioneddata**.</span></span> <span data-ttu-id="979b5-196">Hej **tillgänglighet** avsnittet anger att hello utdatauppsättningen skapas varje månad.</span><span class="sxs-lookup"><span data-stu-id="979b5-196">hello **availability** section specifies that hello output dataset is produced on a monthly basis.</span></span>

### <a name="pipelinejson"></a><span data-ttu-id="979b5-197">pipeline.json</span><span class="sxs-lookup"><span data-stu-id="979b5-197">pipeline.json</span></span>
> [!IMPORTANT]
> <span data-ttu-id="979b5-198">Ersätt **storageaccountname** med namnet på ditt Azure-lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="979b5-198">Replace **storageaccountname** with name of your Azure storage account.</span></span>
>
>

```JSON
{
    "name": "MyFirstPipeline",
    "properties": {
        "description": "My first Azure Data Factory pipeline",
        "activities": [{
            "type": "HDInsightHive",
            "typeProperties": {
                "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                "scriptLinkedService": "AzureStorageLinkedService",
                "defines": {
                    "inputtable": "wasb://adfgetstarted@<stroageaccountname>.blob.core.windows.net/inputdata",
                    "partitionedtable": "wasb://adfgetstarted@<stroageaccountname>t.blob.core.windows.net/partitioneddata"
                }
            },
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
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
        }],
        "start": "2017-07-10T00:00:00Z",
        "end": "2017-07-11T00:00:00Z",
        "isPaused": false
    }
}
```

<span data-ttu-id="979b5-199">Hello JSON kodutdrag skapar du en pipeline som består av en enskild aktivitet som använder Hive tooprocess data på ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="979b5-199">In hello JSON snippet, you are creating a pipeline that consists of a single activity that uses Hive tooprocess data on a HDInsight cluster.</span></span>

<span data-ttu-id="979b5-200">hello Hive-skriptfil, **partitionweblogs.hql**, lagras i hello Azure storage-konto (anges av hello scriptLinkedService, kallas **StorageLinkedService**), och i **skript**  mapp i hello behållaren **adfgetstarted**.</span><span class="sxs-lookup"><span data-stu-id="979b5-200">hello Hive script file, **partitionweblogs.hql**, is stored in hello Azure storage account (specified by hello scriptLinkedService, called **StorageLinkedService**), and in **script** folder in hello container **adfgetstarted**.</span></span>

<span data-ttu-id="979b5-201">Hej **definierar** avsnittet anger körningsinställningar som skickas toohello hive-skript som Hive konfigurationsvärden (t.ex. ${hiveconf: inputtable}, ${hiveconf:partitionedtable}).</span><span class="sxs-lookup"><span data-stu-id="979b5-201">hello **defines** section specifies runtime settings that are passed toohello hive script as Hive configuration values (e.g ${hiveconf:inputtable}, ${hiveconf:partitionedtable}).</span></span>

<span data-ttu-id="979b5-202">Hej **starta** och **end** egenskaper för hello pipeline anger hello aktiva perioden för hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="979b5-202">hello **start** and **end** properties of hello pipeline specifies hello active period of hello pipeline.</span></span>

<span data-ttu-id="979b5-203">I hello aktivitets-JSON, anger du den hello Hive-skript som körs på hello beräkning som anges av hello **linkedServiceName** – **HDInsightOnDemandLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="979b5-203">In hello activity JSON, you specify that hello Hive script runs on hello compute specified by hello **linkedServiceName** – **HDInsightOnDemandLinkedService**.</span></span>

> [!NOTE]
> <span data-ttu-id="979b5-204">Se ”Pipeline-JSON” i [Pipelines och aktiviteter i Azure Data Factory](data-factory-create-pipelines.md) mer information om JSON-egenskaper som används i föregående exempel hello.</span><span class="sxs-lookup"><span data-stu-id="979b5-204">See "Pipeline JSON" in [Pipelines and activities in Azure Data Factory](data-factory-create-pipelines.md) for details about JSON properties used in hello preceding example.</span></span>
>
>

## <a name="set-global-variables"></a><span data-ttu-id="979b5-205">Ange globala variabler</span><span class="sxs-lookup"><span data-stu-id="979b5-205">Set global variables</span></span>
<span data-ttu-id="979b5-206">Kör följande kommandon när du ersätter hello värdena med dina egna hello i Azure PowerShell:</span><span class="sxs-lookup"><span data-stu-id="979b5-206">In Azure PowerShell, execute hello following commands after replacing hello values with your own:</span></span>

> [!IMPORTANT]
> <span data-ttu-id="979b5-207">Anvisningar för hur du hämtar klient-ID, klienthemlighet, klientorganisations-ID och prenumerations-ID finns i [kravavsnittet](#prerequisites).</span><span class="sxs-lookup"><span data-stu-id="979b5-207">See [Prerequisites](#prerequisites) section for instructions on getting client ID, client secret, tenant ID, and subscription ID.</span></span>
>
>

```PowerShell
$client_id = "<client ID of application in AAD>"
$client_secret = "<client key of application in AAD>"
$tenant = "<Azure tenant ID>";
$subscription_id="<Azure subscription ID>";

$rg = "ADFTutorialResourceGroup"
$adf = "FirstDataFactoryREST"
```


## <a name="authenticate-with-aad"></a><span data-ttu-id="979b5-208">Autentisera med AAD</span><span class="sxs-lookup"><span data-stu-id="979b5-208">Authenticate with AAD</span></span>

```PowerShell
$cmd = { .\curl.exe -X POST https://login.microsoftonline.com/$tenant/oauth2/token  -F grant_type=client_credentials  -F resource=https://management.core.windows.net/ -F client_id=$client_id -F client_secret=$client_secret };
$responseToken = Invoke-Command -scriptblock $cmd;
$accessToken = (ConvertFrom-Json $responseToken).access_token;

(ConvertFrom-Json $responseToken)
```


## <a name="create-data-factory"></a><span data-ttu-id="979b5-209">Skapa en datafabrik</span><span class="sxs-lookup"><span data-stu-id="979b5-209">Create data factory</span></span>
<span data-ttu-id="979b5-210">I det här steget ska du skapa en Azure Data Factory-fabrik med namnet **FirstDataFactoryREST**.</span><span class="sxs-lookup"><span data-stu-id="979b5-210">In this step, you create an Azure Data Factory named **FirstDataFactoryREST**.</span></span> <span data-ttu-id="979b5-211">En datafabrik kan ha en eller flera pipelines.</span><span class="sxs-lookup"><span data-stu-id="979b5-211">A data factory can have one or more pipelines.</span></span> <span data-ttu-id="979b5-212">En pipeline kan innehålla en eller flera aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="979b5-212">A pipeline can have one or more activities in it.</span></span> <span data-ttu-id="979b5-213">Till exempel en Kopieringsaktiviteten toocopy data från ett dataarkiv som källa tooa mål och en HDInsight Hive aktiviteten toorun en Hive-skript tootransform data.</span><span class="sxs-lookup"><span data-stu-id="979b5-213">For example, a Copy Activity toocopy data from a source tooa destination data store and a HDInsight Hive activity toorun a Hive script tootransform data.</span></span> <span data-ttu-id="979b5-214">Kör följande kommandon toocreate hello data factory hello:</span><span class="sxs-lookup"><span data-stu-id="979b5-214">Run hello following commands toocreate hello data factory:</span></span>

1. <span data-ttu-id="979b5-215">Tilldela hello kommandot toovariable med namnet **cmd**.</span><span class="sxs-lookup"><span data-stu-id="979b5-215">Assign hello command toovariable named **cmd**.</span></span>

    <span data-ttu-id="979b5-216">Bekräfta det hello-namnet i hello data factory som du anger här (ADFCopyTutorialDF) matchar hello namnet som angetts i hello **datafactory.json**.</span><span class="sxs-lookup"><span data-stu-id="979b5-216">Confirm that hello name of hello data factory you specify here (ADFCopyTutorialDF) matches hello name specified in hello **datafactory.json**.</span></span>

    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@datafactory.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/FirstDataFactoryREST?api-version=2015-10-01};
    ```
2. <span data-ttu-id="979b5-217">Kör hello kommando med hjälp av **Invoke-Command**.</span><span class="sxs-lookup"><span data-stu-id="979b5-217">Run hello command by using **Invoke-Command**.</span></span>

    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. <span data-ttu-id="979b5-218">Visa hello resultat.</span><span class="sxs-lookup"><span data-stu-id="979b5-218">View hello results.</span></span> <span data-ttu-id="979b5-219">Om hello datafabriken har skapats visas hello JSON för hello data factory i hello **resultat**, annars visas ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="979b5-219">If hello data factory has been successfully created, you see hello JSON for hello data factory in hello **results**; otherwise, you see an error message.</span></span>

    ```PowerShell
    Write-Host $results
    ```

<span data-ttu-id="979b5-220">Observera följande punkter hello:</span><span class="sxs-lookup"><span data-stu-id="979b5-220">Note hello following points:</span></span>

* <span data-ttu-id="979b5-221">hello namnet på hello Azure Data Factory måste vara globalt unika.</span><span class="sxs-lookup"><span data-stu-id="979b5-221">hello name of hello Azure Data Factory must be globally unique.</span></span> <span data-ttu-id="979b5-222">Om du ser hello fel i resultat: **datafabriksnamnet ”FirstDataFactoryREST” är inte tillgänglig**, hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="979b5-222">If you see hello error in results: **Data factory name “FirstDataFactoryREST” is not available**, do hello following steps:</span></span>
  1. <span data-ttu-id="979b5-223">Ändra hello namn (till exempel yournameFirstDataFactoryREST) i hello **datafactory.json** fil.</span><span class="sxs-lookup"><span data-stu-id="979b5-223">Change hello name (for example, yournameFirstDataFactoryREST) in hello **datafactory.json** file.</span></span> <span data-ttu-id="979b5-224">Se artikeln [Data Factory – namnregler](data-factory-naming-rules.md) för namnregler för Data Factory-artefakter.</span><span class="sxs-lookup"><span data-stu-id="979b5-224">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>
  2. <span data-ttu-id="979b5-225">I hello första kommandot där hello **$cmd** variabeln har tilldelats ett värde, Ersätt FirstDataFactoryREST med hello nytt namn och kör hello-kommando.</span><span class="sxs-lookup"><span data-stu-id="979b5-225">In hello first command where hello **$cmd** variable is assigned a value, replace FirstDataFactoryREST with hello new name and run hello command.</span></span>
  3. <span data-ttu-id="979b5-226">Kör hello följande två kommandon tooinvoke hello REST API toocreate hello data factory och Skriv ut hello resultaten av hello igen.</span><span class="sxs-lookup"><span data-stu-id="979b5-226">Run hello next two commands tooinvoke hello REST API toocreate hello data factory and print hello results of hello operation.</span></span>
* <span data-ttu-id="979b5-227">toocreate Data Factory instanser måste toobe deltagare/administratören av hello Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="979b5-227">toocreate Data Factory instances, you need toobe a contributor/administrator of hello Azure subscription</span></span>
* <span data-ttu-id="979b5-228">hello namnet på hello data factory kan registreras som en DNS-namnet i hello framtida och därför bli synligt offentligt.</span><span class="sxs-lookup"><span data-stu-id="979b5-228">hello name of hello data factory may be registered as a DNS name in hello future and hence become publicly visible.</span></span>
* <span data-ttu-id="979b5-229">Om felmeddelandet hello ”:**den här prenumerationen är inte registrerad toouse namnområde Microsoft.DataFactory**”, gör du något av följande hello och försök att publicera igen:</span><span class="sxs-lookup"><span data-stu-id="979b5-229">If you receive hello error: "**This subscription is not registered toouse namespace Microsoft.DataFactory**", do one of hello following and try publishing again:</span></span>

  * <span data-ttu-id="979b5-230">Kör hello efter kommandot tooregister hello Data Factory-providern i Azure PowerShell:</span><span class="sxs-lookup"><span data-stu-id="979b5-230">In Azure PowerShell, run hello following command tooregister hello Data Factory provider:</span></span>

    ```PowerShell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```

      <span data-ttu-id="979b5-231">Du kan köra följande kommando tooconfirm hello som hello Data Factory providern är registrerad:</span><span class="sxs-lookup"><span data-stu-id="979b5-231">You can run hello following command tooconfirm that hello Data Factory provider is registered:</span></span>
    ```PowerShell
    Get-AzureRmResourceProvider
    ```
  * <span data-ttu-id="979b5-232">Logga in med hjälp av hello Azure-prenumeration i hello [Azure-portalen](https://portal.azure.com) och navigera tooa Data Factory bladet (eller) skapa en datafabrik i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="979b5-232">Login using hello Azure subscription into hello [Azure portal](https://portal.azure.com) and navigate tooa Data Factory blade (or) create a data factory in hello Azure portal.</span></span> <span data-ttu-id="979b5-233">Den här åtgärden registrerar automatiskt hello-providern för dig.</span><span class="sxs-lookup"><span data-stu-id="979b5-233">This action automatically registers hello provider for you.</span></span>

<span data-ttu-id="979b5-234">Innan du skapar en pipeline, måste toocreate några Data Factory-entiteter först.</span><span class="sxs-lookup"><span data-stu-id="979b5-234">Before creating a pipeline, you need toocreate a few Data Factory entities first.</span></span> <span data-ttu-id="979b5-235">Du måste först skapa länkade tjänster toolink data lagrar/beräknar tooyour data lagras, definiera indata och utdata datauppsättningar toorepresent i länkade datalager.</span><span class="sxs-lookup"><span data-stu-id="979b5-235">You first create linked services toolink data stores/computes tooyour data store, define input and output datasets toorepresent data in linked data stores.</span></span>

## <a name="create-linked-services"></a><span data-ttu-id="979b5-236">Skapa länkade tjänster</span><span class="sxs-lookup"><span data-stu-id="979b5-236">Create linked services</span></span>
<span data-ttu-id="979b5-237">I det här steget kan länka du ditt Azure Storage-konto och en på begäran Azure HDInsight-kluster tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="979b5-237">In this step, you link your Azure Storage account and an on-demand Azure HDInsight cluster tooyour data factory.</span></span> <span data-ttu-id="979b5-238">hello Azure Storage-konto innehåller hello inkommande och utgående data för hello pipeline i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="979b5-238">hello Azure Storage account holds hello input and output data for hello pipeline in this sample.</span></span> <span data-ttu-id="979b5-239">hello länkad HDInsight-tjänst är används toorun en Hive-skript som angetts i hello aktivitet för hello pipeline i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="979b5-239">hello HDInsight linked service is used toorun a Hive script specified in hello activity of hello pipeline in this sample.</span></span>

### <a name="create-azure-storage-linked-service"></a><span data-ttu-id="979b5-240">Skapa en länkad Azure-lagringstjänst</span><span class="sxs-lookup"><span data-stu-id="979b5-240">Create Azure Storage linked service</span></span>
<span data-ttu-id="979b5-241">I det här steget kan länka du din Azure Storage-konto tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="979b5-241">In this step, you link your Azure Storage account tooyour data factory.</span></span> <span data-ttu-id="979b5-242">Med den här självstudiekursen kommer du använda hello samma Azure Storage-konto toostore i/o-data och hello HQL skriptfil.</span><span class="sxs-lookup"><span data-stu-id="979b5-242">With this tutorial, you use hello same Azure Storage account toostore input/output data and hello HQL script file.</span></span>

1. <span data-ttu-id="979b5-243">Tilldela hello kommandot toovariable med namnet **cmd**.</span><span class="sxs-lookup"><span data-stu-id="979b5-243">Assign hello command toovariable named **cmd**.</span></span>

    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@azurestoragelinkedservice.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/AzureStorageLinkedService?api-version=2015-10-01};
    ```
2. <span data-ttu-id="979b5-244">Kör hello kommando med hjälp av **Invoke-Command**.</span><span class="sxs-lookup"><span data-stu-id="979b5-244">Run hello command by using **Invoke-Command**.</span></span>

    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. <span data-ttu-id="979b5-245">Visa hello resultat.</span><span class="sxs-lookup"><span data-stu-id="979b5-245">View hello results.</span></span> <span data-ttu-id="979b5-246">Om hello länkad tjänst har skapats, så se hello JSON hello länkad tjänst i hello **resultat**, annars visas ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="979b5-246">If hello linked service has been successfully created, you see hello JSON for hello linked service in hello **results**; otherwise, you see an error message.</span></span>

    ```PowerShell
    Write-Host $results
    ```

### <a name="create-azure-hdinsight-linked-service"></a><span data-ttu-id="979b5-247">Skapa en Azure HDInsight-länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="979b5-247">Create Azure HDInsight linked service</span></span>
<span data-ttu-id="979b5-248">I det här steget kan länka du ett på begäran HDInsight-kluster tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="979b5-248">In this step, you link an on-demand HDInsight cluster tooyour data factory.</span></span> <span data-ttu-id="979b5-249">Hej HDInsight-kluster skapas under körning och tas bort när den är klar bearbetning och inaktiv för hello angiven tidsperiod automatiskt.</span><span class="sxs-lookup"><span data-stu-id="979b5-249">hello HDInsight cluster is automatically created at runtime and deleted after it is done processing and idle for hello specified amount of time.</span></span> <span data-ttu-id="979b5-250">Du kan använda ditt eget HDInsight-kluster i stället för att använda ett HDInsight-kluster på begäran.</span><span class="sxs-lookup"><span data-stu-id="979b5-250">You could use your own HDInsight cluster instead of using an on-demand HDInsight cluster.</span></span> <span data-ttu-id="979b5-251">Se [Beräkna länkade tjänster](data-factory-compute-linked-services.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="979b5-251">See [Compute Linked Services](data-factory-compute-linked-services.md) for details.</span></span>

1. <span data-ttu-id="979b5-252">Tilldela hello kommandot toovariable med namnet **cmd**.</span><span class="sxs-lookup"><span data-stu-id="979b5-252">Assign hello command toovariable named **cmd**.</span></span>

    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@hdinsightondemandlinkedservice.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/hdinsightondemandlinkedservice?api-version=2015-10-01};
    ```
2. <span data-ttu-id="979b5-253">Kör hello kommando med hjälp av **Invoke-Command**.</span><span class="sxs-lookup"><span data-stu-id="979b5-253">Run hello command by using **Invoke-Command**.</span></span>

    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. <span data-ttu-id="979b5-254">Visa hello resultat.</span><span class="sxs-lookup"><span data-stu-id="979b5-254">View hello results.</span></span> <span data-ttu-id="979b5-255">Om hello länkad tjänst har skapats, så se hello JSON hello länkad tjänst i hello **resultat**, annars visas ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="979b5-255">If hello linked service has been successfully created, you see hello JSON for hello linked service in hello **results**; otherwise, you see an error message.</span></span>

    ```PowerShell
    Write-Host $results
    ```

## <a name="create-datasets"></a><span data-ttu-id="979b5-256">Skapa datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="979b5-256">Create datasets</span></span>
<span data-ttu-id="979b5-257">I det här steget Skapa datauppsättningar toorepresent hello indata och utdata för Hive-bearbetning.</span><span class="sxs-lookup"><span data-stu-id="979b5-257">In this step, you create datasets toorepresent hello input and output data for Hive processing.</span></span> <span data-ttu-id="979b5-258">De här datauppsättningarna finns toohello **StorageLinkedService** du har skapat tidigare i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="979b5-258">These datasets refer toohello **StorageLinkedService** you have created earlier in this tutorial.</span></span> <span data-ttu-id="979b5-259">Hej länkade tjänsten punkter tooan Azure Storage-konto och datauppsättningar anger behållare, mapp, filnamn i hello lagring som innehåller indata och utdata.</span><span class="sxs-lookup"><span data-stu-id="979b5-259">hello linked service points tooan Azure Storage account and datasets specify container, folder, file name in hello storage that holds input and output data.</span></span>

### <a name="create-input-dataset"></a><span data-ttu-id="979b5-260">Skapa indatauppsättning</span><span class="sxs-lookup"><span data-stu-id="979b5-260">Create input dataset</span></span>
<span data-ttu-id="979b5-261">I det här steget skapar du hello inkommande dataset toorepresent inkommande data som lagras i hello Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="979b5-261">In this step, you create hello input dataset toorepresent input data stored in hello Azure Blob storage.</span></span>

1. <span data-ttu-id="979b5-262">Tilldela hello kommandot toovariable med namnet **cmd**.</span><span class="sxs-lookup"><span data-stu-id="979b5-262">Assign hello command toovariable named **cmd**.</span></span>

    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@inputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobInput?api-version=2015-10-01};
    ```
2. <span data-ttu-id="979b5-263">Kör hello kommando med hjälp av **Invoke-Command**.</span><span class="sxs-lookup"><span data-stu-id="979b5-263">Run hello command by using **Invoke-Command**.</span></span>

    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. <span data-ttu-id="979b5-264">Visa hello resultat.</span><span class="sxs-lookup"><span data-stu-id="979b5-264">View hello results.</span></span> <span data-ttu-id="979b5-265">Om hello datauppsättningen har skapats visas hello JSON för hello datauppsättningen i hello **resultat**, annars visas ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="979b5-265">If hello dataset has been successfully created, you see hello JSON for hello dataset in hello **results**; otherwise, you see an error message.</span></span>

    ```PowerShell
    Write-Host $results
    ```

### <a name="create-output-dataset"></a><span data-ttu-id="979b5-266">Skapa datauppsättning för utdata</span><span class="sxs-lookup"><span data-stu-id="979b5-266">Create output dataset</span></span>
<span data-ttu-id="979b5-267">I det här steget skapar du hello utdata dataset toorepresent utdata data som lagras i hello Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="979b5-267">In this step, you create hello output dataset toorepresent output data stored in hello Azure Blob storage.</span></span>

1. <span data-ttu-id="979b5-268">Tilldela hello kommandot toovariable med namnet **cmd**.</span><span class="sxs-lookup"><span data-stu-id="979b5-268">Assign hello command toovariable named **cmd**.</span></span>

    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@outputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobOutput?api-version=2015-10-01};
    ```
2. <span data-ttu-id="979b5-269">Kör hello kommando med hjälp av **Invoke-Command**.</span><span class="sxs-lookup"><span data-stu-id="979b5-269">Run hello command by using **Invoke-Command**.</span></span>

    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. <span data-ttu-id="979b5-270">Visa hello resultat.</span><span class="sxs-lookup"><span data-stu-id="979b5-270">View hello results.</span></span> <span data-ttu-id="979b5-271">Om hello datauppsättningen har skapats visas hello JSON för hello datauppsättningen i hello **resultat**, annars visas ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="979b5-271">If hello dataset has been successfully created, you see hello JSON for hello dataset in hello **results**; otherwise, you see an error message.</span></span>

    ```PowerShell
    Write-Host $results
    ```

## <a name="create-pipeline"></a><span data-ttu-id="979b5-272">Skapa pipeline</span><span class="sxs-lookup"><span data-stu-id="979b5-272">Create pipeline</span></span>
<span data-ttu-id="979b5-273">I det här steget ska du skapa din första pipeline med en **HDInsightHive**-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="979b5-273">In this step, you create your first pipeline with a **HDInsightHive** activity.</span></span> <span data-ttu-id="979b5-274">Inkommande segmentet är tillgängliga månad (frekvens: månad, intervall: 1), utdata segment skapas varje månad och hello scheduler för hello aktiviteten också egenskapen toomonthly.</span><span class="sxs-lookup"><span data-stu-id="979b5-274">Input slice is available monthly (frequency: Month, interval: 1), output slice is produced monthly, and hello scheduler property for hello activity is also set toomonthly.</span></span> <span data-ttu-id="979b5-275">hello inställningar för hello utdatauppsättningen och hello aktivitet Schemaläggaren måste matcha.</span><span class="sxs-lookup"><span data-stu-id="979b5-275">hello settings for hello output dataset and hello activity scheduler must match.</span></span> <span data-ttu-id="979b5-276">Datamängd för utdata är för närvarande vilka enheter hello schema, så du måste skapa en datamängd för utdata även om hello aktiviteten inte producerar några utdata.</span><span class="sxs-lookup"><span data-stu-id="979b5-276">Currently, output dataset is what drives hello schedule, so you must create an output dataset even if hello activity does not produce any output.</span></span> <span data-ttu-id="979b5-277">Om hello aktiviteten inte tar några indata, kan du hoppa över skapar hello inkommande dataset.</span><span class="sxs-lookup"><span data-stu-id="979b5-277">If hello activity doesn't take any input, you can skip creating hello input dataset.</span></span>

<span data-ttu-id="979b5-278">Bekräfta att du ser hello **input.log** filen i hello **adfgetstarted/inputdata** mapp i hello Azure blob storage och kör hello efter kommandot toodeploy hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="979b5-278">Confirm that you see hello **input.log** file in hello **adfgetstarted/inputdata** folder in hello Azure blob storage, and run hello following command toodeploy hello pipeline.</span></span> <span data-ttu-id="979b5-279">Eftersom hello **starta** och **end** gånger ställs i hello senaste och **isPaused** är uppsättningen toofalse, hello pipeline (aktivitet i pipelinen hello) körs omedelbart när du har distribuerat.</span><span class="sxs-lookup"><span data-stu-id="979b5-279">Since hello **start** and **end** times are set in hello past and **isPaused** is set toofalse, hello pipeline (activity in hello pipeline) runs immediately after you deploy.</span></span>

1. <span data-ttu-id="979b5-280">Tilldela hello kommandot toovariable med namnet **cmd**.</span><span class="sxs-lookup"><span data-stu-id="979b5-280">Assign hello command toovariable named **cmd**.</span></span>

    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@pipeline.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datapipelines/MyFirstPipeline?api-version=2015-10-01};
    ```
2. <span data-ttu-id="979b5-281">Kör hello kommando med hjälp av **Invoke-Command**.</span><span class="sxs-lookup"><span data-stu-id="979b5-281">Run hello command by using **Invoke-Command**.</span></span>

    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. <span data-ttu-id="979b5-282">Visa hello resultat.</span><span class="sxs-lookup"><span data-stu-id="979b5-282">View hello results.</span></span> <span data-ttu-id="979b5-283">Om hello datauppsättningen har skapats visas hello JSON för hello datauppsättningen i hello **resultat**, annars visas ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="979b5-283">If hello dataset has been successfully created, you see hello JSON for hello dataset in hello **results**; otherwise, you see an error message.</span></span>

    ```PowerShell
    Write-Host $results
    ```
4. <span data-ttu-id="979b5-284">Grattis, du har skapat din första pipeline med Azure PowerShell!</span><span class="sxs-lookup"><span data-stu-id="979b5-284">Congratulations, you have successfully created your first pipeline using Azure PowerShell!</span></span>

## <a name="monitor-pipeline"></a><span data-ttu-id="979b5-285">Övervaka pipeline</span><span class="sxs-lookup"><span data-stu-id="979b5-285">Monitor pipeline</span></span>
<span data-ttu-id="979b5-286">I det här steget kan använda du Data Factory REST API: et toomonitor segment som produceras av hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="979b5-286">In this step, you use Data Factory REST API toomonitor slices being produced by hello pipeline.</span></span>

```PowerShell
$ds ="AzureBlobOutput"

$cmd = {.\curl.exe -X GET -H "Authorization: Bearer $accessToken" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/$ds/slices?start=1970-01-01T00%3a00%3a00.0000000Z"&"end=2016-08-12T00%3a00%3a00.0000000Z"&"api-version=2015-10-01};

$results2 = Invoke-Command -scriptblock $cmd;

IF ((ConvertFrom-Json $results2).value -ne $NULL) {
    ConvertFrom-Json $results2 | Select-Object -Expand value | Format-Table
} else {
        (convertFrom-Json $results2).RemoteException
}
```

> [!IMPORTANT]
> <span data-ttu-id="979b5-287">Att skapa ett HDInsight-kluster på begäran kan ta lite längre tid (cirka 20 minuter).</span><span class="sxs-lookup"><span data-stu-id="979b5-287">Creation of an on-demand HDInsight cluster usually takes sometime (approximately 20 minutes).</span></span> <span data-ttu-id="979b5-288">Därför förvänta sig hello pipeline tootake **cirka 30 minuter** tooprocess hello sektorn.</span><span class="sxs-lookup"><span data-stu-id="979b5-288">Therefore, expect hello pipeline tootake **approximately 30 minutes** tooprocess hello slice.</span></span>
>
>

<span data-ttu-id="979b5-289">Kör hello Invoke-Command och hello nästa tills du ser hello sektor i **klar** tillstånd eller **misslyckades** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="979b5-289">Run hello Invoke-Command and hello next one until you see hello slice in **Ready** state or **Failed** state.</span></span> <span data-ttu-id="979b5-290">När hello sektorn är i tillståndet Ready, kontrollera hello **partitioneddata** mapp i hello **adfgetstarted** behållare i blobblagring för hello utdata.</span><span class="sxs-lookup"><span data-stu-id="979b5-290">When hello slice is in Ready state, check hello **partitioneddata** folder in hello **adfgetstarted** container in your blob storage for hello output data.</span></span>  <span data-ttu-id="979b5-291">hello skapandet av ett HDInsight-kluster på begäran tar vanligtvis en stund.</span><span class="sxs-lookup"><span data-stu-id="979b5-291">hello creation of an on-demand HDInsight cluster usually takes some time.</span></span>

![utdata](./media/data-factory-build-your-first-pipeline-using-rest-api/three-ouptut-files.png)

> [!IMPORTANT]
> <span data-ttu-id="979b5-293">hello indatafilen hämtar bort när hello segment har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="979b5-293">hello input file gets deleted when hello slice is processed successfully.</span></span> <span data-ttu-id="979b5-294">Om du vill toorerun hello segment eller hello kursen igen överför därför hello indatafilen (input.log) toohello inputdata mapp för hello adfgetstarted behållare.</span><span class="sxs-lookup"><span data-stu-id="979b5-294">Therefore, if you want toorerun hello slice or do hello tutorial again, upload hello input file (input.log) toohello inputdata folder of hello adfgetstarted container.</span></span>
>
>

<span data-ttu-id="979b5-295">Du kan också använda Azure portal toomonitor segment och felsöka eventuella problem.</span><span class="sxs-lookup"><span data-stu-id="979b5-295">You can also use Azure portal toomonitor slices and troubleshoot any issues.</span></span> <span data-ttu-id="979b5-296">Mer information finns i [Övervaka pipelines med hjälp av Azure-portalen](data-factory-build-your-first-pipeline-using-editor.md#monitor-pipeline).</span><span class="sxs-lookup"><span data-stu-id="979b5-296">See [Monitor pipelines using Azure portal](data-factory-build-your-first-pipeline-using-editor.md#monitor-pipeline) details.</span></span>

## <a name="summary"></a><span data-ttu-id="979b5-297">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="979b5-297">Summary</span></span>
<span data-ttu-id="979b5-298">I kursen får skapat du ett Azure data factory tooprocess data genom att köra Hive-skript på ett HDInsight hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="979b5-298">In this tutorial, you created an Azure data factory tooprocess data by running Hive script on a HDInsight hadoop cluster.</span></span> <span data-ttu-id="979b5-299">Du har använt hello Data Factory-redigeraren i hello Azure portal toodo hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="979b5-299">You used hello Data Factory Editor in hello Azure portal toodo hello following steps:</span></span>

1. <span data-ttu-id="979b5-300">Du skapade en Azure **Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="979b5-300">Created an Azure **data factory**.</span></span>
2. <span data-ttu-id="979b5-301">Du skapade två **länkade tjänster**:</span><span class="sxs-lookup"><span data-stu-id="979b5-301">Created two **linked services**:</span></span>
   1. <span data-ttu-id="979b5-302">**Azure Storage** länkade tjänsten toolink din Azure blob-lagring som innehåller in-/ utdata filer toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="979b5-302">**Azure Storage** linked service toolink your Azure blob storage that holds input/output files toohello data factory.</span></span>
   2. <span data-ttu-id="979b5-303">**Azure HDInsight** på begäran länkade tjänsten toolink en på begäran HDInsight Hadoop-kluster toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="979b5-303">**Azure HDInsight** on-demand linked service toolink an on-demand HDInsight Hadoop cluster toohello data factory.</span></span> <span data-ttu-id="979b5-304">Azure Data Factory skapar ett HDInsight Hadoop klustret just-in-time tooprocess indata och utdata för produkten.</span><span class="sxs-lookup"><span data-stu-id="979b5-304">Azure Data Factory creates a HDInsight Hadoop cluster just-in-time tooprocess input data and produce output data.</span></span>
3. <span data-ttu-id="979b5-305">Skapa två **datauppsättningar**, som beskriver inkommande och utgående data för HDInsight Hive aktivitet i hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="979b5-305">Created two **datasets**, which describe input and output data for HDInsight Hive activity in hello pipeline.</span></span>
4. <span data-ttu-id="979b5-306">Du skapade en **pipeline** med en **HDInsight Hive**-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="979b5-306">Created a **pipeline** with a **HDInsight Hive** activity.</span></span>

## <a name="next-steps"></a><span data-ttu-id="979b5-307">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="979b5-307">Next steps</span></span>
<span data-ttu-id="979b5-308">I den här artikeln har du skapat en pipeline med en transformeringsaktivitet (HDInsight-aktivitet) som kör ett Hive-skript på ett Azure HDInsight-kluster på begäran.</span><span class="sxs-lookup"><span data-stu-id="979b5-308">In this article, you have created a pipeline with a transformation activity (HDInsight Activity) that runs a Hive script on an on-demand Azure HDInsight cluster.</span></span> <span data-ttu-id="979b5-309">hur toouse en Kopieringsaktiviteten toocopy data från ett Azure Blob-tooAzure SQL, se toosee [Självstudier: kopiera data från ett Azure Blob-tooAzure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="979b5-309">toosee how toouse a Copy Activity toocopy data from an Azure Blob tooAzure SQL, see [Tutorial: Copy data from an Azure Blob tooAzure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="979b5-310">Se även</span><span class="sxs-lookup"><span data-stu-id="979b5-310">See Also</span></span>
| <span data-ttu-id="979b5-311">Avsnitt</span><span class="sxs-lookup"><span data-stu-id="979b5-311">Topic</span></span> | <span data-ttu-id="979b5-312">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="979b5-312">Description</span></span> |
|:--- |:--- |
| [<span data-ttu-id="979b5-313">Referens för REST-API:et för Data Factory</span><span class="sxs-lookup"><span data-stu-id="979b5-313">Data Factory REST API Reference</span></span>](/rest/api/datafactory/) |<span data-ttu-id="979b5-314">Se den omfattande dokumentationen för Data Factory-cmdletar</span><span class="sxs-lookup"><span data-stu-id="979b5-314">See comprehensive documentation on Data Factory cmdlets</span></span> |
| [<span data-ttu-id="979b5-315">Pipelines</span><span class="sxs-lookup"><span data-stu-id="979b5-315">Pipelines</span></span>](data-factory-create-pipelines.md) |<span data-ttu-id="979b5-316">Den här artikeln hjälper dig att förstå pipelines och aktiviteter i Azure Data Factory och hur toouse dem tooconstruct slutpunkt till slutpunkt datadrivna arbetsflöden för din scenario eller ditt företag.</span><span class="sxs-lookup"><span data-stu-id="979b5-316">This article helps you understand pipelines and activities in Azure Data Factory and how toouse them tooconstruct end-to-end data-driven workflows for your scenario or business.</span></span> |
| [<span data-ttu-id="979b5-317">Datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="979b5-317">Datasets</span></span>](data-factory-create-datasets.md) |<span data-ttu-id="979b5-318">I den här artikeln förklaras hur datauppsättningar fungerar i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="979b5-318">This article helps you understand datasets in Azure Data Factory.</span></span> |
| [<span data-ttu-id="979b5-319">Schemaläggning och körning</span><span class="sxs-lookup"><span data-stu-id="979b5-319">Scheduling and Execution</span></span>](data-factory-scheduling-and-execution.md) |<span data-ttu-id="979b5-320">Den här artikeln förklarar hello schemaläggning och körning av aspekter av Azure Data Factory programmodell.</span><span class="sxs-lookup"><span data-stu-id="979b5-320">This article explains hello scheduling and execution aspects of Azure Data Factory application model.</span></span> |
| [<span data-ttu-id="979b5-321">Övervaka och hantera pipelines med övervakningsappen</span><span class="sxs-lookup"><span data-stu-id="979b5-321">Monitor and manage pipelines using Monitoring App</span></span>](data-factory-monitor-manage-app.md) |<span data-ttu-id="979b5-322">Den här artikeln beskriver hur toomonitor, hantera och felsöka pipelines med hello övervakning & Management-appen.</span><span class="sxs-lookup"><span data-stu-id="979b5-322">This article describes how toomonitor, manage, and debug pipelines using hello Monitoring & Management App.</span></span> |
