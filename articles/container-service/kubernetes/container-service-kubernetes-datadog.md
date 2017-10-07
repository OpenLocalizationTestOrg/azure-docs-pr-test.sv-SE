---
title: aaaMonitor Azure Kubernetes kluster med Datadog | Microsoft Docs
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
ms.openlocfilehash: bccd8b59a048e0f791172fcfc4eeba6370dafcc0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-an-azure-container-service-cluster-with-datadog"></a><span data-ttu-id="4ac57-103">Övervaka ett Azure Container Service-kluster med DataDog</span><span class="sxs-lookup"><span data-stu-id="4ac57-103">Monitor an Azure Container Service cluster with DataDog</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4ac57-104">Krav</span><span class="sxs-lookup"><span data-stu-id="4ac57-104">Prerequisites</span></span>
<span data-ttu-id="4ac57-105">Den här genomgången förutsätter att du har [skapas ett Kubernetes-kluster med Azure Container Service](container-service-kubernetes-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="4ac57-105">This walkthrough assumes that you have [created a Kubernetes cluster using Azure Container Service](container-service-kubernetes-walkthrough.md).</span></span>

<span data-ttu-id="4ac57-106">Det förutsätts även att du har hello `az` Azure cli och `kubectl` verktygen som installeras.</span><span class="sxs-lookup"><span data-stu-id="4ac57-106">It also assumes that you have hello `az` Azure cli and `kubectl` tools installed.</span></span>

<span data-ttu-id="4ac57-107">Du kan testa om du har hello `az` installerat genom att köra verktyget:</span><span class="sxs-lookup"><span data-stu-id="4ac57-107">You can test if you have hello `az` tool installed by running:</span></span>

```console
$ az --version
```

<span data-ttu-id="4ac57-108">Om du inte har hello `az` verktyget är installerat, det finns instruktioner [här](https://github.com/azure/azure-cli#installation).</span><span class="sxs-lookup"><span data-stu-id="4ac57-108">If you don't have hello `az` tool installed, there are instructions [here](https://github.com/azure/azure-cli#installation).</span></span>

<span data-ttu-id="4ac57-109">Du kan testa om du har hello `kubectl` installerat genom att köra verktyget:</span><span class="sxs-lookup"><span data-stu-id="4ac57-109">You can test if you have hello `kubectl` tool installed by running:</span></span>

```console
$ kubectl version
```

<span data-ttu-id="4ac57-110">Om du inte har `kubectl` installerat, kan du köra:</span><span class="sxs-lookup"><span data-stu-id="4ac57-110">If you don't have `kubectl` installed, you can run:</span></span>

```console
$ az acs kubernetes install-cli
```

## <a name="datadog"></a><span data-ttu-id="4ac57-111">DataDog</span><span class="sxs-lookup"><span data-stu-id="4ac57-111">DataDog</span></span>
<span data-ttu-id="4ac57-112">Datadog är en övervakningstjänsten som samlar in övervakningsdata från din behållare i Azure Container Service-kluster.</span><span class="sxs-lookup"><span data-stu-id="4ac57-112">Datadog is a monitoring service that gathers monitoring data from your containers within your Azure Container Service cluster.</span></span> <span data-ttu-id="4ac57-113">Datadog har en Docker-integrering instrumentpanel där du kan se specifika mått i en behållare.</span><span class="sxs-lookup"><span data-stu-id="4ac57-113">Datadog has a Docker Integration Dashboard where you can see specific metrics within your containers.</span></span> <span data-ttu-id="4ac57-114">Mätvärden som samlats in från behållarna ordnas efter CPU, minne, nätverk och i/o.</span><span class="sxs-lookup"><span data-stu-id="4ac57-114">Metrics gathered from your containers are organized by CPU, Memory, Network and I/O.</span></span> <span data-ttu-id="4ac57-115">Datadog delar mått i behållare och bilder.</span><span class="sxs-lookup"><span data-stu-id="4ac57-115">Datadog splits metrics into containers and images.</span></span>

<span data-ttu-id="4ac57-116">Du måste först för[skapa ett konto](https://www.datadoghq.com/lpg/)</span><span class="sxs-lookup"><span data-stu-id="4ac57-116">You first need too[create an account](https://www.datadoghq.com/lpg/)</span></span>

## <a name="installing-hello-datadog-agent-with-a-daemonset"></a><span data-ttu-id="4ac57-117">Installera hello Datadog Agent med en DaemonSet</span><span class="sxs-lookup"><span data-stu-id="4ac57-117">Installing hello Datadog Agent with a DaemonSet</span></span>
<span data-ttu-id="4ac57-118">DaemonSets används av Kubernetes toorun en instans av en behållare på varje värd i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="4ac57-118">DaemonSets are used by Kubernetes toorun a single instance of a container on each host in hello cluster.</span></span>
<span data-ttu-id="4ac57-119">Det är perfekt för att köra övervakningsagenter.</span><span class="sxs-lookup"><span data-stu-id="4ac57-119">They're perfect for running monitoring agents.</span></span>

<span data-ttu-id="4ac57-120">När du har loggat in på Datadog, du kan följa hello [Datadog instruktioner](https://app.datadoghq.com/account/settings#agent/kubernetes) tooinstall Datadog agenter på klustret med hjälp av en DaemonSet.</span><span class="sxs-lookup"><span data-stu-id="4ac57-120">Once you have logged into Datadog, you can follow hello [Datadog instructions](https://app.datadoghq.com/account/settings#agent/kubernetes) tooinstall Datadog agents on your cluster using a DaemonSet.</span></span>

## <a name="conclusion"></a><span data-ttu-id="4ac57-121">Slutsats</span><span class="sxs-lookup"><span data-stu-id="4ac57-121">Conclusion</span></span>
<span data-ttu-id="4ac57-122">Klart!</span><span class="sxs-lookup"><span data-stu-id="4ac57-122">That's it!</span></span> <span data-ttu-id="4ac57-123">När hello agenter är igång och kör du bör se data i hello-konsolen på några minuter.</span><span class="sxs-lookup"><span data-stu-id="4ac57-123">Once hello agents are up and running you should see data in hello console in a few minutes.</span></span> <span data-ttu-id="4ac57-124">Du besöker hello integrerad [kubernetes instrumentpanelen](https://app.datadoghq.com/screen/integration/kubernetes) toosee en sammanfattning av klustret.</span><span class="sxs-lookup"><span data-stu-id="4ac57-124">You can visit hello integrated [kubernetes dashboard](https://app.datadoghq.com/screen/integration/kubernetes) toosee a summary of your cluster.</span></span>
