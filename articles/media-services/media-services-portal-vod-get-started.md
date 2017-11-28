---
title: "Komma igång med VoD med hjälp av Azure Portal | Microsoft Docs"
description: "De här självstudierna visar dig stegen för att implementera ett grundläggande leveransprogram för Video-on-Demand-innehåll (VoD) med Azure Media Services-appen (AMS) med hjälp av Azure Portal."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 6c98fcfa-39e6-43a5-83a5-d4954788f8a4
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/07/2017
ms.author: juliako
ms.openlocfilehash: a8eeeeff412837acba14b441a3c590edf7e3597a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-delivering-content-on-demand-using-the-azure-portal"></a><span data-ttu-id="76be2-103">Komma igång med att leverera innehåll på begäran med hjälp av Azure Portal</span><span class="sxs-lookup"><span data-stu-id="76be2-103">Get started with delivering content on demand using the Azure portal</span></span>
[!INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

<span data-ttu-id="76be2-104">De här självstudierna visar dig stegen för att implementera ett grundläggande leveransprogram för Video-on-Demand-innehåll (VoD) med Azure Media Services-appen (AMS) med hjälp av Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="76be2-104">This tutorial walks you through the steps of implementing a basic Video-on-Demand (VoD) content delivery service with Azure Media Services (AMS) application using the Azure portal.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="76be2-105">Krav</span><span class="sxs-lookup"><span data-stu-id="76be2-105">Prerequisites</span></span>
<span data-ttu-id="76be2-106">Följande krävs för att kunna genomföra vägledningen:</span><span class="sxs-lookup"><span data-stu-id="76be2-106">The following are required to complete the tutorial:</span></span>

* <span data-ttu-id="76be2-107">Ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="76be2-107">An Azure account.</span></span> <span data-ttu-id="76be2-108">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="76be2-108">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
* <span data-ttu-id="76be2-109">Ett Media Services-konto.</span><span class="sxs-lookup"><span data-stu-id="76be2-109">A Media Services account.</span></span> <span data-ttu-id="76be2-110">Information om hur du skapar ett Media Services-konto finns i [Så här skapar du ett Media Services-konto](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="76be2-110">To create a Media Services account, see [How to Create a Media Services Account](media-services-portal-create-account.md).</span></span>

<span data-ttu-id="76be2-111">Vägledningen innehåller följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="76be2-111">This tutorial includes the following tasks:</span></span>

1. <span data-ttu-id="76be2-112">Starta slutpunkt för direktuppspelning.</span><span class="sxs-lookup"><span data-stu-id="76be2-112">Start streaming endpoint.</span></span>
2. <span data-ttu-id="76be2-113">Överföra en videofil.</span><span class="sxs-lookup"><span data-stu-id="76be2-113">Upload a video file.</span></span>
3. <span data-ttu-id="76be2-114">Koda källfilen till en uppsättning MP4-filer med anpassningsbar bithastighet.</span><span class="sxs-lookup"><span data-stu-id="76be2-114">Encode the source file into a set of adaptive bitrate MP4 files.</span></span>
4. <span data-ttu-id="76be2-115">Publicera tillgången och få URL:er för strömning och progressiv överföring.</span><span class="sxs-lookup"><span data-stu-id="76be2-115">Publish the asset and get streaming and progressive download URLs.</span></span>  
5. <span data-ttu-id="76be2-116">Spela upp ditt innehåll.</span><span class="sxs-lookup"><span data-stu-id="76be2-116">Play your content.</span></span>

## <a name="start-streaming-endpoints"></a><span data-ttu-id="76be2-117">Starta slutpunkter för direktuppspelning</span><span class="sxs-lookup"><span data-stu-id="76be2-117">Start streaming endpoints</span></span> 

<span data-ttu-id="76be2-118">När du arbetar med Azure Media Services är ett av de vanligaste scenarierna att leverera video via direktuppspelning med anpassningsbar bithastighet.</span><span class="sxs-lookup"><span data-stu-id="76be2-118">When working with Azure Media Services one of the most common scenarios is delivering video via adaptive bitrate streaming.</span></span> <span data-ttu-id="76be2-119">Media Services tillhandahåller en dynamisk paketering som gör att du kan leverera ditt MP4-kodade innehåll med anpassningsbar bithastighet i direktuppspelningsformat som stöds av Media Services (MPEG DASH, HLS, Smooth Streaming) direkt när du så önskar, utan att du behöver lagra på förhand paketerade versioner av vart och ett av dessa direktuppspelningsformat.</span><span class="sxs-lookup"><span data-stu-id="76be2-119">Media Services provides dynamic packaging, which allows you to deliver your adaptive bitrate MP4 encoded content in streaming formats supported by Media Services (MPEG DASH, HLS, Smooth Streaming) just-in-time, without you having to store pre-packaged versions of each of these streaming formats.</span></span>

>[!NOTE]
><span data-ttu-id="76be2-120">När ditt AMS-konto skapas läggs en **standard**-slutpunkt för direktuppspelning till på ditt konto med tillståndet **Stoppad**.</span><span class="sxs-lookup"><span data-stu-id="76be2-120">When your AMS account is created a **default** streaming endpoint is added to your account in the **Stopped** state.</span></span> <span data-ttu-id="76be2-121">Om du vill starta direktuppspelning av innehåll och dra nytta av dynamisk paketering och dynamisk kryptering måste slutpunkten för direktuppspelning som du vill spela upp innehåll från ha tillståndet **Körs**.</span><span class="sxs-lookup"><span data-stu-id="76be2-121">To start streaming your content and take advantage of dynamic packaging and dynamic encryption, the streaming endpoint from which you want to stream content has to be in the **Running** state.</span></span> 

<span data-ttu-id="76be2-122">Starta slutpunkten för direktuppspelning genom att göra följande:</span><span class="sxs-lookup"><span data-stu-id="76be2-122">To start the streaming endpoint, do the following:</span></span>

1. <span data-ttu-id="76be2-123">Logga in på [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="76be2-123">Log in at the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="76be2-124">I fönstret Inställningar klickar du på Slutpunkter för direktuppspelning.</span><span class="sxs-lookup"><span data-stu-id="76be2-124">In the Settings window, click Streaming endpoints.</span></span> 
3. <span data-ttu-id="76be2-125">Klicka på den slutpunkt för direktuppspelning som är standard.</span><span class="sxs-lookup"><span data-stu-id="76be2-125">Click the default streaming endpoint.</span></span> 

    <span data-ttu-id="76be2-126">Fönstret INFORMATION OM DEN SLUTPUNKT FÖR DIREKTUPPSPELNING SOM ÄR STANDARD visas.</span><span class="sxs-lookup"><span data-stu-id="76be2-126">The DEFAULT STREAMING ENDPOINT DETAILS window appears.</span></span>

4. <span data-ttu-id="76be2-127">Klicka på ikonen Start.</span><span class="sxs-lookup"><span data-stu-id="76be2-127">Click the Start icon.</span></span>
5. <span data-ttu-id="76be2-128">Klicka på knappen Spara för att spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="76be2-128">Click the Save button to save your changes.</span></span>

## <a name="upload-files"></a><span data-ttu-id="76be2-129">Överföra filer</span><span class="sxs-lookup"><span data-stu-id="76be2-129">Upload files</span></span>
<span data-ttu-id="76be2-130">För att strömma videor med Azure Media Services behöver du överföra källvideorna, koda dem till flera olika bithastigheter och publicera resultatet.</span><span class="sxs-lookup"><span data-stu-id="76be2-130">To stream videos using Azure Media Services, you need to upload the source videos, encode them into multiple bitrates, and publish the result.</span></span> <span data-ttu-id="76be2-131">I det här avsnittet beskrivs det första steget.</span><span class="sxs-lookup"><span data-stu-id="76be2-131">The first step is covered in this section.</span></span> 

1. <span data-ttu-id="76be2-132">I fönstret **Inställning** klickar du på **Tillgångar**.</span><span class="sxs-lookup"><span data-stu-id="76be2-132">In the **Setting** window, click **Assets**.</span></span>
   
    ![Överföra filer](./media/media-services-portal-vod-get-started/media-services-upload.png)
2. <span data-ttu-id="76be2-134">Klicka på knappen **Överför**.</span><span class="sxs-lookup"><span data-stu-id="76be2-134">Click the **Upload** button.</span></span>
   
    <span data-ttu-id="76be2-135">Fönstret **Överför en videotillgång** visas.</span><span class="sxs-lookup"><span data-stu-id="76be2-135">The **Upload a video asset** window appears.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="76be2-136">Det finns inga filstorleksbegränsningar.</span><span class="sxs-lookup"><span data-stu-id="76be2-136">There is no file size limitation.</span></span>
   > 
   > 
3. <span data-ttu-id="76be2-137">Bläddra till den önskade videon på datorn, markera den och tryck på OK.</span><span class="sxs-lookup"><span data-stu-id="76be2-137">Browse to the desired video on your computer, select it, and hit OK.</span></span>  
   
    <span data-ttu-id="76be2-138">Överföringen startar och du kan följa förloppet under filnamnet.</span><span class="sxs-lookup"><span data-stu-id="76be2-138">The upload starts and you can see the progress under the file name.</span></span>  

<span data-ttu-id="76be2-139">När överföringen är klar visas den nya tillgången i listan **Tillgångar**.</span><span class="sxs-lookup"><span data-stu-id="76be2-139">Once the upload completes, you see the new asset listed in the **Assets** window.</span></span> 

## <a name="encode-assets"></a><span data-ttu-id="76be2-140">Koda tillgångar</span><span class="sxs-lookup"><span data-stu-id="76be2-140">Encode assets</span></span>

<span data-ttu-id="76be2-141">När du arbetar med Azure Media Services är ett av de vanligaste scenarierna att leverera strömning med anpassad bithastighet till dina klienter.</span><span class="sxs-lookup"><span data-stu-id="76be2-141">When working with Azure Media Services one of the most common scenarios is delivering adaptive bitrate streaming to your clients.</span></span> <span data-ttu-id="76be2-142">Media Services har stöd för följande strömningstekniker med anpassningsbar bithastighet: HTTP-liveuppspelning (HLS), jämn direktuppspelning, MPEG DASH.</span><span class="sxs-lookup"><span data-stu-id="76be2-142">Media Services supports the following adaptive bitrate streaming technologies: HTTP Live Streaming (HLS), Smooth Streaming, MPEG DASH.</span></span> <span data-ttu-id="76be2-143">För att förbereda dina videor för strömning med anpassad bithastighet måste du koda källvideon till filer i multibithastighet.</span><span class="sxs-lookup"><span data-stu-id="76be2-143">To prepare your videos for adaptive bitrate streaming, you need to encode your source video into multi-bitrate files.</span></span> <span data-ttu-id="76be2-144">Du bör använda kodaren **Media Encoder Standard** för att koda dina videor.</span><span class="sxs-lookup"><span data-stu-id="76be2-144">You should use the **Media Encoder Standard** encoder to encode your videos.</span></span>  

<span data-ttu-id="76be2-145">Media Services tillhandahåller också en dynamisk paketering som gör att du kan leverera dina MP4-filer med flera bithastigheter i följande strömningsformat: MPEG DASH, HLS eller jämn direktuppspelning utan att du behöver packa om till dessa strömningsformat.</span><span class="sxs-lookup"><span data-stu-id="76be2-145">Media Services also provides dynamic packaging, which allows you to deliver your multi-bitrate MP4s in the following streaming formats: MPEG DASH, HLS, Smooth Streaming, without you having to repackage into these streaming formats.</span></span> <span data-ttu-id="76be2-146">Med dynamisk paketering behöver du bara lagra och betala för filerna i ett enda lagringsformat, och Media Services skapar och ger lämplig respons baserat på begäranden från en klient.</span><span class="sxs-lookup"><span data-stu-id="76be2-146">With dynamic packaging, you only need to store and pay for the files in single storage format and Media Services builds and serves the appropriate response based on requests from a client.</span></span>

<span data-ttu-id="76be2-147">Om du vill dra nytta av dynamisk paketering måste du koda din källfil till en uppsättning MP4-filer med flera bithastigheter (kodningsstegen visas längre fram i det här avsnittet).</span><span class="sxs-lookup"><span data-stu-id="76be2-147">To take advantage of dynamic packaging, you need to encode your source file into a set of multi-bitrate MP4 files (the encoding steps are demonstrated later in this section).</span></span>

### <a name="to-use-the-portal-to-encode"></a><span data-ttu-id="76be2-148">Använda portalen för att koda</span><span class="sxs-lookup"><span data-stu-id="76be2-148">To use the portal to encode</span></span>
<span data-ttu-id="76be2-149">I det här avsnittet beskrivs de steg som du kan vidta för att koda ditt innehåll med Media Encoder Standard.</span><span class="sxs-lookup"><span data-stu-id="76be2-149">This section describes the steps you can take to encode your content with Media Encoder Standard.</span></span>

1. <span data-ttu-id="76be2-150">I fönstret **Inställningar** väljer du **Tillgångar**.</span><span class="sxs-lookup"><span data-stu-id="76be2-150">In the **Settings** window, select **Assets**.</span></span>  
2. <span data-ttu-id="76be2-151">I fönstret **Tillgångar** väljer du den tillgång som du vill koda.</span><span class="sxs-lookup"><span data-stu-id="76be2-151">In the **Assets** window, select the asset that you would like to encode.</span></span>
3. <span data-ttu-id="76be2-152">Tryck på knappen **Koda**.</span><span class="sxs-lookup"><span data-stu-id="76be2-152">Press the **Encode** button.</span></span>
4. <span data-ttu-id="76be2-153">I fönstret **Koda en tillgång** väljer du processorn ”Media Encoder Standard” och en förinställning.</span><span class="sxs-lookup"><span data-stu-id="76be2-153">In the **Encode an asset** window, select the "Media Encoder Standard" processor and a preset.</span></span> <span data-ttu-id="76be2-154">Information om förinställningar finns i [auto-generate a bitrate ladder](media-services-autogen-bitrate-ladder-with-mes.md) (autogenerera en bithastighetsstege) och [Task Presets for MES](media-services-mes-presets-overview.md) (Uppgiftsförinställningar för MES).</span><span class="sxs-lookup"><span data-stu-id="76be2-154">For information about presets, see [auto-generate a bitrate ladder](media-services-autogen-bitrate-ladder-with-mes.md) and [Task Presets for MES](media-services-mes-presets-overview.md).</span></span> <span data-ttu-id="76be2-155">Om du planerar att styra vilka kodningsförinställningar, kom ihåg att det är viktigt att välja den förinställning som är mest lämplig för din indatavideo.</span><span class="sxs-lookup"><span data-stu-id="76be2-155">If you plan to control which encoding preset is used, keep this in mind: it is important to select the preset that is most appropriate for your input video.</span></span> <span data-ttu-id="76be2-156">Om du till exempel vet att din indatavideo har en upplösning på 1 920 x 1 080 bildpunkter, kan du använda förinställningen ”H264 multibithastighet 1080p”.</span><span class="sxs-lookup"><span data-stu-id="76be2-156">For example, if you know your input video has a resolution of 1920x1080 pixels, then you could use the "H264 Multiple Bitrate 1080p" preset.</span></span> <span data-ttu-id="76be2-157">Om du har en video med låg upplösning (640 x 360) bör du inte använda förinställningen ”H264 multibithastighet 1080p”.</span><span class="sxs-lookup"><span data-stu-id="76be2-157">If you have a low resolution (640x360) video, then you should not be using "H264 Multiple Bitrate 1080p" preset.</span></span>
   
   <span data-ttu-id="76be2-158">Du kan redigera namnet på utdatatillgången och namnet på jobbet för enklare hantering.</span><span class="sxs-lookup"><span data-stu-id="76be2-158">For easier management, you have an option of editing the name of the output asset, and the name of the job.</span></span>
   
   ![Koda tillgångar](./media/media-services-portal-vod-get-started/media-services-encode1.png)
5. <span data-ttu-id="76be2-160">Tryck på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="76be2-160">Press **Create**.</span></span>

### <a name="monitor-encoding-job-progress"></a><span data-ttu-id="76be2-161">Övervaka förloppet för kodningsjobb</span><span class="sxs-lookup"><span data-stu-id="76be2-161">Monitor encoding job progress</span></span>
<span data-ttu-id="76be2-162">Klicka på **Inställningar** (överst på sidan) för att övervaka förloppet för kodningsjobbet och välj sedan **Jobb**.</span><span class="sxs-lookup"><span data-stu-id="76be2-162">To monitor the progress of the encoding job, click **Settings** (at the top of the page) and then select **Jobs**.</span></span>

![Jobb](./media/media-services-portal-vod-get-started/media-services-jobs.png)

## <a name="publish-content"></a><span data-ttu-id="76be2-164">Publicera innehåll</span><span class="sxs-lookup"><span data-stu-id="76be2-164">Publish content</span></span>
<span data-ttu-id="76be2-165">För att ge din användare en URL som kan användas för att strömma eller hämta ditt innehåll måste du först ”publicera” din tillgång genom att skapa en lokaliserare.</span><span class="sxs-lookup"><span data-stu-id="76be2-165">To provide your user with a  URL that can be used to stream or download your content, you first need to "publish" your asset by creating a locator.</span></span> <span data-ttu-id="76be2-166">Lokaliserare ger åtkomst till filer som finns i tillgången.</span><span class="sxs-lookup"><span data-stu-id="76be2-166">Locators provide access to files contained in the asset.</span></span> <span data-ttu-id="76be2-167">Media Services stöder två typer av lokaliserare:</span><span class="sxs-lookup"><span data-stu-id="76be2-167">Media Services supports two types of locators:</span></span> 

* <span data-ttu-id="76be2-168">Strömningslokaliserare (OnDemandOrigin), som används för anpassad strömning (till exempel för strömning av MPEG DASH, HLS och Smooth Streaming).</span><span class="sxs-lookup"><span data-stu-id="76be2-168">Streaming (OnDemandOrigin) locators, used for adaptive streaming (for example, to stream MPEG DASH, HLS, or Smooth Streaming).</span></span> <span data-ttu-id="76be2-169">Om du vill skapa en strömningslokaliserare måste din tillgång innehålla en .ism-fil.</span><span class="sxs-lookup"><span data-stu-id="76be2-169">To create a streaming locator your asset must contain an .ism file.</span></span> 
* <span data-ttu-id="76be2-170">Progressiva SAS-lokaliserare, som används för leverans av video via progressiv hämtning.</span><span class="sxs-lookup"><span data-stu-id="76be2-170">Progressive (SAS) locators, used for delivery of video via progressive download.</span></span>

<span data-ttu-id="76be2-171">En strömnings-URL har följande format och du kan använda det för att spela upp Smooth Streaming-tillgångar.</span><span class="sxs-lookup"><span data-stu-id="76be2-171">A streaming URL has the following format and you can use it to play Smooth Streaming assets.</span></span>

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

<span data-ttu-id="76be2-172">Lägg till (format = m3u8 aapl) till URL:en för att skapa en HLS-strömnings-URL.</span><span class="sxs-lookup"><span data-stu-id="76be2-172">To build an HLS streaming URL, append (format=m3u8-aapl) to the URL.</span></span>

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

<span data-ttu-id="76be2-173">Lägg till (format=mpd-time-csf) till URL:en för att skapa en MPEG DASH-strömnings-URL.</span><span class="sxs-lookup"><span data-stu-id="76be2-173">To build an  MPEG DASH streaming URL, append (format=mpd-time-csf) to the URL.</span></span>

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


<span data-ttu-id="76be2-174">En SAS-URL har följande format.</span><span class="sxs-lookup"><span data-stu-id="76be2-174">A SAS URL has the following format.</span></span>

    {blob container name}/{asset name}/{file name}/{SAS signature}

> [!NOTE]
> <span data-ttu-id="76be2-175">Om du har använt portalen för att skapa lokaliserare före mars 2015, skapades lokaliserare med ett utgångsdatum två år senare.</span><span class="sxs-lookup"><span data-stu-id="76be2-175">If you used the portal to create locators before March 2015, locators with a two-year expiration date were created.</span></span>  
> 
> 

<span data-ttu-id="76be2-176">Du uppdaterar ett utgångsdatum för en lokaliserare med [REST](https://docs.microsoft.com/rest/api/media/operations/locator#update_a_locator)- eller [.NET](http://go.microsoft.com/fwlink/?LinkID=533259)-API:er.</span><span class="sxs-lookup"><span data-stu-id="76be2-176">To update an expiration date on a locator, use [REST](https://docs.microsoft.com/rest/api/media/operations/locator#update_a_locator) or [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) APIs.</span></span> <span data-ttu-id="76be2-177">URL:en ändras när du uppdaterar en SAS-lokaliserare.</span><span class="sxs-lookup"><span data-stu-id="76be2-177">When you update the expiration date of a SAS locator, the URL changes.</span></span>

### <a name="to-use-the-portal-to-publish-an-asset"></a><span data-ttu-id="76be2-178">Använda portalen för att publicera en tillgång</span><span class="sxs-lookup"><span data-stu-id="76be2-178">To use the portal to publish an asset</span></span>
<span data-ttu-id="76be2-179">Gör följande för att använda portalen för att publicera en tillgång:</span><span class="sxs-lookup"><span data-stu-id="76be2-179">To use the portal to publish an asset, do the following:</span></span>

1. <span data-ttu-id="76be2-180">Välj **Inställningar** > **Tillgångar**.</span><span class="sxs-lookup"><span data-stu-id="76be2-180">Select **Settings** > **Assets**.</span></span>
2. <span data-ttu-id="76be2-181">Välj den tillgång som du vill publicera.</span><span class="sxs-lookup"><span data-stu-id="76be2-181">Select the asset that you want to publish.</span></span>
3. <span data-ttu-id="76be2-182">Klicka sedan på knappen **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="76be2-182">Click the **Publish** button.</span></span>
4. <span data-ttu-id="76be2-183">Välj typ av lokaliserare.</span><span class="sxs-lookup"><span data-stu-id="76be2-183">Select the locator type.</span></span>
5. <span data-ttu-id="76be2-184">Tryck på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="76be2-184">Press **Add**.</span></span>
   
    ![Publicera](./media/media-services-portal-vod-get-started/media-services-publish1.png)

<span data-ttu-id="76be2-186">URL:en läggs till i listan över **publicerade URL:er**.</span><span class="sxs-lookup"><span data-stu-id="76be2-186">The URL is added to the list of **Published URLs**.</span></span>

## <a name="play-content-from-the-portal"></a><span data-ttu-id="76be2-187">Spela upp innehåll från portalen</span><span class="sxs-lookup"><span data-stu-id="76be2-187">Play content from the portal</span></span>
<span data-ttu-id="76be2-188">Azure Portal har en innehållsspelare som du kan använda för att testa videon.</span><span class="sxs-lookup"><span data-stu-id="76be2-188">The Azure portal provides a content player that you can use to test your video.</span></span>

<span data-ttu-id="76be2-189">Klicka på önskad video och klicka sedan på knappen **Spela upp**.</span><span class="sxs-lookup"><span data-stu-id="76be2-189">Click the desired video and then click the **Play** button.</span></span>

![Publicera](./media/media-services-portal-vod-get-started/media-services-play.png)

<span data-ttu-id="76be2-191">Vissa förutsättningar gäller:</span><span class="sxs-lookup"><span data-stu-id="76be2-191">Some considerations apply:</span></span>

* <span data-ttu-id="76be2-192">Starta direktuppspelningen genom att börja köra **standard**slutpunkten för direktuppspelning.</span><span class="sxs-lookup"><span data-stu-id="76be2-192">To begin streaming, start running the **default** streaming endpoint.</span></span>
* <span data-ttu-id="76be2-193">Kontrollera att videon har publicerats.</span><span class="sxs-lookup"><span data-stu-id="76be2-193">Make sure the video has been published.</span></span>
* <span data-ttu-id="76be2-194">Denna**Media Player** spelar upp från den strömningsslutpunkt som är standard.</span><span class="sxs-lookup"><span data-stu-id="76be2-194">This **Media player** plays from the default streaming endpoint.</span></span> <span data-ttu-id="76be2-195">Klicka för att kopiera URL:en och använd en annan spelare om du vill spela upp från en strömningsslutpunkt som inte är standard.</span><span class="sxs-lookup"><span data-stu-id="76be2-195">If you want to play from a non-default streaming endpoint, click to copy the URL and use another player.</span></span> <span data-ttu-id="76be2-196">Till exempel [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span><span class="sxs-lookup"><span data-stu-id="76be2-196">For example, [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span></span>

## <a name="next-steps"></a><span data-ttu-id="76be2-197">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="76be2-197">Next steps</span></span>
<span data-ttu-id="76be2-198">Granska sökvägarna för Media Services-utbildning.</span><span class="sxs-lookup"><span data-stu-id="76be2-198">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="76be2-199">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="76be2-199">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

