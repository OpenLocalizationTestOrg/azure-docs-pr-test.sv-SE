---
title: "Läs in terabyte data till SQL Data Warehouse | Microsoft Docs"
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
ms.openlocfilehash: c29f1f01b660c4eb780e178a68036327fafa9ba6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="load-1-tb-into-azure-sql-data-warehouse-under-15-minutes-with-data-factory"></a><span data-ttu-id="f1e9b-103">Läsa in 1 TB i Azure SQL Data Warehouse under 15 minuter med Data Factory</span><span class="sxs-lookup"><span data-stu-id="f1e9b-103">Load 1 TB into Azure SQL Data Warehouse under 15 minutes with Data Factory</span></span>
<span data-ttu-id="f1e9b-104">[Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) är en molnbaserad skalbar databas kan bearbeta massiva mängder data, relationell såväl som icke-relationell.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-104">[Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) is a cloud-based, scale-out database capable of processing massive volumes of data, both relational and non-relational.</span></span>  <span data-ttu-id="f1e9b-105">Bygger på arkitektur med massivt parallell bearbetning (MPP) och är SQL Data Warehouse optimerad för arbetsbelastningar på företag data warehouse.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-105">Built on massively parallel processing (MPP) architecture, SQL Data Warehouse is optimized for enterprise data warehouse workloads.</span></span>  <span data-ttu-id="f1e9b-106">Den erbjuder molntjänster elasticitet möjlighet att skala lagring och beräkning oberoende av varandra.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-106">It offers cloud elasticity with the flexibility to scale storage and compute independently.</span></span>

<span data-ttu-id="f1e9b-107">Komma igång med Azure SQL Data Warehouse är nu enklare än någonsin med **Azure Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-107">Getting started with Azure SQL Data Warehouse is now easier than ever using **Azure Data Factory**.</span></span>  <span data-ttu-id="f1e9b-108">Azure Data Factory är en helt hanterad molnbaserade data integration tjänst som kan användas för att fylla i en SQL Data Warehouse med data från ditt befintliga system och sparar värdefull tid när du utvärderar SQL Data Warehouse och skapa din analytics lösningar.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-108">Azure Data Factory is a fully managed cloud-based data integration service, which can be used to populate a SQL Data Warehouse with the data from your existing system, and saving you valuable time while evaluating SQL Data Warehouse and building your analytics solutions.</span></span> <span data-ttu-id="f1e9b-109">Här är de främsta fördelarna med att läsa in data i Azure SQL Data Warehouse med Azure Data Factory:</span><span class="sxs-lookup"><span data-stu-id="f1e9b-109">Here are the key benefits of loading data into Azure SQL Data Warehouse using Azure Data Factory:</span></span>

* <span data-ttu-id="f1e9b-110">**Enkelt att ställa in**: 5 intuitiva guiden med ingen skript som krävs.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-110">**Easy to set up**: 5-step intuitive wizard with no scripting required.</span></span>
* <span data-ttu-id="f1e9b-111">**Omfattande stöd för datalager**: inbyggt stöd för ett stort utbud av lokala och molnbaserade dataarkiv.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-111">**Rich data store support**: built-in support for a rich set of on-premises and cloud-based data stores.</span></span>
* <span data-ttu-id="f1e9b-112">**Skyddade och kompatibla**: data överförs över HTTPS eller ExpressRoute och tjänsten för global närvaro garanterar att dina data lämnar aldrig geografisk gräns</span><span class="sxs-lookup"><span data-stu-id="f1e9b-112">**Secure and compliant**: data is transferred over HTTPS or ExpressRoute, and global service presence ensures your data never leaves the geographical boundary</span></span>
* <span data-ttu-id="f1e9b-113">**Enastående prestanda genom att använda PolyBase** – med Polybase är det mest effektiva sättet att flytta data till Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-113">**Unparalleled performance by using PolyBase** – Using Polybase is the most efficient way to move data into Azure SQL Data Warehouse.</span></span> <span data-ttu-id="f1e9b-114">Funktionen fristående blob kan uppnå du hög belastning hastigheter från alla typer av datalager utöver Azure Blob storage, Polybase stöder som standard.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-114">Using the staging blob feature, you can achieve high load speeds from all types of data stores besides Azure Blob storage, which the Polybase supports by default.</span></span>

