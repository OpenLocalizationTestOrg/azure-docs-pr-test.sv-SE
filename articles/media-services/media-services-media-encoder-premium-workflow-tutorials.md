---
title: "aaaAvanced Media Encoder Premium arbetsflödet självstudier"
description: "Det här dokumentet innehåller genomgång som visar hur tooperform avancerade uppgifter med Media Encoder Premium arbetsflöde och även hur toocreate komplexa arbetsflöden med Arbetsflödesdesignern."
services: media-services
documentationcenter: 
author: xstof
manager: cfowler
editor: 
ms.assetid: 1ba52865-b4a8-4ca0-ac96-920d55b9d15b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: christoc;xpouyat;juliako
ms.openlocfilehash: 641bd1be3bd795b5e138fa7ffdf299ff6710904e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-media-encoder-premium-workflow-tutorials"></a><span data-ttu-id="12015-103">Avancerade Media Encoder Premium arbetsflödet självstudier</span><span class="sxs-lookup"><span data-stu-id="12015-103">Advanced Media Encoder Premium Workflow tutorials</span></span>
## <a name="overview"></a><span data-ttu-id="12015-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="12015-104">Overview</span></span>
<span data-ttu-id="12015-105">Det här dokumentet innehåller genomgång som visar hur toocustomize arbetsflöden med **Arbetsflödesdesignern**.</span><span class="sxs-lookup"><span data-stu-id="12015-105">This document contains walkthroughs that show how toocustomize workflows with  **Workflow Designer**.</span></span> <span data-ttu-id="12015-106">Du kan hitta hello faktiska Arbetsflödesfiler [här](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/PremiumEncoderWorkflowSamples).</span><span class="sxs-lookup"><span data-stu-id="12015-106">You can find hello actual workflow files [here](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/PremiumEncoderWorkflowSamples).</span></span>  

## <a name="toc"></a><span data-ttu-id="12015-107">INNEHÅLLSFÖRTECKNING</span><span class="sxs-lookup"><span data-stu-id="12015-107">TOC</span></span>
<span data-ttu-id="12015-108">hello följande avsnitt beskrivs:</span><span class="sxs-lookup"><span data-stu-id="12015-108">hello following topics are covered:</span></span>

* [<span data-ttu-id="12015-109">Kodning MXF till en MP4 med enkel bithastighet</span><span class="sxs-lookup"><span data-stu-id="12015-109">Encoding MXF into a single bitrate MP4</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4)
  * [<span data-ttu-id="12015-110">Starta ett nytt arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="12015-110">Starting a new workflow</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_start_new)
  * [<span data-ttu-id="12015-111">Med hjälp av hello Media filen indata</span><span class="sxs-lookup"><span data-stu-id="12015-111">Using hello Media File Input</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_file_input)
  * [<span data-ttu-id="12015-112">Kontrollera dataströmmar media</span><span class="sxs-lookup"><span data-stu-id="12015-112">Inspecting media streams</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_streams)
  * [<span data-ttu-id="12015-113">Lägger till en videokodare för. Generera filen för MP4</span><span class="sxs-lookup"><span data-stu-id="12015-113">Adding a video encoder for .MP4 file generation</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_file_generation)
  * [<span data-ttu-id="12015-114">Kodning hello ljudström</span><span class="sxs-lookup"><span data-stu-id="12015-114">Encoding hello audio stream</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_audio)
  * [<span data-ttu-id="12015-115">Multiplexering ljud och Video strömmar till en MP4-behållare</span><span class="sxs-lookup"><span data-stu-id="12015-115">Multiplexing Audio and Video streams into an MP4 container</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_audio_and_fideo)
  * [<span data-ttu-id="12015-116">Skrivning hello MP4-fil</span><span class="sxs-lookup"><span data-stu-id="12015-116">Writing hello MP4 file</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_writing_mp4)
  * [<span data-ttu-id="12015-117">Skapa ett Media Services tillgångsinformation från hello utdatafil</span><span class="sxs-lookup"><span data-stu-id="12015-117">Creating a Media Services Asset from hello output file</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_asset_from_output)
  * [<span data-ttu-id="12015-118">Testa hello klar arbetsflödet lokalt</span><span class="sxs-lookup"><span data-stu-id="12015-118">Test hello finished workflow locally</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_test)
* [<span data-ttu-id="12015-119">Kodning MXF till multibitrate MP4s - dynamisk paketering aktiverad</span><span class="sxs-lookup"><span data-stu-id="12015-119">Encoding MXF into multibitrate MP4s - dynamic packaging enabled</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging)
  * [<span data-ttu-id="12015-120">Lägga till en eller flera ytterligare MP4-utdata</span><span class="sxs-lookup"><span data-stu-id="12015-120">Adding one or more additional MP4 outputs</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_more_outputs)
  * [<span data-ttu-id="12015-121">Konfigurera hello utdata-filnamn</span><span class="sxs-lookup"><span data-stu-id="12015-121">Configuring hello file output names</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_conf_output_names)
  * [<span data-ttu-id="12015-122">Lägga till ett separat spår för ljud</span><span class="sxs-lookup"><span data-stu-id="12015-122">Adding a separate Audio Track</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_audio_tracks)
  * [<span data-ttu-id="12015-123">Lägger till hello. ISM SMIL-fil</span><span class="sxs-lookup"><span data-stu-id="12015-123">Adding hello .ISM SMIL File</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_ism_file)
