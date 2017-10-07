---
title: "aaaAzure Service Fabric-övervakning och diagnostik översikt | Microsoft Docs"
description: "Läs mer om övervakning och diagnostik för Azure Service Fabric-kluster, program och tjänster."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/17/2017
ms.author: dekapur
ms.openlocfilehash: 1ef6419863b056b76d81e915ab78df4facae88bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitoring-and-diagnostics-for-azure-service-fabric"></a>Övervaknings- och diagnostikfunktionerna för Azure Service Fabric

Övervaknings- och diagnostikfunktionerna är kritiska toodeveloping, testa och distribuera program och tjänster i en miljö. Service Fabric-lösningar som fungerar bäst när du planerar och implementerar övervakning och diagnostik för att kontrollera program och tjänster fungerar som förväntat i en miljö för lokal utveckling eller i produktion.

Hej huvudsakliga målen med övervakning och diagnostik är att:
* Identifiera och diagnostisera problem med maskinvara och infrastruktur
* Identifiera problem med programvara och app, minska avbrottstiden för tjänsten
* Förstå resource förbrukning och hjälper dig att enheten operations beslut
* Optimera prestanda för program, tjänster och infrastruktur
* Generera affärsinsikter och identifiera förbättringsområden


Hej totala arbetsflödet för övervakning och diagnostik består av tre steg:

1. **Händelse genereras**: Detta innefattar händelser (loggar, spårningar, anpassade händelser) på hello-infrastruktur (kluster), plattform och program / service-nivå
2. **Händelsen aggregering**: genererade händelser måste toobe samlas in och sammanställs innan de kan visas
3. **Analysis**: händelser måste toobe visualiserade och är tillgänglig i vissa format, tooallow för analys och visa efter behov

Flera produkter är tillgängliga som omfattar följande tre områden, och du är ledigt toochoose olika tekniker för varje. Det är viktigt att som hello olika delar fungerar tillsammans toodeliver övervakningslösning en slutpunkt till slutpunkt för klustret toomake.

## <a name="event-generation"></a>Händelsegenerering

hello första steget i hello övervakning och diagnostik arbetsflöde är hello skapas och generering av händelser och loggar. Dessa händelser, loggar och spårningar genereras på två nivåer: hello plattformar lager (inklusive hello kluster, hello datorer eller Service Fabric-åtgärder) eller hello programnivå (alla instrumentation läggs till tooapps och tjänster distribueras toohello kluster). Händelser på dessa nivåer kan anpassas, även om Service Fabric innehåller vissa instrumentation som standard.

Läs mer om [plattform händelser](service-fabric-diagnostics-event-generation-infra.md) och [nivå programhändelser](service-fabric-diagnostics-event-generation-app.md) toounderstand vilken finns och hur tooadd ytterligare instrumentation.

När du fattar beslut om hello loggning provider som du vill att toouse måste toomake att loggarna som ska aggregeras och lagras på rätt sätt.

## <a name="event-aggregation"></a>Händelsen aggregering

För att samla in hello loggar och händelser som genereras av dina program och klustret, normalt bör du använda [Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md) (mer liknande tooagent-baserade Logginsamling) eller [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md)(pågående Logginsamling).

Samla in programloggar med hjälp av Azure-diagnostik tillägget är ett bra alternativ för Service Fabric-tjänster om hello uppsättning loggen källor och mål ändras inte ofta och det finns en enkel mappning mellan hello källor och sina mål. hello orsak sker för detta konfigurerar Azure Diagnostics på hello Resource Manager-lagret, så att betydande förändringar toohello konfigurationen kräver uppdatering/omdistribuera hello klustret. Dessutom är det bäst används Kontrollera loggarna är som finns på en lite mer permanent från där de kan nås av olika plattformar för analys. Det innebär att den hamnar är mindre effektivt för en pipeline än med ett alternativ som EventFlow.

