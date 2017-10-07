---
title: "aaaCreate ett Azure Internetriktade belastningsutjämnare med IPv6 - PowerShell | Microsoft Docs"
description: "Lär dig hur toocreate Internet-riktade belastningsutjämnaren med IPv6 med hjälp av PowerShell för Resource Manager"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-resource-manager
keywords: "IPv6, azure belastningsutjämnare, dual stack, offentlig IP-adress, inbyggd ipv6, mobil, iot"
ms.assetid: d4c649e3-84ad-4343-8b6a-0e89f0b9e518
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 6ebb108399b070e06dddc33b7a774481eb44d717
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-creating-an-internet-facing-load-balancer-with-ipv6-using-powershell-for-resource-manager"></a>Komma igång med en Internetuppkopplad belastningsutjämnare med IPv6 med hjälp av PowerShell för Resource Manager

> [!div class="op_single_selector"]
> * [PowerShell](load-balancer-ipv6-internet-ps.md)
> * [Azure CLI](load-balancer-ipv6-internet-cli.md)
> * [Mall](load-balancer-ipv6-internet-template.md)

En Azure belastningsutjämnare är en Layer 4-belastningsutjämnare (TCP, UDP). hello belastningsutjämnare ger hög tillgänglighet genom att distribuera inkommande trafik mellan felfri tjänstinstanser i molntjänster eller virtuella datorer i en belastningen belastningsutjämnaren. Azure belastningsutjämnare kan även presentera dessa tjänster på flera portar, flera IP-adresser eller både och.

## <a name="example-deployment-scenario"></a>Exempelscenario för distribution

hello illustrerar följande diagram hello belastningsutjämning lösning som distribueras i den här artikeln.

![Belastningsutjämningsscenario](./media/load-balancer-ipv6-internet-ps/lb-ipv6-scenario.png)

I det här scenariot skapar du hello följande Azure-resurser:

* en Internetriktade belastningsutjämnare med en IPv4 och IPv6-offentlig IP-adress
* två läsa in belastningsutjämning regler toomap hello offentliga VIP toohello privata slutpunkter
* en Tillgänglighetsuppsättning toothat innehåller hello två virtuella datorer
* två virtuella datorer (VM)
* ett virtuellt nätverksgränssnitt för varje virtuell dator med både IPv4 och IPv6-adresser som tilldelats

## <a name="deploying-hello-solution-using-hello-azure-powershell"></a>Distribuera hello-lösning med hjälp av hello Azure PowerShell

hello följande steg visar hur toocreate Internet-riktade belastningsutjämnaren med Azure Resource Manager med PowerShell. Med Azure Resource Manager varje resurs har skapats och konfigurerats individuellt sedan sätta ihop toocreate en resurs.

toodeploy en belastningsutjämnare som du skapar och konfigurerar hello följande objekt:

* IP-konfiguration på klientsidan – innehåller offentliga IP-adresser för inkommande nätverkstrafik.
* Backend-adresspool - innehåller nätverksgränssnitt (NIC) för virtuella datorer hello tooreceive nätverkstrafik från hello belastningsutjämnaren.
* Belastningsutjämning regler - innehåller regler för mappning av en offentlig port på hello belastningen belastningsutjämnaren tooport i hello backend-adresspool.
* Inkommande NAT-regler - innehåller regler för mappning av en offentlig port på hello belastningen belastningsutjämnaren tooa port för en specifik virtuell dator i hello backend-adresspool.
* Avsökningar - innehåller hälsa-avsökningar som används toocheck tillgängligheten för virtuella datorer instanser i hello backend-adresspool.

Mer information finns i [Azure Resource Manager-stöd för belastningsutjämnare](load-balancer-arm.md).

## <a name="set-up-powershell-toouse-resource-manager"></a>Konfigurera PowerShell toouse Resource Manager

Kontrollera att du har hello senaste produktionsversionen av hello Azure Resource Manager-modul för PowerShell.

1. Logga in på Azure

    ```powershell
    Login-AzureRmAccount
    ```

    Ange dina autentiseringsuppgifter när du uppmanas att göra det.

2. Kontrollera hello prenumerationer för hello-konto

    ```powershell
    Get-AzureRmSubscription
    ```

3. Välj vilka av dina Azure-prenumerationer toouse.

    ```powershell
    Select-AzureRmSubscription -SubscriptionId 'GUID of subscription'
    ```

4. Skapa en resursgrupp (hoppa över detta steg om du använder en befintlig resursgrupp)

    ```powershell
    New-AzureRmResourceGroup -Name NRP-RG -location "West US"
    ```

## <a name="create-a-virtual-network-and-a-public-ip-address-for-hello-front-end-ip-pool"></a>Skapa ett virtuellt nätverk och en offentlig IP-adress för hello frontend IP-adresspool

1. Skapa ett virtuellt nätverk med ett undernät.

    ```powershell
    $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
    $vnet = New-AzureRmvirtualNetwork -Name VNet -ResourceGroupName NRP-RG -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet
    ```

