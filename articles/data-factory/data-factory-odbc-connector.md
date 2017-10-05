---
title: "Flytta data från ODBC datalager | Microsoft Docs"
description: "Lär dig mer om hur du flyttar data från ODBC datalager med Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: ad70a598-c031-4339-a883-c6125403cb76
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: jingwang
ms.openlocfilehash: 269d9802ca4a6a16dbf9021929fe21104cb431f7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="move-data-from-odbc-data-stores-using-azure-data-factory"></a><span data-ttu-id="0e07c-103">Flytta data från ODBC datalager med Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="0e07c-103">Move data From ODBC data stores using Azure Data Factory</span></span>
<span data-ttu-id="0e07c-104">Den här artikeln förklarar hur du använder aktiviteten kopiera i Azure Data Factory för att flytta data från ett lokalt ODBC-dataarkiv.</span><span class="sxs-lookup"><span data-stu-id="0e07c-104">This article explains how to use the Copy Activity in Azure Data Factory to move data from an on-premises ODBC data store.</span></span> <span data-ttu-id="0e07c-105">Den bygger på den [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som presenterar en allmän översikt över dataflyttning med copy-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="0e07c-105">It builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span>

<span data-ttu-id="0e07c-106">Du kan kopiera data från en ODBC-dataarkiv till alla stöds sink-datalagret.</span><span class="sxs-lookup"><span data-stu-id="0e07c-106">You can copy data from an ODBC data store to any supported sink data store.</span></span> <span data-ttu-id="0e07c-107">En lista över datakällor som stöds som sänkor av kopieringsaktiviteten, finns det [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabell.</span><span class="sxs-lookup"><span data-stu-id="0e07c-107">For a list of data stores supported as sinks by the copy activity, see the [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="0e07c-108">Data factory stöder för närvarande endast flytta data från en ODBC-datalager till andra databaser, men inte för att flytta data från andra datalager till en ODBC-datalagret.</span><span class="sxs-lookup"><span data-stu-id="0e07c-108">Data factory currently supports only moving data from an ODBC data store to other data stores, but not for moving data from other data stores to an ODBC data store.</span></span> 

## <a name="enabling-connectivity"></a><span data-ttu-id="0e07c-109">Aktivera anslutning</span><span class="sxs-lookup"><span data-stu-id="0e07c-109">Enabling connectivity</span></span>
<span data-ttu-id="0e07c-110">Data Factory-tjänsten stöder anslutning till lokala ODBC källor med hjälp av Data Management Gateway.</span><span class="sxs-lookup"><span data-stu-id="0e07c-110">Data Factory service supports connecting to on-premises ODBC sources using the Data Management Gateway.</span></span> <span data-ttu-id="0e07c-111">Se [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikeln innehåller information om Data Management Gateway och stegvisa instruktioner om hur du konfigurerar en gateway.</span><span class="sxs-lookup"><span data-stu-id="0e07c-111">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article to learn about Data Management Gateway and step-by-step instructions on setting up the gateway.</span></span> <span data-ttu-id="0e07c-112">Använda gateway för att ansluta till en ODBC-datalagret, även om den finns i en Azure IaaS-VM.</span><span class="sxs-lookup"><span data-stu-id="0e07c-112">Use the gateway to connect to an ODBC data store even if it is hosted in an Azure IaaS VM.</span></span>

<span data-ttu-id="0e07c-113">Du kan installera gatewayen på samma lokala dator eller Azure VM som ODBC-datalager.</span><span class="sxs-lookup"><span data-stu-id="0e07c-113">You can install the gateway on the same on-premises machine or the Azure VM as the ODBC data store.</span></span> <span data-ttu-id="0e07c-114">Vi rekommenderar dock att du installerar gateway på en separat dator/Azure IaaS-VM för att undvika resurskonflikter och för bättre prestanda.</span><span class="sxs-lookup"><span data-stu-id="0e07c-114">However, we recommend that you install the gateway on a separate machine/Azure IaaS VM to avoid resource contention and for better performance.</span></span> <span data-ttu-id="0e07c-115">När du installerar en gateway på en separat dator ska datorn tillgång till datorn med ODBC-datalagret.</span><span class="sxs-lookup"><span data-stu-id="0e07c-115">When you install the gateway on a separate machine, the machine should be able to access the machine with the ODBC data store.</span></span>

<span data-ttu-id="0e07c-116">Förutom Data Management Gateway måste du också installera ODBC-drivrutinen för datalagret på gateway-datorn.</span><span class="sxs-lookup"><span data-stu-id="0e07c-116">Apart from the Data Management Gateway, you also need to install the ODBC driver for the data store on the gateway machine.</span></span>

> [!NOTE]
> <span data-ttu-id="0e07c-117">Se [felsökning av problem med gateway](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) tips om hur du felsöker anslutning /-gateway relaterade problem.</span><span class="sxs-lookup"><span data-stu-id="0e07c-117">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>

## <a name="getting-started"></a><span data-ttu-id="0e07c-118">Komma igång</span><span class="sxs-lookup"><span data-stu-id="0e07c-118">Getting started</span></span>
<span data-ttu-id="0e07c-119">Du kan skapa en pipeline med en kopia-aktivitet som flyttar data från en ODBC-datalagret med hjälp av olika verktyg/API: er.</span><span class="sxs-lookup"><span data-stu-id="0e07c-119">You can create a pipeline with a copy activity that moves data from an ODBC data store by using different tools/APIs.</span></span>

<span data-ttu-id="0e07c-120">Det enklaste sättet att skapa en pipeline är att använda den **guiden Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="0e07c-120">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="0e07c-121">Finns [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden Kopiera data.</span><span class="sxs-lookup"><span data-stu-id="0e07c-121">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span>

<span data-ttu-id="0e07c-122">Du kan också använda följande verktyg för att skapa en pipeline: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall**, **.NET API**, och **REST API**.</span><span class="sxs-lookup"><span data-stu-id="0e07c-122">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="0e07c-123">Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner för att skapa en pipeline med en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="0e07c-123">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> 

<span data-ttu-id="0e07c-124">Om du använder verktyg eller API: er, kan du utföra följande steg för att skapa en pipeline som flyttar data från ett dataarkiv som källa till ett dataarkiv som mottagare:</span><span class="sxs-lookup"><span data-stu-id="0e07c-124">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span> 

1. <span data-ttu-id="0e07c-125">Skapa **länkade tjänster** att länka inkommande och utgående data lagras till din data factory.</span><span class="sxs-lookup"><span data-stu-id="0e07c-125">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="0e07c-126">Skapa **datauppsättningar** att representera inkommande och utgående data för kopieringen.</span><span class="sxs-lookup"><span data-stu-id="0e07c-126">Create **datasets** to represent input and output data for the copy operation.</span></span> 
3. <span data-ttu-id="0e07c-127">Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata.</span><span class="sxs-lookup"><span data-stu-id="0e07c-127">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="0e07c-128">När du använder guiden skapas JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och pipelinen) automatiskt för dig.</span><span class="sxs-lookup"><span data-stu-id="0e07c-128">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="0e07c-129">När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av JSON-format.</span><span class="sxs-lookup"><span data-stu-id="0e07c-129">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="0e07c-130">Ett exempel med JSON-definitioner för Data Factory-entiteter som används för att kopiera data från en ODBC-datalagret finns [JSON-exempel: kopieringsdata från ODBC data lagring till Azure Blob](#json-example-copy-data-from-odbc-data-store-to-azure-blob) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="0e07c-130">For a sample with JSON definitions for Data Factory entities that are used to copy data from an ODBC data store, see [JSON example: Copy data from ODBC data store to Azure Blob](#json-example-copy-data-from-odbc-data-store-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="0e07c-131">Följande avsnitt innehåller information om JSON-egenskaper som används för att definiera Data Factory entiteter till ODBC data store:</span><span class="sxs-lookup"><span data-stu-id="0e07c-131">The following sections provide details about JSON properties that are used to define Data Factory entities specific to ODBC data store:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="0e07c-132">Länkad tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="0e07c-132">Linked service properties</span></span>
<span data-ttu-id="0e07c-133">Följande tabell innehåller beskrivning för JSON-element som är specifika för ODBC länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="0e07c-133">The following table provides description for JSON elements specific to ODBC linked service.</span></span>

| <span data-ttu-id="0e07c-134">Egenskap</span><span class="sxs-lookup"><span data-stu-id="0e07c-134">Property</span></span> | <span data-ttu-id="0e07c-135">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0e07c-135">Description</span></span> | <span data-ttu-id="0e07c-136">Krävs</span><span class="sxs-lookup"><span data-stu-id="0e07c-136">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="0e07c-137">typ</span><span class="sxs-lookup"><span data-stu-id="0e07c-137">type</span></span> |<span data-ttu-id="0e07c-138">Egenskapen type måste anges till: **OnPremisesOdbc**</span><span class="sxs-lookup"><span data-stu-id="0e07c-138">The type property must be set to: **OnPremisesOdbc**</span></span> |<span data-ttu-id="0e07c-139">Ja</span><span class="sxs-lookup"><span data-stu-id="0e07c-139">Yes</span></span> |
| <span data-ttu-id="0e07c-140">connectionString</span><span class="sxs-lookup"><span data-stu-id="0e07c-140">connectionString</span></span> |<span data-ttu-id="0e07c-141">Den icke-autentiseringsuppgifter delen av anslutningssträngen och en valfri krypterade autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="0e07c-141">The non-access credential portion of the connection string and an optional encrypted credential.</span></span> <span data-ttu-id="0e07c-142">Se exemplen i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="0e07c-142">See examples in the following sections.</span></span> |<span data-ttu-id="0e07c-143">Ja</span><span class="sxs-lookup"><span data-stu-id="0e07c-143">Yes</span></span> |
| <span data-ttu-id="0e07c-144">autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="0e07c-144">credential</span></span> |<span data-ttu-id="0e07c-145">Åtkomst autentiseringsuppgifter del av den angivna anslutningssträngen i drivrutinsspecifika egenskapsvärdet format.</span><span class="sxs-lookup"><span data-stu-id="0e07c-145">The access credential portion of the connection string specified in driver-specific property-value format.</span></span> <span data-ttu-id="0e07c-146">Exempel ”: Uid =<user ID>; Pwd =<password>; RefreshToken =<secret refresh token>”;.</span><span class="sxs-lookup"><span data-stu-id="0e07c-146">Example: “Uid=<user ID>;Pwd=<password>;RefreshToken=<secret refresh token>;”.</span></span> |<span data-ttu-id="0e07c-147">Nej</span><span class="sxs-lookup"><span data-stu-id="0e07c-147">No</span></span> |
| <span data-ttu-id="0e07c-148">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="0e07c-148">authenticationType</span></span> |<span data-ttu-id="0e07c-149">Typ av autentisering som används för att ansluta till ODBC-datalagret.</span><span class="sxs-lookup"><span data-stu-id="0e07c-149">Type of authentication used to connect to the ODBC data store.</span></span> <span data-ttu-id="0e07c-150">Möjliga värden är: anonyma och grundläggande.</span><span class="sxs-lookup"><span data-stu-id="0e07c-150">Possible values are: Anonymous and Basic.</span></span> |<span data-ttu-id="0e07c-151">Ja</span><span class="sxs-lookup"><span data-stu-id="0e07c-151">Yes</span></span> |
| <span data-ttu-id="0e07c-152">användarnamn</span><span class="sxs-lookup"><span data-stu-id="0e07c-152">username</span></span> |<span data-ttu-id="0e07c-153">Ange användarnamnet om du använder grundläggande autentisering.</span><span class="sxs-lookup"><span data-stu-id="0e07c-153">Specify user name if you are using Basic authentication.</span></span> |<span data-ttu-id="0e07c-154">Nej</span><span class="sxs-lookup"><span data-stu-id="0e07c-154">No</span></span> |
| <span data-ttu-id="0e07c-155">lösenord</span><span class="sxs-lookup"><span data-stu-id="0e07c-155">password</span></span> |<span data-ttu-id="0e07c-156">Ange lösenordet för det användarkonto som du angav för användarnamnet.</span><span class="sxs-lookup"><span data-stu-id="0e07c-156">Specify password for the user account you specified for the username.</span></span> |<span data-ttu-id="0e07c-157">Nej</span><span class="sxs-lookup"><span data-stu-id="0e07c-157">No</span></span> |
| <span data-ttu-id="0e07c-158">gatewayName</span><span class="sxs-lookup"><span data-stu-id="0e07c-158">gatewayName</span></span> |<span data-ttu-id="0e07c-159">Namnet på den gateway som Data Factory-tjänsten ska använda för att ansluta till ODBC-datalagret.</span><span class="sxs-lookup"><span data-stu-id="0e07c-159">Name of the gateway that the Data Factory service should use to connect to the ODBC data store.</span></span> |<span data-ttu-id="0e07c-160">Ja</span><span class="sxs-lookup"><span data-stu-id="0e07c-160">Yes</span></span> |

### <a name="using-basic-authentication"></a><span data-ttu-id="0e07c-161">Med grundläggande autentisering</span><span class="sxs-lookup"><span data-stu-id="0e07c-161">Using Basic authentication</span></span>

```json
{
    "name": "odbc",
    "properties":
    {
        "type": "OnPremisesOdbc",
        "typeProperties":
        {
            "authenticationType": "Basic",
            "connectionString": "Driver={SQL Server};Server=Server.database.windows.net; Database=TestDatabase;",
            "userName": "username",
            "password": "password",
            "gatewayName": "mygateway"
        }
    }
}
```
### <a name="using-basic-authentication-with-encrypted-credentials"></a><span data-ttu-id="0e07c-162">Med grundläggande autentisering och krypterade autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="0e07c-162">Using Basic authentication with encrypted credentials</span></span>
<span data-ttu-id="0e07c-163">Du kan kryptera autentiseringsuppgifterna med den [ny AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) (version 1.0 av Azure PowerShell) cmdlet eller [ny AzureDataFactoryEncryptValue](https://msdn.microsoft.com/library/dn834940.aspx) (0,9 eller tidigare version av Azure PowerShell).</span><span class="sxs-lookup"><span data-stu-id="0e07c-163">You can encrypt the credentials using the [New-AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) (1.0 version of Azure PowerShell) cmdlet or [New-AzureDataFactoryEncryptValue](https://msdn.microsoft.com/library/dn834940.aspx) (0.9 or earlier version of the Azure PowerShell).</span></span>  

```json
{
    "name": "odbc",
    "properties":
    {
        "type": "OnPremisesOdbc",
        "typeProperties":
        {
            "authenticationType": "Basic",
            "connectionString": "Driver={SQL Server};Server=myserver.database.windows.net; Database=TestDatabase;;EncryptedCredential=eyJDb25uZWN0...........................",
            "gatewayName": "mygateway"
        }
    }
}
```

### <a name="using-anonymous-authentication"></a><span data-ttu-id="0e07c-164">Använder anonym autentisering</span><span class="sxs-lookup"><span data-stu-id="0e07c-164">Using Anonymous authentication</span></span>

```json
{
    "name": "odbc",
    "properties":
    {
        "type": "OnPremisesOdbc",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "connectionString": "Driver={SQL Server};Server={servername}.database.windows.net; Database=TestDatabase;",
            "credential": "UID={uid};PWD={pwd}",
            "gatewayName": "mygateway"
        }
    }
}
```


## <a name="dataset-properties"></a><span data-ttu-id="0e07c-165">Egenskaper för datamängd</span><span class="sxs-lookup"><span data-stu-id="0e07c-165">Dataset properties</span></span>
<span data-ttu-id="0e07c-166">En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns i [skapa datauppsättningar](data-factory-create-datasets.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="0e07c-166">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="0e07c-167">Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av dataset (Azure SQL Azure blob, Azure-tabellen, osv.).</span><span class="sxs-lookup"><span data-stu-id="0e07c-167">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="0e07c-168">Den **typeProperties** avsnitt är olika för varje typ av dataset och innehåller information om placeringen av data i datalagret.</span><span class="sxs-lookup"><span data-stu-id="0e07c-168">The **typeProperties** section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="0e07c-169">TypeProperties avsnittet för dataset av typen **RelationalTable** (som innefattar ODBC dataset) har följande egenskaper</span><span class="sxs-lookup"><span data-stu-id="0e07c-169">The typeProperties section for dataset of type **RelationalTable** (which includes ODBC dataset) has the following properties</span></span>

| <span data-ttu-id="0e07c-170">Egenskap</span><span class="sxs-lookup"><span data-stu-id="0e07c-170">Property</span></span> | <span data-ttu-id="0e07c-171">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0e07c-171">Description</span></span> | <span data-ttu-id="0e07c-172">Krävs</span><span class="sxs-lookup"><span data-stu-id="0e07c-172">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="0e07c-173">tableName</span><span class="sxs-lookup"><span data-stu-id="0e07c-173">tableName</span></span> |<span data-ttu-id="0e07c-174">Namnet på tabellen i ODBC-datakällan.</span><span class="sxs-lookup"><span data-stu-id="0e07c-174">Name of the table in the ODBC data store.</span></span> |<span data-ttu-id="0e07c-175">Ja</span><span class="sxs-lookup"><span data-stu-id="0e07c-175">Yes</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="0e07c-176">Kopiera egenskaper för aktivitet</span><span class="sxs-lookup"><span data-stu-id="0e07c-176">Copy activity properties</span></span>
<span data-ttu-id="0e07c-177">En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns i [skapar Pipelines](data-factory-create-pipelines.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="0e07c-177">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="0e07c-178">Egenskaper som namn, beskrivning, ingående och utgående tabeller och principer är tillgängliga för alla typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="0e07c-178">Properties such as name, description, input and output tables, and policies are available for all types of activities.</span></span>

<span data-ttu-id="0e07c-179">Egenskaper som är tillgängliga i den **typeProperties** avsnitt i aktiviteten å andra sidan varierar med varje aktivitetstyp.</span><span class="sxs-lookup"><span data-stu-id="0e07c-179">Properties available in the **typeProperties** section of the activity on the other hand vary with each activity type.</span></span> <span data-ttu-id="0e07c-180">För Kopieringsaktivitet kan variera de beroende på vilka typer av datakällor och sänkor.</span><span class="sxs-lookup"><span data-stu-id="0e07c-180">For Copy activity, they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="0e07c-181">I en Kopieringsaktivitet när datakällan är av typen **RelationalSource** (vilket innefattar ODBC), följande egenskaper finns i avsnittet typeProperties:</span><span class="sxs-lookup"><span data-stu-id="0e07c-181">In copy activity, when source is of type **RelationalSource** (which includes ODBC), the following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="0e07c-182">Egenskap</span><span class="sxs-lookup"><span data-stu-id="0e07c-182">Property</span></span> | <span data-ttu-id="0e07c-183">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0e07c-183">Description</span></span> | <span data-ttu-id="0e07c-184">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="0e07c-184">Allowed values</span></span> | <span data-ttu-id="0e07c-185">Krävs</span><span class="sxs-lookup"><span data-stu-id="0e07c-185">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="0e07c-186">DocumentDB</span><span class="sxs-lookup"><span data-stu-id="0e07c-186">query</span></span> |<span data-ttu-id="0e07c-187">Använd anpassad fråga för att läsa data.</span><span class="sxs-lookup"><span data-stu-id="0e07c-187">Use the custom query to read data.</span></span> |<span data-ttu-id="0e07c-188">SQL-sträng.</span><span class="sxs-lookup"><span data-stu-id="0e07c-188">SQL query string.</span></span> <span data-ttu-id="0e07c-189">Till exempel: Välj * från mytable prefix.</span><span class="sxs-lookup"><span data-stu-id="0e07c-189">For example: select * from MyTable.</span></span> |<span data-ttu-id="0e07c-190">Ja</span><span class="sxs-lookup"><span data-stu-id="0e07c-190">Yes</span></span> |


## <a name="json-example-copy-data-from-odbc-data-store-to-azure-blob"></a><span data-ttu-id="0e07c-191">JSON-exempel: kopieringsdata från ODBC data lagring till Azure-Blob</span><span class="sxs-lookup"><span data-stu-id="0e07c-191">JSON example: Copy data from ODBC data store to Azure Blob</span></span>
<span data-ttu-id="0e07c-192">Det här exemplet innehåller definitioner av JSON som du kan använda för att skapa en pipeline med hjälp av [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="0e07c-192">This example provides JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="0e07c-193">Den visar hur du kopierar data från en ODBC-datakällan till ett Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="0e07c-193">It shows how to copy data from an ODBC source to an Azure Blob Storage.</span></span> <span data-ttu-id="0e07c-194">Dock datan kan kopieras till någon av sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hjälp av aktiviteten kopiera i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="0e07c-194">However, data can be copied to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>

<span data-ttu-id="0e07c-195">Exemplet har följande data factory enheter:</span><span class="sxs-lookup"><span data-stu-id="0e07c-195">The sample has the following data factory entities:</span></span>

1. <span data-ttu-id="0e07c-196">En länkad tjänst av typen [OnPremisesOdbc](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="0e07c-196">A linked service of type [OnPremisesOdbc](#linked-service-properties).</span></span>
2. <span data-ttu-id="0e07c-197">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="0e07c-197">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="0e07c-198">Indata [dataset](data-factory-create-datasets.md) av typen [RelationalTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="0e07c-198">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](#dataset-properties).</span></span>
4. <span data-ttu-id="0e07c-199">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="0e07c-199">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="0e07c-200">En [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [RelationalSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="0e07c-200">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="0e07c-201">Exemplet kopierar data från ett frågeresultat i ett ODBC-datalager till en blobb varje timme.</span><span class="sxs-lookup"><span data-stu-id="0e07c-201">The sample copies data from a query result in an ODBC data store to a blob every hour.</span></span> <span data-ttu-id="0e07c-202">JSON-egenskaper som används i exemplen beskrivs i exemplen i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="0e07c-202">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="0e07c-203">Som ett första steg bör du ställa in data management gateway.</span><span class="sxs-lookup"><span data-stu-id="0e07c-203">As a first step, set up the data management gateway.</span></span> <span data-ttu-id="0e07c-204">Anvisningarna är i den [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="0e07c-204">The instructions are in the [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="0e07c-205">**ODBC länkade tjänsten** det här exemplet använder grundläggande autentisering.</span><span class="sxs-lookup"><span data-stu-id="0e07c-205">**ODBC linked service** This example uses the Basic authentication.</span></span> <span data-ttu-id="0e07c-206">Se [ODBC länkade tjänsten](#linked-service-properties) avsnittet för olika typer av autentisering som du kan använda.</span><span class="sxs-lookup"><span data-stu-id="0e07c-206">See [ODBC linked service](#linked-service-properties) section for different types of authentication you can use.</span></span>

```json
{
    "name": "OnPremOdbcLinkedService",
    "properties":
    {
        "type": "OnPremisesOdbc",
        "typeProperties":
        {
            "authenticationType": "Basic",
            "connectionString": "Driver={SQL Server};Server=Server.database.windows.net; Database=TestDatabase;",
            "userName": "username",
            "password": "password",
            "gatewayName": "mygateway"
        }
    }
}
```

<span data-ttu-id="0e07c-207">**Länkad Azure Storage-tjänst**</span><span class="sxs-lookup"><span data-stu-id="0e07c-207">**Azure Storage linked service**</span></span>

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

<span data-ttu-id="0e07c-208">**ODBC-inkommande dataset**</span><span class="sxs-lookup"><span data-stu-id="0e07c-208">**ODBC input dataset**</span></span>

<span data-ttu-id="0e07c-209">Exemplet förutsätter att du har skapat en tabell ”mytable” som prefix i en ODBC-databas och innehåller en kolumn med namnet ”timestampcolumn” för tid series-data.</span><span class="sxs-lookup"><span data-stu-id="0e07c-209">The sample assumes you have created a table “MyTable” in an ODBC database and it contains a column called “timestampcolumn” for time series data.</span></span>

<span data-ttu-id="0e07c-210">Inställningen ”externa”: ”true” informerar Data Factory-tjänsten att datamängden är extern till data factory och inte tillverkas av en aktivitet i datafabriken.</span><span class="sxs-lookup"><span data-stu-id="0e07c-210">Setting “external”: ”true” informs the Data Factory service that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span>

```json
{
    "name": "ODBCDataSet",
    "properties": {
        "published": false,
        "type": "RelationalTable",
        "linkedServiceName": "OnPremOdbcLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
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

<span data-ttu-id="0e07c-211">**Azure Blob utdatauppsättningen**</span><span class="sxs-lookup"><span data-stu-id="0e07c-211">**Azure Blob output dataset**</span></span>

<span data-ttu-id="0e07c-212">Data skrivs till en ny blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="0e07c-212">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="0e07c-213">Sökvägen till mappen för blobben utvärderas dynamiskt baserat på starttiden för den sektor som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="0e07c-213">The folder path for the blob is dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="0e07c-214">Mappsökvägen använder år, månad, dag och timmar delar av starttiden.</span><span class="sxs-lookup"><span data-stu-id="0e07c-214">The folder path uses year, month, day, and hours parts of the start time.</span></span>

```json
{
    "name": "AzureBlobOdbcDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/odbc/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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


<span data-ttu-id="0e07c-215">**Kopiera aktivitet i en pipeline med ODBC-datakällan (RelationalSource) och Blob sink (BlobSink)**</span><span class="sxs-lookup"><span data-stu-id="0e07c-215">**Copy activity in a pipeline with ODBC source (RelationalSource) and Blob sink (BlobSink)**</span></span>

<span data-ttu-id="0e07c-216">Pipelinen innehåller en kopia-aktivitet som är konfigurerad för att använda dessa indata och utdata-datauppsättningar och är schemalagd att köras varje timme.</span><span class="sxs-lookup"><span data-stu-id="0e07c-216">The pipeline contains a Copy Activity that is configured to use these input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="0e07c-217">I pipeline-JSON-definitionen av **källa** är inställd på **RelationalSource** och **sink** är inställd på **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="0e07c-217">In the pipeline JSON definition, the **source** type is set to **RelationalSource** and **sink** type is set to **BlobSink**.</span></span> <span data-ttu-id="0e07c-218">SQL-frågan som angetts för den **frågan** egenskapen väljer vilka data under den senaste timmen att kopiera.</span><span class="sxs-lookup"><span data-stu-id="0e07c-218">The SQL query specified for the **query** property selects the data in the past hour to copy.</span></span>

```json
{
    "name": "CopyODBCToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "OdbcDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOdbcDataSet"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "OdbcToBlob"
            }
        ],
        "start": "2016-06-01T18:00:00Z",
        "end": "2016-06-01T19:00:00Z"
    }
}
```
### <a name="type-mapping-for-odbc"></a><span data-ttu-id="0e07c-219">Mappning för ODBC</span><span class="sxs-lookup"><span data-stu-id="0e07c-219">Type mapping for ODBC</span></span>
<span data-ttu-id="0e07c-220">Som anges i den [data movement aktiviteter](data-factory-data-movement-activities.md) artikeln kopieringsaktiviteten utför automatisk konverteringar från källtyper att registrera typer med följande metod i två steg:</span><span class="sxs-lookup"><span data-stu-id="0e07c-220">As mentioned in the [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types to sink types with the following two-step approach:</span></span>

1. <span data-ttu-id="0e07c-221">Konvertera från interna källtyper till .NET-typ</span><span class="sxs-lookup"><span data-stu-id="0e07c-221">Convert from native source types to .NET type</span></span>
2. <span data-ttu-id="0e07c-222">Konvertera från .NET-typ till interna mottagare typ.</span><span class="sxs-lookup"><span data-stu-id="0e07c-222">Convert from .NET type to native sink type</span></span>

<span data-ttu-id="0e07c-223">När du flyttar data från ODBC datalager ODBC-datatyper är mappade till .NET-typer som anges i den [ODBC mappningar av datatyper](https://msdn.microsoft.com/library/cc668763.aspx) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="0e07c-223">When moving data from ODBC data stores, ODBC data types are mapped to .NET types as mentioned in the [ODBC Data Type Mappings](https://msdn.microsoft.com/library/cc668763.aspx) topic.</span></span>

## <a name="map-source-to-sink-columns"></a><span data-ttu-id="0e07c-224">Karta källan till mottagare för kolumner</span><span class="sxs-lookup"><span data-stu-id="0e07c-224">Map source to sink columns</span></span>
<span data-ttu-id="0e07c-225">Mer information om mappning kolumner i datauppsättningen källan till kolumner i datauppsättning mottagare, se [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="0e07c-225">To learn about mapping columns in source dataset to columns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="0e07c-226">Upprepbar läsning från relationella källor</span><span class="sxs-lookup"><span data-stu-id="0e07c-226">Repeatable read from relational sources</span></span>
<span data-ttu-id="0e07c-227">Tänk på att undvika oväntade resultat repeterbarhet när kopiering av data från relationella data lagras.</span><span class="sxs-lookup"><span data-stu-id="0e07c-227">When copying data from relational data stores, keep repeatability in mind to avoid unintended outcomes.</span></span> <span data-ttu-id="0e07c-228">I Azure Data Factory, kan du köra en sektor manuellt.</span><span class="sxs-lookup"><span data-stu-id="0e07c-228">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="0e07c-229">Du kan också konfigurera i principen för en dataset så att ett segment som körs när ett fel uppstår.</span><span class="sxs-lookup"><span data-stu-id="0e07c-229">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="0e07c-230">När ett segment körs på något sätt, måste du kontrollera att samma data läses oavsett hur många gånger ett segment körs.</span><span class="sxs-lookup"><span data-stu-id="0e07c-230">When a slice is rerun in either way, you need to make sure that the same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="0e07c-231">Se [Repeatable läsa från relationella källor](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="0e07c-231">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="ge-historian-store"></a><span data-ttu-id="0e07c-232">GE Historian store</span><span class="sxs-lookup"><span data-stu-id="0e07c-232">GE Historian store</span></span>
<span data-ttu-id="0e07c-233">Du skapar en ODBC-länkad tjänst att länka en [GE Proficy Historian (nu GE Historian)](http://www.geautomation.com/products/proficy-historian) datalager till en Azure data factory som visas i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="0e07c-233">You create an ODBC linked service to link a [GE Proficy Historian (now GE Historian)](http://www.geautomation.com/products/proficy-historian) data store to an Azure data factory as shown in the following example:</span></span>

```json
{
    "name": "HistorianLinkedService",
    "properties":
    {
        "type": "OnPremisesOdbc",
        "typeProperties":
        {
            "connectionString": "DSN=<name of the GE Historian store>",
            "gatewayName": "<gateway name>",
            "authenticationType": "Basic",
            "userName": "<user name>",
            "password": "<password>"
        }
    }
}
```

<span data-ttu-id="0e07c-234">Installera Data Management Gateway på en lokal dator och registrera gatewayen med portalen.</span><span class="sxs-lookup"><span data-stu-id="0e07c-234">Install Data Management Gateway on an on-premises machine and register the gateway with the portal.</span></span> <span data-ttu-id="0e07c-235">Gateway som har installerats på datorn lokalt använder ODBC-drivrutin för GE Historian för att ansluta till datalagret GE Historian.</span><span class="sxs-lookup"><span data-stu-id="0e07c-235">The gateway installed on your on-premises computer uses the ODBC driver for GE Historian to connect to the GE Historian data store.</span></span> <span data-ttu-id="0e07c-236">Därför installera drivrutinen om den redan inte är installerad på gateway-datorn.</span><span class="sxs-lookup"><span data-stu-id="0e07c-236">Therefore, install the driver if it is not already installed on the gateway machine.</span></span> <span data-ttu-id="0e07c-237">Se [aktivera anslutningen](#enabling-connectivity) information.</span><span class="sxs-lookup"><span data-stu-id="0e07c-237">See [Enabling connectivity](#enabling-connectivity) section for details.</span></span>

<span data-ttu-id="0e07c-238">Innan du använder arkivet GE Historian i en Data Factory-lösning bör du kontrollera om gatewayen kan ansluta till datalagret med hjälp av anvisningarna i nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="0e07c-238">Before you use the GE Historian store in a Data Factory solution, verify whether the gateway can connect to the data store using instructions in the next section.</span></span>

<span data-ttu-id="0e07c-239">Läs artikeln från början en detaljerad översikt av med hjälp av ODBC data lagras som källa för datalager i en kopieringsåtgärd.</span><span class="sxs-lookup"><span data-stu-id="0e07c-239">Read the article from the beginning for a detailed overview of using ODBC data stores as source data stores in a copy operation.</span></span>  

## <a name="troubleshoot-connectivity-issues"></a><span data-ttu-id="0e07c-240">Felsökning av problem med nätverksanslutningen</span><span class="sxs-lookup"><span data-stu-id="0e07c-240">Troubleshoot connectivity issues</span></span>
<span data-ttu-id="0e07c-241">Felsökning av anslutningsproblem med använder den **diagnostik** fliken **Data Management Gateway Configuration Manager**.</span><span class="sxs-lookup"><span data-stu-id="0e07c-241">To troubleshoot connection issues, use the **Diagnostics** tab of **Data Management Gateway Configuration Manager**.</span></span>

1. <span data-ttu-id="0e07c-242">Starta **Data Management Gateway Configuration Manager**.</span><span class="sxs-lookup"><span data-stu-id="0e07c-242">Launch **Data Management Gateway Configuration Manager**.</span></span> <span data-ttu-id="0e07c-243">Du kan antingen köra ”C:\Program Files\Microsoft Data Management Gateway\1.0\Shared\ConfigManager.exe” direkt (eller) sökning för **Gateway** att hitta en länk till **Microsoft Data Management Gateway** program som visas i följande bild.</span><span class="sxs-lookup"><span data-stu-id="0e07c-243">You can either run "C:\Program Files\Microsoft Data Management Gateway\1.0\Shared\ConfigManager.exe" directly (or) search for **Gateway** to find a link to **Microsoft Data Management Gateway** application as shown in the following image.</span></span>

    ![Sök-gateway](./media/data-factory-odbc-connector/search-gateway.png)
2. <span data-ttu-id="0e07c-245">Växla till den **diagnostik** fliken.</span><span class="sxs-lookup"><span data-stu-id="0e07c-245">Switch to the **Diagnostics** tab.</span></span>

    ![Gatewaydiagnostik](./media/data-factory-odbc-connector/data-factory-gateway-diagnostics.png)
3. <span data-ttu-id="0e07c-247">Välj den **typen** av data lagras (länkade tjänst).</span><span class="sxs-lookup"><span data-stu-id="0e07c-247">Select the **type** of data store (linked service).</span></span>
4. <span data-ttu-id="0e07c-248">Ange **autentisering** och ange **autentiseringsuppgifter** (eller) ange **anslutningssträngen** som används för att ansluta till datalagret.</span><span class="sxs-lookup"><span data-stu-id="0e07c-248">Specify **authentication** and enter **credentials** (or) enter **connection string** that is used to connect to the data store.</span></span>
5. <span data-ttu-id="0e07c-249">Klicka på **Anslutningstestet** att testa anslutningen till datalagret.</span><span class="sxs-lookup"><span data-stu-id="0e07c-249">Click **Test connection** to test the connection to the data store.</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="0e07c-250">Prestanda och finjustering</span><span class="sxs-lookup"><span data-stu-id="0e07c-250">Performance and Tuning</span></span>
<span data-ttu-id="0e07c-251">Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) vill veta mer om viktiga faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt att optimera den.</span><span class="sxs-lookup"><span data-stu-id="0e07c-251">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
