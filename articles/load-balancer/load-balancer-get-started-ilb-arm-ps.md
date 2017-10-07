---
title: "aaaCreate en Azure-intern belastningsutjämnare - PowerShell | Microsoft Docs"
description: "Lär dig hur toocreate en intern belastningsutjämnare med PowerShell i Resource Manager"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-resource-manager
ms.assetid: c6c98981-df9d-4dd7-a94b-cc7d1dc99369
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 4b9657c77aa32a142de49ff7871ed6a396b22223
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-internal-load-balancer-using-powershell"></a>Skapa en intern belastningsutjämnare med PowerShell

> [!div class="op_single_selector"]
> * [Azure Portal](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [Mall](../load-balancer/load-balancer-get-started-ilb-arm-template.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!NOTE]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md).  Den här artikeln täcker hello Resource Manager-distributionsmodellen, som Microsoft rekommenderar för de flesta nya distributioner i stället för hello [klassiska distributionsmodellen](load-balancer-get-started-ilb-classic-ps.md).

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

hello följande steg förklarar hur toocreate en intern belastningsutjämnare med Azure Resource Manager med PowerShell. Med Azure Resource Manager hello objekt toocreate en intern belastningsutjämnare konfigureras separat och kombinerade toocreate belastningsutjämning.

Du behöver toocreate och konfigurera hello följande objekt toodeploy en belastningsutjämnare:

* Front end IP-konfiguration – konfigurerar hello privata IP-adressen för inkommande trafik
* Serverdelen för adresspoolen - konfigurerar hello nätverksgränssnitt som tar emot hello belastningsutjämnade trafik som kommer från klientdelen IP-pool
* Regler för belastningsutjämning - käll- och lokala portkonfiguration för hello belastningsutjämnare.
* Avsökningar - konfigurerar hello status hälsoavsökningen för hello virtuella instanser.
* Inkommande NAT-regler – konfigurerar hello port regler toodirectly åtkomst en av hello instanser för virtuella datorer.

Mer information om belastningsutjämningskomponenter med Azure Resource Manager finns i [Azure Resource Manager-stöd för belastningsutjämnare](load-balancer-arm.md).

hello följande steg förklarar hur tooconfigure belastningsutjämning mellan två virtuella datorer.

## <a name="setup-powershell-toouse-resource-manager"></a>Installationsprogrammet för PowerShell toouse Resource Manager

Kontrollera att du har hello senaste produktionsversionen av hello Azure-modulen för PowerShell och har PowerShell installationsprogrammet korrekt tooaccess din Azure-prenumeration.

### <a name="step-1"></a>Steg 1

```powershell
Login-AzureRmAccount
```

### <a name="step-2"></a>Steg 2

Kontrollera hello prenumerationer för hello-konto

```powershell
Get-AzureRmSubscription
```

Du kan ange tooAuthenticate med dina autentiseringsuppgifter.

### <a name="step-3"></a>Steg 3

Välj vilka av dina Azure-prenumerationer toouse.

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="create-resource-group-for-load-balancer"></a>Skapa resursgrupp för belastningsutjämnare

Skapa en ny resursgrupp (hoppa över detta steg om du använder en befintlig resursgrupp)

```powershell
New-AzureRmResourceGroup -Name NRP-RG -location "West US"
```

Azure Resource Manager kräver att alla resursgrupper anger en plats. Detta används som hello standardplatsen för resurser i resursgruppen. Kontrollera att alla kommandon som använder en belastningsutjämnare toocreate hello samma resursgrupp.

I hello skapa exemplet ovan vi en resursgrupp med namnet ”NRP-RG” och plats ”West US”.

## <a name="create-virtual-network-and-a-private-ip-address-for-front-end-ip-pool"></a>Skapa virtuella nätverk och en privat IP-adress för IP-pooler på klientsidan

Skapar ett undernät för virtuellt nätverk för hello och tilldelar toovariable $backendSubnet

```powershell
$backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
```

Skapa ett virtuellt nätverk:

```powershell
$vnet= New-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet
```

Skapar hello virtuella nätverk och lägger till hello lb undernät vara toohello virtuella undernätverk NRPVNet och tilldelar toovariable $vnet

## <a name="create-front-end-ip-pool-and-backend-address-pool"></a>Skapa en IP-pool på klientsidan och en serverdelsadresspool

