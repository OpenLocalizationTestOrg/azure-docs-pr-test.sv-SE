---
title: "aaaHow tooperform direktsänd strömning med Azure Media Services toocreate-dataströmmar i multibithastighet med .NET | Microsoft Docs"
description: "Den här kursen får du hello steg som krävs för att skapa en kanal som tar emot en direktsänd dataström med enkel bithastighet och kodar den dataström med multibithastighet toomulti med .NET SDK."
services: media-services
documentationcenter: 
author: anilmur
manager: cfowler
editor: 
ms.assetid: 4df5e690-ff63-47cc-879b-9c57cb8ec240
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/17/2017
ms.author: juliako;anilmur
ms.openlocfilehash: 22088e6a78a49bd839575614a7c17a411ae8081c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooperform-live-streaming-using-azure-media-services-toocreate-multi-bitrate-streams-with-net"></a>Hur tooperform direktsänd strömning med Azure Media Services toocreate multibithastighet dataströmmar med .NET
> [!div class="op_single_selector"]
> * [Portal](media-services-portal-creating-live-encoder-enabled-channel.md)
> * [.NET](media-services-dotnet-creating-live-encoder-enabled-channel.md)
> * [REST-API](https://docs.microsoft.com/rest/api/media/operations/channel)
> 
> [!NOTE]
> toocomplete den här självstudiekursen kommer du behöver ett Azure-konto. Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
> 
> 

## <a name="overview"></a>Översikt
Den här självstudiekursen vägleder dig genom hello stegen för att skapa en **kanal** som tar emot en direktsänd dataström med enkel bithastighet och kodar den dataström med multibithastighet toomulti.

Mer information finns i Närliggande tooChannels som är aktiverade för live encoding [direktsänd strömning med Azure Media Services toocreate multibithastighet dataströmmar](media-services-manage-live-encoder-enabled-channels.md).

## <a name="common-live-streaming-scenario"></a>Vanligt scenario för direktsänd strömning
hello följande steg beskriver uppgifter som ingår i att skapa vanliga program för direktsänd strömning.

> [!NOTE]
> Hej max är för närvarande rekommenderas varaktighet för en direktsänd händelse 8 timmar. Kontakta amslived på Microsoft.com om du behöver toorun en kanal under längre perioder.
> 
> 

1. Anslut en videokamera tooa dator. Starta och konfigurera en lokal livekodare som kan mata ut en dataström med enkel bithastighet i något av hello följande protokoll: RTMP, Smooth Streaming eller RTP (MPEG-TS). Mer information finns i [Support och livekodare för Azure Media Services RTMP](http://go.microsoft.com/fwlink/?LinkId=532824).

    Det här steget kan också utföras när du har skapat din kanal.

2. Skapa och starta en kanal.
3. Hämta hello kanal infognings-URL.

    hello infognings-URL som används av hello livekodaren toosend hello dataströmmen toohello kanal.

4. Hämta hello kanalens förhandsgransknings-URL.

    Använd den här URL: en tooverify att din kanal är tar emot hello direktsänd dataström.

5. Skapa en tillgång.
6. Om du vill använda för hello tillgången toobe dynamiskt krypterad under uppspelningen hello följande:
7. Skapa en innehållsnyckel.
8. Konfigurera hello innehållsnyckelns auktoriseringsprincip.
9. Konfigurera en princip för tillgångsleveranser (används av dynamisk paketering och dynamisk kryptering).
10. Skapa ett program och ange toouse hello tillgång som du skapade.
11. Publicera hello tillgången som är associerad med hello programmet genom att skapa en OnDemand-positionerare.

    >[!NOTE]
    >När AMS-kontot skapas en **standard** strömningsslutpunkt har lagts till tooyour konto i hello **stoppad** tillstånd. Hej strömningsslutpunkt från vilken du vill att toostream innehåll har toobe i hello **kör** tillstånd. 

12. Starta hello programmet när du är klar toostart strömning och arkivering.
13. Du kan också kan hello livekodare vara signalerat toostart en annons. hello annonsen infogas i utdataströmmen hello.
14. Stoppa programmet hello när du vill toostop strömningen och arkiveringen hello händelsen.
15. Ta bort hello programmet (och ta eventuellt bort tillgången hello).

## <a name="what-youll-learn"></a>Detta får du får lära dig
Det här avsnittet beskrivs hur du tooexecute olika åtgärder i kanaler och program med hjälp av Media Services .NET SDK. Eftersom många åtgärder är långvariga används .NET-API:er som hanterar långvariga åtgärder.

Hej avsnittet visar hur toodo hello följande:

1. Skapa och starta en kanal. Långvariga API:er används.
2. Hämta infognings-hello kanaler (indataslutpunkten). Den här slutpunkten ska tillhandahållas toohello kodare som kan skicka en direktsänd dataström med enkel bithastighet.
3. Hämta förhandsgranskningsslutpunkten hello. Den här slutpunkten är används toopreview strömmen.
4. Skapa en tillgång som kommer att använda toostore ditt innehåll. hello principerna för tillgångsleverans ska också, som visas i det här exemplet konfigureras.
5. Skapa ett program och ange toouse hello tillgång som du skapade tidigare. Starta programmet hello. Långvariga API:er används.
6. Skapa en positionerare för tillgången hello, så hello innehållet publiceras och kan strömmas tooyour klienter.
7. Visa eller dölj pekdatorer. Starta och stoppa annonser. Långvariga API:er används.
8. Rensa din kanal och alla hello associerade resurser.

## <a name="prerequisites"></a>Krav
hello följande är obligatoriska toocomplete hello kursen.

* Ett Azure-konto. Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter. Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F). Du får då krediter som kan vara används tootry ut betald Azure-tjänster. Även efter hello krediten är slut, kan du hålla hello-konto och använda kostnadsfria Azure-tjänster och funktioner, till exempel hello Web Apps i Azure App Service.
* Ett Media Services-konto. toocreate Media Services-konto finns [skapa konto](media-services-portal-create-account.md).
* Visual Studio 2010 SP1 (Professional, Premium, Ultimate eller Express) eller senare versioner.
* Du måste använda Media Services .NET SDK version 3.2.0.0 eller senare.
* En webbkamera och en kodare som kan skicka en direktsänd dataström i enkel bithastighet.

## <a name="considerations"></a>Överväganden
* Hej max är för närvarande rekommenderas varaktighet för en direktsänd händelse 8 timmar. Kontakta amslived på Microsoft.com om du behöver toorun en kanal under längre perioder.
* Det finns en gräns på 1 000 000 principer för olika AMS-principer (till exempel för positionerarprincipen eller ContentKeyAuthorizationPolicy). Du bör använda hello samma princip-ID om du alltid använder hello samma dagar / åtkomstbehörigheter, till exempel principer för lokaliserare som är avsedda tooremain på plats för lång tid (icke-överföringen principer). Mer information finns i [detta](media-services-dotnet-manage-entities.md#limit-access-policies) avsnitt.

## <a name="download-sample"></a>Hämta exempel

Du kan hämta hello-exempel som beskrivs i det här avsnittet från [här](https://azure.microsoft.com/documentation/samples/media-services-dotnet-encode-live-stream-with-ams-clear/).

## <a name="set-up-for-development-with-media-services-sdk-for-net"></a>Konfigurera för utveckling med Media Services SDK för .NET

Konfigurera utvecklingsmiljön och fylla hello app.config-fil med anslutningsinformation, enligt beskrivningen i [Media Services-utveckling med .NET](media-services-dotnet-how-to-use.md). 

## <a name="code-example"></a>Kodexempel

    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Net;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;

    namespace EncodeLiveStreamWithAmsClear
    {
        class Program
        {
        private const string ChannelName = "channel001";
        private const string AssetlName = "asset001";
        private const string ProgramlName = "program001";

        // Read values from hello App.config file.
        private static readonly string _AADTenantDomain =
        ConfigurationManager.AppSettings["AADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
        ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

        private static CloudMediaContext _context = null;

        static void Main(string[] args)
        {
            var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

            IChannel channel = CreateAndStartChannel();

            // hello channel's input endpoint:
            string ingestUrl = channel.Input.Endpoints.FirstOrDefault().Url.ToString();

            Console.WriteLine("Intest URL: {0}", ingestUrl);


            // Use hello previewEndpoint toopreview and verify 
            // that hello input from hello encoder is actually reaching hello Channel. 
            string previewEndpoint = channel.Preview.Endpoints.FirstOrDefault().Url.ToString();

            Console.WriteLine("Preview URL: {0}", previewEndpoint);

            // When Live Encoding is enabled, you can now get a preview of hello live feed as it reaches hello Channel. 
            // This can be a valuable tool toocheck whether your live feed is actually reaching hello Channel. 
            // hello thumbnail is exposed via hello same end-point as hello Channel Preview URL.
            string thumbnailUri = new UriBuilder
            {
            Scheme = Uri.UriSchemeHttps,
            Host = channel.Preview.Endpoints.FirstOrDefault().Url.Host,
            Path = "thumbnails/input.jpg"
            }.Uri.ToString();

            Console.WriteLine("Thumbain URL: {0}", thumbnailUri);

            // Once you previewed your stream and verified that it is flowing into your Channel, 
            // you can create an event by creating an Asset, Program, and Streaming Locator. 
            IAsset asset = CreateAndConfigureAsset();

            IProgram program = CreateAndStartProgram(channel, asset);

            ILocator locator = CreateLocatorForAsset(program.Asset, program.ArchiveWindowLength);

            // You can use slates and ads only if hello channel type is Standard.  
            StartStopAdsSlates(channel);

            // Once you are done streaming, clean up your resources.
            Cleanup(channel);
        }

        public static IChannel CreateAndStartChannel()
        {
            var channelInput = CreateChannelInput();
            var channePreview = CreateChannelPreview();
            var channelEncoding = CreateChannelEncoding();

            ChannelCreationOptions options = new ChannelCreationOptions
            {
            EncodingType = ChannelEncodingType.Standard,
            Name = ChannelName,
            Input = channelInput,
            Preview = channePreview,
            Encoding = channelEncoding
            };

            Log("Creating channel");
            IOperation channelCreateOperation = _context.Channels.SendCreateOperation(options);
            string channelId = TrackOperation(channelCreateOperation, "Channel create");

            IChannel channel = _context.Channels.Where(c => c.Id == channelId).FirstOrDefault();

            Log("Starting channel");
            var channelStartOperation = channel.SendStartOperation();
            TrackOperation(channelStartOperation, "Channel start");

            return channel;
        }

        /// <summary>
        /// Create channel input, used in channel creation options. 
        /// </summary>
        /// <returns></returns>
        private static ChannelInput CreateChannelInput()
        {
            return new ChannelInput
            {
            StreamingProtocol = StreamingProtocol.RTPMPEG2TS,
            AccessControl = new ChannelAccessControl
            {
                IPAllowList = new List<IPRange>
                {
                    new IPRange
                    {
                    Name = "TestChannelInput001",
                    Address = IPAddress.Parse("0.0.0.0"),
                    SubnetPrefixLength = 0
                    }
                }
            }
            };
        }

        /// <summary>
        /// Create channel preview, used in channel creation options. 
        /// </summary>
        /// <returns></returns>
        private static ChannelPreview CreateChannelPreview()
        {
            return new ChannelPreview
            {
            AccessControl = new ChannelAccessControl
            {
                IPAllowList = new List<IPRange>
                {
                    new IPRange
                    {
                    Name = "TestChannelPreview001",
                    Address = IPAddress.Parse("0.0.0.0"),
                    SubnetPrefixLength = 0
                    }
                }
            }
            };
        }

        /// <summary>
        /// Create channel encoding, used in channel creation options. 
        /// </summary>
        /// <returns></returns>
        private static ChannelEncoding CreateChannelEncoding()
        {
            return new ChannelEncoding
            {
            SystemPreset = "Default720p",
            IgnoreCea708ClosedCaptions = false,
            AdMarkerSource = AdMarkerSource.Api,
            // You can only set audio if streaming protocol is set tooStreamingProtocol.RTPMPEG2TS.
            AudioStreams = new List<AudioStream> { new AudioStream { Index = 103, Language = "eng" } }.AsReadOnly()
            };
        }

        /// <summary>
        /// Create an asset and configure asset delivery policies.
        /// </summary>
        /// <returns></returns>
        public static IAsset CreateAndConfigureAsset()
        {
            IAsset asset = _context.Assets.Create(AssetlName, AssetCreationOptions.None);

            IAssetDeliveryPolicy policy =
            _context.AssetDeliveryPolicies.Create("Clear Policy",
            AssetDeliveryPolicyType.NoDynamicEncryption,
            AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.Dash, null);

            asset.DeliveryPolicies.Add(policy);

            return asset;
        }

        /// <summary>
        /// Create a Program on hello Channel. You can have multiple Programs that overlap or are sequential;
        /// however each Program must have a unique name within your Media Services account.
        /// </summary>
        /// <param name="channel"></param>
        /// <param name="asset"></param>
        /// <returns></returns>
        public static IProgram CreateAndStartProgram(IChannel channel, IAsset asset)
        {
            IProgram program = channel.Programs.Create(ProgramlName, TimeSpan.FromHours(3), asset.Id);
            Log("Program created", program.Id);

            Log("Starting program");
            var programStartOperation = program.SendStartOperation();
            TrackOperation(programStartOperation, "Program start");

            return program;
        }

        /// <summary>
        /// Create locators in order toobe able toopublish and stream hello video.
        /// </summary>
        /// <param name="asset"></param>
        /// <param name="ArchiveWindowLength"></param>
        /// <returns></returns>
        public static ILocator CreateLocatorForAsset(IAsset asset, TimeSpan ArchiveWindowLength)
        {
            // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.            
            var locator = _context.Locators.CreateLocator
            (
                LocatorType.OnDemandOrigin,
                asset,
                _context.AccessPolicies.Create
                (
                    "Live Stream Policy",
                    ArchiveWindowLength,
                    AccessPermissions.Read
                )
            );

            return locator;
        }

        /// <summary>
        /// Perform operations on slates.
        /// </summary>
        /// <param name="channel"></param>
        public static void StartStopAdsSlates(IChannel channel)
        {
            int cueId = new Random().Next(int.MaxValue);
            var path = Path.GetFullPath(Path.Combine(AppDomain.CurrentDomain.BaseDirectory, @"..\\..\\SlateJPG\\DefaultAzurePortalSlate.jpg"));

            Log("Creating asset");
            var slateAsset = _context.Assets.Create("Slate test asset " + DateTime.Now.ToString("yyyy-MM-dd HH-mm"), AssetCreationOptions.None);
            Log("Slate asset created", slateAsset.Id);

            Log("Uploading file");
            var assetFile = slateAsset.AssetFiles.Create("DefaultAzurePortalSlate.jpg");
            assetFile.Upload(path);
            assetFile.IsPrimary = true;
            assetFile.Update();

            Log("Showing slate");
            var showSlateOpeartion = channel.SendShowSlateOperation(TimeSpan.FromMinutes(1), slateAsset.Id);
            TrackOperation(showSlateOpeartion, "Show slate");

            Log("Hiding slate");
            var hideSlateOperation = channel.SendHideSlateOperation();
            TrackOperation(hideSlateOperation, "Hide slate");

            Log("Starting ad");
            var startAdOperation = channel.SendStartAdvertisementOperation(TimeSpan.FromMinutes(1), cueId, false);
            TrackOperation(startAdOperation, "Start ad");

            Log("Ending ad");
            var endAdOperation = channel.SendEndAdvertisementOperation(cueId);
            TrackOperation(endAdOperation, "End ad");

            Log("Deleting slate asset");
            slateAsset.Delete();
        }

        /// <summary>
        /// Clean up resources associated with hello channel.
        /// </summary>
        /// <param name="channel"></param>
        public static void Cleanup(IChannel channel)
        {
            IAsset asset;
            if (channel != null)
            {
            foreach (var program in channel.Programs)
            {
                asset = _context.Assets.Where(se => se.Id == program.AssetId)
                            .FirstOrDefault();

                Log("Stopping program");
                var programStopOperation = program.SendStopOperation();
                TrackOperation(programStopOperation, "Program stop");

                program.Delete();

                if (asset != null)
                {
                Log("Deleting locators");
                foreach (var l in asset.Locators)
                    l.Delete();

                Log("Deleting asset");
                asset.Delete();
                }
            }

            Log("Stopping channel");
            var channelStopOperation = channel.SendStopOperation();
            TrackOperation(channelStopOperation, "Channel stop");

            Log("Deleting channel");
            var channelDeleteOperation = channel.SendDeleteOperation();
            TrackOperation(channelDeleteOperation, "Channel delete");
            }
        }

        /// <summary>
        /// Track long running operations.
        /// </summary>
        /// <param name="operation"></param>
        /// <param name="description"></param>
        /// <returns></returns>
        public static string TrackOperation(IOperation operation, string description)
        {
            string entityId = null;
            bool isCompleted = false;

            Log("starting tootrack ", null, operation.Id);
            while (isCompleted == false)
            {
            operation = _context.Operations.GetOperation(operation.Id);
            isCompleted = IsCompleted(operation, out entityId);
            System.Threading.Thread.Sleep(TimeSpan.FromSeconds(30));
            }
            // If we got here, hello operation succeeded.
            Log(description + " in completed", operation.TargetEntityId, operation.Id);

            return entityId;
        }

        /// <summary> 
        /// Checks if hello operation has been completed. 
        /// If hello operation succeeded, hello created entity Id is returned in hello out parameter.
        /// </summary> 
        /// <param name="operationId">hello operation Id.</param> 
        /// <param name="channel">
        /// If hello operation succeeded, 
        /// hello entity Id associated with hello sucessful operation is returned in hello out parameter.</param>
        /// <returns>Returns false if hello operation is still in progress; otherwise, true.</returns> 
        private static bool IsCompleted(IOperation operation, out string entityId)
        {
            bool completed = false;

            entityId = null;

            switch (operation.State)
            {
            case OperationState.Failed:
                // Handle hello failure. 
                // For example, throw an exception. 
                // Use hello following information in hello exception: operationId, operation.ErrorMessage.
                Log("operation failed", operation.TargetEntityId, operation.Id);
                break;
            case OperationState.Succeeded:
                completed = true;
                entityId = operation.TargetEntityId;
                break;
            case OperationState.InProgress:
                completed = false;
                Log("operation in progress", operation.TargetEntityId, operation.Id);
                break;
            }
            return completed;
        }

        private static void Log(string action, string entityId = null, string operationId = null)
        {
            Console.WriteLine(
            "{0,-21}{1,-51}{2,-51}{3,-51}",
            DateTime.Now.ToString("yyyy'-'MM'-'dd HH':'mm':'ss"),
            action,
            entityId ?? string.Empty,
            operationId ?? string.Empty);
        }
        }
    }

## <a name="next-step"></a>Nästa steg
Granska sökvägarna för Media Services-utbildning.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


