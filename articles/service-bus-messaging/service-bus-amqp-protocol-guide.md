---
title: "aaaAMQP 1.0 i Azure Service Bus och Händelsehubbar protokollet guiden | Microsoft Docs"
description: "Protokollet guiden tooexpressions och en beskrivning av AMQP 1.0 i Azure Service Bus och Händelsehubbar"
services: service-bus-messaging,event-hubs
documentationcenter: .net
author: clemensv
manager: timlt
editor: 
ms.assetid: d2d3d540-8760-426a-ad10-d5128ce0ae24
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/07/2017
ms.author: clemensv;hillaryc;sethm
ms.openlocfilehash: 882ce0fc84af11d9f61bc95dc3e4db0b67b2b020
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# AMQP 1.0 i Azure Service Bus och Händelsehubbar protocol-guiden

hello är avancerade Message Queueing-protokollet 1.0 en standardiserad synkroniseringstecken och överför protokoll för asynkront säkert och tillförlitligt meddelanden överförs mellan två parter. Är det primära hello-protokollet för Azure Service Bus-meddelanden och Händelsehubbar i Azure. Båda tjänsterna har också stöd för HTTPS. Hej SBMP protokoll som stöds också kommer att överges för AMQP.

AMQP 1.0 är hello resultatet av bred branschen samarbete som mellanprogramvaruleverantörer, till exempel Microsoft och Red Hat med många meddelanden mellanprogram användare, till exempel JP Morgan Chase som representerar hello industrin för finansiella tjänster, samverkar. Det har uppnått formella godkännande som en internationell standard som ISO/IEC 19494 hello teknisk standardisering forum för hello AMQP protokoll och tillägget specifikationer är OASIS.

## Mål

Den här artikeln sammanfattar hello grundbegrepp av meddelanden hello AMQP 1.0-specifikationen tillsammans med en liten uppsättning utkast tillägget specifikationer som för närvarande är att färdigställas i hello OASIS AMQP tekniska kommittén kort och förklarar hur Azure Service Bus implementerar och bygger på dessa specifikationer.

hello målet är för alla utvecklare som med alla befintliga AMQP 1.0-klienten stack på någon plattform toobe kan toointeract med Azure Service Bus via AMQP 1.0.

Vanliga allmänna AMQP 1.0-stackar, till exempel Apache Proton eller AMQP.NET Lite implementera redan alla kärnor AMQP 1.0-gester. De grundläggande gester är ibland omslutna av en högre nivå API; Apache Proton erbjuder även två, hello tvingande Messenger-API och hello reaktiv reaktor API.

I följande diskussion hello, antar vi att hello hantering av AMQP anslutningar, sessioner och länkar och hello hantering av ram överföringar och flödeskontroll hanteras av respektive hello-stacken (till exempel Apache Proton-C) och behöver inte mycket eventuella specifika uppmärksamhet från programutvecklare. Elementattributet antar vi några API primitiver som hello möjlighet tooconnect och toocreate hello finnas någon form av *avsändaren* och *mottagare* abstraction-objekt som har vissa formen på `send()`och `receive()` åtgärder, respektive.

När du ska diskutera avancerade funktioner för Azure Service Bus, till exempel meddelande Bläddra eller hantering av sessioner, beskrivs funktionerna i AMQP villkor, men även som en skiktad genererat implementering ovanpå antagen API abstraktionen.

## Vad är AMQP?

AMQP är ett synkroniseringstecken och överför protokoll. Synkroniseringstecken innebär att det finns struktur för binära dataströmmar som flöda i båda riktningarna för en nätverksanslutning. hello strukturen innehåller en avbildning för distinkta datablock, kallas *ramar*, toobe utbyts mellan hello anslutna parter. hello överföring funktioner Se till att båda kommunicerande parterna kan upprätta en delad förståelse om när ramar överföras och när överföringar anses slutförd.

Till skillnad från tidigare har upphört att gälla utkastversioner produceras av hello AMQP arbetsgruppen som fortfarande används av några meddelande mäklare, fastställa hello arbetsgruppens slutliga och standardiserad AMQP 1.0-protokollet inte hello förekomst av förhandlare meddelande eller en viss topologi för entiteter i en koordinator för meddelandet.

hello-protokollet kan användas för symmetrisk peer-to-peer-kommunikation för interaktion med meddelandet mäklare som stöder köer och publicera/prenumerera entiteter som Azure Service Bus. Det kan också användas för interaktion med meddelandeinfrastruktur där hello interaktion mönster skiljer sig från vanlig köer som hello fallet med Händelsehubbar i Azure. En Händelsehubb fungerar som en kö när händelser skickas tooit, men fungerar mer som en seriell lagringstjänsten när händelser läses från den. en bandenhet eller något liknande. hello klienten hämtar en förskjutning i hello tillgängliga dataström och sedan behandlas alla händelser från den offset toohello senaste tillgängliga.

