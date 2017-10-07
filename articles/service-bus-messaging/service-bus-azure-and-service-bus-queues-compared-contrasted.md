---
title: "aaaAzure lagringsköer och Service Bus-köer - jämfört med och skillnad från | Microsoft Docs"
description: "Analyserar likheter mellan två typer av köer som erbjuds av Azure och."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: tysonn
ms.assetid: f07301dc-ca9b-465c-bd5b-a0f99bab606b
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 08/07/2017
ms.author: sethm
ms.openlocfilehash: f8b915e73ea3c82d823a96bf23c8c9e24c96aa42
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="storage-queues-and-service-bus-queues---compared-and-contrasted"></a>Storage-köer och Service Bus-köer - skillnad från och med
Den här artikeln analyserar hello skillnader och likheter mellan hello två typer av köer som erbjuds av Microsoft Azure idag: lagringsköer och Service Bus-köer. Med den här informationen kan du jämföra och kontrast hello respektive teknik och vara kan toomake välgrundade beslut om vilken lösning som bäst uppfyller dina behov.

## <a name="introduction"></a>Introduktion
Azure stöder två typer av kön mekanismer: **lagringsköer** och **Service Bus-köer**.

**Lagringsköer**, som är en del av hello [Azure storage](https://azure.microsoft.com/services/storage/) infrastruktur, funktionen ett enkelt gränssnitt i REST-baserad GET/PUT/TITT att tillhandahålla tillförlitlig och beständiga messaging inom och mellan tjänster.

**Service Bus-köer** är en del av en bredare [Azure messaging](https://azure.microsoft.com/services/service-bus/) infrastruktur som stöder queuing samt publicera/prenumerera och mer avancerade integration mönster. Mer information om Service Bus-köer/artiklar/prenumerationer finns hello [översikt över Service Bus](service-bus-messaging-overview.md).

Medan båda queuing teknikerna samtidigt kan introducerades lagringsköer först som en mekanism för lagring av dedikerade kön byggda på Azure Storage-tjänster. Service Bus-köer är byggda på hello bredare ”meddelanden” infrastruktur som utformats för toointegrate program eller programkomponenter som kan sträcka sig över flera kommunikationsprotokoll, datakontrakt, betrodda domäner och nätverksmiljöer.

## <a name="technology-selection-considerations"></a>Överväganden för val av teknik
Både lagringsköer och Service Bus-köer är implementeringar av hello message queuing-tjänsten som för närvarande erbjuds via Microsoft Azure. Varje har ett något annorlunda funktionsuppsättningen, vilket innebär att du kan välja ett eller hello andra eller använda både och, beroende på hello behoven hos din lösning eller business-tekniska problem du lösa.

När du fastställer vilka queuing teknik hello syfte för en viss lösning som passar bör lösningsarkitekter och utvecklare hello rekommendationer nedan. Mer information finns i hello nästa avsnitt.

Som lösning systemarkitekt/utvecklare, **bör du använda lagringsköer** när:

* Programmet måste lagra över 80 GB meddelanden i en kö, där hälsningsmeddelande har en livslängd som är kortare än sju dagar.
* Programmet vill tootrack pågår för bearbetning av ett meddelande i hello kön. Detta är användbart om hello worker bearbetning av ett meddelande kraschar. En efterföljande arbetare kan sedan använda denna information toocontinue från där hello tidigare worker slutade.
* Du behöver serverloggen sida av alla hello transaktioner körs mot din köer.

Som lösning systemarkitekt/utvecklare, **bör du använda Service Bus-köer** när:

* Lösningen måste vara kan tooreceive meddelanden utan toopoll hello kön. Med Service Bus kan göra detta via hello användning av hello lång-avsökning mottagningsåtgärd med hello TCP-baserat protokoll som stöds i Service Bus.
* Lösningen kräver hello kön tooprovide en garanterad första-i-first out (FIFO) sorterade leverans.
* Vill du en symmetrisk upplevelse i Azure och Windows Server (privat moln). Mer information finns i [Service Bus för Windows Server](https://msdn.microsoft.com/library/dn282144.aspx).
* Lösningen måste vara kan toosupport automatisk identifiering av dubbletter.
* Programmet tooprocess-meddelanden som parallella tidskrävande dataströmmar (meddelanden som är associerade med en dataström med hjälp av hello [SessionId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.sessionid?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_BrokeredMessage_SessionId) egenskapen hello-meddelande). I den här modellen konkurrerar varje nod i förbrukning av programmet hello om strömmar, som skillnad från toomessages. När en dataström gavs tooa förbrukar nod granska hello nod hello tillstånd hello dataströmmen programtillståndet med transaktioner.
* Lösningen kräver transaktionella beteende och gör att skicka eller ta emot flera meddelanden från en kö.
* hello time to live (TTL) kännetecknar hello programspecifika arbetsbelastningen kan överskrida hello 7 dagar.
* Programmet hanterar meddelanden som kan överskrida 64 KB men sannolikt inte metoden hello 256 KB-gränsen.
* Du hantera ett krav tooprovide en rollbaserad åtkomst modellen toohello köer, och olika rights/behörigheter för avsändare och mottagare.
* Din köstorlek växer inte större än 80 GB.
* Vill du toouse hello AMQP 1.0 standardbaserad messaging-protokollet. Läs mer om AMQP [översikt över Service Bus AMQP](service-bus-amqp-overview.md).
* Du kan föreställa sig en eventuell migrering från kön-baserad Point-to-point-kommunikation tooa meddelande exchange mönster som möjliggör sömlös integration av ytterligare mottagare (prenumeranter), som tar emot oberoende kopior av vissa eller alla meddelanden skickas toohello kön. hello senare refererar toohello Publicera/prenumerera funktioner internt av Service Bus.
* Din meddelandelösning måste vara kan toosupport hello ”i de flesta – en gång” leverans garanti utan hello behöver du toobuild hello ytterligare infrastrukturkomponenter.
* Du skulle t.ex. toobe kan toopublish och använda batchar av meddelanden.

## <a name="comparing-storage-queues-and-service-bus-queues"></a>Jämföra lagringsköer och Service Bus-köer
hello tabeller i hello följande avsnitt innehåller en logisk gruppering av funktioner i kön och gör att du snabbt kan jämföra hello funktioner finns i både lagringsköer och Service Bus-köer.

## <a name="foundational-capabilities"></a>Grundläggande funktioner
Det här avsnittet jämför några av hello grundläggande queuing funktioner som tillhandahålls av lagringsköer och Service Bus-köer.

| Jämförelsevillkor | Lagringsköer | Service Bus-köer |
| --- | --- | --- |
| Ordning garanti |**Nej** <br/><br>Mer information finns i hello första anteckningen i hello ”ytterligare information”.</br> |**Ja - First i First Out (FIFO)**<br/><br>(via hello används av sessioner-meddelanden) |
| Leverans av säkerhet |**På-minst en gång** |**På-minst en gång**<br/><br/>**I de flesta – en gång** |
| Stöd för atomisk åtgärd |**Nej** |**Ja**<br/><br/> |
| Ta emot beteende |**Icke-blockerande**<br/><br/>(har slutförts omedelbart om inga nya meddelanden) |**Blockerar med och utan timeout**<br/><br/>(erbjuder länge avsökning eller hello [”Comet tekniken”](http://go.microsoft.com/fwlink/?LinkId=613759))<br/><br/>**Icke-blockerande**<br/><br/>(via hello användning av .NET hanterad API endast) |
| API för push-format |**Nej** |**Ja**<br/><br/>[OnMessage](/dotnet/api/microsoft.servicebus.messaging.queueclient.onmessage#Microsoft_ServiceBus_Messaging_QueueClient_OnMessage_System_Action_Microsoft_ServiceBus_Messaging_BrokeredMessage__) och **OnMessage** sessioner .NET-API. |
| Mottagningsläge |**Granska & låna ut** |**Granska & Lås**<br/><br/>**Ta bort & ta emot** |
| Exklusivt läge |**Lease-baserade** |**Lås-baserade** |
| / Utlämningslås varaktighet |**30 sekunder (standard)**<br/><br/>**7 dagar (max)** (du kan förnya eller släppa ett meddelande lån med hello [UpdateMessage](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.updatemessage.aspx) API.) |**60 sekunder (standard)**<br/><br/>Du kan förnya ett lås för meddelande med hello [RenewLock](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.renewlock#Microsoft_ServiceBus_Messaging_BrokeredMessage_RenewLock) API. |
| / Utlämningslås precision |**Meddelandenivå**<br/><br/>(varje meddelande kan ha ett annat timeout-värde som du kan sedan uppdatera som behövs under bearbetning av hello-meddelande med hjälp av hello [UpdateMessage](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.updatemessage.aspx) API) |**Kön nivå**<br/><br/>(varje kö har ett lås precision tillämpas tooall av meddelanden, men du kan förnya hello Lås med hello [RenewLock](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.renewlock#Microsoft_ServiceBus_Messaging_BrokeredMessage_RenewLock) API.) |
| Batchar ta emot |**Ja**<br/><br/>(explicit anger antalet meddelanden vid hämtning av meddelanden, upp tooa högst 32 meddelanden) |**Ja**<br/><br/>(implicit att aktivera en före fetch-egenskap eller uttryckligen via hello användning av transaktioner) |
| Batch skicka |**Nej** |**Ja**<br/><br/>(via hello användning av transaktioner eller batchbearbetning på klientsidan) |

### <a name="additional-information"></a>Ytterligare information
* Meddelanden i lagringsköer är vanligtvis första-i-first out, men de kan ibland vara oordning; till exempel när ett meddelande visas timeout-varaktighet upphör att gälla (t.ex, på grund av ett klientprogram kraschar under bearbetning). När tidsgränsen för visning av hello upphör att gälla hello-meddelande är synlig igen på hello kön för en annan worker toodequeue den. Vid den punkten nyligen synliga hello-meddelande kan placeras i kö för hello (toobe togs bort från kön igen) efter ett meddelande som ursprungligen köas efter den.
* hello garanteras FIFO mönster i Service Bus-köer kräver hello messaging-sessioner. Hello händelse som hello programmet kraschar vid bearbetning av ett meddelande som togs emot i hello **Granska & Lås** läge, hello nästa gång en kö mottagare tar emot en session för, startas med misslyckade hälsningsmeddelande efter dess Time to live (TTL) period har löpt ut.
* Storage-köer är utformad toosupport standard queuing scenarier, till exempel Frikoppling programmet komponenter tooincrease skalbarhet och tolerans för fel, Läs in Utjämning och utveckling av processarbetsflöden.
* Service Bus-köer stöder hello *på-minst en gång* leverans av garanti. Dessutom hello *på de flesta – en gång* semantiska kan användas med hjälp av tillstånd toostore hello programmet sessionstillstånd och ta emot meddelanden med hjälp av transaktioner tooatomically och uppdatera hello sessionstillstånd.
* Lagringsköer ger en enhetlig och konsekvent programmeringsmodell över köer, tabeller och Blobbar – både för utvecklare och team.
* Service Bus-köer ger stöd för lokala transaktioner i hello kontexten för en enskild kö.
* Hej **ta emot och ta bort** läge som stöds av Service Bus innehåller hello möjlighet tooreduce hello messaging antal (och associerade kostnaden) mot nedsänkt leverans säkerhet.
* Lagringsköer tillhandahålla lån med hello möjlighet tooextend hello lån för meddelanden. Detta gör att hello arbetare toomaintain kort lån på meddelanden. Därför om en worker kraschar kan hello-meddelande snabbt bearbetas igen med en annan worker. Dessutom kan en arbetare utöka hello lån på ett meddelande om behöver tooprocess den längre än hello aktuella låna tid.
* Lagringsköer erbjuder en synlighet tidsgräns som du kan ange vid hello skulle köas eller mellan köer för ett meddelande. Dessutom kan du uppdatera ett meddelande med olika lån värden under körning och uppdatera olika värden över meddelanden i hello samma kö. Service Bus lås-timeout definieras i hello kön metadata; men du kan förnya hello Lås genom att anropa hello [RenewLock](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.renewlock#Microsoft_ServiceBus_Messaging_BrokeredMessage_RenewLock) metod.
* hello Maximal tidsgräns är för en spärr mottagningsåtgärd i Service Bus-köer 24 dagar. REST-baserad timeout har dock ett maximalt värde 55 sekunder.
* Klientsidans batchbearbetning tillhandahålls av Service Bus gör en kö klienten toobatch flera meddelanden till en enda send-åtgärd. Batchbearbetning är endast tillgängligt för asynkron sändning åtgärder.
* Funktioner, till exempel hello 200 TB taket lagringsköer (Mer information när du virtualisera konton) och obegränsade köer gör det en utmärkt plattform för SaaS-providers.
* Storage-köer ger en flexibel och performant delegerad åtkomstkontroll.

## <a name="advanced-capabilities"></a>Avancerade funktioner
Det här avsnittet jämför avancerade funktioner som tillhandahålls av lagringsköer och Service Bus-köer.

| Jämförelsevillkor | Lagringsköer | Service Bus-köer |
| --- | --- | --- |
| Schemalagd leverans |**Ja** |**Ja** |
| Automatisk döda lettering |**Nej** |**Ja** |
| Öka kön time to live-värde |**Ja**<br/><br/>(via uppdatering på plats av synlighet tidsgräns) |**Ja**<br/><br/>(som tillhandahålls via en dedikerad API-funktion) |
| Skadligt meddelande stöd |**Ja** |**Ja** |
| Uppdatering på plats |**Ja** |**Ja** |
| Transaktionsloggen för serversidan |**Ja** |**Nej** |
| Storage-mätvärden |**Ja**<br/><br/>**Minuters mått**: innehåller realtid mått för tillgänglighet, Transaktionsprogram, API antal fel antal och mer i realtid (sammanställs per minut och rapporterats inom några minuter från vad hände bara i produktion. Mer information finns i [om Storage Analytics mätvärden](/rest/api/storageservices/fileservices/About-Storage-Analytics-Metrics). |**Ja**<br/><br/>(Massredigera frågor genom att anropa [GetQueues](/dotnet/api/microsoft.servicebus.namespacemanager.getqueues#Microsoft_ServiceBus_NamespaceManager_GetQueues)) |
| Hantering av tillstånd |**Nej** |**Ja**<br/><br/>[Microsoft.ServiceBus.Messaging.EntityStatus.Active](/dotnet/api/microsoft.servicebus.messaging.entitystatus.active), [Microsoft.ServiceBus.Messaging.EntityStatus.Disabled](/dotnet/api/microsoft.servicebus.messaging.entitystatus.disabled), [Microsoft.ServiceBus.Messaging.EntityStatus.SendDisabled](/dotnet/api/microsoft.servicebus.messaging.entitystatus.senddisabled), [Microsoft.ServiceBus.Messaging.EntityStatus.ReceiveDisabled](/dotnet/api/microsoft.servicebus.messaging.entitystatus.receivedisabled) |
| Automatisk vidarebefordring av meddelande |**Nej** |**Ja** |
| Rensa kön funktion |**Ja** |**Nej** |
| Meddelande-grupper |**Nej** |**Ja**<br/><br/>(via hello används av sessioner-meddelanden) |
| Programtillstånd per meddelande grupp |**Nej** |**Ja** |
| Identifiering av dubbletter |**Nej** |**Ja**<br/><br/>(kan konfigureras på hello avsändaren sida) |
| Bläddra meddelandet grupper |**Nej** |**Ja** |
| Hämtar sessioner för meddelande-ID: t |**Nej** |**Ja** |

### <a name="additional-information"></a>Ytterligare information
* Båda teknikerna för meddelandeköer kan ett meddelande toobe schemalagd vid ett senare tillfälle.
* Kön automatisk vidarebefordring kan tusentals köer tooauto vidarebefordring sina meddelanden tooa enskild kö, från vilken mottagande programmet hello förbrukar hello-meddelande. Du kan använda den här mekanismen tooachieve säkerhet, Kontrollflöde och isolera lagring mellan varje meddelande utgivare.
* Lagringsköer ger stöd för att uppdatera innehållet. Du kan använda den här funktionen för bestående statusinformation och inkrementell statusuppdateringar till hello-meddelande så att den kan bearbetas från hello senaste kända kontrollpunkten, i stället för att börja om från början. Med Service Bus-köer kan du aktivera hello samma scenario via hello meddelandet sessioner. Sessioner aktivera toosave och hämta hello programtillstånd bearbetning (med hjälp av [SetState](/dotnet/api/microsoft.servicebus.messaging.messagesession.setstate#Microsoft_ServiceBus_Messaging_MessageSession_SetState_System_IO_Stream_) och [GetState](/dotnet/api/microsoft.servicebus.messaging.messagesession.getstate#Microsoft_ServiceBus_Messaging_MessageSession_GetState)).
* [Döda oljekategori](service-bus-dead-letter-queues.md), vilket är endast stöds av Service Bus-köer kan vara användbart för att isolera meddelanden som inte kan bearbetas av hello mottagande programmet eller när meddelanden inte kan nå sina mål på grund av tooan har upphört att gälla Egenskapen time to live (TTL). hello TTL-värde som anger hur lång tid ett meddelande som finns kvar i hello kön. Med Service Bus kan hello-meddelande flyttade tooa särskilda kön anropas $DeadLetterQueue när hello TTL period har löpt ut.
* toofind ”skadligt” meddelanden i lagringsköer när mellan köer ett meddelande hello program undersöker hello  **[DequeueCount](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueuemessage.dequeuecount.aspx)**  -egenskapen för hello-meddelande. Om **DequeueCount** är större än ett visst tröskelvärde hello program flyttar hello meddelandekö tooan programdefinierade ”obeställbara”.
* Lagringsköer aktivera tooobtain en detaljerad logg över alla hello transaktioner körs mot hello kö som aggregeras mått. Båda dessa alternativ är användbara för felsökning och förstå hur programmet använder lagringsköer. De är också användbara för prestandajustering ditt program och minskar kostnaderna för hello med köer.
* hello begreppet ”delta” stöds av Service Bus gör att meddelanden som tillhör tooa vissa logisk grupp toobe som är associerade med en viss mottagare, vilket i sin tur skapar en session-liknande mappning mellan meddelanden och deras respektive mottagare. Du kan aktivera det avancerade funktioner i Service Bus genom att ange hello [SessionID](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.sessionid#Microsoft_ServiceBus_Messaging_BrokeredMessage_SessionId) egenskapen på ett meddelande. Mottagare kan lyssna på en specifik sessions-ID och ta emot meddelanden som delar hello angivna sessions-ID.
* hello duplicering identifiering funktioner som stöds av Service Bus-köer automatiskt tar bort dubbla meddelanden som skickas tooa kö eller ett ämne, baserat på hello värdet för hello [MessageId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.messageid#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId) egenskapen.

## <a name="capacity-and-quotas"></a>Kapacitet och kvoter
Det här avsnittet jämför lagringsköer och Service Bus-köer från hello perspektiv [kapacitet och kvoter](service-bus-quotas.md) som kan tillkomma.

| Jämförelsevillkor | Lagringsköer | Service Bus-köer |
| --- | --- | --- |
| Största köstorlek |**500 TB**<br/><br/>(begränsad tooa [enkel kapacitet för lagringskonton](../storage/common/storage-introduction.md#queue-storage)) |**1 GB too80 GB**<br/><br/>(definieras när du skapar en kö och [aktiverar partitionering](service-bus-partitioning.md) – Se hello ”ytterligare information”) |
| Maximal meddelandestorlek |**64 KB**<br/><br/>(48 KB när du använder **Base64** kodning)<br/><br/>Azure stöder stora meddelanden genom att kombinera köer och blobbar – nu kan du sätta upp too200GB för ett enskilt objekt. |**256 KB** eller **1 MB**<br/><br/>(inklusive både sidhuvud och brödtext, högsta huvudstorlek: 64 KB).<br/><br/>Beror på hello [tjänstnivån](service-bus-premium-messaging.md). |
| Maximal meddelande-TTL |**7 dagar** |**`TimeSpan.Max`** |
| Maximalt antal köer |**Obegränsat** |**10,000**<br/><br/>(per namnområde för tjänsten, kan du öka) |
| Maximalt antal samtidiga klienter |**Obegränsat** |**Obegränsat**<br/><br/>(100 samtidiga anslutningsgränsen gäller endast tooTCP protocol-baserad kommunikation) |

### <a name="additional-information"></a>Ytterligare information
* Service Bus tillämpar storleksbegränsningar för kön. hello största köstorlek har angetts vid skapande av hello kön och kan ha ett värde mellan 1 och 80 GB. Om hello kön storleksvärdet på skapande av hello kön uppnås ytterligare inkommande meddelanden avvisas och tas emot ett undantag av hello anropa kod. Mer information om kvoter i Service Bus finns [Service Bus kvoter](service-bus-quotas.md).
* I hello [standardnivån](service-bus-premium-messaging.md), kan du skapa Service Bus-köer i 1, 2, 3, 4 och 5 GB storlekar (hello standardvärdet är 1 GB). Du kan skapa köer in too80 GB i storlek i hello Premium-nivån. I Standard nivå med partitionering är aktiverad (vilket är hello standard), Service Bus skapar 16 partitioner för varje GB som du anger. Så om du skapar en kö är 5 GB i storlek med 16 partitioner hello största köstorlek blir (5 * 16) = 80 GB. Du kan se hello maximal storlek för din partitionerade kö eller ett ämne genom att titta på posten på hello [Azure-portalen][Azure portal]. Hej premiumnivån skapas endast 2 partitioner per kö.
* Med lagringsköer om hello innehållet i hello-meddelande är inte XML-säkert och det måste vara **Base64** kodad. Om du **Base64**-koda hello-meddelande, hello användaren nyttolasten kan vara upp too48 KB, i stället för 64 KB.
* Med Service Bus-köer, varje meddelande som lagras i en kö består av två delar: en rubrik och text. hello total storlek på hello-meddelande får inte överstiga högsta hälsningsmeddelande storlek som stöds av hello tjänstnivån.
* När klienterna kommunicerar med Service Bus-köer över hello TCP-protokollet, är begränsad too100 hello maximalt antal samtidiga anslutningar tooa enda Service Bus-kö. Det här antalet delas mellan avsändare och mottagare. Om den här kvoten har uppnåtts efterföljande begäranden om ytterligare anslutningar avvisas och tas emot ett undantag av hello anropa kod. Den här begränsningar inte på klienter som ansluter toohello köer med hjälp av REST-baserad API.
* Om du behöver fler än 10 000 köer i en enda Service Bus-namnrymd kan du begära en Kontakta hello Azure-teamet. tooscale utöver 10 000 köer med Service Bus kan du också skapa ytterligare namnområden med hello [Azure-portalen][Azure portal].

## <a name="management-and-operations"></a>Hantering och åtgärder
Det här avsnittet jämför hello hanteringsfunktioner som tillhandahålls av lagringsköer och Service Bus-köer.

| Jämförelsevillkor | Lagringsköer | Service Bus-köer |
| --- | --- | --- |
| Management-protokollet |**REST-via HTTP/HTTPS** |**PLACERAR över HTTPS** |
| Runtime-protokollet |**REST-via HTTP/HTTPS** |**PLACERAR över HTTPS**<br/><br/>**AMQP 1.0 Standard (TCP med TLS)** |
| .NET-API |**Ja**<br/><br/>(.NET lagring klient API) |**Ja**<br/><br/>.NET Service Bus (API) |
| Interna C++ |**Ja** |**Ja** |
| Java-API |**Ja** |**Ja** |
| PHP-API |**Ja** |**Ja** |
| Node.js API |**Ja** |**Ja** |
| Stöd för godtyckliga metadata |**Ja** |**Nej** |
| Kön namngivningsregler |**Konfigurera too63 tecken**<br/><br/>(Bokstäver i en kö måste vara gemener.) |**Konfigurera too260 tecken**<br/><br/>(Kön sökvägar och namn är skiftlägeskänsliga.) |
| Hämta kön längd funktion |**Ja**<br/><br/>(Ungefärliga värdet om meddelanden ut utöver hello TTL utan att tas bort.) |**Ja**<br/><br/>(Exakt, point-in-time-värde.) |
| Granska funktionen |**Ja** |**Ja** |

### <a name="additional-information"></a>Ytterligare information
* Lagringsköer ger stöd för valfria attribut som kan vara tillämpade toohello kön beskrivning i hello form av namn/värde-par.
* Båda teknikerna kön erbjuder hello möjlighet toopeek ett meddelande utan att behöva toolock IT-avdelningen, vilket kan vara användbart när du implementerar en kö webbläsaren och explorer-verktyget.
* hello Service Bus .NET asynkrona meddelanden API: er utnyttjar full duplex TCP-anslutningar för bättre prestanda när jämfört med tooREST över HTTP och stöd för standard hello AMQP 1.0-protokollet.
* Namnen på lagringsköer kan vara 3-63 tecken långt, får innehålla gemena bokstäver, siffror och bindestreck. Mer information finns i [namngivning av köer och Metadata](/rest/api/storageservices/fileservices/Naming-Queues-and-Metadata).
* Service Bus könamn kan in too260 tecken och har lägre namngivningsregler. Service Bus könamn kan innehålla bokstäver, siffror, punkter, bindestreck och understreck.

## <a name="authentication-and-authorization"></a>Autentisering och auktorisering
Det här avsnittet beskrivs hello autentisering och auktorisering funktioner som stöds av lagringsköer och Service Bus-köer.

| Jämförelsevillkor | Lagringsköer | Service Bus-köer |
| --- | --- | --- |
| Autentisering |**Symmetrisk nyckel** |**Symmetrisk nyckel** |
| Säkerhetsmodell |Delegerad åtkomst via SAS-token. |SAS |
| Provider för identitetsfederation |**Nej** |**Ja** |

### <a name="additional-information"></a>Ytterligare information
* Varje begäran tooeither av hello queuing tekniker måste autentiseras. Offentliga köer med anonym åtkomst stöds inte. Med hjälp av [SAS](service-bus-sas.md), du kan åtgärda det här scenariot genom att publicera en lässkyddad SAS, skrivskyddad SAS eller även en full åtkomst SAS.
* hello autentiseringsschema under förutsättning av lagring innebär köer hello användning av en symmetrisk nyckel, vilket är en hashbaserad meddelandeautentiseringskod (HMAC), beräknas med hello SHA-256-algoritmen och kodade som en **Base64** sträng. Mer information om respektive hello-protokollet finns [autentisering för hello Azure Storage-tjänster](/rest/api/storageservices/fileservices/Authentication-for-the-Azure-Storage-Services). Service Bus-köer stöder en liknande modell med symmetriska nycklar. Mer information finns i [signatur autentisering för delad åtkomst med Service Bus](service-bus-sas.md).

## <a name="conclusion"></a>Slutsats
Genom att få en bättre förståelse av hello två tekniker ska som vara kan toomake välgrundade beslut om vilken kö teknik toouse och när. Hej beslut om när toouse lagringsköer eller Service Bus köer tydligt beror på ett antal faktorer. Dessa faktorer kan kraftigt beroende hello behov av programmet och dess arkitektur. Om programmet redan använder hello grundfunktionerna i Microsoft Azure måste överväga du toochoose lagringsköer, särskilt om du kräver grundläggande kommunikation och skickar meddelanden mellan tjänster eller behöver köer som kan vara större än 80 GB i storlek.

Eftersom Service Bus-köer ger ett antal avancerade funktioner, till exempel sessioner, transaktioner, dubblettidentifiering, automatisk dead-lettering och varaktig Publicera/prenumerera funktioner, de kan vara ett önskade alternativ om du skapar en hybrid programmet eller om programmet annars kräver dessa funktioner.

## <a name="next-steps"></a>Nästa steg
hello följande artiklar ge mer vägledning och information om hur du använder Service Bus-köer eller lagringsköer.

* [Komma igång med Service Bus-köer](service-bus-dotnet-get-started-with-queues.md)
* [Hur tooUse hello Queue Storage-tjänst](../storage/queues/storage-dotnet-how-to-use-queues.md)
* [Metodtips för bättre prestanda med hjälp av Service Bus asynkron meddelandetjänst](service-bus-performance-improvements.md)
* [Introduktion till köer och ämnen i Azure Service Bus (blogginlägg)](http://www.code-magazine.com/article.aspx?quickid=1112041)
* [Hej Developer's Guide tooService Bus](http://www.cloudcasts.net/devguide/Default.aspx?id=11030)
* [Använda hello Queuing-tjänsten i Azure](http://www.developerfusion.com/article/120197/using-the-queuing-service-in-windows-azure/)

[Azure portal]: https://portal.azure.com

