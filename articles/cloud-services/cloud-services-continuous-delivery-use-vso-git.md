---
title: aaaContinuous leverans med Git och Visual Studio Team Services i Azure | Microsoft Docs
description: "Lär dig hur tooconfigure din Visual Studio Team Services team projekt toouse Git tooautomatically skapa och distribuera toohello webbprogram funktion i Azure App Service eller cloud services."
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
ms.openlocfilehash: 936c42194f45be55597a77f9a3a6deb4480ed94b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="continuous-delivery-tooazure-using-visual-studio-team-services-and-git"></a><span data-ttu-id="a34e5-103">Kontinuerlig leverans tooAzure med hjälp av Visual Studio Team Services och Git</span><span class="sxs-lookup"><span data-stu-id="a34e5-103">Continuous delivery tooAzure using Visual Studio Team Services and Git</span></span>
<span data-ttu-id="a34e5-104">Du kan använda Visual Studio Team Services team projekt toohost en Git-lagringsplats för din källkod och automatiskt skapa och distribuera tooAzure webbprogram eller molntjänster när du överför en commit toohello databas.</span><span class="sxs-lookup"><span data-stu-id="a34e5-104">You can use Visual Studio Team Services team projects toohost a Git repository for your source code, and automatically build and deploy tooAzure web apps or cloud services whenever you push a commit toohello repository.</span></span>

