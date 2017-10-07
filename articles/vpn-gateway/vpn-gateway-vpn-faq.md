---
title: "aaaAzure VPN-Gateway vanliga frågor och svar | Microsoft Docs"
description: "hello VPN-Gateway och-svar. Vanliga frågor och svar om Microsoft Azure Virtual Network-anslutningar på flera platser, konfiguration av hybridanslutningar och VPN-gateways."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
ms.assetid: 6ce36765-250e-444b-bfc7-5f9ec7ce0742
ms.service: vpn-gateway
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/30/2017
ms.author: cherylmc,yushwang
ms.openlocfilehash: e1737f5832728f513e31f97cc7e752147faaaeb4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="vpn-gateway-faq"></a>Vanliga frågor och svar om VPN Gateway

## <a name="connecting"></a>Ansluta toovirtual nätverk

### <a name="can-i-connect-virtual-networks-in-different-azure-regions"></a>Kan jag ansluta till virtuella nätverk i olika Azure-regioner?

Ja. Faktum är att det inte finns någon regionbegränsning. Ett virtuellt nätverk kan ansluta tooanother virtuellt nätverk i hello samma region eller i en annan Azure-region. 

### <a name="can-i-connect-virtual-networks-in-different-subscriptions"></a>Kan jag ansluta till virtuella nätverk i olika prenumerationer?

Ja.

### <a name="can-i-connect-toomultiple-sites-from-a-single-virtual-network"></a>Kan jag ansluta toomultiple platser från ett enda virtuellt nätverk?

