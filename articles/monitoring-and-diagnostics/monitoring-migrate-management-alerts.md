---
title: "aaaMigrate Azure aviseringar på hanteringshändelser tooActivity loggen aviseringar | Microsoft Docs"
description: "Aviseringar om management-händelser tas bort på 1 oktober. Förbereda genom att migrera befintliga aviseringar."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: johnkem
ms.openlocfilehash: e00bc4f0bad4e8f97443310770c333d250e343ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-azure-alerts-on-management-events-tooactivity-log-alerts"></a>Migrera Azure aviseringar på management tooActivity loggen aviseringar


> [!WARNING]
> Aviseringar om händelser för management inaktiveras från och med 1 oktober. Använd hello anvisningarna nedan toounderstand om du har dessa aviseringar och migrera dem i så fall.
>
> 

## <a name="what-is-changing"></a>Vad ändras

Azure-Monitor (tidigare Azure insikter) erbjuds en kapaciteten toocreate en avisering utlöses från av hanteringshändelser som genererats meddelanden tooa webhook URL eller e-postadresser. Du har skapat en av dessa aviseringar något av följande sätt:
* I hello Azure-portalen för vissa typer av resurser under övervakning -> aviseringar -> Lägg till varning, där ”Varna om” anges för ”händelser”
* Genom att köra hello Lägg till AzureRmLogAlertRule PowerShell-cmdlet
* Med hjälp av direkt [hello avisering REST API](http://docs.microsoft.com/rest/api/monitor/alertrules) med odata.type = ”ManagementEventRuleCondition” och dataSource.odata.type = ”RuleManagementEventDataSource”
 
hello returnerar följande PowerShell-skript en lista över alla aviseringar för av hanteringshändelser som du har i din prenumeration, samt hello villkoren på varje avisering.

```powershell
Login-AzureRmAccount
$alerts = $null
foreach ($rg in Get-AzureRmResourceGroup ) {
  $alerts += Get-AzureRmAlertRule -ResourceGroup $rg.ResourceGroupName
}
foreach ($alert in $alerts) {
  if($alert.Properties.Condition.DataSource.GetType().Name.Equals("RuleManagementEventDataSource")) {
    "Alert Name: " + $alert.Name
    "Alert Resource ID: " + $alert.Id
    "Alert conditions:"
    $alert.Properties.Condition.DataSource
    "---------------------------------"
  }
} 
```

Om du har inga aviseringar för av hanteringshändelser hello PowerShell-cmdleten ovan kommer att skrivas ut en serie varningsmeddelanden som detta:

`WARNING: hello output of this cmdlet will be flattened, i.e. elimination of hello properties field, in a future release tooimprove hello user experience.`

Dessa varningar kan ignoreras. Om du har aviseringar om hanteringshändelser, ser hello resultatet av denna PowerShell-cmdlet ut så här:

```
Alert Name: webhookEvent1
Alert Resource ID: /subscriptions/<subscription-id>/resourceGroups/<resourcegroup-name>/providers/microsoft.insights/alertrules/webhookEvent1
Alert conditions:

EventName            : 
EventSource          : 
Level                : 
OperationName        : microsoft.web/sites/start/action
ResourceGroupName    : 
ResourceProviderName : 
Status               : succeeded
SubStatus            : 
Claims               : Microsoft.Azure.Management.Monitor.Management.Models.RuleManagementEventClaimsDataSource
ResourceUri          : /subscriptions/<subscription-id>/resourceGroups/<resourcegroup-name>/providers/Microsoft.Web/sites/samplealertapp

---------------------------------
Alert Name: someclilogalert
Alert Resource ID: /subscriptions/<subscription-id>/resourceGroups/<resourcegroup-name>/providers/microsoft.insights/alertrules/someclilogalert
Alert conditions:

EventName            : 
EventSource          : 
Level                : 
OperationName        : Start
ResourceGroupName    : 
ResourceProviderName : 
Status               : 
SubStatus            : 
Claims               : Microsoft.Azure.Management.Monitor.Management.Models.RuleManagementEventClaimsDataSource
ResourceUri          : /subscriptions/<subscription-id>/resourceGroups/<resourcegroup-name>/providers/Microsoft.Compute/virtualMachines/Seaofclouds

---------------------------------
```

Varje avisering avgränsas med en streckad linje och innehåller hello resurs-ID för hello aviseringen och hello specifik regel som övervakas.

Den här funktionen har gått över för[Azure övervaka aktiviteten loggen aviseringar](monitoring-activity-log-alerts.md). Dessa nya aviseringar kan du tooset ett villkor på aktivitetsloggen händelser och ett meddelande när en ny händelse matchar hello villkor. De erbjuder också flera förbättringar från aviseringar om hanteringshändelser:
* Du kan återanvända din grupp meddelandemottagare (”åtgärder”) över flera aviseringar via [åtgärdsgrupper](monitoring-action-groups.md), minska komplexiteten hello för att ändra vem som ska få en avisering.
* Du kan få ett meddelande direkt på telefonen med hjälp av SMS med åtgärdsgrupper.
* Du kan [Skapa aktivitet Logga varningar med Resource Manager-mallar](monitoring-create-activity-log-alerts-with-resource-manager-template.md).
* Du kan skapa villkor med större flexibilitet och komplexitet toomeet dina specifika behov.
* Meddelanden levereras snabbare.
 
## <a name="how-toomigrate"></a>Hur toomigrate
 
en ny aktivitet loggen avisering toocreate, kan du antingen:
* Följ [vår vägledning om hur toocreate en avisering i hello Azure-portalen](monitoring-activity-log-alerts.md)
* Lär dig hur för[skapar en avisering med en Resource Manager-mall](monitoring-create-activity-log-alerts-with-resource-manager-template.md)
 
Aviseringar på management-händelser som du tidigare har skapat kommer inte att automatiskt migrerade tooActivity loggen aviseringar. Du måste toouse hello föregående PowerShell-skriptet toolist hello aviseringar på hanteringshändelser att du har konfigurerat och manuellt återskapa dem som aktiviteten loggen aviseringar. Detta måste göras innan 1 oktober efter aviseringar om händelser för management kommer inte längre att synas i din Azure-prenumeration. Andra typer av Azure aviseringar, inklusive Azure övervaka mått aviseringar, Programinsikter aviseringar och logganalys aviseringar påverkas inte av den här ändringen. Om du har några frågor efter i hello kommentarer nedan.


## <a name="next-steps"></a>Nästa steg

* Lär dig mer om [aktivitetsloggen](monitoring-overview-activity-logs.md)
* Konfigurera [aktivitet loggen aviseringar via Azure-portalen](monitoring-activity-log-alerts.md)
* Konfigurera [aktivitet loggen aviseringar via Hanteraren för filserverresurser](monitoring-create-activity-log-alerts-with-resource-manager-template.md)
* Granska hello [avisering webhook för aktivitetslogg](monitoring-activity-log-alerts-webhook.md)
* Lär dig mer om [tjänstmeddelanden](monitoring-service-notifications.md)
* Lär dig mer om [åtgärdsgrupper](monitoring-action-groups.md)
