---
title: aaaLoad data till Azure SQL Data Warehouse | Microsoft Docs
description: "Lär dig hello vanliga scenarier för datainläsning till SQL Data Warehouse. Dessa inkluderar med PolyBase, Azure blob storage, flat-filer och disk leverans. Du kan också använda verktyg från tredje part."
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
ms.openlocfilehash: d1a5063f484e9bd95f854e27a4baed512148aad0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-into-azure-sql-data-warehouse"></a><span data-ttu-id="02296-105">Läs in data till Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="02296-105">Load data into Azure SQL Data Warehouse</span></span>
<span data-ttu-id="02296-106">En sammanfattning av hello scenariot alternativ och rekommendationer för att läsa in data i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="02296-106">A summary of hello scenario options and recommendations for loading data into SQL Data Warehouse.</span></span>

<span data-ttu-id="02296-107">hello svåraste del inläsning av data vanligtvis förbereder hello data för hello belastningen.</span><span class="sxs-lookup"><span data-stu-id="02296-107">hello hardest part of loading data is usually preparing hello data for hello load.</span></span> <span data-ttu-id="02296-108">Azure förenklar inläsning genom att använda Azure blob-lagring som en gemensam datalager för många hello tjänster och använda Azure Data Factory tooorchestrate kommunikations- och flytt mellan hello Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="02296-108">Azure simplifies loading by using Azure blob storage as a common data store for many of hello services, and using Azure Data Factory tooorchestrate communication and data movement between hello Azure services.</span></span> <span data-ttu-id="02296-109">De här processerna är integrerade med PolyBase-teknik som använder massivt parallell bearbetning (MPP) tooload data parallellt från Azure blob storage till SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="02296-109">These processes are integrated with PolyBase technology which uses massively parallel processing (MPP) tooload data in parallel from Azure blob storage into SQL Data Warehouse.</span></span> 

<span data-ttu-id="02296-110">Självstudier som läser in exempeldatabaserna finns [ladda exempeldatabaserna][Load sample databases].</span><span class="sxs-lookup"><span data-stu-id="02296-110">For tutorials that load sample databases, see [Load sample databases][Load sample databases].</span></span>

## <a name="load-from-azure-blob-storage"></a><span data-ttu-id="02296-111">Läsa in från Azure blob storage</span><span class="sxs-lookup"><span data-stu-id="02296-111">Load from Azure blob storage</span></span>
<span data-ttu-id="02296-112">hello snabbaste sättet tooimport data till SQL Data Warehouse är toouse PolyBase tooload data från Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="02296-112">hello fastest way tooimport data into SQL Data Warehouse is toouse PolyBase tooload data from Azure blob storage.</span></span> <span data-ttu-id="02296-113">PolyBase använder SQL Data Warehouses databearbetning massivt parallell (MPP) design tooload parallellt från Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="02296-113">PolyBase uses SQL Data Warehouse's massively parallel processing (MPP) design tooload data in parallel from Azure blob storage.</span></span> <span data-ttu-id="02296-114">toouse PolyBase som du kan använda T-SQL-kommandon eller ett Azure Data Factory-pipelinen.</span><span class="sxs-lookup"><span data-stu-id="02296-114">toouse PolyBase, you can use T-SQL commands or an Azure Data Factory pipeline.</span></span>

### <a name="1-use-polybase-and-t-sql"></a><span data-ttu-id="02296-115">1. Använd PolyBase och T-SQL</span><span class="sxs-lookup"><span data-stu-id="02296-115">1. Use PolyBase and T-SQL</span></span>
<span data-ttu-id="02296-116">Översikt över processen för inläsning:</span><span class="sxs-lookup"><span data-stu-id="02296-116">Summary of loading process:</span></span>

