---
title: Skapa webbappar med Django i Azure
description: "En introduktionskurs till att köra en Python-webbapp i Azure App Service Web Apps."
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
ms.openlocfilehash: 388a2db21dd1669b48b3204aaa322d7915905506
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="creating-web-apps-with-django-in-azure"></a><span data-ttu-id="d2f1f-103">Skapa webbappar med Django i Azure</span><span class="sxs-lookup"><span data-stu-id="d2f1f-103">Creating web apps with Django in Azure</span></span>
<span data-ttu-id="d2f1f-104">Under den här kursen får du veta hur du kommer igång med Python i [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="d2f1f-104">This tutorial describes how to get started running Python on [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span> <span data-ttu-id="d2f1f-105">Med Web Apps får du begränsat ledigt värdutrymme och snabb distribution och kan använda Python.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-105">Web Apps provides limited free hosting and rapid deployment, and you can use Python!</span></span> <span data-ttu-id="d2f1f-106">I takt med att appen växer kan du gå över till betald hantering och även integrera med alla övriga Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-106">As your app grows, you can switch to paid hosting, and you can also integrate with all of the other Azure services.</span></span>

<span data-ttu-id="d2f1f-107">Du får skapa ett program med hjälp av Djangos webbramverk (se motsvarande versioner av kursen för [Flask](web-sites-python-create-deploy-flask-app.md) och [Bottle](web-sites-python-create-deploy-bottle-app.md)).</span><span class="sxs-lookup"><span data-stu-id="d2f1f-107">You will create an application using the Django web framework (see alternate versions of this tutorial for [Flask](web-sites-python-create-deploy-flask-app.md) and [Bottle](web-sites-python-create-deploy-bottle-app.md)).</span></span> <span data-ttu-id="d2f1f-108">Du får skapa webbappen från Azure Marketplace, konfigurera Git-distribution och klona lagringsplatsen lokalt.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-108">You will create the web app from the Azure Marketplace, set up Git deployment, and clone the repository locally.</span></span> <span data-ttu-id="d2f1f-109">Sedan kör du programmet lokalt, gör ändringar, checkar in dem och push-överför dem till Azure.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-109">Then you will run the application locally, make changes, commit and push them to Azure.</span></span> <span data-ttu-id="d2f1f-110">I den här kursen får du veta hur du gör det från Windows eller Mac/Linux.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-110">The tutorial shows how to do this from Windows or Mac/Linux.</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> <span data-ttu-id="d2f1f-111">Om du vill komma igång med Azure App Service innan du registrerar dig för ett Azure-konto kan du gå till [Prova App Service](https://azure.microsoft.com/try/app-service/). Där kan du direkt skapa en tillfällig startwebbapp i App Service.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-111">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="d2f1f-112">Inga kreditkort krävs. Inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-112">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="d2f1f-113">Krav</span><span class="sxs-lookup"><span data-stu-id="d2f1f-113">Prerequisites</span></span>
* <span data-ttu-id="d2f1f-114">Windows, Mac eller Linux</span><span class="sxs-lookup"><span data-stu-id="d2f1f-114">Windows, Mac or Linux</span></span>
* <span data-ttu-id="d2f1f-115">Python 2.7 eller 3.4</span><span class="sxs-lookup"><span data-stu-id="d2f1f-115">Python 2.7 or 3.4</span></span>
* <span data-ttu-id="d2f1f-116">installationsverktyg, pip, virtuell miljö (endast Python 2.7)</span><span class="sxs-lookup"><span data-stu-id="d2f1f-116">setuptools, pip, virtualenv (Python 2.7 only)</span></span>
* <span data-ttu-id="d2f1f-117">Git</span><span class="sxs-lookup"><span data-stu-id="d2f1f-117">Git</span></span>
* <span data-ttu-id="d2f1f-118">[Python Tools för Visual Studio][Python Tools för Visual Studio] (PTVS) - Obs! det här är valfritt</span><span class="sxs-lookup"><span data-stu-id="d2f1f-118">[Python Tools for Visual Studio][Python Tools for Visual Studio] (PTVS) - Note: this is optional</span></span>

<span data-ttu-id="d2f1f-119">**Obs**! TFS-publicering stöds för närvarande inte för Python-projekt.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-119">**Note**: TFS publishing is currently not supported for Python projects.</span></span>

### <a name="windows"></a><span data-ttu-id="d2f1f-120">Windows</span><span class="sxs-lookup"><span data-stu-id="d2f1f-120">Windows</span></span>
<span data-ttu-id="d2f1f-121">Om du inte redan har Python 2.7 eller 3.4 installerat (32-bitars) rekommenderar vi att du installerar [Azure SDK för Python 2.7] eller [Azure SDK för Python 3.4] med hjälp av installationsprogrammet för webbplattform.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-121">If you don't already have Python 2.7 or 3.4 installed (32-bit), we recommend installing [Azure SDK for Python 2.7] or [Azure SDK for Python 3.4] using Web Platform Installer.</span></span> <span data-ttu-id="d2f1f-122">När du gör det installeras 32-bitarsversionen av Python, installationsverktyg, pip, virtuell miljö med mera. (Det är 32-bitarsversionen av Python som finns installerad på Azure-värddatorerna.)</span><span class="sxs-lookup"><span data-stu-id="d2f1f-122">This installs the 32-bit version of Python, setuptools, pip, virtualenv, etc (32-bit Python is what's installed on the Azure host machines).</span></span> <span data-ttu-id="d2f1f-123">Du kan också hämta Python på [python.org].</span><span class="sxs-lookup"><span data-stu-id="d2f1f-123">Alternatively, you can get Python from [python.org].</span></span>

<span data-ttu-id="d2f1f-124">För Git rekommenderar vi [Git för Windows] eller [GitHub för Windows].</span><span class="sxs-lookup"><span data-stu-id="d2f1f-124">For Git, we recommend [Git for Windows] or [GitHub for Windows].</span></span> <span data-ttu-id="d2f1f-125">Om du använder Visual Studio kan du använda det integrerade Git-stödet.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-125">If you use Visual Studio, you can use the integrated Git support.</span></span>

<span data-ttu-id="d2f1f-126">Vi rekommenderar också att du installerar [Python Tools 2.2 för Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="d2f1f-126">We also recommend installing [Python Tools 2.2 for Visual Studio].</span></span> <span data-ttu-id="d2f1f-127">Det här är valfritt, men om du har [Visual Studio], inklusive det kostnadsfria Visual Studio Community 2013 eller Visual Studio Express 2013 för webben, har du en utmärkt Python IDE.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-127">This is optional, but if you have [Visual Studio], including the free Visual Studio Community 2013 or Visual Studio Express 2013 for Web, then this will give you a great Python IDE.</span></span>

### <a name="maclinux"></a><span data-ttu-id="d2f1f-128">Mac/Linux</span><span class="sxs-lookup"><span data-stu-id="d2f1f-128">Mac/Linux</span></span>
<span data-ttu-id="d2f1f-129">Du bör redan ha Python och Git installerade, men kontrollera att du har Python 2.7 eller 3.4.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-129">You should have Python and Git already installed, but make sure you have either Python 2.7 or 3.4.</span></span>

## <a name="web-app-creation-on-portal"></a><span data-ttu-id="d2f1f-130">Skapa en webbapp i Azure Portal</span><span class="sxs-lookup"><span data-stu-id="d2f1f-130">Web App Creation on Portal</span></span>
<span data-ttu-id="d2f1f-131">Det första steget i att skapa din app är att skapa webbappen via [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d2f1f-131">The first step in creating your app is to create the web app via the [Azure Portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="d2f1f-132">Logga in på Azure Portal och klicka på knappen **NY** i det nedre vänstra hörnet.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-132">Log into the Azure Portal and click the **NEW** button in the bottom left corner.</span></span>
2. <span data-ttu-id="d2f1f-133">Skriv "python" i sökrutan.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-133">In the search box, type "python".</span></span>
3. <span data-ttu-id="d2f1f-134">I sökresultaten väljer Välj **Django** (publicerat av PTVS) i sökresultatet och klicka sedan på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-134">In the search results, select **Django** (published by PTVS), then click **Create**.</span></span>
4. <span data-ttu-id="d2f1f-135">Konfigurera den nya Django-appen och skapa till exempel en ny App Service-plan och en ny resursgrupp för den.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-135">Configure the new Django app, such as creating a new App Service plan and a new resource group for it.</span></span> <span data-ttu-id="d2f1f-136">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-136">Then, click **Create**.</span></span>
5. <span data-ttu-id="d2f1f-137">Konfigurera Git-publicering för den nya webbappen genom att följa anvisningarna i [Lokal Git-distribution till Azure App Service](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="d2f1f-137">Configure Git publishing for your newly created web app by following the instructions at [Local Git Deployment to Azure App Service](app-service-deploy-local-git.md).</span></span>

## <a name="application-overview"></a><span data-ttu-id="d2f1f-138">Programöversikt</span><span class="sxs-lookup"><span data-stu-id="d2f1f-138">Application Overview</span></span>
### <a name="git-repository-contents"></a><span data-ttu-id="d2f1f-139">Innehåll på Git-lagringsplatsen</span><span class="sxs-lookup"><span data-stu-id="d2f1f-139">Git repository contents</span></span>
<span data-ttu-id="d2f1f-140">Här är en översikt över filerna på den första Git-lagringsplatsen, som vi ska klona i nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-140">Here's an overview of the files you'll find in the initial Git repository, which we'll clone in the next section.</span></span>

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

<span data-ttu-id="d2f1f-141">Programmets huvudsakliga källor.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-141">Main sources for the application.</span></span> <span data-ttu-id="d2f1f-142">Består av tre sidor (index, om och kontakt) med bakgrundslayout.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-142">Consists of 3 pages (index, about, contact) with a master layout.</span></span> <span data-ttu-id="d2f1f-143">Statiskt innehåll och skript inkluderar bootstrap, jquery, modernizr och respond.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-143">Static content and scripts include bootstrap, jquery, modernizr and respond.</span></span>

    \manage.py

<span data-ttu-id="d2f1f-144">Stöd för lokal hanterings- och utvecklingsserver.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-144">Local management and development server support.</span></span> <span data-ttu-id="d2f1f-145">Används till att köra programmet lokalt, synkronisera databasen med mera.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-145">Use this to run the application locally, synchronize the database, etc.</span></span>

    \db.sqlite3

<span data-ttu-id="d2f1f-146">Standarddatabas.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-146">Default database.</span></span> <span data-ttu-id="d2f1f-147">Innehåller de tabeller som krävs för att programmet ska köras, men inga användare. (Du kan skapa användare genom att synkronisera databasen.)</span><span class="sxs-lookup"><span data-stu-id="d2f1f-147">Includes the necessary tables for the application to run, but does not contain any users (synchronize the database to create a user).</span></span>

    \DjangoWebProject.pyproj
    \DjangoWebProject.sln

<span data-ttu-id="d2f1f-148">Projektfiler för användning med [Python Tools för Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="d2f1f-148">Project files for use with [Python Tools for Visual Studio].</span></span>

    \ptvs_virtualenv_proxy.py

<span data-ttu-id="d2f1f-149">IIS-proxy för virtuella miljöer och PTVS-stöd för fjärrfelsökning.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-149">IIS proxy for virtual environments and PTVS remote debugging support.</span></span>

    \requirements.txt

<span data-ttu-id="d2f1f-150">Externa paket som krävs för programmet.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-150">External packages needed by this application.</span></span> <span data-ttu-id="d2f1f-151">Distributionsskriptet pip-installerar paketen som listas i den här filen.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-151">The deployment script will pip install the packages listed in this file.</span></span>

    \web.2.7.config
    \web.3.4.config

<span data-ttu-id="d2f1f-152">IIS-konfigurationsfiler.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-152">IIS configuration files.</span></span> <span data-ttu-id="d2f1f-153">Distributionsskriptet använder lämplig web.x.y.config och skapar kopian web.config.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-153">The deployment script will use the appropriate web.x.y.config and copy it as web.config.</span></span>

### <a name="optional-files---customizing-deployment"></a><span data-ttu-id="d2f1f-154">Valfria filer – anpassa distributionen</span><span class="sxs-lookup"><span data-stu-id="d2f1f-154">Optional files - Customizing deployment</span></span>
[!INCLUDE [web-sites-python-django-customizing-deployment](../../includes/web-sites-python-django-customizing-deployment.md)]

### <a name="optional-files---python-runtime"></a><span data-ttu-id="d2f1f-155">Valfria filer – Python-körning</span><span class="sxs-lookup"><span data-stu-id="d2f1f-155">Optional files - Python runtime</span></span>
[!INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

### <a name="additional-files-on-server"></a><span data-ttu-id="d2f1f-156">Ytterligare filer på servern</span><span class="sxs-lookup"><span data-stu-id="d2f1f-156">Additional files on server</span></span>
<span data-ttu-id="d2f1f-157">Det finns filer på servern som inte har lagts till på Git-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-157">Some files exist on the server but are not added to the git repository.</span></span> <span data-ttu-id="d2f1f-158">Dessa skapas med skriptet för distribution.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-158">These are created by the deployment script.</span></span>

    \web.config

<span data-ttu-id="d2f1f-159">IIS-konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-159">IIS configuration file.</span></span> <span data-ttu-id="d2f1f-160">Skapas utifrån web.x.y.config vid varje distribution.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-160">Created from web.x.y.config on every deployment.</span></span>

    \env\

<span data-ttu-id="d2f1f-161">Python, virtuell miljö</span><span class="sxs-lookup"><span data-stu-id="d2f1f-161">Python virtual environment.</span></span> <span data-ttu-id="d2f1f-162">Skapas under distributionen om det inte redan finns en kompatibel virtuell miljö i webbappen.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-162">Created during deployment if a compatible virtual environment doesn't already exist on the web app.</span></span> <span data-ttu-id="d2f1f-163">De paket som anges i requirements.txt pip-installeras, men pip hoppar över installationen om paketen redan är installerade.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-163">Packages listed in requirements.txt are pip installed, but pip will skip installation if the packages are already installed.</span></span>

<span data-ttu-id="d2f1f-164">I följande tre avsnitt beskrivs hur du fortsätter med webbappsutvecklingen i tre olika miljöer:</span><span class="sxs-lookup"><span data-stu-id="d2f1f-164">The next 3 sections describe how to proceed with the web app development under 3 different environments:</span></span>

* <span data-ttu-id="d2f1f-165">Windows med Python Tools för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d2f1f-165">Windows, with Python Tools for Visual Studio</span></span>
* <span data-ttu-id="d2f1f-166">Windows, med kommandorad</span><span class="sxs-lookup"><span data-stu-id="d2f1f-166">Windows, with command line</span></span>
* <span data-ttu-id="d2f1f-167">Mac/Linux, med kommandorad</span><span class="sxs-lookup"><span data-stu-id="d2f1f-167">Mac/Linux, with command line</span></span>

## <a name="web-app-development---windows---python-tools-for-visual-studio"></a><span data-ttu-id="d2f1f-168">Webbappsutveckling – Windows – Python Tools för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d2f1f-168">Web app development - Windows - Python Tools for Visual Studio</span></span>
### <a name="clone-the-repository"></a><span data-ttu-id="d2f1f-169">Klona lagringsplatsen</span><span class="sxs-lookup"><span data-stu-id="d2f1f-169">Clone the repository</span></span>
<span data-ttu-id="d2f1f-170">Först klonar du lagringsplatsen med hjälp av webbadressen som tillhandahålls på Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-170">First, clone the repository using the URL provided on the Azure Portal.</span></span> <span data-ttu-id="d2f1f-171">Mer information finns i [Lokal Git-distribution till Azure App Service](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="d2f1f-171">For more information, see [Local Git Deployment to Azure App Service](app-service-deploy-local-git.md).</span></span>

<span data-ttu-id="d2f1f-172">Öppna lösningsfilen (.sln) som finns i lagringsplatsens rot.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-172">Open the solution file (.sln) that is included in the root of the repository.</span></span>

![](./media/web-sites-python-create-deploy-django-app/ptvs-solution-django.png)

### <a name="create-virtual-environment"></a><span data-ttu-id="d2f1f-173">Skapa en virtuell miljö</span><span class="sxs-lookup"><span data-stu-id="d2f1f-173">Create virtual environment</span></span>
<span data-ttu-id="d2f1f-174">Nu ska vi skapa en virtuell miljö för lokal utveckling.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-174">Now we'll create a virtual environment for local development.</span></span> <span data-ttu-id="d2f1f-175">Högerklicka på **Python Environments** (Python-miljöer) och välj **Add Virtual Environment...** (Lägg till virtuell miljö).</span><span class="sxs-lookup"><span data-stu-id="d2f1f-175">Right-click on **Python Environments** select **Add Virtual Environment...**.</span></span>

* <span data-ttu-id="d2f1f-176">Kontrollera att namnet på miljön är `env`.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-176">Make sure the name of the environment is `env`.</span></span>
* <span data-ttu-id="d2f1f-177">Välj bastolk.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-177">Select the base interpreter.</span></span> <span data-ttu-id="d2f1f-178">Se till att använda samma version av Python som valts för webbappen (i runtime.txt eller i bladet **Programinställningar** för webbappen i Azure Portal).</span><span class="sxs-lookup"><span data-stu-id="d2f1f-178">Make sure to use the same version of Python that is selected for your web app (in runtime.txt or the **Application Settings** blade of your web app in the Azure Portal).</span></span>
* <span data-ttu-id="d2f1f-179">Kontrollera att alternativet för att ladda ned och installera paket är markerat.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-179">Make sure the option to download and install packages is checked.</span></span>

![](./media/web-sites-python-create-deploy-django-app/ptvs-add-virtual-env-27.png)

<span data-ttu-id="d2f1f-180">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-180">Click **Create**.</span></span> <span data-ttu-id="d2f1f-181">När du gör det skapas den virtuella miljön och beroenden som är angivna i requirements.txt installeras.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-181">This will create the virtual environment, and install dependencies listed in requirements.txt.</span></span>

### <a name="create-a-superuser"></a><span data-ttu-id="d2f1f-182">Skapa en användare med fullständig behörighet</span><span class="sxs-lookup"><span data-stu-id="d2f1f-182">Create a superuser</span></span>
<span data-ttu-id="d2f1f-183">I databasen som ingår i programmet finns ingen definierad användare med fullständig behörighet.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-183">The database included with the application does not have any superuser defined.</span></span> <span data-ttu-id="d2f1f-184">Du måste skapa en användare med fullständig behörighet för att kunna använda inloggningsfunktionen i programmet och administratörsgränssnittet för Django (om du väljer att aktivera det).</span><span class="sxs-lookup"><span data-stu-id="d2f1f-184">In order to use the sign-in functionality in the application, or the Django admin interface (if you decide to enable it), you'll need to create a superuser.</span></span>

<span data-ttu-id="d2f1f-185">Kör följande från kommandoraden i projektmappen:</span><span class="sxs-lookup"><span data-stu-id="d2f1f-185">Run this from the command-line from your project folder:</span></span>

    env\scripts\python manage.py createsuperuser

<span data-ttu-id="d2f1f-186">Följ anvisningarna för att ange användarnamn, lösenord osv.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-186">Follow the prompts to set the user name, password, etc.</span></span>

### <a name="run-using-development-server"></a><span data-ttu-id="d2f1f-187">Kör med utvecklingsservern</span><span class="sxs-lookup"><span data-stu-id="d2f1f-187">Run using development server</span></span>
<span data-ttu-id="d2f1f-188">Tryck på F5 för att starta felsökningen. Sidan som körs lokalt öppnas automatiskt i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-188">Press F5 to start debugging, and your web browser will open automatically to the page running locally.</span></span>

![](./media/web-sites-python-create-deploy-django-app/windows-browser-django.png)

<span data-ttu-id="d2f1f-189">Du kan ange brytpunkter i källorna, använda bevakningsfönstren med mera. Mer information om de olika funktionerna finns i [Dokumentationen om Python Tools för Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="d2f1f-189">You can set breakpoints in the sources, use the watch windows, etc. See the [Python Tools for Visual Studio Documentation] for more information on the various features.</span></span>

### <a name="make-changes"></a><span data-ttu-id="d2f1f-190">Göra ändringar</span><span class="sxs-lookup"><span data-stu-id="d2f1f-190">Make changes</span></span>
<span data-ttu-id="d2f1f-191">Nu kan du experimentera med att göra ändringar i programmets källor och mallar.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-191">Now you can experiment by making changes to the application sources and/or templates.</span></span>

<span data-ttu-id="d2f1f-192">När du har testat ändringarna checkar du in dem på Git-lagringsplatsen:</span><span class="sxs-lookup"><span data-stu-id="d2f1f-192">After you've tested your changes, commit them to the Git repository:</span></span>

![](./media/web-sites-python-create-deploy-django-app/ptvs-commit-django.png)

### <a name="install-more-packages"></a><span data-ttu-id="d2f1f-193">Installera fler paket</span><span class="sxs-lookup"><span data-stu-id="d2f1f-193">Install more packages</span></span>
<span data-ttu-id="d2f1f-194">Programmet kan ha beroenden utöver Python och Django.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-194">Your application may have dependencies beyond Python and Django.</span></span>

<span data-ttu-id="d2f1f-195">Du kan installera ytterligare paket med hjälp av pip.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-195">You can install additional packages using pip.</span></span> <span data-ttu-id="d2f1f-196">Om du vill installera ett paket högerklickar du på den virtuella miljön och väljer **Install Python Package** (Installera Python-paket).</span><span class="sxs-lookup"><span data-stu-id="d2f1f-196">To install a package, right-click on the virtual environment and select **Install Python Package**.</span></span>

<span data-ttu-id="d2f1f-197">Om du till exempel vill installera Azure SDK för Python, som ger dig tillgång till Azure Storage, Service Bus och andra Azure-tjänster, anger du `azure`:</span><span class="sxs-lookup"><span data-stu-id="d2f1f-197">For example, to install the Azure SDK for Python, which gives you access to Azure storage, service bus and other Azure services, enter `azure`:</span></span>

![](./media/web-sites-python-create-deploy-django-app/ptvs-install-package-dialog.png)

<span data-ttu-id="d2f1f-198">Uppdatera requirements.txt genom att högerklicka på den virtuella miljön och välja **Generate requirements.txt** (Skapa requirements.txt).</span><span class="sxs-lookup"><span data-stu-id="d2f1f-198">Right-click on the virtual environment and select **Generate requirements.txt** to update requirements.txt.</span></span>

<span data-ttu-id="d2f1f-199">Checka sedan in ändringarna i requirements.txt på Git-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-199">Then, commit the changes to requirements.txt to the Git repository.</span></span>

### <a name="deploy-to-azure"></a><span data-ttu-id="d2f1f-200">Distribuera till Azure</span><span class="sxs-lookup"><span data-stu-id="d2f1f-200">Deploy to Azure</span></span>
<span data-ttu-id="d2f1f-201">Utlös en distribution genom att klicka på **Sync** (Synkronisera) eller **Push** (Pusha).</span><span class="sxs-lookup"><span data-stu-id="d2f1f-201">To trigger a deployment, click on **Sync** or **Push**.</span></span> <span data-ttu-id="d2f1f-202">Med synkronisering görs både en överföring och en hämtning.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-202">Sync does both a push and a pull.</span></span>

![](./media/web-sites-python-create-deploy-django-app/ptvs-git-push.png)

<span data-ttu-id="d2f1f-203">Den första distributionen tar en stund i och med att en virtuell miljö skapas, paket installeras osv.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-203">The first deployment will take some time, as it will create a virtual environment, install packages, etc.</span></span>

<span data-ttu-id="d2f1f-204">I Visual Studio visas inte förloppet för distributionen.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-204">Visual Studio doesn't show the progress of the deployment.</span></span> <span data-ttu-id="d2f1f-205">Information om hur du granskar utdata finns i avsnittet [Felsökning – distribution](#troubleshooting-deployment).</span><span class="sxs-lookup"><span data-stu-id="d2f1f-205">If you'd like to review the output, see the section on [Troubleshooting - Deployment](#troubleshooting-deployment).</span></span>

<span data-ttu-id="d2f1f-206">Visa ändringarna genom att gå till Azure-webbadressen.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-206">Browse to the Azure URL to view your changes.</span></span>

## <a name="web-app-development---windows---command-line"></a><span data-ttu-id="d2f1f-207">Webbappsutveckling – Windows – kommandorad</span><span class="sxs-lookup"><span data-stu-id="d2f1f-207">Web app development - Windows - command line</span></span>
### <a name="clone-the-repository"></a><span data-ttu-id="d2f1f-208">Klona lagringsplatsen</span><span class="sxs-lookup"><span data-stu-id="d2f1f-208">Clone the repository</span></span>
<span data-ttu-id="d2f1f-209">Först klonar du lagringsplatsen med hjälp av webbadressen som tillhandahålls på Azure Portal. Lägg till Azure-lagringsplatsen som fjärransluten.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-209">First, clone the repository using the URL provided on the Azure Portal, and add the Azure repository as a remote.</span></span> <span data-ttu-id="d2f1f-210">Mer information finns i [Lokal Git-distribution till Azure App Service](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="d2f1f-210">For more information, see [Local Git Deployment to Azure App Service](app-service-deploy-local-git.md).</span></span>

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url>

### <a name="create-virtual-environment"></a><span data-ttu-id="d2f1f-211">Skapa en virtuell miljö</span><span class="sxs-lookup"><span data-stu-id="d2f1f-211">Create virtual environment</span></span>
<span data-ttu-id="d2f1f-212">Vi ska skapa en ny virtuell miljö för utveckling (lägg inte till den på lagringsplatsen).</span><span class="sxs-lookup"><span data-stu-id="d2f1f-212">We'll create a new virtual environment for development purposes (do not add it to the repository).</span></span> <span data-ttu-id="d2f1f-213">Virtuella miljöer i Python är inte relokerbara. Därför måste alla utvecklare som arbetar med programmet skapa sina egna lokalt.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-213">Virtual environments in Python are not relocatable, so every developer working on the application will create their own locally.</span></span>

<span data-ttu-id="d2f1f-214">Se till att använda samma version av Python som valts för webbappen (i runtime.txt eller i bladet Programinställningar för webbappen i Azure Portal).</span><span class="sxs-lookup"><span data-stu-id="d2f1f-214">Make sure to use the same version of Python that is selected for your web app (in runtime.txt or the Application Settings blade of your web app in the Azure Portal).</span></span>

<span data-ttu-id="d2f1f-215">För Python 2.7:</span><span class="sxs-lookup"><span data-stu-id="d2f1f-215">For Python 2.7:</span></span>

    c:\python27\python.exe -m virtualenv env

<span data-ttu-id="d2f1f-216">För Python 3.4:</span><span class="sxs-lookup"><span data-stu-id="d2f1f-216">For Python 3.4:</span></span>

    c:\python34\python.exe -m venv env

<span data-ttu-id="d2f1f-217">Installera eventuella externa paket som krävs för programmet.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-217">Install any external packages required by your application.</span></span> <span data-ttu-id="d2f1f-218">Du kan använda requirements.txt filen i lagringsplatsens rot till att installera paket i den virtuella miljön:</span><span class="sxs-lookup"><span data-stu-id="d2f1f-218">You can use the requirements.txt file at the root of the repository to install the packages in your virtual environment:</span></span>

    env\scripts\pip install -r requirements.txt

### <a name="create-a-superuser"></a><span data-ttu-id="d2f1f-219">Skapa en användare med fullständig behörighet</span><span class="sxs-lookup"><span data-stu-id="d2f1f-219">Create a superuser</span></span>
<span data-ttu-id="d2f1f-220">I databasen som ingår i programmet finns ingen definierad användare med fullständig behörighet.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-220">The database included with the application does not have any superuser defined.</span></span> <span data-ttu-id="d2f1f-221">Du måste skapa en användare med fullständig behörighet för att kunna använda inloggningsfunktionen i programmet och administratörsgränssnittet för Django (om du väljer att aktivera det).</span><span class="sxs-lookup"><span data-stu-id="d2f1f-221">In order to use the sign-in functionality in the application, or the Django admin interface (if you decide to enable it), you'll need to create a superuser.</span></span>

<span data-ttu-id="d2f1f-222">Kör följande från kommandoraden i projektmappen:</span><span class="sxs-lookup"><span data-stu-id="d2f1f-222">Run this from the command-line from your project folder:</span></span>

    env\scripts\python manage.py createsuperuser

<span data-ttu-id="d2f1f-223">Följ anvisningarna för att ange användarnamn, lösenord osv.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-223">Follow the prompts to set the user name, password, etc.</span></span>

### <a name="run-using-development-server"></a><span data-ttu-id="d2f1f-224">Kör med utvecklingsservern</span><span class="sxs-lookup"><span data-stu-id="d2f1f-224">Run using development server</span></span>
<span data-ttu-id="d2f1f-225">Du kan starta programmet under en utvecklingsserver med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="d2f1f-225">You can launch the application under a development server with the following command:</span></span>

    env\scripts\python manage.py runserver

<span data-ttu-id="d2f1f-226">Konsolen visar webbadressen och porten servern tar emot kommunikation från:</span><span class="sxs-lookup"><span data-stu-id="d2f1f-226">The console will display the URL and port the server listens to:</span></span>

![](./media/web-sites-python-create-deploy-django-app/windows-run-local-django.png)

<span data-ttu-id="d2f1f-227">Sedan öppnar du webbadressen i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-227">Then, open your web browser to that URL.</span></span>

![](./media/web-sites-python-create-deploy-django-app/windows-browser-django.png)

### <a name="make-changes"></a><span data-ttu-id="d2f1f-228">Göra ändringar</span><span class="sxs-lookup"><span data-stu-id="d2f1f-228">Make changes</span></span>
<span data-ttu-id="d2f1f-229">Nu kan du experimentera med att göra ändringar i programmets källor och mallar.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-229">Now you can experiment by making changes to the application sources and/or templates.</span></span>

<span data-ttu-id="d2f1f-230">När du har testat ändringarna checkar du in dem på Git-lagringsplatsen:</span><span class="sxs-lookup"><span data-stu-id="d2f1f-230">After you've tested your changes, commit them to the Git repository:</span></span>

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a><span data-ttu-id="d2f1f-231">Installera fler paket</span><span class="sxs-lookup"><span data-stu-id="d2f1f-231">Install more packages</span></span>
<span data-ttu-id="d2f1f-232">Programmet kan ha beroenden utöver Python och Django.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-232">Your application may have dependencies beyond Python and Django.</span></span>

<span data-ttu-id="d2f1f-233">Du kan installera ytterligare paket med hjälp av pip.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-233">You can install additional packages using pip.</span></span> <span data-ttu-id="d2f1f-234">Om du till exempel vill installera Azure SDK för Python, som ger dig tillgång till Azure Storage, Service Bus och andra Azure-tjänster, anger du:</span><span class="sxs-lookup"><span data-stu-id="d2f1f-234">For example, to install the Azure SDK for Python, which gives you access to Azure storage, service bus and other Azure services, type:</span></span>

    env\scripts\pip install azure

<span data-ttu-id="d2f1f-235">Se till att uppdatera requirements.txt:</span><span class="sxs-lookup"><span data-stu-id="d2f1f-235">Make sure to update requirements.txt:</span></span>

    env\scripts\pip freeze > requirements.txt

<span data-ttu-id="d2f1f-236">Checka in ändringarna:</span><span class="sxs-lookup"><span data-stu-id="d2f1f-236">Commit the changes:</span></span>

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-to-azure"></a><span data-ttu-id="d2f1f-237">Distribuera till Azure</span><span class="sxs-lookup"><span data-stu-id="d2f1f-237">Deploy to Azure</span></span>
<span data-ttu-id="d2f1f-238">Utlös en distribution genom att push-överföra ändringarna till Azure:</span><span class="sxs-lookup"><span data-stu-id="d2f1f-238">To trigger a deployment, push the changes to Azure:</span></span>

    git push azure master

<span data-ttu-id="d2f1f-239">Nu visas utdata från skriptet för distribution, inklusive skapandet av en virtuell miljö, installation av paket och skapande av web.config.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-239">You will see the output of the deployment script, including virtual environment creation, installation of packages, creation of web.config.</span></span>

<span data-ttu-id="d2f1f-240">Visa ändringarna genom att gå till Azure-webbadressen.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-240">Browse to the Azure URL to view your changes.</span></span>

## <a name="web-app-development---maclinux---command-line"></a><span data-ttu-id="d2f1f-241">Webbappsutveckling – Mac/Linux – kommandorad</span><span class="sxs-lookup"><span data-stu-id="d2f1f-241">Web app development - Mac/Linux - command line</span></span>
### <a name="clone-the-repository"></a><span data-ttu-id="d2f1f-242">Klona lagringsplatsen</span><span class="sxs-lookup"><span data-stu-id="d2f1f-242">Clone the repository</span></span>
<span data-ttu-id="d2f1f-243">Först klonar du lagringsplatsen med hjälp av webbadressen som tillhandahålls på Azure Portal. Lägg till Azure-lagringsplatsen som fjärransluten.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-243">First, clone the repository using the URL provided on the Azure Portal, and add the Azure repository as a remote.</span></span> <span data-ttu-id="d2f1f-244">Mer information finns i [Lokal Git-distribution till Azure App Service](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="d2f1f-244">For more information, see [Local Git Deployment to Azure App Service](app-service-deploy-local-git.md).</span></span>

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url>

### <a name="create-virtual-environment"></a><span data-ttu-id="d2f1f-245">Skapa en virtuell miljö</span><span class="sxs-lookup"><span data-stu-id="d2f1f-245">Create virtual environment</span></span>
<span data-ttu-id="d2f1f-246">Vi ska skapa en ny virtuell miljö för utveckling (lägg inte till den på lagringsplatsen).</span><span class="sxs-lookup"><span data-stu-id="d2f1f-246">We'll create a new virtual environment for development purposes (do not add it to the repository).</span></span> <span data-ttu-id="d2f1f-247">Virtuella miljöer i Python är inte relokerbara. Därför måste alla utvecklare som arbetar med programmet skapa sina egna lokalt.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-247">Virtual environments in Python are not relocatable, so every developer working on the application will create their own locally.</span></span>

<span data-ttu-id="d2f1f-248">Se till att använda samma version av Python som valts för webbappen (i runtime.txt eller i bladet Programinställningar för webbappen i Azure Portal).</span><span class="sxs-lookup"><span data-stu-id="d2f1f-248">Make sure to use the same version of Python that is selected for your web app (in runtime.txt or the Application Settings blade of your web app in the Azure Portal).</span></span>

<span data-ttu-id="d2f1f-249">För Python 2.7:</span><span class="sxs-lookup"><span data-stu-id="d2f1f-249">For Python 2.7:</span></span>

    python -m virtualenv env

<span data-ttu-id="d2f1f-250">För Python 3.4:</span><span class="sxs-lookup"><span data-stu-id="d2f1f-250">For Python 3.4:</span></span>

    python -m venv env

<span data-ttu-id="d2f1f-251">eller</span><span class="sxs-lookup"><span data-stu-id="d2f1f-251">or</span></span>

    pyvenv env

<span data-ttu-id="d2f1f-252">Installera eventuella externa paket som krävs för programmet.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-252">Install any external packages required by your application.</span></span> <span data-ttu-id="d2f1f-253">Du kan använda requirements.txt filen i lagringsplatsens rot till att installera paket i den virtuella miljön:</span><span class="sxs-lookup"><span data-stu-id="d2f1f-253">You can use the requirements.txt file at the root of the repository to install the packages in your virtual environment:</span></span>

    env/bin/pip install -r requirements.txt

### <a name="create-a-superuser"></a><span data-ttu-id="d2f1f-254">Skapa en användare med fullständig behörighet</span><span class="sxs-lookup"><span data-stu-id="d2f1f-254">Create a superuser</span></span>
<span data-ttu-id="d2f1f-255">I databasen som ingår i programmet finns ingen definierad användare med fullständig behörighet.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-255">The database included with the application does not have any superuser defined.</span></span> <span data-ttu-id="d2f1f-256">Du måste skapa en användare med fullständig behörighet för att kunna använda inloggningsfunktionen i programmet och administratörsgränssnittet för Django (om du väljer att aktivera det).</span><span class="sxs-lookup"><span data-stu-id="d2f1f-256">In order to use the sign-in functionality in the application, or the Django admin interface (if you decide to enable it), you'll need to create a superuser.</span></span>

<span data-ttu-id="d2f1f-257">Kör följande från kommandoraden i projektmappen:</span><span class="sxs-lookup"><span data-stu-id="d2f1f-257">Run this from the command-line from your project folder:</span></span>

    env/bin/python manage.py createsuperuser

<span data-ttu-id="d2f1f-258">Följ anvisningarna för att ange användarnamn, lösenord osv.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-258">Follow the prompts to set the user name, password, etc.</span></span>

### <a name="run-using-development-server"></a><span data-ttu-id="d2f1f-259">Kör med utvecklingsservern</span><span class="sxs-lookup"><span data-stu-id="d2f1f-259">Run using development server</span></span>
<span data-ttu-id="d2f1f-260">Du kan starta programmet under en utvecklingsserver med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="d2f1f-260">You can launch the application under a development server with the following command:</span></span>

    env/bin/python manage.py runserver

<span data-ttu-id="d2f1f-261">Konsolen visar webbadressen och porten servern tar emot kommunikation från:</span><span class="sxs-lookup"><span data-stu-id="d2f1f-261">The console will display the URL and port the server listens to:</span></span>

![](./media/web-sites-python-create-deploy-django-app/mac-run-local-django.png)

<span data-ttu-id="d2f1f-262">Sedan öppnar du webbadressen i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-262">Then, open your web browser to that URL.</span></span>

![](./media/web-sites-python-create-deploy-django-app/mac-browser-django.png)

### <a name="make-changes"></a><span data-ttu-id="d2f1f-263">Göra ändringar</span><span class="sxs-lookup"><span data-stu-id="d2f1f-263">Make changes</span></span>
<span data-ttu-id="d2f1f-264">Nu kan du experimentera med att göra ändringar i programmets källor och mallar.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-264">Now you can experiment by making changes to the application sources and/or templates.</span></span>

<span data-ttu-id="d2f1f-265">När du har testat ändringarna checkar du in dem på Git-lagringsplatsen:</span><span class="sxs-lookup"><span data-stu-id="d2f1f-265">After you've tested your changes, commit them to the Git repository:</span></span>

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a><span data-ttu-id="d2f1f-266">Installera fler paket</span><span class="sxs-lookup"><span data-stu-id="d2f1f-266">Install more packages</span></span>
<span data-ttu-id="d2f1f-267">Programmet kan ha beroenden utöver Python och Django.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-267">Your application may have dependencies beyond Python and Django.</span></span>

<span data-ttu-id="d2f1f-268">Du kan installera ytterligare paket med hjälp av pip.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-268">You can install additional packages using pip.</span></span> <span data-ttu-id="d2f1f-269">Om du till exempel vill installera Azure SDK för Python, som ger dig tillgång till Azure Storage, Service Bus och andra Azure-tjänster, anger du:</span><span class="sxs-lookup"><span data-stu-id="d2f1f-269">For example, to install the Azure SDK for Python, which gives you access to Azure storage, service bus and other Azure services, type:</span></span>

    env/bin/pip install azure

<span data-ttu-id="d2f1f-270">Se till att uppdatera requirements.txt:</span><span class="sxs-lookup"><span data-stu-id="d2f1f-270">Make sure to update requirements.txt:</span></span>

    env/bin/pip freeze > requirements.txt

<span data-ttu-id="d2f1f-271">Checka in ändringarna:</span><span class="sxs-lookup"><span data-stu-id="d2f1f-271">Commit the changes:</span></span>

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-to-azure"></a><span data-ttu-id="d2f1f-272">Distribuera till Azure</span><span class="sxs-lookup"><span data-stu-id="d2f1f-272">Deploy to Azure</span></span>
<span data-ttu-id="d2f1f-273">Utlös en distribution genom att push-överföra ändringarna till Azure:</span><span class="sxs-lookup"><span data-stu-id="d2f1f-273">To trigger a deployment, push the changes to Azure:</span></span>

    git push azure master

<span data-ttu-id="d2f1f-274">Nu visas utdata från skriptet för distribution, inklusive skapandet av en virtuell miljö, installation av paket och skapande av web.config.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-274">You will see the output of the deployment script, including virtual environment creation, installation of packages, creation of web.config.</span></span>

<span data-ttu-id="d2f1f-275">Visa ändringarna genom att gå till Azure-webbadressen.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-275">Browse to the Azure URL to view your changes.</span></span>

## <a name="troubleshooting---package-installation"></a><span data-ttu-id="d2f1f-276">Felsökning – installation av paket</span><span class="sxs-lookup"><span data-stu-id="d2f1f-276">Troubleshooting - Package Installation</span></span>
[!INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]

## <a name="troubleshooting---virtual-environment"></a><span data-ttu-id="d2f1f-277">Felsökning – virtuell miljö</span><span class="sxs-lookup"><span data-stu-id="d2f1f-277">Troubleshooting - Virtual Environment</span></span>
[!INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]

## <a name="troubleshooting---static-files"></a><span data-ttu-id="d2f1f-278">Felsökning – statiska filer</span><span class="sxs-lookup"><span data-stu-id="d2f1f-278">Troubleshooting - Static Files</span></span>
<span data-ttu-id="d2f1f-279">Med Django samlas statiska filer.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-279">Django has the concept of collecting static files.</span></span> <span data-ttu-id="d2f1f-280">Det gör att alla statiska filer kopieras från sina ursprungliga platser till en enda mapp.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-280">This takes all the static files from their original location and copies them to a single folder.</span></span> <span data-ttu-id="d2f1f-281">För det här programmet kopieras de till `/static`.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-281">For this application, they are copied to `/static`.</span></span>

<span data-ttu-id="d2f1f-282">Det görs eftersom statiska filer kan komma från olika "Django-appar".</span><span class="sxs-lookup"><span data-stu-id="d2f1f-282">This is done because static files may come from different Django 'apps'.</span></span> <span data-ttu-id="d2f1f-283">De statiska filerna från administratörsgränssnittet för Django finns till exempel i en undermapp för Django-biblioteket i den virtuella miljön.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-283">For example, the static files from the Django admin interfaces are located in a Django library subfolder in the virtual environment.</span></span> <span data-ttu-id="d2f1f-284">Statiska filer som definieras av programmet finns i `/app/static`.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-284">Static files defined by this application are located in `/app/static`.</span></span> <span data-ttu-id="d2f1f-285">När du använder flera "Django-appar" sparas statiska filer på flera platser.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-285">As you use more Django 'apps', you'll have static files located in multiple places.</span></span>

<span data-ttu-id="d2f1f-286">När du kör programmet i felsökningsläge hanterar programmet de statiska filerna från deras ursprungliga plats.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-286">When running the application in debug mode, the application serves the static files from their original location.</span></span>

<span data-ttu-id="d2f1f-287">När du kör programmet i versionsläge hanterar programmet **inte** de statiska filerna.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-287">When running the application in release mode, the application does **not** serve the static files.</span></span> <span data-ttu-id="d2f1f-288">Det är webbserverns uppgift att hantera filerna.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-288">It is the responsibility of the web server to serve the files.</span></span> <span data-ttu-id="d2f1f-289">För det här programmet hanterar IIS statiska filer från `/static`.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-289">For this application, IIS will serve the static files from `/static`.</span></span>

<span data-ttu-id="d2f1f-290">Insamling av statiska filer görs automatiskt som en del av distributionsskriptet och tidigare insamlade filer rensas.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-290">The collection of static files is done automatically as part of the deployment script, clearing previously collected files.</span></span> <span data-ttu-id="d2f1f-291">Det innebär att samlingen sker för varje distribution. Det gör distribution något långsammare, men ser till att filer som inte längre behövs inte är tillgängliga, vilket undanröjer potentiella säkerhetsproblem.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-291">This means the collection occurs on every deployment, slowing down deployment a bit, but it ensures that obsolete files won't be available, avoiding a potential security issue.</span></span>

<span data-ttu-id="d2f1f-292">Om du vill hoppa över insamlingen av statiska filer för Django-programmet:</span><span class="sxs-lookup"><span data-stu-id="d2f1f-292">If you want to skip collection of static files for your Django application:</span></span>

    \.skipDjango

<span data-ttu-id="d2f1f-293">måste du göra insamlingen manuellt på den lokala datorn:</span><span class="sxs-lookup"><span data-stu-id="d2f1f-293">Then you'll need to do the collection manually on your local machine:</span></span>

    env\scripts\python manage.py collectstatic

<span data-ttu-id="d2f1f-294">Ta sedan bort `\static`-mappen från `.gitignore` och lägg till den på Git-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-294">Then remove the `\static` folder from `.gitignore` and add it to the Git repository.</span></span>

## <a name="troubleshooting---settings"></a><span data-ttu-id="d2f1f-295">Felsökning – inställningar</span><span class="sxs-lookup"><span data-stu-id="d2f1f-295">Troubleshooting - Settings</span></span>
<span data-ttu-id="d2f1f-296">Ett flertal inställningar för programmet kan ändras i `DjangoWebProject/settings.py`.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-296">Various settings for the application can be changed in `DjangoWebProject/settings.py`.</span></span>

<span data-ttu-id="d2f1f-297">För enklare utveckling är felsökningsläget aktiverat.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-297">For developer convenience, debug mode is enabled.</span></span> <span data-ttu-id="d2f1f-298">En bra bieffekt är att du kan se bilder och annat statiskt innehåll vid lokal körning utan att du behöver samla in statiska filer.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-298">One nice side effect of that is you'll be able to see images and other static content when running locally, without having to collect static files.</span></span>

<span data-ttu-id="d2f1f-299">Så här inaktiverar du felsökningsläget:</span><span class="sxs-lookup"><span data-stu-id="d2f1f-299">To disable debug mode:</span></span>

    DEBUG = False

<span data-ttu-id="d2f1f-300">När felsökningen är inaktiverad måste värdet för `ALLOWED_HOSTS` uppdateras så att det inkluderar Azure-värdnamnet.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-300">When debug is disabled, the value for `ALLOWED_HOSTS` needs to be updated to include the Azure host name.</span></span> <span data-ttu-id="d2f1f-301">Exempel:</span><span class="sxs-lookup"><span data-stu-id="d2f1f-301">For example:</span></span>

    ALLOWED_HOSTS = (
        'pythonapp.azurewebsites.net',
    )

<span data-ttu-id="d2f1f-302">så här aktiverar du valfri:</span><span class="sxs-lookup"><span data-stu-id="d2f1f-302">or to enable any:</span></span>

    ALLOWED_HOSTS = (
        '*',
    )

<span data-ttu-id="d2f1f-303">I praktiken kan du vilja göra något lite mer komplext för att hantera växlingen mellan felsöknings- och versionsläge och hämta värdnamnet.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-303">In practice, you may want to do something more complex to deal with switching between debug and release mode, and getting the host name.</span></span>

<span data-ttu-id="d2f1f-304">Du kan ange miljövariabler i Azure Portal på sidan **Konfigurera** i avsnittet **Programinställningar**.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-304">You can set environment variables through the Azure portal **CONFIGURE** page, in the **app settings** section.</span></span>  <span data-ttu-id="d2f1f-305">Det kan vara lämpligt när du vill ange värden som du inte vill ska visas i källorna (anslutningssträngar, lösenord osv) eller som du vill ställa in på olika sätt för Azure och den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-305">This can be useful for setting values that you may not want to appear in the sources (connection strings, passwords, etc), or that you want to set differently between Azure and your local machine.</span></span> <span data-ttu-id="d2f1f-306">I `settings.py` kan du söka i miljövariablerna med hjälp av `os.getenv`.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-306">In `settings.py`, you can query the environment variables using `os.getenv`.</span></span>

## <a name="using-a-database"></a><span data-ttu-id="d2f1f-307">Använda en databas</span><span class="sxs-lookup"><span data-stu-id="d2f1f-307">Using a Database</span></span>
<span data-ttu-id="d2f1f-308">Den databas som ingår i programmet är en sqlite-databas.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-308">The database that is included with the application is a sqlite database.</span></span> <span data-ttu-id="d2f1f-309">Det är en praktisk standarddatabas för utveckling, eftersom den knappt behöver konfigureras.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-309">This is a convenient and useful default database to use for development, as it requires almost no setup.</span></span> <span data-ttu-id="d2f1f-310">Databasen lagras i filen db.sqlite3 i projektmappen.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-310">The database is stored in the db.sqlite3 file in the project folder.</span></span>

<span data-ttu-id="d2f1f-311">Azure tillhandahåller databastjänster som är lätta att använda i ett Django-program.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-311">Azure provides database services which are easy to use from a Django application.</span></span> <span data-ttu-id="d2f1f-312">I kurserna om att använda [SQL Database] och [MySQL] i ett Django-program får du anvisningar för de steg som krävs för att skapa databastjänsten, ändra databasinställningarna i `DjangoWebProject/settings.py` och de bibliotek som måste installeras.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-312">Tutorials for using [SQL Database] and [MySQL] from a Django application show the steps necessary to create the database service, change the database settings in `DjangoWebProject/settings.py`, and the libraries required to install.</span></span>

<span data-ttu-id="d2f1f-313">Om du föredrar att hantera egna databasservrar kan du göra det med hjälp av virtuella Windows- och Linux-datorer som körs på Azure.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-313">Of course, if you prefer to manage your own database servers, you can do so using Windows or Linux virtual machines running on Azure.</span></span>

## <a name="django-admin-interface"></a><span data-ttu-id="d2f1f-314">Administratörsgränssnittet för Django</span><span class="sxs-lookup"><span data-stu-id="d2f1f-314">Django Admin Interface</span></span>
<span data-ttu-id="d2f1f-315">När du börjar bygga modeller vill du införa data i databasen.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-315">Once you start building your models, you'll want to populate the database with some data.</span></span> <span data-ttu-id="d2f1f-316">Ett enkelt sätt att lägga till och redigera innehåll interaktivt är att använda administratörsgränssnittet för Django.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-316">An easy way to do add and edit content interactively is to use the Django administration interface.</span></span>

<span data-ttu-id="d2f1f-317">Koden för administratörsgränssnittet har kommenterats ut i programkällorna, men den är tydligt markerad så att du enkelt kan aktivera den (Sök efter "admin").</span><span class="sxs-lookup"><span data-stu-id="d2f1f-317">The code for the admin interface is commented out in the application sources, but it's clearly marked so you can easily enable it (search for 'admin').</span></span>

<span data-ttu-id="d2f1f-318">När den är aktiverad, synkroniserar du databasen, kör programmet och går till `/admin`.</span><span class="sxs-lookup"><span data-stu-id="d2f1f-318">After it's enabled, synchronize the database, run the application and navigate to `/admin`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d2f1f-319">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d2f1f-319">Next Steps</span></span>
<span data-ttu-id="d2f1f-320">Via de här länkarna hittar du mer information om Django och Python Tools för Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="d2f1f-320">Follow these links to learn more about Django and Python Tools for Visual Studio:</span></span>

* <span data-ttu-id="d2f1f-321">[Django-dokumentation]</span><span class="sxs-lookup"><span data-stu-id="d2f1f-321">[Django Documentation]</span></span>
* <span data-ttu-id="d2f1f-322">[Dokumentationen om Python Tools för Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="d2f1f-322">[Python Tools for Visual Studio Documentation]</span></span>

<span data-ttu-id="d2f1f-323">Information om hur du använder SQL Database och MySQL finns i:</span><span class="sxs-lookup"><span data-stu-id="d2f1f-323">For information on using SQL Database and MySQL:</span></span>

* <span data-ttu-id="d2f1f-324">[Django och MySQL på Azure med Python Tools för Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="d2f1f-324">[Django and MySQL on Azure with Python Tools for Visual Studio]</span></span>
* <span data-ttu-id="d2f1f-325">[Django och SQL Database på Azure med Python Tools för Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="d2f1f-325">[Django and SQL Database on Azure with Python Tools for Visual Studio]</span></span>

<span data-ttu-id="d2f1f-326">Mer information finns i [Python Developer Center](/develop/python/).</span><span class="sxs-lookup"><span data-stu-id="d2f1f-326">For more information, see the [Python Developer Center](/develop/python/).</span></span>

## <a name="whats-changed"></a><span data-ttu-id="d2f1f-327">Nyheter</span><span class="sxs-lookup"><span data-stu-id="d2f1f-327">What's changed</span></span>
* <span data-ttu-id="d2f1f-328">En guide till övergången från Webbplatser till App Service finns i: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="d2f1f-328">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

<!--Link references-->
[Django och MySQL på Azure med Python Tools för Visual Studio]: web-sites-python-ptvs-django-mysql.md
[Django och SQL Database på Azure med Python Tools för Visual Studio]: web-sites-python-ptvs-django-sql.md
[SQL Database]: web-sites-python-ptvs-django-sql.md
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
[Dokumentationen om Python Tools för Visual Studio]: http://aka.ms/ptvsdocs
[Django-dokumentation]: https://www.djangoproject.com/
