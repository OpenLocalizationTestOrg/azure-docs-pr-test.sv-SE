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
# <a name="how-tooperform-live-streaming-using-azure-media-services-toocreate-multi-bitrate-streams-with-net"></a><span data-ttu-id="527e0-103">Hur tooperform direktsänd strömning med Azure Media Services toocreate multibithastighet dataströmmar med .NET</span><span class="sxs-lookup"><span data-stu-id="527e0-103">How tooperform live streaming using Azure Media Services toocreate multi-bitrate streams with .NET</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="527e0-104">Portal</span><span class="sxs-lookup"><span data-stu-id="527e0-104">Portal</span></span>](media-services-portal-creating-live-encoder-enabled-channel.md)
> * [<span data-ttu-id="527e0-105">.NET</span><span class="sxs-lookup"><span data-stu-id="527e0-105">.NET</span></span>](media-services-dotnet-creating-live-encoder-enabled-channel.md)
> * [<span data-ttu-id="527e0-106">REST-API</span><span class="sxs-lookup"><span data-stu-id="527e0-106">REST API</span></span>](https://docs.microsoft.com/rest/api/media/operations/channel)
> 
> [!NOTE]
> <span data-ttu-id="527e0-107">toocomplete den här självstudiekursen kommer du behöver ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="527e0-107">toocomplete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="527e0-108">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="527e0-108">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
> 
> 

## <a name="overview"></a><span data-ttu-id="527e0-109">Översikt</span><span class="sxs-lookup"><span data-stu-id="527e0-109">Overview</span></span>
<span data-ttu-id="527e0-110">Den här självstudiekursen vägleder dig genom hello stegen för att skapa en **kanal** som tar emot en direktsänd dataström med enkel bithastighet och kodar den dataström med multibithastighet toomulti.</span><span class="sxs-lookup"><span data-stu-id="527e0-110">This tutorial walks you through hello steps of creating a **Channel** that receives a single-bitrate live stream and encodes it toomulti-bitrate stream.</span></span>

<span data-ttu-id="527e0-111">Mer information finns i Närliggande tooChannels som är aktiverade för live encoding [direktsänd strömning med Azure Media Services toocreate multibithastighet dataströmmar](media-services-manage-live-encoder-enabled-channels.md).</span><span class="sxs-lookup"><span data-stu-id="527e0-111">For more conceptual information related tooChannels that are enabled for live encoding, see [Live streaming using Azure Media Services toocreate multi-bitrate streams](media-services-manage-live-encoder-enabled-channels.md).</span></span>

## <a name="common-live-streaming-scenario"></a><span data-ttu-id="527e0-112">Vanligt scenario för direktsänd strömning</span><span class="sxs-lookup"><span data-stu-id="527e0-112">Common Live Streaming Scenario</span></span>
<span data-ttu-id="527e0-113">hello följande steg beskriver uppgifter som ingår i att skapa vanliga program för direktsänd strömning.</span><span class="sxs-lookup"><span data-stu-id="527e0-113">hello following steps describe tasks involved in creating common live streaming applications.</span></span>

> [!NOTE]
> <span data-ttu-id="527e0-114">Hej max är för närvarande rekommenderas varaktighet för en direktsänd händelse 8 timmar.</span><span class="sxs-lookup"><span data-stu-id="527e0-114">Currently, hello max recommended duration of a live event is 8 hours.</span></span> <span data-ttu-id="527e0-115">Kontakta amslived på Microsoft.com om du behöver toorun en kanal under längre perioder.</span><span class="sxs-lookup"><span data-stu-id="527e0-115">Please contact amslived at Microsoft.com if you need toorun a Channel for longer periods of time.</span></span>
> 
> 

1. <span data-ttu-id="527e0-116">Anslut en videokamera tooa dator.</span><span class="sxs-lookup"><span data-stu-id="527e0-116">Connect a video camera tooa computer.</span></span> <span data-ttu-id="527e0-117">Starta och konfigurera en lokal livekodare som kan mata ut en dataström med enkel bithastighet i något av hello följande protokoll: RTMP, Smooth Streaming eller RTP (MPEG-TS).</span><span class="sxs-lookup"><span data-stu-id="527e0-117">Launch and configure an on-premises live encoder that can output a single bitrate stream in one of hello following protocols: RTMP, Smooth Streaming, or RTP (MPEG-TS).</span></span> <span data-ttu-id="527e0-118">Mer information finns i [Support och livekodare för Azure Media Services RTMP](http://go.microsoft.com/fwlink/?LinkId=532824).</span><span class="sxs-lookup"><span data-stu-id="527e0-118">For more information, see [Azure Media Services RTMP Support and Live Encoders](http://go.microsoft.com/fwlink/?LinkId=532824).</span></span>

    <span data-ttu-id="527e0-119">Det här steget kan också utföras när du har skapat din kanal.</span><span class="sxs-lookup"><span data-stu-id="527e0-119">This step could also be performed after you create your Channel.</span></span>

2. <span data-ttu-id="527e0-120">Skapa och starta en kanal.</span><span class="sxs-lookup"><span data-stu-id="527e0-120">Create and start a Channel.</span></span>
3. <span data-ttu-id="527e0-121">Hämta hello kanal infognings-URL.</span><span class="sxs-lookup"><span data-stu-id="527e0-121">Retrieve hello Channel ingest URL.</span></span>

    <span data-ttu-id="527e0-122">hello infognings-URL som används av hello livekodaren toosend hello dataströmmen toohello kanal.</span><span class="sxs-lookup"><span data-stu-id="527e0-122">hello ingest URL is used by hello live encoder toosend hello stream toohello Channel.</span></span>

4. <span data-ttu-id="527e0-123">Hämta hello kanalens förhandsgransknings-URL.</span><span class="sxs-lookup"><span data-stu-id="527e0-123">Retrieve hello Channel preview URL.</span></span>

    <span data-ttu-id="527e0-124">Använd den här URL: en tooverify att din kanal är tar emot hello direktsänd dataström.</span><span class="sxs-lookup"><span data-stu-id="527e0-124">Use this URL tooverify that your channel is properly receiving hello live stream.</span></span>

5. <span data-ttu-id="527e0-125">Skapa en tillgång.</span><span class="sxs-lookup"><span data-stu-id="527e0-125">Create an asset.</span></span>
6. <span data-ttu-id="527e0-126">Om du vill använda för hello tillgången toobe dynamiskt krypterad under uppspelningen hello följande:</span><span class="sxs-lookup"><span data-stu-id="527e0-126">If you want for hello asset toobe dynamically encrypted during playback, do hello following:</span></span>
7. <span data-ttu-id="527e0-127">Skapa en innehållsnyckel.</span><span class="sxs-lookup"><span data-stu-id="527e0-127">Create a content key.</span></span>
8. <span data-ttu-id="527e0-128">Konfigurera hello innehållsnyckelns auktoriseringsprincip.</span><span class="sxs-lookup"><span data-stu-id="527e0-128">Configure hello content key's authorization policy.</span></span>
9. <span data-ttu-id="527e0-129">Konfigurera en princip för tillgångsleveranser (används av dynamisk paketering och dynamisk kryptering).</span><span class="sxs-lookup"><span data-stu-id="527e0-129">Configure asset delivery policy (used by dynamic packaging and dynamic encryption).</span></span>
10. <span data-ttu-id="527e0-130">Skapa ett program och ange toouse hello tillgång som du skapade.</span><span class="sxs-lookup"><span data-stu-id="527e0-130">Create a program and specify toouse hello asset that you created.</span></span>
11. <span data-ttu-id="527e0-131">Publicera hello tillgången som är associerad med hello programmet genom att skapa en OnDemand-positionerare.</span><span class="sxs-lookup"><span data-stu-id="527e0-131">Publish hello asset associated with hello program by creating an OnDemand locator.</span></span>

    >[!NOTE]
    ><span data-ttu-id="527e0-132">När AMS-kontot skapas en **standard** strömningsslutpunkt har lagts till tooyour konto i hello **stoppad** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="527e0-132">When your AMS account is created a **default** streaming endpoint is added tooyour account in hello **Stopped** state.</span></span> <span data-ttu-id="527e0-133">Hej strömningsslutpunkt från vilken du vill att toostream innehåll har toobe i hello **kör** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="527e0-133">hello streaming endpoint from which you want toostream content has toobe in hello **Running** state.</span></span> 

12. <span data-ttu-id="527e0-134">Starta hello programmet när du är klar toostart strömning och arkivering.</span><span class="sxs-lookup"><span data-stu-id="527e0-134">Start hello program when you are ready toostart streaming and archiving.</span></span>
13. <span data-ttu-id="527e0-135">Du kan också kan hello livekodare vara signalerat toostart en annons.</span><span class="sxs-lookup"><span data-stu-id="527e0-135">Optionally, hello live encoder can be signaled toostart an advertisement.</span></span> <span data-ttu-id="527e0-136">hello annonsen infogas i utdataströmmen hello.</span><span class="sxs-lookup"><span data-stu-id="527e0-136">hello advertisement is inserted in hello output stream.</span></span>
14. <span data-ttu-id="527e0-137">Stoppa programmet hello när du vill toostop strömningen och arkiveringen hello händelsen.</span><span class="sxs-lookup"><span data-stu-id="527e0-137">Stop hello program whenever you want toostop streaming and archiving hello event.</span></span>
15. <span data-ttu-id="527e0-138">Ta bort hello programmet (och ta eventuellt bort tillgången hello).</span><span class="sxs-lookup"><span data-stu-id="527e0-138">Delete hello Program (and optionally delete hello asset).</span></span>

## <a name="what-youll-learn"></a><span data-ttu-id="527e0-139">Detta får du får lära dig</span><span class="sxs-lookup"><span data-stu-id="527e0-139">What you'll learn</span></span>
<span data-ttu-id="527e0-140">Det här avsnittet beskrivs hur du tooexecute olika åtgärder i kanaler och program med hjälp av Media Services .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="527e0-140">This topic shows you how tooexecute different operations on channels and programs using Media Services .NET SDK.</span></span> <span data-ttu-id="527e0-141">Eftersom många åtgärder är långvariga används .NET-API:er som hanterar långvariga åtgärder.</span><span class="sxs-lookup"><span data-stu-id="527e0-141">Because many operations are long-running .NET APIs that manage long running operations are used.</span></span>

<span data-ttu-id="527e0-142">Hej avsnittet visar hur toodo hello följande:</span><span class="sxs-lookup"><span data-stu-id="527e0-142">hello topic shows how toodo hello following:</span></span>

1. <span data-ttu-id="527e0-143">Skapa och starta en kanal.</span><span class="sxs-lookup"><span data-stu-id="527e0-143">Create and start a channel.</span></span> <span data-ttu-id="527e0-144">Långvariga API:er används.</span><span class="sxs-lookup"><span data-stu-id="527e0-144">Long-running APIs are used.</span></span>
2. <span data-ttu-id="527e0-145">Hämta infognings-hello kanaler (indataslutpunkten).</span><span class="sxs-lookup"><span data-stu-id="527e0-145">Get hello channels ingest (input) endpoint.</span></span> <span data-ttu-id="527e0-146">Den här slutpunkten ska tillhandahållas toohello kodare som kan skicka en direktsänd dataström med enkel bithastighet.</span><span class="sxs-lookup"><span data-stu-id="527e0-146">This endpoint should be provided toohello encoder that can send a single bitrate live stream.</span></span>
3. <span data-ttu-id="527e0-147">Hämta förhandsgranskningsslutpunkten hello.</span><span class="sxs-lookup"><span data-stu-id="527e0-147">Get hello preview endpoint.</span></span> <span data-ttu-id="527e0-148">Den här slutpunkten är används toopreview strömmen.</span><span class="sxs-lookup"><span data-stu-id="527e0-148">This endpoint is used toopreview your stream.</span></span>
4. <span data-ttu-id="527e0-149">Skapa en tillgång som kommer att använda toostore ditt innehåll.</span><span class="sxs-lookup"><span data-stu-id="527e0-149">Create an asset that will be used toostore your content.</span></span> <span data-ttu-id="527e0-150">hello principerna för tillgångsleverans ska också, som visas i det här exemplet konfigureras.</span><span class="sxs-lookup"><span data-stu-id="527e0-150">hello asset delivery policies should be configured as well, as shown in this example.</span></span>
5. <span data-ttu-id="527e0-151">Skapa ett program och ange toouse hello tillgång som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="527e0-151">Create a program and specify toouse hello asset that was created earlier.</span></span> <span data-ttu-id="527e0-152">Starta programmet hello.</span><span class="sxs-lookup"><span data-stu-id="527e0-152">Start hello program.</span></span> <span data-ttu-id="527e0-153">Långvariga API:er används.</span><span class="sxs-lookup"><span data-stu-id="527e0-153">Long-running APIs are used.</span></span>
6. <span data-ttu-id="527e0-154">Skapa en positionerare för tillgången hello, så hello innehållet publiceras och kan strömmas tooyour klienter.</span><span class="sxs-lookup"><span data-stu-id="527e0-154">Create a locator for hello asset, so hello content gets published and can be streamed tooyour clients.</span></span>
7. <span data-ttu-id="527e0-155">Visa eller dölj pekdatorer.</span><span class="sxs-lookup"><span data-stu-id="527e0-155">Show and hide slates.</span></span> <span data-ttu-id="527e0-156">Starta och stoppa annonser.</span><span class="sxs-lookup"><span data-stu-id="527e0-156">Start and stop advertisements.</span></span> <span data-ttu-id="527e0-157">Långvariga API:er används.</span><span class="sxs-lookup"><span data-stu-id="527e0-157">Long-running APIs are used.</span></span>
8. <span data-ttu-id="527e0-158">Rensa din kanal och alla hello associerade resurser.</span><span class="sxs-lookup"><span data-stu-id="527e0-158">Clean up your channel and all hello associated resources.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="527e0-159">Krav</span><span class="sxs-lookup"><span data-stu-id="527e0-159">Prerequisites</span></span>
<span data-ttu-id="527e0-160">hello följande är obligatoriska toocomplete hello kursen.</span><span class="sxs-lookup"><span data-stu-id="527e0-160">hello following are required toocomplete hello tutorial.</span></span>

* <span data-ttu-id="527e0-161">Ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="527e0-161">An Azure account.</span></span> <span data-ttu-id="527e0-162">Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="527e0-162">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="527e0-163">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="527e0-163">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span> <span data-ttu-id="527e0-164">Du får då krediter som kan vara används tootry ut betald Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="527e0-164">You get credits that can be used tootry out paid Azure services.</span></span> <span data-ttu-id="527e0-165">Även efter hello krediten är slut, kan du hålla hello-konto och använda kostnadsfria Azure-tjänster och funktioner, till exempel hello Web Apps i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="527e0-165">Even after hello credits are used up, you can keep hello account and use free Azure services and features, such as hello Web Apps feature in Azure App Service.</span></span>
* <span data-ttu-id="527e0-166">Ett Media Services-konto.</span><span class="sxs-lookup"><span data-stu-id="527e0-166">A Media Services account.</span></span> <span data-ttu-id="527e0-167">toocreate Media Services-konto finns [skapa konto](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="527e0-167">toocreate a Media Services account, see [Create Account](media-services-portal-create-account.md).</span></span>
* <span data-ttu-id="527e0-168">Visual Studio 2010 SP1 (Professional, Premium, Ultimate eller Express) eller senare versioner.</span><span class="sxs-lookup"><span data-stu-id="527e0-168">Visual Studio 2010 SP1 (Professional, Premium, Ultimate, or Express) or later versions.</span></span>
* <span data-ttu-id="527e0-169">Du måste använda Media Services .NET SDK version 3.2.0.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="527e0-169">You must use Media Services .NET SDK version 3.2.0.0 or newer.</span></span>
* <span data-ttu-id="527e0-170">En webbkamera och en kodare som kan skicka en direktsänd dataström i enkel bithastighet.</span><span class="sxs-lookup"><span data-stu-id="527e0-170">A webcam and an encoder that can send a single bitrate live stream.</span></span>

## <a name="considerations"></a><span data-ttu-id="527e0-171">Överväganden</span><span class="sxs-lookup"><span data-stu-id="527e0-171">Considerations</span></span>
* <span data-ttu-id="527e0-172">Hej max är för närvarande rekommenderas varaktighet för en direktsänd händelse 8 timmar.</span><span class="sxs-lookup"><span data-stu-id="527e0-172">Currently, hello max recommended duration of a live event is 8 hours.</span></span> <span data-ttu-id="527e0-173">Kontakta amslived på Microsoft.com om du behöver toorun en kanal under längre perioder.</span><span class="sxs-lookup"><span data-stu-id="527e0-173">Please contact amslived at Microsoft.com if you need toorun a Channel for longer periods of time.</span></span>
* <span data-ttu-id="527e0-174">Det finns en gräns på 1 000 000 principer för olika AMS-principer (till exempel för positionerarprincipen eller ContentKeyAuthorizationPolicy).</span><span class="sxs-lookup"><span data-stu-id="527e0-174">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="527e0-175">Du bör använda hello samma princip-ID om du alltid använder hello samma dagar / åtkomstbehörigheter, till exempel principer för lokaliserare som är avsedda tooremain på plats för lång tid (icke-överföringen principer).</span><span class="sxs-lookup"><span data-stu-id="527e0-175">You should use hello same policy ID if you are always using hello same days / access permissions, for example, policies for locators that are intended tooremain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="527e0-176">Mer information finns i [detta](media-services-dotnet-manage-entities.md#limit-access-policies) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="527e0-176">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>

## <a name="download-sample"></a><span data-ttu-id="527e0-177">Hämta exempel</span><span class="sxs-lookup"><span data-stu-id="527e0-177">Download sample</span></span>

<span data-ttu-id="527e0-178">Du kan hämta hello-exempel som beskrivs i det här avsnittet från [här](https://azure.microsoft.com/documentation/samples/media-services-dotnet-encode-live-stream-with-ams-clear/).</span><span class="sxs-lookup"><span data-stu-id="527e0-178">You can download hello sample that is described in this topic from [here](https://azure.microsoft.com/documentation/samples/media-services-dotnet-encode-live-stream-with-ams-clear/).</span></span>

## <a name="set-up-for-development-with-media-services-sdk-for-net"></a><span data-ttu-id="527e0-179">Konfigurera för utveckling med Media Services SDK för .NET</span><span class="sxs-lookup"><span data-stu-id="527e0-179">Set up for development with Media Services SDK for .NET</span></span>

<span data-ttu-id="527e0-180">Konfigurera utvecklingsmiljön och fylla hello app.config-fil med anslutningsinformation, enligt beskrivningen i [Media Services-utveckling med .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="527e0-180">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

## <a name="code-example"></a><span data-ttu-id="527e0-181">Kodexempel</span><span class="sxs-lookup"><span data-stu-id="527e0-181">Code example</span></span>

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

## <a name="next-step"></a><span data-ttu-id="527e0-182">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="527e0-182">Next step</span></span>
<span data-ttu-id="527e0-183">Granska sökvägarna för Media Services-utbildning.</span><span class="sxs-lookup"><span data-stu-id="527e0-183">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="527e0-184">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="527e0-184">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


