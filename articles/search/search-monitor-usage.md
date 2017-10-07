---
title: "aaaMonitor användning och statistik i en Azure Search-tjänst | Microsoft Docs"
description: "Spåra resursstorlek för användnings- och index för Azure Search värdbaserade moln search-tjänsten på Microsoft Azure."
services: search
documentationcenter: 
author: bernitorres
manager: jlembicz
editor: 
tags: azure-portal
ms.assetid: 122948de-d29a-426e-88b4-58cbcee4bc23
ms.service: search
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 05/01/2017
ms.author: betorres
ms.openlocfilehash: f38eabb5d04a410e11eaaff22157da8aba9e4845
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitoring-an-azure-search-service"></a>Övervaka en Azure Search-tjänst

Azure Search har olika resurser för att spåra användning och prestanda för search-tjänster. Den ger dig åtkomst till toometrics, loggar, indexstatistik och utökade övervakningsfunktioner på Power BI. Den här artikeln beskriver hur tooenable hello olika strategier för övervakning och hur toointerpret hello resulterande data.

## <a name="azure-search-metrics"></a>Azure Search-mått
Mått får du nära realtid insyn i din söktjänst och är tillgängliga för varje tjänst utan ytterligare inställningar. De kan du spåra hello prestandan för din tjänst för in too30 dagar.

Azure Search samlar in data för tre olika mått:

* Sök svarstid: tid hello söktjänsten behövs tooprocess sökresultat, samman per minut.
* Sök frågor per sekund (QPS): Sök antalet frågor som tagits emot per sekund, samman per minut.
* Begränsad sökning frågar procentandel: procentandelen av sökfrågor som har begränsats samman per minut.

![Skärmbild av QPS aktivitet][1]

### <a name="set-up-alerts"></a>Konfigurera aviseringar
Hello mått detaljsida, kan du konfigurera aviseringar tootrigger ett e-postmeddelande eller en automatisk åtgärd när en måttet överskrider ett tröskelvärde som du har definierat.

Mer information om mått dokumentationen hello fullständig på Azure-Monitor.  

## <a name="how-tootrack-resource-usage"></a>Hur tootrack Resursanvändning
Spåra hello tillväxt index och storlek kan hjälpa dig att justera kapacitet innan träffa hello övre gräns som du har skapat för din tjänst. Du kan göra detta på hello-portalen eller programmässigt med hello REST API.

### <a name="using-hello-portal"></a>Med hjälp av hello portal

