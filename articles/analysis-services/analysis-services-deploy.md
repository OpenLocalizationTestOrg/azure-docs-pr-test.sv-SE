---
title: Distribuera till Azure Analysis Services med SSDT | Microsoft Docs
description: "Lär dig hur du distribuerar en tabellmodell till en Azure Analysis Services-server med SSDT."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 5f1f0ae7-11de-4923-a3da-888b13a3638c
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/01/2017
ms.author: owend
ms.openlocfilehash: e9a3aedfb6e55696e1525e226fada1062fd5eda8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-a-model-from-ssdt"></a><span data-ttu-id="45a14-103">Distribuera en modell från SSDT</span><span class="sxs-lookup"><span data-stu-id="45a14-103">Deploy a model from SSDT</span></span>
<span data-ttu-id="45a14-104">När du har skapat en server i din Azure-prenumeration är du redo att distribuera en tabellmodelldatabas till den.</span><span class="sxs-lookup"><span data-stu-id="45a14-104">Once you've created a server in your Azure subscription, you're ready to deploy a tabular model database to it.</span></span> <span data-ttu-id="45a14-105">Du kan använda SQL Server Data Tools (SSDT) för att skapa och distribuera ett tabellmodellprojekt som du arbetar med.</span><span class="sxs-lookup"><span data-stu-id="45a14-105">You can use SQL Server Data Tools (SSDT) to build and deploy a tabular model project you're working on.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="45a14-106">Krav</span><span class="sxs-lookup"><span data-stu-id="45a14-106">Prerequisites</span></span>
<span data-ttu-id="45a14-107">Du behöver följande för att komma igång:</span><span class="sxs-lookup"><span data-stu-id="45a14-107">To get started, you need:</span></span>

