---
<span data-ttu-id="3d037-101">Rubrik: aaa ”Azure Analysis Services självstudiekursen lektionen 9: skapa hierarkier | Microsoft Docs ”beskrivning: services: analysis services dokumentationcenter: '' författare: minewiskan manager: erikre editor: '' taggar: ''</span><span class="sxs-lookup"><span data-stu-id="3d037-101">title: aaa"Azure Analysis Services tutorial lesson 9: Create hierarchies | Microsoft Docs" description: services: analysis-services documentationcenter: '' author: minewiskan manager: erikre editor: '' tags: ''</span></span>

<span data-ttu-id="3d037-102">MS.AssetID: ms.service: analysis services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 2017-05/26 ms.author: owend</span><span class="sxs-lookup"><span data-stu-id="3d037-102">ms.assetid: ms.service: analysis-services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 05/26/2017 ms.author: owend</span></span>
---
# <a name="lesson-9-create-hierarchies"></a><span data-ttu-id="3d037-103">Lektion 9: Skapa hierarkier</span><span class="sxs-lookup"><span data-stu-id="3d037-103">Lesson 9: Create hierarchies</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="3d037-104">I den här lektionen skapar du hierarkier.</span><span class="sxs-lookup"><span data-stu-id="3d037-104">In this lesson, you create hierarchies.</span></span> <span data-ttu-id="3d037-105">Hierarkier är grupper med kolumner som är ordnade i nivåer. En geografisk hierarki kan till exempel ha undernivåer för land, delstat, län och ort.</span><span class="sxs-lookup"><span data-stu-id="3d037-105">Hierarchies are groups of columns arranged in levels; for example, a Geography hierarchy might have sublevels for Country, State, County, and City.</span></span> <span data-ttu-id="3d037-106">Hierarkier kan visas separat från andra kolumner i en reporting klienten programmet Fältlista, gör dem lättare för klienten användare toonavigate och ingå i en rapport.</span><span class="sxs-lookup"><span data-stu-id="3d037-106">Hierarchies can appear separate from other columns in a reporting client application field list, making them easier for client users toonavigate and include in a report.</span></span> <span data-ttu-id="3d037-107">Det finns fler toolearn [hierarkier](https://docs.microsoft.com/sql/analysis-services/tabular-models/hierarchies-ssas-tabular)</span><span class="sxs-lookup"><span data-stu-id="3d037-107">toolearn more, see [Hierarchies](https://docs.microsoft.com/sql/analysis-services/tabular-models/hierarchies-ssas-tabular)</span></span>
  
<span data-ttu-id="3d037-108">toocreate hierarkier använder hello modellen designer i *diagramvyn*.</span><span class="sxs-lookup"><span data-stu-id="3d037-108">toocreate hierarchies, use hello model designer in *Diagram View*.</span></span> <span data-ttu-id="3d037-109">Skapa och hantera hierarkier stöds inte i datavyn.</span><span class="sxs-lookup"><span data-stu-id="3d037-109">Creating and managing hierarchies is not supported in Data View.</span></span>  
  
<span data-ttu-id="3d037-110">Uppskattad tid toocomplete lektionen: **20 minuter**</span><span class="sxs-lookup"><span data-stu-id="3d037-110">Estimated time toocomplete this lesson: **20 minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="3d037-111">Krav</span><span class="sxs-lookup"><span data-stu-id="3d037-111">Prerequisites</span></span>  
<span data-ttu-id="3d037-112">Det här avsnittet ingår i självstudiekursen för tabellmodellering som bör slutföras i rätt ordning.</span><span class="sxs-lookup"><span data-stu-id="3d037-112">This topic is part of a tabular modeling tutorial, which should be completed in order.</span></span> <span data-ttu-id="3d037-113">Innan du utför hello uppgifter i den här lektionen bör du slutfört hello föregående lektionen: [lektionen 8: skapa perspektiv](../tutorials/aas-lesson-8-create-perspectives.md).</span><span class="sxs-lookup"><span data-stu-id="3d037-113">Before performing hello tasks in this lesson, you should have completed hello previous lesson: [Lesson 8: Create perspectives](../tutorials/aas-lesson-8-create-perspectives.md).</span></span>  
  
## <a name="create-hierarchies"></a><span data-ttu-id="3d037-114">Skapa hierarkier</span><span class="sxs-lookup"><span data-stu-id="3d037-114">Create hierarchies</span></span>  
  
