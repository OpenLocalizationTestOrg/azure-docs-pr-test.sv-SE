---
title: "aaaCalling en Azure Automation-Runbook från en Log Analytics avisering | Microsoft Docs"
description: "Den här artikeln innehåller en översikt över hur tooinvoke en Automation-runbook från en avisering om Microsoft OMS Log Analytics."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/31/2017
ms.author: magoedte
ms.openlocfilehash: 8b745d6e6c2b0294d676e010f52855cd51741cf9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="calling-an-azure-automation-runbook-from-an-oms-log-analytics-alert"></a>Anropa en Azure Automation-runbook från en OMS Log Analytics-avisering

När en avisering har konfigurerats i logganalys toocreate misslyckas en avisering om resultat som matchar vissa villkor, till exempel en långvarig topp i antal processorbelastning eller ett visst program processen kritiska toohello funktionerna i ett affärsprogram-post och skriver en motsvarande händelse hello i händelseloggen i Windows, kan den här aviseringen automatiskt köra en Automation-runbook i ett försök tooauto-reparera hello problemet.  

Det finns två alternativ toocall en runbook när du konfigurerar hello avisering.  Mer specifikt:

1. med en webhook.
   * Detta är hello enda tillgängliga alternativet om din OMS-arbetsyta inte är länkade tooan Automation-konto.
   * Det här alternativet är fortfarande tillgänglig om du redan har en Automation konto länkade tooan OMS-arbetsyta.  

2. Välj en runbook direkt.
   * Det här alternativet är tillgängligt endast när hello OMS-arbetsyta är länkade tooan Automation-konto.  

## <a name="calling-a-runbook-using-a-webhook"></a>Anropa en runbook med en webhook

