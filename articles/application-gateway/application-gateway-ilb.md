---
title: "aaaUsing Azure Programgateway med interna belastningsutjämnare | Microsoft Docs"
description: "Den här sidan finns instruktioner tooconfigure en Programgateway i Azure med en intern belastningsutjämnade slutpunkt"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 7403d28e-909f-46a2-b282-43a8e942f53c
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: gwallace
ms.openlocfilehash: 272ef84a02f92a8521c35aad6f1d9f9bf1675718
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-with-an-internal-load-balancer-ilb"></a>Skapa en Application Gateway med en intern belastningsutjämnare (ILB)

> [!div class="op_single_selector"]
> * [PowerShell och den klassiska Azure-portalen](application-gateway-ilb.md)
> * [PowerShell och Azure Resource Manager](application-gateway-ilb-arm.md)

Programgateway kan konfigureras med en internetuppkopplad virtuell IP-adress eller med en intern slutpunkt exponeras inte toohello internet, även kallat inre belastningen belastningsutjämnare (ILB) slutpunkt. Konfigurera hello gateway med en ILB är användbart för toointernet interna line-of-business-program som inte visas. Det är också användbart för servicenivåerna inom en flernivåapp som placeras i en gräns som exponeras inte toointernet för säkerhet, men kräver fortfarande resursallokering belastningsdistribution, varaktighet för sessionen eller SSL-avslutning. Den här artikeln vägleder dig genom hello steg tooconfigure en Programgateway med en ILB.

## <a name="before-you-begin"></a>Innan du börjar

