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
# <a name="scale-kubernetes-pods-and-kubernetes-infrastructure"></a>Skala Kubernetes skida och Kubernetes infrastruktur

Om du har följande hello självstudiekurser du ha en fungerande Kubernetes kluster i Azure Container Service och du har distribuerat hello Azure röstning app. 

I den här självstudiekursen tillhör fem sju, skala ut hello skida i hello app och försök baljor autoskalning. Du lär dig också hur tooscale hello antal Virtuella Azure-agenten noder toochange hello klustrets kapacitet för värd för arbetsbelastningar. Aktiviteter har slutförts är:

> [!div class="checklist"]
> * Skalning Kubernetes skida manuellt
> * Konfigurera Autoskala skida kör hello app klientdel
> * Skala hello Kubernetes Azure-agenten noder

Hello Azure rösten programmet uppdateras i efterföljande självstudiekurser och Operations Management Suite konfigurerad toomonitor hello Kubernetes klustret.

## <a name="before-you-begin"></a>Innan du börjar

I tidigare självstudier, ett program som har paketerats till en behållare bild, den här avbildningen överfört tooAzure behållare registret och ett Kubernetes kluster skapas. hello programmet körs sedan hello Kubernetes klustret. Om du inte har gjort dessa steg och vill toofollow längs, returnerar toohello [kursen 1 – skapa behållaren bilder](./container-service-tutorial-kubernetes-prepare-app.md). 

Kursen krävs åtminstone ett Kubernetes kluster med ett program som körs.

## <a name="manually-scale-pods"></a>Skala skida manuellt

Hittills, hello Azure rösten frontend och Redis-instansen har distribuerats, var och en med en enskild replik. tooverify, kör hello [kubectl hämta](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) kommando.

```azurecli-interactive
kubectl get pods
```

Resultat:

```bash
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2549686872-4d2r5   1/1       Running   0          31m
azure-vote-front-848767080-tf34m   1/1       Running   0          31m
```

Manuellt ändra hello antal skida i hello `azure-vote-front` distribution med hello [kubectl skala](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#scale) kommando. Det här exemplet ökar hello nummer too5.

```azurecli-interactive
kubectl scale --replicas=5 deployment/azure-vote-front
```

Kör [kubectl hämta skida](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) tooverify att Kubernetes skapar hello skida. Efter en minut eller så körs hello ytterligare skida:

```azurecli-interactive
kubectl get pods
```

Resultat:

```bash
NAME                                READY     STATUS    RESTARTS   AGE
azure-vote-back-2606967446-nmpcf    1/1       Running   0          15m
azure-vote-front-3309479140-2hfh0   1/1       Running   0          3m
azure-vote-front-3309479140-bzt05   1/1       Running   0          3m
azure-vote-front-3309479140-fvcvm   1/1       Running   0          3m
azure-vote-front-3309479140-hrbf2   1/1       Running   0          15m
azure-vote-front-3309479140-qphz8   1/1       Running   0          3m
```

## <a name="autoscale-pods"></a>Autoskala skida

Har stöd för Kubernetes [vågräta baljor autoskalning](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) tooadjust hello antalet skida i en distribution beroende på CPU-användning eller annan välja mått. 

toouse hello autoscaler din skida måste ha CPU-begäranden och gränser har definierats. I hello `azure-vote-front` distribution hello frontend behållaren begäranden 0,25 processor, med högst 0,5 CPU. Det ser ut som hello inställningar:

```YAML
resources:
  requests:
     cpu: 250m
  limits:
     cpu: 500m
```

hello följande exempel används hello [kubectl Autoskala](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#autoscale) kommandot tooautoscale hello antalet skida i hello `azure-vote-front` distribution. Om processoranvändningen överskrider 50%, ökar hello autoscaler här hello skida tooa högst 10.


```azurecli-interactive
kubectl autoscale deployment azure-vote-front --cpu-percent=50 --min=3 --max=10
```

toosee hello status för hello autoscaler, kör hello följande kommando:

```azurecli-interactive
kubectl get hpa
```

Resultat:

```bash
NAME               REFERENCE                     TARGETS    MINPODS   MAXPODS   REPLICAS   AGE
azure-vote-front   Deployment/azure-vote-front   0% / 50%   3         10        3          2m
```

Efter några minuter med minimal belastning på hello Azure rösten app minskar hello antal baljor repliker automatiskt too3.

## <a name="scale-hello-agents"></a>Skala hello agenter

Om du har skapat klustret Kubernetes standard kommandona i hello tidigare kursen har tre agent-noder. Du kan justera hello antalet agenter manuellt om du planerar fler eller färre arbetsbelastningar i behållare på ditt kluster. Använd hello [az acs skala](/cli/azure/acs#scale) kommando och ange hello antalet agenter med hello `--new-agent-count` parameter.

hello följande exempel ökar hello antalet agent noder too4 i hello Kubernetes kluster med namnet *myK8sCluster*. hello kommandot tar några minuter toocomplete.

```azurecli-interactive
az acs scale --resource-group=myResourceGroup --name=myK8SCluster --new-agent-count 4
```

hello kommandoutdata visar hello antalet agent noder i hello värdet av `agentPoolProfiles:count`:

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

## <a name="next-steps"></a>Nästa steg

I den här kursen används olika skalning funktioner i Kubernetes klustret. Uppgifter som omfattas ingår:

> [!div class="checklist"]
> * Skalning Kubernetes skida manuellt
> * Konfigurera Autoskala skida kör hello app klientdel
> * Skala hello Kubernetes Azure-agenten noder

Nästa självstudiekurs toolearn toohello om att uppdatera programmet i Kubernetes i förväg.

> [!div class="nextstepaction"]
> [Uppdatera ett program i Kubernetes](./container-service-tutorial-kubernetes-app-update.md)

