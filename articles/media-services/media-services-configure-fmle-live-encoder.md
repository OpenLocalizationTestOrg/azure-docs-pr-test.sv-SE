---
title: "Konfigurera FMLE kodaren om du vill skicka en direktsänd dataström med enkel bithastighet | Microsoft Docs"
description: "Det här avsnittet visar hur du konfigurerar kodaren Flash Media Live kodare (FMLE) för att skicka en dataström med enkel bithastighet till AMS-kanaler som är aktiverade för live encoding."
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
ms.openlocfilehash: e831048f34ecf6e89595adc4bfd58b5977e04bdb
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="use-the-fmle-encoder-to-send-a-single-bitrate-live-stream"></a><span data-ttu-id="e3de1-103">Använda kodaren FMLE för att skicka en direktsänd dataström med enkel bithastighet</span><span class="sxs-lookup"><span data-stu-id="e3de1-103">Use the FMLE encoder to send a single bitrate live stream</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e3de1-104">FMLE</span><span class="sxs-lookup"><span data-stu-id="e3de1-104">FMLE</span></span>](media-services-configure-fmle-live-encoder.md)
> * [<span data-ttu-id="e3de1-105">Elemental Live</span><span class="sxs-lookup"><span data-stu-id="e3de1-105">Elemental Live</span></span>](media-services-configure-elemental-live-encoder.md)
> * [<span data-ttu-id="e3de1-106">Tricaster</span><span class="sxs-lookup"><span data-stu-id="e3de1-106">Tricaster</span></span>](media-services-configure-tricaster-live-encoder.md)
> * [<span data-ttu-id="e3de1-107">Wirecast</span><span class="sxs-lookup"><span data-stu-id="e3de1-107">Wirecast</span></span>](media-services-configure-wirecast-live-encoder.md)
>
>

