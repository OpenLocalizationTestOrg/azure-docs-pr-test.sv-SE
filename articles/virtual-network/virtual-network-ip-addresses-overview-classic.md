---
title: aaaIP adresstyper i Azure (klassisk) | Microsoft Docs
description: "Läs mer om offentliga och privata IP-adresser (klassisk) i Azure."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
tags: azure-service-management
ms.assetid: 2f8664ab-2daf-43fa-bbeb-be9773efc978
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/11/2016
ms.author: jdial
ms.openlocfilehash: 7e09a5ad5b5f2d55329152b5d843de3c4455d1b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="ip-address-types-and-allocation-methods-classic-in-azure"></a>IP-adresstyper och allokeringsmetoder (klassiskt) i Azure
Du kan tilldela IP-adresser tooAzure resurser toocommunicate med andra Azure-resurser, ditt lokala nätverk och hello Internet. Det finns två typer av IP-adresser som du kan använda i Azure: offentliga och privata.

Offentliga IP-adresser används för kommunikation med hello Internet, inklusive Azure offentliga-tjänster.

Privata IP-adresser används för kommunikation inom Azure-nätverk (VNet), en tjänst i molnet och lokala nätverk när du använder en VPN-gateway eller ExpressRoute-kretsen tooextend tooAzure ditt nätverk.

> [!IMPORTANT]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../resource-manager-deployment-model.md).  Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder Resource Manager. Lär dig mer om IP-adresser i Resource Manager genom att läsa hello [IP-adresser](virtual-network-ip-addresses-overview-arm.md) artikel.

