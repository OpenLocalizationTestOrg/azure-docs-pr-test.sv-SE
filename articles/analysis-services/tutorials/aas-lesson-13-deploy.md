---
title: "Azure Analysis Services självstudiekurs 13: Distribuera | Microsoft Docs"
description: "Beskriver hur du distribuerar självstudieprojektet i Azure Analysis Services."
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
ms.date: 07/17/2017
ms.author: owend
ms.openlocfilehash: 70dbf5786262f75199270aa8009e03b9b48b8559
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="lesson-13-deploy"></a><span data-ttu-id="25d6a-103">Lektion 13: Distribuera</span><span class="sxs-lookup"><span data-stu-id="25d6a-103">Lesson 13: Deploy</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="25d6a-104">Under den här lektionen konfigurerar du distributionsegenskaper, anger en Analysis Services-server i Azure att distribuera till och ett namn för modellen.</span><span class="sxs-lookup"><span data-stu-id="25d6a-104">In this lesson, you configure deployment properties; specifying an Azure Analysis Services server to deploy to and a name for the model.</span></span> <span data-ttu-id="25d6a-105">Distribuera sedan modellen till den instansen.</span><span class="sxs-lookup"><span data-stu-id="25d6a-105">You then deploy the model to that instance.</span></span> <span data-ttu-id="25d6a-106">När modellen har distribuerats kan användarna ansluta till den med hjälp av ett rapporteringsklientprogram.</span><span class="sxs-lookup"><span data-stu-id="25d6a-106">After your model is deployed, users can connect to it by using a reporting client application.</span></span> <span data-ttu-id="25d6a-107">Läs mer i [Distribuera till Azure Analysis Services](https://docs.microsoft.com/azure/analysis-services/analysis-services-deploy).</span><span class="sxs-lookup"><span data-stu-id="25d6a-107">To learn more, see [Deploy to Azure Analysis Services](https://docs.microsoft.com/azure/analysis-services/analysis-services-deploy).</span></span>  
  
<span data-ttu-id="25d6a-108">Uppskattad tidsåtgång för den här lektionen: **5 minuter**</span><span class="sxs-lookup"><span data-stu-id="25d6a-108">Estimated time to complete this lesson: **5 minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="25d6a-109">Krav</span><span class="sxs-lookup"><span data-stu-id="25d6a-109">Prerequisites</span></span>  
<span data-ttu-id="25d6a-110">Det här avsnittet ingår i självstudiekursen för tabellmodellering som bör slutföras i rätt ordning.</span><span class="sxs-lookup"><span data-stu-id="25d6a-110">This topic is part of a tabular modeling tutorial, which should be completed in order.</span></span> <span data-ttu-id="25d6a-111">Innan du utför uppgifterna under den här lektionen bör du ha slutfört föregående lektion: [Lektion 12: Analysera i Excel](../tutorials/aas-lesson-12-analyze-in-excel.md).</span><span class="sxs-lookup"><span data-stu-id="25d6a-111">Before performing the tasks in this lesson, you should have completed the previous lesson: [Lesson 12: Analyze in Excel](../tutorials/aas-lesson-12-analyze-in-excel.md).</span></span>  

> [!IMPORTANT]  
> <span data-ttu-id="25d6a-112">Du måste ha [administratörsbehörighet](../analysis-services-server-admins.md) på den fjärranslutna Analysis Services-servern för att distribuera till den.</span><span class="sxs-lookup"><span data-stu-id="25d6a-112">You must have [Administrator permissions](../analysis-services-server-admins.md) on the remote Analysis Services server in-order to deploy to it.</span></span>  

> [!IMPORTANT]  
> <span data-ttu-id="25d6a-113">Om du har installerat exempeldatabasen AdventureWorksDW2014 på en lokal SQL Server och du distribuerar din modell till en Azure Analysis Services-server så krävs en [Lokal datagateway](../analysis-services-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="25d6a-113">If you installed the AdventureWorksDW2014 sample database on an on-premises SQL Server, and you're deploying your model to an Azure Analysis Services server, an [On-premises data gateway](../analysis-services-gateway.md) is required.</span></span>
  
## <a name="deploy-the-model"></a><span data-ttu-id="25d6a-114">Distribuera modellen</span><span class="sxs-lookup"><span data-stu-id="25d6a-114">Deploy the model</span></span>  
  
#### <a name="to-configure-deployment-properties"></a><span data-ttu-id="25d6a-115">Så här konfigurerar du distributionsegenskaper</span><span class="sxs-lookup"><span data-stu-id="25d6a-115">To configure deployment properties</span></span>  

  
1.  <span data-ttu-id="25d6a-116">Högerklicka på projektet **AW Internet Sales** i **Solution Explorer** och sedan på **Egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="25d6a-116">In **Solution Explorer**, right-click the **AW Internet Sales** project, and then click **Properties**.</span></span>  
  
2.  <span data-ttu-id="25d6a-117">I dialogrutan **AW Internet Sales Property Pages** (AW Internet Sales egenskapssidor) under **Distributionsserver** i egenskapen **Server** anger du namnet servern.</span><span class="sxs-lookup"><span data-stu-id="25d6a-117">In the **AW Internet Sales Property Pages** dialog box, under **Deployment Server**, in the **Server** property, enter the full server.</span></span>  

    ![aas-lesson13-deploy-property](../tutorials/media/aas-lesson13-deploy-property.png)
  
3.  <span data-ttu-id="25d6a-119">I egenskapen **Databas** anger du **Adventure Works Internet Sales**.</span><span class="sxs-lookup"><span data-stu-id="25d6a-119">In the **Database** property, type **Adventure Works Internet Sales**.</span></span>  
  
4.  <span data-ttu-id="25d6a-120">I egenskapen **Modellnamn** anger du **Adventure Works Internet Sales Model**.</span><span class="sxs-lookup"><span data-stu-id="25d6a-120">In the **Model Name** property, type **Adventure Works Internet Sales Model**.</span></span>  
  
5.  <span data-ttu-id="25d6a-121">Kontrollera dina val och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="25d6a-121">Verify your selections and then click **OK**.</span></span>  
  
#### <a name="to-deploy-the-adventure-works-internet-sales"></a><span data-ttu-id="25d6a-122">Så här distribuerar du Adventure Works Internet Sales</span><span class="sxs-lookup"><span data-stu-id="25d6a-122">To deploy the Adventure Works Internet Sales</span></span>
  
1.  <span data-ttu-id="25d6a-123">Högerklicka på projektet **AW Internet Sales** i **Solution Explorer** > **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="25d6a-123">In **Solution Explorer**, right-click the **AW Internet Sales** project > **Build**.</span></span>  

2.  <span data-ttu-id="25d6a-124">Högerklicka på projektet **AW Internet Sales** > **Distribuera**.</span><span class="sxs-lookup"><span data-stu-id="25d6a-124">Right-click the **AW Internet Sales** project > **Deploy**.</span></span>

    <span data-ttu-id="25d6a-125">När du distribuerar till Azure Analysis Services kan du uppmanas att ange ditt konto.</span><span class="sxs-lookup"><span data-stu-id="25d6a-125">When deploying to Azure Analysis Services, you may be prompted to enter your account.</span></span> <span data-ttu-id="25d6a-126">Ange ditt organisationskonto och lösenord, till exempel nancy@adventureworks.com. Det här kontot måste finnas i Administratörer på servern.</span><span class="sxs-lookup"><span data-stu-id="25d6a-126">Enter your organizational account and password, for example nancy@adventureworks.com. This account must be in Admins on the server.</span></span>
  
    <span data-ttu-id="25d6a-127">Dialogrutan Distribuera visas och visar distributionsstatus för metadata och varje tabell som ingår i modellen.</span><span class="sxs-lookup"><span data-stu-id="25d6a-127">The Deploy dialog box appears and displays the deployment status of the metadata and each table included in the model.</span></span>  
    
    ![aas-lesson13-deploy-status](../tutorials/media/aas-lesson13-deploy-status.png)
  
3. <span data-ttu-id="25d6a-129">När distributionen är klar kan du klicka på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="25d6a-129">When deployment successfully completes, go ahead and click **Close**.</span></span>  
  
## <a name="conclusion"></a><span data-ttu-id="25d6a-130">Slutsats</span><span class="sxs-lookup"><span data-stu-id="25d6a-130">Conclusion</span></span>  
<span data-ttu-id="25d6a-131">Grattis!</span><span class="sxs-lookup"><span data-stu-id="25d6a-131">Congratulations!</span></span> <span data-ttu-id="25d6a-132">Du är färdig med att redigera och distribuera din första Analysis Services Tabular-modell.</span><span class="sxs-lookup"><span data-stu-id="25d6a-132">You're finished authoring and deploying your first Analysis Services Tabular model.</span></span> <span data-ttu-id="25d6a-133">Den här självstudien har hjälpt dig att slutföra de vanligaste uppgifterna för att skapa en tabellmodell.</span><span class="sxs-lookup"><span data-stu-id="25d6a-133">This tutorial has helped guide you through completing the most common tasks in creating a tabular model.</span></span> <span data-ttu-id="25d6a-134">Nu när Adventure Works Internet Sales-modellen har distribuerats kan du använda SQL Server Management Studio för att hantera modellen, skapa processkript och en säkerhetskopieringsplan.</span><span class="sxs-lookup"><span data-stu-id="25d6a-134">Now that your Adventure Works Internet Sales model is deployed, you can use SQL Server Management Studio to manage the model; create process scripts and a backup plan.</span></span> <span data-ttu-id="25d6a-135">Användare kan nu även ansluta till modellen med ett rapportklientprogram, till exempel Microsoft Excel eller Power BI.</span><span class="sxs-lookup"><span data-stu-id="25d6a-135">Users can also now connect to the model using a reporting client application such as Microsoft Excel or Power BI.</span></span>  

![aas-lesson13-ssms](../tutorials/media/aas-lesson13-ssms.png)
  
  
  
## <a name="whats-next"></a><span data-ttu-id="25d6a-137">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="25d6a-137">What's next?</span></span>
<span data-ttu-id="25d6a-138">[Anslut med Power BI Desktop](../analysis-services-connect-pbi.md) </span><span class="sxs-lookup"><span data-stu-id="25d6a-138">[Connect with Power BI Desktop](../analysis-services-connect-pbi.md) </span></span>  
<span data-ttu-id="25d6a-139">[Kompletterande lektion – Dynamisk säkerhet](../tutorials/aas-supplemental-lesson-dynamic-security.md) </span><span class="sxs-lookup"><span data-stu-id="25d6a-139">[Supplemental Lesson - Dynamic security](../tutorials/aas-supplemental-lesson-dynamic-security.md) </span></span>  
<span data-ttu-id="25d6a-140">[Kompletterande lektion – Detaljrader](../tutorials/aas-supplemental-lesson-detail-rows.md) </span><span class="sxs-lookup"><span data-stu-id="25d6a-140">[Supplemental Lesson - Detail rows](../tutorials/aas-supplemental-lesson-detail-rows.md) </span></span>  
[<span data-ttu-id="25d6a-141">Kompletterande lektion – Ojämna hierarkier</span><span class="sxs-lookup"><span data-stu-id="25d6a-141">Supplemental Lesson - Ragged hierarchies</span></span>](../tutorials/aas-supplemental-lesson-ragged-hierarchies.md)   
