---
title: "Konfigurera Telestream Wirecast-kodaren om du vill skicka en direktsänd dataström med enkel bithastighet | Microsoft Docs"
description: "Det här avsnittet visar hur du konfigurerar Wirecast livekodaren för att skicka en dataström med enkel bithastighet till AMS-kanaler som är aktiverade för live encoding. "
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
ms.openlocfilehash: c4df14f24650ce431dfb31cc774cab6d3cf3aef0
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="use-the-wirecast-encoder-to-send-a-single-bitrate-live-stream"></a><span data-ttu-id="b8d3f-103">Använda Wirecast-kodaren för att skicka en direktsänd dataström med enkel bithastighet</span><span class="sxs-lookup"><span data-stu-id="b8d3f-103">Use the Wirecast encoder to send a single bitrate live stream</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b8d3f-104">Wirecast</span><span class="sxs-lookup"><span data-stu-id="b8d3f-104">Wirecast</span></span>](media-services-configure-wirecast-live-encoder.md)
> * [<span data-ttu-id="b8d3f-105">Elemental Live</span><span class="sxs-lookup"><span data-stu-id="b8d3f-105">Elemental Live</span></span>](media-services-configure-elemental-live-encoder.md)
> * [<span data-ttu-id="b8d3f-106">Tricaster</span><span class="sxs-lookup"><span data-stu-id="b8d3f-106">Tricaster</span></span>](media-services-configure-tricaster-live-encoder.md)
> * [<span data-ttu-id="b8d3f-107">FMLE</span><span class="sxs-lookup"><span data-stu-id="b8d3f-107">FMLE</span></span>](media-services-configure-fmle-live-encoder.md)
>
>

<span data-ttu-id="b8d3f-108">Det här avsnittet visar hur du konfigurerar den [Telestream Wirecast](http://www.telestream.net/wirecast/overview.htm) livekodare för att skicka en dataström med enkel bithastighet till AMS kanaler som är aktiverade för live encoding.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-108">This topic shows how to configure the [Telestream Wirecast](http://www.telestream.net/wirecast/overview.htm) live encoder to send a single bitrate stream to AMS channels that are enabled for live encoding.</span></span>  <span data-ttu-id="b8d3f-109">Mer information finns i [Arbeta med kanaler som är aktiverade för att utföra Live Encoding med Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span><span class="sxs-lookup"><span data-stu-id="b8d3f-109">For more information, see [Working with Channels that are Enabled to Perform Live Encoding with Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span></span>

<span data-ttu-id="b8d3f-110">Den här kursen visar hur du hanterar Azure Media Services (AMS) med Azure Media Services Explorer (AMSE)-verktyget.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-110">This tutorial shows how to manage Azure Media Services (AMS) with Azure Media Services Explorer (AMSE) tool.</span></span> <span data-ttu-id="b8d3f-111">Det här verktyget körs bara på Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-111">This tool only runs on Windows PC.</span></span> <span data-ttu-id="b8d3f-112">Om du är på Mac- eller Linux använder Azure-portalen för att skapa [kanaler](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) och [program](media-services-portal-creating-live-encoder-enabled-channel.md).</span><span class="sxs-lookup"><span data-stu-id="b8d3f-112">If you are on Mac or Linux, use the Azure portal to create [channels](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) and [programs](media-services-portal-creating-live-encoder-enabled-channel.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b8d3f-113">Krav</span><span class="sxs-lookup"><span data-stu-id="b8d3f-113">Prerequisites</span></span>
* [<span data-ttu-id="b8d3f-114">Skapa ett Azure Media Services-konto</span><span class="sxs-lookup"><span data-stu-id="b8d3f-114">Create an Azure Media Services account</span></span>](media-services-portal-create-account.md)
* <span data-ttu-id="b8d3f-115">Se till att det finns en Strömningsslutpunkt som körs.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-115">Ensure there is a Streaming Endpoint running.</span></span> <span data-ttu-id="b8d3f-116">Mer information finns i [hanterar Strömningsslutpunkter i ett Media Services-konto](media-services-portal-manage-streaming-endpoints.md)</span><span class="sxs-lookup"><span data-stu-id="b8d3f-116">For more information, see [Manage Streaming Endpoints in a Media Services Account](media-services-portal-manage-streaming-endpoints.md)</span></span>
* <span data-ttu-id="b8d3f-117">Installera den senaste versionen av den [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) verktyget.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-117">Install the latest version of the [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) tool.</span></span>
* <span data-ttu-id="b8d3f-118">Starta verktyget och Anslut till AMS-kontot.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-118">Launch the tool and connect to your AMS account.</span></span>

## <a name="tips"></a><span data-ttu-id="b8d3f-119">Tips</span><span class="sxs-lookup"><span data-stu-id="b8d3f-119">Tips</span></span>
* <span data-ttu-id="b8d3f-120">När det är möjligt använda ett inbyggt internet-anslutning.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-120">Whenever possible, use a hardwired internet connection.</span></span>
* <span data-ttu-id="b8d3f-121">En bra tumregel samband med fastställandet av krav på bandbredd är att dubbla strömmande bithastighet.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-121">A good rule of thumb when determining bandwidth requirements is to double the streaming bitrates.</span></span> <span data-ttu-id="b8d3f-122">Även om detta inte är en nödvändighet hjälper minimera effekten av överbelastning.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-122">While this is not a mandatory requirement, it will help mitigate the impact of network congestion.</span></span>
* <span data-ttu-id="b8d3f-123">När du använder programvara baserat kodare, Stäng alla onödiga program.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-123">When using software based encoders, close out any unnecessary programs.</span></span>

## <a name="create-a-channel"></a><span data-ttu-id="b8d3f-124">Skapa en kanal</span><span class="sxs-lookup"><span data-stu-id="b8d3f-124">Create a channel</span></span>
1. <span data-ttu-id="b8d3f-125">I verktyget AMSE navigerar du till den **Live** fliken och högerklicka i området för kanal.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-125">In the AMSE tool, navigate to the **Live** tab, and right click within the channel area.</span></span> <span data-ttu-id="b8d3f-126">Välj **skapa kanal...**</span><span class="sxs-lookup"><span data-stu-id="b8d3f-126">Select **Create channel…**</span></span> <span data-ttu-id="b8d3f-127">från menyn.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-127">from the menu.</span></span>

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast1.png)

2. <span data-ttu-id="b8d3f-129">Ange ett Kanalnamn beskrivningsfältet är valfritt.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-129">Specify a channel name, the description field is optional.</span></span> <span data-ttu-id="b8d3f-130">Markera under inställningar för kanal **Standard** för alternativet Live Encoding med protokollet indata som angetts till **RTMP**.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-130">Under Channel Settings, select **Standard** for the Live Encoding option, with the Input Protocol set to **RTMP**.</span></span> <span data-ttu-id="b8d3f-131">Du kan lämna alla andra inställningar som är.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-131">You can leave all other settings as is.</span></span>

    <span data-ttu-id="b8d3f-132">Kontrollera att den **starta den nya kanalen nu** är markerad.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-132">Make sure the **Start the new channel now** is selected.</span></span>

3. <span data-ttu-id="b8d3f-133">Klicka på **skapa kanal**.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-133">Click **Create Channel**.</span></span>

   ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast2.png)

