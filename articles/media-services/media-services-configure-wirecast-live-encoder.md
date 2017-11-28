---
title: "aaaConfigure hello Telestream Wirecast-kodaren toosend en direktsänd dataström med enkel bithastighet | Microsoft Docs"
description: "Det här avsnittet visar hur live tooconfigure hello Wirecast kodare toosend en enkel bithastighet dataströmmen tooAMS kanaler som är aktiverade för live encoding. "
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 0d2f1e81-51a6-4ca9-894a-6dfa51ce4c70
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 01/05/2017
ms.author: juliako;cenkdin;anilmur
ms.openlocfilehash: e373f6c08232c652e65db584ded409c405d8cffe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-wirecast-encoder-toosend-a-single-bitrate-live-stream"></a><span data-ttu-id="397e8-103">Använd hello Wirecast-kodaren toosend en direktsänd dataström med enkel bithastighet</span><span class="sxs-lookup"><span data-stu-id="397e8-103">Use hello Wirecast encoder toosend a single bitrate live stream</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="397e8-104">Wirecast</span><span class="sxs-lookup"><span data-stu-id="397e8-104">Wirecast</span></span>](media-services-configure-wirecast-live-encoder.md)
> * [<span data-ttu-id="397e8-105">Elemental Live</span><span class="sxs-lookup"><span data-stu-id="397e8-105">Elemental Live</span></span>](media-services-configure-elemental-live-encoder.md)
> * [<span data-ttu-id="397e8-106">Tricaster</span><span class="sxs-lookup"><span data-stu-id="397e8-106">Tricaster</span></span>](media-services-configure-tricaster-live-encoder.md)
> * [<span data-ttu-id="397e8-107">FMLE</span><span class="sxs-lookup"><span data-stu-id="397e8-107">FMLE</span></span>](media-services-configure-fmle-live-encoder.md)
>
>

