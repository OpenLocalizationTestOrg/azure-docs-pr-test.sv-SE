---
title: "Hämta Media Services tillgångar till din dator - Azure | Microsoft Docs"
description: "Lär dig mer om att hämta tillgångar till din dator. Kodexemplen är skrivna i C# och använder Media Services SDK för .NET."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 8908a1dd-3ffb-4f18-955d-4c8e2d82fc5d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako
ms.openlocfilehash: d8e740e969f68c85842f42c109328423da1b4414
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-deliver-an-asset-by-download"></a><span data-ttu-id="3c844-104">Så här: leverera en tillgång för hämtning</span><span class="sxs-lookup"><span data-stu-id="3c844-104">How to: Deliver an Asset by Download</span></span>
<span data-ttu-id="3c844-105">Det här avsnittet beskrivs alternativ för att leverera media tillgångar som har överförts till Media Services.</span><span class="sxs-lookup"><span data-stu-id="3c844-105">This topic discusses options for delivering media assets uploaded to Media Services.</span></span> <span data-ttu-id="3c844-106">Du kan leverera Media Services-innehåll i Programscenarier med flera.</span><span class="sxs-lookup"><span data-stu-id="3c844-106">You can deliver Media Services content in numerous application scenarios.</span></span> <span data-ttu-id="3c844-107">Du kan hämta media tillgångar eller komma åt dem med hjälp av en positionerare.</span><span class="sxs-lookup"><span data-stu-id="3c844-107">You can download media assets, or access them by using a locator.</span></span> <span data-ttu-id="3c844-108">Du kan skicka medieinnehåll till ett annat program eller till en annan innehållsleverantören.</span><span class="sxs-lookup"><span data-stu-id="3c844-108">You can send media content to another application or to another content provider.</span></span> <span data-ttu-id="3c844-109">Du kan också leverera innehåll med hjälp av en innehåll innehållsleveransnätverk (CDN) för bättre prestanda och skalbarhet.</span><span class="sxs-lookup"><span data-stu-id="3c844-109">For improved performance and scalability, you can also deliver content by using a Content Delivery Network (CDN).</span></span>

<span data-ttu-id="3c844-110">Det här exemplet visar hur du hämtar media tillgångar från Media Services till den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="3c844-110">This example shows how to download media assets from Media Services to your local computer.</span></span> <span data-ttu-id="3c844-111">Koden frågar jobb som är associerade med Media Services-konto av jobb-ID och har tillgång till dess **OutputMediaAssets** samling (vilket är den uppsättning tillgångar i media utdata en eller flera som är ett resultat från att köra ett jobb).</span><span class="sxs-lookup"><span data-stu-id="3c844-111">The code queries the jobs associated with the Media Services account by job ID and accesses its **OutputMediaAssets** collection (which is the set of one or more output media assets that results from running a job).</span></span> <span data-ttu-id="3c844-112">Det här exemplet illustrerar hur du hämtar utdata media tillgångar från ett jobb, men du kan använda samma metod för att ladda ned andra tillgångar.</span><span class="sxs-lookup"><span data-stu-id="3c844-112">This  example shows how to download output media assets from a job, but you can apply the same approach to download other assets.</span></span>

>[!NOTE]
><span data-ttu-id="3c844-113">Det finns en gräns på 1 000 000 principer för olika AMS-principer (till exempel för positionerarprincipen eller ContentKeyAuthorizationPolicy).</span><span class="sxs-lookup"><span data-stu-id="3c844-113">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="3c844-114">Du bör använda samma princip-ID om du alltid använder samma dagar/åtkomstbehörigheter, till exempel principer för positionerare som är avsedda att vara på plats under en längre tid (icke-överföringsprinciper).</span><span class="sxs-lookup"><span data-stu-id="3c844-114">You should use the same policy ID if you are always using the same days / access permissions, for example, policies for locators that are intended to remain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="3c844-115">Mer information finns i [detta](media-services-dotnet-manage-entities.md#limit-access-policies) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="3c844-115">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>

    // Download the output asset of the specified job to a local folder.
    static IAsset DownloadAssetToLocal( string jobId, string outputFolder)
    {
        // This method illustrates how to download a single asset. 
        // However, you can iterate through the OutputAssets
        // collection, and download all assets if there are many. 

        // Get a reference to the job. 
        IJob job = GetJob(jobId);

        // Get a reference to the first output asset. If there were multiple 
        // output media assets you could iterate and handle each one.
        IAsset outputAsset = job.OutputMediaAssets[0];

        // Create a SAS locator to download the asset
        IAccessPolicy accessPolicy = _context.AccessPolicies.Create("File Download Policy", TimeSpan.FromDays(30), AccessPermissions.Read);
        ILocator locator = _context.Locators.CreateLocator(LocatorType.Sas, outputAsset, accessPolicy);

        BlobTransferClient blobTransfer = new BlobTransferClient
        {
            NumberOfConcurrentTransfers = 20,
            ParallelTransferThreadCount = 20
        };

        var downloadTasks = new List<Task>();
        foreach (IAssetFile outputFile in outputAsset.AssetFiles)
        {
            // Use the following event handler to check download progress.
            outputFile.DownloadProgressChanged += DownloadProgress;

            string localDownloadPath = Path.Combine(outputFolder, outputFile.Name);

            Console.WriteLine("File download path:  " + localDownloadPath);

            downloadTasks.Add(outputFile.DownloadAsync(Path.GetFullPath(localDownloadPath), blobTransfer, locator, CancellationToken.None));

            outputFile.DownloadProgressChanged -= DownloadProgress;
        }

        Task.WaitAll(downloadTasks.ToArray());

        return outputAsset;
    }

    static void DownloadProgress(object sender, DownloadProgressChangedEventArgs e)
    {
        Console.WriteLine(string.Format("{0} % download progress. ", e.Progress));
    }



## <a name="media-services-learning-paths"></a><span data-ttu-id="3c844-116">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="3c844-116">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="3c844-117">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="3c844-117">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="3c844-118">Se även</span><span class="sxs-lookup"><span data-stu-id="3c844-118">See Also</span></span>
[<span data-ttu-id="3c844-119">Leverera liveströmmat innehåll</span><span class="sxs-lookup"><span data-stu-id="3c844-119">Deliver streaming content</span></span>](media-services-deliver-streaming-content.md)

