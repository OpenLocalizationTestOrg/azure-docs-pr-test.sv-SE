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
# <a name="how-to-deliver-an-asset-by-download"></a>Så här: leverera en tillgång för hämtning
Det här avsnittet beskrivs alternativ för att leverera media tillgångar överförs tooMedia tjänster. Du kan leverera Media Services-innehåll i Programscenarier med flera. Du kan hämta media tillgångar eller komma åt dem med hjälp av en positionerare. Du kan skicka media innehåll tooanother program eller tooanother innehållsleverantören. Du kan också leverera innehåll med hjälp av en innehåll innehållsleveransnätverk (CDN) för bättre prestanda och skalbarhet.

Det här exemplet illustrerar hur toodownload media tillgångar från Media Services tooyour lokal dator. hello kod frågor hello jobb som är associerade med hello Media Services-konto av jobb-ID och har tillgång till dess **OutputMediaAssets** samling (vilket är hello uppsättning tillgångar i media utdata en eller flera som är ett resultat från att köra ett jobb). Det här exemplet illustrerar hur hello toodownload utdatamedia tillgångar från ett jobb, men du kan använda samma metod toodownload andra tillgångar.

>[!NOTE]
>Det finns en gräns på 1 000 000 principer för olika AMS-principer (till exempel för positionerarprincipen eller ContentKeyAuthorizationPolicy). Du bör använda hello samma princip-ID om du alltid använder hello samma dagar / åtkomstbehörigheter, till exempel principer för lokaliserare som är avsedda tooremain på plats för lång tid (icke-överföringen principer). Mer information finns i [detta](media-services-dotnet-manage-entities.md#limit-access-policies) avsnitt.

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



## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Se även
[Leverera liveströmmat innehåll](media-services-deliver-streaming-content.md)

