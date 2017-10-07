---
title: "aaaCollect och analysera prestandaräknare i Azure Log Analytics | Microsoft Docs"
description: "Prestandaräknarna samlas in av logganalys tooanalyze prestanda på Windows och Linux-agenter.  Den här artikeln beskriver hur tooconfigure insamling av prestanda prestandaräknare för både Windows och Linux-agenter, information om de lagras i hello OMS-databas och hur tooanalyze dem i hello OMS-portalen."
services: log-analytics
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: 20e145e4-2ace-4cd9-b252-71fb4f94099e
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/12/2017
ms.author: magoedte
ms.openlocfilehash: 30146fecf8db1d8851b89fdb970f757bbb24abf1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="windows-and-linux-performance-data-sources-in-log-analytics"></a>Windows- och Linux prestanda datakällor i logganalys
Prestandaräknare i Windows och Linux ger kunskaper om hello prestanda för maskinvarukomponenter, operativsystem och program.  Logganalys kan samla in prestandaräknare med återkommande intervall för analys i nära realtid (NRT) i tillägg tooaggregating prestandadata för längre sikt analys och rapportering.

![Prestandaräknare](media/log-analytics-data-sources-performance-counters/overview.png)

## <a name="configuring-performance-counters"></a>Konfigurera prestandaräknare
Konfigurera prestandaräknare i hello OMS-portalen från hello [Data-menyn i logganalys-inställningar](log-analytics-data-sources.md#configuring-data-sources).

När du först konfigurera Windows eller Linux prestandaräknare för en ny OMS-arbetsyta du får skapa hello alternativet tooquickly flera vanliga räknare.  De listas med en kryssruta nästa tooeach.  Se till att alla räknare du vill tooinitially skapar är markerade och klickar sedan på **hello Lägg till valda prestandaräknare**.

Du kan välja en specifik instans för varje prestandaräknare för Windows-prestandaräknare. Hello instans av varje räknare som du väljer gäller för Linux prestandaräknare tooall underordnade räknare för hello överordnade räknaren. hello visar följande tabell hello vanliga instanser tillgängliga tooboth Linux och Windows prestandaräknare.

| Instansnamn | Beskrivning |
| --- | --- |
| \_Totalt |Summan av alla hello-instanser |
| \* |Alla instanser |
| (/ &#124; / var) |Matchar instanser med namnet: / eller /var |

### <a name="windows-performance-counters"></a>Windows-prestandaräknare

![Konfigurera Windows-prestandaräknare](media/log-analytics-data-sources-performance-counters/configure-windows.png)

Följ den här proceduren tooadd toocollect för en ny Windows-prestanda i räknaren.

1. Hello-typnamn för hello räknaren hello textrutan hello format *objekt (förekomst) \counter*.  När du börjar skriva visas med en matchande lista över vanliga räknare.  Du kan välja en räknare från hello listan eller Skriv en egen.  Du kan också returnera alla instanser för räknaren genom att ange *objekt räknare*.  

    När SQL Server-prestandaräknare har samlats in från namngivna instanser, alla namngivna instansen räknare starta med *MSSQL$* och följt av hello namn hello-instans.  Ange till exempel toocollect hello loggen Cache träffar förhållandet räknare för alla databaser från hello databasen prestandaobjektet för namngiven SQL-instansen INST2, `MSSQL$INST2:Databases(*)\Log Cache Hit Ratio`.

2. Klicka på  **+**  eller tryck på **RETUR** tooadd hello toohello listan med räknare.
3. När du lägger till en räknare används hello standard 10 sekunder för dess **provintervallet**.  Du kan ändra den här tooa högre värde av upp too1800 sekunder (30 minuter) om du vill tooreduce hello lagringskrav hello samlas in prestandadata.
4. När du är klar att lägga till räknare klickar du på hello **spara** knappen hello överst i hello skärmen toosave hello konfiguration.

### <a name="linux-performance-counters"></a>Linux-prestandaräknare

![Konfigurera Linux-prestandaräknare](media/log-analytics-data-sources-performance-counters/configure-linux.png)

Följ den här proceduren tooadd toocollect för en ny Linux-prestanda i räknaren.

1. Som standard är alla konfigurationsändringar automatiskt pushas tooall agenter.  Linux-agenter skickas en konfigurationsfil toohello Fluentd datainsamlaren.  Om du inte vill toomodify den här filen manuellt på varje Linux-agent, avmarkerar du kryssrutan hello *tillämpa konfigurationen toomy Linux-datorerna nedan* och följ hello vägledning nedan.
2. Hello-typnamn för hello räknaren hello textrutan hello format *objekt (förekomst) \counter*.  När du börjar skriva visas med en matchande lista över vanliga räknare.  Du kan välja en räknare från hello listan eller Skriv en egen.  
3. Klicka på  **+**  eller tryck på **RETUR** tooadd hello toohello listan med räknare för andra räknare hello-objektet.
4. Alla räknare för användning av ett objekt hello samma **provintervallet**.  hello standardvärdet är 10 sekunder.  Du kan ändra tooa högre värdet för in too1800 sekunder (30 minuter) om du vill tooreduce hello lagringskrav hello samlas in prestandadata.
5. När du är klar att lägga till räknare klickar du på hello **spara** knappen hello överst i hello skärmen toosave hello konfiguration.

#### <a name="configure-linux-performance-counters-in-configuration-file"></a>Konfigurera Linux prestandaräknare i konfigurationsfilen
I stället för att konfigurera Linux prestandaräknare med hello OMS-portalen har hello möjlighet att redigera konfigurationsfiler på hello Linux-agenten.  Prestanda mått toocollect styrs av hello konfigurationen i **/etc/opt/microsoft/omsagent/\<arbetsyte-id\>/conf/omsagent.conf**.

Varje objekt eller kategori av prestanda mått toocollect ska vara definierat i hello konfigurationsfilen som en enda `<source>` element. hello syntax följer hello mönster nedan.

    <source>
      type oms_omi  
      object_name "Processor"
      instance_regex ".*"
      counter_name_regex ".*"
      interval 30s
    </source>


i hello i den följande tabellen beskrivs hello parametrar i det här elementet.

| Parametrar | Beskrivning |
|:--|:--|
| objektet\_namn | Objektnamn för hello samling. |
| instansen\_regex |  En *reguljärt uttryck* definierar vilka instanser toocollect. Hej värde: `.*` anger alla instanser. toocollect processor mätvärden för endast hello \_totala instans kan du ange `_Total`. toocollect Processmätvärden för hello endast crond eller sshd instanser, kan du ange: ' (crond\|sshd)'. |
| räknaren\_namn\_regex | En *reguljärt uttryck* definierar vilka räknare (för hello) toocollect. toocollect alla räknare för hello, ange: `.*`. toocollect växla endast utrymme räknare för hello minnesobjekt, t.ex, kan du ange:`.+Swap.+` |
| interval | Frekvens vid vilken hello objektets räknare samlas in. |


hello följande tabell visas hello objekt och räknare som du kan ange i hello konfigurationsfil.  Det finns ytterligare räknare för vissa program som beskrivs i [samla in prestandaräknare för Linux-program i logganalys](log-analytics-data-sources-linux-applications.md).

| Objektnamn | Räknarens namn |
|:--|:--|
| Logisk Disk | % Ledigai |
| Logisk Disk | Ledigt utrymme i procent |
| Logisk Disk | % Användai |
| Logisk Disk | Använt utrymme i procent |
| Logisk Disk | Disk-lästa byte/sek |
| Logisk Disk | Diskläsningar/sek |
| Logisk Disk | Disköverföringar/sek |
| Logisk Disk | Disk-skrivna byte/s |
| Logisk Disk | Diskskrivningar/sek |
| Logisk Disk | Ledigt utrymme i MB |
| Logisk Disk | Logisk Disk byte/sek |
| Minne | Tillgängligt minne i procent |
| Minne | Tillgängligt växlingsutrymme i procent |
| Minne | Använt minne i procent |
| Minne | Använt växlingsutrymme i procent |
| Minne | Tillgängligt minne i megabyte |
| Minne | Tillgängliga megabyte växlingsutrymme |
| Minne | Sidläsningar/sek |
| Minne | Sidskrivningar/sek |
| Minne | Sidor/sek |
| Minne | Använt växlingsutrymme i megabyte |
| Minne | Använt minne MB |
| Nätverk | Sammanlagt antal överförda byte |
| Nätverk | Totalt antal mottagna byte |
| Nätverk | Totalt antal byte |
| Nätverk | Totalt antal skickade paket |
| Nätverk | Totalt antal mottagna paket |
| Nätverk | Totalt antal Rx fel |
| Nätverk | Totalt antal Tx-fel |
| Nätverk | Totalt antal kollisioner |
| Fysisk Disk | Genomsn. Disk sek/läsning |
| Fysisk Disk | Genomsn. Disk sek/disköverföring |
| Fysisk Disk | Genomsn. Disk sek/skrivning |
| Fysisk Disk | Fysisk Disk byte/sek |
| Processen | PCT privilegierad tid |
| Processen | PCT användartid |
| Processen | Använt minne Kbyte |
| Processen | Virtuella delat minne |
| Processor | % DPC-tid |
| Processor | Inaktivitetstid i procent |
| Processor | Avbrottstid i procent |
| Processor | %-I/o-väntetid |
| Processor | Nice Time i procent |
| Processor | Privilegierad tid i procent |
| Processor | % Processortid |
| Processor | Användartid i procent |
| System | Fysiskt minne |
| System | Ledigt utrymme i växlingsfiler |
| System | Ledigt virtuellt minne |
| System | Processer |
| System | Storlek lagrad i växlingsfiler |
| System | Drifttid |
| System | Användare |


Följande är hello standardkonfigurationen för prestandavärden.

    <source>
      type oms_omi
      object_name "Physical Disk"
      instance_regex ".*"
      counter_name_regex ".*"
      interval 5m
    </source>

    <source>
      type oms_omi
      object_name "Logical Disk"
      instance_regex ".*
      counter_name_regex ".*"
      interval 5m
    </source>

    <source>
      type oms_omi
      object_name "Processor"
      instance_regex ".*
      counter_name_regex ".*"
      interval 30s
    </source>

    <source>
      type oms_omi
      object_name "Memory"
      instance_regex ".*"
      counter_name_regex ".*"
      interval 30s
    </source>

## <a name="data-collection"></a>Datainsamling
Logganalys samlar in alla angivna prestandaräknare på sina angivna provintervallet i alla agenter som har som räknaren installerad.  hello informationen sammanställs inte och hello rådata är tillgänglig i alla loggen Sök vyer för hello varaktighet som anges av din OMS-prenumeration.

## <a name="performance-record-properties"></a>Prestanda post egenskaper
Prestanda poster har en typ av **Perf** och ha hello egenskaper i hello i den följande tabellen.

| Egenskap | Beskrivning |
|:--- |:--- |
| Dator |Dator som hello händelse som samlats in från. |
| CounterName |Namnet på hello prestandaräknaren |
| Räknarsökväg |Fullständig sökväg till hello räknare i form av hello \\ \\ \<dator >\\objekt(instans)\\räknaren. |
| CounterValue |Numeriskt värde för hello räknaren. |
| Instansnamn |Namnet på instansen för hello-händelse.  Tomt om ingen instans. |
| Objektnamn |Namnet på hello prestandaobjekt |
| SourceSystem |Typ av agenten hello data samlades in från. <br><br>Ansluta OpsManager – Windows-agenten, antingen direkt eller SCOM <br> Linux – alla Linux-agenter  <br> AzureStorage – Azure-diagnostik |
| TimeGenerated |Datum och tid hello data var prov. |

## <a name="sizing-estimates"></a>Beräknar storlek
 En grov uppskattning för samling för räknaren vid intervall om 10 sekunder är cirka 1 MB per dag per instans.  Du kan beräkna hello lagringskraven för räknaren med hello följande formel.

    1 MB x (number of counters) x (number of agents) x (number of instances)

## <a name="log-searches-with-performance-records"></a>Loggen sökningar med uppgifter
hello innehåller följande tabell olika exempel på loggen sökningar som hämtar uppgifter.

| Fråga | Beskrivning |
|:--- |:--- |
| Typ = Perf |Alla prestandadata |
| Typ = Perf datorn = ”den här datorn” |Alla data från en viss dator |
| Typ = Perf CounterName = ”den Aktuell diskkölängd” |Alla prestandadata för räknaren |
| Typ = Perf (ObjectName = Processor) CounterName = ”% processortid” InstanceName = _Total &#124; måttet Avg(Average) som AVGCPU per dator |Genomsnittlig CPU-användning på alla datorer |
| Typ = Perf (CounterName = ”% processortid”) &#124;  måttet max(Max) per dator |Högsta CPU-användning på alla datorer |
| Typ = Perf ObjectName = LogicalDisk CounterName = ”aktuella disken Kölängd” datorn = ”MyComputerName” &#124; måttet Avg(Average) av instansnamn |Genomsnittlig Kölängd för aktuella Disk i alla hello instanser av en viss dator |
| Typ = Perf CounterName = ”DiskTransfers/sek” &#124; måttet percentile95(Average) per dator |95: e percentilen för disköverföringar/sek på alla datorer |
| Typ = Perf CounterName = ”% processortid” InstanceName = ”_Total” &#124; Mät avg(CounterValue) datorn intervall 1 timme |Varje timme medelvärdet av CPU-användning på alla datorer |
| Typ = Perf datorn = ”den här datorn” CounterName = % * InstanceName = _Total &#124; måttet percentile70(CounterValue) CounterName intervall 1 timme |Varje timme 70: e percentilen för varje procent räknaren % för en viss dator |
| Typ = Perf CounterName = ”% processortid” InstanceName = ”_Total” (dator = ”den här datorn”) &#124; mäter min(CounterValue), avg(CounterValue), percentile75(CounterValue), max(CounterValue) datorn intervall 1 timme |Varje timme medelvärde, lägsta, högsta och 75 percentil CPU-användning för en specifik dator |
| Typ = Perf ObjectName = ”MSSQL$ INST2: databaser” InstanceName = master | Alla prestandadata från hello databasprestanda objekt för hello master-databasen från hello namngivna SQL Server-instans INST2.  

>[!NOTE]
> Om ditt arbetsområde har uppgraderade toohello [nya Log Analytics-frågespråket](log-analytics-log-search-upgrade.md), hello senare frågorna skulle ändra toohello följande.

> | Fråga | Beskrivning |
|:--- |:--- |
| Perf |Alla prestandadata |
| Perf &#124; Om datorn == ”den här datorn” |Alla data från en viss dator |
| Perf &#124; Om CounterName == ”den Aktuell diskkölängd” |Alla prestandadata för räknaren |
| Perf &#124; där ObjectName == ”-Processor” och CounterName == ”% processortid” och InstanceName == ”_Total” &#124; Sammanfatta AVGCPU = avg(Average) per dator |Genomsnittlig CPU-användning på alla datorer |
| Perf &#124; Om CounterName == ”% processortid” &#124; Sammanfatta AggregatedValue = max(Max) per dator |Högsta CPU-användning på alla datorer |
| Perf &#124; där ObjectName == ”logisk disk” och CounterName == ”den Aktuell diskkölängd” och datorn == ”MyComputerName” &#124; Sammanfatta AggregatedValue = avg(Average) av instansnamn |Genomsnittlig Kölängd för aktuella Disk i alla hello instanser av en viss dator |
| Perf &#124; Om CounterName == ”DiskTransfers/sek” &#124; Sammanfatta AggregatedValue = percentil (medel, 95) per dator |95: e percentilen för disköverföringar/sek på alla datorer |
| Perf &#124; Om CounterName == ”% processortid” och InstanceName == ”_Total” &#124; Sammanfatta AggregatedValue = avg(CounterValue) av bin (TimeGenerated 1 tim), datorn |Varje timme medelvärdet av CPU-användning på alla datorer |
| Perf &#124; Om datorn == ”den här datorn” och CounterName startswith_cs ”%” och InstanceName == ”_Total” &#124; Sammanfatta AggregatedValue = percentil (CounterValue 70) som bin (TimeGenerated 1 tim), CounterName | Varje timme 70: e percentilen för varje procent räknaren % för en viss dator |
| Perf &#124; Om CounterName == ”% processortid” och InstanceName == ”_Total” och datorn == ”den här datorn” &#124; Sammanfatta [”min(CounterValue)”] = min(CounterValue), [”avg(CounterValue)”] = avg(CounterValue), [”percentile75(CounterValue)”] = percentil (CounterValue 75), [”max(CounterValue)”] = max(CounterValue) av bin (TimeGenerated 1 tim), datorn |Varje timme medelvärde, lägsta, högsta och 75 percentil CPU-användning för en specifik dator |
| Perf &#124; där ObjectName == ”MSSQL$ INST2: databaser” och InstanceName == ”master” | Alla prestandadata från hello databasprestanda objekt för hello master-databasen från hello namngivna SQL Server-instans INST2.  

## <a name="viewing-performance-data"></a>Visar prestandadata
När du kör en logg sökning efter prestandadata hello **listan** visas som standard.  tooview hello data i grafisk form Klicka **mått**.  En detaljerad grafisk vy klickar du på hello  **+**  nästa tooa räknaren.  

![Mått Visa dolda](media/log-analytics-data-sources-performance-counters/metricscollapsed.png)

tooaggregate prestandadata i en logg-sökning, se [på begäran mått aggregering och visualisering i OMS](http://blogs.technet.microsoft.com/msoms/2016/02/26/on-demand-metric-aggregation-and-visualization-in-oms/).


## <a name="next-steps"></a>Nästa steg
* [Samla in prestandaräknare från Linux-program](log-analytics-data-sources-linux-applications.md) inklusive MySQL och Apache HTTP-servern.
* Lär dig mer om [logga sökningar](log-analytics-log-searches.md) tooanalyze hello data som samlas in från datakällor och lösningar.  
* Exportera insamlade data för[Power BI](log-analytics-powerbi.md) för ytterligare grafik och analys.
