---
title: aaaTroubleshoot Azure Data Factory-problem
description: "Lär dig hur tootroubleshoot problem med hjälp av Azure Data Factory."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 38fd14c1-5bb7-4eef-a9f5-b289ff9a6942
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: spelluru
ms.openlocfilehash: cf65bcf3e1c3f061d3ac1dbf32e99cc7b014f9dc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-data-factory-issues"></a><span data-ttu-id="d2890-103">Felsök Data Factory-problem</span><span class="sxs-lookup"><span data-stu-id="d2890-103">Troubleshoot Data Factory issues</span></span>
<span data-ttu-id="d2890-104">Den här artikeln innehåller felsökningstips för problem när du använder Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="d2890-104">This article provides troubleshooting tips for issues when using Azure Data Factory.</span></span> <span data-ttu-id="d2890-105">Den här artikeln innehåller inte alla hello möjliga problem när du använder hello service, men omfattar vissa problem och allmänna felsökningstips.</span><span class="sxs-lookup"><span data-stu-id="d2890-105">This article does not list all hello possible issues when using hello service, but it covers some issues and general troubleshooting tips.</span></span>   

## <a name="troubleshooting-tips"></a><span data-ttu-id="d2890-106">Felsökningstips</span><span class="sxs-lookup"><span data-stu-id="d2890-106">Troubleshooting tips</span></span>
### <a name="error-hello-subscription-is-not-registered-toouse-namespace-microsoftdatafactory"></a><span data-ttu-id="d2890-107">Fel: hello prenumerationen är inte registrerad toouse namnområdet 'Microsoft.DataFactory'</span><span class="sxs-lookup"><span data-stu-id="d2890-107">Error: hello subscription is not registered toouse namespace 'Microsoft.DataFactory'</span></span>
<span data-ttu-id="d2890-108">Om du får detta felmeddelande registrerats hello Azure Data Factory-resursprovidern inte på datorn.</span><span class="sxs-lookup"><span data-stu-id="d2890-108">If you receive this error, hello Azure Data Factory resource provider has not been registered on your machine.</span></span> <span data-ttu-id="d2890-109">Hej du följande:</span><span class="sxs-lookup"><span data-stu-id="d2890-109">Do hello following:</span></span>

1. <span data-ttu-id="d2890-110">Starta Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d2890-110">Launch Azure PowerShell.</span></span>
2. <span data-ttu-id="d2890-111">Logga in tooyour Azure-konto med hjälp av följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="d2890-111">Log in tooyour Azure account using hello following command.</span></span>

    ```powershell
    Login-AzureRmAccount
    ```
3. <span data-ttu-id="d2890-112">Kör hello efter kommandot tooregister hello Azure Data Factory-providern.</span><span class="sxs-lookup"><span data-stu-id="d2890-112">Run hello following command tooregister hello Azure Data Factory provider.</span></span>

    ```powershell        
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```

### <a name="problem-unauthorized-error-when-running-a-data-factory-cmdlet"></a><span data-ttu-id="d2890-113">Problem: Obehörig fel när du kör en Data Factory-cmdlet</span><span class="sxs-lookup"><span data-stu-id="d2890-113">Problem: Unauthorized error when running a Data Factory cmdlet</span></span>
<span data-ttu-id="d2890-114">Du använder antagligen inte hello höger Azure-konto eller prenumeration med hello Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d2890-114">You are probably not using hello right Azure account or subscription with hello Azure PowerShell.</span></span> <span data-ttu-id="d2890-115">Använd hello följande cmdlets tooselect hello höger Azure-konto och prenumeration toouse med hello Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d2890-115">Use hello following cmdlets tooselect hello right Azure account and subscription toouse with hello Azure PowerShell.</span></span>

