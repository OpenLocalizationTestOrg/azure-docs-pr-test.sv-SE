---
title: "Skapa en instans av Azure Nätverksbevakaren | Microsoft Docs"
description: "Den här sidan innehåller stegen för att skapa en instans av Nätverksbevakaren med hjälp av portalen och Azure REST API"
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
ms.openlocfilehash: 2aeaffdd5ab552e18677cbd1a24a748dd14bf172
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-network-watcher-instance"></a><span data-ttu-id="48beb-103">Skapa en instans av Azure Nätverksbevakaren</span><span class="sxs-lookup"><span data-stu-id="48beb-103">Create an Azure Network Watcher instance</span></span>

<span data-ttu-id="48beb-104">Nätverksbevakaren är en regionala tjänst som gör att du kan övervaka och diagnostisera villkor på nätverket scenariot nivå, till och från Azure.</span><span class="sxs-lookup"><span data-stu-id="48beb-104">Network Watcher is a regional service that enables you to monitor and diagnose conditions at a network scenario level in, to, and from Azure.</span></span> <span data-ttu-id="48beb-105">Scenariot nivån övervakning kan du diagnostisera problem på vyn för slutpunkt till slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="48beb-105">Scenario level monitoring enables you to diagnose problems at an end to end network level view.</span></span> <span data-ttu-id="48beb-106">Diagnostiska nätverks- och visualiseringsverktyg som finns tillgängliga med Nätverksbevakaren hjälpa dig att förstå, diagnostisera och få insyn i nätverket i Azure.</span><span class="sxs-lookup"><span data-stu-id="48beb-106">Network diagnostic and visualization tools available with Network Watcher help you understand, diagnose, and gain insights to your network in Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="48beb-107">Nätverksbevakaren stöder för närvarande endast CLI 1.0, ges instruktionerna för att skapa en ny instans av Nätverksbevakaren för CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="48beb-107">As Network Watcher currently only supports CLI 1.0, the instructions to create a new Network Watcher instance is provided for CLI 1.0.</span></span>

## <a name="create-a-network-watcher-in-the-portal"></a><span data-ttu-id="48beb-108">Skapa en Nätverksbevakaren i portalen</span><span class="sxs-lookup"><span data-stu-id="48beb-108">Create a Network Watcher in the portal</span></span>

<span data-ttu-id="48beb-109">Gå till **fler tjänster** > **nätverk** > **nätverk Watcher**.</span><span class="sxs-lookup"><span data-stu-id="48beb-109">Navigate to **More Services** > **Networking** > **Network Watcher**.</span></span> <span data-ttu-id="48beb-110">Du kan välja alla prenumerationer som du vill aktivera Nätverksbevakaren för.</span><span class="sxs-lookup"><span data-stu-id="48beb-110">You can select all the subscriptions you want to enable Network Watcher for.</span></span> <span data-ttu-id="48beb-111">Den här åtgärden skapar en Nätverksbevakaren i varje region som är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="48beb-111">This action creates a Network Watcher in every region that is available.</span></span>

![Skapa en nätverksbevakaren][1]

<span data-ttu-id="48beb-113">När du aktiverar Nätverksbevakaren med hjälp av portalen anges namnet på instansen Nätverksbevakaren automatiskt till NetworkWatcher_region_name där region_name motsvarar den Azure-Region där instansen har aktiverats.</span><span class="sxs-lookup"><span data-stu-id="48beb-113">When you enable Network Watcher using the Portal, the name of the Network Watcher instance will automatically be set to NetworkWatcher_region_name where region_name corresponds to the Azure Region where the instance was enabled.</span></span>  <span data-ttu-id="48beb-114">Till exempel får en Nätverksbevakaren aktiverat i West centrala oss region namnet NetworkWatcher_westcentralus</span><span class="sxs-lookup"><span data-stu-id="48beb-114">For example, a Network Watcher enabled in West Central US region will be named NetworkWatcher_westcentralus</span></span>

<span data-ttu-id="48beb-115">Nätverksbevakaren-instansen kommer dessutom automatiskt läggas till i en resursgrupp med namnet NetworkWatcherRG.</span><span class="sxs-lookup"><span data-stu-id="48beb-115">Additionally, the Network Watcher instance will automatically be added into a Resource Group called NetworkWatcherRG.</span></span>  <span data-ttu-id="48beb-116">Den här resursgruppen ska skapas om den inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="48beb-116">This Resource Group will be created if it does not already exist.</span></span>

