---
title: "aaaConfigure geografiska trafikroutningsmetoden med hjälp av Azure Traffic Manager | Microsoft Docs"
description: "Den här artikeln förklarar hur tooconfigure hello geografiska trafikroutningsmetod med hjälp av Azure Traffic Manager"
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2017
ms.author: kumud
ms.openlocfilehash: 4142389211ae54e7feea6564641e01e4477491e8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-hello-geographic-traffic-routing-method-using-traffic-manager"></a>Konfigurera hello geografiska trafikroutningsmetod med Traffic Manager

hello geografiska trafikroutningsmetod kan toodirect trafik toospecific slutpunkter baserat på hello geografiska plats där hello begäran har sitt ursprung. Den här kursen visar hur toocreate en Traffic Manager-profilen med det här routningsmetoden och konfigurera hello slutpunkter tooreceive trafik från specifika geografiska områden.

## <a name="create-a-traffic-manager-profile"></a>Skapa en Trafikhanterarprofil

1. Inloggning från en webbläsare, toohello [Azure-portalen](http://portal.azure.com). Om du inte redan har ett konto kan du [registrera dig för en kostnadsfri utvärderingsmånad](https://azure.microsoft.com/free/).
2. Hej hubbmenyn, klicka på **ny** > **nätverk** > **se alla**, och klicka sedan på **trafikhanterarprofil**tooopen hello **skapa Traffic Manager-profilen** bladet.
3. På hello **skapa Traffic Manager-profilen** bladet:
    1. Ange ett namn för din profil. Det här namnet måste toobe unika inom hello trafficmanager.net zon och leder hello DNS-namnet <profilename>, trafficmanager.net som kommer att använda tooaccess Traffic Manager-profilen.
    2. Välj hello **geografiska** routningsmetoden.
    3. Välj hello-prenumeration som du vill att toocreate profilen under.
    4. Använd en befintlig resursgrupp eller skapa en ny resurs grupp tooplace profilen under. Om du väljer toocreate en ny resursgrupp, använda hello **resursgruppsplats** listrutan toospecify hello platsen för hello resursgruppen. Den här inställningen refererar toohello platsen för hello resursgruppen och har ingen inverkan på hello Traffic Manager-profilen som ska distribueras globalt.
    5. När du klickar på **skapa**, Traffic Manager-profilen har skapats och distribuerats globalt.

![Skapa en Traffic Manager-profil](./media/traffic-manager-geographic-routing-method/create-traffic-manager-profile.png)

## <a name="add-endpoints"></a>Lägga till slutpunkter

1. Sök efter hello Traffic Manager profilnamn som du skapade i sökfältet hello portal och klicka på hello resultat när det visas.
2. Navigera för**inställningar** -> **slutpunkter** hello Traffic Manager-bladet.
3. Klicka på **Lägg till** tooshow hello **Lägg till slutpunkt** bladet.
3. I hello **slutpunkter** bladet, klickar du på **Lägg till** och i hello **lägga till slutpunkten** slutföra bladet som visas på följande sätt:
4. Välj **typen** beroende på hello slutpunkt som du lägger till. Geografisk routning profiler som används i produktionen, rekommenderar vi med kapslade slutpunktstyper som innehåller en underordnad profil med mer än en slutpunkt. Mer information finns i [vanliga frågor och svar om geografiska trafikroutningsmetoder](traffic-manager-FAQs.md).
5. Ange en **namn** som du vill använda toorecognize den här slutpunkten.
6. Vissa fält i det här bladet är beroende av hello typ av slutpunkt som du lägger till:
    1. Om du lägger till en Azure slutpunkt väljer hello **mål resurstypen** och hello **mål** baserat på hello resursen du vill toodirect trafik till
    2. Om du lägger till en **externa** slutpunkt, ange hello **fullständigt kvalificerat domännamn (FQDN)** för slutpunkten.
    3. Om du lägger till en **kapslade endpoint**väljer hello **mål resurs** som motsvarar toohello underordnade profilen toouse och om du vill ange hello **minsta underordnade slutpunkter räkna**.
7. I hello Geo-mappning avsnitt, Använd hello nedrullningsbara tooadd hello regioner där du vill att trafik skickas toobe toothis slutpunkt. Du måste lägga till minst en region och du kan ha flera regioner som mappats.
8. Upprepa detta för alla slutpunkter önskade tooadd under den här profilen

![Lägg till en Traffic Manager-slutpunkt](./media/traffic-manager-geographic-routing-method/add-traffic-manager-endpoint.png)

## <a name="use-hello-traffic-manager-profile"></a>Använd hello Traffic Manager-profilen
1.  I sökfältet hello-portalen, söka efter hello **trafikhanterarprofil** namn som du skapade i föregående avsnitt hello och klicka på hello traffic manager-profil i hello resultatet som hello visas.
2. I hello **trafikhanterarprofil** bladet, klickar du på **översikt**.
3. Hej **trafikhanterarprofil** bladet visar hello DNS-namnet för nyskapade Traffic Manager-profilen. Detta kan användas av alla klienter (till exempel genom att gå tooit med en webbläsare) tooget dirigeras toohello rätt slutpunkten enligt hello routning.  Hello gäller geografiska routning, Traffic Manager söker i hello käll-IP för hello inkommande begäran och anger hello region där ursprung. Om den regionen är mappade tooan slutpunkt, är trafik dirigeras toothere. Om den här regionen inte är mappad tooan slutpunkt, returnerar Traffic Manager ett frågesvar inga data.

## <a name="next-steps"></a>Nästa steg

- Lär dig mer om [geografiska trafikroutningsmetoden](traffic-manager-routing-methods.md#geographic).
- Lär dig hur för[testa inställningarna för Traffic Manager](traffic-manager-testing-settings.md).