<span data-ttu-id="e3de1-108">Det här avsnittet visar hur du konfigurerar den [Flash Live mediekodare](http://www.adobe.com/products/flash-media-encoder.html) kodare (FMLE) att skicka en dataström med enkel bithastighet till AMS kanaler som är aktiverade för live encoding.</span><span class="sxs-lookup"><span data-stu-id="e3de1-108">This topic shows how to configure the [Flash Media Live Encoder](http://www.adobe.com/products/flash-media-encoder.html) (FMLE) encoder to send a single bitrate stream to AMS channels that are enabled for live encoding.</span></span> <span data-ttu-id="e3de1-109">Mer information finns i [Arbeta med kanaler som är aktiverade för att utföra Live Encoding med Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span><span class="sxs-lookup"><span data-stu-id="e3de1-109">For more information, see [Working with Channels that are Enabled to Perform Live Encoding with Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).</span></span>

<span data-ttu-id="e3de1-110">Den här kursen visar hur du hanterar Azure Media Services (AMS) med Azure Media Services Explorer (AMSE)-verktyget.</span><span class="sxs-lookup"><span data-stu-id="e3de1-110">This tutorial shows how to manage Azure Media Services (AMS) with Azure Media Services Explorer (AMSE) tool.</span></span> <span data-ttu-id="e3de1-111">Det här verktyget körs bara på Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="e3de1-111">This tool only runs on Windows PC.</span></span> <span data-ttu-id="e3de1-112">Om du är på Mac- eller Linux använder Azure-portalen för att skapa [kanaler](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) och [program](media-services-portal-creating-live-encoder-enabled-channel.md).</span><span class="sxs-lookup"><span data-stu-id="e3de1-112">If you are on Mac or Linux, use the Azure portal to create [channels](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) and [programs](media-services-portal-creating-live-encoder-enabled-channel.md).</span></span>

<span data-ttu-id="e3de1-113">Observera att den här självstudiekursen beskriver hur du använder AAC.</span><span class="sxs-lookup"><span data-stu-id="e3de1-113">Note that this tutorial describes using AAC.</span></span> <span data-ttu-id="e3de1-114">FMLE stöder inte AAC som standard.</span><span class="sxs-lookup"><span data-stu-id="e3de1-114">However, FMLE doesn’t supports AAC by default.</span></span> <span data-ttu-id="e3de1-115">Du behöver köpa ett plugin-program för AAC kodning till exempel från MainConcept: [AAC plugin-program](http://www.mainconcept.com/products/plug-ins/plug-ins-for-adobe/aac-encoder-fmle.html)</span><span class="sxs-lookup"><span data-stu-id="e3de1-115">You would need to purchase a plugin for AAC encoding such as from MainConcept: [AAC plugin](http://www.mainconcept.com/products/plug-ins/plug-ins-for-adobe/aac-encoder-fmle.html)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e3de1-116">Krav</span><span class="sxs-lookup"><span data-stu-id="e3de1-116">Prerequisites</span></span>
* [<span data-ttu-id="e3de1-117">Skapa ett Azure Media Services-konto</span><span class="sxs-lookup"><span data-stu-id="e3de1-117">Create an Azure Media Services account</span></span>](media-services-portal-create-account.md)
* <span data-ttu-id="e3de1-118">Se till att det finns en Strömningsslutpunkt som körs.</span><span class="sxs-lookup"><span data-stu-id="e3de1-118">Ensure there is a Streaming Endpoint running.</span></span> <span data-ttu-id="e3de1-119">Mer information finns i [hanterar Strömningsslutpunkter i ett Media Services-konto](media-services-portal-manage-streaming-endpoints.md)</span><span class="sxs-lookup"><span data-stu-id="e3de1-119">For more information, see [Manage Streaming Endpoints in a Media Services Account](media-services-portal-manage-streaming-endpoints.md)</span></span>
* <span data-ttu-id="e3de1-120">Installera den senaste versionen av den [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) verktyget.</span><span class="sxs-lookup"><span data-stu-id="e3de1-120">Install the latest version of the [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) tool.</span></span>
* <span data-ttu-id="e3de1-121">Starta verktyget och Anslut till AMS-kontot.</span><span class="sxs-lookup"><span data-stu-id="e3de1-121">Launch the tool and connect to your AMS account.</span></span>

## <a name="tips"></a><span data-ttu-id="e3de1-122">Tips</span><span class="sxs-lookup"><span data-stu-id="e3de1-122">Tips</span></span>
* <span data-ttu-id="e3de1-123">När det är möjligt använda ett inbyggt internet-anslutning.</span><span class="sxs-lookup"><span data-stu-id="e3de1-123">Whenever possible, use a hardwired internet connection.</span></span>
* <span data-ttu-id="e3de1-124">En bra tumregel samband med fastställandet av krav på bandbredd är att dubbla strömmande bithastighet.</span><span class="sxs-lookup"><span data-stu-id="e3de1-124">A good rule of thumb when determining bandwidth requirements is to double the streaming bitrates.</span></span> <span data-ttu-id="e3de1-125">Även om detta inte är en nödvändighet hjälper minimera effekten av överbelastning.</span><span class="sxs-lookup"><span data-stu-id="e3de1-125">While this is not a mandatory requirement, it will help mitigate the impact of network congestion.</span></span>
* <span data-ttu-id="e3de1-126">När du använder programvara baserat kodare, Stäng alla onödiga program.</span><span class="sxs-lookup"><span data-stu-id="e3de1-126">When using software based encoders, close out any unnecessary programs.</span></span>

## <a name="create-a-channel"></a><span data-ttu-id="e3de1-127">Skapa en kanal</span><span class="sxs-lookup"><span data-stu-id="e3de1-127">Create a channel</span></span>
1. <span data-ttu-id="e3de1-128">I verktyget AMSE navigerar du till den **Live** fliken och högerklicka i området för kanal.</span><span class="sxs-lookup"><span data-stu-id="e3de1-128">In the AMSE tool, navigate to the **Live** tab, and right click within the channel area.</span></span> <span data-ttu-id="e3de1-129">Välj **skapa kanal...**</span><span class="sxs-lookup"><span data-stu-id="e3de1-129">Select **Create channel…**</span></span> <span data-ttu-id="e3de1-130">från menyn.</span><span class="sxs-lookup"><span data-stu-id="e3de1-130">from the menu.</span></span>

    ![FMLE](./media/media-services-fmle-live-encoder/media-services-fmle1.png)

2. <span data-ttu-id="e3de1-132">Ange ett Kanalnamn beskrivningsfältet är valfritt.</span><span class="sxs-lookup"><span data-stu-id="e3de1-132">Specify a channel name, the description field is optional.</span></span> <span data-ttu-id="e3de1-133">Markera under inställningar för kanal **Standard** för alternativet Live Encoding med protokollet indata som angetts till **RTMP**.</span><span class="sxs-lookup"><span data-stu-id="e3de1-133">Under Channel Settings, select **Standard** for the Live Encoding option, with the Input Protocol set to **RTMP**.</span></span> <span data-ttu-id="e3de1-134">Du kan lämna alla andra inställningar som är.</span><span class="sxs-lookup"><span data-stu-id="e3de1-134">You can leave all other settings as is.</span></span>

    <span data-ttu-id="e3de1-135">Kontrollera att den **starta den nya kanalen nu** är markerad.</span><span class="sxs-lookup"><span data-stu-id="e3de1-135">Make sure the **Start the new channel now** is selected.</span></span>

3. <span data-ttu-id="e3de1-136">Klicka på **skapa kanal**.</span><span class="sxs-lookup"><span data-stu-id="e3de1-136">Click **Create Channel**.</span></span>

   ![FMLE](./media/media-services-fmle-live-encoder/media-services-fmle2.png)

> [!NOTE]
> <span data-ttu-id="e3de1-138">Kanalen kan ta upp till 20 minuter för att starta.</span><span class="sxs-lookup"><span data-stu-id="e3de1-138">The channel can take as long as 20 minutes to start.</span></span>
>
>

<span data-ttu-id="e3de1-139">När kanalen startar kan du [konfigurera kodaren](media-services-configure-fmle-live-encoder.md#configure_fmle_rtmp).</span><span class="sxs-lookup"><span data-stu-id="e3de1-139">While the channel is starting you can [configure the encoder](media-services-configure-fmle-live-encoder.md#configure_fmle_rtmp).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e3de1-140">Observera att faktureringen påbörjas så snart kanal blir klar att användas.</span><span class="sxs-lookup"><span data-stu-id="e3de1-140">Note that billing starts as soon as Channel goes into a ready state.</span></span> <span data-ttu-id="e3de1-141">Mer information finns i [kanalens tillstånd](media-services-manage-live-encoder-enabled-channels.md#states).</span><span class="sxs-lookup"><span data-stu-id="e3de1-141">For more information, see [Channel's states](media-services-manage-live-encoder-enabled-channels.md#states).</span></span>
>
>

## <span data-ttu-id="e3de1-142"><a id=configure_fmle_rtmp></a>Konfigurera FMLE kodaren</span><span class="sxs-lookup"><span data-stu-id="e3de1-142"><a id=configure_fmle_rtmp></a>Configure the FMLE encoder</span></span>
<span data-ttu-id="e3de1-143">I den här kursen används följande inställningar för utdata.</span><span class="sxs-lookup"><span data-stu-id="e3de1-143">In this tutorial the following output settings are used.</span></span> <span data-ttu-id="e3de1-144">Resten av det här avsnittet beskriver konfigurationssteg i detalj.</span><span class="sxs-lookup"><span data-stu-id="e3de1-144">The rest of this section describes configuration steps in more detail.</span></span>

<span data-ttu-id="e3de1-145">**Video**:</span><span class="sxs-lookup"><span data-stu-id="e3de1-145">**Video**:</span></span>

* <span data-ttu-id="e3de1-146">Codec: H.264</span><span class="sxs-lookup"><span data-stu-id="e3de1-146">Codec: H.264</span></span>
* <span data-ttu-id="e3de1-147">Profil: Hög (nivå 4.0)</span><span class="sxs-lookup"><span data-stu-id="e3de1-147">Profile: High (Level 4.0)</span></span>
* <span data-ttu-id="e3de1-148">Bithastighet: 5000 kbit/s</span><span class="sxs-lookup"><span data-stu-id="e3de1-148">Bitrate: 5000 kbps</span></span>
* <span data-ttu-id="e3de1-149">Keyframe: 2 sekunder (60 sekunder)</span><span class="sxs-lookup"><span data-stu-id="e3de1-149">Keyframe: 2 seconds (60 seconds)</span></span>
* <span data-ttu-id="e3de1-150">RAM-hastighet: 30</span><span class="sxs-lookup"><span data-stu-id="e3de1-150">Frame Rate: 30</span></span>

<span data-ttu-id="e3de1-151">**Ljud**:</span><span class="sxs-lookup"><span data-stu-id="e3de1-151">**Audio**:</span></span>

* <span data-ttu-id="e3de1-152">Codec: AAC (LC)</span><span class="sxs-lookup"><span data-stu-id="e3de1-152">Codec: AAC (LC)</span></span>
* <span data-ttu-id="e3de1-153">Bithastighet: 192 kbit/s</span><span class="sxs-lookup"><span data-stu-id="e3de1-153">Bitrate: 192 kbps</span></span>
* <span data-ttu-id="e3de1-154">Samplingsfrekvens: 44.1 kHz</span><span class="sxs-lookup"><span data-stu-id="e3de1-154">Sample Rate: 44.1 kHz</span></span>

### <a name="configuration-steps"></a><span data-ttu-id="e3de1-155">Konfigurationssteg</span><span class="sxs-lookup"><span data-stu-id="e3de1-155">Configuration steps</span></span>
1. <span data-ttu-id="e3de1-156">Navigera till den Flash Media Live kodarens (FMLE) gränssnitt på den dator som används.</span><span class="sxs-lookup"><span data-stu-id="e3de1-156">Navigate to the Flash Media Live Encoder’s (FMLE) interface on the machine being used.</span></span>

    <span data-ttu-id="e3de1-157">Gränssnittet är en huvudsidan för inställningar.</span><span class="sxs-lookup"><span data-stu-id="e3de1-157">The interface is one main page of settings.</span></span> <span data-ttu-id="e3de1-158">Kom ihåg följande rekommenderade inställningar för att komma igång med strömning med FMLE.</span><span class="sxs-lookup"><span data-stu-id="e3de1-158">Please take note of the following recommended settings to get started with streaming using FMLE.</span></span>

   * <span data-ttu-id="e3de1-159">Format: H.264 bildfrekvens: 30,00</span><span class="sxs-lookup"><span data-stu-id="e3de1-159">Format: H.264 Frame Rate: 30.00</span></span>
   * <span data-ttu-id="e3de1-160">Inkommande storlek: minst 1 280 x 720</span><span class="sxs-lookup"><span data-stu-id="e3de1-160">Input Size: 1280 x 720</span></span>
   * <span data-ttu-id="e3de1-161">Bithastighet: 5000 kbit/s (kan justeras utifrån nätverksbegränsningar)</span><span class="sxs-lookup"><span data-stu-id="e3de1-161">Bit Rate: 5000 Kbps (Can be adjusted based on network limitations)</span></span>  

     ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle3.png)

     <span data-ttu-id="e3de1-163">När du använder sammanflätad källor, du markera alternativet ”Ej sammanflätning”</span><span class="sxs-lookup"><span data-stu-id="e3de1-163">When using interlaced sources, please checkmark the “Deinterlace” option</span></span>
2. <span data-ttu-id="e3de1-164">Välj skiftnyckelikonen bredvid Format, dessa ytterligare inställningar ska vara:</span><span class="sxs-lookup"><span data-stu-id="e3de1-164">Select the wrench icon next to Format, these additional settings should be:</span></span>

   * <span data-ttu-id="e3de1-165">Profil: Main</span><span class="sxs-lookup"><span data-stu-id="e3de1-165">Profile: Main</span></span>
   * <span data-ttu-id="e3de1-166">Nivå: 4.0</span><span class="sxs-lookup"><span data-stu-id="e3de1-166">Level: 4.0</span></span>
   * <span data-ttu-id="e3de1-167">Keyframe frekvens: 2 sekunder</span><span class="sxs-lookup"><span data-stu-id="e3de1-167">Keyframe Frequency: 2 seconds</span></span>

     ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle4.png)
3. <span data-ttu-id="e3de1-169">Ange följande viktiga ljud inställning:</span><span class="sxs-lookup"><span data-stu-id="e3de1-169">Set the following important audio setting:</span></span>

   * <span data-ttu-id="e3de1-170">Format: AAC</span><span class="sxs-lookup"><span data-stu-id="e3de1-170">Format: AAC</span></span>
   * <span data-ttu-id="e3de1-171">Samplingsfrekvens: 44100 Hz</span><span class="sxs-lookup"><span data-stu-id="e3de1-171">Sample Rate: 44100 Hz</span></span>
   * <span data-ttu-id="e3de1-172">Bithastighet: 192 kbit/s</span><span class="sxs-lookup"><span data-stu-id="e3de1-172">Bitrate: 192 Kbps</span></span>

     ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle5.png)
