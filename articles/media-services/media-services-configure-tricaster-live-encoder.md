---
title: "Konfigurera NewTek TriCaster-kodaren om du vill skicka en direktsänd dataström med enkel bithastighet | Microsoft Docs"
description: "Det här avsnittet visar hur du konfigurerar Tricaster livekodaren för att skicka en dataström med enkel bithastighet till AMS-kanaler som är aktiverade för live encoding."
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
ms.openlocfilehash: 42b012fb98bd0504c931ce391d63aecca8c3d311
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="use-the-newtek-tricaster-encoder-to-send-a-single-bitrate-live-stream"></a><span data-ttu-id="7c0ca-103">Använda kodaren NewTek TriCaster för att skicka en direktsänd dataström med enkel bithastighet</span><span class="sxs-lookup"><span data-stu-id="7c0ca-103">Use the NewTek TriCaster encoder to send a single bitrate live stream</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7c0ca-104">Tricaster</span><span class="sxs-lookup"><span data-stu-id="7c0ca-104">Tricaster</span></span>](media-services-configure-tricaster-live-encoder.md)
> * [<span data-ttu-id="7c0ca-105">Elemental Live</span><span class="sxs-lookup"><span data-stu-id="7c0ca-105">Elemental Live</span></span>](media-services-configure-elemental-live-encoder.md)
> * [<span data-ttu-id="7c0ca-106">Wirecast</span><span class="sxs-lookup"><span data-stu-id="7c0ca-106">Wirecast</span></span>](media-services-configure-wirecast-live-encoder.md)
> * [<span data-ttu-id="7c0ca-107">FMLE</span><span class="sxs-lookup"><span data-stu-id="7c0ca-107">FMLE</span></span>](media-services-configure-fmle-live-encoder.md)
>
>

