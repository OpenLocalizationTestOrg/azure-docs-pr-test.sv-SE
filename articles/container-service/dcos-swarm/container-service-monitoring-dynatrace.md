---
title: aaaMonitor Azure DC/OS-kluster - Dynatrace | Microsoft Docs
description: "Övervaka ett Azure Container Service DC/OS-kluster med Dynatrace. Distribuera hello Dynatrace OneAgent med hello DC/OS-instrumentpanelen."
services: container-service
documentationcenter: 
author: MartinGoodwell
manager: 
editor: 
tags: acs, azure-container-service
keywords: "Behållare, DC/OS, Azure"
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/13/2016
ms.author: rogardle
ms.custom: mvc
ms.openlocfilehash: 9e2e2d1c8b454422d1db30dac7c385d31d336853
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-an-azure-container-service-dcos-cluster-with-dynatrace-saasmanaged"></a><span data-ttu-id="820a1-105">Övervaka ett Azure Container Service DC/OS-kluster med Dynatrace SaaS/hanteras</span><span class="sxs-lookup"><span data-stu-id="820a1-105">Monitor an Azure Container Service DC/OS cluster with Dynatrace SaaS/Managed</span></span>
<span data-ttu-id="820a1-106">I den här artikeln visar vi dig hur toodeploy hello [Dynatrace](https://www.dynatrace.com/) OneAgent toomonitor alla hello agent noder i Azure Container Service-kluster.</span><span class="sxs-lookup"><span data-stu-id="820a1-106">In this article, we show you how toodeploy hello [Dynatrace](https://www.dynatrace.com/) OneAgent toomonitor all hello agent nodes in your Azure Container Service cluster.</span></span> <span data-ttu-id="820a1-107">Du behöver ett konto med Dynatrace SaaS/hanteras för den här konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="820a1-107">You need an account with Dynatrace SaaS/Managed for this configuration.</span></span> 

## <a name="dynatrace-saasmanaged"></a><span data-ttu-id="820a1-108">Dynatrace SaaS/hanteras</span><span class="sxs-lookup"><span data-stu-id="820a1-108">Dynatrace SaaS/Managed</span></span>
<span data-ttu-id="820a1-109">Dynatrace är en moln-intern övervakningslösning för klustret miljöer och mycket dynamisk behållare.</span><span class="sxs-lookup"><span data-stu-id="820a1-109">Dynatrace is a cloud-native monitoring solution for highly dynamic container and cluster environments.</span></span> <span data-ttu-id="820a1-110">Det gör att du toobetter optimera din distribution i behållare och en minnesallokering med hjälp av data i realtid.</span><span class="sxs-lookup"><span data-stu-id="820a1-110">It allows you toobetter optimize your container deployments and memory allocations by using real-time usage data.</span></span> <span data-ttu-id="820a1-111">Den är kapabel att automatiskt lokalisera programmet och infrastruktur problem genom att tillhandahålla automatisk baslinjeuppgifter och problemet korrelation grundorsaken identifiering.</span><span class="sxs-lookup"><span data-stu-id="820a1-111">It is capable of automatically pinpointing application and infrastructure issues by providing automated baselining, problem correlation, and root-cause detection.</span></span>

<span data-ttu-id="820a1-112">hello följande bild visar hello Dynatrace UI:</span><span class="sxs-lookup"><span data-stu-id="820a1-112">hello following figure shows hello Dynatrace UI:</span></span>

![Dynatrace UI](./media/container-service-monitoring-dynatrace/dynatrace.png)

## <a name="prerequisites"></a><span data-ttu-id="820a1-114">Krav</span><span class="sxs-lookup"><span data-stu-id="820a1-114">Prerequisites</span></span> 
<span data-ttu-id="820a1-115">[Distribuera](container-service-deployment.md) och [ansluta](./../container-service-connect.md) tooa kluster som har konfigurerats med Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="820a1-115">[Deploy](container-service-deployment.md) and [connect](./../container-service-connect.md) tooa cluster configured by Azure Container Service.</span></span> <span data-ttu-id="820a1-116">Utforska hello [Marathon-Gränssnittet](container-service-mesos-marathon-ui.md).</span><span class="sxs-lookup"><span data-stu-id="820a1-116">Explore hello [Marathon UI](container-service-mesos-marathon-ui.md).</span></span> <span data-ttu-id="820a1-117">Gå för[https://www.dynatrace.com/trial/](https://www.dynatrace.com/trial/) tooset en Dynatrace SaaS-konto.</span><span class="sxs-lookup"><span data-stu-id="820a1-117">Go too[https://www.dynatrace.com/trial/](https://www.dynatrace.com/trial/) tooset up a Dynatrace SaaS account.</span></span>  

## <a name="configure-a-dynatrace-deployment-with-marathon"></a><span data-ttu-id="820a1-118">Konfigurera Dynatrace distribution med Marathon</span><span class="sxs-lookup"><span data-stu-id="820a1-118">Configure a Dynatrace deployment with Marathon</span></span>
<span data-ttu-id="820a1-119">Dessa steg visar hur tooconfigure och distribuera Dynatrace program tooyour kluster med Marathon.</span><span class="sxs-lookup"><span data-stu-id="820a1-119">These steps show you how tooconfigure and deploy Dynatrace applications tooyour cluster with Marathon.</span></span>

1. <span data-ttu-id="820a1-120">Åtkomst till ditt DC/OS-Gränssnittet via [http://localhost:80 /](http://localhost:80/).</span><span class="sxs-lookup"><span data-stu-id="820a1-120">Access your DC/OS UI via [http://localhost:80/](http://localhost:80/).</span></span> <span data-ttu-id="820a1-121">En gång i hello DC/OS-Gränssnittet, navigerar toohello **Universe** fliken och sök sedan efter **Dynatrace**.</span><span class="sxs-lookup"><span data-stu-id="820a1-121">Once in hello DC/OS UI, navigate toohello **Universe** tab and then search for **Dynatrace**.</span></span>

    ![Dynatrace i DC/OS Universe](./media/container-service-monitoring-dynatrace/dynatrace-universe.png)

2. <span data-ttu-id="820a1-123">toocomplete hello konfiguration, behöver du ett Dynatrace SaaS-konto eller ett kostnadsfritt utvärderingskonto.</span><span class="sxs-lookup"><span data-stu-id="820a1-123">toocomplete hello configuration, you need a Dynatrace SaaS account or a free trial account.</span></span> <span data-ttu-id="820a1-124">När du loggar in hello Dynatrace instrumentpanelen välja **distribuera Dynatrace**.</span><span class="sxs-lookup"><span data-stu-id="820a1-124">Once you log into hello Dynatrace dashboard, select **Deploy Dynatrace**.</span></span>

    ![Dynatrace konfigurera PaaS-integrering](./media/container-service-monitoring-dynatrace/setup-paas.png)

3. <span data-ttu-id="820a1-126">Välj på sidan hello **ställa in integration PaaS**.</span><span class="sxs-lookup"><span data-stu-id="820a1-126">On hello page, select **Set up PaaS integration**.</span></span> 

    ![Dynatrace API-token](./media/container-service-monitoring-dynatrace/api-token.png) 

4. <span data-ttu-id="820a1-128">Ange din API-token i hello Dynatrace OneAgent konfiguration i hello DC/OS Universe.</span><span class="sxs-lookup"><span data-stu-id="820a1-128">Enter your API token into hello Dynatrace OneAgent configuration within hello DC/OS Universe.</span></span> 

    ![Dynatrace OneAgent konfigurationen i hello DC/OS Universe](./media/container-service-monitoring-dynatrace/dynatrace-config.png)

5. <span data-ttu-id="820a1-130">Ange hello instanser toohello många noder du planerar toorun.</span><span class="sxs-lookup"><span data-stu-id="820a1-130">Set hello instances toohello number of nodes you intend toorun.</span></span> <span data-ttu-id="820a1-131">Anger en hög siffra också fungerar, men DC/OS kommer fortsätta att försöka toofind nya instanser tills numret faktiskt har uppnåtts.</span><span class="sxs-lookup"><span data-stu-id="820a1-131">Setting a higher number also works, but DC/OS will keep trying toofind new instances until that number is actually reached.</span></span> <span data-ttu-id="820a1-132">Om du vill kan ange du också tooa värdet som 1000000.</span><span class="sxs-lookup"><span data-stu-id="820a1-132">If you prefer, you can also set this tooa value like 1000000.</span></span> <span data-ttu-id="820a1-133">I det här fallet när en ny nod läggs toohello kluster, distribuerar Dynatrace automatiskt en agent toothat nya noden till hello pris DC/OS ständigt försök toodeploy ytterligare instanser.</span><span class="sxs-lookup"><span data-stu-id="820a1-133">In this case, whenever a new node is added toohello cluster, Dynatrace automatically deploys an agent toothat new node, at hello price of DC/OS constantly trying toodeploy further instances.</span></span>

    ![Dynatrace konfigurationen i hello DC/OS Universe-instanser](./media/container-service-monitoring-dynatrace/dynatrace-config2.png)

## <a name="next-steps"></a><span data-ttu-id="820a1-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="820a1-135">Next steps</span></span>

<span data-ttu-id="820a1-136">När du har installerat hello paketet kan gå tillbaka toohello Dynatrace instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="820a1-136">Once you've installed hello package, navigate back toohello Dynatrace dashboard.</span></span> <span data-ttu-id="820a1-137">Du kan utforska hello olika användningsstatistik för hello behållare i klustret.</span><span class="sxs-lookup"><span data-stu-id="820a1-137">You can explore hello different usage metrics for hello containers within your cluster.</span></span> 
