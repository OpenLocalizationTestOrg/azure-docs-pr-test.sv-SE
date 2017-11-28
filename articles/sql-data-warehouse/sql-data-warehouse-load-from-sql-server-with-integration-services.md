---
title: "aaaLoad data från SQL Server till Azure SQL Data Warehouse (SSIS) | Microsoft Docs"
description: "Visar hur toocreate SQL Server Integration Services (SSIS) toomove paketdata från en mängd olika data datakällor tooSQL Data Warehouse."
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
ms.openlocfilehash: bb28a08807a5b07832b85f2f074c2acf912c1dc3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-ssis"></a><span data-ttu-id="a3b16-103">Läs in data från SQL Server till Azure SQL Data Warehouse (SSIS)</span><span class="sxs-lookup"><span data-stu-id="a3b16-103">Load data from SQL Server into Azure SQL Data Warehouse (SSIS)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="a3b16-104">SSIS</span><span class="sxs-lookup"><span data-stu-id="a3b16-104">SSIS</span></span>](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
> * [<span data-ttu-id="a3b16-105">PolyBase</span><span class="sxs-lookup"><span data-stu-id="a3b16-105">PolyBase</span></span>](sql-data-warehouse-load-from-sql-server-with-polybase.md)
> * [<span data-ttu-id="a3b16-106">bcp</span><span class="sxs-lookup"><span data-stu-id="a3b16-106">bcp</span></span>](sql-data-warehouse-load-from-sql-server-with-bcp.md)
> 
> 

<span data-ttu-id="a3b16-107">Skapa ett SQL Server Integration Services (SSIS) paketet tooload data från SQL Server till Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="a3b16-107">Create a SQL Server Integration Services (SSIS) package tooload data from SQL Server into Azure SQL Data Warehouse.</span></span> <span data-ttu-id="a3b16-108">Du kan alternativt omstrukturera, transformera och rensa hello data när det överförs via hello SSIS-dataflöde.</span><span class="sxs-lookup"><span data-stu-id="a3b16-108">You can optionally restructure, transform, and cleanse hello data as it passes through hello SSIS data flow.</span></span>

<span data-ttu-id="a3b16-109">I den här kursen ska du:</span><span class="sxs-lookup"><span data-stu-id="a3b16-109">In this tutorial, you will:</span></span>

* <span data-ttu-id="a3b16-110">Skapa ett nytt Integration Services-projekt i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a3b16-110">Create a new Integration Services project in Visual Studio.</span></span>
* <span data-ttu-id="a3b16-111">Ansluta toodata källor, till exempel SQL Server (som en källa) och SQL Data Warehouse (som mål).</span><span class="sxs-lookup"><span data-stu-id="a3b16-111">Connect toodata sources, including SQL Server (as a source) and SQL Data Warehouse (as a destination).</span></span>
* <span data-ttu-id="a3b16-112">Utforma ett SSIS-paket som läser in data från hello källa i hello mål.</span><span class="sxs-lookup"><span data-stu-id="a3b16-112">Design an SSIS package that loads data from hello source into hello destination.</span></span>
* <span data-ttu-id="a3b16-113">Kör hello SSIS-paket tooload hello data.</span><span class="sxs-lookup"><span data-stu-id="a3b16-113">Run hello SSIS package tooload hello data.</span></span>

<span data-ttu-id="a3b16-114">Den här kursen använder SQL Server som hello datakälla.</span><span class="sxs-lookup"><span data-stu-id="a3b16-114">This tutorial uses SQL Server as hello data source.</span></span> <span data-ttu-id="a3b16-115">SQL Server kan köras lokalt eller i en virtuell Azure-dator.</span><span class="sxs-lookup"><span data-stu-id="a3b16-115">SQL Server could be running on premises or in an Azure virtual machine.</span></span>

## <a name="basic-concepts"></a><span data-ttu-id="a3b16-116">Grundläggande begrepp</span><span class="sxs-lookup"><span data-stu-id="a3b16-116">Basic concepts</span></span>
<span data-ttu-id="a3b16-117">hello-paket är hello arbetsenheten i SSIS.</span><span class="sxs-lookup"><span data-stu-id="a3b16-117">hello package is hello unit of work in SSIS.</span></span> <span data-ttu-id="a3b16-118">Relaterade paket grupperas i projekt.</span><span class="sxs-lookup"><span data-stu-id="a3b16-118">Related packages are grouped in projects.</span></span> <span data-ttu-id="a3b16-119">Du skapar projekt och design paket i Visual Studio med SQL Server Data Tools.</span><span class="sxs-lookup"><span data-stu-id="a3b16-119">You create projects and design packages in Visual Studio with SQL Server Data Tools.</span></span> <span data-ttu-id="a3b16-120">hello design processen är en visuell process där du drar och släpper komponenter från hello verktygslådan toohello designytan, ansluter dem och ange deras egenskaper.</span><span class="sxs-lookup"><span data-stu-id="a3b16-120">hello design process is a visual process in which you drag and drop components from hello Toolbox toohello design surface, connect them, and set their properties.</span></span> <span data-ttu-id="a3b16-121">När du har ditt paket kan distribuera du om du vill den tooSQL Server för omfattande hantering, övervakning och säkerhet.</span><span class="sxs-lookup"><span data-stu-id="a3b16-121">After you finish your package, you can optionally deploy it tooSQL Server for comprehensive management, monitoring, and security.</span></span>

