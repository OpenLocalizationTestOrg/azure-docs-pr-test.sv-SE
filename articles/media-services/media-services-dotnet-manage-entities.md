---
title: Hantera resurser och relaterade entiteter med Media Services .NET SDK
description: "Lär dig hur du hanterar tillgångar och relaterade entiteter med Media Services SDK för .NET."
author: juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: 1bd8fd42-7306-463d-bfe5-f642802f1906
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: juliako
ms.openlocfilehash: 5efe16a09808267d0797521f9e1df2b60aec9cbb
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="managing-assets-and-related-entities-with-media-services-net-sdk"></a><span data-ttu-id="e4ea7-103">Hantera resurser och relaterade entiteter med Media Services .NET SDK</span><span class="sxs-lookup"><span data-stu-id="e4ea7-103">Managing Assets and Related Entities with Media Services .NET SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e4ea7-104">.NET</span><span class="sxs-lookup"><span data-stu-id="e4ea7-104">.NET</span></span>](media-services-dotnet-manage-entities.md)
> * [<span data-ttu-id="e4ea7-105">REST</span><span class="sxs-lookup"><span data-stu-id="e4ea7-105">REST</span></span>](media-services-rest-manage-entities.md)
> 
> 

<span data-ttu-id="e4ea7-106">Det här avsnittet visar hur du hanterar Azure Media Services entiteter med .NET.</span><span class="sxs-lookup"><span data-stu-id="e4ea7-106">This topic shows how to manage Azure Media Services entities with .NET.</span></span> 

>[!NOTE]
> <span data-ttu-id="e4ea7-107">Från och med 1 april 2017 raderas alla jobbposter i ditt konto som är äldre än 90 dagar automatiskt, tillsammans med deras associerade uppgiftsposter, även om det totala antalet poster är lägre än den maximala kvoten.</span><span class="sxs-lookup"><span data-stu-id="e4ea7-107">Starting April 1, 2017, any Job record in your account older than 90 days will be automatically deleted, along with its associated Task records, even if the total number of records is below the maximum quota.</span></span> <span data-ttu-id="e4ea7-108">Till exempel på 1 April 2017 tas alla jobb poster i ditt konto som är äldre än den 31 December 2016 automatiskt bort.</span><span class="sxs-lookup"><span data-stu-id="e4ea7-108">For example, on April 1, 2017, any Job record in your account older than December 31, 2016, will be automatically deleted.</span></span> <span data-ttu-id="e4ea7-109">Du kan använda koden som beskrivs i det här avsnittet om du behöver Arkivera jobb/aktivitetsinformationen.</span><span class="sxs-lookup"><span data-stu-id="e4ea7-109">If you need to archive the job/task information, you can use the code described in this topic.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e4ea7-110">Krav</span><span class="sxs-lookup"><span data-stu-id="e4ea7-110">Prerequisites</span></span>

