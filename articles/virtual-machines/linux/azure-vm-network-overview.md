---
title: "Azure- och Linux Virtuella nätverks-översikt | Microsoft Docs"
description: "En översikt över Azure och Linux VM-nätverk."
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: vlivech
manager: timlt
editor: 
ms.assetid: b5420e35-0463-4456-9803-6142db150f2e
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/25/2016
ms.author: v-livech
ms.openlocfilehash: 1ff4a0482d6dc6ec0eceaa89ca4b87ba1e2f89a1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-and-linux-vm-network-overview"></a>Azure och översikt över Linux VM-nätverk
## <a name="virtual-networks"></a>Virtuella nätverk
Ett virtuellt Azure-nätverk (VNet) är en representation av ditt eget nätverk i molnet. Det är en logisk isolering av Azure-molnet dedikerad till din prenumeration. Du kan helt styra IP-adressblocken, DNS-inställningarna, säkerhetsprinciperna och routingtabellerna inom det här nätverket. Du kan dessutom ytterligare segmentera ditt VNet i undernät och starta Azure IaaS-virtuella maskiner (VMs) och/eller molntjänster (PaaS rollinstanser). Du kan dessutom ansluta det virtuella nätverket till ditt lokala nätverk med hjälp av något av anslutningsalternativen finns i Azure. I princip kan du expandera ditt nätverk till Azure, med fullständig kontroll över IP-adressblock och de fördelar som Azure på företagsnivå erbjuder.

* [Översikt över virtuella nätverk](../../virtual-network/virtual-networks-overview.md)

Om du vill skapa ett VNet med hjälp av Azure CLI, Gå hit för att följa walk genom.

* [Så här skapar du ett VNet med Azure CLI](../../virtual-network/virtual-networks-create-vnet-arm-cli.md)

## <a name="network-security-groups"></a>Nätverkssäkerhetsgrupper
Nätverkssäkerhetsgruppen (NSG) innehåller en lista över regler för åtkomstkontrollistan (ACL) som tillåter eller nekar nätverkstrafik till dina VM-instanser i ett virtuellt nätverk. NSG:er kan antingen associeras med undernät eller individuella VM-instanser inom det undernätet. När en NSG är associerad med ett undernät, tillämpas ACL-reglerna på alla VM-instanser i det undernätet. Dessutom kan trafik till en enskild VM begränsas ytterligare genom att koppla en NSG direkt till den VM:en.

* [Vad är en nätverkssäkerhetsgrupp (NSG)?](../../virtual-network/virtual-networks-nsg.md)
* [Skapa nätverkssäkerhetsgrupper (NSG) i Azure CLI](../../virtual-network/virtual-networks-create-nsg-arm-cli.md)

## <a name="user-defined-routes"></a>Användardefinierade vägar
När du lägger till virtuella datorer (VM:ar) till ett virtuellt nätverk (VNet) i Azure, märker du att VM:arna kan kommunicera automatiskt med varandra över nätverket. Du behöver inte ange någon gateway, även om VM:arna befinner sig på olika undernät. Samma sak gäller för kommunikation från VM:arna till det offentliga Internet samt till ditt lokala nätverk när det finns en hybridanslutning från Azure till ditt eget datacenter.

* [Vad är användardefinierade vägar och IP-vidarebefordring?](../../virtual-network/virtual-networks-udr-overview.md)
* [Skapa en UDR i Azure CLI](../../virtual-network/virtual-network-create-udr-arm-cli.md)

## <a name="associating-a-fqdn-to-your-linux-vm"></a>Associera en FQDN för Linux-VM
En offentlig IP-resurs för den virtuella datorn skapas automatiskt när du skapar en virtuell dator (VM) i Azure-portalen med Resource Manager-distributionsmodellen. Du kan använda denna IP-adress för att komma åt den virtuella datorn. Även om portalen inte skapar ett fullständigt domännamn eller ett FQDN som standard, kan du lägga till en när den virtuella datorn har skapats.

* [Skapa ett fullständigt kvalificerat domännamn i Azure-portalen](portal-create-fqdn.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="network-interfaces"></a>Nätverksgränssnitt
Ett nätverksgränssnitt (NIC) är gränssnittet mellan en virtuell dator (VM) och det underliggande programvarunätverket. Den här artikeln förklarar vad ett nätverksgränssnitt är och hur det används i Azure Resource Manager-distributionsmodellen.

* [Virtuella nätverksgränssnitt](../../virtual-network/virtual-network-network-interface.md)

## <a name="virtual-nics-and-dns-labeling"></a>Virtuella nätverkskort och DNS-etiketter
Om du har en server som du behöver vara beständig, men att servern behandlas som nötkreatur avbryts datakanalen och distribueras ofta, så ska du använder DNS-etiketter på nätverkskortet för att bevara namnet på VNET.  Med följande genomgången ska du konfigurera ett beständigt namngivna nätverkskort med en statisk IP-adress.

* [Skapa en fullständig Linux-miljö med hjälp av Azure CLI](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="virtual-network-gateways"></a>Virtuella nätverksgatewayer
En virtuell nätverksgateway används för att skicka nätverkstrafik mellan virtuella Azure-nätverk och lokala platser, samt mellan virtuella nätverk i Azure (VNet till VNet). När du konfigurerar en VPN-gateway måste du skapa och konfigurera en gateway för virtuellt nätverk och en gateway för virtuell nätverksanslutning.

* [Om VPN-gateway](../../vpn-gateway/vpn-gateway-about-vpngateways.md)

## <a name="internal-load-balancing"></a>Intern belastningsutjämning
En Azure belastningsutjämnare är en Layer 4-belastningsutjämnare (TCP, UDP). Belastningsutjämnaren ger hög tillgänglighet genom att distribuera inkommande trafik mellan felfria tjänstinstanser i molntjänster eller virtuella datorer i en belastningsutjämningsuppsättning. Azure belastningsutjämnare kan även presentera dessa tjänster på flera portar, flera IP-adresser eller både och.

* [Skapa en intern belastningsutjämnare använder Azure CLI](../../load-balancer/load-balancer-get-started-internet-arm-cli.md)

