---
title: Python webbappar med Bottle i Azure
description: "En introduktionskurs till att köra en Python-webbapp i Azure App Service Web Apps."
services: app-service\web
documentationcenter: python
tags: python
author: huguesv
manager: erikre
editor: 
ms.assetid: 84f043b8-9d05-479f-a9d0-d0d9a32a0bb9
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 02/19/2016
ms.author: huvalo
ms.openlocfilehash: de5831defc395cd8a4033be8c1fc5dc6cbc9d683
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="creating-web-apps-with-bottle-in-azure"></a>Skapa webbappar med Bottle i Azure
Den här självstudiekursen beskrivs hur du kommer igång med Python i Azure App Service Web Apps. Med Web Apps får du begränsat ledigt värdutrymme och snabb distribution och kan använda Python. I takt med att appen växer kan du gå över till betald hantering och även integrera med alla övriga Azure-tjänster.

Du skapar ett webbprogram med hjälp av Bottle webbramverk (se motsvarande versioner av kursen för [Django](web-sites-python-create-deploy-django-app.md) och [Flask](web-sites-python-create-deploy-flask-app.md)). Du får skapa webbappen från Azure Marketplace, konfigurera Git-distribution och klona lagringsplatsen lokalt. Du ska köra webbappen lokalt, göra ändringar, genomför och push-installera dem till [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714). I den här kursen får du veta hur du gör det från Windows eller Mac/Linux.

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> Om du vill komma igång med Azure App Service innan du registrerar dig för ett Azure-konto kan du gå till [Prova App Service](https://azure.microsoft.com/try/app-service/). Där kan du direkt skapa en tillfällig startwebbapp i App Service. Inga kreditkort krävs. Inga åtaganden.
> 
> 

## <a name="prerequisites"></a>Krav
* Windows, Mac eller Linux
* Python 2.7 eller 3.4
* installationsverktyg, pip, virtuell miljö (endast Python 2.7)
* Git
* [Python Tools 2.2 för Visual Studio][Python Tools 2.2 för Visual Studio] (PTVS) - Obs: det här är valfritt

**Obs**! TFS-publicering stöds för närvarande inte för Python-projekt.

### <a name="windows"></a>Windows
Om du inte redan har Python 2.7 eller 3.4 installerat (32-bitars) rekommenderar vi att du installerar [Azure SDK för Python 2.7] eller [Azure SDK för Python 3.4] med hjälp av installationsprogrammet för webbplattform. När du gör det installeras 32-bitarsversionen av Python, installationsverktyg, pip, virtuell miljö med mera. (Det är 32-bitarsversionen av Python som finns installerad på Azure-värddatorerna.) Du kan också hämta Python på [python.org].

För Git rekommenderar vi [Git för Windows] eller [GitHub för Windows]. Om du använder Visual Studio kan du använda det integrerade Git-stödet.

Vi rekommenderar också att du installerar [Python Tools 2.2 för Visual Studio]. Det här är valfritt, men om du har [Visual Studio], inklusive det kostnadsfria Visual Studio Community 2013 eller Visual Studio Express 2013 för webben, har du en utmärkt Python IDE.

### <a name="maclinux"></a>Mac/Linux
Du bör redan ha Python och Git installerade, men kontrollera att du har Python 2.7 eller 3.4.

## <a name="web-app-creation-on-the-azure-portal"></a>Skapa en webbapp i Azure Portal
Det första steget i att skapa din app är att skapa webbappen via [Azure Portal](https://portal.azure.com).  

1. Logga in på Azure Portal och klicka på knappen **NY** i det nedre vänstra hörnet. 
2. Skriv "python" i sökrutan.
3. I sökresultaten väljer **Bottle**, klicka på **skapa**.
4. Konfigurera den nya Bottle appen, till exempel skapa en ny App Service-plan och en ny resursgrupp för den. Klicka på **Skapa**.
5. Konfigurera Git-publicering för den nya webbappen genom att följa anvisningarna i [Lokal Git-distribution till Azure App Service](app-service-deploy-local-git.md).

## <a name="application-overview"></a>Programöversikt
### <a name="git-repository-contents"></a>Innehåll på Git-lagringsplatsen
Här är en översikt över filerna på den första Git-lagringsplatsen, som vi ska klona i nästa avsnitt.

    \routes.py
    \static\content\
    \static\fonts\
    \static\scripts\
    \views\about.tpl
    \views\contact.tpl
    \views\index.tpl
    \views\layout.tpl

Programmets huvudsakliga källor. Består av tre sidor (index, om och kontakt) med bakgrundslayout.  Statiskt innehåll och skript inkluderar bootstrap, jquery, modernizr och respond.

    \app.py

Stöd för lokal utveckling server. Används för att köra programmet lokalt.

    \BottleWebProject.pyproj
    \BottleWebProject.sln

Projektfiler för användning med [Python Tools för Visual Studio].

    \ptvs_virtualenv_proxy.py

IIS-proxy för virtuella miljöer och PTVS-stöd för fjärrfelsökning.

    \requirements.txt

Externa paket som krävs för programmet. Distributionsskriptet pip-installerar paketen som listas i den här filen.

    \web.2.7.config
    \web.3.4.config

IIS-konfigurationsfiler. Distributionsskriptet använder lämplig web.x.y.config och skapar kopian web.config.

### <a name="optional-files---customizing-deployment"></a>Valfria filer – anpassa distributionen
[!INCLUDE [web-sites-python-customizing-deployment](../../includes/web-sites-python-customizing-deployment.md)]

### <a name="optional-files---python-runtime"></a>Valfria filer – Python-körning
[!INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

### <a name="additional-files-on-server"></a>Ytterligare filer på servern
Det finns filer på servern som inte har lagts till på Git-lagringsplatsen. Dessa skapas med skriptet för distribution.

    \web.config

IIS-konfigurationsfilen. Skapas utifrån web.x.y.config vid varje distribution.

    \env\

Python, virtuell miljö Skapas under distributionen om det inte redan finns en kompatibel virtuell miljö i webbappen.  De paket som anges i requirements.txt pip-installeras, men pip hoppar över installationen om paketen redan är installerade.

I följande tre avsnitt beskrivs hur du fortsätter med webbappsutvecklingen i tre olika miljöer:

* Windows med Python Tools för Visual Studio
* Windows, med kommandorad
* Mac/Linux, med kommandorad

## <a name="web-app-development---windows---python-tools-for-visual-studio"></a>Webbappsutveckling – Windows – Python Tools för Visual Studio
### <a name="clone-the-repository"></a>Klona lagringsplatsen
Först klonar du lagringsplatsen med hjälp av webbadressen som tillhandahålls på Azure Portal. Mer information finns i [Lokal Git-distribution till Azure App Service](app-service-deploy-local-git.md).

Öppna lösningsfilen (.sln) som finns i lagringsplatsens rot.

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-solution-bottle.png)

### <a name="create-virtual-environment"></a>Skapa en virtuell miljö
Nu ska vi skapa en virtuell miljö för lokal utveckling. Högerklicka på **Python Environments** (Python-miljöer) och välj **Add Virtual Environment...** (Lägg till virtuell miljö).

* Kontrollera att namnet på miljön är `env`.
* Välj bastolk. Se till att använda samma version av Python som valts för webbappen (i runtime.txt eller i bladet **Programinställningar** för webbappen i Azure Portal).
* Kontrollera att alternativet för att ladda ned och installera paket är markerat.

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-add-virtual-env-27.png)

Klicka på **Skapa**. När du gör det skapas den virtuella miljön och beroenden som är angivna i requirements.txt installeras.

### <a name="run-using-development-server"></a>Kör med utvecklingsservern
Tryck på F5 för att starta felsökningen. Sidan som körs lokalt öppnas automatiskt i webbläsaren.

![](./media/web-sites-python-create-deploy-bottle-app/windows-browser-bottle.png)

Du kan ange brytpunkter i källorna, använda bevakningsfönstren med mera. Mer information om de olika funktionerna finns i [Dokumentationen om Python Tools för Visual Studio].

### <a name="make-changes"></a>Göra ändringar
Nu kan du experimentera med att göra ändringar i programmets källor och mallar.

När du har testat ändringarna checkar du in dem på Git-lagringsplatsen:

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-commit-bottle.png)