Ställa in en IP-adresspool för klientdelen för inkommande hälsningspaket att läsa in belastningsutjämning nätverkstrafik och backend-adresspoolen tooreceive hello belastningsutjämnade trafik.

### <a name="step-1"></a>Steg 1

Skapa en IP-adresspool för klientdelen som använder hello privat IP-adress 10.0.2.5 för hello undernät 10.0.2.0/24 som ska vara hello inkommande trafik nätverksslutpunkten.

```powershell
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PrivateIpAddress 10.0.2.5 -SubnetId $vnet.subnets[0].Id
```

### <a name="step-2"></a>Steg 2

Ställ in en backend-adresspool används tooreceive inkommande trafik från klientdelen IP-adresspool:

```powershell
$beaddresspool= New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "LB-backend"
```

## <a name="create-lb-rules-nat-rules-probe-and-load-balancer"></a>Skapa LB-regler, NAT-regler, avsökning och belastningsutjämnare

När du har skapat hello klientdelen IP-adresspool och hello serverdelen för adresspoolen, behöver du toocreate hello regler som kommer att tillhöra toohello belastningsutjämningsresurs:

### <a name="step-1"></a>Steg 1

```powershell
$inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP1" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

$inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP2" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389

$healthProbe = New-AzureRmLoadBalancerProbeConfig -Name "HealthProbe" -RequestPath "HealthProbe.aspx" -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

$lbrule = New-AzureRmLoadBalancerRuleConfig -Name "HTTP" -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80
```

hello-exemplet ovan skapar hello följande objekt:

* NAT-regel som all inkommande trafik tooport 3441 går tooport 3389.
* en annan NAT-regel som all inkommande trafik tooport 3442 går tooport 3389.
* en regel för belastningsutjämnare som läser in balansera all inkommande trafik på port 80 av offentlig port 80 toolocal i hello backend-adresspool.
* en avsökning regel som kontrollerar hello hälsostatus för sökvägen ”HealthProbe.aspx”

### <a name="step-2"></a>Steg 2

Skapa hello belastningsutjämnaren om du lägger till alla objekt (NAT-regler, regler för inläsning av belastningsutjämnare, avsökningen konfigurationer) tillsammans:

```powershell
$NRPLB = New-AzureRmLoadBalancer -ResourceGroupName "NRP-RG" -Name "NRP-LB" -Location "West US" -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe
```

## <a name="create-network-interfaces"></a>Skapa nätverksgränssnitt

När du har skapat hello intern belastningsutjämnare, måste du definiera vilka nätverksgränssnitt får hello inkommande belastningsutjämnade trafik, NAT-regler och avsökning. hello-nätverksgränssnitt i det här fallet konfigureras individuellt och kan tilldelas tooa virtuell dator vid ett senare tillfälle.

### <a name="step-1"></a>Steg 1

Hämta hello resurs virtuella nätverk och undernät toocreate nätverksgränssnitt:

```powershell
$vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG

$backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet
```

Det här steget skapar ett nätverksgränssnitt vilket tillhör toohello belastningen belastningsutjämnarens serverdelspool och associera hello första NAT-regeln för RDP för det här nätverksgränssnittet:

```powershell
$backendnic1= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic1-be -Location "West US" -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]
```

### <a name="step-2"></a>Steg 2

Skapa ett andra nätverksgränssnitt som kallas LB-Nic2-BE:

Det här steget skapar ett andra nätverksgränssnitt, tilldela toohello samma belastningsutjämnare tillbaka avslutas poolen och associera hello andra NAT-regel skapas för RDP:

```powershell
$backendnic2= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic2-be -Location "West US" -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]
```

hello slutresultatet visar hello följande:

    $backendnic1

Förväntad utdata:

    Name                 : lb-nic1-be
    ResourceGroupName    : NRP-RG
    Location             : westus
    Id                   : /subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
    Etag                 : W/"d448256a-e1df-413a-9103-a137e07276d1"
    ProvisioningState    : Succeeded
    Tags                 :
    VirtualMachine       : null
    IpConfigurations     : [
                         {
                           "PrivateIpAddress": "10.0.2.6",
                           "PrivateIpAllocationMethod": "Static",
                           "Subnet": {
                             "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/virtualNetworks/NRPVNet/subnets/LB-Subnet-BE"
                           },
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
                           "ProvisioningState": "Succeeded",
                           "Name": "ipconfig1",
                           "Etag": "W/\"d448256a-e1df-413a-9103-a137e07276d1\"",
                           "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/ipconfig1"
                         }
                       ]
    DnsSettings          : {
                         "DnsServers": [],
                         "AppliedDnsServers": []
                       }
    AppliedDnsSettings   :
    NetworkSecurityGroup : null
    Primary              : False



