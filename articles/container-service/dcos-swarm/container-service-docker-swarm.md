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
# <a name="container-management-with-docker-swarm"></a><span data-ttu-id="24e27-104">Behållarhantering med Docker Swarm</span><span class="sxs-lookup"><span data-stu-id="24e27-104">Container management with Docker Swarm</span></span>
<span data-ttu-id="24e27-105">Med Docker Swarm skapas en miljö för distribuering av arbetsbelastningar i behållare över en pooluppsättning med Docker-värdar.</span><span class="sxs-lookup"><span data-stu-id="24e27-105">Docker Swarm provides an environment for deploying containerized workloads across a pooled set of Docker hosts.</span></span> <span data-ttu-id="24e27-106">Docker Swarm använder interna Docker API.</span><span class="sxs-lookup"><span data-stu-id="24e27-106">Docker Swarm uses the native Docker API.</span></span> <span data-ttu-id="24e27-107">Arbetsflödet för att hantera behållare i Docker Swarm är ungefär detsamma som det skulle ha varit på en enskild behållarvärd.</span><span class="sxs-lookup"><span data-stu-id="24e27-107">The workflow for managing containers on a Docker Swarm is almost identical to what it would be on a single container host.</span></span> <span data-ttu-id="24e27-108">Det här dokumentet innehåller enkla exempel på distribution av arbetsbelastningar i behållare i en Azure Container Service-instans av Docker Swarm.</span><span class="sxs-lookup"><span data-stu-id="24e27-108">This document provides simple examples of deploying containerized workloads in an Azure Container Service instance of Docker Swarm.</span></span> <span data-ttu-id="24e27-109">Mer detaljerad dokumentation om Docker Swarm finns i [Docker Swarm på Docker.com](https://docs.docker.com/swarm/).</span><span class="sxs-lookup"><span data-stu-id="24e27-109">For more in-depth documentation on Docker Swarm, see [Docker Swarm on Docker.com](https://docs.docker.com/swarm/).</span></span>

[!INCLUDE [container-service-swarm-mode-note](../../../includes/container-service-swarm-mode-note.md)]

<span data-ttu-id="24e27-110">Förutsättningar för att kunna göra övningarna i det här dokumentet:</span><span class="sxs-lookup"><span data-stu-id="24e27-110">Prerequisites to the exercises in this document:</span></span>

[<span data-ttu-id="24e27-111">Skapa ett Swarm-kluster i Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="24e27-111">Create a Swarm cluster in Azure Container Service</span></span>](container-service-deployment.md)

[<span data-ttu-id="24e27-112">Anslut till Swarm-klustret i Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="24e27-112">Connect with the Swarm cluster in Azure Container Service</span></span>](../container-service-connect.md)

## <a name="deploy-a-new-container"></a><span data-ttu-id="24e27-113">Distribuera en ny behållare</span><span class="sxs-lookup"><span data-stu-id="24e27-113">Deploy a new container</span></span>
<span data-ttu-id="24e27-114">För att skapa en ny behållare i Docker Swarm använder du `docker run`-kommandot (se till att du har öppnat en SSH-tunnel till huvudservrarna enligt kraven ovan).</span><span class="sxs-lookup"><span data-stu-id="24e27-114">To create a new container in the Docker Swarm, use the `docker run` command (ensuring that you have opened an SSH tunnel to the masters as per the prerequisites above).</span></span> <span data-ttu-id="24e27-115">I det här exemplet skapas en behållare från avbildningen `yeasy/simple-web`:</span><span class="sxs-lookup"><span data-stu-id="24e27-115">This example creates a container from the `yeasy/simple-web` image:</span></span>

```bash
user@ubuntu:~$ docker run -d -p 80:80 yeasy/simple-web

4298d397b9ab6f37e2d1978ef3c8c1537c938e98a8bf096ff00def2eab04bf72
```

<span data-ttu-id="24e27-116">När behållaren har skapats kan du använda `docker ps` för att returnera information om behållaren.</span><span class="sxs-lookup"><span data-stu-id="24e27-116">After the container has been created, use `docker ps` to return information about the container.</span></span> <span data-ttu-id="24e27-117">Observera att Swarm-agenten som är värd för behållaren anges:</span><span class="sxs-lookup"><span data-stu-id="24e27-117">Notice here that the Swarm agent that is hosting the container is listed:</span></span>

```bash
user@ubuntu:~$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
4298d397b9ab        yeasy/simple-web    "/bin/sh -c 'python i"   31 seconds ago      Up 9 seconds        10.0.0.5:80->80/tcp   swarm-agent-34A73819-1/happy_allen
```  

<span data-ttu-id="24e27-118">Du kan nu komma åt programmet som körs i den här behållaren via det offentliga DNS-namnet på Swarm-agentens belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="24e27-118">You can now access the application that is running in this container through the public DNS name of the Swarm agent load balancer.</span></span> <span data-ttu-id="24e27-119">Du hittar den här informationen i Azure Portal:</span><span class="sxs-lookup"><span data-stu-id="24e27-119">You can find this information in the Azure portal:</span></span>  

![Verklighetstrogna besöksresultat](./media/container-service-docker-swarm/real-visit.jpg)  

<span data-ttu-id="24e27-121">Som standard har belastningsutjämnaren portarna 80, 8080 och 443 öppna.</span><span class="sxs-lookup"><span data-stu-id="24e27-121">By default the Load Balancer has ports 80, 8080 and 443 open.</span></span> <span data-ttu-id="24e27-122">Om du vill ansluta till en annan port måste du öppna den porten på Azure Load Balancer för agentpoolen.</span><span class="sxs-lookup"><span data-stu-id="24e27-122">If you want to connect on another port you will need to open that port on the Azure Load Balancer for the Agent Pool.</span></span>

## <a name="deploy-multiple-containers"></a><span data-ttu-id="24e27-123">Distribuera flera behållare</span><span class="sxs-lookup"><span data-stu-id="24e27-123">Deploy multiple containers</span></span>
<span data-ttu-id="24e27-124">När flera behållare startas, genom att köra i ”docker run” flera gånger, kan du använda kommandot `docker ps` för att se vilka värdar som behållarna körs på.</span><span class="sxs-lookup"><span data-stu-id="24e27-124">As multiple containers are started, by executing 'docker run' multiple times, you can use the `docker ps` command to see which hosts the containers are running on.</span></span> <span data-ttu-id="24e27-125">I det här exemplet är tre behållare jämnt fördelade över de tre Swarm-agenterna:</span><span class="sxs-lookup"><span data-stu-id="24e27-125">In the example below, three containers are spread evenly across the three Swarm agents:</span></span>  

```bash
user@ubuntu:~$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
11be062ff602        yeasy/simple-web    "/bin/sh -c 'python i"   11 seconds ago      Up 10 seconds       10.0.0.6:83->80/tcp   swarm-agent-34A73819-2/clever_banach
1ff421554c50        yeasy/simple-web    "/bin/sh -c 'python i"   49 seconds ago      Up 48 seconds       10.0.0.4:82->80/tcp   swarm-agent-34A73819-0/stupefied_ride
4298d397b9ab        yeasy/simple-web    "/bin/sh -c 'python i"   2 minutes ago       Up 2 minutes        10.0.0.5:80->80/tcp   swarm-agent-34A73819-1/happy_allen
```  

## <a name="deploy-containers-by-using-docker-compose"></a><span data-ttu-id="24e27-126">Distribuera behållare med Docker Compose</span><span class="sxs-lookup"><span data-stu-id="24e27-126">Deploy containers by using Docker Compose</span></span>
<span data-ttu-id="24e27-127">Du kan använda Docker Compose för att automatisera distribution och konfiguration av flera behållare.</span><span class="sxs-lookup"><span data-stu-id="24e27-127">You can use Docker Compose to automate the deployment and configuration of multiple containers.</span></span> <span data-ttu-id="24e27-128">Om du vill göra det måste du kontrollera att en SSH-tunnel (Secure Shell) har skapats och att variabeln DOCKER_HOST har angetts (se kraven ovan).</span><span class="sxs-lookup"><span data-stu-id="24e27-128">To do so, ensure that a Secure Shell (SSH) tunnel has been created and that the DOCKER_HOST variable has been set (see the pre-requisites above).</span></span>

<span data-ttu-id="24e27-129">Skapa filen docker-compose.yml på ditt lokala system.</span><span class="sxs-lookup"><span data-stu-id="24e27-129">Create a docker-compose.yml file on your local system.</span></span> <span data-ttu-id="24e27-130">Använd det här [exemplet](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/docker-compose.yml) för att göra det.</span><span class="sxs-lookup"><span data-stu-id="24e27-130">To do this, use this [sample](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/docker-compose.yml).</span></span>

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

<span data-ttu-id="24e27-131">Kör `docker-compose up -d` för att starta behållardistributionerna:</span><span class="sxs-lookup"><span data-stu-id="24e27-131">Run `docker-compose up -d` to start the container deployments:</span></span>

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

<span data-ttu-id="24e27-132">Slutligen returneras listan över behållare som körs.</span><span class="sxs-lookup"><span data-stu-id="24e27-132">Finally, the list of running containers will be returned.</span></span> <span data-ttu-id="24e27-133">Den här listan visar de behållare som distribuerades med Docker Compose:</span><span class="sxs-lookup"><span data-stu-id="24e27-133">This list reflects the containers that were deployed by using Docker Compose:</span></span>

```bash
user@ubuntu:~/compose$ docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                     NAMES
caf185d221b7        adtd/web:0.1        "apache2-foreground"   2 minutes ago       Up About a minute   10.0.0.4:80->80/tcp       swarm-agent-3B7093B8-0/compose_web_1
040efc0ea937        adtd/rest:0.1       "catalina.sh run"      3 minutes ago       Up 2 minutes        10.0.0.4:8080->8080/tcp   swarm-agent-3B7093B8-0/compose_rest_1
```

<span data-ttu-id="24e27-134">Naturligtvis kan du använda `docker-compose ps` för att bara undersöka de behållare som definieras i din `compose.yml`-fil.</span><span class="sxs-lookup"><span data-stu-id="24e27-134">Naturally, you can use `docker-compose ps` to examine only the containers defined in your `compose.yml` file.</span></span>

## <a name="next-steps"></a><span data-ttu-id="24e27-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="24e27-135">Next steps</span></span>
[<span data-ttu-id="24e27-136">Mer information om Docker Swarm</span><span class="sxs-lookup"><span data-stu-id="24e27-136">Learn more about Docker Swarm</span></span>](https://docs.docker.com/swarm/)

