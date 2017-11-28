---
<span data-ttu-id="0edf1-101">Rubrik: aaa ”Azure Analysis Services självstudiekursen lektionen 6: skapa mått | Microsoft Docs ”beskrivning: Beskriver hur toocreate åtgärder i självstudiekursen hello Azure Analysis Services-projekt.</span><span class="sxs-lookup"><span data-stu-id="0edf1-101">title: aaa"Azure Analysis Services tutorial lesson 6: Create measures | Microsoft Docs" description: Describes how toocreate measures in hello Azure Analysis Services tutorial project.</span></span> <span data-ttu-id="0edf1-102">tjänster: analysis services dokumentationcenter: '' författare: minewiskan manager: erikre editor: '' taggar: ''</span><span class="sxs-lookup"><span data-stu-id="0edf1-102">services: analysis-services documentationcenter: '' author: minewiskan manager: erikre editor: '' tags: ''</span></span>

<span data-ttu-id="0edf1-103">MS.AssetID: ms.service: analysis services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 06/01/2017 ms.author: owend</span><span class="sxs-lookup"><span data-stu-id="0edf1-103">ms.assetid: ms.service: analysis-services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 06/01/2017 ms.author: owend</span></span>
---
# <a name="lesson-6-create-measures"></a><span data-ttu-id="0edf1-104">Lektion 6: Skapa mått</span><span class="sxs-lookup"><span data-stu-id="0edf1-104">Lesson 6: Create measures</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="0edf1-105">Nu bör skapa du mått toobe ingår i din modell.</span><span class="sxs-lookup"><span data-stu-id="0edf1-105">In this lesson, you create measures toobe included in your model.</span></span> <span data-ttu-id="0edf1-106">Liknande toohello beräknade kolumner som du har skapat, ett mått är en beräkning som skapats med hjälp av en DAX-formel.</span><span class="sxs-lookup"><span data-stu-id="0edf1-106">Similar toohello calculated columns you created, a measure is a calculation created by using a DAX formula.</span></span> <span data-ttu-id="0edf1-107">Men till skillnad från beräknade kolumner utvärderas mått baserat på ett *filter* som en användare har valt.</span><span class="sxs-lookup"><span data-stu-id="0edf1-107">However, unlike calculated columns, measures are evaluated based on a user selected *filter*.</span></span> <span data-ttu-id="0edf1-108">Till exempel lägga en viss kolumn eller utsnitt toohello radetiketter fält i en pivottabell.</span><span class="sxs-lookup"><span data-stu-id="0edf1-108">For example, a particular column or slicer added toohello Row Labels field in a PivotTable.</span></span> <span data-ttu-id="0edf1-109">Ett värde för varje cell i hello filter beräknas sedan genom hello tillämpas mått.</span><span class="sxs-lookup"><span data-stu-id="0edf1-109">A value for each cell in hello filter is then calculated by hello applied measure.</span></span> <span data-ttu-id="0edf1-110">Åtgärder är kraftfulla, flexibla beräkningar som du vill tooinclude i nästan alla tabellmodeller tooperform dynamiska beräkningar på numeriska data.</span><span class="sxs-lookup"><span data-stu-id="0edf1-110">Measures are powerful, flexible calculations that you want tooinclude in almost all tabular models tooperform dynamic calculations on numerical data.</span></span> <span data-ttu-id="0edf1-111">Det finns fler toolearn [åtgärder](https://docs.microsoft.com/sql/analysis-services/tabular-models/measures-ssas-tabular).</span><span class="sxs-lookup"><span data-stu-id="0edf1-111">toolearn more, see [Measures](https://docs.microsoft.com/sql/analysis-services/tabular-models/measures-ssas-tabular).</span></span>
  
