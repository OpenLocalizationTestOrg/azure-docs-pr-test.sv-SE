---
title: "Läs in data till Azure SQL Data Warehouse | Microsoft Docs"
description: "Läs om vanliga scenarier för datainläsning till SQL Data Warehouse. Dessa inkluderar med PolyBase, Azure blob storage, flat-filer och disk leverans. Du kan också använda verktyg från tredje part."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
ms.assetid: 2253bf46-cf72-4de7-85ce-f267494d55fa
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: c4199a387f5cdbd477a5e348e48ba8e8b5900075
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="load-data-into-azure-sql-data-warehouse"></a><span data-ttu-id="8cca4-105">Läs in data till Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="8cca4-105">Load data into Azure SQL Data Warehouse</span></span>
<span data-ttu-id="8cca4-106">En sammanfattning av scenariot alternativ och rekommendationer för att läsa in data i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="8cca4-106">A summary of the scenario options and recommendations for loading data into SQL Data Warehouse.</span></span>

<span data-ttu-id="8cca4-107">Den svåraste delen av läsning av data är vanligtvis förbereder data för den.</span><span class="sxs-lookup"><span data-stu-id="8cca4-107">The hardest part of loading data is usually preparing the data for the load.</span></span> <span data-ttu-id="8cca4-108">Azure förenklar inläsning genom att använda Azure blob-lagring som en gemensam datalager för många av tjänsterna och använda Azure Data Factory för att dirigera kommunikation och flytt mellan Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="8cca4-108">Azure simplifies loading by using Azure blob storage as a common data store for many of the services, and using Azure Data Factory to orchestrate communication and data movement between the Azure services.</span></span> <span data-ttu-id="8cca4-109">De här processerna är integrerade med PolyBase-teknik som använder massivt parallell bearbetning (MPP) för att läsa in data parallellt från Azure blob storage till SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="8cca4-109">These processes are integrated with PolyBase technology which uses massively parallel processing (MPP) to load data in parallel from Azure blob storage into SQL Data Warehouse.</span></span> 

<span data-ttu-id="8cca4-110">Självstudier som läser in exempeldatabaserna finns [ladda exempeldatabaserna][Load sample databases].</span><span class="sxs-lookup"><span data-stu-id="8cca4-110">For tutorials that load sample databases, see [Load sample databases][Load sample databases].</span></span>

## <a name="load-from-azure-blob-storage"></a><span data-ttu-id="8cca4-111">Läsa in från Azure blob storage</span><span class="sxs-lookup"><span data-stu-id="8cca4-111">Load from Azure blob storage</span></span>
<span data-ttu-id="8cca4-112">Det snabbaste sättet att importera data till SQL Data Warehouse är att använda PolyBase för att läsa in data från Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="8cca4-112">The fastest way to import data into SQL Data Warehouse is to use PolyBase to load data from Azure blob storage.</span></span> <span data-ttu-id="8cca4-113">PolyBase använder SQL Data Warehouse massivt parallell bearbetning (MPP) design för att läsa in data parallellt från Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="8cca4-113">PolyBase uses SQL Data Warehouse's massively parallel processing (MPP) design to load data in parallel from Azure blob storage.</span></span> <span data-ttu-id="8cca4-114">Du kan använda T-SQL-kommandon eller ett Azure Data Factory-pipelinen för att använda PolyBase.</span><span class="sxs-lookup"><span data-stu-id="8cca4-114">To use PolyBase, you can use T-SQL commands or an Azure Data Factory pipeline.</span></span>

### <a name="1-use-polybase-and-t-sql"></a><span data-ttu-id="8cca4-115">1. Använd PolyBase och T-SQL</span><span class="sxs-lookup"><span data-stu-id="8cca4-115">1. Use PolyBase and T-SQL</span></span>
<span data-ttu-id="8cca4-116">Översikt över processen för inläsning:</span><span class="sxs-lookup"><span data-stu-id="8cca4-116">Summary of loading process:</span></span>

