---
title: aaaCreate, starta, eller ta bort en Programgateway | Microsoft Docs
description: "Den här sidan innehåller instruktioner toocreate, konfigurera, starta och ta bort en Azure Programgateway"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 577054ca-8368-4fbf-8d53-a813f29dc3bc
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: gwallace
ms.openlocfilehash: 3efef5b49880c9efdafad8b88d4bce5b749b82af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-start-or-delete-an-application-gateway-with-powershell"></a>Skapa, starta eller ta bort en programgateway med PowerShell 

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-gateway-portal.md)
> * [PowerShell och Azure Resource Manager](application-gateway-create-gateway-arm.md)
> * [PowerShell och den klassiska Azure-portalen](application-gateway-create-gateway.md)
> * [Azure Resource Manager-mall](application-gateway-create-gateway-arm-template.md)
> * [Azure CLI](application-gateway-create-gateway-cli.md)

Azure Application Gateway är en Layer 7-belastningsutjämnare. Det ger redundans, prestanda-routing HTTP-begäranden mellan olika servrar, oavsett om de är på hello molnet eller lokalt. Application Gateway innehåller många ADC-funktioner (Application Delivery Controller), inklusive HTTP-belastningsutjämning, cookie-baserad sessionstilldelning, SSL-avlastning (Secure Sockets Layer), anpassade hälsoavsökningar, stöd för flera platser och mycket mer. toofind en fullständig lista över funktioner som stöds finns [Gateway Programöversikt](application-gateway-introduction.md)

Den här artikeln vägleder dig genom hello steg toocreate, konfigurera, starta och ta bort en Programgateway.

## <a name="before-you-begin"></a>Innan du börjar

