---
title: aaaUnderstanding aviseringar i Azure Log Analytics | Microsoft Docs
description: "Aviseringar i Log Analytics identifiera viktig information i OMS-databasen och kan proaktivt meddelar dig om problem eller anropa åtgärder tooattempt toocorrect dem.  Den här artikeln beskriver hello olika typer av Varningsregler och hur de definieras."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 6cfd2a46-b6a2-4f79-a67b-08ce488f9a91
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: bwren
ms.openlocfilehash: bfa0a5aaeca81674e79a6d647f36d937efeeb439
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="understanding-alerts-in-log-analytics"></a>Förstå aviseringar i logganalys

Aviseringar i Log Analytics identifiera viktig information i logganalys-databasen.  Den här artikeln innehåller information om hur Varningsregler i logganalys arbete och beskriver hello skillnaderna mellan olika typer av Varningsregler.

Hello processen att skapa Varningsregler, finns i hello följande artiklar:

- Skapa Varningsregler med [Azure-portalen](log-analytics-alerts-creating.md)
- Skapa Varningsregler med [Resource Manager-mall](../operations-management-suite/operations-management-suite-solutions-resources-searches-alerts.md)
- Skapa Varningsregler med [REST API](log-analytics-api-alerts.md)


## <a name="alert-rules"></a>Aviseringsregler

Aviseringar skapas med Varningsregler som automatiskt kör loggen söker regelbundet.  Om hello resultaten av hello loggen matchar särskilda skapas en avisering post.  hello regeln kan sedan automatiskt köra en eller flera åtgärder tooproactively meddelar dig om hello aviseringen eller anropa en annan process.  Olika typer av Varningsregler använda olika logik tooperform denna analys.

![Log Analytics-aviseringar](media/log-analytics-alerts/overview.png)

Varningsregler definieras av hello följande information:

- **Sök i loggfilen**.  hello-fråga som körs varje gång hello varningsregeln utlöses.  hello-poster som returneras av den här frågan är används toodetermine om en avisering skapas.
- **Tidsfönstret**.  Anger hello tidsintervall för hello frågan.  hello frågan returnerar bara de poster som har skapats i det här intervallet av hello aktuell tid.  Detta kan vara ett värde mellan 5 minuter och 24 timmar. Om hello fönstret angetts too60 minuter och hello frågan körs klockan 13:15, returneras endast de poster som skapats mellan 12:15:00 och 1:15 i Återställningsmappen.
- **Frekvensen**.  Anger hur ofta hello frågan ska köras. Kan vara ett värde mellan 5 minuter och 24 timmar. Ska vara lika tooor som är mindre än hello tidsperioden.  Om hello-värdet är större än hello tidsfönstret riskerar du poster som saknas.<br>Tänk dig ett tidsfönster på 30 minuter och en frekvens som 60 minuter.  Om hello frågan körs 1:00, returnerar poster mellan 12:30 och 1:00.  hello är hello frågan skulle köras just nu 2:00 när återgår den poster mellan 1:30 och 2:00.  Alla poster som skapats mellan 01:00 och 1:30 skulle aldrig utvärderas.
- **Tröskelvärde för**.  hello resultaten av hello loggen är utvärderade toodetermine om en avisering ska skapas.  hello tröskelvärdet är olika för hello olika typer av Varningsregler.

Varje avisering regel i Log Analytics är en av två typer.  De olika typerna beskrivs i detalj i hello avsnitten som följer.

