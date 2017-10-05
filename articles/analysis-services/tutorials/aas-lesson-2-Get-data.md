---
title: "Azure Analysis Services självstudiekurs 2: Hämta data | Microsoft Docs"
description: "Beskriver hur du hämtar och importerar data i Azure Analysis Services-självstudieprojektet."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 06/01/2017
ms.author: owend
ms.openlocfilehash: e77de4b9a74b528fa8a7ce86424fc14628b2cacc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="lesson-2-get-data"></a><span data-ttu-id="2af12-103">Lektion 2: Hämta data</span><span class="sxs-lookup"><span data-stu-id="2af12-103">Lesson 2: Get data</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="2af12-104">Under den här lektionen använder du Hämta data i SSDT för att ansluta till AdventureWorksDW2014-exempeldatabasen, väljer data, förhandsgranskar och filtrerar, och importerar sedan till modellarbetsytan.</span><span class="sxs-lookup"><span data-stu-id="2af12-104">In this lesson, you use Get Data in SSDT to connect to the AdventureWorksDW2014 sample database, select data, preview and filter, and then import into your model workspace.</span></span>  
  
<span data-ttu-id="2af12-105">Med Hämta data kan du importera data från en mängd olika datakällor: Azure SQL Database, Oracle, Sybase, OData-Feed, Teradata, filer med mera.</span><span class="sxs-lookup"><span data-stu-id="2af12-105">By using Get Data, you can import data from a wide variety of sources: Azure SQL Database, Oracle, Sybase, OData Feed, Teradata, files and more.</span></span> <span data-ttu-id="2af12-106">Det går även att fråga data med ett Power Query M-formeluttryck.</span><span class="sxs-lookup"><span data-stu-id="2af12-106">Data can also be queried using a Power Query M formula expression.</span></span>
  
<span data-ttu-id="2af12-107">Uppskattad tidsåtgång för den här lektionen: **10 minuter**</span><span class="sxs-lookup"><span data-stu-id="2af12-107">Estimated time to complete this lesson: **10 minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="2af12-108">Krav</span><span class="sxs-lookup"><span data-stu-id="2af12-108">Prerequisites</span></span>  
<span data-ttu-id="2af12-109">Det här avsnittet ingår i självstudiekursen för tabellmodellering som bör slutföras i rätt ordning.</span><span class="sxs-lookup"><span data-stu-id="2af12-109">This topic is part of a tabular modeling tutorial, which should be completed in order.</span></span> <span data-ttu-id="2af12-110">Innan du utför uppgifterna i den här lektionen bör du ha slutfört föregående lektion: [Lektion 1: Skapa ett nytt tabellmodellprojekt](../tutorials/aas-lesson-1-create-a-new-tabular-model-project.md).</span><span class="sxs-lookup"><span data-stu-id="2af12-110">Before performing the tasks in this lesson, you should have completed the previous lesson: [Lesson 1: Create a new tabular model project](../tutorials/aas-lesson-1-create-a-new-tabular-model-project.md).</span></span>  
  
## <a name="create-a-connection"></a><span data-ttu-id="2af12-111">Skapa en anslutning</span><span class="sxs-lookup"><span data-stu-id="2af12-111">Create a connection</span></span>  
  
#### <a name="to-create-a-connection-to-the-adventureworksdw2014-database"></a><span data-ttu-id="2af12-112">Att skapa en anslutning till databasen AdventureWorksDW2014</span><span class="sxs-lookup"><span data-stu-id="2af12-112">To create a connection to the AdventureWorksDW2014 database</span></span>  
  
1.  <span data-ttu-id="2af12-113">Högerklicka på **Datakällor** > **Importera från datakälla** i tabellmodellutforskaren.</span><span class="sxs-lookup"><span data-stu-id="2af12-113">In Tabular Model Explorer, right-click **Data Sources** > **Import from Data Source**.</span></span>  
  
    <span data-ttu-id="2af12-114">Det här startar Hämta data, som hjälper dig att ansluta till en datakälla.</span><span class="sxs-lookup"><span data-stu-id="2af12-114">This launches Get Data, which guides you through connecting to a data source.</span></span> <span data-ttu-id="2af12-115">Om du inte ser tabellmodellutforskaren i **Solution Explorer** dubbelklickar du på **Model.bim** för att öppna modellen i designern.</span><span class="sxs-lookup"><span data-stu-id="2af12-115">If you don't see Tabular Model Explorer, in **Solution Explorer**, double-click **Model.bim** to open the model in the designer.</span></span> 
    
    ![aas-lesson2-getdata](../tutorials/media/aas-lesson2-getdata.png)
  
