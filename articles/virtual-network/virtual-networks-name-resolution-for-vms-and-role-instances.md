---
title: "aaaResolution för virtuella datorer och Rollinstanser"
description: "Namn på lösning scenarier för Azure IaaS hybridlösningar mellan olika cloud services, Active Directory och använder egna DNS-server "
services: virtual-network
documentationcenter: na
author: GarethBradshawMSFT
manager: carmonm
editor: tysonn
ms.assetid: 5d73edde-979a-470a-b28c-e103fcf07e3e
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/06/2016
ms.author: telmos
ms.openlocfilehash: 0ec7903cf200c1d04d75601a5b0cefe4268f3dcf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="name-resolution-for-vms-and-role-instances"></a>Name Resolution for VMs and Role Instances (Namnmatchning för virtuella datorer och rollinstanser)
Beroende på hur du använder Azure toohost IaaS och PaaS hybridlösningar, kanske du måste tooallow hello virtuella datorer och rollinstanser som du skapar toocommunicate med varandra. Även om den här kommunikationen kan göras med hjälp av IP-adresser, är det mycket enklare toouse namn som enkelt kan registreras och ändras inte. 

När rollinstanser och virtuella datorer finns i Azure måste du tooresolve domän namn toointernal IP-adresser, kan de använda ett av två sätt:

