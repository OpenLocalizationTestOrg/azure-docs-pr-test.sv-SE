---
title: "aaaUser-initierad Azure Automation Runbook-åtgärden i Log Analytics | Microsoft Docs"
description: "Den här artikeln beskriver hur toorun en Automation-runbook från en logganalys söka resultat på begäran."
services: log-analytics
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: 
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/04/2017
ms.author: magoedte
ms.openlocfilehash: 53c25431572babd5fd54bf964e4683077e2a4c2d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="take-action-with-an-automation-runbook-from-a-log-analytics-log-search-result"></a>Utför en åtgärd med en Automation-Runbook från ett sökresultat för logganalys-logg

Loggen sökningen i Azure Log Analytics, du kan nu välja **åtgärda** toorun en Automation-runbook.  Hej runbook kan användas för tooremediate hello problemet eller vidta vissa åtgärder, till exempel samla in information om felsökning, skicka ett e-postmeddelande eller skapar en tjänstbegäran. 

## <a name="components-and-features-used"></a>Komponenter och funktioner som används
* [Azure Automation-konto](../automation/automation-offering-get-started.md)
* [Log Analytics-arbetsyta](../log-analytics/log-analytics-overview.md)

## <a name="tooinitiate-runbook-from-log-search"></a>tooinitiate runbook från loggen sökning

tootake åtgärd på en händelse och initiera en runbook i sökresultaten loggen, börja med att skapa en logg-sökning och du kan anropa en runbook på begäran från hello resultat.  Du kan göra detta från hello loggen sökfunktionen i hello Azure eller [OMS-portalen](../log-analytics/log-analytics-log-searches.md).  I det här exemplet söka vi loggen från hello Azure-portalen med en enkel demonstration av den här funktionen.

1. I hello Azure-portalen, hello hubb på menyn **fler tjänster** och välj **logganalys**.  
2. Hello logganalys bladet välj logganalys-arbetsytan och hello arbetsytan bladet välj **loggen Sök**.  
3. Hello loggen Sök bladet söka loggen.  
4. Hello loggen sökresultat, klicka på hello ellips toohello till vänster i ett hello fält och från hello popup-fönstret väljer **vidta åtgärder för**.<br><br> ![Välj Ta åtgärd sökresultat](./media/log-analytics-log-search-takeaction/log-search-takeaction-menuoption.png) 
5. Hello vidta åtgärden bladet välj **köra en runbook**, och på hello **köra en runbook** bladet kan du välja en runbook toorun.  Du kan välja valfri runbook i hello Automation-konto som är länkade toohello logganalys-arbetsytan.  Observera följande hello:

    * Runbooks är ordnade efter taggar
    * Runbook Indataparametern värden kan väljas direkt från hello fält i hello sökresultatet.  En listrutan visas alla tillgängliga hello-fält från hello resultatet tooselect från.  
    * Du kan välja toorun hello runbook på en [runbook worker-hybrid](../automation/automation-hybrid-runbook-worker.md) att du har installerat på hello-dator som har hello problemet om du har en motsvarande Hybrid Runbook Worker-grupp som endast innehåller den här datorn som en medlem.  Om hello Hybrid Worker-gruppen hello namn matchar hello namnet på hello-dator som är i hello loggen sökresultat, väljs hello gruppen automatiskt.    

6. När du klickar på **kör**, öppnas hello runbook-jobbet bladet tooallow du tooreview hello hello jobbets status.   

Om du väljer en runbook som var konfigurerad toobe [anropas från en avisering om logganalys](../automation/automation-invoke-runbook-from-omsla-alert.md), den har en indataparameter kallas **WebhookData** som **objekt** typen.  Om hello indataparameter är obligatorisk måste toopass hello sökresultat toohello runbook så att den kan konvertera hello JSON-formaterad sträng till objekttypen så att toofilter på särskilda objekt som du sedan hänvisar till i runbook-aktiviteter.  Det gör du genom att välja **sökresultatet (objekt)** hello nedrullningsbara listan.<br><br> ![Välj Webhook-dataobjekt för runbook-parameter](media/log-analytics-log-search-takeaction/select-runbook-and-properties.png)   
    
## <a name="next-steps"></a>Nästa steg

* Granska hello [logganalys logga Sök referens](log-analytics-search-reference.md) tooview alla hello söka fält och tillgängliga i logganalys-facets.
* hur tooinvoke en Automation-runbook automatiskt, granska toolearn [anropa en Azure Automation-runbook från en OMS logganalys-avisering](../automation/automation-invoke-runbook-from-omsla-alert.md).  