1. <span data-ttu-id="d2890-116">Login-AzureRmAccount - Använd hello rätt användar-ID och lösenord</span><span class="sxs-lookup"><span data-stu-id="d2890-116">Login-AzureRmAccount - Use hello right user ID and password</span></span>
2. <span data-ttu-id="d2890-117">Get-AzureRmSubscription – visa alla hello prenumerationer för hello-kontot.</span><span class="sxs-lookup"><span data-stu-id="d2890-117">Get-AzureRmSubscription - View all hello subscriptions for hello account.</span></span>
3. <span data-ttu-id="d2890-118">SELECT-AzureRmSubscription &lt;prenumerationsnamn&gt; -Välj rätt hello-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="d2890-118">Select-AzureRmSubscription &lt;subscription name&gt; - Select hello right subscription.</span></span> <span data-ttu-id="d2890-119">Använd hello samma du använder toocreate en datafabrik på hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="d2890-119">Use hello same one you use toocreate a data factory on hello Azure portal.</span></span>

### <a name="problem-fail-toolaunch-data-management-gateway-express-setup-from-azure-portal"></a><span data-ttu-id="d2890-120">Problem: Misslyckas toolaunch Express installation av Data Management Gateway från Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="d2890-120">Problem: Fail toolaunch Data Management Gateway Express Setup from Azure portal</span></span>
<span data-ttu-id="d2890-121">hello Express-installationen för hello Data Management Gateway kräver Internet Explorer eller en kompatibel webbläsare med Microsoft ClickOnce.</span><span class="sxs-lookup"><span data-stu-id="d2890-121">hello Express setup for hello Data Management Gateway requires Internet Explorer or a Microsoft ClickOnce compatible web browser.</span></span> <span data-ttu-id="d2890-122">Om hello snabbinstallationen misslyckas toostart, gör du något av följande hello:</span><span class="sxs-lookup"><span data-stu-id="d2890-122">If hello Express Setup fails toostart, do one of hello following:</span></span>

