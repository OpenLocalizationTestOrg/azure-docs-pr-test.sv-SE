---
title: "aaaConfigure hello grundämne Live-kodaren toosend en direktsänd dataström med enkel bithastighet | Microsoft Docs"
description: "Det här avsnittet visar hur hello tooconfigure grundämne Live-kodaren toosend en enkel bithastighet dataströmmen tooAMS kanaler som är aktiverade för live encoding."
services: media-services
documentationcenter: 
author: cenkdin
manager: cfowler
editor: 
ms.assetid: 9c6bf6a9-6273-4fdd-9477-f0e565280b5b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 01/05/2017
ms.author: cenkd;anilmur;juliako
ms.openlocfilehash: 9a5de6189bfb123768a9da038b8c8db69cf85e91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-elemental-live-encoder-toosend-a-single-bitrate-live-stream"></a><span data-ttu-id="196cb-103">Använd hello grundämne Live-kodaren toosend en direktsänd dataström med enkel bithastighet</span><span class="sxs-lookup"><span data-stu-id="196cb-103">Use hello Elemental Live encoder toosend a single bitrate live stream</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="196cb-104">Elemental Live</span><span class="sxs-lookup"><span data-stu-id="196cb-104">Elemental Live</span></span>](media-services-configure-elemental-live-encoder.md)
> * [<span data-ttu-id="196cb-105">Tricaster</span><span class="sxs-lookup"><span data-stu-id="196cb-105">Tricaster</span></span>](media-services-configure-tricaster-live-encoder.md)
> * [<span data-ttu-id="196cb-106">Wirecast</span><span class="sxs-lookup"><span data-stu-id="196cb-106">Wirecast</span></span>](media-services-configure-wirecast-live-encoder.md)
> * [<span data-ttu-id="196cb-107">FMLE</span><span class="sxs-lookup"><span data-stu-id="196cb-107">FMLE</span></span>](media-services-configure-fmle-live-encoder.md)
>
>

