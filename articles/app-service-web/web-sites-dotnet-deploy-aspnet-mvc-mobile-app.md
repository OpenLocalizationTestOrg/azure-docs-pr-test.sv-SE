---
title: aaaDeploy ett ASP.NET MVC 5 mobila webbappen i Azure App Service
description: "En självstudiekurs som lär du dig hur toodeploy en web app tooAzure App Service med mobila funktioner i ASP.NET MVC 5 webbprogram."
services: app-service
documentationcenter: .net
author: cephalin
manager: erikre
editor: jimbe
ms.assetid: 0752c802-8609-4956-a755-686116913645
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/12/2016
ms.author: cephalin
ms.openlocfilehash: 01119c07246c0252fd357562774a2e90b3ef77d0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-aspnet-mvc-5-mobile-web-app-in-azure-app-service"></a><span data-ttu-id="b8e77-103">Distribuera ett ASP.NET MVC 5 mobila webbappen i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="b8e77-103">Deploy an ASP.NET MVC 5 mobile web app in Azure App Service</span></span>
<span data-ttu-id="b8e77-104">Den här självstudiekursen kommer lära du hello grunderna för hur toobuild en ASP.NET MVC 5 web app som är mobilvänlig och distribuerar den tooAzure Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="b8e77-104">This tutorial will teach you hello basics of how toobuild an ASP.NET MVC 5 web app that is mobile-friendly and deploy it tooAzure App Service.</span></span> <span data-ttu-id="b8e77-105">För den här kursen behöver du [Visual Studio Express 2013 för webben] [ Visual Studio Express 2013] eller hello professionell utgåva av Visual Studio om du redan har som.</span><span class="sxs-lookup"><span data-stu-id="b8e77-105">For this tutorial, you need [Visual Studio Express 2013 for Web][Visual Studio Express 2013] or hello professional edition of Visual Studio if you already have that.</span></span> <span data-ttu-id="b8e77-106">Du kan använda [Visual Studio 2015] men hello skärmdumpar är olika och du måste använda hello ASP.NET 4.x mallar.</span><span class="sxs-lookup"><span data-stu-id="b8e77-106">You can use [Visual Studio 2015] but hello screen shots will be different and you must use hello ASP.NET 4.x templates.</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

## <a name="what-youll-build"></a><span data-ttu-id="b8e77-107">Vad du ska skapa</span><span class="sxs-lookup"><span data-stu-id="b8e77-107">What You'll Build</span></span>
<span data-ttu-id="b8e77-108">Den här kursen ska du lägga till mobila funktioner toohello enkelt konferens-lista över program som har angetts i hello [starter projektet][StarterProject].</span><span class="sxs-lookup"><span data-stu-id="b8e77-108">For this tutorial, you'll add mobile features toohello simple conference-listing application that's provided in hello [starter project][StarterProject].</span></span> <span data-ttu-id="b8e77-109">hello följande skärmbild visar hello ASP.NET sessioner i hello slutförts program som det visas i hello webbläsare emulatorn i Internet Explorer 11 F12 utvecklingsverktyg.</span><span class="sxs-lookup"><span data-stu-id="b8e77-109">hello following screenshot shows hello ASP.NET sessions in hello completed application, as seen in hello browser emulator in Internet Explorer 11 F12 developer tools.</span></span>

![][FixedSessionsByTag]

<span data-ttu-id="b8e77-110">Du kan använda hello Internet Explorer 11 F12 utvecklingsverktyg och hello [Fiddler verktyget] [ Fiddler] toohelp felsöka programmet.</span><span class="sxs-lookup"><span data-stu-id="b8e77-110">You can use hello Internet Explorer 11 F12 developer tools and hello [Fiddler tool][Fiddler] toohelp debug your application.</span></span> 

## <a name="skills-youll-learn"></a><span data-ttu-id="b8e77-111">Kunskaper lär du dig</span><span class="sxs-lookup"><span data-stu-id="b8e77-111">Skills You'll Learn</span></span>
<span data-ttu-id="b8e77-112">Här är lär du dig:</span><span class="sxs-lookup"><span data-stu-id="b8e77-112">Here's what you'll learn:</span></span>

* <span data-ttu-id="b8e77-113">Hur toouse Visual Studio 2013 toopublish webbprogrammet direkt tooa webbapp i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="b8e77-113">How toouse Visual Studio 2013 toopublish your web application directly tooa web app in Azure App Service.</span></span>
* <span data-ttu-id="b8e77-114">Hur använder hello ASP.NET MVC 5 mallar hello CSS Bootstrap framework för att förbättra visas på mobila enheter</span><span class="sxs-lookup"><span data-stu-id="b8e77-114">How hello ASP.NET MVC 5 templates use hello CSS Bootstrap framework to improve display on mobile devices</span></span>
* <span data-ttu-id="b8e77-115">Hur toocreate mobile-specifika vyer tootarget specifika mobila webbläsare, till exempel hello iPhone- och Android</span><span class="sxs-lookup"><span data-stu-id="b8e77-115">How toocreate mobile-specific views tootarget specific mobile browsers, such as hello iPhone and Android</span></span>
* <span data-ttu-id="b8e77-116">Hur toocreate responsiv vyer (vyer som svarar toodifferent webbläsare mellan enheter)</span><span class="sxs-lookup"><span data-stu-id="b8e77-116">How toocreate responsive views (views that respond toodifferent browsers across devices)</span></span>

## <a name="set-up-hello-development-environment"></a><span data-ttu-id="b8e77-117">Ställa in hello utvecklingsmiljö</span><span class="sxs-lookup"><span data-stu-id="b8e77-117">Set up hello development environment</span></span>
<span data-ttu-id="b8e77-118">Ställ in din utvecklingsmiljö genom att installera hello Azure SDK för .NET 2.5.1 eller senare.</span><span class="sxs-lookup"><span data-stu-id="b8e77-118">Set up your development environment by installing hello Azure SDK for .NET 2.5.1 or later.</span></span> 

1. <span data-ttu-id="b8e77-119">tooinstall hello Azure SDK för .NET, klicka hello länken nedan.</span><span class="sxs-lookup"><span data-stu-id="b8e77-119">tooinstall hello Azure SDK for .NET, click hello link below.</span></span> <span data-ttu-id="b8e77-120">Om du inte har Visual Studio 2013 har installerats ännu, kommer att installeras av hello-länken.</span><span class="sxs-lookup"><span data-stu-id="b8e77-120">If you don't have Visual Studio 2013 installed yet, it will be installed by hello link.</span></span> <span data-ttu-id="b8e77-121">Den här kursen kräver Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="b8e77-121">This tutorial requires Visual Studio 2013.</span></span> <span data-ttu-id="b8e77-122">[Azure SDK för Visual Studio 2013][AzureSDKVs2013]</span><span class="sxs-lookup"><span data-stu-id="b8e77-122">[Azure SDK for Visual Studio 2013][AzureSDKVs2013]</span></span>
2. <span data-ttu-id="b8e77-123">Klicka på i hello installationsprogram för webbplattform **installera** och fortsätta med installationen hello.</span><span class="sxs-lookup"><span data-stu-id="b8e77-123">In hello Web Platform Installer window, click **Install** and proceed with hello installation.</span></span>

<span data-ttu-id="b8e77-124">Du måste också en emulator mobila webbläsare.</span><span class="sxs-lookup"><span data-stu-id="b8e77-124">You will also need a mobile browser emulator.</span></span> <span data-ttu-id="b8e77-125">Hello följande fungerar:</span><span class="sxs-lookup"><span data-stu-id="b8e77-125">Any of hello following will work:</span></span>

* <span data-ttu-id="b8e77-126">Webbläsaren emulatorn i [utvecklingsverktyg för Internet Explorer 11 F12] [ EmulatorIE11] (används i alla mobila webbläsare skärmbilder).</span><span class="sxs-lookup"><span data-stu-id="b8e77-126">Browser Emulator in [Internet Explorer 11 F12 developer tools][EmulatorIE11] (used in all mobile browser screenshots).</span></span> <span data-ttu-id="b8e77-127">Den har användaren agent sträng förinställningar för Windows Phone 8, Windows Phone 7 och Apple iPad.</span><span class="sxs-lookup"><span data-stu-id="b8e77-127">It has user agent string presets for Windows Phone 8, Windows Phone 7, and Apple iPad.</span></span>
* <span data-ttu-id="b8e77-128">Webbläsaren emulatorn i [Google Chrome DevTools][EmulatorChrome].</span><span class="sxs-lookup"><span data-stu-id="b8e77-128">Browser Emulator in [Google Chrome DevTools][EmulatorChrome].</span></span> <span data-ttu-id="b8e77-129">Den innehåller förinställningar för flera Android-enheter samt Apple iPhone, iPad Apple och Amazon Kindle brand.</span><span class="sxs-lookup"><span data-stu-id="b8e77-129">It contains presets for numerous Android devices, as well as Apple iPhone, Apple iPad, and Amazon Kindle Fire.</span></span> <span data-ttu-id="b8e77-130">Det kan också emulerar touch-händelser.</span><span class="sxs-lookup"><span data-stu-id="b8e77-130">It also emulates touch events.</span></span>
* <span data-ttu-id="b8e77-131">[Opera Mobilemulator][EmulatorOpera]</span><span class="sxs-lookup"><span data-stu-id="b8e77-131">[Opera Mobile Emulator][EmulatorOpera]</span></span>

<span data-ttu-id="b8e77-132">Visual Studio-projekt med C\# källkod och är tillgängliga tooaccompany det här avsnittet:</span><span class="sxs-lookup"><span data-stu-id="b8e77-132">Visual Studio projects with C\# source code are available tooaccompany this topic:</span></span>

* <span data-ttu-id="b8e77-133">[Ladda ned Starter-projekt][StarterProject]</span><span class="sxs-lookup"><span data-stu-id="b8e77-133">[Starter project download][StarterProject]</span></span>
* <span data-ttu-id="b8e77-134">[Slutfört projekt hämtning][CompletedProject]</span><span class="sxs-lookup"><span data-stu-id="b8e77-134">[Completed project download][CompletedProject]</span></span>

