---
title: "Skapa en regel för sökväg-baserade - Azure Application Gateway - Azure-portalen | Microsoft Docs"
description: "Lär dig hur du skapar en sökväg-baserade regler för en Programgateway med hjälp av portalen"
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
ms.openlocfilehash: c184e94a04cfbdedcae70ed154aeb7dd134d1baf
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-path-based-rule-for-an-application-gateway-by-using-the-portal"></a>Skapa en sökväg-baserade regel för en Programgateway med hjälp av portalen

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-url-route-portal.md)
> * [PowerShell och Azure Resource Manager](application-gateway-create-url-route-arm-ps.md)
> * [Azure CLI 2.0](application-gateway-create-url-route-cli.md)

URL-sökväg-baserad routning kan du associera rutter baserat på en URL-sökväg för Http-begäran. Den kontrollerar om det finns en väg till en backend-adresspool som konfigurerats för den URL som visas i Programgatewayen och skickar nätverkstrafiken till definierade backend-poolen. Ett vanligt användningsområde för URL-baserade routning är att belastningsutjämna förfrågningar för olika typer av innehåll till olika backend-serverpooler.

URL-baserade routning introducerar en ny regeltyp av i Programgateway. Programgateway har två regeltyper: basic och sökväg-baserade regler. Grundläggande regeltypen resursallokering tjänst för backend-pooler när sökväg-baserade regler förutom resursallokering (round robin), också beaktas sökvägar för den begärda Webbadressen när du väljer rätt serverdelspoolen.

## <a name="scenario"></a>Scenario

Följande scenario genomgår skapar en sökväg-baserade regel i en befintlig gateway för programmet.
Scenariot förutsätter att du redan har följt stegen för att [skapa en Programgateway](application-gateway-create-gateway-portal.md).

![URL-väg][scenario]

## <a name="createrule"></a>Skapa regel sökväg-baserade

En sökväg-baserade regel kräver sin egen lyssnare, innan du skapar regeln bör du kontrollera du ha en lyssnare som är tillgängliga att använda.

### <a name="step-1"></a>Steg 1

Navigera till den [Azure-portalen](http://portal.azure.com) och välj en befintlig gateway för programmet. Klicka på **regler**

![Översikt över Application Gateway][1]

### <a name="step-2"></a>Steg 2

Klicka på **sökväg-baserade** för att lägga till en ny sökväg-baserade regel.

### <a name="step-3"></a>Steg 3

Den **Lägg till sökväg-baserade regler** bladet innehåller två avsnitt. Det första avsnittet är där du har definierat lyssnaren, namnet på regeln och standardinställningarna för sökvägen. Standardinställningarna för sökvägen är för vägar som inte omfattas av anpassade sökväg-baserade vägen. Det andra avsnittet av den **Lägg till sökväg-baserade regler** bladet är där du kan definiera regler baserat på sökvägen sig själva.

**Grundläggande inställningar**

* **Namnet** -värdet är ett eget namn för regeln som är tillgänglig i portalen.
* **Lyssnare** -värdet är lyssnare som används för regeln.
* **Standard serverdelspool** -inställningen är den inställning som definierar backend som ska användas för Standardregeln
* **Standardinställningarna för HTTP-** -inställningen är den inställning som definierar HTTP-inställningar som ska användas för Standardregeln.

**Sökväg-baserade regler**

* **Namnet** -värdet är ett eget namn för regeln för sökväg-baserade.
* **Sökvägar** -den här inställningen definierar sökvägen regeln söker efter när vidarebefordrar trafik
* **Serverdelspool** -inställningen är den inställning som definierar backend som ska användas för regeln
* **HTTP-inställningen** -inställningen är den inställning som definierar HTTP-inställningar som ska användas för regeln.

> [!IMPORTANT]
> Sökvägar: Listan över sökvägen mönster som matchar. Var och en måste börja med / och endast en ”\*” tillåts i slutet. Giltiga exempel är /xyz, /xyz* eller/xyz / *.  

![Lägg till sökväg-baserade regler bladet med information som har fyllt i][2]

Lägger till en sökväg-baserade regel i en befintlig Programgateway är en enkel process via portalen. När en sökväg-baserade regler har skapats kan redigeras den för att lägga till ytterligare regler. 

![lägga till ytterligare sökväg-baserade regler][3]

Det här steget konfigurerar en väg baserat på sökvägen. Det är viktigt att förstå som begär inte anges som begäran kommer i Programgateway inspektera begäran och basic i url-mönster skickar en begäran till lämplig serverdel.

## <a name="next-steps"></a>Nästa steg

Information om hur du konfigurerar SSL-avlastning med Azure Programgateway finns [Konfigurera SSL-avlastning](application-gateway-ssl-portal.md)

[1]: ./media/application-gateway-create-url-route-portal/figure1.png
[2]: ./media/application-gateway-create-url-route-portal/figure2.png
[3]: ./media/application-gateway-create-url-route-portal/figure3.png
[scenario]: ./media/application-gateway-create-url-route-portal/scenario.png
