---
title: "aaaAgent hälsotillstånd lösning i OMS | Microsoft Docs"
description: "Den här artikeln är avsedd toohelp du förstår hur toouse den här lösningen toomonitor hello hälsotillståndet för dina agenter som rapporterar direkt tooOMS eller System Center Operations Manager."
services: operations-management-suite
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: operations-management-suite
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/17/2017
ms.author: magoedte
ms.openlocfilehash: 071b14b4ab7af6680ae458eaa331246755c5bb56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
#  <a name="agent-health-solution-in-oms"></a>Agenthälsolösning i OMS
Hej Agenthälsa lösning i OMS hjälper dig att förstå för alla hello-agenter som rapporterar direkt toohello OMS-arbetsyta eller System Center Operations Manager management group anslutna tooOMS och som inte svarar och skicka in användningsdata.  Du kan också hålla reda på hur många agenter distribueras, där de är fördelade geografiskt och utföra andra frågor toomaintain medvetenhet om hello distribution av agenter distribueras i Azure, andra miljöer i molnet eller lokalt.    

## <a name="prerequisites"></a>Krav
Innan du distribuerar den här lösningen kan du bekräfta att du har för närvarande stöds [Windows-agenter](../log-analytics/log-analytics-windows-agents.md) toohello OMS-arbetsytan rapportering eller rapportering tooan [Operations Manager-hanteringsgruppen](../log-analytics/log-analytics-om-agents.md) integrerad med din OMS-arbetsyta.    

## <a name="solution-components"></a>Lösningskomponenter
Den här lösningen består av hello efter resurser som har lagts till tooyour arbetsytan och direktanslutna agenter eller Operations Manager ansluten hanteringsgrupp.

### <a name="management-packs"></a>Hanteringspaket
Om din hanteringsgrupp för System Center Operations Manager är anslutna tooan OMS-arbetsyta, är hello följande hanteringspaket installerade i Operations Manager.  Dessa hanteringspaket också har installerats på direktanslutna Windows-datorer när du lägger till den här lösningen. Det finns inget tooconfigure eller hantera med de här hanteringspaketen.

* Microsoft System Center Advisor HealthAssessment Direct Channel Intelligence Pack  (Microsoft.IntelligencePacks.HealthAssessmentDirect)
* Microsoft System Center Advisor HealthAssessment Server Channel Intelligence Pack (Microsoft.IntelligencePacks.HealthAssessmentViaServer).  

Mer information om hur lösningen hanteringspaketen är uppdaterade finns [ansluta Operations Manager tooLog Analytics](../log-analytics/log-analytics-om-agents.md).

## <a name="configuration"></a>Konfiguration
Lägg till hello Agenthälsa lösning tooyour OMS-arbetsyta med hjälp av hello process beskrivs i [lägga till lösningar](../log-analytics/log-analytics-add-solutions.md). Det krävs ingen ytterligare konfiguration.


## <a name="data-collection"></a>Datainsamling
### <a name="supported-agents"></a>Agenter som stöds
hello i den följande tabellen beskrivs hello anslutna källor som stöds av den här lösningen.

| Ansluten källa | Stöds | Beskrivning |
| --- | --- | --- |
| Windows-agenter | Ja | Pulsslagshändelser som samlas in direkt från Windows-agenter.|
| System Center Operations Manager-hanteringsgrupp | Ja | Heartbeat-händelser som samlas in från agenter som rapporterar toohello hanteringsgruppen var 60: e sekund och sedan vidarebefordras tooLog Analytics. En direkt anslutning från Operations Manager-agenter tooLog Analytics krävs inte. Pulsslag händelsedata vidarebefordras från hello management group toohello logganalys-databasen.|

## <a name="using-hello-solution"></a>Med hello-lösning
När du lägger till hello lösning tooyour OMS-arbetsytan hello **Agenthälsa** panelen läggs tooyour OMS-instrumentpanelen. Den här panelen visar hello Totalt antal agenter och hello antalet agenter som inte svarar i hello senaste 24 timmarna.<br><br> ![Panelen för Agenthälsa på instrumentpanelen](./media/oms-solution-agenthealth/agenthealth-solution-tile-homepage.png)

