---
title: "Django och SQL Database på Azure med Python Tools 2.2 för Visual Studio"
description: "Lär dig hur du använder Python Tools för Visual Studio för att skapa en Django-webbapp som lagrar data i en SQL-databasinstans och distribuera den till Azure App Service Web Apps."
services: app-service\web
tags: python
documentationcenter: python
author: huguesv
manager: erikre
editor: 
ms.assetid: 3a677e64-b5a9-4d43-b9c0-66246368b483
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 07/07/2016
ms.author: huvalo
ms.openlocfilehash: 65b59dee2b7bddca77d31c692dab713c68d67e24
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="django-and-sql-database-on-azure-with-python-tools-22-for-visual-studio"></a>Django och SQL Database på Azure med Python Tools 2.2 för Visual Studio
I den här kursen använder vi [Python Tools för Visual Studio] du skapar ett enkelt avsökningar webbapp med någon av exempelmallarna i PTVS. Den här kursen finns också tillgänglig som en [video](https://www.youtube.com/watch?v=ZwcoGcIeHF4).

Vi lära dig hur du använder en SQL-databas i Azure, hur du konfigurerar webbprogram att använda en SQL-databas och hur du publicerar webbappen till [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).

Finns det [Python Developer Center] fler artiklar om utvecklingen av Azure App Service Web Apps med PTVS med hjälp av Bottle, Flask och Django webbramverken med Azure Table Storage, MySQL och SQL-databas. Den här artikeln fokuserar på App Service, men stegen är ungefär desamma för att utveckla [Azure Cloud Services].

## <a name="prerequisites"></a>Krav
* Visual Studio 2015
* [Python 2.7 32-bitars]
* [Python Tools 2.2 för Visual Studio]
* [Python Tools 2.2 för Visual Studio Samples VSIX]
* [Azure SDK Tools för VS 2015]
* Django 1.9 eller senare

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> Om du vill komma igång med Azure Apptjänst innan du registrerar dig för ett Azure-konto kan du gå till [Prova Apptjänst](https://azure.microsoft.com/try/app-service/). Där kan du direkt skapa en tillfällig startwebbapp i Apptjänst. Inget kreditkort krävs, och du gör inga åtaganden.
>
>

## <a name="create-the-project"></a>Skapa projektet
I det här avsnittet skapar vi ett Visual Studio-projekt utifrån en exempelmall. Vi skapa en virtuell miljö och installera nödvändiga paket. Vi ska skapa en lokal databas med hjälp av sqlite. Vi kommer kör webbappen lokalt.

1. I Visual Studio väljer du **Arkiv**, **Nytt projekt**.
2. Du hittar projektmallarna från [Python Tools 2.2 för Visual Studio Samples VSIX] under **Python**, **Exempel**. Välj **Polls Django Web Project** (Omröstningar, Django-webbprojekt) och skapa projektet genom att klicka på OK.

     ![Dialogrutan Nytt projekt](./media/web-sites-python-ptvs-django-sql/PollsDjangoNewProject.png)
3. Du uppmanas att installera externa paket. Välj **Install into a virtual environment** (Installera i en virtuell miljö).

     ![Dialogrutan Externa paket](./media/web-sites-python-ptvs-django-sql/PollsDjangoExternalPackages.png)
4. Välj **Python 2.7** som bastolk.

     ![Dialogrutan Add Virtual Environment (Lägg till virtuell miljö)](./media/web-sites-python-ptvs-django-sql/PollsCommonAddVirtualEnv.png)
5. Högerklickar på projektnoden i **Solution Explorer** och välj först **Python** och sedan **Django Migrate**.  Välj därefter **Django – skapa superanvändare**.
6. Öppnar Django Management Console och en sqlite-databas skapas i projektmappen. Skapa en användare genom att följa anvisningarna.
7. Bekräfta att programmet fungerar genom att trycka på <kbd>F5</kbd>.
8. Klicka på **Log in** (Logga in) i navigeringsfältet längst upp.

     ![Webbläsare](./media/web-sites-python-ptvs-django-sql/PollsDjangoCommonBrowserLocalMenu.png)
9. Ange autentiseringsuppgifterna för användaren du skapade när du synkroniserade databasen.

     ![Webbläsare](./media/web-sites-python-ptvs-django-sql/PollsDjangoCommonBrowserLocalLogin.png)
10. Klicka på **Create Sample Polls** (Skapa exempelomröstningar).

      ![Webbläsare](./media/web-sites-python-ptvs-django-sql/PollsDjangoCommonBrowserNoPolls.png)
11. Klicka på en omröstning och rösta.

      ![Webbläsare](./media/web-sites-python-ptvs-django-sql/PollsDjangoSqliteBrowser.png)

## <a name="create-a-sql-database"></a>Skapa en SQL Database
Vi ska skapa en Azure SQL database för databasen.

Du kan skapa en databas genom att följa dessa steg.

1. Logga in på den [Azure-portalen].
2. Längst ned i navigeringsfönstret klickar du på **ny**. , klickar du på **Data + lagring** > **SQL-databas**.
3. Konfigurera den nya SQL-databasen genom att skapa en ny resursgrupp och välja en lämplig plats för den.
4. När SQL-databasen har skapats klickar du på **öppna i Visual Studio** i databasbladet.
5. Klicka på **konfigurera brandväggen**.
6. I den **brandväggsinställningar** bladet Lägg till en brandväggsregel med **första IP-** och **sista IP** inställd på utvecklingsdatorn offentliga IP-adress. Klicka på **Spara**.

   Detta tillåter anslutningar till databasservern från utvecklingsdatorn.
7. Klicka på tillbaka i databasbladet **egenskaper**, klicka på **visa databasanslutningssträngar**.
8. Använd kopieringsknappen för att ange värdet för **ADO.NET** i Urklipp.

## <a name="configure-the-project"></a>Konfigurera projektet
I det här avsnittet konfigurerar vi webbappen att använda SQL-databas som vi just skapade. Vi måste också installera ytterligare Python-paket som krävs för att använda SQL-databaser med Django. Vi kommer kör webbappen lokalt.

1. I Visual Studio öppnar du **settings.py** i mappen *Projektnamn*. Klistra in anslutningssträngen i redigeringsprogrammet. Anslutningssträngen har det här formatet:

       Server=<ServerName>,<ServerPort>;Database=<DatabaseName>;User ID=<UserName>;Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;

Redigera definition av `DATABASES` värdena ovan ska användas.

        DATABASES = {
            'default': {
                'ENGINE': 'sql_server.pyodbc',
                'NAME': '<DatabaseName>',
                'USER': '<UserName>',
                'PASSWORD': '{your_password_here}',
                'HOST': '<ServerName>',
                'PORT': '<ServerPort>',
                'OPTIONS': {
                    'driver': 'SQL Server Native Client 11.0',
                    'MARS_Connection': 'True',
                }
            }
        }

1. I Solution Explorer under **Python Environments** (Python-miljöer), högerklickar du på den virtuella miljön och väljer **Install Python Package** (Installera Python-paket).
2. Installera `pyodbc`-paketet med **pip**.

     ![Python dialogrutan Installera paket](./media/web-sites-python-ptvs-django-sql/PollsDjangoSqlInstallPackagePyodbc.png)
3. Installera `django-pyodbc-azure`-paketet med **pip**.

     ![Python dialogrutan Installera paket](./media/web-sites-python-ptvs-django-sql/PollsDjangoSqlInstallPackageDjangoPyodbcAzure.png)
4. Högerklickar på projektnoden i **Solution Explorer** och välj först **Python** och sedan **Django Migrate**.  Välj därefter **Django – skapa superanvändare**.

   Detta skapar tabeller för SQL-databas som vi skapade i föregående avsnitt. Följ anvisningarna för att skapa en användare som inte behöver matcha användaren i sqlite-databas som skapats i det första avsnittet.
5. Kör programmet med `F5`. Omröstningar som skapas med **skapa Exempelomröstningar** och de data som skickats vid röstning serialiseras i SQL-databasen.

## <a name="publish-the-web-app-to-azure-app-service"></a>Publicera webbappen i Azure App Service
Azure .NET SDK innehåller ett enkelt sätt att distribuera din web-webbapp till Azure App Service Web Apps.

1. I **Solution Explorer** högerklickar du på projektnoden och väljer **Publicera**.

     ![Dialogrutan Publicera webbplats](./media/web-sites-python-ptvs-django-sql/PollsCommonPublishWebSiteDialog.png)
2. Klicka på **Microsoft Azure Web Apps**.
3. Skapa en ny webbapp genom att klicka på **Ny**.
4. Fyll i följande fält och på **skapa**.

   * **Webbappens namn**
   * **App Service-plan**
   * **Resursgrupp**
   * **Region**
   * Lämna **Databasserver** inställd på **Ingen databas**
5. Godkänn alla standardvärden och klicka på **Publicera**.
6. Den publicerade webbappen öppnas automatiskt i webbläsaren. Du bör se att webbappen fungerar som förväntat, med hjälp av den **SQL** databasen på Azure.

   Grattis!

     ![Webbläsare](./media/web-sites-python-ptvs-django-sql/PollsDjangoAzureBrowser.png)

## <a name="next-steps"></a>Nästa steg
Följa dessa länkar om du vill veta mer om Python Tools för Visual Studio, Django och SQL-databas.

* [Dokumentation för Python Tools för Visual Studio]
  * [Webbprojekt]
  * [Molntjänstprojekt]
  * [Fjärrfelsökning i Microsoft Azure]
* [Django-dokumentation]
* [SQL Database]

## <a name="whats-changed"></a>Nyheter
* En guide till övergången från Webbplatser till App Service finns i: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)

<!--Link references-->
[Python Developer Center]: /develop/python/
[Azure Cloud Services]: ../cloud-services/cloud-services-python-ptvs.md

<!--External Link references-->
[Azure-portalen]: https://portal.azure.com
[Python Tools för Visual Studio]: http://aka.ms/ptvs
[Python Tools 2.2 för Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025
[Python Tools 2.2 för Visual Studio Samples VSIX]: http://go.microsoft.com/fwlink/?LinkID=624025
[Azure SDK Tools för VS 2015]: http://go.microsoft.com/fwlink/?LinkId=518003
[Python 2.7 32-bitars]: http://go.microsoft.com/fwlink/?LinkId=517190
[Dokumentation för Python Tools för Visual Studio]: http://aka.ms/ptvsdocs
[Fjärrfelsökning i Microsoft Azure]: http://go.microsoft.com/fwlink/?LinkId=624026
[Webbprojekt]: http://go.microsoft.com/fwlink/?LinkId=624027
[Molntjänstprojekt]: http://go.microsoft.com/fwlink/?LinkId=624028
[Django-dokumentation]: https://www.djangoproject.com/
[SQL Database]: /documentation/services/sql-database/
