---
title: "aaaManage Network Security Group flöda loggar med Azure Nätverksbevakaren - PowerShell | Microsoft Docs"
description: "Den här sidan förklarar hur toomanage Network Security Group flöda loggar i Azure Nätverksbevakaren med PowerShell"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 2dfc3112-8294-4357-b2f8-f81840da67d3
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 987e8728fb6459fd6ff8eb5cd3d36ff855f2ccfc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-network-security-group-flow-logs-with-powershell"></a><span data-ttu-id="c5cdb-103">Konfigurera Network Security Group flöda loggar med PowerShell</span><span class="sxs-lookup"><span data-stu-id="c5cdb-103">Configuring Network Security Group Flow logs with PowerShell</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="c5cdb-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c5cdb-104">Azure portal</span></span>](network-watcher-nsg-flow-logging-portal.md)
> - [<span data-ttu-id="c5cdb-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="c5cdb-105">PowerShell</span></span>](network-watcher-nsg-flow-logging-powershell.md)
> - [<span data-ttu-id="c5cdb-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="c5cdb-106">CLI 1.0</span></span>](network-watcher-nsg-flow-logging-cli-nodejs.md)
> - [<span data-ttu-id="c5cdb-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="c5cdb-107">CLI 2.0</span></span>](network-watcher-nsg-flow-logging-cli.md)
> - [<span data-ttu-id="c5cdb-108">REST-API</span><span class="sxs-lookup"><span data-stu-id="c5cdb-108">REST API</span></span>](network-watcher-nsg-flow-logging-rest.md)

<span data-ttu-id="c5cdb-109">Nätverkssäkerhetsgruppen flöde loggarna är en funktion i Nätverksbevakaren som gör att du tooview information om ingående och utgående IP-trafik via en Nätverkssäkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="c5cdb-109">Network Security Group flow logs are a feature of Network Watcher that allows you tooview information about ingress and egress IP traffic through a Network Security Group.</span></span> <span data-ttu-id="c5cdb-110">Loggarna flödet skrivs i json-format och visa utgående och inkommande flöden på grundval av per regel hello NIC hello flödet gäller för 5-tuppel information om hello flödet (källan/målet IP-källan/målet Port Protocol) och om hello trafik tilläts eller nekad.</span><span class="sxs-lookup"><span data-stu-id="c5cdb-110">These flow logs are written in json format and show outbound and inbound flows on a per rule basis, hello NIC hello flow applies to, 5-tuple information about hello flow (Source/Destination IP, Source/Destination Port, Protocol), and if hello traffic was allowed or denied.</span></span>

## <a name="register-insights-provider"></a><span data-ttu-id="c5cdb-111">Registrera providern insikter</span><span class="sxs-lookup"><span data-stu-id="c5cdb-111">Register Insights provider</span></span>

<span data-ttu-id="c5cdb-112">Flöde för loggning toowork, hello **Microsoft.Insights** provider måste vara registrerad.</span><span class="sxs-lookup"><span data-stu-id="c5cdb-112">In order for flow logging toowork successfully, hello **Microsoft.Insights** provider must be registered.</span></span> <span data-ttu-id="c5cdb-113">Om du inte är säker på om hello **Microsoft.Insights** provider är registrerad, kör hello följande skript.</span><span class="sxs-lookup"><span data-stu-id="c5cdb-113">If you are not sure if hello **Microsoft.Insights** provider is registered, run hello following script.</span></span>

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Insights
```

## <a name="enable-network-security-group-flow-logs"></a><span data-ttu-id="c5cdb-114">Aktivera Nätverkssäkerhetsgruppen flöde loggar</span><span class="sxs-lookup"><span data-stu-id="c5cdb-114">Enable Network Security Group Flow logs</span></span>

<span data-ttu-id="c5cdb-115">hello kommandot tooenable flöde loggar visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="c5cdb-115">hello command tooenable flow logs is shown in hello following example:</span></span>

```powershell
$NW = Get-AzurermNetworkWatcher -ResourceGroupName NetworkWatcherRg -Name NetworkWatcher_westcentralus
$nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName nsgRG-Name nsgName
$storageAccount = Get-AzureRmStorageAccount -ResourceGroupName StorageRG -Name contosostorage123
Get-AzureRmNetworkWatcherFlowLogStatus -NetworkWatcher $NW -TargetResourceId $nsg.Id
Set-AzureRmNetworkWatcherConfigFlowLog -NetworkWatcher $NW -TargetResourceId $nsg.Id -StorageAccountId $storageAccount.Id -EnableFlowLog $true
```

## <a name="disable-network-security-group-flow-logs"></a><span data-ttu-id="c5cdb-116">Inaktivera Nätverkssäkerhetsgruppen flöde loggar</span><span class="sxs-lookup"><span data-stu-id="c5cdb-116">Disable Network Security Group Flow logs</span></span>

<span data-ttu-id="c5cdb-117">Använd hello följande exempel toodisable flöde loggar:</span><span class="sxs-lookup"><span data-stu-id="c5cdb-117">Use hello following example toodisable flow logs:</span></span>

```powershell
Set-AzureRmNetworkWatcherConfigFlowLog -NetworkWatcher $NW -TargetResourceId $nsg.Id -StorageAccountId $storageAccount.Id -EnableFlowLog $false
```

## <a name="download-a-flow-log"></a><span data-ttu-id="c5cdb-118">Hämta en logg flöde</span><span class="sxs-lookup"><span data-stu-id="c5cdb-118">Download a Flow log</span></span>

<span data-ttu-id="c5cdb-119">hello lagringsplatsen för en flödet logg definieras på Skapa.</span><span class="sxs-lookup"><span data-stu-id="c5cdb-119">hello storage location of a flow log is defined at creation.</span></span> <span data-ttu-id="c5cdb-120">En lämplig verktyget tooaccess dessa flödet loggar som sparas tooa storage-konto är Microsoft Azure Lagringsutforskaren, som kan hämtas här: http://storageexplorer.com/</span><span class="sxs-lookup"><span data-stu-id="c5cdb-120">A convenient tool tooaccess these flow logs saved tooa storage account is Microsoft Azure Storage Explorer, which can be downloaded here:  http://storageexplorer.com/</span></span>

<span data-ttu-id="c5cdb-121">Om ett lagringskonto har angetts sparas paket avbilda filer tooa storage-konto på hello följande plats:</span><span class="sxs-lookup"><span data-stu-id="c5cdb-121">If a storage account is specified, packet capture files are saved tooa storage account at hello following location:</span></span>

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId%3D/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/microsoft.network/networksecuritygroups/{nsgName}/{year}/{month}/{day}/PT1H.json
```

<span data-ttu-id="c5cdb-122">Information om hello struktur hello loggen finns [Network Security Group flöda logg: översikt](network-watcher-nsg-flow-logging-overview.md)</span><span class="sxs-lookup"><span data-stu-id="c5cdb-122">For information about hello structure of hello log visit [Network Security Group Flow log Overview](network-watcher-nsg-flow-logging-overview.md)</span></span>

## <a name="next-steps"></a><span data-ttu-id="c5cdb-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c5cdb-123">Next Steps</span></span>

<span data-ttu-id="c5cdb-124">Lär dig hur för[visualisera dina NSG flödet loggar med PowerBI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span><span class="sxs-lookup"><span data-stu-id="c5cdb-124">Learn how too[Visualize your NSG flow logs with PowerBI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span></span>

<span data-ttu-id="c5cdb-125">Lär dig hur för[visualisera dina NSG flödet loggar med öppen källkod verktyg](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)</span><span class="sxs-lookup"><span data-stu-id="c5cdb-125">Learn how too[Visualize your NSG flow logs with open source tools](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)</span></span>
