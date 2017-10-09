---
title: aaaTroubleshoot Azure Stream Analytics med diagnostik loggar | Microsoft Docs
description: "Lär dig hur tooanalyze diagnostik loggar från Stream Analytics jobb i Microsoft Azure."
keywords: 
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: jeffstok
ms.openlocfilehash: 600fce73636dd137f8f3a137f1d77b59b4a88801
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-stream-analytics-by-using-diagnostics-logs"></a>Felsöka Azure Stream Analytics med hjälp av diagnostik-loggar

Ibland kan slutar en Azure Stream Analytics-jobbet oväntat bearbetning. Det är viktigt toobe kan tootroubleshoot detta kind för händelsen. hello händelse kan orsakas av ett oväntat frågeresultatet, av anslutningen toodevices eller av ett oväntat avbrott. hello diagnostik loggar i Stream Analytics kan hjälpa dig att identifiera hello orsaken till problem när de inträffar och minska tiden för återställning.

## <a name="log-types"></a>Log-typer

Stream Analytics finns två typer av loggar: 
* [Aktivitetsloggar](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) (alltid på). Aktivitetsloggar ge insikter om åtgärder som utförs på jobb.
* [Diagnostik loggar](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) (konfigureras). Diagnostik-loggarna ger bättre insikter om allt som händer med ett jobb. Diagnostik loggar start när hello jobb skapas och slutar när hello jobbet har tagits bort. De täcker händelser när hello jobbet uppdateras och när den körs.

> [!NOTE]
> Du kan använda tjänster som Azure Storage, Azure Event Hubs och Azure logganalys tooanalyze avvikande data. Du debiteras baserat på hello Prismodell för dessa tjänster.
>

## <a name="turn-on-diagnostics-logs"></a>Aktivera diagnostik loggar

Diagnostik loggar är **av** som standard. tooturn på diagnostik loggar slutföra de här stegen:

1.  Logga in toohello Azure-portalen och gå toohello strömning jobb-bladet. Under **övervakning**väljer **diagnostik loggar**.

    ![Bladet navigering toodiagnostics loggar](./media/stream-analytics-job-diagnostic-logs/image1.png)  

2.  Välj **aktivera diagnostiken**.

    ![Aktivera diagnostik loggar](./media/stream-analytics-job-diagnostic-logs/image2.png)

3.  På hello **diagnostikinställningarna** sidan för **Status**väljer **på**.

    ![Ändra status för diagnostik-loggar](./media/stream-analytics-job-diagnostic-logs/image3.png)

4.  Ställ in hello arkivering mål (lagringskonto händelsehubb, logganalys) som du vill. Markera hello kategorier loggar som du vill toocollect (körning, redigering). 

5.  Spara hello nya diagnostik-konfigurationen.

hello diagnostik configuration tar cirka 10 minuter tootake effekt. Efter det hello loggar start i hello konfigurerats arkivering mål (du kan se dem på hello **diagnostik loggar** sidan):

![Bladet navigering toodiagnostics loggar - arkivering mål](./media/stream-analytics-job-diagnostic-logs/image4.png)

Mer information om hur du konfigurerar diagnostik finns [samla in och använda diagnostikdata från resurserna i Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs).

## <a name="diagnostics-log-categories"></a>Diagnostik logga kategorier

För närvarande kan registrera vi två typer av loggar för diagnostik:

* **Redigera**. Registrerar loggar händelser som är relaterade toojob redigering operations: skapandet att lägga till och ta bort indata och utdata, lägga till och uppdatera hello frågan, starta och stoppa hello jobb.
* **Körningen**. Samlar in händelser som inträffar under jobbkörningen:
    * Anslutningsfel
    * Databearbetning fel, inklusive:
        * Händelser som inte överensstämmer toohello fråga definition (felaktig fälttyp och värden, fält saknas och så vidare)
        * Fel för utvärdering av uttryck
    * Andra händelser och fel

## <a name="diagnostics-logs-schema"></a>Diagnostik loggar schema

Alla loggar lagras i JSON-format. Varje post innehåller följande vanliga strängfält hello:

