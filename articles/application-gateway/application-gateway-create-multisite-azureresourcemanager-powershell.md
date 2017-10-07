---
title: "aaaCreate en Programgateway som värd för flera platser | Microsoft Docs"
description: "Den här sidan finns instruktioner toocreate, konfigurera en gateway för Azure-program som värd för flera webbprogram på hello samma gateway."
documentationcenter: na
services: application-gateway
author: amsriva
manager: rossort
editor: amsriva
ms.assetid: b107d647-c9be-499f-8b55-809c4310c783
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/12/2016
ms.author: amsriva
ms.openlocfilehash: bad9a76be0a73a7026a770630fa7156f6e5940c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-for-hosting-multiple-web-applications"></a>Skapa en Programgateway som värd för flera webbprogram

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-multisite-portal.md)
> * [PowerShell och Azure Resource Manager](application-gateway-create-multisite-azureresourcemanager-powershell.md)

Värd för flera plats kan du toodeploy mer än ett webbprogram på hello samma Programgateway. Det är beroende av förekomsten av värdhuvudet i hello inkommande HTTP-begäran, toodetermine vilka lyssnare skulle ta emot trafik. hello lyssnare dirigerar sedan trafik tooappropriate serverdelspool som konfigurerats i hello regler definition av hello gateway. I SSL aktiverat webbprogram beroende Programgateway hello Server Servernamnsindikation (SNI)-tillägget toochoose hello rätt lyssnare för hello webbtrafik. Ett vanligt användningsområde för värd för flera plats är tooload begäranden för annan webbplats domäner toodifferent backend-serverpooler. På liknande sätt hello flera underdomäner i hello samma rotdomänen kan också finnas på samma Programgateway.

## <a name="scenario"></a>Scenario

I följande exempel hello, Programgateway betjäna trafik för contoso.com och fabrikam.com med två backend-server-adresspooler: contoso-serverpoolen och fabrikam-serverpoolen. Liknande installationsprogrammet kan vara används toohost underdomäner som app.contoso.com och blog.contoso.com.

![imageURLroute](./media/application-gateway-create-multisite-azureresourcemanager-powershell/multisite.png)

## <a name="before-you-begin"></a>Innan du börjar

