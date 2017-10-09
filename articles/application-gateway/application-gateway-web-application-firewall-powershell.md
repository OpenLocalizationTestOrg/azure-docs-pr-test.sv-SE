---
title: "aaaConfigure Brandvägg för webbaserade program - Azure Programgateway | Microsoft Docs"
description: "Den här artikeln innehåller råd om hur toostart med hjälp av web application-brandväggen på en befintlig eller ny Programgateway."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 670b9732-874b-43e6-843b-d2585c160982
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/03/2017
ms.author: gwallace
ms.openlocfilehash: bd2a69901f0ec0d6551d633e2588b74c3ab59a71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-web-application-firewall-on-a-new-or-existing-application-gateway"></a>Konfigurera Brandvägg för webbaserade program på en ny eller befintlig Programgateway

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-web-application-firewall-portal.md)
> * [PowerShell](application-gateway-web-application-firewall-powershell.md)
> * [Azure CLI](application-gateway-web-application-firewall-cli.md)

Lär dig hur toocreate Brandvägg för webbaserade program aktiverade Application Gateway eller Lägg till web application brandväggen tooan befintliga Application Gateway.

hello Brandvägg för webbaserade program (Brandvägg) i Azure Application Gateway skyddar webbprogram från vanliga webbaserade attacker som SQL injection, webbplatser scripting attacker och sessionen hijacks.

Azure Application Gateway är en Layer 7-belastningsutjämnare. Det ger redundans, prestanda-routing HTTP-begäranden mellan olika servrar, oavsett om de är på hello molnet eller lokalt. Programgateway innehåller många program leverans (ADC) funktioner i nätverksstyrenheten inklusive HTTP läsa in belastningsutjämning, cookie-baserad session tillhörighet, secure sockets layer (SSL)-avlastning, anpassade hälsoavsökningar, stöd för flera platser och många andra. Besök toofind en fullständig lista över funktioner som stöds: [översikt för Programgateway](application-gateway-introduction.md).

