---
title: "aaaAlert hanteringslösning i Operations Management Suite (OMS) | Microsoft Docs"
description: "hello avisering hanteringslösning i logganalys hjälper dig att analysera alla hello aviseringar i din miljö.  I tillägg tooconsolidating aviseringar som genereras inom OMS, importerar det aviseringar från anslutna hanteringsgrupper för System Center Operations Manager till logganalys."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: fe5d534e-0418-4e2f-9073-8025e13271a8
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/13/2017
ms.author: bwren
ms.openlocfilehash: aff9bd8d88839c5227bb9ec3a1b5209a3cd7cdf8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="alert-management-solution-in-operations-management-suite-oms"></a>Lösning för avisering i Operations Management Suite (OMS)

![Varningsikon för hantering](media/log-analytics-solution-alert-management/icon.png)

hello avisering lösning hjälper dig att analysera alla hello aviseringar i logganalys-databasen.  Dessa aviseringar kan komma från olika källor, inklusive dessa källor [skapas av logganalys](log-analytics-alerts.md) eller [importeras från Nagios eller Zabbix](log-analytics-linux-agents.md).  hello lösning också importerar aviseringar från alla [anslutna hanteringsgrupper för System Center Operations Manager](log-analytics-om-agents.md).

## <a name="prerequisites"></a>Krav
hello lösningen fungerar med alla poster i hello logganalys-databas med en typ av **avisering**, så du måste utföra oavsett konfigurationen är obligatoriska toocollect dessa poster.

- För Log Analytics aviseringar [skapa Varningsregler](log-analytics-alerts.md) toocreate avisering poster direkt i hello-databasen.
- För Nagios och Zabbix aviseringar [Konfigurera servrarna](log-analytics-linux-agents.md) toosend aviseringar tooLog Analytics.
- För System Center Operations Manager-aviseringar [ansluta Operations Manager management group tooyour logganalys-arbetsytan](log-analytics-om-agents.md).  Alla aviseringar som skapats i System Center Operations Manager importeras till logganalys.  

## <a name="configuration"></a>Konfiguration
Lägg till hello Alert Management lösning tooyour OMS-arbetsyta med hjälp av hello process beskrivs i [lägga till lösningar](log-analytics-add-solutions.md).  Det krävs ingen ytterligare konfiguration.

## <a name="management-packs"></a>Hanteringspaket
Om din hanteringsgrupp för System Center Operations Manager är anslutna tooyour OMS-arbetsyta, installeras hello följande hanteringspaket i System Center Operations Manager när du lägger till den här lösningen.  Det finns ingen konfiguration eller underhåll av hello hanteringspaket som krävs.  

* Microsoft System Center Advisor Alert Management (Microsoft.IntelligencePacks.AlertManagement)

Mer information om hur lösningen hanteringspaketen är uppdaterade finns [ansluta Operations Manager tooLog Analytics](log-analytics-om-agents.md).

## <a name="data-collection"></a>Datainsamling
### <a name="agents"></a>Agenter
hello i den följande tabellen beskrivs hello anslutna källor som stöds av den här lösningen.

| Ansluten källa | Support | Beskrivning |
|:--- |:--- |:--- |
| [Windows-agenter](log-analytics-windows-agents.md) | Nej |Direkt Windows-agenter genererar inte aviseringar.  Log Analytics aviseringar kan skapas från händelserna och prestandadata som samlats in från Windows agenter. |
| [Linux-agenter](log-analytics-linux-agents.md) | Nej |Direkt Linux-agenter genererar inte aviseringar.  Log Analytics aviseringar kan skapas från händelserna och prestandadata som samlats in från Linux-agenter.  Nagios och Zabbix aviseringar som samlas in från dessa servrar som kräver hello Linux-agenten. |
| [System Center Operations Manager-hanteringsgruppen](log-analytics-om-agents.md) |Ja |Aviseringar som genereras på Operations Manager-agenter levereras toohello hanteringsgrupp och sedan vidarebefordras tooLog Analytics.<br><br>En direkt anslutning från Operations Manager-agenter tooLog Analytics krävs inte. Aviseringsdata vidarebefordras från hello management group toohello logganalys-databasen. |


