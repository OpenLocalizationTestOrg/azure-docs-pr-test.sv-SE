---
title: "Azure övervakaren mått - stöds mått per resurstypen | Microsoft Docs"
description: "Lista över tillgängliga för varje resurstyp av med Azure-Monitor mått."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 63d4ac65-1688-40d1-85c8-7cd408285b0f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/05/2017
ms.author: johnkem
ms.openlocfilehash: 4cd72c8193d66f164d9afa53af4b5203369b32dc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="supported-metrics-with-azure-monitor"></a>Stöds mått med Azure-Monitor
Azure-Monitor finns flera sätt att interagera med statistik, inklusive diagram dem i portalen, komma åt dem via REST API eller fråga dem med PowerShell eller CLI. Nedan finns en fullständig lista över alla just nu med Azure-Monitor mått pipeline.

> [!NOTE]
> Andra mått kan finnas i portalen eller med äldre API: er. Listan innehåller endast tillgängliga med förhandsversion av konsoliderade Azure-Monitor mått pipeline förhandsversion mått.
>
>

## <a name="microsoftanalysisservicesservers"></a>Microsoft.AnalysisServices/servers

|Mått|Mått visningsnamn|Enhet|Sammansättningstyp|Beskrivning|
|---|---|---|---|---|
|qpu_metric|QPU|Antal|Genomsnittlig|QPU. Intervallet 0-100 för S1, 0-200 för S2 och 0-400 för S4|
|memory_metric|Minne|Byte|Genomsnittlig|Minne. Intervallet 0-25 GB för S1, 0-50 GB för S2 och 0-100 GB för S4|
|TotalConnectionRequests|Totalt antal begäranden|Antal|Genomsnittlig|Totalt antal förfrågningar. Dessa är mottagna.|
|SuccessfullConnectionsPerSec|Lyckade anslutningar Per sekund|CountPerSecond|Genomsnittlig|Hastighet för lyckade anslutningen slutföras.|
|TotalConnectionFailures|Totalt antal anslutningsfel|Antal|Genomsnittlig|Totalt antal misslyckade anslutningsförsök.|
|CurrentUserSessions|Aktuella användarsessioner|Antal|Genomsnittlig|Aktuellt antal användarsessioner som har upprättats.|
|QueryPoolBusyThreads|Frågan poolen upptagen trådar|Antal|Genomsnittlig|Antal upptagen trådar i trådpoolen för frågan.|
|CommandPoolJobQueueLength|Kölängd för kommandot poolen jobb|Antal|Genomsnittlig|Antal jobb i kö i trådpoolen för kommandot.|
|ProcessingPoolJobQueueLength|Kölängd för bearbetning poolen jobb|Antal|Genomsnittlig|Antal icke-I/O-jobb i kö för bearbetningstrådpoolen.|
|CurrentConnections|Anslutning: Aktuella anslutningar|Antal|Genomsnittlig|Aktuellt antal klientanslutningar.|
|CleanerCurrentPrice|Minne: Tydligare aktuella pris|Antal|Genomsnittlig|Aktuella priset minne, $ byte/tidsvärdet, normaliserad till 1000.|
|CleanerMemoryShrinkable|Minne: Tydligare minne krympning|Byte|Genomsnittlig|Mängden minne i byte, föremål rensning av bakgrunden ett rengöringsband.|
|CleanerMemoryNonshrinkable|Minne: Tydligare minne nonshrinkable|Byte|Genomsnittlig|Mängden minne i byte som inte omfattas av rensning av bakgrunden ett rengöringsband.|
|MemoryUsage|Minne: Minnesanvändning|Byte|Genomsnittlig|Mängden minne på server-processen som används för att beräkna tydligare minnespris. Lika med prestandaräknaren Process\PrivateBytes plus storleken på minnesmappade data, ignoreras eventuella som mappats eller som har allokerats av xVelocity i minnet analytics motorn (VertiPaq) överskrider minnesgränsen för xVelocity-motorn.|
|MemoryLimitHard|Minne: Minnesgränsen hårddisken|Byte|Genomsnittlig|Hårda minnesgränsen, från konfigurationsfilen.|
|MemoryLimitHigh|Minne: Minne begränsa hög|Byte|Genomsnittlig|Hög minnesgränsen, från konfigurationsfilen.|
|MemoryLimitLow|Minne: Minne gränsen låg|Byte|Genomsnittlig|Minnesbrist gräns från konfigurationsfilen.|
|MemoryLimitVertiPaq|Minne: Minne gränsen VertiPaq|Byte|Genomsnittlig|I minnet gräns från konfigurationsfilen.|
|Kvot|Minne: kvot|Byte|Genomsnittlig|Aktuella minneskvoten, i byte. Minneskvoten kallas även en reservation för minne grant eller minne.|
|QuotaBlocked|: Minneskvoten blockeras|Antal|Genomsnittlig|Aktuellt antal begäranden om kvoten som blockeras tills andra minneskvoter frigörs.|
|VertiPaqNonpaged|Minne: VertiPaq växlingsbart systemminne|Byte|Genomsnittlig|Byte av minne låst i arbetsminnet för användning av motorn i minnet.|
|VertiPaqPaged|Minne: VertiPaq-växlat minne|Byte|Genomsnittlig|Antal byte växlingsbart minne som används för data i minnet.|
|RowsReadPerSec|Bearbetning: Läsa rader per sek|CountPerSecond|Genomsnittlig|Antal rader att läsa från alla relationsdatabaser.|
|RowsConvertedPerSec|Bearbetning: Rader konverteras per sek|CountPerSecond|Genomsnittlig|Antal rader konverteras under bearbetning.|
|RowsWrittenPerSec|Bearbetning: Rader som skrivs per sekund|CountPerSecond|Genomsnittlig|Antal rader skrivna under bearbetning.|
|CommandPoolBusyThreads|Trådar: Kommandot poolen upptagen trådar|Antal|Genomsnittlig|Antal upptagen trådar i trådpoolen för kommandot.|
|CommandPoolIdleThreads|Trådar: Kommandot poolen inaktiva trådar|Antal|Genomsnittlig|Antalet inaktiva trådar i trådpoolen för kommandot.|
|LongParsingBusyThreads|Trådar: Long parsning upptagen trådar|Antal|Genomsnittlig|Antal upptagen trådar i länge tolkning trådpoolen.|
|LongParsingIdleThreads|Trådar: Long parsning inaktiva trådar|Antal|Genomsnittlig|Antalet inaktiva trådar i länge tolkning trådpoolen.|
|LongParsingJobQueueLength|Trådar: Parsning länge jobbet Kölängd|Antal|Genomsnittlig|Antal jobb i kö för länge tolkning trådpoolen.|
|ProcessingPoolBusyIOJobThreads|Trådar: Bearbetning av poolen upptagen i/o-jobbet trådar|Antal|Genomsnittlig|Antal trådar som körs i/o-jobb i bearbetningstrådpoolen.|
|ProcessingPoolBusyNonIOThreads|Trådar: Bearbetning av poolen upptagen icke-I/O-trådar|Antal|Genomsnittlig|Antal trådar som icke-I/O-jobb som körs i bearbetningstrådpoolen.|
|ProcessingPoolIOJobQueueLength|Trådar: Bearbetning av poolen Kölängd för i/o-jobb|Antal|Genomsnittlig|Antal i/o-jobb i kö för bearbetningstrådpoolen.|
|ProcessingPoolIdleIOJobThreads|Trådar: Bearbetning av poolen inaktiv i/o-jobbet trådar|Antal|Genomsnittlig|Antal inaktiva trådar för i/o-jobb i bearbetningstrådpoolen.|
|ProcessingPoolIdleNonIOThreads|Trådar: Bearbetning av poolen inaktiv icke-I/O-trådar|Antal|Genomsnittlig|Antalet inaktiva trådar i bearbetningstrådpoolen som är dedikerad till icke-I/O-jobb.|
|QueryPoolIdleThreads|Trådar: Frågan poolen inaktiva trådar|Antal|Genomsnittlig|Antal inaktiva trådar för i/o-jobb i bearbetningstrådpoolen.|
|QueryPoolJobQueueLength|Trådar: Frågan poolen jobbet kön lengt|Antal|Genomsnittlig|Antal jobb i kö i trådpoolen för frågan.|
|ShortParsingBusyThreads|Trådar: Kort parsning upptagen trådar|Antal|Genomsnittlig|Antal upptagen trådar i kort tolkning trådpoolen.|
|ShortParsingIdleThreads|Trådar: Kort parsning inaktiva trådar|Antal|Genomsnittlig|Antalet inaktiva trådar i kort tolkning trådpoolen.|
|ShortParsingJobQueueLength|Trådar: Kort parsning jobbet Kölängd|Antal|Genomsnittlig|Antal jobb i kö för kort tolkning trådpoolen.|
|memory_thrashing_metric|Minne knattrar|Procent|Genomsnittlig|Genomsnittlig minne knattrar.|

## <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service

|Mått|Mått visningsnamn|Enhet|Sammansättningstyp|Beskrivning|
|---|---|---|---|---|
|TotalRequests|Totalt antal Gateway-begäranden|Antal|Totalt|Antal begäranden för gateway|
|SuccessfulRequests|Rätt Gateway-begäranden|Antal|Totalt|Antalet begäranden som lyckats gateway|
|UnauthorizedRequests|Obehörig Gateway-begäranden|Antal|Totalt|Antal begäranden för obehörig gateway|
|FailedRequests|Gateway för misslyckade begäranden|Antal|Totalt|Antalet fel i gateway-begäranden|
|OtherRequests|Andra Gateway-begäranden|Antal|Totalt|Antal andra gateway-begäranden|

## <a name="microsoftbatchbatchaccounts"></a>Microsoft.Batch/batchAccounts

