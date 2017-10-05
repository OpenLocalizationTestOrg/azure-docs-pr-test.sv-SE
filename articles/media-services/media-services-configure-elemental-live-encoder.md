---
title: "Konfigurera grundämne Live-kodaren om du vill skicka en direktsänd dataström med enkel bithastighet | Microsoft Docs"
description: "Det här avsnittet visar hur du konfigurerar grundämne Live-kodaren för att skicka en dataström med enkel bithastighet till AMS-kanaler som är aktiverade för live encoding."
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
ms.openlocfilehash: 668a3ab46a70c0ee25fa87031d27c0f4333ec89c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="use-the-elemental-live-encoder-to-send-a-single-bitrate-live-stream"></a><span data-ttu-id="1888e-103">Använda kodaren grundämne Live för att skicka en direktsänd dataström med enkel bithastighet</span><span class="sxs-lookup"><span data-stu-id="1888e-103">Use the Elemental Live encoder to send a single bitrate live stream</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="1888e-104">Elemental Live</span><span class="sxs-lookup"><span data-stu-id="1888e-104">Elemental Live</span></span>](media-services-configure-elemental-live-encoder.md)
> * [<span data-ttu-id="1888e-105">Tricaster</span><span class="sxs-lookup"><span data-stu-id="1888e-105">Tricaster</span></span>](media-services-configure-tricaster-live-encoder.md)
> * [<span data-ttu-id="1888e-106">Wirecast</span><span class="sxs-lookup"><span data-stu-id="1888e-106">Wirecast</span></span>](media-services-configure-wirecast-live-encoder.md)
> * [<span data-ttu-id="1888e-107">FMLE</span><span class="sxs-lookup"><span data-stu-id="1888e-107">FMLE</span></span>](media-services-configure-fmle-live-encoder.md)
>
>

