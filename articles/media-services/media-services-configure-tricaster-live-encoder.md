---
title: "aaaConfigure hello NewTek TriCaster kodare toosend en direktsänd dataström med enkel bithastighet | Microsoft Docs"
description: "Det här avsnittet visar hur live tooconfigure hello Tricaster kodare toosend en enkel bithastighet dataströmmen tooAMS kanaler som är aktiverade för live encoding."
services: media-services
documentationcenter: 
author: cenkdin
manager: cfowler
editor: 
ms.assetid: 8973181a-3059-471a-a6bb-ccda7d3ff297
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 01/05/2017
ms.author: juliako;cenkd;anilmur
ms.openlocfilehash: 57dcf62a6a76b04e69f147a738be78ccb3c3ecdc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-newtek-tricaster-encoder-toosend-a-single-bitrate-live-stream"></a><span data-ttu-id="70ee1-103">Använd hello NewTek TriCaster kodare toosend en direktsänd dataström med enkel bithastighet</span><span class="sxs-lookup"><span data-stu-id="70ee1-103">Use hello NewTek TriCaster encoder toosend a single bitrate live stream</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="70ee1-104">Tricaster</span><span class="sxs-lookup"><span data-stu-id="70ee1-104">Tricaster</span></span>](media-services-configure-tricaster-live-encoder.md)
> * [<span data-ttu-id="70ee1-105">Elemental Live</span><span class="sxs-lookup"><span data-stu-id="70ee1-105">Elemental Live</span></span>](media-services-configure-elemental-live-encoder.md)
> * [<span data-ttu-id="70ee1-106">Wirecast</span><span class="sxs-lookup"><span data-stu-id="70ee1-106">Wirecast</span></span>](media-services-configure-wirecast-live-encoder.md)
> * [<span data-ttu-id="70ee1-107">FMLE</span><span class="sxs-lookup"><span data-stu-id="70ee1-107">FMLE</span></span>](media-services-configure-fmle-live-encoder.md)
>
>

