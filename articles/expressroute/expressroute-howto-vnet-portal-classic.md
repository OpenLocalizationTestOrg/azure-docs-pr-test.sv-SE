---
title: "aaaConfigure ett virtuellt nätverk och Gateway för ExpressRoute i hello klassiska portal | Microsoft Docs"
description: "Den här artikeln vägleder dig genom att konfigurera ett virtuellt nätverk för ExpressRoute med hello klassiska distributionsmodellen och hello klassiska portalen."
documentationcenter: na
services: expressroute
author: cherylmc
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: 688817d9-51c8-4372-9af8-33fa098c7c5a
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/20/2016
ms.author: cherylmc
ms.openlocfilehash: dd1f6c5e85dbb3ad0a53ecd81c13b4d3f5c06e66
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-for-expressroute-in-hello-classic-portal"></a>Skapa ett virtuellt nätverk för ExpressRoute i hello klassiska portalen
hello stegen i den här artikeln vägleder dig genom att konfigurera ett virtuellt nätverk och en virtuell nätverksgateway för användning med ExpressRoute med hello klassiska distributionsmodellen och hello klassiska portalen.

Om du behöver anvisningar för hello Resource Manager-distributionsmodellen kan du använda hello följande artiklar: [skapa ett virtuellt nätverk med hjälp av PowerShell](../virtual-network/virtual-networks-create-vnet-arm-ps.md) och [lägga till en VPN-Gateway tooa Resource Manager VNet för ExpressRoute](expressroute-howto-add-gateway-resource-manager.md).

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**Om distributionsmodeller för Azure**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="create-a-classic-vnet-and-gateway"></a>Skapa en klassiska virtuella nätverk och gateway
hello följande steg skapar du ett klassiskt virtuellt nätverk och en virtuell nätverksgateway. Om du redan har ett klassiska VNet finns hello [konfigurerar ett existerande klassiska VNet](#config) i den här artikeln.

1. Logga in toohello [klassiska Azure-portalen](http://manage.windowsazure.com).
2. I hello nedre vänstra hörnet av hello-skärmen, klickar du på **ny**. Hello navigeringsfönstret, klicka på **nätverkstjänster**, och klicka sedan på **virtuellt nätverk**. Klicka på **skapa anpassade** toobegin hello-konfigurationsguiden.
3. På hello **virtuella nätverksinformation** anger hello följande:
   
   * **Namnet** – Namnge ditt virtuella nätverk. När du distribuerar dina VM: ar och PaaS-instanser, så du kan inte toomake hello namn för komplicerade ska du använda den här virtuella nätverksnamnet.
   * **Plats** – hello platsen är direkt relaterade toohello fysisk plats (region) där du vill att dina resurser (VM)-tooreside. Om du vill hello virtuella datorer som du distribuerar toothis virtuellt nätverk toobe fysiskt belägna i östra USA, markerar du den platsen. Du kan inte ändra hello region har associerats med det virtuella nätverket när du har skapat.
4. På hello **DNS-servrar och VPN-anslutningen** sidan Ange hello följande information och klicka sedan på pilen hello nästa hello nedre högra hörnet. 
   
   * **DNS-servrar** – ange hello DNS-servernamn och IP-adress eller välj en tidigare registrerad DNS-server hello snabbmenyn. Du skapar inte en DNS-server med den här inställningen. Det gör att du toospecify hello DNS-servrar som du vill toouse för namnmatchning för det här virtuella nätverket.
   * **Plats-till-plats-anslutning** – Välj hello kryssrutan för **konfigurera en plats-till-plats-VPN**.
   * **ExpressRoute** – Välj hello kryssrutan **Använd ExpressRoute**. Det här alternativet visas bara om du har valt **konfigurera en plats-till-plats-VPN**.
   * **Lokala nätverket** -du är obligatoriska toohave en lokal nätverksplats för ExpressRoute. Dock hello gäller en ExpressRoute-anslutning angetts hello adressprefix för hello lokal nätverksplats kommer att ignoreras. I stället annonserade hello adressprefix tooMicrosoft via hello ExpressRoute-kretsen ska användas för routning.<BR>Du kan välja den hello listrutan om du redan har ett lokalt nätverk som skapats för ExpressRoute-anslutning. Om inte, Välj **ange ett nytt lokalt nätverk**.
5. Hej **plats-till-plats-anslutning** visas om du har valt toospecify ett nytt lokalt nätverk i hello föregående steg. tooconfigure ditt lokala nätverk, ange hello följande information och klicka på nästa hello-pilen. 
   
   * **Namnet** -hello-namn som du vill toocall din lokala nätverksplats (lokal).
   * **Adressutrymmet** – inklusive första IP- och CIDR (antal adresser). Du kan ange eventuella adressintervallet så länge det inte överlappar hello-adressintervall för det virtuella nätverket. Vanligtvis detta genom att ange hello-adressintervall för ditt lokala nätverk, men ExpressRoute hello gäller de här inställningarna används inte. Den här inställningen krävs dock i ordning toocreate hello lokala nätverk när du använder hello klassiska portalen.
   * **Lägg till adressutrymme** -den här inställningen är inte relevant för ExpressRoute.
6. På hello **virtuella nätverk adressutrymmen** , ange hello följande information och klickar sedan på bockmarkeringen hello hello nedre högra tooconfigure nätverket. 
   
   * **Adressutrymmet** – inklusive startar IP och count-adress. Kontrollera att hello adressutrymmen som du anger inte överlappa hello-adressutrymmen som du har på det lokala nätverket.
   * **Lägg till undernät** – inklusive första IP- och antal adresser. Ytterligare undernät krävs inte.
   * **Lägg till gateway-undernätet** -klickar du på tooadd hello gateway-undernätet. hello gateway-undernätet används endast för hello virtuell nätverksgateway och krävs för den här konfigurationen.<BR>Hej gatewayundernät CIDR (antal adresser) för ExpressRoute måste vara /28 eller större (/ 27/26 osv.). Detta ger tillräckligt med IP-adresser i det undernätet tooallow hello configuration toowork. Om du har valt hello kryssrutan toouse ExpressRoute i hello klassiska portalen anger ett gateway-undernät med /28 hello-portalen.  Du kan justera hello CIDR-adress antalet i hello klassiska portalen. Hej gatewayundernät visas som **Gateway** i hello klassiska portalen, även om hello verkliga namnet på hello gateway-undernät som skapas är faktiskt **GatewaySubnet**. Du kan visa det här namnet med hjälp av PowerShell eller hello Azure-portalen.
7. Klicka på hello bockmarkeringen hello längst ned på sidan för hello och det virtuella nätverket börjar toocreate. När den har slutförts visas **Skapad** visas under **Status** på hello **nätverk** sida i hello klassiska portalen.

## <a name="gw"></a>Skapa hello gateway
1. På hello **nätverk** sidan och hello virtuellt nätverk som du just skapat, klicka **instrumentpanelen** hello överst på hello sidan.
2. På hello längst ned på hello **instrumentpanelen** klickar du på **skapa Gateway** och välj **dynamisk routning**. Klicka på **Ja** tooconfirm som du vill toocreate en gateway.
3. När hello gateway börjar skapa, visas ett meddelande låta som du vet att hello-gatewayen har startats. Det kan ta upp too45 minuter för hello gateway toocreate.
4. Länka nätverket tooa kretsen. Följ instruktionerna för hello i hello artikeln [hur toolink Vnet tooExpressRoute kretsar](expressroute-howto-linkvnet-classic.md).

## <a name="config"></a>Konfigurera en befintlig klassiska virtuella nätverk för ExpressRoute
Om du redan har ett klassiskt virtuellt nätverk kan konfigurera du den tooconnect tooExpressRoute i hello klassiska portalen. hello inställningar är du hello en samma som hello ovan, så Läs igenom dessa avsnitt toobecome bekant med hello nödvändiga inställningar. Om du vill toocreate en samtidiga ExpressRoute/plats-till-plats-anslutning, se [i den här artikeln](expressroute-howto-coexist-classic.md) hello anvisningar. De är annorlunda än hello stegen i den här artikeln.

1. Du måste toocreate hello lokala nätverket innan du uppdaterar hello resten av inställningarna för virtuella nätverk. toocreate en ny lokala nätverk, vilket krävs när du konfigurerar ExpressRoute via hello klassiska portal, klicka på **ny**  **>**  **nätverkstjänster**  **>**  **Virtuellt nätverk**  **>**  **Lägg till lokala nätverket**. Följ hello guiden steg toocreate hello lokala nätverket.
2. Använd **konfigurera** sidan tooupdate hello resten av hello inställningar för virtuella nätverk och tooassociate hello VNet toohello nätverket.
3. När du har konfigurerat inställningarna för hello gå toohello [skapa hello gateway](#gw) avsnitt i den här artikeln toocreate hello gatewayen.

## <a name="next-steps"></a>Nästa steg
* Om du vill tooadd virtuella datorer tooyour virtuellt nätverk, se [virtuella datorer utbildningsvägar](https://azure.microsoft.com/documentation/learning-paths/virtual-machines/).
* Om du vill toolearn mer om ExpressRoute finns hello [ExpressRoute översikt](expressroute-introduction.md).

