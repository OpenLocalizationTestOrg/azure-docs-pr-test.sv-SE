---
title: aaaAuthenticate med Mobile Engagement REST API - manuell installation
description: "Beskriver hur toomanually in autentisering för Mobile Engagement REST API: er"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 2e79f9c9-41e4-45ac-b427-3b8338675163
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 3884f94afcd6b9a62bfcf498fb6ee84bb6e837b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="authenticate-with-mobile-engagement-rest-apis---manual-setup"></a>Autentisera med Mobile Engagement REST API - manuell installation
Detta är ett tillägg dokumentationen för[autentisera med Mobile Engagement REST API: er](mobile-engagement-api-authentication.md). Kontrollera att du läser den första tooget hello kontext. Här beskrivs ett annat sätt toodo hello enstaka installationsprogrammet för att skapa autentiseringen för hello Mobile Engagement REST API: er med hjälp av hello Azure-portalen. 

> [!NOTE]
> hello anvisningarna nedan baseras på detta [guide för Active Directory](../azure-resource-manager/resource-group-create-service-principal-portal.md) och anpassas för vad som krävs för autentisering för Mobile Engagement API: er. Läs tooit så om du vill toounderstand hello stegen nedan i detalj. 
> 
> 

1. Inloggningen tooyour Azure-konto via hello [klassiska portalen](https://manage.windowsazure.com/).
2. Välj **Active Directory** från hello till vänster.
   
     ![Välj Active Directory][1]
3. Välj hello **standard Active Directory** i Azure-portalen. 
   
     ![Välj katalog][2]
   
   > [!IMPORTANT]
   > Den här metoden fungerar bara när du arbetar i hello standard Active Directory för ditt konto och fungerar inte om du vill göra detta i en Active Directory som du har skapat i ditt konto. 
   > 
   > 
4. tooview hello program i din katalog klickar du på **program**.
   
     ![Visa program][3]
5. Klicka på **lägga till**. 
   
     ![Lägg till program][4]
6. Klicka på **Lägg till ett program som min organisation utvecklar**
   
     ![nytt program][5]
7. Fyll i namnet på programmet hello och välj hello typ av program som **WEB APPLICATION och/eller webb-API** och klicka på knappen för nästa hello.
   
     ![namn på program][6]
8. Du kan ange eventuella dummy URL: er för **SIGN-ON-URL** och **APP-ID URI**. Används inte för vårt scenario och hello URL: er själva valideras inte.  
   
     ![Egenskaper för program][7]
9. Hello slutet av det här har du en AAD-app med hello namn du angav tidigare som hello nedan. Det här är din **AD\_APP\_namn** och anteckna den.  
   
     ![Appnamn][8]
10. Klicka på hello appens namn och klicka på **konfigurera**.
    
      ![Konfigurera appen][9]
11. Anteckna hello klient-ID som ska användas som **klienten\_ID** för din API-anrop. 
    
     ![Konfigurera appen][10]
12. Bläddra nedåt toohello **nycklar** avsnittet och lägga till en nyckel med helst 2 år (upphör att gälla) varaktighet och på **spara**. 
    
     ![Konfigurera appen][11]
13. Kopiera omedelbart hello-värde som visas för hello nyckeln som det visas bara nu och lagras inte så visas inte någonsin igen. Om du förlorar den att kommer du ha toogenerate en ny nyckel. Detta blir hello **CLIENT_SECRET** för din API-anrop. 
    
     ![Konfigurera appen][12]
    
    > [!IMPORTANT]
    > Den här nyckeln kommer att upphöra hello slutet av hello varaktighet som du angav så att kontrollera toorenew som du när hello tid annars kommer API-autentisering inte fungerar längre. Du kan också ta bort och återskapa den här nyckeln om du tror att den har komprometterats.
    > 
    > 
14. Klicka på **visa SLUTPUNKTER** knappen nu som öppnas hello **App slutpunkter** dialogrutan. 
    
    ![][13]
15. Kopiera från hello App slutpunkter i dialogrutan hello **OAUTH 2.0-TOKEN för SLUTPUNKT**. 
    
    ![][14]
16. Den här slutpunkten blir i hello följande formulär där hello GUID i hello URL är din **TENANT_ID** så att anteckna den: 
    
        https://login.microsoftonline.com/<GUID>/oauth2/token
17. Nu ska vi fortsätta tooconfigure hello behörigheter på den här appen. För detta måste tooopen in hello [Azure-portalen](https://portal.azure.com). 
18. Klicka på **resursgrupper** och hitta hello **Mobile Engagement** resursgruppen.  
    
    ![][15]
19. Klicka på hello **Mobile Engagement** resurs gruppen och navigera toohello **inställningar** här bladet. 
    
    ![][16]
20. Klicka på **användare** i hello inställningsbladet och klicka sedan på **Lägg till** tooadd en användare. 
    
    ![][17]
21. Klicka på **Välj en roll**
    
    ![][18]
22. Klicka på **ägare**
    
    ![][19]
23. Sök efter hello namnet på ditt program **AD\_APP\_namn** i hello sökrutan. Du kommer inte se det som standard här. När du har hittat markerar du den och klicka på **Välj** längst hello hello-bladet. 
    
    ![][20]
24. På hello **Lägg till åtkomst** bladet den visas som **1 användare, 0 grupper**. Klicka på **OK** på bladet tooconfirm hello ändringen. 
    
    ![][21]

Nu har du slutfört hello krävs AAD-konfiguration och du kan alla set toocall hello API: er. 

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication-manual/active-directory.png
[2]: ./media/mobile-engagement-api-authentication-manual/active-directory-details.png
[3]: ./media/mobile-engagement-api-authentication-manual/view-applications.png
[4]: ./media/mobile-engagement-api-authentication-manual/add-icon.png
[5]: ./media/mobile-engagement-api-authentication-manual/what-do-you-want-to-do.png
[6]: ./media/mobile-engagement-api-authentication-manual/tell-us-about-your-application.png
[7]: ./media/mobile-engagement-api-authentication-manual/app-properties.png
[8]: ./media/mobile-engagement-api-authentication-manual/aad-app.png
[9]: ./media/mobile-engagement-api-authentication-manual/configure-menu.png
[10]: ./media/mobile-engagement-api-authentication-manual/client-id.png
[11]: ./media/mobile-engagement-api-authentication-manual/client_secret.png
[12]: ./media/mobile-engagement-api-authentication-manual/keys.png
[13]: ./media/mobile-engagement-api-authentication-manual/view-endpoints.png
[14]: ./media/mobile-engagement-api-authentication-manual/app-endpoints.png
[15]: ./media/mobile-engagement-api-authentication-manual/resource-groups.png
[16]: ./media/mobile-engagement-api-authentication-manual/resource-groups-settings.png
[17]: ./media/mobile-engagement-api-authentication-manual/add-users.png
[18]: ./media/mobile-engagement-api-authentication-manual/add-role.png
[19]: ./media/mobile-engagement-api-authentication-manual/select-role.png
[20]: ./media/mobile-engagement-api-authentication-manual/add-user-select.png
[21]: ./media/mobile-engagement-api-authentication-manual/add-access-final.png



