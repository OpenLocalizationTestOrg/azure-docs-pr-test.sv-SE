---
title: aaaVisualize SQL Data Warehouse-data med Power BI Microsoft Azure
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
ms.openlocfilehash: 0425cf5abe7bc001b2a41df4d09bf5f2e42527e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="visualize-data-with-power-bi"></a><span data-ttu-id="b0d31-103">Visualisera data med Power BI</span><span class="sxs-lookup"><span data-stu-id="b0d31-103">Visualize data with Power BI</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b0d31-104">Power BI</span><span class="sxs-lookup"><span data-stu-id="b0d31-104">Power BI</span></span>](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [<span data-ttu-id="b0d31-105">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="b0d31-105">Azure Machine Learning</span></span>](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [<span data-ttu-id="b0d31-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b0d31-106">Visual Studio</span></span>](sql-data-warehouse-query-visual-studio.md)
> * [<span data-ttu-id="b0d31-107">sqlcmd</span><span class="sxs-lookup"><span data-stu-id="b0d31-107">sqlcmd</span></span>](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [<span data-ttu-id="b0d31-108">SSMS</span><span class="sxs-lookup"><span data-stu-id="b0d31-108">SSMS</span></span>](sql-data-warehouse-query-ssms.md)
> 
> 

<span data-ttu-id="b0d31-109">De här självstudierna visar hur toouse Power BI tooconnect tooSQL Data Warehouse och skapa några grundläggande visualiseringar.</span><span class="sxs-lookup"><span data-stu-id="b0d31-109">This tutorial shows you how toouse Power BI tooconnect tooSQL Data Warehouse and create a few basic visualizations.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Data-Warehouse-Sample-Data-and-PowerBI/player]
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="b0d31-110">Krav</span><span class="sxs-lookup"><span data-stu-id="b0d31-110">Prerequisites</span></span>
<span data-ttu-id="b0d31-111">toostep igenom den här kursen behöver du:</span><span class="sxs-lookup"><span data-stu-id="b0d31-111">toostep through this tutorial, you need:</span></span>

* <span data-ttu-id="b0d31-112">Ett SQL Data Warehouse förinstallerad med hello AdventureWorksDW-databasen.</span><span class="sxs-lookup"><span data-stu-id="b0d31-112">A SQL Data Warehouse pre-loaded with hello AdventureWorksDW database.</span></span> <span data-ttu-id="b0d31-113">tooprovision detta, se [skapa ett SQL Data Warehouse] [ Create a SQL Data Warehouse] och välj tooload hello exempeldata.</span><span class="sxs-lookup"><span data-stu-id="b0d31-113">tooprovision this, see [Create a SQL Data Warehouse][Create a SQL Data Warehouse] and choose tooload hello sample data.</span></span> <span data-ttu-id="b0d31-114">Om du redan har ett Data Warehouse men inte har exempeldata kan du [läsa in exempeldata manuellt][load sample data manually].</span><span class="sxs-lookup"><span data-stu-id="b0d31-114">If you already have a data warehouse but do not have sample data, you can [load sample data manually][load sample data manually].</span></span>

## <a name="1-connect-tooyour-database"></a><span data-ttu-id="b0d31-115">1. Ansluta tooyour databas</span><span class="sxs-lookup"><span data-stu-id="b0d31-115">1. Connect tooyour database</span></span>
<span data-ttu-id="b0d31-116">tooopen Power BI och ansluta tooyour AdventureWorksDW-databasen:</span><span class="sxs-lookup"><span data-stu-id="b0d31-116">tooopen Power BI and connect tooyour AdventureWorksDW database:</span></span>

1. <span data-ttu-id="b0d31-117">Logga in på hello [Azure-portalen][Azure portal].</span><span class="sxs-lookup"><span data-stu-id="b0d31-117">Sign into hello [Azure portal][Azure portal].</span></span>
2. <span data-ttu-id="b0d31-118">Klicka på **SQL-databaser** och välj din AdventureWorks SQL Data Warehouse-databas.</span><span class="sxs-lookup"><span data-stu-id="b0d31-118">Click **SQL databases** and choose your AdventureWorks SQL Data Warehouse database.</span></span>
   
    ![Hitta din databas][1]
