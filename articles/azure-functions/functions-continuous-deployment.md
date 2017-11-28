---
title: "Kontinuerlig distribution för Azure Functions | Microsoft Docs"
description: "Använd kontinuerlig distribution av Azure App Service för att publicera dina Azure-funktioner."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 361daf37-598c-4703-8d78-c77dbef91643
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/25/2016
ms.author: glenga
ms.openlocfilehash: 3756f1a039730bfd99b0375ce9bfeaf27178f2e0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="continuous-deployment-for-azure-functions"></a><span data-ttu-id="52f08-103">Löpande distribution för Azure Functions</span><span class="sxs-lookup"><span data-stu-id="52f08-103">Continuous deployment for Azure Functions</span></span>
<span data-ttu-id="52f08-104">Azure Functions gör det enkelt att distribuera appen funktionen med hjälp av Apptjänst kontinuerlig integration.</span><span class="sxs-lookup"><span data-stu-id="52f08-104">Azure Functions makes it easy to deploy your function app using App Service continuous integration.</span></span> <span data-ttu-id="52f08-105">Functions kan integreras med BitBucket, Dropbox, GitHub eller Visual Studio Team Services VSTS ().</span><span class="sxs-lookup"><span data-stu-id="52f08-105">Functions integrates with BitBucket, Dropbox, GitHub, and Visual Studio Team Services (VSTS).</span></span> <span data-ttu-id="52f08-106">Detta gör att ett arbetsflöde där Funktionskoden uppdateringar genom att använda en av dessa integrerade tjänster utlösaren distribution till Azure.</span><span class="sxs-lookup"><span data-stu-id="52f08-106">This enables a workflow where function code updates made by using one of these integrated services trigger deployment to Azure.</span></span> <span data-ttu-id="52f08-107">Om du har använt Azure Functions, börja med [översikt över Azure Functions](functions-overview.md).</span><span class="sxs-lookup"><span data-stu-id="52f08-107">If you are new to Azure Functions, start with [Azure Functions Overview](functions-overview.md).</span></span>

<span data-ttu-id="52f08-108">Kontinuerlig distribution är ett bra alternativ för projekt där flera och ofta återkommande bidrag integreras.</span><span class="sxs-lookup"><span data-stu-id="52f08-108">Continuous deployment is a great option for projects where multiple and frequent contributions are being integrated.</span></span> <span data-ttu-id="52f08-109">Du kan även Underhåll källkontrollen på koden funktioner.</span><span class="sxs-lookup"><span data-stu-id="52f08-109">It also lets you maintain source control on your functions code.</span></span> <span data-ttu-id="52f08-110">Följande distribution källor stöds:</span><span class="sxs-lookup"><span data-stu-id="52f08-110">The following deployment sources are currently supported:</span></span>