<span data-ttu-id="1888e-108">Det här avsnittet visar hur du konfigurerar den [grundämne Live](http://www.elementaltechnologies.com/products/elemental-live) att skicka en dataström med enkel bithastighet till AMS kanaler som är aktiverade för live encoding.</span><span class="sxs-lookup"><span data-stu-id="1888e-108">This topic shows how to configure the [Elemental Live](http://www.elementaltechnologies.com/products/elemental-live) encoder to send a single bitrate stream to AMS channels that are enabled for live encoding.</span></span>  <span data-ttu-id="1888e-109">Mer information finns i [Arbeta med kanaler som är aktiverade för att utföra Live Encoding med Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span><span class="sxs-lookup"><span data-stu-id="1888e-109">For more information, see [Working with Channels that are Enabled to Perform Live Encoding with Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span></span>

<span data-ttu-id="1888e-110">Den här kursen visar hur du hanterar Azure Media Services (AMS) med Azure Media Services Explorer (AMSE)-verktyget.</span><span class="sxs-lookup"><span data-stu-id="1888e-110">This tutorial shows how to manage Azure Media Services (AMS) with Azure Media Services Explorer (AMSE) tool.</span></span> <span data-ttu-id="1888e-111">Det här verktyget körs bara på Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="1888e-111">This tool only runs on Windows PC.</span></span> <span data-ttu-id="1888e-112">Om du är på Mac- eller Linux använder Azure-portalen för att skapa [kanaler](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) och [program](media-services-portal-creating-live-encoder-enabled-channel.md).</span><span class="sxs-lookup"><span data-stu-id="1888e-112">If you are on Mac or Linux, use the Azure portal to create [channels](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) and [programs](media-services-portal-creating-live-encoder-enabled-channel.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1888e-113">Krav</span><span class="sxs-lookup"><span data-stu-id="1888e-113">Prerequisites</span></span>
* <span data-ttu-id="1888e-114">Måste ha kunskaper om med grundämne Live Webbgränssnitt för att skapa direktsända händelser.</span><span class="sxs-lookup"><span data-stu-id="1888e-114">Must have a working knowledge of using Elemental Live web interface to create live events.</span></span>
* [<span data-ttu-id="1888e-115">Skapa ett Azure Media Services-konto</span><span class="sxs-lookup"><span data-stu-id="1888e-115">Create an Azure Media Services account</span></span>](media-services-portal-create-account.md)
* <span data-ttu-id="1888e-116">Se till att det finns en Strömningsslutpunkt som körs.</span><span class="sxs-lookup"><span data-stu-id="1888e-116">Ensure there is a Streaming Endpoint running.</span></span> <span data-ttu-id="1888e-117">Mer information finns i [hanterar Strömningsslutpunkter i ett Media Services-konto](media-services-portal-manage-streaming-endpoints.md).</span><span class="sxs-lookup"><span data-stu-id="1888e-117">For more information, see [Manage Streaming Endpoints in a Media Services Account](media-services-portal-manage-streaming-endpoints.md).</span></span>
* <span data-ttu-id="1888e-118">Installera den senaste versionen av den [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) verktyget.</span><span class="sxs-lookup"><span data-stu-id="1888e-118">Install the latest version of the [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) tool.</span></span>
* <span data-ttu-id="1888e-119">Starta verktyget och Anslut till AMS-kontot.</span><span class="sxs-lookup"><span data-stu-id="1888e-119">Launch the tool and connect to your AMS account.</span></span>

## <a name="tips"></a><span data-ttu-id="1888e-120">Tips</span><span class="sxs-lookup"><span data-stu-id="1888e-120">Tips</span></span>
* <span data-ttu-id="1888e-121">När det är möjligt använda ett inbyggt internet-anslutning.</span><span class="sxs-lookup"><span data-stu-id="1888e-121">Whenever possible, use a hardwired internet connection.</span></span>
* <span data-ttu-id="1888e-122">En bra tumregel samband med fastställandet av krav på bandbredd är att dubbla strömmande bithastighet.</span><span class="sxs-lookup"><span data-stu-id="1888e-122">A good rule of thumb when determining bandwidth requirements is to double the streaming bitrates.</span></span> <span data-ttu-id="1888e-123">Även om detta inte är en nödvändighet hjälper minimera effekten av överbelastning.</span><span class="sxs-lookup"><span data-stu-id="1888e-123">While this is not a mandatory requirement, it will help mitigate the impact of network congestion.</span></span>
* <span data-ttu-id="1888e-124">När du använder programvara baserat kodare, Stäng alla onödiga program.</span><span class="sxs-lookup"><span data-stu-id="1888e-124">When using software based encoders, close out any unnecessary programs.</span></span>

## <a name="elemental-live-with-rtp-ingest"></a><span data-ttu-id="1888e-125">Mata in grundämne Live med RTP</span><span class="sxs-lookup"><span data-stu-id="1888e-125">Elemental Live with RTP ingest</span></span>
<span data-ttu-id="1888e-126">Det här avsnittet visar hur du konfigurerar grundämne Live-kodaren som skickar en direktsänd dataström med enkel bithastighet över RTP.</span><span class="sxs-lookup"><span data-stu-id="1888e-126">This section shows how to configure the Elemental Live encoder that sends a single bitrate live stream over RTP.</span></span>  <span data-ttu-id="1888e-127">Mer information finns i [MPEG-TS dataströmmen över RTP](media-services-manage-live-encoder-enabled-channels.md#channel).</span><span class="sxs-lookup"><span data-stu-id="1888e-127">For more information, see [MPEG-TS stream over RTP](media-services-manage-live-encoder-enabled-channels.md#channel).</span></span>

### <a name="create-a-channel"></a><span data-ttu-id="1888e-128">Skapa en kanal</span><span class="sxs-lookup"><span data-stu-id="1888e-128">Create a channel</span></span>

1. <span data-ttu-id="1888e-129">I verktyget AMSE navigerar du till den **Live** fliken och högerklicka i området för kanal.</span><span class="sxs-lookup"><span data-stu-id="1888e-129">In the AMSE tool, navigate to the **Live** tab, and right click within the channel area.</span></span> <span data-ttu-id="1888e-130">Välj **skapa kanal...**</span><span class="sxs-lookup"><span data-stu-id="1888e-130">Select **Create channel…**</span></span> <span data-ttu-id="1888e-131">från menyn.</span><span class="sxs-lookup"><span data-stu-id="1888e-131">from the menu.</span></span>

    ![Grundämne](./media/media-services-elemental-live-encoder/media-services-elemental1.png)

2. <span data-ttu-id="1888e-133">Ange ett Kanalnamn beskrivningsfältet är valfritt.</span><span class="sxs-lookup"><span data-stu-id="1888e-133">Specify a channel name, the description field is optional.</span></span> <span data-ttu-id="1888e-134">Markera under inställningar för kanal **Standard** för alternativet Live Encoding med protokollet indata som angetts till **RTP (MPEG-TS)**.</span><span class="sxs-lookup"><span data-stu-id="1888e-134">Under Channel Settings, select **Standard** for the Live Encoding option, with the Input Protocol set to **RTP (MPEG-TS)**.</span></span> <span data-ttu-id="1888e-135">Du kan lämna alla andra inställningar som är.</span><span class="sxs-lookup"><span data-stu-id="1888e-135">You can leave all other settings as is.</span></span>

    <span data-ttu-id="1888e-136">Kontrollera att den **starta den nya kanalen nu** är markerad.</span><span class="sxs-lookup"><span data-stu-id="1888e-136">Make sure the **Start the new channel now** is selected.</span></span>

3. <span data-ttu-id="1888e-137">Klicka på **skapa kanal**.</span><span class="sxs-lookup"><span data-stu-id="1888e-137">Click **Create Channel**.</span></span>

   ![Grundämne](./media/media-services-elemental-live-encoder/media-services-elemental12.png)

> [!NOTE]
> <span data-ttu-id="1888e-139">Kanalen kan ta upp till 20 minuter för att starta.</span><span class="sxs-lookup"><span data-stu-id="1888e-139">The channel can take as long as 20 minutes to start.</span></span>
>
>

<span data-ttu-id="1888e-140">När kanalen startar kan du [konfigurera kodaren](media-services-configure-elemental-live-encoder.md#configure_elemental_rtp).</span><span class="sxs-lookup"><span data-stu-id="1888e-140">While the channel is starting you can [configure the encoder](media-services-configure-elemental-live-encoder.md#configure_elemental_rtp).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1888e-141">Observera att faktureringen påbörjas så snart kanal blir klar att användas.</span><span class="sxs-lookup"><span data-stu-id="1888e-141">Note that billing starts as soon as Channel goes into a ready state.</span></span> <span data-ttu-id="1888e-142">Mer information finns i [kanalens tillstånd](media-services-manage-live-encoder-enabled-channels.md#states).</span><span class="sxs-lookup"><span data-stu-id="1888e-142">For more information, see [Channel's states](media-services-manage-live-encoder-enabled-channels.md#states).</span></span>
>
>

### <span data-ttu-id="1888e-143"><a id=configure_elemental_rtp></a>Konfigurera grundämne Live-kodaren</span><span class="sxs-lookup"><span data-stu-id="1888e-143"><a id=configure_elemental_rtp></a>Configure the Elemental Live encoder</span></span>
<span data-ttu-id="1888e-144">I den här kursen används följande inställningar för utdata.</span><span class="sxs-lookup"><span data-stu-id="1888e-144">In this tutorial the following output settings are used.</span></span> <span data-ttu-id="1888e-145">Resten av det här avsnittet beskriver konfigurationssteg i detalj.</span><span class="sxs-lookup"><span data-stu-id="1888e-145">The rest of this section describes configuration steps in more detail.</span></span>

<span data-ttu-id="1888e-146">**Video**:</span><span class="sxs-lookup"><span data-stu-id="1888e-146">**Video**:</span></span>

* <span data-ttu-id="1888e-147">Codec: H.264</span><span class="sxs-lookup"><span data-stu-id="1888e-147">Codec: H.264</span></span>
* <span data-ttu-id="1888e-148">Profil: Hög (nivå 4.0)</span><span class="sxs-lookup"><span data-stu-id="1888e-148">Profile: High (Level 4.0)</span></span>
* <span data-ttu-id="1888e-149">Bithastighet: 5000 kbit/s</span><span class="sxs-lookup"><span data-stu-id="1888e-149">Bitrate: 5000 kbps</span></span>
* <span data-ttu-id="1888e-150">Keyframe: 2 sekunder (60 sekunder)</span><span class="sxs-lookup"><span data-stu-id="1888e-150">Keyframe: 2 seconds (60 seconds)</span></span>
* <span data-ttu-id="1888e-151">RAM-hastighet: 30</span><span class="sxs-lookup"><span data-stu-id="1888e-151">Frame Rate: 30</span></span>

<span data-ttu-id="1888e-152">**Ljud**:</span><span class="sxs-lookup"><span data-stu-id="1888e-152">**Audio**:</span></span>

* <span data-ttu-id="1888e-153">Codec: AAC (LC)</span><span class="sxs-lookup"><span data-stu-id="1888e-153">Codec: AAC (LC)</span></span>
* <span data-ttu-id="1888e-154">Bithastighet: 192 kbit/s</span><span class="sxs-lookup"><span data-stu-id="1888e-154">Bitrate: 192 kbps</span></span>
* <span data-ttu-id="1888e-155">Samplingsfrekvens: 44.1 kHz</span><span class="sxs-lookup"><span data-stu-id="1888e-155">Sample Rate: 44.1 kHz</span></span>

#### <a name="configuration-steps"></a><span data-ttu-id="1888e-156">Konfigurationssteg</span><span class="sxs-lookup"><span data-stu-id="1888e-156">Configuration steps</span></span>
1. <span data-ttu-id="1888e-157">Navigera till den **grundämne Live** webbgränssnittet och ställa in kodaren för **UDP/TS** strömning.</span><span class="sxs-lookup"><span data-stu-id="1888e-157">Navigate to the **Elemental Live** web interface, and set up the encoder for **UDP/TS** streaming.</span></span>
2. <span data-ttu-id="1888e-158">När du har skapat en ny händelse rulla utdata-grupper och lägga till den **UDP/TS** utdata grupp.</span><span class="sxs-lookup"><span data-stu-id="1888e-158">Once a new event is created, scroll down to the output groups and add the **UDP/TS** output group.</span></span>
3. <span data-ttu-id="1888e-159">Skapa en ny utdata genom att välja **ny ström** och sedan klicka på **lägga till utdata**.</span><span class="sxs-lookup"><span data-stu-id="1888e-159">Create a new output by selecting **New Stream** and then clicking **Add Output**.</span></span>  

    ![Grundämne](./media/media-services-elemental-live-encoder/media-services-elemental13.png)

   > [!NOTE]
   > <span data-ttu-id="1888e-161">Vi rekommenderar att händelsen grundämne har Tidskoden angetts till ”systemklockan” för att kodaren återansluta vid misslyckanden dataströmmen.</span><span class="sxs-lookup"><span data-stu-id="1888e-161">It is recommended that the Elemental event has the timecode set to "System Clock" to help the encoder reconnect in the case of a stream failure.</span></span>
   >
   >
4. <span data-ttu-id="1888e-162">Nu när utdata har skapats, klicka på **lägga till ett flöde**.</span><span class="sxs-lookup"><span data-stu-id="1888e-162">Now that the Output has been created, click **Add Stream**.</span></span> <span data-ttu-id="1888e-163">Inställningar för utdata kan nu konfigureras.</span><span class="sxs-lookup"><span data-stu-id="1888e-163">The output settings can now be configured.</span></span>
5. <span data-ttu-id="1888e-164">Rulla ”dataströmmen 1” just har skapat, klicka på den **Video** fliken till vänster och expandera den **Avancerat** inställningar.</span><span class="sxs-lookup"><span data-stu-id="1888e-164">Scroll down to the "Stream 1" that was just created, click the **Video** tab on the left and expand the **Advanced** settings section.</span></span>

    ![Grundämne](./media/media-services-elemental-live-encoder/media-services-elemental4.png)

    <span data-ttu-id="1888e-166">Medan grundämne Live har ett stort antal tillgängliga anpassa, rekommenderas följande inställningar för att komma igång med strömning till AMS.</span><span class="sxs-lookup"><span data-stu-id="1888e-166">While Elemental Live has a wide range of available customizing, the following settings are recommended for getting started with streaming to AMS.</span></span>

   * <span data-ttu-id="1888e-167">Lösning: minst 1 280 x 720</span><span class="sxs-lookup"><span data-stu-id="1888e-167">Resolution: 1280 x 720</span></span>
   * <span data-ttu-id="1888e-168">Ramhastighet: 30</span><span class="sxs-lookup"><span data-stu-id="1888e-168">Framerate: 30</span></span>
   * <span data-ttu-id="1888e-169">GOP storlek: 60 ramar</span><span class="sxs-lookup"><span data-stu-id="1888e-169">GOP Size: 60 frames</span></span>
   * <span data-ttu-id="1888e-170">Sammanfläta läge: progressiv</span><span class="sxs-lookup"><span data-stu-id="1888e-170">Interlace Mode: Progressive</span></span>
   * <span data-ttu-id="1888e-171">Bithastighet: 5000000 bit/s (Detta kan ställas in baserat på begränsningar)</span><span class="sxs-lookup"><span data-stu-id="1888e-171">Bitrate: 5000000 bit/s (This can be adjusted based on network limitations)</span></span>

    ![Grundämne](./media/media-services-elemental-live-encoder/media-services-elemental5.png)

1. <span data-ttu-id="1888e-173">Hämta kanalens indata-URL.</span><span class="sxs-lookup"><span data-stu-id="1888e-173">Get the channel's input URL.</span></span>

    <span data-ttu-id="1888e-174">Gå tillbaka till verktyget AMSE och kontrollera status för slutförande kanal.</span><span class="sxs-lookup"><span data-stu-id="1888e-174">Navigate back to the AMSE tool, and check on the channel completion status.</span></span> <span data-ttu-id="1888e-175">När läget har ändrats från **Start** till **kör**, du kan hämta indata-URL.</span><span class="sxs-lookup"><span data-stu-id="1888e-175">Once the State has changed from **Starting** to **Running**, you can get the input URL.</span></span>

    <span data-ttu-id="1888e-176">När kanalen körs, högerklickar du på dess namn, navigera till hovra över **kopiera indata-URL till Urklipp** och välj sedan **primära indata-URL**.</span><span class="sxs-lookup"><span data-stu-id="1888e-176">When the channel is running, right click the channel name, navigate down to hover over **Copy Input URL to clipboard** and then select **Primary Input  URL**.</span></span>  

    ![Grundämne](./media/media-services-elemental-live-encoder/media-services-elemental6.png)
2. <span data-ttu-id="1888e-178">Klistra in informationen i den **primära mål** i Elemental.</span><span class="sxs-lookup"><span data-stu-id="1888e-178">Paste this information in the **Primary Destination** field of the Elemental.</span></span> <span data-ttu-id="1888e-179">Alla andra inställningar kan förbli standardvärdet.</span><span class="sxs-lookup"><span data-stu-id="1888e-179">All other settings can remain the default.</span></span>

    ![Grundämne](./media/media-services-elemental-live-encoder/media-services-elemental14.png)

    <span data-ttu-id="1888e-181">Upprepa dessa steg med sekundära indata-URL genom att skapa en separat flik ”utdata” för direktuppspelning av UDP/TS för extra redundans.</span><span class="sxs-lookup"><span data-stu-id="1888e-181">For extra redundancy, repeat these steps with the Secondary Input URL by creating a separate "Output" tab for UDP/TS Streaming.</span></span>
3. <span data-ttu-id="1888e-182">Klicka på **skapa** (om en ny händelse skapades) eller **uppdatering** (om du redigerar en befintlig händelse) och fortsätt sedan att starta kodaren.</span><span class="sxs-lookup"><span data-stu-id="1888e-182">Click **Create** (if a new event was created) or **Update** (if editing a pre-existing event) and then proceed to start the encoder.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1888e-183">Innan du klickar på **starta** på webbgränssnitt grundämne Live du **måste** se till att kanalen är klar.</span><span class="sxs-lookup"><span data-stu-id="1888e-183">Before you click **Start** on the Elemental Live web interface, you **must** ensure that the Channel is ready.</span></span>
> <span data-ttu-id="1888e-184">Kontrollera också att kanalen med tillståndet klart utan en händelse för > 15 minuter.</span><span class="sxs-lookup"><span data-stu-id="1888e-184">Also, make sure not to leave the Channel in a ready state without an event for longer than > 15 minutes.</span></span>
>
>

<span data-ttu-id="1888e-185">När dataströmmen har körts i 30 sekunder, gå tillbaka till AMSE-verktyget och testa uppspelning.</span><span class="sxs-lookup"><span data-stu-id="1888e-185">After the stream has been running for 30 seconds, navigate back to the AMSE tool and test playback.</span></span>  

### <a name="test-playback"></a><span data-ttu-id="1888e-186">Testa uppspelning</span><span class="sxs-lookup"><span data-stu-id="1888e-186">Test playback</span></span>

<span data-ttu-id="1888e-187">Navigera till verktyget AMSE och högerklicka på kanalen som ska testas.</span><span class="sxs-lookup"><span data-stu-id="1888e-187">Navigate to the AMSE tool, and right click the channel to be tested.</span></span> <span data-ttu-id="1888e-188">Från menyn hovra över **uppspelning förhandsgranskningen** och välj **med Azure Media Player**.</span><span class="sxs-lookup"><span data-stu-id="1888e-188">From the menu, hover over **Playback the Preview** and select **with Azure Media Player**.</span></span>  

    ![Elemental](./media/media-services-elemental-live-encoder/media-services-elemental8.png)

<span data-ttu-id="1888e-189">Om strömmen visas i media player, har sedan kodaren konfigurerats korrekt för att ansluta till AMS.</span><span class="sxs-lookup"><span data-stu-id="1888e-189">If the stream appears in the player, then the encoder has been properly configured to connect to AMS.</span></span>

<span data-ttu-id="1888e-190">Om ett fel tas emot kanalen kommer att behöva återställas och anpassa inställningar för kodare.</span><span class="sxs-lookup"><span data-stu-id="1888e-190">If an error is received, the channel will need to be reset and encoder settings adjusted.</span></span> <span data-ttu-id="1888e-191">Finns det [felsökning](media-services-troubleshooting-live-streaming.md) avsnittet som vägledning.</span><span class="sxs-lookup"><span data-stu-id="1888e-191">Please see the [troubleshooting](media-services-troubleshooting-live-streaming.md) topic for guidance.</span></span>   

### <a name="create-a-program"></a><span data-ttu-id="1888e-192">Skapa ett program</span><span class="sxs-lookup"><span data-stu-id="1888e-192">Create a program</span></span>
1. <span data-ttu-id="1888e-193">När kanalen uppspelning bekräftas, skapa ett program.</span><span class="sxs-lookup"><span data-stu-id="1888e-193">Once channel playback is confirmed, create a program.</span></span> <span data-ttu-id="1888e-194">Under den **Live** i verktyget AMSE, högerklicka i området för programmet och välj **Skapa nytt Program**.</span><span class="sxs-lookup"><span data-stu-id="1888e-194">Under the **Live** tab in the AMSE tool, right click within the program area and select **Create New Program**.</span></span>  

    ![Grundämne](./media/media-services-elemental-live-encoder/media-services-elemental9.png)
2. <span data-ttu-id="1888e-196">Namnge programmet och, om det behövs, justera det **längd** (som standard 4 timmar).</span><span class="sxs-lookup"><span data-stu-id="1888e-196">Name the program and, if needed, adjust the **Archive Window Length** (which defaults to 4 hours).</span></span> <span data-ttu-id="1888e-197">Du kan också ange en lagringsplats eller lämna som standard.</span><span class="sxs-lookup"><span data-stu-id="1888e-197">You can also specify a storage location or leave as the default.</span></span>  
3. <span data-ttu-id="1888e-198">Kontrollera den **starta programmet nu** rutan.</span><span class="sxs-lookup"><span data-stu-id="1888e-198">Check the **Start the Program now** box.</span></span>
4. <span data-ttu-id="1888e-199">Klicka på **skapa Program**.</span><span class="sxs-lookup"><span data-stu-id="1888e-199">Click **Create Program**.</span></span>  

    >[!NOTE]
    > <span data-ttu-id="1888e-200">Skapa en program tar mindre tid än att skapa en kanal.</span><span class="sxs-lookup"><span data-stu-id="1888e-200">Program creation takes less time than channel creation.</span></span>   
      
5. <span data-ttu-id="1888e-201">När programmet körs kan bekräfta uppspelning genom att högerklicka på programmet och gå till **uppspelning av program** och sedan välja **med Azure Media Player**.</span><span class="sxs-lookup"><span data-stu-id="1888e-201">Once the program is running, confirm playback by right clicking the program and navigating to **Playback the program(s)** and then selecting **with Azure Media Player**.</span></span>  
6. <span data-ttu-id="1888e-202">När bekräftat, högerklicka på programmet och välj **kopiera den URL som utdata till Urklipp** (eller hämta den här informationen från den **programmet information och inställningar** alternativ på menyn).</span><span class="sxs-lookup"><span data-stu-id="1888e-202">Once confirmed, right click the program again and select **Copy the Output URL to Clipboard** (or retrieve this information from the **Program information and settings** option from the menu).</span></span>

<span data-ttu-id="1888e-203">Dataströmmen är nu redo att vara inbäddat i ett player eller distribuerats till en målgrupp för live visning.</span><span class="sxs-lookup"><span data-stu-id="1888e-203">The stream is now ready to be embedded in a player, or distributed to an audience for live viewing.</span></span>  

## <a name="troubleshooting"></a><span data-ttu-id="1888e-204">Felsökning</span><span class="sxs-lookup"><span data-stu-id="1888e-204">Troubleshooting</span></span>
<span data-ttu-id="1888e-205">Finns det [felsökning](media-services-troubleshooting-live-streaming.md) avsnittet som vägledning.</span><span class="sxs-lookup"><span data-stu-id="1888e-205">Please see the [troubleshooting](media-services-troubleshooting-live-streaming.md) topic for guidance.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="1888e-206">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="1888e-206">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="1888e-207">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="1888e-207">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
