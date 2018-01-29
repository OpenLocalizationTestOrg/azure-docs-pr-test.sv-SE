---
title: "Kubernetes på Azure tutorial – Förbered appen"
description: "AKS tutorial – Förbered appen"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 10/24/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 60e0feb1e45ac5d9f35eac9667eaf9004d77e86a
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/18/2017
---
# <a name="prepare-application-for-azure-container-service-aks"></a>Förbereda program för Azure-behållaren (AKS)

I den här självstudiekursen, del 1 av åtta, har ett program för flera behållare förberetts för användning i Kubernetes. Slutfört stegen innefattar:  

> [!div class="checklist"]
> * Klona programkällan från GitHub  
> * Skapa en avbildning av behållare från käll-program
> * Testa programmet i en lokal Docker-miljö

Följande program är tillgängligt i din lokala utvecklingsmiljö när slutförd.

![Bild av Kubernetes-kluster i Azure](./media/container-service-tutorial-kubernetes-prepare-app/azure-vote.png)

I efterföljande självstudiekurser är behållaren avbildningen har överförts till ett Azure Container registret och kör sedan i ett AKS-kluster.

## <a name="before-you-begin"></a>Innan du börjar

Den här självstudien förutsätter grundläggande kunskaper om grundläggande Docker-begrepp som behållare, behållaravbildningar och grundläggande docker-kommandon. Om det behövs, se [Kom igång med Docker] [ docker-get-started] för en introduktion om grunderna i behållaren. 

För att slutföra den här självstudien behöver du en Docker-utvecklingsmiljö. Docker innehåller paket som enkelt kan konfigurera Docker på någon [Mac][docker-for-mac], [Windows][docker-for-windows], eller [Linux] [ docker-for-linux] system.

Azure Cloud Shell inkluderar inte Docker-komponenter som krävs för att slutföra varje steg den här kursen. Därför bör du använda en fullständig Docker-utvecklingsmiljö.

## <a name="get-application-code"></a>Hämta programkod

Exempelprogrammet används i den här kursen är en grundläggande röstning app. Programmet består av en frontend-webbservrar komponent och en serverdel Redis-instans. Webbkomponenten paketeras till en bild med anpassade container. Redis-instans använder en oförändrad bild från Docker-hubben.  

Använda git för att hämta en kopia av programmet till din utvecklingsmiljö.

```console
git clone https://github.com/Azure-Samples/azure-voting-app-redis.git
```

Ändra kataloger så att du arbetar från den klonade katalogen.

```console
cd azure-voting-app-redis
```

I katalogen är programmets källkod, en förskapad Docker compose fil- och en Kubernetes manifestfil. De här filerna används i hela uppsättningen av kursen. 

## <a name="create-container-images"></a>Skapa avbildningar av behållare

[Docker Compose] [ docker-compose] kan användas för att automatisera build från behållaren avbildningar och distribution av program med flera behållare.

Kör den `docker-compose.yaml` filen för att skapa avbildningen behållare, ladda ned avbildningen Redis och starta programmet.

```console
docker-compose up -d
```

När du är klar, kan du använda den [docker bilder] [ docker-images] kommandot för att se bilderna som har skapats.

```console
docker images
```

Observera att tre bilder har hämtat eller skapat. Den `azure-vote-front` avbildningen som innehåller programmet och använder den `nginx-flask` avbildningen som bas. Den `redis` bilden används för att starta en Redis-instans.

```
REPOSITORY                   TAG        IMAGE ID            CREATED             SIZE
azure-vote-front             latest     9cc914e25834        40 seconds ago      694MB
redis                        latest     a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask   flask      788ca94b2313        9 months ago        694MB
```

Kör den [docker ps] [ docker-ps] kommandot för att se behållarna som körs.

```console
docker ps
```

Resultat:

```
CONTAINER ID        IMAGE             COMMAND                  CREATED             STATUS              PORTS                           NAMES
82411933e8f9        azure-vote-front  "/usr/bin/supervisord"   57 seconds ago      Up 30 seconds       443/tcp, 0.0.0.0:8080->80/tcp   azure-vote-front
b68fed4b66b6        redis             "docker-entrypoint..."   57 seconds ago      Up 30 seconds       0.0.0.0:6379->6379/tcp          azure-vote-back
```

## <a name="test-application-locally"></a>Testa programmet lokalt

Bläddra till http://localhost: 8080 för att se programmet som körs.

![Bild av Kubernetes-kluster i Azure](./media/container-service-tutorial-kubernetes-prepare-app/azure-vote.png)

## <a name="clean-up-resources"></a>Rensa resurser

Nu när programfunktionen har verifierats, kan behållarna som körs stoppas och tas bort. Ta inte bort behållaren bilder. Den `azure-vote-front` bilden överförs till en Azure-behållare registret instans i nästa kurs.

Kör följande om du vill stoppa körs behållare.

```console
docker-compose stop
```

Ta bort stoppad behållare och resurser med följande kommando.

```console
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
> [Push-avbildningar i Azure Container registret][aks-tutorial-prepare-acr]

<!-- LINKS - external -->
[docker-compose]: https://docs.docker.com/compose/
[docker-for-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-for-mac]: https://docs.docker.com/docker-for-mac/
[docker-for-windows]: https://docs.docker.com/docker-for-windows/
[docker-get-started]: https://docs.docker.com/get-started/
[docker-images]: https://docs.docker.com/engine/reference/commandline/images/
[docker-ps]: https://docs.docker.com/engine/reference/commandline/ps/

<!-- LINKS - internal -->
[aks-tutorial-prepare-acr]: ./tutorial-kubernetes-prepare-acr.md