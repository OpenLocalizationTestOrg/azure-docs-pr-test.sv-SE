---
title: "Använda Redgate för att läsa in data till ditt Azure-informationslager | Microsoft Docs"
description: "Ta reda på hur du använder Redgates Data Platform Studio i olika scenarier för datalagringshantering."
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
ms.openlocfilehash: a38b237d5bfc0450c1ca79b53a5784dbb9bf8602
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="load-data-with-redgate-data-platform-studio"></a><span data-ttu-id="73fb3-103">Läs in data med Redgates Data Platform Studio</span><span class="sxs-lookup"><span data-stu-id="73fb3-103">Load data with Redgate Data Platform Studio</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="73fb3-104">Redgate</span><span class="sxs-lookup"><span data-stu-id="73fb3-104">Redgate</span></span>](sql-data-warehouse-load-with-redgate.md)
> * [<span data-ttu-id="73fb3-105">Data Factory</span><span class="sxs-lookup"><span data-stu-id="73fb3-105">Data Factory</span></span>](sql-data-warehouse-get-started-load-with-azure-data-factory.md)
> * [<span data-ttu-id="73fb3-106">PolyBase</span><span class="sxs-lookup"><span data-stu-id="73fb3-106">PolyBase</span></span>](sql-data-warehouse-get-started-load-with-polybase.md)
> * [<span data-ttu-id="73fb3-107">BCP</span><span class="sxs-lookup"><span data-stu-id="73fb3-107">BCP</span></span>](sql-data-warehouse-load-with-bcp.md)
> 
> 

