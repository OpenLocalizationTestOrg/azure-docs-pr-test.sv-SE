---
title: "aaaCreate en Programgateway använder URL routning regler | Microsoft Docs"
description: "Den här sidan finns instruktioner toocreate, konfigurera en gateway för Azure-program som använder routningsregler för URL"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: d141cfbb-320a-4fc9-9125-10001c6fa4cf
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/03/2017
ms.author: gwallace
ms.openlocfilehash: 54fcccc39e48a933576968ce3d8160518c0d0b7e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-using-path-based-routing"></a>Skapa en Programgateway använder sökväg-baserade Routning

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-url-route-portal.md)
> * [PowerShell och Azure Resource Manager](application-gateway-create-url-route-arm-ps.md)
> * [Azure CLI 2.0](application-gateway-create-url-route-cli.md)

URL-sökväg-baserade routning kan du tooassociate rutter baserat på hello URL-sökvägen för en Http-begäran. Den kontrollerar om det finns en väg tooa backend-adresspool för hello-URL som visas i hello Application Gateway. Sedan skickar hello nätverket trafik toohello definierats backend-adresspool. Ett vanligt användningsområde för URL-baserade routning är tooload begäranden för olika typer av innehåll toodifferent backend-serverpooler.

URL-baserade routning introducerar en ny regel typen tooapplication gateway. Programgateway har två regeltyper: basic och PathBasedRouting. Grundläggande regeltyp tillhandahåller resursallokering tjänst för hello backend-pooler när PathBasedRouting dessutom tooround robin distribution kan också beaktas sökvägar för hello URL-begäran vid val hello serverdelspool.

## <a name="scenario"></a>Scenario

I följande exempel hello, Programgateway betjäna trafik för contoso.com med två backend-server-adresspooler: video-serverpoolen och image-serverpoolen.

Begäran om http://contoso.com/image * dirigeras tooimage serverpoolen (pool1) och http://contoso.com/video * dirigeras toovideo serverpoolen (pool2). Om inget av hello sökväg mönster matchar väljs en standard serverpool (pool1).

![URL-väg](./media/application-gateway-create-url-route-arm-ps/figure1.png)

## <a name="before-you-begin"></a>Innan du börjar

