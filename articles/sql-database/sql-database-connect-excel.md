---
title: aaaConnect Excel tooSQL databasen | Microsoft Docs
description: "Lär dig hur tooconnect Microsoft Excel tooAzure SQL-databas i hello molnet. Importera data till Excel för rapportering och dataundersökning."
services: sql-database
keywords: ansluta excel toosql, importera data tooexcel
documentationcenter: 
author: joseidz
manager: jhubbard
editor: 
ms.assetid: 906924bc-2707-48d3-bac6-397976a0409d
ms.service: sql-database
ms.custom: develop apps
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/10/2017
ms.author: jhubbard
ms.openlocfilehash: 0048849432023145bd1009d45b6d9b64a9c7ac3c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-excel-tooan-azure-sql-database-and-create-a-report"></a><span data-ttu-id="69aa8-105">Ansluta Excel tooan Azure SQL-databas och skapa en rapport</span><span class="sxs-lookup"><span data-stu-id="69aa8-105">Connect Excel tooan Azure SQL database and create a report</span></span>

<span data-ttu-id="69aa8-106">Ansluta Excel tooa SQL-databas i molnet hello och importera data och skapa tabeller och diagram baserat på värdena i hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="69aa8-106">Connect Excel tooa SQL database in hello cloud and import data and create tables and charts based on values in hello database.</span></span> <span data-ttu-id="69aa8-107">I den här kursen ska du ställa in hello anslutningen mellan Excel och en databastabell, spara hello-filen som lagrar data och hello anslutningsinformationen för Excel och skapa ett pivotdiagram från hello databasvärden.</span><span class="sxs-lookup"><span data-stu-id="69aa8-107">In this tutorial you will set up hello connection between Excel and a database table, save hello file that stores data and hello connection information for Excel, and then create a pivot chart from hello database values.</span></span>

<span data-ttu-id="69aa8-108">Du behöver en SQL-databas i Azure innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="69aa8-108">You'll need a SQL database in Azure before you get started.</span></span> <span data-ttu-id="69aa8-109">Om du inte har någon [skapa din första SQL-databas](sql-database-get-started-portal.md) tooget en databas med exempeldata på några minuter.</span><span class="sxs-lookup"><span data-stu-id="69aa8-109">If you don't have one, see [Create your first SQL database](sql-database-get-started-portal.md) tooget a database with sample data up and running in a few minutes.</span></span> <span data-ttu-id="69aa8-110">I den här artikeln, ska du importera exempeldata till Excel från artikeln, men du kan följa liknande steg med dina egna data.</span><span class="sxs-lookup"><span data-stu-id="69aa8-110">In this article, you'll import sample data into Excel from that article, but you can follow similar steps with your own data.</span></span>

<span data-ttu-id="69aa8-111">Du kommer också behöva en kopia av Excel.</span><span class="sxs-lookup"><span data-stu-id="69aa8-111">You'll also need a copy of Excel.</span></span> <span data-ttu-id="69aa8-112">Den här artikeln använder [Microsoft Excel 2016](https://products.office.com/).</span><span class="sxs-lookup"><span data-stu-id="69aa8-112">This article uses [Microsoft Excel 2016](https://products.office.com/).</span></span>

## <a name="connect-excel-tooa-sql-database-and-create-an-odc-file"></a><span data-ttu-id="69aa8-113">Anslut Excel tooa SQL-databas och skapa en odc-fil</span><span class="sxs-lookup"><span data-stu-id="69aa8-113">Connect Excel tooa SQL database and create an odc file</span></span>
1. <span data-ttu-id="69aa8-114">tooconnect Excel tooSQL databas, öppnar du Excel och sedan skapa en ny arbetsbok eller öppna en befintlig Excel-arbetsbok.</span><span class="sxs-lookup"><span data-stu-id="69aa8-114">tooconnect Excel tooSQL database, open Excel and then create a new workbook or open an existing Excel workbook.</span></span>
2. <span data-ttu-id="69aa8-115">I hello menyraden överst hello på hello-sidan klickar du på **Data**, klickar du på **från andra källor**, och klicka sedan på **från SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="69aa8-115">In hello menu bar at hello top of hello page click **Data**, click **From Other Sources**, and then click **From SQL Server**.</span></span>
   
   ![Välj datakälla: Anslut Excel tooSQL-databas.](./media/sql-database-connect-excel/excel_data_source.png)
   
   <span data-ttu-id="69aa8-117">Hej Dataanslutningsguiden öppnas.</span><span class="sxs-lookup"><span data-stu-id="69aa8-117">hello Data Connection Wizard opens.</span></span>
