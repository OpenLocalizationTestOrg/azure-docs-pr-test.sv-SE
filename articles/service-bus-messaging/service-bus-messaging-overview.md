---
title: "aaaAzure Service Bus-meddelanden: översikt | Microsoft Docs"
description: Beskrivning av Service Bus-meddelanden och Azure Relay
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: f99766cb-8f4b-4baf-b061-4b1e2ae570e4
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: get-started-article
ms.date: 05/25/2017
ms.author: sethm
ms.openlocfilehash: ede2904e11544d8f9428a2d657dcc77dacd95ac4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="service-bus-messaging-flexible-data-delivery-in-hello-cloud"></a>Service Bus-meddelanden: flexibel dataöverföring i molnet hello
Microsoft Azure Service Bus är en tillförlitlig tjänst för informationsleverans. hello syftet med den här tjänsten är toomake kommunikation enklare. När två eller flera parter vill tooexchange information, behöver de en för att underlätta kommunikation. Service Bus är en asynkron kommunikationsmekanism, det vill säga via en tredje part. Detta är liknande tooa posttjänster i fysiska hälsningsmeddelande. Posttjänster gör det mycket enkelt toosend olika typer av brev och paket med en rad olika leveransgarantier vart som helst i hälsningsmeddelande.

Liknande toohello-posttjänster levererar brev, Service Bus är flexibel informationsleverans från både hello avsändare och mottagare för hello. hello meddelandetjänsten säkerställer att hello informationen levereras även om hello två parterna aldrig både online på hello samma tid eller om de inte är tillgänglig på hello exakt samma tid. På så sätt kan är meddelanden liknande toosending en bokstav, medan icke asynkron kommunikation är liknande tooplacing ett telefonsamtal (eller hur du använder ett telefonsamtal toobe - innan anropet väntande och anroparen-ID som är mycket mer likt asynkrona meddelanden).

hello avsändaren kan även kräva olika egenskaper för leveransen, inklusive transaktioner, identifiering av dubbletter, tidsbegränsad giltighet och batchbearbetning. Dessa mönster kan också jämföras med posttjänster: upprepad leverans, krav på signatur, adressändring eller återkallning.

Service Bus stöder två distinkta meddelandemönster: *Azure Relay* och *Service Bus-meddelanden*.

## <a name="azure-relay"></a>Azure Relay
Hej [vidarebefordrande WCF](../service-bus-relay/relay-what-is-it.md) komponent i Azure Relay är en centraliserad (men med hög Utjämning av nätverksbelastning) tjänst som stöder en mängd olika transportprotokoll och webbtjänststandarder. Detta inkluderar SOAP, WS-*, och även REST. Hej [vidarebefordrande tjänsten](../service-bus-relay/service-bus-dotnet-how-to-use-relay.md) innehåller en mängd olika anslutningsalternativ och kan hjälpa dig att förhandla direkta peer-to-peer-anslutningar när det är möjligt. Service Bus är optimerad för .NET-utvecklare som använder hello Windows Communication Foundation (WCF), båda med beaktande tooperformance och användbarhet, och ger fullständig åtkomst tooits vidarebefordringstjänst via SOAP- och REST-gränssnitt. Detta gör det möjligt för SOAP- och REST programming miljö toointegrate med Service Bus.

hello vidarebefordrande tjänsten stöder traditionella envägsmeddelanden, frågor och svar-meddelanden och peer-to-peer-meddelanden. Stöder även händelsedistribution på Internet-skala tooenable publicera-och prenumerationsscenarier och dubbelriktad socketkommunikation för ökad point-to-point effektivitet. I hello vidarebefordrande meddelandetjänster mönster, en lokal tjänst ansluter toohello vidarebefordrande tjänsten via en utgående port och skapar en dubbelriktad socket för kommunikation som är kopplad tooa viss rendezvous-adress. hello-klienten kan sedan kommunicera med hello lokala tjänsten genom att skicka meddelanden toohello vidarebefordrande tjänsten hello rendezvous-adressen som mål. hello vidarebefordrande tjänsten kommer sedan att ”vidarebefordra” meddelanden toohello lokala tjänsten via hello dubbelriktad socket redan på plats. hello klienten behöver inte ha en direktanslutning toohello lokal tjänst eller it krävs tooknow där hello-tjänsten finns och hello lokala tjänsten behöver inte ha några inkommande portar är öppna hello-brandväggen.

Du initierar hello anslutningen mellan din lokala tjänst och hello vidarebefordrande tjänsten med hjälp av en uppsättning ”vidarebefordra” WCF-bindningar. Hello bakgrunden mappas vidarebefordringsbindningarna hello tootransport bindning utformade toocreate WCF-kanalkomponenter som integreras med Service Bus i molnet hello.

WCF-relä ger många fördelar men kräver hello-servern och klienten tooboth vara online på hello samma tid i ordning toosend och ta emot meddelanden. Det är inte optimalt för kommunikation med HTTP-format, i vilken hello begäranden inte kan vanligtvis är långlivade, eller för klienter som ansluter bara ibland, till exempel webbläsare, mobila program, och så vidare. Asynkrona meddelandetjänster stöder frikopplad kommunikation och har sina fördelar. Klienter och servrar kan ansluta vid behov och utföra sina åtgärder på ett asynkront sätt.

## <a name="brokered-messaging"></a>Asynkron meddelandetjänst
Däremot toohello relay-schemat, Service Bus-meddelanden är eller [asynkron meddelandetjänst](service-bus-queues-topics-subscriptions.md) kan betraktas som asynkrona eller ”tillfälligt frånkopplade”. Producenter (avsändare) och konsumenter (mottagare) har inte toobe online på hello samtidigt. hello meddelandeinfrastrukturen lagrar på ett tillförlitligt sätt meddelanden i en ”broker” (till exempel en kö) tills hello konsumerande parten är redo tooreceive dem. Detta gör att hello komponenter i hello distribuerade program toobe frånkopplad, antingen frivilligt; till exempel för underhåll, eller på grund av tooa komponentkrasch, utan att påverka hello hela systemet. Dessutom hello mottagartillämpningen kan inte ha toocome online under vissa tider på hello dagen, till exempel ett lagersystem som endast är nödvändiga toorun hello slutet av hello arbetsdag.

hello kärnkomponenterna i hello asynkrona meddelandeinfrastruktur i Service Bus är köer, ämnen och prenumerationer.  hello viktigaste skillnaden är att avsnitt stöder funktioner för Publicera/prenumerera som kan användas för avancerad innehållsbaserad Routning och leverans logik, inklusive sändning toomultiple mottagare. De här komponenterna möjliggör nya asynkrona meddelandescenarier, till exempel temporär frikoppling, publicera/prenumerera och belastningsutjämning. Mer information om dessa meddelandeenheter finns på [Service Bus-köer, -ämnen och -prenumerationer](service-bus-queues-topics-subscriptions.md).

Som med hello vidarebefordrande WCF infrastruktur hello asynkron meddelandetjänst tillhandahålls kapaciteten för WCF- och .NET Framework-programmerare och även via REST.

## <a name="next-steps"></a>Nästa steg
toolearn mer om Service Bus-meddelanden finns i följande avsnitt hello.

* [Service Bus-grunder](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus-köer, ämnen och prenumerationer](service-bus-queues-topics-subscriptions.md)
* [Hur toouse Service Bus-köer](service-bus-dotnet-get-started-with-queues.md)
* [Hur toouse Service Bus-ämnen och prenumerationer](service-bus-dotnet-how-to-use-topics-subscriptions.md)

