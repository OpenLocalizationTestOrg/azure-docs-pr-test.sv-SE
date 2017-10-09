---
title: aaaMonitor Azure Kubernetes kluster - Sysdig | Microsoft Docs
description: "Övervaka Kubernetes kluster i Azure Container Service med hjälp av Sysdig"
services: container-service
documentationcenter: 
author: bburns
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/09/2016
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: a27813d01ee4624b9e993f6185169ad73aeec3a2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-an-azure-container-service-kubernetes-cluster-using-sysdig"></a><span data-ttu-id="830c8-103">Övervaka en Kubernetes för Azure Container Service-kluster med hjälp av Sysdig</span><span class="sxs-lookup"><span data-stu-id="830c8-103">Monitor an Azure Container Service Kubernetes cluster using Sysdig</span></span>

## <a name="prerequisites"></a><span data-ttu-id="830c8-104">Krav</span><span class="sxs-lookup"><span data-stu-id="830c8-104">Prerequisites</span></span>
<span data-ttu-id="830c8-105">Den här genomgången förutsätter att du har [skapas ett Kubernetes-kluster med Azure Container Service](container-service-kubernetes-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="830c8-105">This walkthrough assumes that you have [created a Kubernetes cluster using Azure Container Service](container-service-kubernetes-walkthrough.md).</span></span>

<span data-ttu-id="830c8-106">Det förutsätts även att du har hello azure cli och kubectl tools har installerats.</span><span class="sxs-lookup"><span data-stu-id="830c8-106">It also assumes that you have hello azure cli and kubectl tools installed.</span></span>

<span data-ttu-id="830c8-107">Du kan testa om du har hello `az` installerat genom att köra verktyget:</span><span class="sxs-lookup"><span data-stu-id="830c8-107">You can test if you have hello `az` tool installed by running:</span></span>

```console
$ az --version
```

<span data-ttu-id="830c8-108">Om du inte har hello `az` verktyget är installerat, det finns instruktioner [här](https://github.com/azure/azure-cli#installation).</span><span class="sxs-lookup"><span data-stu-id="830c8-108">If you don't have hello `az` tool installed, there are instructions [here](https://github.com/azure/azure-cli#installation).</span></span>

<span data-ttu-id="830c8-109">Du kan testa om du har hello `kubectl` installerat genom att köra verktyget:</span><span class="sxs-lookup"><span data-stu-id="830c8-109">You can test if you have hello `kubectl` tool installed by running:</span></span>

```console
$ kubectl version
```

<span data-ttu-id="830c8-110">Om du inte har `kubectl` installerat, kan du köra:</span><span class="sxs-lookup"><span data-stu-id="830c8-110">If you don't have `kubectl` installed, you can run:</span></span>

```console
$ az acs kubernetes install-cli
```

## <a name="sysdig"></a><span data-ttu-id="830c8-111">Sysdig</span><span class="sxs-lookup"><span data-stu-id="830c8-111">Sysdig</span></span>
<span data-ttu-id="830c8-112">Sysdig är en extern övervakning som en tjänst-företag som du kan övervaka behållare i Kubernetes klustret körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="830c8-112">Sysdig is an external monitoring as a service company which can monitor containers in your Kubernetes cluster running in Azure.</span></span> <span data-ttu-id="830c8-113">Med hjälp av Sysdig kräver ett aktivt Sysdig-konto.</span><span class="sxs-lookup"><span data-stu-id="830c8-113">Using Sysdig requires an active Sysdig account.</span></span>
<span data-ttu-id="830c8-114">Du kan registrera dig för ett konto sina [plats](https://app.sysdigcloud.com).</span><span class="sxs-lookup"><span data-stu-id="830c8-114">You can sign up for an account on their [site](https://app.sysdigcloud.com).</span></span>

<span data-ttu-id="830c8-115">När du är inloggad på toohello Sysdig moln webbplats klickar du på ditt användarnamn och på sidan hello bör du se din ”åtkomstnyckeln”.</span><span class="sxs-lookup"><span data-stu-id="830c8-115">Once you're logged in toohello Sysdig cloud website, click on your user name, and on hello page you should see your "Access Key."</span></span> 

![API-nyckel för Sysdig](./media/container-service-kubernetes-sysdig/sysdig2.png)

## <a name="installing-hello-sysdig-agents-tookubernetes"></a><span data-ttu-id="830c8-117">Installera hello Sysdig agenter tooKubernetes</span><span class="sxs-lookup"><span data-stu-id="830c8-117">Installing hello Sysdig agents tooKubernetes</span></span>
<span data-ttu-id="830c8-118">toomonitor behållare Sysdig kör en process på varje dator med hjälp av en Kubernetes `DaemonSet`.</span><span class="sxs-lookup"><span data-stu-id="830c8-118">toomonitor your containers, Sysdig runs a process on each machine using a Kubernetes `DaemonSet`.</span></span>
<span data-ttu-id="830c8-119">DaemonSets är Kubernetes API-objekt som kör en instans av en behållare per dator.</span><span class="sxs-lookup"><span data-stu-id="830c8-119">DaemonSets are Kubernetes API objects that run a single instance of a container per machine.</span></span>
<span data-ttu-id="830c8-120">Det är perfekt för att installera verktyg som hello Sysdigs övervakningsagenten.</span><span class="sxs-lookup"><span data-stu-id="830c8-120">They're perfect for installing tools like hello Sysdig's monitoring agent.</span></span>

<span data-ttu-id="830c8-121">tooinstall hello Sysdig daemonset bör du först hämta [hello mallen](https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml) från sysdig.</span><span class="sxs-lookup"><span data-stu-id="830c8-121">tooinstall hello Sysdig daemonset, you should first download [hello template](https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml) from sysdig.</span></span> <span data-ttu-id="830c8-122">Spara filen som `sysdig-daemonset.yaml`.</span><span class="sxs-lookup"><span data-stu-id="830c8-122">Save that file as `sysdig-daemonset.yaml`.</span></span>

<span data-ttu-id="830c8-123">Du kan köra på Linux och OS X:</span><span class="sxs-lookup"><span data-stu-id="830c8-123">On Linux and OS X you can run:</span></span>

```console
$ curl -O https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml
```

<span data-ttu-id="830c8-124">I PowerShell:</span><span class="sxs-lookup"><span data-stu-id="830c8-124">In PowerShell:</span></span>

```console
$ Invoke-WebRequest -Uri https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml | Select-Object -ExpandProperty Content > sysdig-daemonset.yaml
```

<span data-ttu-id="830c8-125">Redigera bredvid den filen tooinsert snabbtangent, som du fick från ditt konto Sysdig.</span><span class="sxs-lookup"><span data-stu-id="830c8-125">Next edit that file tooinsert your Access Key, that you obtained from your Sysdig account.</span></span>

<span data-ttu-id="830c8-126">Skapa slutligen hello DaemonSet:</span><span class="sxs-lookup"><span data-stu-id="830c8-126">Finally, create hello DaemonSet:</span></span>

```console
$ kubectl create -f sysdig-daemonset.yaml
```

## <a name="view-your-monitoring"></a><span data-ttu-id="830c8-127">Visa övervakningen</span><span class="sxs-lookup"><span data-stu-id="830c8-127">View your monitoring</span></span>
<span data-ttu-id="830c8-128">När du installerade och körs, bör hello agenter pump data tillbaka tooSysdig.</span><span class="sxs-lookup"><span data-stu-id="830c8-128">Once installed and running, hello agents should pump data back tooSysdig.</span></span>  <span data-ttu-id="830c8-129">Gå tillbaka toothe [sysdig instrumentpanelen](https://app.sysdigcloud.com) och du bör se information om din behållare.</span><span class="sxs-lookup"><span data-stu-id="830c8-129">Go back toothe [sysdig dashboard](https://app.sysdigcloud.com) and you should see information about your containers.</span></span>

<span data-ttu-id="830c8-130">Du kan också installera Kubernetes-specifika instrumentpaneler via den [guiden Ny instrumentpanel](https://app.sysdigcloud.com/#/dashboards/new).</span><span class="sxs-lookup"><span data-stu-id="830c8-130">You can also install Kubernetes-specific dashboards via the [new dashboard wizard](https://app.sysdigcloud.com/#/dashboards/new).</span></span>
