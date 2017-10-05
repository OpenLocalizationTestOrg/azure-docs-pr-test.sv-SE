---
title: "DC/OS-agenten pooler för Azure Container Service | Microsoft Docs"
description: "Så här fungerar offentliga och privata agent-pooler med ett Azure Container Service DC/OS-kluster"
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, behållare, Micro-tjänster, Mesos, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/04/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: da4a196b1a73c78dfff7d8310edcc349b8d10665
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="dcos-agent-pools-for-azure-container-service"></a><span data-ttu-id="7a767-104">DC/OS-agenten pooler för Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="7a767-104">DC/OS agent pools for Azure Container Service</span></span>
<span data-ttu-id="7a767-105">DC/OS-kluster i Azure Container Service innehålla agent noder i två pooler, en pool med offentliga och privata poolen.</span><span class="sxs-lookup"><span data-stu-id="7a767-105">DC/OS clusters in Azure Container Service contain agent nodes in two pools, a public pool and a private pool.</span></span> <span data-ttu-id="7a767-106">Ett program kan distribueras till antingen poolen, påverkar tillgängligheten mellan datorer i container service.</span><span class="sxs-lookup"><span data-stu-id="7a767-106">An application can be deployed to either pool, affecting accessibility between machines in your container service.</span></span> <span data-ttu-id="7a767-107">Datorerna kan exponerad mot internet (offentlig) eller hålls intern (privat).</span><span class="sxs-lookup"><span data-stu-id="7a767-107">The machines can be exposed to the internet (public) or kept internal (private).</span></span> <span data-ttu-id="7a767-108">Den här artikeln ger en kort översikt över därför offentliga och privata pooler.</span><span class="sxs-lookup"><span data-stu-id="7a767-108">This article gives a brief overview of why there are public and private pools.</span></span>


* <span data-ttu-id="7a767-109">**Privata agenter**: privat agent noder kör via ett icke-dirigerbara nätverk.</span><span class="sxs-lookup"><span data-stu-id="7a767-109">**Private agents**: Private agent nodes run through a non-routable network.</span></span> <span data-ttu-id="7a767-110">Det här nätverket är endast tillgänglig från zonen admin eller via gränsroutern offentliga zon.</span><span class="sxs-lookup"><span data-stu-id="7a767-110">This network is only accessible from the admin zone or through the public zone edge router.</span></span> <span data-ttu-id="7a767-111">Som standard startar DC/OS appar på privata agent noder.</span><span class="sxs-lookup"><span data-stu-id="7a767-111">By default, DC/OS launches apps on private agent nodes.</span></span> 

* <span data-ttu-id="7a767-112">**Offentliga agenter**: offentlig agent noder kör DC/OS-appar och tjänster via ett allmänt tillgängligt nätverk.</span><span class="sxs-lookup"><span data-stu-id="7a767-112">**Public agents**: Public agent nodes run DC/OS apps and services through a publicly accessible network.</span></span> 

<span data-ttu-id="7a767-113">Mer information om nätverkssäkerhet för DC/OS finns det [DC/OS-dokumentationen](https://dcos.io/docs/1.7/administration/securing-your-cluster/).</span><span class="sxs-lookup"><span data-stu-id="7a767-113">For more information about DC/OS network security, see the [DC/OS documentation](https://dcos.io/docs/1.7/administration/securing-your-cluster/).</span></span>

## <a name="deploy-agent-pools"></a><span data-ttu-id="7a767-114">Distribuera agenten pooler</span><span class="sxs-lookup"><span data-stu-id="7a767-114">Deploy agent pools</span></span>

<span data-ttu-id="7a767-115">DC/OS-agenten pooler i Azure Container Service skapas på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="7a767-115">The DC/OS agent pools In Azure Container Service are created as follows:</span></span>

* <span data-ttu-id="7a767-116">Den **privata poolen** innehåller antalet agent noder som du anger när du [distribuera DC/OS-klustret](container-service-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="7a767-116">The **private pool** contains the number of agent nodes that you specify when you [deploy the DC/OS cluster](container-service-deployment.md).</span></span> 

* <span data-ttu-id="7a767-117">Den **offentliga poolen** ursprungligen innehåller ett förbestämt antal agent noder.</span><span class="sxs-lookup"><span data-stu-id="7a767-117">The **public pool** initially contains a predetermined number of agent nodes.</span></span> <span data-ttu-id="7a767-118">Den här poolen läggs till automatiskt när DC/OS-klustret har tillhandahållits.</span><span class="sxs-lookup"><span data-stu-id="7a767-118">This pool is added automatically when the DC/OS cluster is provisioned.</span></span>

<span data-ttu-id="7a767-119">Poolen privata och offentliga poolen är skalningsuppsättningar i virtuella Azure-datorn.</span><span class="sxs-lookup"><span data-stu-id="7a767-119">The private pool and the public pool are Azure virtual machine scale sets.</span></span> <span data-ttu-id="7a767-120">Du kan ändra dessa pooler efter distributionen.</span><span class="sxs-lookup"><span data-stu-id="7a767-120">You can resize these pools after deployment.</span></span>

## <a name="use-agent-pools"></a><span data-ttu-id="7a767-121">Använda agent-pooler</span><span class="sxs-lookup"><span data-stu-id="7a767-121">Use agent pools</span></span>
<span data-ttu-id="7a767-122">Som standard **Marathon** distribuerar alla nya program till den *privata* agent noder.</span><span class="sxs-lookup"><span data-stu-id="7a767-122">By default, **Marathon** deploys any new application to the *private* agent nodes.</span></span> <span data-ttu-id="7a767-123">Du måste uttryckligen distribuera programmet till den *offentliga* noder under genereringen av programmet.</span><span class="sxs-lookup"><span data-stu-id="7a767-123">You have to explicitly deploy the application to the *public* nodes during the creation of the application.</span></span> <span data-ttu-id="7a767-124">Välj den **valfritt** och ange **slave_public** för den **accepterade resursroller** värde.</span><span class="sxs-lookup"><span data-stu-id="7a767-124">Select the **Optional** tab and enter **slave_public** for the **Accepted Resource Roles** value.</span></span> <span data-ttu-id="7a767-125">Den här processen dokumenteras [här](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) och i den [DC/OS](https://dcos.io/docs/1.7/administration/installing/custom/create-public-agent/) dokumentation.</span><span class="sxs-lookup"><span data-stu-id="7a767-125">This process is documented [here](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) and in the [DC/OS](https://dcos.io/docs/1.7/administration/installing/custom/create-public-agent/) documentation.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7a767-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7a767-126">Next steps</span></span>
* <span data-ttu-id="7a767-127">Läs mer om [hantera DC/OS-behållare](container-service-mesos-marathon-ui.md).</span><span class="sxs-lookup"><span data-stu-id="7a767-127">Read more about [managing your DC/OS containers](container-service-mesos-marathon-ui.md).</span></span>

* <span data-ttu-id="7a767-128">Lär dig hur du [öppna brandväggen](container-service-enable-public-access.md) tillhandahålls av Azure för att tillåta offentlig åtkomst till ditt DC/OS-behållare.</span><span class="sxs-lookup"><span data-stu-id="7a767-128">Learn how to [open the firewall](container-service-enable-public-access.md) provided by Azure to allow public access to your DC/OS containers.</span></span>