* [<span data-ttu-id="12015-124">Kodning MXF till multibitrate MP4 - förbättrad utkast</span><span class="sxs-lookup"><span data-stu-id="12015-124">Encoding MXF into multibitrate MP4 - enhanced blueprint</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4)
  * [<span data-ttu-id="12015-125">Översikt över tooenhance för arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="12015-125">Workflow overview tooenhance</span></span>](#workflow-overview-to-enhance)
  * [<span data-ttu-id="12015-126">Filen namngivningskonventioner</span><span class="sxs-lookup"><span data-stu-id="12015-126">File Naming Conventions</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_file_naming)
  * [<span data-ttu-id="12015-127">Publicering av egenskaperna för komponenten på hello arbetsflödet rot</span><span class="sxs-lookup"><span data-stu-id="12015-127">Publishing component properties onto hello workflow root</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_publishing)
  * [<span data-ttu-id="12015-128">Har genererat utdatafilen namn är beroende av publicerade egenskapsvärden</span><span class="sxs-lookup"><span data-stu-id="12015-128">Have generated output file names rely on published property values</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_output_files)
* [<span data-ttu-id="12015-129">Lägger till miniatyrbilder toomultibitrate MP4-utdata</span><span class="sxs-lookup"><span data-stu-id="12015-129">Adding thumbnails toomultibitrate MP4 output</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4)
  * [<span data-ttu-id="12015-130">Arbetsflödet översikt tooadd miniatyrer</span><span class="sxs-lookup"><span data-stu-id="12015-130">Workflow overview tooadd thumbnails to</span></span>](#workflow-overview-to-add-thumbnails-to)
  * [<span data-ttu-id="12015-131">Lägga till JPG kodning</span><span class="sxs-lookup"><span data-stu-id="12015-131">Adding JPG Encoding</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4__with_jpg)
  * [<span data-ttu-id="12015-132">Hantera färg utrymme konvertering</span><span class="sxs-lookup"><span data-stu-id="12015-132">Dealing with Color Space conversion</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_color_space)
  * [<span data-ttu-id="12015-133">Skrivning hello miniatyrer</span><span class="sxs-lookup"><span data-stu-id="12015-133">Writing hello thumbnails</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_writing_thumbnails)
  * [<span data-ttu-id="12015-134">Identifiering av fel i ett arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="12015-134">Detecting errors in a workflow</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_errors)
  * [<span data-ttu-id="12015-135">Klar arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="12015-135">Finished Workflow</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_finish)
* [<span data-ttu-id="12015-136">Tidsbaserade trimning multibitrate MP4-utdata</span><span class="sxs-lookup"><span data-stu-id="12015-136">Time-based trimming of multibitrate MP4 output</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim)
  * [<span data-ttu-id="12015-137">Arbetsflödet översikt toostart lägger till trimning till</span><span class="sxs-lookup"><span data-stu-id="12015-137">Workflow overview toostart adding trimming to</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_start)
  * [<span data-ttu-id="12015-138">Med hjälp av hello dataströmmen Trimmer</span><span class="sxs-lookup"><span data-stu-id="12015-138">Using hello Stream Trimmer</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_use_stream_trimmer)
  * [<span data-ttu-id="12015-139">Klar arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="12015-139">Finished Workflow</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_finish)
* [<span data-ttu-id="12015-140">Introduktion till hello skripta komponent</span><span class="sxs-lookup"><span data-stu-id="12015-140">Introducing hello Scripted Component</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#scripting)
  * [<span data-ttu-id="12015-141">Skript i ett arbetsflöde: hello world</span><span class="sxs-lookup"><span data-stu-id="12015-141">Scripting within a workflow: hello world</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#scripting_hello_world)
* [<span data-ttu-id="12015-142">RAM-baserade trimning multibitrate MP4-utdata</span><span class="sxs-lookup"><span data-stu-id="12015-142">Frame-based trimming of multibitrate MP4 output</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim)
  * [<span data-ttu-id="12015-143">Utkast översikt toostart lägger till trimning till</span><span class="sxs-lookup"><span data-stu-id="12015-143">Blueprint overview toostart adding trimming to</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_start)
  * [<span data-ttu-id="12015-144">Med hjälp av hello Clip listan XML</span><span class="sxs-lookup"><span data-stu-id="12015-144">Using hello Clip List XML</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_clip_list)
  * [<span data-ttu-id="12015-145">När du ändrar hello clip listan från en skript-komponent</span><span class="sxs-lookup"><span data-stu-id="12015-145">Modifying hello clip list from a Scripted Component</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_modify_clip_list)
  * [<span data-ttu-id="12015-146">Lägga till en ClippingEnabled bekvämlighet egenskap</span><span class="sxs-lookup"><span data-stu-id="12015-146">Adding a ClippingEnabled convenience property</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_clippingenabled_prop)

## <span data-ttu-id="12015-147"><a id="MXF_to_MP4"></a>Kodning MXF till en MP4 med enkel bithastighet</span><span class="sxs-lookup"><span data-stu-id="12015-147"><a id="MXF_to_MP4"></a>Encoding MXF into a single bitrate MP4</span></span>
<span data-ttu-id="12015-148">I den här genomgången ska vi skapa en enkel bithastighet. MP4-fil med AAC HAN kodat ljud från en. MXF indatafilen.</span><span class="sxs-lookup"><span data-stu-id="12015-148">In this walkthrough we'll create a single bitrate .MP4 file with AAC-HE encoded audio from an .MXF input file.</span></span>

### <span data-ttu-id="12015-149"><a id="MXF_to_MP4_start_new"></a>Starta ett nytt arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="12015-149"><a id="MXF_to_MP4_start_new"></a>Starting a new workflow</span></span>
<span data-ttu-id="12015-150">Öppna Workflow Designer och väljer ”fil”-”ny arbetsyta”-”Omkoda modell”</span><span class="sxs-lookup"><span data-stu-id="12015-150">Open Workflow Designer and select "File"-"New Workspace"-"Transcode Blueprint"</span></span>

<span data-ttu-id="12015-151">arbetsflöde för ny hello visar 3 element:</span><span class="sxs-lookup"><span data-stu-id="12015-151">hello new workflow will show 3 elements:</span></span>

* <span data-ttu-id="12015-152">Primär källfilen</span><span class="sxs-lookup"><span data-stu-id="12015-152">Primary Source File</span></span>
* <span data-ttu-id="12015-153">Beskär List-XML</span><span class="sxs-lookup"><span data-stu-id="12015-153">Clip List XML</span></span>
* <span data-ttu-id="12015-154">Filen/Utdatatillgången</span><span class="sxs-lookup"><span data-stu-id="12015-154">Output File/Asset</span></span>  

![Arbetsflöde för ny kodning](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-transcode-blueprint.png)

<span data-ttu-id="12015-156">*Arbetsflöde för ny kodning*</span><span class="sxs-lookup"><span data-stu-id="12015-156">*New Encoding Workflow*</span></span>

### <span data-ttu-id="12015-157"><a id="MXF_to_MP4_with_file_input"></a>Med hjälp av hello Media filen indata</span><span class="sxs-lookup"><span data-stu-id="12015-157"><a id="MXF_to_MP4_with_file_input"></a>Using hello Media File Input</span></span>
<span data-ttu-id="12015-158">I ordning tooaccept våra inkommande mediefilen startar med att lägga till en Media filen inkommande komponent.</span><span class="sxs-lookup"><span data-stu-id="12015-158">In order tooaccept our input media file, one starts with adding a Media File Input component.</span></span> <span data-ttu-id="12015-159">tooadd ett komponenten toohello arbetsflöde, leta efter den i sökrutan för hello databasen och dra hello önskad post till hello designern rutan.</span><span class="sxs-lookup"><span data-stu-id="12015-159">tooadd a component toohello workflow, look for it in hello Repository search box and drag hello desired entry onto hello designer pane.</span></span> <span data-ttu-id="12015-160">Gör detta för hello Media filen inkommande och ansluta hello primära källfilen komponenten toohello Filename inkommande PIN-kod från hello Media filen indata.</span><span class="sxs-lookup"><span data-stu-id="12015-160">Do this for hello Media File Input and connect hello Primary Source File component toohello Filename input pin from hello Media File Input.</span></span>

![Anslutna mediafilen indata](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-file-input.png)

<span data-ttu-id="12015-162">*Anslutna mediafilen indata*</span><span class="sxs-lookup"><span data-stu-id="12015-162">*Connected Media File Input*</span></span>

<span data-ttu-id="12015-163">Innan vi kan göra mycket annan först behöver vi tooindicate toohello Arbetsflödesdesignern vilka exempelfilen vill vi toouse toodesign våra arbetsflöde med.</span><span class="sxs-lookup"><span data-stu-id="12015-163">Before we can do much else, we'll first need tooindicate toohello workflow designer what sample file we'd like toouse toodesign our workflow with.</span></span> <span data-ttu-id="12015-164">toodo så klicka på hello designern rutan Bakgrund och leta efter hello primära källfilen egenskapen i hello egenskapen högra fönstret.</span><span class="sxs-lookup"><span data-stu-id="12015-164">toodo so, click hello designer pane background and look for hello Primary Source File property on hello right-hand property pane.</span></span> <span data-ttu-id="12015-165">Klicka på hello mappikon och välj hello önskad filen tootest hello arbetsflöde med.</span><span class="sxs-lookup"><span data-stu-id="12015-165">Click hello folder icon and select hello desired file tootest hello workflow with.</span></span> <span data-ttu-id="12015-166">När det är klart hello Media filen indata komponenten inspektera hello-filen och fylla i sina PIN-koder tooreflect hello utdatafilen det kontrolleras.</span><span class="sxs-lookup"><span data-stu-id="12015-166">As soon as this is done, hello Media File Input component will inspect hello file and populate its output pins tooreflect hello file it inspected.</span></span>

![Ifyllda mediafilen indata](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-populated-media-file-input.png)

<span data-ttu-id="12015-168">*Ifyllda mediafilen indata*</span><span class="sxs-lookup"><span data-stu-id="12015-168">*Populated Media File Input*</span></span>

<span data-ttu-id="12015-169">När detta anger med vad indata vill vi toowork med den tala inte ännu där hello kodade utdata ska gå att.</span><span class="sxs-lookup"><span data-stu-id="12015-169">While this specifies with what input we'd like toowork with, it doesn't tell yet where hello encoded output should go to.</span></span> <span data-ttu-id="12015-170">Liknande toohow hello primära källfilen har konfigurerats kan du nu konfigurera hello utdata mappen variabeln egenskapen under den.</span><span class="sxs-lookup"><span data-stu-id="12015-170">Similar toohow hello Primary Source File was configured, now configure hello Output Folder Variable property, just below it.</span></span>

![Konfigurerade ingående och utgående egenskaper](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-configured-io-properties.png)

<span data-ttu-id="12015-172">*Konfigurerade ingående och utgående egenskaper*</span><span class="sxs-lookup"><span data-stu-id="12015-172">*Configured Input and Output properties*</span></span>

### <span data-ttu-id="12015-173"><a id="MXF_to_MP4_streams"></a>Kontrollera dataströmmar media</span><span class="sxs-lookup"><span data-stu-id="12015-173"><a id="MXF_to_MP4_streams"></a>Inspecting media streams</span></span>
<span data-ttu-id="12015-174">Ofta är den önskade tooknow hur hello dataströmmen som de som ser ut som flödar genom hello arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="12015-174">Often it's desired tooknow how hello stream looks like that flows through hello workflow.</span></span> <span data-ttu-id="12015-175">tooinspect en dataström när som helst i arbetsflöde hello, bara på en utgående eller inkommande PIN-kod på någon av hello komponenter.</span><span class="sxs-lookup"><span data-stu-id="12015-175">tooinspect a stream at any point in hello workflow, just click an output or input pin on any of hello components.</span></span> <span data-ttu-id="12015-176">I det här fallet ska du använda kommandot på hello okomprimerade Video utdata PIN-kod från våra Media filen indata.</span><span class="sxs-lookup"><span data-stu-id="12015-176">In this case, try clicking on hello Uncompressed Video output pin from our Media File Input.</span></span> <span data-ttu-id="12015-177">En dialogruta öppnas som tillåter tooinspect hello utgående video.</span><span class="sxs-lookup"><span data-stu-id="12015-177">A dialog will open up that allows tooinspect hello outbound video.</span></span>

![Kontrollera hello utdata okomprimerade Video PIN-kod](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-inspecting-uncompressed-video-output.png)

<span data-ttu-id="12015-179">*Kontrollera hello utdata okomprimerade Video PIN-kod*</span><span class="sxs-lookup"><span data-stu-id="12015-179">*Inspecting hello Uncompressed Video output pin*</span></span>

<span data-ttu-id="12015-180">I vårt fall meddelar oss till exempel att vi hantera en 1 920 x 1 080 inmatning med 24 bildrutor per sekund i 4:2:2 provtagning en video om nästan 2 minuter.</span><span class="sxs-lookup"><span data-stu-id="12015-180">In our case, it tells us for example that we're dealing with a 1920x1080 input at 24 frames per second in 4:2:2 sampling for a video of almost 2 minutes.</span></span>

### <span data-ttu-id="12015-181"><a id="MXF_to_MP4_file_generation"></a>Lägger till en videokodare för. Generera filen för MP4</span><span class="sxs-lookup"><span data-stu-id="12015-181"><a id="MXF_to_MP4_file_generation"></a>Adding a video encoder for .MP4 file generation</span></span>
<span data-ttu-id="12015-182">Observera att nu, en okomprimerad Video och flera utdata PIN-koder för okomprimerade ljud är tillgängliga för användning på vår Media filen indata.</span><span class="sxs-lookup"><span data-stu-id="12015-182">Note that now, an Uncompressed Video and multiple Uncompressed Audio output pins are available for use on our Media File Input.</span></span> <span data-ttu-id="12015-183">I ordning tooencode hello inkommande video måste vi komponenten kodning – i det här fallet för att generera. MP4-filer.</span><span class="sxs-lookup"><span data-stu-id="12015-183">In order tooencode hello inbound video, we need an encoding component - in this case for generating .MP4 files.</span></span>

<span data-ttu-id="12015-184">tooencode hello video-ström tooH.264, Lägg till hello AVC-videokodare komponenten toohello designytan.</span><span class="sxs-lookup"><span data-stu-id="12015-184">tooencode hello video stream tooH.264, add hello AVC Video Encoder component toohello designer surface.</span></span> <span data-ttu-id="12015-185">Den här komponenten tar en video Expandera-dataström som indata och levererar en AVC komprimerade video ström på dess utdata PIN-kod.</span><span class="sxs-lookup"><span data-stu-id="12015-185">This component takes an uncompress video stream as input and delivers an AVC compressed video stream on its output pin.</span></span>

![Ansluten AVC-kodaren](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-unconnected-avc-encoder.png)

<span data-ttu-id="12015-187">*Ansluten AVC-kodaren*</span><span class="sxs-lookup"><span data-stu-id="12015-187">*Unconnected AVC Encoder*</span></span>

<span data-ttu-id="12015-188">Egenskaperna avgör hur hello kodning exakt händer.</span><span class="sxs-lookup"><span data-stu-id="12015-188">Its properties determine how hello encoding exactly happens.</span></span> <span data-ttu-id="12015-189">Låt oss ta en titt på några av hello viktigare inställningar:</span><span class="sxs-lookup"><span data-stu-id="12015-189">Let's have a look at some of hello more important settings:</span></span>

* <span data-ttu-id="12015-190">Utdata bredd och höjd för utdata: dessa avgör hello lösning av hello kodad video.</span><span class="sxs-lookup"><span data-stu-id="12015-190">Output width and Output height: these determine hello resolution of hello encoded video.</span></span> <span data-ttu-id="12015-191">I vårt fall vi väljer 640 x 360</span><span class="sxs-lookup"><span data-stu-id="12015-191">In our case let's go with 640x360</span></span>
* <span data-ttu-id="12015-192">Bildfrekvens: när set toopassthrough visas bara antar hello källa bildfrekvens, är det möjligt toooverride detta dock.</span><span class="sxs-lookup"><span data-stu-id="12015-192">Frame Rate: when set toopassthrough it will just adopt hello source frame rate, it's possible toooverride this though.</span></span> <span data-ttu-id="12015-193">Observera att sådana ramhastighet konvertering inte rörelse-ersätts.</span><span class="sxs-lookup"><span data-stu-id="12015-193">Note that such framerate conversion is not motion-compensated.</span></span>
* <span data-ttu-id="12015-194">Profil och nivå: dessa avgör hello AVC-profil och nivå.</span><span class="sxs-lookup"><span data-stu-id="12015-194">Profile and Level: these determine hello AVC profile and level.</span></span> <span data-ttu-id="12015-195">tooconveniently få mer information om hello olika nivåer och profiler, klickar du på hello frågetecken på hello AVC Video Encoder komponent och hello hjälpsidan ska visa mer information om varje hello nivåer.</span><span class="sxs-lookup"><span data-stu-id="12015-195">tooconveniently get more information about hello different levels and profiles, click hello question mark icon on hello AVC Video Encoder component and hello help page will show more detail about each of hello levels.</span></span> <span data-ttu-id="12015-196">För exemplet lägger vi väljer Main profil på nivå 3.2 (hello standard).</span><span class="sxs-lookup"><span data-stu-id="12015-196">For our sample, let's go with Main Profile at level 3.2 (hello default).</span></span>
* <span data-ttu-id="12015-197">Betygsätt läge och bithastighet (kbps): i vårt scenario vi välja en konstant bithastighet (CBR) som utdata med 1200 kbit/s</span><span class="sxs-lookup"><span data-stu-id="12015-197">Rate Control Mode and Bitrate (kbps): in our scenario we opt for a constant bitrate (CBR) output at 1200 kbps</span></span>
* <span data-ttu-id="12015-198">Video Format: Detta är om hello VUI (Video användbarhet Information) som skrivs till hello H.264 dataström (sida information som kan användas av en avkodarens tooenhance hello visning men är inte nödvändigt toocorrectly avkoda):</span><span class="sxs-lookup"><span data-stu-id="12015-198">Video Format: this is about hello VUI (Video Usability Information) that gets written into hello H.264 stream (side information that might be used by a decoder tooenhance hello display but not essential toocorrectly decode):</span></span>
* <span data-ttu-id="12015-199">NTSC (typisk för USA eller Japan med 30 fps)</span><span class="sxs-lookup"><span data-stu-id="12015-199">NTSC (typical for US or Japan, using 30 fps)</span></span>
* <span data-ttu-id="12015-200">PAL (typisk för Europa, med 25 bildrutor per sekund)</span><span class="sxs-lookup"><span data-stu-id="12015-200">PAL (typical for Europe, using 25 fps)</span></span>
* <span data-ttu-id="12015-201">GOP storleksläget: vi konfigurerar fast GOP storlek för våra ändamål med ett intervall med nyckel 2 sekunder med stängd GOPs.</span><span class="sxs-lookup"><span data-stu-id="12015-201">GOP Size Mode: we'll configure Fixed GOP Size for our purposes with a Key Interval of 2 seconds with Closed GOPs.</span></span> <span data-ttu-id="12015-202">Detta garanterar kompatibilitet med hello dynamisk paketering Azure Media Services tillhandahåller.</span><span class="sxs-lookup"><span data-stu-id="12015-202">This ensures compatibility with hello dynamic packaging Azure Media Services provides.</span></span>

<span data-ttu-id="12015-203">toofeed våra AVC-kodaren ansluta hello okomprimerade Video utdata PIN-kod från hello Media filen inkommande komponenten toohello okomprimerade Video inkommande PIN-kod från hello AVC-kodaren.</span><span class="sxs-lookup"><span data-stu-id="12015-203">toofeed our AVC encoder, connect hello Uncompressed Video output pin from hello Media File Input component toohello Uncompressed Video input pin from hello AVC encoder.</span></span>

![Anslutna AVC-kodaren](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-avc-encoder.png)

<span data-ttu-id="12015-205">*Anslutna AVC Main-kodaren*</span><span class="sxs-lookup"><span data-stu-id="12015-205">*Connected AVC Main encoder*</span></span>

### <span data-ttu-id="12015-206"><a id="MXF_to_MP4_audio"></a>Kodning hello ljudström</span><span class="sxs-lookup"><span data-stu-id="12015-206"><a id="MXF_to_MP4_audio"></a>Encoding hello audio stream</span></span>
<span data-ttu-id="12015-207">Nu har vi kodad video men hello ursprungliga okomprimerade ljudström måste fortfarande toobe komprimeras.</span><span class="sxs-lookup"><span data-stu-id="12015-207">At this point, we have encoded video but hello original uncompressed audio stream still needs toobe compressed.</span></span> <span data-ttu-id="12015-208">För den här ska vi gå med AAC kodning av hello AAC-kodare (Dolby) komponent.</span><span class="sxs-lookup"><span data-stu-id="12015-208">For this we'll go with AAC encoding by hello AAC Encoder (Dolby) component.</span></span> <span data-ttu-id="12015-209">Lägga till den toohello arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="12015-209">Add it toohello workflow.</span></span>

![Ansluten AVC-kodaren](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-unconnected-aac-encoder.png)

<span data-ttu-id="12015-211">*Ansluten AAC kodaren*</span><span class="sxs-lookup"><span data-stu-id="12015-211">*Unconnected AAC encoder*</span></span>

<span data-ttu-id="12015-212">Nu finns det en inkompatibilitet: det finns bara en enda okomprimerade ljud inkommande PIN-kod från hello AAC kodare när fler än sannolikt hello Media filen indata har två olika okomprimerade ljudström som är tillgängliga: en för hello vänster ljud kanal och en för hello höger.</span><span class="sxs-lookup"><span data-stu-id="12015-212">Now there's an incompatibility: there's only a single uncompressed audio input pin from hello AAC Encoder while more than likely hello Media File Input will have two different uncompressed audio stream available: one for hello left audio channel and one for hello right.</span></span> <span data-ttu-id="12015-213">(Om du hantera surroundljud, som är 6 kanaler). Så det inte går ansluta toodirectly hello ljud från hello Media filen Indatakällan till hello AAC ljud kodare.</span><span class="sxs-lookup"><span data-stu-id="12015-213">(If you're dealing with surround sound, that's 6 channels.) So it's not possible toodirectly connect hello audio from hello Media File Input source into hello AAC audio encoder.</span></span> <span data-ttu-id="12015-214">hello AAC komponenten förväntar sig en så kallad ”överlagrad” ljudström: en enda ström som har både hello vänster och hello rätt kanaler överlagrat med varandra.</span><span class="sxs-lookup"><span data-stu-id="12015-214">hello AAC component expects a so-called "interleaved" audio stream: a single stream that has both hello left and hello right channels interleaved with each other.</span></span> <span data-ttu-id="12015-215">När vi vet från våra media källfil tilldelas vilka ljud spår på vilken position i hello källan vi generera sådana överlagrad ljudström med hello korrekt talare positioner för vänster och höger.</span><span class="sxs-lookup"><span data-stu-id="12015-215">Once we know from our source media file which audio tracks are on what position in hello source, we can generate such interleaved audio stream with hello correctly assigned speaker positions for left and right.</span></span>

<span data-ttu-id="12015-216">Först ska en toogenerated en överlagrad dataström från hello krävs källa ljud kanaler.</span><span class="sxs-lookup"><span data-stu-id="12015-216">First one will want toogenerated an interleaved stream from hello required source audio channels.</span></span> <span data-ttu-id="12015-217">hello ljud dataströmmen Interleaver komponenten hantera det för oss.</span><span class="sxs-lookup"><span data-stu-id="12015-217">hello Audio Stream Interleaver component will handle this for us.</span></span> <span data-ttu-id="12015-218">Lägg till den toohello arbetsflödet och ansluta hello ljud utdata från hello Media filen indata till den.</span><span class="sxs-lookup"><span data-stu-id="12015-218">Add it toohello workflow and connect hello audio outputs from hello Media File Input into it.</span></span>

![Ansluten ljudström Interleaver](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-audio-stream-interleaver.png)

<span data-ttu-id="12015-220">*Ansluten ljudström Interleaver*</span><span class="sxs-lookup"><span data-stu-id="12015-220">*Connected Audio Stream Interleaver*</span></span>

<span data-ttu-id="12015-221">Nu när vi har en överlagrad ljudström vi fortfarande inte har angett där tooassign hello åt vänster eller höger talare positioner åt.</span><span class="sxs-lookup"><span data-stu-id="12015-221">Now that we have an interleaved audio stream, we still didn't specify where tooassign hello left or right speaker positions to.</span></span> <span data-ttu-id="12015-222">I ordning toospecify detta, kan vi utnyttja hello talare Position du.</span><span class="sxs-lookup"><span data-stu-id="12015-222">In order toospecify this, we can leverage hello Speaker Position Assigner.</span></span>

![Lägga till en talare Position du](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-adding-speaker-position-assigner.png)

<span data-ttu-id="12015-224">*Lägga till en talare Position du*</span><span class="sxs-lookup"><span data-stu-id="12015-224">*Adding a Speaker Position Assigner*</span></span>

<span data-ttu-id="12015-225">Konfigurera hello talare Position du för användning med en stereo Indataströmmen via en kodare förinställda Filter för ”anpassad” och hello kanal förinställningen kallas ”2.0 (L, R)”.</span><span class="sxs-lookup"><span data-stu-id="12015-225">Configure hello Speaker Position Assigner for use with a stereo input stream through an Encoder Preset Filter of "Custom" and hello Channel Preset called "2.0 (L,R)".</span></span> <span data-ttu-id="12015-226">(Detta tilldelar hello vänstra högtalaren position toochannel 1 och hello rätt talare position toochannel 2.)</span><span class="sxs-lookup"><span data-stu-id="12015-226">(This will assign hello left speaker position toochannel 1 and hello right speaker position toochannel 2.)</span></span>

<span data-ttu-id="12015-227">Ansluta hello utdata från hello talare Position du toohello indata för hello AAC kodare.</span><span class="sxs-lookup"><span data-stu-id="12015-227">Connect hello output of hello Speaker Position Assigner toohello input of hello AAC Encoder.</span></span> <span data-ttu-id="12015-228">Sedan anger du hello AAC kodare toowork med en ”2.0 (L, R)” kanal förinställningen så att den vet toodeal med stereoljud som indata.</span><span class="sxs-lookup"><span data-stu-id="12015-228">Then, tell hello AAC Encoder toowork with a "2.0 (L,R)" Channel Preset, so it knows toodeal with stereo audio as input.</span></span>

### <span data-ttu-id="12015-229"><a id="MXF_to_MP4_audio_and_fideo"></a>Multiplexering ljud och Video strömmar till en MP4-behållare</span><span class="sxs-lookup"><span data-stu-id="12015-229"><a id="MXF_to_MP4_audio_and_fideo"></a>Multiplexing Audio and Video streams into an MP4 container</span></span>
<span data-ttu-id="12015-230">Beroende på vår AVC kodade video-ström och våra AAC kodade ljudström, vi kan samla in båda till en. MP4-behållare.</span><span class="sxs-lookup"><span data-stu-id="12015-230">Given our AVC encoded video stream and our AAC encoded audio stream, we can capture both into an .MP4 container.</span></span> <span data-ttu-id="12015-231">hello kallas blandar olika dataströmmar i en enda ”multiplexering” (eller ”muxing”).</span><span class="sxs-lookup"><span data-stu-id="12015-231">hello process of mixing different streams into a single one is called "multiplexing" (or "muxing").</span></span> <span data-ttu-id="12015-232">I det här fallet vi interfoliering hello ljud- och videoströmmar för hello i en enda konsekvent. MP4-paketet.</span><span class="sxs-lookup"><span data-stu-id="12015-232">In this case we're interleaving hello audio and hello video streams in a single coherent .MP4 package.</span></span> <span data-ttu-id="12015-233">hello-komponent som samordnar för en. MP4-behållaren kallas hello multiplexor ISO MPEG-4.</span><span class="sxs-lookup"><span data-stu-id="12015-233">hello component that coordinates this for an .MP4 container is called hello ISO MPEG-4 Multiplexer.</span></span> <span data-ttu-id="12015-234">Lägg till en toohello designytan och ansluta både hello AVC Video Encoder och hello AAC kodare tooits indata.</span><span class="sxs-lookup"><span data-stu-id="12015-234">Add one toohello designer surface and connect both hello AVC Video Encoder and hello AAC Encoder tooits inputs.</span></span>

![Anslutna MPEG4 multiplexor](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-mpeg4-multiplexer.png)

<span data-ttu-id="12015-236">*Anslutna MPEG4 multiplexor*</span><span class="sxs-lookup"><span data-stu-id="12015-236">*Connected MPEG4 Multiplexer*</span></span>

### <span data-ttu-id="12015-237"><a id="MXF_to_MP4_writing_mp4"></a>Skrivning hello MP4-fil</span><span class="sxs-lookup"><span data-stu-id="12015-237"><a id="MXF_to_MP4_writing_mp4"></a>Writing hello MP4 file</span></span>
<span data-ttu-id="12015-238">När du skriver en utdatafil används hello utdata från komponenten.</span><span class="sxs-lookup"><span data-stu-id="12015-238">When writing an output file, hello File Output component is used.</span></span> <span data-ttu-id="12015-239">Vi kan ansluta den här toohello utdata från hello ISO MPEG-4 multiplexor så att dess utdata skrivs till toodisk.</span><span class="sxs-lookup"><span data-stu-id="12015-239">We can connect this toohello output of hello ISO MPEG-4 Multiplexer so that its output gets written toodisk.</span></span> <span data-ttu-id="12015-240">toodo, ansluta hello behållare (MPEG-4) utdata PIN-kod toohello skrivåtgärder inkommande PIN-koden för hello utdata.</span><span class="sxs-lookup"><span data-stu-id="12015-240">toodo this, connect hello Container (MPEG-4) output pin toohello Write input pin of hello File Output.</span></span>

![Ansluten utdata](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-file-output.png)

<span data-ttu-id="12015-242">*Ansluten utdata*</span><span class="sxs-lookup"><span data-stu-id="12015-242">*Connected File Output*</span></span>

<span data-ttu-id="12015-243">hello filnamn som kommer att användas bestäms av hello egenskapen.</span><span class="sxs-lookup"><span data-stu-id="12015-243">hello filename that will be used is determined by hello File property.</span></span> <span data-ttu-id="12015-244">Egenskapen kan vara hårdkodad tooa angivet värde, troligen en ska tooset det via ett uttryck i stället.</span><span class="sxs-lookup"><span data-stu-id="12015-244">While that property can be hardcoded tooa given value, most likely one will want tooset it through an expression instead.</span></span>

<span data-ttu-id="12015-245">toohave hello arbetsflödet automatiskt avgöra hello utdata filen namnegenskapen från ett uttryck, klickar på hello buton nästa toohello filnamn (nästa toohello mappikon visas).</span><span class="sxs-lookup"><span data-stu-id="12015-245">toohave hello workflow automatically determine hello output File name property from an expression, click hello buton next toohello File name (next toohello folder icon).</span></span> <span data-ttu-id="12015-246">Från hello listrutan och välj sedan ”uttryck”.</span><span class="sxs-lookup"><span data-stu-id="12015-246">From hello drop down menu then select "Expression".</span></span> <span data-ttu-id="12015-247">Appskärmen hello uttrycksredigerare.</span><span class="sxs-lookup"><span data-stu-id="12015-247">This will bring up hello expression editor.</span></span> <span data-ttu-id="12015-248">Radera hello hello Editor först.</span><span class="sxs-lookup"><span data-stu-id="12015-248">Clear hello contents of hello editor first.</span></span>

![Tom uttrycksredigerare](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-empty-expression-editor.png)

<span data-ttu-id="12015-250">*Tom uttrycksredigerare*</span><span class="sxs-lookup"><span data-stu-id="12015-250">*Empty Expression Editor*</span></span>

<span data-ttu-id="12015-251">hello uttrycksredigerare kan tooenter alla litteralvärde blandas med en eller flera variabler.</span><span class="sxs-lookup"><span data-stu-id="12015-251">hello expression editor allows tooenter any literal value, mixed with one or more variables.</span></span> <span data-ttu-id="12015-252">Variabler börja med ett dollartecken.</span><span class="sxs-lookup"><span data-stu-id="12015-252">Variables start with a dollar sign.</span></span> <span data-ttu-id="12015-253">Eftersom du träffar hello $ nyckel, visas hello editor listrutan med ett urval av tillgängliga variabler.</span><span class="sxs-lookup"><span data-stu-id="12015-253">As you hit hello $ key, hello editor will show a dropdown box with a choice of available variables.</span></span> <span data-ttu-id="12015-254">I vårt fall använder vi en kombination av hello output directory och hello grundläggande indatafilen namnet variabel:</span><span class="sxs-lookup"><span data-stu-id="12015-254">In our case we'll use a combination of hello output directory variable and hello base input file name variable:</span></span>

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}.MP4

![Fylls ut uttrycksredigerare](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-expression-editor.png)

<span data-ttu-id="12015-256">*Fylls ut uttrycksredigerare*</span><span class="sxs-lookup"><span data-stu-id="12015-256">*Filled out Expression Editor*</span></span>

> [!NOTE]
> <span data-ttu-id="12015-257">Du måste ange ett värde i hello Uttrycksredigerare, i ordning toosee hittar en utdatafil kodning jobbet i Azure.</span><span class="sxs-lookup"><span data-stu-id="12015-257">In order toosee see an output file of your encoding job in Azure, you must provide a value in hello expression editor.</span></span>
>
>

<span data-ttu-id="12015-258">När du bekräftar hello uttryck genom att träffa ok kommer hello egenskapen fönstret Förhandsgranska toowhat värdet hello filen egenskapen matchar vid denna tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="12015-258">When you confirm hello expression by hitting ok, hello property window will preview toowhat value hello File property resolves at this point in time.</span></span>

![Fil-uttrycket matchar utdata dir](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-file-expression-resolves-output-dir.png)

<span data-ttu-id="12015-260">*Fil-uttrycket matchar utdata dir*</span><span class="sxs-lookup"><span data-stu-id="12015-260">*File Expression resolves output dir*</span></span>

### <span data-ttu-id="12015-261"><a id="MXF_to_MP4_asset_from_output"></a>Skapa ett Media Services tillgångsinformation från hello utdatafil</span><span class="sxs-lookup"><span data-stu-id="12015-261"><a id="MXF_to_MP4_asset_from_output"></a>Creating a Media Services Asset from hello output file</span></span>
<span data-ttu-id="12015-262">När vi har skapat en MP4-utdatafil vi behöver du fortfarande tooindicate att den här filen tillhör toohello utdata tillgången där media services genererar ett resultat av körning av arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="12015-262">While we have written an MP4 output file, we still need tooindicate that this file belongs toohello output asset which media services will generate as a result of executing this workflow.</span></span> <span data-ttu-id="12015-263">toothis syfte hello fil/Utdatatillgången nod på hello arbetsflödet arbetsytan används.</span><span class="sxs-lookup"><span data-stu-id="12015-263">toothis end, hello Output File/Asset node on hello workflow canvas is used.</span></span> <span data-ttu-id="12015-264">Alla inkommande filer till den här noden gör en del av hello Azure Media Services tillgång.</span><span class="sxs-lookup"><span data-stu-id="12015-264">All incoming files into this node will make part of hello resulting Azure Media Services asset.</span></span>

<span data-ttu-id="12015-265">Ansluta hello utdata från komponenten toohello fil/Utdatatillgången komponenten toofinish hello arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="12015-265">Connect hello File Output component toohello Output File/Asset component toofinish hello workflow.</span></span>

![Klar arbetsflöde](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow.png)

<span data-ttu-id="12015-267">*Klar arbetsflöde*</span><span class="sxs-lookup"><span data-stu-id="12015-267">*Finished Workflow*</span></span>

### <span data-ttu-id="12015-268"><a id="MXF_to_MP4_test"></a>Testa hello klar arbetsflödet lokalt</span><span class="sxs-lookup"><span data-stu-id="12015-268"><a id="MXF_to_MP4_test"></a>Test hello finished workflow locally</span></span>
<span data-ttu-id="12015-269">tootest hello lokalt, arbetsflöde nådde hello uppspelningsknappen i hello verktygsfältet hello överst.</span><span class="sxs-lookup"><span data-stu-id="12015-269">tootest hello workflow locally, hit hello play button in hello toolbar at hello top.</span></span> <span data-ttu-id="12015-270">När du är klar hello arbetsflödet körs inspektera hello utdata som genereras i hello konfigurerats utdatamapp.</span><span class="sxs-lookup"><span data-stu-id="12015-270">When hello workflow finished executing, inspect hello output generated in hello configured output folder.</span></span> <span data-ttu-id="12015-271">Du ser hello klar MP4 utdatafiler som kodats från hello MXF inkommande källfil.</span><span class="sxs-lookup"><span data-stu-id="12015-271">You'll see hello finished MP4 output file that was encoded from hello MXF input source file.</span></span>

## <span data-ttu-id="12015-272"><a id="MXF_to_MP4_with_dyn_packaging"></a>Kodning MXF till MP4 - multibitrate dynamisk paketering aktiverad</span><span class="sxs-lookup"><span data-stu-id="12015-272"><a id="MXF_to_MP4_with_dyn_packaging"></a>Encoding MXF into MP4 - multibitrate dynamic packaging enabled</span></span>
<span data-ttu-id="12015-273">I den här genomgången skapar vi en uppsättning flera MP4-filer med AAC kodade ljud från en enda. MXF indatafilen.</span><span class="sxs-lookup"><span data-stu-id="12015-273">In this walkthrough we'll create a set of multiple bitrate MP4 files with AAC encoded audio from a single .MXF input file.</span></span>

<span data-ttu-id="12015-274">När en multibithastighet tillgången utdata önskas för användning i kombination med hello dynamisk paketering funktioner som erbjuds av Azure Media Services hanterar flera GOP-justerad MP4-filer för var och en annan bithastighet och upplösning behöver toobe genereras.</span><span class="sxs-lookup"><span data-stu-id="12015-274">When a multi-bitrate asset output is desired for use in combination with hello Dynamic Packaging features offered by Azure Media Services, multiple GOP-aligned MP4 files of each a different bitrate and resolution will need toobe generated.</span></span> <span data-ttu-id="12015-275">toodo så hello [kodning MXF i en enkel bithastighet MP4](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4) genomgången ger oss en bra utgångspunkt.</span><span class="sxs-lookup"><span data-stu-id="12015-275">toodo so, hello [Encoding MXF into a single bitrate MP4](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4) walkthrough provides us with a good starting point.</span></span>

![Starta arbetsflöde](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-starting-workflow.png)

<span data-ttu-id="12015-277">*Starta arbetsflöde*</span><span class="sxs-lookup"><span data-stu-id="12015-277">*Starting Workflow*</span></span>

### <span data-ttu-id="12015-278"><a id="MXF_to_MP4_with_dyn_packaging_more_outputs"></a>Lägga till en eller flera ytterligare MP4-utdata</span><span class="sxs-lookup"><span data-stu-id="12015-278"><a id="MXF_to_MP4_with_dyn_packaging_more_outputs"></a>Adding one or more additional MP4 outputs</span></span>
<span data-ttu-id="12015-279">Alla MP4-filer i vårt Azure Media Services tillgång stöder en annan bithastighet och upplösning.</span><span class="sxs-lookup"><span data-stu-id="12015-279">Every MP4 file in our resulting Azure Media Services asset will support a different bitrate and resolution.</span></span> <span data-ttu-id="12015-280">Lägg till en eller flera MP4 utgående filer toohello arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="12015-280">Let's add one or more MP4 output files toohello workflow.</span></span>

<span data-ttu-id="12015-281">toomake att vi har alla våra video kodare som skapats med Hej samma inställningar, enklaste tooduplicate hello redan befintliga AVC-videokodare och konfigurera en annan kombination av upplösning och bithastighet (Lägg till en 960 x 540 med 25 bildrutor per sekund på 2,5 Mbit/s).</span><span class="sxs-lookup"><span data-stu-id="12015-281">toomake sure we have all our video encoders created with hello same settings, it's most convenient tooduplicate hello already existing AVC Video Encoder and configure another combination of resolution and bitrate (let's add one of 960 x 540 at 25 frames per second at 2,5 Mbps).</span></span> <span data-ttu-id="12015-282">tooduplicate hello befintliga kodare, kopiera klistra in den på hello designytan.</span><span class="sxs-lookup"><span data-stu-id="12015-282">tooduplicate hello existing encoder, copy paste it on hello designer surface.</span></span>

<span data-ttu-id="12015-283">Ansluta hello okomprimerade Video utdata PIN-kod hello Media filen indata till vår nya AVC-komponent.</span><span class="sxs-lookup"><span data-stu-id="12015-283">Connect hello Uncompressed Video output pin of hello Media File Input into our new AVC component.</span></span>

![Andra AVC-kodaren som är ansluten](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-second-avc-encoder-connected.png)

<span data-ttu-id="12015-285">*Andra AVC-kodaren som är ansluten*</span><span class="sxs-lookup"><span data-stu-id="12015-285">*Second AVC encoder connected*</span></span>

<span data-ttu-id="12015-286">Anpassa nu hello konfiguration för vår nya AVC-kodaren toooutput 960 x 540 med 2,5 Mbit/s.</span><span class="sxs-lookup"><span data-stu-id="12015-286">Now adapt hello configuration for our new AVC encoder toooutput 960x540 at 2,5 Mbps.</span></span> <span data-ttu-id="12015-287">(Använd egenskaper ”utdata bredd”, ”utdata höjd” och ”bithastighet (kbps)” för detta.)</span><span class="sxs-lookup"><span data-stu-id="12015-287">(Use its properties "Output width", "Output height", and "Bitrate (kbps)" for this.)</span></span>

<span data-ttu-id="12015-288">Får vi vill toouse hello tillgång tillsammans med Azure Media Services dynamisk paketering, hello strömmande slutpunkten måste toobe kan genereras från dessa MP4-filer HLS/fragmenterad MP4/DASH fragment som är exakt justerade tooeach andra på ett sätt att hämta en enda smooth kontinuerlig ljud och video experience klienter som byter mellan olika bithastighet.</span><span class="sxs-lookup"><span data-stu-id="12015-288">Given we want toouse hello resulting asset together with Azure Media Services' dynamic packaging, hello streaming endpoint needs toobe capable of generating from these MP4 files HLS/Fragmented MP4/DASH fragments that are exactly aligned tooeach other in a way that clients that are switching between different bitrates get a single smooth continuous video and audio experience.</span></span> <span data-ttu-id="12015-289">toomake som inträffa, behöver vi tooensure att hello GOP (”grupp av bilder”) storlek för båda MP4-filer har angetts i hello-egenskaperna för både AVC-kodare too2 sekunder, vilket gör du genom att:</span><span class="sxs-lookup"><span data-stu-id="12015-289">toomake that happen, we need tooensure that, in hello properties of both AVC encoders hello GOP ("group of pictures") size for both MP4 files is set too2 seconds, which can be done by:</span></span>

* <span data-ttu-id="12015-290">Ange hello GOP storleksläget tooFixed GOP storlek och</span><span class="sxs-lookup"><span data-stu-id="12015-290">setting hello GOP Size Mode tooFixed GOP size and</span></span>
* <span data-ttu-id="12015-291">hello nyckeln ramsintervall tootwo i sekunder.</span><span class="sxs-lookup"><span data-stu-id="12015-291">hello Key Frame Interval tootwo seconds.</span></span>
* <span data-ttu-id="12015-292">Ange hello GOP IDR kontrollen tooClosed GOP tooensure alla GOP Ständiga på sina egna utan beroenden</span><span class="sxs-lookup"><span data-stu-id="12015-292">also set hello GOP IDR Control tooClosed GOP tooensure all GOP's are standing on their own without dependencies</span></span>

<span data-ttu-id="12015-293">toomake våra arbetsflödet praktiskt toounderstand Byt namn på hello första AVC-kodaren för ”AVC Video Encoder 640 x 360 1200 kbit/s” och hello andra AVC-kodaren ”AVC Video Encoder 960 x 540 2500 kbit/s”.</span><span class="sxs-lookup"><span data-stu-id="12015-293">toomake our workflow convenient toounderstand, rename hello first AVC encoder too"AVC Video Encoder 640x360 1200kbps" and hello second AVC encoder "AVC Video Encoder 960x540 2500 kbps".</span></span>

<span data-ttu-id="12015-294">Nu ska du lägga till en andra ISO MPEG-4-multiplexor och en andra fil-utdata.</span><span class="sxs-lookup"><span data-stu-id="12015-294">Now add a second ISO MPEG-4 Multiplexer and a second File Output.</span></span> <span data-ttu-id="12015-295">Ansluta hello multiplexor toohello nya AVC-kodare och kontrollera att dess utdata dirigeras till hello utdata.</span><span class="sxs-lookup"><span data-stu-id="12015-295">Connect hello multiplexer toohello new AVC encoder and make sure its output is directed into hello File Output.</span></span> <span data-ttu-id="12015-296">Även ansluta hello AAC ljud kodare utdata toohello nya multiplexor indata.</span><span class="sxs-lookup"><span data-stu-id="12015-296">Then also connect hello AAC audio encoder output toohello new multiplexer's input.</span></span> <span data-ttu-id="12015-297">hello utdata i sin tur sedan kan anslutna toohello fil/Utdatatillgången nod tooadd den toohello Media Services tillgång som kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="12015-297">hello File Output in turn can then be connected toohello Output File/Asset node tooadd it toohello Media Services Asset that will be created.</span></span>

![Andra Muxer och utdata från anslutna](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-second-muxer-file-output-connected.png)

<span data-ttu-id="12015-299">*Andra Muxer och utdata från anslutna*</span><span class="sxs-lookup"><span data-stu-id="12015-299">*Second Muxer and File Output connected*</span></span>

<span data-ttu-id="12015-300">Konfigurera hello multiplexor segmentet läge tooGOP antal eller varaktighet för kompatibilitet med Azure Media Services dynamisk paketering och ange hello GOPs per segment too1.</span><span class="sxs-lookup"><span data-stu-id="12015-300">For compatibility with Azure Media Services dynamic packaging, configure hello multiplexer's Chunk Mode tooGOP count or duration and set hello GOPs per chunk too1.</span></span> <span data-ttu-id="12015-301">(Detta ska vara hello standard.)</span><span class="sxs-lookup"><span data-stu-id="12015-301">(This should be hello default.)</span></span>

![Muxer segment lägen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-muxer-chunk-modes.png)

<span data-ttu-id="12015-303">*Muxer segment lägen*</span><span class="sxs-lookup"><span data-stu-id="12015-303">*Muxer Chunk Modes*</span></span>

<span data-ttu-id="12015-304">Obs: du kanske vill toorepeat den här processen för alla andra bithastighet och upplösning kombinationer som du vill toohave toohello tillgången utdata har lagts.</span><span class="sxs-lookup"><span data-stu-id="12015-304">Note: you may want toorepeat this process for any other bitrate and resolution combinations you want toohave added toohello asset output.</span></span>

### <span data-ttu-id="12015-305"><a id="MXF_to_MP4_with_dyn_packaging_conf_output_names"></a>Konfigurera hello utdata-filnamn</span><span class="sxs-lookup"><span data-stu-id="12015-305"><a id="MXF_to_MP4_with_dyn_packaging_conf_output_names"></a>Configuring hello file output names</span></span>
<span data-ttu-id="12015-306">Vi har mer än en fil tillagda toohello utdatatillgången.</span><span class="sxs-lookup"><span data-stu-id="12015-306">We have more than one single file added toohello output asset.</span></span> <span data-ttu-id="12015-307">Detta ger en behovet toomake att hello filnamn för varje hello utgående filer skiljer sig från varandra och kanske även tillämpa ett filnamn så att det blir tydligt hello filnamn vad du arbetar med.</span><span class="sxs-lookup"><span data-stu-id="12015-307">This provides a need toomake sure hello filenames for each of hello output files are different from each other and maybe even apply a file-naming convention so it becomes clear from hello file name what you're dealing with.</span></span>

<span data-ttu-id="12015-308">Utdata filnamngivning kan styras via uttryck i hello designer.</span><span class="sxs-lookup"><span data-stu-id="12015-308">File output naming can be controlled through expressions in hello designer.</span></span> <span data-ttu-id="12015-309">Öppna hello-egenskapen för en av hello utdata komponenter och öppna hello uttrycksredigerare för hello egenskapen.</span><span class="sxs-lookup"><span data-stu-id="12015-309">Open hello property pane for one of hello File Output components and open hello expression editor for hello File property.</span></span> <span data-ttu-id="12015-310">Vårt första utdatafilen konfigurerades via hello följande uttryck (se hello kursen för att gå från [MXF tooa enkel bithastighet MP4 utdata](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4)):</span><span class="sxs-lookup"><span data-stu-id="12015-310">Our first output file was configured through hello following expression (see hello tutorial for going from [MXF tooa single bitrate MP4 output](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4)):</span></span>

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}.MP4

<span data-ttu-id="12015-311">Det innebär att vår filename bestäms av två variabler: hello utdata directory toowrite i och hello källfilens grundläggande namn.</span><span class="sxs-lookup"><span data-stu-id="12015-311">This means that our filename is determined by two variables: hello output directory toowrite in and hello source file base name.</span></span> <span data-ttu-id="12015-312">hello f.d. visas som en egenskap på hello arbetsflödet rot och hello senare bestäms av inkommande hello-fil.</span><span class="sxs-lookup"><span data-stu-id="12015-312">hello former is exposed as a property on hello workflow root and hello latter is determined by hello incoming file.</span></span> <span data-ttu-id="12015-313">Observera att hello målkatalogen är det du använder för att testa lokala; den här egenskapen åsidosätts av hello arbetsflödesmotorn när hello arbetsflödet körs av hello molnbaserade media processor i Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="12015-313">Note that hello output directory is what you use for local testing; this property will be overridden by hello workflow engine when hello workflow is executed by hello cloud-based media processor in Azure Media Services.</span></span>
<span data-ttu-id="12015-314">toogive båda våra utdatafiler namngivning av konsekvent utdata, ändra hello filen först namngivning uttryck som:</span><span class="sxs-lookup"><span data-stu-id="12015-314">toogive both our output files a consistent output naming, change hello first file naming expression to:</span></span>

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_640x360_1.MP4

<span data-ttu-id="12015-315">och andra hello till:</span><span class="sxs-lookup"><span data-stu-id="12015-315">and hello second to:</span></span>

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_960x540_2.MP4

<span data-ttu-id="12015-316">Köra en mellanliggande testkörning toomake att båda utdata MP4-filer skapas korrekt.</span><span class="sxs-lookup"><span data-stu-id="12015-316">Execute an intermediate test run toomake sure both MP4 output files are properly generated.</span></span>

### <span data-ttu-id="12015-317"><a id="MXF_to_MP4_with_dyn_packaging_audio_tracks"></a>Lägga till ett separat spår för ljud</span><span class="sxs-lookup"><span data-stu-id="12015-317"><a id="MXF_to_MP4_with_dyn_packaging_audio_tracks"></a>Adding a separate Audio Track</span></span>
<span data-ttu-id="12015-318">Som vi ser senare när vi skapar en .ism-fil toogo med våra MP4-filer för utdata visar kräver vi också en ljuddata MP4-fil som hello ljud spår för våra anpassningsbar strömning.</span><span class="sxs-lookup"><span data-stu-id="12015-318">As we'll see later when we generate an .ism file toogo with our MP4 output files, we will also require a audio-only MP4 file as hello audio track for our adaptive streaming.</span></span> <span data-ttu-id="12015-319">toocreate detta filen, lägga till en ytterligare muxer toohello arbetsflöde (ISO-MPEG-4 multiplexor) och ansluta hello AAC kodare utdata PIN-kod med dess inkommande PIN-kod för spåra 1.</span><span class="sxs-lookup"><span data-stu-id="12015-319">toocreate this file, add an additional muxer toohello workflow (ISO-MPEG-4 Multiplexer) and connect hello AAC encoder's output pin with its input pin for Track 1.</span></span>

![Ljud Muxer som lagts till](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-audio-muxer-added.png)

<span data-ttu-id="12015-321">*Ljud Muxer som lagts till*</span><span class="sxs-lookup"><span data-stu-id="12015-321">*Audio Muxer Added*</span></span>

<span data-ttu-id="12015-322">Skapa en tredje filen komponenten toooutput hello utgående utdataström från hello muxer och konfigurera hello filen namngivning uttryck som:</span><span class="sxs-lookup"><span data-stu-id="12015-322">Create a third File Output component toooutput hello outbound stream from hello muxer and configure hello file naming expression as:</span></span>

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_128kbps_audio.MP4

![Ljud Muxer skapa filen utdata](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-audio-muxer-creating-file-output.png)

<span data-ttu-id="12015-324">*Ljud Muxer skapa filen utdata*</span><span class="sxs-lookup"><span data-stu-id="12015-324">*Audio Muxer creating File Output*</span></span>

### <span data-ttu-id="12015-325"><a id="MXF_to_MP4_with_dyn_packaging_ism_file"></a>Lägger till hello. ISM SMIL-fil</span><span class="sxs-lookup"><span data-stu-id="12015-325"><a id="MXF_to_MP4_with_dyn_packaging_ism_file"></a>Adding hello .ISM SMIL File</span></span>
<span data-ttu-id="12015-326">För hello dynamisk paketering toowork i kombination med både MP4-filer (och hello ljuddata MP4) i vår Media Services tillgången, men vi behöver också en manifestfil (kallas även en ”SMIL”-fil: synkroniserade Multimedia Integration Language).</span><span class="sxs-lookup"><span data-stu-id="12015-326">For hello dynamic packaging toowork in combination with both MP4 files (and hello audio-only MP4) in our Media Services asset, we also need a manifest file (also called a "SMIL" file: Synchronized Multimedia Integration Language).</span></span> <span data-ttu-id="12015-327">Den här filen anger tooAzure Media Services vilka MP4-filer är tillgängliga för dynamisk paketering och vilken av dessa tooconsider för hello ljud strömning.</span><span class="sxs-lookup"><span data-stu-id="12015-327">This file indicates tooAzure Media Services what MP4 files are available for dynamic packaging and which of those tooconsider for hello audio streaming.</span></span> <span data-ttu-id="12015-328">En typisk manifestfil för en uppsättning MP4's med en enda ljudström ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="12015-328">A typical manifest file for a set of MP4's with a single audio stream looks like this:</span></span>

    <?xml version="1.0" encoding="utf-8" standalone="yes"?>
    <smil xmlns="http://www.w3.org/2001/SMIL20/Language">
      <head>
        <meta name="formats" content="mp4" />
      </head>
      <body>
        <switch>
          <video src="H264_1900kbps_AAC_und_ch2_96kbps.mp4" />
          <video src="H264_1300kbps_AAC_und_ch2_96kbps.mp4" />
          <video src="H264_900kbps_AAC_und_ch2_96kbps.mp4" />
          <audio src="AAC_ch2_96kbps.mp4" title="AAC_und_ch2_96kbps" />
        </switch>
      </body>
    </smil>

<span data-ttu-id="12015-329">hello .ism-fil som innehåller inom en switch-instruktion får en referens tooeach av hello enskilda MP4-filer och dessutom toothose också (minst) ljudfil refererar till tooan MP4 som endast innehåller hello ljud.</span><span class="sxs-lookup"><span data-stu-id="12015-329">hello .ism file contains within a switch statement, a reference tooeach of hello individual MP4 video files and in addition toothose also one (or more) audio file references tooan MP4 that only contains hello audio.</span></span>

<span data-ttu-id="12015-330">Genererar hello-manifestfilen för våra uppsättning MP4's kan göras via en komponent som kallas hello ”AMS Manifest Writer”.</span><span class="sxs-lookup"><span data-stu-id="12015-330">Generating hello manifest file for our set of MP4's can be done through a component called hello "AMS Manifest Writer".</span></span> <span data-ttu-id="12015-331">toouse, drar den till hello yta och ansluter hello ”skriva slutförd” utdata PIN-koder från hello tre utdata komponenter toohello AMS Manifest skrivare som indata.</span><span class="sxs-lookup"><span data-stu-id="12015-331">toouse it, drag it onto hello surface and connect hello "Write Complete" output pins from hello three File Output components toohello AMS Manifest Writer input.</span></span> <span data-ttu-id="12015-332">Gör att tooconnect hello utdata från hello AMS Manifest Writer toohello fil/Utdatatillgången.</span><span class="sxs-lookup"><span data-stu-id="12015-332">Then make sure tooconnect hello output of hello AMS Manifest Writer toohello Output File/Asset.</span></span>

<span data-ttu-id="12015-333">Precis som med våra andra filen utdata komponenter, konfigurerar du hello .ism filnamn utdata med ett uttryck:</span><span class="sxs-lookup"><span data-stu-id="12015-333">As with our other file output components, configure hello .ism file output name with an expression:</span></span>

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}_manifest.ism

