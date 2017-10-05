---
title: "Läs in data från SQL Server till Azure SQL Data Warehouse (SSIS) | Microsoft Docs"
description: "Visar hur du skapar ett paket för SQL Server Integration Services (SSIS) för att flytta data från en mängd olika datakällor till SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
ms.assetid: e2c252e9-0828-47c2-a808-e3bea46c134a
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 03/30/2017
ms.author: cakarst;douglasl;barbkess
ms.openlocfilehash: 6c9cebdd715b6997d0633bc725a3945ba9e0c357
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-ssis"></a><span data-ttu-id="369c9-103">Läs in data från SQL Server till Azure SQL Data Warehouse (SSIS)</span><span class="sxs-lookup"><span data-stu-id="369c9-103">Load data from SQL Server into Azure SQL Data Warehouse (SSIS)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="369c9-104">SSIS</span><span class="sxs-lookup"><span data-stu-id="369c9-104">SSIS</span></span>](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
> * [<span data-ttu-id="369c9-105">PolyBase</span><span class="sxs-lookup"><span data-stu-id="369c9-105">PolyBase</span></span>](sql-data-warehouse-load-from-sql-server-with-polybase.md)
> * [<span data-ttu-id="369c9-106">bcp</span><span class="sxs-lookup"><span data-stu-id="369c9-106">bcp</span></span>](sql-data-warehouse-load-from-sql-server-with-bcp.md)
> 
> 

<span data-ttu-id="369c9-107">Skapa ett SQL Server Integration Services (SSIS)-paket för att läsa in data från SQL Server till Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="369c9-107">Create a SQL Server Integration Services (SSIS) package to load data from SQL Server into Azure SQL Data Warehouse.</span></span> <span data-ttu-id="369c9-108">Du kan alternativt omstrukturera, transformera och rensa data som överförs via SSIS-dataflöde.</span><span class="sxs-lookup"><span data-stu-id="369c9-108">You can optionally restructure, transform, and cleanse the data as it passes through the SSIS data flow.</span></span>

<span data-ttu-id="369c9-109">I den här kursen ska du:</span><span class="sxs-lookup"><span data-stu-id="369c9-109">In this tutorial, you will:</span></span>

* <span data-ttu-id="369c9-110">Skapa ett nytt Integration Services-projekt i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="369c9-110">Create a new Integration Services project in Visual Studio.</span></span>
* <span data-ttu-id="369c9-111">Ansluta till datakällor, inklusive SQL Server (som en källa) och SQL Data Warehouse (som mål).</span><span class="sxs-lookup"><span data-stu-id="369c9-111">Connect to data sources, including SQL Server (as a source) and SQL Data Warehouse (as a destination).</span></span>
* <span data-ttu-id="369c9-112">Utforma ett SSIS-paket som läser in data från källan till målet.</span><span class="sxs-lookup"><span data-stu-id="369c9-112">Design an SSIS package that loads data from the source into the destination.</span></span>
* <span data-ttu-id="369c9-113">Kör SSIS-paket för att läsa in data.</span><span class="sxs-lookup"><span data-stu-id="369c9-113">Run the SSIS package to load the data.</span></span>

<span data-ttu-id="369c9-114">Den här kursen använder SQL Server som datakällan.</span><span class="sxs-lookup"><span data-stu-id="369c9-114">This tutorial uses SQL Server as the data source.</span></span> <span data-ttu-id="369c9-115">SQL Server kan köras lokalt eller i en virtuell Azure-dator.</span><span class="sxs-lookup"><span data-stu-id="369c9-115">SQL Server could be running on premises or in an Azure virtual machine.</span></span>

## <a name="basic-concepts"></a><span data-ttu-id="369c9-116">Grundläggande begrepp</span><span class="sxs-lookup"><span data-stu-id="369c9-116">Basic concepts</span></span>
<span data-ttu-id="369c9-117">Paketet är arbetsenheten i SSIS.</span><span class="sxs-lookup"><span data-stu-id="369c9-117">The package is the unit of work in SSIS.</span></span> <span data-ttu-id="369c9-118">Relaterade paket grupperas i projekt.</span><span class="sxs-lookup"><span data-stu-id="369c9-118">Related packages are grouped in projects.</span></span> <span data-ttu-id="369c9-119">Du skapar projekt och design paket i Visual Studio med SQL Server Data Tools.</span><span class="sxs-lookup"><span data-stu-id="369c9-119">You create projects and design packages in Visual Studio with SQL Server Data Tools.</span></span> <span data-ttu-id="369c9-120">Designprocessen är en visuell process där du dra komponenter från verktygslådan till designytan, ansluter dem, och ange deras egenskaper.</span><span class="sxs-lookup"><span data-stu-id="369c9-120">The design process is a visual process in which you drag and drop components from the Toolbox to the design surface, connect them, and set their properties.</span></span> <span data-ttu-id="369c9-121">När du har ditt paket kan distribuera du om du vill den till SQL Server för omfattande hantering, övervakning och säkerhet.</span><span class="sxs-lookup"><span data-stu-id="369c9-121">After you finish your package, you can optionally deploy it to SQL Server for comprehensive management, monitoring, and security.</span></span>

