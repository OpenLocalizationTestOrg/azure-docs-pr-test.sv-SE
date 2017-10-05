---
title: "Azure Analysis Services självstudiekurs 3: Markera som datumtabell | Microsoft Docs"
description: "Beskriver hur du markerar en datumtabell i Azure Analysis Services-självstudiekursprojektet."
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
ms.openlocfilehash: c62f2726fef5219155a08b70c61162c914600d1d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="lesson-3-mark-as-date-table"></a><span data-ttu-id="23237-103">Lektion 3: Markera som datumtabell</span><span class="sxs-lookup"><span data-stu-id="23237-103">Lesson 3: Mark as Date Table</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="23237-104">I ”lektion 2: Hämta data” importerade du en dimensionstabell med namnet DimDate.</span><span class="sxs-lookup"><span data-stu-id="23237-104">In Lesson 2: Get data, you imported a dimension table named DimDate.</span></span> <span data-ttu-id="23237-105">I din modell heter den här tabellen DimDate, och den kan också kallas *datumtabell* eftersom den innehåller information om datum och tid.</span><span class="sxs-lookup"><span data-stu-id="23237-105">While in your model this table is named DimDate, it can also be known as a *Date table*, in that it contains date and time data.</span></span>  
  
<span data-ttu-id="23237-106">När du använder funktioner för DAX-tidsinformation, till exempel när du skapar mått senare, måste du ange egenskaper som innehåller en *datumtabell* och en unik *datumkolumn*-identifierare i tabellen.</span><span class="sxs-lookup"><span data-stu-id="23237-106">Whenever you use DAX time-intelligence functions, like when you create measures later, you must specify properties which include a *Date table* and a unique identifier *Date column* in that table.</span></span>
  
<span data-ttu-id="23237-107">Under den här lektionen markerar du DimDate-tabellen som *datumtabell* och datumkolumnen (i datumtabellen) som *datumkolumn* (unik identifierare).</span><span class="sxs-lookup"><span data-stu-id="23237-107">In this lesson, you mark the DimDate table as the *Date table* and the Date column (in the Date table) as the *Date column* (unique identifier).</span></span>  

<span data-ttu-id="23237-108">Innan du markerar datumtabellen och datumkolumnen är det dags att göra några ändringar så att det blir enklare att förstå din modell.</span><span class="sxs-lookup"><span data-stu-id="23237-108">Before you mark the date table and date column, it's a good time to do a little housekeeping to make your model easier to understand.</span></span> <span data-ttu-id="23237-109">Observera att det finns en kolumn med namnet **FullDateAlternateKey** i tabellen DimDate.</span><span class="sxs-lookup"><span data-stu-id="23237-109">Notice in the DimDate table a column named **FullDateAlternateKey**.</span></span> <span data-ttu-id="23237-110">Den här kolumnen innehåller en rad för varje dag i varje kalenderår som ingår i tabellen.</span><span class="sxs-lookup"><span data-stu-id="23237-110">This column contains one row for every day in each calendar year included in the table.</span></span> <span data-ttu-id="23237-111">Du använder ofta den här kolumnen i måttformler och i rapporter.</span><span class="sxs-lookup"><span data-stu-id="23237-111">You use this column a lot in measure formulas and in reports.</span></span> <span data-ttu-id="23237-112">Men FullDateAlternateKey är inte så bra identifierare för den här kolumnen.</span><span class="sxs-lookup"><span data-stu-id="23237-112">But, FullDateAlternateKey isn't really a good identifier for this column.</span></span> <span data-ttu-id="23237-113">Du döper om den till **Datum** så att den blir enklare att identifiera och ta med i formler.</span><span class="sxs-lookup"><span data-stu-id="23237-113">You rename it to **Date**, making it easier to identify and include in formulas.</span></span> <span data-ttu-id="23237-114">Om det går är det en god idé att byta namn på objekt som tabeller och kolumner så att de blir enklare att identifiera i SSDT och klientrapporteringsprogram som Power BI och Excel.</span><span class="sxs-lookup"><span data-stu-id="23237-114">Whenever possible, it's a good idea to rename objects like tables and columns to make them easier to identify in SSDT and client reporting applications like Power BI and Excel.</span></span> 
  
