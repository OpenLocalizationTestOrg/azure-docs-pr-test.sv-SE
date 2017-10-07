---
title: "aaaAzure Behållarinstanser tutorial – förbereda din app | Azure-dokument"
description: "Förbereda en app för distribution tooAzure Behållarinstanser"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 406ba796e5fefb1527f2e894cc3f7bbd8f7a5fd1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-container-for-deployment-tooazure-container-instances"></a>Skapa en behållare för distribution tooAzure Behållarinstanser

Azure Container Instances möjliggör distribution av Docker-behållare till en Azure-infrastruktur utan att tillhandahålla några virtuella datorer eller anpassa eventuella tjänster på högre nivå. I den här självstudien får du bygga en enkel webbapp i Node.js och paketera den i en behållare som kan köras med Azure Container Instances. Vi kommer att ta upp:

> [!div class="checklist"]
> * Klona programkällan från GitHub  
> * Skapa behållaravbildningar från en programkälla
> * Testar hello-avbildningar i en lokal Docker-miljö

I efterföljande självstudiekurser du överför din bild tooan Azure Container registret, och sedan distribuera dem tooAzure Behållarinstanser.

## <a name="before-you-begin"></a>Innan du börjar

Den här självstudien förutsätter grundläggande kunskaper om grundläggande Docker-begrepp som behållare, behållaravbildningar och grundläggande docker-kommandon. Om det behövs kan du läsa [Get started with Docker]( https://docs.docker.com/get-started/) (Komma igång med Docker) för att få en genomgång av grunden för behållare. 

toocomplete den här kursen behöver du en Docker-utvecklingsmiljö. Docker innehåller paket som enkelt kan konfigurera Docker på en [Mac-](https://docs.docker.com/docker-for-mac/), [Windows-](https://docs.docker.com/docker-for-windows/) eller [Linux-](https://docs.docker.com/engine/installation/#supported-platforms)dator.

## <a name="get-application-code"></a>Hämta programkod

hello-exempel i den här självstudiekursen innehåller en enkel webbapp som skapats [Node.js](http://nodejs.org). hello appen fungerar statiska HTML-sidan och ser ut så här:

![Självstudieappen visas i webbläsare][aci-tutorial-app]

Använda git toodownload hello exemplet:

```bash
git clone https://github.com/Azure-Samples/aci-helloworld.git
```

## <a name="build-hello-container-image"></a>Skapa hello behållaren image

hello Dockerfile i lagringsplatsen för hello exempel visar hur hello behållare skapas. Den startar från en [officiella Node.js bild] [ dockerhub-nodeimage] baserat på [Alpine Linux](https://alpinelinux.org/), en liten distribution som är väl lämpade toouse med behållare. Den sedan kopierar hello programfiler till hello container, installerar beroenden med hello noden Package Manager och slutligen startar hello-program.

```
FROM node:8.2.0-alpine
RUN mkdir -p /usr/src/app
COPY ./app/* /usr/src/app/
WORKDIR /usr/src/app
RUN npm install
CMD node /usr/src/app/index.js
```

Använd hello `docker build` kommandot toocreate hello behållaren bilden, märkning som *aci kursen app*:

```bash
docker build ./aci-helloworld -t aci-tutorial-app
```

Använd hello `docker images` toosee hello inbyggda avbildningen:

```bash
docker images
```

Resultat:

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
aci-tutorial-app             latest              5c745774dfa9        39 seconds ago       68.1 MB
```

## <a name="run-hello-container-locally"></a>Kör hello behållaren lokalt

Innan du försöker distribuera hello behållaren tooAzure Behållarinstanser köra det lokalt tooconfirm att det fungerar. Hej `-d` växel kan hello-behållare som körs i bakgrunden hello, medan `-p` kan du toomap en valfri port på din beräknings-tooport 80 i hello behållaren.

```bash
docker run -d -p 8080:80 aci-tutorial-app
```

Öppna hello webbläsare toohttp://localhost:8080 tooconfirm som hello behållare körs.

![Hello-app som körs lokalt i hello webbläsare][aci-tutorial-app-local]

## <a name="next-steps"></a>Nästa steg

I kursen får skapat du en avbildning av behållare som kan vara distribuerade tooAzure Behållarinstanser. hello följande steg har slutförts:

> [!div class="checklist"]
> * Kloningen hello programmet källa från GitHub  
> * Skapa behållaravbildningar från en programkälla
> * Testa hello behållaren lokalt

Avancera toohello nästa självstudiekurs toolearn om att lagra behållaren bilder i ett Azure Container registret.

> [!div class="nextstepaction"]
> [Push-avbildningar tooAzure behållare registret](./container-instances-tutorial-prepare-acr.md)

<!-- LINKS -->
[dockerhub-nodeimage]: https://hub.docker.com/r/library/node/tags/8.2.0-alpine/

<!--- IMAGES --->
[aci-tutorial-app]:./media/container-instances-quickstart/aci-app-browser.png
[aci-tutorial-app-local]: ./media/container-instances-tutorial-prepare-app/aci-app-browser-local.png