<span data-ttu-id="e4ea7-111">Konfigurera utvecklingsmiljön och fyll i filen app.config med anslutningsinformation, enligt beskrivningen i [Media Services-utveckling med .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="e4ea7-111">Set up your development environment and populate the app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

## <a name="get-an-asset-reference"></a><span data-ttu-id="e4ea7-112">Hämta en referens till en tillgång</span><span class="sxs-lookup"><span data-stu-id="e4ea7-112">Get an Asset Reference</span></span>
<span data-ttu-id="e4ea7-113">En uppgift som ofta är att hämta en referens till en befintlig tillgång i Media Services.</span><span class="sxs-lookup"><span data-stu-id="e4ea7-113">A frequent task is to get a reference to an existing asset in Media Services.</span></span> <span data-ttu-id="e4ea7-114">Följande kodexempel visar hur du kan hämta en referens för tillgångsinformation från samlingen tillgångar på servern context-objektet, baserat på en tillgång Id. Följande kodexempel använder en Linq-fråga för att hämta en referens till ett befintligt IAsset-objekt.</span><span class="sxs-lookup"><span data-stu-id="e4ea7-114">The following code example shows how you can get an asset reference from the Assets collection on the server context object, based on an asset Id. The following code example uses a Linq query to get a reference to an existing IAsset object.</span></span>

    static IAsset GetAsset(string assetId)
    {
        // Use a LINQ Select query to get an asset.
        var assetInstance =
            from a in _context.Assets
            where a.Id == assetId
            select a;
        // Reference the asset as an IAsset.
        IAsset asset = assetInstance.FirstOrDefault();

        return asset;
    }

## <a name="list-all-assets"></a><span data-ttu-id="e4ea7-115">Visa en lista med alla tillgångar</span><span class="sxs-lookup"><span data-stu-id="e4ea7-115">List All Assets</span></span>
<span data-ttu-id="e4ea7-116">När antalet tillgångar som du har i lagring växer, är det bra att visa dina tillgångar.</span><span class="sxs-lookup"><span data-stu-id="e4ea7-116">As the number of assets you have in storage grows, it is helpful to list your assets.</span></span> <span data-ttu-id="e4ea7-117">Följande kodexempel visar hur du söker igenom tillgångar samling på servern kontextobjektet.</span><span class="sxs-lookup"><span data-stu-id="e4ea7-117">The following code example shows how to iterate through the Assets collection on the server context object.</span></span> <span data-ttu-id="e4ea7-118">Med varje tillgång skriver kodexemplet aktuella även några av dess egenskapsvärden i konsolen.</span><span class="sxs-lookup"><span data-stu-id="e4ea7-118">With each asset, the code example also writes some of its property values to the console.</span></span> <span data-ttu-id="e4ea7-119">Varje tillgång kan exempelvis innehålla många mediefiler.</span><span class="sxs-lookup"><span data-stu-id="e4ea7-119">For example, each asset can contain many media files.</span></span> <span data-ttu-id="e4ea7-120">Exemplet skriver ut alla filer som är associerade med varje tillgång.</span><span class="sxs-lookup"><span data-stu-id="e4ea7-120">The code example writes out all files associated with each asset.</span></span>

    static void ListAssets()
    {
        string waitMessage = "Building the list. This may take a few "
            + "seconds to a few minutes depending on how many assets "
            + "you have."
            + Environment.NewLine + Environment.NewLine
            + "Please wait..."
            + Environment.NewLine;
        Console.Write(waitMessage);

        // Create a Stringbuilder to store the list that we build. 
        StringBuilder builder = new StringBuilder();

        foreach (IAsset asset in _context.Assets)
        {
            // Display the collection of assets.
            builder.AppendLine("");
            builder.AppendLine("******ASSET******");
            builder.AppendLine("Asset ID: " + asset.Id);
            builder.AppendLine("Name: " + asset.Name);
            builder.AppendLine("==============");
            builder.AppendLine("******ASSET FILES******");

            // Display the files associated with each asset. 
            foreach (IAssetFile fileItem in asset.AssetFiles)
            {
                builder.AppendLine("Name: " + fileItem.Name);
                builder.AppendLine("Size: " + fileItem.ContentFileSize);
                builder.AppendLine("==============");
            }
        }

        // Display output in console.
        Console.Write(builder.ToString());
    }

## <a name="get-a-job-reference"></a><span data-ttu-id="e4ea7-121">Hämta en referens i projektet</span><span class="sxs-lookup"><span data-stu-id="e4ea7-121">Get a Job Reference</span></span>

<span data-ttu-id="e4ea7-122">När du arbetar med att bearbeta uppgifter i Media Services kod måste ofta du hämta en referens till ett befintligt jobb baserat på ett Id. Följande kodexempel visar hur du hämtar en referens till ett IJob objekt från samlingen jobb.</span><span class="sxs-lookup"><span data-stu-id="e4ea7-122">When you work with processing tasks in Media Services code, you often need to get a reference to an existing job based on an Id. The following code example shows how to get a reference to an IJob object from the Jobs collection.</span></span>

<span data-ttu-id="e4ea7-123">Du kan behöva hämta en referens för jobbet när du startar en tidskrävande kodningsjobbet och behöver kontrollera Jobbstatus i en tråd.</span><span class="sxs-lookup"><span data-stu-id="e4ea7-123">You may need to get a job reference when starting a long-running encoding job, and need to check the job status on a thread.</span></span> <span data-ttu-id="e4ea7-124">När metoden returnerar från en tråd i detta fall måste du hämta en uppdateras referens till ett jobb.</span><span class="sxs-lookup"><span data-stu-id="e4ea7-124">In cases like this, when the method returns from a thread, you need to retrieve a refreshed reference to a job.</span></span>

    static IJob GetJob(string jobId)
    {
        // Use a Linq select query to get an updated 
        // reference by Id. 
        var jobInstance =
            from j in _context.Jobs
            where j.Id == jobId
            select j;
        // Return the job reference as an Ijob. 
        IJob job = jobInstance.FirstOrDefault();

        return job;
    }

## <a name="list-jobs-and-assets"></a><span data-ttu-id="e4ea7-125">Lista över jobb och tillgångar</span><span class="sxs-lookup"><span data-stu-id="e4ea7-125">List Jobs and Assets</span></span>
<span data-ttu-id="e4ea7-126">En viktig uppgift är att lista över tillgångar med deras associerade jobbet i Media Services.</span><span class="sxs-lookup"><span data-stu-id="e4ea7-126">An important related task is to list assets with their associated job in Media Services.</span></span> <span data-ttu-id="e4ea7-127">Följande kodexempel visar hur du listar alla IJob-objekt, och sedan för varje projekt, den visar egenskaper om jobbet, alla relaterade uppgifter, alla indata-tillgångar och alla utdata tillgångar.</span><span class="sxs-lookup"><span data-stu-id="e4ea7-127">The following code example shows you how to list each IJob object, and then for each job, it displays properties about the job, all related tasks, all input assets, and all output assets.</span></span> <span data-ttu-id="e4ea7-128">Koden i det här exemplet kan vara användbart för flera uppgifter.</span><span class="sxs-lookup"><span data-stu-id="e4ea7-128">The code in this example can be useful for numerous other tasks.</span></span> <span data-ttu-id="e4ea7-129">Om du vill visa en lista med tillgångar utdata från ett eller flera kodning jobb som du tidigare körde visar denna kod hur du kommer åt utdata tillgångar.</span><span class="sxs-lookup"><span data-stu-id="e4ea7-129">For example, if you want to list the output assets from one or more encoding jobs that you ran previously, this code shows how to access the output assets.</span></span> <span data-ttu-id="e4ea7-130">När du har en referens till en utdatatillgången, kan du sedan leverera innehållet till andra användare eller program genom att hämta den eller tillhandahålla URL: er.</span><span class="sxs-lookup"><span data-stu-id="e4ea7-130">When you have a reference to an output asset, you can then deliver the content to other users or applications by downloading it, or providing URLs.</span></span> 

<span data-ttu-id="e4ea7-131">Mer information om alternativ för att leverera tillgångar finns [leverera tillgångar med Media Services SDK för .NET](media-services-deliver-streaming-content.md).</span><span class="sxs-lookup"><span data-stu-id="e4ea7-131">For more information on options for delivering assets, see [Deliver Assets with the Media Services SDK for .NET](media-services-deliver-streaming-content.md).</span></span>

    // List all jobs on the server, and for each job, also list 
    // all tasks, all input assets, all output assets.

    static void ListJobsAndAssets()
    {
        string waitMessage = "Building the list. This may take a few "
            + "seconds to a few minutes depending on how many assets "
            + "you have."
            + Environment.NewLine + Environment.NewLine
            + "Please wait..."
            + Environment.NewLine;
        Console.Write(waitMessage);

        // Create a Stringbuilder to store the list that we build. 
        StringBuilder builder = new StringBuilder();

        foreach (IJob job in _context.Jobs)
        {
            // Display the collection of jobs on the server.
            builder.AppendLine("");
            builder.AppendLine("******JOB*******");
            builder.AppendLine("Job ID: " + job.Id);
            builder.AppendLine("Name: " + job.Name);
            builder.AppendLine("State: " + job.State);
            builder.AppendLine("Order: " + job.Priority);
            builder.AppendLine("==============");


            // For each job, display the associated tasks (a job  
            // has one or more tasks). 
            builder.AppendLine("******TASKS*******");
            foreach (ITask task in job.Tasks)
            {
                builder.AppendLine("Task Id: " + task.Id);
                builder.AppendLine("Name: " + task.Name);
                builder.AppendLine("Progress: " + task.Progress);
                builder.AppendLine("Configuration: " + task.Configuration);
                if (task.ErrorDetails != null)
                {
                    builder.AppendLine("Error: " + task.ErrorDetails);
                }
                builder.AppendLine("==============");
            }

            // For each job, display the list of input media assets.
            builder.AppendLine("******JOB INPUT MEDIA ASSETS*******");
            foreach (IAsset inputAsset in job.InputMediaAssets)
            {

                if (inputAsset != null)
                {
                    builder.AppendLine("Input Asset Id: " + inputAsset.Id);
                    builder.AppendLine("Name: " + inputAsset.Name);
                    builder.AppendLine("==============");
                }
            }

            // For each job, display the list of output media assets.
            builder.AppendLine("******JOB OUTPUT MEDIA ASSETS*******");
            foreach (IAsset theAsset in job.OutputMediaAssets)
            {
                if (theAsset != null)
                {
                    builder.AppendLine("Output Asset Id: " + theAsset.Id);
                    builder.AppendLine("Name: " + theAsset.Name);
                    builder.AppendLine("==============");
                }
            }

        }

        // Display output in console.
        Console.Write(builder.ToString());
    }

## <a name="list-all-access-policies"></a><span data-ttu-id="e4ea7-132">Visa en lista med alla åtkomstprinciper</span><span class="sxs-lookup"><span data-stu-id="e4ea7-132">List all Access Policies</span></span>
<span data-ttu-id="e4ea7-133">I Media Services kan du definiera en åtkomstprincip för en tillgång eller dess filer.</span><span class="sxs-lookup"><span data-stu-id="e4ea7-133">In Media Services, you can define an access policy on an asset or its files.</span></span> <span data-ttu-id="e4ea7-134">En åtkomstprincip definierar behörigheter för en fil eller en tillgång (vilken typ av åtkomst och varaktighet).</span><span class="sxs-lookup"><span data-stu-id="e4ea7-134">An access policy defines the permissions for a file or an asset (what type of access, and the duration).</span></span> <span data-ttu-id="e4ea7-135">I Media Services-koden definiera du vanligtvis en åtkomstprincip genom att skapa ett IAccessPolicy objekt och associera den med en befintlig tillgång.</span><span class="sxs-lookup"><span data-stu-id="e4ea7-135">In your Media Services code, you typically define an access policy by creating an IAccessPolicy object and then associating it with an existing asset.</span></span> <span data-ttu-id="e4ea7-136">Sedan kan du skapa en ILocator-objektet, vilket gör att du kan ge direktåtkomst till tillgångar i Media Services.</span><span class="sxs-lookup"><span data-stu-id="e4ea7-136">Then you create a ILocator object, which lets you provide direct access to assets in Media Services.</span></span> <span data-ttu-id="e4ea7-137">Visual Studio-projekt som medföljer den här dokumentationen serien innehåller flera kodexempel som visar hur du skapar och tilldelar tillgångar åtkomstprinciper och lokaliserare.</span><span class="sxs-lookup"><span data-stu-id="e4ea7-137">The Visual Studio project that accompanies this documentation series contains several code examples that show how to create and assign access policies and locators to assets.</span></span>

<span data-ttu-id="e4ea7-138">Följande kodexempel visar hur du listar alla åtkomstprinciper på servern och visar vilken typ av behörigheter som associeras med varje.</span><span class="sxs-lookup"><span data-stu-id="e4ea7-138">The following code example shows how to list all access policies on the server, and shows the type of permissions associated with each.</span></span> <span data-ttu-id="e4ea7-139">Ett annat bra sätt att visa principer för åtkomst är att lista alla ILocator objekt på servern och sedan för varje lokaliserare, kan du visa dess associerade åtkomstprincip via egenskapen AccessPolicy.</span><span class="sxs-lookup"><span data-stu-id="e4ea7-139">Another useful way to view access policies is to list all ILocator objects on the server, and then for each locator, you can list its associated access policy by using its AccessPolicy property.</span></span>

    static void ListAllPolicies()
    {
        foreach (IAccessPolicy policy in _context.AccessPolicies)
        {
            Console.WriteLine("");
            Console.WriteLine("Name:  " + policy.Name);
            Console.WriteLine("ID:  " + policy.Id);
            Console.WriteLine("Permissions: " + policy.Permissions);
            Console.WriteLine("==============");

        }
    }
    
## <a name="limit-access-policies"></a><span data-ttu-id="e4ea7-140">Gränsen åtkomstprinciper</span><span class="sxs-lookup"><span data-stu-id="e4ea7-140">Limit Access Policies</span></span> 

>[!NOTE]
> <span data-ttu-id="e4ea7-141">Det finns en gräns på 1 000 000 principer för olika AMS-principer (till exempel för positionerarprincipen eller ContentKeyAuthorizationPolicy).</span><span class="sxs-lookup"><span data-stu-id="e4ea7-141">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="e4ea7-142">Du bör använda samma princip-ID om du alltid använder samma dagar/åtkomstbehörigheter, till exempel principer för positionerare som är avsedda att vara på plats under en längre tid (icke-överföringsprinciper).</span><span class="sxs-lookup"><span data-stu-id="e4ea7-142">You should use the same policy ID if you are always using the same days / access permissions, for example, policies for locators that are intended to remain in place for a long time (non-upload policies).</span></span> 

<span data-ttu-id="e4ea7-143">Du kan till exempel skapa en allmän uppsättning principer med följande kod som körs endast en gång i ditt program.</span><span class="sxs-lookup"><span data-stu-id="e4ea7-143">For example, you can create a generic set of policies with the following code that would only run one time in your application.</span></span> <span data-ttu-id="e4ea7-144">Du kan logga ID: N till en loggfil för senare användning:</span><span class="sxs-lookup"><span data-stu-id="e4ea7-144">You can log IDs to a log file for later use:</span></span>

    double year = 365.25;
    double week = 7;
    IAccessPolicy policyYear = _context.AccessPolicies.Create("One Year", TimeSpan.FromDays(year), AccessPermissions.Read);
    IAccessPolicy policy100Year = _context.AccessPolicies.Create("Hundred Years", TimeSpan.FromDays(year * 100), AccessPermissions.Read);
    IAccessPolicy policyWeek = _context.AccessPolicies.Create("One Week", TimeSpan.FromDays(week), AccessPermissions.Read);

    Console.WriteLine("One year policy ID is: " + policyYear.Id);
    Console.WriteLine("100 year policy ID is: " + policy100Year.Id);
    Console.WriteLine("One week policy ID is: " + policyWeek.Id);

<span data-ttu-id="e4ea7-145">Du kan sedan använda de befintliga ID i koden så här:</span><span class="sxs-lookup"><span data-stu-id="e4ea7-145">Then, you can use the existing IDs in your code like this:</span></span>

    const string policy1YearId = "nb:pid:UUID:2a4f0104-51a9-4078-ae26-c730f88d35cf";


    // Get the standard policy for 1 year read only
    var tempPolicyId = from b in _context.AccessPolicies
                       where b.Id == policy1YearId
                       select b;
    IAccessPolicy policy1Year = tempPolicyId.FirstOrDefault();

    // Get the existing asset
    var tempAsset = from a in _context.Assets
                where a.Id == assetID
                select a;
    IAsset asset = tempAsset.SingleOrDefault();

    ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
        policy1Year,
        DateTime.UtcNow.AddMinutes(-5));
    Console.WriteLine("The locator base path is " + originLocator.BaseUri.ToString());