* <span data-ttu-id="d2890-123">Använd Internet Explorer eller en kompatibel webbläsare med Microsoft ClickOnce.</span><span class="sxs-lookup"><span data-stu-id="d2890-123">Use Internet Explorer or a Microsoft ClickOnce compatible web browser.</span></span>

    <span data-ttu-id="d2890-124">Om du använder Chrome, gå toohello [Chrome web store](https://chrome.google.com/webstore/), söka med nyckelordet ”ClickOnce”, väljer du något av hello ClickOnce-tillägg och installera den.</span><span class="sxs-lookup"><span data-stu-id="d2890-124">If you are using Chrome, go toohello [Chrome web store](https://chrome.google.com/webstore/), search with "ClickOnce" keyword, choose one of hello ClickOnce extensions, and install it.</span></span>

    <span data-ttu-id="d2890-125">Hello samma för Firefox (installera tillägget).</span><span class="sxs-lookup"><span data-stu-id="d2890-125">Do hello same for Firefox (install add-in).</span></span> <span data-ttu-id="d2890-126">Klicka på Öppna menyn i verktygsfältet hello (tre horisontella raderna i hello längst upp till höger), klickar du på tilläggskomponenter, söka med nyckelordet ”ClickOnce”, väljer du något av hello ClickOnce-tillägg och installera den.</span><span class="sxs-lookup"><span data-stu-id="d2890-126">Click Open Menu button on hello toolbar (three horizontal lines in hello top-right corner), click Add-ons, search with "ClickOnce" keyword, choose one of hello ClickOnce extensions, and install it.</span></span>
* <span data-ttu-id="d2890-127">Använd hello **manuell installation** länk som visas på hello samma bladet i hello portal.</span><span class="sxs-lookup"><span data-stu-id="d2890-127">Use hello **Manual Setup** link shown on hello same blade in hello portal.</span></span> <span data-ttu-id="d2890-128">Du använder den här metoden toodownload installationsfilen och kör den manuellt.</span><span class="sxs-lookup"><span data-stu-id="d2890-128">You use this approach toodownload installation file and run it manually.</span></span> <span data-ttu-id="d2890-129">När hello-installationen har slutförts visas dialogrutan för hello Data Management Gateway-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="d2890-129">After hello installation is successful, you see hello Data Management Gateway Configuration dialog box.</span></span> <span data-ttu-id="d2890-130">Kopiera hello **nyckeln** från portalen hello-skärmen och använder den i hello configuration manager toomanually registrera hello gateway med hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="d2890-130">Copy hello **key** from hello portal screen and use it in hello configuration manager toomanually register hello gateway with hello service.</span></span>  

### <a name="problem-fail-tooconnect-tooon-premises-sql-server"></a><span data-ttu-id="d2890-131">Problem: Misslyckas tooconnect tooon lokala SQL Server</span><span class="sxs-lookup"><span data-stu-id="d2890-131">Problem: Fail tooconnect tooon-premises SQL Server</span></span>
<span data-ttu-id="d2890-132">Starta **Data Management Gateway Configuration Manager** hello gateway-datorn och använda hello **felsökning** fliken tootest hello anslutning tooSQL Server från hello gateway-datorn.</span><span class="sxs-lookup"><span data-stu-id="d2890-132">Launch **Data Management Gateway Configuration Manager** on hello gateway machine and use hello **Troubleshooting** tab tootest hello connection tooSQL Server from hello gateway machine.</span></span> <span data-ttu-id="d2890-133">Se [felsökning av problem med gateway](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) tips om hur du felsöker anslutning /-gateway relaterade problem.</span><span class="sxs-lookup"><span data-stu-id="d2890-133">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>   

### <a name="problem-input-slices-are-in-waiting-state-for-ever"></a><span data-ttu-id="d2890-134">Problem: Indata segment som finns i väntan på tillstånd till någonsin</span><span class="sxs-lookup"><span data-stu-id="d2890-134">Problem: Input slices are in Waiting state for ever</span></span>
<span data-ttu-id="d2890-135">hello segment kan vara i **väntar på** tillstånd på grund av toovarious skäl.</span><span class="sxs-lookup"><span data-stu-id="d2890-135">hello slices could be in **Waiting** state due toovarious reasons.</span></span> <span data-ttu-id="d2890-136">En av hello vanliga orsaker är att hello **externa** egenskapen har inte angetts för**SANT**.</span><span class="sxs-lookup"><span data-stu-id="d2890-136">One of hello common reasons is that hello **external** property is not set too**true**.</span></span> <span data-ttu-id="d2890-137">En datamängd som är producerade utanför hello omfånget för Azure Data Factory bör markeras med **externa** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="d2890-137">Any dataset that is produced outside hello scope of Azure Data Factory should be marked with **external** property.</span></span> <span data-ttu-id="d2890-138">Den här egenskapen anger att hello data är extern och inte säkerhetskopieras av några pipelines i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="d2890-138">This property indicates that hello data is external and not backed by any pipelines within hello data factory.</span></span> <span data-ttu-id="d2890-139">Hej datasektorer är markerade som **klar** när hello data är tillgängliga i hello respektive store.</span><span class="sxs-lookup"><span data-stu-id="d2890-139">hello data slices are marked as **Ready** once hello data is available in hello respective store.</span></span>

<span data-ttu-id="d2890-140">Se följande exempel för hello användning av hello hello **externa** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="d2890-140">See hello following example for hello usage of hello **external** property.</span></span> <span data-ttu-id="d2890-141">Du kan också ange **externalData*** om du ställer in externa tootrue.</span><span class="sxs-lookup"><span data-stu-id="d2890-141">You can optionally specify **externalData*** when you set external tootrue.</span></span>

<span data-ttu-id="d2890-142">Se [datauppsättningar](data-factory-create-datasets.md) artikeln för mer information om den här egenskapen.</span><span class="sxs-lookup"><span data-stu-id="d2890-142">See [Datasets](data-factory-create-datasets.md) article for more details about this property.</span></span>

```json
{
  "name": "CustomerTable",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "MyLinkedService",
    "typeProperties": {
      "folderPath": "MyContainer/MySubFolder/",
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "rowDelimiter": ";"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      }
    }
  }
}
```

<span data-ttu-id="d2890-143">tooresolve Hej fel, Lägg till hello **externa** egenskap och hello valfria **externalData** avsnittet toohello JSON-definitionen av hello indatatabellen och återskapa hello tabell.</span><span class="sxs-lookup"><span data-stu-id="d2890-143">tooresolve hello error, add hello **external** property and hello optional **externalData** section toohello JSON definition of hello input table and recreate hello table.</span></span>

### <a name="problem-hybrid-copy-operation-fails"></a><span data-ttu-id="d2890-144">Problem: Hybrid inte går att kopiera</span><span class="sxs-lookup"><span data-stu-id="d2890-144">Problem: Hybrid copy operation fails</span></span>
<span data-ttu-id="d2890-145">Se [felsökning av problem med gateway](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) för tootroubleshoot problem med att kopiera till/från en lokal lagring med hjälp av stegen hello Data Management Gateway.</span><span class="sxs-lookup"><span data-stu-id="d2890-145">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for steps tootroubleshoot issues with copying to/from an on-premises data store using hello Data Management Gateway.</span></span>

### <a name="problem-on-demand-hdinsight-provisioning-fails"></a><span data-ttu-id="d2890-146">Problem: På begäran HDInsight etablerar misslyckas</span><span class="sxs-lookup"><span data-stu-id="d2890-146">Problem: On-demand HDInsight provisioning fails</span></span>
<span data-ttu-id="d2890-147">När du använder en länkad tjänst av typen HDInsightOnDemand måste toospecify en linkedServiceName som pekar tooan Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="d2890-147">When using a linked service of type HDInsightOnDemand, you need toospecify a linkedServiceName that points tooan Azure Blob Storage.</span></span> <span data-ttu-id="d2890-148">Data Factory-tjänsten använder den här lagring toostore loggar och stödfiler för ditt HDInsight-kluster på begäran.</span><span class="sxs-lookup"><span data-stu-id="d2890-148">Data Factory service uses this storage toostore logs and supporting files for your on-demand HDInsight cluster.</span></span>  <span data-ttu-id="d2890-149">Ibland är etableringen av ett HDInsight-kluster på begäran misslyckas med hello följande fel:</span><span class="sxs-lookup"><span data-stu-id="d2890-149">Sometimes provisioning of an on-demand HDInsight cluster fails with hello following error:</span></span>

```
Failed toocreate cluster. Exception: Unable toocomplete hello cluster create operation. Operation failed with code '400'. Cluster left behind state: 'Error'. Message: 'StorageAccountNotColocated'.
```

<span data-ttu-id="d2890-150">Det här felet tyder vanligtvis på att hello hello storage-konto som angetts i hello linkedServiceName inte lagras i hello samma Datacenter plats där hello HDInsight etablerar sker.</span><span class="sxs-lookup"><span data-stu-id="d2890-150">This error usually indicates that hello location of hello storage account specified in hello linkedServiceName is not in hello same data center location where hello HDInsight provisioning is happening.</span></span> <span data-ttu-id="d2890-151">Exempel: om din data factory finns i USA, västra och hello Azure storage i östra USA, hello på begäran etablering misslyckas i USA, västra.</span><span class="sxs-lookup"><span data-stu-id="d2890-151">Example: if your data factory is in West US and hello Azure storage is in East US, hello on-demand provisioning fails in West US.</span></span>

<span data-ttu-id="d2890-152">Dessutom finns det en andra JSON-egenskap, additionalLinkedServiceNames, där ytterligare lagringskonton kan anges i HDInsight på begäran.</span><span class="sxs-lookup"><span data-stu-id="d2890-152">Additionally, there is a second JSON property additionalLinkedServiceNames where additional storage accounts may be specified in on-demand HDInsight.</span></span> <span data-ttu-id="d2890-153">Dessa ytterligare länkade storage-konton måste vara i hello samma plats som hello HDInsight-kluster, eller så misslyckas med hello samma fel.</span><span class="sxs-lookup"><span data-stu-id="d2890-153">Those additional linked storage accounts should be in hello same location as hello HDInsight cluster, or it fails with hello same error.</span></span>

### <a name="problem-custom-net-activity-fails"></a><span data-ttu-id="d2890-154">Problem: Anpassad .NET-aktiviteten misslyckas</span><span class="sxs-lookup"><span data-stu-id="d2890-154">Problem: Custom .NET activity fails</span></span>
<span data-ttu-id="d2890-155">Se [felsöka en pipeline med anpassad aktivitet](data-factory-use-custom-activities.md#troubleshoot-failures) detaljerade anvisningar.</span><span class="sxs-lookup"><span data-stu-id="d2890-155">See [Debug a pipeline with custom activity](data-factory-use-custom-activities.md#troubleshoot-failures) for detailed steps.</span></span>

## <a name="use-azure-portal-tootroubleshoot"></a><span data-ttu-id="d2890-156">Använda Azure portal tootroubleshoot</span><span class="sxs-lookup"><span data-stu-id="d2890-156">Use Azure portal tootroubleshoot</span></span>
### <a name="using-portal-blades"></a><span data-ttu-id="d2890-157">Med hjälp av portalen blad</span><span class="sxs-lookup"><span data-stu-id="d2890-157">Using portal blades</span></span>
<span data-ttu-id="d2890-158">Se [övervakaren pipeline](data-factory-build-your-first-pipeline-using-editor.md#monitor-pipeline) anvisningar.</span><span class="sxs-lookup"><span data-stu-id="d2890-158">See [Monitor pipeline](data-factory-build-your-first-pipeline-using-editor.md#monitor-pipeline) for steps.</span></span>

### <a name="using-monitor-and-manage-app"></a><span data-ttu-id="d2890-159">Övervaka och hantera app</span><span class="sxs-lookup"><span data-stu-id="d2890-159">Using Monitor and Manage App</span></span>
<span data-ttu-id="d2890-160">Se [övervaka och hantera data factory pipelines med övervaka och hantera appen](data-factory-monitor-manage-app.md) mer information.</span><span class="sxs-lookup"><span data-stu-id="d2890-160">See [Monitor and manage data factory pipelines using Monitor and Manage App](data-factory-monitor-manage-app.md) for details.</span></span>

## <a name="use-azure-powershell-tootroubleshoot"></a><span data-ttu-id="d2890-161">Använda Azure PowerShell tootroubleshoot</span><span class="sxs-lookup"><span data-stu-id="d2890-161">Use Azure PowerShell tootroubleshoot</span></span>
### <a name="use-azure-powershell-tootroubleshoot-an-error"></a><span data-ttu-id="d2890-162">Använda Azure PowerShell tootroubleshoot ett fel</span><span class="sxs-lookup"><span data-stu-id="d2890-162">Use Azure PowerShell tootroubleshoot an error</span></span>
<span data-ttu-id="d2890-163">Se [övervakaren Data Factory rörledningar med hjälp av Azure PowerShell](data-factory-build-your-first-pipeline-using-powershell.md#monitor-pipeline) mer information.</span><span class="sxs-lookup"><span data-stu-id="d2890-163">See [Monitor Data Factory pipelines using Azure PowerShell](data-factory-build-your-first-pipeline-using-powershell.md#monitor-pipeline) for details.</span></span>

[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456
[json-scripting-reference]: http://go.microsoft.com/fwlink/?LinkId=516971

[azure-portal]: https://portal.azure.com/

[image-data-factory-troubleshoot-with-error-link]: ./media/data-factory-troubleshoot/DataFactoryWithErrorLink.png

[image-data-factory-troubleshoot-datasets-with-errors-blade]: ./media/data-factory-troubleshoot/DatasetsWithErrorsBlade.png

[image-data-factory-troubleshoot-table-blade-with-problem-slices]: ./media/data-factory-troubleshoot/TableBladeWithProblemSlices.png

[image-data-factory-troubleshoot-activity-run-with-error]: ./media/data-factory-troubleshoot/ActivityRunDetailsWithError.png

[image-data-factory-troubleshoot-dataslice-blade-with-active-runs]: ./media/data-factory-troubleshoot/DataSliceBladeWithActivityRuns.png

[image-data-factory-troubleshoot-walkthrough2-with-errors-link]: ./media/data-factory-troubleshoot/Walkthrough2WithErrorsLink.png

[image-data-factory-troubleshoot-walkthrough2-datasets-with-errors]: ./media/data-factory-troubleshoot/Walkthrough2DataSetsWithErrors.png

[image-data-factory-troubleshoot-walkthrough2-table-with-problem-slices]: ./media/data-factory-troubleshoot/Walkthrough2TableProblemSlices.png

[image-data-factory-troubleshoot-walkthrough2-slice-activity-runs]: ./media/data-factory-troubleshoot/Walkthrough2DataSliceActivityRuns.png

[image-data-factory-troubleshoot-activity-run-details]: ./media/data-factory-troubleshoot/Walkthrough2ActivityRunDetails.png
