---
title: aaaCreating web apps med Flask i Azure
description: "En självstudiekurs som presenteras toorunning en Python-webbapp på Azure."
services: app-service\web
documentationcenter: python
tags: python
author: huguesv
manager: erikre
editor: 
ms.assetid: b7f4ca3a-0b52-4108-90a7-f7e07ac73da0
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 02/20/2016
ms.author: huvalo
ms.openlocfilehash: 3362047b3106b4380b5971e47cbd8042d38a792b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="creating-web-apps-with-flask-in-azure"></a><span data-ttu-id="34335-103">Skapa webbappar med Flask i Azure</span><span class="sxs-lookup"><span data-stu-id="34335-103">Creating web apps with Flask in Azure</span></span>
<span data-ttu-id="34335-104">Den här självstudiekursen beskriver hur tooget igång med Python [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="34335-104">This tutorial describes how tooget started running Python in [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span>  <span data-ttu-id="34335-105">Med Web Apps får du begränsat ledigt värdutrymme och snabb distribution och kan använda Python.</span><span class="sxs-lookup"><span data-stu-id="34335-105">Web Apps provides limited free hosting and rapid deployment, and you can use Python!</span></span>  <span data-ttu-id="34335-106">När din app växer, kan du växla toopaid värd och du kan också integrera med alla hello andra Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="34335-106">As your app grows, you can switch toopaid hosting, and you can also integrate with all of hello other Azure services.</span></span>

<span data-ttu-id="34335-107">Du skapar ett program med hello Flask webbramverk (se motsvarande versioner av kursen för [Django](web-sites-python-create-deploy-django-app.md) och [Bottle](web-sites-python-create-deploy-bottle-app.md)).</span><span class="sxs-lookup"><span data-stu-id="34335-107">You will create an application using hello Flask web framework (see alternate versions of this tutorial for [Django](web-sites-python-create-deploy-django-app.md) and [Bottle](web-sites-python-create-deploy-bottle-app.md)).</span></span>  <span data-ttu-id="34335-108">Du ska skapa hello webbplats från hello Azure-galleriet, konfigurera Git-distribution och klona hello lagringsplatsen lokalt.</span><span class="sxs-lookup"><span data-stu-id="34335-108">You will create hello website from hello Azure gallery, set up Git deployment, and clone hello repository locally.</span></span>  <span data-ttu-id="34335-109">Du kommer sedan köra hello programmet lokalt, gör ändringar, genomför och push-installera dem tooAzure.</span><span class="sxs-lookup"><span data-stu-id="34335-109">Then you will run hello application locally, make changes, commit and push them tooAzure.</span></span>  <span data-ttu-id="34335-110">Hej självstudiekurs visar hur toodo från Windows eller Mac/Linux.</span><span class="sxs-lookup"><span data-stu-id="34335-110">hello tutorial shows how toodo this from Windows or Mac/Linux.</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> <span data-ttu-id="34335-111">Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service.</span><span class="sxs-lookup"><span data-stu-id="34335-111">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="34335-112">Inget kreditkort krävs, och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="34335-112">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="34335-113">Krav</span><span class="sxs-lookup"><span data-stu-id="34335-113">Prerequisites</span></span>
* <span data-ttu-id="34335-114">Windows, Mac eller Linux</span><span class="sxs-lookup"><span data-stu-id="34335-114">Windows, Mac or Linux</span></span>
* <span data-ttu-id="34335-115">Python 2.7 eller 3.4</span><span class="sxs-lookup"><span data-stu-id="34335-115">Python 2.7 or 3.4</span></span>
* <span data-ttu-id="34335-116">installationsverktyg, pip, virtuell miljö (endast Python 2.7)</span><span class="sxs-lookup"><span data-stu-id="34335-116">setuptools, pip, virtualenv (Python 2.7 only)</span></span>
* <span data-ttu-id="34335-117">Git</span><span class="sxs-lookup"><span data-stu-id="34335-117">Git</span></span>
* <span data-ttu-id="34335-118">[Python Tools för Visual Studio][Python Tools för Visual Studio] (PTVS) - Obs! det här är valfritt</span><span class="sxs-lookup"><span data-stu-id="34335-118">[Python Tools for Visual Studio][Python Tools for Visual Studio] (PTVS) - Note: this is optional</span></span>

<span data-ttu-id="34335-119">**Obs**! TFS-publicering stöds för närvarande inte för Python-projekt.</span><span class="sxs-lookup"><span data-stu-id="34335-119">**Note**: TFS publishing is currently not supported for Python projects.</span></span>

### <a name="windows"></a><span data-ttu-id="34335-120">Windows</span><span class="sxs-lookup"><span data-stu-id="34335-120">Windows</span></span>
<span data-ttu-id="34335-121">Om du inte redan har Python 2.7 eller 3.4 installerat (32-bitars) rekommenderar vi att du installerar [Azure SDK för Python 2.7] eller [Azure SDK för Python 3.4] med hjälp av installationsprogrammet för webbplattform.</span><span class="sxs-lookup"><span data-stu-id="34335-121">If you don't already have Python 2.7 or 3.4 installed (32-bit), we recommend installing [Azure SDK for Python 2.7] or [Azure SDK for Python 3.4] using Web Platform Installer.</span></span>  <span data-ttu-id="34335-122">Detta installerar hello 32-bitarsversionen av Python, installationsverktyg, pip, virtuell miljö och så vidare (32-bitarsversionen av Python är vad som är installerat på hello Azure-värddatorerna).</span><span class="sxs-lookup"><span data-stu-id="34335-122">This installs hello 32-bit version of Python, setuptools, pip, virtualenv, etc (32-bit Python is what's installed on hello Azure host machines).</span></span>  <span data-ttu-id="34335-123">Du kan också hämta Python på [python.org].</span><span class="sxs-lookup"><span data-stu-id="34335-123">Alternatively, you can get Python from [python.org].</span></span>

<span data-ttu-id="34335-124">För Git rekommenderar vi [Git för Windows] eller [GitHub för Windows].</span><span class="sxs-lookup"><span data-stu-id="34335-124">For Git, we recommend [Git for Windows] or [GitHub for Windows].</span></span>  <span data-ttu-id="34335-125">Om du använder Visual Studio, kan du använda hello integrerad Git-stödet.</span><span class="sxs-lookup"><span data-stu-id="34335-125">If you use Visual Studio, you can use hello integrated Git support.</span></span>

<span data-ttu-id="34335-126">Vi rekommenderar också att du installerar [Python Tools 2.2 för Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="34335-126">We also recommend installing [Python Tools 2.2 for Visual Studio].</span></span>  <span data-ttu-id="34335-127">Det här är valfritt, men om du har [Visual Studio], inklusive hello kostnadsfri Visual Studio Community 2013 eller Visual Studio Express 2013 för webben och det ger dig en utmärkt Python IDE.</span><span class="sxs-lookup"><span data-stu-id="34335-127">This is optional, but if you have [Visual Studio], including hello free Visual Studio Community 2013 or Visual Studio Express 2013 for Web, then this will give you a great Python IDE.</span></span>

### <a name="maclinux"></a><span data-ttu-id="34335-128">Mac/Linux</span><span class="sxs-lookup"><span data-stu-id="34335-128">Mac/Linux</span></span>
<span data-ttu-id="34335-129">Du bör redan ha Python och Git installerade, men kontrollera att du har Python 2.7 eller 3.4.</span><span class="sxs-lookup"><span data-stu-id="34335-129">You should have Python and Git already installed, but make sure you have either Python 2.7 or 3.4.</span></span>

## <a name="web-app-create-on-hello-azure-portal"></a><span data-ttu-id="34335-130">Webbprogrammet skapa på hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="34335-130">Web app create on hello Azure Portal</span></span>
<span data-ttu-id="34335-131">hello första steget i att skapa din app är toocreate hello webbprogram via hello [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="34335-131">hello first step in creating your app is toocreate hello web app via hello [Azure Portal](https://portal.azure.com).</span></span> 

1. <span data-ttu-id="34335-132">Logga in på hello Azure-portalen och klicka på hello **ny** knappen i hello nedre vänstra hörnet.</span><span class="sxs-lookup"><span data-stu-id="34335-132">Log into hello Azure Portal and click hello **NEW** button in hello bottom left corner.</span></span> 
2. <span data-ttu-id="34335-133">Klicka på **Webb + mobilt**.</span><span class="sxs-lookup"><span data-stu-id="34335-133">Click **Web + Mobile**.</span></span>
3. <span data-ttu-id="34335-134">Skriv ”python” i sökrutan hello.</span><span class="sxs-lookup"><span data-stu-id="34335-134">In hello search box, type "python".</span></span>
4. <span data-ttu-id="34335-135">I sökresultaten hello väljer **Flask**, klicka på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="34335-135">In hello search results, select **Flask**, then click **Create**.</span></span>
5. <span data-ttu-id="34335-136">Konfigurera hello nya Flask-appen, till exempel skapa en ny App Service-plan och en ny resursgrupp för den.</span><span class="sxs-lookup"><span data-stu-id="34335-136">Configure hello new Flask app, such as creating a new App Service plan and a new resource group for it.</span></span> <span data-ttu-id="34335-137">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="34335-137">Then, click **Create**.</span></span>
6. <span data-ttu-id="34335-138">Konfigurera Git-publicering för nyskapade webbappen genom att följa anvisningarna hello på [lokal Git-distribution tooAzure Apptjänst](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="34335-138">Configure Git publishing for your newly created web app by following hello instructions at [Local Git Deployment tooAzure App Service](app-service-deploy-local-git.md).</span></span>

## <a name="application-overview"></a><span data-ttu-id="34335-139">Programöversikt</span><span class="sxs-lookup"><span data-stu-id="34335-139">Application Overview</span></span>
### <a name="git-repository-contents"></a><span data-ttu-id="34335-140">Innehåll på Git-lagringsplatsen</span><span class="sxs-lookup"><span data-stu-id="34335-140">Git repository contents</span></span>
<span data-ttu-id="34335-141">Här är en översikt över hello-filer som du hittar i hello första Git-lagringsplatsen, som vi ska klona i nästa avsnitt om hello.</span><span class="sxs-lookup"><span data-stu-id="34335-141">Here's an overview of hello files you'll find in hello initial Git repository, which we'll clone in hello next section.</span></span>

    \FlaskWebProject\__init__.py
    \FlaskWebProject\views.py
    \FlaskWebProject\static\content\
    \FlaskWebProject\static\fonts\
    \FlaskWebProject\static\scripts\
    \FlaskWebProject\templates\about.html
    \FlaskWebProject\templates\contact.html
    \FlaskWebProject\templates\index.html
    \FlaskWebProject\templates\layout.html

<span data-ttu-id="34335-142">Programmet hello huvudsakliga källor.</span><span class="sxs-lookup"><span data-stu-id="34335-142">Main sources for hello application.</span></span>  <span data-ttu-id="34335-143">Består av tre sidor (index, om och kontakt) med bakgrundslayout.</span><span class="sxs-lookup"><span data-stu-id="34335-143">Consists of 3 pages (index, about, contact) with a master layout.</span></span>  <span data-ttu-id="34335-144">Statiskt innehåll och skript inkluderar bootstrap, jquery, modernizr och respond.</span><span class="sxs-lookup"><span data-stu-id="34335-144">Static content and scripts include bootstrap, jquery, modernizr and respond.</span></span>

    \runserver.py

<span data-ttu-id="34335-145">Stöd för lokal utveckling server.</span><span class="sxs-lookup"><span data-stu-id="34335-145">Local development server support.</span></span> <span data-ttu-id="34335-146">Använda toorun hello programmet lokalt.</span><span class="sxs-lookup"><span data-stu-id="34335-146">Use this toorun hello application locally.</span></span>

    \FlaskWebProject.pyproj
    \FlaskWebProject.sln

<span data-ttu-id="34335-147">Projektfiler för användning med [Python Tools för Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="34335-147">Project files for use with [Python Tools for Visual Studio].</span></span>

    \ptvs_virtualenv_proxy.py

<span data-ttu-id="34335-148">IIS-proxy för virtuella miljöer och PTVS-stöd för fjärrfelsökning.</span><span class="sxs-lookup"><span data-stu-id="34335-148">IIS proxy for virtual environments and PTVS remote debugging support.</span></span>

    \requirements.txt

<span data-ttu-id="34335-149">Externa paket som krävs för programmet.</span><span class="sxs-lookup"><span data-stu-id="34335-149">External packages needed by this application.</span></span> <span data-ttu-id="34335-150">hello distributionsskriptet pip hello installationspaket som anges i den här filen.</span><span class="sxs-lookup"><span data-stu-id="34335-150">hello deployment script will pip install hello packages listed in this file.</span></span>

    \web.2.7.config
    \web.3.4.config

<span data-ttu-id="34335-151">IIS-konfigurationsfiler.</span><span class="sxs-lookup"><span data-stu-id="34335-151">IIS configuration files.</span></span>  <span data-ttu-id="34335-152">hello distributionsskriptet använder lämplig web.x.y.config för hello och kopian web.config.</span><span class="sxs-lookup"><span data-stu-id="34335-152">hello deployment script will use hello appropriate web.x.y.config and copy it as web.config.</span></span>

### <a name="optional-files---customizing-deployment"></a><span data-ttu-id="34335-153">Valfria filer – anpassa distributionen</span><span class="sxs-lookup"><span data-stu-id="34335-153">Optional files - Customizing deployment</span></span>
[!INCLUDE [web-sites-python-customizing-deployment](../../includes/web-sites-python-customizing-deployment.md)]

### <a name="optional-files---python-runtime"></a><span data-ttu-id="34335-154">Valfria filer – Python-körning</span><span class="sxs-lookup"><span data-stu-id="34335-154">Optional files - Python runtime</span></span>
[!INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

### <a name="additional-files-on-server"></a><span data-ttu-id="34335-155">Ytterligare filer på servern</span><span class="sxs-lookup"><span data-stu-id="34335-155">Additional files on server</span></span>
<span data-ttu-id="34335-156">Vissa filer finnas på hello-servern men läggs inte toohello git-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="34335-156">Some files exist on hello server but are not added toohello git repository.</span></span>  <span data-ttu-id="34335-157">Dessa skapas med hello distributionsskriptet.</span><span class="sxs-lookup"><span data-stu-id="34335-157">These are created by hello deployment script.</span></span>

    \web.config

<span data-ttu-id="34335-158">IIS-konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="34335-158">IIS configuration file.</span></span>  <span data-ttu-id="34335-159">Skapas utifrån web.x.y.config vid varje distribution.</span><span class="sxs-lookup"><span data-stu-id="34335-159">Created from web.x.y.config on every deployment.</span></span>

    \env\

<span data-ttu-id="34335-160">Python, virtuell miljö</span><span class="sxs-lookup"><span data-stu-id="34335-160">Python virtual environment.</span></span>  <span data-ttu-id="34335-161">Skapas under distributionen om det inte redan finns en kompatibel virtuell miljö hello app.</span><span class="sxs-lookup"><span data-stu-id="34335-161">Created during deployment if a compatible virtual environment doesn't already exist on hello app.</span></span>  <span data-ttu-id="34335-162">Paket som anges i requirements.txt är pip-installeras, men pip hoppar över installationen om paketen hello redan är installerade.</span><span class="sxs-lookup"><span data-stu-id="34335-162">Packages listed in requirements.txt are pip installed, but pip will skip installation if hello packages are already installed.</span></span>

<span data-ttu-id="34335-163">hello 3 nästa avsnitt beskrivs hur tooproceed med hello webbappsutveckling under 3 olika miljöer:</span><span class="sxs-lookup"><span data-stu-id="34335-163">hello next 3 sections describe how tooproceed with hello web app development under 3 different environments:</span></span>

* <span data-ttu-id="34335-164">Windows med Python Tools för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="34335-164">Windows, with Python Tools for Visual Studio</span></span>
* <span data-ttu-id="34335-165">Windows, med kommandorad</span><span class="sxs-lookup"><span data-stu-id="34335-165">Windows, with command line</span></span>
* <span data-ttu-id="34335-166">Mac/Linux, med kommandorad</span><span class="sxs-lookup"><span data-stu-id="34335-166">Mac/Linux, with command line</span></span>

## <a name="web-app-development---windows---python-tools-for-visual-studio"></a><span data-ttu-id="34335-167">Webbappsutveckling – Windows – Python Tools för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="34335-167">Web app development - Windows - Python Tools for Visual Studio</span></span>
### <a name="clone-hello-repository"></a><span data-ttu-id="34335-168">Klona hello databasen</span><span class="sxs-lookup"><span data-stu-id="34335-168">Clone hello repository</span></span>
<span data-ttu-id="34335-169">Först klonar hello-databas med hjälp av hello Webbadressen som tillhandahålls på hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="34335-169">First, clone hello repository using hello URL provided on hello Azure Portal.</span></span> <span data-ttu-id="34335-170">Mer information finns i [lokal Git-distribution tooAzure Apptjänst](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="34335-170">For more information, see [Local Git Deployment tooAzure App Service](app-service-deploy-local-git.md).</span></span>

<span data-ttu-id="34335-171">Öppna hello lösningsfilen (.sln) som ingår i hello rot hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="34335-171">Open hello solution file (.sln) that is included in hello root of hello repository.</span></span>

![](./media/web-sites-python-create-deploy-flask-app/ptvs-solution-flask.png)

### <a name="create-virtual-environment"></a><span data-ttu-id="34335-172">Skapa en virtuell miljö</span><span class="sxs-lookup"><span data-stu-id="34335-172">Create virtual environment</span></span>
<span data-ttu-id="34335-173">Nu ska vi skapa en virtuell miljö för lokal utveckling.</span><span class="sxs-lookup"><span data-stu-id="34335-173">Now we'll create a virtual environment for local development.</span></span>  <span data-ttu-id="34335-174">Högerklicka på **Python Environments** (Python-miljöer) och välj **Add Virtual Environment...** (Lägg till virtuell miljö).</span><span class="sxs-lookup"><span data-stu-id="34335-174">Right-click on **Python Environments** select **Add Virtual Environment...**.</span></span>

* <span data-ttu-id="34335-175">Kontrollera hello hello miljö heter `env`.</span><span class="sxs-lookup"><span data-stu-id="34335-175">Make sure hello name of hello environment is `env`.</span></span>
* <span data-ttu-id="34335-176">Välj hello bastolk.</span><span class="sxs-lookup"><span data-stu-id="34335-176">Select hello base interpreter.</span></span>  <span data-ttu-id="34335-177">Kontrollera att toouse hello samma version av Python som valts för webbappen (i runtime.txt eller hello **programinställningar** bladet för din webbapp i hello Azure Portal).</span><span class="sxs-lookup"><span data-stu-id="34335-177">Make sure toouse hello same version of Python that is selected for your web app (in runtime.txt or hello **Application Settings** blade of your web app in hello Azure Portal).</span></span>
* <span data-ttu-id="34335-178">Kontrollera att hello alternativet toodownload och installera paket är markerat.</span><span class="sxs-lookup"><span data-stu-id="34335-178">Make sure hello option toodownload and install packages is checked.</span></span>

![](./media/web-sites-python-create-deploy-flask-app/ptvs-add-virtual-env-27.png)

<span data-ttu-id="34335-179">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="34335-179">Click **Create**.</span></span>  <span data-ttu-id="34335-180">Detta skapar hello virtuell miljö och installera beroenden som anges i requirements.txt.</span><span class="sxs-lookup"><span data-stu-id="34335-180">This will create hello virtual environment, and install dependencies listed in requirements.txt.</span></span>

### <a name="run-using-development-server"></a><span data-ttu-id="34335-181">Kör med utvecklingsservern</span><span class="sxs-lookup"><span data-stu-id="34335-181">Run using development server</span></span>
<span data-ttu-id="34335-182">Tryck på F5 toostart felsökning och webbläsaren öppnas automatiskt toohello sidan som körs lokalt.</span><span class="sxs-lookup"><span data-stu-id="34335-182">Press F5 toostart debugging, and your web browser will open automatically toohello page running locally.</span></span>

![](./media/web-sites-python-create-deploy-flask-app/windows-browser-flask.png)

<span data-ttu-id="34335-183">Du kan ange brytpunkter i hello källor, Använd hello titta på windows osv.  Se hello [Python Tools för Visual Studio-dokumentationen] hello mer information om olika funktioner.</span><span class="sxs-lookup"><span data-stu-id="34335-183">You can set breakpoints in hello sources, use hello watch windows, etc.  See hello [Python Tools for Visual Studio Documentation] for more information on hello various features.</span></span>

### <a name="make-changes"></a><span data-ttu-id="34335-184">Göra ändringar</span><span class="sxs-lookup"><span data-stu-id="34335-184">Make changes</span></span>
<span data-ttu-id="34335-185">Nu kan du experimentera med att göra ändringar toohello programmets källor och mallar.</span><span class="sxs-lookup"><span data-stu-id="34335-185">Now you can experiment by making changes toohello application sources and/or templates.</span></span>

<span data-ttu-id="34335-186">När du har testat ändringarna checkar du in dem toohello Git-lagringsplatsen:</span><span class="sxs-lookup"><span data-stu-id="34335-186">After you've tested your changes, commit them toohello Git repository:</span></span>

![](./media/web-sites-python-create-deploy-flask-app/ptvs-commit-flask.png)

### <a name="install-more-packages"></a><span data-ttu-id="34335-187">Installera fler paket</span><span class="sxs-lookup"><span data-stu-id="34335-187">Install more packages</span></span>
<span data-ttu-id="34335-188">Programmet kan ha beroenden utöver Python och Flask.</span><span class="sxs-lookup"><span data-stu-id="34335-188">Your application may have dependencies beyond Python and Flask.</span></span>

<span data-ttu-id="34335-189">Du kan installera ytterligare paket med hjälp av pip.</span><span class="sxs-lookup"><span data-stu-id="34335-189">You can install additional packages using pip.</span></span>  <span data-ttu-id="34335-190">tooinstall ett paket, högerklicka på hello virtuell miljö och välj **Install Python Package**.</span><span class="sxs-lookup"><span data-stu-id="34335-190">tooinstall a package, right-click on hello virtual environment and select **Install Python Package**.</span></span>

<span data-ttu-id="34335-191">Till exempel tooinstall hello Azure SDK för Python, som ger dig åtkomst tooAzure storage, service bus och andra Azure-tjänster, ange `azure`:</span><span class="sxs-lookup"><span data-stu-id="34335-191">For example, tooinstall hello Azure SDK for Python, which gives you access tooAzure storage, service bus and other Azure services, enter `azure`:</span></span>

![](./media/web-sites-python-create-deploy-flask-app/ptvs-install-package-dialog.png)

<span data-ttu-id="34335-192">Högerklicka på hello virtuell miljö och välj **generera requirements.txt** tooupdate requirements.txt.</span><span class="sxs-lookup"><span data-stu-id="34335-192">Right-click on hello virtual environment and select **Generate requirements.txt** tooupdate requirements.txt.</span></span>

<span data-ttu-id="34335-193">Checka sedan in hello ändringar toorequirements.txt toohello Git-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="34335-193">Then, commit hello changes toorequirements.txt toohello Git repository.</span></span>

### <a name="deploy-tooazure"></a><span data-ttu-id="34335-194">Distribuera tooAzure</span><span class="sxs-lookup"><span data-stu-id="34335-194">Deploy tooAzure</span></span>
<span data-ttu-id="34335-195">tootrigger en distribution, klicka på **Sync** eller **Push**.</span><span class="sxs-lookup"><span data-stu-id="34335-195">tootrigger a deployment, click on **Sync** or **Push**.</span></span>  <span data-ttu-id="34335-196">Med synkronisering görs både en överföring och en hämtning.</span><span class="sxs-lookup"><span data-stu-id="34335-196">Sync does both a push and a pull.</span></span>

![](./media/web-sites-python-create-deploy-flask-app/ptvs-git-push.png)

<span data-ttu-id="34335-197">hello första distributionen tar en stund, eftersom den skapar en virtuell miljö, paket installeras osv.</span><span class="sxs-lookup"><span data-stu-id="34335-197">hello first deployment will take some time, as it will create a virtual environment, install packages, etc.</span></span>

<span data-ttu-id="34335-198">Visual Studio visas inte hello fortskrider hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="34335-198">Visual Studio doesn't show hello progress of hello deployment.</span></span>  <span data-ttu-id="34335-199">Om du vill att tooreview hello utdata avsnittet hello på [felsökning – distribution](#troubleshooting-deployment).</span><span class="sxs-lookup"><span data-stu-id="34335-199">If you'd like tooreview hello output, see hello section on [Troubleshooting - Deployment](#troubleshooting-deployment).</span></span>

<span data-ttu-id="34335-200">Bläddra toohello Azure URL tooview ändringarna.</span><span class="sxs-lookup"><span data-stu-id="34335-200">Browse toohello Azure URL tooview your changes.</span></span>

## <a name="web-app-development---windows---command-line"></a><span data-ttu-id="34335-201">Webbappsutveckling – Windows – kommandorad</span><span class="sxs-lookup"><span data-stu-id="34335-201">Web app development - Windows - command line</span></span>
### <a name="clone-hello-repository"></a><span data-ttu-id="34335-202">Klona hello databasen</span><span class="sxs-lookup"><span data-stu-id="34335-202">Clone hello repository</span></span>
<span data-ttu-id="34335-203">Först klonar hello lagringsplatsen med hjälp av hello Webbadressen som tillhandahålls på hello Azure-portalen och lägga till hello Azure-lagringsplatsen som fjärransluten.</span><span class="sxs-lookup"><span data-stu-id="34335-203">First, clone hello repository using hello URL provided on hello Azure Portal, and add hello Azure repository as a remote.</span></span> <span data-ttu-id="34335-204">Mer information finns i [lokal Git-distribution tooAzure Apptjänst](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="34335-204">For more information, see [Local Git Deployment tooAzure App Service](app-service-deploy-local-git.md).</span></span>

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url> 

### <a name="create-virtual-environment"></a><span data-ttu-id="34335-205">Skapa en virtuell miljö</span><span class="sxs-lookup"><span data-stu-id="34335-205">Create virtual environment</span></span>
<span data-ttu-id="34335-206">Vi ska skapa en ny virtuell miljö för utveckling (inte Lägg till den toohello databasen).</span><span class="sxs-lookup"><span data-stu-id="34335-206">We'll create a new virtual environment for development purposes (do not add it toohello repository).</span></span>  <span data-ttu-id="34335-207">Virtuella miljöer i Python är inte relokerbara, så alla utvecklare som arbetar på hello programmet skapa sina egna lokalt.</span><span class="sxs-lookup"><span data-stu-id="34335-207">Virtual environments in Python are not relocatable, so every developer working on hello application will create their own locally.</span></span>

<span data-ttu-id="34335-208">Kontrollera att toouse hello samma version av Python som valts för webbappen (i runtime.txt eller hello **programinställningar** bladet för din webbapp i hello Azure Portal).</span><span class="sxs-lookup"><span data-stu-id="34335-208">Make sure toouse hello same version of Python that is selected for your web app (in runtime.txt or hello **Application Settings** blade of your web app in hello Azure Portal).</span></span>

<span data-ttu-id="34335-209">För Python 2.7:</span><span class="sxs-lookup"><span data-stu-id="34335-209">For Python 2.7:</span></span>

    c:\python27\python.exe -m virtualenv env

<span data-ttu-id="34335-210">För Python 3.4:</span><span class="sxs-lookup"><span data-stu-id="34335-210">For Python 3.4:</span></span>

    c:\python34\python.exe -m venv env

<span data-ttu-id="34335-211">Installera eventuella externa paket som krävs för programmet.</span><span class="sxs-lookup"><span data-stu-id="34335-211">Install any external packages required by your application.</span></span> <span data-ttu-id="34335-212">Du kan använda hello requirements.txt filen i hello rot hello databasen tooinstall hello paket i den virtuella miljön:</span><span class="sxs-lookup"><span data-stu-id="34335-212">You can use hello requirements.txt file at hello root of hello repository tooinstall hello packages in your virtual environment:</span></span>

    env\scripts\pip install -r requirements.txt

### <a name="run-using-development-server"></a><span data-ttu-id="34335-213">Kör med utvecklingsservern</span><span class="sxs-lookup"><span data-stu-id="34335-213">Run using development server</span></span>
<span data-ttu-id="34335-214">Du kan starta programmet hello under en utvecklingsserver med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="34335-214">You can launch hello application under a development server with hello following command:</span></span>

    env\scripts\python runserver.py

<span data-ttu-id="34335-215">hello konsolen visar hello URL-adress och port hello servern lyssnar på:</span><span class="sxs-lookup"><span data-stu-id="34335-215">hello console will display hello URL and port hello server listens to:</span></span>

![](./media/web-sites-python-create-deploy-flask-app/windows-run-local-flask.png)

<span data-ttu-id="34335-216">Öppna din Webbadress webbläsare toothat.</span><span class="sxs-lookup"><span data-stu-id="34335-216">Then, open your web browser toothat URL.</span></span>

![](./media/web-sites-python-create-deploy-flask-app/windows-browser-flask.png)

### <a name="make-changes"></a><span data-ttu-id="34335-217">Göra ändringar</span><span class="sxs-lookup"><span data-stu-id="34335-217">Make changes</span></span>
<span data-ttu-id="34335-218">Nu kan du experimentera med att göra ändringar toohello programmets källor och mallar.</span><span class="sxs-lookup"><span data-stu-id="34335-218">Now you can experiment by making changes toohello application sources and/or templates.</span></span>

<span data-ttu-id="34335-219">När du har testat ändringarna checkar du in dem toohello Git-lagringsplatsen:</span><span class="sxs-lookup"><span data-stu-id="34335-219">After you've tested your changes, commit them toohello Git repository:</span></span>

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a><span data-ttu-id="34335-220">Installera fler paket</span><span class="sxs-lookup"><span data-stu-id="34335-220">Install more packages</span></span>
<span data-ttu-id="34335-221">Programmet kan ha beroenden utöver Python och Flask.</span><span class="sxs-lookup"><span data-stu-id="34335-221">Your application may have dependencies beyond Python and Flask.</span></span>

<span data-ttu-id="34335-222">Du kan installera ytterligare paket med hjälp av pip.</span><span class="sxs-lookup"><span data-stu-id="34335-222">You can install additional packages using pip.</span></span>  <span data-ttu-id="34335-223">Till exempel tooinstall hello Azure SDK för Python, som ger dig åtkomst till tooAzure storage, service bus och andra Azure-tjänster, typ:</span><span class="sxs-lookup"><span data-stu-id="34335-223">For example, tooinstall hello Azure SDK for Python, which gives you access tooAzure storage, service bus and other Azure services, type:</span></span>

    env\scripts\pip install azure

<span data-ttu-id="34335-224">Se till att tooupdate requirements.txt:</span><span class="sxs-lookup"><span data-stu-id="34335-224">Make sure tooupdate requirements.txt:</span></span>

    env\scripts\pip freeze > requirements.txt

<span data-ttu-id="34335-225">Genomför hello ändringar:</span><span class="sxs-lookup"><span data-stu-id="34335-225">Commit hello changes:</span></span>

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-tooazure"></a><span data-ttu-id="34335-226">Distribuera tooAzure</span><span class="sxs-lookup"><span data-stu-id="34335-226">Deploy tooAzure</span></span>
<span data-ttu-id="34335-227">tootrigger en distribution push hello ändrar tooAzure:</span><span class="sxs-lookup"><span data-stu-id="34335-227">tootrigger a deployment, push hello changes tooAzure:</span></span>

    git push azure master

<span data-ttu-id="34335-228">Du ser hello utdata från hello distributionsskriptet, inklusive skapande av virtuell miljö, installation av paket och skapande av web.config.</span><span class="sxs-lookup"><span data-stu-id="34335-228">You will see hello output of hello deployment script, including virtual environment creation, installation of packages, creation of web.config.</span></span>

<span data-ttu-id="34335-229">Bläddra toohello Azure URL tooview ändringarna.</span><span class="sxs-lookup"><span data-stu-id="34335-229">Browse toohello Azure URL tooview your changes.</span></span>

## <a name="web-app-development---maclinux---command-line"></a><span data-ttu-id="34335-230">Webbappsutveckling – Mac/Linux – kommandorad</span><span class="sxs-lookup"><span data-stu-id="34335-230">Web app development - Mac/Linux - command line</span></span>
### <a name="clone-hello-repository"></a><span data-ttu-id="34335-231">Klona hello databasen</span><span class="sxs-lookup"><span data-stu-id="34335-231">Clone hello repository</span></span>
<span data-ttu-id="34335-232">Först klonar hello lagringsplatsen med hjälp av hello Webbadressen som tillhandahålls på hello Azure-portalen och lägga till hello Azure-lagringsplatsen som fjärransluten.</span><span class="sxs-lookup"><span data-stu-id="34335-232">First, clone hello repository using hello URL provided on hello Azure Portal, and add hello Azure repository as a remote.</span></span> <span data-ttu-id="34335-233">Mer information finns i [lokal Git-distribution tooAzure Apptjänst](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="34335-233">For more information, see [Local Git Deployment tooAzure App Service](app-service-deploy-local-git.md).</span></span>

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url> 

### <a name="create-virtual-environment"></a><span data-ttu-id="34335-234">Skapa en virtuell miljö</span><span class="sxs-lookup"><span data-stu-id="34335-234">Create virtual environment</span></span>
<span data-ttu-id="34335-235">Vi ska skapa en ny virtuell miljö för utveckling (inte Lägg till den toohello databasen).</span><span class="sxs-lookup"><span data-stu-id="34335-235">We'll create a new virtual environment for development purposes (do not add it toohello repository).</span></span>  <span data-ttu-id="34335-236">Virtuella miljöer i Python är inte relokerbara, så alla utvecklare som arbetar på hello programmet skapa sina egna lokalt.</span><span class="sxs-lookup"><span data-stu-id="34335-236">Virtual environments in Python are not relocatable, so every developer working on hello application will create their own locally.</span></span>

<span data-ttu-id="34335-237">Kontrollera att toouse hello samma version av Python som valts för webbappen (i runtime.txt eller hello **programinställningar** bladet för din webbapp i hello Azure Portal).</span><span class="sxs-lookup"><span data-stu-id="34335-237">Make sure toouse hello same version of Python that is selected for your web app (in runtime.txt or hello **Application Settings** blade of your web app in hello Azure Portal).</span></span>

<span data-ttu-id="34335-238">För Python 2.7:</span><span class="sxs-lookup"><span data-stu-id="34335-238">For Python 2.7:</span></span>

    python -m virtualenv env

<span data-ttu-id="34335-239">För Python 3.4:</span><span class="sxs-lookup"><span data-stu-id="34335-239">For Python 3.4:</span></span>

    python -m venv env
<span data-ttu-id="34335-240">eller pyvenv env</span><span class="sxs-lookup"><span data-stu-id="34335-240">or pyvenv env</span></span>

<span data-ttu-id="34335-241">Installera eventuella externa paket som krävs för programmet.</span><span class="sxs-lookup"><span data-stu-id="34335-241">Install any external packages required by your application.</span></span> <span data-ttu-id="34335-242">Du kan använda hello requirements.txt filen i hello rot hello databasen tooinstall hello paket i den virtuella miljön:</span><span class="sxs-lookup"><span data-stu-id="34335-242">You can use hello requirements.txt file at hello root of hello repository tooinstall hello packages in your virtual environment:</span></span>

    env/bin/pip install -r requirements.txt

### <a name="run-using-development-server"></a><span data-ttu-id="34335-243">Kör med utvecklingsservern</span><span class="sxs-lookup"><span data-stu-id="34335-243">Run using development server</span></span>
<span data-ttu-id="34335-244">Du kan starta programmet hello under en utvecklingsserver med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="34335-244">You can launch hello application under a development server with hello following command:</span></span>

    env/bin/python runserver.py

<span data-ttu-id="34335-245">hello konsolen visar hello URL-adress och port hello servern lyssnar på:</span><span class="sxs-lookup"><span data-stu-id="34335-245">hello console will display hello URL and port hello server listens to:</span></span>

![](./media/web-sites-python-create-deploy-flask-app/mac-run-local-flask.png)

<span data-ttu-id="34335-246">Öppna din Webbadress webbläsare toothat.</span><span class="sxs-lookup"><span data-stu-id="34335-246">Then, open your web browser toothat URL.</span></span>

![](./media/web-sites-python-create-deploy-flask-app/mac-browser-flask.png)

### <a name="make-changes"></a><span data-ttu-id="34335-247">Göra ändringar</span><span class="sxs-lookup"><span data-stu-id="34335-247">Make changes</span></span>
<span data-ttu-id="34335-248">Nu kan du experimentera med att göra ändringar toohello programmets källor och mallar.</span><span class="sxs-lookup"><span data-stu-id="34335-248">Now you can experiment by making changes toohello application sources and/or templates.</span></span>

<span data-ttu-id="34335-249">När du har testat ändringarna checkar du in dem toohello Git-lagringsplatsen:</span><span class="sxs-lookup"><span data-stu-id="34335-249">After you've tested your changes, commit them toohello Git repository:</span></span>

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a><span data-ttu-id="34335-250">Installera fler paket</span><span class="sxs-lookup"><span data-stu-id="34335-250">Install more packages</span></span>
<span data-ttu-id="34335-251">Programmet kan ha beroenden utöver Python och Flask.</span><span class="sxs-lookup"><span data-stu-id="34335-251">Your application may have dependencies beyond Python and Flask.</span></span>

<span data-ttu-id="34335-252">Du kan installera ytterligare paket med hjälp av pip.</span><span class="sxs-lookup"><span data-stu-id="34335-252">You can install additional packages using pip.</span></span>  <span data-ttu-id="34335-253">Till exempel tooinstall hello Azure SDK för Python, som ger dig åtkomst till tooAzure storage, service bus och andra Azure-tjänster, typ:</span><span class="sxs-lookup"><span data-stu-id="34335-253">For example, tooinstall hello Azure SDK for Python, which gives you access tooAzure storage, service bus and other Azure services, type:</span></span>

    env/bin/pip install azure

<span data-ttu-id="34335-254">Se till att tooupdate requirements.txt:</span><span class="sxs-lookup"><span data-stu-id="34335-254">Make sure tooupdate requirements.txt:</span></span>

    env/bin/pip freeze > requirements.txt

<span data-ttu-id="34335-255">Genomför hello ändringar:</span><span class="sxs-lookup"><span data-stu-id="34335-255">Commit hello changes:</span></span>

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-tooazure"></a><span data-ttu-id="34335-256">Distribuera tooAzure</span><span class="sxs-lookup"><span data-stu-id="34335-256">Deploy tooAzure</span></span>
<span data-ttu-id="34335-257">tootrigger en distribution push hello ändrar tooAzure:</span><span class="sxs-lookup"><span data-stu-id="34335-257">tootrigger a deployment, push hello changes tooAzure:</span></span>

    git push azure master

<span data-ttu-id="34335-258">Du ser hello utdata från hello distributionsskriptet, inklusive skapande av virtuell miljö, installation av paket och skapande av web.config.</span><span class="sxs-lookup"><span data-stu-id="34335-258">You will see hello output of hello deployment script, including virtual environment creation, installation of packages, creation of web.config.</span></span>

<span data-ttu-id="34335-259">Bläddra toohello Azure URL tooview ändringarna.</span><span class="sxs-lookup"><span data-stu-id="34335-259">Browse toohello Azure URL tooview your changes.</span></span>

## <a name="troubleshooting---package-installation"></a><span data-ttu-id="34335-260">Felsökning – installation av paket</span><span class="sxs-lookup"><span data-stu-id="34335-260">Troubleshooting - Package Installation</span></span>
[!INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]

## <a name="troubleshooting---virtual-environment"></a><span data-ttu-id="34335-261">Felsökning – virtuell miljö</span><span class="sxs-lookup"><span data-stu-id="34335-261">Troubleshooting - Virtual Environment</span></span>
[!INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]

## <a name="next-steps"></a><span data-ttu-id="34335-262">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="34335-262">Next Steps</span></span>
<span data-ttu-id="34335-263">Följ dessa länkar toolearn mer om Flask och Python Tools för Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="34335-263">Follow these links toolearn more about Flask and Python Tools for Visual Studio:</span></span> 

* <span data-ttu-id="34335-264">[Flask-dokumentation]</span><span class="sxs-lookup"><span data-stu-id="34335-264">[Flask Documentation]</span></span>
* <span data-ttu-id="34335-265">[Dokumentation för Python Tools för Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="34335-265">[Python Tools for Visual Studio Documentation]</span></span>

<span data-ttu-id="34335-266">Information om hur du använder Azure Table Storage och MongoDB:</span><span class="sxs-lookup"><span data-stu-id="34335-266">For information on using Azure Table Storage and MongoDB:</span></span>

* <span data-ttu-id="34335-267">[Flask och MongoDB på Azure med Python Tools för Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="34335-267">[Flask and MongoDB on Azure with Python Tools for Visual Studio]</span></span>
* <span data-ttu-id="34335-268">[Flask och Azure Table Storage på Azure med Python Tools för Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="34335-268">[Flask and Azure Table Storage on Azure with Python Tools for Visual Studio]</span></span>

<span data-ttu-id="34335-269">Mer information finns också hello [Python Developer Center](/develop/python/).</span><span class="sxs-lookup"><span data-stu-id="34335-269">For more information, see also hello [Python Developer Center](/develop/python/).</span></span>

## <a name="whats-changed"></a><span data-ttu-id="34335-270">Nyheter</span><span class="sxs-lookup"><span data-stu-id="34335-270">What's changed</span></span>
* <span data-ttu-id="34335-271">En guide toohello övergången från webbplatser tooApp tjänsten finns: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="34335-271">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

<!--Link references-->
[Flask och MongoDB på Azure med Python Tools för Visual Studio]: https://github.com/microsoft/ptvs/wiki/Flask-and-MongoDB-on-Azure
[Flask och Azure Table Storage på Azure med Python Tools för Visual Studio]: web-sites-python-ptvs-flask-table-storage.md

<!--External Link references-->
[Azure SDK för Python 2.7]: http://go.microsoft.com/fwlink/?linkid=254281
[Azure SDK för Python 3.4]: http://go.microsoft.com/fwlink/?linkid=516990
[python.org]: http://www.python.org/
[Git för Windows]: http://msysgit.github.io/
[GitHub för Windows]: https://windows.github.com/
[Python Tools för Visual Studio]: http://aka.ms/ptvs
[Python Tools 2.2 för Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025
[Visual Studio]: http://www.visualstudio.com/
[Dokumentation för Python Tools för Visual Studio]: http://aka.ms/ptvsdocs
[Flask-dokumentation]: http://flask.pocoo.org/ 

