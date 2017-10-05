---
title: "Självstudiekurs för Azure Container Service - skala program | Microsoft Docs"
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
ms.openlocfilehash: 62e70e34d06f5220734ff85c70a0c9b475f9579b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="scale-kubernetes-pods-and-kubernetes-infrastructure"></a><span data-ttu-id="a8f1a-104">Skala Kubernetes skida och Kubernetes infrastruktur</span><span class="sxs-lookup"><span data-stu-id="a8f1a-104">Scale Kubernetes pods and Kubernetes infrastructure</span></span>

<span data-ttu-id="a8f1a-105">Om du har följande självstudierna du ha en fungerande Kubernetes kluster i Azure Container Service och du har distribuerat appen Azure röstning.</span><span class="sxs-lookup"><span data-stu-id="a8f1a-105">If you've been following the tutorials, you have a working Kubernetes cluster in Azure Container Service and you deployed the Azure Voting app.</span></span> 

<span data-ttu-id="a8f1a-106">I den här självstudiekursen tillhör fem sju, skala ut skida i appen och försök baljor autoskalning.</span><span class="sxs-lookup"><span data-stu-id="a8f1a-106">In this tutorial, part five of seven, you scale out the pods in the app and try pod autoscaling.</span></span> <span data-ttu-id="a8f1a-107">Du också lära dig hur du skalar antalet noder som Virtuella Azure-agenten att ändra klustrets kapacitet för värd för arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="a8f1a-107">You also learn how to scale the number of Azure VM agent nodes to change the cluster's capacity for hosting workloads.</span></span> <span data-ttu-id="a8f1a-108">Aktiviteter har slutförts är:</span><span class="sxs-lookup"><span data-stu-id="a8f1a-108">Tasks completed include:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a8f1a-109">Skalning Kubernetes skida manuellt</span><span class="sxs-lookup"><span data-stu-id="a8f1a-109">Manually scaling Kubernetes pods</span></span>
> * <span data-ttu-id="a8f1a-110">Konfigurera Autoskala skida kör app klientdelen</span><span class="sxs-lookup"><span data-stu-id="a8f1a-110">Configuring Autoscale pods running the app front end</span></span>
> * <span data-ttu-id="a8f1a-111">Skala Kubernetes Azure agent noder</span><span class="sxs-lookup"><span data-stu-id="a8f1a-111">Scale the Kubernetes Azure agent nodes</span></span>

<span data-ttu-id="a8f1a-112">Programmet Azure rösten uppdateras i efterföljande självstudiekurser och Operations Management Suite som konfigurerats för att övervaka Kubernetes klustret.</span><span class="sxs-lookup"><span data-stu-id="a8f1a-112">In subsequent tutorials, the Azure Vote application is updated, and Operations Management Suite configured to monitor the Kubernetes cluster.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="a8f1a-113">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="a8f1a-113">Before you begin</span></span>

<span data-ttu-id="a8f1a-114">I föregående självstudier, ett program som har paketerats i en behållare avbildning, avbildningen har överförts till registret för Azure-behållaren och ett Kubernetes kluster skapas.</span><span class="sxs-lookup"><span data-stu-id="a8f1a-114">In previous tutorials, an application was packaged into a container image, this image uploaded to Azure Container Registry, and a Kubernetes cluster created.</span></span> <span data-ttu-id="a8f1a-115">Programmet körs sedan Kubernetes klustret.</span><span class="sxs-lookup"><span data-stu-id="a8f1a-115">The application was then run on the Kubernetes cluster.</span></span> <span data-ttu-id="a8f1a-116">Om du inte har gjort dessa steg och vill följa med, gå tillbaka till den [kursen 1 – skapa behållaren bilder](./container-service-tutorial-kubernetes-prepare-app.md).</span><span class="sxs-lookup"><span data-stu-id="a8f1a-116">If you have not done these steps, and would like to follow along, return to the [Tutorial 1 – Create container images](./container-service-tutorial-kubernetes-prepare-app.md).</span></span> 

<span data-ttu-id="a8f1a-117">Kursen krävs åtminstone ett Kubernetes kluster med ett program som körs.</span><span class="sxs-lookup"><span data-stu-id="a8f1a-117">At minimum, this tutorial requires a Kubernetes cluster with a running application.</span></span>

## <a name="manually-scale-pods"></a><span data-ttu-id="a8f1a-118">Skala skida manuellt</span><span class="sxs-lookup"><span data-stu-id="a8f1a-118">Manually scale pods</span></span>

<span data-ttu-id="a8f1a-119">Därmed distribuerats långt Azure rösten frontend och Redis-instans har, var och en med en enskild replik.</span><span class="sxs-lookup"><span data-stu-id="a8f1a-119">Thus far, the Azure Vote front-end and Redis instance have been deployed, each with a single replica.</span></span> <span data-ttu-id="a8f1a-120">Du kan kontrollera genom att köra den [kubectl hämta](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) kommando.</span><span class="sxs-lookup"><span data-stu-id="a8f1a-120">To verify, run the [kubectl get](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) command.</span></span>