toomonitor Resursanvändning, visa hello antal och statistik för tjänsten i hello [portal](https://portal.azure.com).

1. Logga in toohello [portal](https://portal.azure.com).
2. Öppna hello service instrumentpanelen i Azure Search-tjänsten. Paneler för hello-tjänsten finns på startsidan för hello eller du kan bläddra toohello tjänsten från Bläddra på hello JumpBar.

hello användning innehåller en mätare som anger vilken del av tillgängliga resurser används för närvarande. Mer information om per service gränser för index, dokument och lagring finns [gränser](search-limits-quotas-capacity.md).

  ![Ikonen användning][2]

> [!NOTE]
> hello skärmbilden ovan för kostnadsfria hello-tjänsten, som har högst en replik och partitionen varje och kan endast värden 3 index, 10 000 dokument eller 50 MB data, beroende på vilket som inträffar först. Tjänster som skapades på en Basic eller Standard nivå har mycket större tjänstbegränsningarna. Mer information om hur du väljer en nivå finns [väljer du en nivå eller SKU](search-sku-tier.md).
>
>

### <a name="using-hello-rest-api"></a>Med hjälp av hello REST API
Både hello Azure Search REST-API och hello .NET SDK ger Programmeringsåtkomst tooservice mått.  Om du använder [indexerare](https://msdn.microsoft.com/library/azure/dn946891.aspx) tooload ett index från Azure SQL Database eller Azure Cosmos DB en ytterligare API är tillgängliga tooget hello nummer som du behöver.

* [Få indexstatistik](/rest/api/searchservice/get-index-statistics)
* [Antal dokument](/rest/api/searchservice/count-documents)
* [Hämta Status för indexerare](/rest/api/searchservice/get-indexer-status)

## <a name="how-tooexport-logs-and-metrics"></a>Hur tooexport loggar och mått

Du kan exportera hello åtgärdsloggar för din tjänst och hello rådata för hello mätvärden som beskrivs i föregående avsnitt hello. Åtgärdsloggar att du vet hur hello tjänsten används och kan användas från Power BI när data kopieras tooa storage-konto. Azure search innehåller en övervakning Power BI-Innehållspaketet för detta ändamål.


### <a name="enabling-monitoring"></a>Aktivera övervakning
Öppna din Azure Search-tjänst i hello [Azure-portalen](http://portal.azure.com) under hello alternativet Aktivera övervakning.

Välj hello data du vill tooexport: loggar, mått eller båda. Du kan kopiera den tooa storage-konto, skickar det tooan händelsehubb eller exportera den tooLog Analytics.

![Hur tooenable övervakning i hello-portalen][3]

tooenable med PowerShell eller hello Azure CLI finns hello dokumentationen [här](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs#how-to-enable-collection-of-diagnostic-logs).

### <a name="logs-and-metrics-schemas"></a>Loggar och mått scheman
När hello data är kopierade tooa lagring konto, hello data formateras som JSON och dess plats i två behållare:

* insikter-loggar-operationlogs: för loggar med webbtrafik
* insikter-mått-pt1m: för mått

Det finns en blob, per timme per behållare.

Exempelsökväg till:`resourceId=/subscriptions/<subscriptionID>/resourcegroups/<resourceGroupName>/providers/microsoft.search/searchservices/<searchServiceName>/y=2015/m=12/d=25/h=01/m=00/name=PT1H.json`

#### <a name="log-schema"></a>Loggen schema
hello loggar blobbar innehålla dina trafikloggar för search-tjänsten.
Varje blobb har en rotobjektet kallas **poster** som innehåller en matris med objekt.
Varje blobb innehåller poster för alla hello-åtgärden som utfördes under hello samma timme.

| Namn | Typ | Exempel | Anteckningar |
| --- | --- | --- | --- |
| time |Datum och tid |”2015-12-07T00:00:43.6872559Z” |Tidsstämpel för hello åtgärden |
| resourceId |Sträng |”/ SUBSCRIPTIONS/11111111-1111-1111-1111-111111111111 /<br/>RESURSGRUPPER-STANDARD-PROVIDERS /<br/> MICROSOFT. SEARCHSERVICE-SÖKNINGEN/SEARCHSERVICES ” |Din ResourceId |
| operationName |Sträng |”Query.Search” |hello namn på hello åtgärd |
| operationVersion |Sträng |"2015-02-28" |hello api-version som används |
| category |Sträng |”OperationLogs” |konstant |
| resultType |Sträng |”Lyckades” |Möjliga värden: lyckats eller misslyckats |
| resultSignature |int |200 |Resultatkod för HTTP |
| durationMS |int |50 |Varaktighet för hello-åtgärden i millisekunder |
| properties |Objektet |Se följande tabell hello |Objekt som innehåller åtgärden-specifika data |

**Egenskaper för schemat**
| Namn | Typ | Exempel | Anteckningar |
| --- | --- | --- | --- |
| Beskrivning |Sträng |”Hämta /indexes('content')/docs” |hello åtgärden slutpunkt |
| Fråga |Sträng |”? Sök = AzureSearch & $count = true & api-version 2015-02-28 =” |frågeparametrar Hej |
| Dokument |int |42 |Antal bearbetade dokument |
| Indexnamn |Sträng |”testindex” |Namnet på hello index som är associerade med hello åtgärden |

#### <a name="metrics-schema"></a>Mått schema
| Namn | Typ | Exempel | Anteckningar |
| --- | --- | --- | --- |
| resourceId |Sträng |”/ SUBSCRIPTIONS/11111111-1111-1111-1111-111111111111 /<br/>RESURSGRUPPER-STANDARD-PROVIDERS /<br/>MICROSOFT. SEARCHSERVICE-SÖKNINGEN/SEARCHSERVICES ” |resurs-id |
| metricName |Sträng |”Latens” |hello namnet på hello mått |
| time |Datum och tid |”2015-12-07T00:00:43.6872559Z” |hello åtgärden tidsstämpel |
| Genomsnittlig |int |64 |hello medelvärdet för hello rådata prover i hello mått tidsintervall |
| minsta |int |37 |hello minimivärdet för hello rådata prover i hello mått tidsintervall |
| Maximalt |int |78 |hello maximivärdet för hello rådata prover i hello mått tidsintervall |
| Totalt |int |258 |hello totalvärdet för hello rådata prover i hello mått tidsintervall |
| Antal |int |4 |hello antalet rådata exempel används toogenerate hello mått |
| tidskorn |Sträng |”PT1M” |Hej tidskornet på hello mått i ISO 8601 |

Alla mätvärden rapporteras i minuts intervall. Varje måttet visas lägsta, högsta och genomsnittliga värden per minut.

Hej SearchQueriesPerSecond mått är minst hello lägsta värdet för sökfrågor per sekund som har registrerats under den minuten. hello gäller samma toohello maximivärdet. Genomsnittlig, är hello sammanställd över hello hela minut.
Tänk på hur det här scenariot under en minut: en andra hög belastning som hello maximala för SearchQueriesPerSecond, följt av 58 sekunders genomsnittlig belastning och slutligen en sekund med enbart en fråga som är hello minsta.

För ThrottledSearchQueriesPercentage, lägsta, högsta, genomsnittlig och Summa, alla har hello samma värde: hello procentandelen sökfrågor som har begränsats från hello Totalt antal sökfrågor under en minut.

## <a name="analyzing-your-data-with-power-bi"></a>Analysera dina data med Power BI

Vi rekommenderar att du använder [Power BI](https://powerbi.microsoft.com) tooexplore och visualisera dina data. Du kan enkelt ansluta tooyour Azure Storage-konto och snabbt börja analysera dina data.

Azure Search ger en [Power BI-Innehållspaketet](https://app.powerbi.com/getdata/services/azure-search) som gör att du toomonitor och förstå din sökning trafik med fördefinierade diagram och tabeller. Den innehåller en uppsättning Power BI-rapporter som automatiskt ansluter tooyour data och tillhandahålla visual insikter om din söktjänst. Mer information finns i hello [Innehållspaketet hjälpsidan](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-search/).

![Power BI-instrumentpanel för Azure Search][4]

## <a name="next-steps"></a>Nästa steg
Granska [skala repliker och partitioner](search-limits-quotas-capacity.md) vägledning om hur toobalance hello allokering av partitioner och repliker för en befintlig tjänst.

Besök [hantera din söktjänst i Microsoft Azure](search-manage.md) mer information om tjänsten administration eller [prestanda och optimering](search-performance-optimization.md) för justering vägledning.

Mer information om hur du skapar häpnadsväckande rapporter. Se [komma igång med Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/) information

<!--Image references-->
[1]: ./media/search-monitor-usage/AzSearch-Monitor-BarChart.PNG
[2]: ./media/search-monitor-usage/AzureSearch-Monitor1.PNG
[3]: ./media/search-monitor-usage/AzureSearch-Enable-Monitoring.PNG
[4]: ./media/search-monitor-usage/AzureSearch-PowerBI-Dashboard.png
