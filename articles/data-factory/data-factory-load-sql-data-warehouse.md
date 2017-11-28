---
title: aaaLoad terabyte data till SQL Data Warehouse | Microsoft Docs
description: "Visar hur 1 TB data kan läsas in i Azure SQL Data Warehouse med Azure Data Factory under 15 minuter"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: a6c133c0-ced2-463c-86f0-a07b00c9e37f
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jingwang
ms.openlocfilehash: e63f60461b082b0e3979004cb631dbf4a6710de3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="load-1-tb-into-azure-sql-data-warehouse-under-15-minutes-with-data-factory"></a><span data-ttu-id="27ff5-103">Läsa in 1 TB i Azure SQL Data Warehouse under 15 minuter med Data Factory</span><span class="sxs-lookup"><span data-stu-id="27ff5-103">Load 1 TB into Azure SQL Data Warehouse under 15 minutes with Data Factory</span></span>
<span data-ttu-id="27ff5-104">[Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) är en molnbaserad skalbar databas kan bearbeta massiva mängder data, relationell såväl som icke-relationell.</span><span class="sxs-lookup"><span data-stu-id="27ff5-104">[Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) is a cloud-based, scale-out database capable of processing massive volumes of data, both relational and non-relational.</span></span>  <span data-ttu-id="27ff5-105">Bygger på arkitektur med massivt parallell bearbetning (MPP) och är SQL Data Warehouse optimerad för arbetsbelastningar på företag data warehouse.</span><span class="sxs-lookup"><span data-stu-id="27ff5-105">Built on massively parallel processing (MPP) architecture, SQL Data Warehouse is optimized for enterprise data warehouse workloads.</span></span>  <span data-ttu-id="27ff5-106">Den erbjuder molnet elasticitet med hello flexibilitet tooscale lagring och beräkna oberoende av varandra.</span><span class="sxs-lookup"><span data-stu-id="27ff5-106">It offers cloud elasticity with hello flexibility tooscale storage and compute independently.</span></span>

<span data-ttu-id="27ff5-107">Komma igång med Azure SQL Data Warehouse är nu enklare än någonsin med **Azure Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="27ff5-107">Getting started with Azure SQL Data Warehouse is now easier than ever using **Azure Data Factory**.</span></span>  <span data-ttu-id="27ff5-108">Azure Data Factory är en helt hanterad molnbaserade integration datatjänst, vilket kan vara används toopopulate ett SQL Data Warehouse med hello data från ditt befintliga system och sparar värdefull tid när du utvärderar SQL Data Warehouse och skapa din analytics lösningar.</span><span class="sxs-lookup"><span data-stu-id="27ff5-108">Azure Data Factory is a fully managed cloud-based data integration service, which can be used toopopulate a SQL Data Warehouse with hello data from your existing system, and saving you valuable time while evaluating SQL Data Warehouse and building your analytics solutions.</span></span> <span data-ttu-id="27ff5-109">Här följer hello viktiga fördelar med att läsa in data i Azure SQL Data Warehouse med Azure Data Factory:</span><span class="sxs-lookup"><span data-stu-id="27ff5-109">Here are hello key benefits of loading data into Azure SQL Data Warehouse using Azure Data Factory:</span></span>

* <span data-ttu-id="27ff5-110">**Enkelt tooset in**: 5 intuitiva guiden med ingen skript som krävs.</span><span class="sxs-lookup"><span data-stu-id="27ff5-110">**Easy tooset up**: 5-step intuitive wizard with no scripting required.</span></span>
* <span data-ttu-id="27ff5-111">**Omfattande stöd för datalager**: inbyggt stöd för ett stort utbud av lokala och molnbaserade dataarkiv.</span><span class="sxs-lookup"><span data-stu-id="27ff5-111">**Rich data store support**: built-in support for a rich set of on-premises and cloud-based data stores.</span></span>
* <span data-ttu-id="27ff5-112">**Skyddade och kompatibla**: data överförs över HTTPS eller ExpressRoute och tjänsten för global närvaro garanterar att dina data lämnar aldrig hello geografisk gräns</span><span class="sxs-lookup"><span data-stu-id="27ff5-112">**Secure and compliant**: data is transferred over HTTPS or ExpressRoute, and global service presence ensures your data never leaves hello geographical boundary</span></span>
* <span data-ttu-id="27ff5-113">**Enastående prestanda genom att använda PolyBase** – med Polybase är hello mest effektiva sättet toomove data till Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="27ff5-113">**Unparalleled performance by using PolyBase** – Using Polybase is hello most efficient way toomove data into Azure SQL Data Warehouse.</span></span> <span data-ttu-id="27ff5-114">Du kan uppnå hög belastning hastigheter från alla typer av datalager utöver Azure Blob storage som hello Polybase stöder som standard använder hello mellanlagring blob-funktionen.</span><span class="sxs-lookup"><span data-stu-id="27ff5-114">Using hello staging blob feature, you can achieve high load speeds from all types of data stores besides Azure Blob storage, which hello Polybase supports by default.</span></span>

