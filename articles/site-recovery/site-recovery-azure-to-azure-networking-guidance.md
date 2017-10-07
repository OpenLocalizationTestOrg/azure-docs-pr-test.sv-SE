---
title: "aaaAzure Site Recovery nätverk vägledning för replikering av virtuella datorer från Azure tooAzure | Microsoft Docs"
description: "Nätverk vägledning för att replikera virtuella Azure-datorer"
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/13/2017
ms.author: sujayt
ms.openlocfilehash: 3a3391b8c3512932d243458fd17d2a2b39248448
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="networking-guidance-for-replicating-azure-virtual-machines"></a>Nätverk vägledning för att replikera virtuella Azure-datorer

>[!NOTE]
> Site Recovery replikering för virtuella Azure-datorer är för närvarande under förhandsgranskning.

Den här artikeln information hello nätverk för Azure Site Recovery när du replikerar och återställa virtuella Azure-datorer från en region tooanother region. Mer information om krav för Azure Site Recovery finns hello [krav](site-recovery-prereq.md) artikel.

## <a name="site-recovery-architecture"></a>Site Recovery-arkitekturen

Site Recovery tillhandahåller en enkel och enkelt sätt tooreplicate program som körs på virtuella Azure-datorer tooanother Azure-region så att de kan återställas om det finns ett avbrott i hello primär region. Lär dig mer om [det här scenariot och Site Recovery-arkitekturen](site-recovery-azure-to-azure-architecture.md).

## <a name="your-network-infrastructure"></a>Nätverkets infrastruktur

hello visar följande diagram hello vanliga Azure-miljön för ett program som körs på virtuella Azure-datorer:

![kund-miljö](./media/site-recovery-azure-to-azure-architecture/source-environment.png)

Om du använder Azure ExpressRoute eller en VPN-anslutning från ett lokalt nätverk tooAzure hello miljö ser ut så här:

![kund-miljö](./media/site-recovery-azure-to-azure-architecture/source-environment-expressroute.png)

Normalt skydda kunder sina nätverk med brandväggar och/eller nätverkssäkerhetsgrupper (NSG: er). hello brandväggar kan använda antingen URL- eller IP-baserade vitlistning för att kontrollera nätverksanslutningen. NSG: er tillåter regler för att använda IP-adressintervall toocontrol nätverksanslutning.

>[!IMPORTANT]
> Om du använder en autentiserad proxyserver toocontrol nätverksanslutningen stöds inte och går inte att aktivera replikering för Site Recovery. 

hello beskrivs följande avsnitt hello utgående ändringar i nätverksanslutningar som krävs från Azure virtuella datorer för Site Recovery replikering toowork.

## <a name="outbound-connectivity-for-azure-site-recovery-urls"></a>Utgående anslutning för Azure Site Recovery URL: er

Om du använder en utgående anslutning för URL-baserade brandväggen proxy toocontrol vara säker på att toowhitelist följande obligatoriska Azure Site Recovery-webbadresser:


**URL: EN** | **Syfte**  
--- | ---
*.blob.core.windows.net | Krävs så att data kan skrivas toohello cache storage-konto i hello källa region från hello VM.
login.microsoftonline.com | Krävs för autentiseringen och auktoriseringen av toohello Site Recovery-webbadresser.
*.hypervrecoverymanager.windowsazure.com | Krävs för att hello Site Recovery-tjänsten kan kommunicera från hello VM.
*. servicebus.windows.net | Krävs för att hello Site Recovery övervakning och diagnostik data kan skrivas från hello VM.

## <a name="outbound-connectivity-for-azure-site-recovery-ip-ranges"></a>Utgående anslutning för Azure Site Recovery IP-adressintervall