<span data-ttu-id="a34e5-105">Du behöver Visual Studio 2013 och hello Azure SDK är installerat.</span><span class="sxs-lookup"><span data-stu-id="a34e5-105">You'll need Visual Studio 2013 and hello Azure SDK installed.</span></span> <span data-ttu-id="a34e5-106">Om du inte redan har Visual Studio 2013 kan du hämta det genom att välja hello **Kom igång gratis** länka när [www.visualstudio.com](http://www.visualstudio.com). Installera hello Azure SDK från [här](http://go.microsoft.com/fwlink/?LinkId=239540).</span><span class="sxs-lookup"><span data-stu-id="a34e5-106">If you don't already have Visual Studio 2013, download it by choosing hello **Get started for free** link at [www.visualstudio.com](http://www.visualstudio.com). Install hello Azure SDK from [here](http://go.microsoft.com/fwlink/?LinkId=239540).</span></span>

> [!NOTE]
> <span data-ttu-id="a34e5-107">Du behöver ett Visual Studio Team Services-konto toocomplete självstudierna: du kan [öppnar ett Visual Studio Team Services-konto gratis](http://go.microsoft.com/fwlink/p/?LinkId=512979).</span><span class="sxs-lookup"><span data-stu-id="a34e5-107">You need an Visual Studio Team Services account toocomplete this tutorial: You can [open a Visual Studio Team Services account for free](http://go.microsoft.com/fwlink/p/?LinkId=512979).</span></span>
> 
> 

<span data-ttu-id="a34e5-108">tooset upp en cloud service tooautomatically skapa och distribuera tooAzure med hjälp av Visual Studio Team Services, Följ dessa steg.</span><span class="sxs-lookup"><span data-stu-id="a34e5-108">tooset up a cloud service tooautomatically build and deploy tooAzure by using Visual Studio Team Services, follow these steps.</span></span>

## <a name="1-create-a-git-repository"></a><span data-ttu-id="a34e5-109">1: skapa en Git-lagringsplats</span><span class="sxs-lookup"><span data-stu-id="a34e5-109">1: Create a Git repository</span></span>
1. <span data-ttu-id="a34e5-110">Om du inte redan har en Visual Studio Team Services-konto kan du skaffa ett [här](http://go.microsoft.com/fwlink/?LinkId=397665).</span><span class="sxs-lookup"><span data-stu-id="a34e5-110">If you don’t already have a Visual Studio Team Services account, you can get one  [here](http://go.microsoft.com/fwlink/?LinkId=397665).</span></span> <span data-ttu-id="a34e5-111">När du skapar ditt team projekt, Välj Git som ditt källkontrollsystem.</span><span class="sxs-lookup"><span data-stu-id="a34e5-111">When you create your team project, choose Git as your source control system.</span></span> <span data-ttu-id="a34e5-112">Följ hello instruktioner tooconnect Visual Studio tooyour team projekt.</span><span class="sxs-lookup"><span data-stu-id="a34e5-112">Follow hello instructions tooconnect Visual Studio tooyour team project.</span></span>
2. <span data-ttu-id="a34e5-113">I **Team Explorer**, Välj hello **klona lagringsplatsen** länk.</span><span class="sxs-lookup"><span data-stu-id="a34e5-113">In **Team Explorer**, choose hello **Clone this repository** link.</span></span>
   
    ![][3]
3. <span data-ttu-id="a34e5-114">Anger hello plats hello lokal kopia och sedan välja hello **klona** knappen.</span><span class="sxs-lookup"><span data-stu-id="a34e5-114">Specify hello location of hello local copy and then choose hello **Clone** button.</span></span>

## <a name="2-create-a-project-and-commit-it-toohello-repository"></a><span data-ttu-id="a34e5-115">2: skapa ett projekt och genomför den toohello databasen</span><span class="sxs-lookup"><span data-stu-id="a34e5-115">2: Create a project and commit it toohello repository</span></span>
1. <span data-ttu-id="a34e5-116">I **Team Explorer**, i hello **lösningar** väljer hello **ny** länka toocreate ett nytt projekt i hello lokal databas.</span><span class="sxs-lookup"><span data-stu-id="a34e5-116">In **Team Explorer**, in hello **Solutions** section, choose hello **New** link toocreate a new project in hello local repository.</span></span>
   
    ![][4]
2. <span data-ttu-id="a34e5-117">Du kan distribuera en webbapp eller en tjänst i molnet (Azure-program) med följande hello steg i den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="a34e5-117">You can deploy a web app or a cloud service (Azure Application) by following hello steps in this walkthrough.</span></span> <span data-ttu-id="a34e5-118">Skapa ett nytt Azure Cloud Service-projekt, eller ett nytt ASP.NET MVC-projekt.</span><span class="sxs-lookup"><span data-stu-id="a34e5-118">Create a new Azure Cloud Service project, or a new ASP.NET MVC project.</span></span> <span data-ttu-id="a34e5-119">Se till att hello projektet mål hello .NET Framework 4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="a34e5-119">Make sure that hello project targets hello .NET Framework 4 or later.</span></span> <span data-ttu-id="a34e5-120">Om du skapar ett molntjänstprojekt kan du lägga till en ASP.NET MVC-webbroll och en arbetsroll.</span><span class="sxs-lookup"><span data-stu-id="a34e5-120">If you are creating a cloud service project, add an ASP.NET MVC web role and a worker role.</span></span>
   <span data-ttu-id="a34e5-121">Om du vill toocreate ett webbprogram väljer hello **ASP.NET-webbprogram** projektmall och välj sedan **MVC**.</span><span class="sxs-lookup"><span data-stu-id="a34e5-121">If you want toocreate a web app, choose hello **ASP.NET Web Application** project template, and then choose **MVC**.</span></span> <span data-ttu-id="a34e5-122">Se [skapa en ASP.NET-webbapp i Azure App Service](../app-service-web/app-service-web-get-started-dotnet.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="a34e5-122">See [Create an ASP.NET web app in Azure App Service](../app-service-web/app-service-web-get-started-dotnet.md) for more information.</span></span>
3. <span data-ttu-id="a34e5-123">Öppna hello snabbmenyn för hello lösning och välj **genomför**.</span><span class="sxs-lookup"><span data-stu-id="a34e5-123">Open hello shortcut menu for hello solution, and choose **Commit**.</span></span>
   
    ![][7]
4. <span data-ttu-id="a34e5-124">Om detta är hello första gången som du har använt Git i Visual Studio Team Services behöver du tooprovide vissa information tooidentify själv i Git.</span><span class="sxs-lookup"><span data-stu-id="a34e5-124">If this is hello first time you've used Git in Visual Studio Team Services, you'll need tooprovide some information tooidentify yourself in Git.</span></span> <span data-ttu-id="a34e5-125">I hello **väntande ändringar** område i **Team Explorer**, ange ditt användarnamn och e-postadress.</span><span class="sxs-lookup"><span data-stu-id="a34e5-125">In hello **Pending Changes** area of **Team Explorer**, enter your username and email address.</span></span> <span data-ttu-id="a34e5-126">Ange en kommentar för hello genomförande och välj hello **Commit** knappen.</span><span class="sxs-lookup"><span data-stu-id="a34e5-126">Enter a comment for hello commit and then choose hello **Commit** button.</span></span>
   
    ![][8]
5. <span data-ttu-id="a34e5-127">Observera hello alternativ tooinclude med eller undanta specifika ändringar när du checkar in.</span><span class="sxs-lookup"><span data-stu-id="a34e5-127">Note hello options tooinclude or exclude specific changes when you check in.</span></span> <span data-ttu-id="a34e5-128">Om hello ändringar du vill utesluts, väljer **inkluderar alla**.</span><span class="sxs-lookup"><span data-stu-id="a34e5-128">If hello changes you want are excluded, choose **Include All**.</span></span>
6. <span data-ttu-id="a34e5-129">Du har nu allokerade hello ändringar i den lokala kopian av hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="a34e5-129">You've now committed hello changes in your local copy of hello repository.</span></span> <span data-ttu-id="a34e5-130">Sedan synkronisera ändringarna med hello server genom att välja hello **Sync** länk.</span><span class="sxs-lookup"><span data-stu-id="a34e5-130">Next, sync those changes with hello server by choosing hello **Sync** link.</span></span>

## <a name="3-connect-hello-project-tooazure"></a><span data-ttu-id="a34e5-131">3: Anslut hello projekt tooAzure</span><span class="sxs-lookup"><span data-stu-id="a34e5-131">3: Connect hello project tooAzure</span></span>
1. <span data-ttu-id="a34e5-132">Nu när du har en Git-lagringsplats i Visual Studio Team Services med vissa källkod i den är du redo tooconnect tooAzure din git-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="a34e5-132">Now that you have a Git repository in Visual Studio Team Services with some source code in it, you are ready tooconnect your git repository tooAzure.</span></span>  <span data-ttu-id="a34e5-133">I hello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885), Välj din cloud service eller web app eller skapa en ny genom att välja hello + längst hello nedre vänstra hörnet och välja **Molntjänsten** eller **webbprogrammet**och sedan **Snabbregistrering**.</span><span class="sxs-lookup"><span data-stu-id="a34e5-133">In hello [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885), select your cloud service or web app, or create a new one by choosing hello + icon at hello bottom left and choosing **Cloud Service** or **Web App** and then **Quick Create**.</span></span>
   
    ![][9]
2. <span data-ttu-id="a34e5-134">Välj hello för molntjänster, **Konfigurera publicering med Visual Studio Team Services** länk.</span><span class="sxs-lookup"><span data-stu-id="a34e5-134">For cloud services, choose hello **Set up publishing with Visual Studio Team Services** link.</span></span> <span data-ttu-id="a34e5-135">Välj hello för web apps **konfigurera distribution från källkontroll** länk.</span><span class="sxs-lookup"><span data-stu-id="a34e5-135">For web apps, choose hello **Set up deployment from source control** link.</span></span>
   
    ![][10]
3. <span data-ttu-id="a34e5-136">Hello i guiden anger hello namnet på ditt konto i Visual Studio Team Services hello textrutan med och välj hello **auktorisera nu** länk.</span><span class="sxs-lookup"><span data-stu-id="a34e5-136">In hello wizard, type hello name of your Visual Studio Team Services account in hello textbox and choose hello **Authorize Now** link.</span></span> <span data-ttu-id="a34e5-137">Du kan uppmanas toosign i.</span><span class="sxs-lookup"><span data-stu-id="a34e5-137">You might be asked toosign in.</span></span>
   
    ![][11]
4. <span data-ttu-id="a34e5-138">I hello **anslutningsbegäran** popup-fönstret, Välj **acceptera** tooauthorize Azure tooconfigure ditt team projekt i Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="a34e5-138">In hello **Connection Request** pop-up dialog, choose **Accept** tooauthorize Azure tooconfigure your team project in Visual Studio Team Services.</span></span>
   
    ![][12]
5. <span data-ttu-id="a34e5-139">När tillståndet lyckas visas en listruta som innehåller din Visual Studio Team Services grupprojekt.</span><span class="sxs-lookup"><span data-stu-id="a34e5-139">After authorization succeeds, you see a dropdown list that contains your Visual Studio Team Services team projects.</span></span>  <span data-ttu-id="a34e5-140">Välj hello grupprojekt som du skapade i föregående steg i hello och välj hello guiden markering.</span><span class="sxs-lookup"><span data-stu-id="a34e5-140">Select hello name of team project that you created in hello previous steps, and choose hello wizard's checkmark button.</span></span>
   
    ![][13]
   
    <span data-ttu-id="a34e5-141">hello nästa gång som du överför en commit tooyour databas, Visual Studio Team Services skapar och distribuerar ditt projekt tooAzure.</span><span class="sxs-lookup"><span data-stu-id="a34e5-141">hello next time you push a commit tooyour repository, Visual Studio Team Services will build and deploy your project tooAzure.</span></span>

## <a name="4-trigger-a-rebuild-and-redeploy-your-project"></a><span data-ttu-id="a34e5-142">4: utlösa en återskapning och omdistribuera ditt projekt</span><span class="sxs-lookup"><span data-stu-id="a34e5-142">4: Trigger a rebuild and redeploy your project</span></span>
1. <span data-ttu-id="a34e5-143">Öppna en fil och ändra den i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a34e5-143">In Visual Studio, open up a file and change it.</span></span> <span data-ttu-id="a34e5-144">Till exempel ändra hello filen `_Layout.cshtml` under hello vyer\\delad mapp i en MVC-webbroll.</span><span class="sxs-lookup"><span data-stu-id="a34e5-144">For example, change hello file `_Layout.cshtml` under hello Views\\Shared folder in an MVC web role.</span></span>
   
    ![][17]
2. <span data-ttu-id="a34e5-145">Redigera hello sidfotstexten för hello plats och spara hello-filen.</span><span class="sxs-lookup"><span data-stu-id="a34e5-145">Edit hello footer text for hello site and save hello file.</span></span>
   
    ![][18]
3. <span data-ttu-id="a34e5-146">I **Solution Explorer**, öppna hello snabbmenyn för hello lösning nod, projektnoden eller hello-filen som du har ändrats och välj sedan **genomför**.</span><span class="sxs-lookup"><span data-stu-id="a34e5-146">In **Solution Explorer**, open hello shortcut menu for hello solution node, project node, or hello file you changed, and then choose **Commit**.</span></span>
4. <span data-ttu-id="a34e5-147">Ange en kommentar och välj **genomför**.</span><span class="sxs-lookup"><span data-stu-id="a34e5-147">Type in a comment and choose **Commit**.</span></span>
   
    ![][20]
5. <span data-ttu-id="a34e5-148">Välj hello **Sync** länk.</span><span class="sxs-lookup"><span data-stu-id="a34e5-148">Choose hello **Sync** link.</span></span>
   
    ![][38]
6. <span data-ttu-id="a34e5-149">Välj hello **Push** länka toopush commit toohello databasen i Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="a34e5-149">Choose hello **Push** link toopush your commit toohello repository in Visual Studio Team Services.</span></span> <span data-ttu-id="a34e5-150">(Du kan också använda hello **Sync** knappen toocopy incheckningar toohello databasen.</span><span class="sxs-lookup"><span data-stu-id="a34e5-150">(You can also use hello **Sync** button toocopy your commits toohello repository.</span></span> <span data-ttu-id="a34e5-151">hello skillnaden är att **Sync** också hämtar hello senaste ändringarna från databasen hello.)</span><span class="sxs-lookup"><span data-stu-id="a34e5-151">hello difference is that **Sync** also pulls hello latest changes from hello repository.)</span></span>
   
    ![][39]
7. <span data-ttu-id="a34e5-152">Välj hello **hem** knappen tooreturn toohello **Team Explorer** startsidan.</span><span class="sxs-lookup"><span data-stu-id="a34e5-152">Choose hello **Home** button tooreturn toohello **Team Explorer** home page.</span></span>
   
    ![][21]
8. <span data-ttu-id="a34e5-153">Välj **bygger** tooview hello bygger pågår.</span><span class="sxs-lookup"><span data-stu-id="a34e5-153">Choose **Builds** tooview hello builds in progress.</span></span>
   
    ![][22]
   
    <span data-ttu-id="a34e5-154">**Teamet Explorer** visar att en version har utlösts för din incheckning.</span><span class="sxs-lookup"><span data-stu-id="a34e5-154">**Team Explorer** shows that a build has been triggered for your check-in.</span></span>
   
    ![][23]
9. <span data-ttu-id="a34e5-155">tooview detaljerade loggfil som hello skapa fortlöper, dubbelklickar du på hello namnet på hello build pågår.</span><span class="sxs-lookup"><span data-stu-id="a34e5-155">tooview a detailed log as hello build progresses, double-click hello name of hello build in progress.</span></span>
10. <span data-ttu-id="a34e5-156">Hello build är pågående, ta en titt på hello build definition som skapades när du använde hello guiden toolink tooAzure.</span><span class="sxs-lookup"><span data-stu-id="a34e5-156">While hello build is in-progress, take a look at hello build definition that was created when you used hello wizard toolink tooAzure.</span></span>  <span data-ttu-id="a34e5-157">Öppna hello snabbmenyn för hello build definition och välj **redigera skapa Definition**.</span><span class="sxs-lookup"><span data-stu-id="a34e5-157">Open hello shortcut menu for hello build definition and choose **Edit Build Definition**.</span></span>
    
    ![][25]
11. <span data-ttu-id="a34e5-158">I hello **utlösaren** fliken ser du att hello build definition anges toobuild på varje incheckning, som standard.</span><span class="sxs-lookup"><span data-stu-id="a34e5-158">In hello **Trigger** tab, you will see that hello build definition is set toobuild on every check-in, by default.</span></span> <span data-ttu-id="a34e5-159">(För en molnbaserad tjänst bör Visual Studio Team Services skapar och distribuerar hello mastergrenen toohello mellanlagring miljö automatiskt.</span><span class="sxs-lookup"><span data-stu-id="a34e5-159">(For a cloud service, Visual Studio Team Services builds and deploys hello master branch toohello staging environment automatically.</span></span> <span data-ttu-id="a34e5-160">Du har fortfarande toodo en manuell åtgärd toodeploy toohello drift.</span><span class="sxs-lookup"><span data-stu-id="a34e5-160">You still have toodo a manual step toodeploy toohello live site.</span></span> <span data-ttu-id="a34e5-161">För en webbapp som inte har mellanlagring miljön distribueras hello mastergrenen direkt toohello live plats.</span><span class="sxs-lookup"><span data-stu-id="a34e5-161">For a web app that doesn't have staging environment, it deploys hello master branch directly toohello live site.</span></span>
    
    ![][26]
12. <span data-ttu-id="a34e5-162">I hello **processen** fliken visas hello distributionsmiljö anges toohello namnet på din cloud service eller web app.</span><span class="sxs-lookup"><span data-stu-id="a34e5-162">In hello **Process** tab, you can see hello deployment environment is set toohello name of your cloud service or web app.</span></span>
    
     ![][27]
13. <span data-ttu-id="a34e5-163">Ange värden för hello egenskaper om du vill att andra värden än hello standardvärden.</span><span class="sxs-lookup"><span data-stu-id="a34e5-163">Specify values for hello properties if you want different values than hello defaults.</span></span> <span data-ttu-id="a34e5-164">hello egenskaper för Azure publicering finns i hello **distribution** avsnitt, och du kanske också tooset MSBuild-parametrar.</span><span class="sxs-lookup"><span data-stu-id="a34e5-164">hello properties for Azure publishing are in hello **Deployment** section, and you might also need tooset MSBuild parameters.</span></span> <span data-ttu-id="a34e5-165">Till exempel i molntjänstprojektet toospecify en tjänstkonfiguration än ”moln” och ange hello MSbuild-parametrar för`/p:TargetProfile=[YourProfile]` där *[YourProfile]* matchar en tjänstkonfigurationsfil med namnet ServiceConfiguration. *YourProfile*.cscfg.</span><span class="sxs-lookup"><span data-stu-id="a34e5-165">For example, in a cloud service project, toospecify a service configuration other than "Cloud", set hello MSbuild parameters too`/p:TargetProfile=[YourProfile]` where *[YourProfile]* matches a service configuration file with a name like ServiceConfiguration.*YourProfile*.cscfg.</span></span>
    
     <span data-ttu-id="a34e5-166">hello följande tabell visar hello tillgängliga egenskaper i hello **distribution** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="a34e5-166">hello following table shows hello available properties in hello **Deployment** section:</span></span>
    
    | <span data-ttu-id="a34e5-167">Egenskap</span><span class="sxs-lookup"><span data-stu-id="a34e5-167">Property</span></span> | <span data-ttu-id="a34e5-168">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="a34e5-168">Default Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="a34e5-169">Tillåt ej betrodda certifikat</span><span class="sxs-lookup"><span data-stu-id="a34e5-169">Allow Untrusted Certificates</span></span> |<span data-ttu-id="a34e5-170">Om värdet är false måste SSL-certifikat signeras av en rotcertifikatutfärdare.</span><span class="sxs-lookup"><span data-stu-id="a34e5-170">If false, SSL certificates must be signed by a root authority.</span></span> |
    | <span data-ttu-id="a34e5-171">Tillåt uppgradering</span><span class="sxs-lookup"><span data-stu-id="a34e5-171">Allow Upgrade</span></span> |<span data-ttu-id="a34e5-172">Tillåter hello distribution tooupdate en befintlig distribution i stället för att skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="a34e5-172">Allows hello deployment tooupdate an existing deployment instead of creating a new one.</span></span> <span data-ttu-id="a34e5-173">Bevarar hello IP-adress.</span><span class="sxs-lookup"><span data-stu-id="a34e5-173">Preserves hello IP address.</span></span> |
    | <span data-ttu-id="a34e5-174">Ta inte bort</span><span class="sxs-lookup"><span data-stu-id="a34e5-174">Do Not Delete</span></span> |<span data-ttu-id="a34e5-175">Om värdet är true, Skriv inte över en befintlig orelaterade distribution (uppgraderingen tillåts).</span><span class="sxs-lookup"><span data-stu-id="a34e5-175">If true, do not overwrite an existing unrelated deployment (upgrade is allowed).</span></span> |
    | <span data-ttu-id="a34e5-176">Sökvägen tooDeployment inställningar</span><span class="sxs-lookup"><span data-stu-id="a34e5-176">Path tooDeployment Settings</span></span> |<span data-ttu-id="a34e5-177">hello sökväg tooyour .pubxml fil för en webbapp, relativa toohello rotmapp hello lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="a34e5-177">hello path tooyour .pubxml file for a web app, relative toohello root folder of hello repo.</span></span> <span data-ttu-id="a34e5-178">Ignoreras för molntjänster.</span><span class="sxs-lookup"><span data-stu-id="a34e5-178">Ignored for cloud services.</span></span> |
    | <span data-ttu-id="a34e5-179">SharePoint-miljön för distribution</span><span class="sxs-lookup"><span data-stu-id="a34e5-179">Sharepoint Deployment Environment</span></span> |<span data-ttu-id="a34e5-180">Hej samma som hello tjänstnamn.</span><span class="sxs-lookup"><span data-stu-id="a34e5-180">hello same as hello service name.</span></span> |
    | <span data-ttu-id="a34e5-181">Av Azure-distributionsmiljö</span><span class="sxs-lookup"><span data-stu-id="a34e5-181">Azure Deployment Environment</span></span> |<span data-ttu-id="a34e5-182">hello web app eller molnet tjänstnamn.</span><span class="sxs-lookup"><span data-stu-id="a34e5-182">hello web app or cloud service name.</span></span> |
14. <span data-ttu-id="a34e5-183">Vid den tidpunkten ska din build slutföras.</span><span class="sxs-lookup"><span data-stu-id="a34e5-183">By this time, your build should be completed successfully.</span></span>
    
     ![][28]
15. <span data-ttu-id="a34e5-184">Om du dubbelklickar på hello build namn, Visual Studio visas en **skapa sammanfattning**, inklusive alla resultat från associerade testprojekt.</span><span class="sxs-lookup"><span data-stu-id="a34e5-184">If you double-click hello build name, Visual Studio shows a **Build Summary**, including any test results from associated unit test projects.</span></span>
    
     ![][29]
16. <span data-ttu-id="a34e5-185">I hello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885), du kan visa hello associerad distribution på hello **distributioner** fliken när hello mellanlagring miljön är markerad.</span><span class="sxs-lookup"><span data-stu-id="a34e5-185">In hello [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885), you can view hello associated deployment on hello **Deployments** tab when hello staging environment is selected.</span></span>
    
     ![][30]
17. <span data-ttu-id="a34e5-186">Bläddra tooyour webbplatsens URL.</span><span class="sxs-lookup"><span data-stu-id="a34e5-186">Browse tooyour site's URL.</span></span> <span data-ttu-id="a34e5-187">För en webbapp väljer hello **Bläddra** knappen hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="a34e5-187">For a web app, just choose  hello **Browse** button in hello portal.</span></span> <span data-ttu-id="a34e5-188">Välj hello URL för en molntjänst i hello **snabb i korthet** avsnitt i hello **instrumentpanelen** sidan som visar hello mellanlagringsmiljön.</span><span class="sxs-lookup"><span data-stu-id="a34e5-188">For a cloud service, choose hello URL in hello **Quick Glance** section of hello **Dashboard** page that shows hello Staging environment.</span></span>
    
    <span data-ttu-id="a34e5-189">Distributioner från kontinuerlig integration för molntjänster är publicerade toohello mellanlagringsmiljön som standard.</span><span class="sxs-lookup"><span data-stu-id="a34e5-189">Deployments from continuous integration for cloud services are published toohello Staging environment by default.</span></span> <span data-ttu-id="a34e5-190">Du kan ändra detta genom att ange hello **alternativa Molntjänstmiljö** egenskapen för**produktion**.</span><span class="sxs-lookup"><span data-stu-id="a34e5-190">You can change this by setting hello **Alternate Cloud Service Environment** property too**Production**.</span></span> <span data-ttu-id="a34e5-191">Här är där hello Webbadress på hello molnbaserad tjänst instrumentpanelssida.</span><span class="sxs-lookup"><span data-stu-id="a34e5-191">Here's where hello site URL is on hello cloud service's dashboard page.</span></span>
    
    ![][31]
    
    <span data-ttu-id="a34e5-192">En ny webbläsarflik öppnas tooreveal webbplatsen körs.</span><span class="sxs-lookup"><span data-stu-id="a34e5-192">A new browser tab will open tooreveal your running site.</span></span>
    
    ![][32]
18. <span data-ttu-id="a34e5-193">Om du gör andra ändringar tooyour projekt och du utlösaren bygger mer du samlar flera distributioner.</span><span class="sxs-lookup"><span data-stu-id="a34e5-193">If you make other changes tooyour project, you trigger more builds, and you will accumulate multiple deployments.</span></span> <span data-ttu-id="a34e5-194">hello senaste en är markerad som aktiv.</span><span class="sxs-lookup"><span data-stu-id="a34e5-194">hello latest one is marked as Active.</span></span>
    
    ![][33]

## <a name="5-redeploy-an-earlier-build"></a><span data-ttu-id="a34e5-195">5: Distribuera om en tidigare version</span><span class="sxs-lookup"><span data-stu-id="a34e5-195">5: Redeploy an earlier build</span></span>
<span data-ttu-id="a34e5-196">Det här steget är valfritt.</span><span class="sxs-lookup"><span data-stu-id="a34e5-196">This step is optional.</span></span> <span data-ttu-id="a34e5-197">I hello klassiska Azure-portalen, Välj en tidigare distribution och **omdistribuera** toorewind din webbplats tooan tidigare incheckning.</span><span class="sxs-lookup"><span data-stu-id="a34e5-197">In hello Azure classic portal, choose an earlier deployment and choose **Redeploy** toorewind your site tooan earlier check-in.</span></span> <span data-ttu-id="a34e5-198">Observera att detta utlöser en ny version i TFS och skapa en ny post i distributionshistoriken.</span><span class="sxs-lookup"><span data-stu-id="a34e5-198">Note that this will trigger a new build in TFS and create a new entry in your deployment history.</span></span>

![][34]

## <a name="6-change-hello-production-deployment"></a><span data-ttu-id="a34e5-199">6: ändra hello Produktionsdistribution</span><span class="sxs-lookup"><span data-stu-id="a34e5-199">6: Change hello Production deployment</span></span>
<span data-ttu-id="a34e5-200">När du är klar kan du befordra hello mellanlagring toohello produktionsmiljön genom att välja **växla** i hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a34e5-200">When you are ready, you can promote hello Staging environment toohello Production environment by choosing **Swap** in hello Azure classic portal.</span></span> <span data-ttu-id="a34e5-201">hello tidigare produktionsmiljö eventuella blir en mellanlagringsmiljön hello nyligen distribuerade mellanlagringsmiljön är upphöjt tooProduction.</span><span class="sxs-lookup"><span data-stu-id="a34e5-201">hello newly deployed Staging environment is promoted tooProduction, and hello previous Production environment, if any, becomes a Staging environment.</span></span> <span data-ttu-id="a34e5-202">hello aktiv distribution kan vara olika för hello produktions- och mellanlagringsmiljöer, men hello distributionshistoriken för senare versioner är hello samma oavsett miljö.</span><span class="sxs-lookup"><span data-stu-id="a34e5-202">hello Active deployment may be different for hello Production and Staging environments, but hello deployment history of recent builds is hello same regardless of environment.</span></span>

![][35]

## <a name="7-deploy-from-a-working-branch"></a><span data-ttu-id="a34e5-203">7: Distribuera från en arbetsgren.</span><span class="sxs-lookup"><span data-stu-id="a34e5-203">7: Deploy from a working branch.</span></span>
<span data-ttu-id="a34e5-204">När du använder Git du vanligtvis göra ändringar i en arbetsgren och integrera i hello mastergrenen när du utvecklar når slutfört tillstånd.</span><span class="sxs-lookup"><span data-stu-id="a34e5-204">When you use Git, you usually make changes in a working branch and integrate into hello master branch when your development reaches a finished state.</span></span> <span data-ttu-id="a34e5-205">Under hello utvecklingsfasen av ett projekt du vill toobuild och distribuera hello fungerande gren tooAzure.</span><span class="sxs-lookup"><span data-stu-id="a34e5-205">During hello development phase of a project, you'll want toobuild and deploy hello working branch tooAzure.</span></span>

1. <span data-ttu-id="a34e5-206">I **Team Explorer**, Välj hello **Start** knappen och välj sedan hello **filialer** knappen.</span><span class="sxs-lookup"><span data-stu-id="a34e5-206">In **Team Explorer**, choose hello **Home** button and then choose hello **Branches** button.</span></span>
   
    ![][40]
2. <span data-ttu-id="a34e5-207">Välj hello **ny gren** länk.</span><span class="sxs-lookup"><span data-stu-id="a34e5-207">Choose hello **New Branch** link.</span></span>
   
    ![][41]
3. <span data-ttu-id="a34e5-208">Ange hello namnet på hello gren, till exempel ”arbetar” och välj **skapa gren**.</span><span class="sxs-lookup"><span data-stu-id="a34e5-208">Enter hello name of hello branch, such as "working," and choose **Create Branch**.</span></span> <span data-ttu-id="a34e5-209">Detta skapar en ny lokal gren.</span><span class="sxs-lookup"><span data-stu-id="a34e5-209">This creates a new local branch.</span></span>
   
    ![][42]
4. <span data-ttu-id="a34e5-210">Publicera hello grenen.</span><span class="sxs-lookup"><span data-stu-id="a34e5-210">Publish hello branch.</span></span> <span data-ttu-id="a34e5-211">Välj hello gren namn i **Opublicerat filialer**, och välj **publicera**.</span><span class="sxs-lookup"><span data-stu-id="a34e5-211">Choose hello branch name in **Unpublished branches**, and choose **Publish**.</span></span>
   
    ![][44]
5. <span data-ttu-id="a34e5-212">Som standard bara ändras toohello mastergrenen utlösaren en kontinuerlig version.</span><span class="sxs-lookup"><span data-stu-id="a34e5-212">By default, only changes toohello master branch trigger a continuous build.</span></span> <span data-ttu-id="a34e5-213">tooset in kontinuerlig build för en arbetsgren, Välj hello **bygger** sidan i **Team Explorer**, och välj **redigera skapa Definition**.</span><span class="sxs-lookup"><span data-stu-id="a34e5-213">tooset up continuous build for a working branch, choose hello **Builds** page in **Team Explorer**, and choose **Edit Build Definition**.</span></span>
6. <span data-ttu-id="a34e5-214">Öppna hello **inställningar för datakälla** fliken. Under **övervakas grenar för kontinuerlig integrering och bygga**, Välj **Klicka här tooadd en ny rad**.</span><span class="sxs-lookup"><span data-stu-id="a34e5-214">Open hello **Source Settings** tab. Under **Monitored branches for continuous integration and build**, choose **Click here tooadd a new row**.</span></span>
   
    ![][47]
7. <span data-ttu-id="a34e5-215">Ange hello grenen som du skapat, till exempel refs, huvuden/fungerande.</span><span class="sxs-lookup"><span data-stu-id="a34e5-215">Specify hello branch you created, such as refs/heads/working.</span></span>
   
    ![][48]
8. <span data-ttu-id="a34e5-216">Gör en ändring i hello kod, öppna hello snabbmenyn för hello ändras filen och välj sedan **genomför**.</span><span class="sxs-lookup"><span data-stu-id="a34e5-216">Make a change in hello code, open hello shortcut menu for hello changed file, and then choose **Commit**.</span></span>
   
    ![][43]
9. <span data-ttu-id="a34e5-217">Välj hello **osynkroniserade incheckningar** länka och välj hello **Sync** knapp eller hello **Push** länk toocopy hello ändras toohello kopia av hello arbetsgren i Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="a34e5-217">Choose hello **Unsynced Commits** link, and choose  hello **Sync** button or hello **Push** link toocopy hello changes toohello copy of hello working branch in Visual Studio Team Services.</span></span>
   
   ![][45]
10. <span data-ttu-id="a34e5-218">Navigera toohello **bygger** visa och hitta hello-version som precis har utlösts för hello arbetsgren.</span><span class="sxs-lookup"><span data-stu-id="a34e5-218">Navigate toohello **Builds** view and find hello build that just got triggered for hello working branch.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a34e5-219">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a34e5-219">Next steps</span></span>
<span data-ttu-id="a34e5-220">toolearn mer tips om att använda Git med Visual Studio Team Services, se [utveckla och dela din kod i Git med Visual Studio](https://www.visualstudio.com/en-us/docs/git/share-your-code-in-git-vs-2017) och för information om hur du använder en Git-lagringsplats som inte hanteras av Visual Studio Team Services toopublish tooAzure, se [kontinuerlig distribution tooAzure Apptjänst](../app-service-web/app-service-continuous-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="a34e5-220">toolearn more tips on using Git with Visual Studio Team Services, see [Develop and share your code in Git using Visual Studio](https://www.visualstudio.com/en-us/docs/git/share-your-code-in-git-vs-2017) and for information about using a Git repository that's not managed by Visual Studio Team Services toopublish tooAzure, see [Continuous Deployment tooAzure App Service](../app-service-web/app-service-continuous-deployment.md).</span></span> <span data-ttu-id="a34e5-221">Mer information om Visual Studio Team Services finns [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).</span><span class="sxs-lookup"><span data-stu-id="a34e5-221">For more information on Visual Studio Team Services, see [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).</span></span>

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
