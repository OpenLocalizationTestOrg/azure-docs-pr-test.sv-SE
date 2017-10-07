---
title: aaaConfigure end tooend SSL med Azure Programgateway | Microsoft Docs
description: "Den här artikeln beskriver hur tooconfigure avslutas tooend SSL med Application Gateway med hjälp av Azure Resource Manager PowerShell"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: e6d80a33-4047-4538-8c83-e88876c8834e
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/19/2017
ms.author: gwallace
ms.openlocfilehash: 7c280478e143d309e7665219441cbee8c81d9a80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-end-tooend-ssl-with-application-gateway-using-powershell"></a>Konfigurera slutet tooend SSL med hjälp av PowerShell-Programgateway

## <a name="overview"></a>Översikt

Ett program har stöd för Gateway avslutas tooend kryptering av trafik. Programgateway gör detta genom att avsluta hello SSL-anslutningen vid hello Programgateway. hello gateway gäller hello routningsregler toohello trafik, krypterar hello paket och vidarebefordrar hello paket toohello lämplig backend utifrån hello routning regler. Alla svar från webbservern hello passerar hello samma process tillbaka toohello slutanvändaren.

En annan funktion som Programgateway stöder definiera anpassade SSL-alternativ. Inaktivera hello följande protokoll, version, har stöd för Programgateway **TLSv1.0**, **TLSv1.1**, och **TLSv1.2** samt definiera hello som cipher paket toouse och hello prioritetsordning.  toolearn mer information om konfigurerbara alternativ som SSL, besök [översikt över SSL-Policy](application-gateway-SSL-policy-overview.md).

> [!NOTE]
> SSL 2.0 och SSL 3.0 är inaktiverade som standard och kan inte aktiveras. De betraktas som osäkra och inte kan toobe används med Application Gateway.

![scenario-bild][scenario]

## <a name="scenario"></a>Scenario

I det här scenariot kan du lära dig hur toocreate som en gateway som använder avslutas tooend SSL med hjälp av PowerShell.

Det här scenariot kommer:

* Skapa en resursgrupp med namnet **appgw rg**
* Skapa ett virtuellt nätverk med namnet **appgwvnet** med ett adressutrymme för 10.0.0.0/16.
* Skapa två undernät kallas **appgwsubnet** och **appsubnet**.
* Skapa ett litet program gateway stödjande slutet tooend SSL-kryptering som gränser SSL-protokoll versioner och krypteringssviter.

## <a name="before-you-begin"></a>Innan du börjar

tooconfigure end tooend SSL med en Programgateway, ett certifikat krävs för hello gateway och certifikat krävs för hello backend-servrar. Hej gatewaycertifikatet är används tooencrypt och dekryptera hello trafik som skickas tooit med SSL. hello gateway-certifikat måste toobe Personal Information Exchange (pfx)-format. Det här filformatet tillåter hello privata nyckel toobe exporteras som krävs för hello programmet gateway tooperform hello kryptering och dekryptering av trafik.

Tooend SSL-kryptering hello backend måste vara godkända med Programgateway för end. Detta görs genom att överföra hello offentliga certifikat för hello serverdelar toohello Programgateway. Detta säkerställer att hello Programgateway kommunicerar endast med kända backend-instanser. Detta skyddar ytterligare tooend hello slutpunkt-kommunikation.

Den här processen beskrivs i hello följande steg:

## <a name="create-hello-resource-group"></a>Skapa hello resursgruppen.

Det här avsnittet vägleder dig genom att skapa en resursgrupp, som innehåller hello Programgateway.

### <a name="step-1"></a>Steg 1

Logga in tooyour Azure-konto.

```powershell
Login-AzureRmAccount
```

### <a name="step-2"></a>Steg 2

Välj hello prenumeration toouse för det här scenariot.

```powershell
Select-AzureRmsubscription -SubscriptionName "<Subscription name>"
```

### <a name="step-3"></a>Steg 3

Skapa en resursgrupp (hoppa över detta steg om du använder en befintlig resursgrupp).

```powershell
New-AzureRmResourceGroup -Name appgw-rg -Location "West US"
```

## <a name="create-a-virtual-network-and-a-subnet-for-hello-application-gateway"></a>Skapa ett virtuellt nätverk och ett undernät för hello Programgateway

hello följande exempel skapas ett virtuellt nätverk och två undernät. Ett undernät är används toohold hello Programgateway. hello används andra undernät för hello serverdelar värd hello webbprogram.

### <a name="step-1"></a>Steg 1

Tilldela ett adressintervall för undernätet hello användas för hello Programgateway sig själv.

```powershell
$gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24
```

> [!NOTE]
> Undernät som konfigurerats för Programgateway bör vara rätt storlek. En Programgateway kan konfigureras för in too10 instanser. Varje instans tar en IP-adress från hello undernät. För litet för ett undernät som kan påverkas negativt och skala ut en Programgateway.
> 
> 