Hej AMQP 1.0-protokollet är utformad toobe extensible att aktivera ytterligare specifikationer tooenhance dess funktioner. hello tre tillägget specifikationer som beskrivs i det här dokumentet illustrerar detta. För kommunikation över den befintliga HTTPS/WebSockets infrastruktur där konfigurera hello interna AMQP TCP-portar kan vara svårt att en bindning specifikation definierar hur toolayer AMQP över WebSockets. För att interagera med hello meddelandeinfrastruktur i en begäran och svar definierar sätt för hanteringsändamål eller tooprovide avancerade funktioner, hello AMQP management specifikation hello krävs grundläggande interaktion primitiver. För federerade auktorisering model-integration hello AMQP anspråk baserade säkerhet specifikationen definierar hur tooassociate och förnya auktorisering token som är associerade med länkar.

## Grundläggande AMQP-scenarier

Det här avsnittet beskrivs hello grundläggande användning av AMQP 1.0 med Azure Service Bus som omfattar att skapa anslutningar, sessioner och länkar och överföring av meddelanden tooand från Service Bus-entiteter, till exempel köer, ämnen och prenumerationer.

hello mest auktoritativ källa toolearn om hur AMQP fungerar är hello AMQP 1.0-specifikation, men hello-specifikationen har skrivits tooprecisely guiden implementering och inte tooteach hello-protokollet. Det här avsnittet fokuserar på introduktion till så mycket terminologi som behövs för att beskriva hur Service Bus använder AMQP 1.0. För en mer omfattande introduktion tooAMQP, samt en bredare beskrivning av AMQP 1.0, kan du granska [kursen video][this video course].

### Anslutningar och sessioner

AMQP anrop hello kommunicerar program *behållare*; dessa innehåller *noder*, vilka hello kommunicerar entiteter i dessa behållare. En kö kan vara felaktiga noden. AMQP tillåter för multiplexering, så att en anslutning kan användas för många Kommunikationssökvägar mellan noder. till exempel en program-klient kan samtidigt tar emot från en kö och tooanother överföringskön över hello samma nätverksanslutning.

![][1]

hello-nätverksanslutning har därför som bas hello behållare. Den initieras av hello-behållaren i hello klienten roll är att göra en utgående TCP-socket anslutning tooa behållare i hello mottagaren roll, som lyssnar efter och accepterar inkommande TCP-anslutningar. hello anslutning handskakning innehåller förhandling hello-protokollversion, deklarerar eller förhandlade hello användning av Transport Level Security (TLS/SSL) och en autentisering/auktorisering handskakning definitionsområdet hello anslutning som baseras på SASL.

Azure Service Bus kräver hello använder TLS vid alla tidpunkter. Stöder anslutningar via TCP-port 5671, innebär att hello TCP-anslutningen läggs först med TLS innan du anger hello AMQP protokollet handshake och har också stöd för anslutningar via TCP-port 5672 där hello servern omedelbart erbjuder en obligatorisk uppgradering av anslutningen tooTLS med hjälp av hello AMQP föreskrivs modellen. Hej AMQP WebSockets bindning skapar en tunnel via TCP-port 443 som sedan motsvarande tooAMQP 5671 anslutningar.

När du har installerat hello anslutningen och TLS, finns Service Bus två SASL mekanism alternativ:

* SASL OFORMATERAD används ofta för att skicka användarnamn och lösenord för autentiseringsuppgifter tooa server. Service Bus saknar konton, men namngivna [delad åtkomst säkerhetsregler](service-bus-sas.md), vilket ger rättigheter och som är associerade med en nyckel. hello namnet på en regel som används som hello användarnamn och hello nyckel (som base64-kodad text) används som hello lösenord. hello rättigheter som är associerade med hello valt regel styr tillåtna på hello anslutning hello-åtgärder.
* SASL anonym används för att kringgå SASL tillstånd när hello klient vill toouse hello anspråk baserade säkerhet (CBS) modell som beskrivs senare. Klienten kan att ansluta anonymt under en kort period under vilken hello klienten bara kan interagera med hello CBS slutpunkt med det här alternativet och hello CBS handskakning måste slutföras.

Efter hello Transportanslutningen har upprättats hello behållare varje deklarera hello ramstorleken minskas de är beredda toohandle och efter en timeout för inaktivitet ska ensidigt frånkoppling om det inte finns någon aktivitet på hello-anslutning.

De förklarar också hur många samtidiga kanaler stöds. En kanal är en enkelriktad, utgående virtuella överföringen ovanpå hello-anslutning. En session tar en kanal från varje hello sammankopplade behållare tooform en dubbelriktad kommunikation sökväg.

Sessioner har en Windows-baserad åtkomstkontroll Dataflödesmodell; När en session skapas varje part förklarar hur många ramar är villigt tooaccept till dess visas fönstret. Eftersom hello parter exchange ramar, överförs ramar fyller fönstret och överföringar stoppa när hello fönstret är full och tills fönstret hello hämtar återställa eller utökats med hello *flöde performative* (*performative*är hello AMQP term för protokollnivå gester som utbyts mellan hello två parter).

Den här Windows-baserad modellen är ungefär detsamma toohello TCP begrepp av Windows-baserad flödesreglering, men på hello session nivå i hello socket. hello finns-protokollet begreppet att tillåta för flera samtidiga sessioner så att trafik med hög prioritet kan vara rusade tidigare begränsad normal trafik som på en motorväg express lane.