### <a name="collection-frequency"></a>Insamlingsfrekvens
- Aviseringen poster är tillgängliga toohello lösning som lagrats i hello-databas.
- Aviseringen data skickas från hello Operations Manager management group tooLog Analytics tre minuters mellanrum.  

## <a name="using-hello-solution"></a>Med hello-lösning
När du lägger till hello Alert Management lösning tooyour OMS-arbetsytan hello **Alert Management** panel har lagts tooyour OMS-instrumentpanelen.  Den här panelen visar antalet och grafisk representation av hello antalet aktiva aviseringar som genererades inom hello senaste 24 timmarna.  Du kan inte ändra den här tidsintervall.

![Alert Management sida vid sida](media/log-analytics-solution-alert-management/tile.png)

Klicka på hello **Aviseringshanteringen** panelen tooopen hello **Alert Management** instrumentpanelen.  hello instrumentpanelen innehåller hello kolumner i hello i den följande tabellen.  Varje kolumnen visar hello översta 10 aviseringar efter antal matchande att kolumnens sökvillkor för hello angivna scope och ett tidsintervall.  Du kan köra en sökning i logg som innehåller hello hela listan genom att klicka på **se alla** längst ned hello hello kolumnen eller genom att klicka på kolumnrubriken hello.

| Kolumn | Beskrivning |
|:--- |:--- |
| Kritiska aviseringar |Alla aviseringar med en allvarlighetsgraden kritiskt grupperade efter namn på avisering.  Klicka på ett namn på avisering toorun en logg sökning returnera alla poster för den här aviseringen. |
| Varningsaviseringar |Alla aviseringar med en allvarlighetsgrad för varning grupperade efter namn på avisering.  Klicka på ett namn på avisering toorun en logg sökning returnera alla poster för den här aviseringen. |
| Aktiva SCOM aviseringar |Alla aviseringar som samlas in från Operations Manager med några tillstånd än *stängd* grupperade efter källan som genererade hello-avisering. |
| Alla aktiva aviseringar |Alla varningar med några allvarlighetsgrad grupperade efter namn på avisering. Endast innehåller Operations Manager-varningar med några tillstånd än *stängd*. |

Om du bläddrar toohello höger hello instrumentpanelen visar några vanliga frågor som du kan klicka på tooperform en [loggen Sök](log-analytics-log-searches.md) för aviseringsdata.

![Hanteringspanel för avisering](media/log-analytics-solution-alert-management/dashboard.png)


## <a name="log-analytics-records"></a>Log Analytics-poster
hello avisering hanteringslösning analyserar en post med en typ av **avisering**.  Aviseringar som skapats av logganalys eller samlas in från Nagios eller Zabbix samlas inte direkt av hello-lösning.

hello lösningen går att importera aviseringar från System Center Operations Manager och skapar en post för var och en med en typ av **avisering** och en SourceSystem av **OpsManager**.  Dessa poster har hello egenskaper i den följande tabellen hello:  

| Egenskap | Beskrivning |
|:--- |:--- |
| Typ |*Varning* |
| SourceSystem |*OpsManager* |
| AlertContext |Information om hello dataobjekt som orsakade aviseringen hello toobe som genereras i XML-format. |
| AlertDescription |Detaljerad beskrivning av hello avisering. |
| AlertId |GUID för hello avisering. |
| AlertName |Namnet på hello avisering. |
| AlertPriority |Prioritetsnivån för hello avisering. |
| AlertSeverity |Allvarlighetsgrad för hello avisering. |
| In AlertState |Senaste lösningstillstånd för avisering hello. |
| LastModifiedBy |Namnet på hello användare som senast ändrade hello avisering. |
| ManagementGroupName |Namnet på hello-hanteringsgrupp där hello aviseringen genererades. |
| RepeatCount |Antal gånger hello samma avisering har genererats för hello samma övervakade objekt eftersom som matchas. |
| ResolvedBy |Namnet på hello användare matchas hello avisering. Tomt om hello avisering ännu inte har lösts. |
| SourceDisplayName |Visningsnamn för hello Övervakningsobjekt som genererade hello avisering. |
| SourceFullName |Fullständigt namn på hello Övervakningsobjekt som genererade hello avisering. |
| TicketId |Biljett-ID för hello aviseringen om hello System Center Operations Manager-miljö är integrerad med en process för att tilldela biljetter för aviseringar.  Tom av inga tjänstbiljett ID tilldelas. |
| TimeGenerated |Datum och tid då hello aviseringen skapades. |
| TimeLastModified |Datum och tid då hello aviseringen senast ändrades. |
| TimeRaised |Datum och tid då hello avisering har genererats. |
| TimeResolved |Datum och tid då hello aviseringen har lösts. Tomt om hello avisering ännu inte har lösts. |

