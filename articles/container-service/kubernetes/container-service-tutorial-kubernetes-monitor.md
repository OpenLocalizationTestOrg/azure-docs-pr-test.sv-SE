---
title: "Självstudier för aaaAzure Container Service - övervakaren Kubernetes | Microsoft Docs"
description: "Självstudiekurs för Azure Container Service - övervakaren Kubernetes med Microsoft Operations Management Suite (OMS)"
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
ms.openlocfilehash: 54fa789453768529deaf25d7575e5b21d0e41882
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-a-kubernetes-cluster-with-operations-management-suite"></a><span data-ttu-id="01a21-104">Övervaka ett Kubernetes kluster med Operations Management Suite</span><span class="sxs-lookup"><span data-stu-id="01a21-104">Monitor a Kubernetes cluster with Operations Management Suite</span></span>

<span data-ttu-id="01a21-105">Övervaka dina Kubernetes klustret och behållare är viktigt, särskilt när du hanterar ett produktionskluster i skala och med flera appar.</span><span class="sxs-lookup"><span data-stu-id="01a21-105">Monitoring your Kubernetes cluster and containers is critical, especially when you manage a production cluster at scale with multiple apps.</span></span> 

<span data-ttu-id="01a21-106">Du kan dra nytta av flera Kubernetes övervakningslösningar, antingen från Microsoft eller andra leverantörer.</span><span class="sxs-lookup"><span data-stu-id="01a21-106">You can take advantage of several Kubernetes monitoring solutions, either from Microsoft or other providers.</span></span> <span data-ttu-id="01a21-107">I kursen får du övervaka Kubernetes klustret med hjälp av hello behållare lösning i [Operations Management Suite](../../operations-management-suite/operations-management-suite-overview.md), Microsofts molnbaserade IT hanteringslösning.</span><span class="sxs-lookup"><span data-stu-id="01a21-107">In this tutorial, you monitor your Kubernetes cluster by using hello Containers solution in [Operations Management Suite](../../operations-management-suite/operations-management-suite-overview.md), Microsoft's cloud-based IT management solution.</span></span> <span data-ttu-id="01a21-108">(hello OMS behållare lösningen är i förhandsvisning.)</span><span class="sxs-lookup"><span data-stu-id="01a21-108">(hello OMS Containers solution is in preview.)</span></span>

<span data-ttu-id="01a21-109">Den här kursen, tillhör sju sju, ingår hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="01a21-109">This tutorial, part seven of seven, covers hello following tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="01a21-110">Hämta OMS-arbetsytan inställningar</span><span class="sxs-lookup"><span data-stu-id="01a21-110">Get OMS Workspace settings</span></span>
> * <span data-ttu-id="01a21-111">Ställ in OMS-agenter på hello Kubernetes noder</span><span class="sxs-lookup"><span data-stu-id="01a21-111">Set up OMS agents on hello Kubernetes nodes</span></span>
> * <span data-ttu-id="01a21-112">Åtkomst till övervakningsinformation i hello OMS-portalen eller Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="01a21-112">Access monitoring information in hello OMS portal or Azure portal</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="01a21-113">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="01a21-113">Before you begin</span></span>

<span data-ttu-id="01a21-114">I tidigare självstudier, ett program som har paketerats till behållaren bilder, dessa avbildningar överfört tooAzure behållare registret och ett Kubernetes kluster skapas.</span><span class="sxs-lookup"><span data-stu-id="01a21-114">In previous tutorials, an application was packaged into container images, these images uploaded tooAzure Container Registry, and a Kubernetes cluster created.</span></span> <span data-ttu-id="01a21-115">Om du inte har gjort dessa steg och vill toofollow längs, returnerar för[kursen 1 – skapa behållaren bilder](./container-service-tutorial-kubernetes-prepare-app.md).</span><span class="sxs-lookup"><span data-stu-id="01a21-115">If you have not done these steps, and would like toofollow along, return too[Tutorial 1 – Create container images](./container-service-tutorial-kubernetes-prepare-app.md).</span></span> 

<span data-ttu-id="01a21-116">Kursen krävs åtminstone ett Kubernetes kluster med Linux-agenten noder och ett konto med Operations Management Suite (OMS).</span><span class="sxs-lookup"><span data-stu-id="01a21-116">At minimum, this tutorial requires a Kubernetes cluster with Linux agent nodes, and an Operations Management Suite (OMS) account.</span></span> <span data-ttu-id="01a21-117">Om det behövs kan du registrera dig för en [kostnadsfri utvärderingsversion OMS](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial).</span><span class="sxs-lookup"><span data-stu-id="01a21-117">If needed, sign up for a [free OMS trial](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial).</span></span>

## <a name="get-workspace-settings"></a><span data-ttu-id="01a21-118">Hämta arbetsytan inställningar</span><span class="sxs-lookup"><span data-stu-id="01a21-118">Get Workspace settings</span></span>