1. Installera hello senaste versionen av hello Azure PowerShell-cmdlets med hello installationsprogram för webbplattform. Du kan hämta och installera hello senaste versionen från hello **Windows PowerShell** avsnitt i hello [Nedladdningssida](https://azure.microsoft.com/downloads/).
2. hello servrar lagts toohello backend-adresspool toouse hello Programgateway måste finnas eller har skapat sina slutpunkter i hello virtuellt nätverk i ett separat undernät eller med en offentlig IP-adress/VIP tilldelad.

## <a name="requirements"></a>Krav

* **Backend-serverpoolen:** hello lista över IP-adresser för hello backend-servrar. hello IP-adresser som anges antingen ska tillhöra toohello undernät för virtuellt nätverk eller ska vara en offentlig IP-adress/VIP. FQDN kan också användas.
* **Inställningar för backend-serverpool:** Varje pool har inställningar som port, protokoll och cookiebaserad tillhörighet. De här inställningarna är bundet tooa poolen och tillämpade tooall servrar inom hello poolen.
* **Frontend-port:** den här porten är hello offentliga som öppnas på hello Programgateway. Trafik träffar den här porten och sedan hämtar omdirigeras tooone hello backend-servrar.
* **Lyssnare:** hello-lyssnare har en frontend-port, ett protokoll (Http eller Https dessa värden är skiftlägeskänsligt), och hello SSL-certifikatnamn (om hur du konfigurerar SSL-avlastning). För flera platser aktiverade programmet gateway läggs värdnamn och SNI indikatorer till.
* **Regel:** hello regeln Binder hello lyssnare, hello backend-serverpoolen, och definierar vilken backend-server pool hello trafik ska vara riktad toowhen den når en viss lyssnare. Regler bearbetas i hello ordning och trafik dirigeras via hello första regeln som överensstämmer oavsett särskilda egenskaper. Till exempel om du har en regel med en grundläggande lyssnare och en regel med en flera platser lyssnare båda på samma port hello regel med hello måste hello flera platser lyssnare anges innan hello regeln med grundläggande hello-lyssnare för hello flera platser regeln toofunction som förväntades.

## <a name="create-an-application-gateway"></a>Skapa en programgateway

hello följande är hello steg behövs toocreate en Programgateway:

1. Skapa en resursgrupp för Resource Manager.
2. Skapa ett virtuellt nätverk, undernät och offentliga IP för hello Programgateway.
3. Skapa ett konfigurationsobjekt för programgatewayen.
4. Skapa en resurs för en programgateway.

## <a name="create-a-resource-group-for-resource-manager"></a>Skapa en resursgrupp för Resource Manager

Kontrollera att du använder hello senaste versionen av Azure PowerShell. Mer information finns på [med hjälp av Windows PowerShell med Resource Manager](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Steg 1

Logga in tooAzure

```powershell
Login-AzureRmAccount
```
Du kan ange tooauthenticate med dina autentiseringsuppgifter.

### <a name="step-2"></a>Steg 2

Kontrollera hello prenumerationer för hello-kontot.

```powershell
Get-AzureRmSubscription
```

### <a name="step-3"></a>Steg 3

Välj vilka av dina Azure-prenumerationer toouse.

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="step-4"></a>Steg 4

Skapa en resursgrupp (hoppa över detta steg om du använder en befintlig resursgrupp).

```powershell
New-AzureRmResourceGroup -Name appgw-RG -location "West US"
```

Alternativt kan du också skapa taggar för en resursgrupp för Programgateway:

```powershell
$resourceGroup = New-AzureRmResourceGroup -Name appgw-RG -Location "West US" -Tags @{Name = "testtag"; Value = "Application Gateway multiple site"}
```

Azure Resource Manager kräver att alla resursgrupper anger en plats. Den här platsen används som hello standardplatsen för resurser i resursgruppen. Se till att alla kommandon toocreate ett program gateway används hello samma resursgrupp.

Vi har skapat en resursgrupp med namnet i hello-exemplet ovan, **appgw RG** med en plats för **västra USA**.

> [!NOTE]
> Om du behöver tooconfigure en anpassad avsökningsåtgärd för din Programgateway finns [skapa en Programgateway med anpassade avsökningar med hjälp av PowerShell](application-gateway-create-probe-ps.md). Besök [anpassade avsökningar, hälsoövervakning och](application-gateway-probe-overview.md) för mer information.

## <a name="create-a-virtual-network-and-subnets"></a>Skapa ett virtuellt nätverk och undernät

följande exempel visar hur hello toocreate ett virtuellt nätverk med hjälp av hanteraren för filserverresurser. Två undernät skapas i det här steget. hello första undernätet är för hello Programgateway sig själv. Programgateway kräver sin egen undernät toohold instanser. Andra programgatewayer kan distribueras i det undernätet. hello andra undernät är används toohold hello programmet backend-servrar.

### <a name="step-1"></a>Steg 1

Tilldela hello adressintervallet 10.0.0.0/24 toohello undernät variabeln toobe används toohold hello Programgateway.

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name appgatewaysubnet -AddressPrefix 10.0.0.0/24
```
### <a name="step-2"></a>Steg 2

Tilldela hello adressintervallet 10.0.1.0/24 toohello Undernät2 variabeln toobe används för hello serverdelspooler.

```powershell
$subnet2 = New-AzureRmVirtualNetworkSubnetConfig -Name backendsubnet -AddressPrefix 10.0.1.0/24
```

### <a name="step-3"></a>Steg 3

Skapa ett virtuellt nätverk med namnet **appgwvnet** i resursgruppen **appgw rg** för hello västra USA region med undernätet 10.0.0.0/24 hello prefixet 10.0.0.0/16 och 10.0.1.0/24.

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet,$subnet2
```

### <a name="step-4"></a>Steg 4

Tilldela en variabel för hello nästa steg, undernät som skapar en Programgateway.

```powershell
$appgatewaysubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name appgatewaysubnet -VirtualNetwork $vnet
$backendsubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name backendsubnet -VirtualNetwork $vnet
```

## <a name="create-a-public-ip-address-for-hello-front-end-configuration"></a>Skapa en offentlig IP-adress för hello frontend-konfiguration

Skapa en offentlig IP-resurs **publicIP01** i resursgruppen **appgw rg** för hello västra USA region.

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

En IP-adress tilldelas toohello Programgateway när hello-tjänsten startas.

## <a name="create-application-gateway-configuration"></a>Skapa program gateway-konfiguration

Du har tooset in alla konfigurationsobjekt innan du skapar hello Programgateway. hello med följande steg skapar hello konfigurationsobjekt som behövs för en gateway programresursen.

### <a name="step-1"></a>Steg 1

Skapa en IP-konfiguration för programgatewayen med namnet **gatewayIP01**. När Programgateway startar hämtar en IP-adress från hello-undernät som konfigurerats och dirigera trafik toohello IP-adresser på nätverket i hello backend-IP-adresspool. Tänk på att varje instans använder en IP-adress.

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $appgatewaysubnet
```

### <a name="step-2"></a>Steg 2

Konfigurera hello backend-IP-adresspool med namnet **pool01** och **pool2** med IP-adresser **134.170.185.46**, **134.170.188.221**, **134.170.185.50** för **pool1** och **134.170.186.46**, **134.170.189.221**, **134.170.186.50**  för **pool2**.

```powershell
$pool1 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 10.0.1.100, 10.0.1.101, 10.0.1.102
$pool2 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool02 -BackendIPAddresses 10.0.1.103, 10.0.1.104, 10.0.1.105
```

I det här exemplet finns det två backend-pooler tooroute nätverkstrafik som baserat på hello begärda webbplatsen. En pool tar emot trafik från platsen ”contoso.com” och andra poolen tar emot trafik från platsen ”fabrikam.com”. Du har tooreplace hello föregående IP-adresser tooadd egna programslutpunkter IP-adress. I stället för interna IP-adresser, kan du också använda offentliga IP-adresser, FQDN eller en virtuell dators nätverkskort för backend-instanser. toospecify FQDN i stället för IP-adresser PowerShell används ”-BackendFQDNs” parametern.

### <a name="step-3"></a>Steg 3

Konfigurera gateway programinställning **poolsetting01** och **poolsetting02** för hello belastningsutjämnad nätverkstrafik i hello backend-adresspool. I det här exemplet kan du konfigurera inställningarna för olika backend-pool för hello backend-pooler. Varje serverdelspool kan ha sin egen serverdelspoolinställning.

```powershell
$poolSetting01 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120
$poolSetting02 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting02" -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 240
```

### <a name="step-4"></a>Steg 4

Konfigurera IP-frontend-hello med offentliga IP-slutpunkt.

```powershell
$fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-5"></a>Steg 5

Konfigurera hello frontend-port för en Programgateway.

```powershell
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "fep01" -Port 443
```

### <a name="step-6"></a>Steg 6

Konfigurera två SSL-certifikat för hello två webbplatser vi toosupport i det här exemplet. Ett certifikat är för contoso.com trafik och hello andra för fabrikam.com trafik. Dessa certifikat ska vara en certifikatutfärdare som utfärdade certifikat för webbplatser. Självsignerade certifikat stöds men rekommenderas inte för produktion trafik.

```powershell
$cert01 = New-AzureRmApplicationGatewaySslCertificate -Name contosocert -CertificateFile <file path> -Password <password>
$cert02 = New-AzureRmApplicationGatewaySslCertificate -Name fabrikamcert -CertificateFile <file path> -Password <password>
```

### <a name="step-7"></a>Steg 7

Konfigurera två lyssnare för hello två webbplatser i det här exemplet. Det här steget konfigurerar hello-lyssnare för offentlig IP-adress, port och värdnamn används tooreceive inkommande trafik. HostName parametern krävs för stöd för flera plats och bör vara set toohello lämplig webbplats för vilken hello trafik som tas emot. RequireServerNameIndication parameter ska anges tootrue för webbplatser som behöver stöd för SSL i ett scenario med flera värden. Om SSL-stöd krävs, måste du också toospecify hello SSL-certifikatet som används toosecure trafik för att webbprogrammet. hello kombination av FrontendIPConfiguration, FrontendPort och värdnamn måste vara unika tooa lyssnare. Varje lyssnare har stöd för ett certifikat.

```powershell
$listener01 = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol Https -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -HostName "contoso11.com" -RequireServerNameIndication true  -SslCertificate $cert01
$listener02 = New-AzureRmApplicationGatewayHttpListener -Name "listener02" -Protocol Https -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -HostName "fabrikam11.com" -RequireServerNameIndication true -SslCertificate $cert02
```

### <a name="step-8"></a>Steg 8

Skapa två regel inställningen för hello två webbprogram i det här exemplet. En regel innehåller lyssnare, serverdelspooler och http-inställningar. Det här steget konfigurerar hello programmet gateway toouse grundläggande routningsregel, ett för varje webbplats. Trafik tooeach webbplats tas emot av dess konfigurerade lyssnare och sedan dirigeras tooits konfigurerats serverdelspoolen, genom att använda hello egenskaper som anges i hello BackendHttpSettings.

```powershell
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule01" -RuleType Basic -HttpListener $listener01 -BackendHttpSettings $poolSetting01 -BackendAddressPool $pool1
$rule02 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule02" -RuleType Basic -HttpListener $listener02 -BackendHttpSettings $poolSetting02 -BackendAddressPool $pool2
```

### <a name="step-9"></a>Steg 9

Konfigurera hello antal förekomster och storleken för hello Programgateway.

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name "Standard_Medium" -Tier Standard -Capacity 2
```

## <a name="create-application-gateway"></a>Skapa Programgateway

Skapa en Programgateway med alla konfigurationsobjekt från hello föregående steg.

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-RG -Location "West US" -BackendAddressPools $pool1,$pool2 -BackendHttpSettingsCollection $poolSetting01, $poolSetting02 -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener01, $listener02 -RequestRoutingRules $rule01, $rule02 -Sku $sku -SslCertificates $cert01, $cert02
```

> [!IMPORTANT]
> Programmet Gateway etablering är en tidskrävande åtgärd och kan ta viss tid toocomplete.
> 
> 

## <a name="get-application-gateway-dns-name"></a>Hämta DNS-namn för programgatewayen

När du har skapat hello gateway är hello nästa steg tooconfigure hello klientdelen för kommunikation. När du använder en offentlig IP-adress krävs ett dynamiskt tilldelat DNS-namn som inte är användarvänligt. tooensure slutanvändare kan träffa hello Programgateway, en CNAME-post kan vara används toopoint toohello offentlig slutpunkt för hello Programgateway. [Konfigurera ett eget domännamn i Azure](../cloud-services/cloud-services-custom-domain-name-portal.md). toodo detta, hämta information om hello Programgateway och dess associerade IP DNS-namn med hjälp av hello PublicIPAddress element bifogade toohello Programgateway. hello programmet gateway DNS-namn ska använda toocreate en CNAME-post som pekar hello två web program toothis DNS-namn. hello rekommenderas A-poster inte eftersom hello VIP ändras vid omstart för Programgateway.

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

Lär dig hur tooprotect dina webbplatser med [Programgateway - Brandvägg för webbaserade program](application-gateway-webapplicationfirewall-overview.md)

