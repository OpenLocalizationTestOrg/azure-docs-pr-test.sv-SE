---
title: "aaaConfigure hello FMLE kodare toosend en direktsänd dataström med enkel bithastighet | Microsoft Docs"
description: "Det här avsnittet visar hur hello tooconfigure Flash Media Live kodare (FMLE) kodare toosend en enkel bithastighet dataströmmen tooAMS kanaler som är aktiverade för live encoding."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 3113f333-517a-47a1-a1b3-57e200c6b2a2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 01/05/2017
ms.author: juliako;cenkdin;anilmur
ms.openlocfilehash: 780d911c5186ec09c784264f9a0d0c3f8059d305
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-fmle-encoder-toosend-a-single-bitrate-live-stream"></a><span data-ttu-id="50452-103">Använd hello FMLE kodare toosend en direktsänd dataström med enkel bithastighet</span><span class="sxs-lookup"><span data-stu-id="50452-103">Use hello FMLE encoder toosend a single bitrate live stream</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="50452-104">FMLE</span><span class="sxs-lookup"><span data-stu-id="50452-104">FMLE</span></span>](media-services-configure-fmle-live-encoder.md)
> * [<span data-ttu-id="50452-105">Elemental Live</span><span class="sxs-lookup"><span data-stu-id="50452-105">Elemental Live</span></span>](media-services-configure-elemental-live-encoder.md)
> * [<span data-ttu-id="50452-106">Tricaster</span><span class="sxs-lookup"><span data-stu-id="50452-106">Tricaster</span></span>](media-services-configure-tricaster-live-encoder.md)
> * [<span data-ttu-id="50452-107">Wirecast</span><span class="sxs-lookup"><span data-stu-id="50452-107">Wirecast</span></span>](media-services-configure-wirecast-live-encoder.md)
>
>