Klicka på hello **Agenthälsa** panelen tooopen hello **Agenthälsa** instrumentpanelen.  hello instrumentpanelen innehåller hello kolumner i hello i den följande tabellen. Varje kolumn visar hello översta tio händelser efter antal som matchar att kolumnens sökvillkor för hello angivna tidsintervallet. Du kan köra en sökning i logg som innehåller hello hela listan genom att välja **se alla** längst ned hello rätt i varje kolumn, eller klicka på kolumnrubriken hello.

| Kolumn | Beskrivning |
|--------|-------------|
| Agentantal över tid | En trend över agentantal under en period på sju dagar för Windows- och Linux-agenter.|
| Antal agenter som inte svarar | En lista över agenter som inte har skickat ett pulsslag i hello senaste 24 timmarna.|
| Distribution enligt OS-typ | En partition av hur många Windows och Linux-agenter du har i din miljö.|
| Distribution enligt Agent-version | En partition av olika hello agentversioner som installerats i din miljö och ett antal på varandra.|
| Distribution enligt Agent-kategori | En partition av hello olika kategorier av agenter som skickar händelser för pulsslag: direkta agenter, OpsMgr agenter eller hello OpsMgr Management Server.|
| Distribution enligt hanteringsgrupp | En partition hello olika SCOM hanteringsgrupper i din miljö.|
| Geoplats för agenter | En partition av hello olika länder där du har agenter och Totalt antal hello antalet agenter som har installerats i varje land.|
| Antalet installerade Gateways | hello antalet servrar som har hello OMS Gateway som har installerats och en lista över dessa servrar.|

![Agenthälsa på instrumentpanelen - exempel](./media/oms-solution-agenthealth/agenthealth-solution-dashboard.png)  

## <a name="log-analytics-records"></a>Log Analytics-poster
hello lösning skapar en typ av post i hello OMS-databasen.  

### <a name="heartbeat-records"></a>Pulsslagsposter
En post med en typ av **pulsslag** skapas.  Dessa poster har hello egenskaper i hello i den följande tabellen.  

| Egenskap | Beskrivning |
| --- | --- |
| Typ | *Pulsslag*|
| Kategori | Värdet är *Direct Agent*, *SCOM Agent* eller *SCOM Management Server*.|
| Dator | Datornamn.|
| OSType | Windows- eller Linux-operativsystem.|
| OSMajorVersion | Högre operativsystemversion.|
| OSMinorVersion | Lägre operativsystemversion.|
| Version | OMS-Agent eller Operations Manager Agent-version.|
| SCAgentChannel | Värdet är *Direkt* och/eller *SCManagementServer*.|
| IsGatewayInstalled | Om OMS-gatewayen har installerats är värdet *SANT*, annars är värdet *FALSKT*.|
| ComputerIP | Hello datorns IP-adress.|
| RemoteIPCountry | Geografisk plats där datorn har distribuerats.|
| ManagementGroupName | Namn på Operations Manager-hanteringsgrupp.|
| SourceComputerId | Unikt ID för datorn.|
| RemoteIPLongitude | Longituden för datorns geografiska plats.|
| RemoteIPLatitude | Latituden för datorns geografiska plats.|

Varje agent som rapporterar tooan Operations Manager-hanteringsservern skickar två pulsslag och SCAgentChannel egenskapsvärdet innehåller både **direkt** och **SCManagementServer** beroende på vad Log Analytics-datakällor och lösningar som du har aktiverat i OMS-prenumeration. Om du kommer ihåg är data från lösningar skickas direkt från ett Operations Manager management server toohello OMS webbtjänsten, eller på grund av hello mängden data som samlas in på hello agent skickas direkt från hello agent tooOMS webbtjänsten. För heartbeat-händelser som har hello värde **SCManagementServer**, hello ComputerIP värde är hello hello management serverns IP-adress eftersom hello data faktiskt har laddats upp av den.  För pulsslag där SCAgentChannel har angetts för**direkt**, det är hello offentliga IP-adress hello agent.  

## <a name="sample-log-searches"></a>Exempel på loggsökningar
hello innehåller följande tabell exempel loggen söker efter poster som samlas in av den här lösningen.

