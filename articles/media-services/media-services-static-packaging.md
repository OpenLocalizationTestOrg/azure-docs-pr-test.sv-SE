---
title: aaaUsing Azure Media Paketeraren tooaccomplish statiska paketering uppgifter | Microsoft Docs
description: "Det här avsnittet visas olika aktiviteter som utförs med Azure Media Paketeraren."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 0582628e-a525-4a78-90ac-9f7fc1cd909f
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/17/2017
ms.author: juliako
ms.openlocfilehash: 64e8ba217bcd3074f5819ac3b74d2969432db5d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-media-packager-tooaccomplish-static-packaging-tasks"></a>Med Azure Media Paketeraren tooaccomplish statiska paketering uppgifter
> [!NOTE]
> hello slutet av livslängd datum för Microsoft Azure Media Paketeraren och Microsoft Azure Media Krypterare har utökats tooMarch 1, 2017. Före detta datum läggs hello funktionerna i de här processorerna tooMedia Encoder Standard (MES). Kunder som levereras med instruktioner om hur toomigrate tooMES sina arbetsflöden toosend jobb. Funktioner för konvertering och kryptering av format kan också vara tillgängliga via dynamisk paketering och dynamisk kryptering.
> 
> 

## <a name="overview"></a>Översikt
I ordning toodeliver digital video via hello hello internet måste du komprimera mediet. Digitala videofiler är ganska stor och kan vara för stort toodeliver över hello internet eller för dina kunders enheter toodisplay korrekt. Kodning är hello process för att komprimera video och ljud så att kunderna kan visa dina media. När en video som har kodats kan den placeras på olika behållare. hello kallas placerar kodade mediet till en behållare för paketering. Du kan till exempel ta en MP4-fil och omvandla dem till Smooth Streaming eller HLS innehåll med hjälp av hello Azure Media Paketeraren. 

Media Services stöder dynamiska och statiska paketering. När du använder statisk paketering måste toocreate en kopia av innehållet i varje format som krävs av dina kunder. Med dynamisk paketering behöver du är toocreate en tillgång som innehåller en uppsättning MP4 eller Smooth Streaming-filer med anpassningsbar bithastighet. Sedan, baserat på hello format som anges i manifestet hello eller Fragmentera begäran, hello strömning på begäran kommer servern att säkerställa att användarna får hello dataström i hello-protokollet som de har valt. Därför behöver du bara toostore och betala för hello filer i ett enda lagringsformat och Media Services-tjänsten skapar och ger hello lämplig respons baserat på begäranden från en klient.

> [!NOTE]
> Det rekommenderas toouse [dynamisk paketering](media-services-dynamic-packaging-overview.md).
> 
> 

Det finns dock vissa scenarier som kräver statiska paketering: 

* Verifiera anpassningsbar bithastighet MP4s kodad med externa kodare (till exempel med hjälp av kodare från tredje part).

Du kan också använda statiska paketering tooperform hello följande uppgifter. Men det rekommenderas att toouse dynamisk kryptering.

* Med statiska kryptering tooprotect Smooth och MPEG DASH med PlayReady
* Med AES-128 statiska kryptering tooprotect HLSv3
* Med hjälp av statiska kryptering tooprotect HLSv3 med PlayReady

