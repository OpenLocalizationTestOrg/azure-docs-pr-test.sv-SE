---
title: aaaIP adresstyper i Azure | Microsoft Docs
description: "Läs mer om offentliga och privata IP-adresser i Azure."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
tags: azure-resource-manager
ms.assetid: 610b911c-f358-4cfe-ad82-8b61b87c3b7e
ms.service: virtual-network
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/27/2016
ms.author: jdial
ms.openlocfilehash: 402d3707c00f0b3bf3ef1febd5ade66223da74bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="ip-address-types-and-allocation-methods-in-azure"></a>IP-adresstyper och allokeringsmetoder i Azure
Du kan tilldela IP-adresser tooAzure resurser toocommunicate med andra Azure-resurser, ditt lokala nätverk och hello Internet. Det finns två typer av IP-adresser som du kan använda i Azure:

* **Offentliga IP-adresser**: används för kommunikation med hello Internet, inklusive Azure offentliga-tjänster
* **Privata IP-adresser**: används för kommunikation inom Azure-nätverk (VNet) och ditt lokala nätverk när du använder en VPN-gateway eller ExpressRoute-kretsen tooextend tooAzure ditt nätverk.

> [!NOTE]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../resource-manager-deployment-model.md).  Den här artikeln täcker hello Resource Manager-distributionsmodellen, som Microsoft rekommenderar för de flesta nya distributioner i stället för hello [klassiska distributionsmodellen](virtual-network-ip-addresses-overview-classic.md).
> 

