---
title: "aaaAzure övervakaren PowerShell Snabbstart-exempel. | Microsoft Docs"
description: "Använd PowerShell tooaccess Azure-Monitor-funktioner, till exempel Autoskala, aviseringar, webhooks och söka aktivitetsloggar."
author: kamathashwin
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: c0761814-7148-4ab5-8c27-a2c9fa4cfef5
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: ashwink
ms.openlocfilehash: 6eece0b0227e0bbf08225bd330d0601169911f55
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-monitor-powershell-quick-start-samples"></a>Azure-Monitor PowerShell Snabbstart-exempel
Den här artikeln innehåller exempel på PowerShell-kommandon toohelp du komma åt Azure-Monitor-funktioner. Azure övervakaren kan tooAutoScale molntjänster, virtuella datorer, och Web Apps och toosend aviseringsmeddelanden eller anropet webbadresser som baseras på värden för konfigurerade telemetridata.

> [!NOTE]
> Azure övervakaren är hello nytt namn för vad anropades ”Azure Insights” förrän den 25 september 2016. Hello namnområden och därmed hello följande kommandon fortfarande innehåller dock insikter ”hello”.
> 
> 

## <a name="set-up-powershell"></a>Konfigurera PowerShell
Om du inte redan gjort ställa in PowerShell toorun på datorn. Mer information finns i [hur tooInstall och konfigurera PowerShell](/powershell/azure/overview).

