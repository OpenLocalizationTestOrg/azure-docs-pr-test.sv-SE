---
title: aaaResponses tooalerts i OMS Log Analytics | Microsoft Docs
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
ms.date: 02/28/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d24bb726a96e7143985f111c0599dc4e7898b4f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-actions-tooalert-rules-in-log-analytics"></a>Lägg till åtgärder tooalert regler i logganalys
När en [avisering skapas i logganalys](log-analytics-alerts.md), har hello möjlighet att [varningsregel för att konfigurera hello](log-analytics-alerts.md) tooperform en eller flera åtgärder.  Den här artikeln beskriver hello olika åtgärder som är tillgängliga och information om hur du konfigurerar varje slag.

| Åtgärd | Beskrivning |
|:--|:--|
| [E-post](#email-actions) | Skicka ett e-postmeddelande med hello information om hello avisering tooone eller flera mottagare. |
| [Webhook](#webhook-actions) | Anropa en extern process via en enkel HTTP POST-begäran. |
| [Runbook](#runbook-actions) | Starta en runbook i Azure Automation. |


## <a name="email-actions"></a>E-post-åtgärder
E-post-åtgärder skicka ett e-postmeddelande med hello information om hello avisering tooone eller flera mottagare.  Du kan ange hello ämne hello e-post, men dess innehåll är ett standardformat genom logganalys.  Tillägg toodetails av upp tooten poster som returneras av hello loggen sökningen innehåller översiktsinformation, till exempel hello namn för hello avisering.  Den innehåller också en länk tooa loggen sökning i logganalys som returnerar hello hela uppsättningen av poster från frågan.   hello avsändaren hello e-post är *Microsoft Operations Management Suite-teamet &lt; noreply@oms.microsoft.com &gt;* . 

E-post-åtgärder kräver hello egenskaper i hello i den följande tabellen.

| Egenskap | Beskrivning |
|:--- |:--- |
| Ämne |Ämnesnamn i hello e-post.  Du kan inte ändra hello brödtext hello e-post. |
| mottagare |Tar med alla e-postmottagare.  Om du anger fler än en adress och sedan separat hello-adresser med ett semikolon (;). |


## <a name="webhook-actions"></a>Webhook-åtgärder

Webhook-åtgärder kan du tooinvoke en extern process via en enkel HTTP POST-begäran.  hello-tjänsten som anropas bör stöder webhooks och kontrollera hur den använder alla nyttolast tas emot.  Du kan också kontakta en REST-API som inte uttryckligen stöder webhooks så länge hello-begäran är i ett format som hello API förstår.  Exempel på användning av en webhook i svaret tooan avisering skickar ett meddelande i [Slack](http://slack.com) eller skapa en incident i [PagerDuty](http://pagerduty.com/).  En fullständig genomgång för att skapa en aviseringsregel med en webhook toocall Slack finns på [Webhooks i logganalys aviseringar](log-analytics-alerts-webhooks.md).

Webhook-åtgärder kräver hello egenskaper i hello i den följande tabellen.

| Egenskap | Beskrivning |
|:--- |:--- |
| Webhooksadressen |hello-URL för hello webhooken. |
| Anpassad JSON-nyttolast |Anpassad nyttolast toosend med hello webhooken.  Mer information finns i nedan. |


Webhooks lägga till en URL och en nyttolast som har formaterats i JSON som är hello data skickas toohello extern tjänst.  Som standard innehåller hello nyttolasten hello värden i den följande tabellen hello.  Du kan välja den här nyttolasten med en anpassad egen tooreplace.  I så fall kan du använda hello variabler i hello tabellen för varje hello parametrar tooinclude deras värde i din anpassade nyttolast.

| Parameter | Variabel | Beskrivning |
|:--- |:--- |:--- |
| AlertRuleName |#alertrulename |Namnet på hello varningsregel. |
| AlertThresholdOperator |#thresholdoperator |Tröskelvärdet operator för hello varningsregel.  *Större än* eller *mindre än*. |
| AlertThresholdValue |#thresholdvalue |Tröskelvärde för hello varningsregel. |
| LinkToSearchResults |#linktosearchresults |Länka tooLog Analytics loggen sökning som returnerar hello poster från hello-fråga som skapade hello avisering. |
| ResultCount |#searchresultcount |Antalet poster i hello sökresultat. |
| SearchIntervalEndtimeUtc |#searchintervalendtimeutc |Sluttid för hello frågan i UTC-format. |
| SearchIntervalInSeconds |#searchinterval |Tidsfönstret för hello varningsregel. |
| SearchIntervalStartTimeUtc |#searchintervalstarttimeutc |Starttid för hello frågan i UTC-format. |
| searchQuery |#searchquery |Loggen sökfråga används av hello varningsregel. |
| SearchResults |Nedan finns |Poster som returneras av hello frågan i JSON-format.  Begränsad toohello första 5 000 poster. |
| WorkspaceID |#workspaceid |ID för din OMS-arbetsyta. |

Du kan till exempel ange hello följande anpassad nyttolast som innehåller en enda parameter med namnet *text*.  hello-tjänst som denna webhook anropar skulle förväntas den här parametern.

    {
        "text":"#alertrulename fired with #searchresultcount over threshold of #thresholdvalue."
    }

Det här exemplet nyttolast löser toosomething som hello efter när skickas toohello webhooken.

    {
        "text":"My Alert Rule fired with 18 records over threshold of 10 ."
    }

tooinclude sökresultat i en anpassad nyttolast Lägg till följande rad som en egenskap för översta nivån i hello json-nyttolast hello.  

    "IncludeSearchResults":true

Till exempel toocreate en anpassad nyttolast som innehåller bara hello aviseringsnamn och hello sökresultat, du kan använda följande hello. 

    {
       "alertname":"#alertrulename",
       "IncludeSearchResults":true
    }


Du kan gå igenom en komplett exempel på hur du skapar en aviseringsregel med en webhook toostart en extern tjänst på [skapar en avisering webhook-åtgärd i OMS logganalys toosend meddelandet tooSlack](log-analytics-alerts-webhooks.md).

## <a name="runbook-actions"></a>Runbook-åtgärder
Runbook-åtgärder startar en runbook i Azure Automation.  Ordna toouse denna typ av åtgärd måste du ha hello [automatiseringslösning](log-analytics-add-solutions.md) installeras och konfigureras i OMS-arbetsyta.  Du kan välja mellan hello runbooks i hello automation-konto som du konfigurerade i hello Automation-lösningen.

Runbook-åtgärder kräver hello egenskaper i hello i den följande tabellen.

| Egenskap | Beskrivning |
|:--- |:---|
| Runbook | Runbook som du vill toostart när en avisering skapas. |
| Kör på | Ange **Azure** toorun hello runbook i hello molnet.  Ange **Hybrid worker** toorun hello runbook på en agent med [Hybrid Runbook Worker](../automation/automation-hybrid-runbook-worker.md ) installerad.  |

Runbook-åtgärder starta hello runbook med hjälp av en [webhook](../automation/automation-webhooks.md).  När du skapar hello varningsregeln automatiskt skapas en ny webhook för hello runbook med namnet hello **OMS avisering reparation** följt av ett GUID.  

Du kan inte direkt fylla eventuella parametrar för hello runbook, men hello [$WebhookData parametern](../automation/automation-webhooks.md) innehåller hello information om hello aviseringen, inklusive hello resultaten av hello logg som den skapades.  Hej runbook behöver toodefine **$WebhookData** som en parameter för den tooaccess hello egenskaper för hello avisering.  hello aviseringsdata är tillgänglig i json-format i en enda egenskap som kallas **SearchResults** i hello **RequestBody** -egenskapen för **$WebhookData**.  Detta har med hello egenskaper i hello i den följande tabellen.

| Node | Beskrivning |
|:--- |:--- |
| id |Sökvägen och GUID hello sökning. |
| __metadata |Information om hello avisering inklusive hello antalet poster och status för hello sökresultat. |
| värde |Separat post för varje post i hello sökresultat.  hello information om hello transaktionen matchar hello egenskaper och värden för hello-post. |

Till exempel skulle hello följande runbook extrahera hello-poster som returneras av hello loggen Sök och tilldela olika egenskaper utifrån hello typ av varje post.  Observera att hello runbook startas genom att konvertera **RequestBody** från json så att den kan bearbetas med som ett objekt i PowerShell.

    param ( 
        [object]$WebhookData
    )

    $RequestBody = ConvertFrom-JSON -InputObject $WebhookData.RequestBody
    $Records     = $RequestBody.SearchResults.value

    foreach ($Record in $Records)
    {
        $Computer = $Record.Computer

        if ($Record.Type -eq 'Event')
        {
            $EventNo    = $Record.EventID
            $EventLevel = $Record.EventLevelName
            $EventData  = $Record.EventData
        }

        if ($Record.Type -eq 'Perf')
        {
            $Object    = $Record.ObjectName
            $Counter   = $Record.CounterName
            $Instance  = $Record.InstanceName
            $Value     = $Record.CounterValue
        }
    }


## <a name="next-steps"></a>Nästa steg
- Slutföra en genomgång för [konfigurerar en webook](log-analytics-alerts-webhooks.md) med en aviseringsregel.  
- Lär dig hur toowrite [runbooks i Azure Automation](https://azure.microsoft.com/documentation/services/automation) tooremediate problem som identifieras av aviseringar.
