---
title: "Identifiera avancerade analyser scenarier för Azure Machine Learning | Microsoft Docs"
description: "Välj lämplig scenarier för detta avancerad förutsägelseanalys med Team av vetenskapliga data."
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
ms.openlocfilehash: fe4f74f2e0602d13eedb6ca186480291a9a5724f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="scenarios-for-advanced-analytics-in-azure-machine-learning"></a><span data-ttu-id="4802f-103">Scenarier för avancerade analyser i Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="4802f-103">Scenarios for advanced analytics in Azure Machine Learning</span></span>
<span data-ttu-id="4802f-104">Den här artikeln beskrivs olika exempel datakällor och målscenarier som kan hanteras av den [Team Data vetenskap processen (TDSP)](data-science-process-overview.md).</span><span class="sxs-lookup"><span data-stu-id="4802f-104">This article outlines the variety of sample data sources and target scenarios that can be handled by the [Team Data Science Process (TDSP)](data-science-process-overview.md).</span></span> <span data-ttu-id="4802f-105">TDSP ger systematiskt för grupper samarbeta i att skapa intelligent program.</span><span class="sxs-lookup"><span data-stu-id="4802f-105">The TDSP provides a systematic approach for teams to collaborate on building intelligent applications.</span></span> <span data-ttu-id="4802f-106">Scenarier som presenteras här visar alternativen i arbetsflödet databearbetning som beror på dataegenskaper, källplatser och mål-databaser i Azure.</span><span class="sxs-lookup"><span data-stu-id="4802f-106">The scenarios presented here illustrate options available in the data processing workflow that depend on the data characteristics, source locations, and target repositories in Azure.</span></span>

<span data-ttu-id="4802f-107">Den **beslutsträdet** för att välja exempelscenarier som passar för dina data och målet visas i det sista avsnittet.</span><span class="sxs-lookup"><span data-stu-id="4802f-107">The **decision tree** for selecting the sample scenarios that is appropriate for your data and objective is presented in the last section.</span></span>

> [!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]
> 
> 

<span data-ttu-id="4802f-108">Följande avsnitt innehåller ett exempelscenario.</span><span class="sxs-lookup"><span data-stu-id="4802f-108">Each of the following sections presents a sample scenario.</span></span> <span data-ttu-id="4802f-109">För varje scenario ett möjligt datavetenskap eller avancerade analyser flödet och ge support för Azure-resurser visas.</span><span class="sxs-lookup"><span data-stu-id="4802f-109">For each scenario, a possible data science or advanced analytics flow and supporting Azure resources are listed.</span></span>

> [!NOTE]
> <span data-ttu-id="4802f-110">**För alla följande scenarier måste du:**
> </span><span class="sxs-lookup"><span data-stu-id="4802f-110">**For all of the following scenarios, you need to:**
</span></span><br/>
> 
> * <span data-ttu-id="4802f-111">[Skapa ett lagringskonto](../storage/common/storage-create-storage-account.md)
>   </span><span class="sxs-lookup"><span data-stu-id="4802f-111">[Create a storage account](../storage/common/storage-create-storage-account.md)
</span></span><br/>
> * [<span data-ttu-id="4802f-112">Skapa en arbetsyta för Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="4802f-112">Create an Azure Machine Learning workspace</span></span>](machine-learning-create-workspace.md)
> 
> 

## <span data-ttu-id="4802f-113"><a name="smalllocal"></a>Scenariot \#1: små till medelstora tabular datauppsättning i lokala filer</span><span class="sxs-lookup"><span data-stu-id="4802f-113"><a name="smalllocal"></a>Scenario \#1: Small to medium tabular dataset in a local files</span></span>
![Små till medelstora lokala filer][1]

