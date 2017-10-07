---
title: aaaManage Azure Swarm-kluster med Docker API | Microsoft Docs
description: "Distribuera behållare tooa Docker Swarm-kluster i Azure Container Service"
services: container-service
documentationcenter: 
author: rgardler
manager: madhana
editor: 
tags: acs, azure-container-service
keywords: "Docker, behållare, Micro-tjänster, Mesos, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/13/2016
ms.author: rogardle
ms.custom: mvc
ms.openlocfilehash: bb9b07c82a7b48caeb2e351455797cbf2a6e7480
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="container-management-with-docker-swarm"></a>Behållarhantering med Docker Swarm
Med Docker Swarm skapas en miljö för distribuering av arbetsbelastningar i behållare över en pooluppsättning med Docker-värdar. Docker Swarm använder interna Docker API för hello. hello arbetsflöde för att hantera behållare i Docker Swarm är nästan identisk toowhat det vore på en enskild behållarvärd. Det här dokumentet innehåller enkla exempel på distribution av arbetsbelastningar i behållare i en Azure Container Service-instans av Docker Swarm. Mer detaljerad dokumentation om Docker Swarm finns i [Docker Swarm på Docker.com](https://docs.docker.com/swarm/).

[!INCLUDE [container-service-swarm-mode-note](../../../includes/container-service-swarm-mode-note.md)]

Förutsättningar toohello övningarna i det här dokumentet:

[Skapa ett Swarm-kluster i Azure Container Service](container-service-deployment.md)

[Ansluta med hello Swarm-kluster i Azure Container Service](../container-service-connect.md)

## <a name="deploy-a-new-container"></a>Distribuera en ny behållare
toocreate en ny behållare i Docker Swarm hello använda hello `docker run` kommando (se till att du har öppnat en SSH-tunnel toohello huvudservrar enligt hello kraven ovan). Det här exemplet skapar en behållare från hello `yeasy/simple-web` avbildningen:

```bash
user@ubuntu:~$ docker run -d -p 80:80 yeasy/simple-web

4298d397b9ab6f37e2d1978ef3c8c1537c938e98a8bf096ff00def2eab04bf72
```

När hello behållaren har skapats kan du använda `docker ps` tooreturn information om hello behållare. Observera att hello Swarm-agenten som är värd för hello behållaren anges:

```bash
user@ubuntu:~$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
4298d397b9ab        yeasy/simple-web    "/bin/sh -c 'python i"   31 seconds ago      Up 9 seconds        10.0.0.5:80->80/tcp   swarm-agent-34A73819-1/happy_allen
```  

Du kan nu komma åt hello-program som körs i den här behållaren via hello offentliga DNS-namnet på hello Swarm-agentens belastningsutjämnare. Du hittar den här informationen i hello Azure-portalen:  

![Verklighetstrogna besöksresultat](./media/container-service-docker-swarm/real-visit.jpg)  

Hello belastningsutjämnaren har som standard portarna 80, 8080 och 443 öppen. Om du vill tooconnect på en annan port måste tooopen porten på hello Azure belastningsutjämnare för hello Agent poolen.

## <a name="deploy-multiple-containers"></a>Distribuera flera behållare
Som flera behållare har startats genom att köra 'docker kör' flera gånger, kan du använda hello `docker ps` kommandot toosee som är värd för hello behållarna körs på. I hello exemplet nedan jämnt tre behållare fördelade över hello tre Swarm-agenterna:  

```bash
user@ubuntu:~$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
11be062ff602        yeasy/simple-web    "/bin/sh -c 'python i"   11 seconds ago      Up 10 seconds       10.0.0.6:83->80/tcp   swarm-agent-34A73819-2/clever_banach
1ff421554c50        yeasy/simple-web    "/bin/sh -c 'python i"   49 seconds ago      Up 48 seconds       10.0.0.4:82->80/tcp   swarm-agent-34A73819-0/stupefied_ride
4298d397b9ab        yeasy/simple-web    "/bin/sh -c 'python i"   2 minutes ago       Up 2 minutes        10.0.0.5:80->80/tcp   swarm-agent-34A73819-1/happy_allen
```  

## <a name="deploy-containers-by-using-docker-compose"></a>Distribuera behållare med Docker Compose
Du kan använda Docker Compose tooautomate hello distributionen och konfigurationen av flera behållare. toodo Kontrollera så att en tunnel SSH (Secure Shell) har skapats och hello variabeln DOCKER_HOST har angetts (se hello förutsättningar ovan).

Skapa filen docker-compose.yml på ditt lokala system. toodo, Använd den här [exempel](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/docker-compose.yml).

```bash
web:
  image: adtd/web:0.1
  ports:
    - "80:80"
  links:
    - rest:rest-demo-azure.marathon.mesos
rest:
  image: adtd/rest:0.1
  ports:
    - "8080:8080"

```

Kör `docker-compose up -d` toostart hello behållardistributionerna:

```bash
user@ubuntu:~/compose$ docker-compose up -d
Pulling rest (adtd/rest:0.1)...
swarm-agent-3B7093B8-0: Pulling adtd/rest:0.1... : downloaded
swarm-agent-3B7093B8-2: Pulling adtd/rest:0.1... : downloaded
swarm-agent-3B7093B8-3: Pulling adtd/rest:0.1... : downloaded
Creating compose_rest_1
Pulling web (adtd/web:0.1)...
swarm-agent-3B7093B8-3: Pulling adtd/web:0.1... : downloaded
swarm-agent-3B7093B8-0: Pulling adtd/web:0.1... : downloaded
swarm-agent-3B7093B8-2: Pulling adtd/web:0.1... : downloaded
Creating compose_web_1
```

Slutligen returneras hello listan över behållare som körs. Den här listan visar hello-behållare som distribuerades med Docker Compose:

```bash
user@ubuntu:~/compose$ docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                     NAMES
caf185d221b7        adtd/web:0.1        "apache2-foreground"   2 minutes ago       Up About a minute   10.0.0.4:80->80/tcp       swarm-agent-3B7093B8-0/compose_web_1
040efc0ea937        adtd/rest:0.1       "catalina.sh run"      3 minutes ago       Up 2 minutes        10.0.0.4:8080->8080/tcp   swarm-agent-3B7093B8-0/compose_rest_1
```

Naturligtvis finns kan du använda `docker-compose ps` tooexamine endast hello behållare som definierats i din `compose.yml` fil.

## <a name="next-steps"></a>Nästa steg
[Mer information om Docker Swarm](https://docs.docker.com/swarm/)

