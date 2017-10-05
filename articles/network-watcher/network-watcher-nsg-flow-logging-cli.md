---
title: "Hantera Network Security Group flöda loggar med Nätverksbevakaren Azure - Azure CLI | Microsoft Docs"
description: "Den här sidan förklarar hur du hanterar Network Security Group flöda loggar i Azure Nätverksbevakaren med Azure CLI"
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
ms.openlocfilehash: d5a8aa0cd274132798a0d8484a950926761dae7f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="configuring-network-security-group-flow-logs-with-azure-cli"></a><span data-ttu-id="e9151-103">Konfigurera Network Security Group flöda loggar med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="e9151-103">Configuring Network Security Group Flow logs with Azure CLI</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="e9151-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e9151-104">Azure portal</span></span>](network-watcher-nsg-flow-logging-portal.md)
> - [<span data-ttu-id="e9151-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e9151-105">PowerShell</span></span>](network-watcher-nsg-flow-logging-powershell.md)
> - [<span data-ttu-id="e9151-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="e9151-106">CLI 1.0</span></span>](network-watcher-nsg-flow-logging-cli-nodejs.md)
> - [<span data-ttu-id="e9151-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="e9151-107">CLI 2.0</span></span>](network-watcher-nsg-flow-logging-cli.md)
> - [<span data-ttu-id="e9151-108">REST-API</span><span class="sxs-lookup"><span data-stu-id="e9151-108">REST API</span></span>](network-watcher-nsg-flow-logging-rest.md)

<span data-ttu-id="e9151-109">Nätverkssäkerhetsgruppen flöde loggarna är en funktion i Nätverksbevakaren där du kan visa information om ingående och utgående IP-trafik via en Nätverkssäkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="e9151-109">Network Security Group flow logs are a feature of Network Watcher that allows you to view information about ingress and egress IP traffic through a Network Security Group.</span></span> <span data-ttu-id="e9151-110">Loggarna flödet skrivs i json-format och visa utgående och inkommande flöden på grundval av per regel, NIC flödet gäller för 5-tuppel information om flödet (källan/målet IP-källan/målet Port Protocol), och om trafiken tillåts eller nekas.</span><span class="sxs-lookup"><span data-stu-id="e9151-110">These flow logs are written in json format and show outbound and inbound flows on a per rule basis, the NIC the flow applies to, 5-tuple information about the flow (Source/Destination IP, Source/Destination Port, Protocol), and if the traffic was allowed or denied.</span></span>

<span data-ttu-id="e9151-111">Den här artikeln använder våra nästa generations CLI för hantering av resursdistributionsmodell, Azure CLI 2.0, som är tillgänglig för Windows, Mac och Linux.</span><span class="sxs-lookup"><span data-stu-id="e9151-111">This article uses our next generation CLI for the resource management deployment model, Azure CLI 2.0, which is available for Windows, Mac and Linux.</span></span>

<span data-ttu-id="e9151-112">Om du vill utföra stegen i den här artikeln, måste du [installera Azure-kommandoradsgränssnittet för Mac, Linux och Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="e9151-112">To perform the steps in this article, you need to [install the Azure Command-Line Interface for Mac, Linux, and Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span>

## <a name="register-insights-provider"></a><span data-ttu-id="e9151-113">Registrera providern insikter</span><span class="sxs-lookup"><span data-stu-id="e9151-113">Register Insights provider</span></span>

<span data-ttu-id="e9151-114">För att flöda loggning för att fungera korrekt, den **Microsoft.Insights** provider måste vara registrerad.</span><span class="sxs-lookup"><span data-stu-id="e9151-114">In order for flow logging to work successfully, the **Microsoft.Insights** provider must be registered.</span></span> <span data-ttu-id="e9151-115">Om du inte är säker på om den **Microsoft.Insights** provider är registrerad, kör följande skript.</span><span class="sxs-lookup"><span data-stu-id="e9151-115">If you are not sure if the **Microsoft.Insights** provider is registered, run the following script.</span></span>

```azurecli
az provider register --namespace Microsoft.Insights
```

## <a name="enable-network-security-group-flow-logs"></a><span data-ttu-id="e9151-116">Aktivera Nätverkssäkerhetsgruppen flöde loggar</span><span class="sxs-lookup"><span data-stu-id="e9151-116">Enable Network Security Group Flow logs</span></span>

<span data-ttu-id="e9151-117">Kommandot för att aktivera flödet loggarna visas i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="e9151-117">The command to enable flow logs is shown in the following example:</span></span>

```azurecli
az network watcher flow-log configure --resource-group resourceGroupName --enabled true --nsg nsgName --storage-account storageAccountName
```

## <a name="disable-network-security-group-flow-logs"></a><span data-ttu-id="e9151-118">Inaktivera Nätverkssäkerhetsgruppen flöde loggar</span><span class="sxs-lookup"><span data-stu-id="e9151-118">Disable Network Security Group Flow logs</span></span>

<span data-ttu-id="e9151-119">Använd följande exempel för att inaktivera flödet loggar:</span><span class="sxs-lookup"><span data-stu-id="e9151-119">Use the following example to disable flow logs:</span></span>

```azurecli
az network watcher flow-log configure --resource-group resourceGroupName --enabled false --nsg nsgName
```

## <a name="download-a-flow-log"></a><span data-ttu-id="e9151-120">Hämta en logg flöde</span><span class="sxs-lookup"><span data-stu-id="e9151-120">Download a Flow log</span></span>

<span data-ttu-id="e9151-121">Lagringsplatsen för en flödet logg definieras på Skapa.</span><span class="sxs-lookup"><span data-stu-id="e9151-121">The storage location of a flow log is defined at creation.</span></span> <span data-ttu-id="e9151-122">Ett enkelt verktyg för att komma åt dessa flödet loggar som sparats i ett lagringskonto är Microsoft Azure Lagringsutforskaren, som kan hämtas här: http://storageexplorer.com/</span><span class="sxs-lookup"><span data-stu-id="e9151-122">A convenient tool to access these flow logs saved to a storage account is Microsoft Azure Storage Explorer, which can be downloaded here:  http://storageexplorer.com/</span></span>

<span data-ttu-id="e9151-123">Om ett storage-konto anges sparas paket avbilda filer till ett lagringskonto på följande plats:</span><span class="sxs-lookup"><span data-stu-id="e9151-123">If a storage account is specified, packet capture files are saved to a storage account at the following location:</span></span>

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId%3D/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/microsoft.network/networksecuritygroups/{nsgName}/{year}/{month}/{day}/PT1H.json
```

<span data-ttu-id="e9151-124">Information om strukturen i loggen finns [Network Security Group flöda logg: översikt](network-watcher-nsg-flow-logging-overview.md)</span><span class="sxs-lookup"><span data-stu-id="e9151-124">For information about the structure of the log visit [Network Security Group Flow log Overview](network-watcher-nsg-flow-logging-overview.md)</span></span>

## <a name="next-steps"></a><span data-ttu-id="e9151-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e9151-125">Next Steps</span></span>

<span data-ttu-id="e9151-126">Lär dig hur du [visualisera dina NSG flödet loggar med PowerBI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span><span class="sxs-lookup"><span data-stu-id="e9151-126">Learn how to [Visualize your NSG flow logs with PowerBI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span></span>

<span data-ttu-id="e9151-127">Lär dig hur du [visualisera dina NSG flödet loggar med öppen källkod verktyg](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)</span><span class="sxs-lookup"><span data-stu-id="e9151-127">Learn how to [Visualize your NSG flow logs with open source tools](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)</span></span>
