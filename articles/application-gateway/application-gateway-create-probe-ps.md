---
title: "aaaCreate en anpassad avsökning - Azure Application Gateway - PowerShell | Microsoft Docs"
description: "Lär dig hur toocreate en anpassad avsökning för Programgateway med PowerShell i Resource Manager"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 68feb660-7fa4-4f69-a7e4-bdd7bdc474db
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: gwallace
ms.openlocfilehash: 44c9ffa75401d6d0db023e66fa82c701fb0cf8bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-custom-probe-for-azure-application-gateway-by-using-powershell-for-azure-resource-manager"></a>Skapa en anpassad avsökningsåtgärd för Programgateway i Azure med hjälp av PowerShell för Azure Resource Manager

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-probe-portal.md)
> * [PowerShell och Azure Resource Manager](application-gateway-create-probe-ps.md)
> * [PowerShell och den klassiska Azure-portalen](application-gateway-create-probe-classic-ps.md)

Lägg till en anpassad avsökningsåtgärd tooan befintliga Programgateway med PowerShell i den här artikeln. Anpassade avsökningar är användbara för program som har ett visst hälsotillstånd Kontrollera sidan eller för program som inte tillhandahåller ett lyckat svar på hello standardwebbprogrammet.

> [!NOTE]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md).  Den här artikeln täcker hello Resource Manager-distributionsmodellen, som Microsoft rekommenderar för de flesta nya distributioner i stället för hello [klassiska distributionsmodellen](application-gateway-create-probe-classic-ps.md).

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-an-application-gateway-with-a-custom-probe"></a>Skapa en Programgateway med en anpassad avsökningsåtgärd

### <a name="sign-in-and-create-resource-group"></a>Logga in och skapa resursgrupp

1. Använd `Login-AzureRmAccount` tooauthenticate.

  ```powershell
  Login-AzureRmAccount
  ```

1. Hämta hello prenumerationer för hello-kontot.

  ```powershell
  Get-AzureRmSubscription
  ```

1. Välj vilka av dina Azure-prenumerationer toouse.

  ```powershell
  Select-AzureRmSubscription -Subscriptionid '{subscriptionGuid}'
  ```

1. Skapa en resursgrupp. Du kan hoppa över det här steget om du har en befintlig resursgrupp.

  ```powershell
  New-AzureRmResourceGroup -Name appgw-rg -Location 'West US'
  ```

Azure Resource Manager kräver att alla resursgrupper anger en plats. Den här platsen används som hello standardplatsen för resurser i resursgruppen. Se till att alla kommandon toocreate ett program gateway används hello samma resursgrupp.

I föregående exempel hello, vi har skapat en resursgrupp med namnet **appgw RG** plats **västra USA**.

### <a name="create-a-virtual-network-and-a-subnet"></a>Skapa ett virtuellt nätverk och ett undernät

hello följande exempel skapas ett virtuellt nätverk och ett undernät för hello Programgateway. Programgateway kräver sin egen undernät för användning. Hello-undernät som skapats för hello Programgateway bör vara mindre än hello adressutrymme hello VNET tooallow för andra undernät toobe skapas och används därför.

```powershell
# Assign hello address range 10.0.0.0/24 tooa subnet variable toobe used toocreate a virtual network.
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

# Create a virtual network named appgwvnet in resource group appgw-rg for hello West US region using hello prefix 10.0.0.0/16 with subnet 10.0.0.0/24.
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $subnet

# Assign a subnet variable for hello next steps, which create an application gateway.
$subnet = $vnet.Subnets[0]
```

### <a name="create-a-public-ip-address-for-hello-front-end-configuration"></a>Skapa en offentlig IP-adress för hello frontend-konfiguration

Skapa en offentlig IP-resurs **publicIP01** i resursgruppen **appgw rg** för hello västra USA region. Det här exemplet används en offentlig IP-adress för hello frontend IP-adress hello Programgateway.  Programgateway kräver hello offentliga IP-adress toohave ett dynamiskt skapade DNS-namn därför hello `-DomainNameLabel` kan inte anges under hello skapandet av hello offentlig IP-adress.

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -Name publicIP01 -Location 'West US' -AllocationMethod Dynamic
```

### <a name="create-an-application-gateway"></a>Skapa en programgateway

Du ställa in alla konfigurationsobjekt innan du skapar hello Programgateway. hello skapar följande exempel hello konfigurationsobjekt som behövs för en gateway programresursen.

| **Komponent** | **Beskrivning** |
|---|---|
| **Gateway-IP-konfiguration** | En IP-konfiguration för en Programgateway.|
| **Serverdelspool** | En pool med IP-adresser, FQDN eller nätverkskort som är toohello programservrar som värd för hello webbprogrammet|
| **Hälsoavsökningen** | En anpassad avsökning används toomonitor hello hälsotillstånd hello backend-poolmedlemmar|
| **HTTP-inställningar** | En samling inställningar inklusive, port, protokoll, cookie-baserad tillhörighet, avsökning och timeout.  De här inställningarna avgör hur trafiken är routade toohello backend-poolmedlemmar|
| **Klientdelsport** | hello-port som hello Programgateway lyssnar efter trafik på|
| **Lyssnare** | En kombination av protokoll, klientdelens IP-konfiguration och klientdelsport. Det här är vad lyssnar efter inkommande begäranden.
|**Regeln**| Vägar hello trafik toohello lämplig backend baserat på HTTP-inställningar.|

```powershell
# Creates a application gateway Frontend IP configuration named gatewayIP01
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

