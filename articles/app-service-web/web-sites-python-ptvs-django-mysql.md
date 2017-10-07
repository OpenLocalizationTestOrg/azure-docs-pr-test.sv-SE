---
title: "aaaDjango och MySQL på Azure med Python Tools 2.2 för Visual Studio"
description: "Lär dig hur toouse hello Python Tools för Visual Studio toocreate en Django-webbapp som lagrar data i en MySQL-databasinstans och distribuera den tooAzure App Service Web Apps."
services: app-service\web
documentationcenter: python
author: huguesv
manager: erikre
editor: 
ms.assetid: c60a50b5-8b5e-4818-a442-16362273dabb
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 07/07/2016
ms.author: huvalo
ms.openlocfilehash: 1597c391d20c8e8ef629b4e4d05c9eb64c83bffc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="django-and-mysql-on-azure-with-python-tools-22-for-visual-studio"></a>Django och MySQL på Azure med Python Tools 2.2 för Visual Studio
[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

I den här kursen ska du använda [Python Tools för Visual Studio](https://www.visualstudio.com/vs/python) toocreate ett enkelt avfrågar webbapp med någon av exempelmallarna i hello PTVS. Du lär dig hur toouse en MySQL-tjänst finns i Azure, hur tooconfigure hello web app toouse MySQL och hur toopublish hello webbprogrammet för[Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).

> [!NOTE]
> hello informationen i den här kursen finns också i följande video hello:
> 
> [PTVS 2.1: Django-app med MySQL][video]
> 
> 

Se hello [Python Developer Center] fler artiklar om utvecklingen av Azure App Service Web Apps med PTVS med hjälp av Bottle, Flask och Django webbramverken med Azure Table Storage, MySQL och SQL-databas. Den här artikeln fokuserar på App Service, hello stegen är liknande när du utvecklar [Azure Cloud Services].

## <a name="prerequisites"></a>Krav
* Visual Studio 2015
* [Python 2.7 32-bitars] eller [Python 3.4 32-bitars]
* [Python Tools 2.2 för Visual Studio]
* [Python Tools 2.2 för Visual Studio Samples VSIX]
* [Azure SDK Tools för VS 2015]
* Django 1.9 eller senare

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

<!-- This note should not render as part of hello hello previous include. -->

> [!NOTE]
> Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service. Inget kreditkort krävs och du behöver inte göra några åtaganden.
> 
> 

## <a name="create-hello-project"></a>Skapa hello projekt
I det här avsnittet får du skapa ett Visual Studio-projekt utifrån en exempelmall. Du får skapa en virtuell miljö och installera nödvändiga paket. Du får skapa en lokal databas med hjälp av sqlite. Du måste köra hello programmet lokalt.

1. I Visual Studio väljer du **Arkiv**, **Nytt projekt**.
2. Hej projektmallar från hello [Python Tools 2.2 för Visual Studio Samples VSI] är tillgängliga under **Python**, **exempel**. Välj **Polls Django Web Project** och klicka på OK toocreate hello projektet.
   
    ![Dialogrutan Nytt projekt](./media/web-sites-python-ptvs-django-mysql/PollsDjangoNewProject.png)
3. Du kommer att tillfrågas tooinstall externa paket. Välj **Install into a virtual environment** (Installera i en virtuell miljö).
   
    ![Dialogrutan Externa paket](./media/web-sites-python-ptvs-django-mysql/PollsDjangoExternalPackages.png)
4. Välj **Python 2.7** eller **Python 3.4** som hello bastolk.
   
    ![Dialogrutan Add Virtual Environment (Lägg till virtuell miljö)](./media/web-sites-python-ptvs-django-mysql/PollsCommonAddVirtualEnv.png)
5. I **Solution Explorer**, högerklicka på hello projektnoden och väljer **Python**, och välj sedan **Django migrera**.  Välj därefter **Django – skapa superanvändare**.
6. Detta öppnar Django Management Console och skapa en sqlite-databas i hello projektmappen. Följ hello prompter toocreate en användare.
7. Bekräfta att hello programmet fungerar genom att trycka på `F5`.
8. Klicka på **logga in** från hello navigeringsfältet hello överst.
   
    ![Navigeringsfältet i Django](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserLocalMenu.png)
9. Ange hello autentiseringsuppgifter för hello användaren du skapade när du synkroniserade hello-databasen.
   
    ![Inloggningsformulär](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserLocalLogin.png)
10. Klicka på **Create Sample Polls** (Skapa exempelomröstningar).
    
     ![Create Sample Polls (Skapa exempelomröstningar)](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserNoPolls.png)
11. Klicka på en omröstning och rösta.
    
     ![Rösta i exempelomröstningar](./media/web-sites-python-ptvs-django-mysql/PollsDjangoSqliteBrowser.png)

## <a name="create-a-mysql-database"></a>Skapa en MySQL-databas
Hello-databas ska du skapa en värdbaserad ClearDB MySQL-databas på Azure.

Du kan också välja att skapa din egen virtuella dator som körs i Azure och sedan installera och administrera MySQL själv.

Du kan skapa en databas med en kostnadsfri plan genom att följa dessa steg.

1. Logga in toohello [Azure Portal].
2. Hello överkant hello navigeringsfönstret, klickar du på **ny**, klicka på **Data + lagring**, och klicka sedan på **MySQL-databas**.
3. Konfigurera hello ny MySQL-databas genom att skapa en ny resursgrupp och välj hello lämplig plats för den.
4. När hello MySQL-databas har skapats klickar du på **egenskaper** i hello databasbladet.
5. Använda hello Kopiera knappen tooput hello värdet **ANSLUTNINGSSTRÄNGEN** hello Urklipp.

## <a name="configure-hello-project"></a>Konfigurera hello projekt
I det här avsnittet får du konfigurera våra web app toouse hello MySQL-databasen du nyss skapade. Du måste också installera ytterligare Python-paket krävs toouse MySQL-databaser med Django. Du måste köra hello webbappen lokalt.

1. Öppna i Visual Studio **settings.py**, från hello *projektnamn* mapp. Klistra in hello anslutningssträngen i hello-redigeraren. hello anslutningssträngen är i formatet:
   
        Database=<NAME>;Data Source=<HOST>;User Id=<USER>;Password=<PASSWORD>
   
    Ändra hello standarddatabasen **motorn** toouse MySQL och ange hello värden för **namn**, **användare**, **lösenord** och ** VÄRDEN** från hello **CONNECTIONSTRING**.
   
        DATABASES = {
            'default': {
                'ENGINE': 'django.db.backends.mysql',
                'NAME': '<Database>',
                'USER': '<User Id>',
                'PASSWORD': '<Password>',
                'HOST': '<Data Source>',
                'PORT': '',
            }
        }
2. I Solution Explorer under **Python-miljöer**, högerklicka på hello virtuella miljön och välj **Install Python Package**.
3. Installera hello paketet `mysqlclient` med **pip**.
   
    ![Dialogrutan Installera paket](./media/web-sites-python-ptvs-django-mysql/PollsDjangoMySQLInstallPackage.png)
4. I **Solution Explorer**, högerklicka på hello projektnoden och väljer **Python**, och välj sedan **Django migrera**.  Välj därefter **Django – skapa superanvändare**.
   
    Detta skapar hello tabeller för hello MySQL-databas som du skapade i föregående avsnitt i hello. Följ hello prompter toocreate en användare som inte har toomatch hello användaren hello sqlite-databas som skapats i hello första delen av den här artikeln.
5. Kör programmet hello med `F5`. Omröstningar som skapas med **skapa Exempelomröstningar** och hello-data som skickats vid röstning serialiseras i hello MySQL-databas.

## <a name="publish-hello-web-app-tooazure-app-service"></a>Publicera hello web app tooAzure Apptjänst
hello Azure .NET SDK innehåller ett enkelt sätt toodeploy din web app tooAzure Apptjänst.

1. I **Solution Explorer**, högerklicka på hello projektnoden och väljer **publicera**.
   
    ![Dialogrutan Publicera webbplats](./media/web-sites-python-ptvs-django-mysql/PollsCommonPublishWebSiteDialog.png)
2. Klicka på **Microsoft Azure App Service**.
3. Klicka på **ny** toocreate ett nytt webbprogram.
4. Fyll i följande fält hello och på **skapa**:
   
   * **Webbappens namn**
   * **App Service-plan**
   * **Resursgrupp**
   * **Region**
   * Lämna **databasserver** ställa in också**ingen databas**
5. Godkänn alla standardvärden och klicka på **Publicera**.
6. Webbläsaren öppnas automatiskt toohello publicerade webbprogram. Du bör se hello web app fungerar som förväntat, med hjälp av hello **MySQL** databasen på Azure.
   
    ![Webbläsare](./media/web-sites-python-ptvs-django-mysql/PollsDjangoAzureBrowser.png)
   
    Grattis! Du har publicerat din MySQL-baserade web app tooAzure.

## <a name="next-steps"></a>Nästa steg
Följ dessa länkar toolearn mer om Python Tools för Visual Studio, Django och MySQL.

* [Dokumentation för Python Tools för Visual Studio]
  * [Webbprojekt]
  * [Molntjänstprojekt]
  * [Fjärrfelsökning i Microsoft Azure]
* [Django-dokumentation]
* [MySQL]

Mer information finns i hello [Python Developer Center](/develop/python/).

<!--Link references-->

[Python Developer Center]: /develop/python/
[Azure Cloud Services]: ../cloud-services/cloud-services-python-ptvs.md

<!--External Link references-->

[Azure-portalen]: https://portal.azure.com
[Python Tools for Visual Studio]: https://www.visualstudio.com/vs/python/
[Python Tools 2.2 för Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025
[Python Tools 2.2 för Visual Studio Samples VSIX]: http://go.microsoft.com/fwlink/?LinkID=624025
[Azure SDK Tools för VS 2015]: http://go.microsoft.com/fwlink/?LinkId=518003
[Python 2.7 32-bitars]: http://go.microsoft.com/fwlink/?LinkId=517190
[Python 3.4 32-bitars]: http://go.microsoft.com/fwlink/?LinkId=517191
[Dokumentation för Python Tools för Visual Studio]: http://aka.ms/ptvsdocs
[Fjärrfelsökning i Microsoft Azure]: http://go.microsoft.com/fwlink/?LinkId=624026
[Webbprojekt]: http://go.microsoft.com/fwlink/?LinkId=624027
[Molntjänstprojekt]: http://go.microsoft.com/fwlink/?LinkId=624028
[Django-dokumentation]: https://www.djangoproject.com/
[MySQL]: http://www.mysql.com/
[video]: http://youtu.be/oKCApIrS0Lo
