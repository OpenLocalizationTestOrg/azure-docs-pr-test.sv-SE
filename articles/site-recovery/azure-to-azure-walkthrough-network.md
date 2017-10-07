---
title: "aaaPlan nätverksfunktioner för VMware tooAzure replikeringen med Site Recovery | Microsoft Docs"
description: "Den här artikeln beskrivs planering av krävs vid replikering av virtuella datorer i Azure med Azure Site Recovery"
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
ms.date: 08/01/2017
ms.author: sujayt
ms.openlocfilehash: e4036351ca707bd4966cf2a855d4a162f88153e8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-3-plan-networking-for-azure-vm-replication"></a>Steg 3: Planera nätverk för Virtuella Azure-replikering

När du har kontrollerat hello [kraven för distribution av](azure-to-azure-walkthrough-prerequisites.md), läsa den här artikeln toounderstand hello-nätverk att tänka på när replikera och återställa virtuella Azure-datorer (VM) från en Azure-region tooanother med hello Azure Site Recovery-tjänsten. 

- När du är klar hello artikeln bör du ha en Rensa förståelse av hur tooset in utgående åtkomst för virtuella Azure-datorer du vill tooreplicate och hur tooconnect från din lokala plats toothose virtuella datorer.
- Skicka kommentarer längst ned hello i den här artikeln eller ställa frågor i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

>[!NOTE]
> Azure VM-replikeringen med Site Recovery är för närvarande under förhandsgranskning.



## <a name="network-overview"></a>Översikt över nätverk

Vanligtvis ditt virtuella Azure-datorer finns i ett Azure virtual network/undernät och det finns en anslutning från din lokala plats tooAzure med hjälp av Azure ExpressRoute eller en VPN-anslutning.

Nätverk skyddas vanligtvis använder brandväggar och/eller nätverkssäkerhetsgrupper (NSG: er). Brandväggar kan använda URL-baserade på IP-baserade listor toocontrol nätverksanslutning. NSG: er använda IP-intervallet-baserade regler. 


För Site Recovery toowork förväntades, behöver du toomake några ändringar i utgående nätverksanslutningen, från virtuella datorer som du vill tooreplicate. Site Recovery stöder inte användning av en nätverksanslutning för autentisering proxy toocontrol. Om du har en autentiseringsproxy går inte att aktivera replikering. 


hello visar följande diagram en normal miljö för ett program som körs på virtuella Azure-datorer.

![kund-miljö](./media/azure-to-azure-walkthrough-network/source-environment.png)

**Azure VM-miljön**

Du kan också ha en anslutning tooAzure konfigurera från din lokala plats med hjälp av Azure ExpressRoute eller en VPN-anslutning. 

![kund-miljö](./media/azure-to-azure-walkthrough-network/source-environment-expressroute.png)

**Lokal anslutning tooAzure**



## <a name="outbound-connectivity-for-urls"></a>Utgående anslutning för URL: er

Om du använder en utgående anslutning för URL-baserade brandväggen proxy toocontrol, kontrollera att du tillåter dessa URL: er som används av Site Recovery

**URL: EN** | **Detaljer**  
--- | ---
*.blob.core.windows.net | Kan data toobe skrivs från hello VM, toohello cache storage-konto i hello källa region.
login.microsoftonline.com | Innehåller autentiseringen och auktoriseringen av tooSite Recovery-webbadresser.
*.hypervrecoverymanager.windowsazure.com | Tillåter kommunikation med hello Site Recovery-tjänsten från hello VM.
*. servicebus.windows.net | Krävs för att hello Site Recovery övervakning och diagnostik data kan skrivas från hello VM.

## <a name="outbound-connectivity-for-ip-address-ranges"></a>Utgående anslutning för IP-adressintervall