## <a name="options-for-loading-data-with-ssis"></a><span data-ttu-id="369c9-122">Alternativ för att läsa in data med SSIS</span><span class="sxs-lookup"><span data-stu-id="369c9-122">Options for loading data with SSIS</span></span>
<span data-ttu-id="369c9-123">SQL Server Integration Services (SSIS) är en flexibel uppsättning verktyg som ger en mängd olika alternativ för att ansluta till och läsa in data i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="369c9-123">SQL Server Integration Services (SSIS) is a flexible set of tools that provides a variety of options for connecting to, and loading data into, SQL Data Warehouse.</span></span>

1. <span data-ttu-id="369c9-124">Använd ett ADO NET mål för att ansluta till SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="369c9-124">Use an ADO NET Destination to connect to SQL Data Warehouse.</span></span> <span data-ttu-id="369c9-125">Den här kursen använder ett ADO NET mål eftersom den har minst antal konfigurationsalternativ.</span><span class="sxs-lookup"><span data-stu-id="369c9-125">This tutorial uses an ADO NET Destination because it has the fewest configuration options.</span></span>
2. <span data-ttu-id="369c9-126">Använd en OLE DB-mål för att ansluta till SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="369c9-126">Use an OLE DB Destination to connect to SQL Data Warehouse.</span></span> <span data-ttu-id="369c9-127">Det här alternativet kan ge något bättre prestanda än ADO NET målet.</span><span class="sxs-lookup"><span data-stu-id="369c9-127">This option may provide slightly better performance than the ADO NET Destination.</span></span>
3. <span data-ttu-id="369c9-128">Använd Azure Blob överför aktiviteten för att mellanlagra data i Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="369c9-128">Use the Azure Blob Upload Task to stage the data in Azure Blob Storage.</span></span> <span data-ttu-id="369c9-129">Använd sedan SSIS kör SQL-uppgiften för att starta en Polybase-skript som läser in data i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="369c9-129">Then use the SSIS Execute SQL task to launch a Polybase script that loads the data into SQL Data Warehouse.</span></span> <span data-ttu-id="369c9-130">Det här alternativet ger den bästa prestandan av tre alternativ som visas här.</span><span class="sxs-lookup"><span data-stu-id="369c9-130">This option provides the best performance of the three options listed here.</span></span> <span data-ttu-id="369c9-131">För att få ladda upp Azure Blob-aktiviteten, hämtar den [Microsoft SQL Server 2016 Integration Services Feature Pack för Azure][Microsoft SQL Server 2016 Integration Services Feature Pack for Azure].</span><span class="sxs-lookup"><span data-stu-id="369c9-131">To get the Azure Blob Upload task, download the [Microsoft SQL Server 2016 Integration Services Feature Pack for Azure][Microsoft SQL Server 2016 Integration Services Feature Pack for Azure].</span></span> <span data-ttu-id="369c9-132">Läs mer om Polybase i [PolyBase-guiden][PolyBase Guide].</span><span class="sxs-lookup"><span data-stu-id="369c9-132">To learn more about Polybase, see [PolyBase Guide][PolyBase Guide].</span></span>

## <a name="before-you-start"></a><span data-ttu-id="369c9-133">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="369c9-133">Before you start</span></span>
<span data-ttu-id="369c9-134">För att gå igenom de här självstudierna, behöver du:</span><span class="sxs-lookup"><span data-stu-id="369c9-134">To step through this tutorial, you need:</span></span>