## <a name="list-all-locators"></a><span data-ttu-id="e4ea7-146">Visa en lista med alla positionerare</span><span class="sxs-lookup"><span data-stu-id="e4ea7-146">List All Locators</span></span>
<span data-ttu-id="e4ea7-147">En positionerare är en URL som innehåller en direkt sökväg för att komma åt en tillgång, tillsammans med behörigheter till tillgången som definieras av den positionerare associerade åtkomstprincip.</span><span class="sxs-lookup"><span data-stu-id="e4ea7-147">A locator is a URL that provides a direct path to access an asset, along with permissions to the asset as defined by the locator's associated access policy.</span></span> <span data-ttu-id="e4ea7-148">Varje tillgång kan ha en samling ILocator objekt som är associerade med den på egenskapen lokaliserare.</span><span class="sxs-lookup"><span data-stu-id="e4ea7-148">Each asset can have a collection of ILocator objects associated with it on its Locators property.</span></span> <span data-ttu-id="e4ea7-149">Serverkontext har också en positionerare samling som innehåller alla lokaliserare.</span><span class="sxs-lookup"><span data-stu-id="e4ea7-149">The server context also has a Locators collection that contains all locators.</span></span>

<span data-ttu-id="e4ea7-150">Följande kodexempel visar alla positionerare på servern.</span><span class="sxs-lookup"><span data-stu-id="e4ea7-150">The following code example lists all locators on the server.</span></span> <span data-ttu-id="e4ea7-151">För varje lokaliserare visas Id för den relaterade tillgången och åtkomst.</span><span class="sxs-lookup"><span data-stu-id="e4ea7-151">For each locator, it shows the Id for the related asset and access policy.</span></span> <span data-ttu-id="e4ea7-152">Den visar även typ av behörigheter, förfallodatum och den fullständiga sökvägen till tillgången.</span><span class="sxs-lookup"><span data-stu-id="e4ea7-152">It also displays the type of permissions, the expiration date, and the full path to the asset.</span></span>