<span data-ttu-id="01a21-119">När du har åtkomst till hello [OMS-portalen](https://mms.microsoft.com), gå för**inställningar** > **anslutna källor** > **Linux-servrar**.</span><span class="sxs-lookup"><span data-stu-id="01a21-119">When you can access hello [OMS portal](https://mms.microsoft.com), go too**Settings** > **Connected Sources** > **Linux Servers**.</span></span> <span data-ttu-id="01a21-120">Där du kan hitta hello *arbetsyte-ID* och en primär eller sekundär *Arbetsytenyckel*.</span><span class="sxs-lookup"><span data-stu-id="01a21-120">There, you can find hello *Workspace ID* and a primary or secondary *Workspace Key*.</span></span> <span data-ttu-id="01a21-121">Anteckna dessa värden som du behöver tooset upp OMS-agenterna på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="01a21-121">Take note of these values, which you need tooset up OMS agents on hello cluster.</span></span>

## <a name="set-up-oms-agents"></a><span data-ttu-id="01a21-122">Ställ in OMS-Agent</span><span class="sxs-lookup"><span data-stu-id="01a21-122">Set up OMS agents</span></span>

<span data-ttu-id="01a21-123">Här är en YAML filen tooset in OMS-agenterna på noderna hello Linux.</span><span class="sxs-lookup"><span data-stu-id="01a21-123">Here is a YAML file tooset up OMS agents on hello Linux cluster nodes.</span></span> <span data-ttu-id="01a21-124">Den skapar en Kubernetes [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/), som körs en enda identiska baljor på varje nod i klustret.</span><span class="sxs-lookup"><span data-stu-id="01a21-124">It creates a Kubernetes [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/), which runs a single identical pod on each cluster node.</span></span> <span data-ttu-id="01a21-125">Hej DaemonSet resursen är idealiskt för distribution av en övervakningsagent.</span><span class="sxs-lookup"><span data-stu-id="01a21-125">hello DaemonSet resource is ideal for deploying a monitoring agent.</span></span> 

<span data-ttu-id="01a21-126">Spara hello följande tooa textfil med namnet `oms-daemonset.yaml`, och Ersätt hello platshållarvärdena för *myWorkspaceID* och *myWorkspaceKey* med din OMS arbetsyte-ID och nyckel.</span><span class="sxs-lookup"><span data-stu-id="01a21-126">Save hello following text tooa file named `oms-daemonset.yaml`, and replace hello placeholder values for *myWorkspaceID* and *myWorkspaceKey* with your OMS Workspace ID and Key.</span></span> <span data-ttu-id="01a21-127">(För i produktion, kan du koda värdena som hemligheter.)</span><span class="sxs-lookup"><span data-stu-id="01a21-127">(In production, you can encode these values as secrets.)</span></span>

```YAML
apiVersion: extensions/v1beta1
kind: DaemonSet
metadata:
 name: omsagent
spec:
 template:
  metadata:
   labels:
    app: omsagent
    agentVersion: v1.3.4-127
    dockerProviderVersion: 10.0.0-25
  spec:
   containers:
     - name: omsagent 
       image: "microsoft/oms"
       imagePullPolicy: Always
       env:
       - name: WSID
         value: myWorkspaceID
       - name: KEY 
         value: myWorkspaceKey
       - name: DOMAIN
         value: opinsights.azure.com
       securityContext:
         privileged: true
       ports:
       - containerPort: 25225
         protocol: TCP 
       - containerPort: 25224
         protocol: UDP
       volumeMounts:
        - mountPath: /var/run/docker.sock
          name: docker-sock
        - mountPath: /var/log 
          name: host-log
       livenessProbe:
        exec:
         command:
         - /bin/bash
         - -c
         - ps -ef | grep omsagent | grep -v "grep"
        initialDelaySeconds: 60
        periodSeconds: 60
   volumes:
    - name: docker-sock 
      hostPath:
       path: /var/run/docker.sock
    - name: host-log
      hostPath:
       path: /var/log
```

<span data-ttu-id="01a21-128">Skapa hello DaemonSet med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="01a21-128">Create hello DaemonSet with hello following command:</span></span>

```azurecli-interactive
kubectl create -f oms-daemonset.yaml
```

<span data-ttu-id="01a21-129">toosee som hello DaemonSet skapas, kör:</span><span class="sxs-lookup"><span data-stu-id="01a21-129">toosee that hello DaemonSet is created, run:</span></span>

```azurecli-interactive
kubectl get daemonset
```

<span data-ttu-id="01a21-130">Utdata är liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="01a21-130">Output is similar toohello following:</span></span>

```azurecli-interactive
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE-SELECTOR   AGE
omsagent   3         3         3         0            3           <none>          5m
```

<span data-ttu-id="01a21-131">När hello agenter körs tar flera minuter för OMS tooingest och processen hello-data.</span><span class="sxs-lookup"><span data-stu-id="01a21-131">After hello agents are running, it takes several minutes for OMS tooingest and process hello data.</span></span>

## <a name="access-monitoring-data"></a><span data-ttu-id="01a21-132">Åtkomst till övervakningsdata</span><span class="sxs-lookup"><span data-stu-id="01a21-132">Access monitoring data</span></span>

<span data-ttu-id="01a21-133">Visa och analysera hello OMS behållaren övervakningsdata med hello [behållare lösning](../../log-analytics/log-analytics-containers.md) i hello OMS-portalen eller hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="01a21-133">View and analyze hello OMS container monitoring data with hello [Container solution](../../log-analytics/log-analytics-containers.md) in either hello OMS portal or hello Azure portal.</span></span> 

<span data-ttu-id="01a21-134">tooinstall hello behållaren lösning med hjälp av hello [OMS-portalen](https://mms.microsoft.com), gå för**lösningar galleriet**.</span><span class="sxs-lookup"><span data-stu-id="01a21-134">tooinstall hello Container solution using hello [OMS portal](https://mms.microsoft.com), go too**Solutions Gallery**.</span></span> <span data-ttu-id="01a21-135">Lägg sedan till **behållare lösning**.</span><span class="sxs-lookup"><span data-stu-id="01a21-135">Then add **Container Solution**.</span></span> <span data-ttu-id="01a21-136">Du kan också lägga till hello behållare lösningen från hello [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft.containersoms?tab=Overview).</span><span class="sxs-lookup"><span data-stu-id="01a21-136">Alternatively, add hello Containers solution from hello [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft.containersoms?tab=Overview).</span></span>

<span data-ttu-id="01a21-137">I hello OMS-portalen, söka efter en **behållare** panelen Sammanfattning på hello OMS-instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="01a21-137">In hello OMS portal, look for a **Containers** summary tile on hello OMS dashboard.</span></span> <span data-ttu-id="01a21-138">Klicka på panelen hello för information, inklusive: behållaren händelser, fel, status, bild inventering och processor- och minnesanvändning.</span><span class="sxs-lookup"><span data-stu-id="01a21-138">Click hello tile for details including: container events, errors, status, image inventory, and CPU and memory usage.</span></span> <span data-ttu-id="01a21-139">Mer detaljerad information finns på en rad på panelen eller utföra en [loggen Sök](../../log-analytics/log-analytics-log-searches.md).</span><span class="sxs-lookup"><span data-stu-id="01a21-139">For more granular information, click a row on any tile, or perform a [log search](../../log-analytics/log-analytics-log-searches.md).</span></span>

![Instrumentpanel för behållare i OMS-portalen](./media/container-service-tutorial-kubernetes-monitor/oms-containers-dashboard.png)

<span data-ttu-id="01a21-141">På samma sätt i hello Azure-portalen, går för**logganalys** och välj namnet på din arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="01a21-141">Similarly, in hello Azure portal, go too**Log Analytics** and select your workspace name.</span></span> <span data-ttu-id="01a21-142">toosee hello **behållare** panelen Sammanfattning, klickar du på **lösningar** > **behållare**.</span><span class="sxs-lookup"><span data-stu-id="01a21-142">toosee hello **Containers** summary tile, click **Solutions** > **Containers**.</span></span> <span data-ttu-id="01a21-143">toosee information Klicka hello-panelen.</span><span class="sxs-lookup"><span data-stu-id="01a21-143">toosee details, click hello tile.</span></span>

<span data-ttu-id="01a21-144">Se hello [Azure logganalys dokumentationen](../../log-analytics/index.md) för detaljerad information om frågor och analys av övervakningsdata.</span><span class="sxs-lookup"><span data-stu-id="01a21-144">See hello [Azure Log Analytics documentation](../../log-analytics/index.md) for detailed guidance on querying and analyzing monitoring data.</span></span>

## <a name="next-steps"></a><span data-ttu-id="01a21-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="01a21-145">Next steps</span></span>

<span data-ttu-id="01a21-146">I den här självstudiekursen övervakade Kubernetes klustret med OMS.</span><span class="sxs-lookup"><span data-stu-id="01a21-146">In this tutorial, you monitored your Kubernetes cluster with OMS.</span></span> <span data-ttu-id="01a21-147">Uppgifter som omfattas ingår:</span><span class="sxs-lookup"><span data-stu-id="01a21-147">Tasks covered included:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="01a21-148">Hämta OMS-arbetsytan inställningar</span><span class="sxs-lookup"><span data-stu-id="01a21-148">Get OMS Workspace settings</span></span>
> * <span data-ttu-id="01a21-149">Ställ in OMS-agenter på hello Kubernetes noder</span><span class="sxs-lookup"><span data-stu-id="01a21-149">Set up OMS agents on hello Kubernetes nodes</span></span>
> * <span data-ttu-id="01a21-150">Åtkomst till övervakningsinformation i hello OMS-portalen eller Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="01a21-150">Access monitoring information in hello OMS portal or Azure portal</span></span>


<span data-ttu-id="01a21-151">Följ den här länken toosee inbyggd skriptexempel för Container Service.</span><span class="sxs-lookup"><span data-stu-id="01a21-151">Follow this link toosee pre-built script samples for Container Service.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="01a21-152">Azure Container Service-skriptexempel</span><span class="sxs-lookup"><span data-stu-id="01a21-152">Azure Container Service script samples</span></span>](cli-samples.md)