<span data-ttu-id="50452-108">Det här avsnittet visar hur tooconfigure hello [Flash Live mediekodare](http://www.adobe.com/products/flash-media-encoder.html) (FMLE) kodare toosend enkel bithastighet strömma tooAMS kanaler som är aktiverade för live encoding.</span><span class="sxs-lookup"><span data-stu-id="50452-108">This topic shows how tooconfigure hello [Flash Media Live Encoder](http://www.adobe.com/products/flash-media-encoder.html) (FMLE) encoder toosend a single bitrate stream tooAMS channels that are enabled for live encoding.</span></span> <span data-ttu-id="50452-109">Mer information finns i [arbeta med kanaler som är aktiverad tooPerform Live Encoding med Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span><span class="sxs-lookup"><span data-stu-id="50452-109">For more information, see [Working with Channels that are Enabled tooPerform Live Encoding with Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span></span>

<span data-ttu-id="50452-110">Den här kursen visar hur toomanage Azure Media Services (AMS) med Azure Media Services Explorer (AMSE)-verktyget.</span><span class="sxs-lookup"><span data-stu-id="50452-110">This tutorial shows how toomanage Azure Media Services (AMS) with Azure Media Services Explorer (AMSE) tool.</span></span> <span data-ttu-id="50452-111">Det här verktyget körs bara på Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="50452-111">This tool only runs on Windows PC.</span></span> <span data-ttu-id="50452-112">Om du är på Mac- eller Linux använda hello Azure portal toocreate [kanaler](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) och [program](media-services-portal-creating-live-encoder-enabled-channel.md).</span><span class="sxs-lookup"><span data-stu-id="50452-112">If you are on Mac or Linux, use hello Azure portal toocreate [channels](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) and [programs](media-services-portal-creating-live-encoder-enabled-channel.md).</span></span>

<span data-ttu-id="50452-113">Observera att den här självstudiekursen beskriver hur du använder AAC.</span><span class="sxs-lookup"><span data-stu-id="50452-113">Note that this tutorial describes using AAC.</span></span> <span data-ttu-id="50452-114">FMLE stöder inte AAC som standard.</span><span class="sxs-lookup"><span data-stu-id="50452-114">However, FMLE doesn’t supports AAC by default.</span></span> <span data-ttu-id="50452-115">Du skulle behöva toopurchase ett plugin-program för AAC kodning till exempel från MainConcept: [AAC plugin-program](http://www.mainconcept.com/products/plug-ins/plug-ins-for-adobe/aac-encoder-fmle.html)</span><span class="sxs-lookup"><span data-stu-id="50452-115">You would need toopurchase a plugin for AAC encoding such as from MainConcept: [AAC plugin](http://www.mainconcept.com/products/plug-ins/plug-ins-for-adobe/aac-encoder-fmle.html)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="50452-116">Krav</span><span class="sxs-lookup"><span data-stu-id="50452-116">Prerequisites</span></span>
* [<span data-ttu-id="50452-117">Skapa ett Azure Media Services-konto</span><span class="sxs-lookup"><span data-stu-id="50452-117">Create an Azure Media Services account</span></span>](media-services-portal-create-account.md)
* <span data-ttu-id="50452-118">Se till att det finns en Strömningsslutpunkt som körs.</span><span class="sxs-lookup"><span data-stu-id="50452-118">Ensure there is a Streaming Endpoint running.</span></span> <span data-ttu-id="50452-119">Mer information finns i [hanterar Strömningsslutpunkter i ett Media Services-konto](media-services-portal-manage-streaming-endpoints.md)</span><span class="sxs-lookup"><span data-stu-id="50452-119">For more information, see [Manage Streaming Endpoints in a Media Services Account](media-services-portal-manage-streaming-endpoints.md)</span></span>
* <span data-ttu-id="50452-120">Installera hello senaste versionen av hello [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) verktyget.</span><span class="sxs-lookup"><span data-stu-id="50452-120">Install hello latest version of hello [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) tool.</span></span>
* <span data-ttu-id="50452-121">Starta verktyget hello och ansluta tooyour AMS-konto.</span><span class="sxs-lookup"><span data-stu-id="50452-121">Launch hello tool and connect tooyour AMS account.</span></span>

## <a name="tips"></a><span data-ttu-id="50452-122">Tips</span><span class="sxs-lookup"><span data-stu-id="50452-122">Tips</span></span>
* <span data-ttu-id="50452-123">När det är möjligt använda ett inbyggt internet-anslutning.</span><span class="sxs-lookup"><span data-stu-id="50452-123">Whenever possible, use a hardwired internet connection.</span></span>
* <span data-ttu-id="50452-124">En bra tumregel samband med fastställandet av krav på bandbredd är toodouble hello strömning bithastighet.</span><span class="sxs-lookup"><span data-stu-id="50452-124">A good rule of thumb when determining bandwidth requirements is toodouble hello streaming bitrates.</span></span> <span data-ttu-id="50452-125">Detta är inte ett obligatoriskt krav, som det kan minska hello effekten av överbelastning.</span><span class="sxs-lookup"><span data-stu-id="50452-125">While this is not a mandatory requirement, it will help mitigate hello impact of network congestion.</span></span>
* <span data-ttu-id="50452-126">När du använder programvara baserat kodare, Stäng alla onödiga program.</span><span class="sxs-lookup"><span data-stu-id="50452-126">When using software based encoders, close out any unnecessary programs.</span></span>

## <a name="create-a-channel"></a><span data-ttu-id="50452-127">Skapa en kanal</span><span class="sxs-lookup"><span data-stu-id="50452-127">Create a channel</span></span>
1. <span data-ttu-id="50452-128">Navigera i hello AMSE-verktyget toohello **Live** och högerklicka på inom området för hello-kanal.</span><span class="sxs-lookup"><span data-stu-id="50452-128">In hello AMSE tool, navigate toohello **Live** tab, and right click within hello channel area.</span></span> <span data-ttu-id="50452-129">Välj **skapa kanal...**</span><span class="sxs-lookup"><span data-stu-id="50452-129">Select **Create channel…**</span></span> <span data-ttu-id="50452-130">hello-menyn.</span><span class="sxs-lookup"><span data-stu-id="50452-130">from hello menu.</span></span>

    ![FMLE](./media/media-services-fmle-live-encoder/media-services-fmle1.png)

2. <span data-ttu-id="50452-132">Ange ett Kanalnamn hello beskrivningsfältet är valfritt.</span><span class="sxs-lookup"><span data-stu-id="50452-132">Specify a channel name, hello description field is optional.</span></span> <span data-ttu-id="50452-133">Markera under inställningar för kanal **Standard** för hello Live Encoding alternativet med hello indata protokollet värdet för**RTMP**.</span><span class="sxs-lookup"><span data-stu-id="50452-133">Under Channel Settings, select **Standard** for hello Live Encoding option, with hello Input Protocol set too**RTMP**.</span></span> <span data-ttu-id="50452-134">Du kan lämna alla andra inställningar som är.</span><span class="sxs-lookup"><span data-stu-id="50452-134">You can leave all other settings as is.</span></span>

    <span data-ttu-id="50452-135">Se till att hello **Start hello ny kanal nu** är markerad.</span><span class="sxs-lookup"><span data-stu-id="50452-135">Make sure hello **Start hello new channel now** is selected.</span></span>

3. <span data-ttu-id="50452-136">Klicka på **skapa kanal**.</span><span class="sxs-lookup"><span data-stu-id="50452-136">Click **Create Channel**.</span></span>

   ![FMLE](./media/media-services-fmle-live-encoder/media-services-fmle2.png)

> [!NOTE]
> <span data-ttu-id="50452-138">hello kanal kan ta upp till 20 minuter toostart.</span><span class="sxs-lookup"><span data-stu-id="50452-138">hello channel can take as long as 20 minutes toostart.</span></span>
>
>

<span data-ttu-id="50452-139">Medan hello kanal startar kan du [konfigurera hello kodare](media-services-configure-fmle-live-encoder.md#configure_fmle_rtmp).</span><span class="sxs-lookup"><span data-stu-id="50452-139">While hello channel is starting you can [configure hello encoder](media-services-configure-fmle-live-encoder.md#configure_fmle_rtmp).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="50452-140">Observera att faktureringen påbörjas så snart kanal blir klar att användas.</span><span class="sxs-lookup"><span data-stu-id="50452-140">Note that billing starts as soon as Channel goes into a ready state.</span></span> <span data-ttu-id="50452-141">Mer information finns i [kanalens tillstånd](media-services-manage-live-encoder-enabled-channels.md#states).</span><span class="sxs-lookup"><span data-stu-id="50452-141">For more information, see [Channel's states](media-services-manage-live-encoder-enabled-channels.md#states).</span></span>
>
>

## <span data-ttu-id="50452-142"><a id=configure_fmle_rtmp></a>Konfigurera hello FMLE kodaren</span><span class="sxs-lookup"><span data-stu-id="50452-142"><a id=configure_fmle_rtmp></a>Configure hello FMLE encoder</span></span>
<span data-ttu-id="50452-143">I den här självstudiekursen hello används följande inställningar för utdata.</span><span class="sxs-lookup"><span data-stu-id="50452-143">In this tutorial hello following output settings are used.</span></span> <span data-ttu-id="50452-144">hello resten av det här avsnittet beskriver konfigurationssteg i detalj.</span><span class="sxs-lookup"><span data-stu-id="50452-144">hello rest of this section describes configuration steps in more detail.</span></span>

<span data-ttu-id="50452-145">**Video**:</span><span class="sxs-lookup"><span data-stu-id="50452-145">**Video**:</span></span>

* <span data-ttu-id="50452-146">Codec: H.264</span><span class="sxs-lookup"><span data-stu-id="50452-146">Codec: H.264</span></span>
* <span data-ttu-id="50452-147">Profil: Hög (nivå 4.0)</span><span class="sxs-lookup"><span data-stu-id="50452-147">Profile: High (Level 4.0)</span></span>
* <span data-ttu-id="50452-148">Bithastighet: 5000 kbit/s</span><span class="sxs-lookup"><span data-stu-id="50452-148">Bitrate: 5000 kbps</span></span>
* <span data-ttu-id="50452-149">Keyframe: 2 sekunder (60 sekunder)</span><span class="sxs-lookup"><span data-stu-id="50452-149">Keyframe: 2 seconds (60 seconds)</span></span>
* <span data-ttu-id="50452-150">RAM-hastighet: 30</span><span class="sxs-lookup"><span data-stu-id="50452-150">Frame Rate: 30</span></span>

<span data-ttu-id="50452-151">**Ljud**:</span><span class="sxs-lookup"><span data-stu-id="50452-151">**Audio**:</span></span>

* <span data-ttu-id="50452-152">Codec: AAC (LC)</span><span class="sxs-lookup"><span data-stu-id="50452-152">Codec: AAC (LC)</span></span>
* <span data-ttu-id="50452-153">Bithastighet: 192 kbit/s</span><span class="sxs-lookup"><span data-stu-id="50452-153">Bitrate: 192 kbps</span></span>
* <span data-ttu-id="50452-154">Samplingsfrekvens: 44.1 kHz</span><span class="sxs-lookup"><span data-stu-id="50452-154">Sample Rate: 44.1 kHz</span></span>

### <a name="configuration-steps"></a><span data-ttu-id="50452-155">Konfigurationssteg</span><span class="sxs-lookup"><span data-stu-id="50452-155">Configuration steps</span></span>
1. <span data-ttu-id="50452-156">Navigera toohello Flash Media Live kodarens (FMLE) gränssnitt på hello datorn används.</span><span class="sxs-lookup"><span data-stu-id="50452-156">Navigate toohello Flash Media Live Encoder’s (FMLE) interface on hello machine being used.</span></span>

    <span data-ttu-id="50452-157">hello-gränssnittet är en huvudsidan för inställningar.</span><span class="sxs-lookup"><span data-stu-id="50452-157">hello interface is one main page of settings.</span></span> <span data-ttu-id="50452-158">Kontrollera anteckna hello följande rekommenderade inställningar tooget igång med strömning med FMLE.</span><span class="sxs-lookup"><span data-stu-id="50452-158">Please take note of hello following recommended settings tooget started with streaming using FMLE.</span></span>

   * <span data-ttu-id="50452-159">Format: H.264 bildfrekvens: 30,00</span><span class="sxs-lookup"><span data-stu-id="50452-159">Format: H.264 Frame Rate: 30.00</span></span>
   * <span data-ttu-id="50452-160">Inkommande storlek: minst 1 280 x 720</span><span class="sxs-lookup"><span data-stu-id="50452-160">Input Size: 1280 x 720</span></span>
   * <span data-ttu-id="50452-161">Bithastighet: 5000 kbit/s (kan justeras utifrån nätverksbegränsningar)</span><span class="sxs-lookup"><span data-stu-id="50452-161">Bit Rate: 5000 Kbps (Can be adjusted based on network limitations)</span></span>  

     ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle3.png)

     <span data-ttu-id="50452-163">När du använder sammanflätad källor, kontrollera markering hello ”Ej sammanflätning” alternativet</span><span class="sxs-lookup"><span data-stu-id="50452-163">When using interlaced sources, please checkmark hello “Deinterlace” option</span></span>
2. <span data-ttu-id="50452-164">Välj hello skiftnyckel ikonen nästa tooFormat, dessa ytterligare inställningar ska vara:</span><span class="sxs-lookup"><span data-stu-id="50452-164">Select hello wrench icon next tooFormat, these additional settings should be:</span></span>

   * <span data-ttu-id="50452-165">Profil: Main</span><span class="sxs-lookup"><span data-stu-id="50452-165">Profile: Main</span></span>
   * <span data-ttu-id="50452-166">Nivå: 4.0</span><span class="sxs-lookup"><span data-stu-id="50452-166">Level: 4.0</span></span>
   * <span data-ttu-id="50452-167">Keyframe frekvens: 2 sekunder</span><span class="sxs-lookup"><span data-stu-id="50452-167">Keyframe Frequency: 2 seconds</span></span>

     ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle4.png)
3. <span data-ttu-id="50452-169">Ange hello följande viktiga ljud inställningen:</span><span class="sxs-lookup"><span data-stu-id="50452-169">Set hello following important audio setting:</span></span>

   * <span data-ttu-id="50452-170">Format: AAC</span><span class="sxs-lookup"><span data-stu-id="50452-170">Format: AAC</span></span>
   * <span data-ttu-id="50452-171">Samplingsfrekvens: 44100 Hz</span><span class="sxs-lookup"><span data-stu-id="50452-171">Sample Rate: 44100 Hz</span></span>
   * <span data-ttu-id="50452-172">Bithastighet: 192 kbit/s</span><span class="sxs-lookup"><span data-stu-id="50452-172">Bitrate: 192 Kbps</span></span>

     ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle5.png)