<span data-ttu-id="196cb-108">Det här avsnittet visar hur tooconfigure hello [grundämne Live](http://www.elementaltechnologies.com/products/elemental-live) kodare toosend enkel bithastighet strömma tooAMS kanaler som är aktiverade för live encoding.</span><span class="sxs-lookup"><span data-stu-id="196cb-108">This topic shows how tooconfigure hello [Elemental Live](http://www.elementaltechnologies.com/products/elemental-live) encoder toosend a single bitrate stream tooAMS channels that are enabled for live encoding.</span></span>  <span data-ttu-id="196cb-109">Mer information finns i [arbeta med kanaler som är aktiverad tooPerform Live Encoding med Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span><span class="sxs-lookup"><span data-stu-id="196cb-109">For more information, see [Working with Channels that are Enabled tooPerform Live Encoding with Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span></span>

<span data-ttu-id="196cb-110">Den här kursen visar hur toomanage Azure Media Services (AMS) med Azure Media Services Explorer (AMSE)-verktyget.</span><span class="sxs-lookup"><span data-stu-id="196cb-110">This tutorial shows how toomanage Azure Media Services (AMS) with Azure Media Services Explorer (AMSE) tool.</span></span> <span data-ttu-id="196cb-111">Det här verktyget körs bara på Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="196cb-111">This tool only runs on Windows PC.</span></span> <span data-ttu-id="196cb-112">Om du är på Mac- eller Linux använda hello Azure portal toocreate [kanaler](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) och [program](media-services-portal-creating-live-encoder-enabled-channel.md).</span><span class="sxs-lookup"><span data-stu-id="196cb-112">If you are on Mac or Linux, use hello Azure portal toocreate [channels](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) and [programs](media-services-portal-creating-live-encoder-enabled-channel.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="196cb-113">Krav</span><span class="sxs-lookup"><span data-stu-id="196cb-113">Prerequisites</span></span>
* <span data-ttu-id="196cb-114">Måste ha kunskaper för att använda grundämne Live web interface toocreate direktsända händelser.</span><span class="sxs-lookup"><span data-stu-id="196cb-114">Must have a working knowledge of using Elemental Live web interface toocreate live events.</span></span>
* [<span data-ttu-id="196cb-115">Skapa ett Azure Media Services-konto</span><span class="sxs-lookup"><span data-stu-id="196cb-115">Create an Azure Media Services account</span></span>](media-services-portal-create-account.md)
* <span data-ttu-id="196cb-116">Se till att det finns en Strömningsslutpunkt som körs.</span><span class="sxs-lookup"><span data-stu-id="196cb-116">Ensure there is a Streaming Endpoint running.</span></span> <span data-ttu-id="196cb-117">Mer information finns i [hanterar Strömningsslutpunkter i ett Media Services-konto](media-services-portal-manage-streaming-endpoints.md).</span><span class="sxs-lookup"><span data-stu-id="196cb-117">For more information, see [Manage Streaming Endpoints in a Media Services Account](media-services-portal-manage-streaming-endpoints.md).</span></span>
* <span data-ttu-id="196cb-118">Installera hello senaste versionen av hello [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) verktyget.</span><span class="sxs-lookup"><span data-stu-id="196cb-118">Install hello latest version of hello [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) tool.</span></span>
* <span data-ttu-id="196cb-119">Starta verktyget hello och ansluta tooyour AMS-konto.</span><span class="sxs-lookup"><span data-stu-id="196cb-119">Launch hello tool and connect tooyour AMS account.</span></span>

## <a name="tips"></a><span data-ttu-id="196cb-120">Tips</span><span class="sxs-lookup"><span data-stu-id="196cb-120">Tips</span></span>
* <span data-ttu-id="196cb-121">När det är möjligt använda ett inbyggt internet-anslutning.</span><span class="sxs-lookup"><span data-stu-id="196cb-121">Whenever possible, use a hardwired internet connection.</span></span>
* <span data-ttu-id="196cb-122">En bra tumregel samband med fastställandet av krav på bandbredd är toodouble hello strömning bithastighet.</span><span class="sxs-lookup"><span data-stu-id="196cb-122">A good rule of thumb when determining bandwidth requirements is toodouble hello streaming bitrates.</span></span> <span data-ttu-id="196cb-123">Detta är inte ett obligatoriskt krav, som det kan minska hello effekten av överbelastning.</span><span class="sxs-lookup"><span data-stu-id="196cb-123">While this is not a mandatory requirement, it will help mitigate hello impact of network congestion.</span></span>
* <span data-ttu-id="196cb-124">När du använder programvara baserat kodare, Stäng alla onödiga program.</span><span class="sxs-lookup"><span data-stu-id="196cb-124">When using software based encoders, close out any unnecessary programs.</span></span>

## <a name="elemental-live-with-rtp-ingest"></a><span data-ttu-id="196cb-125">Mata in grundämne Live med RTP</span><span class="sxs-lookup"><span data-stu-id="196cb-125">Elemental Live with RTP ingest</span></span>
<span data-ttu-id="196cb-126">Det här avsnittet visar hur tooconfigure hello grundämne Live kodare som skickar en enkel bithastighet direktsänd dataström över RTP.</span><span class="sxs-lookup"><span data-stu-id="196cb-126">This section shows how tooconfigure hello Elemental Live encoder that sends a single bitrate live stream over RTP.</span></span>  <span data-ttu-id="196cb-127">Mer information finns i [MPEG-TS dataströmmen över RTP](media-services-manage-live-encoder-enabled-channels.md#channel).</span><span class="sxs-lookup"><span data-stu-id="196cb-127">For more information, see [MPEG-TS stream over RTP](media-services-manage-live-encoder-enabled-channels.md#channel).</span></span>

### <a name="create-a-channel"></a><span data-ttu-id="196cb-128">Skapa en kanal</span><span class="sxs-lookup"><span data-stu-id="196cb-128">Create a channel</span></span>

1. <span data-ttu-id="196cb-129">Navigera i hello AMSE-verktyget toohello **Live** och högerklicka på inom området för hello-kanal.</span><span class="sxs-lookup"><span data-stu-id="196cb-129">In hello AMSE tool, navigate toohello **Live** tab, and right click within hello channel area.</span></span> <span data-ttu-id="196cb-130">Välj **skapa kanal...**</span><span class="sxs-lookup"><span data-stu-id="196cb-130">Select **Create channel…**</span></span> <span data-ttu-id="196cb-131">hello-menyn.</span><span class="sxs-lookup"><span data-stu-id="196cb-131">from hello menu.</span></span>

    ![Grundämne](./media/media-services-elemental-live-encoder/media-services-elemental1.png)

2. <span data-ttu-id="196cb-133">Ange ett Kanalnamn hello beskrivningsfältet är valfritt.</span><span class="sxs-lookup"><span data-stu-id="196cb-133">Specify a channel name, hello description field is optional.</span></span> <span data-ttu-id="196cb-134">Markera under inställningar för kanal **Standard** för hello Live Encoding alternativet med hello indata protokollet värdet för**RTP (MPEG-TS)**.</span><span class="sxs-lookup"><span data-stu-id="196cb-134">Under Channel Settings, select **Standard** for hello Live Encoding option, with hello Input Protocol set too**RTP (MPEG-TS)**.</span></span> <span data-ttu-id="196cb-135">Du kan lämna alla andra inställningar som är.</span><span class="sxs-lookup"><span data-stu-id="196cb-135">You can leave all other settings as is.</span></span>

    <span data-ttu-id="196cb-136">Se till att hello **Start hello ny kanal nu** är markerad.</span><span class="sxs-lookup"><span data-stu-id="196cb-136">Make sure hello **Start hello new channel now** is selected.</span></span>

3. <span data-ttu-id="196cb-137">Klicka på **skapa kanal**.</span><span class="sxs-lookup"><span data-stu-id="196cb-137">Click **Create Channel**.</span></span>

   ![Grundämne](./media/media-services-elemental-live-encoder/media-services-elemental12.png)

> [!NOTE]
> <span data-ttu-id="196cb-139">hello kanal kan ta upp till 20 minuter toostart.</span><span class="sxs-lookup"><span data-stu-id="196cb-139">hello channel can take as long as 20 minutes toostart.</span></span>
>
>

<span data-ttu-id="196cb-140">Medan hello kanal startar kan du [konfigurera hello kodare](media-services-configure-elemental-live-encoder.md#configure_elemental_rtp).</span><span class="sxs-lookup"><span data-stu-id="196cb-140">While hello channel is starting you can [configure hello encoder](media-services-configure-elemental-live-encoder.md#configure_elemental_rtp).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="196cb-141">Observera att faktureringen påbörjas så snart kanal blir klar att användas.</span><span class="sxs-lookup"><span data-stu-id="196cb-141">Note that billing starts as soon as Channel goes into a ready state.</span></span> <span data-ttu-id="196cb-142">Mer information finns i [kanalens tillstånd](media-services-manage-live-encoder-enabled-channels.md#states).</span><span class="sxs-lookup"><span data-stu-id="196cb-142">For more information, see [Channel's states](media-services-manage-live-encoder-enabled-channels.md#states).</span></span>
>
>

### <span data-ttu-id="196cb-143"><a id=configure_elemental_rtp></a>Konfigurera hello grundämne Live kodaren</span><span class="sxs-lookup"><span data-stu-id="196cb-143"><a id=configure_elemental_rtp></a>Configure hello Elemental Live encoder</span></span>
<span data-ttu-id="196cb-144">I den här självstudiekursen hello används följande inställningar för utdata.</span><span class="sxs-lookup"><span data-stu-id="196cb-144">In this tutorial hello following output settings are used.</span></span> <span data-ttu-id="196cb-145">hello resten av det här avsnittet beskriver konfigurationssteg i detalj.</span><span class="sxs-lookup"><span data-stu-id="196cb-145">hello rest of this section describes configuration steps in more detail.</span></span>

<span data-ttu-id="196cb-146">**Video**:</span><span class="sxs-lookup"><span data-stu-id="196cb-146">**Video**:</span></span>

* <span data-ttu-id="196cb-147">Codec: H.264</span><span class="sxs-lookup"><span data-stu-id="196cb-147">Codec: H.264</span></span>
* <span data-ttu-id="196cb-148">Profil: Hög (nivå 4.0)</span><span class="sxs-lookup"><span data-stu-id="196cb-148">Profile: High (Level 4.0)</span></span>
* <span data-ttu-id="196cb-149">Bithastighet: 5000 kbit/s</span><span class="sxs-lookup"><span data-stu-id="196cb-149">Bitrate: 5000 kbps</span></span>
* <span data-ttu-id="196cb-150">Keyframe: 2 sekunder (60 sekunder)</span><span class="sxs-lookup"><span data-stu-id="196cb-150">Keyframe: 2 seconds (60 seconds)</span></span>
* <span data-ttu-id="196cb-151">RAM-hastighet: 30</span><span class="sxs-lookup"><span data-stu-id="196cb-151">Frame Rate: 30</span></span>

<span data-ttu-id="196cb-152">**Ljud**:</span><span class="sxs-lookup"><span data-stu-id="196cb-152">**Audio**:</span></span>

* <span data-ttu-id="196cb-153">Codec: AAC (LC)</span><span class="sxs-lookup"><span data-stu-id="196cb-153">Codec: AAC (LC)</span></span>
* <span data-ttu-id="196cb-154">Bithastighet: 192 kbit/s</span><span class="sxs-lookup"><span data-stu-id="196cb-154">Bitrate: 192 kbps</span></span>
* <span data-ttu-id="196cb-155">Samplingsfrekvens: 44.1 kHz</span><span class="sxs-lookup"><span data-stu-id="196cb-155">Sample Rate: 44.1 kHz</span></span>

#### <a name="configuration-steps"></a><span data-ttu-id="196cb-156">Konfigurationssteg</span><span class="sxs-lookup"><span data-stu-id="196cb-156">Configuration steps</span></span>
1. <span data-ttu-id="196cb-157">Navigera toohello **grundämne Live** webbgränssnittet och ställa in hello kodare för **UDP/TS** strömning.</span><span class="sxs-lookup"><span data-stu-id="196cb-157">Navigate toohello **Elemental Live** web interface, and set up hello encoder for **UDP/TS** streaming.</span></span>
2. <span data-ttu-id="196cb-158">När du har skapat en ny händelse rulla nedåt toohello utdata grupper och Lägg till hello **UDP/TS** utdata grupp.</span><span class="sxs-lookup"><span data-stu-id="196cb-158">Once a new event is created, scroll down toohello output groups and add hello **UDP/TS** output group.</span></span>
3. <span data-ttu-id="196cb-159">Skapa en ny utdata genom att välja **ny ström** och sedan klicka på **lägga till utdata**.</span><span class="sxs-lookup"><span data-stu-id="196cb-159">Create a new output by selecting **New Stream** and then clicking **Add Output**.</span></span>  

    ![Grundämne](./media/media-services-elemental-live-encoder/media-services-elemental13.png)

   > [!NOTE]
   > <span data-ttu-id="196cb-161">Det rekommenderas att grundämne hello-händelsen har hello tidskod som angetts för ”systemklockan” toohelp hello kodare återansluta i hello fall av ett stream-fel.</span><span class="sxs-lookup"><span data-stu-id="196cb-161">It is recommended that hello Elemental event has hello timecode set too"System Clock" toohelp hello encoder reconnect in hello case of a stream failure.</span></span>
   >
   >
4. <span data-ttu-id="196cb-162">Nu när hello utdata har skapats, klicka på **lägga till ett flöde**.</span><span class="sxs-lookup"><span data-stu-id="196cb-162">Now that hello Output has been created, click **Add Stream**.</span></span> <span data-ttu-id="196cb-163">inställningar för hello utdata kan nu konfigureras.</span><span class="sxs-lookup"><span data-stu-id="196cb-163">hello output settings can now be configured.</span></span>
5. <span data-ttu-id="196cb-164">Rulla nedåt toohello ”dataströmmen 1” just har skapat, klicka på hello **Video** fliken hello vänster och expanderar hello **Avancerat** inställningar.</span><span class="sxs-lookup"><span data-stu-id="196cb-164">Scroll down toohello "Stream 1" that was just created, click hello **Video** tab on hello left and expand hello **Advanced** settings section.</span></span>

    ![Grundämne](./media/media-services-elemental-live-encoder/media-services-elemental4.png)

    <span data-ttu-id="196cb-166">Medan grundämne Live har ett stort antal tillgängliga anpassa, hello följande inställningar rekommenderas för att komma igång med direktuppspelning tooAMS.</span><span class="sxs-lookup"><span data-stu-id="196cb-166">While Elemental Live has a wide range of available customizing, hello following settings are recommended for getting started with streaming tooAMS.</span></span>

   * <span data-ttu-id="196cb-167">Lösning: minst 1 280 x 720</span><span class="sxs-lookup"><span data-stu-id="196cb-167">Resolution: 1280 x 720</span></span>
   * <span data-ttu-id="196cb-168">Ramhastighet: 30</span><span class="sxs-lookup"><span data-stu-id="196cb-168">Framerate: 30</span></span>
   * <span data-ttu-id="196cb-169">GOP storlek: 60 ramar</span><span class="sxs-lookup"><span data-stu-id="196cb-169">GOP Size: 60 frames</span></span>
   * <span data-ttu-id="196cb-170">Sammanfläta läge: progressiv</span><span class="sxs-lookup"><span data-stu-id="196cb-170">Interlace Mode: Progressive</span></span>
   * <span data-ttu-id="196cb-171">Bithastighet: 5000000 bit/s (Detta kan ställas in baserat på begränsningar)</span><span class="sxs-lookup"><span data-stu-id="196cb-171">Bitrate: 5000000 bit/s (This can be adjusted based on network limitations)</span></span>

    ![Grundämne](./media/media-services-elemental-live-encoder/media-services-elemental5.png)

1. <span data-ttu-id="196cb-173">Hämta hello kanal indata-URL.</span><span class="sxs-lookup"><span data-stu-id="196cb-173">Get hello channel's input URL.</span></span>

    <span data-ttu-id="196cb-174">Gå tillbaka toohello AMSE-verktyget och kontrollera status för slutförande av hello-kanal.</span><span class="sxs-lookup"><span data-stu-id="196cb-174">Navigate back toohello AMSE tool, and check on hello channel completion status.</span></span> <span data-ttu-id="196cb-175">När hello tillstånd har ändrats från **Start** för**kör**, du kan hämta hello indata-URL.</span><span class="sxs-lookup"><span data-stu-id="196cb-175">Once hello State has changed from **Starting** too**Running**, you can get hello input URL.</span></span>

    <span data-ttu-id="196cb-176">När du kör hello kanal högerklickar du på hello Kanalnamn, Navigera ned toohover över **kopiera indata URL tooclipboard** och välj sedan **primära indata-URL**.</span><span class="sxs-lookup"><span data-stu-id="196cb-176">When hello channel is running, right click hello channel name, navigate down toohover over **Copy Input URL tooclipboard** and then select **Primary Input  URL**.</span></span>  

    ![Grundämne](./media/media-services-elemental-live-encoder/media-services-elemental6.png)
2. <span data-ttu-id="196cb-178">Klistra in informationen i hello **primära mål** i hello Elemental.</span><span class="sxs-lookup"><span data-stu-id="196cb-178">Paste this information in hello **Primary Destination** field of hello Elemental.</span></span> <span data-ttu-id="196cb-179">Alla andra inställningar kan förbli hello standard.</span><span class="sxs-lookup"><span data-stu-id="196cb-179">All other settings can remain hello default.</span></span>

    ![Grundämne](./media/media-services-elemental-live-encoder/media-services-elemental14.png)

    <span data-ttu-id="196cb-181">Upprepa dessa steg med hello sekundära indata-URL genom att skapa en separat flik ”utdata” för direktuppspelning av UDP/TS för extra redundans.</span><span class="sxs-lookup"><span data-stu-id="196cb-181">For extra redundancy, repeat these steps with hello Secondary Input URL by creating a separate "Output" tab for UDP/TS Streaming.</span></span>
3. <span data-ttu-id="196cb-182">Klicka på **skapa** (om en ny händelse skapades) eller **uppdatering** (om du redigerar en befintlig händelse) och gå sedan vidare toostart hello kodare.</span><span class="sxs-lookup"><span data-stu-id="196cb-182">Click **Create** (if a new event was created) or **Update** (if editing a pre-existing event) and then proceed toostart hello encoder.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="196cb-183">Innan du klickar på **starta** på grundämne Live Webbgränssnitt för hello du **måste** se till att hello kanalen är klar.</span><span class="sxs-lookup"><span data-stu-id="196cb-183">Before you click **Start** on hello Elemental Live web interface, you **must** ensure that hello Channel is ready.</span></span>
> <span data-ttu-id="196cb-184">Kontrollera också att inte tooleave hello kanal i klar att användas utan en händelse för > 15 minuter.</span><span class="sxs-lookup"><span data-stu-id="196cb-184">Also, make sure not tooleave hello Channel in a ready state without an event for longer than > 15 minutes.</span></span>
>
>

<span data-ttu-id="196cb-185">När hello dataströmmen har körts i 30 sekunder, gå tillbaka toohello AMSE-verktyget och testa uppspelning.</span><span class="sxs-lookup"><span data-stu-id="196cb-185">After hello stream has been running for 30 seconds, navigate back toohello AMSE tool and test playback.</span></span>  

### <a name="test-playback"></a><span data-ttu-id="196cb-186">Testa uppspelning</span><span class="sxs-lookup"><span data-stu-id="196cb-186">Test playback</span></span>

<span data-ttu-id="196cb-187">Navigera toohello AMSE-verktyget och högerklicka på hello kanal toobe testas.</span><span class="sxs-lookup"><span data-stu-id="196cb-187">Navigate toohello AMSE tool, and right click hello channel toobe tested.</span></span> <span data-ttu-id="196cb-188">Hovra över hello-menyn **uppspelning hello Preview** och välj **med Azure Media Player**.</span><span class="sxs-lookup"><span data-stu-id="196cb-188">From hello menu, hover over **Playback hello Preview** and select **with Azure Media Player**.</span></span>  

    ![Elemental](./media/media-services-elemental-live-encoder/media-services-elemental8.png)

<span data-ttu-id="196cb-189">Om hello dataströmmen visas i hello player har hello kodare varit korrekt konfigurerade tooconnect tooAMS.</span><span class="sxs-lookup"><span data-stu-id="196cb-189">If hello stream appears in hello player, then hello encoder has been properly configured tooconnect tooAMS.</span></span>

<span data-ttu-id="196cb-190">Om ett fel tas emot måste hello kanal toobe Återställ kodare inställningar och justeras.</span><span class="sxs-lookup"><span data-stu-id="196cb-190">If an error is received, hello channel will need toobe reset and encoder settings adjusted.</span></span> <span data-ttu-id="196cb-191">Se hello [felsökning](media-services-troubleshooting-live-streaming.md) avsnittet som vägledning.</span><span class="sxs-lookup"><span data-stu-id="196cb-191">Please see hello [troubleshooting](media-services-troubleshooting-live-streaming.md) topic for guidance.</span></span>   

### <a name="create-a-program"></a><span data-ttu-id="196cb-192">Skapa ett program</span><span class="sxs-lookup"><span data-stu-id="196cb-192">Create a program</span></span>
1. <span data-ttu-id="196cb-193">När kanalen uppspelning bekräftas, skapa ett program.</span><span class="sxs-lookup"><span data-stu-id="196cb-193">Once channel playback is confirmed, create a program.</span></span> <span data-ttu-id="196cb-194">Under hello **Live** i hello AMSE-verktyget, högerklickar du i området för hello-programmet och välj **Skapa nytt Program**.</span><span class="sxs-lookup"><span data-stu-id="196cb-194">Under hello **Live** tab in hello AMSE tool, right click within hello program area and select **Create New Program**.</span></span>  

    ![Grundämne](./media/media-services-elemental-live-encoder/media-services-elemental9.png)
2. <span data-ttu-id="196cb-196">Namnge programmet hello och, om det behövs, justera hello **längd** (som standard too4 timmar).</span><span class="sxs-lookup"><span data-stu-id="196cb-196">Name hello program and, if needed, adjust hello **Archive Window Length** (which defaults too4 hours).</span></span> <span data-ttu-id="196cb-197">Du kan också ange en lagringsplats eller lämna som hello standard.</span><span class="sxs-lookup"><span data-stu-id="196cb-197">You can also specify a storage location or leave as hello default.</span></span>  
3. <span data-ttu-id="196cb-198">Kontrollera hello **Start hello programmet nu** rutan.</span><span class="sxs-lookup"><span data-stu-id="196cb-198">Check hello **Start hello Program now** box.</span></span>
4. <span data-ttu-id="196cb-199">Klicka på **skapa Program**.</span><span class="sxs-lookup"><span data-stu-id="196cb-199">Click **Create Program**.</span></span>  

    >[!NOTE]
    > <span data-ttu-id="196cb-200">Skapa en program tar mindre tid än att skapa en kanal.</span><span class="sxs-lookup"><span data-stu-id="196cb-200">Program creation takes less time than channel creation.</span></span>   
      
5. <span data-ttu-id="196cb-201">När hello programmet körs kan bekräfta uppspelning genom att högerklicka på programmet hello och navigera för**uppspelning hello program** och sedan välja **med Azure Media Player**.</span><span class="sxs-lookup"><span data-stu-id="196cb-201">Once hello program is running, confirm playback by right clicking hello program and navigating too**Playback hello program(s)** and then selecting **with Azure Media Player**.</span></span>  
6. <span data-ttu-id="196cb-202">När bekräftat, högerklicka på programmet hello igen och välj **kopiera hello utdata URL tooClipboard** (eller hämta den här informationen från hello **programmet information och inställningar** alternativet hello-menyn).</span><span class="sxs-lookup"><span data-stu-id="196cb-202">Once confirmed, right click hello program again and select **Copy hello Output URL tooClipboard** (or retrieve this information from hello **Program information and settings** option from hello menu).</span></span>

<span data-ttu-id="196cb-203">hello är nu redo toobe inbäddat i en spelare eller distribuerade tooan målgruppen för live visning.</span><span class="sxs-lookup"><span data-stu-id="196cb-203">hello stream is now ready toobe embedded in a player, or distributed tooan audience for live viewing.</span></span>  

## <a name="troubleshooting"></a><span data-ttu-id="196cb-204">Felsökning</span><span class="sxs-lookup"><span data-stu-id="196cb-204">Troubleshooting</span></span>
<span data-ttu-id="196cb-205">Se hello [felsökning](media-services-troubleshooting-live-streaming.md) avsnittet som vägledning.</span><span class="sxs-lookup"><span data-stu-id="196cb-205">Please see hello [troubleshooting](media-services-troubleshooting-live-streaming.md) topic for guidance.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="196cb-206">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="196cb-206">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="196cb-207">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="196cb-207">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
