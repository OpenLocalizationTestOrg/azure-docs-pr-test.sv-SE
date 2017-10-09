---
title: "aaaManaging tillgångar och relaterade entiteter med Media Services .NET SDK"
description: "Lär dig hur toomanage tillgångar och relaterade entiteter med hello Media Services SDK för .NET."
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
ms.openlocfilehash: 59a8543ffc6f7f30da2c67a6fcae09bc46da7a52
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="managing-assets-and-related-entities-with-media-services-net-sdk"></a><span data-ttu-id="45291-103">Hantera resurser och relaterade entiteter med Media Services .NET SDK</span><span class="sxs-lookup"><span data-stu-id="45291-103">Managing Assets and Related Entities with Media Services .NET SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="45291-104">.NET</span><span class="sxs-lookup"><span data-stu-id="45291-104">.NET</span></span>](media-services-dotnet-manage-entities.md)
> * [<span data-ttu-id="45291-105">REST</span><span class="sxs-lookup"><span data-stu-id="45291-105">REST</span></span>](media-services-rest-manage-entities.md)
> 
> 

<span data-ttu-id="45291-106">Det här avsnittet visar hur toomanage Azure Media Services entiteter med .NET.</span><span class="sxs-lookup"><span data-stu-id="45291-106">This topic shows how toomanage Azure Media Services entities with .NET.</span></span> 

>[!NOTE]
> <span data-ttu-id="45291-107">Startar 1 April 2017 raderas alla jobb poster i ditt konto som är äldre än 90 dagar automatiskt, tillsammans med dess associerade aktiviteten poster, även om hello Totalt antal poster som är lägre än hello maximala kvoten.</span><span class="sxs-lookup"><span data-stu-id="45291-107">Starting April 1, 2017, any Job record in your account older than 90 days will be automatically deleted, along with its associated Task records, even if hello total number of records is below hello maximum quota.</span></span> <span data-ttu-id="45291-108">Till exempel på 1 April 2017 tas alla jobb poster i ditt konto som är äldre än den 31 December 2016 automatiskt bort.</span><span class="sxs-lookup"><span data-stu-id="45291-108">For example, on April 1, 2017, any Job record in your account older than December 31, 2016, will be automatically deleted.</span></span> <span data-ttu-id="45291-109">Om du behöver tooarchive hello projektaktivitet/information kan du använda hello-kod som beskrivs i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="45291-109">If you need tooarchive hello job/task information, you can use hello code described in this topic.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="45291-110">Krav</span><span class="sxs-lookup"><span data-stu-id="45291-110">Prerequisites</span></span>