### <a name="step-3"></a>Steg 3

Använda hello kommandot Lägg till AzureRmVMNetworkInterface tooassign hello NIC tooa virtuell dator.

Du kan hitta hello steg-för-steg-instruktioner toocreate en virtuell dator och tilldela tooa NIC följande hello-dokumentationen: [skapa en virtuell Azure-dator med hjälp av PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).

## <a name="add-hello-network-interface"></a>Lägga till hello nätverksgränssnitt

Du kan lägga till hello nätverksgränssnitt med hello följande steg om du redan har en virtuell dator som skapats:

### <a name="step-1"></a>Steg 1

Läs in hello belastningsutjämningsresurs i en variabel (om du inte har gjort det ännu). hello-variabel som används är kallas $lb och Använd hello samma namn från hello belastningsutjämningsresurs skapade ovan.

```powershell
$lb = Get-AzureRmLoadBalancer –name NRP-LB -resourcegroupname NRP-RG
```

### <a name="step-2"></a>Steg 2

Läsa in hello backend configuration tooa variabeln.

```powershell
$backend = Get-AzureRmLoadBalancerBackendAddressPoolConfig -name backendpool1 -LoadBalancer $lb
```

### <a name="step-3"></a>Steg 3

Läs in hello redan skapat nätverksgränssnitt i en variabel. hello variabelnamnet som används är $nic. hello nätverksgränssnittets namn används hello samma från hello-exemplet ovan.

```powershell
$nic = Get-AzureRmNetworkInterface –name lb-nic1-be -resourcegroupname NRP-RG
```

### <a name="step-4"></a>Steg 4

Ändra hello backend-konfiguration på hello nätverksgränssnitt.

```powershell
$nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend
```

### <a name="step-5"></a>Steg 5

Spara hello objektet för nätverksgränssnittet.

```powershell
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

När ett nätverksgränssnitt läggs toohello belastningsutjämnarens serverdelspool, startar den tar emot nätverkstrafik baserat på hello regler för att belastningsutjämningsresurs för belastningsutjämning.

## <a name="update-an-existing-load-balancer"></a>Uppdatera en befintlig belastningsutjämnare

### <a name="step-1"></a>Steg 1
Använda hello belastningsutjämnare från hello-exemplet ovan, tilldela load balancer objektet toovariable $slb med hjälp av Get-AzureRmLoadBalancer

```powershell
$slb = Get-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG
```

### <a name="step-2"></a>Steg 2

I följande exempel hello, lägger du till en ny inkommande NAT-regel som använder port 81 i hello klientdelen och port 8181 för hello tillbaka sluta poolen tooan befintliga belastningsutjämnare

```powershell
$slb | Add-AzureRmLoadBalancerInboundNatRuleConfig -Name NewRule -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -FrontendPort 81  -BackendPort 8181 -Protocol Tcp
```

### <a name="step-3"></a>Steg 3

Spara hello nya konfigurationen med hjälp av Set-AzureLoadBalancer

```powershell
$slb | Set-AzureRmLoadBalancer
```

## <a name="remove-a-load-balancer"></a>Ta bort en belastningsutjämnare

Använd hello kommandot Remove-AzureRmLoadBalancer toodelete tidigare skapade belastningsutjämning med namnet ”NRP-LB” i en resursgrupp kallas ”NRP-RG”

```powershell
Remove-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG
```

> [!NOTE]
> Du kan använda valfri hello växla - Force tooavoid hello prompt för borttagning.

## <a name="next-steps"></a>Nästa steg

[Konfigurera ett distributionsläge för belastningsutjämnare](load-balancer-distribution-mode.md)

[Konfigurera timeout-inställningar för inaktiv TCP för en belastningsutjämnare](load-balancer-tcp-idle-timeout.md)
