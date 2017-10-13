---
title: "Översikt över Azure Service Bus-meddelanden | Microsoft Docs"
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
ms.date: 08/30/2017
ms.author: sethm
ms.openlocfilehash: 462fa0a6e86237b8c9d9ba97aa377f8d112a8b8e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="service-bus-messaging-flexible-data-delivery-in-the-cloud"></a>Service Bus-meddelanden: flexibel dataöverföring i molnet

Microsoft Azure Service Bus är en tillförlitlig tjänst för informationsleverans. Syftet med den här tjänsten är att underlätta kommunikation. När två eller flera parter vill utbyta information behöver de något som gör kommunikationen möjlig. Service Bus är en asynkron kommunikationsmekanism, det vill säga via en tredje part. Den liknar posttjänster i den fysiska världen. Med posttjänster är det mycket enkelt att skicka olika typer av brev och paket med en rad olika leveransgarantier vart som helst i världen.

På ett liknande sätt som posttjänster levererar brev ger Service Bus en flexibel informationsleverans från både avsändaren och mottagaren. Meddelandetjänsten säkerställer att informationen levereras även om båda parterna aldrig är online samtidigt, eller om de inte är tillgängliga i exakt samma ögonblick. På så vis liknar meddelanden brev, medan icke asynkron kommunikation liknar att ringa ett telefonsamtal (eller hur telefonsamtal brukade vara innan vi hade väntande samtal och nummerpresentatörer, som är mycket mer likt asynkrona meddelanden).

Avsändaren kan även kräva olika egenskaper för leveransen, inklusive transaktioner, identifiering av dubbletter, tidsbegränsad giltighet och batchbearbetning. Dessa mönster kan också jämföras med posttjänster: upprepad leverans, krav på signatur, adressändring eller återkallning.

Service Bus stöder två distinkta meddelandemönster: *Azure Relay* och *Service Bus-meddelanden*.

## <a name="azure-relay"></a>Azure Relay

Den [vidarebefordrande](../service-bus-relay/relay-what-is-it.md) komponenten i Azure Relay är en centraliserad tjänst (men med hög utjämning av nätverksbelastning) som stöder en mängd olika transportprotokoll och webbtjänststandarder. Detta inkluderar SOAP, WS-*, och även REST. Den [vidarebefordrande tjänsten](../service-bus-relay/service-bus-dotnet-how-to-use-relay.md) innehåller en mängd olika anslutningsalternativ för vidarebefordran och kan hjälpa dig att förhandla direkta peer-to-peer-anslutningar när det är möjligt. Service Bus är optimerad för .NET-utvecklare som använder Windows Communication Foundation (WCF) både vad gäller prestanda och användbarhet, och ger fullständig åtkomst till dess vidarebefordringstjänst via SOAP- och REST-gränssnitt. Detta gör det möjligt för SOAP- och REST-programmeringsmiljöer att integrera med Service Bus.

Den vidarebefordrande tjänsten stöder traditionella envägsmeddelanden, fråga-och-svar-meddelanden och peer-to-peer-meddelanden. Den stöder även händelsedistribution på Internet-skala för att möjliggöra publicerings- och prenumerationsscenarier och dubbelriktad socketkommunikation för ökad effektivitet punkt till punkt. I mönstret vidarebefordrande meddelandetjänster ansluter en lokal tjänst till den vidarebefordrande tjänsten via en utgående port och skapar en dubbelriktad socket för kommunikation som är kopplad till en viss rendezvous-adress. Klienten kan sedan kommunicera med den lokala tjänsten genom att skicka meddelanden till den vidarebefordrande tjänsten med rendezvous-adressen som mål. Den vidarebefordrande tjänsten kommer sedan att ”vidarebefordra” meddelanden till den lokala tjänsten via den dubbelriktade socketen som redan är upprättad. Klienten behöver ingen direkt anslutning till den lokala tjänsten och behöver inte heller veta var den finns. Den lokala tjänsten behöver inte ha några öppna ingående portar i brandväggen.

Du upprättar anslutningen mellan din lokala tjänst och den vidarebefordrande tjänsten med hjälp av en uppsättning ”vidarebefordrande” WCF-bindningar. I bakgrunden mappas vidarebefordringsbindningarna till transportbindningselement som är utformade för att skapa WCF-kanalkomponenter som integreras med Service Bus i molnet.

WCF Relay ger många fördelar men kräver att både servern och klienten är online på samma gång för att kunna skicka och ta emot meddelanden. Det är inte optimalt för kommunikation med HTTP-format, där frågorna kanske vanligtvis inte är långlivade, eller för klienter som ansluter bara ibland, till exempel webbläsare, mobila program, och så vidare. Asynkrona meddelandetjänster stöder frikopplad kommunikation och har sina fördelar. Klienter och servrar kan ansluta vid behov och utföra sina åtgärder på ett asynkront sätt.

## <a name="brokered-messaging"></a>Asynkron meddelandetjänst

Till skillnad från det vidarebefordrande schemat kan Service Bus-meddelandefunktionen med [köer, ämnen och prenumerationer](service-bus-queues-topics-subscriptions.md) betraktas som icke-synkroniserade, eller "tillfälligt frånkopplade". Producenter (avsändare) och konsumenter (mottagare) behöver inte vara online samtidigt. Meddelandeinfrastrukturen lagrar meddelanden på ett tillförlitligt sätt hos en ”mellanhand” (till exempel en kö) tills konsumentparten är redo att ta emot dem. Detta gör att komponenterna i den distribuerade appen kan frikopplas, antingen frivilligt, till exempel för underhåll, eller på grund av en komponentkrasch, utan att detta påverkar hela systemet. Dessutom behöver det mottagande programmet bara vara anslutet vid vissa tidpunkter på dagen, till exempel ett lagersystem som endast behöver köras i slutet av arbetsdagen.

Kärnkomponenterna i infrastrukturen för Service Bus-meddelanden är köer, ämnen och prenumerationer. Den viktigaste skillnaden är att avsnitt stöder funktioner för publicering/prenumereration som kan användas för avancerad innehållsbaserad routning och leveranslogik, inklusive sändning till flera mottagare. De här komponenterna möjliggör nya asynkrona meddelandescenarier, till exempel temporär frikoppling, publicera/prenumerera och belastningsutjämning. Mer information om dessa meddelandeenheter finns på [Service Bus-köer, -ämnen och -prenumerationer](service-bus-queues-topics-subscriptions.md).

Som med WCF-infrastrukturen för vidarebefordrande har den asynkrona meddelandetjänsten funktioner för WCF- och .NET Framework-programmerare och även via REST.

## <a name="next-steps"></a>Nästa steg

I följande ämnen kan du lära dig mer om Service Bus-meddelanden.

* [Service Bus-grunder](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus-köer, ämnen och prenumerationer](service-bus-queues-topics-subscriptions.md)
* [Komma igång med Service Bus-köer](service-bus-dotnet-get-started-with-queues.md)
* [Använd Service Bus ämnen och prenumerationer](service-bus-dotnet-how-to-use-topics-subscriptions.md)