<span data-ttu-id="12015-334">Vår klar arbetsflöde ser ut så hello nedan:</span><span class="sxs-lookup"><span data-stu-id="12015-334">Our finished workflow looks like hello below:</span></span>

![Klar MXF toomultibitrate MP4-arbetsflöde](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-mxf-to-multibitrate-mp4-workflow.png)

<span data-ttu-id="12015-336">*Klar MXF toomultibitrate MP4-arbetsflöde*</span><span class="sxs-lookup"><span data-stu-id="12015-336">*Finished MXF toomultibitrate MP4 workflow*</span></span>

## <span data-ttu-id="12015-337"><a id="MXF_to__multibitrate_MP4"></a>Kodning MXF till multibitrate MP4 - förbättrad utkast</span><span class="sxs-lookup"><span data-stu-id="12015-337"><a id="MXF_to__multibitrate_MP4"></a>Encoding MXF into multibitrate MP4 - enhanced blueprint</span></span>
<span data-ttu-id="12015-338">I hello [tidigare arbetsflöde genomgången](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging) vi har sett hur en enda MXF inkommande tillgång kan konverteras till en utdatatillgången med flera bithastigheter MP4-filer, en ljuddata MP4-fil och en manifestfil för användning tillsammans med Azure Media Services dynamisk paketering.</span><span class="sxs-lookup"><span data-stu-id="12015-338">In hello [previous workflow walkthrough](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging) we've seen how a single MXF input asset can be converted into an output asset with multi-bitrate MP4 files, an audio-only MP4 file and a manifest file for use in conjunction with Azure Media Services dynamic packaging.</span></span>

