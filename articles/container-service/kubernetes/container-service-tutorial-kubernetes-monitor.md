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
# <a name="monitor-a-kubernetes-cluster-with-operations-management-suite"></a>Övervaka ett Kubernetes kluster med Operations Management Suite

Övervaka dina Kubernetes klustret och behållare är viktigt, särskilt när du hanterar ett produktionskluster i skala och med flera appar. 

Du kan dra nytta av flera Kubernetes övervakningslösningar, antingen från Microsoft eller andra leverantörer. I kursen får du övervaka Kubernetes klustret med hjälp av hello behållare lösning i [Operations Management Suite](../../operations-management-suite/operations-management-suite-overview.md), Microsofts molnbaserade IT hanteringslösning. (hello OMS behållare lösningen är i förhandsvisning.)

Den här kursen, tillhör sju sju, ingår hello följande uppgifter:

> [!div class="checklist"]
> * Hämta OMS-arbetsytan inställningar
> * Ställ in OMS-agenter på hello Kubernetes noder
> * Åtkomst till övervakningsinformation i hello OMS-portalen eller Azure-portalen

## <a name="before-you-begin"></a>Innan du börjar

I tidigare självstudier, ett program som har paketerats till behållaren bilder, dessa avbildningar överfört tooAzure behållare registret och ett Kubernetes kluster skapas. Om du inte har gjort dessa steg och vill toofollow längs, returnerar för[kursen 1 – skapa behållaren bilder](./container-service-tutorial-kubernetes-prepare-app.md). 

Kursen krävs åtminstone ett Kubernetes kluster med Linux-agenten noder och ett konto med Operations Management Suite (OMS). Om det behövs kan du registrera dig för en [kostnadsfri utvärderingsversion OMS](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial).

## <a name="get-workspace-settings"></a>Hämta arbetsytan inställningar

När du har åtkomst till hello [OMS-portalen](https://mms.microsoft.com), gå för**inställningar** > **anslutna källor** > **Linux-servrar**. Där du kan hitta hello *arbetsyte-ID* och en primär eller sekundär *Arbetsytenyckel*. Anteckna dessa värden som du behöver tooset upp OMS-agenterna på hello klustret.

## <a name="set-up-oms-agents"></a>Ställ in OMS-Agent

Här är en YAML filen tooset in OMS-agenterna på noderna hello Linux. Den skapar en Kubernetes [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/), som körs en enda identiska baljor på varje nod i klustret. Hej DaemonSet resursen är idealiskt för distribution av en övervakningsagent. 

Spara hello följande tooa textfil med namnet `oms-daemonset.yaml`, och Ersätt hello platshållarvärdena för *myWorkspaceID* och *myWorkspaceKey* med din OMS arbetsyte-ID och nyckel. (För i produktion, kan du koda värdena som hemligheter.)

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

Skapa hello DaemonSet med hello följande kommando:

```azurecli-interactive
kubectl create -f oms-daemonset.yaml
```

toosee som hello DaemonSet skapas, kör:

```azurecli-interactive
kubectl get daemonset
```

Utdata är liknande toohello följande:

```azurecli-interactive
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE-SELECTOR   AGE
omsagent   3         3         3         0            3           <none>          5m
```

När hello agenter körs tar flera minuter för OMS tooingest och processen hello-data.

## <a name="access-monitoring-data"></a>Åtkomst till övervakningsdata

Visa och analysera hello OMS behållaren övervakningsdata med hello [behållare lösning](../../log-analytics/log-analytics-containers.md) i hello OMS-portalen eller hello Azure-portalen. 

tooinstall hello behållaren lösning med hjälp av hello [OMS-portalen](https://mms.microsoft.com), gå för**lösningar galleriet**. Lägg sedan till **behållare lösning**. Du kan också lägga till hello behållare lösningen från hello [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft.containersoms?tab=Overview).

I hello OMS-portalen, söka efter en **behållare** panelen Sammanfattning på hello OMS-instrumentpanelen. Klicka på panelen hello för information, inklusive: behållaren händelser, fel, status, bild inventering och processor- och minnesanvändning. Mer detaljerad information finns på en rad på panelen eller utföra en [loggen Sök](../../log-analytics/log-analytics-log-searches.md).

![Instrumentpanel för behållare i OMS-portalen](./media/container-service-tutorial-kubernetes-monitor/oms-containers-dashboard.png)

På samma sätt i hello Azure-portalen, går för**logganalys** och välj namnet på din arbetsyta. toosee hello **behållare** panelen Sammanfattning, klickar du på **lösningar** > **behållare**. toosee information Klicka hello-panelen.

Se hello [Azure logganalys dokumentationen](../../log-analytics/index.md) för detaljerad information om frågor och analys av övervakningsdata.

## <a name="next-steps"></a>Nästa steg

I den här självstudiekursen övervakade Kubernetes klustret med OMS. Uppgifter som omfattas ingår:

> [!div class="checklist"]
> * Hämta OMS-arbetsytan inställningar
> * Ställ in OMS-agenter på hello Kubernetes noder
> * Åtkomst till övervakningsinformation i hello OMS-portalen eller Azure-portalen


Följ den här länken toosee inbyggd skriptexempel för Container Service.

> [!div class="nextstepaction"]
> [Azure Container Service-skriptexempel](cli-samples.md)
