---
title: "aaaLoad utjämning på flera IP-konfigurationer i Azure | Microsoft Docs"
description: "Belastningsutjämning mellan primära och sekundära IP-konfigurationer."
services: load-balancer
documentationcenter: na
author: anavinahar
manager: narayan
editor: na
ms.assetid: 244907cd-b275-4494-aaf7-dcfc4d93edfe
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/16/2017
ms.author: annahar
ms.openlocfilehash: fe1cdb317350942ff759229491c2025e98dd24a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="load-balancing-on-multiple-ip-configurations-using-powershell"></a><span data-ttu-id="4c223-103">Belastningsutjämning på flera IP-konfigurationer med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="4c223-103">Load balancing on multiple IP configurations using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="4c223-104">Portal</span><span class="sxs-lookup"><span data-stu-id="4c223-104">Portal</span></span>](load-balancer-multiple-ip.md)
> * [<span data-ttu-id="4c223-105">CLI</span><span class="sxs-lookup"><span data-stu-id="4c223-105">CLI</span></span>](load-balancer-multiple-ip-cli.md)
> * [<span data-ttu-id="4c223-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="4c223-106">PowerShell</span></span>](load-balancer-multiple-ip-powershell.md)

<span data-ttu-id="4c223-107">Den här artikeln beskriver hur toouse Azure belastningsutjämnaren med flera IP-adresser på sekundärt nätverksgränssnitt (NIC).</span><span class="sxs-lookup"><span data-stu-id="4c223-107">This article describes how toouse Azure Load Balancer with multiple IP addresses on a secondary network interface (NIC).</span></span> <span data-ttu-id="4c223-108">I det här scenariot har vi två virtuella datorer som kör Windows med en primär och sekundär NIC.</span><span class="sxs-lookup"><span data-stu-id="4c223-108">For this scenario, we have two VMs running Windows, each with a primary and a secondary NIC.</span></span> <span data-ttu-id="4c223-109">Varje sekundär hello nätverkskort har två IP-konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="4c223-109">Each of hello secondary NICs have two IP configurations.</span></span> <span data-ttu-id="4c223-110">Varje virtuell värd för både webbplatser contoso.com och fabrikam.com. Varje webbplats är bundna tooone hello IP-konfigurationer på hello sekundära nätverkskortet.</span><span class="sxs-lookup"><span data-stu-id="4c223-110">Each VM hosts both websites contoso.com and fabrikam.com. Each website is bound tooone of hello IP configurations on hello secondary NIC.</span></span> <span data-ttu-id="4c223-111">Vi använder Azure belastningsutjämnare tooexpose två klientdelens IP-adresser, en för varje webbplats toodistribute trafik toohello respektive IP-konfiguration för hello webbplats.</span><span class="sxs-lookup"><span data-stu-id="4c223-111">We use Azure Load Balancer tooexpose two frontend IP addresses, one for each website, toodistribute traffic toohello respective IP configuration for hello website.</span></span> <span data-ttu-id="4c223-112">Det här scenariot använder hello samma portnummer över både frontends som båda backend poolen IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="4c223-112">This scenario uses hello same port number across both frontends, as well as both backend pool IP addresses.</span></span>

![LB scenariot bild](./media/load-balancer-multiple-ip/lb-multi-ip.PNG)

## <a name="steps-tooload-balance-on-multiple-ip-configurations"></a><span data-ttu-id="4c223-114">Steg tooload saldot på flera IP-konfigurationer</span><span class="sxs-lookup"><span data-stu-id="4c223-114">Steps tooload balance on multiple IP configurations</span></span>

<span data-ttu-id="4c223-115">Så hello nedan tooachieve hello scenario som beskrivs i den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="4c223-115">Follow hello steps below tooachieve hello scenario outlined in this article:</span></span>

1. <span data-ttu-id="4c223-116">Installera Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4c223-116">Install Azure PowerShell.</span></span> <span data-ttu-id="4c223-117">Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) information om installation hello senaste versionen av Azure PowerShell, välja din prenumeration och loggar in tooyour konto.</span><span class="sxs-lookup"><span data-stu-id="4c223-117">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for information about installing hello latest version of Azure PowerShell, selecting your subscription, and signing in tooyour account.</span></span>
2. <span data-ttu-id="4c223-118">Skapa en resursgrupp med hello följande inställningar:</span><span class="sxs-lookup"><span data-stu-id="4c223-118">Create a resource group using hello following settings:</span></span>

    ```powershell
    $location = "westcentralus".
    $myResourceGroup = "contosofabrikam"
    ```

    <span data-ttu-id="4c223-119">Mer information finns i steg 2 i [skapa en resursgrupp](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4c223-119">For more information, see Step 2 of [Create a Resource Group](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).</span></span>

3. <span data-ttu-id="4c223-120">[Skapa en Tillgänglighetsuppsättning](../virtual-machines/windows/tutorial-availability-sets.md?toc=%2fazure%2fload-balancer%2ftoc.json) toocontain dina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="4c223-120">[Create an Availability Set](../virtual-machines/windows/tutorial-availability-sets.md?toc=%2fazure%2fload-balancer%2ftoc.json) toocontain your VMs.</span></span> <span data-ttu-id="4c223-121">I det här scenariot Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="4c223-121">For this scenario, use hello following command:</span></span>

    ```powershell
    New-AzureRmAvailabilitySet -ResourceGroupName "contosofabrikam" -Name "myAvailset" -Location "West Central US"
    ```

4. <span data-ttu-id="4c223-122">Följ anvisningarna steg 3 till 5 i [skapa en virtuell Windows-dator](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json) artikel tooprepare hello skapandet av en virtuell dator med ett enda nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="4c223-122">Follow instructions steps 3 through 5 in [Create a Windows VM](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json) article tooprepare hello creation of a VM with a single NIC.</span></span> <span data-ttu-id="4c223-123">Köra steg 6.1 och använda hello följande i stället steg 6.2:</span><span class="sxs-lookup"><span data-stu-id="4c223-123">Execute step 6.1, and use hello following instead of step 6.2:</span></span>

    ```powershell
    $availset = Get-AzureRmAvailabilitySet -ResourceGroupName "contosofabrikam" -Name "myAvailset"
    New-AzureRmVMConfig -VMName "VM1" -VMSize "Standard_DS1_v2" -AvailabilitySetId $availset.Id
    ```

    <span data-ttu-id="4c223-124">Slutför [skapa en virtuell Windows-dator](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json) steg 6.3 via 6.8.</span><span class="sxs-lookup"><span data-stu-id="4c223-124">Then complete [Create a Windows VM](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json) steps 6.3 through 6.8.</span></span>

5. <span data-ttu-id="4c223-125">Lägg till en andra IP-konfiguration tooeach av hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="4c223-125">Add a second IP configuration tooeach of hello VMs.</span></span> <span data-ttu-id="4c223-126">Följ anvisningarna för hello i [tilldela flera IP-adresser toovirtual datorer](../virtual-network/virtual-network-multiple-ip-addresses-powershell.md#add) artikel.</span><span class="sxs-lookup"><span data-stu-id="4c223-126">Follow hello instructions in [Assign multiple IP addresses toovirtual machines](../virtual-network/virtual-network-multiple-ip-addresses-powershell.md#add) article.</span></span> <span data-ttu-id="4c223-127">Använd hello följande inställningar:</span><span class="sxs-lookup"><span data-stu-id="4c223-127">Use hello following configuration settings:</span></span>

    ```powershell
    $NicName = "VM1-NIC2"
    $RgName = "contosofabrikam"
    $NicLocation = "West Central US"
    $IPConfigName4 = "VM1-ipconfig2"
    $Subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "mySubnet" -VirtualNetwork $myVnet
    ```

    <span data-ttu-id="4c223-128">Du behöver inte tooassociate hello sekundär IP-konfigurationer med offentliga IP-adresser för hello syftet med den här kursen.</span><span class="sxs-lookup"><span data-stu-id="4c223-128">You do not need tooassociate hello secondary IP configurations with public IPs for hello purpose of this tutorial.</span></span> <span data-ttu-id="4c223-129">Redigera hello kommandot tooremove hello offentliga IP-association del.</span><span class="sxs-lookup"><span data-stu-id="4c223-129">Edit hello command tooremove hello public IP association part.</span></span>

6. <span data-ttu-id="4c223-130">Slutför steg 4 till 6 i den här artikeln igen för VM2.</span><span class="sxs-lookup"><span data-stu-id="4c223-130">Complete steps 4 through 6 of this article again for VM2.</span></span> <span data-ttu-id="4c223-131">Vara säker på att tooreplace hello VM namnet tooVM2 när du gör detta.</span><span class="sxs-lookup"><span data-stu-id="4c223-131">Be sure tooreplace hello VM name tooVM2 when doing this.</span></span> <span data-ttu-id="4c223-132">Observera att du inte behöver toocreate ett virtuellt nätverk för hello andra VM.</span><span class="sxs-lookup"><span data-stu-id="4c223-132">Note that you do not need toocreate a virtual network for hello second VM.</span></span> <span data-ttu-id="4c223-133">Du kan eller inte kan skapa ett nytt undernät baserat på dina användningsfall.</span><span class="sxs-lookup"><span data-stu-id="4c223-133">You may or may not create a new subnet based on your use case.</span></span>

7. <span data-ttu-id="4c223-134">Skapa två offentliga IP-adresser och lagrar dem i hello lämpliga variabler som visas:</span><span class="sxs-lookup"><span data-stu-id="4c223-134">Create two public IP addresses and store them in hello appropriate variables as shown:</span></span>

    ```powershell
    $publicIP1 = New-AzureRmPublicIpAddress -Name PublicIp1 -ResourceGroupName contosofabrikam -Location 'West Central US' -AllocationMethod Dynamic -DomainNameLabel contoso
    $publicIP2 = New-AzureRmPublicIpAddress -Name PublicIp2 -ResourceGroupName contosofabrikam -Location 'West Central US' -AllocationMethod Dynamic -DomainNameLabel fabrikam

    $publicIP1 = Get-AzureRmPublicIpAddress -Name PublicIp1 -ResourceGroupName contosofabrikam
    $publicIP2 = Get-AzureRmPublicIpAddress -Name PublicIp2 -ResourceGroupName contosofabrikam
    ```

8. <span data-ttu-id="4c223-135">Skapa två klientdelens IP-konfigurationer:</span><span class="sxs-lookup"><span data-stu-id="4c223-135">Create two frontend IP configurations:</span></span>

    ```powershell
    $frontendIP1 = New-AzureRmLoadBalancerFrontendIpConfig -Name contosofe -PublicIpAddress $publicIP1
    $frontendIP2 = New-AzureRmLoadBalancerFrontendIpConfig -Name fabrikamfe -PublicIpAddress $publicIP2
    ```

9. <span data-ttu-id="4c223-136">Skapa din serverdelsadresspooler, en avsökning och regler för belastningsutjämning:</span><span class="sxs-lookup"><span data-stu-id="4c223-136">Create your backend address pools, a probe, and your load balancing rules:</span></span>

    ```powershell
    $beaddresspool1 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name contosopool
    $beaddresspool2 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name fabrikampool

    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HTTP -RequestPath 'index.html' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

    $lbrule1 = New-AzureRmLoadBalancerRuleConfig -Name HTTPc -FrontendIpConfiguration $frontendIP1 -BackendAddressPool $beaddresspool1 -Probe $healthprobe -Protocol Tcp -FrontendPort 80 -BackendPort 80
    $lbrule2 = New-AzureRmLoadBalancerRuleConfig -Name HTTPf -FrontendIpConfiguration $frontendIP2 -BackendAddressPool $beaddresspool2 -Probe $healthprobe -Protocol Tcp -FrontendPort 80 -BackendPort 80
    ```

10. <span data-ttu-id="4c223-137">När du har dessa resurser som skapas kan skapa din belastningsutjämnare:</span><span class="sxs-lookup"><span data-stu-id="4c223-137">Once you have these resources created, create your load balancer:</span></span>

    ```powershell
    $mylb = New-AzureRmLoadBalancer -ResourceGroupName contosofabrikam -Name mylb -Location 'West Central US' -FrontendIpConfiguration $frontendIP1 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe
    ```

11. <span data-ttu-id="4c223-138">Lägga till hello andra backend adress pool och klientdelens IP-konfiguration tooyour nyskapad belastningsutjämnare:</span><span class="sxs-lookup"><span data-stu-id="4c223-138">Add hello second backend address pool and frontend IP configuration tooyour newly created load balancer:</span></span>

    ```powershell
    $mylb = Get-AzureRmLoadBalancer -Name "mylb" -ResourceGroupName $myResourceGroup | Add-AzureRmLoadBalancerBackendAddressPoolConfig -Name fabrikampool | Set-AzureRmLoadBalancer

    $mylb | Add-AzureRmLoadBalancerFrontendIpConfig -Name fabrikamfe -PublicIpAddress $publicIP2 | Set-AzureRmLoadBalancer
    
    Add-AzureRmLoadBalancerRuleConfig -Name HTTP -LoadBalancer $mylb -FrontendIpConfiguration $frontendIP2 -BackendAddressPool $beaddresspool2 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80 | Set-AzureRmLoadBalancer
    ```

12. <span data-ttu-id="4c223-139">hello-kommandona nedan hämta hello nätverkskort och Lägg sedan till båda IP-konfigurationer för varje sekundär NIC toohello serverdelsadresspool för hello belastningsutjämnare:</span><span class="sxs-lookup"><span data-stu-id="4c223-139">hello commands below get hello NICs and then add both IP configurations of each secondary NIC toohello backend address pool of hello load balancer:</span></span>

    ```powershell
    $nic1 = Get-AzureRmNetworkInterface -Name "VM1-NIC2" -ResourceGroupName "MyResourcegroup";
    $nic2 = Get-AzureRmNetworkInterface -Name "VM2-NIC2" -ResourceGroupName "MyResourcegroup";

    $nic1.IpConfigurations[0].LoadBalancerBackendAddressPools.Add($mylb.BackendAddressPools[0]);
    $nic1.IpConfigurations[1].LoadBalancerBackendAddressPools.Add($mylb.BackendAddressPools[1]);
    $nic2.IpConfigurations[0].LoadBalancerBackendAddressPools.Add($mylb.BackendAddressPools[0]);
    $nic2.IpConfigurations[1].LoadBalancerBackendAddressPools.Add($mylb.BackendAddressPools[1]);

    $mylb = $mylb | Set-AzureRmLoadBalancer

    $nic1 | Set-AzureRmNetworkInterface
    $nic2 | Set-AzureRmNetworkInterface
    ```

13. <span data-ttu-id="4c223-140">Slutligen måste du konfigurera DNS-resurs registrerar toopoint toohello respektive klientdel IP-adressen för hello belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="4c223-140">Finally, you must configure DNS resource records toopoint toohello respective frontend IP address of hello Load Balancer.</span></span> <span data-ttu-id="4c223-141">Du kan vara värd för dina domäner i Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="4c223-141">You may host your domains in Azure DNS.</span></span> <span data-ttu-id="4c223-142">Mer information om hur du använder Azure DNS med belastningsutjämnaren finns [med hjälp av Azure DNS med andra Azure-tjänster](../dns/dns-for-azure-services.md).</span><span class="sxs-lookup"><span data-stu-id="4c223-142">For more information about using Azure DNS with Load Balancer, see [Using Azure DNS with other Azure services](../dns/dns-for-azure-services.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="4c223-143">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4c223-143">Next steps</span></span>
- <span data-ttu-id="4c223-144">Mer information om hur toocombine belastningsutjämning services i Azure i [med belastningsutjämning tjänster i Azure](../traffic-manager/traffic-manager-load-balancing-azure.md).</span><span class="sxs-lookup"><span data-stu-id="4c223-144">Learn more about how toocombine load balancing services in Azure in [Using load-balancing services in Azure](../traffic-manager/traffic-manager-load-balancing-azure.md).</span></span>
- <span data-ttu-id="4c223-145">Lär dig hur du kan använda olika typer av loggar i Azure toomanage och felsöka belastningsutjämnare i [logga analytics för Azure belastningsutjämnare](../load-balancer/load-balancer-monitor-log.md).</span><span class="sxs-lookup"><span data-stu-id="4c223-145">Learn how you can use different types of logs in Azure toomanage and troubleshoot load balancer in [Log analytics for Azure Load Balancer](../load-balancer/load-balancer-monitor-log.md).</span></span>
