---
title: "aaaHow tooencode en Azure-tillgång med Media Encoder Standard | Microsoft Docs"
description: "Lär dig hur toouse Media Encoder Standard tooencode media innehåll på Azure Media Services. Kodexempel använda REST API."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 2a7273c6-8a22-4f82-9bfe-4509ff32d4a4
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: juliako
ms.openlocfilehash: b766bafded7ee98eda3e6ef149c31d5d8fe406fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooencode-an-asset-by-using-media-encoder-standard"></a><span data-ttu-id="6c78e-104">Hur tooencode en tillgång med Media Encoder Standard</span><span class="sxs-lookup"><span data-stu-id="6c78e-104">How tooencode an asset by using Media Encoder Standard</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6c78e-105">.NET</span><span class="sxs-lookup"><span data-stu-id="6c78e-105">.NET</span></span>](media-services-dotnet-encode-with-media-encoder-standard.md)
> * [<span data-ttu-id="6c78e-106">REST</span><span class="sxs-lookup"><span data-stu-id="6c78e-106">REST</span></span>](media-services-rest-encode-asset.md)
> * [<span data-ttu-id="6c78e-107">Portal</span><span class="sxs-lookup"><span data-stu-id="6c78e-107">Portal</span></span>](media-services-portal-encode.md)
>
>

## <a name="overview"></a><span data-ttu-id="6c78e-108">Översikt</span><span class="sxs-lookup"><span data-stu-id="6c78e-108">Overview</span></span>
<span data-ttu-id="6c78e-109">toodeliver digital video via hello Internet, måste du komprimera hello media.</span><span class="sxs-lookup"><span data-stu-id="6c78e-109">toodeliver digital video over hello Internet, you must compress hello media.</span></span> <span data-ttu-id="6c78e-110">Digitala videofiler är stort och kan vara för stort toodeliver via hello Internet eller för dina kunders enheter toodisplay korrekt.</span><span class="sxs-lookup"><span data-stu-id="6c78e-110">Digital video files are large and may be too big toodeliver over hello Internet, or for your customers’ devices toodisplay properly.</span></span> <span data-ttu-id="6c78e-111">Kodning är hello process för att komprimera video och ljud så att kunderna kan visa dina media.</span><span class="sxs-lookup"><span data-stu-id="6c78e-111">Encoding is hello process of compressing video and audio so your customers can view your media.</span></span>

