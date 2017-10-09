---
title: "aaaAzure Service Bus vanliga frågor (FAQ) | Microsoft Docs"
description: "Besvarar några vanliga frågor om Azure Service Bus."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: cc75786d-3448-4f79-9fec-eef56c0027ba
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/07/2017
ms.author: sethm
ms.openlocfilehash: 71fe9eac46647a3e4026dbcaf2196984dd4b6a44
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="service-bus-faq"></a>Vanliga frågor och svar om Service Bus
Den här artikeln besvarar några vanliga frågor om Microsoft Azure Service Bus. Du kan också besöka hello [Azure stöder vanliga frågor och svar](http://go.microsoft.com/fwlink/?LinkID=185083) allmän Azure priser och support information.

## <a name="general-questions-about-azure-service-bus"></a>Allmänna frågor om Azure Service Bus
### <a name="what-is-azure-service-bus"></a>Vad är Azure Service Bus?
[Azure Service Bus](service-bus-messaging-overview.md) är en asynkron meddelandetjänst molnplattform som gör att du toosend data mellan frikopplad system. Microsoft erbjuder den här funktionen som en tjänst, vilket innebär att du inte behöver toohost någon maskinvara i ordning toouse den.

### <a name="what-is-a-service-bus-namespace"></a>Vad är en Service Bus-namnrymd?
En [namnområde](service-bus-create-namespace-portal.md) innehåller en omfattningsbehållare för adressering av Service Bus-resurser i ditt program. Skapa en är nödvändiga toouse Service Bus och är ett av hello första stegen i komma igång.

### <a name="what-is-an-azure-service-bus-queue"></a>Vad är en Azure Service Bus-kö?
En [Service Bus-kö](service-bus-queues-topics-subscriptions.md) är en entitet som meddelanden lagras. Köer är särskilt användbara när du har flera program eller flera delar av ett distribuerat program som behöver toocommunicate med varandra. hello kön är liknande tooa distributionscenter i att flera produkter (meddelanden) tas emot och skickas sedan från den platsen.

### <a name="what-are-azure-service-bus-topics-and-subscriptions"></a>Vad är Azure Service Bus-ämnen och prenumerationer?
Ett ämne kan vara visualiseras som en kö och när du använder flera prenumerationer, blir den en rikare meddelanden modellen. i grunden en en-till-många-kommunikationsverktyg. Den här modellen för Publicera/prenumerera (eller *pub/sub*) aktiverar ett program som skickar ett meddelande tooa ämne med flera prenumerationer toohave meddelandet tas emot av flera program.

### <a name="what-is-a-partitioned-entity"></a>Vad är en partitionerad enhet?
En vanlig kö eller ett ämne hanteras av en enda meddelande broker och lagras i ett meddelandearkiv. En [partitionerade kö eller ett ämne](service-bus-partitioning.md) hanteras av flera meddelandet mäklare och lagras i flera meddelandearkiv. Detta innebär att hello totala genomflödet av en partitionerad kö eller ett ämne begränsas inte längre av hello prestanda för ett enda meddelande broker eller meddelandearkiv. Dessutom kan återges ett tillfälligt avbrott i ett meddelandearkiv inte en partitionerad kö eller ett ämne inte tillgänglig.

Observera att sorteringen inte säkerställs när du använder partitionering entiteter. Du kan fortfarande skicka och ta emot meddelanden från hello andra partitioner i hello händelse att en partition är inte tillgänglig.

## <a name="best-practices"></a>Bästa praxis
### <a name="what-are-some-azure-service-bus-best-practices"></a>Vad är Azure Service Bus Metodtips?
* [Metodtips för bättre prestanda med hjälp av Service Bus] [ Best practices for performance improvements using Service Bus] – den här artikeln beskriver hur toooptimize prestanda när du skickar meddelanden.

### <a name="what-should-i-know-before-creating-entities"></a>Vad bör jag veta innan du skapar enheter?
följande egenskaper i en kö och avsnittet hello är oföränderliga. Du beakta detta när du etablerar entiteterna som den inte kan ändras, utan att skapa en ny entitet ersättning.

* Storlek
* Partitionering
* Sessioner
* Identifiering av dubbletter
* Express entitet

## <a name="pricing"></a>Prissättning
Det här avsnittet besvarar några vanliga frågor om hello Service Bus prisnivå struktur.

Hej [Service Bus priser och fakturering](service-bus-pricing-billing.md) artikeln förklarar hello fakturering mätare i Service Bus och information om Service Bus priser alternativ, se [Service Bus prisinformation](https://azure.microsoft.com/pricing/details/service-bus/).

Du kan också besöka hello [Azure svar](http://go.microsoft.com/fwlink/?LinkID=185083) för allmän Azure prisinformation. 

### <a name="how-do-you-charge-for-service-bus"></a>Hur du debiteras för Service Bus?
Fullständig information om priser för Service Bus finns [Service Bus prisinformation][Pricing overview]. Dessutom anges toohello priser, debiteras du för överföring av associerade data för utgående utanför hello datacenter där programmet har etablerats.

### <a name="what-usage-of-service-bus-is-subject-toodata-transfer-what-is-not"></a>Vilka användning av Service Bus är ämne toodata överföring? Vad är inte?
Alla dataöverföring inom en viss Azure-region tillhandahålls utan kostnad, samt eventuella inkommande dataöverföring. Dataöverföring utanför en region är ämne tooegress avgifter som du hittar [här](https://azure.microsoft.com/pricing/details/bandwidth/).

### <a name="does-service-bus-charge-for-storage"></a>Debiterar Service Bus för lagring
Nej, Service Bus inte debitera för lagring. Det finns dock en kvot begränsande hello maximal mängd data som kan vara beständiga per kö/avsnittet. Se hello nästa vanliga frågor och svar.

## <a name="quotas"></a>Kvoter

En lista över Service Bus gränser och kvoter finns hello [översikt över Service Bus-kvoter][Quotas overview].

### <a name="does-service-bus-have-any-usage-quotas"></a>Har Service Bus användning kvoter?
Som standard för alla moln anger tjänsten Microsoft en sammanställd månatlig kvot som har beräknats för alla prenumerationer för en kund. Eftersom vi förstår att du behöver mer än dessa gränser, kontakta kundservice när som helst så att vi kan förstå dina behov och justera dessa gränser korrekt. För Service Bus är hello sammanställd kvot 5 miljarder meddelanden per månad.

Medan vi reservera hello rätt toodisable ett kundkonto som har överskridit sina kvoter för användning i en viss månad, kommer att ange e-postavisering och göra flera försök toocontact en kund innan du vidtar någon åtgärd. Kunder som överstiger dessa kvoter ansvarar för avgifter som överskrider hello kvoter

Precis som med andra tjänster i Azure tillämpar Service Bus en uppsättning specifika kvoter tooensure att det är verkliga förbrukningen av resurser. Du hittar mer information om dessa kvoter i hello [översikt över Service Bus-kvoter][Quotas overview].

## <a name="troubleshooting"></a>Felsökning
### <a name="what-are-some-of-hello-exceptions-generated-by-azure-service-bus-apis-and-their-suggested-actions"></a>Vilka är några av hello-undantag som genereras av Azure Service Bus-API: er och föreslagna åtgärder?
En lista över Service Bus undantag, se [undantag översikt][Exceptions overview].

### <a name="what-is-a-shared-access-signature-and-which-languages-support-generating-a-signature"></a>Vad är en signatur för delad åtkomst och vilka språk som stöd för generering av en signatur?
Signaturer för delad åtkomst är en autentiseringsmekanism baserat på SHA-256 säker hashvärden eller URI: er. Information om hur toogenerate egna signaturer i noden, PHP, Java och C\#, se hello [signaturer för delad åtkomst] [ Shared Access Signatures] artikel.

## <a name="subscription-and-namespace-management"></a>Hantering av prenumerationen och namnområde
### <a name="how-do-i-migrate-a-namespace-tooanother-azure-subscription"></a>Hur jag för att migrera ett namnområde tooanother Azure-prenumeration?

Du kan flytta ett namnområde från en Azure-prenumeration tooanother med hjälp av antingen hello [Azure-portalen](https://portal.azure.com) eller PowerShell-kommandon. I ordning tooexecute hello åtgärd hello namnområdet måste redan vara aktiv. hello användaren hello-kommandon måste vara administratör på både hello källa och mål prenumerationer.

#### <a name="portal"></a>Portalen

toouse hello Azure portal toomigrate Service Bus-namnområden tooanother prenumeration, följer du anvisningarna hello [här](../azure-resource-manager/resource-group-move-resources.md#use-portal). 

#### <a name="powershell"></a>PowerShell

hello flyttar följande kommandosekvens PowerShell ett namnområde från en Azure-prenumeration tooanother. tooexecute den här åtgärden hello namnområde måste redan vara aktivt och hello-användaren som kör PowerShell-kommandon för hello måste vara administratör på båda hello käll- och prenumerationer.

```powershell
# Create a new resource group in target subscription
Select-AzureRmSubscription -SubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff'
New-AzureRmResourceGroup -Name 'targetRG' -Location 'East US'

# Move namespace from source subscription tootarget subscription
Select-AzureRmSubscription -SubscriptionId 'aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa'
$res = Find-AzureRmResource -ResourceNameContains mynamespace -ResourceType 'Microsoft.ServiceBus/namespaces'
Move-AzureRmResource -DestinationResourceGroupName 'targetRG' -DestinationSubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff' -ResourceId $res.ResourceId
```

## <a name="next-steps"></a>Nästa steg
toolearn mer om Service Bus finns i följande avsnitt hello.

* [Introduktion till Azure Service Bus Premium (blogginlägg)](http://azure.microsoft.com/blog/introducing-azure-service-bus-premium-messaging/)
* [Introduktion till Azure Service Bus Premium (Channel9)](https://channel9.msdn.com/Blogs/Subscribe/Introducing-Azure-Service-Bus-Premium-Messaging)
* [Översikt över Service Bus](service-bus-messaging-overview.md)
* [Översikt av Azure Service Bus-arkitektur](service-bus-fundamentals-hybrid-solutions.md)
* [Komma igång med Service Bus-köer](service-bus-dotnet-get-started-with-queues.md)

[Best practices for performance improvements using Service Bus]: service-bus-performance-improvements.md
[Best practices for insulating applications against Service Bus outages and disasters]: service-bus-outages-disasters.md
[Pricing overview]: https://azure.microsoft.com/pricing/details/service-bus/
[Quotas overview]: service-bus-quotas.md
[Exceptions overview]: service-bus-messaging-exceptions.md
[Shared Access Signatures]: service-bus-sas.md