<span data-ttu-id="12015-339">Den här genomgången visar hur vissa aspekter av hello kan förbättras och görs enklare.</span><span class="sxs-lookup"><span data-stu-id="12015-339">This walkthrough will show how some of hello aspects can be enhanced and made more convenient.</span></span>

### <span data-ttu-id="12015-340"><a id="MXF_to_multibitrate_MP4_overview"></a>Översikt över tooenhance för arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="12015-340"><a id="MXF_to_multibitrate_MP4_overview"></a>Workflow overview tooenhance</span></span>
![Multibitrate MP4 arbetsflödet tooenhance](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-multibitrate-mp4-workflow-to-enhance.png)

<span data-ttu-id="12015-342">*Multibitrate MP4 arbetsflödet tooenhance*</span><span class="sxs-lookup"><span data-stu-id="12015-342">*Multibitrate MP4 workflow tooenhance*</span></span>

### <span data-ttu-id="12015-343"><a id="MXF_to__multibitrate_MP4_file_naming"></a>Filen namngivningskonventioner</span><span class="sxs-lookup"><span data-stu-id="12015-343"><a id="MXF_to__multibitrate_MP4_file_naming"></a>File Naming Conventions</span></span>
<span data-ttu-id="12015-344">I tidigare hello-arbetsflöde anges vi ett enkelt uttryck som hello grunden för att generera utdata-filnamn.</span><span class="sxs-lookup"><span data-stu-id="12015-344">In hello previous workflow we specified a simple expression as hello basis for generating output file names.</span></span> <span data-ttu-id="12015-345">Vi har några duplicering men: alla hello hello enskilda utdata filen komponenter angivna sådana uttrycket.</span><span class="sxs-lookup"><span data-stu-id="12015-345">We have some duplication though: all of hello hello individual output file components specified such expression.</span></span>

<span data-ttu-id="12015-346">Till exempel har våra filen utdata-komponenten för hello första videofil konfigurerats med det här uttrycket:</span><span class="sxs-lookup"><span data-stu-id="12015-346">For example, our file output component for hello first video file is configured with this expression:</span></span>

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_640x360_1.MP4

<span data-ttu-id="12015-347">Vi har ett uttryck som medan för hello andra utdata video:</span><span class="sxs-lookup"><span data-stu-id="12015-347">While for hello second output video, we have an expression like:</span></span>

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}_960x540_2.MP4

<span data-ttu-id="12015-348">Skulle det vara tydligare mindre fel felbenägna och bekvämare om vi kan ta bort några av duplicering och göra det mer konfigureras i stället?</span><span class="sxs-lookup"><span data-stu-id="12015-348">Wouldn't it be cleaner, less error prone and more convenient if we could remove some of this duplication and make things more configurable instead?</span></span> <span data-ttu-id="12015-349">Som tur kan vi: hello designer uttryck funktioner i kombination med hello möjlighet toocreate anpassade egenskaper på vår arbetsflödet rot ger oss ett extra lager av bekvämlighet.</span><span class="sxs-lookup"><span data-stu-id="12015-349">Luckily we can: hello designer's expression capabilities in combination with hello ability toocreate custom properties on our workflow root will give us an added layer of convenience.</span></span>

