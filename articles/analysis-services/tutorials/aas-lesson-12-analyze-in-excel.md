---
title: "Azure Analysis Services självstudiekurs 12: Analysera i Excel | Microsoft Docs"
description: "Beskriver hur du använder funktionen Analysera i Excel i Azure Analysis Services-självstudieprojektet."
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
ms.openlocfilehash: 6f47de43ff8d94de22f8b7c12fa0707a8d7ffbbc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="lesson-12-analyze-in-excel"></a><span data-ttu-id="f54d6-103">Lektion 12: Analysera i Excel</span><span class="sxs-lookup"><span data-stu-id="f54d6-103">Lesson 12: Analyze in Excel</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="f54d6-104">Under den här lektionen använder du funktionen Analysera för att öppna Microsoft Excel, automatiskt skapa en anslutning till modellens arbetsyta och automatiskt lägga till en pivottabell i kalkylbladet.</span><span class="sxs-lookup"><span data-stu-id="f54d6-104">In this lesson, you use the Analyze in Excel feature to open Microsoft Excel, automatically create a connection to the model workspace, and automatically add a PivotTable to the worksheet.</span></span> <span data-ttu-id="f54d6-105">Funktionen Analysera i Excel är avsedd att tillhandahålla ett snabbt och enkelt sätt att testa effektiviteten av modelldesignen innan du distribuerar modellen.</span><span class="sxs-lookup"><span data-stu-id="f54d6-105">The Analyze in Excel feature is meant to provide a quick and easy way to test the efficacy of your model design prior to deploying your model.</span></span> <span data-ttu-id="f54d6-106">Du utför inte någon dataanalys under den här lektionen.</span><span class="sxs-lookup"><span data-stu-id="f54d6-106">You do not perform any data analysis in this lesson.</span></span> <span data-ttu-id="f54d6-107">Syftet med den här lektionen är du, modellskaparen, ska kunna bekanta dig med de verktyg du kan använda för att testa modelldesignen.</span><span class="sxs-lookup"><span data-stu-id="f54d6-107">The purpose of this lesson is to familiarize you, the model author, with the tools you can use to test your model design.</span></span>   
  
<span data-ttu-id="f54d6-108">För att slutföra den här lektionen måste Excel vara installerat på samma dator som SSDT.</span><span class="sxs-lookup"><span data-stu-id="f54d6-108">To complete this lesson, Excel must be installed on the same computer as SSDT.</span></span>
  
<span data-ttu-id="f54d6-109">Uppskattad tidsåtgång för den här lektionen: **Fem minuter**</span><span class="sxs-lookup"><span data-stu-id="f54d6-109">Estimated time to complete this lesson: **Five minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="f54d6-110">Krav</span><span class="sxs-lookup"><span data-stu-id="f54d6-110">Prerequisites</span></span>  
<span data-ttu-id="f54d6-111">Det här avsnittet ingår i självstudiekursen för tabellmodellering som bör slutföras i rätt ordning.</span><span class="sxs-lookup"><span data-stu-id="f54d6-111">This topic is part of a tabular modeling tutorial, which should be completed in order.</span></span> <span data-ttu-id="f54d6-112">Innan du utför uppgifterna under den här lektionen bör du ha slutfört föregående lektion: [Lektion 11: Skapa roller](../tutorials/aas-lesson-11-create-roles.md).</span><span class="sxs-lookup"><span data-stu-id="f54d6-112">Before performing the tasks in this lesson, you should have completed the previous lesson: [Lesson 11: Create roles](../tutorials/aas-lesson-11-create-roles.md).</span></span>  
  
## <a name="browse-using-the-default-and-internet-sales-perspectives"></a><span data-ttu-id="f54d6-113">Bläddra med hjälp av standard- och Internet-försäljningsperspektiven</span><span class="sxs-lookup"><span data-stu-id="f54d6-113">Browse using the Default and Internet Sales perspectives</span></span>  
<span data-ttu-id="f54d6-114">I de första uppgifterna bläddrar du till modellen genom att använda både standardperspektivet, som omfattar alla modellobjekt, och genom att använda Internet-försäljningsperspektivet.</span><span class="sxs-lookup"><span data-stu-id="f54d6-114">In these first tasks, you browse your model by using both the default perspective, which includes all model objects, and also by using the Internet Sales perspective you earlier.</span></span> <span data-ttu-id="f54d6-115">Internet-försäljningsperspektivet exkluderar kundtabellobjekt.</span><span class="sxs-lookup"><span data-stu-id="f54d6-115">The Internet Sales perspective excludes the Customer table object.</span></span>  
  