1. <span data-ttu-id="02296-117">Flytta dina data tooAzure blob storage eller Azure Data Lake Store och lagra den i textfiler.</span><span class="sxs-lookup"><span data-stu-id="02296-117">Move your data tooAzure blob storage or Azure Data Lake Store and store it in text files.</span></span>
2. <span data-ttu-id="02296-118">Konfigurera externa objekt i SQL Data Warehouse toodefine hello plats och format för hello data</span><span class="sxs-lookup"><span data-stu-id="02296-118">Configure external objects in SQL Data Warehouse toodefine hello location and format of hello data</span></span>
3. <span data-ttu-id="02296-119">Kör en T-SQL-kommandot tooload hello data parallellt i en ny databastabell.</span><span class="sxs-lookup"><span data-stu-id="02296-119">Run a T-SQL command tooload hello data in parallel into a new database table.</span></span>

<!-- 5. Schedule and run a loading job. --> 

<span data-ttu-id="02296-120">En självstudiekurs finns [läsa in data från Azure blob storage tooSQL Data Warehouse (PolyBase)][Load data from Azure blob storage tooSQL Data Warehouse (PolyBase)].</span><span class="sxs-lookup"><span data-stu-id="02296-120">For a tutorial, see [Load data from Azure blob storage tooSQL Data Warehouse (PolyBase)][Load data from Azure blob storage tooSQL Data Warehouse (PolyBase)].</span></span>

### <a name="2-use-azure-data-factory"></a><span data-ttu-id="02296-121">2. Använda Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="02296-121">2. Use Azure Data Factory</span></span>
<span data-ttu-id="02296-122">För ett enklare sätt toouse PolyBase, kan du skapa ett Azure Data Factory-pipelinen som använder PolyBase tooload data från Azure blob storage till SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="02296-122">For a simpler way toouse PolyBase, you can create an Azure Data Factory pipeline that uses PolyBase tooload data from Azure blob storage into SQL Data Warehouse.</span></span> <span data-ttu-id="02296-123">Detta är snabb tooconfigure eftersom du inte behöver toodefine hello T-SQL-objekt.</span><span class="sxs-lookup"><span data-stu-id="02296-123">This is fast tooconfigure since you don't need toodefine hello T-SQL objects.</span></span> <span data-ttu-id="02296-124">Om du behöver tooquery hello externa data utan att importera den kan använda T-SQL.</span><span class="sxs-lookup"><span data-stu-id="02296-124">If you need tooquery hello external data without importing it, use T-SQL.</span></span> 

<span data-ttu-id="02296-125">Översikt över processen för inläsning:</span><span class="sxs-lookup"><span data-stu-id="02296-125">Summary of loading process:</span></span>

1. <span data-ttu-id="02296-126">Flytta dina data tooAzure blob storage och lagrar den i textfiler.</span><span class="sxs-lookup"><span data-stu-id="02296-126">Move your data tooAzure blob storage and store it in text files.</span></span> <span data-ttu-id="02296-127">Azure Data Factory stöder för närvarande inte ADLS-anslutning med PolyBase).</span><span class="sxs-lookup"><span data-stu-id="02296-127">Azure Data Factory does not currently support ADLS connectivity with PolyBase).</span></span>
2. <span data-ttu-id="02296-128">Skapa ett Azure Data Factory pipeline tooingest hello data.</span><span class="sxs-lookup"><span data-stu-id="02296-128">Create an Azure Data Factory pipeline tooingest hello data.</span></span> <span data-ttu-id="02296-129">Använd hello PolyBase-alternativet.</span><span class="sxs-lookup"><span data-stu-id="02296-129">Use hello PolyBase option.</span></span>
4. <span data-ttu-id="02296-130">Schemalägga och köra hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="02296-130">Schedule and run hello pipeline.</span></span>

<span data-ttu-id="02296-131">En självstudiekurs finns [läsa in data från Azure blob storage tooSQL Data Warehouse (Azure Data Factory)][Load data from Azure blob storage tooSQL Data Warehouse (Azure Data Factory)].</span><span class="sxs-lookup"><span data-stu-id="02296-131">For a tutorial, see [Load data from Azure blob storage tooSQL Data Warehouse (Azure Data Factory)][Load data from Azure blob storage tooSQL Data Warehouse (Azure Data Factory)].</span></span>