<span data-ttu-id="12015-350">Antar vi att vi ska hårddiskkonfiguration filename från hello bithastighet hello enskilda MP4-filer.</span><span class="sxs-lookup"><span data-stu-id="12015-350">Let's assume we'll drive filename configuration from hello bitrates of hello individual MP4 files.</span></span> <span data-ttu-id="12015-351">Dessa bithastighet mål tooconfigure i en central placera (på hello-roten för våra graph) från där användarna ska komma åt tooconfigure och enheten genereringen av filnamnet.</span><span class="sxs-lookup"><span data-stu-id="12015-351">These bitrates we'll aim tooconfigure in one central place (on hello root of our graph), from where they'll be accessed tooconfigure and drive file name generation.</span></span> <span data-ttu-id="12015-352">toodo kan vi börja med att publicera hello bithastighet egenskap från båda AVC kodare toohello roten för våra arbetsflödet, så att den blir tillgänglig från båda hello rot samt från och med hello AVC kodare.</span><span class="sxs-lookup"><span data-stu-id="12015-352">toodo this, we start by publishing hello bitrate property from both AVC encoders toohello root of our workflow, so that it becomes accessible from both hello root as well as from hello AVC encoders.</span></span> <span data-ttu-id="12015-353">(Även om visas i två olika platser, det är endast ett underliggande värde.)</span><span class="sxs-lookup"><span data-stu-id="12015-353">(Even if displayed in two different spots, there's only one underlying value.)</span></span>

### <span data-ttu-id="12015-354"><a id="MXF_to__multibitrate_MP4_publishing"></a>Publicering av egenskaperna för komponenten på hello arbetsflödet rot</span><span class="sxs-lookup"><span data-stu-id="12015-354"><a id="MXF_to__multibitrate_MP4_publishing"></a>Publishing component properties onto hello workflow root</span></span>
<span data-ttu-id="12015-355">Öppna hello första AVC-kodaren, gå toohello bithastighet (kbps) egenskap och välj Publicera hello listrutan.</span><span class="sxs-lookup"><span data-stu-id="12015-355">Open hello first AVC encoder, go toohello Bitrate (kbps) property and from hello dropdown choose Publish.</span></span>

![Publicerade hello bithastighet egenskapen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-bitrate-property.png)

<span data-ttu-id="12015-357">*Publicerade hello bithastighet egenskapen*</span><span class="sxs-lookup"><span data-stu-id="12015-357">*Publishing hello bitrate property*</span></span>

<span data-ttu-id="12015-358">Konfigurera hello publicera dialogrutan toopublish toohello roten för våra arbetsflöde diagram med en publicerade namnet ”video1bitrate” och en läsbar visningsnamn ”Video 1 bithastighet”.</span><span class="sxs-lookup"><span data-stu-id="12015-358">Configure hello publish dialog toopublish toohello root of our workflow graph, with a published name of "video1bitrate" and a readable display name of "Video 1 Bitrate".</span></span> <span data-ttu-id="12015-359">Konfigurera ett eget gruppnamn kallas ”strömning bithastighet” och klicka på Publicera.</span><span class="sxs-lookup"><span data-stu-id="12015-359">Configure a custom group name called "Streaming Bitrates" and hit Publish.</span></span>

![Publicerade hello bithastighet egenskapen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-dialog-for-bitrate-property.png)

<span data-ttu-id="12015-361">*Publishing dialogrutan för egenskapen bithastighet*</span><span class="sxs-lookup"><span data-stu-id="12015-361">*Publishing dialog for bitrate property*</span></span>

<span data-ttu-id="12015-362">Upprepa hello samma för hello bithastighet-egenskapen för hello andra AVC-kodare och ge den namnet ”video2bitrate” med ett namn för ”Video 2 bithastighet”, i hello samma anpassade gruppen ”strömning bithastighet”.</span><span class="sxs-lookup"><span data-stu-id="12015-362">Repeat hello same for hello bitrate property of hello second AVC encoder and name it "video2bitrate" with a display name of "Video 2 Bitrate", in hello same custom group "Streaming Bitrates".</span></span>

<span data-ttu-id="12015-363">Om vi nu granska hello arbetsflödesegenskaper rot, se vi vår anpassad grupp med hello två publicerade egenskaper visas.</span><span class="sxs-lookup"><span data-stu-id="12015-363">If we now inspect hello workflow root properties, we'll see our custom group with hello two published properties show up.</span></span> <span data-ttu-id="12015-364">Både reflektion hello värdet för deras respektive AVC-kodaren bithastighet.</span><span class="sxs-lookup"><span data-stu-id="12015-364">Both are reflecting hello value of their respective AVC encoder bitrate.</span></span>

![Egenskaper för publicerade bithastighet i rot-arbetsflöde](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-published-bitrate-props-on-workflow-root.png)

<span data-ttu-id="12015-366">När vi vill tooaccess dessa egenskaper från kod eller ett uttryck kan vi göra det så här:</span><span class="sxs-lookup"><span data-stu-id="12015-366">Whenever we want tooaccess these properties from code or from an expression, we can do so like this:</span></span>

* <span data-ttu-id="12015-367">från infogad kod från en komponent under hello rot: node.getPropertyAsString('.. / video1bitrate', null)</span><span class="sxs-lookup"><span data-stu-id="12015-367">from inline code from a component right below hello root: node.getPropertyAsString('../video1bitrate',null)</span></span>
* <span data-ttu-id="12015-368">i ett uttryck: ${ROOT_video1bitrate}</span><span class="sxs-lookup"><span data-stu-id="12015-368">within an expression: ${ROOT_video1bitrate}</span></span>

<span data-ttu-id="12015-369">Vi Slutför hello ”strömning bithastighet” grupp genom att publicera våra ljud spåra bithastighet på den också.</span><span class="sxs-lookup"><span data-stu-id="12015-369">Let's complete hello "Streaming Bitrates" group by publishing our audio track bitrate on it as well.</span></span> <span data-ttu-id="12015-370">Sök efter hello bithastighet inställningen i hello egenskaper hello AAC kodare, och välj Publicera hello dropdown nästa tooit.</span><span class="sxs-lookup"><span data-stu-id="12015-370">Within hello properties of hello AAC Encoder, search for hello Bitrate setting and select Publish from hello dropdown next tooit.</span></span> <span data-ttu-id="12015-371">Publicera toohello rot hello diagrammet med namnet ”audio1bitrate” och visningsnamnet ”ljud 1 bithastighet” i vårt anpassade grupp ”strömning bithastighet”.</span><span class="sxs-lookup"><span data-stu-id="12015-371">Publish toohello root of hello graph with name "audio1bitrate" and display name "Audio 1 Bitrate" within our custom group "Streaming Bitrates".</span></span>

![Publishing dialogrutan för ljud bithastighet](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-dialog-for-audio-bitrate.png)

<span data-ttu-id="12015-373">*Publishing dialogrutan för ljud bithastighet*</span><span class="sxs-lookup"><span data-stu-id="12015-373">*Publishing dialog for audio bitrate*</span></span>

![Resulterande video och ljud sammanställer på rot](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-resulting-video-and-audio-props-on-root.png)

<span data-ttu-id="12015-375">*Resulterande video och ljud sammanställer på rot*</span><span class="sxs-lookup"><span data-stu-id="12015-375">*Resulting video and audio props on root*</span></span>

<span data-ttu-id="12015-376">Observera att även om du ändrar något av dessa tre värden konfigurerar igen och ändringar hello värden på respektive hello komponenter som de är kopplade till (och där publicerade från).</span><span class="sxs-lookup"><span data-stu-id="12015-376">Note that changing any of those three values also re-configures and changes hello values on hello respective components they are linked with (and where published from).</span></span>

### <span data-ttu-id="12015-377"><a id="MXF_to__multibitrate_MP4_output_files"></a>Har genererat utdatafilen namn är beroende av publicerade egenskapsvärden</span><span class="sxs-lookup"><span data-stu-id="12015-377"><a id="MXF_to__multibitrate_MP4_output_files"></a>Have generated output file names rely on published property values</span></span>
<span data-ttu-id="12015-378">I stället för hardcoding våra genererade filnamn, vi kan nu ändra våra filename-uttrycket på varje hello utdata komponenter toorely på hello bithastighet egenskaper som vi just publicerad på hello diagrammet rot.</span><span class="sxs-lookup"><span data-stu-id="12015-378">Instead of hardcoding our generated file names, we can now change our filename expression on each of hello File Output components toorely on hello bitrate properties we just published on hello graph root.</span></span> <span data-ttu-id="12015-379">Med vår utdata från första början hitta hello filegenskapen och redigera hello uttryck så här:</span><span class="sxs-lookup"><span data-stu-id="12015-379">Starting with our first file output, find hello File property and edit hello expression like this:</span></span>

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_video1bitrate}kbps.MP4

<span data-ttu-id="12015-380">hello olika parametrar i det här uttrycket kan nås och anges genom att träffa hello dollartecken på hello tangentbord i hello uttrycksfönster.</span><span class="sxs-lookup"><span data-stu-id="12015-380">hello different parameters in this expression can be accessed and entered by hitting hello dollar sign on hello keyboard while in hello expression window.</span></span> <span data-ttu-id="12015-381">En av hello tillgängliga parametrar är vår video1bitrate-egenskap som vi tidigare publicerade.</span><span class="sxs-lookup"><span data-stu-id="12015-381">One of hello available parameters is our video1bitrate property which we published earlier.</span></span>

![Åtkomst till parametrar i ett uttryck](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-accessing-parameters-within-an-expression.png)

<span data-ttu-id="12015-383">*Åtkomst till parametrar i ett uttryck*</span><span class="sxs-lookup"><span data-stu-id="12015-383">*Accessing parameters within an expression*</span></span>

<span data-ttu-id="12015-384">Hello samma för hello utdata för våra andra video:</span><span class="sxs-lookup"><span data-stu-id="12015-384">Do hello same for hello file output for our second video:</span></span>

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_video2bitrate}kbps.MP4

<span data-ttu-id="12015-385">och för hello ljuddata utdata:</span><span class="sxs-lookup"><span data-stu-id="12015-385">and for hello audio-only file output:</span></span>

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_audio1bitrate}bps_audio.MP4

<span data-ttu-id="12015-386">Om vi nu ändra bithastighet hello för någon av hello video eller ljudfiler hello respektive kodaren ska konfigureras och hello bithastighet-baserat namn konventionen att användas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="12015-386">If we now change hello bitrate for any of hello video or audio files, hello respective encoder will be reconfigured and hello bitrate-based file name convention will be honored all automatic.</span></span>

## <span data-ttu-id="12015-387"><a id="thumbnails_to__multibitrate_MP4"></a>Lägger till miniatyrbilder toomultibitrate MP4-utdata</span><span class="sxs-lookup"><span data-stu-id="12015-387"><a id="thumbnails_to__multibitrate_MP4"></a>Adding thumbnails toomultibitrate MP4 output</span></span>
<span data-ttu-id="12015-388">Från ett arbetsflöde som genererar [en multibitrate MP4-utdata från ett MXF indata](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), vi kommer nu att titta på lägger till miniatyrbilder toohello utdata.</span><span class="sxs-lookup"><span data-stu-id="12015-388">Starting from a workflow that generates [a multibitrate MP4 output from an MXF input](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), we will now be looking into adding thumbnails toohello output.</span></span>

### <span data-ttu-id="12015-389"><a id="thumbnails_to__multibitrate_MP4_overview"></a>Arbetsflödet översikt tooadd miniatyrer</span><span class="sxs-lookup"><span data-stu-id="12015-389"><a id="thumbnails_to__multibitrate_MP4_overview"></a>Workflow overview tooadd thumbnails to</span></span>
![Multibitrate MP4 arbetsflöde toostart från](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-multibitrate-mp4-workflow-to-start-from.png)

<span data-ttu-id="12015-391">*Multibitrate MP4 arbetsflöde toostart från*</span><span class="sxs-lookup"><span data-stu-id="12015-391">*Multibitrate MP4 workflow toostart from*</span></span>

### <span data-ttu-id="12015-392"><a id="thumbnails_to__multibitrate_MP4__with_jpg"></a>Lägga till JPG kodning</span><span class="sxs-lookup"><span data-stu-id="12015-392"><a id="thumbnails_to__multibitrate_MP4__with_jpg"></a>Adding JPG Encoding</span></span>
<span data-ttu-id="12015-393">hello hjärtat i vår miniatyr generation blir hello JPG kodare komponenten kan toooutput JPG-filer.</span><span class="sxs-lookup"><span data-stu-id="12015-393">hello heart of our thumbnail generation will be hello JPG Encoder component, able toooutput JPG files.</span></span>

![JPG-kodaren](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-jpg-encoder.png)

<span data-ttu-id="12015-395">*JPG-kodaren*</span><span class="sxs-lookup"><span data-stu-id="12015-395">*JPG Encoder*</span></span>

<span data-ttu-id="12015-396">Vi kan inte ansluta våra okomprimerade Video-ström men direkt från hello Media filen indata till hello JPG-kodaren.</span><span class="sxs-lookup"><span data-stu-id="12015-396">We cannot however directly connect our Uncompressed Video stream from hello Media File Input into hello JPG encoder.</span></span> <span data-ttu-id="12015-397">I stället förväntar toobe lämnas enskilda ramar.</span><span class="sxs-lookup"><span data-stu-id="12015-397">Instead, it expects toobe handed individual frames.</span></span> <span data-ttu-id="12015-398">Detta kan vi kan göra via hello Video ram Gate-komponent.</span><span class="sxs-lookup"><span data-stu-id="12015-398">This, we can do through hello Video Frame Gate component.</span></span>

![Ansluta en ram gate toohello JPG-kodaren](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connect-frame-gate-to-jpg-encoder.png)

<span data-ttu-id="12015-400">*Ansluta en ram gate toohello JPG-kodaren*</span><span class="sxs-lookup"><span data-stu-id="12015-400">*Connecting a frame gate toohello JPG encoder*</span></span>

<span data-ttu-id="12015-401">hello ram gate när varje så många sekunder eller ramar kan en bildruta toopass.</span><span class="sxs-lookup"><span data-stu-id="12015-401">hello frame gate once every so many seconds or frames allows a video frame toopass.</span></span> <span data-ttu-id="12015-402">hello intervall och hello tidsförskjutningen med det här inträffar kan konfigureras i egenskaperna för hello.</span><span class="sxs-lookup"><span data-stu-id="12015-402">hello interval and hello time offset with which this happens is configurable in hello properties.</span></span>

![Video ram Gate-egenskaper](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-video-frame-gate-properties.png)

<span data-ttu-id="12015-404">*Video ram Gate-egenskaper*</span><span class="sxs-lookup"><span data-stu-id="12015-404">*Video Frame Gate properties*</span></span>

<span data-ttu-id="12015-405">Nu ska vi skapa en miniatyr i minuten genom att ange hello läge tooTime (sekunder) och hello intervall too60.</span><span class="sxs-lookup"><span data-stu-id="12015-405">Let's create a thumbnail every minute by setting hello mode tooTime (seconds) and hello Interval too60.</span></span>

### <span data-ttu-id="12015-406"><a id="thumbnails_to__multibitrate_MP4_color_space"></a>Hantera färg utrymme konvertering</span><span class="sxs-lookup"><span data-stu-id="12015-406"><a id="thumbnails_to__multibitrate_MP4_color_space"></a>Dealing with Color Space conversion</span></span>
<span data-ttu-id="12015-407">Medan verkar det logiskt både okomprimerade Video PIN-koder för hello ram-gate och hello Media filen indata kan nu vara ansluten, vi skulle få en varning om vi skulle göra.</span><span class="sxs-lookup"><span data-stu-id="12015-407">While it would seem logical both Uncompressed Video pins of hello frame gate and hello Media File Input can now be connected, we would get a warning if we would do so.</span></span>

![Indatafärgen utrymme fel](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-input-color-space-error.png)

<span data-ttu-id="12015-409">*Indatafärgen utrymme fel*</span><span class="sxs-lookup"><span data-stu-id="12015-409">*Input color space error*</span></span>

<span data-ttu-id="12015-410">Det beror på att hello sätt i vilken färg information visas i vår ursprungliga rådata okomprimerade video-ström, kommer från våra MXF skiljer sig från vilka hello JPG kodaren förväntar sig.</span><span class="sxs-lookup"><span data-stu-id="12015-410">This is because hello way in which colour information is represented in our original raw uncompressed video stream, coming from our MXF, is different from what hello JPG Encoder is expecting.</span></span> <span data-ttu-id="12015-411">Mer specifikt en s.k. ”färg utrymme” av ”RGB” eller ”gråskala” är förväntat tooflow i.</span><span class="sxs-lookup"><span data-stu-id="12015-411">More specifically, a so-called "color space" of "RGB" or "Grayscale" is expected tooflow in.</span></span> <span data-ttu-id="12015-412">Det innebär att hello Video ram Gates inkommande video-ström behöver toohave en konvertering som tillämpas om dess färg utrymme först.</span><span class="sxs-lookup"><span data-stu-id="12015-412">This means that hello Video Frame Gate's inbound video stream will need toohave a conversion applied regarding its color space first.</span></span>

<span data-ttu-id="12015-413">Dra till hello arbetsflödet hello färg utrymme konverteraren - Intel och anslut den tooour ram gate.</span><span class="sxs-lookup"><span data-stu-id="12015-413">Drag onto hello workflow hello Color Space Converter - Intel and connect it tooour frame gate.</span></span>

![Ansluta en färg utrymme konverteraren](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connect-color-space-convertor.png)

<span data-ttu-id="12015-415">*Ansluta en färg utrymme konverteraren*</span><span class="sxs-lookup"><span data-stu-id="12015-415">*Connecting a Color Space Convertor*</span></span>

<span data-ttu-id="12015-416">Välj hello BGR 24 post hello förinställda listan i egenskapsfönstret för hello.</span><span class="sxs-lookup"><span data-stu-id="12015-416">In hello properties window, pick hello BGR 24 entry from hello Preset list.</span></span>

### <span data-ttu-id="12015-417"><a id="thumbnails_to__multibitrate_MP4_writing_thumbnails"></a>Skrivning hello miniatyrer</span><span class="sxs-lookup"><span data-stu-id="12015-417"><a id="thumbnails_to__multibitrate_MP4_writing_thumbnails"></a>Writing hello thumbnails</span></span>
<span data-ttu-id="12015-418">Skiljer sig från våra MP4-video, hello JPG-kodaren komponenten kommer att bli mer än en fil.</span><span class="sxs-lookup"><span data-stu-id="12015-418">Different from our MP4 video's, hello JPG Encoder component will output more than one file.</span></span> <span data-ttu-id="12015-419">I ordning toodeal med den här, en komponent som scen Sök JPG-fil skrivaren kan användas: det tar hello inkommande JPG miniatyrer och skriva ut dem varje filnamn som suffixet av ett annat nummer.</span><span class="sxs-lookup"><span data-stu-id="12015-419">In order toodeal with this, a Scene Search JPG File Writer component can be used: it will take hello incoming JPG thumbnails and write them out, each filename being suffixed by a different number.</span></span> <span data-ttu-id="12015-420">(hello nummer vanligtvis som indikerar hello antal sekunder/enheter i hello dataströmmen som hello miniatyr ritades från.)</span><span class="sxs-lookup"><span data-stu-id="12015-420">(hello number typically indicating hello number of seconds/units in hello stream which hello thumbnail was drawn from.)</span></span>

![Introduktion till hello scen Sök JPG-fil skrivare](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scene-search-jpg-file-writer.png)

<span data-ttu-id="12015-422">*Introduktion till hello scen Sök JPG-fil skrivare*</span><span class="sxs-lookup"><span data-stu-id="12015-422">*Introducing hello Scene Search JPG File Writer*</span></span>

<span data-ttu-id="12015-423">Konfigurera hello utdata mappsökväg med hello uttryck: ${ROOT_outputWriteDirectory}</span><span class="sxs-lookup"><span data-stu-id="12015-423">Configure hello Output Folder Path property with hello expression: ${ROOT_outputWriteDirectory}</span></span>

<span data-ttu-id="12015-424">och hello Filename Prefix egenskap med:</span><span class="sxs-lookup"><span data-stu-id="12015-424">and hello Filename Prefix property with:</span></span>

    ${ROOT_sourceFileBaseName}_thumb_

<span data-ttu-id="12015-425">hello prefix avgör hur hello miniatyr filer namnges.</span><span class="sxs-lookup"><span data-stu-id="12015-425">hello prefix will determine how hello thumbnail files are being named.</span></span> <span data-ttu-id="12015-426">De kommer att med suffixet tal som anger hello par position i hello dataströmmen.</span><span class="sxs-lookup"><span data-stu-id="12015-426">They will be suffixed with a number indicating hello thumb's position in hello stream.</span></span>

![Egenskaper för scen Sök JPG-fil Writer](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scene-search-jpg-file-writer-properties.png)