### <a name="install-more-packages"></a>Installera fler paket
Programmet kan ha beroenden utöver Python och Bottle.

Du kan installera ytterligare paket med hjälp av pip. Om du vill installera ett paket högerklickar du på den virtuella miljön och väljer **Install Python Package** (Installera Python-paket).

Om du till exempel vill installera Azure SDK för Python, som ger dig tillgång till Azure Storage, Service Bus och andra Azure-tjänster, anger du `azure`:

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-install-package-dialog.png)

Uppdatera requirements.txt genom att högerklicka på den virtuella miljön och välja **Generate requirements.txt** (Skapa requirements.txt).

Checka sedan in ändringarna i requirements.txt på Git-lagringsplatsen.

### <a name="deploy-to-azure"></a>Distribuera till Azure
Utlös en distribution genom att klicka på **Sync** (Synkronisera) eller **Push** (Pusha). Med synkronisering görs både en överföring och en hämtning.

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-git-push.png)

Den första distributionen tar en stund i och med att en virtuell miljö skapas, paket installeras osv.

I Visual Studio visas inte förloppet för distributionen. Information om hur du granskar utdata finns i avsnittet [Felsökning – distribution](#troubleshooting-deployment).

Visa ändringarna genom att gå till Azure-webbadressen.

## <a name="web-app-development---windows---command-line"></a>Webbappsutvecklingen - Windows - kommandot rad
### <a name="clone-the-repository"></a>Klona lagringsplatsen
Först klonar du lagringsplatsen med hjälp av webbadressen som tillhandahålls på Azure Portal. Lägg till Azure-lagringsplatsen som fjärransluten. Mer information finns i [Lokal Git-distribution till Azure App Service](app-service-deploy-local-git.md).

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url> 

### <a name="create-virtual-environment"></a>Skapa en virtuell miljö
Vi ska skapa en ny virtuell miljö för utveckling (lägg inte till den på lagringsplatsen). Virtuella miljöer i Python är inte relokerbara. Därför måste alla utvecklare som arbetar med programmet skapa sina egna lokalt.

Se till att använda samma version av Python som valts för webbappen (i runtime.txt eller i bladet programinställningar för webbappen i Azure Portal)

För Python 2.7:

    c:\python27\python.exe -m virtualenv env

För Python 3.4:

    c:\python34\python.exe -m venv env

Installera eventuella externa paket som krävs för programmet. Du kan använda requirements.txt filen i lagringsplatsens rot till att installera paket i den virtuella miljön:

    env\scripts\pip install -r requirements.txt

### <a name="run-using-development-server"></a>Kör med utvecklingsservern
Du kan starta programmet under en utvecklingsserver med följande kommando:

    env\scripts\python app.py

Konsolen visar webbadressen och porten servern tar emot kommunikation från:

![](./media/web-sites-python-create-deploy-bottle-app/windows-run-local-bottle.png)

Sedan öppnar du webbadressen i webbläsaren.

![](./media/web-sites-python-create-deploy-bottle-app/windows-browser-bottle.png)

### <a name="make-changes"></a>Göra ändringar
Nu kan du experimentera med att göra ändringar i programmets källor och mallar.

När du har testat ändringarna checkar du in dem på Git-lagringsplatsen:

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a>Installera fler paket
Programmet kan ha beroenden utöver Python och Bottle.

Du kan installera ytterligare paket med hjälp av pip. Om du till exempel vill installera Azure SDK för Python, som ger dig tillgång till Azure Storage, Service Bus och andra Azure-tjänster, anger du:

    env\scripts\pip install azure

Se till att uppdatera requirements.txt:

    env\scripts\pip freeze > requirements.txt

Checka in ändringarna:

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-to-azure"></a>Distribuera till Azure
Utlös en distribution genom att push-överföra ändringarna till Azure:

    git push azure master

Nu visas utdata från skriptet för distribution, inklusive skapandet av en virtuell miljö, installation av paket och skapande av web.config.

Visa ändringarna genom att gå till Azure-webbadressen.

## <a name="web-app-development---maclinux---command-line"></a>Webbappsutveckling – Mac/Linux – kommandorad
### <a name="clone-the-repository"></a>Klona lagringsplatsen
Först klonar du lagringsplatsen med hjälp av webbadressen som tillhandahålls på Azure Portal. Lägg till Azure-lagringsplatsen som fjärransluten. Mer information finns i [Lokal Git-distribution till Azure App Service](app-service-deploy-local-git.md).

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url> 

### <a name="create-virtual-environment"></a>Skapa en virtuell miljö
Vi ska skapa en ny virtuell miljö för utveckling (lägg inte till den på lagringsplatsen). Virtuella miljöer i Python är inte relokerbara. Därför måste alla utvecklare som arbetar med programmet skapa sina egna lokalt.

Se till att använda samma version av Python som valts för webbappen (i runtime.txt eller i bladet Programinställningar för webbappen i Azure Portal).

För Python 2.7:

    python -m virtualenv env

För Python 3.4:

    python -m venv env
eller pyvenv env

Installera eventuella externa paket som krävs för programmet. Du kan använda requirements.txt filen i lagringsplatsens rot till att installera paket i den virtuella miljön:

    env/bin/pip install -r requirements.txt

### <a name="run-using-development-server"></a>Kör med utvecklingsservern
Du kan starta programmet under en utvecklingsserver med följande kommando:

    env/bin/python app.py

Konsolen visar webbadressen och porten servern tar emot kommunikation från:

![](./media/web-sites-python-create-deploy-bottle-app/mac-run-local-bottle.png)

Sedan öppnar du webbadressen i webbläsaren.

![](./media/web-sites-python-create-deploy-bottle-app/mac-browser-bottle.png)

### <a name="make-changes"></a>Göra ändringar
Nu kan du experimentera med att göra ändringar i programmets källor och mallar.

När du har testat ändringarna checkar du in dem på Git-lagringsplatsen:

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a>Installera fler paket
Programmet kan ha beroenden utöver Python och Bottle.

Du kan installera ytterligare paket med hjälp av pip. Om du till exempel vill installera Azure SDK för Python, som ger dig tillgång till Azure Storage, Service Bus och andra Azure-tjänster, anger du:

    env/bin/pip install azure

Se till att uppdatera requirements.txt:

    env/bin/pip freeze > requirements.txt

Checka in ändringarna:

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-to-azure"></a>Distribuera till Azure
Utlös en distribution genom att push-överföra ändringarna till Azure:

    git push azure master

Nu visas utdata från skriptet för distribution, inklusive skapandet av en virtuell miljö, installation av paket och skapande av web.config.

Visa ändringarna genom att gå till Azure-webbadressen.

## <a name="troubleshooting---package-installation"></a>Felsökning – installation av paket
[!INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]

## <a name="troubleshooting---virtual-environment"></a>Felsökning – virtuell miljö
[!INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]

## <a name="next-steps"></a>Nästa steg
Du kan följa dessa länkar om du vill veta mer om Bottle och Python Tools för Visual Studio: 

* [Bottle dokumentation]
* [Dokumentationen om Python Tools för Visual Studio]

Information om hur du använder Azure Table Storage och MongoDB:

* [Bottle och MongoDB på Azure med Python Tools för Visual Studio]
* [Bottle och Azure Table Storage på Azure med Python Tools för Visual Studio]

## <a name="whats-changed"></a>Nyheter
* En guide till övergången från Webbplatser till App Service finns i: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)

<!--Link references-->
[Bottle och MongoDB på Azure med Python Tools för Visual Studio]: web-sites-python-ptvs-bottle-table-storage.md
[Bottle och Azure Table Storage på Azure med Python Tools för Visual Studio]: web-sites-python-ptvs-bottle-table-storage.md

<!--External Link references-->
[Azure SDK för Python 2.7]: http://go.microsoft.com/fwlink/?linkid=254281
[Azure SDK för Python 3.4]: http://go.microsoft.com/fwlink/?linkid=516990
[python.org]: http://www.python.org/
[Git för Windows]: http://msysgit.github.io/
[GitHub för Windows]: https://windows.github.com/
[Python Tools för Visual Studio]: http://aka.ms/ptvs
[Python Tools 2.2 för Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025
[Visual Studio]: http://www.visualstudio.com/
[Dokumentationen om Python Tools för Visual Studio]: http://aka.ms/ptvsdocs 
[Bottle dokumentation]: http://bottlepy.org/docs/dev/index.html

