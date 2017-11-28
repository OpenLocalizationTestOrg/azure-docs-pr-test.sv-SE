---
title: "aaaUpgrade från Mobile Services tooAzure App Service - Node.js"
description: "Lär dig hur tooeasily uppgradera din Mobile Services programmet tooan Apptjänst Mobile App"
services: app-service\mobile
documentationcenter: 
author: ggailey777
manager: yochayk
editor: 
ms.assetid: c58f6df0-5aad-40a3-bddc-319c378218e3
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile
ms.devlang: node
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 722cda244d4f633247827f58ea6f1397137ea600
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-your-existing-nodejs-azure-mobile-service-tooapp-service"></a><span data-ttu-id="eb93d-103">Uppgradera din befintliga Mobiltjänst för Node.js Azure tooApp Service</span><span class="sxs-lookup"><span data-stu-id="eb93d-103">Upgrade your existing Node.js Azure Mobile Service tooApp Service</span></span>
<span data-ttu-id="eb93d-104">Apptjänst Mobile är ett nytt sätt toobuild mobila program med Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="eb93d-104">App Service Mobile is a new way toobuild mobile applications using Microsoft Azure.</span></span> <span data-ttu-id="eb93d-105">Det finns fler toolearn [vad är Mobilappar?].</span><span class="sxs-lookup"><span data-stu-id="eb93d-105">toolearn more, see [What are Mobile Apps?].</span></span>

<span data-ttu-id="eb93d-106">Det här avsnittet beskrivs hur tooupgrade ett befintligt Node.js-serverdel program från Azure Mobile Services tooa nya Mobile Apps i Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="eb93d-106">This topic describes how tooupgrade an existing Node.js backend application from Azure Mobile Services tooa new App Service Mobile Apps.</span></span> <span data-ttu-id="eb93d-107">Befintliga Mobile Services programmet kan fortsätta toooperate medan du utför uppgraderingen.</span><span class="sxs-lookup"><span data-stu-id="eb93d-107">While you perform this upgrade, your existing Mobile Services application can continue toooperate.</span></span>  <span data-ttu-id="eb93d-108">Om du behöver tooupgrade ett Node.js-serverdel program, se för[uppgradera din Mobile Services .NET](app-service-mobile-net-upgrading-from-mobile-services.md).</span><span class="sxs-lookup"><span data-stu-id="eb93d-108">If you need tooupgrade a Node.js backend application, refer too[Upgrading your .NET Mobile Services](app-service-mobile-net-upgrading-from-mobile-services.md).</span></span>

<span data-ttu-id="eb93d-109">När en mobilserverdel är uppgraderade tooAzure Apptjänst, har den åtkomst tooall App Service-funktionerna och är debiteras enligt för[priser för Apptjänst], inte Mobile Services priser.</span><span class="sxs-lookup"><span data-stu-id="eb93d-109">When a mobile backend is upgraded tooAzure App Service, it has access tooall App Service features and are billed according too[App Service pricing], not Mobile Services pricing.</span></span>

