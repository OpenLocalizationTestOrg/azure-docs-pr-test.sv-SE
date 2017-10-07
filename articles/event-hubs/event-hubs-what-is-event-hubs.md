---
title: "aaaWhat är Händelsehubbar i Azure och varför ska jag använda den | Microsoft Docs"
description: "Översikt över och introduktion tooAzure Händelsehubbar - införandet av skalbar Molnlagring telemetri från webbplatser, appar och enheter"
services: event-hubs
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2017
ms.author: sethm; babanisa
ms.openlocfilehash: f6199a2e5bee8506f529b6f561234d79f9c8d465
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-event-hubs"></a>Vad är Event Hubs?

Azure Event Hubs är en mycket skalbar dataströmningsplattform och händelseinmatningstjänst som kan ta emot och bearbeta flera miljoner händelser per sekund. Event Hubs kan bearbeta och lagra händelser, data eller telemetri som producerats av distribuerade program och enheter. Data skickas tooan händelsehubb kan omvandlas och lagras med hjälp av en leverantör av realtidsanalys eller adaptrar för batchbearbetning/lagring. Med hello möjlighet tooprovide [funktioner för att publicera och prenumerera](https://msdn.microsoft.com/library/aa560414.aspx) med låg latens och i massiv skala Händelsehubbar fungerar som hello ”på ramp” för Stordata.

## <a name="why-use-event-hubs"></a>Varför ska jag använda Event Hubs?

Händelse- och telemetrihanteringsfunktionerna gör Event Hubs särskilt lämpligt för:

* Programinstrumentering
* Bearbetning av användarupplevelser och arbetsflöden
* Scenarier i sakernas internet (IoT)

Andra funktioner i Event Hubs är till exempel beteendespårning i mobilappar, trafikinformation från webbservergrupper, inspelning av spelhändelser i konsolspel eller telemetri som samlats in från industriella datorer, anslutna fordon eller andra enheter.

## <a name="azure-event-hubs-overview"></a>Översikt av händelsehubbar i Azure

hello vanlig roll som Händelsehubbar spelar i lösningsarkitekturer är hello ”ytterdörren” för en händelsepipeline, ofta kallad en *händelseinmatare*. En händelseinmatare är en komponent eller tjänst som placeras mellan händelseutgivare och händelsen konsumenter toodecouple hello framställning av en händelseström från hello användningen av dessa händelser. hello visar följande bild den här arkitekturen:

![Händelsehubbar](./media/event-hubs-what-is-event-hubs/event_hubs_full_pipeline.png)

Event Hubs tillhandahåller funktioner för hantering av meddelandeströmmar men har egenskaper som skiljer sig från traditionell meddelandehantering för företag. Funktionerna i Event Hubs är byggda för scenarier med högt genomflöde och intensiv händelsebearbetning. Därför Händelsehubbar skiljer sig från [Azure Service Bus](https://azure.microsoft.com/services/service-bus/) meddelanden och implementerar inte några av hello-funktioner som är tillgängliga för [Service Bus-meddelanden](/azure/service-bus-messaging/) entiteter, till exempel avsnitt.

## <a name="event-hubs-features"></a>Event Hubs-funktioner

Händelsehubbar innehåller följande nyckelelement hello:

- [**Producenter/händelseutfärdare**](event-hubs-features.md#event-publishers): en enhet som skickar data tooan händelsehubb. En händelse publiceras via AMQP 1.0 eller HTTPS.
- [**Avbilda**](event-hubs-features.md#capture): kan du toocapture Händelsehubbar strömmande data och lagrar den i ett Azure Blob storage-konto.
- [**Partitioner**](event-hubs-features.md#partitions): aktiverar varje konsument tooonly Läs en viss delmängd, eller partition, av hello händelseströmmen.
- [**SAS-token**](event-hubs-features.md#sas-tokens): identifierar och autentiserar hello händelseutfärdare.
- [**Händelsekonsument**](event-hubs-features.md#event-consumers): En enhet som läser händelsedata från en händelsehubb. Händelsekonsumenter ansluter via AMQP 1.0. 
- [**Konsumentgrupper**](event-hubs-features.md#consumer-groups): ger varje flera förbrukar program med en separat vy över händelseströmmen hello, aktivera dessa konsumenter tooact oberoende av varandra.
- [**Genomflödesenheter**](event-hubs-features.md#capacity): Förköpta kapacitetsenheter. En enskild partition har en maximal skala på en genomflödesenhet.

Teknisk information om dessa och andra funktioner i Händelsehubbar finns hello [översikt över funktioner i Händelsehubbar](event-hubs-features.md). 

## <a name="next-steps"></a>Nästa steg

Utförlig prisinformation för Event Hubs finns i [Priser för Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/).

Mer information om Händelsehubbar finns i hello följande länkar:

* Kom igång med en [kurs om händelsehubbar](event-hubs-dotnet-standard-getstarted-send.md)
* [Vanliga frågor och svar om Event Hubs](event-hubs-faq.md)
* [Exempelprogram som använder Event Hubs](https://github.com/Azure/azure-event-hubs/tree/master/samples)
 
 