<span data-ttu-id="7c0ca-108">Det här avsnittet visar hur du konfigurerar den [NewTek TriCaster](http://newtek.com/products/tricaster-40.html) livekodare för att skicka en dataström med enkel bithastighet till AMS kanaler som är aktiverade för live encoding.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-108">This topic shows how to configure the [NewTek TriCaster](http://newtek.com/products/tricaster-40.html) live encoder to send a single bitrate stream to AMS channels that are enabled for live encoding.</span></span> <span data-ttu-id="7c0ca-109">Mer information finns i [Arbeta med kanaler som är aktiverade för att utföra Live Encoding med Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span><span class="sxs-lookup"><span data-stu-id="7c0ca-109">For more information, see [Working with Channels that are Enabled to Perform Live Encoding with Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span></span>

<span data-ttu-id="7c0ca-110">Den här kursen visar hur du hanterar Azure Media Services (AMS) med Azure Media Services Explorer (AMSE)-verktyget.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-110">This tutorial shows how to manage Azure Media Services (AMS) with Azure Media Services Explorer (AMSE) tool.</span></span> <span data-ttu-id="7c0ca-111">Det här verktyget körs bara på Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-111">This tool only runs on Windows PC.</span></span> <span data-ttu-id="7c0ca-112">Om du är på Mac- eller Linux använder Azure-portalen för att skapa [kanaler](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) och [program](media-services-portal-creating-live-encoder-enabled-channel.md).</span><span class="sxs-lookup"><span data-stu-id="7c0ca-112">If you are on Mac or Linux, use the Azure portal to create [channels](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) and [programs](media-services-portal-creating-live-encoder-enabled-channel.md).</span></span>

> [!NOTE]
> <span data-ttu-id="7c0ca-113">När du använder Tricaster för att skicka i ett bidrag till AMS-kanaler som är aktiverade för live encoding, kan det finnas problem med ljud och i din direktsända händelse om du använder vissa funktioner i Tricaster, till exempel snabb klippa ut mellan feeds eller växla till/från pekdatorer.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-113">When using Tricaster for sending in a contribution feed to AMS channels that are enabled for live encoding, there can be video/audio glitches in your live event if you use certain features of Tricaster, such as rapid cutting between feeds, or switching to/from slates.</span></span> <span data-ttu-id="7c0ca-114">AMS-teamet arbetar på att åtgärda dessa problem fram till dess, är den rekommenderar inte att använda dessa funktioner.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-114">The AMS team is working on fixing these issues, until then, it is not recommend to use these features.</span></span>
>
>

## <a name="prerequisites"></a><span data-ttu-id="7c0ca-115">Krav</span><span class="sxs-lookup"><span data-stu-id="7c0ca-115">Prerequisites</span></span>
* [<span data-ttu-id="7c0ca-116">Skapa ett Azure Media Services-konto</span><span class="sxs-lookup"><span data-stu-id="7c0ca-116">Create an Azure Media Services account</span></span>](media-services-portal-create-account.md)
* <span data-ttu-id="7c0ca-117">Se till att det finns en Strömningsslutpunkt som körs.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-117">Ensure there is a Streaming Endpoint running.</span></span> <span data-ttu-id="7c0ca-118">Mer information finns i [hanterar Strömningsslutpunkter i ett Media Services-konto](media-services-portal-manage-streaming-endpoints.md)</span><span class="sxs-lookup"><span data-stu-id="7c0ca-118">For more information, see [Manage Streaming Endpoints in a Media Services Account](media-services-portal-manage-streaming-endpoints.md)</span></span>
* <span data-ttu-id="7c0ca-119">Installera den senaste versionen av den [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) verktyget.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-119">Install the latest version of the [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) tool.</span></span>
* <span data-ttu-id="7c0ca-120">Starta verktyget och Anslut till AMS-kontot.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-120">Launch the tool and connect to your AMS account.</span></span>

## <a name="tips"></a><span data-ttu-id="7c0ca-121">Tips</span><span class="sxs-lookup"><span data-stu-id="7c0ca-121">Tips</span></span>
* <span data-ttu-id="7c0ca-122">När det är möjligt använda ett inbyggt internet-anslutning.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-122">Whenever possible, use a hardwired internet connection.</span></span>
* <span data-ttu-id="7c0ca-123">En bra tumregel samband med fastställandet av krav på bandbredd är att dubbla strömmande bithastighet.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-123">A good rule of thumb when determining bandwidth requirements is to double the streaming bitrates.</span></span> <span data-ttu-id="7c0ca-124">Även om detta inte är en nödvändighet hjälper minimera effekten av överbelastning.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-124">While this is not a mandatory requirement, it will help mitigate the impact of network congestion.</span></span>
* <span data-ttu-id="7c0ca-125">När du använder programvara baserat kodare, Stäng alla onödiga program.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-125">When using software based encoders, close out any unnecessary programs.</span></span>

## <a name="create-a-channel"></a><span data-ttu-id="7c0ca-126">Skapa en kanal</span><span class="sxs-lookup"><span data-stu-id="7c0ca-126">Create a channel</span></span>
1. <span data-ttu-id="7c0ca-127">I verktyget AMSE navigerar du till den **Live** fliken och högerklicka i området för kanal.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-127">In the AMSE tool, navigate to the **Live** tab, and right click within the channel area.</span></span> <span data-ttu-id="7c0ca-128">Välj **skapa kanal...**</span><span class="sxs-lookup"><span data-stu-id="7c0ca-128">Select **Create channel…**</span></span> <span data-ttu-id="7c0ca-129">från menyn.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-129">from the menu.</span></span>

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster1.png)

2. <span data-ttu-id="7c0ca-131">Ange ett Kanalnamn beskrivningsfältet är valfritt.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-131">Specify a channel name, the description field is optional.</span></span> <span data-ttu-id="7c0ca-132">Markera under inställningar för kanal **Standard** för alternativet Live Encoding med protokollet indata som angetts till **RTMP**.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-132">Under Channel Settings, select **Standard** for the Live Encoding option, with the Input Protocol set to **RTMP**.</span></span> <span data-ttu-id="7c0ca-133">Du kan lämna alla andra inställningar som är.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-133">You can leave all other settings as is.</span></span>

    <span data-ttu-id="7c0ca-134">Kontrollera att den **starta den nya kanalen nu** är markerad.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-134">Make sure the **Start the new channel now** is selected.</span></span>

3. <span data-ttu-id="7c0ca-135">Klicka på **skapa kanal**.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-135">Click **Create Channel**.</span></span>

   ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster2.png)

> [!NOTE]
> <span data-ttu-id="7c0ca-137">Kanalen kan ta upp till 20 minuter för att starta.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-137">The channel can take as long as 20 minutes to start.</span></span>
>
>

