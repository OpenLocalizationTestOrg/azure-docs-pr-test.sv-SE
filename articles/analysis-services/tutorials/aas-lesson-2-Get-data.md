---
<span data-ttu-id="c2c46-101">Rubrik: aaa ”Azure Analysis Services självstudiekursen lektionen 2: hämta data | Microsoft Docs ”beskrivning: Beskriver hur tooget och importera data i hello självstudiekursen Azure Analysis Services-projekt.</span><span class="sxs-lookup"><span data-stu-id="c2c46-101">title: aaa"Azure Analysis Services tutorial lesson 2: Get data | Microsoft Docs" description: Describes how tooget and import data in hello Azure Analysis Services tutorial project.</span></span> <span data-ttu-id="c2c46-102">tjänster: analysis services dokumentationcenter: '' författare: minewiskan manager: erikre editor: '' taggar: ''</span><span class="sxs-lookup"><span data-stu-id="c2c46-102">services: analysis-services documentationcenter: '' author: minewiskan manager: erikre editor: '' tags: ''</span></span>

<span data-ttu-id="c2c46-103">MS.AssetID: ms.service: analysis services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 06/01/2017 ms.author: owend</span><span class="sxs-lookup"><span data-stu-id="c2c46-103">ms.assetid: ms.service: analysis-services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 06/01/2017 ms.author: owend</span></span>
---

# <a name="lesson-2-get-data"></a><span data-ttu-id="c2c46-104">Lektion 2: Hämta data</span><span class="sxs-lookup"><span data-stu-id="c2c46-104">Lesson 2: Get data</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="c2c46-105">Nu bör du använda hämta Data i SSDT tooconnect toohello AdventureWorksDW2014 exempeldatabasen, Välj data, förhandsgranskning och filter och sedan importera på din modell-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="c2c46-105">In this lesson, you use Get Data in SSDT tooconnect toohello AdventureWorksDW2014 sample database, select data, preview and filter, and then import into your model workspace.</span></span>  
  
<span data-ttu-id="c2c46-106">Med Hämta data kan du importera data från en mängd olika datakällor: Azure SQL Database, Oracle, Sybase, OData-Feed, Teradata, filer med mera.</span><span class="sxs-lookup"><span data-stu-id="c2c46-106">By using Get Data, you can import data from a wide variety of sources: Azure SQL Database, Oracle, Sybase, OData Feed, Teradata, files and more.</span></span> <span data-ttu-id="c2c46-107">Det går även att fråga data med ett Power Query M-formeluttryck.</span><span class="sxs-lookup"><span data-stu-id="c2c46-107">Data can also be queried using a Power Query M formula expression.</span></span>
  
<span data-ttu-id="c2c46-108">Uppskattad tid toocomplete lektionen: **10 minuter**</span><span class="sxs-lookup"><span data-stu-id="c2c46-108">Estimated time toocomplete this lesson: **10 minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="c2c46-109">Krav</span><span class="sxs-lookup"><span data-stu-id="c2c46-109">Prerequisites</span></span>  
<span data-ttu-id="c2c46-110">Det här avsnittet ingår i självstudiekursen för tabellmodellering som bör slutföras i rätt ordning.</span><span class="sxs-lookup"><span data-stu-id="c2c46-110">This topic is part of a tabular modeling tutorial, which should be completed in order.</span></span> <span data-ttu-id="c2c46-111">Innan du utför hello uppgifter i den här lektionen bör du slutfört hello föregående lektionen: [lektionen 1: skapa ett nytt projekt tabellmodell](../tutorials/aas-lesson-1-create-a-new-tabular-model-project.md).</span><span class="sxs-lookup"><span data-stu-id="c2c46-111">Before performing hello tasks in this lesson, you should have completed hello previous lesson: [Lesson 1: Create a new tabular model project](../tutorials/aas-lesson-1-create-a-new-tabular-model-project.md).</span></span>  
  
