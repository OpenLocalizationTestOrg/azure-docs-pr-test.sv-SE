---
title: "aaaCreate en instans av Azure Nätverksbevakaren | Microsoft Docs"
description: "Den här sidan innehåller hello steg toocreate en instans av Nätverksbevakaren med hello-portalen och Azure REST API"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: b1314119-0b87-4f4d-b44c-2c4d0547fb76
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 90d4f90c9709a80e4b27863e79e5b6e16de145c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-network-watcher-instance"></a><span data-ttu-id="2ec79-103">Skapa en instans av Azure Nätverksbevakaren</span><span class="sxs-lookup"><span data-stu-id="2ec79-103">Create an Azure Network Watcher instance</span></span>

<span data-ttu-id="2ec79-104">Nätverksbevakaren är en regionala tjänst som gör att du toomonitor och diagnostisera villkor på nätverket scenariot nivå, till och från Azure.</span><span class="sxs-lookup"><span data-stu-id="2ec79-104">Network Watcher is a regional service that enables you toomonitor and diagnose conditions at a network scenario level in, to, and from Azure.</span></span> <span data-ttu-id="2ec79-105">Scenariot nivån övervakning kan du toodiagnose problem vid slutet tooend nätverk på vyn.</span><span class="sxs-lookup"><span data-stu-id="2ec79-105">Scenario level monitoring enables you toodiagnose problems at an end tooend network level view.</span></span> <span data-ttu-id="2ec79-106">Diagnostiska nätverks- och visualiseringsverktyg som finns tillgängliga med Nätverksbevakaren hjälpa dig att förstå, diagnostisera och få insikter tooyour nätverk i Azure.</span><span class="sxs-lookup"><span data-stu-id="2ec79-106">Network diagnostic and visualization tools available with Network Watcher help you understand, diagnose, and gain insights tooyour network in Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="2ec79-107">Nätverksbevakaren stöder för närvarande endast CLI 1.0, hello instruktioner toocreate en ny instans av Nätverksbevakaren har angetts för CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="2ec79-107">As Network Watcher currently only supports CLI 1.0, hello instructions toocreate a new Network Watcher instance is provided for CLI 1.0.</span></span>

## <a name="create-a-network-watcher-in-hello-portal"></a><span data-ttu-id="2ec79-108">Skapa en Nätverksbevakaren hello-portalen</span><span class="sxs-lookup"><span data-stu-id="2ec79-108">Create a Network Watcher in hello portal</span></span>

<span data-ttu-id="2ec79-109">Navigera för**fler tjänster** > **nätverk** > **Nätverksbevakaren**.</span><span class="sxs-lookup"><span data-stu-id="2ec79-109">Navigate too**More Services** > **Networking** > **Network Watcher**.</span></span> <span data-ttu-id="2ec79-110">Du kan välja alla hello prenumerationer som du vill tooenable Nätverksbevakaren för.</span><span class="sxs-lookup"><span data-stu-id="2ec79-110">You can select all hello subscriptions you want tooenable Network Watcher for.</span></span> <span data-ttu-id="2ec79-111">Den här åtgärden skapar en Nätverksbevakaren i varje region som är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="2ec79-111">This action creates a Network Watcher in every region that is available.</span></span>

![Skapa en nätverksbevakaren][1]

<span data-ttu-id="2ec79-113">När du aktiverar Nätverksbevakaren med hello Portal anges hello namnet på hello Nätverksbevakaren instans automatiskt tooNetworkWatcher_region_name där region_name motsvarar toohello Azure-Region där hello-instansen har aktiverats.</span><span class="sxs-lookup"><span data-stu-id="2ec79-113">When you enable Network Watcher using hello Portal, hello name of hello Network Watcher instance will automatically be set tooNetworkWatcher_region_name where region_name corresponds toohello Azure Region where hello instance was enabled.</span></span>  <span data-ttu-id="2ec79-114">Till exempel får en Nätverksbevakaren aktiverat i West centrala oss region namnet NetworkWatcher_westcentralus</span><span class="sxs-lookup"><span data-stu-id="2ec79-114">For example, a Network Watcher enabled in West Central US region will be named NetworkWatcher_westcentralus</span></span>

<span data-ttu-id="2ec79-115">Dessutom hello Nätverksbevakaren instans automatiskt till i en resursgrupp med namnet NetworkWatcherRG.</span><span class="sxs-lookup"><span data-stu-id="2ec79-115">Additionally, hello Network Watcher instance will automatically be added into a Resource Group called NetworkWatcherRG.</span></span>  <span data-ttu-id="2ec79-116">Den här resursgruppen ska skapas om den inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="2ec79-116">This Resource Group will be created if it does not already exist.</span></span>

