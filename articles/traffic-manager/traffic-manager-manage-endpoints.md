---
title: aaaManage slutpunkter i Azure Traffic Manager | Microsoft Docs
description: "Den här artikeln beskriver hur du lägger till, tar bort, aktiverar och inaktiverar slutpunkter från Azure Traffic Manager."
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: ade2bbc2-35a7-43c5-8001-4698f7254526
ms.service: traffic-manager
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/08/2017
ms.author: kumud
ms.openlocfilehash: fc65874ae2eaeb6fca5d8c4f33403c258307bdb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-disable-enable-or-delete-endpoints"></a>Lägga till, inaktivera, aktivera eller ta bort slutpunkter

funktionen för hello Web Apps i Azure App Service tillhandahåller redan redundans och resursallokering routningsfunktionen för webbplatser inom ett datacenter, oavsett webbplatsläge hello. Azure Traffic Manager kan toospecify redundans och routning av nätverkstrafik för resursallokering för webbplatser och molntjänster i olika datacenter. hello första steg nödvändiga tooprovide att funktionalitet är tooadd hello molnet webbplats eller slutpunkt tooTraffic Manager.

Du kan också inaktivera enskilda slutpunkter som ingår i en Traffic Manager-profil. Om du inaktiverar en slutpunkt där den som en del av hello profil, men hello profilen fungerar som om hello slutpunkten inte ingår i den. Den här åtgärden är användbar för att tillfälligt ta bort en slutpunkt som är i underhållsläge eller som omdistribueras. När hello slutpunkten är igång igen kan aktiveras den.

> [!NOTE]
> Om du inaktiverar en slutpunkt har inget toodo med dess Distributionsstatus i Azure. En felfri slutpunkt förblir igång och kan tooreceive trafik även om inaktiverad i Traffic Manager. Dessutom påverkar inte inaktiveringen av en slutpunkt i en profil dess status i en annan profil.

## <a name="tooadd-a-cloud-service-or-an-app-service-endpoint-tooa-traffic-manager-profile"></a>tooadd en molnbaserad tjänst eller en App service endpoint tooa trafikhanterarprofil

1. Inloggning från en webbläsare, toohello [Azure-portalen](http://portal.azure.com).
2. I sökfältet hello-portalen, söka efter hello **trafikhanterarprofil** namn som du vill toomodify och klicka sedan på hello Traffic Manager-profilen i hello resulterar det hello visas.
3. I hello **trafikhanterarprofil** bladet i hello **inställningar** klickar du på **slutpunkter**.
4. I hello **slutpunkter** bladet som visas, klickar du på **Lägg till**.
5. I hello **lägga till slutpunkten** bladet slutförts enligt följande:
    1. Klicka på **Azure-slutpunkt** för **Typ**.
    2. Ange en **namn** som du vill använda toorecognize den här slutpunkten.
    3. För **mål resurstypen**, från hello listrutan, Välj hello lämplig resurstyp.
    4. För **mål resurs**, från hello listrutan, Välj hello lämpliga målresurs tooshow hello lista resurser under hello samma prenumeration i hello **resurser bladet**. I hello **resurs** bladet som visas, Välj hello tjänsten som du vill tooadd som hello första slutpunkten.
    5. För **Prioritet**väljer du **1**. Detta resulterar i all trafik toothis endpoint om den är felfri.
    6. Behåll **Lägg till som inaktiverad** som avmarkerat.
    7. Klicka på **OK**
6.  Upprepa steg 4 och 5 tooadd hello nästa Azure slutpunkt. Se till att tooadd med dess **prioritet** värdet på **2**.
7.  När hello lägga till båda slutpunkterna är klar, visas de i hello **trafikhanterarprofil** bladet tillsammans med deras övervakning status som **Online**.

> [!NOTE]
> När du lägger till eller ta bort en slutpunkt från en profil med hjälp av hello *redundans* trafikroutningsmetoden, hello prioritetslistan för redundans kan inte sorteras de som du vill. Du kan justera hello prioritetslistan för redundans på konfigurationssidan för hello hello ordning. Mer information finns i [Konfigurera trafikroutning för redundans](traffic-manager-configure-failover-routing-method.md).

## <a name="toodisable-an-endpoint"></a>toodisable en slutpunkt

1. Inloggning från en webbläsare, toohello [Azure-portalen](http://portal.azure.com).
2. I sökfältet hello-portalen, söka efter hello **trafikhanterarprofil** namn du vill toomodify och klicka sedan på hello Traffic Manager-profilen i hello resultat som visas.
3. I hello **trafikhanterarprofil** bladet i hello **inställningar** klickar du på **slutpunkter**. 
4. Klicka på hello-slutpunkt som du vill toodisable, och klicka sedan på hello **Endpoint** bladet som visas, klickar du på **redigera**.
5. I hello **Endpoint** bladet ändra hello endpoint status för**inaktiverad**, och klicka sedan på **spara**.
6. Klienter fortsätta toosend trafik toohello slutpunkt för hello varaktighet Time-to-Live (TTL). Du kan ändra hello TTL-värde på hello konfigurationssidan för hello Traffic Manager-profilen.

## <a name="tooenable-an-endpoint"></a>tooenable en slutpunkt

1. Inloggning från en webbläsare, toohello [Azure-portalen](http://portal.azure.com).
2. I sökfältet hello-portalen, söka efter hello **trafikhanterarprofil** namn du vill toomodify och klicka sedan på hello Traffic Manager-profilen i hello resultat som visas.
3. I hello **trafikhanterarprofil** bladet i hello **inställningar** klickar du på **slutpunkter**. 
4. Klicka på hello-slutpunkt som du vill toodisable, och klicka sedan på hello **Endpoint** bladet som visas, klickar du på **redigera**.
5. I hello **Endpoint** bladet ändra hello endpoint status för**aktiverad**, och klicka sedan på **spara**.
6. Klienter fortsätta toosend trafik toohello slutpunkt för hello varaktighet Time-to-Live (TTL). Du kan ändra hello TTL-värde på hello konfigurationssidan för hello Traffic Manager-profilen.

## <a name="toodelete-an-endpoint"></a>toodelete en slutpunkt

1. Inloggning från en webbläsare, toohello [Azure-portalen](http://portal.azure.com).
2. I sökfältet hello-portalen, söka efter hello **trafikhanterarprofil** namn du vill toomodify och klicka sedan på hello Traffic Manager-profilen i hello resultat som visas.
3. I hello **trafikhanterarprofil** bladet i hello **inställningar** klickar du på **slutpunkter**. 
4. Klicka på hello-slutpunkt som du vill toodisable, och klicka sedan på hello **Endpoint** bladet som visas, klickar du på **redigera**.
5. I hello **Endpoint** bladet ändra hello endpoint status för**aktiverad**, och klicka sedan på **spara**.


## <a name="next-steps"></a>Nästa steg

* [Hantera Traffic Manager-profiler](traffic-manager-manage-profiles.md)
* [Konfigurera routningsmetoder](traffic-manager-configure-routing-method.md)
* [Felsök degraderat tillstånd i Traffic Manager](traffic-manager-troubleshooting-degraded.md)
* [Prestandaöverväganden för Traffic Manager](traffic-manager-performance-considerations.md)
* [Åtgärder för Traffic Manager (REST API-referens)](http://go.microsoft.com/fwlink/p/?LinkID=313584)

