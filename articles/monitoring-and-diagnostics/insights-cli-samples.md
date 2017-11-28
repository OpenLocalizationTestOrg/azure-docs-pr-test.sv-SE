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
# <a name="azure-monitor--cross-platform-cli-10-quick-start-samples"></a><span data-ttu-id="94cb4-105">Azure CLI för övervakaren plattformsoberoende 1.0 Snabbstart-exempel</span><span class="sxs-lookup"><span data-stu-id="94cb4-105">Azure Monitor  Cross-platform CLI 1.0 quick start samples</span></span>
<span data-ttu-id="94cb4-106">Den här artikeln innehåller exempel på kommandoradsgränssnittet (CLI) kommandon toohelp du komma åt Azure-Monitor-funktioner.</span><span class="sxs-lookup"><span data-stu-id="94cb4-106">This article shows you sample command-line interface (CLI) commands toohelp you access Azure Monitor features.</span></span> <span data-ttu-id="94cb4-107">Azure övervakaren kan tooAutoScale molntjänster, virtuella datorer, och Web Apps och toosend aviseringsmeddelanden eller anropet webbadresser som baseras på värden för konfigurerade telemetridata.</span><span class="sxs-lookup"><span data-stu-id="94cb4-107">Azure Monitor allows you tooAutoScale Cloud Services, Virtual Machines, and Web Apps and toosend alert notifications or call web URLs based on values of configured telemetry data.</span></span>