<span data-ttu-id="2ec79-117">Om du inte vill toocustomize hello namnet på en instans av Nätverksbevakaren och hello resursgruppen har placerats i, kan du använda Powershell, hello REST API eller ARMClient metoder som beskrivs nedan.</span><span class="sxs-lookup"><span data-stu-id="2ec79-117">If you wish toocustomize hello name of a Network Watcher instance and hello Resource Group it's placed into, you can use Powershell, hello REST API, or ARMClient methods described below.</span></span>  <span data-ttu-id="2ec79-118">I varje alternativ, hello resursgruppens namn måste finnas innan du placerar hello Nätverksbevakaren till den.</span><span class="sxs-lookup"><span data-stu-id="2ec79-118">In each option, hello Resource Group must exist before you place hello Network Watcher into it.</span></span>  

## <a name="create-a-network-watcher-with-powershell"></a><span data-ttu-id="2ec79-119">Skapa en Nätverksbevakaren med PowerShell</span><span class="sxs-lookup"><span data-stu-id="2ec79-119">Create a Network Watcher with PowerShell</span></span>

<span data-ttu-id="2ec79-120">toocreate en instans av Nätverksbevakaren kör hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="2ec79-120">toocreate an instance of Network Watcher, run hello following example:</span></span>

```powershell
New-AzureRmNetworkWatcher -Name "NetworkWatcher_westcentralus" -ResourceGroupName "NetworkWatcherRG" -Location "West Central US"
```

## <a name="create-a-network-watcher-with-hello-rest-api"></a><span data-ttu-id="2ec79-121">Skapa en Nätverksbevakaren med hello REST API</span><span class="sxs-lookup"><span data-stu-id="2ec79-121">Create a Network Watcher with hello REST API</span></span>

<span data-ttu-id="2ec79-122">ARMclient är används toocall hello REST-API med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2ec79-122">ARMclient is used toocall hello REST API using PowerShell.</span></span> <span data-ttu-id="2ec79-123">ARMClient hittas på chocolatey på [ARMClient på Chocolatey](https://chocolatey.org/packages/ARMClient)</span><span class="sxs-lookup"><span data-stu-id="2ec79-123">ARMClient is found on chocolatey at [ARMClient on Chocolatey](https://chocolatey.org/packages/ARMClient)</span></span>

### <a name="log-in-with-armclient"></a><span data-ttu-id="2ec79-124">Logga in med ARMClient</span><span class="sxs-lookup"><span data-stu-id="2ec79-124">Log in with ARMClient</span></span>

```powerShell
armclient login
```

### <a name="create-hello-network-watcher"></a><span data-ttu-id="2ec79-125">Skapa hello nätverksbevakaren</span><span class="sxs-lookup"><span data-stu-id="2ec79-125">Create hello network watcher</span></span>

```powershell
$subscriptionId = '<subscription id>'
$networkWatcherName = '<name of network watcher>'
$resourceGroupName = '<resource group name>'
$apiversion = "2016-09-01"
$requestBody = @"
{
'location': 'West Central US'
}
"@

armclient put "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}?api-version=${api-version}" $requestBody
```

## <a name="next-steps"></a><span data-ttu-id="2ec79-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2ec79-126">Next steps</span></span>

<span data-ttu-id="2ec79-127">Nu när du har en instans av Nätverksbevakaren Lär dig mer om hello-funktioner som är tillgängliga:</span><span class="sxs-lookup"><span data-stu-id="2ec79-127">Now that you have an instance of Network Watcher, learn about hello features available:</span></span>

* [<span data-ttu-id="2ec79-128">Topologi</span><span class="sxs-lookup"><span data-stu-id="2ec79-128">Topology</span></span>](network-watcher-topology-overview.md)
* [<span data-ttu-id="2ec79-129">Paketinsamling</span><span class="sxs-lookup"><span data-stu-id="2ec79-129">Packet capture</span></span>](network-watcher-packet-capture-overview.md)
* [<span data-ttu-id="2ec79-130">Verifiera IP-flöde</span><span class="sxs-lookup"><span data-stu-id="2ec79-130">IP flow verify</span></span>](network-watcher-ip-flow-verify-overview.md)
* [<span data-ttu-id="2ec79-131">Nästa hopp</span><span class="sxs-lookup"><span data-stu-id="2ec79-131">Next hop</span></span>](network-watcher-next-hop-overview.md)
* [<span data-ttu-id="2ec79-132">Säkerhetsgruppvy</span><span class="sxs-lookup"><span data-stu-id="2ec79-132">Security group view</span></span>](network-watcher-security-group-view-overview.md)
* [<span data-ttu-id="2ec79-133">NSG flödet loggning</span><span class="sxs-lookup"><span data-stu-id="2ec79-133">NSG flow logging</span></span>](network-watcher-nsg-flow-logging-overview.md)
* [<span data-ttu-id="2ec79-134">Felsökning av virtuella nätverksgateway</span><span class="sxs-lookup"><span data-stu-id="2ec79-134">Virtual Network Gateway troubleshooting</span></span>](network-watcher-troubleshoot-overview.md)

<span data-ttu-id="2ec79-135">När en instans av Nätverksbevakaren har skapats, paket avbilda kan konfigureras med följande hello-artikel: [skapar en avisering utlösta paketinsamling](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="2ec79-135">Once a Network Watcher instance has been created, package capture can be configured by following hello article: [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>

[1]: ./media/network-watcher-create/figure1.png











