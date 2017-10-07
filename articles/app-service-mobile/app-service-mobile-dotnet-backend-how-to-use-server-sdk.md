---
title: "aaaHow toowork med serverdelen för hello .NET SDK för Mobile Apps | Microsoft Docs"
description: "Lär dig hur toowork med hello serverdelen för .NET SDK för Azure Apptjänst Mobilappar."
keywords: "App service, azure app service, mobilapp, mobiltjänsten, skala, skalbara och app-distribution, azure app-distribution"
services: app-service\mobile
documentationcenter: 
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 0620554f-9590-40a8-9f47-61c48c21076b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 2946c5ba4424565ac764e2ce5597bf42362fcedf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="work-with-hello-net-backend-server-sdk-for-azure-mobile-apps"></a><span data-ttu-id="39299-104">Arbeta med serverdelen för hello .NET SDK för Azure Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="39299-104">Work with hello .NET backend server SDK for Azure Mobile Apps</span></span>
[!INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

<span data-ttu-id="39299-105">Det här avsnittet visar hur toouse hello serverdelen för .NET SDK i Azure Apptjänst Mobilappar scenarier.</span><span class="sxs-lookup"><span data-stu-id="39299-105">This topic shows you how toouse hello .NET backend server SDK in key Azure App Service Mobile Apps scenarios.</span></span> <span data-ttu-id="39299-106">hello Azure Mobile Apps-SDK hjälper dig att arbeta med mobila klienter från ASP.NET-programmet.</span><span class="sxs-lookup"><span data-stu-id="39299-106">hello Azure Mobile Apps SDK helps you work with mobile clients from your ASP.NET application.</span></span>

> [!TIP]
> <span data-ttu-id="39299-107">Hej [server .NET SDK för Azure Mobile Apps] [ 2] är öppen källkod på GitHub.</span><span class="sxs-lookup"><span data-stu-id="39299-107">hello [.NET server SDK for Azure Mobile Apps][2] is open source on GitHub.</span></span> <span data-ttu-id="39299-108">hello-databasen innehåller all källkod inklusive hello hela servern SDK enhet test suite och vissa exempelprojekt.</span><span class="sxs-lookup"><span data-stu-id="39299-108">hello repository contains all source code including hello entire server SDK unit test suite and some sample projects.</span></span>
>
>

## <a name="reference-documentation"></a><span data-ttu-id="39299-109">Referensdokumentation</span><span class="sxs-lookup"><span data-stu-id="39299-109">Reference documentation</span></span>
<span data-ttu-id="39299-110">hello-referensdokumentationen för hello server SDK finns här: [.NET-referens för Azure Mobile Apps][1].</span><span class="sxs-lookup"><span data-stu-id="39299-110">hello reference documentation for hello server SDK is located here: [Azure Mobile Apps .NET Reference][1].</span></span>

## <span data-ttu-id="39299-111"><a name="create-app"></a>Så här: skapa en .NET-mobilappsserverdel</span><span class="sxs-lookup"><span data-stu-id="39299-111"><a name="create-app"></a>How to: Create a .NET Mobile App backend</span></span>
<span data-ttu-id="39299-112">Om du startar ett nytt projekt, kan du skapa en Apptjänst-program med hjälp av antingen hello [Azure-portalen] eller Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="39299-112">If you are starting a new project, you can create an App Service application using either hello [Azure portal] or Visual Studio.</span></span> <span data-ttu-id="39299-113">Du kan köra hello Apptjänst program lokalt eller publicera hello projektet tooyour molnbaserade Apptjänst mobilappar.</span><span class="sxs-lookup"><span data-stu-id="39299-113">You can run hello App Service application locally or publish hello project tooyour cloud-based App Service mobile app.</span></span>

<span data-ttu-id="39299-114">Om du lägger till mobila funktioner tooan befintligt projekt, se hello [ladda ned och initiera hello SDK](#install-sdk) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="39299-114">If you are adding mobile capabilities tooan existing project, see hello [Download and initialize hello SDK](#install-sdk) section.</span></span>

### <a name="create-a-net-backend-using-hello-azure-portal"></a><span data-ttu-id="39299-115">Skapa en .NET-serverdel med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="39299-115">Create a .NET backend using hello Azure portal</span></span>
<span data-ttu-id="39299-116">toocreate en Apptjänst mobilserverdel antingen följa hello [Snabbstartsguide] [ 3] eller följa dessa steg:</span><span class="sxs-lookup"><span data-stu-id="39299-116">toocreate an App Service mobile backend, either follow hello [Quickstart tutorial][3] or follow these steps:</span></span>

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

<span data-ttu-id="39299-117">Tillbaka i hello *Kom igång* bladet under **skapa en tabell API**, Välj **C#** som din **språk för serverdel**.</span><span class="sxs-lookup"><span data-stu-id="39299-117">Back in hello *Get started* blade, under **Create a table API**, choose **C#** as your **Backend language**.</span></span> <span data-ttu-id="39299-118">Klicka på **hämta**extrahera den komprimerade filen filer tooyour lokala datorn och öppna hello lösningen i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="39299-118">Click **Download**, extract the compressed project files tooyour local computer, and open hello solution in Visual Studio.</span></span>

### <a name="create-a-net-backend-using-visual-studio-2013-and-visual-studio-2015"></a><span data-ttu-id="39299-119">Skapa en .NET-serverdel med hjälp av Visual Studio 2013 och Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="39299-119">Create a .NET backend using Visual Studio 2013 and Visual Studio 2015</span></span>
<span data-ttu-id="39299-120">Installera hello [Azure SDK för .NET] [ 4] (version 2.9.0 eller senare) toocreate ett Azure Mobile Apps-projekt i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="39299-120">Install hello [Azure SDK for .NET][4] (version 2.9.0 or later) toocreate an Azure Mobile Apps project in Visual Studio.</span></span> <span data-ttu-id="39299-121">När du har installerat hello SDK, skapa ett ASP.NET-program med hjälp av hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="39299-121">Once you have installed hello SDK, create an ASP.NET application using hello following steps:</span></span>

1. <span data-ttu-id="39299-122">Öppna hello **nytt projekt** dialogrutan (från *filen* > **ny** > **projekt...** ).</span><span class="sxs-lookup"><span data-stu-id="39299-122">Open hello **New Project** dialog (from *File* > **New** > **Project...**).</span></span>
2. <span data-ttu-id="39299-123">Expandera **mallar** > **Visual C#**, och välj **Web**.</span><span class="sxs-lookup"><span data-stu-id="39299-123">Expand **Templates** > **Visual C#**, and select **Web**.</span></span>
3. <span data-ttu-id="39299-124">Välj **ASP.NET-webbprogram**.</span><span class="sxs-lookup"><span data-stu-id="39299-124">Select **ASP.NET Web Application**.</span></span>
4. <span data-ttu-id="39299-125">Fyll i hello projektnamn.</span><span class="sxs-lookup"><span data-stu-id="39299-125">Fill in hello project name.</span></span> <span data-ttu-id="39299-126">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="39299-126">Then click **OK**.</span></span>
5. <span data-ttu-id="39299-127">Under *ASP.NET 4.5.2-mallen mallar*väljer **Azure Mobile App**.</span><span class="sxs-lookup"><span data-stu-id="39299-127">Under *ASP.NET 4.5.2 Templates*, select **Azure Mobile App**.</span></span> <span data-ttu-id="39299-128">Kontrollera **värd i molnet hello** toocreate mobilserverdel i hello molnet toowhich som du kan publicera det här projektet.</span><span class="sxs-lookup"><span data-stu-id="39299-128">Check **Host in hello cloud** toocreate a mobile backend in hello cloud toowhich you can publish this project.</span></span>
6. <span data-ttu-id="39299-129">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="39299-129">Click **OK**.</span></span>

## <span data-ttu-id="39299-130"><a name="install-sdk"></a>Så här: hämta och initiera hello SDK</span><span class="sxs-lookup"><span data-stu-id="39299-130"><a name="install-sdk"></a>How to: Download and initialize hello SDK</span></span>
<span data-ttu-id="39299-131">hello SDK är tillgängligt på [NuGet.org]. Det här paketet innehåller hello grundläggande funktion som krävs för tooget igång med hello SDK.</span><span class="sxs-lookup"><span data-stu-id="39299-131">hello SDK is available on [NuGet.org]. This package includes hello base functionality required tooget started using hello SDK.</span></span> <span data-ttu-id="39299-132">tooinitialize hello SDK måste du tooperform åtgärder på hello **HttpConfiguration** objekt.</span><span class="sxs-lookup"><span data-stu-id="39299-132">tooinitialize hello SDK, you need tooperform actions on hello **HttpConfiguration** object.</span></span>

### <a name="install-hello-sdk"></a><span data-ttu-id="39299-133">Installera hello SDK</span><span class="sxs-lookup"><span data-stu-id="39299-133">Install hello SDK</span></span>
<span data-ttu-id="39299-134">tooinstall hello SDK, högerklicka på hello server-projekt i Visual Studio, markera **hantera NuGet-paket**, Sök efter hello [Microsoft.Azure.Mobile.Server] paketet och klicka sedan på  **Installera**.</span><span class="sxs-lookup"><span data-stu-id="39299-134">tooinstall hello SDK, right-click on hello server project in Visual Studio, select **Manage NuGet Packages**, search for hello [Microsoft.Azure.Mobile.Server] package, then click **Install**.</span></span>

### <span data-ttu-id="39299-135"><a name="server-project-setup"></a>Initiera hello serverprojekt</span><span class="sxs-lookup"><span data-stu-id="39299-135"><a name="server-project-setup"></a> Initialize hello server project</span></span>
<span data-ttu-id="39299-136">Ett .NET-serverdel serverprojekt är initierad liknande tooother ASP.NET-projekt, genom att lägga till en OWIN-startklass.</span><span class="sxs-lookup"><span data-stu-id="39299-136">A .NET backend server project is initialized similar tooother ASP.NET projects, by including an OWIN startup class.</span></span> <span data-ttu-id="39299-137">Se till att du har refererade hello NuGet-paketet `Microsoft.Owin.Host.SystemWeb`.</span><span class="sxs-lookup"><span data-stu-id="39299-137">Ensure that you have referenced hello NuGet package `Microsoft.Owin.Host.SystemWeb`.</span></span> <span data-ttu-id="39299-138">tooadd den här klassen i Visual Studio högerklickar du på serverprojektet och välj **Lägg till** >
**nytt objekt**, sedan **Web**  >  ** Allmän** > **OWIN-startklass**.</span><span class="sxs-lookup"><span data-stu-id="39299-138">tooadd this class in Visual Studio, right-click on your server project and select **Add** >
**New Item**, then **Web** > **General** > **OWIN Startup class**.</span></span>  <span data-ttu-id="39299-139">En klass skapas med hello följande attribut:</span><span class="sxs-lookup"><span data-stu-id="39299-139">A class is generated with hello following attribute:</span></span>

    [assembly: OwinStartup(typeof(YourServiceName.YourStartupClassName))]

<span data-ttu-id="39299-140">I hello `Configuration()` metod för din OWIN-startklass, Använd en **HttpConfiguration** objekt tooconfigure hello Azure Mobile Apps miljö.</span><span class="sxs-lookup"><span data-stu-id="39299-140">In hello `Configuration()` method of your OWIN startup class, use an **HttpConfiguration** object tooconfigure hello Azure Mobile Apps environment.</span></span>
<span data-ttu-id="39299-141">följande exempel hello initierar hello serverprojekt med inga extra funktioner:</span><span class="sxs-lookup"><span data-stu-id="39299-141">hello following example initializes hello server project with no added features:</span></span>

    // in OWIN startup class
    public void Configuration(IAppBuilder app)
    {
        HttpConfiguration config = new HttpConfiguration();

        new MobileAppConfiguration()
            // no added features
            .ApplyTo(config);

        app.UseWebApi(config);
    }

<span data-ttu-id="39299-142">tooenable enskilda funktioner, måste du anropa tilläggsmetoder på hello **MobileAppConfiguration** objektet innan du anropar **gällafoderråvaran**.</span><span class="sxs-lookup"><span data-stu-id="39299-142">tooenable individual features, you must call extension methods on hello **MobileAppConfiguration** object before calling **ApplyTo**.</span></span> <span data-ttu-id="39299-143">Till exempel hello följande kod lägger till hello standard dirigerar tooall API-domänkontrollanter som har hello attribut `[MobileAppController]` under initieringen:</span><span class="sxs-lookup"><span data-stu-id="39299-143">For example, hello following code adds hello default routes tooall API controllers that have hello attribute `[MobileAppController]` during initialization:</span></span>

    new MobileAppConfiguration()
        .MapApiControllers()
        .ApplyTo(config);

<span data-ttu-id="39299-144">hello server quickstart från hello Azure portal anrop **UseDefaultConfiguration()**.</span><span class="sxs-lookup"><span data-stu-id="39299-144">hello server quickstart from hello Azure portal calls **UseDefaultConfiguration()**.</span></span> <span data-ttu-id="39299-145">Den här motsvarande toohello efter installationen:</span><span class="sxs-lookup"><span data-stu-id="39299-145">This equivalent toohello following setup:</span></span>

        new MobileAppConfiguration()
            .AddMobileAppHomeController()             // from hello Home package
            .MapApiControllers()
            .AddTables(                               // from hello Tables package
                new MobileAppTableConfiguration()
                    .MapTableControllers()
                    .AddEntityFramework()             // from hello Entity package
                )
            .AddPushNotifications()                   // from hello Notifications package
            .MapLegacyCrossDomainController()         // from hello CrossDomain package
            .ApplyTo(config);

<span data-ttu-id="39299-146">hello tillägget metoderna är:</span><span class="sxs-lookup"><span data-stu-id="39299-146">hello extension methods used are:</span></span>

* <span data-ttu-id="39299-147">`AddMobileAppHomeController()`ger hello standardstartsida för Azure Mobile Apps.</span><span class="sxs-lookup"><span data-stu-id="39299-147">`AddMobileAppHomeController()` provides hello default Azure Mobile Apps home page.</span></span>
* <span data-ttu-id="39299-148">`MapApiControllers()`ger anpassad API-funktioner för WebAPI domänkontrollanter dekorerad med hello `[MobileAppController]` attribut.</span><span class="sxs-lookup"><span data-stu-id="39299-148">`MapApiControllers()` provides custom API capabilities for WebAPI controllers decorated with hello `[MobileAppController]` attribute.</span></span>
* <span data-ttu-id="39299-149">`AddTables()`innehåller en mappning av hello `/tables` slutpunkter tootable domänkontrollanter.</span><span class="sxs-lookup"><span data-stu-id="39299-149">`AddTables()` provides a mapping of hello `/tables` endpoints tootable controllers.</span></span>
* <span data-ttu-id="39299-150">`AddTablesWithEntityFramework()`är en kort hand för mappning hello `/tables` slutpunkter som använder Entity Framework-baserade domänkontrollanter.</span><span class="sxs-lookup"><span data-stu-id="39299-150">`AddTablesWithEntityFramework()` is a short-hand for mapping hello `/tables` endpoints using Entity Framework based controllers.</span></span>
* <span data-ttu-id="39299-151">`AddPushNotifications()`ger en enkel metod för att registrera enheter för Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="39299-151">`AddPushNotifications()` provides a simple method of registering devices for Notification Hubs.</span></span>
* <span data-ttu-id="39299-152">`MapLegacyCrossDomainController()`innehåller standard CORS-huvuden för lokal utveckling.</span><span class="sxs-lookup"><span data-stu-id="39299-152">`MapLegacyCrossDomainController()` provides standard CORS headers for local development.</span></span>

### <a name="sdk-extensions"></a><span data-ttu-id="39299-153">SDK-tillägg</span><span class="sxs-lookup"><span data-stu-id="39299-153">SDK extensions</span></span>
<span data-ttu-id="39299-154">följande NuGet-baserade tilläggspaket hello har olika mobila funktioner som kan användas av ditt program.</span><span class="sxs-lookup"><span data-stu-id="39299-154">hello following NuGet-based extension packages provide various mobile features that can be used by your application.</span></span> <span data-ttu-id="39299-155">Du aktiverar tillägg under initiering med hjälp av hello **MobileAppConfiguration** objekt.</span><span class="sxs-lookup"><span data-stu-id="39299-155">You enable extensions during initialization by using hello **MobileAppConfiguration** object.</span></span>

* <span data-ttu-id="39299-156">[Microsoft.Azure.Mobile.Server.Quickstart] stöder hello grundläggande inställningar för Mobilappar.</span><span class="sxs-lookup"><span data-stu-id="39299-156">[Microsoft.Azure.Mobile.Server.Quickstart] Supports hello basic Mobile Apps setup.</span></span> <span data-ttu-id="39299-157">Tillagda toohello konfiguration genom att anropa hello **UseDefaultConfiguration** tilläggsmetoden under initieringen.</span><span class="sxs-lookup"><span data-stu-id="39299-157">Added toohello configuration by calling hello **UseDefaultConfiguration** extension method during initialization.</span></span> <span data-ttu-id="39299-158">Det här tillägget ingår följande tillägg: meddelanden, autentisering, enhet, tabeller, domäner och Home paket.</span><span class="sxs-lookup"><span data-stu-id="39299-158">This extension includes following extensions: Notifications, Authentication, Entity, Tables, Cross-domain, and Home packages.</span></span> <span data-ttu-id="39299-159">Det här paketet används av hello Mobile Apps Quickstart på hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="39299-159">This package is used by hello Mobile Apps Quickstart available on hello Azure portal.</span></span>
* <span data-ttu-id="39299-160">[Microsoft.Azure.Mobile.Server.Home](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Home/) implementerar hello standard *mobila appen är igång sidan* för hello webbplatsroten.</span><span class="sxs-lookup"><span data-stu-id="39299-160">[Microsoft.Azure.Mobile.Server.Home](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Home/) Implements hello default *this mobile app is up and running page* for hello web site root.</span></span> <span data-ttu-id="39299-161">Lägg till toohello konfiguration genom att anropa den **AddMobileAppHomeController** metod.</span><span class="sxs-lookup"><span data-stu-id="39299-161">Add toohello configuration by calling the **AddMobileAppHomeController** extension method.</span></span>
* <span data-ttu-id="39299-162">[Microsoft.Azure.Mobile.Server.Tables](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Tables/) innehåller klasser för att arbeta med data och anger upp hello data pipeline.</span><span class="sxs-lookup"><span data-stu-id="39299-162">[Microsoft.Azure.Mobile.Server.Tables](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Tables/) includes classes for working with data and sets-up hello data pipeline.</span></span> <span data-ttu-id="39299-163">Lägg till toohello konfiguration genom att anropa hello **AddTables** metod.</span><span class="sxs-lookup"><span data-stu-id="39299-163">Add toohello configuration by calling hello **AddTables** extension method.</span></span>
* <span data-ttu-id="39299-164">[Microsoft.Azure.Mobile.Server.Entity](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Entity/) aktiverar hello Entity Framework tooaccess data i hello SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="39299-164">[Microsoft.Azure.Mobile.Server.Entity](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Entity/) Enables hello Entity Framework tooaccess data in hello SQL Database.</span></span> <span data-ttu-id="39299-165">Lägg till toohello konfiguration genom att anropa hello **AddTablesWithEntityFramework** metod.</span><span class="sxs-lookup"><span data-stu-id="39299-165">Add toohello configuration by calling hello **AddTablesWithEntityFramework** extension method.</span></span>
* <span data-ttu-id="39299-166">[Microsoft.Azure.Mobile.Server.Authentication] aktiverar autentisering och anger upp hello OWIN mellanprogram används toovalidate token.</span><span class="sxs-lookup"><span data-stu-id="39299-166">[Microsoft.Azure.Mobile.Server.Authentication] Enables authentication and sets-up hello OWIN middleware used toovalidate tokens.</span></span> <span data-ttu-id="39299-167">Lägg till toohello konfiguration genom att anropa hello **AddAppServiceAuthentication** och **IAppBuilder**. **UseAppServiceAuthentication** tilläggsmetoder.</span><span class="sxs-lookup"><span data-stu-id="39299-167">Add toohello configuration by calling hello **AddAppServiceAuthentication** and **IAppBuilder**.**UseAppServiceAuthentication** extension methods.</span></span>
* <span data-ttu-id="39299-168">[Microsoft.Azure.Mobile.Server.Notifications] aktiverar push-meddelanden och definierar en slutpunkt för push-registrering.</span><span class="sxs-lookup"><span data-stu-id="39299-168">[Microsoft.Azure.Mobile.Server.Notifications] Enables push notifications and defines a push registration endpoint.</span></span> <span data-ttu-id="39299-169">Lägg till toohello konfiguration genom att anropa hello **AddPushNotifications** metod.</span><span class="sxs-lookup"><span data-stu-id="39299-169">Add toohello configuration by calling hello **AddPushNotifications** extension method.</span></span>
* <span data-ttu-id="39299-170">[Microsoft.Azure.Mobile.Server.CrossDomain](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.CrossDomain/) skapar en domänkontrollant som fungerar data toolegacy webbläsare från din Mobilapp.</span><span class="sxs-lookup"><span data-stu-id="39299-170">[Microsoft.Azure.Mobile.Server.CrossDomain](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.CrossDomain/) Creates a controller that serves data toolegacy web browsers from your Mobile App.</span></span> <span data-ttu-id="39299-171">Lägg till toohello konfiguration genom att anropa den **MapLegacyCrossDomainController** metod.</span><span class="sxs-lookup"><span data-stu-id="39299-171">Add toohello configuration by calling the **MapLegacyCrossDomainController** extension method.</span></span>
* <span data-ttu-id="39299-172">[Microsoft.Azure.Mobile.Server.Login] ger hello AppServiceLoginHandler.CreateToken() metod som är en statisk metod som används under scenarier för anpassad autentisering.</span><span class="sxs-lookup"><span data-stu-id="39299-172">[Microsoft.Azure.Mobile.Server.Login] Provides hello AppServiceLoginHandler.CreateToken() method, which is a static method used during custom authentication scenarios.</span></span>

## <span data-ttu-id="39299-173"><a name="publish-server-project"></a>Så här: publicera hello serverprojekt</span><span class="sxs-lookup"><span data-stu-id="39299-173"><a name="publish-server-project"></a>How to: Publish hello server project</span></span>
<span data-ttu-id="39299-174">Det här avsnittet visar hur toopublish .NET-serverdel projektet i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="39299-174">This section shows you how toopublish your .NET backend project from Visual Studio.</span></span> <span data-ttu-id="39299-175">Du kan också distribuera serverdelsprojektet med Git eller någon av hello andra metoder som beskrivs i hello [dokumentationen för Azure App Service](../app-service-web/web-sites-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="39299-175">You can also deploy your backend project using Git or any of hello other methods covered in hello [Azure App Service deployment documentation](../app-service-web/web-sites-deploy.md).</span></span>

1. <span data-ttu-id="39299-176">Återskapa hello projektet toorestore NuGet-paket i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="39299-176">In Visual Studio, rebuild hello project toorestore NuGet packages.</span></span>
2. <span data-ttu-id="39299-177">I Solution Explorer, högerklicka på hello projektet klickar du på **publicera**.</span><span class="sxs-lookup"><span data-stu-id="39299-177">In Solution Explorer, right-click hello project, click **Publish**.</span></span> <span data-ttu-id="39299-178">hello måste första gången du publicerar toodefine en publiceringsprofilen.</span><span class="sxs-lookup"><span data-stu-id="39299-178">hello first time you publish, you need toodefine a publishing profile.</span></span> <span data-ttu-id="39299-179">När du redan har en profil som har definierats, markerar du den och klicka på **publicera**.</span><span class="sxs-lookup"><span data-stu-id="39299-179">When you already have a profile defined, you can select it and click **Publish**.</span></span>
3. <span data-ttu-id="39299-180">Om du blir tillfrågad tooselect ett publiceringsmål, klickar du på **Microsoft Azure App Service** > **nästa**, och sedan (vid behov) logga in med dina autentiseringsuppgifter för Azure.</span><span class="sxs-lookup"><span data-stu-id="39299-180">If asked tooselect a publish target, click **Microsoft Azure App Service** > **Next**, then (if needed) sign-in with your Azure credentials.</span></span>
   <span data-ttu-id="39299-181">Visual Studio hämtar och lagrar på ett säkert sätt dina publiceringsinställningar direkt från Azure.</span><span class="sxs-lookup"><span data-stu-id="39299-181">Visual Studio downloads and securely stores your publish settings directly from Azure.</span></span>

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-1.png)
4. <span data-ttu-id="39299-182">Välj din **prenumeration**väljer **resurstypen** från **visa**, expandera **Mobilapp**, och serverdelen för Mobilappen klickar **OK**.</span><span class="sxs-lookup"><span data-stu-id="39299-182">Choose your **Subscription**, select **Resource Type** from **View**, expand **Mobile App**, and click your Mobile App backend, then click **OK**.</span></span>

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-2.png)
5. <span data-ttu-id="39299-183">Kontrollera hello publicera profilinformation och klickar på **publicera**.</span><span class="sxs-lookup"><span data-stu-id="39299-183">Verify hello publish profile information and click **Publish**.</span></span>

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-3.png)

    <span data-ttu-id="39299-184">När din mobilappsserverdel har publicerats visas en landningssida som anger lyckades.</span><span class="sxs-lookup"><span data-stu-id="39299-184">When your Mobile App backend has published successfully, you see a landing page indicating success.</span></span>

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-success.png)