## <a name="options-for-loading-data-with-ssis"></a><span data-ttu-id="a3b16-122">Alternativ för att läsa in data med SSIS</span><span class="sxs-lookup"><span data-stu-id="a3b16-122">Options for loading data with SSIS</span></span>
<span data-ttu-id="a3b16-123">SQL Server Integration Services (SSIS) är en flexibel uppsättning verktyg som ger en mängd olika alternativ för att ansluta till och läsa in data i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="a3b16-123">SQL Server Integration Services (SSIS) is a flexible set of tools that provides a variety of options for connecting to, and loading data into, SQL Data Warehouse.</span></span>

1. <span data-ttu-id="a3b16-124">Använd en ADO NET mål tooconnect tooSQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="a3b16-124">Use an ADO NET Destination tooconnect tooSQL Data Warehouse.</span></span> <span data-ttu-id="a3b16-125">Den här kursen använder ett ADO NET mål eftersom den har hello minst antal konfigurationsalternativ.</span><span class="sxs-lookup"><span data-stu-id="a3b16-125">This tutorial uses an ADO NET Destination because it has hello fewest configuration options.</span></span>
2. <span data-ttu-id="a3b16-126">Använd en OLE DB mål tooconnect tooSQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="a3b16-126">Use an OLE DB Destination tooconnect tooSQL Data Warehouse.</span></span> <span data-ttu-id="a3b16-127">Det här alternativet kan ge något bättre prestanda än hello ADO NET mål.</span><span class="sxs-lookup"><span data-stu-id="a3b16-127">This option may provide slightly better performance than hello ADO NET Destination.</span></span>
3. <span data-ttu-id="a3b16-128">Använd hello Azure Blob överför toostage hello aktivitetsdata i Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="a3b16-128">Use hello Azure Blob Upload Task toostage hello data in Azure Blob Storage.</span></span> <span data-ttu-id="a3b16-129">Använd sedan hello SSIS kör SQL-uppgiften toolaunch en Polybase-skript som läses in hello data i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="a3b16-129">Then use hello SSIS Execute SQL task toolaunch a Polybase script that loads hello data into SQL Data Warehouse.</span></span> <span data-ttu-id="a3b16-130">Det här alternativet ger hello bästa prestanda av hello tre alternativ som visas här.</span><span class="sxs-lookup"><span data-stu-id="a3b16-130">This option provides hello best performance of hello three options listed here.</span></span> <span data-ttu-id="a3b16-131">tooget hello Azure Blob-överför aktiviteten hämta hello [Microsoft SQL Server 2016 Integration Services Feature Pack för Azure][Microsoft SQL Server 2016 Integration Services Feature Pack for Azure].</span><span class="sxs-lookup"><span data-stu-id="a3b16-131">tooget hello Azure Blob Upload task, download hello [Microsoft SQL Server 2016 Integration Services Feature Pack for Azure][Microsoft SQL Server 2016 Integration Services Feature Pack for Azure].</span></span> <span data-ttu-id="a3b16-132">toolearn mer om Polybase, se [PolyBase-guiden][PolyBase Guide].</span><span class="sxs-lookup"><span data-stu-id="a3b16-132">toolearn more about Polybase, see [PolyBase Guide][PolyBase Guide].</span></span>

## <a name="before-you-start"></a><span data-ttu-id="a3b16-133">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="a3b16-133">Before you start</span></span>
<span data-ttu-id="a3b16-134">toostep igenom den här kursen behöver du:</span><span class="sxs-lookup"><span data-stu-id="a3b16-134">toostep through this tutorial, you need:</span></span>