<span data-ttu-id="70ee1-108">Det här avsnittet visar hur tooconfigure hello [NewTek TriCaster](http://newtek.com/products/tricaster-40.html) live kodare toosend en enkel bithastighet dataströmmen tooAMS kanaler som är aktiverade för live encoding.</span><span class="sxs-lookup"><span data-stu-id="70ee1-108">This topic shows how tooconfigure hello [NewTek TriCaster](http://newtek.com/products/tricaster-40.html) live encoder toosend a single bitrate stream tooAMS channels that are enabled for live encoding.</span></span> <span data-ttu-id="70ee1-109">Mer information finns i [arbeta med kanaler som är aktiverad tooPerform Live Encoding med Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span><span class="sxs-lookup"><span data-stu-id="70ee1-109">For more information, see [Working with Channels that are Enabled tooPerform Live Encoding with Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span></span>

<span data-ttu-id="70ee1-110">Den här kursen visar hur toomanage Azure Media Services (AMS) med Azure Media Services Explorer (AMSE)-verktyget.</span><span class="sxs-lookup"><span data-stu-id="70ee1-110">This tutorial shows how toomanage Azure Media Services (AMS) with Azure Media Services Explorer (AMSE) tool.</span></span> <span data-ttu-id="70ee1-111">Det här verktyget körs bara på Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="70ee1-111">This tool only runs on Windows PC.</span></span> <span data-ttu-id="70ee1-112">Om du är på Mac- eller Linux använda hello Azure portal toocreate [kanaler](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) och [program](media-services-portal-creating-live-encoder-enabled-channel.md).</span><span class="sxs-lookup"><span data-stu-id="70ee1-112">If you are on Mac or Linux, use hello Azure portal toocreate [channels](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) and [programs](media-services-portal-creating-live-encoder-enabled-channel.md).</span></span>

> [!NOTE]
> <span data-ttu-id="70ee1-113">När du använder Tricaster för att skicka i ett bidrag feed tooAMS kanaler som är aktiverade för live encoding, kan det finnas problem med ljud och i din direktsända händelse om du använder vissa funktioner i Tricaster, till exempel snabb klippa ut mellan feeds eller växla till/från pekdatorer .</span><span class="sxs-lookup"><span data-stu-id="70ee1-113">When using Tricaster for sending in a contribution feed tooAMS channels that are enabled for live encoding, there can be video/audio glitches in your live event if you use certain features of Tricaster, such as rapid cutting between feeds, or switching to/from slates.</span></span> <span data-ttu-id="70ee1-114">hello AMS-teamet arbetar på att åtgärda dessa problem fram till dess rekommenderas inte toouse dessa funktioner.</span><span class="sxs-lookup"><span data-stu-id="70ee1-114">hello AMS team is working on fixing these issues, until then, it is not recommend toouse these features.</span></span>
>
>

## <a name="prerequisites"></a><span data-ttu-id="70ee1-115">Krav</span><span class="sxs-lookup"><span data-stu-id="70ee1-115">Prerequisites</span></span>
* [<span data-ttu-id="70ee1-116">Skapa ett Azure Media Services-konto</span><span class="sxs-lookup"><span data-stu-id="70ee1-116">Create an Azure Media Services account</span></span>](media-services-portal-create-account.md)
* <span data-ttu-id="70ee1-117">Se till att det finns en Strömningsslutpunkt som körs.</span><span class="sxs-lookup"><span data-stu-id="70ee1-117">Ensure there is a Streaming Endpoint running.</span></span> <span data-ttu-id="70ee1-118">Mer information finns i [hanterar Strömningsslutpunkter i ett Media Services-konto](media-services-portal-manage-streaming-endpoints.md)</span><span class="sxs-lookup"><span data-stu-id="70ee1-118">For more information, see [Manage Streaming Endpoints in a Media Services Account](media-services-portal-manage-streaming-endpoints.md)</span></span>
* <span data-ttu-id="70ee1-119">Installera hello senaste versionen av hello [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) verktyget.</span><span class="sxs-lookup"><span data-stu-id="70ee1-119">Install hello latest version of hello [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) tool.</span></span>
* <span data-ttu-id="70ee1-120">Starta verktyget hello och ansluta tooyour AMS-konto.</span><span class="sxs-lookup"><span data-stu-id="70ee1-120">Launch hello tool and connect tooyour AMS account.</span></span>

## <a name="tips"></a><span data-ttu-id="70ee1-121">Tips</span><span class="sxs-lookup"><span data-stu-id="70ee1-121">Tips</span></span>
* <span data-ttu-id="70ee1-122">När det är möjligt använda ett inbyggt internet-anslutning.</span><span class="sxs-lookup"><span data-stu-id="70ee1-122">Whenever possible, use a hardwired internet connection.</span></span>
* <span data-ttu-id="70ee1-123">En bra tumregel samband med fastställandet av krav på bandbredd är toodouble hello strömning bithastighet.</span><span class="sxs-lookup"><span data-stu-id="70ee1-123">A good rule of thumb when determining bandwidth requirements is toodouble hello streaming bitrates.</span></span> <span data-ttu-id="70ee1-124">Detta är inte ett obligatoriskt krav, som det kan minska hello effekten av överbelastning.</span><span class="sxs-lookup"><span data-stu-id="70ee1-124">While this is not a mandatory requirement, it will help mitigate hello impact of network congestion.</span></span>
* <span data-ttu-id="70ee1-125">När du använder programvara baserat kodare, Stäng alla onödiga program.</span><span class="sxs-lookup"><span data-stu-id="70ee1-125">When using software based encoders, close out any unnecessary programs.</span></span>

## <a name="create-a-channel"></a><span data-ttu-id="70ee1-126">Skapa en kanal</span><span class="sxs-lookup"><span data-stu-id="70ee1-126">Create a channel</span></span>
1. <span data-ttu-id="70ee1-127">Navigera i hello AMSE-verktyget toohello **Live** och högerklicka på inom området för hello-kanal.</span><span class="sxs-lookup"><span data-stu-id="70ee1-127">In hello AMSE tool, navigate toohello **Live** tab, and right click within hello channel area.</span></span> <span data-ttu-id="70ee1-128">Välj **skapa kanal...**</span><span class="sxs-lookup"><span data-stu-id="70ee1-128">Select **Create channel…**</span></span> <span data-ttu-id="70ee1-129">hello-menyn.</span><span class="sxs-lookup"><span data-stu-id="70ee1-129">from hello menu.</span></span>

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster1.png)

