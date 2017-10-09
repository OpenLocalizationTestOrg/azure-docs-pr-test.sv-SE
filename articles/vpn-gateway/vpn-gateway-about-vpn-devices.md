---
title: "aaaAbout VPN-enheter för anslutningar mellan lokala Azure-anslutningar | Microsoft Docs"
description: "Den här artikeln beskriver VPN-enheter och IPSec-parametrar för S2S VPN Gateway-anslutningar på olika platser. Länkar finns tooconfiguration anvisningar och exempel."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: azure-resource-manager, azure-service-management
ms.assetid: ba449333-2716-4b7f-9889-ecc521e4d616
ms.service: vpn-gateway
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/14/2017
ms.author: yushwang;cherylmc
ms.openlocfilehash: 8b84afbf93d807342ecd56ab369d5909a13343e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="about-vpn-devices-and-ipsecike-parameters-for-site-to-site-vpn-gateway-connections"></a>Om VPN-enheter och IPSec-/IKE-parametrar för anslutningar för VPN-gateway från plats till plats

En VPN-enhet är obligatoriska tooconfigure en plats-till-plats (S2S) mellan lokala VPN-anslutning via en VPN-gateway. Plats-till-plats-anslutningar kan vara används toocreate hybridlösning eller om du vill skapa säkra anslutningar mellan ditt lokala nätverk och dina virtuella nätverk. Den här artikeln innehåller en lista över verifierade VPN-enheter och en lista över IPsec-/IKE-parametrar för VPN-gatewayer.