```azurecli-interactive
kubectl get pods
```

<span data-ttu-id="a8f1a-121">Resultat:</span><span class="sxs-lookup"><span data-stu-id="a8f1a-121">Output:</span></span>

```bash
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2549686872-4d2r5   1/1       Running   0          31m
azure-vote-front-848767080-tf34m   1/1       Running   0          31m
```

<span data-ttu-id="a8f1a-122">Manuellt ändra antalet skida i den `azure-vote-front` distribution med den [kubectl skala](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#scale) kommando.</span><span class="sxs-lookup"><span data-stu-id="a8f1a-122">Manually change the number of pods in the `azure-vote-front` deployment using the [kubectl scale](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#scale) command.</span></span> <span data-ttu-id="a8f1a-123">Det här exemplet ökar antalet till 5.</span><span class="sxs-lookup"><span data-stu-id="a8f1a-123">This example increases the number to 5.</span></span>

```azurecli-interactive
kubectl scale --replicas=5 deployment/azure-vote-front
```

<span data-ttu-id="a8f1a-124">Kör [kubectl hämta skida](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) att verifiera att Kubernetes skapar skida.</span><span class="sxs-lookup"><span data-stu-id="a8f1a-124">Run [kubectl get pods](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) to verify that Kubernetes is creating the pods.</span></span> <span data-ttu-id="a8f1a-125">När en minut eller så kan kör ytterligare skida:</span><span class="sxs-lookup"><span data-stu-id="a8f1a-125">After a minute or so, the additional pods are running:</span></span>

```azurecli-interactive
kubectl get pods
```

<span data-ttu-id="a8f1a-126">Resultat:</span><span class="sxs-lookup"><span data-stu-id="a8f1a-126">Output:</span></span>

```bash
NAME                                READY     STATUS    RESTARTS   AGE
azure-vote-back-2606967446-nmpcf    1/1       Running   0          15m
azure-vote-front-3309479140-2hfh0   1/1       Running   0          3m
azure-vote-front-3309479140-bzt05   1/1       Running   0          3m
azure-vote-front-3309479140-fvcvm   1/1       Running   0          3m
azure-vote-front-3309479140-hrbf2   1/1       Running   0          15m
azure-vote-front-3309479140-qphz8   1/1       Running   0          3m
```

## <a name="autoscale-pods"></a><span data-ttu-id="a8f1a-127">Autoskala skida</span><span class="sxs-lookup"><span data-stu-id="a8f1a-127">Autoscale pods</span></span>

<span data-ttu-id="a8f1a-128">Har stöd för Kubernetes [vågräta baljor autoskalning](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) för att justera antalet skida i en distribution beroende på CPU-användning eller annan välja mått.</span><span class="sxs-lookup"><span data-stu-id="a8f1a-128">Kubernetes supports [horizontal pod autoscaling](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) to adjust the number of pods in a deployment depending on CPU utilization or other select metrics.</span></span> 

<span data-ttu-id="a8f1a-129">Om du vill använda autoscaler ha din skida CPU-begäranden och gränser har definierats.</span><span class="sxs-lookup"><span data-stu-id="a8f1a-129">To use the autoscaler, your pods must have CPU requests and limits defined.</span></span> <span data-ttu-id="a8f1a-130">I den `azure-vote-front` distribution, behållaren frontend begäranden 0,25 processor, med högst 0,5 CPU.</span><span class="sxs-lookup"><span data-stu-id="a8f1a-130">In the `azure-vote-front` deployment, the front-end container requests 0.25 CPU, with a limit of 0.5 CPU.</span></span> <span data-ttu-id="a8f1a-131">Det ser ut som inställningarna:</span><span class="sxs-lookup"><span data-stu-id="a8f1a-131">The settings look like:</span></span>

```YAML
resources:
  requests:
     cpu: 250m
  limits:
     cpu: 500m
```

<span data-ttu-id="a8f1a-132">I följande exempel används den [kubectl Autoskala](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#autoscale) kommandot Autoskala antalet skida i den `azure-vote-front` distribution.</span><span class="sxs-lookup"><span data-stu-id="a8f1a-132">The following example uses the [kubectl autoscale](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#autoscale) command to autoscale the number of pods in the `azure-vote-front` deployment.</span></span> <span data-ttu-id="a8f1a-133">Om processoranvändningen överskrider 50%, ökar autoscaler här skida högst 10.</span><span class="sxs-lookup"><span data-stu-id="a8f1a-133">Here, if CPU utilization exceeds 50%, the autoscaler increases the pods to a maximum of 10.</span></span>


```azurecli-interactive
kubectl autoscale deployment azure-vote-front --cpu-percent=50 --min=3 --max=10
```

<span data-ttu-id="a8f1a-134">Om du vill se status för autoscaler, kör du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="a8f1a-134">To see the status of the autoscaler, run the following command:</span></span>

```azurecli-interactive
kubectl get hpa
```

<span data-ttu-id="a8f1a-135">Resultat:</span><span class="sxs-lookup"><span data-stu-id="a8f1a-135">Output:</span></span>

```bash
NAME               REFERENCE                     TARGETS    MINPODS   MAXPODS   REPLICAS   AGE
azure-vote-front   Deployment/azure-vote-front   0% / 50%   3         10        3          2m
```

<span data-ttu-id="a8f1a-136">Efter några minuter med minimal belastning på appen Azure röst minskar antalet baljor replikerna automatiskt till 3.</span><span class="sxs-lookup"><span data-stu-id="a8f1a-136">After a few minutes, with minimal load on the Azure Vote app, the number of pod replicas decreases automatically to 3.</span></span>

## <a name="scale-the-agents"></a><span data-ttu-id="a8f1a-137">Skala agenter</span><span class="sxs-lookup"><span data-stu-id="a8f1a-137">Scale the agents</span></span>

<span data-ttu-id="a8f1a-138">Om du har skapat klustret Kubernetes standard kommandona i föregående självstudiekursen har tre agent-noder.</span><span class="sxs-lookup"><span data-stu-id="a8f1a-138">If you created your Kubernetes cluster using default commands in the previous tutorial, it has three agent nodes.</span></span> <span data-ttu-id="a8f1a-139">Du kan justera antalet agenter manuellt om du planerar fler eller färre arbetsbelastningar i behållare på ditt kluster.</span><span class="sxs-lookup"><span data-stu-id="a8f1a-139">You can adjust the number of agents manually if you plan more or fewer container workloads on your cluster.</span></span> <span data-ttu-id="a8f1a-140">Använd den [az acs skala](/cli/azure/acs#scale) kommando och ange hur många agenter med den `--new-agent-count` parameter.</span><span class="sxs-lookup"><span data-stu-id="a8f1a-140">Use the [az acs scale](/cli/azure/acs#scale) command, and specify the number of agents with the `--new-agent-count` parameter.</span></span>

<span data-ttu-id="a8f1a-141">I följande exempel ökar antalet noder som agenten till 4 i Kubernetes klustret med namnet *myK8sCluster*.</span><span class="sxs-lookup"><span data-stu-id="a8f1a-141">The following example increases the number of agent nodes to 4 in the Kubernetes cluster named *myK8sCluster*.</span></span> <span data-ttu-id="a8f1a-142">Kommandot tar några minuter att slutföra.</span><span class="sxs-lookup"><span data-stu-id="a8f1a-142">The command takes a couple of minutes to complete.</span></span>

```azurecli-interactive
az acs scale --resource-group=myResourceGroup --name=myK8SCluster --new-agent-count 4
```

<span data-ttu-id="a8f1a-143">Kommandoutdata visar antalet agent noder i värdet för `agentPoolProfiles:count`:</span><span class="sxs-lookup"><span data-stu-id="a8f1a-143">The command output shows the number of agent nodes in the value of `agentPoolProfiles:count`:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="a8f1a-144">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a8f1a-144">Next steps</span></span>

<span data-ttu-id="a8f1a-145">I den här kursen används olika skalning funktioner i Kubernetes klustret.</span><span class="sxs-lookup"><span data-stu-id="a8f1a-145">In this tutorial, you used different scaling features in your Kubernetes cluster.</span></span> <span data-ttu-id="a8f1a-146">Uppgifter som omfattas ingår:</span><span class="sxs-lookup"><span data-stu-id="a8f1a-146">Tasks covered included:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a8f1a-147">Skalning Kubernetes skida manuellt</span><span class="sxs-lookup"><span data-stu-id="a8f1a-147">Manually scaling Kubernetes pods</span></span>
> * <span data-ttu-id="a8f1a-148">Konfigurera Autoskala skida kör app klientdelen</span><span class="sxs-lookup"><span data-stu-id="a8f1a-148">Configuring Autoscale pods running the app front end</span></span>
> * <span data-ttu-id="a8f1a-149">Skala Kubernetes Azure agent noder</span><span class="sxs-lookup"><span data-stu-id="a8f1a-149">Scale the Kubernetes Azure agent nodes</span></span>

<span data-ttu-id="a8f1a-150">Gå vidare till nästa kurs att lära dig om att uppdatera programmet i Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="a8f1a-150">Advance to the next tutorial to learn about updating application in Kubernetes.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="a8f1a-151">Uppdatera ett program i Kubernetes</span><span class="sxs-lookup"><span data-stu-id="a8f1a-151">Update an application in Kubernetes</span></span>](./container-service-tutorial-kubernetes-app-update.md)

