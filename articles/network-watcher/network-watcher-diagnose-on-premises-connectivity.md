---
title: "aaaDiagnose lokal anslutning via VPN-gateway med Azure Nätverksbevakaren | Microsoft Docs"
description: "Den här artikeln beskriver hur toodiagnose lokal anslutning via VPN-gateway med Azure Nätverksbevakaren resurs felsökning."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: aeffbf3d-fd19-4d61-831d-a7114f7534f9
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 9941c5d1b49bec29062210684dae8653cbdb84b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="diagnose-on-premises-connectivity-via-vpn-gateways"></a>Diagnostisera lokal anslutning via VPN-gatewayer

Azure VPN-Gateway kan du toocreate hybridlösning som löser hello behovet av att en säker anslutning mellan ditt lokala nätverk och dina virtuella Azure-nätverket. Dina behov är unika, så är hello valet av lokala VPN-enhet. Azure stöder för närvarande [flera VPN-enheter](../vpn-gateway/vpn-gateway-about-vpn-devices.md#devicetable) som ständigt verifieras tillsammans med hello enhetsleverantörer. Granska konfigurationsinställningarna för hello enhetsspecifika innan du konfigurerar din lokala VPN-enhet. På liknande sätt Azure VPN-Gateway har konfigurerats med en uppsättning [IPSec-parametrar som stöds](../vpn-gateway/vpn-gateway-about-vpn-devices.md#ipsec) som används för att upprätta anslutningar. Det finns för närvarande inget sätt du toospecify eller välj en specifik kombination av IPSec-parametrar från hello Azure VPN-Gateway. För att upprätta en anslutning mellan lokala och Azure hello lokala inställningar för VPN-enheter måste vara i enlighet med hello IPSec-parametrar som föreskrivs av Azure VPN-Gateway. Om hello inställningarna är korrekt, har en förlust av anslutningen och fram till nu felsökning av problem med dessa var inte trivial och vanligtvis tog timmar tooidentify och fixa hello problemet.

Felsökning av funktionen med hello Azure Nätverksbevakaren, du kan toodiagnose eventuella problem med din Gateway och anslutningar och har tillräckligt med information toomake ett välgrundat beslut toorectify hello problem inom minuter.

## <a name="scenario"></a>Scenario

Du vill tooconfigure plats-till-plats-anslutning mellan Azure och lokala med FortiGate som hello lokala VPN-Gateway. tooachieve i det här scenariot kräver hello efter installationen:

1. Virtuell nätverksgateway - hello VPN-Gateway på Azure
1. Lokal nätverksgateway - hello [lokalt (FortiGate) VPN-Gateway](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md#LocalNetworkGateway) representation i Azure-molnet
1. Plats-till-plats-anslutning (principbaserad) - [anslutningen mellan hello VPN-Gateway och hello lokala router](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md#createconnection)
1. [Konfigurera FortiGate](https://github.com/Azure/Azure-vpn-config-samples/blob/master/Fortinet/Current/Site-to-Site_VPN_using_FortiGate.md)

Detaljerade steg-för-steg-guiden för att konfigurera en plats-till-plats-konfiguration kan hittas genom att besöka: [skapa ett VNet med en plats-till-plats-anslutning med hello Azure-portalen](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md).

En av hello kritiska konfigurationssteg konfigurerar hello IPSec-kommunikation parametrar, alla felkonfiguration leder tooloss för anslutningen mellan hello lokala nätverk och Azure. Azure VPN-gatewayer är för närvarande konfigurerade toosupport hello följande IPSec-parametrar för fas 1. Observera att som tidigare nämnts inte kan ändra de här inställningarna.  Som du ser i hello nedan är hello krypteringsalgoritmer som stöds av Azure VPN-Gateway AES256, AES128 och 3DES.

### <a name="ike-phase-1-setup"></a>Installationsprogrammet för IKE fas 1

| **Egenskap** | **Principbaserad** | **RouteBased och Standard eller högpresterande VPN-gateway** |
| --- | --- | --- |
| IKE-version |IKEv1 |IKEv2 |
| Diffie-Hellman Group |Grupp 2 (1 024 bitar) |Grupp 2 (1 024 bitar) |
| Autentiseringsmetod |I förväg delad nyckel |I förväg delad nyckel |
| Krypteringsalgoritmer |AES256 AES128 3DES |AES256 3DES |
| Hash-algoritm |SHA1(SHA128) |SHA1(SHA128), SHA2(SHA256) |
| Fas 1, Security Association (SA), livslängd (tid) |28 800 sekunder |10 800 sekunder |

Som en användare blir nödvändiga tooconfigure din FortiGate en exempelkonfiguration finns på [GitHub](https://github.com/Azure/Azure-vpn-config-samples/blob/master/Fortinet/Current/fortigate_show%20full-configuration.txt). Utan vetskap du har konfigurerat din FortiGate toouse SHA-512 som hello hash-algoritm. Den här algoritmen inte är en algoritm som stöds för principbaserad anslutningar fungerar din VPN-anslutning.

Dessa problem är hårda tootroubleshoot och rotorsaker är ofta tveksamma. I det här fallet kan du öppna en stöd biljett tooget hjälp om hur du löser problemet hello. Men med Azure Nätverksbevakaren felsöka API, kan du identifiera problemen på egen hand.

## <a name="troubleshooting-using-azure-network-watcher"></a>Felsökning med hjälp av Azure Nätverksbevakaren

toodiagnose din anslutning och ansluta tooAzure PowerShell och initiera hello `Start-AzureRmNetworkWatcherResourceTroubleshooting` cmdlet. Du kan hitta hello information om hur du använder denna cmdlet på [felsöka virtuella nätverksgateway och anslutningar - PowerShell](network-watcher-troubleshoot-manage-powershell.md). Denna cmdlet kan ta toofew minuter toocomplete.

När hello cmdleten har slutförts, du kan navigera toohello lagringsplats som angetts i hello cmdlet tooget detaljerad information om hello problemet och loggar på. Azure Nätverksbevakaren skapar en komprimerad mapp som innehåller hello följande loggfiler:

![1][1]

Öppna hello-fil som heter IKEErrors.txt och visar hello följande fel, som indikerar ett problem med lokala IKE inställningen felaktig konfiguration.

```
Error: On-premises device rejected Quick Mode settings. Check values.
     based on log : Peer sent NO_PROPOSAL_CHOSEN notify
```

Du kan få detaljerad information från hello Scrubbed wfpdiag.txt om hello fel, som i det här fallet nämns att `ERROR_IPSEC_IKE_POLICY_MATCH` som lead tooconnection inte fungerar korrekt.

En annan gemensam felkonfigurering är hello anger felaktigt delade nycklar. I föregående exempel som du har angett olika delade nycklar hello, visar hello IKEErrors.txt hello följande fel: `Error: Authentication failed. Check shared key`.

Azure Nätverksbevakaren Felsöka funktionen kan du toodiagnose och felsöka din VPN-Gateway och en anslutning med en enkel PowerShell-cmdlet hello enkel. För närvarande vi stöder diagnostisera hello följande villkor och arbetar för att lägga till flera villkor.

### <a name="gateway"></a>Gateway

| Feltypen | Orsak | Logga|
|---|---|---|
| NoFault | Om inga fel har identifierats. |Ja|
| GatewayNotFound | Det går inte att hitta Gateway eller Gateway inte har etablerats. |Nej|
| PlannedMaintenance |  Gateway-instans är under underhåll.  |Nej|
| UserDrivenUpdate | När en användare uppdatering pågår. Detta kan vara en åtgärd för storleksändring. | Nej |
| VipUnResponsive | Det går inte att nå hello primära instansen av hello Gateway. Detta händer när hello hälsoavsökningen misslyckas. | Nej |
| PlatformInActive | Det finns ett problem med hello-plattformen. | Nej|
| ServiceNotRunning | hello underliggande tjänst körs inte. | Nej|
| NoConnectionsFoundForGateway | Det finns inga anslutningar på hello gateway. Detta är endast en varning.| Nej|
| ConnectionsNotConnected | Ingen av hello anslutningar är anslutna. Detta är endast en varning.| Ja|
| GatewayCPUUsageExceeded | hello aktuell Gateway användning CPU-användning är > 95%. | Ja |

### <a name="connection"></a>Anslutning

| Feltypen | Orsak | Logga|
|---|---|---|
| NoFault | Om inga fel har identifierats. |Ja|
| GatewayNotFound | Det går inte att hitta Gateway eller Gateway inte har etablerats. |Nej|
| PlannedMaintenance | Gateway-instans är under underhåll.  |Nej|
| UserDrivenUpdate | När en användare uppdatering pågår. Detta kan vara en åtgärd för storleksändring.  | Nej |
| VipUnResponsive | Det går inte att nå hello primära instansen av hello Gateway. Det händer när hello hälsoavsökningen misslyckas. | Nej |
| ConnectionEntityNotFound | Konfigurationen för anslutningen saknas. | Nej |
| ConnectionIsMarkedDisconnected | hello anslutning är markerad med ”frånkopplad”. |Nej|
| ConnectionNotConfiguredOnGateway | hello underliggande tjänsten har inte hello anslutning konfigureras. | Ja |
| ConnectionMarkedStandy | hello underliggande tjänsten har markerats som vänteläge.| Ja|
| Autentisering | I förväg delad nyckel matchar inte. | Ja|
| PeerReachability | hello peer-gateway kan inte nås. | Ja|
| IkePolicyMismatch | hello peer-gateway har IKE-principer som inte stöds av Azure. | Ja|
| WfpParse fel | Ett fel uppstod tolkning hello WFP-loggen. |Ja|

## <a name="next-steps"></a>Nästa steg

Läs toocheck VPN Gateway-anslutningar med PowerShell och Azure Automation genom att besöka [övervakaren VPN-gatewayer Azure Nätverksbevakaren felsökning](network-watcher-monitor-with-azure-automation.md)

[1]: ./media/network-watcher-diagnose-on-premises-connectivity/figure1.png