<span data-ttu-id="7c0ca-138">När kanalen startar kan du [konfigurera kodaren](media-services-configure-tricaster-live-encoder.md#configure_tricaster_rtmp).</span><span class="sxs-lookup"><span data-stu-id="7c0ca-138">While the channel is starting you can [configure the encoder](media-services-configure-tricaster-live-encoder.md#configure_tricaster_rtmp).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7c0ca-139">Observera att faktureringen påbörjas så snart kanal blir klar att användas.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-139">Note that billing starts as soon as Channel goes into a ready state.</span></span> <span data-ttu-id="7c0ca-140">Mer information finns i [kanalens tillstånd](media-services-manage-live-encoder-enabled-channels.md#states).</span><span class="sxs-lookup"><span data-stu-id="7c0ca-140">For more information, see [Channel's states](media-services-manage-live-encoder-enabled-channels.md#states).</span></span>
>
>

## <span data-ttu-id="7c0ca-141"><a id=configure_tricaster_rtmp></a>Konfigurera NewTek TriCaster-kodaren</span><span class="sxs-lookup"><span data-stu-id="7c0ca-141"><a id=configure_tricaster_rtmp></a>Configure the NewTek TriCaster encoder</span></span>
<span data-ttu-id="7c0ca-142">I den här kursen används följande inställningar för utdata.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-142">In this tutorial the following output settings are used.</span></span> <span data-ttu-id="7c0ca-143">Resten av det här avsnittet beskriver konfigurationssteg i detalj.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-143">The rest of this section describes configuration steps in more detail.</span></span>

<span data-ttu-id="7c0ca-144">**Video**:</span><span class="sxs-lookup"><span data-stu-id="7c0ca-144">**Video**:</span></span>

* <span data-ttu-id="7c0ca-145">Codec: H.264</span><span class="sxs-lookup"><span data-stu-id="7c0ca-145">Codec: H.264</span></span>
* <span data-ttu-id="7c0ca-146">Profil: Hög (nivå 4.0)</span><span class="sxs-lookup"><span data-stu-id="7c0ca-146">Profile: High (Level 4.0)</span></span>
* <span data-ttu-id="7c0ca-147">Bithastighet: 5000 kbit/s</span><span class="sxs-lookup"><span data-stu-id="7c0ca-147">Bitrate: 5000 kbps</span></span>
* <span data-ttu-id="7c0ca-148">Keyframe: 2 sekunder (60 sekunder)</span><span class="sxs-lookup"><span data-stu-id="7c0ca-148">Keyframe: 2 seconds (60 seconds)</span></span>
* <span data-ttu-id="7c0ca-149">RAM-hastighet: 30</span><span class="sxs-lookup"><span data-stu-id="7c0ca-149">Frame Rate: 30</span></span>

<span data-ttu-id="7c0ca-150">**Ljud**:</span><span class="sxs-lookup"><span data-stu-id="7c0ca-150">**Audio**:</span></span>

* <span data-ttu-id="7c0ca-151">Codec: AAC (LC)</span><span class="sxs-lookup"><span data-stu-id="7c0ca-151">Codec: AAC (LC)</span></span>
* <span data-ttu-id="7c0ca-152">Bithastighet: 192 kbit/s</span><span class="sxs-lookup"><span data-stu-id="7c0ca-152">Bitrate: 192 kbps</span></span>
* <span data-ttu-id="7c0ca-153">Samplingsfrekvens: 44.1 kHz</span><span class="sxs-lookup"><span data-stu-id="7c0ca-153">Sample Rate: 44.1 kHz</span></span>

### <a name="configuration-steps"></a><span data-ttu-id="7c0ca-154">Konfigurationssteg</span><span class="sxs-lookup"><span data-stu-id="7c0ca-154">Configuration steps</span></span>
1. <span data-ttu-id="7c0ca-155">Skapa en ny **NewTek TriCaster** projekt beroende på vilka inkommande videokällan används.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-155">Create a new **NewTek TriCaster** project depending on what video input source is being used.</span></span>
2. <span data-ttu-id="7c0ca-156">En gång i det aktuella projektet att hitta den **dataströmmen** och klickar på ikonen växeln bredvid komma åt menyn direktuppspelning configuration.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-156">Once within that project, find the **Stream** button, and click the gear icon next to it to access the stream configuration menu.</span></span>

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster3.png)
3. <span data-ttu-id="7c0ca-158">När du har öppnat menyn, klickar du på **ny** under rubriken anslutning.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-158">Once the menu has opened, click **New** under the Connection heading.</span></span> <span data-ttu-id="7c0ca-159">När du tillfrågas om vilken typ av anslutning, Välj **Adobe Flash**.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-159">When prompted for the connection type, select **Adobe Flash**.</span></span>

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster4.png)
4. <span data-ttu-id="7c0ca-161">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-161">Click **OK**.</span></span>
5. <span data-ttu-id="7c0ca-162">En profil för FMLE kan nu importeras genom att klicka på listrutepilen under **strömning profil** och navigera till **Bläddra**.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-162">An FMLE profile can now be imported by clicking the drop down arrow under **Streaming Profile** and navigating to **Browse**.</span></span>

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster5.png)
6. <span data-ttu-id="7c0ca-164">Gå till där konfigurerade FMLE profilen sparades.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-164">Navigate to where the configured FMLE profile was saved.</span></span>
7. <span data-ttu-id="7c0ca-165">Markera den och tryck på **OK**.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-165">Select it, and press **OK**.</span></span>

    <span data-ttu-id="7c0ca-166">När profilen har överförts, fortsätter du till nästa steg.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-166">Once the profile is uploaded, proceed to the next step.</span></span>