- Du kan automatiskt skapa regler för hello krävs på hello NSG genom att hämta och kör [skriptet](https://gallery.technet.microsoft.com/Azure-Recovery-script-to-0c950702).
- Vi rekommenderar att du skapar hello krävs NSG-regler på ett test NSG och kontrollera att det inte finns några problem innan du skapar hello regler på en NSG för produktion.
- toocreate hello krävs antalet NSG-regler, se till att prenumerationen är godkända. Kontakta supporten tooincrease hello NSG regeln gräns i din prenumeration.
Om du använder någon IP-baserade brandväggen proxy- eller NSG-regler toocontrol utgående anslutning måste hello följande IP-adressintervall toobe godkända, beroende på hello käll- och målplatserna hello virtuella datorer:

    - Alla IP-adressintervall som motsvarar toohello källplats. Hämta hello [intervall](https://www.microsoft.com/download/confirmation.aspx?id=41653).) Vitlistning krävs, så att data kan skrivas toohello cache storage-konto från hello VM.
    - Alla IP-adressintervall som motsvarar tooOffice 365 [autentisering och identitet IP V4-slutpunkter](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity). Om nya IP-adresser läggs tooOffice 365 IP-adressintervall, måste toocreate ny NSG-regler.
-     Site Recovery-tjänsten endpoint IP-adresser ([tillgängliga i en XML-fil](https://aka.ms/site-recovery-public-ips)), som beror på din målplats: 

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

## <a name="example-nsg-configuration"></a>NSG exempelkonfiguration

Det här avsnittet visar hur tooconfigure NSG regler så att replikeringar fungerar för en virtuell dator. Om du använder NSG-regler toocontrol utgående anslutning kan du använda ”Tillåt HTTPS utgående” regler för alla hello som krävs för IP-adressintervall.

I det här exemplet är hello VM källplats ”östra USA”. målplatsen för hello replikering är centrala USA.


### <a name="example"></a>Exempel

#### <a name="east-us"></a>Östra USA

1. Skapa regler som motsvarar för[östra USA IP-intervall](https://www.microsoft.com/download/confirmation.aspx?id=41653). Detta krävs så att data kan skrivas toohello cache storage-konto från hello VM.
2. Skapa regler för alla IP-adressintervall som motsvarar tooOffice 365 [autentisering och identitet IP V4-slutpunkter](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity).
3. Skapa regler som motsvarar toohello målplatsen:

   **Plats** | **Site Recovery-tjänsten IP-adresser** |  **Site Recovery övervakning IP**
    --- | --- | ---
   Centrala USA | 40.69.144.231</br>40.69.167.116 | 52.165.34.144

#### <a name="central-us"></a>Centrala USA

Reglerna krävs så att replikering kan aktiveras från hello region toohello källa målregionen, efter växling vid fel.

1. Skapa regler som motsvarar för[centrala USA IP-intervall](https://www.microsoft.com/download/confirmation.aspx?id=41653).
2. Skapa regler för alla IP-adressintervall som motsvarar tooOffice 365 [autentisering och identitet IP V4-slutpunkter](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity).
3. Skapa regler som motsvarar toohello källplats:

   **Plats** | **Site Recovery-tjänsten IP-adresser** |  **Site Recovery övervakning IP**
    --- | --- | ---
   Östra USA | 13.82.88.226</br>40.71.38.173 | 104.45.147.24


## <a name="existing-on-premises-connection"></a>Befintlig lokal anslutning

Om du har en ExpressRoute- eller VPN-anslutningen mellan din lokala plats och hello källplats i Azure, Följ dessa riktlinjer:

**Konfiguration** | **Detaljer**
--- | ---
**Tvingad tunneltrafik** | Vanligtvis tvingar en standardväg (0.0.0.0/0) utgående internet-trafik tooflow via hello lokal plats. Vi rekommenderar inte. Replikeringstrafik och Site Recovery kommunikation ska hålla sig inom Azure.<br/><br/> Lägga till användardefinierade vägar (udr: er) som en lösning för [dessa IP-adressintervall](#outbound-connectivity-for-azure-site-recovery-ip-ranges), så att hello-trafik inte går lokalt.
**Anslutning** | Om appar behöver tooconnect tooon lokala datorer eller lokala klienter ansluter toohello app över lokala via VPN/ExpressRoute, kontrollera att du har minst en [plats-till-plats-anslutning](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md), mellan hello mål Azure-region och hello lokal plats.<br/><br/> Om trafikvolymer är högt mellan hello mål Azure-region och hello lokal plats, kan du skapa en ny [ExpressRoute-anslutning](../expressroute/expressroute-introduction.md), mellan hello målregionen och lokalt.<br/><br/> Om du vill tooretain IP-adresser för virtuella datorer efter redundans, Behåll hello mål region plats-till-plats/ExpressRoute-anslutning i frånkopplat tillstånd. Detta säkerställer att det finns inga intervall konflikt mellan hello käll- och IP-adressintervall.
**ExpressRoute** | Skapa en ExpressRoute-krets i hello käll- och regioner.<br/><br/> Skapa en anslutning mellan hello källnätverket och hello ExpressRoute-krets och mellan hello målnätverket och hello krets.<br/><br/> Vi rekommenderar att du använder olika IP-adressintervall i käll- och områden. hello krets kommer inte att kunna tooconnect toomore än en Azure-nätverk med hello samma IP-adressintervall på hello samtidigt.<br/><br/> Du kan skapa virtuella nätverk med hello samma IP-intervall i båda regioner och sedan skapa ExpressRoute-kretsar i båda regioner. Koppla hello krets från hello källa virtuella nätverket för växling vid fel, och Anslut hello krets i hello mål virtuella nätverk.<br/><br/> Om hello primära regionen är helt nedåt hello koppla från åtgärden misslyckas. I det här fallet kommer hello virtuella målnätverket inte ExpressRoute-anslutning.
**ExpressRoute Premium** | Du kan skapa kretsar i hello samma geopolitiska region.<br/><br/> toocreate kretsar i olika geopolitiska regioner, och du behöver Azure ExpressRoute Premium.<br/><br/> Hello kostnaden är inkrementell med Premium. Om du redan använder den, är det utan extra kostnad.<br/><br/> Lär dig mer om [platser](../expressroute/expressroute-locations.md#azure-regions-to-expressroute-locations-within-a-geopolitical-region) och [priser](https://azure.microsoft.com/pricing/details/expressroute/).



## <a name="next-steps"></a>Nästa steg

Gå för[steg 4: skapa ett valv](azure-to-azure-walkthrough-vault.md)

