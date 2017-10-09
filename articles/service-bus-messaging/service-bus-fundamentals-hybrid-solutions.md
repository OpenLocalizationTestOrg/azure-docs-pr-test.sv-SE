---
title: aaaOverview av grunderna i Azure Service Bus | Microsoft Docs
description: En introduktion toousing Service Bus tooconnect Azure-program tooother programvara.
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 12654cdd-82ab-4b95-b56f-08a5a8bbc6f9
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/15/2017
ms.author: sethm
ms.openlocfilehash: 1abd5cf310ef06ba35e1e2489a7c0a07e1797736
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-service-bus"></a>Azure Service Bus

Om ett program eller tjänst körs i hello molnet eller lokalt, måste den ofta toointeract med andra program eller tjänster. tooprovide ett fritt och praktiskt sätt toodo detta, erbjudanden för Microsoft Azure Service Bus. Den här artikeln beskrivs denna teknik och beskriver vad det är och varför du kanske vill toouse den.

## <a name="service-bus-fundamentals"></a>Grunderna i Service Bus

Olika situationer kräver olika kommunikationsstilar. Ibland är att låta appar skicka och ta emot meddelanden via en enkel kö hello bästa lösningen. I vissa situationer räcker det inte med en vanlig kö, utan det är bättre att använda en kö med en mekanism av typen publicera och prenumerera. I andra fall är allt som behövs en anslutning mellan apparna. Köer är överflödiga. Service Bus innehåller alla tre alternativ att aktivera ditt program toointeract på flera olika sätt.

Service Bus är en tjänst i molnet för flera innehavare, vilket innebär att hello tjänsten delas av flera användare. Varje användare, till exempel en programutvecklare, skapar en *namnområde*, definierar hello kommunikationsmekanismer behövs inom det här namnområdet. Bild 1 visar hur den här arkitekturen ser ut.

![][1]

**Bild 1: Service Bus innehåller en tjänst för flera innehavare för att ansluta appar via hello molnet.**

Du kan använda en eller flera instanser av tre olika kommunikationsmekanismer inom namnområdet. Dessa ansluter till appar på olika sätt. hello-alternativen är:

* *Köer*, som tillåter en enkelriktad kommunikation. Varje kö fungerar som en mellanhand (kallas ibland för en *koordinator*) som sparar skickade meddelanden tills de tas emot. Varje meddelande tas emot av en enda mottagare.
* *Ämnen*, som ger enkelriktad kommunikation med hjälp av *prenumerationer*. Ett ämne kan ha flera prenumerationer. Ett ämne som fungerar som en koordinator precis som en kö, men varje prenumeration kan du använda ett filter tooreceive endast meddelanden som uppfyller specifika villkor.
* *Reläer* (relays), som ger dubbelriktad kommunikation. Till skillnad från köer och ämnen lagrar inte ett relä meddelanden som redan startats. Det är alltså inte en koordinator. I stället Reläet bara meddelandena på toohello målprogrammet.

När du skapar en kö, ett ämne eller ett relä ger du dem ett namn. Det här namnet skapar i kombination med det namn som du gav namnområdet, en unik identifierare för hello-objektet. Program kan ange det här namnet tooService Bus och sedan använda den kön, ämnet eller relay toocommunicate med varandra. 

toouse någon av dessa objekt i hello relay scenario, Windows-program kan använda Windows Communication Foundation (WCF). Den här tjänsten kallas [WCF Relay](../service-bus-relay/relay-what-is-it.md). För köer och ämnen kan Windows-program använda API:er för Service Bus-definierade meddelandefunktioner. toomake dessa objekt enklare toouse från Windows-program, tillhandahåller Microsoft SDK för Java, Node.js och andra språk. Du kan även få åtkomst till köer och ämnen med hjälp av [REST-API:er](/rest/api/servicebus/) via HTTP. 

Det är viktigt toounderstand som även om Service Bus själva körs i molnet hello (det vill säga i datacenter för Microsoft Azure), program som använder den kan köras var som helst. Du kan använda Service Bus tooconnect program som körs på Azure, till exempel eller program som körs i ditt eget datacenter. Du kan också använda den tooconnect ett program som körs på Azure eller en annan molnplattform med ett lokalt program eller med surfplattor och telefoner. Det är även möjligt tooconnect hushållsapparater, sensorer och andra enheter tooa centralt program eller tooone andra. Service Bus är en mekanism för kommunikation i hello moln som är tillgänglig från nästan var som helst. Hur du använder beror det på vilken dina program behöver toodo.

## <a name="queues"></a>Köer

Anta att du bestämmer dig för tooconnect två program med hjälp av en Service Bus-kö. Bild 2 visar den här situationen.

![][2]

**Bild 2: Service Bus-köer erbjuder enkelriktade asynkrona köer.**

