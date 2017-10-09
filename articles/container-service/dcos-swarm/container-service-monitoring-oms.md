---
title: aaaMonitor Azure DC/OS-kluster - Verksamhetsstyrning | Microsoft Docs
description: "Övervaka ett Azure Container Service DC/OS-kluster med Microsoft Operations Management Suite."
services: container-service
documentationcenter: 
author: keikhara
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure
ms.date: 11/17/2016
ms.author: keikhara
ms.custom: mvc
ms.openlocfilehash: 0ebfa3ba3cef8f0205b15731b0e91f5b304bc8fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-an-azure-container-service-dcos-cluster-with-operations-management-suite"></a><span data-ttu-id="17a27-103">Övervaka ett Azure Container Service DC/OS-kluster med Operations Management Suite</span><span class="sxs-lookup"><span data-stu-id="17a27-103">Monitor an Azure Container Service DC/OS cluster with Operations Management Suite</span></span>

<span data-ttu-id="17a27-104">Microsoft Operations Management Suite (OMS) är Microsofts molnbaserade IT-hanteringslösning som hjälper dig att hantera och skydda din lokala och molnbaserade infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="17a27-104">Microsoft Operations Management Suite (OMS) is Microsoft's cloud-based IT management solution that helps you manage and protect your on-premises and cloud infrastructure.</span></span> <span data-ttu-id="17a27-105">Behållaren lösningen är en lösning i OMS logganalys som du kan visa hello behållaren lager, prestanda och loggar på en enda plats.</span><span class="sxs-lookup"><span data-stu-id="17a27-105">Container Solution is a solution in OMS Log Analytics, which helps you view hello container inventory, performance, and logs in a single location.</span></span> <span data-ttu-id="17a27-106">Du kan granska, Felsök behållare genom att visa hello loggar på central plats och hitta störningar förbrukar överdriven behållare på en värd.</span><span class="sxs-lookup"><span data-stu-id="17a27-106">You can audit, troubleshoot containers by viewing hello logs in centralized location, and find noisy consuming excess container on a host.</span></span>

![](media/container-service-monitoring-oms/image1.png)

<span data-ttu-id="17a27-107">Mer information om behållaren lösning hittar toothe [behållare lösning logganalys](../../log-analytics/log-analytics-containers.md).</span><span class="sxs-lookup"><span data-stu-id="17a27-107">For more information about Container Solution, please refer toothe [Container Solution Log Analytics](../../log-analytics/log-analytics-containers.md).</span></span>

## <a name="setting-up-oms-from-hello-dcos-universe"></a><span data-ttu-id="17a27-108">Konfigurera OMS från hello DC/OS universe</span><span class="sxs-lookup"><span data-stu-id="17a27-108">Setting up OMS from hello DC/OS universe</span></span>


<span data-ttu-id="17a27-109">Den här artikeln förutsätter att du har skapat ett DC/OS och har distribuerat enkla behållaren webbprogram på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="17a27-109">This article assumes that you have set up an DC/OS and have deployed simple web container applications on hello cluster.</span></span>

