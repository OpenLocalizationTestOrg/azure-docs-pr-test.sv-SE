---
title: "aaaFind nästa hopp med Azure Network Watcher nexthop - PowerShell | Microsoft Docs"
description: "Den här artikeln beskriver hur du kan hitta vad hello nästa hopptyp är och IP-adress med hjälp av nästa hopp med hjälp av PowerShell."
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
ms.openlocfilehash: fdb0b4a02d95fc45c103fe952fc1afa095414c18
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="find-out-what-hello-next-hop-type-is-using-hello-next-hop-capability-in-azure-network-watcher-using-powershell"></a><span data-ttu-id="4bda0-103">Ta reda på vilka hello nästa hopptyp med hello nexthop-funktionen i Azure Nätverksbevakaren med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="4bda0-103">Find out what hello next hop type is using hello Next Hop capability in Azure Network Watcher using PowerShell</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="4bda0-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="4bda0-104">Azure portal</span></span>](network-watcher-check-next-hop-portal.md)
> - [<span data-ttu-id="4bda0-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="4bda0-105">PowerShell</span></span>](network-watcher-check-next-hop-powershell.md)
> - [<span data-ttu-id="4bda0-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="4bda0-106">CLI 1.0</span></span>](network-watcher-check-next-hop-cli-nodejs.md)
> - [<span data-ttu-id="4bda0-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="4bda0-107">CLI 2.0</span></span>](network-watcher-check-next-hop-cli.md)
> - [<span data-ttu-id="4bda0-108">Azure REST-API</span><span class="sxs-lookup"><span data-stu-id="4bda0-108">Azure REST API</span></span>](network-watcher-check-next-hop-rest.md)

<span data-ttu-id="4bda0-109">Nästa hopp är en funktion i Nätverksbevakaren som ger hello möjlighet hämta hello nästa hopptyp och IP-adress baserat på en angiven virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="4bda0-109">Next hop is a feature of Network Watcher that provides hello ability get hello next hop type and IP address based on a specified virtual machine.</span></span> <span data-ttu-id="4bda0-110">Den här funktionen är användbart för att fastställa om trafik som lämnar en virtuell dator som passerar en gateway, internet eller virtuella nätverk tooget tooits mål.</span><span class="sxs-lookup"><span data-stu-id="4bda0-110">This feature is useful in determining if traffic leaving a virtual machine traverses a gateway, internet, or virtual networks tooget tooits destination.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="4bda0-111">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="4bda0-111">Before you begin</span></span>

<span data-ttu-id="4bda0-112">I det här scenariot använder du hello Azure portal toofind hello nästa hopptyp och IP-adress.</span><span class="sxs-lookup"><span data-stu-id="4bda0-112">In this scenario, you will use hello Azure portal toofind hello next hop type and IP address.</span></span>

<span data-ttu-id="4bda0-113">Det här scenariot förutsätter att du redan har följt stegen hello i [skapa en Nätverksbevakaren](network-watcher-create.md) toocreate en Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="4bda0-113">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span> <span data-ttu-id="4bda0-114">hello scenariot förutsätter att en resursgrupp med en giltig virtuell dator finns toobe används.</span><span class="sxs-lookup"><span data-stu-id="4bda0-114">hello scenario also assumes that a Resource Group with a valid virtual machine exists toobe used.</span></span>

## <a name="scenario"></a><span data-ttu-id="4bda0-115">Scenario</span><span class="sxs-lookup"><span data-stu-id="4bda0-115">Scenario</span></span>

<span data-ttu-id="4bda0-116">hello-scenario som beskrivs i den här artikeln används nästa hopp, en funktion i Nätverksbevakaren som söker efter hello nästa hopptyp och IP-adress för en resurs.</span><span class="sxs-lookup"><span data-stu-id="4bda0-116">hello scenario covered in this article uses Next Hop, a feature of Network Watcher that finds out hello next hop type and IP address for a resource.</span></span> <span data-ttu-id="4bda0-117">Besök toolearn mer om nästa hopp [nästa hopp översikt](network-watcher-next-hop-overview.md).</span><span class="sxs-lookup"><span data-stu-id="4bda0-117">toolearn more about Next Hop, visit [Next Hop Overview](network-watcher-next-hop-overview.md).</span></span>

## <a name="retrieve-network-watcher"></a><span data-ttu-id="4bda0-118">Hämta Nätverksbevakaren</span><span class="sxs-lookup"><span data-stu-id="4bda0-118">Retrieve Network Watcher</span></span>

<span data-ttu-id="4bda0-119">hello första steget är tooretrieve hello Nätverksbevakaren instans.</span><span class="sxs-lookup"><span data-stu-id="4bda0-119">hello first step is tooretrieve hello Network Watcher instance.</span></span> <span data-ttu-id="4bda0-120">Hej `$networkWatcher` variabel har skickats toohello nästa hopp Kontrollera cmdlet.</span><span class="sxs-lookup"><span data-stu-id="4bda0-120">hello `$networkWatcher` variable is passed toohello next hop verify cmdlet.</span></span>

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName 
```

## <a name="get-a-virtual-machine"></a><span data-ttu-id="4bda0-121">Hämta en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="4bda0-121">Get a virtual machine</span></span>

<span data-ttu-id="4bda0-122">Nästa hopp returnerar hello nästa hopp och hello IP-adressen för nästa hopp för hello från en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="4bda0-122">Next hop returns hello next hop and hello IP address of hello next hop from a virtual machine.</span></span> <span data-ttu-id="4bda0-123">Ett Id för en virtuell dator krävs för hello cmdlet.</span><span class="sxs-lookup"><span data-stu-id="4bda0-123">An Id of a virtual machine is required for hello cmdlet.</span></span> <span data-ttu-id="4bda0-124">Om du redan vet hello-ID för hello virtuella toouse kan du hoppa över det här steget.</span><span class="sxs-lookup"><span data-stu-id="4bda0-124">If you already know hello ID of hello virtual machine toouse, you can skip this step.</span></span>

```powershell
$VM = Get-AzurermVM -ResourceGroupName "testrg" -Name "testvm1"
```

> [!NOTE]
> <span data-ttu-id="4bda0-125">Nästa hopp kräver att hello Virtuella datorresursen fördelas toorun.</span><span class="sxs-lookup"><span data-stu-id="4bda0-125">Next hop requires that hello VM resource is allocated toorun.</span></span>

## <a name="get-hello-network-interfaces"></a><span data-ttu-id="4bda0-126">Hämta hello nätverksgränssnitt</span><span class="sxs-lookup"><span data-stu-id="4bda0-126">Get hello network interfaces</span></span>

<span data-ttu-id="4bda0-127">hello IP-adress till ett nätverkskort på den virtuella datorn hello krävs i det här exemplet vi hämta hello nätverkskort på en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="4bda0-127">hello IP address of a NIC on hello virtual machine is needed, in this example we retrieve hello NICs on a virtual machine.</span></span> <span data-ttu-id="4bda0-128">Om du redan vet hello IP-adress som du vill tootest på hello virtuell dator kan du hoppa över det här steget.</span><span class="sxs-lookup"><span data-stu-id="4bda0-128">If you already know hello IP address that you want tootest on hello virtual machine, you can skip this step.</span></span>

```powershell
$Nics = Get-AzureRmNetworkInterface | Where {$_.Id -eq $vm.NetworkProfile.NetworkInterfaces.Id.ForEach({$_})}
```

## <a name="get-next-hop"></a><span data-ttu-id="4bda0-129">Hämta nästa hopp</span><span class="sxs-lookup"><span data-stu-id="4bda0-129">Get Next Hop</span></span>

<span data-ttu-id="4bda0-130">Nu vi kallar hello `Get-AzureRmNetworkWatcherNextHop` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="4bda0-130">Now we call hello `Get-AzureRmNetworkWatcherNextHop` cmdlet.</span></span> <span data-ttu-id="4bda0-131">Vi skickar hello cmdlet hello Nätverksbevakaren, virtuella Id, käll-IP-adress och mål-IP-adress.</span><span class="sxs-lookup"><span data-stu-id="4bda0-131">We pass hello cmdlet hello Network Watcher, virtual machine Id, source IP address, and destination IP address.</span></span> <span data-ttu-id="4bda0-132">I det här exemplet är IP-måladress hello tooa VM i ett annat virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="4bda0-132">In this example, hello destination IP address is tooa VM in another virtual network.</span></span> <span data-ttu-id="4bda0-133">Det finns en virtuell nätverksgateway mellan hello två virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="4bda0-133">There is a virtual network gateway between hello two virtual networks.</span></span>

```powershell
Get-AzureRmNetworkWatcherNextHop -NetworkWatcher $networkWatcher -TargetVirtualMachineId $VM.Id -SourceIPAddress $nics[0].IpConfigurations[0].PrivateIpAddress  -DestinationIPAddress 10.0.2.4 
```

## <a name="review-results"></a><span data-ttu-id="4bda0-134">Granska resultaten</span><span class="sxs-lookup"><span data-stu-id="4bda0-134">Review results</span></span>

<span data-ttu-id="4bda0-135">När du är färdig tillhandahålls hello resultat.</span><span class="sxs-lookup"><span data-stu-id="4bda0-135">When complete, hello results are provided.</span></span> <span data-ttu-id="4bda0-136">hello nästa hopp IP-adress returneras samt hello typ av resurs som det är.</span><span class="sxs-lookup"><span data-stu-id="4bda0-136">hello next hop IP address is returned as well as hello type of resource it is.</span></span> <span data-ttu-id="4bda0-137">I det här scenariot är det hello offentliga IP-adress hello virtuell nätverksgateway.</span><span class="sxs-lookup"><span data-stu-id="4bda0-137">In this scenario, it is hello public IP address of hello virtual network gateway.</span></span>

```
NextHopIpAddress NextHopType           RouteTableId 
---------------- -----------           ------------ 
13.78.238.92     VirtualNetworkGateway Gateway Route
```

<span data-ttu-id="4bda0-138">hello visar följande lista hello tillgängliga NextHopType värden:</span><span class="sxs-lookup"><span data-stu-id="4bda0-138">hello following list shows hello currently available NextHopType values:</span></span>

<span data-ttu-id="4bda0-139">**Nästa Hopptyp**</span><span class="sxs-lookup"><span data-stu-id="4bda0-139">**Next Hop Type**</span></span>

* <span data-ttu-id="4bda0-140">Internet</span><span class="sxs-lookup"><span data-stu-id="4bda0-140">Internet</span></span>
* <span data-ttu-id="4bda0-141">VirtualAppliance</span><span class="sxs-lookup"><span data-stu-id="4bda0-141">VirtualAppliance</span></span>
* <span data-ttu-id="4bda0-142">VirtualNetworkGateway</span><span class="sxs-lookup"><span data-stu-id="4bda0-142">VirtualNetworkGateway</span></span>
* <span data-ttu-id="4bda0-143">VnetLocal</span><span class="sxs-lookup"><span data-stu-id="4bda0-143">VnetLocal</span></span>
* <span data-ttu-id="4bda0-144">HyperNetGateway</span><span class="sxs-lookup"><span data-stu-id="4bda0-144">HyperNetGateway</span></span>
* <span data-ttu-id="4bda0-145">VnetPeering</span><span class="sxs-lookup"><span data-stu-id="4bda0-145">VnetPeering</span></span>
* <span data-ttu-id="4bda0-146">Ingen</span><span class="sxs-lookup"><span data-stu-id="4bda0-146">None</span></span>

## <a name="next-steps"></a><span data-ttu-id="4bda0-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4bda0-147">Next steps</span></span>

<span data-ttu-id="4bda0-148">Lär dig hur tooreview säkerhet grupp nätverksinställningarna via programmering genom att besöka [NSG granskning med Nätverksbevakaren](network-watcher-nsg-auditing-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="4bda0-148">Learn how tooreview your network security group settings programmatically by visiting [NSG Auditing with Network Watcher](network-watcher-nsg-auditing-powershell.md)</span></span>

















