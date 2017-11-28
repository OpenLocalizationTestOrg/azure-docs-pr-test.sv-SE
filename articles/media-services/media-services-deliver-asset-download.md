---
title: "aaaDownload Media Services tillgångar tooyour dator - Azure | Microsoft Docs"
description: "Läs mer om toodownload tillgångar tooyour dator. Kodexemplen är skrivna i C# och använder hello Media Services SDK för .NET."
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
ms.openlocfilehash: 6c6e764720caa59d8371178a2682700345f7bc57
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-to-deliver-an-asset-by-download"></a><span data-ttu-id="06d67-104">Så här: leverera en tillgång för hämtning</span><span class="sxs-lookup"><span data-stu-id="06d67-104">How to: Deliver an Asset by Download</span></span>
<span data-ttu-id="06d67-105">Det här avsnittet beskrivs alternativ för att leverera media tillgångar överförs tooMedia tjänster.</span><span class="sxs-lookup"><span data-stu-id="06d67-105">This topic discusses options for delivering media assets uploaded tooMedia Services.</span></span> <span data-ttu-id="06d67-106">Du kan leverera Media Services-innehåll i Programscenarier med flera.</span><span class="sxs-lookup"><span data-stu-id="06d67-106">You can deliver Media Services content in numerous application scenarios.</span></span> <span data-ttu-id="06d67-107">Du kan hämta media tillgångar eller komma åt dem med hjälp av en positionerare.</span><span class="sxs-lookup"><span data-stu-id="06d67-107">You can download media assets, or access them by using a locator.</span></span> <span data-ttu-id="06d67-108">Du kan skicka media innehåll tooanother program eller tooanother innehållsleverantören.</span><span class="sxs-lookup"><span data-stu-id="06d67-108">You can send media content tooanother application or tooanother content provider.</span></span> <span data-ttu-id="06d67-109">Du kan också leverera innehåll med hjälp av en innehåll innehållsleveransnätverk (CDN) för bättre prestanda och skalbarhet.</span><span class="sxs-lookup"><span data-stu-id="06d67-109">For improved performance and scalability, you can also deliver content by using a Content Delivery Network (CDN).</span></span>

<span data-ttu-id="06d67-110">Det här exemplet illustrerar hur toodownload media tillgångar från Media Services tooyour lokal dator.</span><span class="sxs-lookup"><span data-stu-id="06d67-110">This example shows how toodownload media assets from Media Services tooyour local computer.</span></span> <span data-ttu-id="06d67-111">hello kod frågor hello jobb som är associerade med hello Media Services-konto av jobb-ID och har tillgång till dess **OutputMediaAssets** samling (vilket är hello uppsättning tillgångar i media utdata en eller flera som är ett resultat från att köra ett jobb).</span><span class="sxs-lookup"><span data-stu-id="06d67-111">hello code queries hello jobs associated with hello Media Services account by job ID and accesses its **OutputMediaAssets** collection (which is hello set of one or more output media assets that results from running a job).</span></span> <span data-ttu-id="06d67-112">Det här exemplet illustrerar hur hello toodownload utdatamedia tillgångar från ett jobb, men du kan använda samma metod toodownload andra tillgångar.</span><span class="sxs-lookup"><span data-stu-id="06d67-112">This  example shows how toodownload output media assets from a job, but you can apply hello same approach toodownload other assets.</span></span>

>[!NOTE]
><span data-ttu-id="06d67-113">Det finns en gräns på 1 000 000 principer för olika AMS-principer (till exempel för positionerarprincipen eller ContentKeyAuthorizationPolicy).</span><span class="sxs-lookup"><span data-stu-id="06d67-113">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="06d67-114">Du bör använda hello samma princip-ID om du alltid använder hello samma dagar / åtkomstbehörigheter, till exempel principer för lokaliserare som är avsedda tooremain på plats för lång tid (icke-överföringen principer).</span><span class="sxs-lookup"><span data-stu-id="06d67-114">You should use hello same policy ID if you are always using hello same days / access permissions, for example, policies for locators that are intended tooremain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="06d67-115">Mer information finns i [detta](media-services-dotnet-manage-entities.md#limit-access-policies) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="06d67-115">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>

    // Download hello output asset of hello specified job tooa local folder.
    static IAsset DownloadAssetToLocal( string jobId, string outputFolder)
    {
        // This method illustrates how toodownload a single asset. 
        // However, you can iterate through hello OutputAssets
        // collection, and download all assets if there are many. 

        // Get a reference toohello job. 
        IJob job = GetJob(jobId);

        // Get a reference toohello first output asset. If there were multiple 
        // output media assets you could iterate and handle each one.
        IAsset outputAsset = job.OutputMediaAssets[0];

        // Create a SAS locator toodownload hello asset
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
            // Use hello following event handler toocheck download progress.
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



## <a name="media-services-learning-paths"></a><span data-ttu-id="06d67-116">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="06d67-116">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="06d67-117">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="06d67-117">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="06d67-118">Se även</span><span class="sxs-lookup"><span data-stu-id="06d67-118">See Also</span></span>
[<span data-ttu-id="06d67-119">Leverera liveströmmat innehåll</span><span class="sxs-lookup"><span data-stu-id="06d67-119">Deliver streaming content</span></span>](media-services-deliver-streaming-content.md)

