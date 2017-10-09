---
title: "aaaMonitor åtgärder, händelser och prestandaräknare för NSG: er | Microsoft Docs"
description: "Lär dig hur tooenable räknare, händelser och operativa loggning för NSG: er"
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 2e699078-043f-48bd-8aa8-b011a32d98ca
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/31/2017
ms.author: jdial
ms.openlocfilehash: f16f1a0ad693028ee7aba21574b5c8ddfcd27096
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="log-analytics-for-network-security-groups-nsgs"></a><span data-ttu-id="83328-103">Log Analytics för nätverkssäkerhetsgrupper (NSG:er)</span><span class="sxs-lookup"><span data-stu-id="83328-103">Log analytics for network security groups (NSGs)</span></span>

<span data-ttu-id="83328-104">Du kan aktivera hello följande diagnostiska loggen kategorier för NSG: er:</span><span class="sxs-lookup"><span data-stu-id="83328-104">You can enable hello following diagnostic log categories for NSGs:</span></span>

* <span data-ttu-id="83328-105">**Händelse:** innehåller poster för vilka NSG regler är tillämpade tooVMs och instans roller baserat på MAC-adress.</span><span class="sxs-lookup"><span data-stu-id="83328-105">**Event:** Contains entries for which NSG rules are applied tooVMs and instance roles based on MAC address.</span></span> <span data-ttu-id="83328-106">hello status för de här reglerna samlas var 60: e sekund.</span><span class="sxs-lookup"><span data-stu-id="83328-106">hello status for these rules is collected every 60 seconds.</span></span>
* <span data-ttu-id="83328-107">**Regelräknare:** innehåller poster för hur många gånger varje NSG regel är tillämpade toodeny eller Tillåt trafiken.</span><span class="sxs-lookup"><span data-stu-id="83328-107">**Rule counter:** Contains entries for how many times each NSG rule is applied toodeny or allow traffic.</span></span>