- **[Antalet resultat](#number-of-results-alert-rules)**. Avisering skapas när hello antalet poster som returneras av hello loggen sökningen överskrider ett angivet tal.
- **[Mått mätning](#metric-measurement-alert-rules)**.  Avisering som skapas för varje objekt i hello resultaten av hello logg med värden som överskrider angiven tröskel.

hello skillnaderna mellan varningsregeln typer är som följer.

- **Antalet resultat** varningsregeln skapas alltid en enda avisering stund **mått mätning** varningsregeln skapar en avisering för varje objekt som överskrider tröskeln hello.
- **Antalet resultat** Varningsregler skapar en avisering när hello tröskelvärdet överskrids en gång. **Mått mätning** Varningsregler kan skapa en avisering när hello tröskelvärde har överskridits ett visst antal gånger under ett visst tidsintervall.

## <a name="number-of-results-alert-rules"></a>Antalet resultat Varningsregler
**Antalet resultat** Varningsregler skapar en avisering när hello antalet poster som returneras av hello sökfråga överstiga hello angivet tröskelvärde.

### <a name="threshold"></a>Tröskelvärde
hello tröskelvärdet för en **antalet resultat** varningsregeln är helt enkelt större eller mindre än ett visst värde.  En avisering skapas om hello antalet poster som returneras av hello loggen sökningen matchar det här villkoret.

### <a name="scenarios"></a>Scenarier

#### <a name="events"></a>Händelser
Den här typen av regel för varning är idealisk för att arbeta med händelser, t.ex Windows-händelseloggar Syslog, och anpassade loggar.  Du kanske vill toocreate en avisering när en viss felhändelse skapas eller när flera felhändelser skapas inom ett visst tidsintervall.

tooalert på en enskild händelse hello antal resultat toogreater än 0 och hello frekvens och tid fönstret too5 minuter.  Som kör hello frågan var 5 minuter och Sök efter hello förekomsten av en enskild händelse som har skapats sedan hello senaste gången hello frågan kördes.  En längre frekvens kan fördröja hello tiden mellan hello händelse som samlas in och hello avisering skapas.

Vissa program får logga in ett tillfälligt fel som inte nödvändigtvis rera en avisering.  Hello program kan till exempel försök hello-processen som skapade hello felhändelse och lyckas hello nästa gång.  I så fall måste kanske du inte vill toocreate hello avisering om flera händelser skapas inom ett visst tidsintervall.  

I vissa fall kan kanske du vill toocreate en avisering i hello frånvaron av en händelse.  En process kan till exempel logga regelbundna händelser tooindicate att den fungerar korrekt.  Om det inte logga en av dessa händelser inom ett visst tidsintervall, ska en avisering skapas.  I det här fallet kan du ange hello tröskelvärde för**mindre än 1**.

#### <a name="performance-alerts"></a>Prestandavarningar
[Prestandadata](log-analytics-data-sources-performance-counters.md) lagras som poster i hello OMS databasen liknande tooevents.  Om du vill tooalert när en prestandaräknare överskrider ett visst tröskelvärde ska gränsen tas med i hello-frågan.

Till exempel om du vill använda tooalert när hello processor så körs 90% som du vill använda en fråga som hello följa med hello tröskelvärdet för hello varningsregeln **större än 0**.

    Type=Perf ObjectName=Processor CounterName="% Processor Time" CounterValue>90

Om du vill ha tooalert när hello processor var i genomsnitt över 90% för ett visst tidsintervall, använder du en fråga med hello [mäta kommandot](log-analytics-search-reference.md#commands) som hello följande med hello tröskelvärdet för hello varningsregeln **större än 0** .

    Type=Perf ObjectName=Processor CounterName="% Processor Time" | measure avg(CounterValue) by Computer | where AggregatedValue>90

>[!NOTE]
> Om ditt arbetsområde har uppgraderade toohello [nya Log Analytics-frågespråket](log-analytics-log-search-upgrade.md), hello senare frågorna skulle ändra toohello följande:`Perf | where ObjectName=="Processor" and CounterName=="% Processor Time" and CounterValue>90`
> `Perf | where ObjectName=="Processor" and CounterName=="% Processor Time" | summarize avg(CounterValue) by Computer | where CounterValue>90`


## <a name="metric-measurement-alert-rules"></a>Mått mätning Varningsregler

>[!NOTE]
> Mått mätning Varningsregler är för närvarande i förhandsversion.

**Mått mätning** Varningsregler skapar en avisering för varje objekt i en fråga med ett värde som överskrider ett angivet tröskelvärde.  De har hello efter specifika skillnader från **antalet resultat** Varna regler.

#### <a name="log-search"></a>Loggsökning
Du kan använda en fråga för en **antalet resultat** avisering regel, det finns särskilda krav hello frågan för ett mått mätning varningsregel.  Det måste innehålla en [mäta kommandot](log-analytics-search-reference.md#commands) toogroup hello resultat på ett visst fält. Det här kommandot måste innehålla hello följande element.

- **Mängdfunktion**.  Anger hello beräkning som ska utföras och kan vara ett numeriskt fält tooaggregate.  Till exempel **count()** returnerar hello antalet poster i hello frågan **avg(CounterValue)** returnerar hello medelvärdet av hello CounterValue fältet under hello period.
- **Gruppera fältet**.  En post med ett insamlat värde skapas för varje instans av det här fältet och en avisering genereras för varje.  Till exempel om du vill toogenerate en avisering för varje dator kan du skulle använda **per dator**.   
- **Intervallet**.  Definierar hello tidsintervall under vilken hello informationen sammanställs.  Till exempel om du har angett **5minutes**, skapas en post för varje instans av hello gruppfältet samman på 5 minuters intervall under hello tidsfönster som angetts för hello avisering.

#### <a name="threshold"></a>Tröskelvärde
hello tröskelvärdet för mått mätning Varningsregler definieras av ett samlat värde och ett antal intrång.  Om varje datapunkt i hello loggen Sök överskrider detta värde, anses det har ett intrång.  Om hello antal intrång i för alla objekt i hello resultat överskrider hello anges värdet, skapas en avisering för objektet.

#### <a name="example"></a>Exempel
Föreställ dig ett scenario där du vill lägga till en avisering om alla datorer överskrids processoranvändning 90% tre gånger under 30 minuter.  Du skapar en aviseringsregel med hello följande information.  

**Fråga:** typ = Perf ObjectName = Processor CounterName = ”% processortid” | mäta avg(CounterValue) datorn intervall 5 minut<br>
**Tidsfönstret:** 30 minuter<br>
**Varna frekvens:** 5 minuter<br>
**Aggregera värde:** bra än 90<br>
**Utlösaren avisering baserat på:** totalt överträdelser som är större än 5<br>

hello-frågan skulle skapa ett genomsnittligt värde för varje dator vid 5 minuters mellanrum.  Den här frågan skulle köras var femte minut för data som samlas in via hello tidigare 30 minuter.  Exempeldata visas nedan för tre datorer.

![Exempel frågeresultat](media/log-analytics-alerts/metrics-measurement-sample-graph.png)

I det här exemplet skulle separata aviseringar skapas för srv02 och srv03 eftersom de utsatts för intrång hello 90% tröskelvärdet 3 gånger över hello tidsperioden.  Om hello **utlösaren avisering baserat på:** har ändrats för**sidordning** sedan en avisering skapas endast för srv03 eftersom den utsatts för intrång hello tröskelvärdet för 3 beräkningar i följd.

## <a name="alert-records"></a>Varning-poster
Aviseringen poster som skapats av Varningsregler i logganalys har en **typen** av **avisering** och en **SourceSystem** av **OMS**.  De har hello egenskaper i hello i den följande tabellen.

| Egenskap | Beskrivning |
|:--- |:--- |
| Typ |*Varning* |
| SourceSystem |*OMS* |
| *Objektet*  | [Aviseringar för mått mätning](#metric-measurement-alert-rules) har en egenskap för hello gruppfältet.  Till exempel om hello loggen Sök grupper på datorn, hello avisering post med har en dator fält med hello namnet på datorn som hello som hello-värde.
| AlertName |Namnet på hello avisering. |
| AlertSeverity |Allvarlighetsgrad för hello avisering. |
| LinkToSearchResults |Länka tooLog Analytics loggen sökning som returnerar hello poster från hello-fråga som skapade hello avisering. |
| Fråga |Texten för hello-fråga som kördes. |
| QueryExecutionEndTime |Slutet på hello tidsintervall för hello frågan. |
| QueryExecutionStartTime |Start på hello tidsintervall för hello frågan. |
| ThresholdOperator | Operator som användes av hello varningsregel. |
| ThresholdValue | Värdet som användes av hello varningsregel. |
| TimeGenerated |Datum och tid hello aviseringen skapades. |

Det finns andra typer av aviseringar poster som skapats av hello [lösning för avisering](log-analytics-solution-alert-management.md) och av [Power BI exporterar](log-analytics-powerbi.md).  Dessa alla har en **typen** av **avisering** men särskiljs från varandra med deras **SourceSystem**.


## <a name="next-steps"></a>Nästa steg
* Installera hello [avisering hanteringslösning](log-analytics-solution-alert-management.md) tooanalyze aviseringar som skapats i logganalys tillsammans med aviseringar som samlas in från System Center Operations Manager.
* Läs mer om [logga sökningar](log-analytics-log-searches.md) som kan generera aviseringar.
* Slutföra en genomgång för [konfigurerar en webook](log-analytics-alerts-webhooks.md) med en aviseringsregel.  
* Lär dig hur toowrite [runbooks i Azure Automation](https://azure.microsoft.com/documentation/services/automation) tooremediate problem som identifieras av aviseringar.
