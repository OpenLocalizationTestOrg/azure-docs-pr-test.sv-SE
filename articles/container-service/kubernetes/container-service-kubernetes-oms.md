---
title: "Övervaka Azure Kubernetes kluster - Verksamhetsstyrning | Microsoft Docs"
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
ms.openlocfilehash: bd5c81435c091d25bc14710589b7c043e9f56a25
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-an-azure-container-service-cluster-with-microsoft-operations-management-suite-oms"></a><span data-ttu-id="d2ea8-103">Övervaka ett Azure Container Service-kluster med Microsoft Operations Management Suite (OMS)</span><span class="sxs-lookup"><span data-stu-id="d2ea8-103">Monitor an Azure Container Service cluster with Microsoft Operations Management Suite (OMS)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d2ea8-104">Krav</span><span class="sxs-lookup"><span data-stu-id="d2ea8-104">Prerequisites</span></span>
<span data-ttu-id="d2ea8-105">Den här genomgången förutsätter att du har [skapas ett Kubernetes-kluster med Azure Container Service](container-service-kubernetes-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="d2ea8-105">This walkthrough assumes that you have [created a Kubernetes cluster using Azure Container Service](container-service-kubernetes-walkthrough.md).</span></span>

<span data-ttu-id="d2ea8-106">Det förutsätts även att du har den `az` Azure cli och `kubectl` verktygen som installeras.</span><span class="sxs-lookup"><span data-stu-id="d2ea8-106">It also assumes that you have the `az` Azure cli and `kubectl` tools installed.</span></span>

<span data-ttu-id="d2ea8-107">Du kan testa om du har den `az` installerat genom att köra verktyget:</span><span class="sxs-lookup"><span data-stu-id="d2ea8-107">You can test if you have the `az` tool installed by running:</span></span>

```console
$ az --version
```

<span data-ttu-id="d2ea8-108">Om du inte har den `az` verktyget är installerat, det finns instruktioner [här](https://github.com/azure/azure-cli#installation).</span><span class="sxs-lookup"><span data-stu-id="d2ea8-108">If you don't have the `az` tool installed, there are instructions [here](https://github.com/azure/azure-cli#installation).</span></span>  
<span data-ttu-id="d2ea8-109">Du kan också använda [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview), som har den `az` Azure cli och `kubectl` verktyg som redan har installerat för du.</span><span class="sxs-lookup"><span data-stu-id="d2ea8-109">Alternatively, you can use [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview), which has the `az` Azure cli and `kubectl` tools already installed for you.</span></span>  

<span data-ttu-id="d2ea8-110">Du kan testa om du har den `kubectl` installerat genom att köra verktyget:</span><span class="sxs-lookup"><span data-stu-id="d2ea8-110">You can test if you have the `kubectl` tool installed by running:</span></span>

```console
$ kubectl version
```

<span data-ttu-id="d2ea8-111">Om du inte har `kubectl` installerat, kan du köra:</span><span class="sxs-lookup"><span data-stu-id="d2ea8-111">If you don't have `kubectl` installed, you can run:</span></span>
```console
$ az acs kubernetes install-cli
```

<span data-ttu-id="d2ea8-112">Kontrollera att du har kubernetes nycklar som installerats i din kubectl verktyg som du kan köra:</span><span class="sxs-lookup"><span data-stu-id="d2ea8-112">To test if you have kubernetes keys installed in your kubectl tool you can run:</span></span>
```console
$ kubectl get nodes
```

<span data-ttu-id="d2ea8-113">Om ovanstående kommando felen ut, måste du installera kubernetes klustret nycklar i kubectl-verktyget.</span><span class="sxs-lookup"><span data-stu-id="d2ea8-113">If the above command errors out, you need to install kubernetes cluster keys into your kubectl tool.</span></span> <span data-ttu-id="d2ea8-114">Du kan göra det med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="d2ea8-114">You can do that with the following command:</span></span>
```console
RESOURCE_GROUP=my-resource-group
CLUSTER_NAME=my-acs-name
az acs kubernetes get-credentials --resource-group=$RESOURCE_GROUP --name=$CLUSTER_NAME
```

## <a name="monitoring-containers-with-operations-management-suite-oms"></a><span data-ttu-id="d2ea8-115">Övervakning av behållare med Operations Management Suite (OMS)</span><span class="sxs-lookup"><span data-stu-id="d2ea8-115">Monitoring Containers with Operations Management Suite (OMS)</span></span>

<span data-ttu-id="d2ea8-116">Microsoft Operations Management (OMS) är Microsofts molnbaserade IT lösning som hjälper dig att hantera och skydda dina lokala och molnet infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="d2ea8-116">Microsoft Operations Management (OMS) is Microsoft's cloud-based IT management solution that helps you manage and protect your on-premises and cloud infrastructure.</span></span> <span data-ttu-id="d2ea8-117">Behållaren lösningen är en lösning i OMS logganalys, vilket gör det möjligt att visa behållaren inventering, prestanda och loggar på en enda plats.</span><span class="sxs-lookup"><span data-stu-id="d2ea8-117">Container Solution is a solution in OMS Log Analytics, which helps you view the container inventory, performance, and logs in a single location.</span></span> <span data-ttu-id="d2ea8-118">Du kan granska, Felsök behållare genom att visa loggarna på central plats och hitta störningar förbrukar överdriven behållare på en värd.</span><span class="sxs-lookup"><span data-stu-id="d2ea8-118">You can audit, troubleshoot containers by viewing the logs in centralized location, and find noisy consuming excess container on a host.</span></span>

![](media/container-service-monitoring-oms/image1.png)

<span data-ttu-id="d2ea8-119">Mer information om behållaren lösning hittar du den [behållare lösning logganalys](../../log-analytics/log-analytics-containers.md).</span><span class="sxs-lookup"><span data-stu-id="d2ea8-119">For more information about Container Solution, please refer to the [Container Solution Log Analytics](../../log-analytics/log-analytics-containers.md).</span></span>

## <a name="installing-oms-on-kubernetes"></a><span data-ttu-id="d2ea8-120">Installera OMS på Kubernetes</span><span class="sxs-lookup"><span data-stu-id="d2ea8-120">Installing OMS on Kubernetes</span></span>

### <a name="obtain-your-workspace-id-and-key"></a><span data-ttu-id="d2ea8-121">Hämta ditt arbetsyte-ID och nyckel</span><span class="sxs-lookup"><span data-stu-id="d2ea8-121">Obtain your workspace ID and key</span></span>
<span data-ttu-id="d2ea8-122">Agenten tala med tjänsten den måste konfigureras med en arbetsyte-id och en arbetsyta för OMS.</span><span class="sxs-lookup"><span data-stu-id="d2ea8-122">For the OMS agent to talk to the service it needs to be configured with a workspace id and a workspace key.</span></span> <span data-ttu-id="d2ea8-123">Få arbetsyte-id och nyckel måste du skapa en OMS-konto på <https://mms.microsoft.com>. Följ stegen för att skapa ett konto.</span><span class="sxs-lookup"><span data-stu-id="d2ea8-123">To get the workspace id and key you need to create an OMS account at <https://mms.microsoft.com>. Please follow the steps to create an account.</span></span> <span data-ttu-id="d2ea8-124">När du är klar att skapa kontot, måste du skaffa din id och nyckel genom att klicka på **inställningar**, sedan **anslutna källor**, och sedan **Linux-servrar**, enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="d2ea8-124">Once you are done creating the account, you need to obtain your id and key by clicking **Settings**, then **Connected Sources**, and then **Linux Servers**, as shown below.</span></span>

 ![](media/container-service-monitoring-oms/image5.png)

### <a name="install-the-oms-agent-using-a-daemonset"></a><span data-ttu-id="d2ea8-125">Installera OMS-agenten med hjälp av en DaemonSet</span><span class="sxs-lookup"><span data-stu-id="d2ea8-125">Install the OMS agent using a DaemonSet</span></span>
<span data-ttu-id="d2ea8-126">DaemonSets som används av Kubernetes för att köra en instans av en behållare på varje värd i klustret.</span><span class="sxs-lookup"><span data-stu-id="d2ea8-126">DaemonSets are used by Kubernetes to run a single instance of a container on each host in the cluster.</span></span>
<span data-ttu-id="d2ea8-127">Det är perfekt för att köra övervakningsagenter.</span><span class="sxs-lookup"><span data-stu-id="d2ea8-127">They're perfect for running monitoring agents.</span></span>

<span data-ttu-id="d2ea8-128">Här är den [DaemonSet YAML filen](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes).</span><span class="sxs-lookup"><span data-stu-id="d2ea8-128">Here is the [DaemonSet YAML file](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes).</span></span> <span data-ttu-id="d2ea8-129">Spara den på en fil med namnet `oms-daemonset.yaml` och ersätter värdena platshållare för `WSID` och `KEY` med ditt arbetsyte-id och nyckel i filen.</span><span class="sxs-lookup"><span data-stu-id="d2ea8-129">Save it to a file named `oms-daemonset.yaml` and replace the place-holder values for `WSID` and `KEY` with your workspace id and key in the file.</span></span>

<span data-ttu-id="d2ea8-130">När du har lagt till ditt arbetsyte-ID och nyckel DaemonSet konfigurationen kan du kan installera OMS-agenten på klustret med det `kubectl` kommandoradsverktyget:</span><span class="sxs-lookup"><span data-stu-id="d2ea8-130">Once you have added your workspace ID and key to the DaemonSet configuration, you can install the OMS agent on your cluster with the `kubectl` command line tool:</span></span>

```console
$ kubectl create -f oms-daemonset.yaml
```

### <a name="installing-the-oms-agent-using-a-kubernetes-secret"></a><span data-ttu-id="d2ea8-131">Installerar OMS-agenten med hjälp av en Kubernetes hemlighet</span><span class="sxs-lookup"><span data-stu-id="d2ea8-131">Installing the OMS agent using a Kubernetes Secret</span></span>
<span data-ttu-id="d2ea8-132">För att skydda din OMS arbetsyte-ID och du kan använda Kubernetes hemlighet som en del av DaemonSet YAML-fil.</span><span class="sxs-lookup"><span data-stu-id="d2ea8-132">To protect your OMS workspace ID and key you can use Kubernetes Secret as a part of DaemonSet YAML file.</span></span>

 - <span data-ttu-id="d2ea8-133">Kopiera skriptet, hemliga mallfilen och DaemonSet YAML-filen (från [databasen](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes)) och kontrollera att de finns i samma katalog.</span><span class="sxs-lookup"><span data-stu-id="d2ea8-133">Copy the script, secret template file and the DaemonSet YAML file (from [repository](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes)) and make sure they are on the same directory.</span></span> 
      - <span data-ttu-id="d2ea8-134">Hemligt genererar skript - hemlighet gen.sh</span><span class="sxs-lookup"><span data-stu-id="d2ea8-134">secret generating script - secret-gen.sh</span></span>
      - <span data-ttu-id="d2ea8-135">Hemlig mall - hemlighet template.yaml</span><span class="sxs-lookup"><span data-stu-id="d2ea8-135">secret template - secret-template.yaml</span></span>
   - <span data-ttu-id="d2ea8-136">Filen DaemonSet YAML - omsagent-ds-secrets.yaml</span><span class="sxs-lookup"><span data-stu-id="d2ea8-136">DaemonSet YAML file - omsagent-ds-secrets.yaml</span></span>
 - <span data-ttu-id="d2ea8-137">Kör skriptet.</span><span class="sxs-lookup"><span data-stu-id="d2ea8-137">Run the script.</span></span> <span data-ttu-id="d2ea8-138">Skriptet begär OMS arbetsyte-ID och primärnyckel.</span><span class="sxs-lookup"><span data-stu-id="d2ea8-138">The script will ask for the OMS Workspace ID and Primary Key.</span></span> <span data-ttu-id="d2ea8-139">In sätt som och skriptet skapar en hemlig yaml-fil så att du kan köra den.</span><span class="sxs-lookup"><span data-stu-id="d2ea8-139">Please insert that and the script will create a secret yaml file so you can run it.</span></span>   
   ```
   #> sudo bash ./secret-gen.sh 
   ```

   - <span data-ttu-id="d2ea8-140">Skapa hemligheter baljor genom att köra följande:``` kubectl create -f omsagentsecret.yaml ```</span><span class="sxs-lookup"><span data-stu-id="d2ea8-140">Create the secrets pod by running the following: ``` kubectl create -f omsagentsecret.yaml ```</span></span>
 
   - <span data-ttu-id="d2ea8-141">Om du vill kontrollera, kör du följande:</span><span class="sxs-lookup"><span data-stu-id="d2ea8-141">To check, run the following:</span></span> 

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
 
  - <span data-ttu-id="d2ea8-142">Skapa din omsagent daemon set genom att köra``` kubectl create -f omsagent-ds-secrets.yaml ```</span><span class="sxs-lookup"><span data-stu-id="d2ea8-142">Create your omsagent daemon-set by running ``` kubectl create -f omsagent-ds-secrets.yaml ```</span></span>

### <a name="conclusion"></a><span data-ttu-id="d2ea8-143">Slutsats</span><span class="sxs-lookup"><span data-stu-id="d2ea8-143">Conclusion</span></span>
<span data-ttu-id="d2ea8-144">Klart!</span><span class="sxs-lookup"><span data-stu-id="d2ea8-144">That's it!</span></span> <span data-ttu-id="d2ea8-145">Efter några minuter, bör du kunna se data som flödar till OMS-instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="d2ea8-145">After a few minutes, you should be able to see data flowing to your OMS dashboard.</span></span>
