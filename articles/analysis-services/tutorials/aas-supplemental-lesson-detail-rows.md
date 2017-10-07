---
<span data-ttu-id="9c8ca-101">Rubrik: aaa ”Azure Analysis Services självstudiekursen kompletterande lektionen: detaljrader | Microsoft Docs ”beskrivning: Beskriver hur toocreate en detalj rader uttryck i hello Azure Analysis Services-kursen.</span><span class="sxs-lookup"><span data-stu-id="9c8ca-101">title: aaa"Azure Analysis Services tutorial supplemental lesson: Detail Rows | Microsoft Docs" description: Describes how toocreate a Detail Rows Expression in hello Azure Analysis Services tutorial.</span></span>
<span data-ttu-id="9c8ca-102">tjänster: analysis services dokumentationcenter: '' författare: minewiskan manager: erikre editor: '' taggar: ''</span><span class="sxs-lookup"><span data-stu-id="9c8ca-102">services: analysis-services documentationcenter: '' author: minewiskan manager: erikre editor: '' tags: ''</span></span>

<span data-ttu-id="9c8ca-103">MS.AssetID: ms.service: analysis services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 2017-05/26 ms.author: owend</span><span class="sxs-lookup"><span data-stu-id="9c8ca-103">ms.assetid: ms.service: analysis-services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 05/26/2017 ms.author: owend</span></span>
---
# <a name="supplemental-lesson---detail-rows"></a><span data-ttu-id="9c8ca-104">Kompletterande lektion – Detaljrader</span><span class="sxs-lookup"><span data-stu-id="9c8ca-104">Supplemental lesson - Detail Rows</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="9c8ca-105">Kompletterande nu bör använda du hello DAX Editor toodefine ett uttryck i detalj rader.</span><span class="sxs-lookup"><span data-stu-id="9c8ca-105">In this supplemental lesson, you use hello DAX Editor toodefine a custom Detail Rows Expression.</span></span> <span data-ttu-id="9c8ca-106">Ett uttryck för information om rader är en egenskap på ett mått och ger slutanvändare mer information om hello visar det sammanlagda resultatet för ett mått.</span><span class="sxs-lookup"><span data-stu-id="9c8ca-106">A Detail Rows Expression is a property on a measure, providing end-users more information about hello aggregated results of a measure.</span></span> 
  
<span data-ttu-id="9c8ca-107">Uppskattad tid toocomplete lektionen: **10 minuter**</span><span class="sxs-lookup"><span data-stu-id="9c8ca-107">Estimated time toocomplete this lesson: **10 minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="9c8ca-108">Krav</span><span class="sxs-lookup"><span data-stu-id="9c8ca-108">Prerequisites</span></span>  
<span data-ttu-id="9c8ca-109">Den här kompletterande lektionen ingår i en självstudiekurs om tabellmodeller.</span><span class="sxs-lookup"><span data-stu-id="9c8ca-109">This supplemental lesson topic is part of a tabular modeling tutorial.</span></span> <span data-ttu-id="9c8ca-110">Innan du utför hello uppgifter i den här kompletterande lektionen bör du slutfört alla tidigare erfarenheter eller har en slutförd Adventure Works Internet försäljning modellen exempelprojektet.</span><span class="sxs-lookup"><span data-stu-id="9c8ca-110">Before performing hello tasks in this supplemental lesson, you should have completed all previous lessons or have a completed Adventure Works Internet Sales sample model project.</span></span>  
  
## <a name="what-do-we-need-toosolve"></a><span data-ttu-id="9c8ca-111">Vad gör vi behöver toosolve?</span><span class="sxs-lookup"><span data-stu-id="9c8ca-111">What do we need toosolve?</span></span>
<span data-ttu-id="9c8ca-112">Nu ska vi titta hello information för våra InternetTotalSales mått innan du lägger till ett uttryck för information om rader.</span><span class="sxs-lookup"><span data-stu-id="9c8ca-112">Let's look at hello details of our InternetTotalSales measure, before adding a Detail Rows Expression.</span></span>

