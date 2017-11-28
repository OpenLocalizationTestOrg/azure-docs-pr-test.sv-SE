---
title: "aaaIdentify avancerade analyser scenarier för Azure Machine Learning | Microsoft Docs"
description: "Välj hello lämpliga scenarier för att göra avancerade förutsägelseanalys med hello Team datavetenskap Process."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 53aecc1e-5089-42cf-8d44-77678653f92d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: 52c6bb10d6df4f640a4f66cf17cf4993cc1067b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scenarios-for-advanced-analytics-in-azure-machine-learning"></a><span data-ttu-id="1e6ef-103">Scenarier för avancerade analyser i Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="1e6ef-103">Scenarios for advanced analytics in Azure Machine Learning</span></span>
<span data-ttu-id="1e6ef-104">Den här artikeln beskrivs hello olika exempel datakällor och målscenarier som kan hanteras av hello [Team Data vetenskap processen (TDSP)](data-science-process-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1e6ef-104">This article outlines hello variety of sample data sources and target scenarios that can be handled by hello [Team Data Science Process (TDSP)](data-science-process-overview.md).</span></span> <span data-ttu-id="1e6ef-105">Hej TDSP innehåller systematiskt för team toocollaborate om hur du skapar intelligent program.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-105">hello TDSP provides a systematic approach for teams toocollaborate on building intelligent applications.</span></span> <span data-ttu-id="1e6ef-106">hello-scenarier som presenteras här visar alternativen i hello databearbetning arbetsflöde som beror på hello dataegenskaper källplatser och mål-databaser i Azure.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-106">hello scenarios presented here illustrate options available in hello data processing workflow that depend on hello data characteristics, source locations, and target repositories in Azure.</span></span>

<span data-ttu-id="1e6ef-107">Hej **beslutsträdet** för att välja hello exempelscenarier som passar för dina data och målet visas i hello sista avsnittet.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-107">hello **decision tree** for selecting hello sample scenarios that is appropriate for your data and objective is presented in hello last section.</span></span>

> [!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]
> 
> 

<span data-ttu-id="1e6ef-108">Var och en av hello följande avsnitt innehåller ett exempelscenario.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-108">Each of hello following sections presents a sample scenario.</span></span> <span data-ttu-id="1e6ef-109">För varje scenario ett möjligt datavetenskap eller avancerade analyser flödet och ge support för Azure-resurser visas.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-109">For each scenario, a possible data science or advanced analytics flow and supporting Azure resources are listed.</span></span>

> [!NOTE]
> <span data-ttu-id="1e6ef-110">**För alla hello följande scenarier, måste du:**
> </span><span class="sxs-lookup"><span data-stu-id="1e6ef-110">**For all of hello following scenarios, you need to:**
</span></span><br/>
> 
> * <span data-ttu-id="1e6ef-111">[Skapa ett lagringskonto](../storage/common/storage-create-storage-account.md)
>   </span><span class="sxs-lookup"><span data-stu-id="1e6ef-111">[Create a storage account](../storage/common/storage-create-storage-account.md)
</span></span><br/>
> * [<span data-ttu-id="1e6ef-112">Skapa en arbetsyta för Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="1e6ef-112">Create an Azure Machine Learning workspace</span></span>](machine-learning-create-workspace.md)
> 
> 

## <span data-ttu-id="1e6ef-113"><a name="smalllocal"></a>Scenariot \#1: liten toomedium tabular datauppsättning i lokala filer</span><span class="sxs-lookup"><span data-stu-id="1e6ef-113"><a name="smalllocal"></a>Scenario \#1: Small toomedium tabular dataset in a local files</span></span>
![Lokala filer i små toomedium][1]

#### <a name="additional-azure-resources-none"></a><span data-ttu-id="1e6ef-115">Ytterligare Azure-resurser: ingen</span><span class="sxs-lookup"><span data-stu-id="1e6ef-115">Additional Azure resources: None</span></span>
1. <span data-ttu-id="1e6ef-116">Logga in toohello [Azure Machine Learning Studio](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="1e6ef-116">Sign in toohello [Azure Machine Learning Studio](https://studio.azureml.net/).</span></span>
2. <span data-ttu-id="1e6ef-117">Överför en datamängd.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-117">Upload a dataset.</span></span>
3. <span data-ttu-id="1e6ef-118">Skapa ett flöde för Azure Machine Learning-experiment som börjar med överförda datauppsättning/ar.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-118">Build an Azure Machine Learning experiment flow starting with uploaded dataset(s).</span></span>

## <span data-ttu-id="1e6ef-119"><a name="smalllocalprocess"></a>Scenariot \#2: liten toomedium dataset för lokala filer som krävs</span><span class="sxs-lookup"><span data-stu-id="1e6ef-119"><a name="smalllocalprocess"></a>Scenario \#2: Small toomedium dataset of local files that require processing</span></span>
![Liten toomedium lokala filer med bearbetning][2]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a><span data-ttu-id="1e6ef-121">Ytterligare Azure-resurser: Azure-dator (IPython anteckningsboken server)</span><span class="sxs-lookup"><span data-stu-id="1e6ef-121">Additional Azure resources: Azure Virtual Machine (IPython Notebook server)</span></span>
1. <span data-ttu-id="1e6ef-122">Skapa en Azure-dator som kör IPython bärbar dator.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-122">Create an Azure Virtual Machine running IPython Notebook.</span></span>
2. <span data-ttu-id="1e6ef-123">Ladda upp data tooan Azure storage-behållare.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-123">Upload data tooan Azure storage container.</span></span>
3. <span data-ttu-id="1e6ef-124">Förbearbeta och rensa data i IPython anteckningsbok, kommer åt data från Azure storage-behållare.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-124">Pre-process and clean data in IPython Notebook, accessing data from Azure storage container.</span></span>
4. <span data-ttu-id="1e6ef-125">Transformera data toocleaned, tabellform.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-125">Transform data toocleaned, tabular form.</span></span>
5. <span data-ttu-id="1e6ef-126">Spara transformerade data i Azure BLOB.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-126">Save transformed data in Azure blobs.</span></span>
6. <span data-ttu-id="1e6ef-127">Logga in toohello [Azure Machine Learning Studio](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="1e6ef-127">Sign in toohello [Azure Machine Learning Studio](https://studio.azureml.net/).</span></span>
7. <span data-ttu-id="1e6ef-128">Läsa hello data från Azure BLOB med hello [importera Data] [ import-data] modul.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-128">Read hello data from Azure blobs using hello [Import Data][import-data] module.</span></span>
8. <span data-ttu-id="1e6ef-129">Skapa ett flöde för Azure Machine Learning-experiment som börjar med infogade datauppsättning/ar.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-129">Build an Azure Machine Learning experiment flow starting with ingested dataset(s).</span></span>

## <span data-ttu-id="1e6ef-130"><a name="largelocal"></a>Scenariot \#3: stora dataset av lokala filer, riktad på Azure-BLOB</span><span class="sxs-lookup"><span data-stu-id="1e6ef-130"><a name="largelocal"></a>Scenario \#3: Large dataset of local files, targeting Azure Blobs</span></span>
![Stora lokala filer][3]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a><span data-ttu-id="1e6ef-132">Ytterligare Azure-resurser: Azure-dator (IPython anteckningsboken server)</span><span class="sxs-lookup"><span data-stu-id="1e6ef-132">Additional Azure resources: Azure Virtual Machine (IPython Notebook server)</span></span>
1. <span data-ttu-id="1e6ef-133">Skapa en Azure-dator som kör IPython bärbar dator.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-133">Create an Azure Virtual Machine running IPython Notebook.</span></span>
2. <span data-ttu-id="1e6ef-134">Ladda upp data tooan Azure storage-behållare.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-134">Upload data tooan Azure storage container.</span></span>
3. <span data-ttu-id="1e6ef-135">Förbearbeta och rensa data i IPython anteckningsbok, kommer åt data från Azure BLOB.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-135">Pre-process and clean data in IPython Notebook, accessing data from Azure blobs.</span></span>
4. <span data-ttu-id="1e6ef-136">Transformera data toocleaned, tabellform, om det behövs.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-136">Transform data toocleaned, tabular form, if needed.</span></span>
5. <span data-ttu-id="1e6ef-137">Utforska data och skapa funktioner efter behov.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-137">Explore data, and create features as needed.</span></span>
6. <span data-ttu-id="1e6ef-138">Extrahera ett datasampel för små till medelstora.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-138">Extract a small-to-medium data sample.</span></span>
7. <span data-ttu-id="1e6ef-139">Spara hello exempeldata i Azure BLOB.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-139">Save hello sampled data in Azure blobs.</span></span>
8. <span data-ttu-id="1e6ef-140">Logga in toohello [Azure Machine Learning Studio](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="1e6ef-140">Sign in toohello [Azure Machine Learning Studio](https://studio.azureml.net/).</span></span>
9. <span data-ttu-id="1e6ef-141">Läsa hello data från Azure BLOB med hello [importera Data] [ import-data] modul.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-141">Read hello data from Azure blobs using hello [Import Data][import-data] module.</span></span>
10. <span data-ttu-id="1e6ef-142">Skapa flödet för Azure Machine Learning-experiment från och med infogade datauppsättning/ar.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-142">Build Azure Machine Learning experiment flow starting with ingested dataset(s).</span></span>

## <span data-ttu-id="1e6ef-143"><a name="smalllocaltodb"></a>Scenariot \#4: liten toomedium dataset av lokala filer, SQL Server i en virtuell dator i Azure som mål</span><span class="sxs-lookup"><span data-stu-id="1e6ef-143"><a name="smalllocaltodb"></a>Scenario \#4: Small toomedium dataset of local files, targeting SQL Server in an Azure Virtual Machine</span></span>
![Liten toomedium lokala filer tooSQL DB i Azure][4]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a><span data-ttu-id="1e6ef-145">Ytterligare Azure-resurser: Azure-dator (SQL Server / IPython anteckningsboken server)</span><span class="sxs-lookup"><span data-stu-id="1e6ef-145">Additional Azure resources: Azure Virtual Machine (SQL Server / IPython Notebook server)</span></span>
1. <span data-ttu-id="1e6ef-146">Skapa en Azure-dator som kör SQL Server + IPython bärbar dator.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-146">Create an Azure Virtual Machine running SQL Server + IPython Notebook.</span></span>
2. <span data-ttu-id="1e6ef-147">Ladda upp data tooan Azure storage-behållare.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-147">Upload data tooan Azure storage container.</span></span>
3. <span data-ttu-id="1e6ef-148">Förbearbeta och rensa data i Azure storage-behållare med IPython bärbar dator.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-148">Pre-process and clean data in Azure storage container using IPython Notebook.</span></span>
4. <span data-ttu-id="1e6ef-149">Transformera data toocleaned, tabellform, om det behövs.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-149">Transform data toocleaned, tabular form, if needed.</span></span>
5. <span data-ttu-id="1e6ef-150">Spara tooVM-lokala datafiler (IPython bärbar dator körs på den virtuella datorn finns i lokala enheter tooVM enheter).</span><span class="sxs-lookup"><span data-stu-id="1e6ef-150">Save data tooVM-local files (IPython Notebook is running on VM, local drives refer tooVM drives).</span></span>
6. <span data-ttu-id="1e6ef-151">Läsa in data tooSQL Server-databas som körs på en virtuell dator i Azure.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-151">Load data tooSQL Server database running on an Azure VM.</span></span>
   
   <span data-ttu-id="1e6ef-152">Alternativet \#1: använder SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-152">Option \#1: Using SQL Server Management Studio.</span></span>
   
   * <span data-ttu-id="1e6ef-153">Inloggningen tooSQL Server VM</span><span class="sxs-lookup"><span data-stu-id="1e6ef-153">Login tooSQL Server VM</span></span>
   * <span data-ttu-id="1e6ef-154">Kör SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-154">Run SQL Server Management Studio.</span></span>
   * <span data-ttu-id="1e6ef-155">Skapa tabeller i databasen och målet.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-155">Create database and target tables.</span></span>
   * <span data-ttu-id="1e6ef-156">Använd en av hello bulk importera metoder tooload hello data från VM-lokala filer.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-156">Use one of hello bulk import methods tooload hello data from VM-local files.</span></span>
   
   <span data-ttu-id="1e6ef-157">Alternativet \#2: med IPython anteckningsboken – inte lämpligt för medelstora och större datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="1e6ef-157">Option \#2: Using IPython Notebook – not advisable for medium and larger datasets</span></span>
   
   <!-- -->    
   * <span data-ttu-id="1e6ef-158">Använd ODBC-anslutning sträng tooaccess SQL Server på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-158">Use ODBC connection string tooaccess SQL Server on VM.</span></span>
   * <span data-ttu-id="1e6ef-159">Skapa tabeller i databasen och målet.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-159">Create database and target tables.</span></span>
   * <span data-ttu-id="1e6ef-160">Använd en av hello bulk importera metoder tooload hello data från VM-lokala filer.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-160">Use one of hello bulk import methods tooload hello data from VM-local files.</span></span>
7. <span data-ttu-id="1e6ef-161">Utforska data, skapa funktioner efter behov.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-161">Explore data, create features as needed.</span></span> <span data-ttu-id="1e6ef-162">Observera att hello funktioner inte behöver toobe materialiserad i hello databastabeller.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-162">Note that hello features do not need toobe materialized in hello database tables.</span></span> <span data-ttu-id="1e6ef-163">Endast Observera hello nödvändiga frågan toocreate dem.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-163">Only note hello necessary query toocreate them.</span></span>
8. <span data-ttu-id="1e6ef-164">Besluta om exempel datastorleken, om det behövs och/eller önskas.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-164">Decide on a data sample size, if needed and/or desired.</span></span>
9. <span data-ttu-id="1e6ef-165">Logga in toohello [Azure Machine Learning Studio](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="1e6ef-165">Sign in toohello [Azure Machine Learning Studio](https://studio.azureml.net/).</span></span>
10. <span data-ttu-id="1e6ef-166">Läs hello data direkt från hello SQL Server med hello [importera Data] [ import-data] modul.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-166">Read hello data directly from hello SQL Server using hello [Import Data][import-data] module.</span></span> <span data-ttu-id="1e6ef-167">Klistra in hello nödvändiga frågan som extraherar fält, skapar funktioner och exempel data om det behövs direkt i hello [importera Data] [ import-data] frågan.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-167">Paste hello necessary query which extracts fields, creates features, and samples data if needed directly in hello [Import Data][import-data] query.</span></span>
11. <span data-ttu-id="1e6ef-168">Skapa flödet för Azure Machine Learning-experiment från och med infogade datauppsättning/ar.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-168">Build Azure Machine Learning experiment flow starting with ingested dataset(s).</span></span>

## <span data-ttu-id="1e6ef-169"><a name="largelocaltodb"></a>Scenariot \#5: stora datauppsättning i lokala filer, mål SQL Server i Azure VM</span><span class="sxs-lookup"><span data-stu-id="1e6ef-169"><a name="largelocaltodb"></a>Scenario \#5: Large dataset in a local files, target SQL Server in Azure VM</span></span>
![Stora lokala filer tooSQL DB i Azure][5]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a><span data-ttu-id="1e6ef-171">Ytterligare Azure-resurser: Azure-dator (SQL Server / IPython anteckningsboken server)</span><span class="sxs-lookup"><span data-stu-id="1e6ef-171">Additional Azure resources: Azure Virtual Machine (SQL Server / IPython Notebook server)</span></span>
1. <span data-ttu-id="1e6ef-172">Skapa en Azure-dator som kör SQL Server och IPython anteckningsboken server.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-172">Create an Azure Virtual Machine running SQL Server and IPython Notebook server.</span></span>
2. <span data-ttu-id="1e6ef-173">Ladda upp data tooan Azure storage-behållare.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-173">Upload data tooan Azure storage container.</span></span>
3. <span data-ttu-id="1e6ef-174">(Valfritt) Förbearbeta och rensa data.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-174">(Optional) Pre-process and clean data.</span></span>
   
   <span data-ttu-id="1e6ef-175">a.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-175">a.</span></span>  <span data-ttu-id="1e6ef-176">Förbearbeta och rensa data i IPython anteckningsbok, kommer åt data från Azure</span><span class="sxs-lookup"><span data-stu-id="1e6ef-176">Pre-process and clean data in IPython Notebook, accessing data from Azure</span></span>
   
       blobs.
   
   <span data-ttu-id="1e6ef-177">b.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-177">b.</span></span>  <span data-ttu-id="1e6ef-178">Transformera data toocleaned, tabellform, om det behövs.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-178">Transform data toocleaned, tabular form, if needed.</span></span>
   
   <span data-ttu-id="1e6ef-179">c.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-179">c.</span></span>  <span data-ttu-id="1e6ef-180">Spara tooVM-lokala datafiler (IPython bärbar dator körs på den virtuella datorn finns i lokala enheter tooVM enheter).</span><span class="sxs-lookup"><span data-stu-id="1e6ef-180">Save data tooVM-local files (IPython Notebook is running on VM, local drives refer tooVM drives).</span></span>
4. <span data-ttu-id="1e6ef-181">Läsa in data tooSQL Server-databas som körs på en virtuell dator i Azure.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-181">Load data tooSQL Server database running on an Azure VM.</span></span>
   
   <span data-ttu-id="1e6ef-182">a.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-182">a.</span></span>  <span data-ttu-id="1e6ef-183">Inloggningen tooSQL serverns virtuella dator.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-183">Login tooSQL Server VM.</span></span>
   
   <span data-ttu-id="1e6ef-184">b.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-184">b.</span></span>  <span data-ttu-id="1e6ef-185">Om data inte sparats redan hämta datafiler från Azure</span><span class="sxs-lookup"><span data-stu-id="1e6ef-185">If data not saved already, download data files from Azure</span></span>
   
       storage container toolocal-VM folder.
   
   <span data-ttu-id="1e6ef-186">c.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-186">c.</span></span>  <span data-ttu-id="1e6ef-187">Kör SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-187">Run SQL Server Management Studio.</span></span>
   
   <span data-ttu-id="1e6ef-188">d.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-188">d.</span></span>  <span data-ttu-id="1e6ef-189">Skapa tabeller i databasen och målet.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-189">Create database and target tables.</span></span>
   
   <span data-ttu-id="1e6ef-190">e.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-190">e.</span></span>  <span data-ttu-id="1e6ef-191">Använd en av hello bulk importera metoder tooload hello data.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-191">Use one of hello bulk import methods tooload hello data.</span></span>
   
   <span data-ttu-id="1e6ef-192">f.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-192">f.</span></span>  <span data-ttu-id="1e6ef-193">Om tabellkopplingar krävs, skapa index tooexpedite kopplingar.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-193">If table joins are required, create indexes tooexpedite joins.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="1e6ef-194">För snabbare överföring av stora datamängder rekommenderas det att du skapar partitionerade tabeller och massimportera hello data parallellt.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-194">For faster loading of large data sizes, it is recommended that you create partitioned tables and bulk import hello data in parallel.</span></span> <span data-ttu-id="1e6ef-195">Mer information finns i [Parallel Data importera tooSQL partitionerade tabeller](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).</span><span class="sxs-lookup"><span data-stu-id="1e6ef-195">For more information, see [Parallel Data Import tooSQL Partitioned Tables](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).</span></span>
   > 
   > 
5. <span data-ttu-id="1e6ef-196">Utforska data, skapa funktioner efter behov.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-196">Explore data, create features as needed.</span></span> <span data-ttu-id="1e6ef-197">Observera att hello funktioner inte behöver toobe materialiserad i hello databastabeller.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-197">Note that hello features do not need toobe materialized in hello database tables.</span></span> <span data-ttu-id="1e6ef-198">Endast Observera hello nödvändiga frågan toocreate dem.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-198">Only note hello necessary query toocreate them.</span></span>
6. <span data-ttu-id="1e6ef-199">Besluta om exempel datastorleken, om det behövs och/eller önskas.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-199">Decide on a data sample size, if needed and/or desired.</span></span>
7. <span data-ttu-id="1e6ef-200">Logga in toohello [Azure Machine Learning Studio](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="1e6ef-200">Sign in toohello [Azure Machine Learning Studio](https://studio.azureml.net/).</span></span>
8. <span data-ttu-id="1e6ef-201">Läs hello data direkt från hello SQL Server med hello [importera Data] [ import-data] modul.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-201">Read hello data directly from hello SQL Server using hello [Import Data][import-data] module.</span></span> <span data-ttu-id="1e6ef-202">Klistra in hello nödvändiga frågan som extraherar fält, skapar funktioner och exempel data om det behövs direkt i hello [importera Data] [ import-data] frågan.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-202">Paste hello necessary query which extracts fields, creates features, and samples data if needed directly in hello [Import Data][import-data] query.</span></span>
9. <span data-ttu-id="1e6ef-203">Enkel Azure Machine Learning experiment flödet startar med överförda datamängd</span><span class="sxs-lookup"><span data-stu-id="1e6ef-203">Simple Azure Machine Learning experiment flow starting with uploaded dataset</span></span>

## <span data-ttu-id="1e6ef-204"><a name="largedbtodb"></a>Scenariot \#6: stora dataset i en SQL Server-databas lokalt, SQL Server i en virtuell dator i Azure som mål</span><span class="sxs-lookup"><span data-stu-id="1e6ef-204"><a name="largedbtodb"></a>Scenario \#6: Large dataset in a SQL Server database on-prem, targeting SQL Server in an Azure Virtual Machine</span></span>
![Stora SQL DB lokal tooSQL DB i Azure][6]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a><span data-ttu-id="1e6ef-206">Ytterligare Azure-resurser: Azure-dator (SQL Server / IPython anteckningsboken server)</span><span class="sxs-lookup"><span data-stu-id="1e6ef-206">Additional Azure resources: Azure Virtual Machine (SQL Server / IPython Notebook server)</span></span>
1. <span data-ttu-id="1e6ef-207">Skapa en Azure-dator som kör SQL Server och IPython anteckningsboken server.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-207">Create an Azure Virtual Machine running SQL Server and IPython Notebook server.</span></span>
2. <span data-ttu-id="1e6ef-208">Använd en av hello exportera metoder tooexport hello data från SQL Server toodump filer.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-208">Use one of hello data export methods tooexport hello data from SQL Server toodump files.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="1e6ef-209">Om du väljer toomove alla data från en alternativ (snabbare) metoden toomove hello fullständig toothe SQL Server-instansen i Azure-hello lokal databas.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-209">If you decide toomove all data from hello on-prem database, an alternate (faster) method toomove hello full database toothe SQL Server instance in Azure.</span></span> <span data-ttu-id="1e6ef-210">Hoppa över hello steg tooexport data, skapa och läsa in/import data toohello måldatabasen och följ hello alternativ metod.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-210">Skip hello steps tooexport data, create database, and load/import data toohello target database and follow hello alternate method.</span></span>
   > 
   > 
3. <span data-ttu-id="1e6ef-211">Överför dump filer tooAzure lagringsbehållaren.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-211">Upload dump files tooAzure storage container.</span></span>
4. <span data-ttu-id="1e6ef-212">Läsa in hello data tooa SQL Server-databas som körs på en virtuell dator i Azure.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-212">Load hello data tooa SQL Server database running on an Azure Virtual Machine.</span></span>
   
   <span data-ttu-id="1e6ef-213">a.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-213">a.</span></span>  <span data-ttu-id="1e6ef-214">Inloggningen toohello SQL Server-VM.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-214">Login toohello SQL Server VM.</span></span>
   
   <span data-ttu-id="1e6ef-215">b.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-215">b.</span></span>  <span data-ttu-id="1e6ef-216">Hämta data från en Azure storage-behållare toohello lokala VM-mapp.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-216">Download data files from an Azure storage container toohello local-VM folder.</span></span>
   
   <span data-ttu-id="1e6ef-217">c.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-217">c.</span></span>  <span data-ttu-id="1e6ef-218">Kör SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-218">Run SQL Server Management Studio.</span></span>
   
   <span data-ttu-id="1e6ef-219">d.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-219">d.</span></span>  <span data-ttu-id="1e6ef-220">Skapa tabeller i databasen och målet.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-220">Create database and target tables.</span></span>
   
   <span data-ttu-id="1e6ef-221">e.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-221">e.</span></span>  <span data-ttu-id="1e6ef-222">Använd en av hello bulk importera metoder tooload hello data.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-222">Use one of hello bulk import methods tooload hello data.</span></span>
   
   <span data-ttu-id="1e6ef-223">f.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-223">f.</span></span>  <span data-ttu-id="1e6ef-224">Om tabellkopplingar krävs, skapa index tooexpedite kopplingar.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-224">If table joins are required, create indexes tooexpedite joins.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="1e6ef-225">Skapa partitionerade tabeller och toobulk importera hello data parallellt för snabbare överföring av stora datamängder.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-225">For faster loading of large data sizes, create partitioned tables and toobulk import hello data in parallel.</span></span> <span data-ttu-id="1e6ef-226">Mer information finns i [Parallel Data importera tooSQL partitionerade tabeller](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).</span><span class="sxs-lookup"><span data-stu-id="1e6ef-226">For more information, see [Parallel Data Import tooSQL Partitioned Tables](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).</span></span>
   > 
   > 
5. <span data-ttu-id="1e6ef-227">Utforska data, skapa funktioner efter behov.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-227">Explore data, create features as needed.</span></span> <span data-ttu-id="1e6ef-228">Observera att hello funktioner inte behöver toobe materialiserad i hello databastabeller.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-228">Note that hello features do not need toobe materialized in hello database tables.</span></span> <span data-ttu-id="1e6ef-229">Endast Observera hello nödvändiga frågan toocreate dem.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-229">Only note hello necessary query toocreate them.</span></span>
6. <span data-ttu-id="1e6ef-230">Besluta om exempel datastorleken, om det behövs och/eller önskas.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-230">Decide on a data sample size, if needed and/or desired.</span></span>
7. <span data-ttu-id="1e6ef-231">Logga in toohello [Azure Machine Learning Studio](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="1e6ef-231">Sign in toohello [Azure Machine Learning Studio](https://studio.azureml.net/).</span></span>
8. <span data-ttu-id="1e6ef-232">Läs hello data direkt från hello SQL Server med hello [importera Data] [ import-data] modul.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-232">Read hello data directly from hello SQL Server using hello [Import Data][import-data] module.</span></span> <span data-ttu-id="1e6ef-233">Klistra in hello nödvändiga frågan som extraherar fält, skapar funktioner och exempel data om det behövs direkt i hello [importera Data] [ import-data] frågan.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-233">Paste hello necessary query which extracts fields, creates features, and samples data if needed directly in hello [Import Data][import-data] query.</span></span>
9. <span data-ttu-id="1e6ef-234">Enkel Azure Machine Learning experimentera flödet som börjar med överförda dataset.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-234">Simple Azure Machine Learning experiment flow starting with uploaded dataset.</span></span>

### <a name="alternate-method-toocopy-a-full-database-from-an-on-premises--sql-server-tooazure-sql-database"></a><span data-ttu-id="1e6ef-235">Alternativ metod toocopy en fullständig databas från en lokal SQL Server-tooAzure SQL-databas</span><span class="sxs-lookup"><span data-stu-id="1e6ef-235">Alternate method toocopy a full database from an on-premises  SQL Server tooAzure SQL Database</span></span>
![Koppla från lokala DB och bifoga tooSQL DB i Azure][7]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a><span data-ttu-id="1e6ef-237">Ytterligare Azure-resurser: Azure-dator (SQL Server / IPython anteckningsboken server)</span><span class="sxs-lookup"><span data-stu-id="1e6ef-237">Additional Azure resources: Azure Virtual Machine (SQL Server / IPython Notebook server)</span></span>
<span data-ttu-id="1e6ef-238">tooreplicate hello hela SQL Server-databas i SQL Server-VM, bör du kopiera en databas från en plats/server tooanother, förutsatt att hello-databasen kan tas offline.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-238">tooreplicate hello entire SQL Server database in your SQL Server VM, you should copy a database from one location/server tooanother, assuming that hello database can be taken temporarily offline.</span></span> <span data-ttu-id="1e6ef-239">Du kan göra detta i hello SQL Server Management Studio Object Explorer, eller via hello motsvarande Transact-SQL-kommandon.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-239">You do this in hello SQL Server Management Studio Object Explorer, or using hello equivalent Transact-SQL commands.</span></span>

1. <span data-ttu-id="1e6ef-240">Koppla bort hello databasen på hello källplats.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-240">Detach hello database at hello source location.</span></span> <span data-ttu-id="1e6ef-241">Mer information finns i [koppla från en databas](https://technet.microsoft.com/library/ms191491\(v=sql.110\).aspx).</span><span class="sxs-lookup"><span data-stu-id="1e6ef-241">For more information, see [Detach a database](https://technet.microsoft.com/library/ms191491\(v=sql.110\).aspx).</span></span>
2. <span data-ttu-id="1e6ef-242">I Utforskaren i Windows eller Windows kommandotolk-fönster frånkoppla kopiera hello databasfilen eller filer och loggfilen eller filer toohello målplats på hello SQL Server-VM i Azure.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-242">In Windows Explorer or Windows Command Prompt window, copy hello detached database file or files and log file or files toohello target location on hello SQL Server VM in Azure.</span></span>
3. <span data-ttu-id="1e6ef-243">Koppla hello kopieras filerna toohello mål SQL Server-instansen.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-243">Attach hello copied files toohello target SQL Server instance.</span></span> <span data-ttu-id="1e6ef-244">Mer information finns i [ansluta en databas](https://technet.microsoft.com/library/ms190209\(v=sql.110\).aspx).</span><span class="sxs-lookup"><span data-stu-id="1e6ef-244">For more information, see [Attach a Database](https://technet.microsoft.com/library/ms190209\(v=sql.110\).aspx).</span></span>

<span data-ttu-id="1e6ef-245">[Flytta en databas med hjälp av koppla från och ansluta (Transact-SQL)](https://technet.microsoft.com/library/ms187858\(v=sql.110\).aspx)</span><span class="sxs-lookup"><span data-stu-id="1e6ef-245">[Move a Database Using Detach and Attach (Transact-SQL)](https://technet.microsoft.com/library/ms187858\(v=sql.110\).aspx)</span></span>

## <span data-ttu-id="1e6ef-246"><a name="largedbtohive"></a>Scenariot \#7: stordata i lokala filer mål Hive-databas i Azure HDInsight Hadoop-kluster</span><span class="sxs-lookup"><span data-stu-id="1e6ef-246"><a name="largedbtohive"></a>Scenario \#7: Big data in local files, target Hive database in Azure HDInsight Hadoop clusters</span></span>
![Stordata i lokala mål Hive][9]

#### <a name="additional-azure-resources-azure-hdinsight-hadoop-cluster-and-azure-virtual-machine-ipython-notebook-server"></a><span data-ttu-id="1e6ef-248">Ytterligare Azure-resurser: Azure HDInsight Hadoop-kluster och Azure-dator (IPython anteckningsboken server)</span><span class="sxs-lookup"><span data-stu-id="1e6ef-248">Additional Azure resources: Azure HDInsight Hadoop Cluster and Azure Virtual Machine (IPython Notebook server)</span></span>
1. <span data-ttu-id="1e6ef-249">Skapa en Azure-dator som kör IPython anteckningsboken server.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-249">Create an Azure Virtual Machine running IPython Notebook server.</span></span>
2. <span data-ttu-id="1e6ef-250">Skapa ett Azure HDInsight Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-250">Create an Azure HDInsight Hadoop cluster.</span></span>
3. <span data-ttu-id="1e6ef-251">(Valfritt) Förbearbeta och rensa data.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-251">(Optional) Pre-process and clean data.</span></span>
   
   <span data-ttu-id="1e6ef-252">a.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-252">a.</span></span>  <span data-ttu-id="1e6ef-253">Förbearbeta och rensa data i IPython anteckningsbok, kommer åt data från Azure</span><span class="sxs-lookup"><span data-stu-id="1e6ef-253">Pre-process and clean data in IPython Notebook, accessing data from Azure</span></span>
   
       blobs.
   
   <span data-ttu-id="1e6ef-254">b.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-254">b.</span></span>  <span data-ttu-id="1e6ef-255">Transformera data toocleaned, tabellform, om det behövs.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-255">Transform data toocleaned, tabular form, if needed.</span></span>
   
   <span data-ttu-id="1e6ef-256">c.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-256">c.</span></span>  <span data-ttu-id="1e6ef-257">Spara tooVM-lokala datafiler (IPython bärbar dator körs på den virtuella datorn finns i lokala enheter tooVM enheter).</span><span class="sxs-lookup"><span data-stu-id="1e6ef-257">Save data tooVM-local files (IPython Notebook is running on VM, local drives refer tooVM drives).</span></span>
4. <span data-ttu-id="1e6ef-258">Ladda upp data toohello standardbehållaren för hello Hadoop-kluster som valts i hello steg 2.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-258">Upload data toohello default container of hello Hadoop cluster selected in hello step 2.</span></span>
5. <span data-ttu-id="1e6ef-259">Läs in data tooHive databas i Azure HDInsight Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-259">Load data tooHive database in Azure HDInsight Hadoop cluster.</span></span>
   
   <span data-ttu-id="1e6ef-260">a.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-260">a.</span></span>  <span data-ttu-id="1e6ef-261">Logga in toohello huvudnod hello Hadoop-kluster</span><span class="sxs-lookup"><span data-stu-id="1e6ef-261">Log in toohello head node of hello Hadoop cluster</span></span>
   
   <span data-ttu-id="1e6ef-262">b.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-262">b.</span></span>  <span data-ttu-id="1e6ef-263">Öppna hello Hadoop-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-263">Open hello Hadoop Command Line.</span></span>
   
   <span data-ttu-id="1e6ef-264">c.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-264">c.</span></span>  <span data-ttu-id="1e6ef-265">Ange hello Hive rotkatalog med kommandot `cd %hive_home%\bin` i Hadoop-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-265">Enter hello Hive root directory by command `cd %hive_home%\bin` in Hadoop Command Line.</span></span>
   
   <span data-ttu-id="1e6ef-266">d.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-266">d.</span></span>  <span data-ttu-id="1e6ef-267">Kör hello Hive-frågor toocreate databasen och tabeller och Läs in data från blob storage-tooHive tabeller.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-267">Run hello Hive queries toocreate database and tables, and load data from blob storage tooHive tables.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="1e6ef-268">Om hello data är stort, kan användare skapa hello Hive-tabell med partitioner.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-268">If hello data is big, users can create hello Hive table with partitions.</span></span> <span data-ttu-id="1e6ef-269">Användare kan sedan använda en `for` loop i hello Hadoop kommandoraden på hello huvudnod tooload data till hello Hive-tabell partitioneras av partitionen.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-269">Then, users can use a `for` loop in hello Hadoop Command Line on hello head node tooload data into hello Hive table partitioned by partition.</span></span>
   > 
   > 
6. <span data-ttu-id="1e6ef-270">Utforska data och skapa funktioner som behövs i Hadoop-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-270">Explore data and create features as needed in Hadoop Command Line.</span></span> <span data-ttu-id="1e6ef-271">Observera att hello funktioner inte behöver toobe materialiserad i hello databastabeller.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-271">Note that hello features do not need toobe materialized in hello database tables.</span></span> <span data-ttu-id="1e6ef-272">Endast Observera hello nödvändiga frågan toocreate dem.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-272">Only note hello necessary query toocreate them.</span></span>
   
   <span data-ttu-id="1e6ef-273">a.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-273">a.</span></span>  <span data-ttu-id="1e6ef-274">Logga in toohello huvudnod hello Hadoop-kluster</span><span class="sxs-lookup"><span data-stu-id="1e6ef-274">Log in toohello head node of hello Hadoop cluster</span></span>
   
   <span data-ttu-id="1e6ef-275">b.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-275">b.</span></span>  <span data-ttu-id="1e6ef-276">Öppna hello Hadoop-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-276">Open hello Hadoop Command Line.</span></span>
   
   <span data-ttu-id="1e6ef-277">c.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-277">c.</span></span>  <span data-ttu-id="1e6ef-278">Ange hello Hive rotkatalog med kommandot `cd %hive_home%\bin` i Hadoop-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-278">Enter hello Hive root directory by command `cd %hive_home%\bin` in Hadoop Command Line.</span></span>
   
   <span data-ttu-id="1e6ef-279">d.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-279">d.</span></span>  <span data-ttu-id="1e6ef-280">Kör hello Hive-frågor i Hadoop kommandoraden i hello huvudnod hello Hadoop-kluster tooexplore hello data och skapa funktioner efter behov.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-280">Run hello Hive queries in Hadoop Command Line on hello head node of hello Hadoop cluster tooexplore hello data and create features as needed.</span></span>
7. <span data-ttu-id="1e6ef-281">Om det behövs och/eller önskas, exempel hello data toofit i Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-281">If needed and/or desired, sample hello data toofit in Azure Machine Learning Studio.</span></span>
8. <span data-ttu-id="1e6ef-282">Logga in toohello [Azure Machine Learning Studio](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="1e6ef-282">Sign in toohello [Azure Machine Learning Studio](https://studio.azureml.net/).</span></span>
9. <span data-ttu-id="1e6ef-283">Läsa hello data direkt från hello `Hive Queries` med hello [importera Data] [ import-data] modul.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-283">Read hello data directly from hello `Hive Queries` using hello [Import Data][import-data] module.</span></span> <span data-ttu-id="1e6ef-284">Klistra in hello nödvändiga frågan som extraherar fält, skapar funktioner och exempel data om det behövs direkt i hello [importera Data] [ import-data] frågan.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-284">Paste hello necessary query which extracts fields, creates features, and samples data if needed directly in hello [Import Data][import-data] query.</span></span>
10. <span data-ttu-id="1e6ef-285">Enkel Azure Machine Learning experimentera flödet som börjar med överförda dataset.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-285">Simple Azure Machine Learning experiment flow starting with uploaded dataset.</span></span>

## <span data-ttu-id="1e6ef-286"><a name="decisiontree"></a>Beslutsträd för val av scenariot</span><span class="sxs-lookup"><span data-stu-id="1e6ef-286"><a name="decisiontree"></a>Decision tree for scenario selection</span></span>
- - -
<span data-ttu-id="1e6ef-287">hello sammanfattar följande diagram hello-scenarier som beskrivs ovan och hello Advanced Analytics processen och tekniken val som görs ta tooeach hello specificerade scenarier.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-287">hello following diagram summarizes hello scenarios described above and hello Advanced Analytics Process and Technology choices made that take you tooeach of hello itemized scenarios.</span></span> <span data-ttu-id="1e6ef-288">Observera att databearbetning, undersökning, funktionen tekniker och sampling kan ta i en eller flera metoden/miljö--vid hello källan, mellanliggande, och/eller mål-miljöer, och kan fortsätta upprepade gånger efter behov.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-288">Note that data processing, exploration, feature engineering, and sampling may take place in one or more method/environment -- at hello source, intermediate, and/or target environments – and may proceed iteratively as needed.</span></span> <span data-ttu-id="1e6ef-289">hello diagrammet bara fungerar som en bild på några möjliga flöden och ger inte en uttömmande förteckning.</span><span class="sxs-lookup"><span data-stu-id="1e6ef-289">hello diagram only serves as an illustration of some of possible flows and does not provide an exhaustive enumeration.</span></span>

![Genomgång exempelscenarier DS process][8]

### <a name="advanced-analytics-in-action-examples"></a><span data-ttu-id="1e6ef-291">Avancerade analyser i praktiken exempel</span><span class="sxs-lookup"><span data-stu-id="1e6ef-291">Advanced Analytics in action Examples</span></span>
<span data-ttu-id="1e6ef-292">För slutpunkt till slutpunkt Azure Machine Learning genomgångar som använder Hej Advanced Analytics processen och tekniken med offentliga datauppsättningar, se:</span><span class="sxs-lookup"><span data-stu-id="1e6ef-292">For end-to-end Azure Machine Learning walkthroughs that employ hello Advanced Analytics Process and Technology using public datasets, see:</span></span>

* <span data-ttu-id="1e6ef-293">[Teamet vetenskap av data i praktiken: använder SQL Server](machine-learning-data-science-process-sql-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="1e6ef-293">[Team Data Science Process in action: using SQL Server](machine-learning-data-science-process-sql-walkthrough.md).</span></span>
* <span data-ttu-id="1e6ef-294">[Teamet vetenskap av data i praktiken: med HDInsight Hadoop-kluster](machine-learning-data-science-process-hive-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="1e6ef-294">[Team Data Science Process in action: using HDInsight Hadoop clusters](machine-learning-data-science-process-hive-walkthrough.md).</span></span>

[1]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-small-in-aml.png
[2]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-with-processing.png
[3]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-large.png
[4]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-to-db.png
[5]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-large-to-db.png
[6]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-db-to-db.png
[7]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-attach-db.png
[8]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-sample-scenarios.png
[9]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-to-hive.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
