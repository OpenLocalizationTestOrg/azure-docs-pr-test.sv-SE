---
title: aaaCreate ett Webbjobb .NET i Azure App Service | Microsoft Docs
description: "Skapa en flernivåapp med ASP.NET MVC och Azure. hello front end körs i en webbapp i Azure App Service och hello tillbaka slutet körs som ett Webbjobb. hello appen använder Entity Framework, SQL Database och Azure storage-köer och blobbar."
services: app-service
documentationcenter: .net
author: tdykstra
manager: erikre
editor: mollybos
ms.assetid: 99cb9917-483a-45f8-a98d-07d19c68c753
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 6/14/2017
ms.author: glenga
ms.openlocfilehash: d92fc4b81cc0d58f8e900f257e747af56d32b911
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-net-webjob-in-azure-app-service"></a><span data-ttu-id="199aa-105">Skapa ett .NET WebJob i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="199aa-105">Create a .NET WebJob in Azure App Service</span></span>
<span data-ttu-id="199aa-106">Den här kursen visar hur toowrite Platskod för en enkel ASP.NET MVC 5 flernivåapp som använder hello [WebJobs SDK](websites-dotnet-webjobs-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="199aa-106">This tutorial shows how toowrite code for a simple multi-tier ASP.NET MVC 5 application that uses hello [WebJobs SDK](websites-dotnet-webjobs-sdk.md).</span></span>

[!INCLUDE [app-service-web-webjobs-corenote](../../includes/app-service-web-webjobs-corenote.md)]

<span data-ttu-id="199aa-107">Hej syftet hello [WebJobs SDK](websites-webjobs-resources.md) är toosimplify hello koden du skriver för vanliga uppgifter som ett Webbjobb kan utföra, till exempel bild bearbetning, kö bearbetning, RSS aggregering, filunderhåll, och skicka e-post.</span><span class="sxs-lookup"><span data-stu-id="199aa-107">hello purpose of hello [WebJobs SDK](websites-webjobs-resources.md) is toosimplify hello code you write for common tasks that a WebJob can perform, such as image processing, queue processing, RSS aggregation, file maintenance, and sending emails.</span></span> <span data-ttu-id="199aa-108">Hej WebJobs SDK har inbyggda funktioner för att arbeta med Azure Storage- och Service Bus för schemalagda aktiviteter och hantera fel och för många andra vanliga scenarier.</span><span class="sxs-lookup"><span data-stu-id="199aa-108">hello WebJobs SDK has built-in features for working with Azure Storage and Service Bus, for scheduling tasks and handling errors, and for many other common scenarios.</span></span> <span data-ttu-id="199aa-109">Dessutom har utformats för toobe extensible och det finns en [öppen källkod lagringsplatsen för tillägg](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview).</span><span class="sxs-lookup"><span data-stu-id="199aa-109">In addition, it's designed toobe extensible, and there's an [open source repository for extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview).</span></span>

<span data-ttu-id="199aa-110">hello exempelprogrammet är en anslagstavla för annonser.</span><span class="sxs-lookup"><span data-stu-id="199aa-110">hello sample application is an advertising bulletin board.</span></span> <span data-ttu-id="199aa-111">Användarna kan ladda upp bilder för annonser och en backend-process konverterar hello bilder toothumbnails.</span><span class="sxs-lookup"><span data-stu-id="199aa-111">Users can upload images for ads, and a backend process converts hello images toothumbnails.</span></span> <span data-ttu-id="199aa-112">hello ad sidan visar hello miniatyrer och hello ad informationssidan visar hello bilden.</span><span class="sxs-lookup"><span data-stu-id="199aa-112">hello ad list page shows hello thumbnails, and hello ad details page shows hello full-size image.</span></span> <span data-ttu-id="199aa-113">Här är en skärmbild:</span><span class="sxs-lookup"><span data-stu-id="199aa-113">Here's a screenshot:</span></span>

![Annonslista](./media/websites-dotnet-webjobs-sdk-get-started/list.png)

<span data-ttu-id="199aa-115">Det här exempelprogrammet fungerar med [Azure köer](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern) och [Azure BLOB](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage).</span><span class="sxs-lookup"><span data-stu-id="199aa-115">This sample application works with [Azure queues](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern) and [Azure blobs](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage).</span></span> <span data-ttu-id="199aa-116">hello kursen visar hur toodeploy hello program för[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) och [Azure SQL Database](http://msdn.microsoft.com/library/azure/ee336279).</span><span class="sxs-lookup"><span data-stu-id="199aa-116">hello tutorial shows how toodeploy hello application too[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) and [Azure SQL Database](http://msdn.microsoft.com/library/azure/ee336279).</span></span>

## <span data-ttu-id="199aa-117"><a id="prerequisites"></a>Förhandskrav</span><span class="sxs-lookup"><span data-stu-id="199aa-117"><a id="prerequisites"></a>Prerequisites</span></span>
<span data-ttu-id="199aa-118">hello kursen förutsätter att du vet hur toowork med [ASP.NET MVC 5](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started) projekt i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="199aa-118">hello tutorial assumes that you know how toowork with [ASP.NET MVC 5](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started) projects in Visual Studio.</span></span>

<span data-ttu-id="199aa-119">hello kursen ursprungligen skrevs för Visual Studio 2013, men kan användas med senare versioner av Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="199aa-119">hello tutorial was originally written for Visual Studio 2013, but can be used with later versions of Visual Studio.</span></span> <span data-ttu-id="199aa-120">Om du använder Visual Studio 2015 eller 2017, notera att innan du kör programmet hello lokalt, måste du ändra hello `Data Source` tillhör hello SQL Server LocalDB-anslutningssträngen i hello Web.config och App.config filer från `Data Source=(localdb)\v11.0` för`Data Source=(LocalDb)\MSSQLLocalDB`.</span><span class="sxs-lookup"><span data-stu-id="199aa-120">If you are using Visual Studio 2015 or 2017, make note that before you run hello application locally, you must change hello `Data Source` part of hello SQL Server LocalDB connection string in hello Web.config and App.config files from `Data Source=(localdb)\v11.0` too`Data Source=(LocalDb)\MSSQLLocalDB`.</span></span>

> [!NOTE]
> <span data-ttu-id="199aa-121"><a name="note"></a>Du måste ha en Azure-konto toocomplete den här kursen:</span><span class="sxs-lookup"><span data-stu-id="199aa-121"><a name="note"></a>You must have an Azure account toocomplete this tutorial:</span></span>
>
> * <span data-ttu-id="199aa-122">Du kan [öppna ett Azure-konto gratis](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F): du får kredit att du kan använda tootry ut betald Azure-tjänster och även när de används, kan du hålla hello-konto och använda kostnadsfria Azure-tjänster, till exempel Websites.</span><span class="sxs-lookup"><span data-stu-id="199aa-122">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F): You get credits that you can use tootry out paid Azure services, and even after they're used up, you can keep hello account and use free Azure services, such as Websites.</span></span> <span data-ttu-id="199aa-123">Ditt kreditkort kommer aldrig att debiteras, såvida du inte uttryckligen ändrar dina inställningar och be toobe debiteras.</span><span class="sxs-lookup"><span data-stu-id="199aa-123">Your credit card will never be charged, unless you explicitly change your settings and ask toobe charged.</span></span>
> * <span data-ttu-id="199aa-124">Du kan [aktivera månatlig Azure-kredit för Visual Studio-prenumeranter](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): din prenumeration ger dig krediter varje månad som du kan använda för Azure-betaltjänster.</span><span class="sxs-lookup"><span data-stu-id="199aa-124">You can [activate Monthly Azure credit for Visual Studio subscribers](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): Your subscription gives you credits every month that you can use for paid Azure services.</span></span>
>
> <span data-ttu-id="199aa-125">Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service.</span><span class="sxs-lookup"><span data-stu-id="199aa-125">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="199aa-126">Inget kreditkort krävs, och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="199aa-126">No credit cards required; no commitments.</span></span>
>
>

## <span data-ttu-id="199aa-127"><a id="learn"></a>Lär du dig</span><span class="sxs-lookup"><span data-stu-id="199aa-127"><a id="learn"></a>What you'll learn</span></span>
<span data-ttu-id="199aa-128">hello kursen visar hur toodo hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="199aa-128">hello tutorial shows how toodo hello following tasks:</span></span>

* <span data-ttu-id="199aa-129">Aktivera datorn för Azure-utveckling genom att installera hello Azure SDK (endast för Visual Studio 2013 och 2015 användare).</span><span class="sxs-lookup"><span data-stu-id="199aa-129">Enable your machine for Azure development by installing hello Azure SDK (only for Visual Studio 2013 and 2015 users).</span></span>
* <span data-ttu-id="199aa-130">Skapa ett konsolprogram projekt som distribuerar automatiskt som en Azure-Webbjobb när du distribuerar hello associerade webbprojekt.</span><span class="sxs-lookup"><span data-stu-id="199aa-130">Create a Console Application project that automatically deploys as an Azure WebJob when you deploy hello associated web project.</span></span>
* <span data-ttu-id="199aa-131">Testa en WebJobs-SDK-serverdel lokalt på utvecklingsdatorn hello.</span><span class="sxs-lookup"><span data-stu-id="199aa-131">Test a WebJobs SDK backend locally on hello development computer.</span></span>
* <span data-ttu-id="199aa-132">Publicera ett program med en WebJobs backend tooa webbapp i App Service.</span><span class="sxs-lookup"><span data-stu-id="199aa-132">Publish an application with a WebJobs backend tooa web app in App Service.</span></span>
* <span data-ttu-id="199aa-133">Ladda upp filer och lagrar dem i hello Azure Blob-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="199aa-133">Upload files and store them in hello Azure Blob service.</span></span>
* <span data-ttu-id="199aa-134">Använda hello Azure WebJobs SDK toowork med Azure Storage-köer och blobbar.</span><span class="sxs-lookup"><span data-stu-id="199aa-134">Use hello Azure WebJobs SDK toowork with Azure Storage queues and blobs.</span></span>

## <span data-ttu-id="199aa-135"><a id="contosoads"></a>Programarkitektur</span><span class="sxs-lookup"><span data-stu-id="199aa-135"><a id="contosoads"></a>Application architecture</span></span>
<span data-ttu-id="199aa-136">hello exempelprogrammet använder hello [köcentriska mönster](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern) toooff belastningen hello processorintensiva arbetet med att skapa miniatyrbilder tooa backend-processen.</span><span class="sxs-lookup"><span data-stu-id="199aa-136">hello sample application uses hello [queue-centric work pattern](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern) toooff-load hello CPU-intensive work of creating thumbnails tooa backend process.</span></span>

<span data-ttu-id="199aa-137">hello appen lagrar annonser i en SQL-databas med hjälp av Entity Framework Code First toocreate hello tabellerna och komma åt hello data.</span><span class="sxs-lookup"><span data-stu-id="199aa-137">hello app stores ads in a SQL database, using Entity Framework Code First toocreate hello tables and access hello data.</span></span> <span data-ttu-id="199aa-138">För varje ad hello databasen lagrar två URL: en för hello bilden och en för hello miniatyr.</span><span class="sxs-lookup"><span data-stu-id="199aa-138">For each ad, hello database stores two URLs: one for hello full-size image and one for hello thumbnail.</span></span>

![Annonstabell](./media/websites-dotnet-webjobs-sdk-get-started/adtable.png)

<span data-ttu-id="199aa-140">När en användare laddar upp en bild, hello webbprogram lagrar hello bilden i en [Azure blob](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage), och den lagrar Hej ad i hello-databas med en URL som pekar toohello blob.</span><span class="sxs-lookup"><span data-stu-id="199aa-140">When a user uploads an image, hello web app stores hello image in an [Azure blob](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage), and it stores hello ad information in hello database with a URL that points toohello blob.</span></span> <span data-ttu-id="199aa-141">AT hello samma time, skriver den ett meddelande tooan Azure kön.</span><span class="sxs-lookup"><span data-stu-id="199aa-141">At hello same time, it writes a message tooan Azure queue.</span></span> <span data-ttu-id="199aa-142">I en backend-process som körs som en Azure-Webbjobb, avsöker hello WebJobs SDK hello kön efter nya meddelanden.</span><span class="sxs-lookup"><span data-stu-id="199aa-142">In a backend process running as an Azure WebJob, hello WebJobs SDK polls hello queue for new messages.</span></span> <span data-ttu-id="199aa-143">När ett nytt meddelande visas hello Webbjobb skapar en miniatyrbild för den bilden och uppdateringar hello miniatyrbildens URL-databasfält för den annonsen.</span><span class="sxs-lookup"><span data-stu-id="199aa-143">When a new message appears, hello WebJob creates a thumbnail for that image and updates hello thumbnail URL database field for that ad.</span></span> <span data-ttu-id="199aa-144">Här är ett diagram som visar hur hello delar av programmet hello samverkar:</span><span class="sxs-lookup"><span data-stu-id="199aa-144">Here's a diagram that shows how hello parts of hello application interact:</span></span>

![Contoso Ads-arkitektur](./media/websites-dotnet-webjobs-sdk-get-started/apparchitecture.png)

[!INCLUDE [install-sdk](../../includes/install-sdk-2017-2015-2013.md)]  
<span data-ttu-id="199aa-146">hello självstudiekursen instruktioner gäller tooAzure SDK för .NET 2.7.1 eller senare.</span><span class="sxs-lookup"><span data-stu-id="199aa-146">hello tutorial instructions apply tooAzure SDK for .NET 2.7.1 or later.</span></span>

## <span data-ttu-id="199aa-147"><a id="storage"></a>Skapa ett Azure Storage-konto</span><span class="sxs-lookup"><span data-stu-id="199aa-147"><a id="storage"></a>Create an Azure Storage account</span></span>
<span data-ttu-id="199aa-148">Ett Azure storage-konto innehåller resurser för att lagra kö-och blobbdata i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="199aa-148">An Azure storage account provides resources for storing queue and blob data in hello cloud.</span></span> <span data-ttu-id="199aa-149">Det används också av hello WebJobs SDK toostore loggningsdata för hello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="199aa-149">It's also used by hello WebJobs SDK toostore logging data for hello dashboard.</span></span>

<span data-ttu-id="199aa-150">I ett riktigt program skapar du vanligtvis separata konton för programdata jämfört med loggningsdata och separata konton för testdata jämfört med produktionsdata.</span><span class="sxs-lookup"><span data-stu-id="199aa-150">In a real-world application, you typically create separate accounts for application data versus logging data and separate accounts for test data versus production data.</span></span> <span data-ttu-id="199aa-151">Under den här kursen använder du bara ett konto.</span><span class="sxs-lookup"><span data-stu-id="199aa-151">For this tutorial, you'll use just one account.</span></span>

1. <span data-ttu-id="199aa-152">Öppna hello **Server Explorer** fönstret i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="199aa-152">Open hello **Server Explorer** window in Visual Studio.</span></span>
2. <span data-ttu-id="199aa-153">Högerklicka på hello **Azure** noden och klicka sedan på **ansluta tooMicrosoft Azure-prenumerationen...** .</span><span class="sxs-lookup"><span data-stu-id="199aa-153">Right-click hello **Azure** node, and then click **Connect tooMicrosoft Azure Subscription...**.</span></span>
   
   ![Ansluta tooAzure](./media/websites-dotnet-webjobs-sdk-get-started/connaz.png)

3. <span data-ttu-id="199aa-155">Logga in med dina autentiseringsuppgifter för Azure.</span><span class="sxs-lookup"><span data-stu-id="199aa-155">Sign in using your Azure credentials.</span></span>
4. <span data-ttu-id="199aa-156">Högerklicka på **lagring** under hello Azure noden och klicka sedan på **skapa Lagringskonto**.</span><span class="sxs-lookup"><span data-stu-id="199aa-156">Right-click **Storage** under hello Azure node, and then click **Create Storage Account**.</span></span>
   
   ![Skapa Lagringskonto](./media/websites-dotnet-webjobs-sdk-get-started/createstor.png)
   
5. <span data-ttu-id="199aa-158">I hello **skapa Lagringskonto** dialogrutan, ange ett namn för hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="199aa-158">In hello **Create Storage Account** dialog, enter a name for hello storage account.</span></span>

    <span data-ttu-id="199aa-159">hello namn måste vara unika (ingen Azure storage-konto kan ha hello samma namn).</span><span class="sxs-lookup"><span data-stu-id="199aa-159">hello name must be must be unique (no other Azure storage account can have hello same name).</span></span> <span data-ttu-id="199aa-160">Om hello namnet redan används, får du en chans toochange den.</span><span class="sxs-lookup"><span data-stu-id="199aa-160">If hello name you enter is already in use, you'll get a chance toochange it.</span></span>

    <span data-ttu-id="199aa-161">Hej URL tooaccess storage-konto kommer att *{name}*. core.windows.net.</span><span class="sxs-lookup"><span data-stu-id="199aa-161">hello URL tooaccess your storage account will be *{name}*.core.windows.net.</span></span>
6. <span data-ttu-id="199aa-162">Ange hello **Region eller Affinitetsgrupp** listrutan toohello region närmaste tooyou.</span><span class="sxs-lookup"><span data-stu-id="199aa-162">Set hello **Region or Affinity Group** drop-down list toohello region closest tooyou.</span></span>

    <span data-ttu-id="199aa-163">Den här inställningen anger vilket Azure-datacenter ska vara värd för ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="199aa-163">This setting specifies which Azure datacenter will host your storage account.</span></span> <span data-ttu-id="199aa-164">I den här självstudien blir valet inte någon märkbar skillnad.</span><span class="sxs-lookup"><span data-stu-id="199aa-164">For this tutorial, your choice won't make a noticeable difference.</span></span> <span data-ttu-id="199aa-165">Men för en webbapp för produktion, du vill att webbservern och toobe din storage-konto i hello samma region toominimize svarstid och data utgång avgifter.</span><span class="sxs-lookup"><span data-stu-id="199aa-165">However, for a production web app, you want your web server and your storage account toobe in hello same region toominimize latency and data egress charges.</span></span> <span data-ttu-id="199aa-166">hello-webbprogram (som du skapar senare) datacenter ska vara så nära som möjligt toohello webbläsare åtkomst till webbprogrammet hello i ordning toominimize latens.</span><span class="sxs-lookup"><span data-stu-id="199aa-166">hello web app (which you'll create later) datacenter should be as close as possible toohello browsers accessing hello web app in order toominimize latency.</span></span>