hello-processen är enkel: en avsändare skickar ett meddelande tooa Service Bus-kö och en mottagare hämtar meddelandet vid ett senare tillfälle. En kö kan ha bara en enkel mottagare, som bild 2 visar. Eller flera program kan läsa från hello samma kö. I hello senare fallet läses varje meddelande av enbart en mottagare. För en tjänst med multi-cast bör du istället använda ett ämne.

Varje meddelande består av två delar: en uppsättning egenskaper, som alla är ett nyckel-/värdepar, och en meddelandenyttolast. hello nyttolasten kan vara binary, text eller även XML. Hur de används beror på vilken ett program försöker toodo. Till exempel ett program som skickar ett meddelande om en nyligen genomförd försäljning kan innehålla hello egenskaper **säljare = ”Ava”** och **belopp = 10000**. hello meddelandetexten kan innehålla en skannad bild av hello försäljningsavtalet eller om det inte finns någon, vara tomt.

En mottagare kan läsa ett meddelande från en Service Bus-kö på två olika sätt. Hej första alternativet, som kallas  *[ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode)*, tar bort ett meddelande från hello kön och raderar det direkt. Det här alternativet är enkla, men om hello mottagaren kraschar innan den är klar hello-meddelande, hello-meddelande förloras. Eftersom den har tagits bort från hello kön kan inga andra mottagare komma åt den. 

Hej andra alternativet  *[PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode)*, är avsedd toohelp med det här problemet. Som **ReceiveAndDelete**, **PeekLock** bort meddelandet från kön hello. Hello-meddelande bort inte men. I stället låser hello-meddelande, vilket gör det osynligt tooother mottagare, och väntar på något av följande tre händelser:

* Om hello mottagaren processer hello meddelandet har, anropar [Complete()](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete), och hello kön raderar hello-meddelande. 
* Om hello mottagaren beslutar att den inte kan bearbeta har hello-meddelande, anropar [Abandon()](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Abandon). hello kön tar bort hello lås från hello-meddelande sedan och gör den tillgänglig tooother mottagare.
* Om hello mottagaren inte anropar något av dessa metoder inom en konfigurerbar tidsperiod (som standard 60 sekunder), förutsätter kön hello hello mottagaren har misslyckats. I det här fallet fungerar som om hello mottagaren hade anropat **Avbryt**, gör hello meddelandet tillgängliga tooother mottagare.

Observera vad som kan inträffa här: hello samma meddelande kan levereras två gånger, kanske tootwo olika mottagare. Appar som använder Service Bus-köer måste förberedas för denna händelse. toomake dubblettidentifiering enklare, varje meddelande har ett unikt [MessageID](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId) egenskap som standard förblir hello densamma, oavsett hur många gånger hello-meddelande läses från en kö. 

Köer är användbara i ett ganska stort antal situationer. De gör att program toocommunicate även när båda inte körs på hello samma tid, något som är särskilt praktiskt med batch- och mobilprogram. En kö med flera mottagare ger också automatisk belastningsbalansering eftersom skickade meddelanden sprids ut bland dessa mottagare.

## <a name="topics"></a>Ämnen

Användbara som de är köer är inte alltid hello rätta lösningen. Ibland är det bättre att använda Service Bus-ämnen. Bild 3 ger dig en uppfattning om varför det är så.

![][3]

**Bild 3: Baserat på hello filter som en prenumerationsapp använder, det kan ta emot några eller alla hello-meddelanden skickas tooa Service Bus-ämne.**

En *avsnittet* liknar på många sätt tooa kön. Avsändare skickar meddelanden tooa-avsnittet i hello samma sätt som de skickar meddelanden tooa kön och dessa meddelanden utseende hello samma precis som med köer. hello skillnaden är att ämnen gör att varje mottagande programmet toocreate sin egen *prenumeration* genom att definiera en *filter*. En prenumerant ser bara hello-meddelanden som matchar filtret. Bild 3 visar till exempel en avsändare och ett ämne med tre prenumeranter, var och en med sitt eget filter:

* Prenumerant 1 får endast meddelanden som innehåller hello egenskapen *säljare = ”Ava”*.
* Prenumerant 2 tar emot meddelanden som innehåller hello egenskapen *säljare = ”Ruth”* och/eller innehåller ett *belopp* egenskap vars värde är större än 100 000. Ruth kanske är hello försäljningschef så hon vill toosee både sina egna försäljningar och alla stora försäljningar, oavsett vem som gör dem..
* Prenumerant 3 har ställt in sitt filter för*SANT*, vilket innebär att den tar emot alla meddelanden. Till exempel det här programmet kan ansvara för att bibehålla ett revisionsspår och därför måste toosee alla hälsningsmeddelande.

Som med köer, prenumeranter tooa avsnittet kan läsa meddelanden med antingen [ReceiveAndDelete och PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode). Till skillnad från köer, men skickat ett enda meddelande tooa avsnittet kan tas emot av flera prenumerationer. Den här metoden anropas ofta *publicera och prenumerera* (eller *pub/sub*), är användbart när flera appar är intresserade hello samma meddelanden. Genom att definiera hello rätt filter kan prenumerant varje bara hello del av hello meddelandeströmmen måste toosee.

