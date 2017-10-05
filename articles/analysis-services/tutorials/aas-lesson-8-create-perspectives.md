---
title: "Azure Analysis Services självstudiekurs 8: Skapa perspektiv | Microsoft Docs"
description: "Beskriver hur du skapar perspektiv i Azure Analysis Services-självstudieprojektet."
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
ms.openlocfilehash: 491500909b0de0360afae45e172e85999d764fe0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="lesson-8-create-perspectives"></a><span data-ttu-id="9c01e-103">Lektion 8: Skapa perspektiv</span><span class="sxs-lookup"><span data-stu-id="9c01e-103">Lesson 8: Create perspectives</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="9c01e-104">I den här lektionen skapar du ett perspektiv för Internetförsäljning.</span><span class="sxs-lookup"><span data-stu-id="9c01e-104">In this lesson, you create an Internet Sales perspective.</span></span> <span data-ttu-id="9c01e-105">Ett perspektiv definierar en visningsbar delmängd av en modell som ger fokuserade, affärsspecifika eller programspecifika översiktsvyer.</span><span class="sxs-lookup"><span data-stu-id="9c01e-105">A perspective defines a viewable subset of a model that provides focused, business-specific, or application-specific viewpoints.</span></span> <span data-ttu-id="9c01e-106">När en användare ansluter till en modell med hjälp av ett perspektiv visas endast de modellobjekt (tabeller, kolumner, mått, hierarkier och KPI:er), eller fält, som definierats i det perspektivet.</span><span class="sxs-lookup"><span data-stu-id="9c01e-106">When a user connects to a model by using a perspective, they see only those model objects (tables, columns, measures, hierarchies, and KPIs) as fields defined in that perspective.</span></span> <span data-ttu-id="9c01e-107">Mer information finns i [Partitioner](https://docs.microsoft.com/sql/analysis-services/tabular-models/perspectives-ssas-tabular).</span><span class="sxs-lookup"><span data-stu-id="9c01e-107">To learn more, see [Perspectives](https://docs.microsoft.com/sql/analysis-services/tabular-models/perspectives-ssas-tabular).</span></span>
  
<span data-ttu-id="9c01e-108">Perspektivet Internetförsäljning som du skapar i den här lektionen exkluderar tabellobjektet DimCustomer.</span><span class="sxs-lookup"><span data-stu-id="9c01e-108">The Internet Sales perspective you create in this lesson excludes the DimCustomer table object.</span></span> <span data-ttu-id="9c01e-109">När du skapar ett perspektiv som exkluderar visning av ett visst objekt existerar objektet fortfarande i modellen.</span><span class="sxs-lookup"><span data-stu-id="9c01e-109">When you create a perspective that excludes certain objects from view, that object still exists in the model.</span></span> <span data-ttu-id="9c01e-110">Men det är inte synligt i fältlistan i rapporteringsklienten.</span><span class="sxs-lookup"><span data-stu-id="9c01e-110">However, it is not visible in a reporting client field list.</span></span> <span data-ttu-id="9c01e-111">Beräknade kolumner och mått, som antingen är inkluderade i ett perspektiv eller inte, kan fortfarande göra beräkningar från objektdata som exkluderas.</span><span class="sxs-lookup"><span data-stu-id="9c01e-111">Calculated columns and measures either included in a perspective or not can still calculate from object data that is excluded.</span></span>  
  
<span data-ttu-id="9c01e-112">I den här lektionen visar vi hur du skapar perspektiv och du får bekanta dig med tabellmodellens redigeringsverktyg.</span><span class="sxs-lookup"><span data-stu-id="9c01e-112">The purpose of this lesson is to describe how to create perspectives and become familiar with the tabular model authoring tools.</span></span> <span data-ttu-id="9c01e-113">Om du senare väljer att utöka modellen med ytterligare tabeller kan du skapa ytterligare perspektiv för att definiera olika översiktsvyer för modellen, till exempel Lager och Försäljning.</span><span class="sxs-lookup"><span data-stu-id="9c01e-113">If you later expand this model to include additional tables, you can create additional perspectives to define different viewpoints of the model, for example, Inventory and Sales.</span></span>  
  
<span data-ttu-id="9c01e-114">Uppskattad tidsåtgång för den här lektionen: **Fem minuter**</span><span class="sxs-lookup"><span data-stu-id="9c01e-114">Estimated time to complete this lesson: **Five minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="9c01e-115">Krav</span><span class="sxs-lookup"><span data-stu-id="9c01e-115">Prerequisites</span></span>  
<span data-ttu-id="9c01e-116">Det här avsnittet ingår i självstudiekursen för tabellmodellering som bör slutföras i rätt ordning.</span><span class="sxs-lookup"><span data-stu-id="9c01e-116">This topic is part of a tabular modeling tutorial, which should be completed in order.</span></span> <span data-ttu-id="9c01e-117">Innan du utför uppgifterna under den här lektionen måste du ha slutfört föregående lektion: [Lektion 7: Skapa KPI:er](../tutorials/aas-lesson-7-create-key-performance-indicators.md).</span><span class="sxs-lookup"><span data-stu-id="9c01e-117">Before performing the tasks in this lesson, you should have completed the previous lesson: [Lesson 7: Create Key Performance Indicators](../tutorials/aas-lesson-7-create-key-performance-indicators.md).</span></span>  
  
## <a name="create-perspectives"></a><span data-ttu-id="9c01e-118">Skapa perspektiv</span><span class="sxs-lookup"><span data-stu-id="9c01e-118">Create perspectives</span></span>  
  
#### <a name="to-create-an-internet-sales-perspective"></a><span data-ttu-id="9c01e-119">Så här skapar du ett perspektiv för Internetförsäljning</span><span class="sxs-lookup"><span data-stu-id="9c01e-119">To create an Internet Sales perspective</span></span>  
  
1.  <span data-ttu-id="9c01e-120">Klicka på **Modell**-menyn > **Perspektiv** > **Skapa och hantera**.</span><span class="sxs-lookup"><span data-stu-id="9c01e-120">Click the **Model** menu > **Perspectives** > **Create and Manage**.</span></span>  
  
2.  <span data-ttu-id="9c01e-121">Klicka på **Nytt perspektiv** i dialogrutan **Perspektiv**.</span><span class="sxs-lookup"><span data-stu-id="9c01e-121">In the **Perspectives** dialog box, click **New Perspective**.</span></span>  
  
3.  <span data-ttu-id="9c01e-122">Dubbelklicka på kolumnrubriken **Nytt perspektiv** och ändra sedan namnet till **Internetförsäljning**.</span><span class="sxs-lookup"><span data-stu-id="9c01e-122">Double-click the **New Perspective** column heading, and then rename **Internet Sales**.</span></span>  
  
4.  <span data-ttu-id="9c01e-123">Markera alla tabeller *utom* **DimCustomer**.</span><span class="sxs-lookup"><span data-stu-id="9c01e-123">Select the all the tables *except* **DimCustomer**.</span></span>  
  
    ![aas-lesson8-perspectives](../tutorials/media/aas-lesson8-perspectives.png)
  
    <span data-ttu-id="9c01e-125">I en senare lektion använder du funktionen Analysera i Excel för att testa perspektivet.</span><span class="sxs-lookup"><span data-stu-id="9c01e-125">In a later lesson, you use the Analyze in Excel feature to test this perspective.</span></span> <span data-ttu-id="9c01e-126">Fältlistan för pivottabell i Excel innehåller alla tabeller utom tabellen DimCustomer.</span><span class="sxs-lookup"><span data-stu-id="9c01e-126">The Excel PivotTable Fields List includes each table except the DimCustomer table.</span></span>  

## <a name="whats-next"></a><span data-ttu-id="9c01e-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9c01e-127">What's next?</span></span>
<span data-ttu-id="9c01e-128">[Lektion 9: Skapa hierarkier](../tutorials/aas-lesson-9-create-hierarchies.md).</span><span class="sxs-lookup"><span data-stu-id="9c01e-128">[Lesson 9: Create hierarchies](../tutorials/aas-lesson-9-create-hierarchies.md).</span></span>
  
  
  
  