3. <span data-ttu-id="69aa8-118">I hello **ansluta tooDatabase Server** dialogrutan typen hello SQL Database **servernamn** du vill att tooconnect tooin hello formuläret <*servername* > **. database.windows.net**.</span><span class="sxs-lookup"><span data-stu-id="69aa8-118">In hello **Connect tooDatabase Server** dialog box, type hello SQL Database **Server name** you want tooconnect tooin hello form <*servername*>**.database.windows.net**.</span></span> <span data-ttu-id="69aa8-119">Till exempel **adworkserver.database.windows.net**.</span><span class="sxs-lookup"><span data-stu-id="69aa8-119">For example, **adworkserver.database.windows.net**.</span></span>
4. <span data-ttu-id="69aa8-120">Under **inloggningsuppgifter**, klickar du på **Använd hello följande användarnamn och lösenord**, typen hello **användarnamn** och **lösenord** du ställt in för Hej SQL Database-server när du skapade den och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="69aa8-120">Under **Log on credentials**, click **Use hello following User Name and Password**, type hello **User Name** and **Password** you set up for hello SQL Database server when you created it, and then click **Next**.</span></span>
   
   ![Skriv hello server servernamn och inloggningsuppgifter](./media/sql-database-connect-excel/connect-to-server.png)
   
   > [!TIP]
   > <span data-ttu-id="69aa8-122">Beroende på din nätverksmiljö, kanske du inte kan tooconnect eller hello anslutning kan gå förlorade om hello SQL Database-server som inte tillåter trafik från klientens IP-adress.</span><span class="sxs-lookup"><span data-stu-id="69aa8-122">Depending on your network environment, you may not be able tooconnect or you may lose hello connection if hello SQL Database server doesn't allow traffic from your client IP address.</span></span> <span data-ttu-id="69aa8-123">Gå toohello [Azure-portalen](https://portal.azure.com/), klicka på SQL-servrar, servern, klicka på brandvägg under inställningar och Lägg till klientens IP-adress.</span><span class="sxs-lookup"><span data-stu-id="69aa8-123">Go toohello [Azure portal](https://portal.azure.com/), click SQL servers, click your server, click firewall under settings and add your client IP address.</span></span> <span data-ttu-id="69aa8-124">Se [hur tooconfigure brandväggsinställningar](sql-database-configure-firewall-settings.md) mer information.</span><span class="sxs-lookup"><span data-stu-id="69aa8-124">See [How tooconfigure firewall settings](sql-database-configure-firewall-settings.md) for details.</span></span>
   > 
   > 
5. <span data-ttu-id="69aa8-125">I hello **Välj databas och tabell** dialogrutan, Välj hello databas du vill toowork med hello listan och klicka på hello tabeller eller vyer som du vill toowork med (vi valde **vGetAllCategories**), och sedan Klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="69aa8-125">In hello **Select Database and Table** dialog, select hello database you want toowork with from hello list, and then click hello tables or views you want toowork with (we chose **vGetAllCategories**), and then click **Next**.</span></span>
   
    ![Välj en databas och en tabell.](./media/sql-database-connect-excel/select-database-and-table.png)
   
    <span data-ttu-id="69aa8-127">Hej **Spara dataanslutningsfil och slutför** öppnas, där du kan ange information om hello filen Office-databasanslutningar (*.odc) som används av Excel.</span><span class="sxs-lookup"><span data-stu-id="69aa8-127">hello **Save Data Connection File and Finish** dialog box opens, where you provide information about hello Office database connection (*.odc) file that Excel uses.</span></span> <span data-ttu-id="69aa8-128">Du kan lämna hello standardvärden eller anpassa dina val.</span><span class="sxs-lookup"><span data-stu-id="69aa8-128">You can leave hello defaults or customize your selections.</span></span>
6. <span data-ttu-id="69aa8-129">Du kan lämna hello standard, men Observera hello **filnamn** särskilt.</span><span class="sxs-lookup"><span data-stu-id="69aa8-129">You can leave hello defaults, but note hello **File Name** in particular.</span></span> <span data-ttu-id="69aa8-130">En **beskrivning**, **eget namn**, och **sökord** hjälper dig och andra användare att komma ihåg vad du ansluter tooand hitta hello-anslutning.</span><span class="sxs-lookup"><span data-stu-id="69aa8-130">A **Description**, a **Friendly Name**, and **Search Keywords** help you and other users remember what you're connecting tooand find hello connection.</span></span> <span data-ttu-id="69aa8-131">Klicka på **alltid försöker toouse informationen filen toorefresh** om du vill att anslutningsinformation lagras i hello odc-filen så att den kan uppdatera när du ansluter tooit och klicka sedan på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="69aa8-131">Click **Always attempt toouse this file toorefresh data** if you want connection information stored in hello odc file so it can update when you connect tooit, and then click **Finish**.</span></span>
   
    ![Spara en odc-fil](./media/sql-database-connect-excel/save-odc-file.png)
   
    <span data-ttu-id="69aa8-133">Hej **dataimport** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="69aa8-133">hello **Import data** dialog box appears.</span></span>

