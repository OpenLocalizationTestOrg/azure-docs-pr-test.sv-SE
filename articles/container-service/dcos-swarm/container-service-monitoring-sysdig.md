---
title: aaaMonitor ett Azure Container Service-kluster med Sysdig | Microsoft Docs
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
ms.openlocfilehash: 72f2d3d6f6885f9876fa158b88aae58b84a4610f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-an-azure-container-service-cluster-with-sysdig"></a><span data-ttu-id="dabef-104">Övervaka ett Azure Container Service-kluster med Sysdig</span><span class="sxs-lookup"><span data-stu-id="dabef-104">Monitor an Azure Container Service cluster with Sysdig</span></span>
<span data-ttu-id="dabef-105">I den här artikeln ska vi distribuera Sysdig agenter tooall hello agent noder i Azure Container Service-kluster.</span><span class="sxs-lookup"><span data-stu-id="dabef-105">In this article, we will deploy Sysdig agents tooall hello agent nodes in your Azure Container Service cluster.</span></span> <span data-ttu-id="dabef-106">Du behöver ett konto hos Sysdig för den här konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="dabef-106">You need an account with Sysdig for this configuration.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="dabef-107">Krav</span><span class="sxs-lookup"><span data-stu-id="dabef-107">Prerequisites</span></span>
<span data-ttu-id="dabef-108">[Distribuera](container-service-deployment.md) och [anslut](../container-service-connect.md) ett kluster som konfigurerats av Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="dabef-108">[Deploy](container-service-deployment.md) and [connect](../container-service-connect.md) a cluster configured by Azure Container Service.</span></span> <span data-ttu-id="dabef-109">Utforska hello [Marathon-Gränssnittet](container-service-mesos-marathon-ui.md).</span><span class="sxs-lookup"><span data-stu-id="dabef-109">Explore hello [Marathon UI](container-service-mesos-marathon-ui.md).</span></span> <span data-ttu-id="dabef-110">Gå för[http://app.sysdigcloud.com](http://app.sysdigcloud.com) tooset en Sysdig moln-konto.</span><span class="sxs-lookup"><span data-stu-id="dabef-110">Go too[http://app.sysdigcloud.com](http://app.sysdigcloud.com) tooset up a Sysdig cloud account.</span></span> 

## <a name="sysdig"></a><span data-ttu-id="dabef-111">Sysdig</span><span class="sxs-lookup"><span data-stu-id="dabef-111">Sysdig</span></span>
<span data-ttu-id="dabef-112">Sysdig är en övervakningstjänsten som gör att du toomonitor din behållare i klustret.</span><span class="sxs-lookup"><span data-stu-id="dabef-112">Sysdig is a monitoring service that allows you toomonitor your containers within your cluster.</span></span> <span data-ttu-id="dabef-113">Sysdig är känt toohelp med felsökning har bara din grundläggande övervakning mått för CPU-, nätverks-, minnes- och i/o.</span><span class="sxs-lookup"><span data-stu-id="dabef-113">Sysdig is known toohelp with troubleshooting but it also has your basic monitoring metrics for CPU, Networking, Memory, and I/O.</span></span> <span data-ttu-id="dabef-114">Sysdig gör det enkelt toosee som fungerar som behållare hello hardest eller i stort sett använder hello de flesta minne och processor.</span><span class="sxs-lookup"><span data-stu-id="dabef-114">Sysdig makes it easy toosee which containers are working hello hardest or essentially using hello most memory and CPU.</span></span> <span data-ttu-id="dabef-115">Den här vyn är i hello ”Overview” avsnittet, vilket är för närvarande i betaversion.</span><span class="sxs-lookup"><span data-stu-id="dabef-115">This view is in hello “Overview” section, which is currently in beta.</span></span> 

![Sysdig-gränssnitt](./media/container-service-monitoring-sysdig/sysdig6.png) 

## <a name="configure-a-sysdig-deployment-with-marathon"></a><span data-ttu-id="dabef-117">Konfigurera en Sysdig-distribution med Marathon</span><span class="sxs-lookup"><span data-stu-id="dabef-117">Configure a Sysdig deployment with Marathon</span></span>
<span data-ttu-id="dabef-118">Dessa steg visar hur tooconfigure och distribuera Sysdig program tooyour kluster med Marathon.</span><span class="sxs-lookup"><span data-stu-id="dabef-118">These steps will show you how tooconfigure and deploy Sysdig applications tooyour cluster with Marathon.</span></span> 

<span data-ttu-id="dabef-119">Åtkomst till ditt DC/OS-Gränssnittet via [http://localhost:80 /](http://localhost:80/) en gång i hello DC/OS-Gränssnittet går toohello ”universum” som finns på hello nedre vänster och sedan söka efter ”Sysdig”.</span><span class="sxs-lookup"><span data-stu-id="dabef-119">Access your DC/OS UI via [http://localhost:80/](http://localhost:80/) Once in hello DC/OS UI navigate toohello "Universe", which is on hello bottom left and then search for "Sysdig."</span></span>

![Sysdig i DC/OS Universe](./media/container-service-monitoring-sysdig/sysdig1.png)

<span data-ttu-id="dabef-121">Nu toocomplete hello konfiguration du behöver en Sysdig moln konto eller ett kostnadsfritt utvärderingskonto.</span><span class="sxs-lookup"><span data-stu-id="dabef-121">Now toocomplete hello configuration you need a Sysdig cloud account or a free trial account.</span></span> <span data-ttu-id="dabef-122">När du är inloggad på toohello Sysdig moln webbplats klickar du på ditt användarnamn och på sidan hello bör du se din ”åtkomstnyckeln”.</span><span class="sxs-lookup"><span data-stu-id="dabef-122">Once you're logged in toohello Sysdig cloud website, click on your user name, and on hello page you should see your "Access Key."</span></span> 

![API-nyckel för Sysdig](./media/container-service-monitoring-sysdig/sysdig2.png) 

<span data-ttu-id="dabef-124">Bredvid ange din åtkomstnyckel i hello Sysdig konfiguration i hello DC/OS Universe.</span><span class="sxs-lookup"><span data-stu-id="dabef-124">Next enter your Access Key into hello Sysdig configuration within hello DC/OS Universe.</span></span> 

![Sysdig konfigurationen i hello DC/OS Universe](./media/container-service-monitoring-sysdig/sysdig3.png)

<span data-ttu-id="dabef-126">Ange hello instanser too10000000 så när en ny nod läggs toohello klustret Sysdig automatiskt distribuerar en agent toothat nya noden.</span><span class="sxs-lookup"><span data-stu-id="dabef-126">Now set hello instances too10000000 so whenever a new node is added toohello cluster Sysdig will automatically deploy an agent toothat new node.</span></span> <span data-ttu-id="dabef-127">Det här är en tillfällig lösning toomake att Sysdig distribuerar tooall nya agenter inom hello kluster.</span><span class="sxs-lookup"><span data-stu-id="dabef-127">This is an interim solution toomake sure Sysdig will deploy tooall new agents within hello cluster.</span></span> 

![Sysdig konfigurationen i hello DC/OS Universe-instanser](./media/container-service-monitoring-sysdig/sysdig4.png)

<span data-ttu-id="dabef-129">När du har installerat paketet hello navigera tillbaka toohello Sysdig UI och du kan tooexplore hello olika användningsstatistik för hello behållare inom klustret.</span><span class="sxs-lookup"><span data-stu-id="dabef-129">Once you've installed hello package navigate back toohello Sysdig UI and you'll be able tooexplore hello different usage metrics for hello containers within your cluster.</span></span> 

<span data-ttu-id="dabef-130">Du kan även installera Mesos- och Marathon-specifika instrumentpaneler via [den nya instrumentpanelsguiden](https://app.sysdigcloud.com/#/dashboards/new).</span><span class="sxs-lookup"><span data-stu-id="dabef-130">You can also install Mesos and Marathon specific dashboards via the [new dashboard wizard](https://app.sysdigcloud.com/#/dashboards/new).</span></span>
