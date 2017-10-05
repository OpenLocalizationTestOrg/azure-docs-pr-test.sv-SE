---
title: "Självstudiekurs för Azure Container Service - övervakaren Kubernetes | Microsoft Docs"
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
ms.openlocfilehash: f4d09973ada8e3cd0ff2b00d20aca979e834cd7f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-a-kubernetes-cluster-with-operations-management-suite"></a><span data-ttu-id="91534-104">Övervaka ett Kubernetes kluster med Operations Management Suite</span><span class="sxs-lookup"><span data-stu-id="91534-104">Monitor a Kubernetes cluster with Operations Management Suite</span></span>

<span data-ttu-id="91534-105">Övervaka dina Kubernetes klustret och behållare är viktigt, särskilt när du hanterar ett produktionskluster i skala och med flera appar.</span><span class="sxs-lookup"><span data-stu-id="91534-105">Monitoring your Kubernetes cluster and containers is critical, especially when you manage a production cluster at scale with multiple apps.</span></span> 

<span data-ttu-id="91534-106">Du kan dra nytta av flera Kubernetes övervakningslösningar, antingen från Microsoft eller andra leverantörer.</span><span class="sxs-lookup"><span data-stu-id="91534-106">You can take advantage of several Kubernetes monitoring solutions, either from Microsoft or other providers.</span></span> <span data-ttu-id="91534-107">I kursen får du övervaka Kubernetes klustret med hjälp av behållare lösningen i [Operations Management Suite](../../operations-management-suite/operations-management-suite-overview.md), Microsofts molnbaserade IT hanteringslösning.</span><span class="sxs-lookup"><span data-stu-id="91534-107">In this tutorial, you monitor your Kubernetes cluster by using the Containers solution in [Operations Management Suite](../../operations-management-suite/operations-management-suite-overview.md), Microsoft's cloud-based IT management solution.</span></span> <span data-ttu-id="91534-108">(OMS behållare lösningen är i förhandsvisning.)</span><span class="sxs-lookup"><span data-stu-id="91534-108">(The OMS Containers solution is in preview.)</span></span>

<span data-ttu-id="91534-109">Den här självstudien tillhör sju sju, omfattar följande aktiviteter:</span><span class="sxs-lookup"><span data-stu-id="91534-109">This tutorial, part seven of seven, covers the following tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="91534-110">Hämta OMS-arbetsytan inställningar</span><span class="sxs-lookup"><span data-stu-id="91534-110">Get OMS Workspace settings</span></span>
> * <span data-ttu-id="91534-111">Ställ in OMS-agenterna på noderna Kubernetes</span><span class="sxs-lookup"><span data-stu-id="91534-111">Set up OMS agents on the Kubernetes nodes</span></span>
> * <span data-ttu-id="91534-112">Åtkomst till övervakningsinformation i OMS-portalen eller Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="91534-112">Access monitoring information in the OMS portal or Azure portal</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="91534-113">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="91534-113">Before you begin</span></span>

<span data-ttu-id="91534-114">I föregående självstudiekurser ett program som har paketerats i behållaren bilder, dessa bilder som har överförts till registret för Azure-behållaren och ett Kubernetes kluster skapas.</span><span class="sxs-lookup"><span data-stu-id="91534-114">In previous tutorials, an application was packaged into container images, these images uploaded to Azure Container Registry, and a Kubernetes cluster created.</span></span> <span data-ttu-id="91534-115">Om du inte har gjort dessa steg och vill följa med, gå tillbaka till [kursen 1 – skapa behållaren bilder](./container-service-tutorial-kubernetes-prepare-app.md).</span><span class="sxs-lookup"><span data-stu-id="91534-115">If you have not done these steps, and would like to follow along, return to [Tutorial 1 – Create container images](./container-service-tutorial-kubernetes-prepare-app.md).</span></span> 

<span data-ttu-id="91534-116">Kursen krävs åtminstone ett Kubernetes kluster med Linux-agenten noder och ett konto med Operations Management Suite (OMS).</span><span class="sxs-lookup"><span data-stu-id="91534-116">At minimum, this tutorial requires a Kubernetes cluster with Linux agent nodes, and an Operations Management Suite (OMS) account.</span></span> <span data-ttu-id="91534-117">Om det behövs kan du registrera dig för en [kostnadsfri utvärderingsversion OMS](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial).</span><span class="sxs-lookup"><span data-stu-id="91534-117">If needed, sign up for a [free OMS trial](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial).</span></span>

