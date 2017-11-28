---
title: "Övervaka Azure DC/OS-kluster - Datadog | Microsoft Docs"
description: "Övervaka ett Azure Container Service-kluster med Datadog. Du kan använda webbgränssnittet för DC/OS för att distribuera Datadog agenter i klustret."
services: container-service
documentationcenter: 
author: sauryadas
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Behållare med DC/OS, Docker Swarm Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure
ms.date: 07/28/2016
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: 9dd451f994940d7cc3a59bd7fd08a8f067345e34
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-an-azure-container-service-dcos-cluster-with-datadog"></a><span data-ttu-id="7d8dd-105">Övervaka ett Azure Container Service DC/OS-kluster med Datadog</span><span class="sxs-lookup"><span data-stu-id="7d8dd-105">Monitor an Azure Container Service DC/OS cluster with Datadog</span></span>
<span data-ttu-id="7d8dd-106">I den här artikeln ska vi distribuera Datadog agenter för alla agent-noder i Azure Container Service-kluster.</span><span class="sxs-lookup"><span data-stu-id="7d8dd-106">In this article we will deploy Datadog agents to all the agent nodes in your Azure Container Service cluster.</span></span> <span data-ttu-id="7d8dd-107">Du behöver ett konto med Datadog för den här konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="7d8dd-107">You will need an account with Datadog for this configuration.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="7d8dd-108">Krav</span><span class="sxs-lookup"><span data-stu-id="7d8dd-108">Prerequisites</span></span>
<span data-ttu-id="7d8dd-109">[Distribuera](container-service-deployment.md) och [anslut](../container-service-connect.md) ett kluster som konfigurerats av Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="7d8dd-109">[Deploy](container-service-deployment.md) and [connect](../container-service-connect.md) a cluster configured by Azure Container Service.</span></span> <span data-ttu-id="7d8dd-110">Utforska [Marathon-gränssnittet](container-service-mesos-marathon-ui.md).</span><span class="sxs-lookup"><span data-stu-id="7d8dd-110">Explore the [Marathon UI](container-service-mesos-marathon-ui.md).</span></span> <span data-ttu-id="7d8dd-111">Gå till [http://datadoghq.com](http://datadoghq.com) att ställa in ett Datadog-konto.</span><span class="sxs-lookup"><span data-stu-id="7d8dd-111">Go to [http://datadoghq.com](http://datadoghq.com) to set up a Datadog account.</span></span> 

## <a name="datadog"></a><span data-ttu-id="7d8dd-112">Datadog</span><span class="sxs-lookup"><span data-stu-id="7d8dd-112">Datadog</span></span>
<span data-ttu-id="7d8dd-113">Datadog är en övervakningstjänsten som samlar in övervakningsdata från din behållare i Azure Container Service-kluster.</span><span class="sxs-lookup"><span data-stu-id="7d8dd-113">Datadog is a monitoring service that gathers monitoring data from your containers within your Azure Container Service cluster.</span></span> <span data-ttu-id="7d8dd-114">Datadog har en Docker-integrering instrumentpanel där du kan se specifika mått i en behållare.</span><span class="sxs-lookup"><span data-stu-id="7d8dd-114">Datadog has a Docker Integration Dashboard where you can see specific metrics within your containers.</span></span> <span data-ttu-id="7d8dd-115">Mätvärden som samlats in från behållarna ordnas efter CPU, minne, nätverk och i/o.</span><span class="sxs-lookup"><span data-stu-id="7d8dd-115">Metrics gathered from your containers are organized by CPU, Memory, Network and I/O.</span></span> <span data-ttu-id="7d8dd-116">Datadog delar mått i behållare och bilder.</span><span class="sxs-lookup"><span data-stu-id="7d8dd-116">Datadog splits metrics into containers and images.</span></span> <span data-ttu-id="7d8dd-117">Det är ett exempel på hur Gränssnittet ser ut CPU-användning under.</span><span class="sxs-lookup"><span data-stu-id="7d8dd-117">An example of what the UI looks like for CPU usage is below.</span></span>

![Datadog UI](./media/container-service-monitoring/datadog4.png)

## <a name="configure-a-datadog-deployment-with-marathon"></a><span data-ttu-id="7d8dd-119">Konfigurera Datadog distribution med Marathon</span><span class="sxs-lookup"><span data-stu-id="7d8dd-119">Configure a Datadog deployment with Marathon</span></span>
<span data-ttu-id="7d8dd-120">Dessa steg visar hur du konfigurerar och distribuerar program för Datadog till ditt kluster med Marathon.</span><span class="sxs-lookup"><span data-stu-id="7d8dd-120">These steps will show you how to configure and deploy Datadog applications to your cluster with Marathon.</span></span> 

<span data-ttu-id="7d8dd-121">Åtkomst till ditt DC/OS-Gränssnittet via [http://localhost:80 /](http://localhost:80/).</span><span class="sxs-lookup"><span data-stu-id="7d8dd-121">Access your DC/OS UI via [http://localhost:80/](http://localhost:80/).</span></span> <span data-ttu-id="7d8dd-122">En gång i DC/OS-Gränssnittet går du till ”universum” som finns på ned till vänster och sedan söka efter ”Datadog” och klicka på ”installera”.</span><span class="sxs-lookup"><span data-stu-id="7d8dd-122">Once in the DC/OS UI navigate to the "Universe" which is on the bottom left and then search for "Datadog" and click "Install."</span></span>

![Datadog paketet i DC/OS-Universe](./media/container-service-monitoring/datadog1.png)

<span data-ttu-id="7d8dd-124">Nu för att slutföra konfigurationen behöver du ett Datadog konto eller ett kostnadsfritt utvärderingskonto.</span><span class="sxs-lookup"><span data-stu-id="7d8dd-124">Now to complete the configuration you will need a Datadog account or a free trial account.</span></span> <span data-ttu-id="7d8dd-125">När du är inloggad utseendet på webbplatsen Datadog till vänster och gå till integreringar -> sedan [API: er](https://app.datadoghq.com/account/settings#api).</span><span class="sxs-lookup"><span data-stu-id="7d8dd-125">Once you're logged in to the Datadog website look to the left and go to Integrations -> then [APIs](https://app.datadoghq.com/account/settings#api).</span></span> 

![Datadog API-nyckel](./media/container-service-monitoring/datadog2.png)

<span data-ttu-id="7d8dd-127">Bredvid ange API-nyckel i Datadog konfigurationen i DC/OS-Universe.</span><span class="sxs-lookup"><span data-stu-id="7d8dd-127">Next enter your API key into the Datadog configuration within the DC/OS Universe.</span></span> 

![Datadog konfigurationen i DC/OS-Universe](./media/container-service-monitoring/datadog3.png) 

<span data-ttu-id="7d8dd-129">I konfigurationen ovan instanser är inställda på att 10000000 så när en ny nod läggs till i klustret Datadog automatiskt distribuerar en agent till noden.</span><span class="sxs-lookup"><span data-stu-id="7d8dd-129">In the above configuration instances are set to 10000000 so whenever a new node is added to the cluster Datadog will automatically deploy an agent to that node.</span></span> <span data-ttu-id="7d8dd-130">Det här är en tillfällig lösning.</span><span class="sxs-lookup"><span data-stu-id="7d8dd-130">This is an interim solution.</span></span> <span data-ttu-id="7d8dd-131">När du har installerat paketet ska du gå tillbaka till webbplatsen Datadog och hitta ”[instrumentpaneler](https://app.datadoghq.com/dash/list)”.</span><span class="sxs-lookup"><span data-stu-id="7d8dd-131">Once you've installed the package you should navigate back to the Datadog website and find "[Dashboards](https://app.datadoghq.com/dash/list)."</span></span> <span data-ttu-id="7d8dd-132">Därifrån ser anpassad och integrering instrumentpaneler.</span><span class="sxs-lookup"><span data-stu-id="7d8dd-132">From there you will see Custom and Integration Dashboards.</span></span> <span data-ttu-id="7d8dd-133">Den [Docker instrumentpanelen](https://app.datadoghq.com/screen/integration/docker) har alla behållare mått som du behöver för att övervaka ditt kluster.</span><span class="sxs-lookup"><span data-stu-id="7d8dd-133">The [Docker dashboard](https://app.datadoghq.com/screen/integration/docker) will have all the container metrics you need for monitoring your cluster.</span></span> 

