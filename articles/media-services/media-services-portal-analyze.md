---
title: "Analysera media med hjälp av Azure portal | Microsoft Docs"
description: "Det här avsnittet beskrivs hur du bearbetar media med Media Analytics-medieprocessorer (MP) med Azure-portalen."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 18213fc1-74f5-4074-a32b-02846fe90601
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: juliako
ms.openlocfilehash: 22032aef0cc8b7b015503043028873e70c21ee85
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="analyze-your-media-using-the-azure-portal"></a><span data-ttu-id="943e8-103">Analysera dina medier med Azure Portal</span><span class="sxs-lookup"><span data-stu-id="943e8-103">Analyze your media using the Azure portal</span></span>
> [!NOTE]
> <span data-ttu-id="943e8-104">Du behöver ett Azure-konto för att slutföra den här självstudien.</span><span class="sxs-lookup"><span data-stu-id="943e8-104">To complete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="943e8-105">Mer information finns i [kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="943e8-105">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
> 
> 

## <a name="overview"></a><span data-ttu-id="943e8-106">Översikt</span><span class="sxs-lookup"><span data-stu-id="943e8-106">Overview</span></span>
<span data-ttu-id="943e8-107">Azure Media Services Analytics är en samling tal- och visionskomponenter komponenter (på företagsnivå, kompatibilitet, säkerhet och globalt omfattande) som gör det lättare för organisationer och företag att härleda insikter som går att åtgärda från sina videofiler.</span><span class="sxs-lookup"><span data-stu-id="943e8-107">Azure Media Services Analytics is a collection of speech and vision components (at enterprise scale, compliance, security and global reach) that make it easier for organizations and enterprises to derive actionable insights from their video files.</span></span> <span data-ttu-id="943e8-108">Mer detaljerad översikt över Azure Media Services Analytics finns [detta](media-services-analytics-overview.md) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="943e8-108">For more detailed overview of Azure Media Services Analytics see [this](media-services-analytics-overview.md) topic.</span></span> 

<span data-ttu-id="943e8-109">Det här avsnittet beskrivs hur du bearbetar media med Media Analytics-medieprocessorer (MP) med Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="943e8-109">This topic discusses how to process your media with Media Analytics media processors (MPs) using the Azure portal.</span></span> <span data-ttu-id="943e8-110">Media Analytics MP producerar MP4-filer eller JSON-filer.</span><span class="sxs-lookup"><span data-stu-id="943e8-110">Media Analytics MPs produce MP4 files or JSON files.</span></span> <span data-ttu-id="943e8-111">Om en medieprocessor har producerat en MP4-fil kan du hämta filen progressivt.</span><span class="sxs-lookup"><span data-stu-id="943e8-111">If a media processor produced an MP4 file, you can progressively download the file.</span></span> <span data-ttu-id="943e8-112">Om en medieprocessor har producerat en JSON-fil kan du hämta filen från Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="943e8-112">If a media processor produced a JSON file, you can download the file from the Azure blob storage.</span></span> 

## <a name="choose-an-asset-that-you-want-to-analyze"></a><span data-ttu-id="943e8-113">Välj en tillgång som du vill analysera</span><span class="sxs-lookup"><span data-stu-id="943e8-113">Choose an asset that you want to analyze</span></span>
1. <span data-ttu-id="943e8-114">Välj ditt Azure Media Services-konto i [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="943e8-114">In the [Azure portal](https://portal.azure.com/), select your Azure Media Services account.</span></span>
2. <span data-ttu-id="943e8-115">I fönstret **Inställningar** väljer du **Tillgångar**.</span><span class="sxs-lookup"><span data-stu-id="943e8-115">In the **Settings** window, select **Assets**.</span></span>  
   <span data-ttu-id="943e8-116">.</span><span class="sxs-lookup"><span data-stu-id="943e8-116">.</span></span>
    <span data-ttu-id="943e8-117">![Analysera videor](./media/media-services-portal-analyze/media-services-portal-analyze001.png)</span><span class="sxs-lookup"><span data-stu-id="943e8-117">![Analyze videos](./media/media-services-portal-analyze/media-services-portal-analyze001.png)</span></span>
3. <span data-ttu-id="943e8-118">Välj den tillgång som du vill analysera och tryck på den **analysera** knappen.</span><span class="sxs-lookup"><span data-stu-id="943e8-118">Select the asset that you would like to analyze and press the **Analyze** button.</span></span>
   
    ![Analysera videor](./media/media-services-portal-analyze/media-services-portal-analyze002.png)
4. <span data-ttu-id="943e8-120">I den **processen media tillgång med Media Analytics** fönstret Välj processorn.</span><span class="sxs-lookup"><span data-stu-id="943e8-120">In the **Process media asset with  Media Analytics** window, select the processor.</span></span> 
   
    <span data-ttu-id="943e8-121">Resten av artikeln förklarar varför och hur du använder varje processor.</span><span class="sxs-lookup"><span data-stu-id="943e8-121">The rest of the article explains why and how to use each processor.</span></span> 
5. <span data-ttu-id="943e8-122">Tryck på **skapa** att starta ett jobb.</span><span class="sxs-lookup"><span data-stu-id="943e8-122">Press **Create** to the start a job.</span></span>

## <a name="azure-media-indexer"></a><span data-ttu-id="943e8-123">Azure Media Indexer</span><span class="sxs-lookup"><span data-stu-id="943e8-123">Azure Media Indexer</span></span>
<span data-ttu-id="943e8-124">Den **Azure Media Indexer** medieprocessor kan du göra mediefiler och innehåll sökbara samt generera stängd textning spår.</span><span class="sxs-lookup"><span data-stu-id="943e8-124">The **Azure Media Indexer** media processor enables you to make media files and content searchable, as well as generate closed captioning tracks.</span></span> <span data-ttu-id="943e8-125">Detta avsnitt ger information om alternativ som du kan ange för denna MP.</span><span class="sxs-lookup"><span data-stu-id="943e8-125">This sections gives some details about options that you can specify for this MP.</span></span>

![Analysera videor](./media/media-services-portal-analyze/media-services-portal-analyze003.png)

### <a name="language"></a><span data-ttu-id="943e8-127">Språk</span><span class="sxs-lookup"><span data-stu-id="943e8-127">Language</span></span>
<span data-ttu-id="943e8-128">Naturligt språk ska kunna identifieras i fil.</span><span class="sxs-lookup"><span data-stu-id="943e8-128">The natural language to be recognized in the multimedia file.</span></span> <span data-ttu-id="943e8-129">Till exempel engelska och spanska.</span><span class="sxs-lookup"><span data-stu-id="943e8-129">For example, English or Spanish.</span></span> 

### <a name="captions"></a><span data-ttu-id="943e8-130">Etiketter</span><span class="sxs-lookup"><span data-stu-id="943e8-130">Captions</span></span>
<span data-ttu-id="943e8-131">Du kan välja en etikett-format som kommer att genereras från ditt innehåll.</span><span class="sxs-lookup"><span data-stu-id="943e8-131">You can choose a caption format that will be generated from your content.</span></span> <span data-ttu-id="943e8-132">En indexering jobb kan generera textning filer i följande format:</span><span class="sxs-lookup"><span data-stu-id="943e8-132">An indexing job can generate closed caption files in the following formats:</span></span>  

* <span data-ttu-id="943e8-133">**SAMISKA**</span><span class="sxs-lookup"><span data-stu-id="943e8-133">**SAMI**</span></span>
* <span data-ttu-id="943e8-134">**TTML**</span><span class="sxs-lookup"><span data-stu-id="943e8-134">**TTML**</span></span>
* <span data-ttu-id="943e8-135">**WebVTT**</span><span class="sxs-lookup"><span data-stu-id="943e8-135">**WebVTT**</span></span>

<span data-ttu-id="943e8-136">Stängd beskrivning (CC) filer i dessa format kan användas för att göra det tillgängligt för personer med funktionshinder hörsel ljud- och bildfiler.</span><span class="sxs-lookup"><span data-stu-id="943e8-136">Closed Caption (CC) files in these formats can be used to make audio and video files accessible to people with hearing disability.</span></span>

### <a name="aib-file"></a><span data-ttu-id="943e8-137">AIB fil</span><span class="sxs-lookup"><span data-stu-id="943e8-137">AIB file</span></span>
<span data-ttu-id="943e8-138">Välj det här alternativet om du vill generera ljud Index Blob-fil för användning med anpassade SQL Server IFilter.</span><span class="sxs-lookup"><span data-stu-id="943e8-138">Select this option if you would like to generate the Audio Index Blob file for use with the custom SQL Server IFilter.</span></span> <span data-ttu-id="943e8-139">Mer information finns i [detta](https://azure.microsoft.com/blog/using-aib-files-with-azure-media-indexer-and-sql-server/) blogg.</span><span class="sxs-lookup"><span data-stu-id="943e8-139">For more information, see [this](https://azure.microsoft.com/blog/using-aib-files-with-azure-media-indexer-and-sql-server/) blog.</span></span>

### <a name="keywords"></a><span data-ttu-id="943e8-140">Nyckelord</span><span class="sxs-lookup"><span data-stu-id="943e8-140">Keywords</span></span>
<span data-ttu-id="943e8-141">Välj det här alternativet om du vill generera ett nyckelord XML-fil.</span><span class="sxs-lookup"><span data-stu-id="943e8-141">Select this option if you would like to generate a keywords XML file.</span></span> <span data-ttu-id="943e8-142">Den här filen innehåller nyckelord som extraheras från tal-innehåll med frekvens och offset-information.</span><span class="sxs-lookup"><span data-stu-id="943e8-142">This file contains keywords extracted from the speech content, with frequency and offset information.</span></span>

### <a name="job-name"></a><span data-ttu-id="943e8-143">Jobbnamn</span><span class="sxs-lookup"><span data-stu-id="943e8-143">Job name</span></span>
<span data-ttu-id="943e8-144">Ett eget namn som gör att du kan identifiera jobbet.</span><span class="sxs-lookup"><span data-stu-id="943e8-144">A friendly name that lets you identify the job.</span></span> <span data-ttu-id="943e8-145">[Detta](media-services-portal-check-job-progress.md) artikeln beskriver hur du kan övervaka förloppet för ett jobb.</span><span class="sxs-lookup"><span data-stu-id="943e8-145">[This](media-services-portal-check-job-progress.md) article describes how you can monitor the progress of a job.</span></span> 

### <a name="output-file"></a><span data-ttu-id="943e8-146">Utdatafil</span><span class="sxs-lookup"><span data-stu-id="943e8-146">Output file</span></span>
<span data-ttu-id="943e8-147">Ett eget namn som gör att du kan identifiera vilket innehåll som utdata.</span><span class="sxs-lookup"><span data-stu-id="943e8-147">A friendly name that lets you identify the output content.</span></span> 

## <a name="azure-media-hyperlapse"></a><span data-ttu-id="943e8-148">Azure Media Hyperlapse</span><span class="sxs-lookup"><span data-stu-id="943e8-148">Azure Media Hyperlapse</span></span>
<span data-ttu-id="943e8-149">Azure Media Hyperlapse är en HP som skapar smooth tid upphörde att gälla videor från första person eller åtgärd kamera innehåll.</span><span class="sxs-lookup"><span data-stu-id="943e8-149">Azure Media Hyperlapse is an MP that creates smooth time-lapsed videos from first-person or action-camera content.</span></span>  <span data-ttu-id="943e8-150">Mer information finns i [detta](media-services-hyperlapse-content.md) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="943e8-150">For more information, see [this](media-services-hyperlapse-content.md) topic.</span></span> <span data-ttu-id="943e8-151">Detta avsnitt ger information om alternativ som du kan ange för denna MP.</span><span class="sxs-lookup"><span data-stu-id="943e8-151">This sections gives some details about options that you can specify for this MP.</span></span>

![Analysera videor](./media/media-services-portal-analyze/media-services-portal-analyze004.png)

### <a name="speed"></a><span data-ttu-id="943e8-153">Hastighet</span><span class="sxs-lookup"><span data-stu-id="943e8-153">Speed</span></span>
<span data-ttu-id="943e8-154">Ange den hastighet som kan påskynda inkommande videon.</span><span class="sxs-lookup"><span data-stu-id="943e8-154">Specify the speed with which to speed up the input video.</span></span> <span data-ttu-id="943e8-155">Utdata är ett stabilt och tid slut återgivning av video indata.</span><span class="sxs-lookup"><span data-stu-id="943e8-155">The output is a stabilized and time-lapsed rendition of the input video.</span></span>

### <a name="job-name"></a><span data-ttu-id="943e8-156">Jobbnamn</span><span class="sxs-lookup"><span data-stu-id="943e8-156">Job name</span></span>
<span data-ttu-id="943e8-157">Ett eget namn som gör att du kan identifiera jobbet.</span><span class="sxs-lookup"><span data-stu-id="943e8-157">A friendly name that lets you identify the job.</span></span> <span data-ttu-id="943e8-158">[Detta](media-services-portal-check-job-progress.md) artikeln beskriver hur du kan övervaka förloppet för ett jobb.</span><span class="sxs-lookup"><span data-stu-id="943e8-158">[This](media-services-portal-check-job-progress.md) article describes how you can monitor the progress of a job.</span></span> 

### <a name="output-file"></a><span data-ttu-id="943e8-159">Utdatafil</span><span class="sxs-lookup"><span data-stu-id="943e8-159">Output file</span></span>
<span data-ttu-id="943e8-160">Ett eget namn som gör att du kan identifiera vilket innehåll som utdata.</span><span class="sxs-lookup"><span data-stu-id="943e8-160">A friendly name that lets you identify the output content.</span></span> 

## <a name="azure-media-face-detector"></a><span data-ttu-id="943e8-161">Azure Media Face Detector</span><span class="sxs-lookup"><span data-stu-id="943e8-161">Azure Media Face Detector</span></span>
<span data-ttu-id="943e8-162">Den **Azure Media ansikte detektor** medieprocessor (HP) kan du räkna, spåra förflyttningar och även mäta målgruppen deltagande och reaktion via ansikte uttryck.</span><span class="sxs-lookup"><span data-stu-id="943e8-162">The **Azure Media Face Detector** media processor (MP) enables you to count, track movements, and even gauge audience participation and reaction via facial expressions.</span></span> <span data-ttu-id="943e8-163">Den här tjänsten innehåller två funktioner:</span><span class="sxs-lookup"><span data-stu-id="943e8-163">This service contains two features:</span></span> 

* <span data-ttu-id="943e8-164">**Ansikts-identifiering**</span><span class="sxs-lookup"><span data-stu-id="943e8-164">**Face detection**</span></span>
  
    <span data-ttu-id="943e8-165">Identifiering av framsidan hittar och spårar mänsklig ytor inom en video.</span><span class="sxs-lookup"><span data-stu-id="943e8-165">Face detection finds and tracks human faces within a video.</span></span> <span data-ttu-id="943e8-166">Flera ytor kan identifieras och spåras senare när de flyttas runt, med tid och plats metadata som returneras i en JSON-fil.</span><span class="sxs-lookup"><span data-stu-id="943e8-166">Multiple faces can be detected and subsequently be tracked as they move around, with the time and location metadata returned in a JSON file.</span></span> <span data-ttu-id="943e8-167">Under spårning, görs ett försök att ge en konsekvent ID till samma sida när personen flyttar på skärmen, även om de hindras eller lämna en kort ramen.</span><span class="sxs-lookup"><span data-stu-id="943e8-167">During tracking, it will attempt to give a consistent ID to the same face while the person is moving around on screen, even if they are obstructed or briefly leave the frame.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="943e8-168">Den här tjänster inte att utföra ansiktsigenkänning.</span><span class="sxs-lookup"><span data-stu-id="943e8-168">This services does not perform facial recognition.</span></span> <span data-ttu-id="943e8-169">En person som lämnar ramen eller blir hindras för länge får ett nytt-ID när de kommer tillbaka.</span><span class="sxs-lookup"><span data-stu-id="943e8-169">An individual who leaves the frame or becomes obstructed for too long will be given a new ID when they return.</span></span>
  > 
  > 
* <span data-ttu-id="943e8-170">**Känsloigenkänning**</span><span class="sxs-lookup"><span data-stu-id="943e8-170">**Emotion detection**</span></span>
  
    <span data-ttu-id="943e8-171">Känsloigenkänning är en valfri komponent i ansikte identifiering Medieprocessor som returnerar analysis på flera känslomässiga attribut från ytor upptäckt, inklusive lycka, sadness, fruktan, resulterande och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="943e8-171">Emotion Detection is an optional component of the Face Detection Media Processor that returns analysis on multiple emotional attributes from the faces detected, including happiness, sadness, fear, anger, and more.</span></span> 

![Analysera videor](./media/media-services-portal-analyze/media-services-portal-analyze005.png)

### <a name="detection-mode"></a><span data-ttu-id="943e8-173">Identifieringsläget</span><span class="sxs-lookup"><span data-stu-id="943e8-173">Detection mode</span></span>
<span data-ttu-id="943e8-174">Något av följande lägen kan användas av processorn:</span><span class="sxs-lookup"><span data-stu-id="943e8-174">One of the following modes can be used by the processor:</span></span>

* <span data-ttu-id="943e8-175">ansikts-identifiering</span><span class="sxs-lookup"><span data-stu-id="943e8-175">face detection</span></span>
* <span data-ttu-id="943e8-176">per ansikte känsloigenkänning</span><span class="sxs-lookup"><span data-stu-id="943e8-176">per face emotion detection</span></span>
* <span data-ttu-id="943e8-177">sammanställd känsloigenkänning</span><span class="sxs-lookup"><span data-stu-id="943e8-177">aggregate emotion detection</span></span>

### <a name="job-name"></a><span data-ttu-id="943e8-178">Jobbnamn</span><span class="sxs-lookup"><span data-stu-id="943e8-178">Job name</span></span>
<span data-ttu-id="943e8-179">Ett eget namn som gör att du kan identifiera jobbet.</span><span class="sxs-lookup"><span data-stu-id="943e8-179">A friendly name that lets you identify the job.</span></span> <span data-ttu-id="943e8-180">[Detta](media-services-portal-check-job-progress.md) artikeln beskriver hur du kan övervaka förloppet för ett jobb.</span><span class="sxs-lookup"><span data-stu-id="943e8-180">[This](media-services-portal-check-job-progress.md) article describes how you can monitor the progress of a job.</span></span> 

### <a name="output-file"></a><span data-ttu-id="943e8-181">Utdatafil</span><span class="sxs-lookup"><span data-stu-id="943e8-181">Output file</span></span>
<span data-ttu-id="943e8-182">Ett eget namn som gör att du kan identifiera vilket innehåll som utdata.</span><span class="sxs-lookup"><span data-stu-id="943e8-182">A friendly name that lets you identify the output content.</span></span> 

## <a name="azure-media-motion-detector"></a><span data-ttu-id="943e8-183">Azure Media Motion Detector</span><span class="sxs-lookup"><span data-stu-id="943e8-183">Azure Media Motion Detector</span></span>
<span data-ttu-id="943e8-184">Den **Azure Media rörelsedetektor** medieprocessor (HP) kan du effektivt identifiera avsnitt av intresse i ett annat långa och primärdomänkontrollant video.</span><span class="sxs-lookup"><span data-stu-id="943e8-184">The **Azure Media Motion Detector** media processor (MP) enables you to efficiently identify sections of interest within an otherwise long and uneventful video.</span></span> <span data-ttu-id="943e8-185">Rörelseidentifiering kan användas på statisk övervakningskameror för att identifiera avsnitt av videon där rörelse inträffar.</span><span class="sxs-lookup"><span data-stu-id="943e8-185">Motion detection can be used on static camera footage to identify sections of the video where motion occurs.</span></span> <span data-ttu-id="943e8-186">Den genererar en JSON-fil som innehåller en metadata med tidsstämplar och den omgivande region där händelsen inträffade.</span><span class="sxs-lookup"><span data-stu-id="943e8-186">It generates a JSON file containing a metadata with timestamps and the bounding region where the event occurred.</span></span>

<span data-ttu-id="943e8-187">Riktad mot säkerhet video feeds kan den här tekniken kategorisera rörelse i relevanta händelser och falska positiva identifieringar som skuggor och andra ändringar.</span><span class="sxs-lookup"><span data-stu-id="943e8-187">Targeted towards security video feeds, this technology is able to categorize motion into relevant events and false positives such as shadows and lighting changes.</span></span> <span data-ttu-id="943e8-188">På så sätt kan du skapa säkerhetsaviseringar från kameran feeds utan som skräppost med oändlig irrelevanta händelser när kunna extrahera stund intressanta från extremt långa övervakning videor.</span><span class="sxs-lookup"><span data-stu-id="943e8-188">This allows you to generate security alerts from camera feeds without being spammed with endless irrelevant events, while being able to extract moments of interest from extremely long surveillance videos.</span></span>

![Analysera videor](./media/media-services-portal-analyze/media-services-portal-analyze006.png)

## <a name="azure-media-video-thumbnails"></a><span data-ttu-id="943e8-190">Azure Media Video Thumbnails</span><span class="sxs-lookup"><span data-stu-id="943e8-190">Azure Media Video Thumbnails</span></span>
<span data-ttu-id="943e8-191">Med hjälp av den här processorn kan du skapa sammanfattningar av långa videor automatiskt genom att intressanta kodavsnitt från källvideo.</span><span class="sxs-lookup"><span data-stu-id="943e8-191">This processor can help you create summaries of long videos by automatically selecting interesting snippets from the source video.</span></span> <span data-ttu-id="943e8-192">Detta är användbart när du vill ge en snabb överblick över vad som händer i en lång video.</span><span class="sxs-lookup"><span data-stu-id="943e8-192">This is useful when you want to provide a quick overview of what to expect in a long video.</span></span> <span data-ttu-id="943e8-193">Mer detaljerad information och exempel finns [Använd Azure Media Video-miniatyrer för att skapa en Videosammanfattning](media-services-video-summarization.md)</span><span class="sxs-lookup"><span data-stu-id="943e8-193">For detailed information and examples, see [Use Azure Media Video Thumbnails to Create a Video Summarization](media-services-video-summarization.md)</span></span>

![Analysera videor](./media/media-services-portal-analyze/media-services-portal-analyze008.png)

### <a name="job-name"></a><span data-ttu-id="943e8-195">Jobbnamn</span><span class="sxs-lookup"><span data-stu-id="943e8-195">Job name</span></span>
<span data-ttu-id="943e8-196">Ett eget namn som gör att du kan identifiera jobbet.</span><span class="sxs-lookup"><span data-stu-id="943e8-196">A friendly name that lets you identify the job.</span></span> <span data-ttu-id="943e8-197">[Detta](media-services-portal-check-job-progress.md) artikeln beskriver hur du kan övervaka förloppet för ett jobb.</span><span class="sxs-lookup"><span data-stu-id="943e8-197">[This](media-services-portal-check-job-progress.md) article describes how you can monitor the progress of a job.</span></span> 

### <a name="output-file"></a><span data-ttu-id="943e8-198">Utdatafil</span><span class="sxs-lookup"><span data-stu-id="943e8-198">Output file</span></span>
<span data-ttu-id="943e8-199">Ett eget namn som gör att du kan identifiera vilket innehåll som utdata.</span><span class="sxs-lookup"><span data-stu-id="943e8-199">A friendly name that lets you identify the output content.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="943e8-200">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="943e8-200">Next steps</span></span>
<span data-ttu-id="943e8-201">Visa Media Services utbildningsvägar.</span><span class="sxs-lookup"><span data-stu-id="943e8-201">View Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="943e8-202">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="943e8-202">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

