---
title: "Skapa en Programgateway med hjälp av URL: en routningsregler | Microsoft Docs"
description: "Den här sidan innehåller instruktioner för att skapa, konfigurera en gateway för Azure-program som använder routningsregler för URL"
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
ms.openlocfilehash: ba756d3262b9780c5701e69faad860ba32bba08b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="create-an-application-gateway-using-path-based-routing"></a>Skapa en Programgateway använder sökväg-baserade Routning

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-url-route-portal.md)
> * [PowerShell och Azure Resource Manager](application-gateway-create-url-route-arm-ps.md)
> * [Azure CLI 2.0](application-gateway-create-url-route-cli.md)

URL-sökväg-baserad routning kan du associera rutter baserat på en URL-sökväg för en Http-begäran. Den kontrollerar om det finns en väg till en backend-adresspool som konfigurerats för den URL som visas i Programgatewayen. Sedan skickar den nätverkstrafiken till definierade backend-poolen. Ett vanligt användningsområde för URL-baserade routning är att belastningsutjämna förfrågningar för olika typer av innehåll till olika backend-serverpooler.

URL-baserade routning introducerar en ny regeltyp av i Programgateway. Programgateway har två regeltyper: basic och PathBasedRouting. Grundläggande regeltyp resursallokering tjänst för backend-pooler när PathBasedRouting förutom resursallokering (round robin), också beaktas sökvägar för den begärda Webbadressen när du väljer serverdelspoolen.

## <a name="scenario"></a>Scenario

I följande exempel Programgateway betjäna trafik för contoso.com med två backend-server-adresspooler: video-serverpoolen och image-serverpoolen.

Begäranden för http://contoso.com/image * dirigeras till bilden serverpoolen (pool1) och http://contoso.com/video * dirigeras till video serverpoolen (pool2). Om inget av mönster som sökväg matchar väljs en standard serverpool (pool1).

![URL-väg](./media/application-gateway-create-url-route-arm-ps/figure1.png)

## <a name="before-you-begin"></a>Innan du börjar

