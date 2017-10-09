---
title: "aaaEnabling end tooend SSL på Azure Programgateway | Microsoft Docs"
description: "Den här sidan innehåller en översikt över hello Programgateway slutet tooend stöd för SSL."
documentationcenter: na
services: application-gateway
author: amsriva
manager: rossort
editor: amsriva
ms.assetid: 3976399b-25ad-45eb-8eb3-fdb736a598c5
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 07/19/2017
ms.author: amsriva
ms.openlocfilehash: c5cb398a1e7d9a08662a3120baad98edb5575917
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-end-tooend-ssl-with-application-gateway"></a>Översikt över slutet tooend SSL med Programgateway

Programgateway stöder SSL-avslutning vid hello gateway, efter vilken trafiken flödar vanligtvis okrypterade toohello backend-servrar. Den här funktionen tillåter web servrar toobe unburdened från kostsamma kryptering och dekryptering kostnader. Men för vissa kunder okrypterad kommunikation toohello backend-servrarna är inte en godkänd alternativ. Den här okrypterade kommunikationen kan bero på att toosecurity krav, krav på efterlevnad eller hello program kan endast ha en säker anslutning. För sådana program stöder Programgateway slutet tooend SSL kryptering.

## <a name="overview"></a>Översikt

End tooend SSL kan du toosecurely överföra känsliga data toohello backend krypterade medan fortfarande dra nytta av hello fördelarna med funktioner för belastningsutjämning Layer 7 vilka Programgateway innehåller. Vissa av dessa funktioner är cookie-baserad session tillhörighet, URL-baserade routning, stöd för routning baserat på webbplatser eller möjlighet tooinject X - vidarebefordrad-* huvuden.

När konfigurerad med end tooend SSL-kommunikation läge, avslutas hello SSL-sessioner på hello gateway Programgateway och dekrypterar användaraktiviteten. Den gäller sedan hello konfigurerade regler tooselect en lämplig backend poolen instans tooroute trafik till. Programgateway sedan initierar en ny SSL-anslutning toohello backend-server och krypterar data med hjälp av hello backend-serverns offentliga nyckelcertifikat innan de överförs hello begäran toohello backend. End tooend SSL är aktiverat genom att ange protokollinställningen i BackendHTTPSetting tooHTTPS, som sedan tillämpas tooa serverdelspool. Varje backend-servern i hello serverdelspool med end tooend SSL aktiverat måste konfigureras med en certifikat tooallow säker kommunikation.

![tooend ssl scenariot][1]

I det här exemplet är begäranden som använder TLS1.2 routade toobackend servrar i Pool1 med end tooend SSL.

## <a name="end-tooend-ssl-and-whitelisting-of-certificates"></a>Avsluta tooend SSL och vitlistning av certifikat

Programgateway kommunicerar endast med kända backend-instanser som har godkända sina certifikat med hello Programgateway. tooenable vitlistning av certifikat, måste du överföra hello offentliga nyckeln för backend-servern certifikat toohello Programgateway (inte hello rotcertifikat). Endast anslutningar tooknown och listan över godkända serverdelar tillåts sedan. hello återstående serverdelar resulterar i en gateway-fel. Självsignerade certifikat är enbart för testningsändamål och rekommenderas inte för produktions-arbetsbelastningar. Dessa certifikat har toobe godkända med hello Programgateway enligt beskrivningen i föregående steg innan de kan användas hello.

## <a name="next-steps"></a>Nästa steg

När du lära dig mer om slutet tooend SSL, gå för[aktivera slutet tooend SSL på Programgateway](application-gateway-end-to-end-ssl-powershell.md) toocreate som en gateway som använder avslutas tooend SSL.

<!--Image references-->

[1]: ./media/application-gateway-backend-ssl/scenario.png