| Fråga | Beskrivning |
| --- | --- |
| Skriv=Heartbeat &#124; distinct Computer |Totalt antal agenter |
| Skriv=Heartbeat &#124; measure max(TimeGenerated) som LastCall från Computer &#124; där LastCall < NOW-24HOURS |Räkning av agenter som inte svarar i hello senaste 24 timmarna |
| Type=Heartbeat &#124; measure max(TimeGenerated) som LastCall från Computer &#124; där LastCall < NU-15MINUTES |Räkning av agenter som inte svarar i hello senaste 15 minuterna |
| Skriv=Heartbeat TimeGenerated>NOW-24HOURS Computer IN {Type=Heartbeat TimeGenerated>NOW-24HOURS &#124; distinct Computer} &#124; measure max(TimeGenerated) som LastCall enligt Computer |Datorer online (i hello senaste 24 timmarna) |
| Skriv=Heartbeat TimeGenerated>NOW-24HOURS Computer NOT IN {Type=Heartbeat TimeGenerated>NOW-30MINUTES &#124; distinct Computer} &#124; measure max(TimeGenerated) som LastCall enligt Computer |Totalt antal agenter Offline under senaste 30 minuterna (för hello senaste 24 timmarna) |
| Skriv = Heartbeat &#124; measure countdistinct(Computer) enligt OSType |Hämta en trend över antalet agenter över tid enligt OSType|
| Skriv = Heartbeat&#124;measure countdistinct(Computer) enligt OSType |Distribution enligt OS-typ |
| Skriv = Heartbeat&#124;measure countdistinct(Computer) enligt Version |Distribution enligt Agent-version |
| Skriv = Heartbeat &#124; measure count() enligt Category |Distribution enligt Agent-kategori |
| Skriv = Heartbeat&#124;measure countdistinct(Computer) enligt ManagementGroupName | Distribution enligt hanteringsgrupp |
| Skriv = Heartbeat&#124;measure countdistinct(Computer) enligt RemoteIPCountry |Geoplats för agenter |
| Type=Heartbeat IsGatewayInstalled=true&#124;Distinct Computer |Antalet installerade OMS-Gateways |


>[!NOTE]
> Om ditt arbetsområde har uppgraderade toohello [nya Log Analytics-frågespråket](../log-analytics/log-analytics-log-search-upgrade.md), hello senare frågorna skulle ändra toohello följande.
>
>| Fråga | Beskrivning |
|:---|:---|
| Heartbeat &#124; distinct Computer |Totalt antal agenter |
| Heartbeat &#124; summarize LastCall = max(TimeGenerated) by Computer &#124; where LastCall < ago(24h) |Räkning av agenter som inte svarar i hello senaste 24 timmarna |
| Heartbeat &#124; summarize LastCall = max(TimeGenerated) by Computer &#124; where LastCall < ago(15m) |Räkning av agenter som inte svarar i hello senaste 15 minuterna |
| Heartbeat &#124; where TimeGenerated > ago(24h) and Computer in ((Heartbeat &#124; where TimeGenerated > ago(24h) &#124; distinct Computer)) &#124; summarize LastCall = max(TimeGenerated) by Computer |Datorer online (i hello senaste 24 timmarna) |
| Heartbeat &#124; where TimeGenerated > ago(24h) and Computer !in ((Heartbeat &#124; where TimeGenerated > ago(30m) &#124; distinct Computer)) &#124; summarize LastCall = max(TimeGenerated) by Computer |Totalt antal agenter Offline under senaste 30 minuterna (för hello senaste 24 timmarna) |
| Heartbeat &#124; summarize AggregatedValue = dcount(Computer) by OSType |Hämta en trend över antalet agenter över tid enligt OSType|
| Heartbeat &#124; summarize AggregatedValue = dcount(Computer) by OSType |Distribution enligt OS-typ |
| Heartbeat &#124; summarize AggregatedValue = dcount(Computer) by Version |Distribution enligt Agent-version |
| Heartbeat &#124; summarize AggregatedValue = count() by Category |Distribution enligt Agent-kategori |
| Heartbeat &#124; summarize AggregatedValue = dcount(Computer) by ManagementGroupName | Distribution enligt hanteringsgrupp |
| Heartbeat &#124; summarize AggregatedValue = dcount(Computer) by RemoteIPCountry |Geoplats för agenter |
| Heartbeat &#124; where iff(isnotnull(toint(IsGatewayInstalled)), IsGatewayInstalled == true, IsGatewayInstalled == "true") == true &#124; distinct Computer |Antalet installerade OMS-Gateways |

## <a name="next-steps"></a>Nästa steg

* Lär dig mer om [Aviseringar i Log Analytics](../log-analytics/log-analytics-alerts.md) för information om att generera aviseringar från Log Analytics.
