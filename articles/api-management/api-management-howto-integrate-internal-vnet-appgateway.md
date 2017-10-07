---
title: "aaaHow toouse Azure API Management i det virtuella nätverket med Programgateway | Microsoft Docs"
description: "Lär dig hur toosetup och konfigurera Azure API Management i internt virtuellt nätverk med programmet Gateway (Brandvägg) som klientdel"
services: api-management
documentationcenter: 
author: solankisamir
manager: kjoshi
editor: antonba
ms.assetid: a8c982b2-bca5-4312-9367-4a0bbc1082b1
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2017
ms.author: sasolank
ms.openlocfilehash: 74303a2ee8a10db633ab1740ec7267728eacb473
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-api-management-in-an-internal-vnet-with-application-gateway"></a>Integrera API Management i ett internt virtuellt nätverk med Programgateway 

##<a name="overview"></a> Översikt
 
hello API Management-tjänsten kan konfigureras i ett virtuellt nätverk i internt läge vilket gör att det är enbart tillgänglig från hello virtuellt nätverk. Azure Application Gateway är en PAAS-tjänst som tillhandahåller en lager 7 belastningsutjämnare. Den fungerar som en omvänd proxy-tjänst och innehåller bland erbjudande Web Application Firewall (Brandvägg).

Kombinera API Management etablerad på ett internt virtuellt nätverk med hello Programgateway klientdel kan hello följande scenarier:

* Använd hello samma API Management-resurs för förbrukning av både konsumenter interna och externa användare.
* Använda en enskild resurs för API Management och har en delmängd av API: er som definierats i API-hantering som är tillgänglig för externa användare.
* Ange en nyckelfärdig sätt tooswitch åtkomst tooAPI Management från hello offentliga Internet på och stänga av. 

##<a name="scenario"></a> Scenario
Den här artikeln kommer upp hur toouse ett enda API Management-tjänsten för både interna och externa användare och gör det fungerar som en enda klientdel för både lokala och molntjänster API: er. Du kan även se hur tooexpose bara en del av din API: er (i hello exempel markeringen i grönt) för extern användning med hello PathBasedRouting tillgängliga funktioner i Application Gateway.

I hello första installationen exemplet hanteras alla API: er endast från i ditt virtuella nätverk. Interna användare (markerade i orange) kan komma åt alla dina interna och externa API: er. Trafik som aldrig går ut tooInternet en hög prestanda levereras via Expressroute-kretsar.

![URL-väg](./media/api-management-howto-integrate-internal-vnet-appgateway/api-management-howto-integrate-internal-vnet-appgateway.png)

## <a name="before-you-begin"></a> Innan du börjar

