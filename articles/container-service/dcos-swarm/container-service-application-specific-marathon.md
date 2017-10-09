---
title: "aaaApplication eller användarspecifik Marathon-tjänst | Microsoft Docs"
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
ms.openlocfilehash: 1e6f69ed64e113a3a059788a71ddb57b6d3ad8da
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-or-user-specific-marathon-service"></a><span data-ttu-id="55bc7-104">Skapa en program- eller användarspecifik Marathon-tjänst</span><span class="sxs-lookup"><span data-stu-id="55bc7-104">Create an application or user-specific Marathon service</span></span>
<span data-ttu-id="55bc7-105">Genom Azure Container Service tillhandahålls en uppsättning huvudservrar där Apache Mesos och Marathon förkonfigureras.</span><span class="sxs-lookup"><span data-stu-id="55bc7-105">Azure Container Service provides a set of master servers on which we preconfigure Apache Mesos and Marathon.</span></span> <span data-ttu-id="55bc7-106">Det kan vara används tooorchestrate dina program på hello kluster, men det är bästa inte toouse hello huvudservrarna för det här ändamålet.</span><span class="sxs-lookup"><span data-stu-id="55bc7-106">These can be used tooorchestrate your applications on hello cluster, but it's best not toouse hello master servers for this purpose.</span></span> <span data-ttu-id="55bc7-107">Till exempel modifiera hello konfigurationen av Marathon kräver logga in på hello huvudservrar sig själva och göra ändringar--detta uppmanar unika huvudservrar som är lite annorlunda från hello som standard och måste toobe få och hanterade oberoende av varandra.</span><span class="sxs-lookup"><span data-stu-id="55bc7-107">For example, tweaking hello configuration of Marathon requires logging into hello master servers themselves and making changes--this encourages unique master servers that are a little different from hello standard and need toobe cared for and managed independently.</span></span> <span data-ttu-id="55bc7-108">Dessutom kanske inte hello-konfiguration som krävs av ett team hello bästa konfigurationen för ett annat team.</span><span class="sxs-lookup"><span data-stu-id="55bc7-108">Additionally, hello configuration required by one team might not be hello optimal configuration for another team.</span></span>

<span data-ttu-id="55bc7-109">I den här artikeln förklarar vi hur tooadd en program- eller användarspecifik Marathon-tjänst.</span><span class="sxs-lookup"><span data-stu-id="55bc7-109">In this article, we'll explain how tooadd an application or user-specific Marathon service.</span></span>

<span data-ttu-id="55bc7-110">Eftersom den här tjänsten kommer att tillhöra tooa användare eller grupp, de är kostnadsfria tooconfigure den på något sätt som de önskar.</span><span class="sxs-lookup"><span data-stu-id="55bc7-110">Because this service will belong tooa single user or team, they are free tooconfigure it in any way that they desire.</span></span> <span data-ttu-id="55bc7-111">Dessutom säkerställer Azure Container Service att hello tjänsten fortsätter toorun.</span><span class="sxs-lookup"><span data-stu-id="55bc7-111">Also, Azure Container Service will ensure that hello service continues toorun.</span></span> <span data-ttu-id="55bc7-112">Om det inte går att hello-tjänsten, Azure Container Service startar om den åt dig.</span><span class="sxs-lookup"><span data-stu-id="55bc7-112">If hello service fails, Azure Container Service will restart it for you.</span></span> <span data-ttu-id="55bc7-113">De flesta hello tid märker du inte ens det varit driftstopp.</span><span class="sxs-lookup"><span data-stu-id="55bc7-113">Most of hello time you won't even notice it had downtime.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="55bc7-114">Krav</span><span class="sxs-lookup"><span data-stu-id="55bc7-114">Prerequisites</span></span>
<span data-ttu-id="55bc7-115">[Distribuera en instans av Azure Container Service](container-service-deployment.md) orchestrator Skriv DC/OS och [se till att klienten kan ansluta tooyour klustret](../container-service-connect.md).</span><span class="sxs-lookup"><span data-stu-id="55bc7-115">[Deploy an instance of Azure Container Service](container-service-deployment.md) with orchestrator type DC/OS and  [ensure that your client can connect tooyour cluster](../container-service-connect.md).</span></span> <span data-ttu-id="55bc7-116">Dessutom hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="55bc7-116">Also, do hello following steps.</span></span>

