---
title: "aaaConfigure ett virtuellt nätverk för Premium Azure Redis-Cache | Microsoft Docs"
description: "Lär dig hur toocreate och hantera virtuella nätverk stöd för din Premium-nivån Azure Redis-Cache-instanser"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 8b1e43a0-a70e-41e6-8994-0ac246d8bf7f
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: sdanie
ms.openlocfilehash: fab715f4d9365ee4c2f8b89d2e2e58768c25b671
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-virtual-network-support-for-a-premium-azure-redis-cache"></a>Hur tooconfigure för virtuella nätverk som har stöd för Premium Azure Redis-Cache
Azure Redis-Cache har olika cache erbjudanden, vilket ger flexibilitet i hello valet av cachestorlek och funktioner, inklusive funktioner för Premium-nivån, till exempel klustring, beständiga och stöd för virtuella nätverk. Ett virtuellt nätverk är ett privat nätverk i hello molnet. När en instans av Azure Redis-Cache har konfigurerats med ett VNet, är inte offentligt adresserbara och kan endast nås från virtuella datorer och program i hello virtuella nätverk. Den här artikeln beskriver hur tooconfigure virtuella nätverket har stöd för en premium Azure Redis-Cache-instans.

> [!NOTE]
> Azure Redis-Cache har stöd för både klassiska och Resource Manager VNets.
> 
> 

Information om andra cache premium-funktioner finns [introduktion toohello Azure Redis Cache Premium-nivån](cache-premium-tier-intro.md).

## <a name="why-vnet"></a>Varför VNet?
[Azure Virtual Network (VNet)](https://azure.microsoft.com/services/virtual-network/) distribution ger förbättrad säkerhet och isolering för Azure Redis-Cache, samt undernät, principer för åtkomstkontroll och andra funktioner toofurther begränsa åtkomsten.

## <a name="virtual-network-support"></a>Stöd för virtuella nätverk
Stöd för virtuella nätverk (VNet) är konfigurerat på hello **nytt Redis-Cache** bladet under skapande av cachen. 

[!INCLUDE [redis-cache-create](../../includes/redis-cache-premium-create.md)]

När du har valt en premium-prisnivån, du kan konfigurera Redis VNet integration genom att välja ett VNet i hello samma prenumeration och plats som ditt cacheminne. toouse ett nytt virtuellt nätverk, skapa den första genom att följa stegen hello i [skapa ett virtuellt nätverk med hello Azure-portalen](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) eller [skapa ett virtuellt nätverk (klassiska) med hjälp av hello Azure-portalen](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) och returnerar sedan toohello **Nytt Redis-Cache** bladet toocreate och konfigurera din premium-cache.

tooconfigure hello VNet för ditt nya cacheminne, klickar du på **virtuellt nätverk** på hello **nytt Redis-Cache** bladet och väljer hello önskad VNet hello nedrullningsbara listan.

![Virtuellt nätverk][redis-cache-vnet]

Välj hello önskade undernät från hello **undernät** nedrullningsbara listan, och ange önskade hello **statisk IP-adress**. Om du använder en klassiska VNet hello **statisk IP-adress** fältet är valfritt och om inget anges en väljs från hello valda undernät.

> [!IMPORTANT]
> När du distribuerar ett Azure Redis-Cache tooa Resource Manager VNet måste hello cache vara i ett dedikerat undernät som innehåller några resurser förutom Azure Redis-Cache-instanser. Om ett försök görs toodeploy misslyckas en Azure Redis-Cache tooa Resource Manager VNet tooa undernät som innehåller andra resurser, hello-distribution.
> 
> 

![Virtuellt nätverk][redis-cache-vnet-ip]

> [!IMPORTANT]
> Azure reserverar vissa IP-adresser inom varje undernät, och dessa adresser kan inte användas. hello är första och sista IP-adresser för hello undernät reserverade för överensstämmelse med protokollet, tillsammans med tre flera adresser som används för Azure-tjänster. Mer information finns i [finns det några begränsningar med hjälp av IP-adresser inom dessa undernät?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets)
> 
> I tillägg toohello IP-adresser används av hello Azure VNET infrastruktur, varje Redis-instansen i hello undernät använder två IP-adresser per Fragmentera och en ytterligare IP-adress för hello belastningsutjämnaren. En icke-klustrade cache anses toohave en Fragmentera.
> 
> 

När hello-cache skapas, du kan visa hello konfigurationen för hello VNet genom att klicka på **virtuellt nätverk** från hello **resurs menyn**.

![Virtuellt nätverk][redis-cache-vnet-info]

tooconnect tooyour Azure Redis-cache-instans när du använder ett virtuellt nätverk, ange hello värdnamnet för ditt cacheminne i hello anslutningssträngen som visas i följande exempel hello:

    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        return ConnectionMultiplexer.Connect("contoso5premium.redis.cache.windows.net,abortConnect=false,ssl=true,password=password");
    });

    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