> [!IMPORTANT]
> Om du stöter på problem med nätverksanslutningen mellan din lokala VPN-enheter och VPN-gatewayer finns för[kända kompatibilitetsproblem för enheten](#known).
>
>

### <a name="items-toonote-when-viewing-hello-tables"></a>Objekt toonote när du visar hello tabeller:

* Terminologin har ändrats för Azure VPN-gateways. Hello-namn har ändrats. Funktionaliteten har inte ändrats.
  * Statisk routning = Principbaserad
  * Dynamisk routning = Routningsbaserad
* Specifikationer för HighPerformance VPN-gateway och RouteBased VPN-gateway är hello samma, om inget annat anges. Till exempel är hello verifiera VPN-enheter som är kompatibla med RouteBased VPN-gatewayer också kompatibla med hello HighPerformance VPN-gateway.

## <a name="devicetable"></a>Validerade VPN-enheter och guider för enhetskonfiguration

> [!NOTE]
> När du konfigurerar en plats-till-plats-anslutning krävs en offentlig IPv4-adress för VPN-enheten.
>

Vi har verifierat en uppsättning VPN-standardenheter tillsammans med våra enhetsleverantörer. Alla hello enheter i hello enhetsfamiljer i följande lista hello arbeta med VPN-gatewayer. Se [om VPN-Gateway inställningar](vpn-gateway-about-vpn-gateway-settings.md#vpntype) toounderstand hello VPN skriver användning (PolicyBased eller RouteBased) för hello VPN Gateway-lösning som du vill tooconfigure.

toohelp konfigurera din VPN-enhet, finns toohello länkar som motsvarar tooappropriate enhetsfamilj. hello länkar tooconfiguration anvisningar finns på grundval av bästa prestanda. Kontakta enhetens tillverkare för att se vilka VPN-enheter som stöds.

|**Leverantör**          |**Enhetsfamilj**     |**Minsta operativsystemversion** |**Instruktioner för principbaserad konfiguration** |**Instruktioner för routningsbaserad konfiguration** |
| ---                | ---                  | ---                   | ---            | ---           |
| A10 Networks, Inc. |Thunder CFW           |ACOS 4.1.1             |Inte kompatibel  |[Konfigurationsguide](https://www.a10networks.com/resources/deployment-guides/a10-thunder-cfw-ipsec-vpn-interoperability-azure-vpn-gateways)|
| Allied Telesis     |AR-serie VPN-routrar |2.9.2                  |Kommer snart     |Inte kompatibel  |
| Barracuda Networks, Inc. |Barracuda NextGen Firewall F-serien |Principbaserad: 5.4.3<br>Routningsbaserad: 6.2.0 |[Konfigurationsguide](https://techlib.barracuda.com/NGF/AzurePolicyBasedVPNGW) |[Konfigurationsguide](https://techlib.barracuda.com/NGF/AzureRouteBasedVPNGW) |
| Barracuda Networks, Inc. |Barracuda NextGen Firewall X-serien |Barracuda Firewall 6.5 |[Konfigurationsguide](https://techlib.barracuda.com/BFW/ConfigAzureVPNGateway) |Inte kompatibel |
| Brocade            |Vyatta 5400 vRouter   |Virtuell router 6.6R3 GA|[Konfigurationsguide](http://www1.brocade.com/downloads/documents/html_product_manuals/vyatta/vyatta_5400_manual/wwhelp/wwhimpl/js/html/wwhelp.htm#href=VPN_Site-to-Site%20IPsec%20VPN/Preface.1.1.html) |Inte kompatibel |
| Check Point |Security Gateway |R77.30 |[Konfigurationsguide](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275) |[Konfigurationsguide](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275) |
| Cisco              |ASA       |8.3 |[Konfigurationsexempel](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASA) |Inte kompatibel |
| Cisco |ASR |Principbaserad: IOS 15.1<br>Routningsbaserad: IOS 15.2 |[Konfigurationsexempel](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASR) |[Konfigurationsexempel](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASR) |
| Cisco |ISR |Principbaserad: IOS 15.0<br>Routningsbaserad*: IOS 15.1 |[Konfigurationsexempel](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ISR) |[Konfigurationsexempel*](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ISR) |
| Citrix |NetScaler MPX, SDX, VPX |10.1 och senare |[Konfigurationsguide](https://docs.citrix.com/en-us/netscaler/11-1/system/cloudbridge-connector-introduction/cloudbridge-connector-azure.html) |Inte kompatibel |
| F5 |BIG-IP-serien |12.0 |[Konfigurationsguide](https://devcentral.f5.com/articles/connecting-to-windows-azure-with-the-big-ip) |[Konfigurationsguide](https://devcentral.f5.com/articles/big-ip-to-azure-dynamic-ipsec-tunneling) |
| Fortinet |FortiGate |FortiOS 5.4.2 |  |[Konfigurationsguide](http://cookbook.fortinet.com/ipsec-vpn-microsoft-azure-54) |
| Internet Initiative Japan (IIJ) |SEIL-serien |SEIL/X 4.60<br>SEIL/B1 4.60<br>SEIL/x86 3.20 |[Konfigurationsguide](http://www.iij.ad.jp/biz/seil/ConfigAzureSEILVPN.pdf) |Inte kompatibel |
| Juniper |SRX |Principbaserad: JunOS 10.2<br>Routningsbaserad: JunOSS 11.4 |[Konfigurationsexempel](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SRX) |[Konfigurationsexempel](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SRX) |
| Juniper |J-serien |Principbaserad: JunOS 10.4r9<br>Routningsbaserad: JunOSS 11.4 |[Konfigurationsexempel](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/JSeries) |[Konfigurationsexempel](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/JSeries) |
| Juniper |ISG |ScreenOS 6.3 |[Konfigurationsexempel](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/ISG) |[Konfigurationsexempel](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/ISG) |
| Juniper |SSG |ScreenOS 6.2 |[Konfigurationsexempel](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SSG) |[Konfigurationsexempel](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SSG) |
| Microsoft |Routning och fjärråtkomst |Windows Server 2012 |Inte kompatibel |[Konfigurationsexempel](http://go.microsoft.com/fwlink/p/?LinkId=717761) |
| Open Systems AG |Mission Control Security Gateway |Saknas |[Konfigurationsguide](https://www.open.ch/_pdf/Azure/AzureVPNSetup_Installation_Guide.pdf) |Inte kompatibel |
| Openswan |Openswan |2.6.32 |(Kommer snart) |Inte kompatibel |
| Palo Alto Networks |Alla enheter som kör PAN-OS |PAN-OS<br>Principbaserad: 6.1.5 eller senare<br>Routningsbaserad: 7.1.4 |[Konfigurationsguide](https://live.paloaltonetworks.com/t5/Configuration-Articles/How-to-Configure-VPN-Tunnel-Between-a-Palo-Alto-Networks/ta-p/59065) |[Konfigurationsguide](https://live.paloaltonetworks.com/t5/Integration-Articles/Configuring-IKEv2-VPN-for-Microsoft-Azure-Environment/ta-p/60340) |
| SonicWall |TZ-serie, NSA-serie<br>SuperMassive-serie<br>NSA-serie i E-klassen |SonicOS 5.8.x<br>SonicOS 5.9.x<br>SonicOS 6.x |[Konfigurationsguide för SonicOS 6.2](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide?ParentProduct=646)<br>[Konfigurationsguide för SonicOS 5.9](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide?ParentProduct=850) |[Konfigurationsguide för SonicOS 6.2](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide?ParentProduct=646)<br>[Konfigurationsguide för SonicOS 5.9](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide?ParentProduct=850) |
| WatchGuard |Alla |Fireware XTM<br> Principbaserad: v11.11.x<br>Routningsbaserad: v11.12.x |[Konfigurationsguide](http://watchguardsupport.force.com/publicKB?type=KBArticle&SFDCID=kA2F00000000LI7KAM&lang=en_US) |[Konfigurationsguide](http://watchguardsupport.force.com/publicKB?type=KBArticle&SFDCID=kA22A000000XZogSAG&lang=en_US)|

(*) ISR 7200 serie routrar stöder endast principbaserade VPN.

## <a name="additionaldevices"></a>Icke-verifierade VPN-enheter

Om du inte ser enheten visas i hello verifiera VPN-enheter tabell fungera enheten fortfarande med en plats-till-plats-anslutning. Kontakta enhetstillverkaren för ytterligare information om support och konfiguration.

## <a name="editing"></a>Redigera enhetens konfigurationsexempel

När du har hämtat hello angivna VPN-enhet configuration exempel behöver tooreplace vissa hello värden tooreflect hello inställningar för din miljö.

### <a name="tooedit-a-sample"></a>tooedit ett exempel:

1. Öppna hello exempel med hjälp av anteckningar.
2. Söka och ersätta alla <*text*> strängar hello värden som passar tooyour miljö. Vara säker på att tooinclude < och >. När ett namn har angetts måste hello-namn som du väljer vara unika. Om ett kommando inte fungerar kan du läsa mer i din enhetstillverkares dokumentation.

| **Exempeltext** | **Ändra till** |
| --- | --- |
| &lt;RP_OnPremisesNetwork&gt; |Ditt valda namn för det här objektet. Exempel: myOnPremisesNetwork |
| &lt;RP_AzureNetwork&gt; |Ditt valda namn för det här objektet. Exempel: myAzureNetwork |
| &lt;RP_AccessList&gt; |Ditt valda namn för det här objektet. Exempel: myAzureAccessList |
| &lt;RP_IPSecTransformSet&gt; |Ditt valda namn för det här objektet. Exempel: myIPSecTransformSet |
| &lt;RP_IPSecCryptoMap&gt; |Ditt valda namn för det här objektet. Exempel: myIPSecCryptoMap |
| &lt;SP_AzureNetworkIpRange&gt; |Ange intervallet. Exempel: 192.168.0.0 |
| &lt;SP_AzureNetworkSubnetMask&gt; |Ange nätmasken. Exempel: 255.255.0.0 |
| &lt;SP_OnPremisesNetworkIpRange&gt; |Ange det lokala intervallet. Exempel: 10.2.1.0 |
| &lt;SP_OnPremisesNetworkSubnetMask&gt; |Ange den lokala nätmasken. Exempel: 255.255.255.0 |
| &lt;SP_AzureGatewayIpAddress&gt; |Det här virtuella nätverket information specifik tooyour och finns i hello hanteringsportalen som **Gateway IP-adressen**. |
| &lt;SP_PresharedKey&gt; |Den här informationen är specifik tooyour virtuella nätverk och finns i hello hanteringsportalen som hanterar nyckel. |

## <a name="ipsec"></a>IPsec-/IKE-parametrar

> [!NOTE]
> Även om hello värden som anges i följande tabell hello stöds av hello VPN-gateway för närvarande det finns ingen mekanism för toospecify eller välj en specifik kombination av algoritmer eller parametrar hello VPN-gateway. Du måste ange eventuella villkor från hello lokala VPN-enhet. Du måste dessutom foga ihop **MSS** vid **1350**.
> 
>

I följande tabeller hello:

* SA = Security Association
* IKE fas 1 kallas även "Huvudläge"
* IKE fas 2 kallas även "Snabbläge"

### <a name="ike-phase-1-main-mode-parameters"></a>Parametrar för IKE fas 1 (huvudläge)

| **Egenskap**          |**Principbaserad**    | **Routningsbaserad**    |
| ---                   | ---               | ---               |
| IKE-version           |IKEv1              |IKEv2              |
| Diffie-Hellman Group  |Grupp 2 (1 024 bitar) |Grupp 2 (1 024 bitar) |
| Autentiseringsmetod |I förväg delad nyckel     |I förväg delad nyckel     |
| Krypterings- och hash-algoritmer |1. AES256, SHA256<br>2. AES256, SHA1<br>3. AES128, SHA1<br>4. 3DES, SHA1 |1. AES256, SHA1<br>2. AES256, SHA256<br>3. AES128, SHA1<br>4. AES128, SHA256<br>5. 3DES, SHA1<br>6. 3DES, SHA256 |
| SA-livstid           |28 800 sekunder     |28 800 sekunder     |

### <a name="ike-phase-2-quick-mode-parameters"></a>Parametrar för IKE fas 2 (snabbläge)

| **Egenskap**                  |**Principbaserad**| **Routningsbaserad**                              |
| ---                           | ---           | ---                                         |
| IKE-version                   |IKEv1          |IKEv2                                        |
| Krypterings- och hash-algoritmer |1. AES256, SHA256<br>2. AES256, SHA1<br>3. AES128, SHA1<br>4. 3DES, SHA1 |[RouteBased QM SA-erbjudanden](#RouteBasedOffers) |
| SA-livstid (tid)            |3 600 sekunder  |27 000 sekunder                                |
| SA-livstid (byte)           |102 400 000 kB | -                                           |
| PFS (Perfect Forward Secrecy) |Nej             |[RouteBased QM SA-erbjudanden](#RouteBasedOffers) |
| Utebliven peer-identifiering (DPD)     |Stöds inte  |Stöds                                    |


### <a name ="RouteBasedOffers"></a>Erbjudanden för RouteBased VPN IPsec-säkerhetsassociation (IKE-snabbläge SA)

hello visas följande tabell IPsec SA (snabbläge för IKE)-erbjudanden. Erbjudanden är listade hello ordning preferensordning presenteras som hello erbjudande eller godkänns.

#### <a name="azure-gateway-as-initiator"></a>Azure Gateway som initierare

|-  |**Kryptering**|**Autentisering**|**PFS-grupp**|
|---| ---          |---               |---          |
| 1 |GCM AES256    |GCM (AES256)      |Ingen         |
| 2 |AES256        |SHA1              |Ingen         |
| 3 |3DES          |SHA1              |Ingen         |
| 4 |AES256        |SHA256            |Ingen         |
| 5 |AES128        |SHA1              |Ingen         |
| 6 |3DES          |SHA256            |Ingen         |

#### <a name="azure-gateway-as-responder"></a>Azure Gateway som svarare

|-  |**Kryptering**|**Autentisering**|**PFS-grupp**|
|---| ---          | ---              |---          |
| 1 |GCM AES256    |GCM (AES256)      |Ingen         |
| 2 |AES256        |SHA1              |Ingen         |
| 3 |3DES          |SHA1              |Ingen         |
| 4 |AES256        |SHA256            |Ingen         |
| 5 |AES128        |SHA1              |Ingen         |
| 6 |3DES          |SHA256            |Ingen         |
| 7 |DES           |SHA1              |Ingen         |
| 8 |AES256        |SHA1              |1            |
| 9 |AES256        |SHA1              |2            |
| 10|AES256        |SHA1              |14           |
| 11|AES128        |SHA1              |1            |
| 12|AES128        |SHA1              |2            |
| 13|AES128        |SHA1              |14           |
| 14|3DES          |SHA1              |1            |
| 15|3DES          |SHA1              |2            |
| 16|3DES          |SHA256            |2            |
| 17|AES256        |SHA256            |1            |
| 18|AES256        |SHA256            |2            |
| 19|AES256        |SHA256            |14           |
| 20|AES256        |SHA1              |24           |
| 21|AES256        |SHA256            |24           |
| 22|AES128        |SHA256            |Ingen         |
| 23|AES128        |SHA256            |1            |
| 24|AES128        |SHA256            |2            |
| 25|AES128        |SHA256            |14           |
| 26|3DES          |SHA1              |14           |

* Du kan ange IPsec ESP NULL-kryptering med routningsbaserade VPN-gatewayer och VPN-gatewayer som har hög kapacitet. Null baserat kryptering inte ge skydd toodata under överföring och bör endast användas när maximalt dataflöde och lägsta svarstid måste anges. Klienter kan välja toouse detta i VNet-till-VNet-kommunikationsscenarion eller kryptering som används på andra ställen i hello-lösning.
* Använda hello Azure VPN gateway-standardinställningar för anslutning via hello Internet, med kryptering och hash-algoritmer som anges i hello tabeller ovan tooensure säkerheten för din kritisk kommunikation.

## <a name="known"></a>Kända enhetskompatibilitetsproblem

> [!IMPORTANT]
> Dessa hello kända kompatibilitetsproblem mellan tredje parts VPN-enheter och Azure VPN-gatewayer. hello Azure-teamet arbetar aktivt med hello leverantörer tooaddress hello problem som anges här. När hello problem har åtgärdats, kommer den här sidan uppdateras med hello allra senaste informationen. Kom tillbaka regelbundet.
>
>

### <a name="feb-16-2017"></a>16 februari 2017

**Palo Alto nätverk enheter med version tidigare too7.1.4** för Azure ruttbaserade VPN: Om du använder VPN-enheter från Palo Alto nätverk med PANORERING OS-version tidigare too7.1.4 och har anslutningen utfärdar tooAzure ruttbaserad VPN-gatewayer Utför följande steg hello:

1. Kontrollera hello version på inbyggd programvara för enheten Palo Alto-nätverk. Om din PANORERA OS-version är äldre än 7.1.4, uppgradera too7.1.4.
2. På hello Palo Alto nätverk enhet kan ändra hello fas 2 SA (eller Snabbläge) livstid too28, 800 sekunder (8 timmar) när ansluter toohello Azure VPN-gateway.
3. Om du fortfarande har problem med nätverksanslutningen, öppna en supportbegäran från hello Azure-portalen.