1. <span data-ttu-id="a3b16-135">**SQL Server Integration Services (SSIS)**.</span><span class="sxs-lookup"><span data-stu-id="a3b16-135">**SQL Server Integration Services (SSIS)**.</span></span> <span data-ttu-id="a3b16-136">SSIS är en komponent i SQL Server och kräver en utvärderingsversion eller en licensierad version av SQL Server.</span><span class="sxs-lookup"><span data-stu-id="a3b16-136">SSIS is a component of SQL Server and requires an evaluation version or a licensed version of SQL Server.</span></span> <span data-ttu-id="a3b16-137">tooget en utvärderingsversion av SQL Server 2016 Preview finns [SQL Server-utvärderingar][SQL Server Evaluations].</span><span class="sxs-lookup"><span data-stu-id="a3b16-137">tooget an evaluation version of SQL Server 2016 Preview, see [SQL Server Evaluations][SQL Server Evaluations].</span></span>
2. <span data-ttu-id="a3b16-138">**Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="a3b16-138">**Visual Studio**.</span></span> <span data-ttu-id="a3b16-139">tooget Hej kostnadsfri Visual Studio Community Edition, se [Visual Studio Community][Visual Studio Community].</span><span class="sxs-lookup"><span data-stu-id="a3b16-139">tooget hello free Visual Studio Community Edition, see [Visual Studio Community][Visual Studio Community].</span></span>
3. <span data-ttu-id="a3b16-140">**SQL Server Data Tools för Visual Studio (SSDT)**.</span><span class="sxs-lookup"><span data-stu-id="a3b16-140">**SQL Server Data Tools for Visual Studio (SSDT)**.</span></span> <span data-ttu-id="a3b16-141">tooget SQL Server Data Tools för Visual Studio finns [Hämta SQL Server Data Tools (SSDT)][Download SQL Server Data Tools (SSDT)].</span><span class="sxs-lookup"><span data-stu-id="a3b16-141">tooget SQL Server Data Tools for Visual Studio, see [Download SQL Server Data Tools (SSDT)][Download SQL Server Data Tools (SSDT)].</span></span>
4. <span data-ttu-id="a3b16-142">**Exempeldata**.</span><span class="sxs-lookup"><span data-stu-id="a3b16-142">**Sample data**.</span></span> <span data-ttu-id="a3b16-143">Den här kursen använder exempeldata som lagras i SQL Server i hello exempeldatabasen AdventureWorks som hello källa data toobe läses in i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="a3b16-143">This tutorial uses sample data stored in SQL Server in hello AdventureWorks sample database as hello source data toobe loaded into SQL Data Warehouse.</span></span> <span data-ttu-id="a3b16-144">tooget hello exempeldatabasen AdventureWorks finns [AdventureWorks exempeldatabas för 2014][AdventureWorks 2014 Sample Databases].</span><span class="sxs-lookup"><span data-stu-id="a3b16-144">tooget hello AdventureWorks sample database, see [AdventureWorks 2014 Sample Databases][AdventureWorks 2014 Sample Databases].</span></span>
5. <span data-ttu-id="a3b16-145">**En SQL Data Warehouse-databas och behörigheter**.</span><span class="sxs-lookup"><span data-stu-id="a3b16-145">**A SQL Data Warehouse database and permissions**.</span></span> <span data-ttu-id="a3b16-146">Den här självstudiekursen ansluter tooa SQL Data Warehouse-instans och läser in data i den.</span><span class="sxs-lookup"><span data-stu-id="a3b16-146">This tutorial connects tooa SQL Data Warehouse instance and loads data into it.</span></span> <span data-ttu-id="a3b16-147">Du har toohave behörigheter toocreate en tabell och tooload data.</span><span class="sxs-lookup"><span data-stu-id="a3b16-147">You have toohave permissions toocreate a table and tooload data.</span></span>
6. <span data-ttu-id="a3b16-148">**En brandväggsregel**.</span><span class="sxs-lookup"><span data-stu-id="a3b16-148">**A firewall rule**.</span></span> <span data-ttu-id="a3b16-149">Du har toocreate en brandväggsregel på SQL Data Warehouse med hello IP-adressen för den lokala datorn innan du kan ladda upp data toohello SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="a3b16-149">You have toocreate a firewall rule on SQL Data Warehouse with hello IP address of your local computer before you can upload data toohello SQL Data Warehouse.</span></span>

## <a name="step-1-create-a-new-integration-services-project"></a><span data-ttu-id="a3b16-150">Steg 1: Skapa ett nytt Integration Services-projekt</span><span class="sxs-lookup"><span data-stu-id="a3b16-150">Step 1: Create a new Integration Services project</span></span>
1. <span data-ttu-id="a3b16-151">Starta Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a3b16-151">Launch Visual Studio.</span></span>
2. <span data-ttu-id="a3b16-152">På hello **filen** väljer du **ny | Projektet**.</span><span class="sxs-lookup"><span data-stu-id="a3b16-152">On hello **File** menu, select **New | Project**.</span></span>
3. <span data-ttu-id="a3b16-153">Navigera toohello **installerad | Mallar | Business Intelligence | Integration Services** projekttyper.</span><span class="sxs-lookup"><span data-stu-id="a3b16-153">Navigate toohello **Installed | Templates | Business Intelligence | Integration Services** project types.</span></span>
4. <span data-ttu-id="a3b16-154">Välj **Integration Services-projekt**.</span><span class="sxs-lookup"><span data-stu-id="a3b16-154">Select **Integration Services Project**.</span></span> <span data-ttu-id="a3b16-155">Ange värden för **namn** och **plats**, och välj sedan **OK**.</span><span class="sxs-lookup"><span data-stu-id="a3b16-155">Provide values for **Name** and **Location**, and then select **OK**.</span></span>

