---
title: "aaaMutiple VIP: er för en tjänst i molnet"
description: "Översikt över multiVIP och hur tooset flera VIP: er på en tjänst i molnet"
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
ms.openlocfilehash: b3e0f2b24968cb75a7064484a09ffe94505bb70b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-multiple-vips-for-a-cloud-service"></a><span data-ttu-id="09bc6-103">Konfigurera flera VIP: er för en tjänst i molnet</span><span class="sxs-lookup"><span data-stu-id="09bc6-103">Configure multiple VIPs for a cloud service</span></span>

<span data-ttu-id="09bc6-104">Du kan komma åt Azure-molntjänster via hello offentliga Internet med hjälp av en IP-adress som anges av Azure.</span><span class="sxs-lookup"><span data-stu-id="09bc6-104">You can access Azure cloud services over hello public Internet by using an IP address provided by Azure.</span></span> <span data-ttu-id="09bc6-105">Den här offentliga IP-adressen är refererad tooas VIP (virtuell IP) eftersom den är länkad toohello Azure belastningsutjämnare och inte hello virtuell dator (VM)-instanser inom hello-Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="09bc6-105">This public IP address is referred tooas a VIP (virtual IP) since it is linked toohello Azure load balancer, and not hello Virtual Machine (VM) instances within hello cloud service.</span></span> <span data-ttu-id="09bc6-106">Du kan komma åt alla VM-instans i en molntjänst med hjälp av en enskild VIP.</span><span class="sxs-lookup"><span data-stu-id="09bc6-106">You can access any VM instance within a cloud service by using a single VIP.</span></span>

<span data-ttu-id="09bc6-107">Det finns emellertid scenarier där du kanske behöver mer än en VIP som en post punkt toohello samma molntjänst.</span><span class="sxs-lookup"><span data-stu-id="09bc6-107">However, there are scenarios in which you may need more than one VIP as an entry point toohello same cloud service.</span></span> <span data-ttu-id="09bc6-108">Molntjänsten kan exempelvis vara värd för flera webbplatser som kräver SSL-anslutning med hello standardporten 443, eftersom varje plats är värd för en annan kund eller klient.</span><span class="sxs-lookup"><span data-stu-id="09bc6-108">For instance, your cloud service may host multiple websites that require SSL connectivity using hello default port of 443, as each site is hosted for a different customer, or tenant.</span></span> <span data-ttu-id="09bc6-109">I det här scenariot behöver du toohave en annan offentlig Internetriktade IP-adress för varje webbplats.</span><span class="sxs-lookup"><span data-stu-id="09bc6-109">In this scenario, you need toohave a different public facing IP address for each website.</span></span> <span data-ttu-id="09bc6-110">hello diagrammet nedan illustrerar en typisk flera innehavare webbhotell med behov för flera SSL-certifikat på hello samma offentlig port.</span><span class="sxs-lookup"><span data-stu-id="09bc6-110">hello diagram below illustrates a typical multi-tenant web hosting with a need for multiple SSL certificates on hello same public port.</span></span>

![Scenario med flera VIP SSL](./media/load-balancer-multivip/Figure1.png)

<span data-ttu-id="09bc6-112">Samma offentlig port (443) och trafiken är omdirigerade tooone i hello-exemplet ovan, alla VIP Använd hello eller mer att läsa in belastningsutjämnade virtuella datorer på en unik privat port för hello interna IP-adress hello molntjänst som är värd för alla hello-webbplatser.</span><span class="sxs-lookup"><span data-stu-id="09bc6-112">In hello example above, all VIPs use hello same public port (443) and traffic is redirected tooone or more load balanced VMs on a unique private port for hello internal IP address of hello cloud service hosting all hello websites.</span></span>

> [!NOTE]
> <span data-ttu-id="09bc6-113">En annan situation kräver hello Använd hello flera VIP: er är värd för flera SQL AlwaysOn availability tillgänglighetsgruppslyssnarnas på hello samma uppsättning virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="09bc6-113">Another situation requiring hello use hello multiple VIPs is hosting multiple SQL AlwaysOn availability group listeners on hello same set of Virtual Machines.</span></span>

