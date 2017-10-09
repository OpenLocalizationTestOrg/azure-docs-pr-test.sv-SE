---
title: aaaService bussen priser och fakturering | Microsoft Docs
description: "Översikt över Service Bus priser struktur."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 7c45b112-e911-45ab-9203-a2e5abccd6e0
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/02/2017
ms.author: sethm
ms.openlocfilehash: 4d06fe015baba45fef04e198363447c5541d1724
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="service-bus-pricing-and-billing"></a>Service Bus priser och fakturering
Service Bus erbjuds i Basic, Standard, och [Premium](service-bus-premium-messaging.md) nivåer. Du kan välja en tjänstnivå för varje namnområde för Service Bus-tjänsten som du skapar och detta val av gäller över alla enheter som har skapats inom det här namnområdet.

> [!NOTE]
> Detaljerad information om aktuella Service Bus-priser finns hello [Azure Service Bus sida med priser](https://azure.microsoft.com/pricing/details/service-bus/), och hello [Service Bus FAQ](service-bus-faq.md#pricing).
>
>

Service Bus använder följande två mätare för köer och ämnen/prenumerationer hello:

1. **Messaging Operations**: definieras som API-anrop mot slutpunkter för kön eller ämne /-prenumeration. Den här mätaren ersätter meddelanden som skickas eller tas emot som hello primär enhet för fakturerbar användning för köer och ämnen-prenumerationer.
2. **Asynkrona anslutningar**: definierade som hello högsta antal beständiga anslutningar öppnas mot köer, ämnen och prenumerationer under en viss insamlingsperioden en timme. Den här mätaren tillämpas endast i hello standardnivån, kan du öppna ytterligare anslutningar (tidigare anslutningar har begränsad too100 per kö-avsnittet-prenumeration) avgift nominell per anslutning.

Hej **Standard** nivå introducerar gradvisa priser för åtgärder som utförs med köer och ämnen i prenumerationer, vilket resulterar i volym-baserade rabatter för too80% på hello högsta användning nivåer. Det finns också en grundläggande kostnad standardnivån $ 10 per månad, vilket gör att tooperform in too12.5 miljoner operationer per månad utan extra kostnad.

Hej **Premium** nivå ger resursisolering på hello processor- och minnesnivån så att varje kunds arbetsbelastning körs i isolering. Den här resursbehållaren kallas för en *meddelandefunktionsenhet*. Varje Premium-namnområde allokeras minst en meddelandefunktionsenhet. Du kan köpa 1, 2 eller 4 meddelandefunktionsenheter för varje Service Bus Premium-namnområde. En enda arbetsbelastning eller enhet kan spänna över flera meddelandefunktionsenheter och hello antalet meddelandefunktionsenheter kan ändras när du vill, även om faktureringen är per 24 timmar eller daglig taxa. hello resultatet är förutsägbara och repeterbara prestanda för Service Bus-lösningen. Prestanda är inte bara mer förutsägbara och tillgängliga, utan de är snabbare också.

Observera att hello standardnivån grundläggande kostnad debiteras en gång per månad per Azure-prenumeration. Det innebär att du kommer att kunna toocreate när du har skapat en enda Standard nivå Service Bus-namnrymd så många ytterligare Standard namnområden som du vill använda under den samma Azure-prenumerationen, utan att det medför ytterligare basera avgifter.

Hej [priser för Service Bus](https://azure.microsoft.com/pricing/details/service-bus/) tabell sammanfattas hello funktionella skillnader mellan hello Basic, Standard och Premium nivåer.

## <a name="messaging-operations"></a>Meddelandeåtgärder
Som en del av hello nya Prismodell, ändrar fakturering för köer och ämnen-prenumerationer. Dessa enheter övergång från fakturering per meddelande toobilling per åtgärd. En ”” avser tooany API-anrop som görs mot en tjänstslutpunkt kö eller ämne /-prenumeration. Detta omfattar åtgärder för hantering, skicka och ta emot och session tillstånd.

| Åtgärdstyp | Beskrivning |
| --- | --- |
| Hantering |Skapa, läsa, uppdatera, ta bort CRUD-mot köer och ämnen-prenumerationer. |
| Meddelandetjänster |Skicka och ta emot meddelanden med köer och ämnen-prenumerationer. |
| Sessionstillstånd |Hämtar eller anger sessionens tillstånd för en kö eller ett ämne /-prenumeration. |

Kostnadsinformation finns i hello priser som anges på hello [priser för Service Bus](https://azure.microsoft.com/pricing/details/service-bus/) sidan.

## <a name="brokered-connections"></a>Brokered Connections
*Asynkrona anslutningar* hantera kunden användningsmönster som rör ett stort antal ”beständigt ansluten” avsändare/mottagare mot köer, ämnen och prenumerationer. Beständigt anslutna avsändare/mottagare är de som ansluter via AMQP eller HTTP med icke-noll får timeout (till exempel HTTP lång avsökning). HTTP-avsändare och mottagare med en omedelbar timeout genererar inte asynkrona anslutningar.

Anslutningen kvoter och andra tjänstbegränsningarna finns hello [Service Bus-kvoter](service-bus-quotas.md) artikel.

hello standardnivån om du tar bort hello per namnområde asynkrona anslutningsgränsen och räknar sammanställd asynkrona anslutning användning över hello Azure-prenumeration. Mer information finns i hello [asynkrona anslutningar](https://azure.microsoft.com/pricing/details/service-bus/) tabell.

> [!NOTE]
> 1 000 asynkrona anslutningar ingår i hello meddelanden standardnivån (via hello grundläggande kostnad) och kan delas mellan alla köer, ämnen och prenumerationer i hello associerade Azure-prenumeration.
>
>

<br />

> [!NOTE]
> Fakturering baseras på hello högsta antal samtidiga anslutningar och är linjärt timvis baserat på 744 timmar per månad.
>
>

| Premiumnivå |
| --- |
| Asynkrona anslutningar debiteras inte i hello Premium-nivån. |

Mer information om asynkrona anslutningar finns hello [vanliga frågor och svar](#faq) senare i det här avsnittet.

## <a name="faq"></a>VANLIGA FRÅGOR OCH SVAR

### <a name="what-are-brokered-connections-and-how-do-i-get-charged-for-them"></a>Vad är asynkrona anslutningar och hur jag hämta debiteras för dem?
En asynkron anslutning har definierats som en av följande hello:

1. En AMQP-anslutning från en klient tooa Service Bus-kö eller ett ämne /-prenumeration.
2. Ett HTTP-anrop tooreceive ett meddelande från en Service Bus-ämne eller en kö som har en receive tidsgräns som är större än noll.

Service Bus kostnader för hello högsta antal samtidiga asynkrona anslutningar som överskrider hello med kvantitet (1 000 i hello standardnivån). Toppar mäts timme, linjärt genom att dividera med 744 timmar i månaden och läggs över hello Månadsvis fakturering period. hello ingår kvantitet (1 000 asynkrona anslutningar per månad) har tillämpats hello slutet av hello faktureringsperiod mot hello summan av hello linjärt timvis toppar.

Exempel:

1. Var och en av 10 000 enheter ansluter via en AMQP-anslutning och tar emot kommandon från en Service Bus-ämne. hello enheter skicka telemetri händelser tooan Event Hub. Om alla enheter ansluter 12 timmar varje dag, tillämpa hello efter anslutning avgifter (i tillägget tooany andra avgifter för Service Bus-avsnittet): 10 000 anslutningar * 12 timmar * 31 dagar / 744 = 5 000 asynkrona anslutningar. Efter hello månatlig ersättning av 1 000 asynkrona anslutningar, skulle du debiteras för 4 000 asynkrona anslutningar med hello $0.03 per asynkrona anslutning, för totalt $120.
2. 10 000 enheter ta emot meddelanden från en Service Bus-kö via HTTP, med en icke-noll-timeout. Om alla enheter ansluter 12 timmar varje dag, visas hello efter anslutning avgifter (i tillägget tooany andra Service Bus-avgifter): 10 000 ta emot HTTP-anslutningar * 12 timmar per dag * 31 dagar / 744 timmar = 5 000 asynkrona anslutningar.

### <a name="do-brokered-connection-charges-apply-tooqueues-and-topicssubscriptions"></a>Använd asynkrona anslutning avgifter tooqueues och avsnitt/prenumerationer?
Ja. Det finns inga avgifter för anslutning för att skicka händelser med hjälp av HTTP, oavsett hello antalet skickar datorer eller enheter. Ta emot händelser med hjälp av en tidsgräns som är större än noll, kallas ibland ”länge avsökning”, HTTP genererar asynkrona anslutning avgifter. AMQP anslutningar generera asynkrona anslutning avgifter oavsett om hello anslutningar som ska använda toosend eller ta emot. Observera att 100 asynkrona anslutningar tillåts utan kostnad i en grundläggande namnrymd. Detta är även hello maximala antalet asynkrona anslutningar som tillåts för hello Azure-prenumeration. hello ingår 1 000 första asynkrona anslutningar över alla namnområden som Standard i en Azure-prenumeration utan extra kostnad (utöver hello grundläggande kostnad). Eftersom dessa tillägg är tillräckligt med toocover blivit många service to service meddelandescenarier, asynkrona anslutning avgifter vanligtvis bara relevant om du planerar toouse AMQP eller HTTP lång-avsökning med ett stort antal klienter. till exempel tooachieve effektivare händelse strömning eller Aktivera dubbelriktad kommunikation med många enheter eller programinstanser.

## <a name="next-steps"></a>Nästa steg
* Mer information om Service Bus priser finns i hello [Service Bus sida med priser](https://azure.microsoft.com/pricing/details/service-bus/).
* Se hello [Service Bus FAQ](service-bus-faq.md#pricing) för några vanliga frågor och svar om Service bus priser och fakturering.

[Azure portal]: https://portal.azure.com