## <a name="azure-redis-cache-vnet-faq"></a>Azure Redis-Cache VNet vanliga frågor och svar
hello följande lista innehåller svar toocommonly frågor och svar om hello Azure Redis-Cache skalning.

* [Vilka är några vanliga problem med Azure Redis-Cache och Vnet?](#what-are-some-common-misconfiguration-issues-with-azure-redis-cache-and-vnets)
* [Hur kan jag bekräfta att min cache fungerar i ett virtuellt nätverk?](#how-can-i-verify-that-my-cache-is-working-in-a-vnet)
* [Kan jag använda Vnet med en standard- eller basic-cache?](#can-i-use-vnets-with-a-standard-or-basic-cache)
* [Varför skapa ett Redis-cache misslyckas i vissa undernät, men inte andra?](#why-does-creating-a-redis-cache-fail-in-some-subnets-but-not-others)
* [Vad är utrymmeskraven hello undernät?](#what-are-the-subnet-address-space-requirements)
* [Fungerar alla cachefunktioner när värd en cachelagring i ett virtuellt nätverk?](#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)

## <a name="what-are-some-common-misconfiguration-issues-with-azure-redis-cache-and-vnets"></a>Vilka är några vanliga problem med Azure Redis-Cache och Vnet?
När Azure Redis-Cache finns i ett VNet, används hello portarna i följande tabeller hello. 

>[!IMPORTANT]
>Om hello portar i följande tabeller hello blockeras fungerar hello cachen inte korrekt. Med en eller flera av de här portarna blockeras är hello vanligaste felkonfiguration problem när du använder Azure Redis-Cache i ett VNet.
> 
> 

- [Utgående portkrav](#outbound-port-requirements)
- [Krav för inkommande portar](#inbound-port-requirements)

### <a name="outbound-port-requirements"></a>Utgående portkrav

Det finns sju krav för utgående port.

- Om du vill alla utgående anslutningar toohello internet kan göras via en klient lokalt granskning enhet.
- Tre hello portar vidarebefordra trafik tooAzure slutpunkter servicing Azure Storage- och Azure DNS.
- hello återstående portintervall och för intern kommunikation för Redis-undernät. Inga undernät NSG-regler krävs för intern kommunikation för Redis-undernät.

| Portar | Riktning | Transportprotokoll | Syfte | Lokala IP | Fjärr-IP |
| --- | --- | --- | --- | --- | --- |
| 80, 443 |Utgående |TCP |Redis-beroenden på Azure Storage/PKI (Internet) | (Redis undernät) |* |
| 53 |Utgående |TCP/UDP |Redis-beroenden i DNS (Internet/VNet) | (Redis undernät) |* |
| 8443 |Utgående |TCP |Intern kommunikation för Redis | (Redis undernät) | (Redis undernät) |
| 10221-10231 |Utgående |TCP |Intern kommunikation för Redis | (Redis undernät) | (Redis undernät) |
| 20226 |Utgående |TCP |Intern kommunikation för Redis | (Redis undernät) |(Redis undernät) |
| 13000-13999 |Utgående |TCP |Intern kommunikation för Redis | (Redis undernät) |(Redis undernät) |
| 15000-15999 |Utgående |TCP |Intern kommunikation för Redis | (Redis undernät) |(Redis undernät) |


### <a name="inbound-port-requirements"></a>Krav för inkommande portar

Det finns åtta inkommande port intervallet krav. Inkommande begäranden i dessa områden är antingen inkommande trafik från andra tjänster i hello samma virtuella nätverk eller interna toohello Redis undernät kommunikation.

| Portar | Riktning | Transportprotokoll | Syfte | Lokala IP | Fjärr-IP |
| --- | --- | --- | --- | --- | --- |
| 6379, 6380 |Inkommande |TCP |Klienten kommunikation tooRedis Azure belastningsutjämning | (Redis undernät) |Virtuellt nätverk, Azure belastningsutjämnare |
| 8443 |Inkommande |TCP |Intern kommunikation för Redis | (Redis undernät) |(Redis undernät) |
| 8500 |Inkommande |TCP/UDP |Azure belastningsutjämning | (Redis undernät) |Azure Load Balancer |
| 10221-10231 |Inkommande |TCP |Intern kommunikation för Redis | (Redis undernät) |(Redis undernät), Azure belastningsutjämnare |
| 13000-13999 |Inkommande |TCP |Klienten kommunikation tooRedis kluster, Azure belastningsutjämning | (Redis undernät) |Virtuellt nätverk, Azure belastningsutjämnare |
| 15000-15999 |Inkommande |TCP |Klienten kommunikation tooRedis kluster, Azure belastningsutjämning | (Redis undernät) |Virtuellt nätverk, Azure belastningsutjämnare |
| 16001 |Inkommande |TCP/UDP |Azure belastningsutjämning | (Redis undernät) |Azure Load Balancer |
| 20226 |Inkommande |TCP |Intern kommunikation för Redis | (Redis undernät) |(Redis undernät) |

### <a name="additional-vnet-network-connectivity-requirements"></a>Ytterligare anslutningskrav för virtuella nätverk

Det finns anslutningskrav för Azure Redis-Cache som inte kanske ursprungligen uppfyllas i ett virtuellt nätverk. Azure Redis-Cache måste alla hello följande objekt toofunction när de används i ett virtuellt nätverk.

* Utgående anslutning tooAzure lagring nätverksslutpunkter över hela världen. Detta inkluderar slutpunkter finns i hello samma region som hello Azure Redis-cacheinstansen som lagring slutpunkter finns i **andra** Azure-regioner. Azure Storage-slutpunkter lösa under hello följande DNS-domäner: *table.core.windows.net*, *blob.core.windows.net*, *queue.core.windows.net*, och *file.core.windows.net*. 
* Utgående nätverksanslutningar för*ocsp.msocsp.com*, *mscrl.microsoft.com*, och *crl.microsoft.com*. Den här anslutningen är nödvändiga toosupport SSL-funktion.
* hello DNS-konfiguration för hello virtuella nätverket måste kunna lösa alla hello slutpunkter och domäner som anges i hello tidigare punkter. Dessa DNS-krav kan uppfyllas genom att säkerställa att en giltig DNS-infrastruktur konfigurerad och underhålls för hello virtuellt nätverk.
* Utgående network connectivity toohello följande Azure-övervakning slutpunkter som löser under hello följande DNS-domäner: shoebox2 black.shoebox2.metrics.nsatc.net, norra prod2.prod2.metrics.nsatc.net azglobal-black.azglobal.metrics.nsatc.net, shoebox2 red.shoebox2.metrics.nsatc.net Öst-prod2.prod2.metrics.nsatc.net azglobal-red.azglobal.metrics.nsatc.net.

### <a name="how-can-i-verify-that-my-cache-is-working-in-a-vnet"></a>Hur kan jag bekräfta att min cache fungerar i ett virtuellt nätverk?

>[!IMPORTANT]
>När du ansluter tooan Azure Redis-Cache-instans som är värd för ett virtuellt nätverk hello din cache-klienter måste vara i samma virtuella nätverk, inklusive testprogrammen eller pinga diagnosverktyg.
>
>

När hello portkrav har konfigurerats enligt beskrivningen i föregående avsnitt i hello, kan du kontrollera att ditt cacheminne fungerar genom att utföra följande steg hello.

- [Starta om](cache-administration.md#reboot) alla hello cachelagra noder. Om alla hello krävs cacheberoenden inte kan nås (enligt beskrivningen i [inkommande portkrav](cache-how-to-premium-vnet.md#inbound-port-requirements) och [utgående portkrav](cache-how-to-premium-vnet.md#outbound-port-requirements)), hello cachen inte kan toorestart har.
- När hello cachenoder har startats om (som rapporteras av hello Cachestatus i hello Azure-portalen) kan du utföra hello följande test:
  - Pinga hello cache slutpunkten (via port 6380) från en dator som ligger inom hello samma virtuella nätverk som hello cache, med hjälp av [tcping](https://www.elifulkerson.com/projects/tcping.php). Exempel:
    
    `tcping.exe contosocache.redis.cache.windows.net 6380`
    
    Om hello `tcping` verktyget rapporterar att hello porten är öppen, hello cache är tillgängligt för anslutningen från klienter i hello virtuella nätverk.

  - Ett annat sätt tootest är toocreate en test-cacheklient (som kan vara en enkel konsoltillämpning med StackExchange.Redis) som ansluter toohello cache och lägger till och hämtar några objekt från hello cache. Installera hello exempel klientprogrammet på en virtuell dator som är i hello samma virtuella nätverk som hello cache och kör den tooverify anslutningen toohello cache.


### <a name="can-i-use-vnets-with-a-standard-or-basic-cache"></a>Kan jag använda Vnet med en standard- eller basic-cache?
Vnet kan endast användas med premium-cache.

### <a name="why-does-creating-a-redis-cache-fail-in-some-subnets-but-not-others"></a>Varför skapa ett Redis-cache misslyckas i vissa undernät, men inte andra?
Om du distribuerar ett Azure Redis-Cache tooa Resource Manager VNet måste hello cache vara i ett dedikerat undernät som innehåller inga andra resurstypen. Om ett försök görs toodeploy misslyckas en Azure Redis-Cache tooa Resource Manager VNet-undernät som innehåller andra resurser, hello-distribution. Du måste ta bort hello befintliga resurser i hello undernät innan du kan skapa ett nytt Redis-cache.

Du kan distribuera flera typer av resurser tooa klassiska virtuella nätverk som du har tillräckligt med IP-adresser tillgängliga.

### <a name="what-are-hello-subnet-address-space-requirements"></a>Vad är utrymmeskraven hello undernät?
Azure reserverar vissa IP-adresser inom varje undernät, och dessa adresser kan inte användas. hello är första och sista IP-adresser för hello undernät reserverade för överensstämmelse med protokollet, tillsammans med tre flera adresser som används för Azure-tjänster. Mer information finns i [finns det några begränsningar med hjälp av IP-adresser inom dessa undernät?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets)

I tillägg toohello IP-adresser används av hello Azure VNET infrastruktur, varje Redis-instansen i hello undernät använder två IP-adresser per Fragmentera och en ytterligare IP-adress för hello belastningsutjämnaren. En icke-klustrade cache anses toohave en Fragmentera.

### <a name="do-all-cache-features-work-when-hosting-a-cache-in-a-vnet"></a>Fungerar alla cachefunktioner när värd en cachelagring i ett virtuellt nätverk?
När din cache är en del av ett VNET, endast klienter i hello VNET kan komma åt hello cache. Därför fungerar hello följande funktioner för hantering av cache inte just nu.

* Redis-konsolen - eftersom Redis-konsolen körs i din lokala webbläsare som är utanför hello VNET, det går inte att ansluta tooyour cache.

## <a name="use-expressroute-with-azure-redis-cache"></a>Använda ExpressRoute med Azure Redis-Cache
Kunder kan ansluta en [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/) krets tootheir virtuell nätverksinfrastruktur, vilket utöka sina lokala nätverk tooAzure. 

Som standard en nyligen skapade ExpressRoute-krets utför inte Tvingad tunneling (annons med en standardväg 0.0.0.0/0) på ett virtuellt nätverk. Därför utgående Internetanslutning tillåts direkt från hello VNET och klientprogrammen finns kan tooconnect tooother Azure slutpunkter, inklusive Azure Redis-Cache.

Men en gemensam konfiguration av customer är toouse Tvingad tunneltrafik (annonsera en standardväg) som tvingar utgående Internet-trafik tooinstead flöde lokalt. Den här trafikflöde bryter anslutningen till Azure Redis-Cache om hello utgående trafik sedan blockeras lokalt så att hello Azure Redis-Cache-instansen inte är kan toocommunicate med dess beroenden.

hello-lösning är toodefine (minst) användardefinierade vägar (udr: er) i hello undernät som innehåller hello Azure Redis-Cache. En UDR definierar undernät-specifika vägar som ska användas i stället för hello standardväg.

Om möjligt bör toouse hello följande konfiguration:

* Hej ExpressRoute configuration annonserar 0.0.0.0/0 och som standard kraft tunnlar all utgående trafik lokalt.
* Hej UDR tillämpas toohello undernät som innehåller hello Azure Redis-Cache definierar 0.0.0.0/0 med en fungerande väg för TCP/IP-trafik toohello offentliga internet. till exempel genom att ange hello nästa hopp-typen too'Internet'.

hello är kombinerade effekten av dessa steg att hello undernätverksnivå UDR företräde framför hello ExpressRoute Tvingad tunneltrafik tillse utgående Internetåtkomst från hello Azure Redis-Cache.

Anslutande tooan Azure Redis-Cache-instansen från ett lokalt program som använder ExpressRoute är inte en typisk användningsscenariot på grund av orsaker som tooperformance (för bästa prestanda Azure Redis-Cache ska gälla för klienterna hello samma region som hello Azure Redis-Cache).

>[!IMPORTANT] 
>Hej vägar som definierats i en UDR **måste** vara specifikt nog tootake företräde framför alla vägar annonseras av hello ExpressRoute konfiguration. hello följande exempel använder hello bred 0.0.0.0/0 adressintervallet och som sådan kan potentiellt av misstag åsidosättas av flödet annonser med hjälp av mer specifika adressintervall.

>[!WARNING]  
>Azure Redis-Cache stöds inte med ExpressRoute-konfigurationer som **felaktigt cross-annonsera vägar från hello offentlig peering sökväg toohello privat peering sökväg**. ExpressRoute-konfigurationer som har konfigurerats, offentlig peering får vägannonser från Microsoft för ett stort antal Microsoft Azure IP-adressintervall. Om dessa adressintervall felaktigt cross-annonserade på hello privat peering sökväg, är hello resultatet att alla utgående paket från hello Azure Redis-Cache instans undernät är felaktigt kraft tunneldata tooa kundens lokala nätverk infrastruktur. Det här nätverket flödet bryts Azure Redis-Cache. hello lösning toothis problemet är toostop mellan reklam vägar från hello offentlig peering sökväg toohello privat peering sökväg.


Bakgrundsinformation om användardefinierade vägar är tillgänglig i det här [översikt](../virtual-network/virtual-networks-udr-overview.md).

Mer information om ExpressRoute finns [teknisk översikt för ExpressRoute](../expressroute/expressroute-introduction.md).

## <a name="next-steps"></a>Nästa steg
Lär dig hur toouse mer premium cachelagra funktioner.

* [Introduktion toohello Azure Redis Cache Premium-nivån](cache-premium-tier-intro.md)

<!-- IMAGES -->

[redis-cache-vnet]: ./media/cache-how-to-premium-vnet/redis-cache-vnet.png

[redis-cache-vnet-ip]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-ip.png

[redis-cache-vnet-info]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-info.png