1. <span data-ttu-id="369c9-135">**SQL Server Integration Services (SSIS)**.</span><span class="sxs-lookup"><span data-stu-id="369c9-135">**SQL Server Integration Services (SSIS)**.</span></span> <span data-ttu-id="369c9-136">SSIS är en komponent i SQL Server och kräver en utvärderingsversion eller en licensierad version av SQL Server.</span><span class="sxs-lookup"><span data-stu-id="369c9-136">SSIS is a component of SQL Server and requires an evaluation version or a licensed version of SQL Server.</span></span> <span data-ttu-id="369c9-137">Om du vill hämta en utvärderingsversion av SQL Server 2016 Preview finns [SQL Server-utvärderingar][SQL Server Evaluations].</span><span class="sxs-lookup"><span data-stu-id="369c9-137">To get an evaluation version of SQL Server 2016 Preview, see [SQL Server Evaluations][SQL Server Evaluations].</span></span>
2. <span data-ttu-id="369c9-138">**Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="369c9-138">**Visual Studio**.</span></span> <span data-ttu-id="369c9-139">Kostnadsfri Visual Studio Community Edition finns [Visual Studio Community][Visual Studio Community].</span><span class="sxs-lookup"><span data-stu-id="369c9-139">To get the free Visual Studio Community Edition, see [Visual Studio Community][Visual Studio Community].</span></span>
3. <span data-ttu-id="369c9-140">**SQL Server Data Tools för Visual Studio (SSDT)**.</span><span class="sxs-lookup"><span data-stu-id="369c9-140">**SQL Server Data Tools for Visual Studio (SSDT)**.</span></span> <span data-ttu-id="369c9-141">SQL Server Data Tools för Visual Studio finns [Hämta SQL Server Data Tools (SSDT)][Download SQL Server Data Tools (SSDT)].</span><span class="sxs-lookup"><span data-stu-id="369c9-141">To get SQL Server Data Tools for Visual Studio, see [Download SQL Server Data Tools (SSDT)][Download SQL Server Data Tools (SSDT)].</span></span>
4. <span data-ttu-id="369c9-142">**Exempeldata**.</span><span class="sxs-lookup"><span data-stu-id="369c9-142">**Sample data**.</span></span> <span data-ttu-id="369c9-143">Den här kursen använder exempeldata som lagras i SQL Server i exempeldatabasen AdventureWorks som källdata som ska läsas in i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="369c9-143">This tutorial uses sample data stored in SQL Server in the AdventureWorks sample database as the source data to be loaded into SQL Data Warehouse.</span></span> <span data-ttu-id="369c9-144">För att få exempeldatabasen AdventureWorks finns [AdventureWorks exempeldatabas för 2014][AdventureWorks 2014 Sample Databases].</span><span class="sxs-lookup"><span data-stu-id="369c9-144">To get the AdventureWorks sample database, see [AdventureWorks 2014 Sample Databases][AdventureWorks 2014 Sample Databases].</span></span>
5. <span data-ttu-id="369c9-145">**En SQL Data Warehouse-databas och behörigheter**.</span><span class="sxs-lookup"><span data-stu-id="369c9-145">**A SQL Data Warehouse database and permissions**.</span></span> <span data-ttu-id="369c9-146">Den här självstudiekursen ansluter till en instans av SQL Data Warehouse och läser in data i den.</span><span class="sxs-lookup"><span data-stu-id="369c9-146">This tutorial connects to a SQL Data Warehouse instance and loads data into it.</span></span> <span data-ttu-id="369c9-147">Du måste ha behörigheter att skapa en tabell och läsa in data.</span><span class="sxs-lookup"><span data-stu-id="369c9-147">You have to have permissions to create a table and to load data.</span></span>
6. <span data-ttu-id="369c9-148">**En brandväggsregel**.</span><span class="sxs-lookup"><span data-stu-id="369c9-148">**A firewall rule**.</span></span> <span data-ttu-id="369c9-149">Du måste skapa en brandväggsregel på SQL Data Warehouse med IP-adressen för den lokala datorn innan du kan överföra data till SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="369c9-149">You have to create a firewall rule on SQL Data Warehouse with the IP address of your local computer before you can upload data to the SQL Data Warehouse.</span></span>

## <a name="step-1-create-a-new-integration-services-project"></a><span data-ttu-id="369c9-150">Steg 1: Skapa ett nytt Integration Services-projekt</span><span class="sxs-lookup"><span data-stu-id="369c9-150">Step 1: Create a new Integration Services project</span></span>
1. <span data-ttu-id="369c9-151">Starta Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="369c9-151">Launch Visual Studio.</span></span>
2. <span data-ttu-id="369c9-152">På den **filen** väljer du **ny | Projektet**.</span><span class="sxs-lookup"><span data-stu-id="369c9-152">On the **File** menu, select **New | Project**.</span></span>
3. <span data-ttu-id="369c9-153">Navigera till den **installerat | Mallar | Business Intelligence | Integration Services** projekttyper.</span><span class="sxs-lookup"><span data-stu-id="369c9-153">Navigate to the **Installed | Templates | Business Intelligence | Integration Services** project types.</span></span>
4. <span data-ttu-id="369c9-154">Välj **Integration Services-projekt**.</span><span class="sxs-lookup"><span data-stu-id="369c9-154">Select **Integration Services Project**.</span></span> <span data-ttu-id="369c9-155">Ange värden för **namn** och **plats**, och välj sedan **OK**.</span><span class="sxs-lookup"><span data-stu-id="369c9-155">Provide values for **Name** and **Location**, and then select **OK**.</span></span>