## <a name="import-hello-data-into-excel-and-create-a-pivot-chart"></a><span data-ttu-id="69aa8-134">Importera hello data till Excel och skapa ett pivotdiagram</span><span class="sxs-lookup"><span data-stu-id="69aa8-134">Import hello data into Excel and create a pivot chart</span></span>
<span data-ttu-id="69aa8-135">Nu när du har skapat hello anslutning och skapade hello med data och anslutningsinformation, läser du tooimport hello data.</span><span class="sxs-lookup"><span data-stu-id="69aa8-135">Now that you've established hello connection and created hello file with data and connection information, you're reading tooimport hello data.</span></span>

1. <span data-ttu-id="69aa8-136">I hello **importera Data** dialogrutan hello-alternativ som du vill använda för att presentera dina data i hello kalkylblad och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="69aa8-136">In hello **Import Data** dialog, click hello option you want for presenting your data in hello worksheet and then click **OK**.</span></span> <span data-ttu-id="69aa8-137">Vi valde **PivotChart**.</span><span class="sxs-lookup"><span data-stu-id="69aa8-137">We chose **PivotChart**.</span></span> <span data-ttu-id="69aa8-138">Du kan också välja toocreate en **nytt kalkylblad** eller för**lägga till den här data tooa datamodellen**.</span><span class="sxs-lookup"><span data-stu-id="69aa8-138">You can also choose toocreate a **New worksheet** or too**Add this data tooa Data Model**.</span></span> <span data-ttu-id="69aa8-139">Mer information om datamodeller finns i [Skapa en datamodell i Excel](https://support.office.com/article/Create-a-Data-Model-in-Excel-87E7A54C-87DC-488E-9410-5C75DBCB0F7B).</span><span class="sxs-lookup"><span data-stu-id="69aa8-139">For more information on Data Models, see [Create a data model in Excel](https://support.office.com/article/Create-a-Data-Model-in-Excel-87E7A54C-87DC-488E-9410-5C75DBCB0F7B).</span></span> <span data-ttu-id="69aa8-140">Klicka på **egenskaper** tooexplore information om hello odc-filen som du skapade i hello föregående steg och toochoose alternativ för datauppdatering hello.</span><span class="sxs-lookup"><span data-stu-id="69aa8-140">Click **Properties** tooexplore information about hello odc file you created in hello previous step and toochoose options for refreshing hello data.</span></span>
   
    ![Välja hello dataformat i Excel](./media/sql-database-connect-excel/import-data.png)
   
    <span data-ttu-id="69aa8-142">hello kalkylbladet har nu en tom pivottabell och diagram.</span><span class="sxs-lookup"><span data-stu-id="69aa8-142">hello worksheet now has an empty pivot table and chart.</span></span>
2. <span data-ttu-id="69aa8-143">Under **PivotTable-Fields**, Välj alla hello kryssrutorna för hello fält som du vill tooview.</span><span class="sxs-lookup"><span data-stu-id="69aa8-143">Under **PivotTable Fields**, select all hello check-boxes for hello fields you want tooview.</span></span>
   
    ![Konfigurera databasrapporten.](./media/sql-database-connect-excel/power-pivot-results.png)

> [!TIP]
> <span data-ttu-id="69aa8-145">Om du vill tooconnect andra Excel-arbetsböcker och kalkylblad toohello databasen, klickar du på **Data**, klickar du på **anslutningar**, klickar du på **Lägg till**, Välj hello-anslutning som du skapade hello listan och klicka sedan på **öppna**.</span><span class="sxs-lookup"><span data-stu-id="69aa8-145">If you want tooconnect other Excel workbooks and worksheets toohello database, click **Data**, click **Connections**, click **Add**, choose hello connection you created from hello list, and then click **Open**.</span></span>
> <span data-ttu-id="69aa8-146">![Öppna en anslutning från en annan arbetsbok](./media/sql-database-connect-excel/open-from-another-workbook.png)</span><span class="sxs-lookup"><span data-stu-id="69aa8-146">![Open a connection from another workbook](./media/sql-database-connect-excel/open-from-another-workbook.png)</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="69aa8-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="69aa8-147">Next steps</span></span>
* <span data-ttu-id="69aa8-148">Lär dig hur för[ansluta tooSQL databasen med SQL Server Management Studio](sql-database-connect-query-ssms.md) för avancerade frågor och analys.</span><span class="sxs-lookup"><span data-stu-id="69aa8-148">Learn how too[Connect tooSQL Database with SQL Server Management Studio](sql-database-connect-query-ssms.md) for advanced querying and analysis.</span></span>
* <span data-ttu-id="69aa8-149">Lär dig mer om hello fördelarna med [elastiska pooler](sql-database-elastic-pool.md).</span><span class="sxs-lookup"><span data-stu-id="69aa8-149">Learn about hello benefits of [elastic pools](sql-database-elastic-pool.md).</span></span>
* <span data-ttu-id="69aa8-150">Lär dig hur för[skapa ett webbprogram som ansluter tooSQL databasen på hello backend-](../app-service-web/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="69aa8-150">Learn how too[create a web application that connects tooSQL Database on hello back-end](../app-service-web/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md).</span></span>