#### <a name="to-browse-by-using-the-default-perspective"></a><span data-ttu-id="f54d6-116">Så här bläddrar du med hjälp av standardperspektivet</span><span class="sxs-lookup"><span data-stu-id="f54d6-116">To browse by using the Default perspective</span></span>  
  
1.  <span data-ttu-id="f54d6-117">Klicka på menyn **Modell** > **Analysera i Excel**.</span><span class="sxs-lookup"><span data-stu-id="f54d6-117">Click the **Model** menu > **Analyze in Excel**.</span></span>  
  
2.  <span data-ttu-id="f54d6-118">I dialogrutan **Analysera i Excel** klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="f54d6-118">In the **Analyze in Excel** dialog box, click **OK**.</span></span>  
  
    <span data-ttu-id="f54d6-119">Excel öppnas med en ny arbetsbok.</span><span class="sxs-lookup"><span data-stu-id="f54d6-119">Excel opens with a new workbook.</span></span> <span data-ttu-id="f54d6-120">En anslutning till en datakälla skapas med det aktuella användarkontot och standardperspektivet används för att definiera fält som visas.</span><span class="sxs-lookup"><span data-stu-id="f54d6-120">A data source connection is created using the current user account and the Default perspective is used to define viewable fields.</span></span> <span data-ttu-id="f54d6-121">En pivottabell läggs automatiskt till i kalkylbladet.</span><span class="sxs-lookup"><span data-stu-id="f54d6-121">A PivotTable is automatically added to the worksheet.</span></span>  
  
3.  <span data-ttu-id="f54d6-122">I **fältlistan PivotTable** i Excel ser du hur måttgrupperna **DimDate** och **FactInternetSales** visas.</span><span class="sxs-lookup"><span data-stu-id="f54d6-122">In Excel, in the **PivotTable Field List**, notice the **DimDate** and **FactInternetSales** measure groups appear.</span></span> <span data-ttu-id="f54d6-123">Tabellerna **DimCustomer**, **DimDate**, **DimGeography**, **DimProduct**, **DimProductCategory**, **DimProductSubcategory** och **FactInternetSales** samt deras respektive kolumner visas också.</span><span class="sxs-lookup"><span data-stu-id="f54d6-123">The **DimCustomer**, **DimDate**, **DimGeography**, **DimProduct**, **DimProductCategory**, **DimProductSubcategory**, and **FactInternetSales** tables with their respective columns also appear.</span></span>  
  
4.  <span data-ttu-id="f54d6-124">Stäng Excel utan att spara arbetsboken.</span><span class="sxs-lookup"><span data-stu-id="f54d6-124">Close Excel without saving the workbook.</span></span>  
  
#### <a name="to-browse-by-using-the-internet-sales-perspective"></a><span data-ttu-id="f54d6-125">Så här bläddrar du med hjälp av Internet-försäljningsperspektivet</span><span class="sxs-lookup"><span data-stu-id="f54d6-125">To browse by using the Internet Sales perspective</span></span>  
  
1.  <span data-ttu-id="f54d6-126">Klicka på menyn **Modell** och klicka sedan på **Analysera i Excel**.</span><span class="sxs-lookup"><span data-stu-id="f54d6-126">Click the **Model** menu, and then click **Analyze in Excel**.</span></span>  
  
2.  <span data-ttu-id="f54d6-127">I dialogrutan **Analysera i Excel** behåller du **Aktuell Windows-användare** markerat, sedan väljer du **Internetförsäljning** i den nedrullningsbara listrutan **Perspektiv** och klickar på **OK**.</span><span class="sxs-lookup"><span data-stu-id="f54d6-127">In the **Analyze in Excel** dialog box, leave **Current Windows User** selected, then in the **Perspective** drop-down listbox, select **Internet Sales**, and then click **OK**.</span></span> 
    
    ![aas-lesson12-perspective](../tutorials/media/aas-lesson12-perspective.png)
    