## <a name="load-from-sql-server"></a><span data-ttu-id="02296-132">Läs in från SQLServer</span><span class="sxs-lookup"><span data-stu-id="02296-132">Load from SQL Server</span></span>
<span data-ttu-id="02296-133">tooload data från SQL Server-tooSQL datalagret som du kan använda Integration Services (SSIS), överför flat-filer eller leverera diskar tooMicrosoft.</span><span class="sxs-lookup"><span data-stu-id="02296-133">tooload data from SQL Server tooSQL Data Warehouse you can use Integration Services (SSIS), transfer flat files, or ship disks tooMicrosoft.</span></span> <span data-ttu-id="02296-134">Läs vidare toosee en sammanfattning av hello olika inläsning tootutorials processer och länkar.</span><span class="sxs-lookup"><span data-stu-id="02296-134">Read on toosee a summary of hello different loading processes and links tootutorials.</span></span>

<span data-ttu-id="02296-135">tooplan en fullständig datamigrering från SQL Server-tooSQL datalagret finns hello [migreringsöversikt][Migration overview].</span><span class="sxs-lookup"><span data-stu-id="02296-135">tooplan a full data migration from SQL Server tooSQL Data Warehouse, see hello [Migration overview][Migration overview].</span></span> 

### <a name="use-integration-services-ssis"></a><span data-ttu-id="02296-136">Använda Integration Services (SSIS)</span><span class="sxs-lookup"><span data-stu-id="02296-136">Use Integration Services (SSIS)</span></span>
<span data-ttu-id="02296-137">Om du redan använder Integration Services (SSIS) paket tooload i SQL Server, kan du uppdatera ditt paket toouse SQL Server som hello käll- och SQL Data Warehouse som hello mål.</span><span class="sxs-lookup"><span data-stu-id="02296-137">If you are already using Integration Services (SSIS) packages tooload into SQL Server, you can update your packages toouse SQL Server as hello source and SQL Data Warehouse as hello destination.</span></span> <span data-ttu-id="02296-138">Det går snabbt och enkelt toodo och är ett bra alternativ om du inte försöker toomigrate din inläsning bearbeta toouse data redan i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="02296-138">This is quick and easy toodo, and is a good choice if you are not trying toomigrate your loading process toouse data already in hello cloud.</span></span> <span data-ttu-id="02296-139">hello förhållandet är hello belastningen blir långsammare än att använda PolyBase eftersom den här SSIS inte utför hello belastningen parallellt.</span><span class="sxs-lookup"><span data-stu-id="02296-139">hello tradeoff is hello load will be slower than using PolyBase because this SSIS does not perform hello load in parallel.</span></span>

<span data-ttu-id="02296-140">Översikt över processen för inläsning:</span><span class="sxs-lookup"><span data-stu-id="02296-140">Summary of loading process:</span></span>

1. <span data-ttu-id="02296-141">Ändra Integration Services paketet toopoint toohello SQL Server-instans för hello källa och hello SQL Data Warehouse-databas för hello mål.</span><span class="sxs-lookup"><span data-stu-id="02296-141">Revise your Integration Services package toopoint toohello SQL Server instance for hello source and hello SQL Data Warehouse database for hello destination.</span></span>
2. <span data-ttu-id="02296-142">Migrera din schemat tooSQL datalagret, om du inte redan har gjort det.</span><span class="sxs-lookup"><span data-stu-id="02296-142">Migrate your schema tooSQL Data Warehouse, if it is not there already.</span></span>
3. <span data-ttu-id="02296-143">Ändra hello mappning i dina paket Använd endast hello-datatyper som stöds av SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="02296-143">Change hello mapping in your packages use only hello data types that are supported by SQL Data Warehouse.</span></span>
4. <span data-ttu-id="02296-144">Schemalägga och köra hello-paketet.</span><span class="sxs-lookup"><span data-stu-id="02296-144">Schedule and run hello package.</span></span>