2.  <span data-ttu-id="2af12-117">I Hämta Data klickar du på **Databas** > **SQL Server-databas** > **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="2af12-117">In Get Data, click **Database** > **SQL Server Database** > **Connect**.</span></span>  
  
3.  <span data-ttu-id="2af12-118">I dialogrutan **SQL Server-databas** i **Server** anger du namnet på den server där du installerade AdventureWorksDW2014-databasen och klickar sedan på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="2af12-118">In the **SQL Server Database** dialog, in **Server**, type the name of the server where you installed the AdventureWorksDW2014 database, and then click **Connect**.</span></span>  

4.  <span data-ttu-id="2af12-119">När du uppmanas att ange autentiseringsuppgifter så måste du ange de autentiseringsuppgifter som Analysis Services använder för att ansluta till datakällan vid importering och bearbetning av data.</span><span class="sxs-lookup"><span data-stu-id="2af12-119">When prompted to enter credentials, you need to specify the credentials Analysis Services uses to connect to the data source when importing and processing data.</span></span> <span data-ttu-id="2af12-120">I **personifieringsläget** väljer du **Personifiera konto**, anger autentiseringsuppgifter och klickar på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="2af12-120">In **Impersonation Mode**, select **Impersonate Account**, then enter credentials, and then click **Connect**.</span></span> <span data-ttu-id="2af12-121">Vi rekommenderar att du använder ett konto vars lösenord inte upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="2af12-121">It's recommended you use an account where the password doesn't expire.</span></span>

    ![aas-lesson2-account](../tutorials/media/aas-lesson2-account.png)
  
    > [!NOTE]  
    > <span data-ttu-id="2af12-123">Den säkraste metoden för att ansluta till en datakälla är att använda ett Windows-användarkonto och lösenord.</span><span class="sxs-lookup"><span data-stu-id="2af12-123">Using a Windows user account and password provides the most secure method of connecting to a data source.</span></span>
  
5.  <span data-ttu-id="2af12-124">I navigatören markerar du databasen **AdventureWorksDW2014** och klickar sedan på **OK**. Då skapas en anslutning till databasen.</span><span class="sxs-lookup"><span data-stu-id="2af12-124">In Navigator, select the **AdventureWorksDW2014** database, and then click **OK**.This creates the connection to the database.</span></span> 
  
6.  <span data-ttu-id="2af12-125">I navigatören markerar du kryssrutorna för följande tabeller: **DimCustomer**, **DimDate**, **DimGeography**, **DimProduct**, **DimProductCategory**, **DimProductSubcategory** och **FactInternetSales**.</span><span class="sxs-lookup"><span data-stu-id="2af12-125">In Navigator, select the check box for the following tables: **DimCustomer**, **DimDate**, **DimGeography**, **DimProduct**, **DimProductCategory**, **DimProductSubcategory**, and **FactInternetSales**.</span></span>  

    ![aas-lesson2-select-tables](../tutorials/media/aas-lesson2-select-tables.png)
  
<span data-ttu-id="2af12-127">När du klickar på OK öppnas frågeredigeraren.</span><span class="sxs-lookup"><span data-stu-id="2af12-127">After you click OK, Query Editor opens.</span></span> <span data-ttu-id="2af12-128">I nästa avsnitt väljer du endast de data som du vill importera.</span><span class="sxs-lookup"><span data-stu-id="2af12-128">In the next section, you select only the data you want to import.</span></span>

  
## <a name="filter-the-table-data"></a><span data-ttu-id="2af12-129">Filtrera tabelldata</span><span class="sxs-lookup"><span data-stu-id="2af12-129">Filter the table data</span></span>  
<span data-ttu-id="2af12-130">Tabeller i exempeldatabasen AdventureWorksDW2014 har data som inte är nödvändiga att ta med i modellen.</span><span class="sxs-lookup"><span data-stu-id="2af12-130">Tables in the AdventureWorksDW2014 sample database have data that isn't necessary to include in your model.</span></span> <span data-ttu-id="2af12-131">Om det är möjligt filtrerar du bort onödiga data för att spara på minnesutrymmet som används av modellen.</span><span class="sxs-lookup"><span data-stu-id="2af12-131">When possible, you want to filter out unnecessary data to save in-memory space used by the model.</span></span> <span data-ttu-id="2af12-132">Du kan filtrera bort vissa kolumner från tabeller så att de inte importeras till arbetsytedatabasen eller modelldatabasen när den har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="2af12-132">You filter out some of the columns from tables so they're not imported into the workspace database, or the model database after it has been deployed.</span></span> 
  