1. Installera den senaste versionen av Azure PowerShell-cmdlets med hjälp av installationsprogrammet för webbplattform. Du kan hämta och installera den senaste versionen från avsnittet om **Windows PowerShell** på [hämtningssidan](https://azure.microsoft.com/downloads/).
2. Du skapar ett virtuellt nätverk och undernät för Application Gateway. Kontrollera att inga virtuella datorer eller molndistributioner använder undernätet. Programgatewayen måste vara fristående i ett virtuellt nätverks undernät.
3. Servrar som läggs till backend-poolen som ska användas programgatewayen måste finnas eller har skapat sina slutpunkter i det virtuella nätverket eller med en offentlig IP-adress/VIP tilldelad.

## <a name="what-is-required-to-create-an-application-gateway"></a>Vad krävs för att skapa en programgateway?

* **Backend-serverpool:** Listan med IP-adresser för backend-servrarna. IP-adresserna som anges bör antingen tillhöra det virtuella undernätet eller vara en offentlig IP-/VIP-adress.
* **Inställningar för backend-serverpool:** Varje pool har inställningar som port, protokoll och cookiebaserad tillhörighet. Dessa inställningar är knutna till en pool och tillämpas på alla servrar i poolen.
* **Frontend-port:** Den här porten är den offentliga porten som är öppen på programgatewayen. Trafiken kommer till den här porten och omdirigeras till en av backend-servrarna.
* **Lyssnare:** Lyssnaren har en frontend-port, ett protokoll (Http eller Https; dessa värden är skiftlägeskänsliga) och SSL-certifikatnamnet (om du konfigurerar SSL-avlastning).
* **Regel:** Regeln binder lyssnaren och backend-serverpoolen och definierar vilken backend-serverpool som trafiken ska dirigeras till när den når en viss lyssnare.

## <a name="create-an-application-gateway"></a>Skapa en programgateway

Skillnaden mellan att använda den klassiska Azure-portalen och Azure Resource Manager är i vilken ordning du skapar programgatewayen och de objekt som ska konfigureras.

Med Resource Manager konfigureras alla objekt som bildar en programgateway separat och sätts sedan ihop för att skapa en programgatewayresurs.

Här följer de steg som krävs för att skapa en programgateway:

1. Skapa en resursgrupp för Resource Manager.
2. Skapa ett virtuellt nätverk, ett undernät och en offentlig IP för programgatewayen.
3. Skapa ett konfigurationsobjekt för programgatewayen.
4. Skapa en resurs för en programgateway.

## <a name="create-a-resource-group-for-resource-manager"></a>Skapa en resursgrupp för Resource Manager

Kontrollera att du använder den senaste versionen av Azure PowerShell. Mer information finns i [Använda Windows PowerShell med Resource Manager](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Steg 1

Logga in på Azure

```powershell
Login-AzureRmAccount
```

Du ombeds att autentisera dig med dina autentiseringsuppgifter.<BR>

### <a name="step-2"></a>Steg 2

Kontrollera prenumerationerna för kontot.

```powershell
Get-AzureRmSubscription
```

### <a name="step-3"></a>Steg 3

Välj vilka av dina Azure-prenumerationer som du vill använda. <BR>

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

Azure Resource Manager kräver att alla resursgrupper anger en plats. Den här resursgruppen används som standard för resurser i resursgruppen. Se till att alla kommandon för att skapa en Programgateway använder samma resursgrupp.

I exemplet ovan skapade vi resursgruppen ”appgw-RG” och platsen ”West US”.

> [!NOTE]
> Om du behöver konfigurera en anpassad avsökning för din programgateway läser du [Skapa en programgateway med anpassade avsökningar med hjälp av PowerShell](application-gateway-create-probe-ps.md). Mer information finns i [Anpassade avsökningar och hälsoövervakning](application-gateway-probe-overview.md).
> 
> 

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Skapa ett virtuellt nätverk och ett undernät för programgatewayen

Följande exempel illustrerar hur du skapar ett virtuellt nätverk med hjälp av Resource Manager. Det här exemplet skapar ett virtuellt nätverk för Application Gateway. Programgateway kräver sin egen undernät därför undernät som skapats för Programgateway är mindre än VNET-adressutrymmet. Detta ger andra resurser, inklusive men inte begränsat till webbservrar som ska konfigureras i samma virtuella nätverk.

### <a name="step-1"></a>Steg 1

Tilldela adressintervallet 10.0.0.0/24 till undernätsvariabeln som ska användas för att skapa ett virtuellt nätverk.  Detta skapar undernät konfigurationsobjektet för Programgateway som används i nästa exempel.

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
```

### <a name="step-2"></a>Steg 2

Skapa ett virtuellt nätverk med namnet **appgwvnet** i resursgruppen **appgw-rg** för regionen USA, västra med prefixet 10.0.0.0/16 och undernätet 10.0.0.0/24. Nu är du klar med konfigurationen av virtuellt nätverk med ett enda undernät för Programgateway finns.

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet
```

### <a name="step-3"></a>Steg 3

Tilldela variabeln undernät i nästa steg, detta skickas till den `New-AzureRMApplicationGateway` cmdlet i ett kommande steg.

```powershell
$subnet=$vnet.Subnets[0]
```

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Skapa en offentlig IP-adress för frontend-konfigurationen

Skapa en offentlig IP-resurs, **publicIP01**, i resursgruppen **appgw-rg** för regionen USA, västra. Application Gateway kan använda en offentlig IP-adress, en intern IP-adress eller båda för att ta emot förfrågningar för belastningsutjämning.  I det här exemplet används endast en offentlig IP-adress. I följande exempel konfigureras inget DNS-namn när den offentliga IP-adressen skapas.  Application Gateway stöder inte anpassade DNS-namn med offentliga IP-adresser.  Om ett anpassat namn krävs för den offentliga slutpunkten bör en CNAME-post skapas som pekar på det automatiskt genererade DNS-namnet för den offentliga IP-adressen.

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

En IP-adress tilldelas till programgatewayen när tjänsten startas.

## <a name="create-application-gateway-configuration"></a>Skapa program gateway-konfiguration

Alla konfigurationsobjekt måste konfigureras innan programgatewayen skapas. Följande steg skapar konfigurationsobjekten som behövs för en programgatewayresurs.

### <a name="step-1"></a>Steg 1

Skapa en IP-konfiguration för programgatewayen med namnet **gatewayIP01**. När Application Gateway startar hämtar den en IP-adress från det konfigurerade undernätet och dirigerar nätverkstrafik till IP-adresserna i backend-IP-poolen. Tänk på att varje instans använder en IP-adress.

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
```

### <a name="step-2"></a>Steg 2

Konfigurera backend-IP-adresspool med namnet **pool01** och **pool2** med IP-adresser för **pool1** och **pool2**. Dessa IP-adresser är IP-adresserna för de resurser som är värdar för webbprogrammet som ska skyddas av programgatewayen. Hälsotillståndet för dessa medlemmar i serverdelspoolen verifieras genom grundläggande eller anpassade avsökningar.  Sedan dirigeras trafiken till dem när begäranden kommer till programgatewayen. Serverdelspooler kan användas av flera regler i programgatewayen, vilket innebär att en serverdelspool kan användas för flera webbprogram som finns på samma värd.

```powershell
$pool1 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221, 134.170.185.50

$pool2 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool02 -BackendIPAddresses 134.170.186.47, 134.170.189.222, 134.170.186.51
```

I det här exemplet finns det två serverdelspooler för dirigering av nätverkstrafik baserat på URL-sökvägen. En pool tar emot trafik från URL-sökvägen ”/video” och en annan pool tar emot trafik från sökvägen ”/image”. Ersätt IP-adresserna med IP-adresslutpunkterna för dina egna program. 

### <a name="step-3"></a>Steg 3

Konfigurera gateway programinställning **poolsetting01** och **poolsetting02** för nätverkstrafik Utjämning av nätverksbelastning i backend-poolen. I det här exemplet kan du konfigurera inställningarna för olika backend-pool för backend-pooler. Varje serverdelspool kan ha sin egen serverdelspoolinställning.  Serverdelens HTTP-inställningar används av regler för att dirigera trafik till rätt medlemmar i serverdelspoolen. Anger protokoll och port som används när du skickar trafik till backend-poolmedlemmar. Cookie-baserade sessioner bestäms också av serverdelens HTTP-inställningar.  Om cookie-baserad sessionstillhörighet är aktiverat skickas trafiken till samma serverdel som tidigare begäranden för varje paket.

```powershell
$poolSetting01 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120

$poolSetting02 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting02" -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 240
```

### <a name="step-4"></a>Steg 4

Konfigurera klientdelens IP med den offentliga IP-slutpunkten. Konfigurationsobjektet för klientdelens IP används av en lyssnare för att relatera den utåtgående IP-adressen med lyssnaren.

```powershell
$fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-5"></a>Steg 5

Konfigurera klientdelsporten för en programgateway. Konfigurationsobjektet för klientdelsporten används av en lyssnare för att definiera vilken port som Application Gateway lyssnar efter trafik på.

```powershell
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "fep01" -Port 80
```

### <a name="step-6"></a>Steg 6

Konfigurera lyssnaren. Det här steget konfigurerar lyssnaren för den offentliga IP-adressen och porten som används för att ta emot inkommande nätverkstrafik. I följande exempel tar tidigare konfigurerade frontend IP-konfigurationen och frontend portkonfiguration ett protokoll (http eller https) och konfigurerar lyssnaren. I det här exemplet lyssnar lyssnaren efter HTTP-trafik på port 80 på den offentliga IP-adressen som skapades tidigare.

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol Http -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01
```

### <a name="step-7"></a>Steg 7

Konfigurera URL: en regel sökvägar för backend-pooler. Det här steget konfigurerar du den relativa sökvägen som används av Programgateway och definierar mappningen mellan en URL-sökväg och backend-poolen som har tilldelats för den inkommande trafiken.

> [!IMPORTANT]
> Varje sökväg måste börja med / och endast en ”\*” är tillåtna, finns i slutet. Giltiga exempel är /xyz, /xyz* eller/xyz / *. Strängen som skickas till sökvägen matcher innehåller inte någon text efter först ””? eller ”#”, och dessa tecken är inte tillåtna. 

I följande exempel skapas två regler: en för ”/ bild /” sökväg routning trafik till backend-”pool1” och en annan för ”/ video /” sökväg dirigera trafiken till backend-”pool2”. De här reglerna Kontrollera att trafik för varje uppsättning URL-adresser dirigeras till serverdelen. Till exempel http://contoso.com/image/figure1.jpg går till pool1 och http://contoso.com/video/example.mp4 går till pool2.

```powershell
$imagePathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "pathrule1" -Paths "/image/*" -BackendAddressPool $pool1 -BackendHttpSettings $poolSetting01

$videoPathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "pathrule2" -Paths "/video/*" -BackendAddressPool $pool2 -BackendHttpSettings $poolSetting02
```

Om sökvägen inte matchar någon av de fördefinierade sökvägsreglerna, konfigurerar regelkonfigurationen sökväg kartan även en standard backend-adresspool. Till exempel går http://contoso.com/shoppingcart/test.html till pool1 som har definierats som standard för omatchade trafik.

```powershell
$urlPathMap = New-AzureRmApplicationGatewayUrlPathMapConfig -Name "urlpathmap" -PathRules $videoPathRule, $imagePathRule -DefaultBackendAddressPool $pool1 -DefaultBackendHttpSettings $poolSetting02
```

### <a name="step-8"></a>Steg 8

Skapa en regel inställning. Det här steget konfigurerar Programgateway för att använda URL-sökväg-baserade routning. Den `$urlPathMap` variabel som anges i tidigare steg nu användas för att skapa regeln baserat på sökvägen. Vi associera regeln med en lyssnare och URL: en sökvägsmappning skapade tidigare i det här steget.

```powershell
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule1" -RuleType PathBasedRouting -HttpListener $listener -UrlPathMap $urlPathMap
```

### <a name="step-9"></a>Steg 9

Konfigurera antalet instanser av och storleken på programgatewayen.

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name "Standard_Small" -Tier Standard -Capacity 2
```

## <a name="create-application-gateway"></a>Skapa Programgateway

Skapa en Programgateway med alla konfigurationsobjekt från föregående steg.

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-RG -Location "West US" -BackendAddressPools $pool1,$pool2 -BackendHttpSettingsCollection $poolSetting01, $poolSetting02 -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener -UrlPathMaps $urlPathMap -RequestRoutingRules $rule01 -Sku $sku
```

## <a name="get-application-gateway-dns-name"></a>Hämta DNS-namn för programgatewayen

När du har skapat gatewayen, är nästa steg att konfigurera klientprogrammet för kommunikation. När du använder en offentlig IP-adress krävs ett dynamiskt tilldelat DNS-namn som inte är användarvänligt. För att säkerställa att slutanvändare kan nå programgatewayen kan en CNAME-post användas för att peka på den offentliga slutpunkten för programgatewayen. [Konfigurera ett eget domännamn i Azure](../cloud-services/cloud-services-custom-domain-name-portal.md). Hämta information om programgatewayen och dess tillhörande IP DNS-namn med PublicIPAddress-element som bifogas programgatewayen om du vill konfigurera klientdelens IP-CNAME-post. Den Programgateway DNS-namn ska användas för att skapa en CNAME-post. Användning av A-poster rekommenderas inte eftersom VIP kan ändras vid omstart av programgatewayen.

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

Om du vill veta Secure Sockets Layer (SSL)-avlastning, se [konfigurera en Programgateway för SSL-avlastning](application-gateway-ssl-arm.md).