<span data-ttu-id="0edf1-112">toocreate åtgärder som du använder hello *mått rutnätet*.</span><span class="sxs-lookup"><span data-stu-id="0edf1-112">toocreate measures, you use hello *Measure Grid*.</span></span> <span data-ttu-id="0edf1-113">Varje tabell har som standard ett tomt rutnät för mått, men vanligtvis skapar du inte mått för varje tabell.</span><span class="sxs-lookup"><span data-stu-id="0edf1-113">By default, each table has an empty measure grid; however, you typically do not create measures for every table.</span></span> <span data-ttu-id="0edf1-114">hello mått rutnätet visas under en tabell i hello modellen designer i datavyn.</span><span class="sxs-lookup"><span data-stu-id="0edf1-114">hello measure grid appears below a table in hello model designer when in Data View.</span></span> <span data-ttu-id="0edf1-115">toohide eller visa hello mått rutnät för en tabell, klickar du på hello **tabell** -menyn och klicka sedan på **visa mått rutnät**.</span><span class="sxs-lookup"><span data-stu-id="0edf1-115">toohide or show hello measure grid for a table, click hello **Table** menu, and then click **Show Measure Grid**.</span></span>  
  
<span data-ttu-id="0edf1-116">Du kan skapa ett mått genom att klicka på en tom cell i hello mått rutnätet och sedan skriva en DAX-formel i formelfältet hello.</span><span class="sxs-lookup"><span data-stu-id="0edf1-116">You can create a measure by clicking an empty cell in hello measure grid, and then typing a DAX formula in hello formula bar.</span></span> <span data-ttu-id="0edf1-117">När du klickar du på RETUR toocomplete hello formel, hello mått visas sedan i hello cell.</span><span class="sxs-lookup"><span data-stu-id="0edf1-117">When you click ENTER toocomplete hello formula, hello measure then appears in hello cell.</span></span> <span data-ttu-id="0edf1-118">Du kan också skapa mått med hjälp av en standard aggregeringsfunktionen genom att klicka på en kolumn och sedan klicka på hello Autosumma (**∑**) hello i verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="0edf1-118">You can also create measures using a standard aggregation function by clicking a column, and then clicking hello AutoSum button (**∑**) on hello toolbar.</span></span> <span data-ttu-id="0edf1-119">Åtgärder som skapats med hjälp av funktionen för hello Autosumma visas i hello mått rutnätscell direkt under hello kolumn, men kan flyttas.</span><span class="sxs-lookup"><span data-stu-id="0edf1-119">Measures created using hello AutoSum feature appear in hello measure grid cell directly beneath hello column, but can be moved.</span></span>  
  
<span data-ttu-id="0edf1-120">I kursen skapar du mått med både en DAX-formel inom hello formelfältet och med hello Autosumma-funktionen.</span><span class="sxs-lookup"><span data-stu-id="0edf1-120">In this lesson, you create measures by both entering a DAX formula in hello formula bar, and by using hello AutoSum feature.</span></span>  
  
<span data-ttu-id="0edf1-121">Uppskattad tid toocomplete lektionen: **30 minuter**</span><span class="sxs-lookup"><span data-stu-id="0edf1-121">Estimated time toocomplete this lesson: **30 minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="0edf1-122">Krav</span><span class="sxs-lookup"><span data-stu-id="0edf1-122">Prerequisites</span></span>  
<span data-ttu-id="0edf1-123">Det här avsnittet ingår i självstudiekursen för tabellmodellering som bör slutföras i rätt ordning.</span><span class="sxs-lookup"><span data-stu-id="0edf1-123">This topic is part of a tabular modeling tutorial, which should be completed in order.</span></span> <span data-ttu-id="0edf1-124">Innan du utför hello uppgifter i den här lektionen bör du slutfört hello föregående lektionen: [lektionen 5: skapa beräknade kolumner](../tutorials/aas-lesson-5-create-calculated-columns.md).</span><span class="sxs-lookup"><span data-stu-id="0edf1-124">Before performing hello tasks in this lesson, you should have completed hello previous lesson: [Lesson 5: Create calculated columns](../tutorials/aas-lesson-5-create-calculated-columns.md).</span></span>  
  
## <a name="create-measures"></a><span data-ttu-id="0edf1-125">Skapa mått</span><span class="sxs-lookup"><span data-stu-id="0edf1-125">Create measures</span></span>  
  
