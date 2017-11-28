---
title: "aaa ”Azure Analysis Services självstudiekursen lektionen 8 skapa perspektiv | Microsoft Docs ”"
description: "Beskriver hur toocreate perspektiv i hello självstudiekursen Azure Analysis Services-projekt."
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
ms.openlocfilehash: 25391813e1969ecb22af4d6f9c1ccd8358d812fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="lesson-8-create-perspectives"></a><span data-ttu-id="937d8-103">Lektion 8: Skapa perspektiv</span><span class="sxs-lookup"><span data-stu-id="937d8-103">Lesson 8: Create perspectives</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="937d8-104">I den här lektionen skapar du ett perspektiv för Internetförsäljning.</span><span class="sxs-lookup"><span data-stu-id="937d8-104">In this lesson, you create an Internet Sales perspective.</span></span> <span data-ttu-id="937d8-105">Ett perspektiv definierar en visningsbar delmängd av en modell som ger fokuserade, affärsspecifika eller programspecifika översiktsvyer.</span><span class="sxs-lookup"><span data-stu-id="937d8-105">A perspective defines a viewable subset of a model that provides focused, business-specific, or application-specific viewpoints.</span></span> <span data-ttu-id="937d8-106">När en användare ansluter tooa modellen med hjälp av ett perspektiv, visas endast de modellobjekt (tabeller, kolumner, mått, hierarkier och KPI: er) som fält som definierats i det perspektivet.</span><span class="sxs-lookup"><span data-stu-id="937d8-106">When a user connects tooa model by using a perspective, they see only those model objects (tables, columns, measures, hierarchies, and KPIs) as fields defined in that perspective.</span></span> <span data-ttu-id="937d8-107">Det finns fler toolearn [perspektiv](https://docs.microsoft.com/sql/analysis-services/tabular-models/perspectives-ssas-tabular).</span><span class="sxs-lookup"><span data-stu-id="937d8-107">toolearn more, see [Perspectives](https://docs.microsoft.com/sql/analysis-services/tabular-models/perspectives-ssas-tabular).</span></span>
  
<span data-ttu-id="937d8-108">hello Internet försäljning perspektiv som du skapar i den här lektionen undantar hello DimCustomer tabellobjekt.</span><span class="sxs-lookup"><span data-stu-id="937d8-108">hello Internet Sales perspective you create in this lesson excludes hello DimCustomer table object.</span></span> <span data-ttu-id="937d8-109">När du skapar ett perspektiv som undantar vissa objekt från vyn finns objektet fortfarande i hello modellen.</span><span class="sxs-lookup"><span data-stu-id="937d8-109">When you create a perspective that excludes certain objects from view, that object still exists in hello model.</span></span> <span data-ttu-id="937d8-110">Men det är inte synligt i fältlistan i rapporteringsklienten.</span><span class="sxs-lookup"><span data-stu-id="937d8-110">However, it is not visible in a reporting client field list.</span></span> <span data-ttu-id="937d8-111">Beräknade kolumner och mått, som antingen är inkluderade i ett perspektiv eller inte, kan fortfarande göra beräkningar från objektdata som exkluderas.</span><span class="sxs-lookup"><span data-stu-id="937d8-111">Calculated columns and measures either included in a perspective or not can still calculate from object data that is excluded.</span></span>  
  
<span data-ttu-id="937d8-112">hello syftet med den här lektionen är toodescribe hur toocreate perspektiv och bekanta dig med hello tabellmodell redigeringsverktyg.</span><span class="sxs-lookup"><span data-stu-id="937d8-112">hello purpose of this lesson is toodescribe how toocreate perspectives and become familiar with hello tabular model authoring tools.</span></span> <span data-ttu-id="937d8-113">Om du senare expanderar den här modellen tooinclude ytterligare tabeller kan du skapa ytterligare perspektiv toodefine olika synvinkel hello modell, till exempel inventerings- och försäljning.</span><span class="sxs-lookup"><span data-stu-id="937d8-113">If you later expand this model tooinclude additional tables, you can create additional perspectives toodefine different viewpoints of hello model, for example, Inventory and Sales.</span></span>  
  
<span data-ttu-id="937d8-114">Uppskattad tid toocomplete lektionen: **fem minuter**</span><span class="sxs-lookup"><span data-stu-id="937d8-114">Estimated time toocomplete this lesson: **Five minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="937d8-115">Krav</span><span class="sxs-lookup"><span data-stu-id="937d8-115">Prerequisites</span></span>  
<span data-ttu-id="937d8-116">Det här avsnittet ingår i självstudiekursen för tabellmodellering som bör slutföras i rätt ordning.</span><span class="sxs-lookup"><span data-stu-id="937d8-116">This topic is part of a tabular modeling tutorial, which should be completed in order.</span></span> <span data-ttu-id="937d8-117">Innan du utför hello uppgifter i den här lektionen bör du slutfört hello föregående lektionen: [lektionen 7: skapa prestationsindikatorer](../tutorials/aas-lesson-7-create-key-performance-indicators.md).</span><span class="sxs-lookup"><span data-stu-id="937d8-117">Before performing hello tasks in this lesson, you should have completed hello previous lesson: [Lesson 7: Create Key Performance Indicators](../tutorials/aas-lesson-7-create-key-performance-indicators.md).</span></span>  
  
## <a name="create-perspectives"></a><span data-ttu-id="937d8-118">Skapa perspektiv</span><span class="sxs-lookup"><span data-stu-id="937d8-118">Create perspectives</span></span>  
  
#### <a name="toocreate-an-internet-sales-perspective"></a><span data-ttu-id="937d8-119">toocreate ett Internet försäljning perspektiv</span><span class="sxs-lookup"><span data-stu-id="937d8-119">toocreate an Internet Sales perspective</span></span>  
  
1.  <span data-ttu-id="937d8-120">Klicka på hello **modellen** menyn > **perspektiv** > **skapa och hantera**.</span><span class="sxs-lookup"><span data-stu-id="937d8-120">Click hello **Model** menu > **Perspectives** > **Create and Manage**.</span></span>  
  
2.  <span data-ttu-id="937d8-121">I hello **perspektiv** dialogrutan klickar du på **nytt perspektiv**.</span><span class="sxs-lookup"><span data-stu-id="937d8-121">In hello **Perspectives** dialog box, click **New Perspective**.</span></span>  
  
3.  <span data-ttu-id="937d8-122">Dubbelklicka på hello **nytt perspektiv** kolumnrubriken och Byt sedan namn **Internet försäljning**.</span><span class="sxs-lookup"><span data-stu-id="937d8-122">Double-click hello **New Perspective** column heading, and then rename **Internet Sales**.</span></span>  
  
4.  <span data-ttu-id="937d8-123">Välj hello alla hello tabeller *utom* **DimCustomer**.</span><span class="sxs-lookup"><span data-stu-id="937d8-123">Select hello all hello tables *except* **DimCustomer**.</span></span>  
  
    ![aas-lesson8-perspectives](../tutorials/media/aas-lesson8-perspectives.png)
  
    <span data-ttu-id="937d8-125">I en senare lektionen använder du hello analysera i Excel funktionen tootest det här perspektivet.</span><span class="sxs-lookup"><span data-stu-id="937d8-125">In a later lesson, you use hello Analyze in Excel feature tootest this perspective.</span></span> <span data-ttu-id="937d8-126">hello Excel fältlistan för pivottabell innehåller varje tabell utom hello DimCustomer tabell.</span><span class="sxs-lookup"><span data-stu-id="937d8-126">hello Excel PivotTable Fields List includes each table except hello DimCustomer table.</span></span>  

## <a name="whats-next"></a><span data-ttu-id="937d8-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="937d8-127">What's next?</span></span>
<span data-ttu-id="937d8-128">[Lektion 9: Skapa hierarkier](../tutorials/aas-lesson-9-create-hierarchies.md).</span><span class="sxs-lookup"><span data-stu-id="937d8-128">[Lesson 9: Create hierarchies](../tutorials/aas-lesson-9-create-hierarchies.md).</span></span>
  
  
  
  
