---
title: "aaaAzure Event Hubs vanliga frågor och svar | Microsoft Docs"
description: "Händelsehubbar i Azure vanliga frågor (FAQ)"
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: bfa10984-eb22-4671-861a-f377a90d9372
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2017
ms.author: sethm;shvija
ms.openlocfilehash: cc0844edcc38ad167cde9497d58d44155fc90b7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="event-hubs-frequently-asked-questions"></a>Vanliga och frågor svar om Händelsehubbar

## <a name="general"></a>Allmänt

### <a name="what-is-hello-difference-between-event-hubs-basic-and-standard-tiers"></a>Vad är hello skillnaden mellan Event Hubs Basic och Standard nivåer?

hello standardnivån av Händelsehubbar i Azure tillhandahåller funktioner utöver vad som är tillgängliga i hello grundläggande nivån. hello följande funktioner ingår i Standard:
* Längre kvarhållning av händelse
* Ytterligare asynkrona anslutningar med en överförbrukning kostnad för mer än hello-nummer
* Mer än en enskild konsument-grupp
* [Avbilda](https://docs.microsoft.com/azure/event-hubs/event-hubs-capture-overview)

Mer information om prisnivåer, inklusive Event Hubs dedikerade, se hello [Händelsehubbar prisinformationen](https://azure.microsoft.com/pricing/details/event-hubs/).

### <a name="what-are-event-hubs-throughput-units"></a>Vad är Händelsehubbar enheter?

Du väljer uttryckligen Händelsehubbar genomflödesenheter, antingen via hello Azure-portalen eller Event Hubs Resource Manager-mallar. Genomflödesenheter gäller tooall händelsehubbar i ett namnområde för Händelsehubbar och varje genomflödesenhet kan hello namnområde toohello följande funktioner:

* Konfigurera too1 MB per sekund för ingångshändelser (händelser skickas till en händelsehubb), men inga fler än 1000 ingångshändelser, hanteringsåtgärder eller kontrollen API-anrop per sekund.
* Konfigurera too2 MB per sekund utgång händelser (händelser används från en händelsehubb).
* Konfigurera too84 GB händelse lagring (tillräckligt för hello 24-timmarsformat loggperioden).

Event Hubs genomflödesenheter debiteras timvis, baserat på hello maximala antalet enheter som valdes under hello angivna timme.

### <a name="how-are-event-hubs-throughput-unit-limits-enforced"></a>Hur tillämpas Händelsehubbar genomströmning enhet gränser?
Om hello totala ingång genomströmning eller hello totala ingång händelsehastighet för alla händelsehubbar i ett namnområde överskrider hello sammanställda genomflöde enhet bidrag, avsändare har begränsats och felmeddelanden som anger att hello ingång kvot har överskridits.

Om hello totala utgång genomströmning eller hello totala utgång händelsehastighet för alla händelsehubbar i ett namnområde överskrider hello sammanställda genomflöde enhet bidrag, mottagare har begränsats och felmeddelanden som anger att hello utgång kvot har överskridits. Ingång och utgång kvoter tillämpas separat, så att ingen avsändare kan orsaka händelse förbrukning tooslow ned eller en mottagare förhindra händelser skickas i en händelsehubb.

### <a name="is-there-a-limit-on-hello-number-of-throughput-units-that-can-be-selected"></a>Finns det en gräns för hello antalet enheter som kan väljas?
Det finns en standardkvot på 20 genomflödesenheter per namnområde. Du kan begära en större kvot på enheter genom att arkivera ett supportärende. Gräns hello 20 genomströmning enhet är paket tillgängliga i 20 och 100 genomflödesenheter. Observera att använda fler än 20 genomflödesenheter tar bort hello möjlighet toochange hello antal enheter utan att arkivera ett supportärende.

### <a name="can-i-use-a-single-amqp-connection-toosend-and-receive-from-multiple-event-hubs"></a>Kan jag använda en enda AMQP anslutning toosend och ta emot från flera händelsehubbar?
Ja, så länge som alla hello händelsehubbar finns i hello samma namnområde.

### <a name="what-is-hello-maximum-retention-period-for-events"></a>Vad är hello högsta bevarandeperioden för händelser?
Event Hubs standardnivån stöder för närvarande en högsta loggperioden 7 dagar. Observera att händelsehubbar inte avsedd som en permanent datalager. Kvarhållningsperioder som är större än 24 timmar är avsedda för scenarier där det är praktiskt tooreplay en händelse strömma till hello samma system. till exempel tootrain eller kontrollera en ny maskininlärningsmodell på befintliga data. Om du behöver meddelandet kvarhållning utöver 7 dagar, så att [Event Hubs avbilda](https://docs.microsoft.com/azure/event-hubs/event-hubs-capture-overview) på din händelse hubb hämtar hello data från din event hub toohello Storage-konto eller ett Azure Data Lake-tjänstkontot du väljer. Aktivera avbilda ådrar sig en kostnad som baseras på dina köpta Genomflödesenhet.

### <a name="where-is-azure-event-hubs-available"></a>Där är Händelsehubbar i Azure?
Händelsehubbar i Azure är tillgänglig i alla regioner som stöds Azure. En lista över finns hello [Azure-regioner](https://azure.microsoft.com/regions/) sidan.  

## <a name="best-practices"></a>Bästa praxis

### <a name="how-many-partitions-do-i-need"></a>Hur många partitioner behöver jag?
Tänk på att hello antalet partitioner i en händelsehubb kan du inte ändras efter installationen. Det är viktigt toothink om hur många partitioner som du behöver för att komma igång med detta i åtanke. 

Händelsehubbar är utformad tooallow en enskild partition läsare per konsumentgrupp. Fyra partitioner hello standardinställningen är tillräcklig för de flesta användningsområden. Om du söker tooscale din händelsebearbetning kanske tooconsider att lägga till ytterligare partitioner. Det finns ingen gräns för specifika genomströmning på en partition, men hello sammanställda genomflöde i namnområdet begränsas av hello antalet genomflödesenheter. Om du ökar hello antalet genomflödesenheter i namnområdet kanske du vill ytterligare partitioner tooallow läsare tooachieve sina egna maximalt dataflöde.

Men om du har en modell där programmet har en viss partition tillhörighet tooa kanske öka hello antalet partitioner inte av någon förmån tooyou. Mer information om detta finns [tillgänglighet och konsekvens](event-hubs-availability-and-consistency.md).

## <a name="pricing"></a>Prissättning

### <a name="where-can-i-find-more-pricing-information"></a>Var hittar mer information om priser?
Fullständig information om priser för Händelsehubbar finns hello [Händelsehubbar prisinformationen](https://azure.microsoft.com/pricing/details/event-hubs/).

### <a name="is-there-a-charge-for-retaining-event-hubs-events-for-more-than-24-hours"></a>Finns det en avgift för att bevara Händelsehubbar händelser för mer än 24 timmar?
hello Event Hubs standardnivån tillåter meddelandet kvarhållning punkter som är längre än 24 timmar för 7 dagar. Om hello hello totala antalet lagrade händelser överskrider tillåten för hello lagring för hello antalet valda genomflödesenheter (84 GB per genomflödesenhet), hello storlek överskrider hello ersättning debiteras med hello publicerade Azure Blob storage hastighet. hello tillåten för lagring i varje genomflödesenhet omfattar alla lagringskostnader för kvarhållningsperioder 24 timmar (hello standard) även om hello genomflödesenhet används upp toohello maximala ingång justering.

### <a name="how-is-hello-event-hubs-storage-size-calculated-and-charged"></a>Hur hello lagringsstorleken för Händelsehubbar beräknas och debiteras?
hello totala storleken på alla lagrade händelser, inklusive alla interna kostnader för händelsen huvuden eller på disk lagring strukturer i alla händelsehubbar mäts i hela hello dag. Hello slutet av hello dag beräknas hello belastning lagringsstorlek. hello tillåten för dagliga lagring beräknas baserat på hello minsta antal enheter som valts under hello dag (varje genomflödesenhet ger en justering med 84 GB). Om hello sammanlagda storlek överskrider hello beräknas dagligen tillåten lagring, hello överdriven lagring faktureras med Azure Blob storage-priser (på hello **lokalt Redundant lagring** hastighet).

### <a name="how-are-event-hubs-ingress-events-calculated"></a>Hur beräknas Händelsehubbar ingångshändelser?
Varje händelse skickas tooan händelsehubb räknas som ett fakturerbar meddelande. En *ingång händelse* definieras som en enhet som är mindre än eller lika med too64 KB. Alla händelser som är mindre än eller lika storlek too64 KB anses toobe en fakturerbar händelse. Om hello händelsen är större än 64 KB, beräknas hello antalet händelser som fakturerbar enligt toohello händelsestorleken i multiplar av 64 KB. Till exempel en 8 KB händelse skickas toohello händelsehubb faktureras som en händelse, men meddelandet 96 KB skickas toohello händelsehubb faktureras som två händelser.

Händelser som används från en händelsehubb, samt hanteringsåtgärder och kontroll anrop, till exempel kontrollpunkter räknas inte som en fakturerbar ingångshändelser, men påförs in toohello genomströmning enhet justering.

### <a name="do-brokered-connection-charges-apply-tooevent-hubs"></a>Använd asynkrona anslutning avgifter tooEvent Hubs?
Anslutningen avgifter gäller endast när hello AMQP-protokollet används. Det finns inga avgifter för anslutning för att skicka händelser med hjälp av HTTP, oavsett hello antalet skickar datorer eller enheter. Om du planerar toouse AMQP (till exempel tooachieve effektivare händelse strömning eller tooenable dubbelriktad kommunikation i IoT kommando- och scenarier), finns hello [Händelsehubbar prisinformation](https://azure.microsoft.com/pricing/details/event-hubs/) för information om hur många anslutningar ingår i varje tjänstnivå.

### <a name="how-is-event-hubs-capture-billed"></a>Hur faktureras Event Hubs Capture?
Avbilda aktiveras när alla händelsehubb i hello namnområdet har hello Capture-alternativ är aktiverat. Event Hubs avbilda faktureras timvis per köpta Genomflödesenhet. Som hello Genomflödesenhet antal ökas eller minskas, visar Event Hubs avbilda fakturering ändringarna i steg om hela timme. Mer information om händelsen hubbar avbilda fakturering finns [Händelsehubbar prisinformation](https://azure.microsoft.com/pricing/details/event-hubs/).

### <a name="will-i-be-billed-for-hello-storage-account-i-select-for-event-hubs-capture"></a>Jag debiteras för det Markera för Event Hubs Capture hello storage-konto?
Avbilda använder ett lagringskonto som du anger när aktiverad på en händelsehubb. Eftersom det är ditt lagringskonto är ändringar för den här konfigurationen fakturerade tooyour Azure-prenumeration.

## <a name="quotas"></a>Kvoter

### <a name="are-there-any-quotas-associated-with-event-hubs"></a>Finns det kvoter som är associerade med Händelsehubbar?
En lista över alla Händelsehubbar kvoter finns [kvoter](event-hubs-quotas.md).

## <a name="troubleshooting"></a>Felsökning

### <a name="what-are-some-of-hello-exceptions-generated-by-event-hubs-and-their-suggested-actions"></a>Vilka är några av hello-undantag som genereras av Händelsehubbar och föreslagna åtgärder?
En lista över möjliga Händelsehubbar undantag, se [undantag översikt](event-hubs-messaging-exceptions.md).

### <a name="diagnostic-logs"></a>Diagnostikloggar
Händelsehubbar stöder två typer av [diagnostik loggar](event-hubs-diagnostic-logs.md) -avbildning felloggar och arbetsloggarna - som representeras i json och kan aktiveras via hello Azure-portalen.

### <a name="support-and-sla"></a>Support och servicenivåavtal
Teknisk support för Händelsehubbar är tillgänglig via hello [community-forumen](https://social.msdn.microsoft.com/forums/azure/home). Support för fakturering och prenumerationshantering ges utan kostnad.

toolearn mer om våra SLA finns hello [serviceavtal](https://azure.microsoft.com/support/legal/sla/) sidan.

## <a name="next-steps"></a>Nästa steg
Mer information om Händelsehubbar genom att besöka hello följande länkar:

* [Event Hubs-översikt](event-hubs-what-is-event-hubs.md)
* [Skapa en händelsehubb](event-hubs-create.md)
