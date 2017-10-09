---
title: "aaaCreate ett Azure Internetriktade belastningsutjämnare - PowerShell | Microsoft Docs"
description: "Lär dig hur toocreate en Internetriktade belastningsutjämnare i Resource Manager med hjälp av PowerShell"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-resource-manager
ms.assetid: 8257f548-7019-417f-b15f-d004a1eec826
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: e4e0e5271bc83c23fc62c0910e784c57d2b30065
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started"></a>Skapa en Internetuppkopplad belastningsutjämnare i Resource Manager med hjälp av PowerShell

> [!div class="op_single_selector"]
> * [Portal](../load-balancer/load-balancer-get-started-internet-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-internet-arm-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-internet-arm-cli.md)
> * [Mall](../load-balancer/load-balancer-get-started-internet-arm-template.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Den här artikeln beskriver hello Resource Manager-modellen. Du kan också [Lär dig hur toocreate en Internetriktade belastningsutjämnare med hjälp av hello klassiska distributionsmodellen](load-balancer-get-started-internet-classic-cli.md).

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploying-hello-solution-by-using-azure-powershell"></a>Distribuera hello-lösning med hjälp av Azure PowerShell

hello följande procedurer beskrivs hur toocreate en Internetriktade belastningsutjämnare med hjälp av Azure Resource Manager med PowerShell. Med Azure Resource Manager varje resurs har skapats och konfigurerats individuellt och sedan sätta ihop toocreate belastningsutjämning.

Du måste skapa och konfigurera hello följande objekt toodeploy en belastningsutjämnare:

* IP-konfiguration på klientsidan: innehåller offentliga IP-adresser för inkommande nätverkstrafik.
* Backend-adresspool: innehåller nätverksgränssnitt (NIC) för virtuella datorer hello tooreceive nätverkstrafik från hello belastningsutjämnaren.
* Regler för belastningsutjämning: innehåller regler som mappar en offentlig port på hello belastningen belastningsutjämnaren tooa port i hello backend-adresspool.
* Inkommande NAT-regler: innehåller regler som mappar en offentlig port på hello belastningen belastningsutjämnaren tooa port för en specifik virtuell dator i hello backend-adresspool.
* Avsökningar: innehåller hälsa-avsökningar som används toocheck tillgängligheten för en virtuell datorinstans i hello backend-adresspool.

Mer information finns i [Azure Resource Manager-stöd för belastningsutjämnare](load-balancer-arm.md).

## <a name="set-up-powershell-toouse-resource-manager"></a>Konfigurera PowerShell toouse Resource Manager

Kontrollera att du har hello senaste produktionsversionen av hello Azure Resource Manager-modul för PowerShell:

1. Logga in tooAzure.

    ```powershell
    Login-AzureRmAccount
    ```

    Ange dina autentiseringsuppgifter när du uppmanas att göra det.

2. Kontrollera hello prenumerationer för hello-kontot.

    ```powershell
    Get-AzureRmSubscription
    ```

3. Välj vilka av dina Azure-prenumerationer toouse.

    ```powershell
    Select-AzureRmSubscription -SubscriptionId 'GUID of subscription'
    ```

4. Skapa en resursgrupp. (Hoppa över det här steget om du använder en befintlig resursgrupp.)

    ```powershell
    New-AzureRmResourceGroup -Name NRP-RG -location "West US"
    ```

## <a name="create-a-virtual-network-and-a-public-ip-address-for-hello-front-end-ip-pool"></a>Skapa ett virtuellt nätverk och en offentlig IP-adress för hello frontend IP-adresspool

1. Skapa ett undernät och ett virtuellt nätverk.

    ```powershell
    $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
    New-AzureRmvirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet
    ```

2. Skapa en Azure offentliga IP-adressresurs med namnet **PublicIP**, toobe som används av en frontend IP-adresspool med hello DNS-namnet **loadbalancernrp.westus.cloudapp.azure.com**. hello följande kommando använder hello av statiska allokeringstypen.

    ```powershell
    $publicIP = New-AzureRmPublicIpAddress -Name PublicIp -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Static -DomainNameLabel loadbalancernrp
    ```

   > [!IMPORTANT]
   > hello belastningsutjämnare använder hello domänetikett hello offentlig IP-adress som ett prefix för dess FQDN. Detta skiljer sig från hello klassiska distributionsmodellen, som använder hello molnbaserad tjänst som hello belastningsutjämnaren FQDN.
   > I det här exemplet hello FQDN är **loadbalancernrp.westus.cloudapp.azure.com**.

## <a name="create-a-front-end-ip-pool-and-a-back-end-address-pool"></a>Skapa en IP-adresspool på klientsidan och en backend-adresspool

1. Skapa en frontend IP-pool med namnet **LB-klientdel** som använder hello **PublicIp** resurs.

    ```powershell
    $frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PublicIpAddress $publicIP
    ```

2. Skapa en backend-adresspool med namnet **LB-backend**.

    ```powershell
    $beaddresspool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name LB-backend
    ```

## <a name="create-nat-rules-a-load-balancer-rule-a-probe-and-a-load-balancer"></a>Skapa NAT-regler, en belastningsutjämningsregel, en avsökning och en belastningsutjämnare

Det här exemplet skapar hello följande objekt:

* En NAT-regel tootranslate all inkommande trafik på port 3441 tooport 3389
* En NAT-regel tootranslate all inkommande trafik på port 3442 tooport 3389
* En avsökning regeln toocheck hello hälsostatus på en sida med namnet **HealthProbe.aspx**
* En belastningen belastningsutjämnaren regeln toobalance all inkommande trafik på port 80 tooport 80 på hello-adresser i hello backend-adresspool
* En belastningsutjämnare som använder alla dessa objekt

Följ dessa steg:

1. Skapa hello NAT-regler.

    ```powershell
    $inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name RDP1 -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

    $inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name RDP2 -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389
    ```

2. Skapa en hälsoavsökning. Det finns två sätt tooconfigure en avsökning:

    HTTP-avsökning

    ```powershell
    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HealthProbe -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2
    ```

    TCP-avsökning

    ```powershell
    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HealthProbe -Protocol Tcp -Port 80 -IntervalInSeconds 15 -ProbeCount 2
    ```

3. Skapa en belastningsutjämningsregel.

    ```powershell
    $lbrule = New-AzureRmLoadBalancerRuleConfig -Name HTTP -FrontendIpConfiguration $frontendIP -BackendAddressPool  $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80
    ```

4. Skapa hello belastningsutjämnaren med hello som tidigare har skapat objekt.

    ```powershell
    $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName NRP-RG -Name NRP-LB -Location 'West US' -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe
    ```

## <a name="create-nics"></a>Skapa nätverkskort

Skapa nätverksgränssnitt (eller ändra befintliga) och sedan koppla dem tooNAT regler, regler för inläsning av belastningsutjämnare och avsökningar:

1. Hämta hello virtuella nätverk och undernät för ett virtuellt nätverk där hello nätverkskort måste toobe skapas.

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG
    $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet
    ```

2. Skapa ett nätverkskort med namnet **lb nic1 vara**, och koppla den till hello första NAT-regeln och hello första (och endast) backend-adresspool.

    ```powershell
    $backendnic1= New-AzureRmNetworkInterface -ResourceGroupName NRP-RG -Name lb-nic1-be -Location 'West US' -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]
    ```

3. Skapa ett nätverkskort med namnet **lb nic2 vara**, och koppla den till hello andra NAT-regeln och hello första (och endast) backend-adresspool.

    ```powershell
    $backendnic2= New-AzureRmNetworkInterface -ResourceGroupName NRP-RG -Name lb-nic2-be -Location 'West US' -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]
    ```

4. Kontrollera hello nätverkskort.

        $backendnic1

    Förväntad utdata:

        Name                 : lb-nic1-be
        ResourceGroupName    : NRP-RG
        Location             : westus
        Id                   : /subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
        Etag                 : W/"d448256a-e1df-413a-9103-a137e07276d1"
        ResourceGuid         : 896cac4f-152a-40b9-b079-3e2201a5906e
        ProvisioningState    : Succeeded
        Tags                 :
        VirtualMachine       : null
        IpConfigurations     : [
                            {
                            "Name": "ipconfig1",
                            "Etag": "W/\"d448256a-e1df-413a-9103-a137e07276d1\"",
                            "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/ipconfig1",
                            "PrivateIpAddress": "10.0.2.6",
                            "PrivateIpAllocationMethod": "Static",
                            "Subnet": {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/virtualNetworks/NRPVNet/subnets/LB-Subnet-BE"
                            },
                            "ProvisioningState": "Succeeded",
                            "PrivateIpAddressVersion": "IPv4",
                            "PublicIpAddress": {
                                "Id": null
                            },
                            "LoadBalancerBackendAddressPools": [
                                {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/backendAddressPools/LB-backend"
                                }
                            ],
                            "LoadBalancerInboundNatRules": [
                                {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/inboundNatRules/RDP1"
                                }
                            ],
                            "Primary": true,
                            "ApplicationGatewayBackendAddressPools": []
                            }
                        ]
        DnsSettings          : {
                            "DnsServers": [],
                            "AppliedDnsServers": [],
                            "InternalDomainNameSuffix": "prcwibzcuvie5hnxav0yjks2cd.dx.internal.cloudapp.net"
                        }
        EnableIPForwarding   : False
        NetworkSecurityGroup : null
        Primary              :

5. Använd hello `Add-AzureRmVMNetworkInterface` cmdlet tooassign hello nätverkskort toodifferent virtuella datorer.

## <a name="create-a-virtual-machine"></a>Skapa en virtuell dator

Vägledning om hur du skapar en virtuell dator och tilldelar ett nätverkskort finns i avsnittet om hur du [skapar en virtuell dator i Azure med hjälp av PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).

## <a name="add-hello-network-interface-toohello-load-balancer"></a>Lägga till hello network interface toohello belastningsutjämnare

1. Hämta hello belastningsutjämnaren från Azure.

    Läs in hello belastningsutjämningsresurs i en variabel (om du inte har gjort det ännu). hello variabeln kallas **$lb**. Använd hello samma namn från hello belastningsutjämningsresurs som du skapade tidigare.

    ```powershell
    $lb= get-azurermloadbalancer -name NRP-LB -resourcegroupname NRP-RG
    ```

2. Läsa in hello backend-konfiguration tooa variabeln.

    ```powershell
    $backend=Get-AzureRmLoadBalancerBackendAddressPoolConfig -name LB-backend -LoadBalancer $lb
    ```

3. Läs in hello redan skapat nätverksgränssnitt i en variabel. hello variabelnamnet är **$nic**. hello nätverksgränssnittets namn är hello samma från hello tidigare exemplet.

    ```powershell
    $nic =get-azurermnetworkinterface -name lb-nic1-be -resourcegroupname NRP-RG
    ```

4. Ändra hello backend-konfiguration på hello nätverksgränssnitt.

    ```powershell
    $nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend
    ```

5. Spara hello objektet för nätverksgränssnittet.

    ```powershell
    Set-AzureRmNetworkInterface -NetworkInterface $nic
    ```

    När ett nätverksgränssnitt läggs toohello belastningen belastningsutjämnaren backend-adresspool, startar den tar emot nätverkstrafik baserat på hello belastningsutjämning regler för att belastningsutjämningsresurs.

## <a name="update-an-existing-load-balancer"></a>Uppdatera en befintlig belastningsutjämnare

1. Med hjälp av hello belastningsutjämnaren från Hej tidigare exemplet, tilldela en belastningen belastningsutjämnaren toohello objektvariabel **$slb** med hjälp av `Get-AzureLoadBalancer`.

    ```powershell
    $slb = get-AzureRmLoadBalancer -Name NRP-LB -ResourceGroupName NRP-RG
    ```

2. I följande exempel hello, du lägger till en inkommande NAT-regel--med port 81 i hello frontend-pool och port 8181 för hello backend-poolen--tooan befintliga belastningsutjämnaren.

    ```powershell
    $slb | Add-AzureRmLoadBalancerInboundNatRuleConfig -Name NewRule -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -FrontendPort 81  -BackendPort 8181 -Protocol TCP
    ```

3. Spara hello nya konfigurationen med hjälp av `Set-AzureLoadBalancer`.

    ```powershell
    $slb | Set-AzureRmLoadBalancer
    ```

## <a name="remove-a-load-balancer"></a>Ta bort en belastningsutjämnare

Kommandot hello `Remove-AzureLoadBalancer` toodelete tidigare skapade belastningsutjämning med namnet **NRP-LB** i en resursgrupp kallas **NRP RG**.

```powershell
Remove-AzureRmLoadBalancer -Name NRP-LB -ResourceGroupName NRP-RG
```

> [!NOTE]
> Du kan använda hello valfri växel **-Force** tooavoid hello prompt för borttagning.

## <a name="next-steps"></a>Nästa steg

[Komma igång med att konfigurera en intern belastningsutjämnare](load-balancer-get-started-ilb-arm-ps.md)

[Konfigurera ett distributionsläge för belastningsutjämnare](load-balancer-distribution-mode.md)

[Konfigurera timeout-inställningar för inaktiv TCP för en belastningsutjämnare](load-balancer-tcp-idle-timeout.md)
