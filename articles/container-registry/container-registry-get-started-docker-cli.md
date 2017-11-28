---
title: aaaPush Docker bild tooprivate Azure registret | Microsoft Docs
description: "Push och pull Docker-avbildningar tooa privata behållare registret i Azure med hjälp av hello Docker CLI"
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: cristyg
tags: 
keywords: 
ms.assetid: 64fbe43f-fdde-4c17-a39a-d04f2d6d90a1
ms.service: container-registry
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a81a6f4bfcb23642a89ac7631348d40e2f4911a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="push-your-first-image-tooa-private-docker-container-registry-using-hello-docker-cli"></a><span data-ttu-id="ed1fe-103">Push din första bilden tooa privata Docker behållare registret med hjälp av hello Docker CLI</span><span class="sxs-lookup"><span data-stu-id="ed1fe-103">Push your first image tooa private Docker container registry using hello Docker CLI</span></span>
<span data-ttu-id="ed1fe-104">En Azure-behållaren registret lagrar och hanterar privata [Docker](http://hub.docker.com) behållare bilder, liknande toohello sätt [Docker-hubb](https://hub.docker.com/) lagrar offentliga Docker-avbildningar.</span><span class="sxs-lookup"><span data-stu-id="ed1fe-104">An Azure container registry stores and manages private [Docker](http://hub.docker.com) container images, similar toohello way [Docker Hub](https://hub.docker.com/) stores public Docker images.</span></span> <span data-ttu-id="ed1fe-105">Du använder hello [Docker-kommandoradsgränssnittet](https://docs.docker.com/engine/reference/commandline/cli/) (Docker CLI) för [inloggning](https://docs.docker.com/engine/reference/commandline/login/), [push](https://docs.docker.com/engine/reference/commandline/push/), [pull](https://docs.docker.com/engine/reference/commandline/pull/), och andra åtgärder på din behållare registret.</span><span class="sxs-lookup"><span data-stu-id="ed1fe-105">You use hello [Docker Command-Line Interface](https://docs.docker.com/engine/reference/commandline/cli/) (Docker CLI) for [login](https://docs.docker.com/engine/reference/commandline/login/), [push](https://docs.docker.com/engine/reference/commandline/push/), [pull](https://docs.docker.com/engine/reference/commandline/pull/), and other operations on your container registry.</span></span>

<span data-ttu-id="ed1fe-106">Mer bakgrund och begrepp finns [hello översikt](container-registry-intro.md)</span><span class="sxs-lookup"><span data-stu-id="ed1fe-106">For more background and concepts, see [hello overview](container-registry-intro.md)</span></span>



## <a name="prerequisites"></a><span data-ttu-id="ed1fe-107">Krav</span><span class="sxs-lookup"><span data-stu-id="ed1fe-107">Prerequisites</span></span>
* <span data-ttu-id="ed1fe-108">**Azure-behållarregister** – Skapa ett behållarregister i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="ed1fe-108">**Azure container registry** - Create a container registry in your Azure subscription.</span></span> <span data-ttu-id="ed1fe-109">Till exempel använda hello [Azure-portalen](container-registry-get-started-portal.md) eller hello [Azure CLI 2.0](container-registry-get-started-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="ed1fe-109">For example, use hello [Azure portal](container-registry-get-started-portal.md) or hello [Azure CLI 2.0](container-registry-get-started-azure-cli.md).</span></span>
* <span data-ttu-id="ed1fe-110">**Docker CLI** -tooset din lokala dator som en Docker-värden och åtkomst hello Docker CLI-kommandon, installera [Docker-motorn](https://docs.docker.com/engine/installation/).</span><span class="sxs-lookup"><span data-stu-id="ed1fe-110">**Docker CLI** - tooset up your local computer as a Docker host and access hello Docker CLI commands, install [Docker Engine](https://docs.docker.com/engine/installation/).</span></span>

## <a name="log-in-tooa-registry"></a><span data-ttu-id="ed1fe-111">Logga in tooa registret</span><span class="sxs-lookup"><span data-stu-id="ed1fe-111">Log in tooa registry</span></span>
<span data-ttu-id="ed1fe-112">Kör `docker login` toolog i tooyour behållare registret med din [registret autentiseringsuppgifter](container-registry-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="ed1fe-112">Run `docker login` toolog in tooyour container registry with your [registry credentials](container-registry-authentication.md).</span></span>

<span data-ttu-id="ed1fe-113">hello följande exempel skickar hello-ID och lösenord för ett Azure Active Directory [tjänstens huvudnamn](../active-directory/active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="ed1fe-113">hello following example passes hello ID and password of an Azure Active Directory [service principal](../active-directory/active-directory-application-objects.md).</span></span> <span data-ttu-id="ed1fe-114">Du kan till exempel har tilldelat en service principal tooyour registret för ett automation-scenario.</span><span class="sxs-lookup"><span data-stu-id="ed1fe-114">For example, you might have assigned a service principal tooyour registry for an automation scenario.</span></span>

```
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

> [!TIP]
> <span data-ttu-id="ed1fe-115">Kontrollera att toospecify hello registret fullständigt kvalificerade namn (gemener).</span><span class="sxs-lookup"><span data-stu-id="ed1fe-115">Make sure toospecify hello fully qualified registry name (all lowercase).</span></span> <span data-ttu-id="ed1fe-116">I det här exemplet är det `myregistry.azurecr.io`.</span><span class="sxs-lookup"><span data-stu-id="ed1fe-116">In this example, it is `myregistry.azurecr.io`.</span></span>

## <a name="steps-toopull-and-push-an-image"></a><span data-ttu-id="ed1fe-117">Steg toopull och push en avbildning</span><span class="sxs-lookup"><span data-stu-id="ed1fe-117">Steps toopull and push an image</span></span>
<span data-ttu-id="ed1fe-118">hello följer exempel hämtningar hello Nginx-avbildning från hello offentliga Docker hubb registret taggar för privat Azure-behållaren registret, skickar den tooyour registret, därefter hämtar den igen.</span><span class="sxs-lookup"><span data-stu-id="ed1fe-118">hello follow example downloads hello Nginx image from hello public Docker Hub registry, tags it for your private Azure container registry, pushes it tooyour registry, then pulls it again.</span></span>

<span data-ttu-id="ed1fe-119">**1. Hämta hello Docker officiella bild för Nginx**</span><span class="sxs-lookup"><span data-stu-id="ed1fe-119">**1. Pull hello Docker official image for Nginx**</span></span>

<span data-ttu-id="ed1fe-120">Första pull hello offentliga Nginx bild tooyour lokal dator.</span><span class="sxs-lookup"><span data-stu-id="ed1fe-120">First pull hello public Nginx image tooyour local computer.</span></span>

```
docker pull nginx
```
<span data-ttu-id="ed1fe-121">**2. Starta hello Nginx-behållaren**</span><span class="sxs-lookup"><span data-stu-id="ed1fe-121">**2. Start hello Nginx container**</span></span>

<span data-ttu-id="ed1fe-122">hello följande kommando startar hello lokala Nginx-behållaren interaktivt på port 8080, så att du toosee utdata från Nginx.</span><span class="sxs-lookup"><span data-stu-id="ed1fe-122">hello following command starts hello local Nginx container interactively on port 8080, allowing you toosee output from Nginx.</span></span> <span data-ttu-id="ed1fe-123">Det tar bort hello kör behållaren en gång stoppades.</span><span class="sxs-lookup"><span data-stu-id="ed1fe-123">It removes hello running container once stopped.</span></span>

```
docker run -it --rm -p 8080:80 nginx
```

<span data-ttu-id="ed1fe-124">Bläddra för[http://localhost: 8080](http://localhost:8080) tooview hello kör behållare.</span><span class="sxs-lookup"><span data-stu-id="ed1fe-124">Browse too[http://localhost:8080](http://localhost:8080) tooview hello running container.</span></span> <span data-ttu-id="ed1fe-125">Du kan se en skärm liknande toohello efter.</span><span class="sxs-lookup"><span data-stu-id="ed1fe-125">You see a screen similar toohello following one.</span></span>

![Nginx på lokal dator](./media/container-registry-get-started-docker-cli/nginx.png)

<span data-ttu-id="ed1fe-127">toostop hello körs behållare genom att trycka på [CTRL] + [C].</span><span class="sxs-lookup"><span data-stu-id="ed1fe-127">toostop hello running container, press [CTRL]+[C].</span></span>

<span data-ttu-id="ed1fe-128">**3. Skapa ett alias till hello bild i registret**</span><span class="sxs-lookup"><span data-stu-id="ed1fe-128">**3. Create an alias of hello image in your registry**</span></span>

<span data-ttu-id="ed1fe-129">hello följande kommando skapar ett alias till hello bild med ett fullständigt kvalificerad sökväg tooyour register.</span><span class="sxs-lookup"><span data-stu-id="ed1fe-129">hello following command creates an alias of hello image, with a fully qualified path tooyour registry.</span></span> <span data-ttu-id="ed1fe-130">Det här exemplet anger hello `samples` namnområde tooavoid oreda i hello rot hello registret.</span><span class="sxs-lookup"><span data-stu-id="ed1fe-130">This example specifies hello `samples` namespace tooavoid clutter in hello root of hello registry.</span></span>

```
docker tag nginx myregistry.azurecr.io/samples/nginx
```  

<span data-ttu-id="ed1fe-131">**4. Push hello avbildningen tooyour registret**</span><span class="sxs-lookup"><span data-stu-id="ed1fe-131">**4. Push hello image tooyour registry**</span></span>

```
docker push myregistry.azurecr.io/samples/nginx
```

<span data-ttu-id="ed1fe-132">**5. Pull hello-avbildning från registret**</span><span class="sxs-lookup"><span data-stu-id="ed1fe-132">**5. Pull hello image from your registry**</span></span>

```
docker pull myregistry.azurecr.io/samples/nginx
```

<span data-ttu-id="ed1fe-133">**6. Starta hello Nginx-behållaren från registret**</span><span class="sxs-lookup"><span data-stu-id="ed1fe-133">**6. Start hello Nginx container from your registry**</span></span>

```
docker run -it --rm -p 8080:80 myregistry.azurecr.io/samples/nginx
```

<span data-ttu-id="ed1fe-134">Bläddra för[http://localhost: 8080](http://localhost:8080) tooview hello kör behållare.</span><span class="sxs-lookup"><span data-stu-id="ed1fe-134">Browse too[http://localhost:8080](http://localhost:8080) tooview hello running container.</span></span>

<span data-ttu-id="ed1fe-135">toostop hello körs behållare genom att trycka på [CTRL] + [C].</span><span class="sxs-lookup"><span data-stu-id="ed1fe-135">toostop hello running container, press [CTRL]+[C].</span></span>

<span data-ttu-id="ed1fe-136">**7. (Valfritt) Ta bort hello-avbildningar**</span><span class="sxs-lookup"><span data-stu-id="ed1fe-136">**7. (Optional) Remove hello image**</span></span>

```
docker rmi myregistry.azurecr.io/samples/nginx
```

##<a name="concurrent-limits"></a><span data-ttu-id="ed1fe-137">Gränser för samtidiga anrop</span><span class="sxs-lookup"><span data-stu-id="ed1fe-137">Concurrent Limits</span></span>
<span data-ttu-id="ed1fe-138">I vissa situationer kan anrop som körs samtidigt resultera i fel.</span><span class="sxs-lookup"><span data-stu-id="ed1fe-138">In some scenarios, executing calls concurrently might result in errors.</span></span> <span data-ttu-id="ed1fe-139">hello följande tabell innehåller hello gränserna för samtidiga anrop med ”Push” och ”Pull” åtgärder på Azure-behållaren registret:</span><span class="sxs-lookup"><span data-stu-id="ed1fe-139">hello following table contains hello limits of concurrent calls with "Push" and "Pull" operations on Azure container registry:</span></span>

| <span data-ttu-id="ed1fe-140">Åtgärd</span><span class="sxs-lookup"><span data-stu-id="ed1fe-140">Operation</span></span>  | <span data-ttu-id="ed1fe-141">Gräns</span><span class="sxs-lookup"><span data-stu-id="ed1fe-141">Limit</span></span>                                  |
| ---------- | -------------------------------------- |
| <span data-ttu-id="ed1fe-142">PULL</span><span class="sxs-lookup"><span data-stu-id="ed1fe-142">PULL</span></span>       | <span data-ttu-id="ed1fe-143">Konfigurera samtidiga too10 hämtar per registret</span><span class="sxs-lookup"><span data-stu-id="ed1fe-143">Up too10 concurrent pulls per registry</span></span> |
| <span data-ttu-id="ed1fe-144">PUSH</span><span class="sxs-lookup"><span data-stu-id="ed1fe-144">PUSH</span></span>       | <span data-ttu-id="ed1fe-145">Konfigurera too5 samtidiga push-meddelanden per registret</span><span class="sxs-lookup"><span data-stu-id="ed1fe-145">Up too5 concurrent pushes per registry</span></span> |

## <a name="next-steps"></a><span data-ttu-id="ed1fe-146">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ed1fe-146">Next steps</span></span>
<span data-ttu-id="ed1fe-147">Nu när du vet hello grunderna är klar toostart med hjälp av registret!</span><span class="sxs-lookup"><span data-stu-id="ed1fe-147">Now that you know hello basics, you are ready toostart using your registry!</span></span> <span data-ttu-id="ed1fe-148">Till exempel börja distribuera behållaren bilder tooan [Azure Container Service](https://azure.microsoft.com/documentation/services/container-service/) klustret.</span><span class="sxs-lookup"><span data-stu-id="ed1fe-148">For example, start deploying container images tooan [Azure Container Service](https://azure.microsoft.com/documentation/services/container-service/) cluster.</span></span>
