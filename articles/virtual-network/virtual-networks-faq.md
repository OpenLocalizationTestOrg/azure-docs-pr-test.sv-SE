---
title: "aaaAzure virtuella nätverk vanliga frågor och svar | Microsoft Docs"
description: "Svaren toohello mest vanliga frågor och svar om Microsoft Azure-nätverk."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
ms.assetid: 54bee086-a8a5-4312-9866-19a1fba913d0
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/18/2017
ms.author: jdial
ms.openlocfilehash: c2f9faee41b9c45e8bb196c58282d597ae732e54
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-virtual-network-frequently-asked-questions-faq"></a>Vanliga frågor (FAQ) virtuella Azure-nätverket

## <a name="virtual-network-basics"></a>Grunderna i virtuella nätverk

### <a name="what-is-an-azure-virtual-network-vnet"></a>Vad är ett Azure Virtual Network (VNet)?
Ett Azure Virtual Network (VNet) är en representation av ditt eget nätverk i hello molnet. Det är en logisk isolering av hello Azure-molnet dedikerad tooyour prenumeration. Du kan använda Vnet tooprovision och hantera virtuella privata nätverk (VPN) i Azure och eventuellt länken hello Vnet med andra Vnet i Azure, eller med din lokala IT-infrastruktur toocreate hybrid eller mellan lokala lösningar. Varje VNet som du skapar har sin egen CIDR-block och kan vara länkade tooother Vnet och lokala nätverk som inte överlappar hello CIDR-block. Du kan ha kontroll över DNS-serverinställningarna för Vnet och segmentering av hello VNet i undernät.

Använd Vnet till:

* Skapa en dedikerad privata endast molnbaserad VNet ibland behöver en mellan lokala konfiguration för din lösning. När du skapar ett VNet, kan tjänster och virtuella datorer i ditt VNet kommunicera direkt och på ett säkert sätt med varandra i hello molnet. Detta håller trafiken på ett säkert sätt i hello VNet, men kan du fortfarande tooconfigure endpoint anslutningar för hello virtuella datorer och tjänster som kräver Internet-kommunikation som en del av din lösning.
* Utöka ditt datacenter med Vnet på ett säkert sätt, du kan skapa traditionella plats-till-plats (S2S) VPN toosecurely skala datacenter-kapacitet. S2S VPN-anslutningar använder IPSEC tooprovide en säker anslutning mellan företagets VPN-gateway och Azure.
* Aktivera hybridscenarier för Vnet ger dig hello flexibilitet toosupport ett antal scenarier för hybrid. Du kan på ett säkert sätt ansluta molnbaserade program tooany typ av lokalt system, till exempel stordatorer och Unix-system.

### <a name="how-do-i-know-if-i-need-a-vnet"></a>Hur vet jag om jag behöver ett virtuellt nätverk?
Hej [översikt över virtuella nätverk](virtual-networks-overview.md) artikeln innehåller en beslutstabell som hjälper dig att bestämma hello bästa nätverket designalternativ du.

