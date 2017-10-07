---
title: "regler för en Programgateway med hjälp av Routning av URL - aaaCreate Azure CLI 2.0 | Microsoft Docs"
description: "Den här sidan finns instruktioner toocreate, konfigurera en gateway för Azure-program som använder routningsregler för URL"
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
ms.openlocfilehash: 335b52be258945e1172eb0252b732e0e6ecb2ef0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-using-path-based-routing-with-azure-cli-20"></a>Skapa en Programgateway använder sökväg-baserade routning med Azure CLI 2.0

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-url-route-portal.md)
> * [PowerShell och Azure Resource Manager](application-gateway-create-url-route-arm-ps.md)
> * [Azure CLI 2.0](application-gateway-create-url-route-cli.md)

URL-sökväg-baserade routning kan du tooassociate rutter baserat på hello URL-sökvägen för en Http-begäran. Den kontrollerar om det finns en väg tooa backend-adresspool för hello-URL som visas i hello Programgateway och skickar hello nätverket trafik toohello definierats backend-adresspool. Ett vanligt användningsområde för URL-baserade routning är tooload begäranden för olika typer av innehåll toodifferent backend-serverpooler.

URL-baserade routning introducerar en ny regel typen tooapplication gateway. Programgateway har två regeltyper: basic och PathBasedRouting. Grundläggande regeltyp tillhandahåller resursallokering tjänst för hello backend-pooler när PathBasedRouting dessutom tooround robin distribution kan också beaktas sökvägar för hello URL-begäran vid val hello backend-adresspool.

## <a name="scenario"></a>Scenario

I följande exempel hello, Programgateway betjäna trafik för contoso.com med två backend-server-adresspooler: en standard-serverpoolen och en bild serverpoolen.

Begäranden för http://contoso.com/image * är dirigeras tooimage serverpoolen (imagesBackendPool), om hello sökvägar matchar inte, poolen server standard (appGatewayBackendPool) är markerad.

![URL-väg](./media/application-gateway-create-url-route-cli/scenario.png)

## <a name="log-in-tooazure"></a>Logga in tooAzure

Öppna hello **Kommandotolken för Microsoft Azure**, och logga in. 

```azurecli
az login -u "username"
```

> [!NOTE]
> Du kan också använda `az login` utan hello växeln för inloggning på enheten som kräver att ange en kod i aka.ms/devicelogin.

När du skriver hello föregående exempel, har en kod angetts. Navigera toohttps://aka.ms/devicelogin i en webbläsare toocontinue hello-inloggningen.

![cmd visar enhet inloggning][1]

Ange hello koden du fått i hello webbläsare. Du kan omdirigerade tooa inloggningssidan.

![webbläsaren tooenter kod][2]

När hello kod har registrerats du är inloggad, Stäng hello webbläsare toocontinue med hello scenario.

![loggat in][3]

## <a name="add-a-path-based-rule-tooan-existing-application-gateway"></a>Lägg till en sökväg-baserade regler tooan befintliga Programgateway

Skapa en Programgateway med en regel för en definierad

### <a name="create-a-new-back-end-pool"></a>Skapa en ny backend-pool

Konfigurera gateway programinställning **imagesBackendPool** för hello belastningsutjämnad nätverkstrafik i hello backend-adresspool. I det här exemplet kan du konfigurera olika backend-processpool-inställningar för hello ny backend-pool. Varje serverdelspool kan ha sin egen serverdelspoolinställning.  Serverdelens HTTP-inställningar som används av regler tooroute trafik toohello rätt backend poolmedlemmar. Detta avgör hello protokoll och port som används när en skickar trafik toohello backend poolmedlemmar. Cookie-baserad sessioner också bestäms av hello serverdelens HTTP-inställningar.  Om aktiverad, cookie-baserad session tillhörighet skickar trafik toohello samma backend som tidigare begäranden för varje paket.

```azurecli-interactive
az network application-gateway address-pool create \
--gateway-name AdatumAppGateway \
--name imagesBackendPool  \
--resource-group myresourcegroup \
--servers 10.0.0.6 10.0.0.7
```

### <a name="create-a-new-front-end-port"></a>Skapa en ny frontend-port

Konfigurera hello frontend-port för en Programgateway. konfigurationsobjekt för hello frontend-port används av en lyssnare toodefine vad port hello Programgateway lyssnar efter trafik på hello lyssnare.

```azurecli-interactive
az network application-gateway frontend-port create --port 82 --gateway-name AdatumAppGateway --resource-group myresourcegroup --name port82
```

### <a name="create-a-new-listener"></a>Skapa en ny lyssnare

Konfigurera hello-lyssnaren. Det här steget konfigurerar hello-lyssnare för hello offentlig IP-adress och port används tooreceive inkommande nätverkstrafik. hello följande exempel tar hello som tidigare har konfigurerat frontend IP-konfiguration, frontend portkonfiguration och ett protokoll (http eller https) och konfigurerar hello-lyssnaren. I det här exemplet lyssnar hello lyssnare tooHTTP trafik på port 82 på hello offentlig IP-adress som du skapade tidigare.

```azurecli-interactive
az network application-gateway http-listener create --name imageListener --frontend-ip appGatewayFrontendIP  --frontend-port port82 --resource-group myresourcegroup --gateway-name AdatumAppGateway
```

### <a name="create-hello-url-path-map"></a>Skapa hello Url-sökväg karta

Konfigurera URL: en regel sökvägar för hello backend-pooler. Det här steget konfigurerar hello relativ sökväg används av gateway toodefine hello programmappning mellan URL-sökväg och vilka backend-adresspool har tilldelats toohandle hello inkommande trafik.

> [!IMPORTANT]
> Varje sökväg måste börja med / och hello endast placera en ”\*” tillåts, är hello slutet. Giltiga exempel är /xyz, /xyz* eller/xyz / *. hello strängen matas toohello sökväg matcher inte innehåller någon text efter hello först ””? eller ”#”, och dessa tecken är inte tillåtna. 

hello följande exempel skapas en regel för ”/ bilder / *” sökväg routning trafik tooback slutpunkt ”imagesBackendPool”. Den här regeln garanterar att trafik för varje uppsättning URL: er är routade toohello backend. Till exempel http://adatum.com/images/figure1.jpg går för ”imagesBackendPool”. Om hello sökväg inte matchar någon av hello fördefinierade sökvägsregler, konfigurerar hello regelkonfigurationen sökväg kartan även en standard backend-adresspool. Till exempel blir http://adatum.com/shoppingcart/test.html toopool1 som har definierats som hello Standardpool för omatchade trafik.

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

Om du vill toolearn om Secure Sockets Layer (SSL)-avlastning, se [konfigurera en Programgateway för SSL-avlastning](application-gateway-ssl-cli.md).


[scenario]: ./media/application-gateway-create-url-route-cli/scenario.png
[1]: ./media/application-gateway-create-url-route-cli/figure1.png
[2]: ./media/application-gateway-create-url-route-cli/figure2.png
[3]: ./media/application-gateway-create-url-route-cli/figure3.png