<span data-ttu-id="23237-115">Uppskattad tidsåtgång för den här lektionen: **Tre minuter**</span><span class="sxs-lookup"><span data-stu-id="23237-115">Estimated time to complete this lesson: **Three minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="23237-116">Krav</span><span class="sxs-lookup"><span data-stu-id="23237-116">Prerequisites</span></span>  
<span data-ttu-id="23237-117">Det här avsnittet ingår i självstudiekursen för tabellmodellering som bör slutföras i rätt ordning.</span><span class="sxs-lookup"><span data-stu-id="23237-117">This topic is part of a tabular modeling tutorial, which should be completed in order.</span></span> <span data-ttu-id="23237-118">Innan du utför uppgifterna under den här lektionen bör du ha slutfört föregående lektion: [Lektion 2: Hämta data](../tutorials/aas-lesson-2-get-data.md).</span><span class="sxs-lookup"><span data-stu-id="23237-118">Before performing the tasks in this lesson, you should have completed the previous lesson: [Lesson 2: Get data](../tutorials/aas-lesson-2-get-data.md).</span></span> 

### <a name="to-rename-the-fulldatealternatekey-column"></a><span data-ttu-id="23237-119">Byta namn på kolumnen FullDateAlternateKey</span><span class="sxs-lookup"><span data-stu-id="23237-119">To rename the FullDateAlternateKey column</span></span>

1.  <span data-ttu-id="23237-120">Klicka på tabellen **DimDate** i modelldesignern.</span><span class="sxs-lookup"><span data-stu-id="23237-120">In the model designer, click the **DimDate** table.</span></span>

2.  <span data-ttu-id="23237-121">Dubbelklicka på rubriken för kolumnen **FullDateAlternateKey** och byt sedan namn till **Datum**.</span><span class="sxs-lookup"><span data-stu-id="23237-121">Double-click the header for the **FullDateAlternateKey** column, and then rename it to **Date**.</span></span>

  
### <a name="to-set-mark-as-date-table"></a><span data-ttu-id="23237-122">Markera som datumtabell</span><span class="sxs-lookup"><span data-stu-id="23237-122">To set Mark as Date Table</span></span>  
  
1.  <span data-ttu-id="23237-123">Välj kolumnen **Datum** och kontrollera sedan att **Datum** är markerat under **Datatyp** i fönstret **Egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="23237-123">Select the **Date** column, and then in the **Properties** window, under **Data Type**, make sure  **Date** is selected.</span></span>  
  
2.  <span data-ttu-id="23237-124">Klicka på **Tabell**-menyn, klicka på **Datum** och klicka sedan på **Markera som datumtabell**.</span><span class="sxs-lookup"><span data-stu-id="23237-124">Click the **Table** menu, then click **Date**, and then click **Mark as Date Table**.</span></span>  
  
3.  <span data-ttu-id="23237-125">I dialogrutan **Markera som datumtabell** väljer du kolumnen **Datum** som unik identifierare i listrutan **Datum**.</span><span class="sxs-lookup"><span data-stu-id="23237-125">In the **Mark as Date Table** dialog box, in the **Date** listbox, select the **Date** column as the unique identifier.</span></span> <span data-ttu-id="23237-126">Den är vanligtvis markerad som standard.</span><span class="sxs-lookup"><span data-stu-id="23237-126">It's usually selected by default.</span></span> <span data-ttu-id="23237-127">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="23237-127">Click **OK**.</span></span> 

    ![aas-lesson3-date-table](../tutorials/media/aas-lesson3-date-table.png)
  

## <a name="whats-next"></a><span data-ttu-id="23237-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="23237-129">What's next?</span></span>
<span data-ttu-id="23237-130">[Lektion 4: Skapa relationer](../tutorials/aas-lesson-4-create-relationships.md).</span><span class="sxs-lookup"><span data-stu-id="23237-130">[Lesson 4: Create relationships](../tutorials/aas-lesson-4-create-relationships.md).</span></span>
  
