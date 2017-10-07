---
title: "aaaIntroduction tooAzure Container Service för Kubernetes | Microsoft Docs"
description: "Azure Container Service för Kubernetes gör det enkelt toodeploy och hantera behållare-baserade program på Azure."
services: container-service
documentationcenter: 
author: gabrtv
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Kubernetes, Docker, Containers, Microservices, Azure
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/21/2017
ms.author: gamonroy
ms.custom: mvc
ms.openlocfilehash: bfc85a180bdf4a405c9047eb882d3eed01640dd1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooazure-container-service-for-kubernetes"></a><span data-ttu-id="fa34c-104">Introduktion tooAzure Container Service för Kubernetes</span><span class="sxs-lookup"><span data-stu-id="fa34c-104">Introduction tooAzure Container Service for Kubernetes</span></span>
<span data-ttu-id="fa34c-105">Azure Container Service för Kubernetes gör det enkelt toocreate, konfigurera och hantera kluster för virtuella datorer som är förkonfigurerad toorun av program.</span><span class="sxs-lookup"><span data-stu-id="fa34c-105">Azure Container Service for Kubernetes makes it simple toocreate, configure, and manage a cluster of virtual machines that are preconfigured toorun containerized applications.</span></span> <span data-ttu-id="fa34c-106">Detta gör att du toouse dina befintliga kunskaper eller rita på en stor och växande uppsättning community kunskaper, toodeploy och hantera behållare-baserade program i Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="fa34c-106">This enables you toouse your existing skills, or draw upon a large and growing body of community expertise, toodeploy and manage container-based applications on Microsoft Azure.</span></span>

<span data-ttu-id="fa34c-107">Med Azure Container Service kan du dra nytta av hello företagsklass funktioner i Azure, men har ändå programmet överföring via Kubernetes och hello Docker bildformat.</span><span class="sxs-lookup"><span data-stu-id="fa34c-107">By using Azure Container Service, you can take advantage of hello enterprise-grade features of Azure, while still maintaining application portability through Kubernetes and hello Docker image format.</span></span>