1. <span data-ttu-id="8cca4-117">Flytta dina data till Azure blob storage eller Azure Data Lake Store och lagra den i textfiler.</span><span class="sxs-lookup"><span data-stu-id="8cca4-117">Move your data to Azure blob storage or Azure Data Lake Store and store it in text files.</span></span>
2. <span data-ttu-id="8cca4-118">Konfigurera externa objekt i SQL Data Warehouse definiera plats och formatet för data</span><span class="sxs-lookup"><span data-stu-id="8cca4-118">Configure external objects in SQL Data Warehouse to define the location and format of the data</span></span>
3. <span data-ttu-id="8cca4-119">Köra ett T-SQL-kommando för att läsa in data parallellt i en ny databastabell.</span><span class="sxs-lookup"><span data-stu-id="8cca4-119">Run a T-SQL command to load the data in parallel into a new database table.</span></span>

<!-- 5. Schedule and run a loading job. --> 

<span data-ttu-id="8cca4-120">En självstudiekurs finns [läsa in data från Azure blob storage till SQL Data Warehouse (PolyBase)][Load data from Azure blob storage to SQL Data Warehouse (PolyBase)].</span><span class="sxs-lookup"><span data-stu-id="8cca4-120">For a tutorial, see [Load data from Azure blob storage to SQL Data Warehouse (PolyBase)][Load data from Azure blob storage to SQL Data Warehouse (PolyBase)].</span></span>

### <a name="2-use-azure-data-factory"></a><span data-ttu-id="8cca4-121">2. Använda Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="8cca4-121">2. Use Azure Data Factory</span></span>
<span data-ttu-id="8cca4-122">Ett enklare sätt att använda PolyBase kan du skapa ett Azure Data Factory-pipelinen som använder PolyBase för att läsa in data från Azure blob storage till SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="8cca4-122">For a simpler way to use PolyBase, you can create an Azure Data Factory pipeline that uses PolyBase to load data from Azure blob storage into SQL Data Warehouse.</span></span> <span data-ttu-id="8cca4-123">Det går snabbt att konfigurera eftersom du inte behöver att definiera T-SQL-objekt.</span><span class="sxs-lookup"><span data-stu-id="8cca4-123">This is fast to configure since you don't need to define the T-SQL objects.</span></span> <span data-ttu-id="8cca4-124">Om du behöver fråga efter externa data utan att importera den använda T-SQL.</span><span class="sxs-lookup"><span data-stu-id="8cca4-124">If you need to query the external data without importing it, use T-SQL.</span></span> 

<span data-ttu-id="8cca4-125">Översikt över processen för inläsning:</span><span class="sxs-lookup"><span data-stu-id="8cca4-125">Summary of loading process:</span></span>

1. <span data-ttu-id="8cca4-126">Flytta data till Azure blob storage och lagrar den i textfiler.</span><span class="sxs-lookup"><span data-stu-id="8cca4-126">Move your data to Azure blob storage and store it in text files.</span></span> <span data-ttu-id="8cca4-127">Azure Data Factory stöder för närvarande inte ADLS-anslutning med PolyBase).</span><span class="sxs-lookup"><span data-stu-id="8cca4-127">Azure Data Factory does not currently support ADLS connectivity with PolyBase).</span></span>
2. <span data-ttu-id="8cca4-128">Skapa ett Azure Data Factory-pipelinen för att mata in data.</span><span class="sxs-lookup"><span data-stu-id="8cca4-128">Create an Azure Data Factory pipeline to ingest the data.</span></span> <span data-ttu-id="8cca4-129">Använd PolyBase-alternativet.</span><span class="sxs-lookup"><span data-stu-id="8cca4-129">Use the PolyBase option.</span></span>
4. <span data-ttu-id="8cca4-130">Schemalägga och köra pipelinen.</span><span class="sxs-lookup"><span data-stu-id="8cca4-130">Schedule and run the pipeline.</span></span>

<span data-ttu-id="8cca4-131">En självstudiekurs finns [läsa in data från Azure blob storage till SQL Data Warehouse (Azure Data Factory)][Load data from Azure blob storage to SQL Data Warehouse (Azure Data Factory)].</span><span class="sxs-lookup"><span data-stu-id="8cca4-131">For a tutorial, see [Load data from Azure blob storage to SQL Data Warehouse (Azure Data Factory)][Load data from Azure blob storage to SQL Data Warehouse (Azure Data Factory)].</span></span>

