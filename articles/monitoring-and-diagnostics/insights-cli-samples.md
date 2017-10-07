---
title: "aaaAzure övervakaren CLI 1.0 Snabbstart prover. | Microsoft Docs"
description: "Exempel CLI 1.0-kommandon för Azure-Monitor-funktioner. Azure övervakaren är en Microsoft Azure-tjänst som gör att du toosend aviseringar, anrop webbadresser som baseras på värden för konfigurerade telemetridata och Autoskala molntjänster, virtuella datorer och Web Apps."
author: kamathashwin
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 1653aa81-0ee6-4622-9c65-d4801ed9006f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: ashwink
ms.openlocfilehash: 6cd9cd62b3a1977276563f5e43f5384ccca66247
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-monitor--cross-platform-cli-10-quick-start-samples"></a>Azure CLI för övervakaren plattformsoberoende 1.0 Snabbstart-exempel
Den här artikeln innehåller exempel på kommandoradsgränssnittet (CLI) kommandon toohelp du komma åt Azure-Monitor-funktioner. Azure övervakaren kan tooAutoScale molntjänster, virtuella datorer, och Web Apps och toosend aviseringsmeddelanden eller anropet webbadresser som baseras på värden för konfigurerade telemetridata.

> [!NOTE]
> Azure övervakaren är hello nytt namn för vad anropades ”Azure Insights” förrän den 25 september 2016. Dock innehåller hello namnområden och därmed hello-kommandona nedan fortfarande insikter ”hello”.
> 
> 

## <a name="prerequisites"></a>Krav
Om du inte redan har installerat hello Azure CLI, se [installera hello Azure CLI](../cli-install-nodejs.md). Om du är bekant med Azure CLI, kan du läsa mer om den på [Använd hello Azure CLI för Mac, Linux och Windows med Azure Resource Manager](../xplat-cli-azure-resource-manager.md).

I Windows kan du installera npm från hello [Node.js webbplats](https://nodejs.org/). När du har slutfört installationen hello med Kör som administratör privilegier CMD.exe kör hello följande från hello mapp där npm installeras:

```console
npm install azure-cli --global
```

Nu ska du navigera tooany/mappen du vill använda och Skriv vid hello kommandoraden:

```console
azure help
```

## <a name="log-in-tooazure"></a>Logga in tooAzure
hello första steget är toologin tooyour Azure-konto.

```console
azure login
```

När du har kört kommandot har toosign i via hello anvisningar om hello-skärmen. Därefter kan se du ditt konto, TenantId och standard prenumerations-Id. Alla kommandon fungerar på hello kontexten för prenumerationen standard.

toolist hello information om din aktuella prenumeration använda hello följande kommando.

```console
azure account show
```

toochange arbeta kontexten tooa annan prenumeration, Använd hello följande kommando.

```console
azure account set "subscription ID or subscription name"
```

toouse Azure Resource Manager och Azure-Monitor-kommandon, behöver du toobe i läget Azure Resource Manager.

```console
azure config mode arm
```

tooview en lista över alla kommandon som stöds Azure-Monitor, utföra hello följande.

```console
azure insights
```

## <a name="view-activity-log-for-a-subscription"></a>Visa aktivitetsloggen för en prenumeration
tooview en lista över aktiviteten logghändelser utföra hello följande.

```console
azure insights logs list [options]
```

Försök följande tooview hello alla tillgängliga alternativ.

```console
azure insights logs list -help
```

Här är ett exempel toolist loggar för av en resursgrupp

```console
azure insights logs list --resourceGroup "myrg1"
```

Exempel toolist loggar av anroparen

```console
azure insights logs list --caller "myname@company.com"
```

Exempel toolist loggar av anroparen på en resurstyp i ett start- och datum

```console
azure insights logs list --resourceProvider "Microsoft.Web" --caller "myname@company.com" --startTime 2016-03-08T00:00:00Z --endTime 2016-03-16T00:00:00Z
```

## <a name="work-with-alerts"></a>Arbeta med aviseringar
Du kan använda hello information i hello avsnittet toowork med aviseringar.

### <a name="get-alert-rules-in-a-resource-group"></a>Hämta Varningsregler i en resursgrupp
```console
azure insights alerts rule list abhingrgtest123
azure insights alerts rule list abhingrgtest123 --ruleName andy0323
```

### <a name="create-a-metric-alert-rule"></a>Skapa ett mått varningsregel
```console
azure insights alerts actions email create --customEmails foo@microsoft.com
azure insights alerts actions webhook create https://someuri.com
azure insights alerts rule metric set andy0323 eastus abhingrgtest123 PT5M GreaterThan 2 /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Web-EastUS/providers/Microsoft.Web/serverfarms/Default1 BytesReceived Total
```

### <a name="create-webtest-alert-rule"></a>Skapa varningsregeln för webbtestet
```console
azure insights alerts rule webtest set leowebtestr1-webtestr1 eastus Default-Web-WestUS PT5M 1 GSMT_AvRaw /subscriptions/b67f7fec-69fc-4974-9099-a26bd6ffeda3/resourcegroups/Default-Web-WestUS/providers/microsoft.insights/webtests/leowebtestr1-webtestr1
```

### <a name="delete-an-alert-rule"></a>Ta bort en varningsregel
```console
azure insights alerts rule delete abhingrgtest123 andy0323
```

## <a name="log-profiles"></a>Log-profiler
Använd hello informationen i det här avsnittet toowork med loggen profiler.

### <a name="get-a-log-profile"></a>Hämta en logg-profil
```console
azure insights logprofile list
azure insights logprofile get -n default
```


### <a name="add-a-log-profile-without-retention"></a>Lägg till en logg profil utan kvarhållning
```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia
```

### <a name="remove-a-log-profile"></a>Ta bort en logg-profil
```console
azure insights logprofile delete --name default
```

### <a name="add-a-log-profile-with-retention"></a>Lägg till en logg-profil med kvarhållning
```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia --retentionInDays 90
```

### <a name="add-a-log-profile-with-retention-and-eventhub"></a>Lägg till en logg-profil med kvarhållning och EventHub
```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --serviceBusRuleId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/testshoeboxeastus/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia --retentionInDays 90
```


## <a name="diagnostics"></a>Diagnostik
Använd hello information i det här avsnittet toowork med diagnostikinställningar.

### <a name="get-a-diagnostic-setting"></a>Hämta en diagnostikinställningen
```console
azure insights diagnostic get --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp
```

### <a name="disable-a-diagnostic-setting"></a>Inaktivera en diagnostikinställningen
```console
azure insights diagnostic set --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp --storageId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/shibanitesting --enabled false
```

### <a name="enable-a-diagnostic-setting-without-retention"></a>Aktivera diagnostikinställningen utan kvarhållning
```console
azure insights diagnostic set --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp --storageId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/shibanitesting --enabled true
```


## <a name="autoscale"></a>Automatisk skalning
Använd hello information i det här avsnittet toowork med Autoskala inställningar. Du måste toomodify exemplen.

### <a name="get-autoscale-settings-for-a-resource-group"></a>Hämta Autoskala inställningar för en resursgrupp
```console
azure insights autoscale setting list montest2
```

### <a name="get-autoscale-settings-by-name-in-a-resource-group"></a>Hämta inställningar för Autoskala efter namn i en resursgrupp
```console
azure insights autoscale setting list montest2 -n setting2
```


### <a name="set-auotoscale-settings"></a>Ange inställningar för auotoscale
```console
azure insights autoscale setting set montest2 -n setting2 --settingSpec
```
