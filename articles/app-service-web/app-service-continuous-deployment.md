---
title: "aaaContinuous distribution tooAzure Apptjänst | Microsoft Docs"
description: "Lär dig hur tooenable kontinuerlig distribution tooAzure Apptjänst."
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: mollybos
ms.assetid: 6adb5c84-6cf3-424e-a336-c554f23b4000
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/28/2016
ms.author: dariagrigoriu
ms.openlocfilehash: 62a22cbda354fd5b0a1b9729c8c375408e75049f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="continuous-deployment-tooazure-app-service"></a><span data-ttu-id="2b1d8-103">Kontinuerlig distribution tooAzure Apptjänst</span><span class="sxs-lookup"><span data-stu-id="2b1d8-103">Continuous Deployment tooAzure App Service</span></span>
<span data-ttu-id="2b1d8-104">De här självstudierna visar hur tooconfigure ett arbetsflöde för kontinuerlig distribution för din [Azure App Service] app.</span><span class="sxs-lookup"><span data-stu-id="2b1d8-104">This tutorial shows you how tooconfigure a continuous deployment workflow for your [Azure App Service] app.</span></span> <span data-ttu-id="2b1d8-105">Apptjänst-integrering med BitBucket, GitHub och [Visual Studio Team Services VSTS ()](https://www.visualstudio.com/team-services/) gör en kontinuerlig arbetsflöde för distribution där Azure tar emot hello de senaste uppdateringarna från projektet publicerade tooone av dessa tjänster.</span><span class="sxs-lookup"><span data-stu-id="2b1d8-105">App Service integration with BitBucket, GitHub, and [Visual Studio Team Services (VSTS)](https://www.visualstudio.com/team-services/) enables a continuous deployment workflow where Azure pulls in hello most recent updates from your project published tooone of these services.</span></span> <span data-ttu-id="2b1d8-106">Kontinuerlig distribution är ett bra alternativ för projekt där flera och ofta återkommande bidrag integreras.</span><span class="sxs-lookup"><span data-stu-id="2b1d8-106">Continuous deployment is a great option for projects where multiple and frequent contributions are being integrated.</span></span>

<span data-ttu-id="2b1d8-107">toofind ut hur tooconfigure kontinuerlig distribution manuellt från en moln-databas som inte visas av hello Azure-portalen (exempelvis [GitLab](https://gitlab.com/)), se [ställa in kontinuerlig distribution med manuella steg](https://github.com/projectkudu/kudu/wiki/Continuous-deployment#setting-up-continuous-deployment-using-manual-steps).</span><span class="sxs-lookup"><span data-stu-id="2b1d8-107">toofind out how tooconfigure continuous deployment manually from a cloud repository not listed by hello Azure Portal (such as [GitLab](https://gitlab.com/)), see [Setting up continuous deployment using manual steps](https://github.com/projectkudu/kudu/wiki/Continuous-deployment#setting-up-continuous-deployment-using-manual-steps).</span></span>

## <span data-ttu-id="2b1d8-108"><a name="overview"></a>Aktivera kontinuerlig distribution</span><span class="sxs-lookup"><span data-stu-id="2b1d8-108"><a name="overview"></a>Enable continuous deployment</span></span>
<span data-ttu-id="2b1d8-109">tooenable kontinuerlig distribution</span><span class="sxs-lookup"><span data-stu-id="2b1d8-109">tooenable continuous deployment,</span></span>

1. <span data-ttu-id="2b1d8-110">Publicera appen innehåll toohello databasen som ska användas för kontinuerlig distribution.</span><span class="sxs-lookup"><span data-stu-id="2b1d8-110">Publish your app content toohello repository that will be used for continuous deployment.</span></span>  
    <span data-ttu-id="2b1d8-111">Mer information om hur du publicerar dina projekt toothese tjänster finns [skapa lagringsplatsen (GitHub)], [skapa lagringsplatsen (BitBucket)], och [Kom igång med VSTS].</span><span class="sxs-lookup"><span data-stu-id="2b1d8-111">For more information on publishing your project toothese services, see [Create a repo (GitHub)], [Create a repo (BitBucket)], and [Get started with VSTS].</span></span>
2. <span data-ttu-id="2b1d8-112">I bladet för din app-menyn i hello [Azure-portalen], klickar du på **APPDISTRIBUTION > distributionsalternativ**.</span><span class="sxs-lookup"><span data-stu-id="2b1d8-112">In your app's menu blade in hello [Azure portal], click **APP DEPLOYMENT > Deployment options**.</span></span> <span data-ttu-id="2b1d8-113">Klicka på **Välj källa**och välj hello distributionskälla.</span><span class="sxs-lookup"><span data-stu-id="2b1d8-113">Click **Choose Source**, then select hello deployment source.</span></span>  
   
    ![](./media/app-service-continuous-deployment/cd_options.png)
   
   > [!NOTE]
   > <span data-ttu-id="2b1d8-114">tooconfigure ett VSTS konto för distribution av App Service finns [kursen](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).</span><span class="sxs-lookup"><span data-stu-id="2b1d8-114">tooconfigure a VSTS account for App Service deployment please see this [tutorial](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).</span></span>
   > 
   > 
3. <span data-ttu-id="2b1d8-115">Slutför hello auktorisering arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="2b1d8-115">Complete hello authorization workflow.</span></span>
4. <span data-ttu-id="2b1d8-116">I hello **distributionskälla** bladet välj hello projekt och avdelningskontor toodeploy från.</span><span class="sxs-lookup"><span data-stu-id="2b1d8-116">In hello **Deployment source** blade, choose hello project and branch toodeploy from.</span></span> <span data-ttu-id="2b1d8-117">Klicka på **OK** när du är klar.</span><span class="sxs-lookup"><span data-stu-id="2b1d8-117">When you're done, click **OK**.</span></span>
   
    ![](./media/app-service-continuous-deployment/github_option.png)
   
   > [!NOTE]
   > <span data-ttu-id="2b1d8-118">När du aktiverar kontinuerlig distribution med GitHub eller BitBucket visas både offentliga och privata projekt.</span><span class="sxs-lookup"><span data-stu-id="2b1d8-118">When enabling continuous deployment with GitHub or BitBucket, both public and private projects will be displayed.</span></span>
   > 
   > 
   
    <span data-ttu-id="2b1d8-119">Skapar en association med hello markerade databasen Apptjänst, tar emot hello filer från hello angivna grenen och underhåller en klon av databasen för din Apptjänst-app.</span><span class="sxs-lookup"><span data-stu-id="2b1d8-119">App Service creates an association with hello selected repository, pulls in hello files from hello specified branch, and maintains a clone of your repository for your App Service app.</span></span> <span data-ttu-id="2b1d8-120">När du konfigurerar VSTS kontinuerlig distribution från hello Azure-portalen hello integration använder hello Apptjänst [Kudu-distributionsmotorn](https://github.com/projectkudu/kudu/wiki), vilken redan automatiserar bygg- och distributionsprocessen uppgifter med varje `git push`.</span><span class="sxs-lookup"><span data-stu-id="2b1d8-120">When you configure VSTS continuous deployment from hello Azure portal, hello integration uses hello App Service [Kudu deployment engine](https://github.com/projectkudu/kudu/wiki), which already automates build and deployment tasks with every `git push`.</span></span> <span data-ttu-id="2b1d8-121">Du behöver inte tooseparately ställa in kontinuerlig distribution i VSTS.</span><span class="sxs-lookup"><span data-stu-id="2b1d8-121">You do not need tooseparately set up continuous deployment in VSTS.</span></span> <span data-ttu-id="2b1d8-122">När den här processen är klar hello **distributionsalternativ** app bladet visar en aktiv distribution som anger att distributionen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="2b1d8-122">After this process completes, hello **Deployment options** app blade will show an active deployment that indicates deployment has succeeded.</span></span>
5. <span data-ttu-id="2b1d8-123">tooverify hello appen har distribuerats, klicka på hello **URL** hello överst på bladet hello app i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2b1d8-123">tooverify hello app is successfully deployed, click hello **URL** at hello top of hello app's blade in hello Azure portal.</span></span>
6. <span data-ttu-id="2b1d8-124">tooverify kontinuerlig distribution sker från valfri, hello lagringsplats push en ändring toohello databas.</span><span class="sxs-lookup"><span data-stu-id="2b1d8-124">tooverify that continuous deployment is occurring from hello repository of your choice, push a change toohello repository.</span></span> <span data-ttu-id="2b1d8-125">Appen bör uppdatera tooreflect hello ändringar kort efter hello push toohello databasen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="2b1d8-125">Your app should update tooreflect hello changes shortly after hello push toohello repository completes.</span></span> <span data-ttu-id="2b1d8-126">Du kan verifiera att den har mottagit hello uppdatering i hello **distributionsalternativ** bladet för din app.</span><span class="sxs-lookup"><span data-stu-id="2b1d8-126">You can verify that it has pulled in hello update in hello **Deployment options** blade of your app.</span></span>

## <span data-ttu-id="2b1d8-127"><a name="VSsolution"></a>Kontinuerlig distribution av en Visual Studio-lösning</span><span class="sxs-lookup"><span data-stu-id="2b1d8-127"><a name="VSsolution"></a>Continuous deployment of a Visual Studio solution</span></span>
<span data-ttu-id="2b1d8-128">Överföra en App Service med Visual Studio-lösning tooAzure är lika enkelt som att trycka på en enkel index.html-fil.</span><span class="sxs-lookup"><span data-stu-id="2b1d8-128">Pushing a Visual Studio solution tooAzure App Service is just as easy as pushing a simple index.html file.</span></span> <span data-ttu-id="2b1d8-129">Hej Apptjänst distributionsprocessen förenklar alla hello-information, inklusive återställa NuGet beroenden och bygga hello binärfiler.</span><span class="sxs-lookup"><span data-stu-id="2b1d8-129">hello App Service deployment process streamlines all hello details, including restoring NuGet dependencies and building hello application binaries.</span></span> <span data-ttu-id="2b1d8-130">Du kan följa hello källa kontrollen bästa praxis för underhåll av koden endast i Git-lagringsplatsen och låta App Service-distributionen tar hand om hello rest.</span><span class="sxs-lookup"><span data-stu-id="2b1d8-130">You can follow hello source control best practices of maintaining code only in your Git repository, and let App Service deployment take care of hello rest.</span></span>

<span data-ttu-id="2b1d8-131">hello steg för att överföra din tjänst i Visual Studio-lösning tooApp hello samma som i hello [föregående avsnitt](#overview), förutsatt att du konfigurerar din lösning och databasen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="2b1d8-131">hello steps for pushing your Visual Studio solution tooApp Service are hello same as in hello [previous section](#overview), provided that you configure your solution and repository as follows:</span></span>

* <span data-ttu-id="2b1d8-132">Använd hello Visual Studio källa kontrollen alternativet toogenerate en `.gitignore` filen, till exempel hello bild nedan eller Lägg manuellt till en `.gitignore` filen i Lagringsplatsens rot med innehåll liknande toothis [.gitignore exempel](https://github.com/github/gitignore/blob/master/VisualStudio.gitignore).</span><span class="sxs-lookup"><span data-stu-id="2b1d8-132">Use hello Visual Studio source control option toogenerate a `.gitignore` file such as hello image below or manually add a `.gitignore` file in your repository root with content similar toothis [.gitignore sample](https://github.com/github/gitignore/blob/master/VisualStudio.gitignore).</span></span>
  
  ![](./media/app-service-continuous-deployment/VS_source_control.png)
* <span data-ttu-id="2b1d8-133">Lägg till hello hela lösningen directory trädet tooyour databasen med hello SLN-filen i roten för hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="2b1d8-133">Add hello entire solution's directory tree tooyour repository, with hello .sln file in hello repository root.</span></span>

<span data-ttu-id="2b1d8-134">När du har Ställ in din databas som beskrivs och konfigurerat din app i Azure för kontinuerlig publicering från en hello online Git databaser, kan du utveckla din ASP.NET-program lokalt i Visual Studio och kontinuerligt av helt enkelt distribuera din kod Skicka ändringar tooyour online Git-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="2b1d8-134">Once you have set up your repository as described, and configured your app in Azure for continuous publishing from one of hello online Git repositories, you can develop your ASP.NET application locally in Visual Studio and continuously deploy your code simply by pushing your changes tooyour online Git repository.</span></span>

## <span data-ttu-id="2b1d8-135"><a name="disableCD"></a>Inaktivera kontinuerlig distribution</span><span class="sxs-lookup"><span data-stu-id="2b1d8-135"><a name="disableCD"></a>Disable continuous deployment</span></span>
<span data-ttu-id="2b1d8-136">toodisable kontinuerlig distribution</span><span class="sxs-lookup"><span data-stu-id="2b1d8-136">toodisable continuous deployment,</span></span>

1. <span data-ttu-id="2b1d8-137">I bladet för din app-menyn i hello [Azure-portalen], klickar du på **APPDISTRIBUTION > distributionsalternativ**.</span><span class="sxs-lookup"><span data-stu-id="2b1d8-137">In your app's menu blade in hello [Azure portal], click **APP DEPLOYMENT > Deployment options**.</span></span> <span data-ttu-id="2b1d8-138">Klicka på **frånkoppling** i hello **distributionsalternativ** bladet.</span><span class="sxs-lookup"><span data-stu-id="2b1d8-138">Then click **Disconnect** in hello **Deployment options** blade.</span></span>
   
    ![](./media/app-service-continuous-deployment/cd_disconnect.png)
2. <span data-ttu-id="2b1d8-139">Efter **Ja** toohello bekräftelsemeddelande som du kan returnera tooyour appens blad och klicka på **APPDISTRIBUTION > distributionsalternativ** om du vill att tooset upp publicering från en annan källa.</span><span class="sxs-lookup"><span data-stu-id="2b1d8-139">After answering **Yes** toohello confirmation message, you can return tooyour app's blade and click **APP DEPLOYMENT > Deployment options** if you would like tooset up publishing from another source.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2b1d8-140">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="2b1d8-140">Additional Resources</span></span>
* [<span data-ttu-id="2b1d8-141">Hur tooinvestigate vanliga problem med kontinuerlig distribution</span><span class="sxs-lookup"><span data-stu-id="2b1d8-141">How tooinvestigate common issues with continuous deployment</span></span>](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment)
* <span data-ttu-id="2b1d8-142">[Hur toouse PowerShell för Azure]</span><span class="sxs-lookup"><span data-stu-id="2b1d8-142">[How toouse PowerShell for Azure]</span></span>
* <span data-ttu-id="2b1d8-143">[Hur toouse hello Azure-kommandoradsverktyg för Mac- och Linux]</span><span class="sxs-lookup"><span data-stu-id="2b1d8-143">[How toouse hello Azure Command-Line Tools for Mac and Linux]</span></span>
* <span data-ttu-id="2b1d8-144">[Git-dokumentation]</span><span class="sxs-lookup"><span data-stu-id="2b1d8-144">[Git documentation]</span></span>
* [<span data-ttu-id="2b1d8-145">Kudu-projektet</span><span class="sxs-lookup"><span data-stu-id="2b1d8-145">Project Kudu</span></span>](https://github.com/projectkudu/kudu/wiki)
* [<span data-ttu-id="2b1d8-146">Använda Azure tooautomatically Generera en CI/CD-pipeline toodeploy en ASP.NET 4-app</span><span class="sxs-lookup"><span data-stu-id="2b1d8-146">Use Azure tooautomatically generate a CI/CD pipeline toodeploy an ASP.NET 4 app</span></span>](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic)

> [!NOTE]
> <span data-ttu-id="2b1d8-147">Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service.</span><span class="sxs-lookup"><span data-stu-id="2b1d8-147">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="2b1d8-148">Inget kreditkort krävs, och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="2b1d8-148">No credit cards required; no commitments.</span></span>
> 
> 

[Azure App Service]: https://azure.microsoft.com/en-us/documentation/articles/app-service-changes-existing-services/
[Azure-portalen]: https://portal.azure.com
[VSTS Portal]: https://www.visualstudio.com/en-us/products/visual-studio-team-services-vs.aspx
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[Hur toouse PowerShell för Azure]: /powershell/azureps-cmdlets-docs
[Hur toouse hello Azure-kommandoradsverktyg för Mac- och Linux]:../cli-install-nodejs.md
[Git-dokumentation]: http://git-scm.com/documentation

[skapa lagringsplatsen (GitHub)]: https://help.github.com/articles/create-a-repo
[skapa lagringsplatsen (BitBucket)]: https://confluence.atlassian.com/display/BITBUCKET/Create+an+Account+and+a+Git+Repo
[Kom igång med VSTS]: https://www.visualstudio.com/docs/vsts-tfs-overview