7. <span data-ttu-id="199aa-167">Ange hello **replikering** nedrullningsbara listan för**lokalt redundant**.</span><span class="sxs-lookup"><span data-stu-id="199aa-167">Set hello **Replication** drop-down list too**Locally redundant**.</span></span>

    <span data-ttu-id="199aa-168">När geo-replikering är aktiverat för ett lagringskonto är hello lagras innehållet replikerade tooa sekundärt datacenter tooenable redundans toothat plats vid en större katastrof på hello primär plats.</span><span class="sxs-lookup"><span data-stu-id="199aa-168">When geo-replication is enabled for a storage account, hello stored content is replicated tooa secondary datacenter tooenable failover toothat location in case of a major disaster in hello primary location.</span></span> <span data-ttu-id="199aa-169">Geo-replikering kan medföra ytterligare kostnader.</span><span class="sxs-lookup"><span data-stu-id="199aa-169">Geo-replication can incur additional costs.</span></span> <span data-ttu-id="199aa-170">För testning och utveckling konton vill du normalt inte toopay för geo-replikering.</span><span class="sxs-lookup"><span data-stu-id="199aa-170">For test and development accounts, you generally don't want toopay for geo-replication.</span></span> <span data-ttu-id="199aa-171">Mer information finns i [Skapa, hantera eller ta bort ett lagringskonto](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="199aa-171">For more information, see [Create, manage, or delete a storage account](../storage/common/storage-create-storage-account.md).</span></span>
8. <span data-ttu-id="199aa-172">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="199aa-172">Click **Create**.</span></span>

    ![Nytt lagringskonto](./media/websites-dotnet-webjobs-sdk-get-started/newstorage.png)

