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
# <a name="creating-web-apps-with-bottle-in-azure"></a><span data-ttu-id="29e9e-103">Skapa webbappar med Bottle i Azure</span><span class="sxs-lookup"><span data-stu-id="29e9e-103">Creating web apps with Bottle in Azure</span></span>
<span data-ttu-id="29e9e-104">Den här självstudiekursen beskrivs hur du kommer igång med Python i Azure App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="29e9e-104">This tutorial describes how to get started running Python in Azure App Service Web Apps.</span></span> <span data-ttu-id="29e9e-105">Med Web Apps får du begränsat ledigt värdutrymme och snabb distribution och kan använda Python.</span><span class="sxs-lookup"><span data-stu-id="29e9e-105">Web Apps provides limited free hosting and rapid deployment, and you can use Python!</span></span> <span data-ttu-id="29e9e-106">I takt med att appen växer kan du gå över till betald hantering och även integrera med alla övriga Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="29e9e-106">As your app grows, you can switch to paid hosting, and you can also integrate with all of the other Azure services.</span></span>

<span data-ttu-id="29e9e-107">Du skapar ett webbprogram med hjälp av Bottle webbramverk (se motsvarande versioner av kursen för [Django](web-sites-python-create-deploy-django-app.md) och [Flask](web-sites-python-create-deploy-flask-app.md)).</span><span class="sxs-lookup"><span data-stu-id="29e9e-107">You will create a web app using the Bottle web framework (see alternate versions of this tutorial for [Django](web-sites-python-create-deploy-django-app.md) and [Flask](web-sites-python-create-deploy-flask-app.md)).</span></span> <span data-ttu-id="29e9e-108">Du får skapa webbappen från Azure Marketplace, konfigurera Git-distribution och klona lagringsplatsen lokalt.</span><span class="sxs-lookup"><span data-stu-id="29e9e-108">You will create the web app from the Azure Marketplace, set up Git deployment, and clone the repository locally.</span></span> <span data-ttu-id="29e9e-109">Du ska köra webbappen lokalt, göra ändringar, genomför och push-installera dem till [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="29e9e-109">Then you will run the web app locally, make changes, commit and push them to [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span> <span data-ttu-id="29e9e-110">I den här kursen får du veta hur du gör det från Windows eller Mac/Linux.</span><span class="sxs-lookup"><span data-stu-id="29e9e-110">The tutorial shows how to do this from Windows or Mac/Linux.</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> <span data-ttu-id="29e9e-111">Om du vill komma igång med Azure App Service innan du registrerar dig för ett Azure-konto kan du gå till [Prova App Service](https://azure.microsoft.com/try/app-service/). Där kan du direkt skapa en tillfällig startwebbapp i App Service.</span><span class="sxs-lookup"><span data-stu-id="29e9e-111">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="29e9e-112">Inga kreditkort krävs. Inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="29e9e-112">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="29e9e-113">Krav</span><span class="sxs-lookup"><span data-stu-id="29e9e-113">Prerequisites</span></span>
* <span data-ttu-id="29e9e-114">Windows, Mac eller Linux</span><span class="sxs-lookup"><span data-stu-id="29e9e-114">Windows, Mac or Linux</span></span>
* <span data-ttu-id="29e9e-115">Python 2.7 eller 3.4</span><span class="sxs-lookup"><span data-stu-id="29e9e-115">Python 2.7 or 3.4</span></span>
* <span data-ttu-id="29e9e-116">installationsverktyg, pip, virtuell miljö (endast Python 2.7)</span><span class="sxs-lookup"><span data-stu-id="29e9e-116">setuptools, pip, virtualenv (Python 2.7 only)</span></span>
* <span data-ttu-id="29e9e-117">Git</span><span class="sxs-lookup"><span data-stu-id="29e9e-117">Git</span></span>
* <span data-ttu-id="29e9e-118">[Python Tools 2.2 för Visual Studio][Python Tools 2.2 för Visual Studio] (PTVS) - Obs: det här är valfritt</span><span class="sxs-lookup"><span data-stu-id="29e9e-118">[Python Tools 2.2 for Visual Studio][Python Tools 2.2 for Visual Studio] (PTVS) - Note: this is optional</span></span>

<span data-ttu-id="29e9e-119">**Obs**! TFS-publicering stöds för närvarande inte för Python-projekt.</span><span class="sxs-lookup"><span data-stu-id="29e9e-119">**Note**: TFS publishing is currently not supported for Python projects.</span></span>

### <a name="windows"></a><span data-ttu-id="29e9e-120">Windows</span><span class="sxs-lookup"><span data-stu-id="29e9e-120">Windows</span></span>
<span data-ttu-id="29e9e-121">Om du inte redan har Python 2.7 eller 3.4 installerat (32-bitars) rekommenderar vi att du installerar [Azure SDK för Python 2.7] eller [Azure SDK för Python 3.4] med hjälp av installationsprogrammet för webbplattform.</span><span class="sxs-lookup"><span data-stu-id="29e9e-121">If you don't already have Python 2.7 or 3.4 installed (32-bit), we recommend installing [Azure SDK for Python 2.7] or [Azure SDK for Python 3.4] using Web Platform Installer.</span></span> <span data-ttu-id="29e9e-122">När du gör det installeras 32-bitarsversionen av Python, installationsverktyg, pip, virtuell miljö med mera. (Det är 32-bitarsversionen av Python som finns installerad på Azure-värddatorerna.)</span><span class="sxs-lookup"><span data-stu-id="29e9e-122">This installs the 32-bit version of Python, setuptools, pip, virtualenv, etc (32-bit Python is what's installed on the Azure host machines).</span></span> <span data-ttu-id="29e9e-123">Du kan också hämta Python på [python.org].</span><span class="sxs-lookup"><span data-stu-id="29e9e-123">Alternatively, you can get Python from [python.org].</span></span>

<span data-ttu-id="29e9e-124">För Git rekommenderar vi [Git för Windows] eller [GitHub för Windows].</span><span class="sxs-lookup"><span data-stu-id="29e9e-124">For Git, we recommend [Git for Windows] or [GitHub for Windows].</span></span> <span data-ttu-id="29e9e-125">Om du använder Visual Studio kan du använda det integrerade Git-stödet.</span><span class="sxs-lookup"><span data-stu-id="29e9e-125">If you use Visual Studio, you can use the integrated Git support.</span></span>

<span data-ttu-id="29e9e-126">Vi rekommenderar också att du installerar [Python Tools 2.2 för Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="29e9e-126">We also recommend installing [Python Tools 2.2 for Visual Studio].</span></span> <span data-ttu-id="29e9e-127">Det här är valfritt, men om du har [Visual Studio], inklusive det kostnadsfria Visual Studio Community 2013 eller Visual Studio Express 2013 för webben, har du en utmärkt Python IDE.</span><span class="sxs-lookup"><span data-stu-id="29e9e-127">This is optional, but if you have [Visual Studio], including the free Visual Studio Community 2013 or Visual Studio Express 2013 for Web, then this will give you a great Python IDE.</span></span>

### <a name="maclinux"></a><span data-ttu-id="29e9e-128">Mac/Linux</span><span class="sxs-lookup"><span data-stu-id="29e9e-128">Mac/Linux</span></span>
<span data-ttu-id="29e9e-129">Du bör redan ha Python och Git installerade, men kontrollera att du har Python 2.7 eller 3.4.</span><span class="sxs-lookup"><span data-stu-id="29e9e-129">You should have Python and Git already installed, but make sure you have either Python 2.7 or 3.4.</span></span>

## <a name="web-app-creation-on-the-azure-portal"></a><span data-ttu-id="29e9e-130">Skapa en webbapp i Azure Portal</span><span class="sxs-lookup"><span data-stu-id="29e9e-130">Web app creation on the Azure Portal</span></span>
<span data-ttu-id="29e9e-131">Det första steget i att skapa din app är att skapa webbappen via [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="29e9e-131">The first step in creating your app is to create the web app via the [Azure Portal](https://portal.azure.com).</span></span>  

1. <span data-ttu-id="29e9e-132">Logga in på Azure Portal och klicka på knappen **NY** i det nedre vänstra hörnet.</span><span class="sxs-lookup"><span data-stu-id="29e9e-132">Log into the Azure Portal and click the **NEW** button in the bottom left corner.</span></span> 
2. <span data-ttu-id="29e9e-133">Skriv "python" i sökrutan.</span><span class="sxs-lookup"><span data-stu-id="29e9e-133">In the search box, type "python".</span></span>
3. <span data-ttu-id="29e9e-134">I sökresultaten väljer **Bottle**, klicka på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="29e9e-134">In the search results, select **Bottle**, then click **Create**.</span></span>
4. <span data-ttu-id="29e9e-135">Konfigurera den nya Bottle appen, till exempel skapa en ny App Service-plan och en ny resursgrupp för den.</span><span class="sxs-lookup"><span data-stu-id="29e9e-135">Configure the new Bottle app, such as creating a new App Service plan and a new resource group for it.</span></span> <span data-ttu-id="29e9e-136">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="29e9e-136">Then, click **Create**.</span></span>
5. <span data-ttu-id="29e9e-137">Konfigurera Git-publicering för den nya webbappen genom att följa anvisningarna i [Lokal Git-distribution till Azure App Service](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="29e9e-137">Configure Git publishing for your newly created web app by following the instructions at [Local Git Deployment to Azure App Service](app-service-deploy-local-git.md).</span></span>

## <a name="application-overview"></a><span data-ttu-id="29e9e-138">Programöversikt</span><span class="sxs-lookup"><span data-stu-id="29e9e-138">Application Overview</span></span>
### <a name="git-repository-contents"></a><span data-ttu-id="29e9e-139">Innehåll på Git-lagringsplatsen</span><span class="sxs-lookup"><span data-stu-id="29e9e-139">Git repository contents</span></span>
<span data-ttu-id="29e9e-140">Här är en översikt över filerna på den första Git-lagringsplatsen, som vi ska klona i nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="29e9e-140">Here's an overview of the files you'll find in the initial Git repository, which we'll clone in the next section.</span></span>

    \routes.py
    \static\content\
    \static\fonts\
    \static\scripts\
    \views\about.tpl
    \views\contact.tpl
    \views\index.tpl
    \views\layout.tpl

<span data-ttu-id="29e9e-141">Programmets huvudsakliga källor.</span><span class="sxs-lookup"><span data-stu-id="29e9e-141">Main sources for the application.</span></span> <span data-ttu-id="29e9e-142">Består av tre sidor (index, om och kontakt) med bakgrundslayout.</span><span class="sxs-lookup"><span data-stu-id="29e9e-142">Consists of 3 pages (index, about, contact) with a master layout.</span></span>  <span data-ttu-id="29e9e-143">Statiskt innehåll och skript inkluderar bootstrap, jquery, modernizr och respond.</span><span class="sxs-lookup"><span data-stu-id="29e9e-143">Static content and scripts include bootstrap, jquery, modernizr and respond.</span></span>

    \app.py

<span data-ttu-id="29e9e-144">Stöd för lokal utveckling server.</span><span class="sxs-lookup"><span data-stu-id="29e9e-144">Local development server support.</span></span> <span data-ttu-id="29e9e-145">Används för att köra programmet lokalt.</span><span class="sxs-lookup"><span data-stu-id="29e9e-145">Use this to run the application locally.</span></span>

    \BottleWebProject.pyproj
    \BottleWebProject.sln

<span data-ttu-id="29e9e-146">Projektfiler för användning med [Python Tools för Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="29e9e-146">Project files for use with [Python Tools for Visual Studio].</span></span>

    \ptvs_virtualenv_proxy.py

<span data-ttu-id="29e9e-147">IIS-proxy för virtuella miljöer och PTVS-stöd för fjärrfelsökning.</span><span class="sxs-lookup"><span data-stu-id="29e9e-147">IIS proxy for virtual environments and PTVS remote debugging support.</span></span>

    \requirements.txt

<span data-ttu-id="29e9e-148">Externa paket som krävs för programmet.</span><span class="sxs-lookup"><span data-stu-id="29e9e-148">External packages needed by this application.</span></span> <span data-ttu-id="29e9e-149">Distributionsskriptet pip-installerar paketen som listas i den här filen.</span><span class="sxs-lookup"><span data-stu-id="29e9e-149">The deployment script will pip install the packages listed in this file.</span></span>

    \web.2.7.config
    \web.3.4.config

<span data-ttu-id="29e9e-150">IIS-konfigurationsfiler.</span><span class="sxs-lookup"><span data-stu-id="29e9e-150">IIS configuration files.</span></span> <span data-ttu-id="29e9e-151">Distributionsskriptet använder lämplig web.x.y.config och skapar kopian web.config.</span><span class="sxs-lookup"><span data-stu-id="29e9e-151">The deployment script will use the appropriate web.x.y.config and copy it as web.config.</span></span>

### <a name="optional-files---customizing-deployment"></a><span data-ttu-id="29e9e-152">Valfria filer – anpassa distributionen</span><span class="sxs-lookup"><span data-stu-id="29e9e-152">Optional files - Customizing deployment</span></span>
[!INCLUDE [web-sites-python-customizing-deployment](../../includes/web-sites-python-customizing-deployment.md)]

### <a name="optional-files---python-runtime"></a><span data-ttu-id="29e9e-153">Valfria filer – Python-körning</span><span class="sxs-lookup"><span data-stu-id="29e9e-153">Optional files - Python runtime</span></span>
[!INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

### <a name="additional-files-on-server"></a><span data-ttu-id="29e9e-154">Ytterligare filer på servern</span><span class="sxs-lookup"><span data-stu-id="29e9e-154">Additional files on server</span></span>
<span data-ttu-id="29e9e-155">Det finns filer på servern som inte har lagts till på Git-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="29e9e-155">Some files exist on the server but are not added to the git repository.</span></span> <span data-ttu-id="29e9e-156">Dessa skapas med skriptet för distribution.</span><span class="sxs-lookup"><span data-stu-id="29e9e-156">These are created by the deployment script.</span></span>

    \web.config

<span data-ttu-id="29e9e-157">IIS-konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="29e9e-157">IIS configuration file.</span></span> <span data-ttu-id="29e9e-158">Skapas utifrån web.x.y.config vid varje distribution.</span><span class="sxs-lookup"><span data-stu-id="29e9e-158">Created from web.x.y.config on every deployment.</span></span>

    \env\

<span data-ttu-id="29e9e-159">Python, virtuell miljö</span><span class="sxs-lookup"><span data-stu-id="29e9e-159">Python virtual environment.</span></span> <span data-ttu-id="29e9e-160">Skapas under distributionen om det inte redan finns en kompatibel virtuell miljö i webbappen.</span><span class="sxs-lookup"><span data-stu-id="29e9e-160">Created during deployment if a compatible virtual environment doesn't already exist on the web app.</span></span>  <span data-ttu-id="29e9e-161">De paket som anges i requirements.txt pip-installeras, men pip hoppar över installationen om paketen redan är installerade.</span><span class="sxs-lookup"><span data-stu-id="29e9e-161">Packages listed in requirements.txt are pip installed, but pip will skip installation if the packages are already installed.</span></span>

<span data-ttu-id="29e9e-162">I följande tre avsnitt beskrivs hur du fortsätter med webbappsutvecklingen i tre olika miljöer:</span><span class="sxs-lookup"><span data-stu-id="29e9e-162">The next 3 sections describe how to proceed with the web app development under 3 different environments:</span></span>

* <span data-ttu-id="29e9e-163">Windows med Python Tools för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="29e9e-163">Windows, with Python Tools for Visual Studio</span></span>
* <span data-ttu-id="29e9e-164">Windows, med kommandorad</span><span class="sxs-lookup"><span data-stu-id="29e9e-164">Windows, with command line</span></span>
* <span data-ttu-id="29e9e-165">Mac/Linux, med kommandorad</span><span class="sxs-lookup"><span data-stu-id="29e9e-165">Mac/Linux, with command line</span></span>

## <a name="web-app-development---windows---python-tools-for-visual-studio"></a><span data-ttu-id="29e9e-166">Webbappsutveckling – Windows – Python Tools för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="29e9e-166">Web App development - Windows - Python Tools for Visual Studio</span></span>
### <a name="clone-the-repository"></a><span data-ttu-id="29e9e-167">Klona lagringsplatsen</span><span class="sxs-lookup"><span data-stu-id="29e9e-167">Clone the repository</span></span>
<span data-ttu-id="29e9e-168">Först klonar du lagringsplatsen med hjälp av webbadressen som tillhandahålls på Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="29e9e-168">First, clone the repository using the url provided on the Azure Portal.</span></span> <span data-ttu-id="29e9e-169">Mer information finns i [Lokal Git-distribution till Azure App Service](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="29e9e-169">For more information, see [Local Git Deployment to Azure App Service](app-service-deploy-local-git.md).</span></span>

<span data-ttu-id="29e9e-170">Öppna lösningsfilen (.sln) som finns i lagringsplatsens rot.</span><span class="sxs-lookup"><span data-stu-id="29e9e-170">Open the solution file (.sln) that is included in the root of the repository.</span></span>

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-solution-bottle.png)

### <a name="create-virtual-environment"></a><span data-ttu-id="29e9e-171">Skapa en virtuell miljö</span><span class="sxs-lookup"><span data-stu-id="29e9e-171">Create virtual environment</span></span>
<span data-ttu-id="29e9e-172">Nu ska vi skapa en virtuell miljö för lokal utveckling.</span><span class="sxs-lookup"><span data-stu-id="29e9e-172">Now we'll create a virtual environment for local development.</span></span> <span data-ttu-id="29e9e-173">Högerklicka på **Python Environments** (Python-miljöer) och välj **Add Virtual Environment...** (Lägg till virtuell miljö).</span><span class="sxs-lookup"><span data-stu-id="29e9e-173">Right-click on **Python Environments** select **Add Virtual Environment...**.</span></span>

* <span data-ttu-id="29e9e-174">Kontrollera att namnet på miljön är `env`.</span><span class="sxs-lookup"><span data-stu-id="29e9e-174">Make sure the name of the environment is `env`.</span></span>
* <span data-ttu-id="29e9e-175">Välj bastolk.</span><span class="sxs-lookup"><span data-stu-id="29e9e-175">Select the base interpreter.</span></span> <span data-ttu-id="29e9e-176">Se till att använda samma version av Python som valts för webbappen (i runtime.txt eller i bladet **Programinställningar** för webbappen i Azure Portal).</span><span class="sxs-lookup"><span data-stu-id="29e9e-176">Make sure to use the same version of Python that is selected for your web app (in runtime.txt or the **Application Settings** blade of your web app in the Azure Portal).</span></span>
* <span data-ttu-id="29e9e-177">Kontrollera att alternativet för att ladda ned och installera paket är markerat.</span><span class="sxs-lookup"><span data-stu-id="29e9e-177">Make sure the option to download and install packages is checked.</span></span>

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-add-virtual-env-27.png)

<span data-ttu-id="29e9e-178">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="29e9e-178">Click **Create**.</span></span> <span data-ttu-id="29e9e-179">När du gör det skapas den virtuella miljön och beroenden som är angivna i requirements.txt installeras.</span><span class="sxs-lookup"><span data-stu-id="29e9e-179">This will create the virtual environment, and install dependencies listed in requirements.txt.</span></span>

### <a name="run-using-development-server"></a><span data-ttu-id="29e9e-180">Kör med utvecklingsservern</span><span class="sxs-lookup"><span data-stu-id="29e9e-180">Run using development server</span></span>
<span data-ttu-id="29e9e-181">Tryck på F5 för att starta felsökningen. Sidan som körs lokalt öppnas automatiskt i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="29e9e-181">Press F5 to start debugging, and your web browser will open automatically to the page running locally.</span></span>

![](./media/web-sites-python-create-deploy-bottle-app/windows-browser-bottle.png)

<span data-ttu-id="29e9e-182">Du kan ange brytpunkter i källorna, använda bevakningsfönstren med mera. Mer information om de olika funktionerna finns i [Dokumentationen om Python Tools för Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="29e9e-182">You can set breakpoints in the sources, use the watch windows, etc. See the [Python Tools for Visual Studio Documentation] for more information on the various features.</span></span>

### <a name="make-changes"></a><span data-ttu-id="29e9e-183">Göra ändringar</span><span class="sxs-lookup"><span data-stu-id="29e9e-183">Make changes</span></span>
<span data-ttu-id="29e9e-184">Nu kan du experimentera med att göra ändringar i programmets källor och mallar.</span><span class="sxs-lookup"><span data-stu-id="29e9e-184">Now you can experiment by making changes to the application sources and/or templates.</span></span>

<span data-ttu-id="29e9e-185">När du har testat ändringarna checkar du in dem på Git-lagringsplatsen:</span><span class="sxs-lookup"><span data-stu-id="29e9e-185">After you've tested your changes, commit them to the Git repository:</span></span>

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-commit-bottle.png)

### <a name="install-more-packages"></a><span data-ttu-id="29e9e-186">Installera fler paket</span><span class="sxs-lookup"><span data-stu-id="29e9e-186">Install more packages</span></span>
<span data-ttu-id="29e9e-187">Programmet kan ha beroenden utöver Python och Bottle.</span><span class="sxs-lookup"><span data-stu-id="29e9e-187">Your application may have dependencies beyond Python and Bottle.</span></span>

<span data-ttu-id="29e9e-188">Du kan installera ytterligare paket med hjälp av pip.</span><span class="sxs-lookup"><span data-stu-id="29e9e-188">You can install additional packages using pip.</span></span> <span data-ttu-id="29e9e-189">Om du vill installera ett paket högerklickar du på den virtuella miljön och väljer **Install Python Package** (Installera Python-paket).</span><span class="sxs-lookup"><span data-stu-id="29e9e-189">To install a package, right-click on the virtual environment and select **Install Python Package**.</span></span>

<span data-ttu-id="29e9e-190">Om du till exempel vill installera Azure SDK för Python, som ger dig tillgång till Azure Storage, Service Bus och andra Azure-tjänster, anger du `azure`:</span><span class="sxs-lookup"><span data-stu-id="29e9e-190">For example, to install the Azure SDK for Python, which gives you access to Azure storage, service bus and other Azure services, enter `azure`:</span></span>

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-install-package-dialog.png)

<span data-ttu-id="29e9e-191">Uppdatera requirements.txt genom att högerklicka på den virtuella miljön och välja **Generate requirements.txt** (Skapa requirements.txt).</span><span class="sxs-lookup"><span data-stu-id="29e9e-191">Right-click on the virtual environment and select **Generate requirements.txt** to update requirements.txt.</span></span>

<span data-ttu-id="29e9e-192">Checka sedan in ändringarna i requirements.txt på Git-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="29e9e-192">Then, commit the changes to requirements.txt to the Git repository.</span></span>

### <a name="deploy-to-azure"></a><span data-ttu-id="29e9e-193">Distribuera till Azure</span><span class="sxs-lookup"><span data-stu-id="29e9e-193">Deploy to Azure</span></span>
<span data-ttu-id="29e9e-194">Utlös en distribution genom att klicka på **Sync** (Synkronisera) eller **Push** (Pusha).</span><span class="sxs-lookup"><span data-stu-id="29e9e-194">To trigger a deployment, click on **Sync** or **Push**.</span></span> <span data-ttu-id="29e9e-195">Med synkronisering görs både en överföring och en hämtning.</span><span class="sxs-lookup"><span data-stu-id="29e9e-195">Sync does both a push and a pull.</span></span>

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-git-push.png)

<span data-ttu-id="29e9e-196">Den första distributionen tar en stund i och med att en virtuell miljö skapas, paket installeras osv.</span><span class="sxs-lookup"><span data-stu-id="29e9e-196">The first deployment will take some time, as it will create a virtual environment, install packages, etc.</span></span>

<span data-ttu-id="29e9e-197">I Visual Studio visas inte förloppet för distributionen.</span><span class="sxs-lookup"><span data-stu-id="29e9e-197">Visual Studio doesn't show the progress of the deployment.</span></span> <span data-ttu-id="29e9e-198">Information om hur du granskar utdata finns i avsnittet [Felsökning – distribution](#troubleshooting-deployment).</span><span class="sxs-lookup"><span data-stu-id="29e9e-198">If you'd like to review the output, see the section on [Troubleshooting - Deployment](#troubleshooting-deployment).</span></span>

<span data-ttu-id="29e9e-199">Visa ändringarna genom att gå till Azure-webbadressen.</span><span class="sxs-lookup"><span data-stu-id="29e9e-199">Browse to the Azure URL to view your changes.</span></span>

## <a name="web-app-development---windows---command-line"></a><span data-ttu-id="29e9e-200">Webbappsutvecklingen - Windows - kommandot rad</span><span class="sxs-lookup"><span data-stu-id="29e9e-200">Web app development - Windows - Command Line</span></span>
### <a name="clone-the-repository"></a><span data-ttu-id="29e9e-201">Klona lagringsplatsen</span><span class="sxs-lookup"><span data-stu-id="29e9e-201">Clone the repository</span></span>
<span data-ttu-id="29e9e-202">Först klonar du lagringsplatsen med hjälp av webbadressen som tillhandahålls på Azure Portal. Lägg till Azure-lagringsplatsen som fjärransluten.</span><span class="sxs-lookup"><span data-stu-id="29e9e-202">First, clone the repository using the URL provided on the Azure Portal, and add the Azure repository as a remote.</span></span> <span data-ttu-id="29e9e-203">Mer information finns i [Lokal Git-distribution till Azure App Service](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="29e9e-203">For more information, see [Local Git Deployment to Azure App Service](app-service-deploy-local-git.md).</span></span>

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url> 

### <a name="create-virtual-environment"></a><span data-ttu-id="29e9e-204">Skapa en virtuell miljö</span><span class="sxs-lookup"><span data-stu-id="29e9e-204">Create virtual environment</span></span>
<span data-ttu-id="29e9e-205">Vi ska skapa en ny virtuell miljö för utveckling (lägg inte till den på lagringsplatsen).</span><span class="sxs-lookup"><span data-stu-id="29e9e-205">We'll create a new virtual environment for development purposes (do not add it to the repository).</span></span> <span data-ttu-id="29e9e-206">Virtuella miljöer i Python är inte relokerbara. Därför måste alla utvecklare som arbetar med programmet skapa sina egna lokalt.</span><span class="sxs-lookup"><span data-stu-id="29e9e-206">Virtual environments in Python are not relocatable, so every developer working on the application will create their own locally.</span></span>

<span data-ttu-id="29e9e-207">Se till att använda samma version av Python som valts för webbappen (i runtime.txt eller i bladet programinställningar för webbappen i Azure Portal)</span><span class="sxs-lookup"><span data-stu-id="29e9e-207">Make sure to use the same version of Python that is selected for your web app (in runtime.txt or the Application Settings blade for your web app in the Azure Portal)</span></span>

<span data-ttu-id="29e9e-208">För Python 2.7:</span><span class="sxs-lookup"><span data-stu-id="29e9e-208">For Python 2.7:</span></span>

    c:\python27\python.exe -m virtualenv env

<span data-ttu-id="29e9e-209">För Python 3.4:</span><span class="sxs-lookup"><span data-stu-id="29e9e-209">For Python 3.4:</span></span>

    c:\python34\python.exe -m venv env

<span data-ttu-id="29e9e-210">Installera eventuella externa paket som krävs för programmet.</span><span class="sxs-lookup"><span data-stu-id="29e9e-210">Install any external packages required by your application.</span></span> <span data-ttu-id="29e9e-211">Du kan använda requirements.txt filen i lagringsplatsens rot till att installera paket i den virtuella miljön:</span><span class="sxs-lookup"><span data-stu-id="29e9e-211">You can use the requirements.txt file at the root of the repository to install the packages in your virtual environment:</span></span>

    env\scripts\pip install -r requirements.txt

### <a name="run-using-development-server"></a><span data-ttu-id="29e9e-212">Kör med utvecklingsservern</span><span class="sxs-lookup"><span data-stu-id="29e9e-212">Run using development server</span></span>
<span data-ttu-id="29e9e-213">Du kan starta programmet under en utvecklingsserver med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="29e9e-213">You can launch the application under a development server with the following command:</span></span>

    env\scripts\python app.py

<span data-ttu-id="29e9e-214">Konsolen visar webbadressen och porten servern tar emot kommunikation från:</span><span class="sxs-lookup"><span data-stu-id="29e9e-214">The console will display the URL and port the server listens to:</span></span>

![](./media/web-sites-python-create-deploy-bottle-app/windows-run-local-bottle.png)

<span data-ttu-id="29e9e-215">Sedan öppnar du webbadressen i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="29e9e-215">Then, open your web browser to that URL.</span></span>

![](./media/web-sites-python-create-deploy-bottle-app/windows-browser-bottle.png)

### <a name="make-changes"></a><span data-ttu-id="29e9e-216">Göra ändringar</span><span class="sxs-lookup"><span data-stu-id="29e9e-216">Make changes</span></span>
<span data-ttu-id="29e9e-217">Nu kan du experimentera med att göra ändringar i programmets källor och mallar.</span><span class="sxs-lookup"><span data-stu-id="29e9e-217">Now you can experiment by making changes to the application sources and/or templates.</span></span>

<span data-ttu-id="29e9e-218">När du har testat ändringarna checkar du in dem på Git-lagringsplatsen:</span><span class="sxs-lookup"><span data-stu-id="29e9e-218">After you've tested your changes, commit them to the Git repository:</span></span>

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a><span data-ttu-id="29e9e-219">Installera fler paket</span><span class="sxs-lookup"><span data-stu-id="29e9e-219">Install more packages</span></span>
<span data-ttu-id="29e9e-220">Programmet kan ha beroenden utöver Python och Bottle.</span><span class="sxs-lookup"><span data-stu-id="29e9e-220">Your application may have dependencies beyond Python and Bottle.</span></span>

<span data-ttu-id="29e9e-221">Du kan installera ytterligare paket med hjälp av pip.</span><span class="sxs-lookup"><span data-stu-id="29e9e-221">You can install additional packages using pip.</span></span> <span data-ttu-id="29e9e-222">Om du till exempel vill installera Azure SDK för Python, som ger dig tillgång till Azure Storage, Service Bus och andra Azure-tjänster, anger du:</span><span class="sxs-lookup"><span data-stu-id="29e9e-222">For example, to install the Azure SDK for Python, which gives you access to Azure storage, service bus and other Azure services, type:</span></span>

    env\scripts\pip install azure

<span data-ttu-id="29e9e-223">Se till att uppdatera requirements.txt:</span><span class="sxs-lookup"><span data-stu-id="29e9e-223">Make sure to update requirements.txt:</span></span>

    env\scripts\pip freeze > requirements.txt

<span data-ttu-id="29e9e-224">Checka in ändringarna:</span><span class="sxs-lookup"><span data-stu-id="29e9e-224">Commit the changes:</span></span>

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-to-azure"></a><span data-ttu-id="29e9e-225">Distribuera till Azure</span><span class="sxs-lookup"><span data-stu-id="29e9e-225">Deploy to Azure</span></span>
<span data-ttu-id="29e9e-226">Utlös en distribution genom att push-överföra ändringarna till Azure:</span><span class="sxs-lookup"><span data-stu-id="29e9e-226">To trigger a deployment, push the changes to Azure:</span></span>

    git push azure master

<span data-ttu-id="29e9e-227">Nu visas utdata från skriptet för distribution, inklusive skapandet av en virtuell miljö, installation av paket och skapande av web.config.</span><span class="sxs-lookup"><span data-stu-id="29e9e-227">You will see the output of the deployment script, including virtual environment creation, installation of packages, creation of web.config.</span></span>

<span data-ttu-id="29e9e-228">Visa ändringarna genom att gå till Azure-webbadressen.</span><span class="sxs-lookup"><span data-stu-id="29e9e-228">Browse to the Azure URL to view your changes.</span></span>

## <a name="web-app-development---maclinux---command-line"></a><span data-ttu-id="29e9e-229">Webbappsutveckling – Mac/Linux – kommandorad</span><span class="sxs-lookup"><span data-stu-id="29e9e-229">Web app development - Mac/Linux - command line</span></span>
### <a name="clone-the-repository"></a><span data-ttu-id="29e9e-230">Klona lagringsplatsen</span><span class="sxs-lookup"><span data-stu-id="29e9e-230">Clone the repository</span></span>
<span data-ttu-id="29e9e-231">Först klonar du lagringsplatsen med hjälp av webbadressen som tillhandahålls på Azure Portal. Lägg till Azure-lagringsplatsen som fjärransluten.</span><span class="sxs-lookup"><span data-stu-id="29e9e-231">First, clone the repository using the URL provided on the Azure Portal, and add the Azure repository as a remote.</span></span> <span data-ttu-id="29e9e-232">Mer information finns i [Lokal Git-distribution till Azure App Service](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="29e9e-232">For more information, see [Local Git Deployment to Azure App Service](app-service-deploy-local-git.md).</span></span>

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url> 

### <a name="create-virtual-environment"></a><span data-ttu-id="29e9e-233">Skapa en virtuell miljö</span><span class="sxs-lookup"><span data-stu-id="29e9e-233">Create virtual environment</span></span>
<span data-ttu-id="29e9e-234">Vi ska skapa en ny virtuell miljö för utveckling (lägg inte till den på lagringsplatsen).</span><span class="sxs-lookup"><span data-stu-id="29e9e-234">We'll create a new virtual environment for development purposes (do not add it to the repository).</span></span> <span data-ttu-id="29e9e-235">Virtuella miljöer i Python är inte relokerbara. Därför måste alla utvecklare som arbetar med programmet skapa sina egna lokalt.</span><span class="sxs-lookup"><span data-stu-id="29e9e-235">Virtual environments in Python are not relocatable, so every developer working on the application will create their own locally.</span></span>

<span data-ttu-id="29e9e-236">Se till att använda samma version av Python som valts för webbappen (i runtime.txt eller i bladet Programinställningar för webbappen i Azure Portal).</span><span class="sxs-lookup"><span data-stu-id="29e9e-236">Make sure to use the same version of Python that is selected for your web app (in runtime.txt or the Application Settings blade of your web app in the Azure Portal).</span></span>

<span data-ttu-id="29e9e-237">För Python 2.7:</span><span class="sxs-lookup"><span data-stu-id="29e9e-237">For Python 2.7:</span></span>

    python -m virtualenv env

<span data-ttu-id="29e9e-238">För Python 3.4:</span><span class="sxs-lookup"><span data-stu-id="29e9e-238">For Python 3.4:</span></span>

    python -m venv env
<span data-ttu-id="29e9e-239">eller pyvenv env</span><span class="sxs-lookup"><span data-stu-id="29e9e-239">or pyvenv env</span></span>

<span data-ttu-id="29e9e-240">Installera eventuella externa paket som krävs för programmet.</span><span class="sxs-lookup"><span data-stu-id="29e9e-240">Install any external packages required by your application.</span></span> <span data-ttu-id="29e9e-241">Du kan använda requirements.txt filen i lagringsplatsens rot till att installera paket i den virtuella miljön:</span><span class="sxs-lookup"><span data-stu-id="29e9e-241">You can use the requirements.txt file at the root of the repository to install the packages in your virtual environment:</span></span>

    env/bin/pip install -r requirements.txt

### <a name="run-using-development-server"></a><span data-ttu-id="29e9e-242">Kör med utvecklingsservern</span><span class="sxs-lookup"><span data-stu-id="29e9e-242">Run using development server</span></span>
<span data-ttu-id="29e9e-243">Du kan starta programmet under en utvecklingsserver med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="29e9e-243">You can launch the application under a development server with the following command:</span></span>

    env/bin/python app.py

<span data-ttu-id="29e9e-244">Konsolen visar webbadressen och porten servern tar emot kommunikation från:</span><span class="sxs-lookup"><span data-stu-id="29e9e-244">The console will display the URL and port the server listens to:</span></span>

![](./media/web-sites-python-create-deploy-bottle-app/mac-run-local-bottle.png)

<span data-ttu-id="29e9e-245">Sedan öppnar du webbadressen i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="29e9e-245">Then, open your web browser to that URL.</span></span>

![](./media/web-sites-python-create-deploy-bottle-app/mac-browser-bottle.png)

### <a name="make-changes"></a><span data-ttu-id="29e9e-246">Göra ändringar</span><span class="sxs-lookup"><span data-stu-id="29e9e-246">Make changes</span></span>
<span data-ttu-id="29e9e-247">Nu kan du experimentera med att göra ändringar i programmets källor och mallar.</span><span class="sxs-lookup"><span data-stu-id="29e9e-247">Now you can experiment by making changes to the application sources and/or templates.</span></span>

<span data-ttu-id="29e9e-248">När du har testat ändringarna checkar du in dem på Git-lagringsplatsen:</span><span class="sxs-lookup"><span data-stu-id="29e9e-248">After you've tested your changes, commit them to the Git repository:</span></span>

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a><span data-ttu-id="29e9e-249">Installera fler paket</span><span class="sxs-lookup"><span data-stu-id="29e9e-249">Install more packages</span></span>
<span data-ttu-id="29e9e-250">Programmet kan ha beroenden utöver Python och Bottle.</span><span class="sxs-lookup"><span data-stu-id="29e9e-250">Your application may have dependencies beyond Python and Bottle.</span></span>

<span data-ttu-id="29e9e-251">Du kan installera ytterligare paket med hjälp av pip.</span><span class="sxs-lookup"><span data-stu-id="29e9e-251">You can install additional packages using pip.</span></span> <span data-ttu-id="29e9e-252">Om du till exempel vill installera Azure SDK för Python, som ger dig tillgång till Azure Storage, Service Bus och andra Azure-tjänster, anger du:</span><span class="sxs-lookup"><span data-stu-id="29e9e-252">For example, to install the Azure SDK for Python, which gives you access to Azure storage, service bus and other Azure services, type:</span></span>

    env/bin/pip install azure

<span data-ttu-id="29e9e-253">Se till att uppdatera requirements.txt:</span><span class="sxs-lookup"><span data-stu-id="29e9e-253">Make sure to update requirements.txt:</span></span>

    env/bin/pip freeze > requirements.txt

<span data-ttu-id="29e9e-254">Checka in ändringarna:</span><span class="sxs-lookup"><span data-stu-id="29e9e-254">Commit the changes:</span></span>

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-to-azure"></a><span data-ttu-id="29e9e-255">Distribuera till Azure</span><span class="sxs-lookup"><span data-stu-id="29e9e-255">Deploy to Azure</span></span>
<span data-ttu-id="29e9e-256">Utlös en distribution genom att push-överföra ändringarna till Azure:</span><span class="sxs-lookup"><span data-stu-id="29e9e-256">To trigger a deployment, push the changes to Azure:</span></span>

    git push azure master

<span data-ttu-id="29e9e-257">Nu visas utdata från skriptet för distribution, inklusive skapandet av en virtuell miljö, installation av paket och skapande av web.config.</span><span class="sxs-lookup"><span data-stu-id="29e9e-257">You will see the output of the deployment script, including virtual environment creation, installation of packages, creation of web.config.</span></span>

<span data-ttu-id="29e9e-258">Visa ändringarna genom att gå till Azure-webbadressen.</span><span class="sxs-lookup"><span data-stu-id="29e9e-258">Browse to the Azure URL to view your changes.</span></span>

## <a name="troubleshooting---package-installation"></a><span data-ttu-id="29e9e-259">Felsökning – installation av paket</span><span class="sxs-lookup"><span data-stu-id="29e9e-259">Troubleshooting - Package Installation</span></span>
[!INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]

## <a name="troubleshooting---virtual-environment"></a><span data-ttu-id="29e9e-260">Felsökning – virtuell miljö</span><span class="sxs-lookup"><span data-stu-id="29e9e-260">Troubleshooting - Virtual Environment</span></span>
[!INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]

## <a name="next-steps"></a><span data-ttu-id="29e9e-261">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="29e9e-261">Next Steps</span></span>
<span data-ttu-id="29e9e-262">Du kan följa dessa länkar om du vill veta mer om Bottle och Python Tools för Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="29e9e-262">Follow these links to learn more about Bottle and Python Tools for Visual Studio:</span></span> 

* <span data-ttu-id="29e9e-263">[Bottle dokumentation]</span><span class="sxs-lookup"><span data-stu-id="29e9e-263">[Bottle Documentation]</span></span>
* <span data-ttu-id="29e9e-264">[Dokumentationen om Python Tools för Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="29e9e-264">[Python Tools for Visual Studio Documentation]</span></span>

<span data-ttu-id="29e9e-265">Information om hur du använder Azure Table Storage och MongoDB:</span><span class="sxs-lookup"><span data-stu-id="29e9e-265">For information on using Azure Table Storage and MongoDB:</span></span>

* <span data-ttu-id="29e9e-266">[Bottle och MongoDB på Azure med Python Tools för Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="29e9e-266">[Bottle and MongoDB on Azure with Python Tools for Visual Studio]</span></span>
* <span data-ttu-id="29e9e-267">[Bottle och Azure Table Storage på Azure med Python Tools för Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="29e9e-267">[Bottle and Azure Table Storage on Azure with Python Tools for Visual Studio]</span></span>

## <a name="whats-changed"></a><span data-ttu-id="29e9e-268">Nyheter</span><span class="sxs-lookup"><span data-stu-id="29e9e-268">What's changed</span></span>
* <span data-ttu-id="29e9e-269">En guide till övergången från Webbplatser till App Service finns i: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="29e9e-269">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

<!--Link references-->
<span data-ttu-id="29e9e-270">[Bottle och MongoDB på Azure med Python Tools för Visual Studio]: web-sites-python-ptvs-bottle-table-storage.md</span><span class="sxs-lookup"><span data-stu-id="29e9e-270">[Bottle and MongoDB on Azure with Python Tools for Visual Studio]: web-sites-python-ptvs-bottle-table-storage.md</span></span>
<span data-ttu-id="29e9e-271">[Bottle och Azure Table Storage på Azure med Python Tools för Visual Studio]: web-sites-python-ptvs-bottle-table-storage.md</span><span class="sxs-lookup"><span data-stu-id="29e9e-271">[Bottle and Azure Table Storage on Azure with Python Tools for Visual Studio]: web-sites-python-ptvs-bottle-table-storage.md</span></span>

<!--External Link references-->
<span data-ttu-id="29e9e-272">[Azure SDK för Python 2.7]: http://go.microsoft.com/fwlink/?linkid=254281</span><span class="sxs-lookup"><span data-stu-id="29e9e-272">[Azure SDK for Python 2.7]: http://go.microsoft.com/fwlink/?linkid=254281</span></span>
<span data-ttu-id="29e9e-273">[Azure SDK för Python 3.4]: http://go.microsoft.com/fwlink/?linkid=516990</span><span class="sxs-lookup"><span data-stu-id="29e9e-273">[Azure SDK for Python 3.4]: http://go.microsoft.com/fwlink/?linkid=516990</span></span>
<span data-ttu-id="29e9e-274">[python.org]: http://www.python.org/</span><span class="sxs-lookup"><span data-stu-id="29e9e-274">[python.org]: http://www.python.org/</span></span>
<span data-ttu-id="29e9e-275">[Git för Windows]: http://msysgit.github.io/</span><span class="sxs-lookup"><span data-stu-id="29e9e-275">[Git for Windows]: http://msysgit.github.io/</span></span>
<span data-ttu-id="29e9e-276">[GitHub för Windows]: https://windows.github.com/</span><span class="sxs-lookup"><span data-stu-id="29e9e-276">[GitHub for Windows]: https://windows.github.com/</span></span>
<span data-ttu-id="29e9e-277">[Python Tools för Visual Studio]: http://aka.ms/ptvs</span><span class="sxs-lookup"><span data-stu-id="29e9e-277">[Python Tools for Visual Studio]: http://aka.ms/ptvs</span></span>
<span data-ttu-id="29e9e-278">[Python Tools 2.2 för Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025</span><span class="sxs-lookup"><span data-stu-id="29e9e-278">[Python Tools 2.2 for Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025</span></span>
<span data-ttu-id="29e9e-279">[Visual Studio]: http://www.visualstudio.com/</span><span class="sxs-lookup"><span data-stu-id="29e9e-279">[Visual Studio]: http://www.visualstudio.com/</span></span>
<span data-ttu-id="29e9e-280">[Dokumentationen om Python Tools för Visual Studio]: http://aka.ms/ptvsdocs </span><span class="sxs-lookup"><span data-stu-id="29e9e-280">[Python Tools for Visual Studio Documentation]: http://aka.ms/ptvsdocs </span></span>
<span data-ttu-id="29e9e-281">[Bottle dokumentation]: http://bottlepy.org/docs/dev/index.html</span><span class="sxs-lookup"><span data-stu-id="29e9e-281">[Bottle Documentation]: http://bottlepy.org/docs/dev/index.html</span></span>

