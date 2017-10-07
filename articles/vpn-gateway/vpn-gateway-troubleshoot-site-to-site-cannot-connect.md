---
title: "aaaTroubleshoot en Azure VPN-anslutning för plats-till-plats som inte kan ansluta | Microsoft Docs"
description: "Lär dig hur tootroubleshoot en plats-till-plats VPN-anslutning som plötsligt slutar fungera och kan inte återanslutas."
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
ms.openlocfilehash: 632c75bfcfb93a532eeead2855d43e6614569a99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-an-azure-site-to-site-vpn-connection-cannot-connect-and-stops-working"></a>Felsökning: Azure plats-till-plats VPN-anslutningen kan inte ansluta och slutar fungera

När du har konfigurerat en plats-till-plats VPN-anslutning mellan ett lokalt nätverk och Azure-nätverk slutar fungera plötsligt hello VPN-anslutning och kan inte återanslutas. Den här artikeln innehåller felsökning steg toohelp du lösa problemet. 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="troubleshooting-steps"></a>Felsökningssteg

tooresolve hello problemet först försöka för[Återställ hello Azure VPN-gateway](vpn-gateway-resetgw-classic.md) och återställa hello tunnel från hello lokala VPN-enhet. Följ dessa steg tooidentify hello orsaken hello problemet om hello problemet kvarstår.

### <a name="prerequisite-step"></a>Innan du börjar:

Kontrollera hello typ av hello Azure VPN-gateway.

1. Gå toohello [Azure-portalen](https://portal.azure.com).

2. Kontrollera hello **översikt** sidan hello VPN-gateway för hello typinformation.
    
    ![Översikt över hello gateway](media\vpn-gateway-troubleshoot-site-to-site-cannot-connect\gatewayoverview.png)

### <a name="step-1-check-whether-hello-on-premises-vpn-device-is-validated"></a>Steg 1. Kontrollera om hello lokala VPN-enhet har verifierats

1. Kontrollera om du använder en [verifiera VPN-enhet och operativsystemversion](vpn-gateway-about-vpn-devices.md#devicetable). Om hello enheten inte är en verifierad VPN-enhet, kanske toocontact hello enhetens tillverkare toosee om det finns ett kompatibilitetsproblem.

2. Kontrollera att hello VPN-enheten är korrekt konfigurerad. Mer information finns i [redigera enheten configuration exempel](/vpn-gateway-about-vpn-devices.md#editing).

### <a name="step-2-verify-hello-shared-key"></a>Steg 2. Kontrollera hello delad nyckel

Jämför hello delad nyckel för hello lokala VPN-enhet toohello VPN för Azure Virtual Network toomake att hello nycklar matchar. 

tooview hello delad nyckel för hello Azure VPN-anslutning med någon av följande metoder hello:

**Azure Portal**

1. Gå toohello VPN plats-till-plats-gatewayanslutning som du skapade.

2. I hello **inställningar** klickar du på **delad nyckel**.
    
    ![Delad nyckel](media/vpn-gateway-troubleshoot-site-to-site-cannot-connect/sharedkey.png)

**Azure PowerShell**

För hello Azure Resource Manager-distributionsmodellen:

    Get-AzureRmVirtualNetworkGatewayConnectionSharedKey -Name <Connection name> -ResourceGroupName <Resource group name>

För hello klassiska distributionsmodellen:

    Get-AzureVNetGatewayKey -VNetName -LocalNetworkSiteName

### <a name="step-3-verify-hello-vpn-peer-ips"></a>Steg 3. Kontrollera hello VPN-peer IP-adresser

-   Hej IP-definition i hello **lokala nätverksgateway** objektet i Azure måste matcha hello lokala enhet IP.
-   hello Azure gateway IP-definition som är inställd på hello lokal enhet ska matcha hello Azure gateway IP.

### <a name="step-4-check-udr-and-nsgs-on-hello-gateway-subnet"></a>Steg 4. Kontrollera UDR och NSG: er på hello gateway-undernät

Sök efter och ta bort användardefinierade routning (UDR) eller Nätverkssäkerhetsgrupper (NSG: er) på hello gateway-undernät och testa hello resultat. Validera hello-inställningar som UDR eller NSG tillämpas om hello problemet har lösts.

### <a name="step-5-check-hello-on-premises-vpn-device-external-interface-address"></a>Steg 5. Kontrollera externa gränssnittet för hello lokala VPN-enhetsadress

- Om hello mot Internet IP-adressen för VPN-enhet för hello ingår i hello **lokala nätverket** definition i Azure, kan du uppleva sporadiska frånkopplingar.
- hello måste externa enhetsgränssnittet vara direkt på hello Internet. Det bör finnas några nätverksadresser eller brandvägg mellan hello Internet och hello enhet.
- tooconfigure klustring toohave en virtuell IP-adress för brandvägg måste du bryta hello kluster och exponera hello VPN-enhet direkt tooa offentliga gränssnittet som hello-gateway kan samverka med.

### <a name="step-6-verify-that-hello-subnets-match-exactly-azure-policy-based-gateways"></a>Steg 6. Kontrollera att hello undernät matchar exakt (Azure policy-baserad gateway)

-   Kontrollera att hello undernät matchar exakt mellan hello Azure-nätverk och lokala definitioner för hello virtuella Azure-nätverket.
-   Kontrollera att hello undernät matchar exakt mellan hello **lokala nätverksgateway** och lokala definitioner för hello lokalt nätverk.

### <a name="step-7-verify-hello-azure-gateway-health-probe"></a>Steg 7. Kontrollera hello Azure gateway hälsoavsökningen

1. Gå toohello [hälsoavsökningen](https://&lt;YourVirtualNetworkGatewayIP&gt;:8081/healthprobe).

2. Klicka dig igenom hello certifikatvarning.
3. Om du får svar anses hello VPN-gateway felfritt. Om du inte får ett svar, hello gateway kanske inte är felfri eller hello problemet beror på en NSG på hello gateway-undernätet. hello efter texten är en exempelsvar:

    &lt;? xml-version = ”1.0”? > <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">primära-instansen: GatewayTenantWorker_IN_1 GatewayTenantVersion: 14.7.24.6 < / sträng&gt;

### <a name="step-8-check-whether-hello-on-premises-vpn-device-has-hello-perfect-forward-secrecy-feature-enabled"></a>Steg 8. Kontrollera om hello lokala VPN-enhet har hello PFS-funktionen aktiverad

hello PFS-funktionen kan orsaka problem för frånkoppling. Om hello VPN-enhet har PFS aktiverat, inaktiveras hello. Uppdatera hello VPN-gateway IPsec-princip.

## <a name="next-steps"></a>Nästa steg

-   [Konfigurera ett virtuellt nätverk tooa för plats-till-plats-anslutning](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
-   [Konfigurera en princip för IPsec/IKE för plats-till-plats VPN-anslutningar](vpn-gateway-ipsecikepolicy-rm-powershell.md)
