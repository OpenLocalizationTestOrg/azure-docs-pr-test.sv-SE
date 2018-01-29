---
title: "Självstudiekurs för Azure Container Service - Förbered appen"
description: "Självstudiekurs för Azure Container Service - Förbered appen"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 09/14/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 3dcfc0d454926d6040692b0e8a9d44b13e7603c5
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/06/2017
---
# <a name="create-container-images-to-be-used-with-azure-container-service"></a>Skapa avbildningar av behållare som ska användas med Azure Container Service

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

I den här självstudiekursen, del 1 av sju, har ett program för flera behållare förberetts för användning i Kubernetes. Slutfört stegen innefattar:  

> [!div class="checklist"]
> * Klona programkällan från GitHub  
> * Skapa en avbildning av behållare från käll-program
> * Testa programmet i en lokal Docker-miljö

Följande program är tillgängligt i din lokala utvecklingsmiljö när slutförd.

![Bild av Kubernetes-kluster i Azure](media/container-service-kubernetes-tutorials/azure-vote.png)

I efterföljande självstudiekurser behållaren avbildningen har överförts till ett Azure Container registret och sedan köras i en Azure värd Kubernetes klustret.

## <a name="before-you-begin"></a>Innan du börjar

Den här självstudien förutsätter grundläggande kunskaper om grundläggande Docker-begrepp som behållare, behållaravbildningar och grundläggande docker-kommandon. Om det behövs kan du läsa [Get started with Docker]( https://docs.docker.com/get-started/) (Komma igång med Docker) för att få en genomgång av grunden för behållare. 

För att slutföra den här självstudien behöver du en Docker-utvecklingsmiljö. Docker innehåller paket som enkelt kan konfigurera Docker på en [Mac-](https://docs.docker.com/docker-for-mac/), [Windows-](https://docs.docker.com/docker-for-windows/) eller [Linux-](https://docs.docker.com/engine/installation/#supported-platforms)dator.

Azure Cloud Shell inkluderar inte Docker-komponenter som krävs för att slutföra varje steg den här kursen. Därför bör du använda en fullständig Docker-utvecklingsmiljö.

## <a name="get-application-code"></a>Hämta programkod

Exempelprogrammet används i den här kursen är en grundläggande röstning app. Programmet består av en frontend-webbservrar komponent och en serverdel Redis-instans. Webbkomponenten paketeras till en bild med anpassade container. Redis-instans använder en oförändrad bild från Docker-hubben.  

Använda git för att hämta en kopia av programmet till din utvecklingsmiljö.

```bash
git clone https://github.com/Azure-Samples/azure-voting-app-redis.git
```

Ändra kataloger så att du arbetar från den klonade katalogen.

```
cd azure-voting-app-redis
```

I katalogen är programmets källkod, en förskapad Docker compose fil- och en Kubernetes manifestfil. De här filerna används i hela uppsättningen av kursen. 

## <a name="create-container-images"></a>Skapa avbildningar av behållare

[Docker Compose](https://docs.docker.com/compose/) kan användas för att automatisera build från behållaren avbildningar och distribution av program med flera behållare.

Kör den `docker-compose.yml` filen för att skapa avbildningen behållare, ladda ned avbildningen Redis och starta programmet.

```bash
docker-compose up -d
```

När du är klar, kan du använda den [docker bilder](https://docs.docker.com/engine/reference/commandline/images/) kommandot för att se bilderna som har skapats.

```bash
docker images
```

Observera att tre bilder har hämtat eller skapat. Den `azure-vote-front` avbildningen som innehåller programmet och använder den `nginx-flask` avbildningen som bas. Den `redis` bilden används för att starta en Redis-instans.

```bash
REPOSITORY                   TAG        IMAGE ID            CREATED             SIZE
azure-vote-front             latest     9cc914e25834        40 seconds ago      694MB
redis                        latest     a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask   flask      788ca94b2313        9 months ago        694MB
```

Kör den [docker ps](https://docs.docker.com/engine/reference/commandline/ps/) kommandot för att se behållarna som körs.

```bash
docker ps
```

Resultat:

```bash
CONTAINER ID        IMAGE             COMMAND                  CREATED             STATUS              PORTS                           NAMES
82411933e8f9        azure-vote-front  "/usr/bin/supervisord"   57 seconds ago      Up 30 seconds       443/tcp, 0.0.0.0:8080->80/tcp   azure-vote-front
b68fed4b66b6        redis             "docker-entrypoint..."   57 seconds ago      Up 30 seconds       0.0.0.0:6379->6379/tcp          azure-vote-back
```

## <a name="test-application-locally"></a>Testa programmet lokalt

Bläddra till http://localhost: 8080 för att se programmet som körs.

![Bild av Kubernetes-kluster i Azure](media/container-service-kubernetes-tutorials/azure-vote.png)

## <a name="clean-up-resources"></a>Rensa resurser

Nu när programfunktionen har verifierats, kan behållarna som körs stoppas och tas bort. Ta inte bort behållaren bilder. Den `azure-vote-front` bilden överförs till en Azure-behållare registret instans i nästa kurs.

Kör följande om du vill stoppa körs behållare.

```bash
docker-compose stop
```

Ta bort stoppad behållare och resurser med följande kommando.

```bash
docker-compose down
```

Vid färdigställande har du en behållare avbildning som innehåller programmet Azure röst.

## <a name="next-steps"></a>Nästa steg

I den här självstudiekursen, ett program har testats och behållare bilder skapas för programmet. Följande steg har slutförts:

> [!div class="checklist"]
> * Klona programkällan från GitHub  
> * Skapa en avbildning av behållare från programmet källa
> * Testa programmet i en lokal Docker-miljö

Fortsätt till nästa självstudie och lär dig om att lagra behållaravbildningar i ett Azure Container Registry.

> [!div class="nextstepaction"]
> [Push-avbildningar med Azure Container Registry](./container-service-tutorial-kubernetes-prepare-acr.md)