2. <span data-ttu-id="70ee1-131">Ange ett Kanalnamn hello beskrivningsfältet är valfritt.</span><span class="sxs-lookup"><span data-stu-id="70ee1-131">Specify a channel name, hello description field is optional.</span></span> <span data-ttu-id="70ee1-132">Markera under inställningar för kanal **Standard** för hello Live Encoding alternativet med hello indata protokollet värdet för**RTMP**.</span><span class="sxs-lookup"><span data-stu-id="70ee1-132">Under Channel Settings, select **Standard** for hello Live Encoding option, with hello Input Protocol set too**RTMP**.</span></span> <span data-ttu-id="70ee1-133">Du kan lämna alla andra inställningar som är.</span><span class="sxs-lookup"><span data-stu-id="70ee1-133">You can leave all other settings as is.</span></span>

    <span data-ttu-id="70ee1-134">Se till att hello **Start hello ny kanal nu** är markerad.</span><span class="sxs-lookup"><span data-stu-id="70ee1-134">Make sure hello **Start hello new channel now** is selected.</span></span>

3. <span data-ttu-id="70ee1-135">Klicka på **skapa kanal**.</span><span class="sxs-lookup"><span data-stu-id="70ee1-135">Click **Create Channel**.</span></span>

   ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster2.png)

> [!NOTE]
> <span data-ttu-id="70ee1-137">hello kanal kan ta upp till 20 minuter toostart.</span><span class="sxs-lookup"><span data-stu-id="70ee1-137">hello channel can take as long as 20 minutes toostart.</span></span>
>
>

