---
title: "Övervaka Azure DC/OS-klustret - Verksamhetsstyrning | Microsoft Docs"
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
ms.openlocfilehash: 9b8f96b34b53982c469273a3df9751ceb7930d60
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-an-azure-container-service-dcos-cluster-with-operations-management-suite"></a><span data-ttu-id="6147b-103">Övervaka ett Azure Container Service DC/OS-kluster med Operations Management Suite</span><span class="sxs-lookup"><span data-stu-id="6147b-103">Monitor an Azure Container Service DC/OS cluster with Operations Management Suite</span></span>

<span data-ttu-id="6147b-104">Microsoft Operations Management Suite (OMS) är Microsofts molnbaserade IT-hanteringslösning som hjälper dig att hantera och skydda din lokala och molnbaserade infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="6147b-104">Microsoft Operations Management Suite (OMS) is Microsoft's cloud-based IT management solution that helps you manage and protect your on-premises and cloud infrastructure.</span></span> <span data-ttu-id="6147b-105">Behållaren lösningen är en lösning i OMS logganalys, vilket gör det möjligt att visa behållaren inventering, prestanda och loggar på en enda plats.</span><span class="sxs-lookup"><span data-stu-id="6147b-105">Container Solution is a solution in OMS Log Analytics, which helps you view the container inventory, performance, and logs in a single location.</span></span> <span data-ttu-id="6147b-106">Du kan granska, Felsök behållare genom att visa loggarna på central plats och hitta störningar förbrukar överdriven behållare på en värd.</span><span class="sxs-lookup"><span data-stu-id="6147b-106">You can audit, troubleshoot containers by viewing the logs in centralized location, and find noisy consuming excess container on a host.</span></span>

![](media/container-service-monitoring-oms/image1.png)

<span data-ttu-id="6147b-107">Mer information om behållaren lösning hittar du den [behållare lösning logganalys](../../log-analytics/log-analytics-containers.md).</span><span class="sxs-lookup"><span data-stu-id="6147b-107">For more information about Container Solution, please refer to the [Container Solution Log Analytics](../../log-analytics/log-analytics-containers.md).</span></span>

## <a name="setting-up-oms-from-the-dcos-universe"></a><span data-ttu-id="6147b-108">Konfigurera OMS från DC/OS-universe</span><span class="sxs-lookup"><span data-stu-id="6147b-108">Setting up OMS from the DC/OS universe</span></span>


<span data-ttu-id="6147b-109">Den här artikeln förutsätter att du har skapat ett DC/OS och har distribuerat enkla behållaren webbprogram på klustret.</span><span class="sxs-lookup"><span data-stu-id="6147b-109">This article assumes that you have set up an DC/OS and have deployed simple web container applications on the cluster.</span></span>

