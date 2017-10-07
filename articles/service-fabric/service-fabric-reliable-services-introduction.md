---
title: "aaaOverview av hello tillförlitlig tjänst för Service Fabric-programmeringsmodell | Microsoft Docs"
description: "Lär dig mer om programmeringsmodell för tillförlitlig tjänst för Service Fabric och börja skriva egna tjänster."
services: Service-Fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: vturecek; mani-ramaswamy
ms.assetid: 0c88a533-73f8-4ae1-a939-67d17456ac06
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: masnider;
ms.openlocfilehash: 41d1826df902b1f1845c4702bf2567e6b9ca1f1f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="reliable-services-overview"></a>Översikt över Reliable Services
Azure Service Fabric gör det enklare att skriva och hantera tillståndslösa och tillståndskänsliga Reliable Services. Detta avsnitt:

* hello Reliable Services programmeringsmodell för tillståndslösa och tillståndskänsliga tjänster.
* hello val du har toomake när du skriver en tillförlitlig tjänst.
* Vissa scenarier och exempel på när toouse tillförlitlig tjänster och hur de skrivs.

Reliable Services är en av hello programmeringsmodeller finns på Service Fabric. hello andra är hello tillförlitliga aktören programmeringsmodell, vilket är en programmeringsmodell med virtuella aktören på hello Reliable Services modell. Mer information om hello Reliable Actors Programmeringsmiljön finns [introduktion tooService Fabric Reliable Actors](service-fabric-reliable-actors-introduction.md).

Service Fabric hanterar hello livslängden för tjänster, från etablering och distribution via uppgradering och borttagning, via [Service Fabric programhantering](service-fabric-deploy-remove-applications.md).

## <a name="what-are-reliable-services"></a>Vad är Reliable Services?
Reliable Services ger dig ett enkelt kraftfulla, översta programming modellen toohelp du express vad är viktigt tooyour program. Med hello Reliable Services programmeringsmodellen, får du:

* Åtkomst toohello resten av hello Service Fabric programming API: er. Till skillnad från Service Fabric Services modelleras som [gäst körbara filer](service-fabric-deploy-existing-app.md), Reliable Services hämta toouse hello resten av hello Service Fabric API: er direkt. Detta tillåter tjänster:
  * frågan hello system
  * rapporten hälsotillstånd om entiteter i hello kluster
  * få meddelanden om ändringar i konfigurationen och kod
  * hitta och kommunicera med andra tjänster
  * (valfritt) använda hello [tillförlitliga samlingar](service-fabric-reliable-services-reliable-collections.md)
  * .. och ge dem åtkomst till toomany andra funktioner, allt från en förstklassigt programmeringsmodell i flera programmeringsspråk.
* En enkel modell för att köra din kod som ser ut som programmeringsmodeller som används för att. Koden har en väldefinierad startpunkt och lätthanterade livscykel.
* En modell med pluggable kommunikation. Använd hello transport du själv väljer, till exempel HTTP med [Web API](service-fabric-reliable-services-communication-webapi.md), WebSockets, anpassade TCP-protokoll eller något annat. Reliable Services innehåller några bra out box-alternativ som du kan använda eller ange en egen.
* För tillståndskänsliga tjänster hello Reliable Services programmeringsmodellen kan du tooconsistently och på ett tillförlitligt sätt lagra din tillstånd i din tjänst med hjälp av [tillförlitliga samlingar](service-fabric-reliable-services-reliable-collections.md). Tillförlitliga samlingar är en enkel uppsättning hög tillgänglighet och tillförlitlig Samlingsklasser som kommer att vara bekant tooanyone som har använt C#-samlingar. Traditionellt tjänster som krävs för externa system för tillståndshantering av tillförlitliga. Med tillförlitlig samlingar, kan du lagra dina tillstånd nästa tooyour beräknings med hello samma hög tillgänglighet och tillförlitlighet som du har komma tooexpect från högtillgänglig externa butiker. Den här modellen förbättrar också latens eftersom du samordna hello beräknings- och tillstånd måste toofunction.

Det här videoklippet Microsoft Virtual Academy en översikt över Reliable services:<center>
<a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=HhD9566yC_4106218965">
<img src="./media/service-fabric-reliable-services-introduction/ReliableServicesVid.png" WIDTH="360" HEIGHT="244" />
</a>
</center>

## <a name="what-makes-reliable-services-different"></a>Vad gör Reliable Services olika?
Reliable Services i Service Fabric skiljer sig från tjänster som du har skrivit innan. Service Fabric ger tillförlitlighet, tillgänglighet, konsekvens och skalbarhet.