Azure Service Bus använder för närvarande exakt en session för varje anslutning. hello Service Bus maximalt ram-storlek är 262 144 byte (256 kB) för Service Bus Standard och Händelsehubbar. Det är 1,048,576 (1 MB) för Service Bus Premium. Service Bus medför inte några särskilda session-nivå bandbreddsbegränsning windows, men återställer hello fönstret regelbundet som en del av Koppla nivån flödeskontroll (se [hello nästa avsnitt](#links)).

Anslutningar, kanaler och sessioner är tillfälliga. Om hello underliggande anslutningen döljs, anslutningar, måste TLS-tunnelns, SASL autentiseringskontexten och sessionerna återställas.

### Länkar

AMQP överför meddelanden över länkar. En länk är en sökväg för kommunikation som skapats under en session som gör att överföra meddelanden i en riktning; hello överföring status förhandling är över hello länk och dubbelriktad kommunikation mellan hello anslutna parterna.

![][2]

Länkar kan skapas genom att antingen behållare när som helst och över en befintlig session, vilket gör AMQP skiljer sig från många andra protokoll, inklusive HTTP och MQTT, där hello initiering av överföringar och överföringen är en exklusiv behörighet hello part skapar hello socketanslutning.

hello länken initierar behållaren ställer hello motsatt behållaren tooaccept en länk och den väljer en roll för avsändare eller mottagare. Därför behållare kan initiera skapar enkelriktad eller dubbelriktad Kommunikationssökvägar med hello senare modelleras som par av länkar.

Länkar med namnet och som är kopplade till noder. Enligt informationen i början av hello är noder hello kommunicerar entiteter i en behållare.

I Service Bus är en nod direkt motsvarande tooa kö, ett ämne, en prenumeration eller en systemkön underkön för en kö eller prenumeration. hello nodnamnet som används i AMQP är därför hello relativa namnet på hello entitet inuti hello Service Bus-namnrymd. Om en kö heter **MinKö**, som också är dess AMQP nodnamn. En prenumeration på artikeln följer reglerna för hello http-API av sortering i en resurssamling ”prenumerationer” och därmed en prenumeration **sub** eller ett ämne **mytopic** har hello AMQP nodnamnet **sub-mytopic/prenumerationer**.

hello anslutande klienten är också nödvändig toouse ett lokala nodnamn för att skapa länkar. Service Bus tolka inte dem är inte normativ om dessa nodnamn. AMQP 1.0-klientstackar använder vanligtvis en schema-tooassure att dessa tillfälliga noden namnen är unika i hello omfattning hello-klienten.

### Överföringar

När en länk har upprättats kan meddelanden överföras via den länken. AMQP, en överföring körs med en explicit protokollet gest (hello *överföring* performative) som flyttar ett meddelande från avsändaren tooreceiver via en länk. En överföringen har slutförts när den är ””, vilket innebär att båda parterna har etablerat en delad tolkning av hello resultatet av överföringen.

![][3]

I hello enklaste fallet väljer hello avsändaren toosend meddelanden ”före regleras”, vilket innebär att hello-klienten inte är intresserad av hello resultatet och hello mottagaren inte ge feedback om hello resultatet av åtgärden hello. Det här läget stöds av Service Bus på hello AMQP protokollnivå, men inte visas i någon av hello-klientens API: er.

hello vanliga fall har att meddelanden skickas oreglerade och hello mottagaren anger sedan godkännande eller underkännande med hello *disposition* performative. Avvisande inträffar när hello mottagaren kan inte acceptera hello-meddelande av någon anledning och avvisning hälsningsmeddelande innehåller information om hello orsak, vilket är ett fel struktur som definieras av AMQP. Om meddelanden som avvisas på grund av toointernal fel i Service Bus returnerar hello tjänsten extra information i denna struktur som kan användas för att tillhandahålla diagnostik tips toosupport supportpersonalen om du arkiverar supportärenden. Du lär dig mer information om fel senare.

En speciell typ av avvisande är hello *publicerat* tillstånd, vilket anger att hello mottagare har inga tekniska invändningar toohello överföring, men även inga intresse för att reglera hello överföring. Att fallet finns, till exempel när ett meddelande levereras tooa Service Bus-klienten och hello väljer klienten för ”Avbryt” hello-meddelande eftersom det går inte att utföra hello arbete som härrör från att bearbeta hello-meddelande. hello meddelandeleverans själva är inte vid fel. En variant av det aktuella tillståndet är hello *ändrade* tillstånd som tillåter ändringar toohello meddelande när det är tillgängliga. Det aktuella tillståndet används inte av Service Bus för närvarande.

Hej AMQP 1.0-specifikationen definierar en ytterligare disposition läge som kallas *emot*, som hjälper specifikt toohandle länk återställning. Länken återställning kan Rekonstruerar hello tillståndet för en länk och eventuella väntande leveranser ovanpå en ny anslutning och -sessionen när hello tidigare anslutning och session har gått förlorade.

Service Bus stöder inte länken återställning. Om klienten hello förlorar hello anslutning tooService Bus med meddelandet oreglerade överföra väntande, som meddelandeöverföringen går förlorad och hello klienten måste återansluta återupprätta hello länk och försök hello överföring.

Därför Service Bus och Händelsehubbar stöder ”minst en gång” överföringar där hello avsändaren kan säkerställas för hello-meddelande med lagras och godkänt, men har inte stöd för ”exakt en gång” överföringar på hello AMQP nivå, där hello system skulle försöker toorecover Hej länk och fortsätta toonegotiate hello leverans tillstånd tooavoid duplicering av hello meddelandeöverföringen.

toocompensate för möjlig dubblett skickar Service Bus stöder dubblettidentifiering som en valfri funktion i köer och ämnen. Identifiering av dubbletter poster hello meddelande-ID för inkommande meddelanden under ett användardefinierat tidsfönster sedan tyst således alla meddelanden som skickas med hello samma meddelande-ID: N under samma fönstret.

### Flödeskontroll

Dessutom toohello session nivå flödeskontroll modell som diskuterats tidigare, varje länk har sin egen Dataflödesmodell för kontrollen. Sessionen nivå flödeskontroll skyddar hello behållare från att ha toohandle för många bildrutor i när Koppla nivån flödeskontroll placerar hello-programmet ansvarar för hur många meddelanden den vill toohandle från en länk och när.

![][4]

På en länk överföringar kan bara inträffa när hello avsändaren har tillräckligt med *länka kredit*. Länken kredit är en uppsättning av hello mottagare med hello räknare *flöde* performative, som är begränsad tooa länk. När avsändaren hello tilldelas länken kredit, försöker toouse upp den krediten genom att skicka meddelanden. Varje meddelande leverans minskar hello återstående kredit för länken med 1. När hello länken kredit används stoppa leveranser.

När Service Bus hello mottagaren roll kan ger den direkt hello avsändaren gott om länken kredit, så att meddelanden kan skickas omedelbart. Länken kredit används Service Bus skickar en *flöde* Kreditsaldot för performative toohello avsändaren tooupdate hello-länk.

I hello avsändaren roll skickar Service Bus meddelanden toouse dig krediter utestående länk.

Ett anrop för ”ta emot” på hello API-nivå översätter till en *flöde* performative skickas tooService Bus av hello-klienten och Service Bus förbrukar som kredit av tar hello först tillgängliga, olåst meddelande från hello kö, låsa det. och överför den. Om det finns inget meddelande är tillgängligt för leverans, upprätta krediter utestående med en länk till att viss enheten förblir registrerade i den ordning de anländer och meddelanden är låst och överförs när de blir tillgängliga, toouse krediter utestående.

hello lås på ett meddelande som släpps när hello-överföring är i ett av hello terminal *accepteras*, *avvisade*, eller *publicerat*. hello-meddelande tas bort från Service Bus hello terminal status är *accepteras*. Det finns kvar i Service Bus och levereras toohello nästa mottagare när hello överföringen når någon hello andra tillstånd. Service Bus flyttas automatiskt hello-meddelande till hello entitetens obeställbara meddelanden när den når hello leverans av maximalt antal tillåtna för hello entiteten förfallodatum toorepeated nekanden eller versioner.

Även om hello Service Bus-API: er inte direkt exponerar sådana ett alternativ idag, kan en lägre nivå AMQP protokollet klient använda hello länken kredit modellen tooturn hello ”pull-style” interaktion för att utfärda en enhet av kredit för varje receive-begäran till en ”push-style”-modell genom att utfärda ett stort antal länken krediter och ta emot meddelanden när de blir tillgängliga utan några ytterligare åtgärder. Push stöds via hello [MessagingFactory.PrefetchCount](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_PrefetchCount) eller [MessageReceiver.PrefetchCount](/dotnet/api/microsoft.servicebus.messaging.messagereceiver#Microsoft_ServiceBus_Messaging_MessageReceiver_PrefetchCount) egenskapsinställningar. När de är noll använder hello AMQP klienten den som hello länken kredit.

I den här kontexten startar det är viktigt toounderstand som hello klockan för hello gälla hello lås på hello-meddelande i hello entitet när hello meddelandet hämtas från hello entitet inte när hello-meddelande placeras hello uppkopplat läge. När hello-klienten anger beredskap tooreceive meddelanden genom att utfärda länken kredit, det är därför förväntade toobe aktivt ta emot meddelanden i hello nätverk och vara klar toohandle dem. Annars Lås för hello-meddelande kan ha gått ut innan hello-meddelande levereras även. hello hjälp av länken kredit flödesreglering återspegla direkt hello omedelbar beredskap toodeal med tillgängliga meddelanden skickas toohello mottagare.

Sammanfattningsvis hello följande avsnitt innehåller en schema översikt över hello performative flöde under olika API interaktioner. I avsnitten beskrivs en annan logisk åtgärd. Vissa av dessa interaktioner kanske ”lazy”, vilket innebär att de kan endast utföras när det behövs. Skapar avsändarens kanske inte en nätverket interaktion tills första hello-meddelande skickas eller begärs.

hello pilarna i hello följande tabell visar hello performative flödesriktning.

#### Skapa meddelande mottagare

| Client | Service Bus |
| --- | --- |
| --> Bifoga ()<br/>Name = {namn},<br/>Hantera = {numeriskt referensen}<br/>rollen =**mottagare**,<br/>källa = {entitetsnamnet}<br/>Target = {klienten länken id}<br/>) |Klienten kopplas tooentity som mottagare |
| Service Bus-svar som kopplar slutet av hello länk |<--bifoga ()<br/>Name = {namn},<br/>Hantera = {numeriskt referensen}<br/>rollen =**avsändaren**,<br/>källa = {entitetsnamnet}<br/>Target = {klienten länken id}<br/>) |

#### Skapa avsändaren

| Client | Service Bus |
| --- | --- |
| --> Bifoga ()<br/>Name = {namn},<br/>Hantera = {numeriskt referensen}<br/>rollen =**avsändaren**,<br/>källa = {klienten länken id}<br/>Target = {entitetsnamnet}<br/>) |Ingen åtgärd |
| Ingen åtgärd |<--bifoga ()<br/>Name = {namn},<br/>Hantera = {numeriskt referensen}<br/>rollen =**mottagare**,<br/>källa = {klienten länken id}<br/>Target = {entitetsnamnet}<br/>) |