<span data-ttu-id="369c9-156">Visual Studio öppnas och skapar ett nytt projekt för Integration Services (SSIS).</span><span class="sxs-lookup"><span data-stu-id="369c9-156">Visual Studio opens and creates a new Integration Services (SSIS) project.</span></span> <span data-ttu-id="369c9-157">Visual Studio öppnas designer för det enda nya SSIS-paketet (Package.dtsx) i projektet.</span><span class="sxs-lookup"><span data-stu-id="369c9-157">Then Visual Studio opens the designer for the single new SSIS package (Package.dtsx) in the project.</span></span> <span data-ttu-id="369c9-158">Du kan se på följande områden:</span><span class="sxs-lookup"><span data-stu-id="369c9-158">You see the following screen areas:</span></span>

* <span data-ttu-id="369c9-159">Till vänster i **verktygslådan** SSIS-komponenter.</span><span class="sxs-lookup"><span data-stu-id="369c9-159">On the left, the **Toolbox** of SSIS components.</span></span>
* <span data-ttu-id="369c9-160">I mitten designytan med flera flikar.</span><span class="sxs-lookup"><span data-stu-id="369c9-160">In the middle, the design surface, with multiple tabs.</span></span> <span data-ttu-id="369c9-161">Normalt använder du minst den **Kontrollflöde** och **dataflöde** flikar.</span><span class="sxs-lookup"><span data-stu-id="369c9-161">You typically use at least the **Control Flow** and the **Data Flow** tabs.</span></span>
* <span data-ttu-id="369c9-162">Till höger i **Solution Explorer** och **egenskaper** fönster.</span><span class="sxs-lookup"><span data-stu-id="369c9-162">On the right, the **Solution Explorer** and the **Properties** panes.</span></span>
  
    ![][01]

## <a name="step-2-create-the-basic-data-flow"></a><span data-ttu-id="369c9-163">Steg 2: Skapa grundläggande dataflödet</span><span class="sxs-lookup"><span data-stu-id="369c9-163">Step 2: Create the basic data flow</span></span>
1. <span data-ttu-id="369c9-164">Dra en Data flödar aktivitet från verktygslådan till mitten av designytan (på den **Kontrollflöde** fliken).</span><span class="sxs-lookup"><span data-stu-id="369c9-164">Drag a Data Flow Task from the Toolbox to the center of the design surface (on the **Control Flow** tab).</span></span>
   
    ![][02]
2. <span data-ttu-id="369c9-165">Dubbelklicka på aktiviteten flöda Data för att gå till fliken dataflöde.</span><span class="sxs-lookup"><span data-stu-id="369c9-165">Double-click the Data Flow Task to switch to the Data Flow tab.</span></span>
3. <span data-ttu-id="369c9-166">Dra en ADO.NET-datakälla från listan andra källor i verktygslådan till designytan.</span><span class="sxs-lookup"><span data-stu-id="369c9-166">From the Other Sources list in the Toolbox, drag an ADO.NET Source to the design surface.</span></span> <span data-ttu-id="369c9-167">Käll-kort fortfarande markerat, ändra dess namn till **SQL Server-datakälla** i den **egenskaper** fönstret.</span><span class="sxs-lookup"><span data-stu-id="369c9-167">With the source adapter still selected, change its name to **SQL Server source** in the **Properties** pane.</span></span>
4. <span data-ttu-id="369c9-168">Dra en ADO.NET-mål från listan andra mål i verktygslådan till designytan under ADO.NET-källa.</span><span class="sxs-lookup"><span data-stu-id="369c9-168">From the Other Destinations list in the Toolbox, drag an ADO.NET Destination to the design surface under the ADO.NET Source.</span></span> <span data-ttu-id="369c9-169">Mål-kort fortfarande markerat, ändra dess namn till **SQL DW mål** i den **egenskaper** fönstret.</span><span class="sxs-lookup"><span data-stu-id="369c9-169">With the destination adapter still selected, change its name to **SQL DW destination** in the **Properties** pane.</span></span>
   
    ![][09]

## <a name="step-3-configure-the-source-adapter"></a><span data-ttu-id="369c9-170">Steg 3: Konfigurera käll-kort</span><span class="sxs-lookup"><span data-stu-id="369c9-170">Step 3: Configure the source adapter</span></span>
1. <span data-ttu-id="369c9-171">Dubbelklickar du på käll-kortet för att öppna den **ADO.NET källa Editor**.</span><span class="sxs-lookup"><span data-stu-id="369c9-171">Double-click the source adapter to open the **ADO.NET Source Editor**.</span></span>
   
    ![][03]
