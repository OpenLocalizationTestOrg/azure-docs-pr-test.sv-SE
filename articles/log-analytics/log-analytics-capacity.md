---
title: "aaaCapacity och lösning av prestanda i Azure Log Analytics | Microsoft Docs"
description: "Använd hello kapacitet och prestanda lösning i logganalys toohelp du förstår hello kapacitet för Hyper-V-servrar."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 51617a6f-ffdd-4ed2-8b74-1257149ce3d4
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: banders
ms.openlocfilehash: c47bb1e8bb9d4460b0241e89a616f3b356844b08
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="plan-hyper-v-virtual-machine-capacity-with-hello-capacity-and-performance-solution-preview"></a>Planera kapaciteten för Hyper-V-virtuella datorer med hello kapacitet och prestanda lösning (förhandsgranskning)

![Symbolen kapacitet och prestanda](./media/log-analytics-capacity/capacity-solution.png)

Du kan använda hello kapacitet och prestanda lösning i logganalys toohelp du förstår hello kapacitet för Hyper-V-servrar. hello lösningen ger insikter om din Hyper-V-miljö genom att visa hello sätt (CPU, minne och disk) hello värdar och hello virtuella datorer som körs på dessa Hyper-V-värdar. Mått har samlats in för CPU, minne och diskar på alla värdar och hello virtuella datorer som körs på dem..

hello-lösningen:

-   Visar med högsta och lägsta CPU och minne användning
-   Visar virtuella datorer med högsta och lägsta CPU och minne användning
-   Visar virtuella datorer med högsta och lägsta IOPS och genomströmning användning
-   Visar vilka virtuella datorer som körs på vilka värdar
-   Visar hello översta diskar med hög genomströmning och IOPS, och svarstid i kluster delade klustervolymer
- Gör att du toocustomize och filter baserat på grupper

> [!NOTE]
> hello tidigare version av hello kapacitet och prestanda-lösning som kallas kapacitet Management krävs både System Center Operations Manager och System Center Virtual Machine Manager. Den här uppdaterade lösningen har inte dessa beroenden.


## <a name="connected-sources"></a>Anslutna källor

hello i den följande tabellen beskrivs hello anslutna källor som stöds av den här lösningen.

| Ansluten källa | Support | Beskrivning |
|---|---|---|
| [Windows-agenter](log-analytics-windows-agents.md) | Ja | hello lösningen samlar in information om kapacitet och prestanda från Windows-agenter. |
| [Linux-agenter](log-analytics-linux-agents.md) | Nej    | hello lösningen samlar inte in information om kapacitet och prestanda från direkt Linux-agenter.|
| [SCOM-hanteringsgrupp](log-analytics-om-agents.md) | Ja |hello lösningen samlar in data för kapacitet och prestanda från agenter i en ansluten SCOM-hanteringsgrupp. En direkt anslutning från hello SCOM-agent tooOMS krävs inte. Data vidarebefordras från hello management group toohello OMS-databasen.|
| [Azure Storage-konto](log-analytics-azure-storage.md) | Nej | Azure storage innehåller inte data kapacitet och prestanda.|

## <a name="prerequisites"></a>Krav

- Windows- eller Operations Manager-agenter måste installeras på Windows Server 2012 eller högre Hyper-V-värdar, inte virtuella datorer.


## <a name="configuration"></a>Konfiguration

Utför följande steg tooadd hello Kapacitets- och lösningen tooyour arbetsytan hello.

- Lägg till hello kapacitet och prestanda lösning tooyour OMS-arbetsyta med hjälp av hello process beskrivs i [lägga till logganalys lösningar från hello lösningar galleriet](log-analytics-add-solutions.md).

## <a name="management-packs"></a>Hanteringspaket

Om din SCOM-hanteringsgrupp är anslutna tooyour OMS-arbetsyta, sedan installeras hello följande hanteringspaket i SCOM när du lägger till den här lösningen. Det krävs ingen konfigurering eller underhåll av dessa hanteringspaket.

- Microsoft.IntelligencePacks.CapacityPerformance

hello 1201 händelse liknar:


```
New Management Pack with id:"Microsoft.IntelligencePacks.CapacityPerformance", version:"1.10.3190.0" received.
```

När hello Kapacitets- och lösningen uppdateras ändras hello versionsnumret.

Mer information om hur lösningen hanteringspaketen är uppdaterade finns [ansluta Operations Manager tooLog Analytics](log-analytics-om-agents.md).

## <a name="using-hello-solution"></a>Med hello-lösning

När du lägger till hello Kapacitets- och lösningen tooyour arbetsytan läggs toohello översikt över instrumentpanelen i hello kapacitet och prestanda. Den här panelen visar antalet hello antalet aktiva Hyper-V-värdar och hello antal aktiva virtuella datorer som har övervakas för hello angiven tidsperiod.

![Panelen kapacitet och prestanda](./media/log-analytics-capacity/capacity-tile.png)


### <a name="review-utilization"></a>Granska användning

