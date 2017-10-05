---
title: "Kopiera data till/från ett filsystem med Azure Data Factory | Microsoft Docs"
description: "Lär dig mer om att kopiera data till och från ett lokalt filsystem med hjälp av Azure Data Factory."
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
ms.openlocfilehash: 52305e54f539de6aba2ba9cc856a09e04d608ded
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="copy-data-to-and-from-an-on-premises-file-system-by-using-azure-data-factory"></a><span data-ttu-id="819d6-103">Kopiera data till och från ett lokalt filsystem med hjälp av Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="819d6-103">Copy data to and from an on-premises file system by using Azure Data Factory</span></span>
<span data-ttu-id="819d6-104">Den här artikeln förklarar hur du använder aktiviteten kopiera i Azure Data Factory för att kopiera data till och från ett lokalt filsystem.</span><span class="sxs-lookup"><span data-stu-id="819d6-104">This article explains how to use the Copy Activity in Azure Data Factory to copy data to/from an on-premises file system.</span></span> <span data-ttu-id="819d6-105">Den bygger på den [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som presenterar en allmän översikt över dataflyttning med copy-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="819d6-105">It builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span>

## <a name="supported-scenarios"></a><span data-ttu-id="819d6-106">Scenarier som stöds</span><span class="sxs-lookup"><span data-stu-id="819d6-106">Supported scenarios</span></span>
<span data-ttu-id="819d6-107">Du kan kopiera data **från ett lokalt filsystem** till följande data lagras:</span><span class="sxs-lookup"><span data-stu-id="819d6-107">You can copy data **from an on-premises file system** to the following data stores:</span></span>

[!INCLUDE [data-factory-supported-sink](../../includes/data-factory-supported-sinks.md)]

<span data-ttu-id="819d6-108">Du kan kopiera data från följande datalager **till ett lokalt filsystem**:</span><span class="sxs-lookup"><span data-stu-id="819d6-108">You can copy data from the following data stores **to an on-premises file system**:</span></span>

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

> [!NOTE]
> <span data-ttu-id="819d6-109">Kopieringsaktiviteten tar inte bort källfilen när den har kopierats till målet.</span><span class="sxs-lookup"><span data-stu-id="819d6-109">Copy Activity does not delete the source file after it is successfully copied to the destination.</span></span> <span data-ttu-id="819d6-110">Om du vill ta bort källfilen efter en lyckad kopiering kan du skapa en anpassad aktivitet för att ta bort filen och använda aktiviteten i pipelinen.</span><span class="sxs-lookup"><span data-stu-id="819d6-110">If you need to delete the source file after a successful copy, create a custom activity to delete the file and use the activity in the pipeline.</span></span> 

## <a name="enabling-connectivity"></a><span data-ttu-id="819d6-111">Aktivera anslutning</span><span class="sxs-lookup"><span data-stu-id="819d6-111">Enabling connectivity</span></span>
<span data-ttu-id="819d6-112">Data Factory stöder anslutning till och från ett lokalt filsystem via **Data Management Gateway**.</span><span class="sxs-lookup"><span data-stu-id="819d6-112">Data Factory supports connecting to and from an on-premises file system via **Data Management Gateway**.</span></span> <span data-ttu-id="819d6-113">Data Management Gateway måste du installera i din lokala miljö för tjänsten Data Factory för att ansluta till alla stöds lokala datalagret inklusive filsystemet.</span><span class="sxs-lookup"><span data-stu-id="819d6-113">You must install the Data Management Gateway in your on-premises environment for the Data Factory service to connect to any supported on-premises data store including file system.</span></span> <span data-ttu-id="819d6-114">Mer information om Data Management Gateway och stegvisa instruktioner om hur du konfigurerar gatewayen finns [flytta data mellan lokala källor och molnet med Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md).</span><span class="sxs-lookup"><span data-stu-id="819d6-114">To learn about Data Management Gateway and for step-by-step instructions on setting up the gateway, see [Move data between on-premises sources and the cloud with Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md).</span></span> <span data-ttu-id="819d6-115">Inga andra binära filer måste installeras för att kommunicera till och från ett lokalt filsystem förutom Data Management Gateway.</span><span class="sxs-lookup"><span data-stu-id="819d6-115">Apart from Data Management Gateway, no other binary files need to be installed to communicate to and from an on-premises file system.</span></span> <span data-ttu-id="819d6-116">Du måste installera och använda Data Management Gateway, även om filsystemet är i Azure IaaS-VM.</span><span class="sxs-lookup"><span data-stu-id="819d6-116">You must install and use the Data Management Gateway even if the file system is in Azure IaaS VM.</span></span> <span data-ttu-id="819d6-117">Detaljerad information om gateway finns [Data Management Gateway](data-factory-data-management-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="819d6-117">For detailed information about the gateway, see [Data Management Gateway](data-factory-data-management-gateway.md).</span></span>

<span data-ttu-id="819d6-118">Om du vill använda en Linux-filresurs, installera [Samba](https://www.samba.org/) på Linux-servern och installera Data Management Gateway på en Windows server.</span><span class="sxs-lookup"><span data-stu-id="819d6-118">To use a Linux file share, install [Samba](https://www.samba.org/) on your Linux server, and install Data Management Gateway on a Windows server.</span></span> <span data-ttu-id="819d6-119">Det går inte att installera Data Management Gateway på en Linux-server.</span><span class="sxs-lookup"><span data-stu-id="819d6-119">Installing Data Management Gateway on a Linux server is not supported.</span></span>

## <a name="getting-started"></a><span data-ttu-id="819d6-120">Komma igång</span><span class="sxs-lookup"><span data-stu-id="819d6-120">Getting started</span></span>
<span data-ttu-id="819d6-121">Du kan skapa en pipeline med en kopia-aktivitet som flyttar data till/från ett filsystem med hjälp av olika verktyg/API: er.</span><span class="sxs-lookup"><span data-stu-id="819d6-121">You can create a pipeline with a copy activity that moves data to/from a file system by using different tools/APIs.</span></span>

<span data-ttu-id="819d6-122">Det enklaste sättet att skapa en pipeline är att använda den **guiden Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="819d6-122">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="819d6-123">Finns [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden Kopiera data.</span><span class="sxs-lookup"><span data-stu-id="819d6-123">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span>

<span data-ttu-id="819d6-124">Du kan också använda följande verktyg för att skapa en pipeline: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall**, **.NET API**, och **REST API**.</span><span class="sxs-lookup"><span data-stu-id="819d6-124">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="819d6-125">Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner för att skapa en pipeline med en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="819d6-125">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span>

<span data-ttu-id="819d6-126">Om du använder verktyg eller API: er, kan du utföra följande steg för att skapa en pipeline som flyttar data från ett dataarkiv som källa till ett dataarkiv som mottagare:</span><span class="sxs-lookup"><span data-stu-id="819d6-126">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span>

1. <span data-ttu-id="819d6-127">Skapa en **datafabriken**.</span><span class="sxs-lookup"><span data-stu-id="819d6-127">Create a **data factory**.</span></span> <span data-ttu-id="819d6-128">En datafabrik kan innehålla en eller flera pipelines.</span><span class="sxs-lookup"><span data-stu-id="819d6-128">A data factory may contain one or more pipelines.</span></span> 
2. <span data-ttu-id="819d6-129">Skapa **länkade tjänster** att länka inkommande och utgående data lagras till din data factory.</span><span class="sxs-lookup"><span data-stu-id="819d6-129">Create **linked services** to link input and output data stores to your data factory.</span></span> <span data-ttu-id="819d6-130">Om du kopierar data från ett Azure blob storage till ett lokalt filsystem, skapa till exempel två länkade tjänster om du vill länka dina lokala filsystemet och Azure storage-konto till din data factory.</span><span class="sxs-lookup"><span data-stu-id="819d6-130">For example, if you are copying data from an Azure blob storage to an on-premises file system, you create two linked services to link your on-premises file system and Azure storage account to your data factory.</span></span> <span data-ttu-id="819d6-131">Länkad tjänstegenskaper som är specifika för ett lokalt filsystem, se [länkade tjänstegenskaper](#linked-service-properties) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="819d6-131">For linked service properties that are specific to an on-premises file system, see [linked service properties](#linked-service-properties) section.</span></span>
3. <span data-ttu-id="819d6-132">Skapa **datauppsättningar** att representera inkommande och utgående data för kopieringen.</span><span class="sxs-lookup"><span data-stu-id="819d6-132">Create **datasets** to represent input and output data for the copy operation.</span></span> <span data-ttu-id="819d6-133">I det tidigare exemplet i det sista steget, skapar du en datauppsättning för att ange blob-behållaren och mappen som innehåller indata.</span><span class="sxs-lookup"><span data-stu-id="819d6-133">In the example mentioned in the last step, you create a dataset to specify the blob container and folder that contains the input data.</span></span> <span data-ttu-id="819d6-134">Och du skapar en annan dataset för att ange mappen och filnamnet (valfritt) i filsystemet.</span><span class="sxs-lookup"><span data-stu-id="819d6-134">And, you create another dataset to specify the folder and file name (optional) in your file system.</span></span> <span data-ttu-id="819d6-135">Egenskaper för datamängd som är specifika för lokalt filsystem, se [egenskaper för datamängd](#dataset-properties) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="819d6-135">For dataset properties that are specific to on-premises file system, see [dataset properties](#dataset-properties) section.</span></span>
4. <span data-ttu-id="819d6-136">Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata.</span><span class="sxs-lookup"><span data-stu-id="819d6-136">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> <span data-ttu-id="819d6-137">I exemplet ovan kan använda du BlobSource som en källa och FileSystemSink som en mottagare för kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="819d6-137">In the example mentioned earlier, you use BlobSource as a source and FileSystemSink as a sink for the copy activity.</span></span> <span data-ttu-id="819d6-138">På samma sätt om du kopierar från lokala filsystemet till Azure Blob Storage, använder du FileSystemSource och BlobSink i kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="819d6-138">Similarly, if you are copying from on-premises file system to Azure Blob Storage, you use FileSystemSource and BlobSink in the copy activity.</span></span> <span data-ttu-id="819d6-139">Kopiera Aktivitetsegenskaper som är specifika för lokalt filsystem, se [kopiera Aktivitetsegenskaper](#copy-activity-properties) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="819d6-139">For copy activity properties that are specific to on-premises file system, see [copy activity properties](#copy-activity-properties) section.</span></span> <span data-ttu-id="819d6-140">Mer information om hur du använder ett dataarkiv som en källa eller en mottagare klickar du på länken i föregående avsnitt för datalager.</span><span class="sxs-lookup"><span data-stu-id="819d6-140">For details on how to use a data store as a source or a sink, click the link in the previous section for your data store.</span></span>

<span data-ttu-id="819d6-141">När du använder guiden skapas JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och pipelinen) automatiskt för dig.</span><span class="sxs-lookup"><span data-stu-id="819d6-141">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="819d6-142">När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av JSON-format.</span><span class="sxs-lookup"><span data-stu-id="819d6-142">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="819d6-143">Exempel med JSON-definitioner för Data Factory-entiteter som används för att kopiera data till/från ett filsystem finns [JSON-exempel](#json-examples-for-copying-data-to-and-from-file-system) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="819d6-143">For samples with JSON definitions for Data Factory entities that are used to copy data to/from a file system, see [JSON examples](#json-examples-for-copying-data-to-and-from-file-system) section of this article.</span></span>

<span data-ttu-id="819d6-144">Följande avsnitt innehåller information om JSON-egenskaper som används för att definiera Data Factory entiteter för filsystemet:</span><span class="sxs-lookup"><span data-stu-id="819d6-144">The following sections provide details about JSON properties that are used to define Data Factory entities specific to file system:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="819d6-145">Länkad tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="819d6-145">Linked service properties</span></span>
<span data-ttu-id="819d6-146">Du kan länka ett lokalt filsystem till ett Azure data factory med den **lokala filserver** länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="819d6-146">You can link an on-premises file system to an Azure data factory with the **On-Premises File Server** linked service.</span></span> <span data-ttu-id="819d6-147">Följande tabell innehåller beskrivningar för JSON-element som är specifika för lokala filserver länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="819d6-147">The following table provides descriptions for JSON elements that are specific to the On-Premises File Server linked service.</span></span>

| <span data-ttu-id="819d6-148">Egenskap</span><span class="sxs-lookup"><span data-stu-id="819d6-148">Property</span></span> | <span data-ttu-id="819d6-149">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="819d6-149">Description</span></span> | <span data-ttu-id="819d6-150">Krävs</span><span class="sxs-lookup"><span data-stu-id="819d6-150">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="819d6-151">typ</span><span class="sxs-lookup"><span data-stu-id="819d6-151">type</span></span> |<span data-ttu-id="819d6-152">Se till att egenskapen type har angetts **OnPremisesFileServer**.</span><span class="sxs-lookup"><span data-stu-id="819d6-152">Ensure that the type property is set to **OnPremisesFileServer**.</span></span> |<span data-ttu-id="819d6-153">Ja</span><span class="sxs-lookup"><span data-stu-id="819d6-153">Yes</span></span> |
| <span data-ttu-id="819d6-154">värden</span><span class="sxs-lookup"><span data-stu-id="819d6-154">host</span></span> |<span data-ttu-id="819d6-155">Anger rotsökvägen för den mapp som du vill kopiera.</span><span class="sxs-lookup"><span data-stu-id="819d6-155">Specifies the root path of the folder that you want to copy.</span></span> <span data-ttu-id="819d6-156">Använda escape-tecknet ' \ ' för specialtecken i strängen.</span><span class="sxs-lookup"><span data-stu-id="819d6-156">Use the escape character ‘ \ ’ for special characters in the string.</span></span> <span data-ttu-id="819d6-157">Se [exempel länkad tjänst-och dataset](#sample-linked-service-and-dataset-definitions) exempel.</span><span class="sxs-lookup"><span data-stu-id="819d6-157">See [Sample linked service and dataset definitions](#sample-linked-service-and-dataset-definitions) for examples.</span></span> |<span data-ttu-id="819d6-158">Ja</span><span class="sxs-lookup"><span data-stu-id="819d6-158">Yes</span></span> |
| <span data-ttu-id="819d6-159">användar-ID</span><span class="sxs-lookup"><span data-stu-id="819d6-159">userid</span></span> |<span data-ttu-id="819d6-160">Ange ID för den användare som har åtkomst till servern.</span><span class="sxs-lookup"><span data-stu-id="819d6-160">Specify the ID of the user who has access to the server.</span></span> |<span data-ttu-id="819d6-161">Nej (om du väljer encryptedCredential)</span><span class="sxs-lookup"><span data-stu-id="819d6-161">No (if you choose encryptedCredential)</span></span> |
| <span data-ttu-id="819d6-162">lösenord</span><span class="sxs-lookup"><span data-stu-id="819d6-162">password</span></span> |<span data-ttu-id="819d6-163">Ange lösenord för användaren (användar-ID).</span><span class="sxs-lookup"><span data-stu-id="819d6-163">Specify the password for the user (userid).</span></span> |<span data-ttu-id="819d6-164">Nej (om du väljer encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="819d6-164">No (if you choose encryptedCredential</span></span> |
| <span data-ttu-id="819d6-165">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="819d6-165">encryptedCredential</span></span> |<span data-ttu-id="819d6-166">Ange de kryptera autentiseringsuppgifter som du kan få genom att köra cmdlet New-AzureRmDataFactoryEncryptValue.</span><span class="sxs-lookup"><span data-stu-id="819d6-166">Specify the encrypted credentials that you can get by running the New-AzureRmDataFactoryEncryptValue cmdlet.</span></span> |<span data-ttu-id="819d6-167">Nej (om du väljer att ange användarnamn och lösenord i klartext)</span><span class="sxs-lookup"><span data-stu-id="819d6-167">No (if you choose to specify userid and password in plain text)</span></span> |
| <span data-ttu-id="819d6-168">gatewayName</span><span class="sxs-lookup"><span data-stu-id="819d6-168">gatewayName</span></span> |<span data-ttu-id="819d6-169">Anger namnet på den gateway som Data Factory ska använda för att ansluta till den lokala filservern.</span><span class="sxs-lookup"><span data-stu-id="819d6-169">Specifies the name of the gateway that Data Factory should use to connect to the on-premises file server.</span></span> |<span data-ttu-id="819d6-170">Ja</span><span class="sxs-lookup"><span data-stu-id="819d6-170">Yes</span></span> |


### <a name="sample-linked-service-and-dataset-definitions"></a><span data-ttu-id="819d6-171">Exempel på länkad tjänst och dataset definitioner</span><span class="sxs-lookup"><span data-stu-id="819d6-171">Sample linked service and dataset definitions</span></span>
| <span data-ttu-id="819d6-172">Scenario</span><span class="sxs-lookup"><span data-stu-id="819d6-172">Scenario</span></span> | <span data-ttu-id="819d6-173">Värden i länkade tjänstdefinitionen</span><span class="sxs-lookup"><span data-stu-id="819d6-173">Host in linked service definition</span></span> | <span data-ttu-id="819d6-174">folderPath i datauppsättningsdefinitionen</span><span class="sxs-lookup"><span data-stu-id="819d6-174">folderPath in dataset definition</span></span> |
| --- | --- | --- |
| <span data-ttu-id="819d6-175">Lokal mapp på Data Management Gateway-datorn:</span><span class="sxs-lookup"><span data-stu-id="819d6-175">Local folder on Data Management Gateway machine:</span></span> <br/><br/><span data-ttu-id="819d6-176">Exempel: D:\\ \* eller D:\folder\subfolder\\*</span><span class="sxs-lookup"><span data-stu-id="819d6-176">Examples: D:\\\* or D:\folder\subfolder\\*</span></span> |<span data-ttu-id="819d6-177">D:\\ \\ (för Data Management Gateway 2.0 och senare)</span><span class="sxs-lookup"><span data-stu-id="819d6-177">D:\\\\ (for Data Management Gateway 2.0 and later versions)</span></span> <br/><br/> <span data-ttu-id="819d6-178">localhost (för tidigare versioner än Data Management Gateway 2.0)</span><span class="sxs-lookup"><span data-stu-id="819d6-178">localhost (for earlier versions than Data Management Gateway 2.0)</span></span> |<span data-ttu-id="819d6-179">. \\ \\ eller mappen\\\\undermapp (för Data Management Gateway 2.0 och senare)</span><span class="sxs-lookup"><span data-stu-id="819d6-179">.\\\\ or folder\\\\subfolder (for Data Management Gateway 2.0 and later versions)</span></span> <br/><br/><span data-ttu-id="819d6-180">D:\\ \\ eller D:\\\\mappen\\\\undermapp (för gateway-versionen nedan 2.0)</span><span class="sxs-lookup"><span data-stu-id="819d6-180">D:\\\\ or D:\\\\folder\\\\subfolder (for gateway version below 2.0)</span></span> |
| <span data-ttu-id="819d6-181">Delad fjärrmapp:</span><span class="sxs-lookup"><span data-stu-id="819d6-181">Remote shared folder:</span></span> <br/><br/><span data-ttu-id="819d6-182">Exempel: \\ \\minserver\\dela\\ \* eller \\ \\minserver\\dela\\mappen\\undermapp\\*</span><span class="sxs-lookup"><span data-stu-id="819d6-182">Examples: \\\\myserver\\share\\\* or \\\\myserver\\share\\folder\\subfolder\\*</span></span> |<span data-ttu-id="819d6-183">\\\\\\\\minserver\\\\dela</span><span class="sxs-lookup"><span data-stu-id="819d6-183">\\\\\\\\myserver\\\\share</span></span> |<span data-ttu-id="819d6-184">. \\ \\ eller mappen\\\\undermapp</span><span class="sxs-lookup"><span data-stu-id="819d6-184">.\\\\ or folder\\\\subfolder</span></span> |


### <a name="example-using-username-and-password-in-plain-text"></a><span data-ttu-id="819d6-185">Exempel: Med användarnamn och lösenord i klartext</span><span class="sxs-lookup"><span data-stu-id="819d6-185">Example: Using username and password in plain text</span></span>

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

### <a name="example-using-encryptedcredential"></a><span data-ttu-id="819d6-186">Exempel: Använda encryptedcredential</span><span class="sxs-lookup"><span data-stu-id="819d6-186">Example: Using encryptedcredential</span></span>

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

## <a name="dataset-properties"></a><span data-ttu-id="819d6-187">Egenskaper för datamängd</span><span class="sxs-lookup"><span data-stu-id="819d6-187">Dataset properties</span></span>
<span data-ttu-id="819d6-188">En fullständig lista över avsnitt och egenskaper som är tillgängliga för att definiera datauppsättningar finns [skapa datauppsättningar](data-factory-create-datasets.md).</span><span class="sxs-lookup"><span data-stu-id="819d6-188">For a full list of sections and properties that are available for defining datasets, see [Creating datasets](data-factory-create-datasets.md).</span></span> <span data-ttu-id="819d6-189">Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av datauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="819d6-189">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types.</span></span>

<span data-ttu-id="819d6-190">Avsnittet typeProperties är olika för varje typ av datauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="819d6-190">The typeProperties section is different for each type of dataset.</span></span> <span data-ttu-id="819d6-191">Den ger information, till exempel plats och formatet på data i datalagret.</span><span class="sxs-lookup"><span data-stu-id="819d6-191">It provides information such as the location and format of the data in the data store.</span></span> <span data-ttu-id="819d6-192">TypeProperties avsnittet för datauppsättningen av typen **filresursen** har följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="819d6-192">The typeProperties section for the dataset of type **FileShare** has the following properties:</span></span>

| <span data-ttu-id="819d6-193">Egenskap</span><span class="sxs-lookup"><span data-stu-id="819d6-193">Property</span></span> | <span data-ttu-id="819d6-194">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="819d6-194">Description</span></span> | <span data-ttu-id="819d6-195">Krävs</span><span class="sxs-lookup"><span data-stu-id="819d6-195">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="819d6-196">folderPath</span><span class="sxs-lookup"><span data-stu-id="819d6-196">folderPath</span></span> |<span data-ttu-id="819d6-197">Anger undersökvägen till mappen.</span><span class="sxs-lookup"><span data-stu-id="819d6-197">Specifies the subpath to the folder.</span></span> <span data-ttu-id="819d6-198">Använda escape-tecknet ' \' för specialtecken i strängen.</span><span class="sxs-lookup"><span data-stu-id="819d6-198">Use the escape character ‘\’ for special characters in the string.</span></span> <span data-ttu-id="819d6-199">Se [exempel länkad tjänst-och dataset](#sample-linked-service-and-dataset-definitions) exempel.</span><span class="sxs-lookup"><span data-stu-id="819d6-199">See [Sample linked service and dataset definitions](#sample-linked-service-and-dataset-definitions) for examples.</span></span><br/><br/><span data-ttu-id="819d6-200">Du kan kombinera den här egenskapen med **partitionBy** ha mappen sökvägar baserat på sektorn börja/sluta datum gånger.</span><span class="sxs-lookup"><span data-stu-id="819d6-200">You can combine this property with **partitionBy** to have folder paths based on slice start/end date-times.</span></span> |<span data-ttu-id="819d6-201">Ja</span><span class="sxs-lookup"><span data-stu-id="819d6-201">Yes</span></span> |
| <span data-ttu-id="819d6-202">fileName</span><span class="sxs-lookup"><span data-stu-id="819d6-202">fileName</span></span> |<span data-ttu-id="819d6-203">Ange namnet på filen i den **folderPath** om du vill att referera till en viss fil i mappen.</span><span class="sxs-lookup"><span data-stu-id="819d6-203">Specify the name of the file in the **folderPath** if you want the table to refer to a specific file in the folder.</span></span> <span data-ttu-id="819d6-204">Om du inte anger något värde för den här egenskapen tabellen pekar på alla filer i mappen.</span><span class="sxs-lookup"><span data-stu-id="819d6-204">If you do not specify any value for this property, the table points to all files in the folder.</span></span><br/><br/><span data-ttu-id="819d6-205">När **fileName** har inte angetts för en datamängd för utdata och **preserveHierarchy** har inte angetts i aktiviteten sink namnet på den genererade filen är i följande format:</span><span class="sxs-lookup"><span data-stu-id="819d6-205">When **fileName** is not specified for an output dataset and **preserveHierarchy** is not specified in activity sink, the name of the generated file is in the following format:</span></span> <br/><br/><span data-ttu-id="819d6-206">`Data.<Guid>.txt`(Exempel: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt)</span><span class="sxs-lookup"><span data-stu-id="819d6-206">`Data.<Guid>.txt` (Example: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt)</span></span> |<span data-ttu-id="819d6-207">Nej</span><span class="sxs-lookup"><span data-stu-id="819d6-207">No</span></span> |
| <span data-ttu-id="819d6-208">fileFilter</span><span class="sxs-lookup"><span data-stu-id="819d6-208">fileFilter</span></span> |<span data-ttu-id="819d6-209">Ange ett filter som används för att välja en delmängd av filer i mappsökvägen i stället för alla filer.</span><span class="sxs-lookup"><span data-stu-id="819d6-209">Specify a filter to be used to select a subset of files in the folderPath rather than all files.</span></span> <br/><br/><span data-ttu-id="819d6-210">Tillåtna värden är: `*` (flera tecken) och `?` (valfritt tecken).</span><span class="sxs-lookup"><span data-stu-id="819d6-210">Allowed values are: `*` (multiple characters) and `?` (single character).</span></span><br/><br/><span data-ttu-id="819d6-211">Exempel 1: ”fileFilter” ”: * .log”</span><span class="sxs-lookup"><span data-stu-id="819d6-211">Example 1: "fileFilter": "*.log"</span></span><br/><span data-ttu-id="819d6-212">Exempel 2: ”fileFilter”: 2014 - 1-?. txt ”</span><span class="sxs-lookup"><span data-stu-id="819d6-212">Example 2: "fileFilter": 2014-1-?.txt"</span></span><br/><br/><span data-ttu-id="819d6-213">Observera att fileFilter gäller för en inkommande filresursen datauppsättning.</span><span class="sxs-lookup"><span data-stu-id="819d6-213">Note that fileFilter is applicable for an input FileShare dataset.</span></span> |<span data-ttu-id="819d6-214">Nej</span><span class="sxs-lookup"><span data-stu-id="819d6-214">No</span></span> |
| <span data-ttu-id="819d6-215">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="819d6-215">partitionedBy</span></span> |<span data-ttu-id="819d6-216">Du kan använda partitionedBy för att ange en dynamisk folderPath/filnamn för time series-data.</span><span class="sxs-lookup"><span data-stu-id="819d6-216">You can use partitionedBy to specify a dynamic folderPath/fileName for time-series data.</span></span> <span data-ttu-id="819d6-217">Ett exempel är folderPath parametriserade varje timme av data.</span><span class="sxs-lookup"><span data-stu-id="819d6-217">An example is folderPath parameterized for every hour of data.</span></span> |<span data-ttu-id="819d6-218">Nej</span><span class="sxs-lookup"><span data-stu-id="819d6-218">No</span></span> |
| <span data-ttu-id="819d6-219">Format</span><span class="sxs-lookup"><span data-stu-id="819d6-219">format</span></span> | <span data-ttu-id="819d6-220">Följande format stöds: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="819d6-220">The following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="819d6-221">Ange den **typen** egenskap under format till ett av dessa värden.</span><span class="sxs-lookup"><span data-stu-id="819d6-221">Set the **type** property under format to one of these values.</span></span> <span data-ttu-id="819d6-222">Mer information finns i [textformat](data-factory-supported-file-and-compression-formats.md#text-format), [Json-Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-formatet](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), och [parkettgolv Format](data-factory-supported-file-and-compression-formats.md#parquet-format) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="819d6-222">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="819d6-223">Om du vill **kopiera filer som-är** mellan filbaserade butiker (binär kopia), hoppa över avsnittet format i både inkommande och utgående dataset-definitioner.</span><span class="sxs-lookup"><span data-stu-id="819d6-223">If you want to **copy files as-is** between file-based stores (binary copy), skip the format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="819d6-224">Nej</span><span class="sxs-lookup"><span data-stu-id="819d6-224">No</span></span> |
| <span data-ttu-id="819d6-225">Komprimering</span><span class="sxs-lookup"><span data-stu-id="819d6-225">compression</span></span> | <span data-ttu-id="819d6-226">Ange typ och kompression för data.</span><span class="sxs-lookup"><span data-stu-id="819d6-226">Specify the type and level of compression for the data.</span></span> <span data-ttu-id="819d6-227">Typer som stöds är: **GZip**, **Deflate**, **BZip2**, och **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="819d6-227">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="819d6-228">Nivåer som stöds är: **Optimal** och **snabbast**.</span><span class="sxs-lookup"><span data-stu-id="819d6-228">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="819d6-229">Se [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="819d6-229">see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="819d6-230">Nej</span><span class="sxs-lookup"><span data-stu-id="819d6-230">No</span></span> |

> [!NOTE]
> <span data-ttu-id="819d6-231">Du kan inte använda filnamn och fileFilter samtidigt.</span><span class="sxs-lookup"><span data-stu-id="819d6-231">You cannot use fileName and fileFilter simultaneously.</span></span>

### <a name="using-partitionedby-property"></a><span data-ttu-id="819d6-232">Med hjälp av partitionedBy egenskap</span><span class="sxs-lookup"><span data-stu-id="819d6-232">Using partitionedBy property</span></span>
<span data-ttu-id="819d6-233">Som nämnts ovan, du kan ange en dynamisk folderPath och ett filnamn för tid series-data med den **partitionedBy** egenskapen [Data Factory-funktioner och systemvariablerna](data-factory-functions-variables.md).</span><span class="sxs-lookup"><span data-stu-id="819d6-233">As mentioned in the previous section, you can specify a dynamic folderPath and filename for time series data with the **partitionedBy** property, [Data Factory functions, and the system variables](data-factory-functions-variables.md).</span></span>

<span data-ttu-id="819d6-234">Mer information om tidsserier datauppsättningar, schemaläggning och segment finns [skapa datauppsättningar](data-factory-create-datasets.md), [schemaläggning och körning](data-factory-scheduling-and-execution.md), och [skapar pipelines](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="819d6-234">To understand more details on time-series datasets, scheduling, and slices, see [Creating datasets](data-factory-create-datasets.md), [Scheduling and execution](data-factory-scheduling-and-execution.md), and [Creating pipelines](data-factory-create-pipelines.md).</span></span>

#### <a name="sample-1"></a><span data-ttu-id="819d6-235">Exempel 1:</span><span class="sxs-lookup"><span data-stu-id="819d6-235">Sample 1:</span></span>

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```

<span data-ttu-id="819d6-236">I det här exemplet {segment} ersätts med värdet från Data Factory systemvariabeln SliceStart i formatet (YYYYMMDDHH).</span><span class="sxs-lookup"><span data-stu-id="819d6-236">In this example, {Slice} is replaced with the value of the Data Factory system variable SliceStart in the format (YYYYMMDDHH).</span></span> <span data-ttu-id="819d6-237">SliceStart refererar till starttid av sektorn.</span><span class="sxs-lookup"><span data-stu-id="819d6-237">SliceStart refers to start time of the slice.</span></span> <span data-ttu-id="819d6-238">FolderPath är olika för varje segment.</span><span class="sxs-lookup"><span data-stu-id="819d6-238">The folderPath is different for each slice.</span></span> <span data-ttu-id="819d6-239">Till exempel: 2014100103-wikidatagateway/wikisampledataout eller 2014100104-wikidatagateway/wikisampledataout.</span><span class="sxs-lookup"><span data-stu-id="819d6-239">For example: wikidatagateway/wikisampledataout/2014100103 or wikidatagateway/wikisampledataout/2014100104.</span></span>

#### <a name="sample-2"></a><span data-ttu-id="819d6-240">Exempel 2:</span><span class="sxs-lookup"><span data-stu-id="819d6-240">Sample 2:</span></span>

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

<span data-ttu-id="819d6-241">I det här exemplet extraheras år, månad, dag och tid för SliceStart i separata variabler med egenskaperna folderPath och filnamn.</span><span class="sxs-lookup"><span data-stu-id="819d6-241">In this example, year, month, day, and time of SliceStart are extracted into separate variables that the folderPath and fileName properties use.</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="819d6-242">Kopiera egenskaper för aktivitet</span><span class="sxs-lookup"><span data-stu-id="819d6-242">Copy activity properties</span></span>
<span data-ttu-id="819d6-243">En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns i [skapar Pipelines](data-factory-create-pipelines.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="819d6-243">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="819d6-244">Egenskaper som namn, beskrivning, indata och utdata-datauppsättningar och -principer är tillgängliga för alla typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="819d6-244">Properties such as name, description, input and output datasets, and policies are available for all types of activities.</span></span> <span data-ttu-id="819d6-245">Medan egenskaper som är tillgängliga i den **typeProperties** avsnitt i aktiviteten varierar med varje aktivitetstyp.</span><span class="sxs-lookup"><span data-stu-id="819d6-245">Whereas, properties available in the **typeProperties** section of the activity vary with each activity type.</span></span>

<span data-ttu-id="819d6-246">För Kopieringsaktivitet kan variera de beroende på vilka typer av datakällor och sänkor.</span><span class="sxs-lookup"><span data-stu-id="819d6-246">For Copy activity, they vary depending on the types of sources and sinks.</span></span> <span data-ttu-id="819d6-247">Om du flyttar data från ett lokalt filsystem du ange typen av datakälla i kopieringsaktiviteten till **FileSystemSource**.</span><span class="sxs-lookup"><span data-stu-id="819d6-247">If you are moving data from an on-premises file system, you set the source type in the copy activity to **FileSystemSource**.</span></span> <span data-ttu-id="819d6-248">På samma sätt om du flyttar data till ett lokalt filsystem du anger sink i kopieringsaktiviteten till **FileSystemSink**.</span><span class="sxs-lookup"><span data-stu-id="819d6-248">Similarly, if you are moving data to an on-premises file system, you set the sink type in the copy activity to **FileSystemSink**.</span></span> <span data-ttu-id="819d6-249">Det här avsnittet innehåller en lista över egenskaper som stöds av FileSystemSource och FileSystemSink.</span><span class="sxs-lookup"><span data-stu-id="819d6-249">This section provides a list of properties supported by FileSystemSource and FileSystemSink.</span></span>

<span data-ttu-id="819d6-250">**FileSystemSource** stöder följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="819d6-250">**FileSystemSource** supports the following properties:</span></span>

| <span data-ttu-id="819d6-251">Egenskap</span><span class="sxs-lookup"><span data-stu-id="819d6-251">Property</span></span> | <span data-ttu-id="819d6-252">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="819d6-252">Description</span></span> | <span data-ttu-id="819d6-253">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="819d6-253">Allowed values</span></span> | <span data-ttu-id="819d6-254">Krävs</span><span class="sxs-lookup"><span data-stu-id="819d6-254">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="819d6-255">Rekursiva</span><span class="sxs-lookup"><span data-stu-id="819d6-255">recursive</span></span> |<span data-ttu-id="819d6-256">Anger om data läses rekursivt från undermapparna eller endast från den angivna mappen.</span><span class="sxs-lookup"><span data-stu-id="819d6-256">Indicates whether the data is read recursively from the subfolders or only from the specified folder.</span></span> |<span data-ttu-id="819d6-257">SANT, FALSKT (standard)</span><span class="sxs-lookup"><span data-stu-id="819d6-257">True, False (default)</span></span> |<span data-ttu-id="819d6-258">Nej</span><span class="sxs-lookup"><span data-stu-id="819d6-258">No</span></span> |

<span data-ttu-id="819d6-259">**FileSystemSink** stöder följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="819d6-259">**FileSystemSink** supports the following properties:</span></span>

| <span data-ttu-id="819d6-260">Egenskap</span><span class="sxs-lookup"><span data-stu-id="819d6-260">Property</span></span> | <span data-ttu-id="819d6-261">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="819d6-261">Description</span></span> | <span data-ttu-id="819d6-262">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="819d6-262">Allowed values</span></span> | <span data-ttu-id="819d6-263">Krävs</span><span class="sxs-lookup"><span data-stu-id="819d6-263">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="819d6-264">copyBehavior</span><span class="sxs-lookup"><span data-stu-id="819d6-264">copyBehavior</span></span> |<span data-ttu-id="819d6-265">Definierar beteendet kopia när källan är BlobSource eller filsystem.</span><span class="sxs-lookup"><span data-stu-id="819d6-265">Defines the copy behavior when the source is BlobSource or FileSystem.</span></span> |<span data-ttu-id="819d6-266">**PreserveHierarchy:** bevarar fil hierarkin i målmappen.</span><span class="sxs-lookup"><span data-stu-id="819d6-266">**PreserveHierarchy:** Preserves the file hierarchy in the target folder.</span></span> <span data-ttu-id="819d6-267">Den relativa sökvägen för källfilen för målmappen är samma som den relativa sökvägen för filen till målmappen.</span><span class="sxs-lookup"><span data-stu-id="819d6-267">That is, the relative path of the source file to the source folder is the same as the relative path of the target file to the target folder.</span></span><br/><br/><span data-ttu-id="819d6-268">**FlattenHierarchy:** alla filer från källmappen skapas i den första nivån i målmappen.</span><span class="sxs-lookup"><span data-stu-id="819d6-268">**FlattenHierarchy:** All files from the source folder are created in the first level of target folder.</span></span> <span data-ttu-id="819d6-269">Mål-filer som skapas med ett namn som genererats automatiskt.</span><span class="sxs-lookup"><span data-stu-id="819d6-269">The target files are created with an autogenerated name.</span></span><br/><br/><span data-ttu-id="819d6-270">**MergeFiles:** sammanfogar alla filer från källmappen till en fil.</span><span class="sxs-lookup"><span data-stu-id="819d6-270">**MergeFiles:** Merges all files from the source folder to one file.</span></span> <span data-ttu-id="819d6-271">Om name-blob filnamn har angetts är kopplade filnamnet det angivna namnet.</span><span class="sxs-lookup"><span data-stu-id="819d6-271">If the file name/blob name is specified, the merged file name is the specified name.</span></span> <span data-ttu-id="819d6-272">Annars är ett automatiskt genererat namn.</span><span class="sxs-lookup"><span data-stu-id="819d6-272">Otherwise, it is an auto-generated file name.</span></span> |<span data-ttu-id="819d6-273">Nej</span><span class="sxs-lookup"><span data-stu-id="819d6-273">No</span></span> |

### <a name="recursive-and-copybehavior-examples"></a><span data-ttu-id="819d6-274">rekursiva och copyBehavior exempel</span><span class="sxs-lookup"><span data-stu-id="819d6-274">recursive and copyBehavior examples</span></span>
<span data-ttu-id="819d6-275">Det här avsnittet beskriver resultatet av kopieringen för olika kombinationer av värden för egenskaperna rekursiv och copyBehavior.</span><span class="sxs-lookup"><span data-stu-id="819d6-275">This section describes the resulting behavior of the Copy operation for different combinations of values for the recursive and copyBehavior properties.</span></span>

| <span data-ttu-id="819d6-276">rekursiva värde</span><span class="sxs-lookup"><span data-stu-id="819d6-276">recursive value</span></span> | <span data-ttu-id="819d6-277">copyBehavior värde</span><span class="sxs-lookup"><span data-stu-id="819d6-277">copyBehavior value</span></span> | <span data-ttu-id="819d6-278">Resultatet</span><span class="sxs-lookup"><span data-stu-id="819d6-278">Resulting behavior</span></span> |
| --- | --- | --- |
| <span data-ttu-id="819d6-279">SANT</span><span class="sxs-lookup"><span data-stu-id="819d6-279">true</span></span> |<span data-ttu-id="819d6-280">preserveHierarchy</span><span class="sxs-lookup"><span data-stu-id="819d6-280">preserveHierarchy</span></span> |<span data-ttu-id="819d6-281">För en källmapp Mapp1 med följande struktur</span><span class="sxs-lookup"><span data-stu-id="819d6-281">For a source folder Folder1 with the following structure,</span></span><br/><br/><span data-ttu-id="819d6-282">Mapp1</span><span class="sxs-lookup"><span data-stu-id="819d6-282">Folder1</span></span><br/><span data-ttu-id="819d6-283">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="819d6-283">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="819d6-284">&nbsp;&nbsp;&nbsp;&nbsp;Fil2</span><span class="sxs-lookup"><span data-stu-id="819d6-284">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="819d6-285">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="819d6-285">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="819d6-286">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3</span><span class="sxs-lookup"><span data-stu-id="819d6-286">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="819d6-287">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="819d6-287">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="819d6-288">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="819d6-288">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="819d6-289">målmappen Mapp1 skapas med samma struktur som källa:</span><span class="sxs-lookup"><span data-stu-id="819d6-289">the target folder Folder1 is created with the same structure as the source:</span></span><br/><br/><span data-ttu-id="819d6-290">Mapp1</span><span class="sxs-lookup"><span data-stu-id="819d6-290">Folder1</span></span><br/><span data-ttu-id="819d6-291">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="819d6-291">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="819d6-292">&nbsp;&nbsp;&nbsp;&nbsp;Fil2</span><span class="sxs-lookup"><span data-stu-id="819d6-292">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="819d6-293">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="819d6-293">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="819d6-294">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3</span><span class="sxs-lookup"><span data-stu-id="819d6-294">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="819d6-295">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="819d6-295">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="819d6-296">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="819d6-296">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span> |
| <span data-ttu-id="819d6-297">SANT</span><span class="sxs-lookup"><span data-stu-id="819d6-297">true</span></span> |<span data-ttu-id="819d6-298">flattenHierarchy</span><span class="sxs-lookup"><span data-stu-id="819d6-298">flattenHierarchy</span></span> |<span data-ttu-id="819d6-299">För en källmapp Mapp1 med följande struktur</span><span class="sxs-lookup"><span data-stu-id="819d6-299">For a source folder Folder1 with the following structure,</span></span><br/><br/><span data-ttu-id="819d6-300">Mapp1</span><span class="sxs-lookup"><span data-stu-id="819d6-300">Folder1</span></span><br/><span data-ttu-id="819d6-301">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="819d6-301">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="819d6-302">&nbsp;&nbsp;&nbsp;&nbsp;Fil2</span><span class="sxs-lookup"><span data-stu-id="819d6-302">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="819d6-303">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="819d6-303">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="819d6-304">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3</span><span class="sxs-lookup"><span data-stu-id="819d6-304">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="819d6-305">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="819d6-305">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="819d6-306">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="819d6-306">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="819d6-307">målet Mapp1 skapas med följande struktur:</span><span class="sxs-lookup"><span data-stu-id="819d6-307">the target Folder1 is created with the following structure:</span></span> <br/><br/><span data-ttu-id="819d6-308">Mapp1</span><span class="sxs-lookup"><span data-stu-id="819d6-308">Folder1</span></span><br/><span data-ttu-id="819d6-309">&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererade namnet på File1</span><span class="sxs-lookup"><span data-stu-id="819d6-309">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File1</span></span><br/><span data-ttu-id="819d6-310">&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererat namn för fil2</span><span class="sxs-lookup"><span data-stu-id="819d6-310">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File2</span></span><br/><span data-ttu-id="819d6-311">&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererat namn för fil3</span><span class="sxs-lookup"><span data-stu-id="819d6-311">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File3</span></span><br/><span data-ttu-id="819d6-312">&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererat namn för File4</span><span class="sxs-lookup"><span data-stu-id="819d6-312">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File4</span></span><br/><span data-ttu-id="819d6-313">&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererat namn för File5</span><span class="sxs-lookup"><span data-stu-id="819d6-313">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File5</span></span> |
| <span data-ttu-id="819d6-314">SANT</span><span class="sxs-lookup"><span data-stu-id="819d6-314">true</span></span> |<span data-ttu-id="819d6-315">mergeFiles</span><span class="sxs-lookup"><span data-stu-id="819d6-315">mergeFiles</span></span> |<span data-ttu-id="819d6-316">För en källmapp Mapp1 med följande struktur</span><span class="sxs-lookup"><span data-stu-id="819d6-316">For a source folder Folder1 with the following structure,</span></span><br/><br/><span data-ttu-id="819d6-317">Mapp1</span><span class="sxs-lookup"><span data-stu-id="819d6-317">Folder1</span></span><br/><span data-ttu-id="819d6-318">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="819d6-318">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="819d6-319">&nbsp;&nbsp;&nbsp;&nbsp;Fil2</span><span class="sxs-lookup"><span data-stu-id="819d6-319">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="819d6-320">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="819d6-320">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="819d6-321">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3</span><span class="sxs-lookup"><span data-stu-id="819d6-321">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="819d6-322">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="819d6-322">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="819d6-323">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="819d6-323">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="819d6-324">målet Mapp1 skapas med följande struktur:</span><span class="sxs-lookup"><span data-stu-id="819d6-324">the target Folder1 is created with the following structure:</span></span> <br/><br/><span data-ttu-id="819d6-325">Mapp1</span><span class="sxs-lookup"><span data-stu-id="819d6-325">Folder1</span></span><br/><span data-ttu-id="819d6-326">&nbsp;&nbsp;&nbsp;&nbsp;Fil1 + fil2 + fil3 + File4 + filen 5 innehållet slås samman till en fil med ett automatiskt genererat namn.</span><span class="sxs-lookup"><span data-stu-id="819d6-326">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 + File3 + File4 + File 5 contents are merged into one file with an auto-generated file name.</span></span> |
| <span data-ttu-id="819d6-327">FALSKT</span><span class="sxs-lookup"><span data-stu-id="819d6-327">false</span></span> |<span data-ttu-id="819d6-328">preserveHierarchy</span><span class="sxs-lookup"><span data-stu-id="819d6-328">preserveHierarchy</span></span> |<span data-ttu-id="819d6-329">För en källmapp Mapp1 med följande struktur</span><span class="sxs-lookup"><span data-stu-id="819d6-329">For a source folder Folder1 with the following structure,</span></span><br/><br/><span data-ttu-id="819d6-330">Mapp1</span><span class="sxs-lookup"><span data-stu-id="819d6-330">Folder1</span></span><br/><span data-ttu-id="819d6-331">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="819d6-331">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="819d6-332">&nbsp;&nbsp;&nbsp;&nbsp;Fil2</span><span class="sxs-lookup"><span data-stu-id="819d6-332">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="819d6-333">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="819d6-333">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="819d6-334">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3</span><span class="sxs-lookup"><span data-stu-id="819d6-334">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="819d6-335">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="819d6-335">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="819d6-336">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="819d6-336">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="819d6-337">målmappen Mapp1 skapas med följande struktur:</span><span class="sxs-lookup"><span data-stu-id="819d6-337">the target folder Folder1 is created with the following structure:</span></span><br/><br/><span data-ttu-id="819d6-338">Mapp1</span><span class="sxs-lookup"><span data-stu-id="819d6-338">Folder1</span></span><br/><span data-ttu-id="819d6-339">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="819d6-339">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="819d6-340">&nbsp;&nbsp;&nbsp;&nbsp;Fil2</span><span class="sxs-lookup"><span data-stu-id="819d6-340">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><br/><span data-ttu-id="819d6-341">Subfolder1 med fil3, File4 och File5 hämtas inte.</span><span class="sxs-lookup"><span data-stu-id="819d6-341">Subfolder1 with File3, File4, and File5 is not picked up.</span></span> |
| <span data-ttu-id="819d6-342">FALSKT</span><span class="sxs-lookup"><span data-stu-id="819d6-342">false</span></span> |<span data-ttu-id="819d6-343">flattenHierarchy</span><span class="sxs-lookup"><span data-stu-id="819d6-343">flattenHierarchy</span></span> |<span data-ttu-id="819d6-344">För en källmapp Mapp1 med följande struktur</span><span class="sxs-lookup"><span data-stu-id="819d6-344">For a source folder Folder1 with the following structure,</span></span><br/><br/><span data-ttu-id="819d6-345">Mapp1</span><span class="sxs-lookup"><span data-stu-id="819d6-345">Folder1</span></span><br/><span data-ttu-id="819d6-346">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="819d6-346">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="819d6-347">&nbsp;&nbsp;&nbsp;&nbsp;Fil2</span><span class="sxs-lookup"><span data-stu-id="819d6-347">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="819d6-348">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="819d6-348">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="819d6-349">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3</span><span class="sxs-lookup"><span data-stu-id="819d6-349">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="819d6-350">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="819d6-350">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="819d6-351">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="819d6-351">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="819d6-352">målmappen Mapp1 skapas med följande struktur:</span><span class="sxs-lookup"><span data-stu-id="819d6-352">the target folder Folder1 is created with the following structure:</span></span><br/><br/><span data-ttu-id="819d6-353">Mapp1</span><span class="sxs-lookup"><span data-stu-id="819d6-353">Folder1</span></span><br/><span data-ttu-id="819d6-354">&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererade namnet på File1</span><span class="sxs-lookup"><span data-stu-id="819d6-354">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File1</span></span><br/><span data-ttu-id="819d6-355">&nbsp;&nbsp;&nbsp;&nbsp;automatiskt genererat namn för fil2</span><span class="sxs-lookup"><span data-stu-id="819d6-355">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File2</span></span><br/><br/><span data-ttu-id="819d6-356">Subfolder1 med fil3, File4 och File5 hämtas inte.</span><span class="sxs-lookup"><span data-stu-id="819d6-356">Subfolder1 with File3, File4, and File5 is not picked up.</span></span> |
| <span data-ttu-id="819d6-357">FALSKT</span><span class="sxs-lookup"><span data-stu-id="819d6-357">false</span></span> |<span data-ttu-id="819d6-358">mergeFiles</span><span class="sxs-lookup"><span data-stu-id="819d6-358">mergeFiles</span></span> |<span data-ttu-id="819d6-359">För en källmapp Mapp1 med följande struktur</span><span class="sxs-lookup"><span data-stu-id="819d6-359">For a source folder Folder1 with the following structure,</span></span><br/><br/><span data-ttu-id="819d6-360">Mapp1</span><span class="sxs-lookup"><span data-stu-id="819d6-360">Folder1</span></span><br/><span data-ttu-id="819d6-361">&nbsp;&nbsp;&nbsp;&nbsp;File1</span><span class="sxs-lookup"><span data-stu-id="819d6-361">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="819d6-362">&nbsp;&nbsp;&nbsp;&nbsp;Fil2</span><span class="sxs-lookup"><span data-stu-id="819d6-362">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="819d6-363">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="819d6-363">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="819d6-364">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fil3</span><span class="sxs-lookup"><span data-stu-id="819d6-364">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="819d6-365">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="819d6-365">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="819d6-366">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="819d6-366">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="819d6-367">målmappen Mapp1 skapas med följande struktur:</span><span class="sxs-lookup"><span data-stu-id="819d6-367">the target folder Folder1 is created with the following structure:</span></span><br/><br/><span data-ttu-id="819d6-368">Mapp1</span><span class="sxs-lookup"><span data-stu-id="819d6-368">Folder1</span></span><br/><span data-ttu-id="819d6-369">&nbsp;&nbsp;&nbsp;&nbsp;Fil1 + fil2 innehållet slås samman till en fil med ett automatiskt genererat namn.</span><span class="sxs-lookup"><span data-stu-id="819d6-369">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 contents are merged into one file with an auto-generated file name.</span></span><br/><span data-ttu-id="819d6-370">&nbsp;&nbsp;&nbsp;&nbsp;Automatiskt genererade namnet på File1</span><span class="sxs-lookup"><span data-stu-id="819d6-370">&nbsp;&nbsp;&nbsp;&nbsp;Auto-generated name for File1</span></span><br/><br/><span data-ttu-id="819d6-371">Subfolder1 med fil3, File4 och File5 hämtas inte.</span><span class="sxs-lookup"><span data-stu-id="819d6-371">Subfolder1 with File3, File4, and File5 is not picked up.</span></span> |

## <a name="supported-file-and-compression-formats"></a><span data-ttu-id="819d6-372">Fil- och komprimering format som stöds</span><span class="sxs-lookup"><span data-stu-id="819d6-372">Supported file and compression formats</span></span>
<span data-ttu-id="819d6-373">Se [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md) artikeln för information.</span><span class="sxs-lookup"><span data-stu-id="819d6-373">See [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md) article on details.</span></span>

## <a name="json-examples-for-copying-data-to-and-from-file-system"></a><span data-ttu-id="819d6-374">JSON-exempel för att kopiera data till och från filsystemet</span><span class="sxs-lookup"><span data-stu-id="819d6-374">JSON examples for copying data to and from file system</span></span>
<span data-ttu-id="819d6-375">Följande exempel ger exempel JSON definitioner som du kan använda för att skapa en pipeline med hjälp av den [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="819d6-375">The following examples provide sample JSON definitions that you can use to create a pipeline by using the [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="819d6-376">De visar hur du kopierar data till och från ett lokalt filsystem och Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="819d6-376">They show how to copy data to and from an on-premises file system and Azure Blob storage.</span></span> <span data-ttu-id="819d6-377">Du kan dock kopiera data *direkt* från någon av källorna till någon av sänkor som anges i [källor och sänkor stöds](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hjälp av Kopieringsaktiviteten i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="819d6-377">However, you can copy data *directly* from any of the sources to any of the sinks listed in [Supported sources and sinks](data-factory-data-movement-activities.md#supported-data-stores-and-formats) by using Copy Activity in Azure Data Factory.</span></span>

### <a name="example-copy-data-from-an-on-premises-file-system-to-azure-blob-storage"></a><span data-ttu-id="819d6-378">Exempel: Kopiera data från ett lokalt filsystem till Azure Blob storage</span><span class="sxs-lookup"><span data-stu-id="819d6-378">Example: Copy data from an on-premises file system to Azure Blob storage</span></span>
<span data-ttu-id="819d6-379">Det här exemplet visas hur du kopierar data från ett lokalt filsystem till Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="819d6-379">This sample shows how to copy data from an on-premises file system to Azure Blob storage.</span></span> <span data-ttu-id="819d6-380">Exemplet har följande Data Factory-enheter:</span><span class="sxs-lookup"><span data-stu-id="819d6-380">The sample has the following Data Factory entities:</span></span>

* <span data-ttu-id="819d6-381">En länkad tjänst av typen [OnPremisesFileServer](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="819d6-381">A linked service of type [OnPremisesFileServer](#linked-service-properties).</span></span>
* <span data-ttu-id="819d6-382">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="819d6-382">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
* <span data-ttu-id="819d6-383">Indata [dataset](data-factory-create-datasets.md) av typen [filresursen](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="819d6-383">An input [dataset](data-factory-create-datasets.md) of type [FileShare](#dataset-properties).</span></span>
* <span data-ttu-id="819d6-384">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="819d6-384">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
* <span data-ttu-id="819d6-385">En [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [FileSystemSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="819d6-385">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [FileSystemSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="819d6-386">I följande exempel kopierar time series-data från ett lokalt filsystem till Azure Blob storage varje timme.</span><span class="sxs-lookup"><span data-stu-id="819d6-386">The following sample copies time-series data from an on-premises file system to Azure Blob storage every hour.</span></span> <span data-ttu-id="819d6-387">JSON-egenskaper som används i exemplen beskrivs i avsnitten efter exemplen.</span><span class="sxs-lookup"><span data-stu-id="819d6-387">The JSON properties that are used in these samples are described in the sections after the samples.</span></span>

<span data-ttu-id="819d6-388">Som ett första steg bör ställa in Data Management Gateway enligt anvisningarna i [flytta data mellan lokala källor och molnet med Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md).</span><span class="sxs-lookup"><span data-stu-id="819d6-388">As a first step, set up Data Management Gateway as per the instructions in [Move data between on-premises sources and the cloud with Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md).</span></span>

<span data-ttu-id="819d6-389">**Lokala filserver länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="819d6-389">**On-Premises File Server linked service:**</span></span>

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

<span data-ttu-id="819d6-390">Vi rekommenderar att du använder den **encryptedCredential** egenskapen i stället den **userid** och **lösenord** egenskaper.</span><span class="sxs-lookup"><span data-stu-id="819d6-390">We recommend using the **encryptedCredential** property instead the **userid** and **password** properties.</span></span> <span data-ttu-id="819d6-391">Se [filserver länkade tjänsten](#linked-service-properties) mer information om den här länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="819d6-391">See [File Server linked service](#linked-service-properties) for details about this linked service.</span></span>

<span data-ttu-id="819d6-392">**Azure Storage länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="819d6-392">**Azure Storage linked service:**</span></span>

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

<span data-ttu-id="819d6-393">**Lokal fil system inkommande datauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="819d6-393">**On-premises file system input dataset:**</span></span>

<span data-ttu-id="819d6-394">Data hämtas från en ny fil varje timme.</span><span class="sxs-lookup"><span data-stu-id="819d6-394">Data is picked up from a new file every hour.</span></span> <span data-ttu-id="819d6-395">Egenskaperna folderPath och fileName bestäms baserat på starttid av sektorn.</span><span class="sxs-lookup"><span data-stu-id="819d6-395">The folderPath and fileName properties are determined based on the start time of the slice.</span></span>  

<span data-ttu-id="819d6-396">Ange `"external": "true"` informerar Data Factory att datamängden är extern till data factory och inte tillverkas av en aktivitet i datafabriken.</span><span class="sxs-lookup"><span data-stu-id="819d6-396">Setting `"external": "true"` informs Data Factory that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span>

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

<span data-ttu-id="819d6-397">**Azure Blob storage utdatauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="819d6-397">**Azure Blob storage output dataset:**</span></span>

<span data-ttu-id="819d6-398">Data skrivs till en ny blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="819d6-398">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="819d6-399">Sökvägen till mappen för blobben utvärderas dynamiskt baserat på starttiden för den sektor som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="819d6-399">The folder path for the blob is dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="819d6-400">Mappsökvägen använder år, månad, dag och timme delar av starttiden.</span><span class="sxs-lookup"><span data-stu-id="819d6-400">The folder path uses the year, month, day, and hour parts of the start time.</span></span>

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

<span data-ttu-id="819d6-401">**Kopieringsaktiviteten i en pipeline med filsystemet källa och mottagare för Blob:**</span><span class="sxs-lookup"><span data-stu-id="819d6-401">**A copy activity in a pipeline with File System source and Blob sink:**</span></span>

<span data-ttu-id="819d6-402">Pipelinen innehåller en kopia-aktivitet som är konfigurerad för att använda indata och utdata-datauppsättningar och är schemalagd att köras varje timme.</span><span class="sxs-lookup"><span data-stu-id="819d6-402">The pipeline contains a copy activity that is configured to use the input and output datasets, and is scheduled to run every hour.</span></span> <span data-ttu-id="819d6-403">I pipeline-JSON-definitionen av **källa** är inställd på **FileSystemSource**, och **sink** är inställd på **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="819d6-403">In the pipeline JSON definition, the **source** type is set to **FileSystemSource**, and **sink** type is set to **BlobSink**.</span></span>

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

### <a name="example-copy-data-from-azure-sql-database-to-an-on-premises-file-system"></a><span data-ttu-id="819d6-404">Exempel: Kopiera data från Azure SQL Database till ett lokalt filsystem</span><span class="sxs-lookup"><span data-stu-id="819d6-404">Example: Copy data from Azure SQL Database to an on-premises file system</span></span>
<span data-ttu-id="819d6-405">I följande exempel visas:</span><span class="sxs-lookup"><span data-stu-id="819d6-405">The following sample shows:</span></span>

* <span data-ttu-id="819d6-406">En länkad tjänst av typen [AzureSqlDatabase.](data-factory-azure-sql-connector.md#linked-service-properties)</span><span class="sxs-lookup"><span data-stu-id="819d6-406">A linked service of type [AzureSqlDatabase.](data-factory-azure-sql-connector.md#linked-service-properties)</span></span>
* <span data-ttu-id="819d6-407">En länkad tjänst av typen [OnPremisesFileServer](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="819d6-407">A linked service of type [OnPremisesFileServer](#linked-service-properties).</span></span>
* <span data-ttu-id="819d6-408">En datauppsättning som indata av typen [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="819d6-408">An input dataset of type [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).</span></span>
* <span data-ttu-id="819d6-409">En datamängd för utdata av typen [filresursen](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="819d6-409">An output dataset of type [FileShare](#dataset-properties).</span></span>
* <span data-ttu-id="819d6-410">En pipeline med en kopia-aktivitet som använder [SqlSource](data-factory-azure-sql-connector.md##copy-activity-properties) och [FileSystemSink](#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="819d6-410">A pipeline with a copy activity that uses [SqlSource](data-factory-azure-sql-connector.md##copy-activity-properties) and [FileSystemSink](#copy-activity-properties).</span></span>

<span data-ttu-id="819d6-411">Exemplet kopierar time series-data från en Azure SQL-tabell till ett lokalt filsystem varje timme.</span><span class="sxs-lookup"><span data-stu-id="819d6-411">The sample copies time-series data from an Azure SQL table to an on-premises file system every hour.</span></span> <span data-ttu-id="819d6-412">JSON-egenskaper som används i exemplen beskrivs i avsnitt efter exemplen.</span><span class="sxs-lookup"><span data-stu-id="819d6-412">The JSON properties that are used in these samples are described in sections after the samples.</span></span>

<span data-ttu-id="819d6-413">**Azure SQL Database länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="819d6-413">**Azure SQL Database linked service:**</span></span>

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

<span data-ttu-id="819d6-414">**Lokala filserver länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="819d6-414">**On-Premises File Server linked service:**</span></span>

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

<span data-ttu-id="819d6-415">Vi rekommenderar att du använder den **encryptedCredential** egenskapen istället för att använda den **userid** och **lösenord** egenskaper.</span><span class="sxs-lookup"><span data-stu-id="819d6-415">We recommend using the **encryptedCredential** property instead of using the **userid** and **password** properties.</span></span> <span data-ttu-id="819d6-416">Se [filsystemet länkade tjänsten](#linked-service-properties) mer information om den här länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="819d6-416">See [File System linked service](#linked-service-properties) for details about this linked service.</span></span>

<span data-ttu-id="819d6-417">**Azure SQL inkommande datamängd:**</span><span class="sxs-lookup"><span data-stu-id="819d6-417">**Azure SQL input dataset:**</span></span>

<span data-ttu-id="819d6-418">Exemplet förutsätter att du har skapat en tabell ”mytable” som prefix i Azure SQL och den innehåller en kolumn med namnet ”timestampcolumn” för time series-data.</span><span class="sxs-lookup"><span data-stu-id="819d6-418">The sample assumes that you've created a table “MyTable” in Azure SQL, and it contains a column called “timestampcolumn” for time-series data.</span></span>

<span data-ttu-id="819d6-419">Ange ``“external”: ”true”`` informerar Data Factory att datamängden är extern till data factory och inte tillverkas av en aktivitet i datafabriken.</span><span class="sxs-lookup"><span data-stu-id="819d6-419">Setting ``“external”: ”true”`` informs Data Factory that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span>

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

<span data-ttu-id="819d6-420">**Lokal fil system utdatauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="819d6-420">**On-premises file system output dataset:**</span></span>

<span data-ttu-id="819d6-421">Data kopieras till en ny fil varje timme.</span><span class="sxs-lookup"><span data-stu-id="819d6-421">Data is copied to a new file every hour.</span></span> <span data-ttu-id="819d6-422">FolderPath och filnamn för blob bestäms baserat på starttid av sektorn.</span><span class="sxs-lookup"><span data-stu-id="819d6-422">The folderPath and fileName for the blob are determined based on the start time of the slice.</span></span>

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

<span data-ttu-id="819d6-423">**Kopieringsaktiviteten i en pipeline med SQL-källa och mottagare för filsystemet:**</span><span class="sxs-lookup"><span data-stu-id="819d6-423">**A copy activity in a pipeline with SQL source and File System sink:**</span></span>

<span data-ttu-id="819d6-424">Pipelinen innehåller en kopia-aktivitet som är konfigurerad för att använda indata och utdata-datauppsättningar och är schemalagd att köras varje timme.</span><span class="sxs-lookup"><span data-stu-id="819d6-424">The pipeline contains a copy activity that is configured to use the input and output datasets, and is scheduled to run every hour.</span></span> <span data-ttu-id="819d6-425">I pipeline-JSON-definitionen av **källa** är inställd på **SqlSource**, och **sink** är inställd på **FileSystemSink**.</span><span class="sxs-lookup"><span data-stu-id="819d6-425">In the pipeline JSON definition, the **source** type is set to **SqlSource**, and the **sink** type is set to **FileSystemSink**.</span></span> <span data-ttu-id="819d6-426">SQL-frågan som har angetts för den **SqlReaderQuery** egenskapen väljer vilka data under den senaste timmen att kopiera.</span><span class="sxs-lookup"><span data-stu-id="819d6-426">The SQL query that is specified for the **SqlReaderQuery** property selects the data in the past hour to copy.</span></span>

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


<span data-ttu-id="819d6-427">Du kan också mappa kolumner från källan dataset till kolumner från sink datamängd i aktivitetsdefinitionen kopia.</span><span class="sxs-lookup"><span data-stu-id="819d6-427">You can also map columns from source dataset to columns from sink dataset in the copy activity definition.</span></span> <span data-ttu-id="819d6-428">Mer information finns i [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="819d6-428">For details, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="819d6-429">Prestanda- och justering</span><span class="sxs-lookup"><span data-stu-id="819d6-429">Performance and tuning</span></span>
 <span data-ttu-id="819d6-430">Mer information om viktiga faktorer som påverkar prestandan hos dataflytten (Kopieringsaktiviteten) i Azure Data Factory och olika sätt att optimera det, finns det [prestandajustering guide och Kopieringsaktivitet prestanda](data-factory-copy-activity-performance.md).</span><span class="sxs-lookup"><span data-stu-id="819d6-430">To learn about key factors that impact the performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it, see the [Copy Activity performance and tuning guide](data-factory-copy-activity-performance.md).</span></span>
