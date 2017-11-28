---
<span data-ttu-id="5b8f8-101">Rubrik: aaa ”Azure Analysis Services självstudiekursen lektionen 10: skapa partitioner | Microsoft Docs ”beskrivning: Beskriver hur toocreate partitioner i självstudiekursen hello Azure Analysis Services-projekt.</span><span class="sxs-lookup"><span data-stu-id="5b8f8-101">title: aaa"Azure Analysis Services tutorial lesson 10: Create partitions | Microsoft Docs" description: Describes how toocreate partitions in hello Azure Analysis Services tutorial project.</span></span> <span data-ttu-id="5b8f8-102">tjänster: analysis services dokumentationcenter: '' författare: minewiskan manager: erikre editor: '' taggar: ''</span><span class="sxs-lookup"><span data-stu-id="5b8f8-102">services: analysis-services documentationcenter: '' author: minewiskan manager: erikre editor: '' tags: ''</span></span>

<span data-ttu-id="5b8f8-103">MS.AssetID: ms.service: analysis services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 2017-05/26 ms.author: owend</span><span class="sxs-lookup"><span data-stu-id="5b8f8-103">ms.assetid: ms.service: analysis-services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 05/26/2017 ms.author: owend</span></span>
---
# <a name="lesson-10-create-partitions"></a><span data-ttu-id="5b8f8-104">Lektion 10: Skapa partitioner</span><span class="sxs-lookup"><span data-stu-id="5b8f8-104">Lesson 10: Create partitions</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="5b8f8-105">Nu bör skapa du partitioner toodivide hello FactInternetSales tabellen i mindre logiska delar som kan vara bearbetade (uppdateras) oberoende av andra partitioner.</span><span class="sxs-lookup"><span data-stu-id="5b8f8-105">In this lesson, you create partitions toodivide hello FactInternetSales table into smaller logical parts that can be processed (refreshed) independent of other partitions.</span></span> <span data-ttu-id="5b8f8-106">Som standard har alla tabeller som du inkludera i din modell en partition som innehåller alla hello tabellens kolumner och rader.</span><span class="sxs-lookup"><span data-stu-id="5b8f8-106">By default, every table you include in your model has one partition, which includes all hello table’s columns and rows.</span></span> <span data-ttu-id="5b8f8-107">Tabellen FactInternetSales hello vill vi toodivide hello data per år; en partition för varje hello tabell fem år.</span><span class="sxs-lookup"><span data-stu-id="5b8f8-107">For hello FactInternetSales table, we want toodivide hello data by year; one partition for each of hello table’s five years.</span></span> <span data-ttu-id="5b8f8-108">Varje partition kan sedan bearbetas separat.</span><span class="sxs-lookup"><span data-stu-id="5b8f8-108">Each partition can then be processed independently.</span></span> <span data-ttu-id="5b8f8-109">Det finns fler toolearn [partitioner](https://docs.microsoft.com/sql/analysis-services/tabular-models/partitions-ssas-tabular).</span><span class="sxs-lookup"><span data-stu-id="5b8f8-109">toolearn more, see [Partitions](https://docs.microsoft.com/sql/analysis-services/tabular-models/partitions-ssas-tabular).</span></span> 
  
<span data-ttu-id="5b8f8-110">Uppskattad tid toocomplete lektionen: **15 minuter**</span><span class="sxs-lookup"><span data-stu-id="5b8f8-110">Estimated time toocomplete this lesson: **15 minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="5b8f8-111">Krav</span><span class="sxs-lookup"><span data-stu-id="5b8f8-111">Prerequisites</span></span>  
<span data-ttu-id="5b8f8-112">Det här avsnittet ingår i självstudiekursen för tabellmodellering som bör slutföras i rätt ordning.</span><span class="sxs-lookup"><span data-stu-id="5b8f8-112">This topic is part of a tabular modeling tutorial, which should be completed in order.</span></span> <span data-ttu-id="5b8f8-113">Innan du utför hello uppgifter i den här lektionen bör du slutfört hello föregående lektionen: [lektionen 9: skapa hierarkier](../tutorials/aas-lesson-9-create-hierarchies.md).</span><span class="sxs-lookup"><span data-stu-id="5b8f8-113">Before performing hello tasks in this lesson, you should have completed hello previous lesson: [Lesson 9: Create Hierarchies](../tutorials/aas-lesson-9-create-hierarchies.md).</span></span>  
  
## <a name="create-partitions"></a><span data-ttu-id="5b8f8-114">Skapa partitioner</span><span class="sxs-lookup"><span data-stu-id="5b8f8-114">Create partitions</span></span>  
  
