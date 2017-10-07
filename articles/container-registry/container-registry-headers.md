---
title: "aaaAzure behållaren registret databaser | Microsoft Docs"
description: "Hur toouse Azure Container registret databaser för Docker bilder"
services: container-registry
documentationcenter: 
author: cristy
manager: balans
editor: dlepow
ms.service: container-registry
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/22/2017
ms.author: cristyg
ms.openlocfilehash: 06172a63465838a78a607f268da116d8158789ee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-container-registry-repositories"></a><span data-ttu-id="fcf39-103">Azure-behållaren registret databaser</span><span class="sxs-lookup"><span data-stu-id="fcf39-103">Azure container registry repositories</span></span>

<span data-ttu-id="fcf39-104">Azure Container register är kompatibla med en mängd olika tjänster och orchestrators.</span><span class="sxs-lookup"><span data-stu-id="fcf39-104">Azure Container Registries are compatible with a multitude of services and orchestrators.</span></span> <span data-ttu-id="fcf39-105">toomake den enklare tootrack hello källa tjänster och agenter som ACR används, har vi igång med hello Docker huvudfältet i hello Docker.config fil.</span><span class="sxs-lookup"><span data-stu-id="fcf39-105">toomake it easier tootrack hello source services and agents from which ACR is used, we have started using hello Docker header field in hello Docker.config file.</span></span>



## <a name="viewing-repositories-in-hello-portal"></a><span data-ttu-id="fcf39-106">Visa databaser i hello Portal</span><span class="sxs-lookup"><span data-stu-id="fcf39-106">Viewing repositories in hello Portal</span></span>

<span data-ttu-id="fcf39-107">Hej ACR huvuden följer hello format:</span><span class="sxs-lookup"><span data-stu-id="fcf39-107">hello ACR headers follow hello format:</span></span>
```
X-Meta-Source-Client: <cloud>/<service>/<optionalservicename>
```

* <span data-ttu-id="fcf39-108">Molnet: Azure, Azure Stack eller andra myndigheter eller landsspecifika Azure-moln.</span><span class="sxs-lookup"><span data-stu-id="fcf39-108">Cloud: Azure, Azure Stack, or other government or country-specific Azure clouds.</span></span> <span data-ttu-id="fcf39-109">Även om Azure-stacken och offentliga moln inte stöds för närvarande, kan den här parametern framtida stöd.</span><span class="sxs-lookup"><span data-stu-id="fcf39-109">Although Azure Stack and government clouds are not currently supported, this parameter enables future support.</span></span>
* <span data-ttu-id="fcf39-110">Tjänst: namnet på hello-tjänst.</span><span class="sxs-lookup"><span data-stu-id="fcf39-110">Service: name of hello service.</span></span>
* <span data-ttu-id="fcf39-111">Optionalservicename: valfri parameter för tjänster med subservices eller toospecify en SKU (ex: webbappar motsvarar Azure/app-/-webbappar).</span><span class="sxs-lookup"><span data-stu-id="fcf39-111">Optionalservicename: optional parameter for services with subservices, or toospecify a SKU (ex: web apps correspond with Azure/app-service/web-apps).</span></span>

<span data-ttu-id="fcf39-112">Partnertjänster och orchestrators är rekommenderar toouse-specifikt huvud värden toohelp med våra telemetri.</span><span class="sxs-lookup"><span data-stu-id="fcf39-112">Partner services and orchestrators are encouraged toouse specific header values toohelp with our telemetry.</span></span> <span data-ttu-id="fcf39-113">Användare kan också ändra hello-värdet som skickas toohello huvudet om de så önskar.</span><span class="sxs-lookup"><span data-stu-id="fcf39-113">Users can also modify hello value passed toohello header if they so desire.</span></span>

<span data-ttu-id="fcf39-114">hello-värden som vi vill ACR partners toouse toopopulate hello ”X-Meta-käll-klient” fältet är nedan:</span><span class="sxs-lookup"><span data-stu-id="fcf39-114">hello values we want ACR partners toouse toopopulate hello "X-Meta-Source-Client" field are below:</span></span>