#### <a name="to-filter-the-table-data-before-importing"></a><span data-ttu-id="2af12-133">Filtrera tabelldata innan du importerar</span><span class="sxs-lookup"><span data-stu-id="2af12-133">To filter the table data before importing</span></span>  
  
1.  <span data-ttu-id="2af12-134">Markera tabellen **DimCustomer** i frågeredigeraren.</span><span class="sxs-lookup"><span data-stu-id="2af12-134">In Query Editor, select the **DimCustomer** table.</span></span> <span data-ttu-id="2af12-135">En vy av tabellen DimCustomer i datakällan (exempeldatabasen AdventureWorksDWQ2014) visas.</span><span class="sxs-lookup"><span data-stu-id="2af12-135">A view of the DimCustomer table at the datasource (your AdventureWorksDWQ2014 sample database) appears.</span></span> 
  
2.  <span data-ttu-id="2af12-136">Flermarkera (Ctrl + klicka) **SpanishEducation**, **FrenchEducation**, **SpanishOccupation**, **FrenchOccupation** och högerklicka och klicka sedan på **Ta bort kolumner**.</span><span class="sxs-lookup"><span data-stu-id="2af12-136">Multi-select (Ctrl + click) **SpanishEducation**, **FrenchEducation**, **SpanishOccupation**, **FrenchOccupation**, then right-click, and then click **Remove Columns**.</span></span> 

    ![aas-lesson2-remove-columns](../tutorials/media/aas-lesson2-remove-columns.png)
  
    <span data-ttu-id="2af12-138">Eftersom värdena för de här kolumnerna inte är relevanta för analysen av Internetförsäljning så behöver du inte importera dessa kolumner.</span><span class="sxs-lookup"><span data-stu-id="2af12-138">Since the values for these columns are not relevant to Internet sales analysis, there is no need to import these columns.</span></span> <span data-ttu-id="2af12-139">Genom att ta bort onödiga kolumner blir din modell mindre och effektivare.</span><span class="sxs-lookup"><span data-stu-id="2af12-139">Eliminating unnecessary columns makes your model smaller and more efficient.</span></span>  
  