## <a name="load-from-sql-server"></a><span data-ttu-id="8cca4-132">Läs in från SQLServer</span><span class="sxs-lookup"><span data-stu-id="8cca4-132">Load from SQL Server</span></span>
<span data-ttu-id="8cca4-133">Du kan använda Integration Services (SSIS), överföring flat-filer eller leverera diskar till Microsoft för att läsa in data från SQL Server till SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="8cca4-133">To load data from SQL Server to SQL Data Warehouse you can use Integration Services (SSIS), transfer flat files, or ship disks to Microsoft.</span></span> <span data-ttu-id="8cca4-134">Läs in finns en sammanfattning av de olika överföring processer och länkar till självstudier.</span><span class="sxs-lookup"><span data-stu-id="8cca4-134">Read on to see a summary of the different loading processes and links to tutorials.</span></span>

<span data-ttu-id="8cca4-135">Om du vill planera en fullständig datamigrering från SQL Server till SQL Data Warehouse, finns det [migreringsöversikt][Migration overview].</span><span class="sxs-lookup"><span data-stu-id="8cca4-135">To plan a full data migration from SQL Server to SQL Data Warehouse, see the [Migration overview][Migration overview].</span></span> 

### <a name="use-integration-services-ssis"></a><span data-ttu-id="8cca4-136">Använda Integration Services (SSIS)</span><span class="sxs-lookup"><span data-stu-id="8cca4-136">Use Integration Services (SSIS)</span></span>
<span data-ttu-id="8cca4-137">Om du redan använder Integration Services (SSIS) paket att läsa in i SQL Server, kan du uppdatera dina paket för att använda SQL Server som käll- och SQL Data Warehouse som mål.</span><span class="sxs-lookup"><span data-stu-id="8cca4-137">If you are already using Integration Services (SSIS) packages to load into SQL Server, you can update your packages to use SQL Server as the source and SQL Data Warehouse as the destination.</span></span> <span data-ttu-id="8cca4-138">Det går snabbt och enkelt att göra, och är ett bra alternativ om du inte vill migrera din inläsningen om du vill använda redan data i molnet.</span><span class="sxs-lookup"><span data-stu-id="8cca4-138">This is quick and easy to do, and is a good choice if you are not trying to migrate your loading process to use data already in the cloud.</span></span> <span data-ttu-id="8cca4-139">Förhållandet är belastningen blir långsammare än att använda PolyBase eftersom den här SSIS inte utför belastningen parallellt.</span><span class="sxs-lookup"><span data-stu-id="8cca4-139">The tradeoff is the load will be slower than using PolyBase because this SSIS does not perform the load in parallel.</span></span>

<span data-ttu-id="8cca4-140">Översikt över processen för inläsning:</span><span class="sxs-lookup"><span data-stu-id="8cca4-140">Summary of loading process:</span></span>

1. <span data-ttu-id="8cca4-141">Ändra Integration Services-paket för att peka till SQL Server-instansen för källan och SQL Data Warehouse-databas för mål.</span><span class="sxs-lookup"><span data-stu-id="8cca4-141">Revise your Integration Services package to point to the SQL Server instance for the source and the SQL Data Warehouse database for the destination.</span></span>
2. <span data-ttu-id="8cca4-142">Migrera schemat till SQL Data Warehouse, om du inte redan har gjort det.</span><span class="sxs-lookup"><span data-stu-id="8cca4-142">Migrate your schema to SQL Data Warehouse, if it is not there already.</span></span>
3. <span data-ttu-id="8cca4-143">Ändra mappningen i dina paket använda endast de datatyper som stöds av SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="8cca4-143">Change the mapping in your packages use only the data types that are supported by SQL Data Warehouse.</span></span>
4. <span data-ttu-id="8cca4-144">Schemalägga och köra paketet.</span><span class="sxs-lookup"><span data-stu-id="8cca4-144">Schedule and run the package.</span></span>

