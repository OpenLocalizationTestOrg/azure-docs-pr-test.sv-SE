---
title: "aaaAzure nätverk riktlinjer för infrastruktur - Windows | Microsoft Docs"
description: "Läs mer om hello viktiga design och implementeringslösning riktlinjer för att distribuera virtuella nätverk i Azure infrastrukturtjänster."
documentationcenter: 
services: virtual-machines-windows
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: e2d45973-5eba-4904-8ba0-1821f64feed7
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 64c288957e7c7b89578d871a74ff45ce470ed922
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-networking-infrastructure-guidelines-for-windows-vms"></a>Azure nätverk infrastruktur riktlinjer för virtuella Windows-datorer 

[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)]

Den här artikeln fokuserar på förstå hello krävs planeringssteg för virtuella nätverk i Azure och anslutningen mellan den befintliga lokala miljöer.

## <a name="implementation-guidelines-for-virtual-networks"></a>Implementeringsriktlinjer för virtuella nätverk
Beslut:

* Vilken typ av det virtuella nätverket behöver du toohost din IT-arbetsbelastning eller infrastruktur (endast molnbaserad eller mellan platser)?
* För anslutningar mellan lokala virtuella nätverk, hur mycket adressutrymmet behöver du toohost hello undernät och virtuella datorer nu och för rimliga expansion i hello framtida?
* Är du ska toocreate centraliserad virtuella nätverk eller skapa enskilda virtuella nätverk för varje resursgrupp?

Aktiviteter:

* Definiera hello adressutrymmet för hello virtuella nätverk toobe skapas.
* Definiera hello undernät och hello-adressutrymmet för varje.
* För anslutningar mellan lokala definiera virtuella nätverk, hello lokala nätverket-adressutrymmen för hello lokala platser som hello virtuella datorer i hello virtuella nätverk måste tooreach.
* Arbeta med lokala nätverk team tooensure hello rätt routning konfigureras när du skapar anslutningar mellan lokala virtuella nätverk.
* Skapa hello virtuellt nätverk med en namngivningskonvention.

## <a name="virtual-networks"></a>Virtuella nätverk
Virtuella nätverken är nödvändiga toosupport kommunikation mellan virtuella datorer (VM). Du kan definiera undernät, anpassad IP-adress, DNS-inställningar, säkerhetsfiltrering och belastningsutjämning, som i fysiska nätverk. Med hjälp av en [VPN-gateway](../../vpn-gateway/vpn-gateway-about-vpngateways.md) eller [Express Route-krets](../../expressroute/expressroute-introduction.md), kan du ansluta virtuella Azure-nätverk tooyour lokala nätverk. Du kan läsa mer om [virtuella nätverk och deras komponenter](../../virtual-network/virtual-networks-overview.md).

Med resursgrupper kan ha du flexibilitet i hur du utformar din virtuella nätverkskomponenter. Virtuella datorer kan ansluta toovirtual nätverk utanför sina egna resursgruppen. En vanlig metod för designen skulle vara toocreate centraliserad resursgrupper som innehåller din core nätverksinfrastruktur som kan hanteras av en gemensam team och sedan virtuella datorer och deras program distribueras tooseparate resursgrupper. Den här metoden ger programägarna tillgång toohello resursgruppen som innehåller deras virtuella datorer utan att öppna in toohello konfiguration av hello större virtuella nätverksresurser.

## <a name="site-connectivity"></a>Plats-anslutning
### <a name="cloud-only-virtual-networks"></a>Endast molnbaserad virtuella nätverk
Om lokala användare och datorer inte kräver kontinuerlig anslutning tooVMs i Azure-nätverk, är direkt framåt utformningen virtuellt nätverk:

![Grundläggande endast molnbaserad virtuella nätverksdiagram](./media/infrastructure-networking-guidelines/vnet01.png)

Den här metoden är vanligtvis för Internet-riktade arbetsbelastningar, t.ex en webbserver med Internet. Du kan hantera dessa virtuella datorer med RDP eller punkt-till-plats VPN-anslutningar.

Eftersom de inte ansluta tooyour lokalt nätverk, kan endast Azure-virtuella nätverk använda någon del av hello privata IP-adressutrymme, även om hello samma privata utrymme finns i använda lokalt.

### <a name="cross-premises-virtual-networks"></a>Anslutningar mellan lokala virtuella nätverk
Om lokala användare och datorer kräver pågående anslutning tooVMs i Azure-nätverk, kan du skapa ett virtuellt nätverk mellan platser.  Anslut den tooyour lokalt nätverk med en ExpressRoute eller plats-till-plats VPN-anslutning.

![Anslutningar mellan lokala virtuella nätverksdiagram](./media/infrastructure-networking-guidelines/vnet02.png)

I den här konfigurationen är hello virtuella Azure-nätverket i stort sett molnbaserade tillägget i ditt lokala nätverk.

