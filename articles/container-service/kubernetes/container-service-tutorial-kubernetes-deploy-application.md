---
title: "aaaAzure Container Service tutorial – distribuera programmet | Microsoft Docs"
description: "Självstudiekurs för Azure Container Service - distribuera program"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Docker, Containers, Micro-services, Kubernetes, DC/OS, Azure
ms.assetid: 
ms.service: container-service
ms.devlang: aurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 7e2fa06d359caf83e684df3966624a6e9a8e7efa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-applications-in-kubernetes"></a>Köra program i Kubernetes

I den här självstudiekursen del fyra av sju, ett exempelprogram har distribuerats i ett Kubernetes kluster. Slutfört stegen innefattar:

> [!div class="checklist"]
> * Hämta Kubernetes manifestfiler
> * Kör program i Kubernetes
> * Testa programmet hello

Det här programmet skalas ut, uppdateras i efterföljande självstudiekurser och Operations Management Suite konfigureras toomonitor hello Kubernetes klustret.

Den här kursen förutsätter grundläggande kunskaper om Kubernetes begrepp, detaljerad information om Kubernetes finns hello [Kubernetes dokumentationen](https://kubernetes.io/docs/home/).

## <a name="before-you-begin"></a>Innan du börjar

Ett program som har paketerats till en behållare bild i föregående självstudiekurser, avbildningen har överförda tooAzure behållare registret och en Kubernetes klustret har skapats. Om du inte har gjort dessa steg och vill toofollow längs, returnerar för[kursen 1 – skapa behållaren bilder](./container-service-tutorial-kubernetes-prepare-app.md). 

Den här kursen kräver åtminstone ett Kubernetes kluster.

## <a name="get-manifest-file"></a>Hämta manifestfilen

Den här självstudien [Kubernetes objekt](https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/) distribueras med ett Kubernetes manifest. Ett manifest för Kubernetes är en YAML eller JSON formaterad fil som innehåller Kubernetes objektet anvisningar för distribution och konfiguration.

Hej programmanifestfilen för den här kursen finns i hello Azure rösten programmet lagringsplatsen som klonades i en tidigare vägledning. Om du inte redan har gjort det klona lagringsplatsen hello med hello följande kommando: 

```bash
git clone https://github.com/Azure-Samples/azure-voting-app-redis.git
```

hello manifestfilen finns i hello följande katalogen för hello klona lagringsplatsen.

```bash
/azure-voting-app-redis/kubernetes-manifests/azure-vote-all-in-one-redis.yml
```

## <a name="update-manifest-file"></a>Uppdatera manifestfilen

Om du använder Azure-behållare registret toostore hello behållaren bilder, uppdateras hello manifestet måste toobe med hello ACR loginServer namn.

Hämta hello ACR inloggningsnamnet server med hello [az acr lista](/cli/azure/acr#list) kommando.

```azurecli-interactive
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

hello exempel manifestet har skapats i förväg med databasnamnet *microsoft*. Öppna hello-filen med en textredigerare och Ersätt hello *microsoft* värdet med hello inloggningen servernamnet för din ACR-instans.

```yaml
containers:
- name: azure-vote-front
  image: microsoft/azure-vote-front:redis-v1
```

## <a name="deploy-application"></a>Distribuera program

Använd hello [kubectl skapa](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#create) kommandot toorun hello program. Det här kommandot Parsar hello manifestfilen och skapa hello definierats Kubernetes objekt.

```azurecli-interactive
kubectl create -f ./azure-voting-app-redis/kubernetes-manifests/azure-vote-all-in-one-redis.yml
```

Resultat:

```bash
deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
service "azure-vote-front" created
```

## <a name="test-application"></a>Testa program

En [Kubernetes service](https://kubernetes.io/docs/concepts/services-networking/service/) skapas som visar hello programmet toohello internet. Den här processen kan ta några minuter. 

toomonitor pågår, Använd hello [kubectl hämta service](https://review.docs.microsoft.com/en-us/azure/container-service/container-service-kubernetes-walkthrough?branch=pr-en-us-17681) med hello `--watch` argumentet.

```azurecli-interactive
kubectl get service azure-vote-front --watch
```

Inledningsvis hello **externa IP-** för hello *azure rösten sista* tjänsten visas som *väntande*. När hello extern IP-adress har ändrats från *väntande* tooan *IP-adress*, använda `CTRL-C` toostop hello kubectl titta på processen.

```bash
NAME               CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   10.0.42.158   <pending>     80:31873/TCP   1m
azure-vote-front   10.0.42.158   52.179.23.131 80:31873/TCP   2m
```

toosee hello program, bläddra toohello extern IP-adress.

![Bild av Kubernetes-kluster i Azure](media/container-service-kubernetes-tutorials/azure-vote.png)

## <a name="next-steps"></a>Nästa steg

I den här självstudiekursen har hello Azure rösten program distribuerade tooan Kubernetes för Azure Container Service-kluster. Aktiviteter har slutförts är:  

> [!div class="checklist"]
> * Hämta Kubernetes manifestfiler
> * Kör hello program i Kubernetes
> * Testad hello program

Avancera toohello nästa självstudiekurs toolearn om att skala både ett Kubernetes program och hello underliggande Kubernetes infrastruktur. 

> [!div class="nextstepaction"]
> [Skala Kubernetes program och infrastruktur](./container-service-tutorial-kubernetes-scale.md)