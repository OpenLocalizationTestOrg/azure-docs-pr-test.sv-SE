---
title: "Sök efter nästa hopp med Azure Network Watcher nexthop - Azure CLI 2.0 | Microsoft Docs"
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
ms.openlocfilehash: d1ee6870ba0188ff2c473e4cca12a5bdc1f97d3d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="find-out-what-the-next-hop-type-is-using-the-next-hop-capability-in-azure-network-watcher-using-azure-cli-20"></a><span data-ttu-id="84859-103">Ta reda på vilka nästa hopptyp är med nästa hopp-funktionen i Azure Nätverksbevakaren använder Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="84859-103">Find out what the next hop type is using the Next Hop capability in Azure Network Watcher using Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="84859-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="84859-104">Azure portal</span></span>](network-watcher-check-next-hop-portal.md)
> - [<span data-ttu-id="84859-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="84859-105">PowerShell</span></span>](network-watcher-check-next-hop-powershell.md)
> - [<span data-ttu-id="84859-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="84859-106">CLI 1.0</span></span>](network-watcher-check-next-hop-cli-nodejs.md)
> - [<span data-ttu-id="84859-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="84859-107">CLI 2.0</span></span>](network-watcher-check-next-hop-cli.md)
> - [<span data-ttu-id="84859-108">Azure REST-API</span><span class="sxs-lookup"><span data-stu-id="84859-108">Azure REST API</span></span>](network-watcher-check-next-hop-rest.md)

<span data-ttu-id="84859-109">Nästa hopp är en funktion i Nätverksbevakaren som tillhandahåller möjligheten get nästa hopptyp och IP-adress baserat på en angiven virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="84859-109">Next hop is a feature of Network Watcher that provides the ability get the next hop type and IP address based on a specified virtual machine.</span></span> <span data-ttu-id="84859-110">Den här funktionen är användbart för att fastställa om trafik som lämnar en virtuell dator som passerar en gateway, internet eller virtuella nätverk för att komma till sin destination.</span><span class="sxs-lookup"><span data-stu-id="84859-110">This feature is useful in determining if traffic leaving a virtual machine traverses a gateway, internet, or virtual networks to get to its destination.</span></span>

<span data-ttu-id="84859-111">Den här artikeln använder våra nästa generations CLI för hantering av resursdistributionsmodell, Azure CLI 2.0, som är tillgänglig för Windows, Mac och Linux.</span><span class="sxs-lookup"><span data-stu-id="84859-111">This article uses our next generation CLI for the resource management deployment model, Azure CLI 2.0, which is available for Windows, Mac and Linux.</span></span>

<span data-ttu-id="84859-112">Om du vill utföra stegen i den här artikeln, måste du [installera Azure-kommandoradsgränssnittet för Mac, Linux och Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="84859-112">To perform the steps in this article, you need to [install the Azure Command-Line Interface for Mac, Linux, and Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="84859-113">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="84859-113">Before you begin</span></span>

<span data-ttu-id="84859-114">I detta scenario använder du Azure CLI för att hitta nästa hopptyp och IP-adress.</span><span class="sxs-lookup"><span data-stu-id="84859-114">In this scenario, you will use the Azure CLI to find the next hop type and IP address.</span></span>

<span data-ttu-id="84859-115">Det här scenariot förutsätter att du redan har följt stegen i [skapa en Nätverksbevakaren](network-watcher-create.md) att skapa en Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="84859-115">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher.</span></span> <span data-ttu-id="84859-116">Det här scenariot förutsätter att det finns en resursgrupp med en giltig virtuell dator som ska användas.</span><span class="sxs-lookup"><span data-stu-id="84859-116">The scenario also assumes that a Resource Group with a valid virtual machine exists to be used.</span></span>

## <a name="scenario"></a><span data-ttu-id="84859-117">Scenario</span><span class="sxs-lookup"><span data-stu-id="84859-117">Scenario</span></span>

<span data-ttu-id="84859-118">Det scenario som beskrivs i den här artikeln använder nästa hopp, en funktion i Nätverksbevakaren som söker efter nästa hopptyp och IP-adress för en resurs.</span><span class="sxs-lookup"><span data-stu-id="84859-118">The scenario covered in this article uses Next Hop, a feature of Network Watcher that finds out the next hop type and IP address for a resource.</span></span> <span data-ttu-id="84859-119">Läs mer om nästa hopp [nästa hopp översikt](network-watcher-next-hop-overview.md).</span><span class="sxs-lookup"><span data-stu-id="84859-119">To learn more about Next Hop, visit [Next Hop Overview](network-watcher-next-hop-overview.md).</span></span>


## <a name="get-next-hop"></a><span data-ttu-id="84859-120">Hämta nästa hopp</span><span class="sxs-lookup"><span data-stu-id="84859-120">Get Next Hop</span></span>

<span data-ttu-id="84859-121">Att hämta nästa hopp som vi kallar det `az network watcher show-next-hop` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="84859-121">To get the next hop we call the `az network watcher show-next-hop` cmdlet.</span></span> <span data-ttu-id="84859-122">Vi skickar cmdlet resursgruppen Nätverksbevakaren, NetworkWatcher, virtuella Id, källans IP-adress och mål-IP-adress.</span><span class="sxs-lookup"><span data-stu-id="84859-122">We pass the cmdlet the Network Watcher resource group, the NetworkWatcher, virtual machine Id, source IP address, and destination IP address.</span></span> <span data-ttu-id="84859-123">I det här exemplet är den IP-adressen till en virtuell dator i ett annat virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="84859-123">In this example, the destination IP address is to a VM in another virtual network.</span></span> <span data-ttu-id="84859-124">Det finns en virtuell nätverksgateway mellan de två virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="84859-124">There is a virtual network gateway between the two virtual networks.</span></span>

<span data-ttu-id="84859-125">Om du inte har gjort det ännu, installerar och konfigurerar senast [Azure CLI 2.0](/cli/azure/install-az-cli2) och logga in till en Azure med hjälp av [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="84859-125">If you haven't yet, install and configure the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and log in to an Azure account using [az login](/cli/azure/#login).</span></span> <span data-ttu-id="84859-126">Kör följande kommando:</span><span class="sxs-lookup"><span data-stu-id="84859-126">Then run the following command:</span></span>

```azurecli
az network watcher show-next-hop --resource-group <resourcegroupName> --vm <vmNameorID> --source-ip <source-ip> --dest-ip <destination-ip>

```

> [!NOTE]
<span data-ttu-id="84859-127">Om den virtuella datorn har flera nätverkskort och IP-vidarebefordring är aktiverat på något av nätverkskorten och NIC-parameter (-i nic-id) måste anges.</span><span class="sxs-lookup"><span data-stu-id="84859-127">If the VM has multiple NICs and IP forwarding is enabled on any of the NICs, then the NIC parameter (-i nic-id) must be specified.</span></span> <span data-ttu-id="84859-128">Annars är valfria.</span><span class="sxs-lookup"><span data-stu-id="84859-128">Otherwise it is optional.</span></span>

## <a name="review-results"></a><span data-ttu-id="84859-129">Granska resultaten</span><span class="sxs-lookup"><span data-stu-id="84859-129">Review results</span></span>

<span data-ttu-id="84859-130">När du är färdig tillhandahålls resultaten.</span><span class="sxs-lookup"><span data-stu-id="84859-130">When complete, the results are provided.</span></span> <span data-ttu-id="84859-131">Nästa hopp IP-adress returneras samt typ av resurs som det är.</span><span class="sxs-lookup"><span data-stu-id="84859-131">The next hop IP address is returned as well as the type of resource it is.</span></span>

```azurecli
{
    "nextHopIpAddress": null,
    "nextHopType": "Internet",
    "routeTableId": "System Route"
}
```

<span data-ttu-id="84859-132">I följande lista visas de tillgängliga värdena för NextHopType:</span><span class="sxs-lookup"><span data-stu-id="84859-132">The following list shows the currently available NextHopType values:</span></span>

<span data-ttu-id="84859-133">**Nästa Hopptyp**</span><span class="sxs-lookup"><span data-stu-id="84859-133">**Next Hop Type**</span></span>

* <span data-ttu-id="84859-134">Internet</span><span class="sxs-lookup"><span data-stu-id="84859-134">Internet</span></span>
* <span data-ttu-id="84859-135">VirtualAppliance</span><span class="sxs-lookup"><span data-stu-id="84859-135">VirtualAppliance</span></span>
* <span data-ttu-id="84859-136">VirtualNetworkGateway</span><span class="sxs-lookup"><span data-stu-id="84859-136">VirtualNetworkGateway</span></span>
* <span data-ttu-id="84859-137">VnetLocal</span><span class="sxs-lookup"><span data-stu-id="84859-137">VnetLocal</span></span>
* <span data-ttu-id="84859-138">HyperNetGateway</span><span class="sxs-lookup"><span data-stu-id="84859-138">HyperNetGateway</span></span>
* <span data-ttu-id="84859-139">VnetPeering</span><span class="sxs-lookup"><span data-stu-id="84859-139">VnetPeering</span></span>
* <span data-ttu-id="84859-140">Ingen</span><span class="sxs-lookup"><span data-stu-id="84859-140">None</span></span>

## <a name="next-steps"></a><span data-ttu-id="84859-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="84859-141">Next steps</span></span>

<span data-ttu-id="84859-142">Lär dig hur du granskar din grupp för nätverkssäkerhet via programmering genom att besöka [NSG granskning med Nätverksbevakaren](network-watcher-nsg-auditing-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="84859-142">Learn how to review your network security group settings programmatically by visiting [NSG Auditing with Network Watcher](network-watcher-nsg-auditing-powershell.md)</span></span>
