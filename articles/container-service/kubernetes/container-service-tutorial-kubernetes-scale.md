---
title: "Självstudier för aaaAzure Container Service - skala program | Microsoft Docs"
description: "Självstudiekurs för Azure Container Service - skala program"
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker behållare Micro-tjänster, Kubernetes, Azure"
ms.assetid: 
ms.service: container-service
ms.devlang: aurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 29571eef0fd91bd6b40f00d8c018539f320179bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scale-kubernetes-pods-and-kubernetes-infrastructure"></a><span data-ttu-id="68eeb-104">Skala Kubernetes skida och Kubernetes infrastruktur</span><span class="sxs-lookup"><span data-stu-id="68eeb-104">Scale Kubernetes pods and Kubernetes infrastructure</span></span>

<span data-ttu-id="68eeb-105">Om du har följande hello självstudiekurser du ha en fungerande Kubernetes kluster i Azure Container Service och du har distribuerat hello Azure röstning app.</span><span class="sxs-lookup"><span data-stu-id="68eeb-105">If you've been following hello tutorials, you have a working Kubernetes cluster in Azure Container Service and you deployed hello Azure Voting app.</span></span> 

<span data-ttu-id="68eeb-106">I den här självstudiekursen tillhör fem sju, skala ut hello skida i hello app och försök baljor autoskalning.</span><span class="sxs-lookup"><span data-stu-id="68eeb-106">In this tutorial, part five of seven, you scale out hello pods in hello app and try pod autoscaling.</span></span> <span data-ttu-id="68eeb-107">Du lär dig också hur tooscale hello antal Virtuella Azure-agenten noder toochange hello klustrets kapacitet för värd för arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="68eeb-107">You also learn how tooscale hello number of Azure VM agent nodes toochange hello cluster's capacity for hosting workloads.</span></span> <span data-ttu-id="68eeb-108">Aktiviteter har slutförts är:</span><span class="sxs-lookup"><span data-stu-id="68eeb-108">Tasks completed include:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="68eeb-109">Skalning Kubernetes skida manuellt</span><span class="sxs-lookup"><span data-stu-id="68eeb-109">Manually scaling Kubernetes pods</span></span>
> * <span data-ttu-id="68eeb-110">Konfigurera Autoskala skida kör hello app klientdel</span><span class="sxs-lookup"><span data-stu-id="68eeb-110">Configuring Autoscale pods running hello app front end</span></span>
> * <span data-ttu-id="68eeb-111">Skala hello Kubernetes Azure-agenten noder</span><span class="sxs-lookup"><span data-stu-id="68eeb-111">Scale hello Kubernetes Azure agent nodes</span></span>

<span data-ttu-id="68eeb-112">Hello Azure rösten programmet uppdateras i efterföljande självstudiekurser och Operations Management Suite konfigurerad toomonitor hello Kubernetes klustret.</span><span class="sxs-lookup"><span data-stu-id="68eeb-112">In subsequent tutorials, hello Azure Vote application is updated, and Operations Management Suite configured toomonitor hello Kubernetes cluster.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="68eeb-113">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="68eeb-113">Before you begin</span></span>

<span data-ttu-id="68eeb-114">I tidigare självstudier, ett program som har paketerats till en behållare bild, den här avbildningen överfört tooAzure behållare registret och ett Kubernetes kluster skapas.</span><span class="sxs-lookup"><span data-stu-id="68eeb-114">In previous tutorials, an application was packaged into a container image, this image uploaded tooAzure Container Registry, and a Kubernetes cluster created.</span></span> <span data-ttu-id="68eeb-115">hello programmet körs sedan hello Kubernetes klustret.</span><span class="sxs-lookup"><span data-stu-id="68eeb-115">hello application was then run on hello Kubernetes cluster.</span></span> <span data-ttu-id="68eeb-116">Om du inte har gjort dessa steg och vill toofollow längs, returnerar toohello [kursen 1 – skapa behållaren bilder](./container-service-tutorial-kubernetes-prepare-app.md).</span><span class="sxs-lookup"><span data-stu-id="68eeb-116">If you have not done these steps, and would like toofollow along, return toohello [Tutorial 1 – Create container images](./container-service-tutorial-kubernetes-prepare-app.md).</span></span> 

<span data-ttu-id="68eeb-117">Kursen krävs åtminstone ett Kubernetes kluster med ett program som körs.</span><span class="sxs-lookup"><span data-stu-id="68eeb-117">At minimum, this tutorial requires a Kubernetes cluster with a running application.</span></span>

## <a name="manually-scale-pods"></a><span data-ttu-id="68eeb-118">Skala skida manuellt</span><span class="sxs-lookup"><span data-stu-id="68eeb-118">Manually scale pods</span></span>

<span data-ttu-id="68eeb-119">Hittills, hello Azure rösten frontend och Redis-instansen har distribuerats, var och en med en enskild replik.</span><span class="sxs-lookup"><span data-stu-id="68eeb-119">Thus far, hello Azure Vote front-end and Redis instance have been deployed, each with a single replica.</span></span> <span data-ttu-id="68eeb-120">tooverify, kör hello [kubectl hämta](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) kommando.</span><span class="sxs-lookup"><span data-stu-id="68eeb-120">tooverify, run hello [kubectl get](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) command.</span></span>