<span data-ttu-id="397e8-108">Det här avsnittet visar hur tooconfigure hello [Telestream Wirecast](http://www.telestream.net/wirecast/overview.htm) live kodare toosend en enkel bithastighet dataströmmen tooAMS kanaler som är aktiverade för live encoding.</span><span class="sxs-lookup"><span data-stu-id="397e8-108">This topic shows how tooconfigure hello [Telestream Wirecast](http://www.telestream.net/wirecast/overview.htm) live encoder toosend a single bitrate stream tooAMS channels that are enabled for live encoding.</span></span>  <span data-ttu-id="397e8-109">Mer information finns i [arbeta med kanaler som är aktiverad tooPerform Live Encoding med Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span><span class="sxs-lookup"><span data-stu-id="397e8-109">For more information, see [Working with Channels that are Enabled tooPerform Live Encoding with Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span></span>

<span data-ttu-id="397e8-110">Den här kursen visar hur toomanage Azure Media Services (AMS) med Azure Media Services Explorer (AMSE)-verktyget.</span><span class="sxs-lookup"><span data-stu-id="397e8-110">This tutorial shows how toomanage Azure Media Services (AMS) with Azure Media Services Explorer (AMSE) tool.</span></span> <span data-ttu-id="397e8-111">Det här verktyget körs bara på Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="397e8-111">This tool only runs on Windows PC.</span></span> <span data-ttu-id="397e8-112">Om du är på Mac- eller Linux använda hello Azure portal toocreate [kanaler](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) och [program](media-services-portal-creating-live-encoder-enabled-channel.md).</span><span class="sxs-lookup"><span data-stu-id="397e8-112">If you are on Mac or Linux, use hello Azure portal toocreate [channels](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) and [programs](media-services-portal-creating-live-encoder-enabled-channel.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="397e8-113">Krav</span><span class="sxs-lookup"><span data-stu-id="397e8-113">Prerequisites</span></span>
* [<span data-ttu-id="397e8-114">Skapa ett Azure Media Services-konto</span><span class="sxs-lookup"><span data-stu-id="397e8-114">Create an Azure Media Services account</span></span>](media-services-portal-create-account.md)
* <span data-ttu-id="397e8-115">Se till att det finns en Strömningsslutpunkt som körs.</span><span class="sxs-lookup"><span data-stu-id="397e8-115">Ensure there is a Streaming Endpoint running.</span></span> <span data-ttu-id="397e8-116">Mer information finns i [hanterar Strömningsslutpunkter i ett Media Services-konto](media-services-portal-manage-streaming-endpoints.md)</span><span class="sxs-lookup"><span data-stu-id="397e8-116">For more information, see [Manage Streaming Endpoints in a Media Services Account](media-services-portal-manage-streaming-endpoints.md)</span></span>
* <span data-ttu-id="397e8-117">Installera hello senaste versionen av hello [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) verktyget.</span><span class="sxs-lookup"><span data-stu-id="397e8-117">Install hello latest version of hello [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) tool.</span></span>
* <span data-ttu-id="397e8-118">Starta verktyget hello och ansluta tooyour AMS-konto.</span><span class="sxs-lookup"><span data-stu-id="397e8-118">Launch hello tool and connect tooyour AMS account.</span></span>

## <a name="tips"></a><span data-ttu-id="397e8-119">Tips</span><span class="sxs-lookup"><span data-stu-id="397e8-119">Tips</span></span>
* <span data-ttu-id="397e8-120">När det är möjligt använda ett inbyggt internet-anslutning.</span><span class="sxs-lookup"><span data-stu-id="397e8-120">Whenever possible, use a hardwired internet connection.</span></span>
* <span data-ttu-id="397e8-121">En bra tumregel samband med fastställandet av krav på bandbredd är toodouble hello strömning bithastighet.</span><span class="sxs-lookup"><span data-stu-id="397e8-121">A good rule of thumb when determining bandwidth requirements is toodouble hello streaming bitrates.</span></span> <span data-ttu-id="397e8-122">Detta är inte ett obligatoriskt krav, som det kan minska hello effekten av överbelastning.</span><span class="sxs-lookup"><span data-stu-id="397e8-122">While this is not a mandatory requirement, it will help mitigate hello impact of network congestion.</span></span>
* <span data-ttu-id="397e8-123">När du använder programvara baserat kodare, Stäng alla onödiga program.</span><span class="sxs-lookup"><span data-stu-id="397e8-123">When using software based encoders, close out any unnecessary programs.</span></span>

## <a name="create-a-channel"></a><span data-ttu-id="397e8-124">Skapa en kanal</span><span class="sxs-lookup"><span data-stu-id="397e8-124">Create a channel</span></span>
1. <span data-ttu-id="397e8-125">Navigera i hello AMSE-verktyget toohello **Live** och högerklicka på inom området för hello-kanal.</span><span class="sxs-lookup"><span data-stu-id="397e8-125">In hello AMSE tool, navigate toohello **Live** tab, and right click within hello channel area.</span></span> <span data-ttu-id="397e8-126">Välj **skapa kanal...**</span><span class="sxs-lookup"><span data-stu-id="397e8-126">Select **Create channel…**</span></span> <span data-ttu-id="397e8-127">hello-menyn.</span><span class="sxs-lookup"><span data-stu-id="397e8-127">from hello menu.</span></span>

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast1.png)

2. <span data-ttu-id="397e8-129">Ange ett Kanalnamn hello beskrivningsfältet är valfritt.</span><span class="sxs-lookup"><span data-stu-id="397e8-129">Specify a channel name, hello description field is optional.</span></span> <span data-ttu-id="397e8-130">Markera under inställningar för kanal **Standard** för hello Live Encoding alternativet med hello indata protokollet värdet för**RTMP**.</span><span class="sxs-lookup"><span data-stu-id="397e8-130">Under Channel Settings, select **Standard** for hello Live Encoding option, with hello Input Protocol set too**RTMP**.</span></span> <span data-ttu-id="397e8-131">Du kan lämna alla andra inställningar som är.</span><span class="sxs-lookup"><span data-stu-id="397e8-131">You can leave all other settings as is.</span></span>

    <span data-ttu-id="397e8-132">Se till att hello **Start hello ny kanal nu** är markerad.</span><span class="sxs-lookup"><span data-stu-id="397e8-132">Make sure hello **Start hello new channel now** is selected.</span></span>

3. <span data-ttu-id="397e8-133">Klicka på **skapa kanal**.</span><span class="sxs-lookup"><span data-stu-id="397e8-133">Click **Create Channel**.</span></span>

   ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast2.png)

> [!NOTE]
> <span data-ttu-id="397e8-135">hello kanal kan ta upp till 20 minuter toostart.</span><span class="sxs-lookup"><span data-stu-id="397e8-135">hello channel can take as long as 20 minutes toostart.</span></span>
>
>

