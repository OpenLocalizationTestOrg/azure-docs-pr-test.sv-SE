---
<span data-ttu-id="44331-101">Rubrik: aaa ”Azure Analysis Services självstudiekursen lektionen 5: skapa beräknade kolumner | Microsoft Docs ”beskrivning: Beskriver hur toocreate beräknade kolumner i självstudiekursen hello Azure Analysis Services-projekt.</span><span class="sxs-lookup"><span data-stu-id="44331-101">title: aaa"Azure Analysis Services tutorial lesson 5: Create calculated columns | Microsoft Docs" description: Describes how toocreate calculated columns in hello Azure Analysis Services tutorial project.</span></span> <span data-ttu-id="44331-102">tjänster: analysis services dokumentationcenter: '' författare: minewiskan manager: erikre editor: '' taggar: ''</span><span class="sxs-lookup"><span data-stu-id="44331-102">services: analysis-services documentationcenter: '' author: minewiskan manager: erikre editor: '' tags: ''</span></span>

<span data-ttu-id="44331-103">MS.AssetID: ms.service: analysis services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 06/01/2017 ms.author: owend</span><span class="sxs-lookup"><span data-stu-id="44331-103">ms.assetid: ms.service: analysis-services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 06/01/2017 ms.author: owend</span></span>
---
# <a name="lesson-5-create-calculated-columns"></a><span data-ttu-id="44331-104">Lektion 5: Skapa beräknade kolumner</span><span class="sxs-lookup"><span data-stu-id="44331-104">Lesson 5: Create calculated columns</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="44331-105">Under den här lektionen skapar du data i modellen genom att lägga till beräknade kolumner.</span><span class="sxs-lookup"><span data-stu-id="44331-105">In this lesson, you create data in your model by adding calculated columns.</span></span> <span data-ttu-id="44331-106">Du kan skapa beräknade kolumner (som anpassade kolumner) när du använder hämta Data med hjälp av hello Query Editor eller senare i hello modellen designer liknande gör du här.</span><span class="sxs-lookup"><span data-stu-id="44331-106">You can create calculated columns (as custom columns) when using Get Data, by using hello Query Editor, or later in hello model designer like you do here.</span></span> <span data-ttu-id="44331-107">Det finns fler toolearn [beräknade kolumner](https://docs.microsoft.com/sql/analysis-services/tabular-models/ssas-calculated-columns).</span><span class="sxs-lookup"><span data-stu-id="44331-107">toolearn more, see [Calculated columns](https://docs.microsoft.com/sql/analysis-services/tabular-models/ssas-calculated-columns).</span></span>
  
<span data-ttu-id="44331-108">Du kan skapa fem nya beräknade kolumner i tre olika tabeller.</span><span class="sxs-lookup"><span data-stu-id="44331-108">You create five new calculated columns in three different tables.</span></span> <span data-ttu-id="44331-109">hello stegen är något annorlunda för varje aktivitet som visar att det finns flera sätt toocreate kolumner, byta namn på dem och placerar dem på olika platser i en tabell.</span><span class="sxs-lookup"><span data-stu-id="44331-109">hello steps are slightly different for each task showing there are several ways toocreate columns, rename them, and place them in various locations in a table.</span></span>  