> [!NOTE]
> <span data-ttu-id="b8d3f-135">Kanalen kan ta upp till 20 minuter för att starta.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-135">The channel can take as long as 20 minutes to start.</span></span>
>
>

<span data-ttu-id="b8d3f-136">När kanalen startar kan du [konfigurera kodaren](media-services-configure-wirecast-live-encoder.md#configure_wirecast_rtmp).</span><span class="sxs-lookup"><span data-stu-id="b8d3f-136">While the channel is starting you can [configure the encoder](media-services-configure-wirecast-live-encoder.md#configure_wirecast_rtmp).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b8d3f-137">Observera att faktureringen påbörjas så snart kanal blir klar att användas.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-137">Note that billing starts as soon as Channel goes into a ready state.</span></span> <span data-ttu-id="b8d3f-138">Mer information finns i [kanalens tillstånd](media-services-manage-live-encoder-enabled-channels.md#states).</span><span class="sxs-lookup"><span data-stu-id="b8d3f-138">For more information, see [Channel's states](media-services-manage-live-encoder-enabled-channels.md#states).</span></span>
>
>

## <span data-ttu-id="b8d3f-139"><a id=configure_wirecast_rtmp></a>Konfigurera Telestream Wirecast-kodaren</span><span class="sxs-lookup"><span data-stu-id="b8d3f-139"><a id=configure_wirecast_rtmp></a>Configure the Telestream Wirecast encoder</span></span>
<span data-ttu-id="b8d3f-140">I den här kursen används följande inställningar för utdata.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-140">In this tutorial the following output settings are used.</span></span> <span data-ttu-id="b8d3f-141">Resten av det här avsnittet beskriver konfigurationssteg i detalj.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-141">The rest of this section describes configuration steps in more detail.</span></span>

<span data-ttu-id="b8d3f-142">**Video**:</span><span class="sxs-lookup"><span data-stu-id="b8d3f-142">**Video**:</span></span>

* <span data-ttu-id="b8d3f-143">Codec: H.264</span><span class="sxs-lookup"><span data-stu-id="b8d3f-143">Codec: H.264</span></span>
* <span data-ttu-id="b8d3f-144">Profil: Hög (nivå 4.0)</span><span class="sxs-lookup"><span data-stu-id="b8d3f-144">Profile: High (Level 4.0)</span></span>
* <span data-ttu-id="b8d3f-145">Bithastighet: 5000 kbit/s</span><span class="sxs-lookup"><span data-stu-id="b8d3f-145">Bitrate: 5000 kbps</span></span>
* <span data-ttu-id="b8d3f-146">Keyframe: 2 sekunder (60 sekunder)</span><span class="sxs-lookup"><span data-stu-id="b8d3f-146">Keyframe: 2 seconds (60 seconds)</span></span>
* <span data-ttu-id="b8d3f-147">RAM-hastighet: 30</span><span class="sxs-lookup"><span data-stu-id="b8d3f-147">Frame Rate: 30</span></span>

<span data-ttu-id="b8d3f-148">**Ljud**:</span><span class="sxs-lookup"><span data-stu-id="b8d3f-148">**Audio**:</span></span>

* <span data-ttu-id="b8d3f-149">Codec: AAC (LC)</span><span class="sxs-lookup"><span data-stu-id="b8d3f-149">Codec: AAC (LC)</span></span>
* <span data-ttu-id="b8d3f-150">Bithastighet: 192 kbit/s</span><span class="sxs-lookup"><span data-stu-id="b8d3f-150">Bitrate: 192 kbps</span></span>
* <span data-ttu-id="b8d3f-151">Samplingsfrekvens: 44.1 kHz</span><span class="sxs-lookup"><span data-stu-id="b8d3f-151">Sample Rate: 44.1 kHz</span></span>

### <a name="configuration-steps"></a><span data-ttu-id="b8d3f-152">Konfigurationssteg</span><span class="sxs-lookup"><span data-stu-id="b8d3f-152">Configuration steps</span></span>
1. <span data-ttu-id="b8d3f-153">Öppna Telestream Wirecast-programmet på datorn som används och konfigurera för RTMP strömning.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-153">Open the Telestream Wirecast application on the machine being used, and set up for RTMP streaming.</span></span>
2. <span data-ttu-id="b8d3f-154">Konfigurera utdata genom att navigera till den **utdata** fliken och markera **utdata inställningar...** .</span><span class="sxs-lookup"><span data-stu-id="b8d3f-154">Configure the output by navigating to the **Output** tab and selecting **Output Settings…**.</span></span>

    <span data-ttu-id="b8d3f-155">Kontrollera att den **utdata mål** är inställd på **RTMP-Server**.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-155">Make sure the **Output Destination** is set to **RTMP Server**.</span></span>
3. <span data-ttu-id="b8d3f-156">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-156">Click **OK**.</span></span>
4. <span data-ttu-id="b8d3f-157">På sidan Ange den **mål** fältet ska vara **Azure Media Services**.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-157">On the settings page, set the **Destination** field to be **Azure Media Services**.</span></span>

    <span data-ttu-id="b8d3f-158">Kodning profilen är förvald till **Azure H.264 720 p 16:9 (minst 1 280 x 720)**.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-158">The Encoding profile is pre-selected to **Azure H.264 720p 16:9 (1280x720)**.</span></span> <span data-ttu-id="b8d3f-159">Om du vill anpassa dessa inställningar väljer du växeln ikonen till höger om nedrullningsbara ned och välj sedan **nya förinställningen**.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-159">To customize these settings, select the gear icon to the right of the drop down, and then choose **New Preset**.</span></span>

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast3.png)
5. <span data-ttu-id="b8d3f-161">Konfigurera kodare förinställningar.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-161">Configure encoder presets.</span></span>

    <span data-ttu-id="b8d3f-162">Namn för förinställningen och Sök efter följande rekommenderade inställningar:</span><span class="sxs-lookup"><span data-stu-id="b8d3f-162">Name the preset, and check for the following recommended settings:</span></span>

    <span data-ttu-id="b8d3f-163">**Video**</span><span class="sxs-lookup"><span data-stu-id="b8d3f-163">**Video**</span></span>

   * <span data-ttu-id="b8d3f-164">Kodaren: MainConcept H.264</span><span class="sxs-lookup"><span data-stu-id="b8d3f-164">Encoder: MainConcept H.264</span></span>
   * <span data-ttu-id="b8d3f-165">Bildrutor per sekund: 30</span><span class="sxs-lookup"><span data-stu-id="b8d3f-165">Frames per Second: 30</span></span>
   * <span data-ttu-id="b8d3f-166">Genomsnittlig bithastighet: 5000 kbits per sekund (kan justeras utifrån nätverksbegränsningar)</span><span class="sxs-lookup"><span data-stu-id="b8d3f-166">Average bit rate: 5000 kbits/sec (Can be adjusted based on network limitations)</span></span>
   * <span data-ttu-id="b8d3f-167">Profil: Main</span><span class="sxs-lookup"><span data-stu-id="b8d3f-167">Profile: Main</span></span>
   * <span data-ttu-id="b8d3f-168">Nyckelbilden var: 60 ramar</span><span class="sxs-lookup"><span data-stu-id="b8d3f-168">Key frame every: 60 frames</span></span>

    <span data-ttu-id="b8d3f-169">**Ljud**</span><span class="sxs-lookup"><span data-stu-id="b8d3f-169">**Audio**</span></span>

   * <span data-ttu-id="b8d3f-170">Mål bithastighet: 192 kbits per sekund</span><span class="sxs-lookup"><span data-stu-id="b8d3f-170">Target bit rate: 192 kbits/sec</span></span>
   * <span data-ttu-id="b8d3f-171">Samplingsfrekvens: 44 100 kHz</span><span class="sxs-lookup"><span data-stu-id="b8d3f-171">Sample Rate: 44.100 kHz</span></span>

     ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast4.png)