|Mått|Mått visningsnamn|Enhet|Sammansättningstyp|Beskrivning|
|---|---|---|---|---|
|CoreCount|Antal dedikerade kärnor|Antal|Totalt|Totalt antal dedikerade kärnor i batch-kontot|
|TotalNodeCount|Antalet dedicerade noder|Antal|Totalt|Totalt antal dedikerade noder i batch-kontot|
|LowPriorityCoreCount|Antal för LowPriority kärnor|Antal|Totalt|Totalt antal kärnor låg prioritet i batch-kontot|
|TotalLowPriorityNodeCount|Antalet noder med låg prioritet|Antal|Totalt|Totalt antal noder låg prioritet i batch-kontot|
|CreatingNodeCount|Skapa antalet noder|Antal|Totalt|Antalet noder som skapas|
|StartingNodeCount|Starta antalet noder|Antal|Totalt|Antalet noder som startar|
|WaitingForStartTaskNodeCount|Väntar på för starta uppgiften antalet noder|Antal|Totalt|Antalet noder som väntar på att starta aktiviteten för att slutföra|
|StartTaskFailedNodeCount|Starta kunde inte antalet noder|Antal|Totalt|Antalet noder där aktiviteten starta misslyckades|
|IdleNodeCount|Antalet inaktiva noder|Antal|Totalt|Antalet inaktiva noder|
|OfflineNodeCount|Antalet offline noder|Antal|Totalt|Antalet noder som är offline|
|RebootingNodeCount|Starta om antalet noder|Antal|Totalt|Antalet noder på att startas om|
|ReimagingNodeCount|Återställning av avbildning antalet noder|Antal|Totalt|Antalet noder för återställning av avbildning|
|RunningNodeCount|Kör antalet noder|Antal|Totalt|Antalet noder som körs|
|LeavingPoolNodeCount|Om du lämnar antalet noder för poolen|Antal|Totalt|Antalet noder som lämnar poolen|
|UnusableNodeCount|Antalet oanvändbar noder|Antal|Totalt|Antalet noder som inte kan användas|
|PreemptedNodeCount|Antalet frånkopplas noder|Antal|Totalt|Antalet frånkopplas noder|
|TaskStartEvent|Aktiviteten starta händelser|Antal|Totalt|Totalt antal aktiviteter som har startats|
|TaskCompleteEvent|Uppgift slutförd händelser|Antal|Totalt|Totalt antal aktiviteter som har slutförts|
|TaskFailEvent|Uppgiften misslyckas händelser|Antal|Totalt|Totalt antal aktiviteter som har slutförts i ett felaktigt tillstånd|
|PoolCreateEvent|Poolen skapa händelser|Antal|Totalt|Totalt antal pooler som har skapats|
|PoolResizeStartEvent|Poolen storleksändring Start händelser|Antal|Totalt|Totalt antal poolen storlek som har startat|
|PoolResizeCompleteEvent|Poolen storleksändring slutförd händelser|Antal|Totalt|Totalt antal poolen storlek som har slutförts|
|PoolDeleteStartEvent|Poolen ta bort Start händelser|Antal|Totalt|Totalt antal poolen borttagningar som har startat|
|PoolDeleteCompleteEvent|Poolen ta bort klar händelser|Antal|Totalt|Totalt antal poolen borttagningar som har slutförts|

## <a name="microsoftcacheredis"></a>Microsoft.Cache/redis

|Mått|Mått visningsnamn|Enhet|Sammansättningstyp|Beskrivning|
|---|---|---|---|---|
|connectedclients|Anslutna klienter|Antal|Maximalt||
|totalcommandsprocessed|Totalt antal åtgärder|Antal|Totalt||
|cachehits|Träffar i cache|Antal|Totalt||
|cachemisses|Cachemissar|Antal|Totalt||
|getcommands|Hämtar|Antal|Totalt||
|setcommands|Anger|Antal|Totalt||
|evictedkeys|Avlägsnade nycklar|Antal|Totalt||
|totalkeys|Totalt antal nycklar|Antal|Maximalt||
|expiredkeys|Utgångna nycklar|Antal|Totalt||
|usedmemory|Använt minne|Byte|Maximalt||
|usedmemoryRss|Använt minne RSS|Byte|Maximalt||
|serverLoad|Belastningen på servern|Procent|Maximalt||
|cacheWrite|Cache-skrivåtgärder|BytesPerSecond|Maximalt||
|cacheRead|Läs i cache|BytesPerSecond|Maximalt||
|PercentProcessorTime|Processor|Procent|Maximalt||
|connectedclients0|Anslutna klienter (Fragmentera 0)|Antal|Maximalt||
|totalcommandsprocessed0|Totalt antal åtgärder (Fragmentera 0)|Antal|Totalt||
|cachehits0|Cacheträffar (Fragmentera 0)|Antal|Totalt||
|cachemisses0|Cachemissar (Fragmentera 0)|Antal|Totalt||
|getcommands0|Hämtar (Fragmentera 0)|Antal|Totalt||
|setcommands0|Anger (Fragmentera 0)|Antal|Totalt||
|evictedkeys0|Avlägsnade nycklar (Fragmentera 0)|Antal|Totalt||
|totalkeys0|Totalt antal nycklar (Fragmentera 0)|Antal|Maximalt||
|expiredkeys0|Utgångna nycklar (Fragmentera 0)|Antal|Totalt||
|usedmemory0|Använt minne (Fragmentera 0)|Byte|Maximalt||
|usedmemoryRss0|Använt minne RSS (Fragmentera 0)|Byte|Maximalt||
|serverLoad0|Belastningen på servern (Fragmentera 0)|Procent|Maximalt||
|cacheWrite0|Cache-skrivning (Fragmentera 0)|BytesPerSecond|Maximalt||
|cacheRead0|Cache Läs (Fragmentera 0)|BytesPerSecond|Maximalt||
|percentProcessorTime0|CPU (Fragmentera 0)|Procent|Maximalt||
|connectedclients1|Anslutna klienter (Fragmentera 1)|Antal|Maximalt||
|totalcommandsprocessed1|Totalt antal åtgärder (Fragmentera 1)|Antal|Totalt||
|cachehits1|Cacheträffar (Fragmentera 1)|Antal|Totalt||
|cachemisses1|Cachemissar (Fragmentera 1)|Antal|Totalt||
|getcommands1|Hämtar (Fragmentera 1)|Antal|Totalt||
|setcommands1|Anger (Fragmentera 1)|Antal|Totalt||
|evictedkeys1|Avlägsnade nycklar (Fragmentera 1)|Antal|Totalt||
|totalkeys1|Totalt antal nycklar (Fragmentera 1)|Antal|Maximalt||
|expiredkeys1|Utgångna nycklar (Fragmentera 1)|Antal|Totalt||
|usedmemory1|Använt minne (Fragmentera 1)|Byte|Maximalt||
|usedmemoryRss1|Använt minne RSS (Fragmentera 1)|Byte|Maximalt||
|serverLoad1|Belastningen på servern (Fragmentera 1)|Procent|Maximalt||
|cacheWrite1|Cache-skrivning (Fragmentera 1)|BytesPerSecond|Maximalt||
|cacheRead1|Cache Läs (Fragmentera 1)|BytesPerSecond|Maximalt||
|percentProcessorTime1|CPU (Fragmentera 1)|Procent|Maximalt||
|connectedclients2|Anslutna klienter (Fragmentera 2)|Antal|Maximalt||
|totalcommandsprocessed2|Totalt antal åtgärder (Fragmentera 2)|Antal|Totalt||
|cachehits2|Cacheträffar (Fragmentera 2)|Antal|Totalt||
|cachemisses2|Cachemissar (Fragmentera 2)|Antal|Totalt||
|getcommands2|Hämtar (Fragmentera 2)|Antal|Totalt||
|setcommands2|Anger (Fragmentera 2)|Antal|Totalt||
|evictedkeys2|Avlägsnade nycklar (Fragmentera 2)|Antal|Totalt||
|totalkeys2|Totalt antal nycklar (Fragmentera 2)|Antal|Maximalt||
|expiredkeys2|Utgångna nycklar (Fragmentera 2)|Antal|Totalt||
|usedmemory2|Använt minne (Fragmentera 2)|Byte|Maximalt||
|usedmemoryRss2|Använt minne RSS (Fragmentera 2)|Byte|Maximalt||
|serverLoad2|Belastningen på servern (Fragmentera 2)|Procent|Maximalt||
|cacheWrite2|Cache-skrivning (Fragmentera 2)|BytesPerSecond|Maximalt||
|cacheRead2|Cache Läs (Fragmentera 2)|BytesPerSecond|Maximalt||
|percentProcessorTime2|CPU (Fragmentera 2)|Procent|Maximalt||
|connectedclients3|Anslutna klienter (Fragmentera 3)|Antal|Maximalt||
|totalcommandsprocessed3|Totalt antal åtgärder (Fragmentera 3)|Antal|Totalt||
|cachehits3|Cacheträffar (Fragmentera 3)|Antal|Totalt||
|cachemisses3|Cachemissar (Fragmentera 3)|Antal|Totalt||
|getcommands3|Hämtar (Fragmentera 3)|Antal|Totalt||
|setcommands3|Anger (Fragmentera 3)|Antal|Totalt||
|evictedkeys3|Avlägsnade nycklar (Fragmentera 3)|Antal|Totalt||
|totalkeys3|Totalt antal nycklar (Fragmentera 3)|Antal|Maximalt||
|expiredkeys3|Utgångna nycklar (Fragmentera 3)|Antal|Totalt||
|usedmemory3|Använt minne (Fragmentera 3)|Byte|Maximalt||
|usedmemoryRss3|Använt minne RSS (Fragmentera 3)|Byte|Maximalt||
|serverLoad3|Belastningen på servern (Fragmentera 3)|Procent|Maximalt||
|cacheWrite3|Cache-skrivning (Fragmentera 3)|BytesPerSecond|Maximalt||
|cacheRead3|Cache Läs (Fragmentera 3)|BytesPerSecond|Maximalt||
|percentProcessorTime3|CPU (Fragmentera 3)|Procent|Maximalt||
|connectedclients4|Anslutna klienter (Fragmentera 4)|Antal|Maximalt||
|totalcommandsprocessed4|Totalt antal åtgärder (Fragmentera 4)|Antal|Totalt||
|cachehits4|Cacheträffar (Fragmentera 4)|Antal|Totalt||
|cachemisses4|Cachemissar (Fragmentera 4)|Antal|Totalt||
|getcommands4|Hämtar (Fragmentera 4)|Antal|Totalt||
|setcommands4|Anger (Fragmentera 4)|Antal|Totalt||
|evictedkeys4|Avlägsnade nycklar (Fragmentera 4)|Antal|Totalt||
|totalkeys4|Totalt antal nycklar (Fragmentera 4)|Antal|Maximalt||
|expiredkeys4|Utgångna nycklar (Fragmentera 4)|Antal|Totalt||
|usedmemory4|Använt minne (Fragmentera 4)|Byte|Maximalt||
|usedmemoryRss4|Använt minne RSS (Fragmentera 4)|Byte|Maximalt||
|serverLoad4|Belastningen på servern (Fragmentera 4)|Procent|Maximalt||
|cacheWrite4|Cache-skrivning (Fragmentera 4)|BytesPerSecond|Maximalt||
|cacheRead4|Cache Läs (Fragmentera 4)|BytesPerSecond|Maximalt||
|percentProcessorTime4|CPU (Fragmentera 4)|Procent|Maximalt||
|connectedclients5|Anslutna klienter (Fragmentera 5)|Antal|Maximalt||
|totalcommandsprocessed5|Totalt antal åtgärder (Fragmentera 5)|Antal|Totalt||
|cachehits5|Cacheträffar (Fragmentera 5)|Antal|Totalt||
|cachemisses5|Cachemissar (Fragmentera 5)|Antal|Totalt||
|getcommands5|Hämtar (Fragmentera 5)|Antal|Totalt||
|setcommands5|Anger (Fragmentera 5)|Antal|Totalt||
|evictedkeys5|Avlägsnade nycklar (Fragmentera 5)|Antal|Totalt||
|totalkeys5|Totalt antal nycklar (Fragmentera 5)|Antal|Maximalt||
|expiredkeys5|Utgångna nycklar (Fragmentera 5)|Antal|Totalt||
|usedmemory5|Använt minne (Fragmentera 5)|Byte|Maximalt||
|usedmemoryRss5|Använt minne RSS (Fragmentera 5)|Byte|Maximalt||
|serverLoad5|Belastningen på servern (Fragmentera 5)|Procent|Maximalt||
|cacheWrite5|Cache-skrivning (Fragmentera 5)|BytesPerSecond|Maximalt||
|cacheRead5|Cache Läs (Fragmentera 5)|BytesPerSecond|Maximalt||
|percentProcessorTime5|CPU (Fragmentera 5)|Procent|Maximalt||
|connectedclients6|Anslutna klienter (Fragmentera 6)|Antal|Maximalt||
|totalcommandsprocessed6|Totalt antal åtgärder (Fragmentera 6)|Antal|Totalt||
|cachehits6|Cacheträffar (Fragmentera 6)|Antal|Totalt||
|cachemisses6|Cachemissar (Fragmentera 6)|Antal|Totalt||
|getcommands6|Hämtar (Fragmentera 6)|Antal|Totalt||
|setcommands6|Anger (Fragmentera 6)|Antal|Totalt||
|evictedkeys6|Avlägsnade nycklar (Fragmentera 6)|Antal|Totalt||
|totalkeys6|Totalt antal nycklar (Fragmentera 6)|Antal|Maximalt||
|expiredkeys6|Utgångna nycklar (Fragmentera 6)|Antal|Totalt||
|usedmemory6|Använt minne (Fragmentera 6)|Byte|Maximalt||
|usedmemoryRss6|Använt minne RSS (Fragmentera 6)|Byte|Maximalt||
|serverLoad6|Belastningen på servern (Fragmentera 6)|Procent|Maximalt||
|cacheWrite6|Cache-skrivning (Fragmentera 6)|BytesPerSecond|Maximalt||
|cacheRead6|Cache Läs (Fragmentera 6)|BytesPerSecond|Maximalt||
|percentProcessorTime6|CPU (Fragmentera 6)|Procent|Maximalt||
|connectedclients7|Anslutna klienter (Fragmentera 7)|Antal|Maximalt||
|totalcommandsprocessed7|Totalt antal åtgärder (Fragmentera 7)|Antal|Totalt||
|cachehits7|Cacheträffar (Fragmentera 7)|Antal|Totalt||
|cachemisses7|Cachemissar (Fragmentera 7)|Antal|Totalt||
|getcommands7|Hämtar (Fragmentera 7)|Antal|Totalt||
|setcommands7|Anger (Fragmentera 7)|Antal|Totalt||
|evictedkeys7|Avlägsnade nycklar (Fragmentera 7)|Antal|Totalt||
|totalkeys7|Totalt antal nycklar (Fragmentera 7)|Antal|Maximalt||
|expiredkeys7|Utgångna nycklar (Fragmentera 7)|Antal|Totalt||
|usedmemory7|Använt minne (Fragmentera 7)|Byte|Maximalt||
|usedmemoryRss7|Använt minne RSS (Fragmentera 7)|Byte|Maximalt||
|serverLoad7|Belastningen på servern (Fragmentera 7)|Procent|Maximalt||
|cacheWrite7|Cache-skrivning (Fragmentera 7)|BytesPerSecond|Maximalt||
|cacheRead7|Cache Läs (Fragmentera 7)|BytesPerSecond|Maximalt||
|percentProcessorTime7|CPU (Fragmentera 7)|Procent|Maximalt||
|connectedclients8|Anslutna klienter (Fragmentera 8)|Antal|Maximalt||
|totalcommandsprocessed8|Totalt antal åtgärder (Fragmentera 8)|Antal|Totalt||
|cachehits8|Cacheträffar (Fragmentera 8)|Antal|Totalt||
|cachemisses8|Cachemissar (Fragmentera 8)|Antal|Totalt||
|getcommands8|Hämtar (Fragmentera 8)|Antal|Totalt||
|setcommands8|Anger (Fragmentera 8)|Antal|Totalt||
|evictedkeys8|Avlägsnade nycklar (Fragmentera 8)|Antal|Totalt||
|totalkeys8|Totalt antal nycklar (Fragmentera 8)|Antal|Maximalt||
|expiredkeys8|Utgångna nycklar (Fragmentera 8)|Antal|Totalt||
|usedmemory8|Använt minne (Fragmentera 8)|Byte|Maximalt||
|usedmemoryRss8|Använt minne RSS (Fragmentera 8)|Byte|Maximalt||
|serverLoad8|Belastningen på servern (Fragmentera 8)|Procent|Maximalt||
|cacheWrite8|Cache-skrivning (Fragmentera 8)|BytesPerSecond|Maximalt||
|cacheRead8|Cache Läs (Fragmentera 8)|BytesPerSecond|Maximalt||
|percentProcessorTime8|CPU (Fragmentera 8)|Procent|Maximalt||
|connectedclients9|Anslutna klienter (Fragmentera 9)|Antal|Maximalt||
|totalcommandsprocessed9|Totalt antal åtgärder (Fragmentera 9)|Antal|Totalt||
|cachehits9|Cacheträffar (Fragmentera 9)|Antal|Totalt||
|cachemisses9|Cachemissar (Fragmentera 9)|Antal|Totalt||
|getcommands9|Hämtar (Fragmentera 9)|Antal|Totalt||
|setcommands9|Anger (Fragmentera 9)|Antal|Totalt||
|evictedkeys9|Avlägsnade nycklar (Fragmentera 9)|Antal|Totalt||
|totalkeys9|Totalt antal nycklar (Fragmentera 9)|Antal|Maximalt||
|expiredkeys9|Utgångna nycklar (Fragmentera 9)|Antal|Totalt||
|usedmemory9|Använt minne (Fragmentera 9)|Byte|Maximalt||
|usedmemoryRss9|Använt minne RSS (Fragmentera 9)|Byte|Maximalt||
|serverLoad9|Belastningen på servern (Fragmentera 9)|Procent|Maximalt||
|cacheWrite9|Cache-skrivning (Fragmentera 9)|BytesPerSecond|Maximalt||
|cacheRead9|Cache Läs (Fragmentera 9)|BytesPerSecond|Maximalt||
|percentProcessorTime9|CPU (Fragmentera 9)|Procent|Maximalt||

## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.CognitiveServices/accounts

|Mått|Mått visningsnamn|Enhet|Sammansättningstyp|Beskrivning|
|---|---|---|---|---|
|TotalCalls|Totalt antal anrop|Antal|Totalt|Totalt antal anrop.|
|SuccessfulCalls|Antal samtal|Antal|Totalt|Antal lyckade anrop.|
|TotalErrors|Totalt antal fel|Antal|Totalt|Totalt antal anrop med felsvar (http-svar kod 4xx eller 5xx).|
|BlockedCalls|Blockerade anrop|Antal|Totalt|Antal anrop som överskred hastighet eller kvotgränsen.|
|ServerErrors|Serverfel|Antal|Totalt|Antal anrop med internt tjänstfel (http-svar kod 5xx).|
|ClientErrors|Klientfel|Antal|Totalt|Antal anrop med klientfel sida (http-svar kod 4xx).|
|DataIn|Data i|Byte|Totalt|Storleken på inkommande data i byte.|
|DataOut|Ut data|Byte|Totalt|Storleken på utgående data i byte.|
|Svarstid|Svarstid|Millisekunder|Genomsnittlig|Fördröjning i millisekunder.|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

|Mått|Mått visningsnamn|Enhet|Sammansättningstyp|Beskrivning|
|---|---|---|---|---|
|Procentandel CPU|Procentandel CPU|Procent|Genomsnittlig|Procentandelen allokerat beräknings-enheter som för närvarande används av minst en virtuell dator|
|Nätverk i|Nätverk i|Byte|Totalt|Antalet byte som tagits emot på alla nätverksgränssnitt av de virtuella datorerna (inkommande trafik)|
|Nätverk ut|Nätverk ut|Byte|Totalt|Antalet byte ut på alla nätverksgränssnitt som de virtuella datorerna (utgående trafik)|
|Disk lästa byte|Disk lästa byte|Byte|Totalt|Totalt antal byte som har lästs från disken under perioden|
|Disk byte för skrivning|Disk byte för skrivning|Byte|Totalt|Totalt antal byte som skrivs till disk under perioden|
|Disk-lästa åtgärder/sek|Disk-lästa åtgärder/sek|CountPerSecond|Genomsnittlig|Diskläsning IOPS|
|Disk skrivåtgärder per sekund|Disk skrivåtgärder per sekund|CountPerSecond|Genomsnittlig|Diskskrivning IOPS|

## <a name="microsoftcomputevirtualmachinescalesets"></a>Microsoft.Compute/virtualMachineScaleSets

|Mått|Mått visningsnamn|Enhet|Sammansättningstyp|Beskrivning|
|---|---|---|---|---|
|Procentandel CPU|Procentandel CPU|Procent|Genomsnittlig|Procentandelen allokerat beräknings-enheter som för närvarande används av minst en virtuell dator|
|Nätverk i|Nätverk i|Byte|Totalt|Antalet byte som tagits emot på alla nätverksgränssnitt av de virtuella datorerna (inkommande trafik)|
|Nätverk ut|Nätverk ut|Byte|Totalt|Antalet byte ut på alla nätverksgränssnitt som de virtuella datorerna (utgående trafik)|
|Disk lästa byte|Disk lästa byte|Byte|Totalt|Totalt antal byte som har lästs från disken under perioden|
|Disk byte för skrivning|Disk byte för skrivning|Byte|Totalt|Totalt antal byte som skrivs till disk under perioden|
|Disk-lästa åtgärder/sek|Disk-lästa åtgärder/sek|CountPerSecond|Genomsnittlig|Diskläsning IOPS|
|Disk skrivåtgärder per sekund|Disk skrivåtgärder per sekund|CountPerSecond|Genomsnittlig|Diskskrivning IOPS|

## <a name="microsoftcomputevirtualmachinescalesetsvirtualmachines"></a>Microsoft.Compute/virtualMachineScaleSets/virtualMachines

|Mått|Mått visningsnamn|Enhet|Sammansättningstyp|Beskrivning|
|---|---|---|---|---|
|Procentandel CPU|Procentandel CPU|Procent|Genomsnittlig|Procentandelen allokerat beräknings-enheter som för närvarande används av minst en virtuell dator|
|Nätverk i|Nätverk i|Byte|Totalt|Antalet byte som tagits emot på alla nätverksgränssnitt av de virtuella datorerna (inkommande trafik)|
|Nätverk ut|Nätverk ut|Byte|Totalt|Antalet byte ut på alla nätverksgränssnitt som de virtuella datorerna (utgående trafik)|
|Disk lästa byte|Disk lästa byte|Byte|Totalt|Totalt antal byte som har lästs från disken under perioden|
|Disk byte för skrivning|Disk byte för skrivning|Byte|Totalt|Totalt antal byte som skrivs till disk under perioden|
|Disk-lästa åtgärder/sek|Disk-lästa åtgärder/sek|CountPerSecond|Genomsnittlig|Diskläsning IOPS|
|Disk skrivåtgärder per sekund|Disk skrivåtgärder per sekund|CountPerSecond|Genomsnittlig|Diskskrivning IOPS|

## <a name="microsoftcustomerinsightshubs"></a>Microsoft.CustomerInsights/hubs

