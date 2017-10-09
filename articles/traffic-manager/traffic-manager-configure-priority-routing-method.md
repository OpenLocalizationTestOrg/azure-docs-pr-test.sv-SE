---
title: "aaaConfigure prioritet trafikroutningsmetoden med hjälp av Azure Traffic Manager | Microsoft Docs"
description: "Den här artikeln förklarar hur tooconfigure hello prioritet trafikroutningsmetoden i Traffic Manager"
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
ms.openlocfilehash: dd3e3bb2a727e5ea087cee35962c8e6f7c357282
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-priority-traffic-routing-method-in-traffic-manager"></a>Konfigurera prioritet trafikroutningsmetod i Traffic Manager

Oavsett hello webbplatsläge innehåller Azure Websites redan failover-funktioner för webbplatser inom ett datacenter (även kallat en region). Traffic Manager ger redundans för webbplatser i olika datacenter.

Ett vanligt mönster för-redundans är toosend trafik tooa primära tjänst och ger en uppsättning av identiska Säkerhetskopieringstjänster för redundans. hello följande steg förklarar hur tooconfigure detta prioriteras redundans med Azure-molntjänster och webbplatser:

## <a name="tooconfigure-hello-priority-traffic-routing-method"></a>tooconfigure hello prioritet trafikroutningsmetod

1. Inloggning från en webbläsare, toohello [Azure-portalen](http://portal.azure.com). Om du inte redan har ett konto kan du [registrera dig för en kostnadsfri utvärderingsmånad](https://azure.microsoft.com/free/). 
2. I sökfältet hello-portalen, söka efter hello **Traffic Manager-profiler** och klicka sedan på hello-profilnamn som du vill tooconfigure hello routningsmetod för.
3. I hello **trafikhanterarprofil** bladet, kontrollera att både hello molntjänster och webbplatser som du vill tooinclude i konfigurationen finns.
4. I hello **inställningar** klickar du på **Configuration**, och i hello **Configuration** bladet slutförts enligt följande:
    1. För **trafik routning inställningar**, kontrollera att hello trafikroutningsmetod **prioritet**. Om det inte är det, klickar du på **prioritet** hello listrutan.
    2. Ange hello **övervakaren slutpunktsinställningar** identiska för alla alla slutpunkter i den här profilen på följande sätt:
        1. Välj lämplig hello **protokollet**, och ange hello **Port** nummer. 
        2. För **sökväg** skriver ett snedstreck  */* . toomonitor slutpunkter som du måste ange en sökväg och filnamn. Ett snedstreck ”/” är ett giltigt för hello relativ sökväg och innebär att hello-filen är i hello rotkatalog (standard).
        3. Hello överst på hello-sidan, klickar du på **spara**.
5. I hello **inställningar** klickar du på **slutpunkter**.
6. I hello **slutpunkter** bladet granska hello prioritetsordning för dina slutpunkter. När du väljer hello **prioritet** trafikroutningsmetod, hello ordning hello valt slutpunkter är viktig. Kontrollera hello prioritetsordning slutpunkter.  primär slutpunkt för hello är längst upp. Kontrollera på hello ordning som den visas. alla begäranden som kommer att dirigeras toohello första slutpunkten och om Traffic Manager identifierar den vara felaktiga misslyckas hello trafik automatiskt över toohello nästa slutpunkt. 
7. toochange hello endpoint prioritetsordning, klickar du på hello slutpunkten, och i hello **Endpoint** bladet som visas, klickar du på **redigera** och ändra hello **prioritet** värdet vid behov . 
8. Klicka på **spara** toosave ändra hello endpoint-inställningar.
9. När du har slutfört konfigurationsändringarna klickar du på **spara** på hello hello sidans nederkant.
10. Testa hello ändringar i konfigurationen på följande sätt:
    1.  Sök efter hello Traffic Manager-profilnamn hello portal sökfältet, och klicka på hello Traffic Manager-profilen i hello resultat som hello visas.
    2.  I hello **Traffic Manager** profilen bladet, klickar du på **översikt**.
    3.  Hej **trafikhanterarprofil** bladet visar hello DNS-namnet för nyskapade Traffic Manager-profilen. Detta kan användas av alla klienter (till exempel genom att gå tooit med en webbläsare) tooget dirigeras toohello rätt slutpunkten enligt hello routning. I det här fallet alla förfrågningar är routade toohello första slutpunkten och om Traffic Manager identifierar den vara ohälsosamt misslyckas hello trafik automatiskt över toohello nästa slutpunkt.
11. Redigera hello DNS-post på din auktoritära DNS-server toopoint företagets namn toohello Traffic Manager-domän domännamn när Traffic Manager-profilen fungerar.

![Konfigurera prioritet trafikroutningsmetod med Traffic Manager][1]

## <a name="next-steps"></a>Nästa steg


- Lär dig mer om [viktas trafikroutningsmetod](traffic-manager-configure-weighted-routing-method.md).
- Lär dig mer om [routningsmetoden för prestanda](traffic-manager-configure-performance-routing-method.md).
- Lär dig mer om [geografiska routningsmetoden](traffic-manager-configure-geographic-routing-method.md).
- Lär dig hur för[testa inställningarna för Traffic Manager](traffic-manager-testing-settings.md).

<!--Image references-->
[1]: ./media/traffic-manager-priority-routing-method/traffic-manager-priority-routing-method.png