<span data-ttu-id="70ee1-138">Medan hello kanal startar kan du [konfigurera hello kodare](media-services-configure-tricaster-live-encoder.md#configure_tricaster_rtmp).</span><span class="sxs-lookup"><span data-stu-id="70ee1-138">While hello channel is starting you can [configure hello encoder](media-services-configure-tricaster-live-encoder.md#configure_tricaster_rtmp).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="70ee1-139">Observera att faktureringen påbörjas så snart kanal blir klar att användas.</span><span class="sxs-lookup"><span data-stu-id="70ee1-139">Note that billing starts as soon as Channel goes into a ready state.</span></span> <span data-ttu-id="70ee1-140">Mer information finns i [kanalens tillstånd](media-services-manage-live-encoder-enabled-channels.md#states).</span><span class="sxs-lookup"><span data-stu-id="70ee1-140">For more information, see [Channel's states](media-services-manage-live-encoder-enabled-channels.md#states).</span></span>
>
>

## <span data-ttu-id="70ee1-141"><a id=configure_tricaster_rtmp></a>Konfigurera hello NewTek TriCaster kodaren</span><span class="sxs-lookup"><span data-stu-id="70ee1-141"><a id=configure_tricaster_rtmp></a>Configure hello NewTek TriCaster encoder</span></span>
<span data-ttu-id="70ee1-142">I den här självstudiekursen hello används följande inställningar för utdata.</span><span class="sxs-lookup"><span data-stu-id="70ee1-142">In this tutorial hello following output settings are used.</span></span> <span data-ttu-id="70ee1-143">hello resten av det här avsnittet beskriver konfigurationssteg i detalj.</span><span class="sxs-lookup"><span data-stu-id="70ee1-143">hello rest of this section describes configuration steps in more detail.</span></span>

<span data-ttu-id="70ee1-144">**Video**:</span><span class="sxs-lookup"><span data-stu-id="70ee1-144">**Video**:</span></span>

* <span data-ttu-id="70ee1-145">Codec: H.264</span><span class="sxs-lookup"><span data-stu-id="70ee1-145">Codec: H.264</span></span>
* <span data-ttu-id="70ee1-146">Profil: Hög (nivå 4.0)</span><span class="sxs-lookup"><span data-stu-id="70ee1-146">Profile: High (Level 4.0)</span></span>
* <span data-ttu-id="70ee1-147">Bithastighet: 5000 kbit/s</span><span class="sxs-lookup"><span data-stu-id="70ee1-147">Bitrate: 5000 kbps</span></span>
* <span data-ttu-id="70ee1-148">Keyframe: 2 sekunder (60 sekunder)</span><span class="sxs-lookup"><span data-stu-id="70ee1-148">Keyframe: 2 seconds (60 seconds)</span></span>
* <span data-ttu-id="70ee1-149">RAM-hastighet: 30</span><span class="sxs-lookup"><span data-stu-id="70ee1-149">Frame Rate: 30</span></span>

<span data-ttu-id="70ee1-150">**Ljud**:</span><span class="sxs-lookup"><span data-stu-id="70ee1-150">**Audio**:</span></span>

* <span data-ttu-id="70ee1-151">Codec: AAC (LC)</span><span class="sxs-lookup"><span data-stu-id="70ee1-151">Codec: AAC (LC)</span></span>
* <span data-ttu-id="70ee1-152">Bithastighet: 192 kbit/s</span><span class="sxs-lookup"><span data-stu-id="70ee1-152">Bitrate: 192 kbps</span></span>
* <span data-ttu-id="70ee1-153">Samplingsfrekvens: 44.1 kHz</span><span class="sxs-lookup"><span data-stu-id="70ee1-153">Sample Rate: 44.1 kHz</span></span>

### <a name="configuration-steps"></a><span data-ttu-id="70ee1-154">Konfigurationssteg</span><span class="sxs-lookup"><span data-stu-id="70ee1-154">Configuration steps</span></span>
1. <span data-ttu-id="70ee1-155">Skapa en ny **NewTek TriCaster** projekt beroende på vilka inkommande videokällan används.</span><span class="sxs-lookup"><span data-stu-id="70ee1-155">Create a new **NewTek TriCaster** project depending on what video input source is being used.</span></span>
2. <span data-ttu-id="70ee1-156">Hitta en gång i det aktuella projektet hello **dataströmmen** och klickar på hello växeln ikonen nästa tooit tooaccess hello dataströmmen configuration-menyn.</span><span class="sxs-lookup"><span data-stu-id="70ee1-156">Once within that project, find hello **Stream** button, and click hello gear icon next tooit tooaccess hello stream configuration menu.</span></span>

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster3.png)
3. <span data-ttu-id="70ee1-158">När du har öppnat hello-menyn, klickar du på **ny** under hello anslutning rubrik.</span><span class="sxs-lookup"><span data-stu-id="70ee1-158">Once hello menu has opened, click **New** under hello Connection heading.</span></span> <span data-ttu-id="70ee1-159">När du uppmanas att ange hello anslutningstypen Välj **Adobe Flash**.</span><span class="sxs-lookup"><span data-stu-id="70ee1-159">When prompted for hello connection type, select **Adobe Flash**.</span></span>

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster4.png)
4. <span data-ttu-id="70ee1-161">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="70ee1-161">Click **OK**.</span></span>
5. <span data-ttu-id="70ee1-162">En profil för FMLE kan nu importeras genom att klicka på hello nedpilen under **strömning profil** och navigera för**Bläddra**.</span><span class="sxs-lookup"><span data-stu-id="70ee1-162">An FMLE profile can now be imported by clicking hello drop down arrow under **Streaming Profile** and navigating too**Browse**.</span></span>

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster5.png)
6. <span data-ttu-id="70ee1-164">Navigera toowhere hello konfigurerad FMLE profilen sparades.</span><span class="sxs-lookup"><span data-stu-id="70ee1-164">Navigate toowhere hello configured FMLE profile was saved.</span></span>
7. <span data-ttu-id="70ee1-165">Markera den och tryck på **OK**.</span><span class="sxs-lookup"><span data-stu-id="70ee1-165">Select it, and press **OK**.</span></span>

    <span data-ttu-id="70ee1-166">När hello profil överförs fortsätta toohello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="70ee1-166">Once hello profile is uploaded, proceed toohello next step.</span></span>