Klicka på hello kapacitet och prestanda panelen tooopen hello kapacitet och prestanda för instrumentpanelen. hello instrumentpanelen innehåller hello kolumner i hello i den följande tabellen. Varje kolumnen visar tooten artiklar som matchar att kolumnens sökvillkor för hello angivna scope och ett tidsintervall. Du kan köra en sökning i loggen som returnerar alla poster genom att klicka på **se alla** längst ned hello hello kolumnen eller genom att klicka på kolumnrubriken hello.

- **Värdar**
    - **Värd för processoranvändningen** visar en grafisk trend över hello CPU-utnyttjande på värddatorer och en lista över värdar som baserats på hello tidsperiod. Hovra över hello diagram tooview detaljer för en specifik tidpunkt. Klicka på hello diagram tooview mer information finns i loggen Sök. Klicka på varje värd namn tooopen loggen Sök och visa information om prestandaräknare CPU för värdbaserade virtuella datorer.
    - **Värd för minnesanvändning** visar en grafisk trend över hello minnesanvändning värddatorer och en lista över värdar som baserats på hello tidsperiod. Hovra över hello diagram tooview detaljer för en specifik tidpunkt. Klicka på hello diagram tooview mer information finns i loggen Sök. Klicka på alla värden namnet tooopen loggen Sök- och minne räknaren information för värdbaserade virtuella datorer.
- **Virtual Machines**
    - **VM processoranvändningen** visar en grafisk trend över hello CPU-användning av virtuella datorer och en lista över virtuella datorer baserat på hello tidsperiod. Hovra över hello diagram tooview detaljer för en specifik tidpunkt för hello uppifrån 3 virtuella datorer. Klicka på hello diagram tooview mer information finns i loggen Sök. Klicka på alla VM namn tooopen loggen Sök och visa samman CPU information om prestandaräknare för hello VM.
    - **VM minnesanvändning** visar en grafisk trend över hello minnesanvändning för virtuella datorer och en lista över virtuella datorer baserat på hello tidsperiod. Hovra över hello diagram tooview detaljer för en specifik tidpunkt för hello uppifrån 3 virtuella datorer. Klicka på hello diagram tooview mer information finns i loggen Sök. På alla VM namn tooopen loggen Sök och visa information om prestandaräknare aggregerade minne för hello VM.
    - **VM totala Disk-IOPS** visar en grafisk trend över hello totala IOPS för virtuella datorer och en lista över virtuella datorer med hello IOPS för varje disks hello tidsperiod. Hovra över hello diagram tooview detaljer för en specifik tidpunkt för hello uppifrån 3 virtuella datorer. Klicka på hello diagram tooview mer information finns i loggen Sök. Klicka på alla VM namn tooopen loggen Sök- och aggregerade disk IOPS prestandaräknaren information för hello VM.
    - **VM totala Diskgenomflödet** visar en grafisk trend över hello totala diskgenomflödet för virtuella datorer och en lista över virtuella datorer med hello totala genomflödet för varje, baserat på hello tidsperiod. Hovra över hello diagram tooview detaljer för en specifik tidpunkt för hello uppifrån 3 virtuella datorer. Klicka på hello diagram tooview mer information finns i loggen Sök. Klicka på alla VM namn tooopen loggen Sök och visa sammanställda totala genomflödet räknaren diskinformation för hello VM.
- **Klusterdelade volymer**
    - **Totalt antal genomströmning** visar hello summan av både läsning och skrivning på klusterdelade volymer.
    - **Totalt antal IOPS** visar hello summan av i/o-åtgärder per sekund på klusterdelade volymer.
    - **Total svarstid** visar hello totala svarstiden på klusterdelade volymer.
- **Värd för densitet** hello övre panelen visar hello totala antalet värdar och virtuella datorer tillgängliga toohello lösning. Klicka på hello övre panelen tooview ytterligare information i loggen sökningen. Visar också alla värdar och hello antalet virtuella datorer som är värd för. Klicka på en värd toodrill hello VM resultaten i en sökning i loggen.


![instrumentpanelen värdar bladet](./media/log-analytics-capacity/dashboard-hosts.png)

![instrumentpanelen bladet för virtuella datorer](./media/log-analytics-capacity/dashboard-vms.png)


### <a name="evaluate-performance"></a>Utvärdera prestanda

Produktion datormiljöer skiljer sig avsevärt från en organisation tooanother. Dessutom kapacitet och prestanda för arbetsbelastningar kan beror på hur dina virtuella datorer som körs, och vad du normalt. Specifika procedurer toohelp du mäta prestanda skulle förmodligen inte tooyour miljö. Det innebär att flera generaliserad vägledning är bättre passar toohelp. Microsoft publicerar en mängd av vägledning artiklar toohelp du mäta prestanda.

