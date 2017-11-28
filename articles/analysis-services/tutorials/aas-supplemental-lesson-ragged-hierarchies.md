---
<span data-ttu-id="bcbc8-101">Rubrik: aaa ”Azure Analysis Services självstudiekursen kompletterande lektionen: ojämn hierarkier | Microsoft Docs ”beskrivning: Beskriver hur toofix ojämn hierarkier i hello Azure Analysis Services-kursen.</span><span class="sxs-lookup"><span data-stu-id="bcbc8-101">title: aaa"Azure Analysis Services tutorial supplemental lesson: Ragged hierarchies | Microsoft Docs" description: Describes how toofix ragged hierarchies in hello Azure Analysis Services tutorial.</span></span>
<span data-ttu-id="bcbc8-102">tjänster: analysis services dokumentationcenter: '' författare: minewiskan manager: erikre editor: '' taggar: ''</span><span class="sxs-lookup"><span data-stu-id="bcbc8-102">services: analysis-services documentationcenter: '' author: minewiskan manager: erikre editor: '' tags: ''</span></span>

<span data-ttu-id="bcbc8-103">MS.AssetID: ms.service: analysis services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 2017-05/26 ms.author: owend</span><span class="sxs-lookup"><span data-stu-id="bcbc8-103">ms.assetid: ms.service: analysis-services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 05/26/2017 ms.author: owend</span></span>
---
# <a name="supplemental-lesson---ragged-hierarchies"></a><span data-ttu-id="bcbc8-104">Kompletterande lektion – Ojämna hierarkier</span><span class="sxs-lookup"><span data-stu-id="bcbc8-104">Supplemental lesson - Ragged hierarchies</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="bcbc8-105">I den här kompletterande lektionen löser du ett vanligt problem vid pivotering i hierarkier som innehåller tomma värden (medlemmar) på olika nivåer.</span><span class="sxs-lookup"><span data-stu-id="bcbc8-105">In this supplemental lesson, you resolve a common problem when pivoting on hierarchies that contain blank values (members) at different levels.</span></span> <span data-ttu-id="bcbc8-106">Det kan till exempel förekomma i en organisation där en chef på hög nivå har både avdelningschefer och personer som inte är chefer som direkt underställda.</span><span class="sxs-lookup"><span data-stu-id="bcbc8-106">For example, an organization where a high-level manager has both departmental managers and non-managers as direct reports.</span></span> <span data-ttu-id="bcbc8-107">Eller i geografiska hierarkier som består av land-region-ort, där vissa orter saknar en överordnad delstat eller provins, till exempel Washington D.C. och Vatikanstaten.</span><span class="sxs-lookup"><span data-stu-id="bcbc8-107">Or, geographic hierarchies composed of Country-Region-City, where some cities lack a parent State or Province, such as Washington D.C., Vatican City.</span></span> <span data-ttu-id="bcbc8-108">När en hierarki har tomt medlemmar, det ofta är underordnad toodifferent eller ojämn nivåer.</span><span class="sxs-lookup"><span data-stu-id="bcbc8-108">When a hierarchy has blank members, it often descends toodifferent, or ragged, levels.</span></span>

![aas-lesson-detail-ragged-hierarchies-table](../tutorials/media/aas-lesson-detail-ragged-hierarchies-table.png)

<span data-ttu-id="bcbc8-110">Tabellmodeller på hello 1400 kompatibilitetsnivå har ytterligare **dölja medlemmar** -egenskapen för hierarkier.</span><span class="sxs-lookup"><span data-stu-id="bcbc8-110">Tabular models at hello 1400 compatibility level have an additional **Hide Members** property for hierarchies.</span></span> <span data-ttu-id="bcbc8-111">Hej **standard** inställningen förutsätter att det finns inga tomma medlemmar på alla nivåer.</span><span class="sxs-lookup"><span data-stu-id="bcbc8-111">hello **Default** setting assumes there are no blank members at any level.</span></span> <span data-ttu-id="bcbc8-112">Hej **dölja tomma medlemmar** undantar tomma medlemmar från hello hierarkin när de läggs tooa pivottabell eller rapporten.</span><span class="sxs-lookup"><span data-stu-id="bcbc8-112">hello **Hide blank members** setting excludes blank members from hello hierarchy when added tooa PivotTable or report.</span></span>  
  