<span data-ttu-id="12015-428">*Egenskaper för scen Sök JPG-fil Writer*</span><span class="sxs-lookup"><span data-stu-id="12015-428">*Scene Search JPG File Writer properties*</span></span>

<span data-ttu-id="12015-429">Ansluta hello scen Sök JPG-fil Writer toohello fil/Utdatatillgången nod.</span><span class="sxs-lookup"><span data-stu-id="12015-429">Connect hello Scene Search JPG File Writer toohello Output File/Asset node.</span></span>

### <span data-ttu-id="12015-430"><a id="thumbnails_to__multibitrate_MP4_errors"></a>Identifiering av fel i ett arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="12015-430"><a id="thumbnails_to__multibitrate_MP4_errors"></a>Detecting errors in a workflow</span></span>
<span data-ttu-id="12015-431">Ansluta hello indata för hello färg utrymme konverteraren toohello raw okomprimerade videoutgång.</span><span class="sxs-lookup"><span data-stu-id="12015-431">Connect hello input of hello color space converter toohello raw uncompressed video output.</span></span> <span data-ttu-id="12015-432">Nu utföra lokala testa hello arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="12015-432">Now perform a local test run for hello workflow.</span></span> <span data-ttu-id="12015-433">Det finns ett troligt hello arbetsflöde plötsligt kan avbrytas och ange med en röd ram på hello-komponent som ett fel uppstod:</span><span class="sxs-lookup"><span data-stu-id="12015-433">There's a good chance hello workflow will suddenly stop executing and indicate with a red outline on hello component that encountered an error:</span></span>

![Färg utrymme konverteraren fel](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-color-space-converter-error.png)

<span data-ttu-id="12015-435">*Färg utrymme konverteraren fel*</span><span class="sxs-lookup"><span data-stu-id="12015-435">*Color Space Converter error*</span></span>

<span data-ttu-id="12015-436">Klicka hello lite red ”E”-ikonen i hello övre högra hörnet av hello färg utrymme konverteraren komponenten toosee vad är hello orsak hello kodning försök misslyckades.</span><span class="sxs-lookup"><span data-stu-id="12015-436">Click hello little red "E" icon in hello top right corner of hello Color Space Converter component toosee what's hello reason hello encoding attempt failed.</span></span>

![Färg utrymme konverteraren feldialogrutan](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-color-space-converter-error-dialog.png)

<span data-ttu-id="12015-438">*Färg utrymme konverteraren feldialogrutan*</span><span class="sxs-lookup"><span data-stu-id="12015-438">*Color Space Converter error dialog*</span></span>

<span data-ttu-id="12015-439">Det visar sig, som du kan se att hello inkommande färg utrymme standard för hello färg utrymme konverterare har toobe rec601 för våra begärda konvertering av YUV tooRGB.</span><span class="sxs-lookup"><span data-stu-id="12015-439">It turns out, as you can see, that hello incoming color space standard for hello color space converter has toobe rec601 for our requested conversion of YUV tooRGB.</span></span> <span data-ttu-id="12015-440">Vår dataströmmen anger uppenbarligen inte dess rec601.</span><span class="sxs-lookup"><span data-stu-id="12015-440">Apparently our stream doesn't indicate it's rec601.</span></span> <span data-ttu-id="12015-441">(Rek 601 är en standard för kodning sammanflätade analoga video signaler i digital video form.</span><span class="sxs-lookup"><span data-stu-id="12015-441">(Rec 601 is a standard for encoding interlaced analog video signals in digital video form.</span></span> <span data-ttu-id="12015-442">Det anger en aktiva regionen som täcker 720 ljusstyrkan exemplen och 360 krominans prover per rad.</span><span class="sxs-lookup"><span data-stu-id="12015-442">It specifies an active region covering 720 luminance samples and 360 chrominance samples per line.</span></span> <span data-ttu-id="12015-443">hello färg kodning system kallas YCbCr 4:2:2.)</span><span class="sxs-lookup"><span data-stu-id="12015-443">hello color encoding system is known as YCbCr 4:2:2.)</span></span>

<span data-ttu-id="12015-444">toofix kan vi ska ange hello metadata för våra dataström som vi behandlar rec601 innehåll.</span><span class="sxs-lookup"><span data-stu-id="12015-444">toofix this, we'll indicate on hello metadata of our stream that we're dealing with rec601 content.</span></span> <span data-ttu-id="12015-445">toodo så vi använder en Video Data typen Updater-komponent som vi ska placera between våra rådata käll- och hello färg utrymme konvertering komponent.</span><span class="sxs-lookup"><span data-stu-id="12015-445">toodo so we'll use a Video Data Type Updater component, which we'll put in between our raw source and hello color space conversion component.</span></span> <span data-ttu-id="12015-446">Den här data typen updater tillåter hello manuell uppdatering av vissa video data Typegenskaper.</span><span class="sxs-lookup"><span data-stu-id="12015-446">This data type updater allows for hello manual update of certain video data type properties.</span></span> <span data-ttu-id="12015-447">Konfigurera den tooindicate en färg utrymme Standard för ”Rec 601”.</span><span class="sxs-lookup"><span data-stu-id="12015-447">Configure it tooindicate a Color Space Standard of "Rec 601".</span></span> <span data-ttu-id="12015-448">Detta innebär att hello Video Data typen Updater tootag hello dataström med hello ”Rec 601” färg utrymme om det fanns ingen färg utrymme har definierats ännu.</span><span class="sxs-lookup"><span data-stu-id="12015-448">This will cause hello Video Data Type Updater tootag hello stream with hello "Rec 601" color space if there was no color space defined yet.</span></span> <span data-ttu-id="12015-449">(Inte åsidosätter eventuella befintliga metadata om hello åsidosättning var markerad.)</span><span class="sxs-lookup"><span data-stu-id="12015-449">(It will not override any existing metadata, unless hello Override checkbox was checked.)</span></span>

![Uppdatera färg utrymme Standard på hello Data typen Updater](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-update-color-space-standard-on-data-type.png)

<span data-ttu-id="12015-451">*Uppdatera färg utrymme Standard på hello Data typen Updater*</span><span class="sxs-lookup"><span data-stu-id="12015-451">*Updating Color Space Standard on hello Data Type Updater*</span></span>

### <span data-ttu-id="12015-452"><a id="thumbnails_to__multibitrate_MP4_finish"></a>Klar arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="12015-452"><a id="thumbnails_to__multibitrate_MP4_finish"></a>Finished Workflow</span></span>
<span data-ttu-id="12015-453">Nu när våra våra arbetsflödet har slutförts, göra en annan testkörning toosee den skicka.</span><span class="sxs-lookup"><span data-stu-id="12015-453">Now that our our workflow is finished, do another test run toosee it pass.</span></span>

![Klar arbetsflöde för flera mp4 utdata med miniatyrbilder](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow-for-multi-mp4-thumbnails.png)

<span data-ttu-id="12015-455">*Klar arbetsflöde för flera mp4 utdata med miniatyrbilder*</span><span class="sxs-lookup"><span data-stu-id="12015-455">*Finished workflow for multi-mp4 output with thumbnails*</span></span>

## <span data-ttu-id="12015-456"><a id="time_based_trim"></a>Tidsbaserade trimning multibitrate MP4-utdata</span><span class="sxs-lookup"><span data-stu-id="12015-456"><a id="time_based_trim"></a>Time-based trimming of multibitrate MP4 output</span></span>
<span data-ttu-id="12015-457">Från ett arbetsflöde som genererar [en multibitrate MP4-utdata från ett MXF indata](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), vi kommer nu att titta på trimning hello källa video baserat på tidsstämplar.</span><span class="sxs-lookup"><span data-stu-id="12015-457">Starting from a workflow that generates [a multibitrate MP4 output from an MXF input](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), we will now be looking into trimming hello source video based on time-stamps.</span></span>

### <span data-ttu-id="12015-458"><a id="time_based_trim_start"></a>Arbetsflödet översikt toostart lägger till trimning till</span><span class="sxs-lookup"><span data-stu-id="12015-458"><a id="time_based_trim_start"></a>Workflow overview toostart adding trimming to</span></span>
![Starta arbetsflödet tooadd trimning till](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-starting-workflow-to-add-trimming.png)

<span data-ttu-id="12015-460">*Starta arbetsflödet tooadd trimning till*</span><span class="sxs-lookup"><span data-stu-id="12015-460">*Starting workflow tooadd trimming to*</span></span>

### <span data-ttu-id="12015-461"><a id="time_based_trim_use_stream_trimmer"></a>Med hjälp av hello dataströmmen Trimmer</span><span class="sxs-lookup"><span data-stu-id="12015-461"><a id="time_based_trim_use_stream_trimmer"></a>Using hello Stream Trimmer</span></span>
<span data-ttu-id="12015-462">hello kan dataströmmen Trimmer tootrim hello början och slutet på en indataström som baseras på tidsinformation (sekunder, minuter,...). hello trimmer stöder inte ram-baserade trimning.</span><span class="sxs-lookup"><span data-stu-id="12015-462">hello Stream Trimmer component allows tootrim hello beginning and ending of an input stream base on timing information (seconds, minutes, ...). hello trimmer does not support frame-based trimming.</span></span>

![Dataströmmen Trimmer](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-stream-trimmer.png)

<span data-ttu-id="12015-464">*Dataströmmen Trimmer*</span><span class="sxs-lookup"><span data-stu-id="12015-464">*Stream Trimmer*</span></span>

<span data-ttu-id="12015-465">I stället för att länka hello AVC kodare och talare position du toohello Media filen indata direkt, låter vi mellan dessa hello dataströmmen trimmer.</span><span class="sxs-lookup"><span data-stu-id="12015-465">Instead of linking hello AVC encoders and speaker position assigner toohello Media File Input directly, we'll put in between those hello stream trimmer.</span></span> <span data-ttu-id="12015-466">(En för hello video signal och en för hello överlagrad ljud signal.)</span><span class="sxs-lookup"><span data-stu-id="12015-466">(One for hello video signal and one for hello interleaved audio signal.)</span></span>

![Placera dataströmmen Trimmer mellan](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-put-stream-trimmer-in-between.png)

<span data-ttu-id="12015-468">*Placera dataströmmen Trimmer mellan*</span><span class="sxs-lookup"><span data-stu-id="12015-468">*Put Stream Trimmer in between*</span></span>

<span data-ttu-id="12015-469">Nu ska vi konfigurera hello trimmer så att vi endast bearbeta video och ljud mellan 15 sekunder och 60 sekunder i hello videon.</span><span class="sxs-lookup"><span data-stu-id="12015-469">Let's configure hello trimmer so that we will only process video and audio between 15 seconds and 60 seconds in hello video.</span></span>

<span data-ttu-id="12015-470">Gå toohello egenskaper för hello Video dataströmmen Trimmer och konfigurera både starttid (15 sek) och sluttid (60 s)-egenskaper.</span><span class="sxs-lookup"><span data-stu-id="12015-470">Go toohello properties of hello Video Stream Trimmer and configure both Start Time (15s) and End Time (60s) properties.</span></span> <span data-ttu-id="12015-471">toomake till både våra ljud och video trimmer är alltid konfigurerad toohello samma börja och sluta värden, vi publicerar dessa toohello roten för hello arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="12015-471">toomake sure both our audio and video trimmer are always configured toohello same start and end values, we will publish those toohello root of hello workflow.</span></span>

![Publicera start tid egenskap från dataströmmen Trimmer](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-start-time-from-stream-trimmer.png)

<span data-ttu-id="12015-473">*Publicera start tid egenskap från dataströmmen Trimmer*</span><span class="sxs-lookup"><span data-stu-id="12015-473">*Publish start time property from Stream Trimmer*</span></span>

![Publicera egenskapsdialogrutan för starttid](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-dialog-for-start-time.png)

<span data-ttu-id="12015-475">*Publicera egenskapsdialogrutan för starttid*</span><span class="sxs-lookup"><span data-stu-id="12015-475">*Publish property dialog for start time*</span></span>

![Publicera egenskapsdialogrutan för sluttid](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-dialog-for-end-time.png)

<span data-ttu-id="12015-477">*Publicera egenskapsdialogrutan för sluttid*</span><span class="sxs-lookup"><span data-stu-id="12015-477">*Publish property dialog for end time*</span></span>

<span data-ttu-id="12015-478">Om vi nu granska hello roten för våra arbetsflödet, blir snyggt visas och konfigureras i båda egenskaperna därifrån.</span><span class="sxs-lookup"><span data-stu-id="12015-478">If we now inspect hello root of our workflow, both properties will be neatly displayed and configurable from there.</span></span>

![Publicerade egenskaper som är tillgängliga på rot](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-published-properties-available-on-root.png)

<span data-ttu-id="12015-480">*Publicerade egenskaper som är tillgängliga på rot*</span><span class="sxs-lookup"><span data-stu-id="12015-480">*Published properties available on root*</span></span>

<span data-ttu-id="12015-481">Öppna hello trimning egenskaper från hello ljud trimmer och konfigurera både start- och sluttider med ett uttryck som refererar toohello publicerade egenskaper i vår arbetsflödet hello rot.</span><span class="sxs-lookup"><span data-stu-id="12015-481">Now open hello trimming properties from hello audio trimmer and configure both start and end times with an expression that refers toohello published properties on hello root of our workflow.</span></span>

<span data-ttu-id="12015-482">För hello ljud trimning starttid:</span><span class="sxs-lookup"><span data-stu-id="12015-482">For hello audio trimming start time:</span></span>

    ${ROOT_TrimmingStartTime}

<span data-ttu-id="12015-483">och för dess Sluttid:</span><span class="sxs-lookup"><span data-stu-id="12015-483">and for its end time:</span></span>

    ${ROOT_TrimmingEndTime}

### <span data-ttu-id="12015-484"><a id="time_based_trim_finish"></a>Klar arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="12015-484"><a id="time_based_trim_finish"></a>Finished Workflow</span></span>
![Klar arbetsflöde](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow-time-base-trimming.png)

<span data-ttu-id="12015-486">*Klar arbetsflöde*</span><span class="sxs-lookup"><span data-stu-id="12015-486">*Finished Workflow*</span></span>

## <span data-ttu-id="12015-487"><a id="scripting"></a>Introduktion till hello skripta komponent</span><span class="sxs-lookup"><span data-stu-id="12015-487"><a id="scripting"></a>Introducing hello Scripted Component</span></span>
<span data-ttu-id="12015-488">Skriptbaserade komponenter kan köra godtycklig skript under hello körning av våra arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="12015-488">Scripted Components can execute arbitrary scripts during hello execution phases of our workflow.</span></span> <span data-ttu-id="12015-489">Det finns fyra olika skript som kan utföras med specifika egenskaper och deras egen plats i hello arbetsflödet livscykel:</span><span class="sxs-lookup"><span data-stu-id="12015-489">There are four different scripts that can be executed, each with specific characteristics and their own place in hello workflow life-cycle:</span></span>

* <span data-ttu-id="12015-490">**commandScript**</span><span class="sxs-lookup"><span data-stu-id="12015-490">**commandScript**</span></span>
* <span data-ttu-id="12015-491">**realizeScript**</span><span class="sxs-lookup"><span data-stu-id="12015-491">**realizeScript**</span></span>
* <span data-ttu-id="12015-492">**processInputScript**</span><span class="sxs-lookup"><span data-stu-id="12015-492">**processInputScript**</span></span>
* <span data-ttu-id="12015-493">**lifeCycleScript**</span><span class="sxs-lookup"><span data-stu-id="12015-493">**lifeCycleScript**</span></span>

<span data-ttu-id="12015-494">hello-dokumentationen för hello går skripta komponent i detalj för varje hello ovan.</span><span class="sxs-lookup"><span data-stu-id="12015-494">hello documentation of hello Scripted Component goes in more detail for each of hello above.</span></span> <span data-ttu-id="12015-495">I [hello följande avsnitt](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim), hello **realizeScript** scripting komponenten är används tooconstruct en cliplist xml på hello direkt när hello arbetsflöde startar.</span><span class="sxs-lookup"><span data-stu-id="12015-495">In [hello following section](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim), hello **realizeScript** scripting component is used tooconstruct a cliplist xml on hello fly when hello workflow starts.</span></span> <span data-ttu-id="12015-496">Det här skriptet anropas under hello komponenten installationsprogrammet, vilket sker bara en gång i dess livscykel.</span><span class="sxs-lookup"><span data-stu-id="12015-496">This script is called during hello component setup, which happens only once in it's lifecycle.</span></span>

### <span data-ttu-id="12015-497"><a id="scripting_hello_world"></a>Skript i ett arbetsflöde: hello world</span><span class="sxs-lookup"><span data-stu-id="12015-497"><a id="scripting_hello_world"></a>Scripting within a workflow: hello world</span></span>
<span data-ttu-id="12015-498">Dra en skripta komponent till hello designytan och byta namn på den (till exempel ”SetClipListXML”).</span><span class="sxs-lookup"><span data-stu-id="12015-498">Drag a Scripted Component onto hello designer surface and rename it (for example, "SetClipListXML").</span></span>

![Lägga till en skriptbaserade komponent](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-scripted-comp.png)

<span data-ttu-id="12015-500">*Lägga till en skriptbaserade komponent*</span><span class="sxs-lookup"><span data-stu-id="12015-500">*Adding a Scripted Component*</span></span>

<span data-ttu-id="12015-501">När du undersöker hello egenskaper för hello skripta komponent, hello fyra olika skript typer visas, varje konfigurerbara tooa olika skript.</span><span class="sxs-lookup"><span data-stu-id="12015-501">When you inspect hello properties of hello Scripted Component, hello four different script types will be shown, each configurable tooa different script.</span></span>

![Egenskaperna för komponenten för skript](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scripted-comp-properties.png)

