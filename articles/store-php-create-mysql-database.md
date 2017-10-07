---
title: aaaCreate och ansluta tooa MySQL-databas i Azure
description: "Lär dig hur toouse hello Azure portal toocreate en MySQL-databas och anslut sedan tooit från en PHP-webbapp i Azure."
documentationcenter: php
services: app-service\web
author: cephalin
manager: erikre
editor: 
tags: mysql
ms.assetid: 55465a9a-7e65-4fd9-8a65-dd83ee41f3e5
ms.service: multiple
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm;cephalin
ms.openlocfilehash: 3abc02f8887432625666afd847e9dc0c0a4db2e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-connect-tooa-mysql-database-in-azure"></a>Skapa och ansluta tooa MySQL-databas i Azure
Den här kursen visar hur toocreate en MySQL-databas i hello [Azure-portalen](https://portal.azure.com) (providern är [ClearDB](http://www.cleardb.com/)) och hur tooconnect tooit från en PHP web app som körs i [Azure App Service](app-service/app-service-value-prop-what-is.md).

> [!NOTE]
> Du kan också skapa en MySQL-databas som en del av en [Marketplace-appmallen](app-service-web/app-service-web-create-web-app-from-marketplace.md).
>
>

## <a name="create-a-mysql-database-in-azure-portal"></a>Skapa en MySQL-databas på Azure-portalen
toocreate en MySQL-databas i hello Azure-portalen hello följande:

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Hello vänstra menyn klickar du på **ny** > **Data + lagring** > **MySQL-databas**.

    ![Skapa en MySQL-databas i Azure - start](./media/store-php-create-mysql-database/create-db-1-start.png)
3. I hello ny MySQL-databas [bladet](azure-portal-overview.md), konfigurera den nya MySQL-databasen på följande sätt (*bladet*: en Portalsida som öppnar vågrätt):

   * **Databasnamn**: Ange en unik namn
   * **Prenumerationen**: Välj hello prenumeration toouse
   * **Typ av databas**: Välj **delade** för låg kostnad eller kostnadsfria nivåer eller **dedikerad** tooget särskilda resurser.
   * **Resursgruppen**: Lägg till hello MySQL database tooan befintlig [resursgruppen](azure-resource-manager/resource-group-overview.md) eller placera den i en ny. Resurser i samma grupp kan enkelt hanteras tillsammans hello.
   * **Plats**: Välj en plats Stäng tooyou. När du lägger till tooan befintliga resursgruppen är låst toohello resursgruppens plats.
   * **Prisnivån**: klickar du på **prisnivån**, Välj en prisnivå alternativ (**Merkurius** nivå är ledigt), och klicka sedan på **Välj**.
   * **Juridiska villkor**: Klicka på **juridiska villkor**, granska hello inköpsinformation och på **köpa**.
   * **PIN-kod toodashboard**: Välj om du vill tooaccess den direkt från hello instrumentpanelen. Detta är särskilt användbart om du inte är bekant med portalen navigering ännu.

     följande skärmbild hello är bara ett exempel på hur du kan konfigurera MySQL-databas.  
     ![Skapa en MySQL-databas i Azure - konfigurera](./media/store-php-create-mysql-database/create-db-2-configure.png)
4. När du är klar konfigurerar, klickar du på **skapa**.

    ![Skapa en MySQL-databas i Azure - skapa](./media/store-php-create-mysql-database/create-db-3-create.png)

    Du ser ett popup-meddelande postmeddelande med information om att distributionen har startats.

    ![Skapa en MySQL-databas i Azure - pågår](./media/store-php-create-mysql-database/create-db-4-started-status.png)

    Du får en annan popup när distributionen har slutförts. hello portalen öppnas automatiskt även ditt MySQL-databas-blad.

<a name="connect"></a>

## <a name="connect-tooyour-mysql-database"></a>Ansluta tooyour MySQL-databas
toosee hello anslutningsinformationen för den nya MySQL-databasen, klicka bara på **egenskaper** i din webbapps blad.

![Skapa en MySQL-databas i Azure - bladet för MySQL-databas](./media/store-php-create-mysql-database/create-db-5-finished-db-blade.png)

Du kan nu använda att anslutningsinformationen i ett webbprogram. Ett exempel som visar hur toouse hello anslutningsinformation från en enkel PHP-app är tillgänglig [här](https://github.com/WindowsAzure/azure-sdk-for-php-samples/tree/master/tasklist-mysql).

## <a name="connect-a-laravel-web-app-from-hello-php-get-started-tutorial"></a>Ansluta en webbapp för Laravel (från hello PHP get igång-kursen)
Anta att du bara klar hello kursen [skapa, konfigurera och distribuera en PHP web app tooAzure](app-service-web/app-service-web-php-get-started.md) och har en [Laravel](https://www.laravel.com/) webbprogram som körs i Azure. Du kan enkelt lägga till databasen funktioner tooyour Laravel app. Följ bara hello stegen nedan:

> [!NOTE]
> hello följande steg förutsätter att du har slutfört hello kursen [skapa, konfigurera och distribuera en PHP web app tooAzure](app-service-web/app-service-web-php-get-started.md).
>
>

1. Konfigurera hello Laravel app i din lokala development environment toopoint toohello MySQL-databas. toodo detta, öppna `.env` från appen Laravel rotkatalogen och konfigurera alternativ för hello MySQL-databas.

        DB_CONNECTION=mysql
        DB_HOST=<HOSTNAME_from_properties_blade>
        DB_PORT=<PORT_from_properties_blade>
        DB_DATABASE=<see_note_below>
        DB_USERNAME=<USERNAME_from_properties_blade>
        DB_PASSWORD=<PASSWORD_from_properties_blade>

   > [!NOTE]
   > I hello **egenskaper** bladet hello namnet på din MySQL-databas kanske eller kanske inte hello en visas i hello **DATABASNAMNET** fältet. Det är bättre toocheck hello databasparameter i hello **ANSLUTNINGSSTRÄNGEN** fältet.    
   >
   > ![Skapa en MySQL-databas i Azure - pågår](./media/store-php-create-mysql-database/connect-db-1-database-name.png)
   >
   >
2. hello snabbaste sättet tooverify att du har MySQL åtkomst nu är toouse [scaffold-teknik för autentisering för Laravels standard](https://laravel.com/docs/5.2/authentication#authentication-quickstart).
   Kör följande kommandon från Laravel appens rotkatalog hello i hello kommandoradsverktyget terminalen:

         php artisan migrate
         php artisan make:auth

    hello första kommandot skapar hello tabeller i Azure baserat på fördefinierade migreringar i hello `database/migrations` directory och hello andra kommandot autogenererar hello grundläggande vyerna och vägarna för användarregistrering och autentisering.
3. Kör hello utvecklingsserver nu:

        php artisan serve
4. I hello webbläsare, navigera toohttp://localhost:8000 och registrera en ny användare som visas:

    ![Ansluta tooMySQL databas i Azure - registrera användare](./media/store-php-create-mysql-database/connect-db-2-development-server.png)

    Följ hello UI fråga fullständig hello registrering. När registreringen är klar ska du vara inloggad.

    ![Ansluta tooMySQL databas i Azure - registrera användare](./media/store-php-create-mysql-database/connect-db-3-registered-user.png)

    Du nu utvecklar en app mot hello MySQL-databas i Azure.
5. Nu behöver du bara tooreplicate din `.env` inställningar tooyour Azure-webbapp. Kör följande Azure CLI-kommandona hello:

        azure site appsetting add DB_CONNECTION=mysql
        azure site appsetting add DB_HOST=<HOSTNAME_from_properties_blade>
        azure site appsetting add DB_PORT=<PORT_from_properties_blade>
        azure site appsetting add DB_DATABASE=<Database_param_from_CONNECTION_INFO_from_properties_blade>
        azure site appsetting add DB_USERNAME=<USERNAME_from_properties_blade>
        azure site appsetting add DB_PASSWORD=<PASSWORD_from_properties_blade>

6. Därefter genomför och skicka tooAzure hello lokala ändringar som gjorts tidigare när du kör `php artisan make:auth`.

        git add .
        git commit -m "scaffold auth views and routes"
        git push azure master
7. Bläddra toohello Azure webbapp.

        azure site browse
8. Logga in med hello-autentiseringsuppgifter som du skapade tidigare.

    ![Ansluta tooMySQL databas i Azure - Bläddra tooAzure webbprogram](./media/store-php-create-mysql-database/connect-db-4-browse-azure-webapp.png)

    När du loggar in, bör du se hello eget efter inloggningsskärmen.

    ![Ansluta tooMySQL databas i Azure - inloggad](./media/store-php-create-mysql-database/connect-db-5-logged-in.png)

    Grattis, PHP-webbapp i Azure är nu kommer åt data från din MySQL-databas.

## <a name="next-steps"></a>Nästa steg
Mer information finns i hello [PHP Developer Center](/develop/php/).
