---
title: "aaaGet igång med Azure Cloud Services och ASP.NET | Microsoft Docs"
description: "Lär dig hur toocreate en flernivåapp med ASP.NET MVC och Azure. hello appen körs i en molntjänst med webbroll och en arbetsroll. Appen använder Entity Framework, SQL Database och Azure Storage-köer och -blobbar."
services: cloud-services, storage
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: d7aa440d-af4a-4f80-b804-cc46178df4f9
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 05/15/2017
ms.author: adegeo
ms.openlocfilehash: 86271c29b79fad3f01f8ea0e88fd00c7aefc970c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-cloud-services-and-aspnet"></a><span data-ttu-id="3802b-105">Kom igång med Azure Cloud Services och ASP.NET</span><span class="sxs-lookup"><span data-stu-id="3802b-105">Get started with Azure Cloud Services and ASP.NET</span></span>

## <a name="overview"></a><span data-ttu-id="3802b-106">Översikt</span><span class="sxs-lookup"><span data-stu-id="3802b-106">Overview</span></span>
<span data-ttu-id="3802b-107">Den här kursen visar hur toocreate en .NET-flernivåapp med en ASP.NET MVC frontend, och distribuerar den tooan [Azure-molntjänst](cloud-services-choose-me.md).</span><span class="sxs-lookup"><span data-stu-id="3802b-107">This tutorial shows how toocreate a multi-tier .NET application with an ASP.NET MVC front-end, and deploy it tooan [Azure cloud service](cloud-services-choose-me.md).</span></span> <span data-ttu-id="3802b-108">Hej program använder [Azure SQL Database](http://msdn.microsoft.com/library/azure/ee336279), hello [Azure Blob-tjänsten](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage), och hello [Azure-kötjänsten](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern).</span><span class="sxs-lookup"><span data-stu-id="3802b-108">hello application uses [Azure SQL Database](http://msdn.microsoft.com/library/azure/ee336279), hello [Azure Blob service](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage), and hello [Azure Queue service](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern).</span></span> <span data-ttu-id="3802b-109">Du kan [hämta hello Visual Studio-projekt](http://code.msdn.microsoft.com/Simple-Azure-Cloud-Service-e01df2e4) från hello MSDN Code Gallery.</span><span class="sxs-lookup"><span data-stu-id="3802b-109">You can [download hello Visual Studio project](http://code.msdn.microsoft.com/Simple-Azure-Cloud-Service-e01df2e4) from hello MSDN Code Gallery.</span></span>

<span data-ttu-id="3802b-110">hello kursen visar hur toobuild och kör hello programmet lokalt, hur toodeploy den tooAzure och kör i hello molnet, och hur toobuild det från grunden.</span><span class="sxs-lookup"><span data-stu-id="3802b-110">hello tutorial shows you how toobuild and run hello application locally, how toodeploy it tooAzure and run in hello cloud, and how toobuild it from scratch.</span></span> <span data-ttu-id="3802b-111">Du kan börja med att skapa från grunden och sedan hello test och distributionsstegen efteråt om du föredrar.</span><span class="sxs-lookup"><span data-stu-id="3802b-111">You can start by building from scratch and then do hello test and deploy steps afterward if you prefer.</span></span>

## <a name="contoso-ads-application"></a><span data-ttu-id="3802b-112">Contoso Ads-program</span><span class="sxs-lookup"><span data-stu-id="3802b-112">Contoso Ads application</span></span>
<span data-ttu-id="3802b-113">hello program är en anslagstavla för annonser.</span><span class="sxs-lookup"><span data-stu-id="3802b-113">hello application is an advertising bulletin board.</span></span> <span data-ttu-id="3802b-114">Användare skapar en annons genom att skriva in text och ladda upp en bild.</span><span class="sxs-lookup"><span data-stu-id="3802b-114">Users create an ad by entering text and uploading an image.</span></span> <span data-ttu-id="3802b-115">De kan se en lista över annonser med miniatyrbilder och de kan se hello bilden i full storlek när de väljer ett ad toosee hello information.</span><span class="sxs-lookup"><span data-stu-id="3802b-115">They can see a list of ads with thumbnail images, and they can see hello full-size image when they select an ad toosee hello details.</span></span>

![Annonslista](./media/cloud-services-dotnet-get-started/list.png)

<span data-ttu-id="3802b-117">hello programmet använder hello [köcentriska mönster](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern) toooff belastningen hello processorintensiva arbetet med att skapa miniatyrbilder tooa backend-processen.</span><span class="sxs-lookup"><span data-stu-id="3802b-117">hello application uses hello [queue-centric work pattern](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern) toooff-load hello CPU-intensive work of creating thumbnails tooa back-end process.</span></span>

## <a name="alternative-architecture-websites-and-webjobs"></a><span data-ttu-id="3802b-118">Alternativ arkitektur: Websites och WebJobs</span><span class="sxs-lookup"><span data-stu-id="3802b-118">Alternative architecture: Websites and WebJobs</span></span>
<span data-ttu-id="3802b-119">Den här kursen visar hur toorun både frontend och backend i ett Azure cloud service.</span><span class="sxs-lookup"><span data-stu-id="3802b-119">This tutorial shows how toorun both front-end and back-end in an Azure cloud service.</span></span> <span data-ttu-id="3802b-120">Ett alternativ är toorun hello klientdelen i en [Azure-webbplatsen](/services/web-sites/) och använda hello [WebJobs](http://go.microsoft.com/fwlink/?LinkId=390226) funktionen (för närvarande under förhandsgranskning) för hello serverdel.</span><span class="sxs-lookup"><span data-stu-id="3802b-120">An alternative is toorun hello front-end in an [Azure website](/services/web-sites/) and use hello [WebJobs](http://go.microsoft.com/fwlink/?LinkId=390226) feature (currently in preview) for hello back-end.</span></span> <span data-ttu-id="3802b-121">En självstudiekurs som använder WebJobs finns [Kom igång med hello Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="3802b-121">For a tutorial that uses WebJobs, see [Get Started with hello Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-get-started.md).</span></span> <span data-ttu-id="3802b-122">För information om hur toochoose hello tjänster som bäst passar din situation, se [jämförelse mellan Azure Websites, Cloud Services och virtuella datorer](../app-service-web/choose-web-site-cloud-service-vm.md).</span><span class="sxs-lookup"><span data-stu-id="3802b-122">For information about how toochoose hello services that best fit your scenario, see [Azure Websites, Cloud Services, and virtual machines comparison](../app-service-web/choose-web-site-cloud-service-vm.md).</span></span>

## <a name="what-youll-learn"></a><span data-ttu-id="3802b-123">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="3802b-123">What you'll learn</span></span>
* <span data-ttu-id="3802b-124">Hur tooenable datorn för Azure-utveckling genom att installera hello Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="3802b-124">How tooenable your machine for Azure development by installing hello Azure SDK.</span></span>
* <span data-ttu-id="3802b-125">Hur toocreate Visual Studio cloud service-projekt med en ASP.NET MVC-webbroll och en arbetsroll.</span><span class="sxs-lookup"><span data-stu-id="3802b-125">How toocreate a Visual Studio cloud service project with an ASP.NET MVC web role and a worker role.</span></span>
* <span data-ttu-id="3802b-126">Hur hello tootest molntjänstprojektet lokalt med hjälp av hello Azure storage-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="3802b-126">How tootest hello cloud service project locally, using hello Azure storage emulator.</span></span>
* <span data-ttu-id="3802b-127">Hur toopublish hello molnet projektet tooan Azure cloud service och testa med Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="3802b-127">How toopublish hello cloud project tooan Azure cloud service and test using an Azure storage account.</span></span>
* <span data-ttu-id="3802b-128">Hur tooupload filer och lagrar dem i hello Azure Blob-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3802b-128">How tooupload files and store them in hello Azure Blob service.</span></span>
* <span data-ttu-id="3802b-129">Hur toouse hello Azure-Kötjänsten för kommunikation mellan nivåerna.</span><span class="sxs-lookup"><span data-stu-id="3802b-129">How toouse hello Azure Queue service for communication between tiers.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3802b-130">Krav</span><span class="sxs-lookup"><span data-stu-id="3802b-130">Prerequisites</span></span>
<span data-ttu-id="3802b-131">hello kursen förutsätter att du förstår [grundläggande koncept om Azure-molntjänster](cloud-services-choose-me.md) som *webbroll* och *arbetsrollen* terminologi.</span><span class="sxs-lookup"><span data-stu-id="3802b-131">hello tutorial assumes that you understand [basic concepts about Azure cloud services](cloud-services-choose-me.md) such as *web role* and *worker role* terminology.</span></span>  <span data-ttu-id="3802b-132">Det förutsätts även att du vet hur toowork med [ASP.NET MVC](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started) eller [Web Forms](http://www.asp.net/web-forms/tutorials/aspnet-45/getting-started-with-aspnet-45-web-forms/introduction-and-overview) projekt i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3802b-132">It also assumes that you know how toowork with [ASP.NET MVC](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started) or [Web Forms](http://www.asp.net/web-forms/tutorials/aspnet-45/getting-started-with-aspnet-45-web-forms/introduction-and-overview) projects in Visual Studio.</span></span> <span data-ttu-id="3802b-133">hello exempelprogrammet använder MVC, men de flesta av hello kursen gäller också tooWeb formulär.</span><span class="sxs-lookup"><span data-stu-id="3802b-133">hello sample application uses MVC, but most of hello tutorial also applies tooWeb Forms.</span></span>

<span data-ttu-id="3802b-134">Du kan köra hello appen lokalt utan en Azure-prenumeration, men du behöver ett toodeploy hello programmet toohello moln.</span><span class="sxs-lookup"><span data-stu-id="3802b-134">You can run hello app locally without an Azure subscription, but you'll need one toodeploy hello application toohello cloud.</span></span> <span data-ttu-id="3802b-135">Om du inte har ett konto kan du [aktivera MSDN-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A55E3C668) eller [registrera dig för en kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A55E3C668).</span><span class="sxs-lookup"><span data-stu-id="3802b-135">If you don't have an account, you can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A55E3C668) or [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A55E3C668).</span></span>

<span data-ttu-id="3802b-136">hello självstudiekursen instruktioner fungerar med antingen hello följande produkter:</span><span class="sxs-lookup"><span data-stu-id="3802b-136">hello tutorial instructions work with either of hello following products:</span></span>

* <span data-ttu-id="3802b-137">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="3802b-137">Visual Studio 2013</span></span>
* <span data-ttu-id="3802b-138">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="3802b-138">Visual Studio 2015</span></span>
* <span data-ttu-id="3802b-139">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="3802b-139">Visual Studio 2017</span></span>

<span data-ttu-id="3802b-140">Om du inte har någon av dessa, kan Visual Studio installeras automatiskt när du installerar hello Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="3802b-140">If you don't have one of these, Visual Studio may be installed automatically when you install hello Azure SDK.</span></span>

## <a name="application-architecture"></a><span data-ttu-id="3802b-141">Programarkitektur</span><span class="sxs-lookup"><span data-stu-id="3802b-141">Application architecture</span></span>
<span data-ttu-id="3802b-142">hello appen lagrar annonser i en SQL-databas med hjälp av Entity Framework Code First toocreate hello tabellerna och komma åt hello data.</span><span class="sxs-lookup"><span data-stu-id="3802b-142">hello app stores ads in a SQL database, using Entity Framework Code First toocreate hello tables and access hello data.</span></span> <span data-ttu-id="3802b-143">För varje ad hello hello databasen lagrar två URL: er, en för bilden och en för hello miniatyr.</span><span class="sxs-lookup"><span data-stu-id="3802b-143">For each ad, hello database stores two URLs, one for hello full-size image and one for hello thumbnail.</span></span>

![Annonstabell](./media/cloud-services-dotnet-get-started/adtable.png)

<span data-ttu-id="3802b-145">När en användare laddar upp en bild, hello klientdelen som körs i en webbroll lagrar hello bilden i en [Azure blob](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage), och den lagrar Hej ad i hello-databas med en URL som pekar toohello blob.</span><span class="sxs-lookup"><span data-stu-id="3802b-145">When a user uploads an image, hello front-end running in a web role stores hello image in an [Azure blob](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage), and it stores hello ad information in hello database with a URL that points toohello blob.</span></span> <span data-ttu-id="3802b-146">AT hello samma time, skriver den ett meddelande tooan Azure kön.</span><span class="sxs-lookup"><span data-stu-id="3802b-146">At hello same time, it writes a message tooan Azure queue.</span></span> <span data-ttu-id="3802b-147">En serverdelsprocess som körs i en arbetsroll regelbundet avsöker hello kön efter nya meddelanden.</span><span class="sxs-lookup"><span data-stu-id="3802b-147">A back-end process running in a worker role periodically polls hello queue for new messages.</span></span> <span data-ttu-id="3802b-148">När ett nytt meddelande visas hello arbetsrollen skapar en miniatyrbild för den bilden och uppdateringar hello miniatyrbildens URL-databasfält för den annonsen.</span><span class="sxs-lookup"><span data-stu-id="3802b-148">When a new message appears, hello worker role creates a thumbnail for that image and updates hello thumbnail URL database field for that ad.</span></span> <span data-ttu-id="3802b-149">hello följande diagram visar hur hello delar av programmet hello samverkar.</span><span class="sxs-lookup"><span data-stu-id="3802b-149">hello following diagram shows how hello parts of hello application interact.</span></span>

![Contoso Ads-arkitektur](./media/cloud-services-dotnet-get-started/apparchitecture.png)

[!INCLUDE [install-sdk](../../includes/install-sdk-2017-2015-2013.md)]

## <a name="download-and-run-hello-completed-solution"></a><span data-ttu-id="3802b-151">Hämta och kör hello färdiga lösningen</span><span class="sxs-lookup"><span data-stu-id="3802b-151">Download and run hello completed solution</span></span>
1. <span data-ttu-id="3802b-152">Hämta och packa upp hello [färdiga lösningen](http://code.msdn.microsoft.com/Simple-Azure-Cloud-Service-e01df2e4).</span><span class="sxs-lookup"><span data-stu-id="3802b-152">Download and unzip hello [completed solution](http://code.msdn.microsoft.com/Simple-Azure-Cloud-Service-e01df2e4).</span></span>
2. <span data-ttu-id="3802b-153">Starta Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3802b-153">Start Visual Studio.</span></span>
3. <span data-ttu-id="3802b-154">Från hello **filen** väljer du menyn **Open Project**, navigera toowhere som du hämtade hello lösningen och öppna sedan lösningsfilen hello.</span><span class="sxs-lookup"><span data-stu-id="3802b-154">From hello **File** menu choose **Open Project**, navigate toowhere you downloaded hello solution, and then open hello solution file.</span></span>
4. <span data-ttu-id="3802b-155">Tryck på CTRL + SKIFT + B toobuild hello lösning.</span><span class="sxs-lookup"><span data-stu-id="3802b-155">Press CTRL+SHIFT+B toobuild hello solution.</span></span>

    <span data-ttu-id="3802b-156">Som standard återställer Visual Studio automatiskt hello NuGet-paketinnehållet som inte ingick i hello *.zip* fil.</span><span class="sxs-lookup"><span data-stu-id="3802b-156">By default, Visual Studio automatically restores hello NuGet package content, which was not included in hello *.zip* file.</span></span> <span data-ttu-id="3802b-157">Om inte Återställ hello paket installera dem manuellt genom att gå toohello **hantera NuGet-paket för lösningen** dialogrutan och klicka på hello **återställa** längst hello uppe till höger.</span><span class="sxs-lookup"><span data-stu-id="3802b-157">If hello packages don't restore, install them manually by going toohello **Manage NuGet Packages for Solution** dialog box and clicking hello **Restore** button at hello top right.</span></span>
5. <span data-ttu-id="3802b-158">I **Solution Explorer**, se till att **ContosoAdsCloudService** är markerad som hello Startprojekt.</span><span class="sxs-lookup"><span data-stu-id="3802b-158">In **Solution Explorer**, make sure that **ContosoAdsCloudService** is selected as hello startup project.</span></span>
6. <span data-ttu-id="3802b-159">Om du använder Visual Studio 2015 eller högre, ändrar hello SQL Server-anslutningssträngen i programmet hello *Web.config* filen hello ContosoAdsWeb-projektet och i hello *ServiceConfiguration.Local.cscfg* -filen för hello ContosoAdsCloudService-projektet.</span><span class="sxs-lookup"><span data-stu-id="3802b-159">If you're using Visual Studio 2015 or higher, change hello SQL Server connection string in hello application *Web.config* file of hello ContosoAdsWeb project and in hello *ServiceConfiguration.Local.cscfg* file of hello ContosoAdsCloudService project.</span></span> <span data-ttu-id="3802b-160">I båda fallen kan du ändra ”(localdb) \v11.0” för ”(localdb) \MSSQLLocalDB”.</span><span class="sxs-lookup"><span data-stu-id="3802b-160">In each case, change "(localdb)\v11.0" too"(localdb)\MSSQLLocalDB".</span></span>
7. <span data-ttu-id="3802b-161">Tryck på CTRL + F5 toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="3802b-161">Press CTRL+F5 toorun hello application.</span></span>

    <span data-ttu-id="3802b-162">När du kör ett molntjänstprojekt lokalt anropar Visual Studio automatiskt hello Azure *beräkningsemulatorn* och Azure *lagringsemulatorn*.</span><span class="sxs-lookup"><span data-stu-id="3802b-162">When you run a cloud service project locally, Visual Studio automatically invokes hello Azure *compute emulator* and Azure *storage emulator*.</span></span> <span data-ttu-id="3802b-163">hello compute emulator använder datorns resurser toosimulate hello webbroll och worker roll-miljöer.</span><span class="sxs-lookup"><span data-stu-id="3802b-163">hello compute emulator uses your computer's resources toosimulate hello web role and worker role environments.</span></span> <span data-ttu-id="3802b-164">hello storage-emulatorn använder en [SQL Server Express LocalDB](http://msdn.microsoft.com/library/hh510202.aspx) databasen toosimulate Azure molnlagring.</span><span class="sxs-lookup"><span data-stu-id="3802b-164">hello storage emulator uses a [SQL Server Express LocalDB](http://msdn.microsoft.com/library/hh510202.aspx) database toosimulate Azure cloud storage.</span></span>

    <span data-ttu-id="3802b-165">hello tar första gången du kör ett molntjänstprojekt en minut för hello emulatorerna toostart upp.</span><span class="sxs-lookup"><span data-stu-id="3802b-165">hello first time you run a cloud service project, it takes a minute or so for hello emulators toostart up.</span></span> <span data-ttu-id="3802b-166">När emulatorerna har startats öppnas standardwebbläsaren hello toohello programmets startsida.</span><span class="sxs-lookup"><span data-stu-id="3802b-166">When emulator startup is finished, hello default browser opens toohello application home page.</span></span>

    ![Contoso Ads-arkitektur](./media/cloud-services-dotnet-get-started/home.png)
8. <span data-ttu-id="3802b-168">Klicka på **Create an Ad** (Skapa en annons).</span><span class="sxs-lookup"><span data-stu-id="3802b-168">Click  **Create an Ad**.</span></span>
9. <span data-ttu-id="3802b-169">Ange lite testdata och välj en *.jpg* bild tooupload och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="3802b-169">Enter some test data and select a *.jpg* image tooupload, and then click **Create**.</span></span>

    ![Sidan Create (Skapa)](./media/cloud-services-dotnet-get-started/create.png)

    <span data-ttu-id="3802b-171">hello appen går toohello indexsidan, men den visar inte en miniatyrbild för hello nya annonsen eftersom den bearbetningen inte har utförts än.</span><span class="sxs-lookup"><span data-stu-id="3802b-171">hello app goes toohello Index page, but it doesn't show a thumbnail for hello new ad because that processing hasn't happened yet.</span></span>
10. <span data-ttu-id="3802b-172">Vänta en stund och uppdatera sedan hello Index toosee hello miniatyrbilden.</span><span class="sxs-lookup"><span data-stu-id="3802b-172">Wait a moment and then refresh hello Index page toosee hello thumbnail.</span></span>

     ![Sidan Index](./media/cloud-services-dotnet-get-started/list.png)
11. <span data-ttu-id="3802b-174">Klicka på **information** för bilden din ad toosee hello.</span><span class="sxs-lookup"><span data-stu-id="3802b-174">Click **Details** for your ad toosee hello full-size image.</span></span>

     ![Sidan Details (Detaljer)](./media/cloud-services-dotnet-get-started/details.png)

<span data-ttu-id="3802b-176">Du har kört programmet hello helt på din lokala dator utan anslutning toohello moln.</span><span class="sxs-lookup"><span data-stu-id="3802b-176">You've been running hello application entirely on your local computer, with no connection toohello cloud.</span></span> <span data-ttu-id="3802b-177">Hej lagringsemulatorn lagrar kö hello och blob-data i en SQL Server Express LocalDB-databas och hello programmet lagrar hello ad-data i en annan LocalDB-databas.</span><span class="sxs-lookup"><span data-stu-id="3802b-177">hello storage emulator stores hello queue and blob data in a SQL Server Express LocalDB database, and hello application stores hello ad data in another LocalDB database.</span></span> <span data-ttu-id="3802b-178">Entity Framework Code First automatiskt skapade hello ad-databasen hello första gången hello webbappen försökte tooaccess den.</span><span class="sxs-lookup"><span data-stu-id="3802b-178">Entity Framework Code First automatically created hello ad database hello first time hello web app tried tooaccess it.</span></span>

<span data-ttu-id="3802b-179">I följande avsnitt hello får du konfigurera hello lösning toouse Azure-molnresurser för köer, blobbar och programdatabasen hello när den körs i molnet hello.</span><span class="sxs-lookup"><span data-stu-id="3802b-179">In hello following section you'll configure hello solution toouse Azure cloud resources for queues, blobs, and hello application database when it runs in hello cloud.</span></span> <span data-ttu-id="3802b-180">Om du vill toocontinue toorun lokalt men använda molnet och databasresurser kan du göra det.</span><span class="sxs-lookup"><span data-stu-id="3802b-180">If you wanted toocontinue toorun locally but use cloud storage and database resources, you could do that.</span></span> <span data-ttu-id="3802b-181">Det är bara gäller att ställa in anslutningssträngar, som du ser hur toodo.</span><span class="sxs-lookup"><span data-stu-id="3802b-181">It's just a matter of setting connection strings, which you'll see how toodo.</span></span>

## <a name="deploy-hello-application-tooazure"></a><span data-ttu-id="3802b-182">Distribuera hello programmet tooAzure</span><span class="sxs-lookup"><span data-stu-id="3802b-182">Deploy hello application tooAzure</span></span>
<span data-ttu-id="3802b-183">Gör du hello följande steg toorun hello program i molnet hello:</span><span class="sxs-lookup"><span data-stu-id="3802b-183">You'll do hello following steps toorun hello application in hello cloud:</span></span>

* <span data-ttu-id="3802b-184">Skapa en Azure-molntjänst.</span><span class="sxs-lookup"><span data-stu-id="3802b-184">Create an Azure cloud service.</span></span>
* <span data-ttu-id="3802b-185">Skapa en Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="3802b-185">Create an Azure SQL database.</span></span>
* <span data-ttu-id="3802b-186">Skapa ett Azure-lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="3802b-186">Create an Azure storage account.</span></span>
* <span data-ttu-id="3802b-187">Konfigurera hello lösning toouse Azure SQL database när den körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="3802b-187">Configure hello solution toouse your Azure SQL database when it runs in Azure.</span></span>
* <span data-ttu-id="3802b-188">Konfigurera hello lösning toouse Azure storage-konto när den körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="3802b-188">Configure hello solution toouse your Azure storage account when it runs in Azure.</span></span>
* <span data-ttu-id="3802b-189">Distribuera hello projektet tooyour Azure-molntjänst.</span><span class="sxs-lookup"><span data-stu-id="3802b-189">Deploy hello project tooyour Azure cloud service.</span></span>

### <a name="create-an-azure-cloud-service"></a><span data-ttu-id="3802b-190">Skapa en Azure-molntjänst</span><span class="sxs-lookup"><span data-stu-id="3802b-190">Create an Azure cloud service</span></span>
<span data-ttu-id="3802b-191">En Azure-molntjänst är hello miljö hello programmet ska köras.</span><span class="sxs-lookup"><span data-stu-id="3802b-191">An Azure cloud service is hello environment hello application will run in.</span></span>

1. <span data-ttu-id="3802b-192">Öppna i din webbläsare hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3802b-192">In your browser, open hello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="3802b-193">Klicka på **New > Compute > Cloud Service (Ny > Compute > Molntjänst**.</span><span class="sxs-lookup"><span data-stu-id="3802b-193">Click **New > Compute > Cloud Service**.</span></span>

3. <span data-ttu-id="3802b-194">Ange ett URL-prefix för hello Molntjänsten i hello DNS-namn på indata.</span><span class="sxs-lookup"><span data-stu-id="3802b-194">In hello DNS name input box, enter a URL prefix for hello cloud service.</span></span>

    <span data-ttu-id="3802b-195">Den här Webbadressen har toobe unikt.</span><span class="sxs-lookup"><span data-stu-id="3802b-195">This URL has toobe unique.</span></span>  <span data-ttu-id="3802b-196">Du får ett felmeddelande om hello prefix som du har valt används redan.</span><span class="sxs-lookup"><span data-stu-id="3802b-196">You'll get an error message if hello prefix you choose is already in use.</span></span>
4. <span data-ttu-id="3802b-197">Ange en ny resursgrupp för hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3802b-197">Specify a new Resource group for hello  service.</span></span> <span data-ttu-id="3802b-198">Klicka på **Skapa nytt** och skriv sedan ett namn i hello resursen inkommande gruppruta, till exempel CS_contososadsRG.</span><span class="sxs-lookup"><span data-stu-id="3802b-198">Click **Create new** and then type a name in hello Resource group input box, such as CS_contososadsRG.</span></span>

5. <span data-ttu-id="3802b-199">Välj hello region där du vill att toodeploy hello.</span><span class="sxs-lookup"><span data-stu-id="3802b-199">Choose hello region where you want toodeploy hello application.</span></span>

    <span data-ttu-id="3802b-200">Det här fältet anger vilket datacenter som ska vara värd åt molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="3802b-200">This field specifies which datacenter your cloud service will be hosted in.</span></span> <span data-ttu-id="3802b-201">För ett produktionsprogram väljer du hello region närmaste tooyour kunder.</span><span class="sxs-lookup"><span data-stu-id="3802b-201">For a production application, you'd choose hello region closest tooyour customers.</span></span> <span data-ttu-id="3802b-202">Välj hello region närmaste tooyou för den här kursen.</span><span class="sxs-lookup"><span data-stu-id="3802b-202">For this tutorial, choose hello region closest tooyou.</span></span>
5. <span data-ttu-id="3802b-203">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="3802b-203">Click **Create**.</span></span>

    <span data-ttu-id="3802b-204">I följande bild hello, skapas en molntjänst med hello URL CSvccontosoads.cloudapp.net.</span><span class="sxs-lookup"><span data-stu-id="3802b-204">In hello following image, a cloud service is created with hello URL CSvccontosoads.cloudapp.net.</span></span>

    ![Ny molntjänst](./media/cloud-services-dotnet-get-started/newcs.png)

### <a name="create-an-azure-sql-database"></a><span data-ttu-id="3802b-206">Skapa en Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="3802b-206">Create an Azure SQL database</span></span>
<span data-ttu-id="3802b-207">Om hello app körs i molnet hello, använder en molnbaserad databas.</span><span class="sxs-lookup"><span data-stu-id="3802b-207">When hello app runs in hello cloud, it will use a cloud-based database.</span></span>

1. <span data-ttu-id="3802b-208">I hello [Azure-portalen](https://portal.azure.com), klickar du på **New > databaser > SQL Database**.</span><span class="sxs-lookup"><span data-stu-id="3802b-208">In hello [Azure portal](https://portal.azure.com), click **New > Databases > SQL Database**.</span></span>
2. <span data-ttu-id="3802b-209">I hello **databasnamnet** ange *contosoads*.</span><span class="sxs-lookup"><span data-stu-id="3802b-209">In hello **Database Name** box, enter *contosoads*.</span></span>
3. <span data-ttu-id="3802b-210">I hello **resursgruppen**, klickar du på **använda befintliga** och välj hello resursgruppen som används för hello-Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="3802b-210">In hello **Resource group**, click **Use existing** and select hello resource group used for hello cloud service.</span></span>
4. <span data-ttu-id="3802b-211">Hello följande bild, klicka på **Server – konfigurera nödvändiga inställningar** och **skapa en ny server**.</span><span class="sxs-lookup"><span data-stu-id="3802b-211">In hello following image, click **Server - Configure required settings** and **Create a new server**.</span></span>

    ![Tunnel toodatabase server](./media/cloud-services-dotnet-get-started/newdb.png)

    <span data-ttu-id="3802b-213">Alternativt, om din prenumeration redan har en server, kan du välja den servern hello nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="3802b-213">Alternatively, if your subscription already has a server, you can select that server from hello drop-down list.</span></span>
5. <span data-ttu-id="3802b-214">I hello **servernamn** ange *csvccontosodbserver*.</span><span class="sxs-lookup"><span data-stu-id="3802b-214">In hello **Server name** box, enter *csvccontosodbserver*.</span></span>

6. <span data-ttu-id="3802b-215">Ange ett **inloggningsnamn** och **lösenord** med administratörsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="3802b-215">Enter an administrator **Login Name** and **Password**.</span></span>

    <span data-ttu-id="3802b-216">Om du har valt **Skapa en ny server** anger du inte ett befintligt namn och lösenord här.</span><span class="sxs-lookup"><span data-stu-id="3802b-216">If you selected **Create a new server**, you aren't entering an existing name and password here.</span></span> <span data-ttu-id="3802b-217">Du har angett ett nytt namn och lösenord som du definierar nu toouse senare när du använder hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="3802b-217">You're entering a new name and password that you're defining now toouse later when you access hello database.</span></span> <span data-ttu-id="3802b-218">Om du valde en server som du skapade tidigare, uppmanas du ange hello lösenord toohello administrativt användarkonto du redan skapat.</span><span class="sxs-lookup"><span data-stu-id="3802b-218">If you selected a server that you created previously, you'll be prompted for hello password toohello administrative user account you already created.</span></span>
7. <span data-ttu-id="3802b-219">Välj hello samma **plats** som du valde för Molntjänsten hello.</span><span class="sxs-lookup"><span data-stu-id="3802b-219">Choose hello same **Location** that you chose for hello cloud service.</span></span>

    <span data-ttu-id="3802b-220">När hello Molntjänsten och databasen är i olika datacenter (olika regioner), ökar svarstiden och du kommer att debiteras för bandbredden utanför hello datacenter.</span><span class="sxs-lookup"><span data-stu-id="3802b-220">When hello cloud service and database are in different datacenters (different regions), latency will increase and you will be charged for bandwidth outside hello data center.</span></span> <span data-ttu-id="3802b-221">Bandbredd inom ett datacenter är kostnadsfri.</span><span class="sxs-lookup"><span data-stu-id="3802b-221">Bandwidth within a data center is free.</span></span>
8. <span data-ttu-id="3802b-222">Kontrollera **Tillåt azure-tjänster tooaccess server**.</span><span class="sxs-lookup"><span data-stu-id="3802b-222">Check **Allow azure services tooaccess server**.</span></span>
9. <span data-ttu-id="3802b-223">Klicka på **Välj** för nya hello-server.</span><span class="sxs-lookup"><span data-stu-id="3802b-223">Click **Select** for hello new server.</span></span>

    ![Ny SQL-databasserver](./media/cloud-services-dotnet-get-started/newdbserver.png)
10. <span data-ttu-id="3802b-225">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="3802b-225">Click **Create**.</span></span>

### <a name="create-an-azure-storage-account"></a><span data-ttu-id="3802b-226">Skapa ett Azure-lagringskonto</span><span class="sxs-lookup"><span data-stu-id="3802b-226">Create an Azure storage account</span></span>
<span data-ttu-id="3802b-227">Ett Azure storage-konto innehåller resurser för att lagra kö-och blobbdata i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="3802b-227">An Azure storage account provides resources for storing queue and blob data in hello cloud.</span></span>

<span data-ttu-id="3802b-228">I ett riktigt program skapar du vanligtvis separata konton för programdata jämfört med loggningsdata, samt separata konton för testdata jämfört med produktionsdata.</span><span class="sxs-lookup"><span data-stu-id="3802b-228">In a real-world application, you would typically create separate accounts for application data versus logging data, and separate accounts for test data versus production data.</span></span> <span data-ttu-id="3802b-229">Under den här kursen använder du bara ett konto.</span><span class="sxs-lookup"><span data-stu-id="3802b-229">For this tutorial, you'll use just one account.</span></span>

1. <span data-ttu-id="3802b-230">I hello [Azure-portalen](https://portal.azure.com), klickar du på **New > Storage > lagringskonto - blob, fil, tabell, kö**.</span><span class="sxs-lookup"><span data-stu-id="3802b-230">In hello [Azure portal](https://portal.azure.com), click **New > Storage > Storage account - blob, file, table, queue**.</span></span>
2. <span data-ttu-id="3802b-231">I hello **namn** ange ett URL-prefix.</span><span class="sxs-lookup"><span data-stu-id="3802b-231">In hello **Name** box, enter a URL prefix.</span></span>

    <span data-ttu-id="3802b-232">Det här prefixet och hello text som du ser under rutan hello blir hello unik URL tooyour storage-konto.</span><span class="sxs-lookup"><span data-stu-id="3802b-232">This prefix plus hello text you see under hello box will be hello unique URL tooyour storage account.</span></span> <span data-ttu-id="3802b-233">Om hello-prefix du anger har redan använts av någon annan, har du toochoose ett annat prefix.</span><span class="sxs-lookup"><span data-stu-id="3802b-233">If hello prefix you enter has already been used by someone else, you'll have toochoose a different prefix.</span></span>
3. <span data-ttu-id="3802b-234">Ange hello **distributionsmodellen** för*klassiska*.</span><span class="sxs-lookup"><span data-stu-id="3802b-234">Set hello **Deployment model** too*Classic*.</span></span>

4. <span data-ttu-id="3802b-235">Ange hello **replikering** nedrullningsbara listan för**lokalt redundant lagring**.</span><span class="sxs-lookup"><span data-stu-id="3802b-235">Set hello **Replication** drop-down list too**Locally redundant storage**.</span></span>

    <span data-ttu-id="3802b-236">När geo-replikering är aktiverat för ett lagringskonto, är hello lagras innehållet replikerade tooa sekundärt datacenter tooenable redundans om en större katastrof inträffar på hello primära platsen.</span><span class="sxs-lookup"><span data-stu-id="3802b-236">When geo-replication is enabled for a storage account, hello stored content is replicated tooa secondary datacenter tooenable failover if a major disaster occurs in hello primary location.</span></span> <span data-ttu-id="3802b-237">Geo-replikering kan medföra ytterligare kostnader.</span><span class="sxs-lookup"><span data-stu-id="3802b-237">Geo-replication can incur additional costs.</span></span> <span data-ttu-id="3802b-238">För testning och utveckling konton vill du normalt inte toopay för geo-replikering.</span><span class="sxs-lookup"><span data-stu-id="3802b-238">For test and development accounts, you generally don't want toopay for geo-replication.</span></span> <span data-ttu-id="3802b-239">Mer information finns i [Skapa, hantera eller ta bort ett lagringskonto](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="3802b-239">For more information, see [Create, manage, or delete a storage account](../storage/common/storage-create-storage-account.md).</span></span>

5. <span data-ttu-id="3802b-240">I hello **resursgruppen**, klickar du på **använda befintliga** och välj hello resursgruppen som används för hello-Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="3802b-240">In hello **Resource group**, click **Use existing** and select hello resource group used for hello cloud service.</span></span>
6. <span data-ttu-id="3802b-241">Ange hello **plats** listrutan toohello samma region som du valde för Molntjänsten hello.</span><span class="sxs-lookup"><span data-stu-id="3802b-241">Set hello **Location** drop-down list toohello same region you chose for hello cloud service.</span></span>

    <span data-ttu-id="3802b-242">När hello cloud service och lagringskontot tillhör i olika datacenter (olika regioner), ökar svarstiden och du kommer att debiteras för bandbredden utanför hello datacenter.</span><span class="sxs-lookup"><span data-stu-id="3802b-242">When hello cloud service and storage account are in different datacenters (different regions), latency will increase and you will be charged for bandwidth outside hello data center.</span></span> <span data-ttu-id="3802b-243">Bandbredd inom ett datacenter är kostnadsfri.</span><span class="sxs-lookup"><span data-stu-id="3802b-243">Bandwidth within a data center is free.</span></span>

    <span data-ttu-id="3802b-244">Azure-tillhörighetsgrupper tillhandahåller en mekanism toominimize hello avståndet mellan resurser i datacentret, vilket kan förkorta svarstiden.</span><span class="sxs-lookup"><span data-stu-id="3802b-244">Azure affinity groups provide a mechanism toominimize hello distance between resources in a data center, which can reduce latency.</span></span> <span data-ttu-id="3802b-245">Under den här kursen används inte tillhörighetsgrupper.</span><span class="sxs-lookup"><span data-stu-id="3802b-245">This tutorial does not use affinity groups.</span></span> <span data-ttu-id="3802b-246">Mer information finns i [hur tooCreate tillhörighet gruppen i Azure](http://msdn.microsoft.com/library/jj156209.aspx).</span><span class="sxs-lookup"><span data-stu-id="3802b-246">For more information, see [How tooCreate an Affinity Group in Azure](http://msdn.microsoft.com/library/jj156209.aspx).</span></span>
7. <span data-ttu-id="3802b-247">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="3802b-247">Click **Create**.</span></span>

    ![Nytt lagringskonto](./media/cloud-services-dotnet-get-started/newstorage.png)

    <span data-ttu-id="3802b-249">Hello bild skapas ett lagringskonto med hello URL `csvccontosoads.core.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="3802b-249">In hello image, a storage account is created with hello URL `csvccontosoads.core.windows.net`.</span></span>

### <a name="configure-hello-solution-toouse-your-azure-sql-database-when-it-runs-in-azure"></a><span data-ttu-id="3802b-250">Konfigurera hello lösning toouse Azure SQL database när den körs i Azure</span><span class="sxs-lookup"><span data-stu-id="3802b-250">Configure hello solution toouse your Azure SQL database when it runs in Azure</span></span>
<span data-ttu-id="3802b-251">hello webbprojekt hello arbetsrollsprojektet varje har sin egen databasanslutningssträng och varje måste toopoint toohello Azure SQL database när hello app körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="3802b-251">hello web project and hello worker role project each has its own database connection string, and each needs toopoint toohello Azure SQL database when hello app runs in Azure.</span></span>

<span data-ttu-id="3802b-252">Du använder en [Web.config-transformering](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/web-config-transformations) för hello-webbroll och en cloud service miljöinställning för hello worker-rollen.</span><span class="sxs-lookup"><span data-stu-id="3802b-252">You'll use a [Web.config transform](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/web-config-transformations) for hello web role and a cloud service environment setting for hello worker role.</span></span>

> [!NOTE]
> <span data-ttu-id="3802b-253">I det här avsnittet och hello nästa avsnitt kan lagra du autentiseringsuppgifter i projektfiler.</span><span class="sxs-lookup"><span data-stu-id="3802b-253">In this section and hello next section, you store credentials in project files.</span></span> <span data-ttu-id="3802b-254">[Lagra inte känsliga data i offentliga källkodslager](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control#secrets).</span><span class="sxs-lookup"><span data-stu-id="3802b-254">[Don't store sensitive data in public source code repositories](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control#secrets).</span></span>
>
>

1. <span data-ttu-id="3802b-255">Öppna i ContosoAdsWeb-projektet hello hello *Web.Release.config* transformationsfil för programmet hello *Web.config* fil, ta bort hello kommentarblocket som innehåller en `<connectionStrings>` element, och klistra in hello följande kod i dess ställe.</span><span class="sxs-lookup"><span data-stu-id="3802b-255">In hello ContosoAdsWeb project, open hello *Web.Release.config* transform file for hello application *Web.config* file, delete hello comment block that contains a `<connectionStrings>` element, and paste hello following code in its place.</span></span>

    ```xml
    <connectionStrings>
        <add name="ContosoAdsContext" connectionString="{connectionstring}"
        providerName="System.Data.SqlClient" xdt:Transform="SetAttributes" xdt:Locator="Match(name)"/>
    </connectionStrings>
    ```

    <span data-ttu-id="3802b-256">Lämna hello filen öppen för redigering.</span><span class="sxs-lookup"><span data-stu-id="3802b-256">Leave hello file open for editing.</span></span>
2. <span data-ttu-id="3802b-257">I hello [Azure-portalen](https://portal.azure.com), klickar du på **SQL-databaser** hello vänster klickar du på hello-databasen som du skapade för den här självstudiekursen och klicka sedan på **visa anslutningssträngar**.</span><span class="sxs-lookup"><span data-stu-id="3802b-257">In hello [Azure portal](https://portal.azure.com), click **SQL Databases** in hello left pane, click hello database you created for this tutorial, and then click **Show connection strings**.</span></span>

    ![Visa anslutningssträngar](./media/cloud-services-dotnet-get-started/showcs.png)

    <span data-ttu-id="3802b-259">hello portalen visar anslutningssträngar med en platshållare för hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="3802b-259">hello portal displays connection strings, with a placeholder for hello password.</span></span>

    ![Anslutningssträngar](./media/cloud-services-dotnet-get-started/connstrings.png)
3. <span data-ttu-id="3802b-261">I hello *Web.Release.config* transformeringsfilen, ta bort `{connectionstring}` och klistra in i dess ställe hello ADO.NET-anslutningssträngen från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3802b-261">In hello *Web.Release.config* transform file, delete `{connectionstring}` and paste in its place hello ADO.NET connection string from hello Azure portal.</span></span>
4. <span data-ttu-id="3802b-262">I hello anslutningssträngen som du klistrade in hello *Web.Release.config* transformeringsfilen ersätter `{your_password_here}` med hello lösenordet du skapade för hello ny SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="3802b-262">In hello connection string that you pasted into hello *Web.Release.config* transform file, replace `{your_password_here}` with hello password you created for hello new SQL database.</span></span>
5. <span data-ttu-id="3802b-263">Spara hello-filen.</span><span class="sxs-lookup"><span data-stu-id="3802b-263">Save hello file.</span></span>  
6. <span data-ttu-id="3802b-264">Markera och kopiera hello anslutningssträngen (utan omgivande citattecken hello) för användning i hello följande steg för att konfigurera hello arbetsrollsprojektet.</span><span class="sxs-lookup"><span data-stu-id="3802b-264">Select and copy hello connection string (without hello surrounding quotation marks) for use in hello following steps for configuring hello worker role project.</span></span>
7. <span data-ttu-id="3802b-265">I **Solution Explorer**under **roller** i hello molntjänstprojektet och högerklickar du på **ContosoAdsWorker** och klicka sedan på **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="3802b-265">In **Solution Explorer**, under **Roles** in hello cloud service project, right-click **ContosoAdsWorker** and then click **Properties**.</span></span>

    ![Rollegenskaper](./media/cloud-services-dotnet-get-started/rolepropertiesworker.png)
8. <span data-ttu-id="3802b-267">Klicka på hello **inställningar** fliken.</span><span class="sxs-lookup"><span data-stu-id="3802b-267">Click hello **Settings** tab.</span></span>
9. <span data-ttu-id="3802b-268">Ändra **tjänstkonfiguration** för**moln**.</span><span class="sxs-lookup"><span data-stu-id="3802b-268">Change **Service Configuration** too**Cloud**.</span></span>
10. <span data-ttu-id="3802b-269">Välj hello **värdet** för hello `ContosoAdsDbConnectionString` inställningen och klistra in hello anslutningssträngen som du kopierade tidigare hello-delen av kursen hello.</span><span class="sxs-lookup"><span data-stu-id="3802b-269">Select hello **Value** field for hello `ContosoAdsDbConnectionString` setting, and then paste hello connection string that you copied from hello previous section of hello tutorial.</span></span>

     ![Databasanslutningssträng för arbetsrollen](./media/cloud-services-dotnet-get-started/workerdbcs.png)
11. <span data-ttu-id="3802b-271">Spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="3802b-271">Save your changes.</span></span>  

### <a name="configure-hello-solution-toouse-your-azure-storage-account-when-it-runs-in-azure"></a><span data-ttu-id="3802b-272">Konfigurera hello lösning toouse Azure storage-konto när den körs i Azure</span><span class="sxs-lookup"><span data-stu-id="3802b-272">Configure hello solution toouse your Azure storage account when it runs in Azure</span></span>
<span data-ttu-id="3802b-273">Azure-lagringskontots anslutningssträngar för både hello webbrollsprojektet och arbetsrollsprojektet hello lagras i miljöinställningar i molntjänstprojektet hello.</span><span class="sxs-lookup"><span data-stu-id="3802b-273">Azure storage account connection strings for both hello web role project and hello worker role project are stored in environment settings in hello cloud service project.</span></span> <span data-ttu-id="3802b-274">Det finns en separat uppsättning inställningar toobe används när hello programmet körs lokalt och när den körs i molnet hello för varje projekt.</span><span class="sxs-lookup"><span data-stu-id="3802b-274">For each project, there is a separate set of settings toobe used when hello application runs locally and when it runs in hello cloud.</span></span> <span data-ttu-id="3802b-275">Du ska uppdatera hello molnmiljöinställningarna för både webb-och arbetsrollsprojektet.</span><span class="sxs-lookup"><span data-stu-id="3802b-275">You'll update hello cloud environment settings for both web and worker role projects.</span></span>

1. <span data-ttu-id="3802b-276">I **Solution Explorer**, högerklicka på **ContosoAdsWeb** under **roller** i hello **ContosoAdsCloudService** projektet och klicka sedan på **Egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="3802b-276">In **Solution Explorer**, right-click **ContosoAdsWeb** under **Roles** in hello **ContosoAdsCloudService** project, and then click **Properties**.</span></span>

    ![Rollegenskaper](./media/cloud-services-dotnet-get-started/roleproperties.png)
2. <span data-ttu-id="3802b-278">Klicka på hello **inställningar** fliken. I hello **tjänstkonfiguration** listrutan väljer du **moln**.</span><span class="sxs-lookup"><span data-stu-id="3802b-278">Click hello **Settings** tab. In hello **Service Configuration** drop-down box, choose **Cloud**.</span></span>

    ![Molnkonfiguration](./media/cloud-services-dotnet-get-started/sccloud.png)
3. <span data-ttu-id="3802b-280">Välj hello **StorageConnectionString** post, och sedan ser du en ellipsknapp (**...** ) längst hello högra ände hello rad.</span><span class="sxs-lookup"><span data-stu-id="3802b-280">Select hello **StorageConnectionString** entry, and you'll see an ellipsis (**...**) button at hello right end of hello line.</span></span> <span data-ttu-id="3802b-281">Klicka på hello ellips knappen tooopen hello **skapa Lagringsanslutningssträng för kontot** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3802b-281">Click hello ellipsis button tooopen hello **Create Storage Account Connection String** dialog box.</span></span>

    ![Öppna dialogrutan för att skapa anslutningssträng](./media/cloud-services-dotnet-get-started/opencscreate.png)
4. <span data-ttu-id="3802b-283">I hello **skapa Lagringsanslutningssträng** dialogrutan klickar du på **prenumerationen**, väljer hello storage-konto som du skapade tidigare och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="3802b-283">In hello **Create Storage Connection String** dialog box, click **Your subscription**, choose hello storage account that you created earlier, and then click **OK**.</span></span> <span data-ttu-id="3802b-284">Om du inte redan har loggat in uppmanas du ange dina Azure-autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="3802b-284">If you're not already logged in, you'll be prompted for your Azure account credentials.</span></span>

    ![Skapa lagringsanslutningssträng](./media/cloud-services-dotnet-get-started/createstoragecs.png)
5. <span data-ttu-id="3802b-286">Spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="3802b-286">Save your changes.</span></span>
6. <span data-ttu-id="3802b-287">Följ hello samma procedur som du använde för hello `StorageConnectionString` anslutning sträng tooset hello `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="3802b-287">Follow hello same procedure that you used for hello `StorageConnectionString` connection string tooset hello `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` connection string.</span></span>

    <span data-ttu-id="3802b-288">Den här anslutningssträngen används för loggning.</span><span class="sxs-lookup"><span data-stu-id="3802b-288">This connection string is used for logging.</span></span>
7. <span data-ttu-id="3802b-289">Följ hello samma procedur som du använde för hello **ContosoAdsWeb** rollen tooset båda anslutningssträngar för hello **ContosoAdsWorker** roll.</span><span class="sxs-lookup"><span data-stu-id="3802b-289">Follow hello same procedure that you used for hello **ContosoAdsWeb** role tooset both connection strings for hello **ContosoAdsWorker** role.</span></span> <span data-ttu-id="3802b-290">Glöm inte tooset **tjänstkonfiguration** för**moln**.</span><span class="sxs-lookup"><span data-stu-id="3802b-290">Don't forget tooset **Service Configuration** too**Cloud**.</span></span>

<span data-ttu-id="3802b-291">Hej rollmiljöinställningarna som du har konfigurerat med hello Visual Studio-Gränssnittet lagras i följande filer i ContosoAdsCloudService-projektet hello hello:</span><span class="sxs-lookup"><span data-stu-id="3802b-291">hello role environment settings that you have configured using hello Visual Studio UI are stored in hello following files in hello ContosoAdsCloudService project:</span></span>

* <span data-ttu-id="3802b-292">*ServiceDefinition.csdef* -definierar hello inställningsnamnen.</span><span class="sxs-lookup"><span data-stu-id="3802b-292">*ServiceDefinition.csdef* - Defines hello setting names.</span></span>
* <span data-ttu-id="3802b-293">*ServiceConfiguration.Cloud.cscfg* – tillhandahåller värden när hello appen körs i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="3802b-293">*ServiceConfiguration.Cloud.cscfg* - Provides values for when hello app runs in hello cloud.</span></span>
* <span data-ttu-id="3802b-294">*ServiceConfiguration.Local.cscfg* – tillhandahåller värden när hello appen körs lokalt.</span><span class="sxs-lookup"><span data-stu-id="3802b-294">*ServiceConfiguration.Local.cscfg* - Provides values for when hello app runs locally.</span></span>

<span data-ttu-id="3802b-295">Hej ServiceDefinition.csdef innehåller till exempel följande definitioner hello:</span><span class="sxs-lookup"><span data-stu-id="3802b-295">For example, hello ServiceDefinition.csdef includes hello following definitions:</span></span>

```xml
<ConfigurationSettings>
    <Setting name="StorageConnectionString" />
    <Setting name="ContosoAdsDbConnectionString" />
</ConfigurationSettings>
```

<span data-ttu-id="3802b-296">Och hello *ServiceConfiguration.Cloud.cscfg* filen innehåller hello värden för dessa inställningar i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3802b-296">And hello *ServiceConfiguration.Cloud.cscfg* file includes hello values you entered for those settings in Visual Studio.</span></span>

```xml
<Role name="ContosoAdsWorker">
    <Instances count="1" />
    <ConfigurationSettings>
        <Setting name="StorageConnectionString" value="{yourconnectionstring}" />
        <Setting name="ContosoAdsDbConnectionString" value="{yourconnectionstring}" />
        <!-- other settings not shown -->

    </ConfigurationSettings>
    <!-- other settings not shown -->

</Role>
```

<span data-ttu-id="3802b-297">Hej `<Instances>` anger hello antalet virtuella datorer som Azure ska köras hello worker rollen koden på.</span><span class="sxs-lookup"><span data-stu-id="3802b-297">hello `<Instances>` setting specifies hello number of virtual machines that Azure will run hello worker role code on.</span></span> <span data-ttu-id="3802b-298">Hej [nästa steg](#next-steps) avsnittet innehåller länkar toomore information om att skala ut en tjänst i molnet</span><span class="sxs-lookup"><span data-stu-id="3802b-298">hello [Next steps](#next-steps) section includes links toomore information about scaling out a cloud service,</span></span>

### <a name="deploy-hello-project-tooazure"></a><span data-ttu-id="3802b-299">Distribuera hello projekt tooAzure</span><span class="sxs-lookup"><span data-stu-id="3802b-299">Deploy hello project tooAzure</span></span>
1. <span data-ttu-id="3802b-300">I **Solution Explorer**, högerklicka på hello **ContosoAdsCloudService** molnprojektet och väljer sedan **publicera**.</span><span class="sxs-lookup"><span data-stu-id="3802b-300">In **Solution Explorer**, right-click hello **ContosoAdsCloudService** cloud project and then select **Publish**.</span></span>

   ![Menyn Publish (Publicera)](./media/cloud-services-dotnet-get-started/pubmenu.png)
2. <span data-ttu-id="3802b-302">I hello **logga in** steg i hello **publicera Azure-programmet** guiden, klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="3802b-302">In hello **Sign in** step of hello **Publish Azure Application** wizard, click **Next**.</span></span>

    ![Inloggningssteg](./media/cloud-services-dotnet-get-started/pubsignin.png)
3. <span data-ttu-id="3802b-304">I hello **inställningar** steg hello guiden klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="3802b-304">In hello **Settings** step of hello wizard, click **Next**.</span></span>

    ![Inställningssteg](./media/cloud-services-dotnet-get-started/pubsettings.png)

    <span data-ttu-id="3802b-306">Hej standardinställningarna i hello **Avancerat** är bra för den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="3802b-306">hello default settings in hello **Advanced** tab are fine for this tutorial.</span></span> <span data-ttu-id="3802b-307">Information om hello på fliken Avancerat finns [Publiceringsguiden för Azure-program](http://msdn.microsoft.com/library/hh535756.aspx).</span><span class="sxs-lookup"><span data-stu-id="3802b-307">For information about hello advanced tab, see [Publish Azure Application Wizard](http://msdn.microsoft.com/library/hh535756.aspx).</span></span>
4. <span data-ttu-id="3802b-308">I hello **sammanfattning** , klicka på **publicera**.</span><span class="sxs-lookup"><span data-stu-id="3802b-308">In hello **Summary** step, click **Publish**.</span></span>

    ![Sammanfattningssteg](./media/cloud-services-dotnet-get-started/pubsummary.png)

   <span data-ttu-id="3802b-310">Hej **Azure-aktivitetsloggen** öppnas i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3802b-310">hello **Azure Activity Log** window opens in Visual Studio.</span></span>
5. <span data-ttu-id="3802b-311">Klicka på hello högerpilen ikonen tooexpand hello distributionsinformation.</span><span class="sxs-lookup"><span data-stu-id="3802b-311">Click hello right arrow icon tooexpand hello deployment details.</span></span>

    <span data-ttu-id="3802b-312">hello distributionen kan ta upp too5 minuter eller mer toocomplete.</span><span class="sxs-lookup"><span data-stu-id="3802b-312">hello deployment can take up too5 minutes or more toocomplete.</span></span>

    ![Fönstret med Azure-aktivitetsloggen](./media/cloud-services-dotnet-get-started/waal.png)
6. <span data-ttu-id="3802b-314">När hello Distributionsstatus är klar klickar du på hello **Webbappens URL** toostart hello program.</span><span class="sxs-lookup"><span data-stu-id="3802b-314">When hello deployment status is complete, click hello **Web app URL** toostart hello application.</span></span>
7. <span data-ttu-id="3802b-315">Nu kan du testa hello app genom att skapa, visa och redigera vissa annonser, som du gjorde när du körde hello programmet lokalt.</span><span class="sxs-lookup"><span data-stu-id="3802b-315">You can now test hello app by creating, viewing, and editing some ads, as you did when you ran hello application locally.</span></span>

> [!NOTE]
> <span data-ttu-id="3802b-316">När du är klar testa, ta bort eller stoppa hello-Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="3802b-316">When you're finished testing, delete or stop hello cloud service.</span></span> <span data-ttu-id="3802b-317">Även om du inte använder hello Molntjänsten uppstår avgifter eftersom virtuella datorresurser är reserverade för den.</span><span class="sxs-lookup"><span data-stu-id="3802b-317">Even if you're not using hello cloud service, it's accruing charges because virtual machine resources are reserved for it.</span></span> <span data-ttu-id="3802b-318">Om du låter den köra kan dessutom alla som hittar URL:en skapa och visa annonser.</span><span class="sxs-lookup"><span data-stu-id="3802b-318">And if you leave it running, anyone who finds your URL can create and view ads.</span></span> <span data-ttu-id="3802b-319">I hello [Azure-portalen](https://portal.azure.com), gå toohello **översikt** för Molntjänsten och klicka sedan på hello **ta bort** knappen hello överst på hello sidan.</span><span class="sxs-lookup"><span data-stu-id="3802b-319">In hello [Azure portal](https://portal.azure.com), go toohello **Overview** tab for your cloud service, and then click hello **Delete** button at hello top of hello page.</span></span> <span data-ttu-id="3802b-320">Om du bara vill tootemporarily förhindra andra från att komma åt hello plats, klickar du på **stoppa** i stället.</span><span class="sxs-lookup"><span data-stu-id="3802b-320">If you just want tootemporarily prevent others from accessing hello site, click **Stop** instead.</span></span> <span data-ttu-id="3802b-321">I så fall fortsätter avgifterna tooaccrue.</span><span class="sxs-lookup"><span data-stu-id="3802b-321">In that case, charges will continue tooaccrue.</span></span> <span data-ttu-id="3802b-322">Du kan följa en liknande procedur toodelete hello SQL-databasen och storage-konto när du inte längre behöver.</span><span class="sxs-lookup"><span data-stu-id="3802b-322">You can follow a similar procedure toodelete hello SQL database and storage account when you no longer need them.</span></span>
>
>

## <a name="create-hello-application-from-scratch"></a><span data-ttu-id="3802b-323">Skapa hello programmet från grunden</span><span class="sxs-lookup"><span data-stu-id="3802b-323">Create hello application from scratch</span></span>
<span data-ttu-id="3802b-324">Om du inte redan har hämtat [hello slutförts programmet](http://code.msdn.microsoft.com/Simple-Azure-Cloud-Service-e01df2e4), göra det nu.</span><span class="sxs-lookup"><span data-stu-id="3802b-324">If you haven't already downloaded [hello completed application](http://code.msdn.microsoft.com/Simple-Azure-Cloud-Service-e01df2e4), do that now.</span></span> <span data-ttu-id="3802b-325">Du måste kopiera filer från hello hämtade projektet till hello nytt projekt.</span><span class="sxs-lookup"><span data-stu-id="3802b-325">You'll copy files from hello downloaded project into hello new project.</span></span>

<span data-ttu-id="3802b-326">Skapa hello Contoso Ads-programmet omfattar hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="3802b-326">Creating hello Contoso Ads application involves hello following steps:</span></span>

* <span data-ttu-id="3802b-327">Skapa en Visual Studio-lösning med molntjänst.</span><span class="sxs-lookup"><span data-stu-id="3802b-327">Create a cloud service Visual Studio solution.</span></span>
* <span data-ttu-id="3802b-328">Uppdatera och lägg till NuGet-paket.</span><span class="sxs-lookup"><span data-stu-id="3802b-328">Update and add NuGet packages.</span></span>
* <span data-ttu-id="3802b-329">Ange projektreferenser.</span><span class="sxs-lookup"><span data-stu-id="3802b-329">Set project references.</span></span>
* <span data-ttu-id="3802b-330">Konfigurera anslutningssträngar.</span><span class="sxs-lookup"><span data-stu-id="3802b-330">Configure connection strings.</span></span>
* <span data-ttu-id="3802b-331">Lägg till kodfiler.</span><span class="sxs-lookup"><span data-stu-id="3802b-331">Add code files.</span></span>

<span data-ttu-id="3802b-332">När du har skapat hello lösning ska du granska hello-kod som är unik toocloud service-projekt och Azure-blobbar och köer.</span><span class="sxs-lookup"><span data-stu-id="3802b-332">After hello solution is created, you'll review hello code that is unique toocloud service projects and Azure blobs and queues.</span></span>

### <a name="create-a-cloud-service-visual-studio-solution"></a><span data-ttu-id="3802b-333">Skapa en Visual Studio-lösning med molntjänst</span><span class="sxs-lookup"><span data-stu-id="3802b-333">Create a cloud service Visual Studio solution</span></span>
1. <span data-ttu-id="3802b-334">I Visual Studio väljer **nytt projekt** från hello **filen** menyn.</span><span class="sxs-lookup"><span data-stu-id="3802b-334">In Visual Studio, choose **New Project** from hello **File** menu.</span></span>
2. <span data-ttu-id="3802b-335">Hello vänster i hello **nytt projekt** dialogrutan Expandera **Visual C#** och välj **moln** mallar, och välj sedan hello **Azure Cloud Service** mall.</span><span class="sxs-lookup"><span data-stu-id="3802b-335">In hello left pane of hello **New Project** dialog box, expand **Visual C#** and choose **Cloud** templates, and then choose hello **Azure Cloud Service** template.</span></span>
3. <span data-ttu-id="3802b-336">Namnge hello projektet och lösningen ContosoAdsCloudService och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="3802b-336">Name hello project and solution ContosoAdsCloudService, and then click **OK**.</span></span>

    ![Nytt projekt](./media/cloud-services-dotnet-get-started/newproject.png)
4. <span data-ttu-id="3802b-338">I hello **nya Azure Cloud Service** dialogrutan Lägg till en webbroll och en arbetsroll.</span><span class="sxs-lookup"><span data-stu-id="3802b-338">In hello **New Azure Cloud Service** dialog box, add a web role and a worker role.</span></span> <span data-ttu-id="3802b-339">Namnge hello webbroll ContosoAdsWeb och namnet hello arbetsrollen ContosoAdsWorker.</span><span class="sxs-lookup"><span data-stu-id="3802b-339">Name hello web role ContosoAdsWeb, and name hello worker role ContosoAdsWorker.</span></span> <span data-ttu-id="3802b-340">(Använd pennikonen hello i hello högra fönstret toochange hello standardnamnen hello roller.)</span><span class="sxs-lookup"><span data-stu-id="3802b-340">(Use hello pencil icon in hello right-hand pane toochange hello default names of hello roles.)</span></span>

    ![Nytt molntjänstprojekt](./media/cloud-services-dotnet-get-started/newcsproj.png)
5. <span data-ttu-id="3802b-342">När du ser hello **nytt ASP.NET-projekt** dialogrutan för hello webbrollen, Välj hello MVC-mallen och klicka sedan på **ändra autentisering**.</span><span class="sxs-lookup"><span data-stu-id="3802b-342">When you see hello **New ASP.NET Project** dialog box for hello web role, choose hello MVC template, and then click **Change Authentication**.</span></span>

    ![Ändra autentisering](./media/cloud-services-dotnet-get-started/chgauth.png)
6. <span data-ttu-id="3802b-344">I hello **ändra autentisering** dialogrutan Välj **ingen autentisering**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="3802b-344">In hello **Change Authentication** dialog box, choose **No Authentication**, and then click **OK**.</span></span>

    ![Ingen autentisering](./media/cloud-services-dotnet-get-started/noauth.png)
7. <span data-ttu-id="3802b-346">I hello **nytt ASP.NET-projekt** dialogrutan klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="3802b-346">In hello **New ASP.NET Project** dialog, click **OK**.</span></span>
8. <span data-ttu-id="3802b-347">I **Solution Explorer**, högerklicka på hello lösningen (inte en hello projekt) och välj **Lägg till – nytt projekt**.</span><span class="sxs-lookup"><span data-stu-id="3802b-347">In **Solution Explorer**, right-click hello solution (not one of hello projects), and choose **Add - New Project**.</span></span>
9. <span data-ttu-id="3802b-348">I hello **Lägg till nytt projekt** dialogrutan Välj **Windows** under **Visual C#** i hello till vänster och klicka sedan på hello **klassbiblioteket** mall.</span><span class="sxs-lookup"><span data-stu-id="3802b-348">In hello **Add New Project** dialog box, choose **Windows** under **Visual C#** in hello left pane, and then click hello **Class Library** template.</span></span>  
10. <span data-ttu-id="3802b-349">Namnet hello projektet *ContosoAdsCommon*, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="3802b-349">Name hello project *ContosoAdsCommon*, and then click **OK**.</span></span>

    <span data-ttu-id="3802b-350">Du måste tooreference hello Entity Framework kontexten och hello datamodellen från både webb-och arbetsrollsprojektet.</span><span class="sxs-lookup"><span data-stu-id="3802b-350">You need tooreference hello Entity Framework context and hello data model from both web and worker role projects.</span></span> <span data-ttu-id="3802b-351">Som ett alternativ kan du definiera hello EF-relaterade klasserna i webbrollsprojektet hello och referera till det projektet från arbetsrollsprojektet hello.</span><span class="sxs-lookup"><span data-stu-id="3802b-351">As an alternative, you could define hello EF-related classes in hello web role project and reference that project from hello worker role project.</span></span> <span data-ttu-id="3802b-352">Men i hello alternativa metoden måste arbetsrollsprojektet måste en tooweb för referenssammansättningar som inte behövs.</span><span class="sxs-lookup"><span data-stu-id="3802b-352">But in hello alternative approach, your worker role project would have a reference tooweb assemblies that it doesn't need.</span></span>

### <a name="update-and-add-nuget-packages"></a><span data-ttu-id="3802b-353">Uppdatera och lägga till NuGet-paket</span><span class="sxs-lookup"><span data-stu-id="3802b-353">Update and add NuGet packages</span></span>
1. <span data-ttu-id="3802b-354">Öppna hello **hantera NuGet-paket** dialogrutan för hello lösning.</span><span class="sxs-lookup"><span data-stu-id="3802b-354">Open hello **Manage NuGet Packages** dialog box for hello solution.</span></span>
2. <span data-ttu-id="3802b-355">Överst hello i hello fönster, Välj **uppdateringar**.</span><span class="sxs-lookup"><span data-stu-id="3802b-355">At hello top of hello window, select **Updates**.</span></span>
3. <span data-ttu-id="3802b-356">Leta efter hello *WindowsAzure.Storage* paketet, och om den är i hello listan markerar du den och välj hello webb- och arbetsroller projekt tooupdate det och klicka sedan på **uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="3802b-356">Look for hello *WindowsAzure.Storage* package, and if it's in hello list, select it and select hello web and worker projects tooupdate it in, and then click **Update**.</span></span>

    <span data-ttu-id="3802b-357">hello lagringsklientbiblioteket uppdateras oftare än Visual Studio-projektmallar, så ofta hittar du den hello-versionen i ett projekt som nyligen skapats måste toobe uppdateras.</span><span class="sxs-lookup"><span data-stu-id="3802b-357">hello storage client library is updated more frequently than Visual Studio project templates, so you'll often find that hello version in a newly-created project needs toobe updated.</span></span>
4. <span data-ttu-id="3802b-358">Överst hello i hello fönster, Välj **Bläddra**.</span><span class="sxs-lookup"><span data-stu-id="3802b-358">At hello top of hello window, select **Browse**.</span></span>
5. <span data-ttu-id="3802b-359">Hitta hello *EntityFramework* NuGet-paketet och installera det i alla tre projekt.</span><span class="sxs-lookup"><span data-stu-id="3802b-359">Find hello *EntityFramework* NuGet package, and install it in all three projects.</span></span>
6. <span data-ttu-id="3802b-360">Hitta hello *Microsoft.WindowsAzure.ConfigurationManager* NuGet-paketet och installera det i arbetsrollsprojektet hello.</span><span class="sxs-lookup"><span data-stu-id="3802b-360">Find hello *Microsoft.WindowsAzure.ConfigurationManager* NuGet package, and install it in hello worker role project.</span></span>

### <a name="set-project-references"></a><span data-ttu-id="3802b-361">Ange projektreferenser</span><span class="sxs-lookup"><span data-stu-id="3802b-361">Set project references</span></span>
1. <span data-ttu-id="3802b-362">Ange en referens toohello ContosoAdsCommon-projektet i ContosoAdsWeb-projektet hello.</span><span class="sxs-lookup"><span data-stu-id="3802b-362">In hello ContosoAdsWeb project, set a reference toohello ContosoAdsCommon project.</span></span> <span data-ttu-id="3802b-363">Högerklicka på hello ContosoAdsWeb-projektet och klicka sedan på **referenser** - **Add References**.</span><span class="sxs-lookup"><span data-stu-id="3802b-363">Right-click hello ContosoAdsWeb project, and then click **References** - **Add References**.</span></span> <span data-ttu-id="3802b-364">I hello **Reference Manager** dialogrutan **lösning – projekt** i hello vänster och välj **ContosoAdsCommon**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="3802b-364">In hello **Reference Manager** dialog box, select **Solution – Projects** in hello left pane, select **ContosoAdsCommon**, and then click **OK**.</span></span>
2. <span data-ttu-id="3802b-365">Ange en referens toohello Contosoadscommon-projektet i ContosoAdsWorker-projektet hello.</span><span class="sxs-lookup"><span data-stu-id="3802b-365">In hello ContosoAdsWorker project, set a reference toohello ContosAdsCommon project.</span></span>

    <span data-ttu-id="3802b-366">ContosoAdsCommon innehåller hello Entity Framework data modellen och -kontextklassen som kommer att användas av båda hello frontend- och.</span><span class="sxs-lookup"><span data-stu-id="3802b-366">ContosoAdsCommon will contain hello Entity Framework data model and context class, which will be used by both hello front-end and back-end.</span></span>
3. <span data-ttu-id="3802b-367">I hello ContosoAdsWorker-projektet, ange en referens för`System.Drawing`.</span><span class="sxs-lookup"><span data-stu-id="3802b-367">In hello ContosoAdsWorker project, set a reference too`System.Drawing`.</span></span>

    <span data-ttu-id="3802b-368">Den här sammansättningen används av hello backend-tooconvert bilder toothumbnails.</span><span class="sxs-lookup"><span data-stu-id="3802b-368">This assembly is used by hello back-end tooconvert images toothumbnails.</span></span>

### <a name="configure-connection-strings"></a><span data-ttu-id="3802b-369">Konfigurera anslutningssträngar</span><span class="sxs-lookup"><span data-stu-id="3802b-369">Configure connection strings</span></span>
<span data-ttu-id="3802b-370">I det här avsnittet konfigurerar du Azure Storage- och SQL-anslutningssträngar för lokal testning.</span><span class="sxs-lookup"><span data-stu-id="3802b-370">In this section, you configure Azure Storage and SQL connection strings for testing locally.</span></span> <span data-ttu-id="3802b-371">Hej distributionsanvisningarna tidigare i kursen hello förklarar hur tooset hello anslutning strängarna för när hello appen körs i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="3802b-371">hello deployment instructions earlier in hello tutorial explain how tooset up hello connection strings for when hello app runs in hello cloud.</span></span>

1. <span data-ttu-id="3802b-372">I hello ContosoAdsWeb-projektet, öppna hello webbprogrammets Web.config-fil och infoga hello följande `connectionStrings` elementet efter hello `configSections` element.</span><span class="sxs-lookup"><span data-stu-id="3802b-372">In hello ContosoAdsWeb project, open hello application Web.config file, and insert hello following `connectionStrings` element after hello `configSections` element.</span></span>

    ```xml
    <connectionStrings>
        <add name="ContosoAdsContext" connectionString="Data Source=(localdb)\v11.0; Initial Catalog=ContosoAds; Integrated Security=True; MultipleActiveResultSets=True;" providerName="System.Data.SqlClient" />
    </connectionStrings>
    ```

    <span data-ttu-id="3802b-373">Om du använder Visual Studio 2015 eller högre ersätter du ”v11.0” med ”MSSQLLocalDB”.</span><span class="sxs-lookup"><span data-stu-id="3802b-373">If you're using Visual Studio 2015 or higher, replace "v11.0" with "MSSQLLocalDB".</span></span>
2. <span data-ttu-id="3802b-374">Spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="3802b-374">Save your changes.</span></span>
3. <span data-ttu-id="3802b-375">Hej ContosoAdsCloudService-projektet, högerklicka på ContosoAdsWeb under **roller**, och klicka sedan på **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="3802b-375">In hello ContosoAdsCloudService project, right-click ContosoAdsWeb under **Roles**, and then click **Properties**.</span></span>

    ![Rollegenskaper](./media/cloud-services-dotnet-get-started/roleproperties.png)
4. <span data-ttu-id="3802b-377">I hello **ContosAdsWeb [roll]** i fönstret klickar du på hello **inställningar** fliken och klicka sedan på **Lägg till inställning**.</span><span class="sxs-lookup"><span data-stu-id="3802b-377">In hello **ContosAdsWeb [Role]** properties window, click hello **Settings** tab, and then click **Add Setting**.</span></span>

    <span data-ttu-id="3802b-378">Lämna **tjänstkonfiguration** ställa in också**alla konfigurationer av**.</span><span class="sxs-lookup"><span data-stu-id="3802b-378">Leave **Service Configuration** set too**All Configurations**.</span></span>
5. <span data-ttu-id="3802b-379">Lägg till en inställning med namnet *StorageConnectionString*.</span><span class="sxs-lookup"><span data-stu-id="3802b-379">Add a setting named *StorageConnectionString*.</span></span> <span data-ttu-id="3802b-380">Ange **typen** för*ConnectionString*, och ange **värdet** för*UseDevelopmentStorage = true*.</span><span class="sxs-lookup"><span data-stu-id="3802b-380">Set **Type** too*ConnectionString*, and set **Value** too*UseDevelopmentStorage=true*.</span></span>

    ![Ny anslutningssträng](./media/cloud-services-dotnet-get-started/scall.png)
6. <span data-ttu-id="3802b-382">Spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="3802b-382">Save your changes.</span></span>
7. <span data-ttu-id="3802b-383">Följ hello samma procedur tooadd en lagringsanslutningssträng i ContosoAdsWorker-Rollegenskaperna hello.</span><span class="sxs-lookup"><span data-stu-id="3802b-383">Follow hello same procedure tooadd a storage connection string in hello ContosoAdsWorker role properties.</span></span>
8. <span data-ttu-id="3802b-384">Fortfarande i hello **ContosoAdsWorker [roll]** i fönstret Lägg till ytterligare en anslutningssträng:</span><span class="sxs-lookup"><span data-stu-id="3802b-384">Still in hello **ContosoAdsWorker [Role]** properties window, add another connection string:</span></span>

   * <span data-ttu-id="3802b-385">Namn: ContosoAdsDbConnectionString</span><span class="sxs-lookup"><span data-stu-id="3802b-385">Name: ContosoAdsDbConnectionString</span></span>
   * <span data-ttu-id="3802b-386">Typ: Sträng</span><span class="sxs-lookup"><span data-stu-id="3802b-386">Type: String</span></span>
   * <span data-ttu-id="3802b-387">Värde: Klistra in hello samma anslutningssträng som du använde för webbrollsprojektet hello.</span><span class="sxs-lookup"><span data-stu-id="3802b-387">Value: Paste hello same connection string you used for hello web role project.</span></span> <span data-ttu-id="3802b-388">(följande exempel hello används för Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="3802b-388">(hello following example is for Visual Studio 2013.</span></span> <span data-ttu-id="3802b-389">Glöm inte toochange hello datakälla om du kopierar det här exemplet och du använder Visual Studio 2015 eller högre.)</span><span class="sxs-lookup"><span data-stu-id="3802b-389">Don't forget toochange hello Data Source if you copy this example and you're using Visual Studio 2015 or higher.)</span></span>

       ```
       Data Source=(localdb)\v11.0; Initial Catalog=ContosoAds; Integrated Security=True; MultipleActiveResultSets=True;
       ```

### <a name="add-code-files"></a><span data-ttu-id="3802b-390">Lägga till kodfiler</span><span class="sxs-lookup"><span data-stu-id="3802b-390">Add code files</span></span>
<span data-ttu-id="3802b-391">I det här avsnittet Kopiera kodfiler från hello hämtade lösningen till hello ny lösning.</span><span class="sxs-lookup"><span data-stu-id="3802b-391">In this section, you copy code files from hello downloaded solution into hello new solution.</span></span> <span data-ttu-id="3802b-392">hello följande avsnitt visas och förklaras viktiga delar av den här koden.</span><span class="sxs-lookup"><span data-stu-id="3802b-392">hello following sections will show and explain key parts of this code.</span></span>

<span data-ttu-id="3802b-393">tooadd filer tooa projekt eller en mapp, högerklickar du på hello projektet eller mappen och klicka på **Lägg till** - **befintlig artikel**.</span><span class="sxs-lookup"><span data-stu-id="3802b-393">tooadd files tooa project or a folder, right-click hello project or folder and click **Add** - **Existing Item**.</span></span> <span data-ttu-id="3802b-394">Markerar hello filer och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="3802b-394">Select hello files you want and then click **Add**.</span></span> <span data-ttu-id="3802b-395">Om du blir tillfrågad om du vill tooreplace befintliga filer, klickar du på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="3802b-395">If asked whether you want tooreplace existing files, click **Yes**.</span></span>

1. <span data-ttu-id="3802b-396">Ta bort hello i hello ContosoAdsCommon-projektet *Class1.cs* och Lägg till i dess ställe hello *Ad.cs* och *ContosoAdscontext.cs* filer från hello hämtade projektet.</span><span class="sxs-lookup"><span data-stu-id="3802b-396">In hello ContosoAdsCommon project, delete hello *Class1.cs* file and add in its place hello *Ad.cs* and *ContosoAdscontext.cs* files from hello downloaded project.</span></span>
2. <span data-ttu-id="3802b-397">Lägg till hello följande filer från hello hämtade projektet i ContosoAdsWeb-projektet hello.</span><span class="sxs-lookup"><span data-stu-id="3802b-397">In hello ContosoAdsWeb project, add hello following files from hello downloaded project.</span></span>

   * <span data-ttu-id="3802b-398">*Global.asax.cs*.</span><span class="sxs-lookup"><span data-stu-id="3802b-398">*Global.asax.cs*.</span></span>  
   * <span data-ttu-id="3802b-399">I hello *Views\Shared* mapp:  *\_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="3802b-399">In hello *Views\Shared* folder: *\_Layout.cshtml*.</span></span>
   * <span data-ttu-id="3802b-400">I hello *Views\Home* mapp: *Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="3802b-400">In hello *Views\Home* folder: *Index.cshtml*.</span></span>
   * <span data-ttu-id="3802b-401">I hello *domänkontrollanter* mapp: *AdController.cs*.</span><span class="sxs-lookup"><span data-stu-id="3802b-401">In hello *Controllers* folder: *AdController.cs*.</span></span>
   * <span data-ttu-id="3802b-402">I hello *Views\Ad* mapp (skapa hello mappen först): fem *.cshtml* filer.</span><span class="sxs-lookup"><span data-stu-id="3802b-402">In hello *Views\Ad* folder (create hello folder first): five *.cshtml* files.</span></span>
3. <span data-ttu-id="3802b-403">Lägg till i ContosoAdsWorker-projektet hello *WorkerRole.cs* hello hämtat projektet.</span><span class="sxs-lookup"><span data-stu-id="3802b-403">In hello ContosoAdsWorker project, add *WorkerRole.cs* from hello downloaded project.</span></span>

<span data-ttu-id="3802b-404">Du kan nu skapa och köra programmet hello enligt anvisningarna tidigare i kursen hello och hello appen kommer att använda lokala databasen och storage-emulatorn-resurser.</span><span class="sxs-lookup"><span data-stu-id="3802b-404">You can now build and run hello application as instructed earlier in hello tutorial, and hello app will use local database and storage emulator resources.</span></span>

<span data-ttu-id="3802b-405">hello följande avsnitt beskrivs hello kod relaterade tooworking med hello Azure-miljön, blobbar och köer.</span><span class="sxs-lookup"><span data-stu-id="3802b-405">hello following sections explain hello code related tooworking with hello Azure environment, blobs, and queues.</span></span> <span data-ttu-id="3802b-406">Den här kursen förklaras inte hur toocreate MVC-kontrollanter och vyer med scaffold-teknik, hur toowrite Entity Framework-kod som fungerar med SQL Server-databaser eller hello grunderna för asynkron programmering i ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="3802b-406">This tutorial does not explain how toocreate MVC controllers and views using scaffolding, how toowrite Entity Framework code that works with SQL Server databases, or hello basics of asynchronous programming in ASP.NET 4.5.</span></span> <span data-ttu-id="3802b-407">Information om de här ämnena finns hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="3802b-407">For information about these topics, see hello following resources:</span></span>

* [<span data-ttu-id="3802b-408">Kom igång med MVC 5</span><span class="sxs-lookup"><span data-stu-id="3802b-408">Get started with MVC 5</span></span>](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started)
* [<span data-ttu-id="3802b-409">Kom igång med EF 6 och MVC 5</span><span class="sxs-lookup"><span data-stu-id="3802b-409">Get started with EF 6 and MVC 5</span></span>](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc)
* <span data-ttu-id="3802b-410">[Introduktion tooasynchronous programmering i .NET 4.5](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices#async).</span><span class="sxs-lookup"><span data-stu-id="3802b-410">[Introduction tooasynchronous programming in .NET 4.5](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices#async).</span></span>

### <a name="contosoadscommon---adcs"></a><span data-ttu-id="3802b-411">ContosoAdsCommon – Ad.cs</span><span class="sxs-lookup"><span data-stu-id="3802b-411">ContosoAdsCommon - Ad.cs</span></span>
<span data-ttu-id="3802b-412">filen för hello Ad.cs definierar en uppräkning för annonskategorier och en POCO-entitetsklass för annonsinformation.</span><span class="sxs-lookup"><span data-stu-id="3802b-412">hello Ad.cs file defines an enum for ad categories and a POCO entity class for ad information.</span></span>

```csharp
public enum Category
{
    Cars,
    [Display(Name="Real Estate")]
    RealEstate,
    [Display(Name = "Free Stuff")]
    FreeStuff
}

public class Ad
{
    public int AdId { get; set; }

    [StringLength(100)]
    public string Title { get; set; }

    public int Price { get; set; }

    [StringLength(1000)]
    [DataType(DataType.MultilineText)]
    public string Description { get; set; }

    [StringLength(1000)]
    [DisplayName("Full-size Image")]
    public string ImageURL { get; set; }

    [StringLength(1000)]
    [DisplayName("Thumbnail")]
    public string ThumbnailURL { get; set; }

    [DataType(DataType.Date)]
    [DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
    public DateTime PostedDate { get; set; }

    public Category? Category { get; set; }
    [StringLength(12)]
    public string Phone { get; set; }
}
```

### <a name="contosoadscommon---contosoadscontextcs"></a><span data-ttu-id="3802b-413">ContosoAdsCommon – ContosoAdsContext.cs</span><span class="sxs-lookup"><span data-stu-id="3802b-413">ContosoAdsCommon - ContosoAdsContext.cs</span></span>
<span data-ttu-id="3802b-414">Hej ContosoAdsContext-klassen anger att hello annonsklassen används i en DbSet-samling som Entity Framework lagrar i en SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="3802b-414">hello ContosoAdsContext class specifies that hello Ad class is used in a DbSet collection, which Entity Framework will store in a SQL database.</span></span>

```csharp
public class ContosoAdsContext : DbContext
{
    public ContosoAdsContext() : base("name=ContosoAdsContext")
    {
    }
    public ContosoAdsContext(string connString)
        : base(connString)
    {
    }
    public System.Data.Entity.DbSet<Ad> Ads { get; set; }
}
```

<span data-ttu-id="3802b-415">hello-klassen har två konstruktorer.</span><span class="sxs-lookup"><span data-stu-id="3802b-415">hello class has two constructors.</span></span> <span data-ttu-id="3802b-416">Hej först av dem används av hello webbprojektet och anger en anslutningssträng som lagras i Web.config-filen för hello hello namn.</span><span class="sxs-lookup"><span data-stu-id="3802b-416">hello first of them is used by hello web project, and specifies hello name of a connection string that is stored in hello Web.config file.</span></span> <span data-ttu-id="3802b-417">hello andra konstruktorn gör toopass i faktiska hello anslutningssträngen som används av hello arbetsrollsprojektet eftersom det inte har en Web.config-fil.</span><span class="sxs-lookup"><span data-stu-id="3802b-417">hello second constructor enables you toopass in hello actual connection string used by hello worker role project, since it doesn't have a Web.config file.</span></span> <span data-ttu-id="3802b-418">Du såg tidigare där den här anslutningssträngen lagras, och du ser hur hello koden hämtar anslutningssträngen hello när den instantierar DbContext-klassen hello.</span><span class="sxs-lookup"><span data-stu-id="3802b-418">You saw earlier where this connection string was stored, and you'll see later how hello code retrieves hello connection string when it instantiates hello DbContext class.</span></span>

### <a name="contosoadsweb---globalasaxcs"></a><span data-ttu-id="3802b-419">ContosoAdsWeb – Global.asax.cs</span><span class="sxs-lookup"><span data-stu-id="3802b-419">ContosoAdsWeb - Global.asax.cs</span></span>
<span data-ttu-id="3802b-420">Kod som anropas från hello `Application_Start` metoden skapar en *bilder* blob-behållaren och en *bilder* kö om de inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="3802b-420">Code that is called from hello `Application_Start` method creates an *images* blob container and an *images* queue if they don't already exist.</span></span> <span data-ttu-id="3802b-421">Detta säkerställer att när du börjar med ett nytt lagringskonto eller börjar använda lagringsemulatorn hello på en ny dator, hello nödvändiga blobbehållaren och kön skapas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="3802b-421">This ensures that whenever you start using a new storage account, or start using hello storage emulator on a new computer, hello required blob container and queue will be created automatically.</span></span>

<span data-ttu-id="3802b-422">Hej kod hämtar åtkomst toohello storage-konto med hjälp av hello lagringsanslutningssträngen från hello *.cscfg* fil.</span><span class="sxs-lookup"><span data-stu-id="3802b-422">hello code gets access toohello storage account by using hello storage connection string from hello *.cscfg* file.</span></span>

```csharp
var storageAccount = CloudStorageAccount.Parse
    (RoleEnvironment.GetConfigurationSettingValue("StorageConnectionString"));
```

<span data-ttu-id="3802b-423">Sedan hämtar den en referens toohello *bilder* , skapar hello behållaren om den inte redan finns och anger åtkomstbehörighet för hello ny behållare.</span><span class="sxs-lookup"><span data-stu-id="3802b-423">Then it gets a reference toohello *images* blob container, creates hello container if it doesn't already exist, and sets access permissions on hello new container.</span></span> <span data-ttu-id="3802b-424">Som standard tillåter nya behållare enbart klienter med lagringskonto autentiseringsuppgifter tooaccess blobbar.</span><span class="sxs-lookup"><span data-stu-id="3802b-424">By default, new containers only allow clients with storage account credentials tooaccess blobs.</span></span> <span data-ttu-id="3802b-425">hello webbplats måste hello blobbar toobe offentliga så att den kan visa bilder med URL: er som punkt toohello bildblobbarna.</span><span class="sxs-lookup"><span data-stu-id="3802b-425">hello website needs hello blobs toobe public so that it can display images using URLs that point toohello image blobs.</span></span>

```csharp
var blobClient = storageAccount.CreateCloudBlobClient();
var imagesBlobContainer = blobClient.GetContainerReference("images");
if (imagesBlobContainer.CreateIfNotExists())
{
    imagesBlobContainer.SetPermissions(
        new BlobContainerPermissions
        {
            PublicAccess =BlobContainerPublicAccessType.Blob
        });
}
```

<span data-ttu-id="3802b-426">Liknande kod hämtar en referens toohello *bilder* kön och skapar en ny kö.</span><span class="sxs-lookup"><span data-stu-id="3802b-426">Similar code gets a reference toohello *images* queue and creates a new queue.</span></span> <span data-ttu-id="3802b-427">I så fall krävs ingen ändring av behörigheter.</span><span class="sxs-lookup"><span data-stu-id="3802b-427">In this case, no permissions change is needed.</span></span>

```csharp
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
var imagesQueue = queueClient.GetQueueReference("images");
imagesQueue.CreateIfNotExists();
```

### <a name="contosoadsweb---layoutcshtml"></a><span data-ttu-id="3802b-428">ContosoAdsWeb – \_Layout.cshtml</span><span class="sxs-lookup"><span data-stu-id="3802b-428">ContosoAdsWeb - \_Layout.cshtml</span></span>
<span data-ttu-id="3802b-429">Hej *_Layout.cshtml* filen anger hello programnamn i hello sidhuvud och sidfot och skapar en menypost ”Ads”.</span><span class="sxs-lookup"><span data-stu-id="3802b-429">hello *_Layout.cshtml* file sets hello app name in hello header and footer, and creates an "Ads" menu entry.</span></span>

### <a name="contosoadsweb---viewshomeindexcshtml"></a><span data-ttu-id="3802b-430">ContosoAdsWeb – Views\Home\Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="3802b-430">ContosoAdsWeb - Views\Home\Index.cshtml</span></span>
<span data-ttu-id="3802b-431">Hej *Views\Home\Index.cshtml* visar kategorilänkar på startsidan för hello.</span><span class="sxs-lookup"><span data-stu-id="3802b-431">hello *Views\Home\Index.cshtml* file displays category links on hello home page.</span></span> <span data-ttu-id="3802b-432">hello länkarna överför hello heltalsvärde hello `Category` uppräkning i en querystring-variabel toohello Ads-indexsidan.</span><span class="sxs-lookup"><span data-stu-id="3802b-432">hello links pass hello integer value of hello `Category` enum in a querystring variable toohello Ads Index page.</span></span>

```razor
<li>@Html.ActionLink("Cars", "Index", "Ad", new { category = (int)Category.Cars }, null)</li>
<li>@Html.ActionLink("Real estate", "Index", "Ad", new { category = (int)Category.RealEstate }, null)</li>
<li>@Html.ActionLink("Free stuff", "Index", "Ad", new { category = (int)Category.FreeStuff }, null)</li>
<li>@Html.ActionLink("All", "Index", "Ad", null, null)</li>
```

### <a name="contosoadsweb---adcontrollercs"></a><span data-ttu-id="3802b-433">ContosoAdsWeb – AdController.cs</span><span class="sxs-lookup"><span data-stu-id="3802b-433">ContosoAdsWeb - AdController.cs</span></span>
<span data-ttu-id="3802b-434">I hello *AdController.cs* fil, hello konstruktorn anrop hello `InitializeStorage` metoden toocreate Azure Storage-klientbibliotek objekt som tillhandahåller en API för att arbeta med blobbar och köer.</span><span class="sxs-lookup"><span data-stu-id="3802b-434">In hello *AdController.cs* file, hello constructor calls hello `InitializeStorage` method toocreate Azure Storage Client Library objects that provide an API for working with blobs and queues.</span></span>

<span data-ttu-id="3802b-435">Sedan hello kod hämtar en referens toohello *bilder* blob-behållare som du såg tidigare i *Global.asax.cs*.</span><span class="sxs-lookup"><span data-stu-id="3802b-435">Then hello code gets a reference toohello *images* blob container as you saw earlier in *Global.asax.cs*.</span></span> <span data-ttu-id="3802b-436">När den gör det anger den en [standardpolicy för återförsök](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling) som är lämplig för en webbapp.</span><span class="sxs-lookup"><span data-stu-id="3802b-436">While doing that it sets a default [retry policy](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling) appropriate for a web app.</span></span> <span data-ttu-id="3802b-437">hello standardprincipen exponentiell backoff försök kan hänga hello webbappen längre än en minut vid upprepade återförsök för ett tillfälligt fel.</span><span class="sxs-lookup"><span data-stu-id="3802b-437">hello default exponential backoff retry policy could hang hello web app for longer than a minute on repeated retries for a transient fault.</span></span> <span data-ttu-id="3802b-438">Hej återförsökspolicyn som anges här väntar tre sekunder efter varje försök i upp toothree försök.</span><span class="sxs-lookup"><span data-stu-id="3802b-438">hello retry policy specified here waits three seconds after each try for up toothree tries.</span></span>

```csharp
var blobClient = storageAccount.CreateCloudBlobClient();
blobClient.DefaultRequestOptions.RetryPolicy = new LinearRetry(TimeSpan.FromSeconds(3), 3);
imagesBlobContainer = blobClient.GetContainerReference("images");
```

<span data-ttu-id="3802b-439">Liknande kod hämtar en referens toohello *bilder* kön.</span><span class="sxs-lookup"><span data-stu-id="3802b-439">Similar code gets a reference toohello *images* queue.</span></span>

```csharp
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
queueClient.DefaultRequestOptions.RetryPolicy = new LinearRetry(TimeSpan.FromSeconds(3), 3);
imagesQueue = queueClient.GetQueueReference("images");
```

<span data-ttu-id="3802b-440">De flesta hello kontrollantkoden är typisk när du arbetar med en Entity Framework-datamodell med en DbContext-klassen.</span><span class="sxs-lookup"><span data-stu-id="3802b-440">Most of hello controller code is typical for working with an Entity Framework data model using a DbContext class.</span></span> <span data-ttu-id="3802b-441">Ett undantag är hello HttpPost `Create` metod som överför en fil och sparar den i blob storage.</span><span class="sxs-lookup"><span data-stu-id="3802b-441">An exception is hello HttpPost `Create` method, which uploads a file and saves it in blob storage.</span></span> <span data-ttu-id="3802b-442">Hej modellbindaren tillhandahåller ett [HttpPostedFileBase](http://msdn.microsoft.com/library/system.web.httppostedfilebase.aspx) objekt toohello metod.</span><span class="sxs-lookup"><span data-stu-id="3802b-442">hello model binder provides an [HttpPostedFileBase](http://msdn.microsoft.com/library/system.web.httppostedfilebase.aspx) object toohello method.</span></span>

```csharp
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<ActionResult> Create(
    [Bind(Include = "Title,Price,Description,Category,Phone")] Ad ad,
    HttpPostedFileBase imageFile)
```

<span data-ttu-id="3802b-443">Om hello användaren har valt en fil tooupload hello kod överför hello-fil, sparar den i en blob och uppdaterar hello Ad-databasposten med en URL som pekar toohello blob.</span><span class="sxs-lookup"><span data-stu-id="3802b-443">If hello user selected a file tooupload, hello code uploads hello file, saves it in a blob, and updates hello Ad database record with a URL that points toohello blob.</span></span>

```csharp
if (imageFile != null && imageFile.ContentLength != 0)
{
    blob = await UploadAndSaveBlobAsync(imageFile);
    ad.ImageURL = blob.Uri.ToString();
}
```

<span data-ttu-id="3802b-444">hello kod som hello uppladdningen är i hello `UploadAndSaveBlobAsync` metod.</span><span class="sxs-lookup"><span data-stu-id="3802b-444">hello code that does hello upload is in hello `UploadAndSaveBlobAsync` method.</span></span> <span data-ttu-id="3802b-445">Den skapar ett Guidenamn för hello blob, överföringar och sparar hello fil och returnerar en referens toohello sparade blob.</span><span class="sxs-lookup"><span data-stu-id="3802b-445">It creates a GUID name for hello blob, uploads and saves hello file, and returns a reference toohello saved blob.</span></span>

```csharp
private async Task<CloudBlockBlob> UploadAndSaveBlobAsync(HttpPostedFileBase imageFile)
{
    string blobName = Guid.NewGuid().ToString() + Path.GetExtension(imageFile.FileName);
    CloudBlockBlob imageBlob = imagesBlobContainer.GetBlockBlobReference(blobName);
    using (var fileStream = imageFile.InputStream)
    {
        await imageBlob.UploadFromStreamAsync(fileStream);
    }
    return imageBlob;
}
```

<span data-ttu-id="3802b-446">Efter hello HttpPost `Create` metoden laddar upp en blob och uppdateringar Hej databas, skapar en kö meddelandet tooinform backend-processen att en bild är redo för konvertering tooa miniatyr.</span><span class="sxs-lookup"><span data-stu-id="3802b-446">After hello HttpPost `Create` method uploads a blob and updates hello database, it creates a queue message tooinform that back-end process that an image is ready for conversion tooa thumbnail.</span></span>

```csharp
string queueMessageString = ad.AdId.ToString();
var queueMessage = new CloudQueueMessage(queueMessageString);
await queue.AddMessageAsync(queueMessage);
```

<span data-ttu-id="3802b-447">Hej koden för hello HttpPost `Edit` metoden är liknande, förutom att om hello användaren markerar en ny bildfil måste alla blobbar som redan finns tas bort.</span><span class="sxs-lookup"><span data-stu-id="3802b-447">hello code for hello HttpPost `Edit` method is similar except that if hello user selects a new image file any blobs that already exist must be deleted.</span></span>

```csharp
if (imageFile != null && imageFile.ContentLength != 0)
{
    await DeleteAdBlobsAsync(ad);
    imageBlob = await UploadAndSaveBlobAsync(imageFile);
    ad.ImageURL = imageBlob.Uri.ToString();
}
```

<span data-ttu-id="3802b-448">hello nästa exempel visar hello-kod som tar bort blobbar när du tar bort en annons.</span><span class="sxs-lookup"><span data-stu-id="3802b-448">hello next example shows hello code that deletes blobs when you delete an ad.</span></span>

```csharp
private async Task DeleteAdBlobsAsync(Ad ad)
{
    if (!string.IsNullOrWhiteSpace(ad.ImageURL))
    {
        Uri blobUri = new Uri(ad.ImageURL);
        await DeleteAdBlobAsync(blobUri);
    }
    if (!string.IsNullOrWhiteSpace(ad.ThumbnailURL))
    {
        Uri blobUri = new Uri(ad.ThumbnailURL);
        await DeleteAdBlobAsync(blobUri);
    }
}
private static async Task DeleteAdBlobAsync(Uri blobUri)
{
    string blobName = blobUri.Segments[blobUri.Segments.Length - 1];
    CloudBlockBlob blobToDelete = imagesBlobContainer.GetBlockBlobReference(blobName);
    await blobToDelete.DeleteAsync();
}
```

### <a name="contosoadsweb---viewsadindexcshtml-and-detailscshtml"></a><span data-ttu-id="3802b-449">ContosoAdsWeb – Views\Ad\Index.cshtml och Details.cshtml</span><span class="sxs-lookup"><span data-stu-id="3802b-449">ContosoAdsWeb - Views\Ad\Index.cshtml and Details.cshtml</span></span>
<span data-ttu-id="3802b-450">Hej *Index.cshtml* visar miniatyrbilder med hello andra ad-data.</span><span class="sxs-lookup"><span data-stu-id="3802b-450">hello *Index.cshtml* file displays thumbnails with hello other ad data.</span></span>

```razor
<img src="@Html.Raw(item.ThumbnailURL)" />
```

<span data-ttu-id="3802b-451">Hej *Details.cshtml* visar hello bilden.</span><span class="sxs-lookup"><span data-stu-id="3802b-451">hello *Details.cshtml* file displays hello full-size image.</span></span>

```razor
<img src="@Html.Raw(Model.ImageURL)" />
```

### <a name="contosoadsweb---viewsadcreatecshtml-and-editcshtml"></a><span data-ttu-id="3802b-452">ContosoAdsWeb – Views\Ad\Create.cshtml och Edit.cshtml</span><span class="sxs-lookup"><span data-stu-id="3802b-452">ContosoAdsWeb - Views\Ad\Create.cshtml and Edit.cshtml</span></span>
<span data-ttu-id="3802b-453">Hej *Create.cshtml* och *Edit.cshtml* filer ange kodningen som aktiverar hello controller tooget hello `HttpPostedFileBase` objekt.</span><span class="sxs-lookup"><span data-stu-id="3802b-453">hello *Create.cshtml* and *Edit.cshtml* files specify form encoding that enables hello controller tooget hello `HttpPostedFileBase` object.</span></span>

```razor
@using (Html.BeginForm("Create", "Ad", FormMethod.Post, new { enctype = "multipart/form-data" }))
```

<span data-ttu-id="3802b-454">En `<input>` element instruerar hello webbläsare tooprovide en dialogruta för filval.</span><span class="sxs-lookup"><span data-stu-id="3802b-454">An `<input>` element tells hello browser tooprovide a file selection dialog.</span></span>

```razor
<input type="file" name="imageFile" accept="image/*" class="form-control fileupload" />
```

### <a name="contosoadsworker---workerrolecs---onstart-method"></a><span data-ttu-id="3802b-455">ContosoAdsWorker – WorkerRole.cs – OnStart-metoden</span><span class="sxs-lookup"><span data-stu-id="3802b-455">ContosoAdsWorker - WorkerRole.cs - OnStart method</span></span>
<span data-ttu-id="3802b-456">hello Azure worker-rollen miljö anropar hello `OnStart` metod i hello `WorkerRole` klassen när hello worker-rollen är igång och anropar hello `Run` metod när hello `OnStart` metod har avslutats.</span><span class="sxs-lookup"><span data-stu-id="3802b-456">hello Azure worker role environment calls hello `OnStart` method in hello `WorkerRole` class when hello worker role is getting started, and it calls hello `Run` method when hello `OnStart` method finishes.</span></span>

<span data-ttu-id="3802b-457">Hej `OnStart` metoden hämtar hello databasanslutningssträngen från hello *.cscfg* filen och skickar den toohello Entity Framework DbContext-klassen.</span><span class="sxs-lookup"><span data-stu-id="3802b-457">hello `OnStart` method gets hello database connection string from hello *.cscfg* file and passes it toohello Entity Framework DbContext class.</span></span> <span data-ttu-id="3802b-458">hello SQLClient-leverantören används som standard så hello-providern inte har angetts toobe.</span><span class="sxs-lookup"><span data-stu-id="3802b-458">hello SQLClient provider is used by default, so hello provider does not have toobe specified.</span></span>

```csharp
var dbConnString = CloudConfigurationManager.GetSetting("ContosoAdsDbConnectionString");
db = new ContosoAdsContext(dbConnString);
```

<span data-ttu-id="3802b-459">Efter det hello metoden hämtar en referens toohello storage-konto och skapar hello blobbehållaren och kön om de inte finns.</span><span class="sxs-lookup"><span data-stu-id="3802b-459">After that, hello method gets a reference toohello storage account and creates hello blob container and queue if they don't exist.</span></span> <span data-ttu-id="3802b-460">hello-koden för som är liknande toowhat som du redan har sett i hello webbroll `Application_Start` metod.</span><span class="sxs-lookup"><span data-stu-id="3802b-460">hello code for that is similar toowhat you already saw in hello web role `Application_Start` method.</span></span>

### <a name="contosoadsworker---workerrolecs---run-method"></a><span data-ttu-id="3802b-461">ContosoAdsWorker – WorkerRole.cs – Run-metoden</span><span class="sxs-lookup"><span data-stu-id="3802b-461">ContosoAdsWorker - WorkerRole.cs - Run method</span></span>
<span data-ttu-id="3802b-462">Hej `Run` metoden anropas när hello `OnStart` metod har avslutat initieringsarbetet.</span><span class="sxs-lookup"><span data-stu-id="3802b-462">hello `Run` method is called when hello `OnStart` method finishes its initialization work.</span></span> <span data-ttu-id="3802b-463">hello metoden Kör en oändlig loop som söker efter nya Kömeddelanden och bearbetar dem när de tas emot.</span><span class="sxs-lookup"><span data-stu-id="3802b-463">hello method executes an infinite loop that watches for new queue messages and processes them when they arrive.</span></span>

```csharp
public override void Run()
{
    CloudQueueMessage msg = null;

    while (true)
    {
        try
        {
            msg = this.imagesQueue.GetMessage();
            if (msg != null)
            {
                ProcessQueueMessage(msg);
            }
            else
            {
                System.Threading.Thread.Sleep(1000);
            }
        }
        catch (StorageException e)
        {
            if (msg != null && msg.DequeueCount > 5)
            {
                this.imagesQueue.DeleteMessage(msg);
            }
            System.Threading.Thread.Sleep(5000);
        }
    }
}
```

<span data-ttu-id="3802b-464">Om inget kömeddelande hittades efter varje upprepning av loopen hello viloläge hello program i en sekund.</span><span class="sxs-lookup"><span data-stu-id="3802b-464">After each iteration of hello loop, if no queue message was found, hello program sleeps for a second.</span></span> <span data-ttu-id="3802b-465">Detta förhindrar att arbetsrollen hello medför överdriven CPU tid och lagring transaktionskostnader.</span><span class="sxs-lookup"><span data-stu-id="3802b-465">This prevents hello worker role from incurring excessive CPU time and storage transaction costs.</span></span> <span data-ttu-id="3802b-466">hello Microsoft Customer Advisory Team berättar om en utvecklare som glömde bort tooinclude, distribuerats tooproduction, och åkte på semester.</span><span class="sxs-lookup"><span data-stu-id="3802b-466">hello Microsoft Customer Advisory Team tells a story about a  developer who forgot tooinclude this, deployed tooproduction, and left for vacation.</span></span> <span data-ttu-id="3802b-467">När han fick tillbaka kostade hans misstag mer än hello semester.</span><span class="sxs-lookup"><span data-stu-id="3802b-467">When he got back, his oversight cost more than hello vacation.</span></span>

<span data-ttu-id="3802b-468">Ibland orsakar hello innehållet i ett kömeddelande ett fel i bearbetningen.</span><span class="sxs-lookup"><span data-stu-id="3802b-468">Sometimes hello content of a queue message causes an error in processing.</span></span> <span data-ttu-id="3802b-469">Detta kallas en *skadligt meddelande*, och om du precis har loggat ett fel och startas om hello loop, du oändlighet försöka tooprocess meddelandet.</span><span class="sxs-lookup"><span data-stu-id="3802b-469">This is called a *poison message*, and if you just logged an error and restarted hello loop, you could endlessly try tooprocess that message.</span></span>  <span data-ttu-id="3802b-470">Därför innehåller hello catch-blocket en if-sats som kontrollerar toosee hur många gånger hello appen har försökt tooprocess hello aktuella meddelandet och om den har mer än 5 gånger hello-meddelande tas bort från hello kö.</span><span class="sxs-lookup"><span data-stu-id="3802b-470">Therefore hello catch block includes an if statement that checks toosee how many times hello app has tried tooprocess hello current message, and if it has been more than 5 times, hello message is deleted from hello queue.</span></span>

<span data-ttu-id="3802b-471">`ProcessQueueMessage` anropas när ett kömeddelande hittas.</span><span class="sxs-lookup"><span data-stu-id="3802b-471">`ProcessQueueMessage` is called when a queue message is found.</span></span>

```csharp
private void ProcessQueueMessage(CloudQueueMessage msg)
{
    var adId = int.Parse(msg.AsString);
    Ad ad = db.Ads.Find(adId);
    if (ad == null)
    {
        throw new Exception(String.Format("AdId {0} not found, can't create thumbnail", adId.ToString()));
    }

    CloudBlockBlob inputBlob = this.imagesBlobContainer.GetBlockBlobReference(ad.ImageURL);

    string thumbnailName = Path.GetFileNameWithoutExtension(inputBlob.Name) + "thumb.jpg";
    CloudBlockBlob outputBlob = this.imagesBlobContainer.GetBlockBlobReference(thumbnailName);

    using (Stream input = inputBlob.OpenRead())
    using (Stream output = outputBlob.OpenWrite())
    {
        ConvertImageToThumbnailJPG(input, output);
        outputBlob.Properties.ContentType = "image/jpeg";
    }

    ad.ThumbnailURL = outputBlob.Uri.ToString();
    db.SaveChanges();

    this.imagesQueue.DeleteMessage(msg);
}
```

<span data-ttu-id="3802b-472">Den här koden läser hello databasen tooget hello bild-URL, konverterar hello bildens tooa miniatyrbild, sparar hello miniatyren i en blob, uppdaterar hello databasen med hello miniatyr blob URL och tar bort hälsningsmeddelande för kön.</span><span class="sxs-lookup"><span data-stu-id="3802b-472">This code reads hello database tooget hello image URL, converts hello image tooa thumbnail, saves hello thumbnail in a blob, updates hello database with hello thumbnail blob URL, and deletes hello queue message.</span></span>

> [!NOTE]
> <span data-ttu-id="3802b-473">Hej koden i hello `ConvertImageToThumbnailJPG` metoden använder klasser i namnområdet för hello System.Drawing för enkelhetens skull.</span><span class="sxs-lookup"><span data-stu-id="3802b-473">hello code in hello `ConvertImageToThumbnailJPG` method uses classes in hello System.Drawing namespace for simplicity.</span></span> <span data-ttu-id="3802b-474">Dock har hello klasser i det här namnområdet utformats för användning med Windows Forms.</span><span class="sxs-lookup"><span data-stu-id="3802b-474">However, hello classes in this namespace were designed for use with Windows Forms.</span></span> <span data-ttu-id="3802b-475">De stöds inte för användning i en Windows- eller ASP.NET-tjänst.</span><span class="sxs-lookup"><span data-stu-id="3802b-475">They are not supported for use in a Windows or ASP.NET service.</span></span> <span data-ttu-id="3802b-476">Mer information om alternativ för bildbearbetning finns i [Dynamic Image Generation](http://www.hanselman.com/blog/BackToBasicsDynamicImageGenerationASPNETControllersRoutingIHttpHandlersAndRunAllManagedModulesForAllRequests.aspx) och [Deep Inside Image Resizing](http://www.hanselminutes.com/313/deep-inside-image-resizing-and-scaling-with-aspnet-and-iis-with-imageresizingnet-author-na).</span><span class="sxs-lookup"><span data-stu-id="3802b-476">For more information about image-processing options, see [Dynamic Image Generation](http://www.hanselman.com/blog/BackToBasicsDynamicImageGenerationASPNETControllersRoutingIHttpHandlersAndRunAllManagedModulesForAllRequests.aspx) and [Deep Inside Image Resizing](http://www.hanselminutes.com/313/deep-inside-image-resizing-and-scaling-with-aspnet-and-iis-with-imageresizingnet-author-na).</span></span>
>
>

## <a name="troubleshooting"></a><span data-ttu-id="3802b-477">Felsökning</span><span class="sxs-lookup"><span data-stu-id="3802b-477">Troubleshooting</span></span>
<span data-ttu-id="3802b-478">Om något inte fungerar när du följer hello instruktioner i den här självstudiekursen, här är några vanliga fel och hur tooresolve dem.</span><span class="sxs-lookup"><span data-stu-id="3802b-478">In case something doesn't work while you're following hello instructions in this tutorial, here are some common errors and how tooresolve them.</span></span>

### <a name="serviceruntimeroleenvironmentexception"></a><span data-ttu-id="3802b-479">ServiceRuntime.RoleEnvironmentException</span><span class="sxs-lookup"><span data-stu-id="3802b-479">ServiceRuntime.RoleEnvironmentException</span></span>
<span data-ttu-id="3802b-480">Hej `RoleEnvironment` tillhandahålls av Azure när du kör ett program i Azure eller när du kör lokalt med hjälp av hello Azure-beräkningsemulatorn.</span><span class="sxs-lookup"><span data-stu-id="3802b-480">hello `RoleEnvironment` object is provided by Azure when you run an application in Azure or when you run locally using hello Azure compute emulator.</span></span>  <span data-ttu-id="3802b-481">Om du får detta felmeddelande när du kör lokalt, kontrollerar du att du har angett hello ContosoAdsCloudService-projektet som Startprojekt hello.</span><span class="sxs-lookup"><span data-stu-id="3802b-481">If you get this error when you're running locally, make sure that you have set hello ContosoAdsCloudService project as hello startup project.</span></span> <span data-ttu-id="3802b-482">Detta konfigurerar hello projektet toorun med hello Azure-beräkningsemulatorn.</span><span class="sxs-lookup"><span data-stu-id="3802b-482">This sets up hello project toorun using hello Azure compute emulator.</span></span>

<span data-ttu-id="3802b-483">Hello saker hello program använder hello Azure RoleEnvironment för är tooget hello anslutningssträngarnas värden som lagras i hello *.cscfg* filerna, så en annan orsak till det här undantaget är en anslutningssträng som saknas.</span><span class="sxs-lookup"><span data-stu-id="3802b-483">One of hello things hello application uses hello Azure RoleEnvironment for is tooget hello connection string values that are stored in hello *.cscfg* files, so another cause of this exception is a missing connection string.</span></span> <span data-ttu-id="3802b-484">Kontrollera att som du skapade hello StorageConnectionString-inställningen både för moln och lokala konfigurationer i ContosoAdsWeb-projektet hello och att du har skapat båda anslutningssträngar för båda konfigurationerna i ContosoAdsWorker-projektet hello.</span><span class="sxs-lookup"><span data-stu-id="3802b-484">Make sure that you created hello StorageConnectionString setting for both Cloud and Local configurations in hello ContosoAdsWeb project, and that you created both connection strings for both configurations in hello ContosoAdsWorker project.</span></span> <span data-ttu-id="3802b-485">Om du gör en **Sök alla** Sök efter StorageConnectionString i hela hello-lösning bör du se den 9 gånger i 6-filer.</span><span class="sxs-lookup"><span data-stu-id="3802b-485">If you do a **Find All** search for StorageConnectionString in hello entire solution, you should see it 9 times in 6 files.</span></span>

### <a name="cannot-override-tooport-xxx-new-port-below-minimum-allowed-value-8080-for-protocol-http"></a><span data-ttu-id="3802b-486">Det går inte att åsidosätta tooport xxx.</span><span class="sxs-lookup"><span data-stu-id="3802b-486">Cannot override tooport xxx.</span></span> <span data-ttu-id="3802b-487">New port below minimum allowed value 8080 for protocol http. (Ny port under minsta tillåtna värde 8080 för protokollet http.)</span><span class="sxs-lookup"><span data-stu-id="3802b-487">New port below minimum allowed value 8080 for protocol http</span></span>
<span data-ttu-id="3802b-488">Försök att ändra hello-portnumret som används av hello webbprojekt.</span><span class="sxs-lookup"><span data-stu-id="3802b-488">Try changing hello port number used by hello web project.</span></span> <span data-ttu-id="3802b-489">Högerklicka på hello ContosoAdsWeb-projektet och klicka sedan på **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="3802b-489">Right-click hello ContosoAdsWeb project, and then click **Properties**.</span></span> <span data-ttu-id="3802b-490">Klicka på hello **Web** fliken och sedan ändra hello portnumret i hello **Url för** inställningen.</span><span class="sxs-lookup"><span data-stu-id="3802b-490">Click hello **Web** tab, and then change hello port number in hello **Project Url** setting.</span></span>

<span data-ttu-id="3802b-491">Ett annat alternativ som kan lösa problemet hello finns hello efter avsnittet.</span><span class="sxs-lookup"><span data-stu-id="3802b-491">For another alternative that might resolve hello problem, see hello following  section.</span></span>

### <a name="other-errors-when-running-locally"></a><span data-ttu-id="3802b-492">Andra fel när du kör programmet lokalt</span><span class="sxs-lookup"><span data-stu-id="3802b-492">Other errors when running locally</span></span>
<span data-ttu-id="3802b-493">Serviceprojekt använder som standard för nya molntjänster hello Azure compute emulator express toosimulate hello Azure-miljön.</span><span class="sxs-lookup"><span data-stu-id="3802b-493">By default new cloud service projects use hello Azure compute emulator express toosimulate hello Azure environment.</span></span> <span data-ttu-id="3802b-494">Detta är en förenklad version av hello fullständiga beräkningsemulatorn och under vissa förhållanden hello fullständiga emulatorn fungerar när hello express-version som inte.</span><span class="sxs-lookup"><span data-stu-id="3802b-494">This is a lightweight version of hello full compute emulator, and under some conditions hello full emulator will work when hello express version does not.</span></span>  

<span data-ttu-id="3802b-495">toochange hello projektet toouse hello fullständiga emulatorn, högerklickar hello ContosoAdsCloudService-projektet och klicka sedan på **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="3802b-495">toochange hello project toouse hello full emulator, right-click hello ContosoAdsCloudService project, and then click **Properties**.</span></span> <span data-ttu-id="3802b-496">I hello **egenskaper** fönstret klickar du på hello **Web** fliken och klicka sedan på hello **Använd fullständig Emulator** knappen.</span><span class="sxs-lookup"><span data-stu-id="3802b-496">In hello **Properties** window click hello **Web** tab, and then click hello **Use Full Emulator** radio button.</span></span>

<span data-ttu-id="3802b-497">I ordning toorun hello program med hello fullständiga emulatorn har du tooopen Visual Studio med administratörsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="3802b-497">In order toorun hello application with hello full emulator, you have tooopen Visual Studio with administrator privileges.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3802b-498">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3802b-498">Next steps</span></span>
<span data-ttu-id="3802b-499">hello Contoso Ads-programmet har avsikt förenklats för en komma igång-kursen.</span><span class="sxs-lookup"><span data-stu-id="3802b-499">hello Contoso Ads application has intentionally been kept simple for a getting-started tutorial.</span></span> <span data-ttu-id="3802b-500">Till exempel den implementerar inte [beroendeinmatning](http://www.asp.net/mvc/tutorials/hands-on-labs/aspnet-mvc-4-dependency-injection) eller hello [centrallager och arbetsenhetsmönster](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application#repo), inte [används ett gränssnitt för loggning](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry#log), den använder inte [ EF Code First Migrations](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application) toomanage datamodell ändringar eller [EF Connection Resiliency](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application) toomanage tillfälligt nätverksfel och så vidare.</span><span class="sxs-lookup"><span data-stu-id="3802b-500">For example, it doesn't implement [dependency injection](http://www.asp.net/mvc/tutorials/hands-on-labs/aspnet-mvc-4-dependency-injection) or hello [repository and unit of work patterns](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application#repo), it doesn't [use an interface for logging](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry#log), it doesn't use [EF Code First Migrations](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application) toomanage data model changes or [EF Connection Resiliency](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application) toomanage transient network errors, and so forth.</span></span>

<span data-ttu-id="3802b-501">Här följer några exempelprogram för molntjänster som visar fler verklighetsbaserade kodningsexempel, i från mindre komplex toomore komplexa:</span><span class="sxs-lookup"><span data-stu-id="3802b-501">Here are some cloud service sample applications that demonstrate more real-world coding practices, listed from less complex toomore complex:</span></span>

* <span data-ttu-id="3802b-502">[PhluffyFotos](http://code.msdn.microsoft.com/PhluffyFotos-Sample-7ecffd31).</span><span class="sxs-lookup"><span data-stu-id="3802b-502">[PhluffyFotos](http://code.msdn.microsoft.com/PhluffyFotos-Sample-7ecffd31).</span></span> <span data-ttu-id="3802b-503">Liknande koncept tooContoso Ads men implementerar flera funktioner och fler verklighetsbaserade kodningsexempel.</span><span class="sxs-lookup"><span data-stu-id="3802b-503">Similar in concept tooContoso Ads but implements more features and more real-world coding practices.</span></span>
* <span data-ttu-id="3802b-504">[Azure Cloud Service Multi-Tier Application with Tables, Queues, and Blobs](http://code.msdn.microsoft.com/windowsazure/Windows-Azure-Multi-Tier-eadceb36).</span><span class="sxs-lookup"><span data-stu-id="3802b-504">[Azure Cloud Service Multi-Tier Application with Tables, Queues, and Blobs](http://code.msdn.microsoft.com/windowsazure/Windows-Azure-Multi-Tier-eadceb36).</span></span> <span data-ttu-id="3802b-505">Introducerar Azure Storage-tabeller samt blobbar och köer.</span><span class="sxs-lookup"><span data-stu-id="3802b-505">Introduces Azure Storage tables as well as blobs and queues.</span></span> <span data-ttu-id="3802b-506">Baserat på en äldre version av hello Azure SDK för .NET, kräver vissa ändringar toowork med hello nuvarande version.</span><span class="sxs-lookup"><span data-stu-id="3802b-506">Based on an older version of hello Azure SDK for .NET, will require some modifications toowork with hello current version.</span></span>
* <span data-ttu-id="3802b-507">[Cloud Service Fundamentals in Microsoft Azure](http://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649).</span><span class="sxs-lookup"><span data-stu-id="3802b-507">[Cloud Service Fundamentals in Microsoft Azure](http://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649).</span></span> <span data-ttu-id="3802b-508">Ett omfattande exempel som visar ett stort urval med bästa arbetsmetoder som produceras av hello Microsoft Patterns and Practices-gruppen.</span><span class="sxs-lookup"><span data-stu-id="3802b-508">A comprehensive sample demonstrating a wide range of best practices, produced by hello Microsoft Patterns and Practices group.</span></span>

<span data-ttu-id="3802b-509">Allmän information om hur du utvecklar för molnet hello finns [skapa verkliga Molnappar med Azure](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction).</span><span class="sxs-lookup"><span data-stu-id="3802b-509">For general information about developing for hello cloud, see [Building Real-World Cloud Apps with Azure](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction).</span></span>

<span data-ttu-id="3802b-510">Bästa metoder och mönster för en videopresentation tooAzure lagring, se [Microsoft Azure Storage – What's New, Best Practices and Patterns](http://channel9.msdn.com/Events/Build/2014/3-628).</span><span class="sxs-lookup"><span data-stu-id="3802b-510">For a video introduction tooAzure Storage best practices and patterns, see [Microsoft Azure Storage – What's New, Best Practices and Patterns](http://channel9.msdn.com/Events/Build/2014/3-628).</span></span>

<span data-ttu-id="3802b-511">Mer information finns i hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="3802b-511">For more information, see hello following resources:</span></span>

* [<span data-ttu-id="3802b-512">Azure Cloud Services, del 1: Inledning</span><span class="sxs-lookup"><span data-stu-id="3802b-512">Azure Cloud Services Part 1: Introduction</span></span>](http://justazure.com/microsoft-azure-cloud-services-part-1-introduction/)
* [<span data-ttu-id="3802b-513">Hur toomanage Cloud Services</span><span class="sxs-lookup"><span data-stu-id="3802b-513">How toomanage Cloud Services</span></span>](cloud-services-how-to-manage.md)
* [<span data-ttu-id="3802b-514">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="3802b-514">Azure Storage</span></span>](/documentation/services/storage/)
* [<span data-ttu-id="3802b-515">Hur toochoose ett moln-leverantörer</span><span class="sxs-lookup"><span data-stu-id="3802b-515">How toochoose a cloud service provider</span></span>](https://azure.microsoft.com/overview/choosing-a-cloud-service-provider/)
