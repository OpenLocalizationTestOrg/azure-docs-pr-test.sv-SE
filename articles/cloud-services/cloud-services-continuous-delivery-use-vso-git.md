---
title: Kontinuerlig leverans med Git och Visual Studio Team Services i Azure | Microsoft Docs
description: "Lär dig hur du konfigurerar din Visual Studio Team Services grupprojekt att använda Git för att automatiskt skapa och distribuera till funktionen Web App i Azure App Service eller cloud services."
services: cloud-services
documentationcenter: .net
author: mlearned
manager: douge
editor: 
ms.assetid: 4b3297ef-0de6-4d5f-925c-fcdacc3085ac
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/06/2016
ms.author: mlearned
ms.openlocfilehash: f4f5f231536bc381d17898ff2c592be821168a65
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="continuous-delivery-to-azure-using-visual-studio-team-services-and-git"></a><span data-ttu-id="d3818-103">Kontinuerlig leverans till Azure med Visual Studio Team Services och Git</span><span class="sxs-lookup"><span data-stu-id="d3818-103">Continuous delivery to Azure using Visual Studio Team Services and Git</span></span>
<span data-ttu-id="d3818-104">Du kan använda Visual Studio Team Services grupprojekt att vara värd för en Git-lagringsplats för din källkod och automatiskt skapa och distribuera till Azure-webbappar eller molntjänster när du skickar ett genomförande till lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="d3818-104">You can use Visual Studio Team Services team projects to host a Git repository for your source code, and automatically build and deploy to Azure web apps or cloud services whenever you push a commit to the repository.</span></span>

