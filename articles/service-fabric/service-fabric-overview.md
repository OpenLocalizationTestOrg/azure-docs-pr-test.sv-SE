---
title: aaaOverview av Service Fabric i Azure | Microsoft Docs
description: "En översikt över Service Fabric där program består av många mikrotjänster tooprovide skalning och återhämtning. Service Fabric är en plattform för distribuerade system används toobuild skalbar, tillförlitlig och enkelt hanterade program för hello molnet."
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: masnider
ms.assetid: bbcc652a-a790-4bc4-926b-e8cd966587c0
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/02/2017
ms.author: mfussell
ms.openlocfilehash: 427fcedf97e6b2aae42d240c63e9f85daed8d962
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-azure-service-fabric"></a>Översikt över Azure Service Fabric
Azure Service Fabric är en plattform för distribuerade system som gör det enkelt toopackage, distribuera och hantera skalbara och tillförlitliga mikrotjänster och behållare. Service Fabric adresser även hello betydande utmaningarna med att utveckla och hantera interna molnprogram. Utvecklare och administratörer kan undvika komplexa infrastrukturproblem och fokusera på att implementera verksamhetskritiska, krävande arbetsbelastningar som är skalbara, tillförlitliga och hanterbara. Service Fabric representerar hello nästa generations plattform för att skapa och hantera dessa företagsklass, nivå 1, molnskala program som körs i behållare.

Den här korta videon beskriver Service Fabric och mikrotjänster:<center><a target="_blank" href="https://aka.ms/servicefabricvideo">  
<img src="./media/service-fabric-overview/OverviewVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="applications-composed-of-microservices"></a>Program består av mikrotjänster 
Service Fabric kan du toobuild och hantera skalbara och tillförlitliga program består av mikrotjänster som körs på hög densitet på en delad pool av datorer, som kallas tooas ett kluster. Den innehåller en avancerad, lightweight runtime toobuild distribueras, skalbar, tillståndslösa och tillståndskänsliga mikrotjänster som körs i behållare. Det ger också omfattande program management funktioner tooprovision, distribuera, övervaka, uppgradering/korrigeringsfil och ta bort distribuerade program, bland annat av tjänster.

Service Fabric används många Microsoft-tjänster idag, inklusive Azure SQL Database, Azure Cosmos DB, Cortana, Microsoft Power BI, Microsoft Intune, Azure Event Hubs, Azure IoT Hub, Dynamics 365, Skype för företag och många viktiga Azure-tjänster.

Service Fabric är skräddarsydda toocreate interna molntjänster som kan börja litet, vid behov och växa toomassive skala och med hundratals eller tusentals datorer.

Dagens Internet-skala services bygger på mikrotjänster. Exempel på mikrotjänster är protokollet gateways, användarprofiler, shopping Datorvagnar, inventering, bearbetning, köer, och cachelagrar. Service Fabric är en plattform för mikrotjänster som ger varje mikrotjänster (eller behållaren) ett unikt namn som kan vara tillståndslösa och tillståndskänsliga.

Service Fabric ger omfattande runtime och livscykel hantering/funktioner-tooapplications som består av dessa mikrotjänster. Den är värd för mikrotjänster i behållare som har distribuerats och aktiverats över hello Service Fabric-kluster. Flytta från virtuella datorer toocontainers möjliggör en ökning i ordning för omfattning i densitet. En annan storleksordning i densitet på samma sätt blir möjligt när du flyttar från behållare toomicroservices i dessa behållare. Till exempel består ett enskilt kluster för Azure SQL Database av hundratals datorer med tiotusentals behållare som är värdar för totalt hundratals tusentals databaser. Varje databas är en tillståndskänslig Service Fabric-mikrotjänster. 

Mer information om hello mikrotjänster metod läsa [varför en mikrotjänster metod toobuilding program?](service-fabric-overview-microservices.md)

## <a name="container-deployment-and-orchestration"></a>Distribution i behållare och orchestration
Service Fabric är Microsofts [behållare orchestrator](service-fabric-cluster-resource-manager-introduction.md) distribuera mikrotjänster i ett kluster på datorer. Mikrotjänster kan utvecklas på många sätt använder hello [Service Fabric programmeringsmodeller](service-fabric-choose-framework.md), [ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md), toodeploying [någon kod önskat](service-fabric-deploy-existing-app.md). Allt du kan blanda både tjänster i processer och tjänster i behållare i hello samma program. Om du bara vill för[distribuera och hantera behållare](service-fabric-containers-overview.md), Service Fabric är det perfekta valet som en behållare för orchestrator.

## <a name="any-os-any-cloud"></a>Någon OS alla moln
Service Fabric körs överallt. Du kan skapa kluster för Service Fabric i många miljöer, inklusive Azure eller lokalt, på Windows Server eller på Linux. Du kan även skapa kluster på andra offentliga moln. Hello utvecklingsmiljö i hello SDK är dessutom **identiska** toohello produktionsmiljön med ingen emulatorerna ingår. Med andra ord distribuerar vad körs på klustret för lokal utveckling toohello kluster i andra miljöer.

![Service Fabric-plattformen][Image1]

