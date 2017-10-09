---
title: 'Konfigurera en VPN-Gateway: klassiska portal: Azure | Microsoft Docs'
description: "Den här artikeln beskriver hur du konfigurerar VPN-gateway för det virtuella nätverket och ändra en routning för VPN-gateway. De här stegen gäller toohello klassisk distribution modell och hello klassiska portalen."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: fbe59ba8-b11f-4d21-9bb1-225ec6c6d351
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/04/2017
ms.author: cherylmc
ms.openlocfilehash: 00d2fa18bab6eb24b33ddb18113f2a557db638d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-vpn-gateway-in-hello-classic-portal"></a>Konfigurera en VPN-gateway i hello klassiska portalen 
Om du vill toocreate en säker mellan lokala anslutning mellan Azure och den lokala platsen, måste toocreate en virtuell nätverksgateway. En VPN-gateway är en viss typ av virtuell nätverksgateway. I hello klassiska distributionsmodellen kan en VPN-gateway vara något av två typer av VPN-routing: statisk eller dynamisk. hello VPN-typ du väljer beror på både din designplan för nätverket och hello lokala VPN-enhet som du vill toouse. Mer information om VPN-enheter finns [om VPN-enheter](vpn-gateway-about-vpn-devices.md).

**Om distributionsmodeller för Azure**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="configuration-overview"></a>Konfigurationsöversikt
hello hur följande steg du konfigurerar din VPN-gateway i hello klassiska portalen. De här stegen gäller toogateways för virtuella nätverk som har skapats med hello klassiska distributionsmodellen. För närvarande finns inte alla hello konfigurationsinställningar för gatewayer i hello Azure-portalen. När de är skapar vi en ny uppsättning anvisningar som gäller toohello Azure-portalen.

### <a name="before-you-begin"></a>Innan du börjar
Innan du konfigurerar din gateway, måste du först toocreate ditt virtuella nätverk. Steg toocreate ett virtuellt nätverk för korsanslutningar finns [konfigurera ett virtuellt nätverk med en plats-till-plats VPN-anslutning](vpn-gateway-site-to-site-create.md), eller [konfigurera ett virtuellt nätverk med en punkt-till-plats VPN-anslutning](vpn-gateway-point-to-site-create.md). Sedan använder hello följande steg tooconfigure hello VPN-gateway och samla in hello information du behöver tooconfigure din VPN-enhet. 