## <a name="migrate-vs-upgrade"></a><span data-ttu-id="eb93d-110">Migrera kontra uppgradering</span><span class="sxs-lookup"><span data-stu-id="eb93d-110">Migrate vs. upgrade</span></span>
[!INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

> [!TIP]
> <span data-ttu-id="eb93d-111">Vi rekommenderar att du [utför en migrering](app-service-mobile-migrating-from-mobile-services.md) innan du fortsätter med uppgraderingen.</span><span class="sxs-lookup"><span data-stu-id="eb93d-111">It is recommended that you [perform a migration](app-service-mobile-migrating-from-mobile-services.md) before going through an upgrade.</span></span> <span data-ttu-id="eb93d-112">På så sätt kan du placera båda versionerna av programmet hello samma App Service-Plan och innebära utan extra kostnad.</span><span class="sxs-lookup"><span data-stu-id="eb93d-112">This way, you can put both versions of your application on hello same App Service Plan and incur no additional cost.</span></span>
>
>

### <a name="improvements-in-mobile-apps-nodejs-server-sdk"></a><span data-ttu-id="eb93d-113">Förbättringar i Mobile Apps Node.js server-SDK</span><span class="sxs-lookup"><span data-stu-id="eb93d-113">Improvements in Mobile Apps Node.js server SDK</span></span>
<span data-ttu-id="eb93d-114">Uppgradera toohello nya [Mobile Apps-SDK](https://www.npmjs.com/package/azure-mobile-apps) innehåller många förbättringar, inklusive:</span><span class="sxs-lookup"><span data-stu-id="eb93d-114">Upgrading toohello new [Mobile Apps SDK](https://www.npmjs.com/package/azure-mobile-apps) provides a lot of improvements, including:</span></span>

* <span data-ttu-id="eb93d-115">Baserat på hello [Express framework](http://expressjs.com/en/index.html), hello nya nod SDK är inaktiverad och är utformad tookeep upp med den nya noden versioner när de kommer. Du kan anpassa hello programmets beteende med Express-mellanprogrammet.</span><span class="sxs-lookup"><span data-stu-id="eb93d-115">Based on hello [Express framework](http://expressjs.com/en/index.html), hello new Node SDK is light-weight and designed tookeep up with new Node versions as they come out. You can customize hello application behavior with Express middleware.</span></span>
* <span data-ttu-id="eb93d-116">Betydande prestandaförbättringar jämfört med toohello Mobile Services SDK.</span><span class="sxs-lookup"><span data-stu-id="eb93d-116">Significant performance improvements compared toohello Mobile Services SDK.</span></span>
* <span data-ttu-id="eb93d-117">Du kan nu vara värd för en webbplats tillsammans med din mobila serverdel; på samma sätt är det enkelt tooadd hello Azure Mobile SDK tooany befintligt express.v4 program.</span><span class="sxs-lookup"><span data-stu-id="eb93d-117">You can now host a website together with your mobile backend; similarly, it's easy tooadd hello Azure Mobile SDK tooany existing express.v4 application.</span></span>
* <span data-ttu-id="eb93d-118">Bygger för flera plattformar och lokala utveckling hello Mobile Apps-SDK kan utvecklas och köras lokalt på Windows, Linux och OS x-plattformar.</span><span class="sxs-lookup"><span data-stu-id="eb93d-118">Built for cross-platform and local development, hello Mobile Apps SDK can be developed and run locally on Windows, Linux, and OSX platforms.</span></span> <span data-ttu-id="eb93d-119">Nu är det enkelt toouse vanliga nod utvecklingstekniker som körs [Mocha](https://mochajs.org/) testar tidigare toodeployment.</span><span class="sxs-lookup"><span data-stu-id="eb93d-119">It's now easy toouse common Node development techniques like running [Mocha](https://mochajs.org/) tests prior toodeployment.</span></span>

## <span data-ttu-id="eb93d-120"><a name="overview"></a>Uppgradera översikt</span><span class="sxs-lookup"><span data-stu-id="eb93d-120"><a name="overview"></a>Basic upgrade overview</span></span>
<span data-ttu-id="eb93d-121">tooaid vid uppgradering av en Node.js-serverdel Azure App Service tillhandahåller ett paket för kompatibilitet.</span><span class="sxs-lookup"><span data-stu-id="eb93d-121">tooaid in upgrading a Node.js backend, Azure App Service has provided a compatibility package.</span></span>  <span data-ttu-id="eb93d-122">Efter uppgraderingen måste en niew plats som kan vara distribuerade tooa nya App Service-platsen.</span><span class="sxs-lookup"><span data-stu-id="eb93d-122">After upgrade, you will have a niew site that can be deployed tooa new App Service site.</span></span>

<span data-ttu-id="eb93d-123">hello Mobile Services-klienten SDK: er **inte** kompatibel med hello ny Mobile Apps server SDK.</span><span class="sxs-lookup"><span data-stu-id="eb93d-123">hello Mobile Services client SDKs are **not** compatible with hello new Mobile Apps server SDK.</span></span> <span data-ttu-id="eb93d-124">I ordning tooprovide tjänstens stabilitet för din app, bör du inte publicera ändringar tooa plats som betjänar publicerade klienter.</span><span class="sxs-lookup"><span data-stu-id="eb93d-124">In order tooprovide continuity of service for your app, you should not publish changes tooa site currently serving published clients.</span></span> <span data-ttu-id="eb93d-125">I stället bör du skapa en ny mobil app som fungerar som en dubblett.</span><span class="sxs-lookup"><span data-stu-id="eb93d-125">Instead, you should create a new mobile app that serves as a duplicate.</span></span> <span data-ttu-id="eb93d-126">Du kan publicera programmet hello samma Apptjänst planerar tooavoid medför ytterligare kostnader.</span><span class="sxs-lookup"><span data-stu-id="eb93d-126">You can put this application on hello same App Service plan tooavoid incurring additional financial cost.</span></span>

<span data-ttu-id="eb93d-127">Du måste sedan två versioner av programmet hello: en som förblir hello samma fungerar publicerade appar i hello jokertecken och andra som du kan sedan uppgradera och mål med en ny klient.</span><span class="sxs-lookup"><span data-stu-id="eb93d-127">You will then have two versions of hello application: one that stays hello same and serves published apps in hello wild, and the other which you can then upgrade and target with a new client release.</span></span> <span data-ttu-id="eb93d-128">Du kan flytta och testa din kod i din takt, men du bör se till att alla felkorrigeringar du komma tillämpade tooboth.</span><span class="sxs-lookup"><span data-stu-id="eb93d-128">You can move and test your code at your pace, but you should make sure that any bug fixes you make get applied tooboth.</span></span> <span data-ttu-id="eb93d-129">När du anser att ett önskat antal klientappar i jokertecken har uppdaterat toohello senaste versionen, ta bort hello ursprungliga migrerade appen om du vill ha.</span><span class="sxs-lookup"><span data-stu-id="eb93d-129">Once you feel that a desired number of client apps in the wild have updated toohello latest version, you can delete hello original migrated app if you desire.</span></span> <span data-ttu-id="eb93d-130">Den innebära inte någon ytterligare monetära kostnader, om finns i samma Apptjänst planera som din Mobilapp hello.</span><span class="sxs-lookup"><span data-stu-id="eb93d-130">It doesn't incur any additional monetary costs, if hosted in hello same App Service plan as your Mobile App.</span></span>

<span data-ttu-id="eb93d-131">hello fullständig disposition för hello uppgraderingsprocessen är följande:</span><span class="sxs-lookup"><span data-stu-id="eb93d-131">hello full outline for hello upgrade process is as follows:</span></span>

1. <span data-ttu-id="eb93d-132">Hämta din befintliga (migrerade) Azure-mobiltjänst.</span><span class="sxs-lookup"><span data-stu-id="eb93d-132">Download your existing (migrated) Azure Mobile Service.</span></span>
2. <span data-ttu-id="eb93d-133">Konvertera hello projektet tooan Azure Mobile App använder hello kompatibilitet paketet.</span><span class="sxs-lookup"><span data-stu-id="eb93d-133">Convert hello project tooan Azure Mobile App using hello compatibility package.</span></span>
3. <span data-ttu-id="eb93d-134">Åtgärda eventuella skillnader (t.ex inställningar för autentisering).</span><span class="sxs-lookup"><span data-stu-id="eb93d-134">Correct any differences (such as authentication settings).</span></span>
4. <span data-ttu-id="eb93d-135">Distribuera din konverterade Azure Mobile App-projekt tooa nya App Service.</span><span class="sxs-lookup"><span data-stu-id="eb93d-135">Deploy your converted Azure Mobile App project tooa new App Service.</span></span>
5. <span data-ttu-id="eb93d-136">Släpp en ny version av ditt klientprogram att använda hello nya Mobilapp.</span><span class="sxs-lookup"><span data-stu-id="eb93d-136">Release a new version of your client application that use hello new Mobile App.</span></span>
6. <span data-ttu-id="eb93d-137">(Valfritt) Ta bort appen för migrerade mobiltjänst.</span><span class="sxs-lookup"><span data-stu-id="eb93d-137">(Optional) Delete your original migrated mobile service app.</span></span>

<span data-ttu-id="eb93d-138">Borttagningen kan inträffa när du inte ser någon trafik på din ursprungliga migrerade mobiltjänst.</span><span class="sxs-lookup"><span data-stu-id="eb93d-138">Deletion can occur when you don't see any traffic on your original migrated mobile service.</span></span>

## <span data-ttu-id="eb93d-139"><a name="install-npm-package"></a>Installera förutsättningar för hello</span><span class="sxs-lookup"><span data-stu-id="eb93d-139"><a name="install-npm-package"></a> Install hello Pre-requisites</span></span>
<span data-ttu-id="eb93d-140">Du bör installera [Node] på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="eb93d-140">You should install [Node] on your local machine.</span></span>  <span data-ttu-id="eb93d-141">Du bör också installera hello kompatibilitet paketet.</span><span class="sxs-lookup"><span data-stu-id="eb93d-141">You should also install hello compatibility package.</span></span>  <span data-ttu-id="eb93d-142">När noden har installerats kan du köra följande kommando från en ny cmd eller PowerShell-Kommandotolken hello:</span><span class="sxs-lookup"><span data-stu-id="eb93d-142">After Node is installed, you can run hello following command from a new cmd or PowerShell prompt:</span></span>

```npm i -g azure-mobile-apps-compatibility```

## <span data-ttu-id="eb93d-143"><a name="obtain-ams-scripts"></a>Hämta Azure Mobile Services-skript</span><span class="sxs-lookup"><span data-stu-id="eb93d-143"><a name="obtain-ams-scripts"></a> Obtain your Azure Mobile Services Scripts</span></span>
* <span data-ttu-id="eb93d-144">Logga in toohello [Azure Portal].</span><span class="sxs-lookup"><span data-stu-id="eb93d-144">Log in toohello [Azure Portal].</span></span>
* <span data-ttu-id="eb93d-145">Med hjälp av **alla resurser** eller **Apptjänster**, hitta Mobile Services-plats.</span><span class="sxs-lookup"><span data-stu-id="eb93d-145">Using **All Resources** or **App Services**, find your Mobile Services site.</span></span>
* <span data-ttu-id="eb93d-146">Inom hello plats klickar du på **verktyg** -> **Kudu** -> **Gå** tooopen hello Kudu plats.</span><span class="sxs-lookup"><span data-stu-id="eb93d-146">Within hello site, click on **Tools** -> **Kudu** -> **Go** tooopen hello Kudu site.</span></span>
* <span data-ttu-id="eb93d-147">Klicka på **Felsökningskonsolen** -> **PowerShell** tooopen hello Felsökningskonsolen.</span><span class="sxs-lookup"><span data-stu-id="eb93d-147">Click on **Debug Console** -> **PowerShell** tooopen hello Debug console.</span></span>
* <span data-ttu-id="eb93d-148">Navigera för`site/wwwroot/App_Data/config` genom att klicka på varje katalog i sin tur</span><span class="sxs-lookup"><span data-stu-id="eb93d-148">Navigate too`site/wwwroot/App_Data/config` by clicking on each directory in turn</span></span>
* <span data-ttu-id="eb93d-149">Klicka på hello download ikonen nästa toohello `scripts` directory.</span><span class="sxs-lookup"><span data-stu-id="eb93d-149">Click on hello download icon next toohello `scripts` directory.</span></span>

<span data-ttu-id="eb93d-150">Detta hämtar hello skript i ZIP-format.</span><span class="sxs-lookup"><span data-stu-id="eb93d-150">This will download hello scripts in ZIP format.</span></span>  <span data-ttu-id="eb93d-151">Skapa en ny katalog på den lokala datorn och packa upp hello `scripts.ZIP` filen i hello katalog.</span><span class="sxs-lookup"><span data-stu-id="eb93d-151">Create a new directory on your local machine and unpack hello `scripts.ZIP` file within hello directory.</span></span>  <span data-ttu-id="eb93d-152">Detta skapar en `scripts` directory.</span><span class="sxs-lookup"><span data-stu-id="eb93d-152">This will create a `scripts` directory.</span></span>

## <span data-ttu-id="eb93d-153"><a name="scaffold-app"></a>Autogenerera hello nya Azure Mobile Apps-serverdel</span><span class="sxs-lookup"><span data-stu-id="eb93d-153"><a name="scaffold-app"></a> Scaffold hello new Azure Mobile Apps backend</span></span>
<span data-ttu-id="eb93d-154">Kör följande kommando från hello katalog som innehåller katalogen för hello skript hello:</span><span class="sxs-lookup"><span data-stu-id="eb93d-154">Run hello following command from hello directory containing hello scripts directory:</span></span>

```scaffold-mobile-app scripts out```

<span data-ttu-id="eb93d-155">Detta skapar en autogenererade Azure Mobile Apps-serverdel i hello `out` directory.</span><span class="sxs-lookup"><span data-stu-id="eb93d-155">This will create a scaffolded Azure Mobile Apps backend in hello `out` directory.</span></span>  <span data-ttu-id="eb93d-156">Även om de inte krävs, är det en bra idé toocheck hello `out` katalogen i en databas med källan kod du väljer.</span><span class="sxs-lookup"><span data-stu-id="eb93d-156">Although not required, it's a good idea toocheck hello `out` directory into a source code repository of your choice.</span></span>

## <span data-ttu-id="eb93d-157"><a name="deploy-ama-app"></a>Distribuera Azure Mobile Apps-serverdel</span><span class="sxs-lookup"><span data-stu-id="eb93d-157"><a name="deploy-ama-app"></a> Deploy your Azure Mobile Apps backend</span></span>
<span data-ttu-id="eb93d-158">Under distributionen behöver du toodo hello följande:</span><span class="sxs-lookup"><span data-stu-id="eb93d-158">During deployment, you will need toodo hello following:</span></span>

1. <span data-ttu-id="eb93d-159">Skapa en ny Mobile App i hello [Azure Portal].</span><span class="sxs-lookup"><span data-stu-id="eb93d-159">Create a new Mobile App in hello [Azure Portal].</span></span>
2. <span data-ttu-id="eb93d-160">Kör hello `createViews.sql` skript på den anslutna databasen.</span><span class="sxs-lookup"><span data-stu-id="eb93d-160">Run hello `createViews.sql` script on your connected database.</span></span>
3. <span data-ttu-id="eb93d-161">Länken hello-databas som är länkade tooyour Mobiltjänst tooyour nya App Service.</span><span class="sxs-lookup"><span data-stu-id="eb93d-161">Link hello database that is linked tooyour Mobile Service tooyour new App Service.</span></span>
4. <span data-ttu-id="eb93d-162">Länka andra resurser (till exempel Meddelandehubbar)-toohello nya App Service.</span><span class="sxs-lookup"><span data-stu-id="eb93d-162">Link any other resources (such as Notification Hubs) toohello new App Service.</span></span>
5. <span data-ttu-id="eb93d-163">Distribuera hello genererade koden tooyour ny plats.</span><span class="sxs-lookup"><span data-stu-id="eb93d-163">Deploy hello generated code tooyour new site.</span></span>

### <a name="create-a-new-mobile-app"></a><span data-ttu-id="eb93d-164">Skapa en ny Mobile App</span><span class="sxs-lookup"><span data-stu-id="eb93d-164">Create a new Mobile App</span></span>
1. <span data-ttu-id="eb93d-165">Logga in på hello [Azure Portal].</span><span class="sxs-lookup"><span data-stu-id="eb93d-165">Log in at hello [Azure Portal].</span></span>
2. <span data-ttu-id="eb93d-166">Klicka på **+NY** > **Webb + Mobil** > **Mobilapp** och ange sedan ett namn för din serverdel för mobilappen.</span><span class="sxs-lookup"><span data-stu-id="eb93d-166">Click **+NEW** > **Web + Mobile** > **Mobile App**, then provide a name for your Mobile App backend.</span></span>
3. <span data-ttu-id="eb93d-167">För hello **resursgruppen**, Välj en befintlig resursgrupp eller skapa en ny (med hjälp av hello samma namn som din app.)</span><span class="sxs-lookup"><span data-stu-id="eb93d-167">For hello **Resource Group**, select an existing resource group, or create a new one (using hello same name as your app.)</span></span>

    <span data-ttu-id="eb93d-168">Du kan antingen välja en annan App Service-plan eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="eb93d-168">You can either select another App Service plan or create a new one.</span></span> <span data-ttu-id="eb93d-169">Mer information om App Service-planer och hur toocreate en ny plan i en annan prisnivå på datanivå och på din önskade plats finns [Azure App Service-planer djupgående översikt över](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span><span class="sxs-lookup"><span data-stu-id="eb93d-169">For more about App Services plans and how toocreate a new plan in a different pricing tier and in your desired location, see [Azure App Service plans in-depth overview](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span></span>
4. <span data-ttu-id="eb93d-170">För hello **programtjänstplanen**, hello standardplanen (i hello [standardnivån](https://azure.microsoft.com/pricing/details/app-service/)) är markerad.</span><span class="sxs-lookup"><span data-stu-id="eb93d-170">For hello **App Service plan**, hello default plan (in hello [Standard tier](https://azure.microsoft.com/pricing/details/app-service/)) is selected.</span></span> <span data-ttu-id="eb93d-171">Du kan också välja en annan plan eller [skapa en ny](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md#create-an-app-service-plan).</span><span class="sxs-lookup"><span data-stu-id="eb93d-171">You can also  select a different plan, or [create a new one](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md#create-an-app-service-plan).</span></span> <span data-ttu-id="eb93d-172">hello App Service-planens inställningar avgör hello [plats, funktioner, kostnad och beräkningsresurser](https://azure.microsoft.com/pricing/details/app-service/) som är associerade med din app.</span><span class="sxs-lookup"><span data-stu-id="eb93d-172">hello App Service plan's settings determine hello [location, features, cost and compute resources](https://azure.microsoft.com/pricing/details/app-service/) associated with your app.</span></span>

    <span data-ttu-id="eb93d-173">När du bestämmer dig för hello planen klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="eb93d-173">After you decide on hello plan, click **Create**.</span></span> <span data-ttu-id="eb93d-174">Detta skapar hello mobilappsserverdel.</span><span class="sxs-lookup"><span data-stu-id="eb93d-174">This creates hello Mobile App backend.</span></span>

### <a name="run-createviewssql"></a><span data-ttu-id="eb93d-175">Kör CreateViews.SQL</span><span class="sxs-lookup"><span data-stu-id="eb93d-175">Run CreateViews.SQL</span></span>
<span data-ttu-id="eb93d-176">hello autogenererade appen innehåller en fil med namnet `createViews.sql`.</span><span class="sxs-lookup"><span data-stu-id="eb93d-176">hello scaffolded app contains a file called `createViews.sql`.</span></span>  <span data-ttu-id="eb93d-177">Det här skriptet måste köras mot måldatabasen.</span><span class="sxs-lookup"><span data-stu-id="eb93d-177">This script must be executed against the target database.</span></span>  <span data-ttu-id="eb93d-178">hello anslutningssträngen för hello måldatabasen kan hämtas från mobiltjänsten migrerade från hello **inställningar** bladet under **anslutningssträngar**.</span><span class="sxs-lookup"><span data-stu-id="eb93d-178">hello connection string for hello target database can be obtained from your migrated mobile service from hello **Settings** blade under **Connection Strings**.</span></span>  <span data-ttu-id="eb93d-179">Det heter `MS_TableConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="eb93d-179">It is named `MS_TableConnectionString`.</span></span>

<span data-ttu-id="eb93d-180">Du kan köra skriptet i SQL Server Management Studio eller Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="eb93d-180">You can run this script from within SQL Server Management Studio or Visual Studio.</span></span>

### <a name="link-hello-database-tooyour-app-service"></a><span data-ttu-id="eb93d-181">Länka hello databasen tooyour Apptjänst</span><span class="sxs-lookup"><span data-stu-id="eb93d-181">Link hello Database tooyour App Service</span></span>
<span data-ttu-id="eb93d-182">Länka hello befintlig databas tooyour Apptjänst:</span><span class="sxs-lookup"><span data-stu-id="eb93d-182">Link hello existing database tooyour App Service:</span></span>

* <span data-ttu-id="eb93d-183">I hello [Azure Portal], öppna din Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="eb93d-183">In hello [Azure Portal], open your App Service.</span></span>
* <span data-ttu-id="eb93d-184">Välj **alla inställningar** -> **dataanslutningar**.</span><span class="sxs-lookup"><span data-stu-id="eb93d-184">Select **All Settings** -> **Data Connections**.</span></span>
* <span data-ttu-id="eb93d-185">Klicka på **+ Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="eb93d-185">Click on **+ Add**.</span></span>
* <span data-ttu-id="eb93d-186">I hello listrutan, Välj **SQL-databas**</span><span class="sxs-lookup"><span data-stu-id="eb93d-186">In hello drop-down, select **SQL Database**</span></span>
* <span data-ttu-id="eb93d-187">Under **SQL-databas**, Välj den befintliga databasen, och klicka sedan på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="eb93d-187">Under **SQL Database**, select your existing database, then click on **Select**.</span></span>
* <span data-ttu-id="eb93d-188">Under **anslutningssträngen**, ange hello användarnamn och lösenord för hello-databasen, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="eb93d-188">Under **Connection string**, enter hello username and password for hello database, then click on **OK**.</span></span>
* <span data-ttu-id="eb93d-189">I hello **lägga till dataanslutningar** bladet, klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="eb93d-189">In hello **Add data connections** blade, click on **OK**.</span></span>

<span data-ttu-id="eb93d-190">hello användarnamn och lösenord finns genom att visa hello anslutningssträngen för hello måldatabasen i din migrerade mobiltjänst.</span><span class="sxs-lookup"><span data-stu-id="eb93d-190">hello username and password can be found by viewing hello Connection String for hello target database in your migrated Mobile Service.</span></span>

### <a name="set-up-authentication"></a><span data-ttu-id="eb93d-191">Konfigurera autentisering</span><span class="sxs-lookup"><span data-stu-id="eb93d-191">Set up authentication</span></span>
<span data-ttu-id="eb93d-192">Azure Mobile Apps kan du tooconfigure Azure Active Directory, Facebook, Google, Microsoft och Twitter autentisering inom hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="eb93d-192">Azure Mobile Apps allows you tooconfigure Azure Active Directory, Facebook, Google, Microsoft and Twitter authentication within hello service.</span></span>  <span data-ttu-id="eb93d-193">Anpassad autentisering måste toobe utvecklats separat.</span><span class="sxs-lookup"><span data-stu-id="eb93d-193">Custom authentication will need toobe developed separately.</span></span>  <span data-ttu-id="eb93d-194">Referera till hello [Autentiseringskoncept] dokumentation och [autentisering Quickstart] dokumentationen för mer information.</span><span class="sxs-lookup"><span data-stu-id="eb93d-194">Refer to hello [Authentication Concepts] documentation and [Authentication Quickstart] documentation for more information.</span></span>  

## <span data-ttu-id="eb93d-195"><a name="updating-clients"></a>Uppdatera mobila klienter</span><span class="sxs-lookup"><span data-stu-id="eb93d-195"><a name="updating-clients"></a>Update Mobile clients</span></span>
<span data-ttu-id="eb93d-196">När du har en operativa mobilappsserverdel kan arbeta du med en ny version av ditt klientprogram som använder den.</span><span class="sxs-lookup"><span data-stu-id="eb93d-196">Once you have an operational Mobile App backend, you can work on a new version of your client application which consumes it.</span></span> <span data-ttu-id="eb93d-197">Mobile Apps innehåller också en ny version av hello klienten SDK: er och liknande toohello serveruppgraderingen ovan, behöver du tooremove alla referenser toohello Mobile Services SDK innan du installerar Mobile Apps-versioner.</span><span class="sxs-lookup"><span data-stu-id="eb93d-197">Mobile Apps also includes a new version of hello client SDKs, and similar toohello server upgrade above, you will need tooremove all references toohello Mobile Services SDKs before installing the Mobile Apps versions.</span></span>

<span data-ttu-id="eb93d-198">En av hello huvudsakliga ändringar mellan hello versioner är att hello konstruktorer inte längre behöver en tangent.</span><span class="sxs-lookup"><span data-stu-id="eb93d-198">One of hello main changes between hello versions is that hello constructors no longer require an application key.</span></span>
<span data-ttu-id="eb93d-199">Du nu skickar hello URL för din Mobilapp.</span><span class="sxs-lookup"><span data-stu-id="eb93d-199">You now simply pass in hello URL of your Mobile App.</span></span> <span data-ttu-id="eb93d-200">Till exempel på hello .NET-klienter, hello `MobileServiceClient` konstruktorn är nu:</span><span class="sxs-lookup"><span data-stu-id="eb93d-200">For example, on hello .NET clients, hello `MobileServiceClient` constructor is now:</span></span>

        public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://contoso.azurewebsites.net" // URL of hello Mobile App
        );

<span data-ttu-id="eb93d-201">Du kan läsa om hur du installerar hello nya SDK: er och använder hello ny struktur via hello länkarna nedan:</span><span class="sxs-lookup"><span data-stu-id="eb93d-201">You can read about installing hello new SDKs and using hello new structure via hello links below:</span></span>

* [<span data-ttu-id="eb93d-202">Android-versionen 2.2 eller senare</span><span class="sxs-lookup"><span data-stu-id="eb93d-202">Android version 2.2 or later</span></span>](app-service-mobile-android-how-to-use-client-library.md)
* [<span data-ttu-id="eb93d-203">iOS version 3.0.0 eller senare</span><span class="sxs-lookup"><span data-stu-id="eb93d-203">iOS version 3.0.0 or later</span></span>](app-service-mobile-ios-how-to-use-client-library.md)
* [<span data-ttu-id="eb93d-204">.NET (Windows/Xamarin) version 2.0.0 eller senare</span><span class="sxs-lookup"><span data-stu-id="eb93d-204">.NET (Windows/Xamarin) version 2.0.0 or later</span></span>](app-service-mobile-dotnet-how-to-use-client-library.md)
* [<span data-ttu-id="eb93d-205">Apache Cordova version 2.0 eller senare</span><span class="sxs-lookup"><span data-stu-id="eb93d-205">Apache Cordova version 2.0 or later</span></span>](app-service-mobile-cordova-how-to-use-client-library.md)

<span data-ttu-id="eb93d-206">Om ditt program gör användning av push-meddelanden, anteckna hello registreringen instruktioner för varje plattform, har eftersom det ändringar det också.</span><span class="sxs-lookup"><span data-stu-id="eb93d-206">If your application makes use of push notifications, make note of hello specific registration instructions for each platform, as there have been some changes there as well.</span></span>

<span data-ttu-id="eb93d-207">När du har hello nya klientversionen redo prova mot projektet uppgraderade servern.</span><span class="sxs-lookup"><span data-stu-id="eb93d-207">When you have hello new client version ready, try it out against your upgraded server project.</span></span> <span data-ttu-id="eb93d-208">Du kan släppa en ny version av programmet-toocustomers efter verifierar att den fungerar.</span><span class="sxs-lookup"><span data-stu-id="eb93d-208">After validating that it works, you can release a new version of your application toocustomers.</span></span> <span data-ttu-id="eb93d-209">Slutligen, när dina kunder har fått en chans tooreceive dessa uppdateringar, du kan ta bort hello Mobile Services version av appen.</span><span class="sxs-lookup"><span data-stu-id="eb93d-209">Eventually, once your customers have had a chance tooreceive these updates, you can delete hello Mobile Services version of your app.</span></span> <span data-ttu-id="eb93d-210">Nu är du har uppgraderat tooan App Service-Mobilapp med hjälp av hello senaste Mobile Apps server-SDK.</span><span class="sxs-lookup"><span data-stu-id="eb93d-210">At this point, you have completely upgraded tooan App Service Mobile App using hello latest Mobile Apps server SDK.</span></span>

<!-- URLs. -->

[Azure Portal]: https://portal.azure.com/
[Azure classic portal]: https://manage.windowsazure.com/
[Vad är Mobile Apps?]: app-service-mobile-value-prop.md
[I already use web sites and mobile services – how does App Service help me?]: /en-us/documentation/articles/app-service-mobile-value-prop-migration-from-mobile-services
[Mobile App Server SDK]: https://www.npmjs.com/package/azure-mobile-apps
[Create a Mobile App]: app-service-mobile-xamarin-ios-get-started.md
[Add push notifications tooyour mobile app]: app-service-mobile-xamarin-ios-get-started-push.md
[Add authentication tooyour mobile app]: app-service-mobile-xamarin-ios-get-started-users.md
[Azure Scheduler]: /en-us/documentation/services/scheduler/
[Web Job]: ../app-service-web/websites-webjobs-resources.md
[How toouse hello .NET server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Migrate from Mobile Services tooan App Service Mobile App]: app-service-mobile-migrating-from-mobile-services.md
[Migrate your existing Mobile Service tooApp Service]: app-service-mobile-migrating-from-mobile-services.md
[Priser för Apptjänst]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[.NET server SDK overview]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Autentiseringskoncept]: ../app-service/app-service-authentication-overview.md
[Snabbstart för autentisering]: app-service-mobile-auth.md

[Azure Portal]: https://portal.azure.com/
[OData]: http://www.odata.org
[Promise]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[basicapp sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[todo sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[samples directory on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[Node.js Tools 1.1 for Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[mssql Node.js package]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS Middleware]: http://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston
