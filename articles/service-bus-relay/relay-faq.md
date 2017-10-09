---
title: "aaaAzure Relay vanliga frågor och svar | Microsoft Docs"
description: "Få svar toosome vanliga frågor och svar om Azure Relay."
services: service-bus-relay
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 886d2c7f-838f-4938-bd23-466662fb1c8e
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/23/2017
ms.author: sethm
ms.openlocfilehash: ab14431e27df43287940e7d12ea37e4093638d21
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-relay-faqs"></a>Azure Relay vanliga frågor och svar

Den här artikeln besvarar några vanliga frågor (FAQ) om [Azure Relay](https://azure.microsoft.com/services/service-bus/). För allmän Azure priser och support information, se [Azure stöder vanliga frågor och svar](http://go.microsoft.com/fwlink/?LinkID=185083).

## <a name="general-questions"></a>Allmänna frågor
### <a name="what-is-azure-relay"></a>Vad är Azure Relay?
Hej [Azure vidarebefordrande tjänsten](relay-what-is-it.md) underlättar för dina hybridprogram genom att hjälpa dig mer på ett säkert sätt exponera tjänster som finns i ett företagsnätverk som kopplar nätverk toohello offentligt moln. Du kan exponera hello tjänster utan att öppna en brandväggsanslutning och utan att kräva störande ändringar tooa företagets nätverksinfrastruktur.

### <a name="what-is-a-relay-namespace"></a>Vad är ett namnområde för Relay?
En [namnområde](relay-create-namespace-portal.md) är en omfattningsbehållare som du kan använda tooaddress Relay resurser i ditt program. Du måste skapa ett namnområde toouse Relay. Detta är en hello första stegen i komma igång.

### <a name="what-happened-tooservice-bus-relay-service"></a>Vilka happened tooService service Bus Relay?
hello som tidigare kallades Service Bus Relay tjänsten kallas nu WCF Relay. Du kan fortsätta toouse den här tjänsten som vanligt. Hej Hybridanslutningar funktionen är en uppdaterad version av en tjänst som har varit transplanteras från Azure BizTalk-tjänst. Vidarebefordrande WCF och Hybridanslutningar fortsätta toobe som stöds.

## <a name="pricing"></a>Prissättning
Det här avsnittet svar några vanliga frågor om hello Relay priser struktur. Du kan också se [Azure svar](http://go.microsoft.com/fwlink/?LinkID=185083) för allmän Azure prisinformation. Fullständig information om priser Relay finns [Service Bus prisinformation][Pricing overview].

### <a name="how-do-you-charge-for-hybrid-connections-and-wcf-relay"></a>Hur du debiteras för Hybridanslutningar och WCF Relay?
Fullständig information om priser för Relay finns hello [Hybridanslutningar och WCF-reläer] [ Pricing overview] tabellen på informationssidan för hello Service Bus prisnivå. Dessutom anges toohello priser på sidan, debiteras du för överföring av associerade data för utgående utanför hello datacenter där programmet har etablerats.

### <a name="how-am-i-billed-for-hybrid-connections"></a>Hur är debiteras för Hybridanslutningar?
Här följer tre fakturering exempelscenarier för Hybridanslutningar:

*   Scenario 1:
    *   Du har en enda lyssnare, till exempel en instans av hello Hybrid anslutningar Manager installerad och körs kontinuerligt för hello hela månaden.
    *   Du kan skicka 3 GB data via hello under hello månad. 
    *   Din totala är $5.
*   Scenario 2:
    *   Du har en enda lyssnare, till exempel en instans av hello Hybrid anslutningar Manager installerad och körs kontinuerligt för hello hela månaden.
    *   Du kan skicka 10 GB data via hello under hello månad.
    *   Din totala är $7,50. Som är $5 för hello-anslutning och första 5 GB + $2,50 för hello ytterligare 5 GB data.
*   Scenario 3:
    *   Du har två instanser A och B i hello Hybrid anslutningar Manager installerad och körs kontinuerligt för hello hela månaden.
    *   Du kan skicka 3 GB data via en under hello månad.
    *   Du kan skicka 6 GB data via anslutning B under hello månad.
    *   Din totala är $10,50. Det är $5 för anslutning A + $5 för anslutning B + 0,50 $ (för hello sjätte gigabyte på anslutning B).

Observera att hello priser som används i hello exempel kan bara användas under hello Hybridanslutningar förhandsversionen. Priser som är ämne toochange vid allmän tillgänglighet för Hybridanslutningar.

### <a name="how-are-hours-calculated-for-relay"></a>Hur beräknas timmar för Relay?

Vidarebefordrande WCF är endast tillgänglig i standardnivån namnområden. Priser och [anslutning kvoter](../service-bus-messaging/service-bus-quotas.md) för reläer annars inte har ändrats. Detta innebär att reläer fortsätta toobe debiteras baserat på hello antal meddelanden (inte operations) och vidarebefordra timmar. Mer information finns i hello [”Hybrid anslutningar och WCF reläer”](https://azure.microsoft.com/pricing/details/service-bus/) tabellen på hello informationssida med priser.

### <a name="what-if-i-have-more-than-one-listener-connected-tooa-specific-relay"></a>Vad händer om jag har fler än en lyssnare anslutna tooa specifika relay?
I vissa fall kan kan en enda relay ha flera anslutna lyssnare. Öppna betraktas som ett relä när minst en relay-lyssnaren är anslutna tooit. Lägger till lyssnare tooan öppna relay resultat i ytterligare relätimmar. Hej numret för relä påverkar inte avsändare (klienter som anropar eller skicka meddelanden toorelays) som är anslutna tooa relay hello beräkning av relätimmar.

### <a name="how-is-hello-messages-meter-calculated-for-wcf-relays"></a>Hur beräknas hello meddelanden mätaren för WCF-reläer?
(**Detta gäller endast tooWCF reläer. Meddelanden är inte en kostnad för Hybridanslutningar.** )

I allmänhet fakturerbar meddelanden för reläer beräknas med hjälp av hello samma metod som används för asynkrona entiteter (köer, ämnen och prenumerationer), som beskrivs ovan. Det finns dock några viktiga skillnader.

Skicka ett meddelande tooa Service Bus relay behandlas som en ”full via” skicka toohello relay lyssnare som tar emot hello-meddelande. Den behandlas inte som en skicka åtgärden toohello Service Bus relay, följt av en leverans toohello relay-lyssnare. Ett request-reply-style service-anrop (av upp too64 KB) mot en relay-lyssnaren resultatet i två fakturerbar meddelanden: en fakturerbar meddelande för hello begäran och en fakturerbar meddelandet för hello svar (förutsatt att hello svar är också 64 KB eller mindre). Detta skiljer sig med hjälp av en kö toomediate mellan en klient och en tjänst. Om du använder en kö toomediate mellan en klient och en tjänst hello samma request-reply-mönstret kräver en begäran toohello överföringskön, följt av dequeue/leverans från hello kötjänsten toohello. Detta följs av en svarskö skicka tooanother och dequeue/leverans från kön toohello klienten. Med samma storlek antaganden i hela (upp too64 KB) hello genom hello kön mönster resulterar i 4 fakturerbar meddelanden. Faktureras för två gånger hello antal meddelanden tooimplement hello samma mönster som du skapade med relay. Naturligtvis finns fördelar toousing köer tooachieve detta mönster, till exempel hållbarhet och utjämna belastningen. Dessa fördelar kan motivera hello ytterligare kostnader.

Reläer som öppnas med hjälp av hello **netTCPRelay** WCF bindning behandlar meddelanden inte som enskilda meddelanden, men som en dataström som passerar genom hello system. När du använder den här bindningen bara hello avsändaren och lyssnare har insyn i hello synkroniseringstecken hello enskilda meddelanden skickas och tas emot. För reläer som använder hello **netTCPRelay** bindning, behandlas alla data som en dataström för att beräkna fakturerbar meddelanden. I det här fallet beräknar Service Bus hello totala mängden data som skickas eller tas emot via varje enskild relay på grundval av 5 minuter. Sedan dividerar den den totala mängden data med 64 KB toodetermine hello antal fakturerbar meddelanden för att relay under den tidsperioden.

## <a name="quotas"></a>Kvoter
| Kvoten namn | Omfång | Typ | Beteende när överskridits | Värde |
| --- | --- | --- | --- | --- |
| Samtidiga lyssnare på ett relä |Entitet |Statisk |Efterföljande begäranden om ytterligare anslutningar avvisas och ett undantag tas emot av hello anropa kod. |25 |
| Samtidiga relay-lyssnare |Systemomfattande |Statisk |Efterföljande begäranden om ytterligare anslutningar avvisas och ett undantag tas emot av hello anropa kod. |2,000 |
| Samtidiga relay-anslutningar per alla relay-slutpunkterna i ett namnområde för tjänsten |Systemomfattande |Statisk |- |5,000 |
| Relay slutpunkter per namnområde för tjänsten |Systemomfattande |Statisk |- |10 000 |
| Meddelandestorlek för [NetOnewayRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.netonewayrelaybinding.aspx) och [NetEventRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.neteventrelaybinding.aspx) vidarebefordrar |Systemomfattande |Statisk |Inkommande meddelanden som överskrider dessa kvoter avvisas och ett undantag tas emot av hello anropa kod. |64 kB |
| Meddelandestorlek för [HttpRelayTransportBindingElement](https://msdn.microsoft.com/library/microsoft.servicebus.httprelaytransportbindingelement.aspx) och [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) vidarebefordrar |Systemomfattande |Statisk |- |Obegränsat |

### <a name="does-relay-have-any-usage-quotas"></a>Har Relay användning kvoter?
Som standard för alla Molntjänsten anger Microsoft en sammanställd månatlig kvot som har beräknats för alla prenumerationer för en kund. Vi förstår att dina behov ibland kanske överskrider gränserna. Du kan kontakta kundtjänst när som helst, så att vi kan förstå dina behov och justera dessa gränser korrekt. För Service Bus är hello sammanställd användning kvoter följande:

* 5 miljarder meddelanden
* 2 miljoner relätimmar

Även om vi reservera hello rätt toodisable ett konto som överskrider dess månadens kvot för användning, vi tillhandahåller e-postavisering och vi göra flera försök toocontact hello kunden innan du vidtar någon åtgärd. Kunder som överskrider dessa kvoter är fortfarande ansvarig för överdriven avgifter.

### <a name="naming-restrictions"></a>Namngivningsbegränsningar
Ett namn för namnområdet Relay måste vara mellan 6 och 50 tecken.

## <a name="subscription-and-namespace-management"></a>Hantering av prenumerationen och namnområde
### <a name="how-do-i-migrate-a-namespace-tooanother-azure-subscription"></a>Hur jag för att migrera ett namnområde tooanother Azure-prenumeration?

toomove ett namnområde från en Azure-prenumeration tooanother prenumeration kan du antingen använda hello [Azure-portalen](https://portal.azure.com) eller använda PowerShell-kommandon. toomove en namnområde tooanother prenumeration hello namnområde måste redan att vara aktiv. hello-användaren som kör hello kommandon måste vara en administratörsanvändare på båda hello käll- och prenumerationer.

#### <a name="azure-portal"></a>Azure Portal

toouse hello Azure portal toomigrate Azure Relay-namnområden från en prenumeration tooanother prenumeration finns [flytta resurser tooa ny resursgrupp eller prenumeration](../azure-resource-manager/resource-group-move-resources.md#use-portal). 

#### <a name="powershell"></a>PowerShell

toouse PowerShell toomove ett namnområde från en Azure-prenumeration tooanother prenumeration, Använd hello följande kommandosekvens. tooexecute den här åtgärden hello namnområde måste redan vara aktivt och hello-användaren som kör hello PowerShell-kommandon måste vara en administratörsanvändare på båda hello käll- och prenumerationer.

```powershell
# Create a new resource group in hello target subscription.
Select-AzureRmSubscription -SubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff'
New-AzureRmResourceGroup -Name 'targetRG' -Location 'East US'

# Move hello namespace from hello source subscription toohello target subscription.
Select-AzureRmSubscription -SubscriptionId 'aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa'
$res = Find-AzureRmResource -ResourceNameContains mynamespace -ResourceType 'Microsoft.ServiceBus/namespaces'
Move-AzureRmResource -DestinationResourceGroupName 'targetRG' -DestinationSubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff' -ResourceId $res.ResourceId
```

## <a name="troubleshooting"></a>Felsökning
### <a name="what-are-some-of-hello-exceptions-generated-by-azure-relay-apis-and-suggested-actions-you-can-take"></a>Vad är några av hello undantag genereras av Azure Relay API: er och föreslagna åtgärder du kan vidta?
En beskrivning av vanliga undantag och föreslagna åtgärder du kan vidta finns [Relay undantag][Relay exceptions].

### <a name="what-is-a-shared-access-signature-and-which-languages-can-i-use-toogenerate-a-signature"></a>Vad är en signatur för delad åtkomst och vilka språk kan jag använda toogenerate en signatur?
Delad åtkomst signaturer (SAS) är en autentiseringsmekanism baserat på SHA-256 säker hashvärden eller URI: er. Information om hur toogenerate egna signaturer i noden, PHP, Java, C och C#, se [Service Bus-autentisering med signaturer för delad åtkomst][Shared Access Signatures].

### <a name="is-it-possible-toowhitelist-relay-endpoints"></a>Det är möjligt toowhitelist relay slutpunkter?
Ja. hello relay klienten ansluter toohello Azure vidarebefordrande tjänsten med hjälp av fullständigt kvalificerade domännamn. Kunder kan lägga till en post för `*.servicebus.windows.net` på brandväggar som stöder vitlistning av DNS.

## <a name="next-steps"></a>Nästa steg
* [Skapa ett namnområde](relay-create-namespace-portal.md)
* [Kom igång med .NET](relay-hybrid-connections-dotnet-get-started.md)
* [Kom igång med Node](relay-hybrid-connections-node-get-started.md)

[Pricing overview]: https://azure.microsoft.com/pricing/details/service-bus/
[Relay exceptions]: relay-exceptions.md
[Shared access signatures]: ../service-bus-messaging/service-bus-sas.md