| <span data-ttu-id="fcf39-115">Tjänstnamn</span><span class="sxs-lookup"><span data-stu-id="fcf39-115">Service Name</span></span>              | <span data-ttu-id="fcf39-116">Huvudet</span><span class="sxs-lookup"><span data-stu-id="fcf39-116">Header</span></span>                                |
| ------------------------- | ------------------------------------- |
| <span data-ttu-id="fcf39-117">Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="fcf39-117">Azure Container Service</span></span>   | <span data-ttu-id="fcf39-118">Azure/beräkning/azure-behållaren-tjänst</span><span class="sxs-lookup"><span data-stu-id="fcf39-118">azure/compute/azure-container-service</span></span> |
| <span data-ttu-id="fcf39-119">App Service - Webbappar</span><span class="sxs-lookup"><span data-stu-id="fcf39-119">App Service - Web Apps</span></span>    | <span data-ttu-id="fcf39-120">Azure/app-/-webbappar</span><span class="sxs-lookup"><span data-stu-id="fcf39-120">azure/app-service/web-apps</span></span>            |
| <span data-ttu-id="fcf39-121">Apptjänst - Logic Apps</span><span class="sxs-lookup"><span data-stu-id="fcf39-121">App Service - Logic Apps</span></span>  | <span data-ttu-id="fcf39-122">Azure/app-tjänsten /-logikappar</span><span class="sxs-lookup"><span data-stu-id="fcf39-122">azure/app-service/logic-apps</span></span>          |
| <span data-ttu-id="fcf39-123">Batch</span><span class="sxs-lookup"><span data-stu-id="fcf39-123">Batch</span></span>                     | <span data-ttu-id="fcf39-124">batch-Azure/beräkning</span><span class="sxs-lookup"><span data-stu-id="fcf39-124">azure/compute/batch</span></span>                   |
| <span data-ttu-id="fcf39-125">Cloud-konsolen</span><span class="sxs-lookup"><span data-stu-id="fcf39-125">Cloud Console</span></span>             | <span data-ttu-id="fcf39-126">Azure-cloud-konsolen</span><span class="sxs-lookup"><span data-stu-id="fcf39-126">azure/cloud-console</span></span>                   |
| <span data-ttu-id="fcf39-127">Funktioner</span><span class="sxs-lookup"><span data-stu-id="fcf39-127">Functions</span></span>                 | <span data-ttu-id="fcf39-128">Azure-beräknings-funktioner</span><span class="sxs-lookup"><span data-stu-id="fcf39-128">azure/compute/functions</span></span>               |
| <span data-ttu-id="fcf39-129">Internet saker - hubb</span><span class="sxs-lookup"><span data-stu-id="fcf39-129">Internet of Things - Hub</span></span>  | <span data-ttu-id="fcf39-130">Azure-iot-hubb</span><span class="sxs-lookup"><span data-stu-id="fcf39-130">azure/iot/hub</span></span>                         |
| <span data-ttu-id="fcf39-131">HDInsight</span><span class="sxs-lookup"><span data-stu-id="fcf39-131">HDInsight</span></span>                 | <span data-ttu-id="fcf39-132">hdinsight-Azure/data</span><span class="sxs-lookup"><span data-stu-id="fcf39-132">azure/data/hdinsight</span></span>                  |
| <span data-ttu-id="fcf39-133">Jenkins</span><span class="sxs-lookup"><span data-stu-id="fcf39-133">Jenkins</span></span>                   | <span data-ttu-id="fcf39-134">Azure/jenkins</span><span class="sxs-lookup"><span data-stu-id="fcf39-134">azure/jenkins</span></span>                         |
| <span data-ttu-id="fcf39-135">Machine Learning</span><span class="sxs-lookup"><span data-stu-id="fcf39-135">Machine Learning</span></span>          | <span data-ttu-id="fcf39-136">Azure/data/machile-utbildning</span><span class="sxs-lookup"><span data-stu-id="fcf39-136">azure/data/machile-learning</span></span>           |
| <span data-ttu-id="fcf39-137">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="fcf39-137">Service Fabric</span></span>            | <span data-ttu-id="fcf39-138">Azure/beräkning/service-infrastrukturresurser</span><span class="sxs-lookup"><span data-stu-id="fcf39-138">azure/compute/service-fabric</span></span>          |
| <span data-ttu-id="fcf39-139">VSTS</span><span class="sxs-lookup"><span data-stu-id="fcf39-139">VSTS</span></span>                      | <span data-ttu-id="fcf39-140">Azure/vsts</span><span class="sxs-lookup"><span data-stu-id="fcf39-140">azure/vsts</span></span>                            |


## <a name="next-steps"></a><span data-ttu-id="fcf39-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fcf39-141">Next steps</span></span>
[<span data-ttu-id="fcf39-142">Mer information om register och hello stöds tjänster och orchestrators</span><span class="sxs-lookup"><span data-stu-id="fcf39-142">Learn more about registries and hello supported services and orchestrators</span></span>](container-registry-intro.md)
