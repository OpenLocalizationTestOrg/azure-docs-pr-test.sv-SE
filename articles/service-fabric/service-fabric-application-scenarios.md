---
title: aaaApplication scenarier och design | Microsoft Docs
description: "Översikt över kategorier av molnprogram i Service Fabric. Beskriver programdesign som använder tillståndskänsliga och tillståndslösa tjänster."
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: 3a8ca6ea-b8e9-4bc3-9e20-262437d2528e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 7/02/2017
ms.author: mfussell
ms.openlocfilehash: e36d5b2d21a6a1e3e85c9b21190072616e4921e8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-application-scenarios"></a>Service Fabric-Programscenarier
Azure Service Fabric är en tillförlitlig och flexibel plattform som gör att du toowrite och kör många typer av program och tjänster. Dessa program och mikrotjänster kan vara tillståndslösa och tillståndskänsliga och de är resurs belastningsutjämnade över virtuella datorer toomaximize effektivitet. hello unika arkitektur för Service Fabric kan tooperform nära dataanalys i realtid, beräkning i minnet, parallella transaktioner och för händelsebearbetning i dina program. Du kan enkelt skala dina program uppåt eller nedåt (verkligen in eller ut), beroende på resurskraven ändras.

hello Service Fabric-plattformen i Azure är idealisk för följande typer av program hello:

* **Tjänster med hög tillgänglighet**: Service Fabric services ger snabb växling vid fel genom att skapa flera tjänsten secondary repliker. Om en nod, en process eller en enskild tjänst kraschar på grund av toohardware eller andra fel är hello sekundära repliker upphöjt tooa primära repliken med minimal förlust av tjänsten.
* **Skalbara tjänster**: enskilda tjänster kan partitioneras, vilket ger tillstånd toobe skala ut över hello kluster. Dessutom kan individuella tjänster skapas och tas bort på hello direkt. Tjänster kan snabbt och enkelt skala ut från några instanser på ett par noder toothousands av instanser för många noder och sedan skalas i igen, beroende på dina resursbehov. Du kan använda Service Fabric toobuild dessa tjänster och hantera sina fullständiga livscykler.
* **Beräkningar på icke-statisk data**: Service Fabric kan du toobuild data, i/o och beräkning beräkningsintensiva tillståndskänsliga program. Service Fabric kan hello samplacering av bearbetning (beräkning) och data i program. Vanligtvis när programmet kräver åtkomst toodata, finns Nätverksfördröjningen som är associerade med en extern cache eller lagring datanivå. Med tillståndskänslig Service Fabric-tjänster elimineras den latens, aktivera flera performant läsning och skrivning. Anta till exempel att du har ett program som utför nära realtid rekommendation val för kunder med ett tidsfördröjningen krav för mindre än 100 millisekunder. hello svarstid och prestandaegenskaper för Service Fabric-tjänster (där hello beräkning av rekommendation markeringen är samordnad med hello data och regler) tillhandahåller en dynamisk upplevelse toohello användare jämfört med vanlig hello-implementering modell för att toofetch hello nödvändiga data från Fjärrlagring.  
* **Sessionsbaserade interaktiva program**: Service Fabric är användbart om krävs för ditt program, till exempel onlinespel eller snabbmeddelanden, låg latens läsningar och skrivningar. Service Fabric kan du toobuild dessa interaktiv, tillståndskänsliga program utan att behöva toocreate ett separat Arkiv eller cache, vilket krävs för tillståndslösa appar. (Detta ökar svarstiden och kan vara introduceras problem med konsekvenskontroll.).
* **Dataanalys och arbetsflöden**: hello Snabb läsning och skrivning av Service Fabric kan program som måste på ett tillförlitligt sätt att bearbeta händelser eller dataströmmar. Service Fabric gör det också möjligt för program som beskriver bearbetning pipelines, där resultat måste vara tillförlitliga och skickade på toohello bearbetning av nästa fas utan dataförlust. Dessa inkluderar transaktionella och finansiella system, där data konsekvens och beräkning garantier är av yttersta vikt.
* **Att samla in data, bearbetning och IoT**: eftersom Service Fabric hanterar storskaliga och har låg latens via dess tillståndskänsliga tjänster, är det perfekt för databearbetning på miljontals enheter där hello data hello enhet och hello beräkning är samordnad.
Vi har sett flera kunder som har skapat IoT-system med hjälp av Service Fabric inklusive [BMW](https://blogs.msdn.microsoft.com/azureservicefabric/2016/08/24/service-fabric-customer-profile-bmw-technology-corporation/), [Schneider elektriska](https://blogs.msdn.microsoft.com/azureservicefabric/2016/08/05/service-fabric-customer-profile-schneider-electric/) och [nät system](https://blogs.msdn.microsoft.com/azureservicefabric/2016/06/20/service-fabric-customer-profile-mesh-systems/).

## <a name="application-design-case-studies"></a>Programmet design fallstudier
Ett antal fallstudier som visar hur Service Fabric är används toodesign program publiceras på hello [Service Fabric-teamets blogg](https://blogs.msdn.microsoft.com/azureservicefabric/tag/customer-profile/) och hello [mikrotjänster lösningar plats](https://azure.microsoft.com/solutions/microservice-applications/).

## <a name="design-applications-composed-of-stateless-and-stateful-microservices"></a>Utforma program består av tillståndslösa och tillståndskänsliga mikrotjänster
Skapa program med Azure Cloud Service arbetsroller är ett exempel på en tillståndslös tjänst. Däremot upprätthålla tillståndskänslig mikrotjänster deras auktoritära tillstånd utöver hello begäran och sitt svar. Detta ger hög tillgänglighet och konsekvens i hello tillstånd via enkla API: er som ger transaktionell garantier backas upp av replikering. Service Fabric tillståndskänsliga tjänster demokratisera hög tillgänglighet, gör den tooall typer av program, inte bara databaser och andra dataarkiv. Det här är ett naturlig förlopp. Program har redan flyttat från att använda enbart relationsdatabaser för hög tillgänglighet tooNoSQL databaser. Nu kan hello programmen ha sina ”heta” tillstånd och data som hanteras i dem för ytterligare prestandavinster utan att kompromissa tillförlitlighet, konsekvens och tillgänglighet.

När du skapar program som består av mikrotjänster vanligtvis har en kombination av tillståndslösa webbprogram (ASP.NET, Node.js, etc.) anrop till tillståndslösa och tillståndskänsliga mellannivå företagstjänster, alla som har distribuerats till hello samma Service Fabric-kluster kommandona hello Service Fabric-distribution. Var och en av dessa tjänster är oberoende med beaktande tooscale, tillförlitlighet och Resursanvändning avsevärt förbättra flexibilitet i utveckling och livscykelhantering.

Tillståndskänsliga mikrotjänster förenkla programmet Designer eftersom de tar bort hello behovet av ytterligare hello-köer och cacheminnen har traditionellt krävs tooaddress hello tillgänglighet och svarstid kraven i rent tillståndslösa program. Eftersom tillståndskänsliga tjänster är naturligt hög tillgänglighet och låg latens, innebär det att det finns färre glidande delar toomanage i ditt program som helhet. hello diagram nedan visar hello skillnaderna mellan skapar ett program som är tillståndslösa och ett som är tillståndskänslig. Genom att utnyttja hello [Reliable Services](service-fabric-reliable-services-introduction.md) och [Reliable Actors](service-fabric-reliable-actors-introduction.md) programmeringsmodeller, tillståndskänsliga tjänster minska programmet komplexitet och upprätthålla högt genomflöde och låg latens.

## <a name="an-application-built-using-stateless-services"></a>Ett program som skapats med hjälp av tillståndslösa tjänster
![Program med hjälp av tillståndslösa tjänsten][Image1]

## <a name="an-application-built-using-stateful-services"></a>Ett program som skapats med tillståndskänsliga tjänster
![Program med hjälp av tillståndslösa tjänsten][Image2]

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a>Nästa steg

* Lyssna för[kundfallstudier](https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=qDJnf86yC_5206218965
)
* Läs mer om [kundfallstudier](https://blogs.msdn.microsoft.com/azureservicefabric/tag/customer-profile/)
* Lär dig mer om [mönster och scenarier](service-fabric-patterns-and-scenarios.md)

* Börja bygga tillståndslösa och tillståndskänsliga tjänster med hello Service Fabric [reliable services](service-fabric-reliable-services-quick-start.md) och [tillförlitliga aktörer](service-fabric-reliable-actors-get-started.md) programmeringsmodeller.
* Se även hello följande avsnitt:
  * [Berätta om mikrotjänster](service-fabric-overview-microservices.md)
  * [Definiera och hantera tjänstens tillstånd](service-fabric-concepts-state.md)
  * [Tillgänglighet för Service Fabric-tjänster](service-fabric-availability-services.md)
  * [Skala Service Fabric-tjänster](service-fabric-concepts-scalability.md)
  * [Partitionen Service Fabric-tjänster](service-fabric-concepts-partitioning.md)

[Image1]: media/service-fabric-application-scenarios/AppwithStatelessServices.jpg
[Image2]: media/service-fabric-application-scenarios/AppwithStatefulServices.jpg