#### Skapa avsändaren (fel)

| Client | Service Bus |
| --- | --- |
| --> Bifoga ()<br/>Name = {namn},<br/>Hantera = {numeriskt referensen}<br/>rollen =**avsändaren**,<br/>källa = {klienten länken id}<br/>Target = {entitetsnamnet}<br/>) |Ingen åtgärd |
| Ingen åtgärd |<--bifoga ()<br/>Name = {namn},<br/>Hantera = {numeriskt referensen}<br/>rollen =**mottagare**,<br/>källa = null<br/>Target = null<br/>)<br/><br/><--frånkoppling ()<br/>Hantera = {numeriskt referensen}<br/>stängd =**SANT**,<br/>fel = {felinformationen}<br/>) |

#### Stäng mottagare/avsändaren

| Client | Service Bus |
| --- | --- |
| --> frånkoppling ()<br/>Hantera = {numeriskt referensen}<br/>stängd =**SANT**<br/>) |Ingen åtgärd |
| Ingen åtgärd |<--frånkoppling ()<br/>Hantera = {numeriskt referensen}<br/>stängd =**SANT**<br/>) |

#### Skicka (klart)

| Client | Service Bus |
| --- | --- |
| --> överföring (<br/>leverans-id = {numeriskt referensen}<br/>leverans-tagg = {binära referensen}<br/>regleras =**FALSKT**, mer =**FALSKT**,<br/>tillstånd =**null**,<br/>återuppta =**FALSKT**<br/>) |Ingen åtgärd |
| Ingen åtgärd |<--disposition (<br/>rollen = mottagaren.<br/>först = {leverans id}<br/>senaste = {leverans id}<br/>regleras =**SANT**,<br/>tillstånd =**accepteras**<br/>) |

