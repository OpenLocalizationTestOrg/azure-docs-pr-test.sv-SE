---
title: "Ansluta din lokala nätverk tooan virtuella Azure-nätverket: plats-till-plats-VPN: Portal | Microsoft Docs"
description: "Steg toocreate en IPSec-anslutning från ditt lokala nätverk tooan virtuella Azure-nätverket via hello offentliga Internet. Dessa steg hjälper dig att skapa en anslutning för plats-till-plats VPN-Gateway av mellan platser med hjälp av hello portal."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 827a4db7-7fa5-4eaf-b7e1-e1518c51c815
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/02/2017
ms.author: cherylmc
ms.openlocfilehash: 6f0acbaf1bf016026cefade048a116e94686103d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-site-to-site-connection-in-hello-azure-portal"></a>Skapa en plats-till-plats-anslutning i hello Azure-portalen

Den här artikeln visar hur hello toouse Azure portal toocreate plats-till-plats VPN-gateway-anslutningen från ditt lokala nätverk toohello VNet. hello stegen i den här artikeln gäller toohello Resource Manager-modellen. Du kan också skapa den här konfigurationen med hjälp av en annan distributionsverktyget eller distributionsmodell genom att välja ett annat alternativ hello följande lista:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [CLI](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
> * [Azure Portal (klassisk)](vpn-gateway-howto-site-to-site-classic-portal.md)
> 
>

En plats-till-plats VPN-gateway-anslutningen har använt tooconnect ditt lokala nätverk tooan virtuella Azure-nätverket via en IPsec/IKE (IKEv1 eller IKEv2) VPN-tunnel. Den här typen av anslutning kräver en VPN-enhet som finns lokalt som har ett externt Internetriktade offentliga IP-adress som tilldelats tooit. Mer information om VPN-gatewayer finns i [Om VPN-gateway](vpn-gateway-about-vpngateways.md).

![Diagram över plats-till-plats-anslutning med VPN-gateway](./media/vpn-gateway-howto-site-to-site-resource-manager-portal/site-to-site-diagram.png)

## <a name="before-you-begin"></a>Innan du börjar

Kontrollera att du har uppfyllt hello följande villkor innan du börjar din konfiguration:

* Kontrollera att du har en kompatibel VPN-enhet och någon som kan tooconfigure den. Se [Om VPN-enheter](vpn-gateway-about-vpn-devices.md) för mer information om kompatibla VPN-enheter och enhetskonfiguration.
* Kontrollera att du har en extern offentlig IPv4-adress för VPN-enheten. Den här IP-adressen får inte finnas bakom en NAT.
* Om du är bekant med hello IP-adressintervall som finns i ditt lokala nätverk konfiguration måste du toocoordinate med någon som kan ge de detaljer du. När du skapar den här konfigurationen måste du ange hello IP-adressintervall adressprefix Azure dirigerar tooyour lokal plats. Ingen av hello undernät i ditt lokala nätverk kan över lap med hello virtuella undernät som du vill tooconnect till. 

### <a name="values"></a>Exempelvärden

hello exemplen i den här artikeln används hello följande värden. Du kan använda dessa värden toocreate en testmiljö eller referera toothem toobetter förstå hello exemplen i den här artikeln.

* **VNet-namn:** TestVNet1
* **Adressutrymme:** 
  * 10.11.0.0/16
  * 10.12.0.0/16 (valfritt för den här övningen)
* **Undernät:**
  * FrontEnd: 10.11.0.0/24
  * BackEnd: 10.12.0.0/24 (valfritt för den här övningen)
* **GatewaySubnet:** 10.11.255.0/27
* **Resursgrupp:** TestRG1
* **Plats:** Östra USA
* **DNS Server:** Valfritt. hello IP-adressen för DNS-servern.
* **Namn på virtuell nätverksgateway:** VNet1GW
* **Offentlig IP:** VNet1GWIP
* **VPN-typ:** Routningsbaserad
* **Anslutningstyp:** Plats-till-plats (IPsec)
* **Gateway-typ:** VPN
* **Gateway-namn på lokalt nätverk:** Site2
* **Anslutningsnamn:** VNet1toSite2

## <a name="CreatVNet"></a>1. Skapa ett virtuellt nätverk

[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-s2s-rm-portal-include.md)]

## <a name="dns"></a>2. Ange en DNS-server