## <a name="sample-log-searches"></a>Exempel på loggsökningar
hello innehåller följande tabell exempel loggen söker efter avisering innehåller information som samlas in av den här lösningen: 

| Fråga | Beskrivning |
|:--- |:--- |
| Typ = avisering SourceSystem = OpsManager AlertSeverity = fel TimeRaised > nu 24 timmar |Kritiska aviseringar som genererats under hello senaste 24 timmarna |
| Typ = avisering AlertSeverity = varning TimeRaised > nu 24 timmar |Varningsaviseringar som genererats under hello senaste 24 timmarna |
| Typ = avisering SourceSystem = OpsManager in AlertState! = stängd TimeRaised > nu 24 TIMMARS &#124; måttet count() som antal av SourceDisplayName |Källor med aktiva aviseringar som genererats under hello senaste 24 timmarna |
| Typ = avisering SourceSystem = OpsManager AlertSeverity = fel TimeRaised > nu 24 TIMMARS in AlertState! = stängd |Kritiska aviseringar som genererats under hello senaste 24 timmarna som fortfarande är aktiva |
| Typ = avisering SourceSystem = OpsManager TimeRaised > nu 24 TIMMARS in AlertState = stängd |Aviseringar som genererats under hello senaste 24 timmarna som nu har stängts |
| Typ = avisering SourceSystem = OpsManager TimeRaised > nu 1 dag &#124; måttet count() som antal av AlertSeverity |Aviseringar som genererats under hello senaste 1 dagen grupperat efter allvarlighetsgrad |
| Typ = avisering SourceSystem = OpsManager TimeRaised > nu 1 dag &#124; Sortera RepeatCount desc |Aviseringar som genererats under hello senaste 1 dagen sorterat efter upprepat antalsvärde |


>[!NOTE]
> Om ditt arbetsområde har uppgraderade toohello [nya Log Analytics-frågespråket](log-analytics-log-search-upgrade.md), hello föregående frågor skulle ändra toohello följande:
>
>| Fråga | Beskrivning |
|:---|:---|
| Varna n &#124; där SourceSystem == ”OpsManager” och AlertSeverity == ”error” och TimeRaised > ago(24h) |Kritiska aviseringar som genererats under hello senaste 24 timmarna |
| Varna n &#124; där AlertSeverity == ”varning” och TimeRaised > ago(24h) |Varningsaviseringar som genererats under hello senaste 24 timmarna |
| Varna n &#124; där SourceSystem == ”OpsManager” och in AlertState! = ”stängd” och TimeRaised > ago(24h) &#124; Sammanfatta Count = count() av SourceDisplayName |Källor med aktiva aviseringar som genererats under hello senaste 24 timmarna |
| Varna n &#124; där SourceSystem == ”OpsManager” och AlertSeverity == ”error” och TimeRaised > ago(24h) och in AlertState! = ”stängd” |Kritiska aviseringar som genererats under hello senaste 24 timmarna som fortfarande är aktiva |
| Varna n &#124; där SourceSystem == ”OpsManager” och TimeRaised > ago(24h) och in AlertState == ”stängd” |Aviseringar som genererats under hello senaste 24 timmarna som nu har stängts |
| Varna n &#124; där SourceSystem == ”OpsManager” och TimeRaised > ago(1d) &#124; Sammanfatta Count = count() av AlertSeverity |Aviseringar som genererats under hello senaste 1 dagen grupperat efter allvarlighetsgrad |
| Varna n &#124; där SourceSystem == ”OpsManager” och TimeRaised > ago(1d) &#124; Sortera efter RepeatCount desc |Aviseringar som genererats under hello senaste 1 dagen sorterat efter upprepat antalsvärde |


## <a name="next-steps"></a>Nästa steg
* Lär dig mer om [Aviseringar i Log Analytics](log-analytics-alerts.md) för information om att generera aviseringar från Log Analytics.
