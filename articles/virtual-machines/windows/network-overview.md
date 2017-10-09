---
title: "aaaVirtual nätverk och virtuella Windows-datorer i Azure | Microsoft Docs"
description: "Mer information om nätverk när det gäller toohello grunderna för att skapa virtuella Windows-datorer i Azure."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 5493e9f7-7d45-4e98-be9a-657a53708746
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/17/2017
ms.author: davidmu
ms.openlocfilehash: e28a4782f9f6c69f6101e45dbb560ccd694a1e07
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="virtual-networks-and-windows-virtual-machines-in-azure"></a>Virtuella nätverk och virtuella Windows-datorer i Azure 

När du skapar en virtuell Azure-dator (VM) måste du skapa ett [virtuellt nätverk](../../virtual-network/virtual-networks-overview.md) (VNet) eller använda ett befintligt VNet. Du måste också toodecide hur dina virtuella datorer som är avsedda toobe åt på hello virtuella nätverk. Det är viktigt för[innan du skapar resurser](../../virtual-network/virtual-network-vnet-plan-design-arm.md) och se till att du förstår hello [gränserna för nätverksresurser](../../azure-subscription-service-limits.md#networking-limits).

I följande bild hello, visas virtuella datorer i form av webb- och databasservrar. Varje uppsättning virtuella datorer har tilldelats tooseparate undernät i hello virtuella nätverk.

![Azure-virtuellt nätverk](./media/network-overview/vnetoverview.png)

Du kan antingen skapa ett VNet innan du skapar en virtuell dator eller skapa hello virtuellt nätverk när du skapar en virtuell dator. Du måste antingen skapa ett VNet själv eller så skapas ett för dig när du skapar en virtuell dator.

Skapar du dessa resurser toosupport kommunikation med en virtuell dator:

- Nätverksgränssnitt
- IP-adresser
- Virtuellt nätverk och undernät

Dessutom toothose grundläggande resurser, bör du också beakta dessa valfria resurser:

- Nätverkssäkerhetsgrupper
- Belastningsutjämnare 

## <a name="network-interfaces"></a>Nätverksgränssnitt

En [nätverksgränssnitt (NIC)](../../virtual-network/virtual-network-network-interface.md) är hello sambandet mellan en virtuell dator och ett virtuellt nätverk (VNet). En virtuell dator måste ha minst ett nätverkskort, men kan ha fler än en, beroende på hello storleken på hello VM som du skapar. Lär dig mer om hur många NICs varje virtuell datorstorlek stöder i [Storlekar för virtuella datorer i Azure](sizes.md). 

Om du vill att toocreate en virtuell dator med mer än ett nätverkskort måste du skapa hello VM med minst två.  När den har skapats kan du lägga till ytterligare nätverkskort in toohello antal som stöds av hello VM-storlek, men du kan inte lägga till ytterligare nätverkskort tooa VM endast skapas med en, oavsett hur många nätverkskort stöder hello VM-storlek. 

Om hello VM läggs tooan tillgänglighetsuppsättning, måste alla virtuella datorer i tillgänglighetsuppsättningen hello ha en eller flera nätverkskort. Virtuella datorer med fler än ett nätverkskort är inte krävs toohave hello samma antal nätverkskort, men de måste alla ha minst två.

Varje nätverkskort anslutet tooa VM måste finnas i hello samma plats och prenumerationen som hello VM. Varje nätverkskort måste vara anslutna tooa VNet som finns i hello samma Azure-plats och prenumerationen som hello NIC. När du har skapat ett nätverkskort kan du ändra hello undernät som den är ansluten till, men du kan inte ändra hello virtuella nätverk som den är ansluten till.  Varje nätverkskort anslutet tooa VM har tilldelats en MAC-adress som inte ändras förrän hello VM tas bort.

Den här tabellen visar hello metoder som du kan använda toocreate ett nätverksgränssnitt.

| Metod | Beskrivning |
| ------ | ----------- |
| Azure Portal | När du skapar en virtuell dator i hello Azure-portalen, skapas automatiskt ett nätverksgränssnitt du (du inte kan använda ett nätverkskort som du skapar separat). hello-portalen skapar en virtuell dator med bara ett nätverkskort. Om du vill att toocreate en virtuell dator med mer än ett nätverkskort måste du skapa den med en annan metod. |
| [Azure PowerShell](../../virtual-network/virtual-network-deploy-multinic-arm-ps.md) | Använd [ny AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) med hello **- PublicIpAddressId** parametern tooprovide hello Identiferare hello offentliga IP-adress som du skapade tidigare. |
| [Azure CLI](../../virtual-network/virtual-network-deploy-multinic-arm-cli.md) | tooprovide hello Identiferare hello offentliga IP-adress som du tidigare skapade Använd [az nätverket nic skapa](https://docs.microsoft.com/cli/azure/network/nic#create) med hello **--offentliga IP-adress** parameter. |
| [Mall](../../virtual-network/virtual-network-deploy-multinic-arm-template.md) | Använd [Nätverksgränssnitt i ett virtuellt nätverk med offentliga IP-adresser](https://github.com/Azure/azure-quickstart-templates/tree/master/101-nic-publicip-dns-vnet) som en vägledning för att distribuera ett nätverksgränssnitt med en mall. |

## <a name="ip-addresses"></a>IP-adresser 

Du kan tilldela dessa typer av [IP-adresser](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) tooa NIC i Azure:

- **Offentliga IP-adresser** -används toocommunicate inkommande och utgående (utan network address translation (NAT)) med hello Internet och andra Azure-resurser inte anslutna tooa VNet. Tilldela en offentlig IP-adress tooa är NIC valfritt. Offentliga IP-adresser har en nominell kostnad och det finns ett högsta antal som kan användas per prenumeration.
- **Privata IP-adresser** – används för kommunikation inom ett VNet, ditt lokala nätverk och hello Internet (med NAT). Du måste tilldela minst en privat IP-adress tooa VM. toolearn mer om NAT i Azure, läsa [förstå utgående anslutningar i Azure](../../load-balancer/load-balancer-outbound-connections.md).

Du kan tilldela offentliga IP-adresser tooVMs eller Internetriktade belastningsutjämnare. Du kan tilldela privata IP-adresser tooVMs och interna belastningsutjämnare. Du kan tilldela IP-adresser tooa VM som använder ett nätverksgränssnitt.

Det finns två metoder som en IP-adress tilldelas tooa resurs - dynamiska eller statiska. hello standard allokeringsmetoden är dynamisk, där en IP-adress inte har allokerats när den skapas. I stället tilldelas hello IP-adress när du skapar en virtuell dator eller starta en stoppad virtuell dator. hello IP-adress släpps när du stoppar eller ta bort hello VM. 

tooensure hello IP-adress för hello VM förblir hello samma kan du ange hello allokeringsmetod explicit toostatic. I så fall tilldelas en IP-adress direkt. Släpps när du tar bort hello VM eller ändra dess allokering metoden toodynamic.
    
Den här tabellen visar hello metoder som du kan använda toocreate en IP-adress.

| Metod | Beskrivning |
| ------ | ----------- |
| [Azure Portal](../../virtual-network/virtual-network-deploy-static-pip-arm-portal.md) | Som standard offentliga IP-adresser är dynamiska och hello-adress kopplad toothem kan ändras när hello VM har stoppats eller tagits bort. tooguarantee som hello VM alltid använder Hej samma offentliga IP-adress, skapa en statisk offentlig IP-adress. Hello portal tilldelas som standard en dynamisk privat IP-adress tooa NIC när du skapar en virtuell dator. Du kan ändra den här toostatic efter hello virtuell dator skapas.|
| [Azure PowerShell](../../virtual-network/virtual-network-deploy-static-pip-arm-ps.md) | Du använder [ny AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) med hello **- AllocationMethod** parameter som dynamiska eller statiska. |
| [Azure CLI](../../virtual-network/virtual-network-deploy-static-pip-arm-cli.md) | Du använder [az nätverket offentliga IP-skapa](https://docs.microsoft.com/cli/azure/network/public-ip#create) med hello **--allokeringsmetod** parameter som dynamiska eller statiska. |
| [Mall](../../virtual-network/virtual-network-deploy-static-pip-arm-template.md) | Använd [Nätverksgränssnitt i ett virtuellt nätverk med offentliga IP-adresser](https://github.com/Azure/azure-quickstart-templates/tree/master/101-nic-publicip-dns-vnet) som en vägledning för att distribuera en offentlig IP-adress med en mall. |

När du har skapat en offentlig IP-adress kan du associera den med en virtuell dator genom att tilldela den tooa nätverkskort.

## <a name="virtual-network-and-subnets"></a>Virtuellt nätverk och undernät

Ett undernät är ett intervall med IP-adresser i hello VNet. Du kan dela upp ett VNet i flera undernät av organisations- och säkerhetsskäl. Varje nätverkskort på en virtuell dator är ansluten tooone undernät i ett virtuellt nätverk. Nätverkskort anslutna toosubnets (samma eller olika) inom ett VNet kan kommunicera med varandra utan övrig konfiguration.

När du ställer in ett VNet kan ange du hello topologi, inklusive hello tillgängliga adressutrymmen och undernät. Om hello virtuella nätverk är anslutna toobe tooother Vnet eller lokalt nätverk, måste du välja adressintervall som inte överlappar varandra. hello IP-adresser är privata och inte kan nås från hello Internet, som endast gäller hello-routeable IP-adresser, till exempel 10.0.0.0/8, 172.16.0.0/12 eller 192.168.0.0/16. Nu kan behandlar Azure alla adressintervallet som en del av hello privata VNet IP-adressutrymme som endast kan nås i hello VNet, inom sammankopplade Vnet och från din lokala plats. 

Om du arbetar inom en organisation där någon annan ansvarar för hello interna nätverk, bör du tala toothat person innan du väljer-adressutrymme. Kontrollera att det finns ingen överlappning och meddela hello utrymme som du vill toouse så att de inte försöker toouse hello samma uppsättning IP-adresser. 

Som standard finns inga säkerhetsgräns mellan undernät så att virtuella datorer i var och en av dessa undernät kan prata tooone en annan. Du kan dock konfigurera Nätverkssäkerhetsgrupper (NSG: er) som gör att du toocontrol hello trafik flödet tooand från undernät och tooand från virtuella datorer. 

Den här tabellen visar hello metoder som du kan använda toocreate ett VNet och undernät.   

| Metod | Beskrivning |
| ------ | ----------- |
| [Azure Portal](../../virtual-network/virtual-networks-create-vnet-arm-pportal.md) | Om du låter Azure skapar ett virtuellt nätverk när du skapar en virtuell dator hello namn är en kombination av hello resursgruppens namn som innehåller hello VNet och **- vnet**. hello-adressutrymmet är 10.0.0.0/24 är hello krävs undernätsnamn **standard**, och hello undernät adressintervallet 10.0.0.0/24. |
| [Azure PowerShell](../../virtual-network/virtual-networks-create-vnet-arm-ps.md) | Du använder [ny AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Network/v1.0.13/New-AzureRmVirtualNetworkSubnetConfig) och [New-AzureRmVirtualNetwork](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Network/v1.0.13/New-AzureRmVirtualNetwork) toocreate undernät och virtuella nätverk. Du kan också använda [Lägg till AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig) tooadd ett undernät tooan befintliga VNet. |
| [Azure CLI](../../virtual-network/virtual-networks-create-vnet-arm-cli.md) | hello-undernät och hello VNet har skapats på hello samma tid. Ange en **--undernätsnamn** parameter för[az network vnet skapa](https://docs.microsoft.com/cli/azure/network/vnet#create) med hello undernätsnamn. |
| [Mall](../../virtual-network/virtual-networks-create-vnet-arm-template-click.md) | Hej enklaste sättet toocreate ett VNet och undernät är toodownload en befintlig mall som [virtuellt nätverk med två undernät](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets), och ändra för dina behov. |

## <a name="network-security-groups"></a>Nätverkssäkerhetsgrupper

En [nätverkssäkerhetsgrupp (NSG)](../../virtual-network/virtual-networks-nsg.md) innehåller en lista över regler för åtkomstkontrollistan (ACL) som tillåter eller nekar trafik toosubnets för nätverk, nätverkskort eller båda. NSG: er kan associeras med undernät eller individuella nätverkskort anslutna tooa undernät. När en NSG är associerad med ett undernät, gäller hello ACL-regler tooall hello virtuella datorer i undernätet. Dessutom kan trafik tooan enskilda NIC kan begränsas genom att koppla en NSG direkt tooa nätverkskort.

NSG:er innehåller två regeluppsättningar: inkommande och utgående. hello prioritet för en regel måste vara unika inom varje uppsättning. Varje regel har egenskaperna för protokoll, källa och målportintervall, adressprefix, trafikriktning, prioritet och behörighetstyp. 

Alla NSG:er har en uppsättning standardregler. hello standardreglerna kan inte tas bort, men eftersom de har tilldelats lägst prioritet för hello, de kan åsidosättas med hello regler som du skapar. 

När du kopplar en NSG tooa NIC är hello nätverksåtkomstreglerna i hello NSG tillämpade endast toothat NIC. Om en NSG tillämpad tooa enkel NIC på en virtuell dator i flera nätverkskort, påverkas inte trafik toohello andra nätverkskort. Du kan koppla olika NSG: er tooa NIC (eller VM, beroende på hello distributionsmodell) och hello undernät som ett nätverkskort eller VM är bunden till. Prioritering baserat på hello trafikriktning.

Tänk för[plan](../../virtual-network/virtual-networks-nsg.md#planning) dina NSG: er när du planerar din virtuella datorer och virtuella nätverk.

Den här tabellen visar hello metoder som du kan använda toocreate en nätverkssäkerhetsgrupp.

| Metod | Beskrivning |
| ------ | ----------- |
| [Azure Portal](../../virtual-network/virtual-networks-create-nsg-arm-pportal.md) | När du skapar en virtuell dator i hello Azure-portalen, skapas automatiskt en NSG och skapar associerade toohello NIC hello portalen. hello namnet på hello NSG är en kombination av hello namnet på hello VM och **- nsg**. Den här NSG innehåller en inkommande regel med en prioritet 1000, service set tooRDP, hello protokollet set tooTCP, port angetts too3389 och åtgärden Ange tooAllow. Om du vill tooallow alla andra inkommande trafik toohello VM, måste du lägga till ytterligare regler toohello NSG. |
| [Azure PowerShell](../../virtual-network/virtual-networks-create-nsg-arm-ps.md) | Använd [ny AzureRmNetworkSecurityRuleConfig](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Network/v1.0.13/New-AzureRmNetworkSecurityRuleConfig) och ger information om hello krävs. Använd [ny AzureRmNetworkSecurityGroup](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Network/v1.0.13/New-AzureRmNetworkSecurityGroup) toocreate hello NSG. Använd [Set AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Network/v1.0.13/Set-AzureRmVirtualNetworkSubnetConfig) tooconfigure hello NSG för hello undernät. Använd [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork) tooadd hello NSG toohello VNet. |
| [Azure CLI](../../virtual-network/virtual-networks-create-nsg-arm-cli.md) | Använd [az nätverket nsg skapa](https://docs.microsoft.com/cli/azure/network/nsg#create) tooinitially skapa hello NSG. Använd [az nätverket nsg regeln skapa](https://docs.microsoft.com/cli/azure/network/nsg/rule#create) tooadd toohello NSG-regler. Använd [az network vnet undernät uppdatering](https://docs.microsoft.com/en-us/cli/azure/network/vnet/subnet#update) tooadd hello NSG toohello undernät. |
| [Mall](../../virtual-network/virtual-networks-create-nsg-arm-template.md) | Använd [Skapa en nätverkssäkerhetsgrupp ](https://github.com/Azure/azure-quickstart-templates/tree/master/101-security-group-create) som en vägledning för att distribuera en säkerhetsgrupp i nätverket med hjälp av en mall. |

## <a name="load-balancers"></a>Belastningsutjämnare

[Azure belastningsutjämnare](../../load-balancer/load-balancer-overview.md) ger hög tillgänglighet och nätverksprestanda tooyour program. En belastningsutjämnare kan konfigureras för[balansera inkommande Internettrafik](../../load-balancer/load-balancer-internet-overview.md) tooVMs eller [balansera trafik mellan virtuella datorer i ett VNet](../../load-balancer/load-balancer-internal-overview.md). En belastningsutjämnare kan också utjämna trafiken mellan lokala datorer och virtuella datorer i en mellan lokala nätverk eller externt vidarebefordra trafik tooa specifik VM.

hello belastningsutjämna belastningsutjämnaren maps inkommande och utgående trafik mellan hello offentliga IP-adress och port på hello belastningsutjämnare och hello privat IP-adress och port för hello VM.

Du måste också beakta dessa konfigurationselement när du skapar en belastningsutjämnare:

- **Frontend IP-konfiguration** – En belastningsutjämnare kan innehålla en eller flera frontend IP-adresser, även kallat virtuella IP-adresser (VIPs). IP-adresserna fungera som ingång för hello-trafik.
- **Backend-adresspool** – IP-adresser som är associerade med hello NIC toowhich belastningen distribueras.
- **NAT-regler** -definierar hur inkommande trafik flödar genom hello frontend IP och distribuerade toohello backend-IP.
- **Belastningsutjämningsreglerna** -mappar en given frontend IP och port kombination tooa uppsättning backend-IP-adresser och port-kombination. En enskild belastningsutjämnare kan ha flera regler för belastningsutjämning. Varje regel är en kombination av en frontend-IP och port och backend-IP och port som associeras med virtuella datorer.
- **[Avsökningar](../../load-balancer/load-balancer-custom-probe-overview.md)**  -Övervakare hello hälsotillståndet för virtuella datorer. När en avsökning misslyckas toorespond hello belastningsutjämnaren slutar att skicka nya anslutningar toohello ohälsosamt VM. hello befintliga anslutningar påverkas inte och nya anslutningar skickas toohealthy virtuella datorer.

Den här tabellen visar hello metoder som du kan använda toocreate en Internetriktade belastningsutjämnare.

| Metod | Beskrivning |
| ------ | ----------- |
| Azure Portal | Du skapa för närvarande inte en Internetriktade belastningsutjämnare använder hello Azure-portalen. |
| [Azure PowerShell](../../load-balancer/load-balancer-get-started-internet-arm-ps.md) | tooprovide hello Identiferare hello offentliga IP-adress som du tidigare skapade Använd [ny AzureRmLoadBalancerFrontendIpConfig](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Network/v1.0.13/New-AzureRmLoadBalancerFrontendIpConfig) med hello **- PublicIpAddress** parameter. Använd [ny AzureRmLoadBalancerBackendAddressPoolConfig](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Network/v1.0.13/New-AzureRmLoadBalancerBackendAddressPoolConfig) toocreate hello konfigurationen av hello backend-adresspool. Använd [ny AzureRmLoadBalancerInboundNatRuleConfig](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Network/v1.0.13/New-AzureRmLoadBalancerInboundNatRuleConfig) toocreate inkommande NAT-regler som är associerade med hello frontend IP-konfiguration som du skapade. Använd [ny AzureRmLoadBalancerProbeConfig](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Network/v1.0.13/New-AzureRmLoadBalancerProbeConfig) toocreate hello-avsökningar som du behöver. Använd [ny AzureRmLoadBalancerRuleConfig](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Network/v1.0.13/New-AzureRmLoadBalancerRuleConfig) toocreate hello konfiguration av belastningsutjämning. Använd [ny AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer) toocreate hello belastningsutjämnaren.|
| [Azure CLI](../../load-balancer/load-balancer-get-started-internet-arm-cli.md) | Använd [az nätverket lb skapa](https://docs.microsoft.com/cli/azure/network/lb#create) toocreate hello inledande konfiguration av belastningsutjämning. Använd [az nätverket lb klientdels-IP-skapa](https://docs.microsoft.com/cli/azure/network/lb/frontend-ip#create) tooadd hello offentlig IP-adress som du skapade tidigare. Använd [az lb-nätverksadresspool skapa](https://docs.microsoft.com/cli/azure/network/lb/address-pool#create) tooadd hello konfigurationen av hello backend-adresspool. Använd [az nätverket lb inkommande nat-regeln skapa](https://docs.microsoft.com/cli/azure/network/lb/inbound-nat-rule#create) tooadd NAT-regler. Använd [az nätverket lb regeln skapa](https://docs.microsoft.com/cli/azure/network/lb/rule#create) tooadd hello regler för inläsning av belastningsutjämnaren. Använd [az nätverket lb avsökningen skapa](https://docs.microsoft.com/cli/azure/network/lb/probe#create) tooadd hello-avsökningar. |
| [Mall](../../load-balancer/load-balancer-get-started-internet-arm-template.md) | Använd [2 virtuella datorer i en belastningsutjämnare och konfigurera NAT-regler på hello LB](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-loadbalancer-natrules) som vägledning för att distribuera en belastningsutjämnare med en mall. |
    
Den här tabellen visar hello metoder som du kan använda toocreate en intern belastningsutjämnare.

| Metod | Beskrivning |
| ------ | ----------- |
| Azure Portal | För närvarande kan du skapa en intern belastningsutjämnare använder hello Azure-portalen. |
| [Azure PowerShell](../../load-balancer/load-balancer-get-started-ilb-arm-ps.md) | tooprovide en privat IP-adress i hello undernät, Använd [ny AzureRmLoadBalancerFrontendIpConfig](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Network/v1.0.13/New-AzureRmLoadBalancerFrontendIpConfig) med hello **- PrivateIpAddress** parameter. Använd [ny AzureRmLoadBalancerBackendAddressPoolConfig](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Network/v1.0.13/New-AzureRmLoadBalancerBackendAddressPoolConfig) toocreate hello konfigurationen av hello backend-adresspool. Använd [ny AzureRmLoadBalancerInboundNatRuleConfig](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Network/v1.0.13/New-AzureRmLoadBalancerInboundNatRuleConfig) toocreate inkommande NAT-regler som är associerade med hello frontend IP-konfiguration som du skapade. Använd [ny AzureRmLoadBalancerProbeConfig](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Network/v1.0.13/New-AzureRmLoadBalancerProbeConfig) toocreate hello-avsökningar som du behöver. Använd [ny AzureRmLoadBalancerRuleConfig](https://docs.microsoft.com/powershell/resourcemanager/AzureRM.Network/v1.0.13/New-AzureRmLoadBalancerRuleConfig) toocreate hello konfiguration av belastningsutjämning. Använd [ny AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer) toocreate hello belastningsutjämnaren.|
| [Azure CLI](../../load-balancer/load-balancer-get-started-ilb-arm-cli.md) | Använd hello [az nätverket lb skapa](https://docs.microsoft.com/cli/azure/network/lb#create) kommandot toocreate hello inledande konfiguration av belastningsutjämning. toodefine hello privat IP-adress, Använd [az nätverket lb klientdels-IP-skapa](https://docs.microsoft.com/cli/azure/network/lb/frontend-ip#create) med hello **--privata IP-adress** parameter. Använd [az lb-nätverksadresspool skapa](https://docs.microsoft.com/cli/azure/network/lb/address-pool#create) tooadd hello konfigurationen av hello backend-adresspool. Använd [az nätverket lb inkommande nat-regeln skapa](https://docs.microsoft.com/cli/azure/network/lb/inbound-nat-rule#create) tooadd NAT-regler. Använd [az nätverket lb regeln skapa](https://docs.microsoft.com/cli/azure/network/lb/rule#create) tooadd hello regler för inläsning av belastningsutjämnaren. Använd [az nätverket lb avsökningen skapa](https://docs.microsoft.com/cli/azure/network/lb/probe#create) tooadd hello-avsökningar.|
| [Mall](../../load-balancer/load-balancer-get-started-ilb-arm-template.md) | Använd [2 virtuella datorer i en belastningsutjämnare och konfigurera NAT-regler på hello LB](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-internal-load-balancer) som vägledning för att distribuera en belastningsutjämnare med en mall. |

## <a name="vms"></a>Virtuella datorer

Virtuella datorer kan skapas i hello samma virtuella nätverk och de kan ansluta tooeach andra använder privata IP-adresser. De kan ansluta även om de finns i olika undernät utan hello måste tooconfigure en gateway eller använda offentliga IP-adresser. tooput virtuella datorer i ett virtuellt nätverk, skapar du hello VNet och när du skapar varje virtuell dator kan du tilldela den toohello VNet och undernät. Virtuella datorer får sina nätverksinställningar under distributionen eller starten.  

Virtuella datorer tilldelats en IP-adress när de distribueras. Om du distribuerar flera virtuella datorer i ett VNet eller ett undernät, tilldelas de IP-adresser när de startas. En dynamisk IP-adressen (DIP) är hello intern IP-adress som är kopplad till en virtuell dator. Du kan allokera en statisk DIP tooa VM. Om du allokerar en statisk DIP bör du använda ett specifikt undernät tooavoid av misstag återanvända en statisk DIP för en annan virtuell dator.  

Om du skapar en virtuell dator och senare vill toomigrate det till ett virtuellt nätverk, det är inte en enkel konfigurationsändring. Du måste distribuera hello VM till hello virtuella nätverk. hello enklaste sättet tooredeploy är toodelete hello VM, men inte några diskar kopplade tooit och sedan återskapa hello VM med hjälp av hello ursprungliga diskar i hello virtuella nätverk. 

Den här tabellen visar hello metoder som du kan använda toocreate en virtuell dator i ett virtuellt nätverk.

| Metod | Beskrivning |
| ------ | ----------- |
| [Azure Portal](../virtual-machines-windows-hero-tutorial.md) | Använder hello standardinställningarna för nätverk som redan har nämns toocreate en virtuell dator med ett enda nätverkskort. toocreate en virtuell dator med flera nätverkskort, måste du använda en annan metod. |
| [Azure PowerShell](../virtual-machines-windows-ps-create.md) | Innehåller hello användning av [Lägg till AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface) tooadd hello NIC som du tidigare skapade toohello VM-konfiguration. |
| [Mall](ps-template.md) | Använd [Mycket enkel distribution av virtuella Windows-datorer](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows) som en vägledning för att distribuera en virtuell dator med hjälp av en mall. |

## <a name="next-steps"></a>Nästa steg

- Lär dig hur tooconfigure [användardefinierade vägar och IP-vidarebefordring](../../virtual-network/virtual-networks-udr-overview.md). 
- Lär dig hur tooconfigure [VNet tooVNet anslutningar](../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md).
- Lär dig hur för[felsöka vägar](../../virtual-network/virtual-network-routes-troubleshoot-portal.md).