4. <span data-ttu-id="e3de1-174">Hämta kanalen input URL: en för att tilldela FMLE **RTMP Endpoint**.</span><span class="sxs-lookup"><span data-stu-id="e3de1-174">Get the channel's input URL in order to assign it to the FMLE's **RTMP Endpoint**.</span></span>

    <span data-ttu-id="e3de1-175">Gå tillbaka till verktyget AMSE och kontrollera status för slutförande kanal.</span><span class="sxs-lookup"><span data-stu-id="e3de1-175">Navigate back to the AMSE tool, and check on the channel completion status.</span></span> <span data-ttu-id="e3de1-176">När läget har ändrats från **Start** till **kör**, du kan hämta indata-URL.</span><span class="sxs-lookup"><span data-stu-id="e3de1-176">Once the State has changed from **Starting** to **Running**, you can get the input URL.</span></span>

    <span data-ttu-id="e3de1-177">När kanalen körs, högerklickar du på dess namn, navigera till hovra över **kopiera indata-URL till Urklipp** och välj sedan **primära indata-URL**.</span><span class="sxs-lookup"><span data-stu-id="e3de1-177">When the channel is running, right click the channel name, navigate down to hover over **Copy Input URL to clipboard** and then select **Primary Input  URL**.</span></span>  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle6.png)