## <a name="examples-in-this-article"></a>Exemplen i den här artikeln
hello exemplen i hello artikeln visar hur du kan använda Azure-Monitor-cmdlets. Du kan också granska hello hela listan med Azure-Monitor PowerShell-cmdlets på [Azure-Monitor (insikter) Cmdlets](https://msdn.microsoft.com/library/azure/mt282452#40v=azure.200#41.aspx).

## <a name="sign-in-and-use-subscriptions"></a>Logga in och använda prenumerationer
Logga först in tooyour Azure-prenumeration.

```PowerShell
Login-AzureRmAccount
```

Detta kräver toosign i. När du gör ditt konto, visas TenantID och standard prenumerations-ID. Alla hello Azure-cmdlets arbete i prenumerationen standard hello kontext. tooview hello listan över prenumerationer som du har åtkomst till, Använd hello följande kommando.

```PowerShell
Get-AzureRmSubscription
```

toochange arbeta kontexten tooa olika prenumerationen, Använd hello följande kommando.

```PowerShell
Set-AzureRmContext -SubscriptionId <subscriptionid>
```


## <a name="retrieve-activity-log-for-a-subscription"></a>Hämta aktivitetsloggen för en prenumeration
Använd hello `Get-AzureRmLog` cmdlet.  hello nedan följer några vanliga exempel.

Hämta loggposter från denna tid/datum toopresent:

```PowerShell
Get-AzureRmLog -StartTime 2016-03-01T10:30
```

Hämta loggposter ett värdeområde tid/datum:

```PowerShell
Get-AzureRmLog -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Hämta loggposter från en viss resursgrupp:

```PowerShell
Get-AzureRmLog -ResourceGroup 'myrg1'
```

Hämta loggposter från en viss resurs-leverantör ett värdeområde tid/datum:

```PowerShell
Get-AzureRmLog -ResourceProvider 'Microsoft.Web' -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Hämta alla loggposter med en specifik anropare:

```PowerShell
Get-AzureRmLog -Caller 'myname@company.com'
```

följande kommando hämtar hello senaste 1 000 händelser från hello aktivitetsloggen hello:

```PowerShell
Get-AzureRmLog -MaxEvents 1000
```

`Get-AzureRmLog`har stöd för många parametrar. Se hello `Get-AzureRmLog` referens för mer information.

> [!NOTE]
> `Get-AzureRmLog`endast ger 15 dagar tidigare. Med hjälp av hello **- MaxEvents** parametern kan du tooquery hello sista N händelser, utöver 15 dagar. tooaccess händelser som är äldre än 15 dagar använda hello REST API eller SDK (C# exempel med hjälp av hello SDK). Om du inte inkluderar **StartTime**, då är standardvärdet för hello **EndTime** minus en timme. Om du inte inkluderar **EndTime**, och sedan hello standardvärdet är aktuell tid. Det finns alltid i UTC.
> 
> 

## <a name="retrieve-alerts-history"></a>Hämta aviseringar historik
tooview alla Varna händelser, du kan fråga hello Azure Resource Manager loggar med hello följande exempel.

```PowerShell
Get-AzureRmLog -Caller "Microsoft.Insights/alertRules" -DetailedOutput -StartTime 2015-03-01
```

tooview hello historik för en specifik avisering regel, kan du använda hello `Get-AzureRmAlertHistory` cmdlet, skicka i hello resurs-ID för hello varningsregel.

```PowerShell
Get-AzureRmAlertHistory -ResourceId /subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/alertrules/myalert -StartTime 2016-03-1 -Status Activated
```

Hej `Get-AzureRmAlertHistory` cmdlet har stöd för olika parametrar. Mer information finns i [Get-AlertHistory](https://msdn.microsoft.com/library/mt282453.aspx).

## <a name="retrieve-information-on-alert-rules"></a>Hämta information om Varningsregler
Alla hello följande kommandon fungerar på en resursgrupp med namnet ”montest”.

Visa alla hello egenskaper för hello varningsregeln:

```PowerShell
Get-AzureRmAlertRule -Name simpletestCPU -ResourceGroup montest -DetailedOutput
```

Hämta alla aviseringar på en resursgrupp:

```PowerShell
Get-AzureRmAlertRule -ResourceGroup montest
```

Hämta alla Varningsregler för en målresurs. Till exempel ange alla Varningsregler på en virtuell dator.

```PowerShell
Get-AzureRmAlertRule -ResourceGroup montest -TargetResourceId /subscriptions/s1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig
```

`Get-AzureRmAlertRule`har stöd för andra parametrar. Se [Get-AlertRule](https://msdn.microsoft.com/library/mt282459.aspx) för mer information.

## <a name="create-metric-alerts"></a>Skapa mått aviseringar
Du kan använda hello `Add-AlertRule` cmdlet toocreate, uppdatera eller inaktivera en aviseringsregel.

Du kan skapa e-post och webhook-egenskaper med `New-AzureRmAlertRuleEmail` och `New-AzureRmAlertRuleWebhook`respektive. Tilldela dessa som åtgärder toohello i hello varningsregeln cmdlet **åtgärder** -egenskapen för hello varningsregel.

hello i den följande tabellen beskrivs hello parametrar och värden används toocreate en avisering med ett mått.

| Parametern | värde |
| --- | --- |
| Namn |simpletestdiskwrite |
| Platsen för den här varningsregeln |Östra USA |
| ResourceGroup |montest |
| TargetResourceId |/subscriptions/S1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig |
| MetricName hello varning som skapas |\PhysicalDisk (_Total) \Disk Diskskrivningar/sek. Se hello `Get-MetricDefinitions` cmdlet om hur tooretrieve hello exakta mått namn |
| Operatorn |GreaterThan |
| Tröskelvärde (antal per sekund i för det här måttet) |1 |
| Fönsterstorlek (format: mm: ss) |00:05:00 |
| Aggregator (statistik för hello måttet, som använder Genomsnittligt antal i det här fallet) |Genomsnittlig |
| anpassad e-postmeddelanden (Strängmatrisen) |'foo@example.com','bar@example.com' |
| Skicka e-tooowners, deltagare och läsare |-SendToServiceOwners |

Skapa en e-åtgärd

```PowerShell
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
```

Skapa en Webhook-åtgärd

```PowerShell
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://example.com?token=mytoken
```

Skapa hello varningsregeln på hello CPU % mått på en klassisk virtuell dator

```PowerShell
Add-AzureRmMetricAlertRule -Name vmcpu_gt_1 -Location "East US" -ResourceGroup myrg1 -TargetResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.ClassicCompute/virtualMachines/my_vm1 -MetricName "Percentage CPU" -Operator GreaterThan -Threshold 1 -WindowSize 00:05:00 -TimeAggregationOperator Average -Actions $actionEmail, $actionWebhook -Description "alert on CPU > 1%"
```

Hämta hello varningsregel

```PowerShell
Get-AzureRmAlertRule -Name vmcpu_gt_1 -ResourceGroup myrg1 -DetailedOutput
```

hello Lägg till avisering cmdlet uppdateras även hello regel om det finns redan en varningsregel för hello angivna egenskaper. toodisable en aviseringsregel inkluderar hello parametern **- DisableRule**.

## <a name="get-a-list-of-available-metrics-for-alerts"></a>Hämta en lista över tillgängliga mått för aviseringar
Du kan använda hello `Get-AzureRmMetricDefinition` cmdlet tooview hello lista över alla mätvärden för en viss resurs.

```PowerShell
Get-AzureRmMetricDefinition -ResourceId <resource_id>
```

hello skapar följande exempel en tabell med hello mått namn och hello enhet för den.

```PowerShell
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

En fullständig lista över tillgängliga alternativ för `Get-AzureRmMetricDefinition` finns på [Get-MetricDefinitions](https://msdn.microsoft.com/library/mt282458.aspx).

## <a name="create-and-manage-autoscale-settings"></a>Skapa och hantera Autoskala inställningar
En resurs, till exempel en webbapp VM, molntjänst eller Skaluppsättning för virtuell dator kan ha endast en autoskalningsinställning som konfigurerats för den.
Varje autoskalningsinställning kan dock ha flera profiler. Till exempel en för en prestandabaserad skala profil och en andra princip för en schemabaserade profil. Varje profil kan ha flera regler som konfigurerats på den. Läs mer om Autoskala [hur tooAutoscale ett program](../cloud-services/cloud-services-how-to-scale.md).

Här är hello steg kommer vi att använda:

1. Skapa regler.
2. Skapa eller profilerna mappning hello regler som du tidigare skapade toohello profiler.
3. Valfritt: Skapa meddelanden om autoskalning genom att konfigurera egenskaper för webhook och e-post.
4. Skapa en autoskalningsinställning med ett namn på hello målresurs genom att mappa hello profiler och meddelanden som du skapade i föregående steg i hello.

hello visar följande exempel hur du kan skapa en autoskalningsinställning för en Virtual Machine Scale Set för ett Windows-operativsystem baserade genom att använda hello CPU-användning mått.

Först skapa en regel tooscale ut, med en instans antal ökning.

```PowerShell
$rule1 = New-AzureRmAutoscaleRule -MetricName "Percentage CPU" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 60 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Increase -ScaleActionValue 1
```        

Skapa en regel tooscale i, med en instans antal minska.

```PowerShell
$rule2 = New-AzureRmAutoscaleRule -MetricName "Percentage CPU" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 30 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Decrease -ScaleActionValue 1
```

Skapa sedan en profil för hello regler.

```PowerShell
$profile1 = New-AzureRmAutoscaleProfile -DefaultCapacity 2 -MaximumCapacity 10 -MinimumCapacity 2 -Rules $rule1,$rule2 -Name "My_Profile"
```

Skapa en webhook-egenskap.

```PowerShell
$webhook_scale = New-AzureRmAutoscaleWebhook -ServiceUri "https://example.com?mytoken=mytokenvalue"
```

Skapa hello notification-egenskapen för hello autoskalningsinställning, inklusive e-post och hello webhook som du skapade tidigare.

```PowerShell
$notification1= New-AzureRmAutoscaleNotification -CustomEmails ashwink@microsoft.com -SendEmailToSubscriptionAdministrators SendEmailToSubscriptionCoAdministrators -Webhooks $webhook_scale
```

Skapa slutligen hello Autoskala inställningen tooadd hello profil som du skapade ovan.

```PowerShell
Add-AzureRmAutoscaleSetting -Location "East US" -Name "MyScaleVMSSSetting" -ResourceGroup big2 -TargetResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -AutoscaleProfiles $profile1 -Notifications $notification1
```

Mer information om hur du hanterar Autoskala inställningar finns [Get-AutoscaleSetting](https://msdn.microsoft.com/library/mt282461.aspx).

## <a name="autoscale-history"></a>Autoskala historik
hello följande exempel visar hur du kan visa senaste Autoskala och aviseringen händelser. Använd hello loggen Sök tooview hello Autoskala aktivitetshistorik.

```PowerShell
Get-AzureRmLog -Caller "Microsoft.Insights/autoscaleSettings" -DetailedOutput -StartTime 2015-03-01
```

Du kan använda hello `Get-AzureRmAutoScaleHistory` cmdlet tooretrieve Autoskala historik.

```PowerShell
Get-AzureRmAutoScaleHistory -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/microsoft.insights/autoscalesettings/myScaleSetting -StartTime 2016-03-15 -DetailedOutput
```

Mer information finns i [Get-AutoscaleHistory](https://msdn.microsoft.com/library/mt282464.aspx).

### <a name="view-details-for-an-autoscale-setting"></a>Visa information om en autoskalningsinställning
Du kan använda hello `Get-Autoscalesetting` cmdlet tooretrieve mer information om hello autoskalningsinställning.

hello följande exempel visas information om alla Autoskala inställningar i myrg1' hello resurs grupp'.

```PowerShell
Get-AzureRmAutoscalesetting -ResourceGroup myrg1 -DetailedOutput
```

hello följande exempel visar information om alla Autoskala inställningar i myrg1' hello resurs grupp' och specifikt hello autoskalningsinställning med namnet 'MyScaleVMSSSetting'.

```PowerShell
Get-AzureRmAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting -DetailedOutput
```

### <a name="remove-an-autoscale-setting"></a>Ta bort en autoskalningsinställning
Du kan använda hello `Remove-Autoscalesetting` cmdlet toodelete en autoskalningsinställning.

```PowerShell
Remove-AzureRmAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting
```

## <a name="manage-log-profiles-for-activity-log"></a>Hantera loggen profiler för aktivitetsloggen
Du kan skapa en *logga profil* och exportera data från din verksamhet loggen tooa storage-konto och du kan konfigurera datalagring för den. Alternativt kan strömma du också hello data tooyour Event Hub. Observera att den här funktionen är för närvarande i förhandsvisning och du kan bara skapa en logg profil per prenumeration. Du kan använda följande cmdlets med din aktuella prenumeration toocreate hello och hantera profiler för loggen. Du kan också välja en viss prenumeration. Även om PowerShell standarder toohello aktuell prenumeration, du kan ändra den med hjälp av `Set-AzureRmContext`. Du kan konfigurera aktiviteten loggen tooroute data tooany storage-konto eller Händelsehubb i den prenumerationen. Data skrivs som blob-filer i JSON-format.

### <a name="get-a-log-profile"></a>Hämta en logg-profil
toofetch din befintliga log-profiler kan använda hello `Get-AzureRmLogProfile` cmdlet.

### <a name="add-a-log-profile-without-data-retention"></a>Lägg till en logg profil utan datalagring
```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia
```

### <a name="remove-a-log-profile"></a>Ta bort en logg-profil
```PowerShell
Remove-AzureRmLogProfile -name my_log_profile_s1
```

### <a name="add-a-log-profile-with-data-retention"></a>Lägg till en logg-profil med datalagring
Du kan ange hello **- RetentionInDays** egenskap med hello antalet dagar som ett positivt heltal där hello data sparas.

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

### <a name="add-log-profile-with-retention-and-eventhub"></a>Lägg till loggen profil med kvarhållning och EventHub
I tillägg toorouting data toostorage-konto du kan också strömmas tooan Event Hub. Observera att i den här förhandsgranskningen versionen och hello konto lagringskonfiguration är obligatorisk men Event Hub-konfigurationen är valfria.

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

## <a name="configure-diagnostics-logs"></a>Konfigurera diagnostik-loggar
Många Azure-tjänster ger ytterligare loggar och telemetri som kan vara konfigurerade toosave data i ditt Azure Storage-konto kan du skicka tooEvent NAV och/eller skickas tooan OMS logganalys-arbetsytan. Åtgärden kan endast utföras på en resurs-nivå och hello storage-konto eller händelse hubb ska finnas i hello samma region som hello målresurs där hello diagnostik inställningen konfigureras.

### <a name="get-diagnostic-setting"></a>Hämta diagnostikinställningen
```PowerShell
Get-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp
```

Inaktivera diagnostikinställningen

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $false
```

Aktivera diagnostikinställningen utan kvarhållning

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true
```

Aktivera diagnostikinställningen med kvarhållning

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true -RetentionEnabled $true -RetentionInDays 90
```

Aktivera diagnostikinställningen med kvarhållning för en viss loggning kategori

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/sakteststorage -Categories NetworkSecurityGroupEvent -Enable $true -RetentionEnabled $true -RetentionInDays 90
```

Aktivera diagnostikinställningen för Händelsehubbar

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Enable $true
```

Aktivera diagnostikinställningen för OMS

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -WorkspaceId 76d785fd-d1ce-4f50-8ca3-858fc819ca0f -Enabled $true

```
