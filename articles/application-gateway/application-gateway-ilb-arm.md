---
title: "aaaUsing Azure Programgateway med interna belastningsutjämnare - PowerShell | Microsoft Docs"
description: "Den här sidan innehåller instruktioner toocreate, konfigurera, starta och ta bort en Azure Programgateway med intern belastningsutjämnare (ILB) för Azure Resource Manager"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 75cfd5a2-e378-4365-99ee-a2b2abda2e0d
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: gwallace
ms.openlocfilehash: dd0d7e954b1fa219ae6ebe42cb4b479dbcf08653
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-with-an-internal-load-balancer-ilb-by-using-azure-resource-manager"></a>Skapa en programgateway med en intern belastningsutjämnare (ILB) med hjälp av Azure Resource Manager

> [!div class="op_single_selector"]
> * [PowerShell och den klassiska Azure-portalen](application-gateway-ilb.md)
> * [PowerShell och Azure Resource Manager](application-gateway-ilb-arm.md)

Azure Application Gateway kan konfigureras med en Internet-riktade VIP eller med en intern slutpunkt som inte är synliga toohello Internet, även kallat en intern belastningsutjämnare (ILB) slutpunkt. Konfigurera hello gateway med en ILB är användbart för interna line-of-business-program som inte är synliga toohello Internet. Det är också användbart för tjänster och nivåer inom en flernivåapp som sitter i en säkerhetsgräns som inte är synliga toohello Internet men fortfarande kräver resursallokering belastningsutjämna distribution, varaktighet för sessionen eller Secure Sockets Layer (SSL)-avslutning.

Den här artikeln vägleder dig genom hello steg tooconfigure en Programgateway med en ILB.

## <a name="before-you-begin"></a>Innan du börjar