## <a name="validating-adaptive-bitrate-mp4s-encoded-with-external-encoders"></a>Validera anpassningsbar bithastighet MP4s kodad med externa kodare
Om du vill toouse en uppsättning med anpassningsbar bithastighet (multibithastighet) MP4-filer som inte kodats med Media Services kodare, bör du verifiera dina filer innan du vidare bearbetning. hello Media Services Paketeraren kan verifiera en tillgång som innehåller en uppsättning MP4-filer och kontrollera om hello tillgång kan vara paketerade tooSmooth Streaming eller HLS. Om hello verifieringen misslyckas, Slutför hello jobb som bearbetar hello aktivitet med ett fel. hello XML som definierar hello förinställningen för hello validering aktivitet i hello [förinställda aktivitet för Azure Media Paketeraren](http://msdn.microsoft.com/library/azure/hh973635.aspx) avsnittet.

> [!NOTE]
> Använd hello Media Encoder Standard tooproduce eller hello Media Services Paketeraren toovalidate ditt innehåll i ordning tooavoid runtime problem. Om servern för strömning på begäran av hello inte kan tooparse källfiler vid körning, felmeddelande HTTP 1.1 ”415 Media typ”. Flera gånger orsakar hello server toofail tooparse källfilerna påverkar hello strömning på begäran serverns prestanda och kan minska hello bandbredd tillgänglig tooserving andra begäranden. Azure Media Services erbjuder en serviceavtalet (SLA) för dess strömning på begäran-tjänster. Detta serviceavtal kan inte användas om hello server felanvändning hello sätt som beskrivs ovan.
> 
> 

Det här avsnittet visar hur tooprocess hello validering aktivitet. Den visar även hur toosee hello status och hello felmeddelande för hello jobb som har slutförts med JobStatus.Error.

toovalidate dina MP4-filer med Media Services Paketeraren, måste du skapa en egen manifestet (.ism)-fil och överför den tillsammans med hello källfiler i hello Media Services-konto. Nedan visas ett exempel på hello .ism-fil produceras av hello Media Encoder Standard. hello filnamn är skiftlägeskänsliga. Kontrollera också att kodas hello texten i hello .ism-fil med UTF-8.

    <?xml version="1.0" encoding="utf-8" standalone="yes"?>
    <smil xmlns="http://www.w3.org/2001/SMIL20/Language">
      <head>
    <!-- Tells hello server that these input files are MP4s – specific tooDynamic Packaging -->
        <meta name="formats" content="mp4" /> 
      </head>
      <body>
        <switch>
          <video src="BigBuckBunny_1000.mp4" />
          <video src="BigBuckBunny_1500.mp4" />
          <video src="BigBuckBunny_2250.mp4" />
          <video src="BigBuckBunny_3400.mp4" />
          <video src="BigBuckBunny_400.mp4" />
          <video src="BigBuckBunny_650.mp4" />
          <audio src="BigBuckBunny_400.mp4" />
        </switch>
      </body>
    </smil>

När du har kan hello anpassningsbar bithastighet MP4-uppsättningen du dra nytta av dynamisk paketering. Dynamisk paketering kan du toodeliver dataströmmar i hello angivna protokollet utan ytterligare paket. Mer information finns i [dynamisk paketering](media-services-dynamic-packaging-overview.md).

hello följande kod exempel använder Azure Media Services .NET SDK-tilläggen.  Se till att tooupdate hello kod toopoint toohello mapp inkommande MP4-filer och .ism-fil finns. Och även toowhere MediaPackager_ValidateTask.xml-filen finns. XML-filen har definierats i [aktivitet förinställda för Azure Media Paketeraren](http://msdn.microsoft.com/library/azure/hh973635.aspx) avsnittet.

    using Microsoft.WindowsAzure.MediaServices.Client;
    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using System.Xml.Linq;

    namespace MediaServicesStaticPackaging
    {
        class Program
        {
            private static readonly string _mediaFiles =
                Path.GetFullPath(@"../..\Media");

            // hello MultibitrateMP4Files folder should also
            // contain hello .ism manifest file.
            private static readonly string _multibitrateMP4s =
                Path.Combine(_mediaFiles, @"MultibitrateMP4Files");

            // XML Configruation files path.
            private static readonly string _configurationXMLFiles = @"../..\Configurations";

            private static MediaServicesCredentials _cachedCredentials = null;
            private static CloudMediaContext _context = null;

            // Media Services account information.
            private static readonly string _mediaServicesAccountName =
                ConfigurationManager.AppSettings["MediaServicesAccountName"];
            private static readonly string _mediaServicesAccountKey =
                ConfigurationManager.AppSettings["MediaServicesAccountKey"];

            static void Main(string[] args)
            {
                // Create and cache hello Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
                // Use hello cached credentials toocreate CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);

                // Ingest a set of multibitrate MP4s.
                //
                // Use hello SDK extension method toocreate a new asset by 
                // uploading files from a local directory.
                IAsset multibitrateMP4sAsset = _context.Assets.CreateFromFolder(
                    _multibitrateMP4s,
                    AssetCreationOptions.None,
                    (af, p) =>
                    {
                        Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
                    });

                // Use Azure Media Packager toovalidate hello files.
                IAsset validatedMP4s =
                    ValidateMultibitrateMP4s(multibitrateMP4sAsset);

                // Publish hello asset.
                _context.Locators.Create(
                    LocatorType.OnDemandOrigin,
                    validatedMP4s,
                    AccessPermissions.Read,
                    TimeSpan.FromDays(30));

                                     // Get hello streaming URLs.
                Console.WriteLine("Smooth Streaming URL:");
                Console.WriteLine(validatedMP4s.GetSmoothStreamingUri().ToString());
                Console.WriteLine("MPEG DASH URL:");
                Console.WriteLine(validatedMP4s.GetMpegDashUri().ToString());
                Console.WriteLine("HLS URL:");
                Console.WriteLine(validatedMP4s.GetHlsUri().ToString());
            }

            public static IAsset ValidateMultibitrateMP4s(IAsset multibitrateMP4sAsset)
            {
                // Set .ism as a primary file 
                // in a multibitrate MP4 set.
                SetISMFileAsPrimary(multibitrateMP4sAsset);

                // Create a new job.
                IJob job = _context.Jobs.Create("MP4 validation and converstion tooSmooth Stream job.");

                // Read hello task configuration data into a string. 
                string configMp4Validation = File.ReadAllText(Path.Combine(
                        _configurationXMLFiles,
                        "MediaPackager_ValidateTask.xml"));

                // Get hello SDK extension method too get a reference toohello Azure Media Packager.
                IMediaProcessor processor = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.WindowsAzureMediaPackager);

                // Create a task with hello conversion details, using hello configuration data. 
                ITask task = job.Tasks.AddNew("Mp4 Validation Task",
                    processor,
                    configMp4Validation,
                    TaskOptions.None);

                // Specify hello input asset toobe validated.
                task.InputAssets.Add(multibitrateMP4sAsset);

                // Add an output asset toocontain hello results of hello job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means hello output asset is in hello clear (unencrypted). 
                task.OutputAssets.AddNew("Validated output asset",
                        AssetCreationOptions.None);

                // Submit hello job and wait until it is completed.
                job.Submit();
                job = job.StartExecutionProgressTask(
                    j =>
                    {
                        Console.WriteLine("Job state: {0}", j.State);
                        Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
                    },
                    CancellationToken.None).Result;

                // If hello validation task fails and job completes with JobState.Error,
                // display hello error message and throw an exception.
                if (job.State == JobState.Error)
                {
                    Console.WriteLine("  Job ID: " + job.Id);
                    Console.WriteLine("  Name: " + job.Name);
                    Console.WriteLine("  State: " + job.State);

                    foreach (var jobTask in job.Tasks)
                    {
                        Console.WriteLine("  Task Id: " + jobTask.Id);
                        Console.WriteLine("  Name: " + jobTask.Name);
                        Console.WriteLine("  Progress: " + jobTask.Progress);
                        Console.WriteLine("  Configuration: " + jobTask.Configuration);
                        Console.WriteLine("  Running time: " + jobTask.RunningDuration);
                        if (jobTask.ErrorDetails != null)
                        {
                            foreach (var errordetail in jobTask.ErrorDetails)
                            {

                                Console.WriteLine("  Error Message:" + errordetail.Message);
                                Console.WriteLine("  Error Code:" + errordetail.Code);
                            }
                        }
                    }
                    throw new Exception("hello specified multi-bitrate MP4 set is not valid.");
                }


                return job.OutputMediaAssets[0];
            }

            static void SetISMFileAsPrimary(IAsset asset)
            {
                var ismAssetFiles = asset.AssetFiles.ToList().
                    Where(f => f.Name.EndsWith(".ism", StringComparison.OrdinalIgnoreCase)).ToArray();

                // hello following code assigns hello first .ism file as hello primary file in hello asset.
                // An asset should have one .ism file.  
                ismAssetFiles.First().IsPrimary = true;
                ismAssetFiles.First().Update();
            }
        }
    }

## <a name="using-static-encryption-tooprotect-your-smooth-and-mpeg-dash-with-playready"></a>Med statiska kryptering tooProtect Smooth och MPEG DASH med PlayReady
Om du vill tooprotect ditt innehåll med PlayReady, du kan välja att använda [dynamisk kryptering](media-services-protect-with-drm.md) (hello rekommenderas) eller statisk kryptering (enligt beskrivningen i det här avsnittet).

hello exemplet i det här avsnittet kodar en mezzaninfil (i det här fallet MP4) till anpassningsbar bithastighet MP4-filer. Den sedan paket MP4s till Smooth Streaming och krypterar Smooth Streaming med PlayReady. Du kan därför kan toostream Smooth Streaming eller MPEG DASH.

Media Services är nu en tjänst för att leverera Microsoft PlayReady-licenser. hello exempel i den här artikeln visar hur tooconfigure hello Media Services PlayReady licens delivery service (se hello ConfigureLicenseDeliveryService metod som definieras i hello koden nedan). Läs mer om Media Services Playreadys licensleveranstjänst [med PlayReady dynamisk kryptering och Licensleveranstjänst](media-services-protect-with-drm.md).

> [!NOTE]
> toodeliver MPEG DASH krypterat med PlayReady, göra att toouse CENC alternativ genom att ange hello useSencBox och adjustSubSamples egenskaper (beskrivs i hello [aktivitet förinställda för Azure Media Krypterare](http://msdn.microsoft.com/library/azure/hh973610.aspx) avsnittet) tootrue.  
> 
> 

Se till att tooupdate hello följande kod toopoint toohello mapp där MP4 indatafilen finns.

Och även toowhere MediaPackager_MP4ToSmooth.xml och MediaEncryptor_PlayReadyProtection.xml filerna finns. MediaPackager_MP4ToSmooth.xml har definierats i [aktivitet förinställda för Azure Media Paketeraren](http://msdn.microsoft.com/library/azure/hh973635.aspx) och MediaEncryptor_PlayReadyProtection.xml har definierats i hello [aktivitet förinställda för Azure Media Krypterare](http://msdn.microsoft.com/library/azure/hh973610.aspx) avsnittet. 

hello exempel definierar hello UpdatePlayReadyConfigurationXMLFile metod som du kan använda toodynamically uppdateringsfil hello MediaEncryptor_PlayReadyProtection.xml. Du kan använda hello CommonEncryption.GeneratePlayReadyContentKey metoden toogenerate hello innehållsnyckeln baserat på hello keySeedValue och KeyId värden om du har hello viktiga startvärde som är tillgängliga.

    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Xml.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
    using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;

    namespace PlayReadyStaticEncryptAndKeyDeliverySvc
    {
        class Program
        {

            private static readonly string _mediaFiles =
                Path.GetFullPath(@"../..\Media");

            private static readonly string _singleMP4File =
                Path.Combine(_mediaFiles, @"BigBuckBunny.mp4");

            // XML Configruation files path.
            private static readonly string _configurationXMLFiles = @"../..\Configurations\";


            private static MediaServicesCredentials _cachedCredentials = null;
            private static CloudMediaContext _context = null;

            // Media Services account information.
            private static readonly string _mediaServicesAccountName =
                ConfigurationManager.AppSettings["MediaServiceAccountName"];
            private static readonly string _mediaServicesAccountKey =
                ConfigurationManager.AppSettings["MediaServiceAccountKey"];

            static void Main(string[] args)
            {
                // Create and cache hello Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
                // Use hello cached credentials toocreate CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);

                // Encoding and encrypting assets //////////////////////
                // Load a single MP4 file.
                IAsset asset = IngestSingleMP4File(_singleMP4File, AssetCreationOptions.None);

                // Encode an MP4 file tooa set of multibitrate MP4s.
                // Then, package a set of MP4s tooclear Smooth Streaming.
                IAsset clearSmoothStreamAsset =
                    ConvertMP4ToMultibitrateMP4sToSmoothStreaming(asset);

                // Create a common encryption content key that is used 
                // a) tooset hello key values in hello MediaEncryptor_PlayReadyProtection.xml file
                //    that is used for encryption.
                // b) tooconfigure hello license delivery service and 
                //
                Guid keyId;
                byte[] contentKey;

                IContentKey key = CreateCommonEncryptionKey(out keyId, out contentKey);

                // hello content key authorization policy must be configured by you 
                // and met by hello client in order for hello PlayReady license
                // toobe delivered toohello client. 
                // In this example hello Media Services PlayReady license delivery service is used.
                ConfigureLicenseDeliveryService(key);

                // Get hello Media Services PlayReady license delivery URL.
                // This URL will be assigned toohello licenseAcquisitionUrl property 
                // of hello MediaEncryptor_PlayReadyProtection.xml file.
                Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);

                // Update hello MediaEncryptor_PlayReadyProtection.xml file with hello key and URL info.
                UpdatePlayReadyConfigurationXMLFile(keyId, contentKey, acquisitionUrl);


                // Encrypt your clear Smooth Streaming tooSmooth Streaming with PlayReady.
                IAsset outputAsset = CreateSmoothStreamEncryptedWithPlayReady(clearSmoothStreamAsset);


                // You can use hello http://smf.cloudapp.net/healthmonitor player 
                // tootest hello smoothStreamURL URL.
                string smoothStreamURL = outputAsset.GetSmoothStreamingUri().ToString();
                Console.WriteLine("Smooth Streaming URL:");
                Console.WriteLine(smoothStreamURL);

                // You can use hello http://dashif.org/reference/players/javascript/ player 
                // tootest hello dashURL URL.
                string dashURL = outputAsset.GetMpegDashUri().ToString();
                Console.WriteLine("MPEG DASH URL:");
                Console.WriteLine(dashURL);
            }

            /// <summary>
            /// Creates a job with 2 tasks: 
            /// 1 task - encodes a single MP4 toomultibitrate MP4s,
            /// 2 task - packages MP4s tooSmooth Streaming.
            /// </summary>
            /// <returns>hello output asset.</returns>
            public static IAsset ConvertMP4ToMultibitrateMP4sToSmoothStreaming(IAsset asset)
            {
                // Create a new job.
                IJob job = _context.Jobs.Create("Convert MP4 tooSmooth Streaming.");

                // Add task 1 - Encode single MP4 into multibitrate MP4s.
                IAsset MP4sAsset = EncodeMP4IntoMultibitrateMP4sTask(job, asset);
                // Add task 2 - Package a multibitrate MP4 set tooClear Smooth Stream.
                IAsset packagedAsset = PackageMP4ToSmoothStreamingTask(job, MP4sAsset);

                // Submit hello job and wait until it is completed.
                job.Submit();
                job = job.StartExecutionProgressTask(
                    j =>
                    {
                        Console.WriteLine("Job state: {0}", j.State);
                        Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
                    },
                    CancellationToken.None).Result;

                // Get hello output asset that contains hello Smooth Streaming asset.
                return job.OutputMediaAssets[1];
            }

            /// <summary>
            /// Encrypts Smooth Stream with PlayReady.
            /// Then creates a Smooth Streaming Url.
            /// </summary>
            /// <param name="clearSmoothAsset">Asset that contains clear Smooth Streaming.</param>
            /// <returns>hello output asset.</returns>
            public static IAsset CreateSmoothStreamEncryptedWithPlayReady(IAsset clearSmoothStreamAsset)
            {
                // Create a job.
                IJob job = _context.Jobs.Create("Encrypt tooPlayReady Smooth Streaming.");

                // Add task 1 - Encrypt Smooth Streaming with PlayReady 
                IAsset encryptedSmoothAsset =
                    EncryptSmoothStreamWithPlayReadyTask(job, clearSmoothStreamAsset);

                // Submit hello job and wait until it is completed.
                job.Submit();
                job = job.StartExecutionProgressTask(
                    j =>
                    {
                        Console.WriteLine("Job state: {0}", j.State);
                        Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
                    },
                    CancellationToken.None).Result;

                // hello OutputMediaAssets[0] contains hello desired asset.
                _context.Locators.Create(
                    LocatorType.OnDemandOrigin,
                    job.OutputMediaAssets[0],
                    AccessPermissions.Read,
                    TimeSpan.FromDays(30));

                return job.OutputMediaAssets[0];
            }

            /// <summary>
            /// Create a common encryption content key that is used 
            /// tooset hello key values in hello MediaEncryptor_PlayReadyProtection.xml file
            /// that is used for encryption.
            /// </summary>
            /// <param name="keyId"></param>
            /// <param name="contentKey"></param>
            /// <returns></returns>
            public static IContentKey CreateCommonEncryptionKey(out Guid keyId, out byte[] contentKey)
            {
                keyId = Guid.NewGuid();
                contentKey = GetRandomBuffer(16);

                IContentKey key = _context.ContentKeys.Create(
                                        keyId,
                                        contentKey,
                                        "ContentKey",
                                        ContentKeyType.CommonEncryption);

                return key;
            }

            /// <summary>
            /// Update your configuration .xml file dynamically.
            /// </summary>
            public static void UpdatePlayReadyConfigurationXMLFile(Guid keyId, byte[] keyValue, Uri licenseAcquisitionUrl)
            {
                string xmlFileName = Path.Combine(_configurationXMLFiles,
                                            @"MediaEncryptor_PlayReadyProtection.xml");

                XNamespace xmlns = "http://schemas.microsoft.com/iis/media/v4/TM/TaskDefinition#";

                // Prepare hello encryption task template
                XDocument doc = XDocument.Load(xmlFileName);

                var licenseAcquisitionUrlEl = doc
                        .Descendants(xmlns + "property")
                        .Where(p => p.Attribute("name").Value == "licenseAcquisitionUrl")
                        .FirstOrDefault();
                var contentKeyEl = doc
                        .Descendants(xmlns + "property")
                        .Where(p => p.Attribute("name").Value == "contentKey")
                        .FirstOrDefault();
                var keyIdEl = doc
                        .Descendants(xmlns + "property")
                        .Where(p => p.Attribute("name").Value == "keyId")
                        .FirstOrDefault();

                // Update hello "value" property.
                if (licenseAcquisitionUrlEl != null)
                    licenseAcquisitionUrlEl.Attribute("value").SetValue(licenseAcquisitionUrl.ToString());

                if (contentKeyEl != null)
                    contentKeyEl.Attribute("value").SetValue(Convert.ToBase64String(keyValue));

                if (keyIdEl != null)
                    keyIdEl.Attribute("value").SetValue(keyId);

                doc.Save(xmlFileName);
            }

            /// <summary>
            /// Uploads a single file.
            /// </summary>
            /// <param name="fileDir">hello location of hello files.</param>
            /// <param name="assetCreationOptions">
            ///  You can specify hello following encryption options for hello AssetCreationOptions.
            ///      None:  no encryption.  
            ///      StorageEncrypted: storage encryption. Encrypts a clear input file 
            ///        before it is uploaded tooAzure storage. 
            ///      CommonEncryptionProtected: for Common Encryption Protected (CENC) files. 
            ///        For example, a set of files that are already PlayReady encrypted. 
            ///      EnvelopeEncryptionProtected: for HLS with AES encryption files.
            ///        NOTE: hello files must have been encoded and encrypted by Transform Manager. 
            ///     </param>
            /// <returns>Returns an asset that contains a single file.</returns>
            /// </summary>
            /// <returns></returns>
            private static IAsset IngestSingleMP4File(string fileDir, AssetCreationOptions assetCreationOptions)
            {
                // Use hello SDK extension method toocreate a new asset by 
                // uploading a mezzanine file from a local path.
                IAsset asset = _context.Assets.CreateFromFile(
                    fileDir,
                    assetCreationOptions,
                    (af, p) =>
                    {
                        Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
                    });

                return asset;
            }

            /// <summary>
            /// Creates a task tooencode tooAdaptive Bitrate. 
            /// Adds hello new task tooa job.
            /// </summary>
            /// <param name="job">hello job toowhich tooadd hello new task.</param>
            /// <param name="asset">hello input asset.</param>
            /// <returns>hello output asset.</returns>
            private static IAsset EncodeMP4IntoMultibitrateMP4sTask(IJob job, IAsset asset)
            {
                // Get hello SDK extension method too get a reference toohello Media Encoder Standard.
                IMediaProcessor encoder = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.MediaEncoderStandard);

                ITask adpativeBitrateTask = job.Tasks.AddNew("MP4 tooAdaptive Bitrate Task",
                   encoder,
                   "Adaptive Streaming",
                   TaskOptions.None);

                // Specify hello input Asset
                adpativeBitrateTask.InputAssets.Add(asset);

                // Add an output asset toocontain hello results of hello job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means hello output asset is in hello clear (unencrypted).
                IAsset abrAsset = adpativeBitrateTask.OutputAssets.AddNew("Multibitrate MP4s",
                                        AssetCreationOptions.None);

                return abrAsset;
            }

            /// <summary>
            /// Creates a task tooconvert hello MP4 file(s) tooa Smooth Streaming asset.
            /// Adds hello new task tooa job.
            /// </summary>
            /// <param name="job">hello job toowhich tooadd hello new task.</param>
            /// <param name="asset">hello input asset.</param>
            /// <returns>hello output asset.</returns>
            private static IAsset PackageMP4ToSmoothStreamingTask(IJob job, IAsset asset)
            {
                // Get hello SDK extension method too get a reference toohello Azure Media Packager.
                IMediaProcessor packager = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.WindowsAzureMediaPackager);

                // Azure Media Packager does not accept string presets, so load xml configuration
                string smoothConfig = File.ReadAllText(Path.Combine(
                            _configurationXMLFiles,
                            "MediaPackager_MP4toSmooth.xml"));

                // Create a new Task tooconvert adaptive bitrate tooSmooth Streaming.
                ITask smoothStreamingTask = job.Tasks.AddNew("MP4 tooSmooth Task",
                   packager,
                   smoothConfig,
                   TaskOptions.None);

                // Specify hello input Asset, which is hello output Asset from hello first task
                smoothStreamingTask.InputAssets.Add(asset);

                // Add an output asset toocontain hello results of hello job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means hello output asset is in hello clear (unencrypted).
                IAsset smoothOutputAsset =
                    smoothStreamingTask.OutputAssets.AddNew("Clear Smooth Stream",
                        AssetCreationOptions.None);

                return smoothOutputAsset;
            }


            /// <summary>
            /// Creates a task tooencrypt Smooth Streaming with PlayReady.
            /// Note: toodeliver DASH, make sure tooset hello useSencBox and adjustSubSamples 
            /// configuration properties tootrue. 
            /// In this example, MediaEncryptor_PlayReadyProtection.xml contains configuration.
            /// </summary>
            /// <param name="job">hello job toowhich tooadd hello new task.</param>
            /// <param name="asset">hello input asset.</param>
            /// <returns>hello output asset.</returns>
            private static IAsset EncryptSmoothStreamWithPlayReadyTask(IJob job, IAsset asset)
            {
                // Get hello SDK extension method too get a reference toohello Azure Media Encryptor.
                IMediaProcessor playreadyProcessor = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.WindowsAzureMediaEncryptor);

                // Read hello configuration XML.
                //
                // Note that hello configuration defined in MediaEncryptor_PlayReadyProtection.xml
                // is using keySeedValue. It is recommended that you do this only for testing 
                // and not in production. For more information, see 
                // http://msdn.microsoft.com/library/windowsazure/dn189154.aspx.
                //
                string configPlayReady = File.ReadAllText(Path.Combine(_configurationXMLFiles,
                                            @"MediaEncryptor_PlayReadyProtection.xml"));

                ITask playreadyTask = job.Tasks.AddNew("My PlayReady Task",
                   playreadyProcessor,
                   configPlayReady,
                   TaskOptions.ProtectedConfiguration);

                playreadyTask.InputAssets.Add(asset);

                // Add an output asset toocontain hello results of hello job. 
                // This output is specified as AssetCreationOptions.CommonEncryptionProtected.
                IAsset playreadyAsset = playreadyTask.OutputAssets.AddNew(
                                                "PlayReady Smooth Streaming",
                                                AssetCreationOptions.CommonEncryptionProtected);

                return playreadyAsset;
            }

            /// <summary>
            /// Configures authorization policy for hello content key. 
            /// </summary>
            /// <param name="contentKey">hello content key.</param>
            static public void ConfigureLicenseDeliveryService(IContentKey contentKey)
            {
                // Create ContentKeyAuthorizationPolicy with Open restrictions 
                // and create authorization policy          

                List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
                {
                    new ContentKeyAuthorizationPolicyRestriction 
                    { 
                        Name = "Open", 
                        KeyRestrictionType = (int)ContentKeyRestrictionType.Open, 
                        Requirements = null
                    }
                };

                // Configure PlayReady license template.
                string newLicenseTemplate = ConfigurePlayReadyLicenseTemplate();

                IContentKeyAuthorizationPolicyOption policyOption =
                    _context.ContentKeyAuthorizationPolicyOptions.Create("",
                        ContentKeyDeliveryType.PlayReadyLicense,
                            restrictions, newLicenseTemplate);

                IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                            ContentKeyAuthorizationPolicies.
                            CreateAsync("Deliver Common Content Key with no restrictions").
                            Result;


                contentKeyAuthorizationPolicy.Options.Add(policyOption);

                // Associate hello content key authorization policy with hello content key.
                contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
                contentKey = contentKey.UpdateAsync().Result;
            }

            static private string ConfigurePlayReadyLicenseTemplate()
            {
                // hello following code configures PlayReady License Template using .NET classes
                // and returns hello XML string.

                PlayReadyLicenseResponseTemplate responseTemplate = new PlayReadyLicenseResponseTemplate();
                PlayReadyLicenseTemplate licenseTemplate = new PlayReadyLicenseTemplate();

                responseTemplate.LicenseTemplates.Add(licenseTemplate);

                return MediaServicesLicenseTemplateSerializer.Serialize(responseTemplate);
            }

            static private byte[] GetRandomBuffer(int length)
            {
                var returnValue = new byte[length];

                using (var rng =
                    new System.Security.Cryptography.RNGCryptoServiceProvider())
                {
                    rng.GetBytes(returnValue);
                }

                return returnValue;
            }
        }
    }