#### Skicka (fel)

| Client | Service Bus |
| --- | --- |
| --> överföring (<br/>leverans-id = {numeriskt referensen}<br/>leverans-tagg = {binära referensen}<br/>regleras =**FALSKT**, mer =**FALSKT**,<br/>tillstånd =**null**,<br/>återuppta =**FALSKT**<br/>) |Ingen åtgärd |
| Ingen åtgärd |<--disposition (<br/>rollen = mottagaren.<br/>först = {leverans id}<br/>senaste = {leverans id}<br/>regleras =**SANT**,<br/>tillstånd =**avvisade**()<br/>fel = {felinformationen}<br/>)<br/>) |

#### Ta emot

| Client | Service Bus |
| --- | --- |
| --> flöde (<br/>länken kredit = 1<br/>) |Ingen åtgärd |
| Ingen åtgärd |< transfer ()<br/>leverans-id = {numeriskt referensen}<br/>leverans-tagg = {binära referensen}<br/>regleras =**FALSKT**,<br/>fler =**FALSKT**,<br/>tillstånd =**null**,<br/>återuppta =**FALSKT**<br/>) |
| --> disposition (<br/>rollen =**mottagare**,<br/>först = {leverans id}<br/>senaste = {leverans id}<br/>regleras =**SANT**,<br/>tillstånd =**accepteras**<br/>) |Ingen åtgärd |

#### Flera meddelandemottagning

| Client | Service Bus |
| --- | --- |
| --> flöde (<br/>länken kredit = 3<br/>) |Ingen åtgärd |
| Ingen åtgärd |< transfer ()<br/>leverans-id = {numeriskt referensen}<br/>leverans-tagg = {binära referensen}<br/>regleras =**FALSKT**,<br/>fler =**FALSKT**,<br/>tillstånd =**null**,<br/>återuppta =**FALSKT**<br/>) |
| Ingen åtgärd |< transfer ()<br/>leverans-id = {numeriskt referensen + 1}<br/>leverans-tagg = {binära referensen}<br/>regleras =**FALSKT**,<br/>fler =**FALSKT**,<br/>tillstånd =**null**,<br/>återuppta =**FALSKT**<br/>) |
| Ingen åtgärd |< transfer ()<br/>leverans-id = {numeriskt referensen + 2}<br/>leverans-tagg = {binära referensen}<br/>regleras =**FALSKT**,<br/>fler =**FALSKT**,<br/>tillstånd =**null**,<br/>återuppta =**FALSKT**<br/>) |
| --> disposition (<br/>rollen = mottagaren.<br/>först = {leverans id}<br/>senaste = {leverans id + 2}<br/>regleras =**SANT**,<br/>tillstånd =**accepteras**<br/>) |Ingen åtgärd |

