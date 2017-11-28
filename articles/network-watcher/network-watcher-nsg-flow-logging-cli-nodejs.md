---
title: "aaaManage Network Security Group flöda loggar med Nätverksbevakaren Azure - Azure CLI 1.0 | Microsoft Docs"
description: "Den här sidan förklarar hur toomanage Network Security Group flöda loggar i Azure Nätverksbevakaren med Azure CLI 1.0"
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
ms.openlocfilehash: 2535eea665a99cffe7569a8d976333435f946a80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-network-security-group-flow-logs-with-azure-cli-10"></a><span data-ttu-id="47c57-103">Konfigurera Network Security Group flöda loggar med Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="47c57-103">Configuring Network Security Group Flow logs with Azure CLI 1.0</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="47c57-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="47c57-104">Azure portal</span></span>](network-watcher-nsg-flow-logging-portal.md)
> - [<span data-ttu-id="47c57-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="47c57-105">PowerShell</span></span>](network-watcher-nsg-flow-logging-powershell.md)
> - [<span data-ttu-id="47c57-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="47c57-106">CLI 1.0</span></span>](network-watcher-nsg-flow-logging-cli-nodejs.md)
> - [<span data-ttu-id="47c57-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="47c57-107">CLI 2.0</span></span>](network-watcher-nsg-flow-logging-cli.md)
> - [<span data-ttu-id="47c57-108">REST-API</span><span class="sxs-lookup"><span data-stu-id="47c57-108">REST API</span></span>](network-watcher-nsg-flow-logging-rest.md)

<span data-ttu-id="47c57-109">Nätverkssäkerhetsgruppen flöde loggarna är en funktion i Nätverksbevakaren som gör att du tooview information om ingående och utgående IP-trafik via en Nätverkssäkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="47c57-109">Network Security Group flow logs are a feature of Network Watcher that allows you tooview information about ingress and egress IP traffic through a Network Security Group.</span></span> <span data-ttu-id="47c57-110">Loggarna flödet skrivs i json-format och visa utgående och inkommande flöden på grundval av per regel hello NIC hello flödet gäller för 5-tuppel information om hello flödet (källan/målet IP-källan/målet Port Protocol) och om hello trafik tilläts eller nekad.</span><span class="sxs-lookup"><span data-stu-id="47c57-110">These flow logs are written in json format and show outbound and inbound flows on a per rule basis, hello NIC hello flow applies to, 5-tuple information about hello flow (Source/Destination IP, Source/Destination Port, Protocol), and if hello traffic was allowed or denied.</span></span>

<span data-ttu-id="47c57-111">Den här artikeln använder plattformsoberoende Azure CLI version 1.0, som är tillgänglig för Windows, Mac och Linux.</span><span class="sxs-lookup"><span data-stu-id="47c57-111">This article uses cross-platform Azure CLI 1.0, which is available for Windows, Mac and Linux.</span></span> <span data-ttu-id="47c57-112">Nätverksbevakaren använder för närvarande Azure CLI 1.0 för CLI-stöd.</span><span class="sxs-lookup"><span data-stu-id="47c57-112">Network Watcher currently uses Azure CLI 1.0 for CLI support.</span></span>

## <a name="register-insights-provider"></a><span data-ttu-id="47c57-113">Registrera providern insikter</span><span class="sxs-lookup"><span data-stu-id="47c57-113">Register Insights provider</span></span>

<span data-ttu-id="47c57-114">Flöde för loggning toowork, hello **Microsoft.Insights** provider måste vara registrerad.</span><span class="sxs-lookup"><span data-stu-id="47c57-114">In order for flow logging toowork successfully, hello **Microsoft.Insights** provider must be registered.</span></span> <span data-ttu-id="47c57-115">Om du inte är säker på om hello **Microsoft.Insights** provider är registrerad, kör hello följande skript.</span><span class="sxs-lookup"><span data-stu-id="47c57-115">If you are not sure if hello **Microsoft.Insights** provider is registered, run hello following script.</span></span>

```azurecli
azure provider register --namespace Microsoft.Insights --subscription <subscriptionid>
```

## <a name="enable-network-security-group-flow-logs"></a><span data-ttu-id="47c57-116">Aktivera Nätverkssäkerhetsgruppen flöde loggar</span><span class="sxs-lookup"><span data-stu-id="47c57-116">Enable Network Security Group Flow logs</span></span>

<span data-ttu-id="47c57-117">hello kommandot tooenable flöde loggar visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="47c57-117">hello command tooenable flow logs is shown in hello following example:</span></span>

```azurecli
azure network watcher configure-flow-log -g resourceGroupName -n networkWatcherName -t nsgId -i storageAccountId -e true
```

## <a name="disable-network-security-group-flow-logs"></a><span data-ttu-id="47c57-118">Inaktivera Nätverkssäkerhetsgruppen flöde loggar</span><span class="sxs-lookup"><span data-stu-id="47c57-118">Disable Network Security Group Flow logs</span></span>

<span data-ttu-id="47c57-119">Använd hello följande exempel toodisable flöde loggar:</span><span class="sxs-lookup"><span data-stu-id="47c57-119">Use hello following example toodisable flow logs:</span></span>

```azurecli
azure network watcher configure-flow-log -g resourceGroupName -n networkWatcherName -t nsgId -i storageAccountId -e false
```

## <a name="download-a-flow-log"></a><span data-ttu-id="47c57-120">Hämta en logg flöde</span><span class="sxs-lookup"><span data-stu-id="47c57-120">Download a Flow log</span></span>

<span data-ttu-id="47c57-121">hello lagringsplatsen för en flödet logg definieras på Skapa.</span><span class="sxs-lookup"><span data-stu-id="47c57-121">hello storage location of a flow log is defined at creation.</span></span> <span data-ttu-id="47c57-122">En lämplig verktyget tooaccess dessa flödet loggar som sparas tooa storage-konto är Microsoft Azure Lagringsutforskaren, som kan hämtas här: http://storageexplorer.com/</span><span class="sxs-lookup"><span data-stu-id="47c57-122">A convenient tool tooaccess these flow logs saved tooa storage account is Microsoft Azure Storage Explorer, which can be downloaded here:  http://storageexplorer.com/</span></span>

<span data-ttu-id="47c57-123">Om ett lagringskonto har angetts sparas paket avbilda filer tooa storage-konto på hello följande plats:</span><span class="sxs-lookup"><span data-stu-id="47c57-123">If a storage account is specified, packet capture files are saved tooa storage account at hello following location:</span></span>

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId%3D/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/microsoft.network/networksecuritygroups/{nsgName}/{year}/{month}/{day}/PT1H.json
```

<span data-ttu-id="47c57-124">Information om hello struktur hello loggen finns [Network Security Group flöda logg: översikt](network-watcher-nsg-flow-logging-overview.md)</span><span class="sxs-lookup"><span data-stu-id="47c57-124">For information about hello structure of hello log visit [Network Security Group Flow log Overview](network-watcher-nsg-flow-logging-overview.md)</span></span>

## <a name="next-steps"></a><span data-ttu-id="47c57-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="47c57-125">Next Steps</span></span>

<span data-ttu-id="47c57-126">Lär dig hur för[visualisera dina NSG flödet loggar med PowerBI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span><span class="sxs-lookup"><span data-stu-id="47c57-126">Learn how too[Visualize your NSG flow logs with PowerBI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span></span>

<span data-ttu-id="47c57-127">Lär dig hur för[visualisera dina NSG flödet loggar med öppen källkod verktyg](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)</span><span class="sxs-lookup"><span data-stu-id="47c57-127">Learn how too[Visualize your NSG flow logs with open source tools](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)</span></span>