<span data-ttu-id="e4ea7-153">Observera att en positionerare sökväg till en tillgång är en grundläggande Webbadress till tillgången.</span><span class="sxs-lookup"><span data-stu-id="e4ea7-153">Note that a locator path to an asset is only a base URL to the asset.</span></span> <span data-ttu-id="e4ea7-154">Om du vill skapa en direkt sökväg till enskilda filer som en användare eller ett program kan bläddra till din kod måste lägga till specifika filsökvägen lokaliserare sökvägen.</span><span class="sxs-lookup"><span data-stu-id="e4ea7-154">To create a direct path to individual files that a user or application could browse to, your code must add the specific file path to the locator path.</span></span> <span data-ttu-id="e4ea7-155">Mer information om hur du gör detta finns i avsnittet [leverera tillgångar med Media Services SDK för .NET](media-services-deliver-streaming-content.md).</span><span class="sxs-lookup"><span data-stu-id="e4ea7-155">For more information on how to do this, see the topic [Deliver Assets with the Media Services SDK for .NET](media-services-deliver-streaming-content.md).</span></span>

    static void ListAllLocators()
    {
        foreach (ILocator locator in _context.Locators)
        {
            Console.WriteLine("***********");
            Console.WriteLine("Locator Id: " + locator.Id);
            Console.WriteLine("Locator asset Id: " + locator.AssetId);
            Console.WriteLine("Locator access policy Id: " + locator.AccessPolicyId);
            Console.WriteLine("Access policy permissions: " + locator.AccessPolicy.Permissions);
            Console.WriteLine("Locator expiration: " + locator.ExpirationDateTime);
            // The locator path is the base or parent path (with included permissions) to access  
            // the media content of an asset. To create a full URL to a specific media file, take 
            // the locator path and then append a file name and info as needed.  
            Console.WriteLine("Locator base path: " + locator.Path);
            Console.WriteLine("");
        }
    }