|Mått|Mått visningsnamn|Enhet|Sammansättningstyp|Beskrivning|
|---|---|---|---|---|
|DCIApiCalls|Kunden Insights API-anrop|Antal|Totalt||
|DCIMappingImportOperationSuccessfulLines|Mappning importera åtgärden lyckas rader|Antal|Totalt||
|DCIMappingImportOperationFailedLines|Det gick inte att mappa importen rader|Antal|Totalt||
|DCIMappingImportOperationTotalLines|Totalt antal rader för mappning importera åtgärden|Antal|Totalt||
|DCIMappingImportOperationRuntimeInSeconds|Mappning importera åtgärden körning i sekunder|Sekunder|Totalt||
|DCIOutboundProfileExportSucceeded|Utgående profil exporten har slutförts|Antal|Totalt||
|DCIOutboundProfileExportFailed|Utgående profil exporten misslyckades|Antal|Totalt||
|DCIOutboundProfileExportDuration|Utgående profil Export varaktighet|Sekunder|Totalt||
|DCIOutboundKpiExportSucceeded|Lyckad utgående Kpi-Export|Antal|Totalt||
|DCIOutboundKpiExportFailed|Utgående Kpi-exporten misslyckades|Antal|Totalt||
|DCIOutboundKpiExportDuration|Utgående Kpi Export varaktighet|Sekunder|Totalt||
|DCIOutboundKpiExportStarted|Utgående Kpi Export igång|Sekunder|Totalt||
|DCIOutboundKpiRecordCount|Utgående Kpi postantal|Sekunder|Totalt||
|DCIOutboundProfileExportCount|Antalet för Export av utgående profiler|Sekunder|Totalt||
|DCIOutboundInitialProfileExportFailed|Utgående första profil exporten misslyckades|Sekunder|Totalt||
|DCIOutboundInitialProfileExportSucceeded|Utgående första profil exporten har slutförts|Sekunder|Totalt||
|DCIOutboundInitialKpiExportFailed|Utgående inledande Kpi-exporten misslyckades|Sekunder|Totalt||
|DCIOutboundInitialKpiExportSucceeded|Lyckad utgående inledande Kpi-Export|Sekunder|Totalt||
|DCIOutboundInitialProfileExportDurationInSeconds|Utgående första profil Export varaktighet i sekunder|Sekunder|Totalt||
|AdlaJobForStandardKpiFailed|Adla jobb för Standard-Kpi misslyckades i sekunder|Sekunder|Totalt||
|AdlaJobForStandardKpiTimeOut|Adla jobb för Standard Kpi tidsgräns i sekunder|Sekunder|Totalt||
|AdlaJobForStandardKpiCompleted|Adla jobb för Standard-Kpi slutförts i sekunder|Sekunder|Totalt||
|ImportASAValuesFailed|Importera ASA värden kunde inte räkna|Antal|Totalt||
|ImportASAValuesSucceeded|Importera ASA värden lyckades antal|Antal|Totalt||

## <a name="microsoftdatalakeanalyticsaccounts"></a>Microsoft.DataLakeAnalytics/accounts

|Mått|Mått visningsnamn|Enhet|Sammansättningstyp|Beskrivning|
|---|---|---|---|---|
|JobEndedSuccess|Slutförda jobb|Antal|Totalt|Antal slutförda jobb.|
|JobEndedFailure|Misslyckade jobb|Antal|Totalt|Antal misslyckade jobb.|
|JobEndedCancelled|Avbrutna jobb|Antal|Totalt|Antal avbrutna jobb.|
|JobAUEndedSuccess|Lyckad Australien tid|Sekunder|Totalt|Totalt antal Australien tid för slutförda jobb.|
|JobAUEndedFailure|Misslyckade Australien tid|Sekunder|Totalt|Totalt antal Australien tid för misslyckade jobb.|
|JobAUEndedCancelled|Annullerad Australien tid|Sekunder|Totalt|Total tid Australien för avbrutna jobb.|

## <a name="microsoftdbformysqlservers"></a>Microsoft.DBforMySQL/servers

|Mått|Mått visningsnamn|Enhet|Sammansättningstyp|Beskrivning|
|---|---|---|---|---|
|cpu_percent|CPU-procent|Procent|Genomsnittlig|CPU-procent|
|compute_limit|Compute-gränsen för enhet|Antal|Genomsnittlig|Compute-gränsen för enhet|
|compute_consumption_percent|Beräkna procentandelen enhet|Procent|Genomsnittlig|Beräkna procentandelen enhet|
|memory_percent|Minne|Procent|Genomsnittlig|Minne|
|io_consumption_percent|IO-procent|Procent|Genomsnittlig|IO-procent|
|storage_percent|Lagringsprocent|Procent|Genomsnittlig|Lagringsprocent|
|storage_used|Använt lagringsutrymme|Byte|Genomsnittlig|Använt lagringsutrymme|
|storage_limit|Lagringsgränsen|Byte|Genomsnittlig|Lagringsgränsen|
|active_connections|Totalt antal aktiva anslutningar|Antal|Genomsnittlig|Totalt antal aktiva anslutningar|
|connections_failed|Totalt antal misslyckade anslutningar|Antal|Genomsnittlig|Totalt antal misslyckade anslutningar|

## <a name="microsoftdbforpostgresqlservers"></a>Microsoft.DBforPostgreSQL/servers

|Mått|Mått visningsnamn|Enhet|Sammansättningstyp|Beskrivning|
|---|---|---|---|---|
|cpu_percent|CPU-procent|Procent|Genomsnittlig|CPU-procent|
|compute_limit|Compute-gränsen för enhet|Antal|Genomsnittlig|Compute-gränsen för enhet|
|compute_consumption_percent|Beräkna procentandelen enhet|Procent|Genomsnittlig|Beräkna procentandelen enhet|
|memory_percent|Minne|Procent|Genomsnittlig|Minne|
|io_consumption_percent|IO-procent|Procent|Genomsnittlig|IO-procent|
|storage_percent|Lagringsprocent|Procent|Genomsnittlig|Lagringsprocent|
|storage_used|Använt lagringsutrymme|Byte|Genomsnittlig|Använt lagringsutrymme|
|storage_limit|Lagringsgränsen|Byte|Genomsnittlig|Lagringsgränsen|
|active_connections|Totalt antal aktiva anslutningar|Antal|Genomsnittlig|Totalt antal aktiva anslutningar|
|connections_failed|Totalt antal misslyckade anslutningar|Antal|Genomsnittlig|Totalt antal misslyckade anslutningar|

## <a name="microsoftdevicesiothubs"></a>Microsoft.Devices/IotHubs