* [Azure-tillhandahållna namnmatchning](#azure-provided-name-resolution)
* [Namnmatchning med hjälp av DNS-servern](#name-resolution-using-your-own-dns-server) (som kan vidarebefordra frågor toohello Azure-tillhandahållna DNS-servrar) 

hello typ av namnmatchning som du använder beror på hur dina virtuella datorer och rollinstanser måste toocommunicate med varandra.

**hello visar följande tabell scenarier och motsvarande name resolution lösningar:**

| **Scenario** | **Lösning** | **Suffix** |
| --- | --- | --- |
| Namnmatchning mellan rollinstanser eller virtuella datorer finns i hello samma cloud service eller ett virtuellt nätverk |[Azure-tillhandahållna namnmatchning](#azure-provided-name-resolution) |värdnamn eller fullständigt domännamn |
| Namnmatchning mellan rollinstanser eller virtuella datorer finns i olika virtuella nätverk |Kundhanterad DNS-servrar som vidarebefordrar frågor mellan vnet för matchning av Azure (DNS-proxy).  Se [namnmatchning med hjälp av DNS-servern](#name-resolution-using-your-own-dns-server) |Endast FQDN |
| Matchning av lokal dator- och namn från rollinstanser eller virtuella datorer i Azure |Kundhanterad DNS-servrar (t.ex. lokala domänkontrollant, lokala skrivskyddade domänkontrollanten eller en sekundär DNS synkroniseras med zonöverföringar).  Se [namnmatchning med hjälp av DNS-servern](#name-resolution-using-your-own-dns-server) |Endast FQDN |
| Lösning med Azure värdnamn från lokala datorer |Vidarebefordra frågor tooa kundhanterad DNS-proxyserver i hello motsvarande vnet hello proxyserver vidarebefordrar frågor tooAzure för matchning. Se [namnmatchning med hjälp av DNS-servern](#name-resolution-using-your-own-dns-server) |Endast FQDN |
| Omvänd DNS för interna IP-adresser |[Namnmatchning med hjälp av DNS-servern](#name-resolution-using-your-own-dns-server) |Saknas |
| Namnmatchning mellan VM: ar eller rollinstanser som finns i olika molntjänster, inte i ett virtuellt nätverk |Inte tillämpligt. Anslutningen mellan virtuella datorer och rollinstanser i olika molntjänster kan inte användas utanför ett virtuellt nätverk. |Saknas |

## <a name="azure-provided-name-resolution"></a>Azure-tillhandahållna namnmatchning
Azure erbjuder intern namnmatchning för virtuella datorer och rollinstanser som finns på hello samma virtuella nätverk eller molnet tjänst tillsammans med matchning av offentliga DNS-namn.  Virtuella datorer/instanser i en molntjänst delar hello samma DNS-suffix (så hello hostname enbart räcker) men i olika molntjänster i klassiska virtuella nätverk har olika DNS-suffix så hello FQDN är nödvändiga tooresolve namn mellan olika molntjänster.  I virtuella nätverk i hello Resource Manager-distributionsmodellen hello DNS-suffix är konsekvent på hello virtuella nätverk (så hello FQDN inte behövs) och DNS-namn som kan tilldelas tooboth nätverkskort och virtuella datorer. Även om Azure-tillhandahållna namnmatchning inte kräver någon konfiguration, är det inte hello valet för alla distributionsscenarier, som det visas på hello tabellen ovan.

> [!NOTE]
> Hello gäller webb-och arbetsroller, kan du också öppna hello interna IP-adresser för rollinstanser baserat på hello rollen namnet och instansen nummer med hello Azure Service Management REST API. Mer information finns i [Service Management REST API-referens](https://msdn.microsoft.com/library/azure/ee460799.aspx).
> 
> 

### <a name="features-and-considerations"></a>Funktioner och överväganden
**Funktioner:**

* Användarvänlighet: ingen konfiguration krävs i ordning toouse Azure-tillhandahållna namnmatchning.
* hello Azure-tillhandahållna namnmatchningstjänst har hög tillgänglighet, sparar du hello måste toocreate och hantera kluster för DNS-servrar.
* Kan användas tillsammans med dina egna DNS-servrar tooresolve både lokalt och Azure värdnamn.
* Namnmatchning tillhandahålls mellan rollen instanser/virtuella datorer i hello samma molntjänst utan behov av ett FQDN.
* Namnmatchning tillhandahålls mellan virtuella datorer i virtuella nätverk som använder hello Resource Manager-modellen, utan behov av hello FQDN. Virtuella nätverk i hello klassiska distributionsmodellen kräver hello FQDN vid namnmatchning i olika molntjänster. 
* Du kan använda värdnamn som bäst beskriver dina distributioner i stället för att arbeta med automatiskt genererat namn.

**Att tänka på:**

* hello Azure-skapa DNS-suffix kan inte ändras.
* Du kan registrera dina egna poster manuellt.
* WINS och NetBIOS stöds inte. (Du kan inte se dina virtuella datorer i Utforskaren i Windows.)
* Värdnamn måste vara DNS-kompatibel (måste de använda endast 0-9, a – z och '-', och får inte inledas eller avslutas med en '-'. Avsnittet RFC 3696 2.)
* DNS-frågorna begränsas för varje virtuell dator. Detta bör inte påverka de flesta program.  Om begäran begränsning observeras, kontrollerar du att cachelagring på klientsidan är aktiverad.  Mer information finns i [komma hello de flesta från Azure-tillhandahållna namnmatchning](#Getting-the-most-from-Azure-provided-name-resolution).
* Endast virtuella datorer i hello första 180 molntjänster har registrerats för varje virtuellt nätverk i en klassiska distributionsmodellen. Detta gäller inte toovirtual nätverk i Resource Manager distributionsmodellerna.

### <a name="getting-hello-most-from-azure-provided-name-resolution"></a>Hämta hello de flesta från Azure-tillhandahållna namnmatchning
**Klientcachelagring:**

Inte alla DNS-fråga måste skickas över nätverket hello toobe.  Klientcachelagring hjälper minskar svarstider och förbättrar återhämtning toonetwork signaler genom att lösa återkommande DNS-frågor från en lokal cache.  DNS-poster innehåller en Time-To-Live (TTL) som gör att hello toostore hello cacheposten så länge som möjligt utan att påverka poster dokumentens så klientcachelagring lämpar sig för de flesta situationer.

hello standard Windows DNS-klienten har en inbyggd DNS-cache.  Vissa Linux distributioner som inte inkluderar cachelagring som standard, rekommenderas att en läggs tooeach Linux VM (när du har kontrollerat att det inte är ett lokalt cacheminne redan).

Det finns ett antal olika DNS-cachelagring paket tillgängliga, t.ex. dnsmasq, kan du hello steg tooinstall dnsmasq på hello vanligaste distributioner:

* **Ubuntu (använder resolvconf)**:
  * installerar du bara hello dnsmasq paketet (”sudo lgh get installera dnsmasq”).
* **SUSE (använder netconf)**:
  * installera hello dnsmasq paketet (”sudo zypper installera dnsmasq”) 
  * Aktivera hello dnsmasq-tjänsten (”systemctl aktivera dnsmasq.service”) 
  * Starta hello dnsmasq-tjänsten (”systemctl start dnsmasq.service”) 
  * Redigera ”/ etc/sysconfig/nätverk/config” och ändra NETCONFIG_DNS_FORWARDER = ”” för ”dnsmasq”
  * Uppdatera resolv.conf (”netconfig update”) tooset hello-cachen som hello lokala DNS-matchning
* **OpenLogic (använder NetworkManager)**:
  * installera hello dnsmasq paketet (”sudo yum installera dnsmasq”)
  * Aktivera hello dnsmasq-tjänsten (”systemctl aktivera dnsmasq.service”)
  * Starta hello dnsmasq-tjänsten (”systemctl start dnsmasq.service”)
  * Lägg till ”lägga domän-namnservrar 127.0.0.1”; too"/etc/dhclient-eth0.conf”
  * Starta om hello network service (”tjänsten network omstart”) tooset hello cache vid hello lokala DNS-matchning

> [!NOTE]
> hello 'dnsmasq' paketet är endast ett av hello många DNS-cacheminnet som är tillgängliga för Linux.  Innan du använder den, kontrollera dess lämplighet för dina specifika behov och att ingen annan cache är installerad.
> 
> 

**Klientsidans återförsök:**

DNS är främst ett UDP-protokoll.  Som hello UDP-protokollet inte garantera meddelandeleverans, hanteras logik i hello DNS-protokollet sig själv.  Varje DNS-klient (operativsystemet) kan ha olika logik beroende på hello skapare inställningar:

* Windows-operativsystem försök igen efter 1 sekund och sedan igen efter en annan 2, 4 och en annan 4 sekunder. 
* hello standard Linux installationsprogrammet återförsök efter 5 sekunder.  Det är rekommenderat toochange denna tooretry 5 gånger på 1 sekunders intervall.  

toocheck hello aktuella inställningarna på en Linux VM 'cat /etc/resolv.conf' och titta på hello ”alternativ” rad, t.ex.:

    options timeout:1 attempts:5

Hej resolv.conf filen är vanligtvis automatiskt genererade och bör inte redigeras.  hello specifika stegen för att lägga till raden hello alternativen varierar beroende på distro:

* **Ubuntu** (använder resolvconf):
  * Lägg till hello alternativ rad too'/etc/resolveconf/resolv.conf.d/head' 
  * Kör resolvconf -u tooupdate
* **SUSE** (använder netconf):
  * Lägg till 'timeout:1 försök: 5' toohello NETCONFIG_DNS_RESOLVER_OPTIONS = ”” parametern i ”/ etc/sysconfig/nätverk/config” 
  * Kör ”netconfig uppdatera” tooupdate
* **OpenLogic** (använder NetworkManager):
  * Lägg till 'echo ”alternativ timeout:1 försök: 5”' too'/etc/NetworkManager/dispatcher.d/11-dhclient' 
  * Kör 'service nätverket restart' tooupdate

## <a name="name-resolution-using-your-own-dns-server"></a>Namnmatchning med hjälp av DNS-servern
Det finns ett antal olika situationer där din namnmatchningen kan gå utöver hello funktioner i Azure, till exempel när via Active Directory-domäner eller när du behöver DNS-matchning mellan virtuella nätverk (vnet).  toocover dessa scenarier Azure ger hello möjlighet för du toouse DNS-servrarna.  

DNS-servrar inom ett virtuellt nätverk kan vidarebefordra DNS-frågor tooAzure rekursiv matchare tooresolve värdnamn i det virtuella nätverket.  Till exempel en domänkontrollanten (DC) körs i Azure svara tooDNS frågor för dess domäner och vidarebefordra alla frågor tooAzure.  Detta gör att virtuella datorer toosee både lokala resurser (via hello DC) och Azure-tillhandahållna värdnamn (via hello vidarebefordrare).  Åtkomst tooAzure rekursiv matchare tillhandahålls via hello virtuella IP-Adressen 168.63.129.16.

DNS-vidarebefordran även gör bland vnet DNS-matchning och att dina lokala datorer tooresolve Azure-tillhandahållna värdnamn.  Order tooresolve värdnamnet för en virtuell dator, Virtuella hello DNS-server måste finnas i hello samma virtuella nätverk och vara konfigurerade tooforward hostname frågor tooAzure.  Som hello DNS-suffix skiljer sig åt i varje virtuellt nätverk, kan du använda villkorlig vidarebefordran regler toosend DNS-frågor toohello korrigera vnet för matchning.  hello följande bild visar två vnet och ett lokalt nätverk gör bland vnet DNS-matchning med den här metoden.  Ett exempel DNS-vidarebefordrare är tillgänglig i hello [Azure Quickstart mallgalleriet](https://azure.microsoft.com/documentation/templates/301-dns-forwarder/) och [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/301-dns-forwarder).

![Bland vnet DNS](./media/virtual-networks-name-resolution-for-vms-and-role-instances/inter-vnet-dns.png)

När du använder Azure-tillhandahållna namnmatchning, en intern DNS-suffix (*. internal.cloudapp.net) angivna tooeach VM använder DHCP.  Detta aktiverar värdnamnsmatchning som hello hostname-poster är i hello internal.cloudapp.net zon.  När du använder din egen lösning för name resolution tillhandahålls hello IDN-suffix är tooVMs eftersom den stör andra DNS-arkitekturer (t.ex. domänanslutna scenarier).  Vi ger i stället en icke-fungerande platshållare (reddog.microsoft.com).  

Om det behövs, kan hello interna DNS-suffix fastställas med PowerShell eller hello-API:

* För virtuella nätverk i Resource Manager distributionsmodellerna hello-suffix som är tillgängliga via hello [nätverkskort](https://msdn.microsoft.com/library/azure/mt163668.aspx) resurs eller via hello [Get-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619434.aspx) cmdlet.    
* I klassiska distributionsmodeller hello-suffix som är tillgängliga via hello [hämta distribution API](https://msdn.microsoft.com/library/azure/ee460804.aspx) anropa eller via hello [AzureVM Get-Debug](https://msdn.microsoft.com/library/azure/dn495236.aspx) cmdlet.

Om vidarebefordran av frågor tooAzure inte passar dina behov, behöver du tooprovide DNS-lösningen.  DNS-lösningen behöver du:

* Ange lämpliga värdnamnsmatchning, t.ex. [DDNS](virtual-networks-name-resolution-ddns.md).  Observera att om det finns poster med DDNS som du kan behöva toodisable DNS-posten rensning som Azures DHCP-lån är mycket långa och rensning kan ta bort DNS för tidigt. 
* Ange lämpliga rekursiv namnmatchning tooallow matchning av extern domännamn.
* Tillgänglig (TCP och UDP-port 53) från hello klienter den fungerar och att kan tooaccess vara hello internet.
* Skyddas mot åtkomst från hello internet, toomitigate hot med externa agenter.

> [!NOTE]
> För bästa prestanda när du använder Azure virtuella datorer som DNS-servrar, IPv6 bör inaktiveras och en [offentlig IP på instansnivå](virtual-networks-instance-level-public-ip.md) ska tilldelas tooeach DNS-server VM.  Om du väljer toouse Windows Server som DNS-servern, [i den här artikeln](http://blogs.technet.com/b/networking/archive/2015/08/19/name-resolution-performance-of-a-recursive-windows-dns-server-2012-r2.aspx) ger ytterligare prestandaanalys och optimeringar.
> 
> 

### <a name="specifying-dns-servers"></a>Ange DNS-servrar
När du använder DNS-servrar, Azure tillhandahåller hello möjlighet toospecify flera DNS-servrar per virtuellt nätverk eller gränssnitt (Resource Manager) per nätverk / molntjänst (klassisk).  DNS-servrar som angetts för ett moln service/nätverksgränssnitt får företräde över dem som anges för hello virtuella nätverket.

> [!NOTE]
> Nätverk anslutningsegenskaper, t.ex. DNS-server IP-adresser, inte bör redigeras direkt i virtuella Windows-datorer som de kan hämta raderas under läka när hello virtuella nätverkskort ersätts. 
> 
> 

När du använder hello Resource Manager-modellen, DNS-servrar kan anges i hello Portal, API-mallar ([vnet](https://msdn.microsoft.com/library/azure/mt163661.aspx), [nic](https://msdn.microsoft.com/library/azure/mt163668.aspx)) eller PowerShell ([vnet](https://msdn.microsoft.com/library/mt603657.aspx), [nic](https://msdn.microsoft.com/library/mt619370.aspx)).

När du använder hello klassiska distributionsmodellen, DNS-servrar för hello virtuellt nätverk kan anges i hello portalen eller [hello *nätverkskonfigurationen* filen](https://msdn.microsoft.com/library/azure/jj157100).  För molntjänster, hello DNS-servrar har angetts [hello *tjänstkonfiguration* filen](https://msdn.microsoft.com/library/azure/ee758710) eller i PowerShell ([ny AzureVM](https://msdn.microsoft.com/library/azure/dn495254.aspx)).

> [!NOTE]
> Om du ändrar hello DNS-inställningarna för en virtuell nätverk/virtuell dator som redan har distribuerats, behöver du toorestart varje berörda VM för hello ändringar tootake effekt.
> 
> 

## <a name="next-steps"></a>Nästa steg
Resource Manager-modellen:

* [Skapa eller uppdatera ett virtuellt nätverk](https://msdn.microsoft.com/library/azure/mt163661.aspx)
* [Skapa eller uppdatera ett nätverkskort](https://msdn.microsoft.com/library/azure/mt163668.aspx)
* [Ny AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603657.aspx)
* [Ny AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619370.aspx)

Klassiska distributionsmodellen:

* [Konfigurationsschemat för Azure-tjänst](https://msdn.microsoft.com/library/azure/ee758710)
* [Konfigurationsschemat för virtuellt nätverk](https://msdn.microsoft.com/library/azure/jj157100)
* [Konfigurera ett virtuellt nätverk med hjälp av en konfigurationsfil för nätverk](virtual-networks-using-network-configuration-file.md) 

