---
title: aaaContinuous leverans med Visual Studio Team Services i Azure | Microsoft Docs
description: "Lär dig hur tooconfigure din Visual Studio Team Services team projekt tooautomatically skapa och distribuera toohello webbprogram funktion i Azure App Service eller cloud services."
services: cloud-services
documentationcenter: .net
author: mlearned
manager: douge
editor: 
ms.assetid: 797f67ad-e4d4-4063-ae91-41cbdf154191
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/06/2016
ms.author: mlearned
ms.openlocfilehash: eae75729e1c1a55f9bc3375604a8192f329d0042
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="continuous-delivery-tooazure-using-visual-studio-team-services"></a><span data-ttu-id="4a7d6-103">Kontinuerlig leverans tooAzure med hjälp av Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="4a7d6-103">Continuous delivery tooAzure using Visual Studio Team Services</span></span>
<span data-ttu-id="4a7d6-104">Du kan konfigurera din Visual Studio Team Services team projekt tooautomatically skapa och distribuera tooAzure webbprogram och molntjänster.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-104">You can configure your Visual Studio Team Services team projects tooautomatically build and deploy tooAzure web apps or cloud services.</span></span>  <span data-ttu-id="4a7d6-105">(Mer information om hur tooset upp en kontinuerlig version och distribuera system med en *lokalt* Team Foundation Server finns [kontinuerlig leverans för molntjänster i Azure](cloud-services-dotnet-continuous-delivery.md).)</span><span class="sxs-lookup"><span data-stu-id="4a7d6-105">(For information on how tooset up a continuous build and deploy system using an *on-premises* Team Foundation Server, see [Continuous Delivery for Cloud Services in Azure](cloud-services-dotnet-continuous-delivery.md).)</span></span>

<span data-ttu-id="4a7d6-106">Den här kursen förutsätter att du har Visual Studio 2013 och hello Azure SDK är installerat.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-106">This tutorial assumes you have Visual Studio 2013 and hello Azure SDK installed.</span></span> <span data-ttu-id="4a7d6-107">Om du inte redan har Visual Studio 2013 kan du hämta det genom att välja hello **Kom igång gratis** länka när [www.visualstudio.com](http://www.visualstudio.com). Installera hello Azure SDK från [här](http://go.microsoft.com/fwlink/?LinkId=239540).</span><span class="sxs-lookup"><span data-stu-id="4a7d6-107">If you don't already have Visual Studio 2013, download it by choosing hello **Get started for free** link at [www.visualstudio.com](http://www.visualstudio.com). Install hello Azure SDK from [here](http://go.microsoft.com/fwlink/?LinkId=239540).</span></span>