<span data-ttu-id="09bc6-114">VIP-adresser är dynamiska som standard, vilket innebär att hello faktiska tilldelad IP-adress toohello Molntjänsten kan förändras över tid.</span><span class="sxs-lookup"><span data-stu-id="09bc6-114">VIPs are dynamic by default, which means that hello actual IP address assigned toohello cloud service may change over time.</span></span> <span data-ttu-id="09bc6-115">tooprevent att inträffar, du kan reservera en VIP för din tjänst.</span><span class="sxs-lookup"><span data-stu-id="09bc6-115">tooprevent that from happening, you can reserve a VIP for your service.</span></span> <span data-ttu-id="09bc6-116">toolearn mer information om reserverade virtuella IP-adresser, se [reserverade offentliga IP-Adressen](../virtual-network/virtual-networks-reserved-public-ip.md).</span><span class="sxs-lookup"><span data-stu-id="09bc6-116">toolearn more about reserved VIPs, see [Reserved Public IP](../virtual-network/virtual-networks-reserved-public-ip.md).</span></span>

> [!NOTE]
> <span data-ttu-id="09bc6-117">Se [IP-adress priser](https://azure.microsoft.com/pricing/details/ip-addresses/) information om priser på virtuella IP-adresser och reserverade IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="09bc6-117">Please see [IP Address pricing](https://azure.microsoft.com/pricing/details/ip-addresses/) for information on pricing on VIPs and reserved IPs.</span></span>

<span data-ttu-id="09bc6-118">Du kan använda PowerShell tooverify hello VIP: er som används av dina molntjänster samt lägga till och ta bort VIP-adresser, associera en VIP tooan slutpunkt och konfigurera belastningsutjämning för en specifik VIP.</span><span class="sxs-lookup"><span data-stu-id="09bc6-118">You can use PowerShell tooverify hello VIPs used by your cloud services, as well as add and remove VIPs, associate a VIP tooan endpoint, and configure load balancing on a specific VIP.</span></span>

## <a name="limitations"></a><span data-ttu-id="09bc6-119">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="09bc6-119">Limitations</span></span>

<span data-ttu-id="09bc6-120">Flera VIP funktionen är för tillfället begränsad toohello följande scenarier:</span><span class="sxs-lookup"><span data-stu-id="09bc6-120">At this time, Multi VIP functionality is limited toohello following scenarios:</span></span>

* <span data-ttu-id="09bc6-121">**Endast IaaS**.</span><span class="sxs-lookup"><span data-stu-id="09bc6-121">**IaaS only**.</span></span> <span data-ttu-id="09bc6-122">Du kan bara aktivera Multi-VIP för molntjänster som innehåller virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="09bc6-122">You can only enable Multi VIP for cloud services that contain VMs.</span></span> <span data-ttu-id="09bc6-123">Du kan använda flera VIP i PaaS scenarier med rollinstanser.</span><span class="sxs-lookup"><span data-stu-id="09bc6-123">You cannot use Multi VIP in PaaS scenarios with role instances.</span></span>
* <span data-ttu-id="09bc6-124">**PowerShell endast**.</span><span class="sxs-lookup"><span data-stu-id="09bc6-124">**PowerShell only**.</span></span> <span data-ttu-id="09bc6-125">Du kan endast hantera flera VIP med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="09bc6-125">You can only manage Multi VIP by using PowerShell.</span></span>

<span data-ttu-id="09bc6-126">Dessa begränsningar är tillfälliga och kan ändras när som helst.</span><span class="sxs-lookup"><span data-stu-id="09bc6-126">These limitations are temporary, and may change at any time.</span></span> <span data-ttu-id="09bc6-127">Se till att toorevisit framtida ändringar av den här sidan tooverify.</span><span class="sxs-lookup"><span data-stu-id="09bc6-127">Make sure toorevisit this page tooverify future changes.</span></span>

## <a name="how-tooadd-a-vip-tooa-cloud-service"></a><span data-ttu-id="09bc6-128">Hur tooadd en VIP tooa Molntjänsten</span><span class="sxs-lookup"><span data-stu-id="09bc6-128">How tooadd a VIP tooa cloud service</span></span>
<span data-ttu-id="09bc6-129">tooadd en VIP tooyour tjänsten och köra hello följande PowerShell-kommando:</span><span class="sxs-lookup"><span data-stu-id="09bc6-129">tooadd a VIP tooyour service, run hello following PowerShell command:</span></span>

```powershell
Add-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService
```

<span data-ttu-id="09bc6-130">Detta kommando visar en liknande toohello resultat som följande exempel:</span><span class="sxs-lookup"><span data-stu-id="09bc6-130">This command displays a result similar toohello following sample:</span></span>

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Add-AzureVirtualIP   4bd7b638-d2e7-216f-ba38-5221233d70ce Succeeded

## <a name="how-tooremove-a-vip-from-a-cloud-service"></a><span data-ttu-id="09bc6-131">Hur tooremove en VIP från en molnbaserad tjänst</span><span class="sxs-lookup"><span data-stu-id="09bc6-131">How tooremove a VIP from a cloud service</span></span>
<span data-ttu-id="09bc6-132">tooremove hello VIP läggas till tooyour service i hello-exemplet ovan, kör hello följande PowerShell-kommando:</span><span class="sxs-lookup"><span data-stu-id="09bc6-132">tooremove hello VIP added tooyour service in hello example above, run hello following PowerShell command:</span></span>

```powershell
Remove-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService
```

> [!IMPORTANT]
> <span data-ttu-id="09bc6-133">Du kan bara ta bort en VIP om det har inga slutpunkter kopplade tooit.</span><span class="sxs-lookup"><span data-stu-id="09bc6-133">You can only remove a VIP if it has no endpoints associated tooit.</span></span>


## <a name="how-tooretrieve-vip-information-from-a-cloud-service"></a><span data-ttu-id="09bc6-134">Hur tooretrieve VIP-information från en molnbaserad tjänst</span><span class="sxs-lookup"><span data-stu-id="09bc6-134">How tooretrieve VIP information from a Cloud Service</span></span>
<span data-ttu-id="09bc6-135">tooretrieve hello VIP-adresser som är associerade med en molnbaserad tjänst bör köra hello följande PowerShell-skript:</span><span class="sxs-lookup"><span data-stu-id="09bc6-135">tooretrieve hello VIPs associated with a cloud service, run hello following PowerShell script:</span></span>

```powershell
$deployment = Get-AzureDeployment -ServiceName myService
$deployment.VirtualIPs
```

<span data-ttu-id="09bc6-136">hello-skriptet visar en liknande toohello resultat som följande exempel:</span><span class="sxs-lookup"><span data-stu-id="09bc6-136">hello script displays a result similar toohello following sample:</span></span>

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

<span data-ttu-id="09bc6-137">I det här exemplet har hello Molntjänsten 3 VIP:</span><span class="sxs-lookup"><span data-stu-id="09bc6-137">In this example, hello cloud service has 3 VIPs:</span></span>

* <span data-ttu-id="09bc6-138">**Vip1** är hello standard-VIP vet du som eftersom hello IsDnsProgrammedName värdet för tootrue.</span><span class="sxs-lookup"><span data-stu-id="09bc6-138">**Vip1** is hello default VIP, you know that because hello value for IsDnsProgrammedName is set tootrue.</span></span>
* <span data-ttu-id="09bc6-139">**Vip2** och **Vip3** används inte eftersom de inte har några IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="09bc6-139">**Vip2** and **Vip3** are not used as they do not have any IP addresses.</span></span> <span data-ttu-id="09bc6-140">De ska endast användas om du kopplar en slutpunkt toohello VIP.</span><span class="sxs-lookup"><span data-stu-id="09bc6-140">They will only be used if you associate an endpoint toohello VIP.</span></span>

> [!NOTE]
> <span data-ttu-id="09bc6-141">Din prenumeration kommer bara att debiteras för extra VIP: er när de är kopplade till en slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="09bc6-141">Your subscription will only be charged for extra VIPs once they are associated with an endpoint.</span></span> <span data-ttu-id="09bc6-142">Mer information om priser finns [priser för IP-adressen](https://azure.microsoft.com/pricing/details/ip-addresses/).</span><span class="sxs-lookup"><span data-stu-id="09bc6-142">For more information on pricing, see [IP Address pricing](https://azure.microsoft.com/pricing/details/ip-addresses/).</span></span>

## <a name="how-tooassociate-a-vip-tooan-endpoint"></a><span data-ttu-id="09bc6-143">Hur tooassociate en VIP tooan slutpunkt</span><span class="sxs-lookup"><span data-stu-id="09bc6-143">How tooassociate a VIP tooan endpoint</span></span>

<span data-ttu-id="09bc6-144">tooassociate en VIP på ett moln tooan tjänstslutpunkten, kör hello följande PowerShell-kommando:</span><span class="sxs-lookup"><span data-stu-id="09bc6-144">tooassociate a VIP on a cloud service tooan endpoint, run hello following PowerShell command:</span></span>

```powershell
Get-AzureVM -ServiceName myService -Name myVM1 |
    Add-AzureEndpoint -Name myEndpoint -Protocol tcp -LocalPort 8080 -PublicPort 80 -VirtualIPName Vip2 |
    Update-AzureVM
```

<span data-ttu-id="09bc6-145">hello kommando skapar en slutpunkt som kallas länkade toohello VIP *Vip2* på port *80*, och länkar den toohello virtuella datorn med namnet *myVM1* i en molntjänst med namnet  *myService* med *TCP* på port *8080*.</span><span class="sxs-lookup"><span data-stu-id="09bc6-145">hello command creates an endpoint linked toohello VIP called *Vip2* on port *80*, and links it toohello VM named *myVM1* in a cloud service named *myService* using *TCP* on port *8080*.</span></span>

<span data-ttu-id="09bc6-146">tooverify hello konfiguration, kör hello följande PowerShell-kommando:</span><span class="sxs-lookup"><span data-stu-id="09bc6-146">tooverify hello configuration, run hello following PowerShell command:</span></span>

```powershell
$deployment = Get-AzureDeployment -ServiceName myService
$deployment.VirtualIPs
```

<span data-ttu-id="09bc6-147">hello utdata ser ut ungefär toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="09bc6-147">hello output looks similar toohello following example:</span></span>

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

## <a name="how-tooenable-load-balancing-on-a-specific-vip"></a><span data-ttu-id="09bc6-148">Hur tooenable belastningsutjämning på en specifik VIP</span><span class="sxs-lookup"><span data-stu-id="09bc6-148">How tooenable load balancing on a specific VIP</span></span>

<span data-ttu-id="09bc6-149">Du kan koppla en enskild VIP till flera virtuella datorer för belastningsutjämning syften.</span><span class="sxs-lookup"><span data-stu-id="09bc6-149">You can associate a single VIP with multiple virtual machines for load balancing purposes.</span></span> <span data-ttu-id="09bc6-150">Du till exempel har en molntjänst med namnet *myService*, och två virtuella datorer med namnet *myVM1* och *myVM2*.</span><span class="sxs-lookup"><span data-stu-id="09bc6-150">For example, you have a cloud service named *myService*, and two virtual machines named *myVM1* and *myVM2*.</span></span> <span data-ttu-id="09bc6-151">Och Molntjänsten har flera VIP: er, en av dem med namnet *Vip2*.</span><span class="sxs-lookup"><span data-stu-id="09bc6-151">And your cloud service has multiple VIPs, one of them named *Vip2*.</span></span> <span data-ttu-id="09bc6-152">Om du vill att all trafik tooport tooensure *81* på *Vip2* balanseras mellan *myVM1* och *myVM2* på port *8181* kör hello följande PowerShell-skript:</span><span class="sxs-lookup"><span data-stu-id="09bc6-152">If you want tooensure that all traffic tooport *81* on *Vip2* is balanced between *myVM1* and *myVM2* on port *8181*, run hello following PowerShell script:</span></span>

```powershell
Get-AzureVM -ServiceName myService -Name myVM1 |
    Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2 -DefaultProbe |
    Update-AzureVM

Get-AzureVM -ServiceName myService -Name myVM2 |
    Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2  -DefaultProbe |
    Update-AzureVM
```

<span data-ttu-id="09bc6-153">Du kan också uppdatera din belastningen belastningsutjämnaren toouse en annan VIP.</span><span class="sxs-lookup"><span data-stu-id="09bc6-153">You can also update your load balancer toouse a different VIP.</span></span> <span data-ttu-id="09bc6-154">Om du kör hello PowerShell-kommandot nedan ska du ändra hello belastningsutjämning set toouse en VIP med namnet Vip1:</span><span class="sxs-lookup"><span data-stu-id="09bc6-154">For example, if you run hello PowerShell command below, you will change hello load balancing set toouse a VIP named Vip1:</span></span>

```powershell
Set-AzureLoadBalancedEndpoint -ServiceName myService -LBSetName myLBSet -VirtualIPName Vip1
```

## <a name="next-steps"></a><span data-ttu-id="09bc6-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="09bc6-155">Next Steps</span></span>

[<span data-ttu-id="09bc6-156">Logganalys för belastningsutjämning i Azure</span><span class="sxs-lookup"><span data-stu-id="09bc6-156">Log analytics for Azure Load Balance</span></span>](load-balancer-monitor-log.md)

[<span data-ttu-id="09bc6-157">Internet Internetriktade belastningen översikt över belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="09bc6-157">Internet facing load balancer overview</span></span>](load-balancer-internet-overview.md)

[<span data-ttu-id="09bc6-158">Komma igång med belastningsutjämnaren mot Internet</span><span class="sxs-lookup"><span data-stu-id="09bc6-158">Get started on Internet facing load balancer</span></span>](load-balancer-get-started-internet-arm-ps.md)

[<span data-ttu-id="09bc6-159">Översikt över virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="09bc6-159">Virtual Network Overview</span></span>](../virtual-network/virtual-networks-overview.md)

[<span data-ttu-id="09bc6-160">Reserverad IP REST API: er</span><span class="sxs-lookup"><span data-stu-id="09bc6-160">Reserved IP REST APIs</span></span>](https://msdn.microsoft.com/library/azure/dn722420.aspx)
