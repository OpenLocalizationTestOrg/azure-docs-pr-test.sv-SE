---
title: aaaUse Redgate tooload data tooyour Azure data warehouse | Microsoft Docs
description: "Lär dig hur toouse Redgate Data Platform Studio för informationslagerscenarier."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
ms.assetid: 670aef98-31f7-4436-86c0-cc989a39fe7f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: 6082390c07c8ffa73ebd8ab272ace00ba8bb1897
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-with-redgate-data-platform-studio"></a><span data-ttu-id="65ef6-103">Läs in data med Redgates Data Platform Studio</span><span class="sxs-lookup"><span data-stu-id="65ef6-103">Load data with Redgate Data Platform Studio</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="65ef6-104">Redgate</span><span class="sxs-lookup"><span data-stu-id="65ef6-104">Redgate</span></span>](sql-data-warehouse-load-with-redgate.md)
> * [<span data-ttu-id="65ef6-105">Data Factory</span><span class="sxs-lookup"><span data-stu-id="65ef6-105">Data Factory</span></span>](sql-data-warehouse-get-started-load-with-azure-data-factory.md)
> * [<span data-ttu-id="65ef6-106">PolyBase</span><span class="sxs-lookup"><span data-stu-id="65ef6-106">PolyBase</span></span>](sql-data-warehouse-get-started-load-with-polybase.md)
> * [<span data-ttu-id="65ef6-107">BCP</span><span class="sxs-lookup"><span data-stu-id="65ef6-107">BCP</span></span>](sql-data-warehouse-load-with-bcp.md)
> 
> 