<span data-ttu-id="bcbc8-113">Uppskattad tid toocomplete lektionen: **20 minuter**</span><span class="sxs-lookup"><span data-stu-id="bcbc8-113">Estimated time toocomplete this lesson: **20 minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="bcbc8-114">Krav</span><span class="sxs-lookup"><span data-stu-id="bcbc8-114">Prerequisites</span></span>  
<span data-ttu-id="bcbc8-115">Den här kompletterande lektionen ingår i en självstudiekurs om tabellmodeller.</span><span class="sxs-lookup"><span data-stu-id="bcbc8-115">This supplemental lesson topic is part of a tabular modeling tutorial.</span></span> <span data-ttu-id="bcbc8-116">Innan du utför hello uppgifter i den här kompletterande lektionen bör du slutfört alla tidigare erfarenheter eller har en slutförd Adventure Works Internet försäljning modellen exempelprojektet.</span><span class="sxs-lookup"><span data-stu-id="bcbc8-116">Before performing hello tasks in this supplemental lesson, you should have completed all previous lessons or have a completed Adventure Works Internet Sales sample model project.</span></span> 

<span data-ttu-id="bcbc8-117">Om du har skapat hello AW Internet försäljnings-projekt som en del av hello kursen, innehåller din modell ännu inte några data eller ojämna hierarkier.</span><span class="sxs-lookup"><span data-stu-id="bcbc8-117">If you've created hello AW Internet Sales project as part of hello tutorial, your model does not yet contain any data or hierarchies that are ragged.</span></span> <span data-ttu-id="bcbc8-118">toocomplete kursen kompletterande du först ha toocreate Hej problemet genom att lägga till vissa ytterligare tabeller, skapa relationer, beräknade kolumner, ett mått och en ny organisationshierarki.</span><span class="sxs-lookup"><span data-stu-id="bcbc8-118">toocomplete this supplemental lesson, you first have toocreate hello problem by adding some additional tables, create relationships, calculated columns, a measure, and a new Organization hierarchy.</span></span> <span data-ttu-id="bcbc8-119">Det tar cirka 15 minuter.</span><span class="sxs-lookup"><span data-stu-id="bcbc8-119">That part takes about 15 minutes.</span></span> <span data-ttu-id="bcbc8-120">Sedan kan du hämta toosolve i bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="bcbc8-120">Then, you get toosolve it in just a few minutes.</span></span>  

## <a name="add-tables-and-objects"></a><span data-ttu-id="bcbc8-121">Lägga till tabeller och objekt</span><span class="sxs-lookup"><span data-stu-id="bcbc8-121">Add tables and objects</span></span>
  
### <a name="tooadd-new-tables-tooyour-model"></a><span data-ttu-id="bcbc8-122">tooadd nya tabeller tooyour modellen</span><span class="sxs-lookup"><span data-stu-id="bcbc8-122">tooadd new tables tooyour model</span></span>
  
1.  <span data-ttu-id="bcbc8-123">I tabellmodellutforskaren expanderar du **Datakällor**. Sedan högerklickar du på din anslutning och väljer **Importera nya tabeller**.</span><span class="sxs-lookup"><span data-stu-id="bcbc8-123">In Tabular Model Explorer, expand **Data Sources**, then right-click your connection > **Import New Tables**.</span></span>
  
2.  <span data-ttu-id="bcbc8-124">I navigatören väljer du **DimEmployee** och **FactResellerSales** och klickar sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="bcbc8-124">In Navigator, select **DimEmployee** and **FactResellerSales**, and then click **OK**.</span></span>

3.  <span data-ttu-id="bcbc8-125">Klicka på **Importera** i frågeredigeraren</span><span class="sxs-lookup"><span data-stu-id="bcbc8-125">In Query Editor, click **Import**</span></span>