8. <span data-ttu-id="7c0ca-167">Hämta kanalen input URL: en för att tilldela Tricaster **RTMP Endpoint**.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-167">Get the channel's input URL in order to assign it to the Tricaster **RTMP Endpoint**.</span></span>

    <span data-ttu-id="7c0ca-168">Gå tillbaka till verktyget AMSE och kontrollera status för slutförande kanal.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-168">Navigate back to the AMSE tool, and check on the channel completion status.</span></span> <span data-ttu-id="7c0ca-169">När läget har ändrats från **Start** till **kör**, du kan hämta indata-URL.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-169">Once the State has changed from **Starting** to **Running**, you can get the input URL.</span></span>

    <span data-ttu-id="7c0ca-170">När kanalen körs, högerklickar du på dess namn, navigera till hovra över **kopiera indata-URL till Urklipp** och välj sedan **primära indata-URL**.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-170">When the channel is running, right click the channel name, navigate down to hover over **Copy Input URL to clipboard** and then select **Primary Input  URL**.</span></span>  

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster6.png)
9. <span data-ttu-id="7c0ca-172">Klistra in informationen i den **plats** fältet under **Flash Server** inom Tricaster-projekt.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-172">Paste this information in the **Location** field under **Flash Server** within the Tricaster project.</span></span> <span data-ttu-id="7c0ca-173">Tilldela ett stream-namn i den **dataström-ID** fältet.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-173">Also assign a stream name in the **Stream ID** field.</span></span>

    <span data-ttu-id="7c0ca-174">Om strömmen information har lagts till FMLE profilen också importeras till det här avsnittet genom att klicka på **importinställningarna**, navigera till den sparade FMLE profilen och klickar på **OK**.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-174">If stream information was added to the FMLE profile, it can also be imported to this section by clicking **Import Settings**, navigating to the saved FMLE profile and clicking **OK**.</span></span> <span data-ttu-id="7c0ca-175">Fälten Flash Server bör fyllas i automatiskt med information från FMLE.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-175">The relevant Flash Server fields should populate with the information from FMLE.</span></span>

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster7.png)
10. <span data-ttu-id="7c0ca-177">När du är klar klickar du på **OK** längst ned på skärmen.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-177">When finished, click **OK** at the bottom of the screen.</span></span> <span data-ttu-id="7c0ca-178">När video och ljud indata till Tricaster är klar börjar strömning till AMS genom att klicka på den **dataströmmen** knappen.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-178">When video and audio inputs into the Tricaster are ready, begin streaming to AMS by clicking the **Stream** button.</span></span>

     ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster11.png)

> [!IMPORTANT]
> <span data-ttu-id="7c0ca-180">Innan du klickar på **dataströmmen**, du **måste** se till att kanalen är klar.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-180">Before you click **Stream**, you **must** ensure that the Channel is ready.</span></span>
> <span data-ttu-id="7c0ca-181">Kontrollera också att kanalen klar utan ett inkommande bidrag feed för > 15 minuter.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-181">Also, make sure not to leave the Channel in a ready state without an input contribution feed for longer than > 15 minutes.</span></span>
>
>

## <a name="test-playback"></a><span data-ttu-id="7c0ca-182">Testa uppspelning</span><span class="sxs-lookup"><span data-stu-id="7c0ca-182">Test playback</span></span>
<span data-ttu-id="7c0ca-183">Navigera till verktyget AMSE och högerklicka på kanalen som ska testas.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-183">Navigate to the AMSE tool, and right click the channel to be tested.</span></span> <span data-ttu-id="7c0ca-184">Från menyn hovra över **uppspelning förhandsgranskningen** och välj **med Azure Media Player**.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-184">From the menu, hover over **Playback the Preview** and select **with Azure Media Player**.</span></span>  

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster8.png)