<span data-ttu-id="f1e9b-115">Den här artikeln visar hur du använder guiden för Data Factory kopiera att läsa in 1 TB data från Azure Blobblagring till Azure SQL Data Warehouse under 15 minuter på över 1,2 Gbit/s genomströmning.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-115">This article shows you how to use Data Factory Copy Wizard to load 1-TB data from Azure Blob Storage into Azure SQL Data Warehouse in under 15 minutes, at over 1.2 GBps throughput.</span></span>

<span data-ttu-id="f1e9b-116">Den här artikeln innehåller stegvisa instruktioner för att flytta data till Azure SQL Data Warehouse med hjälp av guiden Kopiera.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-116">This article provides step-by-step instructions for moving data into Azure SQL Data Warehouse by using the Copy Wizard.</span></span>

> [!NOTE]
>  <span data-ttu-id="f1e9b-117">Allmän information om funktionerna i Data Factory för att flytta data till/från Azure SQL Data Warehouse finns [flytta data till och från Azure SQL Data Warehouse med Azure Data Factory](data-factory-azure-sql-data-warehouse-connector.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-117">For general information about capabilities of Data Factory in moving data to/from Azure SQL Data Warehouse, see [Move data to and from Azure SQL Data Warehouse using Azure Data Factory](data-factory-azure-sql-data-warehouse-connector.md) article.</span></span>
>
> <span data-ttu-id="f1e9b-118">Du kan också skapa pipelines med hjälp av Azure portal, Visual Studio, PowerShell, osv. Se [Självstudier: kopiera data från Azure Blob till Azure SQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för en snabb genomgång med stegvisa instruktioner för att använda aktiviteten kopiera i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-118">You can also build pipelines using Azure portal, Visual Studio, PowerShell, etc. See [Tutorial: Copy data from Azure Blob to Azure SQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for a quick walkthrough with step-by-step instructions for using the Copy Activity in Azure Data Factory.</span></span>  
>
>

## <a name="prerequisites"></a><span data-ttu-id="f1e9b-119">Krav</span><span class="sxs-lookup"><span data-stu-id="f1e9b-119">Prerequisites</span></span>
* <span data-ttu-id="f1e9b-120">Azure Blob Storage: experimentet använder Azure Blob Storage (GRS) för att lagra TPC-H tester dataset.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-120">Azure Blob Storage: this experiment uses Azure Blob Storage (GRS) for storing TPC-H testing dataset.</span></span>  <span data-ttu-id="f1e9b-121">Lär dig mer om du inte har ett Azure storage-konto [hur du skapar ett lagringskonto](../storage/common/storage-create-storage-account.md#create-a-storage-account).</span><span class="sxs-lookup"><span data-stu-id="f1e9b-121">If you do not have an Azure storage account, learn [how to create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account).</span></span>
* <span data-ttu-id="f1e9b-122">[TPC-H](http://www.tpc.org/tpch/) data: vi ska använda TPC-H som tester dataset.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-122">[TPC-H](http://www.tpc.org/tpch/) data: we are going to use TPC-H as the testing dataset.</span></span>  <span data-ttu-id="f1e9b-123">För att göra det, måste du använda `dbgen` från TPC-H toolkit som hjälper dig att generera datamängden.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-123">To do that, you need to use `dbgen` from TPC-H toolkit, which helps you generate the dataset.</span></span>  <span data-ttu-id="f1e9b-124">Du kan antingen ladda ned källkoden för `dbgen` från [TPC verktyg](http://www.tpc.org/tpc_documents_current_versions/current_specifications.asp) och kompilera den själv eller hämta kompilerade binärfilen från [GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TPCHTools).</span><span class="sxs-lookup"><span data-stu-id="f1e9b-124">You can either download source code for `dbgen` from [TPC Tools](http://www.tpc.org/tpc_documents_current_versions/current_specifications.asp) and compile it yourself, or download the compiled binary from [GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TPCHTools).</span></span>  <span data-ttu-id="f1e9b-125">Kör dbgen.exe med följande kommandon för att generera 1 TB flat fil för `lineitem` tabell sprids över 10 filer:</span><span class="sxs-lookup"><span data-stu-id="f1e9b-125">Run dbgen.exe with the following commands to generate 1 TB flat file for `lineitem` table spread across 10 files:</span></span>

  * `Dbgen -s 1000 -S **1** -C 10 -T L -v`
  * `Dbgen -s 1000 -S **2** -C 10 -T L -v`
  * <span data-ttu-id="f1e9b-126">…</span><span class="sxs-lookup"><span data-stu-id="f1e9b-126">…</span></span>
  * `Dbgen -s 1000 -S **10** -C 10 -T L -v`

    <span data-ttu-id="f1e9b-127">Nu kopiera genererade filer till Azure-Blob.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-127">Now copy the generated files to Azure Blob.</span></span>  <span data-ttu-id="f1e9b-128">Referera till [flytta data till och från ett lokalt filsystem med hjälp av Azure Data Factory](data-factory-onprem-file-system-connector.md) för hur du gör med ADF kopia.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-128">Refer to [Move data to and from an on-premises file system by using Azure Data Factory](data-factory-onprem-file-system-connector.md) for how to do that using ADF Copy.</span></span>    
* <span data-ttu-id="f1e9b-129">Azure SQL Data Warehouse: experimentet läser in data i Azure SQL Data Warehouse som skapats med 6 000 dwu: er</span><span class="sxs-lookup"><span data-stu-id="f1e9b-129">Azure SQL Data Warehouse: this experiment loads data into Azure SQL Data Warehouse created with 6,000 DWUs</span></span>

    <span data-ttu-id="f1e9b-130">Referera till [skapar ett Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md) detaljerade anvisningar om hur du skapar en SQL Data Warehouse-databas.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-130">Refer to [Create an Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md) for detailed instructions on how to create a SQL Data Warehouse database.</span></span>  <span data-ttu-id="f1e9b-131">För att få bästa möjliga belastningen prestanda i SQL Data Warehouse med Polybase kan väljer vi maxantalet Informationslagerenheter (dwu: er) tillåts i inställningen prestanda, som är 6 000 dwu: er.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-131">To get the best possible load performance into SQL Data Warehouse using Polybase, we choose maximum number of Data Warehouse Units (DWUs) allowed in the Performance setting, which is 6,000 DWUs.</span></span>

  > [!NOTE]
  > <span data-ttu-id="f1e9b-132">Prestanda för datainläsning är proportionell mot antalet dwu: er som du konfigurerar i SQL Data Warehouse vid inläsning från Azure Blob:</span><span class="sxs-lookup"><span data-stu-id="f1e9b-132">When loading from Azure Blob, the data loading performance is directly proportional to the number of DWUs you configure on the SQL Data Warehouse:</span></span>
  >
  > <span data-ttu-id="f1e9b-133">Läsa in 1 TB i 1 000 tar DWU SQL Data Warehouse 87 minuter (~ 200 Mbit/s genomströmning) lästes in 1 TB till 2 000 DWU-informationslager tar 46 minuter (~ 380 Mbit/s genomströmning) lästes in 1 TB till 6 000 DWU SQL Data Warehouse tar 14 minuter (~1.2 Gbit/s genomströmning)</span><span class="sxs-lookup"><span data-stu-id="f1e9b-133">Loading 1 TB into 1,000 DWU SQL Data Warehouse takes 87 minutes (~200 MBps throughput) Loading 1 TB into 2,000 DWU SQL Data Warehouse takes 46 minutes (~380 MBps throughput) Loading 1 TB into 6,000 DWU SQL Data Warehouse takes 14 minutes (~1.2 GBps throughput)</span></span>
  >
  >

    <span data-ttu-id="f1e9b-134">Flytta skjutreglaget prestanda ända till höger om du vill skapa ett SQL Data Warehouse med 6 000 dwu: er:</span><span class="sxs-lookup"><span data-stu-id="f1e9b-134">To create a SQL Data Warehouse with 6,000 DWUs, move the Performance slider all the way to the right:</span></span>

    ![Skjutreglaget för prestanda](media/data-factory-load-sql-data-warehouse/performance-slider.png)

    <span data-ttu-id="f1e9b-136">För en befintlig databas som inte är konfigurerad med 6 000 dwu: er kan skala upp med hjälp av Azure portal.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-136">For an existing database that is not configured with 6,000 DWUs, you can scale it up using Azure portal.</span></span>  <span data-ttu-id="f1e9b-137">Navigera till databasen i Azure-portalen och det finns en **skala** knappen i den **översikt** panelen visas i följande bild:</span><span class="sxs-lookup"><span data-stu-id="f1e9b-137">Navigate to the database in Azure portal, and there is a **Scale** button in the **Overview** panel shown in the following image:</span></span>

    ![Skala knappen](media/data-factory-load-sql-data-warehouse/scale-button.png)    

    <span data-ttu-id="f1e9b-139">Klicka på den **skala** knappen för att öppna följande panelen, flytta skjutreglaget till det högsta värdet och klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-139">Click the **Scale** button to open the following panel, move the slider to the maximum value, and click **Save** button.</span></span>

    ![Dialogruta](media/data-factory-load-sql-data-warehouse/scale-dialog.png)

    <span data-ttu-id="f1e9b-141">Experimentet läser in data i Azure SQL Data Warehouse med hjälp av `xlargerc` resursklassen.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-141">This experiment loads data into Azure SQL Data Warehouse using `xlargerc` resource class.</span></span>

    <span data-ttu-id="f1e9b-142">För att uppnå bästa möjliga genomströmningen, kopiera måste utföras med hjälp av en SQL Data Warehouse-användare som hör till `xlargerc` resursklassen.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-142">To achieve best possible throughput, copy needs to be performed using a SQL Data Warehouse user belonging to `xlargerc` resource class.</span></span>  <span data-ttu-id="f1e9b-143">Lär dig hur du gör det genom att följa [ändra ett exempel på användaren resurs klassen](../sql-data-warehouse/sql-data-warehouse-develop-concurrency.md#changing-user-resource-class-example).</span><span class="sxs-lookup"><span data-stu-id="f1e9b-143">Learn how to do that by following [Change a user resource class example](../sql-data-warehouse/sql-data-warehouse-develop-concurrency.md#changing-user-resource-class-example).</span></span>  
* <span data-ttu-id="f1e9b-144">Skapa mål tabellschemat i Azure SQL Data Warehouse-databas genom att köra följande DDL-instruktion:</span><span class="sxs-lookup"><span data-stu-id="f1e9b-144">Create destination table schema in Azure SQL Data Warehouse database, by running the following DDL statement:</span></span>

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
<span data-ttu-id="f1e9b-145">Med nödvändiga steg för att slutföra, är vi nu redo att konfigurera kopieringsaktiviteten med hjälp av guiden Kopiera.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-145">With the prerequisite steps completed, we are now ready to configure the copy activity using the Copy Wizard.</span></span>

## <a name="launch-copy-wizard"></a><span data-ttu-id="f1e9b-146">Använda guiden Kopiera</span><span class="sxs-lookup"><span data-stu-id="f1e9b-146">Launch Copy Wizard</span></span>
1. <span data-ttu-id="f1e9b-147">Logga in på [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f1e9b-147">Log in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="f1e9b-148">Klicka på **+ ny** från det övre vänstra hörnet, klickar du på **Intelligence + analys**, och klicka på **Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-148">Click **+ NEW** from the top-left corner, click **Intelligence + analytics**, and click **Data Factory**.</span></span>
3. <span data-ttu-id="f1e9b-149">På bladet **Ny datafabrik**:</span><span class="sxs-lookup"><span data-stu-id="f1e9b-149">In the **New data factory** blade:</span></span>

   1. <span data-ttu-id="f1e9b-150">Ange **LoadIntoSQLDWDataFactory** för den **namn**.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-150">Enter **LoadIntoSQLDWDataFactory** for the **name**.</span></span>
       <span data-ttu-id="f1e9b-151">Namnet på Azure Data Factory måste vara globalt unikt.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-151">The name of the Azure data factory must be globally unique.</span></span> <span data-ttu-id="f1e9b-152">Om du får felmeddelandet: **datafabriksnamnet ”LoadIntoSQLDWDataFactory” är inte tillgänglig**, ändra namnet på data factory (till exempel yournameLoadIntoSQLDWDataFactory) och försök att skapa igen.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-152">If you receive the error: **Data factory name “LoadIntoSQLDWDataFactory” is not available**, change the name of the data factory (for example, yournameLoadIntoSQLDWDataFactory) and try creating again.</span></span> <span data-ttu-id="f1e9b-153">Se artikeln [Data Factory – namnregler](data-factory-naming-rules.md) för namnregler för Data Factory-artefakter.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-153">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>  
   2. <span data-ttu-id="f1e9b-154">Välj din Azure-**prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-154">Select your Azure **subscription**.</span></span>
   3. <span data-ttu-id="f1e9b-155">För resursgruppen utför du något av följande steg:</span><span class="sxs-lookup"><span data-stu-id="f1e9b-155">For Resource Group, do one of the following steps:</span></span>
      1. <span data-ttu-id="f1e9b-156">Välj **Använd befintlig** och välj en befintlig resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-156">Select **Use existing** to select an existing resource group.</span></span>
      2. <span data-ttu-id="f1e9b-157">Välj **Skapa nytt** och ange ett namn för en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-157">Select **Create new** to enter a name for a resource group.</span></span>
   4. <span data-ttu-id="f1e9b-158">Välj **plats** för datafabriken.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-158">Select a **location** for the data factory.</span></span>
   5. <span data-ttu-id="f1e9b-159">Välj kryssrutan **PIN-kod för instrumentpanelen** längst ned på bladet.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-159">Select **Pin to dashboard** check box at the bottom of the blade.</span></span>  
   6. <span data-ttu-id="f1e9b-160">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-160">Click **Create**.</span></span>
4. <span data-ttu-id="f1e9b-161">När datafabriken har skapats visas bladet **Datafabrik** enligt nedanstående bild:</span><span class="sxs-lookup"><span data-stu-id="f1e9b-161">After the creation is complete, you see the **Data Factory** blade as shown in the following image:</span></span>

   ![Datafabrikens startsida](media/data-factory-load-sql-data-warehouse/data-factory-home-page-copy-data.png)
5. <span data-ttu-id="f1e9b-163">På startsidan Datafabrik klickar du på ikonen **Kopiera data** för att starta **guiden Kopiera**.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-163">On the Data Factory home page, click the **Copy data** tile to launch **Copy Wizard**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="f1e9b-164">Om du ser att webbläsaren har fastnat på ”Auktoriserar...”, inaktiverar/avmarkerar du inställningen **Blockera cookies från tredje part och platsdata** (eller) behåller den aktiverad och skapar ett undantag för **login.microsoftonline.com**. Försök sedan starta guiden igen.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-164">If you see that the web browser is stuck at "Authorizing...", disable/uncheck **Block third party cookies and site data** setting (or) keep it enabled and create an exception for **login.microsoftonline.com** and then try launching the wizard again.</span></span>
   >
   >

## <a name="step-1-configure-data-loading-schedule"></a><span data-ttu-id="f1e9b-165">Steg 1: Konfigurera schemat för datainläsning</span><span class="sxs-lookup"><span data-stu-id="f1e9b-165">Step 1: Configure data loading schedule</span></span>
<span data-ttu-id="f1e9b-166">Det första steget är att konfigurera schemat för datainläsning.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-166">The first step is to configure the data loading schedule.</span></span>  

<span data-ttu-id="f1e9b-167">På sidan **Egenskaper**:</span><span class="sxs-lookup"><span data-stu-id="f1e9b-167">In the **Properties** page:</span></span>

1. <span data-ttu-id="f1e9b-168">Ange **CopyFromBlobToAzureSqlDataWarehouse** för **aktivitet**</span><span class="sxs-lookup"><span data-stu-id="f1e9b-168">Enter **CopyFromBlobToAzureSqlDataWarehouse** for **Task name**</span></span>
2. <span data-ttu-id="f1e9b-169">Välj **kör nu en gång** alternativet.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-169">Select **Run once now** option.</span></span>   
3. <span data-ttu-id="f1e9b-170">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-170">Click **Next**.</span></span>  

    ![Guiden Kopiera - egenskapssidan](media/data-factory-load-sql-data-warehouse/copy-wizard-properties-page.png)

## <a name="step-2-configure-source"></a><span data-ttu-id="f1e9b-172">Steg 2: Konfigurera källan</span><span class="sxs-lookup"><span data-stu-id="f1e9b-172">Step 2: Configure source</span></span>
<span data-ttu-id="f1e9b-173">Det här avsnittet visas hur du konfigurerar källa: Azure Blob som innehåller 1 TB TPC-H radobjekt filer.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-173">This section shows you the steps to configure the source: Azure Blob containing the 1-TB TPC-H line item files.</span></span>

1. <span data-ttu-id="f1e9b-174">Välj den **Azure Blob Storage** som du lagrar och klickar på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-174">Select the **Azure Blob Storage** as the data store and click **Next**.</span></span>

    ![Kopiera - guidesidan Välj källa](media/data-factory-load-sql-data-warehouse/select-source-connection.png)

2. <span data-ttu-id="f1e9b-176">Fyll i anslutningsinformationen för Azure Blob storage-konto och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-176">Fill in the connection information for the Azure Blob storage account, and click **Next**.</span></span>

    ![Guiden Kopiera - anslutningsinformationen för datakällan](media/data-factory-load-sql-data-warehouse/source-connection-info.png)

3. <span data-ttu-id="f1e9b-178">Välj den **mappen** som innehåller TPC-H objektet filer och klickar på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-178">Choose the **folder** containing the TPC-H line item files and click **Next**.</span></span>

    ![Guiden Kopiera – Välj inkommande mapp](media/data-factory-load-sql-data-warehouse/select-input-folder.png)

4. <span data-ttu-id="f1e9b-180">När du klickar på **nästa**, formatinställningar filen identifieras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-180">Upon clicking **Next**, the file format settings are detected automatically.</span></span>  <span data-ttu-id="f1e9b-181">Kontrollera att den kolumnavgränsaren är ' | 'i stället för standard kommatecken','.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-181">Check to make sure that column delimiter is ‘|’ instead of the default comma ‘,’.</span></span>  <span data-ttu-id="f1e9b-182">Klicka på **nästa** när du har förhandsgranskas data.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-182">Click **Next** after you have previewed the data.</span></span>

    ![Guiden Kopiera - inställningar för format](media/data-factory-load-sql-data-warehouse/file-format-settings.png)

## <a name="step-3-configure-destination"></a><span data-ttu-id="f1e9b-184">Steg 3: Konfigurera mål</span><span class="sxs-lookup"><span data-stu-id="f1e9b-184">Step 3: Configure destination</span></span>
<span data-ttu-id="f1e9b-185">Det här avsnittet visar hur du konfigurerar mål: `lineitem` tabell i Azure SQL Data Warehouse-databas.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-185">This section shows you how to configure the destination: `lineitem` table in the Azure SQL Data Warehouse database.</span></span>

1. <span data-ttu-id="f1e9b-186">Välj **Azure SQL Data Warehouse** som mål att lagra och klickar på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-186">Choose **Azure SQL Data Warehouse** as the destination store and click **Next**.</span></span>

    ![Guiden Kopiera - Välj målserver datalager](media/data-factory-load-sql-data-warehouse/select-destination-data-store.png)

2. <span data-ttu-id="f1e9b-188">Fyll i anslutningsinformationen för Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-188">Fill in the connection information for Azure SQL Data Warehouse.</span></span>  <span data-ttu-id="f1e9b-189">Kontrollera att du anger den användare som är medlem i rollen `xlargerc` (finns i **krav** avsnittet detaljerade anvisningar), och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-189">Make sure you specify the user that is a member of the role `xlargerc` (see the **prerequisites** section for detailed instructions), and click **Next**.</span></span>

    ![Guiden Kopiera - anslutningsinformation för mål](media/data-factory-load-sql-data-warehouse/destination-connection-info.png)

3. <span data-ttu-id="f1e9b-191">Välj tabellen och klickar på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-191">Choose the destination table and click **Next**.</span></span>

    ![Kopiera guidesidan - tabellen mappning](media/data-factory-load-sql-data-warehouse/table-mapping-page.png)

4. <span data-ttu-id="f1e9b-193">Schemat mappning på sidan låter ”tillämpa kolumnmappningen” alternativet är avmarkerat och klickar på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-193">In Schema mapping page, leave "Apply column mapping" option unchecked and click **Next**.</span></span>

## <a name="step-4-performance-settings"></a><span data-ttu-id="f1e9b-194">Steg 4: Prestandainställningar</span><span class="sxs-lookup"><span data-stu-id="f1e9b-194">Step 4: Performance settings</span></span>

<span data-ttu-id="f1e9b-195">**Tillåt polybase** är markerad som standard.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-195">**Allow polybase** is checked by default.</span></span>  <span data-ttu-id="f1e9b-196">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-196">Click **Next**.</span></span>

![Kopiera guidesidan - schemat mappning](media/data-factory-load-sql-data-warehouse/performance-settings-page.png)

## <a name="step-5-deploy-and-monitor-load-results"></a><span data-ttu-id="f1e9b-198">Steg 5: Distribuera och övervaka belastningen resultat</span><span class="sxs-lookup"><span data-stu-id="f1e9b-198">Step 5: Deploy and monitor load results</span></span>
1. <span data-ttu-id="f1e9b-199">Klicka på **Slutför** om du vill distribuera.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-199">Click **Finish** button to deploy.</span></span>

    ![Guiden Kopiera - sammanfattningssida](media/data-factory-load-sql-data-warehouse/summary-page.png)

2. <span data-ttu-id="f1e9b-201">När installationen är klar klickar du på `Click here to monitor copy pipeline` att övervaka kopian kör pågår.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-201">After the deployment is complete, click `Click here to monitor copy pipeline` to monitor the copy run progress.</span></span> <span data-ttu-id="f1e9b-202">Välj Kopiera pipeline som du skapade i den **aktivitet Windows** lista.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-202">Select the copy pipeline you created in the **Activity Windows** list.</span></span>

    ![Guiden Kopiera - sammanfattningssida](media/data-factory-load-sql-data-warehouse/select-pipeline-monitor-manage-app.png)

    <span data-ttu-id="f1e9b-204">Du kan visa kopian kör information i den **aktivitet fönstret Explorer** i den högra panelen, inklusive datavolym läses från källan och skrivs till målet, varaktighet och den genomsnittliga genomflödet för körning.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-204">You can view the copy run details in the **Activity Window Explorer** in the right panel, including the data volume read from source and written into destination, duration, and the average throughput for the run.</span></span>

    <span data-ttu-id="f1e9b-205">Som du ser i följande skärmbild visar tog kopiera 1 TB från Azure Blob Storage till SQL Data Warehouse 14 minuter, ett effektivt sätt att uppnå 1,22 Gbit/s genomströmning!</span><span class="sxs-lookup"><span data-stu-id="f1e9b-205">As you can see from the following screen shot, copying 1 TB from Azure Blob Storage into SQL Data Warehouse took 14 minutes, effectively achieving 1.22 GBps throughput!</span></span>

    ![Guiden Kopiera - lyckades dialogrutan](media/data-factory-load-sql-data-warehouse/succeeded-info.png)

## <a name="best-practices"></a><span data-ttu-id="f1e9b-207">Bästa praxis</span><span class="sxs-lookup"><span data-stu-id="f1e9b-207">Best practices</span></span>
<span data-ttu-id="f1e9b-208">Här följer några Metodtips för att köra din Azure SQL Data Warehouse-databas:</span><span class="sxs-lookup"><span data-stu-id="f1e9b-208">Here are a few best practices for running your Azure SQL Data Warehouse database:</span></span>

* <span data-ttu-id="f1e9b-209">Använd en större resursklassen när inläsning till ett GRUPPERAT COLUMNSTORE-INDEX.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-209">Use a larger resource class when loading into a CLUSTERED COLUMNSTORE INDEX.</span></span>
* <span data-ttu-id="f1e9b-210">Överväg att använda hash-distribution genom att markera kolumn i stället för default round robin distribution för effektivare kopplingar.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-210">For more efficient joins, consider using hash distribution by a select column instead of default round robin distribution.</span></span>
* <span data-ttu-id="f1e9b-211">Överväg att använda heap för tillfälliga data för högre belastning hastighet.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-211">For faster load speeds, consider using heap for transient data.</span></span>
* <span data-ttu-id="f1e9b-212">Skapa statistik när du är klar läser in Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-212">Create statistics after you finish loading Azure SQL Data Warehouse.</span></span>

<span data-ttu-id="f1e9b-213">Se [Metodtips för Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-best-practices.md) mer information.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-213">See [Best practices for Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-best-practices.md) for details.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f1e9b-214">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f1e9b-214">Next steps</span></span>
* <span data-ttu-id="f1e9b-215">[Guiden för data Factory kopiera](data-factory-copy-wizard.md) -den här artikeln innehåller information om guiden Kopiera.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-215">[Data Factory Copy Wizard](data-factory-copy-wizard.md) - This article provides details about the Copy Wizard.</span></span>
* <span data-ttu-id="f1e9b-216">[Kopiera aktivitet prestanda och prestandajustering guide](data-factory-copy-activity-performance.md) -den här artikeln innehåller guiden referens prestandamått och prestandajustering.</span><span class="sxs-lookup"><span data-stu-id="f1e9b-216">[Copy Activity performance and tuning guide](data-factory-copy-activity-performance.md) - This article contains the reference performance measurements and tuning guide.</span></span>