#### <a name="toocreate-a-dayscurrentquartertodate-measure-in-hello-dimdate-table"></a><span data-ttu-id="0edf1-126">toocreate ett DaysCurrentQuarterToDate mått i hello DimDate tabell</span><span class="sxs-lookup"><span data-stu-id="0edf1-126">toocreate a DaysCurrentQuarterToDate measure in hello DimDate table</span></span>  
  
1.  <span data-ttu-id="0edf1-127">Klicka på hello i hello modellen designer **DimDate** tabell.</span><span class="sxs-lookup"><span data-stu-id="0edf1-127">In hello model designer, click hello **DimDate** table.</span></span>  
  
2.  <span data-ttu-id="0edf1-128">Klicka på hello övre vänstra tom cell i hello mått rutnätet.</span><span class="sxs-lookup"><span data-stu-id="0edf1-128">In hello measure grid, click hello top-left empty cell.</span></span>  
  
3.  <span data-ttu-id="0edf1-129">Skriv i formelfältet hello hello följande formel:</span><span class="sxs-lookup"><span data-stu-id="0edf1-129">In hello formula bar, type hello following formula:</span></span>  
  
    ```
    DaysCurrentQuarterToDate:=COUNTROWS( DATESQTD( 'DimDate'[Date])) 
    ```
  
    <span data-ttu-id="0edf1-130">Meddelande hello övre vänstra cellen innehåller nu ett Måttnamn **DaysCurrentQuarterToDate**, följt av hello resultatet **92**.</span><span class="sxs-lookup"><span data-stu-id="0edf1-130">Notice hello top-left cell now contains a measure name, **DaysCurrentQuarterToDate**, followed by hello result, **92**.</span></span>
    
      ![aas-lesson6-newmeasure](../tutorials/media/aas-lesson6-newmeasure.png) 
    
    <span data-ttu-id="0edf1-132">Till skillnad från beräknade kolumner med formler för mått skriver du hello Måttnamn följt av ett kolon och hello formeln uttryck.</span><span class="sxs-lookup"><span data-stu-id="0edf1-132">Unlike calculated columns, with measure formulas you can type hello measure name, followed by a colon, followed by hello formula expression.</span></span>

  
#### <a name="toocreate-a-daysincurrentquarter-measure-in-hello-dimdate-table"></a><span data-ttu-id="0edf1-133">toocreate ett DaysInCurrentQuarter mått i hello DimDate tabell</span><span class="sxs-lookup"><span data-stu-id="0edf1-133">toocreate a DaysInCurrentQuarter measure in hello DimDate table</span></span>  
  
1.  <span data-ttu-id="0edf1-134">Med hello **DimDate** tabell fortfarande aktiva i hello modellen designer i hello mått rutnät, hello tom cell nedan hello mått som du skapade.</span><span class="sxs-lookup"><span data-stu-id="0edf1-134">With hello **DimDate** table still active in hello model designer, in hello measure grid, click hello empty cell below hello measure you created.</span></span>  
  
2.  <span data-ttu-id="0edf1-135">Skriv i formelfältet hello hello följande formel:</span><span class="sxs-lookup"><span data-stu-id="0edf1-135">In hello formula bar, type hello following formula:</span></span>  
  
    ```
    DaysInCurrentQuarter:=COUNTROWS( DATESBETWEEN( 'DimDate'[Date], STARTOFQUARTER( LASTDATE('DimDate'[Date])), ENDOFQUARTER('DimDate'[Date])))
    ```
  
    <span data-ttu-id="0edf1-136">När du skapar en jämförelse förhållandet mellan en ofullständig period och hello föregående period.</span><span class="sxs-lookup"><span data-stu-id="0edf1-136">When creating a comparison ratio between one incomplete period and hello previous period.</span></span> <span data-ttu-id="0edf1-137">hello formeln måste beräkna hello andelen hello period som har gått ut och jämför den toohello samma procentandel hello föregående period.</span><span class="sxs-lookup"><span data-stu-id="0edf1-137">hello formula must calculate hello proportion of hello period that has elapsed and compare it toohello same proportion in hello previous period.</span></span> <span data-ttu-id="0edf1-138">I detta fall [DaysCurrentQuarterToDate] / [DaysInCurrentQuarter] ger hello andelen förfluten tid i hello aktuella perioden.</span><span class="sxs-lookup"><span data-stu-id="0edf1-138">In this case, [DaysCurrentQuarterToDate]/[DaysInCurrentQuarter] gives hello proportion elapsed in hello current period.</span></span>  
  
