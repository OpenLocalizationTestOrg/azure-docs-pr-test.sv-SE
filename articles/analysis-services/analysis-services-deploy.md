---
title: "aaaDeploy tooAzure Analysis Services med hjälp av SSDT | Microsoft Docs"
description: "Lär dig hur toodeploy en tabellmodell tooan Azure Analysis Services-servern med hjälp av SSDT."
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
ms.openlocfilehash: e3f3771fe32a37b9e0173c274080c647152edd4c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-model-from-ssdt"></a><span data-ttu-id="d750b-103">Distribuera en modell från SSDT</span><span class="sxs-lookup"><span data-stu-id="d750b-103">Deploy a model from SSDT</span></span>
<span data-ttu-id="d750b-104">När du har skapat en server i din Azure-prenumeration, är du redo toodeploy tooit en tabellmodell-databasen.</span><span class="sxs-lookup"><span data-stu-id="d750b-104">Once you've created a server in your Azure subscription, you're ready toodeploy a tabular model database tooit.</span></span> <span data-ttu-id="d750b-105">Du kan använda SQL Server Data Tools (SSDT) toobuild och distribuera en tabellmodell projekt som du arbetar med.</span><span class="sxs-lookup"><span data-stu-id="d750b-105">You can use SQL Server Data Tools (SSDT) toobuild and deploy a tabular model project you're working on.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="d750b-106">Krav</span><span class="sxs-lookup"><span data-stu-id="d750b-106">Prerequisites</span></span>
<span data-ttu-id="d750b-107">tooget igång, behöver du:</span><span class="sxs-lookup"><span data-stu-id="d750b-107">tooget started, you need:</span></span>