<span data-ttu-id="73fb3-108">Den här självstudien visar hur du använder [Redgates Data Platform Studio](http://www.red-gate.com/products/azure-development/data-platform-studio/) (DPS) för att flytta data från en lokal SQL Server till Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="73fb3-108">This tutorial shows you how to use [Redgate's Data Platform Studio](http://www.red-gate.com/products/azure-development/data-platform-studio/) (DPS) to move data from an on-premises SQL Server to Azure SQL Data Warehouse.</span></span> <span data-ttu-id="73fb3-109">Data Platform Studio tillämpar de lämpligaste kompatibilitetskorrigeringarna och -optimeringarna, så det är det snabbaste sättet att komma igång med SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="73fb3-109">Data Platform Studio applies the most appropriate compatibility fixes and optimizations, so it's the quickest way to get started with SQL Data Warehouse.</span></span>

> [!NOTE]
> <span data-ttu-id="73fb3-110">[Redgate](http://www.red-gate.com) tillhandahåller olika SQL Server-verktyg, och är sedan länge Microsoft-partner.</span><span class="sxs-lookup"><span data-stu-id="73fb3-110">[Redgate](http://www.red-gate.com) is a long-time Microsoft partner that delivers various SQL Server tools.</span></span> <span data-ttu-id="73fb3-111">Den här funktionen i Data Platform Studio har gjorts tillgängligt kostnadsfritt för såväl kommersiell och icke-kommersiell användning.</span><span class="sxs-lookup"><span data-stu-id="73fb3-111">This feature in Data Platform Studio has been made available freely for both commercial and non-commercial use.</span></span>
> 
> 

## <a name="before-you-begin"></a><span data-ttu-id="73fb3-112">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="73fb3-112">Before you begin</span></span>
### <a name="create-or-identify-resources"></a><span data-ttu-id="73fb3-113">Skapa eller identifiera resurser</span><span class="sxs-lookup"><span data-stu-id="73fb3-113">Create or identify resources</span></span>
<span data-ttu-id="73fb3-114">Innan du sätter igång med den här självstudien behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="73fb3-114">Before starting this tutorial, you need to have:</span></span>

* <span data-ttu-id="73fb3-115">**Lokal SQL Server-databas**: De data du vill importera till SQL Data Warehouse måste komma från en lokal SQL Server (version 2008R2 eller senare).</span><span class="sxs-lookup"><span data-stu-id="73fb3-115">**on-premises SQL Server Database**: The data you want to import to SQL Data Warehouse needs to come from an on-premises SQL Server (version 2008R2 or above).</span></span> <span data-ttu-id="73fb3-116">Data Platform Studio kan inte importera data direkt från en Azure SQL Database eller från textfiler.</span><span class="sxs-lookup"><span data-stu-id="73fb3-116">Data Platform Studio cannot import data directly from an Azure SQL Database or from text files.</span></span>
* <span data-ttu-id="73fb3-117">**Azure Storage-konto**: Data Platform Studio mellanlagrar data i Azure Blob Storage innan de läses in till SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="73fb3-117">**Azure Storage Account**: Data Platform Studio stages the data in Azure Blob Storage before loading it into SQL Data Warehouse.</span></span> <span data-ttu-id="73fb3-118">Lagringskontot måste använda distributionsmodellen Resource Manager (standard), inte den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="73fb3-118">The storage account must be using the “Resource Manager” deployment model (the default) rather than the “Classic” deployment model.</span></span> <span data-ttu-id="73fb3-119">Om du inte har ett lagringskonto kan du lära dig hur du skapar ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="73fb3-119">If you don't have a storage account, learn how to Create a storage account.</span></span> 
* <span data-ttu-id="73fb3-120">**SQL Data Warehouse**: I den här självstudien flyttas data från en lokal SQL Server till SQL Data Warehouse, så du måste ha ett informationslager online.</span><span class="sxs-lookup"><span data-stu-id="73fb3-120">**SQL Data Warehouse**: This tutorial moves the data from on-premises SQL Server to SQL Data Warehouse, so you need to have a data warehouse online.</span></span> <span data-ttu-id="73fb3-121">Om du inte redan har ett informationslager kan du lära dig hur du skapar ett Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="73fb3-121">If you do not already have a data warehouse, learn how to Create an Azure SQL Data Warehouse.</span></span>

> [!NOTE]
> <span data-ttu-id="73fb3-122">Prestandan blir högre om lagringskontot och informationslagret skapas i samma region.</span><span class="sxs-lookup"><span data-stu-id="73fb3-122">Performance is improved if the storage account and the data warehouse are created in the same region.</span></span>
> 
> 

## <a name="step-1-sign-in-to-data-platform-studio-with-your-azure-account"></a><span data-ttu-id="73fb3-123">Steg 1: Logga in på Data Platform Studio med ditt Azure-konto</span><span class="sxs-lookup"><span data-stu-id="73fb3-123">Step 1: Sign in to Data Platform Studio with your Azure account</span></span>
<span data-ttu-id="73fb3-124">Öppna webbläsaren och navigera till webbplatsen [Data Platform Studio](https://www.dataplatformstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="73fb3-124">Open your web browser and navigate to the [Data Platform Studio](https://www.dataplatformstudio.com/) website.</span></span> <span data-ttu-id="73fb3-125">Logga in med samma Azure-konto som du använde för att skapa lagringskontot och informationslagret.</span><span class="sxs-lookup"><span data-stu-id="73fb3-125">Sign in with the same Azure account that you used to create the storage account and data warehouse.</span></span> <span data-ttu-id="73fb3-126">Om din e-postadress är associerad med både ett arbets- eller skolkonto och ett Microsoft-konto måste du välja det konto som har åtkomst till resurserna.</span><span class="sxs-lookup"><span data-stu-id="73fb3-126">If your email address is associated with both a work or school account and a Microsoft account, be sure to choose the account that has access to your resources.</span></span>

> [!NOTE]
> <span data-ttu-id="73fb3-127">Om det här är första gången du använder Data Platform Studio uppmanas du att bevilja programmet behörighet för att hantera dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="73fb3-127">If this is your first time using Data Platform Studio, you are asked to grant the application permission to manage your Azure resources.</span></span>
> 
> 

## <a name="step-2-start-the-import-wizard"></a><span data-ttu-id="73fb3-128">Steg 2: Starta guiden Importera</span><span class="sxs-lookup"><span data-stu-id="73fb3-128">Step 2: Start the Import Wizard</span></span>
<span data-ttu-id="73fb3-129">Från DPS-huvudskärmen väljer du länken för att importera till Azure SQL Data Warehouse för att starta importguiden.</span><span class="sxs-lookup"><span data-stu-id="73fb3-129">From the DPS main screen, select the Import to Azure SQL Data Warehouse link to start the import wizard.</span></span>

![][1]

## <a name="step-3-install-the-data-platform-studio-gateway"></a><span data-ttu-id="73fb3-130">Steg 3: Installera Data Platform Studio-gatewayen</span><span class="sxs-lookup"><span data-stu-id="73fb3-130">Step 3: Install the Data Platform Studio Gateway</span></span>
<span data-ttu-id="73fb3-131">För att kunna ansluta den lokala SQL Server-databasen behöver du installera DPS-gatewayen.</span><span class="sxs-lookup"><span data-stu-id="73fb3-131">To connect to your on-premises SQL Server database, you need to install the DPS Gateway.</span></span> <span data-ttu-id="73fb3-132">Gatewayen är en klientagent som ger åtkomst till din lokala miljö. Den extraherar data och överför dem till ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="73fb3-132">The gateway is a client agent that provides access to your on-premises environment, extracts the data, and uploads it to your storage account.</span></span> <span data-ttu-id="73fb3-133">Dina data passerar aldrig Redgates servrar.</span><span class="sxs-lookup"><span data-stu-id="73fb3-133">Your data never passes through Redgate’s servers.</span></span> <span data-ttu-id="73fb3-134">Så här installerar du gatewayen:</span><span class="sxs-lookup"><span data-stu-id="73fb3-134">To install the Gateway:</span></span>

1. <span data-ttu-id="73fb3-135">Klicka på länken **Skapa gateway**</span><span class="sxs-lookup"><span data-stu-id="73fb3-135">Click the **Create Gateway** link</span></span>
2. <span data-ttu-id="73fb3-136">Hämta och installera gatewayen med det medföljande installationsprogrammet</span><span class="sxs-lookup"><span data-stu-id="73fb3-136">Download and install the Gateway using the provided installer</span></span>

![][2]

> [!NOTE]
> <span data-ttu-id="73fb3-137">Gatewayen kan installeras på en dator med nätverksåtkomst till källplatsens SQL Server-databas.</span><span class="sxs-lookup"><span data-stu-id="73fb3-137">The Gateway can be installed on any machine with network access to the source SQL Server database.</span></span> <span data-ttu-id="73fb3-138">Den ansluter till SQL Server-databasen med hjälp av Windows-autentisering med den aktuella användarens autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="73fb3-138">It accesses the SQL Server database using Windows authentication with the credentials of the current user.</span></span>
> 
> 

<span data-ttu-id="73fb3-139">När du har installerat gatewayen ändras dess status till Ansluten och du kan välja Nästa.</span><span class="sxs-lookup"><span data-stu-id="73fb3-139">Once installed, the Gateway status changes to Connected and you can select Next.</span></span>

## <a name="step-4-identify-the-source-database"></a><span data-ttu-id="73fb3-140">Steg 4: Identifiera källdatabasen</span><span class="sxs-lookup"><span data-stu-id="73fb3-140">Step 4: Identify the source database</span></span>
<span data-ttu-id="73fb3-141">I textrutan *Ange servernamn* anger du namnet på den server som är värd för databasen och väljer **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="73fb3-141">In the *Enter Server Name* textbox, enter the name of the server that hosts your database and select **Next**.</span></span> <span data-ttu-id="73fb3-142">Välj sedan databasen som du vill importera data från på den nedrullningsbara menyn.</span><span class="sxs-lookup"><span data-stu-id="73fb3-142">Then, from the drop-down menu, select the database you want to import data from.</span></span>

![][3]

<span data-ttu-id="73fb3-143">DPS söker igenom den valda databasen efter tabeller att importera.</span><span class="sxs-lookup"><span data-stu-id="73fb3-143">DPS inspects the selected database for tables to import.</span></span> <span data-ttu-id="73fb3-144">Som standard importerar DPS alla tabeller i databasen.</span><span class="sxs-lookup"><span data-stu-id="73fb3-144">By default, DPS imports all the tables in the database.</span></span> <span data-ttu-id="73fb3-145">Du kan markera eller avmarkera tabeller genom att expandera länken Alla tabeller.</span><span class="sxs-lookup"><span data-stu-id="73fb3-145">You can select or deselect tables by expanding the All Tables link.</span></span> <span data-ttu-id="73fb3-146">Välj knappen Nästa för att gå vidare.</span><span class="sxs-lookup"><span data-stu-id="73fb3-146">Select the Next button to move forward.</span></span>

## <a name="step-5-choose-a-storage-account-to-stage-the-data"></a><span data-ttu-id="73fb3-147">Steg 5: Välj ett lagringskonto för att mellanlagra data</span><span class="sxs-lookup"><span data-stu-id="73fb3-147">Step 5: Choose a storage account to stage the data</span></span>
<span data-ttu-id="73fb3-148">DPS uppmanar dig att ange en plats där datan ska mellanlagras.</span><span class="sxs-lookup"><span data-stu-id="73fb3-148">DPS prompts you for a location to stage the data.</span></span> <span data-ttu-id="73fb3-149">Välj ett befintligt lagringskonto från din prenumeration och välj **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="73fb3-149">Choose an existing storage account from your subscription and select **Next**.</span></span>

> [!NOTE]
> <span data-ttu-id="73fb3-150">DPS skapar en ny blob-behållare i det valda lagringskontot och använder en specifik mapp för varje import.</span><span class="sxs-lookup"><span data-stu-id="73fb3-150">DPS will create a new blob container in the chosen storage account and use a distinct folder for each import.</span></span>
> 
> 

![][4]

## <a name="step-6-select-a-data-warehouse"></a><span data-ttu-id="73fb3-151">Steg 6: Välj ett informationslager</span><span class="sxs-lookup"><span data-stu-id="73fb3-151">Step 6: Select a data warehouse</span></span>
<span data-ttu-id="73fb3-152">Välj sedan en [Azure SQL Data Warehouse](http://aka.ms/sqldw)-onlinedatabas som du importerar datan till.</span><span class="sxs-lookup"><span data-stu-id="73fb3-152">Next, you select an online [Azure SQL Data Warehouse](http://aka.ms/sqldw) database to import the data into.</span></span> <span data-ttu-id="73fb3-153">När du har valt databasen måste du ange autentiseringsuppgifterna för att ansluta till databasen. Välj sedan **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="73fb3-153">Once you've selected your database, you need to enter the credentials to connect to the database and select **Next**.</span></span>

![][5]

> [!NOTE]
> <span data-ttu-id="73fb3-154">DPS slår samman källdatatabellerna med informationslagret.</span><span class="sxs-lookup"><span data-stu-id="73fb3-154">DPS merges the source data tables into the data warehouse.</span></span> <span data-ttu-id="73fb3-155">DPS varnar dig om tabellnamnet kräver att de befintliga tabellerna i informationslagret skrivs över.</span><span class="sxs-lookup"><span data-stu-id="73fb3-155">DPS warns you if the table name requires it to overwrite existing tables in the data warehouse.</span></span> <span data-ttu-id="73fb3-156">Om du vill kan du ta bort eventuella befintliga objekt i informationslagret genom att markera alternativet för att ta bort alla befintliga objekt före import.</span><span class="sxs-lookup"><span data-stu-id="73fb3-156">You may optionally delete any existing objects in the data warehouse by ticking Delete all existing objects before import.</span></span>
> 
> 

## <a name="step-7-import-the-data"></a><span data-ttu-id="73fb3-157">Steg 7: Importera data</span><span class="sxs-lookup"><span data-stu-id="73fb3-157">Step 7: Import the data</span></span>
<span data-ttu-id="73fb3-158">DPS bekräftar att du vill importera datan.</span><span class="sxs-lookup"><span data-stu-id="73fb3-158">DPS confirms that you would like to import the data.</span></span> <span data-ttu-id="73fb3-159">Klicka bara på knappen Starta import för att påbörja dataimporten.</span><span class="sxs-lookup"><span data-stu-id="73fb3-159">Simply click the Start import button to begin the data import.</span></span>

![][6]

<span data-ttu-id="73fb3-160">I DPS visas förloppet för extraheringen och dataöverföringen från den lokala SQL Server-databasen samt förloppet för import till SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="73fb3-160">DPS displays a visualization that shows the progress of extracting and uploading the data from the on-premises SQL Server and the progress of the import into SQL Data Warehouse.</span></span>

![][7]

<span data-ttu-id="73fb3-161">När importen är slutförd får du en sammanfattning av dataimporten och en ändringsrapport för kompatibilitetskorrigeringarna som har utförts.</span><span class="sxs-lookup"><span data-stu-id="73fb3-161">Once the import is complete, DPS displays a summary of the data import and a change report of the compatibility fixes that have been performed.</span></span>

![][8]

## <a name="next-steps"></a><span data-ttu-id="73fb3-162">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="73fb3-162">Next steps</span></span>
<span data-ttu-id="73fb3-163">Om du vill utforska dina data mer i SQL Data Warehouse kan du börja med att läsa följande:</span><span class="sxs-lookup"><span data-stu-id="73fb3-163">To explore your data within SQL Data Warehouse, start by viewing:</span></span>

* <span data-ttu-id="73fb3-164">[Fråga Azure SQL Data Warehouse (Visual Studio)][Query Azure SQL Data Warehouse (Visual Studio)]</span><span class="sxs-lookup"><span data-stu-id="73fb3-164">[Query Azure SQL Data Warehouse (Visual Studio)][Query Azure SQL Data Warehouse (Visual Studio)]</span></span>
* <span data-ttu-id="73fb3-165">[Visualisera data med Power BI][Visualize data with Power BI]</span><span class="sxs-lookup"><span data-stu-id="73fb3-165">[Visualize data with Power BI][Visualize data with Power BI]</span></span>

<span data-ttu-id="73fb3-166">Om du vill veta mer om Redgates Data Platform Studio:</span><span class="sxs-lookup"><span data-stu-id="73fb3-166">To learn more about Redgate’s Data Platform Studio:</span></span>

* [<span data-ttu-id="73fb3-167">Besök startsidan för DPS</span><span class="sxs-lookup"><span data-stu-id="73fb3-167">Visit the DPS homepage</span></span>](http://www.dataplatformstudio.com/)
* [<span data-ttu-id="73fb3-168">Titta på en demonstration av DPS på Channel9</span><span class="sxs-lookup"><span data-stu-id="73fb3-168">Watch a demo of DPS on Channel9</span></span>](https://channel9.msdn.com/Blogs/cloud-with-a-silver-lining/Loading-data-into-Azure-SQL-Datawarehouse-with-Redgate-Data-Platform-Studio)

<span data-ttu-id="73fb3-169">Om du vill ha en översikt över andra sätt att migrera och läsa in dina data i SQL Data Warehouse kan du se:</span><span class="sxs-lookup"><span data-stu-id="73fb3-169">For an overview of other ways to migrate and load your data in SQL Data Warehouse see:</span></span>

* <span data-ttu-id="73fb3-170">[Migrera lösningen till SQL Data Warehouse][Migrate your solution to SQL Data Warehouse]</span><span class="sxs-lookup"><span data-stu-id="73fb3-170">[Migrate your solution to SQL Data Warehouse][Migrate your solution to SQL Data Warehouse]</span></span>
* [<span data-ttu-id="73fb3-171">Läs in data till Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="73fb3-171">Load data into Azure SQL Data Warehouse</span></span>](sql-data-warehouse-overview-load.md)

<span data-ttu-id="73fb3-172">Om du vill ha fler utvecklingstips kan du se [Översikt över SQL Data Warehouse-utveckling](sql-data-warehouse-overview-develop.md).</span><span class="sxs-lookup"><span data-stu-id="73fb3-172">For more development tips, see the [SQL Data Warehouse development overview](sql-data-warehouse-overview-develop.md).</span></span>

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
[Migrate your solution to SQL Data Warehouse]: ./sql-data-warehouse-overview-migrate.md
[Load data into Azure SQL Data Warehouse]: ./sql-data-warehouse-overview-load.md
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md