#### <a name="toocreate-an-internetdistinctcountsalesorder-measure-in-hello-factinternetsales-table"></a><span data-ttu-id="0edf1-139">toocreate ett InternetDistinctCountSalesOrder mått i hello FactInternetSales tabell</span><span class="sxs-lookup"><span data-stu-id="0edf1-139">toocreate an InternetDistinctCountSalesOrder measure in hello FactInternetSales table</span></span>  
  
1.  <span data-ttu-id="0edf1-140">Klicka på hello **FactInternetSales** tabell.</span><span class="sxs-lookup"><span data-stu-id="0edf1-140">Click hello **FactInternetSales** table.</span></span>   
  
2.  <span data-ttu-id="0edf1-141">Klicka på hello **SalesOrderNumber** kolumnrubriken.</span><span class="sxs-lookup"><span data-stu-id="0edf1-141">Click hello **SalesOrderNumber** column heading.</span></span>  
  
3.  <span data-ttu-id="0edf1-142">Hello nedåtpilen nästa toohello Autosumma på hello verktygsfältet (**∑**) och välj sedan **DistinctCount**.</span><span class="sxs-lookup"><span data-stu-id="0edf1-142">On hello toolbar, click hello down-arrow next toohello AutoSum (**∑**) button, and then select **DistinctCount**.</span></span>  
  
    <span data-ttu-id="0edf1-143">hello Autosumma funktionen skapar automatiskt ett mått för hello markerade kolumnen, med formeln för hello DistinctCount standard aggregering.</span><span class="sxs-lookup"><span data-stu-id="0edf1-143">hello AutoSum feature automatically creates a measure for hello selected column using hello DistinctCount standard aggregation formula.</span></span>  
    
       ![aas-lesson6-newmeasure2](../tutorials/media/aas-lesson6-newmeasure2.png)
  
4.  <span data-ttu-id="0edf1-145">I hello mått rutnätet klickar du på hello nytt mått, och klicka sedan på hello **egenskaper** fönster i **Måttnamn**, byta namn på hello mått för**InternetDistinctCountSalesOrder**.</span><span class="sxs-lookup"><span data-stu-id="0edf1-145">In hello measure grid, click hello new measure, and then in hello **Properties** window, in **Measure Name**, rename hello measure too**InternetDistinctCountSalesOrder**.</span></span> 
 
  
#### <a name="toocreate-additional-measures-in-hello-factinternetsales-table"></a><span data-ttu-id="0edf1-146">toocreate ytterligare åtgärder i hello FactInternetSales tabell</span><span class="sxs-lookup"><span data-stu-id="0edf1-146">toocreate additional measures in hello FactInternetSales table</span></span>  
  
