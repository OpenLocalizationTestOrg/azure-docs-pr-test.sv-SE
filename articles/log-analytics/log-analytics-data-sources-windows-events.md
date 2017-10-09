---
title: "aaaCollect och analysera händelseloggarna för Windows i OMS Log Analytics | Microsoft Docs"
description: "Windows-händelseloggar är en av hello vanligaste datakällor som används av logganalys.  Den här artikeln beskriver hur tooconfigure insamling av Windows-händelseloggar och information om hello poster skapas i hello OMS-databasen."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: ee52f564-995b-450f-a6ba-0d7b1dac3f32
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/15/2017
ms.author: bwren
ms.openlocfilehash: c05648af39258443f22fd11e1d751b5ccec8c391
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="windows-event-log-data-sources-in-log-analytics"></a>Windows-händelseloggen datakällor i logganalys
Windows-händelseloggar är en av de vanligaste hello [datakällor](log-analytics-data-sources.md) för att samla in data med hjälp av Windows-agenter eftersom många program skriva toohello Windows-händelseloggen.  Du kan samla in händelser från standard loggar, till exempel System och program i tillägg toospecifying på alla anpassade loggar som skapats av program som du behöver toomonitor.

![Windows-händelser](media/log-analytics-data-sources-windows-events/overview.png)     

## <a name="configuring-windows-event-logs"></a>Konfigurera Windows-händelse loggar
Konfigurera Windows-händelseloggar från hello [Data-menyn i logganalys-inställningar](log-analytics-data-sources.md#configuring-data-sources).

Logganalys endast samlar in händelser från händelseloggar för hello Windows som anges i inställningarna för hello.  Du kan lägga till en händelselogg genom att skriva in hello namnet på hello logg och klicka på  **+** .  Endast hello händelser med hello valt allvarlighetsgraderna samlas in för varje logg.  Kontrollera hello allvarlighetsgraderna för hello speciell logg som du vill toocollect.  Du kan ange ytterligare kriterier toofilter händelser.

När du skriver hello namnet på en händelselogg innehåller logganalys förslag på Händelseloggnamn. Hello-logg som du vill använda tooadd inte visas i hello listan, kan du fortfarande lägga till den genom att skriva in hello hello loggen fullständiga namn. Du hittar hello fullständiga namn hello loggen med hjälp av Loggboken. Öppna hello i Loggboken, *egenskaper* för sträng för hello-loggen och kopiera hello från hello *fullständiga namn* fältet.

![Konfigurera Windows-händelser](media/log-analytics-data-sources-windows-events/configure.png)

## <a name="data-collection"></a>Datainsamling
Logganalys samlar in varje händelse som matchar en vald allvarlighetsgrad från en övervakade händelseloggen som hello händelse skapas.  hello agent registrerar dess plats i varje händelseloggen som samlas in från.  Om hello agenten tas offline under en tidsperiod, sedan logganalys samlar in händelser från där den senast slutade, även om de händelserna som skapades när hello-agenten var offline.  Finns det risk för dessa händelser toonot samlas in om hello händelseloggen radbryts med oinsamlat händelser att skrivas över när hello agent är offline.

>[!NOTE]
>Logganalys samlar inte in granskningshändelser som skapats av SQL Server från källan *MSSQLSERVER* med händelse-ID 18453 som innehåller nyckelord - *klassiska* eller *granska lyckade* och nyckelordet *0xa0000000000000*.
>

## <a name="windows-event-records-properties"></a>Windows-händelse registrerar egenskaper
Windows händelseposter har en typ av **händelse** och ha hello egenskaper i den följande tabellen hello:

| Egenskap | Beskrivning |
|:--- |:--- |
| Dator |Namnet på hello-dator som hello händelse som samlats in från. |
| EventCategory |Kategori hello-händelse. |
| EventData |Alla händelsedata i raw-format. |
| Händelse-ID |Antal hello-händelse. |
| EventLevel |Allvarlighetsgrad för händelsen hello i numeriska formatet. |
| EventLevelName |Allvarlighetsgrad för händelsen hello i textformat. |
| Händelseloggen |Namnet på hello-händelseloggen som hello händelse som samlats in från. |
| ParameterXml |Händelsen parametervärden i XML-format. |
| ManagementGroupName |Namnet på hanteringsgruppen för hello för System Center Operations Manager-agenter.  Det här värdet är för andra agenter AOI-<workspace ID> |
| RenderedDescription |Händelsebeskrivning med parametervärden |
| Källa |Källan för hello-händelse. |
| SourceSystem |Typ av agenten hello händelse samlats in från. <br> Anslut OpsManager – Windows-agenten, antingen direkt eller hanteras av Operations Manager <br> Linux – alla Linux-agenter  <br> AzureStorage – Azure-diagnostik |
| TimeGenerated |Datum och tid hello händelsen skapades i Windows. |
| Användarnamn |Användarnamnet för hello-konto som loggade hello. |

## <a name="log-searches-with-windows-events"></a>Loggen sökningar med Windows-händelser
hello innehåller följande tabell olika exempel på loggen sökningar som hämtar poster för Windows-händelse.

| Fråga | Beskrivning |
|:--- |:--- |
| Typ = händelse |Alla Windows-händelser. |
| Typ = händelse EventLevelName = fel |Alla Windows-händelser med allvarlighetsgraden fel. |
| Typ = händelse &#124; Måttet count() av källa |Antal Windows-händelser efter källa. |
| Typ = händelse EventLevelName = fel &#124; Måttet count() av källa |Antal Windows felhändelser efter källa. |


>[!NOTE]
> Om ditt arbetsområde har uppgraderade toohello [nya Log Analytics-frågespråket](log-analytics-log-search-upgrade.md), hello senare frågorna skulle ändra toohello följande.
>
>| Fråga | Beskrivning |
|:---|:---|
| Händelse |Alla Windows-händelser. |
| Händelsen &#124; där EventLevelName == ”error” |Alla Windows-händelser med allvarlighetsgraden fel. |
| Händelsen &#124; Sammanfatta count() av källa |Antal Windows-händelser efter källa. |
| Händelsen &#124; där EventLevelName == ”error” &#124; Sammanfatta count() av källa |Antal Windows felhändelser efter källa. |


## <a name="next-steps"></a>Nästa steg
* Konfigurera logganalys toocollect andra [datakällor](log-analytics-data-sources.md) för analys.
* Lär dig mer om [logga sökningar](log-analytics-log-searches.md) tooanalyze hello data som samlas in från datakällor och lösningar.  
* Använd [anpassade fält](log-analytics-custom-fields.md) tooparse hello händelseposter till enskilda fält.
* Konfigurera [insamling av prestandaräknare](log-analytics-data-sources-performance-counters.md) från Windows-agenter.
