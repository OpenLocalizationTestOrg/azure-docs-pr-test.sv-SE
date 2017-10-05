---
title: "Övervaka Azure DC/OS-kluster - Dynatrace | Microsoft Docs"
description: "Övervaka ett Azure Container Service DC/OS-kluster med Dynatrace. Distribuera Dynatrace OneAgent med hjälp av DC/OS-instrumentpanelen."
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
ms.openlocfilehash: 6fa23728680779e33eda7bb9aa8a01b9cad9a82b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-an-azure-container-service-dcos-cluster-with-dynatrace-saasmanaged"></a><span data-ttu-id="27d03-105">Övervaka ett Azure Container Service DC/OS-kluster med Dynatrace SaaS/hanteras</span><span class="sxs-lookup"><span data-stu-id="27d03-105">Monitor an Azure Container Service DC/OS cluster with Dynatrace SaaS/Managed</span></span>
<span data-ttu-id="27d03-106">I den här artikeln vi hur du distribuerar den [Dynatrace](https://www.dynatrace.com/) OneAgent att övervaka alla agent-noder i Azure Container Service-kluster.</span><span class="sxs-lookup"><span data-stu-id="27d03-106">In this article, we show you how to deploy the [Dynatrace](https://www.dynatrace.com/) OneAgent to monitor all the agent nodes in your Azure Container Service cluster.</span></span> <span data-ttu-id="27d03-107">Du behöver ett konto med Dynatrace SaaS/hanteras för den här konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="27d03-107">You need an account with Dynatrace SaaS/Managed for this configuration.</span></span> 

## <a name="dynatrace-saasmanaged"></a><span data-ttu-id="27d03-108">Dynatrace SaaS/hanteras</span><span class="sxs-lookup"><span data-stu-id="27d03-108">Dynatrace SaaS/Managed</span></span>
<span data-ttu-id="27d03-109">Dynatrace är en moln-intern övervakningslösning för klustret miljöer och mycket dynamisk behållare.</span><span class="sxs-lookup"><span data-stu-id="27d03-109">Dynatrace is a cloud-native monitoring solution for highly dynamic container and cluster environments.</span></span> <span data-ttu-id="27d03-110">På så sätt kan du optimera din distribution i behållare och en minnesallokering bättre genom att använda data i realtid.</span><span class="sxs-lookup"><span data-stu-id="27d03-110">It allows you to better optimize your container deployments and memory allocations by using real-time usage data.</span></span> <span data-ttu-id="27d03-111">Den är kapabel att automatiskt lokalisera programmet och infrastruktur problem genom att tillhandahålla automatisk baslinjeuppgifter och problemet korrelation grundorsaken identifiering.</span><span class="sxs-lookup"><span data-stu-id="27d03-111">It is capable of automatically pinpointing application and infrastructure issues by providing automated baselining, problem correlation, and root-cause detection.</span></span>

<span data-ttu-id="27d03-112">Följande bild visar Dynatrace UI:</span><span class="sxs-lookup"><span data-stu-id="27d03-112">The following figure shows the Dynatrace UI:</span></span>

![Dynatrace UI](./media/container-service-monitoring-dynatrace/dynatrace.png)

## <a name="prerequisites"></a><span data-ttu-id="27d03-114">Krav</span><span class="sxs-lookup"><span data-stu-id="27d03-114">Prerequisites</span></span> 
<span data-ttu-id="27d03-115">[Distribuera](container-service-deployment.md) och [ansluta](./../container-service-connect.md) till ett kluster som har konfigurerats med Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="27d03-115">[Deploy](container-service-deployment.md) and [connect](./../container-service-connect.md) to a cluster configured by Azure Container Service.</span></span> <span data-ttu-id="27d03-116">Utforska [Marathon-gränssnittet](container-service-mesos-marathon-ui.md).</span><span class="sxs-lookup"><span data-stu-id="27d03-116">Explore the [Marathon UI](container-service-mesos-marathon-ui.md).</span></span> <span data-ttu-id="27d03-117">Gå till [https://www.dynatrace.com/trial/](https://www.dynatrace.com/trial/) att ställa in en Dynatrace SaaS-konto.</span><span class="sxs-lookup"><span data-stu-id="27d03-117">Go to [https://www.dynatrace.com/trial/](https://www.dynatrace.com/trial/) to set up a Dynatrace SaaS account.</span></span>  

## <a name="configure-a-dynatrace-deployment-with-marathon"></a><span data-ttu-id="27d03-118">Konfigurera Dynatrace distribution med Marathon</span><span class="sxs-lookup"><span data-stu-id="27d03-118">Configure a Dynatrace deployment with Marathon</span></span>
<span data-ttu-id="27d03-119">Dessa steg visar hur du konfigurerar och distribuerar program för Dynatrace till ditt kluster med Marathon.</span><span class="sxs-lookup"><span data-stu-id="27d03-119">These steps show you how to configure and deploy Dynatrace applications to your cluster with Marathon.</span></span>

1. <span data-ttu-id="27d03-120">Åtkomst till ditt DC/OS-Gränssnittet via [http://localhost:80 /](http://localhost:80/).</span><span class="sxs-lookup"><span data-stu-id="27d03-120">Access your DC/OS UI via [http://localhost:80/](http://localhost:80/).</span></span> <span data-ttu-id="27d03-121">En gång i DC/OS-Gränssnittet går du till den **Universe** fliken och sök sedan efter **Dynatrace**.</span><span class="sxs-lookup"><span data-stu-id="27d03-121">Once in the DC/OS UI, navigate to the **Universe** tab and then search for **Dynatrace**.</span></span>

    ![Dynatrace i DC/OS Universe](./media/container-service-monitoring-dynatrace/dynatrace-universe.png)

2. <span data-ttu-id="27d03-123">Om du vill slutföra konfigurationen behöver du ett Dynatrace SaaS-konto eller ett kostnadsfritt utvärderingskonto.</span><span class="sxs-lookup"><span data-stu-id="27d03-123">To complete the configuration, you need a Dynatrace SaaS account or a free trial account.</span></span> <span data-ttu-id="27d03-124">När du loggar in i instrumentpanelen Dynatrace välja **distribuera Dynatrace**.</span><span class="sxs-lookup"><span data-stu-id="27d03-124">Once you log into the Dynatrace dashboard, select **Deploy Dynatrace**.</span></span>

    ![Dynatrace konfigurera PaaS-integrering](./media/container-service-monitoring-dynatrace/setup-paas.png)

3. <span data-ttu-id="27d03-126">På sidan Välj **ställa in integration PaaS**.</span><span class="sxs-lookup"><span data-stu-id="27d03-126">On the page, select **Set up PaaS integration**.</span></span> 

    ![Dynatrace API-token](./media/container-service-monitoring-dynatrace/api-token.png) 

4. <span data-ttu-id="27d03-128">Ange din API-token i Dynatrace OneAgent konfigurationen i DC/OS-Universe.</span><span class="sxs-lookup"><span data-stu-id="27d03-128">Enter your API token into the Dynatrace OneAgent configuration within the DC/OS Universe.</span></span> 

    ![Dynatrace OneAgent konfigurationen i DC/OS-Universe](./media/container-service-monitoring-dynatrace/dynatrace-config.png)

5. <span data-ttu-id="27d03-130">Ange antalet noder som du vill köra instanserna.</span><span class="sxs-lookup"><span data-stu-id="27d03-130">Set the instances to the number of nodes you intend to run.</span></span> <span data-ttu-id="27d03-131">Anger en hög siffra också fungerar, men DC/OS kommer att försöka hitta nya instanser tills numret faktiskt har uppnåtts.</span><span class="sxs-lookup"><span data-stu-id="27d03-131">Setting a higher number also works, but DC/OS will keep trying to find new instances until that number is actually reached.</span></span> <span data-ttu-id="27d03-132">Om du vill kan du också ställa in det till ett värde som 1000000.</span><span class="sxs-lookup"><span data-stu-id="27d03-132">If you prefer, you can also set this to a value like 1000000.</span></span> <span data-ttu-id="27d03-133">I det här fallet när en ny nod läggs till i klustret, distribuerar Dynatrace automatiskt en agent till den nya noden i priset för DC/OS ständigt försöker distribuera ytterligare instanser.</span><span class="sxs-lookup"><span data-stu-id="27d03-133">In this case, whenever a new node is added to the cluster, Dynatrace automatically deploys an agent to that new node, at the price of DC/OS constantly trying to deploy further instances.</span></span>

    ![Dynatrace konfigurationen i DC/OS Universe-instanser](./media/container-service-monitoring-dynatrace/dynatrace-config2.png)

## <a name="next-steps"></a><span data-ttu-id="27d03-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="27d03-135">Next steps</span></span>

<span data-ttu-id="27d03-136">När du har installerat paketet kan du gå tillbaka till instrumentpanelen Dynatrace.</span><span class="sxs-lookup"><span data-stu-id="27d03-136">Once you've installed the package, navigate back to the Dynatrace dashboard.</span></span> <span data-ttu-id="27d03-137">Du kan utforska olika användningsstatistik för behållarna i klustret.</span><span class="sxs-lookup"><span data-stu-id="27d03-137">You can explore the different usage metrics for the containers within your cluster.</span></span> 