Namn | Beskrivning
------- | -------
time | Tidsstämpeln (i UTC) för hello-loggen.
resourceId | ID för hello-resurs som hello åtgärden ägde rum, i versaler. Den omfattar hello prenumerations-ID och hello resursgruppen hello jobbnamn. Till exempel   **/SUBSCRIPTIONS/6503D296-DAC1-4449-9B03-609A1F4A1C87/RESOURCEGROUPS/MY-RESOURCE-GROUP/PROVIDERS/MICROSOFT. MYSTREAMINGJOB-STREAMANALYTICS/STREAMINGJOBS**.
category | Logga kategori, antingen **körning** eller **redigering**.
operationName | Namnet på hello-åtgärd som loggas. Till exempel **skicka händelser: SQL-utdata skriva fel toomysqloutput**.
status | Status för hello-åtgärd. Till exempel **misslyckades** eller **lyckades**.
nivå | Loggningsnivån. Till exempel **fel**, **varning**, eller **informations**.
properties | Logga post-specifik information serialiserad som en JSON-sträng. Mer information finns i följande avsnitt hello.

### <a name="execution-log-properties-schema"></a>Schemat för körning av loggen egenskaper

Körningsloggar ha information om händelser som inträffade under jobbkörningen Stream Analytics. hello schemat för egenskaper varierar beroende på hello typen av händelse. Vi har för närvarande hello följande typer av körningsloggar:

### <a name="data-errors"></a>Datafel

Alla fel som uppstår när hello jobb behandlar data är i den här kategorin loggar. Dessa loggar oftast skapas under data läses, serialisering och skrivåtgärder. Dessa loggar inkludera inte anslutningsfel. Anslutningsfel behandlas som allmänna händelser.

Namn | Beskrivning
------- | -------
Källa | Namnet på hello jobbet indata eller utdata där hello-fel uppstod.
Meddelande | Meddelande som är associerade med hello-fel.
Typ | Typen av fel. Till exempel **DataConversionError**, **CsvParserError**, eller **ServiceBusPropertyColumnMissingError**.
Data | Innehåller data som är användbar tooaccurately hitta hello orsaken hello felet. Ämne tootruncation, beroende på storleken.

Beroende på hello **operationName** värde, datafel har hello följer schemat:
* **Serialisera händelser**. Serialisera händelser inträffar under händelsen läsåtgärder. De inträffar när hello data på hello indata inte uppfyller hello frågeschemat för något av följande skäl:
    * *Typmatchningsfel under händelsen (Tyskland) serialisera*: identifierar hello fält som orsakar hello-fel.
    * *Det går inte att läsa en händelse, ogiltig serialisering*: Visar information om hello plats i hello indata där hello-fel uppstod. Innehåller blobbnamnet på för blob-indata, förskjutning och ett exempel på hello data.
* **Skicka händelser**. Skicka händelser sker under skrivåtgärder. De identifierar hello strömning händelse som orsakat hello-fel.

### <a name="generic-events"></a>Allmänna händelser

Allmänna händelser omfatta allt annat.

Namn | Beskrivning
-------- | --------
Fel | (valfritt) Information om fel. Detta är vanligtvis undantagsinformation, om den är tillgänglig.
Meddelande| Loggmeddelande.
Typ | Typ av meddelande. Maps toointernal kategorisering av fel. Till exempel **JobValidationError** eller **BlobOutputAdapterInitializationFailure**.
Korrelations-ID | [GUID](https://en.wikipedia.org/wiki/Universally_unique_identifier) som unikt identifierar hello jobbkörningen. Alla loggposter för körning från hello tid hello jobbet startar tills hello jobbet slutar har hello samma **Korrelations-ID** värde.

## <a name="next-steps"></a>Nästa steg

* [Introduktion tooStream Analytics](stream-analytics-introduction.md)
* [Kom igång med Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skala Stream Analytics-jobb](stream-analytics-scale-jobs.md)
* [Språkreferens för Stream Analytics-fråga](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Strömma Analytics management REST API-referens](https://msdn.microsoft.com/library/azure/dn835031.aspx)