|Mått|Mått visningsnamn|Enhet|Sammansättningstyp|Beskrivning|
|---|---|---|---|---|
|d2c.telemetry.Ingress.allProtocol|Telemetri meddelande Skicka försök|Antal|Totalt|Antal meddelanden enhet till moln telemetri försökte skickas till din IoT-hubb|
|d2c.telemetry.Ingress.Success|Telemetri meddelanden som skickas|Antal|Totalt|Antal enhet till moln telemetri meddelanden som skickats till din IoT-hubb|
|c2d.commands.egress.Complete.Success|Kommandon som slutförd|Antal|Totalt|Antal moln till enhet kommandon har slutförts av enheten|
|c2d.commands.egress.Abandon.Success|Avbrutna kommandon|Antal|Totalt|Antal moln till enhet kommandon avbrutna av enheten|
|c2d.commands.egress.Reject.Success|Kommandon som avvisat|Antal|Totalt|Antal moln till enhet kommandon som avvisats av enheten|
|devices.totalDevices|Totalt antal enheter|Antal|Totalt|Antalet enheter som registrerats för din IoT-hubb|
|devices.connectedDevices.allProtocol|Anslutna enheter|Antal|Totalt|Antalet enheter som är anslutna till din IoT-hubb|
|d2c.telemetry.egress.Success|Telemetri meddelanden|Antal|Totalt|Antal gånger som meddelanden som skrivits till slutpunkter (totalt)|
|d2c.telemetry.egress.dropped|Ignorerade meddelanden|Antal|Totalt|Antal meddelanden som utelämnats eftersom leverans slutpunkten har döda|
|d2c.telemetry.egress.orphaned|Överblivna meddelanden|Antal|Totalt|Antal meddelanden som inte matchar några vägar inklusive återställningsplats flöde|
|d2c.telemetry.egress.Invalid|Ogiltig meddelanden|Antal|Totalt|Antalet meddelanden som inte levereras på grund av inkompatibilitet med slutpunkten|
|d2c.telemetry.egress.fallback|Meddelanden som matchar villkoret återställningsplats|Antal|Totalt|Antal meddelanden som skrivs till fallback-slutpunkten|
|d2c.endpoints.egress.eventHubs|Skicka meddelanden till Event Hub-slutpunkter|Antal|Totalt|Antal gånger som meddelanden som skrivits till Event Hub-slutpunkter|
|d2c.endpoints.latency.eventHubs|Meddelandet svarstid för Event Hub-slutpunkter|millisekunder|Genomsnittlig|Genomsnittlig svarstid mellan meddelandet telemetriingång till IoT-hubb och meddelandet ingång i Event Hub slutpunkt, i millisekunder|
|d2c.endpoints.egress.serviceBusQueues|Skicka meddelanden till Service Bus-kö slutpunkter|Antal|Totalt|Antal gånger som meddelanden som skrivits till Service Bus-kö slutpunkter|
|d2c.endpoints.latency.serviceBusQueues|Meddelandet svarstid för Service Bus-kö-slutpunkter|millisekunder|Genomsnittlig|Genomsnittlig svarstid mellan meddelandet telemetriingång till IoT-hubb och meddelandet ingång till en slutpunkt för Service Bus-kö, i millisekunder|
|d2c.endpoints.egress.serviceBusTopics|Skicka meddelanden till Service Bus-ämne slutpunkter|Antal|Totalt|Antal gånger som meddelanden som skrivits till Service Bus-ämne slutpunkter|
|d2c.endpoints.latency.serviceBusTopics|Meddelandet svarstiden för Service Bus-ämne slutpunkter|millisekunder|Genomsnittlig|Genomsnittlig svarstid mellan meddelandet telemetriingång till IoT-hubb och meddelandet ingång till en slutpunkt för Service Bus-ämne, i millisekunder|
|d2c.endpoints.egress.builtIn.events|Skicka meddelanden till inbyggd slutpunkt (meddelanden/händelser)|Antal|Totalt|Antal gånger som meddelanden som skrivits till inbyggd slutpunkt (meddelanden/händelser)|
|d2c.endpoints.latency.builtIn.events|Meddelandet svarstid för inbyggd slutpunkt (meddelanden/händelser)|millisekunder|Genomsnittlig|Genomsnittlig svarstid mellan meddelandet telemetriingång till IoT-hubb och meddelandet ingång i inbyggd slutpunkt (meddelanden/händelser), i millisekunder |
|d2c.Twin.Read.Success|Lyckad dubbla läser från enheter|Antal|Totalt|Antal alla lyckade enhet initieras dubbla läser.|
|d2c.Twin.Read.failure|Det gick inte dubbla läsningar av enheter|Antal|Totalt|Antalet alla misslyckades enhet initieras dubbla läsningar.|
|d2c.Twin.Read.size|Svarsstorlek dubbla läsningar av enheter|Byte|Genomsnittlig|Medel, min och max för alla lyckade enhet initieras dubbla läsningar.|
|d2c.Twin.Update.Success|Lyckad dubbla uppdateringar från enheter|Antal|Totalt|Antal uppdateringar för alla lyckade enhet initieras dubbla.|
|d2c.Twin.Update.failure|Det gick inte dubbla uppdateringar från enheter|Antal|Totalt|Antalet alla misslyckades enhet initieras dubbla uppdateringar.|
|d2c.Twin.Update.size|Storleken på dubbla uppdateringar från enheter|Byte|Genomsnittlig|Medel, min och max storlek för alla lyckade enhet initieras dubbla uppdateringar.|
|c2d.methods.Success|Lyckad direkta metoden anrop|Antal|Totalt|Antal alla lyckade direkt metodanrop.|
|c2d.methods.failure|Det gick inte direkt metod anrop|Antal|Totalt|Det gick inte att direkt metodanrop antalet alla.|
|c2d.methods.requestSize|Storleken på direkta metoden anrop för begäran|Byte|Genomsnittlig|Medel, min och max för alla lyckade direkta metodbegäranden.|
|c2d.methods.responseSize|Storlek för direkta metoden anrop|Byte|Genomsnittlig|Medel, min och max för alla lyckade direkta metoden svar.|
|c2d.Twin.Read.Success|Lyckad dubbla läser från serverdelen|Antal|Totalt|Antal alla lyckade tillbaka-end-initierad dubbla läser.|
|c2d.Twin.Read.failure|Misslyckade dubbla läser från serverdelen|Antal|Totalt|Antalet alla misslyckades tillbaka-end-initierad dubbla läsningar.|
|c2d.Twin.Read.size|Storlek för dubbla läser från serverdelen|Byte|Genomsnittlig|Medel, min och max för alla lyckade tillbaka-end-initierade dubbla läsningar.|
|c2d.Twin.Update.Success|Lyckad dubbla uppdateringar från serverdelen|Antal|Totalt|Antal uppdateringar för alla lyckade tillbaka-end-initierad dubbla.|
|c2d.Twin.Update.failure|Misslyckade dubbla uppdateringar från serverdelen|Antal|Totalt|Antalet alla misslyckades tillbaka-end-initierad dubbla uppdateringar.|
|c2d.Twin.Update.size|Storleken på dubbla uppdateringar från serverdelen|Byte|Genomsnittlig|Medel, min och max storlek för alla lyckade tillbaka-end-initierad dubbla uppdateringar.|
|twinQueries.success|Lyckad dubbla frågor|Antal|Totalt|Antal alla lyckade dubbla frågor.|
|twinQueries.failure|Misslyckade dubbla frågor|Antal|Totalt|Antal alla misslyckade dubbla frågor.|
|twinQueries.resultSize|Dubbla frågor storlek|Byte|Genomsnittlig|Medel, min och max resultatet av alla lyckade dubbla frågor storlek.|
|jobs.createTwinUpdateJob.success|Lyckad skapande av dubbla uppdatera jobb|Antal|Totalt|Antal alla lyckade skapandet av dubbla uppdateringsjobb.|
|jobs.createTwinUpdateJob.failure|Misslyckade skapande av dubbla uppdatera jobb|Antal|Totalt|Antal alla misslyckad generering av dubbla uppdateringsjobb.|
|jobs.createDirectMethodJob.success|Lyckad skapande av metoden anrops-jobb|Antal|Totalt|Antal alla har skapats på direkta metoden anrop jobb.|
|jobs.createDirectMethodJob.failure|Misslyckade skapande av metoden anrops-jobb|Antal|Totalt|Antal alla misslyckad generering av direkta metoden anrop jobb.|
|jobs.listJobs.success|Antal samtal till listan jobb|Antal|Totalt|Antal alla lyckade anrop till listan jobb.|
|jobs.listJobs.failure|Det gick inte anrop till listan jobb|Antal|Totalt|Antal alla misslyckade anrop till listan jobb.|
|jobs.cancelJob.success|Lyckad jobbavbrott|Antal|Totalt|Antal lyckade anrop avbryta ett jobb.|
|jobs.cancelJob.failure|Avbryta misslyckade jobb|Antal|Totalt|Antal misslyckade anrop avbryta ett jobb.|
|jobs.queryJobs.success|Jobbfrågor|Antal|Totalt|Antal alla lyckade anrop till frågan jobb.|
|jobs.queryJobs.failure|Misslyckade jobbfrågor|Antal|Totalt|Antal alla misslyckade anrop till frågan jobb.|
|jobs.Completed|Slutförda jobb|Antal|Totalt|Antal alla slutförda jobb.|
|jobs.Failed|Misslyckade jobb|Antal|Totalt|Antal alla misslyckade jobb.|
|d2c.telemetry.Ingress.sendThrottle|Antalet fel som bandbreddsbegränsning|Antal|Totalt|Begränsar antalet bandbreddsbegränsning felen på grund av enheten genomflöde|
|dailyMessageQuotaUsed|Sammanlagt antal meddelanden som används|Antal|Genomsnittlig|Antal Totalt antal meddelanden som används idag|

## <a name="microsofteventhubnamespaces"></a>Microsoft.EventHub/namespaces

|Mått|Mått visningsnamn|Enhet|Sammansättningstyp|Beskrivning|
|---|---|---|---|---|
|INREQS|Skicka förfrågningar|Antal|Totalt|Totalt antal inkommande skickar begäranden för en meddelandehubb|
|SUCCREQ|Lyckade begäranden|Antal|Totalt|Totalt antal lyckade begäranden för ett namnområde|
|FAILREQ|Misslyckade begäranden|Antal|Totalt|Totalt antal misslyckade begäranden för ett namnområde|
|SVRBSY|Upptagen serverfel|Antal|Totalt|Totalt antal upptagen serverfel för ett namnområde|
|INTERR|Internt serverfel|Antal|Totalt|Totalt antal internt serverfel för ett namnområde|
|MISCERR|Andra fel|Antal|Totalt|Totalt antal misslyckade begäranden för ett namnområde|
|INMSGS|Inkommande meddelanden|Antal|Totalt|Totalt antal inkommande meddelanden för ett namnområde|
|OUTMSGS|Utgående meddelanden|Antal|Totalt|Total utgående meddelanden för ett namnområde|
|EHINMBS|Inkommande byte|Byte|Totalt|Event Hub inkommande genomströmningen för ett namnområde|
|EHOUTMBS|Utgående byte|Byte|Totalt|Total utgående meddelanden för ett namnområde|
|EHABL|Arkivera eftersläpning meddelanden|Antal|Totalt|Händelsemeddelanden hubb Arkiv eftersläpande för ett namnområde|
|EHAMSGS|Arkivera meddelanden|Antal|Totalt|Event Hub arkiverade meddelanden i ett namnområde|
|EHAMBS|Arkivera genomströmningen|Byte|Totalt|Event Hub arkiveras genomströmningen i ett namnområde|

## <a name="microsoftlogicworkflows"></a>Microsoft.Logic/workflows

|Mått|Mått visningsnamn|Enhet|Sammansättningstyp|Beskrivning|
|---|---|---|---|---|
|RunsStarted|Kör igång|Antal|Totalt|Antal startade arbetsflödeskörningar.|
|RunsCompleted|Genomförda körningar|Antal|Totalt|Antal slutförda arbetsflödeskörningar.|
|RunsSucceeded|Kör lyckades|Antal|Totalt|Antal lyckade arbetsflödeskörningar.|
|RunsFailed|Kör misslyckades|Antal|Totalt|Antal misslyckade arbetsflödeskörningar.|
|RunsCancelled|Kör avbröts|Antal|Totalt|Antal avbrutna arbetsflödeskörningar.|
|RunLatency|Kör svarstid|Sekunder|Genomsnittlig|Svarstid för slutförda arbetsflödeskörningar.|
|RunSuccessLatency|Kör lyckade svarstid|Sekunder|Genomsnittlig|Svarstid för lyckade arbetsflödeskörningar.|
|RunThrottledEvents|Kör begränsad händelser|Antal|Totalt|Antal arbetsflödesåtgärds- eller utlösarbegränsade händelser.|
|RunFailurePercentage|Kör procentandel fel|Procent|Totalt|Procentandel av arbetsflödet körs misslyckades.|
|ActionsStarted|Åtgärder som har startats |Antal|Totalt|Antal startade arbetsflödesåtgärder.|
|ActionsCompleted|Åtgärder som har slutförts |Antal|Totalt|Antal slutförda arbetsflödesåtgärder.|
|ActionsSucceeded|Åtgärder har slutförts |Antal|Totalt|Antal lyckade arbetsflödesåtgärder.|
|ActionsFailed|Åtgärder som misslyckades|Antal|Totalt|Antal misslyckade arbetsflödesåtgärder.|
|ActionsSkipped|Åtgärder som hoppades över |Antal|Totalt|Antal överhoppade arbetsflödesåtgärder.|
|ActionLatency|Åtgärden svarstid |Sekunder|Genomsnittlig|Svarstid för slutförda arbetsflödesåtgärder.|
|ActionSuccessLatency|Åtgärden lyckades svarstid |Sekunder|Genomsnittlig|Svarstid för lyckade arbetsflödesåtgärder.|
|ActionThrottledEvents|Åtgärden begränsas händelser|Antal|Totalt|Antal begränsade händelser för arbetsflödesåtgärder.|
|TriggersStarted|Utlösare har startats |Antal|Totalt|Antal startade arbetsflödesutlösare.|
|TriggersCompleted|Utlösare som har slutförts |Antal|Totalt|Antal slutförda arbetsflödesutlösare.|
|TriggersSucceeded|Utlösare lyckades |Antal|Totalt|Antal lyckade arbetsflödesutlösare.|
|TriggersFailed|Utlösare misslyckades |Antal|Totalt|Antal misslyckade arbetsflödesutlösare.|
|TriggersSkipped|Utlösare hoppades över|Antal|Totalt|Antal överhoppade arbetsflödesutlösare.|
|TriggersFired|Utlösare utlöses |Antal|Totalt|Antal utlösta arbetsflödesutlösare.|
|TriggerLatency|Utlösaren svarstid |Sekunder|Genomsnittlig|Svarstid för slutförda arbetsflödesutlösare.|
|TriggerFireLatency|Utlösaren brand svarstid |Sekunder|Genomsnittlig|Svartstid för utlösta arbetsflödesutlösare.|
|TriggerSuccessLatency|Utlösaren lyckade svarstid |Sekunder|Genomsnittlig|Svartstid för lyckade arbetsflödesutlösare.|
|TriggerThrottledEvents|Utlösaren begränsas händelser|Antal|Totalt|Antal begränsade händelser för arbetsflödesutlösare.|
|BillableActionExecutions|Fakturerbar åtgärd körningar|Antal|Totalt|Antal arbetsflödet åtgärd körningar komma debiteras.|
|BillableTriggerExecutions|Fakturerbar utlösaren körningar|Antal|Totalt|Antal arbetsflödet utlösaren körningar komma debiteras.|
|TotalBillableExecutions|Totalt antal fakturerbar körningar|Antal|Totalt|Antal arbetsflödet körningar komma debiteras.|

## <a name="microsoftnetworkapplicationgateways"></a>Microsoft.Network/applicationGateways

|Mått|Mått visningsnamn|Enhet|Sammansättningstyp|Beskrivning|
|---|---|---|---|---|
|Dataflöde|Dataflöde|BytesPerSecond|Genomsnittlig||

## <a name="microsoftnetworkexpressroutecircuits"></a>Microsoft.Network/expressRouteCircuits

|Mått|Mått visningsnamn|Enhet|Sammansättningstyp|Beskrivning|
|---|---|---|---|---|
|BytesIn är större än|BytesIn är större än|Antal|Totalt||
|BytesOut är större än|BytesOut är större än|Antal|Totalt||

## <a name="microsoftnotificationhubsnamespacesnotificationhubs"></a>Microsoft.NotificationHubs/Namespaces/NotificationHubs

|Mått|Mått visningsnamn|Enhet|Sammansättningstyp|Beskrivning|
|---|---|---|---|---|
|Registration.all|Åtgärden för registrering|Antal|Totalt|Antal alla lyckad registrering åtgärder (skapande frågor för uppdateringar och borttagningar). |
|Registration.Create|Registrering skapar operationer|Antal|Totalt|Antal allt skapande av registreringen har lyckats.|
|Registration.Update|Uppdateringsåtgärder för registrering|Antal|Totalt|Antal uppdateringar för alla registreringen har lyckats.|
|Registration.Get|Registrering-läsåtgärder|Antal|Totalt|Antal frågor för alla registreringen har lyckats.|
|Registration.delete|Registrering Delete-åtgärder|Antal|Totalt|Antal alla lyckad registrering borttagningar.|
|inkommande|Inkommande meddelanden|Antal|Totalt|Antal alla lyckade skicka API-anrop. |
|Incoming.Scheduled|Schemalagda Push-meddelanden som skickas|Antal|Totalt|Schemalagda Push-meddelanden avbröts|
|Incoming.Scheduled.Cancel|Schemalagda Push-meddelanden avbröts|Antal|Totalt|Schemalagda Push-meddelanden avbröts|
|Scheduled.Pending|Väntande schemalagda meddelanden|Antal|Totalt|Väntande schemalagda meddelanden|
|installation.all|Installationen hanteringsåtgärder|Antal|Totalt|Installationen hanteringsåtgärder|
|installation.Get|Hämta installationsåtgärder|Antal|Totalt|Hämta installationsåtgärder|
|installation.upsert|Skapa eller uppdatera installationsåtgärder|Antal|Totalt|Skapa eller uppdatera installationsåtgärder|
|installation.patch|Korrigering installationsåtgärder|Antal|Totalt|Korrigering installationsåtgärder|
|installation.delete|Ta bort installationsåtgärder|Antal|Totalt|Ta bort installationsåtgärder|
|Outgoing.allpns.Success|Lyckade meddelanden|Antal|Totalt|Antal alla lyckade meddelanden.|
|Outgoing.allpns.invalidpayload|Nyttolasten fel|Antal|Totalt|Antal push-meddelanden som misslyckades eftersom pns-systemet returnerade ett felaktigt nyttolast-fel.|
|Outgoing.allpns.pnserror|Externa aviseringar systemfel|Antal|Totalt|Antal push-meddelanden kunde inte utföras eftersom det inte att kommunicera med pns-systemet (utesluter autentiseringsproblem).|
|Outgoing.allpns.channelerror|Fel kanal|Antal|Totalt|Antal push-meddelanden som misslyckades eftersom kanalen var ogiltigt inte är associerade med appen rätt begränsas eller upphört att gälla.|
|Outgoing.allpns.badorexpiredchannel|Felaktig eller utgången kanal|Antal|Totalt|Antal push-meddelanden som kunde inte utföras eftersom kanalen/token/registrationId i registreringen har upphört att gälla eller är ogiltig.|
|Outgoing.wns.Success|WNS lyckade meddelanden|Antal|Totalt|Antal alla lyckade meddelanden.|
|Outgoing.wns.invalidcredentials|WNS auktorisering fel (ogiltig behörighet)|Antal|Totalt|Antal push-meddelanden som misslyckades eftersom pns-systemet accepterade inte de angivna autentiseringsuppgifterna eller autentiseringsuppgifterna som har blockerats. (Windows Live kan inte identifiera autentiseringsuppgifterna).|
|Outgoing.wns.badchannel|WNS felaktig kanal fel|Antal|Totalt|Antalet pushes som misslyckades eftersom ChannelURI i registreringen inte kändes igen (WNS status: 404 hittades inte).|
|Outgoing.wns.expiredchannel|WNS gått Kanalfel|Antal|Totalt|Antalet pushes som misslyckades eftersom ChannelURI har upphört att gälla (WNS status: 410 Gone).|
|Outgoing.wns.throttled|WNS begränsas meddelanden|Antal|Totalt|Antalet pushes som misslyckades eftersom WNS begränsning är den här appen (WNS status: 406 inte godtagbara).|
|Outgoing.wns.tokenproviderunreachable|WNS auktorisering fel (kan inte nås)|Antal|Totalt|Windows Live kan inte nås.|
|Outgoing.wns.invalidtoken|WNS auktorisering fel (Ogiltigt Token)|Antal|Totalt|Den token som angetts för WNS är inte giltigt (WNS status: 401 obehörig).|
|Outgoing.wns.wrongtoken|WNS auktorisering fel (felaktig Token)|Antal|Totalt|Den token som angetts för WNS är giltig men för ett annat program (WNS status: 403 nekad). Detta kan inträffa om ChannelURI i registreringen är associerad med en annan app. Kontrollera att klientappen är associerad med samma app vars autentiseringsuppgifter finns i meddelandehubben.|
|Outgoing.wns.invalidnotificationformat|WNS ogiltigt aviseringsresultat Format|Antal|Totalt|Formatet för meddelandet är ogiltigt (WNS status: 400). Observera att WNS inte avvisa alla ogiltig nyttolaster.|
|Outgoing.wns.invalidnotificationsize|WNS – ogiltig meddelandestorlek|Antal|Totalt|Meddelande-nyttolasten är för stor (WNS status: 413).|
|Outgoing.wns.channelthrottled|WNS-kanal begränsas|Antal|Totalt|Meddelandet ignorerades eftersom ChannelURI i registreringen begränsas (WNS svarshuvud: X-WNS-NotificationStatus:channelThrottled).|
|Outgoing.wns.channeldisconnected|WNS-kanal som kopplats från|Antal|Totalt|Meddelandet ignorerades eftersom ChannelURI i registreringen begränsas (WNS svarshuvud: X-WNS-DeviceConnectionStatus: frånkopplad).|
|Outgoing.wns.dropped|WNS bort meddelanden|Antal|Totalt|Meddelandet ignorerades eftersom ChannelURI i registreringen begränsas (X-WNS-NotificationStatus: avbrutna men inte X-WNS-DeviceConnectionStatus: frånkopplad).|
|Outgoing.wns.pnserror|WNS-fel|Antal|Totalt|Meddelande som inte levereras på grund av fel i kommunikationen med WNS.|
|Outgoing.wns.authenticationerror|WNS-autentiseringsfel|Antal|Totalt|Meddelande som inte levereras på grund av fel i kommunikationen med Windows Live ogiltiga autentiseringsuppgifter eller felaktig token.|
|Outgoing.apns.Success|APN lyckade meddelanden|Antal|Totalt|Antal alla lyckade meddelanden.|
|Outgoing.apns.invalidcredentials|APN auktorisering fel|Antal|Totalt|Antal push-meddelanden som misslyckades eftersom pns-systemet accepterade inte de angivna autentiseringsuppgifterna eller autentiseringsuppgifterna som har blockerats.|
|Outgoing.apns.badchannel|APN felaktig kanal fel|Antal|Totalt|Antalet pushes som misslyckades eftersom token är ogiltig (APN-statuskod: 8).|
|Outgoing.apns.expiredchannel|APN gått Kanalfel|Antal|Totalt|Antal token som upphävdes av APN feedback kanalen.|
|Outgoing.apns.invalidnotificationsize|APNS – ogiltig meddelandestorlek|Antal|Totalt|Antalet pushes som misslyckades eftersom nyttolasten är för stort (APN-statuskod: 7).|
|Outgoing.apns.pnserror|APN-fel|Antal|Totalt|Antal push-meddelanden som misslyckades på grund av fel i kommunikationen med APNS.|
|Outgoing.GCM.Success|Lyckad GCM-meddelanden|Antal|Totalt|Antal alla lyckade meddelanden.|
|Outgoing.GCM.invalidcredentials|GCM auktorisering fel (ogiltig behörighet)|Antal|Totalt|Antal push-meddelanden som misslyckades eftersom pns-systemet accepterade inte de angivna autentiseringsuppgifterna eller autentiseringsuppgifterna som har blockerats.|
|Outgoing.GCM.badchannel|GCM felaktig kanal fel|Antal|Totalt|Antalet pushes som misslyckades eftersom registrationId i registreringen inte kändes igen (GCM resultat: Ogiltig registrering).|
|Outgoing.GCM.expiredchannel|GCM gått Kanalfel|Antal|Totalt|Antalet pushes som misslyckades eftersom registrationId i registreringen har upphört att gälla (GCM resultat: NotRegistered).|
|Outgoing.GCM.throttled|GCM begränsas meddelanden|Antal|Totalt|Antalet pushes som misslyckades eftersom GCM begränsas den här appen (GCM-statuskod: 501-599 eller resultat: tillgänglig).|
|Outgoing.GCM.invalidnotificationformat|GCM ogiltigt aviseringsresultat Format|Antal|Totalt|Antalet pushes som misslyckades eftersom nyttolasten inte har formaterats korrekt (GCM resultat: InvalidDataKey eller InvalidTtl).|
|Outgoing.GCM.invalidnotificationsize|GCM – ogiltig meddelandestorlek|Antal|Totalt|Antalet pushes som misslyckades eftersom nyttolasten är för stort (GCM resultat: MessageTooBig).|
|Outgoing.GCM.wrongchannel|GCM fel kanal fel|Antal|Totalt|Antal push-meddelanden som misslyckades eftersom registrationId i registreringen inte är kopplad till den aktuella appen (GCM resultat: InvalidPackageName).|
|Outgoing.GCM.pnserror|GCM-fel|Antal|Totalt|Antal push-meddelanden som misslyckades på grund av fel i kommunikationen med GCM.|
|Outgoing.GCM.authenticationerror|GCM-autentiseringsfel|Antal|Totalt|Antal push-meddelanden som misslyckades eftersom pns-systemet inte accepterade den angivna autentiseringsuppgifter autentiseringsuppgifterna blockeras eller SenderId är inte korrekt konfigurerad i appen (GCM resultat: MismatchedSenderId).|
|Outgoing.mpns.Success|MPNS lyckade meddelanden|Antal|Totalt|Antal alla lyckade meddelanden.|
|Outgoing.mpns.invalidcredentials|Ogiltiga autentiseringsuppgifter för MPNS|Antal|Totalt|Antal push-meddelanden som misslyckades eftersom pns-systemet accepterade inte de angivna autentiseringsuppgifterna eller autentiseringsuppgifterna som har blockerats.|
|Outgoing.mpns.badchannel|MPNS felaktig kanal fel|Antal|Totalt|Antalet pushes som misslyckades eftersom ChannelURI i registreringen inte kändes igen (MPNS status: 404 hittades inte).|
|Outgoing.mpns.throttled|MPNS begränsas meddelanden|Antal|Totalt|Antalet pushes som misslyckades eftersom MPNS begränsning är den här appen (WNS MPNS: 406 inte godtagbara).|
|Outgoing.mpns.invalidnotificationformat|Ogiltigt meddelande-Format för MPNS|Antal|Totalt|Antal push-meddelanden som har misslyckats eftersom nyttolasten för meddelandet är för stort.|
|Outgoing.mpns.channeldisconnected|MPNS kanal frånkopplad|Antal|Totalt|Antalet pushes som misslyckades eftersom ChannelURI i registreringen kopplades (MPNS status: 412 hittades).|
|Outgoing.mpns.dropped|MPNS bort meddelanden|Antal|Totalt|Antal push-meddelanden som utelämnats av MPNS (MPNS svarshuvud: X NotificationStatus: QueueFull eller Suppressed).|
|Outgoing.mpns.pnserror|MPNS fel|Antal|Totalt|Antal push-meddelanden som misslyckades på grund av fel i kommunikationen med MPNS.|
|Outgoing.mpns.authenticationerror|MPNS autentiseringsfel|Antal|Totalt|Antal push-meddelanden som misslyckades eftersom pns-systemet accepterade inte de angivna autentiseringsuppgifterna eller autentiseringsuppgifterna som har blockerats.|
|notificationhub.pushes|Alla utgående meddelanden|Antal|Totalt|Alla utgående meddelanden för notification hub|
|Incoming.all.Requests|Alla inkommande förfrågningar|Antal|Totalt|Totalt antal inkommande begäranden för en meddelandehubb|
|Incoming.all.failedrequests|Alla inkommande förfrågningar som inte kunde slutföras|Antal|Totalt|Totalt antal misslyckade förfrågningar för en meddelandehubb|

