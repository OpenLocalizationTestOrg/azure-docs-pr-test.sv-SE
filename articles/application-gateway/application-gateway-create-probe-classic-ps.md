---
title: "aaaCreate klassiska en anpassad avsökningsåtgärd - Azure Application Gateway - PowerShell | Microsoft Docs"
description: "Lär dig hur toocreate en anpassad avsökning för Programgateway med PowerShell i hello klassiska distributionsmodellen"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 338a7be1-835c-48e9-a072-95662dc30f5e
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: gwallace
ms.openlocfilehash: 68332367c99328bd6456b0c339923765637be986
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-custom-probe-for-azure-application-gateway-classic-by-using-powershell"></a>Skapa en anpassad avsökningsåtgärd för Azure-Programgateway (klassiskt) med hjälp av PowerShell

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-probe-portal.md)
> * [PowerShell och Azure Resource Manager](application-gateway-create-probe-ps.md)
> * [PowerShell och den klassiska Azure-portalen](application-gateway-create-probe-classic-ps.md)

Lägg till en anpassad avsökningsåtgärd tooan befintliga Programgateway med PowerShell i den här artikeln. Anpassade avsökningar är användbara för program som har ett visst hälsotillstånd Kontrollera sidan eller för program som inte tillhandahåller ett lyckat svar på hello standardwebbprogrammet.

> [!IMPORTANT]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen. Lär dig hur för[utföra dessa steg med hello Resource Manager-modellen](application-gateway-create-probe-ps.md).

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-an-application-gateway"></a>Skapa en programgateway

toocreate en Programgateway:

1. Skapa en resurs för en programgateway.
2. Skapa en XML-konfigurationsfil eller ett konfigurationsobjekt.
3. Bekräfta hello configuration toohello nyskapad programresursen gateway.

### <a name="create-an-application-gateway-resource-with-a-custom-probe"></a>Skapa ett program gateway med en anpassad avsökningsåtgärd

Använd hello-toocreate hello gateway `New-AzureApplicationGateway` cmdlet, ersätter hello värden med dina egna. Fakturering för hello gatewayen startar inte nu. Fakturering börjar i ett senare steg när hello gateway har startat.

hello följande exempel skapas en Programgateway med hjälp av ett virtuellt nätverk kallas ”testvnet1” och ett undernät som kallas ”Undernät 1”.

```powershell
New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")
```

toovalidate som hello gateway har skapats kan du använda hello `Get-AzureApplicationGateway` cmdlet.

```powershell
Get-AzureApplicationGateway AppGwTest
```

> [!NOTE]
> Hej standardvärdet för *InstanceCount* är 2, med ett maximalt värde 10. Hej standardvärdet för *GatewaySize* är Medium. Du kan välja mellan små, medelstora och stora.
> 
> 

*Virtualip:* och *DnsName* visas som tomt eftersom hello gateway inte har startat ännu. Dessa värden skapas när hello gateway är i hello körs.

### <a name="configure-an-application-gateway-by-using-xml"></a>Konfigurera en Programgateway genom att använda XML

I följande exempel hello, och använda en XML-filen tooconfigure alla inställningar för Programgateway och bekräfta dem toohello programresursen gateway.  

Kopiera följande text tooNotepad hello.

```xml
<ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
<FrontendIPConfigurations>
    <FrontendIPConfiguration>
        <Name>fip1</Name>
        <Type>Private</Type>
    </FrontendIPConfiguration>
</FrontendIPConfigurations>
<FrontendPorts>
    <FrontendPort>
        <Name>port1</Name>
        <Port>80</Port>
    </FrontendPort>
</FrontendPorts>
<Probes>
    <Probe>
        <Name>Probe01</Name>
        <Protocol>Http</Protocol>
        <Host>contoso.com</Host>
        <Path>/path/custompath.htm</Path>
        <Interval>15</Interval>
        <Timeout>15</Timeout>
        <UnhealthyThreshold>5</UnhealthyThreshold>
    </Probe>
    </Probes>
    <BackendAddressPools>
    <BackendAddressPool>
        <Name>pool1</Name>
        <IPAddresses>
            <IPAddress>1.1.1.1</IPAddress>
            <IPAddress>2.2.2.2</IPAddress>
        </IPAddresses>
    </BackendAddressPool>
</BackendAddressPools>
<BackendHttpSettingsList>
    <BackendHttpSettings>
        <Name>setting1</Name>
        <Port>80</Port>
        <Protocol>Http</Protocol>
        <CookieBasedAffinity>Enabled</CookieBasedAffinity>
        <RequestTimeout>120</RequestTimeout>
        <Probe>Probe01</Probe>
    </BackendHttpSettings>
</BackendHttpSettingsList>
<HttpListeners>
    <HttpListener>
        <Name>listener1</Name>
        <FrontendIP>fip1</FrontendIP>
    <FrontendPort>port1</FrontendPort>
        <Protocol>Http</Protocol>
    </HttpListener>
</HttpListeners>
<HttpLoadBalancingRules>
    <HttpLoadBalancingRule>
        <Name>lbrule1</Name>
        <Type>basic</Type>
        <BackendHttpSettings>setting1</BackendHttpSettings>
        <Listener>listener1</Listener>
        <BackendAddressPool>pool1</BackendAddressPool>
    </HttpLoadBalancingRule>
</HttpLoadBalancingRules>
</ApplicationGatewayConfiguration>
```