<span data-ttu-id="a3b16-156">Visual Studio öppnas och skapar ett nytt projekt för Integration Services (SSIS).</span><span class="sxs-lookup"><span data-stu-id="a3b16-156">Visual Studio opens and creates a new Integration Services (SSIS) project.</span></span> <span data-ttu-id="a3b16-157">Visual Studio öppnas hello designer för hello enda nya SSIS-paket (Package.dtsx) i hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="a3b16-157">Then Visual Studio opens hello designer for hello single new SSIS package (Package.dtsx) in hello project.</span></span> <span data-ttu-id="a3b16-158">Du ser hello följande områden:</span><span class="sxs-lookup"><span data-stu-id="a3b16-158">You see hello following screen areas:</span></span>

* <span data-ttu-id="a3b16-159">Hello vänster hello **verktygslådan** SSIS-komponenter.</span><span class="sxs-lookup"><span data-stu-id="a3b16-159">On hello left, hello **Toolbox** of SSIS components.</span></span>
* <span data-ttu-id="a3b16-160">Hello mitten hello designytan med flera flikar.</span><span class="sxs-lookup"><span data-stu-id="a3b16-160">In hello middle, hello design surface, with multiple tabs.</span></span> <span data-ttu-id="a3b16-161">Normalt använder du minst hello **Kontrollflöde** och hello **dataflöde** flikar.</span><span class="sxs-lookup"><span data-stu-id="a3b16-161">You typically use at least hello **Control Flow** and hello **Data Flow** tabs.</span></span>
* <span data-ttu-id="a3b16-162">På rätt hello, hello **Solution Explorer** och hello **egenskaper** fönster.</span><span class="sxs-lookup"><span data-stu-id="a3b16-162">On hello right, hello **Solution Explorer** and hello **Properties** panes.</span></span>
  
    ![][01]

## <a name="step-2-create-hello-basic-data-flow"></a><span data-ttu-id="a3b16-163">Steg 2: Skapa hello grundläggande dataflöde</span><span class="sxs-lookup"><span data-stu-id="a3b16-163">Step 2: Create hello basic data flow</span></span>
1. <span data-ttu-id="a3b16-164">Dra en Data flödar aktivitet från hello verktygslådan toohello mittpunkt hello designytan (på hello **Kontrollflöde** fliken).</span><span class="sxs-lookup"><span data-stu-id="a3b16-164">Drag a Data Flow Task from hello Toolbox toohello center of hello design surface (on hello **Control Flow** tab).</span></span>
   
    ![][02]
2. <span data-ttu-id="a3b16-165">Dubbelklicka på fliken för hello Data flödar aktivitet tooswitch toohello dataflöde.</span><span class="sxs-lookup"><span data-stu-id="a3b16-165">Double-click hello Data Flow Task tooswitch toohello Data Flow tab.</span></span>
3. <span data-ttu-id="a3b16-166">Dra en ADO.NET källa toohello designytan hello andra källor listan i hello verktygslådan.</span><span class="sxs-lookup"><span data-stu-id="a3b16-166">From hello Other Sources list in hello Toolbox, drag an ADO.NET Source toohello design surface.</span></span> <span data-ttu-id="a3b16-167">Med hello källa kort fortfarande markerat, ändra namnet för**SQL Server-datakälla** i hello **egenskaper** fönstret.</span><span class="sxs-lookup"><span data-stu-id="a3b16-167">With hello source adapter still selected, change its name too**SQL Server source** in hello **Properties** pane.</span></span>
4. <span data-ttu-id="a3b16-168">Dra en ADO.NET mål toohello designytan under hello ADO.NET källa hello andra mål listan i hello verktygslådan.</span><span class="sxs-lookup"><span data-stu-id="a3b16-168">From hello Other Destinations list in hello Toolbox, drag an ADO.NET Destination toohello design surface under hello ADO.NET Source.</span></span> <span data-ttu-id="a3b16-169">Hej målnätverkskort fortfarande markerat, ändra dess namn för**SQL DW mål** i hello **egenskaper** fönstret.</span><span class="sxs-lookup"><span data-stu-id="a3b16-169">With hello destination adapter still selected, change its name too**SQL DW destination** in hello **Properties** pane.</span></span>
   
    ![][09]

