---
title: "aaaDjango och SQL-databas på Azure med Python Tools 2.2 för Visual Studio"
description: "Lär dig hur toouse hello Python Tools för Visual Studio toocreate en Django-webbapp som lagrar data i en SQL-databasinstans och distribuera den tooAzure App Service Web Apps."
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
ms.openlocfilehash: b5b2ef4f3292e7df85007465c5394c8660a7d231
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="django-and-sql-database-on-azure-with-python-tools-22-for-visual-studio"></a>Django och SQL Database på Azure med Python Tools 2.2 för Visual Studio
I den här kursen använder vi [Python Tools för Visual Studio] toocreate ett enkelt avfrågar webbapp med någon av exempelmallarna i hello PTVS. Den här kursen finns också tillgänglig som en [video](https://www.youtube.com/watch?v=ZwcoGcIeHF4).

Vi får lära dig hur toouse en SQL-databas finns i Azure, hur hello tooconfigure web app toouse en SQL-databas och hur toopublish hello webbprogrammet för[Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).

Se hello [Python Developer Center] fler artiklar om utvecklingen av Azure App Service Web Apps med PTVS med hjälp av Bottle, Flask och Django webbramverken med Azure Table Storage, MySQL och SQL-databas. Den här artikeln fokuserar på App Service, hello stegen är liknande när du utvecklar [Azure Cloud Services].

## <a name="prerequisites"></a>Krav
* Visual Studio 2015
* [Python 2.7 32-bitars]
* [Python Tools 2.2 för Visual Studio]
* [Python Tools 2.2 för Visual Studio Samples VSI]
* [Azure SDK Tools för VS 2015]
* Django 1.9 eller senare

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service. Inget kreditkort krävs, och du gör inga åtaganden.
>
>

## <a name="create-hello-project"></a>Skapa hello projekt
I det här avsnittet skapar vi ett Visual Studio-projekt utifrån en exempelmall. Vi skapa en virtuell miljö och installera nödvändiga paket. Vi ska skapa en lokal databas med hjälp av sqlite. Vi kommer kör hello webbappen lokalt.

1. I Visual Studio väljer du **Arkiv**, **Nytt projekt**.
2. Hej projektmallar från hello [Python Tools 2.2 för Visual Studio Samples VSI] är tillgängliga under **Python**, **exempel**. Välj **Polls Django Web Project** och klicka på OK toocreate hello projektet.

     ![Dialogrutan Nytt projekt](./media/web-sites-python-ptvs-django-sql/PollsDjangoNewProject.png)
3. Du kommer att tillfrågas tooinstall externa paket. Välj **Install into a virtual environment** (Installera i en virtuell miljö).

     ![Dialogrutan Externa paket](./media/web-sites-python-ptvs-django-sql/PollsDjangoExternalPackages.png)
4. Välj **Python 2.7** som hello bastolk.

     ![Dialogrutan Add Virtual Environment (Lägg till virtuell miljö)](./media/web-sites-python-ptvs-django-sql/PollsCommonAddVirtualEnv.png)
5. I **Solution Explorer**, högerklicka på hello projektnoden och väljer **Python**, och välj sedan **Django migrera**.  Välj därefter **Django – skapa superanvändare**.
6. Detta öppnar Django Management Console och skapa en sqlite-databas i hello projektmappen. Följ hello prompter toocreate en användare.
7. Bekräfta att hello programmet fungerar genom att trycka på <kbd>F5</kbd>.
8. Klicka på **logga in** från hello navigeringsfältet hello överst.

     ![Webbläsare](./media/web-sites-python-ptvs-django-sql/PollsDjangoCommonBrowserLocalMenu.png)
9. Ange hello autentiseringsuppgifter för hello användaren du skapade när du synkroniserade hello-databasen.

     ![Webbläsare](./media/web-sites-python-ptvs-django-sql/PollsDjangoCommonBrowserLocalLogin.png)
10. Klicka på **Create Sample Polls** (Skapa exempelomröstningar).

      ![Webbläsare](./media/web-sites-python-ptvs-django-sql/PollsDjangoCommonBrowserNoPolls.png)
11. Klicka på en omröstning och rösta.

      ![Webbläsare](./media/web-sites-python-ptvs-django-sql/PollsDjangoSqliteBrowser.png)

## <a name="create-a-sql-database"></a>Skapa en SQL Database
Hello-databas ska du skapa en Azure SQL database.

Du kan skapa en databas genom att följa dessa steg.

1. Logga in på hello [Azure Portal].
2. Hello längst ned i navigeringsfönstret hello klickar du på **ny**. , klickar du på **Data + lagring** > **SQL-databas**.
3. Konfigurera hello ny SQL-databas genom att skapa en ny resursgrupp och välj hello lämplig plats för den.
4. När hello SQL-databas har skapats klickar du på **öppna i Visual Studio** i hello databasbladet.
5. Klicka på **konfigurera brandväggen**.
6. I hello **brandväggsinställningar** bladet Lägg till en brandväggsregel med **första IP-** och **sista IP** toohello offentliga IP-adressen på utvecklingsdatorn. Klicka på **Spara**.

   Detta gör att anslutningar toohello databasservern från utvecklingsdatorn.
7. Klicka på tillbaka i hello databasbladet **egenskaper**, klicka på **visa databasanslutningssträngar**.
8. Använda hello Kopiera knappen tooput hello värdet **ADO.NET** hello Urklipp.

## <a name="configure-hello-project"></a>Konfigurera hello projekt
I det här avsnittet konfigurerar vi våra web app toouse hello SQL-databas vi just skapade. Vi måste också installera ytterligare Python-paket krävs toouse SQL-databaser med Django. Vi kommer kör hello webbappen lokalt.

1. Öppna i Visual Studio **settings.py**, från hello *projektnamn* mapp. Klistra in hello anslutningssträngen i hello-redigeraren. hello anslutningssträngen är i formatet:

       Server=<ServerName>,<ServerPort>;Database=<DatabaseName>;User ID=<UserName>;Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;

Redigera hello definitionen av `DATABASES` toouse hello värdena ovan.

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

1. I Solution Explorer under **Python-miljöer**, högerklicka på hello virtuella miljön och välj **Install Python Package**.
2. Installera hello paketet `pyodbc` med **pip**.

     ![Python dialogrutan Installera paket](./media/web-sites-python-ptvs-django-sql/PollsDjangoSqlInstallPackagePyodbc.png)
3. Installera hello paketet `django-pyodbc-azure` med **pip**.

     ![Python dialogrutan Installera paket](./media/web-sites-python-ptvs-django-sql/PollsDjangoSqlInstallPackageDjangoPyodbcAzure.png)
4. I **Solution Explorer**, högerklicka på hello projektnoden och väljer **Python**, och välj sedan **Django migrera**.  Välj därefter **Django – skapa superanvändare**.

   Detta skapar hello tabeller för hello SQL-databas som vi skapade i föregående avsnitt i hello. Följ hello prompter toocreate en användare som inte har toomatch hello användaren hello sqlite-databas som skapats i hello första avsnittet.
5. Kör programmet hello med `F5`. Omröstningar som skapas med **skapa Exempelomröstningar** och hello-data som skickats vid röstning serialiseras i hello SQL-databas.

## <a name="publish-hello-web-app-tooazure-app-service"></a>Publicera hello web app tooAzure Apptjänst
hello Azure .NET SDK innehåller ett enkelt sätt toodeploy din web web app tooAzure App Service Web Apps.

1. I **Solution Explorer**, högerklicka på hello projektnoden och väljer **publicera**.

     ![Dialogrutan Publicera webbplats](./media/web-sites-python-ptvs-django-sql/PollsCommonPublishWebSiteDialog.png)
2. Klicka på **Microsoft Azure Web Apps**.
3. Klicka på **ny** toocreate ett nytt webbprogram.
4. Fyll i följande fält hello och på **skapa**.

   * **Webbappens namn**
   * **App Service-plan**
   * **Resursgrupp**
   * **Region**
   * Lämna **databasserver** ställa in också**ingen databas**
5. Godkänn alla standardvärden och klicka på **Publicera**.
6. Webbläsaren öppnas automatiskt toohello publicerade webbprogram. Du bör se hello web app fungerar som förväntat, med hjälp av hello **SQL** databasen på Azure.

   Grattis!

     ![Webbläsare](./media/web-sites-python-ptvs-django-sql/PollsDjangoAzureBrowser.png)

## <a name="next-steps"></a>Nästa steg
Följ dessa länkar toolearn mer om Python Tools för Visual Studio, Django och SQL-databas.

* [Dokumentation för Python Tools för Visual Studio]
  * [Webbprojekt]
  * [Molntjänstprojekt]
  * [Fjärrfelsökning i Microsoft Azure]
* [Django-dokumentation]
* [SQL Database]

## <a name="whats-changed"></a>Nyheter
* En guide toohello övergången från webbplatser tooApp tjänsten finns: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)

<!--Link references-->
[Python Developer Center]: /develop/python/
[Azure Cloud Services]: ../cloud-services/cloud-services-python-ptvs.md

<!--External Link references-->
[Azure Portal]: https://portal.azure.com
[Python Tools för Visual Studio]: http://aka.ms/ptvs
[Python Tools 2.2 för Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025
[Python Tools 2.2 för Visual Studio Samples VSI]: http://go.microsoft.com/fwlink/?LinkID=624025
[Azure SDK Tools för VS 2015]: http://go.microsoft.com/fwlink/?LinkId=518003
[Python 2.7 32-bitars]: http://go.microsoft.com/fwlink/?LinkId=517190
[Dokumentation för Python Tools för Visual Studio]: http://aka.ms/ptvsdocs
[Fjärrfelsökning i Microsoft Azure]: http://go.microsoft.com/fwlink/?LinkId=624026
[Webbprojekt]: http://go.microsoft.com/fwlink/?LinkId=624027
[Molntjänstprojekt]: http://go.microsoft.com/fwlink/?LinkId=624028
[Django-dokumentation]: https://www.djangoproject.com/
[SQL Database]: /documentation/services/sql-database/
