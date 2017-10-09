---
title: "en sökväg-baserad aaaCreate regel - Azure Application Gateway - Azure-portalen | Microsoft Docs"
description: "Lär dig hur hello toocreate en sökväg-baserade regler för en Programgateway med hjälp av portalen"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 87bd93bc-e1a6-45db-a226-555948f1feb7
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/03/2017
ms.author: gwallace
ms.openlocfilehash: 21cb52c426ca5f7dfedf07a96e87fbc85d243647
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-path-based-rule-for-an-application-gateway-by-using-hello-portal"></a>Skapa en sökväg-baserade regel för en Programgateway med hello-portalen

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-url-route-portal.md)
> * [PowerShell och Azure Resource Manager](application-gateway-create-url-route-arm-ps.md)
> * [Azure CLI 2.0](application-gateway-create-url-route-cli.md)

URL-sökväg-baserade routning kan du tooassociate rutter baserat på hello URL-sökvägen för Http-begäran. Den kontrollerar om det finns en väg tooa backend-adresspool för hello-URL som anges i hello Programgateway och skickar hello nätverket trafik toohello definierats backend-adresspool. Ett vanligt användningsområde för URL-baserade routning är tooload begäranden för olika typer av innehåll toodifferent backend-serverpooler.

URL-baserade routning introducerar en ny regel typen tooapplication gateway. Programgateway har två regeltyper: basic och sökväg-baserade regler. Hej grundläggande regeltyp, tillhandahåller resursallokering tjänst för hello backend-pooler när sökvägen regler dessutom tooround robin distribution, också beaktas sökvägar för hello URL-begäran vid val hello lämpliga serverdelspool.

## <a name="scenario"></a>Scenario

hello genomgår följande scenario skapar en sökväg-baserade regel i en befintlig gateway för programmet.
hello scenariot förutsätter att du har redan följt hello stegen för[skapa en Programgateway](application-gateway-create-gateway-portal.md).

![URL-väg][scenario]

## <a name="createrule"></a>Skapa hello sökväg-baserade regel

En sökväg-baserade regel kräver sin egen lyssnare, innan du skapar hello regeln vara att tooverify som du har en tillgänglig lyssnare toouse.

### <a name="step-1"></a>Steg 1

Navigera toohello [Azure-portalen](http://portal.azure.com) och välj en befintlig gateway för programmet. Klicka på **regler**

![Översikt över Application Gateway][1]

### <a name="step-2"></a>Steg 2

Klicka på **sökväg-baserade** knappen tooadd en ny sökväg-baserade regel.

### <a name="step-3"></a>Steg 3

Hej **Lägg till sökväg-baserade regler** bladet innehåller två avsnitt. hello första avsnittet är där du har definierat hello lyssnare, hello namnet på regeln för hello och hello standardinställningarna för sökvägen. hello standardinställningarna för sökvägen är för vägar som inte omfattas av hello dirigering av anpassade sökväg-baserad. Hej andra avsnittet av hello **Lägg till sökväg-baserade regler** bladet är där du definierar hello sökväg-baserade regler för sig själva.

**Grundläggande inställningar**

* **Namnet** -värdet är ett eget namn toohello regeln som är tillgänglig i hello portal.
* **Lyssnare** -värdet är hello lyssnare som används för hello regeln.
* **Standard serverdelspool** -inställningen är hello-inställning som definierar hello backend-toobe används för hello standardregel
* **Standardinställningarna för HTTP-** -inställningen är hello-inställning som definierar hello HTTP inställningar toobe används för hello Standardregeln.

**Sökväg-baserade regler**

* **Namnet** -värdet är ett eget namn toopath-baserade regler.
* **Sökvägar** -inställningen definierar hello sökväg hello regeln söker efter när vidarebefordrar trafik
* **Serverdelspool** -inställningen är hello-inställning som definierar hello backend-toobe används för hello regel
* **HTTP-inställningen** -inställningen är hello-inställning som definierar hello HTTP inställningar toobe används för hello regeln.

> [!IMPORTANT]
> Sökvägar: hello lista över sökvägen mönster toomatch. Var och en måste börja med / och hello endast placera en ”\*” tillåts är hello slutet. Giltiga exempel är /xyz, /xyz* eller/xyz / *.  

![Lägg till sökväg-baserade regler bladet med information som har fyllt i][2]

Lägga till en sökväg-baserade regler tooan är befintliga Programgateway en enkel process hello-portalen. När en sökväg-baserade regler har skapats, kan det vara redigerade tooadd ytterligare regler. 

![lägga till ytterligare sökväg-baserade regler][3]

Det här steget konfigurerar en väg baserat på sökvägen. Det är viktigt toounderstand att begäranden inte skrivas om, som begäran kommer i Programgateway inspektera hello begäran och basic på hello url mönster skickar hello begäran toohello lämplig backend.

## <a name="next-steps"></a>Nästa steg

hur tooconfigure SSL-avlastning med Azure Application Gateway, se toolearn [Konfigurera SSL-avlastning](application-gateway-ssl-portal.md)

[1]: ./media/application-gateway-create-url-route-portal/figure1.png
[2]: ./media/application-gateway-create-url-route-portal/figure2.png
[3]: ./media/application-gateway-create-url-route-portal/figure3.png
[scenario]: ./media/application-gateway-create-url-route-portal/scenario.png