### Meddelanden

hello följande avsnitt beskrivs vilka egenskaper från hello standard AMQP meddelande avsnitt används av Service Bus och hur de mappas toohello Service Bus-API-uppsättningen.

#### sidhuvud

| Fältnamn | Användning | API-namn |
| --- | --- | --- |
| beständiga |- |- |
| Prioritet |- |- |
| TTL-värde |Tid som toolive för det här meddelandet |[TimeToLive](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive) |
| första förvärvaren |- |- |
| Antal leverans |- |[DeliveryCount](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_DeliveryCount) |

#### properties

| Fältnamn | Användning | API-namn |
| --- | --- | --- |
| meddelande-id |Programdefinierade, fritt format identifierare för det här meddelandet. Används för identifiering av dubbletter. |[MessageId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId) |
| användar-id |Programdefinierade användaridentifierare inte tolkas av Service Bus. |Inte tillgängligt via hello Service Bus-API. |
| för|Programdefinierade Målidentifierare, inte tolkas av Service Bus. |[Att](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_To) |
| Ämne |Tillämpningsdefinierade syfte identifierare inte tolkas av Service Bus. |[Etikett](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label) |
| svara för|Programdefinierade reply-path indikator, inte tolkas av Service Bus. |[ReplyTo](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_ReplyTo) |
| Korrelations-id |Programdefinierade korrelationsidentifierare, inte tolkas av Service Bus. |[CorrelationId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_CorrelationId) |
| innehållstyp |Programdefinierade innehållstypen indikator för hello brödtexten, inte tolkas av Service Bus. |[ContentType](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_ContentType) |
| Innehållskodning |Programdefinierade Innehållskodning indikator för hello brödtexten, inte tolkas av Service Bus. |Inte tillgängligt via hello Service Bus-API. |
| absolut förfallotiden |Deklarerar på vilka absolut instant hello meddelandet upphör att gälla. Ignoreras på indata (sidhuvud TTL observeras), auktoritativ på utdata. |[ExpiresAtUtc](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_ExpiresAtUtc) |
| Skapa en gång |Deklarerar vid vilken tidpunkt hello meddelandet skapades. Inte används av Service Bus |Inte tillgängligt via hello Service Bus-API. |
| grupp-id |Tillämpningsdefinierad identifierare för en uppsättning meddelanden. Används för Service Bus-sessioner. |[Sessions-ID](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_SessionId) |
| grupp-sekvens |Räknaren identifierar hello relativa sekvensnumret för hello-meddelande i en session. Ignoreras av Service Bus. |Inte tillgängligt via hello Service Bus-API. |
| Svara till grupp-id |- |[ReplyToSessionId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_ReplyToSessionId) |

## Avancerade funktioner för Service Bus

Det här avsnittet beskriver de avancerade funktionerna i Azure Service Bus som baseras på ett utkast till tillägg tooAMQP, för närvarande under utveckling i hello OASIS tekniska kommittén för AMQP. Service Bus implementerar hello senaste versionerna av dessa utkast och tillämpar ändringar som dessa utkast nå standard status.

> [!NOTE]
> Service Bus-meddelanden avancerade åtgärder stöds via ett mönster i begäran och svar. hello information om de här åtgärderna beskrivs i hello dokumentet [AMQP 1.0 i Service Bus: begäran-svar-baserade operations](service-bus-amqp-request-response.md).
> 
> 

### AMQP management

Hej AMQP management specifikation är hello första av hello utkast tillägg som beskrivs här. Den här specifikationen definierar en uppsättning av protokollet gester lager ovanpå hello AMQP-protokollet som tillåter hanteringsinteraktioner med hello infrastrukturen via AMQP. hello-specifikationen som definierar allmänna operations *skapa*, *läsa*, *uppdatera*, och *ta bort* för att hantera enheter i en infrastruktur för meddelanden och en uppsättning frågeåtgärder.

Dessa gester kräver en begäran och svar interaktion mellan hello klient och hello meddelandeinfrastruktur och därför hello specifikationen definierar hur toomodel att interaktionen mönstret ovanpå AMQP: hello klienten ansluter toohello meddelanden infrastruktur, initierar en session och skapar sedan ett par med länkar. Hello klienten fungerar som avsändaren på en länk och på andra hello det fungerar som mottagare, vilket skapar ett par med länkar som kan fungera som en dubbelriktad kanal.