6. <span data-ttu-id="b8d3f-173">Tryck på **spara**.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-173">Press **Save**.</span></span>

    <span data-ttu-id="b8d3f-174">Fältet avkodning har nu den nya profilen kan väljas.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-174">The Encoding field now has the newly created profile available for selection.</span></span>

    <span data-ttu-id="b8d3f-175">Kontrollera att den nya profilen är markerad.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-175">Make sure the new profile is selected.</span></span>
7. <span data-ttu-id="b8d3f-176">Hämta kanalen input URL: en för att tilldela Wirecast **RTMP Endpoint**.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-176">Get the channel's input URL in order to assign it to the Wirecast **RTMP Endpoint**.</span></span>

    <span data-ttu-id="b8d3f-177">Gå tillbaka till verktyget AMSE och kontrollera status för slutförande kanal.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-177">Navigate back to the AMSE tool, and check on the channel completion status.</span></span> <span data-ttu-id="b8d3f-178">När läget har ändrats från **Start** till **kör**, du kan hämta indata-URL.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-178">Once the State has changed from **Starting** to **Running**, you can get the input URL.</span></span>

    <span data-ttu-id="b8d3f-179">När kanalen körs, högerklickar du på dess namn, navigera till hovra över **kopiera indata-URL till Urklipp** och välj sedan **primära indata-URL**.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-179">When the channel is running, right click the channel name, navigate down to hover over **Copy Input URL to clipboard** and then select **Primary Input  URL**.</span></span>  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast6.png)