3. <span data-ttu-id="b0d31-120">Klicka på hello ”öppna i Power BI-knappen.</span><span class="sxs-lookup"><span data-stu-id="b0d31-120">Click hello 'Open in Power BI' button.</span></span>
   
    ![Power BI-knappen][2]
4. <span data-ttu-id="b0d31-122">Du bör nu se hello SQL Data Warehouse anslutningssidan som visar webbadressen för databasen.</span><span class="sxs-lookup"><span data-stu-id="b0d31-122">You should now see hello SQL Data Warehouse connection page displaying your database web address.</span></span> <span data-ttu-id="b0d31-123">Klicka på Nästa.</span><span class="sxs-lookup"><span data-stu-id="b0d31-123">Click next.</span></span>
   
    ![Power BI-anslutning][3]
5. <span data-ttu-id="b0d31-125">Ange din Azure SQL server-användarnamn och lösenord och du blir fullt ansluten tooyour SQL Data Warehouse-databas.</span><span class="sxs-lookup"><span data-stu-id="b0d31-125">Enter your Azure SQL server username and password and you will be fully connected tooyour SQL Data Warehouse database.</span></span>
   
    ![Power BI-inloggning][4]
6. <span data-ttu-id="b0d31-127">Klicka på hello AdventureWorksDW-datauppsättningen på hello vänstra bladet när du har loggat in på Power BI.</span><span class="sxs-lookup"><span data-stu-id="b0d31-127">Once you have signed into Power BI, click hello AdventureWorksDW dataset on hello left blade.</span></span> <span data-ttu-id="b0d31-128">Hello databasen öppnas.</span><span class="sxs-lookup"><span data-stu-id="b0d31-128">This will open hello database.</span></span>
   
    ![Power BI öppna AdventureWorksDW][5]

## <a name="2-create-a-report"></a><span data-ttu-id="b0d31-130">2. Skapa en rapport</span><span class="sxs-lookup"><span data-stu-id="b0d31-130">2. Create a report</span></span>
<span data-ttu-id="b0d31-131">Du är nu redo toouse Power BI tooanalyze AdventureWorksDW-exempeldata.</span><span class="sxs-lookup"><span data-stu-id="b0d31-131">You are now ready toouse Power BI tooanalyze your AdventureWorksDW sample data.</span></span> <span data-ttu-id="b0d31-132">tooperform hello analysen har AdventureWorksDW en vy som heter AggregateSales.</span><span class="sxs-lookup"><span data-stu-id="b0d31-132">tooperform hello analysis, AdventureWorksDW has a view called AggregateSales.</span></span> <span data-ttu-id="b0d31-133">Den här vyn innehåller några hello viktiga mått för att analysera hello försäljningen av hello företag.</span><span class="sxs-lookup"><span data-stu-id="b0d31-133">This view contains a few of hello key metrics for analyzing hello sales of hello company.</span></span>

1. <span data-ttu-id="b0d31-134">toocreate en karta över försäljningsbelopp enligt toopostal kod i hello högra rutan, klicka på hello AggregateSales visa tooexpand den.</span><span class="sxs-lookup"><span data-stu-id="b0d31-134">toocreate a map of sales amount according toopostal code, in hello right-hand fields pane, click hello AggregateSales view tooexpand it.</span></span> <span data-ttu-id="b0d31-135">Klicka på hello PostalCode och SalesAmount kolumner tooselect dem.</span><span class="sxs-lookup"><span data-stu-id="b0d31-135">Click hello PostalCode and SalesAmount columns tooselect them.</span></span>
   
    ![Power BI välj AggregateSales][6]
   
    <span data-ttu-id="b0d31-137">Power BI identifierar automatiskt dessa geografiska data och placerar dem på en karta för dig.</span><span class="sxs-lookup"><span data-stu-id="b0d31-137">Power BI automatically recognizes this is geographic data and put it in a map for you.</span></span>
   
    ![Power BI-karta][7]