1. Installera hello senaste versionen av hello Azure PowerShell-cmdlets med hello installationsprogram för webbplattform. Du kan hämta och installera hello senaste versionen från hello **Windows PowerShell** avsnitt i hello [Nedladdningssida](https://azure.microsoft.com/downloads/).
2. Du ska skapa ett virtuellt nätverk och ett undernät för Application Gateway. Se till att inga virtuella datorer eller molndistributioner använder hello undernät. Application Gateway måste vara fristående i ett virtuellt nätverks undernät.
3. hello-servrar som du konfigurerar toouse hello Programgateway måste finnas eller tilldelats deras slutpunkter har skapats i hello virtuellt nätverk eller med en offentlig IP-adress/VIP.

## <a name="what-is-required-toocreate-an-application-gateway"></a>Vad är obligatoriska toocreate en Programgateway?

* **Backend-serverpoolen:** hello lista över IP-adresser för hello backend-servrar. hello IP-adresser i listan ska antingen tillhöra toohello virtuellt nätverk, men i ett annat undernät för hello Programgateway eller ska vara en offentlig IP-adress/VIP.
* **Inställningar för backend-serverpool:** Varje pool har inställningar som port, protokoll och cookiebaserad tillhörighet. De här inställningarna är bundet tooa poolen och tillämpade tooall servrar inom hello poolen.
* **Frontend-port:** den här porten är hello offentliga som öppnas på hello Programgateway. Trafik träffar den här porten och sedan hämtar omdirigeras tooone hello backend-servrar.
* **Lyssnare:** hello-lyssnare har en frontend-port, ett protokoll (Http eller Https, dessa är skiftlägeskänsligt), och hello SSL-certifikatnamn (om hur du konfigurerar SSL-avlastning).
* **Regel:** hello regeln Binder hello-lyssnare och hello backend-serverpoolen och definierar vilken backend-server pool hello trafik ska vara riktad toowhen den når en viss lyssnare. För närvarande endast hello *grundläggande* regeln stöds. Hej *grundläggande* regeln är resursallokering belastningsdistribution.

## <a name="create-an-application-gateway"></a>Skapa en programgateway

hello skillnaden mellan att använda den klassiska Azure och Azure Resource Manager är hello order som du kan skapa hello Programgateway och hello-objekt som behöver toobe konfigurerats.
Med Resource Manager alla poster i en Programgateway konfigureras individuellt och sedan sätta ihop toocreate hello programresursen gateway.

Här följer hello steg som är nödvändiga toocreate en Programgateway:

1. Skapa en resursgrupp för Resource Manager
2. Skapa ett virtuellt nätverk och ett undernät för hello Programgateway
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

Skapa en ny resursgrupp (hoppa över detta steg om du använder en befintlig resursgrupp).

```powershell
New-AzureRmResourceGroup -Name appgw-rg -location "West US"
```

Azure Resource Manager kräver att alla resursgrupper anger en plats. Detta används som hello standardplatsen för resurser i resursgruppen. Se till att alla kommandon toocreate använder en Programgateway hello samma resursgrupp.

I föregående exempel hello, skapat vi en resursgrupp med namnet ”appgw rg” och plats ”USA, västra”.

## <a name="create-a-virtual-network-and-a-subnet-for-hello-application-gateway"></a>Skapa ett virtuellt nätverk och ett undernät för hello Programgateway

följande exempel visar hur hello toocreate ett virtuellt nätverk med hjälp av hanteraren för filserverresurser:

### <a name="step-1"></a>Steg 1

```powershell
$subnetconfig = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24
```

Det här steget tilldelas hello adressintervallet 10.0.0.0/24 tooa undernät variabeln toobe används toocreate ett virtuellt nätverk.

### <a name="step-2"></a>Steg 2

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnetconfig
```

Det här steget skapar ett virtuellt nätverk med namnet ”appgwvnet” i resursen grupp ”appgw-rg” för hello västra USA region med undernätet 10.0.0.0/24 hello prefixet 10.0.0.0/16.

### <a name="step-3"></a>Steg 3

```powershell
$subnet = $vnet.subnets[0]
```

Det här steget tilldelas hello undernät objektet toovariable $subnet hello nästa steg.

## <a name="create-an-application-gateway-configuration-object"></a>Skapa ett konfigurationsobjekt för programgatewayen

### <a name="step-1"></a>Steg 1

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet
```

Det här steget skapar ett program gateway IP-konfiguration med namnet ”gatewayIP01”. När Programgateway startar hämtar en IP-adress från hello-undernät som konfigurerats och dirigera trafik toohello IP-adresser på nätverket i hello backend-IP-adresspool. Tänk på att varje instans använder en IP-adress.

### <a name="step-2"></a>Steg 2

```powershell
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 10.1.1.8,10.1.1.9,10.1.1.10
```

Det här steget konfigurerar hello backend-IP-adresspool med namnet ”pool01” med IP-adresser ”10.1.1.8, 10.1.1.9, 10.1.1.10”. De är hello IP-adresser som tar emot hello nätverkstrafik som kommer från hello frontend IP-slutpunkt. Ersätt hello föregående IP-adresser tooadd egna programslutpunkter IP-adress.

### <a name="step-3"></a>Steg 3

```powershell
$poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled
```

Det här steget konfigurerar programmet gateway inställningen ”poolsetting01” för hello belastningsutjämnade trafik i hello backend-adresspool.

### <a name="step-4"></a>Steg 4

```powershell
$fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80
```

Det här steget konfigurerar hello frontend IP-port med namnet ”frontendport01” för hello ILB.

### <a name="step-5"></a>Steg 5

```powershell
$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -Subnet $subnet
```

Det här steget skapar hello frontend IP-konfiguration som kallas ”fipconfig01” och associerar den med en privat IP-adress från hello aktuella undernät för virtuellt nätverk.

### <a name="step-6"></a>Steg 6

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp
```

Det här steget skapar hello lyssnare som kallas ”listener01” och associerar hello frontend port toohello frontend IP-konfiguration.

### <a name="step-7"></a>Steg 7

```powershell
$rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
```

Det här steget skapar hello routning belastningsutjämningsregeln kallas ”rule01” som konfigurerar hello belastningsutjämnaren GUID.

### <a name="step-8"></a>Steg 8

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
```

Det här steget konfigurerar hello instansstorleken för hello Programgateway.

> [!NOTE]
> Hej standardvärdet för *InstanceCount* är 2, med ett maximalt värde 10. Hej standardvärdet för *GatewaySize* är Medium. Du kan välja mellan Standard_Small, Standard_Medium och Standard_Large.

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a>Skapa en programgateway med hjälp av New-AzureApplicationGateway

Skapar en Programgateway med alla konfigurationsobjekt från hello föregående steg. I det här exemplet kallas hello Programgateway ”appgwtest”.

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku
```

Det här steget skapar en Programgateway med alla konfigurationsobjekt från hello föregående steg. I exemplet hello kallas hello Programgateway ”appgwtest”.

## <a name="delete-an-application-gateway"></a>Ta bort en programgateway

toodelete en Programgateway måste toodo hello följa stegen i ordning:

1. Använd hello `Stop-AzureRmApplicationGateway` cmdlet toostop hello gateway.
2. Använd hello `Remove-AzureRmApplicationGateway` cmdlet tooremove hello gateway.
3. Kontrollera att hello-gateway har tagits bort med hjälp av hello `Get-AzureApplicationGateway` cmdlet.

### <a name="step-1"></a>Steg 1

Hämta hello programobjektet gateway och associera den tooa variabeln ”$getgw”.

```powershell
$getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg
```

### <a name="step-2"></a>Steg 2

Använd `Stop-AzureRmApplicationGateway` toostop hello Programgateway. Det här exemplet visar hello `Stop-AzureRmApplicationGateway` cmdlet på hello första rad, följt av hello utdata.

```powershell
Stop-AzureRmApplicationGateway -ApplicationGateway $getgw  
```

```
VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error
----       ----------------     ------------                             ----
Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8
```

När hello Programgateway är i ett stoppat tillstånd, använder hello `Remove-AzureRmApplicationGateway` cmdlet tooremove hello-tjänsten.

```powershell
Remove-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Force
```

```
VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error
----       ----------------     ------------                             ----
Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301
```

> [!NOTE]
> Hej **-tvinga** växel kan vara används toosuppress hello ta bort bekräftelsemeddelande.

tooverify som hello tjänsten har tagits bort kan du använda hello `Get-AzureRmApplicationGateway` cmdlet. Det här steget är inte obligatoriskt.

```powershell
Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg
```

```
VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

Get-AzureApplicationGateway : ResourceNotFound: hello gateway does not exist.
```

## <a name="next-steps"></a>Nästa steg

Om du vill tooconfigure SSL-avlastning, se [konfigurera en Programgateway för SSL-avlastning](application-gateway-ssl.md).

Om du vill tooconfigure ett program gateway toouse med en ILB finns [skapa en Programgateway med en intern belastningsutjämnare (ILB)](application-gateway-ilb.md).

Om du vill ha mer information om belastningsutjämningsalternativ i allmänhet läser du:

* [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