2. <span data-ttu-id="369c9-172">På den **Connection Manager** för den **ADO.NET källa Editor**, klickar du på den **ny** knappen bredvid den **ADO.NET Anslutningshanteraren** lista Öppna den **konfigurera ADO.NET Anslutningshanteraren** dialogrutan och skapa anslutningsinställningar för SQL Server-databas som den här kursen läser in data.</span><span class="sxs-lookup"><span data-stu-id="369c9-172">On the **Connection Manager** tab of the **ADO.NET Source Editor**, click the **New** button next to the **ADO.NET connection manager** list to open the **Configure ADO.NET Connection Manager** dialog box and create connection settings for the SQL Server database from which this tutorial loads data.</span></span>
   
    ![][04]
3. <span data-ttu-id="369c9-173">I den **konfigurera ADO.NET Anslutningshanteraren** dialogrutan klickar du på den **ny** för att öppna den **Connection Manager** dialogrutan och skapa en ny anslutning.</span><span class="sxs-lookup"><span data-stu-id="369c9-173">In the **Configure ADO.NET Connection Manager** dialog box, click the **New** button to open the **Connection Manager** dialog box and create a new data connection.</span></span>
   
    ![][05]
4. <span data-ttu-id="369c9-174">I den **Connection Manager** dialogrutan Gör följande saker.</span><span class="sxs-lookup"><span data-stu-id="369c9-174">In the **Connection Manager** dialog box, do the following things.</span></span>
   
   1. <span data-ttu-id="369c9-175">För **Provider**, Välj SqlClient-dataprovidern.</span><span class="sxs-lookup"><span data-stu-id="369c9-175">For **Provider**, select the SqlClient Data Provider.</span></span>
   2. <span data-ttu-id="369c9-176">För **servernamn**, ange namnet på SQL Server.</span><span class="sxs-lookup"><span data-stu-id="369c9-176">For **Server name**, enter the SQL Server name.</span></span>
   3. <span data-ttu-id="369c9-177">I den **logga in på servern** väljer eller ange autentiseringsinformation.</span><span class="sxs-lookup"><span data-stu-id="369c9-177">In the **Log on to the server** section, select or enter authentication information.</span></span>
   4. <span data-ttu-id="369c9-178">I den **Anslut till en databas** väljer exempeldatabasen AdventureWorks.</span><span class="sxs-lookup"><span data-stu-id="369c9-178">In the **Connect to a database** section, select the AdventureWorks sample database.</span></span>
   5. <span data-ttu-id="369c9-179">Klicka på **Anslutningstestet**.</span><span class="sxs-lookup"><span data-stu-id="369c9-179">Click **Test Connection**.</span></span>
      
       ![][06]
   6. <span data-ttu-id="369c9-180">Klicka i dialogrutan som rapporterar resultatet av Anslutningstestet **OK** att återgå till den **Connection Manager** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="369c9-180">In the dialog box that reports the results of the connection test, click **OK** to return to the **Connection Manager** dialog box.</span></span>
   7. <span data-ttu-id="369c9-181">I den **Connection Manager** dialogrutan klickar du på **OK** att återgå till den **konfigurera ADO.NET Anslutningshanteraren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="369c9-181">In the **Connection Manager** dialog box, click **OK** to return to the **Configure ADO.NET Connection Manager** dialog box.</span></span>
5. <span data-ttu-id="369c9-182">I den **konfigurera ADO.NET Anslutningshanteraren** dialogrutan klickar du på **OK** att återgå till den **ADO.NET källa Editor**.</span><span class="sxs-lookup"><span data-stu-id="369c9-182">In the **Configure ADO.NET Connection Manager** dialog box, click **OK** to return to the **ADO.NET Source Editor**.</span></span>
6. <span data-ttu-id="369c9-183">I den **ADO.NET källa Editor**i den **namnet på tabellen eller vyn** väljer den **Sales.SalesOrderDetail** tabell.</span><span class="sxs-lookup"><span data-stu-id="369c9-183">In the **ADO.NET Source Editor**, in the **Name of the table or the view** list, select the **Sales.SalesOrderDetail** table.</span></span>
   
    ![][07]
7. <span data-ttu-id="369c9-184">Klicka på **Preview** att se de första 200 raderna med data i källtabellen i den **Preview frågeresultat** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="369c9-184">Click **Preview** to see the first 200 rows of data in the source table in the **Preview Query Results** dialog box.</span></span>
   
    ![][08]
8. <span data-ttu-id="369c9-185">I den **Preview frågeresultat** dialogrutan klickar du på **Stäng** att återgå till den **ADO.NET källa Editor**.</span><span class="sxs-lookup"><span data-stu-id="369c9-185">In the **Preview Query Results** dialog box, click **Close** to return to the **ADO.NET Source Editor**.</span></span>
9. <span data-ttu-id="369c9-186">I den **ADO.NET källa Editor**, klickar du på **OK** till Slutför konfigurationen av datakällan.</span><span class="sxs-lookup"><span data-stu-id="369c9-186">In the **ADO.NET Source Editor**, click **OK** to finish configuring the data source.</span></span>