## <span data-ttu-id="39299-185"><a name="define-table-controller"></a>Så här: definiera en tabell-styrenhet</span><span class="sxs-lookup"><span data-stu-id="39299-185"><a name="define-table-controller"></a> How to: Define a table controller</span></span>
<span data-ttu-id="39299-186">Definiera en tabell Controller tooexpose en SQL-tabell toomobile klienter.</span><span class="sxs-lookup"><span data-stu-id="39299-186">Define a Table Controller tooexpose a SQL table toomobile clients.</span></span>  <span data-ttu-id="39299-187">Konfigurera en tabell Controller kräver tre steg:</span><span class="sxs-lookup"><span data-stu-id="39299-187">Configuring a Table Controller requires three steps:</span></span>

1. <span data-ttu-id="39299-188">Skapa en klass för Data Transfer objekt (DTO).</span><span class="sxs-lookup"><span data-stu-id="39299-188">Create a Data Transfer Object (DTO) class.</span></span>
2. <span data-ttu-id="39299-189">Konfigurera en tabellreferens i hello Mobile DbContext-klassen.</span><span class="sxs-lookup"><span data-stu-id="39299-189">Configure a table reference in hello Mobile DbContext class.</span></span>
3. <span data-ttu-id="39299-190">Skapa en tabell-styrenhet.</span><span class="sxs-lookup"><span data-stu-id="39299-190">Create a table controller.</span></span>