## <a name="get-workspace-settings"></a><span data-ttu-id="91534-118">Hämta arbetsytan inställningar</span><span class="sxs-lookup"><span data-stu-id="91534-118">Get Workspace settings</span></span>

<span data-ttu-id="91534-119">När du har åtkomst till den [OMS-portalen](https://mms.microsoft.com), gå till **inställningar** > **anslutna källor** > **Linux-servrar**.</span><span class="sxs-lookup"><span data-stu-id="91534-119">When you can access the [OMS portal](https://mms.microsoft.com), go to **Settings** > **Connected Sources** > **Linux Servers**.</span></span> <span data-ttu-id="91534-120">Där hittar du den *arbetsyte-ID* och en primär eller sekundär *Arbetsytenyckel*.</span><span class="sxs-lookup"><span data-stu-id="91534-120">There, you can find the *Workspace ID* and a primary or secondary *Workspace Key*.</span></span> <span data-ttu-id="91534-121">Anteckna dessa värden som du måste ställa in OMS-agenterna på klustret.</span><span class="sxs-lookup"><span data-stu-id="91534-121">Take note of these values, which you need to set up OMS agents on the cluster.</span></span>

## <a name="set-up-oms-agents"></a><span data-ttu-id="91534-122">Ställ in OMS-Agent</span><span class="sxs-lookup"><span data-stu-id="91534-122">Set up OMS agents</span></span>

<span data-ttu-id="91534-123">Här är en YAML-fil för att ställa in OMS-agenterna på noderna i Linux.</span><span class="sxs-lookup"><span data-stu-id="91534-123">Here is a YAML file to set up OMS agents on the Linux cluster nodes.</span></span> <span data-ttu-id="91534-124">Den skapar en Kubernetes [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/), som körs en enda identiska baljor på varje nod i klustret.</span><span class="sxs-lookup"><span data-stu-id="91534-124">It creates a Kubernetes [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/), which runs a single identical pod on each cluster node.</span></span> <span data-ttu-id="91534-125">Resursen DaemonSet är idealiskt för distribution av en övervakningsagent.</span><span class="sxs-lookup"><span data-stu-id="91534-125">The DaemonSet resource is ideal for deploying a monitoring agent.</span></span> 

<span data-ttu-id="91534-126">Spara följande text i en fil med namnet `oms-daemonset.yaml`, och ersätter platshållarvärdena för *myWorkspaceID* och *myWorkspaceKey* med din OMS arbetsyte-ID och nyckel.</span><span class="sxs-lookup"><span data-stu-id="91534-126">Save the following text to a file named `oms-daemonset.yaml`, and replace the placeholder values for *myWorkspaceID* and *myWorkspaceKey* with your OMS Workspace ID and Key.</span></span> <span data-ttu-id="91534-127">(För i produktion, kan du koda värdena som hemligheter.)</span><span class="sxs-lookup"><span data-stu-id="91534-127">(In production, you can encode these values as secrets.)</span></span>

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

<span data-ttu-id="91534-128">Skapa DaemonSet med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="91534-128">Create the DaemonSet with the following command:</span></span>

```azurecli-interactive
kubectl create -f oms-daemonset.yaml
```

<span data-ttu-id="91534-129">För att se att DaemonSet har skapats, kör du:</span><span class="sxs-lookup"><span data-stu-id="91534-129">To see that the DaemonSet is created, run:</span></span>

```azurecli-interactive
kubectl get daemonset
```

<span data-ttu-id="91534-130">De utdata som genereras liknar följande:</span><span class="sxs-lookup"><span data-stu-id="91534-130">Output is similar to the following:</span></span>

```azurecli-interactive
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE-SELECTOR   AGE
omsagent   3         3         3         0            3           <none>          5m
```

<span data-ttu-id="91534-131">När agenterna körs tar flera minuter för OMS att mata in och bearbeta data.</span><span class="sxs-lookup"><span data-stu-id="91534-131">After the agents are running, it takes several minutes for OMS to ingest and process the data.</span></span>

## <a name="access-monitoring-data"></a><span data-ttu-id="91534-132">Åtkomst till övervakningsdata</span><span class="sxs-lookup"><span data-stu-id="91534-132">Access monitoring data</span></span>

<span data-ttu-id="91534-133">Visa och analysera OMS-behållaren övervakningsdata med den [behållare lösning](../../log-analytics/log-analytics-containers.md) i OMS-portalen eller Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="91534-133">View and analyze the OMS container monitoring data with the [Container solution](../../log-analytics/log-analytics-containers.md) in either the OMS portal or the Azure portal.</span></span> 

<span data-ttu-id="91534-134">Att installera lösningen behållaren med den [OMS-portalen](https://mms.microsoft.com), gå till **lösningar galleriet**.</span><span class="sxs-lookup"><span data-stu-id="91534-134">To install the Container solution using the [OMS portal](https://mms.microsoft.com), go to **Solutions Gallery**.</span></span> <span data-ttu-id="91534-135">Lägg sedan till **behållare lösning**.</span><span class="sxs-lookup"><span data-stu-id="91534-135">Then add **Container Solution**.</span></span> <span data-ttu-id="91534-136">Du kan också lägga till behållare lösningar från den [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft.containersoms?tab=Overview).</span><span class="sxs-lookup"><span data-stu-id="91534-136">Alternatively, add the Containers solution from the [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft.containersoms?tab=Overview).</span></span>

<span data-ttu-id="91534-137">I OMS-portalen, söka efter en **behållare** panelen Sammanfattning på OMS-instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="91534-137">In the OMS portal, look for a **Containers** summary tile on the OMS dashboard.</span></span> <span data-ttu-id="91534-138">Klicka på panelen för information, inklusive: behållaren händelser, fel, status, bild inventering och processor- och minnesanvändning.</span><span class="sxs-lookup"><span data-stu-id="91534-138">Click the tile for details including: container events, errors, status, image inventory, and CPU and memory usage.</span></span> <span data-ttu-id="91534-139">Mer detaljerad information finns på en rad på panelen eller utföra en [loggen Sök](../../log-analytics/log-analytics-log-searches.md).</span><span class="sxs-lookup"><span data-stu-id="91534-139">For more granular information, click a row on any tile, or perform a [log search](../../log-analytics/log-analytics-log-searches.md).</span></span>

![Instrumentpanel för behållare i OMS-portalen](./media/container-service-tutorial-kubernetes-monitor/oms-containers-dashboard.png)

<span data-ttu-id="91534-141">På samma sätt i Azure-portalen går du till **logganalys** och välj namnet på din arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="91534-141">Similarly, in the Azure portal, go to **Log Analytics** and select your workspace name.</span></span> <span data-ttu-id="91534-142">Se den **behållare** panelen Sammanfattning, klickar du på **lösningar** > **behållare**.</span><span class="sxs-lookup"><span data-stu-id="91534-142">To see the **Containers** summary tile, click **Solutions** > **Containers**.</span></span> <span data-ttu-id="91534-143">Klicka på panelen om du vill se information.</span><span class="sxs-lookup"><span data-stu-id="91534-143">To see details, click the tile.</span></span>

<span data-ttu-id="91534-144">Finns det [Azure logganalys dokumentationen](../../log-analytics/index.md) för detaljerad information om frågor och analys av övervakningsdata.</span><span class="sxs-lookup"><span data-stu-id="91534-144">See the [Azure Log Analytics documentation](../../log-analytics/index.md) for detailed guidance on querying and analyzing monitoring data.</span></span>

## <a name="next-steps"></a><span data-ttu-id="91534-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="91534-145">Next steps</span></span>

<span data-ttu-id="91534-146">I den här självstudiekursen övervakade Kubernetes klustret med OMS.</span><span class="sxs-lookup"><span data-stu-id="91534-146">In this tutorial, you monitored your Kubernetes cluster with OMS.</span></span> <span data-ttu-id="91534-147">Uppgifter som omfattas ingår:</span><span class="sxs-lookup"><span data-stu-id="91534-147">Tasks covered included:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="91534-148">Hämta OMS-arbetsytan inställningar</span><span class="sxs-lookup"><span data-stu-id="91534-148">Get OMS Workspace settings</span></span>
> * <span data-ttu-id="91534-149">Ställ in OMS-agenterna på noderna Kubernetes</span><span class="sxs-lookup"><span data-stu-id="91534-149">Set up OMS agents on the Kubernetes nodes</span></span>
> * <span data-ttu-id="91534-150">Åtkomst till övervakningsinformation i OMS-portalen eller Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="91534-150">Access monitoring information in the OMS portal or Azure portal</span></span>


<span data-ttu-id="91534-151">Den här länken för att se förskapad skriptexempel Container Service.</span><span class="sxs-lookup"><span data-stu-id="91534-151">Follow this link to see pre-built script samples for Container Service.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="91534-152">Azure Container Service-skriptexempel</span><span class="sxs-lookup"><span data-stu-id="91534-152">Azure Container Service script samples</span></span>](cli-samples.md)
