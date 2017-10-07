---
title: "aaaLive ström med lokala kodare med hjälp av hello Azure-portalen | Microsoft Docs"
description: "Den här självstudiekursen vägleder dig genom hello stegen för att skapa en kanal som är konfigurerad för en genomströmningsleverans."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 6f4acd95-cc64-4dd9-9e2d-8734707de326
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/09/2017
ms.author: juliako
ms.openlocfilehash: 1fb341e022f66f33903e13e07d3e84c0216cad77
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooperform-live-streaming-with-on-premises-encoders-using-hello-azure-portal"></a><span data-ttu-id="bd83c-103">Hur tooperform direktsänd strömning med lokala kodare med hjälp av hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="bd83c-103">How tooperform live streaming with on-premises encoders using hello Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="bd83c-104">Portal</span><span class="sxs-lookup"><span data-stu-id="bd83c-104">Portal</span></span>](media-services-portal-live-passthrough-get-started.md)
> * [<span data-ttu-id="bd83c-105">.NET</span><span class="sxs-lookup"><span data-stu-id="bd83c-105">.NET</span></span>](media-services-dotnet-live-encode-with-onpremises-encoders.md)
> * [<span data-ttu-id="bd83c-106">REST</span><span class="sxs-lookup"><span data-stu-id="bd83c-106">REST</span></span>](https://docs.microsoft.com/rest/api/media/operations/channel)
> 
> 

<span data-ttu-id="bd83c-107">Den här självstudiekursen vägleder dig genom hello stegen för att använda hello Azure portal toocreate en **kanal** som är konfigurerad för en genomströmningsleverans.</span><span class="sxs-lookup"><span data-stu-id="bd83c-107">This tutorial walks you through hello steps of using hello Azure portal toocreate a **Channel** that is configured for a pass-through delivery.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="bd83c-108">Krav</span><span class="sxs-lookup"><span data-stu-id="bd83c-108">Prerequisites</span></span>
<span data-ttu-id="bd83c-109">hello följande är obligatoriska toocomplete hello kursen:</span><span class="sxs-lookup"><span data-stu-id="bd83c-109">hello following are required toocomplete hello tutorial:</span></span>

* <span data-ttu-id="bd83c-110">Ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="bd83c-110">An Azure account.</span></span> <span data-ttu-id="bd83c-111">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="bd83c-111">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
* <span data-ttu-id="bd83c-112">Ett Media Services-konto.</span><span class="sxs-lookup"><span data-stu-id="bd83c-112">A Media Services account.</span></span> <span data-ttu-id="bd83c-113">toocreate Media Services-konto finns [hur tooCreate Media Services-konto](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="bd83c-113">toocreate a Media Services account, see [How tooCreate a Media Services Account](media-services-portal-create-account.md).</span></span>
* <span data-ttu-id="bd83c-114">En webbkamera.</span><span class="sxs-lookup"><span data-stu-id="bd83c-114">A webcam.</span></span> <span data-ttu-id="bd83c-115">Till exempel [Telestream Wirecast-kodaren](http://www.telestream.net/wirecast/overview.htm).</span><span class="sxs-lookup"><span data-stu-id="bd83c-115">For example, [Telestream Wirecast encoder](http://www.telestream.net/wirecast/overview.htm).</span></span>

<span data-ttu-id="bd83c-116">Det rekommenderas starkt tooreview hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="bd83c-116">It is highly recommended tooreview hello following articles:</span></span>

* [<span data-ttu-id="bd83c-117">Azure Media Services RTMP-support och live-kodare</span><span class="sxs-lookup"><span data-stu-id="bd83c-117">Azure Media Services RTMP Support and Live Encoders</span></span>](https://azure.microsoft.com/blog/2014/09/18/azure-media-services-rtmp-support-and-live-encoders/)
* [<span data-ttu-id="bd83c-118">Översikt över liveuppspelning med Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="bd83c-118">Overview of Live Steaming using Azure Media Services</span></span>](media-services-manage-channels-overview.md)
* [<span data-ttu-id="bd83c-119">Liveuppspelning med lokala kodare som skapar strömmar med flera bithastigheter</span><span class="sxs-lookup"><span data-stu-id="bd83c-119">Live streaming with on-premises encoders that create multi-bitrate streams</span></span>](media-services-live-streaming-with-onprem-encoders.md)

## <span data-ttu-id="bd83c-120"><a id="scenario"></a>Vanligt scenario för liveuppspelning</span><span class="sxs-lookup"><span data-stu-id="bd83c-120"><a id="scenario"></a>Common live streaming scenario</span></span>
<span data-ttu-id="bd83c-121">hello beskriver följande steg uppgifter som ingår i att skapa vanliga program för direktsänd strömning som använder kanaler som har konfigurerats för genomströmningsleverans.</span><span class="sxs-lookup"><span data-stu-id="bd83c-121">hello following steps describe tasks involved in creating common live streaming applications that use channels that are configured for pass-through delivery.</span></span> <span data-ttu-id="bd83c-122">Den här kursen visar hur toocreate och hanterar en genomströmningskanal och direktsända händelser.</span><span class="sxs-lookup"><span data-stu-id="bd83c-122">This tutorial shows how toocreate and manage a pass-through channel and live events.</span></span>

>[!NOTE]
><span data-ttu-id="bd83c-123">Se till att hello strömningsslutpunkt från vilken du vill att toostream innehåll hello **kör** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="bd83c-123">Make sure hello streaming endpoint from which you want toostream content is in hello **Running** state.</span></span> 
    
1. <span data-ttu-id="bd83c-124">Anslut en videokamera tooa dator.</span><span class="sxs-lookup"><span data-stu-id="bd83c-124">Connect a video camera tooa computer.</span></span> <span data-ttu-id="bd83c-125">Starta och konfigurera en lokal direktsänd kodare som matar ut en RTMP- eller fragmenterad MP4-dataström i multibithastighet.</span><span class="sxs-lookup"><span data-stu-id="bd83c-125">Launch and configure an on-premises live encoder that outputs a multi-bitrate RTMP or Fragmented MP4 stream.</span></span> <span data-ttu-id="bd83c-126">Mer information finns i [Support och direktsända kodare för Azure Media Services RTMP](http://go.microsoft.com/fwlink/?LinkId=532824).</span><span class="sxs-lookup"><span data-stu-id="bd83c-126">For more information, see [Azure Media Services RTMP Support and Live Encoders](http://go.microsoft.com/fwlink/?LinkId=532824).</span></span>
   
    <span data-ttu-id="bd83c-127">Det här steget kan också utföras när du har skapat din kanal.</span><span class="sxs-lookup"><span data-stu-id="bd83c-127">This step could also be performed after you create your Channel.</span></span>
2. <span data-ttu-id="bd83c-128">Skapa och starta en genomströmningskanal</span><span class="sxs-lookup"><span data-stu-id="bd83c-128">Create and start a pass-through Channel.</span></span>
3. <span data-ttu-id="bd83c-129">Hämta hello kanal infognings-URL.</span><span class="sxs-lookup"><span data-stu-id="bd83c-129">Retrieve hello Channel ingest URL.</span></span> 
   
    <span data-ttu-id="bd83c-130">hello infognings-URL som används av hello livekodaren toosend hello dataströmmen toohello kanal.</span><span class="sxs-lookup"><span data-stu-id="bd83c-130">hello ingest URL is used by hello live encoder toosend hello stream toohello Channel.</span></span>
4. <span data-ttu-id="bd83c-131">Hämta hello kanalens förhandsgransknings-URL.</span><span class="sxs-lookup"><span data-stu-id="bd83c-131">Retrieve hello Channel preview URL.</span></span> 
   
    <span data-ttu-id="bd83c-132">Använd den här URL: en tooverify att din kanal är tar emot hello direktsänd dataström.</span><span class="sxs-lookup"><span data-stu-id="bd83c-132">Use this URL tooverify that your channel is properly receiving hello live stream.</span></span>
5. <span data-ttu-id="bd83c-133">Skapa en direktsänd händelse eller ett direktsänt program.</span><span class="sxs-lookup"><span data-stu-id="bd83c-133">Create a live event/program.</span></span> 
   
    <span data-ttu-id="bd83c-134">När du använder hello Azure-portalen, skapar skapar en direktsänd händelse även en tillgång.</span><span class="sxs-lookup"><span data-stu-id="bd83c-134">When using hello Azure portal, creating a live event also creates an asset.</span></span> 

6. <span data-ttu-id="bd83c-135">Starta hello händelsen eller programmet när du är klar toostart strömning och arkivering.</span><span class="sxs-lookup"><span data-stu-id="bd83c-135">Start hello event/program when you are ready toostart streaming and archiving.</span></span>
7. <span data-ttu-id="bd83c-136">Du kan också kan hello livekodare vara signalerat toostart en annons.</span><span class="sxs-lookup"><span data-stu-id="bd83c-136">Optionally, hello live encoder can be signaled toostart an advertisement.</span></span> <span data-ttu-id="bd83c-137">hello annonsen infogas i utdataströmmen hello.</span><span class="sxs-lookup"><span data-stu-id="bd83c-137">hello advertisement is inserted in hello output stream.</span></span>
8. <span data-ttu-id="bd83c-138">Stoppa hello händelsen eller programmet när du vill toostop strömningen och arkiveringen hello händelsen.</span><span class="sxs-lookup"><span data-stu-id="bd83c-138">Stop hello event/program whenever you want toostop streaming and archiving hello event.</span></span>
9. <span data-ttu-id="bd83c-139">Ta bort hello händelsen eller programmet (och ta eventuellt bort tillgången hello).</span><span class="sxs-lookup"><span data-stu-id="bd83c-139">Delete hello event/program (and optionally delete hello asset).</span></span>     

> [!IMPORTANT]
> <span data-ttu-id="bd83c-140">Granska [direktsänd strömning med lokala kodare som skapar dataströmmar i multibithastighet](media-services-live-streaming-with-onprem-encoders.md) toolearn om koncept och överväganden relaterade toolive strömning med lokala kodare och genomströmningskanaler.</span><span class="sxs-lookup"><span data-stu-id="bd83c-140">Please review [Live streaming with on-premises encoders that create multi-bitrate streams](media-services-live-streaming-with-onprem-encoders.md) toolearn about concepts and considerations related toolive streaming with on-premises encoders and pass-through channels.</span></span>
> 
> 

## <a name="tooview-notifications-and-errors"></a><span data-ttu-id="bd83c-141">tooview meddelanden och fel</span><span class="sxs-lookup"><span data-stu-id="bd83c-141">tooview notifications and errors</span></span>
<span data-ttu-id="bd83c-142">Om du vill tooview meddelanden och fel som genereras av hello Azure-portalen, klicka på ikonen för hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="bd83c-142">If you want tooview notifications and errors produced by hello Azure portal, click on hello Notification icon.</span></span>

![Meddelanden](./media/media-services-portal-passthrough-get-started/media-services-notifications.png)

## <a name="create-and-start-pass-through-channels-and-events"></a><span data-ttu-id="bd83c-144">Skapa och starta genomströmningskanaler och händelser</span><span class="sxs-lookup"><span data-stu-id="bd83c-144">Create and start pass-through channels and events</span></span>
<span data-ttu-id="bd83c-145">En kanal är associerad med händelser och program som gör att du toocontrol hello publicering och lagring av segment i en direktsänd dataström.</span><span class="sxs-lookup"><span data-stu-id="bd83c-145">A channel is associated with events/programs that enable you toocontrol hello publishing and storage of segments in a live stream.</span></span> <span data-ttu-id="bd83c-146">Kanaler hanterar händelser.</span><span class="sxs-lookup"><span data-stu-id="bd83c-146">Channels manage events.</span></span> 

<span data-ttu-id="bd83c-147">Du kan ange hello antal timmar som du vill att tooretain hello registreras innehåll för programmet hello genom att ange hello **Arkivfönster** längd.</span><span class="sxs-lookup"><span data-stu-id="bd83c-147">You can specify hello number of hours you want tooretain hello recorded content for hello program by setting hello **Archive Window** length.</span></span> <span data-ttu-id="bd83c-148">Det här värdet kan anges från minst 5 minuter tooa högst 25 timmar.</span><span class="sxs-lookup"><span data-stu-id="bd83c-148">This value can be set from a minimum of 5 minutes tooa maximum of 25 hours.</span></span> <span data-ttu-id="bd83c-149">Längd avgör också hello hur lång tid som klienter kan söka bakåt i tiden från hello aktuella direktsända positionen.</span><span class="sxs-lookup"><span data-stu-id="bd83c-149">Archive window length also dictates hello maximum amount of time clients can seek back in time from hello current live position.</span></span> <span data-ttu-id="bd83c-150">Händelser kan köras under hello angiven tidsperiod, men innehåll som ligger bakom hello längd ignoreras kontinuerligt.</span><span class="sxs-lookup"><span data-stu-id="bd83c-150">Events can run over hello specified amount of time, but content that falls behind hello window length is continuously discarded.</span></span> <span data-ttu-id="bd83c-151">Det här värdet för den här egenskapen avgör också hur länge hello klient manifest kan växa.</span><span class="sxs-lookup"><span data-stu-id="bd83c-151">This value of this property also determines how long hello client manifests can grow.</span></span>

<span data-ttu-id="bd83c-152">Varje händelse är associerad till en tillgång.</span><span class="sxs-lookup"><span data-stu-id="bd83c-152">Each event is associated with an asset.</span></span> <span data-ttu-id="bd83c-153">toopublish hello-händelse, måste du skapa en positionerare för hello associerade tillgången.</span><span class="sxs-lookup"><span data-stu-id="bd83c-153">toopublish hello event, you must create an OnDemand locator for hello associated asset.</span></span> <span data-ttu-id="bd83c-154">Med den här positioneraren kan toobuild en strömnings-URL som du kan tillhandahålla tooyour klienter.</span><span class="sxs-lookup"><span data-stu-id="bd83c-154">Having this locator enables you toobuild a streaming URL that you can provide tooyour clients.</span></span>

<span data-ttu-id="bd83c-155">En kanal har stöd för upp toothree samtidigt med händelser så att du kan skapa flera Arkiv för hello samma inkommande dataström.</span><span class="sxs-lookup"><span data-stu-id="bd83c-155">A channel supports up toothree concurrently running events so you can create multiple archives of hello same incoming stream.</span></span> <span data-ttu-id="bd83c-156">Detta ger dig toopublish och arkivera olika delar av en händelse efter behov.</span><span class="sxs-lookup"><span data-stu-id="bd83c-156">This allows you toopublish and archive different parts of an event as needed.</span></span> <span data-ttu-id="bd83c-157">Till exempel är dina affärsbehov tooarchive 6 timmar av ett program, men toobroadcast sista 10 minuter.</span><span class="sxs-lookup"><span data-stu-id="bd83c-157">For example, your business requirement is tooarchive 6 hours of a program, but toobroadcast only last 10 minutes.</span></span> <span data-ttu-id="bd83c-158">tooaccomplish detta, behöver du toocreate två program som körs samtidigt.</span><span class="sxs-lookup"><span data-stu-id="bd83c-158">tooaccomplish this, you need toocreate two concurrently running programs.</span></span> <span data-ttu-id="bd83c-159">Ett program är tooarchive 6 timmar av händelsen hello men hello programmet publiceras inte.</span><span class="sxs-lookup"><span data-stu-id="bd83c-159">One program is set tooarchive 6 hours of hello event but hello program is not published.</span></span> <span data-ttu-id="bd83c-160">hello kan inte ange tooarchive i 10 minuter och det här programmet har publicerats.</span><span class="sxs-lookup"><span data-stu-id="bd83c-160">hello other program is set tooarchive for 10 minutes and this program is published.</span></span>

<span data-ttu-id="bd83c-161">Du bör inte återanvända befintliga direktsända händelser.</span><span class="sxs-lookup"><span data-stu-id="bd83c-161">You should not reuse existing live events.</span></span> <span data-ttu-id="bd83c-162">Skapa och starta istället en ny händelse för varje händelse.</span><span class="sxs-lookup"><span data-stu-id="bd83c-162">Instead, create and start a new event for each event.</span></span>

<span data-ttu-id="bd83c-163">Starta hello händelsen när du är klar toostart strömning och arkivering.</span><span class="sxs-lookup"><span data-stu-id="bd83c-163">Start hello event when you are ready toostart streaming and archiving.</span></span> <span data-ttu-id="bd83c-164">Stoppa programmet hello när du vill toostop strömningen och arkiveringen hello händelsen.</span><span class="sxs-lookup"><span data-stu-id="bd83c-164">Stop hello program whenever you want toostop streaming and archiving hello event.</span></span> 

<span data-ttu-id="bd83c-165">toodelete arkiverat innehåll, stoppa och ta bort hello händelsen och ta sedan bort hello associerade tillgången.</span><span class="sxs-lookup"><span data-stu-id="bd83c-165">toodelete archived content, stop and delete hello event and then delete hello associated asset.</span></span> <span data-ttu-id="bd83c-166">En tillgång kan inte tas bort om den används av en händelse; hello händelsen måste tas bort först.</span><span class="sxs-lookup"><span data-stu-id="bd83c-166">An asset cannot be deleted if it is used by an event; hello event must be deleted first.</span></span> 

<span data-ttu-id="bd83c-167">När du stoppar och ta bort hello händelsen hello användare skulle vara kan toostream ditt arkiverade innehåll som en video på begäran för så länge du inte ta bort hello tillgången.</span><span class="sxs-lookup"><span data-stu-id="bd83c-167">Even after you stop and delete hello event, hello users would be able toostream your archived content as a video on demand, for as long as you do not delete hello asset.</span></span>

<span data-ttu-id="bd83c-168">Om du vill tooretain hello arkiverat innehåll, men inte har den tillgänglig för strömning, tar du bort hello strömning lokaliserare.</span><span class="sxs-lookup"><span data-stu-id="bd83c-168">If you do want tooretain hello archived content, but not have it available for streaming, delete hello streaming locator.</span></span>

### <a name="toouse-hello-portal-toocreate-a-channel"></a><span data-ttu-id="bd83c-169">toouse hello portal toocreate en kanal</span><span class="sxs-lookup"><span data-stu-id="bd83c-169">toouse hello portal toocreate a channel</span></span>
<span data-ttu-id="bd83c-170">Det här avsnittet visas hur toouse hello **Snabbregistrering** alternativet toocreate en genomströmningskanal.</span><span class="sxs-lookup"><span data-stu-id="bd83c-170">This section shows how toouse hello **Quick Create** option toocreate a pass-through channel.</span></span>

<span data-ttu-id="bd83c-171">Mer information om genomströmningskanaler finns i [Liveuppspelning med lokala kodare som skapar dataströmmar i multibithastighet](media-services-live-streaming-with-onprem-encoders.md).</span><span class="sxs-lookup"><span data-stu-id="bd83c-171">For more details about pass-through channels, see [Live streaming with on-premises encoders that create multi-bitrate streams](media-services-live-streaming-with-onprem-encoders.md).</span></span>

1. <span data-ttu-id="bd83c-172">I hello [Azure-portalen](https://portal.azure.com/), Välj Azure Media Services-konto.</span><span class="sxs-lookup"><span data-stu-id="bd83c-172">In hello [Azure portal](https://portal.azure.com/), select your Azure Media Services account.</span></span>
2. <span data-ttu-id="bd83c-173">I hello **inställningar** -fönstret klickar du på **direktsänd strömning**.</span><span class="sxs-lookup"><span data-stu-id="bd83c-173">In hello **Settings** window, click **Live streaming**.</span></span> 
   
    ![Komma igång](./media/media-services-portal-passthrough-get-started/media-services-getting-started.png)
   
    <span data-ttu-id="bd83c-175">Hej **direktsänd strömning** visas.</span><span class="sxs-lookup"><span data-stu-id="bd83c-175">hello **Live streaming** window appears.</span></span>
3. <span data-ttu-id="bd83c-176">Klicka på **Snabbregistrering** toocreate en genomströmningskanal med hello RTMP-infogningsprotokollet.</span><span class="sxs-lookup"><span data-stu-id="bd83c-176">Click **Quick Create** toocreate a pass-through channel with hello RTMP ingest protocol.</span></span>
   
    <span data-ttu-id="bd83c-177">Hej **skapa en ny KANAL** visas.</span><span class="sxs-lookup"><span data-stu-id="bd83c-177">hello **CREATE A NEW CHANNEL** window appears.</span></span>
4. <span data-ttu-id="bd83c-178">Namnge hello ny kanal och klickar på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="bd83c-178">Give hello new channel a name and click **Create**.</span></span> 
   
    <span data-ttu-id="bd83c-179">Detta skapar en genomströmningskanal med hello RTMP-infogningsprotokollet.</span><span class="sxs-lookup"><span data-stu-id="bd83c-179">This creates a pass-through channel with hello RTMP ingest protocol.</span></span>

## <a name="create-events"></a><span data-ttu-id="bd83c-180">Skapa händelser</span><span class="sxs-lookup"><span data-stu-id="bd83c-180">Create events</span></span>
1. <span data-ttu-id="bd83c-181">Välj en kanal toowhich som du vill tooadd en händelse.</span><span class="sxs-lookup"><span data-stu-id="bd83c-181">Select a channel toowhich you want tooadd an event.</span></span>
2. <span data-ttu-id="bd83c-182">Tryck på knappen **Direktsänd händelse**.</span><span class="sxs-lookup"><span data-stu-id="bd83c-182">Press **Live Event** button.</span></span>

![Händelse](./media/media-services-portal-passthrough-get-started/media-services-create-events.png)

## <a name="get-ingest-urls"></a><span data-ttu-id="bd83c-184">Hämta infognings-URL:er</span><span class="sxs-lookup"><span data-stu-id="bd83c-184">Get ingest URLs</span></span>
<span data-ttu-id="bd83c-185">När hello kanalen har skapats kan du få infognings-URL: er som du kommer att ge toohello livekodaren.</span><span class="sxs-lookup"><span data-stu-id="bd83c-185">Once hello channel is created, you can get ingest URLs that you will provide toohello live encoder.</span></span> <span data-ttu-id="bd83c-186">hello kodaren använder dessa URL: er tooinput en direktsänd dataström.</span><span class="sxs-lookup"><span data-stu-id="bd83c-186">hello encoder uses these URLs tooinput a live stream.</span></span>

![Skapad](./media/media-services-portal-passthrough-get-started/media-services-channel-created.png)

## <a name="watch-hello-event"></a><span data-ttu-id="bd83c-188">Titta på hello händelse</span><span class="sxs-lookup"><span data-stu-id="bd83c-188">Watch hello event</span></span>
<span data-ttu-id="bd83c-189">toowatch hello-händelse, klickar du på **titta på** i hello Azure-portalen eller kopiera hello strömnings-URL och använder en valfri spelare önskat.</span><span class="sxs-lookup"><span data-stu-id="bd83c-189">toowatch hello event, click **Watch** in hello Azure portal or copy hello streaming URL and use a player of your choice.</span></span> 

![Skapad](./media/media-services-portal-passthrough-get-started/media-services-default-event.png)

<span data-ttu-id="bd83c-191">Direktsänd händelse automatiskt hämta innehåll för konverterade tooon begäran när stoppades.</span><span class="sxs-lookup"><span data-stu-id="bd83c-191">Live event automatically get converted tooon-demand content when stopped.</span></span>

## <a name="clean-up"></a><span data-ttu-id="bd83c-192">Rensa</span><span class="sxs-lookup"><span data-stu-id="bd83c-192">Clean up</span></span>
<span data-ttu-id="bd83c-193">Mer information om genomströmningskanaler finns i [Liveuppspelning med lokala kodare som skapar dataströmmar i multibithastighet](media-services-live-streaming-with-onprem-encoders.md).</span><span class="sxs-lookup"><span data-stu-id="bd83c-193">For more details about pass-through channels, see [Live streaming with on-premises encoders that create multi-bitrate streams](media-services-live-streaming-with-onprem-encoders.md).</span></span>

* <span data-ttu-id="bd83c-194">En kanal kan stoppas endast när alla händelser eller program på hello kanalen har stoppats.</span><span class="sxs-lookup"><span data-stu-id="bd83c-194">A channel can be stopped only when all events/programs on hello channel have been stopped.</span></span>  <span data-ttu-id="bd83c-195">När hello kanalen har stoppats kan det inga avgifter.</span><span class="sxs-lookup"><span data-stu-id="bd83c-195">Once hello Channel is stopped, it does not incur any charges.</span></span> <span data-ttu-id="bd83c-196">När du behöver toostart igen, det har hello samma infognings-URL så du inte behöver tooreconfigure din kodare.</span><span class="sxs-lookup"><span data-stu-id="bd83c-196">When you need toostart it again, it will have hello same ingest URL so you won't need tooreconfigure your encoder.</span></span>
* <span data-ttu-id="bd83c-197">En kanal kan tas bort bara när alla direktsända händelser i hello kanalen har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="bd83c-197">A channel can be deleted only when all live events on hello channel have been deleted.</span></span>

## <a name="view-archived-content"></a><span data-ttu-id="bd83c-198">Visa arkiverat innehåll</span><span class="sxs-lookup"><span data-stu-id="bd83c-198">View archived content</span></span>
<span data-ttu-id="bd83c-199">När du stoppar och ta bort hello händelsen hello användare skulle vara kan toostream ditt arkiverade innehåll som en video på begäran för så länge du inte ta bort hello tillgången.</span><span class="sxs-lookup"><span data-stu-id="bd83c-199">Even after you stop and delete hello event, hello users would be able toostream your archived content as a video on demand, for as long as you do not delete hello asset.</span></span> <span data-ttu-id="bd83c-200">En tillgång kan inte tas bort om den används av en händelse; hello händelsen måste tas bort först.</span><span class="sxs-lookup"><span data-stu-id="bd83c-200">An asset cannot be deleted if it is used by an event; hello event must be deleted first.</span></span> 

<span data-ttu-id="bd83c-201">toomanage dina tillgångar väljer **inställningen** och på **tillgångar**.</span><span class="sxs-lookup"><span data-stu-id="bd83c-201">toomanage your assets, select **Setting** and click **Assets**.</span></span>

![Tillgångar](./media/media-services-portal-passthrough-get-started/media-services-assets.png)

## <a name="next-step"></a><span data-ttu-id="bd83c-203">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bd83c-203">Next step</span></span>
<span data-ttu-id="bd83c-204">Granska sökvägarna för Media Services-utbildning.</span><span class="sxs-lookup"><span data-stu-id="bd83c-204">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="bd83c-205">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="bd83c-205">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