#### <a name="toocreate-partitions-in-hello-factinternetsales-table"></a><span data-ttu-id="5b8f8-115">toocreate partitioner i hello FactInternetSales tabell</span><span class="sxs-lookup"><span data-stu-id="5b8f8-115">toocreate partitions in hello FactInternetSales table</span></span>  
  
1.  <span data-ttu-id="5b8f8-116">Expandera **Tabeller** i tabellmodellutforskaren och högerklicka på **FactInternetSales** > **Partitioner**.</span><span class="sxs-lookup"><span data-stu-id="5b8f8-116">In Tabular Model Explorer, expand **Tables**, and then right-click **FactInternetSales** > **Partitions**.</span></span>  
  
2.  <span data-ttu-id="5b8f8-117">I Partitionshanteraren klickar du på **kopiera**, och sedan ändrar hello namn för**FactInternetSales2010**.</span><span class="sxs-lookup"><span data-stu-id="5b8f8-117">In Partition Manager, click **Copy**, and then change hello name too**FactInternetSales2010**.</span></span>
  
    <span data-ttu-id="5b8f8-118">Eftersom du vill hello partition tooinclude bara de raderna i en viss tid för hello år 2010, måste du ändra hello frågeuttryck.</span><span class="sxs-lookup"><span data-stu-id="5b8f8-118">Because you want hello partition tooinclude only those rows within a certain period, for hello year 2010, you must modify hello query expression.</span></span>
  
4.  <span data-ttu-id="5b8f8-119">Klicka på **Design** tooopen fråga redigeraren och klicka sedan på hello **FactInternetSales2010** frågan.</span><span class="sxs-lookup"><span data-stu-id="5b8f8-119">Click **Design** tooopen Query Editor, and then click hello **FactInternetSales2010** query.</span></span>

5.  <span data-ttu-id="5b8f8-120">I Förhandsgranska klickar du på hello NEDPIL i hello **OrderDate** kolumnrubriken och klicka sedan på **tidsvärdet filter** > **mellan**.</span><span class="sxs-lookup"><span data-stu-id="5b8f8-120">In preview, click hello down arrow in hello **OrderDate** column heading, and then click **Date/Time Filters** > **Between**.</span></span>

    ![aas-lesson10-query-editor](../tutorials/media/aas-lesson10-query-editor.png)

6.  <span data-ttu-id="5b8f8-122">Hello Filtrera rader i dialogrutan i **visa rader där: OrderDate**, lämna **infaller efter eller är lika med**, och ange sedan i hello datumfält **2010-1-1**.</span><span class="sxs-lookup"><span data-stu-id="5b8f8-122">In hello Filter Rows dialog box, in **Show rows where: OrderDate**, leave **is after or equal to**, and then in hello date field, enter **1/1/2010**.</span></span> <span data-ttu-id="5b8f8-123">Lämna hello **och** operatör som valts, välj sedan **innan**, ange i hello datumfält **2011-1-1**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="5b8f8-123">Leave hello **And** operator selected, then select **is before**, then in hello date field, enter **1/1/2011**, and then click **OK**.</span></span>

    ![aas-lesson10-filter-rows](../tutorials/media/aas-lesson10-filter-rows.png)
    
    <span data-ttu-id="5b8f8-125">Observera att ytterligare ett steg med namnet filtrerade rader finns i frågeredigeraren, i TILLÄMPADE STEG.</span><span class="sxs-lookup"><span data-stu-id="5b8f8-125">Notice in Query Editor, in APPLIED STEPS, you see another step named Filtered Rows.</span></span> <span data-ttu-id="5b8f8-126">Det här filtret är tooselect endast datum från 2010.</span><span class="sxs-lookup"><span data-stu-id="5b8f8-126">This filter is tooselect only order dates from 2010.</span></span>

8.  <span data-ttu-id="5b8f8-127">Klicka på **Importera**.</span><span class="sxs-lookup"><span data-stu-id="5b8f8-127">Click **Import**.</span></span>

    <span data-ttu-id="5b8f8-128">I Partitionshanteraren Observera hello fråga uttryck nu har en ytterligare filtrerade rader-instruktion.</span><span class="sxs-lookup"><span data-stu-id="5b8f8-128">In Partition Manager, notice hello query expression now has an additional Filtered Rows clause.</span></span>

    ![aas-lesson10-query](../tutorials/media/aas-lesson10-query.png)
  
    <span data-ttu-id="5b8f8-130">Den här instruktionen anger den här partitionen bör inkludera endast hello data i de rader där hello OrderDate är i hello 2010 kalenderåret som angetts i hello filtrerade rader satsen.</span><span class="sxs-lookup"><span data-stu-id="5b8f8-130">This statement specifies this partition should include only hello data in those rows where hello OrderDate is in hello 2010 calendar year as specified in hello filtered rows clause.</span></span>  
  
  