## <a name="step-4-connect-the-source-adapter-to-the-destination-adapter"></a><span data-ttu-id="369c9-187">Steg 4: Anslut käll-kortet till målnätverkskort</span><span class="sxs-lookup"><span data-stu-id="369c9-187">Step 4: Connect the source adapter to the destination adapter</span></span>
1. <span data-ttu-id="369c9-188">Välj källa kortet i designvyn.</span><span class="sxs-lookup"><span data-stu-id="369c9-188">Select the source adapter on the design surface.</span></span>
2. <span data-ttu-id="369c9-189">Välj en blå pil som sträcker sig från käll-kort och drar den till mål-redigeraren förrän den har fästs på plats.</span><span class="sxs-lookup"><span data-stu-id="369c9-189">Select the blue arrow that extends from the source adapter and drag it to the destination editor until it snaps into place.</span></span>
   
    ![][10]
   
    <span data-ttu-id="369c9-190">I en typisk SSIS-paket använder du ett antal andra komponenter från verktygslådan SSIS mellan källan och målet att omstrukturera, transformera och rensa data som överförs via SSIS-dataflöde.</span><span class="sxs-lookup"><span data-stu-id="369c9-190">In a typical SSIS package, you use a number of other components from the SSIS Toolbox in between the source and the destination to restructure, transform, and cleanse your data as it passes through the SSIS data flow.</span></span> <span data-ttu-id="369c9-191">Om du vill behålla det här exemplet så enkel som möjligt, ansluter vi källan direkt till målet.</span><span class="sxs-lookup"><span data-stu-id="369c9-191">To keep this example as simple as possible, we’re connecting the source directly to the destination.</span></span>

## <a name="step-5-configure-the-destination-adapter"></a><span data-ttu-id="369c9-192">Steg 5: Konfigurera målnätverkskort</span><span class="sxs-lookup"><span data-stu-id="369c9-192">Step 5: Configure the destination adapter</span></span>
1. <span data-ttu-id="369c9-193">Dubbelklicka på målnätverkskort att öppna den **ADO.NET mål Editor**.</span><span class="sxs-lookup"><span data-stu-id="369c9-193">Double-click the destination adapter to open the **ADO.NET Destination Editor**.</span></span>
   
    ![][11]
2. <span data-ttu-id="369c9-194">På den **Connection Manager** för den **ADO.NET mål Editor**, klickar du på den **ny** knappen bredvid den **Anslutningshanteraren** listan till Öppna den **konfigurera ADO.NET Anslutningshanteraren** dialogrutan och skapa anslutningsinställningar för Azure SQL Data Warehouse-databas som den här kursen läser in data.</span><span class="sxs-lookup"><span data-stu-id="369c9-194">On the **Connection Manager** tab of the **ADO.NET Destination Editor**, click the **New** button next to the **Connection manager** list to open the **Configure ADO.NET Connection Manager** dialog box and create connection settings for the Azure SQL Data Warehouse database into which this tutorial loads data.</span></span>
3. <span data-ttu-id="369c9-195">I den **konfigurera ADO.NET Anslutningshanteraren** dialogrutan klickar du på den **ny** för att öppna den **Connection Manager** dialogrutan och skapa en ny anslutning.</span><span class="sxs-lookup"><span data-stu-id="369c9-195">In the **Configure ADO.NET Connection Manager** dialog box, click the **New** button to open the **Connection Manager** dialog box and create a new data connection.</span></span>
4. <span data-ttu-id="369c9-196">I den **Connection Manager** dialogrutan Gör följande saker.</span><span class="sxs-lookup"><span data-stu-id="369c9-196">In the **Connection Manager** dialog box, do the following things.</span></span>
   1. <span data-ttu-id="369c9-197">För **Provider**, Välj SqlClient-dataprovidern.</span><span class="sxs-lookup"><span data-stu-id="369c9-197">For **Provider**, select the SqlClient Data Provider.</span></span>
   2. <span data-ttu-id="369c9-198">För **servernamn**, anger du namnet på SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="369c9-198">For **Server name**, enter the SQL Data Warehouse name.</span></span>
   3. <span data-ttu-id="369c9-199">I den **logga in på servern** väljer **används SQL Server-autentisering** och ange autentiseringsinformation.</span><span class="sxs-lookup"><span data-stu-id="369c9-199">In the **Log on to the server** section, select **Use SQL Server authentication** and enter authentication information.</span></span>
   4. <span data-ttu-id="369c9-200">I den **Anslut till en databas** väljer du en befintlig SQL Data Warehouse-databas.</span><span class="sxs-lookup"><span data-stu-id="369c9-200">In the **Connect to a database** section, select an existing SQL Data Warehouse database.</span></span>
   5. <span data-ttu-id="369c9-201">Klicka på **Anslutningstestet**.</span><span class="sxs-lookup"><span data-stu-id="369c9-201">Click **Test Connection**.</span></span>
   6. <span data-ttu-id="369c9-202">Klicka i dialogrutan som rapporterar resultatet av Anslutningstestet **OK** att återgå till den **Connection Manager** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="369c9-202">In the dialog box that reports the results of the connection test, click **OK** to return to the **Connection Manager** dialog box.</span></span>
   7. <span data-ttu-id="369c9-203">I den **Connection Manager** dialogrutan klickar du på **OK** att återgå till den **konfigurera ADO.NET Anslutningshanteraren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="369c9-203">In the **Connection Manager** dialog box, click **OK** to return to the **Configure ADO.NET Connection Manager** dialog box.</span></span>