* [<span data-ttu-id="52f08-111">Bitbucket</span><span class="sxs-lookup"><span data-stu-id="52f08-111">Bitbucket</span></span>](https://bitbucket.org/)
* [<span data-ttu-id="52f08-112">Dropbox</span><span class="sxs-lookup"><span data-stu-id="52f08-112">Dropbox</span></span>](https://www.dropbox.com/)
* <span data-ttu-id="52f08-113">Extern lagringsplats (Git eller ett)</span><span class="sxs-lookup"><span data-stu-id="52f08-113">External repository (Git or Mercurial)</span></span>
* [<span data-ttu-id="52f08-114">Lokal Git-lagringsplats</span><span class="sxs-lookup"><span data-stu-id="52f08-114">Git local repository</span></span>](../app-service-web/app-service-deploy-local-git.md)
* [<span data-ttu-id="52f08-115">GitHub</span><span class="sxs-lookup"><span data-stu-id="52f08-115">GitHub</span></span>](https://github.com)
* [<span data-ttu-id="52f08-116">OneDrive</span><span class="sxs-lookup"><span data-stu-id="52f08-116">OneDrive</span></span>](https://onedrive.live.com/)
* [<span data-ttu-id="52f08-117">Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="52f08-117">Visual Studio Team Services</span></span>](https://www.visualstudio.com/team-services/)

<span data-ttu-id="52f08-118">Distributioner konfigureras på grundval av per funktion app.</span><span class="sxs-lookup"><span data-stu-id="52f08-118">Deployments are configured on a per-function app basis.</span></span> <span data-ttu-id="52f08-119">Efter kontinuerlig distribution har aktiverats, åtkomst till Funktionskoden i portalen anges till *skrivskyddad*.</span><span class="sxs-lookup"><span data-stu-id="52f08-119">After continuous deployment is enabled, access to function code in the portal is set to *read-only*.</span></span>

## <a name="continuous-deployment-requirements"></a><span data-ttu-id="52f08-120">Krav för kontinuerlig distribution</span><span class="sxs-lookup"><span data-stu-id="52f08-120">Continuous deployment requirements</span></span>

<span data-ttu-id="52f08-121">Du måste ha din distributionskälla konfigurerad och funktioner koden i distributionskälla innan du kan ställa in kontinuerlig distribution.</span><span class="sxs-lookup"><span data-stu-id="52f08-121">You must have your deployment source configured and your functions code in the deployment source before you set up continuous deployment.</span></span> <span data-ttu-id="52f08-122">I en viss funktion appdistribution bor varje funktion i en namngiven underkatalog där katalognamnet är namnet på funktionen.</span><span class="sxs-lookup"><span data-stu-id="52f08-122">In a given function app deployment, each function lives in a named subdirectory, where the directory name is the name of the function.</span></span>  

[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

## <a name="set-up-continuous-deployment"></a><span data-ttu-id="52f08-123">Konfigurera kontinuerlig distribution</span><span class="sxs-lookup"><span data-stu-id="52f08-123">Set up continuous deployment</span></span>
<span data-ttu-id="52f08-124">Använd den här proceduren för att konfigurera kontinuerlig distribution för en befintlig funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="52f08-124">Use this procedure to configure continuous deployment for an existing function app.</span></span> <span data-ttu-id="52f08-125">Dessa steg visar integrering med en GitHub-databas, men liknande steg gäller för Visual Studio Team Services eller andra deployment services.</span><span class="sxs-lookup"><span data-stu-id="52f08-125">These steps demonstrate integration with a GitHub repository, but similar steps apply for Visual Studio Team Services or other deployment services.</span></span>

1. <span data-ttu-id="52f08-126">I funktionen appen i den [Azure-portalen](https://portal.azure.com), klickar du på **plattformsfunktioner** och **distributionsalternativ**.</span><span class="sxs-lookup"><span data-stu-id="52f08-126">In your function app in the [Azure portal](https://portal.azure.com), click **Platform features** and **Deployment options**.</span></span> 
   
    ![Konfigurera kontinuerlig distribution](./media/functions-continuous-deployment/setup-deployment.png)
 
2. <span data-ttu-id="52f08-128">I den **distributioner** bladet och klickar på **installationsprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="52f08-128">Then in the **Deployments** blade click **Setup**.</span></span>
 
    ![Konfigurera kontinuerlig distribution](./media/functions-continuous-deployment/setup-deployment-1.png)
   
2. <span data-ttu-id="52f08-130">I den **distributionskälla** bladet, klickar du på **Välj källa**, Fyll i informationen för din valda distributionskälla och klickar på **OK**.</span><span class="sxs-lookup"><span data-stu-id="52f08-130">In the **Deployment source** blade, click **Choose source**, then fill in the information for your chosen deployment source and click **OK**.</span></span>
   
    ![Välj distributionskälla](./media/functions-continuous-deployment/choose-deployment-source.png)

<span data-ttu-id="52f08-132">När kontinuerlig distribution har konfigurerats, alla ändringar i din distributionskälla kopieras till appen med funktionen och en fullständig webbplatsdistribution utlöses.</span><span class="sxs-lookup"><span data-stu-id="52f08-132">After continuous deployment is configured, all file changes in your deployment source are copied to the function app and a full site deployment is triggered.</span></span> <span data-ttu-id="52f08-133">Platsen är omdistribueras när filer i källinnehållet uppdateras.</span><span class="sxs-lookup"><span data-stu-id="52f08-133">The site is redeployed when files in the source are updated.</span></span>

## <a name="deployment-options"></a><span data-ttu-id="52f08-134">Distributionsalternativ</span><span class="sxs-lookup"><span data-stu-id="52f08-134">Deployment options</span></span>

<span data-ttu-id="52f08-135">Följande är några vanliga distributionsscenarier:</span><span class="sxs-lookup"><span data-stu-id="52f08-135">The following are some typical deployment scenarios:</span></span>

- [<span data-ttu-id="52f08-136">Skapa en fristående distribution</span><span class="sxs-lookup"><span data-stu-id="52f08-136">Create a staging deployment</span></span>](#staging)
- [<span data-ttu-id="52f08-137">Flytta befintliga funktioner för kontinuerlig distribution</span><span class="sxs-lookup"><span data-stu-id="52f08-137">Move existing functions to continuous deployment</span></span>](#existing)

<a name="staging"></a>
### <a name="create-a-staging-deployment"></a><span data-ttu-id="52f08-138">Skapa en fristående distribution</span><span class="sxs-lookup"><span data-stu-id="52f08-138">Create a staging deployment</span></span>

<span data-ttu-id="52f08-139">Funktionen appar stöder inte ännu distributionsplatser.</span><span class="sxs-lookup"><span data-stu-id="52f08-139">Function Apps doesn't yet support deployment slots.</span></span> <span data-ttu-id="52f08-140">Du kan dock fortfarande hantera separata mellanlagring och produktion distributioner med hjälp av kontinuerlig integrering.</span><span class="sxs-lookup"><span data-stu-id="52f08-140">However, you can still manage separate staging and production deployments by using continuous integration.</span></span>

<span data-ttu-id="52f08-141">Hur du konfigurerar och arbetar med en fristående distribution ser vanligtvis ut så här:</span><span class="sxs-lookup"><span data-stu-id="52f08-141">The process to configure and work with a staging deployment looks generally like this:</span></span>

1. <span data-ttu-id="52f08-142">Skapa två funktionen appar i din prenumeration, en för produktion och en för Förproduktion.</span><span class="sxs-lookup"><span data-stu-id="52f08-142">Create two function apps in your subscription, one for the production code and one for staging.</span></span> 

2. <span data-ttu-id="52f08-143">Skapa en distributionskälla om du inte redan har ett.</span><span class="sxs-lookup"><span data-stu-id="52f08-143">Create a deployment source, if you don't already have one.</span></span> <span data-ttu-id="52f08-144">Det här exemplet används [GitHub].</span><span class="sxs-lookup"><span data-stu-id="52f08-144">This example uses [GitHub].</span></span>

3. <span data-ttu-id="52f08-145">Om appen produktion funktionen Slutför de föregående stegen i **ställa in kontinuerlig distribution** och ange distribution gren till mastergrenen för GitHub-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="52f08-145">For your production function app, complete the preceding steps in **Set up continuous deployment** and set the deployment branch to the master branch of your GitHub repository.</span></span>
   
    ![Välj distributionen gren](./media/functions-continuous-deployment/choose-deployment-branch.png)

4. <span data-ttu-id="52f08-147">Upprepa det här steget för mellanlagring funktionsapp, men välja grenen mellanlagring i stället i GitHub-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="52f08-147">Repeat this step for the staging function app, but choose the staging branch instead in your GitHub repo.</span></span> <span data-ttu-id="52f08-148">Om din distribution-källan inte stöder förgrening, använder du en annan mapp.</span><span class="sxs-lookup"><span data-stu-id="52f08-148">If your deployment source doesn't support branching, use a different folder.</span></span>
    
5. <span data-ttu-id="52f08-149">Göra uppdateringar i din kod i fristående branch eller mapp och sedan kontrollera att ändringarna återspeglas i fristående distributionen.</span><span class="sxs-lookup"><span data-stu-id="52f08-149">Make updates to your code in the staging branch or folder, then verify that those changes are reflected in the staging deployment.</span></span>

6. <span data-ttu-id="52f08-150">När du testar, sammanfoga ändringarna från den fristående grenen i mastergrenen.</span><span class="sxs-lookup"><span data-stu-id="52f08-150">After testing, merge changes from the staging branch into the master branch.</span></span> <span data-ttu-id="52f08-151">Den här kopplingen utlöser distribution till funktionen produktionsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="52f08-151">This merge triggers deployment to the production function app.</span></span> <span data-ttu-id="52f08-152">Om din distribution-källan inte stöder filialer, kan du skriva över filer i mappen produktion med filer från den tillfälliga mappen.</span><span class="sxs-lookup"><span data-stu-id="52f08-152">If your deployment source doesn't support branches, overwrite the files in the production folder with the files from the staging folder.</span></span>

<a name="existing"></a>
### <a name="move-existing-functions-to-continuous-deployment"></a><span data-ttu-id="52f08-153">Flytta befintliga funktioner för kontinuerlig distribution</span><span class="sxs-lookup"><span data-stu-id="52f08-153">Move existing functions to continuous deployment</span></span>
<span data-ttu-id="52f08-154">Om du har befintliga funktioner som du har skapat och underhålls i portalen, måste du hämta din befintliga funktionen kodfiler med FTP eller lokal Git-lagringsplats innan du kan ställa in kontinuerlig distribution som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="52f08-154">When you have existing functions that you have created and maintained in the portal, you need to download your existing function code files using FTP or the local Git repository before you can set up continuous deployment as described above.</span></span> <span data-ttu-id="52f08-155">Du kan göra detta i App Service-inställningar för din funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="52f08-155">You can do this in the App Service settings for your function app.</span></span> <span data-ttu-id="52f08-156">När filerna har hämtats, du kan ladda upp dem till valda kontinuerlig distribution-källa.</span><span class="sxs-lookup"><span data-stu-id="52f08-156">After your files are downloaded, you can upload them to your chosen continuous deployment source.</span></span>

> [!NOTE]
> <span data-ttu-id="52f08-157">När du konfigurerar kontinuerlig integration kan att du inte längre kunna redigera källfilerna i Functions-portalen.</span><span class="sxs-lookup"><span data-stu-id="52f08-157">After you configure continuous integration, you will no longer be able to edit your source files in the Functions portal.</span></span>

- [<span data-ttu-id="52f08-158">Så här: Konfigurera autentiseringsuppgifter för distribution</span><span class="sxs-lookup"><span data-stu-id="52f08-158">How to: Configure deployment credentials</span></span>](#credentials)
- [<span data-ttu-id="52f08-159">Så här: hämta filer med FTP</span><span class="sxs-lookup"><span data-stu-id="52f08-159">How to: Download files using FTP</span></span>](#downftp)
- [<span data-ttu-id="52f08-160">Så här: hämta filer med hjälp av lokal Git-lagringsplats</span><span class="sxs-lookup"><span data-stu-id="52f08-160">How to: Download files using the local Git repository</span></span>](#downgit)

<a name="credentials"></a>
#### <a name="how-to-configure-deployment-credentials"></a><span data-ttu-id="52f08-161">Så här: Konfigurera autentiseringsuppgifter för distribution</span><span class="sxs-lookup"><span data-stu-id="52f08-161">How to: Configure deployment credentials</span></span>
<span data-ttu-id="52f08-162">Innan du kan hämta filer från din funktion med FTP- eller lokal Git-lagringsplats, måste du konfigurera dina autentiseringsuppgifter för att komma åt webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="52f08-162">Before you can download files from your function app with FTP or local Git repository, you must configure your credentials to access the site.</span></span> <span data-ttu-id="52f08-163">Autentiseringsuppgifterna är inställda på funktionen app-nivå.</span><span class="sxs-lookup"><span data-stu-id="52f08-163">Credentials are set at the Function app level.</span></span> <span data-ttu-id="52f08-164">Använd följande steg för att ange autentiseringsuppgifter för distribution i Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="52f08-164">Use the following steps to set deployment credentials in the Azure portal:</span></span>

1. <span data-ttu-id="52f08-165">I funktionen appen i den [Azure-portalen](https://portal.azure.com), klickar du på **plattformsfunktioner** och **distributionsbehörigheterna**.</span><span class="sxs-lookup"><span data-stu-id="52f08-165">In your function app in the [Azure portal](https://portal.azure.com), click **Platform features** and **Deployment credentials**.</span></span>
   
    ![Ange autentiseringsuppgifter för lokal distribution](./media/functions-continuous-deployment/setup-deployment-credentials.png)

2. <span data-ttu-id="52f08-167">Ange ett användarnamn och lösenord och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="52f08-167">Type in a username and password, then click **Save**.</span></span> <span data-ttu-id="52f08-168">Du kan nu använda autentiseringsuppgifterna för åtkomst till appen funktionen från FTP- eller inbyggda Git-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="52f08-168">You can now use these credentials to access your function app from FTP or the built-in Git repo.</span></span>

<a name="downftp"></a>
#### <a name="how-to-download-files-using-ftp"></a><span data-ttu-id="52f08-169">Så här: hämta filer med FTP</span><span class="sxs-lookup"><span data-stu-id="52f08-169">How to: Download files using FTP</span></span>

1. <span data-ttu-id="52f08-170">I funktionen appen i den [Azure-portalen](https://portal.azure.com), klickar du på **plattformsfunktioner** och **egenskaper**, kopiera värdena för **FTP/distribution användaren**, **värdnamn för FTP-**, och **FTPS värdnamn**.</span><span class="sxs-lookup"><span data-stu-id="52f08-170">In your function app in the [Azure portal](https://portal.azure.com), click **Platform features** and **Properties**, then copy the values for **FTP/Deployment User**, **FTP Host Name**, and **FTPS Host Name**.</span></span>  

    <span data-ttu-id="52f08-171">**FTP-/ Distributionsanvändare användaren** måste anges som visas i portalen, inklusive namnet på appen, för att ge rätt kontext för FTP-servern.</span><span class="sxs-lookup"><span data-stu-id="52f08-171">**FTP/Deployment User** must be entered as displayed in the portal, including the app name, to provide proper context for the FTP server.</span></span>
   
    ![Hämta information om distribution](./media/functions-continuous-deployment/get-deployment-credentials.png)

2. <span data-ttu-id="52f08-173">Använd informationen från FTP-klient som samlats in för att ansluta till din app och ladda ned källfilerna för dina funktioner.</span><span class="sxs-lookup"><span data-stu-id="52f08-173">From your FTP client, use the connection information you gathered to connect to your app and download the source files for your functions.</span></span>

<a name="downgit"></a>
#### <a name="how-to-download-files-using-a-local-git-repository"></a><span data-ttu-id="52f08-174">Så här: hämta filer med hjälp av en lokal Git-lagringsplats</span><span class="sxs-lookup"><span data-stu-id="52f08-174">How to: Download files using a local Git repository</span></span>

1. <span data-ttu-id="52f08-175">I funktionen appen i den [Azure-portalen](https://portal.azure.com), klickar du på **plattformsfunktioner** och **distributionsalternativ**.</span><span class="sxs-lookup"><span data-stu-id="52f08-175">In your function app in the [Azure portal](https://portal.azure.com), click **Platform features** and **Deployment options**.</span></span> 
   
    ![Konfigurera kontinuerlig distribution](./media/functions-continuous-deployment/setup-deployment.png)
 
2. <span data-ttu-id="52f08-177">I den **distributioner** bladet och klickar på **installationsprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="52f08-177">Then in the **Deployments** blade click **Setup**.</span></span>
 
    ![Konfigurera kontinuerlig distribution](./media/functions-continuous-deployment/setup-deployment-1.png)
   
2. <span data-ttu-id="52f08-179">I den **distributionskälla** bladet, klickar du på **lokal Git-lagringsplats** och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="52f08-179">In the **Deployment source** blade, click **Local Git repository** and then click **OK**.</span></span>

3. <span data-ttu-id="52f08-180">I **plattformsfunktioner**, klickar du på **egenskaper** och anteckna värdet för URL för Git.</span><span class="sxs-lookup"><span data-stu-id="52f08-180">In **Platform features**, click **Properties** and note the value of Git URL.</span></span> 
   
    ![Konfigurera kontinuerlig distribution](./media/functions-continuous-deployment/get-local-git-deployment-url.png)

4. <span data-ttu-id="52f08-182">Klona databasen på den lokala datorn med hjälp av Kommandotolken Git-medveten eller ditt favoritprogram Git-verktyget.</span><span class="sxs-lookup"><span data-stu-id="52f08-182">Clone the repository on your local machine using a Git-aware command prompt or your favorite Git tool.</span></span> <span data-ttu-id="52f08-183">Kommandot Git-klon ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="52f08-183">The Git clone command looks like this:</span></span>
   
        git clone https://username@my-function-app.scm.azurewebsites.net:443/my-function-app.git

5. <span data-ttu-id="52f08-184">Hämta filer från appen funktionen klonen på den lokala datorn, som i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="52f08-184">Fetch files from your function app to the clone on your local computer, as in the following example:</span></span>
   
        git pull origin master
   
    <span data-ttu-id="52f08-185">Om det krävs ange din [konfigurerat autentiseringsuppgifter för distribution](#credentials).</span><span class="sxs-lookup"><span data-stu-id="52f08-185">If requested, supply your [configured deployment credentials](#credentials).</span></span>  

<span data-ttu-id="52f08-186">[GitHub]: https://github.com/</span><span class="sxs-lookup"><span data-stu-id="52f08-186">[GitHub]: https://github.com/</span></span>
