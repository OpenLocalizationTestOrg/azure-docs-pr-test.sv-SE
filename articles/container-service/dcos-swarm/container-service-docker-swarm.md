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
# <a name="container-management-with-docker-swarm"></a><span data-ttu-id="fb79e-104">Behållarhantering med Docker Swarm</span><span class="sxs-lookup"><span data-stu-id="fb79e-104">Container management with Docker Swarm</span></span>
<span data-ttu-id="fb79e-105">Med Docker Swarm skapas en miljö för distribuering av arbetsbelastningar i behållare över en pooluppsättning med Docker-värdar.</span><span class="sxs-lookup"><span data-stu-id="fb79e-105">Docker Swarm provides an environment for deploying containerized workloads across a pooled set of Docker hosts.</span></span> <span data-ttu-id="fb79e-106">Docker Swarm använder interna Docker API för hello.</span><span class="sxs-lookup"><span data-stu-id="fb79e-106">Docker Swarm uses hello native Docker API.</span></span> <span data-ttu-id="fb79e-107">hello arbetsflöde för att hantera behållare i Docker Swarm är nästan identisk toowhat det vore på en enskild behållarvärd.</span><span class="sxs-lookup"><span data-stu-id="fb79e-107">hello workflow for managing containers on a Docker Swarm is almost identical toowhat it would be on a single container host.</span></span> <span data-ttu-id="fb79e-108">Det här dokumentet innehåller enkla exempel på distribution av arbetsbelastningar i behållare i en Azure Container Service-instans av Docker Swarm.</span><span class="sxs-lookup"><span data-stu-id="fb79e-108">This document provides simple examples of deploying containerized workloads in an Azure Container Service instance of Docker Swarm.</span></span> <span data-ttu-id="fb79e-109">Mer detaljerad dokumentation om Docker Swarm finns i [Docker Swarm på Docker.com](https://docs.docker.com/swarm/).</span><span class="sxs-lookup"><span data-stu-id="fb79e-109">For more in-depth documentation on Docker Swarm, see [Docker Swarm on Docker.com](https://docs.docker.com/swarm/).</span></span>

[!INCLUDE [container-service-swarm-mode-note](../../../includes/container-service-swarm-mode-note.md)]

<span data-ttu-id="fb79e-110">Förutsättningar toohello övningarna i det här dokumentet:</span><span class="sxs-lookup"><span data-stu-id="fb79e-110">Prerequisites toohello exercises in this document:</span></span>

[<span data-ttu-id="fb79e-111">Skapa ett Swarm-kluster i Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="fb79e-111">Create a Swarm cluster in Azure Container Service</span></span>](container-service-deployment.md)

[<span data-ttu-id="fb79e-112">Ansluta med hello Swarm-kluster i Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="fb79e-112">Connect with hello Swarm cluster in Azure Container Service</span></span>](../container-service-connect.md)

## <a name="deploy-a-new-container"></a><span data-ttu-id="fb79e-113">Distribuera en ny behållare</span><span class="sxs-lookup"><span data-stu-id="fb79e-113">Deploy a new container</span></span>
<span data-ttu-id="fb79e-114">toocreate en ny behållare i Docker Swarm hello använda hello `docker run` kommando (se till att du har öppnat en SSH-tunnel toohello huvudservrar enligt hello kraven ovan).</span><span class="sxs-lookup"><span data-stu-id="fb79e-114">toocreate a new container in hello Docker Swarm, use hello `docker run` command (ensuring that you have opened an SSH tunnel toohello masters as per hello prerequisites above).</span></span> <span data-ttu-id="fb79e-115">Det här exemplet skapar en behållare från hello `yeasy/simple-web` avbildningen:</span><span class="sxs-lookup"><span data-stu-id="fb79e-115">This example creates a container from hello `yeasy/simple-web` image:</span></span>

```bash
user@ubuntu:~$ docker run -d -p 80:80 yeasy/simple-web

4298d397b9ab6f37e2d1978ef3c8c1537c938e98a8bf096ff00def2eab04bf72
```

<span data-ttu-id="fb79e-116">När hello behållaren har skapats kan du använda `docker ps` tooreturn information om hello behållare.</span><span class="sxs-lookup"><span data-stu-id="fb79e-116">After hello container has been created, use `docker ps` tooreturn information about hello container.</span></span> <span data-ttu-id="fb79e-117">Observera att hello Swarm-agenten som är värd för hello behållaren anges:</span><span class="sxs-lookup"><span data-stu-id="fb79e-117">Notice here that hello Swarm agent that is hosting hello container is listed:</span></span>

```bash
user@ubuntu:~$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
4298d397b9ab        yeasy/simple-web    "/bin/sh -c 'python i"   31 seconds ago      Up 9 seconds        10.0.0.5:80->80/tcp   swarm-agent-34A73819-1/happy_allen
```  

<span data-ttu-id="fb79e-118">Du kan nu komma åt hello-program som körs i den här behållaren via hello offentliga DNS-namnet på hello Swarm-agentens belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="fb79e-118">You can now access hello application that is running in this container through hello public DNS name of hello Swarm agent load balancer.</span></span> <span data-ttu-id="fb79e-119">Du hittar den här informationen i hello Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="fb79e-119">You can find this information in hello Azure portal:</span></span>  

![Verklighetstrogna besöksresultat](./media/container-service-docker-swarm/real-visit.jpg)  

<span data-ttu-id="fb79e-121">Hello belastningsutjämnaren har som standard portarna 80, 8080 och 443 öppen.</span><span class="sxs-lookup"><span data-stu-id="fb79e-121">By default hello Load Balancer has ports 80, 8080 and 443 open.</span></span> <span data-ttu-id="fb79e-122">Om du vill tooconnect på en annan port måste tooopen porten på hello Azure belastningsutjämnare för hello Agent poolen.</span><span class="sxs-lookup"><span data-stu-id="fb79e-122">If you want tooconnect on another port you will need tooopen that port on hello Azure Load Balancer for hello Agent Pool.</span></span>

## <a name="deploy-multiple-containers"></a><span data-ttu-id="fb79e-123">Distribuera flera behållare</span><span class="sxs-lookup"><span data-stu-id="fb79e-123">Deploy multiple containers</span></span>
<span data-ttu-id="fb79e-124">Som flera behållare har startats genom att köra 'docker kör' flera gånger, kan du använda hello `docker ps` kommandot toosee som är värd för hello behållarna körs på.</span><span class="sxs-lookup"><span data-stu-id="fb79e-124">As multiple containers are started, by executing 'docker run' multiple times, you can use hello `docker ps` command toosee which hosts hello containers are running on.</span></span> <span data-ttu-id="fb79e-125">I hello exemplet nedan jämnt tre behållare fördelade över hello tre Swarm-agenterna:</span><span class="sxs-lookup"><span data-stu-id="fb79e-125">In hello example below, three containers are spread evenly across hello three Swarm agents:</span></span>  

```bash
user@ubuntu:~$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
11be062ff602        yeasy/simple-web    "/bin/sh -c 'python i"   11 seconds ago      Up 10 seconds       10.0.0.6:83->80/tcp   swarm-agent-34A73819-2/clever_banach
1ff421554c50        yeasy/simple-web    "/bin/sh -c 'python i"   49 seconds ago      Up 48 seconds       10.0.0.4:82->80/tcp   swarm-agent-34A73819-0/stupefied_ride
4298d397b9ab        yeasy/simple-web    "/bin/sh -c 'python i"   2 minutes ago       Up 2 minutes        10.0.0.5:80->80/tcp   swarm-agent-34A73819-1/happy_allen
```  

## <a name="deploy-containers-by-using-docker-compose"></a><span data-ttu-id="fb79e-126">Distribuera behållare med Docker Compose</span><span class="sxs-lookup"><span data-stu-id="fb79e-126">Deploy containers by using Docker Compose</span></span>
<span data-ttu-id="fb79e-127">Du kan använda Docker Compose tooautomate hello distributionen och konfigurationen av flera behållare.</span><span class="sxs-lookup"><span data-stu-id="fb79e-127">You can use Docker Compose tooautomate hello deployment and configuration of multiple containers.</span></span> <span data-ttu-id="fb79e-128">toodo Kontrollera så att en tunnel SSH (Secure Shell) har skapats och hello variabeln DOCKER_HOST har angetts (se hello förutsättningar ovan).</span><span class="sxs-lookup"><span data-stu-id="fb79e-128">toodo so, ensure that a Secure Shell (SSH) tunnel has been created and that hello DOCKER_HOST variable has been set (see hello pre-requisites above).</span></span>

<span data-ttu-id="fb79e-129">Skapa filen docker-compose.yml på ditt lokala system.</span><span class="sxs-lookup"><span data-stu-id="fb79e-129">Create a docker-compose.yml file on your local system.</span></span> <span data-ttu-id="fb79e-130">toodo, Använd den här [exempel](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/docker-compose.yml).</span><span class="sxs-lookup"><span data-stu-id="fb79e-130">toodo this, use this [sample](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/docker-compose.yml).</span></span>

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

<span data-ttu-id="fb79e-131">Kör `docker-compose up -d` toostart hello behållardistributionerna:</span><span class="sxs-lookup"><span data-stu-id="fb79e-131">Run `docker-compose up -d` toostart hello container deployments:</span></span>

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

<span data-ttu-id="fb79e-132">Slutligen returneras hello listan över behållare som körs.</span><span class="sxs-lookup"><span data-stu-id="fb79e-132">Finally, hello list of running containers will be returned.</span></span> <span data-ttu-id="fb79e-133">Den här listan visar hello-behållare som distribuerades med Docker Compose:</span><span class="sxs-lookup"><span data-stu-id="fb79e-133">This list reflects hello containers that were deployed by using Docker Compose:</span></span>

```bash
user@ubuntu:~/compose$ docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                     NAMES
caf185d221b7        adtd/web:0.1        "apache2-foreground"   2 minutes ago       Up About a minute   10.0.0.4:80->80/tcp       swarm-agent-3B7093B8-0/compose_web_1
040efc0ea937        adtd/rest:0.1       "catalina.sh run"      3 minutes ago       Up 2 minutes        10.0.0.4:8080->8080/tcp   swarm-agent-3B7093B8-0/compose_rest_1
```

<span data-ttu-id="fb79e-134">Naturligtvis finns kan du använda `docker-compose ps` tooexamine endast hello behållare som definierats i din `compose.yml` fil.</span><span class="sxs-lookup"><span data-stu-id="fb79e-134">Naturally, you can use `docker-compose ps` tooexamine only hello containers defined in your `compose.yml` file.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fb79e-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fb79e-135">Next steps</span></span>
[<span data-ttu-id="fb79e-136">Mer information om Docker Swarm</span><span class="sxs-lookup"><span data-stu-id="fb79e-136">Learn more about Docker Swarm</span></span>](https://docs.docker.com/swarm/)

