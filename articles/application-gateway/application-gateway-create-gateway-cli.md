---
title: Skapa en Azure Programgateway - Azure CLI 2.0 | Microsoft Docs
description: "Lär dig hur du skapar en Programgateway med hjälp av Azure CLI 2.0 i Resource Manager"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c2f6516e-3805-49ac-826e-776b909a9104
ms.service: application-gateway
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: gwallace
ms.openlocfilehash: 052410db8c7619c7990dc319951a55663f2c2ba1
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="create-an-application-gateway-by-using-the-azure-cli-20"></a>Skapa en Programgateway med hjälp av Azure CLI 2.0

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-gateway-portal.md)
> * [PowerShell och Azure Resource Manager](application-gateway-create-gateway-arm.md)
> * [PowerShell och den klassiska Azure-portalen](application-gateway-create-gateway.md)
> * [Azure Resource Manager-mall](application-gateway-create-gateway-arm-template.md)
> * [Azure CLI 1.0](application-gateway-create-gateway-cli.md)
> * [Azure CLI 2.0](application-gateway-create-gateway-cli.md)

Programgateway är en dedikerad virtuell installation som ger programmet leverans domänkontrollant (ADC) som en tjänst med olika layer 7 läsa in belastningsutjämning funktioner för ditt program.

## <a name="cli-versions-to-complete-the-task"></a>CLI-versioner för att slutföra uppgiften

Du kan slutföra uppgiften med någon av följande CLI-versioner:

* [Azure CLI 1.0](application-gateway-create-gateway-cli-nodejs.md) – vår CLI för distributionsmodellerna klassisk och resurshantering.
* [Azure CLI 2.0](application-gateway-create-gateway-cli.md) – vår nästa generations CLI för distributionsmodellen resurshantering

## <a name="prerequisite-install-the-azure-cli-20"></a>Förutsättning: Installera Azure CLI 2.0

Om du vill utföra stegen i den här artikeln, måste du [installera Azure-kommandoradsgränssnittet för Mac, Linux och Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).

> [!NOTE]
> Om du inte har ett Azure-konto behöver du en. Registrera dig för en [kostnadsfri utvärderingsversion här](../active-directory/sign-up-organization.md).

## <a name="scenario"></a>Scenario

Lär dig hur du skapar en Programgateway med Azure-portalen i det här scenariot.

Det här scenariot kommer:

* Skapa en medelhög Programgateway med två instanser.
* Skapa ett virtuellt nätverk med namnet AdatumAppGatewayVNET med en 10.0.0.0/16 reserverade CIDR-blocket.
* Skapa undernätet Appgatewaysubnet som använder 10.0.0.0/28 som dess CIDR-block.

> [!NOTE]
> Ytterligare konfiguration för Programgateway, inklusive anpassade hälsa avsökningar, backend-adresser för poolen och ytterligare regler konfigureras när programgatewayen har konfigurerats och inte under första distributionen.

## <a name="before-you-begin"></a>Innan du börjar

Azure Application Gateway kräver sin egen undernät. När du skapar ett virtuellt nätverk, se till att du lämnar tillräckligt med adressutrymme för att du har flera undernät. När du distribuerar en Programgateway till ett undernät kan bara ytterligare programgatewayer läggas till undernätet.

## <a name="log-in-to-azure"></a>Logga in på Azure

Öppna den **Kommandotolken för Microsoft Azure**, och logga in. 

```azurecli-interactive
az login -u "username"
```

> [!NOTE]
> Du kan också använda `az login` utan att växeln för inloggning på enheten som kräver att ange en kod i aka.ms/devicelogin.

När du skriver i föregående exempel, har en kod angetts. Navigera till https://aka.ms/devicelogin i en webbläsare för att fortsätta inloggningen.

![cmd visar enhet inloggning][1]

Ange den kod som du fick i webbläsaren. Du omdirigeras till en inloggningssida.

![webbläsare för att ange koden][2]

När du har angett koden du är inloggad, Stäng webbläsaren för att fortsätta scenariot.

![loggat in][3]

## <a name="create-the-resource-group"></a>Skapa en resursgrupp

Innan du skapar programgatewayen, skapas en resursgrupp för programgatewayen. Nedan visas kommandot.

```azurecli-interactive
az group create --name myresourcegroup --location "eastus"
```

## <a name="create-the-application-gateway"></a>Skapa programgatewayen

IP-adresser som används för serverdelen är IP-adresser för backend-servern. Dessa värden kan vara privata IP-adresser i det virtuella nätverket, offentliga IP-adresser eller fullständigt kvalificerat domännamn för backend-servrar. I följande exempel skapas en Programgateway med ytterligare konfigurationsinställningar för http-inställningar, portar och regler.