## <a name="enumerating-through-large-collections-of-entities"></a><span data-ttu-id="e4ea7-156">Uppräkning av stora mängder av entiteter</span><span class="sxs-lookup"><span data-stu-id="e4ea7-156">Enumerating through large collections of entities</span></span>
<span data-ttu-id="e4ea7-157">När du frågar entiteter, finns det en gräns på 1000 entiteter som returneras i taget eftersom offentlig REST-v2 begränsar frågeresultaten till 1000 resultat.</span><span class="sxs-lookup"><span data-stu-id="e4ea7-157">When querying entities, there is a limit of 1000 entities returned at one time because public REST v2 limits query results to 1000 results.</span></span> <span data-ttu-id="e4ea7-158">Du måste använda Skip och vidta vid uppräkning av stora mängder av entiteter.</span><span class="sxs-lookup"><span data-stu-id="e4ea7-158">You need to use Skip and Take when enumerating through large collections of entities.</span></span> 

<span data-ttu-id="e4ea7-159">Följande funktion loop genom alla jobb i den angivna Media Services-konto.</span><span class="sxs-lookup"><span data-stu-id="e4ea7-159">The following function loops through all the jobs in the provided Media Services Account.</span></span> <span data-ttu-id="e4ea7-160">Media Services returnerar 1000 jobb i Jobbsamlingen.</span><span class="sxs-lookup"><span data-stu-id="e4ea7-160">Media Services returns 1000 jobs in Jobs Collection.</span></span> <span data-ttu-id="e4ea7-161">Funktionen tillämpar Skip och vidta för att se till att alla jobb räknas (om du har fler än 1000 jobb i ditt konto).</span><span class="sxs-lookup"><span data-stu-id="e4ea7-161">The function makes use of Skip and Take to make sure that all jobs are enumerated (in case you have more than 1000 jobs in your account).</span></span>

    static void ProcessJobs()
    {
        try
        {

            int skipSize = 0;
            int batchSize = 1000;
            int currentBatch = 0;

            while (true)
            {
                // Loop through all Jobs (1000 at a time) in the Media Services account
                IQueryable _jobsCollectionQuery = _context.Jobs.Skip(skipSize).Take(batchSize);
                foreach (IJob job in _jobsCollectionQuery)
                {
                    currentBatch++;
                    Console.WriteLine("Processing Job Id:" + job.Id);
                }

                if (currentBatch == batchSize)
                {
                    skipSize += batchSize;
                    currentBatch = 0;
                }
                else
                {
                    break;
                }
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine(ex.Message);
        }
    }

## <a name="delete-an-asset"></a><span data-ttu-id="e4ea7-162">Ta bort en tillgång</span><span class="sxs-lookup"><span data-stu-id="e4ea7-162">Delete an Asset</span></span>
<span data-ttu-id="e4ea7-163">I följande exempel tar bort en tillgång.</span><span class="sxs-lookup"><span data-stu-id="e4ea7-163">The following example deletes an asset.</span></span>

    static void DeleteAsset( IAsset asset)
    {
        // delete the asset
        asset.Delete();

        // Verify asset deletion
        if (GetAsset(asset.Id) == null)
            Console.WriteLine("Deleted the Asset");

    }

## <a name="delete-a-job"></a><span data-ttu-id="e4ea7-164">Ta bort ett jobb</span><span class="sxs-lookup"><span data-stu-id="e4ea7-164">Delete a Job</span></span>
<span data-ttu-id="e4ea7-165">Om du vill ta bort ett jobb, måste du kontrollera status för jobbet som anges i egenskapen State.</span><span class="sxs-lookup"><span data-stu-id="e4ea7-165">To delete a job, you must check the state of the job as indicated in the State property.</span></span> <span data-ttu-id="e4ea7-166">Jobb som har slutförts eller avbrutits kan tas bort medan jobb som har vissa andra tillstånd, till exempel köade schemalagda eller bearbetning, måste först avbrytas och sedan kan du ta bort.</span><span class="sxs-lookup"><span data-stu-id="e4ea7-166">Jobs that are finished or canceled can be deleted, while jobs that are in certain other states, such as queued, scheduled, or processing, must be canceled first, and then they can be deleted.</span></span>

<span data-ttu-id="e4ea7-167">Följande exempel visar en metod för att ta bort ett jobb genom att kontrollera status för jobb och ta sedan bort när tillståndet slutförts eller avbrutits.</span><span class="sxs-lookup"><span data-stu-id="e4ea7-167">The following code example shows a method for deleting a job by checking job states and then deleting when the state is finished or canceled.</span></span> <span data-ttu-id="e4ea7-168">Den här koden är beroende av föregående avsnitt i det här avsnittet för att hämta en referens till ett jobb: hämta en referens i projektet.</span><span class="sxs-lookup"><span data-stu-id="e4ea7-168">This code depends on the previous section in this topic for getting a reference to a job: Get a job reference.</span></span>

    static void DeleteJob(string jobId)
    {
        bool jobDeleted = false;

        while (!jobDeleted)
        {
            // Get an updated job reference.  
            IJob job = GetJob(jobId);

            // Check and handle various possible job states. You can 
            // only delete a job whose state is Finished, Error, or Canceled.   
            // You can cancel jobs that are Queued, Scheduled, or Processing,  
            // and then delete after they are canceled.
            switch (job.State)
            {
                case JobState.Finished:
                case JobState.Canceled:
                case JobState.Error:
                    // Job errors should already be logged by polling or event 
                    // handling methods such as CheckJobProgress or StateChanged.
                    // You can also call job.DeleteAsync to do async deletes.
                    job.Delete();
                    Console.WriteLine("Job has been deleted.");
                    jobDeleted = true;
                    break;
                case JobState.Canceling:
                    Console.WriteLine("Job is cancelling and will be deleted "
                        + "when finished.");
                    Console.WriteLine("Wait while job finishes canceling...");
                    Thread.Sleep(5000);
                    break;
                case JobState.Queued:
                case JobState.Scheduled:
                case JobState.Processing:
                    job.Cancel();
                    Console.WriteLine("Job is scheduled or processing and will "
                        + "be deleted.");
                    break;
                default:
                    break;
            }

        }
    }


## <a name="delete-an-access-policy"></a><span data-ttu-id="e4ea7-169">Ta bort en åtkomstprincip</span><span class="sxs-lookup"><span data-stu-id="e4ea7-169">Delete an Access Policy</span></span>
<span data-ttu-id="e4ea7-170">Följande kodexempel visar hur du hämtar en referens till en princip utifrån en princip-Id, och sedan ta bort principen.</span><span class="sxs-lookup"><span data-stu-id="e4ea7-170">The following code example shows how to get a reference to an access policy based on a policy Id, and then to delete the policy.</span></span>

    static void DeleteAccessPolicy(string existingPolicyId)
    {
        // To delete a specific access policy, get a reference to the policy.  
        // based on the policy Id passed to the method.
        var policyInstance =
                from p in _context.AccessPolicies
                where p.Id == existingPolicyId
                select p;
        IAccessPolicy policy = policyInstance.FirstOrDefault();

        policy.Delete();

    }



## <a name="media-services-learning-paths"></a><span data-ttu-id="e4ea7-171">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="e4ea7-171">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="e4ea7-172">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="e4ea7-172">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