<span data-ttu-id="45291-111">Konfigurera utvecklingsmiljön och fylla hello app.config-fil med anslutningsinformation, enligt beskrivningen i [Media Services-utveckling med .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="45291-111">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

## <a name="get-an-asset-reference"></a><span data-ttu-id="45291-112">Hämta en referens till en tillgång</span><span class="sxs-lookup"><span data-stu-id="45291-112">Get an Asset Reference</span></span>
<span data-ttu-id="45291-113">En uppgift som ofta är tooget en referens tooan befintliga tillgångar i Media Services.</span><span class="sxs-lookup"><span data-stu-id="45291-113">A frequent task is tooget a reference tooan existing asset in Media Services.</span></span> <span data-ttu-id="45291-114">hello följande kodexempel visar hur du kan hämta en referens för tillgångsinformation från hello tillgångar samlingen på hello server context-objektet, baserat på en tillgång Id. hello följande kod i exemplet används en Linq fråga tooget ett tooan befintliga IAsset referensobjekt.</span><span class="sxs-lookup"><span data-stu-id="45291-114">hello following code example shows how you can get an asset reference from hello Assets collection on hello server context object, based on an asset Id. hello following code example uses a Linq query tooget a reference tooan existing IAsset object.</span></span>

    static IAsset GetAsset(string assetId)
    {
        // Use a LINQ Select query tooget an asset.
        var assetInstance =
            from a in _context.Assets
            where a.Id == assetId
            select a;
        // Reference hello asset as an IAsset.
        IAsset asset = assetInstance.FirstOrDefault();

        return asset;
    }

## <a name="list-all-assets"></a><span data-ttu-id="45291-115">Visa en lista med alla tillgångar</span><span class="sxs-lookup"><span data-stu-id="45291-115">List All Assets</span></span>
<span data-ttu-id="45291-116">Hello antalet tillgångar som du har i lagring växer, är det bra toolist dina tillgångar.</span><span class="sxs-lookup"><span data-stu-id="45291-116">As hello number of assets you have in storage grows, it is helpful toolist your assets.</span></span> <span data-ttu-id="45291-117">hello följande kodexempel visar hur tooiterate via hello tillgångar mängden hello server context-objektet.</span><span class="sxs-lookup"><span data-stu-id="45291-117">hello following code example shows how tooiterate through hello Assets collection on hello server context object.</span></span> <span data-ttu-id="45291-118">Med varje tillgång skriver hello kodexemplet aktuella även några av dess egenskapen värden toohello-konsolen.</span><span class="sxs-lookup"><span data-stu-id="45291-118">With each asset, hello code example also writes some of its property values toohello console.</span></span> <span data-ttu-id="45291-119">Varje tillgång kan exempelvis innehålla många mediefiler.</span><span class="sxs-lookup"><span data-stu-id="45291-119">For example, each asset can contain many media files.</span></span> <span data-ttu-id="45291-120">hello kodexempel skriver ut alla filer som är associerade med varje tillgång.</span><span class="sxs-lookup"><span data-stu-id="45291-120">hello code example writes out all files associated with each asset.</span></span>

    static void ListAssets()
    {
        string waitMessage = "Building hello list. This may take a few "
            + "seconds tooa few minutes depending on how many assets "
            + "you have."
            + Environment.NewLine + Environment.NewLine
            + "Please wait..."
            + Environment.NewLine;
        Console.Write(waitMessage);

        // Create a Stringbuilder toostore hello list that we build. 
        StringBuilder builder = new StringBuilder();

        foreach (IAsset asset in _context.Assets)
        {
            // Display hello collection of assets.
            builder.AppendLine("");
            builder.AppendLine("******ASSET******");
            builder.AppendLine("Asset ID: " + asset.Id);
            builder.AppendLine("Name: " + asset.Name);
            builder.AppendLine("==============");
            builder.AppendLine("******ASSET FILES******");

            // Display hello files associated with each asset. 
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

## <a name="get-a-job-reference"></a><span data-ttu-id="45291-121">Hämta en referens i projektet</span><span class="sxs-lookup"><span data-stu-id="45291-121">Get a Job Reference</span></span>

<span data-ttu-id="45291-122">När du arbetar med att bearbeta uppgifter i Media Services-koden måste ofta tooget referens tooan befintliga jobb med ett ID hello följande kodexempel visar hur tooget referens-tooan IJob objekt från hello jobb samling.</span><span class="sxs-lookup"><span data-stu-id="45291-122">When you work with processing tasks in Media Services code, you often need tooget a reference tooan existing job based on an Id. hello following code example shows how tooget a reference tooan IJob object from hello Jobs collection.</span></span>

<span data-ttu-id="45291-123">Du kanske måste tooget jobbet referens när du startar en tidskrävande kodningsjobbet och måste toocheck hello Jobbstatus i en tråd.</span><span class="sxs-lookup"><span data-stu-id="45291-123">You may need tooget a job reference when starting a long-running encoding job, and need toocheck hello job status on a thread.</span></span> <span data-ttu-id="45291-124">I detta fall när hello-metoden returnerar från en tråd måste tooretrieve ett uppdateras referens tooa jobb.</span><span class="sxs-lookup"><span data-stu-id="45291-124">In cases like this, when hello method returns from a thread, you need tooretrieve a refreshed reference tooa job.</span></span>

    static IJob GetJob(string jobId)
    {
        // Use a Linq select query tooget an updated 
        // reference by Id. 
        var jobInstance =
            from j in _context.Jobs
            where j.Id == jobId
            select j;
        // Return hello job reference as an Ijob. 
        IJob job = jobInstance.FirstOrDefault();

        return job;
    }

## <a name="list-jobs-and-assets"></a><span data-ttu-id="45291-125">Lista över jobb och tillgångar</span><span class="sxs-lookup"><span data-stu-id="45291-125">List Jobs and Assets</span></span>
<span data-ttu-id="45291-126">En viktig uppgift är toolist tillgångar med deras associerade jobbet i Media Services.</span><span class="sxs-lookup"><span data-stu-id="45291-126">An important related task is toolist assets with their associated job in Media Services.</span></span> <span data-ttu-id="45291-127">hello följande kodexempel visar hur toolist alla IJob-objekt, och därefter för varje jobb visas egenskaper om hello jobb, alla relaterade aktiviteter, alla indata tillgångar och alla utdata tillgångar.</span><span class="sxs-lookup"><span data-stu-id="45291-127">hello following code example shows you how toolist each IJob object, and then for each job, it displays properties about hello job, all related tasks, all input assets, and all output assets.</span></span> <span data-ttu-id="45291-128">hello koden i det här exemplet kan vara användbart för många andra aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="45291-128">hello code in this example can be useful for numerous other tasks.</span></span> <span data-ttu-id="45291-129">Till exempel om du vill toolist hello utdata tillgångar från en eller flera kodning jobb som du tidigare körde denna kod visar hur tooaccess hello utdata tillgångar.</span><span class="sxs-lookup"><span data-stu-id="45291-129">For example, if you want toolist hello output assets from one or more encoding jobs that you ran previously, this code shows how tooaccess hello output assets.</span></span> <span data-ttu-id="45291-130">När du har en referens tooan utdatatillgången leverera du sedan hello innehåll tooother användare eller program genom att hämta den eller tillhandahålla URL: er.</span><span class="sxs-lookup"><span data-stu-id="45291-130">When you have a reference tooan output asset, you can then deliver hello content tooother users or applications by downloading it, or providing URLs.</span></span> 

<span data-ttu-id="45291-131">Mer information om alternativ för att leverera tillgångar finns [leverera tillgångar med hello Media Services SDK för .NET](media-services-deliver-streaming-content.md).</span><span class="sxs-lookup"><span data-stu-id="45291-131">For more information on options for delivering assets, see [Deliver Assets with hello Media Services SDK for .NET](media-services-deliver-streaming-content.md).</span></span>

    // List all jobs on hello server, and for each job, also list 
    // all tasks, all input assets, all output assets.

    static void ListJobsAndAssets()
    {
        string waitMessage = "Building hello list. This may take a few "
            + "seconds tooa few minutes depending on how many assets "
            + "you have."
            + Environment.NewLine + Environment.NewLine
            + "Please wait..."
            + Environment.NewLine;
        Console.Write(waitMessage);

        // Create a Stringbuilder toostore hello list that we build. 
        StringBuilder builder = new StringBuilder();

        foreach (IJob job in _context.Jobs)
        {
            // Display hello collection of jobs on hello server.
            builder.AppendLine("");
            builder.AppendLine("******JOB*******");
            builder.AppendLine("Job ID: " + job.Id);
            builder.AppendLine("Name: " + job.Name);
            builder.AppendLine("State: " + job.State);
            builder.AppendLine("Order: " + job.Priority);
            builder.AppendLine("==============");


            // For each job, display hello associated tasks (a job  
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

            // For each job, display hello list of input media assets.
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

            // For each job, display hello list of output media assets.
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

## <a name="list-all-access-policies"></a><span data-ttu-id="45291-132">Visa en lista med alla åtkomstprinciper</span><span class="sxs-lookup"><span data-stu-id="45291-132">List all Access Policies</span></span>
<span data-ttu-id="45291-133">I Media Services kan du definiera en åtkomstprincip för en tillgång eller dess filer.</span><span class="sxs-lookup"><span data-stu-id="45291-133">In Media Services, you can define an access policy on an asset or its files.</span></span> <span data-ttu-id="45291-134">En åtkomstprincip definierar hello behörigheter för en fil eller en tillgång (vilken typ av åtkomst och hello varaktighet).</span><span class="sxs-lookup"><span data-stu-id="45291-134">An access policy defines hello permissions for a file or an asset (what type of access, and hello duration).</span></span> <span data-ttu-id="45291-135">I Media Services-koden definiera du vanligtvis en åtkomstprincip genom att skapa ett IAccessPolicy objekt och associera den med en befintlig tillgång.</span><span class="sxs-lookup"><span data-stu-id="45291-135">In your Media Services code, you typically define an access policy by creating an IAccessPolicy object and then associating it with an existing asset.</span></span> <span data-ttu-id="45291-136">Sedan kan du skapa en ILocator-objektet, vilket gör att du kan ge direktåtkomst tooassets i Media Services.</span><span class="sxs-lookup"><span data-stu-id="45291-136">Then you create a ILocator object, which lets you provide direct access tooassets in Media Services.</span></span> <span data-ttu-id="45291-137">hello Visual Studio-projekt som medföljer den här dokumentationen serien innehåller flera kodexempel som visar hur toocreate och tilldela åt principer och lokaliserare tooassets.</span><span class="sxs-lookup"><span data-stu-id="45291-137">hello Visual Studio project that accompanies this documentation series contains several code examples that show how toocreate and assign access policies and locators tooassets.</span></span>

<span data-ttu-id="45291-138">Hej följande exempel visas hur toolist alla åtkomstprinciper på hello-servern och visar hello typ av behörigheter som är associerade med varje.</span><span class="sxs-lookup"><span data-stu-id="45291-138">hello following code example shows how toolist all access policies on hello server, and shows hello type of permissions associated with each.</span></span> <span data-ttu-id="45291-139">Ett annat bra sätt tooview åtkomstprinciper är toolist alla ILocator objekt på hello-servern och sedan för varje lokaliserare, kan du visa dess associerade åtkomstprincip via egenskapen AccessPolicy.</span><span class="sxs-lookup"><span data-stu-id="45291-139">Another useful way tooview access policies is toolist all ILocator objects on hello server, and then for each locator, you can list its associated access policy by using its AccessPolicy property.</span></span>

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
    
## <a name="limit-access-policies"></a><span data-ttu-id="45291-140">Gränsen åtkomstprinciper</span><span class="sxs-lookup"><span data-stu-id="45291-140">Limit Access Policies</span></span> 

>[!NOTE]
> <span data-ttu-id="45291-141">Det finns en gräns på 1 000 000 principer för olika AMS-principer (till exempel för positionerarprincipen eller ContentKeyAuthorizationPolicy).</span><span class="sxs-lookup"><span data-stu-id="45291-141">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="45291-142">Du bör använda hello samma princip-ID om du alltid använder hello samma dagar / åtkomstbehörigheter, till exempel principer för lokaliserare som är avsedda tooremain på plats för lång tid (icke-överföringen principer).</span><span class="sxs-lookup"><span data-stu-id="45291-142">You should use hello same policy ID if you are always using hello same days / access permissions, for example, policies for locators that are intended tooremain in place for a long time (non-upload policies).</span></span> 

<span data-ttu-id="45291-143">Du kan till exempel skapa en allmän uppsättning principer med hello följande kod som körs endast en gång i ditt program.</span><span class="sxs-lookup"><span data-stu-id="45291-143">For example, you can create a generic set of policies with hello following code that would only run one time in your application.</span></span> <span data-ttu-id="45291-144">Du kan logga ID tooa loggfilen för senare användning:</span><span class="sxs-lookup"><span data-stu-id="45291-144">You can log IDs tooa log file for later use:</span></span>

    double year = 365.25;
    double week = 7;
    IAccessPolicy policyYear = _context.AccessPolicies.Create("One Year", TimeSpan.FromDays(year), AccessPermissions.Read);
    IAccessPolicy policy100Year = _context.AccessPolicies.Create("Hundred Years", TimeSpan.FromDays(year * 100), AccessPermissions.Read);
    IAccessPolicy policyWeek = _context.AccessPolicies.Create("One Week", TimeSpan.FromDays(week), AccessPermissions.Read);

    Console.WriteLine("One year policy ID is: " + policyYear.Id);
    Console.WriteLine("100 year policy ID is: " + policy100Year.Id);
    Console.WriteLine("One week policy ID is: " + policyWeek.Id);

<span data-ttu-id="45291-145">Du kan sedan använda hello befintliga ID: N i koden så här:</span><span class="sxs-lookup"><span data-stu-id="45291-145">Then, you can use hello existing IDs in your code like this:</span></span>

    const string policy1YearId = "nb:pid:UUID:2a4f0104-51a9-4078-ae26-c730f88d35cf";


    // Get hello standard policy for 1 year read only
    var tempPolicyId = from b in _context.AccessPolicies
                       where b.Id == policy1YearId
                       select b;
    IAccessPolicy policy1Year = tempPolicyId.FirstOrDefault();

    // Get hello existing asset
    var tempAsset = from a in _context.Assets
                where a.Id == assetID
                select a;
    IAsset asset = tempAsset.SingleOrDefault();

    ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
        policy1Year,
        DateTime.UtcNow.AddMinutes(-5));
    Console.WriteLine("hello locator base path is " + originLocator.BaseUri.ToString());

## <a name="list-all-locators"></a><span data-ttu-id="45291-146">Visa en lista med alla positionerare</span><span class="sxs-lookup"><span data-stu-id="45291-146">List All Locators</span></span>
<span data-ttu-id="45291-147">En positionerare är en URL som innehåller en direkt sökväg tooaccess en tillgång, tillsammans med behörigheter toohello tillgång som definieras av hello lokaliserare associerade åtkomstprincip.</span><span class="sxs-lookup"><span data-stu-id="45291-147">A locator is a URL that provides a direct path tooaccess an asset, along with permissions toohello asset as defined by hello locator's associated access policy.</span></span> <span data-ttu-id="45291-148">Varje tillgång kan ha en samling ILocator objekt som är associerade med den på egenskapen lokaliserare.</span><span class="sxs-lookup"><span data-stu-id="45291-148">Each asset can have a collection of ILocator objects associated with it on its Locators property.</span></span> <span data-ttu-id="45291-149">hello serverkontext har också en positionerare samling som innehåller alla lokaliserare.</span><span class="sxs-lookup"><span data-stu-id="45291-149">hello server context also has a Locators collection that contains all locators.</span></span>

<span data-ttu-id="45291-150">hello visar följande kodexempel alla positionerare på hello-servern.</span><span class="sxs-lookup"><span data-stu-id="45291-150">hello following code example lists all locators on hello server.</span></span> <span data-ttu-id="45291-151">För varje lokaliserare visas hello Id för hello relaterade tillgången och åtkomstprincip.</span><span class="sxs-lookup"><span data-stu-id="45291-151">For each locator, it shows hello Id for hello related asset and access policy.</span></span> <span data-ttu-id="45291-152">Den visar även hello typ av behörighet, hello förfallodatum och hello fullständig sökväg toohello tillgången.</span><span class="sxs-lookup"><span data-stu-id="45291-152">It also displays hello type of permissions, hello expiration date, and hello full path toohello asset.</span></span>

<span data-ttu-id="45291-153">Observera att en positionerare sökvägen tooan tillgång är bara en grundläggande URL toohello tillgång.</span><span class="sxs-lookup"><span data-stu-id="45291-153">Note that a locator path tooan asset is only a base URL toohello asset.</span></span> <span data-ttu-id="45291-154">toocreate som en direkt sökväg tooindividual filer att en användare eller ett program kan bläddra till din kod måste lägga till hello specifika sökvägen toohello lokaliserare filsökväg.</span><span class="sxs-lookup"><span data-stu-id="45291-154">toocreate a direct path tooindividual files that a user or application could browse to, your code must add hello specific file path toohello locator path.</span></span> <span data-ttu-id="45291-155">Mer information om hur toodo detta, se avsnittet hello [leverera tillgångar med hello Media Services SDK för .NET](media-services-deliver-streaming-content.md).</span><span class="sxs-lookup"><span data-stu-id="45291-155">For more information on how toodo this, see hello topic [Deliver Assets with hello Media Services SDK for .NET](media-services-deliver-streaming-content.md).</span></span>

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
            // hello locator path is hello base or parent path (with included permissions) tooaccess  
            // hello media content of an asset. toocreate a full URL tooa specific media file, take 
            // hello locator path and then append a file name and info as needed.  
            Console.WriteLine("Locator base path: " + locator.Path);
            Console.WriteLine("");
        }
    }

## <a name="enumerating-through-large-collections-of-entities"></a><span data-ttu-id="45291-156">Uppräkning av stora mängder av entiteter</span><span class="sxs-lookup"><span data-stu-id="45291-156">Enumerating through large collections of entities</span></span>
<span data-ttu-id="45291-157">När du frågar entiteter finns en gräns på 1000 entiteter som returneras i taget eftersom offentlig REST-v2 begränsar frågans resultat too1000 resultat.</span><span class="sxs-lookup"><span data-stu-id="45291-157">When querying entities, there is a limit of 1000 entities returned at one time because public REST v2 limits query results too1000 results.</span></span> <span data-ttu-id="45291-158">Du behöver toouse hoppa över och vidta vid uppräkning av stora mängder av entiteter.</span><span class="sxs-lookup"><span data-stu-id="45291-158">You need toouse Skip and Take when enumerating through large collections of entities.</span></span> 

<span data-ttu-id="45291-159">hello angetts följande funktion slingor via alla hello jobb i hello Media Services-konto.</span><span class="sxs-lookup"><span data-stu-id="45291-159">hello following function loops through all hello jobs in hello provided Media Services Account.</span></span> <span data-ttu-id="45291-160">Media Services returnerar 1000 jobb i Jobbsamlingen.</span><span class="sxs-lookup"><span data-stu-id="45291-160">Media Services returns 1000 jobs in Jobs Collection.</span></span> <span data-ttu-id="45291-161">hello-funktionen gör att användning av hoppa över och ta toomake Kontrollera som alla jobb räknas (om du har fler än 1000 jobb i ditt konto).</span><span class="sxs-lookup"><span data-stu-id="45291-161">hello function makes use of Skip and Take toomake sure that all jobs are enumerated (in case you have more than 1000 jobs in your account).</span></span>

    static void ProcessJobs()
    {
        try
        {

            int skipSize = 0;
            int batchSize = 1000;
            int currentBatch = 0;

            while (true)
            {
                // Loop through all Jobs (1000 at a time) in hello Media Services account
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

## <a name="delete-an-asset"></a><span data-ttu-id="45291-162">Ta bort en tillgång</span><span class="sxs-lookup"><span data-stu-id="45291-162">Delete an Asset</span></span>
<span data-ttu-id="45291-163">hello följande exempel tar bort en tillgång.</span><span class="sxs-lookup"><span data-stu-id="45291-163">hello following example deletes an asset.</span></span>

    static void DeleteAsset( IAsset asset)
    {
        // delete hello asset
        asset.Delete();

        // Verify asset deletion
        if (GetAsset(asset.Id) == null)
            Console.WriteLine("Deleted hello Asset");

    }

## <a name="delete-a-job"></a><span data-ttu-id="45291-164">Ta bort ett jobb</span><span class="sxs-lookup"><span data-stu-id="45291-164">Delete a Job</span></span>
<span data-ttu-id="45291-165">toodelete ett jobb, måste du kontrollera hello jobb som anges i hello tillstånd egenskapen hello tillstånd.</span><span class="sxs-lookup"><span data-stu-id="45291-165">toodelete a job, you must check hello state of hello job as indicated in hello State property.</span></span> <span data-ttu-id="45291-166">Jobb som har slutförts eller avbrutits kan tas bort medan jobb som har vissa andra tillstånd, till exempel köade schemalagda eller bearbetning, måste först avbrytas och sedan kan du ta bort.</span><span class="sxs-lookup"><span data-stu-id="45291-166">Jobs that are finished or canceled can be deleted, while jobs that are in certain other states, such as queued, scheduled, or processing, must be canceled first, and then they can be deleted.</span></span>

<span data-ttu-id="45291-167">hello visar följande kodexempel en metod för att ta bort ett jobb genom att kontrollera status för jobb och ta sedan bort när hello tillstånd slutförts eller avbrutits.</span><span class="sxs-lookup"><span data-stu-id="45291-167">hello following code example shows a method for deleting a job by checking job states and then deleting when hello state is finished or canceled.</span></span> <span data-ttu-id="45291-168">Den här koden beror på hello föregående avsnitt i det här avsnittet för att hämta en referens tooa jobbet: hämta en referens i projektet.</span><span class="sxs-lookup"><span data-stu-id="45291-168">This code depends on hello previous section in this topic for getting a reference tooa job: Get a job reference.</span></span>

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
                    // You can also call job.DeleteAsync toodo async deletes.
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


## <a name="delete-an-access-policy"></a><span data-ttu-id="45291-169">Ta bort en åtkomstprincip</span><span class="sxs-lookup"><span data-stu-id="45291-169">Delete an Access Policy</span></span>
<span data-ttu-id="45291-170">hello visar följande kodexempel hur tooget en åtkomstprincip för referens tooan baserat på en princip-Id och toodelete hello princip.</span><span class="sxs-lookup"><span data-stu-id="45291-170">hello following code example shows how tooget a reference tooan access policy based on a policy Id, and then toodelete hello policy.</span></span>

    static void DeleteAccessPolicy(string existingPolicyId)
    {
        // toodelete a specific access policy, get a reference toohello policy.  
        // based on hello policy Id passed toohello method.
        var policyInstance =
                from p in _context.AccessPolicies
                where p.Id == existingPolicyId
                select p;
        IAccessPolicy policy = policyInstance.FirstOrDefault();

        policy.Delete();

    }



## <a name="media-services-learning-paths"></a><span data-ttu-id="45291-171">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="45291-171">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="45291-172">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="45291-172">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

