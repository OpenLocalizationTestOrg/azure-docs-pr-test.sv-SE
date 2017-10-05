---
title: "Skapa en Programgateway med hjälp av Routning regler för URL - Azure CLI 2.0 | Microsoft Docs"
description: "Den här sidan innehåller instruktioner för att skapa, konfigurera en gateway för Azure-program som använder routningsregler för URL"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: gwallace
ms.openlocfilehash: 958049830d6753ec26635f18f8f8b2fabdec0733
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="create-an-application-gateway-using-path-based-routing-with-azure-cli-20"></a>Skapa en Programgateway använder sökväg-baserade routning med Azure CLI 2.0

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-url-route-portal.md)
> * [PowerShell och Azure Resource Manager](application-gateway-create-url-route-arm-ps.md)
> * [Azure CLI 2.0](application-gateway-create-url-route-cli.md)

URL-sökväg-baserad routning kan du associera rutter baserat på en URL-sökväg för en Http-begäran. Den kontrollerar om det finns en väg till en backend-adresspool som konfigurerats för den URL som visas i Programgatewayen och skickar nätverkstrafiken till definierade backend-poolen. Ett vanligt användningsområde för URL-baserade routning är att belastningsutjämna förfrågningar för olika typer av innehåll till olika backend-serverpooler.

URL-baserade routning introducerar en ny regeltyp av i Programgateway. Programgateway har två regeltyper: basic och PathBasedRouting. Grundläggande regeltyp resursallokering tjänst för backend-pooler när PathBasedRouting förutom resursallokering (round robin), också beaktas sökvägar för den begärda Webbadressen vid val av backend-poolen.

## <a name="scenario"></a>Scenario

I följande exempel Programgateway betjäna trafik för contoso.com med två backend-server-adresspooler: en standard-serverpoolen och en bild serverpoolen.

Begäranden för http://contoso.com/image * dirigeras till bilden serverpoolen (imagesBackendPool), om inte matchar sökvägar, poolen server standard (appGatewayBackendPool) är markerad.

![URL-väg](./media/application-gateway-create-url-route-cli/scenario.png)

## <a name="log-in-to-azure"></a>Logga in på Azure

Öppna den **Kommandotolken för Microsoft Azure**, och logga in. 

```azurecli
az login -u "username"
```

> [!NOTE]
> Du kan också använda `az login` utan att växeln för inloggning på enheten som kräver att ange en kod i aka.ms/devicelogin.

När du skriver i föregående exempel, har en kod angetts. Navigera till https://aka.ms/devicelogin i en webbläsare för att fortsätta inloggningen.

![cmd visar enhet inloggning][1]

Ange den kod som du fick i webbläsaren. Du omdirigeras till en inloggningssida.

![webbläsare för att ange koden][2]

När du har angett koden du är inloggad, Stäng webbläsaren för att fortsätta scenariot.

![loggat in][3]

## <a name="add-a-path-based-rule-to-an-existing-application-gateway"></a>Lägga till en sökväg-baserade regel till en befintlig Programgateway

Skapa en Programgateway med en regel för en definierad

### <a name="create-a-new-back-end-pool"></a>Skapa en ny backend-pool

Konfigurera gateway programinställning **imagesBackendPool** för nätverkstrafik Utjämning av nätverksbelastning i backend-poolen. I det här exemplet kan du konfigurera olika backend-processpool-inställningar för den nya backend-poolen. Varje serverdelspool kan ha sin egen serverdelspoolinställning.  Serverdelens HTTP-inställningar används av regler för att dirigera trafik till rätt medlemmar i serverdelspoolen. Anger protokoll och port som används när du skickar trafik till backend-poolmedlemmar. Cookie-baserade sessioner bestäms också av serverdelens HTTP-inställningar.  Om cookie-baserad sessionstillhörighet är aktiverat skickas trafiken till samma serverdel som tidigare begäranden för varje paket.

```azurecli-interactive
az network application-gateway address-pool create \
--gateway-name AdatumAppGateway \
--name imagesBackendPool  \
--resource-group myresourcegroup \
--servers 10.0.0.6 10.0.0.7
```

### <a name="create-a-new-front-end-port"></a>Skapa en ny frontend-port

Konfigurera klientdelsporten för en programgateway. Konfigurationsobjektet för klientdelsporten används av en lyssnare för att definiera vilken port som Application Gateway lyssnar efter trafik på.

```azurecli-interactive
az network application-gateway frontend-port create --port 82 --gateway-name AdatumAppGateway --resource-group myresourcegroup --name port82
```

### <a name="create-a-new-listener"></a>Skapa en ny lyssnare

Konfigurera lyssnaren. Det här steget konfigurerar lyssnaren för den offentliga IP-adressen och porten som används för att ta emot inkommande nätverkstrafik. I följande exempel tar tidigare konfigurerade frontend IP-konfigurationen och frontend portkonfiguration ett protokoll (http eller https) och konfigurerar lyssnaren. I det här exemplet lyssnaren lyssnar efter HTTP-trafik på port 82 på den offentliga IP-adressen som du skapade tidigare.

```azurecli-interactive
az network application-gateway http-listener create --name imageListener --frontend-ip appGatewayFrontendIP  --frontend-port port82 --resource-group myresourcegroup --gateway-name AdatumAppGateway
```

### <a name="create-the-url-path-map"></a>Skapa Url-sökväg karta

Konfigurera URL: en regel sökvägar för backend-pooler. Det här steget konfigurerar du den relativa sökvägen som används av Programgateway för att definiera mappning mellan en URL-sökväg och vilka backend-adresspool har tilldelats för den inkommande trafiken.

> [!IMPORTANT]
> Varje sökväg måste börja med / och endast en ”\*” är tillåtna, finns i slutet. Giltiga exempel är /xyz, /xyz* eller/xyz / *. Strängen som skickas till sökvägen matcher innehåller inte någon text efter först ””? eller ”#”, och dessa tecken är inte tillåtna. 

I följande exempel skapas en regel för ”/ bilder / *” sökväg dirigera trafiken till backend-”imagesBackendPool”. Den här regeln innebär att trafik för varje uppsättning URL-adresser dirigeras till serverdelen. Till exempel går http://adatum.com/images/figure1.jpg till ”imagesBackendPool”. Om sökvägen inte matchar någon av de fördefinierade sökvägsreglerna, konfigurerar regelkonfigurationen sökväg kartan även en standard backend-adresspool. Till exempel går http://adatum.com/shoppingcart/test.html till pool1 som har definierats som standard för omatchade trafik.

```azurecli-interactive
az network application-gateway url-path-map create \
--gateway-name AdatumAppGateway \
--name imagespathmap \
--paths /images/* \
--resource-group myresourcegroup2 \
--address-pool imagesBackendPool \
--default-address-pool appGatewayBackendPool \
--default-http-settings appGatewayBackendHttpSettings \
--http-settings appGatewayBackendHttpSettings \
--rule-name images
```

## <a name="next-steps"></a>Nästa steg

Om du vill veta om Secure Sockets Layer (SSL)-avlastning, se [konfigurera en Programgateway för SSL-avlastning](application-gateway-ssl-cli.md).


[scenario]: ./media/application-gateway-create-url-route-cli/scenario.png
[1]: ./media/application-gateway-create-url-route-cli/figure1.png
[2]: ./media/application-gateway-create-url-route-cli/figure2.png
[3]: ./media/application-gateway-create-url-route-cli/figure3.png