> [!NOTE]
> <span data-ttu-id="83328-108">Diagnostikloggar är bara tillgängliga för NSG: er som distribueras via hello Azure Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="83328-108">Diagnostic logs are only available for NSGs deployed through hello Azure Resource Manager deployment model.</span></span> <span data-ttu-id="83328-109">Du kan inte aktivera diagnostikloggning för NSG: er som distribueras via hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="83328-109">You cannot enable diagnostic logging for NSGs deployed through hello classic deployment model.</span></span> <span data-ttu-id="83328-110">För bättre förståelse av hello två modeller, referera till hello [förstå Azure distributionsmodeller](../resource-manager-deployment-model.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="83328-110">For a better understanding of hello two models, reference hello [Understanding Azure deployment models](../resource-manager-deployment-model.md) article.</span></span>

<span data-ttu-id="83328-111">Aktivitetsloggning (tidigare kallat granskningsläge eller operativa loggar) är aktiverad som standard för NSG: er som skapats via antingen Azure distributionsmodell.</span><span class="sxs-lookup"><span data-stu-id="83328-111">Activity logging (previously known as audit or operational logs) is enabled by default for NSGs created through either Azure deployment model.</span></span> <span data-ttu-id="83328-112">vilka åtgärder har slutförts på NSG: er i hello aktivitetsloggen, leta efter poster som innehåller följande typer av resurser hello toodetermine:</span><span class="sxs-lookup"><span data-stu-id="83328-112">toodetermine which operations were completed on NSGs in hello activity log, look for entries that contain hello following resource types:</span></span> 

- <span data-ttu-id="83328-113">Microsoft.ClassicNetwork/networkSecurityGroups</span><span class="sxs-lookup"><span data-stu-id="83328-113">Microsoft.ClassicNetwork/networkSecurityGroups</span></span> 
- <span data-ttu-id="83328-114">Microsoft.ClassicNetwork/networkSecurityGroups/securityRules</span><span class="sxs-lookup"><span data-stu-id="83328-114">Microsoft.ClassicNetwork/networkSecurityGroups/securityRules</span></span>
- <span data-ttu-id="83328-115">Microsoft.Network/networkSecurityGroups</span><span class="sxs-lookup"><span data-stu-id="83328-115">Microsoft.Network/networkSecurityGroups</span></span>
- <span data-ttu-id="83328-116">Microsoft.Network/networkSecurityGroups/securityRules</span><span class="sxs-lookup"><span data-stu-id="83328-116">Microsoft.Network/networkSecurityGroups/securityRules</span></span> 

<span data-ttu-id="83328-117">Läs hello [översikt över hello Azure-aktivitetsloggen](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) artikel toolearn mer om aktivitetsloggar.</span><span class="sxs-lookup"><span data-stu-id="83328-117">Read hello [Overview of hello Azure Activity Log](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) article toolearn more about activity logs.</span></span> 

## <a name="enable-diagnostic-logging"></a><span data-ttu-id="83328-118">Aktivera diagnostikloggning</span><span class="sxs-lookup"><span data-stu-id="83328-118">Enable diagnostic logging</span></span>

<span data-ttu-id="83328-119">Diagnostikloggning måste vara aktiverat för *varje* NSG önskade toocollect data för.</span><span class="sxs-lookup"><span data-stu-id="83328-119">Diagnostic logging must be enabled for *each* NSG you want toocollect data for.</span></span> <span data-ttu-id="83328-120">Hej [översikt av Azure diagnostikloggar](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) artikeln förklarar var diagnostikloggar skickas.</span><span class="sxs-lookup"><span data-stu-id="83328-120">hello [Overview of Azure Diagnostic Logs](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) article explains where diagnostic logs can be sent.</span></span> <span data-ttu-id="83328-121">Om du inte har en befintlig NSG fullständig hello stegen i hello [skapar en nätverkssäkerhetsgrupp](virtual-networks-create-nsg-arm-pportal.md) artikel toocreate en.</span><span class="sxs-lookup"><span data-stu-id="83328-121">If you don't have an existing NSG, complete hello steps in hello [Create a network security group](virtual-networks-create-nsg-arm-pportal.md) article toocreate one.</span></span> <span data-ttu-id="83328-122">Du kan aktivera loggning med hjälp av följande metoder hello NSG:</span><span class="sxs-lookup"><span data-stu-id="83328-122">You can enable NSG diagnostic logging using any of hello following methods:</span></span>

### <a name="azure-portal"></a><span data-ttu-id="83328-123">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="83328-123">Azure portal</span></span>

<span data-ttu-id="83328-124">toouse hello portal tooenable loggning, inloggning toohello [portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="83328-124">toouse hello portal tooenable logging, login toohello [portal](https://portal.azure.com).</span></span> <span data-ttu-id="83328-125">Klicka på **fler tjänster**, Skriv *nätverkssäkerhetsgrupper*.</span><span class="sxs-lookup"><span data-stu-id="83328-125">Click **More services**, then type *network security groups*.</span></span> <span data-ttu-id="83328-126">Välj hello NSG som du vill tooenable loggning för.</span><span class="sxs-lookup"><span data-stu-id="83328-126">Select hello NSG you want tooenable logging for.</span></span> <span data-ttu-id="83328-127">Följ instruktionerna för hello för icke-beräkningsresurser i hello [aktivera diagnostikloggar i hello portal](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs) artikel.</span><span class="sxs-lookup"><span data-stu-id="83328-127">Follow hello instructions for non-compute resources in hello [Enable diagnostic logs in hello portal](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs) article.</span></span> <span data-ttu-id="83328-128">Välj **NetworkSecurityGroupEvent**, **NetworkSecurityGroupRuleCounter**, eller båda kategorier av loggar.</span><span class="sxs-lookup"><span data-stu-id="83328-128">Select **NetworkSecurityGroupEvent**, **NetworkSecurityGroupRuleCounter**, or both categories of logs.</span></span>

### <a name="powershell"></a><span data-ttu-id="83328-129">PowerShell</span><span class="sxs-lookup"><span data-stu-id="83328-129">PowerShell</span></span>

<span data-ttu-id="83328-130">toouse PowerShell tooenable loggning, följer du anvisningarna hello i hello [aktivera diagnostikloggar via PowerShell](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs) artikel.</span><span class="sxs-lookup"><span data-stu-id="83328-130">toouse PowerShell tooenable logging, follow hello instructions in hello [Enable diagnostic logs via PowerShell](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs) article.</span></span> <span data-ttu-id="83328-131">Utvärdera hello följande information innan du anger ett kommando från hello-artikel:</span><span class="sxs-lookup"><span data-stu-id="83328-131">Evaluate hello following information before entering a command from hello article:</span></span>

- <span data-ttu-id="83328-132">Du kan fastställa hello värdet toouse för hello `-ResourceId` parameter genom att ersätta hello följande [text] efter behov, hello kommandot `Get-AzureRmNetworkSecurityGroup -Name [nsg-name] -ResourceGroupName [resource-group-name]`.</span><span class="sxs-lookup"><span data-stu-id="83328-132">You can determine hello value toouse for hello `-ResourceId` parameter by replacing hello following [text], as appropriate, then entering hello command `Get-AzureRmNetworkSecurityGroup -Name [nsg-name] -ResourceGroupName [resource-group-name]`.</span></span> <span data-ttu-id="83328-133">hello ID utdata från kommandot hello ser ut ungefär för*/subscriptions/ [prenumeration Id]/resourceGroups/[resource-group]/providers/Microsoft.Network/networkSecurityGroups/[NSG name]*.</span><span class="sxs-lookup"><span data-stu-id="83328-133">hello ID output from hello command looks similar too*/subscriptions/[Subscription Id]/resourceGroups/[resource-group]/providers/Microsoft.Network/networkSecurityGroups/[NSG name]*.</span></span>
- <span data-ttu-id="83328-134">Om du bara vill toocollect data från loggen kategori lägger du till `-Categories [category]` toohello slutet hello kommandot i hello artikeln, där kategorin är antingen *NetworkSecurityGroupEvent* eller *NetworkSecurityGroupRuleCounter*.</span><span class="sxs-lookup"><span data-stu-id="83328-134">If you only want toocollect data from log category add `-Categories [category]` toohello end of hello command in hello article, where category is either *NetworkSecurityGroupEvent* or *NetworkSecurityGroupRuleCounter*.</span></span> <span data-ttu-id="83328-135">Om du inte använder hello `-Categories` parameter, datainsamling har aktiverats för både loggfilen kategorier.</span><span class="sxs-lookup"><span data-stu-id="83328-135">If you don't use hello `-Categories` parameter, data collection is enabled for both log categories.</span></span>

### <a name="azure-command-line-interface-cli"></a><span data-ttu-id="83328-136">Azure-kommandoradsgränssnittet (CLI)</span><span class="sxs-lookup"><span data-stu-id="83328-136">Azure command-line interface (CLI)</span></span>

<span data-ttu-id="83328-137">toouse Hej CLI tooenable loggning, följer du anvisningarna hello i hello [aktivera diagnostikloggar via CLI](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs) artikel.</span><span class="sxs-lookup"><span data-stu-id="83328-137">toouse hello CLI tooenable logging, follow hello instructions in hello [Enable diagnostic logs via CLI](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs) article.</span></span> <span data-ttu-id="83328-138">Utvärdera hello följande information innan du anger ett kommando från hello-artikel:</span><span class="sxs-lookup"><span data-stu-id="83328-138">Evaluate hello following information before entering a command from hello article:</span></span>

- <span data-ttu-id="83328-139">Du kan fastställa hello värdet toouse för hello `-ResourceId` parameter genom att ersätta hello följande [text] efter behov, hello kommandot `azure network nsg show [resource-group-name] [nsg-name]`.</span><span class="sxs-lookup"><span data-stu-id="83328-139">You can determine hello value toouse for hello `-ResourceId` parameter by replacing hello following [text], as appropriate, then entering hello command `azure network nsg show [resource-group-name] [nsg-name]`.</span></span> <span data-ttu-id="83328-140">hello ID utdata från kommandot hello ser ut ungefär för*/subscriptions/ [prenumeration Id]/resourceGroups/[resource-group]/providers/Microsoft.Network/networkSecurityGroups/[NSG name]*.</span><span class="sxs-lookup"><span data-stu-id="83328-140">hello ID output from hello command looks similar too*/subscriptions/[Subscription Id]/resourceGroups/[resource-group]/providers/Microsoft.Network/networkSecurityGroups/[NSG name]*.</span></span>
- <span data-ttu-id="83328-141">Om du bara vill toocollect data från loggen kategori lägger du till `-Categories [category]` toohello slutet hello kommandot i hello artikeln, där kategorin är antingen *NetworkSecurityGroupEvent* eller *NetworkSecurityGroupRuleCounter*.</span><span class="sxs-lookup"><span data-stu-id="83328-141">If you only want toocollect data from log category add `-Categories [category]` toohello end of hello command in hello article, where category is either *NetworkSecurityGroupEvent* or *NetworkSecurityGroupRuleCounter*.</span></span> <span data-ttu-id="83328-142">Om du inte använder hello `-Categories` parameter, datainsamling har aktiverats för både loggfilen kategorier.</span><span class="sxs-lookup"><span data-stu-id="83328-142">If you don't use hello `-Categories` parameter, data collection is enabled for both log categories.</span></span>

## <a name="logged-data"></a><span data-ttu-id="83328-143">Loggdata</span><span class="sxs-lookup"><span data-stu-id="83328-143">Logged data</span></span>

<span data-ttu-id="83328-144">JSON-formaterade data skrivs för båda loggar.</span><span class="sxs-lookup"><span data-stu-id="83328-144">JSON-formatted data is written for both logs.</span></span> <span data-ttu-id="83328-145">hello specifika data som skrivs för varje loggtyp av visas i hello följande avsnitt:</span><span class="sxs-lookup"><span data-stu-id="83328-145">hello specific data written for each log type is listed in hello following sections:</span></span>

### <a name="event-log"></a><span data-ttu-id="83328-146">Händelseloggen</span><span class="sxs-lookup"><span data-stu-id="83328-146">Event log</span></span>
<span data-ttu-id="83328-147">Den här loggfilen innehåller information om vilka NSG-regler är tillämpade tooVMs och cloud service rollinstanser baserat på MAC-adress.</span><span class="sxs-lookup"><span data-stu-id="83328-147">This log contains information about which NSG rules are applied tooVMs and cloud service role instances, based on MAC address.</span></span> <span data-ttu-id="83328-148">hello följande exempeldata loggas för varje händelse:</span><span class="sxs-lookup"><span data-stu-id="83328-148">hello following example data is logged for each event:</span></span>

```json
{
    "time": "[DATE-TIME]",
    "systemId": "007d0441-5d6b-41f6-8bfd-930db640ec03",
    "category": "NetworkSecurityGroupEvent",
    "resourceId": "/SUBSCRIPTIONS/[SUBSCRIPTION-ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME]/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG-NAME]",
    "operationName": "NetworkSecurityGroupEvents",
    "properties": {
        "vnetResourceGuid":"{5E8AEC16-C728-441F-B0CA-B791E1DBC2F4}",
        "subnetPrefix":"192.168.1.0/24",
        "macAddress":"00-0D-3A-92-6A-7C",
        "primaryIPv4Address":"192.168.1.4",
        "ruleName":"UserRule_default-allow-rdp",
        "direction":"In",
        "priority":1000,
        "type":"allow",
        "conditions":{
            "protocols":"6",
            "destinationPortRange":"3389-3389",
            "sourcePortRange":"0-65535",
            "sourceIP":"0.0.0.0/0",
            "destinationIP":"0.0.0.0/0"
            }
        }
}
```

### <a name="rule-counter-log"></a><span data-ttu-id="83328-149">Logg för räknaren</span><span class="sxs-lookup"><span data-stu-id="83328-149">Rule counter log</span></span>

<span data-ttu-id="83328-150">Den här loggfilen innehåller information om varje regel som tillämpas tooresources.</span><span class="sxs-lookup"><span data-stu-id="83328-150">This log contains information about each rule applied tooresources.</span></span> <span data-ttu-id="83328-151">hello följande exempeldata loggas varje gång som ska tillämpas:</span><span class="sxs-lookup"><span data-stu-id="83328-151">hello following example data is logged each time a rule is applied:</span></span>

```json
{
    "time": "[DATE-TIME]",
    "systemId": "007d0441-5d6b-41f6-8bfd-930db640ec03",
    "category": "NetworkSecurityGroupRuleCounter",
    "resourceId": "/SUBSCRIPTIONS/[SUBSCRIPTION ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME]TESTRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG-NAME]",
    "operationName": "NetworkSecurityGroupCounters",
    "properties": {
        "vnetResourceGuid":"{5E8AEC16-C728-441F-B0CA-791E1DBC2F4}",
        "subnetPrefix":"192.168.1.0/24",
        "macAddress":"00-0D-3A-92-6A-7C",
        "primaryIPv4Address":"192.168.1.4",
        "ruleName":"UserRule_default-allow-rdp",
        "direction":"In",
        "type":"allow",
        "matchedConnections":125
        }
}
```

## <a name="view-and-analyze-logs"></a><span data-ttu-id="83328-152">Visa och analysera loggar</span><span class="sxs-lookup"><span data-stu-id="83328-152">View and analyze logs</span></span>

<span data-ttu-id="83328-153">toolearn hur tooview aktivitet logga data, läsa hello [översikt över hello Azure-aktivitetsloggen](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="83328-153">toolearn how tooview activity log data, read hello [Overview of hello Azure Activity Log](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) article.</span></span> <span data-ttu-id="83328-154">toolearn hur tooview diagnostik logga data, läsa hello [översikt av Azure diagnostikloggar](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="83328-154">toolearn how tooview diagnostic log data, read hello [Overview of Azure Diagnostic Logs](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) article.</span></span> <span data-ttu-id="83328-155">Om du skickar diagnostik data tooLog Analytics, kan du använda hello [Azure Network Security Group analytics](../log-analytics/log-analytics-azure-networking-analytics.md) lösning för förbättrad insights (förhandsversion).</span><span class="sxs-lookup"><span data-stu-id="83328-155">If you send diagnostics data tooLog Analytics, you can use hello [Azure Network Security Group analytics](../log-analytics/log-analytics-azure-networking-analytics.md) (preview) management solution for enhanced insights.</span></span> 
