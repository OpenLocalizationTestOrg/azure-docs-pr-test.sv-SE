---
title: Kontinuerlig leverans med Visual Studio Team Services i Azure | Microsoft Docs
description: "Lär dig hur du konfigurerar din Visual Studio Team Services grupprojekt för att automatiskt skapa och distribuera till funktionen Web App i Azure App Service eller cloud services."
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
ms.openlocfilehash: d80ce63eb7ddfd7c45726be887a772f9a7594b28
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="continuous-delivery-to-azure-using-visual-studio-team-services"></a><span data-ttu-id="62cb2-103">Kontinuerlig leverans till Azure med Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="62cb2-103">Continuous delivery to Azure using Visual Studio Team Services</span></span>
<span data-ttu-id="62cb2-104">Du kan konfigurera dina team projekt i Visual Studio Team Services för att automatiskt skapa och distribuera till Azure-webbappar eller molntjänster.</span><span class="sxs-lookup"><span data-stu-id="62cb2-104">You can configure your Visual Studio Team Services team projects to automatically build and deploy to Azure web apps or cloud services.</span></span>  <span data-ttu-id="62cb2-105">(Mer information om hur du ställer in en kontinuerlig version och distribuera system med hjälp av en *lokalt* Team Foundation Server finns [kontinuerlig leverans för molntjänster i Azure](cloud-services-dotnet-continuous-delivery.md).)</span><span class="sxs-lookup"><span data-stu-id="62cb2-105">(For information on how to set up a continuous build and deploy system using an *on-premises* Team Foundation Server, see [Continuous Delivery for Cloud Services in Azure](cloud-services-dotnet-continuous-delivery.md).)</span></span>