Om du redan har en VPN-gateway och du vill toochange hello VPN-routning, se [hur toochange hello routning VPN-typ för din gateway](#how-to-change-the-vpn-routing-type-for-your-gateway).

## <a name="create-a-vpn-gateway"></a>Skapa en VPN-gateway
1. I hello [klassiska Azure-portalen](https://manage.windowsazure.com), på hello **nätverk** kontrollerar den hello status-kolumnen för det virtuella nätverket är **Skapad**.
2. I hello **namn** kolumnen hello namnet på det virtuella nätverket.
3. På hello **instrumentpanelen** sidan, Observera att detta virtuella nätverk inte har en gateway som konfigurerats ännu. Den här statusen visas när du går igenom hello steg tooconfigure din gateway.

![Gateway inte har skapats](./media/vpn-gateway-configure-vpn-gateway-mp/IC717025.png)

Därefter hello längst hello-sidan, klickar du på **skapa Gateway**. Du kan välja antingen *statisk routning* eller *dynamisk routning*. hello routning VPN-typ du väljer beror på några faktorer. Till exempel din VPN-enhet stöder och om du behöver toosupport punkt-till-plats-anslutningar. Kontrollera [om VPN-enheter för virtuell nätverksanslutning](vpn-gateway-about-vpn-devices.md) tooverify hello routning VPN-typ som du behöver. När hello gateway har skapats kan ändra du inte mellan VPN-gateway routning typer utan att ta bort och återskapa hello gateway. När hello uppmanas du tooconfirm som du vill hello gateway som har skapats klickar du på **Ja**.

![Gateway-VPN-routning](./media/vpn-gateway-configure-vpn-gateway-mp/IC717026.png)

När din gateway Observera hello gateway grafiken hello sidan ändras tooyellow och säger *skapar Gateway*. Det kan ta upp too45 minuter för hello gateway toocreate. Vänta tills hello gateway har slutförts innan du kan gå vidare med andra inställningar.

![Skapar gateway](./media/vpn-gateway-configure-vpn-gateway-mp/IC717027.png)

När hello gateway ändringar för*ansluta*, du kan samla in hello information du behöver för din VPN-enhet.

![Gateway-anslutning](./media/vpn-gateway-configure-vpn-gateway-mp/IC717028.png)

## <a name="site-to-site-connections"></a>Plats-till-plats-anslutningar

### <a name="step-1-gather-information-for-your-vpn-device-configuration"></a>Steg 1. Samla in information för din konfiguration för VPN-enhet
Om du skapar en plats-till-plats-anslutning när hello gateway har skapats kan du samla in information för VPN-enhetens konfiguration. Den här informationen finns på hello **instrumentpanelen** sidan för det virtuella nätverket:

1. **IP-adressen för gateway -** hello IP-adress finns på hello **instrumentpanelen** sidan. Du kommer inte att kunna toosee förrän efter din gateway är klar att skapa.
2. **Delad nyckel -** klickar du på **hantera nyckeln** längst hello hello-skärmen. Klicka på hello ikonen nästa toohello viktiga toocopy den tooyour Urklipp och klistra in och spara hello nyckel. Den här knappen fungerar bara om det finns en S2S VPN-tunnel. Om du har flera S2S VPN-tunnlar använder hello *hämta virtuella nätverk Gateway delad nyckel* API eller PowerShell-cmdlet.

![Hantera nyckel](./media/vpn-gateway-configure-vpn-gateway-mp/IC717029.png)

### <a name="step-2--configure-your-vpn-device"></a>Steg 2.  Konfigurera din VPN-enhet
Plats-till-platsanslutningar tooan lokalt nätverk kräver en VPN-enhet. Medan vi inte tillhandahålla konfigurationssteg för alla VPN-enheter, kan du hitta hello information i hello följande länkar till hjälp:

- Se [VPN-enheter](vpn-gateway-about-vpn-devices.md) för mer information om kompatibla VPN-enheter. 
- Länkar toodevice konfigurationsinställningar, se [verifiera VPN-enheter](vpn-gateway-about-vpn-devices.md#devicetable). Dessa länkar tillhandahålls i mån av möjlighet. Det är alltid bra toocheck med din enhetstillverkare för hello senaste konfigurationsinformation.
- Mer information om att redigera enhetens konfigurationsexempel finns i [Redigera exempel](vpn-gateway-about-vpn-devices.md#editing).
- Mer information om IPsec-/IKE-parametrar finns i [Parametrar](vpn-gateway-about-vpn-devices.md#ipsec).
- Innan du konfigurerar VPN-enhet bör du kontrollera om det finns [kända kompatibilitetsproblem för enheten](vpn-gateway-about-vpn-devices.md#known) för hello VPN-enhet som du vill toouse.

När du konfigurerar VPN-enhet behöver du hello följande objekt:

- hello offentliga IP-adressen för din virtuella nätverksgateway. Du kan hitta detta genom att gå toohello **översikt** bladet för ditt virtuella nätverk.
- En delad nyckel. Detta är hello samma delade nyckel som du anger när du skapar din plats-till-plats VPN-anslutning. I vårt exempel använder vi en enkel delad nyckel. Du ska skapa en mer komplex viktiga toouse.

När hello VPN-enhet har konfigurerats kan visa du din uppdaterad anslutningsinformation på hello instrumentpanelssida för din VNet.

### <a name="step-3-verify-your-local-network-ranges-and-vpn-gateway-ip-address"></a>Steg 3. Kontrollera ditt lokala nätverksintervall och IP-adressen för VPN-gateway
#### <a name="verify-your-vpn-gateway-ip-address"></a>Kontrollera din IP-adressen för VPN-gateway
För gateway-tooconnect korrekt måste hello IP-adress för VPN-enhet korrekt konfigureras för hello lokala nätverket som du angav för konfigurationen för anslutningar mellan platser. Vanligtvis konfigureras under konfigurationsprocessen för hello plats-till-plats. Men om du tidigare har använt den här lokala nätverket med en annan enhet eller hello IP-adress har ändrats för det lokala nätverket, redigera hello inställningar toospecify hello rätt Gateway IP-adress.

1. tooverify IP-adressen för din gateway klickar du på **nätverk** hello portal till vänster och välj sedan **lokala nätverk** hello överst på hello sidan. Hello VPN-Gateway-adress för varje lokala nätverk som du har skapat visas. tooedit hello IP-adress, markera hello VNet och på **redigera** på hello hello sidans nederkant.
2. På hello **ange ditt lokala nätverksinformation** sidan Redigera hello IP-adress och klicka sedan på nästa hello-pilen längst hello hello sidan.
3. På hello **ange hello adressutrymme** klickar du på bockmarkeringen hello hello nedre högra toosave dina inställningar.

#### <a name="verify-hello-address-ranges-for-your-local-networks"></a>Kontrollera hello-adressintervall för ditt lokala nätverk
För hello rätt trafik tooflow via hello gateway tooyour lokal plats behöver du tooverify att varje IP-adressintervall anges. Varje område måste anges i din Azure **lokala nätverk** konfiguration. Detta kan vara ganska stor aktivitet beroende på hello nätverkskonfigurationen på den lokala platsen. Trafik som är bunden till en IP-adress som ingår i hello visas adressintervall skickas via hello VPN-gateway för virtuellt nätverk. hello-intervall som du listan inte har toobe privata adressintervall, även om du vill hello tooverify som din lokala konfiguration kan ta emot inkommande trafik.

tooadd eller redigera hello-intervall för ett lokalt nätverk använder hello följande steg:

1. tooedit hello IP-adressintervall för ett lokalt nätverk, klickar du på **nätverk** hello portal till vänster och välj sedan **lokala nätverk** hello överst på hello sidan. I hello portal hello enklaste sättet tooview hello-intervall som du har angett finns på hello **redigera** sidan. toosee områden, Välj hello VNet och klickar på **redigera** på hello hello sidans nederkant.
2. På hello **ange ditt lokala nätverksinformation** inte gör några ändringar. Klicka på hello nästa-pilen på hello hello sidans nederkant.
3. På hello **ange hello adressutrymme** kontrollerar adressutrymmesändringarna ditt nätverk. Klicka sedan på hello markering toosave din konfiguration.

## <a name="how-tooview-gateway-traffic"></a>Hur tooview gateway-trafik
Du kan visa din gateway och gateway-trafik från det virtuella nätverket **instrumentpanelen** sidan.

På hello **instrumentpanelen** kan du visa hello följande:

* hello mängden data som passerar genom din gateway, både data i och ut data.
* hello namnen på hello DNS-servrar som har angetts för det virtuella nätverket.
* hello anslutningen mellan din gateway och VPN-enhet.
* hello delad nyckel som används tooconfigure din anslutning tooyour VPN-gatewayenhet.

## <a name="how-toochange-hello-vpn-routing-type-for-your-gateway"></a>Hur toochange hello routning VPN-typ för din gateway
Eftersom vissa konfigurationer för anslutningen är bara tillgängliga för vissa typer av gateway routing, kanske du behöver toochange hello gateway VPN routning typ av en befintlig VPN-gateway. Du kan exempelvis vilja tooadd punkt-till-plats-anslutning tooan befintlig plats-till-plats-anslutning som har en statisk gateway. Punkt-till-plats-anslutningar kräver dynamisk gateway. Det innebär att tooconfigure en P2S-anslutning har toochange din gateway routning VPN-typ från statisk toodynamic.

Om du behöver toochange gateway routning VPN-typ, ta bort hello befintlig gateway och skapa en ny gateway med hello ny routning. Du behöver inte toodelete hello hela virtuellt nätverk toochange hello gateway routning-typen.

Vara säker på att tooverify att VPN-enhet stöder hello routning som du vill toouse innan du kan ändra din gateway routning VPN-typ. toodownload nya routning configuration prov och kontrollera krav för VPN-enhet, finns [om VPN-enheter för virtuell nätverksanslutning](vpn-gateway-about-vpn-devices.md).

> [!IMPORTANT]
> När du tar bort en VPN-gateway för virtuellt nätverk släpps hello VIP tilldelade toohello gateway. När du skapar nytt hello gateway tilldelas en ny VIP tooit.
> 
> 

1. **Ta bort hello befintliga VPN-gateway.**
   
    På hello **instrumentpanelen** för det virtuella nätverket, navigera toohello längst ned på sidan för hello och klickar på **ta bort Gateway**. Vänta tills hello-meddelande som hello gateway har tagits bort. När du har fått hello-meddelande på hello-skärmen att din gateway har tagits bort kan du skapa en ny gateway.
2. **Skapa en ny VPN-gateway.**
   
    Använd hello procedur hello överst i hello sidan toocreate en ny gateway: [skapa en VPN-gateway](#create-a-vpn-gateway).

## <a name="next-steps"></a>Nästa steg
Du kan lägga till virtuella datorer tooyour virtuellt nätverk. Se [hur toocreate anpassade virtuella](../virtual-machines/windows/classic/createportal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Om du vill tooconfigure punkt-till-plats VPN-anslutningen finns [konfigurerar en punkt-till-plats VPN-anslutning](vpn-gateway-point-to-site-create.md).

