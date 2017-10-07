---
title: aaaOverview av aviseringar i Microsoft Azure och Azure-Monitor | Microsoft Docs
description: "Aviseringar kan du toomonitor Azure-resurs mått, händelser eller loggar och aviseras när ett angivet villkor uppfylls."
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: a6dea224-57bf-43d8-a292-06523037d70b
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: robb
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d080080666fedc9eefda6b35cdea3714785d2223
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-are-alerts-in-microsoft-azure"></a>Vad är aviseringar i Microsoft Azure?
Den här artikeln beskriver hello olika källor för aviseringar i Microsoft Azure, vilka är hello syften för dessa aviseringar, sina fördelar och hur tooget igång med att använda dem. Den specifikt gäller tooAzure Övervakare, men ger pekare tooother tjänster samt aviseringar. Aviseringar erbjuder en metod för att övervaka i Azure som du kan tooconfigure villkor över data och bli meddelad när hello villkor matchar hello senaste övervakningsdata.

## <a name="taxonomy-of-azure-alerts"></a>Taxonomi för Azure-aviseringar
Azure använder hello följande villkor toodescribe aviseringar och deras funktioner:
* **Varning** -en definition av kriterier (en eller flera regler eller villkor) som blir aktiveras när uppfylls.
* **Aktiva** - hello tillstånd när hello kriterier som definieras av en avisering är uppfyllt.
* **Matcha** - hello tillstånd när hello kriterier som definieras av en avisering är inte längre uppfyllt efter tidigare är uppfyllda.
* **Meddelande** -hello åtgärd baserat på en avisering som blir aktiv.
* **Åtgärden** -en specifik anrop skickas tooa mottagare av ett meddelande (till exempel e-posta en adress eller bokföring tooa Webhooksadressen). Meddelanden kan vanligtvis utlösa flera åtgärder.

## <a name="alerts-in-different-azure-services"></a>Aviseringar i olika Azure-tjänster
Aviseringar är tillgänglig över flera Azure övervaka tjänster. Mer information om hur och när toouse dessa tjänster, [finns i den här artikeln](./monitoring-overview.md). Här är en uppdelning av hello aviseringstyper i Azure:

| Tjänst | Typ av avisering | Tjänster som stöds | Beskrivning |
|---|---|---|---|
| Azure Monitor | [Mått aviseringar](./insights-alerts-portal.md) | [Stöds mått från Azure-Monitor](./monitoring-supported-metrics.md) | Ett meddelande när någon plattform nivå mått uppfyller ett visst villkor (till exempel processor i procent på en virtuell dator är större än 90 för hello senaste 5 minuter). |
| Azure Monitor | [Aktiviteten loggen aviseringar](./monitoring-activity-log-alerts.md) | Alla resurstyper som är tillgängliga i Azure Resource Manager | Få en avisering när någon ny händelse i hello [Azure-aktivitetsloggen](./monitoring-overview-activity-logs.md) matchar vissa villkor (till exempel när en åtgärd för ”ta bort virtuell dator” uppstår i myProductionResourceGroup eller när en ny händelse tjänstens hälsa med ”aktiv” som hello status visas). |
| Application Insights | [Mått aviseringar](../application-insights/app-insights-alerts.md) | Alla program instrumenterats toosend data tooApplication insikter | Ett meddelande när någon på programnivå mått uppfyller ett visst villkor (till exempel serversvarstid är större än 2 sekunder). |
| Application Insights | [Web test varningar](../application-insights/app-insights-monitor-web-app-availability.md) | Alla webbplatser instrumenterats toosend data tooApplication insikter | Få ett meddelande när tillgänglighet och svarstider på en webbplats som är lägre än förväntningar. |
| Log Analytics | [Log Analytics aviseringar](../log-analytics/log-analytics-alerts.md) | Alla tjänster som konfigurerats toosend data i logganalys | Få ett meddelande när en logganalys sökfråga över mått och/eller händelse data uppfyller vissa villkor. |

## <a name="alerts-on-azure-monitor-data"></a>Aviseringar på Azure-Monitor data
Det finns två typer av aviseringar från data från Azure-Monitor--mått aviseringar och aktivitetsloggen aviseringar.