#### <a name="toocreate-a-partition-for-hello-2011-year"></a><span data-ttu-id="5b8f8-131">toocreate en partition för hello 2011 år</span><span class="sxs-lookup"><span data-stu-id="5b8f8-131">toocreate a partition for hello 2011 year</span></span>  
  
1.  <span data-ttu-id="5b8f8-132">Klicka på hello i hello partitioner **FactInternetSales2010** partition som du skapade och klicka sedan på **kopiera**.</span><span class="sxs-lookup"><span data-stu-id="5b8f8-132">In hello partitions list, click hello **FactInternetSales2010** partition you created, and then click **Copy**.</span></span>  <span data-ttu-id="5b8f8-133">Ändra hello partitionsnamnet för**FactInternetSales2011**.</span><span class="sxs-lookup"><span data-stu-id="5b8f8-133">Change hello partition name too**FactInternetSales2011**.</span></span> 

    <span data-ttu-id="5b8f8-134">Du behöver inte toouse frågeredigeraren toocreate en ny filtrerade rader-instruktion.</span><span class="sxs-lookup"><span data-stu-id="5b8f8-134">You do not need toouse Query Editor toocreate a new filtered rows clause.</span></span> <span data-ttu-id="5b8f8-135">Eftersom du har skapat en kopia av hello frågan för 2010 är allt du behöver toodo göra små ändringar i hello frågan 2011.</span><span class="sxs-lookup"><span data-stu-id="5b8f8-135">Because you created a copy of hello query for 2010, all you need toodo is make a slight change in hello query for 2011.</span></span>
  
2.  <span data-ttu-id="5b8f8-136">I **frågeuttryck**, i ordning för den här partitionen tooinclude bara de raderna för hello 2011 år, Ersätt hello år i hello filtrerade rader-sats med **2011** och **2012**, t.ex.:</span><span class="sxs-lookup"><span data-stu-id="5b8f8-136">In **Query Expression**, in-order for this partition tooinclude only those rows for hello 2011 year, replace hello years in hello Filtered Rows clause with **2011** and **2012**, respectively, like:</span></span>  
  
    ```  
    let
        Source = #"SQL/localhost;AdventureWorksDW2014",
        dbo_FactInternetSales = Source{[Schema="dbo",Item="FactInternetSales"]}[Data],
        #"Removed Columns" = Table.RemoveColumns(dbo_FactInternetSales,{"OrderDateKey", "DueDateKey", "ShipDateKey"}),
        #"Filtered Rows" = Table.SelectRows(#"Removed Columns", each [OrderDate] >= #datetime(2011, 1, 1, 0, 0, 0) and [OrderDate] < #datetime(2012, 1, 1, 0, 0, 0))
    in
        #"Filtered Rows"
   
    ```  
  
#### <a name="toocreate-partitions-for-2012-2013-and-2014"></a><span data-ttu-id="5b8f8-137">toocreate partitioner för 2012, 2013 och 2014.</span><span class="sxs-lookup"><span data-stu-id="5b8f8-137">toocreate partitions for 2012, 2013, and 2014.</span></span>  
  
- <span data-ttu-id="5b8f8-138">Följ hello föregående steg är att skapa partitioner för 2012, 2013 och 2014, ändra hello år i hello filtrerade rader satsen tooinclude endast rader för det aktuella året.</span><span class="sxs-lookup"><span data-stu-id="5b8f8-138">Follow hello previous steps, creating partitions for 2012, 2013, and 2014, changing hello years in hello Filtered Rows clause tooinclude only rows for that year.</span></span> 
  

## <a name="delete-hello-factinternetsales-partition"></a><span data-ttu-id="5b8f8-139">Ta bort hello FactInternetSales partition</span><span class="sxs-lookup"><span data-stu-id="5b8f8-139">Delete hello FactInternetSales partition</span></span>
<span data-ttu-id="5b8f8-140">Nu när du har partitioner för varje år kan du ta bort hello FactInternetSales partition. förhindra överlappning när du väljer alla processen vid bearbetning av partitioner.</span><span class="sxs-lookup"><span data-stu-id="5b8f8-140">Now that you have partitions for each year, you can delete hello FactInternetSales partition; preventing overlap when choosing Process all when processing partitions.</span></span>

#### <a name="toodelete-hello-factinternetsales-partition"></a><span data-ttu-id="5b8f8-141">toodelete hello FactInternetSales partition</span><span class="sxs-lookup"><span data-stu-id="5b8f8-141">toodelete hello FactInternetSales partition</span></span>
-  <span data-ttu-id="5b8f8-142">Klicka på hello FactInternetSales partition och klicka sedan på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="5b8f8-142">Click hello FactInternetSales partition, and then click **Delete**.</span></span>



