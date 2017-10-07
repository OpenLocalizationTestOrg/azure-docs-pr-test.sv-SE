---
title: aaaCreating webbappar med Django i Azure
description: "En självstudiekurs som presenteras toorunning en Python-webbapp i Azure App Service Web Apps."
services: app-service\web
documentationcenter: python
tags: python
author: huguesv
manager: erikre
editor: 
ms.assetid: 9be1a05a-9460-49ae-94fb-9798f82c11cf
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 02/19/2016
ms.author: huvalo
ms.openlocfilehash: 26a131da358748bd6fe4ee5c114d0a8f91b83cfe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="creating-web-apps-with-django-in-azure"></a>Skapa webbappar med Django i Azure
Den här självstudiekursen beskriver hur tooget igång med Python i [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714). Med Web Apps får du begränsat ledigt värdutrymme och snabb distribution och kan använda Python. När din app växer, kan du växla toopaid värd och du kan också integrera med alla hello andra Azure-tjänster.

Du skapar ett program med hello Djangos webbramverk (se motsvarande versioner av kursen för [Flask](web-sites-python-create-deploy-flask-app.md) och [Bottle](web-sites-python-create-deploy-bottle-app.md)). Du ska skapa hello webbprogrammet från hello Azure Marketplace, konfigurera Git-distribution och klona hello lagringsplatsen lokalt. Du kommer sedan köra hello programmet lokalt, gör ändringar, genomför och push-installera dem tooAzure. Hej självstudiekurs visar hur toodo från Windows eller Mac/Linux.

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service. Inget kreditkort krävs, och du gör inga åtaganden.
> 
> 

## <a name="prerequisites"></a>Krav
* Windows, Mac eller Linux
* Python 2.7 eller 3.4
* installationsverktyg, pip, virtuell miljö (endast Python 2.7)
* Git
* [Python Tools för Visual Studio][Python Tools för Visual Studio] (PTVS) - Obs! det här är valfritt

**Obs**! TFS-publicering stöds för närvarande inte för Python-projekt.

### <a name="windows"></a>Windows
Om du inte redan har Python 2.7 eller 3.4 installerat (32-bitars) rekommenderar vi att du installerar [Azure SDK för Python 2.7] eller [Azure SDK för Python 3.4] med hjälp av installationsprogrammet för webbplattform. Detta installerar hello 32-bitarsversionen av Python, installationsverktyg, pip, virtuell miljö och så vidare (32-bitarsversionen av Python är vad som är installerat på hello Azure-värddatorerna). Du kan också hämta Python på [python.org].

För Git rekommenderar vi [Git för Windows] eller [GitHub för Windows]. Om du använder Visual Studio, kan du använda hello integrerad Git-stödet.

Vi rekommenderar också att du installerar [Python Tools 2.2 för Visual Studio]. Det här är valfritt, men om du har [Visual Studio], inklusive hello kostnadsfri Visual Studio Community 2013 eller Visual Studio Express 2013 för webben och det ger dig en utmärkt Python IDE.

### <a name="maclinux"></a>Mac/Linux
Du bör redan ha Python och Git installerade, men kontrollera att du har Python 2.7 eller 3.4.

