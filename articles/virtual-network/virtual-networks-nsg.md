---
title: "aaaNetwork säkerhetsgrupper i Azure | Microsoft Docs"
description: "Lär dig hur tooisolate och kontroll trafiken inom dina virtuella nätverk som använder hello distribuerade brandväggen i Azure med Nätverkssäkerhetsgrupper."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
ms.assetid: 20e850fc-6456-4b5f-9a3f-a8379b052bc9
ms.service: virtual-network
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/11/2016
ms.author: jdial
ms.openlocfilehash: 3528ce833dab17977327c3c9ae0e78316e5e6a05
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="filter-network-traffic-with-network-security-groups"></a>Filtrera nätverkstrafik med nätverkssäkerhetsgrupper

En nätverkssäkerhetsgrupp (NSG) innehåller en lista över säkerhetsregler som tillåter eller nekar nätverket trafik tooresources anslutna tooAzure virtuella nätverk (VNet). NSG: er kan vara associerade toosubnets, enskilda virtuella datorer (klassisk) eller enskilda nätverksgränssnitt (NIC) kopplade tooVMs (Resource Manager). När en NSG är associerad tooa undernät, tillämpas hello reglerna tooall resurser anslutna toohello undernät. Trafik kan ytterligare begränsas av också koppla en NSG tooa VM eller nätverkskort.

> [!NOTE]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../resource-manager-deployment-model.md). Den här artikeln täcker bägge modellerna, men Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.

## <a name="nsg-resource"></a>NSG-resurs
NSG: er innehåller hello följande egenskaper:

| Egenskap | Beskrivning | Villkor | Överväganden |
| --- | --- | --- | --- |
| Namn |Namn för hello NSG |Måste vara unikt inom hello region.<br/>Kan innehålla bokstäver, siffror, understreck, punkter och bindestreck.<br/>Måste börja med en bokstav eller en siffra.<br/>Måste sluta med en bokstav, en siffra eller understreck.<br/>Får inte vara längre än 80 tecken. |Eftersom du kan behöva toocreate flera NSG: er, kontrollera att du har en namngivningskonvention som gör det enkelt tooidentify hello-funktionen för din NSG. |
| Region |Azure [region](https://azure.microsoft.com/regions) där hello NSG skapas. |NSG: er kan bara vara associerad tooresources inom hello samma region som hello NSG. |toolearn om hur många NSG: er som du kan ha per region, läsa hello [Azure begränsar](../azure-subscription-service-limits.md#virtual-networking-limits-classic) artikel.|
| Resursgrupp |Hej [resursgruppen](../azure-resource-manager/resource-group-overview.md#resource-groups) hello NSG finns i. |Även om en NSG finns i en resursgrupp, det kan vara associerade tooresources i valfri resursgrupp, så länge hello resursen är en del av hello samma Azure-region som hello NSG. |Resursgrupper är används toomanage flera resurser tillsammans som en distributionsenhet.<br/>Du kan gruppera hello NSG med resurser som den är kopplad till. |
| Regler |Inkommande eller utgående regler som definierar vilken trafik som tillåts eller nekas. | |Se hello [NSG-regler](#Nsg-rules) i den här artikeln. |

> [!NOTE]
> Slutpunktsbaserade ACL: er och nätverkssäkerhet grupper inte stöds på hello samma VM-instans. Om du vill toouse en NSG och redan har en slutpunkts-ACL på plats måste du först ta bort hello slutpunkts-ACL. toolearn hur tooremove en ACL läsa hello [hantera åtkomstkontrollistor (ACL) för slutpunkter med hjälp av PowerShell](virtual-networks-acl-powershell.md) artikel.
> 

### <a name="nsg-rules"></a>NSG-regler
NSG-regler har hello följande egenskaper:

| Egenskap | Beskrivning | Villkor | Överväganden |
| --- | --- | --- | --- |
| **Namn** |Namn för hello regel. |Måste vara unikt inom hello region.<br/>Kan innehålla bokstäver, siffror, understreck, punkter och bindestreck.<br/>Måste börja med en bokstav eller en siffra.<br/>Måste sluta med en bokstav, en siffra eller understreck.<br/>Får inte vara längre än 80 tecken. |Du kan ha flera regler inom en NSG, så se till att du följer en namngivningskonvention som gör att du tooidentify hello-funktionen för din regel. |
| **Protokoll** |Protokollet toomatch för hello regeln. |TCP, UDP eller * |Använda * som ett protokoll inkluderar ICMP (enbart öst-väst-trafik), som samt UDP och TCP och kan minska hello antalet regler du behöver.<br/>AT hello samma tid med * kanske för bred lösning, det rekommenderas att du använder * bara när det behövs. |
| **Källportintervall** |Källan port intervallet toomatch för hello regeln. |Enskilda portnummer mellan 1 too65535, portintervall (exempel: 1-65535), eller * (för alla portar). |Källportar kan vara tillfälliga. Såvida inte klientprogrammet använder en viss port bör du använda * i de flesta fall.<br/>Försök toouse portintervall så mycket som möjligt tooavoid hello behöver för flera regler.<br/>Flera portar eller portintervall kan inte grupperas med kommatecken. |
| **Målportintervall** |Mål-port intervallet toomatch för hello regeln. |Enskilda portnummer mellan 1 too65535, portintervall (exempel: 1-65535), eller \* (för alla portar). |Försök toouse portintervall så mycket som möjligt tooavoid hello behöver för flera regler.<br/>Flera portar eller portintervall kan inte grupperas med kommatecken. |
| **Källadress-prefix** |Källan adress prefix eller tagg toomatch för hello regeln. |Enskild IP-adress (exempel: 10.10.10.10), IP-undernät (exempel: 192.168.1.0/24), [standardtagg](#default-tags) eller * (för alla adresser). |Överväg att använda intervall, standardtaggar och * tooreduce hello antal regler. |
| **Måladress-prefix** |Mål-adress prefix eller tagg toomatch för hello regeln. | Enskild IP-adress (exempel: 10.10.10.10), IP-undernät (exempel: 192.168.1.0/24), [standardtagg](#default-tags) eller * (för alla adresser). |Överväg att använda intervall, standardtaggar och * tooreduce hello antal regler. |
| **Riktning** |Riktningen på trafik toomatch för hello regeln. |Inkommande eller utgående. |Regler för inkommande och utgående bearbetas separat, baserat på riktning. |
| **Prioritet** |Reglerna kontrolleras i hello prioritetsordning. När villkoren för en regel uppfylls testas inga fler regler. | Tal mellan 100 och 4096. | Överväg att skapa regler som hoppar prioritet med 100 för varje regel tooleave utrymme för nya regler som du kan skapa i hello framtida. |
| **Åtkomst** |Typ av åtkomst tooapply om hello regeln matchar. | Tillåt eller neka. | Tänk på att om en regel för Tillåt inte hittas för ett paket, släpps hello-paketet. |

Nätverkssäkerhetsgrupper innehåller två regeluppsättningar: inkommande och utgående. hello prioritet för en regel måste vara unika inom varje uppsättning. 

![NSG-regelbearbetning](./media/virtual-network-nsg-overview/figure3.png) 

hello föregående bild visar hur NSG-regler bearbetas.

### <a name="default-tags"></a>Standardtaggar
Standardtaggar är systemdefinierade identifierare tooaddress en kategori av IP-adresser. Du kan använda standardtaggar i hello **källadress-prefix** och **måladress-prefix** egenskaper för alla regler. Det finns tre standardtaggar som du kan använda:

* **VirtualNetwork** (Resource Manager) (**VIRTUAL_NETWORK** för klassisk): den här taggen innehåller hello virtuella nätverkets adressutrymme (CIDR-intervallen som definieras i Azure), alla anslutna lokala adressutrymmen och anslutna Azure Vnet (lokala nätverk).
* **AzureLoadBalancer** (Resource Manager) (**AZURE_LOADBALANCER** för klassisk): Den här taggen anger belastningsutjämnaren för Azures infrastruktur. hello taggen översätter tooan IP för Azure-datacenter där Azures-hälsoavsökning kommer.
* **Internet** (Resource Manager) (**INTERNET** för klassisk): den här taggen anger hello IP-adressutrymmet som är utanför hello virtuellt nätverk och kan nås via offentligt Internet. hello området inkluderar hello [Azure-ägt offentligt IP-adressutrymme](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="default-rules"></a>Standardregler
Alla NSG:er har en uppsättning standardregler. hello standardreglerna kan inte tas bort, men eftersom de har tilldelats lägst prioritet för hello, de kan åsidosättas med hello regler som du skapar. 

hello standardregler Tillåt och neka trafik på följande sätt:
- **Virtuellt nätverk:** Trafik som kommer från eller som går till ett virtuellt nätverk tillåts både i inkommande och utgående riktning.
- **Internet:** Utgående trafik tillåts, men inkommande trafik blockeras.
- **Belastningsutjämnare:** Tillåt Azure belastningen belastningsutjämnaren tooprobe hello hälsotillståndet för dina virtuella datorer och rollinstanser. Du kan åsidosätta den här regeln om du inte använder en belastningsutjämnad uppsättning.

**Inkommande standardregler**

| Namn | Prioritet | Käll-IP-adress | Källport | Mål-IP-adress | Målport | Protokoll | Access |
| --- | --- | --- | --- | --- | --- | --- | --- |
| AllowVNetInBound |65000 | VirtualNetwork | * | VirtualNetwork | * | * | Tillåt |
| AllowAzureLoadBalancerInBound | 65001 | AzureLoadBalancer | * | * | * | * | Tillåt |
| DenyAllInBound |65500 | * | * | * | * | * | Neka |

**Utgående standardregler**

| Namn | Prioritet | Käll-IP-adress | Källport | Mål-IP-adress | Målport | Protokoll | Access |
| --- | --- | --- | --- | --- | --- | --- | --- |
| AllowVnetOutBound | 65000 | VirtualNetwork | * | VirtualNetwork | * | * | Tillåt |
| AllowInternetOutBound | 65001 | * | * | Internet | * | * | Tillåt |
| DenyAllOutBound | 65500 | * | * | * | * | * | Neka |

## <a name="associating-nsgs"></a>Koppla NSG:er
Du kan koppla en NSG tooVMs, nätverkskort och undernät, beroende på hello distributionsmodell du använder, enligt följande:

* **VM (klassisk):** säkerhetsregler används tooall trafik till/från hello VM. 
* **Nätverkskort (enbart Resource Manager):** säkerhetsregler används tooall trafik till/från hello NIC hello NSG är kopplad till. På en virtuell dator i flera nätverkskort, kan du tillämpa olika (eller hello samma) NSG tooeach NIC individuellt. 
* **Undernät (Resource Manager och klassisk):** säkerhetsregler används tooany trafik till/från alla resurser anslutna toohello VNet.

Du kan koppla olika NSG: er tooa VM (eller nätverkskort, beroende på hello distributionsmodell) och hello undernät som ett nätverkskort eller virtuella datorn är ansluten till. Säkerhetsregler används toohello trafiken efter prioritet i varje NSG, i hello följer ordning:

- **Inkommande trafik**

  1. **NSG tillämpas toosubnet:** om ett undernät NSG har en matchande regel toodeny trafik, släpps hello-paketet.

  2. **NSG tillämpas tooNIC** (Resource Manager) eller VM (klassisk): om VM\NIC NSG har en matchande regel som nekar trafik, paket tappas på hello VM\NIC, även om ett undernät NSG har en matchande regel som tillåter trafik.

- **Utgående trafik**

  1. **NSG tillämpas tooNIC** (Resource Manager) eller VM (klassisk): om en VM\NIC NSG har en matchande regel som nekar trafik, paket tappas.

  2. **NSG tillämpas toosubnet:** om ett undernät NSG har en matchande regel som nekar trafik, paket tappas, även om en VM\NIC NSG har en matchande regel som tillåter trafik.

> [!NOTE]
> Även om du kan bara koppla en enda NSG tooa undernät, VM eller NIC; Du kan associera hello samma NSG tooas många resurser som du vill.
>

## <a name="implementation"></a>Implementering
Du kan implementera NSG: er i hello Resource Manager och klassiska distributionsmodeller med hjälp av följande verktyg hello:

| Distributionsverktyg | Klassisk | Resource Manager |
| --- | --- | --- |
| Azure Portal   | Ja | [Ja](virtual-networks-create-nsg-arm-pportal.md) |
| PowerShell     | [Ja](virtual-networks-create-nsg-classic-ps.md) | [Ja](virtual-networks-create-nsg-arm-ps.md) |
| Azure CLI **V1**   | [Ja](virtual-networks-create-nsg-classic-cli.md) | [Ja](virtual-networks-create-nsg-cli-nodejs.md) |
| Azure CLI **V2**   | Nej | [Ja](virtual-networks-create-nsg-arm-cli.md) |
| Azure Resource Manager-mall   | Nej  | [Ja](virtual-networks-create-nsg-arm-template.md) |

## <a name="planning"></a>Planering
Innan du implementerar NSG: er måste tooanswer hello följande frågor:

1. Vilka typer av resurser vill du toofilter trafik tooor från? Du kan ansluta resurser, t.ex nätverkskort (Resource Manager), virtuella datorer (klassisk), molntjänster, programtjänstmiljöer och skalningsuppsättningar för virtuella datorer. 
2. Är hello-resurser som du vill toofilter trafik till och från anslutna toosubnets i befintliga Vnet?

Mer information om planering för nätverkssäkerhet i Azure finns hello [molntjänster och nätverkssäkerhet](../best-practices-network-security.md) artikel. 

## <a name="design-considerations"></a>Designöverväganden
När du väl vet svaren hello toohello frågor i hello [planera](#Planning) avsnittet kan du granska följande avsnitt innan du definierar dina NSG: er hello:

### <a name="limits"></a>Begränsningar
Det finns begränsningar toohello antalet NSG: er som du kan ha en prenumeration och antalet regler per NSG. Mer om hello gränser, läsa hello toolearn [Azure begränsar](../azure-subscription-service-limits.md#networking-limits) artikel.

### <a name="vnet-and-subnet-design"></a>Utformning av VNet och undernät
Eftersom NSG: er kan vara tillämpade toosubnets, kan du minimera hello antalet NSG: er genom att gruppera dina resurser efter undernät och tillämpa NSG: er toosubnets.  Om du tooapply NSG: er toosubnets kan det hända att befintliga Vnet och undernät som du har inte har definierats med NSG: er i åtanke. Du kanske behöver toodefine nya Vnet och undernät toosupport din NSG-utformning och distribuera dina nya resurser tooyour nya undernät. Sedan kan du definiera en migrering strategi toomove befintliga resurser toohello nya undernät. 

### <a name="special-rules"></a>Särskilda regler
Om du blockerar trafik som tillåts av hello enligt reglerna för inte kan din infrastruktur kommunicera med grundläggande Azure-tjänster:

* **Virtuella IP-Adressen för hello värdnoden:** grundläggande infrastrukturtjänster som DHCP, DNS och hälsoövervakning sker via hello virtualiserade värd IP-adressen 168.63.129.16. Den här offentliga IP-adressen hör tooMicrosoft och hello endast virtualiserade IP-adress som används i alla regioner för det här ändamålet. Den här IP-adressen mappar toohello fysiska IP-adressen för serverdatorn (värdnoden) för hello värd hello VM. Hej värdnoden agerar som hello DHCP-relä, rekursiv hello DNS-matchare och hello avsökningskälla för hello ladda belastningsutjämnaren, hälsoavsökningen och datorhälsoavsökningen hello. Kommunikation toothis IP-adress är inte en attack.
* **Licensiering (nyckelhanteringstjänst):** Windows-avbildningar som körs på virtuella datorer måste vara licensierade. tooensure licensiering, en begäran skickas toohello nyckelhanteringstjänst värdservrar för fjärrskrivbordssessioner som hanterar sådana frågor. hello-begäran har gjorts utgående via port 1688.

### <a name="icmp-traffic"></a>ICMP-trafik
hello nuvarande NSG-reglerna tillåter bara protokollen *TCP* eller *UDP*. Det finns ingen specifik tagg för *ICMP*. ICMP-trafik tillåts dock inom ett VNet av hello AllowVNetInBound standardregel som tillåter trafik tooand från alla portar och protokoll inom hello virtuella nätverk.

### <a name="subnets"></a>Undernät
* Fundera över hello många nivåer din arbetsbelastning behöver. Varje nivå kan isoleras med ett undernät med en NSG tillämpas toohello undernät. 
* Om du måste tooimplement ett undernät för en VPN-gateway eller ExpressRoute-krets **inte** tillämpa ett NSG toothat undernät. Om du gör det kan anslutningar mellan virtuella nätverk eller i det lokala systemet misslyckas. 
* Om du behöver tooimplement en virtuell nätverksenhet (NVA) kan ansluta hello NVA tooits egna undernät och skapa användardefinierade vägar (UDR) tooand från hello NVA. Du kan implementera ett undernätverksnivå NSG toofilter trafik till och från det här undernätet. Mer om udr: er, läsa hello toolearn [användardefinierade vägar](virtual-networks-udr-overview.md) artikel.

### <a name="load-balancers"></a>Belastningsutjämnare
* Överväg att hello belastningsutjämning och network adress translation (NAT) regler för varje belastningsutjämnare som används av varje arbetsbelastning. NAT-regler är bundna tooa backend-pool som innehåller nätverkskort (Resource Manager) eller virtuella datorer/molntjänster rollinstanser (klassiska). Överväg att skapa en NSG för varje backend-poolen, så att endast trafik som mappats genom hello regeln implementeras i hello belastningsutjämnare. Skapa en NSG för varje backend-pool garanterar filtreras även trafik som kommer toohello backend-adresspool direkt (i stället via hello belastningsutjämnare).
* I klassiska distributioner skapar du slutpunkter som mappar portar på en belastningen belastningsutjämnaren tooports på VM: ar eller rollinstanser. Du kan också skapa din egen separata offentliga belastningsutjämnare via Resource Manager. hälsning målporten för inkommande trafik är hello faktiska porten i hello VM eller rollinstans inte hello port som exponeras av belastningsutjämning. hello källport och adress för hello anslutning toohello VM är en port och adress på hello fjärrdator i hello Internet, inte hello porten och adressen som exponeras av belastningsutjämnaren hello.
* När du skapar NSG: er toofilter trafik via en intern belastningsutjämnare (ILB), är hello källa port och adressintervallet som appliceras från hello kommer datorn, inte hello belastningsutjämnaren. hello mål och adressintervallet är de hello-måldatorn inte hello belastningsutjämnaren.

### <a name="other"></a>Annat
* Slutpunktsbaserade åtkomstkontrollistor (ACL) och NSG: er stöds inte på hello samma VM-instans. Om du vill toouse en NSG och redan har en slutpunkts-ACL på plats måste du först ta bort hello slutpunkts-ACL. Information om hur tooremove en slutpunkts-ACL, se hello [hantera slutpunkts-ACL:](virtual-networks-acl-powershell.md) artikel.
* I hanteraren för filserverresurser, du kan använda en NSG som kopplats tooa nätverkskort för virtuella datorer med flera nätverkskort tooenable hantering (fjärråtkomst) på grundval av per nätverkskort. Associera unika NSG: er tooeach NIC kan uppdelning av trafiktyper på nätverkskort.
* Liknande toohello användning av belastningsutjämnare, när du filtrerar trafik från andra Vnet, måste du använda hello källadressintervallet från hello fjärrdatorn inte hello gateway ansluter hello Vnet.
* Många Azure-tjänster kan inte vara anslutna tooVNets. Om en Azure-resurs inte är anslutna tooa VNet, kan du inte använda en NSG toofilter trafik toohello resurs.  Dokumentationen hello hello tjänster använder du toodetermine om hello-tjänsten kan vara anslutna tooa VNet.

## <a name="sample-deployment"></a>Exempeldistribution
tooillustrate hello programmet hello information i den här artikeln, Överväg ett vanligt scenario i ett program för två nivåer som visas i följande bild hello:

![NSG:er](./media/virtual-network-nsg-overview/figure1.png)

I hello diagram visas hello *Web1* och *Web2* virtuella datorer som är anslutna toohello *klientdel* undernät och hello *DB1* och *DB2* virtuella datorer som är anslutna toohello *BackEnd* undernät.  Båda undernäten är en del av hello *TestVNet* VNet. hello programkomponenter varje körs i en virtuell Azure-dator ansluten tooa VNet. hello scenariot har hello följande krav:

1. Avgränsning av trafiken mellan hello WEB och DB-servrar.
2. Belastningsutjämning regler vidarebefordra trafik från webbservrar för hello load balancer tooall på port 80.
3. Läsa in belastningsutjämning NAT-regler vidarebefordra trafik kommer i hello belastningsutjämnare på port 50001 tooport 3389 på hello WEB1 VM.
4. Ingen åtkomst toohello frontend- eller virtuella datorer från hello Internet, utom krav 2 och 3.
5. Inga utgående Internetåtkomst från hello webb- eller DB-servrar.
6. Åtkomst från hello klientdel undernät tillåts tooport 3389 för alla webbservrar.
7. Åtkomst från hello klientdel undernät tillåts tooport 3389 på DB-servern.
8. Tooport 1433 för alla servrar som DB tillåts åtkomst från hello Klientdelens undernät.
9. Avgränsning av hanteringstrafik (port 3389) och databastrafik (1433) på olika nätverkskort på DB-servrar.

Krav 1 – 6 (utom krav 3 och 4) är alla slutna toosubnet blanksteg. hello uppfylla följande NSG: er hello tidigare krav, och minimerar hello antalet NSG: er som krävs:

### <a name="frontend"></a>FrontEnd
**Regler för inkommande trafik**

| Regel | Åtkomst | Prioritet | Källadress-intervall | Källport | Måladress-intervall | Målport | Protokoll |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Allow-Inbound-HTTP-Internet | Tillåt | 100 | Internet | * | * | 80 | TCP |
| Allow-Inbound-RDP-Internet | Tillåt | 200 | Internet | * | * | 3389 | TCP |
| Deny-Inbound-All | Neka | 300 | Internet | * | * | * | TCP |

**Regler för utgående trafik**

| Regel | Åtkomst | Prioritet | Källadress-intervall | Källport | Måladress-intervall | Målport | Protokoll |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Deny-Internet-All |Neka |100 | * | * | Internet | * | * |

### <a name="backend"></a>BackEnd
**Regler för inkommande trafik**

| Regel | Åtkomst | Prioritet | Källadress-intervall | Källport | Måladress-intervall | Målport | Protokoll |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Deny-Internet-All | Neka | 100 | Internet | * | * | * | * |

**Regler för utgående trafik**

| Regel | Åtkomst | Prioritet | Källadress-intervall | Källport | Måladress-intervall | Målport | Protokoll |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Deny-Internet-All | Neka | 100 | * | * | Internet | * | * |

hello följande NSG: er skapas och tillhörande tooNICs i hello följande virtuella datorer:

### <a name="web1"></a>WEB1
**Regler för inkommande trafik**

| Regel | Åtkomst | Prioritet | Källadress-intervall | Källport | Måladress-intervall | Målport | Protokoll |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Allow-Inbound-RDP-Internet | Tillåt | 100 | Internet | * | * | 3389 | TCP |
| Allow-Inbound-HTTP-Internet | Tillåt | 200 | Internet | * | * | 80 | TCP |

> [!NOTE]
> hello källadress-intervallet för hello tidigare regler är **Internet**, inte hello virtuella IP-adresser för hello belastningsutjämnaren. hello källporten är *, inte 500001. NAT-regler för belastningsutjämning är inte hello samma som NSG säkerhetsregler. NSG säkerhetsregler är alltid relaterade toohello ursprungliga källan och slutdestinationen för trafiken, **inte** hello belastningsutjämnaren mellan hello två. 
> 
> 

### <a name="web2"></a>WEB2
**Regler för inkommande trafik**

| Regel | Åtkomst | Prioritet | Källadress-intervall | Källport | Måladress-intervall | Målport | Protokoll |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Deny-Inbound-RDP-Internet | Neka | 100 | Internet | * | * | 3389 | TCP |
| Allow-Inbound-HTTP-Internet | Tillåt | 200 | Internet | * | * | 80 | TCP |

### <a name="db-servers-management-nic"></a>DB-servrar (nätverkskort för hantering)
**Regler för inkommande trafik**

| Regel | Åtkomst | Prioritet | Källadress-intervall | Källport | Måladress-intervall | Målport | Protokoll |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Allow-Inbound-RDP-Front-end | Tillåt | 100 | 192.168.1.0/24 | * | * | 3389 | TCP |

### <a name="db-servers-database-traffic-nic"></a>DB-servrar (nätverkskort för databastrafik)
**Regler för inkommande trafik**

| Regel | Åtkomst | Prioritet | Källadress-intervall | Källport | Måladress-intervall | Målport | Protokoll |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Allow-Inbound-SQL-Front-end | Tillåt | 100 | 192.168.1.0/24 | * | * | 1433 | TCP |

Eftersom vissa av hello NSG: er är associerade tooindividual nätverkskort är hello regler för resurser som har distribuerats via Resource Manager. Regler kombineras för undernät och nätverkskort, beroende på hur de är associerade. 

## <a name="next-steps"></a>Nästa steg
* [Distribuera nätverkssäkerhetsgrupper (Resource Manager)](virtual-networks-create-nsg-arm-pportal.md).
* [Distribuera nätverkssäkerhetsgrupper (klassisk)](virtual-networks-create-nsg-classic-ps.md).
* [Hantera NSG-loggar](virtual-network-nsg-manage-log.md).
* [Felsöka nätverkssäkerhetsgrupper] (virtual-network-nsg-troubleshoot-portal.md)