1.  <span data-ttu-id="9c8ca-113">Klicka på hello i SSDT, **modellen** menyn > **analysera i Excel** tooopen Excel och skapa en tom pivottabell.</span><span class="sxs-lookup"><span data-stu-id="9c8ca-113">In SSDT, click hello **Model** menu > **Analyze in Excel** tooopen Excel and create a blank PivotTable.</span></span>
  
2.  <span data-ttu-id="9c8ca-114">I **PivotTable-Fields**, lägga till hello **InternetTotalSales** för att mäta från hello FactInternetSales tabell**värden**, **CalendarYear**från hello DimDate tabell för**kolumner**, och **EnglishCountryRegionName** för**rader**.</span><span class="sxs-lookup"><span data-stu-id="9c8ca-114">In **PivotTable Fields**, add hello **InternetTotalSales** measure from hello FactInternetSales table too**Values**, **CalendarYear** from hello DimDate table too**Columns**, and **EnglishCountryRegionName** too**Rows**.</span></span> <span data-ttu-id="9c8ca-115">Vår pivottabell kan nu oss sammanlagda resultat från hello InternetTotalSales mått av regioner och år.</span><span class="sxs-lookup"><span data-stu-id="9c8ca-115">Our PivotTable now gives us aggregated results from hello InternetTotalSales measure by regions and year.</span></span> 

    ![aas-lesson-detail-rows-pivottable](../tutorials/media/aas-lesson-detail-rows-pivottable.png)

3. <span data-ttu-id="9c8ca-117">Dubbelklicka på ett insamlat värde för ett år och ett regionnamn i hello pivottabellen.</span><span class="sxs-lookup"><span data-stu-id="9c8ca-117">In hello PivotTable, double-click an aggregated value for a year and a region name.</span></span> <span data-ttu-id="9c8ca-118">Här dubbelklickar vi hello värde för Australien och hello år 2014.</span><span class="sxs-lookup"><span data-stu-id="9c8ca-118">Here we double-clicked hello value for Australia and hello year 2014.</span></span> <span data-ttu-id="9c8ca-119">Ett nytt blad öppnas som innehåller data, men inte användbara data.</span><span class="sxs-lookup"><span data-stu-id="9c8ca-119">A new sheet opens containing data, but not useful data.</span></span>

    ![aas-lesson-detail-rows-pivottable](../tutorials/media/aas-lesson-detail-rows-sheet.png)
  
<span data-ttu-id="9c8ca-121">Vad vi vill toosee här är en tabell som innehåller kolumner och rader med data som bidrar toohello visar det sammanlagda resultatet för våra InternetTotalSales mått.</span><span class="sxs-lookup"><span data-stu-id="9c8ca-121">What we would like toosee here is a table containing columns and rows of data that contribute toohello aggregated result of our InternetTotalSales measure.</span></span> <span data-ttu-id="9c8ca-122">toodo att vi kan lägga till ett uttryck för information om rader som en egenskap för hello mått.</span><span class="sxs-lookup"><span data-stu-id="9c8ca-122">toodo that, we can add a Detail Rows Expression as a property of hello measure.</span></span>

## <a name="add-a-detail-rows-expression"></a><span data-ttu-id="9c8ca-123">Lägga till ett uttryck för rader med detaljerad information</span><span class="sxs-lookup"><span data-stu-id="9c8ca-123">Add a Detail Rows Expression</span></span>

#### <a name="toocreate-a-detail-rows-expression"></a><span data-ttu-id="9c8ca-124">toocreate ett detaljerat rader uttryck</span><span class="sxs-lookup"><span data-stu-id="9c8ca-124">toocreate a Detail Rows Expression</span></span> 
  
1. <span data-ttu-id="9c8ca-125">Klicka på hello i hello FactInternetSales tabell mått rutnät i SSDT, **InternetTotalSales** mått.</span><span class="sxs-lookup"><span data-stu-id="9c8ca-125">In SSDT, in hello FactInternetSales table's measure grid, click hello **InternetTotalSales** measure.</span></span> 

