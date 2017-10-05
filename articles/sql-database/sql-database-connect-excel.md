---
title: Anslut Excel till SQL Database | Microsoft Docs
description: "Lär dig hur man ansluter Microsoft Excel till en Azure SQL-databas i molnet. Importera data till Excel för rapportering och dataundersökning."
services: sql-database
keywords: ansluta excel till sql, importera data till excel
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
ms.openlocfilehash: 97344d7c0be38b3092a3224074d486b5bb984176
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="connect-excel-to-an-azure-sql-database-and-create-a-report"></a><span data-ttu-id="daaa0-105">Anslut Excel till en Azure SQL database och skapa en rapport</span><span class="sxs-lookup"><span data-stu-id="daaa0-105">Connect Excel to an Azure SQL database and create a report</span></span>

<span data-ttu-id="daaa0-106">Anslut Excel till en SQL-databas i molnet och importera data och skapa tabeller och diagram baserat på värdena i databasen.</span><span class="sxs-lookup"><span data-stu-id="daaa0-106">Connect Excel to a SQL database in the cloud and import data and create tables and charts based on values in the database.</span></span> <span data-ttu-id="daaa0-107">I de här självstudierna kommer du att ställa in anslutningen mellan Excel och en databastabell, spara filen som lagrar data och anslutningsinformationen för Excel och sedan skapa ett pivotdiagram från databasvärdena.</span><span class="sxs-lookup"><span data-stu-id="daaa0-107">In this tutorial you will set up the connection between Excel and a database table, save the file that stores data and the connection information for Excel, and then create a pivot chart from the database values.</span></span>

<span data-ttu-id="daaa0-108">Du behöver en SQL-databas i Azure innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="daaa0-108">You'll need a SQL database in Azure before you get started.</span></span> <span data-ttu-id="daaa0-109">Om du inte har någon, kan du se [Skapa din första SQL-databas](sql-database-get-started-portal.md) för att starta en databas med exempeldata på några minuter.</span><span class="sxs-lookup"><span data-stu-id="daaa0-109">If you don't have one, see [Create your first SQL database](sql-database-get-started-portal.md) to get a database with sample data up and running in a few minutes.</span></span> <span data-ttu-id="daaa0-110">I den här artikeln, ska du importera exempeldata till Excel från artikeln, men du kan följa liknande steg med dina egna data.</span><span class="sxs-lookup"><span data-stu-id="daaa0-110">In this article, you'll import sample data into Excel from that article, but you can follow similar steps with your own data.</span></span>

<span data-ttu-id="daaa0-111">Du kommer också behöva en kopia av Excel.</span><span class="sxs-lookup"><span data-stu-id="daaa0-111">You'll also need a copy of Excel.</span></span> <span data-ttu-id="daaa0-112">Den här artikeln använder [Microsoft Excel 2016](https://products.office.com/).</span><span class="sxs-lookup"><span data-stu-id="daaa0-112">This article uses [Microsoft Excel 2016](https://products.office.com/).</span></span>

## <a name="connect-excel-to-a-sql-database-and-create-an-odc-file"></a><span data-ttu-id="daaa0-113">Anslut Excel till en SQL-databas och skapa en odc-fil</span><span class="sxs-lookup"><span data-stu-id="daaa0-113">Connect Excel to a SQL database and create an odc file</span></span>
1. <span data-ttu-id="daaa0-114">För att ansluta Excel till SQL-databasen, öppnar du Excel och skapar en ny arbetsbok, eller öppnar en befintlig Excel-arbetsbok.</span><span class="sxs-lookup"><span data-stu-id="daaa0-114">To connect Excel to SQL database, open Excel and then create a new workbook or open an existing Excel workbook.</span></span>
2. <span data-ttu-id="daaa0-115">I menyraden överst på sidan, klickar du på **Data**, klickar på **Från andra källor** och klickar sedan på **Från SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="daaa0-115">In the menu bar at the top of the page click **Data**, click **From Other Sources**, and then click **From SQL Server**.</span></span>
   
   ![Välj datakälla: Anslut Excel till SQL-databas.](./media/sql-database-connect-excel/excel_data_source.png)
   
   <span data-ttu-id="daaa0-117">Dataanslutningsguiden öppnas.</span><span class="sxs-lookup"><span data-stu-id="daaa0-117">The Data Connection Wizard opens.</span></span>