## <a name="public-ip-addresses"></a>Offentliga IP-adresser
Offentliga IP-adresser tillåter toocommunicate Azure-resurser med Internet och Azure offentligt riktade som [Azure Redis-Cache](https://azure.microsoft.com/services/cache/), [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/), [SQL-databaser](../sql-database/sql-database-technical-overview.md), och [Azure storage](../storage/common/storage-introduction.md).

En offentlig IP-adress är kopplad till hello följande typer av resurser:

* Molntjänster
* IaaS-virtuella maskiner (VMs)
* PaaS-rollinstanser
* VPN-gateways
* Programgateways

### <a name="allocation-method"></a>Allokeringsmetod
När en offentlig IP-adress måste toobe tilldelade tooan Azure-resurs, är det *dynamiskt* allokeras från en pool med tillgängliga offentliga IP-adress inom hello platsresursen hello skapas. IP-adressen frisläpps när hello resursen har stoppats. Om detta händer när alla hello rollinstanser har stoppats av en tjänst i molnet, vilket kan undvikas genom att använda en *Statiska* (reserverad) IP-adress (se [molntjänster](#Cloud-services)).

> [!NOTE]
> hello lista med IP-adressintervall som offentliga IP-adresser tilldelas tooAzure resurser har publicerats på [Azure Datacenter-IP-intervall](https://www.microsoft.com/download/details.aspx?id=41653).
> 
> 

### <a name="dns-hostname-resolution"></a>Matchning av DNS-värdnamn
När du skapar en molnbaserad tjänst eller ett IaaS-VM måste tooprovide ett moln tjänsten DNS-namn som är unikt över alla resurser i Azure. Detta skapar en mappning i hello Azure-hanterade DNS-servrar för *dnsname*. cloudapp.net toohello offentliga IP-adressen för hello resurs. Till exempel när du skapar en tjänst i molnet med cloud service DNS-namnet **contoso**, hello fullständigt kvalificerat domännamn (FQDN) **contoso.cloudapp.net** löser tooa offentliga IP-adress (VIP) hello-Molntjänsten. Du kan använda den här FQDN toocreate en CNAME-post för domänen som pekar toohello offentliga IP-adressen i Azure.

### <a name="cloud-services"></a>Molntjänster
En molnbaserad tjänst har alltid en offentlig IP-adress som anges tooas en virtuell IP-adress (VIP). Du kan skapa slutpunkter i en cloud service tooassociate olika portar i hello VIP toointernal portar på virtuella datorer och rollinstanser i hello-Molntjänsten. 

En molnbaserad tjänst kan innehålla flera IaaS-VM eller PaaS-rollinstanser, alla exponerade via hello samma cloud service VIP. Du kan också tilldela [flera VIP: er tooa Molntjänsten](../load-balancer/load-balancer-multivip.md), vilket gör att multi-VIP-scenarier som klientmiljön med SSL-baserad webbplatser.

Du kan kontrollera hello offentliga IP-adressen för en tjänst i molnet förblir hello detsamma, även om alla hello rollinstanser har stoppats, med hjälp av en *Statiska* offentliga IP-adress, enligt tooas [reserverade IP: N](virtual-networks-reserved-public-ip.md). Du kan skapa en statisk (reserverad) IP-resurs på en specifik plats och tilldela den tooany Molntjänsten på den här platsen. Du kan inte ange hello IP-adress för hello reserverade IP: N, allokeras från pool med tillgängliga IP-adresser i hello-plats som den har skapats. IP-adressen frisläpps inte förrän du uttryckligen har tagit bort den.

Statisk (reserverad) offentliga IP-adresser används ofta i hello scenarier där en tjänst i molnet:

* kräver brandvägg regler toobe installationen av slutanvändare.
* beror på externa DNS-namnmatchning och kräver en dynamisk IP-adress uppdaterar A-poster.
* förbrukar externa webbtjänster som använder IP-baserade säkerhetsmodell.
* använder SSL-certifikat länkade tooan IP-adress.

> [!NOTE]
> När du skapar en klassisk virtuell dator, en behållare *Molntjänsten* har skapats med Azure som har en virtuell IP-adress (VIP). När du är klar hello skapas via portalen standard RDP eller SSH *endpoint* konfigureras av hello portalen så att du kan ansluta toohello VM via hello cloud service VIP. Det här molnet tjänsten VIP kan reserveras, som ger en reserverad IP-adress tooconnect toohello VM. Du kan öppna ytterligare portar genom att konfigurera fler slutpunkter.
> 
> 

### <a name="iaas-vms-and-paas-role-instances"></a>IaaS-VM och PaaS-rollinstanser
Du kan tilldela en offentlig IP-adressen direkt tooan IaaS [VM](../virtual-machines/virtual-machines-linux-about.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) eller PaaS rollinstans i en molntjänst. Detta är refererad tooas en offentlig IP-adress för instansnivå ([går](virtual-networks-instance-level-public-ip.md)). Den här offentliga IP-adressen kan bara vara dynamisk.

> [!NOTE]
> Detta skiljer sig från hello VIP för hello Molntjänsten, som är en behållare för IaaS-VM eller PaaS-rollinstanser, eftersom en molntjänst kan innehålla flera IaaS-VM eller PaaS-rollinstanser, alla exponerade via hello samma cloud service VIP.
> 
> 

### <a name="vpn-gateways"></a>VPN-gateways
En [VPN-gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) kan vara används tooconnect ett Azure VNet tooother virtuella Azure-nätverk eller lokala nätverk. En VPN-gateway har tilldelats en offentlig IP-adress *dynamiskt*, som aktiverar kommunikation med hello fjärrnätverk.

### <a name="application-gateways"></a>Programgateways
En Azure [Programgateway](../application-gateway/application-gateway-introduction.md) kan användas för Layer7 belastningsutjämning tooroute nätverkstrafik baserat på HTTP. Programgateway har tilldelats en offentlig IP-adress *dynamiskt*, som fungerar som hello belastningsutjämnade VIP.

### <a name="at-a-glance"></a>En snabbtitt
hello tabellen nedan visar varje resurstyp med hello allokering av möjliga metoder (dynamisk/statisk) och möjligheten tooassign flera offentliga IP-adresser.

| Resurs | Dynamisk | Statisk | Flera IP-adresser |
| --- | --- | --- | --- |
| Molntjänsten |Ja |Ja |Ja |
| IaaS-VM eller PaaS rollinstans |Ja |Nej |Nej |
| VPN-gateway |Ja |Nej |Nej |
| Programgateway |Ja |Nej |Nej |

## <a name="private-ip-addresses"></a>Privata IP-adresser
Privata IP-adresser låter toocommunicate Azure-resurser med andra resurser i en molntjänst eller en [virtuellt nätverk](virtual-networks-overview.md)(VNet) eller tooon lokala nätverk (via en VPN-gateway eller ExpressRoute-krets), utan att använda en Internet-nåbar IP-adress.

En privat IP-adress kan tilldelas toohello följande Azure-resurser för i Azure klassiska distributionsmodellen:

* IaaS-VM och PaaS-rollinstanser
* Intern belastningsutjämnare
* Programgateway

### <a name="iaas-vms-and-paas-role-instances"></a>IaaS-VM och PaaS-rollinstanser
Virtuella datorer (VM) som skapats med hello klassiska distributionsmodellen placeras alltid i en molnbaserad tjänst bör liknande tooPaaS rollinstanser. hello beteendet för privata IP-adresser är därför liknande för dessa resurser.

Det är viktigt toonote som en tjänst i molnet kan distribueras på två sätt:

* Som en *fristående* Molntjänsten, om det inte är inom ett virtuellt nätverk.
* Som en del av ett virtuellt nätverk.

#### <a name="allocation-method"></a>Allokeringsmetod
I händelse av en *fristående* Molntjänsten, resurser get en privat IP-adress tilldelas *dynamiskt* från hello Azure-datacenter privata IP-adressintervall. Den kan användas för kommunikation med andra virtuella datorer i hello samma molntjänst. Denna IP-adress kan ändra när hello resurs stoppas och startas.

Om av en tjänst i molnet distribueras inom ett virtuellt nätverk, resurser hämta privata associerade IP-adresser som allokerats från hello adressintervall hello adressundernät (som anges i nätverkskonfigurationen). Det här privata IP-adresser kan användas för kommunikation mellan alla virtuella datorer i hello virtuella nätverk.

Dessutom vid molntjänster inom ett VNet, en privat IP-adress tilldelas *dynamiskt* (med DHCP) som standard. Den kan ändras när hello resurs stoppas och startas. tooensure hello IP-adressen förblir hello samma måste du tooset hello allokeringsmetod för*Statiska*, och ange en giltig IP-adress inom hello motsvarande adressintervall.

Statiska privata IP-adresser används ofta för:

* Virtuella datorer som fungerar som domänkontrollanter eller DNS-servrar.
* Virtuella datorer som kräver brandväggsregler med hjälp av IP-adresser.
* Virtuella datorer som kör tjänster som används av andra appar via en IP-adress.

#### <a name="internal-dns-hostname-resolution"></a>Interna DNS-värdnamn
Alla virtuella datorer i Azure och PaaS-rollinstanser har konfigurerats med [Azure-hanterade DNS-servrar](virtual-networks-name-resolution-for-vms-and-role-instances.md#azure-provided-name-resolution) som standard om du inte uttryckligen anger anpassade DNS-servrar. Dessa DNS-servrar ger intern namnmatchning för virtuella datorer och rollinstanser som finns på hello samma virtuella nätverk eller molnbaserade tjänst.

När du skapar en virtuell dator, läggs en mappning för hello hostname tooits privat IP-adress toohello Azure-hanterade DNS-servrar. Vid en VM med flera nätverkskort hello värdnamn mappas toohello privata IP-adressen för primär hello-nätverkskort. Den här mappningsinformationen är dock begränsad tooresources inom hello samma cloud service eller virtuella nätverk.

I händelse av en *fristående* Molntjänsten, kommer du att kunna tooresolve värdnamn för alla virtuella datorer/rollinstanser i hello samma molntjänst endast. Vid en molnbaserad tjänst inom ett VNet, kommer du att kunna tooresolve värdnamn för alla hello VM: ar/rollinstanser i hello virtuella nätverk.

### <a name="internal-load-balancers-ilb--application-gateways"></a>Interna belastningsutjämnare (ILB) och programgateways
Du kan tilldela en privat IP-adress toohello **klientdelen** konfigurationen av en [Azure interna belastningsutjämnare](../load-balancer/load-balancer-internal-overview.md) (ILB) eller en [Azure Programgateway](../application-gateway/application-gateway-introduction.md). Privata IP-adressen som fungerar som en intern slutpunkt är tillgänglig endast toohello resurser inom dess virtuella nätverk (VNet) och hello fjärrnätverk anslutna toohello VNet. Du kan tilldela antingen en dynamisk eller statisk privat IP-toohello klientdelen adresskonfiguration. Du kan också tilldela privata IP-adresser tooenable multi-vip scenarier med flera.

### <a name="at-a-glance"></a>En snabbtitt
hello tabellen nedan visar varje resurstyp med hello allokering av möjliga metoder (dynamisk/statisk) och möjligheten tooassign flera privata IP-adresser.

| Resurs | Dynamisk | Statisk | Flera IP-adresser |
| --- | --- | --- | --- |
| Virtuell dator (i en *fristående* Molntjänsten) |Ja |Ja |Ja |
| PaaS rollinstansen (i en *fristående* Molntjänsten) |Ja |Nej |Ja |
| Virtuell dator eller PaaS rollinstansen (i ett virtuellt nätverk) |Ja |Ja |Ja |
| Interna belastningsutjämnare klientdel |Ja |Ja |Ja |
| Programmet gateway-klientdel |Ja |Ja |Ja |

## <a name="limits"></a>Begränsningar
hello tabellen nedan visar hello begränsningar har införts på IP-adresser i Azure per prenumeration. Du kan [supporten](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooincrease hello standardgränser in toohello gränsvärden baserat på dina affärsbehov.

|  | Standardgräns | Övre gräns |
| --- | --- | --- |
| Offentliga IP-adresser (dynamiska) |5 |kontakta supporten |
| Reserverade offentliga IP-adresser |20 |kontakta supporten |
| Offentliga VIP per distribution (Molntjänsten) |5 |kontakta supporten |
| Privata VIP (ILB) per distribution (Molntjänsten) |1 |1 |

Kontrollera att du läser hello fullständig uppsättning [begränsar för nätverk](../azure-subscription-service-limits.md#networking-limits) i Azure.

## <a name="pricing"></a>Prissättning
I de flesta fall är offentliga IP-adresser kostnadsfri. Det kostar en nominell toouse ytterligare och/eller statiska offentliga IP-adresser. Kontrollera att du förstår hello [priser strukturen för offentliga IP-adresser](https://azure.microsoft.com/pricing/details/ip-addresses/).

## <a name="differences-between-resource-manager-and-classic-deployments"></a>Skillnader mellan Resource Manager och klassiska distributioner
Nedan visas en jämförelse av IP-adressering funktionerna i hanteraren för filserverresurser och hello klassiska distributionsmodellen.

|  | Resurs | Klassisk | Resource Manager |
| --- | --- | --- | --- |
| **Offentlig IP-adress** |***VM*** |Refererad tooas en går (dynamisk) |Som anges tooas en offentlig IP-adress (dynamisk eller statisk) |
|  ||Tilldelade tooan IaaS VM eller en PaaS-rollinstans |Associerade toohello VM-nätverkskort | |
|  |***Internet belastningsutjämnare*** |Kallas tooas VIP (dynamisk) eller reserverade IP: N (statisk) |Som anges tooas en offentlig IP-adress (dynamisk eller statisk) | |
|  ||Tilldelade tooa Molntjänsten |Associerade toohello läsa in belastningsutjämnings klientdelen config | |
|  | | | |
| **Privata IP-adress** |***VM*** |Refererad tooas en DIP |Kallas tooas en privat IP-adress |
|  ||Tilldelade tooan IaaS VM eller en PaaS-rollinstans |Tilldelade toohello VM-nätverkskort | |
|  |***Intern belastningsutjämnare (ILB)*** |Tilldelade toohello ILB (dynamisk eller statisk) |Tilldelade toohello ILB klientdelen configuration (dynamisk eller statisk) | |

## <a name="next-steps"></a>Nästa steg
* [Distribuera en virtuell dator med en statisk privat IP-adress](virtual-networks-static-private-ip-classic-pportal.md) med hello Azure-portalen.

