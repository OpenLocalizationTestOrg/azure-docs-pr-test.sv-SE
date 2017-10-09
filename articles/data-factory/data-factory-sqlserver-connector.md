---
title: "aaaMove data tooand från SQL Server | Microsoft Docs"
description: "Lär dig mer om hur toomove data till/från SQL Server-databas som är lokalt eller i en virtuell Azure-dator med hjälp av Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 864ece28-93b5-4309-9873-b095bbe6fedd
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jingwang
ms.openlocfilehash: f0cccf56a670e62ec893d75052a81eb26d562050
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-tooand-from-sql-server-on-premises-or-on-iaas-azure-vm-using-azure-data-factory"></a><span data-ttu-id="7d388-103">Flytta data tooand från SQL Server lokalt eller på IaaS (Azure VM) med hjälp av Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="7d388-103">Move data tooand from SQL Server on-premises or on IaaS (Azure VM) using Azure Data Factory</span></span>
<span data-ttu-id="7d388-104">Den här artikeln förklarar hur toouse hello Kopieringsaktiviteten i Azure Data Factory toomove data till/från en lokal SQL Server-databas.</span><span class="sxs-lookup"><span data-stu-id="7d388-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data to/from an on-premises SQL Server database.</span></span> <span data-ttu-id="7d388-105">Den bygger på hello [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som ger en allmän översikt över dataflyttning hello kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="7d388-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span> 

## <a name="supported-scenarios"></a><span data-ttu-id="7d388-106">Scenarier som stöds</span><span class="sxs-lookup"><span data-stu-id="7d388-106">Supported scenarios</span></span>
<span data-ttu-id="7d388-107">Du kan kopiera data **från en SQL Server-databas** toohello följande datakällor:</span><span class="sxs-lookup"><span data-stu-id="7d388-107">You can copy data **from a SQL Server database** toohello following data stores:</span></span>

[!INCLUDE [data-factory-supported-sink](../../includes/data-factory-supported-sinks.md)]

<span data-ttu-id="7d388-108">Du kan kopiera data från hello följande datalager **tooa SQL Server-databas**:</span><span class="sxs-lookup"><span data-stu-id="7d388-108">You can copy data from hello following data stores **tooa SQL Server database**:</span></span>

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

## <a name="supported-sql-server-versions"></a><span data-ttu-id="7d388-109">SQL Server-versioner som stöds</span><span class="sxs-lookup"><span data-stu-id="7d388-109">Supported SQL Server versions</span></span>
<span data-ttu-id="7d388-110">Den här connector-stöd för SQL Server som kopierar data från / toohello följande versioner av instansen finns på lokalt eller i Azure IaaS med både SQL-autentisering och Windows-autentisering: SQL Server 2016, SQL Server 2014, SQL Server 2012, SQL Server 2008 R2, SQL Server 2008, SQLServer 2005</span><span class="sxs-lookup"><span data-stu-id="7d388-110">This SQL Server connector support copying data from/toohello following versions of instance hosted on-premises or in Azure IaaS using both SQL authentication and Windows authentication: SQL Server 2016, SQL Server 2014, SQL Server 2012, SQL Server 2008 R2, SQL Server 2008, SQL Server 2005</span></span>

## <a name="enabling-connectivity"></a><span data-ttu-id="7d388-111">Aktivera anslutning</span><span class="sxs-lookup"><span data-stu-id="7d388-111">Enabling connectivity</span></span>
<span data-ttu-id="7d388-112">hello begrepp och steg som krävs för att ansluta med SQL Server finns lokalt eller i Azure IaaS (Infrastructure-as-a-Service) virtuella datorer är hello samma.</span><span class="sxs-lookup"><span data-stu-id="7d388-112">hello concepts and steps needed for connecting with SQL Server hosted on-premises or in Azure IaaS (Infrastructure-as-a-Service) VMs are hello same.</span></span> <span data-ttu-id="7d388-113">I båda fallen måste toouse Data Management Gateway-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="7d388-113">In both cases, you need toouse Data Management Gateway for connectivity.</span></span>

