---
title: Skapa en Analysis Services-server i Azure | Microsoft Docs
description: "Lär dig hur du skapar en Analysis Services-server-instans i Azure."
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
ms.openlocfilehash: 95b367e7cd74405088190c1fe19cf92990759d90
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-an-azure-analysis-services-server-in-azure-portal"></a><span data-ttu-id="2dd49-103">Skapa en Azure Analysis Services-server i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="2dd49-103">Create an Azure Analysis Services server in Azure portal</span></span>
<span data-ttu-id="2dd49-104">Den här artikeln vägleder dig genom att skapa en resurs för Analysis Services-server i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="2dd49-104">This article walks you through creating an Analysis Services server resource in your Azure subscription.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="2dd49-105">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="2dd49-105">Before you begin</span></span>
<span data-ttu-id="2dd49-106">Följande krävs för att slutföra den här snabbstarten:</span><span class="sxs-lookup"><span data-stu-id="2dd49-106">To complete this quickstart, you need:</span></span>

* <span data-ttu-id="2dd49-107">**Azure-prenumeration**: Gå till [Kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/offers/ms-azr-0044p/) för att skapa ett konto.</span><span class="sxs-lookup"><span data-stu-id="2dd49-107">**Azure subscription**: Visit [Azure Free Trial](https://azure.microsoft.com/offers/ms-azr-0044p/) to create an account.</span></span>
* <span data-ttu-id="2dd49-108">**Azure Active Directory**: din prenumeration måste vara kopplad till en Azure Active Directory-klient.</span><span class="sxs-lookup"><span data-stu-id="2dd49-108">**Azure Active Directory**: Your subscription must be associated with an Azure Active Directory tenant.</span></span> <span data-ttu-id="2dd49-109">Och du måste vara inloggad i Azure med ett konto i den Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="2dd49-109">And, you need to be signed in to Azure with an account in that Azure Active Directory.</span></span> <span data-ttu-id="2dd49-110">Microsoft-konton stöds inte.</span><span class="sxs-lookup"><span data-stu-id="2dd49-110">Microsoft accounts are not supported.</span></span> <span data-ttu-id="2dd49-111">Mer information finns i [Autentisering och användarbehörigheter](analysis-services-manage-users.md).</span><span class="sxs-lookup"><span data-stu-id="2dd49-111">To learn more, see [Authentication and user permissions](analysis-services-manage-users.md).</span></span>
* <span data-ttu-id="2dd49-112">**Resursgruppen**: använda en resursgrupp som du redan har eller [skapa en ny](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2dd49-112">**Resource group**: Use a resource group you already have or [create a new one](../azure-resource-manager/resource-group-overview.md).</span></span>

> [!NOTE]
> <span data-ttu-id="2dd49-113">Att skapa en server kan resultera i en ny fakturerbar tjänst.</span><span class="sxs-lookup"><span data-stu-id="2dd49-113">Creating a server might result in a new billable service.</span></span> <span data-ttu-id="2dd49-114">Läs mer i [Priser för Azure Analysis Services](https://azure.microsoft.com/pricing/details/analysis-services/).</span><span class="sxs-lookup"><span data-stu-id="2dd49-114">To learn more, see [Analysis Services pricing](https://azure.microsoft.com/pricing/details/analysis-services/).</span></span>
> 
> 

## <a name="to-create-a-server-in-azure-portal"></a><span data-ttu-id="2dd49-115">Så här skapar du en server i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="2dd49-115">To create a server in Azure portal</span></span>
1. <span data-ttu-id="2dd49-116">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="2dd49-116">Sign in to the [Azure portal](https://portal.azure.com).</span></span>  
2. <span data-ttu-id="2dd49-117">Klicka på **+ ny** > **Data + analys** > **Analysis Services**.</span><span class="sxs-lookup"><span data-stu-id="2dd49-117">Click **+ New** > **Data + analytics** > **Analysis Services**.</span></span>
3. <span data-ttu-id="2dd49-118">I den **Analysis Services** bladet, Fyll i de obligatoriska fälten och tryck sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="2dd49-118">In the **Analysis Services** blade, fill in the required fields, and then press **Create**.</span></span>
   
    ![Skapa server](./media/analysis-services-create-server/aas-create-server-blade.png)
   
   * <span data-ttu-id="2dd49-120">**Servernamnet**: Ange ett unikt namn som refererar till servern.</span><span class="sxs-lookup"><span data-stu-id="2dd49-120">**Server name**: Type a unique name used to reference the server.</span></span>
   * <span data-ttu-id="2dd49-121">**Prenumerationen**: Välj den prenumeration som den här servern växlar till.</span><span class="sxs-lookup"><span data-stu-id="2dd49-121">**Subscription**: Select the subscription this server bills to.</span></span>
   * <span data-ttu-id="2dd49-122">**Resursgruppen**: dessa behållare är avsedda att hjälpa dig att hantera en samling Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="2dd49-122">**Resource group**: These containers are designed to help you manage a collection of Azure resources.</span></span> <span data-ttu-id="2dd49-123">Läs mer i [resursgrupper](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2dd49-123">To learn more, see [resource groups](../azure-resource-manager/resource-group-overview.md).</span></span>
   * <span data-ttu-id="2dd49-124">**Plats**: servern är värd för platsen för den här Azure-datacenter.</span><span class="sxs-lookup"><span data-stu-id="2dd49-124">**Location**: This Azure datacenter location hosts the server.</span></span> <span data-ttu-id="2dd49-125">Välj en plats närmast största användarbasen.</span><span class="sxs-lookup"><span data-stu-id="2dd49-125">Choose a location nearest your largest user base.</span></span>
   * <span data-ttu-id="2dd49-126">**Prisnivån**: Välj en prisnivå.</span><span class="sxs-lookup"><span data-stu-id="2dd49-126">**Pricing tier**: Select a pricing tier.</span></span> <span data-ttu-id="2dd49-127">Tabular-modeller upp till 400 GB stöds.</span><span class="sxs-lookup"><span data-stu-id="2dd49-127">Tabular models up to 400 GB are supported.</span></span> <span data-ttu-id="2dd49-128">Läs mer i [priser för Azure Analysis Services](https://azure.microsoft.com/pricing/details/analysis-services/).</span><span class="sxs-lookup"><span data-stu-id="2dd49-128">To learn more, see [Azure Analysis Services pricing](https://azure.microsoft.com/pricing/details/analysis-services/).</span></span>
4. <span data-ttu-id="2dd49-129">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="2dd49-129">Click **Create**.</span></span>

<span data-ttu-id="2dd49-130">Skapa tar vanligtvis under en minut; ofta bara några sekunder.</span><span class="sxs-lookup"><span data-stu-id="2dd49-130">Create usually takes under a minute; often just a few seconds.</span></span> <span data-ttu-id="2dd49-131">Om du har valt **lägga till till portalen**, gå till portalen för att se den nya servern.</span><span class="sxs-lookup"><span data-stu-id="2dd49-131">If you selected **Add to Portal**, navigate to your portal to see your new server.</span></span> <span data-ttu-id="2dd49-132">Eller navigera till **fler tjänster** > **Analysis Services** att se om din server är klar.</span><span class="sxs-lookup"><span data-stu-id="2dd49-132">Or, navigate to **More services** > **Analysis Services** to see if your server is ready.</span></span>

 ![Instrumentpanel](./media/analysis-services-create-server/aas-create-server-dashboard.png)


## <a name="next-steps"></a><span data-ttu-id="2dd49-134">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2dd49-134">Next steps</span></span>
<span data-ttu-id="2dd49-135">När du har skapat din server, kan du [distribuera en modell](analysis-services-deploy.md) till den med hjälp av SSDT eller med SSMS.</span><span class="sxs-lookup"><span data-stu-id="2dd49-135">Once you've created your server, you can [deploy a model](analysis-services-deploy.md) to it by using SSDT or with SSMS.</span></span>

<span data-ttu-id="2dd49-136">Om en modell som du distribuerar till din server ansluter till lokala datakällor, måste du installera en [lokala datagateway](analysis-services-gateway.md) på en dator i nätverket.</span><span class="sxs-lookup"><span data-stu-id="2dd49-136">If a model you deploy to your server connects to on-premises data sources, you need to install an [On-premises data gateway](analysis-services-gateway.md) on a computer in your network.</span></span>