## <a name="relays"></a>Reläer

Både köer och ämnen ger dig enkelriktad, asynkron kommunikation via en koordinator. Trafiken flödar endast i en riktning och det finns ingen direkt anslutning mellan avsändarna och mottagarna. Men vad händer om du inte vill använda denna anslutning? Anta att dina program behöver tooboth skicka och ta emot meddelanden, eller kanske du vill ha en direktlänk mellan dem och du behöver inte en broker toostore meddelanden. tooaddress scenarier som detta, Service Bus innehåller *vidarebefordrar*, enligt figur 4 visar.

![][4]

**Bild 4: Service Bus Relay erbjuder synkron, dubbelriktad kommunikation mellan program.**

Hej tooask naturliga frågan när det gäller reläer är detta: Varför ska jag använda det? Även om jag inte behöver köer, varför göra program kommunicerar via en molnbaserad tjänst i stället för bara interagera direkt? hello svaret är att prata direkt kan vara svårare än du tror.

Anta att du vill tooconnect två lokala program körs båda på företagets eget datacenter. Var och en av dessa appar ligger bakom en brandvägg och varje datacenter använder antagligen nätadressöversättning (NAT). hello brandväggen blockerar inkommande data på alla utom ett fåtal portar och NAT innebär att hello datorn varje program som körs på inte har en fast IP-adress som du kan nå direkt från utanför hello datacenter. Utan extra hjälp, ansluter programmen via hello offentliga internet är det problematiskt.

Det kan hjälpa att använda ett Service Bus-relä. toocommunicate båda riktningarna via ett relä, varje program upprättar en utgående TCP-anslutning med Service Bus och håller den öppen. All kommunikation mellan två program för hello överförs via dessa anslutningar. Eftersom varje anslutning har upprättats från i hello datacenter hello brandväggen tillåter inkommande trafik tooeach program utan att öppna nya portar. Den här metoden kringgår du också hello NAT-problem, eftersom varje App har en konsistent slutpunkt i hello molnet under hela hello-meddelande. Genom att utbyta data via hello relä undvika hello program hello problem som annars skulle ha gjort kommunikation svårt. 

toouse Service Bus reläer, program som förlitar sig på hello Windows Communication Foundation (WCF). Service Bus innehåller WCF-bindningar som gör det enkelt för Windows-program toointeract via reläer. Program som redan använder WCF kan vanligtvis ange något av dessa bindningar och sedan kommunicera tooeach andra via ett relä. Men till skillnad från köer och ämnen kräver användningen av reläer från andra program än Windows-program en hel del programmeringsarbete, även om det går att genomföra. Det medföljer inga standardbibliotek.

Till skillnad från köer och ämnen skapar apparna inga reläer rent uttryckligen. När ett program som du vill tooreceive meddelanden upprättar en TCP-anslutning med Service Bus, skapas istället ett relä automatiskt. Hello relay tas bort när hello anslutningen bryts. tooenable ett program toofind hello relay skapas av en viss lyssnare, Service Bus innehåller ett register som gör att program toolocate en specifik vidarebefordran efter namn.

Reläer är hello rätta lösningen när du behöver upprätta direkt kommunikation mellan program. Föreställ dig till exempel ett bokningssystem från ett flygbolag som körs i ett lokalt datacenter. Detta måste kunna nås från incheckningskiosker, mobila enheter och andra datorer. Program som körs på dessa system kan förlita sig på Service Bus relays i hello molnet toocommunicate, oavsett var de kan köras.

## <a name="summary"></a>Sammanfattning

Ansluta program har alltid varit en del för att skapa kompletta lösningar och hello olika scenarier som kräver att program och tjänster toocommunicate med varandra anges tooincrease som flera program och enheter är anslutna toohello internet. Genom att tillhandahålla molnbaserad teknik för att uppnå kommunikation via köer, ämnen och reläer, Service Bus syftar toomake denna absolut nödvändiga funktion enklare tooimplement och mer tillgänglig.

## <a name="next-steps"></a>Nästa steg

Nu när du har lärt dig grunderna hello i Azure Service Bus, följa dessa länkar toolearn mer.

* Hur toouse [Service Bus-köer](service-bus-dotnet-get-started-with-queues.md)
* Hur toouse [Service Bus-ämnen](service-bus-dotnet-how-to-use-topics-subscriptions.md)
* Hur toouse [Service Bus relay](../service-bus-relay/service-bus-dotnet-how-to-use-relay.md)
* [Service Bus-exempel](service-bus-samples.md)

[1]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_01_architecture.png
[2]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_02_queues.png
[3]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_03_topicsandsubscriptions.png
[4]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_04_relay.png
