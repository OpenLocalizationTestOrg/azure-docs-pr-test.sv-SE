---
title: "aaaDNS namnmatchningsalternativ för Linux virtuella datorer i Azure"
description: "Namnet upplösning scenarier för Linux virtuella datorer i Azure IaaS, inklusive tillhandahålls DNS-tjänster, hybrid externa DNS- och ta med din egen DNS-server."
services: virtual-machines
documentationcenter: na
author: RicksterCDN
manager: timlt
editor: tysonn
ms.assetid: 787a1e04-cebf-4122-a1b4-1fcf0a2bbf5f
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/19/2016
ms.author: rclaus
ms.openlocfilehash: 7df2320b6f6b42b479bf4070ea60fe5770a78a41
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="dns-name-resolution-options-for-linux-virtual-machines-in-azure"></a>Alternativ för DNS-namnmatchningen för Linux virtuella datorer i Azure
Azure tillhandahåller DNS-namnmatchning som standard för alla virtuella datorer som är i ett enda virtuellt nätverk. Du kan implementera din egen lösning för DNS-namnet lösning genom att konfigurera dina egna DNS-tjänster på dina virtuella datorer som är värd för Azure. hello bör följande scenarier hjälpa dig att välja hello som passar din situation.

* [Namnmatchning som Azure tillhandahåller](#azure-provided-name-resolution)
* [Namnmatchning med hjälp av DNS-servern](#name-resolution-using-your-own-dns-server)

hello typ av namnmatchning som du använder beror på hur dina virtuella datorer och rollinstanser måste toocommunicate med varandra.

hello visar följande tabell scenarier och motsvarande name resolution lösningar:

| **Scenario** | **Lösning** | **Suffix** |
| --- | --- | --- |
| Namnmatchning mellan rollinstanser eller virtuella datorer i hello samma virtuella nätverk |[Namnmatchning som Azure tillhandahåller](#azure-provided-name-resolution) |värdnamn eller fullständigt kvalificerat domännamn (FQDN) |
| Namnmatchning mellan rollinstanser eller virtuella datorer i olika virtuella nätverk |Kundhanterad DNS-servrar som vidarebefordrar frågor mellan virtuella nätverk för matchning av Azure (DNS-proxy). Se [namnmatchning med hjälp av DNS-servern](#name-resolution-using-your-own-dns-server). |Endast FQDN |
| Matchning av lokala datorer och tjänstnamn från rollinstanser eller virtuella datorer i Azure |Kundhanterad DNS-servrar (till exempel lokala domänkontrollant, lokala skrivskyddade domänkontrollanten eller en sekundär DNS synkroniseras med hjälp av zonöverföringar). Se [namnmatchning med hjälp av DNS-servern](#name-resolution-using-your-own-dns-server). |Endast FQDN |
| Lösning med Azure värdnamn från lokala datorer |Vidarebefordra frågor tooa kundhanterad DNS-proxyserver i hello motsvarande virtuella nätverk. hello proxyserver vidarebefordrar frågor tooAzure för matchning. Se [namnmatchning med hjälp av DNS-servern](#name-resolution-using-your-own-dns-server). |Endast FQDN |
| Omvänd DNS för interna IP-adresser |[Namnmatchning med hjälp av DNS-servern](#name-resolution-using-your-own-dns-server) |Saknas |

## <a name="name-resolution-that-azure-provides"></a>Namnmatchning som Azure tillhandahåller
Azure erbjuder intern namnmatchning för virtuella datorer och rollinstanser som finns i hello samma virtuella nätverk tillsammans med matchning av offentliga DNS-namn. I virtuella nätverk som baseras på Azure Resource Manager är hello DNS-suffix konsekvent på hello virtuella nätverk. hello FQDN behövs inte. DNS-namn kan tilldelas tooboth nätverkskort (NIC) och virtuella datorer. Även om hello namnmatchning som Azure tillhandahåller inte kräver någon konfiguration, är det inte hello valet för alla distributionsscenarier som det visas på hello föregående tabell.

### <a name="features-and-considerations"></a>Funktioner och överväganden
**Funktioner:**

* Ingen konfiguration är obligatoriska toouse namnmatchning som Azure tillhandahåller.
* hello namnmatchningstjänst som Azure tillhandahåller har hög tillgänglighet. Du inte behöver toocreate och hantera kluster för DNS-servrar.
* hello namnmatchningstjänst som Azure tillhandahåller kan användas tillsammans med dina egna DNS-servrar tooresolve både lokalt och Azure värdnamn.
* Namnmatchning tillhandahålls mellan virtuella datorer i virtuella nätverk utan behov av hello FQDN.
* Du kan använda värdnamn som bäst beskriver dina distributioner i stället för att arbeta med automatiskt genererat namn.

**Att tänka på:**

* hello DNS-suffix som Azure skapar ändras inte.
* Du kan registrera dina egna poster manuellt.
* WINS och NetBIOS stöds inte.
* Värdnamn måste vara kompatibel med DNS.
    Namn måste använda endast 0-9, a-z och '-', och de får inte inledas eller avslutas med en '-'. Avsnittet RFC 3696 2.
* DNS-frågorna begränsas för varje virtuell dator. Begränsning bör inte påverka de flesta program.  Om begäran begränsning observeras, kontrollerar du att cachelagring på klientsidan är aktiverad.  Mer information finns i [komma hello de flesta av namnmatchning som Azure tillhandahåller](#getting-the-most-from-name-resolution-that-azure-provides).

### <a name="getting-hello-most-from-name-resolution-that-azure-provides"></a>Hämta hello de flesta av namnmatchning som Azure tillhandahåller
**Klientcachelagring:**

Några DNS-frågor skickas inte över hello nätverk. Klientcachelagring hjälper minskar svarstider och förbättrar återhämtning toonetwork inkonsekvenser genom att lösa återkommande DNS-frågor från en lokal cache. DNS-poster innehåller en Time-To-Live (TTL), vilket gör att hello toostore hello cacheposten så länge som möjligt utan att påverka poster dokumentens. Därför lämpar klientcachelagring sig för de flesta situationer.

Vissa Linux-distributioner som innehåller inte cachelagring som standard. Vi rekommenderar att du lägger till en cache tooeach Linux-dator när du har kontrollerat att det inte är ett lokalt cacheminne redan.

Det finns flera olika DNS cachelagring paket, till exempel dnsmasq. Här följer hello steg tooinstall dnsmasq på de vanligaste hello-distributioner:

**Ubuntu (använder resolvconf)**
  * Installera hello dnsmasq paketet (”sudo lgh get installera dnsmasq”).

**SUSE (använder netconf)**:
1. Installera hello dnsmasq paketet (”sudo zypper installera dnsmasq”).
2. Aktivera hello dnsmasq service (”systemctl aktivera dnsmasq.service”).
3. Starta hello dnsmasq (”systemctl start dnsmasq.service”).
4. Redigera ”/ etc/sysconfig/nätverk/config” och ändra NETCONFIG_DNS_FORWARDER = ”” för ”dnsmasq”.
5. Uppdatera resolv.conf (”netconfig update”) tooset hello-cachen som hello lokala DNS-matchning.

**CentOS av falska Wave programvara (tidigare OpenLogic; använder NetworkManager)**
1. Installera hello dnsmasq paketet (”sudo yum installera dnsmasq”).
2. Aktivera hello dnsmasq service (”systemctl aktivera dnsmasq.service”).
3. Starta hello dnsmasq (”systemctl start dnsmasq.service”).
4. Lägg till ”lägga domän-namnservrar 127.0.0.1”; too"/etc/dhclient-eth0.conf”.
5. Starta om hello network service (”tjänsten network omstart”) tooset hello cache vid hello lokala DNS-matchning

> [!NOTE]
> : hello 'dnsmasq' paketet är endast ett av många DNS cachelagrar hello som är tillgängliga för Linux. Innan du använder det. Kontrollera dess lämplighet för dina behov och att ingen annan cache är installerad.
>
>

**Klientsidans återförsök**

DNS är främst ett UDP-protokoll. Eftersom hello UDP-protokollet inte garantera meddelandeleverans, hanterar hello själva DNS-protokollet logik. Varje DNS-klient (operativsystemet) kan ha olika logik beroende på hello creator inställningar:

* Windows-operativsystem försök igen efter att en andra och sedan igen efter en annan två, fyra och ett annat fyra sekunder.
* hello standard Linux installationsprogrammet återförsök efter fem sekunder.  Du bör ändra den här tooretry fem gånger på en sekunds intervall.  

toocheck hello aktuella inställningarna på en virtuell Linux-dator, 'cat /etc/resolv.conf' och titta på hello ”alternativ” rad, till exempel:

    options timeout:1 attempts:5

Hej resolv.conf filen genereras automatiskt och bör inte redigeras. Hej särskilda åtgärder som lägger till hello ”alternativ” rad varierar beroende på distributionsplatser:

**Ubuntu** (använder resolvconf)
1. Lägg till hello alternativ rad too'/etc/resolveconf/resolv.conf.d/head'.
2. Kör resolvconf -u tooupdate.

**SUSE** (använder netconf)
1. Lägg till 'timeout:1 försök: 5' toohello NETCONFIG_DNS_RESOLVER_OPTIONS = ”” parametern i ”/ etc/sysconfig/nätverk/config”.
2. Kör tooupdate netconfig-uppdateringen.

**CentOS av falska Wave-programvara (tidigare OpenLogic)** (använder NetworkManager)
1. Lägg till 'echo ”alternativ timeout:1 försök: 5”' too'/etc/NetworkManager/dispatcher.d/11-dhclient'.
2. Kör 'service nätverket restart' tooupdate.

## <a name="name-resolution-using-your-own-dns-server"></a>Namnmatchning med hjälp av DNS-servern
Din namnmatchningen kan utöver hello-funktioner som Azure tillhandahåller. Du kan till exempel kräva DNS-matchning mellan virtuella nätverk. toocover i det här scenariot kan du använda DNS-servrar.  

DNS-servrar i ett virtuellt nätverk kan vidarebefordra DNS-frågor toorecursive matchare Azure tooresolve värdnamn som finns i hello samma virtuella nätverk. En DNS-server som körs i Azure kan till exempel svara tooDNS frågor för sin egen DNS-zonen filer och vidarebefordra alla frågor tooAzure. Den här funktionen möjliggör toosee för virtuella datorer både din poster i zonen filer och värdnamn som Azure tillhandahåller (via hello vidarebefordrare). Åtkomst toohello rekursiv matchare Azure tillhandahålls via hello virtuella IP-Adressen 168.63.129.16.

DNS-vidarebefordran också aktiverar DNS-matchning mellan virtuella nätverk och gör att dina lokala datorer tooresolve värdnamn som Azure tillhandahåller. tooresolve värdnamn för en virtuell dator, virtuell dator med hello DNS-server måste finnas i hello samma virtuella nätverk och vara konfigurerade tooforward hostname frågor tooAzure. Eftersom hello DNS-suffix skiljer sig åt i varje virtuellt nätverk, kan du använda villkorlig vidarebefordran regler toosend DNS-frågor toohello korrigera virtuellt nätverk för matchning. hello följande bild visar två virtuella nätverk och ett lokalt nätverk som gör att DNS-matchning mellan virtuella nätverk med hjälp av den här metoden:

![DNS-matchning mellan virtuella nätverk](./media/azure-dns/inter-vnet-dns.png)

När du använder namnmatchning som Azure tillhandahåller som hello interna DNS-suffix tooeach virtuell dator med hjälp av DHCP. När du använder din egen lösning för name resolution är detta suffix inte angiven toovirtual datorer eftersom hello suffix stör andra DNS-arkitekturer. toorefer toomachines av FQDN eller tooconfigure hello-suffix på virtuella datorer, kan du använda PowerShell eller hello API toodetermine hello-suffix:

* För virtuella nätverk som hanteras av Azure Resource Manager hello-suffix som är tillgängliga via hello [nätverkskort](https://msdn.microsoft.com/library/azure/mt163668.aspx) resurs. Du kan också köra hello `azure network public-ip show <resource group> <pip name>` kommandot toodisplay hello information om offentlig IP, vilket innefattar hello FQDN för hello NIC.

Om vidarebefordran av frågor tooAzure inte passar dina behov, måste tooprovide DNS-lösningen.  DNS-lösningen behöver:

* Ange lämpliga värdnamnsmatchning, till exempel [DDNS](../../virtual-network/virtual-networks-name-resolution-ddns.md). Om du använder DDNS kan behöva du toodisable DNS-posten rensning. DHCP-lån för Azure är mycket långa och rensning kan ta bort DNS-poster för tidigt.
* Ange lämpliga rekursiv namnmatchning tooallow matchning av extern domännamn.
* Tillgänglig (TCP och UDP-port 53) från hello klienter den fungerar och kan tooaccess hello Internet.
* Skyddas mot åtkomst från hello Internet toomitigate hot med externa agenter.

> [!NOTE]
> För bästa prestanda när du använder virtuella datorer i Azure DNS-servrar, inaktivera IPv6 och tilldela en [offentlig IP på instansnivå](../../virtual-network/virtual-networks-instance-level-public-ip.md) tooeach DNS-server-datorn.  
>
>