## <a name="microsoftpowerbidedicatedcapacities"></a>Microsoft.PowerBIDedicated/capacities

|Mått|Mått visningsnamn|Enhet|Sammansättningstyp|Beskrivning|
|---|---|---|---|---|
|qpu_metric|QPU|Antal|Genomsnittlig|QPU. Intervallet 0-100 för S1, 0-200 för S2 och 0-400 för S4|
|memory_metric|Minne|Byte|Genomsnittlig|Minne. Intervallet 0-25 GB för S1, 0-50 GB för S2 och 0-100 GB för S4|
|TotalConnectionRequests|Totalt antal begäranden|Antal|Genomsnittlig|Totalt antal förfrågningar. Dessa är mottagna.|
|SuccessfullConnectionsPerSec|Lyckade anslutningar Per sekund|CountPerSecond|Genomsnittlig|Hastighet för lyckade anslutningen slutföras.|
|TotalConnectionFailures|Totalt antal anslutningsfel|Antal|Genomsnittlig|Totalt antal misslyckade anslutningsförsök.|
|CurrentUserSessions|Aktuella användarsessioner|Antal|Genomsnittlig|Aktuellt antal användarsessioner som har upprättats.|
|QueryPoolBusyThreads|Frågan poolen upptagen trådar|Antal|Genomsnittlig|Antal upptagen trådar i trådpoolen för frågan.|
|CommandPoolJobQueueLength|Kölängd för kommandot poolen jobb|Antal|Genomsnittlig|Antal jobb i kö i trådpoolen för kommandot.|
|ProcessingPoolJobQueueLength|Kölängd för bearbetning poolen jobb|Antal|Genomsnittlig|Antal icke-I/O-jobb i kö för bearbetningstrådpoolen.|
|CurrentConnections|Anslutning: Aktuella anslutningar|Antal|Genomsnittlig|Aktuellt antal klientanslutningar.|
|CleanerCurrentPrice|Minne: Tydligare aktuella pris|Antal|Genomsnittlig|Aktuella priset minne, $ byte/tidsvärdet, normaliserad till 1000.|
|CleanerMemoryShrinkable|Minne: Tydligare minne krympning|Byte|Genomsnittlig|Mängden minne i byte, föremål rensning av bakgrunden ett rengöringsband.|
|CleanerMemoryNonshrinkable|Minne: Tydligare minne nonshrinkable|Byte|Genomsnittlig|Mängden minne i byte som inte omfattas av rensning av bakgrunden ett rengöringsband.|
|MemoryUsage|Minne: Minnesanvändning|Byte|Genomsnittlig|Mängden minne på server-processen som används för att beräkna tydligare minnespris. Lika med prestandaräknaren Process\PrivateBytes plus storleken på minnesmappade data, ignoreras eventuella som mappats eller som har allokerats av xVelocity i minnet analytics motorn (VertiPaq) överskrider minnesgränsen för xVelocity-motorn.|
|MemoryLimitHard|Minne: Minnesgränsen hårddisken|Byte|Genomsnittlig|Hårda minnesgränsen, från konfigurationsfilen.|
|MemoryLimitHigh|Minne: Minne begränsa hög|Byte|Genomsnittlig|Hög minnesgränsen, från konfigurationsfilen.|
|MemoryLimitLow|Minne: Minne gränsen låg|Byte|Genomsnittlig|Minnesbrist gräns från konfigurationsfilen.|
|MemoryLimitVertiPaq|Minne: Minne gränsen VertiPaq|Byte|Genomsnittlig|I minnet gräns från konfigurationsfilen.|
|Kvot|Minne: kvot|Byte|Genomsnittlig|Aktuella minneskvoten, i byte. Minneskvoten kallas även en reservation för minne grant eller minne.|
|QuotaBlocked|: Minneskvoten blockeras|Antal|Genomsnittlig|Aktuellt antal begäranden om kvoten som blockeras tills andra minneskvoter frigörs.|
|VertiPaqNonpaged|Minne: VertiPaq växlingsbart systemminne|Byte|Genomsnittlig|Byte av minne låst i arbetsminnet för användning av motorn i minnet.|
|VertiPaqPaged|Minne: VertiPaq-växlat minne|Byte|Genomsnittlig|Antal byte växlingsbart minne som används för data i minnet.|
|RowsReadPerSec|Bearbetning: Läsa rader per sek|CountPerSecond|Genomsnittlig|Antal rader att läsa från alla relationsdatabaser.|
|RowsConvertedPerSec|Bearbetning: Rader konverteras per sek|CountPerSecond|Genomsnittlig|Antal rader konverteras under bearbetning.|
|RowsWrittenPerSec|Bearbetning: Rader som skrivs per sekund|CountPerSecond|Genomsnittlig|Antal rader skrivna under bearbetning.|
|CommandPoolBusyThreads|Trådar: Kommandot poolen upptagen trådar|Antal|Genomsnittlig|Antal upptagen trådar i trådpoolen för kommandot.|
|CommandPoolIdleThreads|Trådar: Kommandot poolen inaktiva trådar|Antal|Genomsnittlig|Antalet inaktiva trådar i trådpoolen för kommandot.|
|LongParsingBusyThreads|Trådar: Long parsning upptagen trådar|Antal|Genomsnittlig|Antal upptagen trådar i länge tolkning trådpoolen.|
|LongParsingIdleThreads|Trådar: Long parsning inaktiva trådar|Antal|Genomsnittlig|Antalet inaktiva trådar i länge tolkning trådpoolen.|
|LongParsingJobQueueLength|Trådar: Parsning länge jobbet Kölängd|Antal|Genomsnittlig|Antal jobb i kö för länge tolkning trådpoolen.|
|ProcessingPoolBusyIOJobThreads|Trådar: Bearbetning av poolen upptagen i/o-jobbet trådar|Antal|Genomsnittlig|Antal trådar som körs i/o-jobb i bearbetningstrådpoolen.|
|ProcessingPoolBusyNonIOThreads|Trådar: Bearbetning av poolen upptagen icke-I/O-trådar|Antal|Genomsnittlig|Antal trådar som icke-I/O-jobb som körs i bearbetningstrådpoolen.|
|ProcessingPoolIOJobQueueLength|Trådar: Bearbetning av poolen Kölängd för i/o-jobb|Antal|Genomsnittlig|Antal i/o-jobb i kö för bearbetningstrådpoolen.|
|ProcessingPoolIdleIOJobThreads|Trådar: Bearbetning av poolen inaktiv i/o-jobbet trådar|Antal|Genomsnittlig|Antal inaktiva trådar för i/o-jobb i bearbetningstrådpoolen.|
|ProcessingPoolIdleNonIOThreads|Trådar: Bearbetning av poolen inaktiv icke-I/O-trådar|Antal|Genomsnittlig|Antalet inaktiva trådar i bearbetningstrådpoolen som är dedikerad till icke-I/O-jobb.|
|QueryPoolIdleThreads|Trådar: Frågan poolen inaktiva trådar|Antal|Genomsnittlig|Antal inaktiva trådar för i/o-jobb i bearbetningstrådpoolen.|
|QueryPoolJobQueueLength|Trådar: Frågan poolen jobbet kön lengt|Antal|Genomsnittlig|Antal jobb i kö i trådpoolen för frågan.|
|ShortParsingBusyThreads|Trådar: Kort parsning upptagen trådar|Antal|Genomsnittlig|Antal upptagen trådar i kort tolkning trådpoolen.|
|ShortParsingIdleThreads|Trådar: Kort parsning inaktiva trådar|Antal|Genomsnittlig|Antalet inaktiva trådar i kort tolkning trådpoolen.|
|ShortParsingJobQueueLength|Trådar: Kort parsning jobbet Kölängd|Antal|Genomsnittlig|Antal jobb i kö för kort tolkning trådpoolen.|
|memory_thrashing_metric|Minne knattrar|Procent|Genomsnittlig|Genomsnittlig minne knattrar.|

## <a name="microsoftsearchsearchservices"></a>Microsoft.Search/searchServices

|Mått|Mått visningsnamn|Enhet|Sammansättningstyp|Beskrivning|
|---|---|---|---|---|
|SearchLatency|Sök svarstid|Sekunder|Genomsnittlig|Sök genomsnittlig svarstid för search-tjänsten|
|SearchQueriesPerSecond|Sökfrågor per sekund|CountPerSecond|Genomsnittlig|Sökfrågor per sekund för search-tjänsten|
|ThrottledSearchQueriesPercentage|Begränsad sökning frågor procent|Procent|Genomsnittlig|Procentandelen sökfrågor som har begränsats för söktjänsten|

## <a name="microsoftservicebusnamespaces"></a>Microsoft.ServiceBus/namespaces

|Mått|Mått visningsnamn|Enhet|Sammansättningstyp|Beskrivning|
|---|---|---|---|---|
|CPUXNS|CPU-användning per namnområde|Procent|Maximalt|Service bus premium namnområde CPU användning mått|
|WSXNS|Minnesstorleksanvändning per namnrymd|Procent|Maximalt|Service bus premium-namnområde minne användning mått|

## <a name="microsoftsqlserversdatabases"></a>Microsoft.Sql/servers/databases