<span data-ttu-id="02296-145">En självstudiekurs finns [läsa in data från SQL Server-tooAzure SQL Data Warehouse (SSIS)][Load data from SQL Server tooAzure SQL Data Warehouse (SSIS)].</span><span class="sxs-lookup"><span data-stu-id="02296-145">For a tutorial, see [Load data from SQL Server tooAzure SQL Data Warehouse (SSIS)][Load data from SQL Server tooAzure SQL Data Warehouse (SSIS)].</span></span>

### <a name="use-azcopy-recommended-for--10-tb-data"></a><span data-ttu-id="02296-146">Använda AZCopy (rekommenderas för < 10 TB data)</span><span class="sxs-lookup"><span data-stu-id="02296-146">Use AZCopy (recommended for < 10 TB data)</span></span>
<span data-ttu-id="02296-147">Om dina data är < 10 TB kan du exportera hello data från SQL Server tooflat filer, kopiera hello filer tooAzure blob-lagring och sedan använda PolyBase tooload hello data till SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="02296-147">If your data size is < 10 TB, you can export hello data from SQL Server tooflat files, copy hello files tooAzure blob storage, and then use PolyBase tooload hello data into SQL Data Warehouse</span></span>

<span data-ttu-id="02296-148">Översikt över processen för inläsning:</span><span class="sxs-lookup"><span data-stu-id="02296-148">Summary of loading process:</span></span>

1. <span data-ttu-id="02296-149">Använd hello bcp kommandoradsverktyget tooexport data från SQL Server tooflat filer.</span><span class="sxs-lookup"><span data-stu-id="02296-149">Use hello bcp command-line utility tooexport data from SQL Server tooflat files.</span></span>
2. <span data-ttu-id="02296-150">Använda AZCopy hello kommandoradsverktyget toocopy data från flata filer tooAzure blob storage.</span><span class="sxs-lookup"><span data-stu-id="02296-150">Use hello AZCopy command-line utility toocopy data from flat files tooAzure blob storage.</span></span>
3. <span data-ttu-id="02296-151">Använd PolyBase tooload till SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="02296-151">Use PolyBase tooload into SQL Data Warehouse.</span></span>

<span data-ttu-id="02296-152">En självstudiekurs finns [läsa in data från Azure blob storage tooSQL Data Warehouse (PolyBase)][Load data from Azure blob storage tooSQL Data Warehouse (PolyBase)].</span><span class="sxs-lookup"><span data-stu-id="02296-152">For a tutorial, see [Load data from Azure blob storage tooSQL Data Warehouse (PolyBase)][Load data from Azure blob storage tooSQL Data Warehouse (PolyBase)].</span></span>

### <a name="use-bcp"></a><span data-ttu-id="02296-153">Använd bcp</span><span class="sxs-lookup"><span data-stu-id="02296-153">Use bcp</span></span>
<span data-ttu-id="02296-154">Om du har en liten mängd data kan du använda bcp tooload direkt i Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="02296-154">If you have a small amount of data you can use bcp tooload directly into Azure SQL Data Warehouse.</span></span>

<span data-ttu-id="02296-155">Översikt över processen för inläsning:</span><span class="sxs-lookup"><span data-stu-id="02296-155">Summary of loading process:</span></span>

1. <span data-ttu-id="02296-156">Använd hello bcp kommandoradsverktyget tooexport data från SQL Server tooflat filer.</span><span class="sxs-lookup"><span data-stu-id="02296-156">Use hello bcp command-line utility tooexport data from SQL Server tooflat files.</span></span>
2. <span data-ttu-id="02296-157">Använd bcp tooload data från 2D filer direkt tooSQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="02296-157">Use bcp tooload data from flat files directly tooSQL Data Warehouse.</span></span>

<span data-ttu-id="02296-158">En självstudiekurs finns [läsa in data från SQL Server-tooAzure SQL Data Warehouse (bcp)][Load data from SQL Server tooAzure SQL Data Warehouse (bcp)].</span><span class="sxs-lookup"><span data-stu-id="02296-158">For a tutorial, see [Load data from SQL Server tooAzure SQL Data Warehouse (bcp)][Load data from SQL Server tooAzure SQL Data Warehouse (bcp)].</span></span>