Redigera hello värden mellan hello parenteser för hello konfigurationsobjekt. Spara hello-filen med filnamnstillägget .xml.

hello följande exempel visas hur toouse en konfiguration filen tooset in hello programmet gateway tooload balansera HTTP-trafik på offentlig port 80 och skicka nätverkstrafik tooback avslutande port 80 mellan två IP-adresser med hjälp av en anpassad avsökning.

> [!IMPORTANT]
> hello protokollet objektet Http eller Https är skiftlägeskänsliga.

Ett nytt konfigurationsobjekt \<avsökning\> läggs anpassade tooconfigure-avsökningar.

hello konfigurationsparametrar är:

|Parameter|Beskrivning|
|---|---|
|**Namn** |Referensnamn för anpassade avsökningen. |
* **Protokollet** | Protokoll som används (möjliga värden är HTTP eller HTTPS).|
| **Värden** och **sökväg** | Fullständig URL-sökväg som anropas av hello gateway toodetermine hello programhälsan av hello-instansen. Om du har en webbplats http://contoso.com/ hello anpassad avsökningsåtgärd kan konfigureras för kontrollerar ”http://contoso.com/path/custompath.htm” för avsökningen toohave ett lyckat HTTP-svar.|
| **Intervall** | Konfigurerar hello avsökningen intervall kontroller i sekunder.|
| **Timeout** | Definierar hello avsökningen timeout-värdet för en kontroll för HTTP-svar.|
| **UnhealthyThreshold** | Hej antalet misslyckade HTTP-svar krävs tooflag hello backend-instans som *ohälsosamt*.|

Hej avsökningsnamnet refereras i hello \<BackendHttpSettings\> configuration tooassign vilka backend-adresspool använder anpassad avsökningsåtgärd inställningar.

## <a name="add-a-custom-probe-tooan-existing-application-gateway"></a>Lägg till en anpassad avsökningsåtgärd tooan befintliga Programgateway

Ändra hello aktuella konfigurationen för en Programgateway kräver tre steg: hämta hello aktuella XML-konfigurationsfilen, ändra toohave en anpassad avsökning och konfigurera hello Programgateway med hello nya XML-inställningar.

1. Hämta hello XML-filen genom att använda `Get-AzureApplicationGatewayConfig`. Den här cmdleten export hello configuration XML toobe ändrade tooadd någon avsökningsinställning.

  ```powershell
  Get-AzureApplicationGatewayConfig -Name "<application gateway name>" -Exporttofile "<path toofile>"
  ```

1. Öppna hello XML-filen i en textredigerare. Lägg till en `<probe>` avsnittet efter `<frontendport>`.

  ```xml
<Probes>
    <Probe>
        <Name>Probe01</Name>
        <Protocol>Http</Protocol>
        <Host>contoso.com</Host>
        <Path>/path/custompath.htm</Path>
        <Interval>15</Interval>
        <Timeout>15</Timeout>
        <UnhealthyThreshold>5</UnhealthyThreshold>
    </Probe>
</Probes>
  ```

  Lägg till hello avsökningsnamnet i hello backendHttpSettings avsnittet hello XML, som visas i följande exempel hello:

  ```xml
    <BackendHttpSettings>
        <Name>setting1</Name>
        <Port>80</Port>
        <Protocol>Http</Protocol>
        <CookieBasedAffinity>Enabled</CookieBasedAffinity>
        <RequestTimeout>120</RequestTimeout>
        <Probe>Probe01</Probe>
    </BackendHttpSettings>
  ```

  Spara hello XML-fil.

1. Uppdatera hello programkonfigurationen gateway med hello nya XML-filen med hjälp av `Set-AzureApplicationGatewayConfig`. Denna cmdlet uppdaterar din Programgateway med hello ny konfiguration.

```powershell
Set-AzureApplicationGatewayConfig -Name "<application gateway name>" -Configfile "<path toofile>"
```

## <a name="next-steps"></a>Nästa steg

Om du vill tooconfigure Secure Sockets Layer (SSL)-avlastning, se [konfigurera en Programgateway för SSL-avlastning](application-gateway-ssl.md).

Om du vill tooconfigure ett program gateway toouse med en intern belastningsutjämnare, se [skapa en Programgateway med en intern belastningsutjämnare (ILB)](application-gateway-ilb.md).

