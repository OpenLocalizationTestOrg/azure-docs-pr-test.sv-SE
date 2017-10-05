---
title: "Flytta data till och från SQL Server | Microsoft Docs"
description: "Lär dig mer om hur du flyttar data till/från SQL Server-databas som är lokalt eller i en virtuell Azure-dator med hjälp av Azure Data Factory."
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
ms.openlocfilehash: 9cd2077d897631457925cda5ef5e6df3c0c33177
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="move-data-to-and-from-sql-server-on-premises-or-on-iaas-azure-vm-using-azure-data-factory"></a><span data-ttu-id="8052c-103">Flytta data till och från SQL Server lokalt eller på IaaS (Azure VM) med hjälp av Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="8052c-103">Move data to and from SQL Server on-premises or on IaaS (Azure VM) using Azure Data Factory</span></span>
<span data-ttu-id="8052c-104">Den här artikeln förklarar hur du använder aktiviteten kopiera i Azure Data Factory för att flytta data till/från en lokal SQL Server-databas.</span><span class="sxs-lookup"><span data-stu-id="8052c-104">This article explains how to use the Copy Activity in Azure Data Factory to move data to/from an on-premises SQL Server database.</span></span> <span data-ttu-id="8052c-105">Den bygger på den [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikel som presenterar en allmän översikt över dataflyttning med copy-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="8052c-105">It builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span> 

## <a name="supported-scenarios"></a><span data-ttu-id="8052c-106">Scenarier som stöds</span><span class="sxs-lookup"><span data-stu-id="8052c-106">Supported scenarios</span></span>
<span data-ttu-id="8052c-107">Du kan kopiera data **från en SQL Server-databas** till följande data lagras:</span><span class="sxs-lookup"><span data-stu-id="8052c-107">You can copy data **from a SQL Server database** to the following data stores:</span></span>

[!INCLUDE [data-factory-supported-sink](../../includes/data-factory-supported-sinks.md)]

<span data-ttu-id="8052c-108">Du kan kopiera data från följande datalager **till en SQL Server-databas**:</span><span class="sxs-lookup"><span data-stu-id="8052c-108">You can copy data from the following data stores **to a SQL Server database**:</span></span>

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

## <a name="supported-sql-server-versions"></a><span data-ttu-id="8052c-109">SQL Server-versioner som stöds</span><span class="sxs-lookup"><span data-stu-id="8052c-109">Supported SQL Server versions</span></span>
<span data-ttu-id="8052c-110">Den här connector-stöd för SQL Server som kopierar data från/till följande versioner av instansen finns på lokalt eller i Azure IaaS med både SQL-autentisering och Windows-autentisering: SQL Server 2016, SQL Server 2014, SQL Server 2012, SQL Server 2008 R2, SQL Server 2008, SQLServer 2005</span><span class="sxs-lookup"><span data-stu-id="8052c-110">This SQL Server connector support copying data from/to the following versions of instance hosted on-premises or in Azure IaaS using both SQL authentication and Windows authentication: SQL Server 2016, SQL Server 2014, SQL Server 2012, SQL Server 2008 R2, SQL Server 2008, SQL Server 2005</span></span>

## <a name="enabling-connectivity"></a><span data-ttu-id="8052c-111">Aktivera anslutning</span><span class="sxs-lookup"><span data-stu-id="8052c-111">Enabling connectivity</span></span>
<span data-ttu-id="8052c-112">Koncept och steg som krävs för att ansluta med SQL Server finns lokalt eller i Azure IaaS (Infrastructure-as-a-Service) virtuella datorer är samma.</span><span class="sxs-lookup"><span data-stu-id="8052c-112">The concepts and steps needed for connecting with SQL Server hosted on-premises or in Azure IaaS (Infrastructure-as-a-Service) VMs are the same.</span></span> <span data-ttu-id="8052c-113">I båda fallen måste behöver du använda Data Management Gateway-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="8052c-113">In both cases, you need to use Data Management Gateway for connectivity.</span></span>

<span data-ttu-id="8052c-114">Se [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikeln innehåller information om Data Management Gateway och stegvisa instruktioner om hur du konfigurerar en gateway.</span><span class="sxs-lookup"><span data-stu-id="8052c-114">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article to learn about Data Management Gateway and step-by-step instructions on setting up the gateway.</span></span> <span data-ttu-id="8052c-115">Ställa in en gateway-instans är ett krav för att ansluta med SQL Server.</span><span class="sxs-lookup"><span data-stu-id="8052c-115">Setting up a gateway instance is a pre-requisite for connecting with SQL Server.</span></span>

<span data-ttu-id="8052c-116">Medan du kan installera gatewayen på samma lokala dator eller molnet VM-instans som SQL-servern för bättre prestanda, rekommenderar vi att du installerar dem på separata datorer.</span><span class="sxs-lookup"><span data-stu-id="8052c-116">While you can install gateway on the same on-premises machine or cloud VM instance as the SQL Server for better performance, we recommended that you install them on separate machines.</span></span> <span data-ttu-id="8052c-117">Med gatewayen och SQL Server på separata datorer minskar resurskonflikter.</span><span class="sxs-lookup"><span data-stu-id="8052c-117">Having the gateway and SQL Server on separate machines reduces resource contention.</span></span>

## <a name="getting-started"></a><span data-ttu-id="8052c-118">Komma igång</span><span class="sxs-lookup"><span data-stu-id="8052c-118">Getting started</span></span>
<span data-ttu-id="8052c-119">Du kan skapa en pipeline med en kopia-aktivitet som flyttar data till/från en lokal SQL Server-databas med hjälp av olika verktyg/API: er.</span><span class="sxs-lookup"><span data-stu-id="8052c-119">You can create a pipeline with a copy activity that moves data to/from an on-premises SQL Server database by using different tools/APIs.</span></span>

<span data-ttu-id="8052c-120">Det enklaste sättet att skapa en pipeline är att använda den **guiden Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="8052c-120">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="8052c-121">Finns [Självstudier: skapa en pipeline med hjälp av guiden Kopiera](data-factory-copy-data-wizard-tutorial.md) för en snabb genomgång om hur du skapar en pipeline med hjälp av guiden Kopiera data.</span><span class="sxs-lookup"><span data-stu-id="8052c-121">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span>

<span data-ttu-id="8052c-122">Du kan också använda följande verktyg för att skapa en pipeline: **Azure-portalen**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-mall**, **.NET API**, och **REST API**.</span><span class="sxs-lookup"><span data-stu-id="8052c-122">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="8052c-123">Se [kopiera aktivitet kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för stegvisa instruktioner för att skapa en pipeline med en Kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="8052c-123">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> 

<span data-ttu-id="8052c-124">Om du använder verktyg eller API: er, kan du utföra följande steg för att skapa en pipeline som flyttar data från ett dataarkiv som källa till ett dataarkiv som mottagare:</span><span class="sxs-lookup"><span data-stu-id="8052c-124">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span> 

1. <span data-ttu-id="8052c-125">Skapa en **datafabriken**.</span><span class="sxs-lookup"><span data-stu-id="8052c-125">Create a **data factory**.</span></span> <span data-ttu-id="8052c-126">En datafabrik kan innehålla en eller flera pipelines.</span><span class="sxs-lookup"><span data-stu-id="8052c-126">A data factory may contain one or more pipelines.</span></span> 
2. <span data-ttu-id="8052c-127">Skapa **länkade tjänster** att länka inkommande och utgående data lagras till din data factory.</span><span class="sxs-lookup"><span data-stu-id="8052c-127">Create **linked services** to link input and output data stores to your data factory.</span></span> <span data-ttu-id="8052c-128">Till exempel om du kopierar data från en SQL Server-databas till en Azure blob storage måste skapa du två länkade tjänster om du vill länka till SQL Server-databasen och Azure storage-konto till din data factory.</span><span class="sxs-lookup"><span data-stu-id="8052c-128">For example, if you are copying data from a SQL Server database to an Azure blob storage, you create two linked services to link your SQL Server database and Azure storage account to your data factory.</span></span> <span data-ttu-id="8052c-129">Länkad tjänstegenskaper som är specifika för SQL Server-databasen, se [länkade tjänstegenskaper](#linked-service-properties) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="8052c-129">For linked service properties that are specific to SQL Server database, see [linked service properties](#linked-service-properties) section.</span></span> 
3. <span data-ttu-id="8052c-130">Skapa **datauppsättningar** att representera inkommande och utgående data för kopieringen.</span><span class="sxs-lookup"><span data-stu-id="8052c-130">Create **datasets** to represent input and output data for the copy operation.</span></span> <span data-ttu-id="8052c-131">I det tidigare exemplet i det sista steget, skapar du en datauppsättning för att ange SQL-tabell i SQL Server-databasen som innehåller indata.</span><span class="sxs-lookup"><span data-stu-id="8052c-131">In the example mentioned in the last step, you create a dataset to specify the SQL table in your SQL Server database that contains the input data.</span></span> <span data-ttu-id="8052c-132">Och du skapar en annan dataset för att ange blob-behållaren och mappen som innehåller de data som kopieras från SQL Server-databasen.</span><span class="sxs-lookup"><span data-stu-id="8052c-132">And, you create another dataset to specify the blob container and the folder that holds the data copied from the SQL Server database.</span></span> <span data-ttu-id="8052c-133">Egenskaper för datamängd som är specifika för SQL Server-databasen, se [egenskaper för datamängd](#dataset-properties) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="8052c-133">For dataset properties that are specific to SQL Server database, see [dataset properties](#dataset-properties) section.</span></span>
4. <span data-ttu-id="8052c-134">Skapa en **pipeline** med en kopia-aktivitet som tar en datamängd som indata och en dataset som utdata.</span><span class="sxs-lookup"><span data-stu-id="8052c-134">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> <span data-ttu-id="8052c-135">I exemplet ovan kan använda du SqlSource som en källa och BlobSink som en mottagare för kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="8052c-135">In the example mentioned earlier, you use SqlSource as a source and BlobSink as a sink for the copy activity.</span></span> <span data-ttu-id="8052c-136">På samma sätt om du kopierar från Azure Blob Storage till SQL Server-databas, använder du BlobSource och SqlSink i kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="8052c-136">Similarly, if you are copying from Azure Blob Storage to SQL Server Database, you use BlobSource and SqlSink in the copy activity.</span></span> <span data-ttu-id="8052c-137">Kopiera Aktivitetsegenskaper som är specifika för SQL Server-databas, se [kopiera Aktivitetsegenskaper](#copy-activity-properties) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="8052c-137">For copy activity properties that are specific to SQL Server Database, see [copy activity properties](#copy-activity-properties) section.</span></span> <span data-ttu-id="8052c-138">Mer information om hur du använder ett dataarkiv som en källa eller en mottagare klickar du på länken i föregående avsnitt för datalager.</span><span class="sxs-lookup"><span data-stu-id="8052c-138">For details on how to use a data store as a source or a sink, click the link in the previous section for your data store.</span></span> 

<span data-ttu-id="8052c-139">När du använder guiden skapas JSON definitioner för dessa Data Factory-enheter (länkade tjänster, datauppsättningar och pipelinen) automatiskt för dig.</span><span class="sxs-lookup"><span data-stu-id="8052c-139">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="8052c-140">När du använder Verktyg/API: er (utom .NET API), kan du definiera dessa Data Factory-enheter med hjälp av JSON-format.</span><span class="sxs-lookup"><span data-stu-id="8052c-140">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="8052c-141">Exempel med JSON-definitioner för Data Factory-entiteter som används för att kopiera data till/från en lokal SQL Server-databas finns [JSON-exempel](#json-examples-for-copying-data-from-and-to-sql-server) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="8052c-141">For samples with JSON definitions for Data Factory entities that are used to copy data to/from an on-premises SQL Server database, see [JSON examples](#json-examples-for-copying-data-from-and-to-sql-server) section of this article.</span></span> 

<span data-ttu-id="8052c-142">Följande avsnitt innehåller information om JSON-egenskaper som används för att definiera Data Factory entiteter till SQL Server:</span><span class="sxs-lookup"><span data-stu-id="8052c-142">The following sections provide details about JSON properties that are used to define Data Factory entities specific to SQL Server:</span></span> 

## <a name="linked-service-properties"></a><span data-ttu-id="8052c-143">Länkad tjänstegenskaper</span><span class="sxs-lookup"><span data-stu-id="8052c-143">Linked service properties</span></span>
<span data-ttu-id="8052c-144">Du skapar en länkad tjänst av typen **OnPremisesSqlServer** att länka en lokal SQL Server-databas till en datafabrik.</span><span class="sxs-lookup"><span data-stu-id="8052c-144">You create a linked service of type **OnPremisesSqlServer** to link an on-premises SQL Server database to a data factory.</span></span> <span data-ttu-id="8052c-145">Följande tabell innehåller en beskrivning för JSON-element som är specifika för lokala SQL Server länkad tjänst.</span><span class="sxs-lookup"><span data-stu-id="8052c-145">The following table provides description for JSON elements specific to on-premises SQL Server linked service.</span></span>

<span data-ttu-id="8052c-146">Följande tabell innehåller en beskrivning för JSON-element som är specifika för SQL Server som är länkad tjänst.</span><span class="sxs-lookup"><span data-stu-id="8052c-146">The following table provides description for JSON elements specific to SQL Server linked service.</span></span>

| <span data-ttu-id="8052c-147">Egenskap</span><span class="sxs-lookup"><span data-stu-id="8052c-147">Property</span></span> | <span data-ttu-id="8052c-148">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8052c-148">Description</span></span> | <span data-ttu-id="8052c-149">Krävs</span><span class="sxs-lookup"><span data-stu-id="8052c-149">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8052c-150">typ</span><span class="sxs-lookup"><span data-stu-id="8052c-150">type</span></span> |<span data-ttu-id="8052c-151">Typegenskapen bör anges till: **OnPremisesSqlServer**.</span><span class="sxs-lookup"><span data-stu-id="8052c-151">The type property should be set to: **OnPremisesSqlServer**.</span></span> |<span data-ttu-id="8052c-152">Ja</span><span class="sxs-lookup"><span data-stu-id="8052c-152">Yes</span></span> |
| <span data-ttu-id="8052c-153">connectionString</span><span class="sxs-lookup"><span data-stu-id="8052c-153">connectionString</span></span> |<span data-ttu-id="8052c-154">Ange connectionString information som behövs för att ansluta till lokal SQL Server-databasen med hjälp av SQL-autentisering eller Windows-autentisering.</span><span class="sxs-lookup"><span data-stu-id="8052c-154">Specify connectionString information needed to connect to the on-premises SQL Server database using either SQL authentication or Windows authentication.</span></span> |<span data-ttu-id="8052c-155">Ja</span><span class="sxs-lookup"><span data-stu-id="8052c-155">Yes</span></span> |
| <span data-ttu-id="8052c-156">gatewayName</span><span class="sxs-lookup"><span data-stu-id="8052c-156">gatewayName</span></span> |<span data-ttu-id="8052c-157">Namnet på den gateway som Data Factory-tjänsten ska använda för att ansluta till lokal SQL Server-databasen.</span><span class="sxs-lookup"><span data-stu-id="8052c-157">Name of the gateway that the Data Factory service should use to connect to the on-premises SQL Server database.</span></span> |<span data-ttu-id="8052c-158">Ja</span><span class="sxs-lookup"><span data-stu-id="8052c-158">Yes</span></span> |
| <span data-ttu-id="8052c-159">användarnamn</span><span class="sxs-lookup"><span data-stu-id="8052c-159">username</span></span> |<span data-ttu-id="8052c-160">Ange användarnamnet om du använder Windows-autentisering.</span><span class="sxs-lookup"><span data-stu-id="8052c-160">Specify user name if you are using Windows Authentication.</span></span> <span data-ttu-id="8052c-161">Exempel: **domainname\\användarnamn**.</span><span class="sxs-lookup"><span data-stu-id="8052c-161">Example: **domainname\\username**.</span></span> |<span data-ttu-id="8052c-162">Nej</span><span class="sxs-lookup"><span data-stu-id="8052c-162">No</span></span> |
| <span data-ttu-id="8052c-163">lösenord</span><span class="sxs-lookup"><span data-stu-id="8052c-163">password</span></span> |<span data-ttu-id="8052c-164">Ange lösenordet för det användarkonto som du angav för användarnamnet.</span><span class="sxs-lookup"><span data-stu-id="8052c-164">Specify password for the user account you specified for the username.</span></span> |<span data-ttu-id="8052c-165">Nej</span><span class="sxs-lookup"><span data-stu-id="8052c-165">No</span></span> |

<span data-ttu-id="8052c-166">Du kan kryptera autentiseringsuppgifterna med hjälp av den **ny AzureRmDataFactoryEncryptValue** cmdlet och använda dem i anslutningssträngen som visas i följande exempel (**EncryptedCredential** egenskapen):</span><span class="sxs-lookup"><span data-stu-id="8052c-166">You can encrypt credentials using the **New-AzureRmDataFactoryEncryptValue** cmdlet and use them in the connection string as shown in the following example (**EncryptedCredential** property):</span></span>  

```JSON
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```

### <a name="samples"></a><span data-ttu-id="8052c-167">Exempel</span><span class="sxs-lookup"><span data-stu-id="8052c-167">Samples</span></span>
<span data-ttu-id="8052c-168">**JSON för att använda SQL-autentisering**</span><span class="sxs-lookup"><span data-stu-id="8052c-168">**JSON for using SQL Authentication**</span></span>

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
<span data-ttu-id="8052c-169">**JSON för att använda Windows-autentisering**</span><span class="sxs-lookup"><span data-stu-id="8052c-169">**JSON for using Windows Authentication**</span></span>

<span data-ttu-id="8052c-170">Data Management Gateway kommer att personifiera det angivna användarkontot kan ansluta till lokal SQL Server-databasen.</span><span class="sxs-lookup"><span data-stu-id="8052c-170">Data Management Gateway will impersonate the specified user account to connect to the on-premises SQL Server database.</span></span> 

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

## <a name="dataset-properties"></a><span data-ttu-id="8052c-171">Egenskaper för datamängd</span><span class="sxs-lookup"><span data-stu-id="8052c-171">Dataset properties</span></span>
<span data-ttu-id="8052c-172">I prover, du har använt ett DataSet-objekt av typen **SqlServerTable** som representerar en tabell i SQL Server-databasen.</span><span class="sxs-lookup"><span data-stu-id="8052c-172">In the samples, you have used a dataset of type **SqlServerTable** to represent a table in a SQL Server database.</span></span>  

<span data-ttu-id="8052c-173">En fullständig lista över egenskaper som är tillgängliga för att definiera datauppsättningarna & avsnitt finns i [skapa datauppsättningar](data-factory-create-datasets.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="8052c-173">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="8052c-174">Avsnitt som struktur, tillgänglighet och princip på en datamängd JSON är liknande för alla typer av datauppsättningen (SQL Server, Azure blob, Azure-tabellen, osv.).</span><span class="sxs-lookup"><span data-stu-id="8052c-174">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (SQL Server, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="8052c-175">Avsnittet typeProperties är olika för varje typ av dataset och ger information om placeringen av data i datalagret.</span><span class="sxs-lookup"><span data-stu-id="8052c-175">The typeProperties section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="8052c-176">Den **typeProperties** avsnittet för datauppsättningen av typen **SqlServerTable** har följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="8052c-176">The **typeProperties** section for the dataset of type **SqlServerTable** has the following properties:</span></span>

| <span data-ttu-id="8052c-177">Egenskap</span><span class="sxs-lookup"><span data-stu-id="8052c-177">Property</span></span> | <span data-ttu-id="8052c-178">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8052c-178">Description</span></span> | <span data-ttu-id="8052c-179">Krävs</span><span class="sxs-lookup"><span data-stu-id="8052c-179">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8052c-180">tableName</span><span class="sxs-lookup"><span data-stu-id="8052c-180">tableName</span></span> |<span data-ttu-id="8052c-181">Namnet på den tabell eller vy i SQL Server-databasinstansen som den länkade tjänsten refererar till.</span><span class="sxs-lookup"><span data-stu-id="8052c-181">Name of the table or view in the SQL Server Database instance that linked service refers to.</span></span> |<span data-ttu-id="8052c-182">Ja</span><span class="sxs-lookup"><span data-stu-id="8052c-182">Yes</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="8052c-183">Kopiera egenskaper för aktivitet</span><span class="sxs-lookup"><span data-stu-id="8052c-183">Copy activity properties</span></span>
<span data-ttu-id="8052c-184">Om du flyttar data från en SQL Server-databas måste du ange källtypen i kopieringsaktiviteten till **SqlSource**.</span><span class="sxs-lookup"><span data-stu-id="8052c-184">If you are moving data from a SQL Server database, you set the source type in the copy activity to **SqlSource**.</span></span> <span data-ttu-id="8052c-185">På samma sätt om du flyttar data till en SQL Server-databas måste du anger sink i kopieringsaktiviteten till **SqlSink**.</span><span class="sxs-lookup"><span data-stu-id="8052c-185">Similarly, if you are moving data to a SQL Server database, you set the sink type in the copy activity to **SqlSink**.</span></span> <span data-ttu-id="8052c-186">Det här avsnittet innehåller en lista över egenskaper som stöds av SqlSource och SqlSink.</span><span class="sxs-lookup"><span data-stu-id="8052c-186">This section provides a list of properties supported by SqlSource and SqlSink.</span></span>

<span data-ttu-id="8052c-187">En fullständig lista över avsnitt & egenskaper som är tillgängliga för att definiera aktiviteter finns i [skapar Pipelines](data-factory-create-pipelines.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="8052c-187">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="8052c-188">Egenskaper som namn, beskrivning, ingående och utgående tabeller och principer är tillgängliga för alla typer av aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="8052c-188">Properties such as name, description, input and output tables, and policies are available for all types of activities.</span></span>

> [!NOTE]
> <span data-ttu-id="8052c-189">Kopieringsaktiviteten tar endast en inmatning och ger en enda utdata.</span><span class="sxs-lookup"><span data-stu-id="8052c-189">The Copy Activity takes only one input and produces only one output.</span></span>

<span data-ttu-id="8052c-190">De egenskaper som är tillgängliga i avsnittet typeProperties i aktiviteten varierar beroende på varje aktivitetstyp.</span><span class="sxs-lookup"><span data-stu-id="8052c-190">Whereas, properties available in the typeProperties section of the activity vary with each activity type.</span></span> <span data-ttu-id="8052c-191">För Kopieringsaktivitet kan variera de beroende på vilka typer av datakällor och sänkor.</span><span class="sxs-lookup"><span data-stu-id="8052c-191">For Copy activity, they vary depending on the types of sources and sinks.</span></span>

### <a name="sqlsource"></a><span data-ttu-id="8052c-192">SqlSource</span><span class="sxs-lookup"><span data-stu-id="8052c-192">SqlSource</span></span>
<span data-ttu-id="8052c-193">När datakällan i en Kopieringsaktivitet är av typen **SqlSource**, följande egenskaper är tillgängliga i **typeProperties** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="8052c-193">When source in a copy activity is of type **SqlSource**, the following properties are available in **typeProperties** section:</span></span>

| <span data-ttu-id="8052c-194">Egenskap</span><span class="sxs-lookup"><span data-stu-id="8052c-194">Property</span></span> | <span data-ttu-id="8052c-195">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8052c-195">Description</span></span> | <span data-ttu-id="8052c-196">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="8052c-196">Allowed values</span></span> | <span data-ttu-id="8052c-197">Krävs</span><span class="sxs-lookup"><span data-stu-id="8052c-197">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="8052c-198">sqlReaderQuery</span><span class="sxs-lookup"><span data-stu-id="8052c-198">sqlReaderQuery</span></span> |<span data-ttu-id="8052c-199">Använd anpassad fråga för att läsa data.</span><span class="sxs-lookup"><span data-stu-id="8052c-199">Use the custom query to read data.</span></span> |<span data-ttu-id="8052c-200">SQL-sträng.</span><span class="sxs-lookup"><span data-stu-id="8052c-200">SQL query string.</span></span> <span data-ttu-id="8052c-201">Till exempel: Välj * från mytable prefix.</span><span class="sxs-lookup"><span data-stu-id="8052c-201">For example: select * from MyTable.</span></span> <span data-ttu-id="8052c-202">Kan referera till flera tabeller från databasen som refereras av inkommande dataset.</span><span class="sxs-lookup"><span data-stu-id="8052c-202">May reference multiple tables from the database referenced by the input dataset.</span></span> <span data-ttu-id="8052c-203">Om inget annat anges, SQL-instruktionen som körs: Välj från mytable prefix.</span><span class="sxs-lookup"><span data-stu-id="8052c-203">If not specified, the SQL statement that is executed: select from MyTable.</span></span> |<span data-ttu-id="8052c-204">Nej</span><span class="sxs-lookup"><span data-stu-id="8052c-204">No</span></span> |
| <span data-ttu-id="8052c-205">sqlReaderStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="8052c-205">sqlReaderStoredProcedureName</span></span> |<span data-ttu-id="8052c-206">Namnet på den lagrade proceduren som läser data från källtabellen.</span><span class="sxs-lookup"><span data-stu-id="8052c-206">Name of the stored procedure that reads data from the source table.</span></span> |<span data-ttu-id="8052c-207">Namnet på den lagrade proceduren.</span><span class="sxs-lookup"><span data-stu-id="8052c-207">Name of the stored procedure.</span></span> <span data-ttu-id="8052c-208">Den senaste SQL-instruktionen måste vara en SELECT-instruktion i den lagrade proceduren.</span><span class="sxs-lookup"><span data-stu-id="8052c-208">The last SQL statement must be a SELECT statement in the stored procedure.</span></span> |<span data-ttu-id="8052c-209">Nej</span><span class="sxs-lookup"><span data-stu-id="8052c-209">No</span></span> |
| <span data-ttu-id="8052c-210">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="8052c-210">storedProcedureParameters</span></span> |<span data-ttu-id="8052c-211">Parametrar för den lagrade proceduren.</span><span class="sxs-lookup"><span data-stu-id="8052c-211">Parameters for the stored procedure.</span></span> |<span data-ttu-id="8052c-212">Namn/värde-par.</span><span class="sxs-lookup"><span data-stu-id="8052c-212">Name/value pairs.</span></span> <span data-ttu-id="8052c-213">Namn och skiftläge parametrar måste matcha namn och versaler och gemener i parametrarna för lagrade procedurer.</span><span class="sxs-lookup"><span data-stu-id="8052c-213">Names and casing of parameters must match the names and casing of the stored procedure parameters.</span></span> |<span data-ttu-id="8052c-214">Nej</span><span class="sxs-lookup"><span data-stu-id="8052c-214">No</span></span> |

<span data-ttu-id="8052c-215">Om den **sqlReaderQuery** har angetts för SqlSource, kopiera-aktivitet körs den här frågan mot SQL Server-databas källan för att hämta data.</span><span class="sxs-lookup"><span data-stu-id="8052c-215">If the **sqlReaderQuery** is specified for the SqlSource, the Copy Activity runs this query against the SQL Server Database source to get the data.</span></span>

<span data-ttu-id="8052c-216">Du kan också ange en lagrad procedur genom att ange den **sqlReaderStoredProcedureName** och **storedProcedureParameters** (om den lagrade proceduren har parametrar).</span><span class="sxs-lookup"><span data-stu-id="8052c-216">Alternatively, you can specify a stored procedure by specifying the **sqlReaderStoredProcedureName** and **storedProcedureParameters** (if the stored procedure takes parameters).</span></span>

<span data-ttu-id="8052c-217">Om du inte anger sqlReaderQuery eller sqlReaderStoredProcedureName, används de kolumner som anges i strukturen för att skapa en select-frågan ska köras mot SQL Server-databasen.</span><span class="sxs-lookup"><span data-stu-id="8052c-217">If you do not specify either sqlReaderQuery or sqlReaderStoredProcedureName, the columns defined in the structure section are used to build a select query to run against the SQL Server Database.</span></span> <span data-ttu-id="8052c-218">Om datauppsättningsdefinitionen saknar strukturen, markeras alla kolumner från tabellen.</span><span class="sxs-lookup"><span data-stu-id="8052c-218">If the dataset definition does not have the structure, all columns are selected from the table.</span></span>

> [!NOTE]
> <span data-ttu-id="8052c-219">När du använder **sqlReaderStoredProcedureName**, du måste ange ett värde för den **tableName** egenskap i datauppsättningen JSON.</span><span class="sxs-lookup"><span data-stu-id="8052c-219">When you use **sqlReaderStoredProcedureName**, you still need to specify a value for the **tableName** property in the dataset JSON.</span></span> <span data-ttu-id="8052c-220">Det finns inga verifieringar utföras om mot den här tabellen.</span><span class="sxs-lookup"><span data-stu-id="8052c-220">There are no validations performed against this table though.</span></span>

### <a name="sqlsink"></a><span data-ttu-id="8052c-221">SqlSink</span><span class="sxs-lookup"><span data-stu-id="8052c-221">SqlSink</span></span>
<span data-ttu-id="8052c-222">**SqlSink** stöder följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="8052c-222">**SqlSink** supports the following properties:</span></span>

| <span data-ttu-id="8052c-223">Egenskap</span><span class="sxs-lookup"><span data-stu-id="8052c-223">Property</span></span> | <span data-ttu-id="8052c-224">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8052c-224">Description</span></span> | <span data-ttu-id="8052c-225">Tillåtna värden</span><span class="sxs-lookup"><span data-stu-id="8052c-225">Allowed values</span></span> | <span data-ttu-id="8052c-226">Krävs</span><span class="sxs-lookup"><span data-stu-id="8052c-226">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="8052c-227">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="8052c-227">writeBatchTimeout</span></span> |<span data-ttu-id="8052c-228">Vänta tills batch insert-åtgärden ska slutföras innan tidsgränsen uppnås.</span><span class="sxs-lookup"><span data-stu-id="8052c-228">Wait time for the batch insert operation to complete before it times out.</span></span> |<span data-ttu-id="8052c-229">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="8052c-229">timespan</span></span><br/><br/> <span data-ttu-id="8052c-230">Exempel ”: 00: 30:00” (30 minuter).</span><span class="sxs-lookup"><span data-stu-id="8052c-230">Example: “00:30:00” (30 minutes).</span></span> |<span data-ttu-id="8052c-231">Nej</span><span class="sxs-lookup"><span data-stu-id="8052c-231">No</span></span> |
| <span data-ttu-id="8052c-232">writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="8052c-232">writeBatchSize</span></span> |<span data-ttu-id="8052c-233">Infogar data i SQL-tabellen när buffertstorleken når writeBatchSize.</span><span class="sxs-lookup"><span data-stu-id="8052c-233">Inserts data into the SQL table when the buffer size reaches writeBatchSize.</span></span> |<span data-ttu-id="8052c-234">Heltal (antalet rader)</span><span class="sxs-lookup"><span data-stu-id="8052c-234">Integer (number of rows)</span></span> |<span data-ttu-id="8052c-235">Nej (standard: 10000)</span><span class="sxs-lookup"><span data-stu-id="8052c-235">No (default: 10000)</span></span> |
| <span data-ttu-id="8052c-236">sqlWriterCleanupScript</span><span class="sxs-lookup"><span data-stu-id="8052c-236">sqlWriterCleanupScript</span></span> |<span data-ttu-id="8052c-237">Ange frågan för Kopieringsaktiviteten att köra så att data för ett visst segment har rensats bort.</span><span class="sxs-lookup"><span data-stu-id="8052c-237">Specify query for Copy Activity to execute such that data of a specific slice is cleaned up.</span></span> <span data-ttu-id="8052c-238">Mer information finns i [repeterbara kopiera](#repeatable-copy) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="8052c-238">For more information, see [repeatable copy](#repeatable-copy) section.</span></span> |<span data-ttu-id="8052c-239">En frågesats.</span><span class="sxs-lookup"><span data-stu-id="8052c-239">A query statement.</span></span> |<span data-ttu-id="8052c-240">Nej</span><span class="sxs-lookup"><span data-stu-id="8052c-240">No</span></span> |
| <span data-ttu-id="8052c-241">sliceIdentifierColumnName</span><span class="sxs-lookup"><span data-stu-id="8052c-241">sliceIdentifierColumnName</span></span> |<span data-ttu-id="8052c-242">Ange kolumnnamnet för Kopieringsaktiviteten med genereras automatiskt segment-ID, som används för att rensa data för ett visst segment när köras på nytt.</span><span class="sxs-lookup"><span data-stu-id="8052c-242">Specify column name for Copy Activity to fill with auto generated slice identifier, which is used to clean up data of a specific slice when rerun.</span></span> <span data-ttu-id="8052c-243">Mer information finns i [repeterbara kopiera](#repeatable-copy) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="8052c-243">For more information, see [repeatable copy](#repeatable-copy) section.</span></span> |<span data-ttu-id="8052c-244">Kolumnnamnet för en kolumn med datatypen för binary(32).</span><span class="sxs-lookup"><span data-stu-id="8052c-244">Column name of a column with data type of binary(32).</span></span> |<span data-ttu-id="8052c-245">Nej</span><span class="sxs-lookup"><span data-stu-id="8052c-245">No</span></span> |
| <span data-ttu-id="8052c-246">sqlWriterStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="8052c-246">sqlWriterStoredProcedureName</span></span> |<span data-ttu-id="8052c-247">Namnet på den lagrade proceduren upserts (uppdateringar/infogar) data i måltabellen.</span><span class="sxs-lookup"><span data-stu-id="8052c-247">Name of the stored procedure that upserts (updates/inserts) data into the target table.</span></span> |<span data-ttu-id="8052c-248">Namnet på den lagrade proceduren.</span><span class="sxs-lookup"><span data-stu-id="8052c-248">Name of the stored procedure.</span></span> |<span data-ttu-id="8052c-249">Nej</span><span class="sxs-lookup"><span data-stu-id="8052c-249">No</span></span> |
| <span data-ttu-id="8052c-250">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="8052c-250">storedProcedureParameters</span></span> |<span data-ttu-id="8052c-251">Parametrar för den lagrade proceduren.</span><span class="sxs-lookup"><span data-stu-id="8052c-251">Parameters for the stored procedure.</span></span> |<span data-ttu-id="8052c-252">Namn/värde-par.</span><span class="sxs-lookup"><span data-stu-id="8052c-252">Name/value pairs.</span></span> <span data-ttu-id="8052c-253">Namn och skiftläge parametrar måste matcha namn och versaler och gemener i parametrarna för lagrade procedurer.</span><span class="sxs-lookup"><span data-stu-id="8052c-253">Names and casing of parameters must match the names and casing of the stored procedure parameters.</span></span> |<span data-ttu-id="8052c-254">Nej</span><span class="sxs-lookup"><span data-stu-id="8052c-254">No</span></span> |
| <span data-ttu-id="8052c-255">sqlWriterTableType</span><span class="sxs-lookup"><span data-stu-id="8052c-255">sqlWriterTableType</span></span> |<span data-ttu-id="8052c-256">Ange tabellen typnamn som ska användas i den lagrade proceduren.</span><span class="sxs-lookup"><span data-stu-id="8052c-256">Specify table type name to be used in the stored procedure.</span></span> <span data-ttu-id="8052c-257">Kopieringsaktiviteten tillhandahåller data flyttas i en temporär tabell med den här tabellen.</span><span class="sxs-lookup"><span data-stu-id="8052c-257">Copy activity makes the data being moved available in a temp table with this table type.</span></span> <span data-ttu-id="8052c-258">Lagrade procedurer kan sedan koppla data kopieras med befintliga data.</span><span class="sxs-lookup"><span data-stu-id="8052c-258">Stored procedure code can then merge the data being copied with existing data.</span></span> |<span data-ttu-id="8052c-259">Ett namn för tabellen.</span><span class="sxs-lookup"><span data-stu-id="8052c-259">A table type name.</span></span> |<span data-ttu-id="8052c-260">Nej</span><span class="sxs-lookup"><span data-stu-id="8052c-260">No</span></span> |


## <a name="json-examples-for-copying-data-from-and-to-sql-server"></a><span data-ttu-id="8052c-261">JSON-exempel för att kopiera data från och till SQL Server</span><span class="sxs-lookup"><span data-stu-id="8052c-261">JSON examples for copying data from and to SQL Server</span></span>
<span data-ttu-id="8052c-262">Följande exempel ger exempel JSON definitioner som du kan använda för att skapa en pipeline med hjälp av [Azure-portalen](data-factory-copy-activity-tutorial-using-azure-portal.md) eller [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) eller [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="8052c-262">The following examples provide sample JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="8052c-263">Följande exempel visar hur du kopierar data till och från SQL Server och Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="8052c-263">The following samples show how to copy data to and from SQL Server and Azure Blob Storage.</span></span> <span data-ttu-id="8052c-264">Dock datan kan kopieras **direkt** från alla källor till någon av sänkor anges [här](data-factory-data-movement-activities.md#supported-data-stores-and-formats) med hjälp av aktiviteten kopiera i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="8052c-264">However, data can be copied **directly** from any of sources to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>     

## <a name="example-copy-data-from-sql-server-to-azure-blob"></a><span data-ttu-id="8052c-265">Exempel: Kopiera data från SQL Server till Azure-Blob</span><span class="sxs-lookup"><span data-stu-id="8052c-265">Example: Copy data from SQL Server to Azure Blob</span></span>
<span data-ttu-id="8052c-266">I följande exempel visas:</span><span class="sxs-lookup"><span data-stu-id="8052c-266">The following sample shows:</span></span>

1. <span data-ttu-id="8052c-267">En länkad tjänst av typen [OnPremisesSqlServer](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="8052c-267">A linked service of type [OnPremisesSqlServer](#linked-service-properties).</span></span>
2. <span data-ttu-id="8052c-268">En länkad tjänst av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="8052c-268">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="8052c-269">Indata [dataset](data-factory-create-datasets.md) av typen [SqlServerTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="8052c-269">An input [dataset](data-factory-create-datasets.md) of type [SqlServerTable](#dataset-properties).</span></span>
4. <span data-ttu-id="8052c-270">Utdata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="8052c-270">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="8052c-271">Den [pipeline](data-factory-create-pipelines.md) med kopieringsaktiviteten som använder [SqlSource](#copy-activity-properties) och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="8052c-271">The [pipeline](data-factory-create-pipelines.md) with Copy activity that uses [SqlSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="8052c-272">Exemplet kopierar time series-data från en tabell i SQL Server till en Azure blob varje timme.</span><span class="sxs-lookup"><span data-stu-id="8052c-272">The sample copies time-series data from a SQL Server table to an Azure blob every hour.</span></span> <span data-ttu-id="8052c-273">JSON-egenskaper som används i exemplen beskrivs i exemplen i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="8052c-273">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="8052c-274">Som ett första steg bör du konfigurera data management gateway.</span><span class="sxs-lookup"><span data-stu-id="8052c-274">As a first step, setup the data management gateway.</span></span> <span data-ttu-id="8052c-275">Anvisningarna är i den [flytta data mellan lokala platser och moln](data-factory-move-data-between-onprem-and-cloud.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="8052c-275">The instructions are in the [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="8052c-276">**SQL Server som är länkad tjänst**</span><span class="sxs-lookup"><span data-stu-id="8052c-276">**SQL Server linked service**</span></span>
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
<span data-ttu-id="8052c-277">**Azure Blob länkade lagringstjänsten**</span><span class="sxs-lookup"><span data-stu-id="8052c-277">**Azure Blob storage linked service**</span></span>

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
<span data-ttu-id="8052c-278">**SQL Server inkommande dataset**</span><span class="sxs-lookup"><span data-stu-id="8052c-278">**SQL Server input dataset**</span></span>

<span data-ttu-id="8052c-279">Exemplet förutsätter att du har skapat en tabell ”mytable” som prefix i SQL Server och den innehåller en kolumn med namnet ”timestampcolumn” för tid series-data.</span><span class="sxs-lookup"><span data-stu-id="8052c-279">The sample assumes you have created a table “MyTable” in SQL Server and it contains a column called “timestampcolumn” for time series data.</span></span> <span data-ttu-id="8052c-280">Du kan fråga över flera tabeller i samma databas med hjälp av en enda dataset, men en enskild tabell måste användas för att datauppsättningen tableName typeProperty.</span><span class="sxs-lookup"><span data-stu-id="8052c-280">You can query over multiple tables within the same database using a single dataset, but a single table must be used for the dataset's tableName typeProperty.</span></span>

<span data-ttu-id="8052c-281">Inställningen ”externa”: ”true” informerar Data Factory-tjänsten att datamängden är extern till data factory och inte tillverkas av en aktivitet i datafabriken.</span><span class="sxs-lookup"><span data-stu-id="8052c-281">Setting “external”: ”true” informs Data Factory service that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span>

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
<span data-ttu-id="8052c-282">**Azure Blob utdatauppsättningen**</span><span class="sxs-lookup"><span data-stu-id="8052c-282">**Azure Blob output dataset**</span></span>

<span data-ttu-id="8052c-283">Data skrivs till en ny blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="8052c-283">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="8052c-284">Sökvägen till mappen för blobben utvärderas dynamiskt baserat på starttiden för den sektor som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="8052c-284">The folder path for the blob is dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="8052c-285">Mappsökvägen använder år, månad, dag och timmar delar av starttiden.</span><span class="sxs-lookup"><span data-stu-id="8052c-285">The folder path uses year, month, day, and hours parts of the start time.</span></span>

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
<span data-ttu-id="8052c-286">**Pipeline med kopieringsaktiviteten**</span><span class="sxs-lookup"><span data-stu-id="8052c-286">**Pipeline with Copy activity**</span></span>

<span data-ttu-id="8052c-287">Pipelinen innehåller en kopia-aktivitet som är konfigurerad för att använda dessa indata och utdata-datauppsättningar och är schemalagd att köras varje timme.</span><span class="sxs-lookup"><span data-stu-id="8052c-287">The pipeline contains a Copy Activity that is configured to use these input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="8052c-288">I pipeline-JSON-definitionen av **källa** är inställd på **SqlSource** och **sink** är inställd på **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="8052c-288">In the pipeline JSON definition, the **source** type is set to **SqlSource** and **sink** type is set to **BlobSink**.</span></span> <span data-ttu-id="8052c-289">SQL-frågan som angetts för den **SqlReaderQuery** egenskapen väljer vilka data under den senaste timmen att kopiera.</span><span class="sxs-lookup"><span data-stu-id="8052c-289">The SQL query specified for the **SqlReaderQuery** property selects the data in the past hour to copy.</span></span>

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
<span data-ttu-id="8052c-290">I det här exemplet **sqlReaderQuery** har angetts för SqlSource.</span><span class="sxs-lookup"><span data-stu-id="8052c-290">In this example, **sqlReaderQuery** is specified for the SqlSource.</span></span> <span data-ttu-id="8052c-291">Kopieringsaktiviteten körs den här frågan mot SQL Server-databas källan för att hämta data.</span><span class="sxs-lookup"><span data-stu-id="8052c-291">The Copy Activity runs this query against the SQL Server Database source to get the data.</span></span> <span data-ttu-id="8052c-292">Du kan också ange en lagrad procedur genom att ange den **sqlReaderStoredProcedureName** och **storedProcedureParameters** (om den lagrade proceduren har parametrar).</span><span class="sxs-lookup"><span data-stu-id="8052c-292">Alternatively, you can specify a stored procedure by specifying the **sqlReaderStoredProcedureName** and **storedProcedureParameters** (if the stored procedure takes parameters).</span></span> <span data-ttu-id="8052c-293">SqlReaderQuery kan referera till flera tabeller i databasen som refereras av inkommande dataset.</span><span class="sxs-lookup"><span data-stu-id="8052c-293">The sqlReaderQuery can reference multiple tables within the database referenced by the input dataset.</span></span> <span data-ttu-id="8052c-294">Det är inte begränsad till endast tabellen som den dataset tableName typeProperty.</span><span class="sxs-lookup"><span data-stu-id="8052c-294">It is not limited to only the table set as the dataset's tableName typeProperty.</span></span>

<span data-ttu-id="8052c-295">Om du inte anger sqlReaderQuery eller sqlReaderStoredProcedureName, används de kolumner som anges i strukturen för att skapa en select-frågan ska köras mot SQL Server-databasen.</span><span class="sxs-lookup"><span data-stu-id="8052c-295">If you do not specify sqlReaderQuery or sqlReaderStoredProcedureName, the columns defined in the structure section are used to build a select query to run against the SQL Server Database.</span></span> <span data-ttu-id="8052c-296">Om datauppsättningsdefinitionen saknar strukturen, markeras alla kolumner från tabellen.</span><span class="sxs-lookup"><span data-stu-id="8052c-296">If the dataset definition does not have the structure, all columns are selected from the table.</span></span>

<span data-ttu-id="8052c-297">Finns det [Sql-källans](#sqlsource) avsnitt och [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) lista över egenskaper som stöds av SqlSource och BlobSink.</span><span class="sxs-lookup"><span data-stu-id="8052c-297">See the [Sql Source](#sqlsource) section and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) for the list of properties supported by SqlSource and BlobSink.</span></span>

## <a name="example-copy-data-from-azure-blob-to-sql-server"></a><span data-ttu-id="8052c-298">Exempel: Kopiera data från Azure Blob till SQL Server</span><span class="sxs-lookup"><span data-stu-id="8052c-298">Example: Copy data from Azure Blob to SQL Server</span></span>
<span data-ttu-id="8052c-299">I följande exempel visas:</span><span class="sxs-lookup"><span data-stu-id="8052c-299">The following sample shows:</span></span>

1. <span data-ttu-id="8052c-300">Den länkade tjänsten av typen [OnPremisesSqlServer](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="8052c-300">The linked service of type [OnPremisesSqlServer](#linked-service-properties).</span></span>
2. <span data-ttu-id="8052c-301">Den länkade tjänsten av typen [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="8052c-301">The linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="8052c-302">Indata [dataset](data-factory-create-datasets.md) av typen [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="8052c-302">An input [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="8052c-303">Utdata [dataset](data-factory-create-datasets.md) av typen [SqlServerTable](data-factory-sqlserver-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="8052c-303">An output [dataset](data-factory-create-datasets.md) of type [SqlServerTable](data-factory-sqlserver-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="8052c-304">Den [pipeline](data-factory-create-pipelines.md) med kopieringsaktiviteten som använder [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) och [SqlSink](#sql-server-copy-activity-type-properties).</span><span class="sxs-lookup"><span data-stu-id="8052c-304">The [pipeline](data-factory-create-pipelines.md) with Copy activity that uses [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) and [SqlSink](#sql-server-copy-activity-type-properties).</span></span>

<span data-ttu-id="8052c-305">Exempel kopiorna tidsserier data från ett Azure blob till en SQL Server tabellen varje timme.</span><span class="sxs-lookup"><span data-stu-id="8052c-305">The sample copies time-series data from an Azure blob to a SQL Server table every hour.</span></span> <span data-ttu-id="8052c-306">JSON-egenskaper som används i exemplen beskrivs i exemplen i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="8052c-306">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="8052c-307">**SQL Server som är länkad tjänst**</span><span class="sxs-lookup"><span data-stu-id="8052c-307">**SQL Server linked service**</span></span>

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
<span data-ttu-id="8052c-308">**Azure Blob länkade lagringstjänsten**</span><span class="sxs-lookup"><span data-stu-id="8052c-308">**Azure Blob storage linked service**</span></span>

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
<span data-ttu-id="8052c-309">**Azure Blob-inkommande datamängd**</span><span class="sxs-lookup"><span data-stu-id="8052c-309">**Azure Blob input dataset**</span></span>

<span data-ttu-id="8052c-310">Data hämtas från en ny blob varje timme (frekvens: timme, intervall: 1).</span><span class="sxs-lookup"><span data-stu-id="8052c-310">Data is picked up from a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="8052c-311">Mappen sökvägen och filnamnet för blobben utvärderas dynamiskt baserat på starttiden för den sektor som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="8052c-311">The folder path and file name for the blob are dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="8052c-312">Mappsökvägen använder år, månad och som en dagdel av starttiden filnamn används i timmen som en del av starttiden.</span><span class="sxs-lookup"><span data-stu-id="8052c-312">The folder path uses year, month, and day part of the start time and file name uses the hour part of the start time.</span></span> <span data-ttu-id="8052c-313">”externa”: ”true” inställningen informerar Data Factory-tjänsten att datamängden är extern till data factory och inte tillverkas av en aktivitet i datafabriken.</span><span class="sxs-lookup"><span data-stu-id="8052c-313">“external”: “true” setting informs the Data Factory service that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span>

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
<span data-ttu-id="8052c-314">**SQL Server-utdatauppsättningen**</span><span class="sxs-lookup"><span data-stu-id="8052c-314">**SQL Server output dataset**</span></span>

<span data-ttu-id="8052c-315">Exemplet kopierar data till en tabell med namnet ”mytable” som prefix i SQL Server.</span><span class="sxs-lookup"><span data-stu-id="8052c-315">The sample copies data to a table named “MyTable” in SQL Server.</span></span> <span data-ttu-id="8052c-316">Skapa tabellen i SQL Server med samma antal kolumner som du förväntar dig Blob CSV-fil som innehåller.</span><span class="sxs-lookup"><span data-stu-id="8052c-316">Create the table in SQL Server with the same number of columns as you expect the Blob CSV file to contain.</span></span> <span data-ttu-id="8052c-317">Nya rader läggs till i tabell varje timme.</span><span class="sxs-lookup"><span data-stu-id="8052c-317">New rows are added to the table every hour.</span></span>

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
<span data-ttu-id="8052c-318">**Pipeline med kopieringsaktiviteten**</span><span class="sxs-lookup"><span data-stu-id="8052c-318">**Pipeline with Copy activity**</span></span>

<span data-ttu-id="8052c-319">Pipelinen innehåller en kopia-aktivitet som är konfigurerad för att använda dessa indata och utdata-datauppsättningar och är schemalagd att köras varje timme.</span><span class="sxs-lookup"><span data-stu-id="8052c-319">The pipeline contains a Copy Activity that is configured to use these input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="8052c-320">I pipeline-JSON-definitionen av **källa** är inställd på **BlobSource** och **sink** är inställd på **SqlSink**.</span><span class="sxs-lookup"><span data-stu-id="8052c-320">In the pipeline JSON definition, the **source** type is set to **BlobSource** and **sink** type is set to **SqlSink**.</span></span>

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

## <a name="troubleshooting-connection-issues"></a><span data-ttu-id="8052c-321">Felsökning av anslutningsproblem med</span><span class="sxs-lookup"><span data-stu-id="8052c-321">Troubleshooting connection issues</span></span>
1. <span data-ttu-id="8052c-322">Konfigurera SQL Server för att acceptera fjärranslutningar.</span><span class="sxs-lookup"><span data-stu-id="8052c-322">Configure your SQL Server to accept remote connections.</span></span> <span data-ttu-id="8052c-323">Starta **SQL Server Management Studio**, högerklicka på **server**, och klicka på **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="8052c-323">Launch **SQL Server Management Studio**, right-click **server**, and click **Properties**.</span></span> <span data-ttu-id="8052c-324">Välj **anslutningar** i listan och kontrollera **tillåta fjärranslutningar till servern**.</span><span class="sxs-lookup"><span data-stu-id="8052c-324">Select **Connections** from the list and check **Allow remote connections to the server**.</span></span>

    ![Aktivera fjärranslutningar](./media/data-factory-sqlserver-connector/AllowRemoteConnections.png)

    <span data-ttu-id="8052c-326">Se [konfigurerar fjärråtkomst Server Configuration Option](https://msdn.microsoft.com/library/ms191464.aspx) detaljerade anvisningar.</span><span class="sxs-lookup"><span data-stu-id="8052c-326">See [Configure the remote access Server Configuration Option](https://msdn.microsoft.com/library/ms191464.aspx) for detailed steps.</span></span>
2. <span data-ttu-id="8052c-327">Starta **SQL Server Configuration Manager**.</span><span class="sxs-lookup"><span data-stu-id="8052c-327">Launch **SQL Server Configuration Manager**.</span></span> <span data-ttu-id="8052c-328">Expandera **SQL Server-nätverkskonfigurationen** för instansen och väljer **protokoll för MSSQLSERVER**.</span><span class="sxs-lookup"><span data-stu-id="8052c-328">Expand **SQL Server Network Configuration** for the instance you want, and select **Protocols for MSSQLSERVER**.</span></span> <span data-ttu-id="8052c-329">Du bör se protokoll i den högra rutan.</span><span class="sxs-lookup"><span data-stu-id="8052c-329">You should see protocols in the right-pane.</span></span> <span data-ttu-id="8052c-330">Aktivera TCP/IP genom att högerklicka på **TCP/IP** och klicka på **aktivera**.</span><span class="sxs-lookup"><span data-stu-id="8052c-330">Enable TCP/IP by right-clicking **TCP/IP** and clicking **Enable**.</span></span>

    ![Aktivera TCP/IP](./media/data-factory-sqlserver-connector/EnableTCPProptocol.png)

    <span data-ttu-id="8052c-332">Se [aktivera eller inaktivera nätverksprotokoll Server](https://msdn.microsoft.com/library/ms191294.aspx) information och alternativa metoder för att aktivera TCP/IP-protokollet.</span><span class="sxs-lookup"><span data-stu-id="8052c-332">See [Enable or Disable a Server Network Protocol](https://msdn.microsoft.com/library/ms191294.aspx) for details and alternate ways of enabling TCP/IP protocol.</span></span>
3. <span data-ttu-id="8052c-333">Dubbelklicka i samma fönster **TCP/IP** att starta **TCP/IP-egenskaper** fönster.</span><span class="sxs-lookup"><span data-stu-id="8052c-333">In the same window, double-click **TCP/IP** to launch **TCP/IP Properties** window.</span></span>
4. <span data-ttu-id="8052c-334">Växla till den **IP-adresser** fliken.</span><span class="sxs-lookup"><span data-stu-id="8052c-334">Switch to the **IP Addresses** tab.</span></span> <span data-ttu-id="8052c-335">Rulla Se **IPAll** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="8052c-335">Scroll down to see **IPAll** section.</span></span> <span data-ttu-id="8052c-336">Notera den ** TCP-Port ** (standard är **1433**).</span><span class="sxs-lookup"><span data-stu-id="8052c-336">Note down the **TCP Port **(default is **1433**).</span></span>
5. <span data-ttu-id="8052c-337">Skapa en **regeln för Windows-brandväggen** på datorn för att tillåta inkommande trafik via den här porten.</span><span class="sxs-lookup"><span data-stu-id="8052c-337">Create a **rule for the Windows Firewall** on the machine to allow incoming traffic through this port.</span></span>  
6. <span data-ttu-id="8052c-338">**Verifiera anslutning**: använda SQL Server Management Studio för att ansluta till SQL Server med hjälp av fullständigt kvalificerat namn, från en annan dator.</span><span class="sxs-lookup"><span data-stu-id="8052c-338">**Verify connection**: To connect to the SQL Server using fully qualified name, use SQL Server Management Studio from a different machine.</span></span> <span data-ttu-id="8052c-339">Till exempel ”:<machine>.<domain>. Corp.<company>.com, 1433 ”.</span><span class="sxs-lookup"><span data-stu-id="8052c-339">For example: "<machine>.<domain>.corp.<company>.com,1433."</span></span>

   > [!IMPORTANT]

   > <span data-ttu-id="8052c-340">Se [flytta data mellan lokala källor och molnet med Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md) detaljerad information.</span><span class="sxs-lookup"><span data-stu-id="8052c-340">See [Move data between on-premises sources and the cloud with Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md) for detailed information.</span></span>
   >
   > <span data-ttu-id="8052c-341">Se [felsökning av problem med gateway](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) tips om hur du felsöker anslutning /-gateway relaterade problem.</span><span class="sxs-lookup"><span data-stu-id="8052c-341">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>
   >
   >


## <a name="identity-columns-in-the-target-database"></a><span data-ttu-id="8052c-342">Identitetskolumner i måldatabasen</span><span class="sxs-lookup"><span data-stu-id="8052c-342">Identity columns in the target database</span></span>
<span data-ttu-id="8052c-343">Det här avsnittet innehåller ett exempel som kopierar data från en källtabell med någon identity-kolumn till en måltabell med en identitetskolumn.</span><span class="sxs-lookup"><span data-stu-id="8052c-343">This section provides an example that copies data from a source table with no identity column to a destination table with an identity column.</span></span>

<span data-ttu-id="8052c-344">**Källtabellen:**</span><span class="sxs-lookup"><span data-stu-id="8052c-344">**Source table:**</span></span>

```sql
create table dbo.SourceTbl
(
       name varchar(100),
       age int
)
```
<span data-ttu-id="8052c-345">**Måltabellen:**</span><span class="sxs-lookup"><span data-stu-id="8052c-345">**Destination table:**</span></span>

```sql
create table dbo.TargetTbl
(
       identifier int identity(1,1),
       name varchar(100),
       age int
)
```

<span data-ttu-id="8052c-346">Observera att måltabellen har en identitetskolumn.</span><span class="sxs-lookup"><span data-stu-id="8052c-346">Notice that the target table has an identity column.</span></span>

<span data-ttu-id="8052c-347">**Källa JSON-definitionen för datauppsättning**</span><span class="sxs-lookup"><span data-stu-id="8052c-347">**Source dataset JSON definition**</span></span>

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
<span data-ttu-id="8052c-348">**Mål JSON-definitionen för datauppsättning**</span><span class="sxs-lookup"><span data-stu-id="8052c-348">**Destination dataset JSON definition**</span></span>

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

<span data-ttu-id="8052c-349">Observera att som käll- och tabellen har olika schema (målet har en ytterligare kolumn med identity).</span><span class="sxs-lookup"><span data-stu-id="8052c-349">Notice that as your source and target table have different schema (target has an additional column with identity).</span></span> <span data-ttu-id="8052c-350">I det här scenariot måste du ange **struktur** egenskap i datauppsättningsdefinitionen mål som inte innehåller en identity-kolumn.</span><span class="sxs-lookup"><span data-stu-id="8052c-350">In this scenario, you need to specify **structure** property in the target dataset definition, which doesn’t include the identity column.</span></span>

## <a name="invoke-stored-procedure-from-sql-sink"></a><span data-ttu-id="8052c-351">Anropa lagrade procedur från SQL-mottagare</span><span class="sxs-lookup"><span data-stu-id="8052c-351">Invoke stored procedure from SQL sink</span></span>
<span data-ttu-id="8052c-352">Se [anropa lagrad procedur för SQL-mottagare i en Kopieringsaktivitet](data-factory-invoke-stored-procedure-from-copy-activity.md) artikel ett exempel på att anropa en lagrad procedur från SQL-mottagare i en Kopieringsaktivitet för en pipeline.</span><span class="sxs-lookup"><span data-stu-id="8052c-352">See [Invoke stored procedure for SQL sink in copy activity](data-factory-invoke-stored-procedure-from-copy-activity.md) article for an example of invoking a stored procedure from SQL sink in a copy activity of a pipeline.</span></span>

## <a name="type-mapping-for-sql-server"></a><span data-ttu-id="8052c-353">Mappning för SQLServer</span><span class="sxs-lookup"><span data-stu-id="8052c-353">Type mapping for SQL server</span></span>
<span data-ttu-id="8052c-354">Som anges i den [data movement aktiviteter](data-factory-data-movement-activities.md) artikel, kopieringsaktiviteten utför automatisk konverteringar från källtyper att registrera typer med följande metod i steg 2:</span><span class="sxs-lookup"><span data-stu-id="8052c-354">As mentioned in the [data movement activities](data-factory-data-movement-activities.md) article, the Copy activity performs automatic type conversions from source types to sink types with the following 2-step approach:</span></span>

1. <span data-ttu-id="8052c-355">Konvertera från interna källtyper till .NET-typ</span><span class="sxs-lookup"><span data-stu-id="8052c-355">Convert from native source types to .NET type</span></span>
2. <span data-ttu-id="8052c-356">Konvertera från .NET-typ till interna mottagare typ.</span><span class="sxs-lookup"><span data-stu-id="8052c-356">Convert from .NET type to native sink type</span></span>

<span data-ttu-id="8052c-357">När data flyttas till och från SQLServer, används följande mappning från SQL-typen till .NET-typ och vice versa.</span><span class="sxs-lookup"><span data-stu-id="8052c-357">When moving data to & from SQL server, the following mappings are used from SQL type to .NET type and vice versa.</span></span>

<span data-ttu-id="8052c-358">Mappningen är densamma som SQL-Server-Datatypsmappningen för ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="8052c-358">The mapping is same as the SQL Server Data Type Mapping for ADO.NET.</span></span>

| <span data-ttu-id="8052c-359">SQL Server Database Engine-typ</span><span class="sxs-lookup"><span data-stu-id="8052c-359">SQL Server Database Engine type</span></span> | <span data-ttu-id="8052c-360">.NET framework-typ</span><span class="sxs-lookup"><span data-stu-id="8052c-360">.NET Framework type</span></span> |
| --- | --- |
| <span data-ttu-id="8052c-361">bigint</span><span class="sxs-lookup"><span data-stu-id="8052c-361">bigint</span></span> |<span data-ttu-id="8052c-362">Int64</span><span class="sxs-lookup"><span data-stu-id="8052c-362">Int64</span></span> |
| <span data-ttu-id="8052c-363">Binär</span><span class="sxs-lookup"><span data-stu-id="8052c-363">binary</span></span> |<span data-ttu-id="8052c-364">byte]</span><span class="sxs-lookup"><span data-stu-id="8052c-364">Byte[]</span></span> |
| <span data-ttu-id="8052c-365">bitar</span><span class="sxs-lookup"><span data-stu-id="8052c-365">bit</span></span> |<span data-ttu-id="8052c-366">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="8052c-366">Boolean</span></span> |
| <span data-ttu-id="8052c-367">Char</span><span class="sxs-lookup"><span data-stu-id="8052c-367">char</span></span> |<span data-ttu-id="8052c-368">Sträng, Char]</span><span class="sxs-lookup"><span data-stu-id="8052c-368">String, Char[]</span></span> |
| <span data-ttu-id="8052c-369">Datum</span><span class="sxs-lookup"><span data-stu-id="8052c-369">date</span></span> |<span data-ttu-id="8052c-370">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="8052c-370">DateTime</span></span> |
| <span data-ttu-id="8052c-371">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="8052c-371">Datetime</span></span> |<span data-ttu-id="8052c-372">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="8052c-372">DateTime</span></span> |
| <span data-ttu-id="8052c-373">datetime2</span><span class="sxs-lookup"><span data-stu-id="8052c-373">datetime2</span></span> |<span data-ttu-id="8052c-374">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="8052c-374">DateTime</span></span> |
| <span data-ttu-id="8052c-375">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="8052c-375">Datetimeoffset</span></span> |<span data-ttu-id="8052c-376">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="8052c-376">DateTimeOffset</span></span> |
| <span data-ttu-id="8052c-377">Decimal</span><span class="sxs-lookup"><span data-stu-id="8052c-377">Decimal</span></span> |<span data-ttu-id="8052c-378">Decimal</span><span class="sxs-lookup"><span data-stu-id="8052c-378">Decimal</span></span> |
| <span data-ttu-id="8052c-379">FILESTREAM-attributet (varbinary(max))</span><span class="sxs-lookup"><span data-stu-id="8052c-379">FILESTREAM attribute (varbinary(max))</span></span> |<span data-ttu-id="8052c-380">byte]</span><span class="sxs-lookup"><span data-stu-id="8052c-380">Byte[]</span></span> |
| <span data-ttu-id="8052c-381">flyttal</span><span class="sxs-lookup"><span data-stu-id="8052c-381">Float</span></span> |<span data-ttu-id="8052c-382">dubbla</span><span class="sxs-lookup"><span data-stu-id="8052c-382">Double</span></span> |
| <span data-ttu-id="8052c-383">Bild</span><span class="sxs-lookup"><span data-stu-id="8052c-383">image</span></span> |<span data-ttu-id="8052c-384">byte]</span><span class="sxs-lookup"><span data-stu-id="8052c-384">Byte[]</span></span> |
| <span data-ttu-id="8052c-385">int</span><span class="sxs-lookup"><span data-stu-id="8052c-385">int</span></span> |<span data-ttu-id="8052c-386">Int32</span><span class="sxs-lookup"><span data-stu-id="8052c-386">Int32</span></span> |
| <span data-ttu-id="8052c-387">Money</span><span class="sxs-lookup"><span data-stu-id="8052c-387">money</span></span> |<span data-ttu-id="8052c-388">Decimal</span><span class="sxs-lookup"><span data-stu-id="8052c-388">Decimal</span></span> |
| <span data-ttu-id="8052c-389">nchar</span><span class="sxs-lookup"><span data-stu-id="8052c-389">nchar</span></span> |<span data-ttu-id="8052c-390">Sträng, Char]</span><span class="sxs-lookup"><span data-stu-id="8052c-390">String, Char[]</span></span> |
| <span data-ttu-id="8052c-391">ntext</span><span class="sxs-lookup"><span data-stu-id="8052c-391">ntext</span></span> |<span data-ttu-id="8052c-392">Sträng, Char]</span><span class="sxs-lookup"><span data-stu-id="8052c-392">String, Char[]</span></span> |
| <span data-ttu-id="8052c-393">numeriskt</span><span class="sxs-lookup"><span data-stu-id="8052c-393">numeric</span></span> |<span data-ttu-id="8052c-394">Decimal</span><span class="sxs-lookup"><span data-stu-id="8052c-394">Decimal</span></span> |
| <span data-ttu-id="8052c-395">nvarchar</span><span class="sxs-lookup"><span data-stu-id="8052c-395">nvarchar</span></span> |<span data-ttu-id="8052c-396">Sträng, Char]</span><span class="sxs-lookup"><span data-stu-id="8052c-396">String, Char[]</span></span> |
| <span data-ttu-id="8052c-397">Verklig</span><span class="sxs-lookup"><span data-stu-id="8052c-397">real</span></span> |<span data-ttu-id="8052c-398">Enskild</span><span class="sxs-lookup"><span data-stu-id="8052c-398">Single</span></span> |
| <span data-ttu-id="8052c-399">ROWVERSION</span><span class="sxs-lookup"><span data-stu-id="8052c-399">rowversion</span></span> |<span data-ttu-id="8052c-400">byte]</span><span class="sxs-lookup"><span data-stu-id="8052c-400">Byte[]</span></span> |
| <span data-ttu-id="8052c-401">smalldatetime</span><span class="sxs-lookup"><span data-stu-id="8052c-401">smalldatetime</span></span> |<span data-ttu-id="8052c-402">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="8052c-402">DateTime</span></span> |
| <span data-ttu-id="8052c-403">smallint</span><span class="sxs-lookup"><span data-stu-id="8052c-403">smallint</span></span> |<span data-ttu-id="8052c-404">Int16</span><span class="sxs-lookup"><span data-stu-id="8052c-404">Int16</span></span> |
| <span data-ttu-id="8052c-405">smallmoney</span><span class="sxs-lookup"><span data-stu-id="8052c-405">smallmoney</span></span> |<span data-ttu-id="8052c-406">Decimal</span><span class="sxs-lookup"><span data-stu-id="8052c-406">Decimal</span></span> |
| <span data-ttu-id="8052c-407">sql_variant</span><span class="sxs-lookup"><span data-stu-id="8052c-407">sql_variant</span></span> |<span data-ttu-id="8052c-408">Objektet *</span><span class="sxs-lookup"><span data-stu-id="8052c-408">Object *</span></span> |
| <span data-ttu-id="8052c-409">Text</span><span class="sxs-lookup"><span data-stu-id="8052c-409">text</span></span> |<span data-ttu-id="8052c-410">Sträng, Char]</span><span class="sxs-lookup"><span data-stu-id="8052c-410">String, Char[]</span></span> |
| <span data-ttu-id="8052c-411">time</span><span class="sxs-lookup"><span data-stu-id="8052c-411">time</span></span> |<span data-ttu-id="8052c-412">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="8052c-412">TimeSpan</span></span> |
| <span data-ttu-id="8052c-413">tidsstämpel</span><span class="sxs-lookup"><span data-stu-id="8052c-413">timestamp</span></span> |<span data-ttu-id="8052c-414">byte]</span><span class="sxs-lookup"><span data-stu-id="8052c-414">Byte[]</span></span> |
| <span data-ttu-id="8052c-415">tinyint</span><span class="sxs-lookup"><span data-stu-id="8052c-415">tinyint</span></span> |<span data-ttu-id="8052c-416">Mottagna byte</span><span class="sxs-lookup"><span data-stu-id="8052c-416">Byte</span></span> |
| <span data-ttu-id="8052c-417">Unik identifierare</span><span class="sxs-lookup"><span data-stu-id="8052c-417">uniqueidentifier</span></span> |<span data-ttu-id="8052c-418">GUID</span><span class="sxs-lookup"><span data-stu-id="8052c-418">Guid</span></span> |
| <span data-ttu-id="8052c-419">varbinary</span><span class="sxs-lookup"><span data-stu-id="8052c-419">varbinary</span></span> |<span data-ttu-id="8052c-420">byte]</span><span class="sxs-lookup"><span data-stu-id="8052c-420">Byte[]</span></span> |
| <span data-ttu-id="8052c-421">varchar</span><span class="sxs-lookup"><span data-stu-id="8052c-421">varchar</span></span> |<span data-ttu-id="8052c-422">Sträng, Char]</span><span class="sxs-lookup"><span data-stu-id="8052c-422">String, Char[]</span></span> |
| <span data-ttu-id="8052c-423">xml</span><span class="sxs-lookup"><span data-stu-id="8052c-423">xml</span></span> |<span data-ttu-id="8052c-424">XML</span><span class="sxs-lookup"><span data-stu-id="8052c-424">Xml</span></span> |

## <a name="mapping-source-to-sink-columns"></a><span data-ttu-id="8052c-425">Mappning källan till mottagare för kolumner</span><span class="sxs-lookup"><span data-stu-id="8052c-425">Mapping source to sink columns</span></span>
<span data-ttu-id="8052c-426">Om du vill mappa kolumner från källan dataset till kolumner från sink dataset finns [mappa dataset kolumner i Azure Data Factory](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="8052c-426">To map columns from source dataset to columns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-copy"></a><span data-ttu-id="8052c-427">Repeterbara kopia</span><span class="sxs-lookup"><span data-stu-id="8052c-427">Repeatable copy</span></span>
<span data-ttu-id="8052c-428">När du kopierar data till SQL Server-databas, lägger kopieringsaktiviteten data till tabellen mottagare som standard.</span><span class="sxs-lookup"><span data-stu-id="8052c-428">When copying data to SQL Server Database, the copy activity appends data to the sink table by default.</span></span> <span data-ttu-id="8052c-429">För att genomföra en UPSERT i stället Se [Repeatable skriva till SqlSink](data-factory-repeatable-copy.md#repeatable-write-to-sqlsink) artikel.</span><span class="sxs-lookup"><span data-stu-id="8052c-429">To perform an UPSERT instead,  See [Repeatable write to SqlSink](data-factory-repeatable-copy.md#repeatable-write-to-sqlsink) article.</span></span> 

<span data-ttu-id="8052c-430">Tänk på att undvika oväntade resultat repeterbarhet när kopiering av data från relationella data lagras.</span><span class="sxs-lookup"><span data-stu-id="8052c-430">When copying data from relational data stores, keep repeatability in mind to avoid unintended outcomes.</span></span> <span data-ttu-id="8052c-431">I Azure Data Factory, kan du köra en sektor manuellt.</span><span class="sxs-lookup"><span data-stu-id="8052c-431">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="8052c-432">Du kan också konfigurera i principen för en dataset så att ett segment som körs när ett fel uppstår.</span><span class="sxs-lookup"><span data-stu-id="8052c-432">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="8052c-433">När ett segment körs på något sätt, måste du kontrollera att samma data läses oavsett hur många gånger ett segment körs.</span><span class="sxs-lookup"><span data-stu-id="8052c-433">When a slice is rerun in either way, you need to make sure that the same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="8052c-434">Se [Repeatable läsa från relationella källor](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="8052c-434">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="8052c-435">Prestanda och finjustering</span><span class="sxs-lookup"><span data-stu-id="8052c-435">Performance and Tuning</span></span>
<span data-ttu-id="8052c-436">Se [kopiera aktivitet prestanda och justera guiden](data-factory-copy-activity-performance.md) vill veta mer om viktiga faktorer som påverkan prestanda för flytt av data (Kopieringsaktiviteten) i Azure Data Factory och olika sätt att optimera den.</span><span class="sxs-lookup"><span data-stu-id="8052c-436">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