4.  <span data-ttu-id="bcbc8-126">Skapa följande hello [relationer](../tutorials/aas-lesson-4-create-relationships.md):</span><span class="sxs-lookup"><span data-stu-id="bcbc8-126">Create hello following [relationships](../tutorials/aas-lesson-4-create-relationships.md):</span></span>

    | <span data-ttu-id="bcbc8-127">Tabell 1</span><span class="sxs-lookup"><span data-stu-id="bcbc8-127">Table 1</span></span>           | <span data-ttu-id="bcbc8-128">Kolumn</span><span class="sxs-lookup"><span data-stu-id="bcbc8-128">Column</span></span>       | <span data-ttu-id="bcbc8-129">Filterriktning</span><span class="sxs-lookup"><span data-stu-id="bcbc8-129">Filter Direction</span></span>   | <span data-ttu-id="bcbc8-130">Tabell 2</span><span class="sxs-lookup"><span data-stu-id="bcbc8-130">Table 2</span></span>     | <span data-ttu-id="bcbc8-131">Kolumn</span><span class="sxs-lookup"><span data-stu-id="bcbc8-131">Column</span></span>      | <span data-ttu-id="bcbc8-132">Active</span><span class="sxs-lookup"><span data-stu-id="bcbc8-132">Active</span></span> |
    |-------------------|--------------|--------------------|-------------|-------------|--------|
    | <span data-ttu-id="bcbc8-133">FactResellerSales</span><span class="sxs-lookup"><span data-stu-id="bcbc8-133">FactResellerSales</span></span> | <span data-ttu-id="bcbc8-134">OrderDateKey</span><span class="sxs-lookup"><span data-stu-id="bcbc8-134">OrderDateKey</span></span> | <span data-ttu-id="bcbc8-135">Standard</span><span class="sxs-lookup"><span data-stu-id="bcbc8-135">Default</span></span>            | <span data-ttu-id="bcbc8-136">DimDate</span><span class="sxs-lookup"><span data-stu-id="bcbc8-136">DimDate</span></span>     | <span data-ttu-id="bcbc8-137">Date</span><span class="sxs-lookup"><span data-stu-id="bcbc8-137">Date</span></span>        | <span data-ttu-id="bcbc8-138">Ja</span><span class="sxs-lookup"><span data-stu-id="bcbc8-138">Yes</span></span>    |
    | <span data-ttu-id="bcbc8-139">FactResellerSales</span><span class="sxs-lookup"><span data-stu-id="bcbc8-139">FactResellerSales</span></span> | <span data-ttu-id="bcbc8-140">DueDate</span><span class="sxs-lookup"><span data-stu-id="bcbc8-140">DueDate</span></span>      | <span data-ttu-id="bcbc8-141">Standard</span><span class="sxs-lookup"><span data-stu-id="bcbc8-141">Default</span></span>            | <span data-ttu-id="bcbc8-142">DimDate</span><span class="sxs-lookup"><span data-stu-id="bcbc8-142">DimDate</span></span>     | <span data-ttu-id="bcbc8-143">Date</span><span class="sxs-lookup"><span data-stu-id="bcbc8-143">Date</span></span>        | <span data-ttu-id="bcbc8-144">Nej</span><span class="sxs-lookup"><span data-stu-id="bcbc8-144">No</span></span>     |
    | <span data-ttu-id="bcbc8-145">FactResellerSales</span><span class="sxs-lookup"><span data-stu-id="bcbc8-145">FactResellerSales</span></span> | <span data-ttu-id="bcbc8-146">ShipDateKey</span><span class="sxs-lookup"><span data-stu-id="bcbc8-146">ShipDateKey</span></span>  | <span data-ttu-id="bcbc8-147">Standard</span><span class="sxs-lookup"><span data-stu-id="bcbc8-147">Default</span></span>            | <span data-ttu-id="bcbc8-148">DimDate</span><span class="sxs-lookup"><span data-stu-id="bcbc8-148">DimDate</span></span>     | <span data-ttu-id="bcbc8-149">Date</span><span class="sxs-lookup"><span data-stu-id="bcbc8-149">Date</span></span>        | <span data-ttu-id="bcbc8-150">Nej</span><span class="sxs-lookup"><span data-stu-id="bcbc8-150">No</span></span>     |
    | <span data-ttu-id="bcbc8-151">FactResellerSales</span><span class="sxs-lookup"><span data-stu-id="bcbc8-151">FactResellerSales</span></span> | <span data-ttu-id="bcbc8-152">ProductKey</span><span class="sxs-lookup"><span data-stu-id="bcbc8-152">ProductKey</span></span>   | <span data-ttu-id="bcbc8-153">Standard</span><span class="sxs-lookup"><span data-stu-id="bcbc8-153">Default</span></span>            | <span data-ttu-id="bcbc8-154">DimProduct</span><span class="sxs-lookup"><span data-stu-id="bcbc8-154">DimProduct</span></span>  | <span data-ttu-id="bcbc8-155">ProductKey</span><span class="sxs-lookup"><span data-stu-id="bcbc8-155">ProductKey</span></span>  | <span data-ttu-id="bcbc8-156">Ja</span><span class="sxs-lookup"><span data-stu-id="bcbc8-156">Yes</span></span>    |
    | <span data-ttu-id="bcbc8-157">FactResellerSales</span><span class="sxs-lookup"><span data-stu-id="bcbc8-157">FactResellerSales</span></span> | <span data-ttu-id="bcbc8-158">EmployeeKey</span><span class="sxs-lookup"><span data-stu-id="bcbc8-158">EmployeeKey</span></span>  | <span data-ttu-id="bcbc8-159">tooBoth tabeller</span><span class="sxs-lookup"><span data-stu-id="bcbc8-159">tooBoth Tables</span></span> | <span data-ttu-id="bcbc8-160">DimEmployee</span><span class="sxs-lookup"><span data-stu-id="bcbc8-160">DimEmployee</span></span> | <span data-ttu-id="bcbc8-161">EmployeeKey</span><span class="sxs-lookup"><span data-stu-id="bcbc8-161">EmployeeKey</span></span> | <span data-ttu-id="bcbc8-162">Ja</span><span class="sxs-lookup"><span data-stu-id="bcbc8-162">Yes</span></span>    |

