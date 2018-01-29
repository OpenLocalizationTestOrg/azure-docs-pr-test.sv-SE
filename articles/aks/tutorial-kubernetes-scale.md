---
title: "Kubernetes på Azure tutorial – skala program"
description: "AKS tutorial – skala program"
services: container-service
author: dlepow
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 11/15/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: ff8cf813f9c932f867413dbf7e76f949e0de2f26
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/11/2017
---
# <a name="scale-application-in-azure-container-service-aks"></a>Skala program i Azure Container Service (AKS)

Om du har följande självstudierna du ha en fungerande Kubernetes klustret i AKS och du har distribuerat appen Azure röstning.

I den här kursen ingår fem åtta skala ut skida i appen och försök baljor autoskalning. Du också lära dig hur du skalar antalet Virtuella Azure-noder att ändra klustrets kapacitet för värd för arbetsbelastningar. Aktiviteter har slutförts är:

> [!div class="checklist"]
> * Skala Kubernetes Azure-noder
> * Skalning Kubernetes skida manuellt
> * Konfigurera Autoskala skida kör app klientdelen

Programmet Azure rösten uppdateras i efterföljande självstudiekurser och Operations Management Suite som konfigurerats för att övervaka Kubernetes klustret.

## <a name="before-you-begin"></a>Innan du börjar

I föregående självstudier, ett program som har paketerats i en behållare avbildning, avbildningen har överförts till registret för Azure-behållaren och ett Kubernetes kluster skapas. Programmet körs sedan Kubernetes klustret.

Om du inte har gjort dessa steg och vill följa med, gå tillbaka till den [kursen 1 – skapa behållaren bilder][aks-tutorial-prepare-app].

## <a name="scale-aks-nodes"></a>Skala AKS noder

Om du har skapat klustret Kubernetes med kommandona i föregående kursen har en nod. Du kan justera antalet noder manuellt om du planerar fler eller färre arbetsbelastningar i behållare på ditt kluster.

I följande exempel ökar antalet noder till tre i Kubernetes klustret med namnet *myK8sCluster*. Kommandot tar några minuter att slutföra.

```azurecli
az aks scale --resource-group=myResourceGroup --name=myK8SCluster --node-count 3
```

Utdatan liknar följande:

```
"agentPoolProfiles": [
  {
    "count": 3,
    "dnsPrefix": null,
    "fqdn": null,
    "name": "myK8sCluster",
    "osDiskSizeGb": null,
    "osType": "Linux",
    "ports": null,
    "storageProfile": "ManagedDisks",
    "vmSize": "Standard_D2_v2",
    "vnetSubnetId": null
  }
```

## <a name="manually-scale-pods"></a>Skala skida manuellt

Därmed distribuerats långt Azure rösten frontend och Redis-instans har, var och en med en enskild replik. Du kan kontrollera genom att köra den [kubectl hämta] [ kubectl-get] kommando.

```azurecli
kubectl get pods
```

Resultat:

```
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2549686872-4d2r5   1/1       Running   0          31m
azure-vote-front-848767080-tf34m   1/1       Running   0          31m
```

Manuellt ändra antalet skida i den `azure-vote-front` distribution med den [kubectl skala] [ kubectl-scale] kommando. Det här exemplet ökar antalet till 5.

```azurecli
kubectl scale --replicas=5 deployment/azure-vote-front
```

Kör [kubectl hämta skida] [ kubectl-get] att verifiera att Kubernetes skapar skida. När en minut eller så kan kör ytterligare skida:

```azurecli
kubectl get pods
```

Resultat:

```
NAME                                READY     STATUS    RESTARTS   AGE
azure-vote-back-2606967446-nmpcf    1/1       Running   0          15m
azure-vote-front-3309479140-2hfh0   1/1       Running   0          3m
azure-vote-front-3309479140-bzt05   1/1       Running   0          3m
azure-vote-front-3309479140-fvcvm   1/1       Running   0          3m
azure-vote-front-3309479140-hrbf2   1/1       Running   0          15m
azure-vote-front-3309479140-qphz8   1/1       Running   0          3m
```

## <a name="autoscale-pods"></a>Autoskala skida

Har stöd för Kubernetes [vågräta baljor autoskalning] [ kubernetes-hpa] för att justera antalet skida i en distribution beroende på CPU-användning eller annan välja mått.

Om du vill använda autoscaler ha din skida CPU-begäranden och gränser har definierats. I den `azure-vote-front` distribution, behållaren frontend begäranden 0,25 processor, med högst 0,5 CPU. Det ser ut som inställningarna:

```YAML
resources:
  requests:
     cpu: 250m
  limits:
     cpu: 500m
```

I följande exempel används den [kubectl Autoskala] [ kubectl-autoscale] kommandot Autoskala antalet skida i den `azure-vote-front` distribution. Om processoranvändningen överskrider 50%, ökar autoscaler här skida högst 10.


```azurecli
kubectl autoscale deployment azure-vote-front --cpu-percent=50 --min=3 --max=10
```

Om du vill se status för autoscaler, kör du följande kommando:

```azurecli
kubectl get hpa
```

Resultat:

```
NAME               REFERENCE                     TARGETS    MINPODS   MAXPODS   REPLICAS   AGE
azure-vote-front   Deployment/azure-vote-front   0% / 50%   3         10        3          2m
```

Efter några minuter med minimal belastning på appen Azure röst minskar antalet baljor replikerna automatiskt till 3.

## <a name="next-steps"></a>Nästa steg

I den här kursen används olika skalning funktioner i Kubernetes klustret. Uppgifter som omfattas ingår:

> [!div class="checklist"]
> * Skalning Kubernetes skida manuellt
> * Konfigurera Autoskala skida kör app klientdelen
> * Skala Kubernetes Azure-noder

Gå vidare till nästa kurs att lära dig om att uppdatera programmet i Kubernetes.

> [!div class="nextstepaction"]
> [Uppdatera ett program i Kubernetes][aks-tutorial-update-app]

<!-- LINKS - external -->
[kubectl-autoscale]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#autoscale
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-scale]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#scale
[kubernetes-hpa]: https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[aks-tutorial-update-app]: ./tutorial-kubernetes-app-update.md