hello följande artikeln visar hur för[lägga till befintliga Programgateway web application brandväggen tooan](#add-web-application-firewall-to-an-existing-application-gateway) och [skapa en Gateway för program som använder Brandvägg för webbaserade program](#create-an-application-gateway-with-web-application-firewall).

![scenario-bild][scenario]

## <a name="waf-configuration-differences"></a>Brandvägg configuration skillnader

Om du har läst [skapar en Programgateway med PowerShell](application-gateway-create-gateway-arm.md), du förstår hello SKU inställningar tooconfigure när du skapar en Programgateway. Brandvägg ger ytterligare inställningar toodefine när du konfigurerar hello SKU: N på en Programgateway. Det finns inga ytterligare ändringar du gör på hello Programgateway sig själv.

| **Inställning** | **Detaljer**
|---|---|
|**SKU** |En normal Programgateway utan Brandvägg stöder **Standard\_små**, **Standard\_medel**, och **Standard\_stor** storlekar. Med hello införandet av Brandvägg, det finns två ytterligare SKU: er, **Brandvägg\_medel** och **Brandvägg\_stor**. Brandvägg stöds inte på små Programgatewayer.|
|**Nivå** | hello tillgängliga värden är **Standard** eller **Brandvägg**. När du använder Brandvägg för webbaserade program **Brandvägg** måste väljas.|
|**Läge** | Den här inställningen är hello läge för Brandvägg. tillåtna värden är **identifiering** och **förebyggande**. När Brandvägg är inställd i identifieringsläget lagras alla hot i en loggfil. Händelser loggas fortfarande i förebyggande läge, men hello angripare tar emot ett 403 obehörig svar från hello Application Gateway.|

## <a name="add-web-application-firewall-tooan-existing-application-gateway"></a>Lägg till web application brandväggen tooan befintliga Programgateway

Kontrollera att du använder hello senaste versionen av Azure PowerShell. Mer information finns i [Använda Windows PowerShell med Resource Manager](../powershell-azure-resource-manager.md).

1. Logga in tooyour Azure-konto.

    ```powershell
    Login-AzureRmAccount
    ```

2. Välj hello prenumeration toouse för det här scenariot.

    ```powershell
    Select-AzureRmSubscription -SubscriptionName "<Subscription name>"
    ```

3. Hämta hello-gateway som du lägger till Brandvägg för webbaserade program till.

    ```powershell
    $gw = Get-AzureRmApplicationGateway -Name "AdatumGateway" -ResourceGroupName "MyResourceGroup"
    ```

1. Konfigurera hello web application firewall sku. hello tillgängliga storlekar är **Brandvägg\_stor** och **Brandvägg\_medel**. När Brandvägg för webbaserade program används hello nivå måste vara **Brandvägg**, hello kapacitet måste bekräftas när hello sku.

    ```powershell
    $gw | Set-AzureRmApplicationGatewaySku -Name WAF_Large -Tier WAF -Capacity 2
    ```

1. Konfigurera inställningar för hello Brandvägg som har definierats i följande exempel hello:

   För **FirewallMode**, hello tillgängliga värden är förebyggande och identifiering.

    ```powershell
    $gw | Set-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode Prevention
    ```

1. Uppdatera hello Programgateway med hello inställningarna i hello föregående steg.

    ```powershell
    Set-AzureRmApplicationGateway -ApplicationGateway $gw
    ```

Det här kommandot uppdaterar hello Programgateway med Brandvägg för webbaserade program. Besök [Programgateway diagnostik](application-gateway-diagnostics.md) toounderstand hur tooview loggar för Application Gateway. På grund av toohello säkerhet uppbyggnad Brandvägg, loggar måste toobe ses över regelbundet toounderstand hello säkerhetstillståndet webbprogram.

## <a name="create-an-application-gateway-with-web-application-firewall"></a>Skapa en Programgateway med Brandvägg för webbaserade program

hello följande steg leder dig genom hello hela processen från början tooend för att skapa en Programgateway med Brandvägg för webbaserade program.

Kontrollera att du använder hello senaste versionen av Azure PowerShell. Mer information finns i [Använda Windows PowerShell med Resource Manager](../powershell-azure-resource-manager.md).

1. Logga in tooAzure genom att köra `Login-AzureRmAccount`, du är ange tooauthenticate med dina autentiseringsuppgifter.

1. Kontrollera hello prenumerationer för hello konto genom att köra`Get-AzureRmSubscription`

1. Välj vilka av dina Azure-prenumerationer toouse.

    ```powershell
    Select-AzureRmsubscription -SubscriptionName "<Subscription name>"
    ```

### <a name="create-a-resource-group"></a>Skapa en resursgrupp

Skapa en resursgrupp för hello Application Gateway.

```powershell
New-AzureRmResourceGroup -Name appgw-rg -Location "West US"
```

Azure Resource Manager kräver att alla resursgrupper anger en plats. Den här platsen används som hello standardplatsen för resurser i resursgruppen. Se till att alla kommandon som använder toocreate en Programgateway hello samma resursgrupp.

I föregående exempel hello, vi har skapat en resursgrupp med namnet ”appgw RG” och plats ”USA, västra”.

> [!NOTE]
> Om du behöver tooconfigure en anpassad avsökningsåtgärd för Application Gateway finns [skapa en Programgateway med anpassade avsökningar med hjälp av PowerShell](application-gateway-create-probe-ps.md). Mer information finns i [Anpassade avsökningar och hälsoövervakning](application-gateway-probe-overview.md).

### <a name="configure-virtual-network"></a>Konfigurera ett virtuellt nätverk

Programgateway kräver ett undernät egna. I det här steget skapar du ett virtuellt nätverk med ett adressutrymme för 10.0.0.0/16 och två undernät, en för hello Application Gateway och en för backend-poolmedlemmar.

```powershell
# Create a subnet configuration object for hello Application Gateway subnet. A subnet for an application should have a minimum of 28 mask bits. This value leaves 10 available addresses in hello subnet for Application Gateway instances. With a smaller subnet, you may not be able tooadd more instance of your Application Gateway in hello future.
$gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24

# Create a subnet configuration object for hello backend pool members subnet
$nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24

# Create hello virtual network with hello previous created subnets
$vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet
```

### <a name="configure-public-ip-address"></a>Konfigurera offentliga IP-adress

I ordning toohandle externa förfrågningar kräver Programgateway en offentlig IP-adress. Den här offentliga IP-adressen får inte ha en `DomainNameLabel` definierats toobe som används av hello Application Gateway.

```powershell
# Create a public IP address for use with hello Application Gateway. Defining hello domainnamelabel during creation is not supported for use with Application Gateway
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name 'appgwpip' -Location "West US" -AllocationMethod Dynamic
```

### <a name="configure-hello-application-gateway"></a>Konfigurera hello Programgateway

```powershell
# Create a IP configuration. This configures what subnet hello Application Gateway uses. When Application Gateway starts, it picks up an IP address from hello subnet configured and routes network traffic toohello IP addresses in hello back-end IP pool.
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet

# Create a backend pool toohold hello addresses or NICs for hello application that Application Gateway is protecting.
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3

# Upload hello authenication certificate that will be used toocommunicate with hello backend servers
$authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile <full path too.cer file>

# Conifugre hello backend HTTP settings toobe used toodefine how traffic is routed toohello backend pool. hello authenication certificate used in hello previous step is added toohello backend http settings.
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert

# Create a frontend port toobe used by hello listener.
$fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443

# Create a frontend IP configuration tooassociate hello public IP address with hello Application Gateway
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip

# Configure hello certificate for hello Application Gateway. This certificate is used toodecrypt and re-encrypt hello traffic on hello Application Gateway.
$cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path too.pfx file> -Password <password for certificate file>

# Create hello HTTP listener for hello Application Gateway. Assign hello front-end ip configuration, port, and ssl certificate toouse.
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert

#Create a load balancer routing rule that configures hello load balancer behavior. In this example, a basic round robin rule is created.
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

# Configure hello SKU of hello Application Gateway
$sku = New-AzureRmApplicationGatewaySku -Name WAF_Medium -Tier WAF -Capacity 2

# Define hello SSL policy toouse
$policy = New-AzureRmApplicationGatewaySslPolicy -PolicyType Predefined -PolicyName AppGwSslPolicy20170401S

#Configure hello waf configuration settings.
$config = New-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode "Prevention"

# Create hello Application Gateway utilizing all hello previously created configuration objects
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -WebApplicationFirewallConfig $config -SslCertificates $cert -AuthenticationCertificates $authcert
```

> [!NOTE]
> Programgatewayer som skapats med konfiguration av hello grundläggande webbprogram brandväggen konfigureras med CR 3.0 för skydd.

## <a name="get-application-gateway-dns-name"></a>Hämta program Gateway DNS-namn

När du har skapat hello gateway är hello nästa steg tooconfigure hello klientdelen för kommunikation. När du använder en offentlig IP-adress, kräver en dynamiskt tilldelad DNS-namn som inte är eget Application Gateway. tooensure slutanvändare kan träffa hello Programgateway, en CNAME-post kan vara används toopoint toohello offentlig slutpunkt för hello Application Gateway. [Konfigurera ett eget domännamn i Azure](../cloud-services/cloud-services-custom-domain-name-portal.md). tooconfigure ett alias, hämta information om hello Programgateway och dess associerade IP DNS-namn med hjälp av hello PublicIPAddress element kopplade toohello Application Gateway. hello Application Gateway DNS-namn ska använda toocreate en CNAME-post som pekar hello två web program toothis DNS-namn. hello rekommenderas A-poster inte eftersom hello VIP ändras vid omstart av Application Gateway.

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

Lär dig hur tooconfigure diagnostikloggning, toolog hello händelser som har identifierats eller förhindras med Brandvägg för webbaserade program genom att besöka [Gateway Programdiagnostik](application-gateway-diagnostics.md)

[scenario]: ./media/application-gateway-web-application-firewall-powershell/scenario.png
