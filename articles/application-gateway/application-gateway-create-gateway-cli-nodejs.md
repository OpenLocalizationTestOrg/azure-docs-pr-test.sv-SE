---
title: aaaCreate en Programgateway Azure - Azure CLI 1.0 | Microsoft Docs
description: "Lär dig hur toocreate en Programgateway med hjälp av hello Azure CLI 1.0 i Resource Manager"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c2f6516e-3805-49ac-826e-776b909a9104
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: gwallace
ms.openlocfilehash: 3c0d2d96b6be404d0372d00f0deb2a32959ca419
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-gateway-by-using-hello-azure-cli"></a>Skapa en Programgateway med hello Azure CLI

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-gateway-portal.md)
> * [PowerShell och Azure Resource Manager](application-gateway-create-gateway-arm.md)
> * [PowerShell och den klassiska Azure-portalen](application-gateway-create-gateway.md)
> * [Azure Resource Manager-mall](application-gateway-create-gateway-arm-template.md)
> * [Azure CLI 1.0](application-gateway-create-gateway-cli.md)
> * [Azure CLI 2.0](application-gateway-create-gateway-cli.md)
> 
> 

Azure Application Gateway är en Layer 7-belastningsutjämnare. Det ger redundans, prestanda-routing HTTP-begäranden mellan olika servrar, oavsett om de är på hello molnet eller lokalt. Programgateway har hello efter leverans tillämpningsfunktioner: HTTP läsa in belastningsutjämning, cookie-baserad session tillhörighet och Secure Sockets Layer (SSL)-avlastning, anpassade hälsoavsökningar och stöd för flera platser.

## <a name="prerequisite-install-hello-azure-cli"></a>Förutsättning: Installera hello Azure CLI

tooperform hello stegen i den här artikeln, måste du för[installerar hello Azure-kommandoradsgränssnittet för Mac, Linux och Windows (Azure CLI)](../xplat-cli-install.md) och du behöver för[inloggning tooAzure](../xplat-cli-connect.md). 

> [!NOTE]
> Om du inte har ett Azure-konto behöver du en. Registrera dig för en [kostnadsfri utvärderingsversion här](../active-directory/sign-up-organization.md).

## <a name="scenario"></a>Scenario

I det här scenariot kan du lära dig hur toocreate som en gateway som använder hello Azure-portalen.

Det här scenariot kommer:

* Skapa en medelhög Programgateway med två instanser.
* Skapa ett virtuellt nätverk med namnet ContosoVNET med en 10.0.0.0/16 reserverade CIDR-blocket.
* Skapa ett undernät som kallas subnet01 som använder 10.0.0.0/28 som sitt CIDR-block.

> [!NOTE]
> Ytterligare konfiguration av hello Programgateway, inklusive anpassade hälsa avsökningar, backend-adresser för poolen och ytterligare regler konfigureras när hello Programgateway har konfigurerats och inte under första distributionen.

## <a name="before-you-begin"></a>Innan du börjar

Azure Application Gateway kräver sin egen undernät. När du skapar ett virtuellt nätverk, se till att du lämnar tillräckligt med adressutrymme toohave flera undernät. När du distribuerar ett program tooa gatewayundernät endast ytterligare programgatewayer är kan toobe läggs toohello undernät.

## <a name="log-in-tooazure"></a>Logga in tooAzure

Öppna hello **Kommandotolken för Microsoft Azure**, och logga in. 

```azurecli-interactive
azure login
```

När du skriver hello föregående exempel, har en kod angetts. Navigera toohttps://aka.ms/devicelogin i en webbläsare toocontinue hello-inloggningen.

![cmd visar enhet inloggning][1]

Ange hello koden du fått i hello webbläsare. Du kan omdirigerade tooa inloggningssidan.

![webbläsaren tooenter kod][2]

När hello kod har registrerats du är inloggad, Stäng hello webbläsare toocontinue med hello scenario.

![loggat in][3]

