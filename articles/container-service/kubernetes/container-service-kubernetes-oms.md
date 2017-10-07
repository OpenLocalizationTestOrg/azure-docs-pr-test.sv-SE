---
title: aaaMonitor Azure Kubernetes kluster - Verksamhetsstyrning | Microsoft Docs
description: "Övervaka Kubernetes kluster i Azure Container Service med hjälp av Microsoft Operations Management Suite"
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
ms.openlocfilehash: 7474ee1571134ffe43ff8e4041cf5a64f5635bb7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-an-azure-container-service-cluster-with-microsoft-operations-management-suite-oms"></a><span data-ttu-id="35954-103">Övervaka ett Azure Container Service-kluster med Microsoft Operations Management Suite (OMS)</span><span class="sxs-lookup"><span data-stu-id="35954-103">Monitor an Azure Container Service cluster with Microsoft Operations Management Suite (OMS)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="35954-104">Krav</span><span class="sxs-lookup"><span data-stu-id="35954-104">Prerequisites</span></span>
<span data-ttu-id="35954-105">Den här genomgången förutsätter att du har [skapas ett Kubernetes-kluster med Azure Container Service](container-service-kubernetes-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="35954-105">This walkthrough assumes that you have [created a Kubernetes cluster using Azure Container Service](container-service-kubernetes-walkthrough.md).</span></span>

<span data-ttu-id="35954-106">Det förutsätts även att du har hello `az` Azure cli och `kubectl` verktygen som installeras.</span><span class="sxs-lookup"><span data-stu-id="35954-106">It also assumes that you have hello `az` Azure cli and `kubectl` tools installed.</span></span>

<span data-ttu-id="35954-107">Du kan testa om du har hello `az` installerat genom att köra verktyget:</span><span class="sxs-lookup"><span data-stu-id="35954-107">You can test if you have hello `az` tool installed by running:</span></span>

```console
$ az --version
```

<span data-ttu-id="35954-108">Om du inte har hello `az` verktyget är installerat, det finns instruktioner [här](https://github.com/azure/azure-cli#installation).</span><span class="sxs-lookup"><span data-stu-id="35954-108">If you don't have hello `az` tool installed, there are instructions [here](https://github.com/azure/azure-cli#installation).</span></span>  
<span data-ttu-id="35954-109">Du kan också använda [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview), som har hello `az` Azure cli och `kubectl` verktyg som redan har installerat för du.</span><span class="sxs-lookup"><span data-stu-id="35954-109">Alternatively, you can use [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview), which has hello `az` Azure cli and `kubectl` tools already installed for you.</span></span>  

<span data-ttu-id="35954-110">Du kan testa om du har hello `kubectl` installerat genom att köra verktyget:</span><span class="sxs-lookup"><span data-stu-id="35954-110">You can test if you have hello `kubectl` tool installed by running:</span></span>

```console
$ kubectl version
```

<span data-ttu-id="35954-111">Om du inte har `kubectl` installerat, kan du köra:</span><span class="sxs-lookup"><span data-stu-id="35954-111">If you don't have `kubectl` installed, you can run:</span></span>
```console
$ az acs kubernetes install-cli
```

<span data-ttu-id="35954-112">tootest om du har kubernetes nycklar som installeras i kubectl-verktyg som du kan köra:</span><span class="sxs-lookup"><span data-stu-id="35954-112">tootest if you have kubernetes keys installed in your kubectl tool you can run:</span></span>
```console
$ kubectl get nodes
```

<span data-ttu-id="35954-113">Om hello ovan kommandot fel ut, behöver du tooinstall kubernetes klustret nycklar till kubectl-verktyget.</span><span class="sxs-lookup"><span data-stu-id="35954-113">If hello above command errors out, you need tooinstall kubernetes cluster keys into your kubectl tool.</span></span> <span data-ttu-id="35954-114">Du kan göra det med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="35954-114">You can do that with hello following command:</span></span>
```console
RESOURCE_GROUP=my-resource-group
CLUSTER_NAME=my-acs-name
az acs kubernetes get-credentials --resource-group=$RESOURCE_GROUP --name=$CLUSTER_NAME
```

## <a name="monitoring-containers-with-operations-management-suite-oms"></a><span data-ttu-id="35954-115">Övervakning av behållare med Operations Management Suite (OMS)</span><span class="sxs-lookup"><span data-stu-id="35954-115">Monitoring Containers with Operations Management Suite (OMS)</span></span>

<span data-ttu-id="35954-116">Microsoft Operations Management (OMS) är Microsofts molnbaserade IT lösning som hjälper dig att hantera och skydda dina lokala och molnet infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="35954-116">Microsoft Operations Management (OMS) is Microsoft's cloud-based IT management solution that helps you manage and protect your on-premises and cloud infrastructure.</span></span> <span data-ttu-id="35954-117">Behållaren lösningen är en lösning i OMS logganalys som du kan visa hello behållaren lager, prestanda och loggar på en enda plats.</span><span class="sxs-lookup"><span data-stu-id="35954-117">Container Solution is a solution in OMS Log Analytics, which helps you view hello container inventory, performance, and logs in a single location.</span></span> <span data-ttu-id="35954-118">Du kan granska, Felsök behållare genom att visa hello loggar på central plats och hitta störningar förbrukar överdriven behållare på en värd.</span><span class="sxs-lookup"><span data-stu-id="35954-118">You can audit, troubleshoot containers by viewing hello logs in centralized location, and find noisy consuming excess container on a host.</span></span>

![](media/container-service-monitoring-oms/image1.png)

<span data-ttu-id="35954-119">Mer information om behållaren lösning hittar toothe [behållare lösning logganalys](../../log-analytics/log-analytics-containers.md).</span><span class="sxs-lookup"><span data-stu-id="35954-119">For more information about Container Solution, please refer toothe [Container Solution Log Analytics](../../log-analytics/log-analytics-containers.md).</span></span>

## <a name="installing-oms-on-kubernetes"></a><span data-ttu-id="35954-120">Installera OMS på Kubernetes</span><span class="sxs-lookup"><span data-stu-id="35954-120">Installing OMS on Kubernetes</span></span>

### <a name="obtain-your-workspace-id-and-key"></a><span data-ttu-id="35954-121">Hämta ditt arbetsyte-ID och nyckel</span><span class="sxs-lookup"><span data-stu-id="35954-121">Obtain your workspace ID and key</span></span>
<span data-ttu-id="35954-122">Hello OMS tootalk toohello-agenttjänsten måste toobe som konfigurerats med en arbetsyte-id och en arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="35954-122">For hello OMS agent tootalk toohello service it needs toobe configured with a workspace id and a workspace key.</span></span> <span data-ttu-id="35954-123">tooget hello arbetsyte-id och du behöver toocreate en OMS-kontot på <https://mms.microsoft.com>. Följ hello steg toocreate ett konto.</span><span class="sxs-lookup"><span data-stu-id="35954-123">tooget hello workspace id and key you need toocreate an OMS account at <https://mms.microsoft.com>. Please follow hello steps toocreate an account.</span></span> <span data-ttu-id="35954-124">När du är klar skapar hello-konto måste tooobtain ditt id och nyckel genom att klicka på **inställningar**, sedan **anslutna källor**, och sedan **Linux-servrar**, enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="35954-124">Once you are done creating hello account, you need tooobtain your id and key by clicking **Settings**, then **Connected Sources**, and then **Linux Servers**, as shown below.</span></span>

 ![](media/container-service-monitoring-oms/image5.png)

### <a name="install-hello-oms-agent-using-a-daemonset"></a><span data-ttu-id="35954-125">Installera hello OMS-agent med en DaemonSet</span><span class="sxs-lookup"><span data-stu-id="35954-125">Install hello OMS agent using a DaemonSet</span></span>
<span data-ttu-id="35954-126">DaemonSets används av Kubernetes toorun en instans av en behållare på varje värd i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="35954-126">DaemonSets are used by Kubernetes toorun a single instance of a container on each host in hello cluster.</span></span>
<span data-ttu-id="35954-127">Det är perfekt för att köra övervakningsagenter.</span><span class="sxs-lookup"><span data-stu-id="35954-127">They're perfect for running monitoring agents.</span></span>

<span data-ttu-id="35954-128">Här är hello [DaemonSet YAML filen](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes).</span><span class="sxs-lookup"><span data-stu-id="35954-128">Here is hello [DaemonSet YAML file](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes).</span></span> <span data-ttu-id="35954-129">Spara den tooa filen namnet `oms-daemonset.yaml` och Ersätt hello platshållare värden för `WSID` och `KEY` med ditt arbetsyte-id och nyckel i hello-filen.</span><span class="sxs-lookup"><span data-stu-id="35954-129">Save it tooa file named `oms-daemonset.yaml` and replace hello place-holder values for `WSID` and `KEY` with your workspace id and key in hello file.</span></span>

<span data-ttu-id="35954-130">När du har lagt till ditt arbetsyte-ID och nyckel toohello DaemonSet konfiguration, kan du installera hello OMS-agent på ditt kluster med hello `kubectl` kommandoradsverktyget:</span><span class="sxs-lookup"><span data-stu-id="35954-130">Once you have added your workspace ID and key toohello DaemonSet configuration, you can install hello OMS agent on your cluster with hello `kubectl` command line tool:</span></span>

```console
$ kubectl create -f oms-daemonset.yaml
```

### <a name="installing-hello-oms-agent-using-a-kubernetes-secret"></a><span data-ttu-id="35954-131">Installera med hjälp av en Kubernetes hemlighet hello OMS-agent</span><span class="sxs-lookup"><span data-stu-id="35954-131">Installing hello OMS agent using a Kubernetes Secret</span></span>
<span data-ttu-id="35954-132">tooprotect din OMS arbetsyte-ID och du kan använda Kubernetes hemlighet som en del av DaemonSet YAML-fil.</span><span class="sxs-lookup"><span data-stu-id="35954-132">tooprotect your OMS workspace ID and key you can use Kubernetes Secret as a part of DaemonSet YAML file.</span></span>

 - <span data-ttu-id="35954-133">Kopiera hello skript, hemliga mallfilen och hello DaemonSet YAML-fil (från [databasen](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes)) och kontrollera att de är på hello samma katalog.</span><span class="sxs-lookup"><span data-stu-id="35954-133">Copy hello script, secret template file and hello DaemonSet YAML file (from [repository](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes)) and make sure they are on hello same directory.</span></span> 
      - <span data-ttu-id="35954-134">Hemligt genererar skript - hemlighet gen.sh</span><span class="sxs-lookup"><span data-stu-id="35954-134">secret generating script - secret-gen.sh</span></span>
      - <span data-ttu-id="35954-135">Hemlig mall - hemlighet template.yaml</span><span class="sxs-lookup"><span data-stu-id="35954-135">secret template - secret-template.yaml</span></span>
   - <span data-ttu-id="35954-136">Filen DaemonSet YAML - omsagent-ds-secrets.yaml</span><span class="sxs-lookup"><span data-stu-id="35954-136">DaemonSet YAML file - omsagent-ds-secrets.yaml</span></span>
 - <span data-ttu-id="35954-137">Kör skriptet hello.</span><span class="sxs-lookup"><span data-stu-id="35954-137">Run hello script.</span></span> <span data-ttu-id="35954-138">hello skriptet begär hello OMS arbetsyte-ID och primärnyckel.</span><span class="sxs-lookup"><span data-stu-id="35954-138">hello script will ask for hello OMS Workspace ID and Primary Key.</span></span> <span data-ttu-id="35954-139">In sätt som och hello skriptet skapar en hemlig yaml-fil så att du kan köra den.</span><span class="sxs-lookup"><span data-stu-id="35954-139">Please insert that and hello script will create a secret yaml file so you can run it.</span></span>   
   ```
   #> sudo bash ./secret-gen.sh 
   ```

   - <span data-ttu-id="35954-140">Skapa hello hemligheter baljor genom att köra hello följande:``` kubectl create -f omsagentsecret.yaml ```</span><span class="sxs-lookup"><span data-stu-id="35954-140">Create hello secrets pod by running hello following: ``` kubectl create -f omsagentsecret.yaml ```</span></span>
 
   - <span data-ttu-id="35954-141">toocheck kör hello följande:</span><span class="sxs-lookup"><span data-stu-id="35954-141">toocheck, run hello following:</span></span> 

   ``` 
   root@ubuntu16-13db:~# kubectl get secrets
   NAME                  TYPE                                  DATA      AGE
   default-token-gvl91   kubernetes.io/service-account-token   3         50d
   omsagent-secret       Opaque                                2         1d
   root@ubuntu16-13db:~# kubectl describe secrets omsagent-secret
   Name:           omsagent-secret
   Namespace:      default
   Labels:         <none>
   Annotations:    <none>

   Type:   Opaque

   Data
   ====
   WSID:   36 bytes
   KEY:    88 bytes 
   ```
 
  - <span data-ttu-id="35954-142">Skapa din omsagent daemon set genom att köra``` kubectl create -f omsagent-ds-secrets.yaml ```</span><span class="sxs-lookup"><span data-stu-id="35954-142">Create your omsagent daemon-set by running ``` kubectl create -f omsagent-ds-secrets.yaml ```</span></span>

### <a name="conclusion"></a><span data-ttu-id="35954-143">Slutsats</span><span class="sxs-lookup"><span data-stu-id="35954-143">Conclusion</span></span>
<span data-ttu-id="35954-144">Klart!</span><span class="sxs-lookup"><span data-stu-id="35954-144">That's it!</span></span> <span data-ttu-id="35954-145">Efter några minuter bör du kunna toosee data som flödar tooyour OMS-instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="35954-145">After a few minutes, you should be able toosee data flowing tooyour OMS dashboard.</span></span>