## <a name="create-a-connection"></a><span data-ttu-id="c2c46-112">Skapa en anslutning</span><span class="sxs-lookup"><span data-stu-id="c2c46-112">Create a connection</span></span>  
  
#### <a name="toocreate-a-connection-toohello-adventureworksdw2014-database"></a><span data-ttu-id="c2c46-113">toocreate en toohello AdventureWorksDW2014 databas</span><span class="sxs-lookup"><span data-stu-id="c2c46-113">toocreate a connection toohello AdventureWorksDW2014 database</span></span>  
  
1.  <span data-ttu-id="c2c46-114">Högerklicka på **Datakällor** > **Importera från datakälla** i tabellmodellutforskaren.</span><span class="sxs-lookup"><span data-stu-id="c2c46-114">In Tabular Model Explorer, right-click **Data Sources** > **Import from Data Source**.</span></span>  
  
    <span data-ttu-id="c2c46-115">Detta startar hämta Data som hjälper dig att anslutande tooa datakälla.</span><span class="sxs-lookup"><span data-stu-id="c2c46-115">This launches Get Data, which guides you through connecting tooa data source.</span></span> <span data-ttu-id="c2c46-116">Om du inte ser Tabular modellen Explorer i **Solution Explorer**, dubbelklicka på **Model.bim** tooopen hello modellen i hello designer.</span><span class="sxs-lookup"><span data-stu-id="c2c46-116">If you don't see Tabular Model Explorer, in **Solution Explorer**, double-click **Model.bim** tooopen hello model in hello designer.</span></span> 
    
    ![aas-lesson2-getdata](../tutorials/media/aas-lesson2-getdata.png)
  
2.  <span data-ttu-id="c2c46-118">I Hämta Data klickar du på **Databas** > **SQL Server-databas** > **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="c2c46-118">In Get Data, click **Database** > **SQL Server Database** > **Connect**.</span></span>  
  
3.  <span data-ttu-id="c2c46-119">I hello **SQL Server-databas** dialogrutan i **Server**hello typnamn för hello server där du installerade hello AdventureWorksDW2014 databasen och klicka sedan på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="c2c46-119">In hello **SQL Server Database** dialog, in **Server**, type hello name of hello server where you installed hello AdventureWorksDW2014 database, and then click **Connect**.</span></span>  

4.  <span data-ttu-id="c2c46-120">När du uppmanas tooenter autentiseringsuppgifter, behöver du toospecify hello autentiseringsuppgifter för Analysis Services använder tooconnect toohello datakällan vid import och bearbetning av data.</span><span class="sxs-lookup"><span data-stu-id="c2c46-120">When prompted tooenter credentials, you need toospecify hello credentials Analysis Services uses tooconnect toohello data source when importing and processing data.</span></span> <span data-ttu-id="c2c46-121">I **personifieringsläget** väljer du **Personifiera konto**, anger autentiseringsuppgifter och klickar på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="c2c46-121">In **Impersonation Mode**, select **Impersonate Account**, then enter credentials, and then click **Connect**.</span></span> <span data-ttu-id="c2c46-122">Det rekommenderas att du använder ett konto där hello lösenordet upphör inte att gälla.</span><span class="sxs-lookup"><span data-stu-id="c2c46-122">It's recommended you use an account where hello password doesn't expire.</span></span>

    ![aas-lesson2-account](../tutorials/media/aas-lesson2-account.png)
  
    > [!NOTE]  
    > <span data-ttu-id="c2c46-124">Med ett Windows-användarkonto och lösenord här hello säkraste metoden för anslutande tooa datakälla.</span><span class="sxs-lookup"><span data-stu-id="c2c46-124">Using a Windows user account and password provides hello most secure method of connecting tooa data source.</span></span>
  
5.  <span data-ttu-id="c2c46-125">Välj hello i Navigator **AdventureWorksDW2014** databasen och klicka sedan på **OK**. Detta skapar hello toohello databas.</span><span class="sxs-lookup"><span data-stu-id="c2c46-125">In Navigator, select hello **AdventureWorksDW2014** database, and then click **OK**.This creates hello connection toohello database.</span></span> 
  
6.  <span data-ttu-id="c2c46-126">I Navigator hello väljer du kryssrutan för hello följande tabeller: **DimCustomer**, **DimDate**, **DimGeography**, **DimProduct**,  **DimProductCategory**, **DimProductSubcategory**, och **FactInternetSales**.</span><span class="sxs-lookup"><span data-stu-id="c2c46-126">In Navigator, select hello check box for hello following tables: **DimCustomer**, **DimDate**, **DimGeography**, **DimProduct**, **DimProductCategory**, **DimProductSubcategory**, and **FactInternetSales**.</span></span>  

    ![aas-lesson2-select-tables](../tutorials/media/aas-lesson2-select-tables.png)
  
<span data-ttu-id="c2c46-128">När du klickar på OK öppnas frågeredigeraren.</span><span class="sxs-lookup"><span data-stu-id="c2c46-128">After you click OK, Query Editor opens.</span></span> <span data-ttu-id="c2c46-129">I nästa avsnitt av hello väljer du endast hello data som du vill tooimport.</span><span class="sxs-lookup"><span data-stu-id="c2c46-129">In hello next section, you select only hello data you want tooimport.</span></span>

  
## <a name="filter-hello-table-data"></a><span data-ttu-id="c2c46-130">Filtrera hello tabelldata</span><span class="sxs-lookup"><span data-stu-id="c2c46-130">Filter hello table data</span></span>  
<span data-ttu-id="c2c46-131">Tabeller i databasen hello AdventureWorksDW2014 har data som inte är nödvändiga tooinclude i modellen.</span><span class="sxs-lookup"><span data-stu-id="c2c46-131">Tables in hello AdventureWorksDW2014 sample database have data that isn't necessary tooinclude in your model.</span></span> <span data-ttu-id="c2c46-132">När det är möjligt, vill du toofilter ut onödiga data toosave i-minne som används av hello modell.</span><span class="sxs-lookup"><span data-stu-id="c2c46-132">When possible, you want toofilter out unnecessary data toosave in-memory space used by hello model.</span></span> <span data-ttu-id="c2c46-133">Du kan filtrera bort vissa hello kolumner från tabeller så att de inte har importerats till hello arbetsytan databas eller hello modelldatabasen när den har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="c2c46-133">You filter out some of hello columns from tables so they're not imported into hello workspace database, or hello model database after it has been deployed.</span></span> 
  
#### <a name="toofilter-hello-table-data-before-importing"></a><span data-ttu-id="c2c46-134">toofilter hello tabelldata innan du importerar</span><span class="sxs-lookup"><span data-stu-id="c2c46-134">toofilter hello table data before importing</span></span>  
  
1.  <span data-ttu-id="c2c46-135">I frågeredigeraren Välj hello **DimCustomer** tabell.</span><span class="sxs-lookup"><span data-stu-id="c2c46-135">In Query Editor, select hello **DimCustomer** table.</span></span> <span data-ttu-id="c2c46-136">En vy av hello DimCustomer tabellen hello datasource (AdventureWorksDWQ2014 exempeldatabasen) visas.</span><span class="sxs-lookup"><span data-stu-id="c2c46-136">A view of hello DimCustomer table at hello datasource (your AdventureWorksDWQ2014 sample database) appears.</span></span> 
  