4. <span data-ttu-id="50452-174">Hämta hello kanalens inkommande URL i ordning tooassign den toohello FMLE **RTMP Endpoint**.</span><span class="sxs-lookup"><span data-stu-id="50452-174">Get hello channel's input URL in order tooassign it toohello FMLE's **RTMP Endpoint**.</span></span>

    <span data-ttu-id="50452-175">Gå tillbaka toohello AMSE-verktyget och kontrollera status för slutförande av hello-kanal.</span><span class="sxs-lookup"><span data-stu-id="50452-175">Navigate back toohello AMSE tool, and check on hello channel completion status.</span></span> <span data-ttu-id="50452-176">När hello tillstånd har ändrats från **Start** för**kör**, du kan hämta hello indata-URL.</span><span class="sxs-lookup"><span data-stu-id="50452-176">Once hello State has changed from **Starting** too**Running**, you can get hello input URL.</span></span>

    <span data-ttu-id="50452-177">När du kör hello kanal högerklickar du på hello Kanalnamn, Navigera ned toohover över **kopiera indata URL tooclipboard** och välj sedan **primära indata-URL**.</span><span class="sxs-lookup"><span data-stu-id="50452-177">When hello channel is running, right click hello channel name, navigate down toohover over **Copy Input URL tooclipboard** and then select **Primary Input  URL**.</span></span>  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle6.png)
