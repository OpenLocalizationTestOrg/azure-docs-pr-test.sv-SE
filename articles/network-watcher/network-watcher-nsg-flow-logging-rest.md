---
title: "Hantera Nätverkssäkerhetsgruppen flöde loggar med Azure Nätverksbevakaren - REST API | Microsoft Docs"
description: "Den här sidan förklarar hur du hanterar Nätverkssäkerhetsgruppen flöde loggar i Azure Nätverksbevakaren med REST API"
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
ms.openlocfilehash: c89a2ab4c39978771c940a819493b4e2283d5f9f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="configuring-network-security-group-flow-logs-using-rest-api"></a><span data-ttu-id="8200d-103">Konfigurera Nätverkssäkerhetsgruppen flöde loggar med hjälp av REST API</span><span class="sxs-lookup"><span data-stu-id="8200d-103">Configuring Network Security Group flow logs using REST API</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="8200d-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="8200d-104">Azure portal</span></span>](network-watcher-nsg-flow-logging-portal.md)
> - [<span data-ttu-id="8200d-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="8200d-105">PowerShell</span></span>](network-watcher-nsg-flow-logging-powershell.md)
> - [<span data-ttu-id="8200d-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="8200d-106">CLI 1.0</span></span>](network-watcher-nsg-flow-logging-cli-nodejs.md)
> - [<span data-ttu-id="8200d-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="8200d-107">CLI 2.0</span></span>](network-watcher-nsg-flow-logging-cli.md)
> - [<span data-ttu-id="8200d-108">REST-API</span><span class="sxs-lookup"><span data-stu-id="8200d-108">REST API</span></span>](network-watcher-nsg-flow-logging-rest.md)

<span data-ttu-id="8200d-109">Nätverkssäkerhetsgruppen flöde loggarna är en funktion i Nätverksbevakaren där du kan visa information om ingående och utgående IP-trafik via en Nätverkssäkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="8200d-109">Network Security Group flow logs are a feature of Network Watcher that allows you to view information about ingress and egress IP traffic through a Network Security Group.</span></span> <span data-ttu-id="8200d-110">Loggarna flödet skrivs i json-format och visa utgående och inkommande flöden på grundval av per regel, NIC flödet gäller för 5-tuppel information om flödet (källan/målet IP-källan/målet Port Protocol), och om trafiken tillåts eller nekas.</span><span class="sxs-lookup"><span data-stu-id="8200d-110">These flow logs are written in json format and show outbound and inbound flows on a per rule basis, the NIC the flow applies to, 5-tuple information about the flow (Source/Destination IP, Source/Destination Port, Protocol), and if the traffic was allowed or denied.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="8200d-111">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="8200d-111">Before you begin</span></span>

<span data-ttu-id="8200d-112">ARMclient används för att anropa REST-API med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8200d-112">ARMclient is used to call the REST API using PowerShell.</span></span> <span data-ttu-id="8200d-113">ARMClient hittas på chocolatey på [ARMClient på Chocolatey](https://chocolatey.org/packages/ARMClient)</span><span class="sxs-lookup"><span data-stu-id="8200d-113">ARMClient is found on chocolatey at [ARMClient on Chocolatey](https://chocolatey.org/packages/ARMClient)</span></span>

<span data-ttu-id="8200d-114">Det här scenariot förutsätter att du redan har följt stegen i [skapa en Nätverksbevakaren](network-watcher-create.md) att skapa en Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="8200d-114">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher.</span></span>

> [!Important]
> <span data-ttu-id="8200d-115">För nätverket Watcher REST API-anrop som resursgruppens namn i URI-begäran är resursgruppen som innehåller Nätverksbevakaren, inte resurserna du utför de diagnostiska åtgärderna på.</span><span class="sxs-lookup"><span data-stu-id="8200d-115">For Network Watcher REST API calls the resource group name in the request URI is the resource group that contains the Network Watcher, not the resources you are performing the diagnostic actions on.</span></span>

## <a name="scenario"></a><span data-ttu-id="8200d-116">Scenario</span><span class="sxs-lookup"><span data-stu-id="8200d-116">Scenario</span></span>

<span data-ttu-id="8200d-117">Det scenario som beskrivs i den här artikeln visar hur du aktivera, inaktivera och fråga flödet loggar med hjälp av REST-API.</span><span class="sxs-lookup"><span data-stu-id="8200d-117">The scenario covered in this article shows you how to enable, disable, and query flow logs using the REST API.</span></span> <span data-ttu-id="8200d-118">Läs mer om Nätverkssäkerhetsgrupp flöde loggings [Nätverkssäkerhetsgruppen flöde loggning - översikt över](network-watcher-nsg-flow-logging-overview.md).</span><span class="sxs-lookup"><span data-stu-id="8200d-118">To learn more about Network Security Group flow loggings, visit [Network Security Group flow logging - Overview](network-watcher-nsg-flow-logging-overview.md).</span></span>

<span data-ttu-id="8200d-119">I det här scenariot kommer du att:</span><span class="sxs-lookup"><span data-stu-id="8200d-119">In this scenario, you will:</span></span>

* <span data-ttu-id="8200d-120">Aktivera flödet loggar</span><span class="sxs-lookup"><span data-stu-id="8200d-120">Enable flow logs</span></span>
* <span data-ttu-id="8200d-121">Inaktivera flödet loggar</span><span class="sxs-lookup"><span data-stu-id="8200d-121">Disable flow logs</span></span>
* <span data-ttu-id="8200d-122">Fråga flödet loggar status</span><span class="sxs-lookup"><span data-stu-id="8200d-122">Query flow logs status</span></span>

## <a name="log-in-with-armclient"></a><span data-ttu-id="8200d-123">Logga in med ARMClient</span><span class="sxs-lookup"><span data-stu-id="8200d-123">Log in with ARMClient</span></span>

<span data-ttu-id="8200d-124">Logga in på armclient med dina autentiseringsuppgifter för Azure.</span><span class="sxs-lookup"><span data-stu-id="8200d-124">Log in to armclient with your Azure credentials.</span></span>

```PowerShell
armclient login
```

## <a name="register-insights-provider"></a><span data-ttu-id="8200d-125">Registrera providern insikter</span><span class="sxs-lookup"><span data-stu-id="8200d-125">Register Insights provider</span></span>

<span data-ttu-id="8200d-126">För att flöda loggning för att fungera korrekt, den **Microsoft.Insights** provider måste vara registrerad.</span><span class="sxs-lookup"><span data-stu-id="8200d-126">In order for flow logging to work successfully, the **Microsoft.Insights** provider must be registered.</span></span> <span data-ttu-id="8200d-127">Om du inte är säker på om den **Microsoft.Insights** provider är registrerad, kör följande skript.</span><span class="sxs-lookup"><span data-stu-id="8200d-127">If you are not sure if the **Microsoft.Insights** provider is registered, run the following script.</span></span>

```powershell
$subscriptionId = "00000000-0000-0000-0000-000000000000"
armclient post "https://management.azure.com//subscriptions/${subscriptionId}/providers/Microsoft.Insights/register?api-version=2016-09-01"
```

## <a name="enable-network-security-group-flow-logs"></a><span data-ttu-id="8200d-128">Aktivera Nätverkssäkerhetsgruppen flöde loggar</span><span class="sxs-lookup"><span data-stu-id="8200d-128">Enable Network Security Group flow logs</span></span>

<span data-ttu-id="8200d-129">Kommandot för att aktivera flödet loggarna visas i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="8200d-129">The command to enable flow logs is shown in the following example:</span></span>

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

<span data-ttu-id="8200d-130">Svaret som returnerades från föregående exempel är följande:</span><span class="sxs-lookup"><span data-stu-id="8200d-130">The response returned from the preceding example is as follows:</span></span>

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

## <a name="disable-network-security-group-flow-logs"></a><span data-ttu-id="8200d-131">Inaktivera Nätverkssäkerhetsgruppen flöde loggar</span><span class="sxs-lookup"><span data-stu-id="8200d-131">Disable Network Security Group flow logs</span></span>

<span data-ttu-id="8200d-132">Använd följande exempel för att inaktivera flödet loggar.</span><span class="sxs-lookup"><span data-stu-id="8200d-132">Use the following example to disable flow logs.</span></span> <span data-ttu-id="8200d-133">Anropet är detsamma som att aktivera flödet loggar, utom **FALSKT** har angetts för egenskapen enabled.</span><span class="sxs-lookup"><span data-stu-id="8200d-133">The call is the same as enabling flow logs, except **false** is set for the enabled property.</span></span>

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

<span data-ttu-id="8200d-134">Svaret som returnerades från föregående exempel är följande:</span><span class="sxs-lookup"><span data-stu-id="8200d-134">The response returned from the preceding example is as follows:</span></span>

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

## <a name="query-flow-logs"></a><span data-ttu-id="8200d-135">Frågan flödet loggar</span><span class="sxs-lookup"><span data-stu-id="8200d-135">Query flow logs</span></span>

<span data-ttu-id="8200d-136">Följande frågor för REST-anrop status för flöde loggar på en Nätverkssäkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="8200d-136">The following REST call queries the status of flow logs on a Network Security Group.</span></span>

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

<span data-ttu-id="8200d-137">Följande är ett exempel på svaret som returnerades:</span><span class="sxs-lookup"><span data-stu-id="8200d-137">The following is an example of the response returned:</span></span>

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

## <a name="download-a-flow-log"></a><span data-ttu-id="8200d-138">Hämta en logg flöde</span><span class="sxs-lookup"><span data-stu-id="8200d-138">Download a flow log</span></span>

<span data-ttu-id="8200d-139">Lagringsplatsen för en flödet logg definieras på Skapa.</span><span class="sxs-lookup"><span data-stu-id="8200d-139">The storage location of a flow log is defined at creation.</span></span> <span data-ttu-id="8200d-140">Ett enkelt verktyg för att komma åt dessa flödet loggar som sparats i ett lagringskonto är Microsoft Azure Lagringsutforskaren, som kan hämtas här: http://storageexplorer.com/</span><span class="sxs-lookup"><span data-stu-id="8200d-140">A convenient tool to access these flow logs saved to a storage account is Microsoft Azure Storage Explorer, which can be downloaded here:  http://storageexplorer.com/</span></span>

<span data-ttu-id="8200d-141">Om ett storage-konto anges sparas paket avbilda filer till ett lagringskonto på följande plats:</span><span class="sxs-lookup"><span data-stu-id="8200d-141">If a storage account is specified, packet capture files are saved to a storage account at the following location:</span></span>

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId%3D/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/microsoft.network/networksecuritygroups/{nsgName}/{year}/{month}/{day}/PT1H.json
```

## <a name="next-steps"></a><span data-ttu-id="8200d-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8200d-142">Next steps</span></span>

<span data-ttu-id="8200d-143">Lär dig hur du [visualisera dina NSG flödet loggar med PowerBI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span><span class="sxs-lookup"><span data-stu-id="8200d-143">Learn how to [Visualize your NSG flow logs with PowerBI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span></span>

<span data-ttu-id="8200d-144">Lär dig hur du [visualisera dina NSG flödet loggar med öppen källkod verktyg](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)</span><span class="sxs-lookup"><span data-stu-id="8200d-144">Learn how to [Visualize your NSG flow logs with open source tools](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)</span></span>
