---
title: "aaa ”Azure Analysis Services Adventure Works kursen | Microsoft Docs ”"
description: "Introducerar hello Adventure Works självstudiekurs för Azure Analysis Services"
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
ms.openlocfilehash: 2df8b3ab4e8c4ffbe0086418d60fd2e2abd35e7d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-analysis-services---adventure-works-tutorial"></a><span data-ttu-id="acb02-103">Azure Analysis Services – Självstudiekurs för Adventure Works</span><span class="sxs-lookup"><span data-stu-id="acb02-103">Azure Analysis Services - Adventure Works tutorial</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="acb02-104">Den här självstudiekursen innehåller erfarenheter toocreate och distribuera en tabellmodell på hello 1400 kompatibilitetsnivå med hjälp av [SQL Server Data Tools (SSDT)](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt).</span><span class="sxs-lookup"><span data-stu-id="acb02-104">This tutorial provides lessons on how toocreate and deploy a tabular model at hello 1400 compatibility level by using [SQL Server Data Tools (SSDT)](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt).</span></span>  

<span data-ttu-id="acb02-105">Om du är den nya tooAnalysis tjänster och tabular modellering kan den här kursen är hello snabbaste sättet toolearn hur toocreate och distribuera en grundläggande tabellmodell.</span><span class="sxs-lookup"><span data-stu-id="acb02-105">If you're new tooAnalysis Services and tabular modeling, completing this tutorial is hello quickest way toolearn how toocreate and deploy a basic tabular model.</span></span> <span data-ttu-id="acb02-106">När du har hello krav på plats bör ta mellan två toothree timmar toocomplete.</span><span class="sxs-lookup"><span data-stu-id="acb02-106">Once you have hello prerequisites in-place, it should take between two toothree hours toocomplete.</span></span>  
  
## <a name="what-you-learn"></a><span data-ttu-id="acb02-107">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="acb02-107">What you learn</span></span>   
  
-   <span data-ttu-id="acb02-108">Hur toocreate en ny tabellmodell projektet på hello **1400 kompatibilitetsnivå** i SSDT.</span><span class="sxs-lookup"><span data-stu-id="acb02-108">How toocreate a new tabular model project at hello **1400 compatibility level** in SSDT.</span></span>
  
-   <span data-ttu-id="acb02-109">Hur tooimport data från en relationsdatabas i en tabellmodell-projekt.</span><span class="sxs-lookup"><span data-stu-id="acb02-109">How tooimport data from a relational database into a tabular model project.</span></span>  
  
-   <span data-ttu-id="acb02-110">Hur toocreate och hantera relationer mellan tabeller i hello modellen.</span><span class="sxs-lookup"><span data-stu-id="acb02-110">How toocreate and manage relationships between tables in hello model.</span></span>  
  
-   <span data-ttu-id="acb02-111">Hur beräknas toocreate kolumner, mått och prestationsindikatorer som hjälper användarna att analysera viktiga affärsmått.</span><span class="sxs-lookup"><span data-stu-id="acb02-111">How toocreate calculated columns, measures, and Key Performance Indicators that help users analyze critical business metrics.</span></span>  
  
-   <span data-ttu-id="acb02-112">Hur toocreate och hantera perspektiv och hierarkier som hjälper användarna att enkelt bläddra modelldata genom att tillhandahålla business och programspecifika synpunkter.</span><span class="sxs-lookup"><span data-stu-id="acb02-112">How toocreate and manage perspectives and hierarchies that help users more easily browse model data by providing business and application-specific viewpoints.</span></span>  
  
-   <span data-ttu-id="acb02-113">Hur toocreate partitioner som delar upp tabelldata i mindre logiska delar som kan bearbetas oberoende av andra partitioner.</span><span class="sxs-lookup"><span data-stu-id="acb02-113">How toocreate partitions that divide table data into smaller logical parts that can be processed independent from other partitions.</span></span>  
  
-   <span data-ttu-id="acb02-114">Hur modellen toosecure objekt och data genom att skapa roller med användaren medlemmar.</span><span class="sxs-lookup"><span data-stu-id="acb02-114">How toosecure model objects and data by creating roles with user members.</span></span>  
  