[!INCLUDE [install hello DC/OS CLI](../../../includes/container-service-install-dcos-cli-include.md)]

## <a name="create-an-application-or-user-specific-marathon-service"></a><span data-ttu-id="55bc7-117">Skapa en program- eller användarspecifik Marathon-tjänst</span><span class="sxs-lookup"><span data-stu-id="55bc7-117">Create an application or user-specific Marathon service</span></span>
<span data-ttu-id="55bc7-118">Börja med att skapa en JSON-konfigurationsfil som definierar hello programtjänsten som du vill toocreate hello namn.</span><span class="sxs-lookup"><span data-stu-id="55bc7-118">Begin by creating a JSON configuration file that defines hello name of hello application service that you want toocreate.</span></span> <span data-ttu-id="55bc7-119">Vi använder här `marathon-alice` som hello framework namn.</span><span class="sxs-lookup"><span data-stu-id="55bc7-119">Here we use `marathon-alice` as hello framework name.</span></span> <span data-ttu-id="55bc7-120">Spara hello-fil med något som liknar `marathon-alice.json`:</span><span class="sxs-lookup"><span data-stu-id="55bc7-120">Save hello file as something like `marathon-alice.json`:</span></span>

```json
{"marathon": {"framework-name": "marathon-alice" }}
```

<span data-ttu-id="55bc7-121">Använd sedan hello DC/OS CLI tooinstall hello Marathon-instansen med hello-alternativ som har angetts i konfigurationsfilen:</span><span class="sxs-lookup"><span data-stu-id="55bc7-121">Next, use hello DC/OS CLI tooinstall hello Marathon instance with hello options that are set in your configuration file:</span></span>

```bash
dcos package install --options=marathon-alice.json marathon
```

<span data-ttu-id="55bc7-122">Du bör nu se ditt `marathon-alice` tjänsten som körs i hello tjänstefliken i DC/OS-Gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="55bc7-122">You should now see your `marathon-alice` service running in hello Services tab of your DC/OS UI.</span></span> <span data-ttu-id="55bc7-123">hello Gränssnittet är `http://<hostname>/service/marathon-alice/` om du vill tooaccess den direkt.</span><span class="sxs-lookup"><span data-stu-id="55bc7-123">hello UI will be `http://<hostname>/service/marathon-alice/` if you want tooaccess it directly.</span></span>

## <a name="set-hello-dcos-cli-tooaccess-hello-service"></a><span data-ttu-id="55bc7-124">Ange hello DC/OS CLI tooaccess hello-tjänsten</span><span class="sxs-lookup"><span data-stu-id="55bc7-124">Set hello DC/OS CLI tooaccess hello service</span></span>
<span data-ttu-id="55bc7-125">Du kan också konfigurera den här tjänsten för DC/OS CLI-tooaccess genom att ange hello `marathon.url` egenskapen toopoint toohello `marathon-alice` instans enligt följande:</span><span class="sxs-lookup"><span data-stu-id="55bc7-125">You can optionally configure your DC/OS CLI tooaccess this new service by setting hello `marathon.url` property toopoint toohello `marathon-alice` instance as follows:</span></span>

```bash
dcos config set marathon.url http://<hostname>/service/marathon-alice/
```

<span data-ttu-id="55bc7-126">Du kan kontrollera vilken instans av Marathon som CLI fungerar med hello `dcos config show` kommando.</span><span class="sxs-lookup"><span data-stu-id="55bc7-126">You can verify which instance of Marathon that your CLI is working against with hello `dcos config show` command.</span></span> <span data-ttu-id="55bc7-127">Du kan återställa toousing Marathon-huvudtjänsten med kommandot hello `dcos config unset marathon.url`.</span><span class="sxs-lookup"><span data-stu-id="55bc7-127">You can revert toousing your master Marathon service with hello command `dcos config unset marathon.url`.</span></span>

