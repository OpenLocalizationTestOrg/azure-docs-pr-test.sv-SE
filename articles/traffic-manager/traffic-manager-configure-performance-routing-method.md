---
title: "aaaConfigure prestanda trafikroutningsmetoden med hjälp av Azure Traffic Manager | Microsoft Docs"
description: "Den här artikeln förklarar hur tooconfigure Traffic Manager tooroute trafik toohello slutpunkt med lägsta svarstid"
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 6dca6de1-18f7-4962-bd98-6055771fab22
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/20/2017
ms.author: kumud
ms.openlocfilehash: d0ccd4c9de411844c6f36068859265244f4aa52b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-hello-performance-traffic-routing-method"></a>Konfigurera routningsmetoden för hello prestanda-trafik

hello routningsmetoden för prestanda-trafik kan toodirect trafik toohello slutpunkt med hello lägsta svarstid från hello klientnätverk. Hello datacenter med hello lägsta fördröjningen är vanligtvis hello närmaste i geografiska avstånd. Den här trafikroutningsmetod kan inte kontot för realtid ändringarna i nätverkskonfigurationen eller läsa in.

##  <a name="tooconfigure-performance-routing-method"></a>tooconfigure routningsmetoden för prestanda

1. Inloggning från en webbläsare, toohello [Azure-portalen](http://portal.azure.com). Om du inte redan har ett konto kan du [registrera dig för en kostnadsfri utvärderingsmånad](https://azure.microsoft.com/free/). 
2. I sökfältet hello-portalen, söka efter hello **Traffic Manager-profiler** och klicka sedan på hello-profilnamn som du vill tooconfigure hello routningsmetod för.
3. I hello **trafikhanterarprofil** bladet, kontrollera att både hello molntjänster och webbplatser som du vill tooinclude i konfigurationen finns.
4. I hello **inställningar** klickar du på **Configuration**, och i hello **Configuration** bladet slutförts enligt följande:
    1. För **trafik routning inställningar**, för **routningsmetod** Välj **prestanda**.
    2. Ange hello **övervakaren slutpunktsinställningar** identiska för alla alla slutpunkter i den här profilen på följande sätt:
        1. Välj lämplig hello **protokollet**, och ange hello **Port** nummer. 
        2. För **sökväg** skriver ett snedstreck  */* . toomonitor slutpunkter som du måste ange en sökväg och filnamn. Ett snedstreck ”/” är ett giltigt för hello relativ sökväg och innebär att hello-filen är i hello rotkatalog (standard).
        3. Hello överst på hello-sidan, klickar du på **spara**.
5.  Testa hello ändringar i konfigurationen på följande sätt:
    1.  Sök efter hello Traffic Manager-profilnamn hello portal sökfältet, och klicka på hello Traffic Manager-profilen i hello resultat som hello visas.
    2.  I hello **Traffic Manager** profilen bladet, klickar du på **översikt**.
    3.  Hej **trafikhanterarprofil** bladet visar hello DNS-namnet för nyskapade Traffic Manager-profilen. Detta kan användas av alla klienter (till exempel genom att gå tooit med en webbläsare) tooget dirigeras toohello rätt slutpunkten enligt hello routning. I det här fallet är alla begäranden routade toohello slutpunkt med hello lägsta fördröjningen från hello klientnätverk.
6. Redigera hello DNS-post på din auktoritära DNS-server toopoint företagets namn toohello Traffic Manager-domän domännamn när Traffic Manager-profilen fungerar.

![Konfigurera prestanda trafikroutningsmetod med Traffic Manager][1]

## <a name="next-steps"></a>Nästa steg

- Lär dig mer om [viktas trafikroutningsmetod](traffic-manager-configure-weighted-routing-method.md).
- Lär dig mer om [prioritet routningsmetoden](traffic-manager-configure-priority-routing-method.md).
- Lär dig mer om [geografiska routningsmetoden](traffic-manager-configure-geographic-routing-method.md).
- Lär dig hur för[testa inställningarna för Traffic Manager](traffic-manager-testing-settings.md).

<!--Image references-->
[1]: ./media/traffic-manager-performance-routing-method/traffic-manager-performance-routing-method.png