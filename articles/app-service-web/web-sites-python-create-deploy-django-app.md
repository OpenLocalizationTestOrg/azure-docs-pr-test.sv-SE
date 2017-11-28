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
# <a name="creating-web-apps-with-django-in-azure"></a><span data-ttu-id="0faec-103">Skapa webbappar med Django i Azure</span><span class="sxs-lookup"><span data-stu-id="0faec-103">Creating web apps with Django in Azure</span></span>
<span data-ttu-id="0faec-104">Den här självstudiekursen beskriver hur tooget igång med Python i [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="0faec-104">This tutorial describes how tooget started running Python on [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span> <span data-ttu-id="0faec-105">Med Web Apps får du begränsat ledigt värdutrymme och snabb distribution och kan använda Python.</span><span class="sxs-lookup"><span data-stu-id="0faec-105">Web Apps provides limited free hosting and rapid deployment, and you can use Python!</span></span> <span data-ttu-id="0faec-106">När din app växer, kan du växla toopaid värd och du kan också integrera med alla hello andra Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="0faec-106">As your app grows, you can switch toopaid hosting, and you can also integrate with all of hello other Azure services.</span></span>

<span data-ttu-id="0faec-107">Du skapar ett program med hello Djangos webbramverk (se motsvarande versioner av kursen för [Flask](web-sites-python-create-deploy-flask-app.md) och [Bottle](web-sites-python-create-deploy-bottle-app.md)).</span><span class="sxs-lookup"><span data-stu-id="0faec-107">You will create an application using hello Django web framework (see alternate versions of this tutorial for [Flask](web-sites-python-create-deploy-flask-app.md) and [Bottle](web-sites-python-create-deploy-bottle-app.md)).</span></span> <span data-ttu-id="0faec-108">Du ska skapa hello webbprogrammet från hello Azure Marketplace, konfigurera Git-distribution och klona hello lagringsplatsen lokalt.</span><span class="sxs-lookup"><span data-stu-id="0faec-108">You will create hello web app from hello Azure Marketplace, set up Git deployment, and clone hello repository locally.</span></span> <span data-ttu-id="0faec-109">Du kommer sedan köra hello programmet lokalt, gör ändringar, genomför och push-installera dem tooAzure.</span><span class="sxs-lookup"><span data-stu-id="0faec-109">Then you will run hello application locally, make changes, commit and push them tooAzure.</span></span> <span data-ttu-id="0faec-110">Hej självstudiekurs visar hur toodo från Windows eller Mac/Linux.</span><span class="sxs-lookup"><span data-stu-id="0faec-110">hello tutorial shows how toodo this from Windows or Mac/Linux.</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> <span data-ttu-id="0faec-111">Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service.</span><span class="sxs-lookup"><span data-stu-id="0faec-111">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="0faec-112">Inget kreditkort krävs, och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="0faec-112">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="0faec-113">Krav</span><span class="sxs-lookup"><span data-stu-id="0faec-113">Prerequisites</span></span>
* <span data-ttu-id="0faec-114">Windows, Mac eller Linux</span><span class="sxs-lookup"><span data-stu-id="0faec-114">Windows, Mac or Linux</span></span>
* <span data-ttu-id="0faec-115">Python 2.7 eller 3.4</span><span class="sxs-lookup"><span data-stu-id="0faec-115">Python 2.7 or 3.4</span></span>
* <span data-ttu-id="0faec-116">installationsverktyg, pip, virtuell miljö (endast Python 2.7)</span><span class="sxs-lookup"><span data-stu-id="0faec-116">setuptools, pip, virtualenv (Python 2.7 only)</span></span>
* <span data-ttu-id="0faec-117">Git</span><span class="sxs-lookup"><span data-stu-id="0faec-117">Git</span></span>
* <span data-ttu-id="0faec-118">[Python Tools för Visual Studio][Python Tools för Visual Studio] (PTVS) - Obs! det här är valfritt</span><span class="sxs-lookup"><span data-stu-id="0faec-118">[Python Tools for Visual Studio][Python Tools for Visual Studio] (PTVS) - Note: this is optional</span></span>

<span data-ttu-id="0faec-119">**Obs**! TFS-publicering stöds för närvarande inte för Python-projekt.</span><span class="sxs-lookup"><span data-stu-id="0faec-119">**Note**: TFS publishing is currently not supported for Python projects.</span></span>

### <a name="windows"></a><span data-ttu-id="0faec-120">Windows</span><span class="sxs-lookup"><span data-stu-id="0faec-120">Windows</span></span>
<span data-ttu-id="0faec-121">Om du inte redan har Python 2.7 eller 3.4 installerat (32-bitars) rekommenderar vi att du installerar [Azure SDK för Python 2.7] eller [Azure SDK för Python 3.4] med hjälp av installationsprogrammet för webbplattform.</span><span class="sxs-lookup"><span data-stu-id="0faec-121">If you don't already have Python 2.7 or 3.4 installed (32-bit), we recommend installing [Azure SDK for Python 2.7] or [Azure SDK for Python 3.4] using Web Platform Installer.</span></span> <span data-ttu-id="0faec-122">Detta installerar hello 32-bitarsversionen av Python, installationsverktyg, pip, virtuell miljö och så vidare (32-bitarsversionen av Python är vad som är installerat på hello Azure-värddatorerna).</span><span class="sxs-lookup"><span data-stu-id="0faec-122">This installs hello 32-bit version of Python, setuptools, pip, virtualenv, etc (32-bit Python is what's installed on hello Azure host machines).</span></span> <span data-ttu-id="0faec-123">Du kan också hämta Python på [python.org].</span><span class="sxs-lookup"><span data-stu-id="0faec-123">Alternatively, you can get Python from [python.org].</span></span>

<span data-ttu-id="0faec-124">För Git rekommenderar vi [Git för Windows] eller [GitHub för Windows].</span><span class="sxs-lookup"><span data-stu-id="0faec-124">For Git, we recommend [Git for Windows] or [GitHub for Windows].</span></span> <span data-ttu-id="0faec-125">Om du använder Visual Studio, kan du använda hello integrerad Git-stödet.</span><span class="sxs-lookup"><span data-stu-id="0faec-125">If you use Visual Studio, you can use hello integrated Git support.</span></span>

<span data-ttu-id="0faec-126">Vi rekommenderar också att du installerar [Python Tools 2.2 för Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="0faec-126">We also recommend installing [Python Tools 2.2 for Visual Studio].</span></span> <span data-ttu-id="0faec-127">Det här är valfritt, men om du har [Visual Studio], inklusive hello kostnadsfri Visual Studio Community 2013 eller Visual Studio Express 2013 för webben och det ger dig en utmärkt Python IDE.</span><span class="sxs-lookup"><span data-stu-id="0faec-127">This is optional, but if you have [Visual Studio], including hello free Visual Studio Community 2013 or Visual Studio Express 2013 for Web, then this will give you a great Python IDE.</span></span>

### <a name="maclinux"></a><span data-ttu-id="0faec-128">Mac/Linux</span><span class="sxs-lookup"><span data-stu-id="0faec-128">Mac/Linux</span></span>
<span data-ttu-id="0faec-129">Du bör redan ha Python och Git installerade, men kontrollera att du har Python 2.7 eller 3.4.</span><span class="sxs-lookup"><span data-stu-id="0faec-129">You should have Python and Git already installed, but make sure you have either Python 2.7 or 3.4.</span></span>

## <a name="web-app-creation-on-portal"></a><span data-ttu-id="0faec-130">Skapa en webbapp i Azure Portal</span><span class="sxs-lookup"><span data-stu-id="0faec-130">Web App Creation on Portal</span></span>
<span data-ttu-id="0faec-131">hello första steget i att skapa din app är toocreate hello webbprogram via hello [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="0faec-131">hello first step in creating your app is toocreate hello web app via hello [Azure Portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="0faec-132">Logga in på hello Azure-portalen och klicka på hello **ny** knappen i hello nedre vänstra hörnet.</span><span class="sxs-lookup"><span data-stu-id="0faec-132">Log into hello Azure Portal and click hello **NEW** button in hello bottom left corner.</span></span>
2. <span data-ttu-id="0faec-133">Skriv ”python” i sökrutan hello.</span><span class="sxs-lookup"><span data-stu-id="0faec-133">In hello search box, type "python".</span></span>
3. <span data-ttu-id="0faec-134">I sökresultaten hello väljer **Django** (som publicerats av PTVS) Klicka **skapa**.</span><span class="sxs-lookup"><span data-stu-id="0faec-134">In hello search results, select **Django** (published by PTVS), then click **Create**.</span></span>
4. <span data-ttu-id="0faec-135">Konfigurera hello nya Django-appen, till exempel skapa en ny App Service-plan och en ny resursgrupp för den.</span><span class="sxs-lookup"><span data-stu-id="0faec-135">Configure hello new Django app, such as creating a new App Service plan and a new resource group for it.</span></span> <span data-ttu-id="0faec-136">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="0faec-136">Then, click **Create**.</span></span>
5. <span data-ttu-id="0faec-137">Konfigurera Git-publicering för nyskapade webbappen genom att följa anvisningarna hello på [lokal Git-distribution tooAzure Apptjänst](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="0faec-137">Configure Git publishing for your newly created web app by following hello instructions at [Local Git Deployment tooAzure App Service](app-service-deploy-local-git.md).</span></span>

## <a name="application-overview"></a><span data-ttu-id="0faec-138">Programöversikt</span><span class="sxs-lookup"><span data-stu-id="0faec-138">Application Overview</span></span>
### <a name="git-repository-contents"></a><span data-ttu-id="0faec-139">Innehåll på Git-lagringsplatsen</span><span class="sxs-lookup"><span data-stu-id="0faec-139">Git repository contents</span></span>
<span data-ttu-id="0faec-140">Här är en översikt över hello-filer som du hittar i hello första Git-lagringsplatsen, som vi ska klona i nästa avsnitt om hello.</span><span class="sxs-lookup"><span data-stu-id="0faec-140">Here's an overview of hello files you'll find in hello initial Git repository, which we'll clone in hello next section.</span></span>

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

<span data-ttu-id="0faec-141">Programmet hello huvudsakliga källor.</span><span class="sxs-lookup"><span data-stu-id="0faec-141">Main sources for hello application.</span></span> <span data-ttu-id="0faec-142">Består av tre sidor (index, om och kontakt) med bakgrundslayout.</span><span class="sxs-lookup"><span data-stu-id="0faec-142">Consists of 3 pages (index, about, contact) with a master layout.</span></span> <span data-ttu-id="0faec-143">Statiskt innehåll och skript inkluderar bootstrap, jquery, modernizr och respond.</span><span class="sxs-lookup"><span data-stu-id="0faec-143">Static content and scripts include bootstrap, jquery, modernizr and respond.</span></span>

    \manage.py

<span data-ttu-id="0faec-144">Stöd för lokal hanterings- och utvecklingsserver.</span><span class="sxs-lookup"><span data-stu-id="0faec-144">Local management and development server support.</span></span> <span data-ttu-id="0faec-145">Använda toorun hello programmet lokalt, synkronisera databasen hello osv.</span><span class="sxs-lookup"><span data-stu-id="0faec-145">Use this toorun hello application locally, synchronize hello database, etc.</span></span>

    \db.sqlite3

<span data-ttu-id="0faec-146">Standarddatabas.</span><span class="sxs-lookup"><span data-stu-id="0faec-146">Default database.</span></span> <span data-ttu-id="0faec-147">Inkluderar hello nödvändiga tabeller för hello programmet toorun, men innehåller inte några användare (synkronisera hello databasen toocreate en användare).</span><span class="sxs-lookup"><span data-stu-id="0faec-147">Includes hello necessary tables for hello application toorun, but does not contain any users (synchronize hello database toocreate a user).</span></span>

    \DjangoWebProject.pyproj
    \DjangoWebProject.sln

<span data-ttu-id="0faec-148">Projektfiler för användning med [Python Tools för Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="0faec-148">Project files for use with [Python Tools for Visual Studio].</span></span>

    \ptvs_virtualenv_proxy.py

<span data-ttu-id="0faec-149">IIS-proxy för virtuella miljöer och PTVS-stöd för fjärrfelsökning.</span><span class="sxs-lookup"><span data-stu-id="0faec-149">IIS proxy for virtual environments and PTVS remote debugging support.</span></span>

    \requirements.txt

<span data-ttu-id="0faec-150">Externa paket som krävs för programmet.</span><span class="sxs-lookup"><span data-stu-id="0faec-150">External packages needed by this application.</span></span> <span data-ttu-id="0faec-151">hello distributionsskriptet pip hello installationspaket som anges i den här filen.</span><span class="sxs-lookup"><span data-stu-id="0faec-151">hello deployment script will pip install hello packages listed in this file.</span></span>

    \web.2.7.config
    \web.3.4.config

<span data-ttu-id="0faec-152">IIS-konfigurationsfiler.</span><span class="sxs-lookup"><span data-stu-id="0faec-152">IIS configuration files.</span></span> <span data-ttu-id="0faec-153">hello distributionsskriptet använder lämplig web.x.y.config för hello och kopian web.config.</span><span class="sxs-lookup"><span data-stu-id="0faec-153">hello deployment script will use hello appropriate web.x.y.config and copy it as web.config.</span></span>

### <a name="optional-files---customizing-deployment"></a><span data-ttu-id="0faec-154">Valfria filer – anpassa distributionen</span><span class="sxs-lookup"><span data-stu-id="0faec-154">Optional files - Customizing deployment</span></span>
[!INCLUDE [web-sites-python-django-customizing-deployment](../../includes/web-sites-python-django-customizing-deployment.md)]

### <a name="optional-files---python-runtime"></a><span data-ttu-id="0faec-155">Valfria filer – Python-körning</span><span class="sxs-lookup"><span data-stu-id="0faec-155">Optional files - Python runtime</span></span>
[!INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

### <a name="additional-files-on-server"></a><span data-ttu-id="0faec-156">Ytterligare filer på servern</span><span class="sxs-lookup"><span data-stu-id="0faec-156">Additional files on server</span></span>
<span data-ttu-id="0faec-157">Vissa filer finnas på hello-servern men läggs inte toohello git-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="0faec-157">Some files exist on hello server but are not added toohello git repository.</span></span> <span data-ttu-id="0faec-158">Dessa skapas med hello distributionsskriptet.</span><span class="sxs-lookup"><span data-stu-id="0faec-158">These are created by hello deployment script.</span></span>

    \web.config

<span data-ttu-id="0faec-159">IIS-konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="0faec-159">IIS configuration file.</span></span> <span data-ttu-id="0faec-160">Skapas utifrån web.x.y.config vid varje distribution.</span><span class="sxs-lookup"><span data-stu-id="0faec-160">Created from web.x.y.config on every deployment.</span></span>

    \env\

<span data-ttu-id="0faec-161">Python, virtuell miljö</span><span class="sxs-lookup"><span data-stu-id="0faec-161">Python virtual environment.</span></span> <span data-ttu-id="0faec-162">Skapas under distributionen om det inte redan finns en kompatibel virtuell miljö hello webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="0faec-162">Created during deployment if a compatible virtual environment doesn't already exist on hello web app.</span></span> <span data-ttu-id="0faec-163">Paket som anges i requirements.txt är pip-installeras, men pip hoppar över installationen om paketen hello redan är installerade.</span><span class="sxs-lookup"><span data-stu-id="0faec-163">Packages listed in requirements.txt are pip installed, but pip will skip installation if hello packages are already installed.</span></span>

<span data-ttu-id="0faec-164">hello 3 nästa avsnitt beskrivs hur tooproceed med hello webbappsutveckling under 3 olika miljöer:</span><span class="sxs-lookup"><span data-stu-id="0faec-164">hello next 3 sections describe how tooproceed with hello web app development under 3 different environments:</span></span>

* <span data-ttu-id="0faec-165">Windows med Python Tools för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0faec-165">Windows, with Python Tools for Visual Studio</span></span>
* <span data-ttu-id="0faec-166">Windows, med kommandorad</span><span class="sxs-lookup"><span data-stu-id="0faec-166">Windows, with command line</span></span>
* <span data-ttu-id="0faec-167">Mac/Linux, med kommandorad</span><span class="sxs-lookup"><span data-stu-id="0faec-167">Mac/Linux, with command line</span></span>

## <a name="web-app-development---windows---python-tools-for-visual-studio"></a><span data-ttu-id="0faec-168">Webbappsutveckling – Windows – Python Tools för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0faec-168">Web app development - Windows - Python Tools for Visual Studio</span></span>
### <a name="clone-hello-repository"></a><span data-ttu-id="0faec-169">Klona hello databasen</span><span class="sxs-lookup"><span data-stu-id="0faec-169">Clone hello repository</span></span>
<span data-ttu-id="0faec-170">Först klonar hello-databas med hjälp av hello Webbadressen som tillhandahålls på hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="0faec-170">First, clone hello repository using hello URL provided on hello Azure Portal.</span></span> <span data-ttu-id="0faec-171">Mer information finns i [lokal Git-distribution tooAzure Apptjänst](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="0faec-171">For more information, see [Local Git Deployment tooAzure App Service](app-service-deploy-local-git.md).</span></span>

<span data-ttu-id="0faec-172">Öppna hello lösningsfilen (.sln) som ingår i hello rot hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="0faec-172">Open hello solution file (.sln) that is included in hello root of hello repository.</span></span>

![](./media/web-sites-python-create-deploy-django-app/ptvs-solution-django.png)

### <a name="create-virtual-environment"></a><span data-ttu-id="0faec-173">Skapa en virtuell miljö</span><span class="sxs-lookup"><span data-stu-id="0faec-173">Create virtual environment</span></span>
<span data-ttu-id="0faec-174">Nu ska vi skapa en virtuell miljö för lokal utveckling.</span><span class="sxs-lookup"><span data-stu-id="0faec-174">Now we'll create a virtual environment for local development.</span></span> <span data-ttu-id="0faec-175">Högerklicka på **Python Environments** (Python-miljöer) och välj **Add Virtual Environment...** (Lägg till virtuell miljö).</span><span class="sxs-lookup"><span data-stu-id="0faec-175">Right-click on **Python Environments** select **Add Virtual Environment...**.</span></span>

* <span data-ttu-id="0faec-176">Kontrollera hello hello miljö heter `env`.</span><span class="sxs-lookup"><span data-stu-id="0faec-176">Make sure hello name of hello environment is `env`.</span></span>
* <span data-ttu-id="0faec-177">Välj hello bastolk.</span><span class="sxs-lookup"><span data-stu-id="0faec-177">Select hello base interpreter.</span></span> <span data-ttu-id="0faec-178">Kontrollera att toouse hello samma version av Python som valts för webbappen (i runtime.txt eller hello **programinställningar** bladet för din webbapp i hello Azure Portal).</span><span class="sxs-lookup"><span data-stu-id="0faec-178">Make sure toouse hello same version of Python that is selected for your web app (in runtime.txt or hello **Application Settings** blade of your web app in hello Azure Portal).</span></span>
* <span data-ttu-id="0faec-179">Kontrollera att hello alternativet toodownload och installera paket är markerat.</span><span class="sxs-lookup"><span data-stu-id="0faec-179">Make sure hello option toodownload and install packages is checked.</span></span>

![](./media/web-sites-python-create-deploy-django-app/ptvs-add-virtual-env-27.png)

<span data-ttu-id="0faec-180">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="0faec-180">Click **Create**.</span></span> <span data-ttu-id="0faec-181">Detta skapar hello virtuell miljö och installera beroenden som anges i requirements.txt.</span><span class="sxs-lookup"><span data-stu-id="0faec-181">This will create hello virtual environment, and install dependencies listed in requirements.txt.</span></span>

### <a name="create-a-superuser"></a><span data-ttu-id="0faec-182">Skapa en användare med fullständig behörighet</span><span class="sxs-lookup"><span data-stu-id="0faec-182">Create a superuser</span></span>
<span data-ttu-id="0faec-183">hello har ingår hello programmet inte definierats en superanvändare.</span><span class="sxs-lookup"><span data-stu-id="0faec-183">hello database included with hello application does not have any superuser defined.</span></span> <span data-ttu-id="0faec-184">Ordning toouse hello inloggning funktionsändringar i hello program eller hello administratörsgränssnittet för Django (om du väljer tooenable den), behöver du toocreate superanvändare.</span><span class="sxs-lookup"><span data-stu-id="0faec-184">In order toouse hello sign-in functionality in hello application, or hello Django admin interface (if you decide tooenable it), you'll need toocreate a superuser.</span></span>

<span data-ttu-id="0faec-185">Kör det från hello kommandoraden i projektmappen:</span><span class="sxs-lookup"><span data-stu-id="0faec-185">Run this from hello command-line from your project folder:</span></span>

    env\scripts\python manage.py createsuperuser

<span data-ttu-id="0faec-186">Följ hello prompter tooset hello användarnamn, lösenord osv.</span><span class="sxs-lookup"><span data-stu-id="0faec-186">Follow hello prompts tooset hello user name, password, etc.</span></span>

### <a name="run-using-development-server"></a><span data-ttu-id="0faec-187">Kör med utvecklingsservern</span><span class="sxs-lookup"><span data-stu-id="0faec-187">Run using development server</span></span>
<span data-ttu-id="0faec-188">Tryck på F5 toostart felsökning och webbläsaren öppnas automatiskt toohello sidan som körs lokalt.</span><span class="sxs-lookup"><span data-stu-id="0faec-188">Press F5 toostart debugging, and your web browser will open automatically toohello page running locally.</span></span>

![](./media/web-sites-python-create-deploy-django-app/windows-browser-django.png)

<span data-ttu-id="0faec-189">Du kan ange brytpunkter i hello källor, Använd hello titta på windows osv. Se hello [Python Tools för Visual Studio-dokumentationen] hello mer information om olika funktioner.</span><span class="sxs-lookup"><span data-stu-id="0faec-189">You can set breakpoints in hello sources, use hello watch windows, etc. See hello [Python Tools for Visual Studio Documentation] for more information on hello various features.</span></span>

### <a name="make-changes"></a><span data-ttu-id="0faec-190">Göra ändringar</span><span class="sxs-lookup"><span data-stu-id="0faec-190">Make changes</span></span>
<span data-ttu-id="0faec-191">Nu kan du experimentera med att göra ändringar toohello programmets källor och mallar.</span><span class="sxs-lookup"><span data-stu-id="0faec-191">Now you can experiment by making changes toohello application sources and/or templates.</span></span>

<span data-ttu-id="0faec-192">När du har testat ändringarna checkar du in dem toohello Git-lagringsplatsen:</span><span class="sxs-lookup"><span data-stu-id="0faec-192">After you've tested your changes, commit them toohello Git repository:</span></span>

![](./media/web-sites-python-create-deploy-django-app/ptvs-commit-django.png)

### <a name="install-more-packages"></a><span data-ttu-id="0faec-193">Installera fler paket</span><span class="sxs-lookup"><span data-stu-id="0faec-193">Install more packages</span></span>
<span data-ttu-id="0faec-194">Programmet kan ha beroenden utöver Python och Django.</span><span class="sxs-lookup"><span data-stu-id="0faec-194">Your application may have dependencies beyond Python and Django.</span></span>

<span data-ttu-id="0faec-195">Du kan installera ytterligare paket med hjälp av pip.</span><span class="sxs-lookup"><span data-stu-id="0faec-195">You can install additional packages using pip.</span></span> <span data-ttu-id="0faec-196">tooinstall ett paket, högerklicka på hello virtuell miljö och välj **Install Python Package**.</span><span class="sxs-lookup"><span data-stu-id="0faec-196">tooinstall a package, right-click on hello virtual environment and select **Install Python Package**.</span></span>

<span data-ttu-id="0faec-197">Till exempel tooinstall hello Azure SDK för Python, som ger dig åtkomst tooAzure storage, service bus och andra Azure-tjänster, ange `azure`:</span><span class="sxs-lookup"><span data-stu-id="0faec-197">For example, tooinstall hello Azure SDK for Python, which gives you access tooAzure storage, service bus and other Azure services, enter `azure`:</span></span>

![](./media/web-sites-python-create-deploy-django-app/ptvs-install-package-dialog.png)

<span data-ttu-id="0faec-198">Högerklicka på hello virtuell miljö och välj **generera requirements.txt** tooupdate requirements.txt.</span><span class="sxs-lookup"><span data-stu-id="0faec-198">Right-click on hello virtual environment and select **Generate requirements.txt** tooupdate requirements.txt.</span></span>

<span data-ttu-id="0faec-199">Checka sedan in hello ändringar toorequirements.txt toohello Git-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="0faec-199">Then, commit hello changes toorequirements.txt toohello Git repository.</span></span>

### <a name="deploy-tooazure"></a><span data-ttu-id="0faec-200">Distribuera tooAzure</span><span class="sxs-lookup"><span data-stu-id="0faec-200">Deploy tooAzure</span></span>
<span data-ttu-id="0faec-201">tootrigger en distribution, klicka på **Sync** eller **Push**.</span><span class="sxs-lookup"><span data-stu-id="0faec-201">tootrigger a deployment, click on **Sync** or **Push**.</span></span> <span data-ttu-id="0faec-202">Med synkronisering görs både en överföring och en hämtning.</span><span class="sxs-lookup"><span data-stu-id="0faec-202">Sync does both a push and a pull.</span></span>

![](./media/web-sites-python-create-deploy-django-app/ptvs-git-push.png)

<span data-ttu-id="0faec-203">hello första distributionen tar en stund, eftersom den skapar en virtuell miljö, paket installeras osv.</span><span class="sxs-lookup"><span data-stu-id="0faec-203">hello first deployment will take some time, as it will create a virtual environment, install packages, etc.</span></span>

<span data-ttu-id="0faec-204">Visual Studio visas inte hello fortskrider hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="0faec-204">Visual Studio doesn't show hello progress of hello deployment.</span></span> <span data-ttu-id="0faec-205">Om du vill att tooreview hello utdata avsnittet hello på [felsökning – distribution](#troubleshooting-deployment).</span><span class="sxs-lookup"><span data-stu-id="0faec-205">If you'd like tooreview hello output, see hello section on [Troubleshooting - Deployment](#troubleshooting-deployment).</span></span>

<span data-ttu-id="0faec-206">Bläddra toohello Azure URL tooview ändringarna.</span><span class="sxs-lookup"><span data-stu-id="0faec-206">Browse toohello Azure URL tooview your changes.</span></span>

## <a name="web-app-development---windows---command-line"></a><span data-ttu-id="0faec-207">Webbappsutveckling – Windows – kommandorad</span><span class="sxs-lookup"><span data-stu-id="0faec-207">Web app development - Windows - command line</span></span>
### <a name="clone-hello-repository"></a><span data-ttu-id="0faec-208">Klona hello databasen</span><span class="sxs-lookup"><span data-stu-id="0faec-208">Clone hello repository</span></span>
<span data-ttu-id="0faec-209">Först klonar hello lagringsplatsen med hjälp av hello Webbadressen som tillhandahålls på hello Azure-portalen och lägga till hello Azure-lagringsplatsen som fjärransluten.</span><span class="sxs-lookup"><span data-stu-id="0faec-209">First, clone hello repository using hello URL provided on hello Azure Portal, and add hello Azure repository as a remote.</span></span> <span data-ttu-id="0faec-210">Mer information finns i [lokal Git-distribution tooAzure Apptjänst](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="0faec-210">For more information, see [Local Git Deployment tooAzure App Service](app-service-deploy-local-git.md).</span></span>

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url>

### <a name="create-virtual-environment"></a><span data-ttu-id="0faec-211">Skapa en virtuell miljö</span><span class="sxs-lookup"><span data-stu-id="0faec-211">Create virtual environment</span></span>
<span data-ttu-id="0faec-212">Vi ska skapa en ny virtuell miljö för utveckling (inte Lägg till den toohello databasen).</span><span class="sxs-lookup"><span data-stu-id="0faec-212">We'll create a new virtual environment for development purposes (do not add it toohello repository).</span></span> <span data-ttu-id="0faec-213">Virtuella miljöer i Python är inte relokerbara, så alla utvecklare som arbetar på hello programmet skapa sina egna lokalt.</span><span class="sxs-lookup"><span data-stu-id="0faec-213">Virtual environments in Python are not relocatable, so every developer working on hello application will create their own locally.</span></span>

<span data-ttu-id="0faec-214">Kontrollera att toouse hello samma version av Python som valts för webbappen (i runtime.txt eller hello bladet programinställningar för webbappen i hello Azure Portal).</span><span class="sxs-lookup"><span data-stu-id="0faec-214">Make sure toouse hello same version of Python that is selected for your web app (in runtime.txt or hello Application Settings blade of your web app in hello Azure Portal).</span></span>

<span data-ttu-id="0faec-215">För Python 2.7:</span><span class="sxs-lookup"><span data-stu-id="0faec-215">For Python 2.7:</span></span>

    c:\python27\python.exe -m virtualenv env

<span data-ttu-id="0faec-216">För Python 3.4:</span><span class="sxs-lookup"><span data-stu-id="0faec-216">For Python 3.4:</span></span>

    c:\python34\python.exe -m venv env

<span data-ttu-id="0faec-217">Installera eventuella externa paket som krävs för programmet.</span><span class="sxs-lookup"><span data-stu-id="0faec-217">Install any external packages required by your application.</span></span> <span data-ttu-id="0faec-218">Du kan använda hello requirements.txt filen i hello rot hello databasen tooinstall hello paket i den virtuella miljön:</span><span class="sxs-lookup"><span data-stu-id="0faec-218">You can use hello requirements.txt file at hello root of hello repository tooinstall hello packages in your virtual environment:</span></span>

    env\scripts\pip install -r requirements.txt

### <a name="create-a-superuser"></a><span data-ttu-id="0faec-219">Skapa en användare med fullständig behörighet</span><span class="sxs-lookup"><span data-stu-id="0faec-219">Create a superuser</span></span>
<span data-ttu-id="0faec-220">hello har ingår hello programmet inte definierats en superanvändare.</span><span class="sxs-lookup"><span data-stu-id="0faec-220">hello database included with hello application does not have any superuser defined.</span></span> <span data-ttu-id="0faec-221">Ordning toouse hello inloggning funktionsändringar i hello program eller hello administratörsgränssnittet för Django (om du väljer tooenable den), behöver du toocreate superanvändare.</span><span class="sxs-lookup"><span data-stu-id="0faec-221">In order toouse hello sign-in functionality in hello application, or hello Django admin interface (if you decide tooenable it), you'll need toocreate a superuser.</span></span>

<span data-ttu-id="0faec-222">Kör det från hello kommandoraden i projektmappen:</span><span class="sxs-lookup"><span data-stu-id="0faec-222">Run this from hello command-line from your project folder:</span></span>

    env\scripts\python manage.py createsuperuser

<span data-ttu-id="0faec-223">Följ hello prompter tooset hello användarnamn, lösenord osv.</span><span class="sxs-lookup"><span data-stu-id="0faec-223">Follow hello prompts tooset hello user name, password, etc.</span></span>

### <a name="run-using-development-server"></a><span data-ttu-id="0faec-224">Kör med utvecklingsservern</span><span class="sxs-lookup"><span data-stu-id="0faec-224">Run using development server</span></span>
<span data-ttu-id="0faec-225">Du kan starta programmet hello under en utvecklingsserver med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="0faec-225">You can launch hello application under a development server with hello following command:</span></span>

    env\scripts\python manage.py runserver

<span data-ttu-id="0faec-226">hello konsolen visar hello URL-adress och port hello servern lyssnar på:</span><span class="sxs-lookup"><span data-stu-id="0faec-226">hello console will display hello URL and port hello server listens to:</span></span>

![](./media/web-sites-python-create-deploy-django-app/windows-run-local-django.png)

<span data-ttu-id="0faec-227">Öppna din Webbadress webbläsare toothat.</span><span class="sxs-lookup"><span data-stu-id="0faec-227">Then, open your web browser toothat URL.</span></span>

![](./media/web-sites-python-create-deploy-django-app/windows-browser-django.png)

### <a name="make-changes"></a><span data-ttu-id="0faec-228">Göra ändringar</span><span class="sxs-lookup"><span data-stu-id="0faec-228">Make changes</span></span>
<span data-ttu-id="0faec-229">Nu kan du experimentera med att göra ändringar toohello programmets källor och mallar.</span><span class="sxs-lookup"><span data-stu-id="0faec-229">Now you can experiment by making changes toohello application sources and/or templates.</span></span>

<span data-ttu-id="0faec-230">När du har testat ändringarna checkar du in dem toohello Git-lagringsplatsen:</span><span class="sxs-lookup"><span data-stu-id="0faec-230">After you've tested your changes, commit them toohello Git repository:</span></span>

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a><span data-ttu-id="0faec-231">Installera fler paket</span><span class="sxs-lookup"><span data-stu-id="0faec-231">Install more packages</span></span>
<span data-ttu-id="0faec-232">Programmet kan ha beroenden utöver Python och Django.</span><span class="sxs-lookup"><span data-stu-id="0faec-232">Your application may have dependencies beyond Python and Django.</span></span>

<span data-ttu-id="0faec-233">Du kan installera ytterligare paket med hjälp av pip.</span><span class="sxs-lookup"><span data-stu-id="0faec-233">You can install additional packages using pip.</span></span> <span data-ttu-id="0faec-234">Till exempel tooinstall hello Azure SDK för Python, som ger dig åtkomst till tooAzure storage, service bus och andra Azure-tjänster, typ:</span><span class="sxs-lookup"><span data-stu-id="0faec-234">For example, tooinstall hello Azure SDK for Python, which gives you access tooAzure storage, service bus and other Azure services, type:</span></span>

    env\scripts\pip install azure

<span data-ttu-id="0faec-235">Se till att tooupdate requirements.txt:</span><span class="sxs-lookup"><span data-stu-id="0faec-235">Make sure tooupdate requirements.txt:</span></span>

    env\scripts\pip freeze > requirements.txt

<span data-ttu-id="0faec-236">Genomför hello ändringar:</span><span class="sxs-lookup"><span data-stu-id="0faec-236">Commit hello changes:</span></span>

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-tooazure"></a><span data-ttu-id="0faec-237">Distribuera tooAzure</span><span class="sxs-lookup"><span data-stu-id="0faec-237">Deploy tooAzure</span></span>
<span data-ttu-id="0faec-238">tootrigger en distribution push hello ändrar tooAzure:</span><span class="sxs-lookup"><span data-stu-id="0faec-238">tootrigger a deployment, push hello changes tooAzure:</span></span>

    git push azure master

<span data-ttu-id="0faec-239">Du ser hello utdata från hello distributionsskriptet, inklusive skapande av virtuell miljö, installation av paket och skapande av web.config.</span><span class="sxs-lookup"><span data-stu-id="0faec-239">You will see hello output of hello deployment script, including virtual environment creation, installation of packages, creation of web.config.</span></span>

<span data-ttu-id="0faec-240">Bläddra toohello Azure URL tooview ändringarna.</span><span class="sxs-lookup"><span data-stu-id="0faec-240">Browse toohello Azure URL tooview your changes.</span></span>

## <a name="web-app-development---maclinux---command-line"></a><span data-ttu-id="0faec-241">Webbappsutveckling – Mac/Linux – kommandorad</span><span class="sxs-lookup"><span data-stu-id="0faec-241">Web app development - Mac/Linux - command line</span></span>
### <a name="clone-hello-repository"></a><span data-ttu-id="0faec-242">Klona hello databasen</span><span class="sxs-lookup"><span data-stu-id="0faec-242">Clone hello repository</span></span>
<span data-ttu-id="0faec-243">Först klonar hello lagringsplatsen med hjälp av hello Webbadressen som tillhandahålls på hello Azure-portalen och lägga till hello Azure-lagringsplatsen som fjärransluten.</span><span class="sxs-lookup"><span data-stu-id="0faec-243">First, clone hello repository using hello URL provided on hello Azure Portal, and add hello Azure repository as a remote.</span></span> <span data-ttu-id="0faec-244">Mer information finns i [lokal Git-distribution tooAzure Apptjänst](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="0faec-244">For more information, see [Local Git Deployment tooAzure App Service](app-service-deploy-local-git.md).</span></span>

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url>

### <a name="create-virtual-environment"></a><span data-ttu-id="0faec-245">Skapa en virtuell miljö</span><span class="sxs-lookup"><span data-stu-id="0faec-245">Create virtual environment</span></span>
<span data-ttu-id="0faec-246">Vi ska skapa en ny virtuell miljö för utveckling (inte Lägg till den toohello databasen).</span><span class="sxs-lookup"><span data-stu-id="0faec-246">We'll create a new virtual environment for development purposes (do not add it toohello repository).</span></span> <span data-ttu-id="0faec-247">Virtuella miljöer i Python är inte relokerbara, så alla utvecklare som arbetar på hello programmet skapa sina egna lokalt.</span><span class="sxs-lookup"><span data-stu-id="0faec-247">Virtual environments in Python are not relocatable, so every developer working on hello application will create their own locally.</span></span>

<span data-ttu-id="0faec-248">Kontrollera att toouse hello samma version av Python som valts för webbappen (i runtime.txt eller hello bladet programinställningar för webbappen i hello Azure Portal).</span><span class="sxs-lookup"><span data-stu-id="0faec-248">Make sure toouse hello same version of Python that is selected for your web app (in runtime.txt or hello Application Settings blade of your web app in hello Azure Portal).</span></span>

<span data-ttu-id="0faec-249">För Python 2.7:</span><span class="sxs-lookup"><span data-stu-id="0faec-249">For Python 2.7:</span></span>

    python -m virtualenv env

<span data-ttu-id="0faec-250">För Python 3.4:</span><span class="sxs-lookup"><span data-stu-id="0faec-250">For Python 3.4:</span></span>

    python -m venv env

<span data-ttu-id="0faec-251">eller</span><span class="sxs-lookup"><span data-stu-id="0faec-251">or</span></span>

    pyvenv env

<span data-ttu-id="0faec-252">Installera eventuella externa paket som krävs för programmet.</span><span class="sxs-lookup"><span data-stu-id="0faec-252">Install any external packages required by your application.</span></span> <span data-ttu-id="0faec-253">Du kan använda hello requirements.txt filen i hello rot hello databasen tooinstall hello paket i den virtuella miljön:</span><span class="sxs-lookup"><span data-stu-id="0faec-253">You can use hello requirements.txt file at hello root of hello repository tooinstall hello packages in your virtual environment:</span></span>

    env/bin/pip install -r requirements.txt

### <a name="create-a-superuser"></a><span data-ttu-id="0faec-254">Skapa en användare med fullständig behörighet</span><span class="sxs-lookup"><span data-stu-id="0faec-254">Create a superuser</span></span>
<span data-ttu-id="0faec-255">hello har ingår hello programmet inte definierats en superanvändare.</span><span class="sxs-lookup"><span data-stu-id="0faec-255">hello database included with hello application does not have any superuser defined.</span></span> <span data-ttu-id="0faec-256">Ordning toouse hello inloggning funktionsändringar i hello program eller hello administratörsgränssnittet för Django (om du väljer tooenable den), behöver du toocreate superanvändare.</span><span class="sxs-lookup"><span data-stu-id="0faec-256">In order toouse hello sign-in functionality in hello application, or hello Django admin interface (if you decide tooenable it), you'll need toocreate a superuser.</span></span>

<span data-ttu-id="0faec-257">Kör det från hello kommandoraden i projektmappen:</span><span class="sxs-lookup"><span data-stu-id="0faec-257">Run this from hello command-line from your project folder:</span></span>

    env/bin/python manage.py createsuperuser

<span data-ttu-id="0faec-258">Följ hello prompter tooset hello användarnamn, lösenord osv.</span><span class="sxs-lookup"><span data-stu-id="0faec-258">Follow hello prompts tooset hello user name, password, etc.</span></span>

### <a name="run-using-development-server"></a><span data-ttu-id="0faec-259">Kör med utvecklingsservern</span><span class="sxs-lookup"><span data-stu-id="0faec-259">Run using development server</span></span>
<span data-ttu-id="0faec-260">Du kan starta programmet hello under en utvecklingsserver med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="0faec-260">You can launch hello application under a development server with hello following command:</span></span>

    env/bin/python manage.py runserver

<span data-ttu-id="0faec-261">hello konsolen visar hello URL-adress och port hello servern lyssnar på:</span><span class="sxs-lookup"><span data-stu-id="0faec-261">hello console will display hello URL and port hello server listens to:</span></span>

![](./media/web-sites-python-create-deploy-django-app/mac-run-local-django.png)

<span data-ttu-id="0faec-262">Öppna din Webbadress webbläsare toothat.</span><span class="sxs-lookup"><span data-stu-id="0faec-262">Then, open your web browser toothat URL.</span></span>

![](./media/web-sites-python-create-deploy-django-app/mac-browser-django.png)

### <a name="make-changes"></a><span data-ttu-id="0faec-263">Göra ändringar</span><span class="sxs-lookup"><span data-stu-id="0faec-263">Make changes</span></span>
<span data-ttu-id="0faec-264">Nu kan du experimentera med att göra ändringar toohello programmets källor och mallar.</span><span class="sxs-lookup"><span data-stu-id="0faec-264">Now you can experiment by making changes toohello application sources and/or templates.</span></span>

<span data-ttu-id="0faec-265">När du har testat ändringarna checkar du in dem toohello Git-lagringsplatsen:</span><span class="sxs-lookup"><span data-stu-id="0faec-265">After you've tested your changes, commit them toohello Git repository:</span></span>

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a><span data-ttu-id="0faec-266">Installera fler paket</span><span class="sxs-lookup"><span data-stu-id="0faec-266">Install more packages</span></span>
<span data-ttu-id="0faec-267">Programmet kan ha beroenden utöver Python och Django.</span><span class="sxs-lookup"><span data-stu-id="0faec-267">Your application may have dependencies beyond Python and Django.</span></span>

<span data-ttu-id="0faec-268">Du kan installera ytterligare paket med hjälp av pip.</span><span class="sxs-lookup"><span data-stu-id="0faec-268">You can install additional packages using pip.</span></span> <span data-ttu-id="0faec-269">Till exempel tooinstall hello Azure SDK för Python, som ger dig åtkomst till tooAzure storage, service bus och andra Azure-tjänster, typ:</span><span class="sxs-lookup"><span data-stu-id="0faec-269">For example, tooinstall hello Azure SDK for Python, which gives you access tooAzure storage, service bus and other Azure services, type:</span></span>

    env/bin/pip install azure

<span data-ttu-id="0faec-270">Se till att tooupdate requirements.txt:</span><span class="sxs-lookup"><span data-stu-id="0faec-270">Make sure tooupdate requirements.txt:</span></span>

    env/bin/pip freeze > requirements.txt

<span data-ttu-id="0faec-271">Genomför hello ändringar:</span><span class="sxs-lookup"><span data-stu-id="0faec-271">Commit hello changes:</span></span>

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-tooazure"></a><span data-ttu-id="0faec-272">Distribuera tooAzure</span><span class="sxs-lookup"><span data-stu-id="0faec-272">Deploy tooAzure</span></span>
<span data-ttu-id="0faec-273">tootrigger en distribution push hello ändrar tooAzure:</span><span class="sxs-lookup"><span data-stu-id="0faec-273">tootrigger a deployment, push hello changes tooAzure:</span></span>

    git push azure master

<span data-ttu-id="0faec-274">Du ser hello utdata från hello distributionsskriptet, inklusive skapande av virtuell miljö, installation av paket och skapande av web.config.</span><span class="sxs-lookup"><span data-stu-id="0faec-274">You will see hello output of hello deployment script, including virtual environment creation, installation of packages, creation of web.config.</span></span>

<span data-ttu-id="0faec-275">Bläddra toohello Azure URL tooview ändringarna.</span><span class="sxs-lookup"><span data-stu-id="0faec-275">Browse toohello Azure URL tooview your changes.</span></span>

## <a name="troubleshooting---package-installation"></a><span data-ttu-id="0faec-276">Felsökning – installation av paket</span><span class="sxs-lookup"><span data-stu-id="0faec-276">Troubleshooting - Package Installation</span></span>
[!INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]

## <a name="troubleshooting---virtual-environment"></a><span data-ttu-id="0faec-277">Felsökning – virtuell miljö</span><span class="sxs-lookup"><span data-stu-id="0faec-277">Troubleshooting - Virtual Environment</span></span>
[!INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]

## <a name="troubleshooting---static-files"></a><span data-ttu-id="0faec-278">Felsökning – statiska filer</span><span class="sxs-lookup"><span data-stu-id="0faec-278">Troubleshooting - Static Files</span></span>
<span data-ttu-id="0faec-279">Django har hello begreppet att samla in statiska filer.</span><span class="sxs-lookup"><span data-stu-id="0faec-279">Django has hello concept of collecting static files.</span></span> <span data-ttu-id="0faec-280">Detta tar alla hello statiska filer från deras ursprungliga plats och kopierar dem tooa enstaka mapp.</span><span class="sxs-lookup"><span data-stu-id="0faec-280">This takes all hello static files from their original location and copies them tooa single folder.</span></span> <span data-ttu-id="0faec-281">För det här programmet kopieras de för`/static`.</span><span class="sxs-lookup"><span data-stu-id="0faec-281">For this application, they are copied too`/static`.</span></span>

<span data-ttu-id="0faec-282">Det görs eftersom statiska filer kan komma från olika "Django-appar".</span><span class="sxs-lookup"><span data-stu-id="0faec-282">This is done because static files may come from different Django 'apps'.</span></span> <span data-ttu-id="0faec-283">Till exempel finns hello statiska filer från hello administratörsgränssnittet för django i undermappen Django-biblioteket i hello virtuell miljö.</span><span class="sxs-lookup"><span data-stu-id="0faec-283">For example, hello static files from hello Django admin interfaces are located in a Django library subfolder in hello virtual environment.</span></span> <span data-ttu-id="0faec-284">Statiska filer som definieras av programmet finns i `/app/static`.</span><span class="sxs-lookup"><span data-stu-id="0faec-284">Static files defined by this application are located in `/app/static`.</span></span> <span data-ttu-id="0faec-285">När du använder flera "Django-appar" sparas statiska filer på flera platser.</span><span class="sxs-lookup"><span data-stu-id="0faec-285">As you use more Django 'apps', you'll have static files located in multiple places.</span></span>

<span data-ttu-id="0faec-286">När du kör programmet hello i felsökningsläge hanterar hello programmet hello statiska filer från deras ursprungliga plats.</span><span class="sxs-lookup"><span data-stu-id="0faec-286">When running hello application in debug mode, hello application serves hello static files from their original location.</span></span>

<span data-ttu-id="0faec-287">När du kör hello programmet i versionsläge hello program har **inte** hantera hello statiska filer.</span><span class="sxs-lookup"><span data-stu-id="0faec-287">When running hello application in release mode, hello application does **not** serve hello static files.</span></span> <span data-ttu-id="0faec-288">Det är hello ansvar hello web server tooserve hello-filer.</span><span class="sxs-lookup"><span data-stu-id="0faec-288">It is hello responsibility of hello web server tooserve hello files.</span></span> <span data-ttu-id="0faec-289">För det här programmet hanterar IIS hello statiska filer från `/static`.</span><span class="sxs-lookup"><span data-stu-id="0faec-289">For this application, IIS will serve hello static files from `/static`.</span></span>

<span data-ttu-id="0faec-290">hello insamlingen av statiska filer görs automatiskt som en del av hello distributionsskriptet rensar tidigare insamlade filer.</span><span class="sxs-lookup"><span data-stu-id="0faec-290">hello collection of static files is done automatically as part of hello deployment script, clearing previously collected files.</span></span> <span data-ttu-id="0faec-291">Detta innebär hello samlingen sker för varje distribution långsammare distribution lite, men ser till att filer inte tillgängliga, vilket undanröjer potentiella säkerhetsproblem.</span><span class="sxs-lookup"><span data-stu-id="0faec-291">This means hello collection occurs on every deployment, slowing down deployment a bit, but it ensures that obsolete files won't be available, avoiding a potential security issue.</span></span>

<span data-ttu-id="0faec-292">Om du vill tooskip insamlingen av statiska filer för Django-programmet:</span><span class="sxs-lookup"><span data-stu-id="0faec-292">If you want tooskip collection of static files for your Django application:</span></span>

    \.skipDjango

<span data-ttu-id="0faec-293">Sedan behöver du toodo hello samlingen manuellt på den lokala datorn:</span><span class="sxs-lookup"><span data-stu-id="0faec-293">Then you'll need toodo hello collection manually on your local machine:</span></span>

    env\scripts\python manage.py collectstatic

<span data-ttu-id="0faec-294">Ta bort hello `\static` mapp från `.gitignore` och lägga till den toohello Git-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="0faec-294">Then remove hello `\static` folder from `.gitignore` and add it toohello Git repository.</span></span>

## <a name="troubleshooting---settings"></a><span data-ttu-id="0faec-295">Felsökning – inställningar</span><span class="sxs-lookup"><span data-stu-id="0faec-295">Troubleshooting - Settings</span></span>
<span data-ttu-id="0faec-296">Olika inställningar för programmet hello kan ändras i `DjangoWebProject/settings.py`.</span><span class="sxs-lookup"><span data-stu-id="0faec-296">Various settings for hello application can be changed in `DjangoWebProject/settings.py`.</span></span>

<span data-ttu-id="0faec-297">För enklare utveckling är felsökningsläget aktiverat.</span><span class="sxs-lookup"><span data-stu-id="0faec-297">For developer convenience, debug mode is enabled.</span></span> <span data-ttu-id="0faec-298">En bra bieffekt som är kommer du att kunna toosee bilder och annat statiskt innehåll vid lokal körning utan toocollect statiska filer.</span><span class="sxs-lookup"><span data-stu-id="0faec-298">One nice side effect of that is you'll be able toosee images and other static content when running locally, without having toocollect static files.</span></span>

<span data-ttu-id="0faec-299">toodisable felsökningsläget:</span><span class="sxs-lookup"><span data-stu-id="0faec-299">toodisable debug mode:</span></span>

    DEBUG = False

<span data-ttu-id="0faec-300">När felsökningen är inaktiverad hello värde för `ALLOWED_HOSTS` behov toobe uppdateras tooinclude hello Azure-värdnamnet.</span><span class="sxs-lookup"><span data-stu-id="0faec-300">When debug is disabled, hello value for `ALLOWED_HOSTS` needs toobe updated tooinclude hello Azure host name.</span></span> <span data-ttu-id="0faec-301">Exempel:</span><span class="sxs-lookup"><span data-stu-id="0faec-301">For example:</span></span>

    ALLOWED_HOSTS = (
        'pythonapp.azurewebsites.net',
    )

<span data-ttu-id="0faec-302">eller tooenable alla:</span><span class="sxs-lookup"><span data-stu-id="0faec-302">or tooenable any:</span></span>

    ALLOWED_HOSTS = (
        '*',
    )

<span data-ttu-id="0faec-303">I praktiken kan kanske du vill toodo något mer komplexa toodeal växlingen mellan felsöka och versionsläge och hämta hello-värdnamn.</span><span class="sxs-lookup"><span data-stu-id="0faec-303">In practice, you may want toodo something more complex toodeal with switching between debug and release mode, and getting hello host name.</span></span>

<span data-ttu-id="0faec-304">Du kan ange miljövariabler hello Azure-portalen **konfigurera** i hello sidan **appinställningar** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="0faec-304">You can set environment variables through hello Azure portal **CONFIGURE** page, in hello **app settings** section.</span></span>  <span data-ttu-id="0faec-305">Detta kan vara användbart för att ange värden som du inte kan ha tooappear i hello källorna (anslutningssträngar, lösenord osv) eller som du vill tooset annorlunda mellan Azure och den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="0faec-305">This can be useful for setting values that you may not want tooappear in hello sources (connection strings, passwords, etc), or that you want tooset differently between Azure and your local machine.</span></span> <span data-ttu-id="0faec-306">I `settings.py`, du kan fråga hello miljövariablerna med hjälp av `os.getenv`.</span><span class="sxs-lookup"><span data-stu-id="0faec-306">In `settings.py`, you can query hello environment variables using `os.getenv`.</span></span>

## <a name="using-a-database"></a><span data-ttu-id="0faec-307">Använda en databas</span><span class="sxs-lookup"><span data-stu-id="0faec-307">Using a Database</span></span>
<span data-ttu-id="0faec-308">hello-databasen som ingår i programmet hello är en sqlite-databas.</span><span class="sxs-lookup"><span data-stu-id="0faec-308">hello database that is included with hello application is a sqlite database.</span></span> <span data-ttu-id="0faec-309">Detta är en praktisk standard databasen toouse för utveckling, eftersom den kräver nästan några inställningar.</span><span class="sxs-lookup"><span data-stu-id="0faec-309">This is a convenient and useful default database toouse for development, as it requires almost no setup.</span></span> <span data-ttu-id="0faec-310">hello databasen lagras i hello DB.sqlite3 i projektmappen hello.</span><span class="sxs-lookup"><span data-stu-id="0faec-310">hello database is stored in hello db.sqlite3 file in hello project folder.</span></span>

<span data-ttu-id="0faec-311">Azure tillhandahåller databastjänster som är lätt toouse från ett Django-program.</span><span class="sxs-lookup"><span data-stu-id="0faec-311">Azure provides database services which are easy toouse from a Django application.</span></span> <span data-ttu-id="0faec-312">Självstudier för att använda [SQL-databas] och [MySQL] från ett Django-program visar hello steg nödvändiga toocreate hello databastjänsten, ändra databasinställningarna för hello i `DjangoWebProject/settings.py`, och hello bibliotek krävs tooinstall.</span><span class="sxs-lookup"><span data-stu-id="0faec-312">Tutorials for using [SQL Database] and [MySQL] from a Django application show hello steps necessary toocreate hello database service, change hello database settings in `DjangoWebProject/settings.py`, and hello libraries required tooinstall.</span></span>

<span data-ttu-id="0faec-313">Om du vill toomanage egna databasservrar kan göra du självklart det med hjälp av Windows eller Linux virtuella datorer som körs på Azure.</span><span class="sxs-lookup"><span data-stu-id="0faec-313">Of course, if you prefer toomanage your own database servers, you can do so using Windows or Linux virtual machines running on Azure.</span></span>

## <a name="django-admin-interface"></a><span data-ttu-id="0faec-314">Administratörsgränssnittet för Django</span><span class="sxs-lookup"><span data-stu-id="0faec-314">Django Admin Interface</span></span>
<span data-ttu-id="0faec-315">När du börjar bygga modeller vill du antagligen toopopulate hello databasen med vissa data.</span><span class="sxs-lookup"><span data-stu-id="0faec-315">Once you start building your models, you'll want toopopulate hello database with some data.</span></span> <span data-ttu-id="0faec-316">Ett enkelt sätt toodo lägga till och redigera innehåll interaktivt är toouse hello administratörsgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="0faec-316">An easy way toodo add and edit content interactively is toouse hello Django administration interface.</span></span>

<span data-ttu-id="0faec-317">hello-koden för hello administratörsgränssnittet har kommenterats ut i programkällorna hello, men den är tydligt markerad så att du enkelt kan aktivera den (Sök efter ”admin”).</span><span class="sxs-lookup"><span data-stu-id="0faec-317">hello code for hello admin interface is commented out in hello application sources, but it's clearly marked so you can easily enable it (search for 'admin').</span></span>

<span data-ttu-id="0faec-318">När den har aktiverats synkronisera hello databas, köra programmet hello och navigera för`/admin`.</span><span class="sxs-lookup"><span data-stu-id="0faec-318">After it's enabled, synchronize hello database, run hello application and navigate too`/admin`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0faec-319">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0faec-319">Next Steps</span></span>
<span data-ttu-id="0faec-320">Följ dessa länkar toolearn mer om Django och Python Tools för Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="0faec-320">Follow these links toolearn more about Django and Python Tools for Visual Studio:</span></span>

* <span data-ttu-id="0faec-321">[Django-dokumentation]</span><span class="sxs-lookup"><span data-stu-id="0faec-321">[Django Documentation]</span></span>
* <span data-ttu-id="0faec-322">[Python Tools för Visual Studio-dokumentationen]</span><span class="sxs-lookup"><span data-stu-id="0faec-322">[Python Tools for Visual Studio Documentation]</span></span>

<span data-ttu-id="0faec-323">Information om hur du använder SQL Database och MySQL finns i:</span><span class="sxs-lookup"><span data-stu-id="0faec-323">For information on using SQL Database and MySQL:</span></span>

* <span data-ttu-id="0faec-324">[Django och MySQL på Azure med Python Tools för Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="0faec-324">[Django and MySQL on Azure with Python Tools for Visual Studio]</span></span>
* <span data-ttu-id="0faec-325">[Django och SQL Database på Azure med Python Tools för Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="0faec-325">[Django and SQL Database on Azure with Python Tools for Visual Studio]</span></span>

<span data-ttu-id="0faec-326">Mer information finns i hello [Python Developer Center](/develop/python/).</span><span class="sxs-lookup"><span data-stu-id="0faec-326">For more information, see hello [Python Developer Center](/develop/python/).</span></span>

## <a name="whats-changed"></a><span data-ttu-id="0faec-327">Nyheter</span><span class="sxs-lookup"><span data-stu-id="0faec-327">What's changed</span></span>
* <span data-ttu-id="0faec-328">En guide toohello övergången från webbplatser tooApp tjänsten finns: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="0faec-328">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

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