<span data-ttu-id="65ef6-108">De här självstudierna visar hur toouse [Redgate's Data Platform Studio](http://www.red-gate.com/products/azure-development/data-platform-studio/) (DP) toomove data från en lokal SQL Server-tooAzure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="65ef6-108">This tutorial shows you how toouse [Redgate's Data Platform Studio](http://www.red-gate.com/products/azure-development/data-platform-studio/) (DPS) toomove data from an on-premises SQL Server tooAzure SQL Data Warehouse.</span></span> <span data-ttu-id="65ef6-109">Data Platform Studio gäller hello lämpligaste kompatibilitetsproblem och optimeringar, vilket ger snabbast hello tooget sätt igång med SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="65ef6-109">Data Platform Studio applies hello most appropriate compatibility fixes and optimizations, so it's hello quickest way tooget started with SQL Data Warehouse.</span></span>

> [!NOTE]
> <span data-ttu-id="65ef6-110">[Redgate](http://www.red-gate.com) tillhandahåller olika SQL Server-verktyg, och är sedan länge Microsoft-partner.</span><span class="sxs-lookup"><span data-stu-id="65ef6-110">[Redgate](http://www.red-gate.com) is a long-time Microsoft partner that delivers various SQL Server tools.</span></span> <span data-ttu-id="65ef6-111">Den här funktionen i Data Platform Studio har gjorts tillgängligt kostnadsfritt för såväl kommersiell och icke-kommersiell användning.</span><span class="sxs-lookup"><span data-stu-id="65ef6-111">This feature in Data Platform Studio has been made available freely for both commercial and non-commercial use.</span></span>
> 
> 

## <a name="before-you-begin"></a><span data-ttu-id="65ef6-112">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="65ef6-112">Before you begin</span></span>
### <a name="create-or-identify-resources"></a><span data-ttu-id="65ef6-113">Skapa eller identifiera resurser</span><span class="sxs-lookup"><span data-stu-id="65ef6-113">Create or identify resources</span></span>
<span data-ttu-id="65ef6-114">Innan du påbörjar den här kursen behöver du toohave:</span><span class="sxs-lookup"><span data-stu-id="65ef6-114">Before starting this tutorial, you need toohave:</span></span>

* <span data-ttu-id="65ef6-115">**lokal SQL Server-databas**: hello data som du vill tooimport tooSQL datalagret måste toocome från en lokal SQL Server (version 2008R2 eller senare).</span><span class="sxs-lookup"><span data-stu-id="65ef6-115">**on-premises SQL Server Database**: hello data you want tooimport tooSQL Data Warehouse needs toocome from an on-premises SQL Server (version 2008R2 or above).</span></span> <span data-ttu-id="65ef6-116">Data Platform Studio kan inte importera data direkt från en Azure SQL Database eller från textfiler.</span><span class="sxs-lookup"><span data-stu-id="65ef6-116">Data Platform Studio cannot import data directly from an Azure SQL Database or from text files.</span></span>
* <span data-ttu-id="65ef6-117">**Azure Storage-konto**: Data Platform Studio skapar etapper hello data i Azure Blob Storage innan du läser in den i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="65ef6-117">**Azure Storage Account**: Data Platform Studio stages hello data in Azure Blob Storage before loading it into SQL Data Warehouse.</span></span> <span data-ttu-id="65ef6-118">hello storage-konto måste använda hello ”Resource Manager”-modellen (hello standard) i stället för hello ”klassisk” distributionsmodell.</span><span class="sxs-lookup"><span data-stu-id="65ef6-118">hello storage account must be using hello “Resource Manager” deployment model (hello default) rather than hello “Classic” deployment model.</span></span> <span data-ttu-id="65ef6-119">Om du inte har ett lagringskonto kan du lära dig hur tooCreate ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="65ef6-119">If you don't have a storage account, learn how tooCreate a storage account.</span></span> 
* <span data-ttu-id="65ef6-120">**SQL Data Warehouse**: självstudierna flyttas hello data från lokala SQL Server-tooSQL datalagret, så du måste toohave ett data warehouse online.</span><span class="sxs-lookup"><span data-stu-id="65ef6-120">**SQL Data Warehouse**: This tutorial moves hello data from on-premises SQL Server tooSQL Data Warehouse, so you need toohave a data warehouse online.</span></span> <span data-ttu-id="65ef6-121">Om du inte redan har ett data warehouse, lär du dig hur tooCreate ett Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="65ef6-121">If you do not already have a data warehouse, learn how tooCreate an Azure SQL Data Warehouse.</span></span>

> [!NOTE]
> <span data-ttu-id="65ef6-122">Bättre prestanda om hello storage-konto och hello datalagret skapas i hello samma region.</span><span class="sxs-lookup"><span data-stu-id="65ef6-122">Performance is improved if hello storage account and hello data warehouse are created in hello same region.</span></span>
> 
> 

## <a name="step-1-sign-in-toodata-platform-studio-with-your-azure-account"></a><span data-ttu-id="65ef6-123">Steg 1: Logga in tooData plattform Studio med ditt Azure-konto</span><span class="sxs-lookup"><span data-stu-id="65ef6-123">Step 1: Sign in tooData Platform Studio with your Azure account</span></span>
<span data-ttu-id="65ef6-124">Öppna webbläsaren och gå toohello [Data Platform Studio](https://www.dataplatformstudio.com/) webbplats.</span><span class="sxs-lookup"><span data-stu-id="65ef6-124">Open your web browser and navigate toohello [Data Platform Studio](https://www.dataplatformstudio.com/) website.</span></span> <span data-ttu-id="65ef6-125">Logga in med hello samma Azure-konto som du använt toocreate hello storage-konto och för datalagret.</span><span class="sxs-lookup"><span data-stu-id="65ef6-125">Sign in with hello same Azure account that you used toocreate hello storage account and data warehouse.</span></span> <span data-ttu-id="65ef6-126">Om din e-postadress är associerad med ett arbets eller skolkonto och ett Microsoft-konto kan vara att toochoose hello konto som har åtkomst till tooyour resurser.</span><span class="sxs-lookup"><span data-stu-id="65ef6-126">If your email address is associated with both a work or school account and a Microsoft account, be sure toochoose hello account that has access tooyour resources.</span></span>

> [!NOTE]
> <span data-ttu-id="65ef6-127">Om det här är första gången du använder Data Platform Studio är och toogrant hello program behörighet toomanage Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="65ef6-127">If this is your first time using Data Platform Studio, you are asked toogrant hello application permission toomanage your Azure resources.</span></span>
> 
> 

## <a name="step-2-start-hello-import-wizard"></a><span data-ttu-id="65ef6-128">Steg 2: Starta hello guiden Importera</span><span class="sxs-lookup"><span data-stu-id="65ef6-128">Step 2: Start hello Import Wizard</span></span>
<span data-ttu-id="65ef6-129">Välj hello importera tooAzure SQL Data Warehouse länken toostart hello guiden Importera hello DP huvudskärmen.</span><span class="sxs-lookup"><span data-stu-id="65ef6-129">From hello DPS main screen, select hello Import tooAzure SQL Data Warehouse link toostart hello import wizard.</span></span>

![][1]

## <a name="step-3-install-hello-data-platform-studio-gateway"></a><span data-ttu-id="65ef6-130">Steg 3: Installera hello Data Platform Studio Gateway</span><span class="sxs-lookup"><span data-stu-id="65ef6-130">Step 3: Install hello Data Platform Studio Gateway</span></span>
<span data-ttu-id="65ef6-131">tooconnect tooyour lokala SQL Server-databas måste tooinstall hello DP-Gateway.</span><span class="sxs-lookup"><span data-stu-id="65ef6-131">tooconnect tooyour on-premises SQL Server database, you need tooinstall hello DPS Gateway.</span></span> <span data-ttu-id="65ef6-132">hello-gateway är en klientagent som ger åtkomst tooyour lokala miljö, extraherar hello data och överför det tooyour storage-konto.</span><span class="sxs-lookup"><span data-stu-id="65ef6-132">hello gateway is a client agent that provides access tooyour on-premises environment, extracts hello data, and uploads it tooyour storage account.</span></span> <span data-ttu-id="65ef6-133">Dina data passerar aldrig Redgates servrar.</span><span class="sxs-lookup"><span data-stu-id="65ef6-133">Your data never passes through Redgate’s servers.</span></span> <span data-ttu-id="65ef6-134">tooinstall hello Gateway:</span><span class="sxs-lookup"><span data-stu-id="65ef6-134">tooinstall hello Gateway:</span></span>

1. <span data-ttu-id="65ef6-135">Klicka på hello **skapa Gateway** länk</span><span class="sxs-lookup"><span data-stu-id="65ef6-135">Click hello **Create Gateway** link</span></span>
2. <span data-ttu-id="65ef6-136">Hämta och installera hello en Gateway med hjälp av hello angivna installer</span><span class="sxs-lookup"><span data-stu-id="65ef6-136">Download and install hello Gateway using hello provided installer</span></span>

![][2]

> [!NOTE]
> <span data-ttu-id="65ef6-137">hello Gateway kan installeras på en dator med network access toohello källa SQL Server-databas.</span><span class="sxs-lookup"><span data-stu-id="65ef6-137">hello Gateway can be installed on any machine with network access toohello source SQL Server database.</span></span> <span data-ttu-id="65ef6-138">Den kommer åt hello SQL Server-databas med hjälp av Windows-autentisering med hello autentiseringsuppgifterna för hello aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="65ef6-138">It accesses hello SQL Server database using Windows authentication with hello credentials of hello current user.</span></span>
> 
> 

<span data-ttu-id="65ef6-139">När du har installerat hello Gateway status ändringar tooConnected och du kan välja Nästa.</span><span class="sxs-lookup"><span data-stu-id="65ef6-139">Once installed, hello Gateway status changes tooConnected and you can select Next.</span></span>

## <a name="step-4-identify-hello-source-database"></a><span data-ttu-id="65ef6-140">Steg 4: Identifiera hello källdatabasen</span><span class="sxs-lookup"><span data-stu-id="65ef6-140">Step 4: Identify hello source database</span></span>
<span data-ttu-id="65ef6-141">I hello *ange servernamnet* textruta anger hello namnet på hello-server som är värd för databasen och välj **nästa**.</span><span class="sxs-lookup"><span data-stu-id="65ef6-141">In hello *Enter Server Name* textbox, enter hello name of hello server that hosts your database and select **Next**.</span></span> <span data-ttu-id="65ef6-142">Välj hello-databasen som du vill tooimport data från hello nedrullningsbara menyn.</span><span class="sxs-lookup"><span data-stu-id="65ef6-142">Then, from hello drop-down menu, select hello database you want tooimport data from.</span></span>

![][3]

<span data-ttu-id="65ef6-143">DP kontrollerar hello valda databasen för tabeller tooimport.</span><span class="sxs-lookup"><span data-stu-id="65ef6-143">DPS inspects hello selected database for tables tooimport.</span></span> <span data-ttu-id="65ef6-144">Som standard importerar DP alla hello tabeller i hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="65ef6-144">By default, DPS imports all hello tables in hello database.</span></span> <span data-ttu-id="65ef6-145">Du kan markera eller avmarkera tabeller genom att expandera hello länka alla tabeller.</span><span class="sxs-lookup"><span data-stu-id="65ef6-145">You can select or deselect tables by expanding hello All Tables link.</span></span> <span data-ttu-id="65ef6-146">Välj hello nästa knapp toomove framåt.</span><span class="sxs-lookup"><span data-stu-id="65ef6-146">Select hello Next button toomove forward.</span></span>

## <a name="step-5-choose-a-storage-account-toostage-hello-data"></a><span data-ttu-id="65ef6-147">Steg 5: Välj en storage-konto toostage hello-data</span><span class="sxs-lookup"><span data-stu-id="65ef6-147">Step 5: Choose a storage account toostage hello data</span></span>
<span data-ttu-id="65ef6-148">DP uppmanas du att en plats toostage hello data.</span><span class="sxs-lookup"><span data-stu-id="65ef6-148">DPS prompts you for a location toostage hello data.</span></span> <span data-ttu-id="65ef6-149">Välj ett befintligt lagringskonto från din prenumeration och välj **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="65ef6-149">Choose an existing storage account from your subscription and select **Next**.</span></span>

> [!NOTE]
> <span data-ttu-id="65ef6-150">DP skapar en ny blobbbehållare i hello valt storage-konto och använda en distinkta mapp för varje import.</span><span class="sxs-lookup"><span data-stu-id="65ef6-150">DPS will create a new blob container in hello chosen storage account and use a distinct folder for each import.</span></span>
> 
> 

![][4]

## <a name="step-6-select-a-data-warehouse"></a><span data-ttu-id="65ef6-151">Steg 6: Välj ett informationslager</span><span class="sxs-lookup"><span data-stu-id="65ef6-151">Step 6: Select a data warehouse</span></span>
<span data-ttu-id="65ef6-152">Sedan väljer du ett online [Azure SQL Data Warehouse](http://aka.ms/sqldw) tooimport hello data till databasen.</span><span class="sxs-lookup"><span data-stu-id="65ef6-152">Next, you select an online [Azure SQL Data Warehouse](http://aka.ms/sqldw) database tooimport hello data into.</span></span> <span data-ttu-id="65ef6-153">När du har valt databasen måste tooenter hello autentiseringsuppgifter tooconnect toohello databasen och välj **nästa**.</span><span class="sxs-lookup"><span data-stu-id="65ef6-153">Once you've selected your database, you need tooenter hello credentials tooconnect toohello database and select **Next**.</span></span>

![][5]

> [!NOTE]
> <span data-ttu-id="65ef6-154">DP sammanfogas hello källtabellerna data till hello-datalagret.</span><span class="sxs-lookup"><span data-stu-id="65ef6-154">DPS merges hello source data tables into hello data warehouse.</span></span> <span data-ttu-id="65ef6-155">DP varnar dig om kräver hello tabellnamn toooverwrite befintliga tabeller i hello-datalagret.</span><span class="sxs-lookup"><span data-stu-id="65ef6-155">DPS warns you if hello table name requires it toooverwrite existing tables in hello data warehouse.</span></span> <span data-ttu-id="65ef6-156">Du kan eventuellt ta bort alla befintliga objekt i datalager hello genom markeringar ta bort alla befintliga objekt före import.</span><span class="sxs-lookup"><span data-stu-id="65ef6-156">You may optionally delete any existing objects in hello data warehouse by ticking Delete all existing objects before import.</span></span>
> 
> 

## <a name="step-7-import-hello-data"></a><span data-ttu-id="65ef6-157">Steg 7: Importera hello data</span><span class="sxs-lookup"><span data-stu-id="65ef6-157">Step 7: Import hello data</span></span>
<span data-ttu-id="65ef6-158">DP bekräftar att du vill ha tooimport hello data.</span><span class="sxs-lookup"><span data-stu-id="65ef6-158">DPS confirms that you would like tooimport hello data.</span></span> <span data-ttu-id="65ef6-159">Klicka bara på hello Start importera knappen toobegin hello import av data.</span><span class="sxs-lookup"><span data-stu-id="65ef6-159">Simply click hello Start import button toobegin hello data import.</span></span>

![][6]

<span data-ttu-id="65ef6-160">DP visar en visualisering som visar hello förloppet extrahera och överföra hello data från hello lokala SQL Server och hello fortskrider hello import till SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="65ef6-160">DPS displays a visualization that shows hello progress of extracting and uploading hello data from hello on-premises SQL Server and hello progress of hello import into SQL Data Warehouse.</span></span>

![][7]

<span data-ttu-id="65ef6-161">När hello importen är klar visar DP en sammanfattning av hello dataimporten och en rapport om hello kompatibilitet korrigeringar som har utförts.</span><span class="sxs-lookup"><span data-stu-id="65ef6-161">Once hello import is complete, DPS displays a summary of hello data import and a change report of hello compatibility fixes that have been performed.</span></span>

![][8]

## <a name="next-steps"></a><span data-ttu-id="65ef6-162">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="65ef6-162">Next steps</span></span>
<span data-ttu-id="65ef6-163">tooexplore data inom SQL Data Warehouse starta genom att visa:</span><span class="sxs-lookup"><span data-stu-id="65ef6-163">tooexplore your data within SQL Data Warehouse, start by viewing:</span></span>

* <span data-ttu-id="65ef6-164">[Fråga Azure SQL Data Warehouse (Visual Studio)][Query Azure SQL Data Warehouse (Visual Studio)]</span><span class="sxs-lookup"><span data-stu-id="65ef6-164">[Query Azure SQL Data Warehouse (Visual Studio)][Query Azure SQL Data Warehouse (Visual Studio)]</span></span>
* <span data-ttu-id="65ef6-165">[Visualisera data med Power BI][Visualize data with Power BI]</span><span class="sxs-lookup"><span data-stu-id="65ef6-165">[Visualize data with Power BI][Visualize data with Power BI]</span></span>

<span data-ttu-id="65ef6-166">Mer om Redgate's Data Platform Studio toolearn:</span><span class="sxs-lookup"><span data-stu-id="65ef6-166">toolearn more about Redgate’s Data Platform Studio:</span></span>

* [<span data-ttu-id="65ef6-167">Besök hello DP-webbsida</span><span class="sxs-lookup"><span data-stu-id="65ef6-167">Visit hello DPS homepage</span></span>](http://www.dataplatformstudio.com/)
* [<span data-ttu-id="65ef6-168">Titta på en demonstration av DPS på Channel9</span><span class="sxs-lookup"><span data-stu-id="65ef6-168">Watch a demo of DPS on Channel9</span></span>](https://channel9.msdn.com/Blogs/cloud-with-a-silver-lining/Loading-data-into-Azure-SQL-Datawarehouse-with-Redgate-Data-Platform-Studio)

<span data-ttu-id="65ef6-169">En översikt över andra sätt toomigrate och Läs in finns data i SQL Data Warehouse:</span><span class="sxs-lookup"><span data-stu-id="65ef6-169">For an overview of other ways toomigrate and load your data in SQL Data Warehouse see:</span></span>

* <span data-ttu-id="65ef6-170">[Migrera din lösning tooSQL Data Warehouse][Migrate your solution tooSQL Data Warehouse]</span><span class="sxs-lookup"><span data-stu-id="65ef6-170">[Migrate your solution tooSQL Data Warehouse][Migrate your solution tooSQL Data Warehouse]</span></span>
* [<span data-ttu-id="65ef6-171">Läs in data till Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="65ef6-171">Load data into Azure SQL Data Warehouse</span></span>](sql-data-warehouse-overview-load.md)

<span data-ttu-id="65ef6-172">För fler utvecklingstips, se hello [översikt över SQL Data Warehouse-utveckling](sql-data-warehouse-overview-develop.md).</span><span class="sxs-lookup"><span data-stu-id="65ef6-172">For more development tips, see hello [SQL Data Warehouse development overview](sql-data-warehouse-overview-develop.md).</span></span>

<!--Image references-->
[1]: media/sql-data-warehouse-redgate/2016-10-05_15-59-56.png
[2]: media/sql-data-warehouse-redgate/2016-10-05_11-16-07.png
[3]: media/sql-data-warehouse-redgate/2016-10-05_11-17-46.png
[4]: media/sql-data-warehouse-redgate/2016-10-05_11-20-41.png
[5]: media/sql-data-warehouse-redgate/2016-10-05_11-31-24.png
[6]: media/sql-data-warehouse-redgate/2016-10-05_11-32-20.png
[7]: media/sql-data-warehouse-redgate/2016-10-05_11-49-53.png
[8]: media/sql-data-warehouse-redgate/2016-10-05_12-57-10.png

<!--Article references-->
[Query Azure SQL Data Warehouse (Visual Studio)]: ./sql-data-warehouse-query-visual-studio.md
[Visualize data with Power BI]: ./sql-data-warehouse-get-started-visualize-with-power-bi.md
[Migrate your solution tooSQL Data Warehouse]: ./sql-data-warehouse-overview-migrate.md
[Load data into Azure SQL Data Warehouse]: ./sql-data-warehouse-overview-load.md
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md
