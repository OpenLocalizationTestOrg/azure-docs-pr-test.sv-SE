---
title: "Förhandsversionstestning distribution (betatestning) i Azure App Service"
description: "Lär dig flight nya funktioner i din app eller beta testa dina uppdateringar i den här självstudiekursen för slutpunkt till slutpunkt. Den samlar Apptjänst funktioner som kontinuerlig publicering, platser, routning av nätverkstrafik och Application Insights-integration."
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
ms.openlocfilehash: 83e3247310461ac148fff3c4ade3aa7216478537
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="flighting-deployment-beta-testing-in-azure-app-service"></a><span data-ttu-id="40839-104">Förhandsversionstestning distribution (betatestning) i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="40839-104">Flighting deployment (beta testing) in Azure App Service</span></span>
<span data-ttu-id="40839-105">Den här kursen visar hur du gör *förhandsversionstestning distributioner* genom att integrera olika funktioner i [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) och [Azure Application Insights](/services/application-insights/).</span><span class="sxs-lookup"><span data-stu-id="40839-105">This tutorial shows you how to do *flighting deployments* by integrating the various capabilities of [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) and [Azure Application Insights](/services/application-insights/).</span></span>

<span data-ttu-id="40839-106">*Förhandsversionstestning* är en Distributionsprocess som validerar en ny funktion eller ändra med ett begränsat antal verkliga kunder och är en större testning i form av produktionsscenario.</span><span class="sxs-lookup"><span data-stu-id="40839-106">*Flighting* is a deployment process that validates a new feature or change with a limited number of real customers, and is a major testing in production scenario.</span></span> <span data-ttu-id="40839-107">Det är som beta testning och kallas ibland ”kontrollerade test svarta”.</span><span class="sxs-lookup"><span data-stu-id="40839-107">It is akin to beta testing and is sometimes known as "controlled test flight".</span></span> <span data-ttu-id="40839-108">Många stora företag med webben använda den här metoden för att få tidig validering på deras app-uppdateringar i sina praxis [flexibel utveckling](https://en.wikipedia.org/wiki/Agile_software_development).</span><span class="sxs-lookup"><span data-stu-id="40839-108">Many large enterprises with a web presence use this approach to get early validation on their app updates in their practice of [agile development](https://en.wikipedia.org/wiki/Agile_software_development).</span></span> <span data-ttu-id="40839-109">Azure Apptjänst kan du integrera test i produktion med kontinuerlig publicering och Application Insights för att genomföra samma DevOps-scenario.</span><span class="sxs-lookup"><span data-stu-id="40839-109">Azure App Service enables you to integrate test in production with continous publishing and Application Insights to implement the same DevOps scenario.</span></span> <span data-ttu-id="40839-110">Den här metoden fördelar:</span><span class="sxs-lookup"><span data-stu-id="40839-110">Benefits of this approach include:</span></span>

* <span data-ttu-id="40839-111">**Få verkliga feedback *innan* uppdateringar som släpps till produktion** -enda som är bättre än får feedback så fort du släpper har blivit feedback innan du släpper.</span><span class="sxs-lookup"><span data-stu-id="40839-111">**Gain real feedback *before* updates are released to production** - The only thing better than gaining feedback as soon as you release is gaining feedback before you release.</span></span> <span data-ttu-id="40839-112">Du kan testa uppdateringar med verkliga användaraktiviteten och beteenden tidigt du önskar i produktens livscykel.</span><span class="sxs-lookup"><span data-stu-id="40839-112">You can test updates with real user traffic and behaviors as early as you desire in the product life cycle.</span></span>
* <span data-ttu-id="40839-113">**Förbättra [kontinuerlig test-driven utveckling (CTDD)](https://en.wikipedia.org/wiki/Continuous_test-driven_development)**  – genom att integrera test i produktion med kontinuerlig integrering och instrumentation med Application Insights validering av användare händer tidigt och automatiskt i din produktens livscykel.</span><span class="sxs-lookup"><span data-stu-id="40839-113">**Enhance [continuous test-driven development (CTDD)](https://en.wikipedia.org/wiki/Continuous_test-driven_development)** - By integrating test in production with continuous integration and instrumentation with Application Insights, user validation happens early and automatically in your product life cycle.</span></span> <span data-ttu-id="40839-114">Detta minskar tiden investeringar i manuell testkörning.</span><span class="sxs-lookup"><span data-stu-id="40839-114">This helps reduce time investments in manual test execution.</span></span>
* <span data-ttu-id="40839-115">**Optimera testa arbetsflödet** -genom att automatisera test i produktion med kontinuerlig övervakning instrumentation, du kan potentiellt göra målen för olika typer av testerna i en enda process som [integrering](https://en.wikipedia.org/wiki/Integration_testing), [regression](https://en.wikipedia.org/wiki/Regression_testing), [användbarhet](https://en.wikipedia.org/wiki/Usability_testing), tillgänglighet, lokalisering, [prestanda](https://en.wikipedia.org/wiki/Software_performance_testing), [säkerhet](https://en.wikipedia.org/wiki/Security_testing), och [ godkännande](https://en.wikipedia.org/wiki/Acceptance_testing).</span><span class="sxs-lookup"><span data-stu-id="40839-115">**Optimize test workflow** - By automating test in production with continuous monitoring instrumentation, you can potentially accomplish the goals of various kinds of tests in a single process, such as [integration](https://en.wikipedia.org/wiki/Integration_testing), [regression](https://en.wikipedia.org/wiki/Regression_testing), [usability](https://en.wikipedia.org/wiki/Usability_testing), accessibility, localization, [performance](https://en.wikipedia.org/wiki/Software_performance_testing), [security](https://en.wikipedia.org/wiki/Security_testing), and [acceptance](https://en.wikipedia.org/wiki/Acceptance_testing).</span></span>

<span data-ttu-id="40839-116">En flighting distribution inte bara om routning live trafik.</span><span class="sxs-lookup"><span data-stu-id="40839-116">A flighting deployment is not just about routing live traffic.</span></span> <span data-ttu-id="40839-117">I en sådan distribution du vill få insyn så snabbt som möjligt, om det vara ett oväntat fel, prestandaförsämring, user experience problem.</span><span class="sxs-lookup"><span data-stu-id="40839-117">In such a deployment you want to gain insight as quickly as possible, whether it be an unexpected bug, performance degradation, user experience issues.</span></span> <span data-ttu-id="40839-118">Kom ihåg att du arbetar med verkliga kunder.</span><span class="sxs-lookup"><span data-stu-id="40839-118">Remember, you are dealing with real customers.</span></span> <span data-ttu-id="40839-119">Så här gör du det höger, måste du se till att du har lagt upp flighting distributionen för att samla in alla data som du behöver för att fatta ett välgrundat beslut för din nästa steg.</span><span class="sxs-lookup"><span data-stu-id="40839-119">So to do it right, you must make sure that you have set up your flighting deployment to gather all the data you need in order to make an informed decision for your next step.</span></span> <span data-ttu-id="40839-120">Den här kursen visar hur du samlar in data med Application Insights, men du kan använda New Relic eller andra tekniker som passar ditt scenario.</span><span class="sxs-lookup"><span data-stu-id="40839-120">This tutorial shows you how to collect data with Application Insights, but you can use New Relic or other technologies that suits your scenario.</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="40839-121">Vad du ska göra</span><span class="sxs-lookup"><span data-stu-id="40839-121">What you will do</span></span>
<span data-ttu-id="40839-122">I kursen får du lära dig hur att göra följande scenarier tillsammans för att testa din Apptjänst-app i produktion:</span><span class="sxs-lookup"><span data-stu-id="40839-122">In this tutorial, you will learn how to bring the following scenarios together to test your App Service app in production:</span></span>

* <span data-ttu-id="40839-123">[Vidarebefordra trafik för produktion](app-service-web-test-in-production-get-start.md) i appen beta</span><span class="sxs-lookup"><span data-stu-id="40839-123">[Route production traffic](app-service-web-test-in-production-get-start.md) to your beta app</span></span>
* <span data-ttu-id="40839-124">[Instrumentera appen](../application-insights/app-insights-web-track-usage.md) att hämta användbara mätvärden</span><span class="sxs-lookup"><span data-stu-id="40839-124">[Instrument your app](../application-insights/app-insights-web-track-usage.md) to obtain useful metrics</span></span>
* <span data-ttu-id="40839-125">Distribuera appen beta och spåra aktiva app mått kontinuerligt</span><span class="sxs-lookup"><span data-stu-id="40839-125">Continuously deploy your beta app and track live app metrics</span></span>
* <span data-ttu-id="40839-126">Jämför mått mellan produktionsprogrammet och beta-app för att se hur koden konverteringar till resultat</span><span class="sxs-lookup"><span data-stu-id="40839-126">Compare metrics between the production app and the beta app to see how code changes translate to results</span></span>

## <a name="what-you-will-need"></a><span data-ttu-id="40839-127">Vad du behöver</span><span class="sxs-lookup"><span data-stu-id="40839-127">What you will need</span></span>
* <span data-ttu-id="40839-128">Ett Azure-konto</span><span class="sxs-lookup"><span data-stu-id="40839-128">An Azure account</span></span>
* <span data-ttu-id="40839-129">En [GitHub](https://github.com/) konto</span><span class="sxs-lookup"><span data-stu-id="40839-129">A [GitHub](https://github.com/) account</span></span>
* <span data-ttu-id="40839-130">Visual Studio 2015 – du kan hämta den [Community edition](https://www.visualstudio.com/en-us/products/visual-studio-express-vs.aspx).</span><span class="sxs-lookup"><span data-stu-id="40839-130">Visual Studio 2015 - you can download the [Community edition](https://www.visualstudio.com/en-us/products/visual-studio-express-vs.aspx).</span></span>
* <span data-ttu-id="40839-131">Git Shell (installeras med [GitHub för Windows](https://windows.github.com/))-det här kan du köra Git- och PowerShell-kommandon i samma session</span><span class="sxs-lookup"><span data-stu-id="40839-131">Git Shell (installed with [GitHub for Windows](https://windows.github.com/)) - this enables you to run both the Git and PowerShell commands in the same session</span></span>
* <span data-ttu-id="40839-132">Senaste [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/download/v0.9.8-September2015/azure-powershell.0.9.8.msi) bits</span><span class="sxs-lookup"><span data-stu-id="40839-132">Latest [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/download/v0.9.8-September2015/azure-powershell.0.9.8.msi) bits</span></span>
* <span data-ttu-id="40839-133">Grundläggande förståelse av följande:</span><span class="sxs-lookup"><span data-stu-id="40839-133">Basic understanding of the following:</span></span>
  * <span data-ttu-id="40839-134">[Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) malldistribution (se [distribuerar ett komplexa program förutsägbart i Azure](app-service-deploy-complex-application-predictably.md))</span><span class="sxs-lookup"><span data-stu-id="40839-134">[Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) template deployment (see [Deploy a complex application predictably in Azure](app-service-deploy-complex-application-predictably.md))</span></span>
  * [<span data-ttu-id="40839-135">Git</span><span class="sxs-lookup"><span data-stu-id="40839-135">Git</span></span>](http://git-scm.com/documentation)
  * [<span data-ttu-id="40839-136">PowerShell</span><span class="sxs-lookup"><span data-stu-id="40839-136">PowerShell</span></span>](https://technet.microsoft.com/library/bb978526.aspx)

> [!NOTE]
> <span data-ttu-id="40839-137">Du behöver ett Azure-konto för att kunna slutföra den här guiden:</span><span class="sxs-lookup"><span data-stu-id="40839-137">You need an Azure account to complete this tutorial:</span></span>
>
> * <span data-ttu-id="40839-138">Du kan [öppna ett Azure-konto gratis](https://azure.microsoft.com/pricing/free-trial/) – du får kredit du kan använda för att testa Azure-betaltjänster och även när de används du kan behålla kontot och använda kostnadsfria Azure-tjänster, till exempel Web Apps.</span><span class="sxs-lookup"><span data-stu-id="40839-138">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services, such as Web Apps.</span></span>
> * <span data-ttu-id="40839-139">Du kan [aktivera Visual Studio-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) -din Visual Studio-prenumeration ger dig krediter varje månad som du kan använda för Azure-betaltjänster.</span><span class="sxs-lookup"><span data-stu-id="40839-139">You can [activate Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) - Your Visual Studio subscription gives you credits every month that you can use for paid Azure services.</span></span>
>
> <span data-ttu-id="40839-140">Om du vill komma igång med Azure Apptjänst innan du registrerar dig för ett Azure-konto kan du gå till [Prova Apptjänst](https://azure.microsoft.com/try/app-service/). Där kan du direkt skapa en tillfällig startwebbapp i Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="40839-140">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="40839-141">Inget kreditkort krävs, och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="40839-141">No credit cards required; no commitments.</span></span>
>
>

## <a name="set-up-your-production-web-app"></a><span data-ttu-id="40839-142">Konfigurera ditt webbprogram för produktion</span><span class="sxs-lookup"><span data-stu-id="40839-142">Set up your production web app</span></span>
> [!NOTE]
> <span data-ttu-id="40839-143">Skriptet som används i den här självstudiekursen konfigurerar automatiskt kontinuerlig publicering från GitHub-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="40839-143">The script used in this tutorial will automatically configure continuous publishing from your GitHub repository.</span></span> <span data-ttu-id="40839-144">Detta kräver att dina GitHub-autentiseringsuppgifter lagras redan i Azure, annars skriptbaserade distributionen misslyckas vid försök att konfigurera inställningar för kontroll av datakälla för web apps.</span><span class="sxs-lookup"><span data-stu-id="40839-144">This requires that your GitHub credentials are already stored in Azure, otherwise the scripted deployment will fail when attempting to configure source control settings for the web apps.</span></span>
>
> <span data-ttu-id="40839-145">För att lagra dina GitHub-autentiseringsuppgifter i Azure, skapa en webbapp i den [Azure Portal](https://portal.azure.com/) och [konfigurera GitHub distribution](app-service-continuous-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="40839-145">To store your GitHub credentials in Azure, create a web app in the [Azure Portal](https://portal.azure.com/) and [configure GitHub deployment](app-service-continuous-deployment.md).</span></span> <span data-ttu-id="40839-146">Du behöver bara göra detta en gång.</span><span class="sxs-lookup"><span data-stu-id="40839-146">You only need to do this once.</span></span>
>
>

<span data-ttu-id="40839-147">Du har ett program som körs live i Azure i ett typiskt scenario för DevOps, och du vill göra ändringar genom kontinuerlig publicering.</span><span class="sxs-lookup"><span data-stu-id="40839-147">In a typical DevOps scenario, you have an application that’s running live in Azure, and you want to make changes to it through continuous publishing.</span></span> <span data-ttu-id="40839-148">I detta scenario distribuerar du en mall som du har utvecklats och testats till produktion.</span><span class="sxs-lookup"><span data-stu-id="40839-148">In this scenario, you will deploy to production a template that you have developed and tested.</span></span>

1. <span data-ttu-id="40839-149">Skapa din egen förgrening av den [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) databasen.</span><span class="sxs-lookup"><span data-stu-id="40839-149">Create your own fork of the [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) repository.</span></span> <span data-ttu-id="40839-150">Information om hur du skapar din förgrening finns [duplicera en Repo](https://help.github.com/articles/fork-a-repo/).</span><span class="sxs-lookup"><span data-stu-id="40839-150">For information on creating your fork, see [Fork a Repo](https://help.github.com/articles/fork-a-repo/).</span></span> <span data-ttu-id="40839-151">När din förgrening har skapats kan se du den i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="40839-151">Once your fork is created, you can see it in your browser.</span></span>

   ![](./media/app-service-agile-software-development/production-1-private-repo.png)
2. <span data-ttu-id="40839-152">Öppna en session för Git-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="40839-152">Open a Git Shell session.</span></span> <span data-ttu-id="40839-153">Om du inte har Git Shell ännu, installera [GitHub för Windows](https://windows.github.com/) nu.</span><span class="sxs-lookup"><span data-stu-id="40839-153">If you don't have Git Shell yet, install [GitHub for Windows](https://windows.github.com/) now.</span></span>
3. <span data-ttu-id="40839-154">Skapa en lokal kloning av din förgrening genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="40839-154">Create a local clone of your fork by executing the following command:</span></span>

     <span data-ttu-id="40839-155">Git-klon https://github.com/<your_fork>/ToDoApp.git</span><span class="sxs-lookup"><span data-stu-id="40839-155">git clone https://github.com/<your_fork>/ToDoApp.git</span></span>
4. <span data-ttu-id="40839-156">När du har din lokala kloning, gå till  *&lt;repository_root >*\ARMTemplates och köra deploy.ps1 skript med ett unikt suffix enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="40839-156">Once you have your local clone, navigate to *&lt;repository_root>*\ARMTemplates, and run the deploy.ps1 script with a unique suffix, as shown below:</span></span>

     <span data-ttu-id="40839-157">.\deploy.ps1 – RepoUrl https://github.com/<your_fork>/todoapp.git - ResourceGroupSuffix < your_suffix ></span><span class="sxs-lookup"><span data-stu-id="40839-157">.\deploy.ps1 –RepoUrl https://github.com/<your_fork>/todoapp.git -ResourceGroupSuffix <your_suffix></span></span>
5. <span data-ttu-id="40839-158">När du uppmanas ange önskat användarnamn och lösenord för åtkomst till databasen.</span><span class="sxs-lookup"><span data-stu-id="40839-158">When prompted, type in the desired username and password for database access.</span></span> <span data-ttu-id="40839-159">Kom ihåg autentiseringsuppgifterna databasen eftersom du behöver ange dem igen när du uppdaterar resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="40839-159">Remember your database credentials because you will need to specify them again when updating the resource group.</span></span>

   <span data-ttu-id="40839-160">Du bör se etablering förloppet för olika Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="40839-160">You should see the provisioning progress of various Azure resources.</span></span> <span data-ttu-id="40839-161">När distributionen är klar kommer skriptet Starta program i webbläsaren och ger dig ett eget signal.</span><span class="sxs-lookup"><span data-stu-id="40839-161">When deployment completes, the script will launch the application in the browser and give you a friendly beep.</span></span>
   ![](./media/app-service-web-test-in-production-controlled-test-flight/00.1-app-in-browser.png)
6. <span data-ttu-id="40839-162">Tillbaka i sessionen Git Shell, kör du:</span><span class="sxs-lookup"><span data-stu-id="40839-162">Back in your Git Shell session, run:</span></span>

     <span data-ttu-id="40839-163">. \swap – namn ToDoApp < your_suffix ></span><span class="sxs-lookup"><span data-stu-id="40839-163">.\swap –Name ToDoApp<your_suffix></span></span>

   ![](./media/app-service-web-test-in-production-controlled-test-flight/00.2-swap-to-production.png)
7. <span data-ttu-id="40839-164">Gå tillbaka till Bläddra till den frontend-adress när skriptet har slutförts (http://ToDoApp*&lt;your_suffix >*.azurewebsites.net/) att se det program som körs i produktion.</span><span class="sxs-lookup"><span data-stu-id="40839-164">When the script finishes, go back to browse to the frontend’s address (http://ToDoApp*&lt;your_suffix>*.azurewebsites.net/) to see the application running in production.</span></span>
8. <span data-ttu-id="40839-165">Logga in på den [Azure Portal](https://portal.azure.com/) och ta en titt på vad som har skapats.</span><span class="sxs-lookup"><span data-stu-id="40839-165">Log into the [Azure Portal](https://portal.azure.com/) and take a look at what’s created.</span></span>

   <span data-ttu-id="40839-166">Du ska kunna se två web apps i samma resursgrupp, en med den `Api` suffix i namnet.</span><span class="sxs-lookup"><span data-stu-id="40839-166">You should be able to see two web apps in the same resource group, one with the `Api` suffix in the name.</span></span> <span data-ttu-id="40839-167">Om du tittar på vyn för gruppen kan även se SQL-databasen och server App Service-plan och fristående fack för web apps.</span><span class="sxs-lookup"><span data-stu-id="40839-167">If you look at the resource group view, you will also see the SQL Database and server, the App Service plan, and the staging slots for the web apps.</span></span> <span data-ttu-id="40839-168">Bläddra igenom de olika resurserna och jämför dem med  *&lt;repository_root >*\ARMTemplates\ProdAndStage.json att se hur de är konfigurerade i mallen.</span><span class="sxs-lookup"><span data-stu-id="40839-168">Browse through the different resources and compare them with *&lt;repository_root>*\ARMTemplates\ProdAndStage.json to see how they are configured in the template.</span></span>

   ![](./media/app-service-web-test-in-production-controlled-test-flight/00.3-resource-group-view.png)

<span data-ttu-id="40839-169">Du har lagt upp produktionsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="40839-169">You have set up the production app.</span></span>  <span data-ttu-id="40839-170">Nu kan vi anta att du får feedback att användbarhet är dålig för appen.</span><span class="sxs-lookup"><span data-stu-id="40839-170">Now, let's imagine that you receive feedback that usability is poor for the app.</span></span> <span data-ttu-id="40839-171">Så att du vill undersöka.</span><span class="sxs-lookup"><span data-stu-id="40839-171">So you decide to investigate.</span></span> <span data-ttu-id="40839-172">Ska du instrumentera din app om du vill ge feedback.</span><span class="sxs-lookup"><span data-stu-id="40839-172">You're going to instrument your app to give you feedback.</span></span>

## <a name="investigate-instrument-your-client-app-for-monitoringmetrics"></a><span data-ttu-id="40839-173">Undersök: Instrumentera klientappen för övervakning/mått</span><span class="sxs-lookup"><span data-stu-id="40839-173">Investigate: Instrument your client app for monitoring/metrics</span></span>
1. <span data-ttu-id="40839-174">Öppna  *&lt;repository_root >*\src\MultiChannelToDo.sln i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="40839-174">Open *&lt;repository_root>*\src\MultiChannelToDo.sln in Visual Studio.</span></span>
2. <span data-ttu-id="40839-175">Återställa alla Nuget-paket genom att högerklicka på lösningen > **hantera NuGet-paket för lösningen** > **återställa**.</span><span class="sxs-lookup"><span data-stu-id="40839-175">Restore all Nuget packages by right-clicking solution > **Manage NuGet Packages for Solution** > **Restore**.</span></span>
3. <span data-ttu-id="40839-176">Högerklicka på **MultiChannelToDo.Web** > **Lägg till Application Insights Telemetry** > **konfigurera inställningarna för** > ändra resursgrupp ToDoApp*&lt;your_suffix >* > **Lägg till Application Insights i projekt**.</span><span class="sxs-lookup"><span data-stu-id="40839-176">Right-click **MultiChannelToDo.Web** > **Add Application Insights Telemetry** > **Configure Settings** > Change resource group to ToDoApp*&lt;your_suffix>* > **Add Application Insights to Project**.</span></span>
4. <span data-ttu-id="40839-177">Öppna bladet för i Azure-portalen på **MultiChannelToDo.Web** Application Insights-resurs.</span><span class="sxs-lookup"><span data-stu-id="40839-177">In the Azure Portal, open the blade for the **MultiChannelToDo.Web** Application Insight resource.</span></span> <span data-ttu-id="40839-178">I den **programhälsan** del, klickar du på **Lär dig att samla in sidhämtningsinformation för webbläsare** > Kopiera kod.</span><span class="sxs-lookup"><span data-stu-id="40839-178">Then in the **Application health** part, click **Learn how to collect browser page load data** > copy code.</span></span>
5. <span data-ttu-id="40839-179">Lägg till den kopierade JS instrumentation kod  *&lt;repository_root >*\src\MultiChannelToDo.Web\app\Index.cshtml precis före avslutande `<heading>` tagg.</span><span class="sxs-lookup"><span data-stu-id="40839-179">Add the copied JS instrumentation code to *&lt;repository_root>*\src\MultiChannelToDo.Web\app\Index.cshtml, just before the closing `<heading>` tag.</span></span> <span data-ttu-id="40839-180">Den ska innehålla unika instrumentation nyckeln för Application Insights-resurs.</span><span class="sxs-lookup"><span data-stu-id="40839-180">It should contain the unique instrumentation key of your Application Insight resource.</span></span>

        <script type="text/javascript">
        var appInsights=window.appInsights||function(config){
            function s(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},r=document,f=window,e="script",o=r.createElement(e),i,u;for(o.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js",r.getElementsByTagName(e)[0].parentNode.appendChild(o),t.cookie=r.cookie,t.queue=[],i=["Event","Exception","Metric","PageView","Trace"];i.length;)s("track"+i.pop());return config.disableExceptionTracking||(i="onerror",s("_"+i),u=f[i],f[i]=function(config,r,f,e,o){var s=u&&u(config,r,f,e,o);return s!==!0&&t["_"+i](config,r,f,e,o),s}),t
        }({
            instrumentationKey:"<your_unique_instrumentation_key>"
        });

        window.appInsights=appInsights;
        appInsights.trackPageView();
        </script>
6. <span data-ttu-id="40839-181">Skicka anpassade händelser till Application Insights för mus klickar genom att lägga till följande kod längst ned i brödtext:</span><span class="sxs-lookup"><span data-stu-id="40839-181">Send custom events to Application Insights for mouse clicks by adding the following code to the bottom of body:</span></span>

       <script>
           $(document.body).find("*").click(function(event) {

               appInsights.trackEvent(event.target.tagName + ": " + event.target.className);
           });
       </script>

   <span data-ttu-id="40839-182">JavaScript-kodfragment skickar en anpassad händelse till Application Insights varje gång en användare klickar någonstans i webbapp.</span><span class="sxs-lookup"><span data-stu-id="40839-182">This JavaScript snippet sends a custom event to Application Insights every time a user clicks anywhere in the web app.</span></span>
7. <span data-ttu-id="40839-183">Git Shell, genomför och skicka ändringarna till din förgrening i GitHub.</span><span class="sxs-lookup"><span data-stu-id="40839-183">In Git Shell, commit and push your changes to your fork in GitHub.</span></span> <span data-ttu-id="40839-184">Vänta sedan klienter ska kunna uppdatera webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="40839-184">Then, wait for clients to refresh browser.</span></span>

       git add -A :/
       git commit -m "add AI configuration for client app"
       git push origin master
8. <span data-ttu-id="40839-185">Byt distribuerad app-ändringar till produktion:</span><span class="sxs-lookup"><span data-stu-id="40839-185">Swap the deployed app changes to production:</span></span>

     <span data-ttu-id="40839-186">. \swap – namn ToDoApp < your_suffix ></span><span class="sxs-lookup"><span data-stu-id="40839-186">.\swap –Name ToDoApp<your_suffix></span></span>
9. <span data-ttu-id="40839-187">Bläddra till Application Insights-resursen som du har konfigurerat.</span><span class="sxs-lookup"><span data-stu-id="40839-187">Browse to the Application Insights resource that you configured.</span></span> <span data-ttu-id="40839-188">Klicka på anpassade händelser.</span><span class="sxs-lookup"><span data-stu-id="40839-188">Click Custom events.</span></span>

   ![](./media/app-service-web-test-in-production-controlled-test-flight/01-custom-events.png)

   <span data-ttu-id="40839-189">Om du inte ser mätvärden för anpassade händelser, Vänta några minuter och klicka på **uppdatera**.</span><span class="sxs-lookup"><span data-stu-id="40839-189">If you don't see metrics for custom events, wait a few minutes and click **Refresh**.</span></span>

<span data-ttu-id="40839-190">Anta att du ser ett diagram som nedan:</span><span class="sxs-lookup"><span data-stu-id="40839-190">Suppose you see a chart like below:</span></span>

![](./media/app-service-web-test-in-production-controlled-test-flight/02-custom-events-chart-view.png)

<span data-ttu-id="40839-191">Och händelsen rutnätet nedan:</span><span class="sxs-lookup"><span data-stu-id="40839-191">And the event grid below it:</span></span>

![](./media/app-service-web-test-in-production-controlled-test-flight/03-custom-event-grid-view.png)

<span data-ttu-id="40839-192">Enligt programkoden ToDoApp den **knappen** händelse som motsvarar skickaknappen och **indata** händelse som motsvarar textrutan.</span><span class="sxs-lookup"><span data-stu-id="40839-192">According to your ToDoApp application code, the **BUTTON** event corresponds to the submit button, and the **INPUT** event corresponds to the textbox.</span></span> <span data-ttu-id="40839-193">Hittills vara saker meningsfullt.</span><span class="sxs-lookup"><span data-stu-id="40839-193">So far, things make sense.</span></span> <span data-ttu-id="40839-194">Men det verkar som om det är mycket musklickningar och mycket få klick på att göra-objekt (det **LI** händelser).</span><span class="sxs-lookup"><span data-stu-id="40839-194">However, it looks like there's a good amount of clicks and very few clicks on the to-do items (the **LI** events).</span></span>

<span data-ttu-id="40839-195">Baserat på det formulär du den hypotesen några användare kan blandas ihop vilken del av Användargränssnittet är klickbara och beror det på markören är formaterad för textmarkering när den är placerad på objekten i listan och deras ikoner.</span><span class="sxs-lookup"><span data-stu-id="40839-195">Based on this, you form your hypothesis that some users are confused which part of the UI is clickable and it is because the cursor is styled for text selection when it hovers on the list items and their icons.</span></span>

![](./media/app-service-web-test-in-production-controlled-test-flight/04-to-do-list-item-ui.png)

<span data-ttu-id="40839-196">Det här är ett exempel på contrived.</span><span class="sxs-lookup"><span data-stu-id="40839-196">This might be a contrived example.</span></span> <span data-ttu-id="40839-197">Dock ska du göra en förbättring i appen och utför sedan en flighting distribution för att få användbarhet feedback från live-kunder.</span><span class="sxs-lookup"><span data-stu-id="40839-197">Nevertheless, you're going to make an improvement to your app, and then perform a flighting deployment to get usability feedback from live customers.</span></span>

### <a name="instrument-your-server-app-for-monitoringmetrics"></a><span data-ttu-id="40839-198">Instrumentera din server-app för övervakning/mått</span><span class="sxs-lookup"><span data-stu-id="40839-198">Instrument your server app for monitoring/metrics</span></span>
<span data-ttu-id="40839-199">Detta är en tangent eftersom det scenario som visas i den här självstudiekursen bara behandlar klientappen.</span><span class="sxs-lookup"><span data-stu-id="40839-199">This is a tangent since the scenario demonstrated in this tutorial only deals with the client app.</span></span> <span data-ttu-id="40839-200">För fullständighetens skull kommer du dock ställa in serversidan appen.</span><span class="sxs-lookup"><span data-stu-id="40839-200">However, for completeness you will set up the server-side app.</span></span>

1. <span data-ttu-id="40839-201">Högerklicka på **MultiChannelToDo** > **Lägg till Application Insights Telemetry** > **konfigurera inställningarna för** > ändra resursgrupp ToDoApp*&lt;your_suffix >* > **Lägg till Application Insights i projekt**.</span><span class="sxs-lookup"><span data-stu-id="40839-201">Right-click **MultiChannelToDo** > **Add Application Insights Telemetry** > **Configure Settings** > Change resource group to ToDoApp*&lt;your_suffix>* > **Add Application Insights to Project**.</span></span>
2. <span data-ttu-id="40839-202">Git Shell, genomför och skicka ändringarna till din förgrening i GitHub.</span><span class="sxs-lookup"><span data-stu-id="40839-202">In Git Shell, commit and push your changes to your fork in GitHub.</span></span> <span data-ttu-id="40839-203">Vänta sedan klienter ska kunna uppdatera webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="40839-203">Then, wait for clients to refresh browser.</span></span>

       git add -A :/
       git commit -m "add AI configuration for server app"
       git push origin master
3. <span data-ttu-id="40839-204">Byt distribuerad app-ändringar till produktion:</span><span class="sxs-lookup"><span data-stu-id="40839-204">Swap the deployed app changes to production:</span></span>

     <span data-ttu-id="40839-205">. \swap – namn ToDoApp < your_suffix ></span><span class="sxs-lookup"><span data-stu-id="40839-205">.\swap –Name ToDoApp<your_suffix></span></span>

<span data-ttu-id="40839-206">Klart!</span><span class="sxs-lookup"><span data-stu-id="40839-206">That's it!</span></span>

## <a name="investigate-add-slot-specific-tags-to-your-client-app-metrics"></a><span data-ttu-id="40839-207">Undersök: Lägga till fack-specifika taggar i din app mått för klienten</span><span class="sxs-lookup"><span data-stu-id="40839-207">Investigate: Add slot-specific tags to your client app metrics</span></span>
<span data-ttu-id="40839-208">I det här avsnittet ska du konfigurera olika distributionsplatser för att skicka telemetri om plats-specifika till samma Application Insights-resurs.</span><span class="sxs-lookup"><span data-stu-id="40839-208">In this section, you will configure the different deployment slots to send slot-specific telemetry to the same Application Insights resource.</span></span> <span data-ttu-id="40839-209">På så sätt kan du jämföra telemetridata mellan trafik från olika platser (distributionsmiljöer) för att enkelt se effekten av ändringarna app.</span><span class="sxs-lookup"><span data-stu-id="40839-209">This way, you can compare telemetry data between traffic from different slots (deployment environments) to easily see the effect of your app changes.</span></span> <span data-ttu-id="40839-210">På samma gång, kan du separera trafiken produktion från resten så att du kan fortsätta att övervaka din produktionsapp efter behov.</span><span class="sxs-lookup"><span data-stu-id="40839-210">At the same time, you can separate the production traffic from the rest so you can continue to monitor your production app as needed.</span></span>

<span data-ttu-id="40839-211">Eftersom du samla in data på klientbeteende, kommer du att [lägga till en telemetri initieraren i JavaScript-kod](../application-insights/app-insights-api-filtering-sampling.md) i index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="40839-211">Since you're gathering data on client behavior, you will [add a telemetry initializer to your JavaScript code](../application-insights/app-insights-api-filtering-sampling.md) in index.cshtml.</span></span> <span data-ttu-id="40839-212">Om du vill testa prestanda för serversidan, t.ex, du kan också göra på samma sätt i serverkoden (se [Application Insights API för anpassade händelser och mått](../application-insights/app-insights-api-custom-events-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="40839-212">If you want to test server-side performance, for example, you can also do similarly in your server code (see [Application Insights API for custom events and metrics](../application-insights/app-insights-api-custom-events-metrics.md).</span></span>

1. <span data-ttu-id="40839-213">Lägg först till kod mellan två `//` kommentarer nedan i JavaScript blockera som du har lagts till i `<heading>` tagga tidigare.</span><span class="sxs-lookup"><span data-stu-id="40839-213">First, add the code bewteen the two `//` comments below in the JavaScript block that you added to the `<heading>` tag earlier.</span></span>

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

    <span data-ttu-id="40839-214">Initieraren koden gör den `appInsights` objekt att lägga till den kallas en anpassad egenskap `Environment` till all telemetri som skickas.</span><span class="sxs-lookup"><span data-stu-id="40839-214">This initializer code causes the `appInsights` object to add the a custom property called `Environment` to every piece of telemetry it sends.</span></span>
2. <span data-ttu-id="40839-215">Lägg till den anpassade egenskapen som en [fack inställningen](web-sites-staged-publishing.md#AboutConfiguration) för webbappen i Azure.</span><span class="sxs-lookup"><span data-stu-id="40839-215">Next, add this custom property as a [slot setting](web-sites-staged-publishing.md#AboutConfiguration) for your web app in Azure.</span></span> <span data-ttu-id="40839-216">Gör detta genom att köra följande kommandon i Git Shell-sessionen.</span><span class="sxs-lookup"><span data-stu-id="40839-216">To do this, run the following commands in your Git Shell session.</span></span>

        $app = Get-AzureWebsite -Name todoapp<your_suffix> -Slot production
        $app.AppSettings.Add("environment", "Production")
        $app.SlotStickyAppSettingNames.Add("environment")
        $app | Set-AzureWebsite -Name todoapp<your_suffix> -Slot production

    <span data-ttu-id="40839-217">Filen Web.config i projektet redan definierar den `environment` appinställningen.</span><span class="sxs-lookup"><span data-stu-id="40839-217">The Web.config in your project already defines the `environment` app setting.</span></span> <span data-ttu-id="40839-218">Med den här inställningen när du testar appen lokalt, dina märks med `VS Debugger`.</span><span class="sxs-lookup"><span data-stu-id="40839-218">With this setting, when you test the app locally, your metrics will be tagged with `VS Debugger`.</span></span> <span data-ttu-id="40839-219">Men när du pusha ändringarna till Azure, Azure ska hitta och använda den `environment` app inställningen i konfigurationen för webbapp i stället och dina märkta med `Production`.</span><span class="sxs-lookup"><span data-stu-id="40839-219">However, when you push your changes to Azure, Azure will find and use the `environment` app setting in the web app's configuration instead, and your metrics will be tagged with `Production`.</span></span>
3. <span data-ttu-id="40839-220">Bekräfta och skicka din kodändringar till din förgrening på GitHub och vänta användarna att använda den nya appen (behöver uppdatera webbläsaren).</span><span class="sxs-lookup"><span data-stu-id="40839-220">Commit and push your code changes to your fork on GitHub, and then wait for your users to use the new app (need to refresh the browser).</span></span> <span data-ttu-id="40839-221">Det tar ungefär 15 minuter för den nya egenskapen visas i Application Insights `MultiChannelToDo.Web` resurs.</span><span class="sxs-lookup"><span data-stu-id="40839-221">It takes about 15 minutes for the new property to show up in your Application Insights `MultiChannelToDo.Web` resource.</span></span>

        git add -A :/
        git commit -m "add environment property to AI events for client app"
        git push origin master
4. <span data-ttu-id="40839-222">Gå till den **anpassad händelser** bladet igen och filtrera mätvärdena på `Environment=Production`.</span><span class="sxs-lookup"><span data-stu-id="40839-222">Now, go to the **Custom events** blade again and filter the metrics on `Environment=Production`.</span></span> <span data-ttu-id="40839-223">Nu bör du kunna se alla nya anpassade händelser på produktionsplatsen med det här filtret.</span><span class="sxs-lookup"><span data-stu-id="40839-223">You should now be able to see all the new custom events in the production slot with this filter.</span></span>

    ![](./media/app-service-web-test-in-production-controlled-test-flight/05-filter-on-production-environment.png)
5. <span data-ttu-id="40839-224">Klicka på den **Favoriter** för att spara de aktuella Metrics Explorer-inställningarna till något som liknar **anpassad händelser: produktion**.</span><span class="sxs-lookup"><span data-stu-id="40839-224">Click the **Favorites** button to save the current Metrics Explorer settings to something like **Custom events: Production**.</span></span> <span data-ttu-id="40839-225">Du kan enkelt växla mellan den här vyn och vyn distribution fack senare.</span><span class="sxs-lookup"><span data-stu-id="40839-225">You can easily switch between this view and a deployment slot view later.</span></span>

   > [!TIP]
   > <span data-ttu-id="40839-226">För ännu mer kraftfulla analytics, Överväg [integrera Application Insights-resursen med Power BI](../application-insights/app-insights-export-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="40839-226">For even more powerful analytics, consider [integrating your Application Insights resource with Power BI](../application-insights/app-insights-export-power-bi.md).</span></span>
   >
   >

### <a name="add-slot-specific-tags-to-your-server-app-metrics"></a><span data-ttu-id="40839-227">Lägg till plats-specifika taggar i din app serverstatistik</span><span class="sxs-lookup"><span data-stu-id="40839-227">Add slot-specific tags to your server app metrics</span></span>
<span data-ttu-id="40839-228">Igen, ska du ställa in appen serversidan för fullständighetens skull.</span><span class="sxs-lookup"><span data-stu-id="40839-228">Again, for completeness you will set up the server-side app.</span></span> <span data-ttu-id="40839-229">Till skillnad från klientappen som instrumenterats i JavaScript instrumenterats fack-specifika taggar för server-app med .NET-kod.</span><span class="sxs-lookup"><span data-stu-id="40839-229">Unlike the client app which is instrumented in JavaScript, slot-specific tags for the server app is instrumented with .NET code.</span></span>

1. <span data-ttu-id="40839-230">Öppna  *&lt;repository_root >*\src\MultiChannelToDo\Global.asax.cs.</span><span class="sxs-lookup"><span data-stu-id="40839-230">Open *&lt;repository_root>*\src\MultiChannelToDo\Global.asax.cs.</span></span> <span data-ttu-id="40839-231">Lägg till kodblocket nedan, precis före avslutande klammerparentesen för namnområdet.</span><span class="sxs-lookup"><span data-stu-id="40839-231">Add the code block below, just before the closing namespace curly brace.</span></span>

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
2. <span data-ttu-id="40839-232">Korrigera namnet upplösning fel genom att lägga till den `using` instruktionerna nedan till början av filen:</span><span class="sxs-lookup"><span data-stu-id="40839-232">Correct the name resolution errors by adding the `using` statements below to the beginning of the file:</span></span>

        using Microsoft.ApplicationInsights.Channel;
        using Microsoft.ApplicationInsights.Extensibility;
3. <span data-ttu-id="40839-233">Lägg till koden nedan i början av den `Application_Start()` metoden:</span><span class="sxs-lookup"><span data-stu-id="40839-233">Add the code below to the beginning of the `Application_Start()` method:</span></span>

        TelemetryConfiguration.Active.TelemetryInitializers.Add(new ConfigInitializer());
4. <span data-ttu-id="40839-234">Bekräfta och skicka din kodändringar till din förgrening på GitHub och vänta användarna att använda den nya appen (behöver uppdatera webbläsaren).</span><span class="sxs-lookup"><span data-stu-id="40839-234">Commit and push your code changes to your fork on GitHub, and then wait for your users to use the new app (need to refresh the browser).</span></span> <span data-ttu-id="40839-235">Det tar ungefär 15 minuter för den nya egenskapen visas i Application Insights `MultiChannelToDo` resurs.</span><span class="sxs-lookup"><span data-stu-id="40839-235">It takes about 15 minutes for the new property to show up in your Application Insights `MultiChannelToDo` resource.</span></span>

        git add -A :/
        git commit -m "add environment property to AI events for server app"
        git push origin master

## <a name="update-set-up-your-beta-branch"></a><span data-ttu-id="40839-236">Uppdatering: Ställ in din gren beta</span><span class="sxs-lookup"><span data-stu-id="40839-236">Update: Set up your beta branch</span></span>
1. <span data-ttu-id="40839-237">Öppna  *&lt;repository_root >*\ARMTemplates\ProdAndStagetest.json och söka efter den `appsettings` resurser (söka efter `"name": "appsettings"`).</span><span class="sxs-lookup"><span data-stu-id="40839-237">Open *&lt;repository_root>*\ARMTemplates\ProdAndStagetest.json and find the `appsettings` resources (search for `"name": "appsettings"`).</span></span> <span data-ttu-id="40839-238">Det finns 4 i dem, ett för varje plats.</span><span class="sxs-lookup"><span data-stu-id="40839-238">There are 4 of them, one for each slot.</span></span>
2. <span data-ttu-id="40839-239">För varje `appsettings` resurs, lägga till en `"environment": "[parameters('slotName')]"` appinställningen till slutet av den `properties` matris.</span><span class="sxs-lookup"><span data-stu-id="40839-239">For each `appsettings` resource, add an  `"environment": "[parameters('slotName')]"` app setting to the end of the `properties` array.</span></span> <span data-ttu-id="40839-240">Glöm inte att avsluta föregående rad med kommatecken.</span><span class="sxs-lookup"><span data-stu-id="40839-240">Don't forget to end the previous line with a comma.</span></span>

    ![](./media/app-service-web-test-in-production-controlled-test-flight/06-arm-app-setting-with-slot-name.png)

    <span data-ttu-id="40839-241">Du har lagt till den `environment` appinställningen alla platser i mallen.</span><span class="sxs-lookup"><span data-stu-id="40839-241">You have just added the `environment` app setting to all the slots in the template.</span></span>
3. <span data-ttu-id="40839-242">I samma fil finns på `slotconfignames` resurser (söka efter `"name": "slotconfignames"`).</span><span class="sxs-lookup"><span data-stu-id="40839-242">In the same file, find the `slotconfignames` resources (search for `"name": "slotconfignames"`).</span></span> <span data-ttu-id="40839-243">Det finns 2 av dem, ett för varje app.</span><span class="sxs-lookup"><span data-stu-id="40839-243">There are 2 of them, one for each app.</span></span>
4. <span data-ttu-id="40839-244">För varje `slotconfignames` resurs, lägga till `"environment"` till slutet av den `appSettingNames` matris.</span><span class="sxs-lookup"><span data-stu-id="40839-244">For each `slotconfignames` resource, add `"environment"` to the end of the `appSettingNames` array.</span></span> <span data-ttu-id="40839-245">Glöm inte att avsluta föregående rad med kommatecken.</span><span class="sxs-lookup"><span data-stu-id="40839-245">Don't forget to end the previous line with a comma.</span></span>

    <span data-ttu-id="40839-246">Du har skapat den `environment` app anger minne till sina respektive distributionsplatsen för både appar.</span><span class="sxs-lookup"><span data-stu-id="40839-246">You have just made the `environment` app setting stick to its respective deployment slot for both apps.</span></span>  
5. <span data-ttu-id="40839-247">Kör följande kommandon i sessionen Git-gränssnittet med samma resurs grupp-suffix som du använde tidigare.</span><span class="sxs-lookup"><span data-stu-id="40839-247">In your Git Shell session, run the following commands with the same resource group suffix that you used before.</span></span>

        git checkout -b beta
        git push origin beta
        .\deploy.ps1 -RepoUrl https://github.com/<your_fork>/ToDoApp.git -ResourceGroupSuffix <your_suffix> -SlotName beta -Branch beta
6. <span data-ttu-id="40839-248">När du uppmanas, anger du samma SQL-databas autentiseringsuppgifterna som innan.</span><span class="sxs-lookup"><span data-stu-id="40839-248">When prompted, specify the same SQL database credentials as before.</span></span> <span data-ttu-id="40839-249">När du tillfrågas om du vill uppdatera resursgruppen, Skriv `Y`, sedan `ENTER`.</span><span class="sxs-lookup"><span data-stu-id="40839-249">Then, when asked to update the resource group, type `Y`, then `ENTER`.</span></span>

    <span data-ttu-id="40839-250">När skriptet har slutförts, alla resurser i den ursprungliga resursgruppen finns kvar, men en ny plats med namnet ”betaversionen” har skapats i den med samma konfiguration som ”mellanlagring” facket som skapades i början.</span><span class="sxs-lookup"><span data-stu-id="40839-250">Once the script finishes, all your resources in the original resource group are retained, but a new slot named "beta" is created in it with the same configuration as the "Staging" slot that was created in the beginning.</span></span>

   > [!NOTE]
   > <span data-ttu-id="40839-251">Den här metoden för att skapa av olika distributionsmiljöer skiljer sig från metoden i [flexibel programvaruutveckling med Azure App Service](app-service-agile-software-development.md).</span><span class="sxs-lookup"><span data-stu-id="40839-251">This method of creating different deployment environments is different from the method in [Agile software development with Azure App Service](app-service-agile-software-development.md).</span></span> <span data-ttu-id="40839-252">Här kan skapa du distribution av med distributionsplatser där som det du skapar distributionsmiljöer med resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="40839-252">Here, you create deployment environments with deployment slots, where as there you create deployment environments with resource groups.</span></span> <span data-ttu-id="40839-253">Hantera distribution av med resursgrupper kan du behålla produktionsmiljön off-limits för utvecklare, men det är inte lätt att göra test i produktion, vilket du kan göra enkelt med platser.</span><span class="sxs-lookup"><span data-stu-id="40839-253">Managing deployment environments with resource groups enables you to keep the production environment off-limits to developers, but it's not easy to do testing in production, which you can do easily with slots.</span></span>
   >
   >

<span data-ttu-id="40839-254">Om du vill kan skapa du en alfanumeriska app genom att köra</span><span class="sxs-lookup"><span data-stu-id="40839-254">If you wish, you can also create an alpha app by running</span></span>

    git checkout -b alpha
    git push origin alpha
    .\deploy.ps1 -RepoUrl https://github.com/<your_fork>/ToDoApp.git -ResourceGroupSuffix <your_suffix> -SlotName beta -Branch alpha

<span data-ttu-id="40839-255">För den här självstudiekursen kommer du bara fortsätta använda appen beta.</span><span class="sxs-lookup"><span data-stu-id="40839-255">For this tutorial, you will just keep using your beta app.</span></span>

## <a name="update-push-your-updates-to-the-beta-app"></a><span data-ttu-id="40839-256">Uppdatering: Överför din uppdateringar till appen beta</span><span class="sxs-lookup"><span data-stu-id="40839-256">Update: Push your updates to the beta app</span></span>
<span data-ttu-id="40839-257">Tillbaka till din app som du vill förbättra.</span><span class="sxs-lookup"><span data-stu-id="40839-257">Back to your app that you want to improve.</span></span>

1. <span data-ttu-id="40839-258">Kontrollera att du är nu i grenen beta</span><span class="sxs-lookup"><span data-stu-id="40839-258">Make sure you're now in your beta branch</span></span>

        git checkout beta
2. <span data-ttu-id="40839-259">I  *&lt;repository_root >*\src\MultiChannelToDo.Web\app\Index.cshtml, hitta den `<li>` tagga och Lägg till den `style="cursor:pointer"` attributet som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="40839-259">In *&lt;repository_root>*\src\MultiChannelToDo.Web\app\Index.cshtml, find the `<li>` tag and add the `style="cursor:pointer"` attribute, as shown below.</span></span>

    ![](./media/app-service-web-test-in-production-controlled-test-flight/07-change-cursor-style-on-li.png)
3. <span data-ttu-id="40839-260">genomförande och push i Azure.</span><span class="sxs-lookup"><span data-stu-id="40839-260">commit and push to Azure.</span></span>
4. <span data-ttu-id="40839-261">Kontrollera att ändringen nu visas i beta-plats genom att gå till http://todoapp*&lt;your_suffix >*-beta.azurewebsites.net/.</span><span class="sxs-lookup"><span data-stu-id="40839-261">Verify that the change is now reflected in the beta slot by navigating to http://todoapp*&lt;your_suffix>*-beta.azurewebsites.net/.</span></span> <span data-ttu-id="40839-262">Om du inte se ändringen ännu, uppdatera webbläsaren om du vill hämta den nya javascript-koden.</span><span class="sxs-lookup"><span data-stu-id="40839-262">If you don't see the change yet, refresh your browser to get the new javascript code.</span></span>

    ![](./media/app-service-web-test-in-production-controlled-test-flight/08-verify-change-in-beta-site.png)

<span data-ttu-id="40839-263">Nu när du har din ändring som körs i beta-plats är du redo att utföra en flighting distribution.</span><span class="sxs-lookup"><span data-stu-id="40839-263">Now that you have your change running in the beta slot, you are ready to perform a flighting deployment.</span></span>

## <a name="validate-route-traffic-to-the-beta-app"></a><span data-ttu-id="40839-264">Verifiera: Vidarebefordra trafik till appen beta</span><span class="sxs-lookup"><span data-stu-id="40839-264">Validate: Route traffic to the beta app</span></span>
<span data-ttu-id="40839-265">I det här avsnittet kommer du dirigerar trafik till appen beta.</span><span class="sxs-lookup"><span data-stu-id="40839-265">In this section, you will route traffic to the beta app.</span></span> <span data-ttu-id="40839-266">For sake of förstå demonstration ska du vidarebefordra en betydande del av användartrafiken till den.</span><span class="sxs-lookup"><span data-stu-id="40839-266">For sake of clarity of demonstration, you're going to route a significant portion of the user traffic to it.</span></span> <span data-ttu-id="40839-267">I verkligheten kan beror mängden trafik som du vill dirigera på dina behov.</span><span class="sxs-lookup"><span data-stu-id="40839-267">In reality, the amount of traffic you want to route will depend on your specific situation.</span></span> <span data-ttu-id="40839-268">Exempelvis om webbplatsen är i skala Microsoft.com, kanske du måste mindre än en procent av den totala trafiken för att få användbara data.</span><span class="sxs-lookup"><span data-stu-id="40839-268">For example, if your site is at the scale of microsoft.com, then you may need less than one percent of your total traffic in order to gain useful data.</span></span>

1. <span data-ttu-id="40839-269">Kör följande kommandon för att dirigera hälften av produktion trafiken till facket som beta i sessionen Git Shell:</span><span class="sxs-lookup"><span data-stu-id="40839-269">In your Git Shell session, run the following commands to route half of the production traffic to the beta slot:</span></span>

        $siteName = "ToDoApp<your suffix>"
        $rule = New-Object Microsoft.WindowsAzure.Commands.Utilities.Websites.Services.WebEntities.RampUpRule
        $rule.ActionHostName = "$siteName-beta.azurewebsites.net"
        $rule.ReroutePercentage = 50
        $rule.Name = "beta"
        Set-AzureWebsite $siteName -Slot Production -RoutingRules $rule

   <span data-ttu-id="40839-270">Den `ReroutePercentage=50` egenskapen anger 50% av produktions-trafik vidarebefordras till beta-appens URL (anges av den `ActionHostName` egenskap).</span><span class="sxs-lookup"><span data-stu-id="40839-270">The `ReroutePercentage=50` property specifies that 50% of the production traffic will be routed to the beta app's URL (specified by the `ActionHostName` property).</span></span>
2. <span data-ttu-id="40839-271">Navigera till http://ToDoApp*&lt;your_suffix >*. azurewebsites.net.</span><span class="sxs-lookup"><span data-stu-id="40839-271">Now navigate to http://ToDoApp*&lt;your_suffix>*.azurewebsites.net.</span></span> <span data-ttu-id="40839-272">50% av trafiken ska nu omdirigeras till facket som beta.</span><span class="sxs-lookup"><span data-stu-id="40839-272">50% of the traffic should now be redirected to the beta slot.</span></span>
3. <span data-ttu-id="40839-273">Filtrera mätvärdena efter miljö i Application Insights-resurs = ”betaversionen”.</span><span class="sxs-lookup"><span data-stu-id="40839-273">In your Application Insights resource, filter the metrics by environment="beta".</span></span>

   > [!NOTE]
   > <span data-ttu-id="40839-274">Om du sparar den filtrerade vyn som en annan favorit, kan du enkelt växla mått explorer vyer mellan produktion och beta vyer.</span><span class="sxs-lookup"><span data-stu-id="40839-274">If you save this filtered view as another favorite, then you can easily flip the metric explorer views between production and beta views.</span></span>
   >
   >

<span data-ttu-id="40839-275">Anta att du ser något som liknar följande i Application Insights:</span><span class="sxs-lookup"><span data-stu-id="40839-275">Suppose in Application Insights you see something similar to the following:</span></span>

![](./media/app-service-web-test-in-production-controlled-test-flight/09-test-beta-site-in-production.png)

<span data-ttu-id="40839-276">Inte bara det visar att det finns många fler klick på den `<li>` taggar, men det verkar vara en ökning i klick på `<li>` taggar.</span><span class="sxs-lookup"><span data-stu-id="40839-276">Not only is this showing that there are many more clicks on the `<li>` tags, but there seems to be a surge of clicks on `<li>` tags.</span></span> <span data-ttu-id="40839-277">Du kan sedan avslutar att personer har identifierat nya `<li>` taggar är klickbara och nu Rensa alla sina tidigare slutförts aktiviteter i appen.</span><span class="sxs-lookup"><span data-stu-id="40839-277">You can then conclude that people have discovered the new `<li>` tags are clickable and are now clearing all their previously-completed tasks in the app.</span></span>

<span data-ttu-id="40839-278">Baserat på de flighting distributionen av du bestämmer dig för att ditt nya Användargränssnittet är klar för produktion.</span><span class="sxs-lookup"><span data-stu-id="40839-278">Based on the data of your flighting deployment, you decide that your new UI is ready for production.</span></span>

## <a name="go-live-move-your-new-code-into-production"></a><span data-ttu-id="40839-279">Publicera: Flytta din nya kod till produktion</span><span class="sxs-lookup"><span data-stu-id="40839-279">Go live: Move your new code into production</span></span>
<span data-ttu-id="40839-280">Nu är du redo att flytta din uppdatering till produktionen.</span><span class="sxs-lookup"><span data-stu-id="40839-280">You're now ready to move your update to production.</span></span> <span data-ttu-id="40839-281">Vad är bra är att nu du vet att uppdateringen har redan verifierats *innan* pushas till produktionen.</span><span class="sxs-lookup"><span data-stu-id="40839-281">What's great is that now you know that your update has already been validated *before* it is pushed to production.</span></span> <span data-ttu-id="40839-282">Nu kan du vill distribuera den.</span><span class="sxs-lookup"><span data-stu-id="40839-282">So now you can confidently deploy it.</span></span> <span data-ttu-id="40839-283">Eftersom du har gjort en uppdatering i klientappen AngularJS verifieras endast koden för klientsidan.</span><span class="sxs-lookup"><span data-stu-id="40839-283">Since you made an update to the AngularJS client app, you only validated the client-side code.</span></span> <span data-ttu-id="40839-284">Om du vill göra ändringar för backend-webb-API-app, kan du validera dina ändringar på samma sätt och enkelt.</span><span class="sxs-lookup"><span data-stu-id="40839-284">If you were to make changes to the back-end Web API app, you could validate your changes similarly and easily.</span></span>

1. <span data-ttu-id="40839-285">Ta bort routning regel för nätverkstrafik genom att köra följande kommando i Git-gränssnittet:</span><span class="sxs-lookup"><span data-stu-id="40839-285">In Git Shell, remove the traffic routing rule by running the following command:</span></span>

        Set-AzureWebsite $siteName -Slot Production -RoutingRules @()
2. <span data-ttu-id="40839-286">Kör Git-kommandon:</span><span class="sxs-lookup"><span data-stu-id="40839-286">Run the Git commands:</span></span>

        git checkout master
        git pull origin master
        git merge beta
        git push origin master
3. <span data-ttu-id="40839-287">Vänta några minuter för den nya koden som ska distribueras till mellanlagringsplatsen och sedan starta http://ToDoApp*&lt;your_suffix >*-staging.azurewebsites.net för att verifiera att den nya uppdateringen varmkörts i mellanlagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="40839-287">Wait for a few minutes for the new code to be deployed to the staging slot, then launch http://ToDoApp*&lt;your_suffix>*-staging.azurewebsites.net to verify that the new update is warmed up in the staging slot.</span></span> <span data-ttu-id="40839-288">Kom ihåg att den din förgrening mastergrenen är kopplad till mellanlagringsplatsen för din app.</span><span class="sxs-lookup"><span data-stu-id="40839-288">Remember that the your fork's master branch is linked to the staging slot of your app.</span></span>
4. <span data-ttu-id="40839-289">Nu byta mellanlagringsplatsen till produktion</span><span class="sxs-lookup"><span data-stu-id="40839-289">Now, swap the staging slot into production</span></span>

        cd <ROOT>\ToDoApp\ARMTemplates
        .\swap.ps1 -Name todoapp<your_suffix>

## <a name="summary"></a><span data-ttu-id="40839-290">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="40839-290">Summary</span></span>
<span data-ttu-id="40839-291">Azure App Service gör det enkelt för små till medelstora företag att testa sina kund-riktade appar i produktion, något som traditionellt har utförts i stora företag.</span><span class="sxs-lookup"><span data-stu-id="40839-291">Azure App Service makes it easy for small- to medium-sized businesses to test their customer-facing apps in production, something that has been traditionally done in big enterprises.</span></span> <span data-ttu-id="40839-292">Den här självstudiekursen har förhoppningsvis gett dig kunskap du behöver samordnar App Service och Application Insights för att göra möjlig förhandsversionstestning distribution och även andra test i produktion-scenarier i DevOps-världen.</span><span class="sxs-lookup"><span data-stu-id="40839-292">Hopefully, this tutorial has given you the knowledge you need to bring together App Service and Application Insights to make possible flighting deployment, and even other test-in-production scenarios, in your DevOps world.</span></span>

## <a name="more-resources"></a><span data-ttu-id="40839-293">Fler resurser</span><span class="sxs-lookup"><span data-stu-id="40839-293">More resources</span></span>
* [<span data-ttu-id="40839-294">Flexibel programvaruutveckling med Azure App Service</span><span class="sxs-lookup"><span data-stu-id="40839-294">Agile software development with Azure App Service</span></span>](app-service-agile-software-development.md)
* [<span data-ttu-id="40839-295">Skapa mellanlagringsmiljöer för web apps i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="40839-295">Set up staging environments for web apps in Azure App Service</span></span>](web-sites-staged-publishing.md)
* [<span data-ttu-id="40839-296">Distribuera ett komplexa program förutsägbart i Azure</span><span class="sxs-lookup"><span data-stu-id="40839-296">Deploy a complex application predictably in Azure</span></span>](app-service-deploy-complex-application-predictably.md)
* [<span data-ttu-id="40839-297">Redigera Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="40839-297">Authoring Azure Resource Manager Templates</span></span>](../azure-resource-manager/resource-group-authoring-templates.md)
* [<span data-ttu-id="40839-298">JSONLint - JSON-verifieraren</span><span class="sxs-lookup"><span data-stu-id="40839-298">JSONLint - The JSON Validator</span></span>](http://jsonlint.com/)
* [<span data-ttu-id="40839-299">Git förgrening – grundläggande förgrening och sammanfogning</span><span class="sxs-lookup"><span data-stu-id="40839-299">Git Branching – Basic Branching and Merging</span></span>](http://www.git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
* [<span data-ttu-id="40839-300">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="40839-300">Azure PowerShell</span></span>](/powershell/azure/overview)
* [<span data-ttu-id="40839-301">Projektet Kudu-Wiki</span><span class="sxs-lookup"><span data-stu-id="40839-301">Project Kudu Wiki</span></span>](https://github.com/projectkudu/kudu/wiki)
