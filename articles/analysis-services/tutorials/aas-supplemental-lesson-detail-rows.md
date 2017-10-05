---
title: "Kompletterande lektion i Azure Analysis Services-självstudiekurs: Detaljrader | Microsoft Docs"
description: "Beskriver hur du skapar uttryck för rader med detaljerad information i Azure Analysis Services-självstudiekursen."
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
ms.date: 05/26/2017
ms.author: owend
ms.openlocfilehash: fde5cd9a9efc3a13e731a91962ced5c086a72355
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="supplemental-lesson---detail-rows"></a><span data-ttu-id="7933f-103">Kompletterande lektion – Detaljrader</span><span class="sxs-lookup"><span data-stu-id="7933f-103">Supplemental lesson - Detail Rows</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="7933f-104">I den här kompletterande lektionen använder du DAX-redigeraren för att definiera ett anpassad uttryck för rader med detaljerad information.</span><span class="sxs-lookup"><span data-stu-id="7933f-104">In this supplemental lesson, you use the DAX Editor to define a custom Detail Rows Expression.</span></span> <span data-ttu-id="7933f-105">Ett uttryck för rader med detaljerad information är en egenskap för ett mått som ger slutanvändarna mer information om det aggregerade resultatet av ett mått.</span><span class="sxs-lookup"><span data-stu-id="7933f-105">A Detail Rows Expression is a property on a measure, providing end-users more information about the aggregated results of a measure.</span></span> 
  
<span data-ttu-id="7933f-106">Uppskattad tidsåtgång för den här lektionen: **10 minuter**</span><span class="sxs-lookup"><span data-stu-id="7933f-106">Estimated time to complete this lesson: **10 minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="7933f-107">Krav</span><span class="sxs-lookup"><span data-stu-id="7933f-107">Prerequisites</span></span>  
<span data-ttu-id="7933f-108">Den här kompletterande lektionen ingår i en självstudiekurs om tabellmodeller.</span><span class="sxs-lookup"><span data-stu-id="7933f-108">This supplemental lesson topic is part of a tabular modeling tutorial.</span></span> <span data-ttu-id="7933f-109">Innan du utför uppgifterna i den här kompletterande lektionen måste du ha slutfört alla föregående lektioner eller ha ett slutfört Adventure Works Internet Sales-exempelmodellprojekt.</span><span class="sxs-lookup"><span data-stu-id="7933f-109">Before performing the tasks in this supplemental lesson, you should have completed all previous lessons or have a completed Adventure Works Internet Sales sample model project.</span></span>  
  
## <a name="what-do-we-need-to-solve"></a><span data-ttu-id="7933f-110">Vad behöver vi lösa?</span><span class="sxs-lookup"><span data-stu-id="7933f-110">What do we need to solve?</span></span>
<span data-ttu-id="7933f-111">Låt oss titta på informationen för vårt InternetTotalSales-mått innan vi lägger till ett uttryck för rader med detaljerad information.</span><span class="sxs-lookup"><span data-stu-id="7933f-111">Let's look at the details of our InternetTotalSales measure, before adding a Detail Rows Expression.</span></span>

1.  <span data-ttu-id="7933f-112">Klicka på menyn **Modell** > **Analysera i Excel** i SSDT för att öppna Excel och skapa en tom pivottabell.</span><span class="sxs-lookup"><span data-stu-id="7933f-112">In SSDT, click the **Model** menu > **Analyze in Excel** to open Excel and create a blank PivotTable.</span></span>
  
2.  <span data-ttu-id="7933f-113">I **Pivottabellfält** lägger du till måttet **InternetTotalSales** från tabellen FactInternetSales till **Values**, **CalendarYear** från tabellen DimDate till **Columns** och **EnglishCountryRegionName** till **Rows**.</span><span class="sxs-lookup"><span data-stu-id="7933f-113">In **PivotTable Fields**, add the **InternetTotalSales** measure from the FactInternetSales table to **Values**, **CalendarYear** from the DimDate table to **Columns**, and **EnglishCountryRegionName** to **Rows**.</span></span> <span data-ttu-id="7933f-114">Pivottabellen ger oss nu aggregerade resultat från måttet InternetTotalSales per region och år.</span><span class="sxs-lookup"><span data-stu-id="7933f-114">Our PivotTable now gives us aggregated results from the InternetTotalSales measure by regions and year.</span></span> 

    ![aas-lesson-detail-rows-pivottable](../tutorials/media/aas-lesson-detail-rows-pivottable.png)

3. <span data-ttu-id="7933f-116">Dubbelklicka på ett aggregerat värde för ett år och ett regionnamn i pivottabellen.</span><span class="sxs-lookup"><span data-stu-id="7933f-116">In the PivotTable, double-click an aggregated value for a year and a region name.</span></span> <span data-ttu-id="7933f-117">Här dubbelklickade vi på värdet för Australien år 2014.</span><span class="sxs-lookup"><span data-stu-id="7933f-117">Here we double-clicked the value for Australia and the year 2014.</span></span> <span data-ttu-id="7933f-118">Ett nytt blad öppnas som innehåller data, men inte användbara data.</span><span class="sxs-lookup"><span data-stu-id="7933f-118">A new sheet opens containing data, but not useful data.</span></span>

    ![aas-lesson-detail-rows-pivottable](../tutorials/media/aas-lesson-detail-rows-sheet.png)
  