En webhook kan du toostart en viss runbook i Azure Automation via en HTTP-begäran.  Innan du konfigurerar hello [logganalys avisering](../log-analytics/log-analytics-alerts.md#alert-rules) toocall hello runbook med hjälp av en webhook som en åtgärd måste toofirst skapa en webhook för hello-runbook som anropas med den här metoden.  Granska och följ hello stegen i hello [skapa en webhook](automation-webhooks.md#creating-a-webhook) artikel och komma ihåg toorecord hello Webhooksadressen så att du kan referera till det när du konfigurerar hello varningsregel.   

## <a name="calling-a-runbook-directly"></a>Anropa en runbook direkt

Om du har hello Automation & kontroll erbjudande installeras och konfigureras i OMS-arbetsyta när du konfigurerar hello Runbook åtgärder alternativet för hello aviseringen, kan du visa alla runbooks från hello **Välj en runbook** listrutan välja hello viss runbook du vill toorun i svaret toohello avisering.  hello markerad runbook kan köras i en arbetsyta i hello Azure-molnet eller på en hybrid runbook worker.  När hello avisering har skapats med alternativet för hello runbook, skapas en webhook för hello runbook.  Du kan se hello webhook om du går toohello Automation-konto och navigera toohello webhook-bladet för hello markerad runbook.  Om du tar bort hello avisering hello webhook tas inte bort men hello användaren kan ta bort hello webhook manuellt.  Det är inte ett problem om hello webhook inte tas bort, det är bara ett överblivna objekt som behöver slutligen toobe bort i ordning toomaintain en ordnad Automation-konto.  

## <a name="characteristics-of-a-runbook-for-both-options"></a>Egenskaper för en runbook (för båda alternativen)

Båda metoderna för att anropa hello runbook från hello logganalys avisering har olika beteenden egenskaper som behöver förstå innan du konfigurerar din Varningsregler toobe.  

* Du måste ha en runbook-indataparameter med namnet **WebhookData** som är **objekttypen**.  Det kan vara obligatoriskt eller valfritt.  hello avisering skickar hello sökresultat toohello runbook med hjälp av denna indataparameter.

        param  
         (  
          [Parameter (Mandatory=$true)]  
          [object] $WebhookData  
         )

*  Du måste ha koden tooconvert hello WebhookData tooa PowerShell-objektet.

    `$SearchResults = (ConvertFrom-Json $WebhookData.RequestBody).SearchResults.value`

    *$SearchResults* kommer att vara en matris med objekt; varje objekt innehåller hello fält med värden från en sökresultat

### <a name="webhookdata-inconsistencies-between-hello-webhook-option-and-runbook-option"></a>WebhookData inkonsekvenser mellan hello webhook och runbook

* När du konfigurerar en varning toocall en Webhook måste ange en Webhooksadressen som du har skapat för en runbook och klicka på hello **Test Webhook** knappen.  hello resulterande WebhookData skickas toohello runbook inte innehåller någon *. SearchResult* eller *. SearchResults*.

*  Om du sparar den här aviseringen när hello avisering utlöser och anropar hello webhooken WebhookData skickas toohello runbook hello innehåller *. SearchResult*.
* Om du skapar en avisering och konfigurera den toocall en runbook (som även skapar en webhook), när hello avisering utlösare skickar den WebhookData toohello runbook som innehåller *. SearchResults*.

Därför i hello kodexempel ovan, behöver du tooget *. SearchResult* om hello avisering anropar en webhook och behöver tooget *. SearchResults* om hello avisering direkt anropar en runbook.

## <a name="example-walkthrough"></a>Exempelgenomgång

Visar vi hur det fungerar med hjälp av hello följande exempel grafisk runbook, som startar en Windows-tjänst.<br><br> ![Starta Windows-tjänsten grafisk Runbook](media/automation-invoke-runbook-from-omsla-alert/automation-runbook-restartservice.png)<br>

Hej runbook har en indataparameter av typen **objekt** som kallas **WebhookData** samt hello webhook data som skickas från hello aviseringen som innehåller *. SearchResults*.<br><br> ![Indataparametrar för Runbook](media/automation-invoke-runbook-from-omsla-alert/automation-runbook-restartservice-inputparameter.png)<br>

I det här exemplet i logganalys vi skapat två anpassade fält, *SvcDisplayName_CF* och *SvcState_CF*, tooextract hello tjänstens namn och hello visningsläget för hello service (d.v.s. igång eller stoppad) skrivs toohello systemhändelseloggen från hello-händelsen.  Vi sedan skapa en aviseringsregel med hello följande sökfråga: `Type=Event SvcDisplayName_CF="Print Spooler" SvcState_CF="stopped"` så att vi kan identifiera när hello Utskriftshanteraren har stoppats på hello Windows-system.  Det kan vara någon tjänst av intresse, men det här exemplet vi refererar till en befintlig hello-tjänster som ingår i hello Windows-Operativsystemet.  hello Aviseringsåtgärd är konfigurerade tooexecute våra runbook i detta exempel används och körs på hello Hybrid Runbook Worker som är aktiverade på hello målsystem.   

Hej runbook-aktivitet som koden **hämta tjänstnamn från LA** hello JSON-formaterad sträng konverteras till en objekttyp och filtrera hello objektet *SvcDisplayName_CF* tooextract hello visningsnamn hello Windows-tjänst och skicka detta till hello nästa aktivitet som kontrollerar hello-tjänsten har stoppats innan du försöker toorestart den.  *SvcDisplayName_CF* är en [anpassat fält](../log-analytics/log-analytics-custom-fields.md) skapas i logganalys toodemonstrate det här exemplet.

    $SearchResults = (ConvertFrom-Json $WebhookData.RequestBody).SearchResults.value
    $SearchResults.SvcDisplayName_CF  

När hello-tjänsten stoppar hello varningsregeln i logganalys identifierar en matchning och utlösa hello runbook och skicka hello aviseringskontext toohello runbook. Hej runbook ska vidta åtgärder tooverify hello har stoppats och om så försök toorestart hello service och verifiera den startade korrekt och utdata hello resultatet.     

Du kan också om du inte har din Automation konto länkade tooyour OMS-arbetsyta du skulle konfigurera hello varningsregeln med en webhook åtgärd tootrigger hello runbook och konfigurera hello runbook tooconvert hello JSON-formaterad sträng och filter på *. SearchResult* följande hello riktlinjer som tidigare nämnts.    

## <a name="next-steps"></a>Nästa steg

* Mer om aviseringar i logganalys toolearn och hur toocreate, se [aviseringar i logganalys](../log-analytics/log-analytics-alerts.md).

* hur tootrigger runbooks med en webhook Se toounderstand [Azure Automation webhooks](automation-webhooks.md).
