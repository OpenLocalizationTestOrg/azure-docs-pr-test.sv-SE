---
title: "aaaAzure virtuella nätverk (VNet) planerings- och designguide | Microsoft Docs"
description: "Lär dig hur tooplan och design virtuella nätverk i Azure baserat på dina krav för isolering, anslutning och plats."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: 3a4a9aea-7608-4d2e-bb3c-40de2e537200
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/08/2016
ms.author: jdial
ms.openlocfilehash: f3ffadf8cf254f64b1f86b44f90315d2bc679f63
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="plan-and-design-azure-virtual-networks"></a>Planera och utforma Azure-nätverk
Det är enkelt att skapa en VNet tooexperiment med, men troligen har du ska distribuera flera virtuella nätverk över tid toosupport hello produktion behoven för din organisation. Med lite planering och design, kommer du att kunna toodeploy Vnet och ansluter hello-resurser som du behöver mer effektivt. Om du inte är bekant med VNets rekommenderar vi som du [Lär dig mer om Vnet](virtual-networks-overview.md) och [hur toodeploy](virtual-networks-create-vnet-arm-pportal.md) något innan du fortsätter.

## <a name="plan"></a>Planera
Det är viktigt för lyckad goda kunskaper om Azure-prenumerationer, regioner och nätverksresurser. Du kan använda hello listan med överväganden nedan som utgångspunkt. När du förstår de överväganden kan definiera du hello kraven för din nätverksdesign.

### <a name="considerations"></a>Överväganden
Överväg följande hello innan samtalet besvaras hello Planeringsfrågor nedan:

* Allt som du skapar i Azure består av en eller flera resurser. En virtuell dator (VM) är en resurs, hello nätverkskortet (NIC) används av en virtuell dator är en resurs, hello offentliga IP-adress som används av ett nätverkskort är en resurs, hello VNet hello nätverkskortet är anslutet toois en resurs.
* Du skapar resurser inom en [Azure-region](https://azure.microsoft.com/regions/#services) och prenumeration. Resurser kan bara vara anslutna tooa virtuella nätverk som finns i hello samma region och prenumeration hello-resursen.
* Du kan ansluta virtuella nätverk tooeach andra genom att använda:
    * **[Virtuellt nätverk peering](virtual-network-peering-overview.md)**: hello virtuella nätverk måste finnas i hello samma Azure-region. Bandbredden mellan resurser i peerkoppla virtuella nätverk är hello samma som om hello resurserna var anslutna toohello samma virtuella nätverk.
    * **En Azure [VPN-Gateway](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)**: hello virtuella nätverk kan finnas i hello samma eller olika Azure-regioner. Bandbredden mellan resurser i virtuella nätverk som är anslutna via en VPN-Gateway begränsas av hello bandbredden för hello VPN-Gateway.
* Du kan ansluta Vnet tooyour lokalt nätverk genom att använda en hello [anslutningsalternativ](../vpn-gateway/vpn-gateway-about-vpngateways.md#s2smulti) tillgängliga i Azure.
* Olika resurser kan grupperas i [resursgrupper](../azure-resource-manager/resource-group-overview.md#resource-groups), vilket gör det enklare toomanage hello resurs som en enhet. En resursgrupp kan innehålla resurser från flera områden, så länge hello resurser tillhör toohello samma prenumeration.

### <a name="define-requirements"></a>Definiera krav
Använd hello frågor nedan som utgångspunkt för utformningen av din Azure-nätverk.    

1. Vad Azure platser ska du använda toohost Vnet?
2. Behöver du tooprovide kommunikation mellan dessa Azure platser?
3. Behöver du tooprovide kommunikation mellan din Azure-VNet(s) och din lokala datacenter(s)?
4. Hur många infrastruktur som en tjänst (IaaS) virtuella datorer, cloud services-roller och web apps gör du behöver för din lösning?
5. Behöver du tooisolate trafik baserat på grupper med virtuella datorer (d.v.s. frontend-webbservrar och backend-databasservrar)?
6. Behöver du toocontrol trafikflöde använder virtuella installationer?
7. Göra användare behöver olika uppsättningar av behörigheter toodifferent Azure-resurser?

### <a name="understand-vnet-and-subnet-properties"></a>Förstå VNet och undernät egenskaper
VNet och undernät resurser hjälpa dig att definiera en säkerhetsgräns för arbetsbelastningar som körs i Azure. Ett virtuellt nätverk kännetecknas av en samling adressutrymmen, definierad som CIDR-block.

> [!NOTE]
> Nätverksadministratörer är bekanta med CIDR-notering. Om du inte är bekant med CIDR, [mer information om den](http://whatismyipaddress.com/cidr).
>
>

Vnet innehålla hello följande egenskaper.

| Egenskap | Beskrivning | Villkor |
| --- | --- | --- |
| **Namn** |Namn på virtuella nätverk |Textsträng in too80 tecken. Kan innehålla bokstäver, siffror, understreck, punkter eller bindestreck. Måste börja med en bokstav eller en siffra. Måste sluta med en bokstav, en siffra eller understreck. Kan innehåller gemener och versaler. |
| **Plats** |Azure-plats (även hänvisade tooas region). |Måste vara en hello Azure giltiga platser. |
| **addressSpace** |Samling av adressprefix som utgör hello VNet i CIDR-notation. |Måste vara en matris med giltig CIDR-Adressblock, inklusive offentliga IP-adressintervall. |
| **undernät** |Samling av undernät som utgör hello VNet |Se hello undernät egenskaper tabellen nedan. |
| **dhcpOptions** |Objekt som innehåller en obligatorisk egenskap med namnet **dnsServers**. | |
| **dnsServers** |Matris för DNS-servrar som används av hello virtuella nätverk. Om ingen server har angetts används intern namnmatchning för Azure. |Måste vara en matris med upp too10 DNS-servrar efter IP-adress. |

Ett undernät är en underordnad resurs i ett VNet och hjälper dig att definiera segmenten i adressutrymmen i CIDR-block med IP-adressprefix. Nätverkskort läggas toosubnets och anslutna tooVMs som man har anslutning för olika arbetsbelastningar.

Undernät innehålla hello följande egenskaper.

| Egenskap | Beskrivning | Villkor |
| --- | --- | --- |
| **Namn** |Namn på undernät |Textsträng in too80 tecken. Kan innehålla bokstäver, siffror, understreck, punkter eller bindestreck. Måste börja med en bokstav eller en siffra. Måste sluta med en bokstav, en siffra eller understreck. Kan innehåller gemener och versaler. |
| **Plats** |Azure-plats (även hänvisade tooas region). |Måste vara en hello Azure giltiga platser. |
| **addressPrefix** |Enkel adressprefixet som utgör hello undernät i CIDR-notation |Måste vara ett enda CIDR-block som ingår i någon av hello VNet-adressutrymmen. |
| **networkSecurityGroup** |NSG tillämpas toohello undernät | |
| **Migreringstillståndet** |Routningstabellen tillämpas toohello undernät | |
| **ipConfigurations** |Samling av IP-konfigurationsobjekt som används av nätverkskort anslutna toohello undernät | |

### <a name="name-resolution"></a>Namnmatchning
Som standard används ditt VNet [Azure-tillhandahållna namnmatchning](virtual-networks-name-resolution-for-vms-and-role-instances.md) tooresolve namnen i hello VNet och på hello offentliga Internet. Om du ansluter din Vnet tooyour lokalt datacenter, måste du dock tooprovide [DNS-servern](virtual-networks-name-resolution-for-vms-and-role-instances.md) tooresolve namn mellan ditt nätverk.  

### <a name="limits"></a>Begränsningar
Granska hello nätverk gränser i hello [Azure begränsar](../azure-subscription-service-limits.md#networking-limits) artikel tooensure som din design inte står i konflikt med någon av hello gränser. Vissa begränsningar kan utökas genom att öppna ett supportärende.

### <a name="role-based-access-control-rbac"></a>Rollbaserad åtkomstkontroll (RBAC)
Du kan använda [Azure RBAC](../active-directory/role-based-access-built-in-roles.md) toocontrol hello andelen åtkomst olika användare kan ha toodifferent resurser i Azure. På så sätt kan du särskilja hello arbete som utförs av ditt team utifrån deras behov.

När det gäller de virtuella nätverken är berörda användare i hello **Network-deltagare** roll har full kontroll över Azure Resource Manager nätverksresurser på virtuella datorer. På liknande sätt kan användare i hello **klassiska Network-deltagare** roll har full kontroll över klassiska virtuella nätverksresurser.

> [!NOTE]
> Du kan också [skapa egna roller](../active-directory/role-based-access-control-configure.md) tooseparate dina administrativa behov.
>
>

## <a name="design"></a>Designa
När du väl vet svaren hello toohello frågor i hello [planera](#Plan) avsnittet kan du granska hello följande innan du definierar ditt Vnet.

### <a name="number-of-subscriptions-and-vnets"></a>Antalet prenumerationer och Vnet
Du bör överväga att skapa flera Vnet i hello följande scenarier:

* **Virtuella datorer som behöver toobe placeras i olika Azure platser**. Vnet i Azure är regionala. De kan sträcka sig över platser. Därför behöver du minst ett virtuellt nätverk för varje Azure-plats som du vill ha toohost virtuella datorer.
* **Arbetsbelastningar som behöver toobe helt isolerade från varandra**. Du kan skapa separata Vnet, som även används hello samma IP-adressutrymmen tooisolate olika arbetsbelastningar från varandra.

Tänk på att hello gränserna som du ser ovan är per region per prenumeration. Det innebär att du kan använda flera prenumerationer tooincrease hello gränsen på resurser som du kan upprätthålla i Azure. Du kan använda en plats-till-plats-VPN eller en ExpressRoute-krets tooconnect Vnet för olika prenumerationer.

### <a name="subscription-and-vnet-design-patterns"></a>Prenumerationen och designmönster för virtuella nätverk
hello i tabellen nedan visas några vanliga designmönster för prenumerationer och Vnet.

| Scenario | Diagram | Tekniker | Nackdelar |
| --- | --- | --- | --- |
| En prenumeration, två Vnet per program |![Enda prenumeration](./media/virtual-network-vnet-plan-design-arm/figure1.png) |Endast en prenumeration toomanage. |Maximalt antal Vnet per Azure-region. Du behöver flera prenumerationer efter. Granska hello [Azure begränsar](../azure-subscription-service-limits.md#networking-limits) artikeln för information. |
| En prenumeration per app, två Vnet per program |![Enda prenumeration](./media/virtual-network-vnet-plan-design-arm/figure2.png) |Använder bara två Vnet per prenumeration. |Svårare toomanage när det finns för många appar. |
| En prenumeration per affärsenhet, två Vnet per program. |![Enda prenumeration](./media/virtual-network-vnet-plan-design-arm/figure3.png) |Balansera mellan antalet prenumerationer och virtuella nätverk. |Maximalt antal Vnet per affärsenhet (prenumeration). Granska hello [Azure begränsar](../azure-subscription-service-limits.md#networking-limits) artikeln för information. |
| En prenumeration per affärsenhet, två Vnet per grupp av appar. |![Enda prenumeration](./media/virtual-network-vnet-plan-design-arm/figure4.png) |Balansera mellan antalet prenumerationer och virtuella nätverk. |Appar måste separat med hjälp av undernät och NSG: er. |

### <a name="number-of-subnets"></a>Antalet undernät
Du bör flera undernät i ett VNet i hello följande scenarier:

* **Det finns inte tillräckligt med privata IP-adresser för alla nätverkskort i ett undernät**. Om din undernätsadressutrymmet inte innehåller tillräckligt med IP-adresser för hello antal nätverkskort i hello undernät, måste toocreate flera undernät. Tänk på att Azure reserverar 5 privata IP-adresser från varje undernät som inte kan användas: hello första och sista adresser för hello adressutrymme (för hello undernätsadress och multicast) och 3 adresser toobe används internt (för DHCP och DNS).
* **Säkerhet**. Du kan använda undernät tooseparate grupper med virtuella datorer från varandra för arbetsbelastningarna som har ett flera lager struktur och tillämpa olika [nätverkssäkerhetsgrupper (NSG: er)](virtual-networks-nsg.md#subnets) för dessa undernät.
* **Hybridanslutning**. Du kan använda VPN-gateway och ExpressRoute-kretsar för[ansluta](../vpn-gateway/vpn-gateway-about-vpngateways.md#s2smulti) Vnet-tooone en annan, och tooyour lokala data center(s). VPN-gatewayer och ExpressRoute-kretsar kräver ett undernät för sina egna toobe skapas.
* **Virtuella installationer**. Du kan använda en virtuell installation, till exempel en brandvägg, WAN accelerator eller VPN-gateway i ett Azure VNet. När du gör det måste för[dirigera trafik](virtual-networks-udr-overview.md) toothose installationer och isolera dem i sina egna undernät.

### <a name="subnet-and-nsg-design-patterns"></a>Designmönster för undernät och NSG
hello i tabellen nedan visas några vanliga designmönster för att använda undernät.

| Scenario | Diagram | Tekniker | Nackdelar |
| --- | --- | --- | --- |
| Enskilt undernät, NSG: er per programnivå per program |![Enskilt undernät](./media/virtual-network-vnet-plan-design-arm/figure5.png) |Endast ett undernät toomanage. |Flera NSG: er nödvändiga tooisolate varje program. |
| Ett undernät per app, NSG: er per program lager |![Undernät per program](./media/virtual-network-vnet-plan-design-arm/figure6.png) |Färre NSG: er toomanage. |Flera undernät toomanage. |
| Ett undernät per programnivå NSG: er per app. |![Undernät per lager](./media/virtual-network-vnet-plan-design-arm/figure7.png) |Balansera mellan antalet undernät och NSG: er. |Maximalt antal NSG: er per prenumeration. Granska hello [Azure begränsar](../azure-subscription-service-limits.md#networking-limits) artikeln för information. |
| Ett undernät per programnivå per app, NSG: er per undernät |![Undernät per lager per program](./media/virtual-network-vnet-plan-design-arm/figure8.png) |Eventuellt mindre antal NSG: er. |Flera undernät toomanage. |

## <a name="sample-design"></a>Exempel design
tooillustrate hello programmet hello information i den här artikeln bör du hello följande scenario.

Du arbetar för ett företag som har 2 datacenter i Nordamerika och två Datacenter Europa. Du har identifierat 6 olika kundinriktade program som underhålls av 2 olika affärsenheter som du vill toomigrate tooAzure som en pilot. hello grundläggande arkitektur för hello program är följande:

* App1, App2, App3 och App4 är webbprogram på Linux-servrar som kör Ubuntu. Varje program ansluter tooa separat programserver som är värd för RESTful-tjänster på Linux-servrar. hello RESTful-tjänster ansluta tooa serverdel MySQL-databas.
* App5 och App6 är webbprogram som finns på Windows-servrar som kör Windows Server 2012 R2. Varje program ansluter tooa serverdel SQL Server-databas.
* Alla appar som finns för närvarande i något av hello företagets datacenter i Nordamerika.
* hello lokalt Datacenter använder hello 10.0.0.0/8 adressutrymme.

Du behöver toodesign en lösning för virtuella nätverk som uppfyller hello följande krav:

* Varje affärsenhet bör inte påverkas av resurser som används för andra enheter.
* Du bör minimera hello för Vnet och undernät toomake enklare.
* Varje affärsenhet ska ha en enda test/utveckling VNet som används för alla program.
* Varje program finns i 2 olika Azure-Datacenter per kontinent (Nordamerika och Europa).
* Varje program är helt isolerade från varandra.
* Varje program kan nås av kunder över hello Internet med HTTP.
* Varje program kan användas av användare anslutna toohello lokala datacenter med hjälp av en krypterad tunnel.
* Anslutningen tooon lokala Datacenter ska använda den befintliga VPN-enheter.
* hello företagets nätverk gruppen ska ha fullständig kontroll över hello konfiguration av virtuellt nätverk.
* Utvecklare i varje affärsenhet bara ska kunna toodeploy VMs tooexisting undernät.
* Att kommer migreras alla program som de är tooAzure (lift och SKIFT).
* hello-databaser på varje plats ska replikera tooother Azure platser en gång om dagen.
* Varje program bör använda 5 frontend-webbservrar, 2 programservrar (om det behövs) och 2-databasservrar.

### <a name="plan"></a>Planera
Starta din design planera genom att besvara frågan hello i hello [definiera krav](#Define-requirements) enligt nedan.

1. Vad Azure platser ska du använda toohost Vnet?

    2 platser i Nordamerika och Europa 2 platser. Du bör välja de baserade på hello fysiska platsen för din befintliga lokala datacenter. På så sätt anslutningen från din fysiska platser tooAzure har bättre svarstid.
2. Behöver du tooprovide kommunikation mellan dessa Azure platser?

    Ja. Eftersom hello databaser måste vara replikerade tooall platser.
3. Behöver du tooprovide kommunikation mellan din Azure-VNet(s) och dina lokala data center(s)?

    Ja. Eftersom användarna anslutna måste toohello lokalt Datacenter vara kan tooaccess hello program via en krypterad tunnel.
4. Hur många virtuella IaaS-datorer behöver för din lösning?

    200 IaaS-VM. App1, App2, App3 och App4 kräver 5 webbservrar som varje, 2 programservrarna som varje och 2-databasservrar. Det är totalt 9 virtuella IaaS-datorer per program eller 36 IaaS-VM. App5 och App6 kräver 5 webbservrar och 2-databasservrar. Det är totalt 7 virtuella IaaS-datorer per program eller 14 IaaS-VM. Du måste därför 50 IaaS-VM för alla program i varje Azure-region. Eftersom vi behöver toouse 4 regioner, kan det finnas 200 IaaS-VM.

    Du måste också tooprovide DNS-servrar i varje virtuellt nätverk eller i ditt lokala data Datacenter tooresolve namn mellan Azure IaaS-VM och ditt lokala nätverk.
5. Behöver du tooisolate trafik baserat på grupper med virtuella datorer (d.v.s. frontend-webbservrar och backend-databasservrar)?

    Ja. Varje program bör vara helt isolerade från varandra och varje program lager ska också isoleras.
6. Behöver du toocontrol trafikflöde använder virtuella installationer?

    Nej. Virtuella installationer kan vara används tooprovide mer kontroll över trafikflödet, inklusive mer detaljerad loggning av data plan.
7. Göra användare behöver olika uppsättningar av behörigheter toodifferent Azure-resurser?

    Ja. hello nätverk team måste fullständig kontroll på hello virtuella nätverksinställningar, medan utvecklare bara ska kunna toodeploy deras befintliga toopre undernät, virtuella datorer.

### <a name="design"></a>Designa
Du bör följa hello design anger prenumerationer, Vnet, undernät och NSG: er. Diskuteras NSG: er här, men du får lära dig mer om [NSG: er](virtual-networks-nsg.md) innan du avslutar din design.

**Antalet prenumerationer och Vnet**

hello följande krav är relaterade toosubscriptions och Vnet:

* Varje affärsenhet bör inte påverkas av resurser som används för andra enheter.
* Du bör minimera hello att Vnet och undernät.
* Varje affärsenhet ska ha en enda test/utveckling VNet som används för alla program.
* Varje program finns i 2 olika Azure-Datacenter per kontinent (Nordamerika och Europa).

Baserat på dessa krav, behöver du en prenumeration för varje affärsenhet. På så sätt kan förbrukningen av resurser från en affärsenhet inte räknas som en gränserna för andra enheter. Och eftersom du vill toominimize hello antalet Vnet, bör du använda hello **en prenumeration per affärsenhet, två Vnet per grupp appar** mönstret som du ser nedan.

![Enda prenumeration](./media/virtual-network-vnet-plan-design-arm/figure9.png)

Du måste också toospecify hello-adressutrymmet för varje virtuellt nätverk. Eftersom du behöver anslutningen mellan hello lokalt Datacenter hello Azure-regioner hello-adressutrymmet som används för Azure Vnet kan inte vara i konflikt med hello lokala nätverk och hello-adressutrymmet som används av varje virtuellt nätverk får inte vara i konflikt med andra befintliga Vnet. Du kan använda hello-adressutrymmen i hello tabellen nedan toosatisfy dessa krav.  

| **Prenumeration** | **Virtuella nätverk** | **Azure-region** | **Adressutrymme** |
| --- | --- | --- | --- |
| BU1 |ProdBU1US1 |Västra USA |172.16.0.0/16 |
| BU1 |ProdBU1US2 |Östra USA |172.17.0.0/16 |
| BU1 |ProdBU1EU1 |Norra Europa |172.18.0.0/16 |
| BU1 |ProdBU1EU2 |Västra Europa |172.19.0.0/16 |
| BU1 |TestDevBU1 |Västra USA |172.20.0.0/16 |
| BU2 |TestDevBU2 |Västra USA |172.21.0.0/16 |
| BU2 |ProdBU2US1 |Västra USA |172.22.0.0/16 |
| BU2 |ProdBU2US2 |Östra USA |172.23.0.0/16 |
| BU2 |ProdBU2EU1 |Norra Europa |172.24.0.0/16 |
| BU2 |ProdBU2EU2 |Västra Europa |172.25.0.0/16 |

**Antalet undernät och NSG: er**

hello följande krav är relaterade toosubnets och NSG: er:

* Du bör minimera hello att Vnet och undernät.
* Varje program är helt isolerade från varandra.
* Varje program kan nås av kunder över hello Internet med HTTP.
* Varje program kan användas av användare anslutna toohello lokala datacenter med hjälp av en krypterad tunnel.
* Anslutningen tooon lokala Datacenter ska använda den befintliga VPN-enheter.
* hello-databaser på varje plats ska replikera tooother Azure platser en gång om dagen.

Baserat på dessa krav, kan du använda ett undernät per programnivå och använder NSG: er toofilter trafik per program. På så sätt kan du bara ha 3 undernät i varje virtuellt nätverk (klientdel programnivå och dataskiktet) och en NSG per program per undernät. I det här fallet bör du använda hello **ett undernät per programnivå NSG: er per app** designmönstret. hello bilden nedan visar hello användning av hello designmönstret som representerar hello **ProdBU1US1** VNet.

![Ett undernät per lager, en NSG per program per lager](./media/virtual-network-vnet-plan-design-arm/figure11.png)

Men måste du också toocreate ett extra undernät för hello VPN-anslutning mellan hello Vnet och ditt lokala datacenter. Och du behöver toospecify hello-adressutrymmet för varje undernät. hello bilden nedan visar ett exempel lösning för **ProdBU1US1** VNet. Du kan replikera det här scenariot för varje virtuellt nätverk. Varje färg motsvarar ett annat program.

![Exempel VNet](./media/virtual-network-vnet-plan-design-arm/figure10.png)

**Åtkomstkontroll**

hello följande krav är relaterade tooaccess kontroll:

* hello företagets nätverk gruppen ska ha fullständig kontroll över hello konfiguration av virtuellt nätverk.
* Utvecklare i varje affärsenhet bara ska kunna toodeploy VMs tooexisting undernät.

Baserat på dessa krav, kan du lägga till användare från hello nätverk team toohello inbyggda **Network-deltagare** roll i varje prenumeration, och skapa en anpassad roll för hello programutvecklare i varje prenumeration som ger dem rättigheter tooadd VMs tooexisting undernät.

## <a name="next-steps"></a>Nästa steg
* [Distribuera ett virtuellt nätverk](virtual-networks-create-vnet-arm-template-click.md) baserat på ett scenario.
* Förstå hur för[belastningsutjämna](../load-balancer/load-balancer-overview.md) IaaS-VM och [hantera routning över flera Azure-regioner](../traffic-manager/traffic-manager-overview.md).
* Lär dig mer om [NSG: er och hur tooplan och design](virtual-networks-nsg.md) en NSG-lösning.
* Lär dig mer om din [mellan platser och VNet anslutningsalternativ](../vpn-gateway/vpn-gateway-about-vpngateways.md#s2smulti).
