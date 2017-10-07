---
title: "aaaAzure Container Service tutorial – förbereda ACR | Microsoft Docs"
description: "Självstudiekurs för Azure Container Service - förbereda ACR"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Docker, Containers, Micro-services, Kubernetes, DC/OS, Azure
ms.assetid: 
ms.service: container-service
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/21/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 3980e5ce4eb9836f83c761a2f76c944bb3f13060
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-use-azure-container-registry"></a>Distribuera och använda Azure Container registret

Azure Container registret (ACR) är en Azure-baserade, privat registret för Docker behållare bilder. Den här självstudiekursen, en del två av sju, går igenom distribuera en Azure-behållare registret-instans och trycka på en behållare avbildningen tooit. Slutfört stegen innefattar:

> [!div class="checklist"]
> * Distribuera en Azure Container registret (ACR)-instans
> * En avbildning av behållare för ACR-märkning
> * Överför hello bilden tooACR

Den här ACR-instansen är integrerad med ett Azure Container Service Kubernetes-kluster för säker körning av behållare bilder i efterföljande självstudiekurser. 

## <a name="before-you-begin"></a>Innan du börjar

I hello [tidigare kursen](./container-service-tutorial-kubernetes-prepare-app.md), en behållare avbildning har skapats för ett enkelt Azure röstning program. I den här kursen är den här avbildningen pushas tooan Azure Container registret. Om du inte har skapat hello Azure röstning appavbildning returnerar för[kursen 1 – skapa behållaren bilder](./container-service-tutorial-kubernetes-prepare-app.md). Du kan också detaljerad hello steg här fungerar med alla behållare avbildningen.

Den här kursen kräver att du kör hello Azure CLI version 2.0.4 eller senare. Kör `az --version` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="deploy-azure-container-registry"></a>Distribuera Azure-behållaren registret

När du distribuerar ett Azure Container registret, måste du först en resursgrupp. En Azure-resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras.

Skapa en resursgrupp med hello [az gruppen skapa](/cli/azure/group#create) kommando. I det här exemplet en resursgrupp med namnet *myResourceGroup* skapas i hello *westeurope* region.

```azurecli
az group create --name myResourceGroup --location westeurope
```

Skapa en Azure-behållare registret med hello [az acr skapa](/cli/azure/acr#create) kommando. hello namn på en behållare registernyckel **måste vara unika**.

```azurecli
az acr create --resource-group myResourceGroup --name <acrName> --sku Basic --admin-enabled true
```

I hela hello resten av den här självstudiekursen använder vi ”acrname” som platshållare för hello registret behållarnamn som du har valt.

## <a name="container-registry-login"></a>Behållaren registret inloggning

Du måste logga in tooyour ACR-instansen innan du skickar bilder tooit. Använd hello [az acr inloggning](https://docs.microsoft.com/en-us/cli/azure/acr#login) kommandot toocomplete hello igen. Du måste tooprovide hello unika namn som toohello behållare registret när den skapades.

```azurecli
az acr login --name <acrName>
```

hello kommando returnerar meddelandet inloggningen lyckades när den har slutförts.

## <a name="tag-container-images"></a>Taggen behållaren bilder

Varje behållare avbildning måste toobe märkta med hello loginServer namnet på hello registret. Den här etiketten används för routning vid push-installation behållaren bilder tooan avbildningen registret.

toosee en lista över aktuella bilder, Använd hello [docker bilder](https://docs.docker.com/engine/reference/commandline/images/) kommando.

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

tooget hello loginServer namn, kör följande kommando hello.

```azurecli
az acr show --name <acrName> --query loginServer --output table
```

Nu taggen hello *azure rösten sista* avbildningen med hello loginServer av hello behållaren registret. Lägg även till `:redis-v1` toohello slutet av hello avbildningsnamn. Den här taggen anger hello avbildningen versionen.

```bash
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:redis-v1
```

När taggade, köra [docker bilder] (https://docs.docker.com/engine/reference/commandline/images/) tooverify hello igen.

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

## <a name="push-images-tooregistry"></a>Push-avbildningar tooregistry

Push-hello *azure rösten sista* bild toohello registret. 

Med följande exempel hello Ersätt hello ACR loginServer namn med hello loginServer från din miljö.

```bash
docker push <acrLoginServer>/azure-vote-front:redis-v1
```

Detta tar några minuter toocomplete.

## <a name="list-images-in-registry"></a>Lista över bilder i registret

tooreturn en lista över bilder som har aviserats tooyour Azure-behållare registret, användaren hello [az acr databaslistan](/cli/azure/acr/repository#list) kommando. Uppdatera hello kommando med hello ACR instansnamn.

```azurecli
az acr repository list --name <acrName> --output table
```

Resultat:

```azurecli
Result
----------------
azure-vote-front
```

Och sedan använda toosee hello taggar för en viss bild hello [az acr databasen Visa-taggar](/cli/azure/acr/repository#show-tags) kommando.

```azurecli
az acr repository show-tags --name <acrName> --repository azure-vote-front --output table
```

Resultat:

```azurecli
Result
--------
redis-v1
```

Vid självstudiekursen slut har hello behållaren avbildningen lagrats i en privat Azure-behållare registret-instans. Den här avbildningen distribueras från ACR tooa Kubernetes kluster i efterföljande självstudiekurser.

## <a name="next-steps"></a>Nästa steg

I den här självstudiekursen förbereddes ett Azure Container registret för användning i ett Kubernetes ACS-kluster. hello följande steg har slutförts:

> [!div class="checklist"]
> * Distribuera en Azure-behållare registret-instans
> * En avbildning av behållaren har taggats för ACR
> * Överförda hello bilden tooACR

Avancera toohello nästa självstudiekurs toolearn om hur du distribuerar ett Kubernetes kluster i Azure.

> [!div class="nextstepaction"]
> [Distribuera Kubernetes kluster](./container-service-tutorial-kubernetes-deploy-cluster.md)