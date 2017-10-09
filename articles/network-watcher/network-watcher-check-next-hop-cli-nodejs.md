---
title: "aaaFind nästa hopp med Azure Network Watcher nexthop - Azure CLI 1.0 | Microsoft Docs"
description: "Den här artikeln beskriver hur du kan hitta vad hello nästa hopptyp är och IP-adress med hjälp av nästa hopp med Azure CLI."
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
ms.openlocfilehash: 54124c051021413695d70ba93c370605abc6ebbe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="find-out-what-hello-next-hop-type-is-using-hello-next-hop-capability-in-azure-network-watcher-using-azure-cli-10"></a><span data-ttu-id="8f570-103">Ta reda på vilka hello nästa hopptyp med hello nexthop-funktionen i Azure Nätverksbevakaren använder Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="8f570-103">Find out what hello next hop type is using hello Next Hop capability in Azure Network Watcher using Azure CLI 1.0</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="8f570-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="8f570-104">Azure portal</span></span>](network-watcher-check-next-hop-portal.md)
> - [<span data-ttu-id="8f570-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="8f570-105">PowerShell</span></span>](network-watcher-check-next-hop-powershell.md)
> - [<span data-ttu-id="8f570-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="8f570-106">CLI 1.0</span></span>](network-watcher-check-next-hop-cli-nodejs.md)
> - [<span data-ttu-id="8f570-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="8f570-107">CLI 2.0</span></span>](network-watcher-check-next-hop-cli.md)
> - [<span data-ttu-id="8f570-108">Azure REST-API</span><span class="sxs-lookup"><span data-stu-id="8f570-108">Azure REST API</span></span>](network-watcher-check-next-hop-rest.md)

<span data-ttu-id="8f570-109">Nästa hopp är en funktion i Nätverksbevakaren som ger hello möjlighet hämta hello nästa hopptyp och IP-adress baserat på en angiven virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="8f570-109">Next hop is a feature of Network Watcher that provides hello ability get hello next hop type and IP address based on a specified virtual machine.</span></span> <span data-ttu-id="8f570-110">Den här funktionen är användbart för att fastställa om trafik som lämnar en virtuell dator som passerar en gateway, internet eller virtuella nätverk tooget tooits mål.</span><span class="sxs-lookup"><span data-stu-id="8f570-110">This feature is useful in determining if traffic leaving a virtual machine traverses a gateway, internet, or virtual networks tooget tooits destination.</span></span>

<span data-ttu-id="8f570-111">Den här artikeln använder plattformsoberoende Azure CLI version 1.0, som är tillgänglig för Windows, Mac och Linux.</span><span class="sxs-lookup"><span data-stu-id="8f570-111">This article uses cross-platform Azure CLI 1.0, which is available for Windows, Mac and Linux.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="8f570-112">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="8f570-112">Before you begin</span></span>

<span data-ttu-id="8f570-113">I det här scenariot använder du hello Azure CLI toofind hello nästa hopptyp och IP-adress.</span><span class="sxs-lookup"><span data-stu-id="8f570-113">In this scenario, you will use hello Azure CLI toofind hello next hop type and IP address.</span></span>

<span data-ttu-id="8f570-114">Det här scenariot förutsätter att du redan har följt stegen hello i [skapa en Nätverksbevakaren](network-watcher-create.md) toocreate en Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="8f570-114">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span> <span data-ttu-id="8f570-115">hello scenariot förutsätter att en resursgrupp med en giltig virtuell dator finns toobe används.</span><span class="sxs-lookup"><span data-stu-id="8f570-115">hello scenario also assumes that a Resource Group with a valid virtual machine exists toobe used.</span></span>

## <a name="scenario"></a><span data-ttu-id="8f570-116">Scenario</span><span class="sxs-lookup"><span data-stu-id="8f570-116">Scenario</span></span>

<span data-ttu-id="8f570-117">hello-scenario som beskrivs i den här artikeln används nästa hopp, en funktion i Nätverksbevakaren som söker efter hello nästa hopptyp och IP-adress för en resurs.</span><span class="sxs-lookup"><span data-stu-id="8f570-117">hello scenario covered in this article uses Next Hop, a feature of Network Watcher that finds out hello next hop type and IP address for a resource.</span></span> <span data-ttu-id="8f570-118">Besök toolearn mer om nästa hopp [nästa hopp översikt](network-watcher-next-hop-overview.md).</span><span class="sxs-lookup"><span data-stu-id="8f570-118">toolearn more about Next Hop, visit [Next Hop Overview](network-watcher-next-hop-overview.md).</span></span>


## <a name="get-next-hop"></a><span data-ttu-id="8f570-119">Hämta nästa hopp</span><span class="sxs-lookup"><span data-stu-id="8f570-119">Get Next Hop</span></span>

<span data-ttu-id="8f570-120">tooget hello nästa hopp vi kallar hello `azure netowrk watcher next-hop` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="8f570-120">tooget hello next hop we call hello `azure netowrk watcher next-hop` cmdlet.</span></span> <span data-ttu-id="8f570-121">Vi skickar hello cmdlet hello Nätverksbevakaren resursgrupp, hello NetworkWatcher, virtuella Id, källans IP-adress och mål-IP-adress.</span><span class="sxs-lookup"><span data-stu-id="8f570-121">We pass hello cmdlet hello Network Watcher resource group, hello NetworkWatcher, virtual machine Id, source IP address, and destination IP address.</span></span> <span data-ttu-id="8f570-122">I det här exemplet är IP-måladress hello tooa VM i ett annat virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="8f570-122">In this example, hello destination IP address is tooa VM in another virtual network.</span></span> <span data-ttu-id="8f570-123">Det finns en virtuell nätverksgateway mellan hello två virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="8f570-123">There is a virtual network gateway between hello two virtual networks.</span></span> 

```azurecli
azure network watcher next-hop -g resourceGroupName -n networkWatcherName -t targetResourceId -a <source-ip> -d <destination-ip>
```

> [!NOTE]
<span data-ttu-id="8f570-124">Om hello VM har flera nätverkskort och IP-vidarebefordring är aktiverad på någon av hello nätverkskort, sedan hello NIC-parameter (-i nic-id) måste anges.</span><span class="sxs-lookup"><span data-stu-id="8f570-124">If hello VM has multiple NICs and IP forwarding is enabled on any of hello NICs, then hello NIC parameter (-i nic-id) must be specified.</span></span> <span data-ttu-id="8f570-125">Annars är valfria.</span><span class="sxs-lookup"><span data-stu-id="8f570-125">Otherwise it is optional.</span></span>

## <a name="review-results"></a><span data-ttu-id="8f570-126">Granska resultaten</span><span class="sxs-lookup"><span data-stu-id="8f570-126">Review results</span></span>

<span data-ttu-id="8f570-127">När du är färdig tillhandahålls hello resultat.</span><span class="sxs-lookup"><span data-stu-id="8f570-127">When complete, hello results are provided.</span></span> <span data-ttu-id="8f570-128">hello nästa hopp IP-adress returneras samt hello typ av resurs som det är.</span><span class="sxs-lookup"><span data-stu-id="8f570-128">hello next hop IP address is returned as well as hello type of resource it is.</span></span>

```
data:    Next Hop Ip Address             : 10.0.1.2
info:    network watcher next-hop command OK
```

<span data-ttu-id="8f570-129">hello visar följande lista hello tillgängliga NextHopType värden:</span><span class="sxs-lookup"><span data-stu-id="8f570-129">hello following list shows hello currently available NextHopType values:</span></span>

<span data-ttu-id="8f570-130">**Nästa Hopptyp**</span><span class="sxs-lookup"><span data-stu-id="8f570-130">**Next Hop Type**</span></span>

* <span data-ttu-id="8f570-131">Internet</span><span class="sxs-lookup"><span data-stu-id="8f570-131">Internet</span></span>
* <span data-ttu-id="8f570-132">VirtualAppliance</span><span class="sxs-lookup"><span data-stu-id="8f570-132">VirtualAppliance</span></span>
* <span data-ttu-id="8f570-133">VirtualNetworkGateway</span><span class="sxs-lookup"><span data-stu-id="8f570-133">VirtualNetworkGateway</span></span>
* <span data-ttu-id="8f570-134">VnetLocal</span><span class="sxs-lookup"><span data-stu-id="8f570-134">VnetLocal</span></span>
* <span data-ttu-id="8f570-135">HyperNetGateway</span><span class="sxs-lookup"><span data-stu-id="8f570-135">HyperNetGateway</span></span>
* <span data-ttu-id="8f570-136">VnetPeering</span><span class="sxs-lookup"><span data-stu-id="8f570-136">VnetPeering</span></span>
* <span data-ttu-id="8f570-137">Ingen</span><span class="sxs-lookup"><span data-stu-id="8f570-137">None</span></span>

## <a name="next-steps"></a><span data-ttu-id="8f570-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8f570-138">Next steps</span></span>

<span data-ttu-id="8f570-139">Lär dig hur tooreview säkerhet grupp nätverksinställningarna via programmering genom att besöka [NSG granskning med Nätverksbevakaren](network-watcher-nsg-auditing-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="8f570-139">Learn how tooreview your network security group settings programmatically by visiting [NSG Auditing with Network Watcher](network-watcher-nsg-auditing-powershell.md)</span></span>