2. <span data-ttu-id="9c8ca-126">I **egenskaper** > **detalj rader uttryck**, klicka på hello editor knappen tooopen hello DAX-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="9c8ca-126">In **Properties** > **Detail Rows Expression**, click hello editor button tooopen hello DAX Editor.</span></span>

    ![aas-lesson-detail-rows-ellipse](../tutorials/media/aas-lesson-detail-rows-ellipse.png)

3. <span data-ttu-id="9c8ca-128">DAX-redigeraren, ange hello följande uttryck:</span><span class="sxs-lookup"><span data-stu-id="9c8ca-128">In DAX Editor, enter hello following expression:</span></span>

    ```
    SELECTCOLUMNS(
    FactInternetSales,
    "Sales Order Number", FactInternetSales[SalesOrderNumber],
    "Customer First Name", RELATED(DimCustomer[FirstName]),
    "Customer Last Name", RELATED(DimCustomer[LastName]),
    "City", RELATED(DimGeography[City]),
    "Order Date", FactInternetSales[OrderDate],
    "Internet Total Sales", [InternetTotalSales]
    )

    ```

    <span data-ttu-id="9c8ca-129">Det här uttrycket anger namn, kolumner och mått från hello FactInternetSales tabell och relaterade tabeller returneras när en användare dubbelklickar på ett aggregerade resultat i en pivottabell eller rapporten.</span><span class="sxs-lookup"><span data-stu-id="9c8ca-129">This expression specifies names, columns, and measure results from hello FactInternetSales table and related tables are returned when a user double-clicks an aggregated result in a PivotTable or report.</span></span>

4. <span data-ttu-id="9c8ca-130">Ta bort hello blad skapade i steg3 i Excel, och dubbelklicka på ett insamlat värde.</span><span class="sxs-lookup"><span data-stu-id="9c8ca-130">Back in Excel, delete hello sheet created in Step 3, then double-click an aggregated value.</span></span> <span data-ttu-id="9c8ca-131">Den här gången med en detalj rader uttryck-egenskap som definierats för hello mått, ett nytt blad öppnas med mycket mer användbar data.</span><span class="sxs-lookup"><span data-stu-id="9c8ca-131">This time, with a Detail Rows Expression property defined for hello measure, a new sheet opens containing a lot more useful data.</span></span>

    ![aas-lesson-detail-rows-detailsheet](../tutorials/media/aas-lesson-detail-rows-detailsheet.png)

5. <span data-ttu-id="9c8ca-133">Distribuera om din modell.</span><span class="sxs-lookup"><span data-stu-id="9c8ca-133">Redeploy your model.</span></span>

  
## <a name="see-also"></a><span data-ttu-id="9c8ca-134">Se även</span><span class="sxs-lookup"><span data-stu-id="9c8ca-134">See Also</span></span>  
<span data-ttu-id="9c8ca-135">[Funktionen SELECTCOLUMNS (DAX)](https://msdn.microsoft.com/library/mt761759.aspx) </span><span class="sxs-lookup"><span data-stu-id="9c8ca-135">[SELECTCOLUMNS Function (DAX)](https://msdn.microsoft.com/library/mt761759.aspx) </span></span>  
[<span data-ttu-id="9c8ca-136">Kompletterande lektion – Dynamisk säkerhet</span><span class="sxs-lookup"><span data-stu-id="9c8ca-136">Supplemental Lesson - Dynamic security</span></span>](../tutorials/aas-supplemental-lesson-dynamic-security.md)  
[<span data-ttu-id="9c8ca-137">Kompletterande lektion – Ojämna hierarkier</span><span class="sxs-lookup"><span data-stu-id="9c8ca-137">Supplemental Lesson - Ragged hierarchies</span></span>](../tutorials/aas-supplemental-lesson-ragged-hierarchies.md)  