<span data-ttu-id="62cb2-106">Den här kursen förutsätter att du har Visual Studio 2013 och Azure SDK om du installerade.</span><span class="sxs-lookup"><span data-stu-id="62cb2-106">This tutorial assumes you have Visual Studio 2013 and the Azure SDK installed.</span></span> <span data-ttu-id="62cb2-107">Om du inte redan har Visual Studio 2013 kan du hämta det genom att välja den **Kom igång gratis** länka när [www.visualstudio.com](http://www.visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="62cb2-107">If you don't already have Visual Studio 2013, download it by choosing the **Get started for free** link at [www.visualstudio.com](http://www.visualstudio.com).</span></span> <span data-ttu-id="62cb2-108">Installera Azure SDK från [här](http://go.microsoft.com/fwlink/?LinkId=239540).</span><span class="sxs-lookup"><span data-stu-id="62cb2-108">Install the Azure SDK from [here](http://go.microsoft.com/fwlink/?LinkId=239540).</span></span>

> [!NOTE]
> <span data-ttu-id="62cb2-109">Du behöver ett Visual Studio Team Services-konto för att slutföra den här självstudiekursen: du kan [öppnar ett Visual Studio Team Services-konto gratis](http://go.microsoft.com/fwlink/p/?LinkId=512979).</span><span class="sxs-lookup"><span data-stu-id="62cb2-109">You need an Visual Studio Team Services account to complete this tutorial: You can [open a Visual Studio Team Services account for free](http://go.microsoft.com/fwlink/p/?LinkId=512979).</span></span>
> 
> 

<span data-ttu-id="62cb2-110">Följ dessa steg om du vill konfigurera en molntjänst för att automatiskt skapa och distribuera till Azure med hjälp av Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="62cb2-110">To set up a cloud service to automatically build and deploy to Azure by using Visual Studio Team Services, follow these steps.</span></span>

## <a name="1-create-a-team-project"></a><span data-ttu-id="62cb2-111">1: skapa ett team projekt</span><span class="sxs-lookup"><span data-stu-id="62cb2-111">1: Create a team project</span></span>
<span data-ttu-id="62cb2-112">Följ instruktionerna [här](http://go.microsoft.com/fwlink/?LinkId=512980) att skapa projektet team och länka det till Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="62cb2-112">Follow the instructions [here](http://go.microsoft.com/fwlink/?LinkId=512980) to create your team project and link it to Visual Studio.</span></span> <span data-ttu-id="62cb2-113">Den här genomgången förutsätter att du använder Team Foundation Version kontrollen (TFVC) som käll-kontroll lösning.</span><span class="sxs-lookup"><span data-stu-id="62cb2-113">This walkthrough assumes you are using Team Foundation Version Control (TFVC) as your source control solution.</span></span> <span data-ttu-id="62cb2-114">Om du vill använda Git för versionskontroll, se [Git-versionen av den här genomgången](http://go.microsoft.com/fwlink/p/?LinkId=397358).</span><span class="sxs-lookup"><span data-stu-id="62cb2-114">If you want to use Git for version control, see [the Git version of this walkthrough](http://go.microsoft.com/fwlink/p/?LinkId=397358).</span></span>

## <a name="2-check-in-a-project-to-source-control"></a><span data-ttu-id="62cb2-115">2: Kontrollera i ett projekt till källkontroll</span><span class="sxs-lookup"><span data-stu-id="62cb2-115">2: Check in a project to source control</span></span>
1. <span data-ttu-id="62cb2-116">Öppnar du vill distribuera lösningen i Visual Studio eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="62cb2-116">In Visual Studio, open the solution you want to deploy, or create a new one.</span></span>
   <span data-ttu-id="62cb2-117">Du kan distribuera en webbapp eller en tjänst i molnet (Azure-program) genom att följa stegen i den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="62cb2-117">You can deploy a web app or a cloud service (Azure Application) by following the steps in this walkthrough.</span></span>
   <span data-ttu-id="62cb2-118">Om du vill skapa en ny lösning, skapa ett nytt Azure Cloud Service-projekt, eller ett nytt ASP.NET MVC-projekt.</span><span class="sxs-lookup"><span data-stu-id="62cb2-118">If you want to create a new solution, create a new Azure Cloud Service project, or a new ASP.NET MVC project.</span></span> <span data-ttu-id="62cb2-119">Kontrollera att projektet riktar sig till .NET Framework 4 eller 4.5 och om du skapar ett molntjänstprojekt, lägga till en ASP.NET MVC-webbroll och en arbetsroll och välj Internetprogram för webbrollen.</span><span class="sxs-lookup"><span data-stu-id="62cb2-119">Make sure that the project targets .NET Framework 4 or 4.5, and if you are creating a cloud service project, add an ASP.NET MVC web role and a worker role, and choose Internet application for the web role.</span></span> <span data-ttu-id="62cb2-120">Välj **Internetprogram**.</span><span class="sxs-lookup"><span data-stu-id="62cb2-120">When prompted, choose **Internet Application**.</span></span>
   <span data-ttu-id="62cb2-121">Om du vill skapa en webbapp Välj projektmall för ASP.NET-webbprogram och sedan välja MVC.</span><span class="sxs-lookup"><span data-stu-id="62cb2-121">If you want to create a web app, choose the ASP.NET Web Application project template, and then choose MVC.</span></span> <span data-ttu-id="62cb2-122">Se [skapa en ASP.NET-webbapp i Azure App Service](../app-service-web/app-service-web-get-started-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="62cb2-122">See [Create an ASP.NET web app in Azure App Service](../app-service-web/app-service-web-get-started-dotnet.md).</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="62cb2-123">Visual Studio Team Services har endast stöd för CI-distributioner av Visual Studio-webbprogram just nu.</span><span class="sxs-lookup"><span data-stu-id="62cb2-123">Visual Studio Team Services only support CI deployments of Visual Studio Web Applications at this time.</span></span> <span data-ttu-id="62cb2-124">Webbplatsen projekt ligger utanför omfånget.</span><span class="sxs-lookup"><span data-stu-id="62cb2-124">Web Site projects are out of scope.</span></span>
   > 
   > 
2. <span data-ttu-id="62cb2-125">Öppna snabbmenyn för lösningen och välj **Lägg till lösning till källkontroll**.</span><span class="sxs-lookup"><span data-stu-id="62cb2-125">Open the context menu for the solution, and choose **Add Solution to Source Control**.</span></span>
   
    ![][5]
3. <span data-ttu-id="62cb2-126">Acceptera eller ändra standardinställningar och välj den **OK** knappen.</span><span class="sxs-lookup"><span data-stu-id="62cb2-126">Accept or change the defaults and choose the **OK** button.</span></span> <span data-ttu-id="62cb2-127">När processen har slutförts ikoner för datakällan visas i **Solution Explorer**.</span><span class="sxs-lookup"><span data-stu-id="62cb2-127">Once the process completes, source control icons appear in **Solution Explorer**.</span></span>
   
    ![][6]
4. <span data-ttu-id="62cb2-128">Öppna snabbmenyn för lösningen och välj **checka In**.</span><span class="sxs-lookup"><span data-stu-id="62cb2-128">Open the shortcut menu for the solution, and choose **Check In**.</span></span>
   
    ![][7]
5. <span data-ttu-id="62cb2-129">I den **väntande ändringar** område i **Team Explorer**, skriver du en kommentar för att checka in och välj den **checka In** knappen.</span><span class="sxs-lookup"><span data-stu-id="62cb2-129">In the **Pending Changes** area of **Team Explorer**, type a comment for the check-in and choose the **Check In** button.</span></span>
   
    ![][8]
   
    <span data-ttu-id="62cb2-130">Observera de alternativ som ska tas med eller undanta specifika ändringar när du checkar in.</span><span class="sxs-lookup"><span data-stu-id="62cb2-130">Note the options to include or exclude specific changes when you check in.</span></span> <span data-ttu-id="62cb2-131">Om du vill ändringar utesluts, Välj den **inkluderar alla** länk.</span><span class="sxs-lookup"><span data-stu-id="62cb2-131">If desired changes are excluded, choose the **Include All** link.</span></span>
   
    ![][9]

## <a name="3-connect-the-project-to-azure"></a><span data-ttu-id="62cb2-132">3: Anslut projektet till Azure</span><span class="sxs-lookup"><span data-stu-id="62cb2-132">3: Connect the project to Azure</span></span>
1. <span data-ttu-id="62cb2-133">Nu när du har ett VS Team Services team projekt med vissa källkoden i den är du redo att ansluta din grupprojekt till Azure.</span><span class="sxs-lookup"><span data-stu-id="62cb2-133">Now that you have a VS Team Services team project with some source code in it, you are ready to connect your team project to Azure.</span></span>  <span data-ttu-id="62cb2-134">I den [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885), Välj din cloud service eller web app eller skapa en ny genom att välja den  **+**  ikonen längst ned vänster och välja **Molntjänsten** eller **Web App** och sedan **Snabbregistrering**.</span><span class="sxs-lookup"><span data-stu-id="62cb2-134">In the [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885), select your cloud service or web app, or create a new one by choosing the **+** icon at the bottom left and choosing **Cloud Service** or **Web App** and then **Quick Create**.</span></span> <span data-ttu-id="62cb2-135">Välj den **Konfigurera publicering med Visual Studio Team Services** länk.</span><span class="sxs-lookup"><span data-stu-id="62cb2-135">Choose the **Set up publishing with Visual Studio Team Services** link.</span></span>
   
    ![][10]
2. <span data-ttu-id="62cb2-136">I guiden anger du namnet på din Visual Studio Team Services-konto i textrutan och klicka på den **auktorisera nu** länk.</span><span class="sxs-lookup"><span data-stu-id="62cb2-136">In the wizard, type the name of your Visual Studio Team Services account in the textbox and click the **Authorize Now** link.</span></span> <span data-ttu-id="62cb2-137">Du kan uppmanas att logga in.</span><span class="sxs-lookup"><span data-stu-id="62cb2-137">You might be asked to sign in.</span></span>
   
    ![][11]
3. <span data-ttu-id="62cb2-138">I den **anslutningsbegäran** popup-fönstret väljer du den **acceptera** för att auktorisera Azure för att konfigurera ditt team projekt i VS Team Services.</span><span class="sxs-lookup"><span data-stu-id="62cb2-138">In the **Connection Request** pop-up dialog, choose the **Accept** button to authorize Azure to configure your team project in VS Team Services.</span></span>
   
    ![][12]
4. <span data-ttu-id="62cb2-139">När tillståndet lyckas, ser du en listruta som innehåller en lista över dina Visual Studio Team Services projekt.</span><span class="sxs-lookup"><span data-stu-id="62cb2-139">When authorization succeeds, you see a dropdown containing a list of your Visual Studio Team Services team projects.</span></span> <span data-ttu-id="62cb2-140">Välj namnet på grupprojekt som du skapade i föregående steg och välj guidens markering.</span><span class="sxs-lookup"><span data-stu-id="62cb2-140">Choose  the name of team project that you created in the previous steps, and then choose the wizard's checkmark button.</span></span>
   
    ![][13]
5. <span data-ttu-id="62cb2-141">När projektet har länkats visas några instruktioner för att kontrollera ändringar i projektet Visual Studio Team Services-teamet.</span><span class="sxs-lookup"><span data-stu-id="62cb2-141">After your project is linked, you will see some instructions for checking in changes to your Visual Studio Team Services team project.</span></span>  <span data-ttu-id="62cb2-142">Visual Studio Team Services kommer på din nästa incheckning, skapa och distribuera projektet till Azure.</span><span class="sxs-lookup"><span data-stu-id="62cb2-142">On your next check-in, Visual Studio Team Services will build and deploy your project to Azure.</span></span>  <span data-ttu-id="62cb2-143">Prova den här nu genom att klicka på den **checka In från Visual Studio** länk, och sedan den **starta Visual Studio** länk (eller motsvarande **Visual Studio** knappen längst ned på skärmen portal).</span><span class="sxs-lookup"><span data-stu-id="62cb2-143">Try this now by clicking the **Check In from Visual Studio** link, and then the **Launch Visual Studio** link (or the equivalent **Visual Studio** button at the bottom of the portal screen).</span></span>
   
    ![][14]

## <a name="4-trigger-a-rebuild-and-redeploy-your-project"></a><span data-ttu-id="62cb2-144">4: utlösa en återskapning och omdistribuera ditt projekt</span><span class="sxs-lookup"><span data-stu-id="62cb2-144">4: Trigger a rebuild and redeploy your project</span></span>
1. <span data-ttu-id="62cb2-145">I Visual Studio **Team Explorer**, Välj den **källa kontrollen Explorer** länk.</span><span class="sxs-lookup"><span data-stu-id="62cb2-145">In Visual Studio's **Team Explorer**, choose the **Source Control Explorer** link.</span></span>
   
    ![][15]
2. <span data-ttu-id="62cb2-146">Navigera till din lösningsfilen och öppna den.</span><span class="sxs-lookup"><span data-stu-id="62cb2-146">Navigate to your solution file and open it.</span></span>
   
    ![][16]
3. <span data-ttu-id="62cb2-147">I **Solution Explorer**, öppna upp en fil och ändra den.</span><span class="sxs-lookup"><span data-stu-id="62cb2-147">In **Solution Explorer**, open up a file and change it.</span></span> <span data-ttu-id="62cb2-148">Till exempel ändra filen `_Layout.cshtml` under vyerna\\delad mapp i en MVC-webbroll.</span><span class="sxs-lookup"><span data-stu-id="62cb2-148">For example, change the file `_Layout.cshtml` under the Views\\Shared folder in an MVC web role.</span></span>
   
    ![][17]
4. <span data-ttu-id="62cb2-149">Redigera logotypen för platsen och välj **Ctrl + S** att spara filen.</span><span class="sxs-lookup"><span data-stu-id="62cb2-149">Edit the logo for the site and choose **Ctrl+S** to save the file.</span></span>
   
    ![][18]
5. <span data-ttu-id="62cb2-150">I **Team Explorer**, Välj den **väntande ändringar** länk.</span><span class="sxs-lookup"><span data-stu-id="62cb2-150">In **Team Explorer**, choose the **Pending Changes** link.</span></span>
   
    ![][19]
6. <span data-ttu-id="62cb2-151">Ange en kommentar och välj sedan den **checka In** knappen.</span><span class="sxs-lookup"><span data-stu-id="62cb2-151">Enter a comment and then choose the **Check In** button.</span></span>
   
    ![][20]
7. <span data-ttu-id="62cb2-152">Välj den **hem** vill gå tillbaka till den **Team Explorer** startsidan.</span><span class="sxs-lookup"><span data-stu-id="62cb2-152">Choose the **Home** button to return to the **Team Explorer** home page.</span></span>
   
    ![][21]
8. <span data-ttu-id="62cb2-153">Välj den **bygger** länken om du vill visa versionerna pågår.</span><span class="sxs-lookup"><span data-stu-id="62cb2-153">Choose the **Builds** link to view the builds in progress.</span></span>
   
    ![][22]
   
    <span data-ttu-id="62cb2-154">**Teamet Explorer** visar att en version har utlösts för din incheckning.</span><span class="sxs-lookup"><span data-stu-id="62cb2-154">**Team Explorer** shows that a build has been triggered for your check-in.</span></span>
   
    ![][23]
9. <span data-ttu-id="62cb2-155">Dubbelklicka på namnet på build pågår till en detaljerade loggen när build fortlöper.</span><span class="sxs-lookup"><span data-stu-id="62cb2-155">Double-click the name of the build in progress to view a detailed log as the build progresses.</span></span>
   
    ![][24]
10. <span data-ttu-id="62cb2-156">Versionen är pågående, ta en titt på build-definition som skapades när du har länkat TFS till Azure med hjälp av guiden.</span><span class="sxs-lookup"><span data-stu-id="62cb2-156">While the build is in-progress, take a look at the build definition that was created when you linked TFS to Azure by using the wizard.</span></span>  <span data-ttu-id="62cb2-157">Öppna snabbmenyn för build-definition och välj **redigera skapa Definition**.</span><span class="sxs-lookup"><span data-stu-id="62cb2-157">Open the shortcut menu for the build definition and choose **Edit Build Definition**.</span></span>
    
     ![][25]
    
     <span data-ttu-id="62cb2-158">I den **utlösaren** fliken ser du att skapa definition är standardinställningen att bygga på varje incheckning.</span><span class="sxs-lookup"><span data-stu-id="62cb2-158">In the **Trigger** tab, you will see that the build definition is set to build on every check-in by default.</span></span>
    
     ![][26]
    
     <span data-ttu-id="62cb2-159">I den **processen** fliken visas distributionsmiljö har angetts till namnet på din cloud service eller web app.</span><span class="sxs-lookup"><span data-stu-id="62cb2-159">In the **Process** tab, you can see the deployment environment is set to the name of your cloud service or web app.</span></span> <span data-ttu-id="62cb2-160">Om du arbetar med webbappar skilja egenskaper som du ser sig från de som visas här.</span><span class="sxs-lookup"><span data-stu-id="62cb2-160">If you are working with web apps, the properties you see will be different from those shown here.</span></span>
    
     ![][27]
11. <span data-ttu-id="62cb2-161">Ange värden för egenskaper om du vill att andra värden än standardinställningarna.</span><span class="sxs-lookup"><span data-stu-id="62cb2-161">Specify values for the properties if you want different values than the defaults.</span></span> <span data-ttu-id="62cb2-162">Egenskaperna för Azure-publicering finns i den **distribution** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="62cb2-162">The properties for Azure publishing are in the **Deployment** section.</span></span>
    
     <span data-ttu-id="62cb2-163">I följande tabell visas de tillgängliga egenskaperna i den **distribution** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="62cb2-163">The following table shows the available properties in the **Deployment** section:</span></span>
    
    | <span data-ttu-id="62cb2-164">Egenskap</span><span class="sxs-lookup"><span data-stu-id="62cb2-164">Property</span></span> | <span data-ttu-id="62cb2-165">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="62cb2-165">Default Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="62cb2-166">Tillåt ej betrodda certifikat</span><span class="sxs-lookup"><span data-stu-id="62cb2-166">Allow Untrusted Certificates</span></span> |<span data-ttu-id="62cb2-167">Om värdet är false måste SSL-certifikat signeras av en rotcertifikatutfärdare.</span><span class="sxs-lookup"><span data-stu-id="62cb2-167">If false, SSL certificates must be signed by a root authority.</span></span> |
    | <span data-ttu-id="62cb2-168">Tillåt uppgradering</span><span class="sxs-lookup"><span data-stu-id="62cb2-168">Allow Upgrade</span></span> |<span data-ttu-id="62cb2-169">Gör att distributionen ska uppdatera en befintlig distribution i stället för att skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="62cb2-169">Allows the deployment to update an existing deployment instead of creating a new one.</span></span> <span data-ttu-id="62cb2-170">Bevarar den IP-adressen.</span><span class="sxs-lookup"><span data-stu-id="62cb2-170">Preserves the IP address.</span></span> |
    | <span data-ttu-id="62cb2-171">Ta inte bort</span><span class="sxs-lookup"><span data-stu-id="62cb2-171">Do Not Delete</span></span> |<span data-ttu-id="62cb2-172">Om värdet är true, Skriv inte över en befintlig orelaterade distribution (uppgraderingen tillåts).</span><span class="sxs-lookup"><span data-stu-id="62cb2-172">If true, do not overwrite an existing unrelated deployment (upgrade is allowed).</span></span> |
    | <span data-ttu-id="62cb2-173">Sökvägen till distributionsinställningar</span><span class="sxs-lookup"><span data-stu-id="62cb2-173">Path to Deployment Settings</span></span> |<span data-ttu-id="62cb2-174">Sökvägen till filen .pubxml för en webbapp i förhållande till rotmappen på lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="62cb2-174">The path to your .pubxml file for a web app, relative to the root folder of the repo.</span></span> <span data-ttu-id="62cb2-175">Ignoreras för molntjänster.</span><span class="sxs-lookup"><span data-stu-id="62cb2-175">Ignored for cloud services.</span></span> |
    | <span data-ttu-id="62cb2-176">SharePoint-miljön för distribution</span><span class="sxs-lookup"><span data-stu-id="62cb2-176">Sharepoint Deployment Environment</span></span> |<span data-ttu-id="62cb2-177">Samma som namnet på tjänsten.</span><span class="sxs-lookup"><span data-stu-id="62cb2-177">The same as the service name.</span></span> |
    | <span data-ttu-id="62cb2-178">Av Azure-distributionsmiljö</span><span class="sxs-lookup"><span data-stu-id="62cb2-178">Azure Deployment Environment</span></span> |<span data-ttu-id="62cb2-179">Web app eller molnet tjänstnamnet.</span><span class="sxs-lookup"><span data-stu-id="62cb2-179">The web app or cloud service name.</span></span> |
12. <span data-ttu-id="62cb2-180">Om du använder flera konfigurationer (.cscfg-filer), kan du ange den önskade tjänstinställningar i den **bygga, Avancerat MSBuild-argument** inställningen.</span><span class="sxs-lookup"><span data-stu-id="62cb2-180">If you are using multiple service configurations (.cscfg files), you can specify the desired service configuration in the **Build, Advanced, MSBuild arguments** setting.</span></span> <span data-ttu-id="62cb2-181">Till exempel vill använda ServiceConfiguration.Test.cscfg anger MSBuild-argument alternativet `/p:TargetProfile=Test`.</span><span class="sxs-lookup"><span data-stu-id="62cb2-181">For example, to use ServiceConfiguration.Test.cscfg, set MSBuild arguments line option `/p:TargetProfile=Test`.</span></span>
    
     ![][38]
    
     <span data-ttu-id="62cb2-182">Vid den tidpunkten ska din build slutföras.</span><span class="sxs-lookup"><span data-stu-id="62cb2-182">By this time, your build should be completed successfully.</span></span>
    
     ![][28]
13. <span data-ttu-id="62cb2-183">Om du dubbelklickar på build-namnet, Visual Studio visas en **skapa sammanfattning**, inklusive alla resultat från associerade testprojekt.</span><span class="sxs-lookup"><span data-stu-id="62cb2-183">If you double-click the build name, Visual Studio shows a **Build Summary**, including any test results from associated unit test projects.</span></span>
    
     ![][29]
14. <span data-ttu-id="62cb2-184">I den [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885), du kan visa den associerade distributionen på de **distributioner** fliken när mellanlagringsmiljön är markerad.</span><span class="sxs-lookup"><span data-stu-id="62cb2-184">In the [Azure classic portal](http://go.microsoft.com/fwlink/?LinkID=213885), you can view the associated deployment on the **Deployments** tab when the staging environment is selected.</span></span>
    
     ![][30]
15. <span data-ttu-id="62cb2-185">Gå till webbplatsens URL.</span><span class="sxs-lookup"><span data-stu-id="62cb2-185">Browse to your site's URL.</span></span> <span data-ttu-id="62cb2-186">För en webbapp, klicka bara på den **Bläddra** knappen i kommandofältet.</span><span class="sxs-lookup"><span data-stu-id="62cb2-186">For a web app, just click the **Browse** button on the command bar.</span></span> <span data-ttu-id="62cb2-187">För en tjänst i molnet, väljer du URL-Adressen i den **snabb i korthet** avsnitt i den **instrumentpanelen** sidan som visar mellanlagringsmiljön för en tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="62cb2-187">For a cloud service, choose the URL in the **Quick Glance** section of the **Dashboard** page that shows the Staging environment for a cloud service.</span></span> <span data-ttu-id="62cb2-188">Distributioner från kontinuerlig integration för molntjänster publiceras till mellanlagringsmiljön som standard.</span><span class="sxs-lookup"><span data-stu-id="62cb2-188">Deployments from continuous integration for cloud services are published to the Staging environment by default.</span></span> <span data-ttu-id="62cb2-189">Du kan ändra detta genom att ange den **alternativa Molntjänstmiljö** egenskapen **produktion**.</span><span class="sxs-lookup"><span data-stu-id="62cb2-189">You can change this by setting the **Alternate Cloud Service Environment** property to **Production**.</span></span> <span data-ttu-id="62cb2-190">Den här skärmbilden visas där webbplatsens URL på den molntjänst instrumentpanelssida.</span><span class="sxs-lookup"><span data-stu-id="62cb2-190">This screenshot shows where the site URL is on the cloud service's dashboard page.</span></span>
    
    ![][31]
    
    <span data-ttu-id="62cb2-191">En ny webbläsarflik öppnas för att visa webbplatsen körs.</span><span class="sxs-lookup"><span data-stu-id="62cb2-191">A new browser tab will open to reveal your running site.</span></span>
    
    ![][32]
    
    <span data-ttu-id="62cb2-192">Om du gör andra ändringar i projektet för molntjänster, bygger mer du utlösare och du samlar flera distributioner.</span><span class="sxs-lookup"><span data-stu-id="62cb2-192">For cloud services, if you make other changes to your project, you trigger more builds, and you will accumulate multiple deployments.</span></span> <span data-ttu-id="62cb2-193">Den senaste som markerats som aktiv.</span><span class="sxs-lookup"><span data-stu-id="62cb2-193">The latest one marked as Active.</span></span>
    
    ![][33]

## <a name="5-redeploy-an-earlier-build"></a><span data-ttu-id="62cb2-194">5: Distribuera om en tidigare version</span><span class="sxs-lookup"><span data-stu-id="62cb2-194">5: Redeploy an earlier build</span></span>
<span data-ttu-id="62cb2-195">Det här steget gäller för molntjänster och är valfritt.</span><span class="sxs-lookup"><span data-stu-id="62cb2-195">This step applies to cloud services and is optional.</span></span> <span data-ttu-id="62cb2-196">Välj en tidigare distribution i den klassiska Azure-portalen och välj sedan den **omdistribuera** för att spola platsen till en tidigare incheckning.</span><span class="sxs-lookup"><span data-stu-id="62cb2-196">In the Azure classic portal, choose an earlier deployment and then choose the **Redeploy** button to rewind your site to an earlier check-in.</span></span>  <span data-ttu-id="62cb2-197">Observera att detta utlöser en ny version i TFS och skapa en ny post i distributionshistoriken.</span><span class="sxs-lookup"><span data-stu-id="62cb2-197">Note that this will trigger a new build in TFS and create a new entry in your deployment history.</span></span>

![][34]

## <a name="6-change-the-production-deployment"></a><span data-ttu-id="62cb2-198">6: ändra produktionsdistributionen</span><span class="sxs-lookup"><span data-stu-id="62cb2-198">6: Change the Production deployment</span></span>
<span data-ttu-id="62cb2-199">Det här steget gäller bara för molntjänster, inte webbprogram.</span><span class="sxs-lookup"><span data-stu-id="62cb2-199">This step applies only to cloud services, not web apps.</span></span> <span data-ttu-id="62cb2-200">När du är klar kan du befordra mellanlagringsmiljön till produktionsmiljön genom att välja den **växla** knappen i den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="62cb2-200">When you are ready, you can promote the Staging environment to the production environment by choosing the **Swap** button in the Azure classic portal.</span></span> <span data-ttu-id="62cb2-201">Nyligen distribuerade mellanlagringsmiljön höjs till produktionen och föregående produktionsmiljön, eventuella blir en mellanlagringsmiljön.</span><span class="sxs-lookup"><span data-stu-id="62cb2-201">The newly deployed Staging environment is promoted to Production, and the previous Production environment, if any, becomes a Staging environment.</span></span> <span data-ttu-id="62cb2-202">Aktiv distribution kan vara olika för produktions- och Mellanlagringsmiljöer, men distributionshistoriken för senare versioner är detsamma oavsett miljö.</span><span class="sxs-lookup"><span data-stu-id="62cb2-202">The Active deployment may be different for the Production and Staging environments, but the deployment history of recent builds is the same regardless of environment.</span></span>

![][35]

## <a name="7-run-unit-tests"></a><span data-ttu-id="62cb2-203">7: kör kontroller</span><span class="sxs-lookup"><span data-stu-id="62cb2-203">7: Run unit tests</span></span>
<span data-ttu-id="62cb2-204">Det här steget gäller bara för webbappar, inte molntjänster.</span><span class="sxs-lookup"><span data-stu-id="62cb2-204">This step applies only to web apps, not cloud services.</span></span> <span data-ttu-id="62cb2-205">Du kan köra kontroller för att placera en gate kvalitet på din distribution, och om de inte kan du stoppa distributionen.</span><span class="sxs-lookup"><span data-stu-id="62cb2-205">To put a quality gate on your deployment, you can run unit tests and if they fail, you can stop the deployment.</span></span>

1. <span data-ttu-id="62cb2-206">Lägga till en enhet test-projekt i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="62cb2-206">In Visual Studio, add a unit test project.</span></span>
   
   ![][39]
2. <span data-ttu-id="62cb2-207">Lägg till projektreferenser i projektet som du vill testa.</span><span class="sxs-lookup"><span data-stu-id="62cb2-207">Add project references to the project you want to test.</span></span>
   
   ![][40]
3. <span data-ttu-id="62cb2-208">Lägga till vissa kontroller.</span><span class="sxs-lookup"><span data-stu-id="62cb2-208">Add some unit tests.</span></span> <span data-ttu-id="62cb2-209">Försök ett dummy test som alltid ska gå att komma igång.</span><span class="sxs-lookup"><span data-stu-id="62cb2-209">To get started, try a dummy test that will always pass.</span></span>
   
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
4. <span data-ttu-id="62cb2-210">Redigera build-definition, Välj den **processen** fliken och expandera den **Test** nod.</span><span class="sxs-lookup"><span data-stu-id="62cb2-210">Edit the build definition, choose the **Process** tab, and expand the **Test** node.</span></span>
5. <span data-ttu-id="62cb2-211">Ange den **misslyckas bygger vidare testet misslyckades** till True.</span><span class="sxs-lookup"><span data-stu-id="62cb2-211">Set the **Fail build on test failure** to True.</span></span> <span data-ttu-id="62cb2-212">Detta innebär att distributionen inte uppstår om testet lyckas.</span><span class="sxs-lookup"><span data-stu-id="62cb2-212">This means that the deployment won't occur unless the tests pass.</span></span>
   
   ![][41]
6. <span data-ttu-id="62cb2-213">Kön en ny version.</span><span class="sxs-lookup"><span data-stu-id="62cb2-213">Queue a new build.</span></span>
   
   ![][42]
   
   ![][43]
7. <span data-ttu-id="62cb2-214">Medan bygga fortsätter, kontrollera förloppet.</span><span class="sxs-lookup"><span data-stu-id="62cb2-214">While the build is proceeding, check on its progress.</span></span>
   
    ![][44]
   
    ![][45]
8. <span data-ttu-id="62cb2-215">Kontrollera testresultaten när bygga är klar.</span><span class="sxs-lookup"><span data-stu-id="62cb2-215">When the build is done, check the test results.</span></span>
   
    ![][46]
   
    ![][47]
9. <span data-ttu-id="62cb2-216">Försök att skapa ett test som misslyckas.</span><span class="sxs-lookup"><span data-stu-id="62cb2-216">Try creating a test that will fail.</span></span> <span data-ttu-id="62cb2-217">Lägg till ett nytt test genom att kopiera förstnämnda, byta namn på den och kommentera ut koden om NotImplementedException är ett förväntat undantag för.</span><span class="sxs-lookup"><span data-stu-id="62cb2-217">Add a new test by copying the first one, rename it, and comment out the line of code that states NotImplementedException is an expected exception.</span></span>
   
       ```
       [TestMethod]
       //[ExpectedException(typeof(NotImplementedException))]
       public void TestMethod2()
       {
           throw new NotImplementedException();
       }
       ```
10. <span data-ttu-id="62cb2-218">Kontrollera i Ändra till en ny version-kö.</span><span class="sxs-lookup"><span data-stu-id="62cb2-218">Check in the change to queue a new build.</span></span>
    
     ![][48]
11. <span data-ttu-id="62cb2-219">Visa testresultaten om du vill se information om felet.</span><span class="sxs-lookup"><span data-stu-id="62cb2-219">View the test results to see details about the failure.</span></span>
    
     ![][49]
    
     ![][50]

## <a name="next-steps"></a><span data-ttu-id="62cb2-220">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="62cb2-220">Next steps</span></span>
<span data-ttu-id="62cb2-221">Mer information om enhet testning i Visual Studio Team Services finns i [köra kontroller i din build](http://go.microsoft.com/fwlink/p/?LinkId=510474).</span><span class="sxs-lookup"><span data-stu-id="62cb2-221">For more about unit testing in Visual Studio Team Services, see [Run unit tests in your build](http://go.microsoft.com/fwlink/p/?LinkId=510474).</span></span> <span data-ttu-id="62cb2-222">Om du använder Git finns [dela din kod i Git](http://www.visualstudio.com/get-started/share-your-code-in-git-vs.aspx) och [kontinuerlig distribution till Azure App Service](../app-service-web/app-service-continuous-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="62cb2-222">If you're using Git, see [Share your code in Git](http://www.visualstudio.com/get-started/share-your-code-in-git-vs.aspx) and [Continuous deployment to Azure App Service](../app-service-web/app-service-continuous-deployment.md).</span></span>  <span data-ttu-id="62cb2-223">Läs mer om Visual Studio Team Services [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).</span><span class="sxs-lookup"><span data-stu-id="62cb2-223">For more information about Visual Studio Team Services, see [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).</span></span>

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
