---
title: "aaaGet igång med att leverera innehåll på begäran med hjälp av .NET | Microsoft Docs"
description: "Den här självstudiekursen vägleder dig genom hello stegen för att implementera ett program för leverans av på begäran med Azure Media Services med hjälp av .NET."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 388b8928-9aa9-46b1-b60a-a918da75bd7b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 07/31/2017
ms.author: juliako
ms.openlocfilehash: 4ca9394bd581e1d9062e5a008a410b2c058e017e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-delivering-content-on-demand-using-net-sdk"></a>Kom igång med att leverera innehåll på begäran med hjälp av .NET SDK
[!INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

Den här självstudiekursen vägleder dig genom hello stegen för att implementera en grundläggande Video-on-Demand (VoD) innehållsleverans tjänst med Azure Media Services (AMS)-program med hjälp av hello Azure Media Services .NET SDK.

## <a name="prerequisites"></a>Krav

hello följande är obligatoriska toocomplete hello kursen:

* Ett Azure-konto. Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/).
* Ett Media Services-konto. toocreate Media Services-konto finns [hur tooCreate Media Services-konto](media-services-portal-create-account.md).
* .NET Framework 4.0 eller senare.
* Visual Studio.

Den här självstudiekursen innehåller hello följande uppgifter:

1. Starta strömmande slutpunkten (med hello Azure-portalen).
2. Skapar och konfigurerar ett Visual Studio-projekt.
3. Ansluta toohello Media Services-konto.
2. Överföra en videofil.
3. Koda hello källfilen till en uppsättning MP4-filer med anpassningsbar bithastighet.
4. Publicera hello tillgången och get strömning och progressiv nedladdning URL: er.  
5. Spela upp ditt innehåll.

## <a name="overview"></a>Översikt
Den här självstudiekursen vägleder dig genom hello stegen för att implementera ett program för leverans av Video-on-Demand (VoD) med Azure Media Services (AMS) SDK för .NET.

hello självstudierna innehåller hello grundläggande Media Services-arbetsflödet och hello vanligaste programmeringsobjekt och uppgifter som krävs för Media Services-utveckling. På hello kursen hello är klar kan ska du kunna toostream eller progressivt hämta en exempelmediefil som överförs, kodat och hämtat.

### <a name="ams-model"></a>AMS-modell

hello följande bild visar några av de vanligaste hello objekt när du utvecklar program VoD mot hello Media Services OData-modellen.

Klicka på hello avbildningen tooview den full storlek.  

<a href="./media/media-services-dotnet-get-started/media-services-overview-object-model.png" target="_blank"><img src="./media/media-services-dotnet-get-started/media-services-overview-object-model-small.png"></a> 

