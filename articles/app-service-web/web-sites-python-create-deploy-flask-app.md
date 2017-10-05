---
title: Skapa webbappar med Flask i Azure
description: "En självstudiekurs som ger en introducerar till att köra en Python-webbapp på Azure."
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
ms.openlocfilehash: 29457a39ee3df0bbdbc9869cdce0e14bd85b7302
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="creating-web-apps-with-flask-in-azure"></a><span data-ttu-id="4a7f7-103">Skapa webbappar med Flask i Azure</span><span class="sxs-lookup"><span data-stu-id="4a7f7-103">Creating web apps with Flask in Azure</span></span>
<span data-ttu-id="4a7f7-104">Den här självstudiekursen beskrivs hur du kommer igång med Python [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="4a7f7-104">This tutorial describes how to get started running Python in [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span>  <span data-ttu-id="4a7f7-105">Med Web Apps får du begränsat ledigt värdutrymme och snabb distribution och kan använda Python.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-105">Web Apps provides limited free hosting and rapid deployment, and you can use Python!</span></span>  <span data-ttu-id="4a7f7-106">I takt med att appen växer kan du gå över till betald hantering och även integrera med alla övriga Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-106">As your app grows, you can switch to paid hosting, and you can also integrate with all of the other Azure services.</span></span>

<span data-ttu-id="4a7f7-107">Du skapar ett program med Flask webbramverk (se motsvarande versioner av kursen för [Django](web-sites-python-create-deploy-django-app.md) och [Bottle](web-sites-python-create-deploy-bottle-app.md)).</span><span class="sxs-lookup"><span data-stu-id="4a7f7-107">You will create an application using the Flask web framework (see alternate versions of this tutorial for [Django](web-sites-python-create-deploy-django-app.md) and [Bottle](web-sites-python-create-deploy-bottle-app.md)).</span></span>  <span data-ttu-id="4a7f7-108">Du kan skapa webbplatsen från Azure-galleriet, konfigurera Git-distribution och klona lagringsplatsen lokalt.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-108">You will create the website from the Azure gallery, set up Git deployment, and clone the repository locally.</span></span>  <span data-ttu-id="4a7f7-109">Sedan kör du programmet lokalt, gör ändringar, checkar in dem och push-överför dem till Azure.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-109">Then you will run the application locally, make changes, commit and push them to Azure.</span></span>  <span data-ttu-id="4a7f7-110">I den här kursen får du veta hur du gör det från Windows eller Mac/Linux.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-110">The tutorial shows how to do this from Windows or Mac/Linux.</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> <span data-ttu-id="4a7f7-111">Om du vill komma igång med Azure App Service innan du registrerar dig för ett Azure-konto kan du gå till [Prova App Service](https://azure.microsoft.com/try/app-service/). Där kan du direkt skapa en tillfällig startwebbapp i App Service.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-111">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="4a7f7-112">Inga kreditkort krävs. Inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-112">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="4a7f7-113">Krav</span><span class="sxs-lookup"><span data-stu-id="4a7f7-113">Prerequisites</span></span>
* <span data-ttu-id="4a7f7-114">Windows, Mac eller Linux</span><span class="sxs-lookup"><span data-stu-id="4a7f7-114">Windows, Mac or Linux</span></span>
* <span data-ttu-id="4a7f7-115">Python 2.7 eller 3.4</span><span class="sxs-lookup"><span data-stu-id="4a7f7-115">Python 2.7 or 3.4</span></span>
* <span data-ttu-id="4a7f7-116">installationsverktyg, pip, virtuell miljö (endast Python 2.7)</span><span class="sxs-lookup"><span data-stu-id="4a7f7-116">setuptools, pip, virtualenv (Python 2.7 only)</span></span>
* <span data-ttu-id="4a7f7-117">Git</span><span class="sxs-lookup"><span data-stu-id="4a7f7-117">Git</span></span>
* <span data-ttu-id="4a7f7-118">[Python Tools för Visual Studio][Python Tools för Visual Studio] (PTVS) - Obs! det här är valfritt</span><span class="sxs-lookup"><span data-stu-id="4a7f7-118">[Python Tools for Visual Studio][Python Tools for Visual Studio] (PTVS) - Note: this is optional</span></span>

<span data-ttu-id="4a7f7-119">**Obs**! TFS-publicering stöds för närvarande inte för Python-projekt.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-119">**Note**: TFS publishing is currently not supported for Python projects.</span></span>

### <a name="windows"></a><span data-ttu-id="4a7f7-120">Windows</span><span class="sxs-lookup"><span data-stu-id="4a7f7-120">Windows</span></span>
<span data-ttu-id="4a7f7-121">Om du inte redan har Python 2.7 eller 3.4 installerat (32-bitars) rekommenderar vi att du installerar [Azure SDK för Python 2.7] eller [Azure SDK för Python 3.4] med hjälp av installationsprogrammet för webbplattform.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-121">If you don't already have Python 2.7 or 3.4 installed (32-bit), we recommend installing [Azure SDK for Python 2.7] or [Azure SDK for Python 3.4] using Web Platform Installer.</span></span>  <span data-ttu-id="4a7f7-122">När du gör det installeras 32-bitarsversionen av Python, installationsverktyg, pip, virtuell miljö med mera. (Det är 32-bitarsversionen av Python som finns installerad på Azure-värddatorerna.)</span><span class="sxs-lookup"><span data-stu-id="4a7f7-122">This installs the 32-bit version of Python, setuptools, pip, virtualenv, etc (32-bit Python is what's installed on the Azure host machines).</span></span>  <span data-ttu-id="4a7f7-123">Du kan också hämta Python på [python.org].</span><span class="sxs-lookup"><span data-stu-id="4a7f7-123">Alternatively, you can get Python from [python.org].</span></span>

<span data-ttu-id="4a7f7-124">För Git rekommenderar vi [Git för Windows] eller [GitHub för Windows].</span><span class="sxs-lookup"><span data-stu-id="4a7f7-124">For Git, we recommend [Git for Windows] or [GitHub for Windows].</span></span>  <span data-ttu-id="4a7f7-125">Om du använder Visual Studio kan du använda det integrerade Git-stödet.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-125">If you use Visual Studio, you can use the integrated Git support.</span></span>

<span data-ttu-id="4a7f7-126">Vi rekommenderar också att du installerar [Python Tools 2.2 för Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="4a7f7-126">We also recommend installing [Python Tools 2.2 for Visual Studio].</span></span>  <span data-ttu-id="4a7f7-127">Det här är valfritt, men om du har [Visual Studio], inklusive det kostnadsfria Visual Studio Community 2013 eller Visual Studio Express 2013 för webben, har du en utmärkt Python IDE.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-127">This is optional, but if you have [Visual Studio], including the free Visual Studio Community 2013 or Visual Studio Express 2013 for Web, then this will give you a great Python IDE.</span></span>

### <a name="maclinux"></a><span data-ttu-id="4a7f7-128">Mac/Linux</span><span class="sxs-lookup"><span data-stu-id="4a7f7-128">Mac/Linux</span></span>
<span data-ttu-id="4a7f7-129">Du bör redan ha Python och Git installerade, men kontrollera att du har Python 2.7 eller 3.4.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-129">You should have Python and Git already installed, but make sure you have either Python 2.7 or 3.4.</span></span>

## <a name="web-app-create-on-the-azure-portal"></a><span data-ttu-id="4a7f7-130">Webbprogrammet skapa på Azure Portal</span><span class="sxs-lookup"><span data-stu-id="4a7f7-130">Web app create on the Azure Portal</span></span>
<span data-ttu-id="4a7f7-131">Det första steget i att skapa din app är att skapa webbappen via [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4a7f7-131">The first step in creating your app is to create the web app via the [Azure Portal](https://portal.azure.com).</span></span> 

1. <span data-ttu-id="4a7f7-132">Logga in på Azure Portal och klicka på knappen **NY** i det nedre vänstra hörnet.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-132">Log into the Azure Portal and click the **NEW** button in the bottom left corner.</span></span> 
2. <span data-ttu-id="4a7f7-133">Klicka på **Webb + mobilt**.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-133">Click **Web + Mobile**.</span></span>
3. <span data-ttu-id="4a7f7-134">Skriv "python" i sökrutan.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-134">In the search box, type "python".</span></span>
4. <span data-ttu-id="4a7f7-135">I sökresultaten väljer **Flask**, klicka på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-135">In the search results, select **Flask**, then click **Create**.</span></span>
5. <span data-ttu-id="4a7f7-136">Konfigurera den nya Flask-appen, till exempel skapa en ny App Service-plan och en ny resursgrupp för den.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-136">Configure the new Flask app, such as creating a new App Service plan and a new resource group for it.</span></span> <span data-ttu-id="4a7f7-137">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-137">Then, click **Create**.</span></span>
6. <span data-ttu-id="4a7f7-138">Konfigurera Git-publicering för den nya webbappen genom att följa anvisningarna i [Lokal Git-distribution till Azure App Service](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="4a7f7-138">Configure Git publishing for your newly created web app by following the instructions at [Local Git Deployment to Azure App Service](app-service-deploy-local-git.md).</span></span>

## <a name="application-overview"></a><span data-ttu-id="4a7f7-139">Programöversikt</span><span class="sxs-lookup"><span data-stu-id="4a7f7-139">Application Overview</span></span>
### <a name="git-repository-contents"></a><span data-ttu-id="4a7f7-140">Innehåll på Git-lagringsplatsen</span><span class="sxs-lookup"><span data-stu-id="4a7f7-140">Git repository contents</span></span>
<span data-ttu-id="4a7f7-141">Här är en översikt över filerna på den första Git-lagringsplatsen, som vi ska klona i nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-141">Here's an overview of the files you'll find in the initial Git repository, which we'll clone in the next section.</span></span>

    \FlaskWebProject\__init__.py
    \FlaskWebProject\views.py
    \FlaskWebProject\static\content\
    \FlaskWebProject\static\fonts\
    \FlaskWebProject\static\scripts\
    \FlaskWebProject\templates\about.html
    \FlaskWebProject\templates\contact.html
    \FlaskWebProject\templates\index.html
    \FlaskWebProject\templates\layout.html

<span data-ttu-id="4a7f7-142">Programmets huvudsakliga källor.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-142">Main sources for the application.</span></span>  <span data-ttu-id="4a7f7-143">Består av tre sidor (index, om och kontakt) med bakgrundslayout.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-143">Consists of 3 pages (index, about, contact) with a master layout.</span></span>  <span data-ttu-id="4a7f7-144">Statiskt innehåll och skript inkluderar bootstrap, jquery, modernizr och respond.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-144">Static content and scripts include bootstrap, jquery, modernizr and respond.</span></span>

    \runserver.py

<span data-ttu-id="4a7f7-145">Stöd för lokal utveckling server.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-145">Local development server support.</span></span> <span data-ttu-id="4a7f7-146">Används för att köra programmet lokalt.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-146">Use this to run the application locally.</span></span>

    \FlaskWebProject.pyproj
    \FlaskWebProject.sln

<span data-ttu-id="4a7f7-147">Projektfiler för användning med [Python Tools för Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="4a7f7-147">Project files for use with [Python Tools for Visual Studio].</span></span>

    \ptvs_virtualenv_proxy.py

<span data-ttu-id="4a7f7-148">IIS-proxy för virtuella miljöer och PTVS-stöd för fjärrfelsökning.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-148">IIS proxy for virtual environments and PTVS remote debugging support.</span></span>

    \requirements.txt

<span data-ttu-id="4a7f7-149">Externa paket som krävs för programmet.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-149">External packages needed by this application.</span></span> <span data-ttu-id="4a7f7-150">Distributionsskriptet pip-installerar paketen som listas i den här filen.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-150">The deployment script will pip install the packages listed in this file.</span></span>

    \web.2.7.config
    \web.3.4.config

<span data-ttu-id="4a7f7-151">IIS-konfigurationsfiler.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-151">IIS configuration files.</span></span>  <span data-ttu-id="4a7f7-152">Distributionsskriptet använder lämplig web.x.y.config och skapar kopian web.config.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-152">The deployment script will use the appropriate web.x.y.config and copy it as web.config.</span></span>

### <a name="optional-files---customizing-deployment"></a><span data-ttu-id="4a7f7-153">Valfria filer – anpassa distributionen</span><span class="sxs-lookup"><span data-stu-id="4a7f7-153">Optional files - Customizing deployment</span></span>
[!INCLUDE [web-sites-python-customizing-deployment](../../includes/web-sites-python-customizing-deployment.md)]

### <a name="optional-files---python-runtime"></a><span data-ttu-id="4a7f7-154">Valfria filer – Python-körning</span><span class="sxs-lookup"><span data-stu-id="4a7f7-154">Optional files - Python runtime</span></span>
[!INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

### <a name="additional-files-on-server"></a><span data-ttu-id="4a7f7-155">Ytterligare filer på servern</span><span class="sxs-lookup"><span data-stu-id="4a7f7-155">Additional files on server</span></span>
<span data-ttu-id="4a7f7-156">Det finns filer på servern som inte har lagts till på Git-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-156">Some files exist on the server but are not added to the git repository.</span></span>  <span data-ttu-id="4a7f7-157">Dessa skapas med skriptet för distribution.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-157">These are created by the deployment script.</span></span>

    \web.config

<span data-ttu-id="4a7f7-158">IIS-konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-158">IIS configuration file.</span></span>  <span data-ttu-id="4a7f7-159">Skapas utifrån web.x.y.config vid varje distribution.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-159">Created from web.x.y.config on every deployment.</span></span>

    \env\

<span data-ttu-id="4a7f7-160">Python, virtuell miljö</span><span class="sxs-lookup"><span data-stu-id="4a7f7-160">Python virtual environment.</span></span>  <span data-ttu-id="4a7f7-161">Skapas under distributionen om en kompatibel virtuell miljö inte redan finns i appen.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-161">Created during deployment if a compatible virtual environment doesn't already exist on the app.</span></span>  <span data-ttu-id="4a7f7-162">De paket som anges i requirements.txt pip-installeras, men pip hoppar över installationen om paketen redan är installerade.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-162">Packages listed in requirements.txt are pip installed, but pip will skip installation if the packages are already installed.</span></span>

<span data-ttu-id="4a7f7-163">I följande tre avsnitt beskrivs hur du fortsätter med webbappsutvecklingen i tre olika miljöer:</span><span class="sxs-lookup"><span data-stu-id="4a7f7-163">The next 3 sections describe how to proceed with the web app development under 3 different environments:</span></span>

* <span data-ttu-id="4a7f7-164">Windows med Python Tools för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4a7f7-164">Windows, with Python Tools for Visual Studio</span></span>
* <span data-ttu-id="4a7f7-165">Windows, med kommandorad</span><span class="sxs-lookup"><span data-stu-id="4a7f7-165">Windows, with command line</span></span>
* <span data-ttu-id="4a7f7-166">Mac/Linux, med kommandorad</span><span class="sxs-lookup"><span data-stu-id="4a7f7-166">Mac/Linux, with command line</span></span>

## <a name="web-app-development---windows---python-tools-for-visual-studio"></a><span data-ttu-id="4a7f7-167">Webbappsutveckling – Windows – Python Tools för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4a7f7-167">Web app development - Windows - Python Tools for Visual Studio</span></span>
### <a name="clone-the-repository"></a><span data-ttu-id="4a7f7-168">Klona lagringsplatsen</span><span class="sxs-lookup"><span data-stu-id="4a7f7-168">Clone the repository</span></span>
<span data-ttu-id="4a7f7-169">Först klonar du lagringsplatsen med hjälp av webbadressen som tillhandahålls på Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-169">First, clone the repository using the URL provided on the Azure Portal.</span></span> <span data-ttu-id="4a7f7-170">Mer information finns i [Lokal Git-distribution till Azure App Service](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="4a7f7-170">For more information, see [Local Git Deployment to Azure App Service](app-service-deploy-local-git.md).</span></span>

<span data-ttu-id="4a7f7-171">Öppna lösningsfilen (.sln) som finns i lagringsplatsens rot.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-171">Open the solution file (.sln) that is included in the root of the repository.</span></span>

![](./media/web-sites-python-create-deploy-flask-app/ptvs-solution-flask.png)

### <a name="create-virtual-environment"></a><span data-ttu-id="4a7f7-172">Skapa en virtuell miljö</span><span class="sxs-lookup"><span data-stu-id="4a7f7-172">Create virtual environment</span></span>
<span data-ttu-id="4a7f7-173">Nu ska vi skapa en virtuell miljö för lokal utveckling.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-173">Now we'll create a virtual environment for local development.</span></span>  <span data-ttu-id="4a7f7-174">Högerklicka på **Python Environments** (Python-miljöer) och välj **Add Virtual Environment...** (Lägg till virtuell miljö).</span><span class="sxs-lookup"><span data-stu-id="4a7f7-174">Right-click on **Python Environments** select **Add Virtual Environment...**.</span></span>

* <span data-ttu-id="4a7f7-175">Kontrollera att namnet på miljön är `env`.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-175">Make sure the name of the environment is `env`.</span></span>
* <span data-ttu-id="4a7f7-176">Välj bastolk.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-176">Select the base interpreter.</span></span>  <span data-ttu-id="4a7f7-177">Se till att använda samma version av Python som valts för webbappen (i runtime.txt eller i bladet **Programinställningar** för webbappen i Azure Portal).</span><span class="sxs-lookup"><span data-stu-id="4a7f7-177">Make sure to use the same version of Python that is selected for your web app (in runtime.txt or the **Application Settings** blade of your web app in the Azure Portal).</span></span>
* <span data-ttu-id="4a7f7-178">Kontrollera att alternativet för att ladda ned och installera paket är markerat.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-178">Make sure the option to download and install packages is checked.</span></span>

![](./media/web-sites-python-create-deploy-flask-app/ptvs-add-virtual-env-27.png)

<span data-ttu-id="4a7f7-179">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-179">Click **Create**.</span></span>  <span data-ttu-id="4a7f7-180">När du gör det skapas den virtuella miljön och beroenden som är angivna i requirements.txt installeras.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-180">This will create the virtual environment, and install dependencies listed in requirements.txt.</span></span>

### <a name="run-using-development-server"></a><span data-ttu-id="4a7f7-181">Kör med utvecklingsservern</span><span class="sxs-lookup"><span data-stu-id="4a7f7-181">Run using development server</span></span>
<span data-ttu-id="4a7f7-182">Tryck på F5 för att starta felsökningen. Sidan som körs lokalt öppnas automatiskt i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-182">Press F5 to start debugging, and your web browser will open automatically to the page running locally.</span></span>

![](./media/web-sites-python-create-deploy-flask-app/windows-browser-flask.png)

<span data-ttu-id="4a7f7-183">Du kan ange brytpunkter i källorna, använda bevakningsfönstren med mera.  Mer information om de olika funktionerna finns i [Dokumentationen om Python Tools för Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="4a7f7-183">You can set breakpoints in the sources, use the watch windows, etc.  See the [Python Tools for Visual Studio Documentation] for more information on the various features.</span></span>

### <a name="make-changes"></a><span data-ttu-id="4a7f7-184">Göra ändringar</span><span class="sxs-lookup"><span data-stu-id="4a7f7-184">Make changes</span></span>
<span data-ttu-id="4a7f7-185">Nu kan du experimentera med att göra ändringar i programmets källor och mallar.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-185">Now you can experiment by making changes to the application sources and/or templates.</span></span>

<span data-ttu-id="4a7f7-186">När du har testat ändringarna checkar du in dem på Git-lagringsplatsen:</span><span class="sxs-lookup"><span data-stu-id="4a7f7-186">After you've tested your changes, commit them to the Git repository:</span></span>

![](./media/web-sites-python-create-deploy-flask-app/ptvs-commit-flask.png)

### <a name="install-more-packages"></a><span data-ttu-id="4a7f7-187">Installera fler paket</span><span class="sxs-lookup"><span data-stu-id="4a7f7-187">Install more packages</span></span>
<span data-ttu-id="4a7f7-188">Programmet kan ha beroenden utöver Python och Flask.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-188">Your application may have dependencies beyond Python and Flask.</span></span>

<span data-ttu-id="4a7f7-189">Du kan installera ytterligare paket med hjälp av pip.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-189">You can install additional packages using pip.</span></span>  <span data-ttu-id="4a7f7-190">Om du vill installera ett paket högerklickar du på den virtuella miljön och väljer **Install Python Package** (Installera Python-paket).</span><span class="sxs-lookup"><span data-stu-id="4a7f7-190">To install a package, right-click on the virtual environment and select **Install Python Package**.</span></span>

<span data-ttu-id="4a7f7-191">Om du till exempel vill installera Azure SDK för Python, som ger dig tillgång till Azure Storage, Service Bus och andra Azure-tjänster, anger du `azure`:</span><span class="sxs-lookup"><span data-stu-id="4a7f7-191">For example, to install the Azure SDK for Python, which gives you access to Azure storage, service bus and other Azure services, enter `azure`:</span></span>

![](./media/web-sites-python-create-deploy-flask-app/ptvs-install-package-dialog.png)

<span data-ttu-id="4a7f7-192">Uppdatera requirements.txt genom att högerklicka på den virtuella miljön och välja **Generate requirements.txt** (Skapa requirements.txt).</span><span class="sxs-lookup"><span data-stu-id="4a7f7-192">Right-click on the virtual environment and select **Generate requirements.txt** to update requirements.txt.</span></span>

<span data-ttu-id="4a7f7-193">Checka sedan in ändringarna i requirements.txt på Git-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-193">Then, commit the changes to requirements.txt to the Git repository.</span></span>

### <a name="deploy-to-azure"></a><span data-ttu-id="4a7f7-194">Distribuera till Azure</span><span class="sxs-lookup"><span data-stu-id="4a7f7-194">Deploy to Azure</span></span>
<span data-ttu-id="4a7f7-195">Utlös en distribution genom att klicka på **Sync** (Synkronisera) eller **Push** (Pusha).</span><span class="sxs-lookup"><span data-stu-id="4a7f7-195">To trigger a deployment, click on **Sync** or **Push**.</span></span>  <span data-ttu-id="4a7f7-196">Med synkronisering görs både en överföring och en hämtning.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-196">Sync does both a push and a pull.</span></span>

![](./media/web-sites-python-create-deploy-flask-app/ptvs-git-push.png)

<span data-ttu-id="4a7f7-197">Den första distributionen tar en stund i och med att en virtuell miljö skapas, paket installeras osv.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-197">The first deployment will take some time, as it will create a virtual environment, install packages, etc.</span></span>

<span data-ttu-id="4a7f7-198">I Visual Studio visas inte förloppet för distributionen.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-198">Visual Studio doesn't show the progress of the deployment.</span></span>  <span data-ttu-id="4a7f7-199">Information om hur du granskar utdata finns i avsnittet [Felsökning – distribution](#troubleshooting-deployment).</span><span class="sxs-lookup"><span data-stu-id="4a7f7-199">If you'd like to review the output, see the section on [Troubleshooting - Deployment](#troubleshooting-deployment).</span></span>

<span data-ttu-id="4a7f7-200">Visa ändringarna genom att gå till Azure-webbadressen.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-200">Browse to the Azure URL to view your changes.</span></span>

## <a name="web-app-development---windows---command-line"></a><span data-ttu-id="4a7f7-201">Webbappsutveckling – Windows – kommandorad</span><span class="sxs-lookup"><span data-stu-id="4a7f7-201">Web app development - Windows - command line</span></span>
### <a name="clone-the-repository"></a><span data-ttu-id="4a7f7-202">Klona lagringsplatsen</span><span class="sxs-lookup"><span data-stu-id="4a7f7-202">Clone the repository</span></span>
<span data-ttu-id="4a7f7-203">Först klonar du lagringsplatsen med hjälp av webbadressen som tillhandahålls på Azure Portal. Lägg till Azure-lagringsplatsen som fjärransluten.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-203">First, clone the repository using the URL provided on the Azure Portal, and add the Azure repository as a remote.</span></span> <span data-ttu-id="4a7f7-204">Mer information finns i [Lokal Git-distribution till Azure App Service](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="4a7f7-204">For more information, see [Local Git Deployment to Azure App Service](app-service-deploy-local-git.md).</span></span>

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url> 

### <a name="create-virtual-environment"></a><span data-ttu-id="4a7f7-205">Skapa en virtuell miljö</span><span class="sxs-lookup"><span data-stu-id="4a7f7-205">Create virtual environment</span></span>
<span data-ttu-id="4a7f7-206">Vi ska skapa en ny virtuell miljö för utveckling (lägg inte till den på lagringsplatsen).</span><span class="sxs-lookup"><span data-stu-id="4a7f7-206">We'll create a new virtual environment for development purposes (do not add it to the repository).</span></span>  <span data-ttu-id="4a7f7-207">Virtuella miljöer i Python är inte relokerbara. Därför måste alla utvecklare som arbetar med programmet skapa sina egna lokalt.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-207">Virtual environments in Python are not relocatable, so every developer working on the application will create their own locally.</span></span>

<span data-ttu-id="4a7f7-208">Se till att använda samma version av Python som valts för webbappen (i runtime.txt eller i bladet **Programinställningar** för webbappen i Azure Portal).</span><span class="sxs-lookup"><span data-stu-id="4a7f7-208">Make sure to use the same version of Python that is selected for your web app (in runtime.txt or the **Application Settings** blade of your web app in the Azure Portal).</span></span>

<span data-ttu-id="4a7f7-209">För Python 2.7:</span><span class="sxs-lookup"><span data-stu-id="4a7f7-209">For Python 2.7:</span></span>

    c:\python27\python.exe -m virtualenv env

<span data-ttu-id="4a7f7-210">För Python 3.4:</span><span class="sxs-lookup"><span data-stu-id="4a7f7-210">For Python 3.4:</span></span>

    c:\python34\python.exe -m venv env

<span data-ttu-id="4a7f7-211">Installera eventuella externa paket som krävs för programmet.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-211">Install any external packages required by your application.</span></span> <span data-ttu-id="4a7f7-212">Du kan använda requirements.txt filen i lagringsplatsens rot till att installera paket i den virtuella miljön:</span><span class="sxs-lookup"><span data-stu-id="4a7f7-212">You can use the requirements.txt file at the root of the repository to install the packages in your virtual environment:</span></span>

    env\scripts\pip install -r requirements.txt

### <a name="run-using-development-server"></a><span data-ttu-id="4a7f7-213">Kör med utvecklingsservern</span><span class="sxs-lookup"><span data-stu-id="4a7f7-213">Run using development server</span></span>
<span data-ttu-id="4a7f7-214">Du kan starta programmet under en utvecklingsserver med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="4a7f7-214">You can launch the application under a development server with the following command:</span></span>

    env\scripts\python runserver.py

<span data-ttu-id="4a7f7-215">Konsolen visar webbadressen och porten servern tar emot kommunikation från:</span><span class="sxs-lookup"><span data-stu-id="4a7f7-215">The console will display the URL and port the server listens to:</span></span>

![](./media/web-sites-python-create-deploy-flask-app/windows-run-local-flask.png)

<span data-ttu-id="4a7f7-216">Sedan öppnar du webbadressen i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-216">Then, open your web browser to that URL.</span></span>

![](./media/web-sites-python-create-deploy-flask-app/windows-browser-flask.png)

### <a name="make-changes"></a><span data-ttu-id="4a7f7-217">Göra ändringar</span><span class="sxs-lookup"><span data-stu-id="4a7f7-217">Make changes</span></span>
<span data-ttu-id="4a7f7-218">Nu kan du experimentera med att göra ändringar i programmets källor och mallar.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-218">Now you can experiment by making changes to the application sources and/or templates.</span></span>

<span data-ttu-id="4a7f7-219">När du har testat ändringarna checkar du in dem på Git-lagringsplatsen:</span><span class="sxs-lookup"><span data-stu-id="4a7f7-219">After you've tested your changes, commit them to the Git repository:</span></span>

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a><span data-ttu-id="4a7f7-220">Installera fler paket</span><span class="sxs-lookup"><span data-stu-id="4a7f7-220">Install more packages</span></span>
<span data-ttu-id="4a7f7-221">Programmet kan ha beroenden utöver Python och Flask.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-221">Your application may have dependencies beyond Python and Flask.</span></span>

<span data-ttu-id="4a7f7-222">Du kan installera ytterligare paket med hjälp av pip.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-222">You can install additional packages using pip.</span></span>  <span data-ttu-id="4a7f7-223">Om du till exempel vill installera Azure SDK för Python, som ger dig tillgång till Azure Storage, Service Bus och andra Azure-tjänster, anger du:</span><span class="sxs-lookup"><span data-stu-id="4a7f7-223">For example, to install the Azure SDK for Python, which gives you access to Azure storage, service bus and other Azure services, type:</span></span>

    env\scripts\pip install azure

<span data-ttu-id="4a7f7-224">Se till att uppdatera requirements.txt:</span><span class="sxs-lookup"><span data-stu-id="4a7f7-224">Make sure to update requirements.txt:</span></span>

    env\scripts\pip freeze > requirements.txt

<span data-ttu-id="4a7f7-225">Checka in ändringarna:</span><span class="sxs-lookup"><span data-stu-id="4a7f7-225">Commit the changes:</span></span>

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-to-azure"></a><span data-ttu-id="4a7f7-226">Distribuera till Azure</span><span class="sxs-lookup"><span data-stu-id="4a7f7-226">Deploy to Azure</span></span>
<span data-ttu-id="4a7f7-227">Utlös en distribution genom att push-överföra ändringarna till Azure:</span><span class="sxs-lookup"><span data-stu-id="4a7f7-227">To trigger a deployment, push the changes to Azure:</span></span>

    git push azure master

<span data-ttu-id="4a7f7-228">Nu visas utdata från skriptet för distribution, inklusive skapandet av en virtuell miljö, installation av paket och skapande av web.config.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-228">You will see the output of the deployment script, including virtual environment creation, installation of packages, creation of web.config.</span></span>

<span data-ttu-id="4a7f7-229">Visa ändringarna genom att gå till Azure-webbadressen.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-229">Browse to the Azure URL to view your changes.</span></span>

## <a name="web-app-development---maclinux---command-line"></a><span data-ttu-id="4a7f7-230">Webbappsutveckling – Mac/Linux – kommandorad</span><span class="sxs-lookup"><span data-stu-id="4a7f7-230">Web app development - Mac/Linux - command line</span></span>
### <a name="clone-the-repository"></a><span data-ttu-id="4a7f7-231">Klona lagringsplatsen</span><span class="sxs-lookup"><span data-stu-id="4a7f7-231">Clone the repository</span></span>
<span data-ttu-id="4a7f7-232">Först klonar du lagringsplatsen med hjälp av webbadressen som tillhandahålls på Azure Portal. Lägg till Azure-lagringsplatsen som fjärransluten.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-232">First, clone the repository using the URL provided on the Azure Portal, and add the Azure repository as a remote.</span></span> <span data-ttu-id="4a7f7-233">Mer information finns i [Lokal Git-distribution till Azure App Service](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="4a7f7-233">For more information, see [Local Git Deployment to Azure App Service](app-service-deploy-local-git.md).</span></span>

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url> 

### <a name="create-virtual-environment"></a><span data-ttu-id="4a7f7-234">Skapa en virtuell miljö</span><span class="sxs-lookup"><span data-stu-id="4a7f7-234">Create virtual environment</span></span>
<span data-ttu-id="4a7f7-235">Vi ska skapa en ny virtuell miljö för utveckling (lägg inte till den på lagringsplatsen).</span><span class="sxs-lookup"><span data-stu-id="4a7f7-235">We'll create a new virtual environment for development purposes (do not add it to the repository).</span></span>  <span data-ttu-id="4a7f7-236">Virtuella miljöer i Python är inte relokerbara. Därför måste alla utvecklare som arbetar med programmet skapa sina egna lokalt.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-236">Virtual environments in Python are not relocatable, so every developer working on the application will create their own locally.</span></span>

<span data-ttu-id="4a7f7-237">Se till att använda samma version av Python som valts för webbappen (i runtime.txt eller i bladet **Programinställningar** för webbappen i Azure Portal).</span><span class="sxs-lookup"><span data-stu-id="4a7f7-237">Make sure to use the same version of Python that is selected for your web app (in runtime.txt or the **Application Settings** blade of your web app in the Azure Portal).</span></span>

<span data-ttu-id="4a7f7-238">För Python 2.7:</span><span class="sxs-lookup"><span data-stu-id="4a7f7-238">For Python 2.7:</span></span>

    python -m virtualenv env

<span data-ttu-id="4a7f7-239">För Python 3.4:</span><span class="sxs-lookup"><span data-stu-id="4a7f7-239">For Python 3.4:</span></span>

    python -m venv env
<span data-ttu-id="4a7f7-240">eller pyvenv env</span><span class="sxs-lookup"><span data-stu-id="4a7f7-240">or pyvenv env</span></span>

<span data-ttu-id="4a7f7-241">Installera eventuella externa paket som krävs för programmet.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-241">Install any external packages required by your application.</span></span> <span data-ttu-id="4a7f7-242">Du kan använda requirements.txt filen i lagringsplatsens rot till att installera paket i den virtuella miljön:</span><span class="sxs-lookup"><span data-stu-id="4a7f7-242">You can use the requirements.txt file at the root of the repository to install the packages in your virtual environment:</span></span>

    env/bin/pip install -r requirements.txt

### <a name="run-using-development-server"></a><span data-ttu-id="4a7f7-243">Kör med utvecklingsservern</span><span class="sxs-lookup"><span data-stu-id="4a7f7-243">Run using development server</span></span>
<span data-ttu-id="4a7f7-244">Du kan starta programmet under en utvecklingsserver med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="4a7f7-244">You can launch the application under a development server with the following command:</span></span>

    env/bin/python runserver.py

<span data-ttu-id="4a7f7-245">Konsolen visar webbadressen och porten servern tar emot kommunikation från:</span><span class="sxs-lookup"><span data-stu-id="4a7f7-245">The console will display the URL and port the server listens to:</span></span>

![](./media/web-sites-python-create-deploy-flask-app/mac-run-local-flask.png)

<span data-ttu-id="4a7f7-246">Sedan öppnar du webbadressen i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-246">Then, open your web browser to that URL.</span></span>

![](./media/web-sites-python-create-deploy-flask-app/mac-browser-flask.png)

### <a name="make-changes"></a><span data-ttu-id="4a7f7-247">Göra ändringar</span><span class="sxs-lookup"><span data-stu-id="4a7f7-247">Make changes</span></span>
<span data-ttu-id="4a7f7-248">Nu kan du experimentera med att göra ändringar i programmets källor och mallar.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-248">Now you can experiment by making changes to the application sources and/or templates.</span></span>

<span data-ttu-id="4a7f7-249">När du har testat ändringarna checkar du in dem på Git-lagringsplatsen:</span><span class="sxs-lookup"><span data-stu-id="4a7f7-249">After you've tested your changes, commit them to the Git repository:</span></span>

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a><span data-ttu-id="4a7f7-250">Installera fler paket</span><span class="sxs-lookup"><span data-stu-id="4a7f7-250">Install more packages</span></span>
<span data-ttu-id="4a7f7-251">Programmet kan ha beroenden utöver Python och Flask.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-251">Your application may have dependencies beyond Python and Flask.</span></span>

<span data-ttu-id="4a7f7-252">Du kan installera ytterligare paket med hjälp av pip.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-252">You can install additional packages using pip.</span></span>  <span data-ttu-id="4a7f7-253">Om du till exempel vill installera Azure SDK för Python, som ger dig tillgång till Azure Storage, Service Bus och andra Azure-tjänster, anger du:</span><span class="sxs-lookup"><span data-stu-id="4a7f7-253">For example, to install the Azure SDK for Python, which gives you access to Azure storage, service bus and other Azure services, type:</span></span>

    env/bin/pip install azure

<span data-ttu-id="4a7f7-254">Se till att uppdatera requirements.txt:</span><span class="sxs-lookup"><span data-stu-id="4a7f7-254">Make sure to update requirements.txt:</span></span>

    env/bin/pip freeze > requirements.txt

<span data-ttu-id="4a7f7-255">Checka in ändringarna:</span><span class="sxs-lookup"><span data-stu-id="4a7f7-255">Commit the changes:</span></span>

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-to-azure"></a><span data-ttu-id="4a7f7-256">Distribuera till Azure</span><span class="sxs-lookup"><span data-stu-id="4a7f7-256">Deploy to Azure</span></span>
<span data-ttu-id="4a7f7-257">Utlös en distribution genom att push-överföra ändringarna till Azure:</span><span class="sxs-lookup"><span data-stu-id="4a7f7-257">To trigger a deployment, push the changes to Azure:</span></span>

    git push azure master

<span data-ttu-id="4a7f7-258">Nu visas utdata från skriptet för distribution, inklusive skapandet av en virtuell miljö, installation av paket och skapande av web.config.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-258">You will see the output of the deployment script, including virtual environment creation, installation of packages, creation of web.config.</span></span>

<span data-ttu-id="4a7f7-259">Visa ändringarna genom att gå till Azure-webbadressen.</span><span class="sxs-lookup"><span data-stu-id="4a7f7-259">Browse to the Azure URL to view your changes.</span></span>

## <a name="troubleshooting---package-installation"></a><span data-ttu-id="4a7f7-260">Felsökning – installation av paket</span><span class="sxs-lookup"><span data-stu-id="4a7f7-260">Troubleshooting - Package Installation</span></span>
[!INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]

## <a name="troubleshooting---virtual-environment"></a><span data-ttu-id="4a7f7-261">Felsökning – virtuell miljö</span><span class="sxs-lookup"><span data-stu-id="4a7f7-261">Troubleshooting - Virtual Environment</span></span>
[!INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]

## <a name="next-steps"></a><span data-ttu-id="4a7f7-262">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4a7f7-262">Next Steps</span></span>
<span data-ttu-id="4a7f7-263">Du kan följa dessa länkar om du vill veta mer om Flask och Python Tools för Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="4a7f7-263">Follow these links to learn more about Flask and Python Tools for Visual Studio:</span></span> 

* <span data-ttu-id="4a7f7-264">[Flask-dokumentation]</span><span class="sxs-lookup"><span data-stu-id="4a7f7-264">[Flask Documentation]</span></span>
* <span data-ttu-id="4a7f7-265">[Dokumentationen om Python Tools för Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="4a7f7-265">[Python Tools for Visual Studio Documentation]</span></span>

<span data-ttu-id="4a7f7-266">Information om hur du använder Azure Table Storage och MongoDB:</span><span class="sxs-lookup"><span data-stu-id="4a7f7-266">For information on using Azure Table Storage and MongoDB:</span></span>

* <span data-ttu-id="4a7f7-267">[Flask och MongoDB på Azure med Python Tools för Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="4a7f7-267">[Flask and MongoDB on Azure with Python Tools for Visual Studio]</span></span>
* <span data-ttu-id="4a7f7-268">[Flask och Azure Table Storage på Azure med Python Tools för Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="4a7f7-268">[Flask and Azure Table Storage on Azure with Python Tools for Visual Studio]</span></span>

<span data-ttu-id="4a7f7-269">Mer information finns också i [Python Developer Center](/develop/python/).</span><span class="sxs-lookup"><span data-stu-id="4a7f7-269">For more information, see also the [Python Developer Center](/develop/python/).</span></span>

## <a name="whats-changed"></a><span data-ttu-id="4a7f7-270">Nyheter</span><span class="sxs-lookup"><span data-stu-id="4a7f7-270">What's changed</span></span>
* <span data-ttu-id="4a7f7-271">En guide till övergången från Webbplatser till App Service finns i: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="4a7f7-271">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

<!--Link references-->
<span data-ttu-id="4a7f7-272">[Flask och MongoDB på Azure med Python Tools för Visual Studio]: https://github.com/microsoft/ptvs/wiki/Flask-and-MongoDB-on-Azure</span><span class="sxs-lookup"><span data-stu-id="4a7f7-272">[Flask and MongoDB on Azure with Python Tools for Visual Studio]: https://github.com/microsoft/ptvs/wiki/Flask-and-MongoDB-on-Azure</span></span>
<span data-ttu-id="4a7f7-273">[Flask och Azure Table Storage på Azure med Python Tools för Visual Studio]: web-sites-python-ptvs-flask-table-storage.md</span><span class="sxs-lookup"><span data-stu-id="4a7f7-273">[Flask and Azure Table Storage on Azure with Python Tools for Visual Studio]: web-sites-python-ptvs-flask-table-storage.md</span></span>

<!--External Link references-->
<span data-ttu-id="4a7f7-274">[Azure SDK för Python 2.7]: http://go.microsoft.com/fwlink/?linkid=254281</span><span class="sxs-lookup"><span data-stu-id="4a7f7-274">[Azure SDK for Python 2.7]: http://go.microsoft.com/fwlink/?linkid=254281</span></span>
<span data-ttu-id="4a7f7-275">[Azure SDK för Python 3.4]: http://go.microsoft.com/fwlink/?linkid=516990</span><span class="sxs-lookup"><span data-stu-id="4a7f7-275">[Azure SDK for Python 3.4]: http://go.microsoft.com/fwlink/?linkid=516990</span></span>
<span data-ttu-id="4a7f7-276">[python.org]: http://www.python.org/</span><span class="sxs-lookup"><span data-stu-id="4a7f7-276">[python.org]: http://www.python.org/</span></span>
<span data-ttu-id="4a7f7-277">[Git för Windows]: http://msysgit.github.io/</span><span class="sxs-lookup"><span data-stu-id="4a7f7-277">[Git for Windows]: http://msysgit.github.io/</span></span>
<span data-ttu-id="4a7f7-278">[GitHub för Windows]: https://windows.github.com/</span><span class="sxs-lookup"><span data-stu-id="4a7f7-278">[GitHub for Windows]: https://windows.github.com/</span></span>
<span data-ttu-id="4a7f7-279">[Python Tools för Visual Studio]: http://aka.ms/ptvs</span><span class="sxs-lookup"><span data-stu-id="4a7f7-279">[Python Tools for Visual Studio]: http://aka.ms/ptvs</span></span>
<span data-ttu-id="4a7f7-280">[Python Tools 2.2 för Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025</span><span class="sxs-lookup"><span data-stu-id="4a7f7-280">[Python Tools 2.2 for Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025</span></span>
<span data-ttu-id="4a7f7-281">[Visual Studio]: http://www.visualstudio.com/</span><span class="sxs-lookup"><span data-stu-id="4a7f7-281">[Visual Studio]: http://www.visualstudio.com/</span></span>
<span data-ttu-id="4a7f7-282">[Dokumentationen om Python Tools för Visual Studio]: http://aka.ms/ptvsdocs</span><span class="sxs-lookup"><span data-stu-id="4a7f7-282">[Python Tools for Visual Studio Documentation]: http://aka.ms/ptvsdocs</span></span>
<span data-ttu-id="4a7f7-283">[Flask-dokumentation]: http://flask.pocoo.org/</span><span class="sxs-lookup"><span data-stu-id="4a7f7-283">[Flask Documentation]: http://flask.pocoo.org/</span></span> 