#Creates a back-end IP address pool named pool01 with IP addresses 134.170.185.46, 134.170.188.221, 134.170.185.50.
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221, 134.170.185.50

# Creates a probe that will check health at http://contoso.com/path/path.htm
$probe = New-AzureRmApplicationGatewayProbeConfig -Name probe01 -Protocol Http -HostName 'contoso.com' -Path '/path/path.htm' -Interval 30 -Timeout 120 -UnhealthyThreshold 8

# Creates hello backend http settings toobe used. This component references hello $probe created in hello previous command.
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 80

# Creates a frontend port for hello application gateway toolisten on port 80 that will be used by hello listener.
$fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01 -Port 80

# Creates a frontend IP configuration. This associates hello $publicip variable defined previously with hello front-end IP that will be used by hello listener.
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

# Creates hello listener. hello listener is a combination of protocol and hello frontend IP configuration $fipconfig and frontend port $fp created in previous steps.
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

# Creates hello rule that routes traffic toohello backend pools.  In this example we create a basic rule that uses hello previous defined http settings and backend address pool.  It also associates hello listener toohello rule
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

# Sets hello SKU of hello application gateway, in this example we create a small standard application gateway with 2 instances.
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

# hello final step creates hello application gateway with all hello previously defined components.
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location 'West US' -BackendAddressPools $pool -Probes $probe -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku
```

## <a name="add-a-probe-tooan-existing-application-gateway"></a>Lägg till en avsökning tooan befintliga Programgateway

hello följande kodavsnitt lägger till en avsökning tooan befintliga Programgateway.

```powershell
# Load hello application gateway resource into a PowerShell variable by using Get-AzureRmApplicationGateway.
$getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

# Create hello probe object that will check health at http://contoso.com/path/path.htm
$getgw = Add-AzureRmApplicationGatewayProbeConfig -ApplicationGateway $getgw -Name probe01 -Protocol Http -HostName 'contoso.com' -Path '/path/custompath.htm' -Interval 30 -Timeout 120 -UnhealthyThreshold 8

# Set hello backend HTTP settings toouse hello new probe
$getgw = Set-AzureRmApplicationGatewayBackendHttpSettings -ApplicationGateway $getgw -Name $getgw.BackendHttpSettingsCollection.name -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 120

# Save hello application gateway with hello configuration changes
Set-AzureRmApplicationGateway -ApplicationGateway $getgw
```

## <a name="remove-a-probe-from-an-existing-application-gateway"></a>Ta bort en avsökning från en befintlig Programgateway

hello följande kodavsnitt tar bort en avsökning från en befintlig gateway för programmet.

```powershell
# Load hello application gateway resource into a PowerShell variable by using Get-AzureRmApplicationGateway.
$getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

# Remove hello probe from hello application gateway configuration object
$getgw = Remove-AzureRmApplicationGatewayProbeConfig -ApplicationGateway $getgw -Name $getgw.Probes.name

# Set hello backend HTTP settings tooremove hello reference toohello probe. hello backend http settings now use hello default probe
$getgw = Set-AzureRmApplicationGatewayBackendHttpSettings -ApplicationGateway $getgw -Name $getgw.BackendHttpSettingsCollection.name -Port 80 -Protocol http -CookieBasedAffinity Disabled

# Save hello application gateway with hello configuration changes
Set-AzureRmApplicationGateway -ApplicationGateway $getgw
```

## <a name="get-application-gateway-dns-name"></a>Hämta DNS-namn för programgatewayen

När du har skapat hello gateway är hello nästa steg tooconfigure hello klientdelen för kommunikation. När du använder en offentlig IP-adress krävs ett dynamiskt tilldelat DNS-namn som inte är användarvänligt. tooensure slutanvändare kan träffa hello Programgateway en CNAME-post kan användas för toopoint toohello offentlig slutpunkt för hello Programgateway. [Konfigurera ett eget domännamn i Azure](../cloud-services/cloud-services-custom-domain-name-portal.md). toodo detta, hämta information om hello Programgateway och dess associerade IP DNS-namn med hjälp av hello PublicIPAddress element bifogade toohello Programgateway. hello programmet gateway DNS-namn ska använda toocreate en CNAME-post som pekar hello två web program toothis DNS-namn. hello rekommenderas A-poster inte eftersom hello VIP ändras vid omstart för Programgateway.

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

Läs tooconfigure SSL genom att avlasta genom att besöka: [Konfigurera SSL-avlastning](application-gateway-ssl-arm.md)