```azurecli-interactive
kubectl get pods
```

<span data-ttu-id="68eeb-121">Resultat:</span><span class="sxs-lookup"><span data-stu-id="68eeb-121">Output:</span></span>

```bash
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2549686872-4d2r5   1/1       Running   0          31m
azure-vote-front-848767080-tf34m   1/1       Running   0          31m
```

<span data-ttu-id="68eeb-122">Manuellt ändra hello antal skida i hello `azure-vote-front` distribution med hello [kubectl skala](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#scale) kommando.</span><span class="sxs-lookup"><span data-stu-id="68eeb-122">Manually change hello number of pods in hello `azure-vote-front` deployment using hello [kubectl scale](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#scale) command.</span></span> <span data-ttu-id="68eeb-123">Det här exemplet ökar hello nummer too5.</span><span class="sxs-lookup"><span data-stu-id="68eeb-123">This example increases hello number too5.</span></span>

```azurecli-interactive
kubectl scale --replicas=5 deployment/azure-vote-front
```

<span data-ttu-id="68eeb-124">Kör [kubectl hämta skida](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) tooverify att Kubernetes skapar hello skida.</span><span class="sxs-lookup"><span data-stu-id="68eeb-124">Run [kubectl get pods](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) tooverify that Kubernetes is creating hello pods.</span></span> <span data-ttu-id="68eeb-125">Efter en minut eller så körs hello ytterligare skida:</span><span class="sxs-lookup"><span data-stu-id="68eeb-125">After a minute or so, hello additional pods are running:</span></span>

```azurecli-interactive
kubectl get pods
```

<span data-ttu-id="68eeb-126">Resultat:</span><span class="sxs-lookup"><span data-stu-id="68eeb-126">Output:</span></span>

```bash
NAME                                READY     STATUS    RESTARTS   AGE
azure-vote-back-2606967446-nmpcf    1/1       Running   0          15m
azure-vote-front-3309479140-2hfh0   1/1       Running   0          3m
azure-vote-front-3309479140-bzt05   1/1       Running   0          3m
azure-vote-front-3309479140-fvcvm   1/1       Running   0          3m
azure-vote-front-3309479140-hrbf2   1/1       Running   0          15m
azure-vote-front-3309479140-qphz8   1/1       Running   0          3m
```

## <a name="autoscale-pods"></a><span data-ttu-id="68eeb-127">Autoskala skida</span><span class="sxs-lookup"><span data-stu-id="68eeb-127">Autoscale pods</span></span>

<span data-ttu-id="68eeb-128">Har stöd för Kubernetes [vågräta baljor autoskalning](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) tooadjust hello antalet skida i en distribution beroende på CPU-användning eller annan välja mått.</span><span class="sxs-lookup"><span data-stu-id="68eeb-128">Kubernetes supports [horizontal pod autoscaling](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) tooadjust hello number of pods in a deployment depending on CPU utilization or other select metrics.</span></span> 

<span data-ttu-id="68eeb-129">toouse hello autoscaler din skida måste ha CPU-begäranden och gränser har definierats.</span><span class="sxs-lookup"><span data-stu-id="68eeb-129">toouse hello autoscaler, your pods must have CPU requests and limits defined.</span></span> <span data-ttu-id="68eeb-130">I hello `azure-vote-front` distribution hello frontend behållaren begäranden 0,25 processor, med högst 0,5 CPU.</span><span class="sxs-lookup"><span data-stu-id="68eeb-130">In hello `azure-vote-front` deployment, hello front-end container requests 0.25 CPU, with a limit of 0.5 CPU.</span></span> <span data-ttu-id="68eeb-131">Det ser ut som hello inställningar:</span><span class="sxs-lookup"><span data-stu-id="68eeb-131">hello settings look like:</span></span>

```YAML
resources:
  requests:
     cpu: 250m
  limits:
     cpu: 500m
```

<span data-ttu-id="68eeb-132">hello följande exempel används hello [kubectl Autoskala](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#autoscale) kommandot tooautoscale hello antalet skida i hello `azure-vote-front` distribution.</span><span class="sxs-lookup"><span data-stu-id="68eeb-132">hello following example uses hello [kubectl autoscale](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#autoscale) command tooautoscale hello number of pods in hello `azure-vote-front` deployment.</span></span> <span data-ttu-id="68eeb-133">Om processoranvändningen överskrider 50%, ökar hello autoscaler här hello skida tooa högst 10.</span><span class="sxs-lookup"><span data-stu-id="68eeb-133">Here, if CPU utilization exceeds 50%, hello autoscaler increases hello pods tooa maximum of 10.</span></span>


```azurecli-interactive
kubectl autoscale deployment azure-vote-front --cpu-percent=50 --min=3 --max=10
```

<span data-ttu-id="68eeb-134">toosee hello status för hello autoscaler, kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="68eeb-134">toosee hello status of hello autoscaler, run hello following command:</span></span>

```azurecli-interactive
kubectl get hpa
```

<span data-ttu-id="68eeb-135">Resultat:</span><span class="sxs-lookup"><span data-stu-id="68eeb-135">Output:</span></span>

```bash
NAME               REFERENCE                     TARGETS    MINPODS   MAXPODS   REPLICAS   AGE
azure-vote-front   Deployment/azure-vote-front   0% / 50%   3         10        3          2m
```

<span data-ttu-id="68eeb-136">Efter några minuter med minimal belastning på hello Azure rösten app minskar hello antal baljor repliker automatiskt too3.</span><span class="sxs-lookup"><span data-stu-id="68eeb-136">After a few minutes, with minimal load on hello Azure Vote app, hello number of pod replicas decreases automatically too3.</span></span>

## <a name="scale-hello-agents"></a><span data-ttu-id="68eeb-137">Skala hello agenter</span><span class="sxs-lookup"><span data-stu-id="68eeb-137">Scale hello agents</span></span>

<span data-ttu-id="68eeb-138">Om du har skapat klustret Kubernetes standard kommandona i hello tidigare kursen har tre agent-noder.</span><span class="sxs-lookup"><span data-stu-id="68eeb-138">If you created your Kubernetes cluster using default commands in hello previous tutorial, it has three agent nodes.</span></span> <span data-ttu-id="68eeb-139">Du kan justera hello antalet agenter manuellt om du planerar fler eller färre arbetsbelastningar i behållare på ditt kluster.</span><span class="sxs-lookup"><span data-stu-id="68eeb-139">You can adjust hello number of agents manually if you plan more or fewer container workloads on your cluster.</span></span> <span data-ttu-id="68eeb-140">Använd hello [az acs skala](/cli/azure/acs#scale) kommando och ange hello antalet agenter med hello `--new-agent-count` parameter.</span><span class="sxs-lookup"><span data-stu-id="68eeb-140">Use hello [az acs scale](/cli/azure/acs#scale) command, and specify hello number of agents with hello `--new-agent-count` parameter.</span></span>

<span data-ttu-id="68eeb-141">hello följande exempel ökar hello antalet agent noder too4 i hello Kubernetes kluster med namnet *myK8sCluster*.</span><span class="sxs-lookup"><span data-stu-id="68eeb-141">hello following example increases hello number of agent nodes too4 in hello Kubernetes cluster named *myK8sCluster*.</span></span> <span data-ttu-id="68eeb-142">hello kommandot tar några minuter toocomplete.</span><span class="sxs-lookup"><span data-stu-id="68eeb-142">hello command takes a couple of minutes toocomplete.</span></span>

```azurecli-interactive
az acs scale --resource-group=myResourceGroup --name=myK8SCluster --new-agent-count 4
```

<span data-ttu-id="68eeb-143">hello kommandoutdata visar hello antalet agent noder i hello värdet av `agentPoolProfiles:count`:</span><span class="sxs-lookup"><span data-stu-id="68eeb-143">hello command output shows hello number of agent nodes in hello value of `agentPoolProfiles:count`:</span></span>

```azurecli
{
  "agentPoolProfiles": [
    {
      "count": 4,
      "dnsPrefix": "myK8SCluster-myK8SCluster-e44f25-k8s-agents",
      "fqdn": "",
      "name": "agentpools",
      "vmSize": "Standard_D2_v2"
    }
  ],
...

```

## <a name="next-steps"></a><span data-ttu-id="68eeb-144">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="68eeb-144">Next steps</span></span>

<span data-ttu-id="68eeb-145">I den här kursen används olika skalning funktioner i Kubernetes klustret.</span><span class="sxs-lookup"><span data-stu-id="68eeb-145">In this tutorial, you used different scaling features in your Kubernetes cluster.</span></span> <span data-ttu-id="68eeb-146">Uppgifter som omfattas ingår:</span><span class="sxs-lookup"><span data-stu-id="68eeb-146">Tasks covered included:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="68eeb-147">Skalning Kubernetes skida manuellt</span><span class="sxs-lookup"><span data-stu-id="68eeb-147">Manually scaling Kubernetes pods</span></span>
> * <span data-ttu-id="68eeb-148">Konfigurera Autoskala skida kör hello app klientdel</span><span class="sxs-lookup"><span data-stu-id="68eeb-148">Configuring Autoscale pods running hello app front end</span></span>
> * <span data-ttu-id="68eeb-149">Skala hello Kubernetes Azure-agenten noder</span><span class="sxs-lookup"><span data-stu-id="68eeb-149">Scale hello Kubernetes Azure agent nodes</span></span>

<span data-ttu-id="68eeb-150">Nästa självstudiekurs toolearn toohello om att uppdatera programmet i Kubernetes i förväg.</span><span class="sxs-lookup"><span data-stu-id="68eeb-150">Advance toohello next tutorial toolearn about updating application in Kubernetes.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="68eeb-151">Uppdatera ett program i Kubernetes</span><span class="sxs-lookup"><span data-stu-id="68eeb-151">Update an application in Kubernetes</span></span>](./container-service-tutorial-kubernetes-app-update.md)