* <span data-ttu-id="d750b-108">**Analysis Services-server** i Azure.</span><span class="sxs-lookup"><span data-stu-id="d750b-108">**Analysis Services server** in Azure.</span></span> <span data-ttu-id="d750b-109">Det finns fler toolearn [skapa en Azure Analysis Services-server](analysis-services-create-server.md).</span><span class="sxs-lookup"><span data-stu-id="d750b-109">toolearn more, see [Create an Azure Analysis Services server](analysis-services-create-server.md).</span></span>
* <span data-ttu-id="d750b-110">**Tabellmodell projekt** i SSDT eller en befintlig tabellmodell på hello 1200 eller högre kompatibilitetsnivå.</span><span class="sxs-lookup"><span data-stu-id="d750b-110">**Tabular model project** in SSDT or an existing tabular model at hello 1200 or higher compatibility level.</span></span> <span data-ttu-id="d750b-111">Har du aldrig skapat någon?</span><span class="sxs-lookup"><span data-stu-id="d750b-111">Never created one?</span></span> <span data-ttu-id="d750b-112">Försök hello [Adventure Works Internet försäljning tabular modellering kursen](https://msdn.microsoft.com/library/hh231691.aspx).</span><span class="sxs-lookup"><span data-stu-id="d750b-112">Try hello [Adventure Works Internet sales tabular modeling tutorial](https://msdn.microsoft.com/library/hh231691.aspx).</span></span>
* <span data-ttu-id="d750b-113">**Lokala gateway** – om en eller flera datakällor är lokalt i din organisations nätverk, behöver du tooinstall en [lokala datagateway](analysis-services-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="d750b-113">**On-premises gateway** - If one or more data sources are on-premises in your organization's network, you need tooinstall an [On-premises data gateway](analysis-services-gateway.md).</span></span> <span data-ttu-id="d750b-114">hello gateway krävs för din server i molnet hello ansluta tooyour lokala datakällor tooprocess och uppdatera data i hello-modellen.</span><span class="sxs-lookup"><span data-stu-id="d750b-114">hello gateway is necessary for your server in hello cloud connect tooyour on-premises data sources tooprocess and refresh data in hello model.</span></span>

> [!TIP]
> <span data-ttu-id="d750b-115">Innan du distribuerar måste du kontrollera att du kan bearbeta hello data i tabellerna.</span><span class="sxs-lookup"><span data-stu-id="d750b-115">Before you deploy, make sure you can process hello data in your tables.</span></span> <span data-ttu-id="d750b-116">Klicka på **Modell** > **Bearbeta** > **Bearbeta alla** i SSDT.</span><span class="sxs-lookup"><span data-stu-id="d750b-116">In SSDT, click **Model** > **Process** > **Process All**.</span></span> <span data-ttu-id="d750b-117">Om bearbetningen misslyckas kan inte du distribuera.</span><span class="sxs-lookup"><span data-stu-id="d750b-117">If processing fails, you cannot successfully deploy.</span></span>
> 
> 

## <a name="toodeploy-a-tabular-model-from-ssdt"></a><span data-ttu-id="d750b-118">toodeploy en tabellmodell från SSDT</span><span class="sxs-lookup"><span data-stu-id="d750b-118">toodeploy a tabular model from SSDT</span></span>

1. <span data-ttu-id="d750b-119">Innan du distribuerar måste tooget hello servernamn.</span><span class="sxs-lookup"><span data-stu-id="d750b-119">Before you deploy, you need tooget hello server name.</span></span> <span data-ttu-id="d750b-120">I **Azure-portalen** > server > **översikt** > **servernamn**, kopiera hello servernamn.</span><span class="sxs-lookup"><span data-stu-id="d750b-120">In **Azure portal** > server > **Overview** > **Server name**, copy hello server name.</span></span>
   
    ![Hämta servernamnet i Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)
2. <span data-ttu-id="d750b-122">I SSDT > **Solution Explorer**, högerklicka på hello project > **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="d750b-122">In SSDT > **Solution Explorer**, right-click hello project > **Properties**.</span></span> <span data-ttu-id="d750b-123">I **distribution** > **Server** klistra in hello servernamn.</span><span class="sxs-lookup"><span data-stu-id="d750b-123">Then in **Deployment** > **Server** paste hello server name.</span></span>   
   
    ![Klistra in servernamnet i egenskapen för distributionsservern](./media/analysis-services-deploy/aas-deploy-deployment-server-property.png)
3. <span data-ttu-id="d750b-125">I **Solution Explorer** högerklickar du på **Egenskaper** och klickar sedan på **Distribuera**.</span><span class="sxs-lookup"><span data-stu-id="d750b-125">In **Solution Explorer**, right-click **Properties**, then click **Deploy**.</span></span> <span data-ttu-id="d750b-126">Du kanske ange toosign i tooAzure.</span><span class="sxs-lookup"><span data-stu-id="d750b-126">You may be prompted toosign in tooAzure.</span></span>
   
    ![Distribuera tooserver](./media/analysis-services-deploy/aas-deploy-deploy.png)
   
    <span data-ttu-id="d750b-128">Distributionens status visas i båda hello utdata och distribuera.</span><span class="sxs-lookup"><span data-stu-id="d750b-128">Deployment status appears in both hello Output window and in Deploy.</span></span>
   
    ![Status för distribution](./media/analysis-services-deploy/aas-deploy-status.png)

<span data-ttu-id="d750b-130">Det är allt finns tooit!</span><span class="sxs-lookup"><span data-stu-id="d750b-130">That's all there is tooit!</span></span>


## <a name="troubleshooting"></a><span data-ttu-id="d750b-131">Felsökning</span><span class="sxs-lookup"><span data-stu-id="d750b-131">Troubleshooting</span></span>
<span data-ttu-id="d750b-132">Om distributionen misslyckas när du distribuerar metadata är förmodligen eftersom SSDT inte kunde ansluta tooyour server.</span><span class="sxs-lookup"><span data-stu-id="d750b-132">If deployment fails when deploying metadata, it's likely because SSDT couldn't connect tooyour server.</span></span> <span data-ttu-id="d750b-133">Kontrollera att du kan ansluta tooyour server med hjälp av SSMS.</span><span class="sxs-lookup"><span data-stu-id="d750b-133">Make sure you can connect tooyour server using SSMS.</span></span> <span data-ttu-id="d750b-134">Kontrollera sedan hello Distributionsserver-egenskapen för hello projekt är korrekt.</span><span class="sxs-lookup"><span data-stu-id="d750b-134">Then make sure hello Deployment Server property for hello project is correct.</span></span>

<span data-ttu-id="d750b-135">Om distributionen misslyckas för en tabell är förmodligen eftersom servern inte kunde ansluta tooa datakälla.</span><span class="sxs-lookup"><span data-stu-id="d750b-135">If deployment fails on a table, it's likely because your server couldn't connect tooa data source.</span></span> <span data-ttu-id="d750b-136">Om datakällan är lokalt i din organisations nätverk kan vara att tooinstall en [lokala datagateway](analysis-services-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="d750b-136">If your data source is on-premises in your organization's network, be sure tooinstall an [On-premises data gateway](analysis-services-gateway.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="d750b-137">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d750b-137">Next steps</span></span>
<span data-ttu-id="d750b-138">Nu när du har tabellmodell distribuerade tooyour servern är klar tooconnect tooit.</span><span class="sxs-lookup"><span data-stu-id="d750b-138">Now that you have your tabular model deployed tooyour server, you're ready tooconnect tooit.</span></span> <span data-ttu-id="d750b-139">Du kan [ansluta tooit med SSMS](analysis-services-manage.md) toomanage den.</span><span class="sxs-lookup"><span data-stu-id="d750b-139">You can [connect tooit with SSMS](analysis-services-manage.md) toomanage it.</span></span> <span data-ttu-id="d750b-140">Och du kan [ansluta tooit med hjälp av ett klientverktyg](analysis-services-connect.md) som Power BI, Power BI Desktop eller Excel och börjar skapa rapporter.</span><span class="sxs-lookup"><span data-stu-id="d750b-140">And, you can [connect tooit using a client tool](analysis-services-connect.md) like Power BI, Power BI Desktop, or Excel, and start creating reports.</span></span>

