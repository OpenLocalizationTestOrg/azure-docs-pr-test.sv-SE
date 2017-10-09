---
title: aaaMonitor Azure DC/OS-kluster - Datadog | Microsoft Docs
description: "Övervaka ett Azure Container Service-kluster med Datadog. Använda hello DC/OS web UI toodeploy hello Datadog agenter tooyour kluster."
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
ms.openlocfilehash: 10268c04b5c2ef393429e706ed4a467fff80f718
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-an-azure-container-service-dcos-cluster-with-datadog"></a><span data-ttu-id="2a557-105">Övervaka ett Azure Container Service DC/OS-kluster med Datadog</span><span class="sxs-lookup"><span data-stu-id="2a557-105">Monitor an Azure Container Service DC/OS cluster with Datadog</span></span>
<span data-ttu-id="2a557-106">I den här artikeln ska vi distribuera Datadog agenter tooall hello agent noder i Azure Container Service-kluster.</span><span class="sxs-lookup"><span data-stu-id="2a557-106">In this article we will deploy Datadog agents tooall hello agent nodes in your Azure Container Service cluster.</span></span> <span data-ttu-id="2a557-107">Du behöver ett konto med Datadog för den här konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="2a557-107">You will need an account with Datadog for this configuration.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="2a557-108">Krav</span><span class="sxs-lookup"><span data-stu-id="2a557-108">Prerequisites</span></span>
<span data-ttu-id="2a557-109">[Distribuera](container-service-deployment.md) och [anslut](../container-service-connect.md) ett kluster som konfigurerats av Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="2a557-109">[Deploy](container-service-deployment.md) and [connect](../container-service-connect.md) a cluster configured by Azure Container Service.</span></span> <span data-ttu-id="2a557-110">Utforska hello [Marathon-Gränssnittet](container-service-mesos-marathon-ui.md).</span><span class="sxs-lookup"><span data-stu-id="2a557-110">Explore hello [Marathon UI](container-service-mesos-marathon-ui.md).</span></span> <span data-ttu-id="2a557-111">Gå för[http://datadoghq.com](http://datadoghq.com) tooset en Datadog-konto.</span><span class="sxs-lookup"><span data-stu-id="2a557-111">Go too[http://datadoghq.com](http://datadoghq.com) tooset up a Datadog account.</span></span> 

## <a name="datadog"></a><span data-ttu-id="2a557-112">Datadog</span><span class="sxs-lookup"><span data-stu-id="2a557-112">Datadog</span></span>
<span data-ttu-id="2a557-113">Datadog är en övervakningstjänsten som samlar in övervakningsdata från din behållare i Azure Container Service-kluster.</span><span class="sxs-lookup"><span data-stu-id="2a557-113">Datadog is a monitoring service that gathers monitoring data from your containers within your Azure Container Service cluster.</span></span> <span data-ttu-id="2a557-114">Datadog har en Docker-integrering instrumentpanel där du kan se specifika mått i en behållare.</span><span class="sxs-lookup"><span data-stu-id="2a557-114">Datadog has a Docker Integration Dashboard where you can see specific metrics within your containers.</span></span> <span data-ttu-id="2a557-115">Mätvärden som samlats in från behållarna ordnas efter CPU, minne, nätverk och i/o.</span><span class="sxs-lookup"><span data-stu-id="2a557-115">Metrics gathered from your containers are organized by CPU, Memory, Network and I/O.</span></span> <span data-ttu-id="2a557-116">Datadog delar mått i behållare och bilder.</span><span class="sxs-lookup"><span data-stu-id="2a557-116">Datadog splits metrics into containers and images.</span></span> <span data-ttu-id="2a557-117">Ett exempel på vilka hello UI verkar för CPU-användningen är nedan.</span><span class="sxs-lookup"><span data-stu-id="2a557-117">An example of what hello UI looks like for CPU usage is below.</span></span>

![Datadog UI](./media/container-service-monitoring/datadog4.png)

## <a name="configure-a-datadog-deployment-with-marathon"></a><span data-ttu-id="2a557-119">Konfigurera Datadog distribution med Marathon</span><span class="sxs-lookup"><span data-stu-id="2a557-119">Configure a Datadog deployment with Marathon</span></span>
<span data-ttu-id="2a557-120">Dessa steg visar hur tooconfigure och distribuera Datadog program tooyour kluster med Marathon.</span><span class="sxs-lookup"><span data-stu-id="2a557-120">These steps will show you how tooconfigure and deploy Datadog applications tooyour cluster with Marathon.</span></span> 

<span data-ttu-id="2a557-121">Åtkomst till ditt DC/OS-Gränssnittet via [http://localhost:80 /](http://localhost:80/).</span><span class="sxs-lookup"><span data-stu-id="2a557-121">Access your DC/OS UI via [http://localhost:80/](http://localhost:80/).</span></span> <span data-ttu-id="2a557-122">En gång i hello DC/OS-Gränssnittet navigera toohello ”universum” som finns på hello nedre vänster och sedan söka efter ”Datadog” och klicka på ”installera”.</span><span class="sxs-lookup"><span data-stu-id="2a557-122">Once in hello DC/OS UI navigate toohello "Universe" which is on hello bottom left and then search for "Datadog" and click "Install."</span></span>

![Datadog paketet i hello DC/OS Universe](./media/container-service-monitoring/datadog1.png)

<span data-ttu-id="2a557-124">Nu toocomplete hello konfigurationen behöver du ett Datadog konto eller ett kostnadsfritt utvärderingskonto.</span><span class="sxs-lookup"><span data-stu-id="2a557-124">Now toocomplete hello configuration you will need a Datadog account or a free trial account.</span></span> <span data-ttu-id="2a557-125">När du är inloggad titta i toohello Datadog webbplats toohello vänster och gå sedan-tooIntegrations > [API: er](https://app.datadoghq.com/account/settings#api).</span><span class="sxs-lookup"><span data-stu-id="2a557-125">Once you're logged in toohello Datadog website look toohello left and go tooIntegrations -> then [APIs](https://app.datadoghq.com/account/settings#api).</span></span> 

![Datadog API-nyckel](./media/container-service-monitoring/datadog2.png)

<span data-ttu-id="2a557-127">Bredvid ange API-nyckel i hello Datadog konfiguration i hello DC/OS Universe.</span><span class="sxs-lookup"><span data-stu-id="2a557-127">Next enter your API key into hello Datadog configuration within hello DC/OS Universe.</span></span> 

![Datadog konfigurationen i hello DC/OS Universe](./media/container-service-monitoring/datadog3.png) 

<span data-ttu-id="2a557-129">I hello ovan configuration anges instanser too10000000 så när en ny nod läggs toohello klustret Datadog automatiskt distribuerar en agent toothat nod.</span><span class="sxs-lookup"><span data-stu-id="2a557-129">In hello above configuration instances are set too10000000 so whenever a new node is added toohello cluster Datadog will automatically deploy an agent toothat node.</span></span> <span data-ttu-id="2a557-130">Det här är en tillfällig lösning.</span><span class="sxs-lookup"><span data-stu-id="2a557-130">This is an interim solution.</span></span> <span data-ttu-id="2a557-131">När du har installerat hello paketet ska du gå tillbaka toohello Datadog webbplats och hitta ”[instrumentpaneler](https://app.datadoghq.com/dash/list)”.</span><span class="sxs-lookup"><span data-stu-id="2a557-131">Once you've installed hello package you should navigate back toohello Datadog website and find "[Dashboards](https://app.datadoghq.com/dash/list)."</span></span> <span data-ttu-id="2a557-132">Därifrån ser anpassad och integrering instrumentpaneler.</span><span class="sxs-lookup"><span data-stu-id="2a557-132">From there you will see Custom and Integration Dashboards.</span></span> <span data-ttu-id="2a557-133">Hej [Docker instrumentpanelen](https://app.datadoghq.com/screen/integration/docker) har alla hello behållaren mått som du behöver för att övervaka ditt kluster.</span><span class="sxs-lookup"><span data-stu-id="2a557-133">hello [Docker dashboard](https://app.datadoghq.com/screen/integration/docker) will have all hello container metrics you need for monitoring your cluster.</span></span> 

