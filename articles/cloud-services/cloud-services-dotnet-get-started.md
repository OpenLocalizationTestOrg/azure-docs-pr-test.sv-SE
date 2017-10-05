---
title: "Kom igång med Azure Cloud Services och ASP.NET | Microsoft Docs"
description: "Lär dig hur du kan skapa en app för flera nivåer med ASP.NET MVC och Azure. Appen körs i en molntjänst med en webbroll och en arbetsroll. Appen använder Entity Framework, SQL Database och Azure Storage-köer och -blobbar."
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
ms.openlocfilehash: d2c197db73477d06d86d1c4faa8c4a2f58c7b391
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-azure-cloud-services-and-aspnet"></a><span data-ttu-id="c322c-105">Kom igång med Azure Cloud Services och ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c322c-105">Get started with Azure Cloud Services and ASP.NET</span></span>

## <a name="overview"></a><span data-ttu-id="c322c-106">Översikt</span><span class="sxs-lookup"><span data-stu-id="c322c-106">Overview</span></span>
<span data-ttu-id="c322c-107">Under den här kursen får du lära dig hur du skapar ett .NET-program på flera nivåer med en ASP.NET MVC-klientdel, samt att distribuera det till en [Azure-molntjänst](cloud-services-choose-me.md).</span><span class="sxs-lookup"><span data-stu-id="c322c-107">This tutorial shows how to create a multi-tier .NET application with an ASP.NET MVC front-end, and deploy it to an [Azure cloud service](cloud-services-choose-me.md).</span></span> <span data-ttu-id="c322c-108">Programmet använder [Azure SQL Database](http://msdn.microsoft.com/library/azure/ee336279), [Azure Blob-tjänsten](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage) och [Azure-kötjänsten](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern).</span><span class="sxs-lookup"><span data-stu-id="c322c-108">The application uses [Azure SQL Database](http://msdn.microsoft.com/library/azure/ee336279), the [Azure Blob service](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage), and the [Azure Queue service](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern).</span></span> <span data-ttu-id="c322c-109">Du kan [hämta Visual Studio-projektet](http://code.msdn.microsoft.com/Simple-Azure-Cloud-Service-e01df2e4) från MSDN Code Gallery.</span><span class="sxs-lookup"><span data-stu-id="c322c-109">You can [download the Visual Studio project](http://code.msdn.microsoft.com/Simple-Azure-Cloud-Service-e01df2e4) from the MSDN Code Gallery.</span></span>

<span data-ttu-id="c322c-110">Under kursen får du lära dig hur du skapar och kör programmet lokalt, hur du distribuerar det till Azure och kör det i molnet, och hur du skapar det från grunden.</span><span class="sxs-lookup"><span data-stu-id="c322c-110">The tutorial shows you how to build and run the application locally, how to deploy it to Azure and run in the cloud, and how to build it from scratch.</span></span> <span data-ttu-id="c322c-111">Du kan börja med att skapa från grunden och sedan göra test- och distributionsstegen efteråt om du föredrar det.</span><span class="sxs-lookup"><span data-stu-id="c322c-111">You can start by building from scratch and then do the test and deploy steps afterward if you prefer.</span></span>

## <a name="contoso-ads-application"></a><span data-ttu-id="c322c-112">Contoso Ads-program</span><span class="sxs-lookup"><span data-stu-id="c322c-112">Contoso Ads application</span></span>
<span data-ttu-id="c322c-113">Programmet är en anslagstavla för annonser.</span><span class="sxs-lookup"><span data-stu-id="c322c-113">The application is an advertising bulletin board.</span></span> <span data-ttu-id="c322c-114">Användare skapar en annons genom att skriva in text och ladda upp en bild.</span><span class="sxs-lookup"><span data-stu-id="c322c-114">Users create an ad by entering text and uploading an image.</span></span> <span data-ttu-id="c322c-115">De kan se en lista över annonser med miniatyrbilder, och de kan se bilden i full storlek när de markerar en annons för att se detaljerna.</span><span class="sxs-lookup"><span data-stu-id="c322c-115">They can see a list of ads with thumbnail images, and they can see the full-size image when they select an ad to see the details.</span></span>

![Annonslista](./media/cloud-services-dotnet-get-started/list.png)

<span data-ttu-id="c322c-117">Programmet använder det [köcentriska arbetsmönstret](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern) för att avlasta det processorintensiva arbetet med att skapa miniatyrbilder till en serverdelsprocess.</span><span class="sxs-lookup"><span data-stu-id="c322c-117">The application uses the [queue-centric work pattern](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern) to off-load the CPU-intensive work of creating thumbnails to a back-end process.</span></span>

## <a name="alternative-architecture-websites-and-webjobs"></a><span data-ttu-id="c322c-118">Alternativ arkitektur: Websites och WebJobs</span><span class="sxs-lookup"><span data-stu-id="c322c-118">Alternative architecture: Websites and WebJobs</span></span>
<span data-ttu-id="c322c-119">Under den här kursen får du lära dig hur du kör både klient- och serverdelen i en Azure-molntjänst.</span><span class="sxs-lookup"><span data-stu-id="c322c-119">This tutorial shows how to run both front-end and back-end in an Azure cloud service.</span></span> <span data-ttu-id="c322c-120">Ett alternativ är att köra klientdelen i en [Azure-webbplats](/services/web-sites/) och använda [WebJobs](http://go.microsoft.com/fwlink/?LinkId=390226)-funktionen (för närvarande i förhandsversion) som serverdel.</span><span class="sxs-lookup"><span data-stu-id="c322c-120">An alternative is to run the front-end in an [Azure website](/services/web-sites/) and use the [WebJobs](http://go.microsoft.com/fwlink/?LinkId=390226) feature (currently in preview) for the back-end.</span></span> <span data-ttu-id="c322c-121">Om du vill följa en kurs som använder WebJobs går du till [Kom igång med Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="c322c-121">For a tutorial that uses WebJobs, see [Get Started with the Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-get-started.md).</span></span> <span data-ttu-id="c322c-122">Mer information om hur du väljer de tjänster som bäst passar din situation finns i [Jämförelse mellan Azure Websites, Cloud Services och Virtual Machines](../app-service-web/choose-web-site-cloud-service-vm.md).</span><span class="sxs-lookup"><span data-stu-id="c322c-122">For information about how to choose the services that best fit your scenario, see [Azure Websites, Cloud Services, and virtual machines comparison](../app-service-web/choose-web-site-cloud-service-vm.md).</span></span>

## <a name="what-youll-learn"></a><span data-ttu-id="c322c-123">Det här får du lära du dig</span><span class="sxs-lookup"><span data-stu-id="c322c-123">What you'll learn</span></span>
* <span data-ttu-id="c322c-124">Hur du aktiverar datorn för Azure-utveckling genom att installera Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="c322c-124">How to enable your machine for Azure development by installing the Azure SDK.</span></span>
* <span data-ttu-id="c322c-125">Hur du skapar ett Visual Studio-molntjänstprojekt med en ASP.NET MVC-webbroll och en arbetsroll.</span><span class="sxs-lookup"><span data-stu-id="c322c-125">How to create a Visual Studio cloud service project with an ASP.NET MVC web role and a worker role.</span></span>
* <span data-ttu-id="c322c-126">Hur du testar molntjänstprojektet lokalt med hjälp av Azure-lagringsemulatorn.</span><span class="sxs-lookup"><span data-stu-id="c322c-126">How to test the cloud service project locally, using the Azure storage emulator.</span></span>
* <span data-ttu-id="c322c-127">Hur du publicerar molnprojektet på en Azure-molntjänst och testar det med ett Azure-lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="c322c-127">How to publish the cloud project to an Azure cloud service and test using an Azure storage account.</span></span>
* <span data-ttu-id="c322c-128">Hur du laddar upp filer och lagrar dem i Azure Blob-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="c322c-128">How to upload files and store them in the Azure Blob service.</span></span>
* <span data-ttu-id="c322c-129">Hur du använder Azure-kötjänsten för kommunikation mellan nivåer.</span><span class="sxs-lookup"><span data-stu-id="c322c-129">How to use the Azure Queue service for communication between tiers.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c322c-130">Krav</span><span class="sxs-lookup"><span data-stu-id="c322c-130">Prerequisites</span></span>
<span data-ttu-id="c322c-131">Kursen förutsätter att du förstår [grundläggande koncept om Azure-molntjänster](cloud-services-choose-me.md), t.ex. termerna *webbroll* och *arbetsroll*.</span><span class="sxs-lookup"><span data-stu-id="c322c-131">The tutorial assumes that you understand [basic concepts about Azure cloud services](cloud-services-choose-me.md) such as *web role* and *worker role* terminology.</span></span>  <span data-ttu-id="c322c-132">Det förutsätts även att du kan använda [ASP.NET MVC](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started)- eller [Web Forms](http://www.asp.net/web-forms/tutorials/aspnet-45/getting-started-with-aspnet-45-web-forms/introduction-and-overview)-projekt i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c322c-132">It also assumes that you know how to work with [ASP.NET MVC](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started) or [Web Forms](http://www.asp.net/web-forms/tutorials/aspnet-45/getting-started-with-aspnet-45-web-forms/introduction-and-overview) projects in Visual Studio.</span></span> <span data-ttu-id="c322c-133">Exempelprogrammet använder MVC, men större delen av kursen gäller också Web Forms.</span><span class="sxs-lookup"><span data-stu-id="c322c-133">The sample application uses MVC, but most of the tutorial also applies to Web Forms.</span></span>

<span data-ttu-id="c322c-134">Du kan köra appen lokalt utan en Azure-prenumeration, men du behöver en prenumeration för att kunna distribuera programmet i molnet.</span><span class="sxs-lookup"><span data-stu-id="c322c-134">You can run the app locally without an Azure subscription, but you'll need one to deploy the application to the cloud.</span></span> <span data-ttu-id="c322c-135">Om du inte har ett konto kan du [aktivera MSDN-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A55E3C668) eller [registrera dig för en kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A55E3C668).</span><span class="sxs-lookup"><span data-stu-id="c322c-135">If you don't have an account, you can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A55E3C668) or [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A55E3C668).</span></span>

<span data-ttu-id="c322c-136">Anvisningarna i kursen gäller båda följande produkter:</span><span class="sxs-lookup"><span data-stu-id="c322c-136">The tutorial instructions work with either of the following products:</span></span>

* <span data-ttu-id="c322c-137">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="c322c-137">Visual Studio 2013</span></span>
* <span data-ttu-id="c322c-138">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="c322c-138">Visual Studio 2015</span></span>
* <span data-ttu-id="c322c-139">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="c322c-139">Visual Studio 2017</span></span>

<span data-ttu-id="c322c-140">Om du inte har någon av dessa kan Visual Studio installeras automatiskt när du installerar Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="c322c-140">If you don't have one of these, Visual Studio may be installed automatically when you install the Azure SDK.</span></span>

## <a name="application-architecture"></a><span data-ttu-id="c322c-141">Programarkitektur</span><span class="sxs-lookup"><span data-stu-id="c322c-141">Application architecture</span></span>
<span data-ttu-id="c322c-142">Appen lagrar annonser i en SQL-databas och använder Entity Framework Code First för att skapa tabellerna och komma åt data.</span><span class="sxs-lookup"><span data-stu-id="c322c-142">The app stores ads in a SQL database, using Entity Framework Code First to create the tables and access the data.</span></span> <span data-ttu-id="c322c-143">Databasen lagrar två URL:er för varje annons, en för bilden i full storlek och en för miniatyrbilden.</span><span class="sxs-lookup"><span data-stu-id="c322c-143">For each ad, the database stores two URLs, one for the full-size image and one for the thumbnail.</span></span>

![Annonstabell](./media/cloud-services-dotnet-get-started/adtable.png)

<span data-ttu-id="c322c-145">När en användare laddar upp en bild, lagrar klientdelen som körs i en webbroll bilden i en [Azure-blob](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage), och den lagrar annonsinformationen i databasen med en URL som pekar på blobben.</span><span class="sxs-lookup"><span data-stu-id="c322c-145">When a user uploads an image, the front-end running in a web role stores the image in an [Azure blob](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage), and it stores the ad information in the database with a URL that points to the blob.</span></span> <span data-ttu-id="c322c-146">Samtidigt skriver den ett meddelande till en Azure-kö.</span><span class="sxs-lookup"><span data-stu-id="c322c-146">At the same time, it writes a message to an Azure queue.</span></span> <span data-ttu-id="c322c-147">En serverdelsprocess som körs i en arbetsroll söker regelbundet i kön efter nya meddelanden.</span><span class="sxs-lookup"><span data-stu-id="c322c-147">A back-end process running in a worker role periodically polls the queue for new messages.</span></span> <span data-ttu-id="c322c-148">När ett nytt meddelande dyker upp, skapar arbetsrollen en miniatyrbild för den bilden och uppdaterar miniatyrbildens URL-databasfält för den annonsen.</span><span class="sxs-lookup"><span data-stu-id="c322c-148">When a new message appears, the worker role creates a thumbnail for that image and updates the thumbnail URL database field for that ad.</span></span> <span data-ttu-id="c322c-149">I följande diagram visas hur programmets olika delar fungerar tillsammans.</span><span class="sxs-lookup"><span data-stu-id="c322c-149">The following diagram shows how the parts of the application interact.</span></span>

![Contoso Ads-arkitektur](./media/cloud-services-dotnet-get-started/apparchitecture.png)

[!INCLUDE [install-sdk](../../includes/install-sdk-2017-2015-2013.md)]

## <a name="download-and-run-the-completed-solution"></a><span data-ttu-id="c322c-151">Hämta och köra den färdiga lösningen</span><span class="sxs-lookup"><span data-stu-id="c322c-151">Download and run the completed solution</span></span>
1. <span data-ttu-id="c322c-152">Hämta och packa upp den [färdiga lösningen](http://code.msdn.microsoft.com/Simple-Azure-Cloud-Service-e01df2e4).</span><span class="sxs-lookup"><span data-stu-id="c322c-152">Download and unzip the [completed solution](http://code.msdn.microsoft.com/Simple-Azure-Cloud-Service-e01df2e4).</span></span>
2. <span data-ttu-id="c322c-153">Starta Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c322c-153">Start Visual Studio.</span></span>
3. <span data-ttu-id="c322c-154">Gå till **File** (Arkiv-menyn) och välj **Open project** (Öppna projekt), navigera till platsen där du sparade den hämtade lösningen och öppna sedan lösningsfilen.</span><span class="sxs-lookup"><span data-stu-id="c322c-154">From the **File** menu choose **Open Project**, navigate to where you downloaded the solution, and then open the solution file.</span></span>
4. <span data-ttu-id="c322c-155">Tryck på CTRL+SKIFT+B för att skapa lösningen.</span><span class="sxs-lookup"><span data-stu-id="c322c-155">Press CTRL+SHIFT+B to build the solution.</span></span>

    <span data-ttu-id="c322c-156">Som standard återställer Visual Studio automatiskt NuGet-paketinnehållet som inte ingick i *.zip*-filen.</span><span class="sxs-lookup"><span data-stu-id="c322c-156">By default, Visual Studio automatically restores the NuGet package content, which was not included in the *.zip* file.</span></span> <span data-ttu-id="c322c-157">Om paketinnehållet inte återställs kan du installera det manuellt genom att öppna dialogrutan **Manage NuGet Packages for Solution** (Hantera NuGet-paket för lösningen) och klicka på knappen **Restore** (Återställ) högst upp till höger.</span><span class="sxs-lookup"><span data-stu-id="c322c-157">If the packages don't restore, install them manually by going to the **Manage NuGet Packages for Solution** dialog box and clicking the **Restore** button at the top right.</span></span>
5. <span data-ttu-id="c322c-158">I **Solution Explorer** ska du kontrollera att **ContosoAdsCloudService** har markerats som startprojekt.</span><span class="sxs-lookup"><span data-stu-id="c322c-158">In **Solution Explorer**, make sure that **ContosoAdsCloudService** is selected as the startup project.</span></span>
6. <span data-ttu-id="c322c-159">Om du använder Visual Studio 2015 eller högre, ska du ändra SQL Server-anslutningssträngen i filen *Web.config* i ContosoAdsWeb-projektet samt i filen *ServiceConfiguration.Local.cscfg* i ContosoAdsCloudService-projektet.</span><span class="sxs-lookup"><span data-stu-id="c322c-159">If you're using Visual Studio 2015 or higher, change the SQL Server connection string in the application *Web.config* file of the ContosoAdsWeb project and in the *ServiceConfiguration.Local.cscfg* file of the ContosoAdsCloudService project.</span></span> <span data-ttu-id="c322c-160">I båda fallen ska du ändra ”(localdb)\v11.0” till ”(localdb)\MSSQLLocalDB”.</span><span class="sxs-lookup"><span data-stu-id="c322c-160">In each case, change "(localdb)\v11.0" to "(localdb)\MSSQLLocalDB".</span></span>
7. <span data-ttu-id="c322c-161">Tryck på CTRL+F5 för att köra programmet.</span><span class="sxs-lookup"><span data-stu-id="c322c-161">Press CTRL+F5 to run the application.</span></span>

    <span data-ttu-id="c322c-162">När du kör ett molntjänstprojekt lokalt anropar Visual Studio automatiskt *beräkningsemulatorn* och *lagringsemulatorn* i Azure.</span><span class="sxs-lookup"><span data-stu-id="c322c-162">When you run a cloud service project locally, Visual Studio automatically invokes the Azure *compute emulator* and Azure *storage emulator*.</span></span> <span data-ttu-id="c322c-163">Beräkningsemulatorn använder datorns resurser för att simulera webbrolls- och arbetsrollsmiljöerna.</span><span class="sxs-lookup"><span data-stu-id="c322c-163">The compute emulator uses your computer's resources to simulate the web role and worker role environments.</span></span> <span data-ttu-id="c322c-164">Lagringsemulatorn använder en [SQL Server Express LocalDB](http://msdn.microsoft.com/library/hh510202.aspx)-databas för att simulera Azure-molnlagring.</span><span class="sxs-lookup"><span data-stu-id="c322c-164">The storage emulator uses a [SQL Server Express LocalDB](http://msdn.microsoft.com/library/hh510202.aspx) database to simulate Azure cloud storage.</span></span>

    <span data-ttu-id="c322c-165">Första gången du kör ett molntjänstprojekt tar det ungefär en minut för emulatorerna att starta.</span><span class="sxs-lookup"><span data-stu-id="c322c-165">The first time you run a cloud service project, it takes a minute or so for the emulators to start up.</span></span> <span data-ttu-id="c322c-166">När emulatorerna har startats öppnas standardwebbläsaren med programmets startsida.</span><span class="sxs-lookup"><span data-stu-id="c322c-166">When emulator startup is finished, the default browser opens to the application home page.</span></span>

    ![Contoso Ads-arkitektur](./media/cloud-services-dotnet-get-started/home.png)
8. <span data-ttu-id="c322c-168">Klicka på **Create an Ad** (Skapa en annons).</span><span class="sxs-lookup"><span data-stu-id="c322c-168">Click  **Create an Ad**.</span></span>
9. <span data-ttu-id="c322c-169">Ange lite testdata och välj en *.jpg*-bild som ska laddas upp, och klicka sedan på **Create** (Skapa).</span><span class="sxs-lookup"><span data-stu-id="c322c-169">Enter some test data and select a *.jpg* image to upload, and then click **Create**.</span></span>

    ![Sidan Create (Skapa)](./media/cloud-services-dotnet-get-started/create.png)

    <span data-ttu-id="c322c-171">Appen går till indexsidan, men den visar inte en miniatyrbild för den nya annonsen eftersom den bearbetningen inte har utförts än.</span><span class="sxs-lookup"><span data-stu-id="c322c-171">The app goes to the Index page, but it doesn't show a thumbnail for the new ad because that processing hasn't happened yet.</span></span>
10. <span data-ttu-id="c322c-172">Vänta en stund och uppdatera sedan indexsidan om du vill se miniatyrbilden.</span><span class="sxs-lookup"><span data-stu-id="c322c-172">Wait a moment and then refresh the Index page to see the thumbnail.</span></span>

     ![Sidan Index](./media/cloud-services-dotnet-get-started/list.png)
11. <span data-ttu-id="c322c-174">Klicka på **Details** (Detaljer) för din annons om du vill se bilden i full storlek.</span><span class="sxs-lookup"><span data-stu-id="c322c-174">Click **Details** for your ad to see the full-size image.</span></span>

     ![Sidan Details (Detaljer)](./media/cloud-services-dotnet-get-started/details.png)

<span data-ttu-id="c322c-176">Du har kört programmet helt på din lokala dator utan anslutning till molnet.</span><span class="sxs-lookup"><span data-stu-id="c322c-176">You've been running the application entirely on your local computer, with no connection to the cloud.</span></span> <span data-ttu-id="c322c-177">Lagringsemulatorn lagrar kö- och blobbdata i en SQL Server Express LocalDB-databas, och programmet lagrar annonsdata i en annan LocalDB-databas.</span><span class="sxs-lookup"><span data-stu-id="c322c-177">The storage emulator stores the queue and blob data in a SQL Server Express LocalDB database, and the application stores the ad data in another LocalDB database.</span></span> <span data-ttu-id="c322c-178">Entity Framework Code First skapade automatiskt annonsdatabasen första gången webbappen försökte få tillgång till den.</span><span class="sxs-lookup"><span data-stu-id="c322c-178">Entity Framework Code First automatically created the ad database the first time the web app tried to access it.</span></span>

<span data-ttu-id="c322c-179">I följande avsnitt får du konfigurera lösningen så att den använder Azure-molnresurser för köer, blobbar och programdatabasen när den körs i molnet.</span><span class="sxs-lookup"><span data-stu-id="c322c-179">In the following section you'll configure the solution to use Azure cloud resources for queues, blobs, and the application database when it runs in the cloud.</span></span> <span data-ttu-id="c322c-180">Om du vill fortsätta att köra lösningen lokalt men använda molnet och databasresurser kan du göra det.</span><span class="sxs-lookup"><span data-stu-id="c322c-180">If you wanted to continue to run locally but use cloud storage and database resources, you could do that.</span></span> <span data-ttu-id="c322c-181">Det är bara att ställa in anslutningssträngar, vilket du får lära dig här.</span><span class="sxs-lookup"><span data-stu-id="c322c-181">It's just a matter of setting connection strings, which you'll see how to do.</span></span>

## <a name="deploy-the-application-to-azure"></a><span data-ttu-id="c322c-182">Distribuera programmet till Azure</span><span class="sxs-lookup"><span data-stu-id="c322c-182">Deploy the application to Azure</span></span>
<span data-ttu-id="c322c-183">Följ dessa steg för att köra programmet i molnet:</span><span class="sxs-lookup"><span data-stu-id="c322c-183">You'll do the following steps to run the application in the cloud:</span></span>

* <span data-ttu-id="c322c-184">Skapa en Azure-molntjänst.</span><span class="sxs-lookup"><span data-stu-id="c322c-184">Create an Azure cloud service.</span></span>
* <span data-ttu-id="c322c-185">Skapa en Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="c322c-185">Create an Azure SQL database.</span></span>
* <span data-ttu-id="c322c-186">Skapa ett Azure-lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="c322c-186">Create an Azure storage account.</span></span>
* <span data-ttu-id="c322c-187">Konfigurera lösningen så att den använder Azure SQL Database när den körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="c322c-187">Configure the solution to use your Azure SQL database when it runs in Azure.</span></span>
* <span data-ttu-id="c322c-188">Konfigurera lösningen så att den använder ditt Azure-lagringskonto när den körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="c322c-188">Configure the solution to use your Azure storage account when it runs in Azure.</span></span>
* <span data-ttu-id="c322c-189">Distribuera projektet till Azure-molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="c322c-189">Deploy the project to your Azure cloud service.</span></span>

### <a name="create-an-azure-cloud-service"></a><span data-ttu-id="c322c-190">Skapa en Azure-molntjänst</span><span class="sxs-lookup"><span data-stu-id="c322c-190">Create an Azure cloud service</span></span>
<span data-ttu-id="c322c-191">En Azure-molntjänst är den miljö som programmet kommer att köras i.</span><span class="sxs-lookup"><span data-stu-id="c322c-191">An Azure cloud service is the environment the application will run in.</span></span>

1. <span data-ttu-id="c322c-192">Öppna [Azure-portalen](https://portal.azure.com) i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="c322c-192">In your browser, open the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="c322c-193">Klicka på **New > Compute > Cloud Service (Ny > Compute > Molntjänst**.</span><span class="sxs-lookup"><span data-stu-id="c322c-193">Click **New > Compute > Cloud Service**.</span></span>

3. <span data-ttu-id="c322c-194">Ange ett URL-prefix för molntjänsten i textrutan för DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="c322c-194">In the DNS name input box, enter a URL prefix for the cloud service.</span></span>

    <span data-ttu-id="c322c-195">Den här URL:en måste vara unik.</span><span class="sxs-lookup"><span data-stu-id="c322c-195">This URL has to be unique.</span></span>  <span data-ttu-id="c322c-196">Du får ett felmeddelande om det prefix du har valt redan används.</span><span class="sxs-lookup"><span data-stu-id="c322c-196">You'll get an error message if the prefix you choose is already in use.</span></span>
4. <span data-ttu-id="c322c-197">Ange en ny resursgrupp för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="c322c-197">Specify a new Resource group for the  service.</span></span> <span data-ttu-id="c322c-198">Klicka på **Skapa nytt** och ange ett namn i textrutan för resursgruppen, till exempel CS_contososadsRG.</span><span class="sxs-lookup"><span data-stu-id="c322c-198">Click **Create new** and then type a name in the Resource group input box, such as CS_contososadsRG.</span></span>

5. <span data-ttu-id="c322c-199">Välj den region där du vill distribuera programmet.</span><span class="sxs-lookup"><span data-stu-id="c322c-199">Choose the region where you want to deploy the application.</span></span>

    <span data-ttu-id="c322c-200">Det här fältet anger vilket datacenter som ska vara värd åt molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="c322c-200">This field specifies which datacenter your cloud service will be hosted in.</span></span> <span data-ttu-id="c322c-201">Om det gäller ett produktionsprogram väljer du den region som ligger närmast dina kunder.</span><span class="sxs-lookup"><span data-stu-id="c322c-201">For a production application, you'd choose the region closest to your customers.</span></span> <span data-ttu-id="c322c-202">Under den här kursen väljer du den region som ligger närmast dig.</span><span class="sxs-lookup"><span data-stu-id="c322c-202">For this tutorial, choose the region closest to you.</span></span>
5. <span data-ttu-id="c322c-203">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="c322c-203">Click **Create**.</span></span>

    <span data-ttu-id="c322c-204">I följande bild skapas en molntjänst med URL:en CSvccontosoads.cloudapp.net.</span><span class="sxs-lookup"><span data-stu-id="c322c-204">In the following image, a cloud service is created with the URL CSvccontosoads.cloudapp.net.</span></span>

    ![Ny molntjänst](./media/cloud-services-dotnet-get-started/newcs.png)

### <a name="create-an-azure-sql-database"></a><span data-ttu-id="c322c-206">Skapa en Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="c322c-206">Create an Azure SQL database</span></span>
<span data-ttu-id="c322c-207">När appen körs i molnet använder den en molnbaserad databas.</span><span class="sxs-lookup"><span data-stu-id="c322c-207">When the app runs in the cloud, it will use a cloud-based database.</span></span>

1. <span data-ttu-id="c322c-208">I [Azure-portalen](https://portal.azure.com) klickar du på **New > databaser > SQL Database**.</span><span class="sxs-lookup"><span data-stu-id="c322c-208">In the [Azure portal](https://portal.azure.com), click **New > Databases > SQL Database**.</span></span>
2. <span data-ttu-id="c322c-209">Ange *contosoads* i rutan **Database Name** (Databasnamn).</span><span class="sxs-lookup"><span data-stu-id="c322c-209">In the **Database Name** box, enter *contosoads*.</span></span>
3. <span data-ttu-id="c322c-210">I **resursgruppen** klickar du på **använd befintliga** och markerar den resursgrupp som används för molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="c322c-210">In the **Resource group**, click **Use existing** and select the resource group used for the cloud service.</span></span>
4. <span data-ttu-id="c322c-211">I följande bild klickar du på **Server – konfigurera nödvändiga inställningar** och **Skapa en ny server**.</span><span class="sxs-lookup"><span data-stu-id="c322c-211">In the following image, click **Server - Configure required settings** and **Create a new server**.</span></span>

    ![Tunnel till databasservern](./media/cloud-services-dotnet-get-started/newdb.png)

    <span data-ttu-id="c322c-213">Om din prenumeration redan har en server kan du istället välja den servern i listrutan.</span><span class="sxs-lookup"><span data-stu-id="c322c-213">Alternatively, if your subscription already has a server, you can select that server from the drop-down list.</span></span>
5. <span data-ttu-id="c322c-214">I rutan **Servernamn** anger du *csvccontosodbserver*.</span><span class="sxs-lookup"><span data-stu-id="c322c-214">In the **Server name** box, enter *csvccontosodbserver*.</span></span>

6. <span data-ttu-id="c322c-215">Ange ett **inloggningsnamn** och **lösenord** med administratörsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="c322c-215">Enter an administrator **Login Name** and **Password**.</span></span>

    <span data-ttu-id="c322c-216">Om du har valt **Skapa en ny server** anger du inte ett befintligt namn och lösenord här.</span><span class="sxs-lookup"><span data-stu-id="c322c-216">If you selected **Create a new server**, you aren't entering an existing name and password here.</span></span> <span data-ttu-id="c322c-217">Du har angett ett nytt namn och lösenord som du definierar nu för när du vill komma åt databasen i framtiden.</span><span class="sxs-lookup"><span data-stu-id="c322c-217">You're entering a new name and password that you're defining now to use later when you access the database.</span></span> <span data-ttu-id="c322c-218">Om du valde en server som du har skapat vid ett tidigare tillfälle, uppmanas du att ange lösenordet för det administrativa användarkontot som du redan har skapat.</span><span class="sxs-lookup"><span data-stu-id="c322c-218">If you selected a server that you created previously, you'll be prompted for the password to the administrative user account you already created.</span></span>
7. <span data-ttu-id="c322c-219">Välj samma **Plats** som du valde för molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="c322c-219">Choose the same **Location** that you chose for the cloud service.</span></span>

    <span data-ttu-id="c322c-220">När molntjänsten och databasen är i olika datacenter (olika regioner), ökar svarstiden och du debiteras för bandbredden utanför datacentret.</span><span class="sxs-lookup"><span data-stu-id="c322c-220">When the cloud service and database are in different datacenters (different regions), latency will increase and you will be charged for bandwidth outside the data center.</span></span> <span data-ttu-id="c322c-221">Bandbredd inom ett datacenter är kostnadsfri.</span><span class="sxs-lookup"><span data-stu-id="c322c-221">Bandwidth within a data center is free.</span></span>
8. <span data-ttu-id="c322c-222">Markera **Ge Azure-tjänster åtkomst till servern**.</span><span class="sxs-lookup"><span data-stu-id="c322c-222">Check **Allow azure services to access server**.</span></span>
9. <span data-ttu-id="c322c-223">Klicka på **Välj** för den nya servern.</span><span class="sxs-lookup"><span data-stu-id="c322c-223">Click **Select** for the new server.</span></span>

    ![Ny SQL-databasserver](./media/cloud-services-dotnet-get-started/newdbserver.png)
10. <span data-ttu-id="c322c-225">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="c322c-225">Click **Create**.</span></span>

### <a name="create-an-azure-storage-account"></a><span data-ttu-id="c322c-226">Skapa ett Azure-lagringskonto</span><span class="sxs-lookup"><span data-stu-id="c322c-226">Create an Azure storage account</span></span>
<span data-ttu-id="c322c-227">Ett Azure-lagringskonto tillhandahåller resurser för att lagra kö- och blobbdata i molnet.</span><span class="sxs-lookup"><span data-stu-id="c322c-227">An Azure storage account provides resources for storing queue and blob data in the cloud.</span></span>

<span data-ttu-id="c322c-228">I ett riktigt program skapar du vanligtvis separata konton för programdata jämfört med loggningsdata, samt separata konton för testdata jämfört med produktionsdata.</span><span class="sxs-lookup"><span data-stu-id="c322c-228">In a real-world application, you would typically create separate accounts for application data versus logging data, and separate accounts for test data versus production data.</span></span> <span data-ttu-id="c322c-229">Under den här kursen använder du bara ett konto.</span><span class="sxs-lookup"><span data-stu-id="c322c-229">For this tutorial, you'll use just one account.</span></span>

1. <span data-ttu-id="c322c-230">I [Azure-portalen](https://portal.azure.com) klickar du på **New > Storage > Storage Account - blob, file, table, queue**.</span><span class="sxs-lookup"><span data-stu-id="c322c-230">In the [Azure portal](https://portal.azure.com), click **New > Storage > Storage account - blob, file, table, queue**.</span></span>
2. <span data-ttu-id="c322c-231">Ange ett URL-prefix i **Namn**-rutan.</span><span class="sxs-lookup"><span data-stu-id="c322c-231">In the **Name** box, enter a URL prefix.</span></span>

    <span data-ttu-id="c322c-232">Det här prefixet och texten som du ser under rutan utgör den unika URL:en till ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="c322c-232">This prefix plus the text you see under the box will be the unique URL to your storage account.</span></span> <span data-ttu-id="c322c-233">Om det prefix du anger redan har använts av någon annan, måste du välja ett annat prefix.</span><span class="sxs-lookup"><span data-stu-id="c322c-233">If the prefix you enter has already been used by someone else, you'll have to choose a different prefix.</span></span>
3. <span data-ttu-id="c322c-234">Ange **distributionsmodellen** som *Klassisk*.</span><span class="sxs-lookup"><span data-stu-id="c322c-234">Set the **Deployment model** to *Classic*.</span></span>

4. <span data-ttu-id="c322c-235">Välj **Locally redundant storage** (Lokalt redundant) i listrutan **Replication** (Replikering).</span><span class="sxs-lookup"><span data-stu-id="c322c-235">Set the **Replication** drop-down list to **Locally redundant storage**.</span></span>

    <span data-ttu-id="c322c-236">När geo-replikering har aktiverats för ett lagringskonto, replikeras det lagrade innehållet till ett sekundärt datacenter för att aktivera redundans om det skulle inträffa en större katastrof på den primära platsen.</span><span class="sxs-lookup"><span data-stu-id="c322c-236">When geo-replication is enabled for a storage account, the stored content is replicated to a secondary datacenter to enable failover if a major disaster occurs in the primary location.</span></span> <span data-ttu-id="c322c-237">Geo-replikering kan medföra ytterligare kostnader.</span><span class="sxs-lookup"><span data-stu-id="c322c-237">Geo-replication can incur additional costs.</span></span> <span data-ttu-id="c322c-238">När det gäller test- och utvecklingskonton ska du vanligtvis inte betala för geo-replikering.</span><span class="sxs-lookup"><span data-stu-id="c322c-238">For test and development accounts, you generally don't want to pay for geo-replication.</span></span> <span data-ttu-id="c322c-239">Mer information finns i [Skapa, hantera eller ta bort ett lagringskonto](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="c322c-239">For more information, see [Create, manage, or delete a storage account](../storage/common/storage-create-storage-account.md).</span></span>

5. <span data-ttu-id="c322c-240">I **resursgruppen** klickar du på **använd befintliga** och markerar den resursgrupp som används för molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="c322c-240">In the **Resource group**, click **Use existing** and select the resource group used for the cloud service.</span></span>
6. <span data-ttu-id="c322c-241">Ställ in rullgardinsmenyn **Plats** i samma region som du skulle välja för en molntjänst.</span><span class="sxs-lookup"><span data-stu-id="c322c-241">Set the **Location** drop-down list to the same region you chose for the cloud service.</span></span>

    <span data-ttu-id="c322c-242">När molntjänsten och lagringskontot är i olika datacenter (olika regioner), ökar svarstiden och du debiteras för bandbredden utanför datacentret.</span><span class="sxs-lookup"><span data-stu-id="c322c-242">When the cloud service and storage account are in different datacenters (different regions), latency will increase and you will be charged for bandwidth outside the data center.</span></span> <span data-ttu-id="c322c-243">Bandbredd inom ett datacenter är kostnadsfri.</span><span class="sxs-lookup"><span data-stu-id="c322c-243">Bandwidth within a data center is free.</span></span>

    <span data-ttu-id="c322c-244">Azure-tillhörighetsgrupper tillhandahåller en funktion som minskar avståndet mellan resurser i datacentret, vilket kan förkorta svarstiden.</span><span class="sxs-lookup"><span data-stu-id="c322c-244">Azure affinity groups provide a mechanism to minimize the distance between resources in a data center, which can reduce latency.</span></span> <span data-ttu-id="c322c-245">Under den här kursen används inte tillhörighetsgrupper.</span><span class="sxs-lookup"><span data-stu-id="c322c-245">This tutorial does not use affinity groups.</span></span> <span data-ttu-id="c322c-246">Mer information finns i [Skapa en tillhörighetsgrupp i Azure](http://msdn.microsoft.com/library/jj156209.aspx).</span><span class="sxs-lookup"><span data-stu-id="c322c-246">For more information, see [How to Create an Affinity Group in Azure](http://msdn.microsoft.com/library/jj156209.aspx).</span></span>
7. <span data-ttu-id="c322c-247">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="c322c-247">Click **Create**.</span></span>

    ![Nytt lagringskonto](./media/cloud-services-dotnet-get-started/newstorage.png)

    <span data-ttu-id="c322c-249">I avbildningen skapas ett lagringskonto med URL:en `csvccontosoads.core.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="c322c-249">In the image, a storage account is created with the URL `csvccontosoads.core.windows.net`.</span></span>

### <a name="configure-the-solution-to-use-your-azure-sql-database-when-it-runs-in-azure"></a><span data-ttu-id="c322c-250">Konfigurera lösningen så att den använder Azure SQL Database när den körs i Azure</span><span class="sxs-lookup"><span data-stu-id="c322c-250">Configure the solution to use your Azure SQL database when it runs in Azure</span></span>
<span data-ttu-id="c322c-251">Webbprojektet och arbetsrollsprojektet har varsin databasanslutningssträng, och båda strängarna måste peka på Azure SQL Database när appen körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="c322c-251">The web project and the worker role project each has its own database connection string, and each needs to point to the Azure SQL database when the app runs in Azure.</span></span>

<span data-ttu-id="c322c-252">Du kommer att använda en [Web.config-transformering](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/web-config-transformations) för webbrollen och en inställning för molntjänstmiljö för arbetsrollen.</span><span class="sxs-lookup"><span data-stu-id="c322c-252">You'll use a [Web.config transform](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/web-config-transformations) for the web role and a cloud service environment setting for the worker role.</span></span>

> [!NOTE]
> <span data-ttu-id="c322c-253">I det här och i nästa avsnitt får du lagra autentiseringsuppgifter i projektfiler.</span><span class="sxs-lookup"><span data-stu-id="c322c-253">In this section and the next section, you store credentials in project files.</span></span> <span data-ttu-id="c322c-254">[Lagra inte känsliga data i offentliga källkodslager](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control#secrets).</span><span class="sxs-lookup"><span data-stu-id="c322c-254">[Don't store sensitive data in public source code repositories](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control#secrets).</span></span>
>
>

1. <span data-ttu-id="c322c-255">Öppna transformeringsfilen *Web.Release.config* i ContosoAdsWeb-projektet för programmets *Web.config*-fil, ta bort kommentarblocket som innehåller ett `<connectionStrings>`-element, och ersätt det med följande kod.</span><span class="sxs-lookup"><span data-stu-id="c322c-255">In the ContosoAdsWeb project, open the *Web.Release.config* transform file for the application *Web.config* file, delete the comment block that contains a `<connectionStrings>` element, and paste the following code in its place.</span></span>

    ```xml
    <connectionStrings>
        <add name="ContosoAdsContext" connectionString="{connectionstring}"
        providerName="System.Data.SqlClient" xdt:Transform="SetAttributes" xdt:Locator="Match(name)"/>
    </connectionStrings>
    ```

    <span data-ttu-id="c322c-256">Lämna filen öppen för redigering.</span><span class="sxs-lookup"><span data-stu-id="c322c-256">Leave the file open for editing.</span></span>
2. <span data-ttu-id="c322c-257">I den [klassiska Azure-portalen](https://portal.azure.com) klickar du på **SQL Databases** i den vänstra rutan, klickar på databasen som du skapade för den här kursen och sedan på **Show connection strings** (Visa anslutningssträngar).</span><span class="sxs-lookup"><span data-stu-id="c322c-257">In the [Azure portal](https://portal.azure.com), click **SQL Databases** in the left pane, click the database you created for this tutorial, and then click **Show connection strings**.</span></span>

    ![Visa anslutningssträngar](./media/cloud-services-dotnet-get-started/showcs.png)

    <span data-ttu-id="c322c-259">Portalen visar anslutningssträngar med en platshållare för lösenordet.</span><span class="sxs-lookup"><span data-stu-id="c322c-259">The portal displays connection strings, with a placeholder for the password.</span></span>

    ![Anslutningssträngar](./media/cloud-services-dotnet-get-started/connstrings.png)
3. <span data-ttu-id="c322c-261">I transformeringsfilen *Web.Release.config* tar du bort `{connectionstring}` och ersätter den med ADO.NET-anslutningssträngen från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c322c-261">In the *Web.Release.config* transform file, delete `{connectionstring}` and paste in its place the ADO.NET connection string from the Azure portal.</span></span>
4. <span data-ttu-id="c322c-262">I anslutningssträngen som du klistrade in i transformeringsfilen *Web.Release.config* ska du ersätta `{your_password_here}` med lösenordet du skapade för den nya SQL-databasen.</span><span class="sxs-lookup"><span data-stu-id="c322c-262">In the connection string that you pasted into the *Web.Release.config* transform file, replace `{your_password_here}` with the password you created for the new SQL database.</span></span>
5. <span data-ttu-id="c322c-263">Spara filen.</span><span class="sxs-lookup"><span data-stu-id="c322c-263">Save the file.</span></span>  
6. <span data-ttu-id="c322c-264">Markera och kopiera anslutningssträngen (utan omgivande citattecken) och använd den i följande steg för att konfigurera arbetsrollsprojektet.</span><span class="sxs-lookup"><span data-stu-id="c322c-264">Select and copy the connection string (without the surrounding quotation marks) for use in the following steps for configuring the worker role project.</span></span>
7. <span data-ttu-id="c322c-265">I **Solution Explorer** går du till **Roles** (Roller) i molntjänstprojektet och högerklickar på **ContosoAdsWorker** och klickar sedan på **Properties** (Egenskaper).</span><span class="sxs-lookup"><span data-stu-id="c322c-265">In **Solution Explorer**, under **Roles** in the cloud service project, right-click **ContosoAdsWorker** and then click **Properties**.</span></span>

    ![Rollegenskaper](./media/cloud-services-dotnet-get-started/rolepropertiesworker.png)
8. <span data-ttu-id="c322c-267">Klicka på fliken **Settings** (Inställningar).</span><span class="sxs-lookup"><span data-stu-id="c322c-267">Click the **Settings** tab.</span></span>
9. <span data-ttu-id="c322c-268">Ändra **Service Configuration** (Tjänstkonfiguration) till **Cloud** (Moln).</span><span class="sxs-lookup"><span data-stu-id="c322c-268">Change **Service Configuration** to **Cloud**.</span></span>
10. <span data-ttu-id="c322c-269">Markera fältet **Value** (Värde) för inställningen `ContosoAdsDbConnectionString`, och klistra sedan in anslutningssträngen som du kopierade i kursens förra avsnitt.</span><span class="sxs-lookup"><span data-stu-id="c322c-269">Select the **Value** field for the `ContosoAdsDbConnectionString` setting, and then paste the connection string that you copied from the previous section of the tutorial.</span></span>

     ![Databasanslutningssträng för arbetsrollen](./media/cloud-services-dotnet-get-started/workerdbcs.png)
11. <span data-ttu-id="c322c-271">Spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="c322c-271">Save your changes.</span></span>  

### <a name="configure-the-solution-to-use-your-azure-storage-account-when-it-runs-in-azure"></a><span data-ttu-id="c322c-272">Konfigurera lösningen så att den använder ditt Azure-lagringskonto när den körs i Azure</span><span class="sxs-lookup"><span data-stu-id="c322c-272">Configure the solution to use your Azure storage account when it runs in Azure</span></span>
<span data-ttu-id="c322c-273">Azure-lagringskontots anslutningssträngar för både webbrollsprojektet och arbetsrollsprojektet lagras i miljöinställningar i molntjänstprojektet.</span><span class="sxs-lookup"><span data-stu-id="c322c-273">Azure storage account connection strings for both the web role project and the worker role project are stored in environment settings in the cloud service project.</span></span> <span data-ttu-id="c322c-274">Det finns en separat uppsättning inställningar som ska användas när programmet körs lokalt och när det körs i molnet för varje projekt.</span><span class="sxs-lookup"><span data-stu-id="c322c-274">For each project, there is a separate set of settings to be used when the application runs locally and when it runs in the cloud.</span></span> <span data-ttu-id="c322c-275">Du kommer att uppdatera molnmiljöinställningarna för både webb- och arbetsrollsprojektet.</span><span class="sxs-lookup"><span data-stu-id="c322c-275">You'll update the cloud environment settings for both web and worker role projects.</span></span>

1. <span data-ttu-id="c322c-276">I **Solution Explorer** högerklickar du på **ContosoAdsWeb** under **Roles** (Roller) i **ContosoAdsCloudService**-projektet. Klicka sedan på **Properties** (Egenskaper).</span><span class="sxs-lookup"><span data-stu-id="c322c-276">In **Solution Explorer**, right-click **ContosoAdsWeb** under **Roles** in the **ContosoAdsCloudService** project, and then click **Properties**.</span></span>

    ![Rollegenskaper](./media/cloud-services-dotnet-get-started/roleproperties.png)
2. <span data-ttu-id="c322c-278">Klicka på fliken **Settings** (Inställningar). Välj **Cloud** (Moln) i listrutan **Service Configuration** (Tjänstkonfiguration).</span><span class="sxs-lookup"><span data-stu-id="c322c-278">Click the **Settings** tab. In the **Service Configuration** drop-down box, choose **Cloud**.</span></span>

    ![Molnkonfiguration](./media/cloud-services-dotnet-get-started/sccloud.png)
3. <span data-ttu-id="c322c-280">Markera posten **StorageConnectionString** och sedan ser du en ellipsknapp (**...**) till höger om raden.</span><span class="sxs-lookup"><span data-stu-id="c322c-280">Select the **StorageConnectionString** entry, and you'll see an ellipsis (**...**) button at the right end of the line.</span></span> <span data-ttu-id="c322c-281">Klicka på ellipsknappen för att öppna dialogrutan **Create Storage Connection String** (Skapa lagringsanslutningssträng).</span><span class="sxs-lookup"><span data-stu-id="c322c-281">Click the ellipsis button to open the **Create Storage Account Connection String** dialog box.</span></span>

    ![Öppna dialogrutan för att skapa anslutningssträng](./media/cloud-services-dotnet-get-started/opencscreate.png)
4. <span data-ttu-id="c322c-283">I dialogrutan **Create Storage Connection String** (Skapa lagringsanslutningssträng) markerar du **Your subscription** (Din prenumeration), väljer det lagringskonto som du skapade tidigare och klickar sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c322c-283">In the **Create Storage Connection String** dialog box, click **Your subscription**, choose the storage account that you created earlier, and then click **OK**.</span></span> <span data-ttu-id="c322c-284">Om du inte redan har loggat in uppmanas du ange dina Azure-autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="c322c-284">If you're not already logged in, you'll be prompted for your Azure account credentials.</span></span>

    ![Skapa lagringsanslutningssträng](./media/cloud-services-dotnet-get-started/createstoragecs.png)
5. <span data-ttu-id="c322c-286">Spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="c322c-286">Save your changes.</span></span>
6. <span data-ttu-id="c322c-287">Följ samma steg som du använde för anslutningssträngen `StorageConnectionString` för att ange anslutningssträngen `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="c322c-287">Follow the same procedure that you used for the `StorageConnectionString` connection string to set the `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` connection string.</span></span>

    <span data-ttu-id="c322c-288">Den här anslutningssträngen används för loggning.</span><span class="sxs-lookup"><span data-stu-id="c322c-288">This connection string is used for logging.</span></span>
7. <span data-ttu-id="c322c-289">Följ samma steg som du använde för **ContosoAdsWeb**-rollen för att ange båda anslutningssträngarna för **ContosoAdsWorker**-rollen.</span><span class="sxs-lookup"><span data-stu-id="c322c-289">Follow the same procedure that you used for the **ContosoAdsWeb** role to set both connection strings for the **ContosoAdsWorker** role.</span></span> <span data-ttu-id="c322c-290">Glöm inte att ställa in **Service Configuration** (Tjänstkonfiguration) till **Cloud** (Moln).</span><span class="sxs-lookup"><span data-stu-id="c322c-290">Don't forget to set **Service Configuration** to **Cloud**.</span></span>

<span data-ttu-id="c322c-291">Rollmiljöinställningarna som du har konfigurerat i Visual Studio-gränssnittet lagras i följande filer i ContosoAdsCloudService-projektet:</span><span class="sxs-lookup"><span data-stu-id="c322c-291">The role environment settings that you have configured using the Visual Studio UI are stored in the following files in the ContosoAdsCloudService project:</span></span>

* <span data-ttu-id="c322c-292">*ServiceDefinition.csdef* – anger inställningsnamnen.</span><span class="sxs-lookup"><span data-stu-id="c322c-292">*ServiceDefinition.csdef* - Defines the setting names.</span></span>
* <span data-ttu-id="c322c-293">*ServiceConfiguration.Cloud.cscfg* – tillhandahåller värden när appen körs i molnet.</span><span class="sxs-lookup"><span data-stu-id="c322c-293">*ServiceConfiguration.Cloud.cscfg* - Provides values for when the app runs in the cloud.</span></span>
* <span data-ttu-id="c322c-294">*ServiceConfiguration.Local.cscfg* – tillhandahåller värden när appen körs lokalt.</span><span class="sxs-lookup"><span data-stu-id="c322c-294">*ServiceConfiguration.Local.cscfg* - Provides values for when the app runs locally.</span></span>

<span data-ttu-id="c322c-295">ServiceDefinition.csdef innehåller till exempel följande definitioner:</span><span class="sxs-lookup"><span data-stu-id="c322c-295">For example, the ServiceDefinition.csdef includes the following definitions:</span></span>

```xml
<ConfigurationSettings>
    <Setting name="StorageConnectionString" />
    <Setting name="ContosoAdsDbConnectionString" />
</ConfigurationSettings>
```

<span data-ttu-id="c322c-296">Filen *ServiceConfiguration.Cloud.cscfg* innehåller värdena du angav för de inställningarna i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c322c-296">And the *ServiceConfiguration.Cloud.cscfg* file includes the values you entered for those settings in Visual Studio.</span></span>

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

<span data-ttu-id="c322c-297">Inställningen `<Instances>` anger det antal virtuella datorer som Azure kommer att köra arbetsrollskoden på.</span><span class="sxs-lookup"><span data-stu-id="c322c-297">The `<Instances>` setting specifies the number of virtual machines that Azure will run the worker role code on.</span></span> <span data-ttu-id="c322c-298">I avsnittet [Nästa steg](#next-steps) hittar du länkar till mer information om att skala ut en molntjänst.</span><span class="sxs-lookup"><span data-stu-id="c322c-298">The [Next steps](#next-steps) section includes links to more information about scaling out a cloud service,</span></span>

### <a name="deploy-the-project-to-azure"></a><span data-ttu-id="c322c-299">Distribuera projektet till Azure</span><span class="sxs-lookup"><span data-stu-id="c322c-299">Deploy the project to Azure</span></span>
1. <span data-ttu-id="c322c-300">I **Solution Explorer** högerklickar du på **ContosoAdsCloudService**-molnprojektet och väljer sedan **Publish** (Publicera).</span><span class="sxs-lookup"><span data-stu-id="c322c-300">In **Solution Explorer**, right-click the **ContosoAdsCloudService** cloud project and then select **Publish**.</span></span>

   ![Menyn Publish (Publicera)](./media/cloud-services-dotnet-get-started/pubmenu.png)
2. <span data-ttu-id="c322c-302">Klicka på **Next** (Nästa) i **inloggningssteget** i **publiceringsguiden för Azure-program**.</span><span class="sxs-lookup"><span data-stu-id="c322c-302">In the **Sign in** step of the **Publish Azure Application** wizard, click **Next**.</span></span>

    ![Inloggningssteg](./media/cloud-services-dotnet-get-started/pubsignin.png)
3. <span data-ttu-id="c322c-304">Klicka på **Next** (Nästa) i guidens **inställningssteg**.</span><span class="sxs-lookup"><span data-stu-id="c322c-304">In the **Settings** step of the wizard, click **Next**.</span></span>

    ![Inställningssteg](./media/cloud-services-dotnet-get-started/pubsettings.png)

    <span data-ttu-id="c322c-306">Standardinställningarna på fliken **Advanced** (Avancerat) fungerar bra för den här kursen.</span><span class="sxs-lookup"><span data-stu-id="c322c-306">The default settings in the **Advanced** tab are fine for this tutorial.</span></span> <span data-ttu-id="c322c-307">Mer information om fliken Advanced (Avancerat) finns i [Publiceringsguide för Azure-program](http://msdn.microsoft.com/library/hh535756.aspx).</span><span class="sxs-lookup"><span data-stu-id="c322c-307">For information about the advanced tab, see [Publish Azure Application Wizard](http://msdn.microsoft.com/library/hh535756.aspx).</span></span>
4. <span data-ttu-id="c322c-308">Klicka på **Publish** (Publicera) i **sammanfattningssteget**.</span><span class="sxs-lookup"><span data-stu-id="c322c-308">In the **Summary** step, click **Publish**.</span></span>

    ![Sammanfattningssteg](./media/cloud-services-dotnet-get-started/pubsummary.png)

   <span data-ttu-id="c322c-310">Fönstret med **Azure-aktivitetsloggen** öppnas i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c322c-310">The **Azure Activity Log** window opens in Visual Studio.</span></span>
5. <span data-ttu-id="c322c-311">Klicka på högerpilen för att visa distributionsinformationen.</span><span class="sxs-lookup"><span data-stu-id="c322c-311">Click the right arrow icon to expand the deployment details.</span></span>

    <span data-ttu-id="c322c-312">Distributionen kan ta upp till 5 minuter eller mer att slutföra.</span><span class="sxs-lookup"><span data-stu-id="c322c-312">The deployment can take up to 5 minutes or more to complete.</span></span>

    ![Fönstret med Azure-aktivitetsloggen](./media/cloud-services-dotnet-get-started/waal.png)
6. <span data-ttu-id="c322c-314">När distributionen har slutförts ska du klicka på **webbappens URL** för att starta programmet.</span><span class="sxs-lookup"><span data-stu-id="c322c-314">When the deployment status is complete, click the **Web app URL** to start the application.</span></span>
7. <span data-ttu-id="c322c-315">Nu kan du testa appen genom att skapa, visa och redigera vissa annonser, precis som du gjorde när du körde programmet lokalt.</span><span class="sxs-lookup"><span data-stu-id="c322c-315">You can now test the app by creating, viewing, and editing some ads, as you did when you ran the application locally.</span></span>

> [!NOTE]
> <span data-ttu-id="c322c-316">När du har gjort dina tester kan du ta bort eller stoppa molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="c322c-316">When you're finished testing, delete or stop the cloud service.</span></span> <span data-ttu-id="c322c-317">Även om du inte använder molntjänsten uppstår avgifter eftersom det finns reserverade virtuella datorresurser som är avsedda för den.</span><span class="sxs-lookup"><span data-stu-id="c322c-317">Even if you're not using the cloud service, it's accruing charges because virtual machine resources are reserved for it.</span></span> <span data-ttu-id="c322c-318">Om du låter den köra kan dessutom alla som hittar URL:en skapa och visa annonser.</span><span class="sxs-lookup"><span data-stu-id="c322c-318">And if you leave it running, anyone who finds your URL can create and view ads.</span></span> <span data-ttu-id="c322c-319">Gå till **översiktsfliken** för din molntjänst i [Azure-portalen](https://portal.azure.com), och klicka sedan på knappen **Delete** (Ta bort) längst upp på sidan.</span><span class="sxs-lookup"><span data-stu-id="c322c-319">In the [Azure portal](https://portal.azure.com), go to the **Overview** tab for your cloud service, and then click the **Delete** button at the top of the page.</span></span> <span data-ttu-id="c322c-320">Om du bara tillfälligt vill förhindra andra från att komma åt webbplatsen klickar du på **Stop** (Stoppa) i stället.</span><span class="sxs-lookup"><span data-stu-id="c322c-320">If you just want to temporarily prevent others from accessing the site, click **Stop** instead.</span></span> <span data-ttu-id="c322c-321">I så fall fortsätter avgifterna att tillkomma.</span><span class="sxs-lookup"><span data-stu-id="c322c-321">In that case, charges will continue to accrue.</span></span> <span data-ttu-id="c322c-322">Du kan följa en liknande procedur om du vill ta bort SQL-databasen och lagringskontot när du inte längre behöver dem.</span><span class="sxs-lookup"><span data-stu-id="c322c-322">You can follow a similar procedure to delete the SQL database and storage account when you no longer need them.</span></span>
>
>

## <a name="create-the-application-from-scratch"></a><span data-ttu-id="c322c-323">Skapa programmet från grunden</span><span class="sxs-lookup"><span data-stu-id="c322c-323">Create the application from scratch</span></span>
<span data-ttu-id="c322c-324">Om du inte redan har hämtat [det färdiga programmet](http://code.msdn.microsoft.com/Simple-Azure-Cloud-Service-e01df2e4) ska du göra det nu.</span><span class="sxs-lookup"><span data-stu-id="c322c-324">If you haven't already downloaded [the completed application](http://code.msdn.microsoft.com/Simple-Azure-Cloud-Service-e01df2e4), do that now.</span></span> <span data-ttu-id="c322c-325">Du kommer att kopiera filer från det hämtade projektet till det nya projektet.</span><span class="sxs-lookup"><span data-stu-id="c322c-325">You'll copy files from the downloaded project into the new project.</span></span>

<span data-ttu-id="c322c-326">Contoso Ads-programmet skapas i följande steg:</span><span class="sxs-lookup"><span data-stu-id="c322c-326">Creating the Contoso Ads application involves the following steps:</span></span>

* <span data-ttu-id="c322c-327">Skapa en Visual Studio-lösning med molntjänst.</span><span class="sxs-lookup"><span data-stu-id="c322c-327">Create a cloud service Visual Studio solution.</span></span>
* <span data-ttu-id="c322c-328">Uppdatera och lägg till NuGet-paket.</span><span class="sxs-lookup"><span data-stu-id="c322c-328">Update and add NuGet packages.</span></span>
* <span data-ttu-id="c322c-329">Ange projektreferenser.</span><span class="sxs-lookup"><span data-stu-id="c322c-329">Set project references.</span></span>
* <span data-ttu-id="c322c-330">Konfigurera anslutningssträngar.</span><span class="sxs-lookup"><span data-stu-id="c322c-330">Configure connection strings.</span></span>
* <span data-ttu-id="c322c-331">Lägg till kodfiler.</span><span class="sxs-lookup"><span data-stu-id="c322c-331">Add code files.</span></span>

<span data-ttu-id="c322c-332">När lösningen har skapats granskar du koden som är unik för molntjänstprojekt samt Azure-blobbar och -köer.</span><span class="sxs-lookup"><span data-stu-id="c322c-332">After the solution is created, you'll review the code that is unique to cloud service projects and Azure blobs and queues.</span></span>

### <a name="create-a-cloud-service-visual-studio-solution"></a><span data-ttu-id="c322c-333">Skapa en Visual Studio-lösning med molntjänst</span><span class="sxs-lookup"><span data-stu-id="c322c-333">Create a cloud service Visual Studio solution</span></span>
1. <span data-ttu-id="c322c-334">I Visual Studio väljer du **New Project** (Nytt projekt) på menyn **File** (Arkiv).</span><span class="sxs-lookup"><span data-stu-id="c322c-334">In Visual Studio, choose **New Project** from the **File** menu.</span></span>
2. <span data-ttu-id="c322c-335">Visa **Visual C#** i den vänstra rutan i dialogrutan **New Project** (Nytt projekt), och välj **molnmallar**. Välj sedan **Azure-molntjänstmallen**.</span><span class="sxs-lookup"><span data-stu-id="c322c-335">In the left pane of the **New Project** dialog box, expand **Visual C#** and choose **Cloud** templates, and then choose the **Azure Cloud Service** template.</span></span>
3. <span data-ttu-id="c322c-336">Ange namnet ContosoAdsCloudService för projektet och lösningen, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c322c-336">Name the project and solution ContosoAdsCloudService, and then click **OK**.</span></span>

    ![Nytt projekt](./media/cloud-services-dotnet-get-started/newproject.png)
4. <span data-ttu-id="c322c-338">Lägg till en webbroll och en arbetsroll i dialogrutan **New Azure Cloud Service** (Ny Azure-molntjänst).</span><span class="sxs-lookup"><span data-stu-id="c322c-338">In the **New Azure Cloud Service** dialog box, add a web role and a worker role.</span></span> <span data-ttu-id="c322c-339">Ange namnet ContosoAdsWeb för webbrollen och ContosoAdsWorker för arbetsrollen.</span><span class="sxs-lookup"><span data-stu-id="c322c-339">Name the web role ContosoAdsWeb, and name the worker role ContosoAdsWorker.</span></span> <span data-ttu-id="c322c-340">(Använd pennikonen i den högra rutan för att ändra standardnamnen på rollerna.)</span><span class="sxs-lookup"><span data-stu-id="c322c-340">(Use the pencil icon in the right-hand pane to change the default names of the roles.)</span></span>

    ![Nytt molntjänstprojekt](./media/cloud-services-dotnet-get-started/newcsproj.png)
5. <span data-ttu-id="c322c-342">När du ser dialogrutan **New ASP.NET Project** (Nytt ASP.NET-projekt) för webbrollen, väljer du MVC-mallen och klickar sedan på **Change Authentication** (Ändra autentisering).</span><span class="sxs-lookup"><span data-stu-id="c322c-342">When you see the **New ASP.NET Project** dialog box for the web role, choose the MVC template, and then click **Change Authentication**.</span></span>

    ![Ändra autentisering](./media/cloud-services-dotnet-get-started/chgauth.png)
6. <span data-ttu-id="c322c-344">I dialogrutan **Change Authentication** (Ändra autentisering) väljer du **No Authentication** (Ingen autentisering) och klickar sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c322c-344">In the **Change Authentication** dialog box, choose **No Authentication**, and then click **OK**.</span></span>

    ![Ingen autentisering](./media/cloud-services-dotnet-get-started/noauth.png)
7. <span data-ttu-id="c322c-346">Klicka på **OK** i dialogrutan **New ASP.NET Project** (Nytt ASP.NET-projekt).</span><span class="sxs-lookup"><span data-stu-id="c322c-346">In the **New ASP.NET Project** dialog, click **OK**.</span></span>
8. <span data-ttu-id="c322c-347">I **Solution Explorer** högerklickar du på lösningen (inte på ett av projekten) och väljer **Add – New Project** (Lägg till – Nytt projekt).</span><span class="sxs-lookup"><span data-stu-id="c322c-347">In **Solution Explorer**, right-click the solution (not one of the projects), and choose **Add - New Project**.</span></span>
9. <span data-ttu-id="c322c-348">I dialogrutan **Add New Project** (Lägg till nytt projekt) väljer du **Windows** under **Visual C#** i den vänstra rutan, och klickar sedan på **klassbiblioteksmallen**.</span><span class="sxs-lookup"><span data-stu-id="c322c-348">In the **Add New Project** dialog box, choose **Windows** under **Visual C#** in the left pane, and then click the **Class Library** template.</span></span>  
10. <span data-ttu-id="c322c-349">Ange namnet *ContosoAdsCommon* på projektet och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c322c-349">Name the project *ContosoAdsCommon*, and then click **OK**.</span></span>

    <span data-ttu-id="c322c-350">Du måste referera till Entity Framework-kontexten och datamodellen från både webb- och arbetsrollsprojektet.</span><span class="sxs-lookup"><span data-stu-id="c322c-350">You need to reference the Entity Framework context and the data model from both web and worker role projects.</span></span> <span data-ttu-id="c322c-351">Alternativt kan du ange de EF-relaterade klasserna i webbrollsprojektet och referera till det projektet från arbetsrollsprojektet.</span><span class="sxs-lookup"><span data-stu-id="c322c-351">As an alternative, you could define the EF-related classes in the web role project and reference that project from the worker role project.</span></span> <span data-ttu-id="c322c-352">Men i den alternativa metoden måste arbetsrollsprojektet ha en referens till webbsammansättningar som det inte behöver.</span><span class="sxs-lookup"><span data-stu-id="c322c-352">But in the alternative approach, your worker role project would have a reference to web assemblies that it doesn't need.</span></span>

### <a name="update-and-add-nuget-packages"></a><span data-ttu-id="c322c-353">Uppdatera och lägga till NuGet-paket</span><span class="sxs-lookup"><span data-stu-id="c322c-353">Update and add NuGet packages</span></span>
1. <span data-ttu-id="c322c-354">Öppna dialogrutan **Manage NuGet Packages** (Hantera NuGet-paket) för lösningen.</span><span class="sxs-lookup"><span data-stu-id="c322c-354">Open the **Manage NuGet Packages** dialog box for the solution.</span></span>
2. <span data-ttu-id="c322c-355">Välj **Updates** (Uppdateringar) högst upp i fönstret.</span><span class="sxs-lookup"><span data-stu-id="c322c-355">At the top of the window, select **Updates**.</span></span>
3. <span data-ttu-id="c322c-356">Leta efter paketet *WindowsAzure.Storage*, och om du hittar det i listan ska du markera det och välja de webb- och arbetsrollsprojekt där du vill uppdatera det. Klicka sedan på **Update** (Uppdatera).</span><span class="sxs-lookup"><span data-stu-id="c322c-356">Look for the *WindowsAzure.Storage* package, and if it's in the list, select it and select the web and worker projects to update it in, and then click **Update**.</span></span>

    <span data-ttu-id="c322c-357">Lagringsklientbiblioteket uppdateras oftare än Visual Studio-projektmallar, så det kan ofta hända att versionen i ett projekt som nyligen skapats måste uppdateras.</span><span class="sxs-lookup"><span data-stu-id="c322c-357">The storage client library is updated more frequently than Visual Studio project templates, so you'll often find that the version in a newly-created project needs to be updated.</span></span>
4. <span data-ttu-id="c322c-358">Välj **Browse** (Bläddra) högst upp i fönstret.</span><span class="sxs-lookup"><span data-stu-id="c322c-358">At the top of the window, select **Browse**.</span></span>
5. <span data-ttu-id="c322c-359">Leta upp NuGet-paketet *EntityFramework* och installera det i alla tre projekt.</span><span class="sxs-lookup"><span data-stu-id="c322c-359">Find the *EntityFramework* NuGet package, and install it in all three projects.</span></span>
6. <span data-ttu-id="c322c-360">Leta upp NuGet-paketet *Microsoft.WindowsAzure.ConfigurationManager* och installera det i arbetsrollsprojektet.</span><span class="sxs-lookup"><span data-stu-id="c322c-360">Find the *Microsoft.WindowsAzure.ConfigurationManager* NuGet package, and install it in the worker role project.</span></span>

### <a name="set-project-references"></a><span data-ttu-id="c322c-361">Ange projektreferenser</span><span class="sxs-lookup"><span data-stu-id="c322c-361">Set project references</span></span>
1. <span data-ttu-id="c322c-362">Ange en referens till ContosoAdsCommon-projektet i ContosoAdsWeb-projektet.</span><span class="sxs-lookup"><span data-stu-id="c322c-362">In the ContosoAdsWeb project, set a reference to the ContosoAdsCommon project.</span></span> <span data-ttu-id="c322c-363">Högerklicka på ContosoAdsWeb-projektet och klicka sedan på **References** - **Add References** (Referenser – Lägg till referenser.</span><span class="sxs-lookup"><span data-stu-id="c322c-363">Right-click the ContosoAdsWeb project, and then click **References** - **Add References**.</span></span> <span data-ttu-id="c322c-364">Välj **Solution – Projects** (Lösning – Projekt) i den vänstra rutan i dialogrutan **Reference Manager** (Referenshanterare). Välj sedan **ContosoAdsCommon** och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c322c-364">In the **Reference Manager** dialog box, select **Solution – Projects** in the left pane, select **ContosoAdsCommon**, and then click **OK**.</span></span>
2. <span data-ttu-id="c322c-365">Ange en referens till ContosoAdsCommon-projektet i ContosoAdsWorker-projektet.</span><span class="sxs-lookup"><span data-stu-id="c322c-365">In the ContosoAdsWorker project, set a reference to the ContosAdsCommon project.</span></span>

    <span data-ttu-id="c322c-366">ContosoAdsCommon innehåller Entity Framework-datamodellen och -kontextklassen som kommer att användas både av klientdelen och serverdelen.</span><span class="sxs-lookup"><span data-stu-id="c322c-366">ContosoAdsCommon will contain the Entity Framework data model and context class, which will be used by both the front-end and back-end.</span></span>
3. <span data-ttu-id="c322c-367">Ange en referens till `System.Drawing` i ContosoAdsWorker-projektet.</span><span class="sxs-lookup"><span data-stu-id="c322c-367">In the ContosoAdsWorker project, set a reference to `System.Drawing`.</span></span>

    <span data-ttu-id="c322c-368">Den här sammansättningen används av serverdelen för att konvertera bilder till miniatyrbilder.</span><span class="sxs-lookup"><span data-stu-id="c322c-368">This assembly is used by the back-end to convert images to thumbnails.</span></span>

### <a name="configure-connection-strings"></a><span data-ttu-id="c322c-369">Konfigurera anslutningssträngar</span><span class="sxs-lookup"><span data-stu-id="c322c-369">Configure connection strings</span></span>
<span data-ttu-id="c322c-370">I det här avsnittet konfigurerar du Azure Storage- och SQL-anslutningssträngar för lokal testning.</span><span class="sxs-lookup"><span data-stu-id="c322c-370">In this section, you configure Azure Storage and SQL connection strings for testing locally.</span></span> <span data-ttu-id="c322c-371">Distributionsanvisningarna tidigare i kursen visar hur du anger anslutningssträngarna när appen körs i molnet.</span><span class="sxs-lookup"><span data-stu-id="c322c-371">The deployment instructions earlier in the tutorial explain how to set up the connection strings for when the app runs in the cloud.</span></span>

1. <span data-ttu-id="c322c-372">Öppna programmets Web.config-file i ContosoAdsWeb-projektet och infoga följande `connectionStrings`-element efter `configSections`-elementet.</span><span class="sxs-lookup"><span data-stu-id="c322c-372">In the ContosoAdsWeb project, open the application Web.config file, and insert the following `connectionStrings` element after the `configSections` element.</span></span>

    ```xml
    <connectionStrings>
        <add name="ContosoAdsContext" connectionString="Data Source=(localdb)\v11.0; Initial Catalog=ContosoAds; Integrated Security=True; MultipleActiveResultSets=True;" providerName="System.Data.SqlClient" />
    </connectionStrings>
    ```

    <span data-ttu-id="c322c-373">Om du använder Visual Studio 2015 eller högre ersätter du ”v11.0” med ”MSSQLLocalDB”.</span><span class="sxs-lookup"><span data-stu-id="c322c-373">If you're using Visual Studio 2015 or higher, replace "v11.0" with "MSSQLLocalDB".</span></span>
2. <span data-ttu-id="c322c-374">Spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="c322c-374">Save your changes.</span></span>
3. <span data-ttu-id="c322c-375">Högerklicka på ContosoAdsWeb under **Roles** (Roller) i ContosoAdsCloudService-projektet, och klicka sedan på **Properties** (Egenskaper).</span><span class="sxs-lookup"><span data-stu-id="c322c-375">In the ContosoAdsCloudService project, right-click ContosoAdsWeb under **Roles**, and then click **Properties**.</span></span>

    ![Rollegenskaper](./media/cloud-services-dotnet-get-started/roleproperties.png)
4. <span data-ttu-id="c322c-377">Klicka på fliken **Settings** (Inställningar) i egenskapsfönstret för  **ContosAdsWeb [roll]**, och klicka sedan på **Add Setting** (Lägg till inställning).</span><span class="sxs-lookup"><span data-stu-id="c322c-377">In the **ContosAdsWeb [Role]** properties window, click the **Settings** tab, and then click **Add Setting**.</span></span>

    <span data-ttu-id="c322c-378">Lämna **Service Configuration** (Tjänstkonfiguration) inställd på **All Configurations** (Alla konfigurationer).</span><span class="sxs-lookup"><span data-stu-id="c322c-378">Leave **Service Configuration** set to **All Configurations**.</span></span>
5. <span data-ttu-id="c322c-379">Lägg till en inställning med namnet *StorageConnectionString*.</span><span class="sxs-lookup"><span data-stu-id="c322c-379">Add a setting named *StorageConnectionString*.</span></span> <span data-ttu-id="c322c-380">Ange **typen** som *ConnectionString*, och ställ in **värdet** till *UseDevelopmentStorage=true*.</span><span class="sxs-lookup"><span data-stu-id="c322c-380">Set **Type** to *ConnectionString*, and set **Value** to *UseDevelopmentStorage=true*.</span></span>

    ![Ny anslutningssträng](./media/cloud-services-dotnet-get-started/scall.png)
6. <span data-ttu-id="c322c-382">Spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="c322c-382">Save your changes.</span></span>
7. <span data-ttu-id="c322c-383">Följ samma procedur för att lägga till en lagringsanslutningssträng i ContosoAdsWorker-rollegenskaperna.</span><span class="sxs-lookup"><span data-stu-id="c322c-383">Follow the same procedure to add a storage connection string in the ContosoAdsWorker role properties.</span></span>
8. <span data-ttu-id="c322c-384">När du har egenskapsfönstret för **ContosoAdsWorker [roll]** öppet lägger du till ytterligare en anslutningssträng:</span><span class="sxs-lookup"><span data-stu-id="c322c-384">Still in the **ContosoAdsWorker [Role]** properties window, add another connection string:</span></span>

   * <span data-ttu-id="c322c-385">Namn: ContosoAdsDbConnectionString</span><span class="sxs-lookup"><span data-stu-id="c322c-385">Name: ContosoAdsDbConnectionString</span></span>
   * <span data-ttu-id="c322c-386">Typ: Sträng</span><span class="sxs-lookup"><span data-stu-id="c322c-386">Type: String</span></span>
   * <span data-ttu-id="c322c-387">Värde: Klistra in samma anslutningssträng som du använde för webbrollsprojektet.</span><span class="sxs-lookup"><span data-stu-id="c322c-387">Value: Paste the same connection string you used for the web role project.</span></span> <span data-ttu-id="c322c-388">(Följande exempel gäller Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="c322c-388">(The following example is for Visual Studio 2013.</span></span> <span data-ttu-id="c322c-389">Glöm inte att ändra datakällan om du kopierar det här exemplet och använder Visual Studio 2015 eller högre.)</span><span class="sxs-lookup"><span data-stu-id="c322c-389">Don't forget to change the Data Source if you copy this example and you're using Visual Studio 2015 or higher.)</span></span>

       ```
       Data Source=(localdb)\v11.0; Initial Catalog=ContosoAds; Integrated Security=True; MultipleActiveResultSets=True;
       ```

### <a name="add-code-files"></a><span data-ttu-id="c322c-390">Lägga till kodfiler</span><span class="sxs-lookup"><span data-stu-id="c322c-390">Add code files</span></span>
<span data-ttu-id="c322c-391">I det här avsnittet får du kopiera kodfiler från den hämtade lösningen till den nya lösningen.</span><span class="sxs-lookup"><span data-stu-id="c322c-391">In this section, you copy code files from the downloaded solution into the new solution.</span></span> <span data-ttu-id="c322c-392">I följande avsnitt visas och förklaras viktiga delar av den här koden.</span><span class="sxs-lookup"><span data-stu-id="c322c-392">The following sections will show and explain key parts of this code.</span></span>

<span data-ttu-id="c322c-393">Om du vill lägga till filer i ett projekt eller i en mapp, högerklickar du på projektet eller mappen och klickar på **Add** - **Existing Item** (Lägg till – Befintligt objekt).</span><span class="sxs-lookup"><span data-stu-id="c322c-393">To add files to a project or a folder, right-click the project or folder and click **Add** - **Existing Item**.</span></span> <span data-ttu-id="c322c-394">Välj de filer du vill ha och klicka sedan på **Add** (Lägg till).</span><span class="sxs-lookup"><span data-stu-id="c322c-394">Select the files you want and then click **Add**.</span></span> <span data-ttu-id="c322c-395">Om du blir tillfrågad om du vill ersätta befintliga filer klickar du på **Yes** (Ja).</span><span class="sxs-lookup"><span data-stu-id="c322c-395">If asked whether you want to replace existing files, click **Yes**.</span></span>

1. <span data-ttu-id="c322c-396">Ta bort filen *Class1.cs* i ContosoAdsCommon-projektet och lägg i stället till filerna *Ad.cs* och *ContosoAdscontext.cs* från det hämtade projektet.</span><span class="sxs-lookup"><span data-stu-id="c322c-396">In the ContosoAdsCommon project, delete the *Class1.cs* file and add in its place the *Ad.cs* and *ContosoAdscontext.cs* files from the downloaded project.</span></span>
2. <span data-ttu-id="c322c-397">Lägg till följande filer från det hämtade projektet i ContosoAdsWeb-projektet.</span><span class="sxs-lookup"><span data-stu-id="c322c-397">In the ContosoAdsWeb project, add the following files from the downloaded project.</span></span>

   * <span data-ttu-id="c322c-398">*Global.asax.cs*.</span><span class="sxs-lookup"><span data-stu-id="c322c-398">*Global.asax.cs*.</span></span>  
   * <span data-ttu-id="c322c-399">I mappen *Views\Shared*: *\_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="c322c-399">In the *Views\Shared* folder: *\_Layout.cshtml*.</span></span>
   * <span data-ttu-id="c322c-400">I mappen *Views\Home*: *Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="c322c-400">In the *Views\Home* folder: *Index.cshtml*.</span></span>
   * <span data-ttu-id="c322c-401">I mappen *Controllers*: *AdController.cs*.</span><span class="sxs-lookup"><span data-stu-id="c322c-401">In the *Controllers* folder: *AdController.cs*.</span></span>
   * <span data-ttu-id="c322c-402">I mappen *Views\Ad* (skapa mappen först): fem *.cshtml*-filer.</span><span class="sxs-lookup"><span data-stu-id="c322c-402">In the *Views\Ad* folder (create the folder first): five *.cshtml* files.</span></span>
3. <span data-ttu-id="c322c-403">Lägg till *WorkerRole.cs* från det hämtade projektet i ContosoAdsWorker-projektet.</span><span class="sxs-lookup"><span data-stu-id="c322c-403">In the ContosoAdsWorker project, add *WorkerRole.cs* from the downloaded project.</span></span>

<span data-ttu-id="c322c-404">Du kan nu skapa och köra programmet enligt de tidigare anvisningarna i kursen, och appen använder lokala databas- och lagringsemulatorresurser.</span><span class="sxs-lookup"><span data-stu-id="c322c-404">You can now build and run the application as instructed earlier in the tutorial, and the app will use local database and storage emulator resources.</span></span>

<span data-ttu-id="c322c-405">I de följande avsnitten beskrivs den kod som gäller när du arbetar med Azure-miljön, -blobbar och -köer.</span><span class="sxs-lookup"><span data-stu-id="c322c-405">The following sections explain the code related to working with the Azure environment, blobs, and queues.</span></span> <span data-ttu-id="c322c-406">Under den här kursen förklaras inte hur du skapar MVC-kontrollanter och vyer med scaffold-teknik, hur du skriver Entity Framework-kod som fungerar med SQL Server-databaser eller grundläggande information om asynkron programmering i ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="c322c-406">This tutorial does not explain how to create MVC controllers and views using scaffolding, how to write Entity Framework code that works with SQL Server databases, or the basics of asynchronous programming in ASP.NET 4.5.</span></span> <span data-ttu-id="c322c-407">Mer information om de här ämnena finns i följande resurser:</span><span class="sxs-lookup"><span data-stu-id="c322c-407">For information about these topics, see the following resources:</span></span>

* [<span data-ttu-id="c322c-408">Kom igång med MVC 5</span><span class="sxs-lookup"><span data-stu-id="c322c-408">Get started with MVC 5</span></span>](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started)
* [<span data-ttu-id="c322c-409">Kom igång med EF 6 och MVC 5</span><span class="sxs-lookup"><span data-stu-id="c322c-409">Get started with EF 6 and MVC 5</span></span>](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc)
* <span data-ttu-id="c322c-410">[Introduktion till asynkron programmering i .NET 4.5](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices#async).</span><span class="sxs-lookup"><span data-stu-id="c322c-410">[Introduction to asynchronous programming in .NET 4.5](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices#async).</span></span>

### <a name="contosoadscommon---adcs"></a><span data-ttu-id="c322c-411">ContosoAdsCommon – Ad.cs</span><span class="sxs-lookup"><span data-stu-id="c322c-411">ContosoAdsCommon - Ad.cs</span></span>
<span data-ttu-id="c322c-412">Filen Ad.cs definierar en uppräkning för annonskategorier och en POCO-entitetsklass för annonsinformation.</span><span class="sxs-lookup"><span data-stu-id="c322c-412">The Ad.cs file defines an enum for ad categories and a POCO entity class for ad information.</span></span>

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

### <a name="contosoadscommon---contosoadscontextcs"></a><span data-ttu-id="c322c-413">ContosoAdsCommon – ContosoAdsContext.cs</span><span class="sxs-lookup"><span data-stu-id="c322c-413">ContosoAdsCommon - ContosoAdsContext.cs</span></span>
<span data-ttu-id="c322c-414">ContosoAdsContext-klassen anger att annonsklassen används i en DbSet-samling som Entity Framework lagrar i en SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="c322c-414">The ContosoAdsContext class specifies that the Ad class is used in a DbSet collection, which Entity Framework will store in a SQL database.</span></span>

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

<span data-ttu-id="c322c-415">Klassen har två konstruktorer.</span><span class="sxs-lookup"><span data-stu-id="c322c-415">The class has two constructors.</span></span> <span data-ttu-id="c322c-416">Den första används av webbprojektet och anger namnet på en anslutningssträng som lagras i Web.config-filen.</span><span class="sxs-lookup"><span data-stu-id="c322c-416">The first of them is used by the web project, and specifies the name of a connection string that is stored in the Web.config file.</span></span> <span data-ttu-id="c322c-417">Den andra konstruktorn gör att du kan överföra den faktiska anslutningssträngen som används av arbetsrollsprojektet, eftersom det inte har en Web.config-fil.</span><span class="sxs-lookup"><span data-stu-id="c322c-417">The second constructor enables you to pass in the actual connection string used by the worker role project, since it doesn't have a Web.config file.</span></span> <span data-ttu-id="c322c-418">Du såg tidigare var den här anslutningssträngen lagras, och du kommer längre fram att få se hur koden hämtar anslutningssträngen när den instantierar DbContext-klassen.</span><span class="sxs-lookup"><span data-stu-id="c322c-418">You saw earlier where this connection string was stored, and you'll see later how the code retrieves the connection string when it instantiates the DbContext class.</span></span>

### <a name="contosoadsweb---globalasaxcs"></a><span data-ttu-id="c322c-419">ContosoAdsWeb – Global.asax.cs</span><span class="sxs-lookup"><span data-stu-id="c322c-419">ContosoAdsWeb - Global.asax.cs</span></span>
<span data-ttu-id="c322c-420">Kod som anropas från metoden `Application_Start` skapar en blobbehållare för *images* och en kö för *images* om dessa inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="c322c-420">Code that is called from the `Application_Start` method creates an *images* blob container and an *images* queue if they don't already exist.</span></span> <span data-ttu-id="c322c-421">Det innebär att när du börjar använda ett nytt lagringskonto eller börjar använda lagringsemulatorn på en ny dator, skapas den nödvändiga blobbehållaren och kön automatiskt.</span><span class="sxs-lookup"><span data-stu-id="c322c-421">This ensures that whenever you start using a new storage account, or start using the storage emulator on a new computer, the required blob container and queue will be created automatically.</span></span>

<span data-ttu-id="c322c-422">Koden får tillgång till lagringskontot genom att använda lagringsanslutningssträngen från *.cscfg*-filen.</span><span class="sxs-lookup"><span data-stu-id="c322c-422">The code gets access to the storage account by using the storage connection string from the *.cscfg* file.</span></span>

```csharp
var storageAccount = CloudStorageAccount.Parse
    (RoleEnvironment.GetConfigurationSettingValue("StorageConnectionString"));
```

<span data-ttu-id="c322c-423">Sedan hämtar den en referens till blobbehållaren för *images*, skapar behållaren om den inte redan finns, och anger åtkomstbehörighet för den nya behållaren.</span><span class="sxs-lookup"><span data-stu-id="c322c-423">Then it gets a reference to the *images* blob container, creates the container if it doesn't already exist, and sets access permissions on the new container.</span></span> <span data-ttu-id="c322c-424">Som standard tillåter nya behållare enbart klienter med lagringskontouppgifter att få tillgång till blobbar.</span><span class="sxs-lookup"><span data-stu-id="c322c-424">By default, new containers only allow clients with storage account credentials to access blobs.</span></span> <span data-ttu-id="c322c-425">Webbplatsen kräver att blobbarna är offentliga så att den kan visa bilder med URL:er som pekar på bildblobbarna.</span><span class="sxs-lookup"><span data-stu-id="c322c-425">The website needs the blobs to be public so that it can display images using URLs that point to the image blobs.</span></span>

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

<span data-ttu-id="c322c-426">Liknande kod hämtar en referens till *images*-kön och skapar en ny kö.</span><span class="sxs-lookup"><span data-stu-id="c322c-426">Similar code gets a reference to the *images* queue and creates a new queue.</span></span> <span data-ttu-id="c322c-427">I så fall krävs ingen ändring av behörigheter.</span><span class="sxs-lookup"><span data-stu-id="c322c-427">In this case, no permissions change is needed.</span></span>

```csharp
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
var imagesQueue = queueClient.GetQueueReference("images");
imagesQueue.CreateIfNotExists();
```

### <a name="contosoadsweb---layoutcshtml"></a><span data-ttu-id="c322c-428">ContosoAdsWeb – \_Layout.cshtml</span><span class="sxs-lookup"><span data-stu-id="c322c-428">ContosoAdsWeb - \_Layout.cshtml</span></span>
<span data-ttu-id="c322c-429">Filen *_Layout.cshtml* anger appnamnet i sidhuvudet och sidfoten, och skapar en menypost som heter ”Ads”.</span><span class="sxs-lookup"><span data-stu-id="c322c-429">The *_Layout.cshtml* file sets the app name in the header and footer, and creates an "Ads" menu entry.</span></span>

### <a name="contosoadsweb---viewshomeindexcshtml"></a><span data-ttu-id="c322c-430">ContosoAdsWeb – Views\Home\Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="c322c-430">ContosoAdsWeb - Views\Home\Index.cshtml</span></span>
<span data-ttu-id="c322c-431">Filen *Views\Home\Index.cshtml* visar kategorilänkar på startsidan.</span><span class="sxs-lookup"><span data-stu-id="c322c-431">The *Views\Home\Index.cshtml* file displays category links on the home page.</span></span> <span data-ttu-id="c322c-432">Länkarna överför heltalsvärdet för uppräkningen `Category` i en QueryString-variabel till Ads-indexsidan. </span><span class="sxs-lookup"><span data-stu-id="c322c-432">The links pass the integer value of the `Category` enum in a querystring variable to the Ads Index page.</span></span>

```razor
<li>@Html.ActionLink("Cars", "Index", "Ad", new { category = (int)Category.Cars }, null)</li>
<li>@Html.ActionLink("Real estate", "Index", "Ad", new { category = (int)Category.RealEstate }, null)</li>
<li>@Html.ActionLink("Free stuff", "Index", "Ad", new { category = (int)Category.FreeStuff }, null)</li>
<li>@Html.ActionLink("All", "Index", "Ad", null, null)</li>
```

### <a name="contosoadsweb---adcontrollercs"></a><span data-ttu-id="c322c-433">ContosoAdsWeb – AdController.cs</span><span class="sxs-lookup"><span data-stu-id="c322c-433">ContosoAdsWeb - AdController.cs</span></span>
<span data-ttu-id="c322c-434">I filen *AdController.cs* anropar konstruktorn metoden `InitializeStorage` för att skapa Azure Storage-klientbiblioteksobjekt som tillhandahåller en API som kan användas för blobbar och köer.</span><span class="sxs-lookup"><span data-stu-id="c322c-434">In the *AdController.cs* file, the constructor calls the `InitializeStorage` method to create Azure Storage Client Library objects that provide an API for working with blobs and queues.</span></span>

<span data-ttu-id="c322c-435">Sedan hämtar koden en referens till blobbehållaren för *images* som du såg tidigare i *Global.asax.cs*.</span><span class="sxs-lookup"><span data-stu-id="c322c-435">Then the code gets a reference to the *images* blob container as you saw earlier in *Global.asax.cs*.</span></span> <span data-ttu-id="c322c-436">När den gör det anger den en [standardpolicy för återförsök](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling) som är lämplig för en webbapp.</span><span class="sxs-lookup"><span data-stu-id="c322c-436">While doing that it sets a default [retry policy](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling) appropriate for a web app.</span></span> <span data-ttu-id="c322c-437">Standardpolicyn för återförsök med exponentiell begränsning kan hänga webbappen längre än en minut vid upprepade återförsök för ett tillfälligt fel.</span><span class="sxs-lookup"><span data-stu-id="c322c-437">The default exponential backoff retry policy could hang the web app for longer than a minute on repeated retries for a transient fault.</span></span> <span data-ttu-id="c322c-438">Återförsökspolicyn som anges här väntar i tre sekunder efter varje försök i upp till tre försök.</span><span class="sxs-lookup"><span data-stu-id="c322c-438">The retry policy specified here waits three seconds after each try for up to three tries.</span></span>

```csharp
var blobClient = storageAccount.CreateCloudBlobClient();
blobClient.DefaultRequestOptions.RetryPolicy = new LinearRetry(TimeSpan.FromSeconds(3), 3);
imagesBlobContainer = blobClient.GetContainerReference("images");
```

<span data-ttu-id="c322c-439">Liknande kod hämtar en referens till *images*-kön.</span><span class="sxs-lookup"><span data-stu-id="c322c-439">Similar code gets a reference to the *images* queue.</span></span>

```csharp
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
queueClient.DefaultRequestOptions.RetryPolicy = new LinearRetry(TimeSpan.FromSeconds(3), 3);
imagesQueue = queueClient.GetQueueReference("images");
```

<span data-ttu-id="c322c-440">Större delen av kontrollantkoden är typisk när du arbetar med en Entity Framework-datamodell med en DbContext-klass.</span><span class="sxs-lookup"><span data-stu-id="c322c-440">Most of the controller code is typical for working with an Entity Framework data model using a DbContext class.</span></span> <span data-ttu-id="c322c-441">Ett undantag är HttpPost-metoden `Create`, som laddar upp en fil och sparar den i Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="c322c-441">An exception is the HttpPost `Create` method, which uploads a file and saves it in blob storage.</span></span> <span data-ttu-id="c322c-442">Modellbindaren tillhandahåller ett [HttpPostedFileBase](http://msdn.microsoft.com/library/system.web.httppostedfilebase.aspx)-objekt till metoden.</span><span class="sxs-lookup"><span data-stu-id="c322c-442">The model binder provides an [HttpPostedFileBase](http://msdn.microsoft.com/library/system.web.httppostedfilebase.aspx) object to the method.</span></span>

```csharp
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<ActionResult> Create(
    [Bind(Include = "Title,Price,Description,Category,Phone")] Ad ad,
    HttpPostedFileBase imageFile)
```

<span data-ttu-id="c322c-443">Om användaren har valt en fil att ladda upp, laddar koden upp filen, sparar den i en blob och uppdaterar Ad-databasposten med en URL som pekar på blobben.</span><span class="sxs-lookup"><span data-stu-id="c322c-443">If the user selected a file to upload, the code uploads the file, saves it in a blob, and updates the Ad database record with a URL that points to the blob.</span></span>

```csharp
if (imageFile != null && imageFile.ContentLength != 0)
{
    blob = await UploadAndSaveBlobAsync(imageFile);
    ad.ImageURL = blob.Uri.ToString();
}
```

<span data-ttu-id="c322c-444">Koden som utför uppladdningen är i metoden `UploadAndSaveBlobAsync`.</span><span class="sxs-lookup"><span data-stu-id="c322c-444">The code that does the upload is in the `UploadAndSaveBlobAsync` method.</span></span> <span data-ttu-id="c322c-445">Den skapar ett GUID-namn för blobben, laddar upp och sparar filen, och returnerar en referens till den sparade blobben.</span><span class="sxs-lookup"><span data-stu-id="c322c-445">It creates a GUID name for the blob, uploads and saves the file, and returns a reference to the saved blob.</span></span>

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

<span data-ttu-id="c322c-446">När HttpPost-metoden `Create` laddar upp en blob och uppdaterar databasen, skapar den ett kömeddelande för att informera den serverdelsprocessen att en bild är redo för konvertering till en miniatyrbild.</span><span class="sxs-lookup"><span data-stu-id="c322c-446">After the HttpPost `Create` method uploads a blob and updates the database, it creates a queue message to inform that back-end process that an image is ready for conversion to a thumbnail.</span></span>

```csharp
string queueMessageString = ad.AdId.ToString();
var queueMessage = new CloudQueueMessage(queueMessageString);
await queue.AddMessageAsync(queueMessage);
```

<span data-ttu-id="c322c-447">Koden för HttpPost-metoden `Edit` är liknande, förutom att om användaren väljer en ny bildfil måste alla blobbar som redan finns tas bort.</span><span class="sxs-lookup"><span data-stu-id="c322c-447">The code for the HttpPost `Edit` method is similar except that if the user selects a new image file any blobs that already exist must be deleted.</span></span>

```csharp
if (imageFile != null && imageFile.ContentLength != 0)
{
    await DeleteAdBlobsAsync(ad);
    imageBlob = await UploadAndSaveBlobAsync(imageFile);
    ad.ImageURL = imageBlob.Uri.ToString();
}
```

<span data-ttu-id="c322c-448">I nästa exempel visas den kod som tar bort blobbar när du tar bort en annons.</span><span class="sxs-lookup"><span data-stu-id="c322c-448">The next example shows the code that deletes blobs when you delete an ad.</span></span>

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

### <a name="contosoadsweb---viewsadindexcshtml-and-detailscshtml"></a><span data-ttu-id="c322c-449">ContosoAdsWeb – Views\Ad\Index.cshtml och Details.cshtml</span><span class="sxs-lookup"><span data-stu-id="c322c-449">ContosoAdsWeb - Views\Ad\Index.cshtml and Details.cshtml</span></span>
<span data-ttu-id="c322c-450">Filen *Index.cshtml* visar miniatyrbilder med andra annonsdata.</span><span class="sxs-lookup"><span data-stu-id="c322c-450">The *Index.cshtml* file displays thumbnails with the other ad data.</span></span>

```razor
<img src="@Html.Raw(item.ThumbnailURL)" />
```

<span data-ttu-id="c322c-451">Filen *Details.cshtml* visar bilden i full storlek.</span><span class="sxs-lookup"><span data-stu-id="c322c-451">The *Details.cshtml* file displays the full-size image.</span></span>

```razor
<img src="@Html.Raw(Model.ImageURL)" />
```

### <a name="contosoadsweb---viewsadcreatecshtml-and-editcshtml"></a><span data-ttu-id="c322c-452">ContosoAdsWeb – Views\Ad\Create.cshtml och Edit.cshtml</span><span class="sxs-lookup"><span data-stu-id="c322c-452">ContosoAdsWeb - Views\Ad\Create.cshtml and Edit.cshtml</span></span>
<span data-ttu-id="c322c-453">Filerna *Create.cshtml* och *Edit.cshtml* anger formkodning som gör att kontrollanten kan hämta objektet `HttpPostedFileBase`.</span><span class="sxs-lookup"><span data-stu-id="c322c-453">The *Create.cshtml* and *Edit.cshtml* files specify form encoding that enables the controller to get the `HttpPostedFileBase` object.</span></span>

```razor
@using (Html.BeginForm("Create", "Ad", FormMethod.Post, new { enctype = "multipart/form-data" }))
```

<span data-ttu-id="c322c-454">Ett `<input>`-element instruerar webbläsaren att tillhandahålla en dialogruta för filval.</span><span class="sxs-lookup"><span data-stu-id="c322c-454">An `<input>` element tells the browser to provide a file selection dialog.</span></span>

```razor
<input type="file" name="imageFile" accept="image/*" class="form-control fileupload" />
```

### <a name="contosoadsworker---workerrolecs---onstart-method"></a><span data-ttu-id="c322c-455">ContosoAdsWorker – WorkerRole.cs – OnStart-metoden</span><span class="sxs-lookup"><span data-stu-id="c322c-455">ContosoAdsWorker - WorkerRole.cs - OnStart method</span></span>
<span data-ttu-id="c322c-456">Azure-arbetsrollsmiljön anropar metoden `OnStart` i klassen `WorkerRole` när arbetsrollen påbörjas, och den anropar metoden `Run` när metoden `OnStart` avslutas.</span><span class="sxs-lookup"><span data-stu-id="c322c-456">The Azure worker role environment calls the `OnStart` method in the `WorkerRole` class when the worker role is getting started, and it calls the `Run` method when the `OnStart` method finishes.</span></span>

<span data-ttu-id="c322c-457">Metoden `OnStart` hämtar databasanslutningssträngen från *.cscfg*-filen och skickar den till Entity Framework DbContext-klassen.</span><span class="sxs-lookup"><span data-stu-id="c322c-457">The `OnStart` method gets the database connection string from the *.cscfg* file and passes it to the Entity Framework DbContext class.</span></span> <span data-ttu-id="c322c-458">SQLClient-leverantören används som standard så leverantören behöver inte anges.</span><span class="sxs-lookup"><span data-stu-id="c322c-458">The SQLClient provider is used by default, so the provider does not have to be specified.</span></span>

```csharp
var dbConnString = CloudConfigurationManager.GetSetting("ContosoAdsDbConnectionString");
db = new ContosoAdsContext(dbConnString);
```

<span data-ttu-id="c322c-459">Därefter hämtar metoden en referens till lagringskontot och skapar blobbehållaren och kön om de inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="c322c-459">After that, the method gets a reference to the storage account and creates the blob container and queue if they don't exist.</span></span> <span data-ttu-id="c322c-460">Koden för den åtgärden liknar det du redan har sett i metoden `Application_Start` för webbrollen.</span><span class="sxs-lookup"><span data-stu-id="c322c-460">The code for that is similar to what you already saw in the web role `Application_Start` method.</span></span>

### <a name="contosoadsworker---workerrolecs---run-method"></a><span data-ttu-id="c322c-461">ContosoAdsWorker – WorkerRole.cs – Run-metoden</span><span class="sxs-lookup"><span data-stu-id="c322c-461">ContosoAdsWorker - WorkerRole.cs - Run method</span></span>
<span data-ttu-id="c322c-462">Metoden `Run` anropas när metoden `OnStart` har avslutat initieringsarbetet.</span><span class="sxs-lookup"><span data-stu-id="c322c-462">The `Run` method is called when the `OnStart` method finishes its initialization work.</span></span> <span data-ttu-id="c322c-463">Metoden kör en oändlig loop som söker efter nya kömeddelanden och bearbetar dem när de kommer in.</span><span class="sxs-lookup"><span data-stu-id="c322c-463">The method executes an infinite loop that watches for new queue messages and processes them when they arrive.</span></span>

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

<span data-ttu-id="c322c-464">Om inget kömeddelande hittas efter varje upprepning av loopen, försätts programmet i viloläge i en sekund.</span><span class="sxs-lookup"><span data-stu-id="c322c-464">After each iteration of the loop, if no queue message was found, the program sleeps for a second.</span></span> <span data-ttu-id="c322c-465">Det förhindrar att arbetsrollen använder orimlig processortid och orsakar lagringstransaktionskostnader.</span><span class="sxs-lookup"><span data-stu-id="c322c-465">This prevents the worker role from incurring excessive CPU time and storage transaction costs.</span></span> <span data-ttu-id="c322c-466">Microsoft Customer Advisory Team berättar om en utvecklare som glömde bort att ta med det här, distribuerade till produktion, och åkte på semester.</span><span class="sxs-lookup"><span data-stu-id="c322c-466">The Microsoft Customer Advisory Team tells a story about a  developer who forgot to include this, deployed to production, and left for vacation.</span></span> <span data-ttu-id="c322c-467">När han fick tillbaka kostade hans misstag mer än semestern.</span><span class="sxs-lookup"><span data-stu-id="c322c-467">When he got back, his oversight cost more than the vacation.</span></span>

<span data-ttu-id="c322c-468">Ibland orsakar innehållet i ett kömeddelande ett fel i bearbetningen.</span><span class="sxs-lookup"><span data-stu-id="c322c-468">Sometimes the content of a queue message causes an error in processing.</span></span> <span data-ttu-id="c322c-469">Det kallas för *meddelande om ej utförd åtgärd*, och om du precis har loggat ett fel och startat loopen igen kan du försöka bearbeta det meddelandet i oändlighet.</span><span class="sxs-lookup"><span data-stu-id="c322c-469">This is called a *poison message*, and if you just logged an error and restarted the loop, you could endlessly try to process that message.</span></span>  <span data-ttu-id="c322c-470">Därför innehåller catch-blocket en if-sats som kontrollerar hur många gånger appen har försökt att bearbeta det aktuella meddelandet, och om det har hänt fler än fem gånger, tas meddelandet bort från kön.</span><span class="sxs-lookup"><span data-stu-id="c322c-470">Therefore the catch block includes an if statement that checks to see how many times the app has tried to process the current message, and if it has been more than 5 times, the message is deleted from the queue.</span></span>

<span data-ttu-id="c322c-471">`ProcessQueueMessage` anropas när ett kömeddelande hittas.</span><span class="sxs-lookup"><span data-stu-id="c322c-471">`ProcessQueueMessage` is called when a queue message is found.</span></span>

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

<span data-ttu-id="c322c-472">Den här koden läser databasen för att hämta bildens URL, konverterar bilden till en miniatyrbild, sparar miniatyren i en blob, uppdaterar databasen med URL:en för miniatyren i bloben och tar bort kömeddelandet.</span><span class="sxs-lookup"><span data-stu-id="c322c-472">This code reads the database to get the image URL, converts the image to a thumbnail, saves the thumbnail in a blob, updates the database with the thumbnail blob URL, and deletes the queue message.</span></span>

> [!NOTE]
> <span data-ttu-id="c322c-473">Koden i metoden `ConvertImageToThumbnailJPG` använder klasser i namnområdet System.Drawing för enkelhetens skull.</span><span class="sxs-lookup"><span data-stu-id="c322c-473">The code in the `ConvertImageToThumbnailJPG` method uses classes in the System.Drawing namespace for simplicity.</span></span> <span data-ttu-id="c322c-474">Klasserna i det här namnområdet har emellertid skapats för användning med Windows Forms.</span><span class="sxs-lookup"><span data-stu-id="c322c-474">However, the classes in this namespace were designed for use with Windows Forms.</span></span> <span data-ttu-id="c322c-475">De stöds inte för användning i en Windows- eller ASP.NET-tjänst.</span><span class="sxs-lookup"><span data-stu-id="c322c-475">They are not supported for use in a Windows or ASP.NET service.</span></span> <span data-ttu-id="c322c-476">Mer information om alternativ för bildbearbetning finns i [Dynamic Image Generation](http://www.hanselman.com/blog/BackToBasicsDynamicImageGenerationASPNETControllersRoutingIHttpHandlersAndRunAllManagedModulesForAllRequests.aspx) och [Deep Inside Image Resizing](http://www.hanselminutes.com/313/deep-inside-image-resizing-and-scaling-with-aspnet-and-iis-with-imageresizingnet-author-na).</span><span class="sxs-lookup"><span data-stu-id="c322c-476">For more information about image-processing options, see [Dynamic Image Generation](http://www.hanselman.com/blog/BackToBasicsDynamicImageGenerationASPNETControllersRoutingIHttpHandlersAndRunAllManagedModulesForAllRequests.aspx) and [Deep Inside Image Resizing](http://www.hanselminutes.com/313/deep-inside-image-resizing-and-scaling-with-aspnet-and-iis-with-imageresizingnet-author-na).</span></span>
>
>

## <a name="troubleshooting"></a><span data-ttu-id="c322c-477">Felsökning</span><span class="sxs-lookup"><span data-stu-id="c322c-477">Troubleshooting</span></span>
<span data-ttu-id="c322c-478">Om något inte fungerar när du följer anvisningarna i den här kursen visar vi här några exempel på vanliga fel och hur du kan lösa dem.</span><span class="sxs-lookup"><span data-stu-id="c322c-478">In case something doesn't work while you're following the instructions in this tutorial, here are some common errors and how to resolve them.</span></span>

### <a name="serviceruntimeroleenvironmentexception"></a><span data-ttu-id="c322c-479">ServiceRuntime.RoleEnvironmentException</span><span class="sxs-lookup"><span data-stu-id="c322c-479">ServiceRuntime.RoleEnvironmentException</span></span>
<span data-ttu-id="c322c-480">Objektet `RoleEnvironment` tillhandahålls av Azure när du kör ett program i Azure eller när du kör programmet lokalt med Azure-beräkningsemulatorn.</span><span class="sxs-lookup"><span data-stu-id="c322c-480">The `RoleEnvironment` object is provided by Azure when you run an application in Azure or when you run locally using the Azure compute emulator.</span></span>  <span data-ttu-id="c322c-481">Om det här felet uppstår när du kör programmet lokalt ska du kontrollera att du har angett ContosoAdsCloudService-projektet som startprojekt.</span><span class="sxs-lookup"><span data-stu-id="c322c-481">If you get this error when you're running locally, make sure that you have set the ContosoAdsCloudService project as the startup project.</span></span> <span data-ttu-id="c322c-482">Då konfigureras projektet att köras med Azure-beräkningsemulatorn.</span><span class="sxs-lookup"><span data-stu-id="c322c-482">This sets up the project to run using the Azure compute emulator.</span></span>

<span data-ttu-id="c322c-483">En av de saker som programmet använder Azure RoleEnvironment till är att hämta anslutningssträngvärdena som lagras i *.cscfg*-filerna, så en annan möjlig orsak till det här undantaget är en anslutningssträng som saknas.</span><span class="sxs-lookup"><span data-stu-id="c322c-483">One of the things the application uses the Azure RoleEnvironment for is to get the connection string values that are stored in the *.cscfg* files, so another cause of this exception is a missing connection string.</span></span> <span data-ttu-id="c322c-484">Kontrollera att du har skapat StorageConnectionString-inställningen både för moln- och lokala konfigurationer i ContosoAdsWeb-projektet, samt att du har skapat båda anslutningssträngar för båda konfigurationerna i ContosoAdsWorker-projektet.</span><span class="sxs-lookup"><span data-stu-id="c322c-484">Make sure that you created the StorageConnectionString setting for both Cloud and Local configurations in the ContosoAdsWeb project, and that you created both connection strings for both configurations in the ContosoAdsWorker project.</span></span> <span data-ttu-id="c322c-485">Om du gör sökningen **Sök alla** efter StorageConnectionString i hela lösningen, bör du se den nio gånger i sex filer.</span><span class="sxs-lookup"><span data-stu-id="c322c-485">If you do a **Find All** search for StorageConnectionString in the entire solution, you should see it 9 times in 6 files.</span></span>

### <a name="cannot-override-to-port-xxx-new-port-below-minimum-allowed-value-8080-for-protocol-http"></a><span data-ttu-id="c322c-486">Cannot override to port xxx. (Det går inte att åsidosätta till port xxx.)</span><span class="sxs-lookup"><span data-stu-id="c322c-486">Cannot override to port xxx.</span></span> <span data-ttu-id="c322c-487">New port below minimum allowed value 8080 for protocol http. (Ny port under minsta tillåtna värde 8080 för protokollet http.)</span><span class="sxs-lookup"><span data-stu-id="c322c-487">New port below minimum allowed value 8080 for protocol http</span></span>
<span data-ttu-id="c322c-488">Försök att ändra portnumret som används av webbprojektet.</span><span class="sxs-lookup"><span data-stu-id="c322c-488">Try changing the port number used by the web project.</span></span> <span data-ttu-id="c322c-489">Högerklicka på ContosoAdsWeb-projektet och klicka sedan på **Properties** (Egenskaper).</span><span class="sxs-lookup"><span data-stu-id="c322c-489">Right-click the ContosoAdsWeb project, and then click **Properties**.</span></span> <span data-ttu-id="c322c-490">Klicka på fliken **Web** (Webb) och ändra sedan portnumret i inställningen **Project Url** (Projekt-URL).</span><span class="sxs-lookup"><span data-stu-id="c322c-490">Click the **Web** tab, and then change the port number in the **Project Url** setting.</span></span>

<span data-ttu-id="c322c-491">Ett annat alternativ som eventuellt kan lösa problemet finns i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="c322c-491">For another alternative that might resolve the problem, see the following  section.</span></span>

### <a name="other-errors-when-running-locally"></a><span data-ttu-id="c322c-492">Andra fel när du kör programmet lokalt</span><span class="sxs-lookup"><span data-stu-id="c322c-492">Other errors when running locally</span></span>
<span data-ttu-id="c322c-493">Som standard använder nya molntjänstprojekt expressversionen av Azure-beräkningsemulatorn för att simulera Azure-miljön.</span><span class="sxs-lookup"><span data-stu-id="c322c-493">By default new cloud service projects use the Azure compute emulator express to simulate the Azure environment.</span></span> <span data-ttu-id="c322c-494">Detta är en förenklad version av den fullständiga beräkningsemulatorn, och under vissa förhållanden fungerar den fullständiga emulatorn men inte expressversionen.</span><span class="sxs-lookup"><span data-stu-id="c322c-494">This is a lightweight version of the full compute emulator, and under some conditions the full emulator will work when the express version does not.</span></span>  

<span data-ttu-id="c322c-495">Om du vill ändra så att projektet använder den fullständiga emulatorn, högerklickar du på ContosoAdsCloudService-projektet och klickar sedan på **Properties** (Egenskaper).</span><span class="sxs-lookup"><span data-stu-id="c322c-495">To change the project to use the full emulator, right-click the ContosoAdsCloudService project, and then click **Properties**.</span></span> <span data-ttu-id="c322c-496">Klicka på fliken **Web** (Webb) i fönstret **Properties** (Egenskaper), och markera sedan alternativknappen **Use Full Emulator** (Använd fullständig emulator).</span><span class="sxs-lookup"><span data-stu-id="c322c-496">In the **Properties** window click the **Web** tab, and then click the **Use Full Emulator** radio button.</span></span>

<span data-ttu-id="c322c-497">Du måste öppna Visual Studio med administratörsbehörighet för att köra programmet med den fullständiga emulatorn.</span><span class="sxs-lookup"><span data-stu-id="c322c-497">In order to run the application with the full emulator, you have to open Visual Studio with administrator privileges.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c322c-498">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c322c-498">Next steps</span></span>
<span data-ttu-id="c322c-499">Contoso Ads-programmet har med avsikt förenklats för den här komma igång-kursen.</span><span class="sxs-lookup"><span data-stu-id="c322c-499">The Contoso Ads application has intentionally been kept simple for a getting-started tutorial.</span></span> <span data-ttu-id="c322c-500">Det implementerar exempelvis inte [beroendeinmatning](http://www.asp.net/mvc/tutorials/hands-on-labs/aspnet-mvc-4-dependency-injection) eller [centrallager och arbetsenhetsmönster](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application#repo), det använder inte [ett gränssnitt för loggning](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry#log) och inte heller [EF Code First Migrations](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application) för att hantera datamodelländringar eller [EF Connection Resiliency](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application) för att hantera tillfälliga nätverksfel osv.</span><span class="sxs-lookup"><span data-stu-id="c322c-500">For example, it doesn't implement [dependency injection](http://www.asp.net/mvc/tutorials/hands-on-labs/aspnet-mvc-4-dependency-injection) or the [repository and unit of work patterns](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application#repo), it doesn't [use an interface for logging](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry#log), it doesn't use [EF Code First Migrations](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application) to manage data model changes or [EF Connection Resiliency](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application) to manage transient network errors, and so forth.</span></span>

<span data-ttu-id="c322c-501">Här följer några exempelprogram för molntjänster som visar fler verklighetsbaserade kodningsexempel, i ordningen från mindre till mer komplexa:</span><span class="sxs-lookup"><span data-stu-id="c322c-501">Here are some cloud service sample applications that demonstrate more real-world coding practices, listed from less complex to more complex:</span></span>

* <span data-ttu-id="c322c-502">[PhluffyFotos](http://code.msdn.microsoft.com/PhluffyFotos-Sample-7ecffd31).</span><span class="sxs-lookup"><span data-stu-id="c322c-502">[PhluffyFotos](http://code.msdn.microsoft.com/PhluffyFotos-Sample-7ecffd31).</span></span> <span data-ttu-id="c322c-503">Liknande koncept som i Contoso Ads men här finns fler funktioner och fler verklighetsbaserade kodningsexempel.</span><span class="sxs-lookup"><span data-stu-id="c322c-503">Similar in concept to Contoso Ads but implements more features and more real-world coding practices.</span></span>
* <span data-ttu-id="c322c-504">[Azure Cloud Service Multi-Tier Application with Tables, Queues, and Blobs](http://code.msdn.microsoft.com/windowsazure/Windows-Azure-Multi-Tier-eadceb36).</span><span class="sxs-lookup"><span data-stu-id="c322c-504">[Azure Cloud Service Multi-Tier Application with Tables, Queues, and Blobs](http://code.msdn.microsoft.com/windowsazure/Windows-Azure-Multi-Tier-eadceb36).</span></span> <span data-ttu-id="c322c-505">Introducerar Azure Storage-tabeller samt blobbar och köer.</span><span class="sxs-lookup"><span data-stu-id="c322c-505">Introduces Azure Storage tables as well as blobs and queues.</span></span> <span data-ttu-id="c322c-506">Baserat på en äldre version av Azure SDK för .NET, kräver vissa ändringar för att fungera med den aktuella versionen.</span><span class="sxs-lookup"><span data-stu-id="c322c-506">Based on an older version of the Azure SDK for .NET, will require some modifications to work with the current version.</span></span>
* <span data-ttu-id="c322c-507">[Cloud Service Fundamentals in Microsoft Azure](http://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649).</span><span class="sxs-lookup"><span data-stu-id="c322c-507">[Cloud Service Fundamentals in Microsoft Azure](http://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649).</span></span> <span data-ttu-id="c322c-508">Ett omfattande exempel som visar ett stort urval med bästa arbetsmetoder som har tagits fram av Microsoft Patterns and Practices-gruppen.</span><span class="sxs-lookup"><span data-stu-id="c322c-508">A comprehensive sample demonstrating a wide range of best practices, produced by the Microsoft Patterns and Practices group.</span></span>

<span data-ttu-id="c322c-509">Allmän information om hur du utvecklar för molnet finns i [Skapa verkliga molnappar med Azure](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction).</span><span class="sxs-lookup"><span data-stu-id="c322c-509">For general information about developing for the cloud, see [Building Real-World Cloud Apps with Azure](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction).</span></span>

<span data-ttu-id="c322c-510">Om du vill se en videointroduktion till bästa metoder och mönster i Azure Storage, kan du se [Microsoft Azure Storage – What's New, Best Practices and Patterns](http://channel9.msdn.com/Events/Build/2014/3-628).</span><span class="sxs-lookup"><span data-stu-id="c322c-510">For a video introduction to Azure Storage best practices and patterns, see [Microsoft Azure Storage – What's New, Best Practices and Patterns](http://channel9.msdn.com/Events/Build/2014/3-628).</span></span>

<span data-ttu-id="c322c-511">Mer information finns i följande resurser:</span><span class="sxs-lookup"><span data-stu-id="c322c-511">For more information, see the following resources:</span></span>

* [<span data-ttu-id="c322c-512">Azure Cloud Services, del 1: Inledning</span><span class="sxs-lookup"><span data-stu-id="c322c-512">Azure Cloud Services Part 1: Introduction</span></span>](http://justazure.com/microsoft-azure-cloud-services-part-1-introduction/)
* [<span data-ttu-id="c322c-513">Hantera molntjänster</span><span class="sxs-lookup"><span data-stu-id="c322c-513">How to manage Cloud Services</span></span>](cloud-services-how-to-manage.md)
* [<span data-ttu-id="c322c-514">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="c322c-514">Azure Storage</span></span>](/documentation/services/storage/)
* [<span data-ttu-id="c322c-515">Hur man väljer molntjänstleverantör</span><span class="sxs-lookup"><span data-stu-id="c322c-515">How to choose a cloud service provider</span></span>](https://azure.microsoft.com/overview/choosing-a-cloud-service-provider/)
