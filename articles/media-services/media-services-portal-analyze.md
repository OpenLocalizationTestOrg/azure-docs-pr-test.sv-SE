---
title: "aaaAnalyze media med hjälp av hello Azure-portalen | Microsoft Docs"
description: "Det här avsnittet beskrivs hur tooprocess media med Media Analytics-medieprocessorer (MP) med hjälp av hello Azure-portalen."
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
ms.openlocfilehash: d549c0315cd58c3771420379316b4f2ad3c2cea6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-your-media-using-hello-azure-portal"></a><span data-ttu-id="64c95-103">Analysera dina media med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="64c95-103">Analyze your media using hello Azure portal</span></span>
> [!NOTE]
> <span data-ttu-id="64c95-104">toocomplete den här självstudiekursen kommer du behöver ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="64c95-104">toocomplete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="64c95-105">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="64c95-105">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
> 
> 

## <a name="overview"></a><span data-ttu-id="64c95-106">Översikt</span><span class="sxs-lookup"><span data-stu-id="64c95-106">Overview</span></span>
<span data-ttu-id="64c95-107">Azure Media Services Analytics är en samling tal- och visionskomponenter komponenter (på företagsnivå, kompatibilitet, säkerhet och globalt omfattande) som gör det lättare för organisationer och företag tooderive tillämplig insikter från sina videofiler.</span><span class="sxs-lookup"><span data-stu-id="64c95-107">Azure Media Services Analytics is a collection of speech and vision components (at enterprise scale, compliance, security and global reach) that make it easier for organizations and enterprises tooderive actionable insights from their video files.</span></span> <span data-ttu-id="64c95-108">Mer detaljerad översikt över Azure Media Services Analytics finns [detta](media-services-analytics-overview.md) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="64c95-108">For more detailed overview of Azure Media Services Analytics see [this](media-services-analytics-overview.md) topic.</span></span> 

<span data-ttu-id="64c95-109">Det här avsnittet beskrivs hur tooprocess media med Media Analytics-medieprocessorer (MP) med hjälp av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="64c95-109">This topic discusses how tooprocess your media with Media Analytics media processors (MPs) using hello Azure portal.</span></span> <span data-ttu-id="64c95-110">Media Analytics MP producerar MP4-filer eller JSON-filer.</span><span class="sxs-lookup"><span data-stu-id="64c95-110">Media Analytics MPs produce MP4 files or JSON files.</span></span> <span data-ttu-id="64c95-111">Om en medieprocessor har producerat en MP4-fil kan hämta du progressivt hello-filen.</span><span class="sxs-lookup"><span data-stu-id="64c95-111">If a media processor produced an MP4 file, you can progressively download hello file.</span></span> <span data-ttu-id="64c95-112">Om en medieprocessor har producerat en JSON-fil kan du ladda ned hello filen från hello Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="64c95-112">If a media processor produced a JSON file, you can download hello file from hello Azure blob storage.</span></span> 

