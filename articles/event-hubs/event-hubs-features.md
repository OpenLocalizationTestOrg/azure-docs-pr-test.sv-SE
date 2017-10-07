---
title: "aaaAzure Händelsehubbar funktioner översikt | Microsoft Docs"
description: "Översikt över och information om Händelsehubbar i Azure-funktioner"
services: event-hubs
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2017
ms.author: sethm
ms.openlocfilehash: 8094e48abc8455ed725d4d5d3f9895f431441e2b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="event-hubs-features-overview"></a>Översikt över Event Hubs funktioner

Händelsehubbar i Azure är en skalbar händelsebearbetning tjänsten som en och bearbetar stora mängder händelser och data med låg latens och hög tillförlitlighet. Se [vad är Händelsehubbar?](event-hubs-what-is-event-hubs.md) för en översikt över hello-tjänsten.

Den här artikeln bygger på hello informationen i hello [översikt](event-hubs-what-is-event-hubs.md), och ger information om Händelsehubbar komponenter och funktioner i technical och implementering.

## <a name="event-publishers"></a>Händelseutfärdare

En enhet som skickar data tooan händelsehubb är en händelse tillverkare eller *händelseutfärdare*. Händelseutfärdare kan utfärda händelser med hjälp av HTTPS eller AMQP 1.0. Händelseutfärdare kan använder en delad signatur åtkomst (SAS) token tooidentify själva tooan händelsehubb, och ha en unik identitet, eller använda en vanlig SAS-token.

### <a name="publishing-an-event"></a>Publicera en händelse

