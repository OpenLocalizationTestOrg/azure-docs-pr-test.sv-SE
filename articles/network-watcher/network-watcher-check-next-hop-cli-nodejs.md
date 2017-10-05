---
title: "Sök efter nästa hopp med Azure Network Watcher nexthop - Azure CLI 1.0 | Microsoft Docs"
description: "Den här artikeln beskriver hur du kan hitta nästa hopptyp är och ip-adressen med nästa hopp med Azure CLI."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 0700c274-3e0d-4dca-acfa-3ceac8990613
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: ff88e945060ae033717ceb29db1352e112f05a3f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="find-out-what-the-next-hop-type-is-using-the-next-hop-capability-in-azure-network-watcher-using-azure-cli-10"></a><span data-ttu-id="b87e8-103">Ta reda på vilka nästa hopptyp är med nästa hopp-funktionen i Azure Nätverksbevakaren använder Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="b87e8-103">Find out what the next hop type is using the Next Hop capability in Azure Network Watcher using Azure CLI 1.0</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="b87e8-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b87e8-104">Azure portal</span></span>](network-watcher-check-next-hop-portal.md)
> - [<span data-ttu-id="b87e8-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b87e8-105">PowerShell</span></span>](network-watcher-check-next-hop-powershell.md)
> - [<span data-ttu-id="b87e8-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="b87e8-106">CLI 1.0</span></span>](network-watcher-check-next-hop-cli-nodejs.md)
> - [<span data-ttu-id="b87e8-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="b87e8-107">CLI 2.0</span></span>](network-watcher-check-next-hop-cli.md)
> - [<span data-ttu-id="b87e8-108">Azure REST-API</span><span class="sxs-lookup"><span data-stu-id="b87e8-108">Azure REST API</span></span>](network-watcher-check-next-hop-rest.md)

<span data-ttu-id="b87e8-109">Nästa hopp är en funktion i Nätverksbevakaren som tillhandahåller möjligheten get nästa hopptyp och IP-adress baserat på en angiven virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="b87e8-109">Next hop is a feature of Network Watcher that provides the ability get the next hop type and IP address based on a specified virtual machine.</span></span> <span data-ttu-id="b87e8-110">Den här funktionen är användbart för att fastställa om trafik som lämnar en virtuell dator som passerar en gateway, internet eller virtuella nätverk för att komma till sin destination.</span><span class="sxs-lookup"><span data-stu-id="b87e8-110">This feature is useful in determining if traffic leaving a virtual machine traverses a gateway, internet, or virtual networks to get to its destination.</span></span>

<span data-ttu-id="b87e8-111">Den här artikeln använder plattformsoberoende Azure CLI version 1.0, som är tillgänglig för Windows, Mac och Linux.</span><span class="sxs-lookup"><span data-stu-id="b87e8-111">This article uses cross-platform Azure CLI 1.0, which is available for Windows, Mac and Linux.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="b87e8-112">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="b87e8-112">Before you begin</span></span>

<span data-ttu-id="b87e8-113">I detta scenario använder du Azure CLI för att hitta nästa hopptyp och IP-adress.</span><span class="sxs-lookup"><span data-stu-id="b87e8-113">In this scenario, you will use the Azure CLI to find the next hop type and IP address.</span></span>

<span data-ttu-id="b87e8-114">Det här scenariot förutsätter att du redan har följt stegen i [skapa en Nätverksbevakaren](network-watcher-create.md) att skapa en Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="b87e8-114">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher.</span></span> <span data-ttu-id="b87e8-115">Det här scenariot förutsätter att det finns en resursgrupp med en giltig virtuell dator som ska användas.</span><span class="sxs-lookup"><span data-stu-id="b87e8-115">The scenario also assumes that a Resource Group with a valid virtual machine exists to be used.</span></span>

## <a name="scenario"></a><span data-ttu-id="b87e8-116">Scenario</span><span class="sxs-lookup"><span data-stu-id="b87e8-116">Scenario</span></span>

<span data-ttu-id="b87e8-117">Det scenario som beskrivs i den här artikeln använder nästa hopp, en funktion i Nätverksbevakaren som söker efter nästa hopptyp och IP-adress för en resurs.</span><span class="sxs-lookup"><span data-stu-id="b87e8-117">The scenario covered in this article uses Next Hop, a feature of Network Watcher that finds out the next hop type and IP address for a resource.</span></span> <span data-ttu-id="b87e8-118">Läs mer om nästa hopp [nästa hopp översikt](network-watcher-next-hop-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b87e8-118">To learn more about Next Hop, visit [Next Hop Overview](network-watcher-next-hop-overview.md).</span></span>


## <a name="get-next-hop"></a><span data-ttu-id="b87e8-119">Hämta nästa hopp</span><span class="sxs-lookup"><span data-stu-id="b87e8-119">Get Next Hop</span></span>

<span data-ttu-id="b87e8-120">Att hämta nästa hopp som vi kallar det `azure netowrk watcher next-hop` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="b87e8-120">To get the next hop we call the `azure netowrk watcher next-hop` cmdlet.</span></span> <span data-ttu-id="b87e8-121">Vi skickar cmdlet resursgruppen Nätverksbevakaren, NetworkWatcher, virtuella Id, källans IP-adress och mål-IP-adress.</span><span class="sxs-lookup"><span data-stu-id="b87e8-121">We pass the cmdlet the Network Watcher resource group, the NetworkWatcher, virtual machine Id, source IP address, and destination IP address.</span></span> <span data-ttu-id="b87e8-122">I det här exemplet är den IP-adressen till en virtuell dator i ett annat virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="b87e8-122">In this example, the destination IP address is to a VM in another virtual network.</span></span> <span data-ttu-id="b87e8-123">Det finns en virtuell nätverksgateway mellan de två virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="b87e8-123">There is a virtual network gateway between the two virtual networks.</span></span> 

```azurecli
azure network watcher next-hop -g resourceGroupName -n networkWatcherName -t targetResourceId -a <source-ip> -d <destination-ip>
```

> [!NOTE]
<span data-ttu-id="b87e8-124">Om den virtuella datorn har flera nätverkskort och IP-vidarebefordring är aktiverat på något av nätverkskorten och NIC-parameter (-i nic-id) måste anges.</span><span class="sxs-lookup"><span data-stu-id="b87e8-124">If the VM has multiple NICs and IP forwarding is enabled on any of the NICs, then the NIC parameter (-i nic-id) must be specified.</span></span> <span data-ttu-id="b87e8-125">Annars är valfria.</span><span class="sxs-lookup"><span data-stu-id="b87e8-125">Otherwise it is optional.</span></span>

## <a name="review-results"></a><span data-ttu-id="b87e8-126">Granska resultaten</span><span class="sxs-lookup"><span data-stu-id="b87e8-126">Review results</span></span>

<span data-ttu-id="b87e8-127">När du är färdig tillhandahålls resultaten.</span><span class="sxs-lookup"><span data-stu-id="b87e8-127">When complete, the results are provided.</span></span> <span data-ttu-id="b87e8-128">Nästa hopp IP-adress returneras samt typ av resurs som det är.</span><span class="sxs-lookup"><span data-stu-id="b87e8-128">The next hop IP address is returned as well as the type of resource it is.</span></span>

```
data:    Next Hop Ip Address             : 10.0.1.2
info:    network watcher next-hop command OK
```

<span data-ttu-id="b87e8-129">I följande lista visas de tillgängliga värdena för NextHopType:</span><span class="sxs-lookup"><span data-stu-id="b87e8-129">The following list shows the currently available NextHopType values:</span></span>

<span data-ttu-id="b87e8-130">**Nästa Hopptyp**</span><span class="sxs-lookup"><span data-stu-id="b87e8-130">**Next Hop Type**</span></span>

* <span data-ttu-id="b87e8-131">Internet</span><span class="sxs-lookup"><span data-stu-id="b87e8-131">Internet</span></span>
* <span data-ttu-id="b87e8-132">VirtualAppliance</span><span class="sxs-lookup"><span data-stu-id="b87e8-132">VirtualAppliance</span></span>
* <span data-ttu-id="b87e8-133">VirtualNetworkGateway</span><span class="sxs-lookup"><span data-stu-id="b87e8-133">VirtualNetworkGateway</span></span>
* <span data-ttu-id="b87e8-134">VnetLocal</span><span class="sxs-lookup"><span data-stu-id="b87e8-134">VnetLocal</span></span>
* <span data-ttu-id="b87e8-135">HyperNetGateway</span><span class="sxs-lookup"><span data-stu-id="b87e8-135">HyperNetGateway</span></span>
* <span data-ttu-id="b87e8-136">VnetPeering</span><span class="sxs-lookup"><span data-stu-id="b87e8-136">VnetPeering</span></span>
* <span data-ttu-id="b87e8-137">Ingen</span><span class="sxs-lookup"><span data-stu-id="b87e8-137">None</span></span>

## <a name="next-steps"></a><span data-ttu-id="b87e8-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b87e8-138">Next steps</span></span>

<span data-ttu-id="b87e8-139">Lär dig hur du granskar din grupp för nätverkssäkerhet via programmering genom att besöka [NSG granskning med Nätverksbevakaren](network-watcher-nsg-auditing-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="b87e8-139">Learn how to review your network security group settings programmatically by visiting [NSG Auditing with Network Watcher](network-watcher-nsg-auditing-powershell.md)</span></span>
