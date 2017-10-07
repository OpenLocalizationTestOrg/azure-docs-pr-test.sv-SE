---
title: "aaaDeploy en Umbraco webbapp i hello Azure-portalen på fem minuter | Microsoft Docs"
description: "Lär dig hur lätt det är toorun webbappar i App Service genom att distribuera en exempelapp för ASP.NET. Resultatet visas omedelbart."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: b1e6bd58-48d1-4007-9d6c-53fd6db061e3
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2017
ms.author: cephalin
ms.openlocfilehash: 6da45f99b3043a3684e3d99c14e6443d597b5212
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-umbraco-web-app-in-hello-azure-portal-in-five-minutes"></a>Distribuera en Umbraco webbapp i hello Azure-portalen på fem minuter

Den här kursen hjälper dig att distribuera n [Umbraco](https://our.umbraco.org/) webbapp för[Azure App Service](../app-service/app-service-value-prop-what-is.md) i minuter.

![Umbraco-app](./media/app-service-web-get-started-dotnet-portal/defaultpage.png)

## <a name="prerequisites"></a>Krav
Du behöver ett Microsoft Azure-konto. Om du inte har ett konto kan du [registrera dig för en kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) eller [aktivera Visual Studio-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

> [!NOTE]
> Du kan [Prova App Service](https://azure.microsoft.com/try/app-service/) utan ett Azure-konto. Skapa en startapp och spela upp med den för in tooan timme – inget kreditkort behövs, inga åtaganden.
> 
> 

## <a name="deploy-hello-aspnet-app"></a>Distribuera hello ASP.NET-app
1. Logga in toohello [Azure-portalen](https://portal.azure.com).

2. Öppna [https://portal.azure.com/#create/umbracoorg.UmbracoCMS](https://portal.azure.com/#create/umbracoorg.UmbracoCMS).

    Den här länken är en genväg tooimmediately konfigurerar en ny Umbraco app i hello Azure-portalen.

3. I **Appnamn** anger du webbappens namn. Visas en grön bock i hello rutan om hello namn är unikt i hello `azurewebsites.net` domän.
   
5. I **resursgruppen**, klickar du på **Skapa nytt** toocreate en ny [resursgruppen](../azure-resource-manager/resource-group-overview.md), ge den ett namn.

7. Klicka på **App Service plan/plats** > **Skapa ny**. Konfigurera hello [programtjänstplanen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) som visas:

    - I **programtjänstplanen**, hello önskade namn.
    - I **plats**, Välj en plats toohost planen.
    - Klicka på **Prisnivå**, välj **F1 Kostnadsfri** eller någon annan nivå som passar dig och klicka på **Välj**.
    - Klicka på **OK**.

    Konfigurationen Umbraco CMS bör nu se ut som följande skärmbild hello:

    ![Konfigurationen pågår – första Umbraco i Azure App Service](./media/app-service-web-get-started-dotnet-portal/configure-in-progress.png)

12. Klicka på **SQL Database** > **Skapa en ny databas**. Konfigurera hello SQL-databas som visas:

    - Ange ett namn i fältet **Namn**, till exempel **minDB**.
    - Klicka på **Prisnivå**, välj **1 Kostnadsfri** eller någon annan nivå som passar dig och klicka på **Välj**.
    - Klicka på **Målserver** > **Skapa en ny server**. Konfigurera hello databasserver som visas:

        - Ange ett servernamn i fältet **Servernamn**. Visas en grön bock i hello rutan om hello namn är unikt i hello `.database.windows.net` domän.
        - I **inloggning för serveradministratör**, typen hello önskad administratörsbehörighet användarnamn.
        - I **lösenord** och **Bekräfta lösenord**, ange hello önskade lösenord.
        - Välj hello på plats, samma plats som du använder för hello webbprogrammet.
        - Kontrollera att **Tillåt azure-tjänster tooaccess server** är markerad.
        - Klicka på **Välj**.
    
    - Klicka på **Välj**.

13. Klicka på **Web app-inställningar**anger hello databasanvändarnamn och lösenord och klicka på **OK**.

    Konfigurationen Umbraco CMS bör nu se ut som följande skärmbild hello:

    ![Konfigurationen är slutförd – första Umbraco i Azure App Service](./media/app-service-web-get-started-dotnet-portal/configure-complete.png)

14. Klicka på **Skapa**.
    
    Azure skapar nu Umbraco-appen utifrån din konfiguration. Du bör se meddelandet **Distributionen har påbörjats**.

    ![Distributionen har utförts – första Umbraco i Azure App Service](./media/app-service-web-get-started-dotnet-portal/deployment-started.png)
   
## <a name="launch-and-manage-your-umrbaco-web-app"></a>Starta och hantera Umbraco-webbappen

När appen har distribuerats i Azure visas ett nytt meddelande.

![Distributionen har utförts – första Umbraco i Azure App Service](./media/app-service-web-get-started-dotnet-portal/deployment-succeeded.png)

1. Klicka på hello-meddelande. Om du har missat du alltid har åtkomst till den genom att klicka på hello notification bell (![meddelande nedan - första Umbraco i Azure App Service](./media/app-service-web-get-started-dotnet-portal/notification.png)).

    Du bör nu se webbappens [administrationsblad](../azure-resource-manager/resource-group-portal.md#manage-resources) (*blad*: en portalsida som öppnas horisontellt).

3. Klicka i hello överkant hello översiktssidan **Bläddra**.
   
    ![Bläddra – första Umbraco i Azure App Service](./media/app-service-web-get-started-dotnet-portal/browse.png)

    Nu visas hello Umbraco **Välkommen** sidan. Konfigurera hello Umbraco installationen och starta prova!

    ![Umbraco-konfiguration – första Umbraco i Azure App Service](./media/app-service-web-get-started-dotnet-portal/umbraco-config.png)
    
## <a name="next-steps"></a>Nästa steg
* [Distribuera ett ASP.NET web app tooAzure App Service med Visual Studio](app-service-web-get-started-dotnet.md) – Lär dig hur toocreate en ny Azure-webbapp från Visual Studio, med någon av hello med programmallar.
* [Distribuera din kod tooAzure Apptjänst](web-sites-deploy.md)– Lär dig hur toodeploy från FTP- eller från källan styra databaser.
* [Lägg till funktioner tooyour första webbapp](app-service-web-get-started-2.md) -ta din Azure-app toohello nästa nivå. Autentisera användarna. Skala den på begäran. Konfigurera prestandavarningar. Allt med några få klickningar.