<span data-ttu-id="12015-503">*Egenskaperna för komponenten för skript*</span><span class="sxs-lookup"><span data-stu-id="12015-503">*Scripted Component properties*</span></span>

<span data-ttu-id="12015-504">Rensa hello processInputScript och öppna hello Redigeraren för hello realizeScript.</span><span class="sxs-lookup"><span data-stu-id="12015-504">Clear hello processInputScript and open hello editor for hello realizeScript.</span></span> <span data-ttu-id="12015-505">Nu vi har konfigurerat och redo toostart skript.</span><span class="sxs-lookup"><span data-stu-id="12015-505">Now we're set up and ready toostart scripting.</span></span>

<span data-ttu-id="12015-506">Skript är skrivna i Groovy, ett dynamiskt kompilerade skriptspråk för hello Java platform som behåller kompatibilitet med Java.</span><span class="sxs-lookup"><span data-stu-id="12015-506">Scripts are written in Groovy, a dynamically compiled scripting language for hello Java platform that retains compatibility with Java.</span></span> <span data-ttu-id="12015-507">De flesta Java-kod är faktiskt, giltig Groovy kod.</span><span class="sxs-lookup"><span data-stu-id="12015-507">Actually, most Java code is valid Groovy code.</span></span>

<span data-ttu-id="12015-508">Dags att skriva skript för groovy en enkel hello world hello tillsammans med vår realizeScript.</span><span class="sxs-lookup"><span data-stu-id="12015-508">Let's write a simple hello world groovy script in hello context of our realizeScript.</span></span> <span data-ttu-id="12015-509">Ange följande hello i hello editor:</span><span class="sxs-lookup"><span data-stu-id="12015-509">Enter hello following in hello editor:</span></span>

    node.log("hello world");

<span data-ttu-id="12015-510">Nu ska du köra en lokal testkörning.</span><span class="sxs-lookup"><span data-stu-id="12015-510">Now execute a local test run.</span></span> <span data-ttu-id="12015-511">När du kör, inspektera (via fliken hello på hello skripta komponenten) hello loggar egenskapen.</span><span class="sxs-lookup"><span data-stu-id="12015-511">After this run, inspect (through hello System tab on hello Scripted Component) hello Logs property.</span></span>

![Hello world loggutdata](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-output.png)

<span data-ttu-id="12015-513">*Hello world loggutdata*</span><span class="sxs-lookup"><span data-stu-id="12015-513">*Hello world log output*</span></span>

<span data-ttu-id="12015-514">hello nodobjektet som vi kallar hello Loggmetod, refererar tooour aktuella ”noden” eller hello komponenten vi skript i.</span><span class="sxs-lookup"><span data-stu-id="12015-514">hello node object we call hello log method on, refers tooour current "node" or hello component we're scripting within.</span></span> <span data-ttu-id="12015-515">Varje komponent har därför hello möjlighet toooutput loggningsdata, tillgängliga via fliken hello. I det här fallet utdata vi hello stränglitteral ”hello world”.</span><span class="sxs-lookup"><span data-stu-id="12015-515">Every component as such has hello ability toooutput logging data, available through hello system tab. In this case, we output hello string literal "hello world".</span></span> <span data-ttu-id="12015-516">Viktiga toounderstand här är att denna funktion kan vara toobe ett ovärderligt felsökning verktyg som ger dig information om vilka hello skriptet faktiskt göra.</span><span class="sxs-lookup"><span data-stu-id="12015-516">Important toounderstand here is that this can prove toobe an invaluable debugging tool, providing you with insight on what hello script is actually doing.</span></span>

<span data-ttu-id="12015-517">Från inom vårt skriptmiljö ha vi även åtkomst tooproperties för andra komponenter.</span><span class="sxs-lookup"><span data-stu-id="12015-517">From within our scripting environment, we also have access tooproperties on other components.</span></span> <span data-ttu-id="12015-518">Prova följande:</span><span class="sxs-lookup"><span data-stu-id="12015-518">Try this:</span></span>

    //inspect current node:
    def nodepath = node.getNodePath();
    node.log("this node path: " + nodepath);

    //walking up tooother nodes:
    def parentnode = node.getParentNode();
    def parentnodepath = parentnode.getNodePath();
    node.log("parent node path: " + parentnodepath);

    //read properties from a node:
    def sourceFileExt = parentnode.getPropertyAsString( "sourceFileExtension", null );
    def sourceFileName = parentnode.getPropertyAsString("sourceFileBaseName", null);
    node.log("source file name with extension " + sourceFileExt + " is: " + sourceFileName);

<span data-ttu-id="12015-519">Vår visas i loggfönstret oss hello följande:</span><span class="sxs-lookup"><span data-stu-id="12015-519">Our log window will show us hello following:</span></span>

![Utdata för att komma åt noden sökvägar](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-output2.png)

<span data-ttu-id="12015-521">*Utdata för att komma åt noden sökvägar*</span><span class="sxs-lookup"><span data-stu-id="12015-521">*Log output for accessing node paths*</span></span>

## <span data-ttu-id="12015-522"><a id="frame_based_trim"></a>RAM-baserade trimning multibitrate MP4-utdata</span><span class="sxs-lookup"><span data-stu-id="12015-522"><a id="frame_based_trim"></a>Frame-based trimming of multibitrate MP4 output</span></span>
<span data-ttu-id="12015-523">Från ett arbetsflöde som genererar [en multibitrate MP4-utdata från ett MXF indata](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), vi kommer nu att titta på trimning hello källa video baserat på ramen.</span><span class="sxs-lookup"><span data-stu-id="12015-523">Starting from a workflow that generates [a multibitrate MP4 output from an MXF input](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), we will now be looking into trimming hello source video based on frame counts.</span></span>

### <span data-ttu-id="12015-524"><a id="frame_based_trim_start"></a>Utkast översikt toostart lägger till trimning till</span><span class="sxs-lookup"><span data-stu-id="12015-524"><a id="frame_based_trim_start"></a>Blueprint overview toostart adding trimming to</span></span>
![Arbetsflödet toostart lägger till trimning till](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-workflow-start-adding-trimming-to.png)

<span data-ttu-id="12015-526">*Arbetsflödet toostart lägger till trimning till*</span><span class="sxs-lookup"><span data-stu-id="12015-526">*Workflow toostart adding trimming to*</span></span>

### <span data-ttu-id="12015-527"><a id="frame_based_trim_clip_list"></a>Med hjälp av hello Clip listan XML</span><span class="sxs-lookup"><span data-stu-id="12015-527"><a id="frame_based_trim_clip_list"></a>Using hello Clip List XML</span></span>
<span data-ttu-id="12015-528">I alla tidigare arbetsflöde självstudier använde vi hello Media filen inkommande komponenten som våra video Indatakällan.</span><span class="sxs-lookup"><span data-stu-id="12015-528">In all previous workflow tutorials, we used hello Media File Input component as our video input source.</span></span> <span data-ttu-id="12015-529">För dessa specifika fall men ska vi använda hello Clip listan Källkomponenten i stället.</span><span class="sxs-lookup"><span data-stu-id="12015-529">For this specific scenario though, we'll be using hello Clip List Source component instead.</span></span> <span data-ttu-id="12015-530">Observera att det inte får vara hello önskad sätt att arbeta. endast använda hello Clip datakälla när det finns en verklig orsak toodo så (precis som i hello nedan fall där vi gör användningen av hello clip listan trimning funktioner).</span><span class="sxs-lookup"><span data-stu-id="12015-530">Note that this should not be hello preferred way of working; only use hello Clip List Source when there's a real reason toodo so (like in hello below case, where we're making use of hello clip list trimming capabilities).</span></span>

<span data-ttu-id="12015-531">tooswitch från våra Media filen indata toohello Clip datakälla, dra hello Clip listan Källkomponenten till hello designytan och koppla hello Clip listan XML PIN-kod toohello Clip listan XML-noden av hello workflow designer.</span><span class="sxs-lookup"><span data-stu-id="12015-531">tooswitch from our Media File Input toohello Clip List Source, drag hello Clip List Source component onto hello design surface and connect hello Clip List XML pin toohello Clip List XML node of hello workflow designer.</span></span> <span data-ttu-id="12015-532">Detta ska fylla hello Clip datakälla med utdata PIN-koder, enligt tooour indatavideo.</span><span class="sxs-lookup"><span data-stu-id="12015-532">This should populate hello Clip List Source with output pins, according tooour input video.</span></span> <span data-ttu-id="12015-533">Ansluta nu hello okomprimerade Video- och okomprimerade PIN-koder hello hello Clip datakälla toohello respektive AVC kodare och ljud dataströmmen Interleaver.</span><span class="sxs-lookup"><span data-stu-id="12015-533">Now connect hello Uncompressed Video and Uncompressed Audio pins from hello hello Clip List Source toohello respective AVC Encoders and Audio Stream Interleaver.</span></span> <span data-ttu-id="12015-534">Nu ska du ta bort hello Media filen indata.</span><span class="sxs-lookup"><span data-stu-id="12015-534">Now remove hello Media File Input.</span></span>

![Ersatt hello Media filen indata med hello Clip datakälla](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-replaced-media-file-with-clip-source.png)

<span data-ttu-id="12015-536">*Ersatt hello Media filen indata med hello Clip datakälla*</span><span class="sxs-lookup"><span data-stu-id="12015-536">*Replaced hello Media File Input with hello Clip List Source*</span></span>

<span data-ttu-id="12015-537">hello Clip listan Källkomponenten tar som indata en ”Clip listan XML”.</span><span class="sxs-lookup"><span data-stu-id="12015-537">hello Clip List Source component takes as its input a "Clip List XML".</span></span> <span data-ttu-id="12015-538">När du väljer hello källa filen tootest med lokalt, är den här clip listan xml fylls automatiskt åt dig.</span><span class="sxs-lookup"><span data-stu-id="12015-538">When selecting hello source file tootest with locally, this clip list xml is auto-populated for you.</span></span>

![Automatisk fyllts Clip listan XML-egenskapen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-auto-populated-clip-list-xml-property.png)

<span data-ttu-id="12015-540">*Automatisk fyllts Clip listan XML-egenskapen*</span><span class="sxs-lookup"><span data-stu-id="12015-540">*Auto-populated Clip List XML property*</span></span>

<span data-ttu-id="12015-541">Ser lite närmare toohello xml gör är detta hur det ser ut:</span><span class="sxs-lookup"><span data-stu-id="12015-541">Looking a bit closer toohello xml, this is how it looks like:</span></span>

![Clip Listdialogrutan redigera](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-edit-clip-list-dialog.png)

<span data-ttu-id="12015-543">*Clip Listdialogrutan redigera*</span><span class="sxs-lookup"><span data-stu-id="12015-543">*Edit clip list dialog*</span></span>

<span data-ttu-id="12015-544">Detta men avspeglar inte hello funktioner hello clip listan XML.</span><span class="sxs-lookup"><span data-stu-id="12015-544">This however does not reflect hello capabilities of hello clip list xml.</span></span> <span data-ttu-id="12015-545">Vi kan tooadd elementet ”Rensa” under både hello video och ljud källa, så här:</span><span class="sxs-lookup"><span data-stu-id="12015-545">One option we have is tooadd a "Trim" element under both hello video and audio source, like this:</span></span>

![Lägga till en trim toohello clip elementlistan](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-adding-trim-element-to-clip-list.png)

<span data-ttu-id="12015-547">*Lägga till en trim toohello clip elementlistan*</span><span class="sxs-lookup"><span data-stu-id="12015-547">*Adding a trim element toohello clip list*</span></span>

<span data-ttu-id="12015-548">Om du ändrar hello clip listan xml så här ovan och utföra lokala testa ser du hello video korrekt tagits bort mellan 10 och 20 sekunder i hello videon.</span><span class="sxs-lookup"><span data-stu-id="12015-548">If you modify hello clip list xml like this above and perform a local test run, you'll see hello video correctly been trimmed between 10 and 20 seconds in hello video.</span></span>

<span data-ttu-id="12015-549">Motstridiga toowhat händer när du gör en lokal körning, även om den här samma cliplist xml inte skulle ha hello samma effekt när det används i ett arbetsflöde som körs i Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="12015-549">Contrary toowhat happens when you do a local run though, this very same cliplist xml would not have hello same effect when applied in a workflow that runs in Azure Media Services.</span></span> <span data-ttu-id="12015-550">När Azure Premium-kodare startar, hello cliplist xml genereras varje gång igen, baserat på hello indatafilen hello kodning gavs jobb.</span><span class="sxs-lookup"><span data-stu-id="12015-550">When Azure Premium Encoder starts, hello cliplist xml is generated every time again, based on hello input file hello encoding job was given.</span></span> <span data-ttu-id="12015-551">Det innebär att alla ändringar som vi gör hello XML-tyvärr skulle åsidosättas.</span><span class="sxs-lookup"><span data-stu-id="12015-551">This means that any changes we do on hello xml would unfortunately be overridden.</span></span>

<span data-ttu-id="12015-552">toocounter hello cliplist xml som rensas när ett kodningsjobb startas vi kan generera den på hello direkt precis efter hello början av våra arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="12015-552">toocounter hello cliplist xml being wiped when an encoding job is started, we can re-generate it on hello fly just after hello start of our workflow.</span></span> <span data-ttu-id="12015-553">Dessa anpassade åtgärder kan hämtas via något som kallas en ”Skriptfönster Component”.</span><span class="sxs-lookup"><span data-stu-id="12015-553">Such custom actions can be taken through what is called a "Scripted Component".</span></span> <span data-ttu-id="12015-554">Mer information finns i [introduktion till hello skripta komponenten](media-services-media-encoder-premium-workflow-tutorials.md#scripting).</span><span class="sxs-lookup"><span data-stu-id="12015-554">For more information, see [Introducing hello Scripted Component](media-services-media-encoder-premium-workflow-tutorials.md#scripting).</span></span>

<span data-ttu-id="12015-555">Dra en skripta komponent till hello designytan och byta namn på den för ”SetClipListXML”.</span><span class="sxs-lookup"><span data-stu-id="12015-555">Drag a Scripted Component onto hello designer surface and rename it too"SetClipListXML".</span></span>

![Lägga till en skriptbaserade komponent](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-scripted-comp.png)

<span data-ttu-id="12015-557">*Lägga till en skriptbaserade komponent*</span><span class="sxs-lookup"><span data-stu-id="12015-557">*Adding a Scripted Component*</span></span>

<span data-ttu-id="12015-558">När du undersöker hello egenskaper för hello skripta komponent, hello fyra olika skript typer visas, varje konfigurerbara tooa olika skript.</span><span class="sxs-lookup"><span data-stu-id="12015-558">When you inspect hello properties of hello Scripted Component, hello four different script types will be shown, each configurable tooa different script.</span></span>

![Egenskaperna för komponenten för skript](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scripted-comp-properties.png)

<span data-ttu-id="12015-560">*Egenskaperna för komponenten för skript*</span><span class="sxs-lookup"><span data-stu-id="12015-560">*Scripted Component properties*</span></span>

### <span data-ttu-id="12015-561"><a id="frame_based_trim_modify_clip_list"></a>När du ändrar hello clip listan från en skript-komponent</span><span class="sxs-lookup"><span data-stu-id="12015-561"><a id="frame_based_trim_modify_clip_list"></a>Modifying hello clip list from a Scripted Component</span></span>
<span data-ttu-id="12015-562">Innan vi kan Skriv hello cliplist xml som genereras under starten av arbetsflödet, behöver vi toohave åtkomst toohello cliplist XML-egenskapen och innehållet.</span><span class="sxs-lookup"><span data-stu-id="12015-562">Before we can re-write hello cliplist xml that is generated during workflow startup, we'll need toohave access toohello cliplist xml property and contents.</span></span> <span data-ttu-id="12015-563">Vi kan göra det så här:</span><span class="sxs-lookup"><span data-stu-id="12015-563">We can do so like this:</span></span>

    // get cliplist xml:
    def clipListXML = node.getProperty("../clipListXml");
    node.log("clip list xml coming in: " + clipListXML);

![Inkommande clip lista som loggas](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-incoming-clip-list-logged.png)

<span data-ttu-id="12015-565">*Inkommande clip lista som loggas*</span><span class="sxs-lookup"><span data-stu-id="12015-565">*Incoming clip list being logged*</span></span>

<span data-ttu-id="12015-566">Vi måste först en sätt toodetermine från vilken plats till vilken plats som vi vill tootrim hello video.</span><span class="sxs-lookup"><span data-stu-id="12015-566">First we need a way toodetermine from which point till which point we want tootrim hello video.</span></span> <span data-ttu-id="12015-567">toomake lämplig toohello tekniskt användaren av hello arbetsflöde publicera två egenskaper toohello rot hello diagrammet.</span><span class="sxs-lookup"><span data-stu-id="12015-567">toomake this convenient toohello less-technical user of hello workflow, publish two properties toohello root of hello graph.</span></span> <span data-ttu-id="12015-568">toodo, högerklicka på hello designytan och välj ”Lägg till egenskap”:</span><span class="sxs-lookup"><span data-stu-id="12015-568">toodo this, right click hello designer surface and select "Add Property":</span></span>

* <span data-ttu-id="12015-569">Första egenskapen: ”ClippingTimeStart” av typen: ”TIDSKOD”</span><span class="sxs-lookup"><span data-stu-id="12015-569">First property: "ClippingTimeStart" of type: "TIMECODE"</span></span>
* <span data-ttu-id="12015-570">Andra egenskapen: ”ClippingTimeEnd” av typen: ”TIDSKOD”</span><span class="sxs-lookup"><span data-stu-id="12015-570">Second property: "ClippingTimeEnd" of type: "TIMECODE"</span></span>

![Lägg till dialogrutan för egenskaper för urklippet starttid](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-clip-start-time.png)

<span data-ttu-id="12015-572">*Lägg till dialogrutan för egenskaper för urklippet starttid*</span><span class="sxs-lookup"><span data-stu-id="12015-572">*Add Property dialog for clipping start time*</span></span>

![Publicerade urklippet tid sammanställer i rot-arbetsflöde](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-clip-time-props.png)

<span data-ttu-id="12015-574">*Publicerade urklippet tid sammanställer i rot-arbetsflöde*</span><span class="sxs-lookup"><span data-stu-id="12015-574">*Published clipping time props on workflow root*</span></span>

<span data-ttu-id="12015-575">Konfigurera båda egenskaperna tooa passande värde:</span><span class="sxs-lookup"><span data-stu-id="12015-575">Configure both properties tooa suitable value:</span></span>

![Konfigurera hello urklippet start- och slutdatum egenskaper](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-configure-clip-start-end-prop.png)

<span data-ttu-id="12015-577">*Konfigurera hello urklippet start- och slutdatum egenskaper*</span><span class="sxs-lookup"><span data-stu-id="12015-577">*Configure hello clipping start and end properties*</span></span>

<span data-ttu-id="12015-578">Nu från i vår skript vi kan komma åt båda egenskaperna så här:</span><span class="sxs-lookup"><span data-stu-id="12015-578">Now, from within our script, we can access both properties, like this:</span></span>

    // get start and end of clipping:
    def clipstart = node.getProperty("../ClippingTimeStart").toString();
    def clipend = node.getProperty("../ClippingTimeEnd").toString();

    node.log("clipping start: " + clipstart);
    node.log("clipping end: " + clipend);

![Log-fönster med början och slutet av Urklipp](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-show-start-end-clip.png)

<span data-ttu-id="12015-580">*Log-fönster med början och slutet av Urklipp*</span><span class="sxs-lookup"><span data-stu-id="12015-580">*Log window showing start and end of clipping*</span></span>

<span data-ttu-id="12015-581">Vi parsa hello tidskod strängar till en enklare toouse i form av ett enkelt reguljära uttryck:</span><span class="sxs-lookup"><span data-stu-id="12015-581">Let's parse hello timecode strings into a more convenient toouse form, using a simple regular expression:</span></span>

    //parse hello start timing:
    def startregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipstart);
    startregresult.matches();
    def starttimecode = startregresult.group(1);
    node.log("timecode start is: " + starttimecode);
    def startframerate = startregresult.group(2);
    node.log("framerate start is: " + startframerate);

    //parse hello end timing:
    def endregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipend);
    endregresult.matches();
    def endtimecode = endregresult.group(1);
    node.log("timecode end is: " + endtimecode);
    def endframerate = endregresult.group(2);
    node.log("framerate end is: " + endframerate);