-   <span data-ttu-id="acb02-115">Hur toodeploy en tabellmodell tooan **Azure Analysis Services** servern eller en lokal SQL Server 2017 Analysis Services-server.</span><span class="sxs-lookup"><span data-stu-id="acb02-115">How toodeploy a tabular model tooan **Azure Analysis Services** server or an on-premises SQL Server 2017 Analysis Services server.</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="acb02-116">Krav</span><span class="sxs-lookup"><span data-stu-id="acb02-116">Prerequisites</span></span>  
<span data-ttu-id="acb02-117">toocomplete den här kursen behöver du:</span><span class="sxs-lookup"><span data-stu-id="acb02-117">toocomplete this tutorial, you need:</span></span>  
  
-   <span data-ttu-id="acb02-118">En Azure Analysis Services eller SQL Server 2017 Analysis Services-instansen toodeploy din modell till.</span><span class="sxs-lookup"><span data-stu-id="acb02-118">An Azure Analysis Services or SQL Server 2017 Analysis Services instance toodeploy your model to.</span></span> <span data-ttu-id="acb02-119">Registrera dig för en kostnadsfri [utvärderingsversion av Azure Analysis Services](https://azure.microsoft.com/services/analysis-services/) och [skapa en server](../analysis-services-create-server.md).</span><span class="sxs-lookup"><span data-stu-id="acb02-119">Sign up for a free [Azure Analysis Services trial](https://azure.microsoft.com/services/analysis-services/) and [create a server](../analysis-services-create-server.md).</span></span> <span data-ttu-id="acb02-120">Eller registrera dig och ladda ned [SQL Server 2017 Community Technology Preview](https://www.microsoft.com/evalcenter/evaluate-sql-server-vnext-ctp).</span><span class="sxs-lookup"><span data-stu-id="acb02-120">Or, sign up and download [SQL Server 2017 Community Technology Preview](https://www.microsoft.com/evalcenter/evaluate-sql-server-vnext-ctp).</span></span> 

-   <span data-ttu-id="acb02-121">Ett informationslager för SQL Server eller Azure SQL Data Warehouse med hello [AdventureWorksDW2014 exempeldatabasen](http://go.microsoft.com/fwlink/?LinkID=335807).</span><span class="sxs-lookup"><span data-stu-id="acb02-121">A SQL Server Data Warehouse or Azure SQL Data Warehouse with hello [AdventureWorksDW2014 sample database](http://go.microsoft.com/fwlink/?LinkID=335807).</span></span> <span data-ttu-id="acb02-122">Den här exempeldatabasen innehåller hello data nödvändiga toocomplete den här kursen.</span><span class="sxs-lookup"><span data-stu-id="acb02-122">This sample database includes hello data necessary toocomplete this tutorial.</span></span> <span data-ttu-id="acb02-123">Ladda ned [kostnadsfria versioner av SQL Server Data Tools](https://www.microsoft.com/sql-server/sql-server-downloads).</span><span class="sxs-lookup"><span data-stu-id="acb02-123">Download [SQL Server free editions](https://www.microsoft.com/sql-server/sql-server-downloads).</span></span> <span data-ttu-id="acb02-124">Eller registrera dig för en kostnadsfri [Azure SQL Database-utvärderingsversion](https://azure.microsoft.com/services/sql-database/).</span><span class="sxs-lookup"><span data-stu-id="acb02-124">Or, sign up for a free [Azure SQL Database trial](https://azure.microsoft.com/services/sql-database/).</span></span> 

    <span data-ttu-id="acb02-125">**Viktigt:** om du installerar hello exempeldatabasen på en lokal SQL Server och du distribuerar din modell tooan Azure Analysis Services-server, en [lokala datagateway](../analysis-services-gateway.md) krävs.</span><span class="sxs-lookup"><span data-stu-id="acb02-125">**Important:** If you install hello sample database on an on-premises SQL Server, and you deploy your model tooan Azure Analysis Services server, an [On-premises data gateway](../analysis-services-gateway.md) is required.</span></span>

-   <span data-ttu-id="acb02-126">hello senaste versionen av [SQL Server Data Tools (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx).</span><span class="sxs-lookup"><span data-stu-id="acb02-126">hello latest version of [SQL Server Data Tools (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx).</span></span>

-   <span data-ttu-id="acb02-127">hello senaste versionen av [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).</span><span class="sxs-lookup"><span data-stu-id="acb02-127">hello latest version of [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).</span></span>    

-   <span data-ttu-id="acb02-128">Ett klientprogram som till exempel [Power BI Desktop](https://powerbi.microsoft.com/desktop/) eller Excel.</span><span class="sxs-lookup"><span data-stu-id="acb02-128">A client application such as [Power BI Desktop](https://powerbi.microsoft.com/desktop/) or Excel.</span></span> 

## <a name="scenario"></a><span data-ttu-id="acb02-129">Scenario</span><span class="sxs-lookup"><span data-stu-id="acb02-129">Scenario</span></span>  
<span data-ttu-id="acb02-130">Den här självstudien är baserad på Adventure Works Cycles, ett fiktivt företag.</span><span class="sxs-lookup"><span data-stu-id="acb02-130">This tutorial is based on Adventure Works Cycles, a fictitious company.</span></span> <span data-ttu-id="acb02-131">Adventure Works är en stor, flerspråkig tillverkningsföretag som producerar och distribuerar cyklar, delar och tillbehör toocommercial marknader i Nordamerika, Europa och Asien.</span><span class="sxs-lookup"><span data-stu-id="acb02-131">Adventure Works is a large, multinational manufacturing company that produces and distributes bicycles, parts, and accessories toocommercial markets in North America, Europe, and Asia.</span></span> <span data-ttu-id="acb02-132">hello företaget använder 500 anställda.</span><span class="sxs-lookup"><span data-stu-id="acb02-132">hello company employs 500 workers.</span></span> <span data-ttu-id="acb02-133">Dessutom har Adventure Works flera regionala försäljningsteam inom sitt marknadsområde.</span><span class="sxs-lookup"><span data-stu-id="acb02-133">Additionally, Adventure Works employs several regional sales teams throughout its market base.</span></span> <span data-ttu-id="acb02-134">Projektet är toocreate en tabellmodell för försäljning och marknadsföring användare tooanalyze Internet försäljningsinformation i hello AdventureWorksDW-databasen.</span><span class="sxs-lookup"><span data-stu-id="acb02-134">Your project is toocreate a tabular model for sales and marketing users tooanalyze Internet sales data in hello AdventureWorksDW database.</span></span>  
  
<span data-ttu-id="acb02-135">Du måste slutföra olika erfarenheter toocomplete hello kursen.</span><span class="sxs-lookup"><span data-stu-id="acb02-135">toocomplete hello tutorial, you must complete various lessons.</span></span> <span data-ttu-id="acb02-136">Varje lektion innehåller olika uppgifter.</span><span class="sxs-lookup"><span data-stu-id="acb02-136">In each lesson, there are tasks.</span></span> <span data-ttu-id="acb02-137">Varje uppgift i ordning är nödvändigt för att slutföra hello lektionen.</span><span class="sxs-lookup"><span data-stu-id="acb02-137">Completing each task in order is necessary for completing hello lesson.</span></span> <span data-ttu-id="acb02-138">En viss lektion kan innehålla flera uppgifter som leder till samma resultat, men tillvägagångssättet för att slutföra uppgifterna skiljer sig åt.</span><span class="sxs-lookup"><span data-stu-id="acb02-138">While in a particular lesson there may be several tasks that accomplish a similar outcome, but how you complete each task is slightly different.</span></span> <span data-ttu-id="acb02-139">Den här metoden visar är det ofta mer än ett sätt toocomplete en aktivitet och toochallenge du med hjälp av kunskaper som du har lärt dig i tidigare erfarenheter och uppgifter.</span><span class="sxs-lookup"><span data-stu-id="acb02-139">This method shows there is often more than one way toocomplete a task, and toochallenge you by using skills you've learned in previous lessons and tasks.</span></span>  
  
<span data-ttu-id="acb02-140">hello syftar hello erfarenheter tooguide dig genom att redigera en grundläggande tabellmodell genom att använda många av funktionerna hello ingår i SSDT.</span><span class="sxs-lookup"><span data-stu-id="acb02-140">hello purpose of hello lessons is tooguide you through authoring a basic tabular model by using many of hello features included in SSDT.</span></span> <span data-ttu-id="acb02-141">Eftersom varje lektionen bygger på hello föregående lektionen, bör du genomföra hello erfarenheter i ordning.</span><span class="sxs-lookup"><span data-stu-id="acb02-141">Because each lesson builds upon hello previous lesson, you should complete hello lessons in order.</span></span>
  
<span data-ttu-id="acb02-142">Den här kursen ger inte några erfarenheter om att hantera en server i Azure-portalen, hantera en server eller databas med hjälp av SSMS eller med hjälp av en klient programdata toobrowse modell.</span><span class="sxs-lookup"><span data-stu-id="acb02-142">This tutorial does not provide lessons about managing a server in Azure portal, managing a server or database by using SSMS, or using a client application toobrowse model data.</span></span> 


## <a name="lessons"></a><span data-ttu-id="acb02-143">Lektioner</span><span class="sxs-lookup"><span data-stu-id="acb02-143">Lessons</span></span>  
<span data-ttu-id="acb02-144">Vägledningen innehåller följande erfarenheter hello:</span><span class="sxs-lookup"><span data-stu-id="acb02-144">This tutorial includes hello following lessons:</span></span>  
  
|<span data-ttu-id="acb02-145">Lektion</span><span class="sxs-lookup"><span data-stu-id="acb02-145">Lesson</span></span>|<span data-ttu-id="acb02-146">Beräknad tid toocomplete</span><span class="sxs-lookup"><span data-stu-id="acb02-146">Estimated time toocomplete</span></span>|  
|----------|------------------------------|  
|[<span data-ttu-id="acb02-147">1 - Skapa ett nytt projekt för tabellmodeller</span><span class="sxs-lookup"><span data-stu-id="acb02-147">1 - Create a new tabular model project</span></span>](../tutorials/aas-lesson-1-create-a-new-tabular-model-project.md)|<span data-ttu-id="acb02-148">10 minuter</span><span class="sxs-lookup"><span data-stu-id="acb02-148">10 minutes</span></span>|  
|[<span data-ttu-id="acb02-149">2 - Hämta data</span><span class="sxs-lookup"><span data-stu-id="acb02-149">2 - Get data</span></span>](../tutorials/aas-lesson-2-get-data.md)|<span data-ttu-id="acb02-150">10 minuter</span><span class="sxs-lookup"><span data-stu-id="acb02-150">10 minutes</span></span>|  
|[<span data-ttu-id="acb02-151">3 - Markera som datumtabell</span><span class="sxs-lookup"><span data-stu-id="acb02-151">3 - Mark as Date Table</span></span>](../tutorials/aas-lesson-3-mark-as-date-table.md)|<span data-ttu-id="acb02-152">3 minuter</span><span class="sxs-lookup"><span data-stu-id="acb02-152">3 minutes</span></span>|  
|[<span data-ttu-id="acb02-153">4 - Skapa relationer</span><span class="sxs-lookup"><span data-stu-id="acb02-153">4 - Create relationships</span></span>](../tutorials/aas-lesson-4-create-relationships.md)|<span data-ttu-id="acb02-154">10 minuter</span><span class="sxs-lookup"><span data-stu-id="acb02-154">10 minutes</span></span>|  
|[<span data-ttu-id="acb02-155">5 - Skapa beräknade kolumner</span><span class="sxs-lookup"><span data-stu-id="acb02-155">5 - Create calculated columns</span></span>](../tutorials/aas-lesson-5-create-calculated-columns.md)|<span data-ttu-id="acb02-156">15 minuter</span><span class="sxs-lookup"><span data-stu-id="acb02-156">15 minutes</span></span>|
|[<span data-ttu-id="acb02-157">6 - Skapa mått</span><span class="sxs-lookup"><span data-stu-id="acb02-157">6 - Create measures</span></span>](../tutorials/aas-lesson-6-create-measures.md)|<span data-ttu-id="acb02-158">30 minuter</span><span class="sxs-lookup"><span data-stu-id="acb02-158">30 minutes</span></span>|  
|[<span data-ttu-id="acb02-159">7 - Skapa KPI:er</span><span class="sxs-lookup"><span data-stu-id="acb02-159">7 - Create Key Performance Indicators (KPI)</span></span>](../tutorials/aas-lesson-7-create-key-performance-indicators.md)|<span data-ttu-id="acb02-160">15 minuter</span><span class="sxs-lookup"><span data-stu-id="acb02-160">15 minutes</span></span>|  
|[<span data-ttu-id="acb02-161">8 - Skapa perspektiv</span><span class="sxs-lookup"><span data-stu-id="acb02-161">8 - Create perspectives</span></span>](../tutorials/aas-lesson-8-create-perspectives.md)|<span data-ttu-id="acb02-162">5 minuter</span><span class="sxs-lookup"><span data-stu-id="acb02-162">5 minutes</span></span>|  
|[<span data-ttu-id="acb02-163">9 - Skapa hierarkier</span><span class="sxs-lookup"><span data-stu-id="acb02-163">9 - Create hierarchies</span></span>](../tutorials/aas-lesson-9-create-hierarchies.md)|<span data-ttu-id="acb02-164">20 minuter</span><span class="sxs-lookup"><span data-stu-id="acb02-164">20 minutes</span></span>|  
|[<span data-ttu-id="acb02-165">10 - Skapa partitioner</span><span class="sxs-lookup"><span data-stu-id="acb02-165">10 - Create partitions</span></span>](../tutorials/aas-lesson-10-create-partitions.md)|<span data-ttu-id="acb02-166">15 minuter</span><span class="sxs-lookup"><span data-stu-id="acb02-166">15 minutes</span></span>|  
|[<span data-ttu-id="acb02-167">11 - Skapa roller</span><span class="sxs-lookup"><span data-stu-id="acb02-167">11 - Create roles</span></span>](../tutorials/aas-lesson-11-create-roles.md)|<span data-ttu-id="acb02-168">15 minuter</span><span class="sxs-lookup"><span data-stu-id="acb02-168">15 minutes</span></span>|  
|[<span data-ttu-id="acb02-169">12 - Analysera i Excel</span><span class="sxs-lookup"><span data-stu-id="acb02-169">12 - Analyze in Excel</span></span>](../tutorials/aas-lesson-12-analyze-in-excel.md)|<span data-ttu-id="acb02-170">5 minuter</span><span class="sxs-lookup"><span data-stu-id="acb02-170">5 minutes</span></span>| 
|[<span data-ttu-id="acb02-171">13 - Distribuera</span><span class="sxs-lookup"><span data-stu-id="acb02-171">13 - Deploy</span></span>](../tutorials/aas-lesson-13-deploy.md)|<span data-ttu-id="acb02-172">5 minuter</span><span class="sxs-lookup"><span data-stu-id="acb02-172">5 minutes</span></span>|  
  
## <a name="supplemental-lessons"></a><span data-ttu-id="acb02-173">Kompletterande lektioner</span><span class="sxs-lookup"><span data-stu-id="acb02-173">Supplemental lessons</span></span>  
<span data-ttu-id="acb02-174">Dessa erfarenheter är inte obligatoriska toocomplete hello kursen, men kan vara användbart i bättre förstå avancerade tabellmodell funktioner för redigering.</span><span class="sxs-lookup"><span data-stu-id="acb02-174">These lessons are not required toocomplete hello tutorial, but can be helpful in better understanding advanced tabular model authoring features.</span></span>  
  
|<span data-ttu-id="acb02-175">Lektion</span><span class="sxs-lookup"><span data-stu-id="acb02-175">Lesson</span></span>|<span data-ttu-id="acb02-176">Beräknad tid toocomplete</span><span class="sxs-lookup"><span data-stu-id="acb02-176">Estimated time toocomplete</span></span>|  
|----------|------------------------------|  
|[<span data-ttu-id="acb02-177">Detaljrader</span><span class="sxs-lookup"><span data-stu-id="acb02-177">Detail Rows</span></span>](../tutorials/aas-supplemental-lesson-detail-rows.md)|<span data-ttu-id="acb02-178">10 minuter</span><span class="sxs-lookup"><span data-stu-id="acb02-178">10 minutes</span></span>|
|[<span data-ttu-id="acb02-179">Dynamisk säkerhet</span><span class="sxs-lookup"><span data-stu-id="acb02-179">Dynamic security</span></span>](../tutorials/aas-supplemental-lesson-dynamic-security.md)|<span data-ttu-id="acb02-180">30 minuter</span><span class="sxs-lookup"><span data-stu-id="acb02-180">30 minutes</span></span>|
|[<span data-ttu-id="acb02-181">Ojämna hierarkier</span><span class="sxs-lookup"><span data-stu-id="acb02-181">Ragged hierarchies</span></span>](../tutorials/aas-supplemental-lesson-ragged-hierarchies.md)|<span data-ttu-id="acb02-182">20 minuter</span><span class="sxs-lookup"><span data-stu-id="acb02-182">20 minutes</span></span>| 

  
## <a name="next-steps"></a><span data-ttu-id="acb02-183">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="acb02-183">Next steps</span></span>  
<span data-ttu-id="acb02-184">tooget igång, se [lektionen 1: skapa en ny modell Tabellprojekt](../tutorials/aas-lesson-1-create-a-new-tabular-model-project.md).</span><span class="sxs-lookup"><span data-stu-id="acb02-184">tooget started, see [Lesson 1: Create a New Tabular Model Project](../tutorials/aas-lesson-1-create-a-new-tabular-model-project.md).</span></span>  
  
  
  