<span data-ttu-id="39299-191">En Data Transfer objekt (DTO) är ett vanligt C#-objekt som ärver från `EntityData`.</span><span class="sxs-lookup"><span data-stu-id="39299-191">A Data Transfer Object (DTO) is a plain C# object that inherits from `EntityData`.</span></span>  <span data-ttu-id="39299-192">Exempel:</span><span class="sxs-lookup"><span data-stu-id="39299-192">For example:</span></span>

    public class TodoItem : EntityData
    {
        public string Text { get; set; }
        public bool Complete {get; set;}
    }

<span data-ttu-id="39299-193">Hej DTO är används toodefine hello tabell i hello SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="39299-193">hello DTO is used toodefine hello table within hello SQL database.</span></span>  <span data-ttu-id="39299-194">toocreate hello databasen post, lägga till en `DbSet<>` egenskapen hello DbContext som du använder.</span><span class="sxs-lookup"><span data-stu-id="39299-194">toocreate hello database entry, add a `DbSet<>` property to hello DbContext you are using.</span></span>  <span data-ttu-id="39299-195">I hello projektet standardmallen för Azure Mobile Apps hello DbContext kallas `Models\MobileServiceContext.cs`:</span><span class="sxs-lookup"><span data-stu-id="39299-195">In hello default project template for Azure Mobile Apps, hello DbContext is called `Models\MobileServiceContext.cs`:</span></span>

    public class MobileServiceContext : DbContext
    {
        private const string connectionStringName = "Name=MS_TableConnectionString";

        public MobileServiceContext() : base(connectionStringName)
        {

        }

        public DbSet<TodoItem> TodoItems { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            modelBuilder.Conventions.Add(
                new AttributeToColumnAnnotationConvention<TableColumnAttribute, string>(
                    "ServiceColumnTable", (property, attributes) => attributes.Single().ColumnType.ToString()));
        }
    }

