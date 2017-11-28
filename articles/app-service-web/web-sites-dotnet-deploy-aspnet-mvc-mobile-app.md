---
title: Distribuera ett ASP.NET MVC 5 mobila webbappen i Azure App Service
description: "En självstudiekurs som lär du dig hur du distribuerar en webbapp till Azure App Service med mobila funktioner i ASP.NET MVC 5 webbprogram."
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
ms.openlocfilehash: c98e9b485c52a82e5be5c0f6b0b67912d1e890b9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-an-aspnet-mvc-5-mobile-web-app-in-azure-app-service"></a><span data-ttu-id="08ae8-103">Distribuera ett ASP.NET MVC 5 mobila webbappen i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="08ae8-103">Deploy an ASP.NET MVC 5 mobile web app in Azure App Service</span></span>
<span data-ttu-id="08ae8-104">Den här kursen lär du dig grunderna för hur du skapar ett webbprogram för ASP.NET MVC 5 som mobilvänlig och distribuera den till Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="08ae8-104">This tutorial will teach you the basics of how to build an ASP.NET MVC 5 web app that is mobile-friendly and deploy it to Azure App Service.</span></span> <span data-ttu-id="08ae8-105">För den här kursen behöver du [Visual Studio Express 2013 för webben] [ Visual Studio Express 2013] eller professionell utgåva av Visual Studio om du redan har som.</span><span class="sxs-lookup"><span data-stu-id="08ae8-105">For this tutorial, you need [Visual Studio Express 2013 for Web][Visual Studio Express 2013] or the professional edition of Visual Studio if you already have that.</span></span> <span data-ttu-id="08ae8-106">Du kan använda [Visual Studio 2015] men skärmdumpar är olika och måste du använda mallar för ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="08ae8-106">You can use [Visual Studio 2015] but the screen shots will be different and you must use the ASP.NET 4.x templates.</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

## <a name="what-youll-build"></a><span data-ttu-id="08ae8-107">Vad du ska skapa</span><span class="sxs-lookup"><span data-stu-id="08ae8-107">What You'll Build</span></span>
<span data-ttu-id="08ae8-108">Den här självstudiekursen ska du lägga till mobila funktioner enkelt konferens lista-program som har angetts i den [starter projektet][StarterProject].</span><span class="sxs-lookup"><span data-stu-id="08ae8-108">For this tutorial, you'll add mobile features to the simple conference-listing application that's provided in the [starter project][StarterProject].</span></span> <span data-ttu-id="08ae8-109">Följande skärmbild visar ASP.NET-sessioner i det färdiga programmet som visas i webbläsaren emulatorn i Internet Explorer 11 F12 utvecklingsverktyg.</span><span class="sxs-lookup"><span data-stu-id="08ae8-109">The following screenshot shows the ASP.NET sessions in the completed application, as seen in the browser emulator in Internet Explorer 11 F12 developer tools.</span></span>

![][FixedSessionsByTag]

<span data-ttu-id="08ae8-110">Du kan använda Internet Explorer 11 F12 utvecklingsverktyg och [Fiddler verktyget] [ Fiddler] för att felsöka programmet.</span><span class="sxs-lookup"><span data-stu-id="08ae8-110">You can use the Internet Explorer 11 F12 developer tools and the [Fiddler tool][Fiddler] to help debug your application.</span></span> 

## <a name="skills-youll-learn"></a><span data-ttu-id="08ae8-111">Kunskaper lär du dig</span><span class="sxs-lookup"><span data-stu-id="08ae8-111">Skills You'll Learn</span></span>
<span data-ttu-id="08ae8-112">Här är lär du dig:</span><span class="sxs-lookup"><span data-stu-id="08ae8-112">Here's what you'll learn:</span></span>

* <span data-ttu-id="08ae8-113">Hur du använder Visual Studio 2013 för att publicera webbprogrammet direkt till en webbapp i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="08ae8-113">How to use Visual Studio 2013 to publish your web application directly to a web app in Azure App Service.</span></span>
* <span data-ttu-id="08ae8-114">Hur ASP.NET MVC 5 mallar använder CSS Bootstrap-ramverket för att förbättra visas på mobila enheter</span><span class="sxs-lookup"><span data-stu-id="08ae8-114">How the ASP.NET MVC 5 templates use the CSS Bootstrap framework to improve display on mobile devices</span></span>
* <span data-ttu-id="08ae8-115">Så här skapar du mobile-specifika vyer om du vill rikta specifika mobila webbläsare, till exempel iPhone- och Android</span><span class="sxs-lookup"><span data-stu-id="08ae8-115">How to create mobile-specific views to target specific mobile browsers, such as the iPhone and Android</span></span>
* <span data-ttu-id="08ae8-116">Så här skapar du responsiv vyer (vyer som svarar på olika webbläsare mellan enheter)</span><span class="sxs-lookup"><span data-stu-id="08ae8-116">How to create responsive views (views that respond to different browsers across devices)</span></span>

## <a name="set-up-the-development-environment"></a><span data-ttu-id="08ae8-117">Konfigurera utvecklingsmiljön</span><span class="sxs-lookup"><span data-stu-id="08ae8-117">Set up the development environment</span></span>
<span data-ttu-id="08ae8-118">Ställ in din utvecklingsmiljö genom att installera Azure SDK för .NET 2.5.1 eller senare.</span><span class="sxs-lookup"><span data-stu-id="08ae8-118">Set up your development environment by installing the Azure SDK for .NET 2.5.1 or later.</span></span> 

1. <span data-ttu-id="08ae8-119">Klicka på länken nedan om du vill installera Azure SDK för .NET.</span><span class="sxs-lookup"><span data-stu-id="08ae8-119">To install the Azure SDK for .NET, click the link below.</span></span> <span data-ttu-id="08ae8-120">Om du inte har Visual Studio 2013 har installerats ännu, kommer att installeras av länken.</span><span class="sxs-lookup"><span data-stu-id="08ae8-120">If you don't have Visual Studio 2013 installed yet, it will be installed by the link.</span></span> <span data-ttu-id="08ae8-121">Den här kursen kräver Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="08ae8-121">This tutorial requires Visual Studio 2013.</span></span> <span data-ttu-id="08ae8-122">[Azure SDK för Visual Studio 2013][AzureSDKVs2013]</span><span class="sxs-lookup"><span data-stu-id="08ae8-122">[Azure SDK for Visual Studio 2013][AzureSDKVs2013]</span></span>
2. <span data-ttu-id="08ae8-123">Klicka på i fönstret Installationsprogram för webbplattform **installera** och fortsätta med installationen.</span><span class="sxs-lookup"><span data-stu-id="08ae8-123">In the Web Platform Installer window, click **Install** and proceed with the installation.</span></span>

<span data-ttu-id="08ae8-124">Du måste också en emulator mobila webbläsare.</span><span class="sxs-lookup"><span data-stu-id="08ae8-124">You will also need a mobile browser emulator.</span></span> <span data-ttu-id="08ae8-125">Något av följande fungerar:</span><span class="sxs-lookup"><span data-stu-id="08ae8-125">Any of the following will work:</span></span>

* <span data-ttu-id="08ae8-126">Webbläsaren emulatorn i [utvecklingsverktyg för Internet Explorer 11 F12] [ EmulatorIE11] (används i alla mobila webbläsare skärmbilder).</span><span class="sxs-lookup"><span data-stu-id="08ae8-126">Browser Emulator in [Internet Explorer 11 F12 developer tools][EmulatorIE11] (used in all mobile browser screenshots).</span></span> <span data-ttu-id="08ae8-127">Den har användaren agent sträng förinställningar för Windows Phone 8, Windows Phone 7 och Apple iPad.</span><span class="sxs-lookup"><span data-stu-id="08ae8-127">It has user agent string presets for Windows Phone 8, Windows Phone 7, and Apple iPad.</span></span>
* <span data-ttu-id="08ae8-128">Webbläsaren emulatorn i [Google Chrome DevTools][EmulatorChrome].</span><span class="sxs-lookup"><span data-stu-id="08ae8-128">Browser Emulator in [Google Chrome DevTools][EmulatorChrome].</span></span> <span data-ttu-id="08ae8-129">Den innehåller förinställningar för flera Android-enheter samt Apple iPhone, iPad Apple och Amazon Kindle brand.</span><span class="sxs-lookup"><span data-stu-id="08ae8-129">It contains presets for numerous Android devices, as well as Apple iPhone, Apple iPad, and Amazon Kindle Fire.</span></span> <span data-ttu-id="08ae8-130">Det kan också emulerar touch-händelser.</span><span class="sxs-lookup"><span data-stu-id="08ae8-130">It also emulates touch events.</span></span>
* <span data-ttu-id="08ae8-131">[Opera Mobilemulator][EmulatorOpera]</span><span class="sxs-lookup"><span data-stu-id="08ae8-131">[Opera Mobile Emulator][EmulatorOpera]</span></span>

<span data-ttu-id="08ae8-132">Visual Studio-projekt med C\# källkoden är tillgängliga som åtföljer det här avsnittet:</span><span class="sxs-lookup"><span data-stu-id="08ae8-132">Visual Studio projects with C\# source code are available to accompany this topic:</span></span>