<span data-ttu-id="44331-110">Det är även den här lektionen där du först använder dataanalysuttryck (DAX).</span><span class="sxs-lookup"><span data-stu-id="44331-110">This lesson is also where you first use Data Analysis Expressions (DAX).</span></span> <span data-ttu-id="44331-111">DAX är ett särskilt språk för att skapa mycket anpassningsbara formeluttryck för tabellmodeller.</span><span class="sxs-lookup"><span data-stu-id="44331-111">DAX is a special language for creating highly customizable formula expressions for tabular models.</span></span> <span data-ttu-id="44331-112">I den här kursen använder du DAX toocreate beräknade kolumner, mått och filter för rollen.</span><span class="sxs-lookup"><span data-stu-id="44331-112">In this tutorial, you use DAX toocreate calculated columns, measures, and role filters.</span></span> <span data-ttu-id="44331-113">Det finns fler toolearn [DAX i tabellmodeller](https://docs.microsoft.com/sql/analysis-services/tabular-models/understanding-dax-in-tabular-models-ssas-tabular).</span><span class="sxs-lookup"><span data-stu-id="44331-113">toolearn more, see [DAX in tabular models](https://docs.microsoft.com/sql/analysis-services/tabular-models/understanding-dax-in-tabular-models-ssas-tabular).</span></span> 
  
<span data-ttu-id="44331-114">Uppskattad tid toocomplete lektionen: **15 minuter**</span><span class="sxs-lookup"><span data-stu-id="44331-114">Estimated time toocomplete this lesson: **15 minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="44331-115">Krav</span><span class="sxs-lookup"><span data-stu-id="44331-115">Prerequisites</span></span>  
<span data-ttu-id="44331-116">Det här avsnittet ingår i självstudiekursen för tabellmodellering som bör slutföras i rätt ordning.</span><span class="sxs-lookup"><span data-stu-id="44331-116">This topic is part of a tabular modeling tutorial, which should be completed in order.</span></span> <span data-ttu-id="44331-117">Innan du utför hello uppgifter i den här lektionen bör du slutfört hello föregående lektionen: [lektionen 4: skapa relationer](../tutorials/aas-lesson-4-create-relationships.md).</span><span class="sxs-lookup"><span data-stu-id="44331-117">Before performing hello tasks in this lesson, you should have completed hello previous lesson: [Lesson 4: Create relationships](../tutorials/aas-lesson-4-create-relationships.md).</span></span> 
  
## <a name="create-calculated-columns"></a><span data-ttu-id="44331-118">Skapa beräknade kolumner</span><span class="sxs-lookup"><span data-stu-id="44331-118">Create calculated columns</span></span>  
  
#### <a name="create-a-monthcalendar-calculated-column-in-hello-dimdate-table"></a><span data-ttu-id="44331-119">Skapa en beräknad kolumn MonthCalendar i hello DimDate tabell</span><span class="sxs-lookup"><span data-stu-id="44331-119">Create a MonthCalendar calculated column in hello DimDate table</span></span>  
  
1.  <span data-ttu-id="44331-120">Klicka på hello **modellen** menyn > **Model-View** > **datavy**.</span><span class="sxs-lookup"><span data-stu-id="44331-120">Click hello **Model** menu > **Model View** > **Data View**.</span></span>  
  
    <span data-ttu-id="44331-121">Beräknade kolumner kan endast skapas med hjälp av hello modellen designer i datavyn.</span><span class="sxs-lookup"><span data-stu-id="44331-121">Calculated columns can only be created by using hello model designer in Data View.</span></span>  
  
2.  <span data-ttu-id="44331-122">Klicka på hello i hello modellen designer **DimDate** tabellen (fliken).</span><span class="sxs-lookup"><span data-stu-id="44331-122">In hello model designer, click hello **DimDate** table (tab).</span></span>  
  
3.  <span data-ttu-id="44331-123">Högerklicka på hello **CalendarQuarter** kolumnrubriken och klicka sedan på **Infoga kolumn**.</span><span class="sxs-lookup"><span data-stu-id="44331-123">Right-click hello **CalendarQuarter** column header, and then click **Insert Column**.</span></span>  
  
    <span data-ttu-id="44331-124">En ny kolumn med namnet **beräknad kolumn 1** är infogade toohello till vänster i hello **kvartal** kolumn.</span><span class="sxs-lookup"><span data-stu-id="44331-124">A new column named **Calculated Column 1** is inserted toohello left of hello **Calendar Quarter** column.</span></span>  
  
4.  <span data-ttu-id="44331-125">Skriv i hello formelfältet ovanför hello tabell hello följande DAX-formel: AutoComplete hjälper till att du skriver hello fullständiga namn på kolumner och tabeller och visar hello funktioner som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="44331-125">In hello formula bar above hello table, type hello following DAX formula: AutoComplete helps you type hello fully qualified names of columns and tables, and lists hello functions that are available.</span></span>  
  
    ```  
    =RIGHT(" " & FORMAT([MonthNumberOfYear],"#0"), 2) & " - " & [EnglishMonthName]  
    ``` 
  
    <span data-ttu-id="44331-126">Värden fylls sedan för alla hello rader i hello beräknad kolumn.</span><span class="sxs-lookup"><span data-stu-id="44331-126">Values are then populated for all hello rows in hello calculated column.</span></span> <span data-ttu-id="44331-127">Om du rulla hello tabellen, visas rader kan ha olika värden för den här kolumnen baserat på hello data på varje rad.</span><span class="sxs-lookup"><span data-stu-id="44331-127">If you scroll down through hello table, you see rows can have different values for this column, based on hello data in each row.</span></span>    
  
5.  <span data-ttu-id="44331-128">Byt namn på den här kolumnen för**MonthCalendar**.</span><span class="sxs-lookup"><span data-stu-id="44331-128">Rename this column too**MonthCalendar**.</span></span> 

    ![aas-lesson5-newcolumn](../tutorials/media/aas-lesson5-newcolumn.png) 
  
<span data-ttu-id="44331-130">hello MonthCalendar beräknade kolumnen innehåller en sorterbar för månad.</span><span class="sxs-lookup"><span data-stu-id="44331-130">hello MonthCalendar calculated column provides a sortable name for Month.</span></span>  
  
#### <a name="create-a-dayofweek-calculated-column-in-hello-dimdate-table"></a><span data-ttu-id="44331-131">Skapa en beräknad kolumn DayOfWeek i hello DimDate tabell</span><span class="sxs-lookup"><span data-stu-id="44331-131">Create a DayOfWeek calculated column in hello DimDate table</span></span>  
  
1.  <span data-ttu-id="44331-132">Med hello **DimDate** tabell fortfarande aktiv klickar du på hello **kolumnen** -menyn och klicka sedan på **Add Column**.</span><span class="sxs-lookup"><span data-stu-id="44331-132">With hello **DimDate** table still active, click hello **Column** menu, and then click **Add Column**.</span></span>  
  
2.  <span data-ttu-id="44331-133">Skriv i formelfältet hello hello följande formel:</span><span class="sxs-lookup"><span data-stu-id="44331-133">In hello formula bar, type hello following formula:</span></span>  
    
    ```
    =RIGHT(" " & FORMAT([DayNumberOfWeek],"#0"), 2) & " - " & [EnglishDayNameOfWeek]  
    ```
    
    <span data-ttu-id="44331-134">Tryck på RETUR när du har skapat hello formel.</span><span class="sxs-lookup"><span data-stu-id="44331-134">When you've finished building hello formula, press ENTER.</span></span> <span data-ttu-id="44331-135">hello nya kolumn läggs toohello längst till höger i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="44331-135">hello new column is added toohello far right of hello table.</span></span>  
  
3.  <span data-ttu-id="44331-136">Byt namn på hello kolumnen för**DayOfWeek**.</span><span class="sxs-lookup"><span data-stu-id="44331-136">Rename hello column too**DayOfWeek**.</span></span>  
  
4.  <span data-ttu-id="44331-137">Klicka på kolumnrubriken hello och dra sedan hello kolumnen mellan hello **EnglishDayNameOfWeek** kolumn och hello **DayNumberOfMonth** kolumn.</span><span class="sxs-lookup"><span data-stu-id="44331-137">Click hello column heading, and then drag hello column between hello **EnglishDayNameOfWeek** column and hello **DayNumberOfMonth** column.</span></span>  
  
    > [!TIP]  
    > <span data-ttu-id="44331-138">Flytta kolumner i tabellen gör det enklare toonavigate.</span><span class="sxs-lookup"><span data-stu-id="44331-138">Moving columns in your table makes it easier toonavigate.</span></span>  
  
<span data-ttu-id="44331-139">hello DayOfWeek beräknade kolumnen innehåller ett sorterbar namn för hello dagen i veckan.</span><span class="sxs-lookup"><span data-stu-id="44331-139">hello DayOfWeek calculated column provides a sortable name for hello day of week.</span></span>  
  
#### <a name="create-a-productsubcategoryname-calculated-column-in-hello-dimproduct-table"></a><span data-ttu-id="44331-140">Skapa en beräknad kolumn ProductSubcategoryName i hello DimProduct tabell</span><span class="sxs-lookup"><span data-stu-id="44331-140">Create a ProductSubcategoryName calculated column in hello DimProduct table</span></span>  
  
  
1.  <span data-ttu-id="44331-141">I hello **DimProduct** tabell, rulla toohello långt till höger i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="44331-141">In hello **DimProduct** table, scroll toohello far right of hello table.</span></span> <span data-ttu-id="44331-142">Meddelande hello längst till höger kolumn heter **Add Column** (kursiv), klicka på hello kolumnrubriken.</span><span class="sxs-lookup"><span data-stu-id="44331-142">Notice hello right-most column is named **Add Column** (italicized), click hello column heading.</span></span>  
  
2.  <span data-ttu-id="44331-143">Skriv i formelfältet hello hello följande formel:</span><span class="sxs-lookup"><span data-stu-id="44331-143">In hello formula bar, type hello following formula:</span></span>  
    
    ```
    =RELATED('DimProductSubcategory'[EnglishProductSubcategoryName])  
    ```
  
3.  <span data-ttu-id="44331-144">Byt namn på hello kolumnen för**ProductSubcategoryName**.</span><span class="sxs-lookup"><span data-stu-id="44331-144">Rename hello column too**ProductSubcategoryName**.</span></span>  
  
<span data-ttu-id="44331-145">Hej ProductSubcategoryName beräknad kolumn är används toocreate en hierarki i hello DimProduct tabellen som innehåller data från hello EnglishProductSubcategoryName kolumnen i hello DimProductSubcategory tabell.</span><span class="sxs-lookup"><span data-stu-id="44331-145">hello ProductSubcategoryName calculated column is used toocreate a hierarchy in hello DimProduct table, which includes data from hello EnglishProductSubcategoryName column in hello DimProductSubcategory table.</span></span> <span data-ttu-id="44331-146">Hierarkier kan inte finnas i mer än en tabell.</span><span class="sxs-lookup"><span data-stu-id="44331-146">Hierarchies cannot span more than one table.</span></span> <span data-ttu-id="44331-147">Du kan skapa hierarkier senare i lektion 9.</span><span class="sxs-lookup"><span data-stu-id="44331-147">You create hierarchies later in Lesson 9.</span></span>  
  
#### <a name="create-a-productcategoryname-calculated-column-in-hello-dimproduct-table"></a><span data-ttu-id="44331-148">Skapa en beräknad kolumn ProductCategoryName i hello DimProduct tabell</span><span class="sxs-lookup"><span data-stu-id="44331-148">Create a ProductCategoryName calculated column in hello DimProduct table</span></span>  
  
1.  <span data-ttu-id="44331-149">Med hello **DimProduct** tabell fortfarande aktiv klickar du på hello **kolumnen** -menyn och klicka sedan på **Add Column**.</span><span class="sxs-lookup"><span data-stu-id="44331-149">With hello **DimProduct** table still active, click hello **Column** menu, and then click **Add Column**.</span></span>  
  
2.  <span data-ttu-id="44331-150">Skriv i formelfältet hello hello följande formel:</span><span class="sxs-lookup"><span data-stu-id="44331-150">In hello formula bar, type hello following formula:</span></span>  
  
    ```
    =RELATED('DimProductCategory'[EnglishProductCategoryName]) 
    ```
    
3.  <span data-ttu-id="44331-151">Byt namn på hello kolumnen för**ProductCategoryName**.</span><span class="sxs-lookup"><span data-stu-id="44331-151">Rename hello column too**ProductCategoryName**.</span></span>  
  
<span data-ttu-id="44331-152">Hej ProductCategoryName beräknad kolumn är används toocreate en hierarki i hello DimProduct tabellen som innehåller data från hello EnglishProductCategoryName kolumnen i hello DimProductCategory tabell.</span><span class="sxs-lookup"><span data-stu-id="44331-152">hello ProductCategoryName calculated column is used toocreate a hierarchy in hello DimProduct table, which includes data from hello EnglishProductCategoryName column in hello DimProductCategory table.</span></span> <span data-ttu-id="44331-153">Hierarkier kan inte finnas i mer än en tabell.</span><span class="sxs-lookup"><span data-stu-id="44331-153">Hierarchies cannot span more than one table.</span></span>  
  
#### <a name="create-a-margin-calculated-column-in-hello-factinternetsales-table"></a><span data-ttu-id="44331-154">Skapa en beräknad kolumn marginal i hello FactInternetSales tabell</span><span class="sxs-lookup"><span data-stu-id="44331-154">Create a Margin calculated column in hello FactInternetSales table</span></span>  
  
1.  <span data-ttu-id="44331-155">Välj hello i hello modellen designer **FactInternetSales** tabell.</span><span class="sxs-lookup"><span data-stu-id="44331-155">In hello model designer, select hello **FactInternetSales** table.</span></span>  
  
2.  <span data-ttu-id="44331-156">Skapa en ny beräknad kolumn mellan hello **SalesAmount** kolumn och hello **TaxAmt** kolumn.</span><span class="sxs-lookup"><span data-stu-id="44331-156">Create a new calculated column between hello **SalesAmount** column and hello **TaxAmt** column.</span></span>  
  
3.  <span data-ttu-id="44331-157">Skriv i formelfältet hello hello följande formel:</span><span class="sxs-lookup"><span data-stu-id="44331-157">In hello formula bar, type hello following formula:</span></span>  
  
    ```
    =[SalesAmount]-[TotalProductCost]
    ``` 

4.  <span data-ttu-id="44331-158">Byt namn på hello kolumnen för**marginal**.</span><span class="sxs-lookup"><span data-stu-id="44331-158">Rename hello column too**Margin**.</span></span>  
 
      ![aas-lesson5-newmargin](../tutorials/media/aas-lesson5-newmargin.png)
      
    <span data-ttu-id="44331-160">hello marginal beräknad kolumn är används tooanalyze vinstmarginaler för varje försäljning.</span><span class="sxs-lookup"><span data-stu-id="44331-160">hello Margin calculated column is used tooanalyze profit margins for each sale.</span></span>  
  
## <a name="whats-next"></a><span data-ttu-id="44331-161">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="44331-161">What's next?</span></span>
<span data-ttu-id="44331-162">[Lektion 6: Skapa mått](../tutorials/aas-lesson-6-create-measures.md).</span><span class="sxs-lookup"><span data-stu-id="44331-162">[Lesson 6: Create measures](../tutorials/aas-lesson-6-create-measures.md).</span></span>
  
  
  