* **Mått aviseringar** -den här aviseringen utlöses när hello-värdet för ett visst mått överskrider ett tröskelvärde som du tilldelar. hello aviseringen genererar en avisering när hello aviseringen är ”aktiverad” (när hello tröskelvärde uppnås och hello aviseringen är uppfyllt) som när den är ”löst” (när hello tröskelvärdet skärs igen och hello är inte längre uppfyllt). En växande lista över tillgängliga mått som stöds av Azure-monitor finns [listan över mått stöds på Azure-Monitor](monitoring-supported-metrics.md).
* **Loggen Aktivitetsaviseringar** -en strömmande log-avisering som utlöses när en aktivitetsloggen-händelse genereras som matchar filtervillkor som du har tilldelat. Dessa aviseringar har endast en tillstånd ”aktiverad” eftersom hello avisering motorn gäller bara hello filter kriterier tooany ny händelse. Dessa aviseringar kan vara används toobecome meddelas när en ny incident tjänstens hälsa inträffar eller när en användare eller program utför en åtgärd i din prenumeration, till exempel ”ta bort virtuell dator”.

För diagnostiska loggdata som är tillgängliga via Azure-Monitor föreslår vi routning hello data till logganalys och använder en Log Analytics-avisering. hello sammanfattar följande diagram datakällor för data i Azure-Monitor och, begreppsmässigt, hur du kan ge ut från dessa data.

![Aviseringar som förklaras](./media/monitoring-overview-alerts/Alerts_Overview_Resource_v4.png)

## <a name="how-do-i-receive-a-notification-on-an-azure-monitor-alert"></a>Hur får ett meddelande på en Azure-Monitor alert?
Tidigare använda Azure aviseringar från olika tjänster sina egna inbyggda metoder. Azure-Monitor erbjuder framöver, en återanvändbara notification gruppering som kallas åtgärdsgrupper. Åtgärdsgrupper ange mottagare för ett meddelande--valfritt antal e-postadresser, telefonnummer (för SMS) eller webhook webbadresser-- och när en avisering aktiveras att referenser hello grupp, alla mottagare få detta meddelande. Detta ger tooreuse en gruppering av mottagarna (till exempel på anrop tekniker listan) över många objekt som aviseringen. För närvarande bara aktivitetsloggen aviseringar genom att utnyttja åtgärdsgrupper, men flera andra Azure aviseringstyper arbetar med hjälp av åtgärdsgrupper samt.

Åtgärdsgrupper har stöd för avisering genom att skicka tooa Webhooksadressen i tillägget tooemail adresser och SMS-nummer. Detta kan till exempel automation och reparationer med:
    - Azure Automation – Runbook
    - Azure-funktion
    - Azure Logikapp
    - en tjänst från tredje part

Mått aviseringar använder inte ännu åtgärdsgrupper. Du kan konfigurera meddelanden till på en enskild mått varning:
* Skicka e-post meddelanden toohello administratör, tooco-administratörer eller tooadditional e-postadresser som du anger.
* Anropa webhook, vilket gör att du toolaunch ytterligare automation-åtgärder.

## <a name="next-steps"></a>Nästa steg
Hämta information om Varningsregler och konfigurerar dem med hjälp av:

* Lär dig mer om [mått](monitoring-overview-metrics.md)
* Konfigurera [mått aviseringar via Azure-portalen](insights-alerts-portal.md)
* Konfigurera [mått aviseringar PowerShell](insights-alerts-powershell.md)
* Konfigurera [mått aviseringar kommandoradsgränssnittet (CLI)](insights-alerts-command-line-interface.md)
* Konfigurera [mått aviseringar Azure övervakaren REST-API](https://msdn.microsoft.com/library/azure/dn931945.aspx)
* Lär dig mer om [aktivitetsloggen](monitoring-overview-activity-logs.md)
* Konfigurera [aktivitet loggen aviseringar via Azure-portalen](monitoring-activity-log-alerts.md)
* Konfigurera [aktivitet loggen aviseringar via Hanteraren för filserverresurser](monitoring-create-activity-log-alerts-with-resource-manager-template.md)
* Granska hello [avisering webhook för aktivitetslogg](monitoring-activity-log-alerts-webhook.md)
* Lär dig mer om [tjänstmeddelanden](monitoring-service-notifications.md)
* Lär dig mer om [åtgärdsgrupper](monitoring-action-groups.md)