5. <span data-ttu-id="e3de1-179">Klistra in informationen i den **FMS URL** i utdata och tilldela ett namn på dataströmmen.</span><span class="sxs-lookup"><span data-stu-id="e3de1-179">Paste this information in the **FMS URL** field of the output section, and assign a stream name.</span></span>

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle7.png)

    <span data-ttu-id="e3de1-181">Upprepa dessa steg med sekundära indata-URL för extra redundans.</span><span class="sxs-lookup"><span data-stu-id="e3de1-181">For extra redundancy, repeat these steps with the Secondary Input URL.</span></span>
6. <span data-ttu-id="e3de1-182">Välj **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="e3de1-182">Select **Connect**.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e3de1-183">Innan du klickar på **Anslut**, du **måste** se till att kanalen är klar.</span><span class="sxs-lookup"><span data-stu-id="e3de1-183">Before you click **Connect**, you **must** ensure that the Channel is ready.</span></span>
> <span data-ttu-id="e3de1-184">Kontrollera också att kanalen klar utan ett inkommande bidrag feed för > 15 minuter.</span><span class="sxs-lookup"><span data-stu-id="e3de1-184">Also, make sure not to leave the Channel in a ready state without an input contribution feed for longer than > 15 minutes.</span></span>
>
>

## <a name="test-playback"></a><span data-ttu-id="e3de1-185">Testa uppspelning</span><span class="sxs-lookup"><span data-stu-id="e3de1-185">Test playback</span></span>