#### <a name="toocreate-a-category-hierarchy-in-hello-dimproduct-table"></a><span data-ttu-id="3d037-115">toocreate en kategorihierarki i hello DimProduct tabell</span><span class="sxs-lookup"><span data-stu-id="3d037-115">toocreate a Category hierarchy in hello DimProduct table</span></span>  
  
1.  <span data-ttu-id="3d037-116">Högerklicka på hello i hello modellen designer (diagramvyn) **DimProduct** tabell > **skapa hierarkin**.</span><span class="sxs-lookup"><span data-stu-id="3d037-116">In hello model designer (diagram view), right-click hello **DimProduct** table > **Create Hierarchy**.</span></span> <span data-ttu-id="3d037-117">En ny hierarki visas längst ned hello hello tabell fönster.</span><span class="sxs-lookup"><span data-stu-id="3d037-117">A new hierarchy appears at hello bottom of hello table window.</span></span> <span data-ttu-id="3d037-118">Byt namn på hello hierarkin **kategori**.</span><span class="sxs-lookup"><span data-stu-id="3d037-118">Rename hello hierarchy **Category**.</span></span>  
  
2.  <span data-ttu-id="3d037-119">Klicka och dra hello **ProductCategoryName** kolumnen toohello nya **kategori** hierarki.</span><span class="sxs-lookup"><span data-stu-id="3d037-119">Click and drag hello **ProductCategoryName** column toohello new **Category** hierarchy.</span></span>  
  
3.  <span data-ttu-id="3d037-120">I hello **kategori** hierarki, högerklicka på hello **ProductCategoryName** > **Byt namn på**, och skriv sedan **kategori**.</span><span class="sxs-lookup"><span data-stu-id="3d037-120">In hello **Category** hierarchy, right-click hello **ProductCategoryName** > **Rename**, and then type **Category**.</span></span>  
  
    > [!NOTE]  
    > <span data-ttu-id="3d037-121">Byta namn på en kolumn i en hierarki inte byta namn på kolumnen i tabellen hello.</span><span class="sxs-lookup"><span data-stu-id="3d037-121">Renaming a column in a hierarchy does not rename that column in hello table.</span></span> <span data-ttu-id="3d037-122">En kolumn i en hierarki är bara en representation av hello kolumn i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="3d037-122">A column in a hierarchy is just a representation of hello column in hello table.</span></span>  
  
4.  <span data-ttu-id="3d037-123">Klicka och dra hello **ProductSubcategoryName** kolumnen toohello **kategori** hierarki.</span><span class="sxs-lookup"><span data-stu-id="3d037-123">Click and drag hello **ProductSubcategoryName** column toohello **Category** hierarchy.</span></span> <span data-ttu-id="3d037-124">Byt namn på den till **Underkategori**.</span><span class="sxs-lookup"><span data-stu-id="3d037-124">Rename it **Subcategory**.</span></span> 
  