<span data-ttu-id="7c0ca-185">Om strömmen visas i media player, har sedan kodaren konfigurerats korrekt för att ansluta till AMS.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-185">If the stream appears in the player, then the encoder has been properly configured to connect to AMS.</span></span>

<span data-ttu-id="7c0ca-186">Om ett fel tas emot kanalen kommer att behöva återställas och anpassa inställningar för kodare.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-186">If an error is received, the channel will need to be reset and encoder settings adjusted.</span></span> <span data-ttu-id="7c0ca-187">Finns det [felsökning](media-services-troubleshooting-live-streaming.md) avsnittet som vägledning.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-187">Please see the [troubleshooting](media-services-troubleshooting-live-streaming.md) topic for guidance.</span></span>  

## <a name="create-a-program"></a><span data-ttu-id="7c0ca-188">Skapa ett program</span><span class="sxs-lookup"><span data-stu-id="7c0ca-188">Create a program</span></span>
1. <span data-ttu-id="7c0ca-189">När kanalen uppspelning bekräftas, skapa ett program.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-189">Once channel playback is confirmed, create a program.</span></span> <span data-ttu-id="7c0ca-190">Under den **Live** i verktyget AMSE, högerklicka i området för programmet och välj **Skapa nytt Program**.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-190">Under the **Live** tab in the AMSE tool, right click within the program area and select **Create New Program**.</span></span>  

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster9.png)
2. <span data-ttu-id="7c0ca-192">Namnge programmet och, om det behövs, justera det **längd** (som standard 4 timmar).</span><span class="sxs-lookup"><span data-stu-id="7c0ca-192">Name the program and, if needed, adjust the **Archive Window Length** (which defaults to 4 hours).</span></span> <span data-ttu-id="7c0ca-193">Du kan också ange en lagringsplats eller lämna som standard.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-193">You can also specify a storage location or leave as the default.</span></span>  
3. <span data-ttu-id="7c0ca-194">Kontrollera den **starta programmet nu** rutan.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-194">Check the **Start the Program now** box.</span></span>
4. <span data-ttu-id="7c0ca-195">Klicka på **skapa Program**.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-195">Click **Create Program**.</span></span>  

    >[!NOTE]
    ><span data-ttu-id="7c0ca-196">Skapa en program tar mindre tid än att skapa en kanal.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-196">Program creation takes less time than channel creation.</span></span>
        
5. <span data-ttu-id="7c0ca-197">När programmet körs kan bekräfta uppspelning genom att högerklicka på programmet och gå till **uppspelning av program** och sedan välja **med Azure Media Player**.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-197">Once the program is running, confirm playback by right clicking the program and navigating to **Playback the program(s)** and then selecting **with Azure Media Player**.</span></span>  
6. <span data-ttu-id="7c0ca-198">När bekräftat, högerklicka på programmet och välj **kopiera den URL som utdata till Urklipp** (eller hämta den här informationen från den **programmet information och inställningar** alternativ på menyn).</span><span class="sxs-lookup"><span data-stu-id="7c0ca-198">Once confirmed, right click the program again and select **Copy the Output URL to Clipboard** (or retrieve this information from the **Program information and settings** option from the menu).</span></span>

<span data-ttu-id="7c0ca-199">Dataströmmen är nu redo att vara inbäddat i ett player eller distribuerats till en målgrupp för live visning.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-199">The stream is now ready to be embedded in a player, or distributed to an audience for live viewing.</span></span>  

## <a name="troubleshooting"></a><span data-ttu-id="7c0ca-200">Felsökning</span><span class="sxs-lookup"><span data-stu-id="7c0ca-200">Troubleshooting</span></span>
<span data-ttu-id="7c0ca-201">Finns det [felsökning](media-services-troubleshooting-live-streaming.md) avsnittet som vägledning.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-201">Please see the [troubleshooting](media-services-troubleshooting-live-streaming.md) topic for guidance.</span></span>

## <a name="next-step"></a><span data-ttu-id="7c0ca-202">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7c0ca-202">Next step</span></span>
<span data-ttu-id="7c0ca-203">Granska sökvägarna för Media Services-utbildning.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-203">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="7c0ca-204">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="7c0ca-204">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