2. Skapa Azure offentlig IP-adress (PIP) resurser för hello frontend IP-adresspool.

    ```powershell
    $publicIPv4 = New-AzureRmPublicIpAddress -Name 'pub-ipv4' -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Static -IpAddressVersion IPv4 -DomainNameLabel lbnrpipv4
    $publicIPv6 = New-AzureRmPublicIpAddress -Name 'pub-ipv6' -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Dynamic -IpAddressVersion IPv6 -DomainNameLabel lbnrpipv6
    ```

    > [!IMPORTANT]
    > hello belastningsutjämnare använder hello domänetikett hello offentlig IP-adress som prefix för dess FQDN. I det här exemplet hello FQDN är *lbnrpipv4.westus.cloudapp.azure.com* och *lbnrpipv6.westus.cloudapp.azure.com*.

## <a name="create-a-front-end-ip-configurations-and-a-back-end-address-pool"></a>Skapa en frontend-IP-konfigurationer och en backend-adresspool

1. Skapa frontend-adresskonfiguration som använder hello offentliga IP-adresser som du skapade.

    ```powershell
    $FEIPConfigv4 = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontendv4" -PublicIpAddress $publicIPv4
    $FEIPConfigv6 = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontendv6" -PublicIpAddress $publicIPv6
    ```

2. Skapa backend-adresspool.

    ```powershell
    $backendpoolipv4 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "BackendPoolIPv4"
    $backendpoolipv6 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "BackendPoolIPv6"
    ```

## <a name="create-lb-rules-nat-rules-a-probe-and-a-load-balancer"></a>Skapa LB-regler, NAT-regler, en avsökning och belastningsutjämning

Det här exemplet skapar hello följande objekt:

* en NAT-regel tootranslate all inkommande trafik på port 443 tooport 4443
* en belastningen belastningsutjämnaren regeln toobalance all inkommande trafik på port 80 tooport 80 på hello adresser i hello backend-adresspool.
* en belastningen belastningsutjämnaren regeln tooallow RDP-anslutning toohello virtuella datorer på port 3389.
* en avsökning regeln toocheck hello hälsostatus på en sida med namnet *HealthProbe.aspx* eller en tjänst på port 8080
* en belastningsutjämnare som använder dessa objekt

1. Skapa hello NAT-regler.

    ```powershell
    $inboundNATRule1v4 = New-AzureRmLoadBalancerInboundNatRuleConfig -Name "NicNatRulev4" -FrontendIpConfiguration $FEIPConfigv4 -Protocol TCP -FrontendPort 443 -BackendPort 4443
    $inboundNATRule1v6 = New-AzureRmLoadBalancerInboundNatRuleConfig -Name "NicNatRulev6" -FrontendIpConfiguration $FEIPConfigv6 -Protocol TCP -FrontendPort 443 -BackendPort 4443
    ```

2. Skapa en hälsoavsökning. Det finns två sätt tooconfigure en avsökning:

    HTTP-avsökning

    ```powershell
    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name 'HealthProbe-v4v6' -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2
    ```

    eller TCP-avsökning

    ```powershell
    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name 'HealthProbe-v4v6' -Protocol Tcp -Port 8080 -IntervalInSeconds 15 -ProbeCount 2
    $RDPprobe = New-AzureRmLoadBalancerProbeConfig -Name 'RDPprobe' -Protocol Tcp -Port 3389 -IntervalInSeconds 15 -ProbeCount 2
    ```

    I det här exemplet ska vi ska toouse hello TCP-avsökningar.

3. Skapa en belastningsutjämningsregel.

    ```powershell
    $lbrule1v4 = New-AzureRmLoadBalancerRuleConfig -Name "HTTPv4" -FrontendIpConfiguration $FEIPConfigv4 -BackendAddressPool $backendpoolipv4 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 8080
    $lbrule1v6 = New-AzureRmLoadBalancerRuleConfig -Name "HTTPv6" -FrontendIpConfiguration $FEIPConfigv6 -BackendAddressPool $backendpoolipv6 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 8080
    $RDPrule = New-AzureRmLoadBalancerRuleConfig -Name "RDPrule" -FrontendIpConfiguration $FEIPConfigv4 -BackendAddressPool $backendpoolipv4 -Probe $RDPprobe -Protocol Tcp -FrontendPort 3389 -BackendPort 3389
    ```

4. Skapa hello belastningsutjämnare använder hello som tidigare har skapat objekt.

    ```powershell
    $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName NRP-RG -Name 'myNrpIPv6LB' -Location 'West US' -FrontendIpConfiguration $FEIPConfigv4,$FEIPConfigv6 -InboundNatRule $inboundNATRule1v6,$inboundNATRule1v4 -BackendAddressPool $backendpoolipv4,$backendpoolipv6 -Probe $healthProbe,$RDPprobe -LoadBalancingRule $lbrule1v4,$lbrule1v6,$RDPrule
    ```

## <a name="create-nics-for-hello-back-end-vms"></a>Skapa nätverkskort för hello backend-virtuella datorer

