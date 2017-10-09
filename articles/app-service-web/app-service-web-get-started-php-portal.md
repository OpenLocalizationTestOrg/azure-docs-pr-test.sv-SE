---
title: "aaaDeploy WordPress-appen i hello Azure-portalen på fem minuter | Microsoft Docs"
description: "Lär dig hur lätt det är toorun webbappar i App Service genom att distribuera en WordPress-appen. Resultatet visas omedelbart."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 6feac128-c728-4491-8b79-962da9a40788
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/13/2017
ms.author: cephalin
ms.openlocfilehash: 4cd26bbacf89657997847ded1284e472288ddebe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-wordpress-app-in-hello-azure-portal-in-five-minutes"></a>Distribuera en WordPress-appen i hello Azure-portalen på fem minuter

De här självstudierna visar hur toodeploy din första [WordPress](https://wordpress.org/) webbapp för[Azure App Service](../app-service/app-service-value-prop-what-is.md) i minuter.

![WordPress-webbplats](./media/app-service-web-get-started-php-portal/wpdashboard.png)

## <a name="prerequisites"></a>Krav
Du behöver ett Microsoft Azure-konto. Om du inte har ett konto kan du [registrera dig för en kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) eller [aktivera Visual Studio-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

> [!NOTE]
> Du kan [Prova App Service](https://azure.microsoft.com/try/app-service/) utan ett Azure-konto. Skapa en startapp och spela upp med den för in tooan timme – inget kreditkort behövs, inga åtaganden.
> 
> 

## <a name="deploy-hello-wordpress-app"></a>Distribuera hello WordPress-appen
1. Logga in toohello [Azure-portalen](https://portal.azure.com).

2. Öppna [https://portal.azure.com/#create/WordPress.WordPress](https://portal.azure.com/#create/WordPress.WordPress).

    Den här länken är en genväg tooimmediately konfigurerar en ny WordPress-app i hello Azure-portalen.

3. I **Appnamn** anger du webbappens namn. Visas en grön bock i hello rutan om hello namn är unikt i hello `azurewebsites.net` domän.
   
5. I **resursgruppen**, klickar du på **Skapa nytt** toocreate en ny [resursgruppen](../azure-resource-manager/resource-group-overview.md), ge den ett namn.

6. I **Databasprovider** väljer du **CleaDB**.

7. Klicka på **App Service plan/plats** > **Skapa ny**. Konfigurera hello [programtjänstplanen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) som visas:

    - I **programtjänstplanen**, hello önskade namn.
    - I **plats**, Välj en plats toohost planen.
    - Klicka på **Prisnivå**, välj **F1 Kostnadsfri** eller någon annan nivå som passar dig och klicka på **Välj**.
    - Klicka på **OK**.

8. Klicka på **Databas** > **Skapa ny**. Konfigurera hello SQL-databas som visas:

    - I **Databasnamn** skriver du ett databasnamn. 
    - I **plats**, Välj hello samma plats som hello App Service-plan.
    - Klicka på **Prisnivå**, välj **Mercury** eller någon annan nivå som passar dig och klicka på **Välj**.
    - Välj **Juridiska villkor** och klicka på **Köp**.
    - Klicka på **OK**.

9. Klicka på **Skapa**.

    Azure skapar nu WordPress-appen utifrån din konfiguration. Du bör se meddelandet **Distributionen har påbörjats**.

    ![Distributionen har startat – första WordPress i Azure App Service](./media/app-service-web-get-started-php-portal/deployment-started.png)
   
## <a name="launch-and-manage-your-wordpress-web-app"></a>Starta och hantera WordPress-webbappen

När appen har distribuerats i Azure visas ett nytt meddelande.

![Distributionen har utförts – första WordPress i Azure App Service](./media/app-service-web-get-started-php-portal/deployment-succeeded.png)

1. Klicka på hello-meddelande. Om du har missat du alltid har åtkomst till den genom att klicka på hello notification bell (![meddelande nedan - första WordPress i Azure App Service](./media/app-service-web-get-started-dotnet-portal/notification.png)).

    Du bör nu se webbappens [administrationsblad](../azure-resource-manager/resource-group-portal.md#manage-resources) (*blad*: en portalsida som öppnas horisontellt).

3. Klicka i hello överkant hello översiktssidan **Bläddra**.
   
    ![Bläddra – första WordPress i Azure App Service](./media/app-service-web-get-started-php-portal/browse.png)

    Nu visas hello WordPress **Välkommen** sidan. Konfigurera hello WordPress installationen och starta prova!

    ![WordPress-konfiguration – första WordPress i Azure App Service](./media/app-service-web-get-started-php-portal/wordpress-config.png)
    
## <a name="next-steps"></a>Nästa steg
* [Skapa, konfigurera och distribuera en Laravel web app tooAzure](app-service-web-php-get-started.md) -Läs hello grundläggande kunskaper du behöver toorun ett PHP-webbprogram i Azure, exempelvis:

    * Skapa och konfigurera appar i Azure från PowerShell/Bash.
    * Ange PHP-version.
    * Använda en startfil som inte är i hello rotkatalogen för programmet.
    * Aktivera Composer-automatisering.
    * Få åtkomst till miljöspecifika variabler.
    * Felsöka vanliga fel.

* [Distribuera din kod tooAzure Apptjänst](web-sites-deploy.md)– Lär dig hur toodeploy från FTP- eller från källan styra databaser.
* [Lägg till funktioner tooyour första webbapp](app-service-web-get-started-2.md) -ta din Azure-app toohello nästa nivå. Autentisera användarna. Skala den på begäran. Konfigurera prestandavarningar. Allt med några få klickningar.