2.  <span data-ttu-id="c2c46-137">Flermarkera (Ctrl + klicka) **SpanishEducation**, **FrenchEducation**, **SpanishOccupation**, **FrenchOccupation** och högerklicka och klicka sedan på **Ta bort kolumner**.</span><span class="sxs-lookup"><span data-stu-id="c2c46-137">Multi-select (Ctrl + click) **SpanishEducation**, **FrenchEducation**, **SpanishOccupation**, **FrenchOccupation**, then right-click, and then click **Remove Columns**.</span></span> 

    ![aas-lesson2-remove-columns](../tutorials/media/aas-lesson2-remove-columns.png)
  
    <span data-ttu-id="c2c46-139">Eftersom hello värdena för dessa kolumner inte är relevanta tooInternet försäljning analys, finns inga behov tooimport dessa kolumner.</span><span class="sxs-lookup"><span data-stu-id="c2c46-139">Since hello values for these columns are not relevant tooInternet sales analysis, there is no need tooimport these columns.</span></span> <span data-ttu-id="c2c46-140">Genom att ta bort onödiga kolumner blir din modell mindre och effektivare.</span><span class="sxs-lookup"><span data-stu-id="c2c46-140">Eliminating unnecessary columns makes your model smaller and more efficient.</span></span>  
  