Eftersom de ansluter tooyour lokalt nätverk, virtuella nätverk måste använda en del av hello-adressutrymmet som används av din organisation som är unikt mellan platser. I hello samma sätt som andra företagets platser tilldelas en specifik IP-undernät, Azure blir en annan plats som du sträcker sig utanför nätverket.

tooallow paket tootravel från nätverket mellan lokala virtuella nätverket tooyour lokalt, måste du konfigurera hello uppsättning relevanta lokala adressprefix som en del av hello lokala nätverksdefinition för hello virtuellt nätverk. Beroende på hello adress utrymme för hello virtuella nätverk och hello uppsättning relevanta lokala platser, det kan finnas många adressprefix i hello lokala nätverket.

Du kan konvertera ett virtuellt nätverk för virtuella nätverk för endast molnbaserad tooa mellan platser, men den mest sannolika kräver du toore IP-dina virtuella nätverkets adressutrymme och Azure-resurser. Därför ska du noga överväga om ett virtuellt nätverk måste toobe anslutna tooyour lokala nätverk när du tilldelar en IP-undernät.

## <a name="subnets"></a>Undernät
Undernät kan du tooorganize resurser som är relaterade, antingen logiskt (till exempel ett undernät för virtuella datorer som är associerade toohello samma program), eller fysiskt (till exempel ett undernät per resursgrupp). Du kan också använda undernätet isolering tekniker för extra säkerhet.

För anslutningar mellan lokala virtuella nätverk, bör du utforma undernät med hello samma konventioner som du använder för lokala resurser. **Azure alltid använder hello tre första IP-adresser för varje undernät hello adressutrymme**. toodetermine hello många av adresser krävs för hello undernät, starta genom att räkna hello antalet virtuella datorer som du behöver just nu. Beräkna framtida tillväxt och Använd sedan hello efter tabellen toodetermine hello storleken på hello undernät.

| Antal virtuella datorer krävs | Antal värden bitar som behövs | Storleken på hello undernät |
| --- | --- | --- |
| 1–3 |3 |/29 |
| 4–11 |4 |/28 |
| 12–27 |5 |/27 |
| 28–59 |6 |/26 |
| 60–123 |7 |/25 |

> [!NOTE]
> För normal lokala undernät hello maxantalet för ett undernät med n värden bits-värdadresser är 2<sup> n </sup> – 2. För ett Azure-undernät är hello maxantalet för ett undernät med n värden bits-värdadresser 2<sup> n </sup> – 5 (2 plus 3 för hello-adresser som Azure använder i varje undernät).
> 
> 

Om du väljer en undernätets storlek är för liten har toore IP och omdistribuera hello virtuella datorer i hello undernät.

## <a name="network-security-groups"></a>Nätverkssäkerhetsgrupper
Du kan använda filtrera regler toohello trafik som flödar genom dina virtuella nätverk med Nätverkssäkerhetsgrupper. Du kan skapa detaljerade filtrering regler toosecure din virtuella nätverksmiljö styr inkommande och utgående trafik, käll- och IP-adressintervall, tillåtna portar, osv. Nätverkssäkerhetsgrupper kan vara tillämpade toosubnets inom ett virtuellt nätverk eller direkt tooa givet virtuellt nätverksgränssnitt. Det är rekommenderat toohave vissa andelen Nätverkssäkerhetsgruppen filtrerar trafik i ditt virtuella nätverk. Du kan läsa mer om [Nätverkssäkerhetsgrupper](../../virtual-network/virtual-networks-nsg.md).

## <a name="additional-network-components"></a>Ytterligare nätverkskomponenter
Precis som med en lokal fysiska nätverk infrastruktur, innehålla Azure virtuella nätverk fler än undernät och IP-adresser. När du utformar din infrastruktur du tooincorporate vissa av dessa ytterligare komponenter:

* [VPN-gatewayer](../../vpn-gateway/vpn-gateway-about-vpngateways.md) -ansluta virtuella Azure-nätverk tooother virtuella Azure-nätverk, eller Anslut tooon lokala nätverk via en plats-till-plats VPN-anslutning. Implementera Express Route-anslutningar för dedikerad, säkra anslutningar. Du kan också ge användare direktåtkomst punkt-till-plats VPN-anslutningar.
* [Belastningsutjämnaren](../../load-balancer/load-balancer-overview.md) -ger belastningsutjämning av trafik för både externa och interna trafik efter behov.
* [Programgateway](../../application-gateway/application-gateway-introduction.md) – HTTP belastningsutjämning på programnivå hello, tillhandahåller vissa ytterligare fördelar för webbprogram i stället för att distribuera hello Azure belastningsutjämnare.
* [Traffic Manager](../../traffic-manager/traffic-manager-overview.md) - DNS-baserade trafik distribution toodirect slutanvändare toohello närmaste tillgängliga slutpunkt, vilket gör att du toohost ditt program utanför Azure-datacenter i olika regioner.

## <a name="next-steps"></a>Nästa steg
[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)]