### <a name="pre-requisite"></a><span data-ttu-id="6147b-110">Förhandskrav</span><span class="sxs-lookup"><span data-stu-id="6147b-110">Pre-requisite</span></span>
- <span data-ttu-id="6147b-111">[Microsoft Azure-prenumeration](https://azure.microsoft.com/free/) -du kan få detta kostnadsfritt.</span><span class="sxs-lookup"><span data-stu-id="6147b-111">[Microsoft Azure Subscription](https://azure.microsoft.com/free/) - You can get this for free.</span></span>  
- <span data-ttu-id="6147b-112">Installationsprogrammet för Microsoft OMS-arbetsytan - finns i ”steg 3” nedan</span><span class="sxs-lookup"><span data-stu-id="6147b-112">Microsoft OMS Workspace Setup - see "Step 3" below</span></span>
- <span data-ttu-id="6147b-113">[DC/OS CLI](https://dcos.io/docs/1.8/usage/cli/install/) installerad.</span><span class="sxs-lookup"><span data-stu-id="6147b-113">[DC/OS CLI](https://dcos.io/docs/1.8/usage/cli/install/) installed.</span></span>

1. <span data-ttu-id="6147b-114">Klicka på DC/OS-instrumentpanel på Universe och Sök efter ”OMS-enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="6147b-114">In the DC/OS dashboard, click on Universe and search for ‘OMS’ as shown below.</span></span>

![](media/container-service-monitoring-oms/image2.png)

2. <span data-ttu-id="6147b-115">Klicka på **Installera**.</span><span class="sxs-lookup"><span data-stu-id="6147b-115">Click **Install**.</span></span> <span data-ttu-id="6147b-116">Du ser en pop upp med versionsinformation OMS och en **installera paket** eller **Installation i Avancerat** knappen.</span><span class="sxs-lookup"><span data-stu-id="6147b-116">You will see a pop up with the OMS version information and an **Install Package** or **Advanced Installation** button.</span></span> <span data-ttu-id="6147b-117">När du klickar på **Installation i Avancerat**, vilket innebär att du den **OMS specifika konfigurationsegenskaper** sidan.</span><span class="sxs-lookup"><span data-stu-id="6147b-117">When you click **Advanced Installation**, which leads you to the **OMS specific configuration properties** page.</span></span>

![](media/container-service-monitoring-oms/image3.png)

![](media/container-service-monitoring-oms/image4.png)

3. <span data-ttu-id="6147b-118">Här kan du bli ombedd att ange den `wsid` (OMS arbetsytan ID) och `wskey` (OMS primära nyckeln för ditt arbetsyte-id).</span><span class="sxs-lookup"><span data-stu-id="6147b-118">Here, you will be asked to enter the `wsid` (the OMS workspace ID) and `wskey` (the OMS primary key for the workspace id).</span></span> <span data-ttu-id="6147b-119">Att hämta både `wsid` och `wskey` måste du skapa en OMS-konto på <https://mms.microsoft.com>. Följ stegen för att skapa ett konto.</span><span class="sxs-lookup"><span data-stu-id="6147b-119">To get both `wsid` and `wskey` you need to create an OMS account at <https://mms.microsoft.com>. Please follow the steps to create an account.</span></span> <span data-ttu-id="6147b-120">När du är klar att skapa kontot, måste du skaffa din `wsid` och `wskey` genom att klicka på **inställningar**, sedan **anslutna källor**, och sedan **Linux-servrar**, enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="6147b-120">Once you are done creating the account, you need to obtain your `wsid` and `wskey` by clicking **Settings**, then **Connected Sources**, and then **Linux Servers**, as shown below.</span></span>

 ![](media/container-service-monitoring-oms/image5.png)

4. <span data-ttu-id="6147b-121">Välj de nummer du OMS-instanser som du vill använda och klicka på knappen ”Granska och installera”.</span><span class="sxs-lookup"><span data-stu-id="6147b-121">Select the number you OMS instances that you want and click the ‘Review and Install’ button.</span></span> <span data-ttu-id="6147b-122">Normalt kommer du vilja ha antal OMS-instanser som är lika med antalet Virtuella datorer i klustret för agenten.</span><span class="sxs-lookup"><span data-stu-id="6147b-122">Typically, you will want to have the number of OMS instances equal to the number of VM’s you have in your agent cluster.</span></span> <span data-ttu-id="6147b-123">OMS-Agent för Linux är en installation som enskilda behållare på varje virtuell dator som du vill samla in information för övervakning och loggning.</span><span class="sxs-lookup"><span data-stu-id="6147b-123">OMS Agent for Linux is installs as individual containers on each VM that it wants to collect information for monitoring and logging information.</span></span>

## <a name="setting-up-a-simple-oms-dashboard"></a><span data-ttu-id="6147b-124">Konfigurera en enkel OMS-instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="6147b-124">Setting up a simple OMS dashboard</span></span>

<span data-ttu-id="6147b-125">När du har installerat OMS-Agent för Linux på virtuella datorer, är nästa steg att ställa in OMS-instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="6147b-125">Once you have installed the OMS Agent for Linux on the VMs, next step is to set up the OMS dashboard.</span></span> <span data-ttu-id="6147b-126">Det finns två sätt att göra detta: OMS-portalen och Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6147b-126">There are two ways to do this: OMS Portal or Azure Portal.</span></span>

### <a name="oms-portal"></a><span data-ttu-id="6147b-127">OMS-portalen</span><span class="sxs-lookup"><span data-stu-id="6147b-127">OMS Portal</span></span> 

<span data-ttu-id="6147b-128">Logga in på OMS-portalen (<https://mms.microsoft.com>) och gå till den **lösning galleriet**.</span><span class="sxs-lookup"><span data-stu-id="6147b-128">Log in to the OMS portal (<https://mms.microsoft.com>) and go to the **Solution Gallery**.</span></span>

![](media/container-service-monitoring-oms/image6.png)

<span data-ttu-id="6147b-129">När du är i den **lösning galleriet**väljer **behållare**.</span><span class="sxs-lookup"><span data-stu-id="6147b-129">Once you are in the **Solution Gallery**, select **Containers**.</span></span>

![](media/container-service-monitoring-oms/image7.png)

<span data-ttu-id="6147b-130">När du har valt behållaren lösningen visas panelen på sidan OMS översikt över instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="6147b-130">Once you’ve selected the Container Solution, you will see the tile on the OMS Overview Dashboard page.</span></span> <span data-ttu-id="6147b-131">När de infogade behållardata indexeras visas panelen fylls i med information om lösningen visa paneler.</span><span class="sxs-lookup"><span data-stu-id="6147b-131">Once the ingested container data is indexed, you will see the tile populated with information on the solution view tiles.</span></span>

![](media/container-service-monitoring-oms/image8.png)

### <a name="azure-portal"></a><span data-ttu-id="6147b-132">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="6147b-132">Azure Portal</span></span> 

<span data-ttu-id="6147b-133">Logga in på Azure portal på <https://portal.microsoft.com/>.</span><span class="sxs-lookup"><span data-stu-id="6147b-133">Login to Azure portal at <https://portal.microsoft.com/>.</span></span> <span data-ttu-id="6147b-134">Gå till **Marketplace**väljer **övervakning + management** och på **finns alla**.</span><span class="sxs-lookup"><span data-stu-id="6147b-134">Go to **Marketplace**, select **Monitoring + management** and click **See All**.</span></span> <span data-ttu-id="6147b-135">Skriv `containers` i sökningen.</span><span class="sxs-lookup"><span data-stu-id="6147b-135">Then Type `containers` in search.</span></span> <span data-ttu-id="6147b-136">Du ser ”behållare” i sökresultaten.</span><span class="sxs-lookup"><span data-stu-id="6147b-136">You will see "containers" in the search results.</span></span> <span data-ttu-id="6147b-137">Välj **behållare** och på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="6147b-137">Select **Containers** and click **Create**.</span></span>

![](media/container-service-monitoring-oms/image9.png)

<span data-ttu-id="6147b-138">När du klickar på **skapa**, uppmanas du för din arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="6147b-138">Once you click **Create**, it will ask you for your workspace.</span></span> <span data-ttu-id="6147b-139">Välj din arbetsyta eller om du inte har någon kan du skapa en ny arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="6147b-139">Select your workspace or if you do not have one, create a new workspace.</span></span>

![](media/container-service-monitoring-oms/image10.PNG)

<span data-ttu-id="6147b-140">När du har valt din arbetsyta, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="6147b-140">Once you’ve selected your workspace, click **Create**.</span></span>

![](media/container-service-monitoring-oms/image11.png)

<span data-ttu-id="6147b-141">Mer information om lösningen i OMS-behållaren finns i den [behållare lösning logganalys](../../log-analytics/log-analytics-containers.md).</span><span class="sxs-lookup"><span data-stu-id="6147b-141">For more information about the OMS Container Solution, please refer to the [Container Solution Log Analytics](../../log-analytics/log-analytics-containers.md).</span></span>

### <a name="how-to-scale-oms-agent-with-acs-dcos"></a><span data-ttu-id="6147b-142">Så här skalar du OMS-Agent med ACS DC/OS</span><span class="sxs-lookup"><span data-stu-id="6147b-142">How to scale OMS Agent with ACS DC/OS</span></span> 

<span data-ttu-id="6147b-143">Om du har installerat OMS-agent lite antalet faktiska noder eller du proportionellt VMSS genom att lägga till fler Virtuella, kan du göra det genom skalning av `msoms` service.</span><span class="sxs-lookup"><span data-stu-id="6147b-143">In case you need to have installed OMS agent short of the actual node count or you are scaling up VMSS by adding more VM, you can do so by scaling the `msoms` service.</span></span>

<span data-ttu-id="6147b-144">Du kan gå till Marathon eller fliken DC/OS UI-tjänster och skala upp din antalet noder.</span><span class="sxs-lookup"><span data-stu-id="6147b-144">You can either go to Marathon or the DC/OS UI Services tab and scale up your node count.</span></span>

![](media/container-service-monitoring-oms/image12.PNG)

<span data-ttu-id="6147b-145">Det här distribuerar till andra noder som ännu inte har distribuerat OMS-agenten.</span><span class="sxs-lookup"><span data-stu-id="6147b-145">This will deploy to other nodes which have not yet deployed the OMS agent.</span></span>

## <a name="uninstall-ms-oms"></a><span data-ttu-id="6147b-146">Avinstallera MS OMS</span><span class="sxs-lookup"><span data-stu-id="6147b-146">Uninstall MS OMS</span></span>

<span data-ttu-id="6147b-147">Om du vill avinstallera anger MS OMS du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="6147b-147">To uninstall MS OMS enter the following command:</span></span>

```bash
$ dcos package uninstall msoms
```

## <a name="let-us-know"></a><span data-ttu-id="6147b-148">Berätta för oss!</span><span class="sxs-lookup"><span data-stu-id="6147b-148">Let us know!!!</span></span>
<span data-ttu-id="6147b-149">Vad fungerar?</span><span class="sxs-lookup"><span data-stu-id="6147b-149">What works?</span></span> <span data-ttu-id="6147b-150">Vad som saknas?</span><span class="sxs-lookup"><span data-stu-id="6147b-150">What is missing?</span></span> <span data-ttu-id="6147b-151">Vad behöver du för att detta ska vara användbara för dig?</span><span class="sxs-lookup"><span data-stu-id="6147b-151">What else do you need for this to be useful for you?</span></span> <span data-ttu-id="6147b-152">Berätta för oss på <a href="mailto:OMSContainers@microsoft.com">OMSContainers</a>.</span><span class="sxs-lookup"><span data-stu-id="6147b-152">Let us know at <a href="mailto:OMSContainers@microsoft.com">OMSContainers</a>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6147b-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6147b-153">Next steps</span></span>

 <span data-ttu-id="6147b-154">Nu när du har ställt in OMS att övervaka din behållare[finns i instrumentpanelen behållaren](../../log-analytics/log-analytics-containers.md).</span><span class="sxs-lookup"><span data-stu-id="6147b-154">Now that you have set up OMS to monitor your containers,[see your container dashboard](../../log-analytics/log-analytics-containers.md).</span></span>