> [!NOTE]
> <span data-ttu-id="94cb4-108">Azure övervakaren är hello nytt namn för vad anropades ”Azure Insights” förrän den 25 september 2016.</span><span class="sxs-lookup"><span data-stu-id="94cb4-108">Azure Monitor is hello new name for what was called "Azure Insights" until Sept 25th, 2016.</span></span> <span data-ttu-id="94cb4-109">Dock innehåller hello namnområden och därmed hello-kommandona nedan fortfarande insikter ”hello”.</span><span class="sxs-lookup"><span data-stu-id="94cb4-109">However, hello namespaces and thus hello commands below still contain hello "insights".</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="94cb4-110">Krav</span><span class="sxs-lookup"><span data-stu-id="94cb4-110">Prerequisites</span></span>
<span data-ttu-id="94cb4-111">Om du inte redan har installerat hello Azure CLI, se [installera hello Azure CLI](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="94cb4-111">If you haven't already installed hello Azure CLI, see [Install hello Azure CLI](../cli-install-nodejs.md).</span></span> <span data-ttu-id="94cb4-112">Om du är bekant med Azure CLI, kan du läsa mer om den på [Använd hello Azure CLI för Mac, Linux och Windows med Azure Resource Manager](../xplat-cli-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="94cb4-112">If you're unfamiliar with Azure CLI, you can read more about it at [Use hello Azure CLI for Mac, Linux, and Windows with Azure Resource Manager](../xplat-cli-azure-resource-manager.md).</span></span>

<span data-ttu-id="94cb4-113">I Windows kan du installera npm från hello [Node.js webbplats](https://nodejs.org/).</span><span class="sxs-lookup"><span data-stu-id="94cb4-113">In Windows, install npm from hello [Node.js website](https://nodejs.org/).</span></span> <span data-ttu-id="94cb4-114">När du har slutfört installationen hello med Kör som administratör privilegier CMD.exe kör hello följande från hello mapp där npm installeras:</span><span class="sxs-lookup"><span data-stu-id="94cb4-114">After you complete hello installation, using CMD.exe with Run As Administrator privileges, execute hello following from hello folder where npm is installed:</span></span>

```console
npm install azure-cli --global
```

<span data-ttu-id="94cb4-115">Nu ska du navigera tooany/mappen du vill använda och Skriv vid hello kommandoraden:</span><span class="sxs-lookup"><span data-stu-id="94cb4-115">Next, navigate tooany folder/location you want and type at hello command-line:</span></span>

```console
azure help
```

## <a name="log-in-tooazure"></a><span data-ttu-id="94cb4-116">Logga in tooAzure</span><span class="sxs-lookup"><span data-stu-id="94cb4-116">Log in tooAzure</span></span>
<span data-ttu-id="94cb4-117">hello första steget är toologin tooyour Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="94cb4-117">hello first step is toologin tooyour Azure account.</span></span>

```console
azure login
```

<span data-ttu-id="94cb4-118">När du har kört kommandot har toosign i via hello anvisningar om hello-skärmen.</span><span class="sxs-lookup"><span data-stu-id="94cb4-118">After running this command, you have toosign in via hello instructions on hello screen.</span></span> <span data-ttu-id="94cb4-119">Därefter kan se du ditt konto, TenantId och standard prenumerations-Id. Alla kommandon fungerar på hello kontexten för prenumerationen standard.</span><span class="sxs-lookup"><span data-stu-id="94cb4-119">Afterward, you see your Account, TenantId, and default Subscription Id. All commands work in hello context of your default subscription.</span></span>

<span data-ttu-id="94cb4-120">toolist hello information om din aktuella prenumeration använda hello följande kommando.</span><span class="sxs-lookup"><span data-stu-id="94cb4-120">toolist hello details of your current subscription, use hello following command.</span></span>

```console
azure account show
```

<span data-ttu-id="94cb4-121">toochange arbeta kontexten tooa annan prenumeration, Använd hello följande kommando.</span><span class="sxs-lookup"><span data-stu-id="94cb4-121">toochange working context tooa different subscription, use hello following command.</span></span>

```console
azure account set "subscription ID or subscription name"
```

<span data-ttu-id="94cb4-122">toouse Azure Resource Manager och Azure-Monitor-kommandon, behöver du toobe i läget Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="94cb4-122">toouse Azure Resource Manager and Azure Monitor commands, you need toobe in Azure Resource Manager mode.</span></span>

```console
azure config mode arm
```

<span data-ttu-id="94cb4-123">tooview en lista över alla kommandon som stöds Azure-Monitor, utföra hello följande.</span><span class="sxs-lookup"><span data-stu-id="94cb4-123">tooview a list of all supported Azure Monitor commands, perform hello following.</span></span>

```console
azure insights
```

## <a name="view-activity-log-for-a-subscription"></a><span data-ttu-id="94cb4-124">Visa aktivitetsloggen för en prenumeration</span><span class="sxs-lookup"><span data-stu-id="94cb4-124">View activity log for a subscription</span></span>
<span data-ttu-id="94cb4-125">tooview en lista över aktiviteten logghändelser utföra hello följande.</span><span class="sxs-lookup"><span data-stu-id="94cb4-125">tooview a list of activity log events, perform hello following.</span></span>

```console
azure insights logs list [options]
```

<span data-ttu-id="94cb4-126">Försök följande tooview hello alla tillgängliga alternativ.</span><span class="sxs-lookup"><span data-stu-id="94cb4-126">Try hello following tooview all available options.</span></span>

```console
azure insights logs list -help
```

<span data-ttu-id="94cb4-127">Här är ett exempel toolist loggar för av en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="94cb4-127">Here is an example toolist logs by a resourceGroup</span></span>

```console
azure insights logs list --resourceGroup "myrg1"
```

<span data-ttu-id="94cb4-128">Exempel toolist loggar av anroparen</span><span class="sxs-lookup"><span data-stu-id="94cb4-128">Example toolist logs by caller</span></span>

```console
azure insights logs list --caller "myname@company.com"
```

<span data-ttu-id="94cb4-129">Exempel toolist loggar av anroparen på en resurstyp i ett start- och datum</span><span class="sxs-lookup"><span data-stu-id="94cb4-129">Example toolist logs by caller on a resource type, within a start and end date</span></span>

```console
azure insights logs list --resourceProvider "Microsoft.Web" --caller "myname@company.com" --startTime 2016-03-08T00:00:00Z --endTime 2016-03-16T00:00:00Z
```

## <a name="work-with-alerts"></a><span data-ttu-id="94cb4-130">Arbeta med aviseringar</span><span class="sxs-lookup"><span data-stu-id="94cb4-130">Work with alerts</span></span>
<span data-ttu-id="94cb4-131">Du kan använda hello information i hello avsnittet toowork med aviseringar.</span><span class="sxs-lookup"><span data-stu-id="94cb4-131">You can use hello information in hello section toowork with alerts.</span></span>

### <a name="get-alert-rules-in-a-resource-group"></a><span data-ttu-id="94cb4-132">Hämta Varningsregler i en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="94cb4-132">Get alert rules in a resource group</span></span>
```console
azure insights alerts rule list abhingrgtest123
azure insights alerts rule list abhingrgtest123 --ruleName andy0323
```

### <a name="create-a-metric-alert-rule"></a><span data-ttu-id="94cb4-133">Skapa ett mått varningsregel</span><span class="sxs-lookup"><span data-stu-id="94cb4-133">Create a metric alert rule</span></span>
```console
azure insights alerts actions email create --customEmails foo@microsoft.com
azure insights alerts actions webhook create https://someuri.com
azure insights alerts rule metric set andy0323 eastus abhingrgtest123 PT5M GreaterThan 2 /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Web-EastUS/providers/Microsoft.Web/serverfarms/Default1 BytesReceived Total
```

### <a name="create-webtest-alert-rule"></a><span data-ttu-id="94cb4-134">Skapa varningsregeln för webbtestet</span><span class="sxs-lookup"><span data-stu-id="94cb4-134">Create webtest alert rule</span></span>
```console
azure insights alerts rule webtest set leowebtestr1-webtestr1 eastus Default-Web-WestUS PT5M 1 GSMT_AvRaw /subscriptions/b67f7fec-69fc-4974-9099-a26bd6ffeda3/resourcegroups/Default-Web-WestUS/providers/microsoft.insights/webtests/leowebtestr1-webtestr1
```

### <a name="delete-an-alert-rule"></a><span data-ttu-id="94cb4-135">Ta bort en varningsregel</span><span class="sxs-lookup"><span data-stu-id="94cb4-135">Delete an alert rule</span></span>
```console
azure insights alerts rule delete abhingrgtest123 andy0323
```

## <a name="log-profiles"></a><span data-ttu-id="94cb4-136">Log-profiler</span><span class="sxs-lookup"><span data-stu-id="94cb4-136">Log profiles</span></span>
<span data-ttu-id="94cb4-137">Använd hello informationen i det här avsnittet toowork med loggen profiler.</span><span class="sxs-lookup"><span data-stu-id="94cb4-137">Use hello information in this section toowork with log profiles.</span></span>

### <a name="get-a-log-profile"></a><span data-ttu-id="94cb4-138">Hämta en logg-profil</span><span class="sxs-lookup"><span data-stu-id="94cb4-138">Get a log profile</span></span>
```console
azure insights logprofile list
azure insights logprofile get -n default
```


### <a name="add-a-log-profile-without-retention"></a><span data-ttu-id="94cb4-139">Lägg till en logg profil utan kvarhållning</span><span class="sxs-lookup"><span data-stu-id="94cb4-139">Add a log profile without retention</span></span>
```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia
```

### <a name="remove-a-log-profile"></a><span data-ttu-id="94cb4-140">Ta bort en logg-profil</span><span class="sxs-lookup"><span data-stu-id="94cb4-140">Remove a log profile</span></span>
```console
azure insights logprofile delete --name default
```

### <a name="add-a-log-profile-with-retention"></a><span data-ttu-id="94cb4-141">Lägg till en logg-profil med kvarhållning</span><span class="sxs-lookup"><span data-stu-id="94cb4-141">Add a log profile with retention</span></span>
```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia --retentionInDays 90
```

### <a name="add-a-log-profile-with-retention-and-eventhub"></a><span data-ttu-id="94cb4-142">Lägg till en logg-profil med kvarhållning och EventHub</span><span class="sxs-lookup"><span data-stu-id="94cb4-142">Add a log profile with retention and EventHub</span></span>
```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --serviceBusRuleId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/testshoeboxeastus/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia --retentionInDays 90
```


## <a name="diagnostics"></a><span data-ttu-id="94cb4-143">Diagnostik</span><span class="sxs-lookup"><span data-stu-id="94cb4-143">Diagnostics</span></span>
<span data-ttu-id="94cb4-144">Använd hello information i det här avsnittet toowork med diagnostikinställningar.</span><span class="sxs-lookup"><span data-stu-id="94cb4-144">Use hello information in this section toowork with diagnostic settings.</span></span>

### <a name="get-a-diagnostic-setting"></a><span data-ttu-id="94cb4-145">Hämta en diagnostikinställningen</span><span class="sxs-lookup"><span data-stu-id="94cb4-145">Get a diagnostic setting</span></span>
```console
azure insights diagnostic get --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp
```

### <a name="disable-a-diagnostic-setting"></a><span data-ttu-id="94cb4-146">Inaktivera en diagnostikinställningen</span><span class="sxs-lookup"><span data-stu-id="94cb4-146">Disable a diagnostic setting</span></span>
```console
azure insights diagnostic set --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp --storageId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/shibanitesting --enabled false
```

### <a name="enable-a-diagnostic-setting-without-retention"></a><span data-ttu-id="94cb4-147">Aktivera diagnostikinställningen utan kvarhållning</span><span class="sxs-lookup"><span data-stu-id="94cb4-147">Enable a diagnostic setting without retention</span></span>
```console
azure insights diagnostic set --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp --storageId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/shibanitesting --enabled true
```


## <a name="autoscale"></a><span data-ttu-id="94cb4-148">Automatisk skalning</span><span class="sxs-lookup"><span data-stu-id="94cb4-148">Autoscale</span></span>
<span data-ttu-id="94cb4-149">Använd hello information i det här avsnittet toowork med Autoskala inställningar.</span><span class="sxs-lookup"><span data-stu-id="94cb4-149">Use hello information in this section toowork with autoscale settings.</span></span> <span data-ttu-id="94cb4-150">Du måste toomodify exemplen.</span><span class="sxs-lookup"><span data-stu-id="94cb4-150">You need toomodify these examples.</span></span>

### <a name="get-autoscale-settings-for-a-resource-group"></a><span data-ttu-id="94cb4-151">Hämta Autoskala inställningar för en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="94cb4-151">Get autoscale settings for a resource group</span></span>
```console
azure insights autoscale setting list montest2
```

### <a name="get-autoscale-settings-by-name-in-a-resource-group"></a><span data-ttu-id="94cb4-152">Hämta inställningar för Autoskala efter namn i en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="94cb4-152">Get autoscale settings by name in a resource group</span></span>
```console
azure insights autoscale setting list montest2 -n setting2
```


### <a name="set-auotoscale-settings"></a><span data-ttu-id="94cb4-153">Ange inställningar för auotoscale</span><span class="sxs-lookup"><span data-stu-id="94cb4-153">Set auotoscale settings</span></span>
```console
azure insights autoscale setting set montest2 -n setting2 --settingSpec
```
