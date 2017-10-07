---
title: "aaaAzure virtuellt nätverk peering | Microsoft Docs"
description: "Lär dig mer om virtuell nätverkspeering i Azure."
services: virtual-network
documentationcenter: na
author: NarayanAnnamalai
manager: jefco
editor: tysonn
ms.assetid: eb0ba07d-5fee-4db0-b1cb-a569b7060d2a
ms.service: virtual-network
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/17/2017
ms.author: narayan
ms.openlocfilehash: 46a14b416a7d4389f79a3cd7c55e388b5d312577
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="virtual-network-peering"></a>Virtuell nätverkspeering
Virtuellt nätverk peering aktiverar du tooconnect två virtuella nätverk i hello samma region via hello Azure stamnät nätverk. När peerkoppla, visas hello två virtuella nätverk som en för anslutningen. hello två virtuella nätverk fortfarande hanteras som separata resurser, men virtuella datorer i hello peerkoppla virtuella nätverk kan kommunicera med varandra direkt, genom att använda privata IP-adresser.

hello trafik mellan virtuella datorer i hello peerkoppla virtuella nätverk dirigeras via hello Azure-infrastrukturen, mycket som trafik dirigeras mellan virtuella datorer i hello samma virtuella nätverk. Hello fördelarna med virtuella nätverk peering bland annat:

* En anslutning med korta svarstider och hög bandbredd mellan resurser i olika virtuella nätverk.
* hello möjlighet toouse resurser, till exempel nätverksinstallationer och VPN-gatewayer som överföring punkter i ett peered virtuellt nätverk.
* hello möjlighet toopeer två virtuella nätverk som skapats via hello Azure Resource Manager-distributionsmodellen eller toopeer ett virtuellt nätverk skapas med Resource Manager tooa virtuella nätverk som skapats via hello klassiska distributionsmodellen. Läs hello [förstå Azure distributionsmodeller](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artikel toolearn mer om hello skillnaderna mellan hello två distributionsmodeller för Azure.

## <a name="requirements-constraints"></a>Det finns vissa krav och begränsningar

* Hej peerkoppla virtuella nätverk måste finnas i hello samma Azure-region. Du kan ansluta till virtuella nätverk i olika Azure-regioner med en [VPN-gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V).
* Hej måste peerkoppla virtuella nätverk ha icke-överlappande IP-adressutrymmen.
* Adressutrymmen kan inte läggas till eller tas bort från ett virtuellt nätverk när ett virtuellt nätverk är peerkopplat med ett annat virtuellt nätverk.
* Peer-kopplingarna i virtuella nätverk upprättas mellan två virtuella nätverk. Det finns ingen härledd transitiv relation mellan peer-kopplingar. Om virtualNetworkA är peerkopplat med virtualNetworkB och virtualNetworkB peerkopplat med virtualNetworkC, virtualNetworkA är exempelvis *inte* peerkoppla toovirtualNetworkC.
* -Peer-virtuella nätverk som finns i två olika prenumerationer som länge Privilegierade användare (se [specifika behörigheter](create-peering-different-deployment-models-subscriptions.md#permissions)) för både prenumerationer auktoriserar hello peering och hello-prenumerationer är associerade toohello samma Azure Active Directory-klient. Du kan använda en [VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V) tooconnect virtuella nätverk i prenumerationer som är associerade med toodifferent Active Directory-klienter.
* Virtuella nätverk kan vara peerkoppla om båda skapas via hello Resource Manager-distributionsmodellen eller om ett virtuellt nätverk skapas via hello Resource Manager-modellen och hello andra skapas via hello klassiska distributionsmodellen. Två virtuella nätverk som skapats via hello klassiska distributionsmodellen kan inte vara peerkoppla tooeach andra, men. Du kan använda en [VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V) tooconnect två virtuella nätverk som skapats via hello klassiska distributionsmodellen.
* Även om hello kommunikation mellan virtuella datorer i peerkoppla virtuella nätverk har inga ytterligare bandbredd begränsningar, finns en maximal bandbredd beroende på hello virtuella storlek som fortfarande gäller. Mer om maximal nätverksbandbredd för olika virtuella datorstorlekar, läsa hello toolearn [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) eller [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) virtuella storlekar artiklar.
* Azures interna DNS-namnmatchning för virtuella datorer fungerar inte mellan peer-kopplade virtuella nätverk. Virtuella datorer har interna DNS-namn som matchas endast inom hello lokala virtuella nätverket. Du kan dock konfigurera virtuella nätverk för virtuella datorer anslutna toopeered som DNS-servrar för ett virtuellt nätverk. Mer information finns i hello [namnmatchning med hjälp av DNS-servern](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) artikel.

![Grundläggande virtuell nätverkspeering](./media/virtual-networks-peering-overview/figure01.png)

## <a name="connectivity"></a>Anslutning
När två virtuella nätverk är peerkopplat ansluta resurser i antingen virtuellt nätverk direkt med resurser i hello peerkoppla virtuellt nätverk. hello två virtuella nätverk har fullständig IP-anslutning.

hello Nätverksfördröjningen för kommunikation mellan två virtuella datorer i peerkoppla virtuella nätverk är hello samma som för onödig kommunikation inom ett enda virtuellt nätverk. hello dataflödet i nätverket är baserad på hello bandbredd som tillåts för hello virtuell dator, proportionell tooits storlek. Det finns inte några ytterligare begränsning av bandbredd inom hello peering.

hello trafik mellan virtuella datorer i peerkoppla virtuella nätverk vidarebefordras direkt via hello Azure backend-infrastruktur, inte via en gateway.

Virtuellt nätverk för virtuella datorer anslutna tooa kan komma åt hello interna belastningsutjämnade slutpunkter i hello peerkoppla virtuellt nätverk. Nätverkssäkerhetsgrupper kan användas i virtuella nätverk tooblock åtkomst tooother virtuella nätverk eller undernät, om så önskas.

När du konfigurerar peering virtuellt nätverk, kan du öppna eller stänga hello regler för nätverkssäkerhetsgrupper mellan hello virtuella nätverk. Om du öppnar fullständig anslutningen mellan peerkoppla virtuella nätverk (som är hello standardalternativet), kan du tillämpa network security grupper toospecific undernät eller virtuella datorer tooblock eller neka specifika. Mer om toolearn nätverkssäkerhetsgrupper, läsa hello [Network security groups översikt](virtual-networks-nsg.md) artikel.

## <a name="service-chaining"></a>Tjänstlänkning
Du kan konfigurera användardefinierade vägar som punkt toovirtual datorer i peerkoppla virtuella nätverk som hello ”nästa hopp” IP-adress tooenable service länkning. Tjänsten länkning aktiverar du toodirect trafik från en virtuell tooa virtuell nätverksenhet i ett peered virtuellt nätverk via användardefinierade vägar.

Du kan också effektivt bygga NAV och eker-typen miljöer där hello-hubb kan vara värd för infrastrukturkomponenter, till exempel en virtuell nätverksenhet. Alla hello ekrar virtuella nätverk kan sedan peer med hello hubb virtuellt nätverk. Trafiken kan flöda via nätverket virtuella installationer som körs i hello hubb virtuellt nätverk. Kort sagt, kan virtuella nätverk peering hello IP-adress för nästa hopp på hello användardefinierad väg toobe hello IP-adressen för en virtuell dator i hello peerkoppla virtuellt nätverk. Mer om användardefinierade vägar, läsa hello toolearn [användardefinierade vägar översikt](virtual-networks-udr-overview.md) artikel.

## <a name="gateways-and-on-premises-connectivity"></a>Gateways och lokala anslutningar
Varje virtuellt nätverk, oavsett om den är peerkopplat med ett annat virtuellt nätverk kan fortfarande ha sin egen gateway och använda den tooconnect tooan lokalt nätverk. Du kan också konfigurera [virtuella nätverk till virtuella nätverksanslutningar](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json) med hjälp av gateways, även om hello virtuella nätverk är peerkopplat.

När båda alternativen för virtuellt nätverk anslutbarhet konfigureras hello trafik mellan virtuella nätverk för hello flödar genom hello peering konfiguration (det vill säga via hello Azure stamnät).

När virtuella nätverk är peerkopplat kan också konfigurera hello gateway i hello peerkoppla virtuella nätverk som en överföring punkt tooan lokalt nätverk. I det här fallet kan inte hello virtuella nätverk som använder en fjärr-gateway ha en egen gateway. Ett virtuellt nätverk kan bara ha en gateway. hello gateway kan ha en lokal eller fjärransluten gateway (hello peerkoppla virtuella nätverk), som visas i följande bild hello:

![Överföring med VNet-peering](./media/virtual-networks-peering-overview/figure02.png)

Gateway-överföring stöds inte i hello peering relationen mellan virtuella nätverk som skapats via olika distributionsmodeller. Båda virtuella nätverken i hello peering-relationen måste ha skapats via Resource Manager för en gateway överföring toowork.

När hello virtuella nätverk som delar ett enda Azure ExpressRoute-anslutning är peerkopplat hello trafiken mellan dem passerar hello peering relation (det vill säga via hello Azure stamnät nätverket). Du kan fortfarande använda lokala gateways i varje virtuellt nätverk tooconnect toohello lokalt krets. Du kan även använda en delad gateway och konfigurera överföringen för lokala anslutningar.

## <a name="provisioning"></a>Etablering
Virtuell nätverkspeering är en privilegierad åtgärd. Det är en separat funktion under hello VirtualNetworks namnområde. En användare kan få specifika rättigheter tooauthorize peering. En användare som har skrivskyddad åtkomst toohello virtuellt nätverk ärver rättigheterna automatiskt.

En användare som är antingen en administratör eller en privilegierad användare av hello peering förmåga kan initiera en peering-åtgärd i ett annat virtuellt nätverk. Om det finns en matchande begäran för peering på hello andra sidan och hello peering upprättades om andra krav är uppfyllda.

## <a name="limits"></a>Begränsningar
Det finns begränsningar för hello antalet peerkopplingar som tillåts för ett enda virtuellt nätverk. Mer information finns i hello [Azure nätverk gränser](../azure-subscription-service-limits.md#networking-limits).

## <a name="pricing"></a>Prissättning
En nominell avgift tas ut för ingående och utgående trafik som använder virtuell nätverks-peering. Mer information finns i hello [sida med priser](https://azure.microsoft.com/pricing/details/virtual-network).

## <a name="next-steps"></a>Nästa steg

* Slutför självstudien om peering för virtuella nätverk. Ett virtuellt nätverk som peering skapas mellan virtuella nätverk som skapats via hello samma eller olika distributionsmodeller som finns i hello samma eller olika prenumerationer. Kursen för en hello följande scenarier:
 
    |Azure-distributionsmodell  | Prenumeration  |
    |---------|---------|
    |Båda Resource Manager |[Samma](virtual-network-create-peering.md)|
    | |[Olika](create-peering-different-subscriptions.md)|
    |En Resource Manager, en klassisk     |[Samma](create-peering-different-deployment-models.md)|
    | |[Olika](create-peering-different-deployment-models-subscriptions.md)|

* Lär dig hur toocreate en [NAV och ekrar nätverkstopologi](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) 
* Lär dig mer om alla [peering inställningar för virtuella nätverk och hur toochange dem.](virtual-network-manage-peering.md)