DNS är inte obligatoriska toocreate plats-till-plats-anslutning. Om du vill toohave namnmatchning för resurser som är distribuerade tooyour virtuella nätverk, ska du ange en DNS-server. Den här inställningen kan du ange hello DNS-server som du vill toouse för namnmatchning för det här virtuella nätverket. Den skapar inte någon DNS-server. Mer information om namnmatchning finns i [Namnmatchning för virtuella datorer](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

[!INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="gatewaysubnet"></a>3. Skapa hello gateway-undernät

[!INCLUDE [vpn-gateway-aboutgwsubnet](../../includes/vpn-gateway-about-gwsubnet-include.md)]

[!INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-s2s-rm-portal-include.md)]

## <a name="VNetGateway"></a>4. Skapa hello VPN-gateway

[!INCLUDE [vpn-gateway-add-gw-s2s-rm-portal](../../includes/vpn-gateway-add-gw-s2s-rm-portal-include.md)]

## <a name="LocalNetworkGateway"></a>5. Skapa hello lokal nätverksgateway

hello lokal nätverksgateway refererar vanligtvis tooyour lokal plats. Du namnge hello plats som Azure kan se tooit och sedan ange hello IP-adress för hello lokala VPN-enhet toowhich ska du skapa en anslutning. Du kan också ange hello IP-adressprefix som vidarebefordras via hello VPN-gateway toohello VPN-enhet. hello-adressprefix som du anger är hello prefix finns i ditt lokala nätverk. Om ändringarna lokala nätverk eller om du behöver toochange hello offentliga IP-adressen för hello VPN-enhet kan uppdatera du enkelt hello värden senare.

[!INCLUDE [Add local network gateway](../../includes/vpn-gateway-add-lng-s2s-rm-portal-include.md)]

## <a name="VPNDevice"></a>6. Konfigurera din VPN-enhet

Plats-till-platsanslutningar tooan lokalt nätverk kräver en VPN-enhet. I det här steget konfigurerar du VPN-enheten. När du konfigurerar VPN-enhet behöver du hello följande:

- En delad nyckel. Detta är hello samma delade nyckel som du anger när du skapar din plats-till-plats VPN-anslutning. I vårt exempel använder vi en enkel delad nyckel. Vi rekommenderar att du generera en mer komplex viktiga toouse.
- hello offentliga IP-adressen för din virtuella nätverksgateway. Du kan visa hello offentliga IP-adressen med hjälp av hello Azure-portalen, PowerShell eller CLI. Navigera toofind hello offentliga IP-adressen för din VPN-gateway med hello Azure-portalen för**virtuella nätverksgatewayer**, klicka på hello namnet på din gateway.

[!INCLUDE [Configure a VPN device](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

## <a name="CreateConnection"></a>7. Skapa hello VPN-anslutning

Skapa hello plats-till-plats VPN-anslutning mellan din virtuella nätverksgateway och din lokala VPN-enhet.

[!INCLUDE [Add connections](../../includes/vpn-gateway-add-site-to-site-connection-s2s-rm-portal-include.md)]

## <a name="VerifyConnection"></a>8. Kontrollera hello VPN-anslutning

[!INCLUDE [Verify - Azure portal](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="connectVM"></a>tooconnect tooa virtuell dator

[!INCLUDE [Connect tooa VM](../../includes/vpn-gateway-connect-vm-s2s-include.md)]

## <a name="reset"></a>Hur tooreset en VPN-gateway

Du kan behöva återställa en Azure VPN-gateway om VPN-anslutningen mellan flera platser i en eller flera VPN-tunnlar för plats-till-plats bryts. I så fall kan din lokala VPN-enheter är alla fungerar, men är inte tooestablish IPsec-tunnlar med hello Azure VPN-gatewayer. Stegvisa anvisningar finns i [Återställa en VPN-gateway](vpn-gateway-resetgw-classic.md).

## <a name="resize"></a>Hur toochange en gateway-SKU (ändra storlek på en gateway)

Hello stegen toochange en gateway-SKU, finns [Gateway-SKU: er](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

## <a name="next-steps"></a>Nästa steg

* Information om BGP finns hello [BGP översikt](vpn-gateway-bgp-overview.md) och [hur tooconfigure BGP](vpn-gateway-bgp-resource-manager-ps.md).
* Information om tvingad tunneltrafik finns i [Om forcerade tunnlar](vpn-gateway-forced-tunneling-rm.md).
* Mer information om aktiv-aktiv-anslutningar med hög tillgänglighet finns i [Anslutning med hög tillgänglighet på flera platser och VNet-till-VNet-anslutning](vpn-gateway-highlyavailable.md).