## <span data-ttu-id="199aa-174"><a id="download"></a>Hämta hello-program</span><span class="sxs-lookup"><span data-stu-id="199aa-174"><a id="download"></a>Download hello application</span></span>
1. <span data-ttu-id="199aa-175">Hämta och packa upp hello [färdiga lösningen](http://code.msdn.microsoft.com/Simple-Azure-Website-with-b4391eeb).</span><span class="sxs-lookup"><span data-stu-id="199aa-175">Download and unzip hello [completed solution](http://code.msdn.microsoft.com/Simple-Azure-Website-with-b4391eeb).</span></span>
2. <span data-ttu-id="199aa-176">Starta Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="199aa-176">Start Visual Studio.</span></span>
3. <span data-ttu-id="199aa-177">Från hello **filen** väljer du menyn **Öppna > projekt/lösning**, navigera toowhere som du hämtade hello lösningen och öppna sedan lösningsfilen hello.</span><span class="sxs-lookup"><span data-stu-id="199aa-177">From hello **File** menu choose **Open > Project/Solution**, navigate toowhere you downloaded hello solution, and then open hello solution file.</span></span>
4. <span data-ttu-id="199aa-178">Tryck på CTRL + SKIFT + B toobuild hello lösning.</span><span class="sxs-lookup"><span data-stu-id="199aa-178">Press CTRL+SHIFT+B toobuild hello solution.</span></span>

    <span data-ttu-id="199aa-179">Som standard återställer Visual Studio automatiskt hello NuGet-paketinnehållet som inte ingick i hello *.zip* fil.</span><span class="sxs-lookup"><span data-stu-id="199aa-179">By default, Visual Studio automatically restores hello NuGet package content, which was not included in hello *.zip* file.</span></span> <span data-ttu-id="199aa-180">Om inte Återställ hello paket installera dem manuellt genom att gå toohello **hantera NuGet-paket för lösningen** dialogrutan och klicka på hello **återställa** längst hello uppe till höger.</span><span class="sxs-lookup"><span data-stu-id="199aa-180">If hello packages don't restore, install them manually by going toohello **Manage NuGet Packages for Solution** dialog and clicking hello **Restore** button at hello top right.</span></span>
5. <span data-ttu-id="199aa-181">I **Solution Explorer**, se till att **ContosoAdsWeb** är markerad som hello Startprojekt.</span><span class="sxs-lookup"><span data-stu-id="199aa-181">In **Solution Explorer**, make sure that **ContosoAdsWeb** is selected as hello startup project.</span></span>

## <span data-ttu-id="199aa-182"><a id="configurestorage"></a>Konfigurera hello programmet toouse storage-konto</span><span class="sxs-lookup"><span data-stu-id="199aa-182"><a id="configurestorage"></a>Configure hello application toouse your storage account</span></span>
1. <span data-ttu-id="199aa-183">Öppna programmet hello *Web.config* filen i hello ContosoAdsWeb-projektet.</span><span class="sxs-lookup"><span data-stu-id="199aa-183">Open hello application *Web.config* file in hello ContosoAdsWeb project.</span></span>

    <span data-ttu-id="199aa-184">hello-filen innehåller en SQL-anslutningssträng och ett Azure storage-anslutningssträngen för att arbeta med blobbar och köer.</span><span class="sxs-lookup"><span data-stu-id="199aa-184">hello file contains a SQL connection string and an Azure storage connection string for working with blobs and queues.</span></span>

    <span data-ttu-id="199aa-185">hello SQL-anslutningssträng pekar tooa [SQL Server Express LocalDB](http://msdn.microsoft.com/library/hh510202.aspx) databas.</span><span class="sxs-lookup"><span data-stu-id="199aa-185">hello SQL connection string points tooa [SQL Server Express LocalDB](http://msdn.microsoft.com/library/hh510202.aspx) database.</span></span>

    <span data-ttu-id="199aa-186">Hej lagringsanslutningssträng är ett exempel som har platshållare för hello lagringskontots namn och åtkomstnyckel.</span><span class="sxs-lookup"><span data-stu-id="199aa-186">hello storage connection string is an example that has placeholders for hello storage account name and access key.</span></span> <span data-ttu-id="199aa-187">Du måste du ersätta en anslutningssträng som har hello namn och nyckel för ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="199aa-187">You'll replace this with a connection string that has hello name and key of your storage account.</span></span>  

    ```
    <connectionStrings>
        <add name="ContosoAdsContext" connectionString="Data Source=(localdb)\v11.0; Initial Catalog=ContosoAds; Integrated Security=True; MultipleActiveResultSets=True;" providerName="System.Data.SqlClient" />
        <add name="AzureWebJobsStorage" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
    </connectionStrings>
    ```
    <span data-ttu-id="199aa-188">Hej lagringsanslutningssträng heter AzureWebJobsStorage eftersom det är hello namn hello WebJobs SDK använder som standard.</span><span class="sxs-lookup"><span data-stu-id="199aa-188">hello storage connection string is named AzureWebJobsStorage because that's hello name hello WebJobs SDK uses by default.</span></span> <span data-ttu-id="199aa-189">hello samma namn används här så att du har tooset bara en anslutning strängvärde i hello Azure-miljön.</span><span class="sxs-lookup"><span data-stu-id="199aa-189">hello same name is used here so you have tooset only one connection string value in hello Azure environment.</span></span>
2. <span data-ttu-id="199aa-190">I **Server Explorer**, högerklicka på ditt lagringskonto under hello **lagring** noden och klicka sedan på **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="199aa-190">In **Server Explorer**, right-click your storage account under hello **Storage** node, and then click **Properties**.</span></span>

    ![Klicka på Lagringskontoegenskaperna](./media/websites-dotnet-webjobs-sdk-get-started/storppty.png)
3. <span data-ttu-id="199aa-192">I hello **egenskaper** -fönstret klickar du på **Lagringskontonycklar**, och klicka sedan på hello tre punkter.</span><span class="sxs-lookup"><span data-stu-id="199aa-192">In hello **Properties** window, click **Storage Account Keys**, and then click hello ellipsis.</span></span>

    ![Lagringskontonycklar](./media/websites-dotnet-webjobs-sdk-get-started/stor-account-keys.png)
4. <span data-ttu-id="199aa-194">Kopiera hello **anslutningssträngen**.</span><span class="sxs-lookup"><span data-stu-id="199aa-194">Copy hello **Connection String**.</span></span>

    ![Lagring dialogruta för nycklar](./media/websites-dotnet-webjobs-sdk-get-started/cpak.png)
5. <span data-ttu-id="199aa-196">Ersätt hello lagringsanslutningssträngen i hello *Web.config* fil med hello anslutningssträngen som du precis har kopierats.</span><span class="sxs-lookup"><span data-stu-id="199aa-196">Replace hello storage connection string in hello *Web.config* file with hello connection string you just copied.</span></span> <span data-ttu-id="199aa-197">Kontrollera att du väljer allt inuti hello citattecken, men inte inklusive hello citattecken innan du klistrar in.</span><span class="sxs-lookup"><span data-stu-id="199aa-197">Make sure you select everything inside hello quotation marks but not including hello quotation marks before pasting.</span></span>
6. <span data-ttu-id="199aa-198">Öppna hello *App.config* fil i hello ContosoAdsWebJob projekt.</span><span class="sxs-lookup"><span data-stu-id="199aa-198">Open hello *App.config* file in hello ContosoAdsWebJob project.</span></span>

    <span data-ttu-id="199aa-199">Den här filen har två storage-anslutningssträngar, ett för programdata och ett för loggning.</span><span class="sxs-lookup"><span data-stu-id="199aa-199">This file has two storage connection strings, one for application data and one for logging.</span></span> <span data-ttu-id="199aa-200">Du kan använda separata storage-konton för programdata och loggning och du kan använda [flera lagringskonton för data](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).</span><span class="sxs-lookup"><span data-stu-id="199aa-200">You can use separate storage accounts for application data and logging, and you can use [multiple storage accounts for data](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).</span></span> <span data-ttu-id="199aa-201">Den här kursen använder du ett enda storage-konto.</span><span class="sxs-lookup"><span data-stu-id="199aa-201">For this tutorial, you'll use a single storage account.</span></span> <span data-ttu-id="199aa-202">hello anslutningssträngar har platshållare för hello lagringskontonycklar.</span><span class="sxs-lookup"><span data-stu-id="199aa-202">hello connection strings have placeholders for hello storage account keys.</span></span>

    ```
    <configuration>
        <connectionStrings>
            <add name="AzureWebJobsDashboard" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsStorage" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="ContosoAdsContext" connectionString="Data Source=(localdb)\v11.0; Initial Catalog=ContosoAds; Integrated Security=True; MultipleActiveResultSets=True;"/>
    </connectionStrings>
        <startup>
            <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
    </startup>
    </configuration>

    ```

    <span data-ttu-id="199aa-203">Som standard söker hello WebJobs-SDK för anslutningssträngar som heter AzureWebJobsStorage och AzureWebJobsDashboard.</span><span class="sxs-lookup"><span data-stu-id="199aa-203">By default, hello WebJobs SDK looks for connection strings named AzureWebJobsStorage and AzureWebJobsDashboard.</span></span> <span data-ttu-id="199aa-204">Alternativt kan du kan [lagra hello anslutningssträngen, men du vill skicka in explicit toohello `JobHost` objektet](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#config).</span><span class="sxs-lookup"><span data-stu-id="199aa-204">As an alternative, you can [store hello connection string however you want and pass it in explicitly toohello `JobHost` object](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#config).</span></span>
7. <span data-ttu-id="199aa-205">Ersätt båda storage-anslutningssträngar med hello anslutningssträngen som du kopierade tidigare.</span><span class="sxs-lookup"><span data-stu-id="199aa-205">Replace both storage connection strings with hello connection string you copied earlier.</span></span>
8. <span data-ttu-id="199aa-206">Spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="199aa-206">Save your changes.</span></span>

## <span data-ttu-id="199aa-207"><a id="run"></a>Kör hello programmet lokalt</span><span class="sxs-lookup"><span data-stu-id="199aa-207"><a id="run"></a>Run hello application locally</span></span>
1. <span data-ttu-id="199aa-208">toostart hello web klientdel av programmet hello, tryck på CTRL + F5.</span><span class="sxs-lookup"><span data-stu-id="199aa-208">toostart hello web frontend of hello application, press CTRL+F5.</span></span>

    <span data-ttu-id="199aa-209">hello standardwebbläsaren toohello startsidan.</span><span class="sxs-lookup"><span data-stu-id="199aa-209">hello default browser opens toohello home page.</span></span> <span data-ttu-id="199aa-210">(hello webbprojekt körs eftersom du har gjort det hello Startprojekt.)</span><span class="sxs-lookup"><span data-stu-id="199aa-210">(hello web project runs because you've made it hello startup project.)</span></span>

    ![Contoso Ads-startsida](./media/websites-dotnet-webjobs-sdk-get-started/home.png)
2. <span data-ttu-id="199aa-212">toostart hello Webbjobb serverdelen av programmet hello, högerklicka på hello ContosoAdsWebJob projekt i **Solution Explorer**, och klicka sedan på **felsöka** > **Starta ny instans** .</span><span class="sxs-lookup"><span data-stu-id="199aa-212">toostart hello WebJob backend of hello application, right-click hello ContosoAdsWebJob project in **Solution Explorer**, and then click **Debug** > **Start new instance**.</span></span>

    <span data-ttu-id="199aa-213">Ett konsolfönster-programmet öppnas och visar meddelandeloggning som visar hello WebJobs SDK JobHost objektet har startat toorun.</span><span class="sxs-lookup"><span data-stu-id="199aa-213">A console application window opens and displays logging messages indicating hello WebJobs SDK JobHost object has started toorun.</span></span>

    ![Kör programmet konsolfönstret visar att hello-serverdelen](./media/websites-dotnet-webjobs-sdk-get-started/backendrunning.png)
3. <span data-ttu-id="199aa-215">I webbläsaren, klickar du på **skapar en annons**.</span><span class="sxs-lookup"><span data-stu-id="199aa-215">In your browser, click  **Create an Ad**.</span></span>
4. <span data-ttu-id="199aa-216">Ange lite testdata, Välj en bild tooupload och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="199aa-216">Enter some test data, select an image tooupload, and then click **Create**.</span></span>

    ![Sidan Create (Skapa)](./media/websites-dotnet-webjobs-sdk-get-started/create.png)

    <span data-ttu-id="199aa-218">hello appen går toohello indexsidan, men den visar inte en miniatyrbild för hello nya annonsen eftersom den bearbetningen inte har utförts än.</span><span class="sxs-lookup"><span data-stu-id="199aa-218">hello app goes toohello Index page, but it doesn't show a thumbnail for hello new ad because that processing hasn't happened yet.</span></span>

    <span data-ttu-id="199aa-219">Under tiden visar meddelandet loggning i hello konsolfönstret program efter en kort vänta att ett kömeddelande togs emot och har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="199aa-219">Meanwhile, after a short wait a logging message in hello console application window shows that a queue message was received and has been processed.</span></span>

    ![Programmet konsolfönstret visar att ett kömeddelande har bearbetats](./media/websites-dotnet-webjobs-sdk-get-started/backendlogs.png)
5. <span data-ttu-id="199aa-221">När du ser hello loggar meddelanden i hello konsolfönster för programmet, uppdatera hello Index toosee hello miniatyrbilden.</span><span class="sxs-lookup"><span data-stu-id="199aa-221">After you see hello logging messages in hello console application window, refresh hello Index page toosee hello thumbnail.</span></span>

    ![Sidan Index](./media/websites-dotnet-webjobs-sdk-get-started/list.png)
6. <span data-ttu-id="199aa-223">Klicka på **information** för bilden din ad toosee hello.</span><span class="sxs-lookup"><span data-stu-id="199aa-223">Click **Details** for your ad toosee hello full-size image.</span></span>

    ![Sidan Details (Detaljer)](./media/websites-dotnet-webjobs-sdk-get-started/details.png)

<span data-ttu-id="199aa-225">Du har kört programmet hello på den lokala datorn och den använder en SQL Server-databas som finns på datorn, men fungerar med köer och blobbar i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="199aa-225">You've been running hello application on your local computer, and it's using a SQL Server database located on your computer, but it's working with queues and blobs in hello cloud.</span></span> <span data-ttu-id="199aa-226">I följande avsnitt hello kör du programmet hello i hello molnet med hjälp av en databas i molnet samt molnet blobbar och köer.</span><span class="sxs-lookup"><span data-stu-id="199aa-226">In hello following section you'll run hello application in hello cloud, using a cloud database as well as cloud blobs and queues.</span></span>  

## <span data-ttu-id="199aa-227"><a id="runincloud"></a>Kör programmet hello i hello moln</span><span class="sxs-lookup"><span data-stu-id="199aa-227"><a id="runincloud"></a>Run hello application in hello cloud</span></span>
<span data-ttu-id="199aa-228">Gör du hello följande steg toorun hello program i molnet hello:</span><span class="sxs-lookup"><span data-stu-id="199aa-228">You'll do hello following steps toorun hello application in hello cloud:</span></span>

* <span data-ttu-id="199aa-229">Distribuera tooWeb appar.</span><span class="sxs-lookup"><span data-stu-id="199aa-229">Deploy tooWeb Apps.</span></span> <span data-ttu-id="199aa-230">Visual Studio skapar automatiskt en ny webbapp i App Service och en instans av SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="199aa-230">Visual Studio automatically creates a new web app in App Service and a SQL Database instance.</span></span>
* <span data-ttu-id="199aa-231">Konfigurera hello web app toouse kontot för Azure SQL-databasen och lagring.</span><span class="sxs-lookup"><span data-stu-id="199aa-231">Configure hello web app toouse your Azure SQL database and storage account.</span></span>

<span data-ttu-id="199aa-232">När du har skapat några annonser när det körs i molnet hello, ska du visa hello WebJobs SDK instrumentpanelen toosee hello omfattande övervakning toooffer ännu.</span><span class="sxs-lookup"><span data-stu-id="199aa-232">After you've created some ads while running in hello cloud, you'll view hello WebJobs SDK dashboard toosee hello rich monitoring features it has toooffer.</span></span>

### <a name="deploy-tooweb-apps"></a><span data-ttu-id="199aa-233">Distribuera tooWeb appar</span><span class="sxs-lookup"><span data-stu-id="199aa-233">Deploy tooWeb Apps</span></span>

1. <span data-ttu-id="199aa-234">Stäng hello webbläsare och hello konsolfönster för programmet.</span><span class="sxs-lookup"><span data-stu-id="199aa-234">Close hello browser and hello console application window.</span></span>
2. <span data-ttu-id="199aa-235">Åtgärderna i hello hello [publicera tooAzure med SQL Database](https://docs.microsoft.com/azure/app-service-web/app-service-web-tutorial-dotnet-sqldatabase#publish-to-azure-with-sql-database) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="199aa-235">Follow hello steps in hello [Publish tooAzure with SQL Database](https://docs.microsoft.com/azure/app-service-web/app-service-web-tutorial-dotnet-sqldatabase#publish-to-azure-with-sql-database) section.</span></span>
3. <span data-ttu-id="199aa-236">När du har slutfört hello steg för att distribuera fortsätter du med hello återstående uppgifterna i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="199aa-236">After you complete hello steps for deploying, continue with hello remaining tasks in this article.</span></span>

### <a name="configure-hello-web-app-toouse-your-azure-sql-database-and-storage-account"></a><span data-ttu-id="199aa-237">Konfigurera hello web app toouse kontot för Azure SQL-databasen och lagring</span><span class="sxs-lookup"><span data-stu-id="199aa-237">Configure hello web app toouse your Azure SQL database and storage account</span></span>
<span data-ttu-id="199aa-238">Det är en bra säkerhetsrutin för[undvika att sätta känslig information, till exempel anslutningssträngar i filer som lagras i källkodslager](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control#secrets).</span><span class="sxs-lookup"><span data-stu-id="199aa-238">It's a security best practice too[avoid putting sensitive information such as connection strings in files that are stored in source code repositories](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control#secrets).</span></span> <span data-ttu-id="199aa-239">Azure tillhandahåller en sätt toodo som: du kan ange anslutningssträngen och andra inställningsvärden i hello Azure-miljön och ASP.NET-konfiguration för API: er automatiskt välja upp dessa värden när hello app körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="199aa-239">Azure provides a way toodo that: you can set connection string and other setting values in hello Azure environment, and ASP.NET configuration APIs automatically pick up these values when hello app runs in Azure.</span></span> <span data-ttu-id="199aa-240">Du kan ange dessa värden i Azure med hjälp av **Server Explorer**, hello Azure-portalen, Windows PowerShell, eller hello plattformsoberoende kommandoradsgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="199aa-240">You can set these values in Azure by using **Server Explorer**, hello Azure Portal, Windows PowerShell, or hello cross-platform command-line interface.</span></span> <span data-ttu-id="199aa-241">Mer information finns i [hur programmet strängar och anslutningen strängar arbete](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/).</span><span class="sxs-lookup"><span data-stu-id="199aa-241">For more information, see [How Application Strings and Connection Strings Work](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/).</span></span>

<span data-ttu-id="199aa-242">I det här avsnittet kan du använda **Server Explorer** tooset anslutningssträngarnas värden i Azure.</span><span class="sxs-lookup"><span data-stu-id="199aa-242">In this section, you use **Server Explorer** tooset connection string values in Azure.</span></span>

1. <span data-ttu-id="199aa-243">I **Server Explorer**, högerklicka på ditt webbprogram under **Azure > Apptjänst > {din resursgrupp}**, och klicka sedan på **visningsinställningarna**.</span><span class="sxs-lookup"><span data-stu-id="199aa-243">In **Server Explorer**, right-click your web app under **Azure > App Service > {your resource group}**, and then click **View Settings**.</span></span>

    <span data-ttu-id="199aa-244">Hej **Azure Web App** öppnas på hello **Configuration** fliken.</span><span class="sxs-lookup"><span data-stu-id="199aa-244">hello **Azure Web App** window opens on hello **Configuration** tab.</span></span>
2. <span data-ttu-id="199aa-245">Ändra hello namnet på hello DefaultConnection toohello namn för anslutningssträngen du valde när du konfigurerade hello SQL-databas i hello [publicera tooAzure med SQL Database](https://docs.microsoft.com/azure/app-service-web/app-service-web-tutorial-dotnet-sqldatabase#publish-to-azure-with-sql-database) artikel.</span><span class="sxs-lookup"><span data-stu-id="199aa-245">Change hello name of hello DefaultConnection connection string toohello name you chose when you configured hello SQL database in hello [Publish tooAzure with SQL Database](https://docs.microsoft.com/azure/app-service-web/app-service-web-tutorial-dotnet-sqldatabase#publish-to-azure-with-sql-database) article.</span></span>

    <span data-ttu-id="199aa-246">Azure skapas automatiskt den här anslutningssträngen när du skapade hello webbprogrammet med en associerad databas så att den redan har hello rätt värde.</span><span class="sxs-lookup"><span data-stu-id="199aa-246">Azure automatically created this connection string when you created hello web app with an associated database, so it already has hello right connection string value.</span></span> <span data-ttu-id="199aa-247">Du ändrar bara hello namn toowhat koden är ute efter.</span><span class="sxs-lookup"><span data-stu-id="199aa-247">You're changing just hello name toowhat your code is looking for.</span></span>
3. <span data-ttu-id="199aa-248">Lägga till två nya anslutningssträngar som heter AzureWebJobsStorage och AzureWebJobsDashboard.</span><span class="sxs-lookup"><span data-stu-id="199aa-248">Add two new connection strings, named AzureWebJobsStorage and AzureWebJobsDashboard.</span></span> <span data-ttu-id="199aa-249">Ange hello databas för**anpassad**, och ange hello anslutning sträng värdet toohello samma värde som du tidigare för hello *Web.config* och *App.config* filer.</span><span class="sxs-lookup"><span data-stu-id="199aa-249">Set hello database type too**Custom**, and set hello connection string value toohello same value that you used earlier for hello *Web.config* and *App.config* files.</span></span> <span data-ttu-id="199aa-250">(Vara säker på att du inkluderar hello hela anslutningssträngen, inte bara hello snabbtangent, och inte innehåller hello citattecken).</span><span class="sxs-lookup"><span data-stu-id="199aa-250">(Be sure you include hello entire connection string, not just hello access key, and don't include hello quotation marks.)</span></span>

    <span data-ttu-id="199aa-251">Dessa anslutningssträngar som används av hello WebJobs SDK, ett för programdata och ett för loggning.</span><span class="sxs-lookup"><span data-stu-id="199aa-251">These connection strings are used by hello WebJobs SDK, one for application data and one for logging.</span></span> <span data-ttu-id="199aa-252">Som du såg tidigare används hello en för programdata också av hello web front-end-kod.</span><span class="sxs-lookup"><span data-stu-id="199aa-252">As you saw earlier, hello one for application data is also used by hello web front-end code.</span></span>
4. <span data-ttu-id="199aa-253">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="199aa-253">Click **Save**.</span></span>

    ![Anslutningssträngar i Azure Portal](./media/websites-dotnet-webjobs-sdk-get-started/azconnstr.png)
5. <span data-ttu-id="199aa-255">I **Server Explorer**högerklickar du på hello webbapp och klicka sedan på **stoppa**.</span><span class="sxs-lookup"><span data-stu-id="199aa-255">In **Server Explorer**, right-click hello web app, and then click **Stop**.</span></span>
6. <span data-ttu-id="199aa-256">När hello webbprogrammet slutar, högerklicka på hello webbprogrammet igen och klickar på **starta**.</span><span class="sxs-lookup"><span data-stu-id="199aa-256">After hello web app stops, right-click hello web app again, and then click **Start**.</span></span>

   <span data-ttu-id="199aa-257">Hej Webbjobb startar automatiskt när du publicerar, men den stoppas när du ändrar konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="199aa-257">hello WebJob automatically starts when you publish, but it stops when you make a configuration change.</span></span> <span data-ttu-id="199aa-258">toorestart, kan du antingen starta om webbprogrammet hello eller starta om hello Webbjobb i hello [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715).</span><span class="sxs-lookup"><span data-stu-id="199aa-258">toorestart it, you can either restart hello web app or restart hello WebJob in hello [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715).</span></span> <span data-ttu-id="199aa-259">Det är vanligtvis rekommenderade toorestart hello webbprogrammet efter en konfigurationsändring.</span><span class="sxs-lookup"><span data-stu-id="199aa-259">It's generally recommended toorestart hello web app after a configuration change.</span></span>
7. <span data-ttu-id="199aa-260">Uppdatera hello webbläsarfönster som har hello web app-URL i dess adressfältet.</span><span class="sxs-lookup"><span data-stu-id="199aa-260">Refresh hello browser window that has hello web app URL in its address bar.</span></span>

    <span data-ttu-id="199aa-261">hello startsidan visas.</span><span class="sxs-lookup"><span data-stu-id="199aa-261">hello home page appears.</span></span>
8. <span data-ttu-id="199aa-262">Skapa en annons som du gjorde när du [kördes hello program lokalt](https://docs.microsoft.com/azure/app-service-web/websites-dotnet-webjobs-sdk-get-started#a-idrunarun-the-application-locally).</span><span class="sxs-lookup"><span data-stu-id="199aa-262">Create an ad, as you did when you [ran hello application locally](https://docs.microsoft.com/azure/app-service-web/websites-dotnet-webjobs-sdk-get-started#a-idrunarun-the-application-locally).</span></span>

   <span data-ttu-id="199aa-263">hello indexsidan visar utan en miniatyrbild först.</span><span class="sxs-lookup"><span data-stu-id="199aa-263">hello Index page shows without a thumbnail at first.</span></span>
9. <span data-ttu-id="199aa-264">Uppdatera hello sidan efter några sekunder och hello miniatyr visas.</span><span class="sxs-lookup"><span data-stu-id="199aa-264">Refresh hello page after a few seconds and hello thumbnail appears.</span></span>

   <span data-ttu-id="199aa-265">Om hello miniatyren inte visas kan kanske du toowait en minut för hello Webbjobb toorestart.</span><span class="sxs-lookup"><span data-stu-id="199aa-265">If hello thumbnail doesn't appear, you might have toowait a minute or so for hello WebJob toorestart.</span></span> <span data-ttu-id="199aa-266">Om du efter en stund fortfarande visas inte inte kanske har startats hello miniatyr när du uppdaterar sidan hello hello Webbjobb automatiskt.</span><span class="sxs-lookup"><span data-stu-id="199aa-266">If after a while, you still don't see hello thumbnail when you refresh hello page, hello WebJob might not have started automatically.</span></span> <span data-ttu-id="199aa-267">I så fall gå toohello **Apptjänster** bladet i hello [Azure-portalen](https://portal.azure.com/), leta upp ditt webbprogram och klicka sedan på **starta**.</span><span class="sxs-lookup"><span data-stu-id="199aa-267">In that case, go toohello **App Services** blade in hello [Azure portal](https://portal.azure.com/), locate your web app, and then click **Start**.</span></span>

### <a name="view-hello-webjobs-sdk-dashboard"></a><span data-ttu-id="199aa-268">Visa hello WebJobs SDK-instrumentpanelen</span><span class="sxs-lookup"><span data-stu-id="199aa-268">View hello WebJobs SDK dashboard</span></span>
1. <span data-ttu-id="199aa-269">I hello [Azure-portalen](https://portal.azure.com/)väljer hello **Apptjänster bladet**, leta upp ditt webbprogram och välj **WebJobs**.</span><span class="sxs-lookup"><span data-stu-id="199aa-269">In hello [Azure portal](https://portal.azure.com/), select hello **App Services blade**, locate your web app, and select **WebJobs**.</span></span>
3. <span data-ttu-id="199aa-270">Välj hello **loggar** fliken.</span><span class="sxs-lookup"><span data-stu-id="199aa-270">Select hello **Logs** tab.</span></span>

    ![Fliken loggar](./media/websites-dotnet-webjobs-sdk-get-started/log-tab.png)

    <span data-ttu-id="199aa-272">En ny webbläsarflik öppnas toohello WebJobs-SDK-instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="199aa-272">A new browser tab opens toohello WebJobs SDK dashboard.</span></span> <span data-ttu-id="199aa-273">hello instrumentpanelen visar att hello Webbjobb körs och visar en lista över funktioner i koden som hello WebJobs SDK utlöses.</span><span class="sxs-lookup"><span data-stu-id="199aa-273">hello dashboard shows that hello WebJob is running and shows a list of functions in your code that hello WebJobs SDK triggered.</span></span>
4. <span data-ttu-id="199aa-274">Klicka på en hello funktioner toosee information om körs.</span><span class="sxs-lookup"><span data-stu-id="199aa-274">Click one of hello functions toosee details about its execution.</span></span>

    ![Instrumentpanel för WebJobs SDK](./media/websites-dotnet-webjobs-sdk-get-started/wjdashboardhome.png)

    ![Instrumentpanel för WebJobs SDK](./media/websites-dotnet-webjobs-sdk-get-started/wjfunctiondetails.png)

    <span data-ttu-id="199aa-277">Hej **Replay-funktionen** knappen på den här sidan gör hello WebJobs SDK framework toocall hello funktionen igen och den ger dig en möjlighet toochange hello data som skickas toohello funktion först.</span><span class="sxs-lookup"><span data-stu-id="199aa-277">hello **Replay Function** button on this page causes hello WebJobs SDK framework toocall hello function again, and it gives you a chance toochange hello data passed toohello function first.</span></span>

> [!NOTE]
> <span data-ttu-id="199aa-278">När du är klar testning, bör du ta bort hello webbapp, storage-konto och din SQL Database-instans.</span><span class="sxs-lookup"><span data-stu-id="199aa-278">When you're finished testing, consider deleting hello web app, storage account, and your SQL Database instance.</span></span> <span data-ttu-id="199aa-279">hello webbprogrammet är ledig, men hello SQL storage-konto och databasinstans påförs kostnader (men, minimal på grund av toohello liten storlek).</span><span class="sxs-lookup"><span data-stu-id="199aa-279">hello web app is free, but hello SQL storage account and database instance accrue charges (albeit, minimal due toohello small size).</span></span> <span data-ttu-id="199aa-280">Om du lämnar hello-webbapp körs, kan alla som hittar URL: en skapa och visa annonser.</span><span class="sxs-lookup"><span data-stu-id="199aa-280">Also, if you leave hello web app running, anyone who finds your URL can create and view ads.</span></span> 
>
>

### <a name="delete-your-web-app"></a><span data-ttu-id="199aa-281">Ta bort ditt webbprogram</span><span class="sxs-lookup"><span data-stu-id="199aa-281">Delete your web app</span></span>
<span data-ttu-id="199aa-282">Hello portal, gå toohello **Apptjänster** bladet, leta upp och välj ditt webbprogram och klicka sedan på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="199aa-282">In hello portal, go toohello **App Services** blade, locate and select your web app, and then click **Delete**.</span></span> <span data-ttu-id="199aa-283">Om du bara vill tootemporarily förhindra andra från att komma åt hello webbprogram, klickar du på **stoppa** i stället.</span><span class="sxs-lookup"><span data-stu-id="199aa-283">If you just want tootemporarily prevent others from accessing hello web app, click **Stop** instead.</span></span> <span data-ttu-id="199aa-284">I så fall fortsätter avgifterna tooaccrue för hello SQL-databasen och Lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="199aa-284">In that case, charges will continue tooaccrue for hello SQL Database and Storage account.</span></span>

### <a name="delete-your-storage-account"></a><span data-ttu-id="199aa-285">Ta bort ditt lagringskonto</span><span class="sxs-lookup"><span data-stu-id="199aa-285">Delete your storage account</span></span>
<span data-ttu-id="199aa-286">toodelete ditt lagringskonto finns [ta bort ett lagringskonto](https://docs.microsoft.com/azure/storage/storage-create-storage-account#delete-a-storage-account).</span><span class="sxs-lookup"><span data-stu-id="199aa-286">toodelete your storage account, see [Delete a storage account](https://docs.microsoft.com/azure/storage/storage-create-storage-account#delete-a-storage-account).</span></span> 

### <a name="delete-your-database"></a><span data-ttu-id="199aa-287">Ta bort databasen</span><span class="sxs-lookup"><span data-stu-id="199aa-287">Delete your database</span></span>
<span data-ttu-id="199aa-288">toodelete din SQL-databas, se hello [Azure SQL Database REST API](https://docs.microsoft.com/rest/api/sql/) dokumentation.</span><span class="sxs-lookup"><span data-stu-id="199aa-288">toodelete your SQL database, see hello [Azure SQL Database REST API](https://docs.microsoft.com/rest/api/sql/) documentation.</span></span>

## <span data-ttu-id="199aa-289"><a id="create"></a>Skapa hello programmet från grunden</span><span class="sxs-lookup"><span data-stu-id="199aa-289"><a id="create"></a>Create hello application from scratch</span></span>
<span data-ttu-id="199aa-290">I det här avsnittet gör du hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="199aa-290">In this section you'll do hello following tasks:</span></span>

* <span data-ttu-id="199aa-291">Skapa en Visual Studio-lösning med ett webbprojekt.</span><span class="sxs-lookup"><span data-stu-id="199aa-291">Create a Visual Studio solution with a web project.</span></span>
* <span data-ttu-id="199aa-292">Lägg till en klassbiblioteksprojektet för hello data access-lagret som delas mellan hello klientdelen och serverdelen.</span><span class="sxs-lookup"><span data-stu-id="199aa-292">Add a class library project for hello data access layer that is shared between hello front end and back end.</span></span>
* <span data-ttu-id="199aa-293">Lägga till ett konsolprogram projekt för hello serverdel med WebJobs distribution aktiverad.</span><span class="sxs-lookup"><span data-stu-id="199aa-293">Add a Console Application project for hello backend, with WebJobs deployment enabled.</span></span>
* <span data-ttu-id="199aa-294">Lägg till NuGet-paket.</span><span class="sxs-lookup"><span data-stu-id="199aa-294">Add NuGet packages.</span></span>
* <span data-ttu-id="199aa-295">Ange projektreferenser.</span><span class="sxs-lookup"><span data-stu-id="199aa-295">Set project references.</span></span>
* <span data-ttu-id="199aa-296">Kopiera koden och konfigurationen programfiler från hello hämtat program som du arbetade med i hello föregående avsnitt i hello kursen.</span><span class="sxs-lookup"><span data-stu-id="199aa-296">Copy application code and configuration files from hello downloaded application that you worked with in hello previous section of hello tutorial.</span></span>
* <span data-ttu-id="199aa-297">Granska hello delar av hello kod som fungerar med Azure-blobbar och köer och hello WebJobs-SDK.</span><span class="sxs-lookup"><span data-stu-id="199aa-297">Review hello parts of hello code that work with Azure blobs and queues and hello WebJobs SDK.</span></span>

### <a name="create-a-visual-studio-solution-with-a-web-project-and-class-library-project"></a><span data-ttu-id="199aa-298">Skapa en Visual Studio-lösning med ett webbprojekt och klassbiblioteksprojektet</span><span class="sxs-lookup"><span data-stu-id="199aa-298">Create a Visual Studio solution with a web project and class library project</span></span>
1. <span data-ttu-id="199aa-299">I Visual Studio väljer **filen** > **ny** > **projekt**.</span><span class="sxs-lookup"><span data-stu-id="199aa-299">In Visual Studio, choose **File** > **New** > **Project**.</span></span>
2. <span data-ttu-id="199aa-300">I hello **nytt projekt** dialogrutan Välj **Visual C#** > **Web** > **ASP.NET-webbprogram (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="199aa-300">In hello **New Project** dialog, choose **Visual C#** > **Web** > **ASP.NET Web Application (.NET Framework)**.</span></span>
3. <span data-ttu-id="199aa-301">Namnge projektet hello ContosoAdsWeb, namnge hello lösning ContosoAdsWebJobsSDK (ändra hello lösningens namn om du ska lägga i hello samma mapp som hello hämtade lösningen), och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="199aa-301">Name hello project ContosoAdsWeb, name hello solution ContosoAdsWebJobsSDK (change hello solution name if you're putting it in hello same folder as hello downloaded solution), and then click **OK**.</span></span>

    ![Nytt projekt](./media/websites-dotnet-webjobs-sdk-get-started/newproject.png)
4. <span data-ttu-id="199aa-303">I hello **nytt ASP.NET-webbprogram** dialogrutan Välj hello MVC-mallen och välj **ändra autentisering**.</span><span class="sxs-lookup"><span data-stu-id="199aa-303">In hello **New ASP.NET Web Application** dialog, choose hello MVC template, and select **Change Authentication**.</span></span>

    ![Ändra autentisering](./media/websites-dotnet-webjobs-sdk-get-started/chgauth.png)
5. <span data-ttu-id="199aa-305">I hello **ändra autentisering** dialogrutan Välj **ingen autentisering**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="199aa-305">In hello **Change Authentication** dialog, choose **No Authentication**, and then click **OK**.</span></span>

    ![Ingen autentisering](./media/websites-dotnet-webjobs-sdk-get-started/noauth.png)
6. <span data-ttu-id="199aa-307">I hello **nytt ASP.NET-webbprogram** dialogrutan klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="199aa-307">In hello **New ASP.NET Web Application** dialog, click **OK**.</span></span>

    <span data-ttu-id="199aa-308">Visual Studio skapar hello lösningen och hello webbprojekt.</span><span class="sxs-lookup"><span data-stu-id="199aa-308">Visual Studio creates hello solution and hello web project.</span></span>
7. <span data-ttu-id="199aa-309">I **Solution Explorer**, högerklicka på hello lösningen (inte hello projekt) och välj **Lägg till** > **nytt projekt**.</span><span class="sxs-lookup"><span data-stu-id="199aa-309">In **Solution Explorer**, right-click hello solution (not hello project), and choose **Add** > **New Project**.</span></span>
8. <span data-ttu-id="199aa-310">I hello **Lägg till nytt projekt** dialogrutan Välj **Visual C#** > **klassiska Windows-skrivbordet** > **Class Library (.NET Framework)** mall.</span><span class="sxs-lookup"><span data-stu-id="199aa-310">In hello **Add New Project** dialog, choose **Visual C#** > **Windows Classic Desktop** > **Class Library (.NET Framework)** template.</span></span>  
9. <span data-ttu-id="199aa-311">Namnet hello projektet *ContosoAdsCommon*, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="199aa-311">Name hello project *ContosoAdsCommon*, and then click **OK**.</span></span>

    <span data-ttu-id="199aa-312">Det här projektet innehåller hello Entity Framework-kontexten och datamodellen hello som båda hello klientdelen och serverdelen använder.</span><span class="sxs-lookup"><span data-stu-id="199aa-312">This project will contain hello Entity Framework context and hello data model which both hello front end and back end will use.</span></span> <span data-ttu-id="199aa-313">Som ett alternativ kan du definiera hello EF-relaterade klasserna i hello webbprojektet och referera till det projektet från hello Webbjobb projekt.</span><span class="sxs-lookup"><span data-stu-id="199aa-313">As an alternative, you could define hello EF-related classes in hello web project and reference that project from hello WebJob project.</span></span> <span data-ttu-id="199aa-314">Men sedan projektet Webbjobb skulle ha en referens tooweb sammansättningar som det inte behöver.</span><span class="sxs-lookup"><span data-stu-id="199aa-314">But, then your WebJob project would have a reference tooweb assemblies, which it doesn't need.</span></span>

### <a name="add-a-console-application-project-that-has-webjobs-deployment-enabled"></a><span data-ttu-id="199aa-315">Lägga till ett konsolprogram projekt som har WebJobs distribution aktiverad</span><span class="sxs-lookup"><span data-stu-id="199aa-315">Add a Console Application project that has WebJobs deployment enabled</span></span>
1. <span data-ttu-id="199aa-316">Högerklicka på hello webbprojekt (inte hello lösning eller hello klassbiblioteksprojektet) och klicka sedan på **Lägg till** > **nytt projekt för Azure Webjobs**.</span><span class="sxs-lookup"><span data-stu-id="199aa-316">Right-click hello web project (not hello solution or hello class library project), and then click **Add** > **New Azure WebJob Project**.</span></span>

    ![Nya Menyvalet för Azure Webjobs-projekt](./media/websites-dotnet-webjobs-sdk-get-started/newawjp.png)
2. <span data-ttu-id="199aa-318">I hello **lägga till Azure Webjobs** dialogrutan Ange ContosoAdsWebJob som båda hello **projektnamn** och hello **webbjobbsnamnet**.</span><span class="sxs-lookup"><span data-stu-id="199aa-318">In hello **Add Azure WebJob** dialog, enter ContosoAdsWebJob as both hello **Project name** and hello **WebJob name**.</span></span> <span data-ttu-id="199aa-319">Lämna **Webbjobb kör läge** ställa in också**kör kontinuerligt**.</span><span class="sxs-lookup"><span data-stu-id="199aa-319">Leave **WebJob run mode** set too**Run Continuously**.</span></span>
3. <span data-ttu-id="199aa-320">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="199aa-320">Click **OK**.</span></span>

   <span data-ttu-id="199aa-321">Visual Studio skapar ett konsolprogram som är konfigurerade toodeploy som ett Webbjobb när du distribuerar hello webbprojekt.</span><span class="sxs-lookup"><span data-stu-id="199aa-321">Visual Studio creates a Console application that is configured toodeploy as a WebJob whenever you deploy hello web project.</span></span> <span data-ttu-id="199aa-322">toodo att det utförs hello följande uppgifter när du har skapat hello projektet:</span><span class="sxs-lookup"><span data-stu-id="199aa-322">toodo that, it performed hello following tasks after creating hello project:</span></span>

   * <span data-ttu-id="199aa-323">Lägga till en *webbjobb publicera settings.json* filen i hello Webbjobb projektmappen egenskaper.</span><span class="sxs-lookup"><span data-stu-id="199aa-323">Added a *webjob-publish-settings.json* file in hello WebJob project Properties folder.</span></span>
   * <span data-ttu-id="199aa-324">Lägga till en *webjobs list.json* filen i hello webbprogram projektmapp egenskaper.</span><span class="sxs-lookup"><span data-stu-id="199aa-324">Added a *webjobs-list.json* file in hello web project Properties folder.</span></span>
   * <span data-ttu-id="199aa-325">Installerat hello Microsoft.Web.WebJobs.Publish NuGet-paketet i hello Webbjobb projekt.</span><span class="sxs-lookup"><span data-stu-id="199aa-325">Installed hello Microsoft.Web.WebJobs.Publish NuGet package in hello WebJob project.</span></span>

   <span data-ttu-id="199aa-326">Mer information om dessa ändringar finns [hur toodeploy WebJobs med hjälp av Visual Studio](websites-dotnet-deploy-webjobs.md).</span><span class="sxs-lookup"><span data-stu-id="199aa-326">For more information about these changes, see [How toodeploy WebJobs by using Visual Studio](websites-dotnet-deploy-webjobs.md).</span></span>

### <a name="add-nuget-packages"></a><span data-ttu-id="199aa-327">Lägg till NuGet-paket</span><span class="sxs-lookup"><span data-stu-id="199aa-327">Add NuGet packages</span></span>
<span data-ttu-id="199aa-328">hello nytt projekt mall för ett Webbjobb projekt installerar automatiskt hello WebJobs SDK NuGet-paketet [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs) och dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="199aa-328">hello new-project template for a WebJob project automatically installs hello WebJobs SDK NuGet package [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs) and its dependencies.</span></span>

<span data-ttu-id="199aa-329">En av hello WebJobs SDK beroenden som installeras automatiskt i hello Webbjobb projektet är hello Azure Storage Client bibliotek (SCL).</span><span class="sxs-lookup"><span data-stu-id="199aa-329">One of hello WebJobs SDK dependencies that is installed automatically in hello WebJob project is hello Azure Storage Client Library (SCL).</span></span> <span data-ttu-id="199aa-330">Du måste dock tooadd den toohello web project toowork med blobbar och köer.</span><span class="sxs-lookup"><span data-stu-id="199aa-330">However, you need tooadd it toohello web project toowork with blobs and queues.</span></span>

1. <span data-ttu-id="199aa-331">Öppna hello **hantera NuGet-paket** dialogrutan för hello lösningen.</span><span class="sxs-lookup"><span data-stu-id="199aa-331">Open hello **Manage NuGet Packages** dialog for hello solution.</span></span>
2. <span data-ttu-id="199aa-332">I hello vänster och välj **installerade paket**.</span><span class="sxs-lookup"><span data-stu-id="199aa-332">In hello left pane, select **Installed packages**.</span></span>
3. <span data-ttu-id="199aa-333">Hitta hello *Azure Storage* paketet och klickar sedan på **hantera**.</span><span class="sxs-lookup"><span data-stu-id="199aa-333">Find hello *Azure Storage* package, and then click **Manage**.</span></span>
4. <span data-ttu-id="199aa-334">I hello **Välj projekt** rutan, Välj hello **ContosoAdsWeb** kryssrutan och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="199aa-334">In hello **Select Projects** box, select hello **ContosoAdsWeb** check box, and then click **OK**.</span></span>

    <span data-ttu-id="199aa-335">Alla tre projekt använda hello Entity Framework toowork med data i SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="199aa-335">All three projects use hello Entity Framework toowork with data in SQL Database.</span></span>
5. <span data-ttu-id="199aa-336">I hello vänster och välj **Online**.</span><span class="sxs-lookup"><span data-stu-id="199aa-336">In hello left pane, select **Online**.</span></span>
6. <span data-ttu-id="199aa-337">Hitta hello *EntityFramework* NuGet-paketet och installera det i alla tre projekt.</span><span class="sxs-lookup"><span data-stu-id="199aa-337">Find hello *EntityFramework* NuGet package, and install it in all three projects.</span></span>

### <a name="set-project-references"></a><span data-ttu-id="199aa-338">Ange projektreferenser</span><span class="sxs-lookup"><span data-stu-id="199aa-338">Set project references</span></span>
<span data-ttu-id="199aa-339">Webb- och Webbjobb projekt arbeta med hello SQL-databas, så att båda måste en referens toohello ContosoAdsCommon-projektet.</span><span class="sxs-lookup"><span data-stu-id="199aa-339">Both web and WebJob projects work with hello SQL database, so both need a reference toohello ContosoAdsCommon project.</span></span>

1. <span data-ttu-id="199aa-340">Ange en referens toohello ContosoAdsCommon-projektet i ContosoAdsWeb-projektet hello.</span><span class="sxs-lookup"><span data-stu-id="199aa-340">In hello ContosoAdsWeb project, set a reference toohello ContosoAdsCommon project.</span></span> <span data-ttu-id="199aa-341">(Högerklicka på hello ContosoAdsWeb-projektet och klicka sedan på **Lägg till** > **referens**.</span><span class="sxs-lookup"><span data-stu-id="199aa-341">(Right-click hello ContosoAdsWeb project, and then click **Add** > **Reference**.</span></span> 
2. <span data-ttu-id="199aa-342">I hello **Reference Manager** dialogrutan **projekt** > **lösning** > **ContosoAdsCommon**, och klicka sedan på **OK**.)</span><span class="sxs-lookup"><span data-stu-id="199aa-342">In hello **Reference Manager** dialog box, select **Projects** > **Solution** > **ContosoAdsCommon**, and then click **OK**.)</span></span>
   
    <span data-ttu-id="199aa-343">Hej Webbjobb projekt behöver referenser för att arbeta med bilder och för att komma åt anslutningssträngar.</span><span class="sxs-lookup"><span data-stu-id="199aa-343">hello WebJob project needs references for working with images and for accessing connection strings.</span></span>

4. <span data-ttu-id="199aa-344">I hello ContosoAdsWebJob projekt kan du ange en referens för`System.Drawing` och `System.Configuration`.</span><span class="sxs-lookup"><span data-stu-id="199aa-344">In hello ContosoAdsWebJob project, set a reference too`System.Drawing` and `System.Configuration`.</span></span>

### <a name="add-code-and-configuration-files"></a><span data-ttu-id="199aa-345">Lägg till kod och konfigurationsfiler</span><span class="sxs-lookup"><span data-stu-id="199aa-345">Add code and configuration files</span></span>
<span data-ttu-id="199aa-346">Den här kursen visar inte hur för[skapar MVC-kontrollanter och vyer med scaffold-teknik](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started), hur för[skriver Entity Framework-kod som fungerar med SQL Server-databaser](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc), eller [hello grunderna i asynkron programmering i ASP.NET 4.5](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices#async).</span><span class="sxs-lookup"><span data-stu-id="199aa-346">This tutorial does not show how too[create MVC controllers and views using scaffolding](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started), how too[write Entity Framework code that works with SQL Server databases](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc), or [hello basics of asynchronous programming in ASP.NET 4.5](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices#async).</span></span> <span data-ttu-id="199aa-347">Så, allt som är kvar toodo kopiera koden och konfigurationen filer från hello hämtade lösningen till hello ny lösning.</span><span class="sxs-lookup"><span data-stu-id="199aa-347">So, all that remains toodo is copy code and configuration files from hello downloaded solution into hello new solution.</span></span> <span data-ttu-id="199aa-348">När du gör det hello följande avsnitt visas och förklaras viktiga delar av hello kod.</span><span class="sxs-lookup"><span data-stu-id="199aa-348">After you do that, hello following sections show and explain key parts of hello code.</span></span>

<span data-ttu-id="199aa-349">tooadd filer tooa projekt eller en mapp, högerklickar du på hello projektet eller mappen och klicka på **Lägg till** > **befintlig artikel**.</span><span class="sxs-lookup"><span data-stu-id="199aa-349">tooadd files tooa project or a folder, right-click hello project or folder and click **Add** > **Existing Item**.</span></span> <span data-ttu-id="199aa-350">Välj hello filer du vill ha och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="199aa-350">Select hello files you want and click **Add**.</span></span> <span data-ttu-id="199aa-351">Om du blir tillfrågad om du vill tooreplace befintliga filer, klickar du på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="199aa-351">If asked whether you want tooreplace existing files, click **Yes**.</span></span>

1. <span data-ttu-id="199aa-352">Ta bort hello i hello ContosoAdsCommon-projektet *Class1.cs* och Lägg till i dess ställe hello följande filer från hello hämtade projektet.</span><span class="sxs-lookup"><span data-stu-id="199aa-352">In hello ContosoAdsCommon project, delete hello *Class1.cs* file and add in its place hello following files from hello downloaded project.</span></span>

   * <span data-ttu-id="199aa-353">*AD.CS*</span><span class="sxs-lookup"><span data-stu-id="199aa-353">*Ad.cs*</span></span>
   * <span data-ttu-id="199aa-354">*ContosoAdscontext.cs*</span><span class="sxs-lookup"><span data-stu-id="199aa-354">*ContosoAdscontext.cs*</span></span>
   * <span data-ttu-id="199aa-355">*BlobInformation.cs*</span><span class="sxs-lookup"><span data-stu-id="199aa-355">*BlobInformation.cs*</span></span>
2. <span data-ttu-id="199aa-356">Lägg till hello följande filer från hello hämtade projektet i ContosoAdsWeb-projektet hello.</span><span class="sxs-lookup"><span data-stu-id="199aa-356">In hello ContosoAdsWeb project, add hello following files from hello downloaded project.</span></span>

   * <span data-ttu-id="199aa-357">*Web.config*</span><span class="sxs-lookup"><span data-stu-id="199aa-357">*Web.config*</span></span>
   * <span data-ttu-id="199aa-358">*Global.asax.CS*</span><span class="sxs-lookup"><span data-stu-id="199aa-358">*Global.asax.cs*</span></span>  
   * <span data-ttu-id="199aa-359">I hello *domänkontrollanter* mapp: *AdController.cs*</span><span class="sxs-lookup"><span data-stu-id="199aa-359">In hello *Controllers* folder: *AdController.cs*</span></span>
   * <span data-ttu-id="199aa-360">I hello *Views\Shared* mapp: *_Layout.cshtml* fil</span><span class="sxs-lookup"><span data-stu-id="199aa-360">In hello *Views\Shared* folder: *_Layout.cshtml* file</span></span>
   * <span data-ttu-id="199aa-361">I hello *Views\Home* mapp: *Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="199aa-361">In hello *Views\Home* folder: *Index.cshtml*</span></span>
   * <span data-ttu-id="199aa-362">I hello *Views\Ad* mapp (skapa hello mappen först): fem *.cshtml* filer</span><span class="sxs-lookup"><span data-stu-id="199aa-362">In hello *Views\Ad* folder (create hello folder first): five *.cshtml* files</span></span>
3. <span data-ttu-id="199aa-363">Lägg till hello följande filer från hello hämtade projektet i hello ContosoAdsWebJob-projektet.</span><span class="sxs-lookup"><span data-stu-id="199aa-363">In hello ContosoAdsWebJob project, add hello following files from hello downloaded project.</span></span>

   * <span data-ttu-id="199aa-364">*App.config* (ändra hello filfilter typ för**alla filer**)</span><span class="sxs-lookup"><span data-stu-id="199aa-364">*App.config* (change hello file type filter too**All Files**)</span></span>
   * <span data-ttu-id="199aa-365">*Program.CS*</span><span class="sxs-lookup"><span data-stu-id="199aa-365">*Program.cs*</span></span>
   * <span data-ttu-id="199aa-366">*Functions.CS*</span><span class="sxs-lookup"><span data-stu-id="199aa-366">*Functions.cs*</span></span>

<span data-ttu-id="199aa-367">Du kan nu skapa, köra och distribuera hello program enligt anvisningarna tidigare i kursen hello.</span><span class="sxs-lookup"><span data-stu-id="199aa-367">You can now build, run, and deploy hello application as instructed earlier in hello tutorial.</span></span> <span data-ttu-id="199aa-368">Innan du gör det kan dock stoppa hello Webbjobb som körs fortfarande i hello första webbapp du har distribuerat till.</span><span class="sxs-lookup"><span data-stu-id="199aa-368">Before you do that, however, stop hello WebJob that is still running in hello first web app you deployed to.</span></span> <span data-ttu-id="199aa-369">I annat fall att Webbjobb ska bearbeta meddelanden i kö skapas lokalt eller av hello app körs i en ny webbapp, eftersom alla använder hello samma lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="199aa-369">Otherwise, that WebJob will process queue messages created locally or by hello app running in a new web app, since all are using hello same storage account.</span></span>

## <span data-ttu-id="199aa-370"><a id="code"></a>Granska hello programkod</span><span class="sxs-lookup"><span data-stu-id="199aa-370"><a id="code"></a>Review hello application code</span></span>
<span data-ttu-id="199aa-371">hello följande avsnitt beskrivs hello kod relaterade tooworking med hello WebJobs-SDK och Azure Storage-blobbar och köer.</span><span class="sxs-lookup"><span data-stu-id="199aa-371">hello following sections explain hello code related tooworking with hello WebJobs SDK and Azure Storage blobs and queues.</span></span>

> [!NOTE]
> <span data-ttu-id="199aa-372">Hello kod av specifika toohello WebJobs SDK, gå i toohello [Program.cs och Functions.cs](#programcs) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="199aa-372">For hello code specific toohello WebJobs SDK, go toohello [Program.cs and Functions.cs](#programcs) sections.</span></span>
>
>

### <a name="contosoadscommon---adcs"></a><span data-ttu-id="199aa-373">ContosoAdsCommon – Ad.cs</span><span class="sxs-lookup"><span data-stu-id="199aa-373">ContosoAdsCommon - Ad.cs</span></span>
<span data-ttu-id="199aa-374">filen för hello Ad.cs definierar en uppräkning för annonskategorier och en POCO-entitetsklass för annonsinformation.</span><span class="sxs-lookup"><span data-stu-id="199aa-374">hello Ad.cs file defines an enum for ad categories and a POCO entity class for ad information.</span></span>

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

### <a name="contosoadscommon---contosoadscontextcs"></a><span data-ttu-id="199aa-375">ContosoAdsCommon – ContosoAdsContext.cs</span><span class="sxs-lookup"><span data-stu-id="199aa-375">ContosoAdsCommon - ContosoAdsContext.cs</span></span>
<span data-ttu-id="199aa-376">Hej ContosoAdsContext-klassen anger att hello annonsklassen används i en DbSet-samling som Entity Framework lagrar i en SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="199aa-376">hello ContosoAdsContext class specifies that hello Ad class is used in a DbSet collection, which Entity Framework stores in a SQL database.</span></span>

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

<span data-ttu-id="199aa-377">hello-klassen har två konstruktorer.</span><span class="sxs-lookup"><span data-stu-id="199aa-377">hello class has two constructors.</span></span> <span data-ttu-id="199aa-378">hello först används av hello webbprojektet och anger hello namnet på en anslutningssträng som lagras i Web.config-filen för hello eller hello Azure körningsmiljön.</span><span class="sxs-lookup"><span data-stu-id="199aa-378">hello first is used by hello web project, and specifies hello name of a connection string that is stored in hello Web.config file or hello Azure runtime environment.</span></span> <span data-ttu-id="199aa-379">hello andra konstruktorn gör toopass i hello faktiska anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="199aa-379">hello second constructor enables you toopass in hello actual connection string.</span></span> <span data-ttu-id="199aa-380">Det krävs av hello Webbjobb projektet eftersom det inte har en Web.config-fil.</span><span class="sxs-lookup"><span data-stu-id="199aa-380">That is needed by hello WebJob project since it doesn't have a Web.config file.</span></span> <span data-ttu-id="199aa-381">Du såg tidigare där den här anslutningssträngen lagras, och du ser hur hello koden hämtar anslutningssträngen hello när den instantierar DbContext-klassen hello.</span><span class="sxs-lookup"><span data-stu-id="199aa-381">You saw earlier where this connection string was stored, and you'll see later how hello code retrieves hello connection string when it instantiates hello DbContext class.</span></span>

### <a name="contosoadscommon---blobinformationcs"></a><span data-ttu-id="199aa-382">ContosoAdsCommon – BlobInformation.cs</span><span class="sxs-lookup"><span data-stu-id="199aa-382">ContosoAdsCommon - BlobInformation.cs</span></span>
<span data-ttu-id="199aa-383">Hej `BlobInformation` klassen är används toostore information om en avbildning blob i ett meddelande i kön.</span><span class="sxs-lookup"><span data-stu-id="199aa-383">hello `BlobInformation` class is used toostore information about an image blob in a queue message.</span></span>

        public class BlobInformation
        {
            public Uri BlobUri { get; set; }

            public string BlobName
            {
                get
                {
                    return BlobUri.Segments[BlobUri.Segments.Length - 1];
                }
            }
            public string BlobNameWithoutExtension
            {
                get
                {
                    return Path.GetFileNameWithoutExtension(BlobName);
                }
            }
            public int AdId { get; set; }
        }


### <a name="contosoadsweb---globalasaxcs"></a><span data-ttu-id="199aa-384">ContosoAdsWeb – Global.asax.cs</span><span class="sxs-lookup"><span data-stu-id="199aa-384">ContosoAdsWeb - Global.asax.cs</span></span>
<span data-ttu-id="199aa-385">Kod som anropas från hello `Application_Start` metoden skapar en *bilder* blob-behållaren och en *bilder* kö om de inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="199aa-385">Code that is called from hello `Application_Start` method creates an *images* blob container and an *images* queue if they don't already exist.</span></span> <span data-ttu-id="199aa-386">Detta säkerställer att varje gång du startar med ett nytt lagringskonto hello nödvändiga blobbehållaren och kön skapas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="199aa-386">This ensures that whenever you start using a new storage account, hello required blob container and queue are created automatically.</span></span>

<span data-ttu-id="199aa-387">Hej kod hämtar åtkomst toohello storage-konto med hjälp av hello lagringsanslutningssträngen från hello *Web.config* fil- eller Azure körningsmiljön.</span><span class="sxs-lookup"><span data-stu-id="199aa-387">hello code gets access toohello storage account by using hello storage connection string from hello *Web.config* file or Azure runtime environment.</span></span>

        var storageAccount = CloudStorageAccount.Parse
            (ConfigurationManager.ConnectionStrings["AzureWebJobsStorage"].ToString());

<span data-ttu-id="199aa-388">Sedan hämtar den en referens toohello *bilder* , skapar hello behållaren om den inte redan finns och anger åtkomstbehörighet för hello ny behållare.</span><span class="sxs-lookup"><span data-stu-id="199aa-388">Then, it gets a reference toohello *images* blob container, creates hello container if it doesn't already exist, and sets access permissions on hello new container.</span></span> <span data-ttu-id="199aa-389">Som standard tillåter nya behållare enbart klienter med storage-konto autentiseringsuppgifter tooaccess blobbar.</span><span class="sxs-lookup"><span data-stu-id="199aa-389">By default, new containers allow only clients with storage account credentials tooaccess blobs.</span></span> <span data-ttu-id="199aa-390">hello webbprogrammet måste hello blobbar toobe offentliga så att den kan visa bilder med URL: er som punkt toohello bildblobbarna.</span><span class="sxs-lookup"><span data-stu-id="199aa-390">hello web app needs hello blobs toobe public so that it can display images using URLs that point toohello image blobs.</span></span>

        var blobClient = storageAccount.CreateCloudBlobClient();
        var imagesBlobContainer = blobClient.GetContainerReference("images");
        if (imagesBlobContainer.CreateIfNotExists())
        {
            imagesBlobContainer.SetPermissions(
                new BlobContainerPermissions
                {
                    PublicAccess = BlobContainerPublicAccessType.Blob
                });
        }

<span data-ttu-id="199aa-391">Liknande kod hämtar en referens toohello *thumbnailrequest* kön och skapar en ny kö.</span><span class="sxs-lookup"><span data-stu-id="199aa-391">Similar code gets a reference toohello *thumbnailrequest* queue and creates a new queue.</span></span> <span data-ttu-id="199aa-392">I så fall krävs ingen ändring av behörigheter.</span><span class="sxs-lookup"><span data-stu-id="199aa-392">In this case no permissions change is needed.</span></span>

        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
        var imagesQueue = queueClient.GetQueueReference("thumbnailrequest");
        imagesQueue.CreateIfNotExists();

### <a name="contosoadsweb---layoutcshtml"></a><span data-ttu-id="199aa-393">ContosoAdsWeb – _Layout.cshtml</span><span class="sxs-lookup"><span data-stu-id="199aa-393">ContosoAdsWeb - _Layout.cshtml</span></span>
<span data-ttu-id="199aa-394">Hej *_Layout.cshtml* filen anger hello programnamn i hello sidhuvud och sidfot och skapar en menypost ”Ads”.</span><span class="sxs-lookup"><span data-stu-id="199aa-394">hello *_Layout.cshtml* file sets hello app name in hello header and footer, and creates an "Ads" menu entry.</span></span>

### <a name="contosoadsweb---viewshomeindexcshtml"></a><span data-ttu-id="199aa-395">ContosoAdsWeb – Views\Home\Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="199aa-395">ContosoAdsWeb - Views\Home\Index.cshtml</span></span>
<span data-ttu-id="199aa-396">Hej *Views\Home\Index.cshtml* visar kategorilänkar på startsidan för hello.</span><span class="sxs-lookup"><span data-stu-id="199aa-396">hello *Views\Home\Index.cshtml* file displays category links on hello home page.</span></span> <span data-ttu-id="199aa-397">hello länkarna överför hello heltalsvärde hello `Category` uppräkning i en querystring-variabel toohello Ads-indexsidan.</span><span class="sxs-lookup"><span data-stu-id="199aa-397">hello links pass hello integer value of hello `Category` enum in a querystring variable toohello Ads Index page.</span></span>

        <li>@Html.ActionLink("Cars", "Index", "Ad", new { category = (int)Category.Cars }, null)</li>
        <li>@Html.ActionLink("Real estate", "Index", "Ad", new { category = (int)Category.RealEstate }, null)</li>
        <li>@Html.ActionLink("Free stuff", "Index", "Ad", new { category = (int)Category.FreeStuff }, null)</li>
        <li>@Html.ActionLink("All", "Index", "Ad", null, null)</li>

### <a name="contosoadsweb---adcontrollercs"></a><span data-ttu-id="199aa-398">ContosoAdsWeb – AdController.cs</span><span class="sxs-lookup"><span data-stu-id="199aa-398">ContosoAdsWeb - AdController.cs</span></span>
<span data-ttu-id="199aa-399">I hello *AdController.cs* fil, hello konstruktorn anrop hello `InitializeStorage` metoden toocreate Azure Storage-klientbibliotek objekt som tillhandahåller en API för att arbeta med blobbar och köer.</span><span class="sxs-lookup"><span data-stu-id="199aa-399">In hello *AdController.cs* file, hello constructor calls hello `InitializeStorage` method toocreate Azure Storage Client Library objects that provide an API for working with blobs and queues.</span></span>

<span data-ttu-id="199aa-400">Sedan hello kod hämtar en referens toohello *bilder* blob-behållare som du såg tidigare i *Global.asax.cs*.</span><span class="sxs-lookup"><span data-stu-id="199aa-400">Then, hello code gets a reference toohello *images* blob container as you saw earlier in *Global.asax.cs*.</span></span> <span data-ttu-id="199aa-401">Under tiden som anger den standard [standardpolicy](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling) lämplig för ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="199aa-401">While doing that, it sets a default [retry policy](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling) appropriate for a web app.</span></span> <span data-ttu-id="199aa-402">hello standardprincipen exponentiell backoff försök kan hänga hello webbappen längre än en minut vid upprepade återförsök för ett tillfälligt fel.</span><span class="sxs-lookup"><span data-stu-id="199aa-402">hello default exponential backoff retry policy could hang hello web app for longer than a minute on repeated retries for a transient fault.</span></span> <span data-ttu-id="199aa-403">Hej återförsökspolicyn som anges här väntar tre sekunder efter varje försök i upp toothree försök.</span><span class="sxs-lookup"><span data-stu-id="199aa-403">hello retry policy specified here waits three seconds after each try for up toothree tries.</span></span>

        var blobClient = storageAccount.CreateCloudBlobClient();
        blobClient.DefaultRequestOptions.RetryPolicy = new LinearRetry(TimeSpan.FromSeconds(3), 3);
        imagesBlobContainer = blobClient.GetContainerReference("images");

<span data-ttu-id="199aa-404">Liknande kod hämtar en referens toohello *bilder* kön.</span><span class="sxs-lookup"><span data-stu-id="199aa-404">Similar code gets a reference toohello *images* queue.</span></span>

        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
        queueClient.DefaultRequestOptions.RetryPolicy = new LinearRetry(TimeSpan.FromSeconds(3), 3);
        imagesQueue = queueClient.GetQueueReference("blobnamerequest");

<span data-ttu-id="199aa-405">De flesta hello kontrollantkoden är typisk när du arbetar med en Entity Framework-datamodell med en DbContext-klassen.</span><span class="sxs-lookup"><span data-stu-id="199aa-405">Most of hello controller code is typical for working with an Entity Framework data model using a DbContext class.</span></span> <span data-ttu-id="199aa-406">Ett undantag är hello HttpPost `Create` metod som överför en fil och sparar den i blob storage.</span><span class="sxs-lookup"><span data-stu-id="199aa-406">An exception is hello HttpPost `Create` method, which uploads a file and saves it in blob storage.</span></span> <span data-ttu-id="199aa-407">Hej modellbindaren tillhandahåller ett [HttpPostedFileBase](http://msdn.microsoft.com/library/system.web.httppostedfilebase.aspx) objekt toohello metod.</span><span class="sxs-lookup"><span data-stu-id="199aa-407">hello model binder provides an [HttpPostedFileBase](http://msdn.microsoft.com/library/system.web.httppostedfilebase.aspx) object toohello method.</span></span>

        [HttpPost]
        [ValidateAntiForgeryToken]
        public async Task<ActionResult> Create(
            [Bind(Include = "Title,Price,Description,Category,Phone")] Ad ad,
            HttpPostedFileBase imageFile)

<span data-ttu-id="199aa-408">Om hello användaren har valt en fil tooupload hello kod överför hello-fil, sparar den i en blob och uppdaterar hello Ad-databasposten med en URL som pekar toohello blob.</span><span class="sxs-lookup"><span data-stu-id="199aa-408">If hello user selected a file tooupload, hello code uploads hello file, saves it in a blob, and updates hello Ad database record with a URL that points toohello blob.</span></span>

        if (imageFile != null && imageFile.ContentLength != 0)
        {
            blob = await UploadAndSaveBlobAsync(imageFile);
            ad.ImageURL = blob.Uri.ToString();
        }

<span data-ttu-id="199aa-409">hello kod som hello uppladdningen är i hello `UploadAndSaveBlobAsync` metod.</span><span class="sxs-lookup"><span data-stu-id="199aa-409">hello code that does hello upload is in hello `UploadAndSaveBlobAsync` method.</span></span> <span data-ttu-id="199aa-410">Den skapar ett Guidenamn för hello blob, överföringar och sparar hello fil och returnerar en referens toohello sparade blob.</span><span class="sxs-lookup"><span data-stu-id="199aa-410">It creates a GUID name for hello blob, uploads and saves hello file, and returns a reference toohello saved blob.</span></span>

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

<span data-ttu-id="199aa-411">Efter hello HttpPost `Create` metoden laddar upp en blob och uppdateringar Hej databas, skapar en kö meddelandet tooinform hello serverdelsprocessen att en bild är redo för konvertering tooa miniatyr.</span><span class="sxs-lookup"><span data-stu-id="199aa-411">After hello HttpPost `Create` method uploads a blob and updates hello database, it creates a queue message tooinform hello back-end process that an image is ready for conversion tooa thumbnail.</span></span>

        BlobInformation blobInfo = new BlobInformation() { AdId = ad.AdId, BlobUri = new Uri(ad.ImageURL) };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        await thumbnailRequestQueue.AddMessageAsync(queueMessage);

<span data-ttu-id="199aa-412">Hej koden för hello HttpPost `Edit` metoden är liknande, förutom att om hello användaren markerar en ny bildfil, alla blobbar som redan finns för den här ad måste tas bort.</span><span class="sxs-lookup"><span data-stu-id="199aa-412">hello code for hello HttpPost `Edit` method is similar, except that if hello user selects a new image file, any blobs that already exist for this ad must be deleted.</span></span>

        if (imageFile != null && imageFile.ContentLength != 0)
        {
            await DeleteAdBlobsAsync(ad);
            imageBlob = await UploadAndSaveBlobAsync(imageFile);
            ad.ImageURL = imageBlob.Uri.ToString();
        }

<span data-ttu-id="199aa-413">Här är hello-kod som tar bort blobbar när du tar bort en annons:</span><span class="sxs-lookup"><span data-stu-id="199aa-413">Here is hello code that deletes blobs when you delete an ad:</span></span>

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

### <a name="contosoadsweb---viewsadindexcshtml-and-detailscshtml"></a><span data-ttu-id="199aa-414">ContosoAdsWeb – Views\Ad\Index.cshtml och Details.cshtml</span><span class="sxs-lookup"><span data-stu-id="199aa-414">ContosoAdsWeb - Views\Ad\Index.cshtml and Details.cshtml</span></span>
<span data-ttu-id="199aa-415">Hej *Index.cshtml* visar miniatyrbilder med hello andra ad-data:</span><span class="sxs-lookup"><span data-stu-id="199aa-415">hello *Index.cshtml* file displays thumbnails with hello other ad data:</span></span>

        <img  src="@Html.Raw(item.ThumbnailURL)" />

<span data-ttu-id="199aa-416">Hej *Details.cshtml* visar hello bilden:</span><span class="sxs-lookup"><span data-stu-id="199aa-416">hello *Details.cshtml* file displays hello full-size image:</span></span>

        <img src="@Html.Raw(Model.ImageURL)" />

### <a name="contosoadsweb---viewsadcreatecshtml-and-editcshtml"></a><span data-ttu-id="199aa-417">ContosoAdsWeb – Views\Ad\Create.cshtml och Edit.cshtml</span><span class="sxs-lookup"><span data-stu-id="199aa-417">ContosoAdsWeb - Views\Ad\Create.cshtml and Edit.cshtml</span></span>
<span data-ttu-id="199aa-418">Hej *Create.cshtml* och *Edit.cshtml* filer ange kodningen som aktiverar hello controller tooget hello `HttpPostedFileBase` objekt.</span><span class="sxs-lookup"><span data-stu-id="199aa-418">hello *Create.cshtml* and *Edit.cshtml* files specify form encoding that enables hello controller tooget hello `HttpPostedFileBase` object.</span></span>

        @using (Html.BeginForm("Create", "Ad", FormMethod.Post, new { enctype = "multipart/form-data" }))

<span data-ttu-id="199aa-419">En `<input>` element instruerar hello webbläsare tooprovide en dialogruta för filval.</span><span class="sxs-lookup"><span data-stu-id="199aa-419">An `<input>` element tells hello browser tooprovide a file selection dialog.</span></span>

        <input type="file" name="imageFile" accept="image/*" class="form-control fileupload" />

### <span data-ttu-id="199aa-420"><a id="programcs"></a>ContosoAdsWebJob - Program.cs</span><span class="sxs-lookup"><span data-stu-id="199aa-420"><a id="programcs"></a>ContosoAdsWebJob - Program.cs</span></span>
<span data-ttu-id="199aa-421">När hello Webbjobb startar hello `Main` metodanrop hello WebJobs SDK `JobHost.RunAndBlock` metoden toobegin körning av utlösta funktioner i hello aktuella tråden.</span><span class="sxs-lookup"><span data-stu-id="199aa-421">When hello WebJob starts, hello `Main` method calls hello WebJobs SDK `JobHost.RunAndBlock` method toobegin execution of triggered functions on hello current thread.</span></span>

        static void Main(string[] args)
        {
            JobHost host = new JobHost();
            host.RunAndBlock();
        }

### <span data-ttu-id="199aa-422"><a id="generatethumbnail"></a>ContosoAdsWebJob - Functions.cs - GenerateThumbnail metod</span><span class="sxs-lookup"><span data-stu-id="199aa-422"><a id="generatethumbnail"></a>ContosoAdsWebJob - Functions.cs - GenerateThumbnail method</span></span>
<span data-ttu-id="199aa-423">Hej WebJobs SDK anropar den här metoden när ett kömeddelande tas emot.</span><span class="sxs-lookup"><span data-stu-id="199aa-423">hello WebJobs SDK calls this method when a queue message is received.</span></span> <span data-ttu-id="199aa-424">hello-metoden skapar en miniatyrbild och placeringar hello miniatyr URL i hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="199aa-424">hello method creates a thumbnail and puts hello thumbnail URL in hello database.</span></span>

        public static void GenerateThumbnail(
        [QueueTrigger("thumbnailrequest")] BlobInformation blobInfo,
        [Blob("images/{BlobName}", FileAccess.Read)] Stream input,
        [Blob("images/{BlobNameWithoutExtension}_thumbnail.jpg")] CloudBlockBlob outputBlob)
        {
            using (Stream output = outputBlob.OpenWrite())
            {
                ConvertImageToThumbnailJPG(input, output);
                outputBlob.Properties.ContentType = "image/jpeg";
            }

            // Entity Framework context class is not thread-safe, so it must
            // be instantiated and disposed within hello function.
            using (ContosoAdsContext db = new ContosoAdsContext())
            {
                var id = blobInfo.AdId;
                Ad ad = db.Ads.Find(id);
                if (ad == null)
                {
                    throw new Exception(String.Format("AdId {0} not found, can't create thumbnail", id.ToString()));
                }
                ad.ThumbnailURL = outputBlob.Uri.ToString();
                db.SaveChanges();
            }
        }

* <span data-ttu-id="199aa-425">Hej `QueueTrigger` attributet leder hello WebJobs SDK toocall den här metoden när ett nytt meddelande tas emot på hello thumbnailrequest kön.</span><span class="sxs-lookup"><span data-stu-id="199aa-425">hello `QueueTrigger` attribute directs hello WebJobs SDK toocall this method when a new message is received on hello thumbnailrequest queue.</span></span>

        [QueueTrigger("thumbnailrequest")] BlobInformation blobInfo,

    <span data-ttu-id="199aa-426">Hej `BlobInformation` objekt i kö hello-meddelande är automatiskt avserialiserat i hello `blobInfo` parameter.</span><span class="sxs-lookup"><span data-stu-id="199aa-426">hello `BlobInformation` object in hello queue message is automatically deserialized into hello `blobInfo` parameter.</span></span> <span data-ttu-id="199aa-427">Hej kömeddelande tas bort när hello-metoden har slutförts.</span><span class="sxs-lookup"><span data-stu-id="199aa-427">When hello method completes, hello queue message is deleted.</span></span> <span data-ttu-id="199aa-428">Om hello metoden misslyckas innan du slutför hälsningsmeddelande kön inte att ta bort; När ett 10 minuters lån upphör att gälla är hello-meddelande utgivna toobe tas upp igen och bearbetas.</span><span class="sxs-lookup"><span data-stu-id="199aa-428">If hello method fails before completing, hello queue message is not deleted; after a 10-minute lease expires, hello message is released toobe picked up again and processed.</span></span> <span data-ttu-id="199aa-429">Den här sekvensen upprepas inte under obestämd tid om ett meddelande som leder alltid ett undantag.</span><span class="sxs-lookup"><span data-stu-id="199aa-429">This sequence won't be repeated indefinitely if a message always causes an exception.</span></span> <span data-ttu-id="199aa-430">Efter 5 misslyckade försök tooprocess ett meddelande, hello-meddelande har flyttats tooa kö med namnet {könamn}-skadligt.</span><span class="sxs-lookup"><span data-stu-id="199aa-430">After 5 unsuccessful attempts tooprocess a message, hello message is moved tooa queue named {queuename}-poison.</span></span> <span data-ttu-id="199aa-431">hello högsta antal försök kan konfigureras.</span><span class="sxs-lookup"><span data-stu-id="199aa-431">hello maximum number of attempts is configurable.</span></span>
* <span data-ttu-id="199aa-432">Hej två `Blob` attribut ger objekt som är bundna tooblobs: en toohello befintliga avbildningsbloben och en tooa nya miniatyr blob som hello metoden skapas.</span><span class="sxs-lookup"><span data-stu-id="199aa-432">hello two `Blob` attributes provide objects that are bound tooblobs: one toohello existing image blob and one tooa new thumbnail blob that hello method creates.</span></span>

        [Blob("images/{BlobName}", FileAccess.Read)] Stream input,
        [Blob("images/{BlobNameWithoutExtension}_thumbnail.jpg")] CloudBlockBlob outputBlob)

    <span data-ttu-id="199aa-433">Blobbnamnen kommer från egenskaperna för hello `BlobInformation` objekt som tas emot i kön hälsningsmeddelande (`BlobName` och `BlobNameWithoutExtension`).</span><span class="sxs-lookup"><span data-stu-id="199aa-433">Blob names come from properties of hello `BlobInformation` object received in hello queue message (`BlobName` and `BlobNameWithoutExtension`).</span></span> <span data-ttu-id="199aa-434">tooget hello fullständiga funktionaliteten hos hello Storage-klientbibliotek, du kan använda hello `CloudBlockBlob` klassen toowork med blobbar.</span><span class="sxs-lookup"><span data-stu-id="199aa-434">tooget hello full functionality of hello Storage Client Library, you can use hello `CloudBlockBlob` class toowork with blobs.</span></span> <span data-ttu-id="199aa-435">Om du vill tooreuse kod som har skrivits toowork med `Stream` objekt, kan du använda hello `Stream` klass.</span><span class="sxs-lookup"><span data-stu-id="199aa-435">If you want tooreuse code that was written toowork with `Stream` objects, you can use hello `Stream` class.</span></span>

<span data-ttu-id="199aa-436">Mer information om hur toowrite fungerar som använder WebJobs-SDK-attribut, se hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="199aa-436">For more information about how toowrite functions that use  WebJobs SDK attributes, see hello following resources:</span></span>

* [<span data-ttu-id="199aa-437">Hur toouse Azure kö lagring med hello WebJobs SDK</span><span class="sxs-lookup"><span data-stu-id="199aa-437">How toouse Azure queue storage with hello WebJobs SDK</span></span>](websites-dotnet-webjobs-sdk-storage-queues-how-to.md)
* [<span data-ttu-id="199aa-438">Hur toouse Azure blob storage med hello WebJobs SDK</span><span class="sxs-lookup"><span data-stu-id="199aa-438">How toouse Azure blob storage with hello WebJobs SDK</span></span>](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)
* [<span data-ttu-id="199aa-439">Hur toouse Azure table storage med hello WebJobs SDK</span><span class="sxs-lookup"><span data-stu-id="199aa-439">How toouse Azure table storage with hello WebJobs SDK</span></span>](websites-dotnet-webjobs-sdk-storage-tables-how-to.md)
* [<span data-ttu-id="199aa-440">Hur toouse Azure Service Bus med hello WebJobs SDK</span><span class="sxs-lookup"><span data-stu-id="199aa-440">How toouse Azure Service Bus with hello WebJobs SDK</span></span>](websites-dotnet-webjobs-sdk-service-bus.md)

> [!NOTE]
> * <span data-ttu-id="199aa-441">Om ditt webbprogram som körs på flera virtuella datorer, flera Webbjobb ska köras samtidigt och i vissa fall kan detta resultera i hello samma data komma bearbetas flera gånger.</span><span class="sxs-lookup"><span data-stu-id="199aa-441">If your web app runs on multiple VMs, multiple WebJobs will be running simultaneously, and in some scenarios this can result in hello same data getting processed multiple times.</span></span> <span data-ttu-id="199aa-442">Detta är inte ett problem om du använder hello inbyggda kön, blob och Service Bus-utlösare.</span><span class="sxs-lookup"><span data-stu-id="199aa-442">This is not a problem if you use hello built-in queue, blob, and Service Bus triggers.</span></span> <span data-ttu-id="199aa-443">hello SDK garanterar att dina funktioner kommer att bearbetas bara en gång för varje meddelande eller blob.</span><span class="sxs-lookup"><span data-stu-id="199aa-443">hello SDK ensures that your functions will be processed only once for each message or blob.</span></span>
> * <span data-ttu-id="199aa-444">Information om hur tooimplement korrekt avslutning, se [korrekt avslutning](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#graceful).</span><span class="sxs-lookup"><span data-stu-id="199aa-444">For information about how tooimplement graceful shutdown, see [Graceful Shutdown](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#graceful).</span></span>
> * <span data-ttu-id="199aa-445">Hej koden i hello `ConvertImageToThumbnailJPG` metod (visas inte) använder klasser i hello `System.Drawing` namnområde för enkelhetens skull.</span><span class="sxs-lookup"><span data-stu-id="199aa-445">hello code in hello `ConvertImageToThumbnailJPG` method (not shown) uses classes in hello `System.Drawing` namespace for simplicity.</span></span> <span data-ttu-id="199aa-446">Dock har hello klasser i det här namnområdet utformats för användning med Windows Forms.</span><span class="sxs-lookup"><span data-stu-id="199aa-446">However, hello classes in this namespace were designed for use with Windows Forms.</span></span> <span data-ttu-id="199aa-447">De stöds inte för användning i en Windows- eller ASP.NET-tjänst.</span><span class="sxs-lookup"><span data-stu-id="199aa-447">They are not supported for use in a Windows or ASP.NET service.</span></span> <span data-ttu-id="199aa-448">Mer information om alternativ för bildbearbetning finns i [Dynamic Image Generation](http://www.hanselman.com/blog/BackToBasicsDynamicImageGenerationASPNETControllersRoutingIHttpHandlersAndRunAllManagedModulesForAllRequests.aspx) och [Deep Inside Image Resizing](http://www.hanselminutes.com/313/deep-inside-image-resizing-and-scaling-with-aspnet-and-iis-with-imageresizingnet-author-na).</span><span class="sxs-lookup"><span data-stu-id="199aa-448">For more information about image processing options, see [Dynamic Image Generation](http://www.hanselman.com/blog/BackToBasicsDynamicImageGenerationASPNETControllersRoutingIHttpHandlersAndRunAllManagedModulesForAllRequests.aspx) and [Deep Inside Image Resizing](http://www.hanselminutes.com/313/deep-inside-image-resizing-and-scaling-with-aspnet-and-iis-with-imageresizingnet-author-na).</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="199aa-449">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="199aa-449">Next steps</span></span>
<span data-ttu-id="199aa-450">I den här kursen har du sett ett enkelt program i flera nivåer som använder hello WebJobs-SDK för backend-bearbetning.</span><span class="sxs-lookup"><span data-stu-id="199aa-450">In this tutorial, you've seen a simple multi-tier application that uses hello WebJobs SDK for backend processing.</span></span> <span data-ttu-id="199aa-451">Det här avsnittet innehåller några förslag för att lära dig mer om ASP.NET-program för flera nivåer och WebJobs.</span><span class="sxs-lookup"><span data-stu-id="199aa-451">This section offers some suggestions for learning more about ASP.NET multi-tier applications and WebJobs.</span></span>

### <a name="missing-features"></a><span data-ttu-id="199aa-452">Funktioner som saknas</span><span class="sxs-lookup"><span data-stu-id="199aa-452">Missing features</span></span>
<span data-ttu-id="199aa-453">programmet hello har förenklats för en komma igång-kursen.</span><span class="sxs-lookup"><span data-stu-id="199aa-453">hello application has been kept simple for a getting-started tutorial.</span></span> <span data-ttu-id="199aa-454">I ett riktigt program du vill implementera [beroendeinmatning](http://www.asp.net/mvc/tutorials/hands-on-labs/aspnet-mvc-4-dependency-injection) och hello [centrallager och arbetsenhetsmönster](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application#repo), använda [ett gränssnitt för loggning](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry#log), Använd [ EF Code First Migrations](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application) toomanage data modellera ändringar och använda [EF Connection Resiliency](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application) toomanage tillfälliga nätverksfel.</span><span class="sxs-lookup"><span data-stu-id="199aa-454">In a real-world application you would implement [dependency injection](http://www.asp.net/mvc/tutorials/hands-on-labs/aspnet-mvc-4-dependency-injection) and hello [repository and unit of work patterns](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application#repo), use [an interface for logging](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry#log), use [EF Code First Migrations](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application) toomanage data model changes, and use [EF Connection Resiliency](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application) toomanage transient network errors.</span></span>

### <a name="scaling-webjobs"></a><span data-ttu-id="199aa-455">Skalning WebJobs</span><span class="sxs-lookup"><span data-stu-id="199aa-455">Scaling WebJobs</span></span>
<span data-ttu-id="199aa-456">WebJobs körs i hello kontexten för ett webbprogram och är inte skalbar separat.</span><span class="sxs-lookup"><span data-stu-id="199aa-456">WebJobs run in hello context of a web app and are not scalable separately.</span></span> <span data-ttu-id="199aa-457">Till exempel om du har en Standard web app-instansen du har endast en instans av din bakgrunden körs och den använder vissa hello serverresurser (processor, minne, etc.) som annars skulle vara tillgängliga tooserve webbinnehåll.</span><span class="sxs-lookup"><span data-stu-id="199aa-457">For example, if you have one Standard web app instance, you have only one instance of your background process running, and it is using some of hello server resources (CPU, memory, etc.) that otherwise would be available tooserve web content.</span></span>

<span data-ttu-id="199aa-458">Trafik för olika dag eller veckodag och om hello backend bearbetning av du behöver toodo kan vänta, kan du schemalägga WebJobs-toorun vid tidpunkter med låg trafik.</span><span class="sxs-lookup"><span data-stu-id="199aa-458">If traffic varies by time of day or day of week, and if hello backend processing you need toodo can wait, you could schedule your WebJobs toorun at low-traffic times.</span></span> <span data-ttu-id="199aa-459">Om hello belastningen fortfarande är för högt för lösningen kan köra du hello backend som ett Webbjobb i en separat webbapp som är dedikerad för detta ändamål.</span><span class="sxs-lookup"><span data-stu-id="199aa-459">If hello load is still too high for that solution, you can run hello backend as a WebJob in a separate web app dedicated for that purpose.</span></span> <span data-ttu-id="199aa-460">Du kan sedan skala ditt webbprogram för serverdelen separat från ditt frontend-webbprogram.</span><span class="sxs-lookup"><span data-stu-id="199aa-460">You can then scale your backend web app independently from your frontend web app.</span></span>

<span data-ttu-id="199aa-461">Mer information finns i [skalning WebJobs](websites-webjobs-resources.md#scale).</span><span class="sxs-lookup"><span data-stu-id="199aa-461">For more information, see [Scaling WebJobs](websites-webjobs-resources.md#scale).</span></span>

### <a name="avoiding-web-app-timeout-shut-downs"></a><span data-ttu-id="199aa-462">Undvika web app timeout stängs-listrutor</span><span class="sxs-lookup"><span data-stu-id="199aa-462">Avoiding web app timeout shut-downs</span></span>
<span data-ttu-id="199aa-463">toomake till din WebJobs alltid körs och körs på alla instanser av ditt webbprogram, du har tooenable hello [AlwaysOn](http://weblogs.asp.net/scottgu/archive/2014/01/16/windows-azure-staging-publishing-support-for-web-sites-monitoring-improvements-hyper-v-recovery-manager-ga-and-pci-compliance.aspx) funktion.</span><span class="sxs-lookup"><span data-stu-id="199aa-463">toomake sure your WebJobs are always running, and running on all instances of your web app, you have tooenable hello [AlwaysOn](http://weblogs.asp.net/scottgu/archive/2014/01/16/windows-azure-staging-publishing-support-for-web-sites-monitoring-improvements-hyper-v-recovery-manager-ga-and-pci-compliance.aspx) feature.</span></span>

### <a name="using-hello-webjobs-sdk-outside-of-webjobs"></a><span data-ttu-id="199aa-464">Med hjälp av hello WebJobs SDK utanför WebJobs</span><span class="sxs-lookup"><span data-stu-id="199aa-464">Using hello WebJobs SDK outside of WebJobs</span></span>
<span data-ttu-id="199aa-465">Ett program som använder hello WebJobs SDK har inte toorun i Azure i ett Webbjobb.</span><span class="sxs-lookup"><span data-stu-id="199aa-465">A program that uses hello WebJobs SDK doesn't have toorun in Azure in a WebJob.</span></span> <span data-ttu-id="199aa-466">Det kan köras lokalt och det kan också köras i andra miljöer, till exempel en molntjänst arbetsroll eller en Windows-tjänst.</span><span class="sxs-lookup"><span data-stu-id="199aa-466">It can run locally, and it can also run in other environments such as a Cloud Service worker role or a Windows service.</span></span> <span data-ttu-id="199aa-467">Du kan endast öppna hello WebJobs SDK instrumentpanelen via en Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="199aa-467">However, you can only access hello WebJobs SDK dashboard through an Azure web app.</span></span> <span data-ttu-id="199aa-468">toouse hello instrumentpanelen som du har tooconnect hello web app toohello storage-konto du använder genom att ange hello AzureWebJobsDashboard anslutningssträngen för hello **konfigurera** för hello klassiska portalen.</span><span class="sxs-lookup"><span data-stu-id="199aa-468">toouse hello dashboard you have tooconnect hello web app toohello storage account you're using by setting hello AzureWebJobsDashboard connection string on hello **Configure** tab of hello classic portal.</span></span> <span data-ttu-id="199aa-469">Sedan kan du hämta toohello instrumentpanelen genom att använda hello följande URL:</span><span class="sxs-lookup"><span data-stu-id="199aa-469">Then, you can get toohello Dashboard by using hello following URL:</span></span>

<span data-ttu-id="199aa-470">https://{webappname}.SCM.azurewebsites.NET/azurejobs/#/Functions</span><span class="sxs-lookup"><span data-stu-id="199aa-470">https://{webappname}.scm.azurewebsites.net/azurejobs/#/functions</span></span>

<span data-ttu-id="199aa-471">Mer information finns i [får en instrumentpanel för lokal utveckling med hello WebJobs SDK](http://blogs.msdn.com/b/jmstall/archive/2014/01/27/getting-a-dashboard-for-local-development-with-the-webjobs-sdk.aspx), men Observera att den visar en gamla namn för anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="199aa-471">For more information, see [Getting a dashboard for local development with hello WebJobs SDK](http://blogs.msdn.com/b/jmstall/archive/2014/01/27/getting-a-dashboard-for-local-development-with-the-webjobs-sdk.aspx), but note that it shows an old connection string name.</span></span>

### <a name="more-webjobs-documentation"></a><span data-ttu-id="199aa-472">Mer WebJobs-dokumentation</span><span class="sxs-lookup"><span data-stu-id="199aa-472">More WebJobs documentation</span></span>
<span data-ttu-id="199aa-473">Mer information finns i [Azure WebJobs-dokumentation](http://go.microsoft.com/fwlink/?LinkId=390226).</span><span class="sxs-lookup"><span data-stu-id="199aa-473">For more information, see [Azure WebJobs documentation resources](http://go.microsoft.com/fwlink/?LinkId=390226).</span></span>