<span data-ttu-id="8cca4-145">En självstudiekurs finns [läsa in data från SQL Server till Azure SQL Data Warehouse (SSIS)][Load data from SQL Server to Azure SQL Data Warehouse (SSIS)].</span><span class="sxs-lookup"><span data-stu-id="8cca4-145">For a tutorial, see [Load data from SQL Server to Azure SQL Data Warehouse (SSIS)][Load data from SQL Server to Azure SQL Data Warehouse (SSIS)].</span></span>

### <a name="use-azcopy-recommended-for--10-tb-data"></a><span data-ttu-id="8cca4-146">Använda AZCopy (rekommenderas för < 10 TB data)</span><span class="sxs-lookup"><span data-stu-id="8cca4-146">Use AZCopy (recommended for < 10 TB data)</span></span>
<span data-ttu-id="8cca4-147">Om dina data är < 10 TB kan du exportera data från SQL Server till flat-filer, kopiera filer till Azure blob storage och sedan använda PolyBase för att läsa in data till SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="8cca4-147">If your data size is < 10 TB, you can export the data from SQL Server to flat files, copy the files to Azure blob storage, and then use PolyBase to load the data into SQL Data Warehouse</span></span>

<span data-ttu-id="8cca4-148">Översikt över processen för inläsning:</span><span class="sxs-lookup"><span data-stu-id="8cca4-148">Summary of loading process:</span></span>

1. <span data-ttu-id="8cca4-149">Använd kommandoradsverktyget BCP för att exportera data från SQL Server till flat-filer.</span><span class="sxs-lookup"><span data-stu-id="8cca4-149">Use the bcp command-line utility to export data from SQL Server to flat files.</span></span>
2. <span data-ttu-id="8cca4-150">Använd kommandoradsverktyget azcopy för att kopiera data från flata filer till Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="8cca4-150">Use the AZCopy command-line utility to copy data from flat files to Azure blob storage.</span></span>
3. <span data-ttu-id="8cca4-151">Använd PolyBase för att läsa in i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="8cca4-151">Use PolyBase to load into SQL Data Warehouse.</span></span>

<span data-ttu-id="8cca4-152">En självstudiekurs finns [läsa in data från Azure blob storage till SQL Data Warehouse (PolyBase)][Load data from Azure blob storage to SQL Data Warehouse (PolyBase)].</span><span class="sxs-lookup"><span data-stu-id="8cca4-152">For a tutorial, see [Load data from Azure blob storage to SQL Data Warehouse (PolyBase)][Load data from Azure blob storage to SQL Data Warehouse (PolyBase)].</span></span>

### <a name="use-bcp"></a><span data-ttu-id="8cca4-153">Använd bcp</span><span class="sxs-lookup"><span data-stu-id="8cca4-153">Use bcp</span></span>
<span data-ttu-id="8cca4-154">Om du har en liten mängd data kan du använda bcp för att läsa in direkt i Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="8cca4-154">If you have a small amount of data you can use bcp to load directly into Azure SQL Data Warehouse.</span></span>

<span data-ttu-id="8cca4-155">Översikt över processen för inläsning:</span><span class="sxs-lookup"><span data-stu-id="8cca4-155">Summary of loading process:</span></span>

1. <span data-ttu-id="8cca4-156">Använd kommandoradsverktyget BCP för att exportera data från SQL Server till flat-filer.</span><span class="sxs-lookup"><span data-stu-id="8cca4-156">Use the bcp command-line utility to export data from SQL Server to flat files.</span></span>
2. <span data-ttu-id="8cca4-157">Använd bcp för att läsa in data från flata filer direkt till SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="8cca4-157">Use bcp to load data from flat files directly to SQL Data Warehouse.</span></span>

<span data-ttu-id="8cca4-158">En självstudiekurs finns [läsa data från SQL Server till Azure SQL Data Warehouse (bcp)][Load data from SQL Server to Azure SQL Data Warehouse (bcp)].</span><span class="sxs-lookup"><span data-stu-id="8cca4-158">For a tutorial, see [Load data from SQL Server to Azure SQL Data Warehouse (bcp)][Load data from SQL Server to Azure SQL Data Warehouse (bcp)].</span></span>