## <a name="step-3-configure-hello-source-adapter"></a><span data-ttu-id="a3b16-170">Steg 3: Konfigurera hello källa nätverkskort</span><span class="sxs-lookup"><span data-stu-id="a3b16-170">Step 3: Configure hello source adapter</span></span>
1. <span data-ttu-id="a3b16-171">Dubbelklicka på hello källa kortet tooopen hello **ADO.NET källa Editor**.</span><span class="sxs-lookup"><span data-stu-id="a3b16-171">Double-click hello source adapter tooopen hello **ADO.NET Source Editor**.</span></span>
   
    ![][03]
2. <span data-ttu-id="a3b16-172">På hello **Connection Manager** för hello **ADO.NET källa Editor**, klicka på hello **ny** knappen Nästa toohello **ADO.NET Anslutningshanteraren**lista tooopen hello **konfigurera ADO.NET Anslutningshanteraren** dialogrutan och skapa anslutningsinställningar för hello SQL Server-databas som den här kursen läser in data.</span><span class="sxs-lookup"><span data-stu-id="a3b16-172">On hello **Connection Manager** tab of hello **ADO.NET Source Editor**, click hello **New** button next toohello **ADO.NET connection manager** list tooopen hello **Configure ADO.NET Connection Manager** dialog box and create connection settings for hello SQL Server database from which this tutorial loads data.</span></span>
   
    ![][04]
3. <span data-ttu-id="a3b16-173">I hello **konfigurera ADO.NET Anslutningshanteraren** dialogrutan klickar du på hello **ny** knappen tooopen hello **Connection Manager** dialogrutan och skapa en ny anslutning.</span><span class="sxs-lookup"><span data-stu-id="a3b16-173">In hello **Configure ADO.NET Connection Manager** dialog box, click hello **New** button tooopen hello **Connection Manager** dialog box and create a new data connection.</span></span>
   
    ![][05]
4. <span data-ttu-id="a3b16-174">I hello **Connection Manager** dialogrutan rutan, hello följande saker.</span><span class="sxs-lookup"><span data-stu-id="a3b16-174">In hello **Connection Manager** dialog box, do hello following things.</span></span>
   
   1. <span data-ttu-id="a3b16-175">För **Provider**, Välj hello SqlClient-dataprovidern.</span><span class="sxs-lookup"><span data-stu-id="a3b16-175">For **Provider**, select hello SqlClient Data Provider.</span></span>
   2. <span data-ttu-id="a3b16-176">För **servernamn**, ange hello SQL Server-namn.</span><span class="sxs-lookup"><span data-stu-id="a3b16-176">For **Server name**, enter hello SQL Server name.</span></span>
   3. <span data-ttu-id="a3b16-177">I hello **inloggning toohello server** väljer eller ange autentiseringsinformation.</span><span class="sxs-lookup"><span data-stu-id="a3b16-177">In hello **Log on toohello server** section, select or enter authentication information.</span></span>
   4. <span data-ttu-id="a3b16-178">I hello **Anslut tooa databasen** väljer hello exempeldatabasen AdventureWorks.</span><span class="sxs-lookup"><span data-stu-id="a3b16-178">In hello **Connect tooa database** section, select hello AdventureWorks sample database.</span></span>
   5. <span data-ttu-id="a3b16-179">Klicka på **Anslutningstestet**.</span><span class="sxs-lookup"><span data-stu-id="a3b16-179">Click **Test Connection**.</span></span>
      
       ![][06]
   6. <span data-ttu-id="a3b16-180">Klicka på dialogrutan för hello som rapporterar hello resultatet av anslutningstestet hello, **OK** tooreturn toohello **Connection Manager** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a3b16-180">In hello dialog box that reports hello results of hello connection test, click **OK** tooreturn toohello **Connection Manager** dialog box.</span></span>
   7. <span data-ttu-id="a3b16-181">I hello **Connection Manager** dialogrutan klickar du på **OK** tooreturn toohello **konfigurera ADO.NET Anslutningshanteraren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a3b16-181">In hello **Connection Manager** dialog box, click **OK** tooreturn toohello **Configure ADO.NET Connection Manager** dialog box.</span></span>
5. <span data-ttu-id="a3b16-182">I hello **konfigurera ADO.NET Anslutningshanteraren** dialogrutan klickar du på **OK** tooreturn toohello **ADO.NET källa Editor**.</span><span class="sxs-lookup"><span data-stu-id="a3b16-182">In hello **Configure ADO.NET Connection Manager** dialog box, click **OK** tooreturn toohello **ADO.NET Source Editor**.</span></span>
6. <span data-ttu-id="a3b16-183">I hello **ADO.NET källa Editor**, i hello **namnet på hello tabell eller vy hello** listan, Välj hello **Sales.SalesOrderDetail** tabell.</span><span class="sxs-lookup"><span data-stu-id="a3b16-183">In hello **ADO.NET Source Editor**, in hello **Name of hello table or hello view** list, select hello **Sales.SalesOrderDetail** table.</span></span>
   
    ![][07]