5. <span data-ttu-id="50452-179">Klistra in informationen i hello **FMS URL** i hello utdataavsnitt och tilldela ett namn på dataströmmen.</span><span class="sxs-lookup"><span data-stu-id="50452-179">Paste this information in hello **FMS URL** field of hello output section, and assign a stream name.</span></span>

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle7.png)

    <span data-ttu-id="50452-181">Upprepa dessa steg med hello sekundära indata-URL för extra redundans.</span><span class="sxs-lookup"><span data-stu-id="50452-181">For extra redundancy, repeat these steps with hello Secondary Input URL.</span></span>
6. <span data-ttu-id="50452-182">Välj **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="50452-182">Select **Connect**.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="50452-183">Innan du klickar på **Anslut**, du **måste** se till att hello kanalen är klar.</span><span class="sxs-lookup"><span data-stu-id="50452-183">Before you click **Connect**, you **must** ensure that hello Channel is ready.</span></span>
> <span data-ttu-id="50452-184">Kontrollera också att inte tooleave hello kanal med tillståndet klart utan ett inkommande bidrag feed för > 15 minuter.</span><span class="sxs-lookup"><span data-stu-id="50452-184">Also, make sure not tooleave hello Channel in a ready state without an input contribution feed for longer than > 15 minutes.</span></span>
>
>