Om du är bekant med hello klassiska distributionsmodellen Kontrollera hello [skillnader i IP-adresser mellan klassisk och Resource Manager](virtual-network-ip-addresses-overview-classic.md#differences-between-resource-manager-and-classic-deployments).

## <a name="public-ip-addresses"></a>Offentliga IP-adresser
Offentliga IP-adresser tillåter toocommunicate Azure-resurser med Internet och Azure offentligt riktade som [Azure Redis-Cache](https://azure.microsoft.com/services/cache/), [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/), [SQL-databaser](../sql-database/sql-database-technical-overview.md), och [Azure storage](../storage/common/storage-introduction.md).

I Azure Resource Manager är en [offentlig IP-adress](resource-groups-networking.md#public-ip-address) en resurs som har sina egna egenskaper. Du kan associera en offentlig IP-adressresurs med någon av hello följande resurser:

* Virtuella datorer (VM)
* Internetuppkopplade belastningsutjämnare
* VPN-gateways
* Programgateways

### <a name="allocation-method"></a>Allokeringsmetod
Det finns två metoder som en IP-adress tilldelas tooa *offentliga* IP-resurs - *dynamiska* eller *statiska*. hello standard allokeringsmetoden är *dynamiska*, där en IP-adress är **inte** allokerade vid hello tidpunkt då skapades. I stället tilldelas hello offentliga IP-adress när du startar (eller skapa) hello associerade resurser (t.ex. en virtuell dator eller läsa in belastningsutjämning). hello IP-adress släpps när du stoppar (eller ta bort) hello resurs. Detta medför hello IP-adress toochange när du stoppar och startar en resurs.

tooensure hello IP-adress för hello associerad resurs förblir hello samma kan du ange hello allokeringsmetod explicit för*Statiska*. I så fall tilldelas en IP-adress direkt. Frigörs när du tar bort hello resurs eller ändra dess allokeringsmetod för*dynamiska*.

> [!NOTE]
> Även om du ställer in hello allokeringsmetod för*Statiska*, du kan inte ange hello faktiska IP-adress som tilldelats toohello *offentliga IP-resurs*. I stället hämtar den allokerade från en pool med tillgängliga IP-adresser i hello Azure-plats hello resursen skapas i.
>

Statiska offentliga IP-adresser används ofta i hello följande scenarier:

* Slutanvändare behöver tooupdate brandväggen regler toocommunicate med Azure-resurser.
* DNS-namnmatchning, där en ändring i IP-adressen kräver uppdatering av A-poster.
* Dina Azure-resurser kommunicerar med andra appar eller tjänster som använder en IP-adressbaserad säkerhetsmodell.
* Du kan använda SSL-certifikat länkade tooan IP-adress.

> [!NOTE]
> hello lista med IP-adressintervall som offentliga IP-adresser (dynamisk/statisk) tilldelas tooAzure resurser har publicerats på [Azure Datacenter-IP-intervall](https://www.microsoft.com/download/details.aspx?id=41653).
>

### <a name="dns-hostname-resolution"></a>Matchning av DNS-värdnamn
Du kan ange ett DNS-domännamnet för en offentlig IP-resurs, vilket skapar en mappning för *domainnamelabel*. *plats*. cloudapp.azure.com toohello offentliga IP-adressen i hello Azure-hanterade DNS-servrar. Till exempel om du skapar en offentlig IP-resurs med **contoso** som en *domainnamelabel* i hello **västra USA** Azure *plats*, hello fullständigt kvalificerat domännamn (FQDN) **contoso.westus.cloudapp.azure.com** löser toohello offentliga IP-adress hello resurs. Du kan använda den här FQDN toocreate en CNAME-post för domänen som pekar toohello offentliga IP-adressen i Azure.

> [!IMPORTANT]
> Varje domännamnsetikett som skapas måste vara unik inom dess Azure-plats.  
>

### <a name="virtual-machines"></a>Virtuella datorer
Du kan associera en offentlig IP-adress med en [Windows](../virtual-machines/windows/overview.md) eller [Linux](../virtual-machines/virtual-machines-linux-about.md) VM genom att tilldela den tooits **nätverksgränssnittet**. Hello gäller en virtuell dator med flera nätverksgränssnitt, du kan tilldela toohello *primära* endast nätverksgränssnitt. Du kan tilldela ett dynamiskt eller en statisk offentlig IP-adress tooa VM.

### <a name="internet-facing-load-balancers"></a>Internetuppkopplade belastningsutjämnare
Du kan associera en offentlig IP-adress med en [Azure belastningsutjämnare](../load-balancer/load-balancer-overview.md), genom att tilldela den toohello belastningsutjämnare **klientdel** konfiguration. Den här offentliga IP-adressen fungerar som en belastningsutjämnad virtuella IP-adress (VIP). Du kan tilldela en dynamisk eller statisk offentlig IP-adress tooa belastningsutjämning frontend. Du kan också tilldela flera offentliga IP-adresser tooa belastningsutjämnaren frontend, vilket gör att [multi-VIP](../load-balancer/load-balancer-multivip.md) scenarier som en miljö med flera innehavare med SSL-baserad webbplatser.

### <a name="vpn-gateways"></a>VPN-gateways
[Azure VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) är används tooconnect ett virtuellt Azure-nätverk (VNet) tooother Azure Vnet eller tooan lokalt nätverk. Du behöver tooassign en offentlig IP-adress tooits **IP-konfiguration** tooenable den toocommunicate med hello fjärrnätverk. För närvarande kan endast tilldelas en *dynamiska* offentliga IP-adress tooa VPN-gateway.

### <a name="application-gateways"></a>Programgateways
Du kan associera en offentlig IP-adress med en Azure [Programgateway](../application-gateway/application-gateway-introduction.md), genom att tilldela den toohello gateway **klientdel** konfiguration. Den här offentliga IP-adressen fungerar som en belastningsutjämnad virtuell IP-adress. För närvarande kan endast tilldelas en *dynamiska* offentliga IP-adress tooan gateway klientdel programkonfigurationen.

### <a name="at-a-glance"></a>En snabb översikt
hello tabellen nedan visar hello specifik egenskap som en offentlig IP-adress kan vara associerade tooa översta resurs och hello allokering av möjliga metoder (dynamisk eller statisk) som kan användas.

| Resurs på den översta nivån | IP-adressassociation | Dynamisk | Statisk |
| --- | --- | --- | --- |
| Virtuell dator |Nätverksgränssnitt |Ja |Ja |
| Belastningsutjämnare |Konfiguration på klientsidan |Ja |Ja |
| VPN-gateway |IP-konfiguration för gateway |Ja |Nej |
| Programgateway |Konfiguration på klientsidan |Ja |Nej |

## <a name="private-ip-addresses"></a>Privata IP-adresser
Privata IP-adresser låter Azure-resurser toocommunicate med andra resurser i en [virtuellt nätverk](virtual-networks-overview.md) eller ett lokalt nätverk via en VPN-gateway eller ExpressRoute-krets, utan att använda en Internet-nåbar IP-adress.

I hello Azure Resource Manager-distributionsmodellen är en privat IP-adress kopplad toohello följande typer av Azure-resurser:

* Virtuella datorer
* Interna belastningsutjämnare (ILB)
* Programgateways

### <a name="allocation-method"></a>Allokeringsmetod
En privat IP-adress tilldelas från hello adress intervall för hello undernät toowhich hello resurs är ansluten. hello adressintervall hello och själva undernätet ingår i hello VNet-adressintervall.

En privat IP-adress kan allokeras med två metoder: *dynamisk* eller *statisk* allokering. hello standard allokeringsmetoden är *dynamisk*, där hello IP-adress tilldelas automatiskt från hello resursens undernät (med DHCP). Denna IP-adress kan ändra när du stoppar och startar hello resurs.

Du kan ange hello allokeringsmetod för*Statiska* tooensure hello IP-adressen förblir hello samma. I detta fall måste du också tooprovide en giltig IP-adress som ingår i hello resursens undernät.

Statiska privata IP-adresser används ofta för:

* Virtuella datorer som fungerar som domänkontrollanter eller DNS-servrar.
* Resurser som kräver brandväggsregler med IP-adresser.
* Resurser som nås av andra appar/resurser via en IP-adress.

### <a name="virtual-machines"></a>Virtuella datorer
En privat IP-adress tilldelas toohello **nätverksgränssnittet** av en [Windows](../virtual-machines/windows/overview.md) eller [Linux](../virtual-machines/virtual-machines-linux-about.md) VM. Om du har en virtuell dator med flera nätverksgränssnitt tilldelas varje gränssnitt en privat IP-adress. Du kan ange hello allokeringsmetod som dynamiska eller statiska för ett nätverksgränssnitt.

#### <a name="internal-dns-hostname-resolution-for-vms"></a>Intern DNS-värdnamnsmatchning (för virtuella datorer)
Alla virtuella datorer i Azure konfigureras med [Azure-hanterade DNS-servrar](virtual-networks-name-resolution-for-vms-and-role-instances.md#azure-provided-name-resolution) som standard, om du inte uttryckligen konfigurerar anpassade DNS-servrar. Dessa DNS-servrar ger intern namnmatchning för virtuella datorer som finns på hello samma virtuella nätverk.

När du skapar en virtuell dator, läggs en mappning för hello hostname tooits privat IP-adress toohello Azure-hanterade DNS-servrar. Vid ett med flera nätverksgränssnitt VM hello värdnamn mappas toohello privata IP-adressen för hello primära nätverksgränssnittet.

Virtuella datorer som konfigurerats med Azure-hanterade DNS-servrar kommer att kunna tooresolve hello värdnamn för alla virtuella datorer i sina virtuella nätverk tootheir privata IP-adresser.

### <a name="internal-load-balancers-ilb--application-gateways"></a>Interna belastningsutjämnare (ILB) och programgateways
Du kan tilldela en privat IP-adress toohello **klientdelen** konfigurationen av en [Azure interna belastningsutjämnare](../load-balancer/load-balancer-internal-overview.md) (ILB) eller en [Azure Programgateway](../application-gateway/application-gateway-introduction.md). Privata IP-adressen som fungerar som en intern slutpunkt är tillgänglig endast toohello resurser inom dess virtuella nätverk (VNet) och hello fjärrnätverk anslutna toohello VNet. Du kan tilldela antingen en dynamisk eller statisk privat IP-toohello klientdelen adresskonfiguration.

### <a name="at-a-glance"></a>En snabb översikt
hello tabellen nedan visar hello specifik egenskap som en privat IP-adress kan vara associerade tooa översta resurs och hello allokering av möjliga metoder (dynamisk eller statisk) som kan användas.

| Resurs på den översta nivån | IP-adressassociation | Dynamisk | Statisk |
| --- | --- | --- | --- |
| Virtuell dator |Nätverksgränssnitt |Ja |Ja |
| Belastningsutjämnare |Konfiguration på klientsidan |Ja |Ja |
| Programgateway |Konfiguration på klientsidan |Ja |Ja |

## <a name="limits"></a>Begränsningar
hello begränsningar har införts på IP-adresser anges i hello fullständig uppsättning [begränsar för nätverk](../azure-subscription-service-limits.md#networking-limits) i Azure. Dessa gränser anges per region och per prenumeration. Du kan [supporten](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooincrease hello standardgränser in toohello gränsvärden baserat på dina affärsbehov.

## <a name="pricing"></a>Prissättning
Offentliga IP-adresser kan medföra en nominell avgift. toolearn mer om IP-adress priser i Azure, granska hello [IP-adress priser](https://azure.microsoft.com/pricing/details/ip-addresses) sidan.

## <a name="next-steps"></a>Nästa steg
* [Distribuera en virtuell dator med en statisk offentlig IP-adress med hjälp av hello Azure-portalen](virtual-network-deploy-static-pip-arm-portal.md)
* [Distribuera en virtuell dator med en statisk offentlig IP-adress med hjälp av en mall](virtual-network-deploy-static-pip-arm-template.md)
* [Distribuera en virtuell dator med en statisk privat IP-adress med hjälp av hello Azure-portalen](virtual-networks-static-private-ip-arm-pportal.md)
