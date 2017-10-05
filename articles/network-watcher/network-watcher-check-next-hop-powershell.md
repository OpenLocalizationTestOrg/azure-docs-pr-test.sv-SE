---
title: "Sök efter nästa hopp med Azure Network Watcher nexthop - PowerShell | Microsoft Docs"
description: "Den här artikeln beskriver hur du kan hitta nästa hopptyp är och ip-adressen med nästa hopp med hjälp av PowerShell."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 6a656c55-17bd-40f1-905d-90659087639c
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 00161e7c6fb4becdb7d8eab266fa27128e50f8ca
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="find-out-what-the-next-hop-type-is-using-the-next-hop-capability-in-azure-network-watcher-using-powershell"></a><span data-ttu-id="054ac-103">Ta reda på vilka nästa hopptyp är med nästa hopp-funktionen i Azure Nätverksbevakaren med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="054ac-103">Find out what the next hop type is using the Next Hop capability in Azure Network Watcher using PowerShell</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="054ac-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="054ac-104">Azure portal</span></span>](network-watcher-check-next-hop-portal.md)
> - [<span data-ttu-id="054ac-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="054ac-105">PowerShell</span></span>](network-watcher-check-next-hop-powershell.md)
> - [<span data-ttu-id="054ac-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="054ac-106">CLI 1.0</span></span>](network-watcher-check-next-hop-cli-nodejs.md)
> - [<span data-ttu-id="054ac-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="054ac-107">CLI 2.0</span></span>](network-watcher-check-next-hop-cli.md)
> - [<span data-ttu-id="054ac-108">Azure REST-API</span><span class="sxs-lookup"><span data-stu-id="054ac-108">Azure REST API</span></span>](network-watcher-check-next-hop-rest.md)

<span data-ttu-id="054ac-109">Nästa hopp är en funktion i Nätverksbevakaren som tillhandahåller möjligheten get nästa hopptyp och IP-adress baserat på en angiven virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="054ac-109">Next hop is a feature of Network Watcher that provides the ability get the next hop type and IP address based on a specified virtual machine.</span></span> <span data-ttu-id="054ac-110">Den här funktionen är användbart för att fastställa om trafik som lämnar en virtuell dator som passerar en gateway, internet eller virtuella nätverk för att komma till sin destination.</span><span class="sxs-lookup"><span data-stu-id="054ac-110">This feature is useful in determining if traffic leaving a virtual machine traverses a gateway, internet, or virtual networks to get to its destination.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="054ac-111">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="054ac-111">Before you begin</span></span>

<span data-ttu-id="054ac-112">I det här scenariot använder du Azure-portalen för att hitta nästa hopptyp och IP-adress.</span><span class="sxs-lookup"><span data-stu-id="054ac-112">In this scenario, you will use the Azure portal to find the next hop type and IP address.</span></span>

<span data-ttu-id="054ac-113">Det här scenariot förutsätter att du redan har följt stegen i [skapa en Nätverksbevakaren](network-watcher-create.md) att skapa en Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="054ac-113">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher.</span></span> <span data-ttu-id="054ac-114">Det här scenariot förutsätter att det finns en resursgrupp med en giltig virtuell dator som ska användas.</span><span class="sxs-lookup"><span data-stu-id="054ac-114">The scenario also assumes that a Resource Group with a valid virtual machine exists to be used.</span></span>

## <a name="scenario"></a><span data-ttu-id="054ac-115">Scenario</span><span class="sxs-lookup"><span data-stu-id="054ac-115">Scenario</span></span>

<span data-ttu-id="054ac-116">Det scenario som beskrivs i den här artikeln använder nästa hopp, en funktion i Nätverksbevakaren som söker efter nästa hopptyp och IP-adress för en resurs.</span><span class="sxs-lookup"><span data-stu-id="054ac-116">The scenario covered in this article uses Next Hop, a feature of Network Watcher that finds out the next hop type and IP address for a resource.</span></span> <span data-ttu-id="054ac-117">Läs mer om nästa hopp [nästa hopp översikt](network-watcher-next-hop-overview.md).</span><span class="sxs-lookup"><span data-stu-id="054ac-117">To learn more about Next Hop, visit [Next Hop Overview](network-watcher-next-hop-overview.md).</span></span>

## <a name="retrieve-network-watcher"></a><span data-ttu-id="054ac-118">Hämta Nätverksbevakaren</span><span class="sxs-lookup"><span data-stu-id="054ac-118">Retrieve Network Watcher</span></span>

<span data-ttu-id="054ac-119">Det första steget är att hämta Nätverksbevakaren-instans.</span><span class="sxs-lookup"><span data-stu-id="054ac-119">The first step is to retrieve the Network Watcher instance.</span></span> <span data-ttu-id="054ac-120">Den `$networkWatcher` variabel har skickats till nästa hopp Kontrollera cmdlet.</span><span class="sxs-lookup"><span data-stu-id="054ac-120">The `$networkWatcher` variable is passed to the next hop verify cmdlet.</span></span>

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName 
```

## <a name="get-a-virtual-machine"></a><span data-ttu-id="054ac-121">Hämta en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="054ac-121">Get a virtual machine</span></span>

<span data-ttu-id="054ac-122">Nästa hopp Returnerar nästa hopp och IP-adressen för nästa hopp från en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="054ac-122">Next hop returns the next hop and the IP address of the next hop from a virtual machine.</span></span> <span data-ttu-id="054ac-123">Ett Id för en virtuell dator krävs för cmdlet.</span><span class="sxs-lookup"><span data-stu-id="054ac-123">An Id of a virtual machine is required for the cmdlet.</span></span> <span data-ttu-id="054ac-124">Om du redan känner till ID: T för den virtuella datorn att använda, kan du hoppa över det här steget.</span><span class="sxs-lookup"><span data-stu-id="054ac-124">If you already know the ID of the virtual machine to use, you can skip this step.</span></span>

```powershell
$VM = Get-AzurermVM -ResourceGroupName "testrg" -Name "testvm1"
```

> [!NOTE]
> <span data-ttu-id="054ac-125">Nästa hopp kräver att den Virtuella datorresursen har allokerats för att köras.</span><span class="sxs-lookup"><span data-stu-id="054ac-125">Next hop requires that the VM resource is allocated to run.</span></span>

## <a name="get-the-network-interfaces"></a><span data-ttu-id="054ac-126">Hämta nätverksgränssnitt</span><span class="sxs-lookup"><span data-stu-id="054ac-126">Get the network interfaces</span></span>

<span data-ttu-id="054ac-127">IP-adressen för ett nätverkskort på den virtuella datorn krävs i det här exemplet vi hämta nätverkskort på en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="054ac-127">The IP address of a NIC on the virtual machine is needed, in this example we retrieve the NICs on a virtual machine.</span></span> <span data-ttu-id="054ac-128">Om du redan känner till IP-adressen som du vill testa på den virtuella datorn, kan du hoppa över det här steget.</span><span class="sxs-lookup"><span data-stu-id="054ac-128">If you already know the IP address that you want to test on the virtual machine, you can skip this step.</span></span>

```powershell
$Nics = Get-AzureRmNetworkInterface | Where {$_.Id -eq $vm.NetworkProfile.NetworkInterfaces.Id.ForEach({$_})}
```

## <a name="get-next-hop"></a><span data-ttu-id="054ac-129">Hämta nästa hopp</span><span class="sxs-lookup"><span data-stu-id="054ac-129">Get Next Hop</span></span>

<span data-ttu-id="054ac-130">Nu vi kallar det `Get-AzureRmNetworkWatcherNextHop` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="054ac-130">Now we call the `Get-AzureRmNetworkWatcherNextHop` cmdlet.</span></span> <span data-ttu-id="054ac-131">Vi skickar cmdlet Nätverksbevakaren, virtuella Id, källans IP-adress och mål-IP-adress.</span><span class="sxs-lookup"><span data-stu-id="054ac-131">We pass the cmdlet the Network Watcher, virtual machine Id, source IP address, and destination IP address.</span></span> <span data-ttu-id="054ac-132">I det här exemplet är den IP-adressen till en virtuell dator i ett annat virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="054ac-132">In this example, the destination IP address is to a VM in another virtual network.</span></span> <span data-ttu-id="054ac-133">Det finns en virtuell nätverksgateway mellan de två virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="054ac-133">There is a virtual network gateway between the two virtual networks.</span></span>

```powershell
Get-AzureRmNetworkWatcherNextHop -NetworkWatcher $networkWatcher -TargetVirtualMachineId $VM.Id -SourceIPAddress $nics[0].IpConfigurations[0].PrivateIpAddress  -DestinationIPAddress 10.0.2.4 
```

## <a name="review-results"></a><span data-ttu-id="054ac-134">Granska resultaten</span><span class="sxs-lookup"><span data-stu-id="054ac-134">Review results</span></span>

<span data-ttu-id="054ac-135">När du är färdig tillhandahålls resultaten.</span><span class="sxs-lookup"><span data-stu-id="054ac-135">When complete, the results are provided.</span></span> <span data-ttu-id="054ac-136">Nästa hopp IP-adress returneras samt typ av resurs som det är.</span><span class="sxs-lookup"><span data-stu-id="054ac-136">The next hop IP address is returned as well as the type of resource it is.</span></span> <span data-ttu-id="054ac-137">I det här scenariot är det offentliga IP-adressen för den virtuella nätverksgatewayen.</span><span class="sxs-lookup"><span data-stu-id="054ac-137">In this scenario, it is the public IP address of the virtual network gateway.</span></span>

```
NextHopIpAddress NextHopType           RouteTableId 
---------------- -----------           ------------ 
13.78.238.92     VirtualNetworkGateway Gateway Route
```

<span data-ttu-id="054ac-138">I följande lista visas de tillgängliga värdena för NextHopType:</span><span class="sxs-lookup"><span data-stu-id="054ac-138">The following list shows the currently available NextHopType values:</span></span>

<span data-ttu-id="054ac-139">**Nästa Hopptyp**</span><span class="sxs-lookup"><span data-stu-id="054ac-139">**Next Hop Type**</span></span>

* <span data-ttu-id="054ac-140">Internet</span><span class="sxs-lookup"><span data-stu-id="054ac-140">Internet</span></span>
* <span data-ttu-id="054ac-141">VirtualAppliance</span><span class="sxs-lookup"><span data-stu-id="054ac-141">VirtualAppliance</span></span>
* <span data-ttu-id="054ac-142">VirtualNetworkGateway</span><span class="sxs-lookup"><span data-stu-id="054ac-142">VirtualNetworkGateway</span></span>
* <span data-ttu-id="054ac-143">VnetLocal</span><span class="sxs-lookup"><span data-stu-id="054ac-143">VnetLocal</span></span>
* <span data-ttu-id="054ac-144">HyperNetGateway</span><span class="sxs-lookup"><span data-stu-id="054ac-144">HyperNetGateway</span></span>
* <span data-ttu-id="054ac-145">VnetPeering</span><span class="sxs-lookup"><span data-stu-id="054ac-145">VnetPeering</span></span>
* <span data-ttu-id="054ac-146">Ingen</span><span class="sxs-lookup"><span data-stu-id="054ac-146">None</span></span>

## <a name="next-steps"></a><span data-ttu-id="054ac-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="054ac-147">Next steps</span></span>

<span data-ttu-id="054ac-148">Lär dig hur du granskar din grupp för nätverkssäkerhet via programmering genom att besöka [NSG granskning med Nätverksbevakaren](network-watcher-nsg-auditing-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="054ac-148">Learn how to review your network security group settings programmatically by visiting [NSG Auditing with Network Watcher](network-watcher-nsg-auditing-powershell.md)</span></span>

















