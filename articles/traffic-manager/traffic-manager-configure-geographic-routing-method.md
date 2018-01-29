---
title: "Konfigurera routningsmetoden för geografisk trafik med hjälp av Azure Traffic Manager | Microsoft Docs"
description: "Den här artikeln förklarar hur du konfigurerar geografiska trafikroutningsmetod med hjälp av Azure Traffic Manager"
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
ms.openlocfilehash: 13190189074b24b2d28cd3ce46cf8571f3e1e1d1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="configure-the-geographic-traffic-routing-method-using-traffic-manager"></a>Konfigurera geografiska trafikroutningsmetod med Traffic Manager

Geografisk trafikroutningsmetod kan du dirigera trafik till specifika slutpunkter baserat på enhetens geografiska plats där begäran har sitt ursprung. Den här kursen visar hur du skapar en Traffic Manager-profil med den här routningsmetoden och konfigurerar slutpunkter för att ta emot trafik från specifika geografiska områden.

## <a name="create-a-traffic-manager-profile"></a>Skapa en Trafikhanterarprofil

1. Logga in på [Azure Portal](http://portal.azure.com) från en webbläsare. Om du inte redan har ett konto kan du [registrera dig för en kostnadsfri utvärderingsmånad](https://azure.microsoft.com/free/).
2. På navmenyn klickar du på **ny** > **nätverk** > **se alla**, och klicka sedan på **trafikhanterarprofil** att öppna den **skapa Traffic Manager-profilen** bladet.
3. På den **skapa Traffic Manager-profilen** bladet:
    1. Ange ett namn för din profil. Det här namnet måste vara unikt inom trafficmanager.net zonen och leder till att DNS-namnet <profilename>, trafficmanager.net som ska användas för att komma åt Traffic Manager-profilen.
    2. Välj den **geografiska** routningsmetoden.
    3. Välj den prenumeration som du vill skapa den här profilen under.
    4. Använd en befintlig resursgrupp eller skapa en ny resursgrupp om du vill placera den här profilen under. Om du väljer att skapa en ny resursgrupp med det **resursgruppsplats** listrutan för att ange platsen för resursgruppen. Den här inställningen refererar till platsen där resursgruppen och har ingen inverkan på Traffic Manager-profilen som ska distribueras globalt.
    5. När du klickar på **skapa**, Traffic Manager-profilen har skapats och distribuerats globalt.

![Skapa en Traffic Manager-profil](./media/traffic-manager-geographic-routing-method/create-traffic-manager-profile.png)

## <a name="add-endpoints"></a>Lägga till slutpunkter

1. Sök efter Traffic Manager-profilnamn som du skapade i portalens sökfältet och klicka på resultatet när det visas.
2. Gå till **inställningar** -> **slutpunkter** i Traffic Manager-bladet.
3. Klicka på **Lägg till** att visa den **Lägg till slutpunkt** bladet.
3. I den **slutpunkter** bladet klickar du på **Lägg till** och i den **lägga till slutpunkten** slutföra bladet som visas på följande sätt:
4. Välj **typen** beroende på vilken typ av slutpunkt som du lägger till. Geografisk routning profiler som används i produktionen, rekommenderar vi med kapslade slutpunktstyper som innehåller en underordnad profil med mer än en slutpunkt. Mer information finns i [vanliga frågor och svar om geografiska trafikroutningsmetoder](traffic-manager-FAQs.md).
5. Ange ett **Namn** som du vill använda för att identifiera den här slutpunkten.
6. Vissa fält i det här bladet beror på vilken typ av slutpunkt som du lägger till:
    1. Om du lägger till en Azure-slutpunkt, väljer du den **mål resurstypen** och **mål** baserat på den resurs du vill dirigera trafik till
    2. Om du lägger till en **externa** slutpunkt, ange den **fullständigt kvalificerat domännamn (FQDN)** för slutpunkten.
    3. Om du lägger till en **kapslade endpoint**, Välj den **mål resurs** som motsvarar den underordnade profilen som du vill använda och ange den **minsta antal för underordnade slutpunkter**.
7. I avsnittet Geo-mappning Använd listrutan för att lägga till regionerna där du vill att trafik skickas till den här slutpunkten. Du måste lägga till minst en region och du kan ha flera regioner som mappats.
8. Upprepa detta för alla slutpunkter som du vill lägga till under den här profilen

![Lägg till en Traffic Manager-slutpunkt](./media/traffic-manager-geographic-routing-method/add-traffic-manager-endpoint.png)

## <a name="use-the-traffic-manager-profile"></a>Använd Traffic Manager-profilen
1.  I portalens sökfältet, söker du efter den **trafikhanterarprofil** namn som du skapade i föregående avsnitt och klicka på traffic manager-profil i resultaten som visas.
2. I den **trafikhanterarprofil** bladet, klickar du på **översikt**.
3. Den **trafikhanterarprofil** bladet visar DNS-namnet för nyskapade Traffic Manager-profilen. Detta kan användas av alla klienter (till exempel genom att navigera till den med hjälp av en webbläsare) att hämta dirigeras till rätt slutpunkten som bestäms av typen routning.  Vid geografiska routning Traffic Manager tittar på käll-IP för denna inkommande begäran och anger den region där ursprung. Om den regionen mappas till en slutpunkt dirigeras trafik till där. Om den här regionen inte är mappad till en slutpunkt, returnerar Traffic Manager ett frågesvar inga data.

## <a name="next-steps"></a>Nästa steg

- Lär dig mer om [geografiska trafikroutningsmetoden](traffic-manager-routing-methods.md#geographic).
- Lär dig hur du [testa inställningarna för Traffic Manager](traffic-manager-testing-settings.md).