För mer information om hur du skapar kluster lokalt, kan du läsa [skapar ett kluster på Windows Server- eller Linux](service-fabric-deploy-anywhere.md) eller för att skapa ett kluster Azure [via hello Azure-portalen](service-fabric-cluster-creation-via-portal.md).

## <a name="stateless-and-stateful-microservices-for-service-fabric"></a>Tillståndslösa och tillståndskänsliga mikrotjänster för Service Fabric
Service Fabric kan toobuild-program som består av mikrotjänster eller behållare. Tillståndslösa mikrotjänster (till exempel protokollet gateways och webbproxyservrar) har inte tillståndet föränderliga utanför en begäran och dess svar från hello-tjänsten. Arbetsroller i Azure Cloud Services är ett exempel på en tillståndslös tjänst. Tillståndskänsliga mikrotjänster (till exempel användarkonton, databaser, enheter, shoppingvagnar och köer) Underhåll tillståndet föränderliga, auktoritär utöver hello begäran och sitt svar. Dagens Internet-skala program består av en kombination av tillståndslösa och tillståndskänsliga mikrotjänster. 

En nyckel differentation med Service Fabric är starkt fokus på bygga tillståndskänsliga tjänster, antingen med hello [inbyggda programmeringsmodeller ](service-fabric-choose-framework.md) eller med container tillståndskänsliga tjänster. Hej [Programscenarier](service-fabric-application-scenarios.md) beskrivs hello scenarier där tillståndskänsliga tjänster används.


## <a name="application-lifecycle-management"></a>Livscykel för programhantering
Service Fabric stöder hello fullständiga programmet livscykel och CI/CD-molnprogram inklusive behållare. Den här livscykeln innehåller utveckling till distribution, dagliga hantering och underhåll tooeventual inaktivering.

Funktioner för hantering av Service Fabric application livscykel aktivera administratörer och IT-ansvariga toouse enkel, smidig arbetsflöden tooprovision, distribuera, korrigering, och övervaka program. De här inbyggda arbetsflödena minska avsevärt hello belastningen på IT operatörer tookeep program kontinuerligt tillgänglig.

De flesta program består av en kombination av tillståndslösa och tillståndskänsliga mikrotjänster, behållare och andra körbara filer som har distribuerats tillsammans. Service Fabric aktiveras med starka typer om hello program, hello distribution av flera instanser av programmet. Varje instans hanteras och uppgradera separat. Allt kan Service Fabric distribuera behållare eller alla körbara filer och gör dem tillförlitliga. Service Fabric kan till exempel distribuera .NET, ASP.NET Core, node.js, Windows-behållare, Linux behållare, Java virtuella datorer, skript, Angular eller bokstavligt något som utgör ditt program.

Service Fabric är integrerat med CI/CD-verktyg som [Visual Studio Team Services](https://www.visualstudio.com/team-services/), [Jenkins](https://jenkins.io/index.html), och [bläckfisk distribuera](https://octopus.com/) och kan användas med andra populära verktyg CI/CD.

Mer information om programhantering livscykel [programmet livscykel](service-fabric-application-lifecycle.md). Mer information om hur toodeploy någon kod finns [distribuera en körbar fil gäst](service-fabric-deploy-existing-app.md).

## <a name="key-capabilities"></a>De viktigaste funktionerna
Med hjälp av Service Fabric kan du:

* Distribuera tooAzure eller tooon lokala datacenter som kör Windows eller Linux med noll kodändringar. Skriv en gång och sedan distribuera var som helst tooany Service Fabric-klustret.
* Utveckla skalbara program som består av mikrotjänster med hjälp av hello Service Fabric programmeringsmodeller, behållare eller någon kod.
* Utveckla mycket pålitlig tillståndslösa och tillståndskänsliga mikrotjänster. Förenkla hello utformning av ditt program genom att använda tillståndskänsliga mikrotjänster. 
* Använda hello nya Reliable Actors programming modellen toocreate molnobjekt med självständiga kod och tillstånd.
* Distribuera och samordna behållare som är behållare för Windows och Linux-behållare. Service Fabric är en data-medveten, tillståndsbaserad behållaren orchestrator.
* Distribuera program i sekunder som hög densitet med hundratals eller tusentals program eller behållare per dator.
* Distribuera olika versioner av hello samma program sida vid sida och uppgradera varje program oberoende av varandra.
* Hantera hello livscykeln för dina program utan driftavbrott, dela och hårt uppgraderingar.
* Skala ut eller skala hello antal noder i ett kluster. När du skalar noder skalas dina program automatiskt.
* Övervaka och diagnostisera hello hälsotillståndet för dina program och ange principer för att utföra automatiska reparationer.
* Titta på hello resursutjämnaren samordna hello vidaredistribution av program över hello kluster. Service Fabric återställs från fel och optimerar hello distribution av belastningen baserat på tillgängliga resurser.

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a>Nästa steg
* Mer information:
  * [Varför en mikrotjänster hanterar toobuilding program?](service-fabric-overview-microservices.md)
  * [Terminologi: översikt](service-fabric-technical-overview.md)
* Konfigurera Service Fabric [utvecklingsmiljö](service-fabric-get-started.md)  
* Lär dig mer om [Service Fabric-supportalternativen](service-fabric-support.md)

[Image1]: media/service-fabric-overview/Service-Fabric-Overview.png
