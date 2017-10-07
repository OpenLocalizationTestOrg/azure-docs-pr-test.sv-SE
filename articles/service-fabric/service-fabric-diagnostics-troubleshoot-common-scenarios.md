---
title: "aaaTroubleshooting med händelsespårning | Microsoft Docs"
description: "hello de vanligaste problem som kan uppstå vid distribution av tjänster på Microsoft Azure Service Fabric."
services: service-fabric
documentationcenter: .net
author: mattrowmsft
manager: timlt
editor: 
ms.assetid: 5eb8ef21-da04-4ac8-8b9a-5f7ff1e0a180
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/31/2016
ms.author: mattrow
redirect_url: /azure/service-fabric/service-fabric-diagnostics-overview
ms.openlocfilehash: f5adb7e15fa1e2c964bbbc5726246630c95e13f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-common-issues-when-you-deploy-services-on-azure-service-fabric"></a><span data-ttu-id="ab60c-103">Felsök vanliga problem när du distribuerar tjänster i Azure Service Fabric</span><span class="sxs-lookup"><span data-stu-id="ab60c-103">Troubleshoot common issues when you deploy services on Azure Service Fabric</span></span>
<span data-ttu-id="ab60c-104">När du kör tjänster på datorn utvecklare, är det enkelt toouse [Visual Studio felsökningsverktyg](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).</span><span class="sxs-lookup"><span data-stu-id="ab60c-104">When you're running services on your developer computer, it is easy toouse [Visual Studio's debugging tools](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).</span></span> <span data-ttu-id="ab60c-105">För fjärranslutna kluster [hälsorapporter](service-fabric-view-entities-aggregated-health.md) är alltid en bra toostart.</span><span class="sxs-lookup"><span data-stu-id="ab60c-105">For remote clusters, [health reports](service-fabric-view-entities-aggregated-health.md) are always a good place toostart.</span></span> <span data-ttu-id="ab60c-106">Hej enklaste sätt tooaccess dessa rapporter är via PowerShell eller [SFX](service-fabric-visualizing-your-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="ab60c-106">hello easiest ways tooaccess these reports are through PowerShell or [SFX](service-fabric-visualizing-your-cluster.md).</span></span> <span data-ttu-id="ab60c-107">Den här artikeln förutsätter att du felsöker ett kluster och har en grundläggande förståelse av hur toouse någon av dessa verktyg.</span><span class="sxs-lookup"><span data-stu-id="ab60c-107">This article assumes that you are debugging a remote cluster and have a basic understanding of how toouse either of these tools.</span></span>

## <a name="application-crash"></a><span data-ttu-id="ab60c-108">Programmet kraschar</span><span class="sxs-lookup"><span data-stu-id="ab60c-108">Application crash</span></span>
<span data-ttu-id="ab60c-109">Hej ”partitionen ligger under antalet målrepliker eller instanser mål” rapporten är en bra indikation på att tjänsten kraschar.</span><span class="sxs-lookup"><span data-stu-id="ab60c-109">hello "Partition is below target replica or instance count" report is a good indication that your service is crashing.</span></span> <span data-ttu-id="ab60c-110">toofind ut där tjänsten kraschar tar lite mer undersökning.</span><span class="sxs-lookup"><span data-stu-id="ab60c-110">toofind out where your service is crashing takes a little more investigation.</span></span> <span data-ttu-id="ab60c-111">När din tjänst körs i skala, blir din bästa vän en uppsättning eller thought ut spårningar.</span><span class="sxs-lookup"><span data-stu-id="ab60c-111">When your service is running at scale, your best friend will be a set of well-thought-out traces.</span></span>  <span data-ttu-id="ab60c-112">Vi rekommenderar att du försöker [Azure Diagnostics](service-fabric-diagnostics-how-to-setup-wad.md) för att samla in dessa spårningar och använder en lösning som [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) för att visa och söka spår hello.</span><span class="sxs-lookup"><span data-stu-id="ab60c-112">We suggest that you try [Azure Diagnostics](service-fabric-diagnostics-how-to-setup-wad.md) for collecting those traces and using a solution such as [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) for viewing and searching hello traces.</span></span>

![SFX Partition hälsa](./media/service-fabric-diagnostics-troubleshoot-common-scenarios/crashNewApp.png)

### <a name="during-service-or-actor-initialization"></a><span data-ttu-id="ab60c-114">Initiera tjänst eller aktören</span><span class="sxs-lookup"><span data-stu-id="ab60c-114">During service or actor initialization</span></span>
<span data-ttu-id="ab60c-115">Eventuella undantag innan hello tjänsttypen har initierats kommer hello processen toocrash.</span><span class="sxs-lookup"><span data-stu-id="ab60c-115">Any exceptions before hello service type is initialized will cause hello process toocrash.</span></span> <span data-ttu-id="ab60c-116">För dessa typer av krascher visar hello programhändelseloggen hello fel från din tjänst.</span><span class="sxs-lookup"><span data-stu-id="ab60c-116">For these types of crashes, hello application event log will show hello error from your service.</span></span>
<span data-ttu-id="ab60c-117">Dessa är hello vanligaste undantag toosee innan hello-tjänsten har initierats.</span><span class="sxs-lookup"><span data-stu-id="ab60c-117">These are hello most common exceptions toosee before hello service is initialized.</span></span>

<span data-ttu-id="ab60c-118">***System.IO.FileNotFoundException***</span><span class="sxs-lookup"><span data-stu-id="ab60c-118">***System.IO.FileNotFoundException***</span></span>

<span data-ttu-id="ab60c-119">Det här felet är ofta på grund av toomissing paketberoenden.</span><span class="sxs-lookup"><span data-stu-id="ab60c-119">This error is often due toomissing assembly dependencies.</span></span> <span data-ttu-id="ab60c-120">Kontrollera hello CopyLocal egenskapen i Visual Studio eller hello globala sammansättningscachen för hello-nod.</span><span class="sxs-lookup"><span data-stu-id="ab60c-120">Check hello CopyLocal property in Visual Studio or hello global assembly cache for hello node.</span></span>

<span data-ttu-id="ab60c-121">***System.Runtime.InteropServices.COMException*** *på System.Fabric.Interop.NativeRuntime+IFabricRuntime.RegisterStatefulServiceFactory (IntPtr, IFabricStatefulServiceFactory)*</span><span class="sxs-lookup"><span data-stu-id="ab60c-121">***System.Runtime.InteropServices.COMException*** *at System.Fabric.Interop.NativeRuntime+IFabricRuntime.RegisterStatefulServiceFactory(IntPtr, IFabricStatefulServiceFactory)*</span></span>

 <span data-ttu-id="ab60c-122">Detta anger att hello registrerade typen namn inte matchar hello tjänstmanifestet.</span><span class="sxs-lookup"><span data-stu-id="ab60c-122">This indicates that hello registered service type name does not match hello service manifest.</span></span>

<span data-ttu-id="ab60c-123">[Azure Diagnostics](service-fabric-diagnostics-how-to-setup-wad.md) kan vara konfigurerade tooupload hello programmets händelselogg för alla noder automatiskt.</span><span class="sxs-lookup"><span data-stu-id="ab60c-123">[Azure Diagnostics](service-fabric-diagnostics-how-to-setup-wad.md) can be configured tooupload hello application event log for all your nodes automatically.</span></span>

### <a name="runasync-or-onactivateasync"></a><span data-ttu-id="ab60c-124">RunAsync() eller OnActivateAsync()</span><span class="sxs-lookup"><span data-stu-id="ab60c-124">RunAsync() or OnActivateAsync()</span></span>
<span data-ttu-id="ab60c-125">Om hello krascher inträffar under initieringen hello eller kör registrerade tjänsttypen eller aktören hello undantagsfel kommer att fångas av Azure Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="ab60c-125">If hello crash happens during hello initialization or running of your registered service type or actor, hello exception will be caught by Azure Service Fabric.</span></span> <span data-ttu-id="ab60c-126">Du kan visa dessa från hello EventSource providers som beskrivs i hello ”nästa steg” avsnittet.</span><span class="sxs-lookup"><span data-stu-id="ab60c-126">You can view these from hello EventSource providers detailed in hello "Next steps" section.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ab60c-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ab60c-127">Next steps</span></span>
<span data-ttu-id="ab60c-128">Läs mer om befintlig diagnostik som tillhandahålls av Service Fabric:</span><span class="sxs-lookup"><span data-stu-id="ab60c-128">Learn more about existing diagnostics provided by Service Fabric:</span></span>

* [<span data-ttu-id="ab60c-129">Tillförlitliga aktörer diagnostik</span><span class="sxs-lookup"><span data-stu-id="ab60c-129">Reliable Actors diagnostics</span></span>](service-fabric-reliable-actors-diagnostics.md)
* [<span data-ttu-id="ab60c-130">Diagnostik för Reliable Services</span><span class="sxs-lookup"><span data-stu-id="ab60c-130">Reliable Services diagnostics</span></span>](service-fabric-reliable-services-diagnostics.md)