* <span data-ttu-id="45a14-108">**Analysis Services-server** i Azure.</span><span class="sxs-lookup"><span data-stu-id="45a14-108">**Analysis Services server** in Azure.</span></span> <span data-ttu-id="45a14-109">Läs mer i [Skapa en Azure Analysis Services-server](analysis-services-create-server.md).</span><span class="sxs-lookup"><span data-stu-id="45a14-109">To learn more, see [Create an Azure Analysis Services server](analysis-services-create-server.md).</span></span>
* <span data-ttu-id="45a14-110">**Tabellmodellprojekt** i SSDT eller en befintlig tabellmodell på kompatibilitetsnivå 1 200 eller högre.</span><span class="sxs-lookup"><span data-stu-id="45a14-110">**Tabular model project** in SSDT or an existing tabular model at the 1200 or higher compatibility level.</span></span> <span data-ttu-id="45a14-111">Har du aldrig skapat någon?</span><span class="sxs-lookup"><span data-stu-id="45a14-111">Never created one?</span></span> <span data-ttu-id="45a14-112">Testa [Självstudier för Adventure Works Internetförsäljning – tabellmodell ](https://msdn.microsoft.com/library/hh231691.aspx).</span><span class="sxs-lookup"><span data-stu-id="45a14-112">Try the [Adventure Works Internet sales tabular modeling tutorial](https://msdn.microsoft.com/library/hh231691.aspx).</span></span>
* <span data-ttu-id="45a14-113">**Lokala gateway** – Om en eller flera datakällor finns lokalt i din organisations nätverk måste du installera en [lokal datagateway](analysis-services-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="45a14-113">**On-premises gateway** - If one or more data sources are on-premises in your organization's network, you need to install an [On-premises data gateway](analysis-services-gateway.md).</span></span> <span data-ttu-id="45a14-114">Gatewayen är nödvändig för att din server i molnet ska kunna ansluta till dina lokala datakällor för att bearbeta och uppdatera data i modellen.</span><span class="sxs-lookup"><span data-stu-id="45a14-114">The gateway is necessary for your server in the cloud connect to your on-premises data sources to process and refresh data in the model.</span></span>

> [!TIP]
> <span data-ttu-id="45a14-115">Innan du distribuerar måste du kontrollera att du kan bearbeta data i dina tabeller.</span><span class="sxs-lookup"><span data-stu-id="45a14-115">Before you deploy, make sure you can process the data in your tables.</span></span> <span data-ttu-id="45a14-116">Klicka på **Modell** > **Bearbeta** > **Bearbeta alla** i SSDT.</span><span class="sxs-lookup"><span data-stu-id="45a14-116">In SSDT, click **Model** > **Process** > **Process All**.</span></span> <span data-ttu-id="45a14-117">Om bearbetningen misslyckas kan inte du distribuera.</span><span class="sxs-lookup"><span data-stu-id="45a14-117">If processing fails, you cannot successfully deploy.</span></span>
> 
> 

## <a name="to-deploy-a-tabular-model-from-ssdt"></a><span data-ttu-id="45a14-118">Så här distribuerar du en tabellmodell från SSDT</span><span class="sxs-lookup"><span data-stu-id="45a14-118">To deploy a tabular model from SSDT</span></span>

1. <span data-ttu-id="45a14-119">Innan du distribuerar måste du hämta servernamnet.</span><span class="sxs-lookup"><span data-stu-id="45a14-119">Before you deploy, you need to get the server name.</span></span> <span data-ttu-id="45a14-120">Välj **Azure Portal** > server > **Översikt** > **Servernamn** och kopiera servernamnet.</span><span class="sxs-lookup"><span data-stu-id="45a14-120">In **Azure portal** > server > **Overview** > **Server name**, copy the server name.</span></span>
   
    ![Hämta servernamnet i Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)
2. <span data-ttu-id="45a14-122">Välj SSDT > **Solution Explorer**, högerklicka på projektet > **Egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="45a14-122">In SSDT > **Solution Explorer**, right-click the project > **Properties**.</span></span> <span data-ttu-id="45a14-123">Klistra sedan in servernamnet i **Distribution** > **Server**.</span><span class="sxs-lookup"><span data-stu-id="45a14-123">Then in **Deployment** > **Server** paste the server name.</span></span>   
   
    ![Klistra in servernamnet i egenskapen för distributionsservern](./media/analysis-services-deploy/aas-deploy-deployment-server-property.png)
3. <span data-ttu-id="45a14-125">I **Solution Explorer** högerklickar du på **Egenskaper** och klickar sedan på **Distribuera**.</span><span class="sxs-lookup"><span data-stu-id="45a14-125">In **Solution Explorer**, right-click **Properties**, then click **Deploy**.</span></span> <span data-ttu-id="45a14-126">Du kan uppmanas att logga in i Azure.</span><span class="sxs-lookup"><span data-stu-id="45a14-126">You may be prompted to sign in to Azure.</span></span>
   
    ![Distribuera till en server](./media/analysis-services-deploy/aas-deploy-deploy.png)
   
    <span data-ttu-id="45a14-128">Distributionens status visas både i fönstret Utmatning och i Distribuera.</span><span class="sxs-lookup"><span data-stu-id="45a14-128">Deployment status appears in both the Output window and in Deploy.</span></span>
   
    ![Status för distribution](./media/analysis-services-deploy/aas-deploy-status.png)

<span data-ttu-id="45a14-130">Det var allt!</span><span class="sxs-lookup"><span data-stu-id="45a14-130">That's all there is to it!</span></span>


## <a name="troubleshooting"></a><span data-ttu-id="45a14-131">Felsökning</span><span class="sxs-lookup"><span data-stu-id="45a14-131">Troubleshooting</span></span>
<span data-ttu-id="45a14-132">Om distributionen misslyckas när du distribuerar metadata beror det förmodligen på att SSDT inte kunde ansluta till servern.</span><span class="sxs-lookup"><span data-stu-id="45a14-132">If deployment fails when deploying metadata, it's likely because SSDT couldn't connect to your server.</span></span> <span data-ttu-id="45a14-133">Kontrollera att du kan ansluta till servern med hjälp av SSMS.</span><span class="sxs-lookup"><span data-stu-id="45a14-133">Make sure you can connect to your server using SSMS.</span></span> <span data-ttu-id="45a14-134">Kontrollera egenskapen Distributionsserver för projektet är korrekt.</span><span class="sxs-lookup"><span data-stu-id="45a14-134">Then make sure the Deployment Server property for the project is correct.</span></span>

<span data-ttu-id="45a14-135">Om distributionen misslyckas för en tabell beror det förmodligen på att servern inte kunde ansluta till en datakälla.</span><span class="sxs-lookup"><span data-stu-id="45a14-135">If deployment fails on a table, it's likely because your server couldn't connect to a data source.</span></span> <span data-ttu-id="45a14-136">Om datakällan finns lokalt i din organisations nätverk måste du installera en [lokal datagateway](analysis-services-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="45a14-136">If your data source is on-premises in your organization's network, be sure to install an [On-premises data gateway](analysis-services-gateway.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="45a14-137">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="45a14-137">Next steps</span></span>
<span data-ttu-id="45a14-138">Nu när du har distribuerat en tabellmodell till servern är du redo att ansluta till den.</span><span class="sxs-lookup"><span data-stu-id="45a14-138">Now that you have your tabular model deployed to your server, you're ready to connect to it.</span></span> <span data-ttu-id="45a14-139">Du kan [ansluta till den med SSMS](analysis-services-manage.md) om du vill hantera den.</span><span class="sxs-lookup"><span data-stu-id="45a14-139">You can [connect to it with SSMS](analysis-services-manage.md) to manage it.</span></span> <span data-ttu-id="45a14-140">Och du kan [ansluta till den med ett klientverktyg](analysis-services-connect.md), till exempel Power BI, Power BI Desktop eller Excel, och börja skapa rapporter.</span><span class="sxs-lookup"><span data-stu-id="45a14-140">And, you can [connect to it using a client tool](analysis-services-connect.md) like Power BI, Power BI Desktop, or Excel, and start creating reports.</span></span>

