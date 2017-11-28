---
title: aaaFlighting distribution (betatestning) i Azure App Service
description: "Lär dig hur tooflight nya funktioner i din app eller beta testa dina uppdateringar i den här självstudiekursen för slutpunkt till slutpunkt. Den samlar Apptjänst funktioner som kontinuerlig publicering, platser, routning av nätverkstrafik och Application Insights-integration."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 17953c51-38f8-442d-bb0b-f69c1542f0e9
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/02/2016
ms.author: cephalin
ms.openlocfilehash: e83477b1fe46be09e5baa7bc2bd239b840b05cf7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="flighting-deployment-beta-testing-in-azure-app-service"></a><span data-ttu-id="6c946-104">Förhandsversionstestning distribution (betatestning) i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="6c946-104">Flighting deployment (beta testing) in Azure App Service</span></span>
<span data-ttu-id="6c946-105">De här självstudierna visar hur toodo *förhandsversionstestning distributioner* hello genom att integrera olika funktioner i [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) och [Azure Application Insights](/services/application-insights/).</span><span class="sxs-lookup"><span data-stu-id="6c946-105">This tutorial shows you how toodo *flighting deployments* by integrating hello various capabilities of [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) and [Azure Application Insights](/services/application-insights/).</span></span>

<span data-ttu-id="6c946-106">*Förhandsversionstestning* är en Distributionsprocess som validerar en ny funktion eller ändra med ett begränsat antal verkliga kunder och är en större testning i form av produktionsscenario.</span><span class="sxs-lookup"><span data-stu-id="6c946-106">*Flighting* is a deployment process that validates a new feature or change with a limited number of real customers, and is a major testing in production scenario.</span></span> <span data-ttu-id="6c946-107">Det är akin toobeta testning och kallas ibland ”kontrollerade test svarta”.</span><span class="sxs-lookup"><span data-stu-id="6c946-107">It is akin toobeta testing and is sometimes known as "controlled test flight".</span></span> <span data-ttu-id="6c946-108">Många stora företag med webben använda den här metoden för att få tidig validering på deras app-uppdateringar i sina praxis [flexibel utveckling](https://en.wikipedia.org/wiki/Agile_software_development).</span><span class="sxs-lookup"><span data-stu-id="6c946-108">Many large enterprises with a web presence use this approach to get early validation on their app updates in their practice of [agile development](https://en.wikipedia.org/wiki/Agile_software_development).</span></span> <span data-ttu-id="6c946-109">Azure Apptjänst kan du toointegrate test i produktion med kontinuerlig publicering och Application Insights tooimplement hello samma DevOps-scenario.</span><span class="sxs-lookup"><span data-stu-id="6c946-109">Azure App Service enables you toointegrate test in production with continous publishing and Application Insights tooimplement hello same DevOps scenario.</span></span> <span data-ttu-id="6c946-110">Den här metoden fördelar:</span><span class="sxs-lookup"><span data-stu-id="6c946-110">Benefits of this approach include:</span></span>

* <span data-ttu-id="6c946-111">**Få verkliga feedback *innan* uppdateringar som är utgivna tooproduction** -hello endast sak bättre än får feedback så fort du släpper har blivit feedback innan du släpper.</span><span class="sxs-lookup"><span data-stu-id="6c946-111">**Gain real feedback *before* updates are released tooproduction** - hello only thing better than gaining feedback as soon as you release is gaining feedback before you release.</span></span> <span data-ttu-id="6c946-112">Du kan testa uppdateringar med verkliga användaraktiviteten och beteenden tidigt du önskar i hello produktens livscykel.</span><span class="sxs-lookup"><span data-stu-id="6c946-112">You can test updates with real user traffic and behaviors as early as you desire in hello product life cycle.</span></span>
* <span data-ttu-id="6c946-113">**Förbättra [kontinuerlig test-driven utveckling (CTDD)](https://en.wikipedia.org/wiki/Continuous_test-driven_development) ** – genom att integrera test i produktion med kontinuerlig integrering och instrumentation med Application Insights validering av användare händer tidigt och automatiskt i din produktens livscykel.</span><span class="sxs-lookup"><span data-stu-id="6c946-113">**Enhance [continuous test-driven development (CTDD)](https://en.wikipedia.org/wiki/Continuous_test-driven_development)** - By integrating test in production with continuous integration and instrumentation with Application Insights, user validation happens early and automatically in your product life cycle.</span></span> <span data-ttu-id="6c946-114">Detta minskar tiden investeringar i manuell testkörning.</span><span class="sxs-lookup"><span data-stu-id="6c946-114">This helps reduce time investments in manual test execution.</span></span>
* <span data-ttu-id="6c946-115">**Optimera testa arbetsflödet** -genom att automatisera test i produktion med kontinuerlig övervakning instrumentation, du kan potentiellt göra hello målen för olika typer av testerna i en enda process som [integrering](https://en.wikipedia.org/wiki/Integration_testing), [regression](https://en.wikipedia.org/wiki/Regression_testing), [användbarhet](https://en.wikipedia.org/wiki/Usability_testing), tillgänglighet, lokalisering, [prestanda](https://en.wikipedia.org/wiki/Software_performance_testing), [säkerhet](https://en.wikipedia.org/wiki/Security_testing), och [ godkännande](https://en.wikipedia.org/wiki/Acceptance_testing).</span><span class="sxs-lookup"><span data-stu-id="6c946-115">**Optimize test workflow** - By automating test in production with continuous monitoring instrumentation, you can potentially accomplish hello goals of various kinds of tests in a single process, such as [integration](https://en.wikipedia.org/wiki/Integration_testing), [regression](https://en.wikipedia.org/wiki/Regression_testing), [usability](https://en.wikipedia.org/wiki/Usability_testing), accessibility, localization, [performance](https://en.wikipedia.org/wiki/Software_performance_testing), [security](https://en.wikipedia.org/wiki/Security_testing), and [acceptance](https://en.wikipedia.org/wiki/Acceptance_testing).</span></span>

<span data-ttu-id="6c946-116">En flighting distribution inte bara om routning live trafik.</span><span class="sxs-lookup"><span data-stu-id="6c946-116">A flighting deployment is not just about routing live traffic.</span></span> <span data-ttu-id="6c946-117">Typen av distribution vill du toogain insight så snabbt som möjligt, vare sig det är ett oväntat fel, prestandaförsämring, user experience problem.</span><span class="sxs-lookup"><span data-stu-id="6c946-117">In such a deployment you want toogain insight as quickly as possible, whether it be an unexpected bug, performance degradation, user experience issues.</span></span> <span data-ttu-id="6c946-118">Kom ihåg att du arbetar med verkliga kunder.</span><span class="sxs-lookup"><span data-stu-id="6c946-118">Remember, you are dealing with real customers.</span></span> <span data-ttu-id="6c946-119">Så toodo den höger, måste du se till att du har konfigurerat din flighting distribution toogather alla hello-data som du behöver i ordning toomake ett välgrundat beslut för din nästa steg.</span><span class="sxs-lookup"><span data-stu-id="6c946-119">So toodo it right, you must make sure that you have set up your flighting deployment toogather all hello data you need in order toomake an informed decision for your next step.</span></span> <span data-ttu-id="6c946-120">De här självstudierna visar hur toocollect data med Application Insights, men du kan använda New Relic eller andra tekniker som passar ditt scenario.</span><span class="sxs-lookup"><span data-stu-id="6c946-120">This tutorial shows you how toocollect data with Application Insights, but you can use New Relic or other technologies that suits your scenario.</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="6c946-121">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="6c946-121">What you will do</span></span>
<span data-ttu-id="6c946-122">I den här kursen får du lära dig hur toobring hello följande scenarier tillsammans tootest Apptjänst-app i produktion:</span><span class="sxs-lookup"><span data-stu-id="6c946-122">In this tutorial, you will learn how toobring hello following scenarios together tootest your App Service app in production:</span></span>

* <span data-ttu-id="6c946-123">[Vidarebefordra trafik för produktion](app-service-web-test-in-production-get-start.md) tooyour beta-app</span><span class="sxs-lookup"><span data-stu-id="6c946-123">[Route production traffic](app-service-web-test-in-production-get-start.md) tooyour beta app</span></span>
* <span data-ttu-id="6c946-124">[Instrumentera appen](../application-insights/app-insights-web-track-usage.md) tooobtain användbara mätvärden</span><span class="sxs-lookup"><span data-stu-id="6c946-124">[Instrument your app](../application-insights/app-insights-web-track-usage.md) tooobtain useful metrics</span></span>
* <span data-ttu-id="6c946-125">Distribuera appen beta och spåra aktiva app mått kontinuerligt</span><span class="sxs-lookup"><span data-stu-id="6c946-125">Continuously deploy your beta app and track live app metrics</span></span>
* <span data-ttu-id="6c946-126">Jämför mått mellan hello produktionsprogrammet och hello beta app toosee hur koden ändras översätta tooresults</span><span class="sxs-lookup"><span data-stu-id="6c946-126">Compare metrics between hello production app and hello beta app toosee how code changes translate tooresults</span></span>

## <a name="what-you-will-need"></a><span data-ttu-id="6c946-127">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="6c946-127">What you will need</span></span>
* <span data-ttu-id="6c946-128">Ett Azure-konto</span><span class="sxs-lookup"><span data-stu-id="6c946-128">An Azure account</span></span>
* <span data-ttu-id="6c946-129">En [GitHub](https://github.com/) konto</span><span class="sxs-lookup"><span data-stu-id="6c946-129">A [GitHub](https://github.com/) account</span></span>
* <span data-ttu-id="6c946-130">Visual Studio 2015 – du kan ladda ned hello [Community edition](https://www.visualstudio.com/en-us/products/visual-studio-express-vs.aspx).</span><span class="sxs-lookup"><span data-stu-id="6c946-130">Visual Studio 2015 - you can download hello [Community edition](https://www.visualstudio.com/en-us/products/visual-studio-express-vs.aspx).</span></span>
* <span data-ttu-id="6c946-131">Git Shell (installeras med [GitHub för Windows](https://windows.github.com/))-på så sätt kan du toorun båda hello Git och PowerShell-kommandon i hello samma session</span><span class="sxs-lookup"><span data-stu-id="6c946-131">Git Shell (installed with [GitHub for Windows](https://windows.github.com/)) - this enables you toorun both hello Git and PowerShell commands in hello same session</span></span>
* <span data-ttu-id="6c946-132">Senaste [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/download/v0.9.8-September2015/azure-powershell.0.9.8.msi) bits</span><span class="sxs-lookup"><span data-stu-id="6c946-132">Latest [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/download/v0.9.8-September2015/azure-powershell.0.9.8.msi) bits</span></span>
* <span data-ttu-id="6c946-133">Grundläggande förståelse av hello följande:</span><span class="sxs-lookup"><span data-stu-id="6c946-133">Basic understanding of hello following:</span></span>
  * <span data-ttu-id="6c946-134">[Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) malldistribution (se [distribuerar ett komplexa program förutsägbart i Azure](app-service-deploy-complex-application-predictably.md))</span><span class="sxs-lookup"><span data-stu-id="6c946-134">[Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) template deployment (see [Deploy a complex application predictably in Azure](app-service-deploy-complex-application-predictably.md))</span></span>
  * [<span data-ttu-id="6c946-135">Git</span><span class="sxs-lookup"><span data-stu-id="6c946-135">Git</span></span>](http://git-scm.com/documentation)
  * [<span data-ttu-id="6c946-136">PowerShell</span><span class="sxs-lookup"><span data-stu-id="6c946-136">PowerShell</span></span>](https://technet.microsoft.com/library/bb978526.aspx)

> [!NOTE]
> <span data-ttu-id="6c946-137">Du behöver ett Azure-konto toocomplete den här kursen:</span><span class="sxs-lookup"><span data-stu-id="6c946-137">You need an Azure account toocomplete this tutorial:</span></span>
>
> * <span data-ttu-id="6c946-138">Du kan [öppna ett Azure-konto gratis](https://azure.microsoft.com/pricing/free-trial/) – du får kredit du kan använda tootry ut betald Azure-tjänster och även när de används du kan behålla hello kontot och använda kostnadsfria Azure-tjänster, till exempel Web Apps.</span><span class="sxs-lookup"><span data-stu-id="6c946-138">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/) - You get credits you can use tootry out paid Azure services, and even after they're used up you can keep hello account and use free Azure services, such as Web Apps.</span></span>
> * <span data-ttu-id="6c946-139">Du kan [aktivera Visual Studio-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) -din Visual Studio-prenumeration ger dig krediter varje månad som du kan använda för Azure-betaltjänster.</span><span class="sxs-lookup"><span data-stu-id="6c946-139">You can [activate Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) - Your Visual Studio subscription gives you credits every month that you can use for paid Azure services.</span></span>
>
> <span data-ttu-id="6c946-140">Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service.</span><span class="sxs-lookup"><span data-stu-id="6c946-140">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="6c946-141">Inget kreditkort krävs, och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="6c946-141">No credit cards required; no commitments.</span></span>
>
>

## <a name="set-up-your-production-web-app"></a><span data-ttu-id="6c946-142">Konfigurera ditt webbprogram för produktion</span><span class="sxs-lookup"><span data-stu-id="6c946-142">Set up your production web app</span></span>
> [!NOTE]
> <span data-ttu-id="6c946-143">hello-skriptet som används i den här självstudiekursen konfigurerar automatiskt kontinuerlig publicering från GitHub-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="6c946-143">hello script used in this tutorial will automatically configure continuous publishing from your GitHub repository.</span></span> <span data-ttu-id="6c946-144">Detta kräver att dina GitHub-autentiseringsuppgifter lagras redan i Azure, annars hello skripta distribution misslyckas vid försök tooconfigure inställningar för källkontrollen för hello web apps.</span><span class="sxs-lookup"><span data-stu-id="6c946-144">This requires that your GitHub credentials are already stored in Azure, otherwise hello scripted deployment will fail when attempting tooconfigure source control settings for hello web apps.</span></span>
>
> <span data-ttu-id="6c946-145">toostore din GitHub autentiseringsuppgifter i Azure, skapa en webbapp i hello [Azure Portal](https://portal.azure.com/) och [konfigurera GitHub distribution](app-service-continuous-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="6c946-145">toostore your GitHub credentials in Azure, create a web app in hello [Azure Portal](https://portal.azure.com/) and [configure GitHub deployment](app-service-continuous-deployment.md).</span></span> <span data-ttu-id="6c946-146">Du behöver bara toodo detta en gång.</span><span class="sxs-lookup"><span data-stu-id="6c946-146">You only need toodo this once.</span></span>
>
>

<span data-ttu-id="6c946-147">Du har ett program som körs live i Azure i ett typiskt scenario för DevOps, och du vill toomake ändringar tooit genom kontinuerlig publicering.</span><span class="sxs-lookup"><span data-stu-id="6c946-147">In a typical DevOps scenario, you have an application that’s running live in Azure, and you want toomake changes tooit through continuous publishing.</span></span> <span data-ttu-id="6c946-148">I det här scenariot distribuerar du tooproduction en mall som du har utvecklats och testats.</span><span class="sxs-lookup"><span data-stu-id="6c946-148">In this scenario, you will deploy tooproduction a template that you have developed and tested.</span></span>

1. <span data-ttu-id="6c946-149">Skapa din egen förgrening av hello [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) databasen.</span><span class="sxs-lookup"><span data-stu-id="6c946-149">Create your own fork of hello [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) repository.</span></span> <span data-ttu-id="6c946-150">Information om hur du skapar din förgrening finns [duplicera en Repo](https://help.github.com/articles/fork-a-repo/).</span><span class="sxs-lookup"><span data-stu-id="6c946-150">For information on creating your fork, see [Fork a Repo](https://help.github.com/articles/fork-a-repo/).</span></span> <span data-ttu-id="6c946-151">När din förgrening har skapats kan se du den i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="6c946-151">Once your fork is created, you can see it in your browser.</span></span>

   ![](./media/app-service-agile-software-development/production-1-private-repo.png)
2. <span data-ttu-id="6c946-152">Öppna en session för Git-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="6c946-152">Open a Git Shell session.</span></span> <span data-ttu-id="6c946-153">Om du inte har Git Shell ännu, installera [GitHub för Windows](https://windows.github.com/) nu.</span><span class="sxs-lookup"><span data-stu-id="6c946-153">If you don't have Git Shell yet, install [GitHub for Windows](https://windows.github.com/) now.</span></span>
3. <span data-ttu-id="6c946-154">Skapa en lokal kloning av din förgrening genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="6c946-154">Create a local clone of your fork by executing hello following command:</span></span>

     <span data-ttu-id="6c946-155">Git-klon https://github.com/<your_fork>/ToDoApp.git</span><span class="sxs-lookup"><span data-stu-id="6c946-155">git clone https://github.com/<your_fork>/ToDoApp.git</span></span>
4. <span data-ttu-id="6c946-156">När du har din lokala klona navigera för*&lt;repository_root >*\ARMTemplates och kör hello deploy.ps1 skriptet med ett unikt suffix som visas nedan:</span><span class="sxs-lookup"><span data-stu-id="6c946-156">Once you have your local clone, navigate too*&lt;repository_root>*\ARMTemplates, and run hello deploy.ps1 script with a unique suffix, as shown below:</span></span>

     <span data-ttu-id="6c946-157">.\deploy.ps1 – RepoUrl https://github.com/<your_fork>/todoapp.git - ResourceGroupSuffix < your_suffix ></span><span class="sxs-lookup"><span data-stu-id="6c946-157">.\deploy.ps1 –RepoUrl https://github.com/<your_fork>/todoapp.git -ResourceGroupSuffix <your_suffix></span></span>
5. <span data-ttu-id="6c946-158">När du uppmanas önskat ange hello användarnamn och lösenord för åtkomst till databasen.</span><span class="sxs-lookup"><span data-stu-id="6c946-158">When prompted, type in hello desired username and password for database access.</span></span> <span data-ttu-id="6c946-159">Spara autentiseringsuppgifter för databasen eftersom du behöver toospecify dem igen när uppdatering hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="6c946-159">Remember your database credentials because you will need toospecify them again when updating hello resource group.</span></span>

   <span data-ttu-id="6c946-160">Du bör se hello etablering förloppet för olika Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="6c946-160">You should see hello provisioning progress of various Azure resources.</span></span> <span data-ttu-id="6c946-161">När distributionen är klar kommer hello skriptet Starta hello program i hello webbläsare och ger dig ett eget signal.</span><span class="sxs-lookup"><span data-stu-id="6c946-161">When deployment completes, hello script will launch hello application in hello browser and give you a friendly beep.</span></span>
   ![](./media/app-service-web-test-in-production-controlled-test-flight/00.1-app-in-browser.png)
6. <span data-ttu-id="6c946-162">Tillbaka i sessionen Git Shell, kör du:</span><span class="sxs-lookup"><span data-stu-id="6c946-162">Back in your Git Shell session, run:</span></span>

     <span data-ttu-id="6c946-163">. \swap – namn ToDoApp < your_suffix ></span><span class="sxs-lookup"><span data-stu-id="6c946-163">.\swap –Name ToDoApp<your_suffix></span></span>

   ![](./media/app-service-web-test-in-production-controlled-test-flight/00.2-swap-to-production.png)
7. <span data-ttu-id="6c946-164">När hello skriptet är klar går du tillbaka toobrowse toohello frontend-adress (http://ToDoApp*&lt;your_suffix >*.azurewebsites.net/) toosee hello program som körs i produktion.</span><span class="sxs-lookup"><span data-stu-id="6c946-164">When hello script finishes, go back toobrowse toohello frontend’s address (http://ToDoApp*&lt;your_suffix>*.azurewebsites.net/) toosee hello application running in production.</span></span>
8. <span data-ttu-id="6c946-165">Logga in på hello [Azure Portal](https://portal.azure.com/) och ta en titt på vad som har skapats.</span><span class="sxs-lookup"><span data-stu-id="6c946-165">Log into hello [Azure Portal](https://portal.azure.com/) and take a look at what’s created.</span></span>

   <span data-ttu-id="6c946-166">Du bör vara kan toosee två web apps i hello samma resursgrupp, en med hello `Api` suffix i hello namn.</span><span class="sxs-lookup"><span data-stu-id="6c946-166">You should be able toosee two web apps in hello same resource group, one with hello `Api` suffix in hello name.</span></span> <span data-ttu-id="6c946-167">Om du tittar på hello resursgruppvy visas också hello SQL-databas och server, hello App Service-plan och hello fristående fack för hello web apps.</span><span class="sxs-lookup"><span data-stu-id="6c946-167">If you look at hello resource group view, you will also see hello SQL Database and server, hello App Service plan, and hello staging slots for hello web apps.</span></span> <span data-ttu-id="6c946-168">Bläddra igenom hello olika resurser och jämför dem med * &lt;repository_root >*\ARMTemplates\ProdAndStage.json toosee hur de är konfigurerade i hello mallen.</span><span class="sxs-lookup"><span data-stu-id="6c946-168">Browse through hello different resources and compare them with *&lt;repository_root>*\ARMTemplates\ProdAndStage.json toosee how they are configured in hello template.</span></span>

   ![](./media/app-service-web-test-in-production-controlled-test-flight/00.3-resource-group-view.png)

<span data-ttu-id="6c946-169">Du har lagt upp hello produktionsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="6c946-169">You have set up hello production app.</span></span>  <span data-ttu-id="6c946-170">Nu kan vi anta att du får feedback att användbarhet är dålig för hello app.</span><span class="sxs-lookup"><span data-stu-id="6c946-170">Now, let's imagine that you receive feedback that usability is poor for hello app.</span></span> <span data-ttu-id="6c946-171">Så att du bestämmer tooinvestigate.</span><span class="sxs-lookup"><span data-stu-id="6c946-171">So you decide tooinvestigate.</span></span> <span data-ttu-id="6c946-172">Du kommer tooinstrument app-toogive du feedback.</span><span class="sxs-lookup"><span data-stu-id="6c946-172">You're going tooinstrument your app toogive you feedback.</span></span>

## <a name="investigate-instrument-your-client-app-for-monitoringmetrics"></a><span data-ttu-id="6c946-173">Undersök: Instrumentera klientappen för övervakning/mått</span><span class="sxs-lookup"><span data-stu-id="6c946-173">Investigate: Instrument your client app for monitoring/metrics</span></span>
1. <span data-ttu-id="6c946-174">Öppna * &lt;repository_root >*\src\MultiChannelToDo.sln i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6c946-174">Open *&lt;repository_root>*\src\MultiChannelToDo.sln in Visual Studio.</span></span>
2. <span data-ttu-id="6c946-175">Återställa alla Nuget-paket genom att högerklicka på lösningen > **hantera NuGet-paket för lösningen** > **återställa**.</span><span class="sxs-lookup"><span data-stu-id="6c946-175">Restore all Nuget packages by right-clicking solution > **Manage NuGet Packages for Solution** > **Restore**.</span></span>
3. <span data-ttu-id="6c946-176">Högerklicka på **MultiChannelToDo.Web** > **Lägg till Application Insights Telemetry** > **konfigurera inställningarna för** > ändra resursgrupp tooToDoApp*&lt;your_suffix >* > **Lägg till Application Insights tooProject**.</span><span class="sxs-lookup"><span data-stu-id="6c946-176">Right-click **MultiChannelToDo.Web** > **Add Application Insights Telemetry** > **Configure Settings** > Change resource group tooToDoApp*&lt;your_suffix>* > **Add Application Insights tooProject**.</span></span>
4. <span data-ttu-id="6c946-177">Öppna hello bladet för hello i hello Azure-portalen, **MultiChannelToDo.Web** Application Insights-resurs.</span><span class="sxs-lookup"><span data-stu-id="6c946-177">In hello Azure Portal, open hello blade for hello **MultiChannelToDo.Web** Application Insight resource.</span></span> <span data-ttu-id="6c946-178">I hello **programhälsan** del, klickar du på **Lär dig hur toocollect webbläsare sidinläsning data** > Kopiera kod.</span><span class="sxs-lookup"><span data-stu-id="6c946-178">Then in hello **Application health** part, click **Learn how toocollect browser page load data** > copy code.</span></span>
5. <span data-ttu-id="6c946-179">Lägg till hello kopieras JS instrumentation kod för*&lt;repository_root >*\src\MultiChannelToDo.Web\app\Index.cshtml precis innan du stänger hello `<heading>` tagg.</span><span class="sxs-lookup"><span data-stu-id="6c946-179">Add hello copied JS instrumentation code too*&lt;repository_root>*\src\MultiChannelToDo.Web\app\Index.cshtml, just before hello closing `<heading>` tag.</span></span> <span data-ttu-id="6c946-180">Den ska innehålla hello unika instrumentation nyckeln för Application Insights-resurs.</span><span class="sxs-lookup"><span data-stu-id="6c946-180">It should contain hello unique instrumentation key of your Application Insight resource.</span></span>

        <script type="text/javascript">
        var appInsights=window.appInsights||function(config){
            function s(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},r=document,f=window,e="script",o=r.createElement(e),i,u;for(o.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js",r.getElementsByTagName(e)[0].parentNode.appendChild(o),t.cookie=r.cookie,t.queue=[],i=["Event","Exception","Metric","PageView","Trace"];i.length;)s("track"+i.pop());return config.disableExceptionTracking||(i="onerror",s("_"+i),u=f[i],f[i]=function(config,r,f,e,o){var s=u&&u(config,r,f,e,o);return s!==!0&&t["_"+i](config,r,f,e,o),s}),t
        }({
            instrumentationKey:"<your_unique_instrumentation_key>"
        });

        window.appInsights=appInsights;
        appInsights.trackPageView();
        </script>
6. <span data-ttu-id="6c946-181">Skicka anpassade händelser tooApplication Insights för musklickningar genom att lägga till hello följande kod toohello längst ned i brödtext:</span><span class="sxs-lookup"><span data-stu-id="6c946-181">Send custom events tooApplication Insights for mouse clicks by adding hello following code toohello bottom of body:</span></span>

       <script>
           $(document.body).find("*").click(function(event) {

               appInsights.trackEvent(event.target.tagName + ": " + event.target.className);
           });
       </script>

   <span data-ttu-id="6c946-182">JavaScript-kodfragment skickar en anpassad händelse tooApplication insikter varje gång en användare klickar någonstans i hello webbapp.</span><span class="sxs-lookup"><span data-stu-id="6c946-182">This JavaScript snippet sends a custom event tooApplication Insights every time a user clicks anywhere in hello web app.</span></span>
7. <span data-ttu-id="6c946-183">Git Shell, genomför och skicka din ändringar tooyour förgrening i GitHub.</span><span class="sxs-lookup"><span data-stu-id="6c946-183">In Git Shell, commit and push your changes tooyour fork in GitHub.</span></span> <span data-ttu-id="6c946-184">Vänta sedan klienter toorefresh webbläsare.</span><span class="sxs-lookup"><span data-stu-id="6c946-184">Then, wait for clients toorefresh browser.</span></span>

       git add -A :/
       git commit -m "add AI configuration for client app"
       git push origin master
8. <span data-ttu-id="6c946-185">Växlingen hello distribuerad app ändringar tooproduction:</span><span class="sxs-lookup"><span data-stu-id="6c946-185">Swap hello deployed app changes tooproduction:</span></span>

     <span data-ttu-id="6c946-186">. \swap – namn ToDoApp < your_suffix ></span><span class="sxs-lookup"><span data-stu-id="6c946-186">.\swap –Name ToDoApp<your_suffix></span></span>
9. <span data-ttu-id="6c946-187">Bläddra toohello Application Insights-resurs som du har konfigurerat.</span><span class="sxs-lookup"><span data-stu-id="6c946-187">Browse toohello Application Insights resource that you configured.</span></span> <span data-ttu-id="6c946-188">Klicka på anpassade händelser.</span><span class="sxs-lookup"><span data-stu-id="6c946-188">Click Custom events.</span></span>

   ![](./media/app-service-web-test-in-production-controlled-test-flight/01-custom-events.png)

   <span data-ttu-id="6c946-189">Om du inte ser mätvärden för anpassade händelser, Vänta några minuter och klicka på **uppdatera**.</span><span class="sxs-lookup"><span data-stu-id="6c946-189">If you don't see metrics for custom events, wait a few minutes and click **Refresh**.</span></span>

<span data-ttu-id="6c946-190">Anta att du ser ett diagram som nedan:</span><span class="sxs-lookup"><span data-stu-id="6c946-190">Suppose you see a chart like below:</span></span>

![](./media/app-service-web-test-in-production-controlled-test-flight/02-custom-events-chart-view.png)

<span data-ttu-id="6c946-191">Och hello händelse rutnätet nedan:</span><span class="sxs-lookup"><span data-stu-id="6c946-191">And hello event grid below it:</span></span>

![](./media/app-service-web-test-in-production-controlled-test-flight/03-custom-event-grid-view.png)

<span data-ttu-id="6c946-192">Enligt tooyour ToDoApp programkod hello **knappen** händelse motsvarar toohello Skicka-knapp och hello **indata** händelse motsvarar toohello textruta.</span><span class="sxs-lookup"><span data-stu-id="6c946-192">According tooyour ToDoApp application code, hello **BUTTON** event corresponds toohello submit button, and hello **INPUT** event corresponds toohello textbox.</span></span> <span data-ttu-id="6c946-193">Hittills vara saker meningsfullt.</span><span class="sxs-lookup"><span data-stu-id="6c946-193">So far, things make sense.</span></span> <span data-ttu-id="6c946-194">Men det verkar som om det är mycket musklickningar och mycket få klick på hello-arbetsuppgifter (hello **LI** händelser).</span><span class="sxs-lookup"><span data-stu-id="6c946-194">However, it looks like there's a good amount of clicks and very few clicks on hello to-do items (hello **LI** events).</span></span>

<span data-ttu-id="6c946-195">Baserat på, du formuläret den hypotesen några användare kan blandas ihop vilken del av hello Användargränssnittet är klickbara och eftersom hello markören är formaterad för textmarkering när den är placerad på hello objekt och deras ikoner.</span><span class="sxs-lookup"><span data-stu-id="6c946-195">Based on this, you form your hypothesis that some users are confused which part of hello UI is clickable and it is because hello cursor is styled for text selection when it hovers on hello list items and their icons.</span></span>

![](./media/app-service-web-test-in-production-controlled-test-flight/04-to-do-list-item-ui.png)

<span data-ttu-id="6c946-196">Det här är ett exempel på contrived.</span><span class="sxs-lookup"><span data-stu-id="6c946-196">This might be a contrived example.</span></span> <span data-ttu-id="6c946-197">Dock du är en förbättring tooyour app för pågående toomake och utför sedan en flighting distribution tooget användbarhet feedback från live-kunder.</span><span class="sxs-lookup"><span data-stu-id="6c946-197">Nevertheless, you're going toomake an improvement tooyour app, and then perform a flighting deployment tooget usability feedback from live customers.</span></span>

### <a name="instrument-your-server-app-for-monitoringmetrics"></a><span data-ttu-id="6c946-198">Instrumentera din server-app för övervakning/mått</span><span class="sxs-lookup"><span data-stu-id="6c946-198">Instrument your server app for monitoring/metrics</span></span>
<span data-ttu-id="6c946-199">Detta är en tangens eftersom hello-scenariot som visas i den här självstudiekursen bara behandlar hello-klientappen.</span><span class="sxs-lookup"><span data-stu-id="6c946-199">This is a tangent since hello scenario demonstrated in this tutorial only deals with hello client app.</span></span> <span data-ttu-id="6c946-200">För fullständighetens skull kommer du dock ställa in hello serversidan app.</span><span class="sxs-lookup"><span data-stu-id="6c946-200">However, for completeness you will set up hello server-side app.</span></span>

1. <span data-ttu-id="6c946-201">Högerklicka på **MultiChannelToDo** > **Lägg till Application Insights Telemetry** > **konfigurera inställningarna för** > ändra resursgrupp tooToDoApp*&lt;your_suffix >* > **Lägg till Application Insights tooProject**.</span><span class="sxs-lookup"><span data-stu-id="6c946-201">Right-click **MultiChannelToDo** > **Add Application Insights Telemetry** > **Configure Settings** > Change resource group tooToDoApp*&lt;your_suffix>* > **Add Application Insights tooProject**.</span></span>
2. <span data-ttu-id="6c946-202">Git Shell, genomför och skicka din ändringar tooyour förgrening i GitHub.</span><span class="sxs-lookup"><span data-stu-id="6c946-202">In Git Shell, commit and push your changes tooyour fork in GitHub.</span></span> <span data-ttu-id="6c946-203">Vänta sedan klienter toorefresh webbläsare.</span><span class="sxs-lookup"><span data-stu-id="6c946-203">Then, wait for clients toorefresh browser.</span></span>

       git add -A :/
       git commit -m "add AI configuration for server app"
       git push origin master
3. <span data-ttu-id="6c946-204">Växlingen hello distribuerad app ändringar tooproduction:</span><span class="sxs-lookup"><span data-stu-id="6c946-204">Swap hello deployed app changes tooproduction:</span></span>

     <span data-ttu-id="6c946-205">. \swap – namn ToDoApp < your_suffix ></span><span class="sxs-lookup"><span data-stu-id="6c946-205">.\swap –Name ToDoApp<your_suffix></span></span>

<span data-ttu-id="6c946-206">Klart!</span><span class="sxs-lookup"><span data-stu-id="6c946-206">That's it!</span></span>

## <a name="investigate-add-slot-specific-tags-tooyour-client-app-metrics"></a><span data-ttu-id="6c946-207">Undersök: Lägg till plats-specifika taggar tooyour klienten app mått</span><span class="sxs-lookup"><span data-stu-id="6c946-207">Investigate: Add slot-specific tags tooyour client app metrics</span></span>
<span data-ttu-id="6c946-208">I det här avsnittet konfigurerar du hello annan distribution fack toosend fack-specifika telemetri toohello samma Application Insights-resurs.</span><span class="sxs-lookup"><span data-stu-id="6c946-208">In this section, you will configure hello different deployment slots toosend slot-specific telemetry toohello same Application Insights resource.</span></span> <span data-ttu-id="6c946-209">På så sätt kan du jämföra telemetri data mellan trafik från olika platser (distributionsmiljöer) tooeasily Se hello din app ändringarna.</span><span class="sxs-lookup"><span data-stu-id="6c946-209">This way, you can compare telemetry data between traffic from different slots (deployment environments) tooeasily see hello effect of your app changes.</span></span> <span data-ttu-id="6c946-210">AT hello samma tid och du kan åtskilja hello produktion trafik från hello rest så att du kan fortsätta toomonitor produktion appen efter behov.</span><span class="sxs-lookup"><span data-stu-id="6c946-210">At hello same time, you can separate hello production traffic from hello rest so you can continue toomonitor your production app as needed.</span></span>

<span data-ttu-id="6c946-211">Eftersom du samla in data på klientbeteende, kommer du att [lägga till en telemetri initieraren tooyour JavaScript-kod](../application-insights/app-insights-api-filtering-sampling.md) i index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="6c946-211">Since you're gathering data on client behavior, you will [add a telemetry initializer tooyour JavaScript code](../application-insights/app-insights-api-filtering-sampling.md) in index.cshtml.</span></span> <span data-ttu-id="6c946-212">Om du vill tootest serversidan prestanda, t.ex, du kan också göra på samma sätt i serverkoden (se [Application Insights API för anpassade händelser och mått](../application-insights/app-insights-api-custom-events-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="6c946-212">If you want tootest server-side performance, for example, you can also do similarly in your server code (see [Application Insights API for custom events and metrics](../application-insights/app-insights-api-custom-events-metrics.md).</span></span>

1. <span data-ttu-id="6c946-213">Lägg först till hello kod mellan hello två `//` kommentarer nedan i hello JavaScript-block som du lagt till toohello `<heading>` tagga tidigare.</span><span class="sxs-lookup"><span data-stu-id="6c946-213">First, add hello code bewteen hello two `//` comments below in hello JavaScript block that you added toohello `<heading>` tag earlier.</span></span>

        window.appInsights = appInsights;

        // Begin new code
        appInsights.queue.push(function () {
            appInsights.context.addTelemetryInitializer(function (envelope) {
                var telemetryItem = envelope.data.baseData;
                telemetryItem.properties = telemetryItem.properties || {};
                telemetryItem.properties["Environment"] = "@System.Configuration.ConfigurationManager.AppSettings["environment"]";
            });
        });
        // End new code

        appInsights.trackPageView();

    <span data-ttu-id="6c946-214">Initieraren koden gör hello `appInsights` objekt tooadd hello en anpassad egenskap som kallas `Environment` tooevery typ av telemetri som skickas.</span><span class="sxs-lookup"><span data-stu-id="6c946-214">This initializer code causes hello `appInsights` object tooadd hello a custom property called `Environment` tooevery piece of telemetry it sends.</span></span>
2. <span data-ttu-id="6c946-215">Lägg till den anpassade egenskapen som en [fack inställningen](web-sites-staged-publishing.md#AboutConfiguration) för webbappen i Azure.</span><span class="sxs-lookup"><span data-stu-id="6c946-215">Next, add this custom property as a [slot setting](web-sites-staged-publishing.md#AboutConfiguration) for your web app in Azure.</span></span> <span data-ttu-id="6c946-216">toodo detta, kör hello följande kommandon i sessionen Git-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="6c946-216">toodo this, run hello following commands in your Git Shell session.</span></span>

        $app = Get-AzureWebsite -Name todoapp<your_suffix> -Slot production
        $app.AppSettings.Add("environment", "Production")
        $app.SlotStickyAppSettingNames.Add("environment")
        $app | Set-AzureWebsite -Name todoapp<your_suffix> -Slot production

    <span data-ttu-id="6c946-217">hello Web.config i projektet redan definierar hello `environment` appinställningen.</span><span class="sxs-lookup"><span data-stu-id="6c946-217">hello Web.config in your project already defines hello `environment` app setting.</span></span> <span data-ttu-id="6c946-218">Med den här inställningen när du testar hello appen lokalt, dina märks med `VS Debugger`.</span><span class="sxs-lookup"><span data-stu-id="6c946-218">With this setting, when you test hello app locally, your metrics will be tagged with `VS Debugger`.</span></span> <span data-ttu-id="6c946-219">Men när du överför dina ändringar tooAzure Azure ska hitta och använda hello `environment` app inställningen i konfigurationen för hello webbapp i stället och dina märkta med `Production`.</span><span class="sxs-lookup"><span data-stu-id="6c946-219">However, when you push your changes tooAzure, Azure will find and use hello `environment` app setting in hello web app's configuration instead, and your metrics will be tagged with `Production`.</span></span>
3. <span data-ttu-id="6c946-220">Genomför push din kod ändringar tooyour förgrening på GitHub och väntar sedan på användare toouse hello nya appen (behovet toorefresh hello webbläsare).</span><span class="sxs-lookup"><span data-stu-id="6c946-220">Commit and push your code changes tooyour fork on GitHub, and then wait for your users toouse hello new app (need toorefresh hello browser).</span></span> <span data-ttu-id="6c946-221">Det tar ungefär 15 minuter för hello nya egenskapen tooshow i Application Insights `MultiChannelToDo.Web` resurs.</span><span class="sxs-lookup"><span data-stu-id="6c946-221">It takes about 15 minutes for hello new property tooshow up in your Application Insights `MultiChannelToDo.Web` resource.</span></span>

        git add -A :/
        git commit -m "add environment property tooAI events for client app"
        git push origin master
4. <span data-ttu-id="6c946-222">Gå toohello **anpassad händelser** bladet igen och filtrera hello mått på `Environment=Production`.</span><span class="sxs-lookup"><span data-stu-id="6c946-222">Now, go toohello **Custom events** blade again and filter hello metrics on `Environment=Production`.</span></span> <span data-ttu-id="6c946-223">Nu bör du kunna toosee alla hello nya anpassade händelser i hello produktion fack med det här filtret.</span><span class="sxs-lookup"><span data-stu-id="6c946-223">You should now be able toosee all hello new custom events in hello production slot with this filter.</span></span>

    ![](./media/app-service-web-test-in-production-controlled-test-flight/05-filter-on-production-environment.png)
5. <span data-ttu-id="6c946-224">Klicka på hello **Favoriter** knappen toosave hello aktuella Metrics Explorer-inställningar toosomething som **anpassad händelser: produktion**.</span><span class="sxs-lookup"><span data-stu-id="6c946-224">Click hello **Favorites** button toosave hello current Metrics Explorer settings toosomething like **Custom events: Production**.</span></span> <span data-ttu-id="6c946-225">Du kan enkelt växla mellan den här vyn och vyn distribution fack senare.</span><span class="sxs-lookup"><span data-stu-id="6c946-225">You can easily switch between this view and a deployment slot view later.</span></span>

   > [!TIP]
   > <span data-ttu-id="6c946-226">För ännu mer kraftfulla analytics, Överväg [integrera Application Insights-resursen med Power BI](../application-insights/app-insights-export-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="6c946-226">For even more powerful analytics, consider [integrating your Application Insights resource with Power BI](../application-insights/app-insights-export-power-bi.md).</span></span>
   >
   >

### <a name="add-slot-specific-tags-tooyour-server-app-metrics"></a><span data-ttu-id="6c946-227">Lägg till plats-specifika taggar tooyour serverstatistik app</span><span class="sxs-lookup"><span data-stu-id="6c946-227">Add slot-specific tags tooyour server app metrics</span></span>
<span data-ttu-id="6c946-228">Igen, ska du ställa in hello serversidan app för fullständighetens skull.</span><span class="sxs-lookup"><span data-stu-id="6c946-228">Again, for completeness you will set up hello server-side app.</span></span> <span data-ttu-id="6c946-229">Till skillnad från hello-klientappen som instrumenterats i JavaScript, instrumenterats fack-specifika taggar för hello server app med .NET-kod.</span><span class="sxs-lookup"><span data-stu-id="6c946-229">Unlike hello client app which is instrumented in JavaScript, slot-specific tags for hello server app is instrumented with .NET code.</span></span>

1. <span data-ttu-id="6c946-230">Öppna * &lt;repository_root >*\src\MultiChannelToDo\Global.asax.cs.</span><span class="sxs-lookup"><span data-stu-id="6c946-230">Open *&lt;repository_root>*\src\MultiChannelToDo\Global.asax.cs.</span></span> <span data-ttu-id="6c946-231">Lägg till hello kodblock nedan, precis före hello avslutande klammerparentes för namnområdet.</span><span class="sxs-lookup"><span data-stu-id="6c946-231">Add hello code block below, just before hello closing namespace curly brace.</span></span>

        namespace MultiChannelToDo
        {
                ...

                // Begin new code
            public class ConfigInitializer
            : ITelemetryInitializer
            {
                void ITelemetryInitializer.Initialize(ITelemetry telemetry)
                {
                    telemetry.Context.Properties["Environment"] = System.Configuration.ConfigurationManager.AppSettings["environment"];
                }
            }
                // End new code
        }
2. <span data-ttu-id="6c946-232">Korrigera hello name resolution fel genom att lägga till hello `using` instruktionerna nedan toohello början av hello-fil:</span><span class="sxs-lookup"><span data-stu-id="6c946-232">Correct hello name resolution errors by adding hello `using` statements below toohello beginning of hello file:</span></span>

        using Microsoft.ApplicationInsights.Channel;
        using Microsoft.ApplicationInsights.Extensibility;
3. <span data-ttu-id="6c946-233">Lägg till hello kod nedan toohello början av hello `Application_Start()` metoden:</span><span class="sxs-lookup"><span data-stu-id="6c946-233">Add hello code below toohello beginning of hello `Application_Start()` method:</span></span>

        TelemetryConfiguration.Active.TelemetryInitializers.Add(new ConfigInitializer());
4. <span data-ttu-id="6c946-234">Genomför push din kod ändringar tooyour förgrening på GitHub och väntar sedan på användare toouse hello nya appen (behovet toorefresh hello webbläsare).</span><span class="sxs-lookup"><span data-stu-id="6c946-234">Commit and push your code changes tooyour fork on GitHub, and then wait for your users toouse hello new app (need toorefresh hello browser).</span></span> <span data-ttu-id="6c946-235">Det tar ungefär 15 minuter för hello nya egenskapen tooshow i Application Insights `MultiChannelToDo` resurs.</span><span class="sxs-lookup"><span data-stu-id="6c946-235">It takes about 15 minutes for hello new property tooshow up in your Application Insights `MultiChannelToDo` resource.</span></span>

        git add -A :/
        git commit -m "add environment property tooAI events for server app"
        git push origin master

## <a name="update-set-up-your-beta-branch"></a><span data-ttu-id="6c946-236">Uppdatering: Ställ in din gren beta</span><span class="sxs-lookup"><span data-stu-id="6c946-236">Update: Set up your beta branch</span></span>
1. <span data-ttu-id="6c946-237">Öppna * &lt;repository_root >*\ARMTemplates\ProdAndStagetest.json och hitta hello `appsettings` resurser (söka efter `"name": "appsettings"`).</span><span class="sxs-lookup"><span data-stu-id="6c946-237">Open *&lt;repository_root>*\ARMTemplates\ProdAndStagetest.json and find hello `appsettings` resources (search for `"name": "appsettings"`).</span></span> <span data-ttu-id="6c946-238">Det finns 4 i dem, ett för varje plats.</span><span class="sxs-lookup"><span data-stu-id="6c946-238">There are 4 of them, one for each slot.</span></span>
2. <span data-ttu-id="6c946-239">För varje `appsettings` resurs, lägga till en `"environment": "[parameters('slotName')]"` app inställningen toohello slutet av hello `properties` matris.</span><span class="sxs-lookup"><span data-stu-id="6c946-239">For each `appsettings` resource, add an  `"environment": "[parameters('slotName')]"` app setting toohello end of hello `properties` array.</span></span> <span data-ttu-id="6c946-240">Glöm inte tooend hello föregående rad med kommatecken.</span><span class="sxs-lookup"><span data-stu-id="6c946-240">Don't forget tooend hello previous line with a comma.</span></span>

    ![](./media/app-service-web-test-in-production-controlled-test-flight/06-arm-app-setting-with-slot-name.png)

    <span data-ttu-id="6c946-241">Du har lagt till hello `environment` app inställningen tooall hello fack i hello mallen.</span><span class="sxs-lookup"><span data-stu-id="6c946-241">You have just added hello `environment` app setting tooall hello slots in hello template.</span></span>
3. <span data-ttu-id="6c946-242">I Hej samma fil, hitta hello `slotconfignames` resurser (söka efter `"name": "slotconfignames"`).</span><span class="sxs-lookup"><span data-stu-id="6c946-242">In hello same file, find hello `slotconfignames` resources (search for `"name": "slotconfignames"`).</span></span> <span data-ttu-id="6c946-243">Det finns 2 av dem, ett för varje app.</span><span class="sxs-lookup"><span data-stu-id="6c946-243">There are 2 of them, one for each app.</span></span>
4. <span data-ttu-id="6c946-244">För varje `slotconfignames` resurs, lägga till `"environment"` toohello slutet av hello `appSettingNames` matris.</span><span class="sxs-lookup"><span data-stu-id="6c946-244">For each `slotconfignames` resource, add `"environment"` toohello end of hello `appSettingNames` array.</span></span> <span data-ttu-id="6c946-245">Glöm inte tooend hello föregående rad med kommatecken.</span><span class="sxs-lookup"><span data-stu-id="6c946-245">Don't forget tooend hello previous line with a comma.</span></span>

    <span data-ttu-id="6c946-246">Du har skapat hello `environment` app inställningen minne tooits respektive distributionsplatsen för både appar.</span><span class="sxs-lookup"><span data-stu-id="6c946-246">You have just made hello `environment` app setting stick tooits respective deployment slot for both apps.</span></span>  
5. <span data-ttu-id="6c946-247">I sessionen Git Shell hello kör följande kommandon med hello samma resurs grupp-suffix som du använde tidigare.</span><span class="sxs-lookup"><span data-stu-id="6c946-247">In your Git Shell session, run hello following commands with hello same resource group suffix that you used before.</span></span>

        git checkout -b beta
        git push origin beta
        .\deploy.ps1 -RepoUrl https://github.com/<your_fork>/ToDoApp.git -ResourceGroupSuffix <your_suffix> -SlotName beta -Branch beta
6. <span data-ttu-id="6c946-248">När du uppmanas ange hello autentiseringsuppgifterna för samma SQL-databas som tidigare.</span><span class="sxs-lookup"><span data-stu-id="6c946-248">When prompted, specify hello same SQL database credentials as before.</span></span> <span data-ttu-id="6c946-249">Sedan, när du tillfrågas tooupdate hello resursgrupp, typen `Y`, sedan `ENTER`.</span><span class="sxs-lookup"><span data-stu-id="6c946-249">Then, when asked tooupdate hello resource group, type `Y`, then `ENTER`.</span></span>

    <span data-ttu-id="6c946-250">När hello skriptet har körts, alla resurser i hello ursprungliga resursgruppen finns kvar, men en ny plats med namnet ”betaversionen” skapas i den med hello samma konfiguration som hello ”mellanlagring” fack som skapades i hello början.</span><span class="sxs-lookup"><span data-stu-id="6c946-250">Once hello script finishes, all your resources in hello original resource group are retained, but a new slot named "beta" is created in it with hello same configuration as hello "Staging" slot that was created in hello beginning.</span></span>

   > [!NOTE]
   > <span data-ttu-id="6c946-251">Den här metoden för att skapa av olika distributionsmiljöer skiljer sig från hello-metoden i [flexibel programvaruutveckling med Azure App Service](app-service-agile-software-development.md).</span><span class="sxs-lookup"><span data-stu-id="6c946-251">This method of creating different deployment environments is different from hello method in [Agile software development with Azure App Service](app-service-agile-software-development.md).</span></span> <span data-ttu-id="6c946-252">Här kan skapa du distribution av med distributionsplatser där som det du skapar distributionsmiljöer med resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="6c946-252">Here, you create deployment environments with deployment slots, where as there you create deployment environments with resource groups.</span></span> <span data-ttu-id="6c946-253">Hantera distribution av med resursgrupper kan du tookeep hello produktion miljö off-limits toodevelopers, men det är inte enkelt toodo test i produktion, vilket du kan göra enkelt med platser.</span><span class="sxs-lookup"><span data-stu-id="6c946-253">Managing deployment environments with resource groups enables you tookeep hello production environment off-limits toodevelopers, but it's not easy toodo testing in production, which you can do easily with slots.</span></span>
   >
   >

<span data-ttu-id="6c946-254">Om du vill kan skapa du en alfanumeriska app genom att köra</span><span class="sxs-lookup"><span data-stu-id="6c946-254">If you wish, you can also create an alpha app by running</span></span>

    git checkout -b alpha
    git push origin alpha
    .\deploy.ps1 -RepoUrl https://github.com/<your_fork>/ToDoApp.git -ResourceGroupSuffix <your_suffix> -SlotName beta -Branch alpha

<span data-ttu-id="6c946-255">För den här självstudiekursen kommer du bara fortsätta använda appen beta.</span><span class="sxs-lookup"><span data-stu-id="6c946-255">For this tutorial, you will just keep using your beta app.</span></span>

## <a name="update-push-your-updates-toohello-beta-app"></a><span data-ttu-id="6c946-256">Uppdatering: Push appen uppdateringar toohello beta</span><span class="sxs-lookup"><span data-stu-id="6c946-256">Update: Push your updates toohello beta app</span></span>
<span data-ttu-id="6c946-257">Den bakre tooyour app som du vill tooimprove.</span><span class="sxs-lookup"><span data-stu-id="6c946-257">Back tooyour app that you want tooimprove.</span></span>

1. <span data-ttu-id="6c946-258">Kontrollera att du är nu i grenen beta</span><span class="sxs-lookup"><span data-stu-id="6c946-258">Make sure you're now in your beta branch</span></span>

        git checkout beta
2. <span data-ttu-id="6c946-259">I * &lt;repository_root >*\src\MultiChannelToDo.Web\app\Index.cshtml, hitta hello `<li>` tagga och Lägg till hello `style="cursor:pointer"` attributet som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="6c946-259">In *&lt;repository_root>*\src\MultiChannelToDo.Web\app\Index.cshtml, find hello `<li>` tag and add hello `style="cursor:pointer"` attribute, as shown below.</span></span>

    ![](./media/app-service-web-test-in-production-controlled-test-flight/07-change-cursor-style-on-li.png)
3. <span data-ttu-id="6c946-260">genomför och skicka tooAzure.</span><span class="sxs-lookup"><span data-stu-id="6c946-260">commit and push tooAzure.</span></span>
4. <span data-ttu-id="6c946-261">Kontrollera att hello nu ändringen i hello beta plats genom att gå toohttp://todoapp*&lt;your_suffix >*-beta.azurewebsites.net/.</span><span class="sxs-lookup"><span data-stu-id="6c946-261">Verify that hello change is now reflected in hello beta slot by navigating toohttp://todoapp*&lt;your_suffix>*-beta.azurewebsites.net/.</span></span> <span data-ttu-id="6c946-262">Om du inte ser hello ändra ännu, uppdatera din webbläsare tooget hello nya javascript-kod.</span><span class="sxs-lookup"><span data-stu-id="6c946-262">If you don't see hello change yet, refresh your browser tooget hello new javascript code.</span></span>

    ![](./media/app-service-web-test-in-production-controlled-test-flight/08-verify-change-in-beta-site.png)

<span data-ttu-id="6c946-263">Nu när du har din ändring som körs i hello beta fack är klar tooperform flighting distribution.</span><span class="sxs-lookup"><span data-stu-id="6c946-263">Now that you have your change running in hello beta slot, you are ready tooperform a flighting deployment.</span></span>

## <a name="validate-route-traffic-toohello-beta-app"></a><span data-ttu-id="6c946-264">Verifiera: Väg trafik toohello beta app</span><span class="sxs-lookup"><span data-stu-id="6c946-264">Validate: Route traffic toohello beta app</span></span>
<span data-ttu-id="6c946-265">I det här avsnittet dirigerar trafik toohello beta app.</span><span class="sxs-lookup"><span data-stu-id="6c946-265">In this section, you will route traffic toohello beta app.</span></span> <span data-ttu-id="6c946-266">For sake of bli tydligare i demonstrationssyfte ska tooroute en betydande del av hello användaren trafik tooit.</span><span class="sxs-lookup"><span data-stu-id="6c946-266">For sake of clarity of demonstration, you're going tooroute a significant portion of hello user traffic tooit.</span></span> <span data-ttu-id="6c946-267">I själva verket hello mängden trafik som du vill tooroute beror på dina behov.</span><span class="sxs-lookup"><span data-stu-id="6c946-267">In reality, hello amount of traffic you want tooroute will depend on your specific situation.</span></span> <span data-ttu-id="6c946-268">Till exempel om platsen är i hello skala Microsoft.com, kan måste mindre än en procent av den totala trafiken i ordning toogain användbara data.</span><span class="sxs-lookup"><span data-stu-id="6c946-268">For example, if your site is at hello scale of microsoft.com, then you may need less than one percent of your total traffic in order toogain useful data.</span></span>

1. <span data-ttu-id="6c946-269">I Git Shell sessionen och kör följande kommandon tooroute hello hälften av hello trafik toohello beta produktionsplatsen:</span><span class="sxs-lookup"><span data-stu-id="6c946-269">In your Git Shell session, run hello following commands tooroute half of hello production traffic toohello beta slot:</span></span>

        $siteName = "ToDoApp<your suffix>"
        $rule = New-Object Microsoft.WindowsAzure.Commands.Utilities.Websites.Services.WebEntities.RampUpRule
        $rule.ActionHostName = "$siteName-beta.azurewebsites.net"
        $rule.ReroutePercentage = 50
        $rule.Name = "beta"
        Set-AzureWebsite $siteName -Slot Production -RoutingRules $rule

   <span data-ttu-id="6c946-270">Hej `ReroutePercentage=50` egenskapen anger 50% av hello produktion trafik kommer att dirigeras toohello beta app-URL (anges av hello `ActionHostName` egenskap).</span><span class="sxs-lookup"><span data-stu-id="6c946-270">hello `ReroutePercentage=50` property specifies that 50% of hello production traffic will be routed toohello beta app's URL (specified by hello `ActionHostName` property).</span></span>
2. <span data-ttu-id="6c946-271">Navigera toohttp://ToDoApp*&lt;your_suffix >*. azurewebsites.net.</span><span class="sxs-lookup"><span data-stu-id="6c946-271">Now navigate toohttp://ToDoApp*&lt;your_suffix>*.azurewebsites.net.</span></span> <span data-ttu-id="6c946-272">50% av hello trafik bör nu vara omdirigerade toohello beta fack.</span><span class="sxs-lookup"><span data-stu-id="6c946-272">50% of hello traffic should now be redirected toohello beta slot.</span></span>
3. <span data-ttu-id="6c946-273">Filtrera hello mått i Application Insights-resurs-miljön = ”betaversionen”.</span><span class="sxs-lookup"><span data-stu-id="6c946-273">In your Application Insights resource, filter hello metrics by environment="beta".</span></span>

   > [!NOTE]
   > <span data-ttu-id="6c946-274">Om du sparar den filtrerade vyn som en annan favorit, kan du enkelt växla hello mått explorer vyer mellan produktion och beta vyer.</span><span class="sxs-lookup"><span data-stu-id="6c946-274">If you save this filtered view as another favorite, then you can easily flip hello metric explorer views between production and beta views.</span></span>
   >
   >

<span data-ttu-id="6c946-275">Anta att i Application Insights du se något liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="6c946-275">Suppose in Application Insights you see something similar toohello following:</span></span>

![](./media/app-service-web-test-in-production-controlled-test-flight/09-test-beta-site-in-production.png)

<span data-ttu-id="6c946-276">Inte bara det visar att det finns många fler klick på hello `<li>` taggar, men det verkar toobe en ökning i klick på `<li>` taggar.</span><span class="sxs-lookup"><span data-stu-id="6c946-276">Not only is this showing that there are many more clicks on hello `<li>` tags, but there seems toobe a surge of clicks on `<li>` tags.</span></span> <span data-ttu-id="6c946-277">Du kan sedan avslutar att personer som har identifierats hello nya `<li>` taggar är klickbara och nu Rensa alla sina tidigare slutförts aktiviteter i hello app.</span><span class="sxs-lookup"><span data-stu-id="6c946-277">You can then conclude that people have discovered hello new `<li>` tags are clickable and are now clearing all their previously-completed tasks in hello app.</span></span>

<span data-ttu-id="6c946-278">Baserat på hello data flighting distributionen av du bestämmer dig för att ditt nya Användargränssnittet är klar för produktion.</span><span class="sxs-lookup"><span data-stu-id="6c946-278">Based on hello data of your flighting deployment, you decide that your new UI is ready for production.</span></span>

## <a name="go-live-move-your-new-code-into-production"></a><span data-ttu-id="6c946-279">Publicera: Flytta din nya kod till produktion</span><span class="sxs-lookup"><span data-stu-id="6c946-279">Go live: Move your new code into production</span></span>
<span data-ttu-id="6c946-280">Du är nu redo toomove update-tooproduction.</span><span class="sxs-lookup"><span data-stu-id="6c946-280">You're now ready toomove your update tooproduction.</span></span> <span data-ttu-id="6c946-281">Vad är bra är att nu du vet att uppdateringen har redan verifierats *innan* pushas tooproduction.</span><span class="sxs-lookup"><span data-stu-id="6c946-281">What's great is that now you know that your update has already been validated *before* it is pushed tooproduction.</span></span> <span data-ttu-id="6c946-282">Nu kan du vill distribuera den.</span><span class="sxs-lookup"><span data-stu-id="6c946-282">So now you can confidently deploy it.</span></span> <span data-ttu-id="6c946-283">Eftersom du har gjort en uppdatering toohello AngularJS-klientappen verifieras endast hello klientkod.</span><span class="sxs-lookup"><span data-stu-id="6c946-283">Since you made an update toohello AngularJS client app, you only validated hello client-side code.</span></span> <span data-ttu-id="6c946-284">Om du toomake ändringar toohello backend-webb-API-app, kan du validera dina ändringar på samma sätt och enkelt.</span><span class="sxs-lookup"><span data-stu-id="6c946-284">If you were toomake changes toohello back-end Web API app, you could validate your changes similarly and easily.</span></span>

1. <span data-ttu-id="6c946-285">Git Shell, tar du bort hello trafik routningsregel genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="6c946-285">In Git Shell, remove hello traffic routing rule by running hello following command:</span></span>

        Set-AzureWebsite $siteName -Slot Production -RoutingRules @()
2. <span data-ttu-id="6c946-286">Kör hello Git-kommandon:</span><span class="sxs-lookup"><span data-stu-id="6c946-286">Run hello Git commands:</span></span>

        git checkout master
        git pull origin master
        git merge beta
        git push origin master
3. <span data-ttu-id="6c946-287">Vänta i några minuter för hello nya code toobe distribueras toohello mellanlagringsplatsen och sedan starta http://ToDoApp*&lt;your_suffix >*-staging.azurewebsites.net tooverify som hello ny uppdatering varmkörts i hello mellanlagring fack.</span><span class="sxs-lookup"><span data-stu-id="6c946-287">Wait for a few minutes for hello new code toobe deployed toohello staging slot, then launch http://ToDoApp*&lt;your_suffix>*-staging.azurewebsites.net tooverify that hello new update is warmed up in hello staging slot.</span></span> <span data-ttu-id="6c946-288">Kom ihåg att hello din förgrening mastergrenen är länkad toohello mellanlagring plats för din app.</span><span class="sxs-lookup"><span data-stu-id="6c946-288">Remember that hello your fork's master branch is linked toohello staging slot of your app.</span></span>
4. <span data-ttu-id="6c946-289">Nu kan växla hello mellanlagringsplatsen till produktion</span><span class="sxs-lookup"><span data-stu-id="6c946-289">Now, swap hello staging slot into production</span></span>

        cd <ROOT>\ToDoApp\ARMTemplates
        .\swap.ps1 -Name todoapp<your_suffix>

## <a name="summary"></a><span data-ttu-id="6c946-290">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="6c946-290">Summary</span></span>
<span data-ttu-id="6c946-291">Azure App Service gör det enkelt för toomedium-små företag tootest sina kund-riktade appar i produktion, något som traditionellt har gjorts i stora företag.</span><span class="sxs-lookup"><span data-stu-id="6c946-291">Azure App Service makes it easy for small- toomedium-sized businesses tootest their customer-facing apps in production, something that has been traditionally done in big enterprises.</span></span> <span data-ttu-id="6c946-292">Förhoppningsvis den här kursen har du fått hello kunskap du behöver toobring tillsammans App Service och Application Insights toomake möjlig förhandsversionstestning distribution och även andra test i produktion-scenarier i DevOps-världen.</span><span class="sxs-lookup"><span data-stu-id="6c946-292">Hopefully, this tutorial has given you hello knowledge you need toobring together App Service and Application Insights toomake possible flighting deployment, and even other test-in-production scenarios, in your DevOps world.</span></span>

## <a name="more-resources"></a><span data-ttu-id="6c946-293">Fler resurser</span><span class="sxs-lookup"><span data-stu-id="6c946-293">More resources</span></span>
* [<span data-ttu-id="6c946-294">Flexibel programvaruutveckling med Azure App Service</span><span class="sxs-lookup"><span data-stu-id="6c946-294">Agile software development with Azure App Service</span></span>](app-service-agile-software-development.md)
* [<span data-ttu-id="6c946-295">Skapa mellanlagringsmiljöer för web apps i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="6c946-295">Set up staging environments for web apps in Azure App Service</span></span>](web-sites-staged-publishing.md)
* [<span data-ttu-id="6c946-296">Distribuera ett komplexa program förutsägbart i Azure</span><span class="sxs-lookup"><span data-stu-id="6c946-296">Deploy a complex application predictably in Azure</span></span>](app-service-deploy-complex-application-predictably.md)
* [<span data-ttu-id="6c946-297">Redigera Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="6c946-297">Authoring Azure Resource Manager Templates</span></span>](../azure-resource-manager/resource-group-authoring-templates.md)
* [<span data-ttu-id="6c946-298">JSONLint - hello JSON-verifieraren</span><span class="sxs-lookup"><span data-stu-id="6c946-298">JSONLint - hello JSON Validator</span></span>](http://jsonlint.com/)
* [<span data-ttu-id="6c946-299">Git förgrening – grundläggande förgrening och sammanfogning</span><span class="sxs-lookup"><span data-stu-id="6c946-299">Git Branching – Basic Branching and Merging</span></span>](http://www.git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
* [<span data-ttu-id="6c946-300">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="6c946-300">Azure PowerShell</span></span>](/powershell/azure/overview)
* [<span data-ttu-id="6c946-301">Projektet Kudu-Wiki</span><span class="sxs-lookup"><span data-stu-id="6c946-301">Project Kudu Wiki</span></span>](https://github.com/projectkudu/kudu/wiki)
