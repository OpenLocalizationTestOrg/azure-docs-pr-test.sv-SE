---
title: aaaCreate en WordPress-webbapp i Azure App Service | Microsoft Docs
description: "Lär dig hur toocreate en ny Azure web app för en WordPress-blogg hello Azure-portalen."
services: app-service\web
documentationcenter: php
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 193ae094-0d7c-4749-a09b-ff4b1240149e
ms.service: app-service-web
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 3a95fcb6732c15a8200921ce474b6dde2298dec3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-wordpress-web-app-in-azure-app-service"></a>Skapa en WordPress-webbapp i Azure App Service
[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Den här kursen visar hur toodeploy en WordPress-blogg plats från hello Azure Marketplace.

När du är klar med hello kursen har du en egen WordPress-bloggwebbplats som körs i molnet hello.

![WordPress-webbplats](./media/web-sites-php-web-site-gallery/wpdashboard.png)

Du får lära dig:

* Hur toofind en programmall i hello Azure Marketplace.
* Hur toocreate en webbapp i Azure App Service som baseras på hello mallen.
* Hur tooconfigure Azure App Service-inställningar för nya hello web app och databasen.

hello Azure Marketplace blir tillgängligt på en mängd olika populära webbappar som har utvecklats av Microsoft och tredjepartsföretag programvara med öppen källkod. hello webbprogram som bygger på en mängd olika populära ramverk, [PHP](/develop/nodejs/) i det här WordPress-exemplet [.NET](/develop/net/), [Node.js](/develop/nodejs/), [Java](/develop/java/), och [Python](/develop/python/), tooname ett fåtal. toocreate en webbapp från Azure Marketplace hello endast programvara du behöver är hello webbläsare som du använder för hello hello [Azure Portal](https://portal.azure.com/). 

hello WordPress-webbplats som du distribuerar i den här kursen används MySQL som hello-databasen. Om du vill tooinstead används SQL-databas för hello databasen finns [Project Nami](http://projectnami.org/). **Project Nami** är också tillgängliga via hello Marketplace.

> [!NOTE]
> toocomplete den här självstudiekursen kommer du behöver ett Microsoft Azure-konto. Om du inte har ett konto kan du [aktivera Visual Studio-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) eller [registrera dig för en kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
> 
> Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/). Där kan du direkt skapa en tillfällig startwebbapp i App Service. Inget kreditkort behövs och du gör inga åtaganden.
> 
> 

## <a name="select-wordpress-and-configure-for-azure-app-service"></a>Välj WordPress och konfigurera för Azure App Service
1. Logga in toohello [Azure Portal](https://portal.azure.com/).
2. Klicka på **Ny**.
   
    ![Skapa Ny][5]
3. Sök efter **WordPress** och klicka sedan på **WordPress**. Om du vill toouse SQL-databas i stället för MySQL söker du efter **Project Nami**.
   
    ![WordPress från lista][7]
4. När du har läst hello beskrivning av hello WordPress-appen klickar du på **skapa**.
   
    ![Skapa](./media/web-sites-php-web-site-gallery/create.png)
5. Ange ett namn för hello webbprogrammet i hello **webbprogrammet** rutan.
   
    Det här namnet måste vara unikt i hello azurewebsites.NET eftersom hello URL för hello webbprogrammet blir {name}. azurewebsites.net. Om hello namnet inte är unikt visas ett rött utropstecken i textrutan för hello.
6. Om du har mer än en prenumeration, Välj hello önskat toouse. 
7. Välj en **Resursgrupp** eller skapa en ny.
   
    Mer information om resursgrupper finns i [Översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).
8. Välj en **App Service-plan/plats** eller skapa en ny.
   
    Mer information om App Service-planer finns i [Översikt över Azure App Service-planer](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)    
9. Klicka på **databasen**, och klicka sedan på hello **ny MySQL-databas** bladet ange hello krävs värden för att konfigurera MySQL-databasen.
   
    a. Ange ett nytt namn eller lämna standardnamnet hello.
   
    b. Lämna hello **databastyp** ställa in också**delade**.
   
    c. Välj hello samma plats som hello som du har valt för hello webbprogrammet.
   
    d. Välj en prisnivå. För kursen räcker det bra med Mercury (kostnadsfri med det lägsta antalet tillåtna anslutningar och lägst mängd diskutrymme).
10. I hello **ny MySQL-databas** bladet, klickar du på **OK**. 
11. I hello **WordPress** bladet godkänner hello juridiska villkoren och klicka sedan på **skapa**. 
    
     ![Konfigurera webbappen](./media/web-sites-php-web-site-gallery/configure.png)
    
     Azure App Service skapar hello webbapp vanligen på mindre än en minut. Du kan titta på hello förloppet genom att klicka på hello klockikonen överst hello i hello Portalsida.
    
     ![Förloppsindikator](./media/web-sites-php-web-site-gallery/progress.png)

## <a name="launch-and-manage-your-wordpress-web-app"></a>Starta och hantera WordPress-webbappen
1. När hello webbappen har skapats, gå hello Azure Portal toohello resursgrupp som du skapat hello program och du kan se hello webbapp och hello-databasen.
   
    Hej extraresursen med lampikonen hello är [Programinsikter](/services/application-insights/), som tillhandahåller övervakningstjänster för webbappen.
2. I hello **resursgruppen** bladet, klickar du på webbappsraden hello.
   
    ![Konfigurera webbappen](./media/web-sites-php-web-site-gallery/resourcegroup.png)
3. Hello Web app-bladet, klickar du på **Bläddra**.
   
    ![webbadress][browse]
4. I hello WordPress **Välkommen** sidan Ange hello konfigurationsinformation som krävs av WordPress och klickar sedan på **installera WordPress**.
   
    ![Konfigurera WordPress](./media/web-sites-php-web-site-gallery/wpconfigure.png)
5. Logga in med hello autentiseringsuppgifterna som du skapade på hello **Välkommen** sidan.  
6. Webbplatsens instrumentpanelssida öppnas.    
   
    ![WordPress-webbplats](./media/web-sites-php-web-site-gallery/wpdashboard.png)

## <a name="next-steps"></a>Nästa steg
Du har sett hur toocreate och distribuera en PHP-webbapp från galleriet hello. Mer information om hur du använder PHP i Azure finns hello [PHP Developer Center](/develop/php/).

Mer information om hur toowork med App Service Web Apps Se hello länkar på hello vänster sida av hello sidan (breda webbläsarfönster) eller hello överst i hello sidan (smala webbläsarfönster). 

## <a name="whats-changed"></a>Nyheter
* En guide toohello ändring från webbplatser tooApp tjänsten finns [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714).

[5]: ./media/web-sites-php-web-site-gallery/startmarketplace.png
[7]: ./media/web-sites-php-web-site-gallery/search-web-app.png
[browse]: ./media/web-sites-php-web-site-gallery/browse-web.png
