---
title: "aaaManage Nätverkssäkerhetsgruppen flöde loggar med Azure Nätverksbevakaren - REST API | Microsoft Docs"
description: "Den här sidan förklarar hur toomanage Nätverkssäkerhetsgruppen flöde loggar i Azure Nätverksbevakaren med REST API"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 2ab25379-0fd3-4bfe-9d82-425dfc7ad6bb
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: be81e35f4d01c67efef99773e9b4e2ae4b8e209e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-network-security-group-flow-logs-using-rest-api"></a><span data-ttu-id="47ac0-103">Konfigurera Nätverkssäkerhetsgruppen flöde loggar med hjälp av REST API</span><span class="sxs-lookup"><span data-stu-id="47ac0-103">Configuring Network Security Group flow logs using REST API</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="47ac0-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="47ac0-104">Azure portal</span></span>](network-watcher-nsg-flow-logging-portal.md)
> - [<span data-ttu-id="47ac0-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="47ac0-105">PowerShell</span></span>](network-watcher-nsg-flow-logging-powershell.md)
> - [<span data-ttu-id="47ac0-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="47ac0-106">CLI 1.0</span></span>](network-watcher-nsg-flow-logging-cli-nodejs.md)
> - [<span data-ttu-id="47ac0-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="47ac0-107">CLI 2.0</span></span>](network-watcher-nsg-flow-logging-cli.md)
> - [<span data-ttu-id="47ac0-108">REST-API</span><span class="sxs-lookup"><span data-stu-id="47ac0-108">REST API</span></span>](network-watcher-nsg-flow-logging-rest.md)

<span data-ttu-id="47ac0-109">Nätverkssäkerhetsgruppen flöde loggarna är en funktion i Nätverksbevakaren som gör att du tooview information om ingående och utgående IP-trafik via en Nätverkssäkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="47ac0-109">Network Security Group flow logs are a feature of Network Watcher that allows you tooview information about ingress and egress IP traffic through a Network Security Group.</span></span> <span data-ttu-id="47ac0-110">Loggarna flödet skrivs i json-format och visa utgående och inkommande flöden på grundval av per regel hello NIC hello flödet gäller för 5-tuppel information om hello flödet (källan/målet IP-källan/målet Port Protocol) och om hello trafik tilläts eller nekad.</span><span class="sxs-lookup"><span data-stu-id="47ac0-110">These flow logs are written in json format and show outbound and inbound flows on a per rule basis, hello NIC hello flow applies to, 5-tuple information about hello flow (Source/Destination IP, Source/Destination Port, Protocol), and if hello traffic was allowed or denied.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="47ac0-111">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="47ac0-111">Before you begin</span></span>

<span data-ttu-id="47ac0-112">ARMclient är används toocall hello REST-API med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="47ac0-112">ARMclient is used toocall hello REST API using PowerShell.</span></span> <span data-ttu-id="47ac0-113">ARMClient hittas på chocolatey på [ARMClient på Chocolatey](https://chocolatey.org/packages/ARMClient)</span><span class="sxs-lookup"><span data-stu-id="47ac0-113">ARMClient is found on chocolatey at [ARMClient on Chocolatey](https://chocolatey.org/packages/ARMClient)</span></span>

<span data-ttu-id="47ac0-114">Det här scenariot förutsätter att du redan har följt stegen hello i [skapa en Nätverksbevakaren](network-watcher-create.md) toocreate en Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="47ac0-114">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span>

> [!Important]
> <span data-ttu-id="47ac0-115">För nätverket Watcher REST API-anrop hello resursgruppens namn i hello Begärd URI är hello resursgruppen som innehåller hello Nätverksbevakaren, inte hello resurser som du utför hello diagnostiska åtgärder på.</span><span class="sxs-lookup"><span data-stu-id="47ac0-115">For Network Watcher REST API calls hello resource group name in hello request URI is hello resource group that contains hello Network Watcher, not hello resources you are performing hello diagnostic actions on.</span></span>

## <a name="scenario"></a><span data-ttu-id="47ac0-116">Scenario</span><span class="sxs-lookup"><span data-stu-id="47ac0-116">Scenario</span></span>

<span data-ttu-id="47ac0-117">hello-scenario som beskrivs i den här artikeln visar hur tooenable, inaktivera och fråga flöda loggar med hello REST API.</span><span class="sxs-lookup"><span data-stu-id="47ac0-117">hello scenario covered in this article shows you how tooenable, disable, and query flow logs using hello REST API.</span></span> <span data-ttu-id="47ac0-118">toolearn mer om Nätverkssäkerhetsgrupp flöde loggings finns [Nätverkssäkerhetsgruppen flöde loggning - översikt över](network-watcher-nsg-flow-logging-overview.md).</span><span class="sxs-lookup"><span data-stu-id="47ac0-118">toolearn more about Network Security Group flow loggings, visit [Network Security Group flow logging - Overview](network-watcher-nsg-flow-logging-overview.md).</span></span>

<span data-ttu-id="47ac0-119">I det här scenariot kommer du att:</span><span class="sxs-lookup"><span data-stu-id="47ac0-119">In this scenario, you will:</span></span>

* <span data-ttu-id="47ac0-120">Aktivera flödet loggar</span><span class="sxs-lookup"><span data-stu-id="47ac0-120">Enable flow logs</span></span>
* <span data-ttu-id="47ac0-121">Inaktivera flödet loggar</span><span class="sxs-lookup"><span data-stu-id="47ac0-121">Disable flow logs</span></span>
* <span data-ttu-id="47ac0-122">Fråga flödet loggar status</span><span class="sxs-lookup"><span data-stu-id="47ac0-122">Query flow logs status</span></span>

## <a name="log-in-with-armclient"></a><span data-ttu-id="47ac0-123">Logga in med ARMClient</span><span class="sxs-lookup"><span data-stu-id="47ac0-123">Log in with ARMClient</span></span>

<span data-ttu-id="47ac0-124">Logga in tooarmclient med dina autentiseringsuppgifter för Azure.</span><span class="sxs-lookup"><span data-stu-id="47ac0-124">Log in tooarmclient with your Azure credentials.</span></span>

```PowerShell
armclient login
```

## <a name="register-insights-provider"></a><span data-ttu-id="47ac0-125">Registrera providern insikter</span><span class="sxs-lookup"><span data-stu-id="47ac0-125">Register Insights provider</span></span>

<span data-ttu-id="47ac0-126">Flöde för loggning toowork, hello **Microsoft.Insights** provider måste vara registrerad.</span><span class="sxs-lookup"><span data-stu-id="47ac0-126">In order for flow logging toowork successfully, hello **Microsoft.Insights** provider must be registered.</span></span> <span data-ttu-id="47ac0-127">Om du inte är säker på om hello **Microsoft.Insights** provider är registrerad, kör hello följande skript.</span><span class="sxs-lookup"><span data-stu-id="47ac0-127">If you are not sure if hello **Microsoft.Insights** provider is registered, run hello following script.</span></span>

```powershell
$subscriptionId = "00000000-0000-0000-0000-000000000000"
armclient post "https://management.azure.com//subscriptions/${subscriptionId}/providers/Microsoft.Insights/register?api-version=2016-09-01"
```

## <a name="enable-network-security-group-flow-logs"></a><span data-ttu-id="47ac0-128">Aktivera Nätverkssäkerhetsgruppen flöde loggar</span><span class="sxs-lookup"><span data-stu-id="47ac0-128">Enable Network Security Group flow logs</span></span>

<span data-ttu-id="47ac0-129">hello kommandot tooenable flöde loggar visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="47ac0-129">hello command tooenable flow logs is shown in hello following example:</span></span>

```powershell
$subscriptionId = "00000000-0000-0000-0000-000000000000"
$targetUri = "" # example /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName/providers/Microsoft.Network/networkSecurityGroups/{nsgName}"
$storageId = "/subscriptions/00000000-0000-0000-0000-000000000000/{resourceGroupName/providers/Microsoft.Storage/storageAccounts/{saName}"
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$requestBody = @"
{
    'targetResourceId': '${targetUri}',
    'properties': {
    'storageId': '${storageId}',
    'enabled': 'true',
    'retentionPolicy' : {
            days: 5,
            enabled: true
        }
    }
}
"@

armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/configureFlowLog?api-version=2016-12-01" $requestBody
```

<span data-ttu-id="47ac0-130">hello svar returneras från hello föregående exempel är följande:</span><span class="sxs-lookup"><span data-stu-id="47ac0-130">hello response returned from hello preceding example is as follows:</span></span>

```json
{
  "targetResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}",
  "properties": {
    "storageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{saName}",
    "enabled": true,
    "retentionPolicy": {
      "days": 5,
      "enabled": true
    }
  }
}
```

## <a name="disable-network-security-group-flow-logs"></a><span data-ttu-id="47ac0-131">Inaktivera Nätverkssäkerhetsgruppen flöde loggar</span><span class="sxs-lookup"><span data-stu-id="47ac0-131">Disable Network Security Group flow logs</span></span>

<span data-ttu-id="47ac0-132">Använd hello följande exempel toodisable flöde loggar.</span><span class="sxs-lookup"><span data-stu-id="47ac0-132">Use hello following example toodisable flow logs.</span></span> <span data-ttu-id="47ac0-133">hello anropet är hello detsamma som att aktivera flödet loggar, utom **FALSKT** har angetts för egenskapen hello aktiverad.</span><span class="sxs-lookup"><span data-stu-id="47ac0-133">hello call is hello same as enabling flow logs, except **false** is set for hello enabled property.</span></span>

```powershell
$subscriptionId = "00000000-0000-0000-0000-000000000000"
$targetUri = "" # example /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName/providers/Microsoft.Network/networkSecurityGroups/{nsgName}"
$storageId = "/subscriptions/00000000-0000-0000-0000-000000000000/{resourceGroupName/providers/Microsoft.Storage/storageAccounts/{saName}"
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$requestBody = @"
{
    'targetResourceId': '${targetUri}',
    'properties': {
    'storageId': '${storageId}',
    'enabled': 'false',
    'retentionPolicy' : {
            days: 5,
            enabled: true
        }
    }
}
"@

armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/configureFlowLog?api-version=2016-12-01" $requestBody
```

<span data-ttu-id="47ac0-134">hello svar returneras från hello föregående exempel är följande:</span><span class="sxs-lookup"><span data-stu-id="47ac0-134">hello response returned from hello preceding example is as follows:</span></span>

```json
{
  "targetResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}",
  "properties": {
    "storageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{saName}",
    "enabled": false,
    "retentionPolicy": {
      "days": 5,
      "enabled": true
    }
  }
}
```

## <a name="query-flow-logs"></a><span data-ttu-id="47ac0-135">Frågan flödet loggar</span><span class="sxs-lookup"><span data-stu-id="47ac0-135">Query flow logs</span></span>

<span data-ttu-id="47ac0-136">hello följande frågor hello status för flödet av REST-anrop loggar på en Nätverkssäkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="47ac0-136">hello following REST call queries hello status of flow logs on a Network Security Group.</span></span>

```powershell
$subscriptionId = "00000000-0000-0000-0000-000000000000"
$targetUri = "" # example /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName/providers/Microsoft.Network/networkSecurityGroups/{nsgName}"
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$requestBody = @"
{
    'targetResourceId': '${targetUri}',
}
"@

armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/queryFlowLogStatus?api-version=2016-12-01" $requestBody
```

<span data-ttu-id="47ac0-137">hello följande är ett exempel på hello svaret som returnerades:</span><span class="sxs-lookup"><span data-stu-id="47ac0-137">hello following is an example of hello response returned:</span></span>

```json
{
  "targetResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}",
  "properties": {
    "storageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{saName}",
    "enabled": true,
   "retentionPolicy": {
      "days": 5,
      "enabled": true
    }
  }
}
```

## <a name="download-a-flow-log"></a><span data-ttu-id="47ac0-138">Hämta en logg flöde</span><span class="sxs-lookup"><span data-stu-id="47ac0-138">Download a flow log</span></span>

<span data-ttu-id="47ac0-139">hello lagringsplatsen för en flödet logg definieras på Skapa.</span><span class="sxs-lookup"><span data-stu-id="47ac0-139">hello storage location of a flow log is defined at creation.</span></span> <span data-ttu-id="47ac0-140">En lämplig verktyget tooaccess dessa flödet loggar som sparas tooa storage-konto är Microsoft Azure Lagringsutforskaren, som kan hämtas här: http://storageexplorer.com/</span><span class="sxs-lookup"><span data-stu-id="47ac0-140">A convenient tool tooaccess these flow logs saved tooa storage account is Microsoft Azure Storage Explorer, which can be downloaded here:  http://storageexplorer.com/</span></span>

<span data-ttu-id="47ac0-141">Om ett lagringskonto har angetts sparas paket avbilda filer tooa storage-konto på hello följande plats:</span><span class="sxs-lookup"><span data-stu-id="47ac0-141">If a storage account is specified, packet capture files are saved tooa storage account at hello following location:</span></span>

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId%3D/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/microsoft.network/networksecuritygroups/{nsgName}/{year}/{month}/{day}/PT1H.json
```

## <a name="next-steps"></a><span data-ttu-id="47ac0-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="47ac0-142">Next steps</span></span>

<span data-ttu-id="47ac0-143">Lär dig hur för[visualisera dina NSG flödet loggar med PowerBI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span><span class="sxs-lookup"><span data-stu-id="47ac0-143">Learn how too[Visualize your NSG flow logs with PowerBI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span></span>

<span data-ttu-id="47ac0-144">Lär dig hur för[visualisera dina NSG flödet loggar med öppen källkod verktyg](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)</span><span class="sxs-lookup"><span data-stu-id="47ac0-144">Learn how too[Visualize your NSG flow logs with open source tools](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)</span></span>
