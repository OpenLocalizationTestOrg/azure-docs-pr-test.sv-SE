---
title: aaaConfigure SSL-avlastning - Azure Application Gateway - PowerShell | Microsoft Docs
description: "Den här sidan finns instruktioner toocreate en Programgateway med SSL avlasta med hjälp av Azure Resource Manager"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 3c3681e0-f928-4682-9d97-567f8e278e13
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/19/2017
ms.author: gwallace
ms.openlocfilehash: c2855d8d3caaa97ec05475c67ff0f8dce72ef2a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-azure-resource-manager"></a>Konfigurera en programgateway för SSL-avlastning med hjälp av Azure Resource Manager

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-ssl-portal.md)
> * [PowerShell och Azure Resource Manager](application-gateway-ssl-arm.md)
> * [PowerShell och den klassiska Azure-portalen](application-gateway-ssl.md)
> * [Azure CLI 2.0](application-gateway-ssl-cli.md)

Azure Application Gateway kan vara konfigurerade tooterminate hello Secure Sockets Layer (SSL)-session på hello gateway tooavoid kostsamma SSL dekryptering uppgifter toohappen på hello webbservergrupp. SSL-avlastning förenklar också hello front-end-serverinstallation och hantering av hello webbprogram.

## <a name="before-you-begin"></a>Innan du börjar