### <a name="use-importexport-recommended-for--10-tb-data"></a><span data-ttu-id="8cca4-159">Använd Import/Export (rekommenderas för > 10 TB data)</span><span class="sxs-lookup"><span data-stu-id="8cca4-159">Use Import/Export (recommended for > 10 TB data)</span></span>
<span data-ttu-id="8cca4-160">Om dina data är > 10 TB och du vill flytta den till Azure, rekommenderar vi att du använder våra disk som levererar tjänsten [Import/Export][Import/Export].</span><span class="sxs-lookup"><span data-stu-id="8cca4-160">If your data size is > 10 TB and you want to move it to Azure, we recommend that you use our disk shipping service [Import/Export][Import/Export].</span></span> 

<span data-ttu-id="8cca4-161">Översikt över processen för inläsning</span><span class="sxs-lookup"><span data-stu-id="8cca4-161">Summary of loading process</span></span>

1. <span data-ttu-id="8cca4-162">Använd kommandoradsverktyget BCP för att exportera data från SQL Server till flat-filer på överföringsbar diskar.</span><span class="sxs-lookup"><span data-stu-id="8cca4-162">Use the bcp command-line utility to export data from SQL Server to flat files on transferrable disks.</span></span>
2. <span data-ttu-id="8cca4-163">Leverera diskar till Microsoft.</span><span class="sxs-lookup"><span data-stu-id="8cca4-163">Ship the disks to Microsoft.</span></span>
3. <span data-ttu-id="8cca4-164">Microsoft läser in data i SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="8cca4-164">Microsoft loads the data into SQL Data Warehouse</span></span>

## <a name="load-from-hdinsight"></a><span data-ttu-id="8cca4-165">Läsa in från HDInsight</span><span class="sxs-lookup"><span data-stu-id="8cca4-165">Load from HDInsight</span></span>
<span data-ttu-id="8cca4-166">SQL Data Warehouse stöder läsning av data från HDInsight via PolyBase.</span><span class="sxs-lookup"><span data-stu-id="8cca4-166">SQL Data Warehouse supports loading data from HDInsight via PolyBase.</span></span> <span data-ttu-id="8cca4-167">Processen är densamma som läser in data från Azure Blob Storage - med PolyBase för att ansluta till HDInsight för att läsa in data.</span><span class="sxs-lookup"><span data-stu-id="8cca4-167">The process is the same as loading data from Azure Blob Storage - using PolyBase to connect to HDInsight to load data.</span></span> 

### <a name="1-use-polybase-and-t-sql"></a><span data-ttu-id="8cca4-168">1. Använd PolyBase och T-SQL</span><span class="sxs-lookup"><span data-stu-id="8cca4-168">1. Use PolyBase and T-SQL</span></span>
<span data-ttu-id="8cca4-169">Översikt över processen för inläsning:</span><span class="sxs-lookup"><span data-stu-id="8cca4-169">Summary of loading process:</span></span>

1. <span data-ttu-id="8cca4-170">Flytta data till HDInsight och lagra den på textfiler, ORC eller parkettgolv format.</span><span class="sxs-lookup"><span data-stu-id="8cca4-170">Move your data to HDInsight and store it in text files, ORC or Parquet format.</span></span>
2. <span data-ttu-id="8cca4-171">Konfigurera externa objekt i SQL Data Warehouse definiera plats och formatet för data.</span><span class="sxs-lookup"><span data-stu-id="8cca4-171">Configure external objects in SQL Data Warehouse to define the location and format of the data.</span></span>
3. <span data-ttu-id="8cca4-172">Köra ett T-SQL-kommando för att läsa in data parallellt i en ny databastabell.</span><span class="sxs-lookup"><span data-stu-id="8cca4-172">Run a T-SQL command to load the data in parallel into a new database table.</span></span>

<span data-ttu-id="8cca4-173">En självstudiekurs finns [läsa in data från Azure blob storage till SQL Data Warehouse (PolyBase)][Load data from Azure blob storage to SQL Data Warehouse (PolyBase)].</span><span class="sxs-lookup"><span data-stu-id="8cca4-173">For a tutorial, see [Load data from Azure blob storage to SQL Data Warehouse (PolyBase)][Load data from Azure blob storage to SQL Data Warehouse (PolyBase)].</span></span>

