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
# <a name="troubleshoot-common-issues-when-you-deploy-services-on-azure-service-fabric"></a>Felsök vanliga problem när du distribuerar tjänster i Azure Service Fabric
När du kör tjänster på datorn utvecklare, är det enkelt toouse [Visual Studio felsökningsverktyg](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). För fjärranslutna kluster [hälsorapporter](service-fabric-view-entities-aggregated-health.md) är alltid en bra toostart. Hej enklaste sätt tooaccess dessa rapporter är via PowerShell eller [SFX](service-fabric-visualizing-your-cluster.md). Den här artikeln förutsätter att du felsöker ett kluster och har en grundläggande förståelse av hur toouse någon av dessa verktyg.

## <a name="application-crash"></a>Programmet kraschar
Hej ”partitionen ligger under antalet målrepliker eller instanser mål” rapporten är en bra indikation på att tjänsten kraschar. toofind ut där tjänsten kraschar tar lite mer undersökning. När din tjänst körs i skala, blir din bästa vän en uppsättning eller thought ut spårningar.  Vi rekommenderar att du försöker [Azure Diagnostics](service-fabric-diagnostics-how-to-setup-wad.md) för att samla in dessa spårningar och använder en lösning som [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) för att visa och söka spår hello.

![SFX Partition hälsa](./media/service-fabric-diagnostics-troubleshoot-common-scenarios/crashNewApp.png)

### <a name="during-service-or-actor-initialization"></a>Initiera tjänst eller aktören
Eventuella undantag innan hello tjänsttypen har initierats kommer hello processen toocrash. För dessa typer av krascher visar hello programhändelseloggen hello fel från din tjänst.
Dessa är hello vanligaste undantag toosee innan hello-tjänsten har initierats.

***System.IO.FileNotFoundException***

Det här felet är ofta på grund av toomissing paketberoenden. Kontrollera hello CopyLocal egenskapen i Visual Studio eller hello globala sammansättningscachen för hello-nod.

***System.Runtime.InteropServices.COMException*** *på System.Fabric.Interop.NativeRuntime+IFabricRuntime.RegisterStatefulServiceFactory (IntPtr, IFabricStatefulServiceFactory)*

 Detta anger att hello registrerade typen namn inte matchar hello tjänstmanifestet.

[Azure Diagnostics](service-fabric-diagnostics-how-to-setup-wad.md) kan vara konfigurerade tooupload hello programmets händelselogg för alla noder automatiskt.

### <a name="runasync-or-onactivateasync"></a>RunAsync() eller OnActivateAsync()
Om hello krascher inträffar under initieringen hello eller kör registrerade tjänsttypen eller aktören hello undantagsfel kommer att fångas av Azure Service Fabric. Du kan visa dessa från hello EventSource providers som beskrivs i hello ”nästa steg” avsnittet.

## <a name="next-steps"></a>Nästa steg
Läs mer om befintlig diagnostik som tillhandahålls av Service Fabric:

* [Tillförlitliga aktörer diagnostik](service-fabric-reliable-actors-diagnostics.md)
* [Diagnostik för Reliable Services](service-fabric-reliable-services-diagnostics.md)

