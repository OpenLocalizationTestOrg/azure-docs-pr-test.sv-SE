---
title: "aaaOverview för flera innehavare säkerhetskopiera slutar med Azure Programgateway | Microsoft Docs"
description: "Den här sidan innehåller en översikt av hello Programgateway stöd för flera innehavare-servrar."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: 
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: gwallace
ms.openlocfilehash: b7da8c9c68e34bd83ad2b828fab62c00caea354a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="application-gateway-support-for-multi-tenant-back-ends"></a>Stöd för serverdelar för flera klientorganisationer i Application Gateway

Azure Application Gateway stöder skalningsuppsättningar för virtuella datorer, nätverksgränssnitt, offentliga och privata IP-adresser och fullständigt kvalificerade domännamn (FQDN) som en del av dess serverdelspooler. Som standard Programgateway ändras inte hello inkommande HTTP-värdadressen hello-klient och skickar hello huvudet oförändrat toohello serverdel. Det finns många tjänster som [Azure Web Apps](../app-service-web/app-service-web-overview.md) och [API Management](../api-management/api-management-key-concepts.md) som är multitenant som förlitar sig på en specifik värdadressen eller SNI-tillägg tooresolve toohello korrekt slutpunkt. Application Gateway stöder nu hello möjligheten för användare toooverwrite hello inkommande HTTP-värdadressen baserat på hello serverdelens HTTP-inställningar. Detta gör det möjligt att använda API-hantering och Azure-webbappar med serverdelar för flera klientorganisationer. Den här funktionen är tillgänglig för både hello standard och Brandvägg SKU. Flera innehavare serverdelsnätverk support också fungerar tillsammans med SSL-avslutning- och slutdatum tooend SSL-scenarier.

![Scenario för webbappar](./media/application-gateway-web-app-overview/scenario.png)

hello möjlighet toospecify en värd åsidosättning har definierats på hello HTTP-inställningar och kan vara tillämpade tooany tillbaka avsluta pool när regeln skapades. Flera innehavare tillbaka slutar stöd hello följande två sätt att åsidosätta värdadress och SNI-tillägg.

1. hello möjlighet tooset hello värden namnet tooa fast värde i hello HTTP-inställningar. Den här funktionen innebär att hello värdadressen åsidosätts toothis värde för alla trafik toohello serverdelspool där hello HTTP-inställningar används. När du använder slutet tooend SSL används åsidosatt värddatorns namn i hello SNI-tillägg. Den här funktionen möjliggör scenarier där en serverdel poolen grupp förväntar sig ett värdhuvud som skiljer sig från hello inkommande värdhuvud för kunden.

2. hello möjlighet tooderive hello värdnamnet från hello IP eller FQDN för hello serverdel poolmedlemmar. HTTP-inställningar innehåller också ett värdnamn för alternativet toopick hello från en serverdel poolmedlem FQDN om har konfigurerats med alternativet hello tooderive värdnamn från en enskild serverdel poolmedlem. När du använder slutet tooend SSL, värdnamn är härledd från hello FQDN och används i hello SNI-tillägg. Den här funktionen möjliggör scenarier där en serverdelspool kan ha två eller flera flera innehavare PaaS tjänster som Azure-webbappar och hello begäran värden huvud tooeach medlem innehåller hello värdnamn som härletts från dess FQDN.

> [!NOTE]
> I båda hello föregående fall påverkar hello inställningarna endast hello live trafik beteende och inte hello hälsa avsökningen beteende. Anpassad avsökningar redan stöd hello möjlighet toospecify ett värdhuvud i hello avsökningen konfiguration. Anpassade avsökningar stöder nu också hello möjlighet tooderive hello värden sidhuvud beteende från hello konfigurerad HTTP-inställningar. Den här konfigurationen kan anges med hjälp av hello `PickHostNameFromback endAddress` parameter i hello avsökningen konfiguration. Både hello avsökning och hello HTTP-inställningar måste vara ändrade tooreflect hello korrekt konfiguration för end tooend funktioner toowork.

Med den här funktionen kan ange kunder hello alternativ i hello HTTP-inställningar och anpassade avsökningar toohello har konfigurerats korrekt. Den här inställningen sedan knutna tooa lyssnare och en tillbaka avslutas pool med hjälp av en regel.

## <a name="next-steps"></a>Nästa steg

Lär dig hur tooset upp en Programgateway med en webbapp som en bakåt avsluta poolmedlem genom att besöka: [konfigurera App Service web apps med Programgateway](application-gateway-web-app-powershell.md)
