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
# <a name="migrate-azure-alerts-on-management-events-tooactivity-log-alerts"></a><span data-ttu-id="c59c6-104">Migrera Azure aviseringar på management tooActivity loggen aviseringar</span><span class="sxs-lookup"><span data-stu-id="c59c6-104">Migrate Azure alerts on management events tooActivity Log alerts</span></span>


> [!WARNING]
> <span data-ttu-id="c59c6-105">Aviseringar om händelser för management inaktiveras från och med 1 oktober.</span><span class="sxs-lookup"><span data-stu-id="c59c6-105">Alerts on management events will be turned off on or after October 1.</span></span> <span data-ttu-id="c59c6-106">Använd hello anvisningarna nedan toounderstand om du har dessa aviseringar och migrera dem i så fall.</span><span class="sxs-lookup"><span data-stu-id="c59c6-106">Use hello directions below toounderstand if you have these alerts and migrate them if so.</span></span>
>
> 

## <a name="what-is-changing"></a><span data-ttu-id="c59c6-107">Vad ändras</span><span class="sxs-lookup"><span data-stu-id="c59c6-107">What is changing</span></span>

<span data-ttu-id="c59c6-108">Azure-Monitor (tidigare Azure insikter) erbjuds en kapaciteten toocreate en avisering utlöses från av hanteringshändelser som genererats meddelanden tooa webhook URL eller e-postadresser.</span><span class="sxs-lookup"><span data-stu-id="c59c6-108">Azure Monitor (formerly Azure Insights) offered a capability toocreate an alert that triggered off of management events and generated notifications tooa webhook URL or email addresses.</span></span> <span data-ttu-id="c59c6-109">Du har skapat en av dessa aviseringar något av följande sätt:</span><span class="sxs-lookup"><span data-stu-id="c59c6-109">You may have created one of these alerts any of these ways:</span></span>
* <span data-ttu-id="c59c6-110">I hello Azure-portalen för vissa typer av resurser under övervakning -> aviseringar -> Lägg till varning, där ”Varna om” anges för ”händelser”</span><span class="sxs-lookup"><span data-stu-id="c59c6-110">In hello Azure portal for certain resource types, under Monitoring -> Alerts -> Add Alert, where “Alert on” is set too“Events”</span></span>
* <span data-ttu-id="c59c6-111">Genom att köra hello Lägg till AzureRmLogAlertRule PowerShell-cmdlet</span><span class="sxs-lookup"><span data-stu-id="c59c6-111">By running hello Add-AzureRmLogAlertRule PowerShell cmdlet</span></span>
* <span data-ttu-id="c59c6-112">Med hjälp av direkt [hello avisering REST API](http://docs.microsoft.com/rest/api/monitor/alertrules) med odata.type = ”ManagementEventRuleCondition” och dataSource.odata.type = ”RuleManagementEventDataSource”</span><span class="sxs-lookup"><span data-stu-id="c59c6-112">By directly using [hello alert REST API](http://docs.microsoft.com/rest/api/monitor/alertrules) with odata.type = “ManagementEventRuleCondition” and dataSource.odata.type = “RuleManagementEventDataSource”</span></span>
 
<span data-ttu-id="c59c6-113">hello returnerar följande PowerShell-skript en lista över alla aviseringar för av hanteringshändelser som du har i din prenumeration, samt hello villkoren på varje avisering.</span><span class="sxs-lookup"><span data-stu-id="c59c6-113">hello following PowerShell script returns a list of all alerts on management events that you have in your subscription, as well as hello conditions set on each alert.</span></span>

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

<span data-ttu-id="c59c6-114">Om du har inga aviseringar för av hanteringshändelser hello PowerShell-cmdleten ovan kommer att skrivas ut en serie varningsmeddelanden som detta:</span><span class="sxs-lookup"><span data-stu-id="c59c6-114">If you have no alerts on management events, hello PowerShell cmdlet above will output a series of warning messages like this one:</span></span>

`WARNING: hello output of this cmdlet will be flattened, i.e. elimination of hello properties field, in a future release tooimprove hello user experience.`

<span data-ttu-id="c59c6-115">Dessa varningar kan ignoreras.</span><span class="sxs-lookup"><span data-stu-id="c59c6-115">These warning messages can be ignored.</span></span> <span data-ttu-id="c59c6-116">Om du har aviseringar om hanteringshändelser, ser hello resultatet av denna PowerShell-cmdlet ut så här:</span><span class="sxs-lookup"><span data-stu-id="c59c6-116">If you do have alerts on management events, hello output of this PowerShell cmdlet will look like this:</span></span>

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

<span data-ttu-id="c59c6-117">Varje avisering avgränsas med en streckad linje och innehåller hello resurs-ID för hello aviseringen och hello specifik regel som övervakas.</span><span class="sxs-lookup"><span data-stu-id="c59c6-117">Each alert is separated by a dashed line and details include hello resource ID of hello alert and hello specific rule being monitored.</span></span>

<span data-ttu-id="c59c6-118">Den här funktionen har gått över för[Azure övervaka aktiviteten loggen aviseringar](monitoring-activity-log-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="c59c6-118">This functionality has been transitioned too[Azure Monitor Activity Log Alerts](monitoring-activity-log-alerts.md).</span></span> <span data-ttu-id="c59c6-119">Dessa nya aviseringar kan du tooset ett villkor på aktivitetsloggen händelser och ett meddelande när en ny händelse matchar hello villkor.</span><span class="sxs-lookup"><span data-stu-id="c59c6-119">These new alerts enable you tooset a condition on Activity Log events and receive a notification when a new event matches hello condition.</span></span> <span data-ttu-id="c59c6-120">De erbjuder också flera förbättringar från aviseringar om hanteringshändelser:</span><span class="sxs-lookup"><span data-stu-id="c59c6-120">They also offer several improvements from alerts on management events:</span></span>
* <span data-ttu-id="c59c6-121">Du kan återanvända din grupp meddelandemottagare (”åtgärder”) över flera aviseringar via [åtgärdsgrupper](monitoring-action-groups.md), minska komplexiteten hello för att ändra vem som ska få en avisering.</span><span class="sxs-lookup"><span data-stu-id="c59c6-121">You can reuse your group of notification recipients (“actions”) across many alerts using [Action Groups](monitoring-action-groups.md), reducing hello complexity of changing who should receive an alert.</span></span>
* <span data-ttu-id="c59c6-122">Du kan få ett meddelande direkt på telefonen med hjälp av SMS med åtgärdsgrupper.</span><span class="sxs-lookup"><span data-stu-id="c59c6-122">You can receive a notification directly on your phone using SMS with Action Groups.</span></span>
* <span data-ttu-id="c59c6-123">Du kan [Skapa aktivitet Logga varningar med Resource Manager-mallar](monitoring-create-activity-log-alerts-with-resource-manager-template.md).</span><span class="sxs-lookup"><span data-stu-id="c59c6-123">You can [create Activity Log Alerts with Resource Manager templates](monitoring-create-activity-log-alerts-with-resource-manager-template.md).</span></span>
* <span data-ttu-id="c59c6-124">Du kan skapa villkor med större flexibilitet och komplexitet toomeet dina specifika behov.</span><span class="sxs-lookup"><span data-stu-id="c59c6-124">You can create conditions with greater flexibility and complexity toomeet your specific needs.</span></span>
* <span data-ttu-id="c59c6-125">Meddelanden levereras snabbare.</span><span class="sxs-lookup"><span data-stu-id="c59c6-125">Notifications are delivered more quickly.</span></span>
 
## <a name="how-toomigrate"></a><span data-ttu-id="c59c6-126">Hur toomigrate</span><span class="sxs-lookup"><span data-stu-id="c59c6-126">How toomigrate</span></span>
 
<span data-ttu-id="c59c6-127">en ny aktivitet loggen avisering toocreate, kan du antingen:</span><span class="sxs-lookup"><span data-stu-id="c59c6-127">toocreate a new Activity Log Alert, you can either:</span></span>
* <span data-ttu-id="c59c6-128">Följ [vår vägledning om hur toocreate en avisering i hello Azure-portalen](monitoring-activity-log-alerts.md)</span><span class="sxs-lookup"><span data-stu-id="c59c6-128">Follow [our guide on how toocreate an alert in hello Azure portal](monitoring-activity-log-alerts.md)</span></span>
* <span data-ttu-id="c59c6-129">Lär dig hur för[skapar en avisering med en Resource Manager-mall](monitoring-create-activity-log-alerts-with-resource-manager-template.md)</span><span class="sxs-lookup"><span data-stu-id="c59c6-129">Learn how too[create an alert using a Resource Manager template](monitoring-create-activity-log-alerts-with-resource-manager-template.md)</span></span>
 
<span data-ttu-id="c59c6-130">Aviseringar på management-händelser som du tidigare har skapat kommer inte att automatiskt migrerade tooActivity loggen aviseringar.</span><span class="sxs-lookup"><span data-stu-id="c59c6-130">Alerts on management events that you have previously created will not be automatically migrated tooActivity Log Alerts.</span></span> <span data-ttu-id="c59c6-131">Du måste toouse hello föregående PowerShell-skriptet toolist hello aviseringar på hanteringshändelser att du har konfigurerat och manuellt återskapa dem som aktiviteten loggen aviseringar.</span><span class="sxs-lookup"><span data-stu-id="c59c6-131">You need toouse hello preceding PowerShell script toolist hello alerts on management events that you currently have configured and manually recreate them as Activity Log Alerts.</span></span> <span data-ttu-id="c59c6-132">Detta måste göras innan 1 oktober efter aviseringar om händelser för management kommer inte längre att synas i din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="c59c6-132">This must be done before October 1, after which alerts on management events will no longer be visible in your Azure subscription.</span></span> <span data-ttu-id="c59c6-133">Andra typer av Azure aviseringar, inklusive Azure övervaka mått aviseringar, Programinsikter aviseringar och logganalys aviseringar påverkas inte av den här ändringen.</span><span class="sxs-lookup"><span data-stu-id="c59c6-133">Other types of Azure alerts, including Azure Monitor metric alerts, Application Insights alerts, and Log Analytics alerts are unaffected by this change.</span></span> <span data-ttu-id="c59c6-134">Om du har några frågor efter i hello kommentarer nedan.</span><span class="sxs-lookup"><span data-stu-id="c59c6-134">If you have any questions, post in hello comments below.</span></span>


## <a name="next-steps"></a><span data-ttu-id="c59c6-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c59c6-135">Next steps</span></span>

* <span data-ttu-id="c59c6-136">Lär dig mer om [aktivitetsloggen](monitoring-overview-activity-logs.md)</span><span class="sxs-lookup"><span data-stu-id="c59c6-136">Learn more about [Activity Log](monitoring-overview-activity-logs.md)</span></span>
* <span data-ttu-id="c59c6-137">Konfigurera [aktivitet loggen aviseringar via Azure-portalen](monitoring-activity-log-alerts.md)</span><span class="sxs-lookup"><span data-stu-id="c59c6-137">Configure [Activity Log Alerts via Azure portal](monitoring-activity-log-alerts.md)</span></span>
* <span data-ttu-id="c59c6-138">Konfigurera [aktivitet loggen aviseringar via Hanteraren för filserverresurser](monitoring-create-activity-log-alerts-with-resource-manager-template.md)</span><span class="sxs-lookup"><span data-stu-id="c59c6-138">Configure [Activity Log Alerts via Resource Manager](monitoring-create-activity-log-alerts-with-resource-manager-template.md)</span></span>
* <span data-ttu-id="c59c6-139">Granska hello [avisering webhook för aktivitetslogg](monitoring-activity-log-alerts-webhook.md)</span><span class="sxs-lookup"><span data-stu-id="c59c6-139">Review hello [activity log alert webhook schema](monitoring-activity-log-alerts-webhook.md)</span></span>
* <span data-ttu-id="c59c6-140">Lär dig mer om [tjänstmeddelanden](monitoring-service-notifications.md)</span><span class="sxs-lookup"><span data-stu-id="c59c6-140">Learn more about [Service Notifications](monitoring-service-notifications.md)</span></span>
* <span data-ttu-id="c59c6-141">Lär dig mer om [åtgärdsgrupper](monitoring-action-groups.md)</span><span class="sxs-lookup"><span data-stu-id="c59c6-141">Learn more about [Action Groups](monitoring-action-groups.md)</span></span>