1. Installera hello senaste versionen av hello Azure PowerShell-cmdlets med hello installationsprogram för webbplattform. Du kan hämta och installera hello senaste versionen från hello **Windows PowerShell** avsnitt i hello [Nedladdningssida](https://azure.microsoft.com/downloads/).
2. Om du har ett befintligt virtuellt nätverk, Välj ett befintligt tomma undernät eller skapa ett nytt undernät i ett befintligt virtuellt nätverk enbart för användning med hello Programgateway. Du kan inte distribuera hello programmet gateway tooa annat virtuellt nätverk än hello resurser du avser toodeploy bakom hello Programgateway såvida inte vnet-peering används. toolearn finns mer [Vnet-Peering](../virtual-network/virtual-network-peering-overview.md)
3. Kontrollera att du har ett fungerande virtuellt nätverk med ett giltigt undernät. Se till att inga virtuella datorer eller molndistributioner använder hello undernät. hello programmet gateway måste finnas ensamt i ett undernät för virtuellt nätverk.
4. hello-servrar som du konfigurerar toouse hello Programgateway måste finnas eller tilldelats deras slutpunkter har skapats i hello virtuellt nätverk eller med en offentlig IP-adress/VIP.

## <a name="what-is-required-toocreate-an-application-gateway"></a>Vad är obligatoriska toocreate en Programgateway?

När du använder hello `New-AzureApplicationGateway` kommandot toocreate hello Programgateway, ingen konfiguration är nu och hello nyligen skapade resursen är konfigurerade antingen med hjälp av XML- eller ett konfigurationsobjekt.

hello-värden är:

* **Backend-serverpoolen:** hello lista över IP-adresser för hello backend-servrar. hello IP-adresser som anges antingen ska tillhöra toohello undernät för virtuellt nätverk eller ska vara en offentlig IP-adress/VIP.
* **Inställningar för backend-serverpool:** Varje pool har inställningar som port, protokoll och cookiebaserad tillhörighet. De här inställningarna är bundet tooa poolen och tillämpade tooall servrar inom hello poolen.
* **Frontend-port:** den här porten är hello offentliga som öppnas på hello Programgateway. Trafik träffar den här porten och sedan hämtar omdirigeras tooone hello backend-servrar.
* **Lyssnare:** hello-lyssnare har en frontend-port, ett protokoll (Http eller Https dessa värden är skiftlägeskänsligt), och hello SSL-certifikatnamn (om hur du konfigurerar SSL-avlastning).
* **Regel:** hello regeln Binder hello-lyssnare och hello backend-serverpoolen och definierar vilken backend-server pool hello trafik ska vara riktad toowhen den når en viss lyssnare.

## <a name="create-an-application-gateway"></a>Skapa en programgateway

toocreate en Programgateway:

1. Skapa en resurs för en programgateway.
2. Skapa en XML-konfigurationsfil eller ett konfigurationsobjekt.
3. Bekräfta hello configuration toohello nyskapad programresursen gateway.

> [!NOTE]
> Om du behöver tooconfigure en anpassad avsökningsåtgärd för din Programgateway finns [skapa en Programgateway med anpassade avsökningar med hjälp av PowerShell](application-gateway-create-probe-classic-ps.md). Mer information finns i [Anpassade avsökningar och hälsoövervakning](application-gateway-probe-overview.md).

![Exempel på ett scenario][scenario]

### <a name="create-an-application-gateway-resource"></a>Skapa en resurs för en programgateway

Använd hello-toocreate hello gateway `New-AzureApplicationGateway` cmdlet, ersätter hello värden med dina egna. Fakturering för hello gatewayen startar inte nu. Fakturering börjar i ett senare steg när hello gateway har startat.

hello följande exempel skapas en Programgateway med hjälp av ett virtuellt nätverk kallas ”testvnet1” och ett undernät som kallas ”Undernät 1”:

```powershell
New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")
```

*Description*, *InstanceCount* och *GatewaySize* är valfria parametrar.

toovalidate som hello gateway har skapats kan du använda hello `Get-AzureApplicationGateway` cmdlet.

```powershell
Get-AzureApplicationGateway AppGwTest
```

```
Name          : AppGwTest
Description   :
VnetName      : testvnet1
Subnets       : {Subnet-1}
InstanceCount : 2
GatewaySize   : Medium
State         : Stopped
VirtualIPs    : {}
DnsName       :
```

> [!NOTE]
> Hej standardvärdet för *InstanceCount* är 2, med ett maximalt värde 10. Hej standardvärdet för *GatewaySize* är Medium. Du kan välja mellan Small, Medium eller Large.

*Virtualip:* och *DnsName* visas som tomt eftersom hello gateway inte har startat ännu. Dessa skapas när hello gateway är i hello körs.

## <a name="configure-hello-application-gateway"></a>Konfigurera hello Programgateway

Du kan konfigurera hello Programgateway med hjälp av XML- eller ett konfigurationsobjekt.

### <a name="configure-hello-application-gateway-by-using-xml"></a>Konfigurera hello Programgateway genom att använda XML

I följande exempel hello, och använda en XML-filen tooconfigure alla inställningar för Programgateway och bekräfta dem toohello programresursen gateway.  

#### <a name="step-1"></a>Steg 1

Kopiera följande text tooNotepad hello.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
    <FrontendPorts>
        <FrontendPort>
            <Name>(name-of-your-frontend-port)</Name>
            <Port>(port number)</Port>
        </FrontendPort>
    </FrontendPorts>
    <BackendAddressPools>
        <BackendAddressPool>
            <Name>(name-of-your-backend-pool)</Name>
            <IPAddresses>
                <IPAddress>(your-IP-address-for-backend-pool)</IPAddress>
                <IPAddress>(your-second-IP-address-for-backend-pool)</IPAddress>
            </IPAddresses>
        </BackendAddressPool>
    </BackendAddressPools>
    <BackendHttpSettingsList>
        <BackendHttpSettings>
            <Name>(backend-setting-name-to-configure-rule)</Name>
            <Port>80</Port>
            <Protocol>[Http|Https]</Protocol>
            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
        </BackendHttpSettings>
    </BackendHttpSettingsList>
    <HttpListeners>
        <HttpListener>
            <Name>(name-of-the-listener)</Name>
            <FrontendPort>(name-of-your-frontend-port)</FrontendPort>
            <Protocol>[Http|Https]</Protocol>
        </HttpListener>
    </HttpListeners>
    <HttpLoadBalancingRules>
        <HttpLoadBalancingRule>
            <Name>(name-of-load-balancing-rule)</Name>
            <Type>basic</Type>
            <BackendHttpSettings>(backend-setting-name-to-configure-rule)</BackendHttpSettings>
            <Listener>(name-of-the-listener)</Listener>
            <BackendAddressPool>(name-of-your-backend-pool)</BackendAddressPool>
        </HttpLoadBalancingRule>
    </HttpLoadBalancingRules>
</ApplicationGatewayConfiguration>
```

Redigera hello värden mellan hello parenteser för hello konfigurationsobjekt. Spara hello-filen med filnamnstillägget .xml.

> [!IMPORTANT]
> hello protokollet objektet Http eller Https är skiftlägeskänsliga.

hello som följande exempel visar hur toouse en konfiguration filen tooset in hello Programgateway. hello exempel belastningen balanserar HTTP-trafik på offentlig port 80 och skickar nätverkstrafik tooback avslutande port 80 mellan två IP-adresser.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
    <FrontendPorts>
        <FrontendPort>
            <Name>FrontendPort1</Name>
            <Port>80</Port>
        </FrontendPort>
    </FrontendPorts>
    <BackendAddressPools>
        <BackendAddressPool>
            <Name>BackendPool1</Name>
            <IPAddresses>
                <IPAddress>10.0.0.1</IPAddress>
                <IPAddress>10.0.0.2</IPAddress>
            </IPAddresses>
        </BackendAddressPool>
    </BackendAddressPools>
    <BackendHttpSettingsList>
        <BackendHttpSettings>
            <Name>BackendSetting1</Name>
            <Port>80</Port>
            <Protocol>Http</Protocol>
            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
        </BackendHttpSettings>
    </BackendHttpSettingsList>
    <HttpListeners>
        <HttpListener>
            <Name>HTTPListener1</Name>
            <FrontendPort>FrontendPort1</FrontendPort>
            <Protocol>Http</Protocol>
        </HttpListener>
    </HttpListeners>
    <HttpLoadBalancingRules>
        <HttpLoadBalancingRule>
            <Name>HttpLBRule1</Name>
            <Type>basic</Type>
            <BackendHttpSettings>BackendSetting1</BackendHttpSettings>
            <Listener>HTTPListener1</Listener>
            <BackendAddressPool>BackendPool1</BackendAddressPool>
        </HttpLoadBalancingRule>
    </HttpLoadBalancingRules>
</ApplicationGatewayConfiguration>
```

#### <a name="step-2"></a>Steg 2

Ange därefter hello Programgateway. Använd hello `Set-AzureApplicationGatewayConfig` med en XML-konfigurationsfilen.

```powershell
Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile "D:\config.xml"
```

### <a name="configure-hello-application-gateway-by-using-a-configuration-object"></a>Konfigurera hello Programgateway med hjälp av ett konfigurationsobjekt

hello som följande exempel visar hur tooconfigure hello Programgateway med hjälp av konfigurationsobjekt. Alla konfigurationsobjekt måste konfigureras individuellt och sedan läggs tooan programobjektet gateway-konfiguration. Efter att hello konfigurationsobjekt kan du använda hello `Set-AzureApplicationGateway` kommandot toocommit hello configuration toohello skapat programmet gatewayresursen.

> [!NOTE]
> Innan du tilldelar ett värde tooeach konfigurationsobjekt, måste du toodeclare vilken typ av objekt PowerShell använder för lagring. hello första rad toocreate hello enskilda objekt som definierar vad `Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model(object name)` används.

#### <a name="step-1"></a>Steg 1

Skapa alla enskilda konfigurationsobjekt.

Skapa hello frontend IP som visas i följande exempel hello.

```powershell
$fip = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration
$fip.Name = "fip1"
$fip.Type = "Private"
$fip.StaticIPAddress = "10.0.0.5"
```

Skapa hello frontend-port som visas i följande exempel hello.

```powershell
$fep = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort
$fep.Name = "fep1"
$fep.Port = 80
```

Skapa hello backend-servern i serverpoolen.

Definiera hello IP-adresser som har lagts till toohello backend-serverpoolen enligt hello nästa exempel.

```powershell
$servers = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendServerCollection
$servers.Add("10.0.0.1")
$servers.Add("10.0.0.2")
```

Använda hello $server tooadd hello värden toohello backend-adresspool objekt ($pool).

```powershell
$pool = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool
$pool.BackendServers = $servers
$pool.Name = "pool1"
```

Skapa hello backend-server pool inställning.

```powershell
$setting = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings
$setting.Name = "setting1"
$setting.CookieBasedAffinity = "enabled"
$setting.Port = 80
$setting.Protocol = "http"
```

Skapa hello lyssnare.

```powershell
$listener = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener
$listener.Name = "listener1"
$listener.FrontendPort = "fep1"
$listener.FrontendIP = "fip1"
$listener.Protocol = "http"
$listener.SslCert = ""
```

Skapa hello regel.

```powershell
$rule = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule
$rule.Name = "rule1"
$rule.Type = "basic"
$rule.BackendHttpSettings = "setting1"
$rule.Listener = "listener1"
$rule.BackendAddressPool = "pool1"
```

#### <a name="step-2"></a>Steg 2

Tilldela alla enskilda objekt tooan programmet gateway konfiguration konfigurationsobjektet ($appgwconfig).

Lägg till hello frontend IP toohello konfiguration.

```powershell
$appgwconfig = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.ApplicationGatewayConfiguration
$appgwconfig.FrontendIPConfigurations = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration]"
$appgwconfig.FrontendIPConfigurations.Add($fip)
```

Lägg till hello frontend toohello portkonfigurationen.

```powershell
$appgwconfig.FrontendPorts = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort]"
$appgwconfig.FrontendPorts.Add($fep)
```
Lägg till hello backend-adresspool toohello serverkonfiguration.

```powershell
$appgwconfig.BackendAddressPools = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool]"
$appgwconfig.BackendAddressPools.Add($pool)
```

Lägg till hello backend-adresspool inställningen toohello konfiguration.

```powershell
$appgwconfig.BackendHttpSettingsList = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings]"
$appgwconfig.BackendHttpSettingsList.Add($setting)
```

Lägg till hello lyssnare toohello konfiguration.

```powershell
$appgwconfig.HttpListeners = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener]"
$appgwconfig.HttpListeners.Add($listener)
```

Lägg till hello toohello regelkonfigurationen.

```powershell
$appgwconfig.HttpLoadBalancingRules = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule]"
$appgwconfig.HttpLoadBalancingRules.Add($rule)
```

### <a name="step-3"></a>Steg 3
Genomför hello configuration-objekt toohello gateway programresursen med hjälp av `Set-AzureApplicationGatewayConfig`.

```powershell
Set-AzureApplicationGatewayConfig -Name AppGwTest -Config $appgwconfig
```

## <a name="start-hello-gateway"></a>Starta hello gateway

När hello gateway har konfigurerats kan du använda hello `Start-AzureApplicationGateway` cmdlet toostart hello gateway. Fakturering för en Programgateway startar när hello gatewayen har startats.

> [!NOTE]
> Hej `Start-AzureApplicationGateway` cmdlet kan ta upp toofinish too15 20 minuter.

```powershell
Start-AzureApplicationGateway AppGwTest
```

## <a name="verify-hello-gateway-status"></a>Verifiera hello gatewayens status

Använd hello `Get-AzureApplicationGateway` cmdlet toocheck hello status för hello gateway. Om `Start-AzureApplicationGateway` lyckades hello föregående steg, *tillstånd* ska köras och *Vip* och *DnsName* ska ha giltiga poster.

hello följande exempel visas en Programgateway som är tillgängliga, körs och är redo tootake trafik avsedd för `http://<generated-dns-name>.cloudapp.net`.

```powershell
Get-AzureApplicationGateway AppGwTest
```

```
VERBOSE: 8:09:28 PM - Begin Operation: Get-AzureApplicationGateway
VERBOSE: 8:09:30 PM - Completed Operation: Get-AzureApplicationGateway
Name          : AppGwTest
Description   :
VnetName      : testvnet1
Subnets       : {Subnet-1}
InstanceCount : 2
GatewaySize   : Medium
State         : Running
Vip           : 138.91.170.26
DnsName       : appgw-1b8402e8-3e0d-428d-b661-289c16c82101.cloudapp.net
```

## <a name="delete-hello-application-gateway"></a>Ta bort hello Programgateway

toodelete hello Programgateway:

1. Använd hello `Stop-AzureApplicationGateway` cmdlet toostop hello gateway.
2. Använd hello `Remove-AzureApplicationGateway` cmdlet tooremove hello gateway.
3. Kontrollera att hello-gateway har tagits bort med hjälp av hello `Get-AzureApplicationGateway` cmdlet.

hello följande exempel visar hello `Stop-AzureApplicationGateway` cmdlet på hello första rad, följt av hello utdata.

```powershell
Stop-AzureApplicationGateway AppGwTest
```

```
VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error
----       ----------------     ------------                             ----
Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8
```

När hello Programgateway är i ett stoppat tillstånd, använder hello `Remove-AzureApplicationGateway` cmdlet tooremove hello-tjänsten.

```powershell
Remove-AzureApplicationGateway AppGwTest
```

```
VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error
----       ----------------     ------------                             ----
Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301
```

tooverify som hello tjänsten har tagits bort kan du använda hello `Get-AzureApplicationGateway` cmdlet. Det här steget är inte obligatoriskt.

```powershell
Get-AzureApplicationGateway AppGwTest
```

```
VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

Get-AzureApplicationGateway : ResourceNotFound: hello gateway does not exist.
.....
```

## <a name="next-steps"></a>Nästa steg

Om du vill tooconfigure SSL-avlastning, se [konfigurera en Programgateway för SSL-avlastning](application-gateway-ssl.md).

Om du vill tooconfigure ett program gateway toouse med en intern belastningsutjämnare, se [skapa en Programgateway med en intern belastningsutjämnare (ILB)](application-gateway-ilb.md).

Om du vill ha mer information om belastningsutjämningsalternativ i allmänhet läser du:

* [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

[scenario]: ./media/application-gateway-create-gateway/scenario.png
