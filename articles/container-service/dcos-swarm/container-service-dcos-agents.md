---
title: "aaaDC/OS-agenten pooler för Azure Container Service | Microsoft Docs"
description: "Så här fungerar hello offentliga och privata agent pooler med ett Azure Container Service DC/OS-kluster"
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
ms.openlocfilehash: c7d3889db07cb9908e8b68b668bd8a14ef3c2552
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="dcos-agent-pools-for-azure-container-service"></a><span data-ttu-id="19414-104">DC/OS-agenten pooler för Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="19414-104">DC/OS agent pools for Azure Container Service</span></span>
<span data-ttu-id="19414-105">DC/OS-kluster i Azure Container Service innehålla agent noder i två pooler, en pool med offentliga och privata poolen.</span><span class="sxs-lookup"><span data-stu-id="19414-105">DC/OS clusters in Azure Container Service contain agent nodes in two pools, a public pool and a private pool.</span></span> <span data-ttu-id="19414-106">Ett program kan distribueras tooeither poolen, påverkar tillgängligheten mellan datorer i container service.</span><span class="sxs-lookup"><span data-stu-id="19414-106">An application can be deployed tooeither pool, affecting accessibility between machines in your container service.</span></span> <span data-ttu-id="19414-107">hello datorer kan vara synliga toohello internet (offentlig) eller hålls intern (privat).</span><span class="sxs-lookup"><span data-stu-id="19414-107">hello machines can be exposed toohello internet (public) or kept internal (private).</span></span> <span data-ttu-id="19414-108">Den här artikeln ger en kort översikt över därför offentliga och privata pooler.</span><span class="sxs-lookup"><span data-stu-id="19414-108">This article gives a brief overview of why there are public and private pools.</span></span>


* <span data-ttu-id="19414-109">**Privata agenter**: privat agent noder kör via ett icke-dirigerbara nätverk.</span><span class="sxs-lookup"><span data-stu-id="19414-109">**Private agents**: Private agent nodes run through a non-routable network.</span></span> <span data-ttu-id="19414-110">Det här nätverket är endast tillgänglig från hello admin-zon eller via hello offentliga zon gränsrouter.</span><span class="sxs-lookup"><span data-stu-id="19414-110">This network is only accessible from hello admin zone or through hello public zone edge router.</span></span> <span data-ttu-id="19414-111">Som standard startar DC/OS appar på privata agent noder.</span><span class="sxs-lookup"><span data-stu-id="19414-111">By default, DC/OS launches apps on private agent nodes.</span></span> 

* <span data-ttu-id="19414-112">**Offentliga agenter**: offentlig agent noder kör DC/OS-appar och tjänster via ett allmänt tillgängligt nätverk.</span><span class="sxs-lookup"><span data-stu-id="19414-112">**Public agents**: Public agent nodes run DC/OS apps and services through a publicly accessible network.</span></span> 

<span data-ttu-id="19414-113">Mer information om nätverkssäkerhet för DC/OS finns hello [DC/OS-dokumentationen](https://dcos.io/docs/1.7/administration/securing-your-cluster/).</span><span class="sxs-lookup"><span data-stu-id="19414-113">For more information about DC/OS network security, see hello [DC/OS documentation](https://dcos.io/docs/1.7/administration/securing-your-cluster/).</span></span>

## <a name="deploy-agent-pools"></a><span data-ttu-id="19414-114">Distribuera agenten pooler</span><span class="sxs-lookup"><span data-stu-id="19414-114">Deploy agent pools</span></span>

<span data-ttu-id="19414-115">hello DC/OS-agenten pooler i Azure Container Service skapas på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="19414-115">hello DC/OS agent pools In Azure Container Service are created as follows:</span></span>

* <span data-ttu-id="19414-116">Hej **privata poolen** innehåller hello antal agent noder som du anger när du [distribuera hello DC/OS-klustret](container-service-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="19414-116">hello **private pool** contains hello number of agent nodes that you specify when you [deploy hello DC/OS cluster](container-service-deployment.md).</span></span> 

* <span data-ttu-id="19414-117">Hej **offentliga poolen** ursprungligen innehåller ett förbestämt antal agent noder.</span><span class="sxs-lookup"><span data-stu-id="19414-117">hello **public pool** initially contains a predetermined number of agent nodes.</span></span> <span data-ttu-id="19414-118">Den här poolen läggs till automatiskt när hello DC/OS-klustret har etablerats.</span><span class="sxs-lookup"><span data-stu-id="19414-118">This pool is added automatically when hello DC/OS cluster is provisioned.</span></span>

<span data-ttu-id="19414-119">hello privata poolen och hello offentliga poolen är skalningsuppsättningar i virtuella Azure-datorn.</span><span class="sxs-lookup"><span data-stu-id="19414-119">hello private pool and hello public pool are Azure virtual machine scale sets.</span></span> <span data-ttu-id="19414-120">Du kan ändra dessa pooler efter distributionen.</span><span class="sxs-lookup"><span data-stu-id="19414-120">You can resize these pools after deployment.</span></span>

## <a name="use-agent-pools"></a><span data-ttu-id="19414-121">Använda agent-pooler</span><span class="sxs-lookup"><span data-stu-id="19414-121">Use agent pools</span></span>
<span data-ttu-id="19414-122">Som standard **Marathon** distribuerar några nya program toohello *privata* agent noder.</span><span class="sxs-lookup"><span data-stu-id="19414-122">By default, **Marathon** deploys any new application toohello *private* agent nodes.</span></span> <span data-ttu-id="19414-123">Du måste distribuera hello programmet toohello tooexplicitly *offentliga* noder under hello skapandet av programmet hello.</span><span class="sxs-lookup"><span data-stu-id="19414-123">You have tooexplicitly deploy hello application toohello *public* nodes during hello creation of hello application.</span></span> <span data-ttu-id="19414-124">Välj hello **valfritt** och ange **slave_public** för hello **accepterade resursroller** värde.</span><span class="sxs-lookup"><span data-stu-id="19414-124">Select hello **Optional** tab and enter **slave_public** for hello **Accepted Resource Roles** value.</span></span> <span data-ttu-id="19414-125">Den här processen dokumenteras [här](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) och i hello [DC/OS](https://dcos.io/docs/1.7/administration/installing/custom/create-public-agent/) dokumentation.</span><span class="sxs-lookup"><span data-stu-id="19414-125">This process is documented [here](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) and in hello [DC/OS](https://dcos.io/docs/1.7/administration/installing/custom/create-public-agent/) documentation.</span></span>

## <a name="next-steps"></a><span data-ttu-id="19414-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="19414-126">Next steps</span></span>
* <span data-ttu-id="19414-127">Läs mer om [hantera DC/OS-behållare](container-service-mesos-marathon-ui.md).</span><span class="sxs-lookup"><span data-stu-id="19414-127">Read more about [managing your DC/OS containers](container-service-mesos-marathon-ui.md).</span></span>

* <span data-ttu-id="19414-128">Lär dig hur för[öppna hello brandväggen](container-service-enable-public-access.md) tillhandahålls av Azure tooallow offentlig åtkomst tooyour DC/OS-behållare.</span><span class="sxs-lookup"><span data-stu-id="19414-128">Learn how too[open hello firewall](container-service-enable-public-access.md) provided by Azure tooallow public access tooyour DC/OS containers.</span></span>