5. <span data-ttu-id="bcbc8-163">I hello **DimEmployee** tabell, skapar hello följande [beräknade kolumner](../tutorials/aas-lesson-5-create-calculated-columns.md):</span><span class="sxs-lookup"><span data-stu-id="bcbc8-163">In hello **DimEmployee** table, create hello following [calculated columns](../tutorials/aas-lesson-5-create-calculated-columns.md):</span></span> 

    <span data-ttu-id="bcbc8-164">**Sökväg**</span><span class="sxs-lookup"><span data-stu-id="bcbc8-164">**Path**</span></span> 
    ```
    =PATH([EmployeeKey],[ParentEmployeeKey])
    ```

    <span data-ttu-id="bcbc8-165">**FullName**</span><span class="sxs-lookup"><span data-stu-id="bcbc8-165">**FullName**</span></span> 
    ```
    =[FirstName] & " " & [MiddleName] & " " & [LastName]
    ```

    <span data-ttu-id="bcbc8-166">**Level1**</span><span class="sxs-lookup"><span data-stu-id="bcbc8-166">**Level1**</span></span> 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],1,1)) 
    ```

    <span data-ttu-id="bcbc8-167">**Level2**</span><span class="sxs-lookup"><span data-stu-id="bcbc8-167">**Level2**</span></span> 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],1,2)) 
    ```

    <span data-ttu-id="bcbc8-168">**Level3**</span><span class="sxs-lookup"><span data-stu-id="bcbc8-168">**Level3**</span></span> 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],1,3)) 
    ```

    <span data-ttu-id="bcbc8-169">**Level4**</span><span class="sxs-lookup"><span data-stu-id="bcbc8-169">**Level4**</span></span> 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],1,4)) 
    ```

    <span data-ttu-id="bcbc8-170">**Level5**</span><span class="sxs-lookup"><span data-stu-id="bcbc8-170">**Level5**</span></span> 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],1,5)) 
    ```

6.  <span data-ttu-id="bcbc8-171">I hello **DimEmployee** tabell, skapa en [hierarkin](../tutorials/aas-lesson-9-create-hierarchies.md) med namnet **organisation**.</span><span class="sxs-lookup"><span data-stu-id="bcbc8-171">In hello **DimEmployee** table, create a [hierarchy](../tutorials/aas-lesson-9-create-hierarchies.md) named **Organization**.</span></span> <span data-ttu-id="bcbc8-172">Lägg till följande kolumner i ordning hello: **Level1**, **nivå 2**, **Level3**, **Level4**, **Level5**.</span><span class="sxs-lookup"><span data-stu-id="bcbc8-172">Add hello following columns in-order: **Level1**, **Level2**, **Level3**, **Level4**, **Level5**.</span></span>

7.  <span data-ttu-id="bcbc8-173">I hello **FactResellerSales** tabell, skapar hello följande [mått](../tutorials/aas-lesson-6-create-measures.md):</span><span class="sxs-lookup"><span data-stu-id="bcbc8-173">In hello **FactResellerSales** table, create hello following [measure](../tutorials/aas-lesson-6-create-measures.md):</span></span>

    ```
    ResellerTotalSales:=SUM([SalesAmount])
    ```

8.  <span data-ttu-id="bcbc8-174">Använd [analysera i Excel](../tutorials/aas-lesson-12-analyze-in-excel.md) tooopen Excel och automatiskt skapa en pivottabell.</span><span class="sxs-lookup"><span data-stu-id="bcbc8-174">Use [Analyze in Excel](../tutorials/aas-lesson-12-analyze-in-excel.md) tooopen Excel and automatically create a PivotTable.</span></span>

9.  <span data-ttu-id="bcbc8-175">I **PivotTable-Fields**, lägga till hello **organisation** hierarkin från hello **DimEmployee** tabell för**rader**, och hello **ResellerTotalSales** måttet från hello **FactResellerSales** tabell för**värden**.</span><span class="sxs-lookup"><span data-stu-id="bcbc8-175">In **PivotTable Fields**, add hello **Organization** hierarchy from hello **DimEmployee** table too**Rows**, and hello **ResellerTotalSales** measure from hello **FactResellerSales**  table too**Values**.</span></span>

    ![aas-lesson-detail-ragged-hierarchies-pivottable](../tutorials/media/aas-lesson-detail-ragged-hierarchies-pivottable.png)

    <span data-ttu-id="bcbc8-177">Som du ser i hello pivottabell visar rader som är ojämna hello-hierarkin.</span><span class="sxs-lookup"><span data-stu-id="bcbc8-177">As you can see in hello PivotTable, hello hierarchy displays rows that are ragged.</span></span> <span data-ttu-id="bcbc8-178">Det finns flera rader med tomma medlemmar.</span><span class="sxs-lookup"><span data-stu-id="bcbc8-178">There are many rows where blank members are shown.</span></span>

## <a name="toofix-hello-ragged-hierarchy-by-setting-hello-hide-members-property"></a><span data-ttu-id="bcbc8-179">toofix hello ojämn hierarki med egenskapen hello Dölj medlemmar</span><span class="sxs-lookup"><span data-stu-id="bcbc8-179">toofix hello ragged hierarchy by setting hello Hide members property</span></span>

1.  <span data-ttu-id="bcbc8-180">I **tabellmodellutforskaren** expanderar du **Tabeller** > **DimEmployee** > **Hierarkier** > **Organisation**.</span><span class="sxs-lookup"><span data-stu-id="bcbc8-180">In **Tabular Model Explorer**, expand **Tables** > **DimEmployee** > **Hierarchies** > **Organization**.</span></span>

2.  <span data-ttu-id="bcbc8-181">I **Egenskaper** > **Dölj medlemmar** väljer du **Dölj tomma medlemmar**.</span><span class="sxs-lookup"><span data-stu-id="bcbc8-181">In **Properties** > **Hide Members**, select **Hide blank members**.</span></span> 

    ![aas-lesson-detail-ragged-hierarchies-hidemembers](../tutorials/media/aas-lesson-detail-ragged-hierarchies-hidemembers.png)

3.  <span data-ttu-id="bcbc8-183">Uppdatera hello pivottabell i Excel.</span><span class="sxs-lookup"><span data-stu-id="bcbc8-183">Back in Excel, refresh hello PivotTable.</span></span> 

    ![aas-lesson-detail-ragged-hierarchies-pivottable-refresh](../tutorials/media/aas-lesson-detail-ragged-hierarchies-pivottable-refresh.png)

    <span data-ttu-id="bcbc8-185">Nu ser det betydligt bättre ut!</span><span class="sxs-lookup"><span data-stu-id="bcbc8-185">Now that looks a whole lot better!</span></span>

## <a name="see-also"></a><span data-ttu-id="bcbc8-186">Se även</span><span class="sxs-lookup"><span data-stu-id="bcbc8-186">See Also</span></span>   
[<span data-ttu-id="bcbc8-187">Lektion 9: Skapa hierarkier</span><span class="sxs-lookup"><span data-stu-id="bcbc8-187">Lesson 9: Create hierarchies</span></span>](../tutorials/aas-lesson-9-create-hierarchies.md)  
[<span data-ttu-id="bcbc8-188">Kompletterande lektion – Dynamisk säkerhet</span><span class="sxs-lookup"><span data-stu-id="bcbc8-188">Supplemental Lesson - Dynamic security</span></span>](../tutorials/aas-supplemental-lesson-dynamic-security.md)  
[<span data-ttu-id="bcbc8-189">Kompletterande lektion – Detaljrader</span><span class="sxs-lookup"><span data-stu-id="bcbc8-189">Supplemental Lesson - Detail rows</span></span>](../tutorials/aas-supplemental-lesson-detail-rows.md)  