3.  <span data-ttu-id="f54d6-129">Observera att tabellen DimCustomer är exkluderad från fältlistan i **Pivottabellfält** i Excel.</span><span class="sxs-lookup"><span data-stu-id="f54d6-129">In Excel, in **PivotTable Fields**, notice the DimCustomer table is excluded from the field list.</span></span>  
    
    ![aas-lesson12-fields](../tutorials/media/aas-lesson12-fields.png)
    
4.  <span data-ttu-id="f54d6-131">Stäng Excel utan att spara arbetsboken.</span><span class="sxs-lookup"><span data-stu-id="f54d6-131">Close Excel without saving the workbook.</span></span>  
  
## <a name="browse-by-using-roles"></a><span data-ttu-id="f54d6-132">Bläddra med hjälp av roller</span><span class="sxs-lookup"><span data-stu-id="f54d6-132">Browse by using roles</span></span>  
<span data-ttu-id="f54d6-133">Roller är en viktig del av alla tabellmodeller.</span><span class="sxs-lookup"><span data-stu-id="f54d6-133">Roles are an important part of any tabular model.</span></span> <span data-ttu-id="f54d6-134">Utan minst en roll där användarna läggs till som medlemmar kan användarna inte komma åt och analysera data med hjälp av modellen.</span><span class="sxs-lookup"><span data-stu-id="f54d6-134">Without at least one role to which users are added as members, users cannot access and analyze data using your model.</span></span> <span data-ttu-id="f54d6-135">Funktionen Analysera i Excel ger dig ett sätt att testa de roller som du har definierat.</span><span class="sxs-lookup"><span data-stu-id="f54d6-135">The Analyze in Excel feature provides a way for you to test the roles you have defined.</span></span>  
  
#### <a name="to-browse-by-using-the-sales-manager-user-role"></a><span data-ttu-id="f54d6-136">Bläddra med hjälp av användarrollen Säljchef</span><span class="sxs-lookup"><span data-stu-id="f54d6-136">To browse by using the Sales Manager user role</span></span>  
  
1.  <span data-ttu-id="f54d6-137">Klicka på menyn **Modell** i SSDT och klicka sedan på **Analysera i Excel**.</span><span class="sxs-lookup"><span data-stu-id="f54d6-137">In SSDT, click the **Model** menu, and then click **Analyze in Excel**.</span></span>  
  
2.  <span data-ttu-id="f54d6-138">I **Ange det användarnamn eller den roll som ska användas för att ansluta till modellen** väljer du **Roll**, i den nedrullningsbara listrutan väljer du **Säljchef** och klickar sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="f54d6-138">In **Specify the user name or role to use to connect to the model**, select **Role**, and then in the drop-down listbox, select **Sales Manager**, and then click **OK**.</span></span>  
  
    <span data-ttu-id="f54d6-139">Excel öppnas med en ny arbetsbok.</span><span class="sxs-lookup"><span data-stu-id="f54d6-139">Excel opens with a new workbook.</span></span> <span data-ttu-id="f54d6-140">En pivottabell skapas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="f54d6-140">A PivotTable is automatically created.</span></span> <span data-ttu-id="f54d6-141">Fältlistan för pivottabellen innehåller alla datafält som är tillgängliga i den nya modellen.</span><span class="sxs-lookup"><span data-stu-id="f54d6-141">The Pivot Table Field List includes all the data fields available in your new model.</span></span>  
      
3.  <span data-ttu-id="f54d6-142">Stäng Excel utan att spara arbetsboken.</span><span class="sxs-lookup"><span data-stu-id="f54d6-142">Close Excel without saving the workbook.</span></span>  
  
## <a name="whats-next"></a><span data-ttu-id="f54d6-143">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f54d6-143">What's next?</span></span>
<span data-ttu-id="f54d6-144">Gå till nästa lektion: [Lektion 13: Distribuera](../tutorials/aas-lesson-13-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="f54d6-144">Go to the next lesson: [Lesson 13: Deploy](../tutorials/aas-lesson-13-deploy.md).</span></span>

  
  
  