4.  <span data-ttu-id="2af12-140">Filtrera de återstående tabellerna genom att ta bort följande kolumner i varje tabell:</span><span class="sxs-lookup"><span data-stu-id="2af12-140">Filter the remaining tables by removing the following columns in each table:</span></span>  
    
    <span data-ttu-id="2af12-141">**DimDate**</span><span class="sxs-lookup"><span data-stu-id="2af12-141">**DimDate**</span></span>
    
      |<span data-ttu-id="2af12-142">Kolumn</span><span class="sxs-lookup"><span data-stu-id="2af12-142">Column</span></span>|  
      |--------|  
      |<span data-ttu-id="2af12-143">DateKey</span><span class="sxs-lookup"><span data-stu-id="2af12-143">DateKey</span></span>|  
      |<span data-ttu-id="2af12-144">**SpanishDayNameOfWeek**</span><span class="sxs-lookup"><span data-stu-id="2af12-144">**SpanishDayNameOfWeek**</span></span>|  
      |<span data-ttu-id="2af12-145">**FrenchDayNameOfWeek**</span><span class="sxs-lookup"><span data-stu-id="2af12-145">**FrenchDayNameOfWeek**</span></span>|  
      |<span data-ttu-id="2af12-146">**SpanishMonthName**</span><span class="sxs-lookup"><span data-stu-id="2af12-146">**SpanishMonthName**</span></span>|  
      |<span data-ttu-id="2af12-147">**FrenchMonthName**</span><span class="sxs-lookup"><span data-stu-id="2af12-147">**FrenchMonthName**</span></span>|  
  
    <span data-ttu-id="2af12-148">**DimGeography**</span><span class="sxs-lookup"><span data-stu-id="2af12-148">**DimGeography**</span></span>
  
      |<span data-ttu-id="2af12-149">Kolumn</span><span class="sxs-lookup"><span data-stu-id="2af12-149">Column</span></span>|  
      |-------------|  
      |<span data-ttu-id="2af12-150">**SpanishCountryRegionName**</span><span class="sxs-lookup"><span data-stu-id="2af12-150">**SpanishCountryRegionName**</span></span>|  
      |<span data-ttu-id="2af12-151">**FrenchCountryRegionName**</span><span class="sxs-lookup"><span data-stu-id="2af12-151">**FrenchCountryRegionName**</span></span>|  
      |<span data-ttu-id="2af12-152">**IpAddressLocator**</span><span class="sxs-lookup"><span data-stu-id="2af12-152">**IpAddressLocator**</span></span>|  
  
    <span data-ttu-id="2af12-153">**DimProduct**</span><span class="sxs-lookup"><span data-stu-id="2af12-153">**DimProduct**</span></span>
  
      |<span data-ttu-id="2af12-154">Kolumn</span><span class="sxs-lookup"><span data-stu-id="2af12-154">Column</span></span>|  
      |-----------|  
      |<span data-ttu-id="2af12-155">**SpanishProductName**</span><span class="sxs-lookup"><span data-stu-id="2af12-155">**SpanishProductName**</span></span>|  
      |<span data-ttu-id="2af12-156">**FrenchProductName**</span><span class="sxs-lookup"><span data-stu-id="2af12-156">**FrenchProductName**</span></span>|  
      |<span data-ttu-id="2af12-157">**FrenchDescription**</span><span class="sxs-lookup"><span data-stu-id="2af12-157">**FrenchDescription**</span></span>|  
      |<span data-ttu-id="2af12-158">**ChineseDescription**</span><span class="sxs-lookup"><span data-stu-id="2af12-158">**ChineseDescription**</span></span>|  
      |<span data-ttu-id="2af12-159">**ArabicDescription**</span><span class="sxs-lookup"><span data-stu-id="2af12-159">**ArabicDescription**</span></span>|  
      |<span data-ttu-id="2af12-160">**HebrewDescription**</span><span class="sxs-lookup"><span data-stu-id="2af12-160">**HebrewDescription**</span></span>|  
      |<span data-ttu-id="2af12-161">**ThaiDescription**</span><span class="sxs-lookup"><span data-stu-id="2af12-161">**ThaiDescription**</span></span>|  
      |<span data-ttu-id="2af12-162">**GermanDescription**</span><span class="sxs-lookup"><span data-stu-id="2af12-162">**GermanDescription**</span></span>|  
      |<span data-ttu-id="2af12-163">**JapaneseDescription**</span><span class="sxs-lookup"><span data-stu-id="2af12-163">**JapaneseDescription**</span></span>|  
      |<span data-ttu-id="2af12-164">**TurkishDescription**</span><span class="sxs-lookup"><span data-stu-id="2af12-164">**TurkishDescription**</span></span>|  
  
    <span data-ttu-id="2af12-165">**DimProductCategory**</span><span class="sxs-lookup"><span data-stu-id="2af12-165">**DimProductCategory**</span></span>
  
      |<span data-ttu-id="2af12-166">Kolumn</span><span class="sxs-lookup"><span data-stu-id="2af12-166">Column</span></span>|  
      |--------------------|  
      |<span data-ttu-id="2af12-167">**SpanishProductCategoryName**</span><span class="sxs-lookup"><span data-stu-id="2af12-167">**SpanishProductCategoryName**</span></span>|  
      |<span data-ttu-id="2af12-168">**FrenchProductCategoryName**</span><span class="sxs-lookup"><span data-stu-id="2af12-168">**FrenchProductCategoryName**</span></span>|  
  
    <span data-ttu-id="2af12-169">**DimProductSubcategory**</span><span class="sxs-lookup"><span data-stu-id="2af12-169">**DimProductSubcategory**</span></span>
  
      |<span data-ttu-id="2af12-170">Kolumn</span><span class="sxs-lookup"><span data-stu-id="2af12-170">Column</span></span>|  
      |-----------------------|  
      |<span data-ttu-id="2af12-171">**SpanishProductSubcategoryName**</span><span class="sxs-lookup"><span data-stu-id="2af12-171">**SpanishProductSubcategoryName**</span></span>|  
      |<span data-ttu-id="2af12-172">**FrenchProductSubcategoryName**</span><span class="sxs-lookup"><span data-stu-id="2af12-172">**FrenchProductSubcategoryName**</span></span>|  
  
    <span data-ttu-id="2af12-173">**FactInternetSales**</span><span class="sxs-lookup"><span data-stu-id="2af12-173">**FactInternetSales**</span></span>
  
      |<span data-ttu-id="2af12-174">Kolumn</span><span class="sxs-lookup"><span data-stu-id="2af12-174">Column</span></span>|  
      |------------------|  
      |<span data-ttu-id="2af12-175">**OrderDateKey**</span><span class="sxs-lookup"><span data-stu-id="2af12-175">**OrderDateKey**</span></span>|  
      |<span data-ttu-id="2af12-176">**DueDateKey**</span><span class="sxs-lookup"><span data-stu-id="2af12-176">**DueDateKey**</span></span>|  
      |<span data-ttu-id="2af12-177">**ShipDateKey**</span><span class="sxs-lookup"><span data-stu-id="2af12-177">**ShipDateKey**</span></span>|   
  