## <a name="switch-tooresource-manager-mode"></a>Växla tooResource Manager-läge

```azurecli-interactive
azure config mode arm
```

## <a name="create-hello-resource-group"></a>Skapa hello resursgrupp

Innan du skapar hello Programgateway skapas en resursgrupp toocontain hello Programgateway. hello följande visar hello-kommando.

```azurecli-interactive
azure group create \
--name ContosoRG \
--location eastus
```

## <a name="create-a-virtual-network"></a>Skapa ett virtuellt nätverk

När du har skapat hello resursgruppen skapas ett virtuellt nätverk för hello Programgateway.  I följande exempel hello, var hello-adressutrymmet som 10.0.0.0/16 enligt definitionen i hello föregående scenariot anteckningar.

```azurecli-interactive
azure network vnet create \
--name ContosoVNET \
--address-prefixes 10.0.0.0/16 \
--resource-group ContosoRG \
--location eastus
```

## <a name="create-a-subnet"></a>Skapa ett undernät

När hello virtuella nätverket har skapats läggs ett undernät för hello Programgateway.  Om du planerar toouse Programgateway med ett webbprogram finns i hello samma virtuella nätverk som hello Programgateway, och vara säker på att tooleave tillräckligt med utrymme för ett annat undernät.

```azurecli-interactive
azure network vnet subnet create \
--resource-group ContosoRG \
--name subnet01 \
--vnet-name ContosoVNET \
--address-prefix 10.0.0.0/28 
```

## <a name="create-hello-application-gateway"></a>Skapa hello Programgateway

När hello virtuellt nätverk och undernät skapas, är hello förutsättningar för hello Programgateway klar. En tidigare exporterade PFX-certifikat och hello lösenordet för hello certifikatet krävs dessutom hello följande steg: hello IP-adresser som används för hello serverdel är hello IP-adresser för backend-servern. Dessa värden kan vara antingen privat IP-adresser i hello virtuellt nätverk, offentliga IP-adresser eller fullständigt kvalificerat domännamn för backend-servrar.

```azurecli-interactive
azure network application-gateway create \
--name AdatumAppGateway \
--location eastus \
--resource-group ContosoRG \
--vnet-name ContosoVNET \
--subnet-name subnet01 \
--servers 134.170.185.46,134.170.188.221,134.170.185.50 \
--capacity 2 \
--sku-tier Standard \
--routing-rule-type Basic \
--frontend-port 80 \
--http-settings-cookie-based-affinity Enabled \
--http-settings-port 80 \
--http-settings-protocol http \
--frontend-port http \
--sku-name Standard_Medium
```

> [!NOTE]
> En lista över parametrar som kan tillhandahållas vid skapandet kör följande kommando hello: **azure-nätverk Programgateway skapa--hjälpa**.

Det här exemplet skapar en basic-Programgateway med standardinställningarna för hello lyssnare, serverdelspool, serverdelens http-inställningar och regler. Du kan ändra dessa inställningar toosuit distributionen när hello etablering har lyckats.
Om du redan har ditt webbprogram som definierats med hello serverdelspool i föregående steg, när skapat hello börjar belastningsutjämning.

## <a name="next-steps"></a>Nästa steg

Lär dig hur toocreate anpassade hälsoavsökning genom att besöka [skapa en anpassad hälsoavsökningen](application-gateway-create-probe-portal.md)

Lär dig hur tooconfigure SSL-avlastning och vidta hello kostsamma SSL dekryptering av webbservrar genom att besöka [Konfigurera SSL-avlastning](application-gateway-ssl-arm.md)

<!--Image references-->

[scenario]: ./media/application-gateway-create-gateway-cli-nodejs/scenario.png
[1]: ./media/application-gateway-create-gateway-cli-nodejs/figure1.png
[2]: ./media/application-gateway-create-gateway-cli-nodejs/figure2.png
[3]: ./media/application-gateway-create-gateway-cli-nodejs/figure3.png
