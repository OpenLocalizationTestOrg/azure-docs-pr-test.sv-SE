---
title: aaaPython web apps med Bottle i Azure
description: "En självstudiekurs som presenteras toorunning en Python-webbapp i Azure App Service Web Apps."
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
ms.openlocfilehash: 98acd7d8fcdbba326625121c20f9237d2663ea1e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="creating-web-apps-with-bottle-in-azure"></a><span data-ttu-id="7e83d-103">Skapa webbappar med Bottle i Azure</span><span class="sxs-lookup"><span data-stu-id="7e83d-103">Creating web apps with Bottle in Azure</span></span>
<span data-ttu-id="7e83d-104">Den här självstudiekursen beskrivs hur tooget igång med Python i Azure App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="7e83d-104">This tutorial describes how tooget started running Python in Azure App Service Web Apps.</span></span> <span data-ttu-id="7e83d-105">Med Web Apps får du begränsat ledigt värdutrymme och snabb distribution och kan använda Python.</span><span class="sxs-lookup"><span data-stu-id="7e83d-105">Web Apps provides limited free hosting and rapid deployment, and you can use Python!</span></span> <span data-ttu-id="7e83d-106">När din app växer, kan du växla toopaid värd och du kan också integrera med alla hello andra Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="7e83d-106">As your app grows, you can switch toopaid hosting, and you can also integrate with all of hello other Azure services.</span></span>

