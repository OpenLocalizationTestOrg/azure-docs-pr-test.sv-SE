---
title: "Program- eller användarspecifik Marathon-tjänst | Microsoft Docs"
description: "Skapa en program- eller användarspecifik Marathon-tjänst"
services: container-service
documentationcenter: 
author: rgardler
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Behållare, Marathon, Micro-tjänster, DC/OS, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/12/2016
ms.author: rogardle
ms.custom: mvc
ms.openlocfilehash: b265763fb5dad240edd710cd8d0fb1079e3a7b51
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="create-an-application-or-user-specific-marathon-service"></a><span data-ttu-id="cbeb2-104">Skapa en program- eller användarspecifik Marathon-tjänst</span><span class="sxs-lookup"><span data-stu-id="cbeb2-104">Create an application or user-specific Marathon service</span></span>
<span data-ttu-id="cbeb2-105">Genom Azure Container Service tillhandahålls en uppsättning huvudservrar där Apache Mesos och Marathon förkonfigureras.</span><span class="sxs-lookup"><span data-stu-id="cbeb2-105">Azure Container Service provides a set of master servers on which we preconfigure Apache Mesos and Marathon.</span></span> <span data-ttu-id="cbeb2-106">Servrarna kan användas för att dirigera dina program i klustret, men det är bäst att inte använda huvudservrarna för det här ändamålet.</span><span class="sxs-lookup"><span data-stu-id="cbeb2-106">These can be used to orchestrate your applications on the cluster, but it's best not to use the master servers for this purpose.</span></span> <span data-ttu-id="cbeb2-107">Om du till exempel vill justera konfigurationen av Marathon måste du logga in på själva huvudservrarna och göra ändringar. Det leder lätt till att du får unika huvudservrar som skiljer sig lite från standarden, vilket betyder att de måste skötas och hanteras var för sig.</span><span class="sxs-lookup"><span data-stu-id="cbeb2-107">For example, tweaking the configuration of Marathon requires logging into the master servers themselves and making changes--this encourages unique master servers that are a little different from the standard and need to be cared for and managed independently.</span></span> <span data-ttu-id="cbeb2-108">Dessutom kanske konfigurationen som krävs av ett team inte är den bästa konfigurationen för ett annat team.</span><span class="sxs-lookup"><span data-stu-id="cbeb2-108">Additionally, the configuration required by one team might not be the optimal configuration for another team.</span></span>

<span data-ttu-id="cbeb2-109">I den här artikeln förklarar vi hur du lägger till en användar- eller programspecifik Marathon-tjänst.</span><span class="sxs-lookup"><span data-stu-id="cbeb2-109">In this article, we'll explain how to add an application or user-specific Marathon service.</span></span>

<span data-ttu-id="cbeb2-110">Eftersom den här tjänsten tillhör en enskild användare eller ett enskilt team kan den konfigureras på önskat sätt.</span><span class="sxs-lookup"><span data-stu-id="cbeb2-110">Because this service will belong to a single user or team, they are free to configure it in any way that they desire.</span></span> <span data-ttu-id="cbeb2-111">Azure Container Service säkerställer också att tjänsten fortsätter att köras.</span><span class="sxs-lookup"><span data-stu-id="cbeb2-111">Also, Azure Container Service will ensure that the service continues to run.</span></span> <span data-ttu-id="cbeb2-112">Om tjänsten slutar att fungera, startar Azure Container Service om den åt dig.</span><span class="sxs-lookup"><span data-stu-id="cbeb2-112">If the service fails, Azure Container Service will restart it for you.</span></span> <span data-ttu-id="cbeb2-113">I de flesta fall märker du inte ens att det varit driftstopp.</span><span class="sxs-lookup"><span data-stu-id="cbeb2-113">Most of the time you won't even notice it had downtime.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cbeb2-114">Krav</span><span class="sxs-lookup"><span data-stu-id="cbeb2-114">Prerequisites</span></span>
<span data-ttu-id="cbeb2-115">[Distribuera en instans av Azure Container Service](container-service-deployment.md) med orchestrator-typ DC/OS och [kontrollera att klienten kan ansluta till klustret](../container-service-connect.md).</span><span class="sxs-lookup"><span data-stu-id="cbeb2-115">[Deploy an instance of Azure Container Service](container-service-deployment.md) with orchestrator type DC/OS and  [ensure that your client can connect to your cluster](../container-service-connect.md).</span></span> <span data-ttu-id="cbeb2-116">Utför följande steg.</span><span class="sxs-lookup"><span data-stu-id="cbeb2-116">Also, do the following steps.</span></span>

