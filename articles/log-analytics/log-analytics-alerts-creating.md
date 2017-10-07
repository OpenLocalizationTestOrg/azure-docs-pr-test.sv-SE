---
title: aaaCreating aviseringar i OMS Log Analytics | Microsoft Docs
description: "Aviseringar i Log Analytics identifiera viktig information i OMS-databasen och kan proaktivt meddelar dig om problem eller anropa åtgärder tooattempt toocorrect dem.  Den här artikeln beskriver hur toocreate en aviseringsregel och information hello olika åtgärder de kan vidta."
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
ms.date: 05/23/2017
ms.author: bwren
ms.openlocfilehash: 3d035b2426dda9645b19e6c993dc26a2d95a2a78
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="working-with-alert-rules-in-log-analytics"></a>Arbeta med Varningsregler i logganalys
Aviseringar skapas med Varningsregler som automatiskt kör loggen söker regelbundet.  De skapar en avisering post om hello resultat matchar särskilda.  hello regeln kan sedan automatiskt köra en eller flera åtgärder tooproactively meddelar dig om hello aviseringen eller anropa en annan process.   

Den här artikeln beskriver hello processer toocreate och redigera Varningsregler med hello OMS-portalen.  Mer information om hello olika inställningar och hur tooimplement måste logik finns [förstå aviseringar i logganalys](log-analytics-alerts.md).

>[!NOTE]
> För närvarande kan du skapa eller ändra en aviseringsregel med hjälp av hello Azure-portalen. 

## <a name="create-an-alert-rule"></a>Skapa en aviseringsregel

toocreate en aviseringsregel med hjälp av hello OMS-portalen, börja med att skapa en logg söka efter hello-poster som ska anropa hello avisering.  Hej **avisering** knappen är tillgängliga så att du kan skapa och konfigurera hello varningsregel.

>[!NOTE]
> Högst 250 Varningsregler kan för närvarande skapas i en OMS-arbetsyta. 

