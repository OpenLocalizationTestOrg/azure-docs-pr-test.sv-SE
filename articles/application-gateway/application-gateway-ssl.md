---
title: aaaConfigure SSL-avlastning klassiska - Azure Application Gateway - PowerShell | Microsoft Docs
description: "Den här artikeln innehåller instruktioner som toocreate en Programgateway med SSL avlasta med hjälp av hello Azure klassiska distributionsmodellen."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 63f28d96-9c47-410e-97dd-f5ca1ad1b8a4
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: gwallace
ms.openlocfilehash: 5cb128015747ed4b71802cf751c80b60634601a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-hello-classic-deployment-model"></a>Konfigurera en Programgateway för SSL-avlastning med hjälp av hello klassiska distributionsmodellen

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-ssl-portal.md)
> * [PowerShell och Azure Resource Manager](application-gateway-ssl-arm.md)
> * [PowerShell och den klassiska Azure-portalen](application-gateway-ssl.md)
> * [Azure CLI 2.0](application-gateway-ssl-cli.md)

Azure Application Gateway kan vara konfigurerade tooterminate hello Secure Sockets Layer (SSL)-session på hello gateway tooavoid kostsamma SSL dekryptering uppgifter toohappen på hello webbservergrupp. SSL-avlastning förenklar också hello front-end-serverinstallation och hantering av hello webbprogram.

## <a name="before-you-begin"></a>Innan du börjar