### <a name="step-2"></a>Steg 2

Tilldela en adressintervallet toobe som används för hello serverdelen för adresspoolen.

```powershell
$nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24
```

### <a name="step-3"></a>Steg 3

Skapa ett virtuellt nätverk med hello undernät som definierats i föregående steg hello.

```powershell
$vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet
```

### <a name="step-4"></a>Steg 4

Hämta hello virtuellt nätverk resurs och undernät resurser toobe används i hello följande steg:

```powershell
$vnet = Get-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg
$gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -VirtualNetwork $vnet
$nicSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appsubnet' -VirtualNetwork $vnet
```

## <a name="create-a-public-ip-address-for-hello-front-end-configuration"></a>Skapa en offentlig IP-adress för hello frontend-konfiguration

Skapa en offentlig IP-resurs toobe användas för Programgateway hello. Den här offentliga IP-adressen används följande steg.

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -Name 'publicIP01' -Location "West US" -AllocationMethod Dynamic
```

> [!IMPORTANT]
> Programgateway stöder inte hello användning av en offentlig IP-adress som skapats med en domänetikett. Endast en offentlig IP-adress med en dynamiskt skapade domänetikett stöds. Om du behöver ett eget dns-namn för hello Programgateway rekommenderas toouse en CNAME-post registreras som ett alias.

## <a name="create-an-application-gateway-configuration-object"></a>Skapa ett konfigurationsobjekt för programgatewayen

Innan du skapar hello Programgateway anges alla konfigurationsobjekt. hello med följande steg skapar hello konfigurationsobjekt som behövs för en gateway programresursen.

### <a name="step-1"></a>Steg 1

Skapa ett program gateway IP-konfiguration, den här inställningen konfigurerar vilka undernät hello programmet gateway använder. När Programgateway startar hämtar en IP-adress från hello-undernät som konfigurerats och dirigerar trafik toohello IP-adresser på nätverket i hello backend-IP-adresspool. Tänk på att varje instans använder en IP-adress.

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet
```

### <a name="step-2"></a>Steg 2

Skapa en frontend IP-konfiguration, inställningen mappar en privat eller offentlig IP-adress toohello klientdel för hello Programgateway. hello efter steg associerar hello offentliga IP-adressen i hello föregående steg med hello frontend IP-konfiguration.

```powershell
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip
```

### <a name="step-3"></a>Steg 3

Konfigurera hello backend-IP-adresspool med hello IP-adresser i hello backend-webbservrar. IP-adresserna är hello IP-adresser som tar emot hello nätverkstrafik som kommer från hello frontend IP-slutpunkt. Ersätt hello efter IP-adresser tooadd egna programslutpunkter IP-adress.

```powershell
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3
```

> [!NOTE]
> Ett fullständigt kvalificerat domännamn (FQDN) är också ett giltigt värde i stället för en ip-adress för hello backend-servrar med hjälp av hello - BackendFqdns växel. 

### <a name="step-4"></a>Steg 4

Konfigurera hello frontend IP-port för hello offentlig IP-slutpunkt. Den här porten är hello som användare ansluta till.

```powershell
$fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443
```

### <a name="step-5"></a>Steg 5

Konfigurera hello certifikat för hello Programgateway. Det här certifikatet används toodecrypt och kryptera hello-trafik på hello Programgateway.

```powershell
$cert = New-AzureRmApplicationGatewaySSLCertificate -Name cert01 -CertificateFile <full path too.pfx file> -Password <password for certificate file>
```

> [!NOTE]
> Det här exemplet konfigurerar hello-certifikat som används för SSL-anslutning. hello certifikat måste toobe i PFX-format och hello lösenordet måste innehålla mellan 4 too12 tecken.

### <a name="step-6"></a>Steg 6