3. <span data-ttu-id="daaa0-118">I dialogrutan **Anslut till databasserver**, skriver du **Servernamnet** för den SQL Database du vill ansluta till i formatet <*servernamn*>**. database.windows.net**.</span><span class="sxs-lookup"><span data-stu-id="daaa0-118">In the **Connect to Database Server** dialog box, type the SQL Database **Server name** you want to connect to in the form <*servername*>**.database.windows.net**.</span></span> <span data-ttu-id="daaa0-119">Till exempel **adworkserver.database.windows.net**.</span><span class="sxs-lookup"><span data-stu-id="daaa0-119">For example, **adworkserver.database.windows.net**.</span></span>
4. <span data-ttu-id="daaa0-120">Under **Inloggningsuppgifter**, klickar du på **Använd följande användarnamn och lösenord** och fyller i det **Användarnamn** och **Lösenord** du ställde in för SQL Database-servern när du skapade den och klickar sedan på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="daaa0-120">Under **Log on credentials**, click **Use the following User Name and Password**, type the **User Name** and **Password** you set up for the SQL Database server when you created it, and then click **Next**.</span></span>
   
   ![Ange servernamn och inloggningsuppgifter](./media/sql-database-connect-excel/connect-to-server.png)
   
   > [!TIP]
   > <span data-ttu-id="daaa0-122">Beroende på din nätverksmiljö, är det möjligt att du inte kan ansluta, eller så kan du tappa anslutningen om SQL Database-servern inte tillåter trafik från din klient-IP-adress.</span><span class="sxs-lookup"><span data-stu-id="daaa0-122">Depending on your network environment, you may not be able to connect or you may lose the connection if the SQL Database server doesn't allow traffic from your client IP address.</span></span> <span data-ttu-id="daaa0-123">Gå till [Azure-portalen](https://portal.azure.com/), klicka på SQL-servrar, klicka på din server, klicka på brandvägg under inställningar och lägg till din klient-IP-adress.</span><span class="sxs-lookup"><span data-stu-id="daaa0-123">Go to the [Azure portal](https://portal.azure.com/), click SQL servers, click your server, click firewall under settings and add your client IP address.</span></span> <span data-ttu-id="daaa0-124">Se [Så här konfigurerar du brandväggsinställningar](sql-database-configure-firewall-settings.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="daaa0-124">See [How to configure firewall settings](sql-database-configure-firewall-settings.md) for details.</span></span>
   > 
   > 
5. <span data-ttu-id="daaa0-125">I dialogen **Välj databas och tabell** väljer du den databas du vill arbeta med från listan och klickar sedan på de tabeller eller vyer som du vill arbeta med (vi valde **vGetAllCategories**) och klickar sedan på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="daaa0-125">In the **Select Database and Table** dialog, select the database you want to work with from the list, and then click the tables or views you want to work with (we chose **vGetAllCategories**), and then click **Next**.</span></span>
   
    ![Välj en databas och en tabell.](./media/sql-database-connect-excel/select-database-and-table.png)
   
    <span data-ttu-id="daaa0-127">Dialogrutan **Spara dataanslutningsfil och slutför** öppnas. Där anger du information om Office-databasanslutningen (*.odc)-filen som Excel använder sig av.</span><span class="sxs-lookup"><span data-stu-id="daaa0-127">The **Save Data Connection File and Finish** dialog box opens, where you provide information about the Office database connection (*.odc) file that Excel uses.</span></span> <span data-ttu-id="daaa0-128">Du kan lämna standardvärdena eller anpassa dina val.</span><span class="sxs-lookup"><span data-stu-id="daaa0-128">You can leave the defaults or customize your selections.</span></span>
6. <span data-ttu-id="daaa0-129">Du kan lämna standardvärdena, men lägg speciellt märke till **Filnamn**.</span><span class="sxs-lookup"><span data-stu-id="daaa0-129">You can leave the defaults, but note the **File Name** in particular.</span></span> <span data-ttu-id="daaa0-130">En **Beskrivning**, ett **Eget namn**, och **Sökord** hjälper dig och andra användare att komma ihåg vad du ansluter till och hitta anslutningen.</span><span class="sxs-lookup"><span data-stu-id="daaa0-130">A **Description**, a **Friendly Name**, and **Search Keywords** help you and other users remember what you're connecting to and find the connection.</span></span> <span data-ttu-id="daaa0-131">Klicka på **Försök alltid använda den här filen för datauppdateringar** om du vill att anslutningsinformation lagras i filen så att den kan uppdatera när du ansluter till den och klicka sedan på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="daaa0-131">Click **Always attempt to use this file to refresh data** if you want connection information stored in the odc file so it can update when you connect to it, and then click **Finish**.</span></span>
   
    ![Spara en odc-fil](./media/sql-database-connect-excel/save-odc-file.png)
   
    <span data-ttu-id="daaa0-133">Dialogrutan **Importera data** visas.</span><span class="sxs-lookup"><span data-stu-id="daaa0-133">The **Import data** dialog box appears.</span></span>

