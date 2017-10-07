---
title: "Lägga till ett virtuellt nätverk gateway tooa VNet för ExpressRoute: Portal: Azure | Microsoft Docs"
description: "Den här artikeln vägleder dig genom att lägga till ett virtuellt nätverk gateway tooan redan skapat Resource Manager VNet expressroute."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/17/2017
ms.author: cherylmc
ms.openlocfilehash: 9e922af1f3676eeebc569b57c3ae3a22d4e0b395
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-hello-azure-portal"></a>Konfigurera en virtuell nätverksgateway för ExpressRoute med hello Azure-portalen
> [!div class="op_single_selector"]
> * [Resource Manager – Azure Portal](expressroute-howto-add-gateway-portal-resource-manager.md)
> * [Resource Manager – PowerShell](expressroute-howto-add-gateway-resource-manager.md)
> * [Klassisk - PowerShell](expressroute-howto-add-gateway-classic.md)
> * [Video - Azure-portalen](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network)
> 
> 

Den här artikeln guidar dig igenom hello steg tooadd en virtuell nätverksgateway för ett befintligt virtuellt nätverk. Den här artikeln vägleder dig genom hello steg tooadd, ändra storlek på och ta bort en gateway för virtuellt nätverk (VNet) för en befintlig VNet. hello stegen för den här konfigurationen är specifikt för Vnet som har skapats med hello Resource Manager-distributionsmodellen som ska användas i en ExpressRoute-konfiguration. Mer information om gateways för virtuella datornätverk och gateway-konfigurationsinställningar för ExpressRoute finns [om virtuella nätverksgatewayerna expressroute](expressroute-about-virtual-network-gateways.md). 


## <a name="before-beginning"></a>Innan du börjar

hello steg för den här aktiviteten används ett VNet baserat på hello värdena i hello följande konfiguration är en lista. Vi använder den här listan i vårt exempel steg. Du kan kopiera hello listan toouse som en referens, ersätter hello värden med dina egna.

**Referens för konfigurationslistan**

* Virtuella nätverksnamnet = ”TestVNet”
* Virtuella nätverks-adressutrymme = 192.168.0.0/16
* Undernätnamnet = ”FrontEnd” 
    * Undernätsadressutrymmet = ”192.168.1.0/24”
* Resursgruppens namn = ”TestRG”
* Plats = ”östra USA”
* Namnet för gateway-undernätet: ”GatewaySubnet” måste du alltid namnge ett gateway-undernät *GatewaySubnet*.
    * Gateway-undernätsadressutrymmet = ”192.168.200.0/26”
* Gatewaynamnet = ”ERGW”
* Gateway IP-Name = ”MyERGWVIP”
* Gateway-typ = ”ExpressRoute” den här typen krävs för en ExpressRoute-konfiguration.
* Gateway offentliga IP-namn = ”MyERGWVIP”

Du kan visa en [Video](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network) av de här stegen innan du börjar din konfiguration.

## <a name="create-hello-gateway-subnet"></a>Skapa hello gateway-undernät

1. I hello [portal](http://portal.azure.com), navigera toohello Resource Manager virtuella nätverk som du vill toocreate en virtuell nätverksgateway.
2. I hello **inställningar** avsnitt i din VNet-bladet klickar du på **undernät** tooexpand hello undernät-bladet.
3. På hello **undernät** bladet, klickar du på **+ gatewayundernät** tooopen hello **Lägg till undernät** bladet. 
   
    ![Lägg till gateway-undernätet hello](./media/expressroute-howto-add-gateway-portal-resource-manager/addgwsubnet.png "lägga till hello gateway-undernät")


4. Hej **namn** för ditt undernät fylls i automatiskt med hello värdet 'GatewaySubnet'. Det här värdet krävs för Azure toorecognize hello undernät som hello gateway-undernätet. Justera automatiskt ifylld hello **adressintervall** värden toomatch konfigurationskrav. Vi rekommenderar att du skapar ett gateway-undernät med en minst/27 eller större (/ 26/25, etc.). Klicka på **OK** toosave hello värden och skapa hello gateway-undernätet.

    ![Lägg till undernät hello](./media/expressroute-howto-add-gateway-portal-resource-manager/addsubnetgw.png "hello undernätet")

## <a name="create-hello-virtual-network-gateway"></a>Skapa hello virtuell nätverksgateway

1. I hello portal hello vänster, klickar du på  **+**  och Skriv virtuell nätverksgateway i sökningen. Leta upp **virtuell nätverksgateway** i hello sökningen gå tillbaka och klicka hello-post. På hello **virtuell nätverksgateway** bladet, klickar du på **skapa** längst hello hello-bladet. Då öppnas hello **Skapa virtuell nätverksgateway** bladet.
2. På hello **Skapa virtuell nätverksgateway** bladet, Fyll i hello värden för din virtuella nätverksgateway.

    ![Bladet Skapa virtuell nätverksgateway](./media/expressroute-howto-add-gateway-portal-resource-manager/gw.png "Bladet Skapa virtuell nätverksgateway")
3. **Namn**: namnge din gateway. Detta är inte hello samma som att namnge ett gateway-undernät. Det är hello namnet på hello gateway-objekt som du skapar.
4. **Gateway-typ**: Välj **ExpressRoute**.
5. **SKU**: Välj hello gateway SKU hello listrutan.
6. **Plats**: justera hello **plats** fältet toopoint toohello plats där det virtuella nätverket finns. Om hello plats inte pekar toohello region där det virtuella nätverket finns, visas inte hello virtuellt nätverk i hello ”Välj ett virtuellt nätverk” listrutan.
7. Välj hello virtuellt nätverk toowhich önskade tooadd denna gateway. Klicka på **för virtuella nätverk** tooopen hello **Välj ett virtuellt nätverk** bladet. Välj hello virtuella nätverk. Om du inte ser ditt VNet, se till att hello **plats** fältet pekar toohello region som det virtuella nätverket finns.
9. Välj en offentlig IP-adress. Klicka på **offentliga IP-adressen** tooopen hello **Välj offentlig IP-adress** bladet. Klicka på **+ skapa nya** tooopen hello **skapa offentlig IP-adress-bladet**. Ange ett namn för din offentliga IP-adress. Det här bladet skapar en offentlig IP-adress objektet toowhich offentliga IP-adressen tilldelas dynamiskt. Klicka på **OK** toosave dina ändringar toothis bladet.
10. **Prenumerationen**: Kontrollera att korrekt prenumeration har valts hello.
11. **Resursgruppen**: den här inställningen bestäms av hello virtuella nätverk som du väljer.
12. Justera inte hello **plats** när du har angett hello tidigare inställningar.
13. Verifiera inställningarna för hello. Om du vill att din gateway tooappear på hello instrumentpanelen kan du välja **PIN-kod toodashboard** längst hello hello-bladet.
14. Klicka på **skapa** toobegin skapar hello gateway. hello inställningarna verifieras och distribuerar hello gateway. Skapa virtuell nätverksgateway kan ta upp too45 minuter toocomplete.

## <a name="next-steps"></a>Nästa steg
När du har skapat hello VNet gateway kan länka du ditt VNet tooan ExpressRoute-kretsen. Se [länka ett virtuellt nätverk tooan ExpressRoute-krets](expressroute-howto-linkvnet-portal-resource-manager.md).