<span data-ttu-id="397e8-136">Medan hello kanal startar kan du [konfigurera hello kodare](media-services-configure-wirecast-live-encoder.md#configure_wirecast_rtmp).</span><span class="sxs-lookup"><span data-stu-id="397e8-136">While hello channel is starting you can [configure hello encoder](media-services-configure-wirecast-live-encoder.md#configure_wirecast_rtmp).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="397e8-137">Observera att faktureringen påbörjas så snart kanal blir klar att användas.</span><span class="sxs-lookup"><span data-stu-id="397e8-137">Note that billing starts as soon as Channel goes into a ready state.</span></span> <span data-ttu-id="397e8-138">Mer information finns i [kanalens tillstånd](media-services-manage-live-encoder-enabled-channels.md#states).</span><span class="sxs-lookup"><span data-stu-id="397e8-138">For more information, see [Channel's states](media-services-manage-live-encoder-enabled-channels.md#states).</span></span>
>
>

## <span data-ttu-id="397e8-139"><a id=configure_wirecast_rtmp></a>Konfigurera hello Telestream Wirecast-kodaren</span><span class="sxs-lookup"><span data-stu-id="397e8-139"><a id=configure_wirecast_rtmp></a>Configure hello Telestream Wirecast encoder</span></span>
<span data-ttu-id="397e8-140">I den här självstudiekursen hello används följande inställningar för utdata.</span><span class="sxs-lookup"><span data-stu-id="397e8-140">In this tutorial hello following output settings are used.</span></span> <span data-ttu-id="397e8-141">hello resten av det här avsnittet beskriver konfigurationssteg i detalj.</span><span class="sxs-lookup"><span data-stu-id="397e8-141">hello rest of this section describes configuration steps in more detail.</span></span>

<span data-ttu-id="397e8-142">**Video**:</span><span class="sxs-lookup"><span data-stu-id="397e8-142">**Video**:</span></span>

* <span data-ttu-id="397e8-143">Codec: H.264</span><span class="sxs-lookup"><span data-stu-id="397e8-143">Codec: H.264</span></span>
* <span data-ttu-id="397e8-144">Profil: Hög (nivå 4.0)</span><span class="sxs-lookup"><span data-stu-id="397e8-144">Profile: High (Level 4.0)</span></span>
* <span data-ttu-id="397e8-145">Bithastighet: 5000 kbit/s</span><span class="sxs-lookup"><span data-stu-id="397e8-145">Bitrate: 5000 kbps</span></span>
* <span data-ttu-id="397e8-146">Keyframe: 2 sekunder (60 sekunder)</span><span class="sxs-lookup"><span data-stu-id="397e8-146">Keyframe: 2 seconds (60 seconds)</span></span>
* <span data-ttu-id="397e8-147">RAM-hastighet: 30</span><span class="sxs-lookup"><span data-stu-id="397e8-147">Frame Rate: 30</span></span>

<span data-ttu-id="397e8-148">**Ljud**:</span><span class="sxs-lookup"><span data-stu-id="397e8-148">**Audio**:</span></span>

* <span data-ttu-id="397e8-149">Codec: AAC (LC)</span><span class="sxs-lookup"><span data-stu-id="397e8-149">Codec: AAC (LC)</span></span>
* <span data-ttu-id="397e8-150">Bithastighet: 192 kbit/s</span><span class="sxs-lookup"><span data-stu-id="397e8-150">Bitrate: 192 kbps</span></span>
* <span data-ttu-id="397e8-151">Samplingsfrekvens: 44.1 kHz</span><span class="sxs-lookup"><span data-stu-id="397e8-151">Sample Rate: 44.1 kHz</span></span>

### <a name="configuration-steps"></a><span data-ttu-id="397e8-152">Konfigurationssteg</span><span class="sxs-lookup"><span data-stu-id="397e8-152">Configuration steps</span></span>
1. <span data-ttu-id="397e8-153">Öppna hello Telestream Wirecast programmet hello datorn som används och konfigurera för RTMP strömning.</span><span class="sxs-lookup"><span data-stu-id="397e8-153">Open hello Telestream Wirecast application on hello machine being used, and set up for RTMP streaming.</span></span>
2. <span data-ttu-id="397e8-154">Konfigurera hello utdata genom att gå toohello **utdata** fliken och markera **utdata inställningar...** .</span><span class="sxs-lookup"><span data-stu-id="397e8-154">Configure hello output by navigating toohello **Output** tab and selecting **Output Settings…**.</span></span>

    <span data-ttu-id="397e8-155">Se till att hello **utdata mål** har angetts för**RTMP-Server**.</span><span class="sxs-lookup"><span data-stu-id="397e8-155">Make sure hello **Output Destination** is set too**RTMP Server**.</span></span>
3. <span data-ttu-id="397e8-156">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="397e8-156">Click **OK**.</span></span>
4. <span data-ttu-id="397e8-157">Ange hello hello på sidan Inställningar **mål** fältet toobe **Azure Media Services**.</span><span class="sxs-lookup"><span data-stu-id="397e8-157">On hello settings page, set hello **Destination** field toobe **Azure Media Services**.</span></span>

    <span data-ttu-id="397e8-158">hello kodning profil är markerat för**Azure H.264 720 p 16:9 (minst 1 280 x 720)**.</span><span class="sxs-lookup"><span data-stu-id="397e8-158">hello Encoding profile is pre-selected too**Azure H.264 720p 16:9 (1280x720)**.</span></span> <span data-ttu-id="397e8-159">toocustomize dessa inställningar väljer hello växeln ikonen toohello höger i hello listrutan och välj sedan **nya förinställningen**.</span><span class="sxs-lookup"><span data-stu-id="397e8-159">toocustomize these settings, select hello gear icon toohello right of hello drop down, and then choose **New Preset**.</span></span>

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast3.png)
5. <span data-ttu-id="397e8-161">Konfigurera kodare förinställningar.</span><span class="sxs-lookup"><span data-stu-id="397e8-161">Configure encoder presets.</span></span>

    <span data-ttu-id="397e8-162">Namnet hello förinställningen och söka efter hello följande rekommenderade inställningar:</span><span class="sxs-lookup"><span data-stu-id="397e8-162">Name hello preset, and check for hello following recommended settings:</span></span>

    <span data-ttu-id="397e8-163">**Video**</span><span class="sxs-lookup"><span data-stu-id="397e8-163">**Video**</span></span>

   * <span data-ttu-id="397e8-164">Kodaren: MainConcept H.264</span><span class="sxs-lookup"><span data-stu-id="397e8-164">Encoder: MainConcept H.264</span></span>
   * <span data-ttu-id="397e8-165">Bildrutor per sekund: 30</span><span class="sxs-lookup"><span data-stu-id="397e8-165">Frames per Second: 30</span></span>
   * <span data-ttu-id="397e8-166">Genomsnittlig bithastighet: 5000 kbits per sekund (kan justeras utifrån nätverksbegränsningar)</span><span class="sxs-lookup"><span data-stu-id="397e8-166">Average bit rate: 5000 kbits/sec (Can be adjusted based on network limitations)</span></span>
   * <span data-ttu-id="397e8-167">Profil: Main</span><span class="sxs-lookup"><span data-stu-id="397e8-167">Profile: Main</span></span>
   * <span data-ttu-id="397e8-168">Nyckelbilden var: 60 ramar</span><span class="sxs-lookup"><span data-stu-id="397e8-168">Key frame every: 60 frames</span></span>

    <span data-ttu-id="397e8-169">**Ljud**</span><span class="sxs-lookup"><span data-stu-id="397e8-169">**Audio**</span></span>

   * <span data-ttu-id="397e8-170">Mål bithastighet: 192 kbits per sekund</span><span class="sxs-lookup"><span data-stu-id="397e8-170">Target bit rate: 192 kbits/sec</span></span>
   * <span data-ttu-id="397e8-171">Samplingsfrekvens: 44 100 kHz</span><span class="sxs-lookup"><span data-stu-id="397e8-171">Sample Rate: 44.100 kHz</span></span>

     ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast4.png)
