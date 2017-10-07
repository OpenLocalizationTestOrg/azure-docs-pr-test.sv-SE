---
title: "aaaTroubleshoot Azure plats-till-plats-VPN kopplar från periodvis | Microsoft Docs"
description: "Lär dig hur tootroubleshoot hello problem i vilka hello plats-till-plats VPN-anslutningen kopplas från regelbundet."
services: vpn-gateway
documentationcenter: na
author: chadmath
manager: cshepard
editor: 
tags: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/21/2017
ms.author: genli
ms.openlocfilehash: 1ce3c4ff9d8f650312e45f33b760ebcc6597fc13
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-azure-site-to-site-vpn-disconnects-intermittently"></a>Felsöka: Azure plats-till-plats-VPN kopplar från periodvis

Du kan uppleva hello problem att en ny eller befintlig Microsoft Azure punkt-till-plats VPN-anslutning inte är stabilt eller kopplar från regelbundet. Den här artikeln innehåller felsöka steg toohelp du identifiera och lösa hello orsaken hello problemet. 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="troubleshooting-steps"></a>Felsökningssteg

### <a name="prerequisite-step"></a>Innan du börjar:

Kontrollera hello typ av Azure virtuella nätverkets gateway:

1. Gå för[Azure-portalen](https://portal.azure.com).
2. Kontrollera hello **översikt** sidan hello virtuell nätverksgateway hello typen information.
    
    ![hello översikt över hello gateway](media\vpn-gateway-troubleshoot-site-to-site-disconnected-intermittently\gatewayoverview.png)

### <a name="step-1-check-whether-hello-on-premises-vpn-device-is-validated"></a>Steg 1 Kontrollera om hello lokala VPN-enhet har verifierats

1. Kontrollera om du använder en [verifiera VPN-enhet och operativsystemversion](vpn-gateway-about-vpn-devices.md#devicetable). Om hello VPN-enhet inte har verifierats, kanske toocontact hello enhetens tillverkare toosee om det inte finns några kompatibilitetsproblem.
2. Kontrollera att hello VPN-enheten är korrekt konfigurerad. Mer information finns i [redigera enheten configuration exempel](vpn-gateway-about-vpn-devices.md#editing).

### <a name="step-2-check-hello-security-association-settingsfor-policy-based-azure-virtual-network-gateways"></a>Steg 2 Kontrollera hello säkerhetsassociation inställningar (för gruppolicy-baserad Azure virtuella nätverks-gateway)

1. Se till att hello virtuellt nätverk, undernät och intervall hello **lokal nätverksgateway** definition i Microsoft Azure är samma som hello konfiguration på hello lokala VPN-enhet.
2. Kontrollera som överensstämmer med hello säkerhetsassociation inställningar.

### <a name="step-3-check-for-user-defined-routes-or-network-security-groups-on-gateway-subnet"></a>Steg 3-kontroll för användardefinierade vägar eller Nätverkssäkerhetsgrupper på Gateway-undernät

En användardefinierad väg på hello gateway-undernätet kan begränsa viss trafik och tillåta andra trafik. Detta gör det visas att hello VPN-anslutningen är instabilt för viss trafik och bra för andra. 

### <a name="step-4-check-hello-one-vpn-tunnel-per-subnet-pair-setting-for-policy-based-virtual-network-gateways"></a>Steg 4 Kontrollera Hej ”en VPN-Tunnel undernät par” (för principbaserad virtuella nätverks-gateway)

Kontrollera att hello lokala VPN-enheten anges toohave **en VPN-tunnel undernät par** för principbaserad virtuella nätverksgatewayer.

### <a name="step-5-check-for-security-association-limitation-for-policy-based-virtual-network-gateways"></a>Steg 5 Kontrollera säkerhet Association begränsningen (för principbaserad virtuella nätverksgatewayer)

hello Policy-baserad virtuell nätverksgateway har högst 200 undernät säkerhetsassociation par. Om hello antalet undernät som virtuella Azure-nätverket multiplicerat gånger av hello antalet lokala undernät är större än 200 ser du sporadiska undernät som kopplar från.

### <a name="step-6-check-on-premises-vpn-device-external-interface-address"></a>Steg 6 Kontrollera lokalt externa gränssnittet för VPN-enhetsadress

- Om hello mot IP-adressen för VPN-enhet för hello Internet ingår i hello **lokal nätverksgateway** definition i Azure, sporadiska frånkopplingar kan uppstå.
- hello måste externa enhetsgränssnittet vara direkt på hello Internet. Det bör finnas några NAT (Network Address Translation) eller brandvägg mellan hello Internet och hello enhet.
-  Om du konfigurerar brandväggen Clustering toohave en virtuell IP-adress, måste du bryta hello kluster och exponera hello VPN-enhet direkt tooa offentliga gränssnittet som hello gateway kan samverka med.

### <a name="step-7-check-whether-hello-on-premises-vpn-device-has-perfect-forward-secrecy-enabled"></a>Steg 7 Kontrollera om hello lokala VPN-enhet har Perfect Forward Secrecy aktiverad

Hej **Perfect Forward Secrecy** funktion kan orsaka hello frånkoppling problem. Om hello VPN-enhet har **perfekt forward Secrecy** aktiverat inaktivera hello-funktionen. Sedan [uppdatera hello virtuellt gateway IPsec-princip](vpn-gateway-ipsecikepolicy-rm-powershell.md#managepolicy).

## <a name="next-steps"></a>Nästa steg

- [Konfigurera ett virtuellt nätverk för plats-till-plats-anslutning tooa](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Konfigurera IPsec/IKE-princip för plats-till-plats VPN-anslutningar](vpn-gateway-ipsecikepolicy-rm-powershell.md)