## <a name="import-the-data-into-excel-and-create-a-pivot-chart"></a><span data-ttu-id="daaa0-134">Importera data till Excel och skapa ett pivotdiagram</span><span class="sxs-lookup"><span data-stu-id="daaa0-134">Import the data into Excel and create a pivot chart</span></span>
<span data-ttu-id="daaa0-135">Nu när du har etablerat anslutningen och skapat filen med data och anslutningsinformation, är du redo att börja importera data.</span><span class="sxs-lookup"><span data-stu-id="daaa0-135">Now that you've established the connection and created the file with data and connection information, you're reading to import the data.</span></span>

1. <span data-ttu-id="daaa0-136">I dialogen **Importera data**, klickar du på alternativet som du vill ha för att presentera dina data i kalkylbladet och klickar sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="daaa0-136">In the **Import Data** dialog, click the option you want for presenting your data in the worksheet and then click **OK**.</span></span> <span data-ttu-id="daaa0-137">Vi valde **PivotChart**.</span><span class="sxs-lookup"><span data-stu-id="daaa0-137">We chose **PivotChart**.</span></span> <span data-ttu-id="daaa0-138">Du kan också välja att skapa ett **Nytt kalkylblad** eller **Lägg till den här datan i en Datamodell**.</span><span class="sxs-lookup"><span data-stu-id="daaa0-138">You can also choose to create a **New worksheet** or to **Add this data to a Data Model**.</span></span> <span data-ttu-id="daaa0-139">Mer information om datamodeller finns i [Skapa en datamodell i Excel](https://support.office.com/article/Create-a-Data-Model-in-Excel-87E7A54C-87DC-488E-9410-5C75DBCB0F7B).</span><span class="sxs-lookup"><span data-stu-id="daaa0-139">For more information on Data Models, see [Create a data model in Excel](https://support.office.com/article/Create-a-Data-Model-in-Excel-87E7A54C-87DC-488E-9410-5C75DBCB0F7B).</span></span> <span data-ttu-id="daaa0-140">Klicka på **Egenskaper** för att utforska information om odc-filen som du skapade i föregående steg och för att välja alternativ för datauppdatering.</span><span class="sxs-lookup"><span data-stu-id="daaa0-140">Click **Properties** to explore information about the odc file you created in the previous step and to choose options for refreshing the data.</span></span>
   
    ![Välja dataformat i Excel](./media/sql-database-connect-excel/import-data.png)
   
    <span data-ttu-id="daaa0-142">Kalkylbladet har nu en tom pivottabell och diagram.</span><span class="sxs-lookup"><span data-stu-id="daaa0-142">The worksheet now has an empty pivot table and chart.</span></span>
2. <span data-ttu-id="daaa0-143">Under **PivotTable-fält** väljer du alla kryssrutor för fälten du vill visa.</span><span class="sxs-lookup"><span data-stu-id="daaa0-143">Under **PivotTable Fields**, select all the check-boxes for the fields you want to view.</span></span>
   
    ![Konfigurera databasrapporten.](./media/sql-database-connect-excel/power-pivot-results.png)

> [!TIP]
> <span data-ttu-id="daaa0-145">Om du vill ansluta andra Excel-arbetsböcker och kalkylblad till databasen, klickar du på **Data**, klickar på **Anslutningar**, klickar på **Lägg till**, väljer den anslutning som du skapade i listan och klickar sedan på **Öppna**.</span><span class="sxs-lookup"><span data-stu-id="daaa0-145">If you want to connect other Excel workbooks and worksheets to the database, click **Data**, click **Connections**, click **Add**, choose the connection you created from the list, and then click **Open**.</span></span>
> <span data-ttu-id="daaa0-146">![Öppna en anslutning från en annan arbetsbok](./media/sql-database-connect-excel/open-from-another-workbook.png)</span><span class="sxs-lookup"><span data-stu-id="daaa0-146">![Open a connection from another workbook](./media/sql-database-connect-excel/open-from-another-workbook.png)</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="daaa0-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="daaa0-147">Next steps</span></span>
* <span data-ttu-id="daaa0-148">Lär dig hur du [Ansluter till SQL Database med SQL Server Management Studio](sql-database-connect-query-ssms.md) för avancerade frågor och analys.</span><span class="sxs-lookup"><span data-stu-id="daaa0-148">Learn how to [Connect to SQL Database with SQL Server Management Studio](sql-database-connect-query-ssms.md) for advanced querying and analysis.</span></span>
* <span data-ttu-id="daaa0-149">Lär dig mer om fördelarna med [elastiska pooler](sql-database-elastic-pool.md).</span><span class="sxs-lookup"><span data-stu-id="daaa0-149">Learn about the benefits of [elastic pools](sql-database-elastic-pool.md).</span></span>
* <span data-ttu-id="daaa0-150">Lär dig hur du [skapar en webbapp som ansluter till SQL Database på serverdelen](../app-service-web/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="daaa0-150">Learn how to [create a web application that connects to SQL Database on the back-end](../app-service-web/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md).</span></span>

