---
title: "Azure Resource Manager kärnkvot öka begäranden | Microsoft Docs"
description: "Azure Resource Manager kärnkvot öka begäranden"
author: ganganarayanan
ms.author: gangan
ms.date: 1/18/2017
ms.topic: article
ms.service: microsoft-docs
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: cb6c5b3e86f126d4110d1cd29d8c9891e356e414
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="resource-manager-core-quota-increase-requests"></a><span data-ttu-id="cf98b-103">Kärnkvot för hanteraren för filserverresurser öka begäranden</span><span class="sxs-lookup"><span data-stu-id="cf98b-103">Resource Manager core quota increase requests</span></span>

<span data-ttu-id="cf98b-104">Hanteraren för filserverresurser core kvoter tillämpas på den regionsnivån och SKU-familjen nivå.</span><span class="sxs-lookup"><span data-stu-id="cf98b-104">Resource Manager core quotas are enforced at the region level and SKU family level.</span></span>
<span data-ttu-id="cf98b-105">Mer information om hur kvoter tillämpas på den [Azure-prenumeration och tjänstbegränsningarna](http://aka.ms/quotalimits) sidan.</span><span class="sxs-lookup"><span data-stu-id="cf98b-105">Learn more about how quotas are enforced on the [Azure subscription and service limits](http://aka.ms/quotalimits) page.</span></span>
<span data-ttu-id="cf98b-106">Om du vill veta mer om SKU familjer, du kan jämföra kostnader och prestanda på den [prissättning för Virtual Machines](http://aka.ms/pricingcompute) sidan.</span><span class="sxs-lookup"><span data-stu-id="cf98b-106">To learn more about SKU Families, you may compare cost and performance on the [Virtual Machines Pricing](http://aka.ms/pricingcompute) page.</span></span>

<span data-ttu-id="cf98b-107">Skapa ett supportärende för kvot för kärnor i Azure-portalen om du vill begära en [https://portal.azure.com](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="cf98b-107">To request an increase, create a Quota support case for Cores in the Azure portal, [https://portal.azure.com](https://portal.azure.com).</span></span>

> [!NOTE]
> <span data-ttu-id="cf98b-108">Lär dig hur du [skapa en supportbegäran](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="cf98b-108">Learn how to [create a support request](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) in the Azure portal</span></span>

1. <span data-ttu-id="cf98b-109">Välj typ av problem som ”kvoten” och typ av kvot som ”kärnor” på sidan nytt stöd för begäran.</span><span class="sxs-lookup"><span data-stu-id="cf98b-109">On the new support request page, select Issue type as "Quota" and Quota type as "Cores".</span></span>

    ![Kvoten grunderna bladet](./media/resource-manager-core-quotas-request/Basics-blade.png)

2. <span data-ttu-id="cf98b-111">Välj distributionsmodell som ”Resource Manager” och välj en plats.</span><span class="sxs-lookup"><span data-stu-id="cf98b-111">Select Deployment model as "Resource Manager" and select a location.</span></span>

    ![Kvoten problemet bladet](./media/resource-manager-core-quotas-request/Problem-step.png)

3. <span data-ttu-id="cf98b-113">Välj SKU-familjer som kräver en ökning.</span><span class="sxs-lookup"><span data-stu-id="cf98b-113">Select the SKU Families that require an increase.</span></span>

    ![SKU-serien vald](./media/resource-manager-core-quotas-request/SKU-selected.png)

4. <span data-ttu-id="cf98b-115">Ange de nya gränserna som på prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="cf98b-115">Enter the new limits you would like on the subscription.</span></span>

    ![Begäran om ny SKU kvot](./media/resource-manager-core-quotas-request/SKU-new-quota.png)

- <span data-ttu-id="cf98b-117">Avmarkera SKU: N i SKU-familjen listrutan för att ta bort en rad eller klicka på ikonen Ignorera ”x”.</span><span class="sxs-lookup"><span data-stu-id="cf98b-117">To remove a line, uncheck the SKU from the SKU family dropdown or click the discard "x" icon.</span></span>
<span data-ttu-id="cf98b-118">Klicka på ”Nästa” på sidan problemet steg för att fortsätta med stöd för begäran skapas när du har angett önskade kvoten för varje SKU-serien.</span><span class="sxs-lookup"><span data-stu-id="cf98b-118">After entering the desired quota for each SKU family, click "Next" on the Problem step page to continue with the support request creation.</span></span>