### <a name="use-importexport-recommended-for--10-tb-data"></a><span data-ttu-id="02296-159">Använd Import/Export (rekommenderas för > 10 TB data)</span><span class="sxs-lookup"><span data-stu-id="02296-159">Use Import/Export (recommended for > 10 TB data)</span></span>
<span data-ttu-id="02296-160">Om dina data är > 10 TB och du vill toomove den tooAzure, rekommenderar vi att du använder våra disk som levererar tjänsten [Import/Export][Import/Export].</span><span class="sxs-lookup"><span data-stu-id="02296-160">If your data size is > 10 TB and you want toomove it tooAzure, we recommend that you use our disk shipping service [Import/Export][Import/Export].</span></span> 

<span data-ttu-id="02296-161">Översikt över processen för inläsning</span><span class="sxs-lookup"><span data-stu-id="02296-161">Summary of loading process</span></span>

1. <span data-ttu-id="02296-162">Använd hello bcp kommandoradsverktyget tooexport data från SQL Server tooflat filer på överföringsbar diskar.</span><span class="sxs-lookup"><span data-stu-id="02296-162">Use hello bcp command-line utility tooexport data from SQL Server tooflat files on transferrable disks.</span></span>
2. <span data-ttu-id="02296-163">Leverera hello diskar tooMicrosoft.</span><span class="sxs-lookup"><span data-stu-id="02296-163">Ship hello disks tooMicrosoft.</span></span>
3. <span data-ttu-id="02296-164">Hello data hämtas från Microsoft till SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="02296-164">Microsoft loads hello data into SQL Data Warehouse</span></span>

## <a name="load-from-hdinsight"></a><span data-ttu-id="02296-165">Läsa in från HDInsight</span><span class="sxs-lookup"><span data-stu-id="02296-165">Load from HDInsight</span></span>
<span data-ttu-id="02296-166">SQL Data Warehouse stöder läsning av data från HDInsight via PolyBase.</span><span class="sxs-lookup"><span data-stu-id="02296-166">SQL Data Warehouse supports loading data from HDInsight via PolyBase.</span></span> <span data-ttu-id="02296-167">hello-processen är hello samma som läser in data från Azure Blob Storage - med PolyBase tooconnect tooHDInsight tooload data.</span><span class="sxs-lookup"><span data-stu-id="02296-167">hello process is hello same as loading data from Azure Blob Storage - using PolyBase tooconnect tooHDInsight tooload data.</span></span> 

### <a name="1-use-polybase-and-t-sql"></a><span data-ttu-id="02296-168">1. Använd PolyBase och T-SQL</span><span class="sxs-lookup"><span data-stu-id="02296-168">1. Use PolyBase and T-SQL</span></span>
<span data-ttu-id="02296-169">Översikt över processen för inläsning:</span><span class="sxs-lookup"><span data-stu-id="02296-169">Summary of loading process:</span></span>

1. <span data-ttu-id="02296-170">Flytta dina data tooHDInsight och lagra den i textfiler, ORC eller parkettgolv format.</span><span class="sxs-lookup"><span data-stu-id="02296-170">Move your data tooHDInsight and store it in text files, ORC or Parquet format.</span></span>
2. <span data-ttu-id="02296-171">Konfigurera externa objekt i SQL Data Warehouse toodefine hello plats och format för hello data.</span><span class="sxs-lookup"><span data-stu-id="02296-171">Configure external objects in SQL Data Warehouse toodefine hello location and format of hello data.</span></span>
3. <span data-ttu-id="02296-172">Kör en T-SQL-kommandot tooload hello data parallellt i en ny databastabell.</span><span class="sxs-lookup"><span data-stu-id="02296-172">Run a T-SQL command tooload hello data in parallel into a new database table.</span></span>

<span data-ttu-id="02296-173">En självstudiekurs finns [läsa in data från Azure blob storage tooSQL Data Warehouse (PolyBase)][Load data from Azure blob storage tooSQL Data Warehouse (PolyBase)].</span><span class="sxs-lookup"><span data-stu-id="02296-173">For a tutorial, see [Load data from Azure blob storage tooSQL Data Warehouse (PolyBase)][Load data from Azure blob storage tooSQL Data Warehouse (PolyBase)].</span></span>