Skapa hello HTTP-lyssnare för Programgateway hello. Tilldela hello frontend ip-konfiguration, port och toouse för SSL-certifikat.

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SSLCertificate $cert
```

### <a name="step-7"></a>Steg 7

Överför hello certifikatet toobe används på hello SSL aktiverat backend resurser.

> [!NOTE]
> hello standard avsökningen hämtar hello offentlig nyckel från hello **standard** SSL-bindning på hello backend-'s IP-adress och jämför hello offentliga nyckelvärde den tar emot toohello offentliga nyckel/värde du anger här. hello hämtade offentliga nyckel kan inte nödvändigtvis hello avsedda platsen toowhich trafikflöden **om** du använder värdhuvuden och SNI på hello serverdel. Om du tvekar besöka https://127.0.0.1/ på hello-servrar tooconfirm vilket certifikat som används för hello **standard** SSL-bindning. Använd hello offentliga nyckeln från denna begäran i det här avsnittet. Om du använder värdhuvuden och SNI på HTTPS-bindningar och du får ett svar och certifikat från en manuell webbläsare begäran toohttps://127.0.0.1/ på hello-servrar, måste du ställa in en standard-SSL-bindning på hello-servrar. Om du inte gör det avsökningar misslyckas och hello backend-är inte godkända.

```powershell
$authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile C:\users\gwallace\Desktop\cert.cer
```

> [!NOTE]
> hello-certifikat som angetts i det här steget ska hello offentliga nyckeln för hello pfx-certifikat finns på hello serverdel. Exportera hello-certifikat (inte hello rotcertifikat) installerat på hello backend-servern i. CER-format och använda den i det här steget. Det här steget whitelists hello-serverdelen med hello Programgateway.

### <a name="step-8"></a>Steg 8

Konfigurera hello inställningar för Programgateway backend-http. Tilldela hello certifikat på hello föregående steg toohello http-inställningar.

```powershell
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert
```

### <a name="step-9"></a>Steg 9

Skapa en routning regel för belastningsutjämnare som konfigurerar hello belastningsutjämnaren GUID. I det här exemplet skapas en regel för grundläggande resursallokering.

```powershell
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
```

### <a name="step-10"></a>Steg 10

Konfigurera hello instansstorleken för hello Programgateway.  hello tillgängliga storlekar är **Standard\_små**, **Standard\_medel**, och **Standard\_stor**.  Hello tillgängliga värden är 1 till 10 för kapacitet.

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
```

> [!NOTE]
> Du kan välja ett instansantal på 1 för testning. Det är viktigt att valfri instans antalet under två instanser tooknow omfattas inte av hello SLA och därför rekommenderas inte. Liten gateways är toobe som används för utveckling testning och inte för produktion.

### <a name="step-11"></a>Steg 11

Konfigurera hello SSL princip toobe används på hello Application Gateway. Programgateway stöder hello möjlighet tooset en lägsta version för SSL-protokollversioner.

hello är följande värden en lista över protokollversioner som kan definieras.

* **TLSv1_0**
* **TLSv1_1**
* **TLSv1_2**

Anger hello minsta protokollversion för**TLSv1_2** och möjliggör **TLS\_ECDHE\_ECDSA\_WITH\_AES\_128\_GCM\_ SHA256**, **TLS\_ECDHE\_ECDSA\_WITH\_AES\_256\_GCM\_SHA384**, och **TLS\_RSA\_WITH\_AES\_128\_GCM\_SHA256** endast.

```powershell
$SSLPolicy = New-AzureRmApplicationGatewaySSLPolicy -MinProtocolVersion TLSv1_2 -CipherSuite "TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256", "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384", "TLS_RSA_WITH_AES_128_GCM_SHA256"
```

## <a name="create-hello-application-gateway"></a>Skapa hello Programgateway

Skapa hello Programgateway alla hello föregående steg. hello skapa hello gateway är en tidskrävande process.

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgateway -SSLCertificates $cert -ResourceGroupName "appgw-rg" -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SSLPolicy $SSLPolicy -AuthenticationCertificates $authcert -Verbose
```

## <a name="limit-ssl-protocol-versions-on-an-existing-application-gateway"></a>Begränsa SSL-protokoll version på en befintlig Programgateway

hello föregående steg leder dig genom att skapa ett program med end tooend SSL och inaktivera vissa versioner för SSL-protokollet. hello inaktiverar följande exempel vissa SSL-principer på en befintlig gateway för programmet.

### <a name="step-1"></a>Steg 1

Hämta hello programmet gateway tooupdate.

```powershell
$gw = Get-AzureRmApplicationGateway -Name AdatumAppGateway -ResourceGroupName AdatumAppGatewayRG
```

### <a name="step-2"></a>Steg 2

Definiera ett SSL-policy. I följande exempel hello, TLSv1.0 och TLSv1.1 är inaktiverade och hello krypteringssviter **TLS\_ECDHE\_ECDSA\_WITH\_AES\_128\_GCM\_SHA256** , **TLS\_ECDHE\_ECDSA\_WITH\_AES\_256\_GCM\_SHA384**, och  **TLS\_RSA\_WITH\_AES\_128\_GCM\_SHA256** är hello enda tillåtna.

```powershell
Set-AzureRmApplicationGatewaySSLPolicy -MinProtocolVersion -PolicyType Custom -CipherSuite "TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256", "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384", "TLS_RSA_WITH_AES_128_GCM_SHA256" -ApplicationGateway $gw

```

### <a name="step-3"></a>Steg 3

Slutligen uppdatera hello gateway. Det är viktigt toonote att det sista steget är en tidskrävande uppgift. Avsluta tooend SSL är konfigurerat på hello Programgateway när du är klar.

```powershell
$gw | Set-AzureRmApplicationGateway
```

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

Lär dig mer om härdning hello säkerheten för ditt webbprogram med Brandvägg för webbaserade program via Programgateway genom att besöka [översikt över webbprogram brandväggen](application-gateway-webapplicationfirewall-overview.md)

[scenario]: ./media/application-gateway-end-to-end-SSL-powershell/scenario.png
