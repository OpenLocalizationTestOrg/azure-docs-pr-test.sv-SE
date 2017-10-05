---
title: Visualisera SQL Data Warehouse-data med Power BI Microsoft Azure
description: Visualisera SQL Data Warehouse-data med Power BI
services: sql-data-warehouse
documentationcenter: NA
author: mlee3gsd
manager: jhubbard
editor: 
ms.assetid: d7fb89d1-da1d-4788-a111-68d0e3fda799
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 10/31/2016
ms.author: martinle;barbkess
ms.openlocfilehash: a41393730143b14e91318a61858d989fff3786c1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="visualize-data-with-power-bi"></a><span data-ttu-id="95031-103">Visualisera data med Power BI</span><span class="sxs-lookup"><span data-stu-id="95031-103">Visualize data with Power BI</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="95031-104">Power BI</span><span class="sxs-lookup"><span data-stu-id="95031-104">Power BI</span></span>](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [<span data-ttu-id="95031-105">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="95031-105">Azure Machine Learning</span></span>](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [<span data-ttu-id="95031-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="95031-106">Visual Studio</span></span>](sql-data-warehouse-query-visual-studio.md)
> * [<span data-ttu-id="95031-107">sqlcmd</span><span class="sxs-lookup"><span data-stu-id="95031-107">sqlcmd</span></span>](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [<span data-ttu-id="95031-108">SSMS</span><span class="sxs-lookup"><span data-stu-id="95031-108">SSMS</span></span>](sql-data-warehouse-query-ssms.md)
> 
> 

<span data-ttu-id="95031-109">De här självstudierna visar hur du använder Power BI för att ansluta till SQL Data Warehouse och skapa några grundläggande visualiseringar.</span><span class="sxs-lookup"><span data-stu-id="95031-109">This tutorial shows you how to use Power BI to connect to SQL Data Warehouse and create a few basic visualizations.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Data-Warehouse-Sample-Data-and-PowerBI/player]
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="95031-110">Krav</span><span class="sxs-lookup"><span data-stu-id="95031-110">Prerequisites</span></span>
<span data-ttu-id="95031-111">För att gå igenom de här självstudierna, behöver du:</span><span class="sxs-lookup"><span data-stu-id="95031-111">To step through this tutorial, you need:</span></span>

* <span data-ttu-id="95031-112">En SQL Data Warehouse förinstallerad med AdventureWorksDW-databasen.</span><span class="sxs-lookup"><span data-stu-id="95031-112">A SQL Data Warehouse pre-loaded with the AdventureWorksDW database.</span></span> <span data-ttu-id="95031-113">Om du vill distribuera detta läser du [Skapa ett SQL Data Warehouse][Create a SQL Data Warehouse] och väljer att läsa in exempeldata.</span><span class="sxs-lookup"><span data-stu-id="95031-113">To provision this, see [Create a SQL Data Warehouse][Create a SQL Data Warehouse] and choose to load the sample data.</span></span> <span data-ttu-id="95031-114">Om du redan har ett Data Warehouse men inte har exempeldata kan du [läsa in exempeldata manuellt][load sample data manually].</span><span class="sxs-lookup"><span data-stu-id="95031-114">If you already have a data warehouse but do not have sample data, you can [load sample data manually][load sample data manually].</span></span>

## <a name="1-connect-to-your-database"></a><span data-ttu-id="95031-115">1. Ansluta till databasen</span><span class="sxs-lookup"><span data-stu-id="95031-115">1. Connect to your database</span></span>
<span data-ttu-id="95031-116">Om du vill öppna Power BI och ansluta till databasen AdventureWorksDW:</span><span class="sxs-lookup"><span data-stu-id="95031-116">To open Power BI and connect to your AdventureWorksDW database:</span></span>

1. <span data-ttu-id="95031-117">Logga in på [Azure Portal][Azure portal].</span><span class="sxs-lookup"><span data-stu-id="95031-117">Sign into the [Azure portal][Azure portal].</span></span>
2. <span data-ttu-id="95031-118">Klicka på **SQL-databaser** och välj din AdventureWorks SQL Data Warehouse-databas.</span><span class="sxs-lookup"><span data-stu-id="95031-118">Click **SQL databases** and choose your AdventureWorks SQL Data Warehouse database.</span></span>
   
    ![Hitta din databas][1]
3. <span data-ttu-id="95031-120">Klicka på knappen Öppna i Power BI.</span><span class="sxs-lookup"><span data-stu-id="95031-120">Click the 'Open in Power BI' button.</span></span>
   
    ![Power BI-knappen][2]
4. <span data-ttu-id="95031-122">Du bör nu se SQL Data Warehouse-anslutningssidan som visar webbadressen för databasen.</span><span class="sxs-lookup"><span data-stu-id="95031-122">You should now see the SQL Data Warehouse connection page displaying your database web address.</span></span> <span data-ttu-id="95031-123">Klicka på Nästa.</span><span class="sxs-lookup"><span data-stu-id="95031-123">Click next.</span></span>
   
    ![Power BI-anslutning][3]
5. <span data-ttu-id="95031-125">Ange ditt användarnamn och lösenord för Azure SQL-servern så kommer du att anslutas fullständigt till SQL Data Warehouse-databasen.</span><span class="sxs-lookup"><span data-stu-id="95031-125">Enter your Azure SQL server username and password and you will be fully connected to your SQL Data Warehouse database.</span></span>
   
    ![Power BI-inloggning][4]
6. <span data-ttu-id="95031-127">Klicka på AdventureWorksDW-datauppsättningen på det vänstra bladet när du har loggat in på Power BI.</span><span class="sxs-lookup"><span data-stu-id="95031-127">Once you have signed into Power BI, click the AdventureWorksDW dataset on the left blade.</span></span> <span data-ttu-id="95031-128">Databasen öppnas.</span><span class="sxs-lookup"><span data-stu-id="95031-128">This will open the database.</span></span>
   
    ![Power BI öppna AdventureWorksDW][5]

## <a name="2-create-a-report"></a><span data-ttu-id="95031-130">2. Skapa en rapport</span><span class="sxs-lookup"><span data-stu-id="95031-130">2. Create a report</span></span>
<span data-ttu-id="95031-131">Du är nu redo att använda Power BI för att analysera dina AdventureWorksDW-exempeldata.</span><span class="sxs-lookup"><span data-stu-id="95031-131">You are now ready to use Power BI to analyze your AdventureWorksDW sample data.</span></span> <span data-ttu-id="95031-132">För att utföra analysen har AdventureWorksDW en vy som heter AggregateSales.</span><span class="sxs-lookup"><span data-stu-id="95031-132">To perform the analysis, AdventureWorksDW has a view called AggregateSales.</span></span> <span data-ttu-id="95031-133">Vyn innehåller några nyckelvärden för att analysera företagets försäljning.</span><span class="sxs-lookup"><span data-stu-id="95031-133">This view contains a few of the key metrics for analyzing the sales of the company.</span></span>

1. <span data-ttu-id="95031-134">Klicka på vyn AggregateSales i högra fältet för att expandera den och skapa en karta över försäljningsbelopp enligt postnummer.</span><span class="sxs-lookup"><span data-stu-id="95031-134">To create a map of sales amount according to postal code, in the right-hand fields pane, click the AggregateSales view to expand it.</span></span> <span data-ttu-id="95031-135">Klicka på kolumnerna PostalCode och SalesAmount för att markera dem.</span><span class="sxs-lookup"><span data-stu-id="95031-135">Click the PostalCode and SalesAmount columns to select them.</span></span>
   
    ![Power BI välj AggregateSales][6]
   
    <span data-ttu-id="95031-137">Power BI identifierar automatiskt dessa geografiska data och placerar dem på en karta för dig.</span><span class="sxs-lookup"><span data-stu-id="95031-137">Power BI automatically recognizes this is geographic data and put it in a map for you.</span></span>
   
    ![Power BI-karta][7]
2. <span data-ttu-id="95031-139">Det här steget skapar ett stapeldiagram som visar försäljningsintäkter efter kund.</span><span class="sxs-lookup"><span data-stu-id="95031-139">This step creates a bar graph that shows amount of sales per customer income.</span></span> <span data-ttu-id="95031-140">För att skapa detta går du till den expanderade AggregateSales-vyn.</span><span class="sxs-lookup"><span data-stu-id="95031-140">To create this go to the expanded AggregateSales view.</span></span> <span data-ttu-id="95031-141">Klicka på fältet SalesAmount.</span><span class="sxs-lookup"><span data-stu-id="95031-141">Click the SalesAmount field.</span></span> <span data-ttu-id="95031-142">Dra fältet Customer Income till vänster och släpp det i axeln.</span><span class="sxs-lookup"><span data-stu-id="95031-142">Drag the Customer Income field to the left and drop it into Axis.</span></span>
   
    ![Power BI välj axel][8]
   
    <span data-ttu-id="95031-144">Vi har flyttat över stapeldiagrammet till vänster.</span><span class="sxs-lookup"><span data-stu-id="95031-144">We moved the bar chart over the left.</span></span>
   
    ![Power BI-stapel][9]
3. <span data-ttu-id="95031-146">Det här steget skapar ett linjediagram som visar försäljning per beställningsdatum.</span><span class="sxs-lookup"><span data-stu-id="95031-146">This step creates a line chart that shows sales amount per order date.</span></span> <span data-ttu-id="95031-147">För att skapa detta går du till den expanderade AggregateSales-vyn.</span><span class="sxs-lookup"><span data-stu-id="95031-147">To create this go to the expanded AggregateSales view.</span></span> <span data-ttu-id="95031-148">Klicka på SalesAmount och OrderDate.</span><span class="sxs-lookup"><span data-stu-id="95031-148">Click SalesAmount and OrderDate.</span></span> <span data-ttu-id="95031-149">I kolumnen Visualiseringar klickar du på Linjediagram-ikonen. Den första ikonen i den andra raden under visualiseringar.</span><span class="sxs-lookup"><span data-stu-id="95031-149">In the Visualizations column click the Line Chart icon; this is the first icon in the second line under visualizations.</span></span>
   
    ![Power BI välj linjediagram][10]
   
    <span data-ttu-id="95031-151">Nu har du en rapport som visar tre olika datavisualiseringar.</span><span class="sxs-lookup"><span data-stu-id="95031-151">You now have a report that shows three different visualizations of the data.</span></span>
   
    ![Power BI-linje][11]

<span data-ttu-id="95031-153">Du kan spara ditt arbete när som helst genom att klicka på **Arkiv** och välja **Spara**.</span><span class="sxs-lookup"><span data-stu-id="95031-153">You can save your progress at any time by clicking **File** and selecting **Save**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="95031-154">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="95031-154">Next steps</span></span>
<span data-ttu-id="95031-155">Nu när du har värmt upp med exempeldata, kan du se mer information om att [utveckla][develop], [läsa in][load] eller [migrera][migrate].</span><span class="sxs-lookup"><span data-stu-id="95031-155">Now that we've given you some time to warm up with the sample data, see how to [develop][develop], [load][load], or [migrate][migrate].</span></span> <span data-ttu-id="95031-156">Eller ta en titt på [Power BI-webbplatsen][Power BI website].</span><span class="sxs-lookup"><span data-stu-id="95031-156">Or take a look at the [Power BI website][Power BI website].</span></span>

<!--Image references-->
[1]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-find-database.png
[2]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-button.png
[3]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-connect-to-azure.png
[4]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-sign-in.png
[5]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-open-adventureworks.png
[6]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-aggregatesales.png
[7]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-map.png
[8]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-chooseaxis.png
[9]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-bar.png
[10]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-prepare-line.png
[11]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-line.png
[12]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-save.png

<!--Article references-->
[migrate]: sql-data-warehouse-overview-migrate.md
[develop]: sql-data-warehouse-overview-develop.md
[load]: sql-data-warehouse-overview-load.md
[load sample data manually]: sql-data-warehouse-load-sample-databases.md
[connecting to SQL Data Warehouse]: sql-data-warehouse-integrate-power-bi.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md

<!--Other-->
[Azure portal]: https://portal.azure.com/
[Power BI website]: http://www.powerbi.com/
