---
title: "Svar på aviseringar i OMS Log Analytics | Microsoft Docs"
description: "Aviseringar i Log Analytics kan identifiera viktig information i OMS-databasen och proaktivt meddelar dig om problem eller anropa åtgärder om du vill försöka åtgärda.  Den här artikeln beskriver hur du skapar en aviseringsregel och information om olika åtgärder som de kan ta."
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
ms.openlocfilehash: b8731e1fe48b7d809b113eb5273e3962542b8f34
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="add-actions-to-alert-rules-in-log-analytics"></a>Lägg till åtgärder i Varningsregler i logganalys
När en [avisering skapas i logganalys](log-analytics-alerts.md), har möjlighet att [konfigurera varningsregeln](log-analytics-alerts.md) att utföra en eller flera åtgärder.  Den här artikeln beskrivs olika åtgärder som är tillgängliga och information om hur du konfigurerar varje slag.

| Åtgärd | Beskrivning |
|:--|:--|
| [E-post](#email-actions) | Skicka ett e-postmeddelande med information om aviseringen till en eller flera mottagare. |
| [Webhook](#webhook-actions) | Anropa en extern process via en enkel HTTP POST-begäran. |
| [Runbook](#runbook-actions) | Starta en runbook i Azure Automation. |


## <a name="email-actions"></a>E-post-åtgärder
E-post-åtgärder skicka ett e-postmeddelande med information om aviseringen till en eller flera mottagare.  Du kan ange ämnet för e-post, men dess innehåll är ett standardformat genom logganalys.  Den innehåller översiktsinformation till exempel namnet för aviseringen förutom information om upp till tio poster som returneras av loggen sökningen.  Den innehåller också en länk till en logg sökning i logganalys som att returnera hela uppsättningen av poster från frågan.   Avsändaren av e-postmeddelandet är *Microsoft Operations Management Suite-teamet &lt; noreply@oms.microsoft.com &gt;* . 

E-post-åtgärder kräver egenskaperna i följande tabell.

| Egenskap | Beskrivning |
|:--- |:--- |
| Ämne |Ämne för e-postmeddelandet.  Du kan inte ändra innehållet i e-postmeddelandet. |
| mottagare |Tar med alla e-postmottagare.  Om du anger fler än en adress sedan Avgränsa adresserna med semikolon (;). |


## <a name="webhook-actions"></a>Webhook-åtgärder

Webhook-åtgärder kan du anropa en extern process via en enkel HTTP POST-begäran.  Tjänsten som anropas bör stöder webhooks och kontrollera hur den använder alla nyttolast tas emot.  Du kan också kontakta en REST-API som inte uttryckligen stöder webhooks som begäran finns i ett format som kan användas med API: et.  Exempel på användning av en webhook som svar på en avisering skickas ett meddelande [Slack](http://slack.com) eller skapa en incident i [PagerDuty](http://pagerduty.com/).  En fullständig genomgång för att skapa en aviseringsregel med en webhook att anropa Slack finns på [Webhooks i logganalys aviseringar](log-analytics-alerts-webhooks.md).

Webhook-åtgärder kräver egenskaperna i följande tabell.

| Egenskap | Beskrivning |
|:--- |:--- |
| Webhooksadressen |URL till webhooken. |
| Anpassad JSON-nyttolast |Anpassad nyttolast för att skicka med webhooken.  Mer information finns i nedan. |


Webhooks är en URL och en nyttolast som har formaterats i JSON som är data som skickas till externa-tjänsten.  Som standard innehåller nyttolasten värdena i tabellen nedan.  Du kan välja att ersätta den här nyttolasten med ett anpassat egen.  Du kan i så fall använda variabler i tabellen för var och en av parametrarna ta sina värdet i din anpassade nyttolast.

| Parameter | Variabel | Beskrivning |
|:--- |:--- |:--- |
| AlertRuleName |#alertrulename |Namnet på regeln. |
| AlertThresholdOperator |#thresholdoperator |Tröskelvärdet operator för regeln.  *Större än* eller *mindre än*. |
| AlertThresholdValue |#thresholdvalue |Tröskelvärde för regeln. |
| LinkToSearchResults |#linktosearchresults |Länka till logganalys loggen sökning som returnerar poster från frågan som skapade aviseringen. |
| ResultCount |#searchresultcount |Antalet poster i sökresultaten. |
| SearchIntervalEndtimeUtc |#searchintervalendtimeutc |Sluttid för frågan i UTC-format. |
| SearchIntervalInSeconds |#searchinterval |Tidsfönstret för regeln. |
| SearchIntervalStartTimeUtc |#searchintervalstarttimeutc |Starttid för frågan i UTC-format. |
| searchQuery |#searchquery |Loggen sökfråga används av regeln. |
| SearchResults |Nedan finns |Poster som returneras av frågan i JSON-format.  Begränsad till de första 5 000 posterna. |
| WorkspaceID |#workspaceid |ID för din OMS-arbetsyta. |

Du kan till exempel ange följande anpassad nyttolast som innehåller en enda parameter med namnet *text*.  Den tjänst som denna webhook anropar skulle förväntas den här parametern.

    {
        "text":"#alertrulename fired with #searchresultcount over threshold of #thresholdvalue."
    }

Nyttolasten i det här exemplet skulle matchas till något som liknar följande när du skickar webhooken.

    {
        "text":"My Alert Rule fired with 18 records over threshold of 10 ."
    }

Inkludera sökresultat i en anpassad nyttolast genom att lägga till följande rad som en egenskap för översta nivån i json-nyttolast.  

    "IncludeSearchResults":true

Om du vill skapa en anpassad nyttolast som innehåller bara aviseringsnamn och sökresultaten kan du exempelvis använda följande. 

    {
       "alertname":"#alertrulename",
       "IncludeSearchResults":true
    }


Du kan gå igenom en komplett exempel på hur du skapar en aviseringsregel med en webhook att starta en extern tjänst på [skapar en avisering webhook-åtgärd i OMS Log Analytics för att skicka meddelanden till Slack](log-analytics-alerts-webhooks.md).

## <a name="runbook-actions"></a>Runbook-åtgärder
Runbook-åtgärder startar en runbook i Azure Automation.  För att kunna använda den här typen av åtgärden, måste du ha den [automatiseringslösning](log-analytics-add-solutions.md) installeras och konfigureras i OMS-arbetsyta.  Du kan välja från runbooks i automation-kontot som du konfigurerade i Automation-lösningen.

Runbook-åtgärder kräver egenskaperna i följande tabell.

| Egenskap | Beskrivning |
|:--- |:---|
| Runbook | Runbook som du vill starta när en avisering skapas. |
| Kör på | Ange **Azure** att köra runbook i molnet.  Ange **Hybrid worker** att köra runbook på en agent med [Hybrid Runbook Worker](../automation/automation-hybrid-runbook-worker.md ) installerad.  |

Runbook-åtgärder starta en runbook med hjälp av en [webhook](../automation/automation-webhooks.md).  När du skapar varningsregeln automatiskt skapas en ny webhook för runbook med namnet **OMS avisering reparation** följt av ett GUID.  

Direkt kan du fylla i parametrar av runbook, men [$WebhookData parametern](../automation/automation-webhooks.md) innehåller information om aviseringen, inklusive resultaten av den logg som den skapades.  Runbook måste du definiera **$WebhookData** som en parameter att komma åt egenskaper för aviseringen.  Aviseringen data är tillgängliga i json-format i en enda egenskap som kallas **SearchResults** i den **RequestBody** -egenskapen för **$WebhookData**.  Detta har med egenskaper i följande tabell.

| Node | Beskrivning |
|:--- |:--- |
| id |Sökvägen och GUID för sökningen. |
| __metadata |Information om aviseringen, inklusive antalet poster och status i sökresultaten. |
| värde |Separat post för varje post i sökresultatet.  Information om transaktionen matchar egenskaperna och värdena för posten. |

Följande runbook skulle exempelvis extrahera de poster som returneras av loggen sökningen och tilldela olika egenskaper baserat på vilken typ av varje post.  Observera att startar runbook genom att konvertera **RequestBody** från json så att den kan bearbetas med som ett objekt i PowerShell.

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
- Lär dig hur du skriver [runbooks i Azure Automation](https://azure.microsoft.com/documentation/services/automation) att åtgärda problem som identifieras av aviseringar.