![Fönstret med utdata från parsade tidskod](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-output-parsed-timecode.png)

<span data-ttu-id="12015-583">*Fönstret med utdata från parsade tidskod*</span><span class="sxs-lookup"><span data-stu-id="12015-583">*Log window with output of parsed timecode*</span></span>

<span data-ttu-id="12015-584">Vi kan nu ändra hello cliplist xml tooreflect hello start och sluttider för hello önskad ram korrekt urklippet av hello film med den här vi information.</span><span class="sxs-lookup"><span data-stu-id="12015-584">With this information at hand, we can now modify hello cliplist xml tooreflect hello start and end times for hello desired frame-accurate clipping of hello movie.</span></span>

![Skriptet tooadd trim kodelement](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-trim-elements.png)

<span data-ttu-id="12015-586">*Skriptet tooadd trim kodelement*</span><span class="sxs-lookup"><span data-stu-id="12015-586">*Script code tooadd trim elements*</span></span>

<span data-ttu-id="12015-587">Detta gjordes via normala sträng manipulering åtgärder.</span><span class="sxs-lookup"><span data-stu-id="12015-587">This was done through normal string manipulation operations.</span></span> <span data-ttu-id="12015-588">hello resulterande ändrade clip listan xml skrivs tillbaka toohello clipListXML egenskapen hello arbetsflödet roten hello ”setProperty” med metoden.</span><span class="sxs-lookup"><span data-stu-id="12015-588">hello resulting modified clip list xml is written back toohello clipListXML property on hello workflow root through hello "setProperty" method.</span></span> <span data-ttu-id="12015-589">hello log-fönstret när du kör ett annat test skulle visar hello följande:</span><span class="sxs-lookup"><span data-stu-id="12015-589">hello log window after another test run would show us hello following:</span></span>

![Loggning hello clip lista](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-result-clip-list.png)

<span data-ttu-id="12015-591">*Loggning hello clip lista*</span><span class="sxs-lookup"><span data-stu-id="12015-591">*Logging hello resulting clip list*</span></span>

<span data-ttu-id="12015-592">Gör en testkörning toosee hur klipps hello video och ljud dataströmmar.</span><span class="sxs-lookup"><span data-stu-id="12015-592">Do a test-run toosee how hello video and audio streams have been clipped.</span></span> <span data-ttu-id="12015-593">När du ska göra mer än en testkörning med olika värden för hello trimning punkter, Lägg märke till att de inte kommer beaktas men!</span><span class="sxs-lookup"><span data-stu-id="12015-593">As you'll do more than one test-run with different values for hello trimming points, you'll notice that those will not be taken into account however!</span></span> <span data-ttu-id="12015-594">hello anledningen är att hello designer, till skillnad från hello Azure runtime, har inte åsidosättning hello cliplist xml var kör.</span><span class="sxs-lookup"><span data-stu-id="12015-594">hello reason for this is that hello designer, unlike hello Azure runtime, does NOT override hello cliplist xml every run.</span></span> <span data-ttu-id="12015-595">Det innebär att endast hello första gången som du har angett hello in och ut punkter, resulterar hello xml tootransform, alla hello andra tider våra guard-satsen (om (clipListXML.indexOf (”<trim>”) == -1)) förhindrar att hello-arbetsflödet lägger till en ny trim elementet när det redan finns en.</span><span class="sxs-lookup"><span data-stu-id="12015-595">This means that only hello very first time you have set hello in and out points, will cause hello xml tootransform, all hello other times, our guard clause (if(clipListXML.indexOf("<trim>") == -1)) will prevent hello workflow from adding another trim element when there's already one present.</span></span>

<span data-ttu-id="12015-596">toomake våra arbetsflödet praktiskt tootest lokalt, vi bästa lägga till dagliga rutiner kod som kontrollerar om en trim elementet fanns redan.</span><span class="sxs-lookup"><span data-stu-id="12015-596">toomake our workflow convenient tootest locally, we best add some house-keeping code that inspects if a trim element was already present.</span></span> <span data-ttu-id="12015-597">I så fall, kan vi ta bort den innan du fortsätter genom att ändra hello xml med hello nya värden.</span><span class="sxs-lookup"><span data-stu-id="12015-597">If so, we can remove it before continuing by modifying hello xml with hello new values.</span></span> <span data-ttu-id="12015-598">I stället för vanliga strängändringar, är det förmodligen säkrare toodo genom verkliga XML-objekt modellen parsning.</span><span class="sxs-lookup"><span data-stu-id="12015-598">Rather than using plain string manipulations, it's probably safer toodo this through real xml object model parsing.</span></span>

<span data-ttu-id="12015-599">Innan vi kan dock lägga till sådan kod, behöver vi tooadd ett antal importuttryck längst hello början av våra skriptet först:</span><span class="sxs-lookup"><span data-stu-id="12015-599">Before we can add such code though, we'll need tooadd a number of import statements at hello start of our script first:</span></span>

    import javax.xml.parsers.*;
    import org.xml.sax.*;
    import org.w3c.dom.*;
    import javax.xml.*;
    import javax.xml.xpath.*;
    import javax.xml.transform.*;
    import javax.xml.transform.stream.*;
    import javax.xml.transform.dom.*;

<span data-ttu-id="12015-600">Sedan kan vi lägga till hello krävs Rensa kod:</span><span class="sxs-lookup"><span data-stu-id="12015-600">After this, we can add hello required cleaning code:</span></span>

    //for local testing: delete any pre-existing trim elements from hello clip list xml by parsing hello xml into a DOM:
    DocumentBuilderFactory factory=DocumentBuilderFactory.newInstance();
    DocumentBuilder builder=factory.newDocumentBuilder();
    InputSource is=new InputSource(new StringReader(clipListXML));
    Document dom=builder.parse(is);

    //find hello trim element inside videoSource and audioSource and remove it if it exists already:
    XPath xpath = XPathFactory.newInstance().newXPath();
    String findAllTrimElements = "//trim";
    NodeList trimelems = xpath.evaluate(findAllTrimElements,dom,XPathConstants.NODESET);

    //copy trim nodes into a "to-be-deleted" collection
    Set<Element> elementsToDelete = new HashSet<Element>();
    for (int i = 0; i < trimelems.getLength(); i++) {
        Element e = (Element)trimelems.item(i);
        elementsToDelete.add(e);
    }

    node.log("about toodelete any existing trim nodes");
     //delete hello trim nodes:
    elementsToDelete.each{
        e -> e.getParentNode().removeChild(e);
    };
    node.log("deleted any existing trim nodes");

    //serialize hello modified clip list xml dom into a string:
    def transformer = TransformerFactory.newInstance().newTransformer();
    StreamResult result = new StreamResult(new StringWriter());
    DOMSource source = new DOMSource(dom);
    transformer.transform(source, result);
    clipListXML = result.getWriter().toString();

<span data-ttu-id="12015-601">Den här koden går ovanför hello punkt där vi lägger till hello trim element toohello cliplist xml.</span><span class="sxs-lookup"><span data-stu-id="12015-601">This code goes just above hello point at which we add hello trim elements toohello cliplist xml.</span></span>

<span data-ttu-id="12015-602">Vi kan nu köra och ändra våra arbetsflödet så mycket som vi vill samtidigt som du har ändringar hello någonsin tid.</span><span class="sxs-lookup"><span data-stu-id="12015-602">At this point, we can run and modify our workflow as much as we want while having hello changes applied ever time.</span></span>    

### <span data-ttu-id="12015-603"><a id="frame_based_trim_clippingenabled_prop"></a>Lägga till en ClippingEnabled bekvämlighet egenskap</span><span class="sxs-lookup"><span data-stu-id="12015-603"><a id="frame_based_trim_clippingenabled_prop"></a>Adding a ClippingEnabled convenience property</span></span>
<span data-ttu-id="12015-604">Du kanske inte alltid vill trimning toohappen, Slutför vi av våra arbetsflöde genom att lägga till en lämplig boolesk flagga som anger om vi vill tooenable trimning / urklippet.</span><span class="sxs-lookup"><span data-stu-id="12015-604">As you might not always want trimming toohappen, let's finish off our workflow by adding a convenient boolean flag that indicates whether or not we want tooenable trimming / clipping.</span></span>

<span data-ttu-id="12015-605">Publicera en ny egenskap toohello rot för våra arbetsflöde kallas ”ClippingEnabled” av typen ”BOOLESKT” före, precis som.</span><span class="sxs-lookup"><span data-stu-id="12015-605">Just as before, publish a new property toohello root of our workflow called "ClippingEnabled" of type "BOOLEAN".</span></span>

![Publicerade en egenskap för att aktivera Urklipp](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-enable-clip.png)

<span data-ttu-id="12015-607">*Publicerade en egenskap för att aktivera Urklipp*</span><span class="sxs-lookup"><span data-stu-id="12015-607">*Published a property for enabling clipping*</span></span>

<span data-ttu-id="12015-608">Med hello nedan enkla guard-satsen, vi kontrollera om trimning krävs och bestämma om våra clip listan som sådana måste toobe ändras eller inte.</span><span class="sxs-lookup"><span data-stu-id="12015-608">With hello below simple guard clause, we can check if trimming is required and decide if our clip list as such needs toobe modified or not.</span></span>

    //check if clipping is required:
    def clippingrequired = node.getProperty("../ClippingEnabled");
    node.log("clipping required: " + clippingrequired.toString());
    if(clippingrequired == null || clippingrequired == false)
    {
        node.setProperty("../clipListXml",clipListXML);
        node.log("no clipping required");
        return;
    }


### <span data-ttu-id="12015-609"><a id="code"></a>Fullständiga koden</span><span class="sxs-lookup"><span data-stu-id="12015-609"><a id="code"></a>Complete code</span></span>
    import javax.xml.parsers.*;
    import org.xml.sax.*;
    import org.w3c.dom.*;
    import javax.xml.*;
    import javax.xml.xpath.*;
    import javax.xml.transform.*;
    import javax.xml.transform.stream.*;
    import javax.xml.transform.dom.*;

    // get cliplist xml:
    def clipListXML = node.getProperty("../clipListXml");
    node.log("clip list xml coming in: \n" + clipListXML);
    // get start and end of clipping:
    def clipstart = node.getProperty("../ClippingTimeStart").toString();
    def clipend = node.getProperty("../ClippingTimeEnd").toString();

    //parse hello start timing:
    def startregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipstart);
    startregresult.matches();
    def starttimecode = startregresult.group(1);
    node.log("timecode start is: " + starttimecode);
    def startframerate = startregresult.group(2);
    node.log("framerate start is: " + startframerate);

    //parse hello end timing:
    def endregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipend);
    endregresult.matches();
    def endtimecode = endregresult.group(1);
    node.log("timecode end is: " + endtimecode);
    def endframerate = endregresult.group(2);

    node.log("framerate end is: " + endframerate);

    //for local testing: delete any pre-existing trim elements
    //from hello clip list xml by parsing hello xml into a DOM:

    DocumentBuilderFactory factory=DocumentBuilderFactory.newInstance();
    DocumentBuilder builder=factory.newDocumentBuilder();
    InputSource is=new InputSource(new StringReader(clipListXML));
    Document dom=builder.parse(is);

    //find hello trim element inside videoSource and audioSource and remove it if it exists already:
    XPath xpath = XPathFactory.newInstance().newXPath();
    String findAllTrimElements = "//trim";
    NodeList trimelems = xpath.evaluate(findAllTrimElements, dom, XPathConstants.NODESET);

    //copy trim nodes into a "to-be-deleted" collection
    Set<Element> elementsToDelete = new HashSet<Element>();
    for (int i = 0; i < trimelems.getLength(); i++) {
        Element e = (Element)trimelems.item(i);
        elementsToDelete.add(e);
    }

    node.log("about toodelete any existing trim nodes");
    //delete hello trim nodes:
    elementsToDelete.each{ e ->
        e.getParentNode().removeChild(e);
    };
    node.log("deleted any existing trim nodes");

    //serialize hello modified clip list xml dom into a string:
    def transformer = TransformerFactory.newInstance().newTransformer();
    StreamResult result = new StreamResult(new StringWriter());
    DOMSource source = new DOMSource(dom);
    transformer.transform(source, result);
    clipListXML = result.getWriter().toString();

    //check if clipping is required:
    def clippingrequired = node.getProperty("../ClippingEnabled");
    node.log("clipping required: " + clippingrequired.toString());
    if(clippingrequired == null || clippingrequired == false)
    {
        node.setProperty("../clipListXml",clipListXML);
        node.log("no clipping required");
        return;
    }

    //add trim elements toocliplist xml
    if ( clipListXML.indexOf("<trim>") == -1 )
    {
        //trim video
        clipListXML = clipListXML.replace("<videoSource>","<videoSource>\n <trim>\n <inPoint fps=\""+
            startframerate +"\">" + starttimecode +
            "</inPoint>\n" + "<outPoint fps=\"" + endframerate +"\"> " + endtimecode +
            " </outPoint>\n </trim> \n");
        //trim audio
        clipListXML = clipListXML.replace("<audioSource>","<audioSource>\n <trim>\n <inPoint fps=\""+
            startframerate +"\">" + starttimecode +
            "</inPoint>\n" + "<outPoint fps=\""+ endframerate +"\">" +
            endtimecode + "</outPoint>\n </trim>\n");
        node.log( "clip list going out: \n" +clipListXML );
        node.setProperty("../clipListXml",clipListXML);
    }


## <a name="also-see"></a><span data-ttu-id="12015-610">Se även</span><span class="sxs-lookup"><span data-stu-id="12015-610">Also see</span></span>
[<span data-ttu-id="12015-611">Introduktion till Premium-kodning i Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="12015-611">Introducing Premium Encoding in Azure Media Services</span></span>](http://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services)

[<span data-ttu-id="12015-612">Hur tooUse Premium kodning i Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="12015-612">How tooUse Premium Encoding in Azure Media Services</span></span>](http://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services)

[<span data-ttu-id="12015-613">Koda innehåll på begäran med Azure Media Service</span><span class="sxs-lookup"><span data-stu-id="12015-613">Encoding On-Demand Content with Azure Media Service</span></span>](media-services-encode-asset.md#media-encoder-premium-workflow)

[<span data-ttu-id="12015-614">Arbetsflödesformat och codecs för Media Encoder Premium</span><span class="sxs-lookup"><span data-stu-id="12015-614">Media Encoder Premium Workflow Formats and Codecs</span></span>](media-services-premium-workflow-encoder-formats.md)

[<span data-ttu-id="12015-615">Exempelfiler för arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="12015-615">Sample workflow files</span></span>](http://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows)

[<span data-ttu-id="12015-616">Azure Media Services Explorer-verktyget</span><span class="sxs-lookup"><span data-stu-id="12015-616">Azure Media Services Explorer tool</span></span>](http://aka.ms/amse)

## <a name="media-services-learning-paths"></a><span data-ttu-id="12015-617">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="12015-617">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="12015-618">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="12015-618">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