| Logisk åtgärd | Client | Service Bus |
| --- | --- | --- |
| Skapa begäran svar sökvägen |--> Bifoga ()<br/>name = {*länknamn*},<br/>hantera = {*numeriska referensen*},<br/>rollen =**avsändaren**,<br/>källa =**null**,<br/>Target = ”myentity / $management”<br/>) |Ingen åtgärd |
| Skapa begäran svar sökvägen |Ingen åtgärd |\<--bifoga ()<br/>name = {*länknamn*},<br/>hantera = {*numeriska referensen*},<br/>rollen =**mottagare**,<br/>källa = null<br/>Target = ”myentity”<br/>) |
| Skapa begäran svar sökvägen |--> Bifoga ()<br/>name = {*länknamn*},<br/>hantera = {*numeriska referensen*},<br/>rollen =**mottagare**,<br/>källa = ”myentity / $management”<br/>Target = ”myclient$-ID.<br/>) | |
| Skapa begäran svar sökvägen |Ingen åtgärd |\<--bifoga ()<br/>name = {*länknamn*},<br/>hantera = {*numeriska referensen*},<br/>rollen =**avsändaren**,<br/>källa = ”myentity”<br/>Target = ”myclient$-ID.<br/>) |

Att ha paret med länkar, hello begäran och svar implementering är enkla: en begäran är ett meddelande som skickas tooan entiteten i hello meddelandeinfrastruktur som kan användas med det här mönstret. I det begärandemeddelandet hello *svar till* i hello *egenskaper* avsnittet anges toohello *mål* identifierare för hello länk till vilka toodeliver hello svar. hello hanterar entitet behandlar hello begäran och ger hello svar över hello länka vars *mål* identifierare som matchar hello anges *svar till* identifierare.

hello mönstret kräver givetvis att hello klienten behållare och hello klient-genererade identifierare för hello svar mål är unikt över alla klienter och av säkerhetsskäl bör också svårt toopredict.

hello meddelandeutbyten används för hello management-protokollet och för alla andra protokoll som används hello samma mönster som sker på programnivå hello; de definierar inte nya AMQP protokollnivå gester. Det är avsiktlig, så att program kan dra omedelbar nytta av dessa tillägg med kompatibla AMQP 1.0-stackar.

Service Bus för närvarande implementerar inte någon av hello kärnfunktioner hello management specifikation, men hello begäran och svar mönstret som definieras med hello management specifikation är grundläggande för hello anspråk baserade säkerhet funktion och nästan alla hello avancerade funktioner som beskrivs i följande avsnitt hello.

### Anspråksbaserad auktorisering

Hej AMQP anspråk baserad auktorisering (CBS) specifikation utkast bygger på hello management specifikation begäran och svar mönster och beskriver en generaliserad modell för hur toouse federerad säkerhetstoken med AMQP.

hello standard säkerhetsmodellen i AMQP som beskrivs i hello introduktion baseras på SASL och integreras med hello AMQP anslutning handskakning. Använda SASL har hello nytta som den tillhandahåller en utökningsbar modell som en uppsättning metoder har definierats från alla protokoll som formellt leans på SASL kan dra. Bland dessa mekanismer är ”OFORMATERAD” för överföring av användarnamn och lösenord, ”externa” toobind tooTLS säkerhet, ”anonym” tooexpress hello frånvaro av explicit autentisering/auktorisering och ett stort antal ytterligare mekanismer som ger Skicka autentiseringsuppgifter för autentisering och auktorisering eller token.

AMQP'S SASL integrering har två nackdelar:

* Alla autentiseringsuppgifter och token är begränsade toohello anslutning. En meddelandeinfrastruktur kanske tooprovide differentierade åtkomstkontroll på grundval av per enhet; till exempel tillåta att hello innehavare av en token toosend tooqueue A men inte tooqueue B. Med hello autentiseringskontexten hello anslutning som bas, det är inte möjlig toouse en anslutning och ännu använder olika åtkomsttoken för kö A och kön B.
* Åtkomst-token gäller vanligtvis endast för en begränsad tid. Denna giltighet kräver hello användaren tooperiodically återfå token och ger en möjlighet toohello token utfärdaren toorefuse utfärda en ny token om hello användarbehörigheter har ändrats. AMQP anslutningar kan det sista under långa perioder. hello SASL modellen bara ger en möjlighet tooset en token vid anslutningen, vilket innebär att hello meddelandeinfrastruktur antingen har toodisconnect hello klienten när hello-token upphör att gälla eller måste tooaccept hello risken för att fortsatt kommunikation med en klient som har behörighet som krävs kan ha återkallats i hello tillfälligt.

Hej AMQP CBS-specifikationen som implementeras av Service Bus, kan en elegant lösning för båda dessa problem: Det gör att en klient tooassociate åtkomst-token med varje nod och tooupdate de token innan de upphör att gälla, utan att avbryta flödet för hello-meddelande.

CBS definierar en virtuell nod med namnet *$cbs*, toobe som tillhandahålls av hello meddelandeinfrastrukturen. hälsning hanteringsnod accepterar token för andra noder i hello infrastrukturen.

hello protokollet gest är en request/reply-exchange som definieras av hello management specifikation. Att innebär hello klienten upprättar ett par med länkar med hello *$cbs* nod och överför en begäran på hello utgående länk och väntar på hello svar på hello inkommande länk.