> [!NOTE]
> <span data-ttu-id="4a7d6-108">Du behöver ett Visual Studio Team Services-konto toocomplete självstudierna: du kan [öppnar ett Visual Studio Team Services-konto gratis](http://go.microsoft.com/fwlink/p/?LinkId=512979).</span><span class="sxs-lookup"><span data-stu-id="4a7d6-108">You need an Visual Studio Team Services account toocomplete this tutorial: You can [open a Visual Studio Team Services account for free](http://go.microsoft.com/fwlink/p/?LinkId=512979).</span></span>
> 
> 

<span data-ttu-id="4a7d6-109">tooset upp en cloud service tooautomatically skapa och distribuera tooAzure med hjälp av Visual Studio Team Services, Följ dessa steg.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-109">tooset up a cloud service tooautomatically build and deploy tooAzure by using Visual Studio Team Services, follow these steps.</span></span>

## <a name="1-create-a-team-project"></a><span data-ttu-id="4a7d6-110">1: skapa ett team projekt</span><span class="sxs-lookup"><span data-stu-id="4a7d6-110">1: Create a team project</span></span>
<span data-ttu-id="4a7d6-111">Följ instruktionerna för hello [här](http://go.microsoft.com/fwlink/?LinkId=512980) toocreate ditt team projekt och länka det tooVisual Studio.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-111">Follow hello instructions [here](http://go.microsoft.com/fwlink/?LinkId=512980) toocreate your team project and link it tooVisual Studio.</span></span> <span data-ttu-id="4a7d6-112">Den här genomgången förutsätter att du använder Team Foundation Version kontrollen (TFVC) som käll-kontroll lösning.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-112">This walkthrough assumes you are using Team Foundation Version Control (TFVC) as your source control solution.</span></span> <span data-ttu-id="4a7d6-113">Om du vill toouse Git för versionskontroll, se [hello Git-versionen av den här genomgången](http://go.microsoft.com/fwlink/p/?LinkId=397358).</span><span class="sxs-lookup"><span data-stu-id="4a7d6-113">If you want toouse Git for version control, see [hello Git version of this walkthrough](http://go.microsoft.com/fwlink/p/?LinkId=397358).</span></span>

## <a name="2-check-in-a-project-toosource-control"></a><span data-ttu-id="4a7d6-114">2: Kontrollera i projektet toosource kontroll</span><span class="sxs-lookup"><span data-stu-id="4a7d6-114">2: Check in a project toosource control</span></span>
1. <span data-ttu-id="4a7d6-115">Öppna i Visual Studio hello lösning du vill toodeploy eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-115">In Visual Studio, open hello solution you want toodeploy, or create a new one.</span></span>
   <span data-ttu-id="4a7d6-116">Du kan distribuera en webbapp eller en tjänst i molnet (Azure-program) med följande hello steg i den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-116">You can deploy a web app or a cloud service (Azure Application) by following hello steps in this walkthrough.</span></span>
   <span data-ttu-id="4a7d6-117">Om du vill toocreate en ny lösning, skapa ett nytt Azure Cloud Service-projekt, eller ett nytt ASP.NET MVC-projekt.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-117">If you want toocreate a new solution, create a new Azure Cloud Service project, or a new ASP.NET MVC project.</span></span> <span data-ttu-id="4a7d6-118">Kontrollera att som hello projektet mål .NET Framework 4 eller 4.5 och om du skapar ett molntjänstprojekt lägga till en ASP.NET MVC-webbroll och en arbetsroll och välj Internetprogram för hello webbroll.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-118">Make sure that hello project targets .NET Framework 4 or 4.5, and if you are creating a cloud service project, add an ASP.NET MVC web role and a worker role, and choose Internet application for hello web role.</span></span> <span data-ttu-id="4a7d6-119">Välj **Internetprogram**.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-119">When prompted, choose **Internet Application**.</span></span>
   <span data-ttu-id="4a7d6-120">Om du vill toocreate en webbapp, Välj hello projektmall för ASP.NET-webbprogram och välja MVC.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-120">If you want toocreate a web app, choose hello ASP.NET Web Application project template, and then choose MVC.</span></span> <span data-ttu-id="4a7d6-121">Se [skapa en ASP.NET-webbapp i Azure App Service](../app-service-web/app-service-web-get-started-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="4a7d6-121">See [Create an ASP.NET web app in Azure App Service](../app-service-web/app-service-web-get-started-dotnet.md).</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="4a7d6-122">Visual Studio Team Services har endast stöd för CI-distributioner av Visual Studio-webbprogram just nu.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-122">Visual Studio Team Services only support CI deployments of Visual Studio Web Applications at this time.</span></span> <span data-ttu-id="4a7d6-123">Webbplatsen projekt ligger utanför omfånget.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-123">Web Site projects are out of scope.</span></span>
   > 
   > 
2. <span data-ttu-id="4a7d6-124">Öppna hello snabbmenyn för hello lösning och välj **Lägg till lösning tooSource kontrollen**.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-124">Open hello context menu for hello solution, and choose **Add Solution tooSource Control**.</span></span>
   
    ![][5]
3. <span data-ttu-id="4a7d6-125">Acceptera eller ändra hello standard och välj hello **OK** knappen.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-125">Accept or change hello defaults and choose hello **OK** button.</span></span> <span data-ttu-id="4a7d6-126">När hello är klart ikoner för datakällan visas i **Solution Explorer**.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-126">Once hello process completes, source control icons appear in **Solution Explorer**.</span></span>
   
    ![][6]
4. <span data-ttu-id="4a7d6-127">Öppna hello snabbmenyn för hello lösning och välj **checka In**.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-127">Open hello shortcut menu for hello solution, and choose **Check In**.</span></span>
   
    ![][7]
5. <span data-ttu-id="4a7d6-128">I hello **väntande ändringar** område i **Team Explorer**, skriver du en kommentar för incheckning hello och välj hello **checka In** knappen.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-128">In hello **Pending Changes** area of **Team Explorer**, type a comment for hello check-in and choose hello **Check In** button.</span></span>
   
    ![][8]
   
    <span data-ttu-id="4a7d6-129">Observera hello alternativ tooinclude med eller undanta specifika ändringar när du checkar in.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-129">Note hello options tooinclude or exclude specific changes when you check in.</span></span> <span data-ttu-id="4a7d6-130">Om du vill ändringar utesluts, Välj hello **inkluderar alla** länk.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-130">If desired changes are excluded, choose hello **Include All** link.</span></span>
   
    ![][9]

## <a name="3-connect-hello-project-tooazure"></a><span data-ttu-id="4a7d6-131">3: Anslut hello projekt tooAzure</span><span class="sxs-lookup"><span data-stu-id="4a7d6-131">3: Connect hello project tooAzure</span></span>
1. <span data-ttu-id="4a7d6-132">Nu när du har ett VS Team Services team projekt med vissa källkoden i den är du redo tooconnect ditt team projekt tooAzure.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-132">Now that you have a VS Team Services team project with some source code in it, you are ready tooconnect your team project tooAzure.</span></span>  <span data-ttu-id="4a7d6-133">I hello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885), Välj din cloud service eller web app eller skapa en ny genom att välja hello  **+**  längst hello nedre vänstra hörnet och välja **Molntjänsten** eller **Webbapp** och sedan **Snabbregistrering**.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-133">In hello [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885), select your cloud service or web app, or create a new one by choosing hello **+** icon at hello bottom left and choosing **Cloud Service** or **Web App** and then **Quick Create**.</span></span> <span data-ttu-id="4a7d6-134">Välj hello **Konfigurera publicering med Visual Studio Team Services** länk.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-134">Choose hello **Set up publishing with Visual Studio Team Services** link.</span></span>
   
    ![][10]
2. <span data-ttu-id="4a7d6-135">Hello i guiden anger hello namnet på ditt konto i Visual Studio Team Services hello textrutan och klickar på hello **auktorisera nu** länk.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-135">In hello wizard, type hello name of your Visual Studio Team Services account in hello textbox and click hello **Authorize Now** link.</span></span> <span data-ttu-id="4a7d6-136">Du kan uppmanas toosign i.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-136">You might be asked toosign in.</span></span>
   
    ![][11]
3. <span data-ttu-id="4a7d6-137">I hello **anslutningsbegäran** popup-fönstret Välj hello **acceptera** knappen tooauthorize Azure tooconfigure ditt team projekt i VS Team Services.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-137">In hello **Connection Request** pop-up dialog, choose hello **Accept** button tooauthorize Azure tooconfigure your team project in VS Team Services.</span></span>
   
    ![][12]
4. <span data-ttu-id="4a7d6-138">När tillståndet lyckas, ser du en listruta som innehåller en lista över dina Visual Studio Team Services projekt.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-138">When authorization succeeds, you see a dropdown containing a list of your Visual Studio Team Services team projects.</span></span> <span data-ttu-id="4a7d6-139">Välj hello namnet på grupprojekt som du skapade i föregående steg i hello och välj hello guiden markering.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-139">Choose  hello name of team project that you created in hello previous steps, and then choose hello wizard's checkmark button.</span></span>
   
    ![][13]
5. <span data-ttu-id="4a7d6-140">När projektet har länkats visas några instruktioner för att kontrollera i ändringar tooyour Visual Studio Team Services grupprojekt.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-140">After your project is linked, you will see some instructions for checking in changes tooyour Visual Studio Team Services team project.</span></span>  <span data-ttu-id="4a7d6-141">På din nästa incheckning, Visual Studio Team Services skapar och distribuerar ditt projekt tooAzure.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-141">On your next check-in, Visual Studio Team Services will build and deploy your project tooAzure.</span></span>  <span data-ttu-id="4a7d6-142">Prova den här nu genom att klicka på hello **checka In från Visual Studio** länka och sedan hello **starta Visual Studio** länk (eller motsvarande hello **Visual Studio** knappen längst ned hello hello portal skärmen).</span><span class="sxs-lookup"><span data-stu-id="4a7d6-142">Try this now by clicking hello **Check In from Visual Studio** link, and then hello **Launch Visual Studio** link (or hello equivalent **Visual Studio** button at hello bottom of hello portal screen).</span></span>
   
    ![][14]

## <a name="4-trigger-a-rebuild-and-redeploy-your-project"></a><span data-ttu-id="4a7d6-143">4: utlösa en återskapning och omdistribuera ditt projekt</span><span class="sxs-lookup"><span data-stu-id="4a7d6-143">4: Trigger a rebuild and redeploy your project</span></span>
1. <span data-ttu-id="4a7d6-144">I Visual Studio **Team Explorer**, Välj hello **källa kontrollen Explorer** länk.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-144">In Visual Studio's **Team Explorer**, choose hello **Source Control Explorer** link.</span></span>
   
    ![][15]
2. <span data-ttu-id="4a7d6-145">Navigera tooyour lösningsfilen och öppna den.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-145">Navigate tooyour solution file and open it.</span></span>
   
    ![][16]
3. <span data-ttu-id="4a7d6-146">I **Solution Explorer**, öppna upp en fil och ändra den.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-146">In **Solution Explorer**, open up a file and change it.</span></span> <span data-ttu-id="4a7d6-147">Till exempel ändra hello filen `_Layout.cshtml` under hello vyer\\delad mapp i en MVC-webbroll.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-147">For example, change hello file `_Layout.cshtml` under hello Views\\Shared folder in an MVC web role.</span></span>
   
    ![][17]
4. <span data-ttu-id="4a7d6-148">Redigera hello logotyp för hello webbplats och väljer **Ctrl + S** toosave hello-filen.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-148">Edit hello logo for hello site and choose **Ctrl+S** toosave hello file.</span></span>
   
    ![][18]
5. <span data-ttu-id="4a7d6-149">I **Team Explorer**, Välj hello **väntande ändringar** länk.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-149">In **Team Explorer**, choose hello **Pending Changes** link.</span></span>
   
    ![][19]
6. <span data-ttu-id="4a7d6-150">Ange en kommentar och välj hello **checka In** knappen.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-150">Enter a comment and then choose hello **Check In** button.</span></span>
   
    ![][20]
7. <span data-ttu-id="4a7d6-151">Välj hello **hem** knappen tooreturn toohello **Team Explorer** startsidan.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-151">Choose hello **Home** button tooreturn toohello **Team Explorer** home page.</span></span>
   
    ![][21]
8. <span data-ttu-id="4a7d6-152">Välj hello **bygger** länk tooview hello bygger pågår.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-152">Choose hello **Builds** link tooview hello builds in progress.</span></span>
   
    ![][22]
   
    <span data-ttu-id="4a7d6-153">**Teamet Explorer** visar att en version har utlösts för din incheckning.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-153">**Team Explorer** shows that a build has been triggered for your check-in.</span></span>
   
    ![][23]
9. <span data-ttu-id="4a7d6-154">Dubbelklicka hello namnet på hello skapa i förloppet tooview detaljerade loggfil som hello build fortskrider.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-154">Double-click hello name of hello build in progress tooview a detailed log as hello build progresses.</span></span>
   
    ![][24]
10. <span data-ttu-id="4a7d6-155">Hello build är pågående, ta en titt på hello build definition som skapades när du länkade TFS tooAzure med hjälp av guiden hello.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-155">While hello build is in-progress, take a look at hello build definition that was created when you linked TFS tooAzure by using hello wizard.</span></span>  <span data-ttu-id="4a7d6-156">Öppna hello snabbmenyn för hello build definition och välj **redigera skapa Definition**.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-156">Open hello shortcut menu for hello build definition and choose **Edit Build Definition**.</span></span>
    
     ![][25]
    
     <span data-ttu-id="4a7d6-157">I hello **utlösaren** fliken ser du att hello build definition är standardinställningen toobuild på varje incheckning.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-157">In hello **Trigger** tab, you will see that hello build definition is set toobuild on every check-in by default.</span></span>
    
     ![][26]
    
     <span data-ttu-id="4a7d6-158">I hello **processen** fliken visas hello distributionsmiljö anges toohello namnet på din cloud service eller web app.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-158">In hello **Process** tab, you can see hello deployment environment is set toohello name of your cloud service or web app.</span></span> <span data-ttu-id="4a7d6-159">Om du arbetar med webbappar skilja hello egenskaper som du ser sig från de som visas här.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-159">If you are working with web apps, hello properties you see will be different from those shown here.</span></span>
    
     ![][27]
11. <span data-ttu-id="4a7d6-160">Ange värden för hello egenskaper om du vill att andra värden än hello standardvärden.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-160">Specify values for hello properties if you want different values than hello defaults.</span></span> <span data-ttu-id="4a7d6-161">hello egenskaper för Azure publicering finns i hello **distribution** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-161">hello properties for Azure publishing are in hello **Deployment** section.</span></span>
    
     <span data-ttu-id="4a7d6-162">hello följande tabell visar hello tillgängliga egenskaper i hello **distribution** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="4a7d6-162">hello following table shows hello available properties in hello **Deployment** section:</span></span>
    
    | <span data-ttu-id="4a7d6-163">Egenskap</span><span class="sxs-lookup"><span data-stu-id="4a7d6-163">Property</span></span> | <span data-ttu-id="4a7d6-164">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="4a7d6-164">Default Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="4a7d6-165">Tillåt ej betrodda certifikat</span><span class="sxs-lookup"><span data-stu-id="4a7d6-165">Allow Untrusted Certificates</span></span> |<span data-ttu-id="4a7d6-166">Om värdet är false måste SSL-certifikat signeras av en rotcertifikatutfärdare.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-166">If false, SSL certificates must be signed by a root authority.</span></span> |
    | <span data-ttu-id="4a7d6-167">Tillåt uppgradering</span><span class="sxs-lookup"><span data-stu-id="4a7d6-167">Allow Upgrade</span></span> |<span data-ttu-id="4a7d6-168">Tillåter hello distribution tooupdate en befintlig distribution i stället för att skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-168">Allows hello deployment tooupdate an existing deployment instead of creating a new one.</span></span> <span data-ttu-id="4a7d6-169">Bevarar hello IP-adress.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-169">Preserves hello IP address.</span></span> |
    | <span data-ttu-id="4a7d6-170">Ta inte bort</span><span class="sxs-lookup"><span data-stu-id="4a7d6-170">Do Not Delete</span></span> |<span data-ttu-id="4a7d6-171">Om värdet är true, Skriv inte över en befintlig orelaterade distribution (uppgraderingen tillåts).</span><span class="sxs-lookup"><span data-stu-id="4a7d6-171">If true, do not overwrite an existing unrelated deployment (upgrade is allowed).</span></span> |
    | <span data-ttu-id="4a7d6-172">Sökvägen tooDeployment inställningar</span><span class="sxs-lookup"><span data-stu-id="4a7d6-172">Path tooDeployment Settings</span></span> |<span data-ttu-id="4a7d6-173">hello sökväg tooyour .pubxml fil för en webbapp, relativa toohello rotmapp hello lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-173">hello path tooyour .pubxml file for a web app, relative toohello root folder of hello repo.</span></span> <span data-ttu-id="4a7d6-174">Ignoreras för molntjänster.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-174">Ignored for cloud services.</span></span> |
    | <span data-ttu-id="4a7d6-175">SharePoint-miljön för distribution</span><span class="sxs-lookup"><span data-stu-id="4a7d6-175">Sharepoint Deployment Environment</span></span> |<span data-ttu-id="4a7d6-176">Hej samma som hello tjänstnamn.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-176">hello same as hello service name.</span></span> |
    | <span data-ttu-id="4a7d6-177">Av Azure-distributionsmiljö</span><span class="sxs-lookup"><span data-stu-id="4a7d6-177">Azure Deployment Environment</span></span> |<span data-ttu-id="4a7d6-178">hello web app eller molnet tjänstnamn.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-178">hello web app or cloud service name.</span></span> |
12. <span data-ttu-id="4a7d6-179">Om du använder flera konfigurationer (.cscfg-filer), kan du ange hello önskad konfiguration i hello **bygga, Avancerat MSBuild-argument** inställningen.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-179">If you are using multiple service configurations (.cscfg files), you can specify hello desired service configuration in hello **Build, Advanced, MSBuild arguments** setting.</span></span> <span data-ttu-id="4a7d6-180">Till exempel ange toouse ServiceConfiguration.Test.cscfg, MSBuild-argument alternativet `/p:TargetProfile=Test`.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-180">For example, toouse ServiceConfiguration.Test.cscfg, set MSBuild arguments line option `/p:TargetProfile=Test`.</span></span>
    
     ![][38]
    
     <span data-ttu-id="4a7d6-181">Vid den tidpunkten ska din build slutföras.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-181">By this time, your build should be completed successfully.</span></span>
    
     ![][28]
13. <span data-ttu-id="4a7d6-182">Om du dubbelklickar på hello build namn, Visual Studio visas en **skapa sammanfattning**, inklusive alla resultat från associerade testprojekt.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-182">If you double-click hello build name, Visual Studio shows a **Build Summary**, including any test results from associated unit test projects.</span></span>
    
     ![][29]
14. <span data-ttu-id="4a7d6-183">I hello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885), du kan visa hello associerad distribution på hello **distributioner** fliken när hello mellanlagring miljön är markerad.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-183">In hello [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885), you can view hello associated deployment on hello **Deployments** tab when hello staging environment is selected.</span></span>
    
     ![][30]
15. <span data-ttu-id="4a7d6-184">Bläddra tooyour webbplatsens URL.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-184">Browse tooyour site's URL.</span></span> <span data-ttu-id="4a7d6-185">För en webbapp och klicka bara på hello **Bläddra** hello kommandofältet-knappen.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-185">For a web app, just click hello **Browse** button on hello command bar.</span></span> <span data-ttu-id="4a7d6-186">Välj hello URL för en molntjänst i hello **snabb i korthet** avsnitt i hello **instrumentpanelen** sidan som visar hello mellanlagringsmiljön för en tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-186">For a cloud service, choose hello URL in hello **Quick Glance** section of hello **Dashboard** page that shows hello Staging environment for a cloud service.</span></span> <span data-ttu-id="4a7d6-187">Distributioner från kontinuerlig integration för molntjänster är publicerade toohello mellanlagringsmiljön som standard.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-187">Deployments from continuous integration for cloud services are published toohello Staging environment by default.</span></span> <span data-ttu-id="4a7d6-188">Du kan ändra detta genom att ange hello **alternativa Molntjänstmiljö** egenskapen för**produktion**.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-188">You can change this by setting hello **Alternate Cloud Service Environment** property too**Production**.</span></span> <span data-ttu-id="4a7d6-189">Den här skärmbilden visar där hello Webbadress är på hello molnbaserad tjänst instrumentpanelssida.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-189">This screenshot shows where hello site URL is on hello cloud service's dashboard page.</span></span>
    
    ![][31]
    
    <span data-ttu-id="4a7d6-190">En ny webbläsarflik öppnas tooreveal webbplatsen körs.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-190">A new browser tab will open tooreveal your running site.</span></span>
    
    ![][32]
    
    <span data-ttu-id="4a7d6-191">För molntjänster, om du gör andra ändringar tooyour projekt bygger mer du utlösare och du samlar flera distributioner.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-191">For cloud services, if you make other changes tooyour project, you trigger more builds, and you will accumulate multiple deployments.</span></span> <span data-ttu-id="4a7d6-192">hello senaste markerats som aktiv.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-192">hello latest one marked as Active.</span></span>
    
    ![][33]

## <a name="5-redeploy-an-earlier-build"></a><span data-ttu-id="4a7d6-193">5: Distribuera om en tidigare version</span><span class="sxs-lookup"><span data-stu-id="4a7d6-193">5: Redeploy an earlier build</span></span>
<span data-ttu-id="4a7d6-194">Det här steget gäller toocloud tjänster och är valfritt.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-194">This step applies toocloud services and is optional.</span></span> <span data-ttu-id="4a7d6-195">I hello klassiska Azure-portalen, Välj en tidigare distribution och sedan hello **omdistribuera** knappen toorewind din webbplats tooan tidigare incheckning.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-195">In hello Azure classic portal, choose an earlier deployment and then choose hello **Redeploy** button toorewind your site tooan earlier check-in.</span></span>  <span data-ttu-id="4a7d6-196">Observera att detta utlöser en ny version i TFS och skapa en ny post i distributionshistoriken.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-196">Note that this will trigger a new build in TFS and create a new entry in your deployment history.</span></span>

![][34]

## <a name="6-change-hello-production-deployment"></a><span data-ttu-id="4a7d6-197">6: ändra hello Produktionsdistribution</span><span class="sxs-lookup"><span data-stu-id="4a7d6-197">6: Change hello Production deployment</span></span>
<span data-ttu-id="4a7d6-198">Det här steget gäller endast toocloud tjänster, inte webbprogram.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-198">This step applies only toocloud services, not web apps.</span></span> <span data-ttu-id="4a7d6-199">När du är klar kan du befordra hello mellanlagring toohello produktionsmiljön genom att välja hello **växla** knapp i hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-199">When you are ready, you can promote hello Staging environment toohello production environment by choosing hello **Swap** button in hello Azure classic portal.</span></span> <span data-ttu-id="4a7d6-200">hello tidigare produktionsmiljö eventuella blir en mellanlagringsmiljön hello nyligen distribuerade mellanlagringsmiljön är upphöjt tooProduction.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-200">hello newly deployed Staging environment is promoted tooProduction, and hello previous Production environment, if any, becomes a Staging environment.</span></span> <span data-ttu-id="4a7d6-201">hello aktiv distribution kan vara olika för hello produktions- och mellanlagringsmiljöer, men hello distributionshistoriken för senare versioner är hello samma oavsett miljö.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-201">hello Active deployment may be different for hello Production and Staging environments, but hello deployment history of recent builds is hello same regardless of environment.</span></span>

![][35]

## <a name="7-run-unit-tests"></a><span data-ttu-id="4a7d6-202">7: kör kontroller</span><span class="sxs-lookup"><span data-stu-id="4a7d6-202">7: Run unit tests</span></span>
<span data-ttu-id="4a7d6-203">Det här steget gäller endast tooweb appar, inte molntjänster.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-203">This step applies only tooweb apps, not cloud services.</span></span> <span data-ttu-id="4a7d6-204">tooput en kvalitet gate på din distribution, kan du köra kontroller och om de inte kan du stoppa hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-204">tooput a quality gate on your deployment, you can run unit tests and if they fail, you can stop hello deployment.</span></span>

1. <span data-ttu-id="4a7d6-205">Lägga till en enhet test-projekt i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-205">In Visual Studio, add a unit test project.</span></span>
   
   ![][39]
2. <span data-ttu-id="4a7d6-206">Lägg till projektet referenser toohello projektet som du vill tootest.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-206">Add project references toohello project you want tootest.</span></span>
   
   ![][40]
3. <span data-ttu-id="4a7d6-207">Lägga till vissa kontroller.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-207">Add some unit tests.</span></span> <span data-ttu-id="4a7d6-208">tooget igång, försök ett dummy test som släpps alltid.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-208">tooget started, try a dummy test that will always pass.</span></span>
   
       ```
       using System;
       using Microsoft.VisualStudio.TestTools.UnitTesting;
   
       namespace UnitTestProject1
       {
           [TestClass]
           public class UnitTest1
           {
               [TestMethod]
               [ExpectedException(typeof(NotImplementedException))]
               public void TestMethod1()
               {
                   throw new NotImplementedException();
               }
           }
       }
       ```
4. <span data-ttu-id="4a7d6-209">Redigera definition av hello build, Välj hello **processen** fliken och expandera hello **Test** nod.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-209">Edit hello build definition, choose hello **Process** tab, and expand hello **Test** node.</span></span>
5. <span data-ttu-id="4a7d6-210">Ange hello **misslyckas bygger vidare testet misslyckades** tooTrue.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-210">Set hello **Fail build on test failure** tooTrue.</span></span> <span data-ttu-id="4a7d6-211">Detta innebär att hello distributionen inte uppstår om hello tester lyckas.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-211">This means that hello deployment won't occur unless hello tests pass.</span></span>
   
   ![][41]
6. <span data-ttu-id="4a7d6-212">Kön en ny version.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-212">Queue a new build.</span></span>
   
   ![][42]
   
   ![][43]
7. <span data-ttu-id="4a7d6-213">Medan hello build fortsätter, kontrollera förloppet.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-213">While hello build is proceeding, check on its progress.</span></span>
   
    ![][44]
   
    ![][45]
8. <span data-ttu-id="4a7d6-214">Kontrollera hello testresultaten när hello build är klar.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-214">When hello build is done, check hello test results.</span></span>
   
    ![][46]
   
    ![][47]
9. <span data-ttu-id="4a7d6-215">Försök att skapa ett test som misslyckas.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-215">Try creating a test that will fail.</span></span> <span data-ttu-id="4a7d6-216">Lägg till ett nytt test genom att kopiera hello första, byta namn på den och kommentera ut hello kodrad om NotImplementedException är ett förväntat undantag.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-216">Add a new test by copying hello first one, rename it, and comment out hello line of code that states NotImplementedException is an expected exception.</span></span>
   
       ```
       [TestMethod]
       //[ExpectedException(typeof(NotImplementedException))]
       public void TestMethod2()
       {
           throw new NotImplementedException();
       }
       ```
10. <span data-ttu-id="4a7d6-217">Incheckning hello ändra tooqueue en ny version.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-217">Check in hello change tooqueue a new build.</span></span>
    
     ![][48]
11. <span data-ttu-id="4a7d6-218">Visa hello test resultat toosee information om hello-fel.</span><span class="sxs-lookup"><span data-stu-id="4a7d6-218">View hello test results toosee details about hello failure.</span></span>
    
     ![][49]
    
     ![][50]

## <a name="next-steps"></a><span data-ttu-id="4a7d6-219">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4a7d6-219">Next steps</span></span>
<span data-ttu-id="4a7d6-220">Mer information om enhet testning i Visual Studio Team Services finns i [köra kontroller i din build](http://go.microsoft.com/fwlink/p/?LinkId=510474).</span><span class="sxs-lookup"><span data-stu-id="4a7d6-220">For more about unit testing in Visual Studio Team Services, see [Run unit tests in your build](http://go.microsoft.com/fwlink/p/?LinkId=510474).</span></span> <span data-ttu-id="4a7d6-221">Om du använder Git finns [dela din kod i Git](http://www.visualstudio.com/get-started/share-your-code-in-git-vs.aspx) och [kontinuerlig distribution tooAzure Apptjänst](../app-service-web/app-service-continuous-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="4a7d6-221">If you're using Git, see [Share your code in Git](http://www.visualstudio.com/get-started/share-your-code-in-git-vs.aspx) and [Continuous deployment tooAzure App Service](../app-service-web/app-service-continuous-deployment.md).</span></span>  <span data-ttu-id="4a7d6-222">Läs mer om Visual Studio Team Services [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).</span><span class="sxs-lookup"><span data-stu-id="4a7d6-222">For more information about Visual Studio Team Services, see [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).</span></span>

[0]: ./media/cloud-services-continuous-delivery-use-vso/tfs0.PNG
[1]: ./media/cloud-services-continuous-delivery-use-vso/tfs1.png
[2]: ./media/cloud-services-continuous-delivery-use-vso/tfs2.png

[5]: ./media/cloud-services-continuous-delivery-use-vso/tfs5.png
[6]: ./media/cloud-services-continuous-delivery-use-vso/tfs6.png
[7]: ./media/cloud-services-continuous-delivery-use-vso/tfs7.png
[8]: ./media/cloud-services-continuous-delivery-use-vso/tfs8.png
[9]: ./media/cloud-services-continuous-delivery-use-vso/tfs9.png
[10]: ./media/cloud-services-continuous-delivery-use-vso/tfs10.png
[11]: ./media/cloud-services-continuous-delivery-use-vso/tfs11.png
[12]: ./media/cloud-services-continuous-delivery-use-vso/tfs12.png
[13]: ./media/cloud-services-continuous-delivery-use-vso/tfs13.png
[14]: ./media/cloud-services-continuous-delivery-use-vso/tfs14.png
[15]: ./media/cloud-services-continuous-delivery-use-vso/tfs15.png
[16]: ./media/cloud-services-continuous-delivery-use-vso/tfs16.png
[17]: ./media/cloud-services-continuous-delivery-use-vso/tfs17.png
[18]: ./media/cloud-services-continuous-delivery-use-vso/tfs18.png
[19]: ./media/cloud-services-continuous-delivery-use-vso/tfs19.png
[20]: ./media/cloud-services-continuous-delivery-use-vso/tfs20.png
[21]: ./media/cloud-services-continuous-delivery-use-vso/tfs21.png
[22]: ./media/cloud-services-continuous-delivery-use-vso/tfs22.png
[23]: ./media/cloud-services-continuous-delivery-use-vso/tfs23.png
[24]: ./media/cloud-services-continuous-delivery-use-vso/tfs24.png
[25]: ./media/cloud-services-continuous-delivery-use-vso/tfs25.png
[26]: ./media/cloud-services-continuous-delivery-use-vso/tfs26.png
[27]: ./media/cloud-services-continuous-delivery-use-vso/tfs27.png
[28]: ./media/cloud-services-continuous-delivery-use-vso/tfs28.png
[29]: ./media/cloud-services-continuous-delivery-use-vso/tfs29.png
[30]: ./media/cloud-services-continuous-delivery-use-vso/tfs30.png
[31]: ./media/cloud-services-continuous-delivery-use-vso/tfs31.png
[32]: ./media/cloud-services-continuous-delivery-use-vso/tfs32.png
[33]: ./media/cloud-services-continuous-delivery-use-vso/tfs33.png
[34]: ./media/cloud-services-continuous-delivery-use-vso/tfs34.png
[35]: ./media/cloud-services-continuous-delivery-use-vso/tfs35.png
[36]: ./media/cloud-services-continuous-delivery-use-vso/tfs36.PNG
[37]: ./media/cloud-services-continuous-delivery-use-vso/tfs37.PNG
[38]: ./media/cloud-services-continuous-delivery-use-vso/AdvancedMSBuildSettings.PNG
[39]: ./media/cloud-services-continuous-delivery-use-vso/AddUnitTestProject.PNG
[40]: ./media/cloud-services-continuous-delivery-use-vso/AddProjectReferences.PNG
[41]: ./media/cloud-services-continuous-delivery-use-vso/EditBuildDefinitionForTest.PNG
[42]: ./media/cloud-services-continuous-delivery-use-vso/QueueNewBuild.PNG
[43]: ./media/cloud-services-continuous-delivery-use-vso/QueueBuildDialog.PNG
[44]: ./media/cloud-services-continuous-delivery-use-vso/BuildInTeamExplorer.PNG
[45]: ./media/cloud-services-continuous-delivery-use-vso/BuildRequestInfo.PNG
[46]: ./media/cloud-services-continuous-delivery-use-vso/BuildSucceeded.PNG
[47]: ./media/cloud-services-continuous-delivery-use-vso/TestResultsPassed.PNG
[48]: ./media/cloud-services-continuous-delivery-use-vso/CheckInChangeToMakeTestsFail.PNG
[49]: ./media/cloud-services-continuous-delivery-use-vso/TestsFailed.PNG
[50]: ./media/cloud-services-continuous-delivery-use-vso/TestsResultsFailed.PNG
