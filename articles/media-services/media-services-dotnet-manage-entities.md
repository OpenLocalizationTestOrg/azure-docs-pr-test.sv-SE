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
# <a name="managing-assets-and-related-entities-with-media-services-net-sdk"></a>Hantera resurser och relaterade entiteter med Media Services .NET SDK
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-manage-entities.md)
> * [REST](media-services-rest-manage-entities.md)
> 
> 

Det här avsnittet visar hur toomanage Azure Media Services entiteter med .NET. 

>[!NOTE]
> Startar 1 April 2017 raderas alla jobb poster i ditt konto som är äldre än 90 dagar automatiskt, tillsammans med dess associerade aktiviteten poster, även om hello Totalt antal poster som är lägre än hello maximala kvoten. Till exempel på 1 April 2017 tas alla jobb poster i ditt konto som är äldre än den 31 December 2016 automatiskt bort. Om du behöver tooarchive hello projektaktivitet/information kan du använda hello-kod som beskrivs i det här avsnittet.

## <a name="prerequisites"></a>Krav

Konfigurera utvecklingsmiljön och fylla hello app.config-fil med anslutningsinformation, enligt beskrivningen i [Media Services-utveckling med .NET](media-services-dotnet-how-to-use.md). 

## <a name="get-an-asset-reference"></a>Hämta en referens till en tillgång
En uppgift som ofta är tooget en referens tooan befintliga tillgångar i Media Services. hello följande kodexempel visar hur du kan hämta en referens för tillgångsinformation från hello tillgångar samlingen på hello server context-objektet, baserat på en tillgång Id. hello följande kod i exemplet används en Linq fråga tooget ett tooan befintliga IAsset referensobjekt.

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

## <a name="list-all-assets"></a>Visa en lista med alla tillgångar
Hello antalet tillgångar som du har i lagring växer, är det bra toolist dina tillgångar. hello följande kodexempel visar hur tooiterate via hello tillgångar mängden hello server context-objektet. Med varje tillgång skriver hello kodexemplet aktuella även några av dess egenskapen värden toohello-konsolen. Varje tillgång kan exempelvis innehålla många mediefiler. hello kodexempel skriver ut alla filer som är associerade med varje tillgång.

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

## <a name="get-a-job-reference"></a>Hämta en referens i projektet

När du arbetar med att bearbeta uppgifter i Media Services-koden måste ofta tooget referens tooan befintliga jobb med ett ID hello följande kodexempel visar hur tooget referens-tooan IJob objekt från hello jobb samling.

Du kanske måste tooget jobbet referens när du startar en tidskrävande kodningsjobbet och måste toocheck hello Jobbstatus i en tråd. I detta fall när hello-metoden returnerar från en tråd måste tooretrieve ett uppdateras referens tooa jobb.

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

## <a name="list-jobs-and-assets"></a>Lista över jobb och tillgångar
En viktig uppgift är toolist tillgångar med deras associerade jobbet i Media Services. hello följande kodexempel visar hur toolist alla IJob-objekt, och därefter för varje jobb visas egenskaper om hello jobb, alla relaterade aktiviteter, alla indata tillgångar och alla utdata tillgångar. hello koden i det här exemplet kan vara användbart för många andra aktiviteter. Till exempel om du vill toolist hello utdata tillgångar från en eller flera kodning jobb som du tidigare körde denna kod visar hur tooaccess hello utdata tillgångar. När du har en referens tooan utdatatillgången leverera du sedan hello innehåll tooother användare eller program genom att hämta den eller tillhandahålla URL: er. 

Mer information om alternativ för att leverera tillgångar finns [leverera tillgångar med hello Media Services SDK för .NET](media-services-deliver-streaming-content.md).

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

## <a name="list-all-access-policies"></a>Visa en lista med alla åtkomstprinciper
I Media Services kan du definiera en åtkomstprincip för en tillgång eller dess filer. En åtkomstprincip definierar hello behörigheter för en fil eller en tillgång (vilken typ av åtkomst och hello varaktighet). I Media Services-koden definiera du vanligtvis en åtkomstprincip genom att skapa ett IAccessPolicy objekt och associera den med en befintlig tillgång. Sedan kan du skapa en ILocator-objektet, vilket gör att du kan ge direktåtkomst tooassets i Media Services. hello Visual Studio-projekt som medföljer den här dokumentationen serien innehåller flera kodexempel som visar hur toocreate och tilldela åt principer och lokaliserare tooassets.

Hej följande exempel visas hur toolist alla åtkomstprinciper på hello-servern och visar hello typ av behörigheter som är associerade med varje. Ett annat bra sätt tooview åtkomstprinciper är toolist alla ILocator objekt på hello-servern och sedan för varje lokaliserare, kan du visa dess associerade åtkomstprincip via egenskapen AccessPolicy.

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
    