2. <span data-ttu-id="b0d31-139">Det här steget skapar ett stapeldiagram som visar försäljningsintäkter efter kund.</span><span class="sxs-lookup"><span data-stu-id="b0d31-139">This step creates a bar graph that shows amount of sales per customer income.</span></span> <span data-ttu-id="b0d31-140">toocreate denna gå toohello expanderas AggregateSales-vyn.</span><span class="sxs-lookup"><span data-stu-id="b0d31-140">toocreate this go toohello expanded AggregateSales view.</span></span> <span data-ttu-id="b0d31-141">Klicka på hello SalesAmount fält.</span><span class="sxs-lookup"><span data-stu-id="b0d31-141">Click hello SalesAmount field.</span></span> <span data-ttu-id="b0d31-142">Dra hello Customer Income fältet toohello vänster och släpp det i axeln.</span><span class="sxs-lookup"><span data-stu-id="b0d31-142">Drag hello Customer Income field toohello left and drop it into Axis.</span></span>
   
    ![Power BI välj axel][8]
   
    <span data-ttu-id="b0d31-144">Vi har flyttat hello liggande diagram över hello vänster.</span><span class="sxs-lookup"><span data-stu-id="b0d31-144">We moved hello bar chart over hello left.</span></span>
   
    ![Power BI-stapel][9]
3. <span data-ttu-id="b0d31-146">Det här steget skapar ett linjediagram som visar försäljning per beställningsdatum.</span><span class="sxs-lookup"><span data-stu-id="b0d31-146">This step creates a line chart that shows sales amount per order date.</span></span> <span data-ttu-id="b0d31-147">toocreate denna gå toohello expanderas AggregateSales-vyn.</span><span class="sxs-lookup"><span data-stu-id="b0d31-147">toocreate this go toohello expanded AggregateSales view.</span></span> <span data-ttu-id="b0d31-148">Klicka på SalesAmount och OrderDate.</span><span class="sxs-lookup"><span data-stu-id="b0d31-148">Click SalesAmount and OrderDate.</span></span> <span data-ttu-id="b0d31-149">Klicka i hello visualiseringar kolumnen hello linjediagram-ikonen. Detta är hello första ikonen i hello andra raden under visualiseringar.</span><span class="sxs-lookup"><span data-stu-id="b0d31-149">In hello Visualizations column click hello Line Chart icon; this is hello first icon in hello second line under visualizations.</span></span>
   
    ![Power BI välj linjediagram][10]
   
    <span data-ttu-id="b0d31-151">Nu har du en rapport som visar tre olika datavisualiseringar hello data.</span><span class="sxs-lookup"><span data-stu-id="b0d31-151">You now have a report that shows three different visualizations of hello data.</span></span>
   
    ![Power BI-linje][11]

<span data-ttu-id="b0d31-153">Du kan spara ditt arbete när som helst genom att klicka på **Arkiv** och välja **Spara**.</span><span class="sxs-lookup"><span data-stu-id="b0d31-153">You can save your progress at any time by clicking **File** and selecting **Save**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b0d31-154">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b0d31-154">Next steps</span></span>
<span data-ttu-id="b0d31-155">Nu när vi har gett dig vissa tid toowarm med exempeldata hello, se hur för[utveckla][develop], [ladda][load], eller [ Migrera][migrate].</span><span class="sxs-lookup"><span data-stu-id="b0d31-155">Now that we've given you some time toowarm up with hello sample data, see how too[develop][develop], [load][load], or [migrate][migrate].</span></span> <span data-ttu-id="b0d31-156">Eller titta på hello [Power BI-webbplatsen][Power BI website].</span><span class="sxs-lookup"><span data-stu-id="b0d31-156">Or take a look at hello [Power BI website][Power BI website].</span></span>

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
[connecting tooSQL Data Warehouse]: sql-data-warehouse-integrate-power-bi.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md

<!--Other-->
[Azure portal]: https://portal.azure.com/
[Power BI website]: http://www.powerbi.com/