6. <span data-ttu-id="397e8-173">Tryck på **spara**.</span><span class="sxs-lookup"><span data-stu-id="397e8-173">Press **Save**.</span></span>

    <span data-ttu-id="397e8-174">fältet för avkodning av hello har nu hello nyskapad profil kan väljas.</span><span class="sxs-lookup"><span data-stu-id="397e8-174">hello Encoding field now has hello newly created profile available for selection.</span></span>

    <span data-ttu-id="397e8-175">Kontrollera att hello ny profil är markerad.</span><span class="sxs-lookup"><span data-stu-id="397e8-175">Make sure hello new profile is selected.</span></span>
7. <span data-ttu-id="397e8-176">Hämta hello kanalens inkommande URL i ordning tooassign den toohello Wirecast **RTMP Endpoint**.</span><span class="sxs-lookup"><span data-stu-id="397e8-176">Get hello channel's input URL in order tooassign it toohello Wirecast **RTMP Endpoint**.</span></span>

    <span data-ttu-id="397e8-177">Gå tillbaka toohello AMSE-verktyget och kontrollera status för slutförande av hello-kanal.</span><span class="sxs-lookup"><span data-stu-id="397e8-177">Navigate back toohello AMSE tool, and check on hello channel completion status.</span></span> <span data-ttu-id="397e8-178">När hello tillstånd har ändrats från **Start** för**kör**, du kan hämta hello indata-URL.</span><span class="sxs-lookup"><span data-stu-id="397e8-178">Once hello State has changed from **Starting** too**Running**, you can get hello input URL.</span></span>

    <span data-ttu-id="397e8-179">När du kör hello kanal högerklickar du på hello Kanalnamn, Navigera ned toohover över **kopiera indata URL tooclipboard** och välj sedan **primära indata-URL**.</span><span class="sxs-lookup"><span data-stu-id="397e8-179">When hello channel is running, right click hello channel name, navigate down toohover over **Copy Input URL tooclipboard** and then select **Primary Input  URL**.</span></span>  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast6.png)
