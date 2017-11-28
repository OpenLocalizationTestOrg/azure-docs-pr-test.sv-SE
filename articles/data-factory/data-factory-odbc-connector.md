---
title: "aaaMove data från ODBC datalager | Microsoft Docs"
description: "Läs mer om hur toomove data från ODBC data lagras med Azure Data Factory."
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
ms.openlocfilehash: bf96e71da449313b6144bb194205c572d2ca2030
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-odbc-data-stores-using-azure-data-factory"></a><span data-ttu-id="a58c6-103">Flytta data från ODBC datalager med Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="a58c6-103">Move data From ODBC data stores using Azure Data Factory</span></span>
<span data-ttu-id="a58c6-104">Den här artikeln förklarar hur toouse hello Kopieringsaktiviteten i Azure Data Factory toomove data från ett lokalt ODBC lagrar.</span><span class="sxs-lookup"><span data-stu-id="a58c6-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data from an on-premises ODBC data store.</span></span> <span data-ttu-id="a58c6-105">Den bygger på hello [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som ger en allmän översikt över dataflyttning hello kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="a58c6-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>

<span data-ttu-id="a58c6-106">Du kan kopiera data från ett ODBC data store tooany stöds sink dataarkiv.</span><span class="sxs-lookup"><span data-stu-id="a58c6-106">You can copy data from an ODBC data store tooany supported sink data store.</span></span> <span data-ttu-id="a58c6-107">En lista över data lagras som stöds när egenskaperna av hello kopieringsaktiviteten Se hello [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabell.</span><span class="sxs-lookup"><span data-stu-id="a58c6-107">For a list of data stores supported as sinks by hello copy activity, see hello [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="a58c6-108">Data factory stöder för närvarande endast flytta data från ett ODBC-lagra tooother datalager, men inte för att flytta data från andra lagrar tooan ODBC data dataarkiv.</span><span class="sxs-lookup"><span data-stu-id="a58c6-108">Data factory currently supports only moving data from an ODBC data store tooother data stores, but not for moving data from other data stores tooan ODBC data store.</span></span> 

## <a name="enabling-connectivity"></a><span data-ttu-id="a58c6-109">Aktivera anslutning</span><span class="sxs-lookup"><span data-stu-id="a58c6-109">Enabling connectivity</span></span>
<span data-ttu-id="a58c6-110">Data Factory-tjänsten stöder anslutande tooon lokala ODBC källor med hello Data Management Gateway.</span><span class="sxs-lookup"><span data-stu-id="a58c6-110">Data Factory service supports connecting tooon-premises ODBC sources using hello Data Management Gateway.</span></span> <span data-ttu-id="a58c6-111">Se [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel toolearn om Data Management Gateway och stegvisa instruktioner om hur du konfigurerar hello gateway.</span><span class="sxs-lookup"><span data-stu-id="a58c6-111">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article toolearn about Data Management Gateway and step-by-step instructions on setting up hello gateway.</span></span> <span data-ttu-id="a58c6-112">Använda hello gateway tooconnect tooan ODBC datalagret även om den finns i en Azure IaaS-VM.</span><span class="sxs-lookup"><span data-stu-id="a58c6-112">Use hello gateway tooconnect tooan ODBC data store even if it is hosted in an Azure IaaS VM.</span></span>

<span data-ttu-id="a58c6-113">Du kan installera hello gateway på hello samma lokala datorn eller hello Azure VM som hello ODBC-datalager.</span><span class="sxs-lookup"><span data-stu-id="a58c6-113">You can install hello gateway on hello same on-premises machine or hello Azure VM as hello ODBC data store.</span></span> <span data-ttu-id="a58c6-114">Vi rekommenderar dock att du installerar hello gateway på en separat dator/Azure IaaS-VM tooavoid resurskonflikter och bättre prestanda.</span><span class="sxs-lookup"><span data-stu-id="a58c6-114">However, we recommend that you install hello gateway on a separate machine/Azure IaaS VM tooavoid resource contention and for better performance.</span></span> <span data-ttu-id="a58c6-115">När du installerar hello gateway på en separat dator ska hello datorn kunna tooaccess hello dator med hello ODBC-datalagret.</span><span class="sxs-lookup"><span data-stu-id="a58c6-115">When you install hello gateway on a separate machine, hello machine should be able tooaccess hello machine with hello ODBC data store.</span></span>

<span data-ttu-id="a58c6-116">Förutom hello Data Management Gateway måste du också tooinstall hello ODBC-drivrutinen för hello datalager på hello gateway-datorn.</span><span class="sxs-lookup"><span data-stu-id="a58c6-116">Apart from hello Data Management Gateway, you also need tooinstall hello ODBC driver for hello data store on hello gateway machine.</span></span>

> [!NOTE]
> <span data-ttu-id="a58c6-117">Se [felsökning av problem med gateway](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) tips om hur du felsöker anslutning /-gateway relaterade problem.</span><span class="sxs-lookup"><span data-stu-id="a58c6-117">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>

## <a name="getting-started"></a><span data-ttu-id="a58c6-118">Komma igång</span><span class="sxs-lookup"><span data-stu-id="a58c6-118">Getting started</span></span>
<span data-ttu-id="a58c6-119">Du kan skapa en pipeline med en kopia-aktivitet som flyttar data från en ODBC-datalagret med hjälp av olika verktyg/API: er.</span><span class="sxs-lookup"><span data-stu-id="a58c6-119">You can create a pipeline with a copy activity that moves data from an ODBC data store by using different tools/APIs.</span></span>

<span data-ttu-id="a58c6-120">hello enklaste sättet toocreate en pipeline är toouse hello **guiden Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="a58c6-120">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="a58c6-121">Se [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden för hello kopiera data.</span><span class="sxs-lookup"><span data-stu-id="a58c6-121">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span>

<span data-ttu-id="a58c6-122">Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall** , **.NET API**, och **REST API**.</span><span class="sxs-lookup"><span data-stu-id="a58c6-122">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="a58c6-123">Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="a58c6-123">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="a58c6-124">Om du använder hello verktyg eller API: er kan utföra du hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalagret:</span><span class="sxs-lookup"><span data-stu-id="a58c6-124">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span> 

1. <span data-ttu-id="a58c6-125">Skapa **länkade tjänster** toolink indata och utdata lagrar tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="a58c6-125">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="a58c6-126">Skapa **datauppsättningar** toorepresent indata och utdata för hello kopieringsåtgärden.</span><span class="sxs-lookup"><span data-stu-id="a58c6-126">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="a58c6-127">Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata.</span><span class="sxs-lookup"><span data-stu-id="a58c6-127">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="a58c6-128">När du använder guiden hello skapas automatiskt JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och hello pipeline) för dig.</span><span class="sxs-lookup"><span data-stu-id="a58c6-128">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="a58c6-129">När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av hello JSON-format.</span><span class="sxs-lookup"><span data-stu-id="a58c6-129">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="a58c6-130">Ett exempel med JSON-definitioner för Data Factory-entiteter som ska använda toocopy data från en ODBC-datalagret finns [JSON-exempel: kopieringsdata från ODBC data lagra tooAzure Blob](#json-example-copy-data-from-odbc-data-store-to-azure-blob) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="a58c6-130">For a sample with JSON definitions for Data Factory entities that are used toocopy data from an ODBC data store, see [JSON example: Copy data from ODBC data store tooAzure Blob](#json-example-copy-data-from-odbc-data-store-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="a58c6-131">hello följande avsnitt innehåller information om JSON-egenskaper används toodefine Data Factory entiteter specifika tooODBC datalager:</span><span class="sxs-lookup"><span data-stu-id="a58c6-131">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooODBC data store:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="a58c6-132">Länkad tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="a58c6-132">Linked service properties</span></span>
<span data-ttu-id="a58c6-133">hello följande tabell innehåller en beskrivning för JSON-element specifika tooODBC länkad tjänst.</span><span class="sxs-lookup"><span data-stu-id="a58c6-133">hello following table provides description for JSON elements specific tooODBC linked service.</span></span>

| <span data-ttu-id="a58c6-134">Egenskap</span><span class="sxs-lookup"><span data-stu-id="a58c6-134">Property</span></span> | <span data-ttu-id="a58c6-135">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a58c6-135">Description</span></span> | <span data-ttu-id="a58c6-136">Krävs</span><span class="sxs-lookup"><span data-stu-id="a58c6-136">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a58c6-137">typ</span><span class="sxs-lookup"><span data-stu-id="a58c6-137">type</span></span> |<span data-ttu-id="a58c6-138">hello Typegenskapen måste anges till: **OnPremisesOdbc**</span><span class="sxs-lookup"><span data-stu-id="a58c6-138">hello type property must be set to: **OnPremisesOdbc**</span></span> |<span data-ttu-id="a58c6-139">Ja</span><span class="sxs-lookup"><span data-stu-id="a58c6-139">Yes</span></span> |
| <span data-ttu-id="a58c6-140">connectionString</span><span class="sxs-lookup"><span data-stu-id="a58c6-140">connectionString</span></span> |<span data-ttu-id="a58c6-141">hello-access credential del av hello anslutningssträngen och en valfri krypteras autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="a58c6-141">hello non-access credential portion of hello connection string and an optional encrypted credential.</span></span> <span data-ttu-id="a58c6-142">Se exemplen i följande avsnitt hello.</span><span class="sxs-lookup"><span data-stu-id="a58c6-142">See examples in hello following sections.</span></span> |<span data-ttu-id="a58c6-143">Ja</span><span class="sxs-lookup"><span data-stu-id="a58c6-143">Yes</span></span> |
| <span data-ttu-id="a58c6-144">autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="a58c6-144">credential</span></span> |<span data-ttu-id="a58c6-145">hello access credential delen av hello anslutningssträngen som angetts i drivrutinsspecifika egenskapsvärdet format.</span><span class="sxs-lookup"><span data-stu-id="a58c6-145">hello access credential portion of hello connection string specified in driver-specific property-value format.</span></span> <span data-ttu-id="a58c6-146">Exempel ”: Uid =<user ID>; Pwd =<password>; RefreshToken =<secret refresh token>”;.</span><span class="sxs-lookup"><span data-stu-id="a58c6-146">Example: “Uid=<user ID>;Pwd=<password>;RefreshToken=<secret refresh token>;”.</span></span> |<span data-ttu-id="a58c6-147">Nej</span><span class="sxs-lookup"><span data-stu-id="a58c6-147">No</span></span> |
| <span data-ttu-id="a58c6-148">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="a58c6-148">authenticationType</span></span> |<span data-ttu-id="a58c6-149">Typ av autentisering används tooconnect toohello ODBC-datalagret.</span><span class="sxs-lookup"><span data-stu-id="a58c6-149">Type of authentication used tooconnect toohello ODBC data store.</span></span> <span data-ttu-id="a58c6-150">Möjliga värden är: anonyma och grundläggande.</span><span class="sxs-lookup"><span data-stu-id="a58c6-150">Possible values are: Anonymous and Basic.</span></span> |<span data-ttu-id="a58c6-151">Ja</span><span class="sxs-lookup"><span data-stu-id="a58c6-151">Yes</span></span> |
| <span data-ttu-id="a58c6-152">användarnamn</span><span class="sxs-lookup"><span data-stu-id="a58c6-152">username</span></span> |<span data-ttu-id="a58c6-153">Ange användarnamnet om du använder grundläggande autentisering.</span><span class="sxs-lookup"><span data-stu-id="a58c6-153">Specify user name if you are using Basic authentication.</span></span> |<span data-ttu-id="a58c6-154">Nej</span><span class="sxs-lookup"><span data-stu-id="a58c6-154">No</span></span> |
| <span data-ttu-id="a58c6-155">lösenord</span><span class="sxs-lookup"><span data-stu-id="a58c6-155">password</span></span> |<span data-ttu-id="a58c6-156">Ange lösenord för hello-användarkonto som du angav för hello användarnamn.</span><span class="sxs-lookup"><span data-stu-id="a58c6-156">Specify password for hello user account you specified for hello username.</span></span> |<span data-ttu-id="a58c6-157">Nej</span><span class="sxs-lookup"><span data-stu-id="a58c6-157">No</span></span> |
| <span data-ttu-id="a58c6-158">gatewayName</span><span class="sxs-lookup"><span data-stu-id="a58c6-158">gatewayName</span></span> |<span data-ttu-id="a58c6-159">Namnet på hello-gateway som hello Data Factory-tjänsten ska använda tooconnect toohello ODBC-datalagret.</span><span class="sxs-lookup"><span data-stu-id="a58c6-159">Name of hello gateway that hello Data Factory service should use tooconnect toohello ODBC data store.</span></span> |<span data-ttu-id="a58c6-160">Ja</span><span class="sxs-lookup"><span data-stu-id="a58c6-160">Yes</span></span> |

### <a name="using-basic-authentication"></a><span data-ttu-id="a58c6-161">Med grundläggande autentisering</span><span class="sxs-lookup"><span data-stu-id="a58c6-161">Using Basic authentication</span></span>

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
### <a name="using-basic-authentication-with-encrypted-credentials"></a><span data-ttu-id="a58c6-162">Med grundläggande autentisering och krypterade autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="a58c6-162">Using Basic authentication with encrypted credentials</span></span>
<span data-ttu-id="a58c6-163">Du kan kryptera hello autentiseringsuppgifterna med hjälp av hello [ny AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) (version 1.0 av Azure PowerShell) cmdlet eller [ny AzureDataFactoryEncryptValue](https://msdn.microsoft.com/library/dn834940.aspx) (0,9 eller tidigare version av hello Azure PowerShell).</span><span class="sxs-lookup"><span data-stu-id="a58c6-163">You can encrypt hello credentials using hello [New-AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) (1.0 version of Azure PowerShell) cmdlet or [New-AzureDataFactoryEncryptValue](https://msdn.microsoft.com/library/dn834940.aspx) (0.9 or earlier version of hello Azure PowerShell).</span></span>  

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

### <a name="using-anonymous-authentication"></a><span data-ttu-id="a58c6-164">Använder anonym autentisering</span><span class="sxs-lookup"><span data-stu-id="a58c6-164">Using Anonymous authentication</span></span>

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


## <a name="dataset-properties"></a><span data-ttu-id="a58c6-165">Egenskaper för datamängd</span><span class="sxs-lookup"><span data-stu-id="a58c6-165">Dataset properties</span></span>
<span data-ttu-id="a58c6-166">En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns hello [skapa datauppsättningar](data-factory-create-datasets.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="a58c6-166">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="a58c6-167">Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av dataset (Azure SQL Azure blob, Azure-tabellen, osv.).</span><span class="sxs-lookup"><span data-stu-id="a58c6-167">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="a58c6-168">Hej **typeProperties** avsnitt är olika för varje typ av dataset och ger information om hello platsen för hello data i datalagret hello.</span><span class="sxs-lookup"><span data-stu-id="a58c6-168">hello **typeProperties** section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="a58c6-169">Hej typeProperties avsnittet för dataset av typen **RelationalTable** (som innefattar ODBC dataset) har hello följande egenskaper</span><span class="sxs-lookup"><span data-stu-id="a58c6-169">hello typeProperties section for dataset of type **RelationalTable** (which includes ODBC dataset) has hello following properties</span></span>

| <span data-ttu-id="a58c6-170">Egenskap</span><span class="sxs-lookup"><span data-stu-id="a58c6-170">Property</span></span> | <span data-ttu-id="a58c6-171">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a58c6-171">Description</span></span> | <span data-ttu-id="a58c6-172">Krävs</span><span class="sxs-lookup"><span data-stu-id="a58c6-172">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a58c6-173">tableName</span><span class="sxs-lookup"><span data-stu-id="a58c6-173">tableName</span></span> |<span data-ttu-id="a58c6-174">Namnet på hello tabell i hello ODBC-datalagret.</span><span class="sxs-lookup"><span data-stu-id="a58c6-174">Name of hello table in hello ODBC data store.</span></span> |<span data-ttu-id="a58c6-175">Ja</span><span class="sxs-lookup"><span data-stu-id="a58c6-175">Yes</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="a58c6-176">Kopiera egenskaper för aktivitet</span><span class="sxs-lookup"><span data-stu-id="a58c6-176">Copy activity properties</span></span>
<span data-ttu-id="a58c6-177">En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns hello [skapar Pipelines](data-factory-create-pipelines.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="a58c6-177">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="a58c6-178">Egenskaper som namn, beskrivning, ingående och utgående tabeller och principer är tillgängliga för alla typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="a58c6-178">Properties such as name, description, input and output tables, and policies are available for all types of activities.</span></span>

<span data-ttu-id="a58c6-179">Egenskaper som är tillgängliga i hello **typeProperties** avsnittet hello aktivitet på hello andra sidan varierar med varje aktivitetstyp.</span><span class="sxs-lookup"><span data-stu-id="a58c6-179">Properties available in hello **typeProperties** section of hello activity on hello other hand vary with each activity type.</span></span> <span data-ttu-id="a58c6-180">För Kopieringsaktivitet kan varierar de beroende på hello typer av datakällor och sänkor.</span><span class="sxs-lookup"><span data-stu-id="a58c6-180">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="a58c6-181">I en Kopieringsaktivitet när datakällan är av typen **RelationalSource** (vilket innefattar ODBC), hello följande egenskaper finns i avsnittet typeProperties:</span><span class="sxs-lookup"><span data-stu-id="a58c6-181">In copy activity, when source is of type **RelationalSource** (which includes ODBC), hello following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="a58c6-182">Egenskap</span><span class="sxs-lookup"><span data-stu-id="a58c6-182">Property</span></span> | <span data-ttu-id="a58c6-183">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a58c6-183">Description</span></span> | <span data-ttu-id="a58c6-184">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="a58c6-184">Allowed values</span></span> | <span data-ttu-id="a58c6-185">Krävs</span><span class="sxs-lookup"><span data-stu-id="a58c6-185">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="a58c6-186">DocumentDB</span><span class="sxs-lookup"><span data-stu-id="a58c6-186">query</span></span> |<span data-ttu-id="a58c6-187">Använda hello anpassad fråga tooread data.</span><span class="sxs-lookup"><span data-stu-id="a58c6-187">Use hello custom query tooread data.</span></span> |<span data-ttu-id="a58c6-188">SQL-sträng.</span><span class="sxs-lookup"><span data-stu-id="a58c6-188">SQL query string.</span></span> <span data-ttu-id="a58c6-189">Till exempel: Välj * från mytable prefix.</span><span class="sxs-lookup"><span data-stu-id="a58c6-189">For example: select * from MyTable.</span></span> |<span data-ttu-id="a58c6-190">Ja</span><span class="sxs-lookup"><span data-stu-id="a58c6-190">Yes</span></span> |


## <a name="json-example-copy-data-from-odbc-data-store-tooazure-blob"></a><span data-ttu-id="a58c6-191">JSON-exempel: kopieringsdata från ODBC data lagra tooAzure Blob</span><span class="sxs-lookup"><span data-stu-id="a58c6-191">JSON example: Copy data from ODBC data store tooAzure Blob</span></span>
<span data-ttu-id="a58c6-192">Det här exemplet innehåller definitioner av JSON som du kan använda toocreate en pipeline med hjälp av [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="a58c6-192">This example provides JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="a58c6-193">Den visar hur toocopy från en ODBC-datakällan tooan Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="a58c6-193">It shows how toocopy data from an ODBC source tooan Azure Blob Storage.</span></span> <span data-ttu-id="a58c6-194">Data kan dock vara kopierade tooany av hello sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hello Kopieringsaktiviteten i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="a58c6-194">However, data can be copied tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>

<span data-ttu-id="a58c6-195">hello exemplet har hello följande data factory-enheter:</span><span class="sxs-lookup"><span data-stu-id="a58c6-195">hello sample has hello following data factory entities:</span></span>

1. <span data-ttu-id="a58c6-196">En länkad tjänst av typen [OnPremisesOdbc](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="a58c6-196">A linked service of type [OnPremisesOdbc](#linked-service-properties).</span></span>
2. <span data-ttu-id="a58c6-197">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="a58c6-197">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="a58c6-198">Indata [dataset](data-factory-create-datasets.md) av typen [RelationalTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="a58c6-198">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](#dataset-properties).</span></span>
4. <span data-ttu-id="a58c6-199">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="a58c6-199">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="a58c6-200">En [pipeline](data-factory-create-pipelines.md) med Kopieringsaktiviteten som använder [RelationalSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="a58c6-200">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="a58c6-201">hello exemplet kopierar data från ett frågeresultat i ett ODBC data store tooa blob varje timme.</span><span class="sxs-lookup"><span data-stu-id="a58c6-201">hello sample copies data from a query result in an ODBC data store tooa blob every hour.</span></span> <span data-ttu-id="a58c6-202">hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a58c6-202">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="a58c6-203">Som ett första steg bör du ställa in hello data management gateway.</span><span class="sxs-lookup"><span data-stu-id="a58c6-203">As a first step, set up hello data management gateway.</span></span> <span data-ttu-id="a58c6-204">hello anvisningar finns i hello [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="a58c6-204">hello instructions are in hello [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="a58c6-205">**ODBC länkade tjänsten** det här exemplet använder hello grundläggande autentisering.</span><span class="sxs-lookup"><span data-stu-id="a58c6-205">**ODBC linked service** This example uses hello Basic authentication.</span></span> <span data-ttu-id="a58c6-206">Se [ODBC länkade tjänsten](#linked-service-properties) avsnittet för olika typer av autentisering som du kan använda.</span><span class="sxs-lookup"><span data-stu-id="a58c6-206">See [ODBC linked service](#linked-service-properties) section for different types of authentication you can use.</span></span>

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

<span data-ttu-id="a58c6-207">**Länkad Azure Storage-tjänst**</span><span class="sxs-lookup"><span data-stu-id="a58c6-207">**Azure Storage linked service**</span></span>

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

<span data-ttu-id="a58c6-208">**ODBC-inkommande dataset**</span><span class="sxs-lookup"><span data-stu-id="a58c6-208">**ODBC input dataset**</span></span>

<span data-ttu-id="a58c6-209">hello exemplet förutsätter att du har skapat en tabell ”mytable” som prefix i en ODBC-databas och innehåller en kolumn med namnet ”timestampcolumn” för tid series-data.</span><span class="sxs-lookup"><span data-stu-id="a58c6-209">hello sample assumes you have created a table “MyTable” in an ODBC database and it contains a column called “timestampcolumn” for time series data.</span></span>

<span data-ttu-id="a58c6-210">Inställningen ”externa”: ”true” informerar hello Data Factory-tjänsten som hello dataset är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="a58c6-210">Setting “external”: ”true” informs hello Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

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

<span data-ttu-id="a58c6-211">**Azure Blob utdatauppsättningen**</span><span class="sxs-lookup"><span data-stu-id="a58c6-211">**Azure Blob output dataset**</span></span>

<span data-ttu-id="a58c6-212">Data skrivs tooa nya blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="a58c6-212">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="a58c6-213">hello mappsökväg för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="a58c6-213">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="a58c6-214">hello mappsökväg använder år, månad, dag och timmar delar av hello starttid.</span><span class="sxs-lookup"><span data-stu-id="a58c6-214">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

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


<span data-ttu-id="a58c6-215">**Kopiera aktivitet i en pipeline med ODBC-datakällan (RelationalSource) och Blob sink (BlobSink)**</span><span class="sxs-lookup"><span data-stu-id="a58c6-215">**Copy activity in a pipeline with ODBC source (RelationalSource) and Blob sink (BlobSink)**</span></span>

<span data-ttu-id="a58c6-216">hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse dessa indata och utdata-datauppsättningar och schemalagda toorun varje timme.</span><span class="sxs-lookup"><span data-stu-id="a58c6-216">hello pipeline contains a Copy Activity that is configured toouse these input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="a58c6-217">I hello pipeline JSON-definitionen hello **källa** typ har angetts för**RelationalSource** och **sink** typ har angetts för**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="a58c6-217">In hello pipeline JSON definition, hello **source** type is set too**RelationalSource** and **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="a58c6-218">hello SQL-frågan som angetts för hello **frågan** egenskapen väljer hello data i hello tidigare timme toocopy.</span><span class="sxs-lookup"><span data-stu-id="a58c6-218">hello SQL query specified for hello **query** property selects hello data in hello past hour toocopy.</span></span>

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
### <a name="type-mapping-for-odbc"></a><span data-ttu-id="a58c6-219">Mappning för ODBC</span><span class="sxs-lookup"><span data-stu-id="a58c6-219">Type mapping for ODBC</span></span>
<span data-ttu-id="a58c6-220">Som anges i hello [data movement aktiviteter](data-factory-data-movement-activities.md) artikeln kopieringsaktiviteten utför automatisk konverteringar från källan typer toosink typer med hello följande två sätt:</span><span class="sxs-lookup"><span data-stu-id="a58c6-220">As mentioned in hello [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types toosink types with hello following two-step approach:</span></span>

1. <span data-ttu-id="a58c6-221">Konvertera från inbyggda typer too.NET källtypen</span><span class="sxs-lookup"><span data-stu-id="a58c6-221">Convert from native source types too.NET type</span></span>
2. <span data-ttu-id="a58c6-222">Konvertera från .NET typen toonative Mottagartypen</span><span class="sxs-lookup"><span data-stu-id="a58c6-222">Convert from .NET type toonative sink type</span></span>

<span data-ttu-id="a58c6-223">När du flyttar data från ODBC datalager ODBC-datatyper är mappade too.NET typer som anges i hello [ODBC mappningar av datatyper](https://msdn.microsoft.com/library/cc668763.aspx) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="a58c6-223">When moving data from ODBC data stores, ODBC data types are mapped too.NET types as mentioned in hello [ODBC Data Type Mappings](https://msdn.microsoft.com/library/cc668763.aspx) topic.</span></span>

## <a name="map-source-toosink-columns"></a><span data-ttu-id="a58c6-224">Mappa källkolumner toosink</span><span class="sxs-lookup"><span data-stu-id="a58c6-224">Map source toosink columns</span></span>
<span data-ttu-id="a58c6-225">toolearn mappning tabellkolumner i källan dataset toocolumns i sink dataset finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="a58c6-225">toolearn about mapping columns in source dataset toocolumns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="a58c6-226">Upprepbar läsning från relationella källor</span><span class="sxs-lookup"><span data-stu-id="a58c6-226">Repeatable read from relational sources</span></span>
<span data-ttu-id="a58c6-227">När du kopierar data från relationella datalager, Kom ihåg tooavoid repeterbarhet oönskade resultat.</span><span class="sxs-lookup"><span data-stu-id="a58c6-227">When copying data from relational data stores, keep repeatability in mind tooavoid unintended outcomes.</span></span> <span data-ttu-id="a58c6-228">I Azure Data Factory, kan du köra en sektor manuellt.</span><span class="sxs-lookup"><span data-stu-id="a58c6-228">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="a58c6-229">Du kan också konfigurera i principen för en dataset så att ett segment som körs när ett fel uppstår.</span><span class="sxs-lookup"><span data-stu-id="a58c6-229">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="a58c6-230">När ett segment körs på antingen sätt måste toomake att som hello samma data läses oavsett hur många gånger ett segment körs.</span><span class="sxs-lookup"><span data-stu-id="a58c6-230">When a slice is rerun in either way, you need toomake sure that hello same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="a58c6-231">Se [Repeatable läsa från relationella källor](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="a58c6-231">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="ge-historian-store"></a><span data-ttu-id="a58c6-232">GE Historian store</span><span class="sxs-lookup"><span data-stu-id="a58c6-232">GE Historian store</span></span>
<span data-ttu-id="a58c6-233">Du skapar en ODBC-länkad tjänst toolink en [GE Proficy Historian (nu GE Historian)](http://www.geautomation.com/products/proficy-historian) tooan Azure data factory datalager som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="a58c6-233">You create an ODBC linked service toolink a [GE Proficy Historian (now GE Historian)](http://www.geautomation.com/products/proficy-historian) data store tooan Azure data factory as shown in hello following example:</span></span>

```json
{
    "name": "HistorianLinkedService",
    "properties":
    {
        "type": "OnPremisesOdbc",
        "typeProperties":
        {
            "connectionString": "DSN=<name of hello GE Historian store>",
            "gatewayName": "<gateway name>",
            "authenticationType": "Basic",
            "userName": "<user name>",
            "password": "<password>"
        }
    }
}
```

<span data-ttu-id="a58c6-234">Installera Data Management Gateway på en lokal dator och registrera hello gateway med hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="a58c6-234">Install Data Management Gateway on an on-premises machine and register hello gateway with hello portal.</span></span> <span data-ttu-id="a58c6-235">hello gateway som har installerats på datorn lokalt använder hello ODBC-drivrutinen för att GE Historian tooconnect toohello GE Historian datalagret.</span><span class="sxs-lookup"><span data-stu-id="a58c6-235">hello gateway installed on your on-premises computer uses hello ODBC driver for GE Historian tooconnect toohello GE Historian data store.</span></span> <span data-ttu-id="a58c6-236">Installera drivrutinen för hello därför om det redan inte är installerad på hello gateway-datorn.</span><span class="sxs-lookup"><span data-stu-id="a58c6-236">Therefore, install hello driver if it is not already installed on hello gateway machine.</span></span> <span data-ttu-id="a58c6-237">Se [aktivera anslutningen](#enabling-connectivity) information.</span><span class="sxs-lookup"><span data-stu-id="a58c6-237">See [Enabling connectivity](#enabling-connectivity) section for details.</span></span>

<span data-ttu-id="a58c6-238">Innan du använder hello GE Historian lagra i en Data Factory-lösning kan du kontrollera om hello gateway kan ansluta toohello datalagret enligt anvisningarna i nästa avsnitt av hello.</span><span class="sxs-lookup"><span data-stu-id="a58c6-238">Before you use hello GE Historian store in a Data Factory solution, verify whether hello gateway can connect toohello data store using instructions in hello next section.</span></span>

<span data-ttu-id="a58c6-239">Läs hello artikel från början hello en detaljerad översikt av med hjälp av ODBC data lagras som källa för datalager i en kopieringsåtgärd.</span><span class="sxs-lookup"><span data-stu-id="a58c6-239">Read hello article from hello beginning for a detailed overview of using ODBC data stores as source data stores in a copy operation.</span></span>  

## <a name="troubleshoot-connectivity-issues"></a><span data-ttu-id="a58c6-240">Felsökning av problem med nätverksanslutningen</span><span class="sxs-lookup"><span data-stu-id="a58c6-240">Troubleshoot connectivity issues</span></span>
<span data-ttu-id="a58c6-241">tootroubleshoot anslutningsproblem använda hello **diagnostik** fliken **Data Management Gateway Configuration Manager**.</span><span class="sxs-lookup"><span data-stu-id="a58c6-241">tootroubleshoot connection issues, use hello **Diagnostics** tab of **Data Management Gateway Configuration Manager**.</span></span>

1. <span data-ttu-id="a58c6-242">Starta **Data Management Gateway Configuration Manager**.</span><span class="sxs-lookup"><span data-stu-id="a58c6-242">Launch **Data Management Gateway Configuration Manager**.</span></span> <span data-ttu-id="a58c6-243">Du kan antingen köra ”C:\Program Files\Microsoft Data Management Gateway\1.0\Shared\ConfigManager.exe” direkt (eller) sökning för **Gateway** toofind en länk för**Microsoft Data Management Gateway** program som visas i följande bild hello.</span><span class="sxs-lookup"><span data-stu-id="a58c6-243">You can either run "C:\Program Files\Microsoft Data Management Gateway\1.0\Shared\ConfigManager.exe" directly (or) search for **Gateway** toofind a link too**Microsoft Data Management Gateway** application as shown in hello following image.</span></span>

    ![Sök-gateway](./media/data-factory-odbc-connector/search-gateway.png)
2. <span data-ttu-id="a58c6-245">Växla toohello **diagnostik** fliken.</span><span class="sxs-lookup"><span data-stu-id="a58c6-245">Switch toohello **Diagnostics** tab.</span></span>

    ![Gatewaydiagnostik](./media/data-factory-odbc-connector/data-factory-gateway-diagnostics.png)
3. <span data-ttu-id="a58c6-247">Välj hello **typen** av data lagras (länkade tjänst).</span><span class="sxs-lookup"><span data-stu-id="a58c6-247">Select hello **type** of data store (linked service).</span></span>
4. <span data-ttu-id="a58c6-248">Ange **autentisering** och ange **autentiseringsuppgifter** (eller) ange **anslutningssträngen** som har använt tooconnect toohello datalagret.</span><span class="sxs-lookup"><span data-stu-id="a58c6-248">Specify **authentication** and enter **credentials** (or) enter **connection string** that is used tooconnect toohello data store.</span></span>
5. <span data-ttu-id="a58c6-249">Klicka på **Testanslutningen** tootest hello anslutning toohello datalagret.</span><span class="sxs-lookup"><span data-stu-id="a58c6-249">Click **Test connection** tootest hello connection toohello data store.</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="a58c6-250">Prestanda och finjustering</span><span class="sxs-lookup"><span data-stu-id="a58c6-250">Performance and Tuning</span></span>
<span data-ttu-id="a58c6-251">Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) toolearn om nyckeln faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt toooptimize den.</span><span class="sxs-lookup"><span data-stu-id="a58c6-251">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
