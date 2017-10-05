---
title: Kontinuerlig distribution till Azure App Service | Microsoft Docs
description: "Läs om hur du aktiverar kontinuerlig distribution till Azure App Service."
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
ms.openlocfilehash: 7d978e623aaff25a9400090bd3ac18ed560d2ebf
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="continuous-deployment-to-azure-app-service"></a><span data-ttu-id="ac4e2-103">Kontinuerligt distribution till Azure App Service</span><span class="sxs-lookup"><span data-stu-id="ac4e2-103">Continuous Deployment to Azure App Service</span></span>
<span data-ttu-id="ac4e2-104">Den här självstudien visar hur du konfigurerar ett arbetsflöde för kontinuerlig distribution för en [Azure App Service]-app.</span><span class="sxs-lookup"><span data-stu-id="ac4e2-104">This tutorial shows you how to configure a continuous deployment workflow for your [Azure App Service] app.</span></span> <span data-ttu-id="ac4e2-105">Apptjänst-integrering med BitBucket, GitHub och [Visual Studio Team Services VSTS ()](https://www.visualstudio.com/team-services/) aktiverar ett arbetsflöde för kontinuerlig distribution där Azure tar emot de senaste uppdateringarna från projektet publiceras på någon av dessa tjänster.</span><span class="sxs-lookup"><span data-stu-id="ac4e2-105">App Service integration with BitBucket, GitHub, and [Visual Studio Team Services (VSTS)](https://www.visualstudio.com/team-services/) enables a continuous deployment workflow where Azure pulls in the most recent updates from your project published to one of these services.</span></span> <span data-ttu-id="ac4e2-106">Kontinuerlig distribution är ett bra alternativ för projekt där flera och ofta återkommande bidrag integreras.</span><span class="sxs-lookup"><span data-stu-id="ac4e2-106">Continuous deployment is a great option for projects where multiple and frequent contributions are being integrated.</span></span>

<span data-ttu-id="ac4e2-107">Ta reda på hur du konfigurerar kontinuerlig distribution manuellt från en moln-databas som inte finns med i Azure Portal (t.ex [GitLab](https://gitlab.com/)), se [ställa in kontinuerlig distribution med manuella steg](https://github.com/projectkudu/kudu/wiki/Continuous-deployment#setting-up-continuous-deployment-using-manual-steps).</span><span class="sxs-lookup"><span data-stu-id="ac4e2-107">To find out how to configure continuous deployment manually from a cloud repository not listed by the Azure Portal (such as [GitLab](https://gitlab.com/)), see [Setting up continuous deployment using manual steps](https://github.com/projectkudu/kudu/wiki/Continuous-deployment#setting-up-continuous-deployment-using-manual-steps).</span></span>

## <span data-ttu-id="ac4e2-108"><a name="overview"></a>Aktivera kontinuerlig distribution</span><span class="sxs-lookup"><span data-stu-id="ac4e2-108"><a name="overview"></a>Enable continuous deployment</span></span>
<span data-ttu-id="ac4e2-109">Så här aktiverar du kontinuerlig distribution:</span><span class="sxs-lookup"><span data-stu-id="ac4e2-109">To enable continuous deployment,</span></span>

1. <span data-ttu-id="ac4e2-110">Publicera ditt appinnehåll på den lagringsplats som ska användas för kontinuerlig distribution.</span><span class="sxs-lookup"><span data-stu-id="ac4e2-110">Publish your app content to the repository that will be used for continuous deployment.</span></span>  
    <span data-ttu-id="ac4e2-111">Mer information om hur du publicerar projektet till dessa tjänster finns i avsnitten [Skapa en repo (GitHub)], [Skapa en repo (BitBucket)] samt [Kom igång med VSTS].</span><span class="sxs-lookup"><span data-stu-id="ac4e2-111">For more information on publishing your project to these services, see [Create a repo (GitHub)], [Create a repo (BitBucket)], and [Get started with VSTS].</span></span>
2. <span data-ttu-id="ac4e2-112">Gå till appmenyns blad i [Azure Portal] och klicka på **APPDISTRIBUTION > Distributionsalternativ**.</span><span class="sxs-lookup"><span data-stu-id="ac4e2-112">In your app's menu blade in the [Azure portal], click **APP DEPLOYMENT > Deployment options**.</span></span> <span data-ttu-id="ac4e2-113">Klicka på **Välj källa** och välj sedan distributionskällan.</span><span class="sxs-lookup"><span data-stu-id="ac4e2-113">Click **Choose Source**, then select the deployment source.</span></span>  
   
    ![](./media/app-service-continuous-deployment/cd_options.png)
   
   > [!NOTE]
   > <span data-ttu-id="ac4e2-114">Information att konfigurera ett VSTS-konto för App Service-distribution finns i denna [självstudie](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).</span><span class="sxs-lookup"><span data-stu-id="ac4e2-114">To configure a VSTS account for App Service deployment please see this [tutorial](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).</span></span>
   > 
   > 
3. <span data-ttu-id="ac4e2-115">Slutför auktoriseringsarbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="ac4e2-115">Complete the authorization workflow.</span></span>
4. <span data-ttu-id="ac4e2-116">På bladet **Distributionskälla** väljer du projektet och den förgrening som du vill distribuera från.</span><span class="sxs-lookup"><span data-stu-id="ac4e2-116">In the **Deployment source** blade, choose the project and branch to deploy from.</span></span> <span data-ttu-id="ac4e2-117">Klicka på **OK** när du är klar.</span><span class="sxs-lookup"><span data-stu-id="ac4e2-117">When you're done, click **OK**.</span></span>
   
    ![](./media/app-service-continuous-deployment/github_option.png)
   
   > [!NOTE]
   > <span data-ttu-id="ac4e2-118">När du aktiverar kontinuerlig distribution med GitHub eller BitBucket visas både offentliga och privata projekt.</span><span class="sxs-lookup"><span data-stu-id="ac4e2-118">When enabling continuous deployment with GitHub or BitBucket, both public and private projects will be displayed.</span></span>
   > 
   > 
   
    <span data-ttu-id="ac4e2-119">App Service-skapar en association till den valda lagringsplatsen, tar emot filer från den angivna förgreningen och underhåller en klon av lagringsplatsen för App Service-appen.</span><span class="sxs-lookup"><span data-stu-id="ac4e2-119">App Service creates an association with the selected repository, pulls in the files from the specified branch, and maintains a clone of your repository for your App Service app.</span></span> <span data-ttu-id="ac4e2-120">När du konfigurerar kontinuerlig VSTS-distribution från Azure Portal använder integrationen [Kudu-distributionsmotor](https://github.com/projectkudu/kudu/wiki) för App Service, som redan automatiserar Build and Deployment-aktiviteter med varje `git push`.</span><span class="sxs-lookup"><span data-stu-id="ac4e2-120">When you configure VSTS continuous deployment from the Azure portal, the integration uses the App Service [Kudu deployment engine](https://github.com/projectkudu/kudu/wiki), which already automates build and deployment tasks with every `git push`.</span></span> <span data-ttu-id="ac4e2-121">Du behöver inte konfigurera kontinuerlig distribution separat i VSTS.</span><span class="sxs-lookup"><span data-stu-id="ac4e2-121">You do not need to separately set up continuous deployment in VSTS.</span></span> <span data-ttu-id="ac4e2-122">När den här processen är klar visas en aktiv distribution på appbladet **Distributionsalternativ** som anger att distributionen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="ac4e2-122">After this process completes, the **Deployment options** app blade will show an active deployment that indicates deployment has succeeded.</span></span>
5. <span data-ttu-id="ac4e2-123">Kontrollera att appen har distribuerats genom att klicka på **URL**:en överst på appbladet i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="ac4e2-123">To verify the app is successfully deployed, click the **URL** at the top of the app's blade in the Azure portal.</span></span>
6. <span data-ttu-id="ac4e2-124">Kontrollera att det sker en kontinuerlig distribution från önskad lagringsplats genom att ändringar push-överförs till lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="ac4e2-124">To verify that continuous deployment is occurring from the repository of your choice, push a change to the repository.</span></span> <span data-ttu-id="ac4e2-125">Din app ska uppdateras så att den avspeglar ändringarna strax efter det att push-överföringen till lagringsplatsen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="ac4e2-125">Your app should update to reflect the changes shortly after the push to the repository completes.</span></span> <span data-ttu-id="ac4e2-126">Du kan verifiera att den har hämtat uppdateringen på bladet **Distributionsalternativ** i appen.</span><span class="sxs-lookup"><span data-stu-id="ac4e2-126">You can verify that it has pulled in the update in the **Deployment options** blade of your app.</span></span>

## <span data-ttu-id="ac4e2-127"><a name="VSsolution"></a>Kontinuerlig distribution av en Visual Studio-lösning</span><span class="sxs-lookup"><span data-stu-id="ac4e2-127"><a name="VSsolution"></a>Continuous deployment of a Visual Studio solution</span></span>
<span data-ttu-id="ac4e2-128">Att push-överföra en Visual Studio-lösning till Azure App Service är lika enkelt som att push-överföra en enkel index.html-fil.</span><span class="sxs-lookup"><span data-stu-id="ac4e2-128">Pushing a Visual Studio solution to Azure App Service is just as easy as pushing a simple index.html file.</span></span> <span data-ttu-id="ac4e2-129">App Service-distributionsprocessen effektiviserar all information, inklusive återställning av NuGet-beroenden, och bygger upp binärfilerna för programmet.</span><span class="sxs-lookup"><span data-stu-id="ac4e2-129">The App Service deployment process streamlines all the details, including restoring NuGet dependencies and building the application binaries.</span></span> <span data-ttu-id="ac4e2-130">Du kan följa bästa praxis för källkontroll genom att upprätthålla kod enbart på Git-lagringsplatsen och låta App Service-distributionen ta hand om resten.</span><span class="sxs-lookup"><span data-stu-id="ac4e2-130">You can follow the source control best practices of maintaining code only in your Git repository, and let App Service deployment take care of the rest.</span></span>

<span data-ttu-id="ac4e2-131">Stegen för push-överföring av Visual Studio-lösningen till App Service är desamma som i [föregående avsnitt](#overview), förutsatt att du konfigurerar lösningen och lagringsplatsen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="ac4e2-131">The steps for pushing your Visual Studio solution to App Service are the same as in the [previous section](#overview), provided that you configure your solution and repository as follows:</span></span>

* <span data-ttu-id="ac4e2-132">Använd alternativet Visual Studio-källkontroll för att skapa en `.gitignore`-fil som på bilden nedan eller lägg till en `.gitignore`-fil manuellt i lagringsplatsens rot med innehåll som liknar det här [.gitignore-exemplet](https://github.com/github/gitignore/blob/master/VisualStudio.gitignore).</span><span class="sxs-lookup"><span data-stu-id="ac4e2-132">Use the Visual Studio source control option to generate a `.gitignore` file such as the image below or manually add a `.gitignore` file in your repository root with content similar to this [.gitignore sample](https://github.com/github/gitignore/blob/master/VisualStudio.gitignore).</span></span>
  
  ![](./media/app-service-continuous-deployment/VS_source_control.png)
* <span data-ttu-id="ac4e2-133">Lägg till hela lösningens katalogträd i lagringsplatsen, med .sln-filen i roten för lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="ac4e2-133">Add the entire solution's directory tree to your repository, with the .sln file in the repository root.</span></span>

<span data-ttu-id="ac4e2-134">När du har konfigurerat lagringsplatsen på det sätt som beskrivs, och konfigurerat appen i Azure för kontinuerlig publicering från en av Git-onlinedatabaserna, kan du utveckla ASP.NET-programmet lokalt i Visual Studio och kontinuerligt distribuera koden genom att push-överföra ändringarna till lagringsplatsen för Git-onlinedatabasen.</span><span class="sxs-lookup"><span data-stu-id="ac4e2-134">Once you have set up your repository as described, and configured your app in Azure for continuous publishing from one of the online Git repositories, you can develop your ASP.NET application locally in Visual Studio and continuously deploy your code simply by pushing your changes to your online Git repository.</span></span>

## <span data-ttu-id="ac4e2-135"><a name="disableCD"></a>Inaktivera kontinuerlig distribution</span><span class="sxs-lookup"><span data-stu-id="ac4e2-135"><a name="disableCD"></a>Disable continuous deployment</span></span>
<span data-ttu-id="ac4e2-136">Så här inaktiverar du kontinuerlig distribution:</span><span class="sxs-lookup"><span data-stu-id="ac4e2-136">To disable continuous deployment,</span></span>

1. <span data-ttu-id="ac4e2-137">Gå till appmenyns blad i [Azure Portal] och klicka på **APPDISTRIBUTION > Distributionsalternativ**.</span><span class="sxs-lookup"><span data-stu-id="ac4e2-137">In your app's menu blade in the [Azure portal], click **APP DEPLOYMENT > Deployment options**.</span></span> <span data-ttu-id="ac4e2-138">Klicka sedan på **Koppla från** på bladet **Distributionsalternativ**.</span><span class="sxs-lookup"><span data-stu-id="ac4e2-138">Then click **Disconnect** in the **Deployment options** blade.</span></span>
   
    ![](./media/app-service-continuous-deployment/cd_disconnect.png)
2. <span data-ttu-id="ac4e2-139">När du har svarat **Ja** på bekräftelsemeddelandet kan du gå tillbaka till appbladet och klicka på **APPDISTRIBUTION > Distributionsalternativ** om du vill konfigurera publicering från en annan källa.</span><span class="sxs-lookup"><span data-stu-id="ac4e2-139">After answering **Yes** to the confirmation message, you can return to your app's blade and click **APP DEPLOYMENT > Deployment options** if you would like to set up publishing from another source.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ac4e2-140">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="ac4e2-140">Additional Resources</span></span>
* [<span data-ttu-id="ac4e2-141">Undersöka vanliga problem med kontinuerlig distribution</span><span class="sxs-lookup"><span data-stu-id="ac4e2-141">How to investigate common issues with continuous deployment</span></span>](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment)
* <span data-ttu-id="ac4e2-142">[Använda PowerShell för Azure]</span><span class="sxs-lookup"><span data-stu-id="ac4e2-142">[How to use PowerShell for Azure]</span></span>
* <span data-ttu-id="ac4e2-143">[Använda Azures kommandoradsverktyg för Mac och Linux]</span><span class="sxs-lookup"><span data-stu-id="ac4e2-143">[How to use the Azure Command-Line Tools for Mac and Linux]</span></span>
* <span data-ttu-id="ac4e2-144">[Git-dokumentation]</span><span class="sxs-lookup"><span data-stu-id="ac4e2-144">[Git documentation]</span></span>
* [<span data-ttu-id="ac4e2-145">Kudu-projektet</span><span class="sxs-lookup"><span data-stu-id="ac4e2-145">Project Kudu</span></span>](https://github.com/projectkudu/kudu/wiki)
* [<span data-ttu-id="ac4e2-146">Använda Azure att automatiskt generera en CI/CD-pipeline för att distribuera en app för ASP.NET 4</span><span class="sxs-lookup"><span data-stu-id="ac4e2-146">Use Azure to automatically generate a CI/CD pipeline to deploy an ASP.NET 4 app</span></span>](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic)

> [!NOTE]
> <span data-ttu-id="ac4e2-147">Om du vill komma igång med Azure Apptjänst innan du registrerar dig för ett Azure-konto kan du gå till [Prova Apptjänst](https://azure.microsoft.com/try/app-service/). Där kan du direkt skapa en tillfällig startwebbapp i Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="ac4e2-147">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="ac4e2-148">Inget kreditkort krävs, och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="ac4e2-148">No credit cards required; no commitments.</span></span>
> 
> 

<span data-ttu-id="ac4e2-149">[Azure App Service]: https://azure.microsoft.com/en-us/documentation/articles/app-service-changes-existing-services/</span><span class="sxs-lookup"><span data-stu-id="ac4e2-149">[Azure App Service]: https://azure.microsoft.com/en-us/documentation/articles/app-service-changes-existing-services/</span></span>
<span data-ttu-id="ac4e2-150">[Azure Portal]: https://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="ac4e2-150">[Azure portal]: https://portal.azure.com</span></span>
[VSTS Portal]: https://www.visualstudio.com/en-us/products/visual-studio-team-services-vs.aspx
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
<span data-ttu-id="ac4e2-151">[Använda PowerShell för Azure]: /powershell/azureps-cmdlets-docs</span><span class="sxs-lookup"><span data-stu-id="ac4e2-151">[How to use PowerShell for Azure]: /powershell/azureps-cmdlets-docs</span></span>
<span data-ttu-id="ac4e2-152">[Använda Azures kommandoradsverktyg för Mac och Linux]:../cli-install-nodejs.md</span><span class="sxs-lookup"><span data-stu-id="ac4e2-152">[How to use the Azure Command-Line Tools for Mac and Linux]:../cli-install-nodejs.md</span></span>
<span data-ttu-id="ac4e2-153">[Git-dokumentation]: http://git-scm.com/documentation</span><span class="sxs-lookup"><span data-stu-id="ac4e2-153">[Git Documentation]: http://git-scm.com/documentation</span></span>

<span data-ttu-id="ac4e2-154">[Skapa en repo (GitHub)]: https://help.github.com/articles/create-a-repo</span><span class="sxs-lookup"><span data-stu-id="ac4e2-154">[Create a repo (GitHub)]: https://help.github.com/articles/create-a-repo</span></span>
<span data-ttu-id="ac4e2-155">[Skapa en repo (BitBucket)]: https://confluence.atlassian.com/display/BITBUCKET/Create+an+Account+and+a+Git+Repo</span><span class="sxs-lookup"><span data-stu-id="ac4e2-155">[Create a repo (BitBucket)]: https://confluence.atlassian.com/display/BITBUCKET/Create+an+Account+and+a+Git+Repo</span></span>
<span data-ttu-id="ac4e2-156">[Kom igång med VSTS]: https://www.visualstudio.com/docs/vsts-tfs-overview</span><span class="sxs-lookup"><span data-stu-id="ac4e2-156">[Get started with VSTS]: https://www.visualstudio.com/docs/vsts-tfs-overview</span></span>
