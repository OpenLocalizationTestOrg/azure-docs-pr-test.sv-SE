---
title: "aaaMove data från lokala HDFS | Microsoft Docs"
description: "Läs mer om hur toomove data från lokala HDFS med hjälp av Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 3215b82d-291a-46db-8478-eac1a3219614
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jingwang
ms.openlocfilehash: 96387e5dd089099fc2e983ab26d67c2044b973b0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-on-premises-hdfs-using-azure-data-factory"></a><span data-ttu-id="2ff81-103">Flytta data från lokala HDFS med hjälp av Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="2ff81-103">Move data from on-premises HDFS using Azure Data Factory</span></span>
<span data-ttu-id="2ff81-104">Den här artikeln förklarar hur toouse hello Kopieringsaktiviteten i Azure Data Factory toomove data från en lokal HDFS.</span><span class="sxs-lookup"><span data-stu-id="2ff81-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data from an on-premises HDFS.</span></span> <span data-ttu-id="2ff81-105">Den bygger på hello [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som ger en allmän översikt över dataflyttning hello kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="2ff81-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>

<span data-ttu-id="2ff81-106">Du kan kopiera data från datalagret för HDFS tooany stöds mottagare.</span><span class="sxs-lookup"><span data-stu-id="2ff81-106">You can copy data from HDFS tooany supported sink data store.</span></span> <span data-ttu-id="2ff81-107">En lista över data lagras som stöds när egenskaperna av hello kopieringsaktiviteten Se hello [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabell.</span><span class="sxs-lookup"><span data-stu-id="2ff81-107">For a list of data stores supported as sinks by hello copy activity, see hello [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="2ff81-108">Data factory stöder för närvarande endast flytta data från ett datalager för lokala HDFS tooother, men inte för att flytta data från andra data lagras tooan lokala HDFS.</span><span class="sxs-lookup"><span data-stu-id="2ff81-108">Data factory currently supports only moving data from an on-premises HDFS tooother data stores, but not for moving data from other data stores tooan on-premises HDFS.</span></span>

> [!NOTE]
> <span data-ttu-id="2ff81-109">Kopieringsaktiviteten tar inte bort hello källfilen när den inte har kopierade toohello mål.</span><span class="sxs-lookup"><span data-stu-id="2ff81-109">Copy Activity does not delete hello source file after it is successfully copied toohello destination.</span></span> <span data-ttu-id="2ff81-110">Om du behöver toodelete hello källfilen efter en lyckad kopiering, skapa en anpassad aktivitet toodelete hello-fil och använda hello aktivitet i hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="2ff81-110">If you need toodelete hello source file after a successful copy, create a custom activity toodelete hello file and use hello activity in hello pipeline.</span></span> 

## <a name="enabling-connectivity"></a><span data-ttu-id="2ff81-111">Aktivera anslutning</span><span class="sxs-lookup"><span data-stu-id="2ff81-111">Enabling connectivity</span></span>
<span data-ttu-id="2ff81-112">Data Factory-tjänsten har stöd för den anslutande tooon lokala HDFS med hello Data Management Gateway.</span><span class="sxs-lookup"><span data-stu-id="2ff81-112">Data Factory service supports connecting tooon-premises HDFS using hello Data Management Gateway.</span></span> <span data-ttu-id="2ff81-113">Se [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel toolearn om Data Management Gateway och stegvisa instruktioner om hur du konfigurerar hello gateway.</span><span class="sxs-lookup"><span data-stu-id="2ff81-113">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article toolearn about Data Management Gateway and step-by-step instructions on setting up hello gateway.</span></span> <span data-ttu-id="2ff81-114">Använda hello gateway tooconnect tooHDFS även om den finns i en Azure IaaS-VM.</span><span class="sxs-lookup"><span data-stu-id="2ff81-114">Use hello gateway tooconnect tooHDFS even if it is hosted in an Azure IaaS VM.</span></span>

> [!NOTE]
> <span data-ttu-id="2ff81-115">Se till att hello Data Management Gateway kan komma åt för**alla** hello [namn på noden server]: [name nod port] och [nod dataservrar]: [data nod port] hello Hadoop-klustret.</span><span class="sxs-lookup"><span data-stu-id="2ff81-115">Make sure hello Data Management Gateway can access too**ALL** hello [name node server]:[name node port] and [data node servers]:[data node port] of hello Hadoop cluster.</span></span> <span data-ttu-id="2ff81-116">Standard [namn på noden port] är 50070 och standardvärdet [data nod port] är 50075.</span><span class="sxs-lookup"><span data-stu-id="2ff81-116">Default [name node port] is 50070, and default [data node port] is 50075.</span></span>

<span data-ttu-id="2ff81-117">Medan du kan installera gatewayen på hello samma lokala dator eller hello Azure VM hello HDFS rekommenderar vi att du installerar hello gateway på en separat dator/Azure IaaS-VM.</span><span class="sxs-lookup"><span data-stu-id="2ff81-117">While you can install gateway on hello same on-premises machine or hello Azure VM as hello HDFS, we recommend that you install hello gateway on a separate machine/Azure IaaS VM.</span></span> <span data-ttu-id="2ff81-118">Med gatewayen på en separat dator minskar resurskonflikter och förbättrar prestandan.</span><span class="sxs-lookup"><span data-stu-id="2ff81-118">Having gateway on a separate machine reduces resource contention and improves performance.</span></span> <span data-ttu-id="2ff81-119">När du installerar hello gateway på en separat dator ska hello datorn kunna tooaccess hello dator med hello HDFS.</span><span class="sxs-lookup"><span data-stu-id="2ff81-119">When you install hello gateway on a separate machine, hello machine should be able tooaccess hello machine with hello HDFS.</span></span>

## <a name="getting-started"></a><span data-ttu-id="2ff81-120">Komma igång</span><span class="sxs-lookup"><span data-stu-id="2ff81-120">Getting started</span></span>
<span data-ttu-id="2ff81-121">Du kan skapa en pipeline med en kopia-aktivitet som flyttar data från en källa för HDFS med hjälp av olika verktyg/API: er.</span><span class="sxs-lookup"><span data-stu-id="2ff81-121">You can create a pipeline with a copy activity that moves data from a HDFS source by using different tools/APIs.</span></span>

<span data-ttu-id="2ff81-122">hello enklaste sättet toocreate en pipeline är toouse hello **guiden Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="2ff81-122">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="2ff81-123">Se [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden för hello kopiera data.</span><span class="sxs-lookup"><span data-stu-id="2ff81-123">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span>

<span data-ttu-id="2ff81-124">Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall** , **.NET API**, och **REST API**.</span><span class="sxs-lookup"><span data-stu-id="2ff81-124">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="2ff81-125">Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="2ff81-125">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span>

<span data-ttu-id="2ff81-126">Om du använder hello verktyg eller API: er kan utföra du hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalagret:</span><span class="sxs-lookup"><span data-stu-id="2ff81-126">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="2ff81-127">Skapa **länkade tjänster** toolink indata och utdata lagrar tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="2ff81-127">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="2ff81-128">Skapa **datauppsättningar** toorepresent indata och utdata för hello kopieringsåtgärden.</span><span class="sxs-lookup"><span data-stu-id="2ff81-128">Create **datasets** toorepresent input and output data for hello copy operation.</span></span>
3. <span data-ttu-id="2ff81-129">Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata.</span><span class="sxs-lookup"><span data-stu-id="2ff81-129">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span>

<span data-ttu-id="2ff81-130">När du använder guiden hello skapas automatiskt JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och hello pipeline) för dig.</span><span class="sxs-lookup"><span data-stu-id="2ff81-130">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="2ff81-131">När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av hello JSON-format.</span><span class="sxs-lookup"><span data-stu-id="2ff81-131">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="2ff81-132">Ett exempel med JSON-definitioner för Data Factory-entiteter som ska använda toocopy data från ett HDFS-datalager finns [JSON-exempel: kopiera data från lokala HDFS tooAzure Blob](#json-example-copy-data-from-on-premises-hdfs-to-azure-blob) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="2ff81-132">For a sample with JSON definitions for Data Factory entities that are used toocopy data from a HDFS data store, see [JSON example: Copy data from on-premises HDFS tooAzure Blob](#json-example-copy-data-from-on-premises-hdfs-to-azure-blob) section of this article.</span></span>

<span data-ttu-id="2ff81-133">hello följande avsnitt innehåller information om JSON-egenskaper som är specifika tooHDFS för används toodefine Data Factory-enheter:</span><span class="sxs-lookup"><span data-stu-id="2ff81-133">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooHDFS:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="2ff81-134">Länkad tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="2ff81-134">Linked service properties</span></span>
<span data-ttu-id="2ff81-135">En länkad tjänst länkar en data store tooa data factory.</span><span class="sxs-lookup"><span data-stu-id="2ff81-135">A linked service links a data store tooa data factory.</span></span> <span data-ttu-id="2ff81-136">Du skapar en länkad tjänst av typen **Hdfs** toolink ett lokalt HDFS tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="2ff81-136">You create a linked service of type **Hdfs** toolink an on-premises HDFS tooyour data factory.</span></span> <span data-ttu-id="2ff81-137">hello följande tabell innehåller en beskrivning för JSON-element specifika tooHDFS länkad tjänst.</span><span class="sxs-lookup"><span data-stu-id="2ff81-137">hello following table provides description for JSON elements specific tooHDFS linked service.</span></span>

| <span data-ttu-id="2ff81-138">Egenskap</span><span class="sxs-lookup"><span data-stu-id="2ff81-138">Property</span></span> | <span data-ttu-id="2ff81-139">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2ff81-139">Description</span></span> | <span data-ttu-id="2ff81-140">Krävs</span><span class="sxs-lookup"><span data-stu-id="2ff81-140">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="2ff81-141">typ</span><span class="sxs-lookup"><span data-stu-id="2ff81-141">type</span></span> |<span data-ttu-id="2ff81-142">hello Typegenskapen måste anges till: **Hdfs**</span><span class="sxs-lookup"><span data-stu-id="2ff81-142">hello type property must be set to: **Hdfs**</span></span> |<span data-ttu-id="2ff81-143">Ja</span><span class="sxs-lookup"><span data-stu-id="2ff81-143">Yes</span></span> |
| <span data-ttu-id="2ff81-144">URL</span><span class="sxs-lookup"><span data-stu-id="2ff81-144">Url</span></span> |<span data-ttu-id="2ff81-145">URL: en toohello HDFS</span><span class="sxs-lookup"><span data-stu-id="2ff81-145">URL toohello HDFS</span></span> |<span data-ttu-id="2ff81-146">Ja</span><span class="sxs-lookup"><span data-stu-id="2ff81-146">Yes</span></span> |
| <span data-ttu-id="2ff81-147">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="2ff81-147">authenticationType</span></span> |<span data-ttu-id="2ff81-148">Anonym, eller Windows.</span><span class="sxs-lookup"><span data-stu-id="2ff81-148">Anonymous, or Windows.</span></span> <br><br> <span data-ttu-id="2ff81-149">toouse **Kerberos-autentisering** HDFS-anslutningen finns för[i det här avsnittet](#use-kerberos-authentication-for-hdfs-connector) tooset upp din lokala miljö därefter.</span><span class="sxs-lookup"><span data-stu-id="2ff81-149">toouse **Kerberos authentication** for HDFS connector, refer too[this section](#use-kerberos-authentication-for-hdfs-connector) tooset up your on-premises environment accordingly.</span></span> |<span data-ttu-id="2ff81-150">Ja</span><span class="sxs-lookup"><span data-stu-id="2ff81-150">Yes</span></span> |
| <span data-ttu-id="2ff81-151">Användarnamn</span><span class="sxs-lookup"><span data-stu-id="2ff81-151">userName</span></span> |<span data-ttu-id="2ff81-152">Användarnamn för Windows-autentisering.</span><span class="sxs-lookup"><span data-stu-id="2ff81-152">Username for Windows authentication.</span></span> |<span data-ttu-id="2ff81-153">Ja (för Windows-autentisering)</span><span class="sxs-lookup"><span data-stu-id="2ff81-153">Yes (for Windows Authentication)</span></span> |
| <span data-ttu-id="2ff81-154">lösenord</span><span class="sxs-lookup"><span data-stu-id="2ff81-154">password</span></span> |<span data-ttu-id="2ff81-155">Lösenordet för Windows-autentisering.</span><span class="sxs-lookup"><span data-stu-id="2ff81-155">Password for Windows authentication.</span></span> |<span data-ttu-id="2ff81-156">Ja (för Windows-autentisering)</span><span class="sxs-lookup"><span data-stu-id="2ff81-156">Yes (for Windows Authentication)</span></span> |
| <span data-ttu-id="2ff81-157">gatewayName</span><span class="sxs-lookup"><span data-stu-id="2ff81-157">gatewayName</span></span> |<span data-ttu-id="2ff81-158">Namnet på hello-gateway som hello Data Factory-tjänsten ska använda tooconnect toohello HDFS.</span><span class="sxs-lookup"><span data-stu-id="2ff81-158">Name of hello gateway that hello Data Factory service should use tooconnect toohello HDFS.</span></span> |<span data-ttu-id="2ff81-159">Ja</span><span class="sxs-lookup"><span data-stu-id="2ff81-159">Yes</span></span> |
| <span data-ttu-id="2ff81-160">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="2ff81-160">encryptedCredential</span></span> |<span data-ttu-id="2ff81-161">[Nya AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) utdata från hello autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="2ff81-161">[New-AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) output of hello access credential.</span></span> |<span data-ttu-id="2ff81-162">Nej</span><span class="sxs-lookup"><span data-stu-id="2ff81-162">No</span></span> |

### <a name="using-anonymous-authentication"></a><span data-ttu-id="2ff81-163">Använder anonym autentisering</span><span class="sxs-lookup"><span data-stu-id="2ff81-163">Using Anonymous authentication</span></span>

```JSON
{
    "name": "hdfs",
    "properties":
    {
        "type": "Hdfs",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "userName": "hadoop",
            "url" : "http://<machine>:50070/webhdfs/v1/",
            "gatewayName": "mygateway"
        }
    }
}
```

### <a name="using-windows-authentication"></a><span data-ttu-id="2ff81-164">Med Windows-autentisering</span><span class="sxs-lookup"><span data-stu-id="2ff81-164">Using Windows authentication</span></span>

```JSON
{
    "name": "hdfs",
    "properties":
    {
        "type": "Hdfs",
        "typeProperties":
        {
            "authenticationType": "Windows",
            "userName": "Administrator",
            "password": "password",
            "url" : "http://<machine>:50070/webhdfs/v1/",
            "gatewayName": "mygateway"
        }
    }
}
```
## <a name="dataset-properties"></a><span data-ttu-id="2ff81-165">Egenskaper för datamängd</span><span class="sxs-lookup"><span data-stu-id="2ff81-165">Dataset properties</span></span>
<span data-ttu-id="2ff81-166">En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns hello [skapa datauppsättningar](data-factory-create-datasets.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="2ff81-166">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="2ff81-167">Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av dataset (Azure SQL Azure blob, Azure-tabellen, osv.).</span><span class="sxs-lookup"><span data-stu-id="2ff81-167">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="2ff81-168">Hej **typeProperties** avsnitt är olika för varje typ av dataset och ger information om hello platsen för hello data i datalagret hello.</span><span class="sxs-lookup"><span data-stu-id="2ff81-168">hello **typeProperties** section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="2ff81-169">Hej typeProperties avsnittet för dataset av typen **filresursen** (som omfattar HDFS dataset) har hello följande egenskaper</span><span class="sxs-lookup"><span data-stu-id="2ff81-169">hello typeProperties section for dataset of type **FileShare** (which includes HDFS dataset) has hello following properties</span></span>

| <span data-ttu-id="2ff81-170">Egenskap</span><span class="sxs-lookup"><span data-stu-id="2ff81-170">Property</span></span> | <span data-ttu-id="2ff81-171">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2ff81-171">Description</span></span> | <span data-ttu-id="2ff81-172">Krävs</span><span class="sxs-lookup"><span data-stu-id="2ff81-172">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="2ff81-173">folderPath</span><span class="sxs-lookup"><span data-stu-id="2ff81-173">folderPath</span></span> |<span data-ttu-id="2ff81-174">Sökvägen toohello mapp.</span><span class="sxs-lookup"><span data-stu-id="2ff81-174">Path toohello folder.</span></span> <span data-ttu-id="2ff81-175">Exempel:`myfolder`</span><span class="sxs-lookup"><span data-stu-id="2ff81-175">Example: `myfolder`</span></span><br/><br/><span data-ttu-id="2ff81-176">Använda escape-tecknet ' \ ' för specialtecken i hello-sträng.</span><span class="sxs-lookup"><span data-stu-id="2ff81-176">Use escape character ‘ \ ’ for special characters in hello string.</span></span> <span data-ttu-id="2ff81-177">Till exempel: Ange mapp för folder\subfolder,\\\\undermapp och ange d: för d:\samplefolder,\\\\Exempelmapp.</span><span class="sxs-lookup"><span data-stu-id="2ff81-177">For example: for folder\subfolder, specify folder\\\\subfolder and for d:\samplefolder, specify d:\\\\samplefolder.</span></span><br/><br/><span data-ttu-id="2ff81-178">Du kan kombinera den här egenskapen med **partitionBy** toohave mappsökvägar baserat på sektorn börja/sluta datum gånger.</span><span class="sxs-lookup"><span data-stu-id="2ff81-178">You can combine this property with **partitionBy** toohave folder paths based on slice start/end date-times.</span></span> |<span data-ttu-id="2ff81-179">Ja</span><span class="sxs-lookup"><span data-stu-id="2ff81-179">Yes</span></span> |
| <span data-ttu-id="2ff81-180">fileName</span><span class="sxs-lookup"><span data-stu-id="2ff81-180">fileName</span></span> |<span data-ttu-id="2ff81-181">Ange hello namnet på hello-filen i hello **folderPath** om du vill hello tabell toorefer tooa specifika filen i mappen hello.</span><span class="sxs-lookup"><span data-stu-id="2ff81-181">Specify hello name of hello file in hello **folderPath** if you want hello table toorefer tooa specific file in hello folder.</span></span> <span data-ttu-id="2ff81-182">Om du inte anger något värde för den här egenskapen pekar hello tabell tooall filer i hello-mappen.</span><span class="sxs-lookup"><span data-stu-id="2ff81-182">If you do not specify any value for this property, hello table points tooall files in hello folder.</span></span><br/><br/><span data-ttu-id="2ff81-183">Om filnamnet inte anges för en utdatauppsättningen skulle hello namnet på hello genereras vara i hello efter det här formatet:</span><span class="sxs-lookup"><span data-stu-id="2ff81-183">When fileName is not specified for an output dataset, hello name of hello generated file would be in hello following this format:</span></span> <br/><br/><span data-ttu-id="2ff81-184">Data. <Guid>.txt (till exempel:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span><span class="sxs-lookup"><span data-stu-id="2ff81-184">Data.<Guid>.txt (for example: : Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span></span> |<span data-ttu-id="2ff81-185">Nej</span><span class="sxs-lookup"><span data-stu-id="2ff81-185">No</span></span> |
| <span data-ttu-id="2ff81-186">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="2ff81-186">partitionedBy</span></span> |<span data-ttu-id="2ff81-187">partitionedBy kan vara används toospecify en dynamisk folderPath filnamn för tid series-data.</span><span class="sxs-lookup"><span data-stu-id="2ff81-187">partitionedBy can be used toospecify a dynamic folderPath, filename for time series data.</span></span> <span data-ttu-id="2ff81-188">Exempel: folderPath parametriserade varje timme av data.</span><span class="sxs-lookup"><span data-stu-id="2ff81-188">Example: folderPath parameterized for every hour of data.</span></span> |<span data-ttu-id="2ff81-189">Nej</span><span class="sxs-lookup"><span data-stu-id="2ff81-189">No</span></span> |
| <span data-ttu-id="2ff81-190">Format</span><span class="sxs-lookup"><span data-stu-id="2ff81-190">format</span></span> | <span data-ttu-id="2ff81-191">hello följande formattyper stöds: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="2ff81-191">hello following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="2ff81-192">Ange hello **typen** egenskap under format tooone av dessa värden.</span><span class="sxs-lookup"><span data-stu-id="2ff81-192">Set hello **type** property under format tooone of these values.</span></span> <span data-ttu-id="2ff81-193">Mer information finns i [textformat](data-factory-supported-file-and-compression-formats.md#text-format), [Json-Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-formatet](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), och [parkettgolv Format](data-factory-supported-file-and-compression-formats.md#parquet-format) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="2ff81-193">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="2ff81-194">Om du vill använda för**kopiera filer som-är** mellan filbaserade butiker (binär kopia), hoppar du över hello format-avsnittet i både inkommande och utgående dataset-definitioner.</span><span class="sxs-lookup"><span data-stu-id="2ff81-194">If you want too**copy files as-is** between file-based stores (binary copy), skip hello format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="2ff81-195">Nej</span><span class="sxs-lookup"><span data-stu-id="2ff81-195">No</span></span> |
| <span data-ttu-id="2ff81-196">Komprimering</span><span class="sxs-lookup"><span data-stu-id="2ff81-196">compression</span></span> | <span data-ttu-id="2ff81-197">Ange hello typ och kompression för hello data.</span><span class="sxs-lookup"><span data-stu-id="2ff81-197">Specify hello type and level of compression for hello data.</span></span> <span data-ttu-id="2ff81-198">Typer som stöds är: **GZip**, **Deflate**, **BZip2**, och **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="2ff81-198">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="2ff81-199">Nivåer som stöds är: **Optimal** och **snabbast**.</span><span class="sxs-lookup"><span data-stu-id="2ff81-199">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="2ff81-200">Mer information finns i [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="2ff81-200">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="2ff81-201">Nej</span><span class="sxs-lookup"><span data-stu-id="2ff81-201">No</span></span> |

> [!NOTE]
> <span data-ttu-id="2ff81-202">filnamnet och fileFilter kan inte användas samtidigt.</span><span class="sxs-lookup"><span data-stu-id="2ff81-202">filename and fileFilter cannot be used simultaneously.</span></span>

### <a name="using-partionedby-property"></a><span data-ttu-id="2ff81-203">Med hjälp av egenskapen partionedBy</span><span class="sxs-lookup"><span data-stu-id="2ff81-203">Using partionedBy property</span></span>
<span data-ttu-id="2ff81-204">Som nämnts tidigare under hello, du kan ange en dynamisk folderPath och ett filnamn för tid series-data med hello **partitionedBy** egenskapen [Data Factory-funktioner och hello systemvariabler](data-factory-functions-variables.md).</span><span class="sxs-lookup"><span data-stu-id="2ff81-204">As mentioned in hello previous section, you can specify a dynamic folderPath and filename for time series data with hello **partitionedBy** property, [Data Factory functions, and hello system variables](data-factory-functions-variables.md).</span></span>

<span data-ttu-id="2ff81-205">toolearn mer om tid serien datauppsättningar, schemaläggning och segment, se [skapa datauppsättningar](data-factory-create-datasets.md), [schemaläggning och utförande](data-factory-scheduling-and-execution.md), och [skapar Pipelines](data-factory-create-pipelines.md) artiklar.</span><span class="sxs-lookup"><span data-stu-id="2ff81-205">toolearn more about time series datasets, scheduling, and slices, see [Creating Datasets](data-factory-create-datasets.md), [Scheduling & Execution](data-factory-scheduling-and-execution.md), and [Creating Pipelines](data-factory-create-pipelines.md) articles.</span></span>

#### <a name="sample-1"></a><span data-ttu-id="2ff81-206">Exempel 1:</span><span class="sxs-lookup"><span data-stu-id="2ff81-206">Sample 1:</span></span>

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```
<span data-ttu-id="2ff81-207">I det här exemplet {segment} ersätts med hello värde för Data Factory systemvariabel SliceStart hello-format (YYYYMMDDHH).</span><span class="sxs-lookup"><span data-stu-id="2ff81-207">In this example {Slice} is replaced with hello value of Data Factory system variable SliceStart in hello format (YYYYMMDDHH) specified.</span></span> <span data-ttu-id="2ff81-208">Hej SliceStart refererar toostart tiden för hello sektorn.</span><span class="sxs-lookup"><span data-stu-id="2ff81-208">hello SliceStart refers toostart time of hello slice.</span></span> <span data-ttu-id="2ff81-209">hello folderPath är olika för varje segment.</span><span class="sxs-lookup"><span data-stu-id="2ff81-209">hello folderPath is different for each slice.</span></span> <span data-ttu-id="2ff81-210">Till exempel: 2014100103-wikidatagateway/wikisampledataout eller 2014100104-wikidatagateway/wikisampledataout.</span><span class="sxs-lookup"><span data-stu-id="2ff81-210">For example: wikidatagateway/wikisampledataout/2014100103 or wikidatagateway/wikisampledataout/2014100104.</span></span>

#### <a name="sample-2"></a><span data-ttu-id="2ff81-211">Exempel 2:</span><span class="sxs-lookup"><span data-stu-id="2ff81-211">Sample 2:</span></span>

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
"fileName": "{Hour}.csv",
"partitionedBy":
 [
    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
    { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
],
```
<span data-ttu-id="2ff81-212">I det här exemplet extraheras år, månad, dag och tidpunkt för SliceStart till olika variabler som används av egenskaperna folderPath och filnamn.</span><span class="sxs-lookup"><span data-stu-id="2ff81-212">In this example, year, month, day, and time of SliceStart are extracted into separate variables that are used by folderPath and fileName properties.</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="2ff81-213">Kopiera egenskaper för aktivitet</span><span class="sxs-lookup"><span data-stu-id="2ff81-213">Copy activity properties</span></span>
<span data-ttu-id="2ff81-214">En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns hello [skapar Pipelines](data-factory-create-pipelines.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="2ff81-214">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="2ff81-215">Egenskaper som namn, beskrivning, ingående och utgående tabeller och principer är tillgängliga för alla typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="2ff81-215">Properties such as name, description, input and output tables, and policies are available for all types of activities.</span></span>

<span data-ttu-id="2ff81-216">De egenskaper som är tillgängliga under hello typeProperties i hello aktivitet varierar beroende på varje aktivitetstyp.</span><span class="sxs-lookup"><span data-stu-id="2ff81-216">Whereas, properties available in hello typeProperties section of hello activity vary with each activity type.</span></span> <span data-ttu-id="2ff81-217">För Kopieringsaktivitet kan varierar de beroende på hello typer av datakällor och sänkor.</span><span class="sxs-lookup"><span data-stu-id="2ff81-217">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="2ff81-218">För Kopieringsaktiviteten när datakällan är av typen **FileSystemSource** hello följande egenskaper finns i avsnittet typeProperties:</span><span class="sxs-lookup"><span data-stu-id="2ff81-218">For Copy Activity, when source is of type **FileSystemSource** hello following properties are available in typeProperties section:</span></span>

<span data-ttu-id="2ff81-219">**FileSystemSource** stöder hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="2ff81-219">**FileSystemSource** supports hello following properties:</span></span>

| <span data-ttu-id="2ff81-220">Egenskap</span><span class="sxs-lookup"><span data-stu-id="2ff81-220">Property</span></span> | <span data-ttu-id="2ff81-221">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2ff81-221">Description</span></span> | <span data-ttu-id="2ff81-222">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="2ff81-222">Allowed values</span></span> | <span data-ttu-id="2ff81-223">Krävs</span><span class="sxs-lookup"><span data-stu-id="2ff81-223">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="2ff81-224">Rekursiva</span><span class="sxs-lookup"><span data-stu-id="2ff81-224">recursive</span></span> |<span data-ttu-id="2ff81-225">Anger om hello data läses rekursivt från hello undermappar eller endast hello angivna mappen.</span><span class="sxs-lookup"><span data-stu-id="2ff81-225">Indicates whether hello data is read recursively from hello sub folders or only from hello specified folder.</span></span> |<span data-ttu-id="2ff81-226">SANT, FALSKT (standard)</span><span class="sxs-lookup"><span data-stu-id="2ff81-226">True, False (default)</span></span> |<span data-ttu-id="2ff81-227">Nej</span><span class="sxs-lookup"><span data-stu-id="2ff81-227">No</span></span> |

## <a name="supported-file-and-compression-formats"></a><span data-ttu-id="2ff81-228">Fil- och komprimering format som stöds</span><span class="sxs-lookup"><span data-stu-id="2ff81-228">Supported file and compression formats</span></span>
<span data-ttu-id="2ff81-229">Se [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md) artikeln för information.</span><span class="sxs-lookup"><span data-stu-id="2ff81-229">See [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md) article on details.</span></span>

## <a name="json-example-copy-data-from-on-premises-hdfs-tooazure-blob"></a><span data-ttu-id="2ff81-230">JSON-exempel: kopiera data från lokala HDFS tooAzure Blob</span><span class="sxs-lookup"><span data-stu-id="2ff81-230">JSON example: Copy data from on-premises HDFS tooAzure Blob</span></span>
<span data-ttu-id="2ff81-231">Det här exemplet visas hur toocopy data från en lokal HDFS tooAzure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="2ff81-231">This sample shows how toocopy data from an on-premises HDFS tooAzure Blob Storage.</span></span> <span data-ttu-id="2ff81-232">Dock datan kan kopieras **direkt** tooany av hello sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hello Kopieringsaktiviteten i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="2ff81-232">However, data can be copied **directly** tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>  

<span data-ttu-id="2ff81-233">hello exempel innehåller JSON definitioner för hello följande Data Factory entiteter.</span><span class="sxs-lookup"><span data-stu-id="2ff81-233">hello sample provides JSON definitions for hello following Data Factory entities.</span></span> <span data-ttu-id="2ff81-234">Du kan använda dessa definitioner toocreate en pipeline toocopy data från HDFS tooAzure Blob Storage med hjälp av [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="2ff81-234">You can use these definitions toocreate a pipeline toocopy data from HDFS tooAzure Blob Storage by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span>

1. <span data-ttu-id="2ff81-235">En länkad tjänst av typen [OnPremisesHdfs](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="2ff81-235">A linked service of type [OnPremisesHdfs](#linked-service-properties).</span></span>
2. <span data-ttu-id="2ff81-236">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="2ff81-236">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="2ff81-237">Indata [dataset](data-factory-create-datasets.md) av typen [filresursen](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="2ff81-237">An input [dataset](data-factory-create-datasets.md) of type [FileShare](#dataset-properties).</span></span>
4. <span data-ttu-id="2ff81-238">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="2ff81-238">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="2ff81-239">En [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [FileSystemSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="2ff81-239">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [FileSystemSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="2ff81-240">hello exemplet kopierar data från en lokal HDFS tooan Azure blob varje timme.</span><span class="sxs-lookup"><span data-stu-id="2ff81-240">hello sample copies data from an on-premises HDFS tooan Azure blob every hour.</span></span> <span data-ttu-id="2ff81-241">hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="2ff81-241">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="2ff81-242">Som ett första steg bör du ställa in hello data management gateway.</span><span class="sxs-lookup"><span data-stu-id="2ff81-242">As a first step, set up hello data management gateway.</span></span> <span data-ttu-id="2ff81-243">Hej instruktionerna i hello [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="2ff81-243">hello instructions in hello [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="2ff81-244">**HDFS länkade tjänsten:** det här exemplet använder hello Windows-autentisering.</span><span class="sxs-lookup"><span data-stu-id="2ff81-244">**HDFS linked service:** This example uses hello Windows authentication.</span></span> <span data-ttu-id="2ff81-245">Se [HDFS länkade tjänsten](#linked-service-properties) avsnittet för olika typer av autentisering som du kan använda.</span><span class="sxs-lookup"><span data-stu-id="2ff81-245">See [HDFS linked service](#linked-service-properties) section for different types of authentication you can use.</span></span>

```JSON
{
    "name": "HDFSLinkedService",
    "properties":
    {
        "type": "Hdfs",
        "typeProperties":
        {
            "authenticationType": "Windows",
            "userName": "Administrator",
            "password": "password",
            "url" : "http://<machine>:50070/webhdfs/v1/",
            "gatewayName": "mygateway"
        }
    }
}
```

<span data-ttu-id="2ff81-246">**Azure Storage länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="2ff81-246">**Azure Storage linked service:**</span></span>

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

<span data-ttu-id="2ff81-247">**HDFS indatauppsättning:** den här datamängden refererar toohello HDFS mappen DataTransfer/UnitTest /.</span><span class="sxs-lookup"><span data-stu-id="2ff81-247">**HDFS input dataset:** This dataset refers toohello HDFS folder DataTransfer/UnitTest/.</span></span> <span data-ttu-id="2ff81-248">hello pipeline kopierar alla hello-filer i den här mappen toohello målet.</span><span class="sxs-lookup"><span data-stu-id="2ff81-248">hello pipeline copies all hello files in this folder toohello destination.</span></span>

<span data-ttu-id="2ff81-249">Inställningen ”externa”: ”true” informerar hello Data Factory-tjänsten som hello dataset är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="2ff81-249">Setting “external”: ”true” informs hello Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

```JSON
{
    "name": "InputDataset",
    "properties": {
        "type": "FileShare",
        "linkedServiceName": "HDFSLinkedService",
        "typeProperties": {
            "folderPath": "DataTransfer/UnitTest/"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```

<span data-ttu-id="2ff81-250">**Azure Blob utdatauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="2ff81-250">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="2ff81-251">Data skrivs tooa nya blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="2ff81-251">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="2ff81-252">hello mappsökväg för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="2ff81-252">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="2ff81-253">hello mappsökväg använder år, månad, dag och timmar delar av hello starttid.</span><span class="sxs-lookup"><span data-stu-id="2ff81-253">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

```JSON
{
    "name": "OutputDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/hdfs/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            },
            "partitionedBy": [
                {
                    "name": "Year",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "yyyy"
                    }
                },
                {
                    "name": "Month",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "MM"
                    }
                },
                {
                    "name": "Day",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "dd"
                    }
                },
                {
                    "name": "Hour",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "HH"
                    }
                }
            ]
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

<span data-ttu-id="2ff81-254">**Kopieringsaktiviteten i en pipeline med filsystemet källa och mottagare för Blob:**</span><span class="sxs-lookup"><span data-stu-id="2ff81-254">**A copy activity in a pipeline with File System source and Blob sink:**</span></span>

<span data-ttu-id="2ff81-255">hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse dessa indata och utdata-datauppsättningar och schemalagda toorun varje timme.</span><span class="sxs-lookup"><span data-stu-id="2ff81-255">hello pipeline contains a Copy Activity that is configured toouse these input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="2ff81-256">I hello pipeline JSON-definitionen hello **källa** typ har angetts för**FileSystemSource** och **sink** typ har angetts för**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="2ff81-256">In hello pipeline JSON definition, hello **source** type is set too**FileSystemSource** and **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="2ff81-257">hello SQL-frågan som angetts för hello **frågan** egenskapen väljer hello data i hello tidigare timme toocopy.</span><span class="sxs-lookup"><span data-stu-id="2ff81-257">hello SQL query specified for hello **query** property selects hello data in hello past hour toocopy.</span></span>

```JSON
{
    "name": "pipeline",
    "properties":
    {
        "activities":
        [
            {
                "name": "HdfsToBlobCopy",
                "inputs": [ {"name": "InputDataset"} ],
                "outputs": [ {"name": "OutputDataset"} ],
                "type": "Copy",
                "typeProperties":
                {
                    "source":
                    {
                        "type": "FileSystemSource"
                    },
                    "sink":
                    {
                        "type": "BlobSink"
                    }
                },
                "policy":
                {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "00:05:00"
                }
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z"
    }
}
```

## <a name="use-kerberos-authentication-for-hdfs-connector"></a><span data-ttu-id="2ff81-258">Använda Kerberos-autentisering för HDFS-anslutningen</span><span class="sxs-lookup"><span data-stu-id="2ff81-258">Use Kerberos authentication for HDFS connector</span></span>
<span data-ttu-id="2ff81-259">Det finns två alternativ tooset in hello lokala miljö så toouse Kerberos-autentisering i HDFS-koppling.</span><span class="sxs-lookup"><span data-stu-id="2ff81-259">There are two options tooset up hello on-premises environment so as toouse Kerberos Authentication in HDFS connector.</span></span> <span data-ttu-id="2ff81-260">Du kan välja hello en bättre passar ditt ärende.</span><span class="sxs-lookup"><span data-stu-id="2ff81-260">You can choose hello one better fits your case.</span></span>
* <span data-ttu-id="2ff81-261">Alternativ 1: [anslutning till gateway-datorn i Kerberos-sfär](#kerberos-join-realm)</span><span class="sxs-lookup"><span data-stu-id="2ff81-261">Option 1: [Join gateway machine in Kerberos realm](#kerberos-join-realm)</span></span>
* <span data-ttu-id="2ff81-262">Alternativ 2: [aktivera ömsesidigt förtroende mellan Windows-domän och Kerberos-sfär](#kerberos-mutual-trust)</span><span class="sxs-lookup"><span data-stu-id="2ff81-262">Option 2: [Enable mutual trust between Windows domain and Kerberos realm](#kerberos-mutual-trust)</span></span>

### <span data-ttu-id="2ff81-263"><a name="kerberos-join-realm"></a>Alternativ 1: Ansluta till gateway-datorn i Kerberos-sfär</span><span class="sxs-lookup"><span data-stu-id="2ff81-263"><a name="kerberos-join-realm"></a>Option 1: Join gateway machine in Kerberos realm</span></span>

#### <a name="requirement"></a><span data-ttu-id="2ff81-264">Krav:</span><span class="sxs-lookup"><span data-stu-id="2ff81-264">Requirement:</span></span>

* <span data-ttu-id="2ff81-265">hello gateway-datorn måste toojoin hello Kerberos-sfär och kan inte ansluta till en Windows-domän.</span><span class="sxs-lookup"><span data-stu-id="2ff81-265">hello gateway machine needs toojoin hello Kerberos realm and can’t join any Windows domain.</span></span>

#### <a name="how-tooconfigure"></a><span data-ttu-id="2ff81-266">Hur tooconfigure:</span><span class="sxs-lookup"><span data-stu-id="2ff81-266">How tooconfigure:</span></span>

<span data-ttu-id="2ff81-267">**På gateway-datorn:**</span><span class="sxs-lookup"><span data-stu-id="2ff81-267">**On gateway machine:**</span></span>

1.  <span data-ttu-id="2ff81-268">Kör hello **Ksetup** verktyget tooconfigure hello Kerberos KDC-server och sfären.</span><span class="sxs-lookup"><span data-stu-id="2ff81-268">Run hello **Ksetup** utility tooconfigure hello Kerberos KDC server and realm.</span></span>

    <span data-ttu-id="2ff81-269">hello datorn måste konfigureras som en medlem i en arbetsgrupp, eftersom en Kerberos-sfär skiljer sig från en Windows-domän.</span><span class="sxs-lookup"><span data-stu-id="2ff81-269">hello machine must be configured as a member of a workgroup since a Kerberos realm is different from a Windows domain.</span></span> <span data-ttu-id="2ff81-270">Detta kan uppnås genom att ange hello Kerberos-sfär och lägga till en KDC-server på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="2ff81-270">This can be achieved by setting hello Kerberos realm and adding a KDC server as follows.</span></span> <span data-ttu-id="2ff81-271">Ersätt *REALM.COM* med din egen respektive sfär efter behov.</span><span class="sxs-lookup"><span data-stu-id="2ff81-271">Replace *REALM.COM* with your own respective realm as needed.</span></span>

            C:> Ksetup /setdomain REALM.COM
            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>

    <span data-ttu-id="2ff81-272">**Starta om** hello datorn efter körning kommandona 2.</span><span class="sxs-lookup"><span data-stu-id="2ff81-272">**Restart** hello machine after executing these 2 commands.</span></span>

2.  <span data-ttu-id="2ff81-273">Verifiera konfigurationen av hello med **Ksetup** kommando.</span><span class="sxs-lookup"><span data-stu-id="2ff81-273">Verify hello configuration with **Ksetup** command.</span></span> <span data-ttu-id="2ff81-274">hello utdata ska vara som:</span><span class="sxs-lookup"><span data-stu-id="2ff81-274">hello output should be like:</span></span>

            C:> Ksetup
            default realm = REALM.COM (external)
            REALM.com:
                kdc = <your_kdc_server_address>

<span data-ttu-id="2ff81-275">**I Azure Data Factory:**</span><span class="sxs-lookup"><span data-stu-id="2ff81-275">**In Azure Data Factory:**</span></span>

* <span data-ttu-id="2ff81-276">Konfigurera hello HDFS en koppling med hjälp av **Windows-autentisering** tillsammans med Kerberos-huvudnamn namn och lösenord tooconnect toohello HDFS datakällan.</span><span class="sxs-lookup"><span data-stu-id="2ff81-276">Configure hello HDFS connector using **Windows authentication** together with your Kerberos principal name and password tooconnect toohello HDFS data source.</span></span> <span data-ttu-id="2ff81-277">Kontrollera [HDFS länkade tjänstegenskaper](#linked-service-properties) avsnittet konfigurationsinformation.</span><span class="sxs-lookup"><span data-stu-id="2ff81-277">Check [HDFS Linked Service properties](#linked-service-properties) section on configuration details.</span></span>

### <span data-ttu-id="2ff81-278"><a name="kerberos-mutual-trust"></a>Alternativ 2: Aktivera ömsesidigt förtroende mellan Windows-domän och Kerberos-sfär</span><span class="sxs-lookup"><span data-stu-id="2ff81-278"><a name="kerberos-mutual-trust"></a>Option 2: Enable mutual trust between Windows domain and Kerberos realm</span></span>

#### <a name="requirement"></a><span data-ttu-id="2ff81-279">Krav:</span><span class="sxs-lookup"><span data-stu-id="2ff81-279">Requirement:</span></span>
*   <span data-ttu-id="2ff81-280">hello gateway-datorn måste ansluta till en Windows-domän.</span><span class="sxs-lookup"><span data-stu-id="2ff81-280">hello gateway machine must join a Windows domain.</span></span>
*   <span data-ttu-id="2ff81-281">Du behöver behörighet tooupdate hello domänkontrollantens inställningar.</span><span class="sxs-lookup"><span data-stu-id="2ff81-281">You need permission tooupdate hello domain controller's settings.</span></span>

#### <a name="how-tooconfigure"></a><span data-ttu-id="2ff81-282">Hur tooconfigure:</span><span class="sxs-lookup"><span data-stu-id="2ff81-282">How tooconfigure:</span></span>

> [!NOTE]
> <span data-ttu-id="2ff81-283">Ersätt REALM.COM och AD.COM i hello följande självstudier med din egen respektive domän och domänkontrollanten efter behov.</span><span class="sxs-lookup"><span data-stu-id="2ff81-283">Replace REALM.COM and AD.COM in hello following tutorial with your own respective realm and domain controller as needed.</span></span>

<span data-ttu-id="2ff81-284">**På KDC-server:**</span><span class="sxs-lookup"><span data-stu-id="2ff81-284">**On KDC server:**</span></span>

1.  <span data-ttu-id="2ff81-285">Redigera hello KDC konfigurationen i **krb5.conf** filen toolet KDC förtroende hänvisar toohello följande konfigurationsmallen Windows-domän.</span><span class="sxs-lookup"><span data-stu-id="2ff81-285">Edit hello KDC configuration in **krb5.conf** file toolet KDC trust Windows Domain referring toohello following configuration template.</span></span> <span data-ttu-id="2ff81-286">Som standard hello konfiguration finns på **/etc/krb5.conf**.</span><span class="sxs-lookup"><span data-stu-id="2ff81-286">By default, hello configuration is located at **/etc/krb5.conf**.</span></span>

            [logging]
             default = FILE:/var/log/krb5libs.log
             kdc = FILE:/var/log/krb5kdc.log
             admin_server = FILE:/var/log/kadmind.log

            [libdefaults]
             default_realm = REALM.COM
             dns_lookup_realm = false
             dns_lookup_kdc = false
             ticket_lifetime = 24h
             renew_lifetime = 7d
             forwardable = true

            [realms]
             REALM.COM = {
              kdc = node.REALM.COM
              admin_server = node.REALM.COM
             }
            AD.COM = {
             kdc = windc.ad.com
             admin_server = windc.ad.com
            }

            [domain_realm]
             .REALM.COM = REALM.COM
             REALM.COM = REALM.COM
             .ad.com = AD.COM
             ad.com = AD.COM

            [capaths]
             AD.COM = {
              REALM.COM = .
             }

  <span data-ttu-id="2ff81-287">**Starta om** hello KDC-tjänsten efter konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="2ff81-287">**Restart** hello KDC service after configuration.</span></span>

2.  <span data-ttu-id="2ff81-288">Förbereda en huvudansvarig med namnet  **krbtgt/REALM.COM@AD.COM**  i KDC-server med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="2ff81-288">Prepare a principal named **krbtgt/REALM.COM@AD.COM** in KDC server with hello following command:</span></span>

            Kadmin> addprinc krbtgt/REALM.COM@AD.COM

3.  <span data-ttu-id="2ff81-289">I **hadoop.security.auth_to_local** HDFS tjänstkonfiguration lägger du till `RULE:[1:$1@$0](.*@AD.COM)s/@.*//`.</span><span class="sxs-lookup"><span data-stu-id="2ff81-289">In **hadoop.security.auth_to_local** HDFS service configuration file, add `RULE:[1:$1@$0](.*@AD.COM)s/@.*//`.</span></span>

<span data-ttu-id="2ff81-290">**På domänkontrollanten:**</span><span class="sxs-lookup"><span data-stu-id="2ff81-290">**On domain controller:**</span></span>

1.  <span data-ttu-id="2ff81-291">Kör följande hello **Ksetup** tooadd en sfär post-kommandon:</span><span class="sxs-lookup"><span data-stu-id="2ff81-291">Run hello following **Ksetup** commands tooadd a realm entry:</span></span>

            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
            C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM

2.  <span data-ttu-id="2ff81-292">Upprätta förtroende från Windows-domän tooKerberos sfär.</span><span class="sxs-lookup"><span data-stu-id="2ff81-292">Establish trust from Windows Domain tooKerberos Realm.</span></span> <span data-ttu-id="2ff81-293">[lösenord] är hello lösenord för hello principal  **krbtgt/REALM.COM@AD.COM** .</span><span class="sxs-lookup"><span data-stu-id="2ff81-293">[password] is hello password for hello principal **krbtgt/REALM.COM@AD.COM**.</span></span>

            C:> netdom trust REALM.COM /Domain: AD.COM /add /realm /passwordt:[password]

3.  <span data-ttu-id="2ff81-294">Välj krypteringsalgoritm som används i Kerberos.</span><span class="sxs-lookup"><span data-stu-id="2ff81-294">Select encryption algorithm used in Kerberos.</span></span>

    1. <span data-ttu-id="2ff81-295">Gå tooServer Manager > hantering av Grupprincip > domän > grupprincipobjekt > standard eller aktiva domänprincip och redigera.</span><span class="sxs-lookup"><span data-stu-id="2ff81-295">Go tooServer Manager > Group Policy Management > Domain > Group Policy Objects > Default or Active Domain Policy, and Edit.</span></span>

    2. <span data-ttu-id="2ff81-296">I hello **Redigeraren för Grupprinciphantering** popup-fönster, gå tooComputer Configuration > Principer > Windows-inställningar > säkerhetsinställningar > lokala principer > säkerhetsalternativ, och konfigurera **nätverk säkerhet: Konfigurera krypteringstyper som tillåts för Kerberos**.</span><span class="sxs-lookup"><span data-stu-id="2ff81-296">In hello **Group Policy Management Editor** popup window, go tooComputer Configuration > Policies > Windows Settings > Security Settings > Local Policies > Security Options, and configure **Network security: Configure Encryption types allowed for Kerberos**.</span></span>

    3. <span data-ttu-id="2ff81-297">Välj hello krypteringsalgoritm som du vill toouse när ansluter tooKDC.</span><span class="sxs-lookup"><span data-stu-id="2ff81-297">Select hello encryption algorithm you want toouse when connect tooKDC.</span></span> <span data-ttu-id="2ff81-298">Ofta, kan du bara välja alla hello-alternativ.</span><span class="sxs-lookup"><span data-stu-id="2ff81-298">Commonly, you can simply select all hello options.</span></span>

        ![Config krypteringstyper för Kerberos](media/data-factory-hdfs-connector/config-encryption-types-for-kerberos.png)

    4. <span data-ttu-id="2ff81-300">Använd **Ksetup** kommandot toospecify hello kryptering algoritmen toobe används på hello specifika SFÄR.</span><span class="sxs-lookup"><span data-stu-id="2ff81-300">Use **Ksetup** command toospecify hello encryption algorithm toobe used on hello specific REALM.</span></span>

                C:> ksetup /SetEncTypeAttr REALM.COM DES-CBC-CRC DES-CBC-MD5 RC4-HMAC-MD5 AES128-CTS-HMAC-SHA1-96 AES256-CTS-HMAC-SHA1-96

4.  <span data-ttu-id="2ff81-301">Skapa hello mappning mellan hello domänkonto och Kerberos-huvudnamn i ordning toouse Kerberos-huvudnamn i Windows-domän.</span><span class="sxs-lookup"><span data-stu-id="2ff81-301">Create hello mapping between hello domain account and Kerberos principal, in order toouse Kerberos principal in Windows Domain.</span></span>

    1. <span data-ttu-id="2ff81-302">Starta hello Administrationsverktyg > **Active Directory-användare och datorer**.</span><span class="sxs-lookup"><span data-stu-id="2ff81-302">Start hello Administrative tools > **Active Directory Users and Computers**.</span></span>

    2. <span data-ttu-id="2ff81-303">Konfigurera avancerade funktioner genom att klicka på **visa** > **avancerade funktioner**.</span><span class="sxs-lookup"><span data-stu-id="2ff81-303">Configure advanced features by clicking **View** > **Advanced Features**.</span></span>

    3. <span data-ttu-id="2ff81-304">Leta upp hello konto toowhich du vill använda toocreate mappningar och högerklicka på tooview **namnmappningar** > klickar du på **Kerberos-namn** fliken.</span><span class="sxs-lookup"><span data-stu-id="2ff81-304">Locate hello account toowhich you want toocreate mappings, and right-click tooview **Name Mappings** > click **Kerberos Names** tab.</span></span>

    4. <span data-ttu-id="2ff81-305">Lägga till en huvudansvarig från hello sfär.</span><span class="sxs-lookup"><span data-stu-id="2ff81-305">Add a principal from hello realm.</span></span>

        ![Mappa säkerhetsidentitet](media/data-factory-hdfs-connector/map-security-identity.png)

<span data-ttu-id="2ff81-307">**På gateway-datorn:**</span><span class="sxs-lookup"><span data-stu-id="2ff81-307">**On gateway machine:**</span></span>

* <span data-ttu-id="2ff81-308">Kör följande hello **Ksetup** tooadd en sfär post-kommandon.</span><span class="sxs-lookup"><span data-stu-id="2ff81-308">Run hello following **Ksetup** commands tooadd a realm entry.</span></span>

            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
            C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM

<span data-ttu-id="2ff81-309">**I Azure Data Factory:**</span><span class="sxs-lookup"><span data-stu-id="2ff81-309">**In Azure Data Factory:**</span></span>

* <span data-ttu-id="2ff81-310">Konfigurera hello HDFS en koppling med hjälp av **Windows-autentisering** tillsammans med din domänkonto eller Kerberos-huvudnamn tooconnect toohello HDFS-datakälla.</span><span class="sxs-lookup"><span data-stu-id="2ff81-310">Configure hello HDFS connector using **Windows authentication** together with either your Domain Account or Kerberos Principal tooconnect toohello HDFS data source.</span></span> <span data-ttu-id="2ff81-311">Kontrollera [HDFS länkade tjänstegenskaper](#linked-service-properties) avsnittet konfigurationsinformation.</span><span class="sxs-lookup"><span data-stu-id="2ff81-311">Check [HDFS Linked Service properties](#linked-service-properties) section on configuration details.</span></span>

> [!NOTE]
> <span data-ttu-id="2ff81-312">toomap kolumner från källan dataset toocolumns från sink dataset finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="2ff81-312">toomap columns from source dataset toocolumns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>


## <a name="performance-and-tuning"></a><span data-ttu-id="2ff81-313">Prestanda och finjustering</span><span class="sxs-lookup"><span data-stu-id="2ff81-313">Performance and Tuning</span></span>
<span data-ttu-id="2ff81-314">Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) toolearn om nyckeln faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt toooptimize den.</span><span class="sxs-lookup"><span data-stu-id="2ff81-314">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
