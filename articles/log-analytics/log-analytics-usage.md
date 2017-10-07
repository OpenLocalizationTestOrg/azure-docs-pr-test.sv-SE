---
title: "aaaAnalyze dataanvändning i Log Analytics | Microsoft Docs"
description: "Använd instrumentpanelen för användning av hello i logganalys tooview hur mycket data skickas toohello logganalys-tjänsten och felsöka anledningen till att stora mängder data skickas."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 74d0adcb-4dc2-425e-8b62-c65537cef270
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/21/2017
ms.author: magoedte
ms.openlocfilehash: c30373dd6edbe3ff900fbebc865575fee61ce14c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-data-usage-in-log-analytics"></a>Analysera dataanvändning i Log Analytics
Logganalys innehåller information om hello mängden data som samlas in, som datorer skickas hello data och hello olika typer av data som skickas.  Använd hello **Log Analytics användning** instrumentpanelen toosee hello mängden data skickas toohello logganalys-tjänsten. hello instrumentpanelen visar hur mycket data som samlas in av varje lösning och hur mycket data datorer skickar.

## <a name="understand-hello-usage-dashboard"></a>Förstå hello användning instrumentpanelen
Hej **logganalys användning** instrumentpanelen visar hello följande information:

- Datavolym
    - Datavolymen över tid (baserat på ditt aktuella tidsomfång)
    - Datavolym per lösning
    - Data som inte är associerade med en dator
- Datorer
    - Datorer som skickar data
    - Datorer utan data de senaste 24 timmarna
- Erbjudanden
    - Insikts- och analysnoder
    - Automatiserings- och styrningsnoder
    - Säkerhetsnoder
- Prestanda
    - Tidsåtgång toocollect och index data
- Lista med frågor

![instrumentpanelen användning](./media/log-analytics-usage/usage-dashboard01.png)