5. <span data-ttu-id="369c9-204">I den **konfigurera ADO.NET Anslutningshanteraren** dialogrutan klickar du på **OK** att återgå till den **ADO.NET mål Editor**.</span><span class="sxs-lookup"><span data-stu-id="369c9-204">In the **Configure ADO.NET Connection Manager** dialog box, click **OK** to return to the **ADO.NET Destination Editor**.</span></span>
6. <span data-ttu-id="369c9-205">I den **ADO.NET mål Editor**, klickar du på **ny** bredvid den **använder en tabell eller vy** att öppna den **Create Table** dialogrutan för att skapa en nya måltabellen med en kolumnlista som matchar källtabellen.</span><span class="sxs-lookup"><span data-stu-id="369c9-205">In the **ADO.NET Destination Editor**, click **New** next to the **Use a table or view** list to open the **Create Table** dialog box to create a new destination table with a column list that matches the source table.</span></span>
   
    ![][12a]
7. <span data-ttu-id="369c9-206">I den **Create Table** dialogrutan Gör följande saker.</span><span class="sxs-lookup"><span data-stu-id="369c9-206">In the **Create Table** dialog box, do the following things.</span></span>
   
   1. <span data-ttu-id="369c9-207">Ändra namnet på tabellen till **SalesOrderDetail**.</span><span class="sxs-lookup"><span data-stu-id="369c9-207">Change the name of the destination table to **SalesOrderDetail**.</span></span>
   2. <span data-ttu-id="369c9-208">Ta bort den **rowguid** kolumn.</span><span class="sxs-lookup"><span data-stu-id="369c9-208">Remove the **rowguid** column.</span></span> <span data-ttu-id="369c9-209">Den **uniqueidentifier** datatyp stöds inte i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="369c9-209">The **uniqueidentifier** data type is not supported in SQL Data Warehouse.</span></span>
   3. <span data-ttu-id="369c9-210">Ändra datatypen för den **LineTotal** kolumnen till **pengar**.</span><span class="sxs-lookup"><span data-stu-id="369c9-210">Change the data type of the **LineTotal** column to **money**.</span></span> <span data-ttu-id="369c9-211">Den **decimal** datatyp stöds inte i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="369c9-211">The **decimal** data type is not supported in SQL Data Warehouse.</span></span> <span data-ttu-id="369c9-212">Information om vilka datatyper finns [CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)][CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)].</span><span class="sxs-lookup"><span data-stu-id="369c9-212">For info about supported data types, see [CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)][CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)].</span></span>
      
       ![][12b]
   4. <span data-ttu-id="369c9-213">Klicka på **OK** att skapa tabellen och återgå till den **ADO.NET mål Editor**.</span><span class="sxs-lookup"><span data-stu-id="369c9-213">Click **OK** to create the table and return to the **ADO.NET Destination Editor**.</span></span>
8. <span data-ttu-id="369c9-214">I den **ADO.NET mål Editor**, Välj den **mappningar** fliken för att se hur kolumner i källans mappas till kolumnerna i mål.</span><span class="sxs-lookup"><span data-stu-id="369c9-214">In the **ADO.NET Destination Editor**, select the **Mappings** tab to see how columns in the source are mapped to columns in the destination.</span></span>
   
    ![][13]
9. <span data-ttu-id="369c9-215">Klicka på **OK** till Slutför konfigurationen av datakällan.</span><span class="sxs-lookup"><span data-stu-id="369c9-215">Click **OK** to finish configuring the data source.</span></span>

## <a name="step-6-run-the-package-to-load-the-data"></a><span data-ttu-id="369c9-216">Steg 6: Kör för att läsa in data</span><span class="sxs-lookup"><span data-stu-id="369c9-216">Step 6: Run the package to load the data</span></span>
<span data-ttu-id="369c9-217">Kör paketet genom att klicka på den **starta** knappen i verktygsfältet eller genom att välja något av de **kör** alternativ på den **felsöka** menyn.</span><span class="sxs-lookup"><span data-stu-id="369c9-217">Run the package by clicking the **Start** button on the toolbar or by selecting one of the **Run** options on the **Debug** menu.</span></span>

