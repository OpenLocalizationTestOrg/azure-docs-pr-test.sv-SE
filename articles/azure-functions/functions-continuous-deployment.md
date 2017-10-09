---
title: "aaaContinuous distribution för Azure Functions | Microsoft Docs"
description: "Använd kontinuerlig distribution för Azure App Service toopublish Azure Functions."
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
ms.openlocfilehash: 28c44f737dad3feab3cf54f7dd42b6a978d0617e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="continuous-deployment-for-azure-functions"></a><span data-ttu-id="1b605-103">Löpande distribution för Azure Functions</span><span class="sxs-lookup"><span data-stu-id="1b605-103">Continuous deployment for Azure Functions</span></span>
<span data-ttu-id="1b605-104">Azure Functions gör det enkelt toodeploy appen funktionen med hjälp av Apptjänst kontinuerlig integration.</span><span class="sxs-lookup"><span data-stu-id="1b605-104">Azure Functions makes it easy toodeploy your function app using App Service continuous integration.</span></span> <span data-ttu-id="1b605-105">Functions kan integreras med BitBucket, Dropbox, GitHub eller Visual Studio Team Services VSTS ().</span><span class="sxs-lookup"><span data-stu-id="1b605-105">Functions integrates with BitBucket, Dropbox, GitHub, and Visual Studio Team Services (VSTS).</span></span> <span data-ttu-id="1b605-106">Detta gör att ett arbetsflöde där Funktionskoden uppdateringar genom att använda en av dessa integrerade tjänster tooAzure för distribution av utlösare.</span><span class="sxs-lookup"><span data-stu-id="1b605-106">This enables a workflow where function code updates made by using one of these integrated services trigger deployment tooAzure.</span></span> <span data-ttu-id="1b605-107">Om du är ny tooAzure funktioner, starta med [översikt över Azure Functions](functions-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1b605-107">If you are new tooAzure Functions, start with [Azure Functions Overview](functions-overview.md).</span></span>

<span data-ttu-id="1b605-108">Kontinuerlig distribution är ett bra alternativ för projekt där flera och ofta återkommande bidrag integreras.</span><span class="sxs-lookup"><span data-stu-id="1b605-108">Continuous deployment is a great option for projects where multiple and frequent contributions are being integrated.</span></span> <span data-ttu-id="1b605-109">Du kan även Underhåll källkontrollen på koden funktioner.</span><span class="sxs-lookup"><span data-stu-id="1b605-109">It also lets you maintain source control on your functions code.</span></span> <span data-ttu-id="1b605-110">följande distribution källor hello stöds:</span><span class="sxs-lookup"><span data-stu-id="1b605-110">hello following deployment sources are currently supported:</span></span>

* [<span data-ttu-id="1b605-111">Bitbucket</span><span class="sxs-lookup"><span data-stu-id="1b605-111">Bitbucket</span></span>](https://bitbucket.org/)
* [<span data-ttu-id="1b605-112">Dropbox</span><span class="sxs-lookup"><span data-stu-id="1b605-112">Dropbox</span></span>](https://www.dropbox.com/)
* <span data-ttu-id="1b605-113">Extern lagringsplats (Git eller ett)</span><span class="sxs-lookup"><span data-stu-id="1b605-113">External repository (Git or Mercurial)</span></span>
* [<span data-ttu-id="1b605-114">Lokal Git-lagringsplats</span><span class="sxs-lookup"><span data-stu-id="1b605-114">Git local repository</span></span>](../app-service-web/app-service-deploy-local-git.md)
* [<span data-ttu-id="1b605-115">GitHub</span><span class="sxs-lookup"><span data-stu-id="1b605-115">GitHub</span></span>](https://github.com)
* [<span data-ttu-id="1b605-116">OneDrive</span><span class="sxs-lookup"><span data-stu-id="1b605-116">OneDrive</span></span>](https://onedrive.live.com/)
* [<span data-ttu-id="1b605-117">Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="1b605-117">Visual Studio Team Services</span></span>](https://www.visualstudio.com/team-services/)

<span data-ttu-id="1b605-118">Distributioner konfigureras på grundval av per funktion app.</span><span class="sxs-lookup"><span data-stu-id="1b605-118">Deployments are configured on a per-function app basis.</span></span> <span data-ttu-id="1b605-119">Kontinuerlig distribution har aktiverats åtkomst toofunction koden i hello portal anges när för*skrivskyddad*.</span><span class="sxs-lookup"><span data-stu-id="1b605-119">After continuous deployment is enabled, access toofunction code in hello portal is set too*read-only*.</span></span>

## <a name="continuous-deployment-requirements"></a><span data-ttu-id="1b605-120">Krav för kontinuerlig distribution</span><span class="sxs-lookup"><span data-stu-id="1b605-120">Continuous deployment requirements</span></span>

<span data-ttu-id="1b605-121">Du måste ha din distributionskälla konfigurerad och funktioner koden i hello distributionskälla innan du kan ställa in kontinuerlig distribution.</span><span class="sxs-lookup"><span data-stu-id="1b605-121">You must have your deployment source configured and your functions code in hello deployment source before you set up continuous deployment.</span></span> <span data-ttu-id="1b605-122">Varje funktion bor i en namngiven underkatalog där hello katalognamnet är hello namnet på hello-funktion i en viss funktion app-distribution.</span><span class="sxs-lookup"><span data-stu-id="1b605-122">In a given function app deployment, each function lives in a named subdirectory, where hello directory name is hello name of hello function.</span></span>  

[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

## <a name="set-up-continuous-deployment"></a><span data-ttu-id="1b605-123">Konfigurera kontinuerlig distribution</span><span class="sxs-lookup"><span data-stu-id="1b605-123">Set up continuous deployment</span></span>
<span data-ttu-id="1b605-124">Använd den här proceduren tooconfigure kontinuerlig distribution för en befintlig funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="1b605-124">Use this procedure tooconfigure continuous deployment for an existing function app.</span></span> <span data-ttu-id="1b605-125">Dessa steg visar integrering med en GitHub-databas, men liknande steg gäller för Visual Studio Team Services eller andra deployment services.</span><span class="sxs-lookup"><span data-stu-id="1b605-125">These steps demonstrate integration with a GitHub repository, but similar steps apply for Visual Studio Team Services or other deployment services.</span></span>

1. <span data-ttu-id="1b605-126">I funktionen appen i hello [Azure-portalen](https://portal.azure.com), klickar du på **plattformsfunktioner** och **distributionsalternativ**.</span><span class="sxs-lookup"><span data-stu-id="1b605-126">In your function app in hello [Azure portal](https://portal.azure.com), click **Platform features** and **Deployment options**.</span></span> 
   
    ![Konfigurera kontinuerlig distribution](./media/functions-continuous-deployment/setup-deployment.png)
 
2. <span data-ttu-id="1b605-128">I hello **distributioner** bladet och klickar på **installationsprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="1b605-128">Then in hello **Deployments** blade click **Setup**.</span></span>
 
    ![Konfigurera kontinuerlig distribution](./media/functions-continuous-deployment/setup-deployment-1.png)
   
2. <span data-ttu-id="1b605-130">I hello **distributionskälla** bladet, klickar du på **Välj källa**, fyll sedan i hello information för din valda distributionskälla och klickar på **OK**.</span><span class="sxs-lookup"><span data-stu-id="1b605-130">In hello **Deployment source** blade, click **Choose source**, then fill in hello information for your chosen deployment source and click **OK**.</span></span>
   
    ![Välj distributionskälla](./media/functions-continuous-deployment/choose-deployment-source.png)

<span data-ttu-id="1b605-132">När kontinuerlig distribution har konfigurerats, alla ändringar i din distributionskälla är kopierade toohello funktionsapp och en fullständig webbplatsdistribution utlöses.</span><span class="sxs-lookup"><span data-stu-id="1b605-132">After continuous deployment is configured, all file changes in your deployment source are copied toohello function app and a full site deployment is triggered.</span></span> <span data-ttu-id="1b605-133">hello plats är omdistribueras när filer i hello källan uppdateras.</span><span class="sxs-lookup"><span data-stu-id="1b605-133">hello site is redeployed when files in hello source are updated.</span></span>

## <a name="deployment-options"></a><span data-ttu-id="1b605-134">Distributionsalternativ</span><span class="sxs-lookup"><span data-stu-id="1b605-134">Deployment options</span></span>

<span data-ttu-id="1b605-135">hello följande är några vanliga distributionsscenarier:</span><span class="sxs-lookup"><span data-stu-id="1b605-135">hello following are some typical deployment scenarios:</span></span>

- [<span data-ttu-id="1b605-136">Skapa en fristående distribution</span><span class="sxs-lookup"><span data-stu-id="1b605-136">Create a staging deployment</span></span>](#staging)
- [<span data-ttu-id="1b605-137">Flytta befintliga funktioner toocontinuous distribution</span><span class="sxs-lookup"><span data-stu-id="1b605-137">Move existing functions toocontinuous deployment</span></span>](#existing)

<a name="staging"></a>
### <a name="create-a-staging-deployment"></a><span data-ttu-id="1b605-138">Skapa en fristående distribution</span><span class="sxs-lookup"><span data-stu-id="1b605-138">Create a staging deployment</span></span>

<span data-ttu-id="1b605-139">Funktionen appar stöder inte ännu distributionsplatser.</span><span class="sxs-lookup"><span data-stu-id="1b605-139">Function Apps doesn't yet support deployment slots.</span></span> <span data-ttu-id="1b605-140">Du kan dock fortfarande hantera separata mellanlagring och produktion distributioner med hjälp av kontinuerlig integrering.</span><span class="sxs-lookup"><span data-stu-id="1b605-140">However, you can still manage separate staging and production deployments by using continuous integration.</span></span>

<span data-ttu-id="1b605-141">Hej processen tooconfigure och arbeta med en fristående distribution vanligtvis ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="1b605-141">hello process tooconfigure and work with a staging deployment looks generally like this:</span></span>

1. <span data-ttu-id="1b605-142">Skapa två funktionen appar i din prenumeration, en för hello kod och en för Förproduktion.</span><span class="sxs-lookup"><span data-stu-id="1b605-142">Create two function apps in your subscription, one for hello production code and one for staging.</span></span> 

2. <span data-ttu-id="1b605-143">Skapa en distributionskälla om du inte redan har ett.</span><span class="sxs-lookup"><span data-stu-id="1b605-143">Create a deployment source, if you don't already have one.</span></span> <span data-ttu-id="1b605-144">Det här exemplet används [GitHub].</span><span class="sxs-lookup"><span data-stu-id="1b605-144">This example uses [GitHub].</span></span>

3. <span data-ttu-id="1b605-145">Om appen produktion funktionen fullständig hello föregående steg i **ställa in kontinuerlig distribution** och ange hello distribution gren toohello mastergrenen av GitHub-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="1b605-145">For your production function app, complete hello preceding steps in **Set up continuous deployment** and set hello deployment branch toohello master branch of your GitHub repository.</span></span>
   
    ![Välj distributionen gren](./media/functions-continuous-deployment/choose-deployment-branch.png)

4. <span data-ttu-id="1b605-147">Upprepa det här steget för hello mellanlagring funktionsapp, men välja hello mellanlagring gren i stället i GitHub-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="1b605-147">Repeat this step for hello staging function app, but choose hello staging branch instead in your GitHub repo.</span></span> <span data-ttu-id="1b605-148">Om din distribution-källan inte stöder förgrening, använder du en annan mapp.</span><span class="sxs-lookup"><span data-stu-id="1b605-148">If your deployment source doesn't support branching, use a different folder.</span></span>
    
5. <span data-ttu-id="1b605-149">Göra uppdateringar tooyour koden i hello mellanlagring branch eller mapp och sedan kontrollera att ändringarna återspeglas i hello mellanlagring av distribution.</span><span class="sxs-lookup"><span data-stu-id="1b605-149">Make updates tooyour code in hello staging branch or folder, then verify that those changes are reflected in hello staging deployment.</span></span>

6. <span data-ttu-id="1b605-150">Sammanfoga ändringar från hello mellanlagring gren till mastergrenen hello efter tester.</span><span class="sxs-lookup"><span data-stu-id="1b605-150">After testing, merge changes from hello staging branch into hello master branch.</span></span> <span data-ttu-id="1b605-151">Den här kopplingen utlöser distribution toohello funktionen produktionsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="1b605-151">This merge triggers deployment toohello production function app.</span></span> <span data-ttu-id="1b605-152">Om din distribution-källan inte stöder filialer, kan du skriva över hello filer i hello produktion mappen med hello filer från hello mellanlagring mapp.</span><span class="sxs-lookup"><span data-stu-id="1b605-152">If your deployment source doesn't support branches, overwrite hello files in hello production folder with hello files from hello staging folder.</span></span>

<a name="existing"></a>
### <a name="move-existing-functions-toocontinuous-deployment"></a><span data-ttu-id="1b605-153">Flytta befintliga funktioner toocontinuous distribution</span><span class="sxs-lookup"><span data-stu-id="1b605-153">Move existing functions toocontinuous deployment</span></span>
<span data-ttu-id="1b605-154">När du har befintliga funktioner som du har skapat och underhålls i hello portal toodownload måste din befintliga fungera kodfiler med hjälp av FTP- eller hello lokal Git-lagringsplats innan du kan ställa in kontinuerlig distribution som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="1b605-154">When you have existing functions that you have created and maintained in hello portal, you need toodownload your existing function code files using FTP or hello local Git repository before you can set up continuous deployment as described above.</span></span> <span data-ttu-id="1b605-155">Du kan göra detta i hello App Service-inställningar för din funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="1b605-155">You can do this in hello App Service settings for your function app.</span></span> <span data-ttu-id="1b605-156">När filerna har hämtats överför du dem. tooyour valt kontinuerlig distributionskälla.</span><span class="sxs-lookup"><span data-stu-id="1b605-156">After your files are downloaded, you can upload them tooyour chosen continuous deployment source.</span></span>

> [!NOTE]
> <span data-ttu-id="1b605-157">När du har konfigurerat kontinuerlig integration kommer du inte längre att kunna tooedit källfiler i hello Functions-portalen.</span><span class="sxs-lookup"><span data-stu-id="1b605-157">After you configure continuous integration, you will no longer be able tooedit your source files in hello Functions portal.</span></span>

- [<span data-ttu-id="1b605-158">Så här: Konfigurera autentiseringsuppgifter för distribution</span><span class="sxs-lookup"><span data-stu-id="1b605-158">How to: Configure deployment credentials</span></span>](#credentials)
- [<span data-ttu-id="1b605-159">Så här: hämta filer med FTP</span><span class="sxs-lookup"><span data-stu-id="1b605-159">How to: Download files using FTP</span></span>](#downftp)
- [<span data-ttu-id="1b605-160">Så här: hämta filer med hjälp av hello lokal Git-lagringsplats</span><span class="sxs-lookup"><span data-stu-id="1b605-160">How to: Download files using hello local Git repository</span></span>](#downgit)

<a name="credentials"></a>
#### <a name="how-to-configure-deployment-credentials"></a><span data-ttu-id="1b605-161">Så här: Konfigurera autentiseringsuppgifter för distribution</span><span class="sxs-lookup"><span data-stu-id="1b605-161">How to: Configure deployment credentials</span></span>
<span data-ttu-id="1b605-162">Innan du kan hämta filer från din funktion med FTP- eller lokal Git-lagringsplats, måste du konfigurera autentiseringsuppgifter tooaccess hello platsen.</span><span class="sxs-lookup"><span data-stu-id="1b605-162">Before you can download files from your function app with FTP or local Git repository, you must configure your credentials tooaccess hello site.</span></span> <span data-ttu-id="1b605-163">Autentiseringsuppgifter anges vid hello funktionen app-nivå.</span><span class="sxs-lookup"><span data-stu-id="1b605-163">Credentials are set at hello Function app level.</span></span> <span data-ttu-id="1b605-164">Använd hello följande steg tooset autentiseringsuppgifter för distribution i hello Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="1b605-164">Use hello following steps tooset deployment credentials in hello Azure portal:</span></span>

1. <span data-ttu-id="1b605-165">I funktionen appen i hello [Azure-portalen](https://portal.azure.com), klickar du på **plattformsfunktioner** och **distributionsbehörigheterna**.</span><span class="sxs-lookup"><span data-stu-id="1b605-165">In your function app in hello [Azure portal](https://portal.azure.com), click **Platform features** and **Deployment credentials**.</span></span>
   
    ![Ange autentiseringsuppgifter för lokal distribution](./media/functions-continuous-deployment/setup-deployment-credentials.png)

2. <span data-ttu-id="1b605-167">Ange ett användarnamn och lösenord och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="1b605-167">Type in a username and password, then click **Save**.</span></span> <span data-ttu-id="1b605-168">Nu kan du använda dessa autentiseringsuppgifter tooaccess funktionen appen från FTP- eller hello inbyggda Git-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="1b605-168">You can now use these credentials tooaccess your function app from FTP or hello built-in Git repo.</span></span>

<a name="downftp"></a>
#### <a name="how-to-download-files-using-ftp"></a><span data-ttu-id="1b605-169">Så här: hämta filer med FTP</span><span class="sxs-lookup"><span data-stu-id="1b605-169">How to: Download files using FTP</span></span>

1. <span data-ttu-id="1b605-170">I funktionen appen i hello [Azure-portalen](https://portal.azure.com), klickar du på **plattformsfunktioner** och **egenskaper**, kopiera hello värden för **FTP/distribution användaren**, **Värdnamn för FTP-**, och **FTPS värdnamn**.</span><span class="sxs-lookup"><span data-stu-id="1b605-170">In your function app in hello [Azure portal](https://portal.azure.com), click **Platform features** and **Properties**, then copy hello values for **FTP/Deployment User**, **FTP Host Name**, and **FTPS Host Name**.</span></span>  

    <span data-ttu-id="1b605-171">**FTP-/ Distributionsanvändare användaren** måste anges som visas i hello portal, inklusive hello programnamn, tooprovide rätt kontext för hello FTP-servern.</span><span class="sxs-lookup"><span data-stu-id="1b605-171">**FTP/Deployment User** must be entered as displayed in hello portal, including hello app name, tooprovide proper context for hello FTP server.</span></span>
   
    ![Hämta information om distribution](./media/functions-continuous-deployment/get-deployment-credentials.png)

2. <span data-ttu-id="1b605-173">Använd hello anslutningsinformation du samlade in tooconnect tooyour app från FTP-klient och hämta hello källfilerna för dina funktioner.</span><span class="sxs-lookup"><span data-stu-id="1b605-173">From your FTP client, use hello connection information you gathered tooconnect tooyour app and download hello source files for your functions.</span></span>

<a name="downgit"></a>
#### <a name="how-to-download-files-using-a-local-git-repository"></a><span data-ttu-id="1b605-174">Så här: hämta filer med hjälp av en lokal Git-lagringsplats</span><span class="sxs-lookup"><span data-stu-id="1b605-174">How to: Download files using a local Git repository</span></span>

1. <span data-ttu-id="1b605-175">I funktionen appen i hello [Azure-portalen](https://portal.azure.com), klickar du på **plattformsfunktioner** och **distributionsalternativ**.</span><span class="sxs-lookup"><span data-stu-id="1b605-175">In your function app in hello [Azure portal](https://portal.azure.com), click **Platform features** and **Deployment options**.</span></span> 
   
    ![Konfigurera kontinuerlig distribution](./media/functions-continuous-deployment/setup-deployment.png)
 
2. <span data-ttu-id="1b605-177">I hello **distributioner** bladet och klickar på **installationsprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="1b605-177">Then in hello **Deployments** blade click **Setup**.</span></span>
 
    ![Konfigurera kontinuerlig distribution](./media/functions-continuous-deployment/setup-deployment-1.png)
   
2. <span data-ttu-id="1b605-179">I hello **distributionskälla** bladet, klickar du på **lokal Git-lagringsplats** och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="1b605-179">In hello **Deployment source** blade, click **Local Git repository** and then click **OK**.</span></span>

3. <span data-ttu-id="1b605-180">I **plattformsfunktioner**, klickar du på **egenskaper** och anteckna värdet för hello för Git-URL.</span><span class="sxs-lookup"><span data-stu-id="1b605-180">In **Platform features**, click **Properties** and note hello value of Git URL.</span></span> 
   
    ![Konfigurera kontinuerlig distribution](./media/functions-continuous-deployment/get-local-git-deployment-url.png)

4. <span data-ttu-id="1b605-182">Klona hello-databasen på den lokala datorn med hjälp av Kommandotolken Git-medveten eller ditt favoritprogram Git-verktyget.</span><span class="sxs-lookup"><span data-stu-id="1b605-182">Clone hello repository on your local machine using a Git-aware command prompt or your favorite Git tool.</span></span> <span data-ttu-id="1b605-183">Hej Git klonade kommandot ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="1b605-183">hello Git clone command looks like this:</span></span>
   
        git clone https://username@my-function-app.scm.azurewebsites.net:443/my-function-app.git

5. <span data-ttu-id="1b605-184">Hämta filer från din funktion app toohello klon på den lokala datorn, som i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="1b605-184">Fetch files from your function app toohello clone on your local computer, as in hello following example:</span></span>
   
        git pull origin master
   
    <span data-ttu-id="1b605-185">Om det krävs ange din [konfigurerat autentiseringsuppgifter för distribution](#credentials).</span><span class="sxs-lookup"><span data-stu-id="1b605-185">If requested, supply your [configured deployment credentials](#credentials).</span></span>  

[GitHub]: https://github.com/
