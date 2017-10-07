---
title: aaaCreate en Programgateway Azure - Azure CLI 2.0 | Microsoft Docs
description: "Lär dig hur toocreate en Programgateway med hjälp av hello Azure CLI 2.0 i Resource Manager"
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
ms.openlocfilehash: 952065586cd87d253882438bb779b768d9fd59fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-by-using-hello-azure-cli-20"></a>Skapa en Programgateway med hello Azure CLI 2.0

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-gateway-portal.md)
> * [PowerShell och Azure Resource Manager](application-gateway-create-gateway-arm.md)
> * [PowerShell och den klassiska Azure-portalen](application-gateway-create-gateway.md)
> * [Azure Resource Manager-mall](application-gateway-create-gateway-arm-template.md)
> * [Azure CLI 1.0](application-gateway-create-gateway-cli.md)
> * [Azure CLI 2.0](application-gateway-create-gateway-cli.md)

Programgateway är en dedikerad virtuell installation som ger programmet leverans domänkontrollant (ADC) som en tjänst med olika layer 7 läsa in belastningsutjämning funktioner för ditt program.

## <a name="cli-versions-toocomplete-hello-task"></a>CLI versioner toocomplete hello aktivitet

Du kan göra hello med hjälp av något av följande versioner av CLI hello:

* [Azure CLI 1.0](application-gateway-create-gateway-cli-nodejs.md) -vår CLI för hello klassisk och resurs management distributionsmodeller.
* [Azure CLI 2.0](application-gateway-create-gateway-cli.md) -vår nästa generations CLI för hello resursdistributionsmodell för hantering

## <a name="prerequisite-install-hello-azure-cli-20"></a>Förutsättning: Installera hello Azure CLI 2.0

tooperform hello stegen i den här artikeln, måste du för[installerar hello Azure-kommandoradsgränssnittet för Mac, Linux och Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).

> [!NOTE]
> Om du inte har ett Azure-konto behöver du en. Registrera dig för en [kostnadsfri utvärderingsversion här](../active-directory/sign-up-organization.md).

## <a name="scenario"></a>Scenario

I det här scenariot kan du lära dig hur toocreate som en gateway som använder hello Azure-portalen.

Det här scenariot kommer:

* Skapa en medelhög Programgateway med två instanser.
* Skapa ett virtuellt nätverk med namnet AdatumAppGatewayVNET med en 10.0.0.0/16 reserverade CIDR-blocket.
* Skapa undernätet Appgatewaysubnet som använder 10.0.0.0/28 som dess CIDR-block.

> [!NOTE]
> Ytterligare konfiguration av hello Programgateway, inklusive anpassade hälsa avsökningar, backend-adresser för poolen och ytterligare regler konfigureras när hello Programgateway har konfigurerats och inte under första distributionen.

## <a name="before-you-begin"></a>Innan du börjar

Azure Application Gateway kräver sin egen undernät. När du skapar ett virtuellt nätverk, se till att du lämnar tillräckligt med adressutrymme toohave flera undernät. När du distribuerar ett program gateway tooa undernät kan bara ytterligare programgatewayer läggas toohello undernät.

## <a name="log-in-tooazure"></a>Logga in tooAzure

Öppna hello **Kommandotolken för Microsoft Azure**, och logga in. 

```azurecli-interactive
az login -u "username"
```

> [!NOTE]
> Du kan också använda `az login` utan hello växeln för inloggning på enheten som kräver att ange en kod i aka.ms/devicelogin.

När du skriver hello föregående exempel, har en kod angetts. Navigera toohttps://aka.ms/devicelogin i en webbläsare toocontinue hello-inloggningen.

![cmd visar enhet inloggning][1]

Ange hello koden du fått i hello webbläsare. Du kan omdirigerade tooa inloggningssidan.

![webbläsaren tooenter kod][2]

När hello kod har registrerats du är inloggad, Stäng hello webbläsare toocontinue med hello scenario.

![loggat in][3]

## <a name="create-hello-resource-group"></a>Skapa hello resursgrupp

Innan du skapar hello Programgateway skapas en resursgrupp toocontain hello Programgateway. hello följande visar hello-kommando.

```azurecli-interactive
az group create --name myresourcegroup --location "eastus"
```

## <a name="create-hello-application-gateway"></a>Skapa hello Programgateway

hello IP-adresser som används för hello serverdel är hello IP-adresser för backend-servern. Dessa värden kan vara antingen privat IP-adresser i hello virtuellt nätverk, offentliga IP-adresser eller fullständigt kvalificerat domännamn för backend-servrar. hello följande exempel skapas en Programgateway med ytterligare konfigurationsinställningar för http-inställningar, portar och regler.

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

hello visas föregående exempel ett antal egenskaper som inte krävs under hello skapandet av en Programgateway. Följande kodexempel hello skapar en Programgateway med hello krävs information.

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
> En lista över parametrar som kan anges under hello för att skapa kör följande kommando: `az network application-gateway create --help`.

Det här exemplet skapar en basic-Programgateway med standardinställningarna för hello lyssnare, serverdelspool, serverdelens http-inställningar och regler. Du kan ändra dessa inställningar toosuit distributionen när hello etablering har lyckats.
Om du redan har ditt webbprogram som definierats med hello serverdelspool i föregående steg, när skapat hello börjar belastningsutjämning.

## <a name="get-application-gateway-dns-name"></a>Hämta DNS-namn för programgatewayen

När du har skapat hello gateway är hello nästa steg tooconfigure hello klientdelen för kommunikation. När du använder en offentlig IP-adress krävs ett dynamiskt tilldelat DNS-namn som inte är användarvänligt. tooensure slutanvändare kan träffa hello Programgateway, en CNAME-post kan vara används toopoint toohello offentlig slutpunkt för hello Programgateway. [Konfigurera ett eget domännamn i Azure](../dns/dns-custom-domain.md). tooconfigure ett alias, hämta information om hello Programgateway och dess associerade IP DNS-namn med hjälp av hello PublicIPAddress element bifogade toohello Programgateway. hello programmet gateway DNS-namn ska använda toocreate en CNAME-post som pekar hello två web program toothis DNS-namn. hello rekommenderas A-poster inte eftersom hello VIP ändras vid omstart för Programgateway.


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

toodelete alla resurser skapas i den här artikeln, fullständig hello följande steg:

```azurecli-interactive
az group delete --name AdatumAppGatewayRG
```
 
## <a name="next-steps"></a>Nästa steg

Lär dig hur toocreate anpassade hälsoavsökning genom att besöka [skapa en anpassad hälsoavsökningen](application-gateway-create-probe-portal.md)

Lär dig hur tooconfigure SSL-avlastning och vidta hello kostsamma SSL dekryptering av webbservrar genom att besöka [Konfigurera SSL-avlastning](application-gateway-ssl-arm.md)

<!--Image references-->

[scenario]: ./media/application-gateway-create-gateway-cli/scenario.png
[1]: ./media/application-gateway-create-gateway-cli/figure1.png
[2]: ./media/application-gateway-create-gateway-cli/figure2.png
[3]: ./media/application-gateway-create-gateway-cli/figure3.png