8. <span data-ttu-id="70ee1-167">Hämta hello kanalens inkommande URL i ordning tooassign den toohello Tricaster **RTMP Endpoint**.</span><span class="sxs-lookup"><span data-stu-id="70ee1-167">Get hello channel's input URL in order tooassign it toohello Tricaster **RTMP Endpoint**.</span></span>

    <span data-ttu-id="70ee1-168">Gå tillbaka toohello AMSE-verktyget och kontrollera status för slutförande av hello-kanal.</span><span class="sxs-lookup"><span data-stu-id="70ee1-168">Navigate back toohello AMSE tool, and check on hello channel completion status.</span></span> <span data-ttu-id="70ee1-169">När hello tillstånd har ändrats från **Start** för**kör**, du kan hämta hello indata-URL.</span><span class="sxs-lookup"><span data-stu-id="70ee1-169">Once hello State has changed from **Starting** too**Running**, you can get hello input URL.</span></span>

    <span data-ttu-id="70ee1-170">När du kör hello kanal högerklickar du på hello Kanalnamn, Navigera ned toohover över **kopiera indata URL tooclipboard** och välj sedan **primära indata-URL**.</span><span class="sxs-lookup"><span data-stu-id="70ee1-170">When hello channel is running, right click hello channel name, navigate down toohover over **Copy Input URL tooclipboard** and then select **Primary Input  URL**.</span></span>  

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster6.png)
9. <span data-ttu-id="70ee1-172">Klistra in informationen i hello **plats** fältet under **Flash Server** inom hello Tricaster projekt.</span><span class="sxs-lookup"><span data-stu-id="70ee1-172">Paste this information in hello **Location** field under **Flash Server** within hello Tricaster project.</span></span> <span data-ttu-id="70ee1-173">Tilldela ett stream-namn i hello **dataström-ID** fältet.</span><span class="sxs-lookup"><span data-stu-id="70ee1-173">Also assign a stream name in hello **Stream ID** field.</span></span>

    <span data-ttu-id="70ee1-174">Om strömmen information har lagts till toohello FMLE profil, även importeras toothis avsnittet genom att klicka på **importinställningarna**, navigera toohello sparade FMLE profil och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="70ee1-174">If stream information was added toohello FMLE profile, it can also be imported toothis section by clicking **Import Settings**, navigating toohello saved FMLE profile and clicking **OK**.</span></span> <span data-ttu-id="70ee1-175">hello relevanta fält i Flash Server bör fyllas i automatiskt med hello information från FMLE.</span><span class="sxs-lookup"><span data-stu-id="70ee1-175">hello relevant Flash Server fields should populate with hello information from FMLE.</span></span>

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster7.png)
10. <span data-ttu-id="70ee1-177">När du är klar klickar du på **OK** längst hello hello-skärmen.</span><span class="sxs-lookup"><span data-stu-id="70ee1-177">When finished, click **OK** at hello bottom of hello screen.</span></span> <span data-ttu-id="70ee1-178">När video och ljud indata till hello Tricaster är klar börjar strömning tooAMS genom att klicka på hello **dataströmmen** knappen.</span><span class="sxs-lookup"><span data-stu-id="70ee1-178">When video and audio inputs into hello Tricaster are ready, begin streaming tooAMS by clicking hello **Stream** button.</span></span>

     ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster11.png)

> [!IMPORTANT]
> <span data-ttu-id="70ee1-180">Innan du klickar på **dataströmmen**, du **måste** se till att hello kanalen är klar.</span><span class="sxs-lookup"><span data-stu-id="70ee1-180">Before you click **Stream**, you **must** ensure that hello Channel is ready.</span></span>
> <span data-ttu-id="70ee1-181">Kontrollera också att inte tooleave hello kanal med tillståndet klart utan ett inkommande bidrag feed för > 15 minuter.</span><span class="sxs-lookup"><span data-stu-id="70ee1-181">Also, make sure not tooleave hello Channel in a ready state without an input contribution feed for longer than > 15 minutes.</span></span>
>
>

## <a name="test-playback"></a><span data-ttu-id="70ee1-182">Testa uppspelning</span><span class="sxs-lookup"><span data-stu-id="70ee1-182">Test playback</span></span>
<span data-ttu-id="70ee1-183">Navigera toohello AMSE-verktyget och högerklicka på hello kanal toobe testas.</span><span class="sxs-lookup"><span data-stu-id="70ee1-183">Navigate toohello AMSE tool, and right click hello channel toobe tested.</span></span> <span data-ttu-id="70ee1-184">Hovra över hello-menyn **uppspelning hello Preview** och välj **med Azure Media Player**.</span><span class="sxs-lookup"><span data-stu-id="70ee1-184">From hello menu, hover over **Playback hello Preview** and select **with Azure Media Player**.</span></span>  

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster8.png)

<span data-ttu-id="70ee1-185">Om hello dataströmmen visas i hello player har hello kodare varit korrekt konfigurerade tooconnect tooAMS.</span><span class="sxs-lookup"><span data-stu-id="70ee1-185">If hello stream appears in hello player, then hello encoder has been properly configured tooconnect tooAMS.</span></span>

<span data-ttu-id="70ee1-186">Om ett fel tas emot måste hello kanal toobe Återställ kodare inställningar och justeras.</span><span class="sxs-lookup"><span data-stu-id="70ee1-186">If an error is received, hello channel will need toobe reset and encoder settings adjusted.</span></span> <span data-ttu-id="70ee1-187">Se hello [felsökning](media-services-troubleshooting-live-streaming.md) avsnittet som vägledning.</span><span class="sxs-lookup"><span data-stu-id="70ee1-187">Please see hello [troubleshooting](media-services-troubleshooting-live-streaming.md) topic for guidance.</span></span>  