Du kan visa hela modellen hello [här](https://media.windows.net/API/$metadata?api-version=2.15).  

## <a name="start-streaming-endpoints-using-hello-azure-portal"></a>Starta strömningsslutpunkter med hello Azure-portalen

När du arbetar med Azure Media Services är en av hello vanligaste scenarierna att leverera video via strömning med anpassningsbar bithastighet. Media Services tillhandahåller en dynamisk paketering som gör att du toodeliver dina MP4-kodade innehåll i strömningsformat som stöds av Media Services (MPEG DASH, HLS, Smooth Streaming) just-in-time, utan att behöva toostore tillsammans med anpassad bithastighet versioner av var och en av dessa strömningsformat.

>[!NOTE]
>När AMS-kontot skapas en **standard** strömningsslutpunkt har lagts till tooyour konto i hello **stoppad** tillstånd. toostart strömning ditt innehåll och dra nytta av dynamisk paketering och dynamisk kryptering hello strömningsslutpunkt som du vill toostream innehåll har toobe i hello **kör** tillstånd.

toostart Hej strömmande slutpunkten, hello följande:

1. Logga in på hello [Azure-portalen](https://portal.azure.com/).
2. Streaming slutpunkter på hello inställningar i fönstret.
3. Klicka på hello standard strömmande slutpunkten.

    hello visas standard information om den STRÖMNINGSSLUTPUNKT.

4. Klicka på hello Start-ikonen.
5. Klicka på hello spara knappen toosave ändringarna.

## <a name="create-and-configure-a-visual-studio-project"></a>Skapa och konfigurera ett Visual Studio-projekt

1. Konfigurera utvecklingsmiljön och fylla hello app.config-fil med anslutningsinformation, enligt beskrivningen i [Media Services-utveckling med .NET](media-services-dotnet-how-to-use.md). 
2. Skapa en ny mapp (mappen kan finnas var som helst på den lokala enheten) och kopiera en .mp4-fil som du vill att tooencode och dataströmmen eller progressivt hämta. I det här exemplet används sökvägen för hello ”C:\VideoFiles”.

## <a name="connect-toohello-media-services-account"></a>Ansluta toohello Media Services-konto

När du använder Media Services med .NET, måste du använda hello **CloudMediaContext** klass för de flesta Media Services-programmeringsuppgifter: ansluta tooMedia Services-konto, skapa, uppdatera, åtkomst till och ta bort följande hello objekt: tillgångar, tillgångsfiler, jobb, åtkomstprinciper, positionerare o.s.v..

Skriv över hello programklass som standard med hello följande kod. hello kod visar hur tooread hello anslutningsvärdena från hello App.config-fil och toocreate hello **CloudMediaContext** objekt i ordning tooconnect tooMedia Services. Mer information finns i [ansluter toohello Media Services API](media-services-use-aad-auth-to-access-ams-api.md).

Se till att tooupdate hello filen namnet och sökvägen toowhere du har din mediefilen.

Hej **Main** anropar metoder som definieras ytterligare i det här avsnittet.

> [!NOTE]
> Du kommer att få kompileringsfel tills du lägger till definitioner för alla hello-funktioner.

    class Program
    {
        // Read values from hello App.config file.
        private static readonly string _AADTenantDomain =
        ConfigurationManager.AppSettings["AADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
        ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

        private static CloudMediaContext _context = null;

        static void Main(string[] args)
        {
        try
        {
            var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

            // Add calls toomethods defined in this section.
            // Make sure tooupdate hello file name and path toowhere you have your media file.
            IAsset inputAsset =
            UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.None);

            IAsset encodedAsset =
            EncodeToAdaptiveBitrateMP4s(inputAsset, AssetCreationOptions.None);

            PublishAssetGetURLs(encodedAsset);
        }
        catch (Exception exception)
        {
            // Parse hello XML error message in hello Media Services response and create a new
            // exception with its content.
            exception = MediaServicesExceptionParser.Parse(exception);

            Console.Error.WriteLine(exception.Message);
        }
        finally
        {
            Console.ReadLine();
        }
        }
    }

## <a name="create-a-new-asset-and-upload-a-video-file"></a>Skapa en ny tillgång och ladda upp en videofil

I Media Services överför du (eller för in) dina digitala filer till en tillgång. Hej **tillgången** entitet kan innehålla video, ljud, bilder, miniatyrsamlingar, textspår och textning filer (och hello metadata om dessa filer.)  När hello filerna har överförts lagras innehållet på ett säkert sätt i hello molnet för ytterligare bearbetning och strömning. hello filerna i hello tillgången kallas **Tillgångsfiler**.

Hej **UploadFile** metod som definieras nedan kallas **CreateFromFile** (definieras i .NET SDK-tillägg). **CreateFromFile** skapar en ny tillgång till vilka hello angivna källfilen överförs.

Hej **CreateFromFile** metoden tar **AssetCreationOptions** kan du ange något av följande alternativ för skapande av tillgångsinformation hello:

* **Ingen** – Ingen kryptering används. Detta är standardvärdet för hello. Observera att när du använder det här alternativet skyddas inte innehållet under överföring eller i vila i lagringsutrymmet.
  Använd det här alternativet om du planerar toodeliver en MP4 med progressivt nedladdning.
* **StorageEncrypted** -och använda det här alternativet tooencrypt innehållet lokalt med hjälp av Advanced Encryption Standard (AES) 256-bitars kryptering, som sedan överför tooAzure lagring där den lagras krypterat i vila. Tillgångar som skyddas med Lagringskryptering okrypterad och placeras i en krypterad fil system tidigare tooencoding och eventuellt omkrypterade tidigare toouploading tillbaka som en ny utdatatillgång automatiskt. hello primära användningsfall för Lagringskryptering är om du vill toosecure hög kvalitet inkommande mediefilerna med stark kryptering i vila på disk.
* **CommonEncryptionProtected** – Använd det här alternativet om du överför innehåll som redan har krypterats och som skyddas med vanlig kryptering eller PlayReady DRM (till exempel Smooth Streaming som skyddas med PlayReady DRM).
* **EnvelopeEncryptionProtected** – Använd det här alternativet om du överför HLS som krypterats med AES. Observera att hello filer måste har kodats och krypterats av Transform Manager.

Hej **CreateFromFile** metod kan du även ange ett återanrop i ordning tooreport hello Överföringsförlopp hello-filen.

I följande exempel hello, anger vi **ingen** för hello tillgångsalternativen.

Lägg till följande metodklassen toohello programmet hello.

    static public IAsset UploadFile(string fileName, AssetCreationOptions options)
    {
        IAsset inputAsset = _context.Assets.CreateFromFile(
            fileName,
            options,
            (af, p) =>
            {
                Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });

        Console.WriteLine("Asset {0} created.", inputAsset.Id);

        return inputAsset;
    }


## <a name="encode-hello-source-file-into-a-set-of-adaptive-bitrate-mp4-files"></a>Koda hello källfilen till en uppsättning MP4-filer med anpassningsbar bithastighet
När du har fört in tillgångar i Media Services kan media kodas, transmuxed, en vattenstämpel, och så vidare innan de skickas tooclients. Dessa aktiviteter schemaläggs och körs mot flera bakgrund rollen instanser tooensure hög prestanda och tillgänglighet. Aktiviteterna kallas jobb och varje jobb består av atomiska uppgifter som hello faktiska arbetet i tillgångsfilen hello.

Som tidigare nämnts, när du arbetar med Azure Media Services ett av de vanligaste scenarierna för hello levererar strömning tooyour klienter med anpassningsbar bithastighet. Media Services kan dynamiskt Paketera en uppsättning MP4-filer med anpassningsbar bithastighet till en hello följande format: HTTP Live Streaming (HLS), Smooth Streaming och MPEG DASH.

tootake nytta av dynamisk paketering behöver du tooencode eller omkoda din mezzaninfil (källa) till en uppsättning med anpassningsbar bithastighet MP4-filer eller Smooth Streaming-filer.  

hello följande kod visar hur toosubmit kodning jobbet. hello jobbet innehåller en uppgift som anger tootranscode hello mezzaninfil till en uppsättning med anpassningsbar bithastighet MP4s **Media Encoder Standard**. hello koden skickar hello jobbet och väntar tills den är klar.

När hello jobbet är slutfört, skulle du vara kan toostream din tillgång eller progressivt hämta MP4-filer som skapades på grund av omkodningen.

Lägg till följande metodklassen toohello programmet hello.

    static public IAsset EncodeToAdaptiveBitrateMP4s(IAsset asset, AssetCreationOptions options)
    {

        // Prepare a job with a single task tootranscode hello specified asset
        // into a multi-bitrate asset.

        IJob job = _context.Jobs.CreateWithSingleTask(
            "Media Encoder Standard",
            "Adaptive Streaming",
            asset,
            "Adaptive Bitrate MP4",
            options);

        Console.WriteLine("Submitting transcoding job...");


        // Submit hello job and wait until it is completed.
        job.Submit();

        job = job.StartExecutionProgressTask(
            j =>
            {
                Console.WriteLine("Job state: {0}", j.State);
                Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
            },
            CancellationToken.None).Result;

        Console.WriteLine("Transcoding job finished.");

        IAsset outputAsset = job.OutputMediaAssets[0];

        return outputAsset;
    }

## <a name="publish-hello-asset-and-get-urls-for-streaming-and-progressive-download"></a>Publicera hello tillgången och få URL: er för strömning och progressiv hämtning

toostream eller hämta en tillgång, du behöver för ”publicera” den genom att skapa en positionerare. Positionerare ger åtkomst toofiles i hello tillgången. Media Services stöder två typer av positionerare: OnDemandOrigin-positionerare som används toostream media (till exempel MPEG DASH, HLS eller Smooth Streaming) och signatur åtkomst (SAS)-positionerare används toodownload mediefiler (för mer information om SAS-positionerare finns [detta](http://southworks.com/blog/2015/05/27/reusing-azure-media-services-locators-to-avoid-facing-the-5-shared-access-policy-limitation/) blogg).

### <a name="some-details-about-url-formats"></a>Information om URL-format

Du kan skapa hello URL: er som skulle vara används toostream eller hämta dina filer när du har skapat hello lokaliserare. hello-exempel i den här självstudiekursen kommer utdata URL: er som du kan klistra in i lämplig webbläsare. Det här avsnittet ger bara korta exempel på hur olika format ser ut.

#### <a name="a-streaming-url-for-mpeg-dash-has-hello-following-format"></a>En strömmande URL för MPEG DASH har hello följande format:

{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest**(format=mpd-time-csf)**

#### <a name="a-streaming-url-for-hls-has-hello-following-format"></a>En strömmande URL för HLS har hello följande format:

{namn på slutpunkt för direktuppspelning-namn på mediaservicekonto}.streaming.mediaservices.windows.net/{lokalisator-ID}/{filnamn}.ism/Manifest**(format=m3u8-aapl)**

#### <a name="a-streaming-url-for-smooth-streaming-has-hello-following-format"></a>En strömmande URL för Smooth Streaming har hello följande format:

{namn på slutpunkt för direktuppspelning-namn på mediaservicekonto}.streaming.mediaservices.windows.net/lokalisator-ID}/{filnamn}.ism/Manifest


#### <a name="a-sas-url-used-toodownload-files-has-hello-following-format"></a>En SAS-URL som används för toodownload filer har hello följande format:

{blob-behållarnamn}/{tillgångsnamn}/{filnamn}/{SAS-signatur}

Media Services .NET SDK-tillägg ger praktiska hjälpmetoder som returnerar formaterade URL: er för hello publicerade tillgången.

hello följande kod använder .NET SDK-tilläggen toocreate positionerare och tooget strömning och progressiv överföring URL: er. hello koden visar även hur toodownload filer tooa lokal mapp.

Lägg till följande metodklassen toohello programmet hello.

    static public void PublishAssetGetURLs(IAsset asset)
    {
        // Publish hello output asset by creating an Origin locator for adaptive streaming,
        // and a SAS locator for progressive download.

        _context.Locators.Create(
            LocatorType.OnDemandOrigin,
            asset,
            AccessPermissions.Read,
            TimeSpan.FromDays(30));

        _context.Locators.Create(
            LocatorType.Sas,
            asset,
            AccessPermissions.Read,
            TimeSpan.FromDays(30));


        IEnumerable<IAssetFile> mp4AssetFiles = asset
                .AssetFiles
                .ToList()
                .Where(af => af.Name.EndsWith(".mp4", StringComparison.OrdinalIgnoreCase));

        // Get hello Smooth Streaming, HLS and MPEG-DASH URLs for adaptive streaming,
        // and hello Progressive Download URL.
        Uri smoothStreamingUri = asset.GetSmoothStreamingUri();
        Uri hlsUri = asset.GetHlsUri();
        Uri mpegDashUri = asset.GetMpegDashUri();

        // Get hello URls for progressive download for each MP4 file that was generated as a result
        // of encoding.
        List<Uri> mp4ProgressiveDownloadUris = mp4AssetFiles.Select(af => af.GetSasUri()).ToList();


        // Display  hello streaming URLs.
        Console.WriteLine("Use hello following URLs for adaptive streaming: ");
        Console.WriteLine(smoothStreamingUri);
        Console.WriteLine(hlsUri);
        Console.WriteLine(mpegDashUri);
        Console.WriteLine();

        // Display hello URLs for progressive download.
        Console.WriteLine("Use hello following URLs for progressive download.");
        mp4ProgressiveDownloadUris.ForEach(uri => Console.WriteLine(uri + "\n"));
        Console.WriteLine();

        // Download hello output asset tooa local folder.
        string outputFolder = "job-output";
        if (!Directory.Exists(outputFolder))
        {
            Directory.CreateDirectory(outputFolder);
        }

        Console.WriteLine();
        Console.WriteLine("Downloading output asset files tooa local folder...");
        asset.DownloadToFolder(
            outputFolder,
            (af, p) =>
            {
                Console.WriteLine("Downloading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });

        Console.WriteLine("Output asset files available at '{0}'.", Path.GetFullPath(outputFolder));
    }

## <a name="test-by-playing-your-content"></a>Testa genom att spela upp ditt innehåll

När du kör hello-programmet som definieras i föregående avsnitt i hello hello URL: er liknande toohello följande visas i hello konsolfönstret.

URL:er för anpassningsbar strömning:

Smooth Streaming

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest

HLS

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=m3u8-aapl)

MPEG DASH

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=mpd-time-csf)

URL:er för progressiv nedladdning (ljud och video).

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_56kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z


toostream videon, klistra in URL: en i hello URL: en textruta i hello [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).

tootest progressiv hämtning, klistra in en URL i en webbläsare (till exempel Internet Explorer, Chrome eller Safari).

Mer information finns i följande avsnitt hello:

- [Spela upp ditt innehåll med befintliga spelare](media-services-playback-content-with-existing-players.md)
- [Utveckla videospelarprogram](media-services-develop-video-players.md)
- [Bädda in MPEG-DASH-anpassad direktuppspelad video i ett HTML5-program med DASH.js](media-services-embed-mpeg-dash-in-html5.md)

## <a name="download-sample"></a>Hämta exempel
hello följande kodexempel innehåller hello kod som du skapade i den här kursen: [exempel](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).

## <a name="next-steps"></a>Nästa steg

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



<!-- Anchors. -->


<!-- URLs. -->
[Web Platform Installer]: http://go.microsoft.com/fwlink/?linkid=255386
[Portal]: http://manage.windowsazure.com/