<span data-ttu-id="39299-196">Om du har hello Azure SDK har installerats kan skapa du nu en mall för tabellen domänkontrollant på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="39299-196">If you have hello Azure SDK installed, you can now create a template table controller as follows:</span></span>

1. <span data-ttu-id="39299-197">Högerklicka på hello domänkontrollanter mapp och välj **Lägg till** > **domänkontrollant...** .</span><span class="sxs-lookup"><span data-stu-id="39299-197">Right-click on hello Controllers folder and select **Add** > **Controller...**.</span></span>
2. <span data-ttu-id="39299-198">Välj hello **Azure Mobile Apps tabell Controller** alternativ och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="39299-198">Select hello **Azure Mobile Apps Table Controller** option, then click **Add**.</span></span>
3. <span data-ttu-id="39299-199">I hello **Lägg till styrenhet** dialogrutan:</span><span class="sxs-lookup"><span data-stu-id="39299-199">In hello **Add Controller** dialog:</span></span>
   * <span data-ttu-id="39299-200">I hello **Modellklass** listrutan, Välj din nya DTO.</span><span class="sxs-lookup"><span data-stu-id="39299-200">In hello **Model class** dropdown, select your new DTO.</span></span>
   * <span data-ttu-id="39299-201">I hello **DbContext** listrutan, Välj hello mobila tjänsten DbContext-klassen.</span><span class="sxs-lookup"><span data-stu-id="39299-201">In hello **DbContext** dropdown, select hello Mobile Service DbContext class.</span></span>
   * <span data-ttu-id="39299-202">hello Kontrollnamn skapas åt dig.</span><span class="sxs-lookup"><span data-stu-id="39299-202">hello Controller name is created for you.</span></span>
4. <span data-ttu-id="39299-203">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="39299-203">Click **Add**.</span></span>

<span data-ttu-id="39299-204">hello server snabbstartsprojektet innehåller ett exempel på en enkel **TodoItemController**.</span><span class="sxs-lookup"><span data-stu-id="39299-204">hello quickstart server project contains an example for a simple **TodoItemController**.</span></span>

### <span data-ttu-id="39299-205"><a name="adjust-pagesize"></a>Så här: justera hello tabell växlingsfilens storlek</span><span class="sxs-lookup"><span data-stu-id="39299-205"><a name="adjust-pagesize"></a>How to: Adjust hello table paging size</span></span>
<span data-ttu-id="39299-206">Som standard returnerar Azure Mobile Apps 50 poster per begäran.</span><span class="sxs-lookup"><span data-stu-id="39299-206">By default, Azure Mobile Apps returns 50 records per request.</span></span>  <span data-ttu-id="39299-207">Sidindelning säkerställer att hello-klienten inte binder det sina UI-tråden eller hello Server under för lång tid att säkerställa en bra användarupplevelse.</span><span class="sxs-lookup"><span data-stu-id="39299-207">Paging ensures that hello client does not tie up their UI thread nor hello server for too long, ensuring a good user experience.</span></span> <span data-ttu-id="39299-208">toochange hello tabellens växlingsfilens storlek, öka hello serversidan ”tillåtna storleken för frågan” och hello klientsidan sidan storlek hello serversidan ”tillåtna storleken för frågan” justeras med hello `EnableQuery` attribut:</span><span class="sxs-lookup"><span data-stu-id="39299-208">toochange hello table paging size, increase hello server side "allowed query size" and hello client-side page size hello server side "allowed query size" is adjusted using hello `EnableQuery` attribute:</span></span>

    [EnableQuery(PageSize = 500)]

<span data-ttu-id="39299-209">Kontrollera hello PageSize hello samma eller större än hello storlek som har begärts av hello-klienten.</span><span class="sxs-lookup"><span data-stu-id="39299-209">Ensure hello PageSize is hello same or larger than hello size requested by hello client.</span></span>  <span data-ttu-id="39299-210">Finns toohello specifika klient ta dokumentation för information om hur du ändrar hello sidstorleken för klienten.</span><span class="sxs-lookup"><span data-stu-id="39299-210">Refer toohello specific client HOWTO documentation for details on changing hello client page size.</span></span>