## <a name="choose-an-asset-that-you-want-tooanalyze"></a><span data-ttu-id="64c95-113">Välj en tillgång som du vill tooanalyze</span><span class="sxs-lookup"><span data-stu-id="64c95-113">Choose an asset that you want tooanalyze</span></span>
1. <span data-ttu-id="64c95-114">I hello [Azure-portalen](https://portal.azure.com/), Välj Azure Media Services-konto.</span><span class="sxs-lookup"><span data-stu-id="64c95-114">In hello [Azure portal](https://portal.azure.com/), select your Azure Media Services account.</span></span>
2. <span data-ttu-id="64c95-115">I hello **inställningar** väljer **tillgångar**.</span><span class="sxs-lookup"><span data-stu-id="64c95-115">In hello **Settings** window, select **Assets**.</span></span>  
   <span data-ttu-id="64c95-116">.</span><span class="sxs-lookup"><span data-stu-id="64c95-116">.</span></span>
    <span data-ttu-id="64c95-117">![Analysera videor](./media/media-services-portal-analyze/media-services-portal-analyze001.png)</span><span class="sxs-lookup"><span data-stu-id="64c95-117">![Analyze videos](./media/media-services-portal-analyze/media-services-portal-analyze001.png)</span></span>
3. <span data-ttu-id="64c95-118">Välj hello tillgång som du vill att tooanalyze och tryck på hello **analysera** knappen.</span><span class="sxs-lookup"><span data-stu-id="64c95-118">Select hello asset that you would like tooanalyze and press hello **Analyze** button.</span></span>
   
    ![Analysera videor](./media/media-services-portal-analyze/media-services-portal-analyze002.png)
4. <span data-ttu-id="64c95-120">I hello **processen media tillgång med Media Analytics** fönster, Välj hello processor.</span><span class="sxs-lookup"><span data-stu-id="64c95-120">In hello **Process media asset with  Media Analytics** window, select hello processor.</span></span> 
   
    <span data-ttu-id="64c95-121">hello resten av hello artikel som förklarar varför och hur toouse varje processor.</span><span class="sxs-lookup"><span data-stu-id="64c95-121">hello rest of hello article explains why and how toouse each processor.</span></span> 
5. <span data-ttu-id="64c95-122">Tryck på **skapa** toohello starta ett jobb.</span><span class="sxs-lookup"><span data-stu-id="64c95-122">Press **Create** toohello start a job.</span></span>

## <a name="azure-media-indexer"></a><span data-ttu-id="64c95-123">Azure Media Indexer</span><span class="sxs-lookup"><span data-stu-id="64c95-123">Azure Media Indexer</span></span>
<span data-ttu-id="64c95-124">Hej **Azure Media Indexer** medieprocessor kan du toomake mediefiler och innehåll sökbara, samt generera stängd textning spår.</span><span class="sxs-lookup"><span data-stu-id="64c95-124">hello **Azure Media Indexer** media processor enables you toomake media files and content searchable, as well as generate closed captioning tracks.</span></span> <span data-ttu-id="64c95-125">Detta avsnitt ger information om alternativ som du kan ange för denna MP.</span><span class="sxs-lookup"><span data-stu-id="64c95-125">This sections gives some details about options that you can specify for this MP.</span></span>

![Analysera videor](./media/media-services-portal-analyze/media-services-portal-analyze003.png)

### <a name="language"></a><span data-ttu-id="64c95-127">Språk</span><span class="sxs-lookup"><span data-stu-id="64c95-127">Language</span></span>
<span data-ttu-id="64c95-128">hello naturligt språk toobe identifieras i hello fil.</span><span class="sxs-lookup"><span data-stu-id="64c95-128">hello natural language toobe recognized in hello multimedia file.</span></span> <span data-ttu-id="64c95-129">Till exempel engelska och spanska.</span><span class="sxs-lookup"><span data-stu-id="64c95-129">For example, English or Spanish.</span></span> 

### <a name="captions"></a><span data-ttu-id="64c95-130">Etiketter</span><span class="sxs-lookup"><span data-stu-id="64c95-130">Captions</span></span>
<span data-ttu-id="64c95-131">Du kan välja en etikett-format som kommer att genereras från ditt innehåll.</span><span class="sxs-lookup"><span data-stu-id="64c95-131">You can choose a caption format that will be generated from your content.</span></span> <span data-ttu-id="64c95-132">En indexering jobb kan generera textning filer i hello följande format:</span><span class="sxs-lookup"><span data-stu-id="64c95-132">An indexing job can generate closed caption files in hello following formats:</span></span>  

* <span data-ttu-id="64c95-133">**SAMISKA**</span><span class="sxs-lookup"><span data-stu-id="64c95-133">**SAMI**</span></span>
* <span data-ttu-id="64c95-134">**TTML**</span><span class="sxs-lookup"><span data-stu-id="64c95-134">**TTML**</span></span>
* <span data-ttu-id="64c95-135">**WebVTT**</span><span class="sxs-lookup"><span data-stu-id="64c95-135">**WebVTT**</span></span>

<span data-ttu-id="64c95-136">Stängd beskrivning (CC) filer i dessa format kan vara används toomake ljud och video filer tillgänglig toopeople med hörsel funktionshinder.</span><span class="sxs-lookup"><span data-stu-id="64c95-136">Closed Caption (CC) files in these formats can be used toomake audio and video files accessible toopeople with hearing disability.</span></span>

### <a name="aib-file"></a><span data-ttu-id="64c95-137">AIB fil</span><span class="sxs-lookup"><span data-stu-id="64c95-137">AIB file</span></span>
<span data-ttu-id="64c95-138">Välj det här alternativet om du skulle t.ex toogenerate hello ljud Index Blob-fil för användning med hello anpassade IFilter för SQL Server.</span><span class="sxs-lookup"><span data-stu-id="64c95-138">Select this option if you would like toogenerate hello Audio Index Blob file for use with hello custom SQL Server IFilter.</span></span> <span data-ttu-id="64c95-139">Mer information finns i [detta](https://azure.microsoft.com/blog/using-aib-files-with-azure-media-indexer-and-sql-server/) blogg.</span><span class="sxs-lookup"><span data-stu-id="64c95-139">For more information, see [this](https://azure.microsoft.com/blog/using-aib-files-with-azure-media-indexer-and-sql-server/) blog.</span></span>

### <a name="keywords"></a><span data-ttu-id="64c95-140">Nyckelord</span><span class="sxs-lookup"><span data-stu-id="64c95-140">Keywords</span></span>
<span data-ttu-id="64c95-141">Välj det här alternativet om du vill att toogenerate en nyckelord XML-fil.</span><span class="sxs-lookup"><span data-stu-id="64c95-141">Select this option if you would like toogenerate a keywords XML file.</span></span> <span data-ttu-id="64c95-142">Den här filen innehåller nyckelord som extraheras från hello tal innehåll med frekvens och offset-information.</span><span class="sxs-lookup"><span data-stu-id="64c95-142">This file contains keywords extracted from hello speech content, with frequency and offset information.</span></span>

### <a name="job-name"></a><span data-ttu-id="64c95-143">Jobbnamn</span><span class="sxs-lookup"><span data-stu-id="64c95-143">Job name</span></span>
<span data-ttu-id="64c95-144">Ett eget namn som gör att du kan identifiera hello jobb.</span><span class="sxs-lookup"><span data-stu-id="64c95-144">A friendly name that lets you identify hello job.</span></span> <span data-ttu-id="64c95-145">[Detta](media-services-portal-check-job-progress.md) artikeln beskriver hur du övervakar hello förloppet för ett jobb.</span><span class="sxs-lookup"><span data-stu-id="64c95-145">[This](media-services-portal-check-job-progress.md) article describes how you can monitor hello progress of a job.</span></span> 

### <a name="output-file"></a><span data-ttu-id="64c95-146">Utdatafil</span><span class="sxs-lookup"><span data-stu-id="64c95-146">Output file</span></span>
<span data-ttu-id="64c95-147">Ett eget namn som gör att du kan identifiera hello utdata innehåll.</span><span class="sxs-lookup"><span data-stu-id="64c95-147">A friendly name that lets you identify hello output content.</span></span> 

## <a name="azure-media-hyperlapse"></a><span data-ttu-id="64c95-148">Azure Media Hyperlapse</span><span class="sxs-lookup"><span data-stu-id="64c95-148">Azure Media Hyperlapse</span></span>
<span data-ttu-id="64c95-149">Azure Media Hyperlapse är en HP som skapar smooth tid upphörde att gälla videor från första person eller åtgärd kamera innehåll.</span><span class="sxs-lookup"><span data-stu-id="64c95-149">Azure Media Hyperlapse is an MP that creates smooth time-lapsed videos from first-person or action-camera content.</span></span>  <span data-ttu-id="64c95-150">Mer information finns i [detta](media-services-hyperlapse-content.md) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="64c95-150">For more information, see [this](media-services-hyperlapse-content.md) topic.</span></span> <span data-ttu-id="64c95-151">Detta avsnitt ger information om alternativ som du kan ange för denna MP.</span><span class="sxs-lookup"><span data-stu-id="64c95-151">This sections gives some details about options that you can specify for this MP.</span></span>

![Analysera videor](./media/media-services-portal-analyze/media-services-portal-analyze004.png)

### <a name="speed"></a><span data-ttu-id="64c95-153">Hastighet</span><span class="sxs-lookup"><span data-stu-id="64c95-153">Speed</span></span>
<span data-ttu-id="64c95-154">Ange hello hastighet med vilken toospeed in hello indatavideo.</span><span class="sxs-lookup"><span data-stu-id="64c95-154">Specify hello speed with which toospeed up hello input video.</span></span> <span data-ttu-id="64c95-155">hello-utdata är ett stabilt och tid slut återgivning av hello indatavideo.</span><span class="sxs-lookup"><span data-stu-id="64c95-155">hello output is a stabilized and time-lapsed rendition of hello input video.</span></span>

### <a name="job-name"></a><span data-ttu-id="64c95-156">Jobbnamn</span><span class="sxs-lookup"><span data-stu-id="64c95-156">Job name</span></span>
<span data-ttu-id="64c95-157">Ett eget namn som gör att du kan identifiera hello jobb.</span><span class="sxs-lookup"><span data-stu-id="64c95-157">A friendly name that lets you identify hello job.</span></span> <span data-ttu-id="64c95-158">[Detta](media-services-portal-check-job-progress.md) artikeln beskriver hur du övervakar hello förloppet för ett jobb.</span><span class="sxs-lookup"><span data-stu-id="64c95-158">[This](media-services-portal-check-job-progress.md) article describes how you can monitor hello progress of a job.</span></span> 

### <a name="output-file"></a><span data-ttu-id="64c95-159">Utdatafil</span><span class="sxs-lookup"><span data-stu-id="64c95-159">Output file</span></span>
<span data-ttu-id="64c95-160">Ett eget namn som gör att du kan identifiera hello utdata innehåll.</span><span class="sxs-lookup"><span data-stu-id="64c95-160">A friendly name that lets you identify hello output content.</span></span> 

## <a name="azure-media-face-detector"></a><span data-ttu-id="64c95-161">Azure Media Face Detector</span><span class="sxs-lookup"><span data-stu-id="64c95-161">Azure Media Face Detector</span></span>
<span data-ttu-id="64c95-162">Hej **Azure Media ansikte detektor** medieprocessor (HP) kan du toocount, spåra förflyttningar och även mätaren målgruppen deltagande och reaktion via ansikte uttryck.</span><span class="sxs-lookup"><span data-stu-id="64c95-162">hello **Azure Media Face Detector** media processor (MP) enables you toocount, track movements, and even gauge audience participation and reaction via facial expressions.</span></span> <span data-ttu-id="64c95-163">Den här tjänsten innehåller två funktioner:</span><span class="sxs-lookup"><span data-stu-id="64c95-163">This service contains two features:</span></span> 

* <span data-ttu-id="64c95-164">**Ansikts-identifiering**</span><span class="sxs-lookup"><span data-stu-id="64c95-164">**Face detection**</span></span>
  
    <span data-ttu-id="64c95-165">Identifiering av framsidan hittar och spårar mänsklig ytor inom en video.</span><span class="sxs-lookup"><span data-stu-id="64c95-165">Face detection finds and tracks human faces within a video.</span></span> <span data-ttu-id="64c95-166">Flera ytor kan identifieras och spåras senare när de flyttas runt, med hello tid och plats metadata som returneras i en JSON-fil.</span><span class="sxs-lookup"><span data-stu-id="64c95-166">Multiple faces can be detected and subsequently be tracked as they move around, with hello time and location metadata returned in a JSON file.</span></span> <span data-ttu-id="64c95-167">Under spårning försöker toogive en konsekvent ID toohello samma står inför när hello person flyttar på skärmen, även om de hindras eller lämna en kort hello ram.</span><span class="sxs-lookup"><span data-stu-id="64c95-167">During tracking, it will attempt toogive a consistent ID toohello same face while hello person is moving around on screen, even if they are obstructed or briefly leave hello frame.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="64c95-168">Den här tjänster inte att utföra ansiktsigenkänning.</span><span class="sxs-lookup"><span data-stu-id="64c95-168">This services does not perform facial recognition.</span></span> <span data-ttu-id="64c95-169">En person som lämnar hello ram eller blir hindras för länge får ett nytt-ID när de kommer tillbaka.</span><span class="sxs-lookup"><span data-stu-id="64c95-169">An individual who leaves hello frame or becomes obstructed for too long will be given a new ID when they return.</span></span>
  > 
  > 
* <span data-ttu-id="64c95-170">**Känsloigenkänning**</span><span class="sxs-lookup"><span data-stu-id="64c95-170">**Emotion detection**</span></span>
  
    <span data-ttu-id="64c95-171">Känsloigenkänning är en valfri komponent i hello ansikte identifiering Medieprocessor som returnerar analysis på flera känslomässiga attribut från hello ytor upptäckt, inklusive lycka, sadness, fruktan, resulterande och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="64c95-171">Emotion Detection is an optional component of hello Face Detection Media Processor that returns analysis on multiple emotional attributes from hello faces detected, including happiness, sadness, fear, anger, and more.</span></span> 

![Analysera videor](./media/media-services-portal-analyze/media-services-portal-analyze005.png)

### <a name="detection-mode"></a><span data-ttu-id="64c95-173">Identifieringsläget</span><span class="sxs-lookup"><span data-stu-id="64c95-173">Detection mode</span></span>
<span data-ttu-id="64c95-174">Något av följande lägen hello kan användas av hello processor:</span><span class="sxs-lookup"><span data-stu-id="64c95-174">One of hello following modes can be used by hello processor:</span></span>

* <span data-ttu-id="64c95-175">ansikts-identifiering</span><span class="sxs-lookup"><span data-stu-id="64c95-175">face detection</span></span>
* <span data-ttu-id="64c95-176">per ansikte känsloigenkänning</span><span class="sxs-lookup"><span data-stu-id="64c95-176">per face emotion detection</span></span>
* <span data-ttu-id="64c95-177">sammanställd känsloigenkänning</span><span class="sxs-lookup"><span data-stu-id="64c95-177">aggregate emotion detection</span></span>

### <a name="job-name"></a><span data-ttu-id="64c95-178">Jobbnamn</span><span class="sxs-lookup"><span data-stu-id="64c95-178">Job name</span></span>
<span data-ttu-id="64c95-179">Ett eget namn som gör att du kan identifiera hello jobb.</span><span class="sxs-lookup"><span data-stu-id="64c95-179">A friendly name that lets you identify hello job.</span></span> <span data-ttu-id="64c95-180">[Detta](media-services-portal-check-job-progress.md) artikeln beskriver hur du övervakar hello förloppet för ett jobb.</span><span class="sxs-lookup"><span data-stu-id="64c95-180">[This](media-services-portal-check-job-progress.md) article describes how you can monitor hello progress of a job.</span></span> 

### <a name="output-file"></a><span data-ttu-id="64c95-181">Utdatafil</span><span class="sxs-lookup"><span data-stu-id="64c95-181">Output file</span></span>
<span data-ttu-id="64c95-182">Ett eget namn som gör att du kan identifiera hello utdata innehåll.</span><span class="sxs-lookup"><span data-stu-id="64c95-182">A friendly name that lets you identify hello output content.</span></span> 

## <a name="azure-media-motion-detector"></a><span data-ttu-id="64c95-183">Azure Media Motion Detector</span><span class="sxs-lookup"><span data-stu-id="64c95-183">Azure Media Motion Detector</span></span>
<span data-ttu-id="64c95-184">Hej **Azure Media rörelsedetektor** media-processor (HP) aktiverar du tooefficiently identifierar avsnitt av intresse i ett annat långa och primärdomänkontrollant video.</span><span class="sxs-lookup"><span data-stu-id="64c95-184">hello **Azure Media Motion Detector** media processor (MP) enables you tooefficiently identify sections of interest within an otherwise long and uneventful video.</span></span> <span data-ttu-id="64c95-185">Rörelseidentifiering kan användas på statisk kamera tagning tooidentify avsnitt av hello video där rörelse inträffar.</span><span class="sxs-lookup"><span data-stu-id="64c95-185">Motion detection can be used on static camera footage tooidentify sections of hello video where motion occurs.</span></span> <span data-ttu-id="64c95-186">Den genererar en JSON-fil som innehåller en metadata med tidsstämplar och hello avgränsar region där hello händelsen inträffade.</span><span class="sxs-lookup"><span data-stu-id="64c95-186">It generates a JSON file containing a metadata with timestamps and hello bounding region where hello event occurred.</span></span>

<span data-ttu-id="64c95-187">Den här tekniken är riktade mot säkerhet video feeds kan toocategorize rörelse in relevanta händelser och falska positiva identifieringar som skuggor och andra ändringar.</span><span class="sxs-lookup"><span data-stu-id="64c95-187">Targeted towards security video feeds, this technology is able toocategorize motion into relevant events and false positives such as shadows and lighting changes.</span></span> <span data-ttu-id="64c95-188">Detta ger dig toogenerate säkerhetsaviseringar från kameran feeds utan som skräppost med oändlig irrelevanta händelser inte kan tooextract stund intressanta från extremt långa övervakning videor.</span><span class="sxs-lookup"><span data-stu-id="64c95-188">This allows you toogenerate security alerts from camera feeds without being spammed with endless irrelevant events, while being able tooextract moments of interest from extremely long surveillance videos.</span></span>

![Analysera videor](./media/media-services-portal-analyze/media-services-portal-analyze006.png)

## <a name="azure-media-video-thumbnails"></a><span data-ttu-id="64c95-190">Azure Media Video Thumbnails</span><span class="sxs-lookup"><span data-stu-id="64c95-190">Azure Media Video Thumbnails</span></span>
<span data-ttu-id="64c95-191">Med hjälp av den här processorn kan du skapa sammanfattningar av långa videor automatiskt genom att intressanta kodavsnitt från hello källa video.</span><span class="sxs-lookup"><span data-stu-id="64c95-191">This processor can help you create summaries of long videos by automatically selecting interesting snippets from hello source video.</span></span> <span data-ttu-id="64c95-192">Detta är användbart när du vill tooprovide en snabb överblick över vilka tooexpect i en lång video.</span><span class="sxs-lookup"><span data-stu-id="64c95-192">This is useful when you want tooprovide a quick overview of what tooexpect in a long video.</span></span> <span data-ttu-id="64c95-193">Mer detaljerad information och exempel finns [Använd Azure Media Video-miniatyrer tooCreate en Videosammanfattning](media-services-video-summarization.md)</span><span class="sxs-lookup"><span data-stu-id="64c95-193">For detailed information and examples, see [Use Azure Media Video Thumbnails tooCreate a Video Summarization](media-services-video-summarization.md)</span></span>

![Analysera videor](./media/media-services-portal-analyze/media-services-portal-analyze008.png)

### <a name="job-name"></a><span data-ttu-id="64c95-195">Jobbnamn</span><span class="sxs-lookup"><span data-stu-id="64c95-195">Job name</span></span>
<span data-ttu-id="64c95-196">Ett eget namn som gör att du kan identifiera hello jobb.</span><span class="sxs-lookup"><span data-stu-id="64c95-196">A friendly name that lets you identify hello job.</span></span> <span data-ttu-id="64c95-197">[Detta](media-services-portal-check-job-progress.md) artikeln beskriver hur du övervakar hello förloppet för ett jobb.</span><span class="sxs-lookup"><span data-stu-id="64c95-197">[This](media-services-portal-check-job-progress.md) article describes how you can monitor hello progress of a job.</span></span> 

### <a name="output-file"></a><span data-ttu-id="64c95-198">Utdatafil</span><span class="sxs-lookup"><span data-stu-id="64c95-198">Output file</span></span>
<span data-ttu-id="64c95-199">Ett eget namn som gör att du kan identifiera hello utdata innehåll.</span><span class="sxs-lookup"><span data-stu-id="64c95-199">A friendly name that lets you identify hello output content.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="64c95-200">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="64c95-200">Next steps</span></span>
<span data-ttu-id="64c95-201">Visa Media Services utbildningsvägar.</span><span class="sxs-lookup"><span data-stu-id="64c95-201">View Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="64c95-202">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="64c95-202">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