## <a name="test-playback"></a><span data-ttu-id="50452-185">Testa uppspelning</span><span class="sxs-lookup"><span data-stu-id="50452-185">Test playback</span></span>

<span data-ttu-id="50452-186">Navigera toohello AMSE-verktyget och högerklicka på hello kanal toobe testas.</span><span class="sxs-lookup"><span data-stu-id="50452-186">Navigate toohello AMSE tool, and right click hello channel toobe tested.</span></span> <span data-ttu-id="50452-187">Hovra över hello-menyn **uppspelning hello Preview** och välj **med Azure Media Player**.</span><span class="sxs-lookup"><span data-stu-id="50452-187">From hello menu, hover over **Playback hello Preview** and select **with Azure Media Player**.</span></span>  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle8.png)

<span data-ttu-id="50452-188">Om hello dataströmmen visas i hello player har hello kodare varit korrekt konfigurerade tooconnect tooAMS.</span><span class="sxs-lookup"><span data-stu-id="50452-188">If hello stream appears in hello player, then hello encoder has been properly configured tooconnect tooAMS.</span></span>

<span data-ttu-id="50452-189">Om ett fel tas emot måste hello kanal toobe Återställ kodare inställningar och justeras.</span><span class="sxs-lookup"><span data-stu-id="50452-189">If an error is received, hello channel will need toobe reset and encoder settings adjusted.</span></span> <span data-ttu-id="50452-190">Se hello [felsökning](media-services-troubleshooting-live-streaming.md) avsnittet som vägledning.</span><span class="sxs-lookup"><span data-stu-id="50452-190">Please see hello [troubleshooting](media-services-troubleshooting-live-streaming.md) topic for guidance.</span></span>  

