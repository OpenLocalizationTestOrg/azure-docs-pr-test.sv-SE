---
title: "Sök efter nästa hopp med Azure Network Watcher nexthop - Azure-portalen | Microsoft Docs"
description: "Den här artikeln beskriver hur du kan hitta nästa hopptyp är och ip-adressen med nästa hopp med Azure-portalen"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 7b459dcf-4077-424e-a774-f7bfa34c5975
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 5434b7972346821985c459fc4620805adb88676b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="find-out-what-the-next-hop-type-is-using-the-next-hop-capability-in-azure-network-watcher-using-the-portal"></a><span data-ttu-id="d61a4-103">Ta reda på vilka nästa hopptyp är med nästa hopp-funktionen i Azure Nätverksbevakaren med hjälp av portalen</span><span class="sxs-lookup"><span data-stu-id="d61a4-103">Find out what the next hop type is using the Next Hop capability in Azure Network Watcher using the portal</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="d61a4-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="d61a4-104">Azure portal</span></span>](network-watcher-check-next-hop-portal.md)
> - [<span data-ttu-id="d61a4-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="d61a4-105">PowerShell</span></span>](network-watcher-check-next-hop-powershell.md)
> - [<span data-ttu-id="d61a4-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="d61a4-106">CLI 1.0</span></span>](network-watcher-check-next-hop-cli-nodejs.md)
> - [<span data-ttu-id="d61a4-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="d61a4-107">CLI 2.0</span></span>](network-watcher-check-next-hop-cli.md)
> - [<span data-ttu-id="d61a4-108">Azure REST-API</span><span class="sxs-lookup"><span data-stu-id="d61a4-108">Azure REST API</span></span>](network-watcher-check-next-hop-rest.md)

<span data-ttu-id="d61a4-109">Nästa hopp är en funktion i Nätverksbevakaren som tillhandahåller möjligheten get nästa hopptyp och IP-adress baserat på en angiven virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="d61a4-109">Next hop is a feature of Network Watcher that provides the ability get the next hop type and IP address based on a specified virtual machine.</span></span> <span data-ttu-id="d61a4-110">Den här funktionen är användbart för att fastställa om trafik som lämnar en virtuell dator som passerar en gateway, internet eller virtuella nätverk för att komma till sin destination.</span><span class="sxs-lookup"><span data-stu-id="d61a4-110">This feature is useful in determining if traffic leaving a virtual machine traverses a gateway, internet, or virtual networks to get to its destination.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="d61a4-111">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="d61a4-111">Before you begin</span></span>

<span data-ttu-id="d61a4-112">Det här scenariot förutsätter att du redan har följt stegen i [skapa en Nätverksbevakaren](network-watcher-create.md) att skapa en Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="d61a4-112">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher.</span></span> <span data-ttu-id="d61a4-113">Det här scenariot förutsätter att det finns en resursgrupp med en giltig virtuell dator som ska användas.</span><span class="sxs-lookup"><span data-stu-id="d61a4-113">The scenario also assumes that a Resource Group with a valid virtual machine exists to be used.</span></span>

## <a name="scenario"></a><span data-ttu-id="d61a4-114">Scenario</span><span class="sxs-lookup"><span data-stu-id="d61a4-114">Scenario</span></span>

<span data-ttu-id="d61a4-115">Det scenario som beskrivs i den här artikeln använder nästa hopp för att ta reda på nästa hopptyp och IP-adress för en resurs.</span><span class="sxs-lookup"><span data-stu-id="d61a4-115">The scenario covered in this article uses Next hop to find out the next hop type and IP address for a resource.</span></span> <span data-ttu-id="d61a4-116">Läs mer om nästa hopp [nästa hopp översikt](network-watcher-next-hop-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d61a4-116">To learn more about Next Hop, visit [Next Hop Overview](network-watcher-next-hop-overview.md).</span></span>

<span data-ttu-id="d61a4-117">I det här scenariot kommer du att:</span><span class="sxs-lookup"><span data-stu-id="d61a4-117">In this scenario, you will:</span></span>

* <span data-ttu-id="d61a4-118">Hämta nästa hopp från en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="d61a4-118">Retrieve the next hop from a virtual machine.</span></span>

## <a name="get-next-hop"></a><span data-ttu-id="d61a4-119">Hämta nästa hopp</span><span class="sxs-lookup"><span data-stu-id="d61a4-119">Get Next Hop</span></span>

### <a name="step-1"></a><span data-ttu-id="d61a4-120">Steg 1</span><span class="sxs-lookup"><span data-stu-id="d61a4-120">Step 1</span></span>

<span data-ttu-id="d61a4-121">Gå till din Nätverksbevakaren resurs i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="d61a4-121">Navigate to your Network Watcher resource in the Azure portal.</span></span>

### <a name="step-2"></a><span data-ttu-id="d61a4-122">Steg 2</span><span class="sxs-lookup"><span data-stu-id="d61a4-122">Step 2</span></span>

<span data-ttu-id="d61a4-123">Klicka på **nästa hopp** i navigeringsfönstret, Välj den virtuella datorn och nätverksgränssnittet fylla källa och mål-IP, och klicka på den **nästa hopp** knappen.</span><span class="sxs-lookup"><span data-stu-id="d61a4-123">Click **Next hop** in the navigation pane, select the virtual machine and network interface, fill out the source and destination IP, and click the **Next hop** button.</span></span>

> [!NOTE]
> <span data-ttu-id="d61a4-124">Nästa hopp kräver att den Virtuella datorresursen har allokerats för att köras.</span><span class="sxs-lookup"><span data-stu-id="d61a4-124">Next hop requires that the VM resource is allocated to run.</span></span>

![Hämta nästa hopp-översikt][1]

### <a name="step-3"></a><span data-ttu-id="d61a4-126">Steg 3</span><span class="sxs-lookup"><span data-stu-id="d61a4-126">Step 3</span></span>

<span data-ttu-id="d61a4-127">När aktiviteten har slutförts tillhandahålls resultaten.</span><span class="sxs-lookup"><span data-stu-id="d61a4-127">Once the task is complete, the results are provided.</span></span> <span data-ttu-id="d61a4-128">IP-adressen och typ av enhet nästa hopp är visas.</span><span class="sxs-lookup"><span data-stu-id="d61a4-128">The IP address and type of device the next hop is, is displayed.</span></span> <span data-ttu-id="d61a4-129">Följande tabell visar de tillgängliga returnerade värdena i portalen.</span><span class="sxs-lookup"><span data-stu-id="d61a4-129">The following table shows the available returned values in the portal.</span></span>

<span data-ttu-id="d61a4-130">**Nästa Hopptyp**</span><span class="sxs-lookup"><span data-stu-id="d61a4-130">**Next Hop Type**</span></span>

* <span data-ttu-id="d61a4-131">Internet</span><span class="sxs-lookup"><span data-stu-id="d61a4-131">Internet</span></span>
* <span data-ttu-id="d61a4-132">VirtualAppliance</span><span class="sxs-lookup"><span data-stu-id="d61a4-132">VirtualAppliance</span></span>
* <span data-ttu-id="d61a4-133">VirtualNetworkGateway</span><span class="sxs-lookup"><span data-stu-id="d61a4-133">VirtualNetworkGateway</span></span>
* <span data-ttu-id="d61a4-134">VnetLocal</span><span class="sxs-lookup"><span data-stu-id="d61a4-134">VnetLocal</span></span>
* <span data-ttu-id="d61a4-135">HyperNetGateway</span><span class="sxs-lookup"><span data-stu-id="d61a4-135">HyperNetGateway</span></span>
* <span data-ttu-id="d61a4-136">VnetPeering</span><span class="sxs-lookup"><span data-stu-id="d61a4-136">VnetPeering</span></span>
* <span data-ttu-id="d61a4-137">Ingen</span><span class="sxs-lookup"><span data-stu-id="d61a4-137">None</span></span>

<span data-ttu-id="d61a4-138">Om en anpassad väg användes för att dirigera trafiken, visas användardefinierad väg (UDR) samt med resultat.</span><span class="sxs-lookup"><span data-stu-id="d61a4-138">If a custom route was used to route this traffic, the User-defined route (UDR) is shown as well with the results.</span></span>

![Nästa hopp-resultat][2]

## <a name="next-steps"></a><span data-ttu-id="d61a4-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d61a4-140">Next steps</span></span>

<span data-ttu-id="d61a4-141">Lär dig hur du granskar din grupp för nätverkssäkerhet via programmering genom att besöka [NSG granskning med Nätverksbevakaren](network-watcher-nsg-auditing-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="d61a4-141">Learn how to review your network security group settings programmatically by visiting [NSG Auditing with Network Watcher](network-watcher-nsg-auditing-powershell.md)</span></span>

[1]: ./media/network-watcher-check-next-hop-portal/figure1.png
[2]: ./media/network-watcher-check-next-hop-portal/figure2.png