>[!NOTE]
> tooautomatically skapa hello krävs NSG-regler på hello nätverkssäkerhetsgruppen, kan du [hämtar och använder det här skriptet](https://gallery.technet.microsoft.com/Azure-Recovery-script-to-0c950702).

>[!IMPORTANT]
> * Vi rekommenderar att du skapar hello krävs NSG-regler på en test nätverkssäkerhetsgrupp och kontrollera att det inte finns några problem innan du skapar hello regler på en nätverkssäkerhetsgrupp för produktion.
> * toocreate hello krävs antalet NSG-regler, se till att prenumerationen är godkända. Kontakta supporten tooincrease hello NSG regeln gräns i din prenumeration.

Om du använder någon IP-baserade brandväggen proxy- eller NSG-regler toocontrol utgående anslutning måste hello följande IP-adressintervall toobe godkända, beroende på hello käll- och målplatserna hello virtuella datorer:

- Alla IP-adressintervall som motsvarar toohello källplats. (Du kan hämta hello [IP-adressintervall](https://www.microsoft.com/download/confirmation.aspx?id=41653).) Vitlistning krävs så att data kan skrivas toohello cache storage-konto från hello VM.

- Alla IP-adressintervall som motsvarar tooOffice 365 [autentisering och identitet IP V4-slutpunkter](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity).

    >[!NOTE]
    > Om nya IP-adresser läggs tooOffice 365 IP-adressintervall i hello framtida, måste toocreate ny NSG-regler.
    
- Site Recovery tjänstslutpunkten IP-adresser ([tillgängliga i en XML-fil](https://aka.ms/site-recovery-public-ips)), som beror på din målplats: 

   **Målplats** | **Site Recovery-tjänsten IP-adresser** |  **Site Recovery övervakning IP**
   --- | --- | ---
   Östasien | 52.175.17.132</br>40.83.121.61 | 13.94.47.61
   Sydostasien | 52.187.58.193</br>52.187.169.104 | 13.76.179.223
   Indien, centrala | 52.172.187.37</br>52.172.157.193 | 104.211.98.185
   Södra Indien | 52.172.46.220</br>52.172.13.124 | 104.211.224.190
   Norra centrala USA | 23.96.195.247</br>23.96.217.22 | 168.62.249.226
   Norra Europa | 40.69.212.238</br>13.74.36.46 | 52.169.18.8
   Västra Europa | 52.166.13.64</br>52.166.6.245 | 40.68.93.145
   Östra USA | 13.82.88.226</br>40.71.38.173 | 104.45.147.24
   Västra USA | 40.83.179.48</br>13.91.45.163 | 104.40.26.199
   Södra centrala USA | 13.84.148.14</br>13.84.172.239 | 104.210.146.250
   Centrala USA | 40.69.144.231</br>40.69.167.116 | 52.165.34.144
   Östra USA 2 | 52.184.158.163</br>52.225.216.31 | 40.79.44.59
   Östra Japan | 52.185.150.140</br>13.78.87.185 | 138.91.1.105
   Västra Japan | 52.175.146.69</br>52.175.145.200 | 138.91.17.38
   Södra Brasilien | 191.234.185.172</br>104.41.62.15 | 23.97.97.36
   Östra Australien | 104.210.113.114</br>40.126.226.199 | 191.239.64.144
   Sydöstra Australien | 13.70.159.158</br>13.73.114.68 | 191.239.160.45
   Centrala Kanada | 52.228.36.192</br>52.228.39.52 | 40.85.226.62
   Östra Kanada | 52.229.125.98</br>52.229.126.170 | 40.86.225.142
   Västra centrala USA | 52.161.20.168</br>13.78.230.131 | 13.78.149.209
   Västra USA 2 | 52.183.45.166</br>52.175.207.234 | 13.66.228.204
   Storbritannien, västra | 51.141.3.203</br>51.140.226.176 | 51.141.14.113
   Storbritannien, södra | 51.140.43.158</br>51.140.29.146 | 51.140.189.52

## <a name="sample-nsg-configuration"></a>Exempel på NSG konfiguration
Det här avsnittet beskrivs hello steg tooconfigure NSG-regler så att Site Recovery replikering kan fungera på en virtuell dator. Om du använder NSG-regler toocontrol utgående anslutning, kan du använda ”Tillåt HTTPS utgående” regler för alla hello som krävs för IP-adressintervall.

>[!Note]
> tooautomatically skapa hello krävs NSG-regler på hello nätverkssäkerhetsgruppen, kan du [hämtar och använder det här skriptet](https://gallery.technet.microsoft.com/Azure-Recovery-script-to-0c950702).

Till exempel om den Virtuella datorns plats har ”östra USA” och din replikering målplatsen är ”centrala oss” åtgärderna hello i hello följande två avsnitt.

>[!IMPORTANT]
> * Vi rekommenderar att du skapar hello krävs NSG-regler på en test nätverkssäkerhetsgrupp och kontrollera att det inte finns några problem innan du skapar hello regler på en nätverkssäkerhetsgrupp för produktion.
> * toocreate hello krävs antalet NSG-regler, se till att prenumerationen är godkända. Kontakta supporten tooincrease hello NSG regeln gräns i din prenumeration. 

### <a name="nsg-rules-on-hello-east-us-network-security-group"></a>NSG-regler på hello östra USA nätverkssäkerhetsgrupp

* Skapa regler som motsvarar för[östra USA IP-intervall](https://www.microsoft.com/download/confirmation.aspx?id=41653). Detta krävs så att data kan skrivas toohello cache storage-konto från hello VM.

* Skapa regler för alla IP-adressintervall som motsvarar tooOffice 365 [autentisering och identitet IP V4-slutpunkter](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity).

* Skapa regler som motsvarar toohello målplatsen:

   **Plats** | **Site Recovery-tjänsten IP-adresser** |  **Site Recovery övervakning IP**
    --- | --- | ---
   Centrala USA | 40.69.144.231</br>40.69.167.116 | 52.165.34.144

### <a name="nsg-rules-on-hello-central-us-network-security-group"></a>NSG-regler på hello centrala USA nätverkssäkerhetsgrupp

Reglerna krävs så att replikering kan aktiveras från hello mål region toohello källa region postredundans:

* Regler som motsvarar för[centrala USA IP-intervall](https://www.microsoft.com/download/confirmation.aspx?id=41653). Dessa krävs så att data kan skrivas toohello cache storage-konto från hello VM.

* Regler för alla IP-adressintervall som motsvarar tooOffice 365 [autentisering och identitet IP V4-slutpunkter](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity).

* Regler som motsvarar toohello källplats:

   **Plats** | **Site Recovery-tjänsten IP-adresser** |  **Site Recovery övervakning IP**
    --- | --- | ---
   Östra USA | 13.82.88.226</br>40.71.38.173 | 104.45.147.24


## <a name="guidelines-for-existing-azure-to-on-premises-expressroutevpn-configuration"></a>Riktlinjer för befintliga Azure till lokala ExpressRoute/VPN-konfiguration

Om du har en ExpressRoute- eller VPN-anslutning mellan lokala och hello datakällplats i Azure, följ hello riktlinjerna i det här avsnittet.

### <a name="forced-tunneling-configuration"></a>Tvingad tunneltrafik konfiguration

En vanlig konfiguration av customer är toodefine en standardväg (0.0.0.0/0) som tvingar utgående Internet-trafik tooflow via hello lokal plats. Vi rekommenderar inte detta. lämna inte hello Azure gräns hello replikeringstrafik och Site Recovery service-kommunikation. hello lösningen är tooadd användardefinierade vägar (udr: er) för [dessa IP-adressintervall](#outbound-connectivity-for-azure-site-recovery-ip-ranges) så att hello replikeringstrafiken inte gå lokalt.

### <a name="connectivity-between-hello-target-and-on-premises-location"></a>Anslutningen mellan hello mål och lokala plats

Följ dessa riktlinjer för anslutningar mellan hello mål och hello lokal plats:
- Om programmet behöver tooconnect toohello lokala datorer eller om det finns klienter som ansluter toohello program från lokala via VPN/ExpressRoute, kontrollera att du har minst en [plats-till-plats-anslutning](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) mellan ditt mål Azure region och hello lokalt datacenter.

- Om du förväntar dig en stor mängd trafik tooflow mellan dina mål Azure-region och hello lokala datacenter, bör du skapa en annan [ExpressRoute-anslutning](../expressroute/expressroute-introduction.md) mellan hello mål Azure region och hello lokalt datacenter.

- Om du vill tooretain IP-adresser för hello virtuella datorer när de växlar över, Behåll hello mål region plats-till-plats/ExpressRoute-anslutning i frånkopplat tillstånd. Detta är att det finns inga intervall krockar mellan hello källa region IP-intervall och IP-adressintervall för målet region toomake.

### <a name="best-practices-for-expressroute-configuration"></a>Metodtips för ExpressRoute-konfiguration
Följa dessa rekommendationer för ExpressRoute-konfiguration:

- Du måste toocreate en ExpressRoute-krets i båda hello käll- och områden. Sedan måste toocreate en anslutning mellan:
  - hello källa virtuella nätverk och hello ExpressRoute-kretsen.
  - virtuella hello målnätverket och hello ExpressRoute-kretsen.

- Som en del av ExpressRoute-standard, kan du skapa kretsar i hello samma geopolitiska region. toocreate ExpressRoute-kretsar i olika geopolitiska regioner, Azure ExpressRoute Premium krävs, vilket innebär att en inkrementell kostnad. (Om du redan använder ExpressRoute Premium, det finns inget extra kostnad.) Mer information finns i hello [ExpressRoute platser dokumentet](../expressroute/expressroute-locations.md#azure-regions-to-expressroute-locations-within-a-geopolitical-region) och [ExpressRoute priser](https://azure.microsoft.com/pricing/details/expressroute/).

- Vi rekommenderar att du använder olika IP-adressintervall i käll- och områden. Hej ExpressRoute-kretsen kan inte tooconnect med två virtuella Azure-nätverk av hello samma IP-intervall på hello samtidigt.

- Du kan skapa virtuella nätverk med hello samma IP-intervall i båda regioner och sedan skapa ExpressRoute-kretsar i båda regioner. Hello gäller en redundansväxling, koppla hello krets från hello källa virtuella nätverket och Anslut hello krets i hello mål virtuella nätverk.

 >[!IMPORTANT]
 > Om hello primära regionen är helt nedåt hello koppla från åtgärden misslyckas. Som förhindrar att virtuella hello målnätverket komma ExpressRoute-anslutning.

## <a name="next-steps"></a>Nästa steg
Börja skydda dina arbetsbelastningar av [replikering av Azure virtuella datorer](site-recovery-azure-to-azure.md).