## <a name="limit-access-policies"></a>Gränsen åtkomstprinciper 

>[!NOTE]
> Det finns en gräns på 1 000 000 principer för olika AMS-principer (till exempel för positionerarprincipen eller ContentKeyAuthorizationPolicy). Du bör använda hello samma princip-ID om du alltid använder hello samma dagar / åtkomstbehörigheter, till exempel principer för lokaliserare som är avsedda tooremain på plats för lång tid (icke-överföringen principer). 

Du kan till exempel skapa en allmän uppsättning principer med hello följande kod som körs endast en gång i ditt program. Du kan logga ID tooa loggfilen för senare användning:

    double year = 365.25;
    double week = 7;
    IAccessPolicy policyYear = _context.AccessPolicies.Create("One Year", TimeSpan.FromDays(year), AccessPermissions.Read);
    IAccessPolicy policy100Year = _context.AccessPolicies.Create("Hundred Years", TimeSpan.FromDays(year * 100), AccessPermissions.Read);
    IAccessPolicy policyWeek = _context.AccessPolicies.Create("One Week", TimeSpan.FromDays(week), AccessPermissions.Read);

    Console.WriteLine("One year policy ID is: " + policyYear.Id);
    Console.WriteLine("100 year policy ID is: " + policy100Year.Id);
    Console.WriteLine("One week policy ID is: " + policyWeek.Id);

Du kan sedan använda hello befintliga ID: N i koden så här:

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

## <a name="list-all-locators"></a>Visa en lista med alla positionerare
En positionerare är en URL som innehåller en direkt sökväg tooaccess en tillgång, tillsammans med behörigheter toohello tillgång som definieras av hello lokaliserare associerade åtkomstprincip. Varje tillgång kan ha en samling ILocator objekt som är associerade med den på egenskapen lokaliserare. hello serverkontext har också en positionerare samling som innehåller alla lokaliserare.

hello visar följande kodexempel alla positionerare på hello-servern. För varje lokaliserare visas hello Id för hello relaterade tillgången och åtkomstprincip. Den visar även hello typ av behörighet, hello förfallodatum och hello fullständig sökväg toohello tillgången.

Observera att en positionerare sökvägen tooan tillgång är bara en grundläggande URL toohello tillgång. toocreate som en direkt sökväg tooindividual filer att en användare eller ett program kan bläddra till din kod måste lägga till hello specifika sökvägen toohello lokaliserare filsökväg. Mer information om hur toodo detta, se avsnittet hello [leverera tillgångar med hello Media Services SDK för .NET](media-services-deliver-streaming-content.md).

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

## <a name="enumerating-through-large-collections-of-entities"></a>Uppräkning av stora mängder av entiteter
När du frågar entiteter finns en gräns på 1000 entiteter som returneras i taget eftersom offentlig REST-v2 begränsar frågans resultat too1000 resultat. Du behöver toouse hoppa över och vidta vid uppräkning av stora mängder av entiteter. 

hello angetts följande funktion slingor via alla hello jobb i hello Media Services-konto. Media Services returnerar 1000 jobb i Jobbsamlingen. hello-funktionen gör att användning av hoppa över och ta toomake Kontrollera som alla jobb räknas (om du har fler än 1000 jobb i ditt konto).

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

## <a name="delete-an-asset"></a>Ta bort en tillgång
hello följande exempel tar bort en tillgång.

    static void DeleteAsset( IAsset asset)
    {
        // delete hello asset
        asset.Delete();

        // Verify asset deletion
        if (GetAsset(asset.Id) == null)
            Console.WriteLine("Deleted hello Asset");

    }

## <a name="delete-a-job"></a>Ta bort ett jobb
toodelete ett jobb, måste du kontrollera hello jobb som anges i hello tillstånd egenskapen hello tillstånd. Jobb som har slutförts eller avbrutits kan tas bort medan jobb som har vissa andra tillstånd, till exempel köade schemalagda eller bearbetning, måste först avbrytas och sedan kan du ta bort.

hello visar följande kodexempel en metod för att ta bort ett jobb genom att kontrollera status för jobb och ta sedan bort när hello tillstånd slutförts eller avbrutits. Den här koden beror på hello föregående avsnitt i det här avsnittet för att hämta en referens tooa jobbet: hämta en referens i projektet.

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


## <a name="delete-an-access-policy"></a>Ta bort en åtkomstprincip
hello visar följande kodexempel hur tooget en åtkomstprincip för referens tooan baserat på en princip-Id och toodelete hello princip.

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



## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