* <span data-ttu-id="08ae8-133">[Ladda ned Starter-projekt][StarterProject]</span><span class="sxs-lookup"><span data-stu-id="08ae8-133">[Starter project download][StarterProject]</span></span>
* <span data-ttu-id="08ae8-134">[Slutfört projekt hämtning][CompletedProject]</span><span class="sxs-lookup"><span data-stu-id="08ae8-134">[Completed project download][CompletedProject]</span></span>

## <span data-ttu-id="08ae8-135"><a name="bkmk_DeployStarterProject"></a>Distribuera starter-projektet till en Azure-webbapp</span><span class="sxs-lookup"><span data-stu-id="08ae8-135"><a name="bkmk_DeployStarterProject"></a>Deploy the starter project to an Azure web app</span></span>
1. <span data-ttu-id="08ae8-136">Hämta programmet konferens lista [starter projektet][StarterProject].</span><span class="sxs-lookup"><span data-stu-id="08ae8-136">Download the conference-listing application [starter project][StarterProject].</span></span>
2. <span data-ttu-id="08ae8-137">Högerklicka på den hämta ZIP-filen i Utforskaren och välj sedan *egenskaper*.</span><span class="sxs-lookup"><span data-stu-id="08ae8-137">Then in Windows Explorer, right-click the downloaded ZIP file and choose *Properties*.</span></span>
3. <span data-ttu-id="08ae8-138">I den **egenskaper** dialogrutan väljer du den **avblockera** knappen.</span><span class="sxs-lookup"><span data-stu-id="08ae8-138">In the **Properties** dialog box, choose the **Unblock** button.</span></span> <span data-ttu-id="08ae8-139">(Avblockera förhindrar att en säkerhetsvarning som uppstår vid försök att använda en *.zip* -fil som du har laddat ned från webben.)</span><span class="sxs-lookup"><span data-stu-id="08ae8-139">(Unblocking prevents a security warning that occurs when you try to use a *.zip* file that you've downloaded from the web.)</span></span>
4. <span data-ttu-id="08ae8-140">Högerklicka på ZIP-filen och välj **extrahera alla** att packa upp filen.</span><span class="sxs-lookup"><span data-stu-id="08ae8-140">Right-click the ZIP file and select **Extract All** to unzip the file.</span></span> 
5. <span data-ttu-id="08ae8-141">Öppna i Visual Studio den *C#\Mvc5Mobile.sln* fil.</span><span class="sxs-lookup"><span data-stu-id="08ae8-141">In Visual Studio, open the *C#\Mvc5Mobile.sln* file.</span></span>
6. <span data-ttu-id="08ae8-142">Högerklicka på projektet i Solution Explorer och klicka på **publicera**.</span><span class="sxs-lookup"><span data-stu-id="08ae8-142">In Solution Explorer, right-click the project and click **Publish**.</span></span>
   
   ![][DeployClickPublish]
7. <span data-ttu-id="08ae8-143">Klicka på Publicera webbplats **Microsoft Azure App Service**.</span><span class="sxs-lookup"><span data-stu-id="08ae8-143">In Publish Web, click **Microsoft Azure App Service**.</span></span>
   
   ![][DeployClickWebSites]
8. <span data-ttu-id="08ae8-144">Om du inte har redan loggat in på Azure, klickar du på **Lägg till ett konto**.</span><span class="sxs-lookup"><span data-stu-id="08ae8-144">If you haven't already logged into Azure, click **Add an account**.</span></span>
   
   ![][DeploySignIn]
9. <span data-ttu-id="08ae8-145">Följ anvisningarna för att logga in på ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="08ae8-145">Follow the prompts to log into your Azure account.</span></span>
10. <span data-ttu-id="08ae8-146">Dialogrutan för Apptjänst ska nu visa du som loggat in.</span><span class="sxs-lookup"><span data-stu-id="08ae8-146">The App Service dialog should now show you as signed in.</span></span> <span data-ttu-id="08ae8-147">Klicka på **Ny**.</span><span class="sxs-lookup"><span data-stu-id="08ae8-147">Click **New**.</span></span>
    
    ![][DeployNewWebsite]  
11. <span data-ttu-id="08ae8-148">I den **Webbprogramnamnet** anger ett unikt app namnprefix.</span><span class="sxs-lookup"><span data-stu-id="08ae8-148">In the **Web App Name** field, specify a unique app name prefix.</span></span> <span data-ttu-id="08ae8-149">Din fullständiga webbprogrammets namn kommer att  *&lt;prefix >*. azurewebsites.net.</span><span class="sxs-lookup"><span data-stu-id="08ae8-149">Your fully-qualified web app name will be *&lt;prefix>*.azurewebsites.net.</span></span> <span data-ttu-id="08ae8-150">Dessutom väljer eller anger ett namn på ny resursgrupp i **resursgruppen**.</span><span class="sxs-lookup"><span data-stu-id="08ae8-150">Also, select or specify a new resource group name in **Resource group**.</span></span> <span data-ttu-id="08ae8-151">Klicka på **ny** att skapa en ny programtjänstplan.</span><span class="sxs-lookup"><span data-stu-id="08ae8-151">Then, click **New** to create a new App Service plan.</span></span>
    
    ![][DeploySiteSettings]
12. <span data-ttu-id="08ae8-152">Konfigurera den nya programtjänstplanen och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="08ae8-152">Configure the new App Service plan and click **OK**.</span></span> 
    
    ![](./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-7a.png)
13. <span data-ttu-id="08ae8-153">Klicka på tillbaka i dialogrutan Skapa Apptjänst **skapa**.</span><span class="sxs-lookup"><span data-stu-id="08ae8-153">Back in the Create App Service dialog, click **Create**.</span></span>
    
    ![](./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-7b.png) 
14. <span data-ttu-id="08ae8-154">När Azure skapas resurser i Publicera webbplats dialogrutan fylls med inställningarna för den nya appen.</span><span class="sxs-lookup"><span data-stu-id="08ae8-154">After the Azure resources are created, the Publish Web dialog will be filled with the settings for your new app.</span></span> <span data-ttu-id="08ae8-155">Klicka på **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="08ae8-155">Click **Publish**.</span></span>
    
    ![][DeployPublishSite]
    
    <span data-ttu-id="08ae8-156">När Visual Studio har slutförts publicering starter-projektet till Azure-webbappen öppnas skrivbord webbläsaren om du vill visa live-webb-app.</span><span class="sxs-lookup"><span data-stu-id="08ae8-156">Once Visual Studio finishes publishing the starter project to the Azure web app, the desktop browser opens to display the live web app.</span></span>
15. <span data-ttu-id="08ae8-157">Starta din mobila webbläsare emulator, Kopiera URL-Adressen för programmets konferens (*<prefix>*. azurewebsites.net) i emulatorn och sedan klicka på knappen längst upp till höger och välj **Bläddra efter tagg**.</span><span class="sxs-lookup"><span data-stu-id="08ae8-157">Start your mobile browser emulator, copy the URL for the conference application (*<prefix>*.azurewebsites.net) into the emulator, and then click the top-right button and select **Browse by tag**.</span></span> <span data-ttu-id="08ae8-158">Om du använder Internet Explorer 11 som standardwebbläsare, du behöver bara ange `F12`, sedan `Ctrl+8`, och sedan ändra profilen som webbläsaren ska **Windows Phone**.</span><span class="sxs-lookup"><span data-stu-id="08ae8-158">If you are using Internet Explorer 11 as the default browser, you just need to type `F12`, then `Ctrl+8`, and then change the browser profile to **Windows Phone**.</span></span> <span data-ttu-id="08ae8-159">Bilden nedan visar den *AllTags* vyn i stående läge (från att välja **Bläddra efter tagg**).</span><span class="sxs-lookup"><span data-stu-id="08ae8-159">The image below shows the *AllTags* view in portrait mode (from choosing **Browse by tag**).</span></span>
    
    ![][AllTags]

> [!TIP]
> <span data-ttu-id="08ae8-160">Medan du kan felsöka MVC 5 programmet från Visual Studio, kan du publicera webbappen till Azure igen för att verifiera live webbprogrammet direkt från din mobila webbläsare eller en webbläsare emulator.</span><span class="sxs-lookup"><span data-stu-id="08ae8-160">While you can debug your MVC 5 application from within Visual Studio, you can publish your web app to Azure again to verify the live web app directly from your mobile browser or a browser emulator.</span></span>
> 
> 

<span data-ttu-id="08ae8-161">Visningen kan mycket läsas på en mobil enhet.</span><span class="sxs-lookup"><span data-stu-id="08ae8-161">The display is very readable on a mobile device.</span></span> <span data-ttu-id="08ae8-162">Du kan också redan se några visual effekter som används av Bootstrap CSS-ramverket.</span><span class="sxs-lookup"><span data-stu-id="08ae8-162">You can also already see some of the visual effects applied by the Bootstrap CSS framework.</span></span>
<span data-ttu-id="08ae8-163">Klicka på den **ASP.NET** länk.</span><span class="sxs-lookup"><span data-stu-id="08ae8-163">Click the **ASP.NET** link.</span></span>

![][SessionsByTagASP.NET]

<span data-ttu-id="08ae8-164">ASP.NET-tagg vyn är zoomning monterade på skärmen som Bootstrap sparas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="08ae8-164">The ASP.NET tag view is zoom-fitted to the screen, which Bootstrap does for you automatically.</span></span> <span data-ttu-id="08ae8-165">Du kan dock förbättra den här vyn så att de bättre passar din mobila webbläsare.</span><span class="sxs-lookup"><span data-stu-id="08ae8-165">However, you can improve this view to better suit the mobile browser.</span></span> <span data-ttu-id="08ae8-166">Till exempel den **datum** kolumnen är svåra att läsa.</span><span class="sxs-lookup"><span data-stu-id="08ae8-166">For example, the **Date** column is difficult to read.</span></span> <span data-ttu-id="08ae8-167">Senare under kursen ska du ändra den *AllTags* vy så att den mobilvänlig.</span><span class="sxs-lookup"><span data-stu-id="08ae8-167">Later in the tutorial you'll change the *AllTags* view to make it mobile-friendly.</span></span>

## <span data-ttu-id="08ae8-168"><a name="bkmk_bootstrap"></a>Bootstrap CSS Framework</span><span class="sxs-lookup"><span data-stu-id="08ae8-168"><a name="bkmk_bootstrap"></a> Bootstrap CSS Framework</span></span>
<span data-ttu-id="08ae8-169">Nya är MVC 5 inbyggt stöd för Bootstrap på mallen.</span><span class="sxs-lookup"><span data-stu-id="08ae8-169">New in the MVC 5 template is built-in Bootstrap support.</span></span> <span data-ttu-id="08ae8-170">Du har redan sett hur de olika vyerna i tillämpningsprogrammet omedelbart förbättras.</span><span class="sxs-lookup"><span data-stu-id="08ae8-170">You have already seen how it immediately improves the different views in your application.</span></span> <span data-ttu-id="08ae8-171">Navigeringsfältet längst upp är till exempel automatiskt döljas när webbläsaren är mindre.</span><span class="sxs-lookup"><span data-stu-id="08ae8-171">For example, the navigation bar at the top is automatically collapsible when the browser width is smaller.</span></span> <span data-ttu-id="08ae8-172">Försök ändra storlek på webbläsarfönstret skrivbord webbläsare och se hur navigeringsfältet ändras dess utseende.</span><span class="sxs-lookup"><span data-stu-id="08ae8-172">On the desktop browser, try resizing the browser window and see how the navigation bar changes its look and feel.</span></span> <span data-ttu-id="08ae8-173">Detta är dynamisk webbdesign som är inbyggd i starttjänsten.</span><span class="sxs-lookup"><span data-stu-id="08ae8-173">This is the responsive web design that is built into Bootstrap.</span></span>

<span data-ttu-id="08ae8-174">Om du vill se hur webbappen ser ut utan Bootstrap, öppna *App\_starta\\BundleConfig.cs* och kommentera ut rader som innehåller *bootstrap.js* och *bootstrap.css*.</span><span class="sxs-lookup"><span data-stu-id="08ae8-174">To see how the Web app would look without Bootstrap, open *App\_Start\\BundleConfig.cs* and comment out the lines that contain *bootstrap.js* and *bootstrap.css*.</span></span> <span data-ttu-id="08ae8-175">Följande kod visar de två sista rapporterna för den `RegisterBundles` metoden när ändringen:</span><span class="sxs-lookup"><span data-stu-id="08ae8-175">The following code shows the last two statements of the `RegisterBundles` method after the change:</span></span>

     bundles.Add(new ScriptBundle("~/bundles/bootstrap").Include(
              //"~/Scripts/bootstrap.js",
              "~/Scripts/respond.js"));

    bundles.Add(new StyleBundle("~/Content/css").Include(
              //"~/Content/bootstrap.css",
              "~/Content/site.css"));

<span data-ttu-id="08ae8-176">Tryck på `Ctrl+F5` att köra programmet.</span><span class="sxs-lookup"><span data-stu-id="08ae8-176">Press `Ctrl+F5` to run the application.</span></span>

<span data-ttu-id="08ae8-177">Observera att döljas navigeringsfältet nu är bara en vanlig osorterad lista.</span><span class="sxs-lookup"><span data-stu-id="08ae8-177">Observe that the collapsible navigation bar is now just an ordinary unordered list.</span></span> <span data-ttu-id="08ae8-178">Klicka på **Bläddra efter tagg** igen och klicka sedan på **ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="08ae8-178">Click **Browse by tag** again, then click **ASP.NET**.</span></span>
<span data-ttu-id="08ae8-179">I vyn mobilemulator ser nu att den inte längre zoomning monterade på skärmen och du måste rulla åt sidan för att visa till höger i tabellen.</span><span class="sxs-lookup"><span data-stu-id="08ae8-179">In the mobile emulator view, you can see now that it is no longer zoom-fitted to the screen, and you must scroll sideways in order to see the right side of the table.</span></span>

![][SessionsByTagASP.NETNoBootstrap]

<span data-ttu-id="08ae8-180">Ångra ändringarna och uppdatera din mobila webbläsare för att verifiera att mobilvänlig visningen har återställts.</span><span class="sxs-lookup"><span data-stu-id="08ae8-180">Undo your changes and refresh the mobile browser to verify that the mobile-friendly display has been restored.</span></span>

<span data-ttu-id="08ae8-181">Starttjänsten är inte specifik för ASP.NET MVC 5 och du kan dra nytta av funktionerna i alla webbprogram.</span><span class="sxs-lookup"><span data-stu-id="08ae8-181">Bootstrap is not specific to ASP.NET MVC 5, and you can take advantage of these features in any web application.</span></span> <span data-ttu-id="08ae8-182">Men det är nu inbyggda i projektmall ASP.NET MVC 5 så att ditt webbprogram för MVC 5 kan dra nytta av Bootstrap som standard.</span><span class="sxs-lookup"><span data-stu-id="08ae8-182">But it is now built into the ASP.NET MVC 5 project template, so that your MVC 5 Web application can take advantage of Bootstrap by default.</span></span>

<span data-ttu-id="08ae8-183">Mer information om Bootstrap går du till den [Bootstrap] [ BootstrapSite] plats.</span><span class="sxs-lookup"><span data-stu-id="08ae8-183">For more information about Bootstrap, go to the [Bootstrap][BootstrapSite] site.</span></span>

<span data-ttu-id="08ae8-184">I nästa avsnitt visas hur du skapar mobila webbläsare vissa vyer.</span><span class="sxs-lookup"><span data-stu-id="08ae8-184">In the next section you'll see how to provide mobile-browser specific views.</span></span>

## <span data-ttu-id="08ae8-185"><a name="bkmk_overrideviews"></a>Åsidosätt vyer, layout och partiella vyer</span><span class="sxs-lookup"><span data-stu-id="08ae8-185"><a name="bkmk_overrideviews"></a> Override the Views, Layouts, and Partial Views</span></span>
<span data-ttu-id="08ae8-186">Du kan åsidosätta en vy (inklusive partiella vyer och layouter) för mobila webbläsare i allmänhet för en enskild mobila webbläsare eller för specifika webbläsare.</span><span class="sxs-lookup"><span data-stu-id="08ae8-186">You can override any view (including layouts and partial views) for mobile browsers in general, for an individual mobile browser, or for any specific browser.</span></span> <span data-ttu-id="08ae8-187">För att ge en mobile-specifika vy, kan du kopiera en fil i vyn och lägga till *. Mobila* i filnamnet.</span><span class="sxs-lookup"><span data-stu-id="08ae8-187">To provide a mobile-specific view, you can copy a view file and add *.Mobile* to the file name.</span></span> <span data-ttu-id="08ae8-188">Till exempel för att skapa en mobil *Index* vy, kan du kopiera *vyer\\Start\\Index.cshtml* till *vyer\\Start\\Index.Mobile.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="08ae8-188">For example, to create a mobile *Index* view, you can copy *Views\\Home\\Index.cshtml* to *Views\\Home\\Index.Mobile.cshtml*.</span></span>

<span data-ttu-id="08ae8-189">I det här avsnittet skapar du en mobile-specifika layout-fil.</span><span class="sxs-lookup"><span data-stu-id="08ae8-189">In this section, you'll create a mobile-specific layout file.</span></span>

<span data-ttu-id="08ae8-190">Starta genom att kopiera *vyer\\delade\\\_Layout.cshtml* till *vyer\\delade\\\_Layout.Mobile.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="08ae8-190">To start, copy *Views\\Shared\\\_Layout.cshtml* to *Views\\Shared\\\_Layout.Mobile.cshtml*.</span></span> <span data-ttu-id="08ae8-191">Öppna  *\_Layout.Mobile.cshtml* och ändra rubriken från **MVC5 programmet** till **MVC5 program (Mobile)**.</span><span class="sxs-lookup"><span data-stu-id="08ae8-191">Open *\_Layout.Mobile.cshtml* and change the title from **MVC5 Application** to **MVC5 Application (Mobile)**.</span></span>

<span data-ttu-id="08ae8-192">I varje `Html.ActionLink` anropa för navigeringsfältet, ta bort ”Bläddra efter” i varje länk *ActionLink*.</span><span class="sxs-lookup"><span data-stu-id="08ae8-192">In each `Html.ActionLink` call for the navigation bar, remove "Browse by" in each link *ActionLink*.</span></span> <span data-ttu-id="08ae8-193">Följande kod visar den färdiga `<ul class="nav navbar-nav">` taggen för layoutfilen mobila.</span><span class="sxs-lookup"><span data-stu-id="08ae8-193">The following code shows the completed `<ul class="nav navbar-nav">` tag of the mobile layout file.</span></span>

    <ul class="nav navbar-nav">
        <li>@Html.ActionLink("Home", "Index", "Home")</li>
        <li>@Html.ActionLink("Date", "AllDates", "Home")</li>
        <li>@Html.ActionLink("Speaker", "AllSpeakers", "Home")</li>
        <li>@Html.ActionLink("Tag", "AllTags", "Home")</li>
    </ul>

<span data-ttu-id="08ae8-194">Kopiera den *vyer\\Start\\AllTags.cshtml* filen till *vyer\\Start\\AllTags.Mobile.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="08ae8-194">Copy the *Views\\Home\\AllTags.cshtml* file to *Views\\Home\\AllTags.Mobile.cshtml*.</span></span> <span data-ttu-id="08ae8-195">Öppna den nya filen och ändra den `<h2>` element från ”taggar” till ”taggar (M)”:</span><span class="sxs-lookup"><span data-stu-id="08ae8-195">Open the new file and change the `<h2>` element from "Tags" to "Tags (M)":</span></span>

    <h2>Tags (M)</h2>

<span data-ttu-id="08ae8-196">Gå till sidan taggar med en webbläsare för stationära och använder mobila webbläsare emulatorn.</span><span class="sxs-lookup"><span data-stu-id="08ae8-196">Browse to the tags page using a desktop browser and using mobile browser emulator.</span></span> <span data-ttu-id="08ae8-197">Mobila webbläsare emulatorn visar två ändringar (titel från  *\_Layout.Mobile.cshtml* och titel från *AllTags.Mobile.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="08ae8-197">The mobile browser emulator shows the two changes you made (the title from *\_Layout.Mobile.cshtml* and the title from *AllTags.Mobile.cshtml*).</span></span>

![][AllTagsMobile_LayoutMobile]

<span data-ttu-id="08ae8-198">Däremot stationära visningen inte har ändrats (med rubriker från  *\_Layout.cshtml* och *AllTags.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="08ae8-198">In contrast, the desktop display has not changed (with titles from *\_Layout.cshtml* and *AllTags.cshtml*).</span></span>

![][AllTagsMobile_LayoutMobileDesktop]

## <span data-ttu-id="08ae8-199"><a name="bkmk_browserviews"></a>Skapa specifika webbläsare vyer</span><span class="sxs-lookup"><span data-stu-id="08ae8-199"><a name="bkmk_browserviews"></a> Create Browser-Specific Views</span></span>
<span data-ttu-id="08ae8-200">Du kan skapa vyer för en enskild webbläsare förutom mobile-specifika och desktop-specifika vyer.</span><span class="sxs-lookup"><span data-stu-id="08ae8-200">In addition to mobile-specific and desktop-specific views, you can create views for an individual browser.</span></span> <span data-ttu-id="08ae8-201">Du kan till exempel skapa vyer som är specifikt för iPhone- eller Android-webbläsare.</span><span class="sxs-lookup"><span data-stu-id="08ae8-201">For example, you can create views that are specifically for the iPhone or the Android browser.</span></span> <span data-ttu-id="08ae8-202">I det här avsnittet skapar du en layout för iPhone webbläsaren och en iPhone-version av den *AllTags* vyn.</span><span class="sxs-lookup"><span data-stu-id="08ae8-202">In this section, you'll create a layout for the iPhone browser and an iPhone version of the *AllTags* view.</span></span>

<span data-ttu-id="08ae8-203">Öppna den *Global.asax* och Lägg till följande kod längst ned på den `Application_Start` metoden.</span><span class="sxs-lookup"><span data-stu-id="08ae8-203">Open the *Global.asax* file and add the following code to the bottom of the `Application_Start` method.</span></span>

    DisplayModeProvider.Instance.Modes.Insert(0, new DefaultDisplayMode("iPhone")
    {
        ContextCondition = (context => context.GetOverriddenUserAgent().IndexOf
            ("iPhone", StringComparison.OrdinalIgnoreCase) >= 0)
    });

<span data-ttu-id="08ae8-204">Den här koden definierar ett nytt visningsläge med namnet ”iPhone” som matchas mot varje inkommande begäran.</span><span class="sxs-lookup"><span data-stu-id="08ae8-204">This code defines a new display mode named "iPhone" that will be matched against each incoming request.</span></span> <span data-ttu-id="08ae8-205">Om den inkommande begäranden matchar de villkor som du definierade (det vill säga om användaragenten innehåller strängen ”iPhone”), kollar ASP.NET MVC för vyer vars namn innehåller suffixet ”iPhone”.</span><span class="sxs-lookup"><span data-stu-id="08ae8-205">If the incoming request matches the condition you defined (that is, if the user agent contains the string "iPhone"), ASP.NET MVC will look for views whose name contains the "iPhone" suffix.</span></span>

> [!NOTE]
> <span data-ttu-id="08ae8-206">När du lägger till mobila visningslägen specifika webbläsare, till exempel för iPhone- och Android, måste du ange det första argumentet `0` (Infoga överst i listan) se till att specifika webbläsare läge företräde framför mobila mallen (*. Mobile.cshtml).</span><span class="sxs-lookup"><span data-stu-id="08ae8-206">When adding mobile browser-specific display modes, such as for iPhone and Android, be sure to set the first argument to `0` (insert at the top of the list) to make sure that the browser-specific mode takes precedence over the mobile template (*.Mobile.cshtml).</span></span> <span data-ttu-id="08ae8-207">Om mallen mobila är överst i listan i stället, markeras den via din avsedda visningsläget (första matchar wins och mallen för mobila matchar alla mobila webbläsare).</span><span class="sxs-lookup"><span data-stu-id="08ae8-207">If the mobile template is at the top of the list instead, it will be selected over your intended display mode (the first match wins, and the mobile template matches all mobile browsers).</span></span> 
> 
> 

<span data-ttu-id="08ae8-208">Högerklicka i koden, `DefaultDisplayMode`, Välj **lösa**, och välj sedan `using System.Web.WebPages;`.</span><span class="sxs-lookup"><span data-stu-id="08ae8-208">In the code, right-click `DefaultDisplayMode`, choose **Resolve**, and then choose `using System.Web.WebPages;`.</span></span> <span data-ttu-id="08ae8-209">Detta lägger till en referens till den `System.Web.WebPages` namnområdet, som är var den `DisplayModeProvider` och `DefaultDisplayMode` anges.</span><span class="sxs-lookup"><span data-stu-id="08ae8-209">This adds a reference to the `System.Web.WebPages` namespace, which is where the `DisplayModeProvider` and `DefaultDisplayMode` types are defined.</span></span>

![][ResolveDefaultDisplayMode]

<span data-ttu-id="08ae8-210">Du kan också du kan bara manuellt lägga till följande rad i den `using` avsnitt i filen.</span><span class="sxs-lookup"><span data-stu-id="08ae8-210">Alternatively, you can just manually add the following line to the `using` section of the file.</span></span>

    using System.Web.WebPages;

<span data-ttu-id="08ae8-211">Spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="08ae8-211">Save the changes.</span></span> <span data-ttu-id="08ae8-212">Kopiera den *vyer\\delade\\\_Layout.Mobile.cshtml* filen till *vyer\\delade\\\_Layout.iPhone.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="08ae8-212">Copy the *Views\\Shared\\\_Layout.Mobile.cshtml* file to *Views\\Shared\\\_Layout.iPhone.cshtml*.</span></span> <span data-ttu-id="08ae8-213">Öppna den nya filen och ändra rubriken från `MVC5 Application (Mobile)` till `MVC5 Application (iPhone)`.</span><span class="sxs-lookup"><span data-stu-id="08ae8-213">Open the new file and then change the title from `MVC5 Application (Mobile)` to `MVC5 Application (iPhone)`.</span></span>

<span data-ttu-id="08ae8-214">Kopiera den *vyer\\Start\\AllTags.Mobile.cshtml* filen till *vyer\\Start\\AllTags.iPhone.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="08ae8-214">Copy the *Views\\Home\\AllTags.Mobile.cshtml* file to *Views\\Home\\AllTags.iPhone.cshtml*.</span></span> <span data-ttu-id="08ae8-215">I den nya filen ändrar den `<h2>` element från ”taggar (M)” till ”-taggar (iPhone)”.</span><span class="sxs-lookup"><span data-stu-id="08ae8-215">In the new file, change the `<h2>` element from "Tags (M)" to "Tags (iPhone)".</span></span>

<span data-ttu-id="08ae8-216">Kör appen.</span><span class="sxs-lookup"><span data-stu-id="08ae8-216">Run the application.</span></span> <span data-ttu-id="08ae8-217">Kör en emulator mobila webbläsare och kontrollera att dess användaragent är inställd på ”iPhone”, bläddra till den *AllTags* vyn.</span><span class="sxs-lookup"><span data-stu-id="08ae8-217">Run a mobile browser emulator, make sure its user agent is set to "iPhone", and browse to the *AllTags* view.</span></span> <span data-ttu-id="08ae8-218">Om du använder emulatorn i Internet Explorer 11 F12 utvecklingsverktyg, konfigurera emulering av följande:</span><span class="sxs-lookup"><span data-stu-id="08ae8-218">If you are using the emulator in Internet Explorer 11 F12 developer tools, configure emulation to the following:</span></span>

* <span data-ttu-id="08ae8-219">Webbläsaren profil = **Windows Phone**</span><span class="sxs-lookup"><span data-stu-id="08ae8-219">Browser profile = **Windows Phone**</span></span>
* <span data-ttu-id="08ae8-220">Användaragentsträngen = **anpassad**</span><span class="sxs-lookup"><span data-stu-id="08ae8-220">User agent string =  **Custom**</span></span>
* <span data-ttu-id="08ae8-221">Anpassad sträng = **1001.525-Apple-iPhone5C1**</span><span class="sxs-lookup"><span data-stu-id="08ae8-221">Custom string = **Apple-iPhone5C1/1001.525**</span></span>

<span data-ttu-id="08ae8-222">I följande skärmbild visas den *AllTags* visa återges i emulatorn i Internet Explorer 11 F12 utvecklingsverktyg med anpassade användaragentsträngen (detta är en iPhone 5 C användaragentsträngen).</span><span class="sxs-lookup"><span data-stu-id="08ae8-222">The following screenshot shows the *AllTags* view rendered in the emulator in Internet Explorer 11 F12 developer tools with the custom user agent string (this is an iPhone 5C user agent string).</span></span>

![][AllTagsIPhone_LayoutIPhone]

<span data-ttu-id="08ae8-223">I den mobila webbläsaren, Välj den **högtalare** länk.</span><span class="sxs-lookup"><span data-stu-id="08ae8-223">In the mobile browser, select the **Speakers** link.</span></span> <span data-ttu-id="08ae8-224">Eftersom det inte är en mobil vy (*AllSpeakers.Mobile.cshtml*), standard högtalarna visa (*AllSpeakers.cshtml*) återges med hjälp av vyn mobila layout (*\_Layout.Mobile.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="08ae8-224">Because there's not a mobile view (*AllSpeakers.Mobile.cshtml*), the default speakers view (*AllSpeakers.cshtml*) is rendered using the mobile layout view (*\_Layout.Mobile.cshtml*).</span></span> <span data-ttu-id="08ae8-225">Som visas under rubriken **MVC5 program (Mobile)** har definierats i  *\_Layout.Mobile.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="08ae8-225">As shown below, the title **MVC5 Application (Mobile)** is defined in *\_Layout.Mobile.cshtml*.</span></span>

![][AllSpeakers_LayoutMobile]

<span data-ttu-id="08ae8-226">Du kan inaktivera en standardvy (icke-mobila) från återgivning inuti en mobila layout globalt genom att ange `RequireConsistentDisplayMode` till `true` i den *vyer\\\_ViewStart.cshtml* filen, så här:</span><span class="sxs-lookup"><span data-stu-id="08ae8-226">You can globally disable a default (non-mobile) view from rendering inside a mobile layout by setting `RequireConsistentDisplayMode` to `true` in the *Views\\\_ViewStart.cshtml* file, like this:</span></span>

    @{
        Layout = "~/Views/Shared/_Layout.cshtml";
        DisplayModeProvider.Instance.RequireConsistentDisplayMode = true;
    }

<span data-ttu-id="08ae8-227">När `RequireConsistentDisplayMode` är inställd på `true`, mobila layouten (*\_Layout.Mobile.cshtml*) används endast för mobila vyer (d.v.s. när filen vyn är i formatet  ***vynamn**. Mobile.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="08ae8-227">When `RequireConsistentDisplayMode` is set to `true`, the mobile layout (*\_Layout.Mobile.cshtml*) is used only for mobile views (i.e. when the view file is of the form ***ViewName**.Mobile.cshtml*).</span></span> <span data-ttu-id="08ae8-228">Du kan vilja ange `RequireConsistentDisplayMode` till `true` om mobila layouten inte fungerar bra med dina icke-mobila vyer.</span><span class="sxs-lookup"><span data-stu-id="08ae8-228">You might want to set `RequireConsistentDisplayMode` to `true` if your mobile layout doesn't work well with your non-mobile views.</span></span> <span data-ttu-id="08ae8-229">Skärmbilden nedan visar hur *högtalare* sidan återger när `RequireConsistentDisplayMode` är inställd på `true` (utan strängen ”(mobilt)” i navigerings-fältet högst upp).</span><span class="sxs-lookup"><span data-stu-id="08ae8-229">The screenshot below shows how the *Speakers* page renders when `RequireConsistentDisplayMode` is set to `true` (without the string "(Mobile)" in the navigational bar at the top).</span></span>

![][AllSpeakers_LayoutMobileOverridden]

<span data-ttu-id="08ae8-230">Du kan inaktivera konsekvent visningsläge i en specifik vy genom att ange `RequireConsistentDisplayMode` till `false` i filen vyn.</span><span class="sxs-lookup"><span data-stu-id="08ae8-230">You can disable consistent display mode in a specific view by setting `RequireConsistentDisplayMode` to `false` in the view file.</span></span> <span data-ttu-id="08ae8-231">Följande markering i den *vyer\\Start\\AllSpeakers.cshtml* filen anger `RequireConsistentDisplayMode` till `false`:</span><span class="sxs-lookup"><span data-stu-id="08ae8-231">The following markup in the *Views\\Home\\AllSpeakers.cshtml* file sets `RequireConsistentDisplayMode` to `false`:</span></span>

    @model IEnumerable<string>

    @{
        ViewBag.Title = "All speakers";
        DisplayModeProvider.Instance.RequireConsistentDisplayMode = false;
    }

<span data-ttu-id="08ae8-232">Vi har sett hur du skapar mobila layouter och vyer och hur du skapar vyer för specifika enheter, till exempel iPhone och layouter i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="08ae8-232">In this section we've seen how to create mobile layouts and views and how to create layouts and views for specific devices such as the iPhone.</span></span>
<span data-ttu-id="08ae8-233">Den största fördelen Bootstrap CSS-ramverket är dock dynamisk layout, vilket innebär att en enda formatmall kan tillämpas på skrivbordet, phone och tablet webbläsare för att skapa ett konsekvent utseende.</span><span class="sxs-lookup"><span data-stu-id="08ae8-233">However, the main advantage of the Bootstrap CSS framework is the responsive layout, which means that a single stylesheet can be applied across desktop, phone, and tablet browsers to create a consistent look and feel.</span></span> <span data-ttu-id="08ae8-234">I nästa avsnitt visas hur man utnyttjar Bootstrap för att skapa mobilvänlig vyer.</span><span class="sxs-lookup"><span data-stu-id="08ae8-234">In the next section you'll see how to leverage Bootstrap to create mobile-friendly views.</span></span>

## <span data-ttu-id="08ae8-235"><a name="bkmk_Improvespeakerslist"></a>Förbättra listan högtalare</span><span class="sxs-lookup"><span data-stu-id="08ae8-235"><a name="bkmk_Improvespeakerslist"></a> Improve the Speakers List</span></span>
<span data-ttu-id="08ae8-236">Som du såg bara kan den *högtalare* läget läsbar men länkarna små och är svåra att trycka på en mobil enhet.</span><span class="sxs-lookup"><span data-stu-id="08ae8-236">As you just saw, the *Speakers* view is readable, but the links are small and are difficult to tap on a mobile device.</span></span> <span data-ttu-id="08ae8-237">I det här avsnittet ska du göra det *AllSpeakers* visa mobilvänlig, som visar stora, enkelt att tryck länkar och innehåller en kryssruta för att snabbt hitta högtalare.</span><span class="sxs-lookup"><span data-stu-id="08ae8-237">In this section, you'll make the *AllSpeakers* view mobile-friendly, which displays large, easy-to-tap links and contains a search box to quickly find speakers.</span></span>

<span data-ttu-id="08ae8-238">Du kan använda Bootstrap [länkad lista grupp] [ linked list group] formatmalls att förbättra den *högtalare* vyn.</span><span class="sxs-lookup"><span data-stu-id="08ae8-238">You can use the Bootstrap [linked list group][linked list group] styling to improve the *Speakers* view.</span></span> <span data-ttu-id="08ae8-239">I *vyer\\Start\\AllSpeakers.cshtml*, ersätta innehållet i filen Razor med koden nedan.</span><span class="sxs-lookup"><span data-stu-id="08ae8-239">In *Views\\Home\\AllSpeakers.cshtml*, replace the contents of the Razor file with the code below.</span></span>

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

<span data-ttu-id="08ae8-240">Den `class="list-group"` attribut i den `<div>` taggen gäller Bootstrap lista-stil och `class="input-group-item"` attributet gäller Bootstrap listan objektet formatmalls för varje länk.</span><span class="sxs-lookup"><span data-stu-id="08ae8-240">The `class="list-group"` attribute in the `<div>` tag applies the Bootstrap list styling, and the `class="input-group-item"` attribute applies Bootstrap list item styling to each link.</span></span>

<span data-ttu-id="08ae8-241">Uppdatera din mobila webbläsare.</span><span class="sxs-lookup"><span data-stu-id="08ae8-241">Refresh the mobile browser.</span></span> <span data-ttu-id="08ae8-242">Den uppdaterade vyn ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="08ae8-242">The updated view looks like this:</span></span>

![][AllSpeakersFixed]

<span data-ttu-id="08ae8-243">Bootstrap [länkad lista grupp] [ linked list group] formatmalls gör hela rutan för varje länk klickbara, vilket är en mycket bättre användarupplevelse.</span><span class="sxs-lookup"><span data-stu-id="08ae8-243">The Bootstrap [linked list group][linked list group] styling makes the entire box for each link clickable, which is a much better user experience.</span></span> <span data-ttu-id="08ae8-244">Växla till vyn skrivbord och se konsekvent utseende.</span><span class="sxs-lookup"><span data-stu-id="08ae8-244">Switch to the desktop view and observe the consistent look and feel.</span></span>

![][AllSpeakersFixedDesktop]

<span data-ttu-id="08ae8-245">Även om mobila webbläsare vyn har förbättrats, är det svårt att navigera lång lista med högtalare.</span><span class="sxs-lookup"><span data-stu-id="08ae8-245">Although the mobile browser view has improved, it's difficult to navigate the long list of speakers.</span></span> <span data-ttu-id="08ae8-246">Starttjänsten ger inte en sökning filter funktioner out-of-the-box, men du kan lägga till den med några få rader med kod.</span><span class="sxs-lookup"><span data-stu-id="08ae8-246">Bootstrap doesn't provide a search filter functionality out-of-the-box, but you can add it with a few lines of code.</span></span> <span data-ttu-id="08ae8-247">Du kommer först lägger till en kryssruta i vyn och sedan koppla samman med JavaScript-koden för filterfunktionen.</span><span class="sxs-lookup"><span data-stu-id="08ae8-247">You will first add a search box to the view, then hook up with the JavaScript code for the filter function.</span></span> <span data-ttu-id="08ae8-248">I *vyer\\Start\\AllSpeakers.cshtml*, lägga till en \<formuläret\> tagga precis efter den \<h2\> tagg, enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="08ae8-248">In *Views\\Home\\AllSpeakers.cshtml*, add a \<form\> tag just after the \<h2\> tag, as shown below:</span></span>

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

<span data-ttu-id="08ae8-249">Observera att den `<form>` och `<input>` taggar som båda har Bootstrap stilar används.</span><span class="sxs-lookup"><span data-stu-id="08ae8-249">Notice that the `<form>` and `<input>` tags both have the Bootstrap styles applied to them.</span></span> <span data-ttu-id="08ae8-250">Den `<span>` element lägger till en Bootstrap [glyphicon] [ glyphicon] till sökrutan.</span><span class="sxs-lookup"><span data-stu-id="08ae8-250">The `<span>` element adds a Bootstrap [glyphicon][glyphicon] to the search box.</span></span>

<span data-ttu-id="08ae8-251">I den *skript* mapp, lägga till en JavaScript-fil som heter *filter.js*.</span><span class="sxs-lookup"><span data-stu-id="08ae8-251">In the *Scripts* folder, add a JavaScript file called *filter.js*.</span></span> <span data-ttu-id="08ae8-252">Öppna filen och klistra in följande kod i den:</span><span class="sxs-lookup"><span data-stu-id="08ae8-252">Open the file and paste the following code into it:</span></span>

    $(function () {

        // reset the search form when the page loads
        $("form").each(function () {
            this.reset();
        });

        // wire up the events to the <input> element for search/filter
        $("input").bind("keyup change", function () {
            var searchtxt = this.value.toLowerCase();
            var items = $(".list-group-item");

            // show all speakers that begin with the typed text and hide others
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

<span data-ttu-id="08ae8-253">Du måste också innehålla filter.js i din registrerade paket.</span><span class="sxs-lookup"><span data-stu-id="08ae8-253">You also need to include filter.js in your registered bundles.</span></span> <span data-ttu-id="08ae8-254">Öppna *App\_starta\\BundleConfig.cs* och ändra första paket.</span><span class="sxs-lookup"><span data-stu-id="08ae8-254">Open *App\_Start\\BundleConfig.cs* and change the first bundles.</span></span> <span data-ttu-id="08ae8-255">Ändra först `bundles.Add` instruktionen (för den **jquery** paket) att inkludera *skript\\filter.js*, enligt följande:</span><span class="sxs-lookup"><span data-stu-id="08ae8-255">Change the first `bundles.Add` statement (for the **jquery** bundle) to include *Scripts\\filter.js*, as follows:</span></span>

     bundles.Add(new ScriptBundle("~/bundles/jquery").Include(
                "~/Scripts/jquery-{version}.js",
                "~/Scripts/filter.js"));

<span data-ttu-id="08ae8-256">Den **jquery** paket redan återges av standard  *\_Layout* vyn.</span><span class="sxs-lookup"><span data-stu-id="08ae8-256">The **jquery** bundle is already rendered by the default *\_Layout* view.</span></span> <span data-ttu-id="08ae8-257">Senare kan använda du samma JavaScript-kod om du vill använda filter-funktioner till andra vyer.</span><span class="sxs-lookup"><span data-stu-id="08ae8-257">Later, you can utilize the same JavaScript code to apply the filter functionality to other list views.</span></span>

<span data-ttu-id="08ae8-258">Uppdatera mobila webbläsare och gå till den *AllSpeakers* vyn.</span><span class="sxs-lookup"><span data-stu-id="08ae8-258">Refresh the mobile browser and go to the *AllSpeakers* view.</span></span> <span data-ttu-id="08ae8-259">Skriv ”sc” i sökrutan.</span><span class="sxs-lookup"><span data-stu-id="08ae8-259">In the search box, type "sc".</span></span> <span data-ttu-id="08ae8-260">Listan över högtalare bör nu filtreras enligt en söksträng.</span><span class="sxs-lookup"><span data-stu-id="08ae8-260">The speakers list should now be filtered according to your search string.</span></span>

![][AllSpeakersFixedSearchBySC]

## <span data-ttu-id="08ae8-261"><a name="bkmk_improvetags"></a>Förbättra listan taggar</span><span class="sxs-lookup"><span data-stu-id="08ae8-261"><a name="bkmk_improvetags"></a> Improve the Tags List</span></span>
<span data-ttu-id="08ae8-262">Som det *högtalare* vyn den *taggar* läget läsbar men länkarna är små och svåra att trycka på en mobil enhet.</span><span class="sxs-lookup"><span data-stu-id="08ae8-262">Like the *Speakers* view, the *Tags* view is readable, but the links are small and difficult to tap on a mobile device.</span></span> <span data-ttu-id="08ae8-263">Du kan åtgärda den *taggar* visa på samma sätt som du har åtgärdat den *högtalare* visas om du använder kodändringar beskrivs tidigare, men med följande `Html.ActionLink` metodsyntax i *vyer\\Start\\AllTags.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="08ae8-263">You can fix the *Tags* view the same way you fix the *Speakers* view, if you use the code changes described earlier, but with the following `Html.ActionLink` method syntax in *Views\\Home\\AllTags.cshtml*:</span></span>

    @Html.ActionLink(tag, 
                     "SessionsByTag", 
                     new { tag }, 
                     new { @class = "list-group-item" })

<span data-ttu-id="08ae8-264">Uppdatera skrivbord webbläsaren ser ut som följer:</span><span class="sxs-lookup"><span data-stu-id="08ae8-264">The refreshed desktop browser looks as follows:</span></span>

![][AllTagsFixedDesktop]

<span data-ttu-id="08ae8-265">Och uppdateras mobila webbläsare ser ut som följer:</span><span class="sxs-lookup"><span data-stu-id="08ae8-265">And the refreshed mobile browser looks as follows:</span></span> 

![][AllTagsFixed]

> [!NOTE]
> <span data-ttu-id="08ae8-266">Om du upptäcker att den ursprungliga listformateringen finns kvar i din mobila webbläsare och undrar vad hände med din nice Bootstrap formatmalls är detta ett resultat av den tidigare åtgärden Skapa mobila vissa vyer.</span><span class="sxs-lookup"><span data-stu-id="08ae8-266">If you notice that the original list formatting is still there in the mobile browser and wonder what happened to your nice Bootstrap styling, this is an artifact of your earlier action to create mobile specific views.</span></span> <span data-ttu-id="08ae8-267">Nu när du använder Bootstrap CSS-ramverket för att skapa en responsiv design, gå head och ta bort dessa mobile-specifika vyer och mobile-specifika layout vyer.</span><span class="sxs-lookup"><span data-stu-id="08ae8-267">However, now that you are using the Bootstrap CSS framework to create a responsive web design, go head and remove these mobile-specific views and the mobile-specific layout views.</span></span> <span data-ttu-id="08ae8-268">När du har gjort det, visas uppdateras mobila webbläsare Bootstrap stil.</span><span class="sxs-lookup"><span data-stu-id="08ae8-268">Once you have done so, the refreshed mobile browser will show the Bootstrap styling.</span></span>
> 
> 

## <span data-ttu-id="08ae8-269"><a name="bkmk_improvedates"></a>Förbättra listan datum</span><span class="sxs-lookup"><span data-stu-id="08ae8-269"><a name="bkmk_improvedates"></a> Improve the Dates List</span></span>
<span data-ttu-id="08ae8-270">Du kan förbättra den *datum* Visa som du har förbättrats i *högtalare* och *taggar* vyer om du använder kodändringar beskrivs tidigare, men med följande `Html.ActionLink` metodsyntax i *vyer\\Start\\AllDates.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="08ae8-270">You can improve the *Dates* view like you improved the *Speakers* and *Tags* views if you use the code changes described earlier, but with the following `Html.ActionLink` method syntax in *Views\\Home\\AllDates.cshtml*:</span></span>

    @Html.ActionLink(date.ToString("ddd, MMM dd, h:mm tt"), 
                     "SessionsByDate", 
                     new { date }, 
                     new { @class = "list-group-item" })

<span data-ttu-id="08ae8-271">Du får en Webbläsarvy uppdateras mobila så här:</span><span class="sxs-lookup"><span data-stu-id="08ae8-271">You will get a refreshed mobile browser view like this:</span></span>

![][AllDatesFixed]

<span data-ttu-id="08ae8-272">Du kan förbättra ytterligare den *datum* vyn genom att ordna datum / tid-värden efter datum.</span><span class="sxs-lookup"><span data-stu-id="08ae8-272">You can further improve the *Dates* view by organizing the date-time values by date.</span></span> <span data-ttu-id="08ae8-273">Detta kan göras med Bootstrap [paneler] [ panels] formatering.</span><span class="sxs-lookup"><span data-stu-id="08ae8-273">This can be done with the Bootstrap [panels][panels] styling.</span></span> <span data-ttu-id="08ae8-274">Ersätt innehållet i den *vyer\\Start\\AllDates.cshtml* med följande kod:</span><span class="sxs-lookup"><span data-stu-id="08ae8-274">Replace the contents of the *Views\\Home\\AllDates.cshtml* file with the following code:</span></span>

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

<span data-ttu-id="08ae8-275">Den här koden skapar en separat `<div class="panel panel-primary">` tagg för varje distinkta datum i listan och använder den [länkad lista grupp] [ linked list group] för respektive länkar som tidigare.</span><span class="sxs-lookup"><span data-stu-id="08ae8-275">This code creates a separate `<div class="panel panel-primary">` tag for each distinct date in the list, and uses the [linked list group][linked list group] for the respective links as before.</span></span> <span data-ttu-id="08ae8-276">Här är hur mobila webbläsare ser ut när den här koden körs:</span><span class="sxs-lookup"><span data-stu-id="08ae8-276">Here's what the mobile browser looks like when this code runs:</span></span>

![][AllDatesFixed2]

<span data-ttu-id="08ae8-277">Växla till skrivbord webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="08ae8-277">Switch to the desktop browser.</span></span> <span data-ttu-id="08ae8-278">Tänk återigen enhetligt utseende.</span><span class="sxs-lookup"><span data-stu-id="08ae8-278">Again, note the consistent look.</span></span>

![][AllDatesFixed2Desktop]

## <span data-ttu-id="08ae8-279"><a name="bkmk_improvesessionstable"></a>Förbättra SessionsTable vyn</span><span class="sxs-lookup"><span data-stu-id="08ae8-279"><a name="bkmk_improvesessionstable"></a> Improve the SessionsTable View</span></span>
<span data-ttu-id="08ae8-280">I det här avsnittet ska du göra det *SessionsTable* visa mer mobilvänlig.</span><span class="sxs-lookup"><span data-stu-id="08ae8-280">In this section, you'll make the *SessionsTable* view more mobile-friendly.</span></span> <span data-ttu-id="08ae8-281">Den här ändringen är mer omfattande tidigare ändringar.</span><span class="sxs-lookup"><span data-stu-id="08ae8-281">This change is more extensive the previous changes.</span></span>

<span data-ttu-id="08ae8-282">I den mobila webbläsaren trycker du på den **taggen** knappen och klicka sedan på Ange `asp` i sökrutan.</span><span class="sxs-lookup"><span data-stu-id="08ae8-282">In the mobile browser, tap the **Tag** button, then enter `asp` in the search box.</span></span>

![][AllTagsFixedSearchByASP]

<span data-ttu-id="08ae8-283">Tryck på den **ASP.NET** länk.</span><span class="sxs-lookup"><span data-stu-id="08ae8-283">Tap the **ASP.NET** link.</span></span>

![][SessionsTableTagASP.NET]

<span data-ttu-id="08ae8-284">Som du ser formateras visningen som en tabell som för närvarande används för att visas i webbläsaren skrivbord.</span><span class="sxs-lookup"><span data-stu-id="08ae8-284">As you can see, the display is formatted as a table, which is currently designed to be viewed in the desktop browser.</span></span> <span data-ttu-id="08ae8-285">Det är dock lite svårt att läsa mobila webbläsare.</span><span class="sxs-lookup"><span data-stu-id="08ae8-285">However, it's a little bit difficult to read on a mobile browser.</span></span> <span data-ttu-id="08ae8-286">Lös problemet genom att öppna *vyer\\Start\\SessionsTable.cshtml* och Ersätt innehållet i filen med följande kod:</span><span class="sxs-lookup"><span data-stu-id="08ae8-286">To fix this, open *Views\\Home\\SessionsTable.cshtml* and then replace the contents of the file with the following code:</span></span>

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

<span data-ttu-id="08ae8-287">Koden gör 3 saker:</span><span class="sxs-lookup"><span data-stu-id="08ae8-287">The code does 3 things:</span></span>

* <span data-ttu-id="08ae8-288">använder Bootstrap [anpassad länkade lista grupp] [ custom linked list group] att formatera sessionsinformation lodrätt, så att den här informationen kan läsas på en mobila webbläsare (med klasser, till exempel listan-grupp-objekt-text)</span><span class="sxs-lookup"><span data-stu-id="08ae8-288">uses the Bootstrap [custom linked list group][custom linked list group] to format the session information vertically, so that all this information is readable on a mobile browser (using classes such as list-group-item-text)</span></span>
* <span data-ttu-id="08ae8-289">gäller de [nätet] [ grid system] till layout, så att sessionen objekt flöda vågrätt i webbläsaren skrivbord och lodrätt i din mobila webbläsare (med klassen kolumn-md-4)</span><span class="sxs-lookup"><span data-stu-id="08ae8-289">applies the [grid system][grid system] to the layout, so that the session items flow horizontally in the desktop browser and vertically in the mobile browser (using the col-md-4 class)</span></span>
* <span data-ttu-id="08ae8-290">använder den [dynamisk verktygen] [ responsive utilities] Dölj taggarna sessionen när den visas i din mobila webbläsare (med klassen dolda xs)</span><span class="sxs-lookup"><span data-stu-id="08ae8-290">uses the [responsive utilities][responsive utilities] to hide the session tags when viewed in the mobile browser (using the hidden-xs class)</span></span>

<span data-ttu-id="08ae8-291">Du kan också trycka på en avdelning länken till respektive sessionen.</span><span class="sxs-lookup"><span data-stu-id="08ae8-291">You can also tap a title link to go to the respective session.</span></span> <span data-ttu-id="08ae8-292">Bilden nedan visar kodändringarna.</span><span class="sxs-lookup"><span data-stu-id="08ae8-292">The image below reflects the code changes.</span></span>

![][FixedSessionsByTag]

<span data-ttu-id="08ae8-293">Bootstrap nätet som du använde automatiskt ordnar sessioner lodrätt i din mobila webbläsare.</span><span class="sxs-lookup"><span data-stu-id="08ae8-293">The Bootstrap grid system that you applied automatically arranges the sessions vertically in the mobile browser.</span></span> <span data-ttu-id="08ae8-294">Observera också att taggar inte visas.</span><span class="sxs-lookup"><span data-stu-id="08ae8-294">Also, notice that the tags are not shown.</span></span> <span data-ttu-id="08ae8-295">Växla till skrivbord webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="08ae8-295">Switch to the desktop browser.</span></span>

![][SessionsTableFixedTagASP.NETDesktop]

<span data-ttu-id="08ae8-296">Observera att taggar visas nu i webbläsaren skrivbord.</span><span class="sxs-lookup"><span data-stu-id="08ae8-296">In the desktop browser, notice that the tags are now displayed.</span></span> <span data-ttu-id="08ae8-297">Du kan också se att du använt Bootstrap nätet ordnar session objekten i två kolumner.</span><span class="sxs-lookup"><span data-stu-id="08ae8-297">Also, you can see that the Bootstrap grid system you applied arranges the session items in two columns.</span></span> <span data-ttu-id="08ae8-298">Om du förstorar webbläsaren visas som hur ändringar i tre kolumner.</span><span class="sxs-lookup"><span data-stu-id="08ae8-298">If you enlarge the browser, you will see that the arrangement changes to three columns.</span></span>

## <span data-ttu-id="08ae8-299"><a name="bkmk_improvesessionbycode"></a>Förbättra SessionByCode vyn</span><span class="sxs-lookup"><span data-stu-id="08ae8-299"><a name="bkmk_improvesessionbycode"></a> Improve the SessionByCode View</span></span>
<span data-ttu-id="08ae8-300">Slutligen kan du korrigera den *SessionByCode* vy så att den mobilvänlig.</span><span class="sxs-lookup"><span data-stu-id="08ae8-300">Finally, you'll fix the *SessionByCode* view to make it mobile-friendly.</span></span>

<span data-ttu-id="08ae8-301">I den mobila webbläsaren trycker du på den **taggen** knappen och klicka sedan på Ange `asp` i sökrutan.</span><span class="sxs-lookup"><span data-stu-id="08ae8-301">In the mobile browser, tap the **Tag** button, then enter `asp` in the search box.</span></span>

![][AllTagsFixedSearchByASP]

<span data-ttu-id="08ae8-302">Tryck på den **ASP.NET** länk.</span><span class="sxs-lookup"><span data-stu-id="08ae8-302">Tap the **ASP.NET** link.</span></span> <span data-ttu-id="08ae8-303">Sessioner för ASP.NET-taggen visas.</span><span class="sxs-lookup"><span data-stu-id="08ae8-303">Sessions for the ASP.NET tag are displayed.</span></span>

![][FixedSessionsByTag]

<span data-ttu-id="08ae8-304">Välj den **skapa ett program med ASP.NET och AngularJS sidan** länk.</span><span class="sxs-lookup"><span data-stu-id="08ae8-304">Choose the **Building a Single Page Application with ASP.NET and AngularJS** link.</span></span>

![][SessionByCode3-644]

<span data-ttu-id="08ae8-305">Standardvyn för fjärrskrivbord är bra, men du kan förbättra utseendet enkelt vissa Bootstrap GUI-komponenter.</span><span class="sxs-lookup"><span data-stu-id="08ae8-305">The default desktop view is fine, but you can improve the look easily by using some Bootstrap GUI components.</span></span>

<span data-ttu-id="08ae8-306">Öppna *vyer\\Start\\SessionByCode.cshtml* och Ersätt innehållet med följande kod:</span><span class="sxs-lookup"><span data-stu-id="08ae8-306">Open *Views\\Home\\SessionByCode.cshtml* and replace the contents with the following markup:</span></span>

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

<span data-ttu-id="08ae8-307">Nya koden använder Bootstrap paneler formatering för att förbättra mobila vyn.</span><span class="sxs-lookup"><span data-stu-id="08ae8-307">The new markup uses Bootstrap panels styling to improve the mobile view.</span></span> 

<span data-ttu-id="08ae8-308">Uppdatera din mobila webbläsare.</span><span class="sxs-lookup"><span data-stu-id="08ae8-308">Refresh the mobile browser.</span></span> <span data-ttu-id="08ae8-309">Följande bild visar kodändringar som du skapat:</span><span class="sxs-lookup"><span data-stu-id="08ae8-309">The following image reflects the code changes that you just made:</span></span>

![][SessionByCodeFixed3-644]

## <a name="wrap-up-and-review"></a><span data-ttu-id="08ae8-310">Sammanfattning och granska</span><span class="sxs-lookup"><span data-stu-id="08ae8-310">Wrap Up and Review</span></span>
<span data-ttu-id="08ae8-311">Den här självstudiekursen har visas hur du använder ASP.NET MVC 5 för att utveckla mobilvänlig webbprogram.</span><span class="sxs-lookup"><span data-stu-id="08ae8-311">This tutorial has shown you how to use ASP.NET MVC 5 to develop mobile-friendly Web applications.</span></span> <span data-ttu-id="08ae8-312">Exempel på dessa är:</span><span class="sxs-lookup"><span data-stu-id="08ae8-312">These include:</span></span>

* <span data-ttu-id="08ae8-313">Distribuera ett ASP.NET MVC 5-program till en Apptjänst-webbapp</span><span class="sxs-lookup"><span data-stu-id="08ae8-313">Deploy an ASP.NET MVC 5 application to an App Service web app</span></span>
* <span data-ttu-id="08ae8-314">Använd Bootstrap för att skapa responsiv web-layouten i MVC 5-program</span><span class="sxs-lookup"><span data-stu-id="08ae8-314">Use Bootstrap to create responsive web layout in your MVC 5 application</span></span>
* <span data-ttu-id="08ae8-315">Åsidosätt layout, vyer och partiella vyer, både globalt och för en enskild vy</span><span class="sxs-lookup"><span data-stu-id="08ae8-315">Override layout, views, and partial views, both globally and for an individual view</span></span>
* <span data-ttu-id="08ae8-316">Kontrollens layout och partiella åsidosätter tvingande med hjälp av den `RequireConsistentDisplayMode` egenskapen</span><span class="sxs-lookup"><span data-stu-id="08ae8-316">Control layout and partial override enforcement using the `RequireConsistentDisplayMode` property</span></span>
* <span data-ttu-id="08ae8-317">Skapa vyer som är avsedda för vissa webbläsare, till exempel iPhone-webbläsare</span><span class="sxs-lookup"><span data-stu-id="08ae8-317">Create views that target specific browsers, such as the iPhone browser</span></span>
* <span data-ttu-id="08ae8-318">Tillämpa Bootstrap formatmalls i Razor-kod</span><span class="sxs-lookup"><span data-stu-id="08ae8-318">Apply Bootstrap styling in Razor code</span></span>

## <a name="see-also"></a><span data-ttu-id="08ae8-319">Se även</span><span class="sxs-lookup"><span data-stu-id="08ae8-319">See Also</span></span>
* [<span data-ttu-id="08ae8-320">9 grundläggande principerna för dynamisk webbdesign</span><span class="sxs-lookup"><span data-stu-id="08ae8-320">9 basic principles of responsive web design</span></span>](http://blog.froont.com/9-basic-principles-of-responsive-web-design/)
* <span data-ttu-id="08ae8-321">[Starttjänsten][BootstrapSite]</span><span class="sxs-lookup"><span data-stu-id="08ae8-321">[Bootstrap][BootstrapSite]</span></span>
* <span data-ttu-id="08ae8-322">[Officiell Bootstrap blogg][Official Bootstrap Blog]</span><span class="sxs-lookup"><span data-stu-id="08ae8-322">[Official Bootstrap Blog][Official Bootstrap Blog]</span></span>
* <span data-ttu-id="08ae8-323">[Twitter Bootstrap kursen från kursen Republiken][Twitter Bootstrap Tutorial from Tutorial Republic]</span><span class="sxs-lookup"><span data-stu-id="08ae8-323">[Twitter Bootstrap Tutorial from Tutorial Republic][Twitter Bootstrap Tutorial from Tutorial Republic]</span></span>
* <span data-ttu-id="08ae8-324">[Bootstrap Playground][The Bootstrap Playground]</span><span class="sxs-lookup"><span data-stu-id="08ae8-324">[The Bootstrap Playground][The Bootstrap Playground]</span></span>
* <span data-ttu-id="08ae8-325">[Metodtips för W3C rekommendation mobilen program][W3C Recommendation Mobile Web Application Best Practices]</span><span class="sxs-lookup"><span data-stu-id="08ae8-325">[W3C Recommendation Mobile Web Application Best Practices][W3C Recommendation Mobile Web Application Best Practices]</span></span>
* <span data-ttu-id="08ae8-326">[W3C kandidat rekommendation för media frågor][W3C Candidate Recommendation for media queries]</span><span class="sxs-lookup"><span data-stu-id="08ae8-326">[W3C Candidate Recommendation for media queries][W3C Candidate Recommendation for media queries]</span></span>

## <a name="whats-changed"></a><span data-ttu-id="08ae8-327">Nyheter</span><span class="sxs-lookup"><span data-stu-id="08ae8-327">What's changed</span></span>
* <span data-ttu-id="08ae8-328">En guide till övergången från Webbplatser till App Service finns i: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="08ae8-328">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

<!-- Internal Links -->
[Deploy the starter project to an Azure web app]: #bkmk_DeployStarterProject
[Bootstrap CSS Framework]: #bkmk_bootstrap
[Override the Views, Layouts, and Partial Views]: #bkmk_overrideviews
[Create Browser-Specific Views]:#bkmk_browserviews
[Improve the Speakers List]: #bkmk_Improvespeakerslist
[Improve the Tags List]: #bkmk_improvetags
[Improve the Dates List]: #bkmk_improvedates
[Improve the SessionsTable View]: #bkmk_improvesessionstable
[Improve the SessionByCode View]: #bkmk_improvesessionbycode

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
[The Bootstrap Playground]: http://www.bootply.com/
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