Du kan publicera en händelse via AMQP 1.0 eller HTTPS. Händelsehubbar ger [klientbibliotek och klasser](event-hubs-dotnet-framework-api-overview.md) för att publicera händelser tooan händelsehubb från .NET-klienter. För andra körningar och plattformar kan du använda alla AMQP 1.0-klienter, t.ex. [Apache Qpid](http://qpid.apache.org/). Du kan publicera händelser individuellt eller i batchar. En enstaka publikation (en instans av händelsedata ) har en begränsning på 256 KB, oavsett om det är en enskild händelse eller en batch. Publicera händelser som är större än tröskelvärdet resultatet i ett fel. Det är bästa praxis för utgivare toobe ovetande om partitioner i händelsehubben hello och tooonly anger en *partitionsnyckel* (som introducerades i hello nästa avsnitt) eller sin identitet via sin SAS-token.

hello val toouse AMQP eller HTTPS är specifikt toohello användningsscenariot. AMQP kräver hello upprättande av en beständig dubbelriktad socket i tillägg tootransport säkerhet på radnivå (TLS) eller SSL/TLS. AMQP har högre nätverkskostnader för vid initieringen hello session, men HTTPS kräver ytterligare SSL-omkostnader för varje begäran. AMQP har högre prestanda för frekventa utfärdare.

![Händelsehubbar](./media/event-hubs-features/partition_keys.png)

Händelsehubbar garanterar att alla händelser som delar en partitionsnyckelvärde levereras i ordning och toohello samma partition. Om partitionsnycklar används med utfärdarprinciper, hello hello utgivarens identitet och hello värdet för hello Partitionsnyckeln matcha. Annars uppstår ett fel.

### <a name="publisher-policy"></a>Utgivarprincip

Med händelsehubbar får du granulär kontroll över utgivare via *utgivarprinciper*. Utgivarprinciper är körningen funktioner som designats toofacilitate stort antal oberoende utgivare. Med utgivarprinciper använder varje utgivare sin egen unika identifierare vid publicering av händelser tooan händelsehubb, med hjälp av följande mekanism hello:

```
//[my namespace].servicebus.windows.net/[event hub name]/publishers/[my publisher name]
```

Du har inte toocreate utgivarnamnen i förväg, men de måste matcha hello SAS-token som används när du publicerar en händelse i ordning tooensure oberoende utgivaridentiteter. När du använder utgivarprinciper hello **PartitionKey** värdet toohello utgivarens namn. toowork korrekt måste dessa värden måste matcha.

## <a name="capture"></a>Capture

[Event Hubs avbilda](event-hubs-capture-overview.md) kan du tooautomatically avbilda hello strömmande data i Händelsehubbar och spara den tooyour valet av ett Blob storage-konto eller ett tjänstkonto för Azure Data Lake. Du kan aktivera insamlingen från hello Azure-portalen och ange minsta storlek och tid fönstret tooperform hello avbildning. Med Event Hubs avbilda kan du ange egna Azure Blob Storage-konto och en behållare eller Azure Data Lake Service-konto som har använt toostore hello infångade data. Insamlade data skrivs i hello Apache Avro-formatet.

## <a name="partitions"></a>Partitioner

Händelsehubbar ger meddelandeströmmar via ett partitionerat konsumentmönster där varje konsument bara läser en viss delmängd, eller partition, av meddelandeströmmen hello. Det här mönstret gör det möjligt att skala horisontellt för händelsebearbetning och tillhandahåller andra strömfokuserade funktioner som inte är tillgängliga i köer och ämnen.

En partition är en ordnad sekvens av händelser som hålls kvar i en händelsehubb. När nya händelser anländer läggs de toohello slutet av denna sekvens. En partition kan betraktas som en ”genomförandelogg”.

![Händelsehubbar](./media/event-hubs-features/partition.png)

Händelsehubbar behåller data under en konfigurerad kvarhållningstid som gäller för alla partitioner i hello händelsehubb. Händelser löper ut enligt ett tidsschema. Det går inte att ta bort dem. Eftersom partitioner är oberoende av varandra och innehåller sina egna sekvenser med data, växer de ofta i olika takt.

![Händelsehubbar](./media/event-hubs-features/multiple_partitions.png)

hello antalet partitioner anges när skapas och måste vara mellan 2 och 32. Hej partitionsantal är inte kan ändras, så bör du långsiktiga skala när antalet partitioner. Partitioner är en mekanism för organisation av data som rör toohello nedströms Parallellism som krävs inom användarprogram. hello antalet partitioner i en händelsehubb direkt relaterat toohello nummer för samtidiga läsare du förväntar dig toohave. Du kan öka hello antalet partitioner utöver 32 genom att kontakta hello Händelsehubbar team.

Du rekommenderas att inte skicka direkt tooa partition medan partitioner kan identifieras och kan skickas toodirectly. I stället kan du använda högre nivå konstruktioner som introducerades i hello [händelseutfärdare](#event-publishers) och [kapacitet](#capacity) avsnitt. 

Partitioner är fyllda med en sekvens av händelsedata som innehåller hello händelse hello brödtext, en användardefinierad egenskapsuppsättning och metadata, till exempel dess offset i hello partitionen och dess nummer i dataströmsekvensen hello.

Mer information om partitioner och hello säkerhetsaspekten tillgänglighet och tillförlitlighet finns hello [Programmeringsguide för Händelsehubbar](event-hubs-programming-guide.md#partition-key) och hello [tillgänglighet och konsekvens i Händelsehubbar](event-hubs-availability-and-consistency.md) artikel .

### <a name="partition-key"></a>Partitionsnyckeln

Du kan använda en [partitionsnyckel](event-hubs-programming-guide.md#partition-key) toomap inkommande händelsedata med specifika partitioner för hello syftet organisation av data. hello Partitionsnyckeln är ett värde som avsändaren anger skickades till en händelsehubb. Det bearbetas via en statisk hash-funktion, vilket skapar hello tilldelning av partitionen. Om du inte anger en partitionsnyckel när du publicerar en händelse, används en tilldelning enligt resursallokeringsmodellen.

Hej händelseutfärdaren bara medveten om sin partitionsnyckel, inte hello partition toowhich hello händelserna publiceras. Frikopplingen av nyckeln och partitionen gör hello avsändaren inte behöver tooknow så mycket om hello nedströms bearbetning. En per enhet eller en användarunik identitet utgör en bra partitionsnyckel, men andra attribut, till exempel Geografi kan också använda toogroup relaterade händelser i en enda partition.

## <a name="sas-tokens"></a>SAS-token

Händelsehubbar använder *signaturer för delad åtkomst*, som är tillgängliga på nivån för hello namnområde och händelse-hubb. En SAS-token genereras från en SAS-nyckel och är en SHA-hash för en URL som kodats i ett specifikt format. Med hello namn hello nyckeln (principen) och hello token Händelsehubbar återskapa hello hash och därmed autentisera avsändaren hello. SAS-token för händelseutfärdare skapas vanligtvis med enbart behörighet för att **skicka** i en specifik händelsehubb. Den här token URL-mekanismen med SAS är hello grund för utfärdaridentifiering som presenterades i principen för hello utgivare. Mer information om hur du arbetar med SAS finns i [autentisering med signatur för delad åtkomst med Service Bus](../service-bus-messaging/service-bus-sas.md).

## <a name="event-consumers"></a>Händelsekonsumenter

En enhet som läser händelsedata från en händelsehubb är en *händelsekonsument*. Alla konsumenter i Händelsehubbar ansluter via hello AMQP 1.0-session och händelser levereras via hello session när de blir tillgängliga. hello klienten behöver inte toopoll efter datatillgänglighet.

### <a name="consumer-groups"></a>Konsumentgrupper

hello Publicera/prenumerera mekanismen för Händelsehubbar aktiveras via *konsumentgrupper*. En konsumentgrupp är en vy (tillstånd, position eller offset) av en hel händelsehubb. Konsumentgrupper aktivera flera användningsprogram tooeach har en separat vy över händelseströmmen hello och tooread hello dataströmmen oberoende i sin egen takt och med sina egna förskjuts.

Varje nedströms program är lika tooa konsumentgrupp i en arkitektur för bearbetning av dataströmmen. Om du vill toowrite händelse toolong termen datalagring är utgör programmet för att en konsumentgrupp. Komplex händelsebearbetning kan sedan utföras av en annan, separat konsumentgrupp. Du får bara åtkomst till en partition via en konsumentgrupp. Det får finnas högst 5 läsare på en partition per konsumentgrupp; men **rekommenderas att det finns endast en aktiv mottagaren på en partition per konsumentgrupp**. Det finns alltid en förinställd konsumentgrupp i en händelsehubb, och du kan skapa upp too20 konsumentgrupper för en händelsehubb på standardnivå.

hello följande är exempel på URI-konventionen hello konsumenten grupp:

```http
//[my namespace].servicebus.windows.net/[event hub name]/[Consumer Group #1]
//[my namespace].servicebus.windows.net/[event hub name]/[Consumer Group #2]
```

hello visar följande bild hello Händelsehubbar dataströmmen bearbetning arkitektur:

![Händelsehubbar](./media/event-hubs-features/event_hubs_architecture.png)

### <a name="stream-offsets"></a>Offsets för strömmar

En *offset* är hello positionen för en händelse inom en partition. Föreställ dig en offset som en markör på klientsidan. hello offset är en byte numrering av hello-händelse. Denna offset kan en händelse konsumenten (läsare) toospecify en punkt i hello händelseströmmen som de vill toobegin läsa händelser. Du kan ange hello offset som en tidsstämpel eller offset-värde. Konsumenterna ansvarar för att lagra sina egna offset-värden utanför hello händelsehubbtjänsten. Inom en partition innehåller varje händelse en offset.

![Händelsehubbar](./media/event-hubs-features/partition_offset.png)

### <a name="checkpointing"></a>Kontrollpunkter

*Att skapa kontrollpunkter* är en process genom vilken läsare markerar eller sparar sin position inom en händelsesekvens i en partition. Kontrollpunkter är hello ansvar hello konsumenten och sker på grundval av per partition inom en konsumentgrupp. Det ansvaret innebär att för varje konsumentgrupp måste varje partition läsaren måste hålla reda på dess aktuella position i händelseströmmen hello och kan informera hello-tjänsten när de anser att hello dataströmmen är klar.

Om en läsare kopplar från en partition, när den sedan återansluts kan han börja läsa vid hello kontrollpunkt som tidigare skickades av hello senaste läsaren i den aktuella partitionen i just den konsumentgruppen. När hello läsaren ansluter skickar denna offset toohello händelse toospecify hello plats i navet på vilka toostart läsning. På så sätt kan du kan använda kontrollpunkter tooboth Markera händelser som ”Slutför” i nedströms program och tooprovide återhämtning om växling mellan läsare som körs på olika datorer sker. Det är möjligt tooreturn tooolder data genom att ange en lägre offset tack vare kontrollpunkterna. Den här mekanismen möjliggör både återhämtning vid redundansväxlingar och återuppspelning av händelseströmmar.

### <a name="common-consumer-tasks"></a>Vanliga konsumentuppgifter

Alla konsumenter i Händelsehubbar ansluter via en AMQP 1.0-session, en tillståndsmedveten dubbelriktad kommunikationskanal. Varje partition har en AMQP 1.0-session som underlättar hello transport av händelser som åtskiljs av partitioner.

#### <a name="connect-tooa-partition"></a>Ansluta tooa partition

När du ansluter toopartitions, är det vanliga praxis toouse en leasing mekanism toocoordinate reader anslutningar toospecific partitioner. På så sätt kan det är möjligt för varje partition i en konsument grupp toohave endast en aktiv läsare. Kontrollpunkter leasing och hantera läsare förenklas med hello [EventProcessorHost](/dotnet/api/microsoft.servicebus.messaging.eventprocessorhost) klassen för .NET-klienter. hello värd för händelsebearbetning är en intelligent konsumentagent.

#### <a name="read-events"></a>Läsa händelser

När en AMQP 1.0-session och -länk har öppnats för en specifik partition, levereras händelser toohello AMQP 1.0-klienten av hello händelsehubbtjänsten. Den här leveransmekanismen gör det möjligt med ett högre genomflöde och kortare svarstid än i pull-baserade mekanismer som t.ex. HTTP GET. När händelser skickas toohello klienten, innehåller varje instans av händelsedata viktiga metadata, till exempel hello offset- och sekvensnumret många som används toofacilitate skapa kontrollpunkter i händelsesekvensen hello.

Händelsedata:
* Offset
* Sekvensnummer
* Innehåll
* Användaregenskaper
* Systemegenskaper

Det är ditt ansvar toomanage hello förskjutning.

## <a name="capacity"></a>Kapacitet

Händelsehubbar har en mycket skalbar parallell arkitektur och det finns flera viktiga faktorer tooconsider när storleken på och skala.

### <a name="throughput-units"></a>Genomflödesenheter

Hej genomflödeskapaciteten i Händelsehubbar styrs av *genomflödesenheter*. Genomflödesenheter är färdiga kapacitetsenheter. En genomflödesenhet omfattar följande kapacitet hello:

* Ingång: upp too1 MB per sekund eller 1 000 händelser per sekund (beroende på vilket som kommer först)
* Utgång: upp too2 MB per sekund

Utöver hello kapaciteten på hello inköpta genomflödesenheter, ingången begränsas och en [ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) returneras. Utgående skapar inte begränsningsfel, men är fortfarande begränsad toohello kapacitet för hello inköpta genomflödesenheter. Om du får felmeddelanden om publiceringsfrekvensen eller förväntar dig större utgång för toosee vara säker på att toocheck hur många genomflödesenheter du har köpt för hello namnområde. Du kan hantera enheter hello **skala** bladet hello namnområden i hello [Azure-portalen](https://portal.azure.com). Du kan också hantera enheter via programmering med hello [Event Hubs API: er](event-hubs-api-overview.md).

Genomflödesenheter debiteras per timme och köps i förväg. När de väl har köpts debiteras de för minst en timme. Too20 genomflödesenheter kan köpas för ett namnområde för Händelsehubbar och är gemensamma för alla Händelsehubbar i hello namnområde.

Fler genomflödesenheter kan köpas i block på 20 too100 genomströmning enheter genom att kontakta Azure-supporten. Utöver det kan du också köpa block med 100 genomflödesenheter.

Vi rekommenderar att du balansera genomströmning enheter och partitioner tooachieve bästa skala. En enstaka partition har en maximal skala på en genomflödesenhet. hello antalet genomflödesenheter ska vara mindre än eller lika toohello antalet partitioner i en händelsehubb.

Detaljerad Händelsehubbar information om priser finns i [priser för Händelsehubbar](https://azure.microsoft.com/pricing/details/event-hubs/).

## <a name="next-steps"></a>Nästa steg

Mer information om Händelsehubbar finns i hello följande länkar:

* Kom igång med en [kurs om händelsehubbar][Event Hubs tutorial]
* [Programmeringsguide för Event Hubs](event-hubs-programming-guide.md)
* [Tillgänglighet och konsekvens i Event Hubs](event-hubs-availability-and-consistency.md)
* [Vanliga frågor och svar om Event Hubs](event-hubs-faq.md)
* [Event Hubs-exempel][]

[Event Hubs tutorial]: event-hubs-dotnet-standard-getstarted-send.md
[Event Hubs-exempel]: https://github.com/Azure/azure-event-hubs/tree/master/samples