Med hjälp av [EventFlow](https://github.com/Azure/diagnostics-eventflow) tillåter du toohave tjänster skickar sina loggar direkt tooan analys och visualisering plattform och/eller toostorage. Andra bibliotek (ILogger, Serilog osv.) kan användas för hello samma syfte, men EventFlow har hello fördelen med har utformats speciellt för pågående loggen samlingen och toosupport Service Fabric-tjänster. Detta brukar toohave flera potentiella fördelar:

* Enkel konfiguration och distribution
    * hello konfigurationen av diagnostikdata samlingen har bara en del av hello tjänstkonfigurationen. Det är enkelt tooalways behålla det ”synkroniserad” med hello resten av programmet hello
    * Programspecifika eller per service configuration kan enkelt uppnås
    * Konfigurera datamål via EventFlow är bara gäller att lägga till hello lämpligt NuGet-paket och ändra hello *eventFlowConfig.json* fil
* Flexibilitet
    * hello-program kan skicka hello data oavsett var den måste toogo, så länge det finns ett klientbiblioteket som stöder hello riktade Datalagringssystemet. Nya mål kan läggas till på önskat sätt
    * Komplexa capture, filtrering och Datasammanställning regler kan genomföras
* Åtkomst toointernal programdata och kontext
    * hello diagnostiska undersystemet körs i processen för hello-program/tjänst kan enkelt utöka hello spårningar med relevant information

En sak toonote är att de här två alternativen är inte ömsesidigt uteslutande och när det är möjligt tooget ungefär samma sätt med hjälp av en eller hello andra det kan även vara klokt du tooset in båda. I de flesta fall leda att kombinera en agent med pågående samling tooa mer tillförlitlig övervakning av arbetsflödet. hello Azure Diagnostics tillägg (agent) kan vara din valda sökväg för plattformen nivån loggar medan du kan använda EventFlow (pågående samling) för din programloggarna för nivån. När du har förstått vad som fungerar bäst för dig är det tid toothink om hur du vill att dina data toobe visas och analyseras.

## <a name="event-analysis"></a>Händelseanalys

Det finns flera bra plattformar som finns på marknaden hello när det gäller toohello analys och visualisering av data för övervakning och diagnostik. hello två som vi rekommenderar är [OMS](service-fabric-diagnostics-event-analysis-oms.md) och [Programinsikter](service-fabric-diagnostics-event-analysis-appinsights.md) på grund av tootheir bättre integrering med Service Fabric, men du bör också se ut till hello [elastisk Stack](https://www.elastic.co/products) (särskilt om du använder ett kluster i en frånkopplad miljö), [Splunk](https://www.splunk.com/), eller valfri plattform för dina inställningar.

hello huvudpunkter för någon plattform som du bör inkludera hur nöjd du är med hello användargränssnittet och hämtning av alternativ för hello möjlighet toovisualize data och skapa lättläst instrumentpaneler och hello ytterligare verktyg ger tooenhance din övervakning, till exempel automatiserade aviseringar.

Dessutom toohello plattform som du väljer när du ställer in ett Service Fabric-kluster som en Azure-resurs, du får också åtkomst tooAzure out box övervakning för datorer som kan vara användbar för specifika prestanda och övervakning av mått.

### <a name="azure-monitor"></a>Azure Monitor

Du kan använda [Azure-Monitor](../monitoring-and-diagnostics/monitoring-overview.md) toomonitor många av hello Azure-resurser som en Service Fabric-klustret har skapats. En uppsättning mätvärden för hello [virtuella datorns skaluppsättning](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesets) och enskilda [virtuella datorer](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesetsvirtualmachines) samlas in automatiskt och visas i hello Azure-portalen. tooview hello samlat in information i hello Azure-portalen väljer hello resursgruppen som innehåller hello Service Fabric-klustret. Sedan skaluppsättning väljer hello virtuella som du vill tooview. I hello **övervakning** väljer **mått** tooview ett diagram över hello värden.

![Azure portal-vy av Insamlad information för mått](media/service-fabric-diagnostics-overview/azure-monitoring-metrics.png)

toocustomize hello diagram, följer du anvisningarna hello i [mått i Microsoft Azure](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md). Du kan också skapa varningar baserat på dessa mått, enligt beskrivningen i [skapa aviseringar i Azure-Monitor för Azure-tjänster](../monitoring-and-diagnostics/insights-alerts-portal.md). Du kan skicka aviseringar tooa meddelandetjänsten med hjälp av web hook enligt beskrivningen i [konfigurera ett webbprogram hook på en Azure mått avisering](../monitoring-and-diagnostics/insights-webhooks-alerts.md). Azure-Monitor stöder bara en prenumeration. Om du behöver toomonitor flera prenumerationer, eller om du behöver ytterligare funktioner [logganalys](https://azure.microsoft.com/documentation/services/log-analytics/), del av Microsoft Operations Management Suite ger en heltäckande lösning för IT både för lokala och molnbaserade infrastruktur. Du kan vidarebefordra data från Azure-Monitor direkt tooLog Analytics, så att du kan se mått och loggfiler för hela miljön i en enda plats.

## <a name="next-steps"></a>Nästa steg

### <a name="watchdogs"></a>Watchdogs

En watchdog är en separat tjänst som kan titta på hälsa och läsa in över tjänster och rapporten hälsa för någonting i hello hälsa modellen hierarki. Detta kan hjälpa att förhindra fel som inte skulle kunna identifieras baserat på hello vy av en enskild tjänst. Watchdogs är också en bra toohost kod som utför vidtar åtgärder utan interaktion från användaren (t.ex, rensa loggfiler i lagring vid vissa intervall). Du hittar en implementering av exemplet watchdog [här](https://github.com/Azure-Samples/service-fabric-watchdog-service).

Komma igång med att förstå hur händelser och loggar hämta genereras vid hello [plattformsnivå](service-fabric-diagnostics-event-generation-infra.md) och hello [programnivå](service-fabric-diagnostics-event-generation-app.md).
