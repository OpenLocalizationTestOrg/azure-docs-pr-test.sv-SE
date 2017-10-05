---
title: "Flytta data från lokala HDFS | Microsoft Docs"
description: "Läs mer om hur du flyttar data från lokala HDFS med hjälp av Azure Data Factory."
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
ms.openlocfilehash: 9a8f3156a62a1a7aa49377349e8a85454efeda50
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="move-data-from-on-premises-hdfs-using-azure-data-factory"></a><span data-ttu-id="df5d3-103">Flytta data från lokala HDFS med hjälp av Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="df5d3-103">Move data from on-premises HDFS using Azure Data Factory</span></span>
<span data-ttu-id="df5d3-104">Den här artikeln förklarar hur du använder aktiviteten kopiera i Azure Data Factory för att flytta data från en lokal HDFS.</span><span class="sxs-lookup"><span data-stu-id="df5d3-104">This article explains how to use the Copy Activity in Azure Data Factory to move data from an on-premises HDFS.</span></span> <span data-ttu-id="df5d3-105">Den bygger på den [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som presenterar en allmän översikt över dataflyttning med copy-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="df5d3-105">It builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span>

<span data-ttu-id="df5d3-106">Du kan kopiera data från HDFS till alla stöds sink-datalagret.</span><span class="sxs-lookup"><span data-stu-id="df5d3-106">You can copy data from HDFS to any supported sink data store.</span></span> <span data-ttu-id="df5d3-107">En lista över datakällor som stöds som sänkor av kopieringsaktiviteten, finns det [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabell.</span><span class="sxs-lookup"><span data-stu-id="df5d3-107">For a list of data stores supported as sinks by the copy activity, see the [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="df5d3-108">Data factory stöder för närvarande endast flytta data från en lokal HDFS till andra databaser, men inte för att flytta data från andra datalager till en lokal HDFS.</span><span class="sxs-lookup"><span data-stu-id="df5d3-108">Data factory currently supports only moving data from an on-premises HDFS to other data stores, but not for moving data from other data stores to an on-premises HDFS.</span></span>

> [!NOTE]
> <span data-ttu-id="df5d3-109">Kopieringsaktiviteten tar inte bort källfilen när den har kopierats till målet.</span><span class="sxs-lookup"><span data-stu-id="df5d3-109">Copy Activity does not delete the source file after it is successfully copied to the destination.</span></span> <span data-ttu-id="df5d3-110">Om du vill ta bort källfilen efter en lyckad kopiering kan du skapa en anpassad aktivitet för att ta bort filen och använda aktiviteten i pipelinen.</span><span class="sxs-lookup"><span data-stu-id="df5d3-110">If you need to delete the source file after a successful copy, create a custom activity to delete the file and use the activity in the pipeline.</span></span> 

## <a name="enabling-connectivity"></a><span data-ttu-id="df5d3-111">Aktivera anslutning</span><span class="sxs-lookup"><span data-stu-id="df5d3-111">Enabling connectivity</span></span>
<span data-ttu-id="df5d3-112">Data Factory-tjänsten stöder anslutning till lokala HDFS med Data Management Gateway.</span><span class="sxs-lookup"><span data-stu-id="df5d3-112">Data Factory service supports connecting to on-premises HDFS using the Data Management Gateway.</span></span> <span data-ttu-id="df5d3-113">Se [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikeln innehåller information om Data Management Gateway och stegvisa instruktioner om hur du konfigurerar en gateway.</span><span class="sxs-lookup"><span data-stu-id="df5d3-113">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article to learn about Data Management Gateway and step-by-step instructions on setting up the gateway.</span></span> <span data-ttu-id="df5d3-114">Använd en gateway för att ansluta till HDFS även om den finns i en Azure IaaS-VM.</span><span class="sxs-lookup"><span data-stu-id="df5d3-114">Use the gateway to connect to HDFS even if it is hosted in an Azure IaaS VM.</span></span>

> [!NOTE]
> <span data-ttu-id="df5d3-115">Se till att Data Management Gateway har åtkomst till **alla** [nod namnservern]: [name nod port] och [nod dataservrar]: [data nod port] i Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="df5d3-115">Make sure the Data Management Gateway can access to **ALL** the [name node server]:[name node port] and [data node servers]:[data node port] of the Hadoop cluster.</span></span> <span data-ttu-id="df5d3-116">Standard [namn på noden port] är 50070 och standardvärdet [data nod port] är 50075.</span><span class="sxs-lookup"><span data-stu-id="df5d3-116">Default [name node port] is 50070, and default [data node port] is 50075.</span></span>

<span data-ttu-id="df5d3-117">Medan du kan installera gatewayen på samma lokala dator eller virtuell Azure-dator som HDFS, rekommenderar vi att du installerar en gateway på en separat dator/Azure IaaS-VM.</span><span class="sxs-lookup"><span data-stu-id="df5d3-117">While you can install gateway on the same on-premises machine or the Azure VM as the HDFS, we recommend that you install the gateway on a separate machine/Azure IaaS VM.</span></span> <span data-ttu-id="df5d3-118">Med gatewayen på en separat dator minskar resurskonflikter och förbättrar prestandan.</span><span class="sxs-lookup"><span data-stu-id="df5d3-118">Having gateway on a separate machine reduces resource contention and improves performance.</span></span> <span data-ttu-id="df5d3-119">När du installerar en gateway på en separat dator ska datorn kunna få åtkomst till datorn med HDFS.</span><span class="sxs-lookup"><span data-stu-id="df5d3-119">When you install the gateway on a separate machine, the machine should be able to access the machine with the HDFS.</span></span>

## <a name="getting-started"></a><span data-ttu-id="df5d3-120">Komma igång</span><span class="sxs-lookup"><span data-stu-id="df5d3-120">Getting started</span></span>
<span data-ttu-id="df5d3-121">Du kan skapa en pipeline med en kopia-aktivitet som flyttar data från en källa för HDFS med hjälp av olika verktyg/API: er.</span><span class="sxs-lookup"><span data-stu-id="df5d3-121">You can create a pipeline with a copy activity that moves data from a HDFS source by using different tools/APIs.</span></span>

<span data-ttu-id="df5d3-122">Det enklaste sättet att skapa en pipeline är att använda den **guiden Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="df5d3-122">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="df5d3-123">Finns [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden Kopiera data.</span><span class="sxs-lookup"><span data-stu-id="df5d3-123">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span>

<span data-ttu-id="df5d3-124">Du kan också använda följande verktyg för att skapa en pipeline: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall**, **.NET API**, och **REST API**.</span><span class="sxs-lookup"><span data-stu-id="df5d3-124">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="df5d3-125">Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner för att skapa en pipeline med en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="df5d3-125">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span>

<span data-ttu-id="df5d3-126">Om du använder verktyg eller API: er, kan du utföra följande steg för att skapa en pipeline som flyttar data från ett dataarkiv som källa till ett dataarkiv som mottagare:</span><span class="sxs-lookup"><span data-stu-id="df5d3-126">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span>

1. <span data-ttu-id="df5d3-127">Skapa **länkade tjänster** att länka inkommande och utgående data lagras till din data factory.</span><span class="sxs-lookup"><span data-stu-id="df5d3-127">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="df5d3-128">Skapa **datauppsättningar** att representera inkommande och utgående data för kopieringen.</span><span class="sxs-lookup"><span data-stu-id="df5d3-128">Create **datasets** to represent input and output data for the copy operation.</span></span>
3. <span data-ttu-id="df5d3-129">Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata.</span><span class="sxs-lookup"><span data-stu-id="df5d3-129">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span>

<span data-ttu-id="df5d3-130">När du använder guiden skapas JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och pipelinen) automatiskt för dig.</span><span class="sxs-lookup"><span data-stu-id="df5d3-130">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="df5d3-131">När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av JSON-format.</span><span class="sxs-lookup"><span data-stu-id="df5d3-131">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="df5d3-132">Ett exempel med JSON-definitioner för Data Factory-entiteter som används för att kopiera data från ett HDFS-datalager finns [JSON-exempel: kopiera data från lokala HDFS till Azure Blob](#json-example-copy-data-from-on-premises-hdfs-to-azure-blob) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="df5d3-132">For a sample with JSON definitions for Data Factory entities that are used to copy data from a HDFS data store, see [JSON example: Copy data from on-premises HDFS to Azure Blob](#json-example-copy-data-from-on-premises-hdfs-to-azure-blob) section of this article.</span></span>

<span data-ttu-id="df5d3-133">Följande avsnitt innehåller information om JSON-egenskaper som används för att definiera Data Factory entiteter till HDFS:</span><span class="sxs-lookup"><span data-stu-id="df5d3-133">The following sections provide details about JSON properties that are used to define Data Factory entities specific to HDFS:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="df5d3-134">Länkad tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="df5d3-134">Linked service properties</span></span>
<span data-ttu-id="df5d3-135">En länkad tjänst länkar ett datalager till en data factory.</span><span class="sxs-lookup"><span data-stu-id="df5d3-135">A linked service links a data store to a data factory.</span></span> <span data-ttu-id="df5d3-136">Du skapar en länkad tjänst av typen **Hdfs** att länka en lokala HDFS till din data factory.</span><span class="sxs-lookup"><span data-stu-id="df5d3-136">You create a linked service of type **Hdfs** to link an on-premises HDFS to your data factory.</span></span> <span data-ttu-id="df5d3-137">Följande tabell innehåller beskrivning för JSON-element som är specifika för HDFS länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="df5d3-137">The following table provides description for JSON elements specific to HDFS linked service.</span></span>

| <span data-ttu-id="df5d3-138">Egenskap</span><span class="sxs-lookup"><span data-stu-id="df5d3-138">Property</span></span> | <span data-ttu-id="df5d3-139">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="df5d3-139">Description</span></span> | <span data-ttu-id="df5d3-140">Krävs</span><span class="sxs-lookup"><span data-stu-id="df5d3-140">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="df5d3-141">typ</span><span class="sxs-lookup"><span data-stu-id="df5d3-141">type</span></span> |<span data-ttu-id="df5d3-142">Egenskapen type måste anges till: **Hdfs**</span><span class="sxs-lookup"><span data-stu-id="df5d3-142">The type property must be set to: **Hdfs**</span></span> |<span data-ttu-id="df5d3-143">Ja</span><span class="sxs-lookup"><span data-stu-id="df5d3-143">Yes</span></span> |
| <span data-ttu-id="df5d3-144">URL</span><span class="sxs-lookup"><span data-stu-id="df5d3-144">Url</span></span> |<span data-ttu-id="df5d3-145">URL till HDFS</span><span class="sxs-lookup"><span data-stu-id="df5d3-145">URL to the HDFS</span></span> |<span data-ttu-id="df5d3-146">Ja</span><span class="sxs-lookup"><span data-stu-id="df5d3-146">Yes</span></span> |
| <span data-ttu-id="df5d3-147">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="df5d3-147">authenticationType</span></span> |<span data-ttu-id="df5d3-148">Anonym, eller Windows.</span><span class="sxs-lookup"><span data-stu-id="df5d3-148">Anonymous, or Windows.</span></span> <br><br> <span data-ttu-id="df5d3-149">Att använda **Kerberos-autentisering** HDFS-anslutningen finns i [i det här avsnittet](#use-kerberos-authentication-for-hdfs-connector) därefter konfigurera din lokala miljö.</span><span class="sxs-lookup"><span data-stu-id="df5d3-149">To use **Kerberos authentication** for HDFS connector, refer to [this section](#use-kerberos-authentication-for-hdfs-connector) to set up your on-premises environment accordingly.</span></span> |<span data-ttu-id="df5d3-150">Ja</span><span class="sxs-lookup"><span data-stu-id="df5d3-150">Yes</span></span> |
| <span data-ttu-id="df5d3-151">Användarnamn</span><span class="sxs-lookup"><span data-stu-id="df5d3-151">userName</span></span> |<span data-ttu-id="df5d3-152">Användarnamn för Windows-autentisering.</span><span class="sxs-lookup"><span data-stu-id="df5d3-152">Username for Windows authentication.</span></span> |<span data-ttu-id="df5d3-153">Ja (för Windows-autentisering)</span><span class="sxs-lookup"><span data-stu-id="df5d3-153">Yes (for Windows Authentication)</span></span> |
| <span data-ttu-id="df5d3-154">lösenord</span><span class="sxs-lookup"><span data-stu-id="df5d3-154">password</span></span> |<span data-ttu-id="df5d3-155">Lösenordet för Windows-autentisering.</span><span class="sxs-lookup"><span data-stu-id="df5d3-155">Password for Windows authentication.</span></span> |<span data-ttu-id="df5d3-156">Ja (för Windows-autentisering)</span><span class="sxs-lookup"><span data-stu-id="df5d3-156">Yes (for Windows Authentication)</span></span> |
| <span data-ttu-id="df5d3-157">gatewayName</span><span class="sxs-lookup"><span data-stu-id="df5d3-157">gatewayName</span></span> |<span data-ttu-id="df5d3-158">Namnet på den gateway som Data Factory-tjänsten ska använda för att ansluta till HDFS.</span><span class="sxs-lookup"><span data-stu-id="df5d3-158">Name of the gateway that the Data Factory service should use to connect to the HDFS.</span></span> |<span data-ttu-id="df5d3-159">Ja</span><span class="sxs-lookup"><span data-stu-id="df5d3-159">Yes</span></span> |
| <span data-ttu-id="df5d3-160">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="df5d3-160">encryptedCredential</span></span> |<span data-ttu-id="df5d3-161">[Nya AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) utdata för autentiseringsuppgifterna för åtkomst.</span><span class="sxs-lookup"><span data-stu-id="df5d3-161">[New-AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) output of the access credential.</span></span> |<span data-ttu-id="df5d3-162">Nej</span><span class="sxs-lookup"><span data-stu-id="df5d3-162">No</span></span> |

### <a name="using-anonymous-authentication"></a><span data-ttu-id="df5d3-163">Använder anonym autentisering</span><span class="sxs-lookup"><span data-stu-id="df5d3-163">Using Anonymous authentication</span></span>

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

### <a name="using-windows-authentication"></a><span data-ttu-id="df5d3-164">Med Windows-autentisering</span><span class="sxs-lookup"><span data-stu-id="df5d3-164">Using Windows authentication</span></span>

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
## <a name="dataset-properties"></a><span data-ttu-id="df5d3-165">Egenskaper för datamängd</span><span class="sxs-lookup"><span data-stu-id="df5d3-165">Dataset properties</span></span>
<span data-ttu-id="df5d3-166">En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns i [skapa datauppsättningar](data-factory-create-datasets.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="df5d3-166">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="df5d3-167">Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av dataset (Azure SQL Azure blob, Azure-tabellen, osv.).</span><span class="sxs-lookup"><span data-stu-id="df5d3-167">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="df5d3-168">Den **typeProperties** avsnitt är olika för varje typ av dataset och innehåller information om placeringen av data i datalagret.</span><span class="sxs-lookup"><span data-stu-id="df5d3-168">The **typeProperties** section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="df5d3-169">TypeProperties avsnittet för dataset av typen **filresursen** (som omfattar HDFS dataset) har följande egenskaper</span><span class="sxs-lookup"><span data-stu-id="df5d3-169">The typeProperties section for dataset of type **FileShare** (which includes HDFS dataset) has the following properties</span></span>

| <span data-ttu-id="df5d3-170">Egenskap</span><span class="sxs-lookup"><span data-stu-id="df5d3-170">Property</span></span> | <span data-ttu-id="df5d3-171">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="df5d3-171">Description</span></span> | <span data-ttu-id="df5d3-172">Krävs</span><span class="sxs-lookup"><span data-stu-id="df5d3-172">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="df5d3-173">folderPath</span><span class="sxs-lookup"><span data-stu-id="df5d3-173">folderPath</span></span> |<span data-ttu-id="df5d3-174">Sökvägen till mappen.</span><span class="sxs-lookup"><span data-stu-id="df5d3-174">Path to the folder.</span></span> <span data-ttu-id="df5d3-175">Exempel:`myfolder`</span><span class="sxs-lookup"><span data-stu-id="df5d3-175">Example: `myfolder`</span></span><br/><br/><span data-ttu-id="df5d3-176">Använda escape-tecknet ' \ ' för specialtecken i strängen.</span><span class="sxs-lookup"><span data-stu-id="df5d3-176">Use escape character ‘ \ ’ for special characters in the string.</span></span> <span data-ttu-id="df5d3-177">Till exempel: Ange mapp för folder\subfolder,\\\\undermapp och ange d: för d:\samplefolder,\\\\Exempelmapp.</span><span class="sxs-lookup"><span data-stu-id="df5d3-177">For example: for folder\subfolder, specify folder\\\\subfolder and for d:\samplefolder, specify d:\\\\samplefolder.</span></span><br/><br/><span data-ttu-id="df5d3-178">Du kan kombinera den här egenskapen med **partitionBy** ha mappen sökvägar baserat på sektorn börja/sluta datum gånger.</span><span class="sxs-lookup"><span data-stu-id="df5d3-178">You can combine this property with **partitionBy** to have folder paths based on slice start/end date-times.</span></span> |<span data-ttu-id="df5d3-179">Ja</span><span class="sxs-lookup"><span data-stu-id="df5d3-179">Yes</span></span> |
| <span data-ttu-id="df5d3-180">fileName</span><span class="sxs-lookup"><span data-stu-id="df5d3-180">fileName</span></span> |<span data-ttu-id="df5d3-181">Ange namnet på filen i den **folderPath** om du vill att referera till en viss fil i mappen.</span><span class="sxs-lookup"><span data-stu-id="df5d3-181">Specify the name of the file in the **folderPath** if you want the table to refer to a specific file in the folder.</span></span> <span data-ttu-id="df5d3-182">Om du inte anger något värde för den här egenskapen tabellen pekar på alla filer i mappen.</span><span class="sxs-lookup"><span data-stu-id="df5d3-182">If you do not specify any value for this property, the table points to all files in the folder.</span></span><br/><br/><span data-ttu-id="df5d3-183">Om filnamnet inte anges för en datamängd för utdata är namnet på den genererade filen i följande det här formatet:</span><span class="sxs-lookup"><span data-stu-id="df5d3-183">When fileName is not specified for an output dataset, the name of the generated file would be in the following this format:</span></span> <br/><br/><span data-ttu-id="df5d3-184">Data. <Guid>.txt (till exempel:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span><span class="sxs-lookup"><span data-stu-id="df5d3-184">Data.<Guid>.txt (for example: : Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span></span> |<span data-ttu-id="df5d3-185">Nej</span><span class="sxs-lookup"><span data-stu-id="df5d3-185">No</span></span> |
| <span data-ttu-id="df5d3-186">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="df5d3-186">partitionedBy</span></span> |<span data-ttu-id="df5d3-187">partitionedBy kan användas för att ange en dynamisk folderPath filnamn för tid series-data.</span><span class="sxs-lookup"><span data-stu-id="df5d3-187">partitionedBy can be used to specify a dynamic folderPath, filename for time series data.</span></span> <span data-ttu-id="df5d3-188">Exempel: folderPath parametriserade varje timme av data.</span><span class="sxs-lookup"><span data-stu-id="df5d3-188">Example: folderPath parameterized for every hour of data.</span></span> |<span data-ttu-id="df5d3-189">Nej</span><span class="sxs-lookup"><span data-stu-id="df5d3-189">No</span></span> |
| <span data-ttu-id="df5d3-190">Format</span><span class="sxs-lookup"><span data-stu-id="df5d3-190">format</span></span> | <span data-ttu-id="df5d3-191">Följande format stöds: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="df5d3-191">The following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="df5d3-192">Ange den **typen** egenskap under format till ett av dessa värden.</span><span class="sxs-lookup"><span data-stu-id="df5d3-192">Set the **type** property under format to one of these values.</span></span> <span data-ttu-id="df5d3-193">Mer information finns i [textformat](data-factory-supported-file-and-compression-formats.md#text-format), [Json-Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-formatet](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), och [parkettgolv Format](data-factory-supported-file-and-compression-formats.md#parquet-format) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="df5d3-193">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="df5d3-194">Om du vill **kopiera filer som-är** mellan filbaserade butiker (binär kopia), hoppa över avsnittet format i både inkommande och utgående dataset-definitioner.</span><span class="sxs-lookup"><span data-stu-id="df5d3-194">If you want to **copy files as-is** between file-based stores (binary copy), skip the format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="df5d3-195">Nej</span><span class="sxs-lookup"><span data-stu-id="df5d3-195">No</span></span> |
| <span data-ttu-id="df5d3-196">Komprimering</span><span class="sxs-lookup"><span data-stu-id="df5d3-196">compression</span></span> | <span data-ttu-id="df5d3-197">Ange typ och kompression för data.</span><span class="sxs-lookup"><span data-stu-id="df5d3-197">Specify the type and level of compression for the data.</span></span> <span data-ttu-id="df5d3-198">Typer som stöds är: **GZip**, **Deflate**, **BZip2**, och **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="df5d3-198">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="df5d3-199">Nivåer som stöds är: **Optimal** och **snabbast**.</span><span class="sxs-lookup"><span data-stu-id="df5d3-199">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="df5d3-200">Mer information finns i [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="df5d3-200">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="df5d3-201">Nej</span><span class="sxs-lookup"><span data-stu-id="df5d3-201">No</span></span> |

> [!NOTE]
> <span data-ttu-id="df5d3-202">filnamnet och fileFilter kan inte användas samtidigt.</span><span class="sxs-lookup"><span data-stu-id="df5d3-202">filename and fileFilter cannot be used simultaneously.</span></span>

### <a name="using-partionedby-property"></a><span data-ttu-id="df5d3-203">Med hjälp av egenskapen partionedBy</span><span class="sxs-lookup"><span data-stu-id="df5d3-203">Using partionedBy property</span></span>
<span data-ttu-id="df5d3-204">Som nämnts ovan, du kan ange en dynamisk folderPath och ett filnamn för tid series-data med den **partitionedBy** egenskapen [Data Factory-funktioner och systemvariablerna](data-factory-functions-variables.md).</span><span class="sxs-lookup"><span data-stu-id="df5d3-204">As mentioned in the previous section, you can specify a dynamic folderPath and filename for time series data with the **partitionedBy** property, [Data Factory functions, and the system variables](data-factory-functions-variables.md).</span></span>

<span data-ttu-id="df5d3-205">Läs mer om tid serien datauppsättningar, schemaläggning och segment i [skapa datauppsättningar](data-factory-create-datasets.md), [schemaläggning och utförande](data-factory-scheduling-and-execution.md), och [skapar Pipelines](data-factory-create-pipelines.md) artiklar.</span><span class="sxs-lookup"><span data-stu-id="df5d3-205">To learn more about time series datasets, scheduling, and slices, see [Creating Datasets](data-factory-create-datasets.md), [Scheduling & Execution](data-factory-scheduling-and-execution.md), and [Creating Pipelines](data-factory-create-pipelines.md) articles.</span></span>

#### <a name="sample-1"></a><span data-ttu-id="df5d3-206">Exempel 1:</span><span class="sxs-lookup"><span data-stu-id="df5d3-206">Sample 1:</span></span>

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```
<span data-ttu-id="df5d3-207">I det här exemplet {segment} ersättas med det angivna värdet för Data Factory systemvariabel SliceStart i formatet (YYYYMMDDHH).</span><span class="sxs-lookup"><span data-stu-id="df5d3-207">In this example {Slice} is replaced with the value of Data Factory system variable SliceStart in the format (YYYYMMDDHH) specified.</span></span> <span data-ttu-id="df5d3-208">SliceStart refererar till starttid av sektorn.</span><span class="sxs-lookup"><span data-stu-id="df5d3-208">The SliceStart refers to start time of the slice.</span></span> <span data-ttu-id="df5d3-209">FolderPath är olika för varje segment.</span><span class="sxs-lookup"><span data-stu-id="df5d3-209">The folderPath is different for each slice.</span></span> <span data-ttu-id="df5d3-210">Till exempel: 2014100103-wikidatagateway/wikisampledataout eller 2014100104-wikidatagateway/wikisampledataout.</span><span class="sxs-lookup"><span data-stu-id="df5d3-210">For example: wikidatagateway/wikisampledataout/2014100103 or wikidatagateway/wikisampledataout/2014100104.</span></span>

#### <a name="sample-2"></a><span data-ttu-id="df5d3-211">Exempel 2:</span><span class="sxs-lookup"><span data-stu-id="df5d3-211">Sample 2:</span></span>

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
<span data-ttu-id="df5d3-212">I det här exemplet extraheras år, månad, dag och tidpunkt för SliceStart till olika variabler som används av egenskaperna folderPath och filnamn.</span><span class="sxs-lookup"><span data-stu-id="df5d3-212">In this example, year, month, day, and time of SliceStart are extracted into separate variables that are used by folderPath and fileName properties.</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="df5d3-213">Kopiera egenskaper för aktivitet</span><span class="sxs-lookup"><span data-stu-id="df5d3-213">Copy activity properties</span></span>
<span data-ttu-id="df5d3-214">En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns i [skapar Pipelines](data-factory-create-pipelines.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="df5d3-214">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="df5d3-215">Egenskaper som namn, beskrivning, ingående och utgående tabeller och principer är tillgängliga för alla typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="df5d3-215">Properties such as name, description, input and output tables, and policies are available for all types of activities.</span></span>

<span data-ttu-id="df5d3-216">De egenskaper som är tillgängliga i avsnittet typeProperties i aktiviteten varierar beroende på varje aktivitetstyp.</span><span class="sxs-lookup"><span data-stu-id="df5d3-216">Whereas, properties available in the typeProperties section of the activity vary with each activity type.</span></span> <span data-ttu-id="df5d3-217">För Kopieringsaktivitet kan variera de beroende på vilka typer av datakällor och sänkor.</span><span class="sxs-lookup"><span data-stu-id="df5d3-217">For Copy activity, they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="df5d3-218">För Kopieringsaktiviteten när datakällan är av typen **FileSystemSource** följande egenskaper finns i avsnittet typeProperties:</span><span class="sxs-lookup"><span data-stu-id="df5d3-218">For Copy Activity, when source is of type **FileSystemSource** the following properties are available in typeProperties section:</span></span>

<span data-ttu-id="df5d3-219">**FileSystemSource** stöder följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="df5d3-219">**FileSystemSource** supports the following properties:</span></span>

| <span data-ttu-id="df5d3-220">Egenskap</span><span class="sxs-lookup"><span data-stu-id="df5d3-220">Property</span></span> | <span data-ttu-id="df5d3-221">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="df5d3-221">Description</span></span> | <span data-ttu-id="df5d3-222">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="df5d3-222">Allowed values</span></span> | <span data-ttu-id="df5d3-223">Krävs</span><span class="sxs-lookup"><span data-stu-id="df5d3-223">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="df5d3-224">Rekursiva</span><span class="sxs-lookup"><span data-stu-id="df5d3-224">recursive</span></span> |<span data-ttu-id="df5d3-225">Anger om data läses rekursivt från undermappar eller endast från den angivna mappen.</span><span class="sxs-lookup"><span data-stu-id="df5d3-225">Indicates whether the data is read recursively from the sub folders or only from the specified folder.</span></span> |<span data-ttu-id="df5d3-226">SANT, FALSKT (standard)</span><span class="sxs-lookup"><span data-stu-id="df5d3-226">True, False (default)</span></span> |<span data-ttu-id="df5d3-227">Nej</span><span class="sxs-lookup"><span data-stu-id="df5d3-227">No</span></span> |

## <a name="supported-file-and-compression-formats"></a><span data-ttu-id="df5d3-228">Fil- och komprimering format som stöds</span><span class="sxs-lookup"><span data-stu-id="df5d3-228">Supported file and compression formats</span></span>
<span data-ttu-id="df5d3-229">Se [format och komprimering i Azure Data Factory](data-factory-supported-file-and-compression-formats.md) artikeln för information.</span><span class="sxs-lookup"><span data-stu-id="df5d3-229">See [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md) article on details.</span></span>

## <a name="json-example-copy-data-from-on-premises-hdfs-to-azure-blob"></a><span data-ttu-id="df5d3-230">JSON-exempel: kopiera data från lokala HDFS till Azure-Blob</span><span class="sxs-lookup"><span data-stu-id="df5d3-230">JSON example: Copy data from on-premises HDFS to Azure Blob</span></span>
<span data-ttu-id="df5d3-231">Det här exemplet visas hur du kopierar data från en lokal HDFS till Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="df5d3-231">This sample shows how to copy data from an on-premises HDFS to Azure Blob Storage.</span></span> <span data-ttu-id="df5d3-232">Dock datan kan kopieras **direkt** till någon av sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hjälp av aktiviteten kopiera i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="df5d3-232">However, data can be copied **directly** to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>  

<span data-ttu-id="df5d3-233">Exemplet innehåller JSON definitioner för följande Data Factory-enheter.</span><span class="sxs-lookup"><span data-stu-id="df5d3-233">The sample provides JSON definitions for the following Data Factory entities.</span></span> <span data-ttu-id="df5d3-234">Du kan använda dessa definitioner för att skapa en pipeline för att kopiera data från HDFS till Azure Blob Storage med hjälp av [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="df5d3-234">You can use these definitions to create a pipeline to copy data from HDFS to Azure Blob Storage by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span>

1. <span data-ttu-id="df5d3-235">En länkad tjänst av typen [OnPremisesHdfs](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="df5d3-235">A linked service of type [OnPremisesHdfs](#linked-service-properties).</span></span>
2. <span data-ttu-id="df5d3-236">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="df5d3-236">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="df5d3-237">Indata [dataset](data-factory-create-datasets.md) av typen [filresursen](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="df5d3-237">An input [dataset](data-factory-create-datasets.md) of type [FileShare](#dataset-properties).</span></span>
4. <span data-ttu-id="df5d3-238">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="df5d3-238">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="df5d3-239">En [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [FileSystemSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="df5d3-239">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [FileSystemSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="df5d3-240">Exemplet kopierar data från en lokal HDFS till en Azure blob varje timme.</span><span class="sxs-lookup"><span data-stu-id="df5d3-240">The sample copies data from an on-premises HDFS to an Azure blob every hour.</span></span> <span data-ttu-id="df5d3-241">JSON-egenskaper som används i exemplen beskrivs i exemplen i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="df5d3-241">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="df5d3-242">Som ett första steg bör du ställa in data management gateway.</span><span class="sxs-lookup"><span data-stu-id="df5d3-242">As a first step, set up the data management gateway.</span></span> <span data-ttu-id="df5d3-243">Anvisningarna i den [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="df5d3-243">The instructions in the [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="df5d3-244">**HDFS länkade tjänsten:** det här exemplet använder Windows-autentisering.</span><span class="sxs-lookup"><span data-stu-id="df5d3-244">**HDFS linked service:** This example uses the Windows authentication.</span></span> <span data-ttu-id="df5d3-245">Se [HDFS länkade tjänsten](#linked-service-properties) avsnittet för olika typer av autentisering som du kan använda.</span><span class="sxs-lookup"><span data-stu-id="df5d3-245">See [HDFS linked service](#linked-service-properties) section for different types of authentication you can use.</span></span>

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

<span data-ttu-id="df5d3-246">**Azure Storage länkade tjänsten:**</span><span class="sxs-lookup"><span data-stu-id="df5d3-246">**Azure Storage linked service:**</span></span>

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

<span data-ttu-id="df5d3-247">**HDFS indatauppsättning:** den här datamängden refererar till mappen HDFS DataTransfer/UnitTest /.</span><span class="sxs-lookup"><span data-stu-id="df5d3-247">**HDFS input dataset:** This dataset refers to the HDFS folder DataTransfer/UnitTest/.</span></span> <span data-ttu-id="df5d3-248">Pipelinen kopierar alla filer i den här mappen till målet.</span><span class="sxs-lookup"><span data-stu-id="df5d3-248">The pipeline copies all the files in this folder to the destination.</span></span>

<span data-ttu-id="df5d3-249">Inställningen ”externa”: ”true” informerar Data Factory-tjänsten att datamängden är extern till data factory och inte tillverkas av en aktivitet i datafabriken.</span><span class="sxs-lookup"><span data-stu-id="df5d3-249">Setting “external”: ”true” informs the Data Factory service that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span>

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

<span data-ttu-id="df5d3-250">**Azure Blob utdatauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="df5d3-250">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="df5d3-251">Data skrivs till en ny blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="df5d3-251">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="df5d3-252">Sökvägen till mappen för blobben utvärderas dynamiskt baserat på starttiden för den sektor som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="df5d3-252">The folder path for the blob is dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="df5d3-253">Mappsökvägen använder år, månad, dag och timmar delar av starttiden.</span><span class="sxs-lookup"><span data-stu-id="df5d3-253">The folder path uses year, month, day, and hours parts of the start time.</span></span>

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

<span data-ttu-id="df5d3-254">**Kopieringsaktiviteten i en pipeline med filsystemet källa och mottagare för Blob:**</span><span class="sxs-lookup"><span data-stu-id="df5d3-254">**A copy activity in a pipeline with File System source and Blob sink:**</span></span>

<span data-ttu-id="df5d3-255">Pipelinen innehåller en kopia-aktivitet som är konfigurerad för att använda dessa indata och utdata-datauppsättningar och är schemalagd att köras varje timme.</span><span class="sxs-lookup"><span data-stu-id="df5d3-255">The pipeline contains a Copy Activity that is configured to use these input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="df5d3-256">I pipeline-JSON-definitionen av **källa** är inställd på **FileSystemSource** och **sink** är inställd på **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="df5d3-256">In the pipeline JSON definition, the **source** type is set to **FileSystemSource** and **sink** type is set to **BlobSink**.</span></span> <span data-ttu-id="df5d3-257">SQL-frågan som angetts för den **frågan** egenskapen väljer vilka data under den senaste timmen att kopiera.</span><span class="sxs-lookup"><span data-stu-id="df5d3-257">The SQL query specified for the **query** property selects the data in the past hour to copy.</span></span>

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

## <a name="use-kerberos-authentication-for-hdfs-connector"></a><span data-ttu-id="df5d3-258">Använda Kerberos-autentisering för HDFS-anslutningen</span><span class="sxs-lookup"><span data-stu-id="df5d3-258">Use Kerberos authentication for HDFS connector</span></span>
<span data-ttu-id="df5d3-259">Det finns två alternativ för att konfigurera den lokala miljön för att använda Kerberos-autentisering i HDFS-koppling.</span><span class="sxs-lookup"><span data-stu-id="df5d3-259">There are two options to set up the on-premises environment so as to use Kerberos Authentication in HDFS connector.</span></span> <span data-ttu-id="df5d3-260">Du kan välja den bättre passar ditt ärende.</span><span class="sxs-lookup"><span data-stu-id="df5d3-260">You can choose the one better fits your case.</span></span>
* <span data-ttu-id="df5d3-261">Alternativ 1: [anslutning till gateway-datorn i Kerberos-sfär](#kerberos-join-realm)</span><span class="sxs-lookup"><span data-stu-id="df5d3-261">Option 1: [Join gateway machine in Kerberos realm](#kerberos-join-realm)</span></span>
* <span data-ttu-id="df5d3-262">Alternativ 2: [aktivera ömsesidigt förtroende mellan Windows-domän och Kerberos-sfär](#kerberos-mutual-trust)</span><span class="sxs-lookup"><span data-stu-id="df5d3-262">Option 2: [Enable mutual trust between Windows domain and Kerberos realm](#kerberos-mutual-trust)</span></span>

### <span data-ttu-id="df5d3-263"><a name="kerberos-join-realm"></a>Alternativ 1: Ansluta till gateway-datorn i Kerberos-sfär</span><span class="sxs-lookup"><span data-stu-id="df5d3-263"><a name="kerberos-join-realm"></a>Option 1: Join gateway machine in Kerberos realm</span></span>

#### <a name="requirement"></a><span data-ttu-id="df5d3-264">Krav:</span><span class="sxs-lookup"><span data-stu-id="df5d3-264">Requirement:</span></span>

* <span data-ttu-id="df5d3-265">Gateway-datorn måste ansluta till Kerberos-sfären och kan inte ansluta till en Windows-domän.</span><span class="sxs-lookup"><span data-stu-id="df5d3-265">The gateway machine needs to join the Kerberos realm and can’t join any Windows domain.</span></span>

#### <a name="how-to-configure"></a><span data-ttu-id="df5d3-266">Så här konfigurerar du:</span><span class="sxs-lookup"><span data-stu-id="df5d3-266">How to configure:</span></span>

<span data-ttu-id="df5d3-267">**På gateway-datorn:**</span><span class="sxs-lookup"><span data-stu-id="df5d3-267">**On gateway machine:**</span></span>

1.  <span data-ttu-id="df5d3-268">Kör den **Ksetup** verktyg för att konfigurera Kerberos KDC-servern och sfären.</span><span class="sxs-lookup"><span data-stu-id="df5d3-268">Run the **Ksetup** utility to configure the Kerberos KDC server and realm.</span></span>

    <span data-ttu-id="df5d3-269">Datorn måste konfigureras som en medlem i en arbetsgrupp, eftersom en Kerberos-sfär skiljer sig från en Windows-domän.</span><span class="sxs-lookup"><span data-stu-id="df5d3-269">The machine must be configured as a member of a workgroup since a Kerberos realm is different from a Windows domain.</span></span> <span data-ttu-id="df5d3-270">Detta kan uppnås genom att Kerberos-sfären och lägga till en KDC-server på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="df5d3-270">This can be achieved by setting the Kerberos realm and adding a KDC server as follows.</span></span> <span data-ttu-id="df5d3-271">Ersätt *REALM.COM* med din egen respektive sfär efter behov.</span><span class="sxs-lookup"><span data-stu-id="df5d3-271">Replace *REALM.COM* with your own respective realm as needed.</span></span>

            C:> Ksetup /setdomain REALM.COM
            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>

    <span data-ttu-id="df5d3-272">**Starta om** datorn efter körning kommandona 2.</span><span class="sxs-lookup"><span data-stu-id="df5d3-272">**Restart** the machine after executing these 2 commands.</span></span>

2.  <span data-ttu-id="df5d3-273">Verifiera konfigurationen med **Ksetup** kommando.</span><span class="sxs-lookup"><span data-stu-id="df5d3-273">Verify the configuration with **Ksetup** command.</span></span> <span data-ttu-id="df5d3-274">Utdata ska vara som:</span><span class="sxs-lookup"><span data-stu-id="df5d3-274">The output should be like:</span></span>

            C:> Ksetup
            default realm = REALM.COM (external)
            REALM.com:
                kdc = <your_kdc_server_address>

<span data-ttu-id="df5d3-275">**I Azure Data Factory:**</span><span class="sxs-lookup"><span data-stu-id="df5d3-275">**In Azure Data Factory:**</span></span>

* <span data-ttu-id="df5d3-276">Konfigurera anslutningen HDFS med **Windows-autentisering** tillsammans med dina Kerberos-huvudnamn och ett lösenord för att ansluta till HDFS-datakälla.</span><span class="sxs-lookup"><span data-stu-id="df5d3-276">Configure the HDFS connector using **Windows authentication** together with your Kerberos principal name and password to connect to the HDFS data source.</span></span> <span data-ttu-id="df5d3-277">Kontrollera [HDFS länkade tjänstegenskaper](#linked-service-properties) avsnittet konfigurationsinformation.</span><span class="sxs-lookup"><span data-stu-id="df5d3-277">Check [HDFS Linked Service properties](#linked-service-properties) section on configuration details.</span></span>

### <span data-ttu-id="df5d3-278"><a name="kerberos-mutual-trust"></a>Alternativ 2: Aktivera ömsesidigt förtroende mellan Windows-domän och Kerberos-sfär</span><span class="sxs-lookup"><span data-stu-id="df5d3-278"><a name="kerberos-mutual-trust"></a>Option 2: Enable mutual trust between Windows domain and Kerberos realm</span></span>

#### <a name="requirement"></a><span data-ttu-id="df5d3-279">Krav:</span><span class="sxs-lookup"><span data-stu-id="df5d3-279">Requirement:</span></span>
*   <span data-ttu-id="df5d3-280">Gateway-datorn måste ansluta till en Windows-domän.</span><span class="sxs-lookup"><span data-stu-id="df5d3-280">The gateway machine must join a Windows domain.</span></span>
*   <span data-ttu-id="df5d3-281">Du behöver behörighet att uppdatera inställningarna för domänkontrollanten.</span><span class="sxs-lookup"><span data-stu-id="df5d3-281">You need permission to update the domain controller's settings.</span></span>

#### <a name="how-to-configure"></a><span data-ttu-id="df5d3-282">Så här konfigurerar du:</span><span class="sxs-lookup"><span data-stu-id="df5d3-282">How to configure:</span></span>

> [!NOTE]
> <span data-ttu-id="df5d3-283">Ersätt REALM.COM och AD.COM i följande självstudier med respektive sfär och domänkontrollern efter behov.</span><span class="sxs-lookup"><span data-stu-id="df5d3-283">Replace REALM.COM and AD.COM in the following tutorial with your own respective realm and domain controller as needed.</span></span>

<span data-ttu-id="df5d3-284">**På KDC-server:**</span><span class="sxs-lookup"><span data-stu-id="df5d3-284">**On KDC server:**</span></span>

1.  <span data-ttu-id="df5d3-285">Redigera KDC-konfigurationen i **krb5.conf** filen så att KDC litar på Windows-domän som refererar till följande konfigurationsmall.</span><span class="sxs-lookup"><span data-stu-id="df5d3-285">Edit the KDC configuration in **krb5.conf** file to let KDC trust Windows Domain referring to the following configuration template.</span></span> <span data-ttu-id="df5d3-286">Som standard konfigurationen finns på **/etc/krb5.conf**.</span><span class="sxs-lookup"><span data-stu-id="df5d3-286">By default, the configuration is located at **/etc/krb5.conf**.</span></span>

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

  <span data-ttu-id="df5d3-287">**Starta om** KDC-tjänsten efter konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="df5d3-287">**Restart** the KDC service after configuration.</span></span>

2.  <span data-ttu-id="df5d3-288">Förbereda en huvudansvarig med namnet  **krbtgt/REALM.COM@AD.COM**  i KDC-server med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="df5d3-288">Prepare a principal named **krbtgt/REALM.COM@AD.COM** in KDC server with the following command:</span></span>

            Kadmin> addprinc krbtgt/REALM.COM@AD.COM

3.  <span data-ttu-id="df5d3-289">I **hadoop.security.auth_to_local** HDFS tjänstkonfiguration lägger du till `RULE:[1:$1@$0](.*@AD.COM)s/@.*//`.</span><span class="sxs-lookup"><span data-stu-id="df5d3-289">In **hadoop.security.auth_to_local** HDFS service configuration file, add `RULE:[1:$1@$0](.*@AD.COM)s/@.*//`.</span></span>

<span data-ttu-id="df5d3-290">**På domänkontrollanten:**</span><span class="sxs-lookup"><span data-stu-id="df5d3-290">**On domain controller:**</span></span>

1.  <span data-ttu-id="df5d3-291">Kör följande **Ksetup** kommandon för att lägga till en sfär post:</span><span class="sxs-lookup"><span data-stu-id="df5d3-291">Run the following **Ksetup** commands to add a realm entry:</span></span>

            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
            C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM

2.  <span data-ttu-id="df5d3-292">Upprätta förtroende från Windows-domän till Kerberos-sfär.</span><span class="sxs-lookup"><span data-stu-id="df5d3-292">Establish trust from Windows Domain to Kerberos Realm.</span></span> <span data-ttu-id="df5d3-293">[lösenord] är lösenordet för objektet  **krbtgt/REALM.COM@AD.COM** .</span><span class="sxs-lookup"><span data-stu-id="df5d3-293">[password] is the password for the principal **krbtgt/REALM.COM@AD.COM**.</span></span>

            C:> netdom trust REALM.COM /Domain: AD.COM /add /realm /passwordt:[password]

3.  <span data-ttu-id="df5d3-294">Välj krypteringsalgoritm som används i Kerberos.</span><span class="sxs-lookup"><span data-stu-id="df5d3-294">Select encryption algorithm used in Kerberos.</span></span>

    1. <span data-ttu-id="df5d3-295">Gå till Serverhanteraren > hantering av Grupprincip > domän > grupprincipobjekt > standard eller aktiva domänprincip och redigera.</span><span class="sxs-lookup"><span data-stu-id="df5d3-295">Go to Server Manager > Group Policy Management > Domain > Group Policy Objects > Default or Active Domain Policy, and Edit.</span></span>

    2. <span data-ttu-id="df5d3-296">I den **Redigeraren för Grupprinciphantering** popup-fönster, gå till Datorkonfiguration > Principer > Windows-inställningar > säkerhetsinställningar > lokala principer > säkerhetsalternativ, och konfigurera **Nätverkssäkerhet: Konfigurera krypteringstyper tillåts för Kerberos**.</span><span class="sxs-lookup"><span data-stu-id="df5d3-296">In the **Group Policy Management Editor** popup window, go to Computer Configuration > Policies > Windows Settings > Security Settings > Local Policies > Security Options, and configure **Network security: Configure Encryption types allowed for Kerberos**.</span></span>

    3. <span data-ttu-id="df5d3-297">Välj den krypteringsalgoritm som du vill använda när du ansluter till KDC.</span><span class="sxs-lookup"><span data-stu-id="df5d3-297">Select the encryption algorithm you want to use when connect to KDC.</span></span> <span data-ttu-id="df5d3-298">Ofta, kan du bara välja alla alternativ.</span><span class="sxs-lookup"><span data-stu-id="df5d3-298">Commonly, you can simply select all the options.</span></span>

        ![Config krypteringstyper för Kerberos](media/data-factory-hdfs-connector/config-encryption-types-for-kerberos.png)

    4. <span data-ttu-id="df5d3-300">Använd **Ksetup** kommando för att ange krypteringsalgoritmen som ska användas på den specifika SFÄREN.</span><span class="sxs-lookup"><span data-stu-id="df5d3-300">Use **Ksetup** command to specify the encryption algorithm to be used on the specific REALM.</span></span>

                C:> ksetup /SetEncTypeAttr REALM.COM DES-CBC-CRC DES-CBC-MD5 RC4-HMAC-MD5 AES128-CTS-HMAC-SHA1-96 AES256-CTS-HMAC-SHA1-96

4.  <span data-ttu-id="df5d3-301">Skapa mappning mellan domänkontot och Kerberos-huvudnamn för att kunna använda Kerberos-huvudnamn i Windows-domän.</span><span class="sxs-lookup"><span data-stu-id="df5d3-301">Create the mapping between the domain account and Kerberos principal, in order to use Kerberos principal in Windows Domain.</span></span>

    1. <span data-ttu-id="df5d3-302">Starta administrativa verktyg > **Active Directory-användare och datorer**.</span><span class="sxs-lookup"><span data-stu-id="df5d3-302">Start the Administrative tools > **Active Directory Users and Computers**.</span></span>

    2. <span data-ttu-id="df5d3-303">Konfigurera avancerade funktioner genom att klicka på **visa** > **avancerade funktioner**.</span><span class="sxs-lookup"><span data-stu-id="df5d3-303">Configure advanced features by clicking **View** > **Advanced Features**.</span></span>

    3. <span data-ttu-id="df5d3-304">Leta upp det konto som du vill skapa mappningar och högerklicka om du vill visa **namnmappningar** > klickar du på **Kerberos-namn** fliken.</span><span class="sxs-lookup"><span data-stu-id="df5d3-304">Locate the account to which you want to create mappings, and right-click to view **Name Mappings** > click **Kerberos Names** tab.</span></span>

    4. <span data-ttu-id="df5d3-305">Lägga till en huvudansvarig från domänen.</span><span class="sxs-lookup"><span data-stu-id="df5d3-305">Add a principal from the realm.</span></span>

        ![Mappa säkerhetsidentitet](media/data-factory-hdfs-connector/map-security-identity.png)

<span data-ttu-id="df5d3-307">**På gateway-datorn:**</span><span class="sxs-lookup"><span data-stu-id="df5d3-307">**On gateway machine:**</span></span>

* <span data-ttu-id="df5d3-308">Kör följande **Ksetup** kommandon för att lägga till en domän-post.</span><span class="sxs-lookup"><span data-stu-id="df5d3-308">Run the following **Ksetup** commands to add a realm entry.</span></span>

            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
            C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM

<span data-ttu-id="df5d3-309">**I Azure Data Factory:**</span><span class="sxs-lookup"><span data-stu-id="df5d3-309">**In Azure Data Factory:**</span></span>

* <span data-ttu-id="df5d3-310">Konfigurera anslutningen HDFS med **Windows-autentisering** tillsammans med din domänkonto eller Kerberos-huvudnamn att ansluta till HDFS-datakälla.</span><span class="sxs-lookup"><span data-stu-id="df5d3-310">Configure the HDFS connector using **Windows authentication** together with either your Domain Account or Kerberos Principal to connect to the HDFS data source.</span></span> <span data-ttu-id="df5d3-311">Kontrollera [HDFS länkade tjänstegenskaper](#linked-service-properties) avsnittet konfigurationsinformation.</span><span class="sxs-lookup"><span data-stu-id="df5d3-311">Check [HDFS Linked Service properties](#linked-service-properties) section on configuration details.</span></span>

> [!NOTE]
> <span data-ttu-id="df5d3-312">Om du vill mappa kolumner från källan dataset till kolumner från sink dataset finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="df5d3-312">To map columns from source dataset to columns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>


## <a name="performance-and-tuning"></a><span data-ttu-id="df5d3-313">Prestanda och finjustering</span><span class="sxs-lookup"><span data-stu-id="df5d3-313">Performance and Tuning</span></span>
<span data-ttu-id="df5d3-314">Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) vill veta mer om viktiga faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt att optimera den.</span><span class="sxs-lookup"><span data-stu-id="df5d3-314">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
