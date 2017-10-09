---
title: "Lägga till flera VPN-gateway för plats-till-plats-anslutningar tooa VNet: Azure-portalen: Resource Manager | Microsoft Docs"
description: "Lägga till flera platser S2S-anslutningar tooa VPN-gateway som har en befintlig anslutning"
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f3e8b165-f20a-42ab-afbb-bf60974bb4b1
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/20/2017
ms.author: cherylmc
ms.openlocfilehash: b8c9ff454967f509dcef725f8bcec8564fad9b00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-site-to-site-connection-tooa-vnet-with-an-existing-vpn-gateway-connection"></a>Lägg till en plats-till-plats-anslutning tooa VNet med en befintlig anslutning för VPN-gateway

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
> * [PowerShell (klassisk)](vpn-gateway-multi-site.md)
>
> 

Den här artikeln vägleder dig genom att använda hello Azure portal tooadd plats-till-plats (S2S) anslutningar tooa VPN-gateway som har en befintlig anslutning. Den här typen av anslutning är ofta hänvisas tooas ”flera” platskonfiguration. Du kan lägga till en S2S-anslutning tooa VNet som redan har en S2S-anslutning, punkt-till-plats-anslutning eller VNet-till-VNet-anslutningen. Det finns vissa begränsningar när du lägger till anslutningar. Kontrollera hello [innan du börjar](#before) avsnitt i den här artikeln tooverify innan du börjar din konfiguration. 

Den här artikeln gäller tooVNets som skapats med hjälp av hello Resource Manager-modellen som har en RouteBased VPN-gateway. De här stegen gäller inte konfigurationer för samtidiga tooExpressRoute/plats-till-plats-anslutning. Se [ExpressRoute/S2S samtidiga anslutningar](../expressroute/expressroute-howto-coexist-resource-manager.md) information om samtidiga anslutningar.

### <a name="deployment-models-and-methods"></a>Distributionsmodeller och distributionsmetoder
[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

Vi uppdatera den här tabellen som nya artiklar och ytterligare verktyg blir tillgängliga för den här konfigurationen. När det finns en artikel länkar vi direkt tooit från den här tabellen.

[!INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)]

## <a name="before"></a>Innan du börjar
Kontrollera hello följande objekt:

* Du skapar inte en samtidiga ExpressRoute/S2S-anslutning.
* Du har ett virtuellt nätverk som har skapats med en befintlig anslutning hello Resource Manager-modellen.
* hello virtuell nätverksgateway för din VNet är RouteBased. Om du har en PolicyBased VPN-gateway, måste du ta bort hello virtuell nätverksgateway och skapa en ny VPN-gateway som RouteBased.
* Ingen av hello adressintervall överlappa för någon av hello Vnet som ansluter till detta virtuella nätverk.
* Du har kompatibla VPN-enhet och någon som kan tooconfigure den. Se [Om VPN-enheter](vpn-gateway-about-vpn-devices.md). Om du inte är bekant med att konfigurera din VPN-enhet eller inte är bekant med hello IP-adressintervall som finns i din lokala nätverkskonfigurationen, måste toocoordinate med någon som kan ge de detaljer du.
* Du har ett externt Internetriktade offentliga IP-adressen för VPN-enhet. Den här IP-adressen får inte finnas bakom en NAT.

## <a name="part1"></a>Del 1 – konfigurera en anslutning
1. Från en webbläsare, navigerar du toohello [Azure-portalen](http://portal.azure.com) och vid behov, logga in med ditt Azure-konto.
2. Klicka på **alla resurser** och leta upp din **virtuell nätverksgateway** hello listan över resurser och klicka på den.
3. På hello **virtuell nätverksgateway** bladet, klickar du på **anslutningar**.
   
    ![Bladet Anslutningar](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/connectionsblade.png "Bladet Anslutningar")<br>
4. På hello **anslutningar** bladet, klickar du på **+ Lägg till**.
   
    ![Lägg till anslutning för](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addbutton.png "Lägg till anslutning för")<br>
5. På hello **Lägg till anslutning** bladet registrerar hello följande fält:
   
   * **Namn:** hello-namn som du vill toogive toohello plats som du skapar hello anslutning till.
   * **Anslutningstyp:** Välj **plats-till-plats (IPsec)**.
     
     ![Lägg till anslutning bladet](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addconnectionblade.png "Lägg till anslutning bladet")<br>

## <a name="part2"></a>Del 2 – Lägg till en lokal nätverksgateway
1. Klicka på **lokal nätverksgateway** ***Välj en lokal nätverksgateway***. Då öppnas hello **Välj lokal nätverksgateway** bladet.
   
    ![Välj lokal nätverksgateway](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/chooselng.png "Välj lokal nätverksgateway")<br>
2. Klicka på **Skapa nytt** tooopen hello **skapa lokal nätverksgateway** bladet.
   
    ![Skapa lokal nätverksgateway-bladet](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/createlngblade.png "skapa lokal nätverksgateway")<br>
3. På hello **skapa lokal nätverksgateway** bladet registrerar hello följande fält:
   
   * **Namn:** hello-namn som du vill toogive toohello lokala gateway nätverksresurs.
   * **IP-adress:** hello hello VPN-enheten på hello-plats som du vill tooconnect till offentliga IP-adress.
   * **Adressutrymmet:** hello-adressutrymmet som du vill toobe dirigeras toohello ny lokal nätverksplats.
4. Klicka på **OK** på hello **skapa lokal nätverksgateway** bladet toosave hello ändringar.

## <a name="part3"></a>Del 3 – Lägg till hello delad nyckel och skapa hello-anslutning
1. På hello **Lägg till anslutning** bladet Lägg till hello delad nyckel som du vill toouse toocreate anslutningen. Du kan hämta hello delade nyckeln från din VPN-enhet eller skapa en här och konfigurera din VPN-enhet toouse hello samma delad nyckel. hello viktig sak är att hello nycklar är exakt hello samma.
   
    ![Delad nyckel](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/sharedkey.png "Delad nyckel")<br>
2. Hello längst ned på hello-bladet, klickar du på **OK** toocreate hello anslutning.

## <a name="part4"></a>Del 4 – verifiera hello VPN-anslutning


[!INCLUDE [vpn-gateway-verify-connection-ps-rm](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="next-steps"></a>Nästa steg

När anslutningen är klar kan du lägga till virtuella datorer tooyour virtuella nätverk. Se hello virtuella datorer [Utbildningsväg](https://azure.microsoft.com/documentation/learning-paths/virtual-machines) för mer information.