hello Begärandemeddelandet har följande egenskaper för programmet hello:

| Nyckel | Valfri | Värdetyp | Värdet innehållet |
| --- | --- | --- | --- |
| åtgärden |Nej |Sträng |**PUT-token** |
| typ |Nej |Sträng |hello-token som är put hello typ. |
| namn |Nej |Sträng |Hej ”målgruppen” toowhich hello token gäller. |
| förfallodatum |Ja |tidsstämpel |hello förfallotiden för hello-token. |

Hej *namn* egenskapen identifierar hello entitet med vilka hello token vara associerat. I Service Bus det är hello sökvägen toohello kö eller ämne /-prenumeration. Hej *typen* egenskapen identifierar hello tokentyp:

| Tokentypen | Token beskrivning | Brödtexten | Anteckningar |
| --- | --- | --- | --- |
| amqp:jwt |JSON Web Token (JWT) |AMQP värde (sträng) |Ännu ej tillgänglig. |
| amqp:SWT |Enkel Web Token (SWT) |AMQP värde (sträng) |Stöds endast för SWT token som utfärdas av AAD/ACS |
| servicebus.Windows.NET:sastoken |Service Bus SAS-Token |AMQP värde (sträng) |- |

Token ge rättigheter. Service Bus medveten om tre grundläggande rättigheter: ”skicka” gör det möjligt att skicka ”lyssna” kan ta emot och ”hantera” gör att manipulera entiteter. SWT token som utfärdats av AAD/ACS explicit med dessa rättigheter som anspråk. Service Bus SAS-token finns toorules som konfigurerats på hello namnområde eller enhet och dessa regler är konfigurerade med rättigheter. Signeringstoken hello med hello-nyckel som är associerade med regeln blir därmed hello token express hello respektive rättigheter. hello-token som är associerat med en entitet *put-token* tillåter hello ansluten klient toointeract med hello entitet per hello token rättigheter. En länk där hello klienten tar på sig hello *avsändaren* rollen kräver hello ”skicka” höger; tar på hello *mottagare* rollen kräver hello ”lyssna” höger.

hello reply-meddelande är hello följande *egenskaper för program* värden

| Nyckel | Valfri | Värdetyp | Värdet innehållet |
| --- | --- | --- | --- |
| statuskod |Nej |int |HTTP-svarskoden **[RFC2616]**. |
| Beskrivning av status |Ja |Sträng |Beskrivning av hello status. |

hello-klienten kan anropa *put-token* upprepade gånger och för varje entitet i hello meddelandeinfrastrukturen. hello token är begränsade toohello aktuella klienten och fäst på hello aktuella anslutningen betydelse hello server släpper eventuella tokens som behålls när hello anslutningen bryts.

hello aktuella Service Bus-implementeringen kan bara CBS tillsammans med hello SASL metoden ”anonym”. En SSL/TLS-anslutning måste alltid finnas tidigare toohello SASL handskakning.

hello anonym mekanism måste därför stödjas av hello valt AMQP 1.0-klienter. Anonym åtkomst innebär att hello handskakningen inledande anslutningen, inklusive att skapa hello första sessionen sker utan Service Bus att veta vem som skapar hello-anslutning.

När hello anslutning och sessionen har upprättats kan bifoga hello länkar toohello *$cbs* nod och skicka hello *put-token* begäran är hello endast tillåtna åtgärder. En giltig token måste anges med en *put-token* begäran för vissa entitet nod inom 20 sekunder efter hello anslutning har upprättats, annars hello anslutningen ensidigt bryts av Service Bus.

hello klienten ansvarar senare för att hålla reda på token upphör att gälla. När en token upphör att gälla utelämnar Service Bus omedelbart alla länkar i hello anslutning toohello respektive entiteten. tooprevent detta, hello klienten kan ersätta hello token för hello nod med en ny när som helst via hello virtuella *$cbs* hanteringsnod med hello samma *put-token* rörelser och utan hämtar i hello sätt att hello nyttolast trafik som flödar på olika länkar.

## Nästa steg

toolearn mer om AMQP, finns hello följande länkar:

* [Översikt över Service Bus AMQP]
* [AMQP 1.0-stöd för Service Bus partitionerad köer och ämnen]
* [AMQP i Service Bus för Windows Server]

[this video course]: https://www.youtube.com/playlist?list=PLmE4bZU0qx-wAP02i0I7PJWvDWoCytEjD
[1]: ./media/service-bus-amqp-protocol-guide/amqp1.png
[2]: ./media/service-bus-amqp-protocol-guide/amqp2.png
[3]: ./media/service-bus-amqp-protocol-guide/amqp3.png
[4]: ./media/service-bus-amqp-protocol-guide/amqp4.png

[Översikt över Service Bus AMQP]: service-bus-amqp-overview.md
[AMQP 1.0-stöd för Service Bus partitionerad köer och ämnen]: service-bus-partitioned-queues-and-topics-amqp-overview.md
[AMQP i Service Bus för Windows Server]: https://msdn.microsoft.com/library/dn574799.aspx
