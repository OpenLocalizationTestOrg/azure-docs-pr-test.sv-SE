---
title: "aaaAzure Behållarinstanser tutorial – förbereda Azure Container registret | Microsoft Docs"
description: "Azure Behållarinstanser tutorial – förbereda registret för Azure-behållare"
services: container-instances
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Docker, Containers, Micro-services, Kubernetes, DC/OS, Azure
ms.assetid: 
ms.service: container-instances
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 2525626125740c3c861fad36aad207d0b587ff54
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-use-azure-container-registry"></a>Distribuera och använda Azure Container registret

Detta är en del två av tre delar självstudiekursen. I hello [föregående steg](./container-instances-tutorial-prepare-app.md), en behållare avbildning har skapats för en enkel webbapp som skrivits i [Node.js](http://nodejs.org). I den här kursen är den här avbildningen pushas tooan Azure Container registret. Om du inte har skapat hello behållaren image returnerar för[kursen 1 – skapa behållaren image](./container-instances-tutorial-prepare-app.md). 

hello Azure Container registret är en Azure-baserade, privat registret för Docker behållare bilder. Den här kursen går igenom distribuera en Azure-behållare registret-instans och trycka på en behållare avbildningen tooit. Slutfört stegen innefattar:

> [!div class="checklist"]
> * Distribuera en Azure-behållare registret-instans
> * Behållaren image-märkning för Azure-behållare registernyckeln
> * Överför avbildningen tooAzure behållare registret

I efterföljande självstudiekurser distribuera hello behållare från din privata registret tooAzure Behållarinstanser.

## <a name="before-you-begin"></a>Innan du börjar

Den här kursen kräver att du kör hello Azure CLI version 2.0.4 eller senare. Kör `az --version` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).

## <a name="deploy-azure-container-registry"></a>Distribuera Azure-behållaren registret

När du distribuerar ett Azure Container registret, måste du först en resursgrupp. En Azure-resursgrupp är en logisk samling i vilka Azure resurser distribueras och hanteras.

Skapa en resursgrupp med hello [az gruppen skapa](/cli/azure/group#create) kommando. I det här exemplet en resursgrupp med namnet *myResourceGroup* skapas i hello *eastus* region.

```azurecli
az group create --name myResourceGroup --location eastus
```

Skapa en Azure-behållare registret med hello [az acr skapa](/cli/azure/acr#create) kommando. hello namn på en behållare registernyckel **måste vara unika**. I följande exempel hello, vi använda hello namnet *mycontainerregistry082*.

```azurecli
az acr create --resource-group myResourceGroup --name mycontainerregistry082 --sku Basic --admin-enabled true
```

I hela hello resten av den här självstudiekursen, använder vi `<acrname>` som platshållare för hello registret behållarnamn som du har valt.

## <a name="container-registry-login"></a>Behållaren registret inloggning

Du måste logga in tooyour ACR-instansen innan du skickar bilder tooit. Använd hello [az acr inloggning](https://docs.microsoft.com/en-us/cli/azure/acr#login) kommandot toocomplete hello igen. Du måste tooprovide hello unika namn som toohello behållare registret när den skapades.

```azurecli
az acr login --name <acrName>
```

hello kommando returnerar meddelandet inloggningen lyckades när den har slutförts.

## <a name="tag-container-image"></a>Taggen behållaren bild

toodeploy en behållare avbildning från ett privat register hello avbildningen måste toobe märkta med hello `loginServer` namn på hello registret.

toosee en lista över aktuella bilder, Använd hello `docker images` kommando.

```bash
docker images
```

Resultat:

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
aci-tutorial-app             latest              5c745774dfa9        39 seconds ago       68.1 MB
```

tooget hello loginServer namn, kör följande kommando hello.

```azurecli
az acr show --name <acrName> --query loginServer --output table
```

Taggen hello *aci kursen app* avbildningen med hello loginServer av hello behållaren registret. Lägg även till `:v1` toohello slutet av hello avbildningsnamn. Den här taggen anger versionsnumret för hello avbildningen.

```bash
docker tag aci-tutorial-app <acrLoginServer>/aci-tutorial-app:v1
```

När taggade, köra `docker images` tooverify hello igen.

```bash
docker images
```

Resultat:

```bash
REPOSITORY                                                TAG                 IMAGE ID            CREATED             SIZE
aci-tutorial-app                                          latest              5c745774dfa9        39 seconds ago      68.1 MB
mycontainerregistry082.azurecr.io/aci-tutorial-app        v1                  a9dace4e1a17        7 minutes ago       68.1 MB
```

## <a name="push-image-tooazure-container-registry"></a>Push-avbildningen tooAzure behållare registret

Push-hello *aci kursen app* bild toohello registret.

Med följande exempel hello Ersätt hello behållarnamn registret loginServer med hello loginServer från din miljö.

```bash
docker push <acrLoginServer>/aci-tutorial-app:v1
```

## <a name="list-images-in-azure-container-registry"></a>Listan avbildningar i Azure Container registret

tooreturn en lista över bilder som har aviserats tooyour Azure-behållare registret, användaren hello [az acr databaslistan](/cli/azure/acr/repository#list) kommando. Uppdatera hello kommando med hello behållarnamn registret.

```azurecli
az acr repository list --name <acrName> --output table
```

Resultat:

```azurecli
Result
----------------
aci-tutorial-app
```

Och sedan använda toosee hello taggar för en viss bild hello [az acr databasen Visa-taggar](/cli/azure/acr/repository#show-tags) kommando.

```azurecli
az acr repository show-tags --name <acrName> --repository aci-tutorial-app --output table
```

Resultat:

```azurecli
Result
--------
v1
```

## <a name="next-steps"></a>Nästa steg

Ett Azure Container registret förbereddes för användning med Azure Container instanser i den här självstudiekursen och hello behållaren avbildningen har flyttas. hello följande steg har slutförts:

> [!div class="checklist"]
> * Distribuera en Azure-behållare registret-instans
> * Behållaren image-märkning för Azure-behållare registernyckeln
> * Överför avbildningen tooAzure behållare registret

Avancera toohello nästa självstudiekurs toolearn om hur du distribuerar hello behållaren tooAzure med Azure Container instanser.

> [!div class="nextstepaction"]
> [Distribuera behållare tooAzure Behållarinstanser](./container-instances-tutorial-deploy-app.md)