5.  <span data-ttu-id="3d037-125">Högerklicka på hello **%{ModelName/** kolumnen > **lägga till toohierarchy**, och välj sedan **kategori**.</span><span class="sxs-lookup"><span data-stu-id="3d037-125">Right-click hello **ModelName** column > **Add toohierarchy**, and then select **Category**.</span></span> <span data-ttu-id="3d037-126">Byt namn på den till **Modell**.</span><span class="sxs-lookup"><span data-stu-id="3d037-126">Rename it **Model**.</span></span>

6.  <span data-ttu-id="3d037-127">Slutligen lägger du till **EnglishProductName** toohello kategorihierarki.</span><span class="sxs-lookup"><span data-stu-id="3d037-127">Finally, add **EnglishProductName** toohello Category hierarchy.</span></span> <span data-ttu-id="3d037-128">Byt namn på den till **Produkt**.</span><span class="sxs-lookup"><span data-stu-id="3d037-128">Rename it **Product**.</span></span>  

    ![aas-lesson9-category](../tutorials/media/aas-lesson9-category.png)
  
#### <a name="toocreate-hierarchies-in-hello-dimdate-table"></a><span data-ttu-id="3d037-130">toocreate hierarkier i hello DimDate tabell</span><span class="sxs-lookup"><span data-stu-id="3d037-130">toocreate hierarchies in hello DimDate table</span></span>  
  
1.  <span data-ttu-id="3d037-131">I hello **DimDate** tabell, skapa en hierarki med namnet **kalender**.</span><span class="sxs-lookup"><span data-stu-id="3d037-131">In hello **DimDate** table, create a hierarchy named **Calendar**.</span></span>  
  
3.  <span data-ttu-id="3d037-132">Lägg till hello följande kolumner i ordning:</span><span class="sxs-lookup"><span data-stu-id="3d037-132">Add hello following columns in-order:</span></span>

    *  <span data-ttu-id="3d037-133">CalendarYear</span><span class="sxs-lookup"><span data-stu-id="3d037-133">CalendarYear</span></span>
    *  <span data-ttu-id="3d037-134">CalendarSemester</span><span class="sxs-lookup"><span data-stu-id="3d037-134">CalendarSemester</span></span>
    *  <span data-ttu-id="3d037-135">CalendarQuarter</span><span class="sxs-lookup"><span data-stu-id="3d037-135">CalendarQuarter</span></span>
    *  <span data-ttu-id="3d037-136">MonthCalendar</span><span class="sxs-lookup"><span data-stu-id="3d037-136">MonthCalendar</span></span>
    *  <span data-ttu-id="3d037-137">DayNumberOfMonth</span><span class="sxs-lookup"><span data-stu-id="3d037-137">DayNumberOfMonth</span></span>
    
4.  <span data-ttu-id="3d037-138">I hello **DimDate** tabell, skapa en **fiskala** hierarki.</span><span class="sxs-lookup"><span data-stu-id="3d037-138">In hello **DimDate** table, create a **Fiscal** hierarchy.</span></span> <span data-ttu-id="3d037-139">Inkludera hello följande kolumner i ordning:</span><span class="sxs-lookup"><span data-stu-id="3d037-139">Include hello following columns in-order:</span></span>  
  
    *  <span data-ttu-id="3d037-140">FiscalYear</span><span class="sxs-lookup"><span data-stu-id="3d037-140">FiscalYear</span></span>
    *  <span data-ttu-id="3d037-141">FiscalSemester</span><span class="sxs-lookup"><span data-stu-id="3d037-141">FiscalSemester</span></span>
    *  <span data-ttu-id="3d037-142">FiscalQuarter</span><span class="sxs-lookup"><span data-stu-id="3d037-142">FiscalQuarter</span></span>
    *  <span data-ttu-id="3d037-143">MonthCalendar</span><span class="sxs-lookup"><span data-stu-id="3d037-143">MonthCalendar</span></span>
    *  <span data-ttu-id="3d037-144">DayNumberOfMonth</span><span class="sxs-lookup"><span data-stu-id="3d037-144">DayNumberOfMonth</span></span>
  
5.  <span data-ttu-id="3d037-145">Slutligen i hello **DimDate** tabell, skapa en **ProductionCalendar** hierarki.</span><span class="sxs-lookup"><span data-stu-id="3d037-145">Finally, in hello **DimDate** table, create a **ProductionCalendar** hierarchy.</span></span> <span data-ttu-id="3d037-146">Inkludera hello följande kolumner i ordning:</span><span class="sxs-lookup"><span data-stu-id="3d037-146">Include hello following columns in-order:</span></span>  
    *  <span data-ttu-id="3d037-147">CalendarYear</span><span class="sxs-lookup"><span data-stu-id="3d037-147">CalendarYear</span></span>
    *  <span data-ttu-id="3d037-148">WeekNumberOfYear</span><span class="sxs-lookup"><span data-stu-id="3d037-148">WeekNumberOfYear</span></span>
    *  <span data-ttu-id="3d037-149">DayNumberOfWeek</span><span class="sxs-lookup"><span data-stu-id="3d037-149">DayNumberOfWeek</span></span>
  
 ## <a name="whats-next"></a><span data-ttu-id="3d037-150">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3d037-150">What's next?</span></span>
<span data-ttu-id="3d037-151">[Lektion 10: Skapa partitioner](../tutorials/aas-lesson-10-create-partitions.md).</span><span class="sxs-lookup"><span data-stu-id="3d037-151">[Lesson 10: Create partitions](../tutorials/aas-lesson-10-create-partitions.md).</span></span> 
  
  