## <a name="using-azure-container-service-for-kubernetes"></a><span data-ttu-id="fa34c-108">Använda Azure Container Service för Kubernetes</span><span class="sxs-lookup"><span data-stu-id="fa34c-108">Using Azure Container Service for Kubernetes</span></span>
<span data-ttu-id="fa34c-109">Vårt mål med Azure Container Service är tooprovide värdmiljön en behållare med öppen källkod verktyg och tekniker som är populärt bland våra kunder idag.</span><span class="sxs-lookup"><span data-stu-id="fa34c-109">Our goal with Azure Container Service is tooprovide a container hosting environment by using open-source tools and technologies that are popular among our customers today.</span></span> <span data-ttu-id="fa34c-110">toothis slutet kan du använda hello standard Kubernetes API-slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="fa34c-110">toothis end, we expose hello standard Kubernetes API endpoints.</span></span> <span data-ttu-id="fa34c-111">Du kan utnyttja all programvara som kan prata tooa Kubernetes kluster med hjälp av dessa slutpunkter som standard.</span><span class="sxs-lookup"><span data-stu-id="fa34c-111">By using these standard endpoints, you can leverage any software that is capable of talking tooa Kubernetes cluster.</span></span> <span data-ttu-id="fa34c-112">Du kan till exempel välja [kubectl](https://kubernetes.io/docs/user-guide/kubectl-overview/), [helm](https://helm.sh/) eller [draft](https://github.com/Azure/draft).</span><span class="sxs-lookup"><span data-stu-id="fa34c-112">For example, you might choose [kubectl](https://kubernetes.io/docs/user-guide/kubectl-overview/), [helm](https://helm.sh/), or [draft](https://github.com/Azure/draft).</span></span>

## <a name="creating-a-kubernetes-cluster-using-azure-container-service"></a><span data-ttu-id="fa34c-113">Skapa ett Kubernetes-kluster med Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="fa34c-113">Creating a Kubernetes cluster using Azure Container Service</span></span>
<span data-ttu-id="fa34c-114">toobegin med Azure Container Service distribuera ett Azure Container Service-kluster med hello [Azure CLI 2.0](container-service-kubernetes-walkthrough.md) eller via hello-portal (Sök hello Marketplace för **Azure Container Service**).</span><span class="sxs-lookup"><span data-stu-id="fa34c-114">toobegin using Azure Container Service, deploy an Azure Container Service cluster with hello [Azure CLI 2.0](container-service-kubernetes-walkthrough.md) or via hello portal (search hello Marketplace for **Azure Container Service**).</span></span> <span data-ttu-id="fa34c-115">Om du är en avancerad användare som behöver mer kontroll över hello Azure Resource Manager-mallar kan du använda hello öppen källkod [acs-motorn](https://github.com/Azure/acs-engine) projekt toobuild egna anpassade Kubernetes kluster och distribuera det via hello `az` CLI.</span><span class="sxs-lookup"><span data-stu-id="fa34c-115">If you are an advanced user who needs more control over hello Azure Resource Manager templates, you can use hello open source [acs-engine](https://github.com/Azure/acs-engine) project toobuild your own custom Kubernetes cluster and deploy it via hello `az` CLI.</span></span>

### <a name="using-kubernetes"></a><span data-ttu-id="fa34c-116">Använda Kubernetes</span><span class="sxs-lookup"><span data-stu-id="fa34c-116">Using Kubernetes</span></span>
<span data-ttu-id="fa34c-117">Kubernetes automatiserar distributionen, skalningen och hanteringen av program som använder behållare.</span><span class="sxs-lookup"><span data-stu-id="fa34c-117">Kubernetes automates deployment, scaling, and management of containerized applications.</span></span> <span data-ttu-id="fa34c-118">Det har en omfattande uppsättning funktioner. Till exempel:</span><span class="sxs-lookup"><span data-stu-id="fa34c-118">It has a rich set of features including:</span></span>
* <span data-ttu-id="fa34c-119">Automatisk paketering</span><span class="sxs-lookup"><span data-stu-id="fa34c-119">Automatic binpacking</span></span>
* <span data-ttu-id="fa34c-120">Självåterställning</span><span class="sxs-lookup"><span data-stu-id="fa34c-120">Self-healing</span></span>
* <span data-ttu-id="fa34c-121">Horisontell skalning</span><span class="sxs-lookup"><span data-stu-id="fa34c-121">Horizontal scaling</span></span>
* <span data-ttu-id="fa34c-122">Tjänstidentifiering och belastningsutjämning</span><span class="sxs-lookup"><span data-stu-id="fa34c-122">Service discovery and load balancing</span></span>
* <span data-ttu-id="fa34c-123">Automatiserade distributioner och återställningar</span><span class="sxs-lookup"><span data-stu-id="fa34c-123">Automated rollouts and rollbacks</span></span>
* <span data-ttu-id="fa34c-124">Hemlighets- och konfigurationshantering</span><span class="sxs-lookup"><span data-stu-id="fa34c-124">Secret and configuration management</span></span>
* <span data-ttu-id="fa34c-125">Storage-dirigering</span><span class="sxs-lookup"><span data-stu-id="fa34c-125">Storage orchestration</span></span>
* <span data-ttu-id="fa34c-126">Batch-körning</span><span class="sxs-lookup"><span data-stu-id="fa34c-126">Batch execution</span></span>

<span data-ttu-id="fa34c-127">Arkitekturdiagram över Kubernetes som distribuerats via Azure Container Service:</span><span class="sxs-lookup"><span data-stu-id="fa34c-127">Architectural diagram of Kubernetes deployed via Azure Container Service:</span></span>

![Azure Container Service konfigurerad toouse Kubernetes.](media/acs-intro/kubernetes.png)

## <a name="videos"></a><span data-ttu-id="fa34c-129">Videoklipp</span><span class="sxs-lookup"><span data-stu-id="fa34c-129">Videos</span></span>

<span data-ttu-id="fa34c-130">Stöd för Kubernetes i Azure Container Service (Azure fredag, januari 2017):</span><span class="sxs-lookup"><span data-stu-id="fa34c-130">Kubernetes Support in Azure Container Services (Azure Friday, January 2017):</span></span>

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Kubernetes-Support-in-Azure-Container-Services/player]
>
>

<span data-ttu-id="fa34c-131">Verktyg för utveckling och distribution av program på Kubernetes (Azure OpenDev, juni 2017):</span><span class="sxs-lookup"><span data-stu-id="fa34c-131">Tools for Developing and Deploying Applications on Kubernetes (Azure OpenDev, June 2017):</span></span>

> [!VIDEO https://channel9.msdn.com/Events/AzureOpenDev/June2017/Tools-for-Developing-and-Deploying-Applications-on-Kubernetes/player]
>
>

## <a name="next-steps"></a><span data-ttu-id="fa34c-132">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fa34c-132">Next steps</span></span>

<span data-ttu-id="fa34c-133">Utforska hello [Kubernetes Quickstart](container-service-kubernetes-walkthrough.md) toobegin utforska Azure Container Service idag.</span><span class="sxs-lookup"><span data-stu-id="fa34c-133">Explore hello [Kubernetes Quickstart](container-service-kubernetes-walkthrough.md) toobegin exploring Azure Container Service today.</span></span>
