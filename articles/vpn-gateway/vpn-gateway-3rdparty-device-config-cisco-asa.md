---
title: "Exempel på konfiguration - Cisco ASA-enhet som ansluter till Azure VPN-gatewayer | Microsoft Docs"
description: "Den här artikeln innehåller ett exempel på konfiguration för Cisco ASA-enhet som ansluter till Azure VPN-gatewayer."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: 
ms.assetid: a8bfc955-de49-4172-95ac-5257e262d7ea
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/20/2017
ms.author: yushwang
ms.openlocfilehash: 10466b8928e2cd687f7961a2956b6d60823b82be
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="sample-configuration-cisco-asa-device-ikev2no-bgp"></a>Exempel på konfiguration: Cisco ASA enhet (IKEv2/Nej BGP)
Den här artikeln innehåller exempel konfigurationer för Cisco ASA-enheter som ansluter till Azure VPN-gatewayer.

## <a name="device-at-a-glance"></a>Enheten i korthet

|                        |                                   |
| ---                    | ---                               |
| Leverantören av enheten          | Cisco                             |
| Enhetsmodell           | ASA (anpassningsbar postsäkerhet) |
| Målversionen         | 8.4+                              |
| Testad modellen           | ASA 5505                          |
| Testad version         | 9.2                               |
| IKE-version            | **IKEv2**                         |
| BGP                    | **Nej**                            |
| Azure VPN gateway-typ | **Ruttbaserad** VPN-gateway       |
|                        |                                   |

> [!NOTE]
> 1. Konfigurationen nedan ansluter en Cisco ASA-enhet till en Azure **ruttbaserad** VPN-gateway med hjälp av anpassade IPsec/IKE-princip med alternativet ”UserPolicyBasedTrafficSelectors”, enligt beskrivningen i [i den här artikeln](vpn-gateway-connect-multiple-policybased-rm-ps.md).
> 2. Det krävs ASA enheter ska kunna använda **IKEv2** med åtkomst-list-baserade konfigurationer inte VTI-baserade.
> 3. Kontakta din VPN-leverantör enhetsspecifikationerna så principen stöds på din lokala VPN-enheter.

## <a name="vpn-device-requirements"></a>Krav för VPN-enhet
Azure VPN-gatewayer använder standard IPsec/IKE-protokoll-paket för att upprätta S2S VPN-tunnlar. Referera till [om VPN-enheter](vpn-gateway-about-vpn-devices.md) för detaljerad IPsec/IKE-protokollparametrar och standard kryptografiska algoritmer för Azure VPN-gatewayer. Du kan också ange den exakta kombinationen av kryptografiska algoritmer och viktiga fördelar för en viss anslutning enligt beskrivningen i [om krav för kryptografiska](vpn-gateway-about-compliance-crypto.md). Om du väljer en specifik kombination av kryptografiska algoritmer och viktiga styrkor, kontrollera att du använder de motsvarande specifikationerna på VPN-enheter.

## <a name="single-vpn-tunnel"></a>En VPN-tunnel
Den här topologin består av en S2S VPN-tunnel mellan en Azure VPN-gateway och din lokala VPN-enhet. Du kan också konfigurera BGP över VPN-tunnel.

![en tunnel](./media/vpn-gateway-3rdparty-device-config-cisco-asa/singletunnel.png)