8. <span data-ttu-id="397e8-181">I hello Wirecast **inställningar utdata** och klistra in informationen i hello **adress** i hello utdataavsnitt och tilldela ett namn på dataströmmen.</span><span class="sxs-lookup"><span data-stu-id="397e8-181">In hello Wirecast **Output Settings** window, paste this information in hello **Address** field of hello output section, and assign a stream name.</span></span>

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast5.png)

1. <span data-ttu-id="397e8-183">Välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="397e8-183">Select **OK**.</span></span>
2. <span data-ttu-id="397e8-184">På hello huvudsakliga **Wirecast** skärmen, bekräfta indatakällor för video och ljud är klar och tryck sedan på **dataströmmen** i hello övre vänstra hörnet.</span><span class="sxs-lookup"><span data-stu-id="397e8-184">On hello main **Wirecast** screen, confirm input sources for video and audio are ready and then hit **Stream** in hello top left hand corner.</span></span>

   ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast7.png)

> [!IMPORTANT]
> <span data-ttu-id="397e8-186">Innan du klickar på **dataströmmen**, du **måste** se till att hello kanalen är klar.</span><span class="sxs-lookup"><span data-stu-id="397e8-186">Before you click **Stream**, you **must** ensure that hello Channel is ready.</span></span>
> <span data-ttu-id="397e8-187">Kontrollera också att inte tooleave hello kanal med tillståndet klart utan ett inkommande bidrag feed för > 15 minuter.</span><span class="sxs-lookup"><span data-stu-id="397e8-187">Also, make sure not tooleave hello Channel in a ready state without an input contribution feed for longer than > 15 minutes.</span></span>
>
>

## <a name="test-playback"></a><span data-ttu-id="397e8-188">Testa uppspelning</span><span class="sxs-lookup"><span data-stu-id="397e8-188">Test playback</span></span>

<span data-ttu-id="397e8-189">Navigera toohello AMSE-verktyget och högerklicka på hello kanal toobe testas.</span><span class="sxs-lookup"><span data-stu-id="397e8-189">Navigate toohello AMSE tool, and right click hello channel toobe tested.</span></span> <span data-ttu-id="397e8-190">Hovra över hello-menyn **uppspelning hello Preview** och välj **med Azure Media Player**.</span><span class="sxs-lookup"><span data-stu-id="397e8-190">From hello menu, hover over **Playback hello Preview** and select **with Azure Media Player**.</span></span>  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast8.png)

<span data-ttu-id="397e8-191">Om hello dataströmmen visas i hello player har hello kodare varit korrekt konfigurerade tooconnect tooAMS.</span><span class="sxs-lookup"><span data-stu-id="397e8-191">If hello stream appears in hello player, then hello encoder has been properly configured tooconnect tooAMS.</span></span>