* **Tillförlitlighet** – din tjänst förblir in även i instabilt miljöer där dina datorer inte eller nådde nätverksproblem, eller i fall där hello tjänsterna själva uppstår fel och krascher eller misslyckas. För tillståndskänsliga tjänster bevaras din även i hello förekomst av nätverket eller andra fel.
* **Tillgänglighet** -tjänsten är kan nås och svarstid. Service Fabric underhåller din önskade antalet kopior som körs.
* **Skalbarhet** - tjänster är fristående från maskinvara, och de kan öka eller minska vid behov via hello tillägg eller borttagning av maskinvara eller andra resurser. Tjänster som är lätt partitionerade (särskilt i hello tillståndskänslig fallet) tooensure som hello-tjänsten kan skalas och referensen misslyckades delvis. Tjänster kan skapas och tas bort dynamiskt via kod, aktivera flera instanser toobe efter en redundansväxling kan vid behov Säg i svaret toocustomer begäranden. Slutligen uppmuntrar Service Fabric services toobe lightweight. Service Fabric kan tusentals services toobe tillhandahållas inom en enda process i stället för att kräva eller trafikklass hela OS-instanser eller processer tooa instans av en tjänst.
* **Konsekvenskontroll** -all information som lagras i den här tjänsten kan garanteras toobe konsekvent. Detta gäller även över flera tillförlitliga samlingar i en tjänst. Över samlingarna i en tjänst kan ändras i ett transaktionellt atomiska sätt.

## <a name="service-lifecycle"></a>Tjänsten livscykel
Om din tjänst är tillståndskänslig eller tillståndslösa, tillhandahåller tillförlitlig en enkel livscykel kan du snabbt ansluta din kod och komma igång.  Det finns en eller två metoder som du behöver tooimplement tooget din tjänst igång.

* **CreateServiceReplicaListeners/CreateServiceInstanceListeners** -den här metoden är där hello service definierar hello kommunikation stack(s) läggs toouse. Hej kommunikation stack som [Web API](service-fabric-reliable-services-communication-webapi.md), är det definierar hello lyssnarslutpunkten eller slutpunkter för hello service (hur klienter nå hello service). Den definierar hur hälsningsmeddelande som visas interagera med hello resten av hello service-kod.
* **RunAsync** -metoden är där tjänsten körs dess affärslogik och där det skulle startar alla bakgrundsuppgifter som ska köras under hello livstid hello-tjänsten. hello annullering token som har angetts är en signal för när arbetet ska sluta. Till exempel om hello service måste toopull meddelanden från en tillförlitlig kö och bearbeta dem, är där som fungerar som händer.

Om du lär dig tillförlitliga tjänster för hello första gången, läsa på! Om du letar efter en detaljerad genomgång av hello livscykeln för tillförlitlig tjänster, du kan gå för[i den här artikeln](service-fabric-reliable-services-lifecycle.md).

## <a name="example-services"></a>Exempel services
Att veta den här programmeringsmodell ska vi ta en titt på två olika tjänster toosee hur dessa delar hänger ihop.

### <a name="stateless-reliable-services"></a>Tillståndslösa Reliable Services
Tillståndslösa tjänsten är en där det finns ingen delstat bibehålls inom hello service mellan anrop. Alla lägen som finns är enbart tillgänglig och behöver inte synkronisering, replikering, beständiga eller hög tillgänglighet.

Anta till exempel att en kalkylator som har inget minne och tar emot alla villkor och åtgärder tooperform på samma gång.