## <a name="web-app-creation-on-portal"></a>Skapa en webbapp i Azure Portal
hello första steget i att skapa din app är toocreate hello webbprogram via hello [Azure Portal](https://portal.azure.com).

1. Logga in på hello Azure-portalen och klicka på hello **ny** knappen i hello nedre vänstra hörnet.
2. Skriv ”python” i sökrutan hello.
3. I sökresultaten hello väljer **Django** (som publicerats av PTVS) Klicka **skapa**.
4. Konfigurera hello nya Django-appen, till exempel skapa en ny App Service-plan och en ny resursgrupp för den. Klicka på **Skapa**.
5. Konfigurera Git-publicering för nyskapade webbappen genom att följa anvisningarna hello på [lokal Git-distribution tooAzure Apptjänst](app-service-deploy-local-git.md).

## <a name="application-overview"></a>Programöversikt
### <a name="git-repository-contents"></a>Innehåll på Git-lagringsplatsen
Här är en översikt över hello-filer som du hittar i hello första Git-lagringsplatsen, som vi ska klona i nästa avsnitt om hello.

    \app\__init__.py
    \app\forms.py
    \app\models.py
    \app\tests.py
    \app\views.py
    \app\static\content\
    \app\static\fonts\
    \app\static\scripts\
    \app\templates\about.html
    \app\templates\contact.html
    \app\templates\index.html
    \app\templates\layout.html
    \app\templates\login.html
    \app\templates\loginpartial.html
    \DjangoWebProject\__init__.py
    \DjangoWebProject\settings.py
    \DjangoWebProject\urls.py
    \DjangoWebProject\wsgi.py

Programmet hello huvudsakliga källor. Består av tre sidor (index, om och kontakt) med bakgrundslayout. Statiskt innehåll och skript inkluderar bootstrap, jquery, modernizr och respond.

    \manage.py

Stöd för lokal hanterings- och utvecklingsserver. Använda toorun hello programmet lokalt, synkronisera databasen hello osv.

    \db.sqlite3

Standarddatabas. Inkluderar hello nödvändiga tabeller för hello programmet toorun, men innehåller inte några användare (synkronisera hello databasen toocreate en användare).

    \DjangoWebProject.pyproj
    \DjangoWebProject.sln

Projektfiler för användning med [Python Tools för Visual Studio].

    \ptvs_virtualenv_proxy.py

IIS-proxy för virtuella miljöer och PTVS-stöd för fjärrfelsökning.

    \requirements.txt

Externa paket som krävs för programmet. hello distributionsskriptet pip hello installationspaket som anges i den här filen.

    \web.2.7.config
    \web.3.4.config

IIS-konfigurationsfiler. hello distributionsskriptet använder lämplig web.x.y.config för hello och kopian web.config.

### <a name="optional-files---customizing-deployment"></a>Valfria filer – anpassa distributionen
[!INCLUDE [web-sites-python-django-customizing-deployment](../../includes/web-sites-python-django-customizing-deployment.md)]

### <a name="optional-files---python-runtime"></a>Valfria filer – Python-körning
[!INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

### <a name="additional-files-on-server"></a>Ytterligare filer på servern
Vissa filer finnas på hello-servern men läggs inte toohello git-lagringsplats. Dessa skapas med hello distributionsskriptet.

    \web.config

IIS-konfigurationsfilen. Skapas utifrån web.x.y.config vid varje distribution.

    \env\

Python, virtuell miljö Skapas under distributionen om det inte redan finns en kompatibel virtuell miljö hello webbprogrammet. Paket som anges i requirements.txt är pip-installeras, men pip hoppar över installationen om paketen hello redan är installerade.

hello 3 nästa avsnitt beskrivs hur tooproceed med hello webbappsutveckling under 3 olika miljöer:

* Windows med Python Tools för Visual Studio
* Windows, med kommandorad
* Mac/Linux, med kommandorad

## <a name="web-app-development---windows---python-tools-for-visual-studio"></a>Webbappsutveckling – Windows – Python Tools för Visual Studio
### <a name="clone-hello-repository"></a>Klona hello databasen
Först klonar hello-databas med hjälp av hello Webbadressen som tillhandahålls på hello Azure-portalen. Mer information finns i [lokal Git-distribution tooAzure Apptjänst](app-service-deploy-local-git.md).

Öppna hello lösningsfilen (.sln) som ingår i hello rot hello-databasen.

![](./media/web-sites-python-create-deploy-django-app/ptvs-solution-django.png)

### <a name="create-virtual-environment"></a>Skapa en virtuell miljö
Nu ska vi skapa en virtuell miljö för lokal utveckling. Högerklicka på **Python Environments** (Python-miljöer) och välj **Add Virtual Environment...** (Lägg till virtuell miljö).

* Kontrollera hello hello miljö heter `env`.
* Välj hello bastolk. Kontrollera att toouse hello samma version av Python som valts för webbappen (i runtime.txt eller hello **programinställningar** bladet för din webbapp i hello Azure Portal).
* Kontrollera att hello alternativet toodownload och installera paket är markerat.

![](./media/web-sites-python-create-deploy-django-app/ptvs-add-virtual-env-27.png)

Klicka på **Skapa**. Detta skapar hello virtuell miljö och installera beroenden som anges i requirements.txt.

### <a name="create-a-superuser"></a>Skapa en användare med fullständig behörighet
hello har ingår hello programmet inte definierats en superanvändare. Ordning toouse hello inloggning funktionsändringar i hello program eller hello administratörsgränssnittet för Django (om du väljer tooenable den), behöver du toocreate superanvändare.

Kör det från hello kommandoraden i projektmappen:

    env\scripts\python manage.py createsuperuser

Följ hello prompter tooset hello användarnamn, lösenord osv.

### <a name="run-using-development-server"></a>Kör med utvecklingsservern
Tryck på F5 toostart felsökning och webbläsaren öppnas automatiskt toohello sidan som körs lokalt.

![](./media/web-sites-python-create-deploy-django-app/windows-browser-django.png)

Du kan ange brytpunkter i hello källor, Använd hello titta på windows osv. Se hello [Python Tools för Visual Studio-dokumentationen] hello mer information om olika funktioner.

### <a name="make-changes"></a>Göra ändringar
Nu kan du experimentera med att göra ändringar toohello programmets källor och mallar.

När du har testat ändringarna checkar du in dem toohello Git-lagringsplatsen:

![](./media/web-sites-python-create-deploy-django-app/ptvs-commit-django.png)

### <a name="install-more-packages"></a>Installera fler paket
Programmet kan ha beroenden utöver Python och Django.

Du kan installera ytterligare paket med hjälp av pip. tooinstall ett paket, högerklicka på hello virtuell miljö och välj **Install Python Package**.

Till exempel tooinstall hello Azure SDK för Python, som ger dig åtkomst tooAzure storage, service bus och andra Azure-tjänster, ange `azure`:

![](./media/web-sites-python-create-deploy-django-app/ptvs-install-package-dialog.png)

Högerklicka på hello virtuell miljö och välj **generera requirements.txt** tooupdate requirements.txt.

Checka sedan in hello ändringar toorequirements.txt toohello Git-lagringsplats.

### <a name="deploy-tooazure"></a>Distribuera tooAzure
tootrigger en distribution, klicka på **Sync** eller **Push**. Med synkronisering görs både en överföring och en hämtning.

![](./media/web-sites-python-create-deploy-django-app/ptvs-git-push.png)

hello första distributionen tar en stund, eftersom den skapar en virtuell miljö, paket installeras osv.

Visual Studio visas inte hello fortskrider hello-distribution. Om du vill att tooreview hello utdata avsnittet hello på [felsökning – distribution](#troubleshooting-deployment).

Bläddra toohello Azure URL tooview ändringarna.

## <a name="web-app-development---windows---command-line"></a>Webbappsutveckling – Windows – kommandorad
### <a name="clone-hello-repository"></a>Klona hello databasen
Först klonar hello lagringsplatsen med hjälp av hello Webbadressen som tillhandahålls på hello Azure-portalen och lägga till hello Azure-lagringsplatsen som fjärransluten. Mer information finns i [lokal Git-distribution tooAzure Apptjänst](app-service-deploy-local-git.md).

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url>

### <a name="create-virtual-environment"></a>Skapa en virtuell miljö
Vi ska skapa en ny virtuell miljö för utveckling (inte Lägg till den toohello databasen). Virtuella miljöer i Python är inte relokerbara, så alla utvecklare som arbetar på hello programmet skapa sina egna lokalt.

Kontrollera att toouse hello samma version av Python som valts för webbappen (i runtime.txt eller hello bladet programinställningar för webbappen i hello Azure Portal).

För Python 2.7:

    c:\python27\python.exe -m virtualenv env

För Python 3.4:

    c:\python34\python.exe -m venv env

Installera eventuella externa paket som krävs för programmet. Du kan använda hello requirements.txt filen i hello rot hello databasen tooinstall hello paket i den virtuella miljön:

    env\scripts\pip install -r requirements.txt

### <a name="create-a-superuser"></a>Skapa en användare med fullständig behörighet
hello har ingår hello programmet inte definierats en superanvändare. Ordning toouse hello inloggning funktionsändringar i hello program eller hello administratörsgränssnittet för Django (om du väljer tooenable den), behöver du toocreate superanvändare.

Kör det från hello kommandoraden i projektmappen:

    env\scripts\python manage.py createsuperuser

Följ hello prompter tooset hello användarnamn, lösenord osv.

### <a name="run-using-development-server"></a>Kör med utvecklingsservern
Du kan starta programmet hello under en utvecklingsserver med hello följande kommando:

    env\scripts\python manage.py runserver

hello konsolen visar hello URL-adress och port hello servern lyssnar på:

![](./media/web-sites-python-create-deploy-django-app/windows-run-local-django.png)

Öppna din Webbadress webbläsare toothat.

![](./media/web-sites-python-create-deploy-django-app/windows-browser-django.png)

### <a name="make-changes"></a>Göra ändringar
Nu kan du experimentera med att göra ändringar toohello programmets källor och mallar.

När du har testat ändringarna checkar du in dem toohello Git-lagringsplatsen:

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a>Installera fler paket
Programmet kan ha beroenden utöver Python och Django.

Du kan installera ytterligare paket med hjälp av pip. Till exempel tooinstall hello Azure SDK för Python, som ger dig åtkomst till tooAzure storage, service bus och andra Azure-tjänster, typ:

    env\scripts\pip install azure

Se till att tooupdate requirements.txt:

    env\scripts\pip freeze > requirements.txt

Genomför hello ändringar:

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-tooazure"></a>Distribuera tooAzure
tootrigger en distribution push hello ändrar tooAzure:

    git push azure master

Du ser hello utdata från hello distributionsskriptet, inklusive skapande av virtuell miljö, installation av paket och skapande av web.config.

Bläddra toohello Azure URL tooview ändringarna.

## <a name="web-app-development---maclinux---command-line"></a>Webbappsutveckling – Mac/Linux – kommandorad
### <a name="clone-hello-repository"></a>Klona hello databasen
Först klonar hello lagringsplatsen med hjälp av hello Webbadressen som tillhandahålls på hello Azure-portalen och lägga till hello Azure-lagringsplatsen som fjärransluten. Mer information finns i [lokal Git-distribution tooAzure Apptjänst](app-service-deploy-local-git.md).

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url>

### <a name="create-virtual-environment"></a>Skapa en virtuell miljö
Vi ska skapa en ny virtuell miljö för utveckling (inte Lägg till den toohello databasen). Virtuella miljöer i Python är inte relokerbara, så alla utvecklare som arbetar på hello programmet skapa sina egna lokalt.

Kontrollera att toouse hello samma version av Python som valts för webbappen (i runtime.txt eller hello bladet programinställningar för webbappen i hello Azure Portal).

För Python 2.7:

    python -m virtualenv env

För Python 3.4:

    python -m venv env

eller

    pyvenv env

Installera eventuella externa paket som krävs för programmet. Du kan använda hello requirements.txt filen i hello rot hello databasen tooinstall hello paket i den virtuella miljön:

    env/bin/pip install -r requirements.txt

### <a name="create-a-superuser"></a>Skapa en användare med fullständig behörighet
hello har ingår hello programmet inte definierats en superanvändare. Ordning toouse hello inloggning funktionsändringar i hello program eller hello administratörsgränssnittet för Django (om du väljer tooenable den), behöver du toocreate superanvändare.

Kör det från hello kommandoraden i projektmappen:

    env/bin/python manage.py createsuperuser

Följ hello prompter tooset hello användarnamn, lösenord osv.

### <a name="run-using-development-server"></a>Kör med utvecklingsservern
Du kan starta programmet hello under en utvecklingsserver med hello följande kommando:

    env/bin/python manage.py runserver

hello konsolen visar hello URL-adress och port hello servern lyssnar på:

![](./media/web-sites-python-create-deploy-django-app/mac-run-local-django.png)

Öppna din Webbadress webbläsare toothat.

![](./media/web-sites-python-create-deploy-django-app/mac-browser-django.png)

### <a name="make-changes"></a>Göra ändringar
Nu kan du experimentera med att göra ändringar toohello programmets källor och mallar.

När du har testat ändringarna checkar du in dem toohello Git-lagringsplatsen:

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a>Installera fler paket
Programmet kan ha beroenden utöver Python och Django.

Du kan installera ytterligare paket med hjälp av pip. Till exempel tooinstall hello Azure SDK för Python, som ger dig åtkomst till tooAzure storage, service bus och andra Azure-tjänster, typ:

    env/bin/pip install azure

Se till att tooupdate requirements.txt:

    env/bin/pip freeze > requirements.txt

Genomför hello ändringar:

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-tooazure"></a>Distribuera tooAzure
tootrigger en distribution push hello ändrar tooAzure:

    git push azure master

Du ser hello utdata från hello distributionsskriptet, inklusive skapande av virtuell miljö, installation av paket och skapande av web.config.

Bläddra toohello Azure URL tooview ändringarna.

## <a name="troubleshooting---package-installation"></a>Felsökning – installation av paket
[!INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]

## <a name="troubleshooting---virtual-environment"></a>Felsökning – virtuell miljö
[!INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]

## <a name="troubleshooting---static-files"></a>Felsökning – statiska filer
Django har hello begreppet att samla in statiska filer. Detta tar alla hello statiska filer från deras ursprungliga plats och kopierar dem tooa enstaka mapp. För det här programmet kopieras de för`/static`.

Det görs eftersom statiska filer kan komma från olika "Django-appar". Till exempel finns hello statiska filer från hello administratörsgränssnittet för django i undermappen Django-biblioteket i hello virtuell miljö. Statiska filer som definieras av programmet finns i `/app/static`. När du använder flera "Django-appar" sparas statiska filer på flera platser.

När du kör programmet hello i felsökningsläge hanterar hello programmet hello statiska filer från deras ursprungliga plats.

När du kör hello programmet i versionsläge hello program har **inte** hantera hello statiska filer. Det är hello ansvar hello web server tooserve hello-filer. För det här programmet hanterar IIS hello statiska filer från `/static`.

hello insamlingen av statiska filer görs automatiskt som en del av hello distributionsskriptet rensar tidigare insamlade filer. Detta innebär hello samlingen sker för varje distribution långsammare distribution lite, men ser till att filer inte tillgängliga, vilket undanröjer potentiella säkerhetsproblem.

Om du vill tooskip insamlingen av statiska filer för Django-programmet:

    \.skipDjango

Sedan behöver du toodo hello samlingen manuellt på den lokala datorn:

    env\scripts\python manage.py collectstatic

Ta bort hello `\static` mapp från `.gitignore` och lägga till den toohello Git-lagringsplats.

## <a name="troubleshooting---settings"></a>Felsökning – inställningar
Olika inställningar för programmet hello kan ändras i `DjangoWebProject/settings.py`.

För enklare utveckling är felsökningsläget aktiverat. En bra bieffekt som är kommer du att kunna toosee bilder och annat statiskt innehåll vid lokal körning utan toocollect statiska filer.

toodisable felsökningsläget:

    DEBUG = False

När felsökningen är inaktiverad hello värde för `ALLOWED_HOSTS` behov toobe uppdateras tooinclude hello Azure-värdnamnet. Exempel:

    ALLOWED_HOSTS = (
        'pythonapp.azurewebsites.net',
    )

eller tooenable alla:

    ALLOWED_HOSTS = (
        '*',
    )

I praktiken kan kanske du vill toodo något mer komplexa toodeal växlingen mellan felsöka och versionsläge och hämta hello-värdnamn.

Du kan ange miljövariabler hello Azure-portalen **konfigurera** i hello sidan **appinställningar** avsnitt.  Detta kan vara användbart för att ange värden som du inte kan ha tooappear i hello källorna (anslutningssträngar, lösenord osv) eller som du vill tooset annorlunda mellan Azure och den lokala datorn. I `settings.py`, du kan fråga hello miljövariablerna med hjälp av `os.getenv`.

## <a name="using-a-database"></a>Använda en databas
hello-databasen som ingår i programmet hello är en sqlite-databas. Detta är en praktisk standard databasen toouse för utveckling, eftersom den kräver nästan några inställningar. hello databasen lagras i hello DB.sqlite3 i projektmappen hello.

Azure tillhandahåller databastjänster som är lätt toouse från ett Django-program. Självstudier för att använda [SQL-databas] och [MySQL] från ett Django-program visar hello steg nödvändiga toocreate hello databastjänsten, ändra databasinställningarna för hello i `DjangoWebProject/settings.py`, och hello bibliotek krävs tooinstall.

Om du vill toomanage egna databasservrar kan göra du självklart det med hjälp av Windows eller Linux virtuella datorer som körs på Azure.

## <a name="django-admin-interface"></a>Administratörsgränssnittet för Django
När du börjar bygga modeller vill du antagligen toopopulate hello databasen med vissa data. Ett enkelt sätt toodo lägga till och redigera innehåll interaktivt är toouse hello administratörsgränssnittet.

hello-koden för hello administratörsgränssnittet har kommenterats ut i programkällorna hello, men den är tydligt markerad så att du enkelt kan aktivera den (Sök efter ”admin”).

När den har aktiverats synkronisera hello databas, köra programmet hello och navigera för`/admin`.

## <a name="next-steps"></a>Nästa steg
Följ dessa länkar toolearn mer om Django och Python Tools för Visual Studio:

* [Django-dokumentation]
* [Python Tools för Visual Studio-dokumentationen]

Information om hur du använder SQL Database och MySQL finns i:

* [Django och MySQL på Azure med Python Tools för Visual Studio]
* [Django och SQL Database på Azure med Python Tools för Visual Studio]

Mer information finns i hello [Python Developer Center](/develop/python/).

## <a name="whats-changed"></a>Nyheter
* En guide toohello övergången från webbplatser tooApp tjänsten finns: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)

<!--Link references-->
[Django och MySQL på Azure med Python Tools för Visual Studio]: web-sites-python-ptvs-django-mysql.md
[Django och SQL Database på Azure med Python Tools för Visual Studio]: web-sites-python-ptvs-django-sql.md
[SQL-databas]: web-sites-python-ptvs-django-sql.md
[MySQL]: web-sites-python-ptvs-django-mysql.md

<!--External Link references-->
[Azure SDK för Python 2.7]: http://go.microsoft.com/fwlink/?linkid=254281
[Azure SDK för Python 3.4]: http://go.microsoft.com/fwlink/?linkid=516990
[python.org]: http://www.python.org/
[Git för Windows]: http://msysgit.github.io/
[GitHub för Windows]: https://windows.github.com/
[Python Tools för Visual Studio]: http://aka.ms/ptvs
[Python Tools 2.2 för Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025
[Visual Studio]: http://www.visualstudio.com/
[Python Tools för Visual Studio-dokumentationen]: http://aka.ms/ptvsdocs
[Django-dokumentation]: https://www.djangoproject.com/