## <a name="create-a-program"></a><span data-ttu-id="50452-191">Skapa ett program</span><span class="sxs-lookup"><span data-stu-id="50452-191">Create a program</span></span>
1. <span data-ttu-id="50452-192">När kanalen uppspelning bekräftas, skapa ett program.</span><span class="sxs-lookup"><span data-stu-id="50452-192">Once channel playback is confirmed, create a program.</span></span> <span data-ttu-id="50452-193">Under hello **Live** i hello AMSE-verktyget, högerklickar du i området för hello-programmet och välj **Skapa nytt Program**.</span><span class="sxs-lookup"><span data-stu-id="50452-193">Under hello **Live** tab in hello AMSE tool, right click within hello program area and select **Create New Program**.</span></span>  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle9.png)
2. <span data-ttu-id="50452-195">Namnge programmet hello och, om det behövs, justera hello **längd** (som standard too4 timmar).</span><span class="sxs-lookup"><span data-stu-id="50452-195">Name hello program and, if needed, adjust hello **Archive Window Length** (which defaults too4 hours).</span></span> <span data-ttu-id="50452-196">Du kan också ange en lagringsplats eller lämna som hello standard.</span><span class="sxs-lookup"><span data-stu-id="50452-196">You can also specify a storage location or leave as hello default.</span></span>  
3. <span data-ttu-id="50452-197">Kontrollera hello **Start hello programmet nu** rutan.</span><span class="sxs-lookup"><span data-stu-id="50452-197">Check hello **Start hello Program now** box.</span></span>
4. <span data-ttu-id="50452-198">Klicka på **skapa Program**.</span><span class="sxs-lookup"><span data-stu-id="50452-198">Click **Create Program**.</span></span>  

    >[!NOTE]
    ><span data-ttu-id="50452-199">Skapa en program tar mindre tid än att skapa en kanal.</span><span class="sxs-lookup"><span data-stu-id="50452-199">Program creation takes less time than channel creation.</span></span>
        
5. <span data-ttu-id="50452-200">När hello programmet körs kan bekräfta uppspelning genom att högerklicka på programmet hello och navigera för**uppspelning hello program** och sedan välja **med Azure Media Player**.</span><span class="sxs-lookup"><span data-stu-id="50452-200">Once hello program is running, confirm playback by right clicking hello program and navigating too**Playback hello program(s)** and then selecting **with Azure Media Player**.</span></span>  
6. <span data-ttu-id="50452-201">När bekräftat, högerklicka på programmet hello igen och välj **kopiera hello utdata URL tooClipboard** (eller hämta den här informationen från hello **programmet information och inställningar** alternativet hello-menyn).</span><span class="sxs-lookup"><span data-stu-id="50452-201">Once confirmed, right click hello program again and select **Copy hello Output URL tooClipboard** (or retrieve this information from hello **Program information and settings** option from hello menu).</span></span>

<span data-ttu-id="50452-202">hello är nu redo toobe inbäddat i en spelare eller distribuerade tooan målgruppen för live visning.</span><span class="sxs-lookup"><span data-stu-id="50452-202">hello stream is now ready toobe embedded in a player, or distributed tooan audience for live viewing.</span></span>  

## <a name="troubleshooting"></a><span data-ttu-id="50452-203">Felsökning</span><span class="sxs-lookup"><span data-stu-id="50452-203">Troubleshooting</span></span>
<span data-ttu-id="50452-204">Se hello [felsökning](media-services-troubleshooting-live-streaming.md) avsnittet som vägledning.</span><span class="sxs-lookup"><span data-stu-id="50452-204">Please see hello [troubleshooting](media-services-troubleshooting-live-streaming.md) topic for guidance.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="50452-205">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="50452-205">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="50452-206">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="50452-206">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