1. Installera hello senaste versionen av hello Azure PowerShell-cmdlets med hello installationsprogram för webbplattform. Du kan hämta och installera hello senaste versionen från hello **Windows PowerShell** avsnitt i hello [Nedladdningssida](https://azure.microsoft.com/downloads/).
2. Du skapar ett virtuellt nätverk och ett undernät för hello Programgateway. Se till att inga virtuella datorer eller molndistributioner använder hello undernät. Application Gateway måste vara fristående i ett virtuellt nätverks undernät.
3. hello-servrar som du konfigurerar toouse hello Programgateway måste finnas eller har skapat sina slutpunkter i hello virtuellt nätverk eller med en offentlig IP-adress/VIP tilldelad.

## <a name="what-is-required-toocreate-an-application-gateway"></a>Vad är obligatoriska toocreate en Programgateway?

* **Backend-serverpoolen:** hello lista över IP-adresser för hello backend-servrar. hello IP-adresser som anges antingen ska tillhöra toohello undernät för virtuellt nätverk eller ska vara en offentlig IP-adress/VIP.
* **Inställningar för backend-serverpool:** Varje pool har inställningar som port, protokoll och cookiebaserad tillhörighet. De här inställningarna är bundet tooa poolen och tillämpade tooall servrar inom hello poolen.
* **Frontend-port:** den här porten är hello offentliga som öppnas på hello Programgateway. Trafik träffar den här porten och sedan hämtar omdirigeras tooone hello backend-servrar.
* **Lyssnare:** hello-lyssnare har en frontend-port, ett protokoll (Http eller Https inställningarna är skiftlägeskänsligt), och hello SSL-certifikatnamn (om hur du konfigurerar SSL-avlastning).
* **Regel:** hello regeln Binder hello-lyssnare och hello backend-serverpoolen och definierar vilken backend-server pool hello trafik ska vara riktad toowhen den når en viss lyssnare. För närvarande endast hello *grundläggande* regeln stöds. Hej *grundläggande* regeln är resursallokering belastningsdistribution.

**Information om ytterligare konfiguration**

Konfiguration för SSL-certifikat, hello-protokollet i **HttpListener** bör ändra för*Https* (skiftlägeskänsligt). Hej **SslCertificate** läggs elementet för**HttpListener** med hello variabelvärde som konfigurerats för hello SSL-certifikat. hello frontend-porten ska vara uppdaterade too443.

**tooenable cookie-baserad tillhörighet**: en Programgateway kan vara konfigurerade tooensure att en begäran från en klientsession är alltid dirigerad toohello samma virtuella dator i hello webbservergrupp. Det här scenariot sker genom injektion av en sessions-cookie som tillåter trafik för hello gateway toodirect på lämpligt sätt. Ange tooenable cookie-baserad tillhörighet **CookieBasedAffinity** för*aktiverad* i hello **BackendHttpSettings** element.

## <a name="create-an-application-gateway"></a>Skapa en programgateway

hello skillnaden mellan att använda hello Azure klassiska distributionsmodellen och Azure Resource Manager är hello order som du skapar ett program gateway och hello objekt behöver toobe konfigurerats.

Med Resource Manager kan alla komponenter i en Programgateway konfigureras individuellt och publicera sedan tillsammans toocreate gatewayresursen ett program.

Här följer hello steg som krävs för toocreate en Programgateway:

1. Skapa en resursgrupp för Resource Manager
2. Skapa virtuella nätverk, undernät och offentliga IP för hello Programgateway
3. Skapa ett konfigurationsobjekt för programgatewayen
4. Skapa en resurs för programgatewayen

## <a name="create-a-resource-group-for-resource-manager"></a>Skapa en resursgrupp för Resource Manager

Kontrollera att du växla PowerShell läge toouse hello Azure Resource Manager-cmdlets. Mer information finns i [Använda Windows PowerShell med Resource Manager](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Steg 1

```powershell
Login-AzureRmAccount
```

### <a name="step-2"></a>Steg 2

Kontrollera hello prenumerationer för hello-kontot.

```powershell
Get-AzureRmSubscription
```

Du kan ange tooauthenticate med dina autentiseringsuppgifter.

### <a name="step-3"></a>Steg 3

Välj vilka av dina Azure-prenumerationer toouse.

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="step-4"></a>Steg 4

Skapa en resursgrupp (hoppa över detta steg om du använder en befintlig resursgrupp).

```powershell
New-AzureRmResourceGroup -Name appgw-rg -Location "West US"
```

Azure Resource Manager kräver att alla resursgrupper anger en plats. Den här inställningen används som hello standardplatsen för resurser i resursgruppen. Se till att alla kommandon toocreate använder en Programgateway hello samma resursgrupp.

Vi har skapat en resursgrupp med namnet i hello-exemplet ovan, **appgw RG** och plats **västra USA**.

## <a name="create-a-virtual-network-and-a-subnet-for-hello-application-gateway"></a>Skapa ett virtuellt nätverk och ett undernät för hello Programgateway

följande exempel visar hur hello toocreate ett virtuellt nätverk med hjälp av hanteraren för filserverresurser:

### <a name="step-1"></a>Steg 1

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
```

Det här exemplet tilldelas hello adressintervallet 10.0.0.0/24 tooa undernät variabeln toobe används toocreate ett virtuellt nätverk.

### <a name="step-2"></a>Steg 2

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet
```

Det här exemplet skapar ett virtuellt nätverk med namnet **appgwvnet** i resursgruppen **appgw rg** för hello västra USA region med undernätet 10.0.0.0/24 hello prefixet 10.0.0.0/16.

### <a name="step-3"></a>Steg 3

```powershell
$subnet = $vnet.Subnets[0]
```

Det här exemplet tilldelas hello undernät objektet toovariable $subnet hello nästa steg.

## <a name="create-a-public-ip-address-for-hello-front-end-configuration"></a>Skapa en offentlig IP-adress för hello frontend-konfiguration

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

Det här exemplet skapar en offentlig IP-resurs **publicIP01** i resursgruppen **appgw rg** för hello västra USA region.

## <a name="create-an-application-gateway-configuration-object"></a>Skapa ett konfigurationsobjekt för programgatewayen

### <a name="step-1"></a>Steg 1

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
```

Det här exemplet skapar ett program gateway IP-konfiguration med namnet **gatewayIP01**. När Programgateway startar hämtar en IP-adress från hello-undernät som konfigurerats och dirigera trafik toohello IP-adresser på nätverket i hello backend-IP-adresspool. Tänk på att varje instans använder en IP-adress.

### <a name="step-2"></a>Steg 2

```powershell
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50
```

Det här exemplet konfigurerar hello backend-IP-adresspool med namnet **pool01** med IP-adresser **134.170.185.46**, **134.170.188.221**, **134.170.185.50** . Dessa värden är hello IP-adresser som tar emot hello nätverkstrafik som kommer från hello frontend IP-slutpunkt. Ersätt hello IP-adresser från hello föregående exempel med hello IP-adresser på din web application slutpunkter.

### <a name="step-3"></a>Steg 3

```powershell
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Enabled
```

Det här exemplet konfigurerar gateway programinställning **poolsetting01** tooload belastningsutjämnade trafik i hello backend-adresspool.

### <a name="step-4"></a>Steg 4

```powershell
$fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 443
```

Det här exemplet konfigurerar hello frontend IP-port med namnet **frontendport01** för hello offentlig IP-slutpunkt.

### <a name="step-5"></a>Steg 5

```powershell
$cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path for certificate file> -Password "<password>"
```

Det här exemplet konfigurerar hello-certifikat som används för SSL-anslutning. hello certifikat måste toobe i PFX-format och hello lösenordet måste innehålla mellan 4 too12 tecken.

### <a name="step-6"></a>Steg 6

```powershell
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip
```

Det här exemplet skapar hello frontend IP-konfiguration med namnet **fipconfig01** och associerats hello offentlig IP-adress med hello frontend IP-konfiguration.

### <a name="step-7"></a>Steg 7

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert
```

Det här exemplet skapar hello grupplyssnarens namn **listener01** och associerats hello frontend port toohello frontend IP-konfiguration och certifikat.

### <a name="step-8"></a>Steg 8

```powershell
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
```

Det här exemplet skapar hello routning-regel för belastningsutjämning med namnet **rule01** som konfigurerar hello belastningsutjämnaren GUID.

### <a name="step-9"></a>Steg 9

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
```

Det här exemplet konfigurerar hello instansstorleken för hello Programgateway.

> [!NOTE]
> Hej standardvärdet för *InstanceCount* är 2, med ett maximalt värde 10. Hej standardvärdet för *GatewaySize* är Medium. Du kan välja mellan Standard_Small, Standard_Medium och Standard_Large.

### <a name="step-10"></a>Steg 10

```powershell
$policy = New-AzureRmApplicationGatewaySslPolicy -PolicyType Predefined -PolicyName AppGwSslPolicy20170401S
```

Det här steget definierar hello SSL princip toouse på hello Programgateway. Besök [Konfigurera SSL princip versioner och krypteringssviter på Programgateway](application-gateway-configure-ssl-policy-powershell.md) toolearn mer.

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a>Skapa en programgateway med hjälp av New-AzureApplicationGateway

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SslCertificates $cert -SslPolicy $policy
```

Det här exemplet skapar en Programgateway med alla konfigurationsobjekt från hello föregående steg. I exemplet hello hello Programgateway kallas **appgwtest**.

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

Om du vill tooconfigure ett program gateway toouse med en intern belastningsutjämnare (ILB), se [skapa en Programgateway med en intern belastningsutjämnare (ILB)](application-gateway-ilb.md).

Om du vill ha mer information om belastningsutjämningsalternativ i allmänhet läser du:

* [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