Du kan ansluta toomultiple platser med hjälp av Windows PowerShell och hello Azure REST API: er. Se hello [flera platser och VNet-till-VNet-anslutningar](#V2VMulti) frågor och svar.

### <a name="what-are-my-cross-premises-connection-options"></a>Vilka alternativ finns det för min anslutning till flera platser?

hello efter mellan lokala anslutningar stöds:

* Plats-till-plats – VPN-anslutning via IPsec (IKE v1 och IKE v2). Den här typen av anslutning kräver en VPN-enhet eller RRAS. Mer information finns i [Plats-till-plats](vpn-gateway-howto-site-to-site-resource-manager-portal.md).
* Punkt-till-plats – VPN-anslutning över SSTP (Secure Socket Tunneling Protocol). Den här anslutningen kräver inte någon VPN-enhet. Mer information finns i [Punkt-till-plats](vpn-gateway-howto-point-to-site-resource-manager-portal.md).
* VNet-till-VNet – den här typen av anslutning är hello samma som en plats-till-plats-konfiguration. VNet tooVNet är en VPN-anslutning via IPsec (IKE v1 och v2 för IKE). Den kräver inte någon VPN-enhet. Mer information finns i [VNet-till-VNet](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md).
* Flera platser – detta är en variation av en konfiguration för plats-till-plats som du kan använda tooconnect flera lokala platser tooa virtuella nätverk. Mer information finns i [Flera platser](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md).
* ExpressRoute – ExpressRoute är en direktanslutning tooAzure från din WAN, inte en VPN-anslutning via hello offentliga Internet. Mer information finns i hello [teknisk översikt för ExpressRoute](../expressroute/expressroute-introduction.md) och hello [ExpressRoute vanliga frågor och svar](../expressroute/expressroute-faqs.md).

Mer information om VPN-gatewayanslutningar finns i [Om VPN Gateway](vpn-gateway-about-vpngateways.md).

### <a name="what-is-hello-difference-between-a-site-to-site-connection-and-point-to-site"></a>Vad är hello skillnaden mellan en plats-till-plats-anslutning och punkt-till-plats?

**Plats-till-plats**-konfigurationer (IPsec/IKE VPN-tunnel) sker mellan en lokal plats och Azure. Det innebär att du kan ansluta från någon av dina datorer finns på din lokala tooany virtuell dator eller rollinstans i ditt virtuella nätverk, beroende på hur du väljer tooconfigure Routning och behörigheter. Det är ett bra alternativ för att få en anslutning mellan flera platser som alltid är tillgänglig och den passar bra för hybridkonfigurationer. Den här typen av anslutning är beroende av en IPsec VPN-enhet (maskinvaruenhet eller mjuka installation), som måste distribueras i nätverket hello kant. toocreate den här typen av anslutning måste du ha en externt Internetriktade IPv4-adress som inte finns bakom en NAT

**Punkt-till-plats** (VPN över SSTP) konfigurationer kan du ansluta tooanything finns i det virtuella nätverket från en enda dator varifrån som helst. Hello Windows rutan i VPN-klienten används. Som en del av konfigurationen hello punkt-till-plats kan installera du ett certifikat och VPN-klienten konfigurationspaketet, som innehåller hello-inställningar som gör att din dator tooconnect tooany virtuell dator eller rollinstans hello virtuellt nätverk. Det är bra när du vill tooconnect tooa virtuellt nätverk, men inte finnas lokalt. Det är också ett bra alternativ när du inte har åtkomst tooVPN maskinvara eller ett externt Internetriktade IPv4-adress, som krävs för en plats-till-plats-anslutning.

Du kan konfigurera din virtuella nätverk toouse både plats-till-plats och punkt-till-plats samtidigt, så länge du skapar din plats-till-plats-anslutning med en ruttbaserad VPN för din gateway. Ruttbaserad kallas VPN dynamiska gatewayer i hello klassiska distributionsmodellen.

## <a name="gateways"></a>Virtuella nätverksgatewayer

### <a name="is-a-vpn-gateway-a-virtual-network-gateway"></a>Är en VPN-gateway en virtuell nätverksgateway?

En VPN-gateway är en typ av virtuell nätverksgateway. En VPN-gateway skickar krypterad trafik mellan det virtuella nätverket och lokal plats via en offentlig anslutning. Du kan också använda en VPN-gateway toosend trafik mellan virtuella nätverk. När du skapar en VPN-gateway kan du använda hello - GatewayType värdet 'Vpn'. Mer information finns i [Om konfigurationsinställningar för VPN-gateway](vpn-gateway-about-vpn-gateway-settings.md).

### <a name="what-is-a-policy-based-static-routing-gateway"></a>Vad är en principbaserad gateway (statisk routning)?

Principbaserade gateways implementerar principbaserade VPN:er. Principbaserade VPN: er krypterar och dirigerar paket via IPsec-tunnlar baserat på hello kombinationer av adressprefix mellan ditt lokala nätverk och hello Azure VNet. hello principen (eller Trafikväljaren) definieras vanligtvis som en åtkomstlista i hello VPN-konfiguration.

### <a name="what-is-a-route-based-dynamic-routing-gateway"></a>Vad är en routningsbaserad gateway (dynamisk routning)?

Ruttbaserade gatewayer implementera hello ruttbaserade VPN: er. Ruttbaserade VPN: er använder ”rutter” i hello IP-vidarebefordran eller -Routning tabell toodirect paket till sina motsvarande tunnelgränssnitt. hello tunnelgränssnitt sedan kryptera eller dekryptera hälsningspaket till och från hello tunnlar. Hej principen eller trafik väljaren för ruttbaserade VPN: er konfigureras som alla-till-alla (eller jokertecken).

### <a name="do-i-need-a-gatewaysubnet"></a>Behöver jag ett gatewayundernät?

Ja. hello gateway-undernätet innehåller hello IP-adresser som hello virtuellt gateway-tjänster. Du behöver toocreate ett gateway-undernät för ditt VNet i ordning tooconfigure en virtuell nätverksgateway. Alla gateway-undernät måste ha namnet ”GatewaySubnet' toowork korrekt. Ge inte något annat namn till gateway-undernätet. Och inte distribuera virtuella datorer eller något annat toohello gateway-undernätet.

När du skapar hello gateway-undernät kan ange du hello antalet IP-adresser som hello undernät innehåller. hello IP-adresser i hello gateway-undernät tilldelas toohello gateway-tjänsten. Vissa konfigurationer kräver flera IP-adresser toobe allokerade toohello gateway-tjänster än andra. Vill du att din gateway-undernätet innehåller tillräckligt med IP-adresser tooaccommodate framtida tillväxt och eventuella ytterligare nya anslutning konfigurationer toomake. Även om du kan skapa ett gatewayundernät som är så litet som /29, rekommenderar vi att du skapar ett gatewayundernät på /27 eller större (/27, /26, /25 osv.). Titta på hello krav för hello konfiguration som du vill använda toocreate och kontrollera att hello gateway-undernätet som du uppfyller dessa krav.

### <a name="can-i-deploy-virtual-machines-or-role-instances-toomy-gateway-subnet"></a>Kan jag distribuera virtuella datorer eller rollen instanser toomy gateway-undernät?

Nej.

### <a name="can-i-get-my-vpn-gateway-ip-address-before-i-create-it"></a>Kan jag få min IP-adress för VPN-gatewayen innan jag skapar den?

Nej. Du har toocreate din gateway första tooget hello IP-adress. hello IP-adressen ändras om du tar bort och återskapa din VPN-gateway.

### <a name="can-i-request-a-static-public-ip-address-for-my-vpn-gateway"></a>Kan jag begära en statisk offentlig IP-adress för min VPN-gateway?

Nej. Endast dynamisk IP-adresstilldelning stöds. Detta innebär dock inte att hello IP-adressen ändras när den har tilldelats tooyour VPN-gateway. hello endast tid hello VPN-gateway IP-adressändringarna är när hello gateway bort och återskapas. hello VPN-gateway offentliga IP-adressen ändras inte över storleksändring, återställa eller andra internt Underhåll/uppgraderingar av din VPN-gateway. 

### <a name="how-does-my-vpn-tunnel-get-authenticated"></a>Hur blir min VPN-tunnel autentiserad?

Azure VPN använder PSK-autentisering (I förväg delad nyckel). Vi skapar en i förväg delad nyckel (PSK) när vi skapa hello VPN-tunnel. Du kan ändra hello autogenererade PSK tooyour egna med hello ange förväg delad nyckel PowerShell-cmdlet eller REST API.

### <a name="can-i-use-hello-set-pre-shared-key-api-tooconfigure-my-policy-based-static-routing-gateway-vpn"></a>Kan jag använda hello ange förväg delad nyckel API tooconfigure min principbaserad (statisk routning) gateway VPN?

Ja, hello ange förväg delad nyckel API och PowerShell-cmdlet kan vara används tooconfigure både Azure principbaserad (statisk) VPN-anslutningar och ruttbaserad (dynamisk) routning VPN-anslutningar.

### <a name="can-i-use-other-authentication-options"></a>Kan jag använda andra autentiseringsalternativ?

Vi är begränsad toousing i förväg delade nycklar (PSK) för autentisering.

### <a name="how-do-i-specify-which-traffic-goes-through-hello-vpn-gateway"></a>Hur jag anger vilken trafik som passerar hello VPN-gateway?

#### <a name="resource-manager-deployment-model"></a>Distributionsmodell med Resource Manager

* PowerShell: Använd ”AddressPrefix” toospecify trafik för hello lokal nätverksgateway.
* Azure-portalen: navigera toohello lokal nätverksgateway > Configuration >-adressutrymme.

#### <a name="classic-deployment-model"></a>Klassisk distributionsmodell

* Azure-portalen: navigera toohello klassiskt virtuellt nätverk > VPN-anslutningar > plats-till-plats VPN-anslutningar > lokala platsnamn > lokal plats > klientens adressutrymme. 
* Klassiska portal: Lägg till varje intervall som du vill skickas via hello gateway för det virtuella nätverket på hello nätverk sidan under lokala nätverk. 

### <a name="can-i-configure-forced-tunneling"></a>Kan jag konfigurera tvingad tunneltrafik?

Ja. Se [Konfigurera tvingad tunneltrafik](vpn-gateway-about-forced-tunneling.md).

### <a name="can-i-set-up-my-own-vpn-server-in-azure-and-use-it-tooconnect-toomy-on-premises-network"></a>Kan jag konfigurera min egen VPN-server i Azure och använda det tooconnect toomy lokala nätverk?

Ja, kan du distribuera din egen VPN-gatewayer eller servrar i Azure från hello Azure Marketplace eller att skapa VPN-routrarna. Du behöver tooconfigure användardefinierade vägar i det virtuella nätverket tooensure trafik dirigeras korrekt mellan ditt lokala nätverk och dina undernät för virtuellt nätverk.

### <a name="why-are-certain-ports-opened-on-my-vpn-gateway"></a>Varför är vissa portar öppna på min VPN-gateway?

De krävs för Azures infrastrukturkommunikation. De är skyddade (låsta) med Azure-certifikat. Utan rätt certifikat kommer externa enheter, inklusive hello kunder i dessa gateways inte att kunna toocause någon effekten på dessa slutpunkter.

En VPN-gateway är grunden en multi-homed enhet med ett nätverkskort knackar till hello kunden privat nätverks- och en NIC Internetriktade hello offentligt nätverk. Azure-infrastrukturen entiteter kan inte utnyttja kundens privata nätverk av kompatibilitetsskäl, så de måste tooutilize offentliga slutpunkter för infrastruktur-kommunikation. Azure säkerhetsgranskning skannas regelbundet hello offentliga slutpunkter.

### <a name="more-information-about-gateway-types-requirements-and-throughput"></a>Mer information om gateway-typer, krav och dataflöde

Mer information finns i [Om konfigurationsinställningar för VPN-gateway](vpn-gateway-about-vpn-gateway-settings.md).

## <a name="s2s"></a>Plats-till-plats-anslutningar och VPN-enheter

### <a name="what-should-i-consider-when-selecting-a-vpn-device"></a>Vad bör jag tänka på när jag väljer en VPN-enhet?

Vi har verifierat en uppsättning VPN-standardenheter för plats-till-plats tillsammans med våra enhetsleverantörer. En lista över kända kompatibel VPN-enheter, motsvarande konfigurationsanvisningar eller exempel och specifikationer för enheten finns i hello [om VPN-enheter](vpn-gateway-about-vpn-devices.md) artikel. Alla enheter i hello enhetsfamiljer som kända kompatibel bör arbeta tillsammans med ett virtuellt nätverk. toohelp konfigurera din VPN-enhet, finns toohello enheten configuration prov eller länk som motsvarar tooappropriate enhetsfamilj.

### <a name="where-can-i-find-vpn-device-configuration-settings"></a>Var hittar jag konfigurationsinställningar för VPN-enheter?

[!INCLUDE [vpn devices](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

### <a name="how-do-i-edit-vpn-device-configuration-samples"></a>Hur redigerar jag konfigurationsexempel för VPN-enheter?

Mer information om att redigera enhetens konfigurationsexempel finns i [Redigera exempel](vpn-gateway-about-vpn-devices.md#editing).

### <a name="where-do-i-find-ipsec-and-ike-parameters"></a>Var hittar jag IPsec- och IKE-parametrar?

Mer information om IPsec-/IKE-parametrar finns i [Parametrar](vpn-gateway-about-vpn-devices.md#ipsec).

### <a name="why-does-my-policy-based-vpn-tunnel-go-down-when-traffic-is-idle"></a>Varför stängs min principbaserade VPN-tunnel när trafiken är inaktiv?

Detta är ett förväntat beteende för principbaserade VPN-gatewayer (även kallat statisk routning). När hello trafik via tunneln hello är inaktiv under mer än 5 minuter, kommer hello tunnel att datakanalen. När trafiken börjar flöda i båda riktningarna, återupprättas hello tunnel omedelbart.

### <a name="can-i-use-software-vpns-tooconnect-tooazure"></a>Kan jag använda programvara VPN tooconnect tooAzure?

Vi har stöd för Routning och fjärråtkomst (RRAS) i Windows Server 2012 för plats-till-plats-konfiguration mellan flera platser.

Andra VPN-lösningar bör fungera med vår gateway förutsatt att de uppfyller tooindustry standard IPSec-implementeringar. Leverantören hello hello programvara anvisningar för konfiguration och support.

## <a name="P2S"></a>Punkt-till-plats-anslutningar

[!INCLUDE [vpn-gateway-point-to-site-faq-include](../../includes/vpn-gateway-point-to-site-faq-include.md)]

## <a name="V2VMulti"></a>Anslutning mellan virtuella nätverk och flera platser

[!INCLUDE [vpn-gateway-vnet-vnet-faq-include](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

### <a name="can-i-use-azure-vpn-gateway-tootransit-traffic-between-my-on-premises-sites-or-tooanother-virtual-network"></a>Kan jag använda Azure VPN gateway tootransit trafik mellan min lokala platser eller tooanother virtuellt nätverk?

**Distributionsmodell för Resource Manager**<br>
Ja. Se hello [BGP](#bgp) avsnitt för mer information.

**Klassisk distributionsmodell**<br>
Överföring trafik via Azure VPN-gateway är möjligt med hjälp av hello klassiska distributionsmodellen, men förlitar sig på statiskt definierade adressutrymmen i konfigurationsfilen för hello nätverk. BGP är ännu inte stöds med virtuella Azure-nätverk och VPN-gatewayer hello klassiska distributionsmodellen. Utan BGP är manuellt definierade överföringsadressutrymmen mycket felbenägna och rekommenderas inte.

### <a name="does-azure-generate-hello-same-ipsecike-pre-shared-key-for-all-my-vpn-connections-for-hello-same-virtual-network"></a>Genererar Azure hello samma IPsec/IKE i förväg delad nyckel för alla min VPN-anslutningar för hello samma virtuella nätverk?

Nej, Azure genererar som standard olika nycklar för olika VPN-anslutningar. Du kan dock använda hello ange VPN-Gateway nyckeln REST API eller PowerShell-cmdlet tooset hello nyckelvärdet du föredrar. hello-nyckeln måste vara alfanumeriska sträng med längden mellan 1 too128 tecken.

### <a name="do-i-get-more-bandwidth-with-more-site-to-site-vpns-than-for-a-single-virtual-network"></a>Får jag mer bandbredd med fler plats-till-plats-VPN:er än med ett enda virtuellt nätverk?

Nej, alla VPN-tunnlar, inklusive punkt-till-plats-VPN kan dela hello samma Azure VPN gateway och hello tillgänglig bandbredd.

### <a name="can-i-configure-multiple-tunnels-between-my-virtual-network-and-my-on-premises-site-using-multi-site-vpn"></a>Kan jag konfigurera flera tunnlar mellan mitt virtuella nätverk och min lokala plats med hjälp av multisite-VPN?

Ja, men du måste konfigurera BGP på båda tunnlar toohello samma plats.

### <a name="can-i-use-point-to-site-vpns-with-my-virtual-network-with-multiple-vpn-tunnels"></a>Kan jag använda punkt-till-plats-VPN med mitt virtuella nätverk med flera VPN-tunnlar?

Ja, VPN för punkt-till-plats (P2S) kan användas med hello VPN-gatewayer ansluter toomultiple lokala platser och andra virtuella nätverk.

### <a name="can-i-connect-a-virtual-network-with-ipsec-vpns-toomy-expressroute-circuit"></a>Kan jag ansluta ett virtuellt nätverk med IPsec VPN toomy ExpressRoute-krets?

Ja, det stöds. Mer information finns i [Konfigurera ExpressRoute och VPN-anslutningar för plats till plats som kan samexistera](../expressroute/expressroute-howto-coexist-classic.md)

## <a name="ipsecike"></a>IPsec/IKE-princip

[!INCLUDE [vpn-gateway-ipsecikepolicy-faq-include](../../includes/vpn-gateway-ipsecikepolicy-faq-include.md)]


## <a name="bgp"></a>BGP

[!INCLUDE [vpn-gateway-bgp-faq-include](../../includes/vpn-gateway-bpg-faq-include.md)]

## <a name="vms"></a>Anslutning på flera platser och virtuella datorer

### <a name="if-my-virtual-machine-is-in-a-virtual-network-and-i-have-a-cross-premises-connection-how-should-i-connect-toohello-vm"></a>Om min virtuella datorn är i ett virtuellt nätverk och jag har en anslutning mellan platser, hur ska ansluta toohello VM?

Du har några alternativ att välja mellan. Om du har RDP aktiverad för den virtuella datorn kan ansluta du tooyour virtuell dator med hjälp av hello privat IP-adress. Ange i så fall hello privat IP-adress och hello-port som du vill tooconnect för (vanligtvis port 3389). Du behöver tooconfigure hello port på den virtuella datorn för hello-trafik.

Du kan också ansluta tooyour virtuell dator med privata IP-adress från en annan virtuell dator som finns på hello samma virtuella nätverk. Det går inte att RDP tooyour virtuell dator med hjälp av hello privat IP-adress om du ansluter från en plats utanför det virtuella nätverket. Till exempel om du har en punkt-till-plats virtuella datornätverk och du inte upprätta en anslutning från datorn, kan du ansluta toohello virtuella genom privat IP-adress.

### <a name="if-my-virtual-machine-is-in-a-virtual-network-with-cross-premises-connectivity-does-all-hello-traffic-from-my-vm-go-through-that-connection"></a>Om min virtuella dator finns i ett virtuellt nätverk med anslutning, alla hello trafik från den virtuella datorn går via anslutningen?

Nej. Endast hello-trafik som har ett mål IP-adress som ingår i hello virtuella lokala nätverk IP-adressintervall som du har angett går igenom hello virtuell nätverksgateway. Trafik har ett mål i hello virtuellt IP håller sig inom hello virtuellt nätverk. Andra trafik skickas via hello load balancer toohello offentliga nätverk, eller om Tvingad tunneling används, skickas via hello Azure VPN-gateway.

### <a name="how-do-i-troubleshoot-an-rdp-connection-tooa-vm"></a>Hur felsöker jag en RDP-anslutning tooa VM

[!INCLUDE [Troubleshoot VM connection](../../includes/vpn-gateway-connect-vm-troubleshoot-include.md)]


## <a name="faq"></a>Vanliga frågor och svar om Virtual Network

Du kan visa information om ytterligare virtuella nätverk i hello [virtuella nätverk vanliga frågor och svar](../virtual-network/virtual-networks-faq.md).

## <a name="next-steps"></a>Nästa steg

* Mer information om VPN-gateway finns i [Om VPN-gateway](vpn-gateway-about-vpngateways.md).
* Mer information om konfigurationsinställningar för VPN-gateway finns i [Om konfigurationsinställningar för VPN-gateway](vpn-gateway-about-vpn-gateway-settings.md).