1. Installera hello senaste versionen av hello Azure PowerShell-cmdlets med hello installationsprogram för webbplattform. Du kan hämta och installera hello senaste versionen från hello **Windows PowerShell** avsnitt i hello [Nedladdningssida](https://azure.microsoft.com/downloads/).
2. Du skapar ett virtuellt nätverk och undernät för Application Gateway. Se till att inga virtuella datorer eller molndistributioner använder hello undernät. hello programmet gateway måste finnas ensamt i ett undernät för virtuellt nätverk.
3. hello servrar lagts toohello backend-adresspool toouse hello Programgateway måste finnas eller har skapat sina slutpunkter i hello virtuellt nätverk eller med en offentlig IP-adress/VIP tilldelad.

## <a name="what-is-required-toocreate-an-application-gateway"></a>Vad är obligatoriska toocreate en Programgateway?

* **Backend-serverpoolen:** hello lista över IP-adresser för hello backend-servrar. hello IP-adresser som anges antingen ska tillhöra toohello undernät för virtuellt nätverk eller ska vara en offentlig IP-adress/VIP.
* **Inställningar för backend-serverpool:** Varje pool har inställningar som port, protokoll och cookiebaserad tillhörighet. De här inställningarna är bundet tooa poolen och tillämpade tooall servrar inom hello poolen.
* **Frontend-port:** den här porten är hello offentliga som öppnas på hello Programgateway. Trafik träffar den här porten och sedan hämtar omdirigeras tooone hello backend-servrar.
* **Lyssnare:** hello-lyssnare har en frontend-port, ett protokoll (Http eller Https dessa värden är skiftlägeskänsligt), och hello SSL-certifikatnamn (om hur du konfigurerar SSL-avlastning).
* **Regel:** hello regeln Binder hello lyssnare, hello backend-serverpoolen och definierar vilken backend-server pool hello trafik ska vara riktad toowhen den når en viss lyssnare.

## <a name="create-an-application-gateway"></a>Skapa en programgateway

hello skillnaden mellan att använda den klassiska Azure och Azure Resource Manager är hello order som du kan skapa hello Programgateway och hello-objekt som behöver toobe konfigurerats.

Med Resource Manager alla poster i en Programgateway konfigureras separat och sedan sätta ihop toocreate hello programresursen gateway.

Här följer hello steg som är nödvändiga toocreate en Programgateway:

1. Skapa en resursgrupp för Resource Manager.
2. Skapa ett virtuellt nätverk och undernät offentlig IP för hello Programgateway.
3. Skapa ett konfigurationsobjekt för programgatewayen.
4. Skapa en resurs för en programgateway.

## <a name="create-a-resource-group-for-resource-manager"></a>Skapa en resursgrupp för Resource Manager

Kontrollera att du använder hello senaste versionen av Azure PowerShell. Mer information finns i [Använda Windows PowerShell med Resource Manager](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Steg 1

Logga in tooAzure

```powershell
Login-AzureRmAccount
```

Du kan ange tooauthenticate med dina autentiseringsuppgifter.<BR>

### <a name="step-2"></a>Steg 2

Kontrollera hello prenumerationer för hello-kontot.

```powershell
Get-AzureRmSubscription
```

### <a name="step-3"></a>Steg 3

Välj vilka av dina Azure-prenumerationer toouse. <BR>

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="step-4"></a>Steg 4

Skapa en resursgrupp (hoppa över detta steg om du använder en befintlig resursgrupp).

```powershell
$resourceGroup = New-AzureRmResourceGroup -Name appgw-RG -Location "West US"
```

Alternativt kan du också skapa taggar för en resursgrupp för Programgateway:

```powershell
$resourceGroup = New-AzureRmResourceGroup -Name appgw-RG -Location "West US" -Tags @{Name = "testtag"; Value = "Application Gateway URL routing"} 
```

Azure Resource Manager kräver att alla resursgrupper anger en plats. Den här resursgruppen används som hello standardplatsen för resurser i resursgruppen. Se till att alla kommandon toocreate ett program gateway används hello samma resursgrupp.

I hello-exemplet ovan kan skapat vi en resursgrupp med namnet ”appgw RG” och plats ”West US”.

> [!NOTE]
> Om du behöver tooconfigure en anpassad avsökningsåtgärd för din Programgateway finns [skapa en Programgateway med anpassade avsökningar med hjälp av PowerShell](application-gateway-create-probe-ps.md). Mer information finns i [Anpassade avsökningar och hälsoövervakning](application-gateway-probe-overview.md).
> 
> 

## <a name="create-a-virtual-network-and-a-subnet-for-hello-application-gateway"></a>Skapa ett virtuellt nätverk och ett undernät för hello Programgateway

följande exempel visar hur hello toocreate ett virtuellt nätverk med hjälp av hanteraren för filserverresurser. Det här exemplet skapar ett virtuellt nätverk för hello Application Gateway. Programgateway kräver sin egen undernät därför hello undernät som skapats för hello Programgateway är mindre än hello VNET-adressutrymmet. Detta ger andra resurser, inklusive men inte begränsat tooweb servrar toobe konfigurerad i hello samma virtuella nätverk.

### <a name="step-1"></a>Steg 1

Tilldela hello adressintervallet 10.0.0.0/24 toohello undernät variabeln toobe används toocreate ett virtuellt nätverk.  Detta skapar hello configuration undernätsobjekt för hello Programgateway som används i hello nästa exempel.

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
```

### <a name="step-2"></a>Steg 2

Skapa ett virtuellt nätverk med namnet **appgwvnet** i resursgruppen **appgw rg** för hello västra USA region med undernätet 10.0.0.0/24 hello prefixet 10.0.0.0/16. Detta avslutar hello konfigurationen av hello virtuellt nätverk med ett enda undernät för hello Programgateway tooreside.

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet
```

### <a name="step-3"></a>Steg 3

Tilldela hello undernät variabel för hello nästa steg, detta skickas toohello `New-AzureRMApplicationGateway` cmdlet i ett kommande steg.

```powershell
$subnet=$vnet.Subnets[0]
```

## <a name="create-a-public-ip-address-for-hello-front-end-configuration"></a>Skapa en offentlig IP-adress för hello frontend-konfiguration

Skapa en offentlig IP-resurs **publicIP01** i resursgruppen **appgw rg** för hello västra USA region. Programgateway kan använda en offentlig IP-adress, interna IP-adress eller båda tooreceive begäranden för belastningsutjämning.  I det här exemplet används endast en offentlig IP-adress. I följande exempel hello, konfigureras inget DNS-namn för att skapa hello offentlig IP-adress.  Application Gateway stöder inte anpassade DNS-namn med offentliga IP-adresser.  Om ett anpassat namn krävs för hello offentlig slutpunkt, skapas en CNAME-post toopoint toohello genereras automatiskt DNS-namn för hello offentlig IP-adress.

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

En IP-adress tilldelas toohello Programgateway när hello-tjänsten startas.

## <a name="create-application-gateway-configuration"></a>Skapa program gateway-konfiguration

Alla konfigurationsobjekt måste ställas in innan du skapar hello Programgateway. hello med följande steg skapar hello konfigurationsobjekt som behövs för en gateway programresursen.

### <a name="step-1"></a>Steg 1

Skapa en IP-konfiguration för programgatewayen med namnet **gatewayIP01**. När Programgateway startar hämtar en IP-adress från hello-undernät som konfigurerats och dirigera trafik toohello IP-adresser på nätverket i hello backend-IP-adresspool. Tänk på att varje instans använder en IP-adress.

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
```

### <a name="step-2"></a>Steg 2

Konfigurera hello backend-IP-adresspool med namnet **pool01** och **pool2** med IP-adresser för **pool1** och **pool2**. Dessa IP-adresser är IP-adresser för hello hello-resurser som är värd för hello web application toobe skyddas av hello Programgateway. Dessa backend poolmedlemmar alla verifierade toobe som felfri av avsökningar oavsett om de är grundläggande avsökningar eller anpassade avsökningar.  Trafik dirigeras sedan toothem när begäranden som kommer till hello Programgateway. Serverdelspooler kan användas av flera regler inom hello Programgateway, vilket innebär en serverdelspool kan användas i flera webbprogram som finns på hello samma värd.

```powershell
$pool1 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221, 134.170.185.50

$pool2 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool02 -BackendIPAddresses 134.170.186.47, 134.170.189.222, 134.170.186.51
```

I det här exemplet finns det två backend-pooler tooroute nätverkstrafik som baserat på hello URL-sökväg. En pool tar emot trafik från URL-sökvägen ”/video” och en annan pool tar emot trafik från sökvägen ”/image”. Ersätt hello föregående IP-adresser tooadd egna programslutpunkter IP-adress. 

### <a name="step-3"></a>Steg 3

Konfigurera gateway programinställning **poolsetting01** och **poolsetting02** för hello belastningsutjämnad nätverkstrafik i hello backend-adresspool. I det här exemplet kan du konfigurera inställningarna för olika backend-pool för hello backend-pooler. Varje serverdelspool kan ha sin egen serverdelspoolinställning.  Serverdelens HTTP-inställningar som används av regler tooroute trafik toohello rätt backend poolmedlemmar. Detta avgör hello protokoll och port som används när en skickar trafik toohello backend poolmedlemmar. Cookie-baserad sessioner också bestäms av hello serverdelens HTTP-inställningar.  Om aktiverad, cookie-baserad session tillhörighet skickar trafik toohello samma backend som tidigare begäranden för varje paket.

```powershell
$poolSetting01 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120

$poolSetting02 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting02" -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 240
```

### <a name="step-4"></a>Steg 4

Konfigurera IP-frontend-hello med offentliga IP-slutpunkt. hello frontend IP configuration-objekt används av en lyssnare toorelate hello passiv mot IP-adress med hello-lyssnaren.

```powershell
$fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-5"></a>Steg 5

Konfigurera hello frontend-port för en Programgateway. konfigurationsobjekt för hello frontend-port används av en lyssnare toodefine vad port hello Programgateway lyssnar efter trafik på hello lyssnare.

```powershell
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "fep01" -Port 80
```

### <a name="step-6"></a>Steg 6

Konfigurera hello-lyssnaren. Det här steget konfigurerar hello-lyssnare för hello offentlig IP-adress och port används tooreceive inkommande nätverkstrafik. hello följande exempel tar hello som tidigare har konfigurerat frontend IP-konfiguration, frontend portkonfiguration och ett protokoll (http eller https) och konfigurerar hello-lyssnaren. I det här exemplet lyssnar hello lyssnare tooHTTP trafik på port 80 på hello offentlig IP-adress som du skapade tidigare.

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol Http -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01
```

### <a name="step-7"></a>Steg 7

Konfigurera URL: en regel sökvägar för hello backend-pooler. Det här steget konfigurerar hello relativ sökväg används av Programgateway och definierar hello mappning mellan hello URL-sökväg och hello backend-adresspool som är tilldelad toohandle hello inkommande trafik.

> [!IMPORTANT]
> Varje sökväg måste börja med / och hello endast placera en ”\*” tillåts, är hello slutet. Giltiga exempel är /xyz, /xyz* eller/xyz / *. hello strängen matas toohello sökväg matcher inte innehåller någon text efter hello först ””? eller ”#”, och dessa tecken är inte tillåtna. 

hello följande exempel skapar två regler: en för ”/ bild /” sökväg routning trafik tooback slutpunkt ”pool1” och en annan för ”/ video /” sökväg routning trafik tooback slutpunkt ”pool2”. Dessa regler kan du kontrollera att trafik för varje uppsättning URL: er är routade toohello backend. Till exempel http://contoso.com/image/figure1.jpg hamnar toopool1 och http://contoso.com/video/example.mp4 går toopool2.

```powershell
$imagePathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "pathrule1" -Paths "/image/*" -BackendAddressPool $pool1 -BackendHttpSettings $poolSetting01

$videoPathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "pathrule2" -Paths "/video/*" -BackendAddressPool $pool2 -BackendHttpSettings $poolSetting02
```

Om hello sökväg inte matchar någon av hello fördefinierade sökvägsregler, konfigurerar hello regelkonfigurationen sökväg kartan även en standard backend-adresspool. Till exempel blir http://contoso.com/shoppingcart/test.html toopool1 som har definierats som hello Standardpool för omatchade trafik.

```powershell
$urlPathMap = New-AzureRmApplicationGatewayUrlPathMapConfig -Name "urlpathmap" -PathRules $videoPathRule, $imagePathRule -DefaultBackendAddressPool $pool1 -DefaultBackendHttpSettings $poolSetting02
```

### <a name="step-8"></a>Steg 8

Skapa en regel inställning. Det här steget konfigurerar hello programmet gateway toouse URL-sökväg-baserad routning. Hej `$urlPathMap` variabel som anges i hello tidigare steget är nu används toocreate hello sökväg-baserade regler. I det här steget associera vi hello regel med en lyssnare och hello url sökvägsmappning skapade tidigare.

```powershell
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule1" -RuleType PathBasedRouting -HttpListener $listener -UrlPathMap $urlPathMap
```

### <a name="step-9"></a>Steg 9

Konfigurera hello antal förekomster och storleken för hello Programgateway.

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name "Standard_Small" -Tier Standard -Capacity 2
```

## <a name="create-application-gateway"></a>Skapa Programgateway

Skapa en Programgateway med alla konfigurationsobjekt från hello föregående steg.

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-RG -Location "West US" -BackendAddressPools $pool1,$pool2 -BackendHttpSettingsCollection $poolSetting01, $poolSetting02 -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener -UrlPathMaps $urlPathMap -RequestRoutingRules $rule01 -Sku $sku
```

## <a name="get-application-gateway-dns-name"></a>Hämta DNS-namn för programgatewayen

När du har skapat hello gateway är hello nästa steg tooconfigure hello klientdelen för kommunikation. När du använder en offentlig IP-adress krävs ett dynamiskt tilldelat DNS-namn som inte är användarvänligt. tooensure slutanvändare kan träffa hello Programgateway, en CNAME-post kan vara används toopoint toohello offentlig slutpunkt för hello Programgateway. [Konfigurera ett eget domännamn i Azure](../cloud-services/cloud-services-custom-domain-name-portal.md). tooconfigure hello klientdelens IP-CNAME-post, hämta information om hello Programgateway och dess associerade IP DNS-namn med hjälp av hello PublicIPAddress element bifogade toohello Programgateway. hello programmet gateway DNS-namn ska använda toocreate en CNAME-post. hello rekommenderas A-poster inte eftersom hello VIP ändras vid omstart för Programgateway.

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -Name publicIP01
```

```
Name                     : publicIP01
ResourceGroupName        : appgw-RG
Location                 : westus
Id                       : /subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/publicIPAddresses/publicIP01
Etag                     : W/"00000d5b-54ed-4907-bae8-99bd5766d0e5"
ResourceGuid             : 00000000-0000-0000-0000-000000000000
ProvisioningState        : Succeeded
Tags                     : 
PublicIpAllocationMethod : Dynamic
IpAddress                : xx.xx.xxx.xx
PublicIpAddressVersion   : IPv4
IdleTimeoutInMinutes     : 4
IpConfiguration          : {
                                "Id": "/subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/applicationGateways/appgwtest/frontendIP
                            Configurations/frontend1"
                            }
DnsSettings              : {
                                "Fqdn": "00000000-0000-xxxx-xxxx-xxxxxxxxxxxx.cloudapp.net"
                            }
```

## <a name="next-steps"></a>Nästa steg

Om du vill toolearn Secure Sockets Layer (SSL)-avlastning, se [konfigurera en Programgateway för SSL-avlastning](application-gateway-ssl-arm.md).