<span data-ttu-id="369c9-218">När paketet börjar köras, se gul snurrande hjul som visar aktivitet samt antalet bearbetade hittills rader.</span><span class="sxs-lookup"><span data-stu-id="369c9-218">As the package begins to run, you see yellow spinning wheels to indicate activity as well as the number of rows processed so far.</span></span>

![][14]

<span data-ttu-id="369c9-219">När paketet har körts kan du se grön bockmarkering som anger lyckad samt det totala antalet rader med data som har lästs in från källan till målet.</span><span class="sxs-lookup"><span data-stu-id="369c9-219">When the package has finished running, you see green check marks to indicate success as well as the total number of rows of data loaded from the source to the destination.</span></span>

![][15]

<span data-ttu-id="369c9-220">Grattis!</span><span class="sxs-lookup"><span data-stu-id="369c9-220">Congratulations!</span></span> <span data-ttu-id="369c9-221">Du har använt SQL Server Integration Services att läsa in data till Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="369c9-221">You’ve successfully used SQL Server Integration Services to load data into Azure SQL Data Warehouse.</span></span>

## <a name="next-steps"></a><span data-ttu-id="369c9-222">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="369c9-222">Next steps</span></span>
* <span data-ttu-id="369c9-223">Läs mer om SSIS-dataflöde.</span><span class="sxs-lookup"><span data-stu-id="369c9-223">Learn more about the SSIS data flow.</span></span> <span data-ttu-id="369c9-224">Börja här: [dataflöde][Data Flow].</span><span class="sxs-lookup"><span data-stu-id="369c9-224">Start here: [Data Flow][Data Flow].</span></span>
* <span data-ttu-id="369c9-225">Lär dig att felsöka dina paket direkt i design-miljön.</span><span class="sxs-lookup"><span data-stu-id="369c9-225">Learn how to debug and troubleshoot your packages right in the design environment.</span></span> <span data-ttu-id="369c9-226">Börja här: [felsökningsverktyg för paketet utveckling][Troubleshooting Tools for Package Development].</span><span class="sxs-lookup"><span data-stu-id="369c9-226">Start here: [Troubleshooting Tools for Package Development][Troubleshooting Tools for Package Development].</span></span>
* <span data-ttu-id="369c9-227">Lär dig mer om att distribuera SSIS-projekt och paket till Integration Services-servern eller en annan lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="369c9-227">Learn how to deploy your SSIS projects and packages to Integration Services Server or to another storage location.</span></span> <span data-ttu-id="369c9-228">Börja här: [projekt för distribution av och paket][Deployment of Projects and Packages].</span><span class="sxs-lookup"><span data-stu-id="369c9-228">Start here: [Deployment of Projects and Packages][Deployment of Projects and Packages].</span></span>

<!-- Image references -->
[01]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ssis-designer-01.png
[02]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ssis-data-flow-task-02.png
[03]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-source-03.png
[04]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-connection-manager-04.png
[05]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-connection-05.png
[06]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/test-connection-06.png
[07]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-source-07.png
[08]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/preview-data-08.png
[09]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/source-destination-09.png
[10]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/connect-source-destination-10.png
[11]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-destination-11.png
[12a]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/destination-query-before-12a.png
[12b]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/destination-query-after-12b.png
[13]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/column-mapping-13.png
[14]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/package-running-14.png
[15]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/package-success-15.png

<!-- Article references -->

<!-- MSDN references -->
[PolyBase Guide]: https://msdn.microsoft.com/library/mt143171.aspx
[Download SQL Server Data Tools (SSDT)]: https://msdn.microsoft.com/library/mt204009.aspx
[CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)]: https://msdn.microsoft.com/library/mt203953.aspx
[Data Flow]: https://msdn.microsoft.com/library/ms140080.aspx
[Troubleshooting Tools for Package Development]: https://msdn.microsoft.com/library/ms137625.aspx
[Deployment of Projects and Packages]: https://msdn.microsoft.com/library/hh213290.aspx

<!--Other Web references-->
[Microsoft SQL Server 2016 Integration Services Feature Pack for Azure]: http://go.microsoft.com/fwlink/?LinkID=626967
[SQL Server Evaluations]: https://www.microsoft.com/en-us/evalcenter/evaluate-sql-server-2016
[Visual Studio Community]: https://www.visualstudio.com/en-us/products/visual-studio-community-vs.aspx
[AdventureWorks 2014 Sample Databases]: https://msftdbprodsamples.codeplex.com/releases/view/125550