1.  <span data-ttu-id="0edf1-147">Med funktionen hello Autosumma kan skapa och namnet hello följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="0edf1-147">By using hello AutoSum feature, create and name hello following measures:</span></span>  

    |<span data-ttu-id="0edf1-148">Kolumn</span><span class="sxs-lookup"><span data-stu-id="0edf1-148">Column</span></span>|<span data-ttu-id="0edf1-149">Måttnamn</span><span class="sxs-lookup"><span data-stu-id="0edf1-149">Measure name</span></span>|<span data-ttu-id="0edf1-150">Autosumma (∑)</span><span class="sxs-lookup"><span data-stu-id="0edf1-150">AutoSum (∑)</span></span>|<span data-ttu-id="0edf1-151">Formel</span><span class="sxs-lookup"><span data-stu-id="0edf1-151">Formula</span></span>|  
    |----------------|----------|-----------------|-----------|  
    |<span data-ttu-id="0edf1-152">SalesOrderLineNumber</span><span class="sxs-lookup"><span data-stu-id="0edf1-152">SalesOrderLineNumber</span></span>|<span data-ttu-id="0edf1-153">InternetOrderLinesCount</span><span class="sxs-lookup"><span data-stu-id="0edf1-153">InternetOrderLinesCount</span></span>|<span data-ttu-id="0edf1-154">Antal</span><span class="sxs-lookup"><span data-stu-id="0edf1-154">Count</span></span>|<span data-ttu-id="0edf1-155">=COUNTA([SalesOrderLineNumber])</span><span class="sxs-lookup"><span data-stu-id="0edf1-155">=COUNTA([SalesOrderLineNumber])</span></span>|  
    |<span data-ttu-id="0edf1-156">OrderQuantity</span><span class="sxs-lookup"><span data-stu-id="0edf1-156">OrderQuantity</span></span>|<span data-ttu-id="0edf1-157">InternetTotalUnits</span><span class="sxs-lookup"><span data-stu-id="0edf1-157">InternetTotalUnits</span></span>|<span data-ttu-id="0edf1-158">Summa</span><span class="sxs-lookup"><span data-stu-id="0edf1-158">Sum</span></span>|<span data-ttu-id="0edf1-159">=SUM([OrderQuantity])</span><span class="sxs-lookup"><span data-stu-id="0edf1-159">=SUM([OrderQuantity])</span></span>|  
    |<span data-ttu-id="0edf1-160">DiscountAmount</span><span class="sxs-lookup"><span data-stu-id="0edf1-160">DiscountAmount</span></span>|<span data-ttu-id="0edf1-161">InternetTotalDiscountAmount</span><span class="sxs-lookup"><span data-stu-id="0edf1-161">InternetTotalDiscountAmount</span></span>|<span data-ttu-id="0edf1-162">Summa</span><span class="sxs-lookup"><span data-stu-id="0edf1-162">Sum</span></span>|<span data-ttu-id="0edf1-163">=SUM([DiscountAmount])</span><span class="sxs-lookup"><span data-stu-id="0edf1-163">=SUM([DiscountAmount])</span></span>|  
    |<span data-ttu-id="0edf1-164">TotalProductCost</span><span class="sxs-lookup"><span data-stu-id="0edf1-164">TotalProductCost</span></span>|<span data-ttu-id="0edf1-165">InternetTotalProductCost</span><span class="sxs-lookup"><span data-stu-id="0edf1-165">InternetTotalProductCost</span></span>|<span data-ttu-id="0edf1-166">Summa</span><span class="sxs-lookup"><span data-stu-id="0edf1-166">Sum</span></span>|<span data-ttu-id="0edf1-167">=SUM([TotalProductCost])</span><span class="sxs-lookup"><span data-stu-id="0edf1-167">=SUM([TotalProductCost])</span></span>|  
    |<span data-ttu-id="0edf1-168">SalesAmount</span><span class="sxs-lookup"><span data-stu-id="0edf1-168">SalesAmount</span></span>|<span data-ttu-id="0edf1-169">InternetTotalSales</span><span class="sxs-lookup"><span data-stu-id="0edf1-169">InternetTotalSales</span></span>|<span data-ttu-id="0edf1-170">Summa</span><span class="sxs-lookup"><span data-stu-id="0edf1-170">Sum</span></span>|<span data-ttu-id="0edf1-171">=SUM([SalesAmount])</span><span class="sxs-lookup"><span data-stu-id="0edf1-171">=SUM([SalesAmount])</span></span>|  
    |<span data-ttu-id="0edf1-172">Marginal</span><span class="sxs-lookup"><span data-stu-id="0edf1-172">Margin</span></span>|<span data-ttu-id="0edf1-173">InternetTotalMargin</span><span class="sxs-lookup"><span data-stu-id="0edf1-173">InternetTotalMargin</span></span>|<span data-ttu-id="0edf1-174">Summa</span><span class="sxs-lookup"><span data-stu-id="0edf1-174">Sum</span></span>|<span data-ttu-id="0edf1-175">=SUM([Margin])</span><span class="sxs-lookup"><span data-stu-id="0edf1-175">=SUM([Margin])</span></span>|  
    |<span data-ttu-id="0edf1-176">TaxAmt</span><span class="sxs-lookup"><span data-stu-id="0edf1-176">TaxAmt</span></span>|<span data-ttu-id="0edf1-177">InternetTotalTaxAmt</span><span class="sxs-lookup"><span data-stu-id="0edf1-177">InternetTotalTaxAmt</span></span>|<span data-ttu-id="0edf1-178">Summa</span><span class="sxs-lookup"><span data-stu-id="0edf1-178">Sum</span></span>|<span data-ttu-id="0edf1-179">=SUM([TaxAmt])</span><span class="sxs-lookup"><span data-stu-id="0edf1-179">=SUM([TaxAmt])</span></span>|  
    |<span data-ttu-id="0edf1-180">Frakt</span><span class="sxs-lookup"><span data-stu-id="0edf1-180">Freight</span></span>|<span data-ttu-id="0edf1-181">InternetTotalFreight</span><span class="sxs-lookup"><span data-stu-id="0edf1-181">InternetTotalFreight</span></span>|<span data-ttu-id="0edf1-182">Summa</span><span class="sxs-lookup"><span data-stu-id="0edf1-182">Sum</span></span>|<span data-ttu-id="0edf1-183">=SUM([Freight])</span><span class="sxs-lookup"><span data-stu-id="0edf1-183">=SUM([Freight])</span></span>|  
  