Referera till [en tunnel installationsprogrammet](vpn-gateway-3rdparty-device-config-overview.md#singletunnel) för detaljerad, stegvisa anvisningar att skapa Azure-konfigurationer.

### <a name="network-and-vpn-gateway-information"></a>Information om nätverk och VPN-gateway
Det här avsnittet listas parametrarna för det här exemplet.

| **Parametern**                | **Värde**                    |
| ---                          | ---                          |
| VNet-adressprefixer        | 10.11.0.0/16<br>10.12.0.0/16 |
| Azure VPN gateway-IP         | Azure_Gateway_Public_IP      |
| Lokala adressprefix | 10.51.0.0/16<br>10.52.0.0/16 |
| Lokala VPN-enhetens IP    | OnPrem_Device_Public_IP     |
| * VNet BGP ASN                | 65010                        |
| * Azure BGP-peer-IP           | 10.12.255.30                 |
| * Lokala BGP ASN         | 65050                        |
| * Lokala BGP-peer-IP     | 10.52.255.254                |
|                              |                              |

* (*) Valfria parametrar för BGP endast.

### <a name="ipsecike-policy--parameters"></a>IPSec-/ princip & parametrar

I tabellen nedan visas de IPsec/IKE-algoritmer och parametrar som används i exemplet. Kontakta din VPN-enhetsspecifikationer se till alla algoritmer som anges ovan stöds av din VPN-enhetsmodeller och inbyggd programvara.

| **IPsec/IKEv2**  | **Värde**                            |
| ---              | ---                                  |
| IKEv2-kryptering | AES256                               |
| IKEv2 Integrity  | SHA384                               |
| DH-grupp         | DHGroup24                            |
| IPsec-kryptering | AES256                               |
| IPsec Integrity  | SHA1                                 |
| PFS-grupp        | PFS24                                |
| QM SA-livstid   | 7200 sekunder                         |
| Trafikväljare | UsePolicyBasedTrafficSelectors $True |
| I förväg delad nyckel   | PreSharedKey                         |
|                  |                                      |

- (*) IPSec-integritet måste vara ”null” om GCM-AES används som IPSec-krypteringsalgoritmen på vissa enheter.

### <a name="device-notes"></a>Anteckningar för enhet

>[!NOTE]
>
> 1. Stödet för IKEv2 kräver ASA version 8.4 och högre.
> 2. Stöd för högre DH och PFS (utöver gruppen 5) kräver ASA version 9.x.
> 3. IPSec-kryptering med AES-GCM och IPsec integritet med SHA-256, SHA-384, stöd för SHA-512 kräver ASA version 9.x på nyare ASA maskinvara; 5505 ASA, 5510, 5520, 5540, 5550, 5580 är **inte** stöds. (Kontrollera Leverantörsspecifikationer för att bekräfta.)
>


### <a name="sample-device-configurations"></a>Exempel enhetskonfigurationer
Skriptet nedan innehåller en exempelkonfiguration baserat på topologin och parametrarna som anges ovan. S2S VPN-tunnel konfigurationen består av följande delar:

1. Gränssnitt & vägar
2. Listor
3. IKE-principen och parametrar (fas 1 eller Main-läge)
4. IPSec-principen och parametrar (fas 2 eller snabbläge)
5. Andra parametrar (TCP-MSS minskning, etc.)

>[!IMPORTANT] 
>Kontrollera att du slutföra ytterligare konfigurationen nedan och ersätta platshållarna med faktiska värden:
> 
> - Gränssnittskonfiguration för både inom och utanför gränssnitt
> - Vägar för din innanför/privat och utanför och offentliga nätverk
> - Se till att alla namn och nummer är unika på enheten
> - Se till att de kryptografiska algoritmerna stöds på enheten
> - Ersätt följande plats innehavare med de faktiska värdena
>   - Utanför gränssnittsnamn: ”externa”
>   - Azure_Gateway_Public_IP
>   - OnPrem_Device_Public_IP
>   - IKE-Pre_Shared_Key
>   - Virtuella nätverk och lokala gateway nätverksnamn (VNetName, LNGName)
>   - Virtuella nätverk och lokala nätverks-adressprefix
>   - Rätt nätmasker

#### <a name="sample-configuration"></a>Exempel på konfiguration

```
! Sample ASA configuration for connecting to Azure VPN gateway
!
! Tested hardware: ASA 5505
! Tested version:  ASA version 9.2(4)
!
! Replace the following place holders with your actual values:
!   - Interface names - default are "outside" and "inside"
!   - <Azure_Gateway_Public_IP>
!   - <OnPrem_Device_Public_IP>
!   - <Pre_Shared_Key>
!   - <VNetName>*
!   - <LNGName>* ==> LocalNetworkGateway - the Azure resource that represents the
!     on-premises network, specifies network prefixes, device public IP, BGP info, etc.
!   - <PrivateIPAddress> ==> Replace it with a private IP address if applicable
!   - <Netmask> ==> Replace it with appropriate netmasks
!   - <Nexthop> ==> Replace it with the actual nexthop IP address
!
! (*) Must be unique names in the device configuration
!
! ==> Interface & route configurations
!
!     > <OnPrem_Device_Public_IP> address on the outside interface or vlan
!     > <PrivateIPAddress> on the inside interface or vlan; e.g., 10.51.0.1/24
!     > Route to connect to <Azure_Gateway_Public_IP> address
!
!     > Example:
!
!       interface Ethernet0/0
!        switchport access vlan 2
!       exit
!
!       interface vlan 1
!        nameif inside
!        security-level 100
!        ip address <PrivateIPAddress> <Netmask>
!       exit
!
!       interface vlan 2
!        nameif outside
!        security-level 0
!        ip address <OnPrem_Device_Public_IP> <Netmask>
!       exit
!
!       route outside 0.0.0.0 0.0.0.0 <NextHop IP> 1
!
! ==> Access lists
!
!     > Most firewall devices deny all traffic by default. Create access lists to
!       (1) Allow S2S VPN tunnels between the ASA and the Azure gateway public IP address
!       (2) Construct traffic selectors as part of IPsec policy or proposal
!
access-list outside_access_in extended permit ip host <Azure_Gateway_Public_IP> host <OnPrem_Device_Public_IP>
!
!     > Object group that consists of all VNet prefixes (e.g., 10.11.0.0/16 &
!       10.12.0.0/16)
!
object-group network Azure-<VNetName>
 description Azure virtual network <VNetName> prefixes
 network-object 10.11.0.0 255.255.0.0
 network-object 10.12.0.0 255.255.0.0
exit
!
!     > Object group that corresponding to the <LNGName> prefixes.
!       E.g., 10.51.0.0/16 and 10.52.0.0/16. Note that LNG = "local network gateway".
!       In Azure network resource, a local network gateway defines the on-premises
!       network properties (address prefixes, VPN device IP, BGP ASN, etc.)
!
object-group network <LNGName>
 description On-Premises network <LNGName> prefixes
 network-object 10.51.0.0 255.255.0.0
 network-object 10.52.0.0 255.255.0.0
exit
!
!     > Specify the access-list between the Azure VNet and your on-premises network.
!       This access list defines the IPsec SA traffic selectors.
!
access-list Azure-<VNetName>-acl extended permit ip object-group <LNGName> object-group Azure-<VNetName>
!
!     > No NAT required between the on-premises network and Azure VNet
!
nat (inside,outside) source static <LNGName> <LNGName> destination static Azure-<VNetName> Azure-<VNetName>
!
! ==> IKEv2 configuration
!
!     > General IKEv2 configuration - enable IKEv2 for VPN
!
group-policy DfltGrpPolicy attributes
 vpn-tunnel-protocol ikev1 ikev2
exit
!
crypto isakmp identity address
crypto ikev2 enable outside
!
!     > Define IKEv2 Phase 1/Main Mode policy
!       - Make sure the policy number is not used
!       - integrity and prf must be the same
!       - DH group 14 and above require ASA version 9.x.
!
crypto ikev2 policy 1
 encryption       aes-256
 integrity        sha384
 prf              sha384
 group            24
 lifetime seconds 86400
exit
!
!     > Set connection type and pre-shared key
!
tunnel-group <Azure_Gateway_Public_IP> type ipsec-l2l
tunnel-group <Azure_Gateway_Public_IP> ipsec-attributes
 ikev2 remote-authentication pre-shared-key <Pre_Shared_Key> 
 ikev2 local-authentication  pre-shared-key <Pre_Shared_Key> 
exit
!
! ==> IPsec configuration
!
!     > IKEv2 Phase 2/Quick Mode proposal
!       - AES-GCM and SHA-2 requires ASA version 9.x on newer ASA models. ASA
!         5505, 5510, 5520, 5540, 5550, 5580 are not supported.
!       - ESP integrity must be null if AES-GCM is configured as ESP encryption
!
crypto ipsec ikev2 ipsec-proposal AES-256
 protocol esp encryption aes-256
 protocol esp integrity  sha-1
exit
!
!     > Set access list & traffic selectors, PFS, IPsec protposal, SA lifetime
!       - This sample uses "Azure-<VNetName>-map" as the crypto map name
!       - ASA supports only one crypto map per interface, if you already have
!         an existing crypto map assigned to your outside interface, you must use
!         the same crypto map name, but with a different sequence number for
!         this policy
!       - "match address" policy uses the access-list "Azure-<VNetName>-acl" defined 
!         previously
!       - "ipsec-proposal" uses the proposal "AES-256" defined previously 
!       - PFS groups 14 and beyond requires ASA version 9.x.
!
crypto map Azure-<VNetName>-map 1 match address Azure-<VNetName>-acl
crypto map Azure-<VNetName>-map 1 set pfs group24
crypto map Azure-<VNetName>-map 1 set peer <Azure_Gateway_Public_IP>
crypto map Azure-<VNetName>-map 1 set ikev2 ipsec-proposal AES-256
crypto map Azure-<VNetName>-map 1 set security-association lifetime seconds 7200
crypto map Azure-<VNetName>-map interface outside
!
! ==> Set TCP MSS to 1350
!
sysopt connection tcpmss 1350
!
```

## <a name="simple-debugging-commands"></a>Enkla kommandon för felsökning

Här följer några ASA-kommandon för felsökning:

1. Visa IPsec- och IKE SA
    - ”Visa kryptografi ipsec sa”
    - ”Visa crypto ikev2 sa”
2. Anger felsökningsläge - detta kan få väldigt mycket brus på konsolen
    - ”debug kryptografi ikev2 plattform <level>”
    - ”Felsöka kryptografi ikev2-protokoll <level>”
3. Lista över aktuella konfigurationer
    - ”Visa kör” - visar aktuella konfigurationen på enhet; Du kan använda olika underordnade kommandon för att lista vissa delar av konfigurationen. Till exempel ”visa kör crypto”, ”visa kör åtkomstlista”, ”visa kör tunnel-grupp” osv.


## <a name="next-steps"></a>Nästa steg
Anvisningar om hur du konfigurerar aktiv-aktiv VPN-gateways för flera platser och VNet-till-VNet-anslutningar finns i [Konfigurera aktiv-aktiv VPN-gateways för flera platser och VNet-till-VNet-anslutningar](vpn-gateway-activeactive-rm-powershell.md).