<span data-ttu-id="d3818-105">Du behöver Visual Studio 2013 och Azure SDK om du installerade.</span><span class="sxs-lookup"><span data-stu-id="d3818-105">You'll need Visual Studio 2013 and the Azure SDK installed.</span></span> <span data-ttu-id="d3818-106">Om du inte redan har Visual Studio 2013 kan du hämta det genom att välja den **Kom igång gratis** länka när [www.visualstudio.com](http://www.visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="d3818-106">If you don't already have Visual Studio 2013, download it by choosing the **Get started for free** link at [www.visualstudio.com](http://www.visualstudio.com).</span></span> <span data-ttu-id="d3818-107">Installera Azure SDK från [här](http://go.microsoft.com/fwlink/?LinkId=239540).</span><span class="sxs-lookup"><span data-stu-id="d3818-107">Install the Azure SDK from [here](http://go.microsoft.com/fwlink/?LinkId=239540).</span></span>

> [!NOTE]
> <span data-ttu-id="d3818-108">Du behöver ett Visual Studio Team Services-konto för att slutföra den här självstudiekursen: du kan [öppnar ett Visual Studio Team Services-konto gratis](http://go.microsoft.com/fwlink/p/?LinkId=512979).</span><span class="sxs-lookup"><span data-stu-id="d3818-108">You need an Visual Studio Team Services account to complete this tutorial: You can [open a Visual Studio Team Services account for free](http://go.microsoft.com/fwlink/p/?LinkId=512979).</span></span>
> 
> 

<span data-ttu-id="d3818-109">Följ dessa steg om du vill konfigurera en molntjänst för att automatiskt skapa och distribuera till Azure med hjälp av Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="d3818-109">To set up a cloud service to automatically build and deploy to Azure by using Visual Studio Team Services, follow these steps.</span></span>

## <a name="1-create-a-git-repository"></a><span data-ttu-id="d3818-110">1: skapa en Git-lagringsplats</span><span class="sxs-lookup"><span data-stu-id="d3818-110">1: Create a Git repository</span></span>
1. <span data-ttu-id="d3818-111">Om du inte redan har en Visual Studio Team Services-konto kan du skaffa ett [här](http://go.microsoft.com/fwlink/?LinkId=397665).</span><span class="sxs-lookup"><span data-stu-id="d3818-111">If you don’t already have a Visual Studio Team Services account, you can get one  [here](http://go.microsoft.com/fwlink/?LinkId=397665).</span></span> <span data-ttu-id="d3818-112">När du skapar ditt team projekt, Välj Git som ditt källkontrollsystem.</span><span class="sxs-lookup"><span data-stu-id="d3818-112">When you create your team project, choose Git as your source control system.</span></span> <span data-ttu-id="d3818-113">Följ instruktionerna för att ansluta Visual Studio till ditt team projekt.</span><span class="sxs-lookup"><span data-stu-id="d3818-113">Follow the instructions to connect Visual Studio to your team project.</span></span>
2. <span data-ttu-id="d3818-114">I **Team Explorer**, Välj den **klona lagringsplatsen** länk.</span><span class="sxs-lookup"><span data-stu-id="d3818-114">In **Team Explorer**, choose the **Clone this repository** link.</span></span>
   
    ![][3]
3. <span data-ttu-id="d3818-115">Ange platsen för den lokala kopian och välj sedan den **klona** knappen.</span><span class="sxs-lookup"><span data-stu-id="d3818-115">Specify the location of the local copy and then choose the **Clone** button.</span></span>

## <a name="2-create-a-project-and-commit-it-to-the-repository"></a><span data-ttu-id="d3818-116">2: skapa ett projekt och överför den till databasen</span><span class="sxs-lookup"><span data-stu-id="d3818-116">2: Create a project and commit it to the repository</span></span>
1. <span data-ttu-id="d3818-117">I **Team Explorer**i den **lösningar** väljer du den **ny** länken för att skapa ett nytt projekt i lokal databas.</span><span class="sxs-lookup"><span data-stu-id="d3818-117">In **Team Explorer**, in the **Solutions** section, choose the **New** link to create a new project in the local repository.</span></span>
   
    ![][4]
2. <span data-ttu-id="d3818-118">Du kan distribuera en webbapp eller en tjänst i molnet (Azure-program) genom att följa stegen i den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="d3818-118">You can deploy a web app or a cloud service (Azure Application) by following the steps in this walkthrough.</span></span> <span data-ttu-id="d3818-119">Skapa ett nytt Azure Cloud Service-projekt, eller ett nytt ASP.NET MVC-projekt.</span><span class="sxs-lookup"><span data-stu-id="d3818-119">Create a new Azure Cloud Service project, or a new ASP.NET MVC project.</span></span> <span data-ttu-id="d3818-120">Se till att projektet riktar sig till .NET Framework 4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="d3818-120">Make sure that the project targets the .NET Framework 4 or later.</span></span> <span data-ttu-id="d3818-121">Om du skapar ett molntjänstprojekt kan du lägga till en ASP.NET MVC-webbroll och en arbetsroll.</span><span class="sxs-lookup"><span data-stu-id="d3818-121">If you are creating a cloud service project, add an ASP.NET MVC web role and a worker role.</span></span>
   <span data-ttu-id="d3818-122">Om du vill skapa en webbapp, välja den **ASP.NET-webbprogram** projektmall och välj sedan **MVC**.</span><span class="sxs-lookup"><span data-stu-id="d3818-122">If you want to create a web app, choose the **ASP.NET Web Application** project template, and then choose **MVC**.</span></span> <span data-ttu-id="d3818-123">Se [skapa en ASP.NET-webbapp i Azure App Service](../app-service-web/app-service-web-get-started-dotnet.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="d3818-123">See [Create an ASP.NET web app in Azure App Service](../app-service-web/app-service-web-get-started-dotnet.md) for more information.</span></span>
3. <span data-ttu-id="d3818-124">Öppna snabbmenyn för lösningen och välj **genomför**.</span><span class="sxs-lookup"><span data-stu-id="d3818-124">Open the shortcut menu for the solution, and choose **Commit**.</span></span>
   
    ![][7]
4. <span data-ttu-id="d3818-125">Om du har använt Git i Visual Studio Team Services måste du ange information för att identifiera dig i Git.</span><span class="sxs-lookup"><span data-stu-id="d3818-125">If this is the first time you've used Git in Visual Studio Team Services, you'll need to provide some information to identify yourself in Git.</span></span> <span data-ttu-id="d3818-126">I den **väntande ändringar** område i **Team Explorer**, ange ditt användarnamn och e-postadress.</span><span class="sxs-lookup"><span data-stu-id="d3818-126">In the **Pending Changes** area of **Team Explorer**, enter your username and email address.</span></span> <span data-ttu-id="d3818-127">Skriv en kommentar för genomförandet och välj sedan den **Commit** knappen.</span><span class="sxs-lookup"><span data-stu-id="d3818-127">Enter a comment for the commit and then choose the **Commit** button.</span></span>
   
    ![][8]
5. <span data-ttu-id="d3818-128">Observera de alternativ som ska tas med eller undanta specifika ändringar när du checkar in.</span><span class="sxs-lookup"><span data-stu-id="d3818-128">Note the options to include or exclude specific changes when you check in.</span></span> <span data-ttu-id="d3818-129">Om ändringarna är undantagna väljer **inkluderar alla**.</span><span class="sxs-lookup"><span data-stu-id="d3818-129">If the changes you want are excluded, choose **Include All**.</span></span>
6. <span data-ttu-id="d3818-130">Du har nu genomfört ändringarna i den lokala kopian av databasen.</span><span class="sxs-lookup"><span data-stu-id="d3818-130">You've now committed the changes in your local copy of the repository.</span></span> <span data-ttu-id="d3818-131">Sedan synkronisera ändringarna med servern genom att välja den **Sync** länk.</span><span class="sxs-lookup"><span data-stu-id="d3818-131">Next, sync those changes with the server by choosing the **Sync** link.</span></span>

## <a name="3-connect-the-project-to-azure"></a><span data-ttu-id="d3818-132">3: Anslut projektet till Azure</span><span class="sxs-lookup"><span data-stu-id="d3818-132">3: Connect the project to Azure</span></span>
1. <span data-ttu-id="d3818-133">Nu när du har en Git-lagringsplats i Visual Studio Team Services med vissa källkod i den är du redo att ansluta git-lagringsplatsen till Azure.</span><span class="sxs-lookup"><span data-stu-id="d3818-133">Now that you have a Git repository in Visual Studio Team Services with some source code in it, you are ready to connect your git repository to Azure.</span></span>  <span data-ttu-id="d3818-134">I den [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885), Välj din cloud service eller web app eller skapa en ny genom att välja den + -ikonen längst ned vänster och välja **Molntjänsten** eller **Web App** och sedan **Snabbregistrering**.</span><span class="sxs-lookup"><span data-stu-id="d3818-134">In the [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885), select your cloud service or web app, or create a new one by choosing the + icon at the bottom left and choosing **Cloud Service** or **Web App** and then **Quick Create**.</span></span>
   
    ![][9]
2. <span data-ttu-id="d3818-135">Molntjänster, Välj den **Konfigurera publicering med Visual Studio Team Services** länk.</span><span class="sxs-lookup"><span data-stu-id="d3818-135">For cloud services, choose the **Set up publishing with Visual Studio Team Services** link.</span></span> <span data-ttu-id="d3818-136">Webbappar, Välj den **konfigurera distribution från källkontroll** länk.</span><span class="sxs-lookup"><span data-stu-id="d3818-136">For web apps, choose the **Set up deployment from source control** link.</span></span>
   
    ![][10]
3. <span data-ttu-id="d3818-137">I guiden anger du namnet på ditt konto i Visual Studio Team Services i textrutan och välj den **auktorisera nu** länk.</span><span class="sxs-lookup"><span data-stu-id="d3818-137">In the wizard, type the name of your Visual Studio Team Services account in the textbox and choose the **Authorize Now** link.</span></span> <span data-ttu-id="d3818-138">Du kan uppmanas att logga in.</span><span class="sxs-lookup"><span data-stu-id="d3818-138">You might be asked to sign in.</span></span>
   
    ![][11]
4. <span data-ttu-id="d3818-139">I den **anslutningsbegäran** popup-fönstret, Välj **acceptera** att auktorisera Azure konfigurera ditt team projekt i Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="d3818-139">In the **Connection Request** pop-up dialog, choose **Accept** to authorize Azure to configure your team project in Visual Studio Team Services.</span></span>
   
    ![][12]
5. <span data-ttu-id="d3818-140">När tillståndet lyckas visas en listruta som innehåller din Visual Studio Team Services grupprojekt.</span><span class="sxs-lookup"><span data-stu-id="d3818-140">After authorization succeeds, you see a dropdown list that contains your Visual Studio Team Services team projects.</span></span>  <span data-ttu-id="d3818-141">Välj namnet på grupprojekt som du skapade i föregående steg och välj guidens markering.</span><span class="sxs-lookup"><span data-stu-id="d3818-141">Select the name of team project that you created in the previous steps, and choose the wizard's checkmark button.</span></span>
   
    ![][13]
   
    <span data-ttu-id="d3818-142">Nästa gång du skicka ett genomförande till lagringsplatsen, Visual Studio Team Services skapar och distribuera projektet till Azure.</span><span class="sxs-lookup"><span data-stu-id="d3818-142">The next time you push a commit to your repository, Visual Studio Team Services will build and deploy your project to Azure.</span></span>

## <a name="4-trigger-a-rebuild-and-redeploy-your-project"></a><span data-ttu-id="d3818-143">4: utlösa en återskapning och omdistribuera ditt projekt</span><span class="sxs-lookup"><span data-stu-id="d3818-143">4: Trigger a rebuild and redeploy your project</span></span>
1. <span data-ttu-id="d3818-144">Öppna en fil och ändra den i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d3818-144">In Visual Studio, open up a file and change it.</span></span> <span data-ttu-id="d3818-145">Till exempel ändra filen `_Layout.cshtml` under vyerna\\delad mapp i en MVC-webbroll.</span><span class="sxs-lookup"><span data-stu-id="d3818-145">For example, change the file `_Layout.cshtml` under the Views\\Shared folder in an MVC web role.</span></span>
   
    ![][17]
2. <span data-ttu-id="d3818-146">Redigera sidfotstexten för platsen och spara filen.</span><span class="sxs-lookup"><span data-stu-id="d3818-146">Edit the footer text for the site and save the file.</span></span>
   
    ![][18]
3. <span data-ttu-id="d3818-147">I **Solution Explorer**, öppna snabbmenyn för noden lösning, projektnoden eller filen du ändras och välj sedan **genomför**.</span><span class="sxs-lookup"><span data-stu-id="d3818-147">In **Solution Explorer**, open the shortcut menu for the solution node, project node, or the file you changed, and then choose **Commit**.</span></span>
4. <span data-ttu-id="d3818-148">Ange en kommentar och välj **genomför**.</span><span class="sxs-lookup"><span data-stu-id="d3818-148">Type in a comment and choose **Commit**.</span></span>
   
    ![][20]
5. <span data-ttu-id="d3818-149">Välj den **Sync** länk.</span><span class="sxs-lookup"><span data-stu-id="d3818-149">Choose the **Sync** link.</span></span>
   
    ![][38]
6. <span data-ttu-id="d3818-150">Välj den **Push** länk trycka på din genomförande till lagringsplatsen i Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="d3818-150">Choose the **Push** link to push your commit to the repository in Visual Studio Team Services.</span></span> <span data-ttu-id="d3818-151">(Du kan också använda den **Sync** för att kopiera ditt incheckningar till databasen.</span><span class="sxs-lookup"><span data-stu-id="d3818-151">(You can also use the **Sync** button to copy your commits to the repository.</span></span> <span data-ttu-id="d3818-152">Skillnaden är att **Sync** också tar emot de senaste ändringarna från databasen.)</span><span class="sxs-lookup"><span data-stu-id="d3818-152">The difference is that **Sync** also pulls the latest changes from the repository.)</span></span>
   
    ![][39]
7. <span data-ttu-id="d3818-153">Välj den **hem** vill gå tillbaka till den **Team Explorer** startsidan.</span><span class="sxs-lookup"><span data-stu-id="d3818-153">Choose the **Home** button to return to the **Team Explorer** home page.</span></span>
   
    ![][21]
8. <span data-ttu-id="d3818-154">Välj **bygger** att visa versionerna pågår.</span><span class="sxs-lookup"><span data-stu-id="d3818-154">Choose **Builds** to view the builds in progress.</span></span>
   
    ![][22]
   
    <span data-ttu-id="d3818-155">**Teamet Explorer** visar att en version har utlösts för din incheckning.</span><span class="sxs-lookup"><span data-stu-id="d3818-155">**Team Explorer** shows that a build has been triggered for your check-in.</span></span>
   
    ![][23]
9. <span data-ttu-id="d3818-156">Om du vill visa detaljerade loggfil när build fortlöper, dubbelklicka på namnet på build pågår.</span><span class="sxs-lookup"><span data-stu-id="d3818-156">To view a detailed log as the build progresses, double-click the name of the build in progress.</span></span>
10. <span data-ttu-id="d3818-157">Versionen är pågående, ta en titt på build-definition som skapades när du använde guiden för att länka till Azure.</span><span class="sxs-lookup"><span data-stu-id="d3818-157">While the build is in-progress, take a look at the build definition that was created when you used the wizard to link to Azure.</span></span>  <span data-ttu-id="d3818-158">Öppna snabbmenyn för build-definition och välj **redigera skapa Definition**.</span><span class="sxs-lookup"><span data-stu-id="d3818-158">Open the shortcut menu for the build definition and choose **Edit Build Definition**.</span></span>
    
    ![][25]
11. <span data-ttu-id="d3818-159">I den **utlösaren** fliken visas build-definition anges att bygga på varje incheckning, som standard.</span><span class="sxs-lookup"><span data-stu-id="d3818-159">In the **Trigger** tab, you will see that the build definition is set to build on every check-in, by default.</span></span> <span data-ttu-id="d3818-160">(För en molnbaserad tjänst bör Visual Studio Team Services skapar och distribuerar mastergrenen till mellanlagringsmiljön automatiskt.</span><span class="sxs-lookup"><span data-stu-id="d3818-160">(For a cloud service, Visual Studio Team Services builds and deploys the master branch to the staging environment automatically.</span></span> <span data-ttu-id="d3818-161">Du måste göra ett manuellt steg att distribuera liveplatsen.</span><span class="sxs-lookup"><span data-stu-id="d3818-161">You still have to do a manual step to deploy to the live site.</span></span> <span data-ttu-id="d3818-162">För en webbapp som inte har mellanlagring miljö distribuerar mastergrenen direkt till webbplatsen live.</span><span class="sxs-lookup"><span data-stu-id="d3818-162">For a web app that doesn't have staging environment, it deploys the master branch directly to the live site.</span></span>
    
    ![][26]
12. <span data-ttu-id="d3818-163">I den **processen** fliken visas distributionsmiljö har angetts till namnet på din cloud service eller web app.</span><span class="sxs-lookup"><span data-stu-id="d3818-163">In the **Process** tab, you can see the deployment environment is set to the name of your cloud service or web app.</span></span>
    
     ![][27]
13. <span data-ttu-id="d3818-164">Ange värden för egenskaper om du vill att andra värden än standardinställningarna.</span><span class="sxs-lookup"><span data-stu-id="d3818-164">Specify values for the properties if you want different values than the defaults.</span></span> <span data-ttu-id="d3818-165">Egenskaperna för Azure-publicering finns i den **distribution** avsnitt, och du kan också behöva ange MSBuild-parametrar.</span><span class="sxs-lookup"><span data-stu-id="d3818-165">The properties for Azure publishing are in the **Deployment** section, and you might also need to set MSBuild parameters.</span></span> <span data-ttu-id="d3818-166">Till exempel i ett moln service-projekt att ange en tjänstkonfiguration än ”moln” ange MSbuild parametrarna för `/p:TargetProfile=[YourProfile]` där *[YourProfile]* matchar en tjänstkonfigurationsfil med namnet ServiceConfiguration. *YourProfile*.cscfg.</span><span class="sxs-lookup"><span data-stu-id="d3818-166">For example, in a cloud service project, to specify a service configuration other than "Cloud", set the MSbuild parameters to `/p:TargetProfile=[YourProfile]` where *[YourProfile]* matches a service configuration file with a name like ServiceConfiguration.*YourProfile*.cscfg.</span></span>
    
     <span data-ttu-id="d3818-167">I följande tabell visas de tillgängliga egenskaperna i den **distribution** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="d3818-167">The following table shows the available properties in the **Deployment** section:</span></span>
    
    | <span data-ttu-id="d3818-168">Egenskap</span><span class="sxs-lookup"><span data-stu-id="d3818-168">Property</span></span> | <span data-ttu-id="d3818-169">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="d3818-169">Default Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="d3818-170">Tillåt ej betrodda certifikat</span><span class="sxs-lookup"><span data-stu-id="d3818-170">Allow Untrusted Certificates</span></span> |<span data-ttu-id="d3818-171">Om värdet är false måste SSL-certifikat signeras av en rotcertifikatutfärdare.</span><span class="sxs-lookup"><span data-stu-id="d3818-171">If false, SSL certificates must be signed by a root authority.</span></span> |
    | <span data-ttu-id="d3818-172">Tillåt uppgradering</span><span class="sxs-lookup"><span data-stu-id="d3818-172">Allow Upgrade</span></span> |<span data-ttu-id="d3818-173">Gör att distributionen ska uppdatera en befintlig distribution i stället för att skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="d3818-173">Allows the deployment to update an existing deployment instead of creating a new one.</span></span> <span data-ttu-id="d3818-174">Bevarar den IP-adressen.</span><span class="sxs-lookup"><span data-stu-id="d3818-174">Preserves the IP address.</span></span> |
    | <span data-ttu-id="d3818-175">Ta inte bort</span><span class="sxs-lookup"><span data-stu-id="d3818-175">Do Not Delete</span></span> |<span data-ttu-id="d3818-176">Om värdet är true, Skriv inte över en befintlig orelaterade distribution (uppgraderingen tillåts).</span><span class="sxs-lookup"><span data-stu-id="d3818-176">If true, do not overwrite an existing unrelated deployment (upgrade is allowed).</span></span> |
    | <span data-ttu-id="d3818-177">Sökvägen till distributionsinställningar</span><span class="sxs-lookup"><span data-stu-id="d3818-177">Path to Deployment Settings</span></span> |<span data-ttu-id="d3818-178">Sökvägen till filen .pubxml för en webbapp i förhållande till rotmappen på lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="d3818-178">The path to your .pubxml file for a web app, relative to the root folder of the repo.</span></span> <span data-ttu-id="d3818-179">Ignoreras för molntjänster.</span><span class="sxs-lookup"><span data-stu-id="d3818-179">Ignored for cloud services.</span></span> |
    | <span data-ttu-id="d3818-180">SharePoint-miljön för distribution</span><span class="sxs-lookup"><span data-stu-id="d3818-180">Sharepoint Deployment Environment</span></span> |<span data-ttu-id="d3818-181">Samma som namnet på tjänsten.</span><span class="sxs-lookup"><span data-stu-id="d3818-181">The same as the service name.</span></span> |
    | <span data-ttu-id="d3818-182">Av Azure-distributionsmiljö</span><span class="sxs-lookup"><span data-stu-id="d3818-182">Azure Deployment Environment</span></span> |<span data-ttu-id="d3818-183">Web app eller molnet tjänstnamnet.</span><span class="sxs-lookup"><span data-stu-id="d3818-183">The web app or cloud service name.</span></span> |
14. <span data-ttu-id="d3818-184">Vid den tidpunkten ska din build slutföras.</span><span class="sxs-lookup"><span data-stu-id="d3818-184">By this time, your build should be completed successfully.</span></span>
    
     ![][28]
15. <span data-ttu-id="d3818-185">Om du dubbelklickar på build-namnet, Visual Studio visas en **skapa sammanfattning**, inklusive alla resultat från associerade testprojekt.</span><span class="sxs-lookup"><span data-stu-id="d3818-185">If you double-click the build name, Visual Studio shows a **Build Summary**, including any test results from associated unit test projects.</span></span>
    
     ![][29]
16. <span data-ttu-id="d3818-186">I den [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885), du kan visa den associerade distributionen på de **distributioner** fliken när mellanlagringsmiljön är markerad.</span><span class="sxs-lookup"><span data-stu-id="d3818-186">In the [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885), you can view the associated deployment on the **Deployments** tab when the staging environment is selected.</span></span>
    
     ![][30]
17. <span data-ttu-id="d3818-187">Gå till webbplatsens URL.</span><span class="sxs-lookup"><span data-stu-id="d3818-187">Browse to your site's URL.</span></span> <span data-ttu-id="d3818-188">Välj bara för en webbapp i **Bläddra** knappen i portalen.</span><span class="sxs-lookup"><span data-stu-id="d3818-188">For a web app, just choose  the **Browse** button in the portal.</span></span> <span data-ttu-id="d3818-189">För en tjänst i molnet, väljer du URL-Adressen i den **snabb i korthet** avsnitt i den **instrumentpanelen** sidan som visar mellanlagringsmiljön.</span><span class="sxs-lookup"><span data-stu-id="d3818-189">For a cloud service, choose the URL in the **Quick Glance** section of the **Dashboard** page that shows the Staging environment.</span></span>
    
    <span data-ttu-id="d3818-190">Distributioner från kontinuerlig integration för molntjänster publiceras till mellanlagringsmiljön som standard.</span><span class="sxs-lookup"><span data-stu-id="d3818-190">Deployments from continuous integration for cloud services are published to the Staging environment by default.</span></span> <span data-ttu-id="d3818-191">Du kan ändra detta genom att ange den **alternativa Molntjänstmiljö** egenskapen **produktion**.</span><span class="sxs-lookup"><span data-stu-id="d3818-191">You can change this by setting the **Alternate Cloud Service Environment** property to **Production**.</span></span> <span data-ttu-id="d3818-192">Här är där webbplatsens URL på den molntjänst instrumentpanelssida.</span><span class="sxs-lookup"><span data-stu-id="d3818-192">Here's where the site URL is on the cloud service's dashboard page.</span></span>
    
    ![][31]
    
    <span data-ttu-id="d3818-193">En ny webbläsarflik öppnas för att visa webbplatsen körs.</span><span class="sxs-lookup"><span data-stu-id="d3818-193">A new browser tab will open to reveal your running site.</span></span>
    
    ![][32]
18. <span data-ttu-id="d3818-194">Om du gör andra ändringar i ditt projekt bygger mer du utlösare och du samlar flera distributioner.</span><span class="sxs-lookup"><span data-stu-id="d3818-194">If you make other changes to your project, you trigger more builds, and you will accumulate multiple deployments.</span></span> <span data-ttu-id="d3818-195">Den senaste som har markerats som aktiv.</span><span class="sxs-lookup"><span data-stu-id="d3818-195">The latest one is marked as Active.</span></span>
    
    ![][33]

## <a name="5-redeploy-an-earlier-build"></a><span data-ttu-id="d3818-196">5: Distribuera om en tidigare version</span><span class="sxs-lookup"><span data-stu-id="d3818-196">5: Redeploy an earlier build</span></span>
<span data-ttu-id="d3818-197">Det här steget är valfritt.</span><span class="sxs-lookup"><span data-stu-id="d3818-197">This step is optional.</span></span> <span data-ttu-id="d3818-198">Välj en tidigare distribution i den klassiska Azure-portalen och väljer **omdistribuera** till spola platsen till en tidigare incheckning.</span><span class="sxs-lookup"><span data-stu-id="d3818-198">In the Azure classic portal, choose an earlier deployment and choose **Redeploy** to rewind your site to an earlier check-in.</span></span> <span data-ttu-id="d3818-199">Observera att detta utlöser en ny version i TFS och skapa en ny post i distributionshistoriken.</span><span class="sxs-lookup"><span data-stu-id="d3818-199">Note that this will trigger a new build in TFS and create a new entry in your deployment history.</span></span>

![][34]

## <a name="6-change-the-production-deployment"></a><span data-ttu-id="d3818-200">6: ändra produktionsdistributionen</span><span class="sxs-lookup"><span data-stu-id="d3818-200">6: Change the Production deployment</span></span>
<span data-ttu-id="d3818-201">När du är klar kan du befordra mellanlagringsmiljön till produktionsmiljön genom att välja **växla** i den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="d3818-201">When you are ready, you can promote the Staging environment to the Production environment by choosing **Swap** in the Azure classic portal.</span></span> <span data-ttu-id="d3818-202">Nyligen distribuerade mellanlagringsmiljön höjs till produktionen och föregående produktionsmiljön, eventuella blir en mellanlagringsmiljön.</span><span class="sxs-lookup"><span data-stu-id="d3818-202">The newly deployed Staging environment is promoted to Production, and the previous Production environment, if any, becomes a Staging environment.</span></span> <span data-ttu-id="d3818-203">Aktiv distribution kan vara olika för produktions- och Mellanlagringsmiljöer, men distributionshistoriken för senare versioner är detsamma oavsett miljö.</span><span class="sxs-lookup"><span data-stu-id="d3818-203">The Active deployment may be different for the Production and Staging environments, but the deployment history of recent builds is the same regardless of environment.</span></span>

![][35]

## <a name="7-deploy-from-a-working-branch"></a><span data-ttu-id="d3818-204">7: Distribuera från en arbetsgren.</span><span class="sxs-lookup"><span data-stu-id="d3818-204">7: Deploy from a working branch.</span></span>
<span data-ttu-id="d3818-205">När du använder Git du vanligtvis göra ändringar i en arbetsgren och integrera i mastergrenen när du utvecklar når slutfört tillstånd.</span><span class="sxs-lookup"><span data-stu-id="d3818-205">When you use Git, you usually make changes in a working branch and integrate into the master branch when your development reaches a finished state.</span></span> <span data-ttu-id="d3818-206">Under utvecklingsfasen av ett projekt ska du vill skapa och distribuera arbetsgren till Azure.</span><span class="sxs-lookup"><span data-stu-id="d3818-206">During the development phase of a project, you'll want to build and deploy the working branch to Azure.</span></span>

1. <span data-ttu-id="d3818-207">I **Team Explorer**, Välj den **Start** knappen och välj sedan den **filialer** knappen.</span><span class="sxs-lookup"><span data-stu-id="d3818-207">In **Team Explorer**, choose the **Home** button and then choose the **Branches** button.</span></span>
   
    ![][40]
2. <span data-ttu-id="d3818-208">Välj den **ny gren** länk.</span><span class="sxs-lookup"><span data-stu-id="d3818-208">Choose the **New Branch** link.</span></span>
   
    ![][41]
3. <span data-ttu-id="d3818-209">Ange namnet på grenen, till exempel ”arbetar” och välj **skapa gren**.</span><span class="sxs-lookup"><span data-stu-id="d3818-209">Enter the name of the branch, such as "working," and choose **Create Branch**.</span></span> <span data-ttu-id="d3818-210">Detta skapar en ny lokal gren.</span><span class="sxs-lookup"><span data-stu-id="d3818-210">This creates a new local branch.</span></span>
   
    ![][42]
4. <span data-ttu-id="d3818-211">Publicera grenen.</span><span class="sxs-lookup"><span data-stu-id="d3818-211">Publish the branch.</span></span> <span data-ttu-id="d3818-212">Välj filialen namn i **Opublicerat filialer**, och välj **publicera**.</span><span class="sxs-lookup"><span data-stu-id="d3818-212">Choose the branch name in **Unpublished branches**, and choose **Publish**.</span></span>
   
    ![][44]
5. <span data-ttu-id="d3818-213">Som standard utlösa endast ändringar till mastergrenen en kontinuerlig version.</span><span class="sxs-lookup"><span data-stu-id="d3818-213">By default, only changes to the master branch trigger a continuous build.</span></span> <span data-ttu-id="d3818-214">Ställ in kontinuerlig build för en arbetsgren genom att välja den **bygger** sidan i **Team Explorer**, och välj **redigera skapa Definition**.</span><span class="sxs-lookup"><span data-stu-id="d3818-214">To set up continuous build for a working branch, choose the **Builds** page in **Team Explorer**, and choose **Edit Build Definition**.</span></span>
6. <span data-ttu-id="d3818-215">Öppna den **inställningar för datakälla** fliken.</span><span class="sxs-lookup"><span data-stu-id="d3818-215">Open the **Source Settings** tab.</span></span> <span data-ttu-id="d3818-216">Under **övervakas grenar för kontinuerlig integrering och bygga**, Välj **Klicka här om du vill lägga till en ny rad**.</span><span class="sxs-lookup"><span data-stu-id="d3818-216">Under **Monitored branches for continuous integration and build**, choose **Click here to add a new row**.</span></span>
   
    ![][47]
7. <span data-ttu-id="d3818-217">Ange grenen som du skapat, till exempel refs, huvuden/fungerande.</span><span class="sxs-lookup"><span data-stu-id="d3818-217">Specify the branch you created, such as refs/heads/working.</span></span>
   
    ![][48]
8. <span data-ttu-id="d3818-218">Gör en ändring i koden, öppna snabbmenyn för den ändrade filen och välj sedan **genomför**.</span><span class="sxs-lookup"><span data-stu-id="d3818-218">Make a change in the code, open the shortcut menu for the changed file, and then choose **Commit**.</span></span>
   
    ![][43]
9. <span data-ttu-id="d3818-219">Välj den **osynkroniserade incheckningar** länka och välj den **Sync** knappen eller **Push** länken för att kopiera ändringarna till kopian av arbetsgren i Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="d3818-219">Choose the **Unsynced Commits** link, and choose  the **Sync** button or the **Push** link to copy the changes to the copy of the working branch in Visual Studio Team Services.</span></span>
   
   ![][45]
10. <span data-ttu-id="d3818-220">Navigera till den **bygger** visa och Sök efter den version som precis har utlösts för arbetsgren.</span><span class="sxs-lookup"><span data-stu-id="d3818-220">Navigate to the **Builds** view and find the build that just got triggered for the working branch.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d3818-221">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d3818-221">Next steps</span></span>
<span data-ttu-id="d3818-222">Läs mer tips om att använda Git med Visual Studio Team Services i [utveckla och dela din kod i Git med Visual Studio](https://www.visualstudio.com/en-us/docs/git/share-your-code-in-git-vs-2017) och för information om hur du använder en Git-lagringsplats som inte hanteras av Visual Studio Team Services att publicera till Azure, se [kontinuerlig distribution till Azure App Service](../app-service-web/app-service-continuous-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="d3818-222">To learn more tips on using Git with Visual Studio Team Services, see [Develop and share your code in Git using Visual Studio](https://www.visualstudio.com/en-us/docs/git/share-your-code-in-git-vs-2017) and for information about using a Git repository that's not managed by Visual Studio Team Services to publish to Azure, see [Continuous Deployment to Azure App Service](../app-service-web/app-service-continuous-deployment.md).</span></span> <span data-ttu-id="d3818-223">Mer information om Visual Studio Team Services finns [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).</span><span class="sxs-lookup"><span data-stu-id="d3818-223">For more information on Visual Studio Team Services, see [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).</span></span>

[0]: ./media/cloud-services-continuous-delivery-use-vso/tfs0.PNG
[1]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateTeamProjectInGit.PNG
[2]: ./media/cloud-services-continuous-delivery-use-vso/tfs2.png
[3]: ./media/cloud-services-continuous-delivery-use-vso-git/CloneThisRepository.PNG
[4]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateNewSolutionInClonedRepo.PNG
[7]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitMenuItem.PNG
[8]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[9]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateCloudService.PNG
[10]: ./media/cloud-services-continuous-delivery-use-vso-git/SetUpPublishingWithVSO.PNG
[11]: ./media/cloud-services-continuous-delivery-use-vso-git/AuthorizeConnection.PNG
[12]: ./media/cloud-services-continuous-delivery-use-vso-git/ConnectionRequest.PNG
[13]: ./media/cloud-services-continuous-delivery-use-vso-git/ChooseARepo3.PNG
[14]: ./media/cloud-services-continuous-delivery-use-vso/tfs14.png
[15]: ./media/cloud-services-continuous-delivery-use-vso/tfs15.png
[16]: ./media/cloud-services-continuous-delivery-use-vso/tfs16.png
[17]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs17.png
[18]: ./media/cloud-services-continuous-delivery-use-vso-git/MakeACodeChange.PNG
[20]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[21]: ./media/cloud-services-continuous-delivery-use-vso-git/TeamExplorerHome.png
[22]: ./media/cloud-services-continuous-delivery-use-vso-git/TeamExplorerBuilds.PNG
[23]: ./media/cloud-services-continuous-delivery-use-vso-git/BuildInQueue.png
[24]: ./media/cloud-services-continuous-delivery-use-vso/tfs24.png
[25]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs25.png
[26]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs26.png
[27]: ./media/cloud-services-continuous-delivery-use-vso-git/ProcessTab.PNG
[28]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs28.png
[29]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs29.png
[30]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs30.png
[31]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs31.png
[32]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs32.png
[33]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs33.png
[34]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs34.png
[35]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs35.png
[36]: ./media/cloud-services-continuous-delivery-use-vso/tfs36.PNG
[37]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateANewAccount.PNG
[38]: ./media/cloud-services-continuous-delivery-use-vso-git/SyncChanges2.PNG
[39]: ./media/cloud-services-continuous-delivery-use-vso-git/PushCurrentBranch.PNG
[40]: ./media/cloud-services-continuous-delivery-use-vso-git/BranchesInTeamExplorer.PNG
[41]: ./media/cloud-services-continuous-delivery-use-vso-git/NewBranch.PNG
[42]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateBranch.PNG
[43]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[44]: ./media/cloud-services-continuous-delivery-use-vso-git/PublishBranch.PNG
[45]: ./media/cloud-services-continuous-delivery-use-vso-git/SyncChanges2.PNG
[47]: ./media/cloud-services-continuous-delivery-use-vso-git/SourceSettingsPage.PNG
[48]: ./media/cloud-services-continuous-delivery-use-vso-git/IncludeWorkingBranch.PNG
