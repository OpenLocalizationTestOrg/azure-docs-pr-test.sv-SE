---
title: "aaaService Fabric tillförlitliga aktörer översikt | Microsoft Docs"
description: Introduktion toohello Service Fabric Reliable Actors programmeringsmodell.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 7fdad07f-f2d6-4c74-804d-e0d56131f060
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: ab010cbf936c6cf723b3d453ef95a9bf51f76c95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooservice-fabric-reliable-actors"></a>Introduktion tooService Fabric Reliable Actors
Reliable Actors är ett ramverk för Service Fabric-program som baseras på hello [virtuella aktören](http://research.microsoft.com/en-us/projects/orleans/) mönster. hello tillförlitliga aktörer API är en enkeltrådig programmeringsmodell som bygger på hello skalbarhet och tillförlitlighet garantier från Service Fabric.

## <a name="what-are-actors"></a>Vad är aktörer?
En aktör är en isolerad, oberoende beräknings- och tillstånd med Enkeltrådig körning. Hej [aktören mönster](https://en.wikipedia.org/wiki/Actor_model) är en beräkningar modell för samtidiga eller distribuerade system som ett stort antal dessa aktörer kan köras samtidigt och oberoende av varandra. Aktörer kan kommunicera med varandra och de kan skapa flera aktörer.

### <a name="when-toouse-reliable-actors"></a>När toouse Reliable Actors
Service Fabric Reliable Actors är en implementering av hello aktören designmönstret. Precis som med alla programvara designmönstret passar hello beslut om görs toouse ett specifikt mönster baserat på om huruvida en programvara utforma problemet hello mönster.

Även om hello aktören designmönstret kan vara ett bra val tooa antal distribuerade system problem och scenarier, noggrant övervägande av hello begränsningarna för hello mönster och hello framework implementera måste göras. Som en allmän vägledning Tänk hello aktören mönster toomodel ditt problem eller scenario om:

* Sidan problem innebär att ett stort antal (tusentalsavgränsare eller mer) liten, oberoende och isolerade enheter av tillstånd och logik.
* Vill du toowork med Enkeltrådig objekt som inte kräver betydande interaktion från externa komponenter, inklusive frågor tillstånd på en uppsättning aktörer.
* Aktören-instanser blockera inte anropare med oväntade fördröjningar genom att utfärda o-åtgärder.

## <a name="actors-in-service-fabric"></a>Aktörer i Service Fabric
I Service Fabric aktörer implementeras i hello Reliable Actors framework: ett programramverk för aktören-mönster-baserade byggt ovanpå [Service Fabric Reliable Services](service-fabric-reliable-services-introduction.md). Varje tillförlitliga aktören-tjänst som du skriver är faktiskt en partitionerad tillståndskänslig tillförlitlig tjänst.

Varje aktören har definierats som en instans av en aktörstyp av, identiska toohello sätt en .NET-objekt är en instans av en .NET-typen. Till exempel kan det finnas en aktörstyp som implementerar hello funktionerna i en kalkylator och det kan finnas flera aktörer av den typen som distribueras på olika noder i ett kluster. Varje sådan aktören identifieras unikt genom ett aktören-ID.

### <a name="actor-lifetime"></a>Aktören livslängd
Service Fabric aktörer är virtuell, vilket innebär att deras livstid inte är bundet tootheir InMemory-representation. Därför kan behöver de inte toobe explicit skapas eller förstörs. hello Reliable Actors runtime aktiveras automatiskt en aktören hello första gången som den tar emot en begäran om att aktören-ID. Om en aktör inte används för en viss tidsperiod, hello Reliable Actors runtime skräp-samlar in hello InMemory-objektet. Den kommer också upprätthålla kunskap om hello aktören förekomsten ska måste toobe igen senare. Mer information finns i [aktören livscykel och skräp samling](service-fabric-reliable-actors-lifecycle.md).

Denna virtuella aktören livstid framställning har vissa varningar på grund av hello virtuella aktören modellen och faktum hello Reliable Actors implementering avviker ibland från den här modellen.

* En aktör aktiveras automatiskt (och en aktör objektet toobe konstrueras) hello första gången ett meddelande skickas tooits aktören-ID. Efter en viss tidsperiod samlas hello aktören objekt in som skräp. I hello orsakar framtiden kan använda hello aktörs-ID igen, en ny aktören objektet toobe konstrueras. En aktörstillstånd användbara under begränsad livslängd hello objekt när lagras i hello tillståndshanterare.
* Anropar någon aktören för aktörs-ID aktiverar att aktören. Därför har aktören typer sin konstruktor anropas implicit av hello runtime. Därför går inte att klientkod ange parametrarna toohello aktören typens konstruktor, även om parametrar kan överföras toohello aktören konstruktorn av själva hello-tjänsten. hello resultatet är att aktörer kan konstrueras i en delvis initierats tillstånd med hello gång andra metoder anropas, om hello aktören kräver initieringsparametrar hello-klient. Det finns ingen enskild startpunkt för hello aktivering av en aktör hello-klient.
* Även om Reliable Actors skapa implicit aktören objekt. du har hello möjlighet tooexplicitly ta bort en aktör och dess tillstånd.

### <a name="distribution-and-failover"></a>Distribution och växling vid fel
tooprovide skalbarhet och tillförlitlighet, Service Fabric distribuerar aktörer i hela klustret hello och automatiskt migrerar dem från felande noder toohealthy dem efter behov. Detta är en abstraktion via en [partitionerade och tillståndskänsliga tillförlitlig tjänst](service-fabric-concepts-partitioning.md). Distribution, skalbarhet, tillförlitlighet och automatisk redundans har angetts tack vare hello fakta som körs inom en tillståndskänslig tillförlitlig tjänst som kallas hello aktörer *aktören tjänsten*.

Aktörer är fördelade på hello partitioner i hello aktören Service och dessa partitioner är fördelade på hello noder i ett Service Fabric-kluster. Varje tjänst partition innehåller en uppsättning aktörer. Service Fabric hanterar distribution och redundans för hello service partitioner.

Till exempel distribuera en aktören tjänsten med nio partitioner toothree noder som använder hello standardplacering aktören partition skulle distribueras thusly:

![Tillförlitliga aktörer distribution][2]

hello aktören Framework hanterar schema och nyckeln intervallet partitionsinställningar för dig. Detta förenklar vissa alternativ men har viss planering:

* Reliable Services kan du toochoose en partitioneringsschema viktiga intervallet (när du använder ett intervall som partitioneringsschema) och partition count. Reliable Actors kräver att du använder hello Int64 viktiga komplett är begränsad toohello intervallet partitioneringsschema (hello uniform Int64 schemat).
* Som standard placeras slumpmässigt aktörer partitioner, vilket resulterar i jämn fördelning.
* Eftersom aktörer placeras slumpmässigt, bör det förväntas att aktören operations alltid kommer att kräva nätverkskommunikation, inklusive serialisering och deserialisering av metoden anropsdata, medför latens och kostnader.
* I avancerade scenarier är möjliga toocontrol aktören partition placering med hjälp av Int64 aktören ID: N som mappar toospecific partitioner. Dock kan göra så resultera i ett Obalanserat fördelning av aktörer över partitioner.

Mer information om hur aktörstjänster partitioneras finns för[partitionering begrepp för aktörer](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-actors).

### <a name="actor-communication"></a>Aktören kommunikation
Aktören interaktioner definieras i ett gränssnitt som delas av hello aktören som implementerar gränssnittet hello och hello-klient som hämtar en proxy tooan aktören via hello samma gränssnitt. Eftersom det här gränssnittet är används tooinvoke aktören metoder asynkront, måste varje metod i hello gränssnitt vara åtgärdsreturnerande.

Anrop av metoden och deras svar slutligen ger nätverksbegäranden över hello klustret, så hello argument och hello resultattyper hello uppgifter som att de returnerar måste kunna serialiseras av hello-plattformen. I synnerhet de måste vara [data minimera serialiserbara](service-fabric-reliable-actors-notes-on-actor-type-serialization.md).

#### <a name="hello-actor-proxy"></a>hello aktören proxy
hello Reliable Actors klienten API ger kommunikation mellan en aktören-instans och en aktören-klient. toocommunicate med en aktör en klient skapar ett aktören proxy-objekt som implementerar hello aktören gränssnittet. hello klienten kommunicerar med hello aktören genom att anropa metoder på hello proxy-objekt. hello aktören proxy kan användas för kommunikation i klient – aktören och aktören – aktören.

```csharp
// Create a randomly distributed actor ID
ActorId actorId = ActorId.CreateRandom();

// This only creates a proxy object, it does not activate an actor or invoke any methods yet.
IMyActor myActor = ActorProxy.Create<IMyActor>(actorId, new Uri("fabric:/MyApp/MyActorService"));

// This will invoke a method on hello actor. If an actor with hello given ID does not exist, it will be activated by this method call.
await myActor.DoWorkAsync();
```

```java
// Create actor ID with some name
ActorId actorId = new ActorId("Actor1");

// This only creates a proxy object, it does not activate an actor or invoke any methods yet.
MyActor myActor = ActorProxyBase.create(actorId, new URI("fabric:/MyApp/MyActorService"), MyActor.class);

// This will invoke a method on hello actor. If an actor with hello given ID does not exist, it will be activated by this method call.
myActor.DoWorkAsync().get();
```


Observera att hello två typer av information som används toocreate hello aktören proxyobjekt är hello aktörs-ID och hello programnamn. hello aktörs-ID identifierar hello aktören medan hello programnamn identifierar hello [Service Fabric application](service-fabric-reliable-actors-platform.md#application-model) där hello aktören har distribuerats.

Hej `ActorProxy`(C#) / `ActorProxyBase`klass (Java) på klientsidan för hello utför hello nödvändiga upplösning toolocate hello aktören-ID: t och öppna en kommunikationskanal till den. Den försöker också toolocate hello aktören hello gäller kommunikationsfel och växling vid fel. Därför har meddelandeleverans hello följande egenskaper:

* Meddelandeleverans är bästa prestanda.
* Aktörer får dubbla meddelanden från hello samma klient.

### <a name="concurrency"></a>Samtidighet
hello Reliable Actors runtime ger en enkel bygger åtkomst-modell för att komma åt aktören metoder. Det innebär att mer än en tråd kan vara aktiva i objektets aktören kod när som helst. Stäng-baserad åtkomst förenklar samtidiga system som det ingen behövs synkroniseringsmekanismer för dataåtkomst. Det innebär också måste vara konstruerade med speciella överväganden vid hello Enkeltrådig åtkomst natur för varje aktören-instans.

* En enda aktören-instans kan inte bearbeta fler än en begäran i taget. En instans av aktören kan orsaka en flaskhals för genomströmning om är det förväntade toohandle samtidiga begäranden.
* Aktörer kan låsas i varandra om det finns en cirkulär begäran mellan två aktörer medan en extern begäran görs tooone hello aktörer samtidigt. hello aktören runtime kommer automatiskt tid ut på aktören anropar och utlösa ett undantag toohello anroparen toointerrupt möjligt deadlock situationer.

![Tillförlitlig aktörer kommunikation][3]

#### <a name="turn-based-access"></a>Stäng-baserad åtkomst
En tur består av hello fullständig körningen av en aktören metod i svaret tooa begäran från andra aktörer eller klienter eller hello fullständig körningen av en [timer/påminnelse](service-fabric-reliable-actors-timers-reminders.md) återanrop. Även om dessa metoder och återanrop asynkrona, interleave dem inte av hello aktörer runtime. En Stäng måste vara helt färdiga innan en ny Stäng tillåts. Med andra ord ett aktören metod eller timer/påminnelse motanrop som körs för tillfället måste vara helt färdiga innan en ny anropet tooa metod eller återanrop tillåts. En metod eller ett återanrop anses toohave klar om hello körningen har returnerats från hello-metoden eller motringning och hello aktiviteten som returneras av metoden hello eller återanrop har slutförts. Det är värt som betonar som bygger samtidighet följs även över olika metoder och timers återanrop.

hello aktörer runtime tvingar bygger samtidighet genom att skaffa ett lås per aktör hello början av en tur och släppa hello Lås hello slutet av hello aktivera. Därför tillämpas bygger samtidighet på grundval av per aktör och inte över aktörer. Aktören metoder och timer/påminnelse återanrop kan köra samtidigt för olika aktörer.

hello följande exempel illustrerar hello ovan begrepp. Överväg ett aktörstyp som implementerar två asynkrona metoder (exempelvis *metod1* och *metod2*), en timer och en påminnelse. hello diagrammet nedan visar ett exempel på en tidslinje för hello körning av dessa metoder och återanrop för två aktörer (*ActorId1* och *ActorId2*) som tillhör toothis aktören typen.

![Tillförlitliga aktörer runtime bygger samtidighet och åtkomst][1]

Det här diagrammet följer dessa regler:

* Varje lodrät linje visar hello logiska flödet för körning av en metod eller ett återanrop för en viss aktören.
* hello händelser markerats på varje lodrät linje i kronologisk ordning med nyare händelser som inträffar under äldre filer.
* Olika färger används för tidslinjer motsvarande toodifferent aktörer.
* Markeringen är används tooindicate hello varaktighet för vilka hello lås per aktör utförs åt en metod eller ett återanrop.

Några viktiga punkter tooconsider:

* Medan *metod1* körs på uppdrag av *ActorId2* i svaret tooclient begäran *xyz789*, en annan klientbegäran (*abc123*) anländer som kräver också *metod1* toobe som körs av *ActorId2*. Dock hello andra körningen av *metod1* börjar inte förrän hello tidigare körningen har slutförts. På samma sätt kan en påminnelse registreras av *ActorId2* utlöses när *metod1* som körs i svaret tooclient begäran *xyz789*. hello påminnelse återanrop körs förrän både körningar av *metod1* har slutförts. Allt detta är på grund av tooturn-baserade samtidighet som tillämpas för *ActorId2*.
* På samma sätt tillämpas även bygger samtidighet för *ActorId1*, vilket framgår av hello körningen av *metod1*, *metod2*, och hello timer återanrop för *ActorId1* som händer i ett seriella sätt.
* Körningen av *metod1* för *ActorId1* överlappar körningen för *ActorId2*. Det beror på att bygger samtidighet tillämpas endast inom en aktör och inte över aktörer.
* I vissa hello metoden/återanrop körningar hello `Task`(C#) / `CompletableFuture`(Java) returnerades av hello metoden/återanrop slutförs efter hello-metoden returnerar. I vissa andra har hello asynkron åtgärd redan gått ut av hello tid hello metoden/återanrop returnerar. I båda fallen släpps lås per aktör för hello endast när båda hello metoden/återanrop returnerar och hello asynkrona åtgärden har slutförts.

#### <a name="reentrancy"></a>Återinträde
hello aktörer körning kan återinträde som standard. Det innebär att om en aktörsmetod av *aktören A* anropar en metod i *aktören B*, som i sin tur anropar en annan metod på *aktören A*,-metoden tillåts toorun. Detta beror på att den är del av hello samma logiska anrop-kedjan kontext. Alla timer- och påminnelse anrop starta med hello nya logiska anropet kontext. Se hello [Reliable Actors återinträde](service-fabric-reliable-actors-reentrancy.md) för mer information.

#### <a name="scope-of-concurrency-guarantees"></a>Omfattning samtidighet garantier
hello aktörer runtime tillhandahåller dessa samtidighet garantier i situationer där den styr hello anrop av metoderna. Till exempel ger dessa garantier för hello metoden anrop som görs i svaret tooa klientbegäran samt timer och påminnelser. Men om hello aktören koden anropar direkt metoderna utanför hello-metoder som tillhandahålls av hello aktörer runtime, går inte att hello runtime ange samtidighet garantier. Till exempel om hello-metoden har anropats hello kontexten för en uppgift som inte är associerad med hello aktiviteten som returneras av hello aktören metoder går inte att hello runtime ange samtidighet garantier. Om hello-metoden anropas från en tråd som hello aktören skapar själv och sedan hello runtime kan också ge samtidighet garantier. Tooperform bakgrundsåtgärder aktörer bör därför använda [aktören timers och aktören påminnelser](service-fabric-reliable-actors-timers-reminders.md) som respekterar bygger samtidighet.

## <a name="next-steps"></a>Nästa steg
* Kom igång genom att skapa din första Reliable Actors-tjänst:
   * [Komma igång med Reliable Actors på .NET](service-fabric-reliable-actors-get-started.md)
   * [Komma igång med Reliable Actors Java](service-fabric-reliable-actors-get-started-java.md)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-introduction/concurrency.png
[2]: ./media/service-fabric-reliable-actors-introduction/distribution.png
[3]: ./media/service-fabric-reliable-actors-introduction/actor-communication.png