<span data-ttu-id="397e8-192">Om ett fel tas emot måste hello kanal toobe Återställ kodare inställningar och justeras.</span><span class="sxs-lookup"><span data-stu-id="397e8-192">If an error is received, hello channel will need toobe reset and encoder settings adjusted.</span></span> <span data-ttu-id="397e8-193">Se hello [felsökning](media-services-troubleshooting-live-streaming.md) avsnittet som vägledning.</span><span class="sxs-lookup"><span data-stu-id="397e8-193">Please see hello [troubleshooting](media-services-troubleshooting-live-streaming.md) topic for guidance.</span></span>  

## <a name="create-a-program"></a><span data-ttu-id="397e8-194">Skapa ett program</span><span class="sxs-lookup"><span data-stu-id="397e8-194">Create a program</span></span>
1. <span data-ttu-id="397e8-195">När kanalen uppspelning bekräftas, skapa ett program.</span><span class="sxs-lookup"><span data-stu-id="397e8-195">Once channel playback is confirmed, create a program.</span></span> <span data-ttu-id="397e8-196">Under hello **Live** i hello AMSE-verktyget, högerklickar du i området för hello-programmet och välj **Skapa nytt Program**.</span><span class="sxs-lookup"><span data-stu-id="397e8-196">Under hello **Live** tab in hello AMSE tool, right click within hello program area and select **Create New Program**.</span></span>  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast9.png)
2. <span data-ttu-id="397e8-198">Namnge programmet hello och, om det behövs, justera hello **längd** (som standard too4 timmar).</span><span class="sxs-lookup"><span data-stu-id="397e8-198">Name hello program and, if needed, adjust hello **Archive Window Length** (which defaults too4 hours).</span></span> <span data-ttu-id="397e8-199">Du kan också ange en lagringsplats eller lämna som hello standard.</span><span class="sxs-lookup"><span data-stu-id="397e8-199">You can also specify a storage location or leave as hello default.</span></span>  
3. <span data-ttu-id="397e8-200">Kontrollera hello **Start hello programmet nu** rutan.</span><span class="sxs-lookup"><span data-stu-id="397e8-200">Check hello **Start hello Program now** box.</span></span>
4. <span data-ttu-id="397e8-201">Klicka på **skapa Program**.</span><span class="sxs-lookup"><span data-stu-id="397e8-201">Click **Create Program**.</span></span>  

   >[!NOTE]
   ><span data-ttu-id="397e8-202">Skapa en program tar mindre tid än att skapa en kanal.</span><span class="sxs-lookup"><span data-stu-id="397e8-202">Program creation takes less time than channel creation.</span></span>
       
5. <span data-ttu-id="397e8-203">När hello programmet körs kan bekräfta uppspelning genom att högerklicka på programmet hello och navigera för**uppspelning hello program** och sedan välja **med Azure Media Player**.</span><span class="sxs-lookup"><span data-stu-id="397e8-203">Once hello program is running, confirm playback by right clicking hello program and navigating too**Playback hello program(s)** and then selecting **with Azure Media Player**.</span></span>  
6. <span data-ttu-id="397e8-204">När bekräftat, högerklicka på programmet hello igen och välj **kopiera hello utdata URL tooClipboard** (eller hämta den här informationen från hello **programmet information och inställningar** alternativet hello-menyn).</span><span class="sxs-lookup"><span data-stu-id="397e8-204">Once confirmed, right click hello program again and select **Copy hello Output URL tooClipboard** (or retrieve this information from hello **Program information and settings** option from hello menu).</span></span>

<span data-ttu-id="397e8-205">hello är nu redo toobe inbäddat i en spelare eller distribuerade tooan målgruppen för live visning.</span><span class="sxs-lookup"><span data-stu-id="397e8-205">hello stream is now ready toobe embedded in a player, or distributed tooan audience for live viewing.</span></span>  

## <a name="troubleshooting"></a><span data-ttu-id="397e8-206">Felsökning</span><span class="sxs-lookup"><span data-stu-id="397e8-206">Troubleshooting</span></span>
<span data-ttu-id="397e8-207">Se hello [felsökning](media-services-troubleshooting-live-streaming.md) avsnittet som vägledning.</span><span class="sxs-lookup"><span data-stu-id="397e8-207">Please see hello [troubleshooting](media-services-troubleshooting-live-streaming.md) topic for guidance.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="397e8-208">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="397e8-208">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="397e8-209">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="397e8-209">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