<span data-ttu-id="e3de1-186">Navigera till verktyget AMSE och högerklicka på kanalen som ska testas.</span><span class="sxs-lookup"><span data-stu-id="e3de1-186">Navigate to the AMSE tool, and right click the channel to be tested.</span></span> <span data-ttu-id="e3de1-187">Från menyn hovra över **uppspelning förhandsgranskningen** och välj **med Azure Media Player**.</span><span class="sxs-lookup"><span data-stu-id="e3de1-187">From the menu, hover over **Playback the Preview** and select **with Azure Media Player**.</span></span>  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle8.png)

<span data-ttu-id="e3de1-188">Om strömmen visas i media player, har sedan kodaren konfigurerats korrekt för att ansluta till AMS.</span><span class="sxs-lookup"><span data-stu-id="e3de1-188">If the stream appears in the player, then the encoder has been properly configured to connect to AMS.</span></span>

<span data-ttu-id="e3de1-189">Om ett fel tas emot kanalen kommer att behöva återställas och anpassa inställningar för kodare.</span><span class="sxs-lookup"><span data-stu-id="e3de1-189">If an error is received, the channel will need to be reset and encoder settings adjusted.</span></span> <span data-ttu-id="e3de1-190">Finns det [felsökning](media-services-troubleshooting-live-streaming.md) avsnittet som vägledning.</span><span class="sxs-lookup"><span data-stu-id="e3de1-190">Please see the [troubleshooting](media-services-troubleshooting-live-streaming.md) topic for guidance.</span></span>  

