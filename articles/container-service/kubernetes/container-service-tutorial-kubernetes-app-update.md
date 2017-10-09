---
title: "Självstudier för aaaAzure Container Service - uppdateringsprogrammet | Microsoft Docs"
description: "Självstudiekurs för Azure Container Service - uppdateringsprogrammet"
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
ms.date: 07/26/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: c467498bab7952926a18e45ffbb21051a98739d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="update-an-application-in-kubernetes"></a>Uppdatera ett program i Kubernetes

När du distribuerar ett program i Kubernetes, kan den uppdateras genom att ange en ny behållare bild eller Bildversion. När du uppdaterar ett program, mellanlagras hello programuppdatering så att endast en del av hello distributionen uppdateras samtidigt. Uppdateringen mellanlagrade kan hello programmet tookeep körs under hello uppdatering och tillhandahåller en mekanism för återställning om ett distributionsfel inträffar. 

I den här självstudiekursen uppdateras sex tillhör sju hello Azure röst exempelapp. Uppgifterna som du har slutfört är:

> [!div class="checklist"]
> * Uppdatera hello frontend programkod
> * Skapa en bild av uppdaterade behållare
> * Skicka hello behållaren image tooAzure behållare registret
> * Distribuera hello uppdaterade behållaren avbildningen

I efterföljande självstudiekurser är Operations Management Suite konfigurerade toomonitor hello Kubernetes kluster.

## <a name="before-you-begin"></a>Innan du börjar

I tidigare självstudier, ett program som har paketerats till en behållare bild, hello avbildningen överfört tooAzure behållare registret och ett Kubernetes kluster skapas. hello programmet körs sedan hello Kubernetes klustret. 

Om du inte har utfört stegen och vill toofollow längs, returnerar för[kursen 1 – skapa behållaren bilder](./container-service-tutorial-kubernetes-prepare-app.md). 

## <a name="update-application"></a>Uppdatera program

toocomplete hello steg i den här självstudiekursen, du måste ha klona en kopia av hello Azure rösten program. Om det behövs kan du skapa klonade kopian med hello följande kommando:

```bash
git clone https://github.com/Azure-Samples/azure-voting-app-redis.git
```

Öppna hello `config_file.cfg` fil med någon kod eller textredigerare. Du hittar den här filen under hello följande katalogen för hello klona lagringsplatsen.

```bash
 /azure-voting-app-redis/azure-vote/azure-vote/config_file.cfg
```

Ändra hello värden för `VOTE1VALUE` och `VOTE2VALUE`, och sedan spara hello-filen.

```bash
# UI Configurations
TITLE = 'Azure Voting App'
VOTE1VALUE = 'Blue'
VOTE2VALUE = 'Purple'
SHOWHOST = 'false'
```

Använd [docker compose](https://docs.docker.com/compose/) toore-skapa hello frontend image- och kör hello uppdateras.

```bash
docker-compose -f ./azure-voting-app-redis/docker-compose.yml up --build -d
```

## <a name="test-application-locally"></a>Testa programmet lokalt

Bläddra för`http://localhost:8080` toosee hello uppdaterade tillämpningsprogrammet.

![Bild av Kubernetes-kluster i Azure](media/container-service-kubernetes-tutorials/vote-app-updated.png)

## <a name="tag-and-push-images"></a>Taggen och push-avbildningar

Taggen hello *azure rösten sista* avbildningen med hello loginServer av hello behållaren registret.

Om du använder Azure-behållare registret hämta hello inloggningen servernamn med hello [az acr lista](/cli/azure/acr#list) kommando.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Använd [docker-taggen](https://docs.docker.com/engine/reference/commandline/tag/) tootag hello avbildningen. Ersätt `<acrLoginServer>` med Azure Container registret inloggningsnamnet server eller offentliga registret värdnamn.

```bash
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:redis-v2
```

Använd [docker push](https://docs.docker.com/engine/reference/commandline/push/) tooupload hello avbildningen tooyour registret. Ersätt `<acrLoginServer>` med Azure Container registret inloggningsnamnet server eller offentliga registret värdnamn.

```bash
docker push <acrLoginServer>/azure-vote-front:redis-v2
```

## <a name="deploy-update-application"></a>Distribuera program för uppdatering

tooensure maximala drifttid, flera instanser av hello programmet baljor måste köras. Kontrollera konfigurationen med hello [kubectl hämta baljor](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) kommando.

```bash
kubectl get pod
```

Resultat:

```bash
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-217588096-5w632    1/1       Running   0          10m
azure-vote-front-233282510-b5pkz   1/1       Running   0          10m
azure-vote-front-233282510-dhrtr   1/1       Running   0          10m
azure-vote-front-233282510-pqbfk   1/1       Running   0          10m
```

Om du inte har flera skida kör hello azure rösten sista bilden skala hello *azure rösten sista* distribution.


```azurecli-interactive
kubectl scale --replicas=3 deployment/azure-vote-front
```

tooupdate hello program, Använd hello [kubectl set](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#set) kommando. Uppdatera `<acrLoginServer>` med hello server eller en värd inloggningsnamnet behållaren registret.

```azurecli-interactive
kubectl set image deployment azure-vote-front azure-vote-front=<acrLoginServer>/azure-vote-front:redis-v2
```

toomonitor hello distribution, Använd hello [kubectl hämta baljor](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) kommando. Eftersom hello uppdatera programmet distribueras avslutas din skida och återskapas med hello ny behållare avbildning.

```azurecli-interactive
kubectl get pod
```

Resultat:

```bash
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2978095810-gq9g0   1/1       Running   0          5m
azure-vote-front-1297194256-tpjlg   1/1       Running   0         1m
azure-vote-front-1297194256-tptnx   1/1       Running   0         5m
azure-vote-front-1297194256-zktw9   1/1       Terminating   0         1m
```

## <a name="test-updated-application"></a>Testa uppdaterade program

Hämta hello externa IP-adress hello *azure rösten sista* service.

```azurecli-interactive
kubectl get service azure-vote-front
```

Bläddra toohello IP-adress toosee hello uppdaterade tillämpningsprogrammet.

![Bild av Kubernetes-kluster i Azure](media/container-service-kubernetes-tutorials/vote-app-updated-external.png)

## <a name="next-steps"></a>Nästa steg

I den här självstudiekursen, uppdatera ett program och distribuerat den här uppdateringen tooa Kubernetes klustret. hello följande uppgifter har slutförts:

> [!div class="checklist"]
> * Uppdaterade hello frontend programkod
> * Skapa en bild av uppdaterade behållare
> * Pushas hello behållaren image tooAzure behållare registret
> * Distribuerade hello uppdateras program

I förväg toohello nästa självstudiekurs toolearn om hur toomonitor Kubernetes med Operations Management Suite.

> [!div class="nextstepaction"]
> [Övervaka Kubernetes med OMS (Monitor Kubernetes with OMS)](./container-service-tutorial-kubernetes-monitor.md)