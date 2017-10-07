---
title: aaaVPN Gateway klassiska tooResource migreringen av hanteraren | Microsoft Docs
description: "Den här sidan innehåller en översikt över hello VPN-Gateway klassiska tooResource migreringen av hanteraren."
documentationcenter: na
services: vpn-gateway
author: amsriva
manager: rossort
editor: amsriva
ms.assetid: caa8eb19-825a-4031-8b49-18fbf3ebc04e
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/02/2017
ms.author: amsriva
ms.openlocfilehash: c1d84bf5315224c5c8faac53d7957b496ed205ed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="vpn-gateway-classic-tooresource-manager-migration"></a>VPN-Gateway klassiska tooResource Manager-migrering
VPN-gatewayer kan nu migreras från klassiska tooResource Manager-distributionsmodellen. Du kan läsa mer om Azure Resource Manager [funktioner och fördelar](../azure-resource-manager/resource-group-overview.md). I den här artikeln får information om hur toomigrate från klassiska distributioner toonewer Resource Manager baserade modellen. 

VPN-gatewayer migreras som en del av VNet migreringen från klassiskt tooResource Manager. Migreringen är klar ett virtuellt nätverk i taget. Det finns inga ytterligare krav vad gäller toomigration verktyg eller krav. Migreringssteg är identiska tooexisting VNet migrering och dokumenteras i [IaaS resurser migrering sida](../virtual-machines/windows/migration-classic-resource-manager-ps.md). Det finns inga data sökväg avbrottstid vid migrering och därmed befintliga arbetsbelastningar kan fortsätta toofunction utan lokal anslutningsproblem under migreringen. hello offentlig IP-adress som är associerade med hello VPN-gateway ändras inte under hello migreringsprocessen. Detta innebär att du inte behöver tooreconfigure lokala routern när hello migreringen är klar.  

hello-modellen i Resource Manager skiljer sig från den klassiska modellen och består av virtuella nätverks-gateway, lokala nätverksgatewayer och resurser för anslutningen. Dessa representerar hello VPN-gatewayen, hello lokal plats, som representerar på lokala adressutrymmet och anslutningen mellan hello två respektive. När migreringen är klar din gateway är inte tillgängliga i den klassiska modellen och alla hanteringsåtgärder på gateway för virtuellt nätverk och lokala nätverksgatewayer anslutningsobjekt måste utföras med hjälp av Resource Manager-modellen.

## <a name="supported-scenarios"></a>Scenarier som stöds
Vanliga scenarier för VPN-anslutningen omfattas av klassiska tooResource Manager-migrering. hello stöds scenarier är-

* Punkt toosite anslutning
* Plats-anslutning med VPN-Gateway på toosite anslutna tooon lokal plats
* VNet tooVNet anslutning mellan två Vnet med hjälp av VPN-gatewayer
* Flera Vnet anslutna toosame på lokal plats
* Flera plats-anslutning
* Tvingad tunneling aktiverat Vnet

Scenarier som inte stöds är-  

* Virtuella nätverk med både ExpressRoute-Gateway och VPN-Gateway stöds inte för närvarande.
* Överföring scenarier där VM-tillägg är anslutna tooon lokala servrar. Begränsningar för överföring VPN-anslutningen beskrivs nedan.

> [!NOTE]
> CIDR-verifiering i Resource Manager-modellen är striktare än hello något i den klassiska modellen. Innan du migrerar du säkerställa att klassiska adressintervall anges överensstämmer toovalid CIDR-format innan du påbörjar migreringen hello. CIDR kan verifieras med hjälp av några vanliga CIDR verifierare. Virtuellt nätverk eller lokala platser med ogiltig CIDR-intervallen när migreras skulle resultera i felaktigt tillstånd.
> 
> 

## <a name="vnet-toovnet-connectivity-migration"></a>VNet tooVNet anslutning migrering
VNet tooVNet anslutning i klassiskt uppnåddes genom att skapa en lokal plats representation av hello anslutna virtuella nätverk. Kunder har nödvändiga toocreate två lokala webbplatser som representerade hello två Vnet som behövs för toobe är sammankopplade. Dessa har sedan anslutna toohello motsvarande Vnet med hjälp av IPSec-tunneln tooestablish anslutningen mellan hello två Vnet. Den här modellen har hanterbarhet utmaningar eftersom adressintervallet ändringar i ett virtuellt nätverk måste även hanteras i hello motsvarande lokala platsen representation. Den här lösningen behövs inte längre i Resource Manager-modellen. hello-anslutning mellan två Vnet kan uppnås direkt med anslutningstypen 'Vnet2Vnet' i anslutningen resursen hello. 

![Skärmbild av VNet tooVNet migrering.](./media/vpn-gateway-migration/migration1.png)

Under VNet-migreringen vi identifiera som hello anslutna entiteten toocurrent VNet VPN-gateway är ett annat VNet och kontrollera att när migreringen av båda Vnet har slutförts, inte längre visas två lokala webbplatser som representerar hello andra VNet. hello klassiska modellen av två VPN-gatewayer, två lokala webbplatser och två anslutningar mellan dem är transformerad tooResource Manager-modellen med två VPN-gatewayer och två anslutningar av typen Vnet2Vnet.

## <a name="transit-vpn-connectivity"></a>VPN-anslutning för överföring
Du kan konfigurera VPN-gatewayer i en topologi som lokal anslutning för ett virtuellt nätverk uppnås genom att ansluta tooanother virtuella nätverk som är direkt anslutna tooon plats. Detta är överföring VPN-anslutning där instanser i första virtuella nätverk är anslutna tooon lokala resurser via överföring toohello VPN-gateway i anslutna virtuella nätverk som är direkt anslutna tooon plats. tooachieve denna konfiguration i klassiska distributionsmodellen måste toocreate en lokal plats som har samman prefix som representerar både hello anslutna virtuella nätverk och lokala adressutrymmet. Representational lokala platsen är anslutna toohello VNet tooachieve transit anslutningen. Den här klassiska modellen har också liknande hanterbarhet utmaningar eftersom ändringar i lokala adressintervallet måste även hanteras på hello lokala platsen som representerar hello aggregering av VNet och lokalt. Introduktion av BGP-stöd i Resource Manager stöds gateways underlättas hanterbarhet hello anslutna gateways kan lära sig vägarna från lokalt utan manuella ändringar tooprefixes.

![Skärmbild av överföring routningsscenario.](./media/vpn-gateway-migration/migration2.png)

Eftersom vi transformera VNet tooVNet anslutning utan lokala platser förlorar hello överföring scenariot lokala anslutningen för virtuella nätverk som är indirekt anslutna tooon plats. hello anslutningsproblem kan begränsas i hello följande två sätt, när migreringen är klar - 

* Aktivera BGP på VPN-gatewayer som ansluts till varandra och tooon lokaler. Aktivera BGP återställer anslutning utan någon konfigurationsändring eftersom vägar lärt dig och annonserade mellan VNet-gateway. Observera att BGP-alternativet är bara tillgängligt på Standard och högre SKU: er.
* Skapa en explicit anslutning från berörda VNet toohello lokala nätverksgateway som representerar lokal plats. Detta skulle också behöva ändras konfigurationen på hello lokala router toocreate och konfigurera hello IPsec-tunneln.

## <a name="next-steps"></a>Nästa steg
Efter att lära dig mer om stöd för VPN-gateway migreringen gå för[plattform som stöds migrering av IaaS-resurser från klassiska tooResource Manager](../virtual-machines/windows/migration-classic-resource-manager-ps.md) tooget igång.