<span data-ttu-id="27ff5-115">Den här artikeln beskrivs hur du toouse Data Factory kopiera guiden tooload 1 TB data från Azure Blobblagring till Azure SQL Data Warehouse i under 15 minuter vid över 1,2 Gbit/s genomströmning.</span><span class="sxs-lookup"><span data-stu-id="27ff5-115">This article shows you how toouse Data Factory Copy Wizard tooload 1-TB data from Azure Blob Storage into Azure SQL Data Warehouse in under 15 minutes, at over 1.2 GBps throughput.</span></span>

<span data-ttu-id="27ff5-116">Den här artikeln innehåller stegvisa instruktioner för att flytta data till Azure SQL Data Warehouse med hjälp av hello guiden Kopiera.</span><span class="sxs-lookup"><span data-stu-id="27ff5-116">This article provides step-by-step instructions for moving data into Azure SQL Data Warehouse by using hello Copy Wizard.</span></span>

> [!NOTE]
>  <span data-ttu-id="27ff5-117">Allmän information om funktionerna i Data Factory för att flytta data till/från Azure SQL Data Warehouse finns [flytta data tooand från Azure SQL Data Warehouse med Azure Data Factory](data-factory-azure-sql-data-warehouse-connector.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="27ff5-117">For general information about capabilities of Data Factory in moving data to/from Azure SQL Data Warehouse, see [Move data tooand from Azure SQL Data Warehouse using Azure Data Factory](data-factory-azure-sql-data-warehouse-connector.md) article.</span></span>
>
> <span data-ttu-id="27ff5-118">Du kan också skapa pipelines med hjälp av Azure portal, Visual Studio, PowerShell, osv. Se [Självstudier: kopiera data från Azure Blob tooAzure SQL-databas](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för en snabb genomgång med stegvisa instruktioner för att använda hello Kopieringsaktiviteten i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="27ff5-118">You can also build pipelines using Azure portal, Visual Studio, PowerShell, etc. See [Tutorial: Copy data from Azure Blob tooAzure SQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for a quick walkthrough with step-by-step instructions for using hello Copy Activity in Azure Data Factory.</span></span>  
>
>

## <a name="prerequisites"></a><span data-ttu-id="27ff5-119">Krav</span><span class="sxs-lookup"><span data-stu-id="27ff5-119">Prerequisites</span></span>
* <span data-ttu-id="27ff5-120">Azure Blob Storage: experimentet använder Azure Blob Storage (GRS) för att lagra TPC-H tester dataset.</span><span class="sxs-lookup"><span data-stu-id="27ff5-120">Azure Blob Storage: this experiment uses Azure Blob Storage (GRS) for storing TPC-H testing dataset.</span></span>  <span data-ttu-id="27ff5-121">Lär dig mer om du inte har ett Azure storage-konto [hur toocreate ett lagringskonto](../storage/common/storage-create-storage-account.md#create-a-storage-account).</span><span class="sxs-lookup"><span data-stu-id="27ff5-121">If you do not have an Azure storage account, learn [how toocreate a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account).</span></span>
* <span data-ttu-id="27ff5-122">[TPC-H](http://www.tpc.org/tpch/) data: vi toouse TPC-H som hello tester dataset.</span><span class="sxs-lookup"><span data-stu-id="27ff5-122">[TPC-H](http://www.tpc.org/tpch/) data: we are going toouse TPC-H as hello testing dataset.</span></span>  <span data-ttu-id="27ff5-123">toodo att du behöver toouse `dbgen` från TPC-H toolkit som hjälper dig att generera hello dataset.</span><span class="sxs-lookup"><span data-stu-id="27ff5-123">toodo that, you need toouse `dbgen` from TPC-H toolkit, which helps you generate hello dataset.</span></span>  <span data-ttu-id="27ff5-124">Du kan antingen ladda ned källkoden för `dbgen` från [TPC verktyg](http://www.tpc.org/tpc_documents_current_versions/current_specifications.asp) och kompilera den själv eller hämta hello kompileras binära från [GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TPCHTools).</span><span class="sxs-lookup"><span data-stu-id="27ff5-124">You can either download source code for `dbgen` from [TPC Tools](http://www.tpc.org/tpc_documents_current_versions/current_specifications.asp) and compile it yourself, or download hello compiled binary from [GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TPCHTools).</span></span>  <span data-ttu-id="27ff5-125">Kör dbgen.exe med hello följande kommandon toogenerate 1 TB flat fil för `lineitem` tabell sprids över 10 filer:</span><span class="sxs-lookup"><span data-stu-id="27ff5-125">Run dbgen.exe with hello following commands toogenerate 1 TB flat file for `lineitem` table spread across 10 files:</span></span>

  * `Dbgen -s 1000 -S **1** -C 10 -T L -v`
  * `Dbgen -s 1000 -S **2** -C 10 -T L -v`
  * <span data-ttu-id="27ff5-126">…</span><span class="sxs-lookup"><span data-stu-id="27ff5-126">…</span></span>
  * `Dbgen -s 1000 -S **10** -C 10 -T L -v`

    <span data-ttu-id="27ff5-127">Kopiera hello genererat filer tooAzure Blob.</span><span class="sxs-lookup"><span data-stu-id="27ff5-127">Now copy hello generated files tooAzure Blob.</span></span>  <span data-ttu-id="27ff5-128">Se för[flytta data tooand från ett lokalt filsystem med hjälp av Azure Data Factory](data-factory-onprem-file-system-connector.md) för hur toodo som med ADF kopia.</span><span class="sxs-lookup"><span data-stu-id="27ff5-128">Refer too[Move data tooand from an on-premises file system by using Azure Data Factory](data-factory-onprem-file-system-connector.md) for how toodo that using ADF Copy.</span></span>    
* <span data-ttu-id="27ff5-129">Azure SQL Data Warehouse: experimentet läser in data i Azure SQL Data Warehouse som skapats med 6 000 dwu: er</span><span class="sxs-lookup"><span data-stu-id="27ff5-129">Azure SQL Data Warehouse: this experiment loads data into Azure SQL Data Warehouse created with 6,000 DWUs</span></span>

    <span data-ttu-id="27ff5-130">Se för[skapar ett Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md) detaljerade anvisningar om hur toocreate en SQL Data Warehouse-databas.</span><span class="sxs-lookup"><span data-stu-id="27ff5-130">Refer too[Create an Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md) for detailed instructions on how toocreate a SQL Data Warehouse database.</span></span>  <span data-ttu-id="27ff5-131">tooget hello bästa möjliga belastningen prestanda i SQL Data Warehouse med Polybase vi väljer maxantalet Informationslagerenheter (dwu: er) tillåts i hello prestanda inställning, som är 6 000 dwu: er.</span><span class="sxs-lookup"><span data-stu-id="27ff5-131">tooget hello best possible load performance into SQL Data Warehouse using Polybase, we choose maximum number of Data Warehouse Units (DWUs) allowed in hello Performance setting, which is 6,000 DWUs.</span></span>

  > [!NOTE]
  > <span data-ttu-id="27ff5-132">Vid inläsning från Azure Blob är hello prestanda för datainläsning proportionell toohello antalet dwu: er som du konfigurerar på hello SQL Data Warehouse:</span><span class="sxs-lookup"><span data-stu-id="27ff5-132">When loading from Azure Blob, hello data loading performance is directly proportional toohello number of DWUs you configure on hello SQL Data Warehouse:</span></span>
  >
  > <span data-ttu-id="27ff5-133">Läsa in 1 TB i 1 000 tar DWU SQL Data Warehouse 87 minuter (~ 200 Mbit/s genomströmning) lästes in 1 TB till 2 000 DWU-informationslager tar 46 minuter (~ 380 Mbit/s genomströmning) lästes in 1 TB till 6 000 DWU SQL Data Warehouse tar 14 minuter (~1.2 Gbit/s genomströmning)</span><span class="sxs-lookup"><span data-stu-id="27ff5-133">Loading 1 TB into 1,000 DWU SQL Data Warehouse takes 87 minutes (~200 MBps throughput) Loading 1 TB into 2,000 DWU SQL Data Warehouse takes 46 minutes (~380 MBps throughput) Loading 1 TB into 6,000 DWU SQL Data Warehouse takes 14 minutes (~1.2 GBps throughput)</span></span>
  >
  >

    <span data-ttu-id="27ff5-134">toocreate ett SQL Data Warehouse med 6 000 dwu: er skjutreglaget hello prestanda alla hello sätt toohello höger:</span><span class="sxs-lookup"><span data-stu-id="27ff5-134">toocreate a SQL Data Warehouse with 6,000 DWUs, move hello Performance slider all hello way toohello right:</span></span>

    ![Skjutreglaget för prestanda](media/data-factory-load-sql-data-warehouse/performance-slider.png)

    <span data-ttu-id="27ff5-136">För en befintlig databas som inte är konfigurerad med 6 000 dwu: er kan skala upp med hjälp av Azure portal.</span><span class="sxs-lookup"><span data-stu-id="27ff5-136">For an existing database that is not configured with 6,000 DWUs, you can scale it up using Azure portal.</span></span>  <span data-ttu-id="27ff5-137">Navigera toohello databas i Azure-portalen och det finns en **skala** knapp i hello **översikt** panelen visas i följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="27ff5-137">Navigate toohello database in Azure portal, and there is a **Scale** button in hello **Overview** panel shown in hello following image:</span></span>

    ![Skala knappen](media/data-factory-load-sql-data-warehouse/scale-button.png)    

    <span data-ttu-id="27ff5-139">Klicka på hello **skala** knappen tooopen hello följande panelen, flytta skjutreglaget hello toohello maxvärdet och på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="27ff5-139">Click hello **Scale** button tooopen hello following panel, move hello slider toohello maximum value, and click **Save** button.</span></span>

    ![Dialogruta](media/data-factory-load-sql-data-warehouse/scale-dialog.png)

    <span data-ttu-id="27ff5-141">Experimentet läser in data i Azure SQL Data Warehouse med hjälp av `xlargerc` resursklassen.</span><span class="sxs-lookup"><span data-stu-id="27ff5-141">This experiment loads data into Azure SQL Data Warehouse using `xlargerc` resource class.</span></span>

    <span data-ttu-id="27ff5-142">tooachieve bästa möjliga genomströmningen, kopiera måste toobe utförs med hjälp av en SQL Data Warehouse användare för`xlargerc` resursklassen.</span><span class="sxs-lookup"><span data-stu-id="27ff5-142">tooachieve best possible throughput, copy needs toobe performed using a SQL Data Warehouse user belonging too`xlargerc` resource class.</span></span>  <span data-ttu-id="27ff5-143">Lär dig hur toodo som genom att följa [ändra ett exempel på användaren resurs klassen](../sql-data-warehouse/sql-data-warehouse-develop-concurrency.md#changing-user-resource-class-example).</span><span class="sxs-lookup"><span data-stu-id="27ff5-143">Learn how toodo that by following [Change a user resource class example](../sql-data-warehouse/sql-data-warehouse-develop-concurrency.md#changing-user-resource-class-example).</span></span>  
* <span data-ttu-id="27ff5-144">Skapa mål tabellschemat i Azure SQL Data Warehouse-databas genom att köra följande DDL-instruktion hello:</span><span class="sxs-lookup"><span data-stu-id="27ff5-144">Create destination table schema in Azure SQL Data Warehouse database, by running hello following DDL statement:</span></span>

    ```SQL  
    CREATE TABLE [dbo].[lineitem]
    (
        [L_ORDERKEY] [bigint] NOT NULL,
        [L_PARTKEY] [bigint] NOT NULL,
        [L_SUPPKEY] [bigint] NOT NULL,
        [L_LINENUMBER] [int] NOT NULL,
        [L_QUANTITY] [decimal](15, 2) NULL,
        [L_EXTENDEDPRICE] [decimal](15, 2) NULL,
        [L_DISCOUNT] [decimal](15, 2) NULL,
        [L_TAX] [decimal](15, 2) NULL,
        [L_RETURNFLAG] [char](1) NULL,
        [L_LINESTATUS] [char](1) NULL,
        [L_SHIPDATE] [date] NULL,
        [L_COMMITDATE] [date] NULL,
        [L_RECEIPTDATE] [date] NULL,
        [L_SHIPINSTRUCT] [char](25) NULL,
        [L_SHIPMODE] [char](10) NULL,
        [L_COMMENT] [varchar](44) NULL
    )
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    ```
<span data-ttu-id="27ff5-145">Med hello nödvändiga steg slutförts, är vi nu redo tooconfigure hello kopieringsaktiviteten med hello guiden Kopiera.</span><span class="sxs-lookup"><span data-stu-id="27ff5-145">With hello prerequisite steps completed, we are now ready tooconfigure hello copy activity using hello Copy Wizard.</span></span>

## <a name="launch-copy-wizard"></a><span data-ttu-id="27ff5-146">Använda guiden Kopiera</span><span class="sxs-lookup"><span data-stu-id="27ff5-146">Launch Copy Wizard</span></span>
1. <span data-ttu-id="27ff5-147">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="27ff5-147">Log in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="27ff5-148">Klicka på **+ ny** hello övre vänstra hörnet och klicka på **Intelligence + analys**, och klicka på **Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="27ff5-148">Click **+ NEW** from hello top-left corner, click **Intelligence + analytics**, and click **Data Factory**.</span></span>
3. <span data-ttu-id="27ff5-149">I hello **nya data factory** bladet:</span><span class="sxs-lookup"><span data-stu-id="27ff5-149">In hello **New data factory** blade:</span></span>

   1. <span data-ttu-id="27ff5-150">Ange **LoadIntoSQLDWDataFactory** för hello **namn**.</span><span class="sxs-lookup"><span data-stu-id="27ff5-150">Enter **LoadIntoSQLDWDataFactory** for hello **name**.</span></span>
       <span data-ttu-id="27ff5-151">hello namn i hello Azure data factory måste vara globalt unika.</span><span class="sxs-lookup"><span data-stu-id="27ff5-151">hello name of hello Azure data factory must be globally unique.</span></span> <span data-ttu-id="27ff5-152">Om felmeddelandet hello: **datafabriksnamnet ”LoadIntoSQLDWDataFactory” är inte tillgänglig**, ändra hello namn i hello data factory (till exempel yournameLoadIntoSQLDWDataFactory) och försök att skapa igen.</span><span class="sxs-lookup"><span data-stu-id="27ff5-152">If you receive hello error: **Data factory name “LoadIntoSQLDWDataFactory” is not available**, change hello name of hello data factory (for example, yournameLoadIntoSQLDWDataFactory) and try creating again.</span></span> <span data-ttu-id="27ff5-153">Se artikeln [Data Factory – namnregler](data-factory-naming-rules.md) för namnregler för Data Factory-artefakter.</span><span class="sxs-lookup"><span data-stu-id="27ff5-153">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>  
   2. <span data-ttu-id="27ff5-154">Välj din Azure-**prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="27ff5-154">Select your Azure **subscription**.</span></span>
   3. <span data-ttu-id="27ff5-155">För resursgruppen, gör du något av följande hello:</span><span class="sxs-lookup"><span data-stu-id="27ff5-155">For Resource Group, do one of hello following steps:</span></span>
      1. <span data-ttu-id="27ff5-156">Välj **använda befintliga** tooselect en befintlig resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="27ff5-156">Select **Use existing** tooselect an existing resource group.</span></span>
      2. <span data-ttu-id="27ff5-157">Välj **Skapa nytt** tooenter ett namn för en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="27ff5-157">Select **Create new** tooenter a name for a resource group.</span></span>
   4. <span data-ttu-id="27ff5-158">Välj en **plats** för hello data factory.</span><span class="sxs-lookup"><span data-stu-id="27ff5-158">Select a **location** for hello data factory.</span></span>
   5. <span data-ttu-id="27ff5-159">Välj **PIN-kod toodashboard** kryssrutan längst hello hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="27ff5-159">Select **Pin toodashboard** check box at hello bottom of hello blade.</span></span>  
   6. <span data-ttu-id="27ff5-160">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="27ff5-160">Click **Create**.</span></span>
4. <span data-ttu-id="27ff5-161">När hello har skapats visas hello **Data Factory** bladet som visas i följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="27ff5-161">After hello creation is complete, you see hello **Data Factory** blade as shown in hello following image:</span></span>

   ![Datafabrikens startsida](media/data-factory-load-sql-data-warehouse/data-factory-home-page-copy-data.png)
5. <span data-ttu-id="27ff5-163">Klicka på startsidan för hello Data Factory hello **kopiera data** panelen toolaunch **guiden Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="27ff5-163">On hello Data Factory home page, click hello **Copy data** tile toolaunch **Copy Wizard**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="27ff5-164">Om du ser att hello webbläsare har ”auktorisera...”, inaktivera/avmarkera **blockerar cookies från tredje part och platsdata** inställning (eller) se till att den är aktiverad och skapa ett undantag för **login.microsoftonline.com**och försök sedan starta hello guiden igen.</span><span class="sxs-lookup"><span data-stu-id="27ff5-164">If you see that hello web browser is stuck at "Authorizing...", disable/uncheck **Block third party cookies and site data** setting (or) keep it enabled and create an exception for **login.microsoftonline.com** and then try launching hello wizard again.</span></span>
   >
   >

## <a name="step-1-configure-data-loading-schedule"></a><span data-ttu-id="27ff5-165">Steg 1: Konfigurera schemat för datainläsning</span><span class="sxs-lookup"><span data-stu-id="27ff5-165">Step 1: Configure data loading schedule</span></span>
<span data-ttu-id="27ff5-166">hello första steget är tooconfigure hello datainläsning schema.</span><span class="sxs-lookup"><span data-stu-id="27ff5-166">hello first step is tooconfigure hello data loading schedule.</span></span>  

<span data-ttu-id="27ff5-167">I hello **egenskaper** sidan:</span><span class="sxs-lookup"><span data-stu-id="27ff5-167">In hello **Properties** page:</span></span>

1. <span data-ttu-id="27ff5-168">Ange **CopyFromBlobToAzureSqlDataWarehouse** för **aktivitet**</span><span class="sxs-lookup"><span data-stu-id="27ff5-168">Enter **CopyFromBlobToAzureSqlDataWarehouse** for **Task name**</span></span>
2. <span data-ttu-id="27ff5-169">Välj **kör nu en gång** alternativet.</span><span class="sxs-lookup"><span data-stu-id="27ff5-169">Select **Run once now** option.</span></span>   
3. <span data-ttu-id="27ff5-170">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="27ff5-170">Click **Next**.</span></span>  

    ![Guiden Kopiera - egenskapssidan](media/data-factory-load-sql-data-warehouse/copy-wizard-properties-page.png)

## <a name="step-2-configure-source"></a><span data-ttu-id="27ff5-172">Steg 2: Konfigurera källan</span><span class="sxs-lookup"><span data-stu-id="27ff5-172">Step 2: Configure source</span></span>
<span data-ttu-id="27ff5-173">Det här avsnittet visar du hello steg tooconfigure hello källa: Azure Blob som innehåller hello 1 TB TPC-H radobjekt filer.</span><span class="sxs-lookup"><span data-stu-id="27ff5-173">This section shows you hello steps tooconfigure hello source: Azure Blob containing hello 1-TB TPC-H line item files.</span></span>

1. <span data-ttu-id="27ff5-174">Välj hello **Azure Blob Storage** som hello data lagras och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="27ff5-174">Select hello **Azure Blob Storage** as hello data store and click **Next**.</span></span>

    ![Kopiera - guidesidan Välj källa](media/data-factory-load-sql-data-warehouse/select-source-connection.png)

2. <span data-ttu-id="27ff5-176">Fyll i hello anslutningsinformationen för hello Azure Blob storage-konto och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="27ff5-176">Fill in hello connection information for hello Azure Blob storage account, and click **Next**.</span></span>

    ![Guiden Kopiera - anslutningsinformationen för datakällan](media/data-factory-load-sql-data-warehouse/source-connection-info.png)

3. <span data-ttu-id="27ff5-178">Välj hello **mappen** som innehåller hello TPC-H rad filer och klickar på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="27ff5-178">Choose hello **folder** containing hello TPC-H line item files and click **Next**.</span></span>

    ![Guiden Kopiera – Välj inkommande mapp](media/data-factory-load-sql-data-warehouse/select-input-folder.png)

4. <span data-ttu-id="27ff5-180">När du klickar på **nästa**, hello formatinställningar identifieras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="27ff5-180">Upon clicking **Next**, hello file format settings are detected automatically.</span></span>  <span data-ttu-id="27ff5-181">Kontrollera toomake till en kolumnavgränsaren för är ' | 'i stället för hello standard kommatecken','.</span><span class="sxs-lookup"><span data-stu-id="27ff5-181">Check toomake sure that column delimiter is ‘|’ instead of hello default comma ‘,’.</span></span>  <span data-ttu-id="27ff5-182">Klicka på **nästa** när du har förhandsgranskas hello data.</span><span class="sxs-lookup"><span data-stu-id="27ff5-182">Click **Next** after you have previewed hello data.</span></span>

    ![Guiden Kopiera - inställningar för format](media/data-factory-load-sql-data-warehouse/file-format-settings.png)

## <a name="step-3-configure-destination"></a><span data-ttu-id="27ff5-184">Steg 3: Konfigurera mål</span><span class="sxs-lookup"><span data-stu-id="27ff5-184">Step 3: Configure destination</span></span>
<span data-ttu-id="27ff5-185">Det här avsnittet visas hur tooconfigure hello mål: `lineitem` tabell i hello Azure SQL Data Warehouse-databas.</span><span class="sxs-lookup"><span data-stu-id="27ff5-185">This section shows you how tooconfigure hello destination: `lineitem` table in hello Azure SQL Data Warehouse database.</span></span>

1. <span data-ttu-id="27ff5-186">Välj **Azure SQL Data Warehouse** som mål för hello lagra och klickar på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="27ff5-186">Choose **Azure SQL Data Warehouse** as hello destination store and click **Next**.</span></span>

    ![Guiden Kopiera - Välj målserver datalager](media/data-factory-load-sql-data-warehouse/select-destination-data-store.png)

2. <span data-ttu-id="27ff5-188">Fyll i hello anslutningsinformationen för Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="27ff5-188">Fill in hello connection information for Azure SQL Data Warehouse.</span></span>  <span data-ttu-id="27ff5-189">Kontrollera att du anger hello-användare som är medlem i rollen hello `xlargerc` (se hello **krav** avsnittet detaljerade anvisningar), och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="27ff5-189">Make sure you specify hello user that is a member of hello role `xlargerc` (see hello **prerequisites** section for detailed instructions), and click **Next**.</span></span>

    ![Guiden Kopiera - anslutningsinformation för mål](media/data-factory-load-sql-data-warehouse/destination-connection-info.png)

3. <span data-ttu-id="27ff5-191">Välj hello måltabellen och på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="27ff5-191">Choose hello destination table and click **Next**.</span></span>

    ![Kopiera guidesidan - tabellen mappning](media/data-factory-load-sql-data-warehouse/table-mapping-page.png)

4. <span data-ttu-id="27ff5-193">Schemat mappning på sidan låter ”tillämpa kolumnmappningen” alternativet är avmarkerat och klickar på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="27ff5-193">In Schema mapping page, leave "Apply column mapping" option unchecked and click **Next**.</span></span>

## <a name="step-4-performance-settings"></a><span data-ttu-id="27ff5-194">Steg 4: Prestandainställningar</span><span class="sxs-lookup"><span data-stu-id="27ff5-194">Step 4: Performance settings</span></span>

<span data-ttu-id="27ff5-195">**Tillåt polybase** är markerad som standard.</span><span class="sxs-lookup"><span data-stu-id="27ff5-195">**Allow polybase** is checked by default.</span></span>  <span data-ttu-id="27ff5-196">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="27ff5-196">Click **Next**.</span></span>

![Kopiera guidesidan - schemat mappning](media/data-factory-load-sql-data-warehouse/performance-settings-page.png)

## <a name="step-5-deploy-and-monitor-load-results"></a><span data-ttu-id="27ff5-198">Steg 5: Distribuera och övervaka belastningen resultat</span><span class="sxs-lookup"><span data-stu-id="27ff5-198">Step 5: Deploy and monitor load results</span></span>
1. <span data-ttu-id="27ff5-199">Klicka på **Slutför** knappen toodeploy.</span><span class="sxs-lookup"><span data-stu-id="27ff5-199">Click **Finish** button toodeploy.</span></span>

    ![Guiden Kopiera - sammanfattningssida](media/data-factory-load-sql-data-warehouse/summary-page.png)

2. <span data-ttu-id="27ff5-201">När hello distributionen är klar klickar du på `Click here toomonitor copy pipeline` toomonitor hello kopiera kör pågår.</span><span class="sxs-lookup"><span data-stu-id="27ff5-201">After hello deployment is complete, click `Click here toomonitor copy pipeline` toomonitor hello copy run progress.</span></span> <span data-ttu-id="27ff5-202">Välj hello kopiera pipeline som du skapade i hello **aktivitet Windows** lista.</span><span class="sxs-lookup"><span data-stu-id="27ff5-202">Select hello copy pipeline you created in hello **Activity Windows** list.</span></span>

    ![Guiden Kopiera - sammanfattningssida](media/data-factory-load-sql-data-warehouse/select-pipeline-monitor-manage-app.png)

    <span data-ttu-id="27ff5-204">Du kan visa hello kopiera kör information i hello **aktivitet fönstret Explorer** i hello högra panelen, inklusive hello datavolym läses från källan och skrivs till målet, varaktighet och hello genomsnittlig genomströmning för hello kör.</span><span class="sxs-lookup"><span data-stu-id="27ff5-204">You can view hello copy run details in hello **Activity Window Explorer** in hello right panel, including hello data volume read from source and written into destination, duration, and hello average throughput for hello run.</span></span>

    <span data-ttu-id="27ff5-205">Som du kan se hello följande skärmdump, kopierar 1 TB från Azure Blob Storage till SQL Data Warehouse tog 14 minuter, ett effektivt sätt att uppnå 1,22 Gbit/s genomströmning!</span><span class="sxs-lookup"><span data-stu-id="27ff5-205">As you can see from hello following screen shot, copying 1 TB from Azure Blob Storage into SQL Data Warehouse took 14 minutes, effectively achieving 1.22 GBps throughput!</span></span>

    ![Guiden Kopiera - lyckades dialogrutan](media/data-factory-load-sql-data-warehouse/succeeded-info.png)

## <a name="best-practices"></a><span data-ttu-id="27ff5-207">Bästa praxis</span><span class="sxs-lookup"><span data-stu-id="27ff5-207">Best practices</span></span>
<span data-ttu-id="27ff5-208">Här följer några Metodtips för att köra din Azure SQL Data Warehouse-databas:</span><span class="sxs-lookup"><span data-stu-id="27ff5-208">Here are a few best practices for running your Azure SQL Data Warehouse database:</span></span>

* <span data-ttu-id="27ff5-209">Använd en större resursklassen när inläsning till ett GRUPPERAT COLUMNSTORE-INDEX.</span><span class="sxs-lookup"><span data-stu-id="27ff5-209">Use a larger resource class when loading into a CLUSTERED COLUMNSTORE INDEX.</span></span>
* <span data-ttu-id="27ff5-210">Överväg att använda hash-distribution genom att markera kolumn i stället för default round robin distribution för effektivare kopplingar.</span><span class="sxs-lookup"><span data-stu-id="27ff5-210">For more efficient joins, consider using hash distribution by a select column instead of default round robin distribution.</span></span>
* <span data-ttu-id="27ff5-211">Överväg att använda heap för tillfälliga data för högre belastning hastighet.</span><span class="sxs-lookup"><span data-stu-id="27ff5-211">For faster load speeds, consider using heap for transient data.</span></span>
* <span data-ttu-id="27ff5-212">Skapa statistik när du är klar läser in Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="27ff5-212">Create statistics after you finish loading Azure SQL Data Warehouse.</span></span>

<span data-ttu-id="27ff5-213">Se [Metodtips för Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-best-practices.md) mer information.</span><span class="sxs-lookup"><span data-stu-id="27ff5-213">See [Best practices for Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-best-practices.md) for details.</span></span>

## <a name="next-steps"></a><span data-ttu-id="27ff5-214">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="27ff5-214">Next steps</span></span>
* <span data-ttu-id="27ff5-215">[Guiden för data Factory kopiera](data-factory-copy-wizard.md) -den här artikeln innehåller information om hello guiden Kopiera.</span><span class="sxs-lookup"><span data-stu-id="27ff5-215">[Data Factory Copy Wizard](data-factory-copy-wizard.md) - This article provides details about hello Copy Wizard.</span></span>
* <span data-ttu-id="27ff5-216">[Kopiera aktivitet prestanda och prestandajustering guide](data-factory-copy-activity-performance.md) -den här artikeln innehåller prestandajustering guide och hello referens prestandamått.</span><span class="sxs-lookup"><span data-stu-id="27ff5-216">[Copy Activity performance and tuning guide](data-factory-copy-activity-performance.md) - This article contains hello reference performance measurements and tuning guide.</span></span>
