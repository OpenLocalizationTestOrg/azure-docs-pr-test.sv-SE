---
title: "aaaCopy data till/från ett filsystem med Azure Data Factory | Microsoft Docs"
description: "Lär dig hur toocopy data tooand från ett lokalt filsystem med hjälp av Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: ce19f1ae-358e-4ffc-8a80-d802505c9c84
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: jingwang
ms.openlocfilehash: 201b8bc3ffa639df781443aa0c3f95c975d280be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="copy-data-tooand-from-an-on-premises-file-system-by-using-azure-data-factory"></a><span data-ttu-id="5338d-103">Kopiera data tooand från ett lokalt filsystem med hjälp av Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="5338d-103">Copy data tooand from an on-premises file system by using Azure Data Factory</span></span>
<span data-ttu-id="5338d-104">Den här artikeln förklarar hur toouse hello Kopieringsaktiviteten i Azure Data Factory toocopy data till och från ett lokalt filsystem.</span><span class="sxs-lookup"><span data-stu-id="5338d-104">This article explains how toouse hello Copy Activity in Azure Data Factory toocopy data to/from an on-premises file system.</span></span> <span data-ttu-id="5338d-105">Den bygger på hello [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som ger en allmän översikt över dataflyttning hello kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="5338d-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>

## <a name="supported-scenarios"></a><span data-ttu-id="5338d-106">Scenarier som stöds</span><span class="sxs-lookup"><span data-stu-id="5338d-106">Supported scenarios</span></span>
<span data-ttu-id="5338d-107">Du kan kopiera data **från ett lokalt filsystem** toohello följande datakällor:</span><span class="sxs-lookup"><span data-stu-id="5338d-107">You can copy data **from an on-premises file system** toohello following data stores:</span></span>

[!INCLUDE [data-factory-supported-sink](../../includes/data-factory-supported-sinks.md)]

<span data-ttu-id="5338d-108">Du kan kopiera data från hello följande datalager **tooan lokalt filsystem**:</span><span class="sxs-lookup"><span data-stu-id="5338d-108">You can copy data from hello following data stores **tooan on-premises file system**:</span></span>

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

> [!NOTE]
> <span data-ttu-id="5338d-109">Kopieringsaktiviteten tar inte bort hello källfilen när den inte har kopierade toohello mål.</span><span class="sxs-lookup"><span data-stu-id="5338d-109">Copy Activity does not delete hello source file after it is successfully copied toohello destination.</span></span> <span data-ttu-id="5338d-110">Om du behöver toodelete hello källfilen efter en lyckad kopiering, skapa en anpassad aktivitet toodelete hello-fil och använda hello aktivitet i hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="5338d-110">If you need toodelete hello source file after a successful copy, create a custom activity toodelete hello file and use hello activity in hello pipeline.</span></span> 

## <a name="enabling-connectivity"></a><span data-ttu-id="5338d-111">Aktivera anslutning</span><span class="sxs-lookup"><span data-stu-id="5338d-111">Enabling connectivity</span></span>
<span data-ttu-id="5338d-112">Data Factory stöder anslutande tooand från ett lokalt filsystem via **Data Management Gateway**.</span><span class="sxs-lookup"><span data-stu-id="5338d-112">Data Factory supports connecting tooand from an on-premises file system via **Data Management Gateway**.</span></span> <span data-ttu-id="5338d-113">Du måste installera hello Data Management Gateway i din lokala miljö för hello Data Factory-tjänsten tooconnect tooany stöds lokalt datalager inklusive filsystemet.</span><span class="sxs-lookup"><span data-stu-id="5338d-113">You must install hello Data Management Gateway in your on-premises environment for hello Data Factory service tooconnect tooany supported on-premises data store including file system.</span></span> <span data-ttu-id="5338d-114">toolearn om Data Management Gateway och stegvisa instruktioner om hur du konfigurerar hello gateway finns [flytta data mellan lokala källor och hello moln med Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md).</span><span class="sxs-lookup"><span data-stu-id="5338d-114">toolearn about Data Management Gateway and for step-by-step instructions on setting up hello gateway, see [Move data between on-premises sources and hello cloud with Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md).</span></span> <span data-ttu-id="5338d-115">Förutom Data Management Gateway måste inte några binära filer toobe installerat toocommunicate tooand från ett lokalt filsystem.</span><span class="sxs-lookup"><span data-stu-id="5338d-115">Apart from Data Management Gateway, no other binary files need toobe installed toocommunicate tooand from an on-premises file system.</span></span> <span data-ttu-id="5338d-116">Du måste installera och använda hello Data Management Gateway, även om hello filsystemet är i Azure IaaS-VM.</span><span class="sxs-lookup"><span data-stu-id="5338d-116">You must install and use hello Data Management Gateway even if hello file system is in Azure IaaS VM.</span></span> <span data-ttu-id="5338d-117">Detaljerad information om hello gateway finns [Data Management Gateway](data-factory-data-management-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="5338d-117">For detailed information about hello gateway, see [Data Management Gateway](data-factory-data-management-gateway.md).</span></span>

<span data-ttu-id="5338d-118">toouse en Linux-filresurs, installera [Samba](https://www.samba.org/) på Linux-servern och installera Data Management Gateway på en Windows server.</span><span class="sxs-lookup"><span data-stu-id="5338d-118">toouse a Linux file share, install [Samba](https://www.samba.org/) on your Linux server, and install Data Management Gateway on a Windows server.</span></span> <span data-ttu-id="5338d-119">Det går inte att installera Data Management Gateway på en Linux-server.</span><span class="sxs-lookup"><span data-stu-id="5338d-119">Installing Data Management Gateway on a Linux server is not supported.</span></span>

## <a name="getting-started"></a><span data-ttu-id="5338d-120">Komma igång</span><span class="sxs-lookup"><span data-stu-id="5338d-120">Getting started</span></span>
<span data-ttu-id="5338d-121">Du kan skapa en pipeline med en kopia-aktivitet som flyttar data till/från ett filsystem med hjälp av olika verktyg/API: er.</span><span class="sxs-lookup"><span data-stu-id="5338d-121">You can create a pipeline with a copy activity that moves data to/from a file system by using different tools/APIs.</span></span>

<span data-ttu-id="5338d-122">hello enklaste sättet toocreate en pipeline är toouse hello **guiden Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="5338d-122">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="5338d-123">Se [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden för hello kopiera data.</span><span class="sxs-lookup"><span data-stu-id="5338d-123">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span>

<span data-ttu-id="5338d-124">Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall** , **.NET API**, och **REST API**.</span><span class="sxs-lookup"><span data-stu-id="5338d-124">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="5338d-125">Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="5338d-125">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span>

<span data-ttu-id="5338d-126">Om du använder hello verktyg eller API: er kan utföra du hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalagret:</span><span class="sxs-lookup"><span data-stu-id="5338d-126">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="5338d-127">Skapa en **datafabriken**.</span><span class="sxs-lookup"><span data-stu-id="5338d-127">Create a **data factory**.</span></span> <span data-ttu-id="5338d-128">En datafabrik kan innehålla en eller flera pipelines.</span><span class="sxs-lookup"><span data-stu-id="5338d-128">A data factory may contain one or more pipelines.</span></span> 
2. <span data-ttu-id="5338d-129">Skapa **länkade tjänster** toolink indata och utdata lagrar tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="5338d-129">Create **linked services** toolink input and output data stores tooyour data factory.</span></span> <span data-ttu-id="5338d-130">Till exempel om du kopierar data från ett Azure blob storage tooan lokalt filsystem skapa du två länkade tjänster toolink lokala filsystemet och Azure storage-konto tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="5338d-130">For example, if you are copying data from an Azure blob storage tooan on-premises file system, you create two linked services toolink your on-premises file system and Azure storage account tooyour data factory.</span></span> <span data-ttu-id="5338d-131">Länkad tjänstegenskaper som är specifika tooan lokalt filsystem, se [länkade tjänstegenskaper](#linked-service-properties) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="5338d-131">For linked service properties that are specific tooan on-premises file system, see [linked service properties](#linked-service-properties) section.</span></span>
3. <span data-ttu-id="5338d-132">Skapa **datauppsättningar** toorepresent indata och utdata för hello kopieringsåtgärden.</span><span class="sxs-lookup"><span data-stu-id="5338d-132">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> <span data-ttu-id="5338d-133">I hello exempelvis nämns i hello sista steget skapar du en dataset toospecify hello blob-behållaren och mappen som innehåller hello indata.</span><span class="sxs-lookup"><span data-stu-id="5338d-133">In hello example mentioned in hello last step, you create a dataset toospecify hello blob container and folder that contains hello input data.</span></span> <span data-ttu-id="5338d-134">Och du skapar en annan dataset toospecify hello mapp och fil namn (valfritt) i filsystemet.</span><span class="sxs-lookup"><span data-stu-id="5338d-134">And, you create another dataset toospecify hello folder and file name (optional) in your file system.</span></span> <span data-ttu-id="5338d-135">Egenskaper för datamängd som är specifika tooon lokala filsystem, finns [egenskaper för datamängd](#dataset-properties) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="5338d-135">For dataset properties that are specific tooon-premises file system, see [dataset properties](#dataset-properties) section.</span></span>
4. <span data-ttu-id="5338d-136">Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata.</span><span class="sxs-lookup"><span data-stu-id="5338d-136">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> <span data-ttu-id="5338d-137">I hello-exemplet ovan, använder du BlobSource som en källa och FileSystemSink som en mottagare för hello kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="5338d-137">In hello example mentioned earlier, you use BlobSource as a source and FileSystemSink as a sink for hello copy activity.</span></span> <span data-ttu-id="5338d-138">På samma sätt om du vill kopiera från lokalt Arkiv system tooAzure Blob Storage, använder du FileSystemSource och BlobSink i hello kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="5338d-138">Similarly, if you are copying from on-premises file system tooAzure Blob Storage, you use FileSystemSource and BlobSink in hello copy activity.</span></span> <span data-ttu-id="5338d-139">Kopiera Aktivitetsegenskaper som är specifika tooon lokala filsystem, finns [kopiera Aktivitetsegenskaper](#copy-activity-properties) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="5338d-139">For copy activity properties that are specific tooon-premises file system, see [copy activity properties](#copy-activity-properties) section.</span></span> <span data-ttu-id="5338d-140">Mer information om hur toouse en databas som en källa eller en mottagare klickar du på hello länk under föregående hello för datalager.</span><span class="sxs-lookup"><span data-stu-id="5338d-140">For details on how toouse a data store as a source or a sink, click hello link in hello previous section for your data store.</span></span>

<span data-ttu-id="5338d-141">När du använder guiden hello skapas automatiskt JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och hello pipeline) för dig.</span><span class="sxs-lookup"><span data-stu-id="5338d-141">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="5338d-142">När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av hello JSON-format.</span><span class="sxs-lookup"><span data-stu-id="5338d-142">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="5338d-143">Exempel med JSON-definitioner för Data Factory-entiteter som har använt toocopy data till/från ett filsystem finns [JSON-exempel](#json-examples-for-copying-data-to-and-from-file-system) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="5338d-143">For samples with JSON definitions for Data Factory entities that are used toocopy data to/from a file system, see [JSON examples](#json-examples-for-copying-data-to-and-from-file-system) section of this article.</span></span>

<span data-ttu-id="5338d-144">hello följande avsnitt innehåller information om JSON-egenskaper används toodefine Data Factory entiteter specifika toofile system:</span><span class="sxs-lookup"><span data-stu-id="5338d-144">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific toofile system:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="5338d-145">Länkad tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="5338d-145">Linked service properties</span></span>
<span data-ttu-id="5338d-146">Du kan länka en lokal fil system tooan Azure data factory med hello **lokala filserver** länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="5338d-146">You can link an on-premises file system tooan Azure data factory with hello **On-Premises File Server** linked service.</span></span> <span data-ttu-id="5338d-147">hello i den följande tabellen finns beskrivningar JSON-element som är specifika toohello lokala filserver länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="5338d-147">hello following table provides descriptions for JSON elements that are specific toohello On-Premises File Server linked service.</span></span>

| <span data-ttu-id="5338d-148">Egenskap</span><span class="sxs-lookup"><span data-stu-id="5338d-148">Property</span></span> | <span data-ttu-id="5338d-149">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="5338d-149">Description</span></span> | <span data-ttu-id="5338d-150">Krävs</span><span class="sxs-lookup"><span data-stu-id="5338d-150">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5338d-151">typ</span><span class="sxs-lookup"><span data-stu-id="5338d-151">type</span></span> |<span data-ttu-id="5338d-152">Kontrollera att hello type-egenskap är inställd för**OnPremisesFileServer**.</span><span class="sxs-lookup"><span data-stu-id="5338d-152">Ensure that hello type property is set too**OnPremisesFileServer**.</span></span> |<span data-ttu-id="5338d-153">Ja</span><span class="sxs-lookup"><span data-stu-id="5338d-153">Yes</span></span> |
| <span data-ttu-id="5338d-154">värden</span><span class="sxs-lookup"><span data-stu-id="5338d-154">host</span></span> |<span data-ttu-id="5338d-155">Anger hello rotsökvägen för hello mappen som du vill toocopy.</span><span class="sxs-lookup"><span data-stu-id="5338d-155">Specifies hello root path of hello folder that you want toocopy.</span></span> <span data-ttu-id="5338d-156">Använd hello escape-tecknet ' \ ' för specialtecken i hello-sträng.</span><span class="sxs-lookup"><span data-stu-id="5338d-156">Use hello escape character ‘ \ ’ for special characters in hello string.</span></span> <span data-ttu-id="5338d-157">Se [exempel länkad tjänst-och dataset](#sample-linked-service-and-dataset-definitions) exempel.</span><span class="sxs-lookup"><span data-stu-id="5338d-157">See [Sample linked service and dataset definitions](#sample-linked-service-and-dataset-definitions) for examples.</span></span> |<span data-ttu-id="5338d-158">Ja</span><span class="sxs-lookup"><span data-stu-id="5338d-158">Yes</span></span> |
| <span data-ttu-id="5338d-159">användar-ID</span><span class="sxs-lookup"><span data-stu-id="5338d-159">userid</span></span> |<span data-ttu-id="5338d-160">Ange hello-ID för hello-användare som har åtkomst till toohello servern.</span><span class="sxs-lookup"><span data-stu-id="5338d-160">Specify hello ID of hello user who has access toohello server.</span></span> |<span data-ttu-id="5338d-161">Nej (om du väljer encryptedCredential)</span><span class="sxs-lookup"><span data-stu-id="5338d-161">No (if you choose encryptedCredential)</span></span> |
| <span data-ttu-id="5338d-162">lösenord</span><span class="sxs-lookup"><span data-stu-id="5338d-162">password</span></span> |<span data-ttu-id="5338d-163">Ange hello användarlösenord hello (användar-ID).</span><span class="sxs-lookup"><span data-stu-id="5338d-163">Specify hello password for hello user (userid).</span></span> |<span data-ttu-id="5338d-164">Nej (om du väljer encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="5338d-164">No (if you choose encryptedCredential</span></span> |
| <span data-ttu-id="5338d-165">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="5338d-165">encryptedCredential</span></span> |<span data-ttu-id="5338d-166">Ange hello krypterade autentiseringsuppgifter som du kan få genom att köra hello ny AzureRmDataFactoryEncryptValue cmdlet.</span><span class="sxs-lookup"><span data-stu-id="5338d-166">Specify hello encrypted credentials that you can get by running hello New-AzureRmDataFactoryEncryptValue cmdlet.</span></span> |<span data-ttu-id="5338d-167">Nej (om du väljer toospecify användar-ID och lösenord i klartext)</span><span class="sxs-lookup"><span data-stu-id="5338d-167">No (if you choose toospecify userid and password in plain text)</span></span> |
| <span data-ttu-id="5338d-168">gatewayName</span><span class="sxs-lookup"><span data-stu-id="5338d-168">gatewayName</span></span> |<span data-ttu-id="5338d-169">Anger hello namnet på hello gateway som Data Factory ska använda tooconnect toohello lokal server.</span><span class="sxs-lookup"><span data-stu-id="5338d-169">Specifies hello name of hello gateway that Data Factory should use tooconnect toohello on-premises file server.</span></span> |<span data-ttu-id="5338d-170">Ja</span><span class="sxs-lookup"><span data-stu-id="5338d-170">Yes</span></span> |


### <a name="sample-linked-service-and-dataset-definitions"></a><span data-ttu-id="5338d-171">Exempel på länkad tjänst och dataset definitioner</span><span class="sxs-lookup"><span data-stu-id="5338d-171">Sample linked service and dataset definitions</span></span>
| <span data-ttu-id="5338d-172">Scenario</span><span class="sxs-lookup"><span data-stu-id="5338d-172">Scenario</span></span> | <span data-ttu-id="5338d-173">Värden i länkade tjänstdefinitionen</span><span class="sxs-lookup"><span data-stu-id="5338d-173">Host in linked service definition</span></span> | <span data-ttu-id="5338d-174">folderPath i datauppsättningsdefinitionen</span><span class="sxs-lookup"><span data-stu-id="5338d-174">folderPath in dataset definition</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5338d-175">Lokal mapp på Data Management Gateway-datorn:</span><span class="sxs-lookup"><span data-stu-id="5338d-175">Local folder on Data Management Gateway machine:</span></span> <br/><br/><span data-ttu-id="5338d-176">Exempel: D:\\ \* eller D:\folder\subfolder\\*</span><span class="sxs-lookup"><span data-stu-id="5338d-176">Examples: D:\\\* or D:\folder\subfolder\\*</span></span> |<span data-ttu-id="5338d-177">D:\\ \\ (för Data Management Gateway 2.0 och senare)</span><span class="sxs-lookup"><span data-stu-id="5338d-177">D:\\\\ (for Data Management Gateway 2.0 and later versions)</span></span> <br/><br/> <span data-ttu-id="5338d-178">localhost (för tidigare versioner än Data Management Gateway 2.0)</span><span class="sxs-lookup"><span data-stu-id="5338d-178">localhost (for earlier versions than Data Management Gateway 2.0)</span></span> |<span data-ttu-id="5338d-179">. \\ \\ eller mappen\\\\undermapp (för Data Management Gateway 2.0 och senare)</span><span class="sxs-lookup"><span data-stu-id="5338d-179">.\\\\ or folder\\\\subfolder (for Data Management Gateway 2.0 and later versions)</span></span> <br/><br/><span data-ttu-id="5338d-180">D:\\ \\ eller D:\\\\mappen\\\\undermapp (för gateway-versionen nedan 2.0)</span><span class="sxs-lookup"><span data-stu-id="5338d-180">D:\\\\ or D:\\\\folder\\\\subfolder (for gateway version below 2.0)</span></span> |
| <span data-ttu-id="5338d-181">Delad fjärrmapp:</span><span class="sxs-lookup"><span data-stu-id="5338d-181">Remote shared folder:</span></span> <br/><br/><span data-ttu-id="5338d-182">Exempel: \\ \\minserver\\dela\\ \* eller \\ \\minserver\\dela\\mappen\\undermapp\\*</span><span class="sxs-lookup"><span data-stu-id="5338d-182">Examples: \\\\myserver\\share\\\* or \\\\myserver\\share\\folder\\subfolder\\*</span></span> |<span data-ttu-id="5338d-183">\\\\\\\\minserver\\\\dela</span><span class="sxs-lookup"><span data-stu-id="5338d-183">\\\\\\\\myserver\\\\share</span></span> |<span data-ttu-id="5338d-184">. \\ \\ eller mappen\\\\undermapp</span><span class="sxs-lookup"><span data-stu-id="5338d-184">.\\\\ or folder\\\\subfolder</span></span> |


### <a name="example-using-username-and-password-in-plain-text"></a><span data-ttu-id="5338d-185">Exempel: Med användarnamn och lösenord i klartext</span><span class="sxs-lookup"><span data-stu-id="5338d-185">Example: Using username and password in plain text</span></span>

```JSON
{
  "Name": "OnPremisesFileServerLinkedService",
  "properties": {
    "type": "OnPremisesFileServer",
    "typeProperties": {
      "host": "\\\\Contosogame-Asia",
      "userid": "Admin",
      "password": "123456",
      "gatewayName": "mygateway"
    }
  }
}
```

### <a name="example-using-encryptedcredential"></a><span data-ttu-id="5338d-186">Exempel: Använda encryptedcredential</span><span class="sxs-lookup"><span data-stu-id="5338d-186">Example: Using encryptedcredential</span></span>

```JSON
{
  "Name": " OnPremisesFileServerLinkedService ",
  "properties": {
    "type": "OnPremisesFileServer",
    "typeProperties": {
      "host": "D:\\",
      "encryptedCredential": "WFuIGlzIGRpc3Rpbmd1aXNoZWQsIG5vdCBvbmx5IGJ5xxxxxxxxxxxxxxxxx",
      "gatewayName": "mygateway"
    }
  }
}
```

## <a name="dataset-properties"></a><span data-ttu-id="5338d-187">Egenskaper för datamängd</span><span class="sxs-lookup"><span data-stu-id="5338d-187">Dataset properties</span></span>
<span data-ttu-id="5338d-188">En fullständig lista över avsnitt och egenskaper som är tillgängliga för att definiera datauppsättningar finns [skapa datauppsättningar](data-factory-create-datasets.md).</span><span class="sxs-lookup"><span data-stu-id="5338d-188">For a full list of sections and properties that are available for defining datasets, see [Creating datasets](data-factory-create-datasets.md).</span></span> <span data-ttu-id="5338d-189">Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av datauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="5338d-189">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types.</span></span>

<span data-ttu-id="5338d-190">Hej typeProperties avsnittet är olika för varje typ av datauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="5338d-190">hello typeProperties section is different for each type of dataset.</span></span> <span data-ttu-id="5338d-191">Den ger information, till exempel hello plats och dataformat hello i hello-datalagret.</span><span class="sxs-lookup"><span data-stu-id="5338d-191">It provides information such as hello location and format of hello data in hello data store.</span></span> <span data-ttu-id="5338d-192">Hej typeProperties avsnittet för hello dataset av typen **filresursen** har hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="5338d-192">hello typeProperties section for hello dataset of type **FileShare** has hello following properties:</span></span>

| <span data-ttu-id="5338d-193">Egenskap</span><span class="sxs-lookup"><span data-stu-id="5338d-193">Property</span></span> | <span data-ttu-id="5338d-194">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="5338d-194">Description</span></span> | <span data-ttu-id="5338d-195">Krävs</span><span class="sxs-lookup"><span data-stu-id="5338d-195">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5338d-196">folderPath</span><span class="sxs-lookup"><span data-stu-id="5338d-196">folderPath</span></span> |<span data-ttu-id="5338d-197">Anger hello undersökvägen toohello mapp.</span><span class="sxs-lookup"><span data-stu-id="5338d-197">Specifies hello subpath toohello folder.</span></span> <span data-ttu-id="5338d-198">Använd hello escape-tecknet ' \' för specialtecken i hello-sträng.</span><span class="sxs-lookup"><span data-stu-id="5338d-198">Use hello escape character ‘\’ for special characters in hello string.</span></span> <span data-ttu-id="5338d-199">Se [exempel länkad tjänst-och dataset](#sample-linked-service-and-dataset-definitions) exempel.</span><span class="sxs-lookup"><span data-stu-id="5338d-199">See [Sample linked service and dataset definitions](#sample-linked-service-and-dataset-definitions) for examples.</span></span><br/><br/><span data-ttu-id="5338d-200">Du kan kombinera den här egenskapen med **partitionBy** toohave mappsökvägar baserat på sektorn börja/sluta datum gånger.</span><span class="sxs-lookup"><span data-stu-id="5338d-200">You can combine this property with **partitionBy** toohave folder paths based on slice start/end date-times.</span></span> |<span data-ttu-id="5338d-201">Ja</span><span class="sxs-lookup"><span data-stu-id="5338d-201">Yes</span></span> |
| <span data-ttu-id="5338d-202">fileName</span><span class="sxs-lookup"><span data-stu-id="5338d-202">fileName</span></span> |<span data-ttu-id="5338d-203">Ange hello namnet på hello-filen i hello **folderPath** om du vill hello tabell toorefer tooa specifika filen i mappen hello.</span><span class="sxs-lookup"><span data-stu-id="5338d-203">Specify hello name of hello file in hello **folderPath** if you want hello table toorefer tooa specific file in hello folder.</span></span> <span data-ttu-id="5338d-204">Om du inte anger något värde för den här egenskapen pekar hello tabell tooall filer i hello-mappen.</span><span class="sxs-lookup"><span data-stu-id="5338d-204">If you do not specify any value for this property, hello table points tooall files in hello folder.</span></span><br/><br/><span data-ttu-id="5338d-205">När **fileName** har inte angetts för en datamängd för utdata och **preserveHierarchy** har inte angetts i aktiviteten sink hello hello genereras filen heter i hello följande format:</span><span class="sxs-lookup"><span data-stu-id="5338d-205">When **fileName** is not specified for an output dataset and **preserveHierarchy** is not specified in activity sink, hello name of hello generated file is in hello following format:</span></span> <br/><br/><span data-ttu-id="5338d-206">`Data.<Guid>.txt`(Exempel: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt)</span><span class="sxs-lookup"><span data-stu-id="5338d-206">`Data.<Guid>.txt` (Example: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt)</span></span> |<span data-ttu-id="5338d-207">Nej</span><span class="sxs-lookup"><span data-stu-id="5338d-207">No</span></span> |
| <span data-ttu-id="5338d-208">fileFilter</span><span class="sxs-lookup"><span data-stu-id="5338d-208">fileFilter</span></span> |<span data-ttu-id="5338d-209">Ange ett filter toobe används tooselect en delmängd av filer i hello folderPath i stället för alla filer.</span><span class="sxs-lookup"><span data-stu-id="5338d-209">Specify a filter toobe used tooselect a subset of files in hello folderPath rather than all files.</span></span> <br/><br/><span data-ttu-id="5338d-210">Tillåtna värden är: `*` (flera tecken) och `?` (valfritt tecken).</span><span class="sxs-lookup"><span data-stu-id="5338d-210">Allowed values are: `*` (multiple characters) and `?` (single character).</span></span><br/><br/><span data-ttu-id="5338d-211">Exempel 1: ”fileFilter” ”: * .log”</span><span class="sxs-lookup"><span data-stu-id="5338d-211">Example 1: "fileFilter": "*.log"</span></span><br/><span data-ttu-id="5338d-212">Exempel 2: ”fileFilter”: 2014 - 1-?. txt ”</span><span class="sxs-lookup"><span data-stu-id="5338d-212">Example 2: "fileFilter": 2014-1-?.txt"</span></span><br/><br/><span data-ttu-id="5338d-213">Observera att fileFilter gäller för en inkommande filresursen datauppsättning.</span><span class="sxs-lookup"><span data-stu-id="5338d-213">Note that fileFilter is applicable for an input FileShare dataset.</span></span> |<span data-ttu-id="5338d-214">Nej</span><span class="sxs-lookup"><span data-stu-id="5338d-214">No</span></span> |
| <span data-ttu-id="5338d-215">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="5338d-215">partitionedBy</span></span> |<span data-ttu-id="5338d-216">Du kan använda partitionedBy toospecify dynamiska folderPath/filnamn för time series-data.</span><span class="sxs-lookup"><span data-stu-id="5338d-216">You can use partitionedBy toospecify a dynamic folderPath/fileName for time-series data.</span></span> <span data-ttu-id="5338d-217">Ett exempel är folderPath parametriserade varje timme av data.</span><span class="sxs-lookup"><span data-stu-id="5338d-217">An example is folderPath parameterized for every hour of data.</span></span> |<span data-ttu-id="5338d-218">Nej</span><span class="sxs-lookup"><span data-stu-id="5338d-218">No</span></span> |
| <span data-ttu-id="5338d-219">Format</span><span class="sxs-lookup"><span data-stu-id="5338d-219">format</span></span> | <span data-ttu-id="5338d-220">hello följande formattyper stöds: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="5338d-220">hello following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="5338d-221">Ange hello **typen** egenskap under format tooone av dessa värden.</span><span class="sxs-lookup"><span data-stu-id="5338d-221">Set hello **type** property under format tooone of these values.</span></span> <span data-ttu-id="5338d-222">Mer information finns i [textformat](data-factory-supported-file-and-compression-formats.md#text-format), [Json-Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-formatet](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), och [parkettgolv Format](data-factory-supported-file-and-compression-formats.md#parquet-format) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="5338d-222">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="5338d-223">Om du vill använda för**kopiera filer som-är** mellan filbaserade butiker (binär kopia), hoppar du över hello format-avsnittet i både inkommande och utgående dataset-definitioner.</span><span class="sxs-lookup"><span data-stu-id="5338d-223">If you want too**copy files as-is** between file-based stores (binary copy), skip hello format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="5338d-224">Nej</span><span class="sxs-lookup"><span data-stu-id="5338d-224">No</span></span> |
| <span data-ttu-id="5338d-225">Komprimering</span><span class="sxs-lookup"><span data-stu-id="5338d-225">compression</span></span> | <span data-ttu-id="5338d-226">Ange hello typ och kompression för hello data.</span><span class="sxs-lookup"><span data-stu-id="5338d-226">Specify hello type and level of compression for hello data.</span></span> <span data-ttu-id="5338d-227">Typer som stöds är: **GZip**, **Deflate**, **BZip2**, och **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="5338d-227">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="5338d-228">Nivåer som stöds är: **Optimal** och **snabbast**.</span><span class="sxs-lookup"><span data-stu-id="5338d-228">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="5338d-229">Se [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="5338d-229">see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="5338d-230">Nej</span><span class="sxs-lookup"><span data-stu-id="5338d-230">No</span></span> |

> [!NOTE]
> <span data-ttu-id="5338d-231">Du kan inte använda filnamn och fileFilter samtidigt.</span><span class="sxs-lookup"><span data-stu-id="5338d-231">You cannot use fileName and fileFilter simultaneously.</span></span>

### <a name="using-partitionedby-property"></a><span data-ttu-id="5338d-232">Med hjälp av partitionedBy egenskap</span><span class="sxs-lookup"><span data-stu-id="5338d-232">Using partitionedBy property</span></span>
<span data-ttu-id="5338d-233">Som nämnts tidigare under hello, du kan ange en dynamisk folderPath och ett filnamn för tid series-data med hello **partitionedBy** egenskapen [Data Factory-funktioner och hello systemvariabler](data-factory-functions-variables.md).</span><span class="sxs-lookup"><span data-stu-id="5338d-233">As mentioned in hello previous section, you can specify a dynamic folderPath and filename for time series data with hello **partitionedBy** property, [Data Factory functions, and hello system variables](data-factory-functions-variables.md).</span></span>

<span data-ttu-id="5338d-234">toounderstand mer information om tidsserier datauppsättningar, schemaläggning och segment, se [skapa datauppsättningar](data-factory-create-datasets.md), [schemaläggning och körning](data-factory-scheduling-and-execution.md), och [skapar pipelines](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="5338d-234">toounderstand more details on time-series datasets, scheduling, and slices, see [Creating datasets](data-factory-create-datasets.md), [Scheduling and execution](data-factory-scheduling-and-execution.md), and [Creating pipelines](data-factory-create-pipelines.md).</span></span>

#### <a name="sample-1"></a><span data-ttu-id="5338d-235">Exempel 1:</span><span class="sxs-lookup"><span data-stu-id="5338d-235">Sample 1:</span></span>

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```

<span data-ttu-id="5338d-236">I det här exemplet {segment} ersätts med värdet hello hello Data Factory systemvariabeln SliceStart hello-format (YYYYMMDDHH).</span><span class="sxs-lookup"><span data-stu-id="5338d-236">In this example, {Slice} is replaced with hello value of hello Data Factory system variable SliceStart in hello format (YYYYMMDDHH).</span></span> <span data-ttu-id="5338d-237">SliceStart refererar toostart tiden för hello sektorn.</span><span class="sxs-lookup"><span data-stu-id="5338d-237">SliceStart refers toostart time of hello slice.</span></span> <span data-ttu-id="5338d-238">hello folderPath är olika för varje segment.</span><span class="sxs-lookup"><span data-stu-id="5338d-238">hello folderPath is different for each slice.</span></span> <span data-ttu-id="5338d-239">Till exempel: 2014100103-wikidatagateway/wikisampledataout eller 2014100104-wikidatagateway/wikisampledataout.</span><span class="sxs-lookup"><span data-stu-id="5338d-239">For example: wikidatagateway/wikisampledataout/2014100103 or wikidatagateway/wikisampledataout/2014100104.</span></span>

#### <a name="sample-2"></a><span data-ttu-id="5338d-240">Exempel 2:</span><span class="sxs-lookup"><span data-stu-id="5338d-240">Sample 2:</span></span>

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

<span data-ttu-id="5338d-241">I det här exemplet extraheras år, månad, dag och tid för SliceStart i separata variabler med hello folderPath och filnamnet egenskaper.</span><span class="sxs-lookup"><span data-stu-id="5338d-241">In this example, year, month, day, and time of SliceStart are extracted into separate variables that hello folderPath and fileName properties use.</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="5338d-242">Kopiera egenskaper för aktivitet</span><span class="sxs-lookup"><span data-stu-id="5338d-242">Copy activity properties</span></span>
<span data-ttu-id="5338d-243">En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns hello [skapar Pipelines](data-factory-create-pipelines.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="5338d-243">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="5338d-244">Egenskaper som namn, beskrivning, indata och utdata-datauppsättningar och -principer är tillgängliga för alla typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="5338d-244">Properties such as name, description, input and output datasets, and policies are available for all types of activities.</span></span> <span data-ttu-id="5338d-245">Medan egenskaper som är tillgängliga i hello **typeProperties** avsnittet hello aktivitet varierar med varje aktivitetstyp.</span><span class="sxs-lookup"><span data-stu-id="5338d-245">Whereas, properties available in hello **typeProperties** section of hello activity vary with each activity type.</span></span>

<span data-ttu-id="5338d-246">För Kopieringsaktivitet kan varierar de beroende på hello typer av datakällor och sänkor.</span><span class="sxs-lookup"><span data-stu-id="5338d-246">For Copy activity, they vary depending on hello types of sources and sinks.</span></span> <span data-ttu-id="5338d-247">Om du flyttar data från ett lokalt filsystem du ställa in hello källtypen i hello kopieringsaktiviteten också**FileSystemSource**.</span><span class="sxs-lookup"><span data-stu-id="5338d-247">If you are moving data from an on-premises file system, you set hello source type in hello copy activity too**FileSystemSource**.</span></span> <span data-ttu-id="5338d-248">På samma sätt om du flyttar data tooan lokalt filsystem, du anger hello Mottagartypen i hello kopieringsaktiviteten för**FileSystemSink**.</span><span class="sxs-lookup"><span data-stu-id="5338d-248">Similarly, if you are moving data tooan on-premises file system, you set hello sink type in hello copy activity too**FileSystemSink**.</span></span> <span data-ttu-id="5338d-249">Det här avsnittet innehåller en lista över egenskaper som stöds av FileSystemSource och FileSystemSink.</span><span class="sxs-lookup"><span data-stu-id="5338d-249">This section provides a list of properties supported by FileSystemSource and FileSystemSink.</span></span>

<span data-ttu-id="5338d-250">**FileSystemSource** stöder hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="5338d-250">**FileSystemSource** supports hello following properties:</span></span>

| <span data-ttu-id="5338d-251">Egenskap</span><span class="sxs-lookup"><span data-stu-id="5338d-251">Property</span></span> | <span data-ttu-id="5338d-252">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="5338d-252">Description</span></span> | <span data-ttu-id="5338d-253">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="5338d-253">Allowed values</span></span> | <span data-ttu-id="5338d-254">Krävs</span><span class="sxs-lookup"><span data-stu-id="5338d-254">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5338d-255">Rekursiva</span><span class="sxs-lookup"><span data-stu-id="5338d-255">recursive</span></span> |<span data-ttu-id="5338d-256">Anger om hello data läses rekursivt från hello undermappar eller endast hello angivna mappen.</span><span class="sxs-lookup"><span data-stu-id="5338d-256">Indicates whether hello data is read recursively from hello subfolders or only from hello specified folder.</span></span> |<span data-ttu-id="5338d-257">SANT, FALSKT (standard)</span><span class="sxs-lookup"><span data-stu-id="5338d-257">True, False (default)</span></span> |<span data-ttu-id="5338d-258">Nej</span><span class="sxs-lookup"><span data-stu-id="5338d-258">No</span></span> |

<span data-ttu-id="5338d-259">**FileSystemSink** stöder hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="5338d-259">**FileSystemSink** supports hello following properties:</span></span>

| <span data-ttu-id="5338d-260">Egenskap</span><span class="sxs-lookup"><span data-stu-id="5338d-260">Property</span></span> | <span data-ttu-id="5338d-261">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="5338d-261">Description</span></span> | <span data-ttu-id="5338d-262">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="5338d-262">Allowed values</span></span> | <span data-ttu-id="5338d-263">Krävs</span><span class="sxs-lookup"><span data-stu-id="5338d-263">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5338d-264">copyBehavior</span><span class="sxs-lookup"><span data-stu-id="5338d-264">copyBehavior</span></span> |<span data-ttu-id="5338d-265">Definierar hello kopiera beteende när hello källa är BlobSource eller filsystem.</span><span class="sxs-lookup"><span data-stu-id="5338d-265">Defines hello copy behavior when hello source is BlobSource or FileSystem.</span></span> |<span data-ttu-id="5338d-266">**PreserveHierarchy:** bevarar hello filen hierarki i hello målmappen.</span><span class="sxs-lookup"><span data-stu-id="5338d-266">**PreserveHierarchy:** Preserves hello file hierarchy in hello target folder.</span></span> <span data-ttu-id="5338d-267">Hello relativ sökväg hello filen toohello källa källmappen är det vill säga hello samma som hello relativa sökväg hello filen toohello mål målmappen.</span><span class="sxs-lookup"><span data-stu-id="5338d-267">That is, hello relative path of hello source file toohello source folder is hello same as hello relative path of hello target file toohello target folder.</span></span><br/><br/><span data-ttu-id="5338d-268">**FlattenHierarchy:** alla filer från källmappen hello skapas i hello första nivån i målmappen.</span><span class="sxs-lookup"><span data-stu-id="5338d-268">**FlattenHierarchy:** All files from hello source folder are created in hello first level of target folder.</span></span> <span data-ttu-id="5338d-269">hello mål filer skapas med ett namn som genererats automatiskt.</span><span class="sxs-lookup"><span data-stu-id="5338d-269">hello target files are created with an autogenerated name.</span></span><br/><br/><span data-ttu-id="5338d-270">**MergeFiles:** sammanfogar alla filer från hello källfil mappen tooone.</span><span class="sxs-lookup"><span data-stu-id="5338d-270">**MergeFiles:** Merges all files from hello source folder tooone file.</span></span> <span data-ttu-id="5338d-271">Om hello namn/blob filnamn har angetts är hello kopplade filnamnet hello angivet namn.</span><span class="sxs-lookup"><span data-stu-id="5338d-271">If hello file name/blob name is specified, hello merged file name is hello specified name.</span></span> <span data-ttu-id="5338d-272">Annars är ett automatiskt genererat namn.</span><span class="sxs-lookup"><span data-stu-id="5338d-272">Otherwise, it is an auto-generated file name.</span></span> |<span data-ttu-id="5338d-273">Nej</span><span class="sxs-lookup"><span data-stu-id="5338d-273">No</span></span> |

### <a name="recursive-and-copybehavior-examples"></a><span data-ttu-id="5338d-274">rekursiva och copyBehavior exempel</span><span class="sxs-lookup"><span data-stu-id="5338d-274">recursive and copyBehavior examples</span></span>
<span data-ttu-id="5338d-275">Det här avsnittet beskrivs hello resultatet av hello kopia för olika kombinationer av värden för egenskaperna för hello rekursiv och copyBehavior.</span><span class="sxs-lookup"><span data-stu-id="5338d-275">This section describes hello resulting behavior of hello Copy operation for different combinations of values for hello recursive and copyBehavior properties.</span></span>

| <span data-ttu-id="5338d-276">rekursiva värde</span><span class="sxs-lookup"><span data-stu-id="5338d-276">recursive value</span></span> | <span data-ttu-id="5338d-277">copyBehavior värde</span><span class="sxs-lookup"><span data-stu-id="5338d-277">copyBehavior value</span></span> | <span data-ttu-id="5338d-278">Resultatet</span><span class="sxs-lookup"><span data-stu-id="5338d-278">Resulting behavior</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5338d-279">SANT</span><span class="sxs-lookup"><span data-stu-id="5338d-279">true</span></span> |<span data-ttu-id="5338d-280">preserveHierarchy</span><span class="sxs-lookup"><span data-stu-id="5338d-280">preserveHierarchy</span></span> |<span data-ttu-id="5338d-281">För en källmapp Mapp1 med hello följande struktur,</span><span class="sxs-lookup"><span data-stu-id="5338d-281">For a source folder Folder1 with hello following structure,</span></span><br/><br/><span data-ttu-id="5338d-282">Mapp1</span><span class="sxs-lookup"><span data-stu-id="5338d-282">Folder1</span></span><br/><span data-ttu-id="5338d-283">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="5338d-283">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="5338d-284">&nbsp;&nbsp;&nbsp;&nbsp;Fil2</span><span class="sxs-lookup"><span data-stu-id="5338d-284">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="5338d-285">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="5338d-285">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="5338d-286">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3</span><span class="sxs-lookup"><span data-stu-id="5338d-286">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="5338d-287">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="5338d-287">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="5338d-288">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="5338d-288">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="5338d-289">hello målmappen Mapp1 skapas med samma struktur som källa för hello hello:</span><span class="sxs-lookup"><span data-stu-id="5338d-289">hello target folder Folder1 is created with hello same structure as hello source:</span></span><br/><br/><span data-ttu-id="5338d-290">Mapp1</span><span class="sxs-lookup"><span data-stu-id="5338d-290">Folder1</span></span><br/><span data-ttu-id="5338d-291">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="5338d-291">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="5338d-292">&nbsp;&nbsp;&nbsp;&nbsp;Fil2</span><span class="sxs-lookup"><span data-stu-id="5338d-292">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="5338d-293">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="5338d-293">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="5338d-294">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3</span><span class="sxs-lookup"><span data-stu-id="5338d-294">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="5338d-295">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="5338d-295">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="5338d-296">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="5338d-296">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span> |
| <span data-ttu-id="5338d-297">SANT</span><span class="sxs-lookup"><span data-stu-id="5338d-297">true</span></span> |<span data-ttu-id="5338d-298">flattenHierarchy</span><span class="sxs-lookup"><span data-stu-id="5338d-298">flattenHierarchy</span></span> |<span data-ttu-id="5338d-299">För en källmapp Mapp1 med hello följande struktur,</span><span class="sxs-lookup"><span data-stu-id="5338d-299">For a source folder Folder1 with hello following structure,</span></span><br/><br/><span data-ttu-id="5338d-300">Mapp1</span><span class="sxs-lookup"><span data-stu-id="5338d-300">Folder1</span></span><br/><span data-ttu-id="5338d-301">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="5338d-301">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="5338d-302">&nbsp;&nbsp;&nbsp;&nbsp;Fil2</span><span class="sxs-lookup"><span data-stu-id="5338d-302">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="5338d-303">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="5338d-303">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="5338d-304">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3</span><span class="sxs-lookup"><span data-stu-id="5338d-304">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="5338d-305">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="5338d-305">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="5338d-306">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="5338d-306">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="5338d-307">hello mål Mapp1 skapas med följande struktur hello:</span><span class="sxs-lookup"><span data-stu-id="5338d-307">hello target Folder1 is created with hello following structure:</span></span> <br/><br/><span data-ttu-id="5338d-308">Mapp1</span><span class="sxs-lookup"><span data-stu-id="5338d-308">Folder1</span></span><br/><span data-ttu-id="5338d-309">&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererade namnet på File1</span><span class="sxs-lookup"><span data-stu-id="5338d-309">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File1</span></span><br/><span data-ttu-id="5338d-310">&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererat namn för fil2</span><span class="sxs-lookup"><span data-stu-id="5338d-310">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File2</span></span><br/><span data-ttu-id="5338d-311">&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererat namn för fil3</span><span class="sxs-lookup"><span data-stu-id="5338d-311">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File3</span></span><br/><span data-ttu-id="5338d-312">&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererat namn för File4</span><span class="sxs-lookup"><span data-stu-id="5338d-312">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File4</span></span><br/><span data-ttu-id="5338d-313">&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererat namn för File5</span><span class="sxs-lookup"><span data-stu-id="5338d-313">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File5</span></span> |
| <span data-ttu-id="5338d-314">SANT</span><span class="sxs-lookup"><span data-stu-id="5338d-314">true</span></span> |<span data-ttu-id="5338d-315">mergeFiles</span><span class="sxs-lookup"><span data-stu-id="5338d-315">mergeFiles</span></span> |<span data-ttu-id="5338d-316">För en källmapp Mapp1 med hello följande struktur,</span><span class="sxs-lookup"><span data-stu-id="5338d-316">For a source folder Folder1 with hello following structure,</span></span><br/><br/><span data-ttu-id="5338d-317">Mapp1</span><span class="sxs-lookup"><span data-stu-id="5338d-317">Folder1</span></span><br/><span data-ttu-id="5338d-318">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="5338d-318">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="5338d-319">&nbsp;&nbsp;&nbsp;&nbsp;Fil2</span><span class="sxs-lookup"><span data-stu-id="5338d-319">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="5338d-320">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="5338d-320">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="5338d-321">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3</span><span class="sxs-lookup"><span data-stu-id="5338d-321">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="5338d-322">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="5338d-322">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="5338d-323">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="5338d-323">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="5338d-324">hello mål Mapp1 skapas med följande struktur hello:</span><span class="sxs-lookup"><span data-stu-id="5338d-324">hello target Folder1 is created with hello following structure:</span></span> <br/><br/><span data-ttu-id="5338d-325">Mapp1</span><span class="sxs-lookup"><span data-stu-id="5338d-325">Folder1</span></span><br/><span data-ttu-id="5338d-326">&nbsp;&nbsp;&nbsp;&nbsp;Fil1 + fil2 + fil3 + File4 + filen 5 innehållet slås samman till en fil med ett automatiskt genererat namn.</span><span class="sxs-lookup"><span data-stu-id="5338d-326">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 + File3 + File4 + File 5 contents are merged into one file with an auto-generated file name.</span></span> |
| <span data-ttu-id="5338d-327">FALSKT</span><span class="sxs-lookup"><span data-stu-id="5338d-327">false</span></span> |<span data-ttu-id="5338d-328">preserveHierarchy</span><span class="sxs-lookup"><span data-stu-id="5338d-328">preserveHierarchy</span></span> |<span data-ttu-id="5338d-329">För en källmapp Mapp1 med hello följande struktur,</span><span class="sxs-lookup"><span data-stu-id="5338d-329">For a source folder Folder1 with hello following structure,</span></span><br/><br/><span data-ttu-id="5338d-330">Mapp1</span><span class="sxs-lookup"><span data-stu-id="5338d-330">Folder1</span></span><br/><span data-ttu-id="5338d-331">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="5338d-331">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="5338d-332">&nbsp;&nbsp;&nbsp;&nbsp;Fil2</span><span class="sxs-lookup"><span data-stu-id="5338d-332">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="5338d-333">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="5338d-333">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="5338d-334">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3</span><span class="sxs-lookup"><span data-stu-id="5338d-334">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="5338d-335">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="5338d-335">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="5338d-336">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="5338d-336">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="5338d-337">hello målmappen Mapp1 skapas med följande struktur hello:</span><span class="sxs-lookup"><span data-stu-id="5338d-337">hello target folder Folder1 is created with hello following structure:</span></span><br/><br/><span data-ttu-id="5338d-338">Mapp1</span><span class="sxs-lookup"><span data-stu-id="5338d-338">Folder1</span></span><br/><span data-ttu-id="5338d-339">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="5338d-339">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="5338d-340">&nbsp;&nbsp;&nbsp;&nbsp;Fil2</span><span class="sxs-lookup"><span data-stu-id="5338d-340">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><br/><span data-ttu-id="5338d-341">Subfolder1 med fil3, File4 och File5 hämtas inte.</span><span class="sxs-lookup"><span data-stu-id="5338d-341">Subfolder1 with File3, File4, and File5 is not picked up.</span></span> |
| <span data-ttu-id="5338d-342">FALSKT</span><span class="sxs-lookup"><span data-stu-id="5338d-342">false</span></span> |<span data-ttu-id="5338d-343">flattenHierarchy</span><span class="sxs-lookup"><span data-stu-id="5338d-343">flattenHierarchy</span></span> |<span data-ttu-id="5338d-344">För en källmapp Mapp1 med hello följande struktur,</span><span class="sxs-lookup"><span data-stu-id="5338d-344">For a source folder Folder1 with hello following structure,</span></span><br/><br/><span data-ttu-id="5338d-345">Mapp1</span><span class="sxs-lookup"><span data-stu-id="5338d-345">Folder1</span></span><br/><span data-ttu-id="5338d-346">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="5338d-346">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="5338d-347">&nbsp;&nbsp;&nbsp;&nbsp;Fil2</span><span class="sxs-lookup"><span data-stu-id="5338d-347">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="5338d-348">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="5338d-348">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="5338d-349">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3</span><span class="sxs-lookup"><span data-stu-id="5338d-349">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="5338d-350">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="5338d-350">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="5338d-351">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="5338d-351">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="5338d-352">hello målmappen Mapp1 skapas med följande struktur hello:</span><span class="sxs-lookup"><span data-stu-id="5338d-352">hello target folder Folder1 is created with hello following structure:</span></span><br/><br/><span data-ttu-id="5338d-353">Mapp1</span><span class="sxs-lookup"><span data-stu-id="5338d-353">Folder1</span></span><br/><span data-ttu-id="5338d-354">&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererade namnet på File1</span><span class="sxs-lookup"><span data-stu-id="5338d-354">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File1</span></span><br/><span data-ttu-id="5338d-355">&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererat namn för fil2</span><span class="sxs-lookup"><span data-stu-id="5338d-355">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File2</span></span><br/><br/><span data-ttu-id="5338d-356">Subfolder1 med fil3, File4 och File5 hämtas inte.</span><span class="sxs-lookup"><span data-stu-id="5338d-356">Subfolder1 with File3, File4, and File5 is not picked up.</span></span> |
| <span data-ttu-id="5338d-357">FALSKT</span><span class="sxs-lookup"><span data-stu-id="5338d-357">false</span></span> |<span data-ttu-id="5338d-358">mergeFiles</span><span class="sxs-lookup"><span data-stu-id="5338d-358">mergeFiles</span></span> |<span data-ttu-id="5338d-359">För en källmapp Mapp1 med hello följande struktur,</span><span class="sxs-lookup"><span data-stu-id="5338d-359">For a source folder Folder1 with hello following structure,</span></span><br/><br/><span data-ttu-id="5338d-360">Mapp1</span><span class="sxs-lookup"><span data-stu-id="5338d-360">Folder1</span></span><br/><span data-ttu-id="5338d-361">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="5338d-361">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="5338d-362">&nbsp;&nbsp;&nbsp;&nbsp;Fil2</span><span class="sxs-lookup"><span data-stu-id="5338d-362">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="5338d-363">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="5338d-363">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="5338d-364">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3</span><span class="sxs-lookup"><span data-stu-id="5338d-364">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="5338d-365">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="5338d-365">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="5338d-366">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="5338d-366">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="5338d-367">hello målmappen Mapp1 skapas med följande struktur hello:</span><span class="sxs-lookup"><span data-stu-id="5338d-367">hello target folder Folder1 is created with hello following structure:</span></span><br/><br/><span data-ttu-id="5338d-368">Mapp1</span><span class="sxs-lookup"><span data-stu-id="5338d-368">Folder1</span></span><br/><span data-ttu-id="5338d-369">&nbsp;&nbsp;&nbsp;&nbsp;Fil1 + fil2 innehållet slås samman till en fil med ett automatiskt genererat namn.</span><span class="sxs-lookup"><span data-stu-id="5338d-369">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 contents are merged into one file with an auto-generated file name.</span></span><br/><span data-ttu-id="5338d-370">&nbsp;&nbsp;&nbsp;&nbsp;Automatiskt genererade namnet på File1</span><span class="sxs-lookup"><span data-stu-id="5338d-370">&nbsp;&nbsp;&nbsp;&nbsp;Auto-generated name for File1</span></span><br/><br/><span data-ttu-id="5338d-371">Subfolder1 med fil3, File4 och File5 hämtas inte.</span><span class="sxs-lookup"><span data-stu-id="5338d-371">Subfolder1 with File3, File4, and File5 is not picked up.</span></span> |

## <a name="supported-file-and-compression-formats"></a><span data-ttu-id="5338d-372">Fil- och komprimering format som stöds</span><span class="sxs-lookup"><span data-stu-id="5338d-372">Supported file and compression formats</span></span>
<span data-ttu-id="5338d-373">Se [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md) artikeln för information.</span><span class="sxs-lookup"><span data-stu-id="5338d-373">See [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md) article on details.</span></span>

## <a name="json-examples-for-copying-data-tooand-from-file-system"></a><span data-ttu-id="5338d-374">JSON-exempel för att kopiera data tooand från filsystemet</span><span class="sxs-lookup"><span data-stu-id="5338d-374">JSON examples for copying data tooand from file system</span></span>
<span data-ttu-id="5338d-375">hello följande exempel ger exempel JSON definitioner som du kan använda toocreate en pipeline med hjälp av hello [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="5338d-375">hello following examples provide sample JSON definitions that you can use toocreate a pipeline by using hello [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="5338d-376">De visar hur toocopy data tooand från ett lokalt filsystem och Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="5338d-376">They show how toocopy data tooand from an on-premises file system and Azure Blob storage.</span></span> <span data-ttu-id="5338d-377">Du kan dock kopiera data *direkt* från någon av hello källor tooany av hello sänkor som anges i [källor och sänkor stöds](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hjälp av Kopieringsaktiviteten i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="5338d-377">However, you can copy data *directly* from any of hello sources tooany of hello sinks listed in [Supported sources and sinks](data-factory-data-movement-activities.md#supported-data-stores-and-formats) by using Copy Activity in Azure Data Factory.</span></span>

### <a name="example-copy-data-from-an-on-premises-file-system-tooazure-blob-storage"></a><span data-ttu-id="5338d-378">Exempel: Kopiera data från en lokal fil system tooAzure Blob storage</span><span class="sxs-lookup"><span data-stu-id="5338d-378">Example: Copy data from an on-premises file system tooAzure Blob storage</span></span>
<span data-ttu-id="5338d-379">Det här exemplet visas hur toocopy data från en lokal fil system tooAzure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="5338d-379">This sample shows how toocopy data from an on-premises file system tooAzure Blob storage.</span></span> <span data-ttu-id="5338d-380">hello exemplet har hello följande Data Factory-enheter:</span><span class="sxs-lookup"><span data-stu-id="5338d-380">hello sample has hello following Data Factory entities:</span></span>

* <span data-ttu-id="5338d-381">En länkad tjänst av typen [OnPremisesFileServer](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="5338d-381">A linked service of type [OnPremisesFileServer](#linked-service-properties).</span></span>
* <span data-ttu-id="5338d-382">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="5338d-382">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
* <span data-ttu-id="5338d-383">Indata [dataset](data-factory-create-datasets.md) av typen [filresursen](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="5338d-383">An input [dataset](data-factory-create-datasets.md) of type [FileShare](#dataset-properties).</span></span>
* <span data-ttu-id="5338d-384">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="5338d-384">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
* <span data-ttu-id="5338d-385">En [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [FileSystemSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="5338d-385">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [FileSystemSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="5338d-386">hello följande exempel kopierar time series-data från en lokal fil system tooAzure Blob storage varje timme.</span><span class="sxs-lookup"><span data-stu-id="5338d-386">hello following sample copies time-series data from an on-premises file system tooAzure Blob storage every hour.</span></span> <span data-ttu-id="5338d-387">hello JSON egenskaper som används i exemplen beskrivs i hello avsnitt efter hello prover.</span><span class="sxs-lookup"><span data-stu-id="5338d-387">hello JSON properties that are used in these samples are described in hello sections after hello samples.</span></span>

<span data-ttu-id="5338d-388">Som ett första steg bör ställa in Data Management Gateway enligt hello instruktionerna i [flytta data mellan lokala källor och hello moln med Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md).</span><span class="sxs-lookup"><span data-stu-id="5338d-388">As a first step, set up Data Management Gateway as per hello instructions in [Move data between on-premises sources and hello cloud with Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md).</span></span>

<span data-ttu-id="5338d-389">**Lokala filserver länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="5338d-389">**On-Premises File Server linked service:**</span></span>

```JSON
{
  "Name": "OnPremisesFileServerLinkedService",
  "properties": {
    "type": "OnPremisesFileServer",
    "typeProperties": {
      "host": "\\\\Contosogame-Asia.<region>.corp.<company>.com",
      "userid": "Admin",
      "password": "123456",
      "gatewayName": "mygateway"
    }
  }
}
```

<span data-ttu-id="5338d-390">Vi rekommenderar att du använder hello **encryptedCredential** egenskapen i stället hello **userid** och **lösenord** egenskaper.</span><span class="sxs-lookup"><span data-stu-id="5338d-390">We recommend using hello **encryptedCredential** property instead hello **userid** and **password** properties.</span></span> <span data-ttu-id="5338d-391">Se [filserver länkade tjänsten](#linked-service-properties) mer information om den här länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="5338d-391">See [File Server linked service](#linked-service-properties) for details about this linked service.</span></span>

<span data-ttu-id="5338d-392">**Azure Storage länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="5338d-392">**Azure Storage linked service:**</span></span>

```JSON
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```

<span data-ttu-id="5338d-393">**Lokal fil system inkommande datauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="5338d-393">**On-premises file system input dataset:**</span></span>

<span data-ttu-id="5338d-394">Data hämtas från en ny fil varje timme.</span><span class="sxs-lookup"><span data-stu-id="5338d-394">Data is picked up from a new file every hour.</span></span> <span data-ttu-id="5338d-395">Hej folderPath och fileName egenskaper bestäms baserat på hello starttiden för hello sektorn.</span><span class="sxs-lookup"><span data-stu-id="5338d-395">hello folderPath and fileName properties are determined based on hello start time of hello slice.</span></span>  

<span data-ttu-id="5338d-396">Ange `"external": "true"` informerar Data Factory som hello dataset är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="5338d-396">Setting `"external": "true"` informs Data Factory that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

```JSON
{
  "name": "OnpremisesFileSystemInput",
  "properties": {
    "type": " FileShare",
    "linkedServiceName": " OnPremisesFileServerLinkedService ",
    "typeProperties": {
      "folderPath": "mysharedfolder/yearno={Year}/monthno={Month}/dayno={Day}",
      "fileName": "{Hour}.csv",
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
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      "externalData": {
        "retryInterval": "00:01:00",
        "retryTimeout": "00:10:00",
        "maximumRetry": 3
      }
    }
  }
}
```

<span data-ttu-id="5338d-397">**Azure Blob storage utdatauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="5338d-397">**Azure Blob storage output dataset:**</span></span>

<span data-ttu-id="5338d-398">Data skrivs tooa nya blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="5338d-398">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="5338d-399">hello mappsökväg för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="5338d-399">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="5338d-400">hello mappsökväg använder hello år, månad, dag och timme delar av hello starttid.</span><span class="sxs-lookup"><span data-stu-id="5338d-400">hello folder path uses hello year, month, day, and hour parts of hello start time.</span></span>

```JSON
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": "\t",
        "rowDelimiter": "\n"
      }
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

<span data-ttu-id="5338d-401">**Kopieringsaktiviteten i en pipeline med filsystemet källa och mottagare för Blob:**</span><span class="sxs-lookup"><span data-stu-id="5338d-401">**A copy activity in a pipeline with File System source and Blob sink:**</span></span>

<span data-ttu-id="5338d-402">hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme.</span><span class="sxs-lookup"><span data-stu-id="5338d-402">hello pipeline contains a copy activity that is configured toouse hello input and output datasets, and is scheduled toorun every hour.</span></span> <span data-ttu-id="5338d-403">I hello pipeline JSON-definitionen hello **källa** typ har angetts för**FileSystemSource**, och **sink** typ har angetts för**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="5338d-403">In hello pipeline JSON definition, hello **source** type is set too**FileSystemSource**, and **sink** type is set too**BlobSink**.</span></span>

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2015-06-01T18:00:00",
    "end":"2015-06-01T19:00:00",
    "description":"Pipeline for copy activity",
    "activities":[  
      {
        "name": "OnpremisesFileSystemtoBlob",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "OnpremisesFileSystemInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "FileSystemSource"
          },
          "sink": {
            "type": "BlobSink"
          }
        },
       "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
     ]
   }
}
```

### <a name="example-copy-data-from-azure-sql-database-tooan-on-premises-file-system"></a><span data-ttu-id="5338d-404">Exempel: Kopiera data från Azure SQL Database tooan lokalt filsystem</span><span class="sxs-lookup"><span data-stu-id="5338d-404">Example: Copy data from Azure SQL Database tooan on-premises file system</span></span>
<span data-ttu-id="5338d-405">följande exempel visar hello:</span><span class="sxs-lookup"><span data-stu-id="5338d-405">hello following sample shows:</span></span>

* <span data-ttu-id="5338d-406">En länkad tjänst av typen [AzureSqlDatabase.](data-factory-azure-sql-connector.md#linked-service-properties)</span><span class="sxs-lookup"><span data-stu-id="5338d-406">A linked service of type [AzureSqlDatabase.](data-factory-azure-sql-connector.md#linked-service-properties)</span></span>
* <span data-ttu-id="5338d-407">En länkad tjänst av typen [OnPremisesFileServer](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="5338d-407">A linked service of type [OnPremisesFileServer](#linked-service-properties).</span></span>
* <span data-ttu-id="5338d-408">En datauppsättning som indata av typen [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="5338d-408">An input dataset of type [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).</span></span>
* <span data-ttu-id="5338d-409">En datamängd för utdata av typen [filresursen](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="5338d-409">An output dataset of type [FileShare](#dataset-properties).</span></span>
* <span data-ttu-id="5338d-410">En pipeline med en kopia-aktivitet som använder [SqlSource](data-factory-azure-sql-connector.md##copy-activity-properties) och [FileSystemSink](#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="5338d-410">A pipeline with a copy activity that uses [SqlSource](data-factory-azure-sql-connector.md##copy-activity-properties) and [FileSystemSink](#copy-activity-properties).</span></span>

<span data-ttu-id="5338d-411">hello exemplet kopierar time series-data från ett Azure SQL tabell tooan lokalt filsystem varje timme.</span><span class="sxs-lookup"><span data-stu-id="5338d-411">hello sample copies time-series data from an Azure SQL table tooan on-premises file system every hour.</span></span> <span data-ttu-id="5338d-412">hello JSON egenskaper som används i exemplen beskrivs i avsnitt efter hello prover.</span><span class="sxs-lookup"><span data-stu-id="5338d-412">hello JSON properties that are used in these samples are described in sections after hello samples.</span></span>

<span data-ttu-id="5338d-413">**Azure SQL Database länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="5338d-413">**Azure SQL Database linked service:**</span></span>

```JSON
{
  "name": "AzureSqlLinkedService",
  "properties": {
    "type": "AzureSqlDatabase",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```

<span data-ttu-id="5338d-414">**Lokala filserver länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="5338d-414">**On-Premises File Server linked service:**</span></span>

```JSON
{
  "Name": "OnPremisesFileServerLinkedService",
  "properties": {
    "type": "OnPremisesFileServer",
    "typeProperties": {
      "host": "\\\\Contosogame-Asia.<region>.corp.<company>.com",
      "userid": "Admin",
      "password": "123456",
      "gatewayName": "mygateway"
    }
  }
}
```

<span data-ttu-id="5338d-415">Vi rekommenderar att du använder hello **encryptedCredential** egenskapen istället för att använda hello **userid** och **lösenord** egenskaper.</span><span class="sxs-lookup"><span data-stu-id="5338d-415">We recommend using hello **encryptedCredential** property instead of using hello **userid** and **password** properties.</span></span> <span data-ttu-id="5338d-416">Se [filsystemet länkade tjänsten](#linked-service-properties) mer information om den här länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="5338d-416">See [File System linked service](#linked-service-properties) for details about this linked service.</span></span>

<span data-ttu-id="5338d-417">**Azure SQL inkommande datamängd:**</span><span class="sxs-lookup"><span data-stu-id="5338d-417">**Azure SQL input dataset:**</span></span>

<span data-ttu-id="5338d-418">hello exemplet förutsätter att du har skapat en tabell ”mytable” som prefix i Azure SQL och den innehåller en kolumn med namnet ”timestampcolumn” för time series-data.</span><span class="sxs-lookup"><span data-stu-id="5338d-418">hello sample assumes that you've created a table “MyTable” in Azure SQL, and it contains a column called “timestampcolumn” for time-series data.</span></span>

<span data-ttu-id="5338d-419">Ange ``“external”: ”true”`` informerar Data Factory som hello dataset är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="5338d-419">Setting ``“external”: ”true”`` informs Data Factory that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

```JSON
{
  "name": "AzureSqlInput",
  "properties": {
    "type": "AzureSqlTable",
    "linkedServiceName": "AzureSqlLinkedService",
    "typeProperties": {
      "tableName": "MyTable"
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      "externalData": {
        "retryInterval": "00:01:00",
        "retryTimeout": "00:10:00",
        "maximumRetry": 3
      }
    }
  }
}
```

<span data-ttu-id="5338d-420">**Lokal fil system utdatauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="5338d-420">**On-premises file system output dataset:**</span></span>

<span data-ttu-id="5338d-421">Data är kopierade tooa ny fil varje timme.</span><span class="sxs-lookup"><span data-stu-id="5338d-421">Data is copied tooa new file every hour.</span></span> <span data-ttu-id="5338d-422">hello folderPath och filnamn för hello blob bestäms baserat på hello starttiden för hello sektorn.</span><span class="sxs-lookup"><span data-stu-id="5338d-422">hello folderPath and fileName for hello blob are determined based on hello start time of hello slice.</span></span>

```JSON
{
  "name": "OnpremisesFileSystemOutput",
  "properties": {
    "type": "FileShare",
    "linkedServiceName": " OnPremisesFileServerLinkedService ",
    "typeProperties": {
      "folderPath": "mysharedfolder/yearno={Year}/monthno={Month}/dayno={Day}",
      "fileName": "{Hour}.csv",
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
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      "externalData": {
        "retryInterval": "00:01:00",
        "retryTimeout": "00:10:00",
        "maximumRetry": 3
      }
    }
  }
}
```

<span data-ttu-id="5338d-423">**Kopieringsaktiviteten i en pipeline med SQL-källa och mottagare för filsystemet:**</span><span class="sxs-lookup"><span data-stu-id="5338d-423">**A copy activity in a pipeline with SQL source and File System sink:**</span></span>

<span data-ttu-id="5338d-424">hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse hello inkommande och utgående datauppsättningar och är schemalagda toorun varje timme.</span><span class="sxs-lookup"><span data-stu-id="5338d-424">hello pipeline contains a copy activity that is configured toouse hello input and output datasets, and is scheduled toorun every hour.</span></span> <span data-ttu-id="5338d-425">I hello pipeline JSON-definitionen hello **källa** typ har angetts för**SqlSource**, och hello **sink** typ har angetts för**FileSystemSink**.</span><span class="sxs-lookup"><span data-stu-id="5338d-425">In hello pipeline JSON definition, hello **source** type is set too**SqlSource**, and hello **sink** type is set too**FileSystemSink**.</span></span> <span data-ttu-id="5338d-426">hello SQL-fråga som har angetts för hello **SqlReaderQuery** egenskapen väljer hello data i hello tidigare timme toocopy.</span><span class="sxs-lookup"><span data-stu-id="5338d-426">hello SQL query that is specified for hello **SqlReaderQuery** property selects hello data in hello past hour toocopy.</span></span>

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2015-06-01T18:00:00",
    "end":"2015-06-01T20:00:00",
    "description":"pipeline for copy activity",
    "activities":[  
      {
        "name": "AzureSQLtoOnPremisesFile",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureSQLInput"
          }
        ],
        "outputs": [
          {
            "name": "OnpremisesFileSystemOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "SqlSource",
            "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd}\\'', WindowStart, WindowEnd)"
          },
          "sink": {
            "type": "FileSystemSink"
          }
        },
       "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 3,
          "timeout": "01:00:00"
        }
      }
     ]
   }
}
```


<span data-ttu-id="5338d-427">Du kan också mappa kolumner från källan dataset toocolumns från sink datauppsättning i aktivitetsdefinitionen för hello kopia.</span><span class="sxs-lookup"><span data-stu-id="5338d-427">You can also map columns from source dataset toocolumns from sink dataset in hello copy activity definition.</span></span> <span data-ttu-id="5338d-428">Mer information finns i [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="5338d-428">For details, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="5338d-429">Prestanda- och justering</span><span class="sxs-lookup"><span data-stu-id="5338d-429">Performance and tuning</span></span>
 <span data-ttu-id="5338d-430">toolearn om nyckeln faktorer som påverkan hello prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt toooptimize, se hello [prestandajustering guide och Kopieringsaktivitet prestanda](data-factory-copy-activity-performance.md).</span><span class="sxs-lookup"><span data-stu-id="5338d-430">toolearn about key factors that impact hello performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it, see hello [Copy Activity performance and tuning guide](data-factory-copy-activity-performance.md).</span></span>
