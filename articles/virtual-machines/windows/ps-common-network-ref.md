---
title: "aaaCommon PowerShell-kommandon för Azure Virtual Networks | Microsoft Docs"
description: "Vanliga PowerShell-kommandon tooget du igång med att skapa ett virtuellt nätverk och dess associerade resurser för virtuella datorer."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 56e1a73c-8299-4996-bd03-f74585caa1dc
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: davidmu
ms.openlocfilehash: b46b78f1b7c5a0c5ba917fb48f568d871e5e8789
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="common-powershell-commands-for-azure-virtual-networks"></a>Vanliga PowerShell-kommandon för virtuella Azure-nätverk

Om du vill toocreate en virtuell dator måste toocreate en [virtuellt nätverk](../../virtual-network/virtual-networks-overview.md) eller veta om ett befintligt virtuellt nätverk i vilka hello VM kan läggas till. Vanligtvis när du skapar en virtuell dator, behöver du också tooconsider skapar hello resurser som beskrivs i den här artikeln.

Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) information om installation hello senaste versionen av Azure PowerShell, välja din prenumeration och loggar in tooyour konto.

Några variabler som kan vara användbar för dig om kör flera hello kommandon i den här artikeln:

- $location - hello platsen för hello nätverksresurser. Du kan använda [Get-AzureRmLocation](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermlocation) toofind en [geografisk region](https://azure.microsoft.com/regions/) som passar dig.
- $myResourceGroup - hello namnet på hello resursgruppen där hello nätverksresurser finns.

## <a name="create-network-resources"></a>Skapa nätverksresurser

| Aktivitet | Kommando |
| ---- | ------- |
| Skapa undernät |$Undernät1 = [nya AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) -Name ”mySubnet1” - AddressPrefix XX. X.X.X/XX<BR>$Undernät2 = ny AzureRmVirtualNetworkSubnetConfig-Name ”mySubnet2” - AddressPrefix XX. X.X.X/XX<BR><BR>En typisk nätverk kan ha ett undernät för ett [internetuppkopplad belastningsutjämnaren](../../load-balancer/load-balancer-internet-overview.md) och ett separat undernät för ett [intern belastningsutjämnare](../../load-balancer/load-balancer-internal-overview.md). |
| Skapa ett virtuellt nätverk |$vnet = [New-AzureRmVirtualNetwork](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetwork) -Name ”myVNet” - ResourceGroupName $myResourceGroup-plats $location - AddressPrefix XX. X.X.X/XX-undernätet $Undernät1, $Undernät2 |
| Testa ett unikt domännamn |[Testa AzureRmDnsAvailability](https://docs.microsoft.com/powershell/module/azurerm.network/test-azurermdnsavailability) - DomainNameLabel ”myDNS”-plats $location<BR><BR>Du kan ange ett DNS-domännamn för en [offentliga IP-resurs](../../virtual-network/virtual-network-ip-addresses-overview-arm.md), vilket skapar en mappning för domainname.location.cloudapp.azure.com toohello offentliga IP-adressen i hello Azure-hanterade DNS-servrar. hello namnet får innehålla endast bokstäver, siffror och bindestreck. hello första och sista tecknet måste vara en bokstav eller siffra och hello domännamnet måste vara unikt i Azure-plats. Om **SANT** returneras ditt föreslagna namn är globalt unika. |
| Skapa en offentlig IP-adress |$pip = [ny AzureRmPublicIpAddress](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermpublicipaddress) -Name ”myPublicIp” - ResourceGroupName $myResourceGroup - DomainNameLabel ”myDNS”-plats $location - AllocationMethod dynamiska<BR><BR>hello offentlig IP-adress använder hello domännamn som du tidigare har testat och används av hello klientdel konfiguration av hello belastningsutjämnare. |
| Skapa en IP-konfiguration för klientdel |$frontendIP = [ny AzureRmLoadBalancerFrontendIpConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig) -Name ”myFrontendIP” - PublicIpAddress $pip<BR><BR>hello klientdel konfigurationen innefattar hello offentlig IP-adress som du skapade tidigare för inkommande trafik. |
| Skapa en backend-adresspool |$beAddressPool = [ny AzureRmLoadBalancerBackendAddressPoolConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig) -Name ”myBackendAddressPool”<BR><BR>Innehåller interna adresser för hello serverdelen av hello belastningsutjämnare som nås via ett nätverksgränssnitt. |
| Skapa en avsökning |$healthProbe = [ny AzureRmLoadBalancerProbeConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermloadbalancerprobeconfig) -Name ”myProbe” - RequestPath 'HealthProbe.aspx'-protokollet http-Port 80 - IntervalInSeconds 15 - ProbeCount 2<BR><BR>Innehåller hälsa-avsökningar som används toocheck tillgängligheten för virtuella datorer instanser i hello serverdelen för adresspoolen. |
| Skapa en regel för belastningsutjämning |$lbRule = [ny AzureRmLoadBalancerRuleConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermloadbalancerruleconfig) -namnet HTTP - FrontendIpConfiguration $frontendIP - BackendAddressPool $beAddressPool-avsökning $healthProbe-protokollet Tcp - FrontendPort 80 - BackendPort 80<BR><BR>Innehåller regler som tilldelar en offentlig port på hello belastningsutjämnaren tooa port i hello serverdelen för adresspoolen. |
| Skapa en inkommande NAT-regel |$inboundNATRule = [ny AzureRmLoadBalancerInboundNatRuleConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermloadbalancerinboundnatruleconfig) -Name ”myInboundRule1” - FrontendIpConfiguration $frontendIP-protokollet TCP - FrontendPort 3441 - BackendPort 3389<BR><BR>Innehåller regler för mappning av en offentlig port på hello belastningen belastningsutjämnaren tooa port för en specifik virtuell dator i hello serverdelen för adresspoolen. |
| Skapa en belastningsutjämnare |$loadBalancer = [ny AzureRmLoadBalancer](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermloadbalancer) - ResourceGroupName $myResourceGroup-Name ”myLoadBalancer”-plats $location - FrontendIpConfiguration $frontendIP - InboundNatRule $inboundNATRule - LoadBalancingRule $lbRule - BackendAddressPool $beAddressPool-avsökning $healthProbe |
| Skapa ett nätverksgränssnitt |$nic1 = [ny AzureRmNetworkInterface](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermnetworkinterface) - ResourceGroupName $myResourceGroup-Name ”myNIC” - plats $location - PrivateIpAddress XX. X.X.X-undernätet $Undernät2 - LoadBalancerBackendAddressPool $loadBalancer.BackendAddressPools[0] - LoadBalancerInboundNatRule $loadBalancer.InboundNatRules[0]<BR><BR>Skapa ett nätverksgränssnitt med hello offentlig IP-adress och undernät för virtuellt nätverk som du skapade tidigare. |

## <a name="get-information-about-network-resources"></a>Hämta information om nätverksresurser

| Aktivitet | Kommando |
| ---- | ------- |
| Lista över virtuella nätverk |[Get-AzureRmVirtualNetwork](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermvirtualnetwork) - ResourceGroupName $myResourceGroup<BR><BR>Listar alla hello virtuella nätverk i hello resursgruppen. |
| Hämta information om ett virtuellt nätverk |Get-AzureRmVirtualNetwork-Name ”myVNet” - ResourceGroupName $myResourceGroup |
| Lista över undernät i ett virtuellt nätverk |Get-AzureRmVirtualNetwork-Name ”myVNet” - ResourceGroupName $myResourceGroup &#124; Välj undernät |
| Hämta information om ett undernät |[Get-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig) -Name ”mySubnet1” - VirtualNetwork $vnet<BR><BR>Hämtar information om hello undernät i hello angivna virtuella nätverket. Hej $vnet värdet representerar hello-objektet som returneras av Get-AzureRmVirtualNetwork. |
| Lista över IP-adresser |[Get-AzureRmPublicIpAddress](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermpublicipaddress) - ResourceGroupName $myResourceGroup<BR><BR>Visar hello offentliga IP-adresser i hello resursgruppen. |
| Lista över belastningsutjämnare |[Get-AzureRmLoadBalancer](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermloadbalancer) - ResourceGroupName $myResourceGroup<BR><BR>Listar alla hello belastningsutjämnare i hello resursgruppen. |
| Lista över nätverksgränssnitt |[Get-AzureRmNetworkInterface](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermnetworkinterface) - ResourceGroupName $myResourceGroup<BR><BR>Listar alla hello-nätverksgränssnitt i hello resursgruppen. |
| Hämta information om ett nätverksgränssnitt |Get-AzureRmNetworkInterface-Name ”myNIC” - ResourceGroupName $myResourceGroup<BR><BR>Hämtar information om ett visst nätverksgränssnitt. |
| Få hello IP-konfiguration för ett nätverksgränssnitt |[Get-AzureRmNetworkInterfaceIPConfig](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermnetworkinterfaceipconfig) -Name ”myNICIP” - NetworkInterface $nic<BR><BR>Hämtar information om hello IP-konfiguration av hello angivna nätverksgränssnitt. Hej $nic värdet representerar hello-objektet som returneras av Get-AzureRmNetworkInterface. |

## <a name="manage-network-resources"></a>Hantera nätverksresurser

| Aktivitet | Kommando |
| ---- | ------- |
| Lägg till ett undernät tooa virtuellt nätverk |[Lägg till AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig) - AddressPrefix XX. X.X.X/XX-Name ”mySubnet1” - VirtualNetwork $vnet<BR><BR>Lägger till ett undernät tooan befintligt virtuellt nätverk. Hej $vnet värdet representerar hello-objektet som returneras av Get-AzureRmVirtualNetwork. |
| Ta bort ett virtuellt nätverk |[Ta bort-AzureRmVirtualNetwork](https://docs.microsoft.com/powershell/module/azurerm.network/remove-azurermvirtualnetwork) -Name ”myVNet” - ResourceGroupName $myResourceGroup<BR><BR>Tar bort hello angivna virtuella nätverket från hello resursgruppen. |
| Ta bort ett nätverksgränssnitt |[Ta bort AzureRmNetworkInterface](https://docs.microsoft.com/powershell/module/azurerm.network/remove-azurermnetworkinterface) -Name ”myNIC” - ResourceGroupName $myResourceGroup<BR><BR>Tar bort hello angivna nätverksgränssnitt från hello resursgruppen. |
| Ta bort en belastningsutjämnare |[Ta bort AzureRmLoadBalancer](https://docs.microsoft.com/powershell/module/azurerm.network/remove-azurermloadbalancer) -Name ”myLoadBalancer” - ResourceGroupName $myResourceGroup<BR><BR>Tar bort hello angetts belastningsutjämnaren från hello resursgrupp. |
| Ta bort en offentlig IP-adress |[Ta bort AzureRmPublicIpAddress](https://docs.microsoft.com/powershell/module/azurerm.network/remove-azurermpublicipaddress)-Name ”myIPAddress” - ResourceGroupName $myResourceGroup<BR><BR>Tar bort hello angetts offentlig IP-adress från hello resursgrupp. |

## <a name="next-steps"></a>Nästa steg
* Använd hello nätverksgränssnitt som du precis skapade när du [skapa en virtuell dator](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Lär dig mer om hur du kan [skapa en virtuell dator med flera nätverksgränssnitt](../../virtual-network/virtual-networks-multiple-nics.md).