### <a name="toowork-with-usage-data"></a>toowork med användningsdata
1. Om du inte redan har gjort det loggar du in toohello [Azure-portalen](https://portal.azure.com) med din Azure-prenumeration.
2. På hello **hubb** -menyn klickar du på **fler tjänster** och Skriv i hello lista över resurser, **logganalys**. När du börjar skriva in hello listan filtreras baserat på dina indata. Klicka på **Log Analytics**.  
    ![Azure-hubb](./media/log-analytics-usage/hub.png)
3. Hej **logganalys** instrumentpanelen visar en lista över dina arbetsytor. Välj en arbetsyta.
4. I hello *arbetsytan* instrumentpanelen, klickar du på **logganalys användning**.
5. På hello **Analytics Logganvändning** instrumentpanelen, klickar du på **tid: senaste 24 timmarna** toochange hello tidsintervall.  
    ![tidsintervall](./media/log-analytics-usage/time.png)
6. Visa hello användning kategori blad som visar områden du är intresserad av. Välj ett blad och klicka på ett objekt i den tooview mer information finns i [loggen Sök](log-analytics-log-searches.md).  
    ![användningsblad med exempeldata](./media/log-analytics-usage/blade.png)
7. Granska hello resultat som returneras från hello sökning på hello loggen Sök instrumentpanelen.  
    ![exempel på loggsökning för användning](./media/log-analytics-usage/usage-log-search.png)

## <a name="create-an-alert-when-data-collection-is-higher-than-expected"></a>Skapa en avisering när datainsamlingen är högre än väntat
Det här avsnittet beskrivs hur toocreate en avisering om:
- Datavolymen överskrider en angiven mängd.
- Datavolym är förväntade tooexceed en angiven mängd.

Log Analytics-[aviseringar](log-analytics-alerts-creating.md) använder sökfrågor. hello har följande fråga ett resultat när det finns fler än 100 GB data som samlas in i hello senaste 24 timmarna:

`Type=Usage QuantityUnit=MBytes IsBillable=true | measure sum(div(Quantity,1024)) as DataGB by Type | where DataGB > 100`

hello följande frågan använder en enkel formeln toopredict när mer än 100 GB data skickas i en dag: 

`Type=Usage QuantityUnit=MBytes IsBillable=true | measure sum(div(mul(Quantity,8),1024)) as EstimatedGB by Type | where EstimatedGB > 100`

tooalert på en annan volym, ändra hello 100 hello frågor toohello antal GB som du vill använda tooalert på.

Använd hello stegen som beskrivs i [skapa en aviseringsregel](log-analytics-alerts-creating.md#create-an-alert-rule) toobe avisering när datainsamlingen är högre än förväntat.

Ange när du skapar hello varning för hello första frågan--när det finns fler än 100 GB data i 24 timmar på:
- **Namnet** för*datavolym som är större än 100 GB i 24 timmar*
- **Allvarlighetsgrad** för*varning*
- **Sökfråga** för`Type=Usage QuantityUnit=MBytes IsBillable=true | measure sum(div(Quantity,1024)) as DataGB by Type | where DataGB > 100`
- **Tidsfönstret** för*24 timmar*.
- **Varna frekvens** toobe en timme sedan hello användningsdata uppdateras bara en gång i timmen.
- **Generera en avisering baserat på** toobe *antal resultat*
- **Antalet resultat** toobe *större än 0*

Använd hello stegen som beskrivs i [Lägg till åtgärder tooalert regler](log-analytics-alerts-actions.md) konfigurerar en åtgärd för hello varningsregeln att e-post, webhook eller runbook.

När skapar hello varning för hello andra frågan--när prognoser kommer att mer än 100 GB data i 24 timmar, ange den:
- **Namnet** för*datavolym förväntas toogreater än 100 GB i 24 timmar*
- **Allvarlighetsgrad** för*varning*
- **Sökfråga** för`Type=Usage QuantityUnit=MBytes IsBillable=true | measure sum(div(mul(Quantity,8),1024)) as EstimatedGB by Type | where EstimatedGB > 100`
- **Tidsfönstret** för*3 timmar*.
- **Varna frekvens** toobe en timme sedan hello användningsdata uppdateras bara en gång i timmen.
- **Generera en avisering baserat på** toobe *antal resultat*
- **Antalet resultat** toobe *större än 0*

När du får en avisering kan använda hello steg i hello följande avsnitt tootroubleshoot varför användningen är högre än väntat.

## <a name="troubleshooting-why-usage-is-higher-than-expected"></a>Felsökning varför användningen är större än förväntat
hello användning instrumentpanelen kan du tooidentify varför användning (och därför kostnad) är högre än vad du förväntar dig.

Högre användning orsakas av en eller båda:
- Mer data än förväntat skickas tooLog Analytics
- Fler noder än förväntat att skicka data tooLog Analytics

### <a name="check-if-there-is-more-data-than-expected"></a>Kontrollera om datamängden är större än förväntat 
Det finns två viktiga avsnitt på sidan för användning av hello som hjälper dig att identifiera vad som orsakar hello toobe för de flesta data som samlas in.

Hej *datavolym över tid* diagrammet visar hello totala mängden data som skickas och hello datorer skickar hello de flesta data. hello diagrammet överst hello kan toosee om din övergripande dataanvändning blir kvar oförändrad eller minskande. hello listan över datorer som visar hello 10-datorer som de flesta data skickas hello.

Hej *datavolym per lösning* diagrammet visar hello mängden data som skickas av varje lösning och hello lösningar och skicka hello de flesta data. hello diagrammet överst hello visar hello totala mängden data som skickas av varje lösning över tid. Den här informationen kan du tooidentify om en lösning skickar mer information om hello samma belopp av data eller mindre data över tid. lösningar hello lista visar hello 10 lösningar och skicka hello de flesta data. 

I de här två diagrammen visas alla data. Vissa data är fakturerbara och andra är kostnadsfria. toofocus endast på data som fakturerbar ändra hello frågan på hello Sök sidan tooinclude `IsBillable=true`.  

![datavolymdiagram](./media/log-analytics-usage/log-analytics-usage-data-volume.png)

Titta på hello *datavolym över tid* diagram. toosee hello lösningar och datatyper som skickar hello de flesta data för en viss dator, klicka på hello namnet på hello-dator. Klicka på hello namnet på hello första dator i hello-listan.

I följande skärmbild hello, hello *hantering / Perf* datatyp skickar hello de flesta data för hello-datorn. 

![datavolym för en dator](./media/log-analytics-usage/log-analytics-usage-data-volume-computer.png)

Gå sedan tillbaka toohello *användning* instrumentpanel och titta på hello *datavolym per lösning* diagram. toosee hello datorer skickar hello de flesta data för en lösning, klicka på hello namn i hello lösning i hello-listan. Klicka på hello namn i hello första lösning i hello-listan. 

I följande skärmbild hello, bekräftar det att hello *acmetomcat* datorn skickar hello de flesta data för hello loggen hanteringslösning.

![datavolymen för en lösning](./media/log-analytics-usage/log-analytics-usage-data-volume-solution.png)

Om det behövs, utför ytterligare analys tooidentify stora volymer av samma typ av lösning eller data. Exempelfrågor omfattar:

+ **Security**-lösningen
  - `Type=SecurityEvent | measure count() by EventID`
+ **Log Management**-lösningen
  - `Type=Usage Solution=LogManagement IsBillable=true | measure count() by DataType`
+ **Perf**-datatypen
  - `Type=Perf | measure count() by CounterPath`
  - `Type=Perf | measure count() by CounterName`
+ **Event**-datatypen
  - `Type=Event | measure count() by EventID`
  - `Type=Event | measure count() by EventLog, EventLevelName`
+ **Syslog**-datatypen
  - `Type=Syslog | measure count() by Facility, SeverityLevel`
  - `Type=Syslog | measure count() by ProcessName`
+ Datatypen **AzureDiagnostics**
  - `Type=AzureDiagnostics | measure count() by ResourceProvider, ResourceId`

Använd följande steg tooreduce hello volym loggar samlas in hello:

| Källan för hög datavolym | Hur tooreduce datavolym |
| -------------------------- | ------------------------- |
| Säkerhetshändelser            | Välj [vanliga eller minimala säkerhetshändelser](https://blogs.technet.microsoft.com/msoms/2016/11/08/filter-the-security-events-the-oms-security-collects/) <br> Ändra hello princip toocollect behövs endast säkerhetsgranskningshändelser. I synnerhet granska hello måste toocollect händelser för <br> - [granska filtreringplattform](https://technet.microsoft.com/library/dd772749(WS.10).aspx) <br> - [granska register](https://docs.microsoft.com/windows/device-security/auditing/audit-registry)<br> - [granska filsystem](https://docs.microsoft.com/windows/device-security/auditing/audit-file-system)<br> - [granska kernelobjekt](https://docs.microsoft.com/windows/device-security/auditing/audit-kernel-object)<br> - [granska hantering av manipulering](https://docs.microsoft.com/windows/device-security/auditing/audit-handle-manipulation)<br> - [granska flyttbara lagringsmedia](https://docs.microsoft.com/windows/device-security/auditing/audit-removable-storage) |
| Prestandaräknare       | Ändra [prestandaräknarens konfiguration](log-analytics-data-sources-performance-counters.md) för att: <br> -Minska hello frekvensen för samlingen <br> - Minska antalet prestandaräknare |
| Händelseloggar                 | Ändra [händelseloggens konfiguration](log-analytics-data-sources-windows-events.md) för att: <br> -Minska hello antalet händelseloggar samlas in <br> - Endast samla in obligatoriska händelsenivåer. Till exempel, samla inte in händelser på *Informationsnivå* |
| Syslog                     | Ändra [systemloggkonfigurationen](log-analytics-data-sources-syslog.md) för att: <br> -Minska hello antalet verksamhet samlas in <br> - Endast samla in obligatoriska händelsenivåer. Till exempel, samla inte in händelser på *Informations-* eller *Felsökningsnivå* |
| AzureDiagnostics           | Ändra logginsamlingen för resurser för att: <br> -Minska hello antalet resurser skicka loggar tooLog Analytics <br> – Endast samla in nödvändiga loggar |
| Lösning data från datorer som inte behöver hello lösning | Använd [lösning riktad](../operations-management-suite/operations-management-suite-solution-targeting.md) toocollect data från endast obligatoriska grupper med datorer. |

### <a name="check-if-there-are-more-nodes-than-expected"></a>Kontrollera om det finns fler noder än förväntat
Om du är på hello *per nod (OMS)* prisnivån och sedan du debiteras baserat på hello antal noder och lösningar som du använder. Du kan se hur många noder i varje erbjudande som används i hello *erbjudanden* avsnittet hello användning instrumentpanelen.

![instrumentpanelen användning](./media/log-analytics-usage/log-analytics-usage-offerings.png)

Klicka på **visas alla...**  tooview hello fullständig lista över datorer som skickar data för valda hello-erbjudandet.

Använd [lösning riktad](../operations-management-suite/operations-management-suite-solution-targeting.md) toocollect data från endast obligatoriska grupper med datorer.


## <a name="next-steps"></a>Nästa steg
* Se [logga sökningar i logganalys](log-analytics-log-searches.md) toolearn hur toouse hello söka språk. Du kan använda Sök frågor tooperform ytterligare analys på hello användningsdata.
* Använd hello stegen som beskrivs i [skapa en aviseringsregel](log-analytics-alerts-creating.md#create-an-alert-rule) toobe meddelas när ett sökvillkor uppfylls
* Använd [lösning riktad](../operations-management-suite/operations-management-suite-solution-targeting.md) toocollect data från endast obligatoriska grupper av datorer
* Välj [vanliga eller minimala säkerhetshändelser](https://blogs.technet.microsoft.com/msoms/2016/11/08/filter-the-security-events-the-oms-security-collects/)
* Ändra [prestandaräknarens konfiguration](log-analytics-data-sources-performance-counters.md)
* Ändra [händelseloggens konfiguration](log-analytics-data-sources-windows-events.md)
* Ändra [systemloggens konfiguration](log-analytics-data-sources-syslog.md)