toosummarize, hello lösningen samlar in Kapacitets- och data från olika källor, till exempel prestandaräknare. Kapacitets- och data som visas i olika underlag i hello lösning och jämför dina resultat toothose på hello [mäta prestanda på Hyper-V](https://msdn.microsoft.com/library/cc768535.aspx) artikel. Även om hello artikeln publicerades en tid sedan är hello mått, överväganden och riktlinjer fortfarande giltiga. hello artikeln innehåller länkar tooother användbara resurser.


## <a name="sample-log-searches"></a>Exempel på loggsökningar

hello följande tabell innehåller exempel loggen söker efter Kapacitets- och data som samlas in och beräknas av den här lösningen.

| Fråga | Beskrivning |
|---|---|
| Alla värden minneskonfigurationer | <code>Type=Perf ObjectName="Capacity and Performance" CounterName="Host Assigned Memory MB" &#124; measure avg(CounterValue) as MB by InstanceName</code> |
| Alla konfigurationer av Virtuellt minne | <code>Type=Perf ObjectName="Capacity and Performance" CounterName="VM Assigned Memory MB" &#124; measure avg(CounterValue) as MB by InstanceName</code> |
| Uppdelning av totala Disk-IOPS över alla virtuella datorer | <code>Type=Perf ObjectName="Capacity and Performance" (CounterName="VHD Reads/s" OR CounterName="VHD Writes/s") &#124; top 2500 &#124; measure avg(CounterValue) by CounterName, InstanceName interval 1HOUR</code> |
| Uppdelning av totala Diskgenomflödet över alla virtuella datorer | <code>Type=Perf ObjectName="Capacity and Performance" (CounterName="VHD Read MB/s" OR CounterName="VHD Write MB/s") &#124; top 2500 &#124; measure avg(CounterValue) by CounterName, InstanceName interval 1HOUR</code> |
| Uppdelning av IOPS totalt över alla CSV: er | <code>Type=Perf ObjectName="Capacity and Performance" (CounterName="CSV Reads/s" OR CounterName="CSV Writes/s") &#124; top 2500 &#124; measure avg(CounterValue) by CounterName, InstanceName interval 1HOUR</code> |
| Uppdelning av totala genomflödet över alla CSV: er | <code>Type=Perf ObjectName="Capacity and Performance" (CounterName="CSV Read MB/s" OR CounterName="CSV Write MB/s") &#124; top 2500 &#124; measure avg(CounterValue) by CounterName, InstanceName interval 1HOUR</code> |
| Uppdelning av totala svarstiden i alla CSV: er | <code> Type=Perf ObjectName="Capacity and Performance" (CounterName="CSV Read Latency" OR CounterName="CSV Write Latency") &#124; top 2500 &#124; measure avg(CounterValue) by CounterName, InstanceName interval 1HOUR</code> |

>[!NOTE]
> Om ditt arbetsområde har uppgraderade toohello [nya Log Analytics-frågespråket](log-analytics-log-search-upgrade.md), hello senare frågorna skulle ändra toohello följande.

> | Fråga | Beskrivning |
|:--- |:--- |
| Alla värden minneskonfigurationer | Perf &#124; där ObjectName == ”kapacitet och prestanda” och CounterName == ”värd tilldelade minnes-MB” &#124; Sammanfatta MB = avg(CounterValue) av instansnamn |
| Alla konfigurationer av Virtuellt minne | Perf &#124; där ObjectName == ”kapacitet och prestanda” och CounterName == ”VM tilldelade minnes-MB” &#124; Sammanfatta MB = avg(CounterValue) av instansnamn |
| Uppdelning av totala Disk-IOPS över alla virtuella datorer | Perf &#124; där ObjectName == ”kapacitet och prestanda” och (CounterName == ”VHD Diskläsningar/s- eller CounterName ==” VHD skrivningar/s ”) &#124; Sammanfatta AggregatedValue = avg(CounterValue) av bin (TimeGenerated 1 tim), CounterName, InstanceName |
| Uppdelning av totala Diskgenomflödet över alla virtuella datorer | Perf &#124; där ObjectName == ”kapacitet och prestanda” och (CounterName == ”VHD läsa MB/s” eller CounterName == ”VHD Write MB/s”) &#124; Sammanfatta AggregatedValue = avg(CounterValue) av bin (TimeGenerated 1 tim), CounterName, InstanceName |
| Uppdelning av IOPS totalt över alla CSV: er | Perf &#124; där ObjectName == ”kapacitet och prestanda” och (CounterName == ”CSV Diskläsningar/s- eller CounterName ==” CSV skrivningar/s ”) &#124; Sammanfatta AggregatedValue = avg(CounterValue) av bin (TimeGenerated 1 tim), CounterName, InstanceName |
| Uppdelning av totala genomflödet över alla CSV: er | Perf &#124; där ObjectName == ”kapacitet och prestanda” och (CounterName == ”CSV Diskläsningar/s- eller CounterName ==” CSV skrivningar/s ”) &#124; Sammanfatta AggregatedValue = avg(CounterValue) av bin (TimeGenerated 1 tim), CounterName, InstanceName |
| Uppdelning av totala svarstiden i alla CSV: er | Perf &#124; där ObjectName == ”kapacitet och prestanda” och (CounterName == ”CSV Läs latens” eller CounterName == ”CSV Write latens”) &#124; Sammanfatta AggregatedValue = avg(CounterValue) av bin (TimeGenerated 1 tim), CounterName, InstanceName |


## <a name="next-steps"></a>Nästa steg
* Använd [logga sökningar i logganalys](log-analytics-log-searches.md) tooview detaljerade Kapacitets- och data.