7. <span data-ttu-id="a3b16-184">Klicka på **Preview** toosee hello först 200 rader med data i hello källtabellen i hello **Preview frågeresultat** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a3b16-184">Click **Preview** toosee hello first 200 rows of data in hello source table in hello **Preview Query Results** dialog box.</span></span>
   
    ![][08]
8. <span data-ttu-id="a3b16-185">I hello **Preview frågeresultat** dialogrutan klickar du på **Stäng** tooreturn toohello **ADO.NET källa Editor**.</span><span class="sxs-lookup"><span data-stu-id="a3b16-185">In hello **Preview Query Results** dialog box, click **Close** tooreturn toohello **ADO.NET Source Editor**.</span></span>
9. <span data-ttu-id="a3b16-186">I hello **ADO.NET källa Editor**, klickar du på **OK** toofinish konfigurera hello-datakällan.</span><span class="sxs-lookup"><span data-stu-id="a3b16-186">In hello **ADO.NET Source Editor**, click **OK** toofinish configuring hello data source.</span></span>

## <a name="step-4-connect-hello-source-adapter-toohello-destination-adapter"></a><span data-ttu-id="a3b16-187">Steg 4: Anslut hello källa kortet toohello målnätverkskort</span><span class="sxs-lookup"><span data-stu-id="a3b16-187">Step 4: Connect hello source adapter toohello destination adapter</span></span>
1. <span data-ttu-id="a3b16-188">Välj hello käll-kortet på hello designytan.</span><span class="sxs-lookup"><span data-stu-id="a3b16-188">Select hello source adapter on hello design surface.</span></span>
2. <span data-ttu-id="a3b16-189">Välj hello blå pil som sträcker sig från hello källa nätverkskort och dra toohello mål editor tills den har fästs på plats.</span><span class="sxs-lookup"><span data-stu-id="a3b16-189">Select hello blue arrow that extends from hello source adapter and drag it toohello destination editor until it snaps into place.</span></span>
   
    ![][10]
   
    <span data-ttu-id="a3b16-190">I en typisk SSIS-paket, använda ett antal andra komponenter från hello SSIS verktygslådan between hello källa och hello mål toorestructure, transformering och rensa data som överförs via hello SSIS-dataflöde.</span><span class="sxs-lookup"><span data-stu-id="a3b16-190">In a typical SSIS package, you use a number of other components from hello SSIS Toolbox in between hello source and hello destination toorestructure, transform, and cleanse your data as it passes through hello SSIS data flow.</span></span> <span data-ttu-id="a3b16-191">tookeep det här exemplet är så enkel som möjligt, vi ansluter hello datakälla direkt toohello mål.</span><span class="sxs-lookup"><span data-stu-id="a3b16-191">tookeep this example as simple as possible, we’re connecting hello source directly toohello destination.</span></span>

## <a name="step-5-configure-hello-destination-adapter"></a><span data-ttu-id="a3b16-192">Steg 5: Konfigurera hello-målnätverkskort</span><span class="sxs-lookup"><span data-stu-id="a3b16-192">Step 5: Configure hello destination adapter</span></span>
1. <span data-ttu-id="a3b16-193">Dubbelklicka på hello målet nätverkskort tooopen hello **ADO.NET mål Editor**.</span><span class="sxs-lookup"><span data-stu-id="a3b16-193">Double-click hello destination adapter tooopen hello **ADO.NET Destination Editor**.</span></span>
   
    ![][11]