<span data-ttu-id="7e83d-107">Du skapar ett webbprogram med hjälp av hello Bottle webbramverk (se motsvarande versioner av kursen för [Django](web-sites-python-create-deploy-django-app.md) och [Flask](web-sites-python-create-deploy-flask-app.md)).</span><span class="sxs-lookup"><span data-stu-id="7e83d-107">You will create a web app using hello Bottle web framework (see alternate versions of this tutorial for [Django](web-sites-python-create-deploy-django-app.md) and [Flask](web-sites-python-create-deploy-flask-app.md)).</span></span> <span data-ttu-id="7e83d-108">Du ska skapa hello webbprogrammet från hello Azure Marketplace, konfigurera Git-distribution och klona hello lagringsplatsen lokalt.</span><span class="sxs-lookup"><span data-stu-id="7e83d-108">You will create hello web app from hello Azure Marketplace, set up Git deployment, and clone hello repository locally.</span></span> <span data-ttu-id="7e83d-109">Du ska köra hello webbprogram lokalt, gör ändringar, genomför och push-installera dem för[Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="7e83d-109">Then you will run hello web app locally, make changes, commit and push them too[Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span> <span data-ttu-id="7e83d-110">Hej självstudiekurs visar hur toodo från Windows eller Mac/Linux.</span><span class="sxs-lookup"><span data-stu-id="7e83d-110">hello tutorial shows how toodo this from Windows or Mac/Linux.</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> <span data-ttu-id="7e83d-111">Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service.</span><span class="sxs-lookup"><span data-stu-id="7e83d-111">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="7e83d-112">Inget kreditkort krävs, och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="7e83d-112">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="7e83d-113">Krav</span><span class="sxs-lookup"><span data-stu-id="7e83d-113">Prerequisites</span></span>
* <span data-ttu-id="7e83d-114">Windows, Mac eller Linux</span><span class="sxs-lookup"><span data-stu-id="7e83d-114">Windows, Mac or Linux</span></span>
* <span data-ttu-id="7e83d-115">Python 2.7 eller 3.4</span><span class="sxs-lookup"><span data-stu-id="7e83d-115">Python 2.7 or 3.4</span></span>
* <span data-ttu-id="7e83d-116">installationsverktyg, pip, virtuell miljö (endast Python 2.7)</span><span class="sxs-lookup"><span data-stu-id="7e83d-116">setuptools, pip, virtualenv (Python 2.7 only)</span></span>
* <span data-ttu-id="7e83d-117">Git</span><span class="sxs-lookup"><span data-stu-id="7e83d-117">Git</span></span>
* <span data-ttu-id="7e83d-118">[Python Tools 2.2 för Visual Studio][Python Tools 2.2 för Visual Studio] (PTVS) - Obs: det här är valfritt</span><span class="sxs-lookup"><span data-stu-id="7e83d-118">[Python Tools 2.2 for Visual Studio][Python Tools 2.2 for Visual Studio] (PTVS) - Note: this is optional</span></span>

<span data-ttu-id="7e83d-119">**Obs**! TFS-publicering stöds för närvarande inte för Python-projekt.</span><span class="sxs-lookup"><span data-stu-id="7e83d-119">**Note**: TFS publishing is currently not supported for Python projects.</span></span>

### <a name="windows"></a><span data-ttu-id="7e83d-120">Windows</span><span class="sxs-lookup"><span data-stu-id="7e83d-120">Windows</span></span>
<span data-ttu-id="7e83d-121">Om du inte redan har Python 2.7 eller 3.4 installerat (32-bitars) rekommenderar vi att du installerar [Azure SDK för Python 2.7] eller [Azure SDK för Python 3.4] med hjälp av installationsprogrammet för webbplattform.</span><span class="sxs-lookup"><span data-stu-id="7e83d-121">If you don't already have Python 2.7 or 3.4 installed (32-bit), we recommend installing [Azure SDK for Python 2.7] or [Azure SDK for Python 3.4] using Web Platform Installer.</span></span> <span data-ttu-id="7e83d-122">Detta installerar hello 32-bitarsversionen av Python, installationsverktyg, pip, virtuell miljö och så vidare (32-bitarsversionen av Python är vad som är installerat på hello Azure-värddatorerna).</span><span class="sxs-lookup"><span data-stu-id="7e83d-122">This installs hello 32-bit version of Python, setuptools, pip, virtualenv, etc (32-bit Python is what's installed on hello Azure host machines).</span></span> <span data-ttu-id="7e83d-123">Du kan också hämta Python på [python.org].</span><span class="sxs-lookup"><span data-stu-id="7e83d-123">Alternatively, you can get Python from [python.org].</span></span>

<span data-ttu-id="7e83d-124">För Git rekommenderar vi [Git för Windows] eller [GitHub för Windows].</span><span class="sxs-lookup"><span data-stu-id="7e83d-124">For Git, we recommend [Git for Windows] or [GitHub for Windows].</span></span> <span data-ttu-id="7e83d-125">Om du använder Visual Studio, kan du använda hello integrerad Git-stödet.</span><span class="sxs-lookup"><span data-stu-id="7e83d-125">If you use Visual Studio, you can use hello integrated Git support.</span></span>

<span data-ttu-id="7e83d-126">Vi rekommenderar också att du installerar [Python Tools 2.2 för Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="7e83d-126">We also recommend installing [Python Tools 2.2 for Visual Studio].</span></span> <span data-ttu-id="7e83d-127">Det här är valfritt, men om du har [Visual Studio], inklusive hello kostnadsfri Visual Studio Community 2013 eller Visual Studio Express 2013 för webben och det ger dig en utmärkt Python IDE.</span><span class="sxs-lookup"><span data-stu-id="7e83d-127">This is optional, but if you have [Visual Studio], including hello free Visual Studio Community 2013 or Visual Studio Express 2013 for Web, then this will give you a great Python IDE.</span></span>

### <a name="maclinux"></a><span data-ttu-id="7e83d-128">Mac/Linux</span><span class="sxs-lookup"><span data-stu-id="7e83d-128">Mac/Linux</span></span>
<span data-ttu-id="7e83d-129">Du bör redan ha Python och Git installerade, men kontrollera att du har Python 2.7 eller 3.4.</span><span class="sxs-lookup"><span data-stu-id="7e83d-129">You should have Python and Git already installed, but make sure you have either Python 2.7 or 3.4.</span></span>

## <a name="web-app-creation-on-hello-azure-portal"></a><span data-ttu-id="7e83d-130">Skapa en webbapp i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="7e83d-130">Web app creation on hello Azure Portal</span></span>
<span data-ttu-id="7e83d-131">hello första steget i att skapa din app är toocreate hello webbprogram via hello [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7e83d-131">hello first step in creating your app is toocreate hello web app via hello [Azure Portal](https://portal.azure.com).</span></span>  

1. <span data-ttu-id="7e83d-132">Logga in på hello Azure-portalen och klicka på hello **ny** knappen i hello nedre vänstra hörnet.</span><span class="sxs-lookup"><span data-stu-id="7e83d-132">Log into hello Azure Portal and click hello **NEW** button in hello bottom left corner.</span></span> 
2. <span data-ttu-id="7e83d-133">Skriv ”python” i sökrutan hello.</span><span class="sxs-lookup"><span data-stu-id="7e83d-133">In hello search box, type "python".</span></span>
3. <span data-ttu-id="7e83d-134">I sökresultaten hello väljer **Bottle**, klicka på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="7e83d-134">In hello search results, select **Bottle**, then click **Create**.</span></span>
4. <span data-ttu-id="7e83d-135">Konfigurera hello nya Bottle appen, till exempel skapa en ny App Service-plan och en ny resursgrupp för den.</span><span class="sxs-lookup"><span data-stu-id="7e83d-135">Configure hello new Bottle app, such as creating a new App Service plan and a new resource group for it.</span></span> <span data-ttu-id="7e83d-136">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="7e83d-136">Then, click **Create**.</span></span>
5. <span data-ttu-id="7e83d-137">Konfigurera Git-publicering för nyskapade webbappen genom att följa anvisningarna hello på [lokal Git-distribution tooAzure Apptjänst](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="7e83d-137">Configure Git publishing for your newly created web app by following hello instructions at [Local Git Deployment tooAzure App Service](app-service-deploy-local-git.md).</span></span>

## <a name="application-overview"></a><span data-ttu-id="7e83d-138">Programöversikt</span><span class="sxs-lookup"><span data-stu-id="7e83d-138">Application Overview</span></span>
### <a name="git-repository-contents"></a><span data-ttu-id="7e83d-139">Innehåll på Git-lagringsplatsen</span><span class="sxs-lookup"><span data-stu-id="7e83d-139">Git repository contents</span></span>
<span data-ttu-id="7e83d-140">Här är en översikt över hello-filer som du hittar i hello första Git-lagringsplatsen, som vi ska klona i nästa avsnitt om hello.</span><span class="sxs-lookup"><span data-stu-id="7e83d-140">Here's an overview of hello files you'll find in hello initial Git repository, which we'll clone in hello next section.</span></span>

    \routes.py
    \static\content\
    \static\fonts\
    \static\scripts\
    \views\about.tpl
    \views\contact.tpl
    \views\index.tpl
    \views\layout.tpl

<span data-ttu-id="7e83d-141">Programmet hello huvudsakliga källor.</span><span class="sxs-lookup"><span data-stu-id="7e83d-141">Main sources for hello application.</span></span> <span data-ttu-id="7e83d-142">Består av tre sidor (index, om och kontakt) med bakgrundslayout.</span><span class="sxs-lookup"><span data-stu-id="7e83d-142">Consists of 3 pages (index, about, contact) with a master layout.</span></span>  <span data-ttu-id="7e83d-143">Statiskt innehåll och skript inkluderar bootstrap, jquery, modernizr och respond.</span><span class="sxs-lookup"><span data-stu-id="7e83d-143">Static content and scripts include bootstrap, jquery, modernizr and respond.</span></span>

    \app.py

<span data-ttu-id="7e83d-144">Stöd för lokal utveckling server.</span><span class="sxs-lookup"><span data-stu-id="7e83d-144">Local development server support.</span></span> <span data-ttu-id="7e83d-145">Använda toorun hello programmet lokalt.</span><span class="sxs-lookup"><span data-stu-id="7e83d-145">Use this toorun hello application locally.</span></span>

    \BottleWebProject.pyproj
    \BottleWebProject.sln

<span data-ttu-id="7e83d-146">Projektfiler för användning med [Python Tools för Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="7e83d-146">Project files for use with [Python Tools for Visual Studio].</span></span>

    \ptvs_virtualenv_proxy.py

<span data-ttu-id="7e83d-147">IIS-proxy för virtuella miljöer och PTVS-stöd för fjärrfelsökning.</span><span class="sxs-lookup"><span data-stu-id="7e83d-147">IIS proxy for virtual environments and PTVS remote debugging support.</span></span>

    \requirements.txt

<span data-ttu-id="7e83d-148">Externa paket som krävs för programmet.</span><span class="sxs-lookup"><span data-stu-id="7e83d-148">External packages needed by this application.</span></span> <span data-ttu-id="7e83d-149">hello distributionsskriptet pip hello installationspaket som anges i den här filen.</span><span class="sxs-lookup"><span data-stu-id="7e83d-149">hello deployment script will pip install hello packages listed in this file.</span></span>

    \web.2.7.config
    \web.3.4.config

<span data-ttu-id="7e83d-150">IIS-konfigurationsfiler.</span><span class="sxs-lookup"><span data-stu-id="7e83d-150">IIS configuration files.</span></span> <span data-ttu-id="7e83d-151">hello distributionsskriptet använder lämplig web.x.y.config för hello och kopian web.config.</span><span class="sxs-lookup"><span data-stu-id="7e83d-151">hello deployment script will use hello appropriate web.x.y.config and copy it as web.config.</span></span>

### <a name="optional-files---customizing-deployment"></a><span data-ttu-id="7e83d-152">Valfria filer – anpassa distributionen</span><span class="sxs-lookup"><span data-stu-id="7e83d-152">Optional files - Customizing deployment</span></span>
[!INCLUDE [web-sites-python-customizing-deployment](../../includes/web-sites-python-customizing-deployment.md)]

### <a name="optional-files---python-runtime"></a><span data-ttu-id="7e83d-153">Valfria filer – Python-körning</span><span class="sxs-lookup"><span data-stu-id="7e83d-153">Optional files - Python runtime</span></span>
[!INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

### <a name="additional-files-on-server"></a><span data-ttu-id="7e83d-154">Ytterligare filer på servern</span><span class="sxs-lookup"><span data-stu-id="7e83d-154">Additional files on server</span></span>
<span data-ttu-id="7e83d-155">Vissa filer finnas på hello-servern men läggs inte toohello git-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="7e83d-155">Some files exist on hello server but are not added toohello git repository.</span></span> <span data-ttu-id="7e83d-156">Dessa skapas med hello distributionsskriptet.</span><span class="sxs-lookup"><span data-stu-id="7e83d-156">These are created by hello deployment script.</span></span>

    \web.config

<span data-ttu-id="7e83d-157">IIS-konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="7e83d-157">IIS configuration file.</span></span> <span data-ttu-id="7e83d-158">Skapas utifrån web.x.y.config vid varje distribution.</span><span class="sxs-lookup"><span data-stu-id="7e83d-158">Created from web.x.y.config on every deployment.</span></span>

    \env\

<span data-ttu-id="7e83d-159">Python, virtuell miljö</span><span class="sxs-lookup"><span data-stu-id="7e83d-159">Python virtual environment.</span></span> <span data-ttu-id="7e83d-160">Skapas under distributionen om det inte redan finns en kompatibel virtuell miljö hello webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="7e83d-160">Created during deployment if a compatible virtual environment doesn't already exist on hello web app.</span></span>  <span data-ttu-id="7e83d-161">Paket som anges i requirements.txt är pip-installeras, men pip hoppar över installationen om paketen hello redan är installerade.</span><span class="sxs-lookup"><span data-stu-id="7e83d-161">Packages listed in requirements.txt are pip installed, but pip will skip installation if hello packages are already installed.</span></span>

<span data-ttu-id="7e83d-162">hello 3 nästa avsnitt beskrivs hur tooproceed med hello webbappsutveckling under 3 olika miljöer:</span><span class="sxs-lookup"><span data-stu-id="7e83d-162">hello next 3 sections describe how tooproceed with hello web app development under 3 different environments:</span></span>

* <span data-ttu-id="7e83d-163">Windows med Python Tools för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7e83d-163">Windows, with Python Tools for Visual Studio</span></span>
* <span data-ttu-id="7e83d-164">Windows, med kommandorad</span><span class="sxs-lookup"><span data-stu-id="7e83d-164">Windows, with command line</span></span>
* <span data-ttu-id="7e83d-165">Mac/Linux, med kommandorad</span><span class="sxs-lookup"><span data-stu-id="7e83d-165">Mac/Linux, with command line</span></span>

## <a name="web-app-development---windows---python-tools-for-visual-studio"></a><span data-ttu-id="7e83d-166">Webbappsutveckling – Windows – Python Tools för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7e83d-166">Web App development - Windows - Python Tools for Visual Studio</span></span>
### <a name="clone-hello-repository"></a><span data-ttu-id="7e83d-167">Klona hello databasen</span><span class="sxs-lookup"><span data-stu-id="7e83d-167">Clone hello repository</span></span>
<span data-ttu-id="7e83d-168">Först klonar hello-databas med hjälp av hello webbadressen som tillhandahålls på hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7e83d-168">First, clone hello repository using hello url provided on hello Azure Portal.</span></span> <span data-ttu-id="7e83d-169">Mer information finns i [lokal Git-distribution tooAzure Apptjänst](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="7e83d-169">For more information, see [Local Git Deployment tooAzure App Service](app-service-deploy-local-git.md).</span></span>

<span data-ttu-id="7e83d-170">Öppna hello lösningsfilen (.sln) som ingår i hello rot hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="7e83d-170">Open hello solution file (.sln) that is included in hello root of hello repository.</span></span>

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-solution-bottle.png)

### <a name="create-virtual-environment"></a><span data-ttu-id="7e83d-171">Skapa en virtuell miljö</span><span class="sxs-lookup"><span data-stu-id="7e83d-171">Create virtual environment</span></span>
<span data-ttu-id="7e83d-172">Nu ska vi skapa en virtuell miljö för lokal utveckling.</span><span class="sxs-lookup"><span data-stu-id="7e83d-172">Now we'll create a virtual environment for local development.</span></span> <span data-ttu-id="7e83d-173">Högerklicka på **Python Environments** (Python-miljöer) och välj **Add Virtual Environment...** (Lägg till virtuell miljö).</span><span class="sxs-lookup"><span data-stu-id="7e83d-173">Right-click on **Python Environments** select **Add Virtual Environment...**.</span></span>

* <span data-ttu-id="7e83d-174">Kontrollera hello hello miljö heter `env`.</span><span class="sxs-lookup"><span data-stu-id="7e83d-174">Make sure hello name of hello environment is `env`.</span></span>
* <span data-ttu-id="7e83d-175">Välj hello bastolk.</span><span class="sxs-lookup"><span data-stu-id="7e83d-175">Select hello base interpreter.</span></span> <span data-ttu-id="7e83d-176">Kontrollera att toouse hello samma version av Python som valts för webbappen (i runtime.txt eller hello **programinställningar** bladet för din webbapp i hello Azure Portal).</span><span class="sxs-lookup"><span data-stu-id="7e83d-176">Make sure toouse hello same version of Python that is selected for your web app (in runtime.txt or hello **Application Settings** blade of your web app in hello Azure Portal).</span></span>
* <span data-ttu-id="7e83d-177">Kontrollera att hello alternativet toodownload och installera paket är markerat.</span><span class="sxs-lookup"><span data-stu-id="7e83d-177">Make sure hello option toodownload and install packages is checked.</span></span>

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-add-virtual-env-27.png)

<span data-ttu-id="7e83d-178">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="7e83d-178">Click **Create**.</span></span> <span data-ttu-id="7e83d-179">Detta skapar hello virtuell miljö och installera beroenden som anges i requirements.txt.</span><span class="sxs-lookup"><span data-stu-id="7e83d-179">This will create hello virtual environment, and install dependencies listed in requirements.txt.</span></span>

### <a name="run-using-development-server"></a><span data-ttu-id="7e83d-180">Kör med utvecklingsservern</span><span class="sxs-lookup"><span data-stu-id="7e83d-180">Run using development server</span></span>
<span data-ttu-id="7e83d-181">Tryck på F5 toostart felsökning och webbläsaren öppnas automatiskt toohello sidan som körs lokalt.</span><span class="sxs-lookup"><span data-stu-id="7e83d-181">Press F5 toostart debugging, and your web browser will open automatically toohello page running locally.</span></span>

![](./media/web-sites-python-create-deploy-bottle-app/windows-browser-bottle.png)

<span data-ttu-id="7e83d-182">Du kan ange brytpunkter i hello källor, Använd hello titta på windows osv. Se hello [Python Tools för Visual Studio-dokumentationen] hello mer information om olika funktioner.</span><span class="sxs-lookup"><span data-stu-id="7e83d-182">You can set breakpoints in hello sources, use hello watch windows, etc. See hello [Python Tools for Visual Studio Documentation] for more information on hello various features.</span></span>

### <a name="make-changes"></a><span data-ttu-id="7e83d-183">Göra ändringar</span><span class="sxs-lookup"><span data-stu-id="7e83d-183">Make changes</span></span>
<span data-ttu-id="7e83d-184">Nu kan du experimentera med att göra ändringar toohello programmets källor och mallar.</span><span class="sxs-lookup"><span data-stu-id="7e83d-184">Now you can experiment by making changes toohello application sources and/or templates.</span></span>

<span data-ttu-id="7e83d-185">När du har testat ändringarna checkar du in dem toohello Git-lagringsplatsen:</span><span class="sxs-lookup"><span data-stu-id="7e83d-185">After you've tested your changes, commit them toohello Git repository:</span></span>

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-commit-bottle.png)

### <a name="install-more-packages"></a><span data-ttu-id="7e83d-186">Installera fler paket</span><span class="sxs-lookup"><span data-stu-id="7e83d-186">Install more packages</span></span>
<span data-ttu-id="7e83d-187">Programmet kan ha beroenden utöver Python och Bottle.</span><span class="sxs-lookup"><span data-stu-id="7e83d-187">Your application may have dependencies beyond Python and Bottle.</span></span>

<span data-ttu-id="7e83d-188">Du kan installera ytterligare paket med hjälp av pip.</span><span class="sxs-lookup"><span data-stu-id="7e83d-188">You can install additional packages using pip.</span></span> <span data-ttu-id="7e83d-189">tooinstall ett paket, högerklicka på hello virtuell miljö och välj **Install Python Package**.</span><span class="sxs-lookup"><span data-stu-id="7e83d-189">tooinstall a package, right-click on hello virtual environment and select **Install Python Package**.</span></span>

<span data-ttu-id="7e83d-190">Till exempel tooinstall hello Azure SDK för Python, som ger dig åtkomst tooAzure storage, service bus och andra Azure-tjänster, ange `azure`:</span><span class="sxs-lookup"><span data-stu-id="7e83d-190">For example, tooinstall hello Azure SDK for Python, which gives you access tooAzure storage, service bus and other Azure services, enter `azure`:</span></span>

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-install-package-dialog.png)

<span data-ttu-id="7e83d-191">Högerklicka på hello virtuell miljö och välj **generera requirements.txt** tooupdate requirements.txt.</span><span class="sxs-lookup"><span data-stu-id="7e83d-191">Right-click on hello virtual environment and select **Generate requirements.txt** tooupdate requirements.txt.</span></span>

<span data-ttu-id="7e83d-192">Checka sedan in hello ändringar toorequirements.txt toohello Git-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="7e83d-192">Then, commit hello changes toorequirements.txt toohello Git repository.</span></span>

### <a name="deploy-tooazure"></a><span data-ttu-id="7e83d-193">Distribuera tooAzure</span><span class="sxs-lookup"><span data-stu-id="7e83d-193">Deploy tooAzure</span></span>
<span data-ttu-id="7e83d-194">tootrigger en distribution, klicka på **Sync** eller **Push**.</span><span class="sxs-lookup"><span data-stu-id="7e83d-194">tootrigger a deployment, click on **Sync** or **Push**.</span></span> <span data-ttu-id="7e83d-195">Med synkronisering görs både en överföring och en hämtning.</span><span class="sxs-lookup"><span data-stu-id="7e83d-195">Sync does both a push and a pull.</span></span>

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-git-push.png)

<span data-ttu-id="7e83d-196">hello första distributionen tar en stund, eftersom den skapar en virtuell miljö, paket installeras osv.</span><span class="sxs-lookup"><span data-stu-id="7e83d-196">hello first deployment will take some time, as it will create a virtual environment, install packages, etc.</span></span>

<span data-ttu-id="7e83d-197">Visual Studio visas inte hello fortskrider hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="7e83d-197">Visual Studio doesn't show hello progress of hello deployment.</span></span> <span data-ttu-id="7e83d-198">Om du vill att tooreview hello utdata avsnittet hello på [felsökning – distribution](#troubleshooting-deployment).</span><span class="sxs-lookup"><span data-stu-id="7e83d-198">If you'd like tooreview hello output, see hello section on [Troubleshooting - Deployment](#troubleshooting-deployment).</span></span>

<span data-ttu-id="7e83d-199">Bläddra toohello Azure URL tooview ändringarna.</span><span class="sxs-lookup"><span data-stu-id="7e83d-199">Browse toohello Azure URL tooview your changes.</span></span>

## <a name="web-app-development---windows---command-line"></a><span data-ttu-id="7e83d-200">Webbappsutvecklingen - Windows - kommandot rad</span><span class="sxs-lookup"><span data-stu-id="7e83d-200">Web app development - Windows - Command Line</span></span>
### <a name="clone-hello-repository"></a><span data-ttu-id="7e83d-201">Klona hello databasen</span><span class="sxs-lookup"><span data-stu-id="7e83d-201">Clone hello repository</span></span>
<span data-ttu-id="7e83d-202">Först klonar hello lagringsplatsen med hjälp av hello Webbadressen som tillhandahålls på hello Azure-portalen och lägga till hello Azure-lagringsplatsen som fjärransluten.</span><span class="sxs-lookup"><span data-stu-id="7e83d-202">First, clone hello repository using hello URL provided on hello Azure Portal, and add hello Azure repository as a remote.</span></span> <span data-ttu-id="7e83d-203">Mer information finns i [lokal Git-distribution tooAzure Apptjänst](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="7e83d-203">For more information, see [Local Git Deployment tooAzure App Service](app-service-deploy-local-git.md).</span></span>

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url> 

### <a name="create-virtual-environment"></a><span data-ttu-id="7e83d-204">Skapa en virtuell miljö</span><span class="sxs-lookup"><span data-stu-id="7e83d-204">Create virtual environment</span></span>
<span data-ttu-id="7e83d-205">Vi ska skapa en ny virtuell miljö för utveckling (inte Lägg till den toohello databasen).</span><span class="sxs-lookup"><span data-stu-id="7e83d-205">We'll create a new virtual environment for development purposes (do not add it toohello repository).</span></span> <span data-ttu-id="7e83d-206">Virtuella miljöer i Python är inte relokerbara, så alla utvecklare som arbetar på hello programmet skapa sina egna lokalt.</span><span class="sxs-lookup"><span data-stu-id="7e83d-206">Virtual environments in Python are not relocatable, so every developer working on hello application will create their own locally.</span></span>

<span data-ttu-id="7e83d-207">Kontrollera att toouse hello samma version av Python som valts för webbappen (i runtime.txt eller hello bladet programinställningar för webbappen i hello Azure Portal)</span><span class="sxs-lookup"><span data-stu-id="7e83d-207">Make sure toouse hello same version of Python that is selected for your web app (in runtime.txt or hello Application Settings blade for your web app in hello Azure Portal)</span></span>

<span data-ttu-id="7e83d-208">För Python 2.7:</span><span class="sxs-lookup"><span data-stu-id="7e83d-208">For Python 2.7:</span></span>

    c:\python27\python.exe -m virtualenv env

<span data-ttu-id="7e83d-209">För Python 3.4:</span><span class="sxs-lookup"><span data-stu-id="7e83d-209">For Python 3.4:</span></span>

    c:\python34\python.exe -m venv env

<span data-ttu-id="7e83d-210">Installera eventuella externa paket som krävs för programmet.</span><span class="sxs-lookup"><span data-stu-id="7e83d-210">Install any external packages required by your application.</span></span> <span data-ttu-id="7e83d-211">Du kan använda hello requirements.txt filen i hello rot hello databasen tooinstall hello paket i den virtuella miljön:</span><span class="sxs-lookup"><span data-stu-id="7e83d-211">You can use hello requirements.txt file at hello root of hello repository tooinstall hello packages in your virtual environment:</span></span>

    env\scripts\pip install -r requirements.txt

### <a name="run-using-development-server"></a><span data-ttu-id="7e83d-212">Kör med utvecklingsservern</span><span class="sxs-lookup"><span data-stu-id="7e83d-212">Run using development server</span></span>
<span data-ttu-id="7e83d-213">Du kan starta programmet hello under en utvecklingsserver med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="7e83d-213">You can launch hello application under a development server with hello following command:</span></span>

    env\scripts\python app.py

<span data-ttu-id="7e83d-214">hello konsolen visar hello URL-adress och port hello servern lyssnar på:</span><span class="sxs-lookup"><span data-stu-id="7e83d-214">hello console will display hello URL and port hello server listens to:</span></span>

![](./media/web-sites-python-create-deploy-bottle-app/windows-run-local-bottle.png)

<span data-ttu-id="7e83d-215">Öppna din Webbadress webbläsare toothat.</span><span class="sxs-lookup"><span data-stu-id="7e83d-215">Then, open your web browser toothat URL.</span></span>

![](./media/web-sites-python-create-deploy-bottle-app/windows-browser-bottle.png)

### <a name="make-changes"></a><span data-ttu-id="7e83d-216">Göra ändringar</span><span class="sxs-lookup"><span data-stu-id="7e83d-216">Make changes</span></span>
<span data-ttu-id="7e83d-217">Nu kan du experimentera med att göra ändringar toohello programmets källor och mallar.</span><span class="sxs-lookup"><span data-stu-id="7e83d-217">Now you can experiment by making changes toohello application sources and/or templates.</span></span>

<span data-ttu-id="7e83d-218">När du har testat ändringarna checkar du in dem toohello Git-lagringsplatsen:</span><span class="sxs-lookup"><span data-stu-id="7e83d-218">After you've tested your changes, commit them toohello Git repository:</span></span>

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a><span data-ttu-id="7e83d-219">Installera fler paket</span><span class="sxs-lookup"><span data-stu-id="7e83d-219">Install more packages</span></span>
<span data-ttu-id="7e83d-220">Programmet kan ha beroenden utöver Python och Bottle.</span><span class="sxs-lookup"><span data-stu-id="7e83d-220">Your application may have dependencies beyond Python and Bottle.</span></span>

<span data-ttu-id="7e83d-221">Du kan installera ytterligare paket med hjälp av pip.</span><span class="sxs-lookup"><span data-stu-id="7e83d-221">You can install additional packages using pip.</span></span> <span data-ttu-id="7e83d-222">Till exempel tooinstall hello Azure SDK för Python, som ger dig åtkomst till tooAzure storage, service bus och andra Azure-tjänster, typ:</span><span class="sxs-lookup"><span data-stu-id="7e83d-222">For example, tooinstall hello Azure SDK for Python, which gives you access tooAzure storage, service bus and other Azure services, type:</span></span>

    env\scripts\pip install azure

<span data-ttu-id="7e83d-223">Se till att tooupdate requirements.txt:</span><span class="sxs-lookup"><span data-stu-id="7e83d-223">Make sure tooupdate requirements.txt:</span></span>

    env\scripts\pip freeze > requirements.txt

<span data-ttu-id="7e83d-224">Genomför hello ändringar:</span><span class="sxs-lookup"><span data-stu-id="7e83d-224">Commit hello changes:</span></span>

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-tooazure"></a><span data-ttu-id="7e83d-225">Distribuera tooAzure</span><span class="sxs-lookup"><span data-stu-id="7e83d-225">Deploy tooAzure</span></span>
<span data-ttu-id="7e83d-226">tootrigger en distribution push hello ändrar tooAzure:</span><span class="sxs-lookup"><span data-stu-id="7e83d-226">tootrigger a deployment, push hello changes tooAzure:</span></span>

    git push azure master

<span data-ttu-id="7e83d-227">Du ser hello utdata från hello distributionsskriptet, inklusive skapande av virtuell miljö, installation av paket och skapande av web.config.</span><span class="sxs-lookup"><span data-stu-id="7e83d-227">You will see hello output of hello deployment script, including virtual environment creation, installation of packages, creation of web.config.</span></span>

<span data-ttu-id="7e83d-228">Bläddra toohello Azure URL tooview ändringarna.</span><span class="sxs-lookup"><span data-stu-id="7e83d-228">Browse toohello Azure URL tooview your changes.</span></span>

## <a name="web-app-development---maclinux---command-line"></a><span data-ttu-id="7e83d-229">Webbappsutveckling – Mac/Linux – kommandorad</span><span class="sxs-lookup"><span data-stu-id="7e83d-229">Web app development - Mac/Linux - command line</span></span>
### <a name="clone-hello-repository"></a><span data-ttu-id="7e83d-230">Klona hello databasen</span><span class="sxs-lookup"><span data-stu-id="7e83d-230">Clone hello repository</span></span>
<span data-ttu-id="7e83d-231">Först klonar hello lagringsplatsen med hjälp av hello Webbadressen som tillhandahålls på hello Azure-portalen och lägga till hello Azure-lagringsplatsen som fjärransluten.</span><span class="sxs-lookup"><span data-stu-id="7e83d-231">First, clone hello repository using hello URL provided on hello Azure Portal, and add hello Azure repository as a remote.</span></span> <span data-ttu-id="7e83d-232">Mer information finns i [lokal Git-distribution tooAzure Apptjänst](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="7e83d-232">For more information, see [Local Git Deployment tooAzure App Service](app-service-deploy-local-git.md).</span></span>

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url> 

### <a name="create-virtual-environment"></a><span data-ttu-id="7e83d-233">Skapa en virtuell miljö</span><span class="sxs-lookup"><span data-stu-id="7e83d-233">Create virtual environment</span></span>
<span data-ttu-id="7e83d-234">Vi ska skapa en ny virtuell miljö för utveckling (inte Lägg till den toohello databasen).</span><span class="sxs-lookup"><span data-stu-id="7e83d-234">We'll create a new virtual environment for development purposes (do not add it toohello repository).</span></span> <span data-ttu-id="7e83d-235">Virtuella miljöer i Python är inte relokerbara, så alla utvecklare som arbetar på hello programmet skapa sina egna lokalt.</span><span class="sxs-lookup"><span data-stu-id="7e83d-235">Virtual environments in Python are not relocatable, so every developer working on hello application will create their own locally.</span></span>

<span data-ttu-id="7e83d-236">Kontrollera att toouse hello samma version av Python som valts för webbappen (i runtime.txt eller hello bladet programinställningar för webbappen i hello Azure Portal).</span><span class="sxs-lookup"><span data-stu-id="7e83d-236">Make sure toouse hello same version of Python that is selected for your web app (in runtime.txt or hello Application Settings blade of your web app in hello Azure Portal).</span></span>

<span data-ttu-id="7e83d-237">För Python 2.7:</span><span class="sxs-lookup"><span data-stu-id="7e83d-237">For Python 2.7:</span></span>

    python -m virtualenv env

<span data-ttu-id="7e83d-238">För Python 3.4:</span><span class="sxs-lookup"><span data-stu-id="7e83d-238">For Python 3.4:</span></span>

    python -m venv env
<span data-ttu-id="7e83d-239">eller pyvenv env</span><span class="sxs-lookup"><span data-stu-id="7e83d-239">or pyvenv env</span></span>

<span data-ttu-id="7e83d-240">Installera eventuella externa paket som krävs för programmet.</span><span class="sxs-lookup"><span data-stu-id="7e83d-240">Install any external packages required by your application.</span></span> <span data-ttu-id="7e83d-241">Du kan använda hello requirements.txt filen i hello rot hello databasen tooinstall hello paket i den virtuella miljön:</span><span class="sxs-lookup"><span data-stu-id="7e83d-241">You can use hello requirements.txt file at hello root of hello repository tooinstall hello packages in your virtual environment:</span></span>

    env/bin/pip install -r requirements.txt

### <a name="run-using-development-server"></a><span data-ttu-id="7e83d-242">Kör med utvecklingsservern</span><span class="sxs-lookup"><span data-stu-id="7e83d-242">Run using development server</span></span>
<span data-ttu-id="7e83d-243">Du kan starta programmet hello under en utvecklingsserver med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="7e83d-243">You can launch hello application under a development server with hello following command:</span></span>

    env/bin/python app.py

<span data-ttu-id="7e83d-244">hello konsolen visar hello URL-adress och port hello servern lyssnar på:</span><span class="sxs-lookup"><span data-stu-id="7e83d-244">hello console will display hello URL and port hello server listens to:</span></span>

![](./media/web-sites-python-create-deploy-bottle-app/mac-run-local-bottle.png)

<span data-ttu-id="7e83d-245">Öppna din Webbadress webbläsare toothat.</span><span class="sxs-lookup"><span data-stu-id="7e83d-245">Then, open your web browser toothat URL.</span></span>

![](./media/web-sites-python-create-deploy-bottle-app/mac-browser-bottle.png)

### <a name="make-changes"></a><span data-ttu-id="7e83d-246">Göra ändringar</span><span class="sxs-lookup"><span data-stu-id="7e83d-246">Make changes</span></span>
<span data-ttu-id="7e83d-247">Nu kan du experimentera med att göra ändringar toohello programmets källor och mallar.</span><span class="sxs-lookup"><span data-stu-id="7e83d-247">Now you can experiment by making changes toohello application sources and/or templates.</span></span>

<span data-ttu-id="7e83d-248">När du har testat ändringarna checkar du in dem toohello Git-lagringsplatsen:</span><span class="sxs-lookup"><span data-stu-id="7e83d-248">After you've tested your changes, commit them toohello Git repository:</span></span>

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a><span data-ttu-id="7e83d-249">Installera fler paket</span><span class="sxs-lookup"><span data-stu-id="7e83d-249">Install more packages</span></span>
<span data-ttu-id="7e83d-250">Programmet kan ha beroenden utöver Python och Bottle.</span><span class="sxs-lookup"><span data-stu-id="7e83d-250">Your application may have dependencies beyond Python and Bottle.</span></span>

<span data-ttu-id="7e83d-251">Du kan installera ytterligare paket med hjälp av pip.</span><span class="sxs-lookup"><span data-stu-id="7e83d-251">You can install additional packages using pip.</span></span> <span data-ttu-id="7e83d-252">Till exempel tooinstall hello Azure SDK för Python, som ger dig åtkomst till tooAzure storage, service bus och andra Azure-tjänster, typ:</span><span class="sxs-lookup"><span data-stu-id="7e83d-252">For example, tooinstall hello Azure SDK for Python, which gives you access tooAzure storage, service bus and other Azure services, type:</span></span>

    env/bin/pip install azure

<span data-ttu-id="7e83d-253">Se till att tooupdate requirements.txt:</span><span class="sxs-lookup"><span data-stu-id="7e83d-253">Make sure tooupdate requirements.txt:</span></span>

    env/bin/pip freeze > requirements.txt

<span data-ttu-id="7e83d-254">Genomför hello ändringar:</span><span class="sxs-lookup"><span data-stu-id="7e83d-254">Commit hello changes:</span></span>

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-tooazure"></a><span data-ttu-id="7e83d-255">Distribuera tooAzure</span><span class="sxs-lookup"><span data-stu-id="7e83d-255">Deploy tooAzure</span></span>
<span data-ttu-id="7e83d-256">tootrigger en distribution push hello ändrar tooAzure:</span><span class="sxs-lookup"><span data-stu-id="7e83d-256">tootrigger a deployment, push hello changes tooAzure:</span></span>

    git push azure master

<span data-ttu-id="7e83d-257">Du ser hello utdata från hello distributionsskriptet, inklusive skapande av virtuell miljö, installation av paket och skapande av web.config.</span><span class="sxs-lookup"><span data-stu-id="7e83d-257">You will see hello output of hello deployment script, including virtual environment creation, installation of packages, creation of web.config.</span></span>

<span data-ttu-id="7e83d-258">Bläddra toohello Azure URL tooview ändringarna.</span><span class="sxs-lookup"><span data-stu-id="7e83d-258">Browse toohello Azure URL tooview your changes.</span></span>

## <a name="troubleshooting---package-installation"></a><span data-ttu-id="7e83d-259">Felsökning – installation av paket</span><span class="sxs-lookup"><span data-stu-id="7e83d-259">Troubleshooting - Package Installation</span></span>
[!INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]

## <a name="troubleshooting---virtual-environment"></a><span data-ttu-id="7e83d-260">Felsökning – virtuell miljö</span><span class="sxs-lookup"><span data-stu-id="7e83d-260">Troubleshooting - Virtual Environment</span></span>
[!INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]

## <a name="next-steps"></a><span data-ttu-id="7e83d-261">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7e83d-261">Next Steps</span></span>
<span data-ttu-id="7e83d-262">Följ dessa länkar toolearn mer om Bottle och Python Tools för Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="7e83d-262">Follow these links toolearn more about Bottle and Python Tools for Visual Studio:</span></span> 

* <span data-ttu-id="7e83d-263">[Bottle dokumentation]</span><span class="sxs-lookup"><span data-stu-id="7e83d-263">[Bottle Documentation]</span></span>
* <span data-ttu-id="7e83d-264">[Python Tools för Visual Studio-dokumentationen]</span><span class="sxs-lookup"><span data-stu-id="7e83d-264">[Python Tools for Visual Studio Documentation]</span></span>

<span data-ttu-id="7e83d-265">Information om hur du använder Azure Table Storage och MongoDB:</span><span class="sxs-lookup"><span data-stu-id="7e83d-265">For information on using Azure Table Storage and MongoDB:</span></span>

* <span data-ttu-id="7e83d-266">[Bottle och MongoDB på Azure med Python Tools för Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="7e83d-266">[Bottle and MongoDB on Azure with Python Tools for Visual Studio]</span></span>
* <span data-ttu-id="7e83d-267">[Bottle och Azure Table Storage på Azure med Python Tools för Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="7e83d-267">[Bottle and Azure Table Storage on Azure with Python Tools for Visual Studio]</span></span>

## <a name="whats-changed"></a><span data-ttu-id="7e83d-268">Nyheter</span><span class="sxs-lookup"><span data-stu-id="7e83d-268">What's changed</span></span>
* <span data-ttu-id="7e83d-269">En guide toohello övergången från webbplatser tooApp tjänsten finns: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="7e83d-269">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

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
[Python Tools för Visual Studio-dokumentationen]: http://aka.ms/ptvsdocs 
[Bottle dokumentation]: http://bottlepy.org/docs/dev/index.html