## <span data-ttu-id="2af12-178"><a name="Import"></a>Importera de markerade tabellerna och kolumndata</span><span class="sxs-lookup"><span data-stu-id="2af12-178"><a name="Import"></a>Import the selected tables and column data</span></span>  
<span data-ttu-id="2af12-179">Nu när du har förhandsgranskat och filtrerat bort onödiga data kan du importera resten av de data som du behöver.</span><span class="sxs-lookup"><span data-stu-id="2af12-179">Now that you've previewed and filtered out unnecessary data, you can import the rest of the data you do want.</span></span> <span data-ttu-id="2af12-180">Med guiden importeras tabelldata och eventuella relationer mellan tabellerna.</span><span class="sxs-lookup"><span data-stu-id="2af12-180">The wizard imports the table data along with any relationships between tables.</span></span> <span data-ttu-id="2af12-181">Nya tabeller och kolumner skapas i modellen och data som du filtrerat ut importeras inte.</span><span class="sxs-lookup"><span data-stu-id="2af12-181">New tables and columns are created in the model and data that you filtered out is not be imported.</span></span>  
  
#### <a name="to-import-the-selected-tables-and-column-data"></a><span data-ttu-id="2af12-182">Så här importerar du de markerade tabellerna och kolumndata</span><span class="sxs-lookup"><span data-stu-id="2af12-182">To import the selected tables and column data</span></span>  
  
1.  <span data-ttu-id="2af12-183">Granska dina val.</span><span class="sxs-lookup"><span data-stu-id="2af12-183">Review your selections.</span></span> <span data-ttu-id="2af12-184">Om allt ser bra ut klickar du på **Importera**.</span><span class="sxs-lookup"><span data-stu-id="2af12-184">If everything looks okay, click **Import**.</span></span> <span data-ttu-id="2af12-185">I dialogrutan Databearbetning visas status för data som importeras från din datakälla till arbetsytedatabasen.</span><span class="sxs-lookup"><span data-stu-id="2af12-185">The Data Processing dialog shows the status of data being imported from your datasource into your workspace database.</span></span>
  
    ![aas-lesson2-success](../tutorials/media/aas-lesson2-success.png) 
  
2.  <span data-ttu-id="2af12-187">Klicka på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="2af12-187">Click **Close**.</span></span>  

  
## <a name="save-your-model-project"></a><span data-ttu-id="2af12-188">Spara modellprojektet</span><span class="sxs-lookup"><span data-stu-id="2af12-188">Save your model project</span></span>  
<span data-ttu-id="2af12-189">Det är viktigt att spara modellprojektet ofta.</span><span class="sxs-lookup"><span data-stu-id="2af12-189">It's important to frequently save your model project.</span></span>  
  
#### <a name="to-save-the-model-project"></a><span data-ttu-id="2af12-190">Så här sparar du modellprojektet</span><span class="sxs-lookup"><span data-stu-id="2af12-190">To save the model project</span></span>  
  
-   <span data-ttu-id="2af12-191">Klicka på **Arkiv** > **Spara alla**.</span><span class="sxs-lookup"><span data-stu-id="2af12-191">Click **File** > **Save All**.</span></span>  
  
## <a name="whats-next"></a><span data-ttu-id="2af12-192">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2af12-192">What's next?</span></span>
<span data-ttu-id="2af12-193">[Lektion 3: Markera som datumtabell](../tutorials/aas-lesson-3-mark-as-date-table.md).</span><span class="sxs-lookup"><span data-stu-id="2af12-193">[Lesson 3: Mark as Date Table](../tutorials/aas-lesson-3-mark-as-date-table.md).</span></span>

  
  