2. <span data-ttu-id="a3b16-194">På hello **Connection Manager** för hello **ADO.NET mål Editor**, klicka på hello **ny** knappen Nästa toohello **Anslutningshanteraren**lista tooopen hello **konfigurera ADO.NET Anslutningshanteraren** dialogrutan och skapa anslutningsinställningar för hello Azure SQL Data Warehouse-databas som den här kursen läser in data.</span><span class="sxs-lookup"><span data-stu-id="a3b16-194">On hello **Connection Manager** tab of hello **ADO.NET Destination Editor**, click hello **New** button next toohello **Connection manager** list tooopen hello **Configure ADO.NET Connection Manager** dialog box and create connection settings for hello Azure SQL Data Warehouse database into which this tutorial loads data.</span></span>
3. <span data-ttu-id="a3b16-195">I hello **konfigurera ADO.NET Anslutningshanteraren** dialogrutan klickar du på hello **ny** knappen tooopen hello **Connection Manager** dialogrutan och skapa en ny anslutning.</span><span class="sxs-lookup"><span data-stu-id="a3b16-195">In hello **Configure ADO.NET Connection Manager** dialog box, click hello **New** button tooopen hello **Connection Manager** dialog box and create a new data connection.</span></span>
4. <span data-ttu-id="a3b16-196">I hello **Connection Manager** dialogrutan rutan, hello följande saker.</span><span class="sxs-lookup"><span data-stu-id="a3b16-196">In hello **Connection Manager** dialog box, do hello following things.</span></span>
   1. <span data-ttu-id="a3b16-197">För **Provider**, Välj hello SqlClient-dataprovidern.</span><span class="sxs-lookup"><span data-stu-id="a3b16-197">For **Provider**, select hello SqlClient Data Provider.</span></span>
   2. <span data-ttu-id="a3b16-198">För **servernamn**, ange hello SQL Data Warehouse namn.</span><span class="sxs-lookup"><span data-stu-id="a3b16-198">For **Server name**, enter hello SQL Data Warehouse name.</span></span>
   3. <span data-ttu-id="a3b16-199">I hello **inloggning toohello server** väljer **används SQL Server-autentisering** och ange autentiseringsinformation.</span><span class="sxs-lookup"><span data-stu-id="a3b16-199">In hello **Log on toohello server** section, select **Use SQL Server authentication** and enter authentication information.</span></span>
   4. <span data-ttu-id="a3b16-200">I hello **Anslut tooa databasen** väljer du en befintlig SQL Data Warehouse-databas.</span><span class="sxs-lookup"><span data-stu-id="a3b16-200">In hello **Connect tooa database** section, select an existing SQL Data Warehouse database.</span></span>
   5. <span data-ttu-id="a3b16-201">Klicka på **Anslutningstestet**.</span><span class="sxs-lookup"><span data-stu-id="a3b16-201">Click **Test Connection**.</span></span>
   6. <span data-ttu-id="a3b16-202">Klicka på dialogrutan för hello som rapporterar hello resultatet av anslutningstestet hello, **OK** tooreturn toohello **Connection Manager** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a3b16-202">In hello dialog box that reports hello results of hello connection test, click **OK** tooreturn toohello **Connection Manager** dialog box.</span></span>
   7. <span data-ttu-id="a3b16-203">I hello **Connection Manager** dialogrutan klickar du på **OK** tooreturn toohello **konfigurera ADO.NET Anslutningshanteraren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a3b16-203">In hello **Connection Manager** dialog box, click **OK** tooreturn toohello **Configure ADO.NET Connection Manager** dialog box.</span></span>
5. <span data-ttu-id="a3b16-204">I hello **konfigurera ADO.NET Anslutningshanteraren** dialogrutan klickar du på **OK** tooreturn toohello **ADO.NET mål Editor**.</span><span class="sxs-lookup"><span data-stu-id="a3b16-204">In hello **Configure ADO.NET Connection Manager** dialog box, click **OK** tooreturn toohello **ADO.NET Destination Editor**.</span></span>
6. <span data-ttu-id="a3b16-205">I hello **ADO.NET mål Editor**, klickar du på **ny** nästa toohello **använder en tabell eller vy** lista tooopen hello **Create Table** dialogrutan toocreate en ny måltabell med en kolumnlista som matchar hello källtabellen.</span><span class="sxs-lookup"><span data-stu-id="a3b16-205">In hello **ADO.NET Destination Editor**, click **New** next toohello **Use a table or view** list tooopen hello **Create Table** dialog box toocreate a new destination table with a column list that matches hello source table.</span></span>
   
    ![][12a]
7. <span data-ttu-id="a3b16-206">I hello **Create Table** dialogrutan rutan, hello följande saker.</span><span class="sxs-lookup"><span data-stu-id="a3b16-206">In hello **Create Table** dialog box, do hello following things.</span></span>
   
   1. <span data-ttu-id="a3b16-207">Ändra hello hello måltabellen för**SalesOrderDetail**.</span><span class="sxs-lookup"><span data-stu-id="a3b16-207">Change hello name of hello destination table too**SalesOrderDetail**.</span></span>
   2. <span data-ttu-id="a3b16-208">Ta bort hello **rowguid** kolumn.</span><span class="sxs-lookup"><span data-stu-id="a3b16-208">Remove hello **rowguid** column.</span></span> <span data-ttu-id="a3b16-209">Hej **uniqueidentifier** datatyp stöds inte i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="a3b16-209">hello **uniqueidentifier** data type is not supported in SQL Data Warehouse.</span></span>
   3. <span data-ttu-id="a3b16-210">Ändra hello datatyp hello **LineTotal** kolumn för**pengar**.</span><span class="sxs-lookup"><span data-stu-id="a3b16-210">Change hello data type of hello **LineTotal** column too**money**.</span></span> <span data-ttu-id="a3b16-211">Hej **decimal** datatyp stöds inte i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="a3b16-211">hello **decimal** data type is not supported in SQL Data Warehouse.</span></span> <span data-ttu-id="a3b16-212">Information om vilka datatyper finns [CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)][CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)].</span><span class="sxs-lookup"><span data-stu-id="a3b16-212">For info about supported data types, see [CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)][CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)].</span></span>
      
       ![][12b]
   4. <span data-ttu-id="a3b16-213">Klicka på **OK** toocreate hello tabellen och returnerar toohello **ADO.NET mål Editor**.</span><span class="sxs-lookup"><span data-stu-id="a3b16-213">Click **OK** toocreate hello table and return toohello **ADO.NET Destination Editor**.</span></span>