[!INCLUDE [install the DC/OS CLI](../../../includes/container-service-install-dcos-cli-include.md)]

## <a name="create-an-application-or-user-specific-marathon-service"></a><span data-ttu-id="cbeb2-117">Skapa en program- eller användarspecifik Marathon-tjänst</span><span class="sxs-lookup"><span data-stu-id="cbeb2-117">Create an application or user-specific Marathon service</span></span>
<span data-ttu-id="cbeb2-118">Börja med att skapa en JSON-konfigurationsfil som definierar namnet på den programtjänst du vill skapa.</span><span class="sxs-lookup"><span data-stu-id="cbeb2-118">Begin by creating a JSON configuration file that defines the name of the application service that you want to create.</span></span> <span data-ttu-id="cbeb2-119">I det här exemplet använder vi `marathon-alice` som ramverksnamn.</span><span class="sxs-lookup"><span data-stu-id="cbeb2-119">Here we use `marathon-alice` as the framework name.</span></span> <span data-ttu-id="cbeb2-120">Spara filen med något som liknar `marathon-alice.json`:</span><span class="sxs-lookup"><span data-stu-id="cbeb2-120">Save the file as something like `marathon-alice.json`:</span></span>

```json
{"marathon": {"framework-name": "marathon-alice" }}
```

<span data-ttu-id="cbeb2-121">Använd sedan DC/OS CLI för att installera Marathon-instansen med alternativen som anges i konfigurationsfilen:</span><span class="sxs-lookup"><span data-stu-id="cbeb2-121">Next, use the DC/OS CLI to install the Marathon instance with the options that are set in your configuration file:</span></span>

```bash
dcos package install --options=marathon-alice.json marathon
```

<span data-ttu-id="cbeb2-122">Nu bör du kunna se att tjänsten `marathon-alice` körs på tjänstefliken i DC/OS-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="cbeb2-122">You should now see your `marathon-alice` service running in the Services tab of your DC/OS UI.</span></span> <span data-ttu-id="cbeb2-123">Gränssnittet är `http://<hostname>/service/marathon-alice/` om du vill komma åt det direkt.</span><span class="sxs-lookup"><span data-stu-id="cbeb2-123">The UI will be `http://<hostname>/service/marathon-alice/` if you want to access it directly.</span></span>

## <a name="set-the-dcos-cli-to-access-the-service"></a><span data-ttu-id="cbeb2-124">Ställa in DC/OS CLI för att komma åt tjänsten</span><span class="sxs-lookup"><span data-stu-id="cbeb2-124">Set the DC/OS CLI to access the service</span></span>
<span data-ttu-id="cbeb2-125">Du kan även konfigurera DC/OS CLI för att komma åt den här nya tjänsten genom att ange att egenskapen `marathon.url` ska peka på instansen `marathon-alice` enligt följande:</span><span class="sxs-lookup"><span data-stu-id="cbeb2-125">You can optionally configure your DC/OS CLI to access this new service by setting the `marathon.url` property to point to the `marathon-alice` instance as follows:</span></span>

```bash
dcos config set marathon.url http://<hostname>/service/marathon-alice/
```

<span data-ttu-id="cbeb2-126">Du kan kontrollera vilken instans av Marathon som din CLI arbetar mot med `dcos config show`-kommandot.</span><span class="sxs-lookup"><span data-stu-id="cbeb2-126">You can verify which instance of Marathon that your CLI is working against with the `dcos config show` command.</span></span> <span data-ttu-id="cbeb2-127">Du kan återgå till att använda din huvudsakliga Marathon-tjänst med kommandot `dcos config unset marathon.url`.</span><span class="sxs-lookup"><span data-stu-id="cbeb2-127">You can revert to using your master Marathon service with the command `dcos config unset marathon.url`.</span></span>

