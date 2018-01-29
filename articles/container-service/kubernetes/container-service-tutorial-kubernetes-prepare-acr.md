---
title: "Självstudiekurs för Azure Container Service - förbereda ACR"
description: "Självstudiekurs för Azure Container Service - förbereda ACR"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 09/14/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: c9c8ad6dfd6df0e99f9e41eaf1da12ebeb2a2da6
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/08/2017
---
# <a name="deploy-and-use-azure-container-registry"></a>Distribuera och använda Azure Container registret

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

Azure Container registret (ACR) är en Azure-baserade, privat registret för Docker behållare bilder. Den här självstudiekursen, en del två av sju, går igenom distribution av en instans av Azure-behållare registret och överföra en behållare avbildning till den. Slutfört stegen innefattar:

> [!div class="checklist"]
> * Distribuera en Azure Container registret (ACR)-instans
> * En avbildning av behållare för ACR-märkning
> * Överför avbildningen till ACR

Den här ACR-instansen är integrerad med ett Azure Container Service Kubernetes kluster i efterföljande självstudiekurser. 

## <a name="before-you-begin"></a>Innan du börjar

I den [tidigare kursen](./container-service-tutorial-kubernetes-prepare-app.md), en behållare avbildning har skapats för ett enkelt Azure röstning program. Om du inte har skapat appavbildning Azure röstning återgå till [kursen 1 – skapa behållaren bilder](./container-service-tutorial-kubernetes-prepare-app.md).

Den här kursen kräver att du använder Azure CLI version 2.0.4 eller senare. Kör `az --version` för att hitta versionen. Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="deploy-azure-container-registry"></a>Distribuera Azure-behållaren registret

När du distribuerar ett Azure Container registret, måste du först en resursgrupp. En Azure-resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras.

Skapa en resursgrupp med kommandot [az group create](/cli/azure/group#create). I det här exemplet en resursgrupp med namnet `myResourceGroup` skapas i den `westeurope` region.

```azurecli
az group create --name myResourceGroup --location westeurope
```

Skapa en Azure-behållare registret med den [az acr skapa](/cli/azure/acr#create) kommando. Namnet på en behållare registret **måste vara unika**.

```azurecli
az acr create --resource-group myResourceGroup --name <acrName> --sku Basic
```

I resten av den här kursen använder vi `<acrname>` som platshållare för registret behållarnamn.

## <a name="container-registry-login"></a>Behållaren registret inloggning

Använd den [az acr inloggning](https://docs.microsoft.com/cli/azure/acr#az_acr_login) kommando för att logga in till ACR-instans. Du måste ange unika namnet på behållaren registret när den skapades.

```azurecli
az acr login --name <acrName>
```

Kommandot returnerar ett inloggningen lyckades meddelande när den har slutförts.

## <a name="tag-container-images"></a>Taggen behållaren bilder

Om du vill se en lista över aktuella bilder i [docker bilder](https://docs.docker.com/engine/reference/commandline/images/) kommando.

```bash
docker images
```

Resultat:

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED             SIZE
azure-vote-front             latest              4675398c9172        13 minutes ago      694MB
redis                        latest              a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask   flask               788ca94b2313        9 months ago        694MB
```

Varje behållare avbildning måste taggas med loginServer namn i registret. Den här etiketten används för routning vid push-installation behållaren avbildningar till en bild-registret.

Kör följande kommando för att få namnet loginServer.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Tagga nu den `azure-vote-front` avbildningen med loginServer av registret i behållaren. Lägg även till `:redis-v1` till slutet av avbildningens namn. Den här taggen anger den bildversionen.

```bash
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:redis-v1
```

När taggade, köra [docker bilder] (https://docs.docker.com/engine/reference/commandline/images/) att bekräfta åtgärden.

```bash
docker images
```

Resultat:

```bash
REPOSITORY                                           TAG                 IMAGE ID            CREATED             SIZE
azure-vote-front                                     latest              eaf2b9c57e5e        8 minutes ago       716 MB
mycontainerregistry082.azurecr.io/azure-vote-front   redis-v1            eaf2b9c57e5e        8 minutes ago       716 MB
redis                                                latest              a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask                           flask               788ca94b2313        8 months ago        694 MB
```

## <a name="push-images-to-registry"></a>Push-avbildningar till registret

Tryck på `azure-vote-front` avbildningen till registret. 

Med följande exempel ersätta ACR loginServer namn med loginServer från din miljö.

```bash
docker push <acrLoginServer>/azure-vote-front:redis-v1
```

Detta tar några minuter att slutföra.

## <a name="list-images-in-registry"></a>Lista över bilder i registret

Returnera en lista över bilder som har aviserats i Azure-behållare i registret användaren den [az acr databaslistan](/cli/azure/acr/repository#list) kommando. Uppdatera kommandot med namnet på ACR-instansen.

```azurecli
az acr repository list --name <acrName> --output table
```

Resultat:

```azurecli
Result
----------------
azure-vote-front
```

Och sedan använda taggar för en viss bild visas den [az acr databasen Visa-taggar](/cli/azure/acr/repository#show-tags) kommando.

```azurecli
az acr repository show-tags --name <acrName> --repository azure-vote-front --output table
```

Resultat:

```azurecli
Result
--------
redis-v1
```

Vid självstudiekursen slut har behållaren avbildningen lagrats i en privat Azure-behållare registret-instans. Den här avbildningen distribueras från ACR till ett kluster som Kubernetes i efterföljande självstudiekurser.

## <a name="next-steps"></a>Nästa steg

I den här självstudiekursen förbereddes ett Azure Container registret för användning i ett Kubernetes ACS-kluster. Följande steg har slutförts:

> [!div class="checklist"]
> * Distribuera en Azure-behållare registret-instans
> * En avbildning av behållaren har taggats för ACR
> * Överföra avbildningen till ACR

Gå vidare till nästa kurs mer information om hur du distribuerar ett Kubernetes kluster i Azure.

> [!div class="nextstepaction"]
> [Distribuera Kubernetes kluster](./container-service-tutorial-kubernetes-deploy-cluster.md)