I det här fallet hello `RunAsync()` (C#) eller `runAsync()` (Java) för hello-tjänsten kan vara tom, eftersom det finns ingen bakgrund aktivitet-bearbetning hello tjänsten måste toodo. När hello Kalkylatorn tjänst skapas, returneras ett `ICommunicationListener` (C#) eller `CommunicationListener` (Java) (till exempel [Web API](service-fabric-reliable-services-communication-webapi.md)) som öppnar en lyssnarslutpunkten på vissa port. Den här lyssnarslutpunkten skapar toohello beräkning av olika metoder (exempel: ”Lägg till (n1, n2)”) som definierar hello Kalkylatorn offentliga API: N.

När ett anrop görs från en klient, hello lämplig metod har anropats och hello Kalkylatorn tjänsten utför hello på hello data som anges och returnerar hello resultat. Informationen lagras inte i några tillstånd.

Inte lagrar några interna tillstånd enkelt kalkylator för det här exemplet. Men de flesta tjänster är inte helt tillståndslösa. I stället gör de deras tillstånd toosome andra store. (Till exempel ett webbprogram som förlitar sig på att hålla sessionstillstånd i en säkerhetskopiering eller cache är inte tillståndslösa.)

Ett vanligt exempel på hur tillståndslösa tjänster används i Service Fabric är som en klientdel som visar hello offentliga API: et för ett webbprogram. hello klienttjänst pratar sedan toostateful services toocomplete en användarbegäran. I det här fallet är anrop från klienter dirigerad tooa kända port, till exempel 80, där hello tillståndslösa tjänsten lyssnar. Tillståndslösa tjänsten tar emot hello anrop och avgör om hello anropet kommer från en betrodd part och vilken tjänst som den är avsedd för.  Hello tillståndslösa tjänsten vidarebefordrar hello anropet toohello rätt partition av hello tillståndskänslig service och väntar på svar. När hello tillståndslösa tjänsten tar emot ett svar, svarar den ursprungliga toohello-klienten. Ett exempel på dessa tjänster finns i våra exempel [C#](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started) / [Java](https://github.com/Azure-Samples/service-fabric-java-getting-started/tree/master/Actors/VisualObjectActor/VisualObjectWebService). Detta är bara ett exempel på detta mönster i hello-exempel finns det andra i samt andra prover.

### <a name="stateful-reliable-services"></a>Tillståndskänsliga Reliable Services
En tillståndskänslig service är en som måste ha en del av tillstånd hålls konsekvent och för hello tjänsten toofunction. Överväg att en tjänst som ständigt beräknar en rullande medelvärdet för ett värde baserat på den tar emot uppdateringar. toodo, det måste ha hello aktuella uppsättningen av inkommande begäranden måste tooprocess och hello aktuella medelvärde. Alla tjänster som hämtar bearbetar och lagrar information i en extern butik (till exempel ett Azure blob eller tabell store idag) är tillståndskänslig. Den bevarar bara dess tillstånd i hello externa tillståndslagret.

De flesta tjänster lagra idag deras tillstånd externt, eftersom den erbjuder tillförlitlighet, tillgänglighet, skalbarhet och konsekvens för det aktuella tillståndet hello extern butik. I Service Fabric services är inte obligatoriska toostore externt deras tillstånd. Service Fabric hand tar om kraven för både hello service-koden och hello-tjänstens tillstånd.

> [!NOTE]
> Stöd för tillståndskänsliga Reliable Services är inte tillgängligt på Linux ännu (för C# och Java).
>

Anta att vi vill toowrite en tjänst som bearbetar bilder. toodo detta hello tjänsten tar in en avbildning och hello serie konverteringar tooperform på avbildningen. Den här tjänsten returnerar en lyssnare för kommunikation (vi anta att det är en WebAPI) som visar en API som `ConvertImage(Image i, IList<Conversion> conversions)`. När den tar emot en begäran hello-tjänsten lagrar den i en `IReliableQueue`, och returnerar vissa id toohello klienten så att den kan spåra hello-begäran.

I den här tjänsten `RunAsync()` kan vara mer komplexa. hello-tjänsten har en loop i dess `RunAsync()` som tar emot förfrågningar från `IReliableQueue` och utför hello konverteringar som begärdes. hello resultat lagras i en `IReliableDictionary` så att när hello klienten kommer tillbaka kan de få deras konverterade bilder. tooensure som även om något misslyckas hello bilden inte förlorade, tillförlitlig tjänsten skulle hämtar utanför hello kön, utföra hello konverteringar och lagra hello resultat i en enda transaktion. I det här fallet hello-meddelande tas bort från kön hello och hello resultat lagras i hello resultatet ordlista endast när hello konverteringar har slutförts. Hello-tjänsten kan även pull hello bilden utanför hello kön och omedelbart lagra det i ett Arkiv för fjärråtkomst. Detta minskar hello mängden hello tillståndstjänsten har toomanage, men ökar komplexiteten eftersom hello-tjänsten har tookeep hello nödvändiga metadata toomanage hello remote store. Om något fel i med antingen metoden kvar hello mellersta hello begäran i hello kön väntar toobe bearbetas.

En sak toonote om den här tjänsten är att det låter som en normal .NET-tjänsten! hello enda skillnaden är att hello datastrukturer som används (`IReliableQueue` och `IReliableDictionary`) tillhandahålls av Service Fabric och är tillförlitliga, hög tillgänglighet och konsekvent.

## <a name="when-toouse-reliable-services-apis"></a>När toouse Reliable Services API: er
Om hello följande karaktäriserar behöver ditt program bör du överväga Reliable Services API: er:

* Du vill din tjänst kod (och alternativt tillstånd) toobe hög tillgänglighet och tillförlitlig
* Du behöver transaktionella garantier över flera enheter av tillstånd (till exempel order och ordning artiklar).
* Programmets tillstånd kan modelleras naturligt som tillförlitliga ordböcker och köer.
* Ditt program, kod eller tillstånd måste toobe hög tillgänglighet med låg latens läsningar och skrivningar.
* Programmet måste toocontrol hello samtidighet eller Granulariteten för överförda åtgärder mellan en eller flera tillförlitliga samlingar.
* Vill du toomanage hello kommunikation eller kontrollen hello partitioneringsschema för din tjänst.
* Din kod måste en fritrådade körningsmiljö.
* Programmet måste toodynamically skapa eller förstöra tillförlitliga ordlistor eller köer eller hela tjänster vid körning.
* Du behöver tooprogrammatically styr förutsatt att Service Fabric-säkerhetskopiering och återställning av funktioner för din tjänst tillstånd.
* Programmet måste toomaintain ändringshistoriken för dess enheter av tillstånd.
* Du vill toodevelop eller använda tillstånd för tredje part utvecklade, anpassade providers.

## <a name="next-steps"></a>Nästa steg
* [Snabbstart för Reliable Services](service-fabric-reliable-services-quick-start.md)
* [Reliable Services avancerade användning](service-fabric-reliable-services-advanced-usage.md)
* [hello programmeringsmodell för Reliable Actors](service-fabric-reliable-actors-introduction.md)