<span data-ttu-id="6c78e-112">Kodning jobb är en hello vanligaste bearbetningen i Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="6c78e-112">Encoding jobs are one of hello most common processing operations in Azure Media Services.</span></span> <span data-ttu-id="6c78e-113">Du skapar kodning jobb tooconvert media-filer från en kodning tooanother.</span><span class="sxs-lookup"><span data-stu-id="6c78e-113">You create encoding jobs tooconvert media files from one encoding tooanother.</span></span> <span data-ttu-id="6c78e-114">När du kodar kan du använda hello Media Services inbyggda-kodare (Media Encoder Standard).</span><span class="sxs-lookup"><span data-stu-id="6c78e-114">When you encode, you can use hello Media Services built-in encoder (Media Encoder Standard).</span></span> <span data-ttu-id="6c78e-115">Du kan också använda en kodare som tillhandahålls av Media Services partner.</span><span class="sxs-lookup"><span data-stu-id="6c78e-115">You can also use an encoder provided by a Media Services partner.</span></span> <span data-ttu-id="6c78e-116">Kodare från tredje part är tillgängliga via hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="6c78e-116">Third-party encoders are available through hello Azure Marketplace.</span></span> <span data-ttu-id="6c78e-117">Du kan ange hello information om kodning uppgifter med hjälp av förinställda strängar som definierats för din kodare eller med hjälp av förinställda konfigurationsfiler.</span><span class="sxs-lookup"><span data-stu-id="6c78e-117">You can specify hello details of encoding tasks by using preset strings defined for your encoder, or by using preset configuration files.</span></span> <span data-ttu-id="6c78e-118">toosee hello typer av förinställda som är tillgängliga, se [aktivitet förinställningar för Media Encoder Standard](http://msdn.microsoft.com/library/mt269960).</span><span class="sxs-lookup"><span data-stu-id="6c78e-118">toosee hello types of presets that are available, see [Task Presets for Media Encoder Standard](http://msdn.microsoft.com/library/mt269960).</span></span>

<span data-ttu-id="6c78e-119">Varje jobb kan ha en eller flera aktiviteter beroende på hello typ av bearbetning som du vill tooaccomplish.</span><span class="sxs-lookup"><span data-stu-id="6c78e-119">Each job can have one or more tasks depending on hello type of processing that you want tooaccomplish.</span></span> <span data-ttu-id="6c78e-120">Via hello REST-API, kan du skapa jobb och deras relaterade uppgifter i ett av två sätt:</span><span class="sxs-lookup"><span data-stu-id="6c78e-120">Through hello REST API, you can create jobs and their related tasks in one of two ways:</span></span>

* <span data-ttu-id="6c78e-121">Aktiviteter kan vara definierade infogade via hello uppgifter navigeringsegenskap på jobbet entiteter.</span><span class="sxs-lookup"><span data-stu-id="6c78e-121">Tasks can be defined inline through hello Tasks navigation property on Job entities.</span></span>
* <span data-ttu-id="6c78e-122">Använd OData-batch-bearbetning.</span><span class="sxs-lookup"><span data-stu-id="6c78e-122">Use OData batch processing.</span></span>

<span data-ttu-id="6c78e-123">Vi rekommenderar att du alltid koda källfilerna till en MP4-uppsättningen med anpassad bithastighet och sedan konvertera hello set toohello önskat format med hjälp av [dynamisk paketering](media-services-dynamic-packaging-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6c78e-123">We recommend that you always encode your source files into an adaptive bitrate MP4 set, and then convert hello set toohello desired format by using [dynamic packaging](media-services-dynamic-packaging-overview.md).</span></span>

<span data-ttu-id="6c78e-124">Om din utdatatillgången är lagringskrypterad, måste du konfigurera hello tillgångsleveransprincip.</span><span class="sxs-lookup"><span data-stu-id="6c78e-124">If your output asset is storage encrypted, you must configure hello asset delivery policy.</span></span> <span data-ttu-id="6c78e-125">Mer information finns i [konfigurera tillgångsleveransprincip](media-services-rest-configure-asset-delivery-policy.md).</span><span class="sxs-lookup"><span data-stu-id="6c78e-125">For more information, see [Configuring asset delivery policy](media-services-rest-configure-asset-delivery-policy.md).</span></span>

## <a name="considerations"></a><span data-ttu-id="6c78e-126">Överväganden</span><span class="sxs-lookup"><span data-stu-id="6c78e-126">Considerations</span></span>

<span data-ttu-id="6c78e-127">Vid åtkomst till entiteter i Media Services måste du ange specifika namn på huvudfält och värden i HTTP-begäranden.</span><span class="sxs-lookup"><span data-stu-id="6c78e-127">When accessing entities in Media Services, you must set specific header fields and values in your HTTP requests.</span></span> <span data-ttu-id="6c78e-128">Mer information finns i [installationsprogrammet för Media Services REST API-utveckling](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="6c78e-128">For more information, see [Setup for Media Services REST API Development](media-services-rest-how-to-use.md).</span></span>

<span data-ttu-id="6c78e-129">Innan du börjar refererar till media processorer, kontrollera att du har hello rätt media processor-ID.</span><span class="sxs-lookup"><span data-stu-id="6c78e-129">Before you start referencing media processors, verify that you have hello correct media processor ID.</span></span> <span data-ttu-id="6c78e-130">Mer information finns i [hämta medieprocessorer](media-services-rest-get-media-processor.md).</span><span class="sxs-lookup"><span data-stu-id="6c78e-130">For more information, see [Get media processors](media-services-rest-get-media-processor.md).</span></span>

## <a name="connect-toomedia-services"></a><span data-ttu-id="6c78e-131">Ansluta tooMedia tjänster</span><span class="sxs-lookup"><span data-stu-id="6c78e-131">Connect tooMedia Services</span></span>

<span data-ttu-id="6c78e-132">Mer information om hur tooconnect toohello AMS API, se [åtkomst hello Azure Media Services API med Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="6c78e-132">For information on how tooconnect toohello AMS API, see [Access hello Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

>[!NOTE]
><span data-ttu-id="6c78e-133">När du har anslutit toohttps://media.windows.net, får du en 301 omdirigering att ange en annan Media Services-URI.</span><span class="sxs-lookup"><span data-stu-id="6c78e-133">After successfully connecting toohttps://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="6c78e-134">Du måste göra följande anrop toohello ny URI.</span><span class="sxs-lookup"><span data-stu-id="6c78e-134">You must make subsequent calls toohello new URI.</span></span>

## <a name="create-a-job-with-a-single-encoding-task"></a><span data-ttu-id="6c78e-135">Skapa ett jobb med en enda kodning aktivitet</span><span class="sxs-lookup"><span data-stu-id="6c78e-135">Create a job with a single encoding task</span></span>
> [!NOTE]
> <span data-ttu-id="6c78e-136">När du arbetar med hello Media Services REST API gäller hello följande:</span><span class="sxs-lookup"><span data-stu-id="6c78e-136">When you're working with hello Media Services REST API, hello following considerations apply:</span></span>
>
> <span data-ttu-id="6c78e-137">Vid åtkomst till entiteter i Media Services måste du ange specifika namn på huvudfält och värden i HTTP-begäranden.</span><span class="sxs-lookup"><span data-stu-id="6c78e-137">When accessing entities in Media Services, you must set specific header fields and values in your HTTP requests.</span></span> <span data-ttu-id="6c78e-138">Mer information finns i [installationsprogrammet för Media Services REST API-utveckling](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="6c78e-138">For more information, see [Setup for Media Services REST API development](media-services-rest-how-to-use.md).</span></span>
>
> <span data-ttu-id="6c78e-139">När du har anslutit toohttps://media.windows.net, får du en 301 omdirigering att ange en annan Media Services-URI.</span><span class="sxs-lookup"><span data-stu-id="6c78e-139">After successfully connecting toohttps://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="6c78e-140">Du måste göra följande anrop toohello ny URI.</span><span class="sxs-lookup"><span data-stu-id="6c78e-140">You must make subsequent calls toohello new URI.</span></span> <span data-ttu-id="6c78e-141">Mer information om hur tooconnect toohello AMS API, se [åtkomst hello Azure Media Services API med Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="6c78e-141">For information on how tooconnect toohello AMS API, see [Access hello Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span>
>
> <span data-ttu-id="6c78e-142">När med JSON och ange toouse hello **__metadata** nyckelord i hello-begäran (till exempel tooreferences ett länkat kvar), måste du ange hello **acceptera** huvud för[utförlig JSON-format ](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/): Acceptera: application/json; odata = utförlig.</span><span class="sxs-lookup"><span data-stu-id="6c78e-142">When using JSON and specifying toouse hello **__metadata** keyword in hello request (for example, tooreferences a linked object), you must set hello **Accept** header too[JSON Verbose format](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/): Accept: application/json;odata=verbose.</span></span>
>
>

<span data-ttu-id="6c78e-143">hello som följande exempel visar hur toocreate och efter ett jobb med en uppgift ange tooencode en video på en viss upplösning och kvalitet.</span><span class="sxs-lookup"><span data-stu-id="6c78e-143">hello following example shows you how toocreate and post a job with one task set tooencode a video at a specific resolution and quality.</span></span> <span data-ttu-id="6c78e-144">När du kodar med Media Encoder Standard kan du använda aktiviteten configuration förinställningar anges [här](http://msdn.microsoft.com/library/mt269960).</span><span class="sxs-lookup"><span data-stu-id="6c78e-144">When you encode with Media Encoder Standard, you can use task configuration presets specified [here](http://msdn.microsoft.com/library/mt269960).</span></span>

<span data-ttu-id="6c78e-145">Begäran:</span><span class="sxs-lookup"><span data-stu-id="6c78e-145">Request:</span></span>

    POST https://media.windows.net/API/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    Host: media.windows.net

    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Aaab7f15b-3136-4ddf-9962-e9ecb28fb9d2')"}}],  "Tasks" : [{"Configuration" : "Adaptive Streaming", "MediaProcessorId" : "nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",  "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"}]}

<span data-ttu-id="6c78e-146">Svar:</span><span class="sxs-lookup"><span data-stu-id="6c78e-146">Response:</span></span>

    HTTP/1.1 201 Created

    . . .

### <a name="set-hello-output-assets-name"></a><span data-ttu-id="6c78e-147">Ange namnet på hello utdata tillgångsinformation</span><span class="sxs-lookup"><span data-stu-id="6c78e-147">Set hello output asset's name</span></span>
<span data-ttu-id="6c78e-148">hello som följande exempel visar hur tooset hello assetName attribut:</span><span class="sxs-lookup"><span data-stu-id="6c78e-148">hello following example shows how tooset hello assetName attribute:</span></span>

    { "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"CustomOutputAssetName\">JobOutputAsset(0)</outputAsset></taskBody>"}

## <a name="considerations"></a><span data-ttu-id="6c78e-149">Överväganden</span><span class="sxs-lookup"><span data-stu-id="6c78e-149">Considerations</span></span>
* <span data-ttu-id="6c78e-150">TaskBody egenskaper måste använda literal XML toodefine hello antal indata eller utdata tillgångar som används av hello-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="6c78e-150">TaskBody properties must use literal XML toodefine hello number of input, or output assets that are used by hello task.</span></span> <span data-ttu-id="6c78e-151">hello avsnittet innehåller hello XML Schema Definition för hello XML.</span><span class="sxs-lookup"><span data-stu-id="6c78e-151">hello task topic contains hello XML Schema Definition for hello XML.</span></span>
* <span data-ttu-id="6c78e-152">I hello TaskBody definition, varje inre värde för <inputAsset> och <outputAsset> måste anges som JobInputAsset(value) eller JobOutputAsset(value).</span><span class="sxs-lookup"><span data-stu-id="6c78e-152">In hello TaskBody definition, each inner value for <inputAsset> and <outputAsset> must be set as JobInputAsset(value) or JobOutputAsset(value).</span></span>
* <span data-ttu-id="6c78e-153">En aktivitet kan innehålla flera utdata tillgångar.</span><span class="sxs-lookup"><span data-stu-id="6c78e-153">A task can have multiple output assets.</span></span> <span data-ttu-id="6c78e-154">En JobOutputAsset(x) kan bara användas en gång som utdata för en aktivitet i ett jobb.</span><span class="sxs-lookup"><span data-stu-id="6c78e-154">One JobOutputAsset(x) can only be used once as an output of a task in a job.</span></span>
* <span data-ttu-id="6c78e-155">Du kan ange JobInputAsset eller JobOutputAsset som inkommande tillgång för en aktivitet.</span><span class="sxs-lookup"><span data-stu-id="6c78e-155">You can specify JobInputAsset or JobOutputAsset as an input asset of a task.</span></span>
* <span data-ttu-id="6c78e-156">Aktiviteter måste inte utgör en cykel.</span><span class="sxs-lookup"><span data-stu-id="6c78e-156">Tasks must not form a cycle.</span></span>
* <span data-ttu-id="6c78e-157">Parametern för hello-värde som du skickar tooJobInputAsset eller JobOutputAsset representerar hello indexvärdet för en tillgång.</span><span class="sxs-lookup"><span data-stu-id="6c78e-157">hello value parameter that you pass tooJobInputAsset or JobOutputAsset represents hello index value for an asset.</span></span> <span data-ttu-id="6c78e-158">hello faktiska tillgångar har definierats i hello InputMediaAssets och OutputMediaAssets navigeringsegenskaper på hello jobbdefinitionen entitet.</span><span class="sxs-lookup"><span data-stu-id="6c78e-158">hello actual assets are defined in hello InputMediaAssets and OutputMediaAssets navigation properties on hello job entity definition.</span></span>
* <span data-ttu-id="6c78e-159">Eftersom Media Services bygger på OData v3 hello enskilda tillgångar i hello InputMediaAssets och OutputMediaAssets navigering egenskapsuppsättningar refereras via en ”__metadata: uri” namn / värde-par.</span><span class="sxs-lookup"><span data-stu-id="6c78e-159">Because Media Services is built on OData v3, hello individual assets in hello InputMediaAssets and OutputMediaAssets navigation property collections are referenced through a "__metadata : uri" name-value pair.</span></span>
* <span data-ttu-id="6c78e-160">InputMediaAssets mappar tooone eller flera resurser som du skapade i Media Services.</span><span class="sxs-lookup"><span data-stu-id="6c78e-160">InputMediaAssets maps tooone or more assets that you created in Media Services.</span></span> <span data-ttu-id="6c78e-161">OutputMediaAssets skapas med hello system.</span><span class="sxs-lookup"><span data-stu-id="6c78e-161">OutputMediaAssets are created by hello system.</span></span> <span data-ttu-id="6c78e-162">De refererar inte till en befintlig tillgång.</span><span class="sxs-lookup"><span data-stu-id="6c78e-162">They don't reference an existing asset.</span></span>
* <span data-ttu-id="6c78e-163">OutputMediaAssets kan namnges genom att använda hello assetName attributet.</span><span class="sxs-lookup"><span data-stu-id="6c78e-163">OutputMediaAssets can be named by using hello assetName attribute.</span></span> <span data-ttu-id="6c78e-164">Om det här attributet är inte tillgängligt och sedan hello hello OutputMediaAsset heter det inre textvärdet hello hello <outputAsset> elementet är med suffixet hello jobbnamn värde eller hello jobb-Id-värde (i hello fall där hello namnegenskapen har inte definierats).</span><span class="sxs-lookup"><span data-stu-id="6c78e-164">If this attribute is not present, then hello name of hello OutputMediaAsset is whatever hello inner text value of hello <outputAsset> element is with a suffix of either hello Job Name value, or hello Job Id value (in hello case where hello Name property isn't defined).</span></span> <span data-ttu-id="6c78e-165">Till exempel om du anger ett värde för assetName Sample för ”Sample” och sedan hello OutputMediaAsset Name-egenskapen anges för ””.</span><span class="sxs-lookup"><span data-stu-id="6c78e-165">For example, if you set a value for assetName too"Sample," then hello OutputMediaAsset Name property is set too"Sample."</span></span> <span data-ttu-id="6c78e-166">Om du inte ange ett värde för assetName men ställts in hello jobbnamn skulle för ”NewJob” och sedan hello OutputMediaAsset namn dock vara ”JobOutputAsset (värde) _NewJob”.</span><span class="sxs-lookup"><span data-stu-id="6c78e-166">However, if you didn't set a value for assetName, but did set hello job name too"NewJob," then hello OutputMediaAsset Name would be "JobOutputAsset(value)_NewJob."</span></span>

## <a name="create-a-job-with-chained-tasks"></a><span data-ttu-id="6c78e-167">Skapa ett jobb med länkad uppgifter</span><span class="sxs-lookup"><span data-stu-id="6c78e-167">Create a job with chained tasks</span></span>
<span data-ttu-id="6c78e-168">I många Programscenarier vill utvecklare toocreate en serie bearbetningsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="6c78e-168">In many application scenarios, developers want toocreate a series of processing tasks.</span></span> <span data-ttu-id="6c78e-169">Du kan skapa en serie länkad aktiviteter i Media Services.</span><span class="sxs-lookup"><span data-stu-id="6c78e-169">In Media Services, you can create a series of chained tasks.</span></span> <span data-ttu-id="6c78e-170">Varje aktivitet utför steg för behandling och kan använda olika media processorer.</span><span class="sxs-lookup"><span data-stu-id="6c78e-170">Each task performs different processing steps and can use different media processors.</span></span> <span data-ttu-id="6c78e-171">Hej sammankedjade uppgifter kan lämnar in en tillgång från en aktivitet tooanother, utför en linjär aktivitetssekvensen på hello tillgången.</span><span class="sxs-lookup"><span data-stu-id="6c78e-171">hello chained tasks can hand off an asset from one task tooanother, performing a linear sequence of tasks on hello asset.</span></span> <span data-ttu-id="6c78e-172">Hello-uppgifter som utförs i ett jobb är dock inte obligatoriskt toobe i en sekvens.</span><span class="sxs-lookup"><span data-stu-id="6c78e-172">However, hello tasks performed in a job are not required toobe in a sequence.</span></span> <span data-ttu-id="6c78e-173">När du skapar en länkad aktivitet hello sammankedjade **ITask** objekt som skapas i en enda **IJob** objekt.</span><span class="sxs-lookup"><span data-stu-id="6c78e-173">When you create a chained task, hello chained **ITask** objects are created in a single **IJob** object.</span></span>

> [!NOTE]
> <span data-ttu-id="6c78e-174">Det finns en gräns på 30 uppgifter per jobb.</span><span class="sxs-lookup"><span data-stu-id="6c78e-174">There is currently a limit of 30 tasks per job.</span></span> <span data-ttu-id="6c78e-175">Om du behöver toochain mer än 30 uppgifter kan du skapa mer än ett jobb toocontain hello uppgifter.</span><span class="sxs-lookup"><span data-stu-id="6c78e-175">If you need toochain more than 30 tasks, create more than one job toocontain hello tasks.</span></span>
>
>

    POST https://media.windows.net/api/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000

    {  
       "Name":"NewTestJob",
       "InputMediaAssets":[  
          {  
             "__metadata":{  
                "uri":"https://testrest.cloudapp.net/api/Assets('nb%3Acid%3AUUID%3A910ffdc1-2e25-4b17-8a42-61ffd4b8914c')"
             }
          }
       ],
       "Tasks":[  
          {  
             "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"
          },
          {  
             "Configuration":"H264 Smooth Streaming 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-16\"?><taskBody><inputAsset>JobOutputAsset(0)</inputAsset><outputAsset>JobOutputAsset(1)</outputAsset></taskBody>"
          }
       ]
    }


### <a name="considerations"></a><span data-ttu-id="6c78e-176">Överväganden</span><span class="sxs-lookup"><span data-stu-id="6c78e-176">Considerations</span></span>
<span data-ttu-id="6c78e-177">tooenable länkning för uppgiften:</span><span class="sxs-lookup"><span data-stu-id="6c78e-177">tooenable task chaining:</span></span>

* <span data-ttu-id="6c78e-178">Ett jobb måste ha minst två aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="6c78e-178">A job must have at least two tasks.</span></span>
* <span data-ttu-id="6c78e-179">Det måste finnas minst en aktivitet vars indata är hello utdata från en annan aktivitet i hello-jobbet.</span><span class="sxs-lookup"><span data-stu-id="6c78e-179">There must be at least one task whose input is hello output of another task in hello job.</span></span>

## <a name="use-odata-batch-processing"></a><span data-ttu-id="6c78e-180">Använd OData-batch-bearbetning</span><span class="sxs-lookup"><span data-stu-id="6c78e-180">Use OData batch processing</span></span>
<span data-ttu-id="6c78e-181">hello som följande exempel visar hur toouse OData batch bearbetning toocreate ett jobb och uppgifter.</span><span class="sxs-lookup"><span data-stu-id="6c78e-181">hello following example shows how toouse OData batch processing toocreate a job and tasks.</span></span> <span data-ttu-id="6c78e-182">Mer information om batchbearbetning finns [Open Data Protocol (OData) gruppbearbetning](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).</span><span class="sxs-lookup"><span data-stu-id="6c78e-182">For information on batch processing, see [Open Data Protocol (OData) Batch Processing](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).</span></span>

    POST https://media.windows.net/api/$batch HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Content-Type: multipart/mixed; boundary=batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e
    Accept: multipart/mixed
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    Host: media.windows.net


    --batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e
    Content-Type: multipart/mixed; boundary=changeset_122fb0a4-cd80-4958-820f-346309967e4d

    --changeset_122fb0a4-cd80-4958-820f-346309967e4d
    Content-Type: application/http
    Content-Transfer-Encoding: binary

    POST https://media.windows.net/api/Jobs HTTP/1.1
    Content-ID: 1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000

    {"Name" : "NewTestJob", "InputMediaAssets@odata.bind":["https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A2a22445d-1500-80c6-4b34-f1e5190d33c6')"]}

    --changeset_122fb0a4-cd80-4958-820f-346309967e4d
    Content-Type: application/http
    Content-Transfer-Encoding: binary

    POST https://media.windows.net/api/$1/Tasks HTTP/1.1
    Content-ID: 2
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000

    {  
       "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
       "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
       "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"Custom output name\">JobOutputAsset(0)</outputAsset></taskBody>"
    }

    --changeset_122fb0a4-cd80-4958-820f-346309967e4d--
    --batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e--



## <a name="create-a-job-by-using-a-jobtemplate"></a><span data-ttu-id="6c78e-183">Skapa ett jobb med hjälp av en jobbmall</span><span class="sxs-lookup"><span data-stu-id="6c78e-183">Create a job by using a JobTemplate</span></span>
<span data-ttu-id="6c78e-184">När du bearbetar flera resurser med hjälp av en gemensam uppsättning aktiviteter, Använd en jobbmall toospecify hello standarduppgiften förinställningar eller tooset hello ordningen för aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="6c78e-184">When you process multiple assets by using a common set of tasks, use a JobTemplate toospecify hello default task presets, or tooset hello order of tasks.</span></span>

<span data-ttu-id="6c78e-185">hello som följande exempel visar hur toocreate en jobbmall med en TaskTemplate är infogade datadefinitioner.</span><span class="sxs-lookup"><span data-stu-id="6c78e-185">hello following example shows how toocreate a JobTemplate with a TaskTemplate that is defined inline.</span></span> <span data-ttu-id="6c78e-186">Hej TaskTemplate använder hello Media Encoder Standard som hello MediaProcessor tooencode hello resursfil.</span><span class="sxs-lookup"><span data-stu-id="6c78e-186">hello TaskTemplate uses hello Media Encoder Standard as hello MediaProcessor tooencode hello asset file.</span></span> <span data-ttu-id="6c78e-187">Dock kan andra MediaProcessors också användas.</span><span class="sxs-lookup"><span data-stu-id="6c78e-187">However, other MediaProcessors can be used as well.</span></span>

    POST https://media.windows.net/API/JobTemplates HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    Host: media.windows.net


    {"Name" : "NewJobTemplate25", "JobTemplateBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><jobTemplate><taskBody taskTemplateId=\"nb:ttid:UUID:071370A3-E63E-4E81-A099-AD66BCAC3789\"><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody></jobTemplate>", "TaskTemplates" : [{"Id" : "nb:ttid:UUID:071370A3-E63E-4E81-A099-AD66BCAC3789", "Configuration" : "H264 Smooth Streaming 720p", "MediaProcessorId" : "nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56", "Name" : "SampleTaskTemplate2", "NumberofInputAssets" : 1, "NumberofOutputAssets" : 1}] }


> [!NOTE]
> <span data-ttu-id="6c78e-188">Till skillnad från andra Media Services-enheter måste du definiera en ny GUID-identifierare för varje TaskTemplate och placera det i hello taskTemplateId och Id-egenskap i frågans brödtext.</span><span class="sxs-lookup"><span data-stu-id="6c78e-188">Unlike other Media Services entities, you must define a new GUID identifier for each TaskTemplate and place it in hello taskTemplateId and Id property in your request body.</span></span> <span data-ttu-id="6c78e-189">hello innehåll identifieringsschemat måste följa hello schemat som beskrivs i identifiera Azure Media Services entiteter.</span><span class="sxs-lookup"><span data-stu-id="6c78e-189">hello content identification scheme must follow hello scheme described in Identify Azure Media Services Entities.</span></span> <span data-ttu-id="6c78e-190">JobTemplates kan inte uppdateras.</span><span class="sxs-lookup"><span data-stu-id="6c78e-190">Also, JobTemplates cannot be updated.</span></span> <span data-ttu-id="6c78e-191">I stället måste du skapa en ny med ändringarna uppdaterade.</span><span class="sxs-lookup"><span data-stu-id="6c78e-191">Instead, you must create a new one with your updated changes.</span></span>
>
>

<span data-ttu-id="6c78e-192">Om det lyckas, returneras hello efter svar:</span><span class="sxs-lookup"><span data-stu-id="6c78e-192">If successful, hello following response is returned:</span></span>

    HTTP/1.1 201 Created

    . . .


<span data-ttu-id="6c78e-193">följande exempel visar hur hello toocreate ett jobb som refererar till en jobbmall-Id:</span><span class="sxs-lookup"><span data-stu-id="6c78e-193">hello following example shows how toocreate a job that references a JobTemplate Id:</span></span>

    POST https://media.windows.net/API/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    Host: media.windows.net


    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A3f1fe4a2-68f5-4190-9557-cd45beccef92')"}}], "TemplateId" : "nb:jtid:UUID:15e6e5e6-ac85-084e-9dc2-db3645fbf0aa"}


<span data-ttu-id="6c78e-194">Om det lyckas, returneras hello efter svar:</span><span class="sxs-lookup"><span data-stu-id="6c78e-194">If successful, hello following response is returned:</span></span>

    HTTP/1.1 201 Created

    . . .



## <a name="media-services-learning-paths"></a><span data-ttu-id="6c78e-195">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="6c78e-195">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="6c78e-196">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="6c78e-196">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a><span data-ttu-id="6c78e-197">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6c78e-197">Next steps</span></span>
<span data-ttu-id="6c78e-198">Nu när du vet hur toocreate ett jobb tooencode en tillgång, se [hur toocheck jobb pågår med Media Services](media-services-rest-check-job-progress.md).</span><span class="sxs-lookup"><span data-stu-id="6c78e-198">Now that you know how toocreate a job tooencode an asset, see [How toocheck job progress with Media Services](media-services-rest-check-job-progress.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="6c78e-199">Se även</span><span class="sxs-lookup"><span data-stu-id="6c78e-199">See also</span></span>
[<span data-ttu-id="6c78e-200">Hämta Media-processorer</span><span class="sxs-lookup"><span data-stu-id="6c78e-200">Get Media Processors</span></span>](media-services-rest-get-media-processor.md)