## <a name="recommendations"></a><span data-ttu-id="8cca4-174">Rekommendationer</span><span class="sxs-lookup"><span data-stu-id="8cca4-174">Recommendations</span></span>
<span data-ttu-id="8cca4-175">Många av våra samarbetspartners har inläsning av lösningar.</span><span class="sxs-lookup"><span data-stu-id="8cca4-175">Many of our partners have loading solutions.</span></span> <span data-ttu-id="8cca4-176">Om du vill veta mer, se en lista över våra [lösningspartners][solution partners].</span><span class="sxs-lookup"><span data-stu-id="8cca4-176">To find out more, see a list of our [solution partners][solution partners].</span></span> 

<span data-ttu-id="8cca4-177">Om dina data kommer från en icke-relationella källa och du vill läsa in det i SQL Data Warehouse måste du omvandla det till rader och kolumner innan du läser in.</span><span class="sxs-lookup"><span data-stu-id="8cca4-177">If your data is coming from a non-relational source and you want to load it into SQL Data Warehouse you will need to transform it into rows and columns before you load it.</span></span> <span data-ttu-id="8cca4-178">Omvandlade data behöver inte lagras i en databas, den kan lagras i textfiler.</span><span class="sxs-lookup"><span data-stu-id="8cca4-178">The transformed data doesn't need to be stored in a database, it can be stored in text files.</span></span>

<span data-ttu-id="8cca4-179">Skapa statistik på nyinlästa data.</span><span class="sxs-lookup"><span data-stu-id="8cca4-179">Create statistics on newly loaded data.</span></span> <span data-ttu-id="8cca4-180">SQL Data Warehouse stöder inte än automatiskt skapande eller uppdatering av statistik.</span><span class="sxs-lookup"><span data-stu-id="8cca4-180">Azure SQL Data Warehouse does not yet support auto create or auto update statistics.</span></span>  <span data-ttu-id="8cca4-181">För att få bästa möjliga prestanda från dina frågor, är det viktigt att skapa statistik på alla kolumner i alla tabeller efter den första inläsningen eller vid betydande dataändringar i data.</span><span class="sxs-lookup"><span data-stu-id="8cca4-181">In order to get the best performance from your queries, it's important to create statistics on all columns of all tables after the first load or any substantial changes occur in the data.</span></span>  <span data-ttu-id="8cca4-182">Mer information finns i [statistik][Statistics].</span><span class="sxs-lookup"><span data-stu-id="8cca4-182">For details, see [Statistics][Statistics].</span></span>

## <a name="next-steps"></a><span data-ttu-id="8cca4-183">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8cca4-183">Next steps</span></span>
<span data-ttu-id="8cca4-184">För fler utvecklingstips, se den [utvecklingsöversikt][development overview].</span><span class="sxs-lookup"><span data-stu-id="8cca4-184">For more development tips, see the [development overview][development overview].</span></span>

<!--Image references-->

<!--Article references-->
[Load data from Azure blob storage to SQL Data Warehouse (PolyBase)]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md
[Load data from Azure blob storage to SQL Data Warehouse (Azure Data Factory)]: ./sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md
[Load data from SQL Server to Azure SQL Data Warehouse (SSIS)]: ./sql-data-warehouse-load-from-sql-server-with-integration-services.md
[Load data from SQL Server to Azure SQL Data Warehouse (bcp)]: ./sql-data-warehouse-load-from-sql-server-with-bcp.md
[Load data from SQL Server to Azure SQL Data Warehouse (AZCopy)]: ./sql-data-warehouse-load-from-sql-server-with-azcopy.md

[Load sample databases]: ./sql-data-warehouse-load-sample-databases.md
[Migration overview]: ./sql-data-warehouse-overview-migrate.md
[solution partners]: ./sql-data-warehouse-partner-business-intelligence.md
[development overview]: ./sql-data-warehouse-overview-develop.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->

<!--Other Web references-->
[Import/Export]: https://azure.microsoft.com/documentation/articles/storage-import-export-service/