1. Installera hello senaste versionen av hello Azure PowerShell-cmdlets med hello installationsprogram för webbplattform. Du kan hämta och installera hello senaste versionen från hello **Windows PowerShell** avsnitt i hello [Nedladdningssida](https://azure.microsoft.com/downloads/).
2. Skapa ett virtuellt nätverk och skapa separata undernät för API Management och Application Gateway. 
3. Om du planerar toocreate en anpassad DNS-server för hello virtuellt nätverk, kan du göra det innan du börjar hello distribueringen. Kontrollera fungerar genom att säkerställa att en virtuell dator som skapats i ett nytt undernät i hello virtuellt nätverk kan lösa och komma åt alla Azure-Tjänsteslutpunkter.

## <a name="what-is-required-toocreate-an-integration-between-api-management-and-application-gateway"></a>Vad är obligatoriska toocreate integration mellan API Management och Programgateway?

* **Backend-serverpoolen:** hello interna virtuella IP-adress för hello API Management-tjänsten.
* **Inställningar för backend-serverpool:** Varje pool har inställningar som port, protokoll och cookiebaserad tillhörighet. Dessa inställningar är tillämpade tooall servrar inom hello poolen.
* **Frontend-port:** hello offentliga porten som öppnas på hello Programgateway. Trafik träffa den hämtar omdirigerade tooone av hello backend-servrar.
* **Lyssnare:** hello-lyssnare har en frontend-port, ett protokoll (Http eller Https dessa värden är skiftlägeskänsligt), och hello SSL-certifikatnamn (om hur du konfigurerar SSL-avlastning).
* **Regel:** hello regeln Binder poolen lyssnare tooa backend-server.
* **Anpassade Hälsoavsökningen:** Application Gateway som standard använder IP-adress baserat avsökningar toofigure reda på vilka servrar i hello BackendAddressPool är aktiva. hello API Management-tjänsten svarar bara toorequests som har hello rätt värdhuvud, därför hello standard avsökningar misslyckas. En anpassad hälsoavsökningen måste toobe definierats toohelp Programgateway bestämma att hello-tjänsten är igång och att den ska vidarebefordra begäranden.
* **Certifikatet för domänen:** tooaccess API-hantering från hello internet måste toocreate en CNAME-mappning av ett värdnamn toohello Programgateway frontend DNS-namn. Detta garanterar att hello hostname sidhuvud och certifikat som skickats tooApplication Gateway som vidarebefordras tooAPI Management är en APIM kan identifiera som giltig.

## <a name="overview-steps"></a> Steg som krävs för att integrera API Management och Programgateway 

1. Skapa en resursgrupp för Resource Manager.
2. Skapa ett virtuellt nätverk och undernät offentlig IP för hello Application Gateway. Skapa ett annat undernät för API-hantering.
3. Skapa en API Management-tjänst i hello VNET subnet skapade ovan och se till att du använder intern hello-läget.
4. Konfigurera hello domännamn i hello API Management-tjänsten.
5. Skapa ett konfigurationsobjekt för Application Gateway.
6. Skapa en Programgateway resurs.
7. Skapa en CNAME-post från hello offentliga DNS-namnet på hello Programgateway toohello API Management proxy värdnamn.

## <a name="create-a-resource-group-for-resource-manager"></a>Skapa en resursgrupp för Resource Manager

Kontrollera att du använder hello senaste versionen av Azure PowerShell. Mer information finns i [Använda Windows PowerShell med Resource Manager](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Steg 1

Logga in tooAzure

```powershell
Login-AzureRmAccount
```

Autentisera med dina autentiseringsuppgifter.<BR>

### <a name="step-2"></a>Steg 2

Kontrollera hello prenumerationer för hello-kontot och markera den.

```powershell
Get-AzureRmSubscription -Subscriptionid "GUID of subscription" | Select-AzureRmSubscription
```

### <a name="step-3"></a>Steg 3

Skapa en resursgrupp (hoppa över detta steg om du använder en befintlig resursgrupp).

```powershell
New-AzureRmResourceGroup -Name "apim-appGw-RG" -Location "West US"
```
Azure Resource Manager kräver att alla resursgrupper anger en plats. Detta används som hello standardplatsen för resurser i resursgruppen. Se till att alla kommandon toocreate ett program gateway används hello samma resursgrupp.

## <a name="create-a-virtual-network-and-a-subnet-for-hello-application-gateway"></a>Skapa ett virtuellt nätverk och ett undernät för hello Programgateway

hello som följande exempel visar hur hello toocreate ett virtuellt nätverk med hjälp av hanteraren för filserverresurser.

### <a name="step-1"></a>Steg 1

Tilldela hello adressintervallet 10.0.0.0/24 toohello undernät variabeln toobe användas för Programgateway när du skapar ett virtuellt nätverk.

```powershell
$appgatewaysubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "apim01" -AddressPrefix "10.0.0.0/24"
```

### <a name="step-2"></a>Steg 2

Tilldela hello adressintervallet 10.0.1.0/24 toohello undernät variabeln toobe används för API Management när du skapar ett virtuellt nätverk.

```powershell
$apimsubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "apim02" -AddressPrefix "10.0.1.0/24"
```

### <a name="step-3"></a>Steg 3

Skapa ett virtuellt nätverk med namnet **appgwvnet** i resursgruppen **apim-appGw-RG** för hello västra USA region med hello prefixet 10.0.0.0/16 med undernät 10.0.0.0/24 och 10.0.1.0/24.

```powershell
$vnet = New-AzureRmVirtualNetwork -Name "appgwvnet" -ResourceGroupName "apim-appGw-RG" -Location "West US" -AddressPrefix "10.0.0.0/16" -Subnet $appgatewaysubnet,$apimsubnet
```

### <a name="step-4"></a>Steg 4

Tilldela en variabel för undernätet för hello nästa steg

```powershell
$appgatewaysubnetdata=$vnet.Subnets[0]
$apimsubnetdata=$vnet.Subnets[1]
```
## <a name="create-an-api-management-service-inside-a-vnet-configured-in-internal-mode"></a>Skapa en API Management-tjänst i ett virtuellt nätverk som konfigurerats i internt läge

hello följande exempel visas hur toocreate en API Management-tjänsten i ett VNET konfigurerats för intern åtkomst.

### <a name="step-1"></a>Steg 1
Skapa ett virtuellt nätverk för API Management-objekt med hello undernät $apimsubnetdata skapade ovan.

```powershell
$apimVirtualNetwork = New-AzureRmApiManagementVirtualNetwork -Location "West US" -SubnetResourceId $apimsubnetdata.Id
```
### <a name="step-2"></a>Steg 2
Skapa en API Management service i hello virtuellt nätverk.

```powershell
$apimService = New-AzureRmApiManagement -ResourceGroupName "apim-appGw-RG" -Location "West US" -Name "ContosoApi" -Organization "Contoso" -AdminEmail "admin@contoso.com" -VirtualNetwork $apimVirtualNetwork -VpnType "Internal" -Sku "Developer"
```
När hello ovanstående kommando lyckas Läs för[DNS-konfiguration krävs tooaccess interna virtuella nätverk API Management-tjänsten](api-management-using-with-internal-vnet.md#apim-dns-configuration) tooaccess den.

## <a name="set-up-a-custom-domain-name-in-api-management"></a>Ställa in ett anpassat domännamn i API Management

### <a name="step-1"></a>Steg 1
Ladda upp hello certifikat med privat nyckel för hello domän. Det här exemplet blir `*.contoso.net`. 

```powershell
$certUploadResult = Import-AzureRmApiManagementHostnameCertificate -ResourceGroupName "apim-appGw-RG" -Name "ContosoApi" -HostnameType "Proxy" -PfxPath <full path too.pfx file> -PfxPassword <password for certificate file> -PassThru
```

### <a name="step-2"></a>Steg 2
När hello certifikat har överförts, skapa ett konfigurationsobjekt för värdnamn för hello proxy med ett värdnamn för `api.contoso.net`, enligt hello exempel certifikatet ger behörighet för hello `*.contoso.net` domän. 

```powershell
$proxyHostnameConfig = New-AzureRmApiManagementHostnameConfiguration -CertificateThumbprint $certUploadResult.Thumbprint -Hostname "api.contoso.net"
$result = Set-AzureRmApiManagementHostnames -Name "ContosoApi" -ResourceGroupName "apim-appGw-RG" -ProxyHostnameConfiguration $proxyHostnameConfig
```

## <a name="create-a-public-ip-address-for-hello-front-end-configuration"></a>Skapa en offentlig IP-adress för hello frontend-konfiguration

Skapa en offentlig IP-resurs **publicIP01** i resursgruppen **apim-appGw-RG** för hello västra USA region.

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName "apim-appGw-RG" -name "publicIP01" -location "West US" -AllocationMethod Dynamic
```

En IP-adress tilldelas toohello Programgateway när hello-tjänsten startas.

## <a name="create-application-gateway-configuration"></a>Skapa program gateway-konfiguration

Alla konfigurationsobjekt måste ställas in innan du skapar hello Programgateway. hello med följande steg skapar hello konfigurationsobjekt som behövs för en gateway programresursen.

### <a name="step-1"></a>Steg 1

Skapa en IP-konfiguration för programgatewayen med namnet **gatewayIP01**. När Programgateway startar hämtar en IP-adress från hello-undernät som konfigurerats och dirigera trafik toohello IP-adresser på nätverket i hello backend-IP-adresspool. Tänk på att varje instans använder en IP-adress.

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name "gatewayIP01" -Subnet $appgatewaysubnetdata
```

### <a name="step-2"></a>Steg 2

Konfigurera hello frontend IP-port för hello offentlig IP-slutpunkt. Den här porten är hello som användare ansluta till.

```powershell
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "port01"  -Port 443
```
### <a name="step-3"></a>Steg 3

Konfigurera IP-frontend-hello med offentliga IP-slutpunkt.

```powershell
$fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-4"></a>Steg 4

Konfigurera hello certifikat för hello Application Gateway används toodecrypt och kryptera hello-trafik som passerar genom.

```powershell
$cert = New-AzureRmApplicationGatewaySslCertificate -Name "cert01" -CertificateFile <full path too.pfx file> -Password <password for certificate file>
```

### <a name="step-5"></a>Steg 5

Skapa hello HTTP-lyssnare för hello Application Gateway. Tilldela hello frontend IP-konfiguration, port och tooit för ssl-certifikat.

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol "Https" -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -SslCertificate $cert
```

### <a name="step-6"></a>Steg 6

Skapa en anpassad avsökningsåtgärd toohello API Management-tjänsten `ContosoApi` proxy domän slutpunkt. hello sökvägen `/status-0123456789abcdef` är en standardslutpunkt för hälsotillstånd som finns på alla hello API Management-tjänster. Ange `api.contoso.net` som en anpassad avsökningsåtgärd värdnamn toosecure med SSL-certifikat.

> [!NOTE]
> Hej hostname `contosoapi.azure-api.net` konfigureras hello standard proxy värdnamn när en tjänst med namnet `contosoapi` skapas i offentliga Azure. 
> 

```powershell
$apimprobe = New-AzureRmApplicationGatewayProbeConfig -Name "apimproxyprobe" -Protocol "Https" -HostName "api.contoso.net" -Path "/status-0123456789abcdef" -Interval 30 -Timeout 120 -UnhealthyThreshold 8
```

### <a name="step-7"></a>Steg 7

Överför hello certifikatet toobe används på hello backend SSL-aktiverade resurser. Detta är hello samma certifikat som du angav i steg 4 ovan.

```powershell
$authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name "whitelistcert1" -CertificateFile <full path too.cer file>
```

### <a name="step-8"></a>Steg 8

Konfigurera inställningar för HTTP-serverdelen för hello Application Gateway. Detta innefattar att ställa in en timeout-gränsen för backend-begäran som de avbryts. Det här värdet skiljer sig från hello avsökningen timeout.

```powershell
$apimPoolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "apimPoolSetting" -Port 443 -Protocol "Https" -CookieBasedAffinity "Disabled" -Probe $apimprobe -AuthenticationCertificates $authcert -RequestTimeout 180
```

### <a name="step-9"></a>Steg 9

Konfigurera backend-IP-adresspool med namnet **apimbackend** med hello interna virtuella IP-adressen för hello API Management-tjänsten skapade ovan.

```powershell
$apimProxyBackendPool = New-AzureRmApplicationGatewayBackendAddressPool -Name "apimbackend" -BackendIPAddresses $apimService.StaticIPs[0]
```

### <a name="step-10"></a>Steg 10

Skapa inställningar för en dummy (obefintligt)-serverdel. Begäranden tooAPI sökvägar som vi inte vill tooexpose från API-hantering via Programgateway kommer uppnått denna backend och returnera 404.

Konfigurera HTTP-inställningar för hello dummy serverdel.

```powershell
$dummyBackendSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "dummySetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled
```

Konfigurera en dummy serverdel **dummyBackendPool**, som anger tooa FQDN-adressen **dummybackend.com**. Den här FQDN-adressen finns inte i hello virtuella nätverk.

```powershell
$dummyBackendPool = New-AzureRmApplicationGatewayBackendAddressPool -Name "dummyBackendPool" -BackendFqdns "dummybackend.com"
```

Skapa en regel som anger att hello Programgateway används som standard som pekar toohello obefintlig backend **dummybackend.com** i hello virtuellt nätverk.

```powershell
$dummyPathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "nonexistentapis" -Paths "/*" -BackendAddressPool $dummyBackendPool -BackendHttpSettings $dummyBackendSetting
```

### <a name="step-11"></a>Steg 11

Konfigurera URL: en regel sökvägar för hello backend-pooler. Detta gör att bara vissa av hello API: er från API-hantering för att vara exponeras toohello offentliga Markera. Till exempel om det finns `Echo API` (echo-), `Calculator API` (/calc/) o.s.v. Kontrollera endast `Echo API` tillgänglig från Internet). 

hello skapas följande exempel en enkel regel för hello ”echo-” sökväg routning trafik toohello serverdel ”apimProxyBackendPool”.

```powershell
$echoapiRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "externalapis" -Paths "/echo/*" -BackendAddressPool $apimProxyBackendPool -BackendHttpSettings $apimPoolSetting
```

Om hello sökväg inte matchar hello sökväg regler vi vill tooenable från API Management hello regeln kartan flervägskonfiguration konfigurerar också en standard backend-adresspool med namnet **dummyBackendPool**. Till exempel http://api.contoso.net/calc/ * går för**dummyBackendPool** som har definierats som hello Standardpool för icke matchade trafik.

```powershell
$urlPathMap = New-AzureRmApplicationGatewayUrlPathMapConfig -Name "urlpathmap" -PathRules $echoapiRule, $dummyPathRule -DefaultBackendAddressPool $dummyBackendPool -DefaultBackendHttpSettings $dummyBackendSetting
```

hello senare steg garanterar som endast begär för hello sökvägen ”/ echo” tillåts via hello Application Gateway. Begäranden tooother API: er som konfigurerats i API Management genereras 404-fel från Programgateway vid åtkomst från hello Internet. 

### <a name="step-12"></a>Steg 12

Skapa en regel inställning för hello Programgateway toouse URL-sökväg-baserad routning.

```powershell
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule1" -RuleType PathBasedRouting -HttpListener $listener -UrlPathMap $urlPathMap
```

### <a name="step-13"></a>Steg 13

Konfigurera hello antal förekomster och storleken för hello Application Gateway. Här använder vi hello [Brandvägg SKU](../application-gateway/application-gateway-webapplicationfirewall-overview.md) för ökad säkerhet för hello API Management-resurs.

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name "WAF_Medium" -Tier "WAF" -Capacity 2
```

### <a name="step-14"></a>Steg 14

Konfigurera Brandvägg toobe i ”förebyggande” läge.
```powershell
$config = New-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode "Prevention"
```

## <a name="create-application-gateway"></a>Skapa Programgateway

Skapa en Programgateway med alla hello-konfigurationsobjekt från hello föregående steg.

```powershell
$appgw = New-AzureRmApplicationGateway -Name $applicationGatewayName -ResourceGroupName $resourceGroupName  -Location $location -BackendAddressPools $apimProxyBackendPool, $dummyBackendPool -BackendHttpSettingsCollection $apimPoolSetting, $dummyBackendSetting  -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener -UrlPathMaps $urlPathMap -RequestRoutingRules $rule01 -Sku $sku -WebApplicationFirewallConfig $config -SslCertificates $cert -AuthenticationCertificates $authcert -Probes $apimprobe
```

## <a name="cname-hello-api-management-proxy-hostname-toohello-public-dns-name-of-hello-application-gateway-resource"></a>CNAME hello API Management proxy värdnamn toohello offentliga DNS-namnet på hello Programgateway resurs

När du har skapat hello gateway är hello nästa steg tooconfigure hello klientdelen för kommunikation. När du använder en offentlig IP-adress, kräver Programgateway en dynamiskt tilldelad DNS-namn som inte får enkelt toouse. 

hello Application Gateway DNS-namn ska använda toocreate en CNAME-post som pekar hello APIM proxyvärdnamn (t.ex. `api.contoso.net` i hello exemplen ovan) toothis DNS-namn. tooconfigure hello klientdelens IP-CNAME-post, hämta hello detaljer om hello Programgateway och dess tillhörande IP DNS-namn med hello PublicIPAddress-element. hello rekommenderas A-poster inte eftersom hello VIP ändras vid omstart för gateway.

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName "apim-appGw-RG" -Name "publicIP01"
```

##<a name="summary"></a> Sammanfattning
Azure API-hantering som konfigurerats i ett virtuellt nätverk har ett enda gateway-gränssnitt för alla konfigurerade API: er, oavsett om de är värdbaserade lokalt eller i molnet hello. Integrera Programgateway med API Management ger hello flexibilitet för att selektivt aktivera specifika API: er toobe som är tillgängliga på hello Internet samt ge en brandvägg för webbaserade program som en klientdel tooyour API Management-instans.

##<a name="next-steps"></a> Nästa steg
* Lär dig mer om Azure Programgateway
  * [Översikt över Gateway](../application-gateway/application-gateway-introduction.md)
  * [Programmet Gateway Brandvägg för webbaserade program](../application-gateway/application-gateway-webapplicationfirewall-overview.md)
  * [Programgateway med sökväg-baserade Routning](../application-gateway/application-gateway-create-url-route-arm-ps.md)
* Lär dig mer om API-hantering och Vnet
  * [Använda API Management bara användas i hello VNET](api-management-using-with-internal-vnet.md)
  * [Med hjälp av API-hantering i virtuella nätverk](api-management-using-with-vnet.md)