<span data-ttu-id="7d388-114">Se [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel toolearn om Data Management Gateway och stegvisa instruktioner om hur du konfigurerar hello gateway.</span><span class="sxs-lookup"><span data-stu-id="7d388-114">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article toolearn about Data Management Gateway and step-by-step instructions on setting up hello gateway.</span></span> <span data-ttu-id="7d388-115">Ställa in en gateway-instans är ett krav för att ansluta med SQL Server.</span><span class="sxs-lookup"><span data-stu-id="7d388-115">Setting up a gateway instance is a pre-requisite for connecting with SQL Server.</span></span>

<span data-ttu-id="7d388-116">Du kan installera gatewayen hello samma på lokala datorer eller moln VM-instans hello SQL Server för bättre prestanda, rekommenderar vi att du installerar dem på separata datorer.</span><span class="sxs-lookup"><span data-stu-id="7d388-116">While you can install gateway on hello same on-premises machine or cloud VM instance as hello SQL Server for better performance, we recommended that you install them on separate machines.</span></span> <span data-ttu-id="7d388-117">Med hello gateway och SQL Server på separata datorer minskar resurskonflikter.</span><span class="sxs-lookup"><span data-stu-id="7d388-117">Having hello gateway and SQL Server on separate machines reduces resource contention.</span></span>

## <a name="getting-started"></a><span data-ttu-id="7d388-118">Komma igång</span><span class="sxs-lookup"><span data-stu-id="7d388-118">Getting started</span></span>
<span data-ttu-id="7d388-119">Du kan skapa en pipeline med en kopia-aktivitet som flyttar data till/från en lokal SQL Server-databas med hjälp av olika verktyg/API: er.</span><span class="sxs-lookup"><span data-stu-id="7d388-119">You can create a pipeline with a copy activity that moves data to/from an on-premises SQL Server database by using different tools/APIs.</span></span>

<span data-ttu-id="7d388-120">hello enklaste sättet toocreate en pipeline är toouse hello **guiden Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="7d388-120">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="7d388-121">Se [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden för hello kopiera data.</span><span class="sxs-lookup"><span data-stu-id="7d388-121">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span>

<span data-ttu-id="7d388-122">Du kan också använda följande verktyg toocreate en pipeline hello: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall** , **.NET API**, och **REST API**.</span><span class="sxs-lookup"><span data-stu-id="7d388-122">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="7d388-123">Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner toocreate en pipeline med en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="7d388-123">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="7d388-124">Om du använder hello verktyg eller API: er kan utföra du hello följande steg toocreate en pipeline som flyttar data från en källdata lagra tooa sink-datalagret:</span><span class="sxs-lookup"><span data-stu-id="7d388-124">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span> 

1. <span data-ttu-id="7d388-125">Skapa en **datafabriken**.</span><span class="sxs-lookup"><span data-stu-id="7d388-125">Create a **data factory**.</span></span> <span data-ttu-id="7d388-126">En datafabrik kan innehålla en eller flera pipelines.</span><span class="sxs-lookup"><span data-stu-id="7d388-126">A data factory may contain one or more pipelines.</span></span> 
2. <span data-ttu-id="7d388-127">Skapa **länkade tjänster** toolink indata och utdata lagrar tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="7d388-127">Create **linked services** toolink input and output data stores tooyour data factory.</span></span> <span data-ttu-id="7d388-128">Exempelvis om du kopierar data från en SQL Server-databasen tooan Azure blob-lagring måste skapa du två länkade tjänster toolink din SQL Server-databas och Azure storage-konto tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="7d388-128">For example, if you are copying data from a SQL Server database tooan Azure blob storage, you create two linked services toolink your SQL Server database and Azure storage account tooyour data factory.</span></span> <span data-ttu-id="7d388-129">Länkad tjänstegenskaper som är specifika tooSQL Server-databasen, se [länkade tjänstegenskaper](#linked-service-properties) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7d388-129">For linked service properties that are specific tooSQL Server database, see [linked service properties](#linked-service-properties) section.</span></span> 
3. <span data-ttu-id="7d388-130">Skapa **datauppsättningar** toorepresent indata och utdata för hello kopieringsåtgärden.</span><span class="sxs-lookup"><span data-stu-id="7d388-130">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> <span data-ttu-id="7d388-131">I hello exempelvis nämns i hello sista steget skapar du en dataset toospecify hello SQL-tabell i SQL Server-databasen som innehåller hello indata.</span><span class="sxs-lookup"><span data-stu-id="7d388-131">In hello example mentioned in hello last step, you create a dataset toospecify hello SQL table in your SQL Server database that contains hello input data.</span></span> <span data-ttu-id="7d388-132">Och, skapar du en annan dataset toospecify hello blob-behållaren och hello-mappen som innehåller hello data kopieras från hello SQL Server-databas.</span><span class="sxs-lookup"><span data-stu-id="7d388-132">And, you create another dataset toospecify hello blob container and hello folder that holds hello data copied from hello SQL Server database.</span></span> <span data-ttu-id="7d388-133">Egenskaper för datamängd som är specifika tooSQL Server-databasen, se [egenskaper för datamängd](#dataset-properties) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7d388-133">For dataset properties that are specific tooSQL Server database, see [dataset properties](#dataset-properties) section.</span></span>
4. <span data-ttu-id="7d388-134">Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata.</span><span class="sxs-lookup"><span data-stu-id="7d388-134">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> <span data-ttu-id="7d388-135">I hello-exemplet ovan, använder du SqlSource som en källa och BlobSink som en mottagare för hello kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="7d388-135">In hello example mentioned earlier, you use SqlSource as a source and BlobSink as a sink for hello copy activity.</span></span> <span data-ttu-id="7d388-136">På samma sätt om du vill kopiera från Azure Blob Storage tooSQL Server-databas, använder du BlobSource och SqlSink i hello kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="7d388-136">Similarly, if you are copying from Azure Blob Storage tooSQL Server Database, you use BlobSource and SqlSink in hello copy activity.</span></span> <span data-ttu-id="7d388-137">Kopiera Aktivitetsegenskaper som är specifika tooSQL Server-databasen, se [kopiera Aktivitetsegenskaper](#copy-activity-properties) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7d388-137">For copy activity properties that are specific tooSQL Server Database, see [copy activity properties](#copy-activity-properties) section.</span></span> <span data-ttu-id="7d388-138">Mer information om hur toouse en databas som en källa eller en mottagare klickar du på hello länk under föregående hello för datalager.</span><span class="sxs-lookup"><span data-stu-id="7d388-138">For details on how toouse a data store as a source or a sink, click hello link in hello previous section for your data store.</span></span> 

<span data-ttu-id="7d388-139">När du använder guiden hello skapas automatiskt JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och hello pipeline) för dig.</span><span class="sxs-lookup"><span data-stu-id="7d388-139">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="7d388-140">När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av hello JSON-format.</span><span class="sxs-lookup"><span data-stu-id="7d388-140">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="7d388-141">Exempel med JSON-definitioner för Data Factory-entiteter som har använt toocopy data till/från en lokal SQL Server-databas finns [JSON-exempel](#json-examples-for-copying-data-from-and-to-sql-server) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="7d388-141">For samples with JSON definitions for Data Factory entities that are used toocopy data to/from an on-premises SQL Server database, see [JSON examples](#json-examples-for-copying-data-from-and-to-sql-server) section of this article.</span></span> 

<span data-ttu-id="7d388-142">hello följande avsnitt innehåller information om JSON-egenskaper används toodefine Data Factory entiteter specifika tooSQL Server:</span><span class="sxs-lookup"><span data-stu-id="7d388-142">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooSQL Server:</span></span> 

## <a name="linked-service-properties"></a><span data-ttu-id="7d388-143">Länkad tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="7d388-143">Linked service properties</span></span>
<span data-ttu-id="7d388-144">Du skapar en länkad tjänst av typen **OnPremisesSqlServer** toolink fabriken för tooa en lokal SQL Server-databasen.</span><span class="sxs-lookup"><span data-stu-id="7d388-144">You create a linked service of type **OnPremisesSqlServer** toolink an on-premises SQL Server database tooa data factory.</span></span> <span data-ttu-id="7d388-145">hello följande tabell ger en beskrivning för JSON-element specifika tooon lokala SQL Server som är länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="7d388-145">hello following table provides description for JSON elements specific tooon-premises SQL Server linked service.</span></span>

<span data-ttu-id="7d388-146">hello följande tabell ger en beskrivning för JSON-element specifika tooSQL länkad Server-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="7d388-146">hello following table provides description for JSON elements specific tooSQL Server linked service.</span></span>

| <span data-ttu-id="7d388-147">Egenskap</span><span class="sxs-lookup"><span data-stu-id="7d388-147">Property</span></span> | <span data-ttu-id="7d388-148">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="7d388-148">Description</span></span> | <span data-ttu-id="7d388-149">Krävs</span><span class="sxs-lookup"><span data-stu-id="7d388-149">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7d388-150">typ</span><span class="sxs-lookup"><span data-stu-id="7d388-150">type</span></span> |<span data-ttu-id="7d388-151">hello typegenskapen ska anges till: **OnPremisesSqlServer**.</span><span class="sxs-lookup"><span data-stu-id="7d388-151">hello type property should be set to: **OnPremisesSqlServer**.</span></span> |<span data-ttu-id="7d388-152">Ja</span><span class="sxs-lookup"><span data-stu-id="7d388-152">Yes</span></span> |
| <span data-ttu-id="7d388-153">connectionString</span><span class="sxs-lookup"><span data-stu-id="7d388-153">connectionString</span></span> |<span data-ttu-id="7d388-154">Ange connectionString information som behövs för tooconnect toohello lokala SQL Server-databas med SQL-autentisering eller Windows-autentisering.</span><span class="sxs-lookup"><span data-stu-id="7d388-154">Specify connectionString information needed tooconnect toohello on-premises SQL Server database using either SQL authentication or Windows authentication.</span></span> |<span data-ttu-id="7d388-155">Ja</span><span class="sxs-lookup"><span data-stu-id="7d388-155">Yes</span></span> |
| <span data-ttu-id="7d388-156">gatewayName</span><span class="sxs-lookup"><span data-stu-id="7d388-156">gatewayName</span></span> |<span data-ttu-id="7d388-157">Namnet på hello-gateway som hello Data Factory-tjänsten ska använda tooconnect toohello lokala SQL Server-databas.</span><span class="sxs-lookup"><span data-stu-id="7d388-157">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises SQL Server database.</span></span> |<span data-ttu-id="7d388-158">Ja</span><span class="sxs-lookup"><span data-stu-id="7d388-158">Yes</span></span> |
| <span data-ttu-id="7d388-159">användarnamn</span><span class="sxs-lookup"><span data-stu-id="7d388-159">username</span></span> |<span data-ttu-id="7d388-160">Ange användarnamnet om du använder Windows-autentisering.</span><span class="sxs-lookup"><span data-stu-id="7d388-160">Specify user name if you are using Windows Authentication.</span></span> <span data-ttu-id="7d388-161">Exempel: **domainname\\användarnamn**.</span><span class="sxs-lookup"><span data-stu-id="7d388-161">Example: **domainname\\username**.</span></span> |<span data-ttu-id="7d388-162">Nej</span><span class="sxs-lookup"><span data-stu-id="7d388-162">No</span></span> |
| <span data-ttu-id="7d388-163">lösenord</span><span class="sxs-lookup"><span data-stu-id="7d388-163">password</span></span> |<span data-ttu-id="7d388-164">Ange lösenord för hello-användarkonto som du angav för hello användarnamn.</span><span class="sxs-lookup"><span data-stu-id="7d388-164">Specify password for hello user account you specified for hello username.</span></span> |<span data-ttu-id="7d388-165">Nej</span><span class="sxs-lookup"><span data-stu-id="7d388-165">No</span></span> |

<span data-ttu-id="7d388-166">Du kan kryptera autentiseringsuppgifterna med hjälp av hello **ny AzureRmDataFactoryEncryptValue** cmdlet och använda dem i hello anslutningssträngen som visas i följande exempel hello (**EncryptedCredential** egenskapen):</span><span class="sxs-lookup"><span data-stu-id="7d388-166">You can encrypt credentials using hello **New-AzureRmDataFactoryEncryptValue** cmdlet and use them in hello connection string as shown in hello following example (**EncryptedCredential** property):</span></span>  

```JSON
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```

### <a name="samples"></a><span data-ttu-id="7d388-167">Exempel</span><span class="sxs-lookup"><span data-stu-id="7d388-167">Samples</span></span>
<span data-ttu-id="7d388-168">**JSON för att använda SQL-autentisering**</span><span class="sxs-lookup"><span data-stu-id="7d388-168">**JSON for using SQL Authentication**</span></span>

```json
{
    "name": "MyOnPremisesSQLDB",
    "properties":
    {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "connectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=False;User ID=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```
<span data-ttu-id="7d388-169">**JSON för att använda Windows-autentisering**</span><span class="sxs-lookup"><span data-stu-id="7d388-169">**JSON for using Windows Authentication**</span></span>

<span data-ttu-id="7d388-170">Data Management Gateway kommer personifiera hello angivna användare konto tooconnect toohello lokala SQL Server-databas.</span><span class="sxs-lookup"><span data-stu-id="7d388-170">Data Management Gateway will impersonate hello specified user account tooconnect toohello on-premises SQL Server database.</span></span> 

```json
{
     "Name": " MyOnPremisesSQLDB",
     "Properties":
     {
         "type": "OnPremisesSqlServer",
         "typeProperties": {
             "ConnectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=True;",
             "username": "<domain\\username>",
             "password": "<password>",
             "gatewayName": "<gateway name>"
        }
     }
}
```

## <a name="dataset-properties"></a><span data-ttu-id="7d388-171">Egenskaper för datamängd</span><span class="sxs-lookup"><span data-stu-id="7d388-171">Dataset properties</span></span>
<span data-ttu-id="7d388-172">Hello samplingarna, du har använt ett DataSet-objekt av typen **SqlServerTable** toorepresent en tabell i SQL Server-databasen.</span><span class="sxs-lookup"><span data-stu-id="7d388-172">In hello samples, you have used a dataset of type **SqlServerTable** toorepresent a table in a SQL Server database.</span></span>  

<span data-ttu-id="7d388-173">En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns hello [skapa datauppsättningar](data-factory-create-datasets.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="7d388-173">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="7d388-174">Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av datauppsättningen (SQL Server, Azure blob, Azure-tabellen, osv.).</span><span class="sxs-lookup"><span data-stu-id="7d388-174">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (SQL Server, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="7d388-175">hello typeProperties avsnittet är olika för varje typ av dataset och ger information om hello platsen för hello data i datalagret hello.</span><span class="sxs-lookup"><span data-stu-id="7d388-175">hello typeProperties section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="7d388-176">Hej **typeProperties** avsnittet för hello dataset av typen **SqlServerTable** har hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="7d388-176">hello **typeProperties** section for hello dataset of type **SqlServerTable** has hello following properties:</span></span>

| <span data-ttu-id="7d388-177">Egenskap</span><span class="sxs-lookup"><span data-stu-id="7d388-177">Property</span></span> | <span data-ttu-id="7d388-178">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="7d388-178">Description</span></span> | <span data-ttu-id="7d388-179">Krävs</span><span class="sxs-lookup"><span data-stu-id="7d388-179">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7d388-180">tableName</span><span class="sxs-lookup"><span data-stu-id="7d388-180">tableName</span></span> |<span data-ttu-id="7d388-181">Namnet på hello tabellen eller vyn i hello SQL Server-databasinstansen som den länkade tjänsten refererar till.</span><span class="sxs-lookup"><span data-stu-id="7d388-181">Name of hello table or view in hello SQL Server Database instance that linked service refers to.</span></span> |<span data-ttu-id="7d388-182">Ja</span><span class="sxs-lookup"><span data-stu-id="7d388-182">Yes</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="7d388-183">Kopiera egenskaper för aktivitet</span><span class="sxs-lookup"><span data-stu-id="7d388-183">Copy activity properties</span></span>
<span data-ttu-id="7d388-184">Om du flyttar data från en SQL Server-databas måste du ange hello källtypen i hello kopieringsaktiviteten för**SqlSource**.</span><span class="sxs-lookup"><span data-stu-id="7d388-184">If you are moving data from a SQL Server database, you set hello source type in hello copy activity too**SqlSource**.</span></span> <span data-ttu-id="7d388-185">På samma sätt om du flyttar data tooa SQL Server-databas måste du ange hello Mottagartypen i hello kopieringsaktiviteten för**SqlSink**.</span><span class="sxs-lookup"><span data-stu-id="7d388-185">Similarly, if you are moving data tooa SQL Server database, you set hello sink type in hello copy activity too**SqlSink**.</span></span> <span data-ttu-id="7d388-186">Det här avsnittet innehåller en lista över egenskaper som stöds av SqlSource och SqlSink.</span><span class="sxs-lookup"><span data-stu-id="7d388-186">This section provides a list of properties supported by SqlSource and SqlSink.</span></span>

<span data-ttu-id="7d388-187">En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns hello [skapar Pipelines](data-factory-create-pipelines.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="7d388-187">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="7d388-188">Egenskaper som namn, beskrivning, ingående och utgående tabeller och principer är tillgängliga för alla typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="7d388-188">Properties such as name, description, input and output tables, and policies are available for all types of activities.</span></span>

> [!NOTE]
> <span data-ttu-id="7d388-189">Hej Kopieringsaktiviteten tar endast en inmatning och ger en enda utdata.</span><span class="sxs-lookup"><span data-stu-id="7d388-189">hello Copy Activity takes only one input and produces only one output.</span></span>

<span data-ttu-id="7d388-190">De egenskaper som är tillgängliga under hello typeProperties i hello aktivitet varierar beroende på varje aktivitetstyp.</span><span class="sxs-lookup"><span data-stu-id="7d388-190">Whereas, properties available in hello typeProperties section of hello activity vary with each activity type.</span></span> <span data-ttu-id="7d388-191">För Kopieringsaktivitet kan varierar de beroende på hello typer av datakällor och sänkor.</span><span class="sxs-lookup"><span data-stu-id="7d388-191">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

### <a name="sqlsource"></a><span data-ttu-id="7d388-192">SqlSource</span><span class="sxs-lookup"><span data-stu-id="7d388-192">SqlSource</span></span>
<span data-ttu-id="7d388-193">När datakällan i en Kopieringsaktivitet är av typen **SqlSource**, hello följande egenskaper är tillgängliga i **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="7d388-193">When source in a copy activity is of type **SqlSource**, hello following properties are available in **typeProperties** section:</span></span>

| <span data-ttu-id="7d388-194">Egenskap</span><span class="sxs-lookup"><span data-stu-id="7d388-194">Property</span></span> | <span data-ttu-id="7d388-195">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="7d388-195">Description</span></span> | <span data-ttu-id="7d388-196">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="7d388-196">Allowed values</span></span> | <span data-ttu-id="7d388-197">Krävs</span><span class="sxs-lookup"><span data-stu-id="7d388-197">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="7d388-198">sqlReaderQuery</span><span class="sxs-lookup"><span data-stu-id="7d388-198">sqlReaderQuery</span></span> |<span data-ttu-id="7d388-199">Använda hello anpassad fråga tooread data.</span><span class="sxs-lookup"><span data-stu-id="7d388-199">Use hello custom query tooread data.</span></span> |<span data-ttu-id="7d388-200">SQL-sträng.</span><span class="sxs-lookup"><span data-stu-id="7d388-200">SQL query string.</span></span> <span data-ttu-id="7d388-201">Till exempel: Välj * från mytable prefix.</span><span class="sxs-lookup"><span data-stu-id="7d388-201">For example: select * from MyTable.</span></span> <span data-ttu-id="7d388-202">Kan referera till flera tabeller från hello-databas som refereras av hello inkommande dataset.</span><span class="sxs-lookup"><span data-stu-id="7d388-202">May reference multiple tables from hello database referenced by hello input dataset.</span></span> <span data-ttu-id="7d388-203">Om inget anges hello SQL-instruktionen som körs: Välj från mytable prefix.</span><span class="sxs-lookup"><span data-stu-id="7d388-203">If not specified, hello SQL statement that is executed: select from MyTable.</span></span> |<span data-ttu-id="7d388-204">Nej</span><span class="sxs-lookup"><span data-stu-id="7d388-204">No</span></span> |
| <span data-ttu-id="7d388-205">sqlReaderStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="7d388-205">sqlReaderStoredProcedureName</span></span> |<span data-ttu-id="7d388-206">Namnet på hello lagrad procedur som läser data från hello källtabellen.</span><span class="sxs-lookup"><span data-stu-id="7d388-206">Name of hello stored procedure that reads data from hello source table.</span></span> |<span data-ttu-id="7d388-207">Namnet på hello lagrad procedur.</span><span class="sxs-lookup"><span data-stu-id="7d388-207">Name of hello stored procedure.</span></span> <span data-ttu-id="7d388-208">hello senaste SQL-instruktionen måste vara en SELECT-instruktion i hello lagrad procedur.</span><span class="sxs-lookup"><span data-stu-id="7d388-208">hello last SQL statement must be a SELECT statement in hello stored procedure.</span></span> |<span data-ttu-id="7d388-209">Nej</span><span class="sxs-lookup"><span data-stu-id="7d388-209">No</span></span> |
| <span data-ttu-id="7d388-210">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="7d388-210">storedProcedureParameters</span></span> |<span data-ttu-id="7d388-211">Parametrar för hello lagrad procedur.</span><span class="sxs-lookup"><span data-stu-id="7d388-211">Parameters for hello stored procedure.</span></span> |<span data-ttu-id="7d388-212">Namn/värde-par.</span><span class="sxs-lookup"><span data-stu-id="7d388-212">Name/value pairs.</span></span> <span data-ttu-id="7d388-213">Namn och skiftläge parametrar måste matcha hello namn och versaler och gemener i hello lagrade procedurparametrar.</span><span class="sxs-lookup"><span data-stu-id="7d388-213">Names and casing of parameters must match hello names and casing of hello stored procedure parameters.</span></span> |<span data-ttu-id="7d388-214">Nej</span><span class="sxs-lookup"><span data-stu-id="7d388-214">No</span></span> |

<span data-ttu-id="7d388-215">Om hello **sqlReaderQuery** har angetts för hello SqlSource, hello Kopieringsaktiviteten kör den här frågan mot hello SQL Server-databas källa tooget hello data.</span><span class="sxs-lookup"><span data-stu-id="7d388-215">If hello **sqlReaderQuery** is specified for hello SqlSource, hello Copy Activity runs this query against hello SQL Server Database source tooget hello data.</span></span>

<span data-ttu-id="7d388-216">Du kan också ange en lagrad procedur genom att ange hello **sqlReaderStoredProcedureName** och **storedProcedureParameters** (om hello tar lagrade proceduren parametrar).</span><span class="sxs-lookup"><span data-stu-id="7d388-216">Alternatively, you can specify a stored procedure by specifying hello **sqlReaderStoredProcedureName** and **storedProcedureParameters** (if hello stored procedure takes parameters).</span></span>

<span data-ttu-id="7d388-217">Om du inte anger sqlReaderQuery eller sqlReaderStoredProcedureName är hello kolumner har definierats i hello struktur avsnitt används toobuild en select-frågan toorun mot hello SQL Server-databas.</span><span class="sxs-lookup"><span data-stu-id="7d388-217">If you do not specify either sqlReaderQuery or sqlReaderStoredProcedureName, hello columns defined in hello structure section are used toobuild a select query toorun against hello SQL Server Database.</span></span> <span data-ttu-id="7d388-218">Om hello datauppsättningsdefinitionen inte har hello struktur, markeras alla kolumner från tabellen hello.</span><span class="sxs-lookup"><span data-stu-id="7d388-218">If hello dataset definition does not have hello structure, all columns are selected from hello table.</span></span>

> [!NOTE]
> <span data-ttu-id="7d388-219">När du använder **sqlReaderStoredProcedureName**, du behöver fortfarande toospecify ett värde för hello **tableName** egenskap i hello dataset JSON.</span><span class="sxs-lookup"><span data-stu-id="7d388-219">When you use **sqlReaderStoredProcedureName**, you still need toospecify a value for hello **tableName** property in hello dataset JSON.</span></span> <span data-ttu-id="7d388-220">Det finns inga verifieringar utföras om mot den här tabellen.</span><span class="sxs-lookup"><span data-stu-id="7d388-220">There are no validations performed against this table though.</span></span>

### <a name="sqlsink"></a><span data-ttu-id="7d388-221">SqlSink</span><span class="sxs-lookup"><span data-stu-id="7d388-221">SqlSink</span></span>
<span data-ttu-id="7d388-222">**SqlSink** stöder hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="7d388-222">**SqlSink** supports hello following properties:</span></span>

| <span data-ttu-id="7d388-223">Egenskap</span><span class="sxs-lookup"><span data-stu-id="7d388-223">Property</span></span> | <span data-ttu-id="7d388-224">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="7d388-224">Description</span></span> | <span data-ttu-id="7d388-225">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="7d388-225">Allowed values</span></span> | <span data-ttu-id="7d388-226">Krävs</span><span class="sxs-lookup"><span data-stu-id="7d388-226">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="7d388-227">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="7d388-227">writeBatchTimeout</span></span> |<span data-ttu-id="7d388-228">Vänta tills hello batch insert-åtgärden toocomplete innan tidsgränsen uppnås.</span><span class="sxs-lookup"><span data-stu-id="7d388-228">Wait time for hello batch insert operation toocomplete before it times out.</span></span> |<span data-ttu-id="7d388-229">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="7d388-229">timespan</span></span><br/><br/> <span data-ttu-id="7d388-230">Exempel ”: 00: 30:00” (30 minuter).</span><span class="sxs-lookup"><span data-stu-id="7d388-230">Example: “00:30:00” (30 minutes).</span></span> |<span data-ttu-id="7d388-231">Nej</span><span class="sxs-lookup"><span data-stu-id="7d388-231">No</span></span> |
| <span data-ttu-id="7d388-232">writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="7d388-232">writeBatchSize</span></span> |<span data-ttu-id="7d388-233">Infogar data i hello SQL-tabellen när hello buffertstorlek når writeBatchSize.</span><span class="sxs-lookup"><span data-stu-id="7d388-233">Inserts data into hello SQL table when hello buffer size reaches writeBatchSize.</span></span> |<span data-ttu-id="7d388-234">Heltal (antalet rader)</span><span class="sxs-lookup"><span data-stu-id="7d388-234">Integer (number of rows)</span></span> |<span data-ttu-id="7d388-235">Nej (standard: 10000)</span><span class="sxs-lookup"><span data-stu-id="7d388-235">No (default: 10000)</span></span> |
| <span data-ttu-id="7d388-236">sqlWriterCleanupScript</span><span class="sxs-lookup"><span data-stu-id="7d388-236">sqlWriterCleanupScript</span></span> |<span data-ttu-id="7d388-237">Ange frågan för Kopieringsaktiviteten tooexecute så att data för ett visst segment har rensats bort.</span><span class="sxs-lookup"><span data-stu-id="7d388-237">Specify query for Copy Activity tooexecute such that data of a specific slice is cleaned up.</span></span> <span data-ttu-id="7d388-238">Mer information finns i [repeterbara kopiera](#repeatable-copy) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7d388-238">For more information, see [repeatable copy](#repeatable-copy) section.</span></span> |<span data-ttu-id="7d388-239">En frågesats.</span><span class="sxs-lookup"><span data-stu-id="7d388-239">A query statement.</span></span> |<span data-ttu-id="7d388-240">Nej</span><span class="sxs-lookup"><span data-stu-id="7d388-240">No</span></span> |
| <span data-ttu-id="7d388-241">sliceIdentifierColumnName</span><span class="sxs-lookup"><span data-stu-id="7d388-241">sliceIdentifierColumnName</span></span> |<span data-ttu-id="7d388-242">Ange kolumnnamn för Kopieringsaktiviteten toofill med genereras automatiskt segment-ID som har använt tooclean in data för ett visst segment när köras på nytt.</span><span class="sxs-lookup"><span data-stu-id="7d388-242">Specify column name for Copy Activity toofill with auto generated slice identifier, which is used tooclean up data of a specific slice when rerun.</span></span> <span data-ttu-id="7d388-243">Mer information finns i [repeterbara kopiera](#repeatable-copy) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7d388-243">For more information, see [repeatable copy](#repeatable-copy) section.</span></span> |<span data-ttu-id="7d388-244">Kolumnnamnet för en kolumn med datatypen för binary(32).</span><span class="sxs-lookup"><span data-stu-id="7d388-244">Column name of a column with data type of binary(32).</span></span> |<span data-ttu-id="7d388-245">Nej</span><span class="sxs-lookup"><span data-stu-id="7d388-245">No</span></span> |
| <span data-ttu-id="7d388-246">sqlWriterStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="7d388-246">sqlWriterStoredProcedureName</span></span> |<span data-ttu-id="7d388-247">Namnet på hello lagrad procedur upserts (uppdateringar/infogar) data i hello måltabellen.</span><span class="sxs-lookup"><span data-stu-id="7d388-247">Name of hello stored procedure that upserts (updates/inserts) data into hello target table.</span></span> |<span data-ttu-id="7d388-248">Namnet på hello lagrad procedur.</span><span class="sxs-lookup"><span data-stu-id="7d388-248">Name of hello stored procedure.</span></span> |<span data-ttu-id="7d388-249">Nej</span><span class="sxs-lookup"><span data-stu-id="7d388-249">No</span></span> |
| <span data-ttu-id="7d388-250">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="7d388-250">storedProcedureParameters</span></span> |<span data-ttu-id="7d388-251">Parametrar för hello lagrad procedur.</span><span class="sxs-lookup"><span data-stu-id="7d388-251">Parameters for hello stored procedure.</span></span> |<span data-ttu-id="7d388-252">Namn/värde-par.</span><span class="sxs-lookup"><span data-stu-id="7d388-252">Name/value pairs.</span></span> <span data-ttu-id="7d388-253">Namn och skiftläge parametrar måste matcha hello namn och versaler och gemener i hello lagrade procedurparametrar.</span><span class="sxs-lookup"><span data-stu-id="7d388-253">Names and casing of parameters must match hello names and casing of hello stored procedure parameters.</span></span> |<span data-ttu-id="7d388-254">Nej</span><span class="sxs-lookup"><span data-stu-id="7d388-254">No</span></span> |
| <span data-ttu-id="7d388-255">sqlWriterTableType</span><span class="sxs-lookup"><span data-stu-id="7d388-255">sqlWriterTableType</span></span> |<span data-ttu-id="7d388-256">Ange tabellen typen namnet toobe används i hello lagrade proceduren.</span><span class="sxs-lookup"><span data-stu-id="7d388-256">Specify table type name toobe used in hello stored procedure.</span></span> <span data-ttu-id="7d388-257">Kopieringsaktiviteten tillgängliggör hello data flyttas i en temporär tabell med den här tabellen.</span><span class="sxs-lookup"><span data-stu-id="7d388-257">Copy activity makes hello data being moved available in a temp table with this table type.</span></span> <span data-ttu-id="7d388-258">Lagrade procedurer kan sedan koppla hello data kopieras med befintliga data.</span><span class="sxs-lookup"><span data-stu-id="7d388-258">Stored procedure code can then merge hello data being copied with existing data.</span></span> |<span data-ttu-id="7d388-259">Ett namn för tabellen.</span><span class="sxs-lookup"><span data-stu-id="7d388-259">A table type name.</span></span> |<span data-ttu-id="7d388-260">Nej</span><span class="sxs-lookup"><span data-stu-id="7d388-260">No</span></span> |


## <a name="json-examples-for-copying-data-from-and-toosql-server"></a><span data-ttu-id="7d388-261">JSON-exempel för att kopiera data från och tooSQL Server</span><span class="sxs-lookup"><span data-stu-id="7d388-261">JSON examples for copying data from and tooSQL Server</span></span>
<span data-ttu-id="7d388-262">hello följande exempel ger exempel JSON definitioner som du kan använda toocreate en pipeline med hjälp av [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="7d388-262">hello following examples provide sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="7d388-263">Hej följande exempel visar hur toocopy data tooand från SQL Server och Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="7d388-263">hello following samples show how toocopy data tooand from SQL Server and Azure Blob Storage.</span></span> <span data-ttu-id="7d388-264">Dock datan kan kopieras **direkt** från alla källor tooany av hello sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hello Kopieringsaktiviteten i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="7d388-264">However, data can be copied **directly** from any of sources tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>     

## <a name="example-copy-data-from-sql-server-tooazure-blob"></a><span data-ttu-id="7d388-265">Exempel: Kopiera data från SQL Server-tooAzure Blob</span><span class="sxs-lookup"><span data-stu-id="7d388-265">Example: Copy data from SQL Server tooAzure Blob</span></span>
<span data-ttu-id="7d388-266">följande exempel visar hello:</span><span class="sxs-lookup"><span data-stu-id="7d388-266">hello following sample shows:</span></span>

1. <span data-ttu-id="7d388-267">En länkad tjänst av typen [OnPremisesSqlServer](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="7d388-267">A linked service of type [OnPremisesSqlServer](#linked-service-properties).</span></span>
2. <span data-ttu-id="7d388-268">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="7d388-268">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="7d388-269">Indata [dataset](data-factory-create-datasets.md) av typen [SqlServerTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="7d388-269">An input [dataset](data-factory-create-datasets.md) of type [SqlServerTable](#dataset-properties).</span></span>
4. <span data-ttu-id="7d388-270">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="7d388-270">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="7d388-271">Hej [pipeline](data-factory-create-pipelines.md) med kopieringsaktiviteten som använder [SqlSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="7d388-271">hello [pipeline](data-factory-create-pipelines.md) with Copy activity that uses [SqlSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="7d388-272">hello exemplet kopierar time series-data från en SQL Server tabellen tooan Azure blob varje timme.</span><span class="sxs-lookup"><span data-stu-id="7d388-272">hello sample copies time-series data from a SQL Server table tooan Azure blob every hour.</span></span> <span data-ttu-id="7d388-273">hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7d388-273">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="7d388-274">Som ett första steg bör du konfigurera hello data management gateway.</span><span class="sxs-lookup"><span data-stu-id="7d388-274">As a first step, setup hello data management gateway.</span></span> <span data-ttu-id="7d388-275">hello anvisningar finns i hello [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="7d388-275">hello instructions are in hello [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="7d388-276">**SQL Server som är länkad tjänst**</span><span class="sxs-lookup"><span data-stu-id="7d388-276">**SQL Server linked service**</span></span>
```json
{
  "Name": "SqlServerLinkedService",
  "properties": {
    "type": "OnPremisesSqlServer",
    "typeProperties": {
      "connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;",
      "gatewayName": "<gatewayname>"
    }
  }
}
```
<span data-ttu-id="7d388-277">**Azure Blob länkade lagringstjänsten**</span><span class="sxs-lookup"><span data-stu-id="7d388-277">**Azure Blob storage linked service**</span></span>

```json
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
<span data-ttu-id="7d388-278">**SQL Server inkommande dataset**</span><span class="sxs-lookup"><span data-stu-id="7d388-278">**SQL Server input dataset**</span></span>

<span data-ttu-id="7d388-279">hello exemplet förutsätter att du har skapat en tabell ”mytable” som prefix i SQL Server och den innehåller en kolumn med namnet ”timestampcolumn” för tid series-data.</span><span class="sxs-lookup"><span data-stu-id="7d388-279">hello sample assumes you have created a table “MyTable” in SQL Server and it contains a column called “timestampcolumn” for time series data.</span></span> <span data-ttu-id="7d388-280">Du kan fråga över flera tabeller i samma databas som använder en enda dataset, men en enskild tabell måste användas för hello dataset tableName typeProperty hello.</span><span class="sxs-lookup"><span data-stu-id="7d388-280">You can query over multiple tables within hello same database using a single dataset, but a single table must be used for hello dataset's tableName typeProperty.</span></span>

<span data-ttu-id="7d388-281">Inställningen ”externa”: ”true” informerar Data Factory-tjänsten som hello dataset är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="7d388-281">Setting “external”: ”true” informs Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

```json
{
  "name": "SqlServerInput",
  "properties": {
    "type": "SqlServerTable",
    "linkedServiceName": "SqlServerLinkedService",
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
<span data-ttu-id="7d388-282">**Azure Blob utdatauppsättningen**</span><span class="sxs-lookup"><span data-stu-id="7d388-282">**Azure Blob output dataset**</span></span>

<span data-ttu-id="7d388-283">Data skrivs tooa nya blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="7d388-283">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="7d388-284">hello mappsökväg för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="7d388-284">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="7d388-285">hello mappsökväg använder år, månad, dag och timmar delar av hello starttid.</span><span class="sxs-lookup"><span data-stu-id="7d388-285">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

```json
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
<span data-ttu-id="7d388-286">**Pipeline med kopieringsaktiviteten**</span><span class="sxs-lookup"><span data-stu-id="7d388-286">**Pipeline with Copy activity**</span></span>

<span data-ttu-id="7d388-287">hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse dessa indata och utdata-datauppsättningar och schemalagda toorun varje timme.</span><span class="sxs-lookup"><span data-stu-id="7d388-287">hello pipeline contains a Copy Activity that is configured toouse these input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="7d388-288">I hello pipeline JSON-definitionen hello **källa** typ har angetts för**SqlSource** och **sink** typ har angetts för**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="7d388-288">In hello pipeline JSON definition, hello **source** type is set too**SqlSource** and **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="7d388-289">hello SQL-frågan som angetts för hello **SqlReaderQuery** egenskapen väljer hello data i hello tidigare timme toocopy.</span><span class="sxs-lookup"><span data-stu-id="7d388-289">hello SQL query specified for hello **SqlReaderQuery** property selects hello data in hello past hour toocopy.</span></span>

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2016-06-01T18:00:00",
    "end":"2016-06-01T19:00:00",
    "description":"pipeline for copy activity",
    "activities":[  
      {
        "name": "SqlServertoBlob",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": " SqlServerInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "SqlSource",
            "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
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
<span data-ttu-id="7d388-290">I det här exemplet **sqlReaderQuery** har angetts för hello SqlSource.</span><span class="sxs-lookup"><span data-stu-id="7d388-290">In this example, **sqlReaderQuery** is specified for hello SqlSource.</span></span> <span data-ttu-id="7d388-291">Hej Kopieringsaktiviteten kör den här frågan mot hello SQL Server-databas källdata tooget hello.</span><span class="sxs-lookup"><span data-stu-id="7d388-291">hello Copy Activity runs this query against hello SQL Server Database source tooget hello data.</span></span> <span data-ttu-id="7d388-292">Du kan också ange en lagrad procedur genom att ange hello **sqlReaderStoredProcedureName** och **storedProcedureParameters** (om hello tar lagrade proceduren parametrar).</span><span class="sxs-lookup"><span data-stu-id="7d388-292">Alternatively, you can specify a stored procedure by specifying hello **sqlReaderStoredProcedureName** and **storedProcedureParameters** (if hello stored procedure takes parameters).</span></span> <span data-ttu-id="7d388-293">Hej sqlReaderQuery kan referera till flera tabeller i hello-databas som refereras av hello inkommande dataset.</span><span class="sxs-lookup"><span data-stu-id="7d388-293">hello sqlReaderQuery can reference multiple tables within hello database referenced by hello input dataset.</span></span> <span data-ttu-id="7d388-294">Det är inte begränsad tooonly hello tabellen som hello datauppsättnings tableName typeProperty.</span><span class="sxs-lookup"><span data-stu-id="7d388-294">It is not limited tooonly hello table set as hello dataset's tableName typeProperty.</span></span>

<span data-ttu-id="7d388-295">Om du inte anger sqlReaderQuery eller sqlReaderStoredProcedureName är hello kolumner har definierats i hello struktur avsnitt används toobuild en select-frågan toorun mot hello SQL Server-databas.</span><span class="sxs-lookup"><span data-stu-id="7d388-295">If you do not specify sqlReaderQuery or sqlReaderStoredProcedureName, hello columns defined in hello structure section are used toobuild a select query toorun against hello SQL Server Database.</span></span> <span data-ttu-id="7d388-296">Om hello datauppsättningsdefinitionen inte har hello struktur, markeras alla kolumner från tabellen hello.</span><span class="sxs-lookup"><span data-stu-id="7d388-296">If hello dataset definition does not have hello structure, all columns are selected from hello table.</span></span>

<span data-ttu-id="7d388-297">Se hello [Sql-källans](#sqlsource) avsnitt och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) hello lista över egenskaper som stöds av SqlSource och BlobSink.</span><span class="sxs-lookup"><span data-stu-id="7d388-297">See hello [Sql Source](#sqlsource) section and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) for hello list of properties supported by SqlSource and BlobSink.</span></span>

## <a name="example-copy-data-from-azure-blob-toosql-server"></a><span data-ttu-id="7d388-298">Exempel: Kopiera data från Azure Blob-tooSQL Server</span><span class="sxs-lookup"><span data-stu-id="7d388-298">Example: Copy data from Azure Blob tooSQL Server</span></span>
<span data-ttu-id="7d388-299">följande exempel visar hello:</span><span class="sxs-lookup"><span data-stu-id="7d388-299">hello following sample shows:</span></span>

1. <span data-ttu-id="7d388-300">hello länkade tjänsten av typen [OnPremisesSqlServer](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="7d388-300">hello linked service of type [OnPremisesSqlServer](#linked-service-properties).</span></span>
2. <span data-ttu-id="7d388-301">hello länkade tjänsten av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="7d388-301">hello linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="7d388-302">Indata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="7d388-302">An input [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="7d388-303">Utdata [dataset](data-factory-create-datasets.md) av typen [SqlServerTable](data-factory-sqlserver-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="7d388-303">An output [dataset](data-factory-create-datasets.md) of type [SqlServerTable](data-factory-sqlserver-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="7d388-304">Hej [pipeline](data-factory-create-pipelines.md) med kopieringsaktiviteten som använder [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) och [SqlSink](#sql-server-copy-activity-type-properties).</span><span class="sxs-lookup"><span data-stu-id="7d388-304">hello [pipeline](data-factory-create-pipelines.md) with Copy activity that uses [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) and [SqlSink](#sql-server-copy-activity-type-properties).</span></span>

<span data-ttu-id="7d388-305">hello exemplet kopierar time series-data från en tabell i Azure blob tooa SQL Server varje timme.</span><span class="sxs-lookup"><span data-stu-id="7d388-305">hello sample copies time-series data from an Azure blob tooa SQL Server table every hour.</span></span> <span data-ttu-id="7d388-306">hello JSON egenskaper som används i exemplen beskrivs i hello-exempel i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7d388-306">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="7d388-307">**SQL Server som är länkad tjänst**</span><span class="sxs-lookup"><span data-stu-id="7d388-307">**SQL Server linked service**</span></span>

```json
{
  "Name": "SqlServerLinkedService",
  "properties": {
    "type": "OnPremisesSqlServer",
    "typeProperties": {
      "connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;",
      "gatewayName": "<gatewayname>"
    }
  }
}
```
<span data-ttu-id="7d388-308">**Azure Blob länkade lagringstjänsten**</span><span class="sxs-lookup"><span data-stu-id="7d388-308">**Azure Blob storage linked service**</span></span>

```json
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
<span data-ttu-id="7d388-309">**Azure Blob-inkommande datamängd**</span><span class="sxs-lookup"><span data-stu-id="7d388-309">**Azure Blob input dataset**</span></span>

<span data-ttu-id="7d388-310">Data hämtas från en ny blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="7d388-310">Data is picked up from a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="7d388-311">hello mappen sökvägen och filnamnet för hello blob utvärderas dynamiskt baserat på hello starttiden för hello-segment som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="7d388-311">hello folder path and file name for hello blob are dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="7d388-312">hello mappsökväg använder år, månad och som en dagdel av hello starttid och filnamn använder hello timme tillhör hello starttid.</span><span class="sxs-lookup"><span data-stu-id="7d388-312">hello folder path uses year, month, and day part of hello start time and file name uses hello hour part of hello start time.</span></span> <span data-ttu-id="7d388-313">”externa”: ”true” inställningen informerar hello Data Factory-tjänsten som hello dataset är externa toohello data factory och inte tillverkas av en aktivitet i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="7d388-313">“external”: “true” setting informs hello Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

```json
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
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
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "rowDelimiter": "\n"
      }
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
<span data-ttu-id="7d388-314">**SQL Server-utdatauppsättningen**</span><span class="sxs-lookup"><span data-stu-id="7d388-314">**SQL Server output dataset**</span></span>

<span data-ttu-id="7d388-315">hello exemplet kopierar data tooa tabell med namnet ”mytable” som prefix i SQL Server.</span><span class="sxs-lookup"><span data-stu-id="7d388-315">hello sample copies data tooa table named “MyTable” in SQL Server.</span></span> <span data-ttu-id="7d388-316">Skapa hello tabell i SQL Server med hello samma antal kolumner som du förväntar dig hello Blob CSV-filen toocontain.</span><span class="sxs-lookup"><span data-stu-id="7d388-316">Create hello table in SQL Server with hello same number of columns as you expect hello Blob CSV file toocontain.</span></span> <span data-ttu-id="7d388-317">Nya rader läggs toohello tabell varje timme.</span><span class="sxs-lookup"><span data-stu-id="7d388-317">New rows are added toohello table every hour.</span></span>

```json
{
  "name": "SqlServerOutput",
  "properties": {
    "type": "SqlServerTable",
    "linkedServiceName": "SqlServerLinkedService",
    "typeProperties": {
      "tableName": "MyOutputTable"
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```
<span data-ttu-id="7d388-318">**Pipeline med kopieringsaktiviteten**</span><span class="sxs-lookup"><span data-stu-id="7d388-318">**Pipeline with Copy activity**</span></span>

<span data-ttu-id="7d388-319">hello pipelinen innehåller en kopia-aktivitet som är konfigurerade toouse dessa indata och utdata-datauppsättningar och schemalagda toorun varje timme.</span><span class="sxs-lookup"><span data-stu-id="7d388-319">hello pipeline contains a Copy Activity that is configured toouse these input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="7d388-320">I hello pipeline JSON-definitionen hello **källa** typ har angetts för**BlobSource** och **sink** typ har angetts för**SqlSink**.</span><span class="sxs-lookup"><span data-stu-id="7d388-320">In hello pipeline JSON definition, hello **source** type is set too**BlobSource** and **sink** type is set too**SqlSink**.</span></span>

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "AzureBlobtoSQL",
        "description": "Copy Activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": " SqlServerOutput "
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource",
            "blobColumnSeparators": ","
          },
          "sink": {
            "type": "SqlSink"
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

## <a name="troubleshooting-connection-issues"></a><span data-ttu-id="7d388-321">Felsökning av anslutningsproblem med</span><span class="sxs-lookup"><span data-stu-id="7d388-321">Troubleshooting connection issues</span></span>
1. <span data-ttu-id="7d388-322">Konfigurera din SQL Server tooaccept fjärranslutningar.</span><span class="sxs-lookup"><span data-stu-id="7d388-322">Configure your SQL Server tooaccept remote connections.</span></span> <span data-ttu-id="7d388-323">Starta **SQL Server Management Studio**, högerklicka på **server**, och klicka på **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="7d388-323">Launch **SQL Server Management Studio**, right-click **server**, and click **Properties**.</span></span> <span data-ttu-id="7d388-324">Välj **anslutningar** hello listan och kontrollera **Tillåt fjärranslutningar toohello server**.</span><span class="sxs-lookup"><span data-stu-id="7d388-324">Select **Connections** from hello list and check **Allow remote connections toohello server**.</span></span>

    ![Aktivera fjärranslutningar](./media/data-factory-sqlserver-connector/AllowRemoteConnections.png)

    <span data-ttu-id="7d388-326">Se [konfigurerar hello fjärråtkomst Server Configuration Option](https://msdn.microsoft.com/library/ms191464.aspx) detaljerade anvisningar.</span><span class="sxs-lookup"><span data-stu-id="7d388-326">See [Configure hello remote access Server Configuration Option](https://msdn.microsoft.com/library/ms191464.aspx) for detailed steps.</span></span>
2. <span data-ttu-id="7d388-327">Starta **SQL Server Configuration Manager**.</span><span class="sxs-lookup"><span data-stu-id="7d388-327">Launch **SQL Server Configuration Manager**.</span></span> <span data-ttu-id="7d388-328">Expandera **SQL Server-nätverkskonfigurationen** för hello-instans du vill och välj **protokoll för MSSQLSERVER**.</span><span class="sxs-lookup"><span data-stu-id="7d388-328">Expand **SQL Server Network Configuration** for hello instance you want, and select **Protocols for MSSQLSERVER**.</span></span> <span data-ttu-id="7d388-329">Du bör se protokoll i hello högra rutan.</span><span class="sxs-lookup"><span data-stu-id="7d388-329">You should see protocols in hello right-pane.</span></span> <span data-ttu-id="7d388-330">Aktivera TCP/IP genom att högerklicka på **TCP/IP** och klicka på **aktivera**.</span><span class="sxs-lookup"><span data-stu-id="7d388-330">Enable TCP/IP by right-clicking **TCP/IP** and clicking **Enable**.</span></span>

    ![Aktivera TCP/IP](./media/data-factory-sqlserver-connector/EnableTCPProptocol.png)

    <span data-ttu-id="7d388-332">Se [aktivera eller inaktivera nätverksprotokoll Server](https://msdn.microsoft.com/library/ms191294.aspx) information och alternativa metoder för att aktivera TCP/IP-protokollet.</span><span class="sxs-lookup"><span data-stu-id="7d388-332">See [Enable or Disable a Server Network Protocol](https://msdn.microsoft.com/library/ms191294.aspx) for details and alternate ways of enabling TCP/IP protocol.</span></span>
3. <span data-ttu-id="7d388-333">I hello samma fönster genom att dubbelklicka på **TCP/IP** toolaunch **TCP/IP-egenskaper** fönster.</span><span class="sxs-lookup"><span data-stu-id="7d388-333">In hello same window, double-click **TCP/IP** toolaunch **TCP/IP Properties** window.</span></span>
4. <span data-ttu-id="7d388-334">Växla toohello **IP-adresser** fliken. Bläddra nedåt toosee **IPAll** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7d388-334">Switch toohello **IP Addresses** tab. Scroll down toosee **IPAll** section.</span></span> <span data-ttu-id="7d388-335">Skriv ner hello ** TCP-Port ** (standard är **1433**).</span><span class="sxs-lookup"><span data-stu-id="7d388-335">Note down hello **TCP Port **(default is **1433**).</span></span>
5. <span data-ttu-id="7d388-336">Skapa en **regel för hello Windows-brandväggen** på hello tooallow inkommande trafik via den här porten.</span><span class="sxs-lookup"><span data-stu-id="7d388-336">Create a **rule for hello Windows Firewall** on hello machine tooallow incoming traffic through this port.</span></span>  
6. <span data-ttu-id="7d388-337">**Verifiera anslutning**: tooconnect toohello SQL Server med fullständigt kvalificerade namnet använda SQL Server Management Studio från en annan dator.</span><span class="sxs-lookup"><span data-stu-id="7d388-337">**Verify connection**: tooconnect toohello SQL Server using fully qualified name, use SQL Server Management Studio from a different machine.</span></span> <span data-ttu-id="7d388-338">Till exempel ”:<machine>.<domain>. Corp.<company>.com, 1433 ”.</span><span class="sxs-lookup"><span data-stu-id="7d388-338">For example: "<machine>.<domain>.corp.<company>.com,1433."</span></span>

   > [!IMPORTANT]

   > <span data-ttu-id="7d388-339">Se [flytta data mellan lokala källor och hello moln med Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md) detaljerad information.</span><span class="sxs-lookup"><span data-stu-id="7d388-339">See [Move data between on-premises sources and hello cloud with Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md) for detailed information.</span></span>
   >
   > <span data-ttu-id="7d388-340">Se [felsökning av problem med gateway](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) tips om hur du felsöker anslutning /-gateway relaterade problem.</span><span class="sxs-lookup"><span data-stu-id="7d388-340">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>
   >
   >


## <a name="identity-columns-in-hello-target-database"></a><span data-ttu-id="7d388-341">Identitetskolumner i hello måldatabasen</span><span class="sxs-lookup"><span data-stu-id="7d388-341">Identity columns in hello target database</span></span>
<span data-ttu-id="7d388-342">Det här avsnittet innehåller ett exempel som kopierar data från en källtabell med ingen identitet kolumnen tooa måltabellen med en identitetskolumn.</span><span class="sxs-lookup"><span data-stu-id="7d388-342">This section provides an example that copies data from a source table with no identity column tooa destination table with an identity column.</span></span>

<span data-ttu-id="7d388-343">**Källtabellen:**</span><span class="sxs-lookup"><span data-stu-id="7d388-343">**Source table:**</span></span>

```sql
create table dbo.SourceTbl
(
       name varchar(100),
       age int
)
```
<span data-ttu-id="7d388-344">**Måltabellen:**</span><span class="sxs-lookup"><span data-stu-id="7d388-344">**Destination table:**</span></span>

```sql
create table dbo.TargetTbl
(
       identifier int identity(1,1),
       name varchar(100),
       age int
)
```

<span data-ttu-id="7d388-345">Observera att hello måltabellen har en identitetskolumn.</span><span class="sxs-lookup"><span data-stu-id="7d388-345">Notice that hello target table has an identity column.</span></span>

<span data-ttu-id="7d388-346">**Källa JSON-definitionen för datauppsättning**</span><span class="sxs-lookup"><span data-stu-id="7d388-346">**Source dataset JSON definition**</span></span>

```json
{
    "name": "SampleSource",
    "properties": {
        "published": false,
        "type": " SqlServerTable",
        "linkedServiceName": "TestIdentitySQL",
        "typeProperties": {
            "tableName": "SourceTbl"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
```
<span data-ttu-id="7d388-347">**Mål JSON-definitionen för datauppsättning**</span><span class="sxs-lookup"><span data-stu-id="7d388-347">**Destination dataset JSON definition**</span></span>

```json
{
    "name": "SampleTarget",
    "properties": {
        "structure": [
            { "name": "name" },
            { "name": "age" }
        ],
        "published": false,
        "type": "AzureSqlTable",
        "linkedServiceName": "TestIdentitySQLSource",
        "typeProperties": {
            "tableName": "TargetTbl"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": false,
        "policy": {}
    }
}
```

<span data-ttu-id="7d388-348">Observera att som käll- och tabellen har olika schema (målet har en ytterligare kolumn med identity).</span><span class="sxs-lookup"><span data-stu-id="7d388-348">Notice that as your source and target table have different schema (target has an additional column with identity).</span></span> <span data-ttu-id="7d388-349">I det här scenariot behöver du toospecify **struktur** egenskap i hello mål datauppsättningsdefinitionen, som inte innehåller hello identity-kolumn.</span><span class="sxs-lookup"><span data-stu-id="7d388-349">In this scenario, you need toospecify **structure** property in hello target dataset definition, which doesn’t include hello identity column.</span></span>

## <a name="invoke-stored-procedure-from-sql-sink"></a><span data-ttu-id="7d388-350">Anropa lagrade procedur från SQL-mottagare</span><span class="sxs-lookup"><span data-stu-id="7d388-350">Invoke stored procedure from SQL sink</span></span>
<span data-ttu-id="7d388-351">Se [anropa lagrad procedur för SQL-mottagare i en Kopieringsaktivitet](data-factory-invoke-stored-procedure-from-copy-activity.md) artikel ett exempel på att anropa en lagrad procedur från SQL-mottagare i en Kopieringsaktivitet för en pipeline.</span><span class="sxs-lookup"><span data-stu-id="7d388-351">See [Invoke stored procedure for SQL sink in copy activity](data-factory-invoke-stored-procedure-from-copy-activity.md) article for an example of invoking a stored procedure from SQL sink in a copy activity of a pipeline.</span></span>

## <a name="type-mapping-for-sql-server"></a><span data-ttu-id="7d388-352">Mappning för SQLServer</span><span class="sxs-lookup"><span data-stu-id="7d388-352">Type mapping for SQL server</span></span>
<span data-ttu-id="7d388-353">Som anges i hello [data movement aktiviteter](data-factory-data-movement-activities.md) artikeln hello kopieringsaktiviteten utför automatisk konverteringar från källan typer toosink typer med hello följande metod i steg 2:</span><span class="sxs-lookup"><span data-stu-id="7d388-353">As mentioned in hello [data movement activities](data-factory-data-movement-activities.md) article, hello Copy activity performs automatic type conversions from source types toosink types with hello following 2-step approach:</span></span>

1. <span data-ttu-id="7d388-354">Konvertera från inbyggda typer too.NET källtypen</span><span class="sxs-lookup"><span data-stu-id="7d388-354">Convert from native source types too.NET type</span></span>
2. <span data-ttu-id="7d388-355">Konvertera från .NET typen toonative Mottagartypen</span><span class="sxs-lookup"><span data-stu-id="7d388-355">Convert from .NET type toonative sink type</span></span>

<span data-ttu-id="7d388-356">När du flyttar data & från SQLServer hello används följande mappningar från SQL too.NET typ och vice versa.</span><span class="sxs-lookup"><span data-stu-id="7d388-356">When moving data too& from SQL server, hello following mappings are used from SQL type too.NET type and vice versa.</span></span>

<span data-ttu-id="7d388-357">hello-mappning är samma som hello Datatypsmappningen i SQL Server för ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="7d388-357">hello mapping is same as hello SQL Server Data Type Mapping for ADO.NET.</span></span>

| <span data-ttu-id="7d388-358">SQL Server Database Engine-typ</span><span class="sxs-lookup"><span data-stu-id="7d388-358">SQL Server Database Engine type</span></span> | <span data-ttu-id="7d388-359">.NET framework-typ</span><span class="sxs-lookup"><span data-stu-id="7d388-359">.NET Framework type</span></span> |
| --- | --- |
| <span data-ttu-id="7d388-360">bigint</span><span class="sxs-lookup"><span data-stu-id="7d388-360">bigint</span></span> |<span data-ttu-id="7d388-361">Int64</span><span class="sxs-lookup"><span data-stu-id="7d388-361">Int64</span></span> |
| <span data-ttu-id="7d388-362">Binär</span><span class="sxs-lookup"><span data-stu-id="7d388-362">binary</span></span> |<span data-ttu-id="7d388-363">byte]</span><span class="sxs-lookup"><span data-stu-id="7d388-363">Byte[]</span></span> |
| <span data-ttu-id="7d388-364">bitar</span><span class="sxs-lookup"><span data-stu-id="7d388-364">bit</span></span> |<span data-ttu-id="7d388-365">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="7d388-365">Boolean</span></span> |
| <span data-ttu-id="7d388-366">Char</span><span class="sxs-lookup"><span data-stu-id="7d388-366">char</span></span> |<span data-ttu-id="7d388-367">Sträng, Char]</span><span class="sxs-lookup"><span data-stu-id="7d388-367">String, Char[]</span></span> |
| <span data-ttu-id="7d388-368">Datum</span><span class="sxs-lookup"><span data-stu-id="7d388-368">date</span></span> |<span data-ttu-id="7d388-369">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="7d388-369">DateTime</span></span> |
| <span data-ttu-id="7d388-370">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="7d388-370">Datetime</span></span> |<span data-ttu-id="7d388-371">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="7d388-371">DateTime</span></span> |
| <span data-ttu-id="7d388-372">datetime2</span><span class="sxs-lookup"><span data-stu-id="7d388-372">datetime2</span></span> |<span data-ttu-id="7d388-373">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="7d388-373">DateTime</span></span> |
| <span data-ttu-id="7d388-374">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="7d388-374">Datetimeoffset</span></span> |<span data-ttu-id="7d388-375">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="7d388-375">DateTimeOffset</span></span> |
| <span data-ttu-id="7d388-376">Decimal</span><span class="sxs-lookup"><span data-stu-id="7d388-376">Decimal</span></span> |<span data-ttu-id="7d388-377">Decimal</span><span class="sxs-lookup"><span data-stu-id="7d388-377">Decimal</span></span> |
| <span data-ttu-id="7d388-378">FILESTREAM-attributet (varbinary(max))</span><span class="sxs-lookup"><span data-stu-id="7d388-378">FILESTREAM attribute (varbinary(max))</span></span> |<span data-ttu-id="7d388-379">byte]</span><span class="sxs-lookup"><span data-stu-id="7d388-379">Byte[]</span></span> |
| <span data-ttu-id="7d388-380">flyttal</span><span class="sxs-lookup"><span data-stu-id="7d388-380">Float</span></span> |<span data-ttu-id="7d388-381">dubbla</span><span class="sxs-lookup"><span data-stu-id="7d388-381">Double</span></span> |
| <span data-ttu-id="7d388-382">Bild</span><span class="sxs-lookup"><span data-stu-id="7d388-382">image</span></span> |<span data-ttu-id="7d388-383">byte]</span><span class="sxs-lookup"><span data-stu-id="7d388-383">Byte[]</span></span> |
| <span data-ttu-id="7d388-384">int</span><span class="sxs-lookup"><span data-stu-id="7d388-384">int</span></span> |<span data-ttu-id="7d388-385">Int32</span><span class="sxs-lookup"><span data-stu-id="7d388-385">Int32</span></span> |
| <span data-ttu-id="7d388-386">Money</span><span class="sxs-lookup"><span data-stu-id="7d388-386">money</span></span> |<span data-ttu-id="7d388-387">Decimal</span><span class="sxs-lookup"><span data-stu-id="7d388-387">Decimal</span></span> |
| <span data-ttu-id="7d388-388">nchar</span><span class="sxs-lookup"><span data-stu-id="7d388-388">nchar</span></span> |<span data-ttu-id="7d388-389">Sträng, Char]</span><span class="sxs-lookup"><span data-stu-id="7d388-389">String, Char[]</span></span> |
| <span data-ttu-id="7d388-390">ntext</span><span class="sxs-lookup"><span data-stu-id="7d388-390">ntext</span></span> |<span data-ttu-id="7d388-391">Sträng, Char]</span><span class="sxs-lookup"><span data-stu-id="7d388-391">String, Char[]</span></span> |
| <span data-ttu-id="7d388-392">numeriskt</span><span class="sxs-lookup"><span data-stu-id="7d388-392">numeric</span></span> |<span data-ttu-id="7d388-393">Decimal</span><span class="sxs-lookup"><span data-stu-id="7d388-393">Decimal</span></span> |
| <span data-ttu-id="7d388-394">nvarchar</span><span class="sxs-lookup"><span data-stu-id="7d388-394">nvarchar</span></span> |<span data-ttu-id="7d388-395">Sträng, Char]</span><span class="sxs-lookup"><span data-stu-id="7d388-395">String, Char[]</span></span> |
| <span data-ttu-id="7d388-396">Verklig</span><span class="sxs-lookup"><span data-stu-id="7d388-396">real</span></span> |<span data-ttu-id="7d388-397">Enskild</span><span class="sxs-lookup"><span data-stu-id="7d388-397">Single</span></span> |
| <span data-ttu-id="7d388-398">ROWVERSION</span><span class="sxs-lookup"><span data-stu-id="7d388-398">rowversion</span></span> |<span data-ttu-id="7d388-399">byte]</span><span class="sxs-lookup"><span data-stu-id="7d388-399">Byte[]</span></span> |
| <span data-ttu-id="7d388-400">smalldatetime</span><span class="sxs-lookup"><span data-stu-id="7d388-400">smalldatetime</span></span> |<span data-ttu-id="7d388-401">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="7d388-401">DateTime</span></span> |
| <span data-ttu-id="7d388-402">smallint</span><span class="sxs-lookup"><span data-stu-id="7d388-402">smallint</span></span> |<span data-ttu-id="7d388-403">Int16</span><span class="sxs-lookup"><span data-stu-id="7d388-403">Int16</span></span> |
| <span data-ttu-id="7d388-404">smallmoney</span><span class="sxs-lookup"><span data-stu-id="7d388-404">smallmoney</span></span> |<span data-ttu-id="7d388-405">Decimal</span><span class="sxs-lookup"><span data-stu-id="7d388-405">Decimal</span></span> |
| <span data-ttu-id="7d388-406">sql_variant</span><span class="sxs-lookup"><span data-stu-id="7d388-406">sql_variant</span></span> |<span data-ttu-id="7d388-407">Objektet *</span><span class="sxs-lookup"><span data-stu-id="7d388-407">Object *</span></span> |
| <span data-ttu-id="7d388-408">Text</span><span class="sxs-lookup"><span data-stu-id="7d388-408">text</span></span> |<span data-ttu-id="7d388-409">Sträng, Char]</span><span class="sxs-lookup"><span data-stu-id="7d388-409">String, Char[]</span></span> |
| <span data-ttu-id="7d388-410">time</span><span class="sxs-lookup"><span data-stu-id="7d388-410">time</span></span> |<span data-ttu-id="7d388-411">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="7d388-411">TimeSpan</span></span> |
| <span data-ttu-id="7d388-412">tidsstämpel</span><span class="sxs-lookup"><span data-stu-id="7d388-412">timestamp</span></span> |<span data-ttu-id="7d388-413">byte]</span><span class="sxs-lookup"><span data-stu-id="7d388-413">Byte[]</span></span> |
| <span data-ttu-id="7d388-414">tinyint</span><span class="sxs-lookup"><span data-stu-id="7d388-414">tinyint</span></span> |<span data-ttu-id="7d388-415">Mottagna byte</span><span class="sxs-lookup"><span data-stu-id="7d388-415">Byte</span></span> |
| <span data-ttu-id="7d388-416">Unik identifierare</span><span class="sxs-lookup"><span data-stu-id="7d388-416">uniqueidentifier</span></span> |<span data-ttu-id="7d388-417">GUID</span><span class="sxs-lookup"><span data-stu-id="7d388-417">Guid</span></span> |
| <span data-ttu-id="7d388-418">varbinary</span><span class="sxs-lookup"><span data-stu-id="7d388-418">varbinary</span></span> |<span data-ttu-id="7d388-419">byte]</span><span class="sxs-lookup"><span data-stu-id="7d388-419">Byte[]</span></span> |
| <span data-ttu-id="7d388-420">varchar</span><span class="sxs-lookup"><span data-stu-id="7d388-420">varchar</span></span> |<span data-ttu-id="7d388-421">Sträng, Char]</span><span class="sxs-lookup"><span data-stu-id="7d388-421">String, Char[]</span></span> |
| <span data-ttu-id="7d388-422">xml</span><span class="sxs-lookup"><span data-stu-id="7d388-422">xml</span></span> |<span data-ttu-id="7d388-423">XML</span><span class="sxs-lookup"><span data-stu-id="7d388-423">Xml</span></span> |

## <a name="mapping-source-toosink-columns"></a><span data-ttu-id="7d388-424">Mappning källkolumner toosink</span><span class="sxs-lookup"><span data-stu-id="7d388-424">Mapping source toosink columns</span></span>
<span data-ttu-id="7d388-425">toomap kolumner från källan dataset toocolumns från sink dataset finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="7d388-425">toomap columns from source dataset toocolumns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-copy"></a><span data-ttu-id="7d388-426">Repeterbara kopia</span><span class="sxs-lookup"><span data-stu-id="7d388-426">Repeatable copy</span></span>
<span data-ttu-id="7d388-427">När du kopierar data tooSQL Server-databasen läggs hello kopieringsaktiviteten datatabell toohello mottagare som standard.</span><span class="sxs-lookup"><span data-stu-id="7d388-427">When copying data tooSQL Server Database, hello copy activity appends data toohello sink table by default.</span></span> <span data-ttu-id="7d388-428">tooperform en UPSERT Läs [repeterbara skrivåtgärder tooSqlSink](data-factory-repeatable-copy.md#repeatable-write-to-sqlsink) artikel.</span><span class="sxs-lookup"><span data-stu-id="7d388-428">tooperform an UPSERT instead,  See [Repeatable write tooSqlSink](data-factory-repeatable-copy.md#repeatable-write-to-sqlsink) article.</span></span> 

<span data-ttu-id="7d388-429">När du kopierar data från relationella datalager, Kom ihåg tooavoid repeterbarhet oönskade resultat.</span><span class="sxs-lookup"><span data-stu-id="7d388-429">When copying data from relational data stores, keep repeatability in mind tooavoid unintended outcomes.</span></span> <span data-ttu-id="7d388-430">I Azure Data Factory, kan du köra en sektor manuellt.</span><span class="sxs-lookup"><span data-stu-id="7d388-430">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="7d388-431">Du kan också konfigurera i principen för en dataset så att ett segment som körs när ett fel uppstår.</span><span class="sxs-lookup"><span data-stu-id="7d388-431">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="7d388-432">När ett segment körs på antingen sätt måste toomake att som hello samma data läses oavsett hur många gånger ett segment körs.</span><span class="sxs-lookup"><span data-stu-id="7d388-432">When a slice is rerun in either way, you need toomake sure that hello same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="7d388-433">Se [Repeatable läsa från relationella källor](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="7d388-433">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="7d388-434">Prestanda och finjustering</span><span class="sxs-lookup"><span data-stu-id="7d388-434">Performance and Tuning</span></span>
<span data-ttu-id="7d388-435">Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) toolearn om nyckeln faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt toooptimize den.</span><span class="sxs-lookup"><span data-stu-id="7d388-435">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