## <span data-ttu-id="b8e77-135"><a name="bkmk_DeployStarterProject"></a>Distribuera hello starter projektet tooan Azure-webbapp</span><span class="sxs-lookup"><span data-stu-id="b8e77-135"><a name="bkmk_DeployStarterProject"></a>Deploy hello starter project tooan Azure web app</span></span>
1. <span data-ttu-id="b8e77-136">Hämta hello konferens lista program [starter projektet][StarterProject].</span><span class="sxs-lookup"><span data-stu-id="b8e77-136">Download hello conference-listing application [starter project][StarterProject].</span></span>
2. <span data-ttu-id="b8e77-137">Högerklicka på hello hämtade ZIP-filen i Utforskaren och välj sedan *egenskaper*.</span><span class="sxs-lookup"><span data-stu-id="b8e77-137">Then in Windows Explorer, right-click hello downloaded ZIP file and choose *Properties*.</span></span>
3. <span data-ttu-id="b8e77-138">I hello **egenskaper** dialogrutan Välj hello **avblockera** knappen.</span><span class="sxs-lookup"><span data-stu-id="b8e77-138">In hello **Properties** dialog box, choose hello **Unblock** button.</span></span> <span data-ttu-id="b8e77-139">(Avblockera förhindrar att en säkerhetsvarning som uppstår när du försöker toouse en *.zip* -fil som du har laddat ned från hello webbserver.)</span><span class="sxs-lookup"><span data-stu-id="b8e77-139">(Unblocking prevents a security warning that occurs when you try toouse a *.zip* file that you've downloaded from hello web.)</span></span>
4. <span data-ttu-id="b8e77-140">Högerklicka på hello ZIP-filen och välj **extrahera alla** att packa hello-filen.</span><span class="sxs-lookup"><span data-stu-id="b8e77-140">Right-click hello ZIP file and select **Extract All** to unzip hello file.</span></span> 
5. <span data-ttu-id="b8e77-141">Öppna i Visual Studio hello *C#\Mvc5Mobile.sln* fil.</span><span class="sxs-lookup"><span data-stu-id="b8e77-141">In Visual Studio, open hello *C#\Mvc5Mobile.sln* file.</span></span>
6. <span data-ttu-id="b8e77-142">Högerklicka på hello-projektet i Solution Explorer och klicka på **publicera**.</span><span class="sxs-lookup"><span data-stu-id="b8e77-142">In Solution Explorer, right-click hello project and click **Publish**.</span></span>
   
   ![][DeployClickPublish]
7. <span data-ttu-id="b8e77-143">Klicka på Publicera webbplats **Microsoft Azure App Service**.</span><span class="sxs-lookup"><span data-stu-id="b8e77-143">In Publish Web, click **Microsoft Azure App Service**.</span></span>
   
   ![][DeployClickWebSites]
8. <span data-ttu-id="b8e77-144">Om du inte har redan loggat in på Azure, klickar du på **Lägg till ett konto**.</span><span class="sxs-lookup"><span data-stu-id="b8e77-144">If you haven't already logged into Azure, click **Add an account**.</span></span>
   
   ![][DeploySignIn]
9. <span data-ttu-id="b8e77-145">Följ hello prompter toolog i ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="b8e77-145">Follow hello prompts toolog into your Azure account.</span></span>
10. <span data-ttu-id="b8e77-146">hello dialogrutan för Apptjänst ska nu visa du som loggat in.</span><span class="sxs-lookup"><span data-stu-id="b8e77-146">hello App Service dialog should now show you as signed in.</span></span> <span data-ttu-id="b8e77-147">Klicka på **Ny**.</span><span class="sxs-lookup"><span data-stu-id="b8e77-147">Click **New**.</span></span>
    
    ![][DeployNewWebsite]  
11. <span data-ttu-id="b8e77-148">I hello **Webbprogramnamnet** anger ett unikt app namnprefix.</span><span class="sxs-lookup"><span data-stu-id="b8e77-148">In hello **Web App Name** field, specify a unique app name prefix.</span></span> <span data-ttu-id="b8e77-149">Din fullständiga webbprogrammets namn kommer att  *&lt;prefix >*. azurewebsites.net.</span><span class="sxs-lookup"><span data-stu-id="b8e77-149">Your fully-qualified web app name will be *&lt;prefix>*.azurewebsites.net.</span></span> <span data-ttu-id="b8e77-150">Dessutom väljer eller anger ett namn på ny resursgrupp i **resursgruppen**.</span><span class="sxs-lookup"><span data-stu-id="b8e77-150">Also, select or specify a new resource group name in **Resource group**.</span></span> <span data-ttu-id="b8e77-151">Klicka på **ny** toocreate en ny programtjänstplan.</span><span class="sxs-lookup"><span data-stu-id="b8e77-151">Then, click **New** toocreate a new App Service plan.</span></span>
    
    ![][DeploySiteSettings]
12. <span data-ttu-id="b8e77-152">Konfigurera hello nya App Service-plan och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="b8e77-152">Configure hello new App Service plan and click **OK**.</span></span> 
    
    ![](./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-7a.png)
13. <span data-ttu-id="b8e77-153">Klicka på tillbaka i dialogrutan Skapa App Service hello **skapa**.</span><span class="sxs-lookup"><span data-stu-id="b8e77-153">Back in hello Create App Service dialog, click **Create**.</span></span>
    
    ![](./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-7b.png) 
14. <span data-ttu-id="b8e77-154">När hello Azure-resurser skapas, fylls hello Publicera webbplats dialogrutan med hello inställningar för din nya app.</span><span class="sxs-lookup"><span data-stu-id="b8e77-154">After hello Azure resources are created, hello Publish Web dialog will be filled with hello settings for your new app.</span></span> <span data-ttu-id="b8e77-155">Klicka på **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="b8e77-155">Click **Publish**.</span></span>
    
    ![][DeployPublishSite]
    
    <span data-ttu-id="b8e77-156">När Visual Studio har slutförts publishing hello projektet toohello Azure startwebbapp öppnas hello skrivbord webbläsaren toodisplay hello live-webbprogram.</span><span class="sxs-lookup"><span data-stu-id="b8e77-156">Once Visual Studio finishes publishing hello starter project toohello Azure web app, hello desktop browser opens toodisplay hello live web app.</span></span>
15. <span data-ttu-id="b8e77-157">Starta din mobila webbläsare emulator, kopiera hello URL för hello konferensprogram (*<prefix>*. azurewebsites.net) till hello-emulatorn och sedan klicka på knappen längst upp till höger och välj **Bläddra efter tagg**.</span><span class="sxs-lookup"><span data-stu-id="b8e77-157">Start your mobile browser emulator, copy hello URL for hello conference application (*<prefix>*.azurewebsites.net) into hello emulator, and then click the top-right button and select **Browse by tag**.</span></span> <span data-ttu-id="b8e77-158">Om du använder Internet Explorer 11 som hello standardwebbläsare, behöver du bara tootype `F12`, sedan `Ctrl+8`, och sedan ändra hello webbläsare profil för**Windows Phone**.</span><span class="sxs-lookup"><span data-stu-id="b8e77-158">If you are using Internet Explorer 11 as hello default browser, you just need tootype `F12`, then `Ctrl+8`, and then change hello browser profile too**Windows Phone**.</span></span> <span data-ttu-id="b8e77-159">Bilden nedan visar hello *AllTags* vyn i stående läge (från att välja **Bläddra efter tagg**).</span><span class="sxs-lookup"><span data-stu-id="b8e77-159">The image below shows hello *AllTags* view in portrait mode (from choosing **Browse by tag**).</span></span>
    
    ![][AllTags]

> [!TIP]
> <span data-ttu-id="b8e77-160">Medan du kan felsöka MVC 5 programmet från Visual Studio, kan du publicera din web app tooAzure igen tooverify hello live webbprogrammet direkt från din mobila webbläsare eller en webbläsare emulator.</span><span class="sxs-lookup"><span data-stu-id="b8e77-160">While you can debug your MVC 5 application from within Visual Studio, you can publish your web app tooAzure again tooverify hello live web app directly from your mobile browser or a browser emulator.</span></span>
> 
> 

<span data-ttu-id="b8e77-161">Visa hello är mycket läsbar på en mobil enhet.</span><span class="sxs-lookup"><span data-stu-id="b8e77-161">hello display is very readable on a mobile device.</span></span> <span data-ttu-id="b8e77-162">Du kan också redan se några av hello visual effekter av hello Bootstrap CSS framework.</span><span class="sxs-lookup"><span data-stu-id="b8e77-162">You can also already see some of hello visual effects applied by hello Bootstrap CSS framework.</span></span>
<span data-ttu-id="b8e77-163">Klicka på hello **ASP.NET** länk.</span><span class="sxs-lookup"><span data-stu-id="b8e77-163">Click hello **ASP.NET** link.</span></span>

![][SessionsByTagASP.NET]

<span data-ttu-id="b8e77-164">hello ASP.NET taggen vyn är monterade zoomning toohello skärmen som Bootstrap sparas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="b8e77-164">hello ASP.NET tag view is zoom-fitted toohello screen, which Bootstrap does for you automatically.</span></span> <span data-ttu-id="b8e77-165">Du kan dock förbättra den här vyn toobetter färg hello mobila webbläsare.</span><span class="sxs-lookup"><span data-stu-id="b8e77-165">However, you can improve this view toobetter suit hello mobile browser.</span></span> <span data-ttu-id="b8e77-166">Till exempel hello **datum** kolumnen är svåra att läsa.</span><span class="sxs-lookup"><span data-stu-id="b8e77-166">For example, hello **Date** column is difficult to read.</span></span> <span data-ttu-id="b8e77-167">Senare i självstudiekursen hello ska du ändra hello *AllTags* visa toomake den mobilvänlig.</span><span class="sxs-lookup"><span data-stu-id="b8e77-167">Later in hello tutorial you'll change hello *AllTags* view toomake it mobile-friendly.</span></span>

## <span data-ttu-id="b8e77-168"><a name="bkmk_bootstrap"></a>Bootstrap CSS Framework</span><span class="sxs-lookup"><span data-stu-id="b8e77-168"><a name="bkmk_bootstrap"></a> Bootstrap CSS Framework</span></span>
<span data-ttu-id="b8e77-169">Nya är hello MVC 5 inbyggt stöd för Bootstrap på mallen.</span><span class="sxs-lookup"><span data-stu-id="b8e77-169">New in hello MVC 5 template is built-in Bootstrap support.</span></span> <span data-ttu-id="b8e77-170">Du har redan sett hur det omedelbart förbättrar hello olika vyer i ditt program.</span><span class="sxs-lookup"><span data-stu-id="b8e77-170">You have already seen how it immediately improves hello different views in your application.</span></span> <span data-ttu-id="b8e77-171">Hello navigeringsfältet längst upp i hello är till exempel automatiskt döljas när hello webbläsarbredd är mindre.</span><span class="sxs-lookup"><span data-stu-id="b8e77-171">For example, hello navigation bar at hello top is automatically collapsible when hello browser width is smaller.</span></span> <span data-ttu-id="b8e77-172">Hello skrivbord webbläsare, försök storleksändring hello webbläsarfönster och se hur hello navigeringsfältet ändras dess utseende.</span><span class="sxs-lookup"><span data-stu-id="b8e77-172">On hello desktop browser, try resizing hello browser window and see how hello navigation bar changes its look and feel.</span></span> <span data-ttu-id="b8e77-173">Detta är hello responsiv webbdesign som är inbyggd i starttjänsten.</span><span class="sxs-lookup"><span data-stu-id="b8e77-173">This is hello responsive web design that is built into Bootstrap.</span></span>

<span data-ttu-id="b8e77-174">toosee hur hello webbprogrammet ser utan Bootstrap, öppna *App\_starta\\BundleConfig.cs* och kommentera ut hello rader som innehåller *bootstrap.js* och *bootstrap.css*.</span><span class="sxs-lookup"><span data-stu-id="b8e77-174">toosee how hello Web app would look without Bootstrap, open *App\_Start\\BundleConfig.cs* and comment out hello lines that contain *bootstrap.js* and *bootstrap.css*.</span></span> <span data-ttu-id="b8e77-175">hello följande kod visar hello två sista rapporter för hello `RegisterBundles` metoden efter hello ändring:</span><span class="sxs-lookup"><span data-stu-id="b8e77-175">hello following code shows hello last two statements of hello `RegisterBundles` method after hello change:</span></span>

     bundles.Add(new ScriptBundle("~/bundles/bootstrap").Include(
              //"~/Scripts/bootstrap.js",
              "~/Scripts/respond.js"));

    bundles.Add(new StyleBundle("~/Content/css").Include(
              //"~/Content/bootstrap.css",
              "~/Content/site.css"));

<span data-ttu-id="b8e77-176">Tryck på `Ctrl+F5` toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="b8e77-176">Press `Ctrl+F5` toorun hello application.</span></span>

<span data-ttu-id="b8e77-177">Se hello döljas navigeringsfältet är nu bara en vanlig osorterad lista.</span><span class="sxs-lookup"><span data-stu-id="b8e77-177">Observe that hello collapsible navigation bar is now just an ordinary unordered list.</span></span> <span data-ttu-id="b8e77-178">Klicka på **Bläddra efter tagg** igen och klicka sedan på **ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="b8e77-178">Click **Browse by tag** again, then click **ASP.NET**.</span></span>
<span data-ttu-id="b8e77-179">I hello mobilemulator vyn visas nu när den inte längre zoomning monterade toohello skärmen och du måste rulla åt sidan i ordning toosee hello höger sida av hello tabell.</span><span class="sxs-lookup"><span data-stu-id="b8e77-179">In hello mobile emulator view, you can see now that it is no longer zoom-fitted toohello screen, and you must scroll sideways in order toosee hello right side of hello table.</span></span>

![][SessionsByTagASP.NETNoBootstrap]

<span data-ttu-id="b8e77-180">Ångra ändringarna och uppdatera hello mobila webbläsare tooverify hello mobilvänlig visa har återställts.</span><span class="sxs-lookup"><span data-stu-id="b8e77-180">Undo your changes and refresh hello mobile browser tooverify that hello mobile-friendly display has been restored.</span></span>

<span data-ttu-id="b8e77-181">Starttjänsten är inte specifik tooASP.NET MVC 5 och du kan dra nytta av funktionerna i alla webbprogram.</span><span class="sxs-lookup"><span data-stu-id="b8e77-181">Bootstrap is not specific tooASP.NET MVC 5, and you can take advantage of these features in any web application.</span></span> <span data-ttu-id="b8e77-182">Men det är nu inbyggda i projektmall ASP.NET MVC 5 så att ditt webbprogram för MVC 5 kan dra nytta av Bootstrap som standard.</span><span class="sxs-lookup"><span data-stu-id="b8e77-182">But it is now built into the ASP.NET MVC 5 project template, so that your MVC 5 Web application can take advantage of Bootstrap by default.</span></span>

<span data-ttu-id="b8e77-183">Mer information om Bootstrap gå toothe [Bootstrap] [ BootstrapSite] plats.</span><span class="sxs-lookup"><span data-stu-id="b8e77-183">For more information about Bootstrap, go toothe [Bootstrap][BootstrapSite] site.</span></span>

<span data-ttu-id="b8e77-184">I hello nästa avsnitt får du se hur tooprovide mobila webbläsare vissa vyer.</span><span class="sxs-lookup"><span data-stu-id="b8e77-184">In hello next section you'll see how tooprovide mobile-browser specific views.</span></span>

## <span data-ttu-id="b8e77-185"><a name="bkmk_overrideviews"></a>Åsidosätt hello vyer och layouter partiella vyer</span><span class="sxs-lookup"><span data-stu-id="b8e77-185"><a name="bkmk_overrideviews"></a> Override hello Views, Layouts, and Partial Views</span></span>
<span data-ttu-id="b8e77-186">Du kan åsidosätta en vy (inklusive partiella vyer och layouter) för mobila webbläsare i allmänhet för en enskild mobila webbläsare eller för specifika webbläsare.</span><span class="sxs-lookup"><span data-stu-id="b8e77-186">You can override any view (including layouts and partial views) for mobile browsers in general, for an individual mobile browser, or for any specific browser.</span></span> <span data-ttu-id="b8e77-187">tooprovide mobile-specifika visa kan du kopiera en fil i vyn och lägga till *. Mobila* toohello filnamn.</span><span class="sxs-lookup"><span data-stu-id="b8e77-187">tooprovide a mobile-specific view, you can copy a view file and add *.Mobile* toohello file name.</span></span> <span data-ttu-id="b8e77-188">Till exempel toocreate en mobil *Index* vy, kan du kopiera *vyer\\Start\\Index.cshtml* till *vyer\\Start\\ Index.Mobile.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b8e77-188">For example, toocreate a mobile *Index* view, you can copy *Views\\Home\\Index.cshtml* to *Views\\Home\\Index.Mobile.cshtml*.</span></span>

<span data-ttu-id="b8e77-189">I det här avsnittet skapar du en mobile-specifika layout-fil.</span><span class="sxs-lookup"><span data-stu-id="b8e77-189">In this section, you'll create a mobile-specific layout file.</span></span>

<span data-ttu-id="b8e77-190">toostart, kopiera *vyer\\delade\\\_Layout.cshtml* till *vyer\\delade\\\_Layout.Mobile.cshtml* .</span><span class="sxs-lookup"><span data-stu-id="b8e77-190">toostart, copy *Views\\Shared\\\_Layout.cshtml* to *Views\\Shared\\\_Layout.Mobile.cshtml*.</span></span> <span data-ttu-id="b8e77-191">Öppna  *\_Layout.Mobile.cshtml* och ändra hello rubrik från **MVC5 programmet** för**MVC5 program (Mobile)**.</span><span class="sxs-lookup"><span data-stu-id="b8e77-191">Open *\_Layout.Mobile.cshtml* and change hello title from **MVC5 Application** too**MVC5 Application (Mobile)**.</span></span>

<span data-ttu-id="b8e77-192">I varje `Html.ActionLink` anropa för hello navigeringsfältet, ta bort ”Bläddra efter” i varje länk *ActionLink*.</span><span class="sxs-lookup"><span data-stu-id="b8e77-192">In each `Html.ActionLink` call for hello navigation bar, remove "Browse by" in each link *ActionLink*.</span></span> <span data-ttu-id="b8e77-193">hello följande kod visar hello slutförts `<ul class="nav navbar-nav">` taggen för hello mobila layoutfilen.</span><span class="sxs-lookup"><span data-stu-id="b8e77-193">hello following code shows hello completed `<ul class="nav navbar-nav">` tag of hello mobile layout file.</span></span>

    <ul class="nav navbar-nav">
        <li>@Html.ActionLink("Home", "Index", "Home")</li>
        <li>@Html.ActionLink("Date", "AllDates", "Home")</li>
        <li>@Html.ActionLink("Speaker", "AllSpeakers", "Home")</li>
        <li>@Html.ActionLink("Tag", "AllTags", "Home")</li>
    </ul>

<span data-ttu-id="b8e77-194">Kopiera hello *vyer\\Start\\AllTags.cshtml* filen till *vyer\\Start\\AllTags.Mobile.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b8e77-194">Copy hello *Views\\Home\\AllTags.cshtml* file to *Views\\Home\\AllTags.Mobile.cshtml*.</span></span> <span data-ttu-id="b8e77-195">Öppna ny hello-filen och ändra den `<h2>` element från ”taggar” för ”taggar (M)”:</span><span class="sxs-lookup"><span data-stu-id="b8e77-195">Open hello new file and change the `<h2>` element from "Tags" too"Tags (M)":</span></span>

    <h2>Tags (M)</h2>

<span data-ttu-id="b8e77-196">Bläddra toohello taggar sida med en webbläsare för stationära och mobila webbläsare emulatorn använder.</span><span class="sxs-lookup"><span data-stu-id="b8e77-196">Browse toohello tags page using a desktop browser and using mobile browser emulator.</span></span> <span data-ttu-id="b8e77-197">hello mobila webbläsare emulator visar hello två ändringar (hello titel från  *\_Layout.Mobile.cshtml* och hello titel från *AllTags.Mobile.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="b8e77-197">hello mobile browser emulator shows hello two changes you made (hello title from *\_Layout.Mobile.cshtml* and hello title from *AllTags.Mobile.cshtml*).</span></span>

![][AllTagsMobile_LayoutMobile]

<span data-ttu-id="b8e77-198">Däremot hello visning av skrivbordet inte har ändrats (med rubriker från  *\_Layout.cshtml* och *AllTags.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="b8e77-198">In contrast, hello desktop display has not changed (with titles from *\_Layout.cshtml* and *AllTags.cshtml*).</span></span>

![][AllTagsMobile_LayoutMobileDesktop]

## <span data-ttu-id="b8e77-199"><a name="bkmk_browserviews"></a>Skapa specifika webbläsare vyer</span><span class="sxs-lookup"><span data-stu-id="b8e77-199"><a name="bkmk_browserviews"></a> Create Browser-Specific Views</span></span>
<span data-ttu-id="b8e77-200">Dessutom toomobile-specifika och desktop-specifika vyer, du kan skapa vyer för en enskild webbläsare.</span><span class="sxs-lookup"><span data-stu-id="b8e77-200">In addition toomobile-specific and desktop-specific views, you can create views for an individual browser.</span></span> <span data-ttu-id="b8e77-201">Du kan till exempel skapa vyer som är specifikt för hello iPhone- eller hello Android-webbläsare.</span><span class="sxs-lookup"><span data-stu-id="b8e77-201">For example, you can create views that are specifically for hello iPhone or hello Android browser.</span></span> <span data-ttu-id="b8e77-202">I det här avsnittet skapar du en layout för hello iPhone webbläsare och en iPhone-version av hello *AllTags* vyn.</span><span class="sxs-lookup"><span data-stu-id="b8e77-202">In this section, you'll create a layout for hello iPhone browser and an iPhone version of hello *AllTags* view.</span></span>

<span data-ttu-id="b8e77-203">Öppna hello *Global.asax* och Lägg till hello följande kod toohello längst ned i `Application_Start` metod.</span><span class="sxs-lookup"><span data-stu-id="b8e77-203">Open hello *Global.asax* file and add hello following code toohello bottom of the `Application_Start` method.</span></span>

    DisplayModeProvider.Instance.Modes.Insert(0, new DefaultDisplayMode("iPhone")
    {
        ContextCondition = (context => context.GetOverriddenUserAgent().IndexOf
            ("iPhone", StringComparison.OrdinalIgnoreCase) >= 0)
    });

<span data-ttu-id="b8e77-204">Den här koden definierar ett nytt visningsläge med namnet ”iPhone” som matchas mot varje inkommande begäran.</span><span class="sxs-lookup"><span data-stu-id="b8e77-204">This code defines a new display mode named "iPhone" that will be matched against each incoming request.</span></span> <span data-ttu-id="b8e77-205">Om hello inkommande begäran matchar de villkor som du definierade (det vill säga om hello användaragent innehåller hello strängen ”iPhone”), kollar ASP.NET MVC för vyer vars namn innehåller suffixet ”iPhone”.</span><span class="sxs-lookup"><span data-stu-id="b8e77-205">If hello incoming request matches the condition you defined (that is, if hello user agent contains hello string "iPhone"), ASP.NET MVC will look for views whose name contains the "iPhone" suffix.</span></span>

> [!NOTE]
> <span data-ttu-id="b8e77-206">När lägger till specifika mobila webbläsare visningslägen, såsom för iPhone- och Android, vara säker på att tooset hello första argumentet för`0` (insert hello överst i listan hello) toomake till hello specifika webbläsare läget företräde framför hello mobila mall (*. Mobile.cshtml).</span><span class="sxs-lookup"><span data-stu-id="b8e77-206">When adding mobile browser-specific display modes, such as for iPhone and Android, be sure tooset hello first argument too`0` (insert at hello top of hello list) toomake sure that hello browser-specific mode takes precedence over hello mobile template (*.Mobile.cshtml).</span></span> <span data-ttu-id="b8e77-207">Om hello mobila mall är hello överst i listan hello i stället, väljs den via din avsedda visningsläget (hello första matchar wins och hello mobila mall matchar alla mobila webbläsare).</span><span class="sxs-lookup"><span data-stu-id="b8e77-207">If hello mobile template is at hello top of hello list instead, it will be selected over your intended display mode (hello first match wins, and hello mobile template matches all mobile browsers).</span></span> 
> 
> 

<span data-ttu-id="b8e77-208">I hello kod, högerklickar du på `DefaultDisplayMode`, Välj **lösa**, och välj sedan `using System.Web.WebPages;`.</span><span class="sxs-lookup"><span data-stu-id="b8e77-208">In hello code, right-click `DefaultDisplayMode`, choose **Resolve**, and then choose `using System.Web.WebPages;`.</span></span> <span data-ttu-id="b8e77-209">Detta lägger till en referens toothe `System.Web.WebPages` namnområdet, som är var den `DisplayModeProvider` och `DefaultDisplayMode` anges.</span><span class="sxs-lookup"><span data-stu-id="b8e77-209">This adds a reference toothe `System.Web.WebPages` namespace, which is where the `DisplayModeProvider` and `DefaultDisplayMode` types are defined.</span></span>

![][ResolveDefaultDisplayMode]

<span data-ttu-id="b8e77-210">Du kan också du kan bara manuellt lägga till följande rad toothe hello `using` i hello-filen.</span><span class="sxs-lookup"><span data-stu-id="b8e77-210">Alternatively, you can just manually add hello following line toothe `using` section of hello file.</span></span>

    using System.Web.WebPages;

<span data-ttu-id="b8e77-211">Spara hello ändringar.</span><span class="sxs-lookup"><span data-stu-id="b8e77-211">Save hello changes.</span></span> <span data-ttu-id="b8e77-212">Kopiera den *vyer\\delade\\\_Layout.Mobile.cshtml* filen till *vyer\\delade\\\_Layout.iPhone.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b8e77-212">Copy the *Views\\Shared\\\_Layout.Mobile.cshtml* file to *Views\\Shared\\\_Layout.iPhone.cshtml*.</span></span> <span data-ttu-id="b8e77-213">Öppna ny hello-fil och ändra sedan hello titel från `MVC5 Application (Mobile)` till `MVC5 Application (iPhone)`.</span><span class="sxs-lookup"><span data-stu-id="b8e77-213">Open hello new file and then change hello title from `MVC5 Application (Mobile)` to `MVC5 Application (iPhone)`.</span></span>

<span data-ttu-id="b8e77-214">Kopiera hello *vyer\\Start\\AllTags.Mobile.cshtml* filen till *vyer\\Start\\AllTags.iPhone.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b8e77-214">Copy hello *Views\\Home\\AllTags.Mobile.cshtml* file to *Views\\Home\\AllTags.iPhone.cshtml*.</span></span> <span data-ttu-id="b8e77-215">Ny hello-fil, ändra hello `<h2>` element från ”taggar (M)” för ”taggar (iPhone)”.</span><span class="sxs-lookup"><span data-stu-id="b8e77-215">In hello new file, change hello `<h2>` element from "Tags (M)" too"Tags (iPhone)".</span></span>

<span data-ttu-id="b8e77-216">Kör hello program.</span><span class="sxs-lookup"><span data-stu-id="b8e77-216">Run hello application.</span></span> <span data-ttu-id="b8e77-217">Kör en mobila webbläsare emulator, se till att dess användaragent anges för ”iPhone”, och bläddra toohello *AllTags* vyn.</span><span class="sxs-lookup"><span data-stu-id="b8e77-217">Run a mobile browser emulator, make sure its user agent is set too"iPhone", and browse toohello *AllTags* view.</span></span> <span data-ttu-id="b8e77-218">Om du använder hello emulatorn i Internet Explorer 11 F12 utvecklingsverktyg, konfigurera emulering toohello följande:</span><span class="sxs-lookup"><span data-stu-id="b8e77-218">If you are using hello emulator in Internet Explorer 11 F12 developer tools, configure emulation toohello following:</span></span>

* <span data-ttu-id="b8e77-219">Webbläsaren profil = **Windows Phone**</span><span class="sxs-lookup"><span data-stu-id="b8e77-219">Browser profile = **Windows Phone**</span></span>
* <span data-ttu-id="b8e77-220">Användaragentsträngen = **anpassad**</span><span class="sxs-lookup"><span data-stu-id="b8e77-220">User agent string =  **Custom**</span></span>
* <span data-ttu-id="b8e77-221">Anpassad sträng = **1001.525-Apple-iPhone5C1**</span><span class="sxs-lookup"><span data-stu-id="b8e77-221">Custom string = **Apple-iPhone5C1/1001.525**</span></span>

<span data-ttu-id="b8e77-222">hello följande skärmbild visar hello *AllTags* visa återges i emulatorn i Internet Explorer 11 F12 utvecklingsverktyg med hello anpassade användaragentsträngen (detta är en iPhone 5 C användaragentsträngen).</span><span class="sxs-lookup"><span data-stu-id="b8e77-222">hello following screenshot shows hello *AllTags* view rendered in the emulator in Internet Explorer 11 F12 developer tools with hello custom user agent string (this is an iPhone 5C user agent string).</span></span>

![][AllTagsIPhone_LayoutIPhone]

<span data-ttu-id="b8e77-223">I hello mobila webbläsare väljer hello **högtalare** länk.</span><span class="sxs-lookup"><span data-stu-id="b8e77-223">In hello mobile browser, select hello **Speakers** link.</span></span> <span data-ttu-id="b8e77-224">Eftersom det inte är en mobil vy (*AllSpeakers.Mobile.cshtml*), visa hello standard högtalare (*AllSpeakers.cshtml*) återges med hello mobila vyn ( *\_ Layout.Mobile.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="b8e77-224">Because there's not a mobile view (*AllSpeakers.Mobile.cshtml*), hello default speakers view (*AllSpeakers.cshtml*) is rendered using hello mobile layout view (*\_Layout.Mobile.cshtml*).</span></span> <span data-ttu-id="b8e77-225">Enligt nedan, hello rubrik **MVC5 program (Mobile)** har definierats i  *\_Layout.Mobile.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b8e77-225">As shown below, hello title **MVC5 Application (Mobile)** is defined in *\_Layout.Mobile.cshtml*.</span></span>

![][AllSpeakers_LayoutMobile]

<span data-ttu-id="b8e77-226">Du kan inaktivera en standardvy (icke-mobila) från återgivning inuti en mobila layout globalt genom att ange `RequireConsistentDisplayMode` till `true` i hello *vyer\\\_ViewStart.cshtml* filen, så här:</span><span class="sxs-lookup"><span data-stu-id="b8e77-226">You can globally disable a default (non-mobile) view from rendering inside a mobile layout by setting `RequireConsistentDisplayMode` to `true` in hello *Views\\\_ViewStart.cshtml* file, like this:</span></span>

    @{
        Layout = "~/Views/Shared/_Layout.cshtml";
        DisplayModeProvider.Instance.RequireConsistentDisplayMode = true;
    }

<span data-ttu-id="b8e77-227">När `RequireConsistentDisplayMode` har angetts för`true`, hello mobila layout (*\_Layout.Mobile.cshtml*) används endast för mobila vyer (d.v.s. när filen vyn är i formatet hello  ***vynamn** . Mobile.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="b8e77-227">When `RequireConsistentDisplayMode` is set too`true`, hello mobile layout (*\_Layout.Mobile.cshtml*) is used only for mobile views (i.e. when the view file is of hello form ***ViewName**.Mobile.cshtml*).</span></span> <span data-ttu-id="b8e77-228">Du kanske vill tooset `RequireConsistentDisplayMode` för`true` om mobila layouten inte fungerar bra med dina icke-mobila vyer.</span><span class="sxs-lookup"><span data-stu-id="b8e77-228">You might want tooset `RequireConsistentDisplayMode` too`true` if your mobile layout doesn't work well with your non-mobile views.</span></span> <span data-ttu-id="b8e77-229">hello skärmbilden nedan visar hur hello *högtalare* sidan återger när `RequireConsistentDisplayMode` har angetts för`true` (utan hello strängen ”(mobilt)” i hello navigering liggande hello överst).</span><span class="sxs-lookup"><span data-stu-id="b8e77-229">hello screenshot below shows how hello *Speakers* page renders when `RequireConsistentDisplayMode` is set too`true` (without hello string "(Mobile)" in hello navigational bar at hello top).</span></span>

![][AllSpeakers_LayoutMobileOverridden]

<span data-ttu-id="b8e77-230">Du kan inaktivera konsekvent visningsläge i en specifik vy genom att ange `RequireConsistentDisplayMode` för`false` i hello visa filen.</span><span class="sxs-lookup"><span data-stu-id="b8e77-230">You can disable consistent display mode in a specific view by setting `RequireConsistentDisplayMode` too`false` in hello view file.</span></span> <span data-ttu-id="b8e77-231">Följande markering i hello *vyer\\Start\\AllSpeakers.cshtml* filen anger `RequireConsistentDisplayMode` för`false`:</span><span class="sxs-lookup"><span data-stu-id="b8e77-231">The following markup in hello *Views\\Home\\AllSpeakers.cshtml* file sets `RequireConsistentDisplayMode` too`false`:</span></span>

    @model IEnumerable<string>

    @{
        ViewBag.Title = "All speakers";
        DisplayModeProvider.Instance.RequireConsistentDisplayMode = false;
    }

<span data-ttu-id="b8e77-232">I det här avsnittet har vi sett hur toocreate mobila layouter och vyer och hur toocreate layouter och vyer för specifika enheter såsom hello iPhone.</span><span class="sxs-lookup"><span data-stu-id="b8e77-232">In this section we've seen how toocreate mobile layouts and views and how toocreate layouts and views for specific devices such as hello iPhone.</span></span>
<span data-ttu-id="b8e77-233">Hello största fördelen hello Bootstrap CSS framework är dock dynamisk layout, vilket innebär att en enda formatmall kan tillämpas på skrivbordet, phone och tablet webbläsare toocreate ett konsekvent utseende.</span><span class="sxs-lookup"><span data-stu-id="b8e77-233">However, hello main advantage of hello Bootstrap CSS framework is the responsive layout, which means that a single stylesheet can be applied across desktop, phone, and tablet browsers toocreate a consistent look and feel.</span></span> <span data-ttu-id="b8e77-234">I hello nästa avsnitt får du se hur tooleverage Bootstrap toocreate mobilvänlig vyer.</span><span class="sxs-lookup"><span data-stu-id="b8e77-234">In hello next section you'll see how tooleverage Bootstrap toocreate mobile-friendly views.</span></span>

## <span data-ttu-id="b8e77-235"><a name="bkmk_Improvespeakerslist"></a>Förbättra hello högtalare lista</span><span class="sxs-lookup"><span data-stu-id="b8e77-235"><a name="bkmk_Improvespeakerslist"></a> Improve hello Speakers List</span></span>
<span data-ttu-id="b8e77-236">Som du precis såg hello *högtalare* läget läsbar men hello länkar är mindre och svåra tootap på en mobil enhet.</span><span class="sxs-lookup"><span data-stu-id="b8e77-236">As you just saw, hello *Speakers* view is readable, but hello links are small and are difficult tootap on a mobile device.</span></span> <span data-ttu-id="b8e77-237">I det här avsnittet ska du göra hello *AllSpeakers* view mobilvänlig, som visar stora, enkelt att tryck länkar och innehåller en Sök-rutan tooquickly hitta högtalare.</span><span class="sxs-lookup"><span data-stu-id="b8e77-237">In this section, you'll make hello *AllSpeakers* view mobile-friendly, which displays large, easy-to-tap links and contains a search box tooquickly find speakers.</span></span>

<span data-ttu-id="b8e77-238">Du kan använda hello Bootstrap [länkad lista grupp] [ linked list group] formatmalls att förbättra hello *högtalare* vyn.</span><span class="sxs-lookup"><span data-stu-id="b8e77-238">You can use hello Bootstrap [linked list group][linked list group] styling to improve hello *Speakers* view.</span></span> <span data-ttu-id="b8e77-239">I *vyer\\Start\\AllSpeakers.cshtml*, Ersätt hello innehållet i hello Razor-filen med hello koden nedan.</span><span class="sxs-lookup"><span data-stu-id="b8e77-239">In *Views\\Home\\AllSpeakers.cshtml*, replace hello contents of hello Razor file with hello code below.</span></span>

     @model IEnumerable<string>

    @{
        ViewBag.Title = "All Speakers";
    }

    <h2>Speakers</h2>

    <div class="list-group">
        @foreach (var speaker in Model)
        {
            @Html.ActionLink(speaker, "SessionsBySpeaker", new { speaker }, new { @class = "list-group-item" })
        }
    </div>

<span data-ttu-id="b8e77-240">Hej `class="list-group"` attribut i hello `<div>` taggen gäller Bootstrap listan formatmalls och hello `class="input-group-item"` attributet gäller Bootstrap listan artikel formatmalls tooeach länk.</span><span class="sxs-lookup"><span data-stu-id="b8e77-240">hello `class="list-group"` attribute in hello `<div>` tag applies the Bootstrap list styling, and hello `class="input-group-item"` attribute applies Bootstrap list item styling tooeach link.</span></span>

<span data-ttu-id="b8e77-241">Uppdatera hello mobila webbläsare.</span><span class="sxs-lookup"><span data-stu-id="b8e77-241">Refresh hello mobile browser.</span></span> <span data-ttu-id="b8e77-242">hello uppdaterat vyn ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="b8e77-242">hello updated view looks like this:</span></span>

![][AllSpeakersFixed]

<span data-ttu-id="b8e77-243">hello Bootstrap [länkad lista grupp] [ linked list group] formatmalls gör hello hela rutan för varje länk klickbara, vilket är en mycket bättre användarupplevelse.</span><span class="sxs-lookup"><span data-stu-id="b8e77-243">hello Bootstrap [linked list group][linked list group] styling makes hello entire box for each link clickable, which is a much better user experience.</span></span> <span data-ttu-id="b8e77-244">Växla toothe skrivbordsvy och se hello konsekvent utseende.</span><span class="sxs-lookup"><span data-stu-id="b8e77-244">Switch toothe desktop view and observe hello consistent look and feel.</span></span>

![][AllSpeakersFixedDesktop]

<span data-ttu-id="b8e77-245">Även om hello mobila webbläsare vyn har förbättrats, är det svårt att navigera hello lång lista med högtalare.</span><span class="sxs-lookup"><span data-stu-id="b8e77-245">Although hello mobile browser view has improved, it's difficult to navigate hello long list of speakers.</span></span> <span data-ttu-id="b8e77-246">Starttjänsten ger inte en sökning filter funktioner out-of-the-box, men du kan lägga till den med några få rader med kod.</span><span class="sxs-lookup"><span data-stu-id="b8e77-246">Bootstrap doesn't provide a search filter functionality out-of-the-box, but you can add it with a few lines of code.</span></span> <span data-ttu-id="b8e77-247">Du kommer först lägga till en sökning rutan toohello vy och sedan koppla samman med hello JavaScript-kod för hello filterfunktionen.</span><span class="sxs-lookup"><span data-stu-id="b8e77-247">You will first add a search box toohello view, then hook up with hello JavaScript code for hello filter function.</span></span> <span data-ttu-id="b8e77-248">I *vyer\\Start\\AllSpeakers.cshtml*, lägga till en \<formuläret\> taggen precis efter hello \<h2\> tagg, enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="b8e77-248">In *Views\\Home\\AllSpeakers.cshtml*, add a \<form\> tag just after hello \<h2\> tag, as shown below:</span></span>

    @model IEnumerable<string>

    @{
        ViewBag.Title = "All Speakers";
    }

    <h2>Speakers</h2>

    <form class="input-group">
        <span class="input-group-addon"><span class="glyphicon glyphicon-search"></span></span>
        <input type="text" class="form-control" placeholder="Search speaker">
    </form>
    <br />
    <div class="list-group">
        @foreach (var speaker in Model)
        {
            @Html.ActionLink(speaker, 
                             "SessionsBySpeaker", 
                             new { speaker }, 
                             new { @class = "list-group-item" })
        }
    </div>

<span data-ttu-id="b8e77-249">Observera att hello `<form>` och `<input>` taggar som båda har hello Bootstrap formatmallar toothem.</span><span class="sxs-lookup"><span data-stu-id="b8e77-249">Notice that hello `<form>` and `<input>` tags both have hello Bootstrap styles applied toothem.</span></span> <span data-ttu-id="b8e77-250">Hej `<span>` element lägger till en Bootstrap [glyphicon] [ glyphicon] toothe sökrutan.</span><span class="sxs-lookup"><span data-stu-id="b8e77-250">hello `<span>` element adds a Bootstrap [glyphicon][glyphicon] toothe search box.</span></span>

<span data-ttu-id="b8e77-251">I hello *skript* mapp, lägga till en JavaScript-fil som heter *filter.js*.</span><span class="sxs-lookup"><span data-stu-id="b8e77-251">In hello *Scripts* folder, add a JavaScript file called *filter.js*.</span></span> <span data-ttu-id="b8e77-252">Öppna hello-filen och klistra in följande kod till den hello:</span><span class="sxs-lookup"><span data-stu-id="b8e77-252">Open hello file and paste hello following code into it:</span></span>

    $(function () {

        // reset hello search form when hello page loads
        $("form").each(function () {
            this.reset();
        });

        // wire up hello events toohello <input> element for search/filter
        $("input").bind("keyup change", function () {
            var searchtxt = this.value.toLowerCase();
            var items = $(".list-group-item");

            // show all speakers that begin with hello typed text and hide others
            for (var i = 0; i < items.length; i++) {
                var val = items[i].text.toLowerCase();
                val = val.substring(0, searchtxt.length);
                if (val == searchtxt) {
                    $(items[i]).show();
                }
                else {
                    $(items[i]).hide();
                }
            }
        });
    });

<span data-ttu-id="b8e77-253">Du måste också tooinclude filter.js i din registrerade paket.</span><span class="sxs-lookup"><span data-stu-id="b8e77-253">You also need tooinclude filter.js in your registered bundles.</span></span> <span data-ttu-id="b8e77-254">Öppna *App\_starta\\BundleConfig.cs* och ändra hello första paket.</span><span class="sxs-lookup"><span data-stu-id="b8e77-254">Open *App\_Start\\BundleConfig.cs* and change hello first bundles.</span></span> <span data-ttu-id="b8e77-255">Ändra först `bundles.Add` instruktionen (för hello **jquery** paket) tooinclude *skript\\filter.js*, enligt följande:</span><span class="sxs-lookup"><span data-stu-id="b8e77-255">Change the first `bundles.Add` statement (for hello **jquery** bundle) tooinclude *Scripts\\filter.js*, as follows:</span></span>

     bundles.Add(new ScriptBundle("~/bundles/jquery").Include(
                "~/Scripts/jquery-{version}.js",
                "~/Scripts/filter.js"));

<span data-ttu-id="b8e77-256">Hej **jquery** paket återges redan hello standard  *\_Layout* vyn.</span><span class="sxs-lookup"><span data-stu-id="b8e77-256">hello **jquery** bundle is already rendered by hello default *\_Layout* view.</span></span> <span data-ttu-id="b8e77-257">Senare kan du använda hello samma JavaScript-kod tooapply filtrera funktioner tooother listvyer.</span><span class="sxs-lookup"><span data-stu-id="b8e77-257">Later, you can utilize hello same JavaScript code tooapply the filter functionality tooother list views.</span></span>

<span data-ttu-id="b8e77-258">Uppdatera hello mobila webbläsare och gå toohello *AllSpeakers* vyn.</span><span class="sxs-lookup"><span data-stu-id="b8e77-258">Refresh hello mobile browser and go toohello *AllSpeakers* view.</span></span> <span data-ttu-id="b8e77-259">Skriv ”sc” i sökrutan.</span><span class="sxs-lookup"><span data-stu-id="b8e77-259">In the search box, type "sc".</span></span> <span data-ttu-id="b8e77-260">hello högtalare lista bör nu filtreras enligt tooyour söksträngen.</span><span class="sxs-lookup"><span data-stu-id="b8e77-260">hello speakers list should now be filtered according tooyour search string.</span></span>

![][AllSpeakersFixedSearchBySC]

## <span data-ttu-id="b8e77-261"><a name="bkmk_improvetags"></a>Förbättra hello Tagglista</span><span class="sxs-lookup"><span data-stu-id="b8e77-261"><a name="bkmk_improvetags"></a> Improve hello Tags List</span></span>
<span data-ttu-id="b8e77-262">Som hello *högtalare* visas hello *taggar* läget läsbar men hello länkar är liten och svår tootap på en mobil enhet.</span><span class="sxs-lookup"><span data-stu-id="b8e77-262">Like hello *Speakers* view, hello *Tags* view is readable, but hello links are small and difficult tootap on a mobile device.</span></span> <span data-ttu-id="b8e77-263">Du kan åtgärda hello *taggar* visa hello samma sätt du åtgärda hello *högtalare* visas om du använder hello kodändringar beskrivs tidigare, men med följande hello `Html.ActionLink` metodsyntax i  *Vyer\\Start\\AllTags.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="b8e77-263">You can fix hello *Tags* view hello same way you fix hello *Speakers* view, if you use hello code changes described earlier, but with hello following `Html.ActionLink` method syntax in *Views\\Home\\AllTags.cshtml*:</span></span>

    @Html.ActionLink(tag, 
                     "SessionsByTag", 
                     new { tag }, 
                     new { @class = "list-group-item" })

<span data-ttu-id="b8e77-264">hello uppdateras skrivbord webbläsaren ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="b8e77-264">hello refreshed desktop browser looks as follows:</span></span>

![][AllTagsFixedDesktop]

<span data-ttu-id="b8e77-265">Och hello uppdateras mobila webbläsare ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="b8e77-265">And hello refreshed mobile browser looks as follows:</span></span> 

![][AllTagsFixed]

> [!NOTE]
> <span data-ttu-id="b8e77-266">Om du märker att hello listformatering finns kvar i Hej mobila webbläsare och undrar vad hände tooyour nice Bootstrap formatmalls, detta är en metod med en tidigare åtgärd toocreate mobila vissa vyer.</span><span class="sxs-lookup"><span data-stu-id="b8e77-266">If you notice that hello original list formatting is still there in hello mobile browser and wonder what happened tooyour nice Bootstrap styling, this is an artifact of your earlier action toocreate mobile specific views.</span></span> <span data-ttu-id="b8e77-267">Nu när du använder hello Bootstrap CSS framework toocreate en responsiv design, gå head och ta bort dessa mobile-specifika vyer och hello mobile-specifika layout vyer.</span><span class="sxs-lookup"><span data-stu-id="b8e77-267">However, now that you are using hello Bootstrap CSS framework toocreate a responsive web design, go head and remove these mobile-specific views and hello mobile-specific layout views.</span></span> <span data-ttu-id="b8e77-268">När du har gjort det, kommer hello uppdateras mobila webbläsare visar hello Bootstrap formatmalls.</span><span class="sxs-lookup"><span data-stu-id="b8e77-268">Once you have done so, hello refreshed mobile browser will show hello Bootstrap styling.</span></span>
> 
> 

## <span data-ttu-id="b8e77-269"><a name="bkmk_improvedates"></a>Förbättra hello datum lista</span><span class="sxs-lookup"><span data-stu-id="b8e77-269"><a name="bkmk_improvedates"></a> Improve hello Dates List</span></span>
<span data-ttu-id="b8e77-270">Du kan förbättra hello *datum* Visa som du förbättrad hello *högtalare* och *taggar* vyer om du använder hello kodändringar beskrivs tidigare, men med följande hello `Html.ActionLink` metodsyntax i *vyer\\Start\\AllDates.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="b8e77-270">You can improve hello *Dates* view like you improved hello *Speakers* and *Tags* views if you use hello code changes described earlier, but with hello following `Html.ActionLink` method syntax in *Views\\Home\\AllDates.cshtml*:</span></span>

    @Html.ActionLink(date.ToString("ddd, MMM dd, h:mm tt"), 
                     "SessionsByDate", 
                     new { date }, 
                     new { @class = "list-group-item" })

<span data-ttu-id="b8e77-271">Du får en Webbläsarvy uppdateras mobila så här:</span><span class="sxs-lookup"><span data-stu-id="b8e77-271">You will get a refreshed mobile browser view like this:</span></span>

![][AllDatesFixed]

<span data-ttu-id="b8e77-272">Du kan förbättra hello ytterligare *datum* vyn genom att ordna hello datum / tid-värden efter datum.</span><span class="sxs-lookup"><span data-stu-id="b8e77-272">You can further improve hello *Dates* view by organizing hello date-time values by date.</span></span> <span data-ttu-id="b8e77-273">Detta kan göras med hello Bootstrap [paneler] [ panels] formatering.</span><span class="sxs-lookup"><span data-stu-id="b8e77-273">This can be done with hello Bootstrap [panels][panels] styling.</span></span> <span data-ttu-id="b8e77-274">Ersätt hello innehållet i hello *vyer\\Start\\AllDates.cshtml* med följande kod:</span><span class="sxs-lookup"><span data-stu-id="b8e77-274">Replace hello contents of hello *Views\\Home\\AllDates.cshtml* file with the following code:</span></span>

    @model IEnumerable<DateTime>

    @{
        ViewBag.Title = "All Dates";
    }

    <h2>Dates</h2>

    @foreach (var dategroup in Model.GroupBy(x=>x.Date))
    {
        <div class="panel panel-primary">
            <div class="panel-heading">
                @dategroup.Key.ToString("ddd, MMM dd")
            </div>
            <div class="panel-body list-group">
                @foreach (var date in dategroup)
                {
                    @Html.ActionLink(date.ToString("h:mm tt"), 
                                     "SessionsByDate", 
                                     new { date }, 
                                     new { @class = "list-group-item" })
                }
            </div>
        </div>
    }

<span data-ttu-id="b8e77-275">Den här koden skapar en separat `<div class="panel panel-primary">` taggen för varje distinkta datum i hello-lista och använder hello [länkad lista grupp] [ linked list group] för respektive länkar som tidigare.</span><span class="sxs-lookup"><span data-stu-id="b8e77-275">This code creates a separate `<div class="panel panel-primary">` tag for each distinct date in hello list, and uses hello [linked list group][linked list group] for the respective links as before.</span></span> <span data-ttu-id="b8e77-276">Här är vilken hello mobila webbläsare ser ut som när den här koden körs:</span><span class="sxs-lookup"><span data-stu-id="b8e77-276">Here's what hello mobile browser looks like when this code runs:</span></span>

![][AllDatesFixed2]

<span data-ttu-id="b8e77-277">Växla toohello skrivbord webbläsare.</span><span class="sxs-lookup"><span data-stu-id="b8e77-277">Switch toohello desktop browser.</span></span> <span data-ttu-id="b8e77-278">Tänk återigen hello konsekvent.</span><span class="sxs-lookup"><span data-stu-id="b8e77-278">Again, note hello consistent look.</span></span>

![][AllDatesFixed2Desktop]

## <span data-ttu-id="b8e77-279"><a name="bkmk_improvesessionstable"></a>Förbättra hello SessionsTable vy</span><span class="sxs-lookup"><span data-stu-id="b8e77-279"><a name="bkmk_improvesessionstable"></a> Improve hello SessionsTable View</span></span>
<span data-ttu-id="b8e77-280">I det här avsnittet ska du göra hello *SessionsTable* visa mer mobilvänlig.</span><span class="sxs-lookup"><span data-stu-id="b8e77-280">In this section, you'll make hello *SessionsTable* view more mobile-friendly.</span></span> <span data-ttu-id="b8e77-281">Den här ändringen är mer omfattande hello tidigare ändringar.</span><span class="sxs-lookup"><span data-stu-id="b8e77-281">This change is more extensive hello previous changes.</span></span>

<span data-ttu-id="b8e77-282">Tryck på hello i hello mobila webbläsare **taggen** knappen och klicka sedan på Ange `asp` i sökrutan.</span><span class="sxs-lookup"><span data-stu-id="b8e77-282">In hello mobile browser, tap hello **Tag** button, then enter `asp` in the search box.</span></span>

![][AllTagsFixedSearchByASP]

<span data-ttu-id="b8e77-283">Tryck på hello **ASP.NET** länk.</span><span class="sxs-lookup"><span data-stu-id="b8e77-283">Tap hello **ASP.NET** link.</span></span>

![][SessionsTableTagASP.NET]

<span data-ttu-id="b8e77-284">Som du ser formateras hello visas som en tabell som är för närvarande utformad toobe visas i hello skrivbord webbläsare.</span><span class="sxs-lookup"><span data-stu-id="b8e77-284">As you can see, hello display is formatted as a table, which is currently designed toobe viewed in hello desktop browser.</span></span> <span data-ttu-id="b8e77-285">Det är dock lite svårt tooread mobila webbläsare.</span><span class="sxs-lookup"><span data-stu-id="b8e77-285">However, it's a little bit difficult tooread on a mobile browser.</span></span> <span data-ttu-id="b8e77-286">toofix detta, öppna *vyer\\Start\\SessionsTable.cshtml* och Ersätt hello innehållet i filen med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="b8e77-286">toofix this, open *Views\\Home\\SessionsTable.cshtml* and then replace hello contents of the file with hello following code:</span></span>

    @model IEnumerable<Mvc5Mobile.Models.Session>

    <h2>@ViewBag.Title</h2>

    <div class="container">
        <div class="row">
            @foreach (var session in Model)
            {
                <div class="col-md-4">
                    <div class="list-group">
                        @Html.ActionLink(session.Title, 
                                         "SessionByCode", 
                                         new { session.Code }, 
                                         new { @class="list-group-item active" })
                        <div class="list-group-item">
                            <div class="list-group-item-text">
                                @Html.Partial("_SpeakersLinks", session)
                            </div>
                            <div class="list-group-item-info">
                                @session.DateText
                            </div>
                            <div class="list-group-item-info small hidden-xs">
                                @Html.Partial("_TagsLinks", session)
                            </div>
                        </div>
                    </div>
                </div>
            }
        </div>
    </div>

<span data-ttu-id="b8e77-287">hello koden gör 3 saker:</span><span class="sxs-lookup"><span data-stu-id="b8e77-287">hello code does 3 things:</span></span>

* <span data-ttu-id="b8e77-288">använder hello Bootstrap [anpassad länkade lista grupp] [ custom linked list group] tooformat hello sessionsinformation lodrätt, så att den här informationen kan läsas på en mobila webbläsare (med klasser som lista-grupp-objekt-text)</span><span class="sxs-lookup"><span data-stu-id="b8e77-288">uses hello Bootstrap [custom linked list group][custom linked list group] tooformat hello session information vertically, so that all this information is readable on a mobile browser (using classes such as list-group-item-text)</span></span>
* <span data-ttu-id="b8e77-289">gäller hello [nätet] [ grid system] toothe layout så hello sessionen objekt flödet vågrätt i hello skrivbord webbläsare och lodrätt i hello mobila webbläsare (med hello kolumn md 4 klassen)</span><span class="sxs-lookup"><span data-stu-id="b8e77-289">applies hello [grid system][grid system] toothe layout, so that hello session items flow horizontally in hello desktop browser and vertically in hello mobile browser (using hello col-md-4 class)</span></span>
* <span data-ttu-id="b8e77-290">använder hello [dynamisk verktygen] [ responsive utilities] att dölja hello session taggar när visas i hello mobila webbläsare (med hello dolda xs klassen)</span><span class="sxs-lookup"><span data-stu-id="b8e77-290">uses hello [responsive utilities][responsive utilities] to hide hello session tags when viewed in hello mobile browser (using hello hidden-xs class)</span></span>

<span data-ttu-id="b8e77-291">Du kan också trycka på en avdelning länken toogo toohello respektive session.</span><span class="sxs-lookup"><span data-stu-id="b8e77-291">You can also tap a title link toogo toohello respective session.</span></span> <span data-ttu-id="b8e77-292">hello bilden nedan visar hello kodändringar.</span><span class="sxs-lookup"><span data-stu-id="b8e77-292">hello image below reflects hello code changes.</span></span>

![][FixedSessionsByTag]

<span data-ttu-id="b8e77-293">hello Bootstrap nätet som du använde automatiskt ordnar sessioner lodrätt i hello mobila webbläsare.</span><span class="sxs-lookup"><span data-stu-id="b8e77-293">hello Bootstrap grid system that you applied automatically arranges the sessions vertically in hello mobile browser.</span></span> <span data-ttu-id="b8e77-294">Observera också att hello taggar inte visas.</span><span class="sxs-lookup"><span data-stu-id="b8e77-294">Also, notice that hello tags are not shown.</span></span> <span data-ttu-id="b8e77-295">Växla toohello skrivbord webbläsare.</span><span class="sxs-lookup"><span data-stu-id="b8e77-295">Switch toohello desktop browser.</span></span>

![][SessionsTableFixedTagASP.NETDesktop]

<span data-ttu-id="b8e77-296">Observera att hello taggar visas nu i hello skrivbord webbläsare.</span><span class="sxs-lookup"><span data-stu-id="b8e77-296">In hello desktop browser, notice that hello tags are now displayed.</span></span> <span data-ttu-id="b8e77-297">Du kan också se att hello Bootstrap nätet du använt ordnar hello session objekt i två kolumner.</span><span class="sxs-lookup"><span data-stu-id="b8e77-297">Also, you can see that hello Bootstrap grid system you applied arranges hello session items in two columns.</span></span> <span data-ttu-id="b8e77-298">Om du förstorar webbläsaren visas som hello arrangemang ändringar toothree kolumner.</span><span class="sxs-lookup"><span data-stu-id="b8e77-298">If you enlarge the browser, you will see that hello arrangement changes toothree columns.</span></span>

## <span data-ttu-id="b8e77-299"><a name="bkmk_improvesessionbycode"></a>Förbättra hello SessionByCode vy</span><span class="sxs-lookup"><span data-stu-id="b8e77-299"><a name="bkmk_improvesessionbycode"></a> Improve hello SessionByCode View</span></span>
<span data-ttu-id="b8e77-300">Slutligen hello korrigera *SessionByCode* visa toomake den mobilvänlig.</span><span class="sxs-lookup"><span data-stu-id="b8e77-300">Finally, you'll fix hello *SessionByCode* view toomake it mobile-friendly.</span></span>

<span data-ttu-id="b8e77-301">Tryck på hello i hello mobila webbläsare **taggen** knappen och klicka sedan på Ange `asp` i sökrutan.</span><span class="sxs-lookup"><span data-stu-id="b8e77-301">In hello mobile browser, tap hello **Tag** button, then enter `asp` in the search box.</span></span>

![][AllTagsFixedSearchByASP]

<span data-ttu-id="b8e77-302">Tryck på hello **ASP.NET** länk.</span><span class="sxs-lookup"><span data-stu-id="b8e77-302">Tap hello **ASP.NET** link.</span></span> <span data-ttu-id="b8e77-303">Sessioner hello ASP.NET taggar för visas.</span><span class="sxs-lookup"><span data-stu-id="b8e77-303">Sessions for hello ASP.NET tag are displayed.</span></span>

![][FixedSessionsByTag]

<span data-ttu-id="b8e77-304">Välj hello **skapa ett program med ASP.NET och AngularJS sidan** länk.</span><span class="sxs-lookup"><span data-stu-id="b8e77-304">Choose hello **Building a Single Page Application with ASP.NET and AngularJS** link.</span></span>

![][SessionByCode3-644]

<span data-ttu-id="b8e77-305">hello standardvyn för fjärrskrivbord är bra, men du kan förbättra hello utseende enkelt vissa Bootstrap GUI-komponenter.</span><span class="sxs-lookup"><span data-stu-id="b8e77-305">hello default desktop view is fine, but you can improve hello look easily by using some Bootstrap GUI components.</span></span>

<span data-ttu-id="b8e77-306">Öppna *vyer\\Start\\SessionByCode.cshtml* och Ersätt hello innehållet med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="b8e77-306">Open *Views\\Home\\SessionByCode.cshtml* and replace hello contents with hello following markup:</span></span>

    @model Mvc5Mobile.Models.Session

    @{
        ViewBag.Title = "Session details";
    }
    <h3>@Model.Title (@Model.Code)</h3>
    <p>
        <strong>@Model.DateText</strong> in <strong>@Model.Room</strong>
    </p>

    <div class="panel panel-primary">
        <div class="panel-heading">
            Speakers
        </div>
        @foreach (var speaker in Model.Speakers)
        {
            @Html.ActionLink(speaker, 
                             "SessionsBySpeaker", 
                             new { speaker }, 
                             new { @class="panel-body" })
        }
    </div>

    <p>@Model.Abstract</p>

    <div class="panel panel-primary">
        <div class="panel-heading">
            Tags
        </div>
        @foreach (var tag in Model.Tags)
        {
            @Html.ActionLink(tag, 
                             "SessionsByTag", 
                             new { tag }, 
                             new { @class = "panel-body" })
        }
    </div>

<span data-ttu-id="b8e77-307">hello nya markup använder Bootstrap paneler formatering tooimprove hello mobila vyn.</span><span class="sxs-lookup"><span data-stu-id="b8e77-307">hello new markup uses Bootstrap panels styling tooimprove hello mobile view.</span></span> 

<span data-ttu-id="b8e77-308">Uppdatera hello mobila webbläsare.</span><span class="sxs-lookup"><span data-stu-id="b8e77-308">Refresh hello mobile browser.</span></span> <span data-ttu-id="b8e77-309">hello visar följande bild hello kodändringar som du skapat:</span><span class="sxs-lookup"><span data-stu-id="b8e77-309">hello following image reflects hello code changes that you just made:</span></span>

![][SessionByCodeFixed3-644]

## <a name="wrap-up-and-review"></a><span data-ttu-id="b8e77-310">Sammanfattning och granska</span><span class="sxs-lookup"><span data-stu-id="b8e77-310">Wrap Up and Review</span></span>
<span data-ttu-id="b8e77-311">Den här kursen visar du hur toouse ASP.NET MVC 5 toodevelop mobilvänlig webbprogram.</span><span class="sxs-lookup"><span data-stu-id="b8e77-311">This tutorial has shown you how toouse ASP.NET MVC 5 toodevelop mobile-friendly Web applications.</span></span> <span data-ttu-id="b8e77-312">Exempel på dessa är:</span><span class="sxs-lookup"><span data-stu-id="b8e77-312">These include:</span></span>

* <span data-ttu-id="b8e77-313">Distribuera ett ASP.NET MVC 5 programmet tooan App Service web-appen</span><span class="sxs-lookup"><span data-stu-id="b8e77-313">Deploy an ASP.NET MVC 5 application tooan App Service web app</span></span>
* <span data-ttu-id="b8e77-314">Använd Bootstrap toocreate responsiv Webblayout i MVC 5-program</span><span class="sxs-lookup"><span data-stu-id="b8e77-314">Use Bootstrap toocreate responsive web layout in your MVC 5 application</span></span>
* <span data-ttu-id="b8e77-315">Åsidosätt layout, vyer och partiella vyer, både globalt och för en enskild vy</span><span class="sxs-lookup"><span data-stu-id="b8e77-315">Override layout, views, and partial views, both globally and for an individual view</span></span>
* <span data-ttu-id="b8e77-316">Kontrollens layout och partiella åsidosätter tvingande med hjälp av den `RequireConsistentDisplayMode` egenskapen</span><span class="sxs-lookup"><span data-stu-id="b8e77-316">Control layout and partial override enforcement using the `RequireConsistentDisplayMode` property</span></span>
* <span data-ttu-id="b8e77-317">Skapa vyer som är avsedda för vissa webbläsare, till exempel hello iPhone webbläsare</span><span class="sxs-lookup"><span data-stu-id="b8e77-317">Create views that target specific browsers, such as hello iPhone browser</span></span>
* <span data-ttu-id="b8e77-318">Tillämpa Bootstrap formatmalls i Razor-kod</span><span class="sxs-lookup"><span data-stu-id="b8e77-318">Apply Bootstrap styling in Razor code</span></span>

## <a name="see-also"></a><span data-ttu-id="b8e77-319">Se även</span><span class="sxs-lookup"><span data-stu-id="b8e77-319">See Also</span></span>
* [<span data-ttu-id="b8e77-320">9 grundläggande principerna för dynamisk webbdesign</span><span class="sxs-lookup"><span data-stu-id="b8e77-320">9 basic principles of responsive web design</span></span>](http://blog.froont.com/9-basic-principles-of-responsive-web-design/)
* <span data-ttu-id="b8e77-321">[Starttjänsten][BootstrapSite]</span><span class="sxs-lookup"><span data-stu-id="b8e77-321">[Bootstrap][BootstrapSite]</span></span>
* <span data-ttu-id="b8e77-322">[Officiell Bootstrap blogg][Official Bootstrap Blog]</span><span class="sxs-lookup"><span data-stu-id="b8e77-322">[Official Bootstrap Blog][Official Bootstrap Blog]</span></span>
* <span data-ttu-id="b8e77-323">[Twitter Bootstrap kursen från kursen Republiken][Twitter Bootstrap Tutorial from Tutorial Republic]</span><span class="sxs-lookup"><span data-stu-id="b8e77-323">[Twitter Bootstrap Tutorial from Tutorial Republic][Twitter Bootstrap Tutorial from Tutorial Republic]</span></span>
* <span data-ttu-id="b8e77-324">[hello Bootstrap Playground][hello Bootstrap Playground]</span><span class="sxs-lookup"><span data-stu-id="b8e77-324">[hello Bootstrap Playground][hello Bootstrap Playground]</span></span>
* <span data-ttu-id="b8e77-325">[Metodtips för W3C rekommendation mobilen program][W3C Recommendation Mobile Web Application Best Practices]</span><span class="sxs-lookup"><span data-stu-id="b8e77-325">[W3C Recommendation Mobile Web Application Best Practices][W3C Recommendation Mobile Web Application Best Practices]</span></span>
* <span data-ttu-id="b8e77-326">[W3C kandidat rekommendation för media frågor][W3C Candidate Recommendation for media queries]</span><span class="sxs-lookup"><span data-stu-id="b8e77-326">[W3C Candidate Recommendation for media queries][W3C Candidate Recommendation for media queries]</span></span>

## <a name="whats-changed"></a><span data-ttu-id="b8e77-327">Nyheter</span><span class="sxs-lookup"><span data-stu-id="b8e77-327">What's changed</span></span>
* <span data-ttu-id="b8e77-328">En guide toohello övergången från webbplatser tooApp tjänsten finns: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="b8e77-328">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

<!-- Internal Links -->
[Deploy hello starter project tooan Azure web app]: #bkmk_DeployStarterProject
[Bootstrap CSS Framework]: #bkmk_bootstrap
[Override hello Views, Layouts, and Partial Views]: #bkmk_overrideviews
[Create Browser-Specific Views]:#bkmk_browserviews
[Improve hello Speakers List]: #bkmk_Improvespeakerslist
[Improve hello Tags List]: #bkmk_improvetags
[Improve hello Dates List]: #bkmk_improvedates
[Improve hello SessionsTable View]: #bkmk_improvesessionstable
[Improve hello SessionByCode View]: #bkmk_improvesessionbycode

<!-- External Links -->
[Visual Studio Express 2013]: http://www.visualstudio.com/downloads/download-visual-studio-vs#d-express-web
[Visual Studio 2015]: https://www.visualstudio.com/downloads/download-visual-studio-vs
[AzureSDKVs2013]: http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409
[Fiddler]: http://www.fiddler2.com/fiddler2/
[EmulatorIE11]: http://msdn.microsoft.com/library/ie/dn255001.aspx
[EmulatorChrome]: https://developers.google.com/chrome-developer-tools/docs/mobile-emulation
[EmulatorOpera]: http://www.opera.com/developer/tools/mobile/
[StarterProject]: http://go.microsoft.com/fwlink/?LinkID=398780&clcid=0x409
[CompletedProject]: http://go.microsoft.com/fwlink/?LinkID=398781&clcid=0x409
[BootstrapSite]: http://getbootstrap.com/
[WebPIAzureSdk23NetVS13]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/WebPIAzureSdk23NetVS13.png
[linked list group]: http://getbootstrap.com/components/#list-group-linked
[glyphicon]: http://getbootstrap.com/components/#glyphicons
[panels]: http://getbootstrap.com/components/#panels
[custom linked list group]: http://getbootstrap.com/components/#list-group-custom-content
[grid system]: http://getbootstrap.com/css/#grid
[responsive utilities]: http://getbootstrap.com/css/#responsive-utilities
[Official Bootstrap Blog]: http://blog.getbootstrap.com/
[Twitter Bootstrap Tutorial from Tutorial Republic]: http://www.tutorialrepublic.com/twitter-bootstrap-tutorial/
[hello Bootstrap Playground]: http://www.bootply.com/
[W3C Recommendation Mobile Web Application Best Practices]: http://www.w3.org/TR/mwabp/
[W3C Candidate Recommendation for media queries]: http://www.w3.org/TR/css3-mediaqueries/

<!-- Images -->
[DeployClickPublish]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-1.png
[DeployClickWebSites]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-2.png
[DeploySignIn]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-3.png
[DeployUsername]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-4.png
[DeployPassword]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-5.png
[DeployNewWebsite]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-6.png
[DeploySiteSettings]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-7.png
[DeployPublishSite]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-8.png
[MobileHomePage]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/mobile-home-page.png
[FixedSessionsByTag]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionsByTag-ASP.NET-Fixed.png
[AllTags]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTags.png
[SessionsByTagASP.NET]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionsByTag-ASP.NET.png
[SessionsByTagASP.NETNoBootstrap]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionsByTag-ASP.NET-NoBootstrap.png
[AllTagsMobile_LayoutMobile]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTagsMobile-_LayoutMobile.png
[AllTagsMobile_LayoutMobileDesktop]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTagsMobile-_LayoutMobile-Desktop.png
[ResolveDefaultDisplayMode]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/Resolve-DefaultDisplayMode.png
[AllTagsIPhone_LayoutIPhone]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTagsIPhone-_LayoutIPhone.png
[AllSpeakers_LayoutMobile]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllSpeakers-_LayoutMobile.png
[AllSpeakers_LayoutMobileOverridden]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllSpeakers-_LayoutMobile-Overridden.png
[AllSpeakersFixed]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllSpeakers-Fixed.png
[AllSpeakersFixedDesktop]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllSpeakers-Fixed-Desktop.png
[AllSpeakersFixedSearchBySC]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllSpeakers-Fixed-SearchBySC.png
[AllTagsFixedDesktop]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTags-Fixed-Desktop.png 
[AllTagsFixed]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTags-Fixed.png
[AllDatesFixed]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllDates-Fixed.png
[AllDatesFixed2]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllDates-Fixed2.png
[AllDatesFixed2Desktop]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllDates-Fixed2-Desktop.png
[AllTagsFixedSearchByASP]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTags-Fixed-SearchByASP.png
[SessionsTableTagASP.NET]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionsTable-Tag-ASP.NET.png
[SessionsTableFixedTagASP.NETDesktop]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionsTable-Fixed-Tag-ASP.NET-Desktop.png
[SessionByCode3-644]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionByCode-3-644.png
[SessionByCodeFixed3-644]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionByCode-Fixed-3-644.png