```azurecli-interactive
az network application-gateway create \
--name "AdatumAppGateway" \
--location "eastus" \
--resource-group "myresourcegroup" \
--vnet-name "AdatumAppGatewayVNET" \
--vnet-address-prefix "10.0.0.0/16" \
--subnet "Appgatewaysubnet" \
--subnet-address-prefix "10.0.0.0/28" \
--servers 10.0.0.4 10.0.0.5 \
--capacity 2 \
--sku Standard_Small \
--http-settings-cookie-based-affinity Enabled \
--http-settings-protocol Http \
--frontend-port 80 \
--routing-rule-type Basic \
--http-settings-port 80 \
--public-ip-address "pip2" \
--public-ip-address-allocation "dynamic" \

```

I föregående exempel visas ett antal egenskaper som inte krävs under genereringen av en Programgateway. Följande exempel skapar en Programgateway med informationen som krävs.

```azurecli-interactive
az network application-gateway create \
--name "AdatumAppGateway" \
--location "eastus" \
--resource-group "myresourcegroup" \
--vnet-name "AdatumAppGatewayVNET" \
--vnet-address-prefix "10.0.0.0/16" \
--subnet "Appgatewaysubnet \
--subnet-address-prefix "10.0.0.0/28" \
--servers "10.0.0.5"  \
--public-ip-address pip
```
 
> [!NOTE]
> En lista över parametrar som kan tillhandahållas vid skapandet kör följande kommando: `az network application-gateway create --help`.

Det här exemplet skapar en basic-Programgateway med standardinställningarna för lyssnare, serverdelspool, serverdelens http-inställningar och regler. Du kan ändra dessa inställningar efter distributionen när etableringen har lyckats.
Om du redan har ditt webbprogram som definierats med serverdelspoolen i föregående steg, skapas en gång, börjar belastningsutjämning.

## <a name="get-application-gateway-dns-name"></a>Hämta DNS-namn för programgatewayen

När du har skapat gatewayen, är nästa steg att konfigurera klientprogrammet för kommunikation. När du använder en offentlig IP-adress krävs ett dynamiskt tilldelat DNS-namn som inte är användarvänligt. För att säkerställa att slutanvändare kan nå programgatewayen kan en CNAME-post användas för att peka på den offentliga slutpunkten för programgatewayen. [Konfigurera ett eget domännamn i Azure](../dns/dns-custom-domain.md). Hämta information om programgatewayen och dess tillhörande IP DNS-namn med PublicIPAddress-element som bifogas programgatewayen om du vill konfigurera ett alias. programgatewayens DNS-namn ska användas för att skapa en CNAME-post som leder de två webbapparna till detta DNS-namn. Användning av A-poster rekommenderas inte eftersom VIP kan ändras vid omstart av programgatewayen.


```azurecli-interactive
az network public-ip show --name "pip" --resource-group "AdatumAppGatewayRG"
```

```
{
  "dnsSettings": {
    "domainNameLabel": null,
    "fqdn": "8c786058-96d4-4f3e-bb41-660860ceae4c.cloudapp.net",
    "reverseFqdn": null
  },
  "etag": "W/\"3b0ac031-01f0-4860-b572-e3c25e0c57ad\"",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/AdatumAppGatewayRG/providers/Microsoft.Network/publicIPAddresses/pip2",
  "idleTimeoutInMinutes": 4,
  "ipAddress": "40.121.167.250",
  "ipConfiguration": {
    "etag": null,
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/AdatumAppGatewayRG/providers/Microsoft.Network/applicationGateways/AdatumAppGateway2/frontendIPConfigurations/appGatewayFrontendIP",
    "name": null,
    "privateIpAddress": null,
    "privateIpAllocationMethod": null,
    "provisioningState": null,
    "publicIpAddress": null,
    "resourceGroup": "AdatumAppGatewayRG",
    "subnet": null
  },
  "location": "eastus",
  "name": "pip2",
  "provisioningState": "Succeeded",
  "publicIpAddressVersion": "IPv4",
  "publicIpAllocationMethod": "Dynamic",
  "resourceGroup": "AdatumAppGatewayRG",
  "resourceGuid": "3c30d310-c543-4e9d-9c72-bbacd7fe9b05",
  "tags": {
    "cli[2] owner[administrator]": ""
  },
  "type": "Microsoft.Network/publicIPAddresses"
}
```

## <a name="delete-all-resources"></a>Ta bort alla resurser

Så här tar du bort alla resurser som skapats i den här artikeln:

```azurecli-interactive
az group delete --name AdatumAppGatewayRG
```
 
## <a name="next-steps"></a>Nästa steg

Lär dig hur du skapar anpassade hälsoavsökningar genom att besöka [skapa en anpassad hälsoavsökningen](application-gateway-create-probe-portal.md)

Lär dig hur du konfigurerar SSL-avlastning och ta kostsamma SSL-dekryptering av webbservrar genom att besöka [Konfigurera SSL-avlastning](application-gateway-ssl-arm.md)

<!--Image references-->

[scenario]: ./media/application-gateway-create-gateway-cli/scenario.png
[1]: ./media/application-gateway-create-gateway-cli/figure1.png
[2]: ./media/application-gateway-create-gateway-cli/figure2.png
[3]: ./media/application-gateway-create-gateway-cli/figure3.png