### <a name="how-do-i-get-started"></a>Hur kommer jag igång?
Besök hello [virtuella nätverk dokumentationen](https://docs.microsoft.com/azure/virtual-network/) tooget igång. Det här innehållet ger information om översikt och distribution för alla hello VNet-funktioner.

### <a name="can-i-use-vnets-without-cross-premises-connectivity"></a>Kan jag använda Vnet utan korsanslutningar?
Ja. Du kan använda ett virtuellt nätverk utan att använda hybridanslutning. Detta är särskilt användbart om du vill toorun Microsoft Windows Server Active Directory-domänkontrollanter och SharePoint-servergrupper i Azure.

### <a name="can-i-perform-wan-optimization-between-vnets-or-a-vnet-and-my-on-premises-data-center"></a>Kan jag utföra WAN-optimering mellan Vnet eller ett VNet och min lokala Datacenter?

Ja. Du kan distribuera en [WAN-optimering virtuell nätverksenhet](https://azure.microsoft.com/marketplace/?term=wan+optimization) från flera leverantörer via hello Azure Marketplace.

## <a name="configuration"></a>Konfiguration

### <a name="what-tools-do-i-use-toocreate-a-vnet"></a>Verktyg kan jag använda toocreate ett VNet?
Du kan använda följande verktyg toocreate hello eller konfigurera ett virtuellt nätverk:

* Azure-portalen (för klassisk och Resource Manager VNets).
* En nätverket konfigurationsfil (netcfg - för klassiska Vnet). Se hello [konfigurera ett virtuellt nätverk med en konfigurationsfil för nätverket](virtual-networks-using-network-configuration-file.md) artikel.
* PowerShell (för klassisk och Resource Manager VNets).
* Azure CLI (för klassisk och Resource Manager VNets).

### <a name="what-address-ranges-can-i-use-in-my-vnets"></a>Vilka adressintervall kan jag använda min Vnet?
Alla IP-adressintervall som definierats i [RFC 1918](http://tools.ietf.org/html/rfc1918). Till exempel 10.0.0.0/16.

### <a name="can-i-have-public-ip-addresses-in-my-vnets"></a>Kan jag ha offentliga IP-adresser i Mina Vnet?
Ja. Mer information om offentliga IP-adressintervall finns hello [offentliga IP-adressutrymme i ett virtuellt nätverk](virtual-networks-public-ip-within-vnet.md) artikel. Din offentliga IP-adresser är inte tillgänglig direkt från hello Internet.

### <a name="is-there-a-limit-toohello-number-of-subnets-in-my-vnet"></a>Finns det en begränsa toohello antalet undernät i mitt virtuella nätverk?
Ja. Läs hello [Azure begränsar](../azure-subscription-service-limits.md#networking-limits) artikeln för information. Undernät adressutrymmen får inte överlappa varandra.

### <a name="are-there-any-restrictions-on-using-ip-addresses-within-these-subnets"></a>Finns det några begränsningar med hjälp av IP-adresser inom dessa undernät?
Ja. Azure reserverar vissa IP-adresser inom varje undernät. hello är första och sista IP-adresser för hello undernät reserverade för överensstämmelse med protokollet, tillsammans med 3 flera adresser som används för Azure-tjänster.

### <a name="how-small-and-how-large-can-vnets-and-subnets-be"></a>Hur små och hur stor kan vara Vnet och undernät?
hello minsta undernät vi stöder är en /29 och hello största är en/8 som (med CIDR-subnätsdefinitioner).

### <a name="can-i-bring-my-vlans-tooazure-using-vnets"></a>Kan jag använda min VLAN tooAzure med Vnet?
Nej. Vnet är nivå 3 överlägg. Azure stöder inte någon semantik för Layer-2.

### <a name="can-i-specify-custom-routing-policies-on-my-vnets-and-subnets"></a>Kan jag ange anpassade routningsprinciper på Mina Vnet och undernät?
Ja. Du kan använda användaren definierat routning (UDR). Mer information om UDR finns [användardefinierade vägar och IP-vidarebefordring](virtual-networks-udr-overview.md).

### <a name="do-vnets-support-multicast-or-broadcast"></a>Stöder Vnet multicast eller broadcast?
Nej. Vi stöder inte multicast eller broadcast.

### <a name="what-protocols-can-i-use-within-vnets"></a>Vilka protokoll inom Vnet kan jag använda?
Du kan använda TCP och UDP ICMP TCP/IP-protokoll inom Vnet. Multicast, broadcast, kapslas in IP-i-IP-paket och allmänna Routing Encapsulation (GRE) paket blockeras inom Vnet. 

### <a name="can-i-ping-my-default-routers-within-a-vnet"></a>Kan jag Pinga standard-routrar inom ett VNet?
Nej.

### <a name="can-i-use-tracert-toodiagnose-connectivity"></a>Kan jag använda tracert toodiagnose anslutningen?
Nej.

### <a name="can-i-add-subnets-after-hello-vnet-is-created"></a>Kan jag lägga till undernät efter hello VNet har skapats?
Ja. Undernät kan läggas tooVNets när som helst så länge hello undernätsadress inte ingår i ett annat undernät i hello virtuella nätverk.

### <a name="can-i-modify-hello-size-of-my-subnet-after-i-create-it"></a>Kan jag ändra hello storleken på mitt undernät när jag har skapat den?
Ja. Du kan lägga till, ta bort, expandera eller förminska ett undernät om det finns inga virtuella datorer eller tjänster som distribueras inom den.

### <a name="can-i-modify-subnets-after-i-created-them"></a>Kan jag ändra undernät när jag skapar dem?
Ja. Du kan lägga till, ta bort och ändra hello CIDR-block som används av ett virtuellt nätverk.

### <a name="can-i-connect-toohello-internet-if-i-am-running-my-services-in-a-vnet"></a>Kan jag ansluta toohello internet om jag använder Mina tjänster i ett virtuellt nätverk?
Ja. Alla tjänster som distribueras inom ett virtuellt nätverk kan ansluta toohello internet. Varje distribuerad i Azure-Molntjänsten har ett offentligt adresserbara VIP tilldelade tooit. Har du toodefine inkommande slutpunkter för PaaS-roller och slutpunkter för virtuella datorer tooenable dessa tjänster tooaccept anslutningar från hello internet.

### <a name="do-vnets-support-ipv6"></a>Vnet som har stöd för IPv6?
Nej. Du kan inte använda IPv6 med VNets just nu.

### <a name="can-a-vnet-span-regions"></a>Ett virtuellt nätverk kan omfatta regioner?
Nej. Ett virtuellt nätverk är begränsad tooa enskild region.

### <a name="can-i-connect-a-vnet-tooanother-vnet-in-azure"></a>Kan jag ansluta en VNet tooanother VNet i Azure?
Ja. Du kan ansluta ett virtuellt nätverk tooanother VNet med:
- En Azure VPN-Gateway. Läs hello [konfigurera VNet-till-VNet-anslutningen](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) artikeln för information. 
- VNet-peering. Läs hello [VNet-peering översikt](virtual-network-peering-overview.md) artikeln för information.

## <a name="name-resolution-dns"></a>Namnmatchning (DNS)

### <a name="what-are-my-dns-options-for-vnets"></a>Vad är DNS-alternativ för Vnet?
Använd hello beslutstabell på hello [namnmatchning för virtuella datorer och Rollinstanser](virtual-networks-name-resolution-for-vms-and-role-instances.md) sidan tooguide igenom hello DNS alternativ tillgängliga.

### <a name="can-i-specify-dns-servers-for-a-vnet"></a>Kan jag ange DNS-servrar för ett virtuellt nätverk?
Ja. Du kan ange IP-adresser för DNS-servern i hello VNet-inställningarna. Detta används som hello standard-DNS-servrar för alla virtuella datorer i hello virtuella nätverk.

### <a name="how-many-dns-servers-can-i-specify"></a>Hur många DNS-servrar kan ange?
Referens hello [Azure begränsar](../azure-subscription-service-limits.md#networking-limits) artikeln för information.

### <a name="can-i-modify-my-dns-servers-after-i-have-created-hello-network"></a>Kan jag ändra Mina DNS-servrar när jag har skapat hello nätverk?
Ja. Du kan ändra hello DNS-serverlistan för din VNet när som helst. Om du ändrar DNS-serverlistan måste toorestart hello virtuella datorer i ditt virtuella nätverk för att de toopick in hello nya DNS-server.

### <a name="what-is-azure-provided-dns-and-does-it-work-with-vnets"></a>Vad är Azure-tillhandahållna DNS och fungerar det med VNets?
Azure-tillhandahållna DNS är en DNS-tjänsten för flera innehavare som erbjuds av Microsoft. Azure registrerar alla virtuella datorer och molnet tjänstinstanser roll i den här tjänsten. Den här tjänsten tillhandahåller namnmatchning av värdnamn för virtuella datorer och rollinstanser som ingår i hello samma molntjänst och hello av FQDN för virtuella datorer och rollinstanser i samma virtuella nätverk. Läs hello [namnmatchning för virtuella datorer och Rollinstanser](virtual-networks-name-resolution-for-vms-and-role-instances.md) artikel toolearn mer om DNS.

> [!NOTE]
> Det finns en begränsning på den här gången toohello först 100 molntjänster i ett virtuellt nätverk för flera innehavare namnmatchning med hjälp av Azure-tillhandahållna DNS. Den här begränsningen gäller inte om du använder DNS-servern.
> 
> 

### <a name="can-i-override-my-dns-settings-on-a-per-vm--service-basis"></a>Kan jag åsidosätta DNS-inställningarna på en per VM / service bas?
Ja. Du kan ange DNS-servrar på en per moln service bas toooverride hello standardinställningarna för nätverk. Vi rekommenderar dock att du använder över hela nätverket DNS så mycket som möjligt.

### <a name="can-i-bring-my-own-dns-suffix"></a>Kan jag använda min egen DNS-suffix?
Nej. Du kan inte ange en anpassad DNS-suffix för din Vnet.

## <a name="connecting-virtual-machines"></a>Ansluta virtuella datorer

### <a name="can-i-deploy-vms-tooa-vnet"></a>Kan jag distribuera virtuella datorer tooa VNet?
Ja. Alla nätverk gränssnitt (NIC) som är anslutna tooa VM distribueras via hello Resource Manager-modellen måste vara anslutna tooa VNet. Virtuella datorer som distribueras via hello klassiska distributionsmodellen kan du kan också vara anslutna tooa VNet.

### <a name="what-are-hello-different-types-of-ip-addresses-i-can-assign-toovms"></a>Vad är hello olika typer av IP-adresser I tilldela tooVMs?
* **Privat:** tilldelade tooeach nätverkskort på varje virtuell dator. hello-adress har tilldelats med hjälp av antingen hello statisk eller dynamisk metod. Privata IP-adresser tilldelas från hello-intervall som du angav i hello undernätsinställningar för ditt VNet. Resurser som distribueras via hello klassiska distributionsmodellen tilldelas privata IP-adresser, även om de inte är ansluten tooa VNet. En privat IP-adress med hello dynamisk metod förblir tilldelade tooa resursen tills hello resurs tas bort (virtuella datorer eller tjänst i molnet distributionsplatser). En privat IP-adress med hello dynamisk metod kan ändras när en virtuell dator startas om efter att ha i hello stoppats (frigjorts) tillstånd. En privat IP-adress med hello statisk metod förblir tilldelade tooa resurs tills hello resursen tas bort. Om du behöver tooensure att hello privata IP-adress för en resurs ändras aldrig tills hello resurs tas bort, tilldela en privat IP-adress med hello statisk metod.
* **Publik:** eventuellt tilldelade tooNICs kopplade tooVMs distribueras via hello Azure Resource Manager-distributionsmodellen. hello-adress kan tilldelas med hello statisk eller dynamisk tilldelningsmetod. Alla virtuella datorer och molntjänster rollinstanser som distribuerats via hello klassiska distributionsmodellen finns i en molntjänst som är tilldelade en *dynamisk*, offentliga virtuella IP-adressen. En offentlig *Statiska* IP-adress, som kallas en [reserverad IP-adress](virtual-networks-reserved-public-ip.md), kan också tilldelas som en VIP. Du kan tilldela offentliga IP-adresser tooindividual virtuella datorer eller molntjänster rollinstanser som distribuerats via hello klassiska distributionsmodellen. Dessa adresser kallas [nivå offentliga IP-instans (går](virtual-networks-instance-level-public-ip.md) adresser och kan tilldelas dynamiskt.

### <a name="can-i-reserve-a-private-ip-address-for-a-vm-that-i-will-create-at-a-later-time"></a>Kan jag reservera en privat IP-adress för en virtuell dator som jag skapar vid ett senare tillfälle?
Nej. Du kan inte reservera en privat IP-adress. Om en privat IP-adress är tillgänglig tilldelas den tooa VM eller roll instans av hello DHCP-server. Att VM kan eller kan inte vara hello något som du vill hello privata IP-adress toobe tilldelats. Du kan dock ändra hello privata IP-adressen för en redan har skapat VM tooany tillgänglig privat IP-adress.

### <a name="do-private-ip-addresses-change-for-vms-in-a-vnet"></a>Gör privat förändring av IP-adresser för virtuella datorer i ett virtuellt nätverk?
Det beror på. Dynamisk privata IP-adresser fortfarande med en virtuell dator till dess Stoppad (frigjord) eller tas bort. Statisk privat IP-adresser inte är publicerat från en virtuell dator tills det tas bort.

### <a name="can-i-manually-assign-ip-addresses-toonics-within-hello-vm-operating-system"></a>Kan jag manuellt tilldela IP-adresser tooNICs inom hello VM operativsystem?
Ja, men det rekommenderas inte. Manuellt ändra hello IP-adress till ett nätverkskort i en virtuell dators operativsystem eventuellt kan orsaka toolosing anslutning toohello VM om hello IP-adress som tilldelats tooa NIC inom hello Azure VM toochange.

### <a name="what-happens-toomy-ip-addresses-if-i-stop-a-cloud-service-deployment-slot-or-shutdown-a-vm-from-within-hello-operating-system"></a>Vad händer toomy IP-adresser om jag stoppa en molntjänst distributionsplatsen eller avstängning för en virtuell dator i hello operativsystem?
Det finns inget. hello IP-adresser (offentlig VIP, offentliga och privata) kvar tilldelade toohello cloud service distributionsplatsen eller VM. Dynamiska adresser släpps endast om en virtuell dator har stoppats (frigjorts) eller tagits bort, eller en distributionsplats i molnet tjänsten tas bort. Klicka på hello **stoppa** knappen för en virtuell dator i hello Azure-portalen anger dess tillstånd tooStopped (frigjord). I det här fallet förlorar hello VM sin IP-adresser.

### <a name="can-i-move-vms-from-one-subnet-tooanother-subnet-in-a-vnet-without-re-deploying"></a>Kan jag flytta virtuella datorer från ett undernät tooanother undernät i ett virtuellt nätverk utan att distribuera igen?
Ja. Du hittar mer information i hello [hur toomove en virtuell dator eller en roll instansen tooa annat undernät](virtual-networks-move-vm-role-to-subnet.md) artikel.

### <a name="can-i-configure-a-static-mac-address-for-my-vm"></a>Kan jag konfigurera en statisk MAC-adress för den virtuella datorn?
Nej. En MAC-adress kan inte vara statiskt konfigurerade.

### <a name="will-hello-mac-address-remain-hello-same-for-my-vm-once-it-has-been-created"></a>Hello MAC-adressen förblir hello samma för den virtuella datorn när den har skapats?
Ja, hello MAC-adressen förblir hello samma för en virtuell dator distribueras via både hello Resource Manager och klassiska distributionsmodeller tills det tas bort. Tidigare släpptes hello MAC-adress om hello VM har stoppats (frigjorts), men nu hello MAC-adressen sparas även om hello VM finns i hello frigjorts tillstånd.

### <a name="can-i-connect-toohello-internet-from-a-vm-in-a-vnet"></a>Kan jag ansluta toohello internet från en virtuell dator i ett virtuellt nätverk?
Ja. Alla virtuella datorer och molntjänster rollinstanser som distribuerats i ett virtuellt nätverk kan ansluta toohello Internet.

## <a name="azure-services-that-connect-toovnets"></a>Azure-tjänster som ansluter tooVNets

### <a name="can-i-use-azure-app-service-web-apps-with-a-vnet"></a>Kan jag använda Azure App Service Web Apps med ett virtuellt nätverk?
Ja. Du kan distribuera webbprogram i ett VNet med en ASE (Apptjänstmiljö). Alla webbprogram kan på ett säkert sätt ansluta och komma åt resurser i ditt virtuella Azure-om du har en punkt-till-plats-anslutning som konfigurerats för ditt VNet. Mer information finns i följande artiklar hello:

* [Skapa Webbappar i Apptjänst-miljö](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md)
* [Integrera din app med Azure-nätverk](../app-service-web/web-sites-integrate-with-vnet.md)
* [Med Web Apps VNet-integrering och Hybridanslutningar](../app-service-web/web-sites-integrate-with-vnet.md#hybrid-connections-and-app-service-environments)

### <a name="can-i-deploy-cloud-services-with-web-and-worker-roles-paas-in-a-vnet"></a>Kan jag distribuera molntjänster med webb- och arbetsroller roller (PaaS) i ett virtuellt nätverk?
Ja. (Valfritt) kan du distribuera molntjänster rollinstanser inom Vnet. toodo så anger du hello VNet namn och hello roll /-undernätet mappningar i hello nätverket konfigurationsavsnittet för tjänstkonfigurationen av. Du behöver inte tooupdate någon av dina binärfiler.

### <a name="can-i-connect-a-virtual-machine-scale-set-vmss-tooa-vnet"></a>Kan jag ansluta en virtuell dator skala ange (VMSS) tooa VNet?
Ja. Du måste ansluta en VMSS tooa VNet.

### <a name="can-i-move-my-services-in-and-out-of-vnets"></a>Kan jag flytta Mina tjänster till och från Vnet?
Nej. Du kan inte flytta tjänster till och från Vnet. Du har toodelete och omdistribuera hello service toomove den tooanother VNet.

## <a name="security"></a>Säkerhet

### <a name="what-is-hello-security-model-for-vnets"></a>Vad är hello säkerhetsmodell för Vnet?
Vnet är helt isolerade från varandra och andra tjänster som finns i hello Azure-infrastrukturen. Ett virtuellt nätverk är en förtroendegräns.

### <a name="can-i-restrict-inbound-or-outbound-traffic-flow-toovnet-connected-resources"></a>Kan jag begränsa inkommande eller utgående datatrafik flödet tooVNet-anslutna resurser?
Ja. Du kan använda [Nätverkssäkerhetsgrupper](virtual-networks-nsg.md) tooindividual undernät inom ett VNet nätverkskort kopplas tooa VNet eller båda.

### <a name="can-i-implement-a-firewall-between-vnet-connected-resources"></a>Kan jag implementera en brandvägg mellan VNet-anslutna resurser?
Ja. Du kan distribuera en [brandväggen virtuell nätverksenhet](https://azure.microsoft.com/en-us/marketplace/?term=firewall) från flera leverantörer via hello Azure Marketplace.

### <a name="is-there-information-available-about-securing-vnets"></a>Finns det information om hur du skyddar Vnet?
Ja. Se hello [översikt över Azure Network Security](../security/security-network-overview.md) artikeln för information.

## <a name="apis-schemas-and-tools"></a>API: er, scheman och verktyg

### <a name="can-i-manage-vnets-from-code"></a>Kan jag hantera Vnet från kod?
Ja. Du kan använda REST API: er för VNets i hello [Azure Resource Manager](https://msdn.microsoft.com/library/mt163658.aspx) och [klassisk (servicehantering)](http://go.microsoft.com/fwlink/?LinkId=296833)) distributionsmodeller.

### <a name="is-there-tooling-support-for-vnets"></a>Finns det verktygsuppsättning stöd för Vnet?
Ja. Lär dig mer om hur du använder:
- hello Azure portal toodeploy Vnet via hello [Azure Resource Manager](virtual-networks-create-vnet-arm-pportal.md) och [klassiska](virtual-networks-create-vnet-classic-pportal.md) distributionsmodeller.
- PowerShell toomanage Vnet distribueras via hello [Resource Manager](/powershell/resourcemanager/azurerm.network/v3.1.0/azurerm.network.md) och [klassiska](/powershell/module/azure/?view=azuresmps-3.7.0) distributionsmodeller.
- Hej [Azure-kommandoradsgränssnittet (CLI)](../virtual-machines/azure-cli-arm-commands.md#azure-network-commands-to-manage-network-resources) toomanage Vnet som distribueras via båda distributionsmodellerna.  