4.  <span data-ttu-id="c2c46-141">Filtrera hello återstående tabeller genom att ta bort hello följande kolumner i varje tabell:</span><span class="sxs-lookup"><span data-stu-id="c2c46-141">Filter hello remaining tables by removing hello following columns in each table:</span></span>  
    
    <span data-ttu-id="c2c46-142">**DimDate**</span><span class="sxs-lookup"><span data-stu-id="c2c46-142">**DimDate**</span></span>
    
      |<span data-ttu-id="c2c46-143">Kolumn</span><span class="sxs-lookup"><span data-stu-id="c2c46-143">Column</span></span>|  
      |--------|  
      |<span data-ttu-id="c2c46-144">DateKey</span><span class="sxs-lookup"><span data-stu-id="c2c46-144">DateKey</span></span>|  
      |<span data-ttu-id="c2c46-145">**SpanishDayNameOfWeek**</span><span class="sxs-lookup"><span data-stu-id="c2c46-145">**SpanishDayNameOfWeek**</span></span>|  
      |<span data-ttu-id="c2c46-146">**FrenchDayNameOfWeek**</span><span class="sxs-lookup"><span data-stu-id="c2c46-146">**FrenchDayNameOfWeek**</span></span>|  
      |<span data-ttu-id="c2c46-147">**SpanishMonthName**</span><span class="sxs-lookup"><span data-stu-id="c2c46-147">**SpanishMonthName**</span></span>|  
      |<span data-ttu-id="c2c46-148">**FrenchMonthName**</span><span class="sxs-lookup"><span data-stu-id="c2c46-148">**FrenchMonthName**</span></span>|  
  
    <span data-ttu-id="c2c46-149">**DimGeography**</span><span class="sxs-lookup"><span data-stu-id="c2c46-149">**DimGeography**</span></span>
  
      |<span data-ttu-id="c2c46-150">Kolumn</span><span class="sxs-lookup"><span data-stu-id="c2c46-150">Column</span></span>|  
      |-------------|  
      |<span data-ttu-id="c2c46-151">**SpanishCountryRegionName**</span><span class="sxs-lookup"><span data-stu-id="c2c46-151">**SpanishCountryRegionName**</span></span>|  
      |<span data-ttu-id="c2c46-152">**FrenchCountryRegionName**</span><span class="sxs-lookup"><span data-stu-id="c2c46-152">**FrenchCountryRegionName**</span></span>|  
      |<span data-ttu-id="c2c46-153">**IpAddressLocator**</span><span class="sxs-lookup"><span data-stu-id="c2c46-153">**IpAddressLocator**</span></span>|  
  
    <span data-ttu-id="c2c46-154">**DimProduct**</span><span class="sxs-lookup"><span data-stu-id="c2c46-154">**DimProduct**</span></span>
  
      |<span data-ttu-id="c2c46-155">Kolumn</span><span class="sxs-lookup"><span data-stu-id="c2c46-155">Column</span></span>|  
      |-----------|  
      |<span data-ttu-id="c2c46-156">**SpanishProductName**</span><span class="sxs-lookup"><span data-stu-id="c2c46-156">**SpanishProductName**</span></span>|  
      |<span data-ttu-id="c2c46-157">**FrenchProductName**</span><span class="sxs-lookup"><span data-stu-id="c2c46-157">**FrenchProductName**</span></span>|  
      |<span data-ttu-id="c2c46-158">**FrenchDescription**</span><span class="sxs-lookup"><span data-stu-id="c2c46-158">**FrenchDescription**</span></span>|  
      |<span data-ttu-id="c2c46-159">**ChineseDescription**</span><span class="sxs-lookup"><span data-stu-id="c2c46-159">**ChineseDescription**</span></span>|  
      |<span data-ttu-id="c2c46-160">**ArabicDescription**</span><span class="sxs-lookup"><span data-stu-id="c2c46-160">**ArabicDescription**</span></span>|  
      |<span data-ttu-id="c2c46-161">**HebrewDescription**</span><span class="sxs-lookup"><span data-stu-id="c2c46-161">**HebrewDescription**</span></span>|  
      |<span data-ttu-id="c2c46-162">**ThaiDescription**</span><span class="sxs-lookup"><span data-stu-id="c2c46-162">**ThaiDescription**</span></span>|  
      |<span data-ttu-id="c2c46-163">**GermanDescription**</span><span class="sxs-lookup"><span data-stu-id="c2c46-163">**GermanDescription**</span></span>|  
      |<span data-ttu-id="c2c46-164">**JapaneseDescription**</span><span class="sxs-lookup"><span data-stu-id="c2c46-164">**JapaneseDescription**</span></span>|  
      |<span data-ttu-id="c2c46-165">**TurkishDescription**</span><span class="sxs-lookup"><span data-stu-id="c2c46-165">**TurkishDescription**</span></span>|  
  
    <span data-ttu-id="c2c46-166">**DimProductCategory**</span><span class="sxs-lookup"><span data-stu-id="c2c46-166">**DimProductCategory**</span></span>
  
      |<span data-ttu-id="c2c46-167">Kolumn</span><span class="sxs-lookup"><span data-stu-id="c2c46-167">Column</span></span>|  
      |--------------------|  
      |<span data-ttu-id="c2c46-168">**SpanishProductCategoryName**</span><span class="sxs-lookup"><span data-stu-id="c2c46-168">**SpanishProductCategoryName**</span></span>|  
      |<span data-ttu-id="c2c46-169">**FrenchProductCategoryName**</span><span class="sxs-lookup"><span data-stu-id="c2c46-169">**FrenchProductCategoryName**</span></span>|  
  
    <span data-ttu-id="c2c46-170">**DimProductSubcategory**</span><span class="sxs-lookup"><span data-stu-id="c2c46-170">**DimProductSubcategory**</span></span>
  
      |<span data-ttu-id="c2c46-171">Kolumn</span><span class="sxs-lookup"><span data-stu-id="c2c46-171">Column</span></span>|  
      |-----------------------|  
      |<span data-ttu-id="c2c46-172">**SpanishProductSubcategoryName**</span><span class="sxs-lookup"><span data-stu-id="c2c46-172">**SpanishProductSubcategoryName**</span></span>|  
      |<span data-ttu-id="c2c46-173">**FrenchProductSubcategoryName**</span><span class="sxs-lookup"><span data-stu-id="c2c46-173">**FrenchProductSubcategoryName**</span></span>|  
  
    <span data-ttu-id="c2c46-174">**FactInternetSales**</span><span class="sxs-lookup"><span data-stu-id="c2c46-174">**FactInternetSales**</span></span>
  
      |<span data-ttu-id="c2c46-175">Kolumn</span><span class="sxs-lookup"><span data-stu-id="c2c46-175">Column</span></span>|  
      |------------------|  
      |<span data-ttu-id="c2c46-176">**OrderDateKey**</span><span class="sxs-lookup"><span data-stu-id="c2c46-176">**OrderDateKey**</span></span>|  
      |<span data-ttu-id="c2c46-177">**DueDateKey**</span><span class="sxs-lookup"><span data-stu-id="c2c46-177">**DueDateKey**</span></span>|  
      |<span data-ttu-id="c2c46-178">**ShipDateKey**</span><span class="sxs-lookup"><span data-stu-id="c2c46-178">**ShipDateKey**</span></span>|   
  
