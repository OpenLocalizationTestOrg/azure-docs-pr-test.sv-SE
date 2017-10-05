---
title: "Mutiple VIP: er för en tjänst i molnet"
description: "Översikt över multiVIP och hur du anger flera VIP: er för en tjänst i molnet"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
ms.assetid: 85f6d26a-3df5-4b8e-96a1-92b2793b5284
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
ms.openlocfilehash: f40e0501eed8d5f296e7c79d8a35705a695ae6fd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="configure-multiple-vips-for-a-cloud-service"></a><span data-ttu-id="5a629-103">Konfigurera flera VIP: er för en tjänst i molnet</span><span class="sxs-lookup"><span data-stu-id="5a629-103">Configure multiple VIPs for a cloud service</span></span>

<span data-ttu-id="5a629-104">Du kan komma åt Azure-molntjänster via det offentliga Internet med hjälp av en IP-adress som tillhandahålls av Azure.</span><span class="sxs-lookup"><span data-stu-id="5a629-104">You can access Azure cloud services over the public Internet by using an IP address provided by Azure.</span></span> <span data-ttu-id="5a629-105">Den här offentliga IP-adressen kallas för en VIP (virtuell IP) eftersom den är kopplad till Azure belastningsutjämnare och inte den virtuella datorn (VM) instanser i Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="5a629-105">This public IP address is referred to as a VIP (virtual IP) since it is linked to the Azure load balancer, and not the Virtual Machine (VM) instances within the cloud service.</span></span> <span data-ttu-id="5a629-106">Du kan komma åt alla VM-instans i en molntjänst med hjälp av en enskild VIP.</span><span class="sxs-lookup"><span data-stu-id="5a629-106">You can access any VM instance within a cloud service by using a single VIP.</span></span>

<span data-ttu-id="5a629-107">Det finns emellertid scenarier där du kanske behöver flera VIP som en post som pekar på samma tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="5a629-107">However, there are scenarios in which you may need more than one VIP as an entry point to the same cloud service.</span></span> <span data-ttu-id="5a629-108">Molntjänsten kan exempelvis vara värd för flera webbplatser som kräver SSL-anslutning med standardporten 443, eftersom varje plats är värd för en annan kund eller klient.</span><span class="sxs-lookup"><span data-stu-id="5a629-108">For instance, your cloud service may host multiple websites that require SSL connectivity using the default port of 443, as each site is hosted for a different customer, or tenant.</span></span> <span data-ttu-id="5a629-109">I det här scenariot behöver du en annan offentlig Internetriktade IP-adress för varje webbplats.</span><span class="sxs-lookup"><span data-stu-id="5a629-109">In this scenario, you need to have a different public facing IP address for each website.</span></span> <span data-ttu-id="5a629-110">Diagrammet nedan illustrerar en typisk flera innehavare webbvärd med behov för flera SSL-certifikat på samma offentlig port.</span><span class="sxs-lookup"><span data-stu-id="5a629-110">The diagram below illustrates a typical multi-tenant web hosting with a need for multiple SSL certificates on the same public port.</span></span>

![Scenario med flera VIP SSL](./media/load-balancer-multivip/Figure1.png)

<span data-ttu-id="5a629-112">I exemplet ovan alla VIP: er använda samma offentlig port (443) och trafiken dirigeras till en eller flera belastningsutjämnade virtuella datorer på en unik privat port för interna IP-adressen för den molntjänst som är värd för alla webbplatser.</span><span class="sxs-lookup"><span data-stu-id="5a629-112">In the example above, all VIPs use the same public port (443) and traffic is redirected to one or more load balanced VMs on a unique private port for the internal IP address of the cloud service hosting all the websites.</span></span>

> [!NOTE]
> <span data-ttu-id="5a629-113">En annan situation att använda flera VIP: er är värd för flera SQL AlwaysOn availability tillgänglighetsgruppslyssnarnas på samma uppsättning virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="5a629-113">Another situation requiring the use the multiple VIPs is hosting multiple SQL AlwaysOn availability group listeners on the same set of Virtual Machines.</span></span>

<span data-ttu-id="5a629-114">VIP-adresser är dynamiska som standard, vilket innebär att den faktiska IP-adress som tilldelats Molntjänsten kan förändras över tid.</span><span class="sxs-lookup"><span data-stu-id="5a629-114">VIPs are dynamic by default, which means that the actual IP address assigned to the cloud service may change over time.</span></span> <span data-ttu-id="5a629-115">Om du vill förhindra detta att reservera en VIP för din tjänst.</span><span class="sxs-lookup"><span data-stu-id="5a629-115">To prevent that from happening, you can reserve a VIP for your service.</span></span> <span data-ttu-id="5a629-116">Mer information om reserverade virtuella IP-adresser finns [reserverade offentliga IP-Adressen](../virtual-network/virtual-networks-reserved-public-ip.md).</span><span class="sxs-lookup"><span data-stu-id="5a629-116">To learn more about reserved VIPs, see [Reserved Public IP](../virtual-network/virtual-networks-reserved-public-ip.md).</span></span>

