---
title: "aaaAzure och översikt över Linux VM-nätverk | Microsoft Docs"
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
ms.openlocfilehash: c3de2dc583c62160e10c0e97e96fef49b9eaffbf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-and-linux-vm-network-overview"></a>Azure och översikt över Linux VM-nätverk
## <a name="virtual-networks"></a>Virtuella nätverk
Azure-nätverk (VNet) är en representation av ditt eget nätverk i hello molnet. Det är en logisk isolering av hello Azure-molnet dedikerad tooyour prenumeration. Du kan helt styra hello IP-Adressblock, DNS-inställningar, säkerhetsprinciper och Routingtabellerna inom det här nätverket. Du kan dessutom ytterligare segmentera ditt VNet i undernät och starta Azure IaaS-virtuella maskiner (VMs) och/eller molntjänster (PaaS rollinstanser). Du kan dessutom ansluta hello virtuellt nätverk tooyour lokalt nätverk med ett hello anslutningsmöjligheter som är tillgängliga i Azure. I princip kan du expandera ditt nätverk tooAzure, med fullständig kontroll över IP-Adressblock med hello förmån på företagsnivå som Azure tillhandahåller.

* [Översikt över virtuella nätverk](../../virtual-network/virtual-networks-overview.md)

toocreate ett VNet med hjälp av hello Azure CLI, gå här toofollow hello genomgång.

* [Hur toocreate ett VNet med hjälp av hello Azure CLI](../../virtual-network/virtual-networks-create-vnet-arm-cli.md)

## <a name="network-security-groups"></a>Nätverkssäkerhetsgrupper
Nätverkssäkerhetsgrupp (NSG) innehåller en lista över regler för åtkomstkontrollistan (ACL) som tillåter eller nekar nätverkstrafik tooyour VM-instanser i ett virtuellt nätverk. NSG:er kan antingen associeras med undernät eller individuella VM-instanser inom det undernätet. När en NSG är associerad med ett undernät, gäller hello ACL-regler tooall hello VM-instanser i det undernätet. Dessutom kan trafik tooan enskild VM begränsas ytterligare genom att koppla en NSG direkt toothat VM.

* [Vad är en nätverkssäkerhetsgrupp (NSG)?](../../virtual-network/virtual-networks-nsg.md)
* [Hur toocreate NSG: er i hello Azure CLI](../../virtual-network/virtual-networks-create-nsg-arm-cli.md)

## <a name="user-defined-routes"></a>Användardefinierade vägar
När du lägger till virtuella datorer (VM) tooa virtuella nätverk (VNet) i Azure, ser du att hello är kan toocommunicate med varandra över hello nätverk automatiskt. Du behöver inte toospecify en gateway, även om hello virtuella datorer i olika undernät. hello samma sak gäller för kommunikation från hello VMs toohello offentliga Internet och även tooyour lokala nätverk när en hybridanslutning från Azure tooyour eget datacenter finns.

* [Vad är användardefinierade vägar och IP-vidarebefordring?](../../virtual-network/virtual-networks-udr-overview.md)
* [Skapa en UDR i hello Azure CLI](../../virtual-network/virtual-network-create-udr-arm-cli.md)

## <a name="associating-a-fqdn-tooyour-linux-vm"></a>Associera en FQDN tooyour Linux VM
När du skapar en virtuell dator (VM) i hello Azure-portalen skapas med hello Resource Manager-distributionsmodellen, en offentlig IP-resurs för hello virtuella datorn automatiskt. Du kan använda den här IP-adressen tooremotely åtkomst hello VM. Även om hello portal inte skapar ett fullständigt domännamn eller ett FQDN som standard, kan du lägga till en när hello VM har skapats.

* [Skapa ett fullständigt kvalificerat domännamn i hello Azure-portalen](portal-create-fqdn.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="network-interfaces"></a>Nätverksgränssnitt
Ett nätverksgränssnitt (NIC) är hello sambandet mellan en virtuell dator (VM) och hello underliggande programvara nätverk. Den här artikeln förklarar ett nätverksgränssnitt är och hur de används i hello Azure Resource Manager-distributionsmodellen.

* [Virtuella nätverksgränssnitt](../../virtual-network/virtual-network-network-interface.md)

## <a name="virtual-nics-and-dns-labeling"></a>Virtuella nätverkskort och DNS-etiketter
Om du har en server som du behöver toobe beständiga, men servern behandlas som nötkreatur och avbryts datakanalen och distribueras ofta vill toouse DNS-etiketter på NIC toopersist hello namn på hello virtuella nätverk.  Till hello efter genomgången ska du konfigurera ett beständigt namngivna nätverkskort med statisk IP.

* [Skapa en fullständig Linux-miljö med hjälp av hello Azure CLI](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="virtual-network-gateways"></a>Virtuella nätverksgatewayer
En virtuell nätverksgateway används toosend trafik mellan virtuella Azure-nätverk och lokala platser och mellan virtuella nätverk i Azure (VNet-till-VNet). När du konfigurerar en VPN-gateway måste du skapa och konfigurera en gateway för virtuellt nätverk och en gateway för virtuell nätverksanslutning.

* [Om VPN-gateway](../../vpn-gateway/vpn-gateway-about-vpngateways.md)

## <a name="internal-load-balancing"></a>Intern belastningsutjämning
En Azure belastningsutjämnare är en Layer 4-belastningsutjämnare (TCP, UDP). hello belastningsutjämnare ger hög tillgänglighet genom att distribuera inkommande trafik mellan felfri tjänstinstanser i molntjänster eller virtuella datorer i en belastningen belastningsutjämnaren. Azure belastningsutjämnare kan även presentera dessa tjänster på flera portar, flera IP-adresser eller både och.

* [Skapa en intern belastningsutjämnare använder hello Azure CLI](../../load-balancer/load-balancer-get-started-internet-arm-cli.md)

