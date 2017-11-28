---
title: aaaCreate Analysis Services-server i Azure | Microsoft Docs
description: "Lär dig hur toocreate Analysis Services-server-instansen i Azure."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 7f560216-8a9a-4d06-852e-48cf24deab19
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: 3668f659039f79f3dd71498d1066e8682bf33228
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-analysis-services-server-in-azure-portal"></a><span data-ttu-id="45f6c-103">Skapa en Azure Analysis Services-server i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="45f6c-103">Create an Azure Analysis Services server in Azure portal</span></span>
<span data-ttu-id="45f6c-104">Den här artikeln vägleder dig genom att skapa en resurs för Analysis Services-server i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="45f6c-104">This article walks you through creating an Analysis Services server resource in your Azure subscription.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="45f6c-105">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="45f6c-105">Before you begin</span></span>
<span data-ttu-id="45f6c-106">toocomplete Snabbstart, behöver du:</span><span class="sxs-lookup"><span data-stu-id="45f6c-106">toocomplete this quickstart, you need:</span></span>

* <span data-ttu-id="45f6c-107">**Azure-prenumeration**: Besök [kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/offers/ms-azr-0044p/) toocreate ett konto.</span><span class="sxs-lookup"><span data-stu-id="45f6c-107">**Azure subscription**: Visit [Azure Free Trial](https://azure.microsoft.com/offers/ms-azr-0044p/) toocreate an account.</span></span>
* <span data-ttu-id="45f6c-108">**Azure Active Directory**: din prenumeration måste vara kopplad till en Azure Active Directory-klient.</span><span class="sxs-lookup"><span data-stu-id="45f6c-108">**Azure Active Directory**: Your subscription must be associated with an Azure Active Directory tenant.</span></span> <span data-ttu-id="45f6c-109">Och du behöver toobe tooAzure inloggad med ett konto i den Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="45f6c-109">And, you need toobe signed in tooAzure with an account in that Azure Active Directory.</span></span> <span data-ttu-id="45f6c-110">Microsoft-konton stöds inte.</span><span class="sxs-lookup"><span data-stu-id="45f6c-110">Microsoft accounts are not supported.</span></span> <span data-ttu-id="45f6c-111">Det finns fler toolearn [autentisering och användarbehörigheter](analysis-services-manage-users.md).</span><span class="sxs-lookup"><span data-stu-id="45f6c-111">toolearn more, see [Authentication and user permissions](analysis-services-manage-users.md).</span></span>
* <span data-ttu-id="45f6c-112">**Resursgruppen**: använda en resursgrupp som du redan har eller [skapa en ny](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="45f6c-112">**Resource group**: Use a resource group you already have or [create a new one](../azure-resource-manager/resource-group-overview.md).</span></span>

> [!NOTE]
> <span data-ttu-id="45f6c-113">Att skapa en server kan resultera i en ny fakturerbar tjänst.</span><span class="sxs-lookup"><span data-stu-id="45f6c-113">Creating a server might result in a new billable service.</span></span> <span data-ttu-id="45f6c-114">Det finns fler toolearn [priser för Analysis Services](https://azure.microsoft.com/pricing/details/analysis-services/).</span><span class="sxs-lookup"><span data-stu-id="45f6c-114">toolearn more, see [Analysis Services pricing](https://azure.microsoft.com/pricing/details/analysis-services/).</span></span>
> 
> 

## <a name="toocreate-a-server-in-azure-portal"></a><span data-ttu-id="45f6c-115">toocreate en server i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="45f6c-115">toocreate a server in Azure portal</span></span>
1. <span data-ttu-id="45f6c-116">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="45f6c-116">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>  
2. <span data-ttu-id="45f6c-117">Klicka på **+ ny** > **Data + analys** > **Analysis Services**.</span><span class="sxs-lookup"><span data-stu-id="45f6c-117">Click **+ New** > **Data + analytics** > **Analysis Services**.</span></span>
3. <span data-ttu-id="45f6c-118">I hello **Analysis Services** bladet, Fyll i hello krävs fält och tryck sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="45f6c-118">In hello **Analysis Services** blade, fill in hello required fields, and then press **Create**.</span></span>
   
    ![Skapa server](./media/analysis-services-create-server/aas-create-server-blade.png)
   
   * <span data-ttu-id="45f6c-120">**Servernamnet**: Skriv ett unikt namn som används tooreference hello-server.</span><span class="sxs-lookup"><span data-stu-id="45f6c-120">**Server name**: Type a unique name used tooreference hello server.</span></span>
   * <span data-ttu-id="45f6c-121">**Prenumerationen**: Välj hello-prenumeration som den här servern växlar till.</span><span class="sxs-lookup"><span data-stu-id="45f6c-121">**Subscription**: Select hello subscription this server bills to.</span></span>
   * <span data-ttu-id="45f6c-122">**Resursgruppen**: dessa behållare är utformad toohelp du hantera en samling Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="45f6c-122">**Resource group**: These containers are designed toohelp you manage a collection of Azure resources.</span></span> <span data-ttu-id="45f6c-123">Det finns fler toolearn [resursgrupper](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="45f6c-123">toolearn more, see [resource groups](../azure-resource-manager/resource-group-overview.md).</span></span>
   * <span data-ttu-id="45f6c-124">**Plats**: den här Azure datacenter plats värdar hello server.</span><span class="sxs-lookup"><span data-stu-id="45f6c-124">**Location**: This Azure datacenter location hosts hello server.</span></span> <span data-ttu-id="45f6c-125">Välj en plats närmast största användarbasen.</span><span class="sxs-lookup"><span data-stu-id="45f6c-125">Choose a location nearest your largest user base.</span></span>
   * <span data-ttu-id="45f6c-126">**Prisnivån**: Välj en prisnivå.</span><span class="sxs-lookup"><span data-stu-id="45f6c-126">**Pricing tier**: Select a pricing tier.</span></span> <span data-ttu-id="45f6c-127">Tabellmodeller in too400 GB stöds.</span><span class="sxs-lookup"><span data-stu-id="45f6c-127">Tabular models up too400 GB are supported.</span></span> <span data-ttu-id="45f6c-128">Det finns fler toolearn [priser för Azure Analysis Services](https://azure.microsoft.com/pricing/details/analysis-services/).</span><span class="sxs-lookup"><span data-stu-id="45f6c-128">toolearn more, see [Azure Analysis Services pricing](https://azure.microsoft.com/pricing/details/analysis-services/).</span></span>
4. <span data-ttu-id="45f6c-129">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="45f6c-129">Click **Create**.</span></span>

<span data-ttu-id="45f6c-130">Skapa tar vanligtvis under en minut; ofta bara några sekunder.</span><span class="sxs-lookup"><span data-stu-id="45f6c-130">Create usually takes under a minute; often just a few seconds.</span></span> <span data-ttu-id="45f6c-131">Om du har valt **lägga till tooPortal**, navigera tooyour portal toosee den nya servern.</span><span class="sxs-lookup"><span data-stu-id="45f6c-131">If you selected **Add tooPortal**, navigate tooyour portal toosee your new server.</span></span> <span data-ttu-id="45f6c-132">Eller navigera för**fler tjänster** > **Analysis Services** toosee om din server är klar.</span><span class="sxs-lookup"><span data-stu-id="45f6c-132">Or, navigate too**More services** > **Analysis Services** toosee if your server is ready.</span></span>

 ![Instrumentpanel](./media/analysis-services-create-server/aas-create-server-dashboard.png)


## <a name="next-steps"></a><span data-ttu-id="45f6c-134">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="45f6c-134">Next steps</span></span>
<span data-ttu-id="45f6c-135">När du har skapat din server, kan du [distribuera en modell](analysis-services-deploy.md) tooit med SSDT eller med SSMS.</span><span class="sxs-lookup"><span data-stu-id="45f6c-135">Once you've created your server, you can [deploy a model](analysis-services-deploy.md) tooit by using SSDT or with SSMS.</span></span>

<span data-ttu-id="45f6c-136">Om en modell som du distribuerar tooyour server ansluter tooon lokala datakällor, behöver du tooinstall en [lokala datagateway](analysis-services-gateway.md) på en dator i nätverket.</span><span class="sxs-lookup"><span data-stu-id="45f6c-136">If a model you deploy tooyour server connects tooon-premises data sources, you need tooinstall an [On-premises data gateway](analysis-services-gateway.md) on a computer in your network.</span></span>