<span data-ttu-id="48beb-117">Om du vill anpassa namnet på en Nätverksbevakaren-instans och resursgruppen har placerats i, kan du använda Powershell, REST-API eller ARMClient metoder som beskrivs nedan.</span><span class="sxs-lookup"><span data-stu-id="48beb-117">If you wish to customize the name of a Network Watcher instance and the Resource Group it's placed into, you can use Powershell, the REST API, or ARMClient methods described below.</span></span>  <span data-ttu-id="48beb-118">I varje alternativ, måste resursgruppens namn finnas innan du placerar Nätverksbevakaren i den.</span><span class="sxs-lookup"><span data-stu-id="48beb-118">In each option, the Resource Group must exist before you place the Network Watcher into it.</span></span>  

## <a name="create-a-network-watcher-with-powershell"></a><span data-ttu-id="48beb-119">Skapa en Nätverksbevakaren med PowerShell</span><span class="sxs-lookup"><span data-stu-id="48beb-119">Create a Network Watcher with PowerShell</span></span>

<span data-ttu-id="48beb-120">Om du vill skapa en instans av Nätverksbevakaren, kör du följande exempel:</span><span class="sxs-lookup"><span data-stu-id="48beb-120">To create an instance of Network Watcher, run the following example:</span></span>

```powershell
New-AzureRmNetworkWatcher -Name "NetworkWatcher_westcentralus" -ResourceGroupName "NetworkWatcherRG" -Location "West Central US"
```

## <a name="create-a-network-watcher-with-the-rest-api"></a><span data-ttu-id="48beb-121">Skapa en Nätverksbevakaren med REST API</span><span class="sxs-lookup"><span data-stu-id="48beb-121">Create a Network Watcher with the REST API</span></span>

<span data-ttu-id="48beb-122">ARMclient används för att anropa REST-API med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="48beb-122">ARMclient is used to call the REST API using PowerShell.</span></span> <span data-ttu-id="48beb-123">ARMClient hittas på chocolatey på [ARMClient på Chocolatey](https://chocolatey.org/packages/ARMClient)</span><span class="sxs-lookup"><span data-stu-id="48beb-123">ARMClient is found on chocolatey at [ARMClient on Chocolatey](https://chocolatey.org/packages/ARMClient)</span></span>

### <a name="log-in-with-armclient"></a><span data-ttu-id="48beb-124">Logga in med ARMClient</span><span class="sxs-lookup"><span data-stu-id="48beb-124">Log in with ARMClient</span></span>

```powerShell
armclient login
```

### <a name="create-the-network-watcher"></a><span data-ttu-id="48beb-125">Skapa nätverksbevakaren</span><span class="sxs-lookup"><span data-stu-id="48beb-125">Create the network watcher</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="48beb-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="48beb-126">Next steps</span></span>

<span data-ttu-id="48beb-127">Nu när du har en instans av Nätverksbevakaren Lär dig mer om funktionerna:</span><span class="sxs-lookup"><span data-stu-id="48beb-127">Now that you have an instance of Network Watcher, learn about the features available:</span></span>

* [<span data-ttu-id="48beb-128">Topologi</span><span class="sxs-lookup"><span data-stu-id="48beb-128">Topology</span></span>](network-watcher-topology-overview.md)
* [<span data-ttu-id="48beb-129">Paketinsamling</span><span class="sxs-lookup"><span data-stu-id="48beb-129">Packet capture</span></span>](network-watcher-packet-capture-overview.md)
* [<span data-ttu-id="48beb-130">Verifiera IP-flöde</span><span class="sxs-lookup"><span data-stu-id="48beb-130">IP flow verify</span></span>](network-watcher-ip-flow-verify-overview.md)
* [<span data-ttu-id="48beb-131">Nästa hopp</span><span class="sxs-lookup"><span data-stu-id="48beb-131">Next hop</span></span>](network-watcher-next-hop-overview.md)
* [<span data-ttu-id="48beb-132">Säkerhetsgruppvy</span><span class="sxs-lookup"><span data-stu-id="48beb-132">Security group view</span></span>](network-watcher-security-group-view-overview.md)
* [<span data-ttu-id="48beb-133">NSG flödet loggning</span><span class="sxs-lookup"><span data-stu-id="48beb-133">NSG flow logging</span></span>](network-watcher-nsg-flow-logging-overview.md)
* [<span data-ttu-id="48beb-134">Felsökning av virtuella nätverksgateway</span><span class="sxs-lookup"><span data-stu-id="48beb-134">Virtual Network Gateway troubleshooting</span></span>](network-watcher-troubleshoot-overview.md)

<span data-ttu-id="48beb-135">När en instans av Nätverksbevakaren har skapats, paket avbilda kan konfigureras genom att följa artikeln: [skapar en avisering utlösta paketinsamling](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="48beb-135">Once a Network Watcher instance has been created, package capture can be configured by following the article: [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>

[1]: ./media/network-watcher-create/figure1.png