8. <span data-ttu-id="b8d3f-181">I Wirecast **inställningar för utdata** och klistra in den här informationen i den **adress** i utdata och tilldela ett namn på dataströmmen.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-181">In the Wirecast **Output Settings** window, paste this information in the **Address** field of the output section, and assign a stream name.</span></span>

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast5.png)

1. <span data-ttu-id="b8d3f-183">Välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-183">Select **OK**.</span></span>
2. <span data-ttu-id="b8d3f-184">På primära **Wirecast** skärmen, bekräfta indatakällor för video och ljud är klar och tryck sedan på **dataströmmen** i det övre vänstra hörnet.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-184">On the main **Wirecast** screen, confirm input sources for video and audio are ready and then hit **Stream** in the top left hand corner.</span></span>

   ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast7.png)

> [!IMPORTANT]
> <span data-ttu-id="b8d3f-186">Innan du klickar på **dataströmmen**, du **måste** se till att kanalen är klar.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-186">Before you click **Stream**, you **must** ensure that the Channel is ready.</span></span>
> <span data-ttu-id="b8d3f-187">Kontrollera också att kanalen klar utan ett inkommande bidrag feed för > 15 minuter.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-187">Also, make sure not to leave the Channel in a ready state without an input contribution feed for longer than > 15 minutes.</span></span>
>
>

## <a name="test-playback"></a><span data-ttu-id="b8d3f-188">Testa uppspelning</span><span class="sxs-lookup"><span data-stu-id="b8d3f-188">Test playback</span></span>