## <a name="process-partitions"></a><span data-ttu-id="5b8f8-143">Bearbeta partitioner</span><span class="sxs-lookup"><span data-stu-id="5b8f8-143">Process partitions</span></span>  
<span data-ttu-id="5b8f8-144">I Partitionshanteraren Observera hello **sista bearbetade** kolumn för varje hello nya partitioner som du har skapat visas dessa partitioner inte har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="5b8f8-144">In Partition Manager, notice hello **Last Processed** column for each of hello new partitions you created shows these partitions have never been processed.</span></span> <span data-ttu-id="5b8f8-145">När du skapar partitioner, bör du köra en Process partitioner eller processen igen toorefresh hello tabelldata i dessa partitioner.</span><span class="sxs-lookup"><span data-stu-id="5b8f8-145">When you create partitions, you should run a Process Partitions or Process Table operation toorefresh hello data in those partitions.</span></span>  
  
#### <a name="tooprocess-hello-factinternetsales-partitions"></a><span data-ttu-id="5b8f8-146">tooprocess hello FactInternetSales partitioner</span><span class="sxs-lookup"><span data-stu-id="5b8f8-146">tooprocess hello FactInternetSales partitions</span></span>  
  
1.  <span data-ttu-id="5b8f8-147">Klicka på **OK** tooclose Partitionshanteraren.</span><span class="sxs-lookup"><span data-stu-id="5b8f8-147">Click **OK** tooclose Partition Manager.</span></span>  
  
2.  <span data-ttu-id="5b8f8-148">Klicka på hello **FactInternetSales** tabell och klicka sedan på hello **modellen** menyn > **processen** > **processen partitioner**.</span><span class="sxs-lookup"><span data-stu-id="5b8f8-148">Click hello **FactInternetSales** table, then click hello **Model** menu > **Process** > **Process Partitions**.</span></span>  
  
3.  <span data-ttu-id="5b8f8-149">Hello processen partitioner i dialogrutan Kontrollera **läge** har angetts för**processen standard**.</span><span class="sxs-lookup"><span data-stu-id="5b8f8-149">In hello Process Partitions dialog box, verify **Mode** is set too**Process Default**.</span></span>  
  
4.  <span data-ttu-id="5b8f8-150">Markera kryssrutan för hello i hello **processen** kolumn för varje hello fem partitionerar du skapade och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="5b8f8-150">Select hello checkbox in hello **Process** column for each of hello five partitions you created, and then click **OK**.</span></span>  

    ![aas-lesson10-process-partitions](../tutorials/media/aas-lesson10-process-partitions.png)
  
    <span data-ttu-id="5b8f8-152">Om du uppmanas att ange autentiseringsuppgifter för personifiering ange hello Windows-användarnamn och lösenord som du angav i lektionen 2.</span><span class="sxs-lookup"><span data-stu-id="5b8f8-152">If you're prompted for Impersonation credentials, enter hello Windows user name and password you specified in Lesson 2.</span></span>  
  
    <span data-ttu-id="5b8f8-153">Hej **databearbetning** dialogrutan visas och visar information om processen för varje partition.</span><span class="sxs-lookup"><span data-stu-id="5b8f8-153">hello **Data Processing** dialog box appears and displays process details for each partition.</span></span> <span data-ttu-id="5b8f8-154">Observera att olika antal rader överförs för varje partition.</span><span class="sxs-lookup"><span data-stu-id="5b8f8-154">Notice that a different number of rows for each partition are transferred.</span></span> <span data-ttu-id="5b8f8-155">Varje partition innehåller bara de raderna för hello år som anges i hello WHERE-satsen i hello SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="5b8f8-155">Each partition includes only those rows for hello year specified in hello WHERE clause in hello SQL Statement.</span></span> <span data-ttu-id="5b8f8-156">När bearbetningen är klar kan du gå vidare och stänga dialogrutan för hello databearbetning.</span><span class="sxs-lookup"><span data-stu-id="5b8f8-156">When processing is finished, go ahead and close hello Data Processing dialog box.</span></span>  
  
    ![aas-lesson10-process-complete](../tutorials/media/aas-lesson10-process-complete.png)
  
 ## <a name="whats-next"></a><span data-ttu-id="5b8f8-158">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5b8f8-158">What's next?</span></span>
<span data-ttu-id="5b8f8-159">Gå toohello kursen: [lektionen 11: skapa roller](../tutorials/aas-lesson-11-create-roles.md).</span><span class="sxs-lookup"><span data-stu-id="5b8f8-159">Go toohello next lesson: [Lesson 11: Create Roles](../tutorials/aas-lesson-11-create-roles.md).</span></span> 
