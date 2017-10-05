---
title: "Övervaka ett Azure Container Service-kluster med Sysdig | Microsoft Docs"
description: "Övervaka ett Azure Container Service-kluster med Sysdig."
services: container-service
documentationcenter: 
author: sauryadas
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Behållare, DC/OS, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2016
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: e61001161e632a5d2e513107e30f1eaf06103989
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-an-azure-container-service-cluster-with-sysdig"></a><span data-ttu-id="d4b5f-104">Övervaka ett Azure Container Service-kluster med Sysdig</span><span class="sxs-lookup"><span data-stu-id="d4b5f-104">Monitor an Azure Container Service cluster with Sysdig</span></span>
<span data-ttu-id="d4b5f-105">I den här artikeln ska vi distribuera Sysdig-agenter till alla agentnoder i ditt Azure Container Service-kluster.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-105">In this article, we will deploy Sysdig agents to all the agent nodes in your Azure Container Service cluster.</span></span> <span data-ttu-id="d4b5f-106">Du behöver ett konto hos Sysdig för den här konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-106">You need an account with Sysdig for this configuration.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="d4b5f-107">Krav</span><span class="sxs-lookup"><span data-stu-id="d4b5f-107">Prerequisites</span></span>
<span data-ttu-id="d4b5f-108">[Distribuera](container-service-deployment.md) och [anslut](../container-service-connect.md) ett kluster som konfigurerats av Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-108">[Deploy](container-service-deployment.md) and [connect](../container-service-connect.md) a cluster configured by Azure Container Service.</span></span> <span data-ttu-id="d4b5f-109">Utforska [Marathon-gränssnittet](container-service-mesos-marathon-ui.md).</span><span class="sxs-lookup"><span data-stu-id="d4b5f-109">Explore the [Marathon UI](container-service-mesos-marathon-ui.md).</span></span> <span data-ttu-id="d4b5f-110">Gå till [http://app.sysdigcloud.com](http://app.sysdigcloud.com) för att ställa in ett Sysdig-molnkonto.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-110">Go to [http://app.sysdigcloud.com](http://app.sysdigcloud.com) to set up a Sysdig cloud account.</span></span> 

## <a name="sysdig"></a><span data-ttu-id="d4b5f-111">Sysdig</span><span class="sxs-lookup"><span data-stu-id="d4b5f-111">Sysdig</span></span>
<span data-ttu-id="d4b5f-112">Sysdig är en övervakningstjänst som gör att du kan övervaka dina behållare i klustret.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-112">Sysdig is a monitoring service that allows you to monitor your containers within your cluster.</span></span> <span data-ttu-id="d4b5f-113">Sysdig är kända för att hjälpa till med felsökning men innehåller även grundläggande övervakningsmätvärden för processor, nätverk, minne och I/O.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-113">Sysdig is known to help with troubleshooting but it also has your basic monitoring metrics for CPU, Networking, Memory, and I/O.</span></span> <span data-ttu-id="d4b5f-114">Sysdig gör det enkelt att se vilka behållare som arbetar hårdast eller använder mest minne och CPU.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-114">Sysdig makes it easy to see which containers are working the hardest or essentially using the most memory and CPU.</span></span> <span data-ttu-id="d4b5f-115">Den här vyn finns i avsnittet ”Översikt”, som för närvarande är en betaversion.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-115">This view is in the “Overview” section, which is currently in beta.</span></span> 

![Sysdig-gränssnitt](./media/container-service-monitoring-sysdig/sysdig6.png) 

## <a name="configure-a-sysdig-deployment-with-marathon"></a><span data-ttu-id="d4b5f-117">Konfigurera en Sysdig-distribution med Marathon</span><span class="sxs-lookup"><span data-stu-id="d4b5f-117">Configure a Sysdig deployment with Marathon</span></span>
<span data-ttu-id="d4b5f-118">Här visas hur du konfigurerar och distribuerar Sysdig-program till klustret med Marathon.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-118">These steps will show you how to configure and deploy Sysdig applications to your cluster with Marathon.</span></span> 

<span data-ttu-id="d4b5f-119">Gå till ditt DC/OS-gränssnittet via [http://localhost:80/](http://localhost:80/) När du kommer till DC/OS-användargränssnittet navigerar du till ”Universe”, som finns längst ned till vänster och söker sedan efter ”Sysdig.”</span><span class="sxs-lookup"><span data-stu-id="d4b5f-119">Access your DC/OS UI via [http://localhost:80/](http://localhost:80/) Once in the DC/OS UI navigate to the "Universe", which is on the bottom left and then search for "Sysdig."</span></span>

![Sysdig i DC/OS Universe](./media/container-service-monitoring-sysdig/sysdig1.png)

<span data-ttu-id="d4b5f-121">Nu för att slutföra konfigurationen behöver du ett Sysdig-molnkonto eller ett kostnadsfritt utvärderingskonto.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-121">Now to complete the configuration you need a Sysdig cloud account or a free trial account.</span></span> <span data-ttu-id="d4b5f-122">När du har loggat in på Sysdig-molnwebbplatsen klickar du på ditt användarnamn. Din ”snabbtangent” bör nu visas på sidan.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-122">Once you're logged in to the Sysdig cloud website, click on your user name, and on the page you should see your "Access Key."</span></span> 

![API-nyckel för Sysdig](./media/container-service-monitoring-sysdig/sysdig2.png) 

<span data-ttu-id="d4b5f-124">Ange härnäst din snabbtangent i Sysdig-konfigurationen i DC/OS Universe.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-124">Next enter your Access Key into the Sysdig configuration within the DC/OS Universe.</span></span> 

![Sysdig-konfigurationen i DC/OS Universe](./media/container-service-monitoring-sysdig/sysdig3.png)

<span data-ttu-id="d4b5f-126">Nu anger du instanserna till 10000000 så när du har lagt till en ny nod i klustret, kommer Sysdig automatiskt att distribuera en agent till den nya noden.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-126">Now set the instances to 10000000 so whenever a new node is added to the cluster Sysdig will automatically deploy an agent to that new node.</span></span> <span data-ttu-id="d4b5f-127">Det här är en tillfällig lösning för att kontrollera att Sysdig distribueras till alla nya agenter i klustret.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-127">This is an interim solution to make sure Sysdig will deploy to all new agents within the cluster.</span></span> 

![Sysdig-konfigurationen i DC/OS Universe-instanser](./media/container-service-monitoring-sysdig/sysdig4.png)

<span data-ttu-id="d4b5f-129">När du har installerat paketet navigerar du tillbaka till Sysdig-gränssnittet. Nu kan du utforska de olika användningsmätvärdena för behållarna i klustret.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-129">Once you've installed the package navigate back to the Sysdig UI and you'll be able to explore the different usage metrics for the containers within your cluster.</span></span> 

<span data-ttu-id="d4b5f-130">Du kan även installera Mesos- och Marathon-specifika instrumentpaneler via [den nya instrumentpanelsguiden](https://app.sysdigcloud.com/#/dashboards/new).</span><span class="sxs-lookup"><span data-stu-id="d4b5f-130">You can also install Mesos and Marathon specific dashboards via the [new dashboard wizard](https://app.sysdigcloud.com/#/dashboards/new).</span></span>