8. <span data-ttu-id="a3b16-214">I hello **ADO.NET mål Editor**väljer hello **mappningar** fliken toosee hur kolumner i hello källan mappas toocolumns i hello målet.</span><span class="sxs-lookup"><span data-stu-id="a3b16-214">In hello **ADO.NET Destination Editor**, select hello **Mappings** tab toosee how columns in hello source are mapped toocolumns in hello destination.</span></span>
   
    ![][13]
9. <span data-ttu-id="a3b16-215">Klicka på **OK** toofinish konfigurera hello-datakällan.</span><span class="sxs-lookup"><span data-stu-id="a3b16-215">Click **OK** toofinish configuring hello data source.</span></span>

## <a name="step-6-run-hello-package-tooload-hello-data"></a><span data-ttu-id="a3b16-216">Steg 6: Kör hello paketdata tooload hello</span><span class="sxs-lookup"><span data-stu-id="a3b16-216">Step 6: Run hello package tooload hello data</span></span>
<span data-ttu-id="a3b16-217">Kör hello paketet genom att klicka på hello **starta** knappen hello verktygsfältet eller välja ett hello **kör** alternativ på hello **felsöka** menyn.</span><span class="sxs-lookup"><span data-stu-id="a3b16-217">Run hello package by clicking hello **Start** button on hello toolbar or by selecting one of hello **Run** options on hello **Debug** menu.</span></span>

<span data-ttu-id="a3b16-218">Hello paketet börjar toorun, se gul snurrande hjul tooindicate som hello antalet bearbetade hittills rader.</span><span class="sxs-lookup"><span data-stu-id="a3b16-218">As hello package begins toorun, you see yellow spinning wheels tooindicate activity as well as hello number of rows processed so far.</span></span>

![][14]

<span data-ttu-id="a3b16-219">När hello paketet har körts du finns grön bockmarkering tooindicate lyckade samt hello Totalt antal rader med data som har lästs in från hello källa toohello mål.</span><span class="sxs-lookup"><span data-stu-id="a3b16-219">When hello package has finished running, you see green check marks tooindicate success as well as hello total number of rows of data loaded from hello source toohello destination.</span></span>

![][15]

<span data-ttu-id="a3b16-220">Grattis!</span><span class="sxs-lookup"><span data-stu-id="a3b16-220">Congratulations!</span></span> <span data-ttu-id="a3b16-221">Du har använt SQL Server Integration Services tooload data till Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="a3b16-221">You’ve successfully used SQL Server Integration Services tooload data into Azure SQL Data Warehouse.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a3b16-222">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a3b16-222">Next steps</span></span>
* <span data-ttu-id="a3b16-223">Läs mer om hello SSIS-dataflöde.</span><span class="sxs-lookup"><span data-stu-id="a3b16-223">Learn more about hello SSIS data flow.</span></span> <span data-ttu-id="a3b16-224">Börja här: [dataflöde][Data Flow].</span><span class="sxs-lookup"><span data-stu-id="a3b16-224">Start here: [Data Flow][Data Flow].</span></span>
* <span data-ttu-id="a3b16-225">Lär dig hur toodebug och felsöka dina paket direkt i hello design-miljön.</span><span class="sxs-lookup"><span data-stu-id="a3b16-225">Learn how toodebug and troubleshoot your packages right in hello design environment.</span></span> <span data-ttu-id="a3b16-226">Börja här: [felsökningsverktyg för paketet utveckling][Troubleshooting Tools for Package Development].</span><span class="sxs-lookup"><span data-stu-id="a3b16-226">Start here: [Troubleshooting Tools for Package Development][Troubleshooting Tools for Package Development].</span></span>
* <span data-ttu-id="a3b16-227">Lär dig hur toodeploy din SSIS projekt och paketerar tooIntegration Services-Server eller tooanother lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="a3b16-227">Learn how toodeploy your SSIS projects and packages tooIntegration Services Server or tooanother storage location.</span></span> <span data-ttu-id="a3b16-228">Börja här: [projekt för distribution av och paket][Deployment of Projects and Packages].</span><span class="sxs-lookup"><span data-stu-id="a3b16-228">Start here: [Deployment of Projects and Packages][Deployment of Projects and Packages].</span></span>

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