### <a name="pre-requisite"></a><span data-ttu-id="17a27-110">Förhandskrav</span><span class="sxs-lookup"><span data-stu-id="17a27-110">Pre-requisite</span></span>
- <span data-ttu-id="17a27-111">[Microsoft Azure-prenumeration](https://azure.microsoft.com/free/) -du kan få detta kostnadsfritt.</span><span class="sxs-lookup"><span data-stu-id="17a27-111">[Microsoft Azure Subscription](https://azure.microsoft.com/free/) - You can get this for free.</span></span>  
- <span data-ttu-id="17a27-112">Installationsprogrammet för Microsoft OMS-arbetsytan - finns i ”steg 3” nedan</span><span class="sxs-lookup"><span data-stu-id="17a27-112">Microsoft OMS Workspace Setup - see "Step 3" below</span></span>
- <span data-ttu-id="17a27-113">[DC/OS CLI](https://dcos.io/docs/1.8/usage/cli/install/) installerad.</span><span class="sxs-lookup"><span data-stu-id="17a27-113">[DC/OS CLI](https://dcos.io/docs/1.8/usage/cli/install/) installed.</span></span>

1. <span data-ttu-id="17a27-114">Hello DC/OS-instrumentpanelen, klicka på Universe och Sök efter ”OMS-enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="17a27-114">In hello DC/OS dashboard, click on Universe and search for ‘OMS’ as shown below.</span></span>

![](media/container-service-monitoring-oms/image2.png)

2. <span data-ttu-id="17a27-115">Klicka på **Installera**.</span><span class="sxs-lookup"><span data-stu-id="17a27-115">Click **Install**.</span></span> <span data-ttu-id="17a27-116">Du ser en pop upp med hello OMS versionsinformation och en **installera paket** eller **Installation i Avancerat** knappen.</span><span class="sxs-lookup"><span data-stu-id="17a27-116">You will see a pop up with hello OMS version information and an **Install Package** or **Advanced Installation** button.</span></span> <span data-ttu-id="17a27-117">När du klickar på **Installation i Avancerat**, som leder dig toohello **OMS specifika konfigurationsegenskaper** sidan.</span><span class="sxs-lookup"><span data-stu-id="17a27-117">When you click **Advanced Installation**, which leads you toohello **OMS specific configuration properties** page.</span></span>

![](media/container-service-monitoring-oms/image3.png)

![](media/container-service-monitoring-oms/image4.png)

3. <span data-ttu-id="17a27-118">Här kan du bli ombedd tooenter hello `wsid` (hello OMS arbetsyte-ID) och `wskey` (hello OMS primär nyckel för hello arbetsyte-id).</span><span class="sxs-lookup"><span data-stu-id="17a27-118">Here, you will be asked tooenter hello `wsid` (hello OMS workspace ID) and `wskey` (hello OMS primary key for hello workspace id).</span></span> <span data-ttu-id="17a27-119">tooget båda `wsid` och `wskey` toocreate måste en OMS-konto på <https://mms.microsoft.com>. Följ hello steg toocreate ett konto.</span><span class="sxs-lookup"><span data-stu-id="17a27-119">tooget both `wsid` and `wskey` you need toocreate an OMS account at <https://mms.microsoft.com>. Please follow hello steps toocreate an account.</span></span> <span data-ttu-id="17a27-120">När du är klar skapar hello-konto måste tooobtain din `wsid` och `wskey` genom att klicka på **inställningar**, sedan **anslutna källor**, och sedan **Linux-servrar** , enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="17a27-120">Once you are done creating hello account, you need tooobtain your `wsid` and `wskey` by clicking **Settings**, then **Connected Sources**, and then **Linux Servers**, as shown below.</span></span>

 ![](media/container-service-monitoring-oms/image5.png)

4. <span data-ttu-id="17a27-121">Välj hello tal du OMS-instanser som du vill använda och klicka hello, granska och installera'.</span><span class="sxs-lookup"><span data-stu-id="17a27-121">Select hello number you OMS instances that you want and click hello ‘Review and Install’ button.</span></span> <span data-ttu-id="17a27-122">Vanligtvis vill du toohave hello antalet OMS instanser lika toohello Virtuella datorer i klustret för agenten.</span><span class="sxs-lookup"><span data-stu-id="17a27-122">Typically, you will want toohave hello number of OMS instances equal toohello number of VM’s you have in your agent cluster.</span></span> <span data-ttu-id="17a27-123">OMS-Agent för Linux är en installation som enskilda behållare på varje virtuell dator att vill toocollect information för övervakning och loggning.</span><span class="sxs-lookup"><span data-stu-id="17a27-123">OMS Agent for Linux is installs as individual containers on each VM that it wants toocollect information for monitoring and logging information.</span></span>

## <a name="setting-up-a-simple-oms-dashboard"></a><span data-ttu-id="17a27-124">Konfigurera en enkel OMS-instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="17a27-124">Setting up a simple OMS dashboard</span></span>

<span data-ttu-id="17a27-125">När du har installerat hello OMS-Agent för Linux på hello virtuella datorer, är nästa steg tooset in hello OMS-instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="17a27-125">Once you have installed hello OMS Agent for Linux on hello VMs, next step is tooset up hello OMS dashboard.</span></span> <span data-ttu-id="17a27-126">Det finns två sätt toodo detta: OMS-portalen och Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="17a27-126">There are two ways toodo this: OMS Portal or Azure Portal.</span></span>

### <a name="oms-portal"></a><span data-ttu-id="17a27-127">OMS-portalen</span><span class="sxs-lookup"><span data-stu-id="17a27-127">OMS Portal</span></span> 

<span data-ttu-id="17a27-128">Logga in toohello OMS-portalen (<https://mms.microsoft.com>) och gå toohello **lösning galleriet**.</span><span class="sxs-lookup"><span data-stu-id="17a27-128">Log in toohello OMS portal (<https://mms.microsoft.com>) and go toohello **Solution Gallery**.</span></span>

![](media/container-service-monitoring-oms/image6.png)

<span data-ttu-id="17a27-129">När du är i hello **lösning galleriet**väljer **behållare**.</span><span class="sxs-lookup"><span data-stu-id="17a27-129">Once you are in hello **Solution Gallery**, select **Containers**.</span></span>

![](media/container-service-monitoring-oms/image7.png)

<span data-ttu-id="17a27-130">När du har valt hello behållaren lösning visas hello panelen på hello OMS översikt instrumentpanelssida.</span><span class="sxs-lookup"><span data-stu-id="17a27-130">Once you’ve selected hello Container Solution, you will see hello tile on hello OMS Overview Dashboard page.</span></span> <span data-ttu-id="17a27-131">När hello inhämtas behållardata indexeras visas hello sida vid sida med information om lösningen visa paneler.</span><span class="sxs-lookup"><span data-stu-id="17a27-131">Once hello ingested container data is indexed, you will see hello tile populated with information on the solution view tiles.</span></span>

![](media/container-service-monitoring-oms/image8.png)

### <a name="azure-portal"></a><span data-ttu-id="17a27-132">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="17a27-132">Azure Portal</span></span> 

<span data-ttu-id="17a27-133">Inloggningen tooAzure portalen på <https://portal.microsoft.com/>.</span><span class="sxs-lookup"><span data-stu-id="17a27-133">Login tooAzure portal at <https://portal.microsoft.com/>.</span></span> <span data-ttu-id="17a27-134">Gå till **Marketplace**väljer **övervakning + management** och på **finns alla**.</span><span class="sxs-lookup"><span data-stu-id="17a27-134">Go to **Marketplace**, select **Monitoring + management** and click **See All**.</span></span> <span data-ttu-id="17a27-135">Skriv `containers` i sökningen.</span><span class="sxs-lookup"><span data-stu-id="17a27-135">Then Type `containers` in search.</span></span> <span data-ttu-id="17a27-136">Du ser ”behållare” i hello sökresultat.</span><span class="sxs-lookup"><span data-stu-id="17a27-136">You will see "containers" in hello search results.</span></span> <span data-ttu-id="17a27-137">Välj **behållare** och på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="17a27-137">Select **Containers** and click **Create**.</span></span>

![](media/container-service-monitoring-oms/image9.png)

<span data-ttu-id="17a27-138">När du klickar på **skapa**, uppmanas du för din arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="17a27-138">Once you click **Create**, it will ask you for your workspace.</span></span> <span data-ttu-id="17a27-139">Välj din arbetsyta eller om du inte har någon kan du skapa en ny arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="17a27-139">Select your workspace or if you do not have one, create a new workspace.</span></span>

![](media/container-service-monitoring-oms/image10.PNG)

<span data-ttu-id="17a27-140">När du har valt din arbetsyta, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="17a27-140">Once you’ve selected your workspace, click **Create**.</span></span>

![](media/container-service-monitoring-oms/image11.png)

<span data-ttu-id="17a27-141">Mer information om hello OMS behållaren lösning Se toothe [behållare lösning logganalys](../../log-analytics/log-analytics-containers.md).</span><span class="sxs-lookup"><span data-stu-id="17a27-141">For more information about hello OMS Container Solution, please refer toothe [Container Solution Log Analytics](../../log-analytics/log-analytics-containers.md).</span></span>

### <a name="how-tooscale-oms-agent-with-acs-dcos"></a><span data-ttu-id="17a27-142">Hur tooscale OMS-Agent med ACS DC/OS</span><span class="sxs-lookup"><span data-stu-id="17a27-142">How tooscale OMS Agent with ACS DC/OS</span></span> 

<span data-ttu-id="17a27-143">Om du behöver toohave installerat OMS-agent lite hello faktiska nodsantalet eller du proportionellt VMSS genom att lägga till fler Virtuella du göra det genom att skala hello `msoms` service.</span><span class="sxs-lookup"><span data-stu-id="17a27-143">In case you need toohave installed OMS agent short of hello actual node count or you are scaling up VMSS by adding more VM, you can do so by scaling hello `msoms` service.</span></span>

<span data-ttu-id="17a27-144">Du kan gå tooMarathon eller hello DC/OS UI tjänstefliken och skala upp din antalet noder.</span><span class="sxs-lookup"><span data-stu-id="17a27-144">You can either go tooMarathon or hello DC/OS UI Services tab and scale up your node count.</span></span>

![](media/container-service-monitoring-oms/image12.PNG)

<span data-ttu-id="17a27-145">Det här distribuerar tooother noder som ännu inte har distribuerat hello OMS-agent.</span><span class="sxs-lookup"><span data-stu-id="17a27-145">This will deploy tooother nodes which have not yet deployed hello OMS agent.</span></span>

## <a name="uninstall-ms-oms"></a><span data-ttu-id="17a27-146">Avinstallera MS OMS</span><span class="sxs-lookup"><span data-stu-id="17a27-146">Uninstall MS OMS</span></span>

<span data-ttu-id="17a27-147">toouninstall MS OMS ange hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="17a27-147">toouninstall MS OMS enter hello following command:</span></span>

```bash
$ dcos package uninstall msoms
```

## <a name="let-us-know"></a><span data-ttu-id="17a27-148">Berätta för oss!</span><span class="sxs-lookup"><span data-stu-id="17a27-148">Let us know!!!</span></span>
<span data-ttu-id="17a27-149">Vad fungerar?</span><span class="sxs-lookup"><span data-stu-id="17a27-149">What works?</span></span> <span data-ttu-id="17a27-150">Vad som saknas?</span><span class="sxs-lookup"><span data-stu-id="17a27-150">What is missing?</span></span> <span data-ttu-id="17a27-151">Vad behöver du för den här toobe användbara för dig?</span><span class="sxs-lookup"><span data-stu-id="17a27-151">What else do you need for this toobe useful for you?</span></span> <span data-ttu-id="17a27-152">Berätta för oss på <a href="mailto:OMSContainers@microsoft.com">OMSContainers</a>.</span><span class="sxs-lookup"><span data-stu-id="17a27-152">Let us know at <a href="mailto:OMSContainers@microsoft.com">OMSContainers</a>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="17a27-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="17a27-153">Next steps</span></span>

 <span data-ttu-id="17a27-154">Nu när du har ställt in OMS toomonitor din behållare[finns i instrumentpanelen behållaren](../../log-analytics/log-analytics-containers.md).</span><span class="sxs-lookup"><span data-stu-id="17a27-154">Now that you have set up OMS toomonitor your containers,[see your container dashboard](../../log-analytics/log-analytics-containers.md).</span></span>