## <a name="recommendations"></a><span data-ttu-id="02296-174">Rekommendationer</span><span class="sxs-lookup"><span data-stu-id="02296-174">Recommendations</span></span>
<span data-ttu-id="02296-175">Många av våra samarbetspartners har inläsning av lösningar.</span><span class="sxs-lookup"><span data-stu-id="02296-175">Many of our partners have loading solutions.</span></span> <span data-ttu-id="02296-176">toofind mer information, se en lista över våra [lösningspartners][solution partners].</span><span class="sxs-lookup"><span data-stu-id="02296-176">toofind out more, see a list of our [solution partners][solution partners].</span></span> 

<span data-ttu-id="02296-177">Om dina data kommer från en icke-relationella källa och du vill tooload till SQL Data Warehouse du behöver tootransform till rader och kolumner innan du läser in.</span><span class="sxs-lookup"><span data-stu-id="02296-177">If your data is coming from a non-relational source and you want tooload it into SQL Data Warehouse you will need tootransform it into rows and columns before you load it.</span></span> <span data-ttu-id="02296-178">hello omvandlas data behöver inte toobe som lagras i en databas, den kan lagras i textfiler.</span><span class="sxs-lookup"><span data-stu-id="02296-178">hello transformed data doesn't need toobe stored in a database, it can be stored in text files.</span></span>

<span data-ttu-id="02296-179">Skapa statistik på nyinlästa data.</span><span class="sxs-lookup"><span data-stu-id="02296-179">Create statistics on newly loaded data.</span></span> <span data-ttu-id="02296-180">SQL Data Warehouse stöder inte än automatiskt skapande eller uppdatering av statistik.</span><span class="sxs-lookup"><span data-stu-id="02296-180">Azure SQL Data Warehouse does not yet support auto create or auto update statistics.</span></span>  <span data-ttu-id="02296-181">Det är viktigt toocreate statistik på alla kolumner i alla tabeller efter hello först läsa in eller vid betydande dataändringar i hello data i ordning tooget hello bästa prestanda från dina frågor.</span><span class="sxs-lookup"><span data-stu-id="02296-181">In order tooget hello best performance from your queries, it's important toocreate statistics on all columns of all tables after hello first load or any substantial changes occur in hello data.</span></span>  <span data-ttu-id="02296-182">Mer information finns i [statistik][Statistics].</span><span class="sxs-lookup"><span data-stu-id="02296-182">For details, see [Statistics][Statistics].</span></span>

## <a name="next-steps"></a><span data-ttu-id="02296-183">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="02296-183">Next steps</span></span>
<span data-ttu-id="02296-184">För fler utvecklingstips, se hello [utvecklingsöversikt][development overview].</span><span class="sxs-lookup"><span data-stu-id="02296-184">For more development tips, see hello [development overview][development overview].</span></span>

<!--Image references-->

<!--Article references-->
[Load data from Azure blob storage tooSQL Data Warehouse (PolyBase)]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md
[Load data from Azure blob storage tooSQL Data Warehouse (Azure Data Factory)]: ./sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md
[Load data from SQL Server tooAzure SQL Data Warehouse (SSIS)]: ./sql-data-warehouse-load-from-sql-server-with-integration-services.md
[Load data from SQL Server tooAzure SQL Data Warehouse (bcp)]: ./sql-data-warehouse-load-from-sql-server-with-bcp.md
[Load data from SQL Server tooAzure SQL Data Warehouse (AZCopy)]: ./sql-data-warehouse-load-from-sql-server-with-azcopy.md

[Load sample databases]: ./sql-data-warehouse-load-sample-databases.md
[Migration overview]: ./sql-data-warehouse-overview-migrate.md
[solution partners]: ./sql-data-warehouse-partner-business-intelligence.md
[development overview]: ./sql-data-warehouse-overview-develop.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->

<!--Other Web references-->
[Import/Export]: https://azure.microsoft.com/documentation/articles/storage-import-export-service/