1. Hämta hello virtuella nätverk och virtuella undernät i nätverket, där hello nätverkskort måste toobe skapas.

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -Name VNet -ResourceGroupName NRP-RG
    $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet
    ```

2. Skapa IP-konfigurationer och nätverkskort för hello virtuella datorer.

    ```powershell
    $nic1IPv4 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv4IPConfig" -PrivateIpAddressVersion "IPv4" -Subnet $backendSubnet -LoadBalancerBackendAddressPool $backendpoolipv4 -LoadBalancerInboundNatRule $inboundNATRule1v4
    $nic1IPv6 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv6IPConfig" -PrivateIpAddressVersion "IPv6" -LoadBalancerBackendAddressPool $backendpoolipv6 -LoadBalancerInboundNatRule $inboundNATRule1v6
    $nic1 = New-AzureRmNetworkInterface -Name 'myNrpIPv6Nic0' -IpConfiguration $nic1IPv4,$nic1IPv6 -ResourceGroupName NRP-RG -Location 'West US'

    $nic2IPv4 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv4IPConfig" -PrivateIpAddressVersion "IPv4" -Subnet $backendSubnet -LoadBalancerBackendAddressPool $backendpoolipv4
    $nic2IPv6 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv6IPConfig" -PrivateIpAddressVersion "IPv6" -LoadBalancerBackendAddressPool $backendpoolipv6
    $nic2 = New-AzureRmNetworkInterface -Name 'myNrpIPv6Nic1' -IpConfiguration $nic2IPv4,$nic2IPv6 -ResourceGroupName NRP-RG -Location 'West US'
    ```

## <a name="create-virtual-machines-and-assign-hello-newly-created-nics"></a>Skapa virtuella datorer och tilldela hello nyskapad nätverkskort

Mer information om hur du skapar en virtuell dator finns [skapa och förkonfigurera en virtuell Windows-dator med Resource Manager och Azure PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json)

1. Skapa en Tillgänglighetsuppsättning och lagring

    ```powershell
    New-AzureRmAvailabilitySet -Name 'myNrpIPv6AvSet' -ResourceGroupName NRP-RG -location 'West US'
    $availabilitySet = Get-AzureRmAvailabilitySet -Name 'myNrpIPv6AvSet' -ResourceGroupName NRP-RG
    New-AzureRmStorageAccount -ResourceGroupName NRP-RG -Name 'mynrpipv6stacct' -Location 'West US' -SkuName "Standard_LRS"
    $CreatedStorageAccount = Get-AzureRmStorageAccount -ResourceGroupName NRP-RG -Name 'mynrpipv6stacct'
    ```

2. Skapa varje virtuell dator och tilldela hello tidigare skapade nätverkskort

    ```powershell
    $mySecureCredentials= Get-Credential -Message "Type hello username and password of hello local administrator account."

    $vm1 = New-AzureRmVMConfig -VMName 'myNrpIPv6VM0' -VMSize 'Standard_G1' -AvailabilitySetId $availabilitySet.Id
    $vm1 = Set-AzureRmVMOperatingSystem -VM $vm1 -Windows -ComputerName 'myNrpIPv6VM0' -Credential $mySecureCredentials -ProvisionVMAgent -EnableAutoUpdate
    $vm1 = Set-AzureRmVMSourceImage -VM $vm1 -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm1 = Add-AzureRmVMNetworkInterface -VM $vm1 -Id $nic1.Id -Primary
    $osDisk1Uri = $CreatedStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/myNrpIPv6VM0osdisk.vhd"
    $vm1 = Set-AzureRmVMOSDisk -VM $vm1 -Name 'myNrpIPv6VM0osdisk' -VhdUri $osDisk1Uri -CreateOption FromImage
    New-AzureRmVM -ResourceGroupName NRP-RG -Location 'West US' -VM $vm1

    $vm2 = New-AzureRmVMConfig -VMName 'myNrpIPv6VM1' -VMSize 'Standard_G1' -AvailabilitySetId $availabilitySet.Id
    $vm2 = Set-AzureRmVMOperatingSystem -VM $vm2 -Windows -ComputerName 'myNrpIPv6VM1' -Credential $mySecureCredentials -ProvisionVMAgent -EnableAutoUpdate
    $vm2 = Set-AzureRmVMSourceImage -VM $vm2 -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm2 = Add-AzureRmVMNetworkInterface -VM $vm2 -Id $nic2.Id -Primary
    $osDisk2Uri = $CreatedStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/myNrpIPv6VM1osdisk.vhd"
    $vm2 = Set-AzureRmVMOSDisk -VM $vm2 -Name 'myNrpIPv6VM1osdisk' -VhdUri $osDisk2Uri -CreateOption FromImage
    New-AzureRmVM -ResourceGroupName NRP-RG -Location 'West US' -VM $vm2
    ```

## <a name="next-steps"></a>Nästa steg

[Komma igång med att konfigurera en intern belastningsutjämnare](load-balancer-get-started-ilb-arm-ps.md)

[Konfigurera ett distributionsläge för belastningsutjämnare](load-balancer-distribution-mode.md)

[Konfigurera timeout-inställningar för inaktiv TCP för en belastningsutjämnare](load-balancer-tcp-idle-timeout.md)
