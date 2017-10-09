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
ms.date: 06/20/2017
ms.author: gwallace
ms.openlocfilehash: d5354984760ceab12ed49efa9e18836e9f1d3c96
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-web-application-firewall-on-a-new-or-existing-application-gateway-with-azure-cli"></a>Konfigurera Brandvägg för webbaserade program på en ny eller befintlig Programgateway med Azure CLI

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-web-application-firewall-portal.md)
> * [PowerShell](application-gateway-web-application-firewall-powershell.md)
> * [Azure CLI](application-gateway-web-application-firewall-cli.md)

Lär dig hur toocreate Brandvägg för webbaserade program aktiverat Programgateway eller Lägg till web application brandväggen tooan befintliga Programgateway.

hello Brandvägg för webbaserade program (Brandvägg) i Azure Application Gateway skyddar webbprogram från vanliga webbaserade attacker som SQL injection, webbplatser scripting attacker och sessionen hijacks.

Azure Application Gateway är en Layer 7-belastningsutjämnare. Det ger redundans, prestanda-routing HTTP-begäranden mellan olika servrar, oavsett om de är på hello molnet eller lokalt. Programgateway innehåller många program leverans (ADC) funktioner i nätverksstyrenheten inklusive HTTP läsa in belastningsutjämning, cookie-baserad session tillhörighet, secure sockets layer (SSL)-avlastning, anpassade hälsoavsökningar, stöd för flera platser och många andra. Besök toofind en fullständig lista över funktioner som stöds: [översikt för Programgateway](application-gateway-introduction.md).

hello följande artikeln visar hur för[lägga till web application brandväggen tooan befintliga Programgateway](#add-web-application-firewall-to-an-existing-application-gateway) och [skapa en Programgateway som använder Brandvägg för webbaserade program](#create-an-application-gateway-with-web-application-firewall).

![scenario-bild][scenario]

## <a name="prerequisite-install-hello-azure-cli-20"></a>Förutsättning: Installera hello Azure CLI 2.0

tooperform hello stegen i den här artikeln, måste du för[installerar hello Azure-kommandoradsgränssnittet för Mac, Linux och Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).

## <a name="waf-configuration-differences"></a>Brandvägg configuration skillnader

Om du har läst [skapa en Programgateway med Azure CLI](application-gateway-create-gateway-cli.md), du förstår hello SKU inställningar tooconfigure när du skapar en Programgateway. Brandvägg ger ytterligare inställningar toodefine när du konfigurerar hello SKU: N på en Programgateway. Det finns inga ytterligare ändringar du gör på hello Programgateway sig själv.

| **Inställning** | **Detaljer**
|---|---|
|**SKU** |En normal Programgateway utan Brandvägg stöder **Standard\_små**, **Standard\_medel**, och **Standard\_stor** storlekar. Med hello införandet av Brandvägg, det finns två ytterligare SKU: er, **Brandvägg\_medel** och **Brandvägg\_stor**. Brandvägg stöds inte på små programgatewayer.|
|**Läge** | Den här inställningen är hello läge för Brandvägg. tillåtna värden är **identifiering** och **förebyggande**. När Brandvägg är inställd i identifieringsläget lagras alla hot i en loggfil. Händelser loggas fortfarande i förebyggande läge, men hello angripare tar emot ett 403 obehörig svar från hello Programgateway.|

## <a name="add-web-application-firewall-tooan-existing-application-gateway"></a>Lägg till web application brandväggen tooan befintliga Programgateway

hello Följ kommandot ändringar en befintlig standard gateway tooa Brandvägg aktiverade programmet Programgateway.

```azurecli-interactive
#!/bin/bash

az network application-gateway waf-config set \
  --enabled true \
  --firewall-mode Prevention \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"
```

Det här kommandot uppdaterar hello Programgateway med Brandvägg för webbaserade program. Besök [Gateway Programdiagnostik](application-gateway-diagnostics.md) toounderstand hur tooview loggar för din Programgateway. På grund av toohello säkerhet uppbyggnad Brandvägg, loggar måste toobe ses över regelbundet toounderstand hello säkerhetstillståndet webbprogram.

## <a name="create-an-application-gateway-with-web-application-firewall"></a>Skapa en Programgateway med Brandvägg för webbaserade program

hello följande kommando skapar en Programgateway med Brandvägg för webbaserade program.

```azurecli-interactive
#!/bin/bash

az network application-gateway create \
  --name "AdatumAppGateway2" \
  --location "eastus" \
  --resource-group "AdatumAppGatewayRG" \
  --vnet-name "AdatumAppGatewayVNET2" \
  --vnet-address-prefix "10.0.0.0/16" \
  --subnet "Appgatewaysubnet2" \
  --subnet-address-prefix "10.0.0.0/28" \
 --servers "10.0.0.5 10.0.0.4" \
  --capacity 2 
  --sku "WAF_Medium" \
  --http-settings-cookie-based-affinity "Enabled" \
  --http-settings-protocol "Http" \
  --frontend-port "80" \
  --routing-rule-type "Basic" \
  --http-settings-port "80" \
  --public-ip-address "pip2" \
  --public-ip-address-allocation "dynamic" \
  --tags "cli[2] owner[administrator]"
```

> [!NOTE]
> Programgatewayer som skapats med konfiguration av hello grundläggande webbprogram brandväggen konfigureras med CR 3.0 för skydd.

## <a name="get-application-gateway-dns-name"></a>Hämta DNS-namn för programgatewayen

När du har skapat hello gateway är hello nästa steg tooconfigure hello klientdelen för kommunikation. När du använder en offentlig IP-adress krävs ett dynamiskt tilldelat DNS-namn som inte är användarvänligt. tooensure slutanvändare kan träffa hello Programgateway, en CNAME-post kan vara används toopoint toohello offentlig slutpunkt för hello Programgateway. [Konfigurera ett eget domännamn i Azure](../cloud-services/cloud-services-custom-domain-name-portal.md). tooconfigure en CNAME-post att hämta information om hello Programgateway och dess associerade IP DNS-namn med hjälp av hello PublicIPAddress element bifogade toohello Programgateway. hello programmet gateway DNS-namn ska använda toocreate en CNAME-post som pekar hello två web program toothis DNS-namn. hello rekommenderas A-poster inte eftersom hello VIP ändras vid omstart för Programgateway.

```azurecli-interactive
#!/bin/bash

az network public-ip show \
  --name pip2 \
  --resource-group "AdatumAppGatewayRG"
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

## <a name="next-steps"></a>Nästa steg

Lär dig hur toocustomize Brandvägg regler genom att besöka: [anpassa web application brandväggsregler via hello Azure CLI 2.0](application-gateway-customize-waf-rules-cli.md).

[scenario]: ./media/application-gateway-web-application-firewall-cli/scenario.png