## <span data-ttu-id="c2c46-179"><a name="Import"></a>Importera hello valt kolumndata</span><span class="sxs-lookup"><span data-stu-id="c2c46-179"><a name="Import"></a>Import hello selected tables and column data</span></span>  
<span data-ttu-id="c2c46-180">Nu när du förhandsgranskas och filtreras bort onödiga data kan importera du hello resten av hello data som du vill.</span><span class="sxs-lookup"><span data-stu-id="c2c46-180">Now that you've previewed and filtered out unnecessary data, you can import hello rest of hello data you do want.</span></span> <span data-ttu-id="c2c46-181">hello guiden importerar hello tabelldata tillsammans med eventuella relationer mellan tabeller.</span><span class="sxs-lookup"><span data-stu-id="c2c46-181">hello wizard imports hello table data along with any relationships between tables.</span></span> <span data-ttu-id="c2c46-182">Nya tabeller och kolumner som skapas i hello modellen och importeras inte data som du har filtrerats ut.</span><span class="sxs-lookup"><span data-stu-id="c2c46-182">New tables and columns are created in hello model and data that you filtered out is not be imported.</span></span>  
  
#### <a name="tooimport-hello-selected-tables-and-column-data"></a><span data-ttu-id="c2c46-183">tooimport hello valt kolumndata</span><span class="sxs-lookup"><span data-stu-id="c2c46-183">tooimport hello selected tables and column data</span></span>  
  
1.  <span data-ttu-id="c2c46-184">Granska dina val.</span><span class="sxs-lookup"><span data-stu-id="c2c46-184">Review your selections.</span></span> <span data-ttu-id="c2c46-185">Om allt ser bra ut klickar du på **Importera**.</span><span class="sxs-lookup"><span data-stu-id="c2c46-185">If everything looks okay, click **Import**.</span></span> <span data-ttu-id="c2c46-186">hello databearbetning dialogrutan visar hello status för data som importeras från din datakälla till din arbetsyta-databas.</span><span class="sxs-lookup"><span data-stu-id="c2c46-186">hello Data Processing dialog shows hello status of data being imported from your datasource into your workspace database.</span></span>
  
    ![aas-lesson2-success](../tutorials/media/aas-lesson2-success.png) 
  
2.  <span data-ttu-id="c2c46-188">Klicka på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="c2c46-188">Click **Close**.</span></span>  

  
## <a name="save-your-model-project"></a><span data-ttu-id="c2c46-189">Spara modellprojektet</span><span class="sxs-lookup"><span data-stu-id="c2c46-189">Save your model project</span></span>  
<span data-ttu-id="c2c46-190">Det är viktigt toofrequently spara projektet modellen.</span><span class="sxs-lookup"><span data-stu-id="c2c46-190">It's important toofrequently save your model project.</span></span>  
  
#### <a name="toosave-hello-model-project"></a><span data-ttu-id="c2c46-191">toosave hello modellprojekt</span><span class="sxs-lookup"><span data-stu-id="c2c46-191">toosave hello model project</span></span>  
  
-   <span data-ttu-id="c2c46-192">Klicka på **Arkiv** > **Spara alla**.</span><span class="sxs-lookup"><span data-stu-id="c2c46-192">Click **File** > **Save All**.</span></span>  
  
## <a name="whats-next"></a><span data-ttu-id="c2c46-193">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c2c46-193">What's next?</span></span>
<span data-ttu-id="c2c46-194">[Lektion 3: Markera som datumtabell](../tutorials/aas-lesson-3-mark-as-date-table.md).</span><span class="sxs-lookup"><span data-stu-id="c2c46-194">[Lesson 3: Mark as Date Table](../tutorials/aas-lesson-3-mark-as-date-table.md).</span></span>

  
  
