---
title: Hantera Azure Swarm-kluster med Docker API | Microsoft Docs
description: "Distribuera behållare till Docker Swarm-kluster i Azure Container Service"
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
ms.openlocfilehash: 6ca2d2e49c4b7f5eb0580e7091b09209f8b73a7c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="container-management-with-docker-swarm"></a>Behållarhantering med Docker Swarm
Med Docker Swarm skapas en miljö för distribuering av arbetsbelastningar i behållare över en pooluppsättning med Docker-värdar. Docker Swarm använder interna Docker API. Arbetsflödet för att hantera behållare i Docker Swarm är ungefär detsamma som det skulle ha varit på en enskild behållarvärd. Det här dokumentet innehåller enkla exempel på distribution av arbetsbelastningar i behållare i en Azure Container Service-instans av Docker Swarm. Mer detaljerad dokumentation om Docker Swarm finns i [Docker Swarm på Docker.com](https://docs.docker.com/swarm/).

[!INCLUDE [container-service-swarm-mode-note](../../../includes/container-service-swarm-mode-note.md)]

Förutsättningar för att kunna göra övningarna i det här dokumentet:

[Skapa ett Swarm-kluster i Azure Container Service](container-service-deployment.md)

[Anslut till Swarm-klustret i Azure Container Service](../container-service-connect.md)

## <a name="deploy-a-new-container"></a>Distribuera en ny behållare
För att skapa en ny behållare i Docker Swarm använder du `docker run`-kommandot (se till att du har öppnat en SSH-tunnel till huvudservrarna enligt kraven ovan). I det här exemplet skapas en behållare från avbildningen `yeasy/simple-web`:

```bash
user@ubuntu:~$ docker run -d -p 80:80 yeasy/simple-web

4298d397b9ab6f37e2d1978ef3c8c1537c938e98a8bf096ff00def2eab04bf72
```

När behållaren har skapats kan du använda `docker ps` för att returnera information om behållaren. Observera att Swarm-agenten som är värd för behållaren anges:

```bash
user@ubuntu:~$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
4298d397b9ab        yeasy/simple-web    "/bin/sh -c 'python i"   31 seconds ago      Up 9 seconds        10.0.0.5:80->80/tcp   swarm-agent-34A73819-1/happy_allen
```  

Du kan nu komma åt programmet som körs i den här behållaren via det offentliga DNS-namnet på Swarm-agentens belastningsutjämnare. Du hittar den här informationen i Azure Portal:  

![Verklighetstrogna besöksresultat](./media/container-service-docker-swarm/real-visit.jpg)  

Som standard har belastningsutjämnaren portarna 80, 8080 och 443 öppna. Om du vill ansluta till en annan port måste du öppna den porten på Azure Load Balancer för agentpoolen.

## <a name="deploy-multiple-containers"></a>Distribuera flera behållare
När flera behållare startas, genom att köra i ”docker run” flera gånger, kan du använda kommandot `docker ps` för att se vilka värdar som behållarna körs på. I det här exemplet är tre behållare jämnt fördelade över de tre Swarm-agenterna:  

```bash
user@ubuntu:~$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
11be062ff602        yeasy/simple-web    "/bin/sh -c 'python i"   11 seconds ago      Up 10 seconds       10.0.0.6:83->80/tcp   swarm-agent-34A73819-2/clever_banach
1ff421554c50        yeasy/simple-web    "/bin/sh -c 'python i"   49 seconds ago      Up 48 seconds       10.0.0.4:82->80/tcp   swarm-agent-34A73819-0/stupefied_ride
4298d397b9ab        yeasy/simple-web    "/bin/sh -c 'python i"   2 minutes ago       Up 2 minutes        10.0.0.5:80->80/tcp   swarm-agent-34A73819-1/happy_allen
```  

## <a name="deploy-containers-by-using-docker-compose"></a>Distribuera behållare med Docker Compose
Du kan använda Docker Compose för att automatisera distribution och konfiguration av flera behållare. Om du vill göra det måste du kontrollera att en SSH-tunnel (Secure Shell) har skapats och att variabeln DOCKER_HOST har angetts (se kraven ovan).

Skapa filen docker-compose.yml på ditt lokala system. Använd det här [exemplet](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/docker-compose.yml) för att göra det.

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

Kör `docker-compose up -d` för att starta behållardistributionerna:

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

Slutligen returneras listan över behållare som körs. Den här listan visar de behållare som distribuerades med Docker Compose:

```bash
user@ubuntu:~/compose$ docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                     NAMES
caf185d221b7        adtd/web:0.1        "apache2-foreground"   2 minutes ago       Up About a minute   10.0.0.4:80->80/tcp       swarm-agent-3B7093B8-0/compose_web_1
040efc0ea937        adtd/rest:0.1       "catalina.sh run"      3 minutes ago       Up 2 minutes        10.0.0.4:8080->8080/tcp   swarm-agent-3B7093B8-0/compose_rest_1
```

Naturligtvis kan du använda `docker-compose ps` för att bara undersöka de behållare som definieras i din `compose.yml`-fil.

## <a name="next-steps"></a>Nästa steg
[Mer information om Docker Swarm](https://docs.docker.com/swarm/)