1. Hello OMS översikt över sidan klickar du på **loggen Sök**.
2. Skapa en ny logg sökfråga eller välj en sparad logg-sökning. 
3. Klicka på **avisering** hello överst i hello sidan tooopen hello **lägga till Varningsregeln** skärmen.
4. Konfigurera hello varningsregeln med hjälp av informationen i [information om Varningsregler](#details-of-alert-rules) nedan.
6. Klicka på **spara** toocomplete hello varningsregel.  Den ska börja köras omedelbart.


## <a name="edit-an-alert-rule"></a>Redigera en varningsregel
Du kan hämta en lista över alla Varningsregler i hello **aviseringar** -menyn i logganalys **inställningar**.  

![Hantera aviseringar](./media/log-analytics-alerts/configure.png)

1. I hello OMS-konsolen väljer hello **inställningar** panelen.
2. Välj **aviseringar**.

Du kan utföra flera åtgärder från den här vyn.

* Inaktivera en regel genom att välja **av** nästa tooit.
* Redigera en aviseringsregel genom att klicka på nästa tooit för hello penna ikon.
* Ta bort en varningsregel genom att klicka på hello **X** ikonen nästa tooit. 

## <a name="details-of-alert-rules"></a>Information om Varningsregler
När du skapar eller redigerar en aviseringsregel i hello OMS-portalen kan du arbeta med hello **Lägg till regel varning** eller **redigera Aviseringsregel** sidan.  hello följande tabeller beskrivs hello fälten i den här skärmen.

![Varningsregeln](media/log-analytics-alerts/add-alert-rule.png)

### <a name="alert-information"></a>Aviseringsinformation
Dessa är grundläggande inställningar för hello varningsregeln och hello aviseringar skapas.

| Egenskap | Beskrivning |
|:--- |:---|
| Namn | Unikt namn tooidentify hello varningsregel. Det här namnet ingår i alla varningar som skapats av hello regel.  |
| Beskrivning | Valfri beskrivning av hello varningsregel. |
| Allvarsgrad |Allvarlighetsgrad för alla aviseringar som skapats av den här regeln. |

### <a name="search-query-and-time-window"></a>Fråge- och sökfönstret
hello fråge- och sökfönstret som returnerar hello-poster som har utvärderats toodetermine om alla aviseringar som ska skapas.

| Egenskap | Beskrivning |
|:--- |:---|
| Sökfråga | Detta är hello-fråga som ska köras.  hello-poster som returneras av den här frågan kommer att använda toodetermine om en avisering skapas.<br><br>Välj **Använd aktuell sökfråga** toouse hello aktuell fråga eller välj en befintlig sparad sökning hello-listan.  hello frågesyntaxen tillhandahålls i hello textruta där du kan ändra det om det behövs. |
| Tidsfönster |Anger hello tidsintervall för hello frågan.  hello frågan returnerar bara de poster som har skapats i det här intervallet av hello aktuell tid.  Detta kan vara ett värde mellan 5 minuter och 24 timmar.  Det bör vara större än eller lika med toohello aviseringsfrekvensen.  <br><br> Till exempel returneras hello fönstret angetts too60 minuter och hello frågan körs klockan 13:15, endast de poster som skapats mellan 12:15:00 och 1:15 i Återställningsmappen. |

När du anger hello tidsfönstret för hello varningsregeln visas hello antalet befintliga poster som matchar hello sökkriterierna för den tidsperioden.  Detta kan hjälpa dig att avgöra hello frekvens som ger dig hello antalet resultat som du förväntar dig.

### <a name="schedule"></a>Schema
Definierar hur ofta hello sökfråga körs.

| Egenskap | Beskrivning |
|:--- |:---|
| Aviseringsfrekvensen | Anger hur ofta hello frågan ska köras. Kan vara ett värde mellan 5 minuter och 24 timmar. Ska vara lika tooor som är mindre än hello tidsperioden.  Om hello-värdet är större än hello tidsfönstret riskerar du poster som saknas.<br><br>Tänk dig ett tidsfönster på 30 minuter och en frekvens som 60 minuter.  Om hello frågan körs 1:00, returnerar poster mellan 12:30 och 1:00.  hello är hello frågan skulle köras just nu 2:00 när återgår den poster mellan 1:30 och 2:00.  Alla poster som skapats mellan 01:00 och 1:30 skulle aldrig utvärderas. |


### <a name="generate-alert-based-on"></a>Generera en avisering baserat på
Definierar hello kriterier som ska utvärderas mot hello resultaten av hello Sök fråga toodetermine om en avisering ska skapas.  Detaljerna är olika beroende på hello typ av regel för varning som du väljer.  Du kan hämta information för hello varningsregel för olika typer från [förstå aviseringar i logganalys](log-analytics-alerts.md).

| Egenskap | Beskrivning |
|:--- |:---|
| Ignorera aviseringar | När du aktiverar Undertryckning för hello varningsregeln har åtgärder för hello regeln inaktiverats för en angiven tidsperiod när du har skapat en ny avisering. hello regeln körs och skapar avisering poster om hello villkoret är uppfyllt. Detta är tooallow tid toocorrect hello problem utan att köra dubbla åtgärder. |

#### <a name="number-of-results-alert-rules"></a>Antalet resultat Varningsregler

| Egenskap | Beskrivning |
|:--- |:---|
| Antalet resultat |En avisering skapas om hello antalet poster som returneras av frågan hello är antingen **större än** eller **mindre än** hello värden du anger.  |

#### <a name="metric-measurement-alert-rules"></a>Mått mätning Varningsregler

| Egenskap | Beskrivning |
|:--- |:---|
| Samlat värde | Tröskelvärdet för att varje samlat värde i hello resultat får överstiga toobe anses vara ett intrång. |
| Utlösaren avisering baserat på | hello antal överträdelser för en avisering toobe skapas.  Du kan ange **Totalt antal överträdelser** för valfri kombination av överträdelser över hello resultat eller **på varandra följande överträdelser** toorequire som hello överträdelser måste ske i beräkningar. |

### <a name="actions"></a>Åtgärder
Varningsregler skapas alltid en [Varna post](#alert-records) när hello tröskelvärdet har uppnåtts.  Du kan också definiera en eller flera svar toobe kör till exempel e-post eller starta en runbook.



#### <a name="email-actions"></a>E-post-åtgärder
E-post-åtgärder skicka ett e-postmeddelande med hello information om hello avisering tooone eller flera mottagare.

| Egenskap | Beskrivning |
|:--- |:---|
| E-postavisering |Ange **Ja** om du vill att ett e-toobe skickas när hello avisering utlöses. |
| Ämne |Ämnesnamn i hello e-post.  Du kan inte ändra hello brödtext hello e-post. |
| mottagare |Tar med alla e-postmottagare.  Om du anger fler än en adress och sedan separat hello-adresser med ett semikolon (;). |

#### <a name="webhook-actions"></a>Webhook-åtgärder
Webhook-åtgärder kan du tooinvoke en extern process via en enkel HTTP POST-begäran.

| Egenskap | Beskrivning |
|:--- |:---|
| Webhook |Ange **Ja** om du vill toocall en webhook när hello avisering utlöses. |
| Webhooksadressen |hello-URL för hello webhooken. |
| Inkludera anpassad JSON-nyttolast |Välj det här alternativet om du vill tooreplace hello standard nyttolasten med en anpassad nyttolast. |
| Ange anpassad JSON-nyttolast |hello anpassad nyttolast för hello webhooken.  Se föregående avsnitt för mer information. |

#### <a name="runbook-actions"></a>Runbook-åtgärder
Runbook-åtgärder startar en runbook i Azure Automation. 

>[!NOTE]
> Du måste ha installerats i din arbetsyta för den här åtgärden toobe aktiverat hello Automation-lösningen. 


| Egenskap | Beskrivning |
|:--- |:---|
| Runbook | Ange **Ja** om du vill toostart en Azure Automation-runbook när hello avisering utlöses.  |
| Automation-konto | Anger hello Automation-konto som väljs från runbooks.  Detta är hello konto som har länkade toohello arbetsytan. |
| Välj en runbook | Välj hello runbook som du vill toostart när en avisering skapas. |
| Kör på | Välj **Azure** toorun hello runbook i hello molnet.  Välj **Hybrid worker** toorun hello runbook på en agent med [Hybrid Runbook Worker](../automation/automation-hybrid-runbook-worker.md ) installerad.  |




## <a name="next-steps"></a>Nästa steg
* Installera hello [avisering hanteringslösning](log-analytics-solution-alert-management.md) tooanalyze aviseringar som skapats i logganalys tillsammans med aviseringar som samlas in från System Center Operations Manager (SCOM).
* Läs mer om [logga sökningar](log-analytics-log-searches.md) som kan generera aviseringar.
* Slutföra en genomgång för [konfigurerar en webook](log-analytics-alerts-webhooks.md) med en aviseringsregel.  
* Lär dig hur toowrite [runbooks i Azure Automation](https://azure.microsoft.com/documentation/services/automation) tooremediate problem som identifieras av aviseringar.

