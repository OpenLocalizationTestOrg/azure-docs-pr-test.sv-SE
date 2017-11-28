---
title: "aaaManage Network Security Group flöda loggar med Nätverksbevakaren Azure - Azure CLI | Microsoft Docs"
description: "Den här sidan förklarar hur toomanage Network Security Group flöda loggar i Azure Nätverksbevakaren med Azure CLI"
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
ms.openlocfilehash: 2d0b02e7d0a5a9ab20beb491d49a5747f976a079
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-network-security-group-flow-logs-with-azure-cli"></a><span data-ttu-id="5bca4-103">Konfigurera Network Security Group flöda loggar med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="5bca4-103">Configuring Network Security Group Flow logs with Azure CLI</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="5bca4-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="5bca4-104">Azure portal</span></span>](network-watcher-nsg-flow-logging-portal.md)
> - [<span data-ttu-id="5bca4-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="5bca4-105">PowerShell</span></span>](network-watcher-nsg-flow-logging-powershell.md)
> - [<span data-ttu-id="5bca4-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="5bca4-106">CLI 1.0</span></span>](network-watcher-nsg-flow-logging-cli-nodejs.md)
> - [<span data-ttu-id="5bca4-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="5bca4-107">CLI 2.0</span></span>](network-watcher-nsg-flow-logging-cli.md)
> - [<span data-ttu-id="5bca4-108">REST-API</span><span class="sxs-lookup"><span data-stu-id="5bca4-108">REST API</span></span>](network-watcher-nsg-flow-logging-rest.md)

<span data-ttu-id="5bca4-109">Nätverkssäkerhetsgruppen flöde loggarna är en funktion i Nätverksbevakaren som gör att du tooview information om ingående och utgående IP-trafik via en Nätverkssäkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="5bca4-109">Network Security Group flow logs are a feature of Network Watcher that allows you tooview information about ingress and egress IP traffic through a Network Security Group.</span></span> <span data-ttu-id="5bca4-110">Loggarna flödet skrivs i json-format och visa utgående och inkommande flöden på grundval av per regel hello NIC hello flödet gäller för 5-tuppel information om hello flödet (källan/målet IP-källan/målet Port Protocol) och om hello trafik tilläts eller nekad.</span><span class="sxs-lookup"><span data-stu-id="5bca4-110">These flow logs are written in json format and show outbound and inbound flows on a per rule basis, hello NIC hello flow applies to, 5-tuple information about hello flow (Source/Destination IP, Source/Destination Port, Protocol), and if hello traffic was allowed or denied.</span></span>

<span data-ttu-id="5bca4-111">Den här artikeln använder våra nästa generations CLI för hello management resursdistributionsmodell, Azure CLI 2.0, som är tillgänglig för Windows, Mac och Linux.</span><span class="sxs-lookup"><span data-stu-id="5bca4-111">This article uses our next generation CLI for hello resource management deployment model, Azure CLI 2.0, which is available for Windows, Mac and Linux.</span></span>

<span data-ttu-id="5bca4-112">tooperform hello stegen i den här artikeln, måste du för[installerar hello Azure-kommandoradsgränssnittet för Mac, Linux och Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="5bca4-112">tooperform hello steps in this article, you need too[install hello Azure Command-Line Interface for Mac, Linux, and Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span>

## <a name="register-insights-provider"></a><span data-ttu-id="5bca4-113">Registrera providern insikter</span><span class="sxs-lookup"><span data-stu-id="5bca4-113">Register Insights provider</span></span>

<span data-ttu-id="5bca4-114">Flöde för loggning toowork, hello **Microsoft.Insights** provider måste vara registrerad.</span><span class="sxs-lookup"><span data-stu-id="5bca4-114">In order for flow logging toowork successfully, hello **Microsoft.Insights** provider must be registered.</span></span> <span data-ttu-id="5bca4-115">Om du inte är säker på om hello **Microsoft.Insights** provider är registrerad, kör hello följande skript.</span><span class="sxs-lookup"><span data-stu-id="5bca4-115">If you are not sure if hello **Microsoft.Insights** provider is registered, run hello following script.</span></span>

```azurecli
az provider register --namespace Microsoft.Insights
```

## <a name="enable-network-security-group-flow-logs"></a><span data-ttu-id="5bca4-116">Aktivera Nätverkssäkerhetsgruppen flöde loggar</span><span class="sxs-lookup"><span data-stu-id="5bca4-116">Enable Network Security Group Flow logs</span></span>

<span data-ttu-id="5bca4-117">hello kommandot tooenable flöde loggar visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="5bca4-117">hello command tooenable flow logs is shown in hello following example:</span></span>

```azurecli
az network watcher flow-log configure --resource-group resourceGroupName --enabled true --nsg nsgName --storage-account storageAccountName
```

## <a name="disable-network-security-group-flow-logs"></a><span data-ttu-id="5bca4-118">Inaktivera Nätverkssäkerhetsgruppen flöde loggar</span><span class="sxs-lookup"><span data-stu-id="5bca4-118">Disable Network Security Group Flow logs</span></span>

<span data-ttu-id="5bca4-119">Använd hello följande exempel toodisable flöde loggar:</span><span class="sxs-lookup"><span data-stu-id="5bca4-119">Use hello following example toodisable flow logs:</span></span>

```azurecli
az network watcher flow-log configure --resource-group resourceGroupName --enabled false --nsg nsgName
```

## <a name="download-a-flow-log"></a><span data-ttu-id="5bca4-120">Hämta en logg flöde</span><span class="sxs-lookup"><span data-stu-id="5bca4-120">Download a Flow log</span></span>

<span data-ttu-id="5bca4-121">hello lagringsplatsen för en flödet logg definieras på Skapa.</span><span class="sxs-lookup"><span data-stu-id="5bca4-121">hello storage location of a flow log is defined at creation.</span></span> <span data-ttu-id="5bca4-122">En lämplig verktyget tooaccess dessa flödet loggar som sparas tooa storage-konto är Microsoft Azure Lagringsutforskaren, som kan hämtas här: http://storageexplorer.com/</span><span class="sxs-lookup"><span data-stu-id="5bca4-122">A convenient tool tooaccess these flow logs saved tooa storage account is Microsoft Azure Storage Explorer, which can be downloaded here:  http://storageexplorer.com/</span></span>

<span data-ttu-id="5bca4-123">Om ett lagringskonto har angetts sparas paket avbilda filer tooa storage-konto på hello följande plats:</span><span class="sxs-lookup"><span data-stu-id="5bca4-123">If a storage account is specified, packet capture files are saved tooa storage account at hello following location:</span></span>

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId%3D/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/microsoft.network/networksecuritygroups/{nsgName}/{year}/{month}/{day}/PT1H.json
```

<span data-ttu-id="5bca4-124">Information om hello struktur hello loggen finns [Network Security Group flöda logg: översikt](network-watcher-nsg-flow-logging-overview.md)</span><span class="sxs-lookup"><span data-stu-id="5bca4-124">For information about hello structure of hello log visit [Network Security Group Flow log Overview](network-watcher-nsg-flow-logging-overview.md)</span></span>

## <a name="next-steps"></a><span data-ttu-id="5bca4-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5bca4-125">Next Steps</span></span>

<span data-ttu-id="5bca4-126">Lär dig hur för[visualisera dina NSG flödet loggar med PowerBI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span><span class="sxs-lookup"><span data-stu-id="5bca4-126">Learn how too[Visualize your NSG flow logs with PowerBI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span></span>

<span data-ttu-id="5bca4-127">Lär dig hur för[visualisera dina NSG flödet loggar med öppen källkod verktyg](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)</span><span class="sxs-lookup"><span data-stu-id="5bca4-127">Learn how too[Visualize your NSG flow logs with open source tools](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)</span></span>