|Mått|Mått visningsnamn|Enhet|Sammansättningstyp|Beskrivning|
|---|---|---|---|---|
|cpu_percent|CPU-procent|Procent|Genomsnittlig|CPU-procent|
|physical_data_read_percent|Data IO-procent|Procent|Genomsnittlig|Data IO-procent|
|log_write_percent|Loggen IO-procent|Procent|Genomsnittlig|Loggen IO-procent|
|dtu_consumption_percent|DTU-procent|Procent|Genomsnittlig|DTU-procent|
|Lagring|Totalt antal databasens storlek|Byte|Maximalt|Totalt antal databasens storlek|
|connection_successful|Lyckade anslutningar|Antal|Totalt|Lyckade anslutningar|
|connection_failed|Misslyckade anslutningar|Antal|Totalt|Misslyckade anslutningar|
|blocked_by_firewall|Blockeras av brandvägg|Antal|Totalt|Blockeras av brandvägg|
|deadlock|Deadlocks|Antal|Totalt|Deadlocks|
|storage_percent|Databasstorlek i procent|Procent|Maximalt|Databasstorlek i procent|
|xtp_storage_percent|Minnesintern OLTP lagring procent|Procent|Genomsnittlig|Minnesintern OLTP lagring procent|
|workers_percent|Procentsatsen för arbetare|Procent|Genomsnittlig|Procentsatsen för arbetare|
|sessions_percent|Sessioner procent|Procent|Genomsnittlig|Sessioner procent|
|dtu_limit|DTU gräns|Antal|Genomsnittlig|DTU gräns|
|dtu_used|DTU används|Antal|Genomsnittlig|DTU används|
|dwu_limit|DWU gräns|Antal|Maximalt|DWU gräns|
|dwu_consumption_percent|DWU-procent|Procent|Maximalt|DWU-procent|
|dwu_used|DWU används|Antal|Maximalt|DWU används|

## <a name="microsoftsqlserverselasticpools"></a>Microsoft.Sql/servers/elasticPools

|Mått|Mått visningsnamn|Enhet|Sammansättningstyp|Beskrivning|
|---|---|---|---|---|
|cpu_percent|CPU-procent|Procent|Genomsnittlig|CPU-procent|
|database_cpu_percent|CPU-procent|Procent|Genomsnittlig|CPU-procent|
|physical_data_read_percent|Data IO-procent|Procent|Genomsnittlig|Data IO-procent|
|database_physical_data_read_percent|Data IO-procent|Procent|Genomsnittlig|Data IO-procent|
|log_write_percent|Loggen IO-procent|Procent|Genomsnittlig|Loggen IO-procent|
|database_log_write_percent|Loggen IO-procent|Procent|Genomsnittlig|Loggen IO-procent|
|dtu_consumption_percent|DTU-procent|Procent|Genomsnittlig|DTU-procent|
|database_dtu_consumption_percent|DTU-procent|Procent|Genomsnittlig|DTU-procent|
|storage_percent|Lagringsprocent|Procent|Genomsnittlig|Lagringsprocent|
|workers_percent|Procentsatsen för arbetare|Procent|Genomsnittlig|Procentsatsen för arbetare|
|database_workers_percent|Procentsatsen för arbetare|Procent|Genomsnittlig|Procentsatsen för arbetare|
|sessions_percent|Sessioner procent|Procent|Genomsnittlig|Sessioner procent|
|database_sessions_percent|Sessioner procent|Procent|Genomsnittlig|Sessioner procent|
|eDTU_limit|eDTU gräns|Antal|Genomsnittlig|eDTU gräns|
|storage_limit|Lagringsgränsen|Byte|Genomsnittlig|Lagringsgränsen|
|eDTU_used|eDTU används|Antal|Genomsnittlig|eDTU används|
|storage_used|Använt lagringsutrymme|Byte|Genomsnittlig|Använt lagringsutrymme|
|database_storage_used|Använt lagringsutrymme|Byte|Genomsnittlig|Använt lagringsutrymme|
|xtp_storage_percent|Minnesintern OLTP lagring procent|Procent|Genomsnittlig|Minnesintern OLTP lagring procent|

## <a name="microsoftsqlservers"></a>Microsoft.Sql/servers

|Mått|Mått visningsnamn|Enhet|Sammansättningstyp|Beskrivning|
|---|---|---|---|---|
|dtu_consumption_percent|DTU-procent|Procent|Genomsnittlig|DTU-procent|
|database_dtu_consumption_percent|DTU-procent|Procent|Genomsnittlig|DTU-procent|
|storage_used|Använt lagringsutrymme|Byte|Genomsnittlig|Använt lagringsutrymme|
|database_storage_used|Använt lagringsutrymme|Byte|Genomsnittlig|Använt lagringsutrymme|

## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs

|Mått|Mått visningsnamn|Enhet|Sammansättningstyp|Beskrivning|
|---|---|---|---|---|
|ResourceUtilization|SU % användning|Procent|Maximalt|SU % användning|
|InputEvents|Inkommande händelser|Antal|Totalt|Inkommande händelser|
|InputEventBytes|Händelse-byte|Byte|Totalt|Händelse-byte|
|LateInputEvents|Inkommande händelser|Antal|Totalt|Inkommande händelser|
|OutputEvents|Utdatahändelserna|Antal|Totalt|Utdatahändelserna|
|ConversionErrors|Data Konverteringsfel|Antal|Totalt|Data Konverteringsfel|
|Fel|Körningsfel|Antal|Totalt|Körningsfel|
|DroppedOrAdjustedEvents|Oordnade händelser|Antal|Totalt|Oordnade händelser|
|AMLCalloutRequests|Funktionen begäranden|Antal|Totalt|Funktionen begäranden|
|AMLCalloutFailedRequests|Funktionsfel begäranden|Antal|Totalt|Funktionsfel begäranden|
|AMLCalloutInputEvents|Funktionshändelser|Antal|Totalt|Funktionshändelser|

## <a name="microsoftwebserverfarms"></a>Microsoft.Web/serverfarms

|Mått|Mått visningsnamn|Enhet|Sammansättningstyp|Beskrivning|
|---|---|---|---|---|
|CpuPercentage|CPU-procent|Procent|Genomsnittlig|CPU-procent|
|MemoryPercentage|Minnesprocent|Procent|Genomsnittlig|Minnesprocent|
|DiskQueueLength|Diskkölängd|Antal|Totalt|Diskkölängd|
|HttpQueueLength|HTTP-Kölängd|Antal|Totalt|HTTP-Kölängd|
|BytesReceived|Data i|Byte|Totalt|Data i|
|BytesSent|Ut data|Byte|Totalt|Ut data|

## <a name="microsoftwebsites-excluding-functions"></a>Microsoft.Web/sites (exklusive funktioner)

|Mått|Mått visningsnamn|Enhet|Sammansättningstyp|Beskrivning|
|---|---|---|---|---|
|CpuTime|CPU-tid|Sekunder|Totalt|CPU-tid|
|Begäranden|Begäranden|Antal|Totalt|Begäranden|
|BytesReceived|Data i|Byte|Totalt|Data i|
|BytesSent|Ut data|Byte|Totalt|Ut data|
|Http101|HTTP 101|Antal|Totalt|HTTP 101|
|Http2xx|HTTP-2xx|Antal|Totalt|HTTP-2xx|
|Http3xx|HTTP-3xx|Antal|Totalt|HTTP-3xx|
|Http401|HTTP 401|Antal|Totalt|HTTP 401|
|Http403|HTTP 403|Antal|Totalt|HTTP 403|
|Http404|HTTP 404|Antal|Totalt|HTTP 404|
|Http406|HTTP 406|Antal|Totalt|HTTP 406|
|Http4xx|HTTP-4xx|Antal|Totalt|HTTP-4xx|
|Http5xx|HTTP-fel|Antal|Totalt|HTTP-fel|
|MemoryWorkingSet|Arbetsminnet för minne|Byte|Genomsnittlig|Arbetsminnet för minne|
|AverageMemoryWorkingSet|Genomsnittlig minne arbetsminne|Byte|Genomsnittlig|Genomsnittlig minne arbetsminne|
|AverageResponseTime|Genomsnittlig svarstid|Sekunder|Genomsnittlig|Genomsnittlig svarstid|

## <a name="microsoftwebsites-functions"></a>Microsoft.Web/sites (funktioner)

|Mått|Mått visningsnamn|Enhet|Sammansättningstyp|Beskrivning|
|---|---|---|---|---|
|BytesReceived|Data i|Byte|Totalt|Data i|
|BytesSent|Ut data|Byte|Totalt|Ut data|
|Http5xx|HTTP-fel|Antal|Totalt|HTTP-fel|
|MemoryWorkingSet|Arbetsminnet för minne|Byte|Genomsnittlig|Arbetsminnet för minne|
|AverageMemoryWorkingSet|Genomsnittlig minne arbetsminne|Byte|Genomsnittlig|Genomsnittlig minne arbetsminne|
|FunctionExecutionUnits|Funktionen körning av enheter|Antal|Genomsnittlig|Funktionen körning av enheter|
|FunctionExecutionCount|Funktionen körningar|Antal|Genomsnittlig|Funktionen körningar|

## <a name="microsoftwebsitesslots"></a>Microsoft.Web/sites/slots

|Mått|Mått visningsnamn|Enhet|Sammansättningstyp|Beskrivning|
|---|---|---|---|---|
|CpuTime|CPU-tid|Sekunder|Totalt|CPU-tid|
|Begäranden|Begäranden|Antal|Totalt|Begäranden|
|BytesReceived|Data i|Byte|Totalt|Data i|
|BytesSent|Ut data|Byte|Totalt|Ut data|
|Http101|HTTP 101|Antal|Totalt|HTTP 101|
|Http2xx|HTTP-2xx|Antal|Totalt|HTTP-2xx|
|Http3xx|HTTP-3xx|Antal|Totalt|HTTP-3xx|
|Http401|HTTP 401|Antal|Totalt|HTTP 401|
|Http403|HTTP 403|Antal|Totalt|HTTP 403|
|Http404|HTTP 404|Antal|Totalt|HTTP 404|
|Http406|HTTP 406|Antal|Totalt|HTTP 406|
|Http4xx|HTTP-4xx|Antal|Totalt|HTTP-4xx|
|Http5xx|HTTP-fel|Antal|Totalt|HTTP-fel|
|MemoryWorkingSet|Arbetsminnet för minne|Byte|Genomsnittlig|Arbetsminnet för minne|
|AverageMemoryWorkingSet|Genomsnittlig minne arbetsminne|Byte|Genomsnittlig|Genomsnittlig minne arbetsminne|
|AverageResponseTime|Genomsnittlig svarstid|Sekunder|Genomsnittlig|Genomsnittlig svarstid|
|FunctionExecutionUnits|Funktionen körning av enheter|Antal|Genomsnittlig|Funktionen körning av enheter|
|FunctionExecutionCount|Funktionen körningar|Antal|Genomsnittlig|Funktionen körningar|

## <a name="next-steps"></a>Nästa steg
* [Läs mer om mätvärden i Azure-Monitor](monitoring-overview-metrics.md)
* [Skapa aviseringar på mått](insights-receive-alert-notifications.md)
* [Exportera mått till lagring, Event Hub eller logganalys](monitoring-overview-of-diagnostic-logs.md)