#### <a name="additional-azure-resources-none"></a><span data-ttu-id="4802f-115">Ytterligare Azure-resurser: ingen</span><span class="sxs-lookup"><span data-stu-id="4802f-115">Additional Azure resources: None</span></span>
1. <span data-ttu-id="4802f-116">Logga in på den [Azure Machine Learning Studio](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="4802f-116">Sign in to the [Azure Machine Learning Studio](https://studio.azureml.net/).</span></span>
2. <span data-ttu-id="4802f-117">Överför en datamängd.</span><span class="sxs-lookup"><span data-stu-id="4802f-117">Upload a dataset.</span></span>
3. <span data-ttu-id="4802f-118">Skapa ett flöde för Azure Machine Learning-experiment som börjar med överförda datauppsättning/ar.</span><span class="sxs-lookup"><span data-stu-id="4802f-118">Build an Azure Machine Learning experiment flow starting with uploaded dataset(s).</span></span>

## <span data-ttu-id="4802f-119"><a name="smalllocalprocess"></a>Scenariot \#2: små till medelstora dataset för lokala filer som krävs</span><span class="sxs-lookup"><span data-stu-id="4802f-119"><a name="smalllocalprocess"></a>Scenario \#2: Small to medium dataset of local files that require processing</span></span>
![Små till medelstora lokala filer med bearbetning][2]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a><span data-ttu-id="4802f-121">Ytterligare Azure-resurser: Azure-dator (IPython anteckningsboken server)</span><span class="sxs-lookup"><span data-stu-id="4802f-121">Additional Azure resources: Azure Virtual Machine (IPython Notebook server)</span></span>
1. <span data-ttu-id="4802f-122">Skapa en Azure-dator som kör IPython bärbar dator.</span><span class="sxs-lookup"><span data-stu-id="4802f-122">Create an Azure Virtual Machine running IPython Notebook.</span></span>
2. <span data-ttu-id="4802f-123">Överföra data till en Azure storage-behållare.</span><span class="sxs-lookup"><span data-stu-id="4802f-123">Upload data to an Azure storage container.</span></span>
3. <span data-ttu-id="4802f-124">Förbearbeta och rensa data i IPython anteckningsbok, kommer åt data från Azure storage-behållare.</span><span class="sxs-lookup"><span data-stu-id="4802f-124">Pre-process and clean data in IPython Notebook, accessing data from Azure storage container.</span></span>
4. <span data-ttu-id="4802f-125">Transformera data till rensad tabellform.</span><span class="sxs-lookup"><span data-stu-id="4802f-125">Transform data to cleaned, tabular form.</span></span>
5. <span data-ttu-id="4802f-126">Spara transformerade data i Azure BLOB.</span><span class="sxs-lookup"><span data-stu-id="4802f-126">Save transformed data in Azure blobs.</span></span>
6. <span data-ttu-id="4802f-127">Logga in på den [Azure Machine Learning Studio](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="4802f-127">Sign in to the [Azure Machine Learning Studio](https://studio.azureml.net/).</span></span>
7. <span data-ttu-id="4802f-128">Läsa data från Azure blobar med hjälp av den [importera Data] [ import-data] modul.</span><span class="sxs-lookup"><span data-stu-id="4802f-128">Read the data from Azure blobs using the [Import Data][import-data] module.</span></span>
8. <span data-ttu-id="4802f-129">Skapa ett flöde för Azure Machine Learning-experiment som börjar med infogade datauppsättning/ar.</span><span class="sxs-lookup"><span data-stu-id="4802f-129">Build an Azure Machine Learning experiment flow starting with ingested dataset(s).</span></span>

## <span data-ttu-id="4802f-130"><a name="largelocal"></a>Scenariot \#3: stora dataset av lokala filer, riktad på Azure-BLOB</span><span class="sxs-lookup"><span data-stu-id="4802f-130"><a name="largelocal"></a>Scenario \#3: Large dataset of local files, targeting Azure Blobs</span></span>
![Stora lokala filer][3]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a><span data-ttu-id="4802f-132">Ytterligare Azure-resurser: Azure-dator (IPython anteckningsboken server)</span><span class="sxs-lookup"><span data-stu-id="4802f-132">Additional Azure resources: Azure Virtual Machine (IPython Notebook server)</span></span>
1. <span data-ttu-id="4802f-133">Skapa en Azure-dator som kör IPython bärbar dator.</span><span class="sxs-lookup"><span data-stu-id="4802f-133">Create an Azure Virtual Machine running IPython Notebook.</span></span>
2. <span data-ttu-id="4802f-134">Överföra data till en Azure storage-behållare.</span><span class="sxs-lookup"><span data-stu-id="4802f-134">Upload data to an Azure storage container.</span></span>
3. <span data-ttu-id="4802f-135">Förbearbeta och rensa data i IPython anteckningsbok, kommer åt data från Azure BLOB.</span><span class="sxs-lookup"><span data-stu-id="4802f-135">Pre-process and clean data in IPython Notebook, accessing data from Azure blobs.</span></span>
4. <span data-ttu-id="4802f-136">Transformera data till rensad tabellform, om det behövs.</span><span class="sxs-lookup"><span data-stu-id="4802f-136">Transform data to cleaned, tabular form, if needed.</span></span>
5. <span data-ttu-id="4802f-137">Utforska data och skapa funktioner efter behov.</span><span class="sxs-lookup"><span data-stu-id="4802f-137">Explore data, and create features as needed.</span></span>
6. <span data-ttu-id="4802f-138">Extrahera ett datasampel för små till medelstora.</span><span class="sxs-lookup"><span data-stu-id="4802f-138">Extract a small-to-medium data sample.</span></span>
7. <span data-ttu-id="4802f-139">Spara samplade data i Azure BLOB.</span><span class="sxs-lookup"><span data-stu-id="4802f-139">Save the sampled data in Azure blobs.</span></span>
8. <span data-ttu-id="4802f-140">Logga in på den [Azure Machine Learning Studio](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="4802f-140">Sign in to the [Azure Machine Learning Studio](https://studio.azureml.net/).</span></span>
9. <span data-ttu-id="4802f-141">Läsa data från Azure blobar med hjälp av den [importera Data] [ import-data] modul.</span><span class="sxs-lookup"><span data-stu-id="4802f-141">Read the data from Azure blobs using the [Import Data][import-data] module.</span></span>
10. <span data-ttu-id="4802f-142">Skapa flödet för Azure Machine Learning-experiment från och med infogade datauppsättning/ar.</span><span class="sxs-lookup"><span data-stu-id="4802f-142">Build Azure Machine Learning experiment flow starting with ingested dataset(s).</span></span>

## <span data-ttu-id="4802f-143"><a name="smalllocaltodb"></a>Scenariot \#4: liten medelhög DataSet av lokala filer, SQL Server i en virtuell dator i Azure som mål</span><span class="sxs-lookup"><span data-stu-id="4802f-143"><a name="smalllocaltodb"></a>Scenario \#4: Small to medium dataset of local files, targeting SQL Server in an Azure Virtual Machine</span></span>
![Små till medelstora lokala filer till SQL DB i Azure][4]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a><span data-ttu-id="4802f-145">Ytterligare Azure-resurser: Azure-dator (SQL Server / IPython anteckningsboken server)</span><span class="sxs-lookup"><span data-stu-id="4802f-145">Additional Azure resources: Azure Virtual Machine (SQL Server / IPython Notebook server)</span></span>
1. <span data-ttu-id="4802f-146">Skapa en Azure-dator som kör SQL Server + IPython bärbar dator.</span><span class="sxs-lookup"><span data-stu-id="4802f-146">Create an Azure Virtual Machine running SQL Server + IPython Notebook.</span></span>
2. <span data-ttu-id="4802f-147">Överföra data till en Azure storage-behållare.</span><span class="sxs-lookup"><span data-stu-id="4802f-147">Upload data to an Azure storage container.</span></span>
3. <span data-ttu-id="4802f-148">Förbearbeta och rensa data i Azure storage-behållare med IPython bärbar dator.</span><span class="sxs-lookup"><span data-stu-id="4802f-148">Pre-process and clean data in Azure storage container using IPython Notebook.</span></span>
4. <span data-ttu-id="4802f-149">Transformera data till rensad tabellform, om det behövs.</span><span class="sxs-lookup"><span data-stu-id="4802f-149">Transform data to cleaned, tabular form, if needed.</span></span>
5. <span data-ttu-id="4802f-150">Sparar data till VM-lokala filer (IPython bärbar dator körs på den virtuella datorn finns i lokala enheter för VM-enheter).</span><span class="sxs-lookup"><span data-stu-id="4802f-150">Save data to VM-local files (IPython Notebook is running on VM, local drives refer to VM drives).</span></span>
6. <span data-ttu-id="4802f-151">Läsa in data till SQL Server-databas som körs på en virtuell dator i Azure.</span><span class="sxs-lookup"><span data-stu-id="4802f-151">Load data to SQL Server database running on an Azure VM.</span></span>
   
   <span data-ttu-id="4802f-152">Alternativet \#1: använder SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="4802f-152">Option \#1: Using SQL Server Management Studio.</span></span>
   
   * <span data-ttu-id="4802f-153">Logga in på SQLServer-dator</span><span class="sxs-lookup"><span data-stu-id="4802f-153">Login to SQL Server VM</span></span>
   * <span data-ttu-id="4802f-154">Kör SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="4802f-154">Run SQL Server Management Studio.</span></span>
   * <span data-ttu-id="4802f-155">Skapa tabeller i databasen och målet.</span><span class="sxs-lookup"><span data-stu-id="4802f-155">Create database and target tables.</span></span>
   * <span data-ttu-id="4802f-156">Använd en av flesta importera metoder för att läsa in data från VM-lokala filer.</span><span class="sxs-lookup"><span data-stu-id="4802f-156">Use one of the bulk import methods to load the data from VM-local files.</span></span>
   
   <span data-ttu-id="4802f-157">Alternativet \#2: med IPython anteckningsboken – inte lämpligt för medelstora och större datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="4802f-157">Option \#2: Using IPython Notebook – not advisable for medium and larger datasets</span></span>
   
   <!-- -->    
   * <span data-ttu-id="4802f-158">Använd ODBC-anslutningssträng för att få åtkomst till SQL Server på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="4802f-158">Use ODBC connection string to access SQL Server on VM.</span></span>
   * <span data-ttu-id="4802f-159">Skapa tabeller i databasen och målet.</span><span class="sxs-lookup"><span data-stu-id="4802f-159">Create database and target tables.</span></span>
   * <span data-ttu-id="4802f-160">Använd en av flesta importera metoder för att läsa in data från VM-lokala filer.</span><span class="sxs-lookup"><span data-stu-id="4802f-160">Use one of the bulk import methods to load the data from VM-local files.</span></span>
7. <span data-ttu-id="4802f-161">Utforska data, skapa funktioner efter behov.</span><span class="sxs-lookup"><span data-stu-id="4802f-161">Explore data, create features as needed.</span></span> <span data-ttu-id="4802f-162">Observera att funktionerna för inte behöver materialiseras i databastabeller.</span><span class="sxs-lookup"><span data-stu-id="4802f-162">Note that the features do not need to be materialized in the database tables.</span></span> <span data-ttu-id="4802f-163">Observera endast nödvändiga frågan för att skapa dem.</span><span class="sxs-lookup"><span data-stu-id="4802f-163">Only note the necessary query to create them.</span></span>
8. <span data-ttu-id="4802f-164">Besluta om exempel datastorleken, om det behövs och/eller önskas.</span><span class="sxs-lookup"><span data-stu-id="4802f-164">Decide on a data sample size, if needed and/or desired.</span></span>
9. <span data-ttu-id="4802f-165">Logga in på den [Azure Machine Learning Studio](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="4802f-165">Sign in to the [Azure Machine Learning Studio](https://studio.azureml.net/).</span></span>
10. <span data-ttu-id="4802f-166">Läsa data direkt från en SQL Server med hjälp av den [importera Data] [ import-data] modul.</span><span class="sxs-lookup"><span data-stu-id="4802f-166">Read the data directly from the SQL Server using the [Import Data][import-data] module.</span></span> <span data-ttu-id="4802f-167">Klistra in den nödvändiga fråga som extraherar fält, skapar funktioner och exempel data om det behövs direkt i den [importera Data] [ import-data] frågan.</span><span class="sxs-lookup"><span data-stu-id="4802f-167">Paste the necessary query which extracts fields, creates features, and samples data if needed directly in the [Import Data][import-data] query.</span></span>
11. <span data-ttu-id="4802f-168">Skapa flödet för Azure Machine Learning-experiment från och med infogade datauppsättning/ar.</span><span class="sxs-lookup"><span data-stu-id="4802f-168">Build Azure Machine Learning experiment flow starting with ingested dataset(s).</span></span>

## <span data-ttu-id="4802f-169"><a name="largelocaltodb"></a>Scenariot \#5: stora datauppsättning i lokala filer, mål SQL Server i Azure VM</span><span class="sxs-lookup"><span data-stu-id="4802f-169"><a name="largelocaltodb"></a>Scenario \#5: Large dataset in a local files, target SQL Server in Azure VM</span></span>
![Stora lokala filer till SQL DB i Azure][5]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a><span data-ttu-id="4802f-171">Ytterligare Azure-resurser: Azure-dator (SQL Server / IPython anteckningsboken server)</span><span class="sxs-lookup"><span data-stu-id="4802f-171">Additional Azure resources: Azure Virtual Machine (SQL Server / IPython Notebook server)</span></span>
1. <span data-ttu-id="4802f-172">Skapa en Azure-dator som kör SQL Server och IPython anteckningsboken server.</span><span class="sxs-lookup"><span data-stu-id="4802f-172">Create an Azure Virtual Machine running SQL Server and IPython Notebook server.</span></span>
2. <span data-ttu-id="4802f-173">Överföra data till en Azure storage-behållare.</span><span class="sxs-lookup"><span data-stu-id="4802f-173">Upload data to an Azure storage container.</span></span>
3. <span data-ttu-id="4802f-174">(Valfritt) Förbearbeta och rensa data.</span><span class="sxs-lookup"><span data-stu-id="4802f-174">(Optional) Pre-process and clean data.</span></span>
   
   <span data-ttu-id="4802f-175">a.</span><span class="sxs-lookup"><span data-stu-id="4802f-175">a.</span></span>  <span data-ttu-id="4802f-176">Förbearbeta och rensa data i IPython anteckningsbok, kommer åt data från Azure</span><span class="sxs-lookup"><span data-stu-id="4802f-176">Pre-process and clean data in IPython Notebook, accessing data from Azure</span></span>
   
       blobs.
   
   <span data-ttu-id="4802f-177">b.</span><span class="sxs-lookup"><span data-stu-id="4802f-177">b.</span></span>  <span data-ttu-id="4802f-178">Transformera data till rensad tabellform, om det behövs.</span><span class="sxs-lookup"><span data-stu-id="4802f-178">Transform data to cleaned, tabular form, if needed.</span></span>
   
   <span data-ttu-id="4802f-179">c.</span><span class="sxs-lookup"><span data-stu-id="4802f-179">c.</span></span>  <span data-ttu-id="4802f-180">Sparar data till VM-lokala filer (IPython bärbar dator körs på den virtuella datorn finns i lokala enheter för VM-enheter).</span><span class="sxs-lookup"><span data-stu-id="4802f-180">Save data to VM-local files (IPython Notebook is running on VM, local drives refer to VM drives).</span></span>
4. <span data-ttu-id="4802f-181">Läsa in data till SQL Server-databas som körs på en virtuell dator i Azure.</span><span class="sxs-lookup"><span data-stu-id="4802f-181">Load data to SQL Server database running on an Azure VM.</span></span>
   
   <span data-ttu-id="4802f-182">a.</span><span class="sxs-lookup"><span data-stu-id="4802f-182">a.</span></span>  <span data-ttu-id="4802f-183">Logga in på SQLServer-dator.</span><span class="sxs-lookup"><span data-stu-id="4802f-183">Login to SQL Server VM.</span></span>
   
   <span data-ttu-id="4802f-184">b.</span><span class="sxs-lookup"><span data-stu-id="4802f-184">b.</span></span>  <span data-ttu-id="4802f-185">Om data inte sparats redan hämta datafiler från Azure</span><span class="sxs-lookup"><span data-stu-id="4802f-185">If data not saved already, download data files from Azure</span></span>
   
       storage container to local-VM folder.
   
   <span data-ttu-id="4802f-186">c.</span><span class="sxs-lookup"><span data-stu-id="4802f-186">c.</span></span>  <span data-ttu-id="4802f-187">Kör SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="4802f-187">Run SQL Server Management Studio.</span></span>
   
   <span data-ttu-id="4802f-188">d.</span><span class="sxs-lookup"><span data-stu-id="4802f-188">d.</span></span>  <span data-ttu-id="4802f-189">Skapa tabeller i databasen och målet.</span><span class="sxs-lookup"><span data-stu-id="4802f-189">Create database and target tables.</span></span>
   
   <span data-ttu-id="4802f-190">e.</span><span class="sxs-lookup"><span data-stu-id="4802f-190">e.</span></span>  <span data-ttu-id="4802f-191">Använd en av flesta importera metoder för att läsa in data.</span><span class="sxs-lookup"><span data-stu-id="4802f-191">Use one of the bulk import methods to load the data.</span></span>
   
   <span data-ttu-id="4802f-192">f.</span><span class="sxs-lookup"><span data-stu-id="4802f-192">f.</span></span>  <span data-ttu-id="4802f-193">Om tabellkopplingar krävs, skapa index för att påskynda kopplingar.</span><span class="sxs-lookup"><span data-stu-id="4802f-193">If table joins are required, create indexes to expedite joins.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="4802f-194">Det rekommenderas för snabbare överföring av stora datamängder som du skapar partitionerade tabeller och massimportera data parallellt.</span><span class="sxs-lookup"><span data-stu-id="4802f-194">For faster loading of large data sizes, it is recommended that you create partitioned tables and bulk import the data in parallel.</span></span> <span data-ttu-id="4802f-195">Mer information finns i [parallella Import av Data till SQL-partitionerade tabeller](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).</span><span class="sxs-lookup"><span data-stu-id="4802f-195">For more information, see [Parallel Data Import to SQL Partitioned Tables](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).</span></span>
   > 
   > 
5. <span data-ttu-id="4802f-196">Utforska data, skapa funktioner efter behov.</span><span class="sxs-lookup"><span data-stu-id="4802f-196">Explore data, create features as needed.</span></span> <span data-ttu-id="4802f-197">Observera att funktionerna för inte behöver materialiseras i databastabeller.</span><span class="sxs-lookup"><span data-stu-id="4802f-197">Note that the features do not need to be materialized in the database tables.</span></span> <span data-ttu-id="4802f-198">Observera endast nödvändiga frågan för att skapa dem.</span><span class="sxs-lookup"><span data-stu-id="4802f-198">Only note the necessary query to create them.</span></span>
6. <span data-ttu-id="4802f-199">Besluta om exempel datastorleken, om det behövs och/eller önskas.</span><span class="sxs-lookup"><span data-stu-id="4802f-199">Decide on a data sample size, if needed and/or desired.</span></span>
7. <span data-ttu-id="4802f-200">Logga in på den [Azure Machine Learning Studio](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="4802f-200">Sign in to the [Azure Machine Learning Studio](https://studio.azureml.net/).</span></span>
8. <span data-ttu-id="4802f-201">Läsa data direkt från en SQL Server med hjälp av den [importera Data] [ import-data] modul.</span><span class="sxs-lookup"><span data-stu-id="4802f-201">Read the data directly from the SQL Server using the [Import Data][import-data] module.</span></span> <span data-ttu-id="4802f-202">Klistra in den nödvändiga fråga som extraherar fält, skapar funktioner och exempel data om det behövs direkt i den [importera Data] [ import-data] frågan.</span><span class="sxs-lookup"><span data-stu-id="4802f-202">Paste the necessary query which extracts fields, creates features, and samples data if needed directly in the [Import Data][import-data] query.</span></span>
9. <span data-ttu-id="4802f-203">Enkel Azure Machine Learning experiment flödet startar med överförda datamängd</span><span class="sxs-lookup"><span data-stu-id="4802f-203">Simple Azure Machine Learning experiment flow starting with uploaded dataset</span></span>

## <span data-ttu-id="4802f-204"><a name="largedbtodb"></a>Scenariot \#6: stora dataset i en SQL Server-databas lokalt, SQL Server i en virtuell dator i Azure som mål</span><span class="sxs-lookup"><span data-stu-id="4802f-204"><a name="largedbtodb"></a>Scenario \#6: Large dataset in a SQL Server database on-prem, targeting SQL Server in an Azure Virtual Machine</span></span>
![Stora SQL DB lokalt till SQL DB i Azure][6]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a><span data-ttu-id="4802f-206">Ytterligare Azure-resurser: Azure-dator (SQL Server / IPython anteckningsboken server)</span><span class="sxs-lookup"><span data-stu-id="4802f-206">Additional Azure resources: Azure Virtual Machine (SQL Server / IPython Notebook server)</span></span>
1. <span data-ttu-id="4802f-207">Skapa en Azure-dator som kör SQL Server och IPython anteckningsboken server.</span><span class="sxs-lookup"><span data-stu-id="4802f-207">Create an Azure Virtual Machine running SQL Server and IPython Notebook server.</span></span>
2. <span data-ttu-id="4802f-208">Använd en av data exportera metoder för att exportera data från SQL Server till dumpfiler.</span><span class="sxs-lookup"><span data-stu-id="4802f-208">Use one of the data export methods to export the data from SQL Server to dump files.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="4802f-209">Om du vill flytta alla data från lokala databas, en alternativ (snabbare) metod att flytta hela databasen till SQL Server-instansen i Azure.</span><span class="sxs-lookup"><span data-stu-id="4802f-209">If you decide to move all data from the on-prem database, an alternate (faster) method to move the full database to the SQL Server instance in Azure.</span></span> <span data-ttu-id="4802f-210">Hoppa över stegen för att exportera data, skapa databas, och Läs in/importera data till måldatabasen och följer alternativ metod.</span><span class="sxs-lookup"><span data-stu-id="4802f-210">Skip the steps to export data, create database, and load/import data to the target database and follow the alternate method.</span></span>
   > 
   > 
3. <span data-ttu-id="4802f-211">Överför filer med felsökningsdumpar till Azure storage-behållare.</span><span class="sxs-lookup"><span data-stu-id="4802f-211">Upload dump files to Azure storage container.</span></span>
4. <span data-ttu-id="4802f-212">Läsa in data till en SQL Server-databas som körs på en virtuell dator i Azure.</span><span class="sxs-lookup"><span data-stu-id="4802f-212">Load the data to a SQL Server database running on an Azure Virtual Machine.</span></span>
   
   <span data-ttu-id="4802f-213">a.</span><span class="sxs-lookup"><span data-stu-id="4802f-213">a.</span></span>  <span data-ttu-id="4802f-214">Logga in på SQLServer-dator.</span><span class="sxs-lookup"><span data-stu-id="4802f-214">Login to the SQL Server VM.</span></span>
   
   <span data-ttu-id="4802f-215">b.</span><span class="sxs-lookup"><span data-stu-id="4802f-215">b.</span></span>  <span data-ttu-id="4802f-216">Hämta data från en Azure storage-behållare till den lokala VM-mappen.</span><span class="sxs-lookup"><span data-stu-id="4802f-216">Download data files from an Azure storage container to the local-VM folder.</span></span>
   
   <span data-ttu-id="4802f-217">c.</span><span class="sxs-lookup"><span data-stu-id="4802f-217">c.</span></span>  <span data-ttu-id="4802f-218">Kör SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="4802f-218">Run SQL Server Management Studio.</span></span>
   
   <span data-ttu-id="4802f-219">d.</span><span class="sxs-lookup"><span data-stu-id="4802f-219">d.</span></span>  <span data-ttu-id="4802f-220">Skapa tabeller i databasen och målet.</span><span class="sxs-lookup"><span data-stu-id="4802f-220">Create database and target tables.</span></span>
   
   <span data-ttu-id="4802f-221">e.</span><span class="sxs-lookup"><span data-stu-id="4802f-221">e.</span></span>  <span data-ttu-id="4802f-222">Använd en av flesta importera metoder för att läsa in data.</span><span class="sxs-lookup"><span data-stu-id="4802f-222">Use one of the bulk import methods to load the data.</span></span>
   
   <span data-ttu-id="4802f-223">f.</span><span class="sxs-lookup"><span data-stu-id="4802f-223">f.</span></span>  <span data-ttu-id="4802f-224">Om tabellkopplingar krävs, skapa index för att påskynda kopplingar.</span><span class="sxs-lookup"><span data-stu-id="4802f-224">If table joins are required, create indexes to expedite joins.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="4802f-225">Importera data parallellt för snabbare överföring av stora datamängder, skapa partitionerade tabeller och omfång.</span><span class="sxs-lookup"><span data-stu-id="4802f-225">For faster loading of large data sizes, create partitioned tables and to bulk import the data in parallel.</span></span> <span data-ttu-id="4802f-226">Mer information finns i [parallella Import av Data till SQL-partitionerade tabeller](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).</span><span class="sxs-lookup"><span data-stu-id="4802f-226">For more information, see [Parallel Data Import to SQL Partitioned Tables](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).</span></span>
   > 
   > 
5. <span data-ttu-id="4802f-227">Utforska data, skapa funktioner efter behov.</span><span class="sxs-lookup"><span data-stu-id="4802f-227">Explore data, create features as needed.</span></span> <span data-ttu-id="4802f-228">Observera att funktionerna för inte behöver materialiseras i databastabeller.</span><span class="sxs-lookup"><span data-stu-id="4802f-228">Note that the features do not need to be materialized in the database tables.</span></span> <span data-ttu-id="4802f-229">Observera endast nödvändiga frågan för att skapa dem.</span><span class="sxs-lookup"><span data-stu-id="4802f-229">Only note the necessary query to create them.</span></span>
6. <span data-ttu-id="4802f-230">Besluta om exempel datastorleken, om det behövs och/eller önskas.</span><span class="sxs-lookup"><span data-stu-id="4802f-230">Decide on a data sample size, if needed and/or desired.</span></span>
7. <span data-ttu-id="4802f-231">Logga in på den [Azure Machine Learning Studio](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="4802f-231">Sign in to the [Azure Machine Learning Studio](https://studio.azureml.net/).</span></span>
8. <span data-ttu-id="4802f-232">Läsa data direkt från en SQL Server med hjälp av den [importera Data] [ import-data] modul.</span><span class="sxs-lookup"><span data-stu-id="4802f-232">Read the data directly from the SQL Server using the [Import Data][import-data] module.</span></span> <span data-ttu-id="4802f-233">Klistra in den nödvändiga fråga som extraherar fält, skapar funktioner och exempel data om det behövs direkt i den [importera Data] [ import-data] frågan.</span><span class="sxs-lookup"><span data-stu-id="4802f-233">Paste the necessary query which extracts fields, creates features, and samples data if needed directly in the [Import Data][import-data] query.</span></span>
9. <span data-ttu-id="4802f-234">Enkel Azure Machine Learning experimentera flödet som börjar med överförda dataset.</span><span class="sxs-lookup"><span data-stu-id="4802f-234">Simple Azure Machine Learning experiment flow starting with uploaded dataset.</span></span>

### <a name="alternate-method-to-copy-a-full-database-from-an-on-premises--sql-server-to-azure-sql-database"></a><span data-ttu-id="4802f-235">Alternativ metod för att kopiera en fullständig databas från en lokal SQL Server till Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="4802f-235">Alternate method to copy a full database from an on-premises  SQL Server to Azure SQL Database</span></span>
![Koppla från lokala DB och ansluta till SQL-databas i Azure][7]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a><span data-ttu-id="4802f-237">Ytterligare Azure-resurser: Azure-dator (SQL Server / IPython anteckningsboken server)</span><span class="sxs-lookup"><span data-stu-id="4802f-237">Additional Azure resources: Azure Virtual Machine (SQL Server / IPython Notebook server)</span></span>
<span data-ttu-id="4802f-238">Om du vill replikera hela SQL Server-databasen i SQL Server-VM, bör du kopiera en databas från en plats-server till en annan, förutsatt att databasen kan tas offline.</span><span class="sxs-lookup"><span data-stu-id="4802f-238">To replicate the entire SQL Server database in your SQL Server VM, you should copy a database from one location/server to another, assuming that the database can be taken temporarily offline.</span></span> <span data-ttu-id="4802f-239">Detta gör du i SQL Server Management Studio Object Explorer eller med hjälp av motsvarande Transact-SQL-kommandon.</span><span class="sxs-lookup"><span data-stu-id="4802f-239">You do this in the SQL Server Management Studio Object Explorer, or using the equivalent Transact-SQL commands.</span></span>

1. <span data-ttu-id="4802f-240">Koppla från databasen på källplatsen.</span><span class="sxs-lookup"><span data-stu-id="4802f-240">Detach the database at the source location.</span></span> <span data-ttu-id="4802f-241">Mer information finns i [koppla från en databas](https://technet.microsoft.com/library/ms191491\(v=sql.110\).aspx).</span><span class="sxs-lookup"><span data-stu-id="4802f-241">For more information, see [Detach a database](https://technet.microsoft.com/library/ms191491\(v=sql.110\).aspx).</span></span>
2. <span data-ttu-id="4802f-242">Kopiera frånkopplad databasfilen eller filerna och loggfilen eller filer i Utforskaren i Windows eller Windows kommandotolk-fönster till platsen på SQL Server-VM i Azure.</span><span class="sxs-lookup"><span data-stu-id="4802f-242">In Windows Explorer or Windows Command Prompt window, copy the detached database file or files and log file or files to the target location on the SQL Server VM in Azure.</span></span>
3. <span data-ttu-id="4802f-243">Bifoga filerna till mål SQL Server-instansen.</span><span class="sxs-lookup"><span data-stu-id="4802f-243">Attach the copied files to the target SQL Server instance.</span></span> <span data-ttu-id="4802f-244">Mer information finns i [ansluta en databas](https://technet.microsoft.com/library/ms190209\(v=sql.110\).aspx).</span><span class="sxs-lookup"><span data-stu-id="4802f-244">For more information, see [Attach a Database](https://technet.microsoft.com/library/ms190209\(v=sql.110\).aspx).</span></span>

<span data-ttu-id="4802f-245">[Flytta en databas med hjälp av koppla från och ansluta (Transact-SQL)](https://technet.microsoft.com/library/ms187858\(v=sql.110\).aspx)</span><span class="sxs-lookup"><span data-stu-id="4802f-245">[Move a Database Using Detach and Attach (Transact-SQL)](https://technet.microsoft.com/library/ms187858\(v=sql.110\).aspx)</span></span>

## <span data-ttu-id="4802f-246"><a name="largedbtohive"></a>Scenariot \#7: stordata i lokala filer mål Hive-databas i Azure HDInsight Hadoop-kluster</span><span class="sxs-lookup"><span data-stu-id="4802f-246"><a name="largedbtohive"></a>Scenario \#7: Big data in local files, target Hive database in Azure HDInsight Hadoop clusters</span></span>
![Stordata i lokala mål Hive][9]

#### <a name="additional-azure-resources-azure-hdinsight-hadoop-cluster-and-azure-virtual-machine-ipython-notebook-server"></a><span data-ttu-id="4802f-248">Ytterligare Azure-resurser: Azure HDInsight Hadoop-kluster och Azure-dator (IPython anteckningsboken server)</span><span class="sxs-lookup"><span data-stu-id="4802f-248">Additional Azure resources: Azure HDInsight Hadoop Cluster and Azure Virtual Machine (IPython Notebook server)</span></span>
1. <span data-ttu-id="4802f-249">Skapa en Azure-dator som kör IPython anteckningsboken server.</span><span class="sxs-lookup"><span data-stu-id="4802f-249">Create an Azure Virtual Machine running IPython Notebook server.</span></span>
2. <span data-ttu-id="4802f-250">Skapa ett Azure HDInsight Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="4802f-250">Create an Azure HDInsight Hadoop cluster.</span></span>
3. <span data-ttu-id="4802f-251">(Valfritt) Förbearbeta och rensa data.</span><span class="sxs-lookup"><span data-stu-id="4802f-251">(Optional) Pre-process and clean data.</span></span>
   
   <span data-ttu-id="4802f-252">a.</span><span class="sxs-lookup"><span data-stu-id="4802f-252">a.</span></span>  <span data-ttu-id="4802f-253">Förbearbeta och rensa data i IPython anteckningsbok, kommer åt data från Azure</span><span class="sxs-lookup"><span data-stu-id="4802f-253">Pre-process and clean data in IPython Notebook, accessing data from Azure</span></span>
   
       blobs.
   
   <span data-ttu-id="4802f-254">b.</span><span class="sxs-lookup"><span data-stu-id="4802f-254">b.</span></span>  <span data-ttu-id="4802f-255">Transformera data till rensad tabellform, om det behövs.</span><span class="sxs-lookup"><span data-stu-id="4802f-255">Transform data to cleaned, tabular form, if needed.</span></span>
   
   <span data-ttu-id="4802f-256">c.</span><span class="sxs-lookup"><span data-stu-id="4802f-256">c.</span></span>  <span data-ttu-id="4802f-257">Sparar data till VM-lokala filer (IPython bärbar dator körs på den virtuella datorn finns i lokala enheter för VM-enheter).</span><span class="sxs-lookup"><span data-stu-id="4802f-257">Save data to VM-local files (IPython Notebook is running on VM, local drives refer to VM drives).</span></span>
4. <span data-ttu-id="4802f-258">Överföra data till standardbehållaren för det Hadoop-kluster som är markerade i steg 2.</span><span class="sxs-lookup"><span data-stu-id="4802f-258">Upload data to the default container of the Hadoop cluster selected in the step 2.</span></span>
5. <span data-ttu-id="4802f-259">Läsa in data till Hive-databas i Azure HDInsight Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="4802f-259">Load data to Hive database in Azure HDInsight Hadoop cluster.</span></span>
   
   <span data-ttu-id="4802f-260">a.</span><span class="sxs-lookup"><span data-stu-id="4802f-260">a.</span></span>  <span data-ttu-id="4802f-261">Logga in till huvudnod i Hadoop-kluster</span><span class="sxs-lookup"><span data-stu-id="4802f-261">Log in to the head node of the Hadoop cluster</span></span>
   
   <span data-ttu-id="4802f-262">b.</span><span class="sxs-lookup"><span data-stu-id="4802f-262">b.</span></span>  <span data-ttu-id="4802f-263">Öppna Hadoop-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="4802f-263">Open the Hadoop Command Line.</span></span>
   
   <span data-ttu-id="4802f-264">c.</span><span class="sxs-lookup"><span data-stu-id="4802f-264">c.</span></span>  <span data-ttu-id="4802f-265">Ange Hive rotkatalogen av kommandot `cd %hive_home%\bin` i Hadoop-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="4802f-265">Enter the Hive root directory by command `cd %hive_home%\bin` in Hadoop Command Line.</span></span>
   
   <span data-ttu-id="4802f-266">d.</span><span class="sxs-lookup"><span data-stu-id="4802f-266">d.</span></span>  <span data-ttu-id="4802f-267">Köra Hive-frågor för att skapa databasen och tabeller och läsa data från blob storage till Hive-tabeller.</span><span class="sxs-lookup"><span data-stu-id="4802f-267">Run the Hive queries to create database and tables, and load data from blob storage to Hive tables.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="4802f-268">Om data är stor, kan användare skapa Hive-tabell med partitioner.</span><span class="sxs-lookup"><span data-stu-id="4802f-268">If the data is big, users can create the Hive table with partitions.</span></span> <span data-ttu-id="4802f-269">Användare kan sedan använda en `for` loop i Hadoop kommandoraden på huvudnoden att läsa in data i Hive-tabell som har partitionerats med partitionen.</span><span class="sxs-lookup"><span data-stu-id="4802f-269">Then, users can use a `for` loop in the Hadoop Command Line on the head node to load data into the Hive table partitioned by partition.</span></span>
   > 
   > 
6. <span data-ttu-id="4802f-270">Utforska data och skapa funktioner som behövs i Hadoop-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="4802f-270">Explore data and create features as needed in Hadoop Command Line.</span></span> <span data-ttu-id="4802f-271">Observera att funktionerna för inte behöver materialiseras i databastabeller.</span><span class="sxs-lookup"><span data-stu-id="4802f-271">Note that the features do not need to be materialized in the database tables.</span></span> <span data-ttu-id="4802f-272">Observera endast nödvändiga frågan för att skapa dem.</span><span class="sxs-lookup"><span data-stu-id="4802f-272">Only note the necessary query to create them.</span></span>
   
   <span data-ttu-id="4802f-273">a.</span><span class="sxs-lookup"><span data-stu-id="4802f-273">a.</span></span>  <span data-ttu-id="4802f-274">Logga in till huvudnod i Hadoop-kluster</span><span class="sxs-lookup"><span data-stu-id="4802f-274">Log in to the head node of the Hadoop cluster</span></span>
   
   <span data-ttu-id="4802f-275">b.</span><span class="sxs-lookup"><span data-stu-id="4802f-275">b.</span></span>  <span data-ttu-id="4802f-276">Öppna Hadoop-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="4802f-276">Open the Hadoop Command Line.</span></span>
   
   <span data-ttu-id="4802f-277">c.</span><span class="sxs-lookup"><span data-stu-id="4802f-277">c.</span></span>  <span data-ttu-id="4802f-278">Ange Hive rotkatalogen av kommandot `cd %hive_home%\bin` i Hadoop-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="4802f-278">Enter the Hive root directory by command `cd %hive_home%\bin` in Hadoop Command Line.</span></span>
   
   <span data-ttu-id="4802f-279">d.</span><span class="sxs-lookup"><span data-stu-id="4802f-279">d.</span></span>  <span data-ttu-id="4802f-280">Köra Hive-frågor i Hadoop kommandoraden i huvudnod i Hadoop-kluster och utforska data och skapa funktioner efter behov.</span><span class="sxs-lookup"><span data-stu-id="4802f-280">Run the Hive queries in Hadoop Command Line on the head node of the Hadoop cluster to explore the data and create features as needed.</span></span>
7. <span data-ttu-id="4802f-281">Om det behövs och/eller önskas, exempeldata att få plats i Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="4802f-281">If needed and/or desired, sample the data to fit in Azure Machine Learning Studio.</span></span>
8. <span data-ttu-id="4802f-282">Logga in på den [Azure Machine Learning Studio](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="4802f-282">Sign in to the [Azure Machine Learning Studio](https://studio.azureml.net/).</span></span>
9. <span data-ttu-id="4802f-283">Läsa data direkt från den `Hive Queries` med hjälp av den [importera Data] [ import-data] modul.</span><span class="sxs-lookup"><span data-stu-id="4802f-283">Read the data directly from the `Hive Queries` using the [Import Data][import-data] module.</span></span> <span data-ttu-id="4802f-284">Klistra in den nödvändiga fråga som extraherar fält, skapar funktioner och exempel data om det behövs direkt i den [importera Data] [ import-data] frågan.</span><span class="sxs-lookup"><span data-stu-id="4802f-284">Paste the necessary query which extracts fields, creates features, and samples data if needed directly in the [Import Data][import-data] query.</span></span>
10. <span data-ttu-id="4802f-285">Enkel Azure Machine Learning experimentera flödet som börjar med överförda dataset.</span><span class="sxs-lookup"><span data-stu-id="4802f-285">Simple Azure Machine Learning experiment flow starting with uploaded dataset.</span></span>

## <span data-ttu-id="4802f-286"><a name="decisiontree"></a>Beslutsträd för val av scenariot</span><span class="sxs-lookup"><span data-stu-id="4802f-286"><a name="decisiontree"></a>Decision tree for scenario selection</span></span>
- - -
<span data-ttu-id="4802f-287">Följande diagram sammanfattar de scenarier som beskrivs ovan och Advanced Analytics processen och Teknikval som görs som tar dig vidare till var och en av de specificerade scenarierna.</span><span class="sxs-lookup"><span data-stu-id="4802f-287">The following diagram summarizes the scenarios described above and the Advanced Analytics Process and Technology choices made that take you to each of the itemized scenarios.</span></span> <span data-ttu-id="4802f-288">Observera att databearbetning, undersökning, funktionen tekniker och sampling kan ta i metoden/miljön--på källan, mellanliggande, eller mål-miljöer, och kan fortsätta upprepade gånger efter behov.</span><span class="sxs-lookup"><span data-stu-id="4802f-288">Note that data processing, exploration, feature engineering, and sampling may take place in one or more method/environment -- at the source, intermediate, and/or target environments – and may proceed iteratively as needed.</span></span> <span data-ttu-id="4802f-289">Diagrammet bara fungerar som en bild på några möjliga flöden och ger inte en uttömmande förteckning.</span><span class="sxs-lookup"><span data-stu-id="4802f-289">The diagram only serves as an illustration of some of possible flows and does not provide an exhaustive enumeration.</span></span>

![Genomgång exempelscenarier DS process][8]

### <a name="advanced-analytics-in-action-examples"></a><span data-ttu-id="4802f-291">Avancerade analyser i praktiken exempel</span><span class="sxs-lookup"><span data-stu-id="4802f-291">Advanced Analytics in action Examples</span></span>
<span data-ttu-id="4802f-292">För slutpunkt till slutpunkt Azure Machine Learning genomgångar som använder den Advanced Analytics processen och teknik som använder offentliga datauppsättningar, se:</span><span class="sxs-lookup"><span data-stu-id="4802f-292">For end-to-end Azure Machine Learning walkthroughs that employ the Advanced Analytics Process and Technology using public datasets, see:</span></span>

* <span data-ttu-id="4802f-293">[Teamet vetenskap av data i praktiken: använder SQL Server](machine-learning-data-science-process-sql-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="4802f-293">[Team Data Science Process in action: using SQL Server](machine-learning-data-science-process-sql-walkthrough.md).</span></span>
* <span data-ttu-id="4802f-294">[Teamet vetenskap av data i praktiken: med HDInsight Hadoop-kluster](machine-learning-data-science-process-hive-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="4802f-294">[Team Data Science Process in action: using HDInsight Hadoop clusters](machine-learning-data-science-process-hive-walkthrough.md).</span></span>

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
