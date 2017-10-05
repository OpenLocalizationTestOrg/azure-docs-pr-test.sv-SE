---
title: "Övervaka Azure Kubernetes kluster med Datadog | Microsoft Docs"
description: "Övervaka Kubernetes kluster i Azure Container Service med hjälp av Datadog"
services: container-service
documentationcenter: 
author: bburns
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/09/2016
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: 40b34457447a8f80d8cdf77579750e0c42df22d0
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-an-azure-container-service-cluster-with-datadog"></a><span data-ttu-id="7e5d9-103">Övervaka ett Azure Container Service-kluster med DataDog</span><span class="sxs-lookup"><span data-stu-id="7e5d9-103">Monitor an Azure Container Service cluster with DataDog</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7e5d9-104">Krav</span><span class="sxs-lookup"><span data-stu-id="7e5d9-104">Prerequisites</span></span>
<span data-ttu-id="7e5d9-105">Den här genomgången förutsätter att du har [skapas ett Kubernetes-kluster med Azure Container Service](container-service-kubernetes-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="7e5d9-105">This walkthrough assumes that you have [created a Kubernetes cluster using Azure Container Service](container-service-kubernetes-walkthrough.md).</span></span>

<span data-ttu-id="7e5d9-106">Det förutsätts även att du har den `az` Azure cli och `kubectl` verktygen som installeras.</span><span class="sxs-lookup"><span data-stu-id="7e5d9-106">It also assumes that you have the `az` Azure cli and `kubectl` tools installed.</span></span>

<span data-ttu-id="7e5d9-107">Du kan testa om du har den `az` installerat genom att köra verktyget:</span><span class="sxs-lookup"><span data-stu-id="7e5d9-107">You can test if you have the `az` tool installed by running:</span></span>

```console
$ az --version
```

<span data-ttu-id="7e5d9-108">Om du inte har den `az` verktyget är installerat, det finns instruktioner [här](https://github.com/azure/azure-cli#installation).</span><span class="sxs-lookup"><span data-stu-id="7e5d9-108">If you don't have the `az` tool installed, there are instructions [here](https://github.com/azure/azure-cli#installation).</span></span>

<span data-ttu-id="7e5d9-109">Du kan testa om du har den `kubectl` installerat genom att köra verktyget:</span><span class="sxs-lookup"><span data-stu-id="7e5d9-109">You can test if you have the `kubectl` tool installed by running:</span></span>

```console
$ kubectl version
```

<span data-ttu-id="7e5d9-110">Om du inte har `kubectl` installerat, kan du köra:</span><span class="sxs-lookup"><span data-stu-id="7e5d9-110">If you don't have `kubectl` installed, you can run:</span></span>

```console
$ az acs kubernetes install-cli
```

## <a name="datadog"></a><span data-ttu-id="7e5d9-111">DataDog</span><span class="sxs-lookup"><span data-stu-id="7e5d9-111">DataDog</span></span>
<span data-ttu-id="7e5d9-112">Datadog är en övervakningstjänsten som samlar in övervakningsdata från din behållare i Azure Container Service-kluster.</span><span class="sxs-lookup"><span data-stu-id="7e5d9-112">Datadog is a monitoring service that gathers monitoring data from your containers within your Azure Container Service cluster.</span></span> <span data-ttu-id="7e5d9-113">Datadog har en Docker-integrering instrumentpanel där du kan se specifika mått i en behållare.</span><span class="sxs-lookup"><span data-stu-id="7e5d9-113">Datadog has a Docker Integration Dashboard where you can see specific metrics within your containers.</span></span> <span data-ttu-id="7e5d9-114">Mätvärden som samlats in från behållarna ordnas efter CPU, minne, nätverk och i/o.</span><span class="sxs-lookup"><span data-stu-id="7e5d9-114">Metrics gathered from your containers are organized by CPU, Memory, Network and I/O.</span></span> <span data-ttu-id="7e5d9-115">Datadog delar mått i behållare och bilder.</span><span class="sxs-lookup"><span data-stu-id="7e5d9-115">Datadog splits metrics into containers and images.</span></span>

<span data-ttu-id="7e5d9-116">Du måste först [skapa ett konto](https://www.datadoghq.com/lpg/)</span><span class="sxs-lookup"><span data-stu-id="7e5d9-116">You first need to [create an account](https://www.datadoghq.com/lpg/)</span></span>

## <a name="installing-the-datadog-agent-with-a-daemonset"></a><span data-ttu-id="7e5d9-117">Installera agenten Datadog med en DaemonSet</span><span class="sxs-lookup"><span data-stu-id="7e5d9-117">Installing the Datadog Agent with a DaemonSet</span></span>
<span data-ttu-id="7e5d9-118">DaemonSets som används av Kubernetes för att köra en instans av en behållare på varje värd i klustret.</span><span class="sxs-lookup"><span data-stu-id="7e5d9-118">DaemonSets are used by Kubernetes to run a single instance of a container on each host in the cluster.</span></span>
<span data-ttu-id="7e5d9-119">Det är perfekt för att köra övervakningsagenter.</span><span class="sxs-lookup"><span data-stu-id="7e5d9-119">They're perfect for running monitoring agents.</span></span>

<span data-ttu-id="7e5d9-120">När du har loggat in på Datadog, kan du följa den [Datadog instruktioner](https://app.datadoghq.com/account/settings#agent/kubernetes) installera Datadog agenter på klustret med hjälp av en DaemonSet.</span><span class="sxs-lookup"><span data-stu-id="7e5d9-120">Once you have logged into Datadog, you can follow the [Datadog instructions](https://app.datadoghq.com/account/settings#agent/kubernetes) to install Datadog agents on your cluster using a DaemonSet.</span></span>

## <a name="conclusion"></a><span data-ttu-id="7e5d9-121">Slutsats</span><span class="sxs-lookup"><span data-stu-id="7e5d9-121">Conclusion</span></span>
<span data-ttu-id="7e5d9-122">Klart!</span><span class="sxs-lookup"><span data-stu-id="7e5d9-122">That's it!</span></span> <span data-ttu-id="7e5d9-123">Du bör se data i konsolen för några minuter när agenterna är igång.</span><span class="sxs-lookup"><span data-stu-id="7e5d9-123">Once the agents are up and running you should see data in the console in a few minutes.</span></span> <span data-ttu-id="7e5d9-124">Du kan besöka den integrerade [kubernetes instrumentpanelen](https://app.datadoghq.com/screen/integration/kubernetes) visas en sammanfattning av klustret.</span><span class="sxs-lookup"><span data-stu-id="7e5d9-124">You can visit the integrated [kubernetes dashboard](https://app.datadoghq.com/screen/integration/kubernetes) to see a summary of your cluster.</span></span>