## <a name="create-a-program"></a><span data-ttu-id="e3de1-191">Skapa ett program</span><span class="sxs-lookup"><span data-stu-id="e3de1-191">Create a program</span></span>
1. <span data-ttu-id="e3de1-192">När kanalen uppspelning bekräftas, skapa ett program.</span><span class="sxs-lookup"><span data-stu-id="e3de1-192">Once channel playback is confirmed, create a program.</span></span> <span data-ttu-id="e3de1-193">Under den **Live** i verktyget AMSE, högerklicka i området för programmet och välj **Skapa nytt Program**.</span><span class="sxs-lookup"><span data-stu-id="e3de1-193">Under the **Live** tab in the AMSE tool, right click within the program area and select **Create New Program**.</span></span>  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle9.png)
2. <span data-ttu-id="e3de1-195">Namnge programmet och, om det behövs, justera det **längd** (som standard 4 timmar).</span><span class="sxs-lookup"><span data-stu-id="e3de1-195">Name the program and, if needed, adjust the **Archive Window Length** (which defaults to 4 hours).</span></span> <span data-ttu-id="e3de1-196">Du kan också ange en lagringsplats eller lämna som standard.</span><span class="sxs-lookup"><span data-stu-id="e3de1-196">You can also specify a storage location or leave as the default.</span></span>  
3. <span data-ttu-id="e3de1-197">Kontrollera den **starta programmet nu** rutan.</span><span class="sxs-lookup"><span data-stu-id="e3de1-197">Check the **Start the Program now** box.</span></span>
4. <span data-ttu-id="e3de1-198">Klicka på **skapa Program**.</span><span class="sxs-lookup"><span data-stu-id="e3de1-198">Click **Create Program**.</span></span>  

    >[!NOTE]
    ><span data-ttu-id="e3de1-199">Skapa en program tar mindre tid än att skapa en kanal.</span><span class="sxs-lookup"><span data-stu-id="e3de1-199">Program creation takes less time than channel creation.</span></span>
        
5. <span data-ttu-id="e3de1-200">När programmet körs kan bekräfta uppspelning genom att högerklicka på programmet och gå till **uppspelning av program** och sedan välja **med Azure Media Player**.</span><span class="sxs-lookup"><span data-stu-id="e3de1-200">Once the program is running, confirm playback by right clicking the program and navigating to **Playback the program(s)** and then selecting **with Azure Media Player**.</span></span>  
6. <span data-ttu-id="e3de1-201">När bekräftat, högerklicka på programmet och välj **kopiera den URL som utdata till Urklipp** (eller hämta den här informationen från den **programmet information och inställningar** alternativ på menyn).</span><span class="sxs-lookup"><span data-stu-id="e3de1-201">Once confirmed, right click the program again and select **Copy the Output URL to Clipboard** (or retrieve this information from the **Program information and settings** option from the menu).</span></span>

<span data-ttu-id="e3de1-202">Dataströmmen är nu redo att vara inbäddat i ett player eller distribuerats till en målgrupp för live visning.</span><span class="sxs-lookup"><span data-stu-id="e3de1-202">The stream is now ready to be embedded in a player, or distributed to an audience for live viewing.</span></span>  

## <a name="troubleshooting"></a><span data-ttu-id="e3de1-203">Felsökning</span><span class="sxs-lookup"><span data-stu-id="e3de1-203">Troubleshooting</span></span>
<span data-ttu-id="e3de1-204">Finns det [felsökning](media-services-troubleshooting-live-streaming.md) avsnittet som vägledning.</span><span class="sxs-lookup"><span data-stu-id="e3de1-204">Please see the [troubleshooting](media-services-troubleshooting-live-streaming.md) topic for guidance.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="e3de1-205">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="e3de1-205">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="e3de1-206">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="e3de1-206">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