> [!NOTE]
> <span data-ttu-id="5a629-117">Se [IP-adress priser](https://azure.microsoft.com/pricing/details/ip-addresses/) information om priser på virtuella IP-adresser och reserverade IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="5a629-117">Please see [IP Address pricing](https://azure.microsoft.com/pricing/details/ip-addresses/) for information on pricing on VIPs and reserved IPs.</span></span>

<span data-ttu-id="5a629-118">Du kan använda PowerShell för att kontrollera VIP som används av dina molntjänster samt lägga till och ta bort VIP-adresser, associera en VIP till en slutpunkt och konfigurera belastningsutjämning för en specifik VIP.</span><span class="sxs-lookup"><span data-stu-id="5a629-118">You can use PowerShell to verify the VIPs used by your cloud services, as well as add and remove VIPs, associate a VIP to an endpoint, and configure load balancing on a specific VIP.</span></span>

## <a name="limitations"></a><span data-ttu-id="5a629-119">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="5a629-119">Limitations</span></span>

<span data-ttu-id="5a629-120">För tillfället är Multi-VIP-funktionerna begränsade till följande scenarier:</span><span class="sxs-lookup"><span data-stu-id="5a629-120">At this time, Multi VIP functionality is limited to the following scenarios:</span></span>

* <span data-ttu-id="5a629-121">**Endast IaaS**.</span><span class="sxs-lookup"><span data-stu-id="5a629-121">**IaaS only**.</span></span> <span data-ttu-id="5a629-122">Du kan bara aktivera Multi-VIP för molntjänster som innehåller virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="5a629-122">You can only enable Multi VIP for cloud services that contain VMs.</span></span> <span data-ttu-id="5a629-123">Du kan använda flera VIP i PaaS scenarier med rollinstanser.</span><span class="sxs-lookup"><span data-stu-id="5a629-123">You cannot use Multi VIP in PaaS scenarios with role instances.</span></span>
* <span data-ttu-id="5a629-124">**PowerShell endast**.</span><span class="sxs-lookup"><span data-stu-id="5a629-124">**PowerShell only**.</span></span> <span data-ttu-id="5a629-125">Du kan endast hantera flera VIP med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5a629-125">You can only manage Multi VIP by using PowerShell.</span></span>

<span data-ttu-id="5a629-126">Dessa begränsningar är tillfälliga och kan ändras när som helst.</span><span class="sxs-lookup"><span data-stu-id="5a629-126">These limitations are temporary, and may change at any time.</span></span> <span data-ttu-id="5a629-127">Se till att ångra den här sidan om du vill kontrollera framtida ändringar.</span><span class="sxs-lookup"><span data-stu-id="5a629-127">Make sure to revisit this page to verify future changes.</span></span>

## <a name="how-to-add-a-vip-to-a-cloud-service"></a><span data-ttu-id="5a629-128">Hur du lägger till en VIP till en molntjänst</span><span class="sxs-lookup"><span data-stu-id="5a629-128">How to add a VIP to a cloud service</span></span>
<span data-ttu-id="5a629-129">Kör följande PowerShell-kommando för att lägga till en VIP till din tjänst:</span><span class="sxs-lookup"><span data-stu-id="5a629-129">To add a VIP to your service, run the following PowerShell command:</span></span>

```powershell
Add-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService
```

<span data-ttu-id="5a629-130">Detta kommando visar ett resultat som liknar följande exempel:</span><span class="sxs-lookup"><span data-stu-id="5a629-130">This command displays a result similar to the following sample:</span></span>

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Add-AzureVirtualIP   4bd7b638-d2e7-216f-ba38-5221233d70ce Succeeded

## <a name="how-to-remove-a-vip-from-a-cloud-service"></a><span data-ttu-id="5a629-131">Ta bort en VIP från en molnbaserad tjänst</span><span class="sxs-lookup"><span data-stu-id="5a629-131">How to remove a VIP from a cloud service</span></span>
<span data-ttu-id="5a629-132">Kör följande PowerShell-kommando för att ta bort VIP-Adressen till din tjänst i exemplet ovan:</span><span class="sxs-lookup"><span data-stu-id="5a629-132">To remove the VIP added to your service in the example above, run the following PowerShell command:</span></span>

```powershell
Remove-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService
```

> [!IMPORTANT]
> <span data-ttu-id="5a629-133">Du kan bara ta bort en VIP om det har inga slutpunkter som är kopplade till den.</span><span class="sxs-lookup"><span data-stu-id="5a629-133">You can only remove a VIP if it has no endpoints associated to it.</span></span>


## <a name="how-to-retrieve-vip-information-from-a-cloud-service"></a><span data-ttu-id="5a629-134">Hur du hämtar VIP information från en molnbaserad tjänst</span><span class="sxs-lookup"><span data-stu-id="5a629-134">How to retrieve VIP information from a Cloud Service</span></span>
<span data-ttu-id="5a629-135">Kör följande PowerShell-skript för att hämta VIP som är associerade med en tjänst i molnet:</span><span class="sxs-lookup"><span data-stu-id="5a629-135">To retrieve the VIPs associated with a cloud service, run the following PowerShell script:</span></span>

```powershell
$deployment = Get-AzureDeployment -ServiceName myService
$deployment.VirtualIPs
```

<span data-ttu-id="5a629-136">Skriptet visar resultatet liknar följande exempel:</span><span class="sxs-lookup"><span data-stu-id="5a629-136">The script displays a result similar to the following sample:</span></span>

    Address         : 191.238.74.148
    IsDnsProgrammed : True
    Name            : Vip1
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip2
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip3
    ReservedIPName  :
    ExtensionData   :

<span data-ttu-id="5a629-137">I det här exemplet har Molntjänsten 3 VIP:</span><span class="sxs-lookup"><span data-stu-id="5a629-137">In this example, the cloud service has 3 VIPs:</span></span>

* <span data-ttu-id="5a629-138">**Vip1** är standard-VIP du vet att eftersom värdet för IsDnsProgrammedName har angetts till true.</span><span class="sxs-lookup"><span data-stu-id="5a629-138">**Vip1** is the default VIP, you know that because the value for IsDnsProgrammedName is set to true.</span></span>
* <span data-ttu-id="5a629-139">**Vip2** och **Vip3** används inte eftersom de inte har några IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="5a629-139">**Vip2** and **Vip3** are not used as they do not have any IP addresses.</span></span> <span data-ttu-id="5a629-140">De ska endast användas om du kopplar en slutpunkt till VIP.</span><span class="sxs-lookup"><span data-stu-id="5a629-140">They will only be used if you associate an endpoint to the VIP.</span></span>

> [!NOTE]
> <span data-ttu-id="5a629-141">Din prenumeration kommer bara att debiteras för extra VIP: er när de är kopplade till en slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="5a629-141">Your subscription will only be charged for extra VIPs once they are associated with an endpoint.</span></span> <span data-ttu-id="5a629-142">Mer information om priser finns [priser för IP-adressen](https://azure.microsoft.com/pricing/details/ip-addresses/).</span><span class="sxs-lookup"><span data-stu-id="5a629-142">For more information on pricing, see [IP Address pricing](https://azure.microsoft.com/pricing/details/ip-addresses/).</span></span>

## <a name="how-to-associate-a-vip-to-an-endpoint"></a><span data-ttu-id="5a629-143">Så här associerar du en VIP till en slutpunkt</span><span class="sxs-lookup"><span data-stu-id="5a629-143">How to associate a VIP to an endpoint</span></span>

<span data-ttu-id="5a629-144">Kör följande PowerShell-kommando om du vill associera en VIP på en tjänst i molnet till en slutpunkt:</span><span class="sxs-lookup"><span data-stu-id="5a629-144">To associate a VIP on a cloud service to an endpoint, run the following PowerShell command:</span></span>

```powershell
Get-AzureVM -ServiceName myService -Name myVM1 |
    Add-AzureEndpoint -Name myEndpoint -Protocol tcp -LocalPort 8080 -PublicPort 80 -VirtualIPName Vip2 |
    Update-AzureVM
```

<span data-ttu-id="5a629-145">Kommandot skapar en slutpunkt som är kopplad till VIP kallas *Vip2* på port *80*, samt länkar till den virtuella datorn med namnet *myVM1* i en molntjänst med namnet *myService* med *TCP* på port *8080*.</span><span class="sxs-lookup"><span data-stu-id="5a629-145">The command creates an endpoint linked to the VIP called *Vip2* on port *80*, and links it to the VM named *myVM1* in a cloud service named *myService* using *TCP* on port *8080*.</span></span>

<span data-ttu-id="5a629-146">Kör följande PowerShell-kommando för att verifiera konfigurationen:</span><span class="sxs-lookup"><span data-stu-id="5a629-146">To verify the configuration, run the following PowerShell command:</span></span>

```powershell
$deployment = Get-AzureDeployment -ServiceName myService
$deployment.VirtualIPs
```

<span data-ttu-id="5a629-147">Utdata liknar följande exempel:</span><span class="sxs-lookup"><span data-stu-id="5a629-147">The output looks similar to the following example:</span></span>

    Address         : 191.238.74.148
    IsDnsProgrammed : True
    Name            : Vip1
    ReservedIPName  :
    ExtensionData   :

    Address         : 191.238.74.13
    IsDnsProgrammed :
    Name            : Vip2
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip3
    ReservedIPName  :
    ExtensionData   :

## <a name="how-to-enable-load-balancing-on-a-specific-vip"></a><span data-ttu-id="5a629-148">Så här aktiverar du belastningsutjämning på en specifik VIP</span><span class="sxs-lookup"><span data-stu-id="5a629-148">How to enable load balancing on a specific VIP</span></span>

<span data-ttu-id="5a629-149">Du kan koppla en enskild VIP till flera virtuella datorer för belastningsutjämning syften.</span><span class="sxs-lookup"><span data-stu-id="5a629-149">You can associate a single VIP with multiple virtual machines for load balancing purposes.</span></span> <span data-ttu-id="5a629-150">Du till exempel har en molntjänst med namnet *myService*, och två virtuella datorer med namnet *myVM1* och *myVM2*.</span><span class="sxs-lookup"><span data-stu-id="5a629-150">For example, you have a cloud service named *myService*, and two virtual machines named *myVM1* and *myVM2*.</span></span> <span data-ttu-id="5a629-151">Och Molntjänsten har flera VIP: er, en av dem med namnet *Vip2*.</span><span class="sxs-lookup"><span data-stu-id="5a629-151">And your cloud service has multiple VIPs, one of them named *Vip2*.</span></span> <span data-ttu-id="5a629-152">Om du vill se till att all trafik till port *81* på *Vip2* balanseras mellan *myVM1* och *myVM2* på port *8181*, kör följande PowerShell-skript:</span><span class="sxs-lookup"><span data-stu-id="5a629-152">If you want to ensure that all traffic to port *81* on *Vip2* is balanced between *myVM1* and *myVM2* on port *8181*, run the following PowerShell script:</span></span>

```powershell
Get-AzureVM -ServiceName myService -Name myVM1 |
    Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2 -DefaultProbe |
    Update-AzureVM

Get-AzureVM -ServiceName myService -Name myVM2 |
    Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2  -DefaultProbe |
    Update-AzureVM
```

<span data-ttu-id="5a629-153">Du kan också uppdatera din belastningsutjämnare för att använda en annan VIP.</span><span class="sxs-lookup"><span data-stu-id="5a629-153">You can also update your load balancer to use a different VIP.</span></span> <span data-ttu-id="5a629-154">Om du kör PowerShell-kommandot nedan ska du ändra belastningsutjämning som ska användas av en VIP med namnet Vip1:</span><span class="sxs-lookup"><span data-stu-id="5a629-154">For example, if you run the PowerShell command below, you will change the load balancing set to use a VIP named Vip1:</span></span>

```powershell
Set-AzureLoadBalancedEndpoint -ServiceName myService -LBSetName myLBSet -VirtualIPName Vip1
```

## <a name="next-steps"></a><span data-ttu-id="5a629-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5a629-155">Next Steps</span></span>

[<span data-ttu-id="5a629-156">Logganalys för belastningsutjämning i Azure</span><span class="sxs-lookup"><span data-stu-id="5a629-156">Log analytics for Azure Load Balance</span></span>](load-balancer-monitor-log.md)

[<span data-ttu-id="5a629-157">Internet Internetriktade belastningen översikt över belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="5a629-157">Internet facing load balancer overview</span></span>](load-balancer-internet-overview.md)

[<span data-ttu-id="5a629-158">Komma igång med belastningsutjämnaren mot Internet</span><span class="sxs-lookup"><span data-stu-id="5a629-158">Get started on Internet facing load balancer</span></span>](load-balancer-get-started-internet-arm-ps.md)

[<span data-ttu-id="5a629-159">Översikt över virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="5a629-159">Virtual Network Overview</span></span>](../virtual-network/virtual-networks-overview.md)

[<span data-ttu-id="5a629-160">Reserverad IP REST API: er</span><span class="sxs-lookup"><span data-stu-id="5a629-160">Reserved IP REST APIs</span></span>](https://msdn.microsoft.com/library/azure/dn722420.aspx)
