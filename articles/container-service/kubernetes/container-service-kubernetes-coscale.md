---
title: "Övervaka ett Azure Kubernetes kluster med CoScale | Microsoft Docs"
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
ms.openlocfilehash: f894191baced710fc0f5a8c8692df98033341a48
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-an-azure-container-service-kubernetes-cluster-with-coscale"></a><span data-ttu-id="afb10-103">Övervaka ett Azure Container Service Kubernetes kluster med CoScale</span><span class="sxs-lookup"><span data-stu-id="afb10-103">Monitor an Azure Container Service Kubernetes cluster with CoScale</span></span>

<span data-ttu-id="afb10-104">I den här artikeln vi hur du distribuerar den [CoScale](https://www.coscale.com/) agenten ska övervaka alla noder och behållare i Kubernetes klustret i Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="afb10-104">In this article, we show you how to deploy the [CoScale](https://www.coscale.com/) agent to monitor all nodes and containers in your Kubernetes cluster in Azure Container Service.</span></span> <span data-ttu-id="afb10-105">Du behöver ett konto med CoScale för den här konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="afb10-105">You need an account with CoScale for this configuration.</span></span> 


## <a name="about-coscale"></a><span data-ttu-id="afb10-106">Om CoScale</span><span class="sxs-lookup"><span data-stu-id="afb10-106">About CoScale</span></span> 

<span data-ttu-id="afb10-107">CoScale är en övervakning plattform som samlar in mätvärden och -händelser från alla behållare i flera orchestration-plattformar.</span><span class="sxs-lookup"><span data-stu-id="afb10-107">CoScale is a monitoring platform that gathers metrics and events from all containers in several orchestration platforms.</span></span> <span data-ttu-id="afb10-108">CoScale ger fullständig stack övervakning för Kubernetes miljöer.</span><span class="sxs-lookup"><span data-stu-id="afb10-108">CoScale offers full-stack monitoring for Kubernetes environments.</span></span> <span data-ttu-id="afb10-109">Den innehåller visualiseringar och analytics för alla lager i stacken: OS, Kubernetes, Docker och program som körs på din behållare.</span><span class="sxs-lookup"><span data-stu-id="afb10-109">It provides visualizations and analytics for all layers in the stack: the OS, Kubernetes, Docker, and applications running inside your containers.</span></span> <span data-ttu-id="afb10-110">CoScale erbjuder flera inbyggda övervakning instrumentpaneler och har inbyggda avvikelseidentifiering att operatörer och utvecklare att snabbt hitta problem infrastruktur och program.</span><span class="sxs-lookup"><span data-stu-id="afb10-110">CoScale offers several built-in monitoring dashboards, and it has built-in anomaly detection to allow operators and developers to find infrastructure and application issues fast.</span></span>

![CoScale UI](./media/container-service-kubernetes-coscale/coscale.png)

<span data-ttu-id="afb10-112">Som det visas i den här artikeln, kan du installera agenter på ett Kubernetes kluster som kör CoScale som ett SaaS-lösningen.</span><span class="sxs-lookup"><span data-stu-id="afb10-112">As shown in this article, you can install agents on a Kubernetes cluster to run CoScale as a SaaS solution.</span></span> <span data-ttu-id="afb10-113">Om du vill behålla dina data på plats är CoScale också tillgängligt för lokal installation.</span><span class="sxs-lookup"><span data-stu-id="afb10-113">If you want to keep your data on-site, CoScale is also available for on-premises installation.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="afb10-114">Krav</span><span class="sxs-lookup"><span data-stu-id="afb10-114">Prerequisites</span></span>

<span data-ttu-id="afb10-115">Du måste först [skapa ett konto för CoScale](https://www.coscale.com/free-trial).</span><span class="sxs-lookup"><span data-stu-id="afb10-115">You first need to [create a CoScale account](https://www.coscale.com/free-trial).</span></span>

<span data-ttu-id="afb10-116">Den här genomgången förutsätter att du har [skapas ett Kubernetes-kluster med Azure Container Service](container-service-kubernetes-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="afb10-116">This walkthrough assumes that you have [created a Kubernetes cluster using Azure Container Service](container-service-kubernetes-walkthrough.md).</span></span>

<span data-ttu-id="afb10-117">Det förutsätts även att du har den `az` Azure CLI och `kubectl` verktygen som installeras.</span><span class="sxs-lookup"><span data-stu-id="afb10-117">It also assumes that you have the `az` Azure CLI and `kubectl` tools installed.</span></span>

<span data-ttu-id="afb10-118">Du kan testa om du har den `az` installerat genom att köra verktyget:</span><span class="sxs-lookup"><span data-stu-id="afb10-118">You can test if you have the `az` tool installed by running:</span></span>

```azurecli
az --version
```

<span data-ttu-id="afb10-119">Om du inte har den `az` verktyget är installerat, det finns instruktioner [här](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="afb10-119">If you don't have the `az` tool installed, there are instructions [here](/cli/azure/install-azure-cli).</span></span>

<span data-ttu-id="afb10-120">Du kan testa om du har den `kubectl` installerat genom att köra verktyget:</span><span class="sxs-lookup"><span data-stu-id="afb10-120">You can test if you have the `kubectl` tool installed by running:</span></span>

```bash
kubectl version
```

<span data-ttu-id="afb10-121">Om du inte har `kubectl` installerat, kan du köra:</span><span class="sxs-lookup"><span data-stu-id="afb10-121">If you don't have `kubectl` installed, you can run:</span></span>

```azurecli
az acs kubernetes install-cli
```

## <a name="installing-the-coscale-agent-with-a-daemonset"></a><span data-ttu-id="afb10-122">Installera agenten CoScale med en DaemonSet</span><span class="sxs-lookup"><span data-stu-id="afb10-122">Installing the CoScale agent with a DaemonSet</span></span>
<span data-ttu-id="afb10-123">[DaemonSets](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) Kubernetes för att köra en instans av en behållare på varje värd i klustret.</span><span class="sxs-lookup"><span data-stu-id="afb10-123">[DaemonSets](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) are used by Kubernetes to run a single instance of a container on each host in the cluster.</span></span>
<span data-ttu-id="afb10-124">Det är perfekt för att köra övervakningsagenter, till exempel CoScale agenten.</span><span class="sxs-lookup"><span data-stu-id="afb10-124">They're perfect for running monitoring agents such as the CoScale agent.</span></span>

<span data-ttu-id="afb10-125">När du loggar in på CoScale går du till den [agent sidan](https://app.coscale.com/) installera CoScale agenter på klustret med hjälp av en DaemonSet.</span><span class="sxs-lookup"><span data-stu-id="afb10-125">After you log in to CoScale, go to the [agent page](https://app.coscale.com/) to install CoScale agents on your cluster using a DaemonSet.</span></span> <span data-ttu-id="afb10-126">CoScale Användargränssnittet innehåller interaktiv konfigurationssteg för att skapa en agent och börja övervaka fullständig Kubernetes klustret.</span><span class="sxs-lookup"><span data-stu-id="afb10-126">The CoScale UI provides guided configuration steps to create an agent and start monitoring your complete Kubernetes cluster.</span></span>

![CoScale agentkonfiguration](./media/container-service-kubernetes-coscale/installation.png)

<span data-ttu-id="afb10-128">Kör det angivna kommandot för att starta agenten på klustret:</span><span class="sxs-lookup"><span data-stu-id="afb10-128">To start the agent on the cluster, run the supplied command:</span></span>

![Starta CoScale-agent](./media/container-service-kubernetes-coscale/agent_script.png)

<span data-ttu-id="afb10-130">Klart!</span><span class="sxs-lookup"><span data-stu-id="afb10-130">That's it!</span></span> <span data-ttu-id="afb10-131">När agenterna är igång kan bör du se data i konsolen på några minuter.</span><span class="sxs-lookup"><span data-stu-id="afb10-131">Once the agents are up and running, you should see data in the console in a few minutes.</span></span> <span data-ttu-id="afb10-132">Besök den [agent sidan](https://app.coscale.com/) om du vill visa en sammanfattning av klustret, utföra ytterligare konfigurationssteg och se instrumentpaneler som den **Kubernetes kluster översikt**.</span><span class="sxs-lookup"><span data-stu-id="afb10-132">Visit the [agent page](https://app.coscale.com/) to see a summary of your cluster, perform additional configuration steps, and see dashboards such as the **Kubernetes cluster overview**.</span></span>

![Översikt över kluster Kubernetes](./media/container-service-kubernetes-coscale/dashboard_clusteroverview.png)

<span data-ttu-id="afb10-134">CoScale-agenten distribueras automatiskt på nya datorer i klustret.</span><span class="sxs-lookup"><span data-stu-id="afb10-134">The CoScale agent is automatically deployed on new machines in the cluster.</span></span> <span data-ttu-id="afb10-135">Agentuppdateringar automatiskt när en ny version släpps.</span><span class="sxs-lookup"><span data-stu-id="afb10-135">The agent updates automatically when a new version is released.</span></span>


## <a name="next-steps"></a><span data-ttu-id="afb10-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="afb10-136">Next steps</span></span>

<span data-ttu-id="afb10-137">Finns det [CoScale dokumentationen](http://docs.coscale.com/) och [blogg](https://www.coscale.com/blog) för mer information om CoScale övervakningslösningar.</span><span class="sxs-lookup"><span data-stu-id="afb10-137">See the [CoScale documentation](http://docs.coscale.com/) and [blog](https://www.coscale.com/blog) for more more information about CoScale monitoring solutions.</span></span> 