## <a name="using-static-encryption-tooprotect-hlsv3-with-aes-128"></a>Med AES-128 statiska kryptering tooProtect HLSv3
Om du vill tooencrypt din HLS med AES-128, du kan välja att använda dynamisk kryptering (hello rekommenderas) eller statisk kryptering (som visas i det här avsnittet). Om du väljer toouse dynamisk kryptering, se [med hjälp av dynamisk AES-128-kryptering och leverans av tjänsten](media-services-protect-with-aes128.md).

> [!NOTE]
> I order tooconvert ditt innehåll till HLS, du måste först konvertera/koda ditt innehåll till Smooth Streaming.
> Tooget som krypterats med AES också hello HLS göra att tooset hello följande egenskaper i filen MediaPackager_SmoothToHLS.xml: Ange hello kryptera egenskapen tootrue och ange hello nyckelvärde hello keyuri värdet toopoint tooyour authentication\ server för auktorisering.
> Media Services skapar en nyckelfil och placera den i hello tillgången behållaren. Du bör kopiera hello /asset-containerguid/*.key tooyour filserver (eller skapa en egen nyckel fil) och ta sedan bort hello *.key filen från hello tillgången behållare.
> 
> 

hello exemplet i det här avsnittet kodar en mezzaninfil (i det här fallet MP4) till multibitrate MP4-filer och sedan paket MP4s till Smooth Streaming. Den paketerar sedan Smooth Streaming i HTTP Live Streaming (HLS) krypteras med Advanced Encryption Standard (AES) dataströmmen 128-bitars kryptering. Se till att tooupdate hello följande kod toopoint toohello mapp där MP4 indatafilen finns. Och även toowhere konfigurationsfilerna MediaPackager_MP4ToSmooth.xml och MediaPackager_SmoothToHLS.xml finns. Du hittar hello definition för filerna i hello [aktivitet förinställda för Azure Media Paketeraren](http://msdn.microsoft.com/library/azure/hh973635.aspx) avsnittet.

    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Xml.Linq;

    namespace MediaServicesContentProtection
    {
        class Program
        {
            // Paths toosupport files (within hello above base path). You can use 
            // hello provided sample media files from hello "SupportFiles" folder, or 
            // provide paths tooyour own media files below toorun these samples.

            private static readonly string _mediaFiles =
                Path.GetFullPath(@"../..\Media");

            private static readonly string _singleMP4File =
                Path.Combine(_mediaFiles, @"SingleMP4\BigBuckBunny.mp4");

            // XML Configruation files path.
            private static readonly string _configurationXMLFiles = @"../..\Configurations\";

            private static MediaServicesCredentials _cachedCredentials = null;
            private static CloudMediaContext _context = null;

            // Media Services account information.
            private static readonly string _mediaServicesAccountName = 
                ConfigurationManager.AppSettings["MediaServiceAccountName"];
            private static readonly string _mediaServicesAccountKey = 
                ConfigurationManager.AppSettings["MediaServiceAccountKey"];

            static void Main(string[] args)
            {
                // Create and cache hello Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName, 
                                _mediaServicesAccountKey);
                // Use hello cached credentials toocreate CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);

                // Encoding and encrypting assets //////////////////////

                // Load an MP4 file.
                IAsset asset = IngestSingleMP4File(_singleMP4File, AssetCreationOptions.None);

                // Encode an MP4 file tooa set of multibitrate MP4s.
                // Then, package a set of MP4s tooclear Smooth Streaming.
                IAsset clearSmoothStreamAsset = ConvertMP4ToMultibitrateMP4sToSmoothStreaming(asset);

                // Create HLS encrypted with AES.
                IAsset HLSEncryptedWithAESAsset = CreateHLSEncryptedWithAES(clearSmoothStreamAsset);

                // You can use hello following player tootest hello HLS with AES stream.
                // http://apps.microsoft.com/windows/app/3ivx-hls-player/f79ce7d0-2993-4658-bc4e-83dc182a0614 
                string hlsWithAESURL = HLSEncryptedWithAESAsset.GetHlsUri().ToString();
                Console.WriteLine("HLS with AES URL:");
                Console.WriteLine(hlsWithAESURL);
            }


            /// <summary>
            /// Creates a job with 2 tasks: 
            /// 1 task - encodes a single MP4 toomultibitrate MP4s,
            /// 2 task - packages MP4s tooSmooth Streaming.
            /// </summary>
            /// <returns>hello output asset.</returns>
            public static IAsset ConvertMP4ToMultibitrateMP4sToSmoothStreaming(IAsset asset)
            {
                // Create a new job.
                IJob job = _context.Jobs.Create("Convert MP4 tooSmooth Streaming.");

                // Add task 1 - Encode single MP4 into multibitrate MP4s.
                IAsset MP4sAsset = EncodeSingleMP4IntoMultibitrateMP4sTask(job, asset);
                // Add task 2 - Package a multibitrate MP4 set tooClear Smooth Streaming.
                IAsset packagedAsset = PackageMP4ToSmoothStreamingTask(job, MP4sAsset);

                // Submit hello job and wait until it is completed.
                job.Submit();
                job = job.StartExecutionProgressTask(
                    j =>
                    {
                        Console.WriteLine("Job state: {0}", j.State);
                        Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
                    },
                    CancellationToken.None).Result;

                // Get hello output asset that contains Smooth Streaming.
                return job.OutputMediaAssets[1];
            }

            /// <summary>
            /// Encrypts an HLS with AES-128.
            /// </summary>
            /// <param name="clearSmoothAsset">Asset that contains clear Smooth Streaming.</param>
            /// <returns>hello output asset.</returns>
            public static IAsset CreateHLSEncryptedWithAES(IAsset clearSmoothStreamAsset)
            {
                IJob job = _context.Jobs.Create("Encrypt tooHLS with AES.");

                // Add task 1 - Package clear Smooth Streaming tooHLS with AES.
                PackageSmoothStreamToHLS(job, clearSmoothStreamAsset);

                // Submit hello job and wait until it is completed.
                job.Submit();
                job = job.StartExecutionProgressTask(
                    j =>
                    {
                        Console.WriteLine("Job state: {0}", j.State);
                        Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
                    },
                    CancellationToken.None).Result;

                // hello OutputMediaAssets[0] contains hello desired asset.
                _context.Locators.Create(
                    LocatorType.OnDemandOrigin,
                    job.OutputMediaAssets[0],
                    AccessPermissions.Read,
                    TimeSpan.FromDays(30));

                return job.OutputMediaAssets[0];
            }

            /// <summary>
            /// Uploads a single file.
            /// </summary>
            /// <param name="fileDir">hello location of hello files.</param>
            /// <param name="assetCreationOptions">
            ///  You can specify hello following encryption options for hello AssetCreationOptions.
            ///      None:  no encryption.  
            ///      StorageEncrypted: storage encryption. Encrypts a clear input file 
            ///        before it is uploaded tooAzure storage. 
            ///      CommonEncryptionProtected: for Common Encryption Protected (CENC) files. 
            ///        For example, a set of files that are already PlayReady encrypted. 
            ///      EnvelopeEncryptionProtected: for HLS with AES encryption files.
            ///        NOTE: hello files must have been encoded and encrypted by Transform Manager. 
            ///     </param>
            /// <returns>Returns an asset that contains a single file.</returns>
            /// </summary>
            /// <returns></returns>
            private static IAsset IngestSingleMP4File(string fileDir, AssetCreationOptions assetCreationOptions)
            {
                // Use hello SDK extension method toocreate a new asset by 
                // uploading a mezzanine file from a local path.
                IAsset asset = _context.Assets.CreateFromFile(
                    fileDir,
                    assetCreationOptions,
                    (af, p) =>
                    {
                        Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
                    });

                return asset;
            }

            /// <summary>
            /// Creates a task tooencode tooAdaptive Bitrate. 
            /// Adds hello new task tooa job.
            /// </summary>
            /// <param name="job">hello job toowhich tooadd hello new task.</param>
            /// <param name="asset">hello input asset.</param>
            /// <returns>hello output asset.</returns>
            private static IAsset EncodeSingleMP4IntoMultibitrateMP4sTask(IJob job, IAsset asset)
            {
                // Get hello SDK extension method too get a reference toohello Media Encoder Standard.
                IMediaProcessor encoder = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.MediaEncoderStandard);

                ITask adpativeBitrateTask = job.Tasks.AddNew("MP4 tooAdaptive Bitrate Task",
                   encoder,
                   "Adaptive Streaming",
                   TaskOptions.None);

                // Specify hello input Asset
                adpativeBitrateTask.InputAssets.Add(asset);

                // Add an output asset toocontain hello results of hello job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means hello output asset is in hello clear (unencrypted).
                IAsset abrAsset = adpativeBitrateTask.OutputAssets.AddNew("Multibitrate MP4s", 
                                        AssetCreationOptions.None);

                return abrAsset;
            }

            /// <summary>
            /// Creates a task tooconvert hello MP4 file(s) tooa Smooth Streaming asset.
            /// Adds hello new task tooa job.
            /// </summary>
            /// <param name="job">hello job toowhich tooadd hello new task.</param>
            /// <param name="asset">hello input asset.</param>
            /// <returns>hello output asset.</returns>
            private static IAsset PackageMP4ToSmoothStreamingTask(IJob job, IAsset asset)
            {
                // Get hello SDK extension method too get a reference toohello Azure Media Packager.
                IMediaProcessor packager = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.WindowsAzureMediaPackager);

                // Azure Media Packager does not accept string presets, so load xml configuration
                string smoothConfig = File.ReadAllText(Path.Combine(
                            _configurationXMLFiles, 
                            "MediaPackager_MP4toSmooth.xml"));

                // Create a new Task tooconvert adaptive bitrate tooSmooth Streaming.
                ITask smoothStreamingTask = job.Tasks.AddNew("MP4 tooSmooth Task",
                   packager,
                   smoothConfig,
                   TaskOptions.None);

                // Specify hello input Asset, which is hello output Asset from hello first task
                smoothStreamingTask.InputAssets.Add(asset);

                // Add an output asset toocontain hello results of hello job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means hello output asset is in hello clear (unencrypted).
                IAsset smoothOutputAsset = 
                    smoothStreamingTask.OutputAssets.AddNew("Clear Smooth Streaming", 
                        AssetCreationOptions.None);

                return smoothOutputAsset;
            }

            /// <summary>
            /// Converts Smooth Streaming tooHLS.
            /// </summary>
            /// <param name="job">hello job toowhich tooadd hello new task.</param>
            /// <param name="asset">hello Smooth Streaming asset.</param>
            /// <returns>hello asset that was packaged tooHLS.</returns>
            private static IAsset PackageSmoothStreamToHLS(IJob job, IAsset smoothStreamAsset)
            {
                // Get hello SDK extension method too get a reference toohello Azure Media Packager.
                IMediaProcessor processor = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.WindowsAzureMediaPackager);

                // Read hello configuration data into a string. 
                // For hello HLS tooget encrypted with AES make sure tooset the
                // encrypt configuration property tootrue.
                //
                // In production, it is recommended toodo hello following:
                //    Set a Key url for your authn/authz server.
                //    Copy hello /asset-containerguid/*.key file tooyour server (or craft a key file for yourself).
                //    Delete *.key from hello asset container.
                //
                string configuration = File.ReadAllText(Path.Combine(_configurationXMLFiles, @"MediaPackager_SmoothToHLS.xml"));

                // Create a task with hello encoding details, using a configuration file.
                ITask task = job.Tasks.AddNew("My Smooth Streaming tooHLS Task",
                   processor,
                   configuration,
                   TaskOptions.ProtectedConfiguration);

                // Specify hello input asset toobe encoded.
                task.InputAssets.Add(smoothStreamAsset);

                // Add an output asset toocontain hello results of hello job. 
                IAsset outputAsset = 
                    task.OutputAssets.AddNew("HLS asset", AssetCreationOptions.None);


                return outputAsset;
            }
        }
    }

## <a name="using-static-encryption-tooprotect-hlsv3-with-playready"></a>Med hjälp av statiska kryptering tooProtect HLSv3 med PlayReady
Om du vill tooprotect ditt innehåll med PlayReady, du kan välja att använda [dynamisk kryptering](media-services-protect-with-drm.md) (hello rekommenderas) eller statisk kryptering (enligt beskrivningen i det här avsnittet).

> [!NOTE]
> I ordning tooprotect måste innehåll med PlayReady du först konvertera/koda ditt innehåll till ett Smooth Streaming-format.
> 
> 

hello exemplet i det här avsnittet kodar en mezzaninfil (i det här fallet MP4) till multibitrate MP4-filer. Den sedan paket MP4s till Smooth Streaming och krypterar Smooth Streaming med PlayReady. tooproduce HTTP Live Streaming (HLS) krypteras med PlayReady hello PlayReady Smooth Streaming-tillgångar måste toobe paketeras till HLS. Det här avsnittet visar hur tooperform dessa steg.

Media Services är nu en tjänst för att leverera Microsoft PlayReady-licenser. hello exempel i den här artikeln visar hur tooconfigure hello Media Services PlayReady licens delivery service (se hello **ConfigureLicenseDeliveryService** metod som definieras i hello koden nedan). 

Se till att tooupdate hello följande kod toopoint toohello mapp där MP4 indatafilen finns. Och även toowhere MediaPackager_MP4ToSmooth.xml och MediaPackager_SmoothToHLS.xml MediaEncryptor_PlayReadyProtection.xml filerna finns. MediaPackager_MP4ToSmooth.xml och MediaPackager_SmoothToHLS.xml definieras i [aktivitet förinställda för Azure Media Paketeraren](http://msdn.microsoft.com/library/azure/hh973635.aspx) och MediaEncryptor_PlayReadyProtection.xml har definierats i hello [aktivitet förinställningen för Azure Media Krypterare](http://msdn.microsoft.com/library/azure/hh973610.aspx) avsnittet.

    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Xml.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
    using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;

    namespace MediaServicesContentProtection
    {
        class Program
        {
            // Paths toosupport files (within hello above base path). You can use 
            // hello provided sample media files from hello "SupportFiles" folder, or 
            // provide paths tooyour own media files below toorun these samples.

            private static readonly string _mediaFiles =
                Path.GetFullPath(@"../..\Media");

            private static readonly string _singleMP4File =
                Path.Combine(_mediaFiles, @"SingleMP4\BigBuckBunny.mp4");

            // XML Configruation files path.
            private static readonly string _configurationXMLFiles = @"../..\Configurations\";


            private static MediaServicesCredentials _cachedCredentials = null;
            private static CloudMediaContext _context = null;

            // Media Services account information.
            private static readonly string _mediaServicesAccountName =
                ConfigurationManager.AppSettings["MediaServiceAccountName"];
            private static readonly string _mediaServicesAccountKey =
                ConfigurationManager.AppSettings["MediaServiceAccountKey"];

            static void Main(string[] args)
            {
                // Create and cache hello Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
                // Used hello chached credentials toocreate CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);

                // Load an MP4 file.
                IAsset asset = IngestSingleMP4File(_singleMP4File, AssetCreationOptions.None);

                // Encode an MP4 file tooa set of multibitrate MP4s.
                // Then, package a set of MP4s tooclear Smooth Streaming.
                IAsset clearSmoothStreamAsset = ConvertMP4ToMultibitrateMP4sToSmoothStreaming(asset);

                // Create a common encryption content key that is used 
                // a) tooset hello key values in hello MediaEncryptor_PlayReadyProtection.xml file
                //    that is used for encryption.
                // b) tooconfigure hello license delivery service and 
                //
                Guid keyId;
                byte[] contentKey;

                IContentKey key = CreateCommonEncryptionKey(out keyId, out contentKey);

                // hello content key authorization policy must be configured by you 
                // and met by hello client in order for hello PlayReady license
                // toobe delivered toohello client. 
                // In this example hello Media Services PlayReady license delivery service is used.
                ConfigureLicenseDeliveryService(key);

                // Get hello Media Services PlayReady license delivery URL.
                // This URL will be assigned toohello licenseAcquisitionUrl property 
                // of hello MediaEncryptor_PlayReadyProtection.xml file.
                Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);

                // Update hello MediaEncryptor_PlayReadyProtection.xml file with hello key and URL info.
                UpdatePlayReadyConfigurationXMLFile(keyId, contentKey, acquisitionUrl);

                // Create HLS encrypted with PlayReady.
                IAsset playReadyHLSAsset = CreateHLSEncryptedWithPlayReady(clearSmoothStreamAsset);
                //
                string hlsWithPlayReadyURL = playReadyHLSAsset.GetHlsUri().ToString();
                Console.WriteLine("HLS with PlayReady URL:");
                Console.WriteLine(hlsWithPlayReadyURL);
            }

            /// <summary>
            /// Creates a job with 2 tasks: 
            /// 1 task - encodes a single MP4 toomultibitrate MP4s,
            /// 2 task - packages MP4s tooSmooth Streaming.
            /// </summary>
            /// <returns>hello output asset.</returns>
            public static IAsset ConvertMP4ToMultibitrateMP4sToSmoothStreaming(IAsset asset)
            {
                // Create a new job.
                IJob job = _context.Jobs.Create("Convert MP4 tooSmooth Streaming.");

                // Add task 1 - Encode single MP4 into multibitrate MP4s.
                IAsset MP4sAsset = EncodeSingleMP4IntoMultibitrateMP4sTask(job, asset);
                // Add task 2 - Package a multibitrate MP4 set tooClear Smooth Streaming.
                IAsset packagedAsset = PackageMP4ToSmoothStreamingTask(job, MP4sAsset);

                // Submit hello job and wait until it is completed.
                job.Submit();
                job = job.StartExecutionProgressTask(
                    j =>
                    {
                        Console.WriteLine("Job state: {0}", j.State);
                        Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
                    },
                    CancellationToken.None).Result;

                // Get hello output asset that contains Smooth Streaming.
                return job.OutputMediaAssets[1];
            }

            /// <summary>
            /// Create a common encryption content key that is used 
            /// tooset hello key values in hello MediaEncryptor_PlayReadyProtection.xml file
            /// that is used for encryption.
            /// </summary>
            /// <param name="keyId"></param>
            /// <param name="contentKey"></param>
            /// <returns></returns>
            public static IContentKey CreateCommonEncryptionKey(out Guid keyId, out byte[] contentKey)
            {
                keyId = Guid.NewGuid();
                contentKey = GetRandomBuffer(16);

                IContentKey key = _context.ContentKeys.Create(
                                        keyId,
                                        contentKey,
                                        "ContentKey",
                                        ContentKeyType.CommonEncryption);

                return key;
            }

            /// <summary>
            /// Update your configuration .xml file dynamically.
            /// </summary>
            public static void UpdatePlayReadyConfigurationXMLFile(Guid keyId, byte[] keyValue, Uri licenseAcquisitionUrl)
            {
                string xmlFileName = Path.Combine(_configurationXMLFiles,
                                            @"MediaEncryptor_PlayReadyProtection.xml");

                XNamespace xmlns = "http://schemas.microsoft.com/iis/media/v4/TM/TaskDefinition#";

                // Prepare hello encryption task template
                XDocument doc = XDocument.Load(xmlFileName);

                var licenseAcquisitionUrlEl = doc
                        .Descendants(xmlns + "property")
                        .Where(p => p.Attribute("name").Value == "licenseAcquisitionUrl")
                        .FirstOrDefault();
                var contentKeyEl = doc
                        .Descendants(xmlns + "property")
                        .Where(p => p.Attribute("name").Value == "contentKey")
                        .FirstOrDefault();
                var keyIdEl = doc
                        .Descendants(xmlns + "property")
                        .Where(p => p.Attribute("name").Value == "keyId")
                        .FirstOrDefault();

                // Update hello "value" property.
                if (licenseAcquisitionUrlEl != null)
                    licenseAcquisitionUrlEl.Attribute("value").SetValue(licenseAcquisitionUrl.ToString());

                if (contentKeyEl != null)
                    contentKeyEl.Attribute("value").SetValue(Convert.ToBase64String(keyValue));

                if (keyIdEl != null)
                    keyIdEl.Attribute("value").SetValue(keyId);

                doc.Save(xmlFileName);
            }

            /// <summary>
            // Encrypts clear Smooth Streaming tooSmooth Streaming with PlayReady.
            // Then, packages hello PlayReady Smooth Streaming tooHLS with PlayReady.
            /// </summary>
            /// <param name="clearSmoothAsset">Asset that contains clear Smooth Streaming.</param>
            /// <returns>hello output asset.</returns>
            public static IAsset CreateHLSEncryptedWithPlayReady(IAsset clearSmoothStreamAsset)
            {
                IJob job = _context.Jobs.Create("Encrypt tooHLS with PlayReady.");

                // Add task 1 - Encrypt Smooth Streaming with PlayReady 
                IAsset encryptedSmoothAsset =
                    EncryptSmoothStreamWithPlayReadyTask(job, clearSmoothStreamAsset);

                // Add task 2 - Package tooHLS with PlayReady.
                PackageSmoothStreamToHLS(job, encryptedSmoothAsset);

                // Submit hello job and wait until it is completed.
                job.Submit();
                job = job.StartExecutionProgressTask(
                    j =>
                    {
                        Console.WriteLine("Job state: {0}", j.State);
                        Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
                    },
                    CancellationToken.None).Result;

                // Since we had two tasks, hello OutputMediaAssets[1]
                // contains hello desired asset.
                _context.Locators.Create(
                    LocatorType.OnDemandOrigin,
                    job.OutputMediaAssets[1],
                    AccessPermissions.Read,
                    TimeSpan.FromDays(30));

                return job.OutputMediaAssets[1];
            }

            /// <summary>
            /// Uploads a single file.
            /// </summary>
            /// <param name="fileDir">hello location of hello files.</param>
            /// <param name="assetCreationOptions">
            ///  You can specify hello following encryption options for hello AssetCreationOptions.
            ///      None:  no encryption.  
            ///      StorageEncrypted: storage encryption. Encrypts a clear input file 
            ///        before it is uploaded tooAzure storage. 
            ///      CommonEncryptionProtected: for Common Encryption Protected (CENC) files. 
            ///        For example, a set of files that are already PlayReady encrypted. 
            ///      EnvelopeEncryptionProtected: for HLS with AES encryption files.
            ///        NOTE: hello files must have been encoded and encrypted by Transform Manager. 
            ///     </param>
            /// <returns>Returns an asset that contains a single file.</returns>
            /// </summary>
            /// <returns></returns>
            private static IAsset IngestSingleMP4File(string fileDir, AssetCreationOptions assetCreationOptions)
            {
                // Use hello SDK extension method toocreate a new asset by 
                // uploading a mezzanine file from a local path.
                IAsset asset = _context.Assets.CreateFromFile(
                    fileDir,
                    assetCreationOptions,
                    (af, p) =>
                    {
                        Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
                    });


                return asset;

            }
            /// <summary>
            /// Creates a task tooencode tooAdaptive Bitrate. 
            /// Adds hello new task tooa job.
            /// </summary>
            /// <param name="job">hello job toowhich tooadd hello new task.</param>
            /// <param name="asset">hello input asset.</param>
            /// <returns>hello output asset.</returns>
            private static IAsset EncodeSingleMP4IntoMultibitrateMP4sTask(IJob job, IAsset asset)
            {
                // Get hello SDK extension method too get a reference toohello Media Encoder Standard.
                IMediaProcessor encoder = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.MediaEncoderStandard);

                ITask adpativeBitrateTask = job.Tasks.AddNew("MP4 tooAdaptive Bitrate Task",
                   encoder,
                   "Adaptive Streaming",
                   TaskOptions.None);

                // Specify hello input Asset
                adpativeBitrateTask.InputAssets.Add(asset);

                // Add an output asset toocontain hello results of hello job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means hello output asset is in hello clear (unencrypted).
                IAsset abrAsset = adpativeBitrateTask.OutputAssets.AddNew("Multibitrate MP4s",
                                        AssetCreationOptions.None);

                return abrAsset;
            }

            /// <summary>
            /// Creates a task tooconvert hello MP4 file(s) tooa Smooth Streaming asset.
            /// Adds hello new task tooa job.
            /// </summary>
            /// <param name="job">hello job toowhich tooadd hello new task.</param>
            /// <param name="asset">hello input asset.</param>
            /// <returns>hello output asset.</returns>
            private static IAsset PackageMP4ToSmoothStreamingTask(IJob job, IAsset asset)
            {
                // Get hello SDK extension method too get a reference toohello Azure Media Packager.
                IMediaProcessor packager = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.WindowsAzureMediaPackager);

                // Azure Media Packager does not accept string presets, so load xml configuration
                string smoothConfig = File.ReadAllText(Path.Combine(
                            _configurationXMLFiles,
                            "MediaPackager_MP4toSmooth.xml"));

                // Create a new Task tooconvert adaptive bitrate tooSmooth Streaming.
                ITask smoothStreamingTask = job.Tasks.AddNew("MP4 tooSmooth Task",
                   packager,
                   smoothConfig,
                   TaskOptions.None);

                // Specify hello input Asset, which is hello output Asset from hello first task
                smoothStreamingTask.InputAssets.Add(asset);

                // Add an output asset toocontain hello results of hello job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means hello output asset is in hello clear (unencrypted).
                IAsset smoothOutputAsset =
                    smoothStreamingTask.OutputAssets.AddNew("Clear Smooth Streaming",
                        AssetCreationOptions.None);

                return smoothOutputAsset;
            }


            /// <summary>
            /// Converts Smooth Stream tooHLS.
            /// </summary>
            /// <param name="job">hello job toowhich tooadd hello new task.</param>
            /// <param name="asset">hello Smooth Stream asset.</param>
            /// <returns>hello asset that was packaged tooHLS.</returns>
            private static IAsset PackageSmoothStreamToHLS(IJob job, IAsset smoothStreamAsset)
            {
                // Get hello SDK extension method too get a reference toohello Azure Media Packager.
                IMediaProcessor processor = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.WindowsAzureMediaPackager);

                // Read hello configuration data into a string. 
                //
                string configuration = File.ReadAllText(
                            Path.Combine(_configurationXMLFiles,
                                        @"MediaPackager_SmoothToHLS.xml"));

                // Create a task with hello encoding details, using a configuration file.
                ITask task = job.Tasks.AddNew("My Smooth tooHLS Task",
                   processor,
                   configuration,
                   TaskOptions.ProtectedConfiguration);

                // Specify hello input asset toobe encoded.
                task.InputAssets.Add(smoothStreamAsset);

                // Add an output asset toocontain hello results of hello job. 
                IAsset outputAsset =
                    task.OutputAssets.AddNew("HLS asset", AssetCreationOptions.None);


                return outputAsset;
            }

            /// <summary>
            /// Creates a task tooencrypt Smooth Streaming with PlayReady.
            /// Note: Do deliver DASH, make sure tooset hello useSencBox and adjustSubSamples 
            /// configuration properties tootrue.
            /// </summary>
            /// <param name="job">hello job toowhich tooadd hello new task.</param>
            /// <param name="asset">hello input asset.</param>
            /// <returns>hello output asset.</returns>
            private static IAsset EncryptSmoothStreamWithPlayReadyTask(IJob job, IAsset asset)
            {
                // Get hello SDK extension method too get a reference toohello Azure Media Encryptor.
                IMediaProcessor playreadyProcessor = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.WindowsAzureMediaEncryptor);

                // Read hello configuration XML.
                //
                // Note that hello configuration defined in MediaEncryptor_PlayReadyProtection.xml
                // is using keySeedValue. It is recommended that you do this only for testing 
                // and not in production. For more information, see 
                // http://msdn.microsoft.com/library/windowsazure/dn189154.aspx.
                //
                string configPlayReady = File.ReadAllText(Path.Combine(_configurationXMLFiles,
                                            @"MediaEncryptor_PlayReadyProtection.xml"));

                ITask playreadyTask = job.Tasks.AddNew("My PlayReady Task",
                   playreadyProcessor,
                   configPlayReady,
                   TaskOptions.ProtectedConfiguration);

                playreadyTask.InputAssets.Add(asset);

                // Add an output asset toocontain hello results of hello job. 
                // This output is specified as AssetCreationOptions.CommonEncryptionProtected.
                IAsset playreadyAsset = playreadyTask.OutputAssets.AddNew(
                                                "PlayReady Smooth Streaming",
                                                AssetCreationOptions.CommonEncryptionProtected);


                return playreadyAsset;
            }


            /// <summary>
            /// Configures authorization policy for hello content key. 
            /// </summary>
            /// <param name="contentKey">hello content key.</param>
            static public void ConfigureLicenseDeliveryService(IContentKey contentKey)
            {
                // Create ContentKeyAuthorizationPolicy with Open restrictions 
                // and create authorization policy          

                List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
                {
                    new ContentKeyAuthorizationPolicyRestriction 
                    { 
                        Name = "Open", 
                        KeyRestrictionType = (int)ContentKeyRestrictionType.Open, 
                        Requirements = null
                    }
                };

                // Configure PlayReady license template.
                string newLicenseTemplate = ConfigurePlayReadyLicenseTemplate();

                IContentKeyAuthorizationPolicyOption policyOption =
                    _context.ContentKeyAuthorizationPolicyOptions.Create("",
                        ContentKeyDeliveryType.PlayReadyLicense,
                            restrictions, newLicenseTemplate);

                IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                            ContentKeyAuthorizationPolicies.
                            CreateAsync("Deliver Common Content Key with no restrictions").
                            Result;


                contentKeyAuthorizationPolicy.Options.Add(policyOption);

                // Associate hello content key authorization policy with hello content key.
                contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
                contentKey = contentKey.UpdateAsync().Result;
            }

            static private string ConfigurePlayReadyLicenseTemplate()
            {
                // hello following code configures PlayReady License Template using .NET classes
                // and returns hello XML string.

                PlayReadyLicenseResponseTemplate responseTemplate = new PlayReadyLicenseResponseTemplate();
                PlayReadyLicenseTemplate licenseTemplate = new PlayReadyLicenseTemplate();

                responseTemplate.LicenseTemplates.Add(licenseTemplate);

                return MediaServicesLicenseTemplateSerializer.Serialize(responseTemplate);
            }
            static private byte[] GetRandomBuffer(int length)
            {
                var returnValue = new byte[length];

                using (var rng =
                    new System.Security.Cryptography.RNGCryptoServiceProvider())
                {
                    rng.GetBytes(returnValue);
                }

                return returnValue;
            }

        }
    }

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