## <a name="create-a-program"></a><span data-ttu-id="70ee1-188">Skapa ett program</span><span class="sxs-lookup"><span data-stu-id="70ee1-188">Create a program</span></span>
1. <span data-ttu-id="70ee1-189">När kanalen uppspelning bekräftas, skapa ett program.</span><span class="sxs-lookup"><span data-stu-id="70ee1-189">Once channel playback is confirmed, create a program.</span></span> <span data-ttu-id="70ee1-190">Under hello **Live** i hello AMSE-verktyget, högerklickar du i området för hello-programmet och välj **Skapa nytt Program**.</span><span class="sxs-lookup"><span data-stu-id="70ee1-190">Under hello **Live** tab in hello AMSE tool, right click within hello program area and select **Create New Program**.</span></span>  

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster9.png)
2. <span data-ttu-id="70ee1-192">Namnge programmet hello och, om det behövs, justera hello **längd** (som standard too4 timmar).</span><span class="sxs-lookup"><span data-stu-id="70ee1-192">Name hello program and, if needed, adjust hello **Archive Window Length** (which defaults too4 hours).</span></span> <span data-ttu-id="70ee1-193">Du kan också ange en lagringsplats eller lämna som hello standard.</span><span class="sxs-lookup"><span data-stu-id="70ee1-193">You can also specify a storage location or leave as hello default.</span></span>  
3. <span data-ttu-id="70ee1-194">Kontrollera hello **Start hello programmet nu** rutan.</span><span class="sxs-lookup"><span data-stu-id="70ee1-194">Check hello **Start hello Program now** box.</span></span>
4. <span data-ttu-id="70ee1-195">Klicka på **skapa Program**.</span><span class="sxs-lookup"><span data-stu-id="70ee1-195">Click **Create Program**.</span></span>  

    >[!NOTE]
    ><span data-ttu-id="70ee1-196">Skapa en program tar mindre tid än att skapa en kanal.</span><span class="sxs-lookup"><span data-stu-id="70ee1-196">Program creation takes less time than channel creation.</span></span>
        
5. <span data-ttu-id="70ee1-197">När hello programmet körs kan bekräfta uppspelning genom att högerklicka på programmet hello och navigera för**uppspelning hello program** och sedan välja **med Azure Media Player**.</span><span class="sxs-lookup"><span data-stu-id="70ee1-197">Once hello program is running, confirm playback by right clicking hello program and navigating too**Playback hello program(s)** and then selecting **with Azure Media Player**.</span></span>  
6. <span data-ttu-id="70ee1-198">När bekräftat, högerklicka på programmet hello igen och välj **kopiera hello utdata URL tooClipboard** (eller hämta den här informationen från hello **programmet information och inställningar** alternativet hello-menyn).</span><span class="sxs-lookup"><span data-stu-id="70ee1-198">Once confirmed, right click hello program again and select **Copy hello Output URL tooClipboard** (or retrieve this information from hello **Program information and settings** option from hello menu).</span></span>

<span data-ttu-id="70ee1-199">hello är nu redo toobe inbäddat i en spelare eller distribuerade tooan målgruppen för live visning.</span><span class="sxs-lookup"><span data-stu-id="70ee1-199">hello stream is now ready toobe embedded in a player, or distributed tooan audience for live viewing.</span></span>  

## <a name="troubleshooting"></a><span data-ttu-id="70ee1-200">Felsökning</span><span class="sxs-lookup"><span data-stu-id="70ee1-200">Troubleshooting</span></span>
<span data-ttu-id="70ee1-201">Se hello [felsökning](media-services-troubleshooting-live-streaming.md) avsnittet som vägledning.</span><span class="sxs-lookup"><span data-stu-id="70ee1-201">Please see hello [troubleshooting](media-services-troubleshooting-live-streaming.md) topic for guidance.</span></span>

## <a name="next-step"></a><span data-ttu-id="70ee1-202">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="70ee1-202">Next step</span></span>
<span data-ttu-id="70ee1-203">Granska sökvägarna för Media Services-utbildning.</span><span class="sxs-lookup"><span data-stu-id="70ee1-203">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="70ee1-204">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="70ee1-204">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