<span data-ttu-id="b8d3f-189">Navigera till verktyget AMSE och högerklicka på kanalen som ska testas.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-189">Navigate to the AMSE tool, and right click the channel to be tested.</span></span> <span data-ttu-id="b8d3f-190">Från menyn hovra över **uppspelning förhandsgranskningen** och välj **med Azure Media Player**.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-190">From the menu, hover over **Playback the Preview** and select **with Azure Media Player**.</span></span>  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast8.png)

<span data-ttu-id="b8d3f-191">Om strömmen visas i media player, har sedan kodaren konfigurerats korrekt för att ansluta till AMS.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-191">If the stream appears in the player, then the encoder has been properly configured to connect to AMS.</span></span>

<span data-ttu-id="b8d3f-192">Om ett fel tas emot kanalen kommer att behöva återställas och anpassa inställningar för kodare.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-192">If an error is received, the channel will need to be reset and encoder settings adjusted.</span></span> <span data-ttu-id="b8d3f-193">Finns det [felsökning](media-services-troubleshooting-live-streaming.md) avsnittet som vägledning.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-193">Please see the [troubleshooting](media-services-troubleshooting-live-streaming.md) topic for guidance.</span></span>  

## <a name="create-a-program"></a><span data-ttu-id="b8d3f-194">Skapa ett program</span><span class="sxs-lookup"><span data-stu-id="b8d3f-194">Create a program</span></span>
1. <span data-ttu-id="b8d3f-195">När kanalen uppspelning bekräftas, skapa ett program.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-195">Once channel playback is confirmed, create a program.</span></span> <span data-ttu-id="b8d3f-196">Under den **Live** i verktyget AMSE, högerklicka i området för programmet och välj **Skapa nytt Program**.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-196">Under the **Live** tab in the AMSE tool, right click within the program area and select **Create New Program**.</span></span>  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast9.png)
2. <span data-ttu-id="b8d3f-198">Namnge programmet och, om det behövs, justera det **längd** (som standard 4 timmar).</span><span class="sxs-lookup"><span data-stu-id="b8d3f-198">Name the program and, if needed, adjust the **Archive Window Length** (which defaults to 4 hours).</span></span> <span data-ttu-id="b8d3f-199">Du kan också ange en lagringsplats eller lämna som standard.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-199">You can also specify a storage location or leave as the default.</span></span>  
3. <span data-ttu-id="b8d3f-200">Kontrollera den **starta programmet nu** rutan.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-200">Check the **Start the Program now** box.</span></span>
4. <span data-ttu-id="b8d3f-201">Klicka på **skapa Program**.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-201">Click **Create Program**.</span></span>  

   >[!NOTE]
   ><span data-ttu-id="b8d3f-202">Skapa en program tar mindre tid än att skapa en kanal.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-202">Program creation takes less time than channel creation.</span></span>
       
5. <span data-ttu-id="b8d3f-203">När programmet körs kan bekräfta uppspelning genom att högerklicka på programmet och gå till **uppspelning av program** och sedan välja **med Azure Media Player**.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-203">Once the program is running, confirm playback by right clicking the program and navigating to **Playback the program(s)** and then selecting **with Azure Media Player**.</span></span>  
6. <span data-ttu-id="b8d3f-204">När bekräftat, högerklicka på programmet och välj **kopiera den URL som utdata till Urklipp** (eller hämta den här informationen från den **programmet information och inställningar** alternativ på menyn).</span><span class="sxs-lookup"><span data-stu-id="b8d3f-204">Once confirmed, right click the program again and select **Copy the Output URL to Clipboard** (or retrieve this information from the **Program information and settings** option from the menu).</span></span>

<span data-ttu-id="b8d3f-205">Dataströmmen är nu redo att vara inbäddat i ett player eller distribuerats till en målgrupp för live visning.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-205">The stream is now ready to be embedded in a player, or distributed to an audience for live viewing.</span></span>  

## <a name="troubleshooting"></a><span data-ttu-id="b8d3f-206">Felsökning</span><span class="sxs-lookup"><span data-stu-id="b8d3f-206">Troubleshooting</span></span>
<span data-ttu-id="b8d3f-207">Finns det [felsökning](media-services-troubleshooting-live-streaming.md) avsnittet som vägledning.</span><span class="sxs-lookup"><span data-stu-id="b8d3f-207">Please see the [troubleshooting](media-services-troubleshooting-live-streaming.md) topic for guidance.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="b8d3f-208">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="b8d3f-208">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="b8d3f-209">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="b8d3f-209">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