1. Installera hello senaste versionen av hello Azure PowerShell-cmdlets med hello installationsprogram för webbplattform. Du kan hämta och installera hello senaste versionen från hello **Windows PowerShell** avsnitt i hello [Nedladdningssida](https://azure.microsoft.com/downloads/).
2. Kontrollera att du har ett fungerande virtuellt nätverk med ett giltigt undernät. Se till att inga virtuella datorer eller molndistributioner använder hello undernät. hello programmet gateway måste finnas ensamt i ett undernät för virtuellt nätverk.
3. hello-servrar som du konfigurerar toouse hello Programgateway måste finnas eller tilldelats deras slutpunkter har skapats i hello virtuellt nätverk eller med en offentlig IP-adress/VIP.

tooconfigure SSL avlasta på en Programgateway, hello följa stegen i ordning hello:

1. [Skapa en Programgateway](#create-an-application-gateway)
2. [Överför SSL-certifikat](#upload-ssl-certificates)
3. [Konfigurera hello-gateway](#configure-the-gateway)
4. [Ange hello gateway-konfiguration](#set-the-gateway-configuration)
5. [Starta hello gateway](#start-the-gateway)
6. [Verifiera hello gatewayens status](#verify-the-gateway-status)

## <a name="create-an-application-gateway"></a>Skapa en programgateway

Använd hello-toocreate hello gateway `New-AzureApplicationGateway` cmdlet, ersätter hello värden med dina egna. Fakturering för hello gatewayen startar inte nu. Fakturering börjar i ett senare steg när hello gateway har startat.

```powershell
New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")
```

toovalidate som hello gateway har skapats kan du använda hello `Get-AzureApplicationGateway` cmdlet.

I exemplet hello *beskrivning*, *InstanceCount*, och *GatewaySize* är valfria parametrar. Hej standardvärdet för *InstanceCount* är 2, med ett maximalt värde 10. Hej standardvärdet för *GatewaySize* är Medium. Små och stora är andra tillgängliga värden. *Virtualip:* och *DnsName* visas som tomt eftersom hello gateway inte har startat ännu. Dessa värden skapas när hello gateway är i hello körs.

```powershell
Get-AzureApplicationGateway AppGwTest
```

## <a name="upload-ssl-certificates"></a>Överför SSL-certifikat

Använd `Add-AzureApplicationGatewaySslCertificate` tooupload hello servercertifikat i *pfx* format toohello Programgateway. hello certifikatets namn är ett namn som valts av användaren och måste vara unika inom hello Programgateway. Det här certifikatet är refererad tooby det här namnet i alla certifikat hanteringsåtgärder på hello Programgateway.

Det här exemplet nedan visar hello cmdleten Ersätt hello värden i hello exemplet med dina egna.

```powershell
Add-AzureApplicationGatewaySslCertificate  -Name AppGwTest -CertificateName GWCert -Password <password> -CertificateFile <full path toopfx file>
```

Därefter Validera hello certifikat överföringen. Använd hello `Get-AzureApplicationGatewayCertificate` cmdlet.

Det här exemplet visar hello-cmdleten på hello första rad, följt av hello utdata.

```powershell
Get-AzureApplicationGatewaySslCertificate AppGwTest
```

```
VERBOSE: 5:07:54 PM - Begin Operation: Get-AzureApplicationGatewaySslCertificate
VERBOSE: 5:07:55 PM - Completed Operation: Get-AzureApplicationGatewaySslCertificate
Name           : SslCert
SubjectName    : CN=gwcert.app.test.contoso.com
Thumbprint     : AF5ADD77E160A01A6......EE48D1A
ThumbprintAlgo : sha1RSA
State..........: Provisioned
```

> [!NOTE]
> hello certifikatlösenord har toobe mellan 4 too12 tecken, bokstäver eller siffror. Specialtecken godkänns inte.

## <a name="configure-hello-gateway"></a>Konfigurera hello-gateway

En gateway programkonfigurationen består av flera värden. hello-värden kan knytas samman tooconstruct hello konfiguration.

hello-värden är:

* **Backend-serverpoolen:** hello lista över IP-adresser för hello backend-servrar. hello IP-adresser som anges antingen ska tillhöra toohello undernät för virtuellt nätverk eller ska vara en offentlig IP-adress/VIP.
* **Inställningar för backend-serverpool:** Varje pool har inställningar som port, protokoll och cookiebaserad tillhörighet. De här inställningarna är bundet tooa poolen och tillämpade tooall servrar inom hello poolen.
* **Frontend-port:** den här porten är hello offentliga som öppnas på hello Programgateway. Trafik träffar den här porten och sedan hämtar omdirigeras tooone hello backend-servrar.
* **Lyssnare:** hello-lyssnare har en frontend-port, ett protokoll (Http eller Https dessa värden är skiftlägeskänsligt), och hello SSL-certifikatnamn (om hur du konfigurerar SSL-avlastning).
* **Regel:** hello regeln Binder hello-lyssnare och hello backend-serverpoolen och definierar vilken backend-server pool hello trafik ska vara riktad toowhen den når en viss lyssnare. För närvarande endast hello *grundläggande* regeln stöds. Hej *grundläggande* regeln är resursallokering belastningsdistribution.

**Information om ytterligare konfiguration**

Konfiguration för SSL-certifikat, hello-protokollet i **HttpListener** bör ändra för*Https* (skiftlägeskänsligt). Hej **SslCert** läggs elementet för**HttpListener** med hello värdet toohello samma namn som används i hello överföringen av föregående avsnitt för SSL-certifikat. hello frontend-porten ska vara uppdaterade too443.

**tooenable cookie-baserad tillhörighet**: en Programgateway kan vara konfigurerade tooensure att en begäran från en klientsession är alltid dirigerad toohello samma virtuella dator i hello webbservergrupp. Det här scenariot sker genom injektion av en sessions-cookie som tillåter trafik för hello gateway toodirect på lämpligt sätt. Ange tooenable cookie-baserad tillhörighet **CookieBasedAffinity** för*aktiverad* i hello **BackendHttpSettings** element.

Du kan skapa din konfiguration genom att skapa ett konfigurationsobjekt eller med hjälp av en XML-konfigurationsfilen.
tooconstruct konfigurationen med hjälp av en konfiguration för XML-fil, använda hello följande exempel:

**Konfiguration av XML-exempel**

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
    <FrontendIPConfigurations />
    <FrontendPorts>
        <FrontendPort>
            <Name>FrontendPort1</Name>
            <Port>443</Port>
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
            <Protocol>Https</Protocol>
            <SslCert>GWCert</SslCert>
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

## <a name="set-hello-gateway-configuration"></a>Ange hello gateway-konfiguration

Därefter måste ange du hello Programgateway. Du kan använda hello `Set-AzureApplicationGatewayConfig` cmdlet med ett konfigurationsobjekt eller med en XML-konfigurationsfilen.

```powershell
Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml
```

## <a name="start-hello-gateway"></a>Starta hello gateway

När hello gateway har konfigurerats kan du använda hello `Start-AzureApplicationGateway` cmdlet toostart hello gateway. Fakturering för en Programgateway startar när hello gatewayen har startats.

> [!NOTE]
> Hej `Start-AzureApplicationGateway` cmdlet kan ta upp toofinish too15 20 minuter.
>
>

```powershell
Start-AzureApplicationGateway AppGwTest
```

## <a name="verify-hello-gateway-status"></a>Verifiera hello gatewayens status

Använd hello `Get-AzureApplicationGateway` cmdlet toocheck hello status för hello gateway. Om `Start-AzureApplicationGateway` lyckades hello föregående steg, *tillstånd* ska köras och *virtualip:* och *DnsName* ska ha giltiga poster.

Det här exemplet visar en Programgateway som är igång, körs och är redo tootake trafik.

```powershell
Get-AzureApplicationGateway AppGwTest
```

```
Name          : AppGwTest2
Description   :
VnetName      : testvnet1
Subnets       : {Subnet-1}
InstanceCount : 2
GatewaySize   : Medium
State         : Running
VirtualIPs    : {23.96.22.241}
DnsName       : appgw-4c960426-d1e6-4aae-8670-81fd7a519a43.cloudapp.net
```

## <a name="next-steps"></a>Nästa steg

Om du vill ha mer information om belastningsutjämningsalternativ i allmänhet läser du:

* [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