<span data-ttu-id="7933f-120">Det vi vill se här är en tabell med kolumner och rader med data som bidrar till det aggregerade resultatet av vårt InternetTotalSales-mått.</span><span class="sxs-lookup"><span data-stu-id="7933f-120">What we would like to see here is a table containing columns and rows of data that contribute to the aggregated result of our InternetTotalSales measure.</span></span> <span data-ttu-id="7933f-121">Det ordnar vi genom att lägga till ett uttryck för rader med detaljerad information som en egenskap för måttet.</span><span class="sxs-lookup"><span data-stu-id="7933f-121">To do that, we can add a Detail Rows Expression as a property of the measure.</span></span>

## <a name="add-a-detail-rows-expression"></a><span data-ttu-id="7933f-122">Lägga till ett uttryck för rader med detaljerad information</span><span class="sxs-lookup"><span data-stu-id="7933f-122">Add a Detail Rows Expression</span></span>

#### <a name="to-create-a-detail-rows-expression"></a><span data-ttu-id="7933f-123">Så här skapar du ett uttryck för rader med detaljerad information</span><span class="sxs-lookup"><span data-stu-id="7933f-123">To create a Detail Rows Expression</span></span> 
  
1. <span data-ttu-id="7933f-124">Klicka på måttet **InternetTotalSales** i FactInternetSales-tabellens rutnät för mått i SSDT.</span><span class="sxs-lookup"><span data-stu-id="7933f-124">In SSDT, in the FactInternetSales table's measure grid, click the **InternetTotalSales** measure.</span></span> 

2. <span data-ttu-id="7933f-125">I **Egenskaper** > **Uttryck för rader med detaljerad information** klickar du på knappen för redigeraren för att öppna DAX-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="7933f-125">In **Properties** > **Detail Rows Expression**, click the editor button to open the DAX Editor.</span></span>

    ![aas-lesson-detail-rows-ellipse](../tutorials/media/aas-lesson-detail-rows-ellipse.png)

3. <span data-ttu-id="7933f-127">Ange följande uttryck i DAX-redigeraren:</span><span class="sxs-lookup"><span data-stu-id="7933f-127">In DAX Editor, enter the following expression:</span></span>

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

    <span data-ttu-id="7933f-128">Det här uttrycket anger namn, kolumner och måttresultat från tabellen FactInternetSales och relaterade tabeller returneras om en användare dubbelklickar på ett aggregerat resultat i en pivottabell eller rapport.</span><span class="sxs-lookup"><span data-stu-id="7933f-128">This expression specifies names, columns, and measure results from the FactInternetSales table and related tables are returned when a user double-clicks an aggregated result in a PivotTable or report.</span></span>

4. <span data-ttu-id="7933f-129">Gå tillbaka till Excel, ta bort bladet som du skapade i steg 3 och dubbelklicka på ett aggregerat värde.</span><span class="sxs-lookup"><span data-stu-id="7933f-129">Back in Excel, delete the sheet created in Step 3, then double-click an aggregated value.</span></span> <span data-ttu-id="7933f-130">Nu när en egenskap för uttryck för rader med detaljerad information har definierats för måttet öppnas ett nytt blad med mer användbara data.</span><span class="sxs-lookup"><span data-stu-id="7933f-130">This time, with a Detail Rows Expression property defined for the measure, a new sheet opens containing a lot more useful data.</span></span>

    ![aas-lesson-detail-rows-detailsheet](../tutorials/media/aas-lesson-detail-rows-detailsheet.png)

5. <span data-ttu-id="7933f-132">Distribuera om din modell.</span><span class="sxs-lookup"><span data-stu-id="7933f-132">Redeploy your model.</span></span>

  
## <a name="see-also"></a><span data-ttu-id="7933f-133">Se även</span><span class="sxs-lookup"><span data-stu-id="7933f-133">See Also</span></span>  
<span data-ttu-id="7933f-134">[Funktionen SELECTCOLUMNS (DAX)](https://msdn.microsoft.com/library/mt761759.aspx) </span><span class="sxs-lookup"><span data-stu-id="7933f-134">[SELECTCOLUMNS Function (DAX)](https://msdn.microsoft.com/library/mt761759.aspx) </span></span>  
[<span data-ttu-id="7933f-135">Kompletterande lektion – Dynamisk säkerhet</span><span class="sxs-lookup"><span data-stu-id="7933f-135">Supplemental Lesson - Dynamic security</span></span>](../tutorials/aas-supplemental-lesson-dynamic-security.md)  
[<span data-ttu-id="7933f-136">Kompletterande lektion – Ojämna hierarkier</span><span class="sxs-lookup"><span data-stu-id="7933f-136">Supplemental Lesson - Ragged hierarchies</span></span>](../tutorials/aas-supplemental-lesson-ragged-hierarchies.md)  