1. Installera senaste versionen av hello Azure PowerShell-cmdlets med hello installationsprogram för webbplattform. Du kan hämta och installera hello senaste versionen från hello **Windows PowerShell** avsnitt i hello [hämtningssidan](https://azure.microsoft.com/downloads/).
2. Kontrollera att du har ett virtuellt nätverk fungerar med giltig nätmask.
3. Kontrollera att du har backend-servrar i hello virtuellt nätverk eller med en offentlig IP-adress/VIP tilldelad.

toocreate en Programgateway utföra hello följa stegen i ordning hello. 

1. [Skapa en Programgateway](#create-a-new-application-gateway)
2. [Konfigurera hello-gateway](#configure-the-gateway)
3. [Ange hello gateway-konfiguration](#set-the-gateway-configuration)
4. [Starta hello gateway](#start-the-gateway)
5. [Kontrollera hello gateway](#verify-the-gateway-status)

## <a name="create-an-application-gateway"></a>Skapa en Programgateway:

**toocreate hello gateway**, använda hello `New-AzureApplicationGateway` cmdlet, ersätter hello värden med dina egna. Observera att faktureringen för hello gatewayen inte startar nu. Fakturering börjar i ett senare steg när hello gateway har startat.

```powershell
New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")
```

```
VERBOSE: 4:31:35 PM - Begin Operation: New-AzureApplicationGateway 
VERBOSE: 4:32:37 PM - Completed Operation: New-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error 
----       ----------------     ------------                             ----
Successful OK                   55ef0460-825d-2981-ad20-b9a8af41b399
```

**toovalidate** att hello gateway har skapats kan du använda hello `Get-AzureApplicationGateway` cmdlet. 

I exemplet hello *beskrivning*, *InstanceCount*, och *GatewaySize* är valfria parametrar. Hej standardvärdet för *InstanceCount* är 2, med ett maximalt värde 10. Hej standardvärdet för *GatewaySize* är Medium. Små och stora är andra tillgängliga värden. *VIP* och *DnsName* visas som tomt eftersom hello gateway inte har startat ännu. Dessa skapas när hello gateway är i hello körs. 

```powershell
Get-AzureApplicationGateway AppGwTest
```

```
VERBOSE: 4:39:39 PM - Begin Operation:
Get-AzureApplicationGateway VERBOSE: 4:39:40 PM - Completed 
Operation: Get-AzureApplicationGateway
Name: AppGwTest    
Description: 
VnetName: testvnet1 
Subnets: {Subnet-1} 
InstanceCount: 2 
GatewaySize: Medium 
State: Stopped 
VirtualIPs: 
DnsName:
```

## <a name="configure-hello-gateway"></a>Konfigurera hello-gateway
En gateway programkonfigurationen består av flera värden. hello-värden kan knytas samman tooconstruct hello konfiguration.

hello-värden är:

* **Backend-serverpoolen:** hello lista över IP-adresser på hello backend-servrar. hello IP-adresser som anges antingen ska tillhöra toohello VNet subnet eller ska vara en offentlig IP-adress/VIP. 
* **Inställningar för backend-serverpool:** Varje pool har inställningar som port, protokoll och cookie-baserad tillhörighet. De här inställningarna är bundet tooa poolen och tillämpade tooall servrar inom hello poolen.
* **Klientdelsport:** den här porten är hello offentliga öppnas på hello Programgateway. Trafik träffar den här porten och hämtar omdirigerade tooone på hello backend-servrar.
* **Lyssnare:** hello-lyssnare har en klientdelsport, ett protokoll (Http eller Https, dessa är skiftlägeskänsligt), och hello SSL-certifikatnamn (om hur du konfigurerar SSL-avlastning). 
* **Regel:** hello regeln Binder hello-lyssnare och hello backend-serverpoolen och definierar vilken backend-servern poolen hello trafik ska vara riktad toowhen den når en viss lyssnare. För närvarande endast hello *grundläggande* regeln stöds. Hej *grundläggande* regeln är resursallokering belastningsdistribution.

Du kan skapa din konfiguration genom att skapa ett konfigurationsobjekt eller genom att använda en XML-konfigurationsfilen. tooconstruct konfigurationen med hjälp av en XML-konfigurationsfilen, Använd hello exempel nedan.

Observera följande hello:

* Hej *FrontendIPConfigurations* element beskriver hello ILB uppgifter för att konfigurera Application Gateway med en ILB. 
* hello klientdel IP *typen* ska anges too'Private'
* Hej *StaticIPAddress* ska anges toohello önskad intern IP-adress på vilken hello gateway tar emot trafik. Observera att hello *StaticIPAddress* element är valfria. Om du inte kan en tillgänglig intern IP-adress från hello distribueras undernät väljs. 
* Hej värdet för hello *namn* element som anges i *FrontendIPConfiguration* ska användas i hello HTTPListener *FrontendIP* elementet toorefer toohello FrontendIPConfiguration.
  
  **Konfiguration av XML-exempel**
```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
    <FrontendIPConfigurations>
        <FrontendIPConfiguration>
            <Name>fip1</Name> 
            <Type>Private</Type> 
            <StaticIPAddress>10.0.0.10</StaticIPAddress> 
        </FrontendIPConfiguration>
    </FrontendIPConfigurations>
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
            <FrontendIP>fip1</FrontendIP>
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


## <a name="set-hello-gateway-configuration"></a>Ange hello gateway-konfiguration
Sedan ställer du hello Programgateway. Du kan använda hello `Set-AzureApplicationGatewayConfig` cmdlet med ett konfigurationsobjekt eller med en XML-konfigurationsfilen. 

```powershell
Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml
```

```
VERBOSE: 7:54:59 PM - Begin Operation: Set-AzureApplicationGatewayConfig 
VERBOSE: 7:55:32 PM - Completed Operation: Set-AzureApplicationGatewayConfig
Name       HTTP Status Code     Operation ID                             Error 
----       ----------------     ------------                             ----
Successful OK                   9b995a09-66fe-2944-8b67-9bb04fcccb9d
```

## <a name="start-hello-gateway"></a>Starta hello gateway

När hello gateway har konfigurerats kan du använda hello `Start-AzureApplicationGateway` cmdlet toostart hello gateway. Fakturering för en Programgateway startar när hello gatewayen har startats. 

> [!NOTE]
> Hej `Start-AzureApplicationGateway` cmdlet kan ta upp toocomplete too15 20 minuter. 
> 
> 

```powershell
Start-AzureApplicationGateway AppGwTest 
```

```
VERBOSE: 7:59:16 PM - Begin Operation: Start-AzureApplicationGateway 
VERBOSE: 8:05:52 PM - Completed Operation: Start-AzureApplicationGateway
Name       HTTP Status Code     Operation ID                             Error 
----       ----------------     ------------                             ----
Successful OK                   fc592db8-4c58-2c8e-9a1d-1c97880f0b9b
```

## <a name="verify-hello-gateway-status"></a>Verifiera hello gatewayens status

Använd hello `Get-AzureApplicationGateway` cmdlet toocheck hello statusen för gateway. Om `Start-AzureApplicationGateway` lyckades hello föregående steg, hello-tillståndet ska vara *kör*, hello Vip och DNS-namn ska ha giltiga poster. Det här exemplet visar hello-cmdleten på hello första rad, följt av hello utdata. I det här exemplet hello gateway körs och är redo tootake trafik. 

> [!NOTE]
> hello Programgateway konfigureras tooaccept trafik i hello konfigurerad ILB-slutpunkten för 10.0.0.10 i det här exemplet.

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
VirtualIPs    : {10.0.0.10}
DnsName       : appgw-b2a11563-2b3a-4172-a4aa-226ee4c23eed.cloudapp.net
```

## <a name="next-steps"></a>Nästa steg
Om du vill ha mer information om belastningsutjämningsalternativ i allmänhet läser du:

* [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