## <a name="how-to-define-a-custom-api-controller"></a><span data-ttu-id="39299-211">Så här: definiera en anpassad API-styrenhet</span><span class="sxs-lookup"><span data-stu-id="39299-211">How to: Define a custom API controller</span></span>
<span data-ttu-id="39299-212">hello anpassade API-kontrollanten ger hello mest grundläggande funktioner tooyour mobilappsserverdel genom att exponera en slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="39299-212">hello custom API controller provides hello most basic functionality tooyour Mobile App backend by exposing an endpoint.</span></span> <span data-ttu-id="39299-213">Du kan registrera en mobil-specifika API-kontrollanten med hello [MobileAppController]-attributet.</span><span class="sxs-lookup"><span data-stu-id="39299-213">You can register a mobile-specific API controller using hello [MobileAppController] attribute.</span></span> <span data-ttu-id="39299-214">Hej `MobileAppController` attributet registrerar hello flödet, ställer in hello Mobile Apps JSON-serialisering och aktiverar [klienten versionskontroll](app-service-mobile-client-and-server-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="39299-214">hello `MobileAppController` attribute registers hello route, sets up hello Mobile Apps JSON serializer, and turns on [client version checking](app-service-mobile-client-and-server-versioning.md).</span></span>

1. <span data-ttu-id="39299-215">I Visual Studio, högerklicka på hello domänkontrollanter mappen och klicka på **Lägg till** > **domänkontrollant**väljer **Web API 2-styrenhet&mdash;tom** och Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="39299-215">In Visual Studio, right-click hello Controllers folder, then click **Add** > **Controller**, select **Web API 2 Controller&mdash;Empty** and click **Add**.</span></span>
2. <span data-ttu-id="39299-216">Ange en **Kontrollnamn**, som `CustomController`, och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="39299-216">Supply a **Controller name**, such as `CustomController`, and click **Add**.</span></span>
3. <span data-ttu-id="39299-217">Hello nya klassen för styrenheten, lägga till hello följande med instruktionen:</span><span class="sxs-lookup"><span data-stu-id="39299-217">In hello new controller class file, add hello following using statement:</span></span>

        using Microsoft.Azure.Mobile.Server.Config;
4. <span data-ttu-id="39299-218">Tillämpa hello **[MobileAppController]** attributet toohello API klassdefinitionen styrenhet, som i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="39299-218">Apply hello **[MobileAppController]** attribute toohello API controller class definition, as in hello following example:</span></span>

        [MobileAppController]
        public class CustomController : ApiController
        {
              //...
        }
5. <span data-ttu-id="39299-219">App_Start/Startup.MobileApp.cs filen, lägga till ett anrop toohello **MapApiControllers** metod, som i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="39299-219">In App_Start/Startup.MobileApp.cs file, add a call toohello **MapApiControllers** extension method, as in hello following example:</span></span>

        new MobileAppConfiguration()
            .MapApiControllers()
            .ApplyTo(config);

<span data-ttu-id="39299-220">Du kan också använda hello `UseDefaultConfiguration()` metod i stället för `MapApiControllers()`.</span><span class="sxs-lookup"><span data-stu-id="39299-220">You can also use hello `UseDefaultConfiguration()` extension method instead of `MapApiControllers()`.</span></span> <span data-ttu-id="39299-221">Alla domänkontrollanter som inte har **MobileAppControllerAttribute** tillämpas kan fortfarande användas av klienter, men det kanske inte korrekt förbrukas av klienter som använder en Mobilapp klient-SDK.</span><span class="sxs-lookup"><span data-stu-id="39299-221">Any controller that does not have **MobileAppControllerAttribute** applied can still be accessed by clients, but it may not be correctly consumed by clients using any Mobile App client SDK.</span></span>

## <a name="how-to-work-with-authentication"></a><span data-ttu-id="39299-222">Så här: arbeta med autentisering</span><span class="sxs-lookup"><span data-stu-id="39299-222">How to: Work with authentication</span></span>
<span data-ttu-id="39299-223">Azure Mobile Apps använder App Service-autentisering / auktorisering toosecure din mobila serverdel.</span><span class="sxs-lookup"><span data-stu-id="39299-223">Azure Mobile Apps uses App Service Authentication / Authorization toosecure your mobile backend.</span></span>  <span data-ttu-id="39299-224">Det här avsnittet beskrivs hur du tooperform hello efter autentisering-relaterade uppgifter i projektet .NET backend-servern:</span><span class="sxs-lookup"><span data-stu-id="39299-224">This section shows you how tooperform hello following authentication-related tasks in your .NET backend server project:</span></span>

* [<span data-ttu-id="39299-225">Så här: Lägg till autentisering tooa serverprojekt</span><span class="sxs-lookup"><span data-stu-id="39299-225">How to: Add authentication tooa server project</span></span>](#add-auth)
* [<span data-ttu-id="39299-226">Så här: Använd anpassad autentisering för ditt program</span><span class="sxs-lookup"><span data-stu-id="39299-226">How to: Use custom authentication for your application</span></span>](#custom-auth)
* [<span data-ttu-id="39299-227">Så här: hämta autentiserad användarinformation</span><span class="sxs-lookup"><span data-stu-id="39299-227">How to: Retrieve authenticated user information</span></span>](#user-info)
* [<span data-ttu-id="39299-228">Så här: begränsa åtkomst till data för behöriga användare</span><span class="sxs-lookup"><span data-stu-id="39299-228">How to: Restrict data access for authorized users</span></span>](#authorize)

### <span data-ttu-id="39299-229"><a name="add-auth"></a>Så här: Lägg till autentisering tooa serverprojekt</span><span class="sxs-lookup"><span data-stu-id="39299-229"><a name="add-auth"></a>How to: Add authentication tooa server project</span></span>
<span data-ttu-id="39299-230">Du kan lägga till autentisering tooyour serverprojekt genom att utöka hello **MobileAppConfiguration** objekt och konfigurera OWIN mellanprogram.</span><span class="sxs-lookup"><span data-stu-id="39299-230">You can add authentication tooyour server project by extending hello **MobileAppConfiguration** object and configuring OWIN middleware.</span></span> <span data-ttu-id="39299-231">När du installerar hello [Microsoft.Azure.Mobile.Server.Quickstart] paket- och anrop hello **UseDefaultConfiguration** metod, kan du hoppa över toostep 3.</span><span class="sxs-lookup"><span data-stu-id="39299-231">When you install hello [Microsoft.Azure.Mobile.Server.Quickstart] package and call hello **UseDefaultConfiguration** extension method, you can skip toostep 3.</span></span>

1. <span data-ttu-id="39299-232">I Visual Studio, installera hello [Microsoft.Azure.Mobile.Server.Authentication] paketet.</span><span class="sxs-lookup"><span data-stu-id="39299-232">In Visual Studio, install hello [Microsoft.Azure.Mobile.Server.Authentication] package.</span></span>
2. <span data-ttu-id="39299-233">Hej Startup.cs projektfilen, lägga till följande kodrad hello början av hello hello **Configuration** metoden:</span><span class="sxs-lookup"><span data-stu-id="39299-233">In hello Startup.cs project file, add hello following line of code at hello beginning of hello **Configuration** method:</span></span>

        app.UseAppServiceAuthentication(config);

    <span data-ttu-id="39299-234">Den här OWIN mellanprogram komponenten verifierar token som utfärdats av hello associerade Apptjänst gateway.</span><span class="sxs-lookup"><span data-stu-id="39299-234">This OWIN middleware component validates tokens issued by hello associated App Service gateway.</span></span>
3. <span data-ttu-id="39299-235">Lägg till hello `[Authorize]` attributet tooany domänkontrollant eller metoden som kräver autentisering.</span><span class="sxs-lookup"><span data-stu-id="39299-235">Add hello `[Authorize]` attribute tooany controller or method that requires authentication.</span></span>

<span data-ttu-id="39299-236">toolearn om hur tooauthenticate klienter tooyour Mobile Apps-serverdel finns [Lägg till autentisering tooyour app](app-service-mobile-ios-get-started-users.md).</span><span class="sxs-lookup"><span data-stu-id="39299-236">toolearn about how tooauthenticate clients tooyour Mobile Apps backend, see [Add authentication tooyour app](app-service-mobile-ios-get-started-users.md).</span></span>

### <span data-ttu-id="39299-237"><a name="custom-auth"></a>Så här: Använd anpassad autentisering för ditt program</span><span class="sxs-lookup"><span data-stu-id="39299-237"><a name="custom-auth"></a>How to: Use custom authentication for your application</span></span>
<span data-ttu-id="39299-238">Du kan implementera inloggningen systemet om du inte vill att toouse en av leverantörerna för hello autentisering/auktorisering i Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="39299-238">If you do not wish toouse one of hello App Service Authentication/Authorization providers, you can implement your own login system.</span></span> <span data-ttu-id="39299-239">Installera hello [Microsoft.Azure.Mobile.Server.Login] paketet tooassist med autentisering-token generering.</span><span class="sxs-lookup"><span data-stu-id="39299-239">Install hello [Microsoft.Azure.Mobile.Server.Login] package tooassist with authentication token generation.</span></span>  <span data-ttu-id="39299-240">Ange egen kod vid verifiering av autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="39299-240">Provide your own code for validating user credentials.</span></span> <span data-ttu-id="39299-241">Du kan exempelvis kontrollera mot saltat och hashformaterats lösenord i en databas.</span><span class="sxs-lookup"><span data-stu-id="39299-241">For example, you might check against salted and hashed passwords in a database.</span></span> <span data-ttu-id="39299-242">I hello exemplet nedan hello `isValidAssertion()` metod (definierat någon annanstans) ansvarar för dessa kontroller.</span><span class="sxs-lookup"><span data-stu-id="39299-242">In hello example below, hello `isValidAssertion()` method (defined elsewhere) is responsible for these checks.</span></span>

<span data-ttu-id="39299-243">hello anpassad autentisering exponeras genom att skapa en ApiController och exponera `register` och `login` åtgärder.</span><span class="sxs-lookup"><span data-stu-id="39299-243">hello custom authentication is exposed by creating an ApiController and exposing `register` and `login` actions.</span></span> <span data-ttu-id="39299-244">hello klienten bör använda en anpassad UI toocollect hello-information från hello användare.</span><span class="sxs-lookup"><span data-stu-id="39299-244">hello client should use a custom UI toocollect hello information from hello user.</span></span>  <span data-ttu-id="39299-245">hello information är skickade toohello API med en standard HTTP POST anropa.</span><span class="sxs-lookup"><span data-stu-id="39299-245">hello information is then submitted toohello API with a standard HTTP POST call.</span></span> <span data-ttu-id="39299-246">När hello server verifierar hello assertion, ett token utfärdas med hello `AppServiceLoginHandler.CreateToken()` metod.</span><span class="sxs-lookup"><span data-stu-id="39299-246">Once hello server validates hello assertion, a token is issued using hello `AppServiceLoginHandler.CreateToken()` method.</span></span>  <span data-ttu-id="39299-247">Hej ApiController **bör inte** använda hello `[MobileAppController]` attribut.</span><span class="sxs-lookup"><span data-stu-id="39299-247">hello ApiController **should not** use hello `[MobileAppController]` attribute.</span></span>

<span data-ttu-id="39299-248">Ett exempel `login` åtgärd:</span><span class="sxs-lookup"><span data-stu-id="39299-248">An example `login` action:</span></span>

        public IHttpActionResult Post([FromBody] JObject assertion)
        {
            if (isValidAssertion(assertion)) // user-defined function, checks against a database
            {
                JwtSecurityToken token = AppServiceLoginHandler.CreateToken(new Claim[] { new Claim(JwtRegisteredClaimNames.Sub, assertion["username"]) },
                    mySigningKey,
                    myAppURL,
                    myAppURL,
                    TimeSpan.FromHours(24) );
                return Ok(new LoginResult()
                {
                    AuthenticationToken = token.RawData,
                    User = new LoginResultUser() { UserId = userName.ToString() }
                });
            }
            else // user assertion was not valid
            {
                return this.Request.CreateUnauthorizedResponse();
            }
        }

<span data-ttu-id="39299-249">Hello föregående exempel, är LoginResult och LoginResultUser serialiserbara objekt med synliga nödvändiga egenskaper.</span><span class="sxs-lookup"><span data-stu-id="39299-249">In hello preceding example, LoginResult and LoginResultUser are serializable objects exposing required properties.</span></span> <span data-ttu-id="39299-250">hello klienten förväntar sig inloggningen svar toobe returneras som JSON-objekt på hello formulär:</span><span class="sxs-lookup"><span data-stu-id="39299-250">hello client expects login responses toobe returned as JSON objects of hello form:</span></span>

        {
            "authenticationToken": "<token>",
            "user": {
                "userId": "<userId>"
            }
        }

<span data-ttu-id="39299-251">Hej `AppServiceLoginHandler.CreateToken()` metoden innehåller en *målgruppen* och en *utfärdaren* parameter.</span><span class="sxs-lookup"><span data-stu-id="39299-251">hello `AppServiceLoginHandler.CreateToken()` method includes an *audience* and an *issuer* parameter.</span></span> <span data-ttu-id="39299-252">Båda parametrarna anges toohello URL-Adressen för din programrot med hello HTTPS-schema.</span><span class="sxs-lookup"><span data-stu-id="39299-252">Both of these parameters are set toohello URL of your application root, using hello HTTPS scheme.</span></span> <span data-ttu-id="39299-253">På samma sätt bör du ange *secretKey* signeringsnyckel för toobe hello värdet för ditt program.</span><span class="sxs-lookup"><span data-stu-id="39299-253">Similarly you should set *secretKey* toobe hello value of your application's signing key.</span></span> <span data-ttu-id="39299-254">Distribuera inte hello signeringsnyckel i en klient som kan använda toomint nycklar och Personifiera användare.</span><span class="sxs-lookup"><span data-stu-id="39299-254">Do not distribute hello signing key in a client as it can be used toomint keys and impersonate users.</span></span> <span data-ttu-id="39299-255">Du kan hämta hello signeringsnyckel medan finns i App Service genom att referera hello *webbplats\_AUTH\_signering\_NYCKELN* miljövariabeln.</span><span class="sxs-lookup"><span data-stu-id="39299-255">You can obtain hello signing key while hosted in App Service by referencing hello *WEBSITE\_AUTH\_SIGNING\_KEY* environment variable.</span></span> <span data-ttu-id="39299-256">Om det behövs i en kontext som lokala felsökning, följer du anvisningarna för hello i hello [lokala felsökning med autentisering](#local-debug) avsnittet tooretrieve hello nyckeln och spara den som en programinställning.</span><span class="sxs-lookup"><span data-stu-id="39299-256">If needed in a local debugging context, follow hello instructions in hello [Local debugging with authentication](#local-debug) section tooretrieve hello key and store it as an application setting.</span></span>

<span data-ttu-id="39299-257">hello utfärdade token kan också omfatta andra anspråk och upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="39299-257">hello issued token may also include other claims and an expiry date.</span></span>  <span data-ttu-id="39299-258">Åtminstone hello utfärdade token måste innehålla ett ämne (**sub**) anspråk.</span><span class="sxs-lookup"><span data-stu-id="39299-258">Minimally, hello issued token must include a subject (**sub**) claim.</span></span>

<span data-ttu-id="39299-259">Du kan använda hello standard klienten `loginAsync()` metoden av överbelastning hello autentisering vägen.</span><span class="sxs-lookup"><span data-stu-id="39299-259">You can support hello standard client `loginAsync()` method by overloading hello authentication route.</span></span>  <span data-ttu-id="39299-260">Om hello klienten anropar `client.loginAsync('custom');` toolog i rutten måste vara `/.auth/login/custom`.</span><span class="sxs-lookup"><span data-stu-id="39299-260">If hello client calls `client.loginAsync('custom');` toolog in, your route must be `/.auth/login/custom`.</span></span>  <span data-ttu-id="39299-261">Du kan ange hello väg för hello anpassad autentisering domänkontrollanten med `MapHttpRoute()`:</span><span class="sxs-lookup"><span data-stu-id="39299-261">You can set hello route for hello custom authentication controller using `MapHttpRoute()`:</span></span>

    config.Routes.MapHttpRoute("custom", ".auth/login/custom", new { controller = "CustomAuth" });

> [!TIP]
> <span data-ttu-id="39299-262">Med hjälp av hello `loginAsync()` metoden gör att hello autentiseringstoken är ansluten tooevery efterföljande anrop toohello service.</span><span class="sxs-lookup"><span data-stu-id="39299-262">Using hello `loginAsync()` approach ensures that hello authentication token is attached tooevery subsequent call toohello service.</span></span>
>
>

### <span data-ttu-id="39299-263"><a name="user-info"></a>Så här: hämta autentiserad användarinformation</span><span class="sxs-lookup"><span data-stu-id="39299-263"><a name="user-info"></a>How to: Retrieve authenticated user information</span></span>
<span data-ttu-id="39299-264">När en användare autentiseras av App Service kan du komma åt hello tilldelade användar-ID och annan information i serverdelskoden .NET.</span><span class="sxs-lookup"><span data-stu-id="39299-264">When a user is authenticated by App Service, you can access hello assigned user ID and other information in your .NET backend code.</span></span> <span data-ttu-id="39299-265">hello användarinformation kan användas för att göra auktoriseringsbeslut i hello backend.</span><span class="sxs-lookup"><span data-stu-id="39299-265">hello user information can be used for making authorization decisions in hello backend.</span></span> <span data-ttu-id="39299-266">hello hämtar följande kod hello användar-ID som är associerade med en begäran:</span><span class="sxs-lookup"><span data-stu-id="39299-266">hello following code obtains hello user ID associated with a request:</span></span>

    // Get hello SID of hello current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;

<span data-ttu-id="39299-267">hello SID är härledd från hello provider-specifik användar-ID och är statisk för en viss användare och inloggningen provider.</span><span class="sxs-lookup"><span data-stu-id="39299-267">hello SID is derived from hello provider-specific user ID and is static for a given user and login provider.</span></span>  <span data-ttu-id="39299-268">hello SID är null för ogiltig autentiseringstoken.</span><span class="sxs-lookup"><span data-stu-id="39299-268">hello SID is null for invalid authentication tokens.</span></span>

<span data-ttu-id="39299-269">Apptjänst kan du begära specifika anspråk från leverantören inloggningen.</span><span class="sxs-lookup"><span data-stu-id="39299-269">App Service also lets you request specific claims from your login provider.</span></span> <span data-ttu-id="39299-270">Varje identitetsleverantör kan ge mer information med hjälp av identitetsleverantör SDK.</span><span class="sxs-lookup"><span data-stu-id="39299-270">Each identity provider can provide more information using the identity provider SDK.</span></span>  <span data-ttu-id="39299-271">Du kan till exempel använda hello Facebook Graph API vänner information.</span><span class="sxs-lookup"><span data-stu-id="39299-271">For example, you can use hello Facebook Graph API for friends information.</span></span>  <span data-ttu-id="39299-272">Du kan ange anspråk som begärs i hello providern bladet i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="39299-272">You can specify claims that are requested in hello provider blade in hello Azure portal.</span></span> <span data-ttu-id="39299-273">Vissa anspråk kräva ytterligare konfiguration med hello identitetsprovider.</span><span class="sxs-lookup"><span data-stu-id="39299-273">Some claims require additional configuration with hello identity provider.</span></span>

<span data-ttu-id="39299-274">hello följande kod anrop hello **GetAppServiceIdentityAsync** tillägget metoden tooget hello inloggningsuppgifterna, bland annat hello token nödvändiga toomake förfrågningar mot hello Facebook Graph API:</span><span class="sxs-lookup"><span data-stu-id="39299-274">hello following code calls hello **GetAppServiceIdentityAsync** extension method tooget hello login credentials, which include hello access token needed toomake requests against hello Facebook Graph API:</span></span>

    // Get hello credentials for hello logged-in user.
    var credentials =
        await this.User
        .GetAppServiceIdentityAsync<FacebookCredentials>(this.Request);

    if (credentials.Provider == "Facebook")
    {
        // Create a query string with hello Facebook access token.
        var fbRequestUrl = "https://graph.facebook.com/me/feed?access_token="
            + credentials.AccessToken;

        // Create an HttpClient request.
        var client = new System.Net.Http.HttpClient();

        // Request hello current user info from Facebook.
        var resp = await client.GetAsync(fbRequestUrl);
        resp.EnsureSuccessStatusCode();

        // Do something here with hello Facebook user information.
        var fbInfo = await resp.Content.ReadAsStringAsync();
    }

<span data-ttu-id="39299-275">Lägga till ett using-uttryck för `System.Security.Principal` tooprovide hello **GetAppServiceIdentityAsync** metod.</span><span class="sxs-lookup"><span data-stu-id="39299-275">Add a using statement for `System.Security.Principal` tooprovide hello **GetAppServiceIdentityAsync** extension method.</span></span>

### <span data-ttu-id="39299-276"><a name="authorize"></a>Så här: begränsa åtkomst till data för behöriga användare</span><span class="sxs-lookup"><span data-stu-id="39299-276"><a name="authorize"></a>How to: Restrict data access for authorized users</span></span>
<span data-ttu-id="39299-277">Under hello tidigare visades hur tooretrieve hello användar-ID för en autentiserad användare.</span><span class="sxs-lookup"><span data-stu-id="39299-277">In hello previous section, we showed how tooretrieve hello user ID of an authenticated user.</span></span> <span data-ttu-id="39299-278">Du kan begränsa åtkomst toodata och andra resurser baserat på det här värdet.</span><span class="sxs-lookup"><span data-stu-id="39299-278">You can restrict access toodata and other resources based on this value.</span></span> <span data-ttu-id="39299-279">Till exempel är att lägga till en kolumn tootables för användar-ID och filtrering hello frågeresultat av hello användar-ID ett enkelt sätt toolimit returnerade data endast tooauthorized användare.</span><span class="sxs-lookup"><span data-stu-id="39299-279">For example, adding a userId column tootables and filtering hello query results by hello user ID is a simple way toolimit returned data only tooauthorized users.</span></span> <span data-ttu-id="39299-280">hello returnerar följande kod rader med data endast när hello SID matchar värdet i kolumnen för hello användar-ID för hello TodoItem-tabellen:</span><span class="sxs-lookup"><span data-stu-id="39299-280">hello following code returns data rows only when hello SID matches the value in hello UserId column on hello TodoItem table:</span></span>

    // Get hello SID of hello current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;

    // Only return data rows that belong toohello current user.
    return Query().Where(t => t.UserId == sid);

<span data-ttu-id="39299-281">Hej `Query()` metoden returnerar en `IQueryable` som kan ändras av LINQ toohandle filtrering.</span><span class="sxs-lookup"><span data-stu-id="39299-281">hello `Query()` method returns an `IQueryable` that can be manipulated by LINQ toohandle filtering.</span></span>

## <a name="how-to-add-push-notifications-tooa-server-project"></a><span data-ttu-id="39299-282">Så här: Lägg till push-meddelanden tooa server-projekt</span><span class="sxs-lookup"><span data-stu-id="39299-282">How to: Add push notifications tooa server project</span></span>
<span data-ttu-id="39299-283">Lägg till push-meddelanden tooyour serverprojektet genom att utöka hello **MobileAppConfiguration** objektet och skapar en klient med Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="39299-283">Add push notifications tooyour server project by extending hello **MobileAppConfiguration** object and creating a Notification Hubs client.</span></span>

1. <span data-ttu-id="39299-284">Högerklicka på hello serverprojekt i Visual Studio och klicka på **hantera NuGet-paket**, söka efter `Microsoft.Azure.Mobile.Server.Notifications`, klicka på **installera**.</span><span class="sxs-lookup"><span data-stu-id="39299-284">In Visual Studio, right-click hello server project and click **Manage NuGet Packages**, search for `Microsoft.Azure.Mobile.Server.Notifications`, then click **Install**.</span></span>
2. <span data-ttu-id="39299-285">Upprepa det här steget tooinstall hello `Microsoft.Azure.NotificationHubs` paket som innehåller klientbiblioteket för hello Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="39299-285">Repeat this step tooinstall hello `Microsoft.Azure.NotificationHubs` package, which includes hello Notification Hubs client library.</span></span>
3. <span data-ttu-id="39299-286">I App_Start/Startup.MobileApp.cs, och Lägg till ett anrop toohello **AddPushNotifications()** tilläggsmetoden under initieringen:</span><span class="sxs-lookup"><span data-stu-id="39299-286">In App_Start/Startup.MobileApp.cs, and add a call toohello **AddPushNotifications()** extension method during initialization:</span></span>

        new MobileAppConfiguration()
            // other features...
            .AddPushNotifications()
            .ApplyTo(config);
4. <span data-ttu-id="39299-287">Lägg till hello följande kod som skapar en Meddelandehubbar klient:</span><span class="sxs-lookup"><span data-stu-id="39299-287">Add hello following code that creates a Notification Hubs client:</span></span>

        // Get hello settings for hello server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings =
            config.GetMobileAppSettingsProvider().GetMobileAppSettings();

        // Get hello Notification Hubs credentials for hello Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create a new Notification Hub client.
        NotificationHubClient hub = NotificationHubClient
            .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

<span data-ttu-id="39299-288">Du kan nu använda hello Meddelandehubbar toosend push-meddelanden tooregistered klientenheter.</span><span class="sxs-lookup"><span data-stu-id="39299-288">You can now use hello Notification Hubs client toosend push notifications tooregistered devices.</span></span> <span data-ttu-id="39299-289">Mer information finns i [Lägg till push-meddelanden tooyour app](app-service-mobile-ios-get-started-push.md).</span><span class="sxs-lookup"><span data-stu-id="39299-289">For more information, see [Add push notifications tooyour app](app-service-mobile-ios-get-started-push.md).</span></span> <span data-ttu-id="39299-290">toolearn mer information om Notification Hubs finns [översikt över Notification Hubs](../notification-hubs/notification-hubs-push-notification-overview.md).</span><span class="sxs-lookup"><span data-stu-id="39299-290">toolearn more about Notification Hubs, see [Notification Hubs Overview](../notification-hubs/notification-hubs-push-notification-overview.md).</span></span>

## <span data-ttu-id="39299-291"><a name="tags"></a>Så här: aktivera riktade push med hjälp av taggar</span><span class="sxs-lookup"><span data-stu-id="39299-291"><a name="tags"></a>How to: Enable targeted push using Tags</span></span>
<span data-ttu-id="39299-292">Notification Hubs kan du skicka riktade meddelanden toospecific registreringar med hjälp av taggar.</span><span class="sxs-lookup"><span data-stu-id="39299-292">Notification Hubs lets you send targeted notifications toospecific registrations by using tags.</span></span> <span data-ttu-id="39299-293">Flera etiketter skapas automatiskt:</span><span class="sxs-lookup"><span data-stu-id="39299-293">Several tags are created automatically:</span></span>

* <span data-ttu-id="39299-294">hello installations-ID identifierar en specifik enhet.</span><span class="sxs-lookup"><span data-stu-id="39299-294">hello Installation ID identifies a specific device.</span></span>
* <span data-ttu-id="39299-295">hello användar-Id utifrån hello autentiserad SID identifierar en specifik användare.</span><span class="sxs-lookup"><span data-stu-id="39299-295">hello User Id based on hello authenticated SID identifies a specific user.</span></span>

<span data-ttu-id="39299-296">Hej installationen ID kan nås från hello **installationId** egenskapen hello **MobileServiceClient**.</span><span class="sxs-lookup"><span data-stu-id="39299-296">hello installation ID can be accessed from hello **installationId** property on hello **MobileServiceClient**.</span></span>  <span data-ttu-id="39299-297">hello visar följande exempel hur du använder ett installations-ID tooadd en tagg tooa installation i Notification Hubs:</span><span class="sxs-lookup"><span data-stu-id="39299-297">hello following example shows how to use an installation ID tooadd a tag tooa specific installation in Notification Hubs:</span></span>

    hub.PatchInstallation("my-installation-id", new[]
    {
        new PartialUpdateOperation
        {
            Operation = UpdateOperationType.Add,
            Path = "/tags",
            Value = "{my-tag}"
        }
    });

<span data-ttu-id="39299-298">Alla taggar som tillhandahålls av hello klienten under registreringen av push-meddelanden ignoreras av hello backend när du skapar hello-installationen.</span><span class="sxs-lookup"><span data-stu-id="39299-298">Any tags supplied by hello client during push notification registration are ignored by hello backend when creating hello installation.</span></span> <span data-ttu-id="39299-299">tooenable en klient tooadd taggar toohello installationen måste du skapa en anpassad API som lägger till taggar i hello föregående mönster.</span><span class="sxs-lookup"><span data-stu-id="39299-299">tooenable a client tooadd tags toohello installation, you must create a custom API that adds tags using hello preceding pattern.</span></span>

<span data-ttu-id="39299-300">Se [klienten tillagda push notification taggar] [ 5] i hello Apptjänst Mobilappar slutförda quickstart prov ett exempel.</span><span class="sxs-lookup"><span data-stu-id="39299-300">See [Client-added push notification tags][5] in hello App Service Mobile Apps completed quickstart sample for an example.</span></span>

## <span data-ttu-id="39299-301"><a name="push-user"></a>Så här: skicka push-meddelanden tooan autentiserade användare</span><span class="sxs-lookup"><span data-stu-id="39299-301"><a name="push-user"></a>How to: Send push notifications tooan authenticated user</span></span>
<span data-ttu-id="39299-302">När en autentiserad användare registrerar för push-meddelanden, läggs en tagg för användar-ID automatiskt toohello registrering.</span><span class="sxs-lookup"><span data-stu-id="39299-302">When an authenticated user registers for push notifications, a user ID tag is automatically added toohello registration.</span></span> <span data-ttu-id="39299-303">Du kan skicka meddelanden tooall enheter som registrerats som personen push med hjälp av den här taggen.</span><span class="sxs-lookup"><span data-stu-id="39299-303">By using this tag, you can send push notifications tooall devices registered by that person.</span></span> <span data-ttu-id="39299-304">hello följande kod hämtar hello SID för användare som skickar begäran och skickar en mall push notification tooevery registrering av enheten för den personen:</span><span class="sxs-lookup"><span data-stu-id="39299-304">hello following code gets hello SID of user making the request and sends a template push notification tooevery device registration for that person:</span></span>

    // Get hello current user SID and create a tag for hello current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;
    string userTag = "_UserId:" + sid;

    // Build a dictionary for hello template with hello item message text.
    var notification = new Dictionary<string, string> { { "message", item.Text } };

    // Send a template notification toohello user ID.
    await hub.SendTemplateNotificationAsync(notification, userTag);

<span data-ttu-id="39299-305">När du registrerar dig för push-meddelanden från en autentiserad klient, se till att autentiseringen har slutförts innan du försöker utföra registreringen.</span><span class="sxs-lookup"><span data-stu-id="39299-305">When registering for push notifications from an authenticated client, make sure that authentication is complete before attempting registration.</span></span> <span data-ttu-id="39299-306">Mer information finns i [Push toousers] [ 6] i hello Apptjänst Mobilappar slutförda quickstart prov för .NET-serverdel.</span><span class="sxs-lookup"><span data-stu-id="39299-306">For more information, see [Push toousers][6] in hello App Service Mobile Apps completed quickstart sample for .NET backend.</span></span>

## <a name="how-to-debug-and-troubleshoot-hello-net-server-sdk"></a><span data-ttu-id="39299-307">Så här: felsöka och felsöka hello SDK för .NET-Server</span><span class="sxs-lookup"><span data-stu-id="39299-307">How to: Debug and troubleshoot hello .NET Server SDK</span></span>
<span data-ttu-id="39299-308">Azure App Service tillhandahåller flera felsökning och felsökningsmetoder för ASP.NET-program:</span><span class="sxs-lookup"><span data-stu-id="39299-308">Azure App Service provides several debugging and troubleshooting techniques for ASP.NET applications:</span></span>

* [<span data-ttu-id="39299-309">Övervaka en Azure Apptjänst</span><span class="sxs-lookup"><span data-stu-id="39299-309">Monitoring an Azure App Service</span></span>](../app-service-web/web-sites-monitor.md)
* [<span data-ttu-id="39299-310">Aktivera diagnostikloggning i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="39299-310">Enable Diagnostic Logging in Azure App Service</span></span>](../app-service-web/web-sites-enable-diagnostic-log.md)
* [<span data-ttu-id="39299-311">Felsöka en Azure Apptjänst i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="39299-311">Troubleshoot an Azure App Service in Visual Studio</span></span>](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md)

### <a name="logging"></a><span data-ttu-id="39299-312">Loggning</span><span class="sxs-lookup"><span data-stu-id="39299-312">Logging</span></span>
<span data-ttu-id="39299-313">Du kan skriva tooApp diagnostikloggarna för tjänsten med hjälp av hello standard ASP.NET trace skrivning.</span><span class="sxs-lookup"><span data-stu-id="39299-313">You can write tooApp Service diagnostic logs by using hello standard ASP.NET trace writing.</span></span> <span data-ttu-id="39299-314">Innan du kan skriva toohello loggar, måste du aktivera diagnostik i din mobilappsserverdel.</span><span class="sxs-lookup"><span data-stu-id="39299-314">Before you can write toohello logs, you must enable diagnostics in your Mobile App backend.</span></span>

<span data-ttu-id="39299-315">tooenable diagnostik- och skrivåtgärder toohello loggar:</span><span class="sxs-lookup"><span data-stu-id="39299-315">tooenable diagnostics and write toohello logs:</span></span>

1. <span data-ttu-id="39299-316">Gör så hello i [hur tooenable diagnostik](../app-service-web/web-sites-enable-diagnostic-log.md#enablediag).</span><span class="sxs-lookup"><span data-stu-id="39299-316">Follow hello steps in [How tooenable diagnostics](../app-service-web/web-sites-enable-diagnostic-log.md#enablediag).</span></span>
2. <span data-ttu-id="39299-317">Lägg till hello följande med instruktionen i kodfilen:</span><span class="sxs-lookup"><span data-stu-id="39299-317">Add hello following using statement in your code file:</span></span>

        using System.Web.Http.Tracing;
3. <span data-ttu-id="39299-318">Skapa en spårning writer toowrite från hello .NET-serverdel toohello diagnostikloggar, enligt följande:</span><span class="sxs-lookup"><span data-stu-id="39299-318">Create a trace writer toowrite from hello .NET backend toohello diagnostic logs, as follows:</span></span>

        ITraceWriter traceWriter = this.Configuration.Services.GetTraceWriter();
        traceWriter.Info("Hello, World");
4. <span data-ttu-id="39299-319">Publicera serverprojektet och komma åt hello Mobilapp backend tooexecute hello kodsökvägen med hello loggning.</span><span class="sxs-lookup"><span data-stu-id="39299-319">Republish your server project, and access hello Mobile App backend tooexecute hello code path with hello logging.</span></span>
5. <span data-ttu-id="39299-320">Hämta och utvärdera hello loggar, enligt beskrivningen i [så här: hämta loggar](../app-service-web/web-sites-enable-diagnostic-log.md#download).</span><span class="sxs-lookup"><span data-stu-id="39299-320">Download and evaluate hello logs, as described in [How to: Download logs](../app-service-web/web-sites-enable-diagnostic-log.md#download).</span></span>

### <span data-ttu-id="39299-321"><a name="local-debug"></a>Lokala felsökning med autentisering</span><span class="sxs-lookup"><span data-stu-id="39299-321"><a name="local-debug"></a>Local debugging with authentication</span></span>
<span data-ttu-id="39299-322">Du kan köra programmet lokalt tootest ändras innan du publicerar dem. toohello moln.</span><span class="sxs-lookup"><span data-stu-id="39299-322">You can run your application locally tootest changes before publishing them toohello cloud.</span></span> <span data-ttu-id="39299-323">Tryck på för de flesta Azure Mobile Apps serverdelar *F5* medan i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="39299-323">For most Azure Mobile Apps backends, press *F5* while in Visual Studio.</span></span> <span data-ttu-id="39299-324">Det finns dock några ytterligare överväganden när du använder autentisering.</span><span class="sxs-lookup"><span data-stu-id="39299-324">However, there are some additional considerations when using authentication.</span></span>

<span data-ttu-id="39299-325">Du måste ha en molnbaserad mobilapp med autentisering/auktorisering i Apptjänst konfigurerats och klienten måste ha hello molnet slutpunkten som specificeras som hello alternativt inloggnings-värd.</span><span class="sxs-lookup"><span data-stu-id="39299-325">You must have a cloud-based mobile app with App Service Authentication/Authorization configured, and your client must have hello cloud endpoint specified as hello alternate login host.</span></span> <span data-ttu-id="39299-326">Hello dokumentationen för din klientplattform för hello särskilda åtgärder krävs.</span><span class="sxs-lookup"><span data-stu-id="39299-326">See hello documentation for your client platform for hello specific steps required.</span></span>

<span data-ttu-id="39299-327">Kontrollera att din mobila serverdel har [Microsoft.Azure.Mobile.Server.Authentication] installerad.</span><span class="sxs-lookup"><span data-stu-id="39299-327">Ensure that your mobile backend has [Microsoft.Azure.Mobile.Server.Authentication] installed.</span></span> <span data-ttu-id="39299-328">Sedan, i ditt program OWIN-startklass, Lägg till hello följande efter `MobileAppConfiguration` har tillämpade tooyour `HttpConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="39299-328">Then, in your application's OWIN startup class, add hello following, after `MobileAppConfiguration` has been applied tooyour `HttpConfiguration`:</span></span>

        app.UseAppServiceAuthentication(new AppServiceAuthenticationOptions()
        {
            SigningKey = ConfigurationManager.AppSettings["authSigningKey"],
            ValidAudiences = new[] { ConfigurationManager.AppSettings["authAudience"] },
            ValidIssuers = new[] { ConfigurationManager.AppSettings["authIssuer"] },
            TokenHandler = config.GetAppServiceTokenHandler()
        });

<span data-ttu-id="39299-329">I föregående exempel hello, bör du konfigurera hello *authAudience* och *authIssuer* programinställningar i din Web.config-filen tooeach att URL-Adressen för din programrot med hello HTTPS schemat.</span><span class="sxs-lookup"><span data-stu-id="39299-329">In hello preceding example, you should configure hello *authAudience* and *authIssuer* application settings within your Web.config file tooeach be the URL of your application root, using hello HTTPS scheme.</span></span> <span data-ttu-id="39299-330">På samma sätt bör du ange *authSigningKey* signeringsnyckel för toobe hello värdet för ditt program.</span><span class="sxs-lookup"><span data-stu-id="39299-330">Similarly you should set *authSigningKey* toobe hello value of your application's signing key.</span></span>
<span data-ttu-id="39299-331">tooobtain hello signeringsnyckeln:</span><span class="sxs-lookup"><span data-stu-id="39299-331">tooobtain hello signing key:</span></span>

1. <span data-ttu-id="39299-332">Navigera tooyour app i hello [Azure-portalen]</span><span class="sxs-lookup"><span data-stu-id="39299-332">Navigate tooyour app within hello [Azure portal]</span></span>
2. <span data-ttu-id="39299-333">Klicka på **verktyg**, **Kudu**, **Gå**.</span><span class="sxs-lookup"><span data-stu-id="39299-333">Click **Tools**, **Kudu**, **Go**.</span></span>
3. <span data-ttu-id="39299-334">I hello Kudu hanteringswebbplats, klickar du på **miljö**.</span><span class="sxs-lookup"><span data-stu-id="39299-334">In hello Kudu Management site, click **Environment**.</span></span>
4. <span data-ttu-id="39299-335">Hitta hello värdet för *webbplats\_AUTH\_signering\_NYCKELN*.</span><span class="sxs-lookup"><span data-stu-id="39299-335">Find hello value for *WEBSITE\_AUTH\_SIGNING\_KEY*.</span></span>

<span data-ttu-id="39299-336">Använd hello signeringsnyckel för hello *authSigningKey* parameter i din lokala program config.  Din mobila serverdel är nu utrustad toovalidate token när du kör lokalt, vilka hello-klienten hämtar hello-token från hello molnbaserade slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="39299-336">Use hello signing key for hello *authSigningKey* parameter in your local application config.  Your mobile backend is now equipped toovalidate tokens when running locally, which hello client obtains hello token from hello cloud-based endpoint.</span></span>

[1]: https://msdn.microsoft.com/library/azure/dn961176.aspx
[2]: https://github.com/Azure/azure-mobile-apps-net-server
[3]: app-service-mobile-ios-get-started.md
[4]: https://azure.microsoft.com/downloads/
[5]: https://github.com/Azure-Samples/app-service-mobile-dotnet-backend-quickstart/blob/master/README.md#client-added-push-notification-tags
[6]: https://github.com/Azure-Samples/app-service-mobile-dotnet-backend-quickstart/blob/master/README.md#push-to-users
[Azure-portalen]: https://portal.azure.com
[NuGet.org]: http://www.nuget.org/
[Microsoft.Azure.Mobile.Server]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/
[Microsoft.Azure.Mobile.Server.Quickstart]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Quickstart/
[Microsoft.Azure.Mobile.Server.Authentication]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/
[Microsoft.Azure.Mobile.Server.Login]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Login/
[Microsoft.Azure.Mobile.Server.Notifications]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Notifications/
[MapHttpAttributeRoutes]: https://msdn.microsoft.com/library/dn479134(v=vs.118).aspx