2.  <span data-ttu-id="0edf1-184">Genom att klicka på en tom cell i hello mått rutnät och genom att använda hello formelfältet, skapa och namnet hello följande åtgärder i ordning:</span><span class="sxs-lookup"><span data-stu-id="0edf1-184">By clicking an empty cell in hello measure grid, and by using hello formula bar, create, and name hello following measures in order:</span></span>  
  
      ```
      InternetPreviousQuarterMargin:=CALCULATE([InternetTotalMargin],PREVIOUSQUARTER('DimDate'[Date]))
      ```
      
      ```
      InternetCurrentQuarterMargin:=TOTALQTD([InternetTotalMargin],'DimDate'[Date])
      ```
  
      ```
      InternetPreviousQuarterMarginProportionToQTD:=[InternetPreviousQuarterMargin]*([DaysCurrentQuarterToDate]/[DaysInCurrentQuarter])
      ```
  
      ```
      InternetPreviousQuarterSales:=CALCULATE([InternetTotalSales],PREVIOUSQUARTER('DimDate'[Date]))
      ```
  
      ```
      InternetCurrentQuarterSales:=TOTALQTD([InternetTotalSales],'DimDate'[Date])
      ```
      
      ```
      InternetPreviousQuarterSalesProportionToQTD:=[InternetPreviousQuarterSales]*([DaysCurrentQuarterToDate]/[DaysInCurrentQuarter])
      ```
  
<span data-ttu-id="0edf1-185">Åtgärder för hello FactInternetSales tabell kan vara används tooanalyze kritiska ekonomiska data, till exempel försäljning, kostnader och vinst för objekt som definieras av valda hello Användarfilter.</span><span class="sxs-lookup"><span data-stu-id="0edf1-185">Measures created for hello FactInternetSales table can be used tooanalyze critical financial data such as sales, costs, and profit margin for items defined by hello user selected filter.</span></span>  
  
## <a name="whats-next"></a><span data-ttu-id="0edf1-186">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0edf1-186">What's next?</span></span>
<span data-ttu-id="0edf1-187">[Lektion 7: Skapa KPI:er](../tutorials/aas-lesson-7-create-key-performance-indicators.md).</span><span class="sxs-lookup"><span data-stu-id="0edf1-187">[Lesson 7: Create Key Performance Indicators](../tutorials/aas-lesson-7-create-key-performance-indicators.md).</span></span>  

  
