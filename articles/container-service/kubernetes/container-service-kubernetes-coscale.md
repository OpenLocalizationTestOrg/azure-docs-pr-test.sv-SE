---
title: aaaMonitor en Azure-Kubernetes kluster med CoScale | Microsoft Docs
description: "Övervaka ett Kubernetes kluster i Azure Container Service med hjälp av CoScale"
services: container-service
documentationcenter: 
author: fryckbos
manager: 
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/22/2017
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: f835e82d2be3afe1d85070bd0bf69649cc6dd2ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-an-azure-container-service-kubernetes-cluster-with-coscale"></a><span data-ttu-id="0d045-103">Övervaka ett Azure Container Service Kubernetes kluster med CoScale</span><span class="sxs-lookup"><span data-stu-id="0d045-103">Monitor an Azure Container Service Kubernetes cluster with CoScale</span></span>

<span data-ttu-id="0d045-104">I den här artikeln visar vi dig hur toodeploy hello [CoScale](https://www.coscale.com/) agent toomonitor alla noder och behållare i din Kubernetes kluster i Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="0d045-104">In this article, we show you how toodeploy hello [CoScale](https://www.coscale.com/) agent toomonitor all nodes and containers in your Kubernetes cluster in Azure Container Service.</span></span> <span data-ttu-id="0d045-105">Du behöver ett konto med CoScale för den här konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="0d045-105">You need an account with CoScale for this configuration.</span></span> 


## <a name="about-coscale"></a><span data-ttu-id="0d045-106">Om CoScale</span><span class="sxs-lookup"><span data-stu-id="0d045-106">About CoScale</span></span> 

<span data-ttu-id="0d045-107">CoScale är en övervakning plattform som samlar in mätvärden och -händelser från alla behållare i flera orchestration-plattformar.</span><span class="sxs-lookup"><span data-stu-id="0d045-107">CoScale is a monitoring platform that gathers metrics and events from all containers in several orchestration platforms.</span></span> <span data-ttu-id="0d045-108">CoScale ger fullständig stack övervakning för Kubernetes miljöer.</span><span class="sxs-lookup"><span data-stu-id="0d045-108">CoScale offers full-stack monitoring for Kubernetes environments.</span></span> <span data-ttu-id="0d045-109">Den innehåller visualiseringar och analytics för alla lager i hello stacken: hello OS, Kubernetes, Docker och program som körs på din behållare.</span><span class="sxs-lookup"><span data-stu-id="0d045-109">It provides visualizations and analytics for all layers in hello stack: hello OS, Kubernetes, Docker, and applications running inside your containers.</span></span> <span data-ttu-id="0d045-110">CoScale erbjuder flera inbyggda övervakning instrumentpaneler, och den har inbyggda avvikelseidentifiering identifiering tooallow operatorer och utvecklare toofind infrastruktur- och problem för snabb.</span><span class="sxs-lookup"><span data-stu-id="0d045-110">CoScale offers several built-in monitoring dashboards, and it has built-in anomaly detection tooallow operators and developers toofind infrastructure and application issues fast.</span></span>

![CoScale UI](./media/container-service-kubernetes-coscale/coscale.png)

<span data-ttu-id="0d045-112">Som det visas i den här artikeln, kan du installera agenter på en Kubernetes klustret toorun CoScale som ett SaaS-lösningen.</span><span class="sxs-lookup"><span data-stu-id="0d045-112">As shown in this article, you can install agents on a Kubernetes cluster toorun CoScale as a SaaS solution.</span></span> <span data-ttu-id="0d045-113">Om du vill tookeep dina data på plats, är CoScale också tillgängligt för lokal installation.</span><span class="sxs-lookup"><span data-stu-id="0d045-113">If you want tookeep your data on-site, CoScale is also available for on-premises installation.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="0d045-114">Krav</span><span class="sxs-lookup"><span data-stu-id="0d045-114">Prerequisites</span></span>

<span data-ttu-id="0d045-115">Du måste först för[skapa ett konto för CoScale](https://www.coscale.com/free-trial).</span><span class="sxs-lookup"><span data-stu-id="0d045-115">You first need too[create a CoScale account](https://www.coscale.com/free-trial).</span></span>

<span data-ttu-id="0d045-116">Den här genomgången förutsätter att du har [skapas ett Kubernetes-kluster med Azure Container Service](container-service-kubernetes-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="0d045-116">This walkthrough assumes that you have [created a Kubernetes cluster using Azure Container Service](container-service-kubernetes-walkthrough.md).</span></span>

<span data-ttu-id="0d045-117">Det förutsätts även att du har hello `az` Azure CLI och `kubectl` verktygen som installeras.</span><span class="sxs-lookup"><span data-stu-id="0d045-117">It also assumes that you have hello `az` Azure CLI and `kubectl` tools installed.</span></span>

<span data-ttu-id="0d045-118">Du kan testa om du har hello `az` installerat genom att köra verktyget:</span><span class="sxs-lookup"><span data-stu-id="0d045-118">You can test if you have hello `az` tool installed by running:</span></span>

```azurecli
az --version
```

<span data-ttu-id="0d045-119">Om du inte har hello `az` verktyget är installerat, det finns instruktioner [här](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="0d045-119">If you don't have hello `az` tool installed, there are instructions [here](/cli/azure/install-azure-cli).</span></span>

<span data-ttu-id="0d045-120">Du kan testa om du har hello `kubectl` installerat genom att köra verktyget:</span><span class="sxs-lookup"><span data-stu-id="0d045-120">You can test if you have hello `kubectl` tool installed by running:</span></span>

```bash
kubectl version
```

<span data-ttu-id="0d045-121">Om du inte har `kubectl` installerat, kan du köra:</span><span class="sxs-lookup"><span data-stu-id="0d045-121">If you don't have `kubectl` installed, you can run:</span></span>

```azurecli
az acs kubernetes install-cli
```

## <a name="installing-hello-coscale-agent-with-a-daemonset"></a><span data-ttu-id="0d045-122">Hej CoScale agent installeras med en DaemonSet</span><span class="sxs-lookup"><span data-stu-id="0d045-122">Installing hello CoScale agent with a DaemonSet</span></span>
<span data-ttu-id="0d045-123">[DaemonSets](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) är används av Kubernetes toorun en instans av en behållare på varje värd i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="0d045-123">[DaemonSets](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) are used by Kubernetes toorun a single instance of a container on each host in hello cluster.</span></span>
<span data-ttu-id="0d045-124">Det är perfekt för att köra övervakningsagenter, till exempel hello CoScale agent.</span><span class="sxs-lookup"><span data-stu-id="0d045-124">They're perfect for running monitoring agents such as hello CoScale agent.</span></span>

<span data-ttu-id="0d045-125">När du loggar in tooCoScale går toohello [agent sidan](https://app.coscale.com/) tooinstall CoScale agenter på klustret med hjälp av en DaemonSet.</span><span class="sxs-lookup"><span data-stu-id="0d045-125">After you log in tooCoScale, go toohello [agent page](https://app.coscale.com/) tooinstall CoScale agents on your cluster using a DaemonSet.</span></span> <span data-ttu-id="0d045-126">Hej CoScale Användargränssnittet innehåller interaktiv configuration steg toocreate en agent och starta övervakning fullständig Kubernetes klustret.</span><span class="sxs-lookup"><span data-stu-id="0d045-126">hello CoScale UI provides guided configuration steps toocreate an agent and start monitoring your complete Kubernetes cluster.</span></span>

![CoScale agentkonfiguration](./media/container-service-kubernetes-coscale/installation.png)

<span data-ttu-id="0d045-128">toostart hello agent på hello klustret kör hello angivna kommando:</span><span class="sxs-lookup"><span data-stu-id="0d045-128">toostart hello agent on hello cluster, run hello supplied command:</span></span>

![Starta hello CoScale agent](./media/container-service-kubernetes-coscale/agent_script.png)

<span data-ttu-id="0d045-130">Klart!</span><span class="sxs-lookup"><span data-stu-id="0d045-130">That's it!</span></span> <span data-ttu-id="0d045-131">När hello agenter är igång kan bör du se data i hello-konsolen på några minuter.</span><span class="sxs-lookup"><span data-stu-id="0d045-131">Once hello agents are up and running, you should see data in hello console in a few minutes.</span></span> <span data-ttu-id="0d045-132">Besök hello [agent sidan](https://app.coscale.com/) toosee en sammanfattning av klustret, utföra ytterligare konfigurationssteg och se instrumentpaneler, till exempel hello **Kubernetes kluster översikt**.</span><span class="sxs-lookup"><span data-stu-id="0d045-132">Visit hello [agent page](https://app.coscale.com/) toosee a summary of your cluster, perform additional configuration steps, and see dashboards such as hello **Kubernetes cluster overview**.</span></span>

![Översikt över kluster Kubernetes](./media/container-service-kubernetes-coscale/dashboard_clusteroverview.png)

<span data-ttu-id="0d045-134">Hej CoScale agenten distribueras automatiskt på nya datorer i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="0d045-134">hello CoScale agent is automatically deployed on new machines in hello cluster.</span></span> <span data-ttu-id="0d045-135">hello-agenten uppdateras automatiskt när en ny version släpps.</span><span class="sxs-lookup"><span data-stu-id="0d045-135">hello agent updates automatically when a new version is released.</span></span>


## <a name="next-steps"></a><span data-ttu-id="0d045-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0d045-136">Next steps</span></span>

<span data-ttu-id="0d045-137">Se hello [CoScale dokumentationen](http://docs.coscale.com/) och [blogg](https://www.coscale.com/blog) för mer information om CoScale övervakningslösningar.</span><span class="sxs-lookup"><span data-stu-id="0d045-137">See hello [CoScale documentation](http://docs.coscale.com/) and [blog](https://www.coscale.com/blog) for more more information about CoScale monitoring solutions.</span></span> 

