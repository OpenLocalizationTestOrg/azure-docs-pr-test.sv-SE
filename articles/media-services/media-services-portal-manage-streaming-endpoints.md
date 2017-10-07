---
title: "aaaManage strömningsslutpunkter med hello Azure-portalen | Microsoft Docs"
description: "Det här avsnittet visar hur toomanage strömningsslutpunkter med hello Azure-portalen."
services: media-services
documentationcenter: 
author: Juliako
writer: juliako
manager: cfowler
editor: 
ms.assetid: bb1aca25-d23a-4520-8c45-44ef3ecd5371
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako
ms.openlocfilehash: dfa9352894d37edb317a6334d7f109419deb362b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-streaming-endpoints-with-hello-azure-portal"></a><span data-ttu-id="cf44b-103">Hantera strömningsslutpunkter med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="cf44b-103">Manage streaming endpoints with hello Azure portal</span></span>

<span data-ttu-id="cf44b-104">Det här avsnittet visar hur toouse hello Azure portal toomanage strömningsslutpunkter.</span><span class="sxs-lookup"><span data-stu-id="cf44b-104">This topic shows  how toouse hello Azure portal toomanage streaming endpoints.</span></span> 

>[!NOTE]
><span data-ttu-id="cf44b-105">Se till att tooreview hello [översikt](media-services-streaming-endpoints-overview.md) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="cf44b-105">Make sure tooreview hello [overview](media-services-streaming-endpoints-overview.md) topic.</span></span> 

<span data-ttu-id="cf44b-106">Information om hur tooscale hello strömmande slutpunkten finns [detta](media-services-portal-scale-streaming-endpoints.md) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="cf44b-106">For information about how tooscale hello streaming endpoint, see [this](media-services-portal-scale-streaming-endpoints.md) topic.</span></span>

## <a name="start-managing-streaming-endpoints"></a><span data-ttu-id="cf44b-107">Börja hantera strömningsslutpunkter</span><span class="sxs-lookup"><span data-stu-id="cf44b-107">Start managing streaming endpoints</span></span> 

<span data-ttu-id="cf44b-108">toostart hantera strömningsslutpunkter för ditt konto hello följande.</span><span class="sxs-lookup"><span data-stu-id="cf44b-108">toostart managing streaming endpoints for your account, do hello following.</span></span>

1. <span data-ttu-id="cf44b-109">I hello [Azure-portalen](https://portal.azure.com/), Välj Azure Media Services-konto.</span><span class="sxs-lookup"><span data-stu-id="cf44b-109">In hello [Azure portal](https://portal.azure.com/), select your Azure Media Services account.</span></span>
2. <span data-ttu-id="cf44b-110">I hello **inställningar** bladet väljer **Strömningsslutpunkter**.</span><span class="sxs-lookup"><span data-stu-id="cf44b-110">In hello **Settings** blade, select **Streaming endpoints**.</span></span>
   
    ![Strömmande slutpunkt](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints1.png)

> [!NOTE]
> <span data-ttu-id="cf44b-112">Du debiteras endast när din Strömningsslutpunkt är i körningstillstånd.</span><span class="sxs-lookup"><span data-stu-id="cf44b-112">You are only billed when your Streaming Endpoint is in running state.</span></span>

## <a name="adddelete-a-streaming-endpoint"></a><span data-ttu-id="cf44b-113">Lägg till/ta bort en strömmande slutpunkt</span><span class="sxs-lookup"><span data-stu-id="cf44b-113">Add/delete a streaming endpoint</span></span>

>[!NOTE]
><span data-ttu-id="cf44b-114">hello standard strömmande slutpunkten kan inte tas bort.</span><span class="sxs-lookup"><span data-stu-id="cf44b-114">hello default streaming endpoint cannot be deleted.</span></span>

<span data-ttu-id="cf44b-115">tooadd/ta bort strömmande slutpunkten med hello Azure-portalen, hello följande:</span><span class="sxs-lookup"><span data-stu-id="cf44b-115">tooadd/delete streaming endpoint using hello Azure portal, do hello following:</span></span>

1. <span data-ttu-id="cf44b-116">tooadd strömningsslutpunkt, klicka på hello **+ Endpoint** hello överst på hello sidan.</span><span class="sxs-lookup"><span data-stu-id="cf44b-116">tooadd a streaming endpoint, click hello **+ Endpoint** at hello top of hello page.</span></span> 

    <span data-ttu-id="cf44b-117">Du kanske vill flera Strömningsslutpunkter om du planerar toohave olika CDN-nät eller en CDN och direktåtkomst.</span><span class="sxs-lookup"><span data-stu-id="cf44b-117">You might want multiple Streaming Endpoints if you plan toohave different CDNs or a CDN and direct access.</span></span>

2. <span data-ttu-id="cf44b-118">toodelete strömningsslutpunkt, tryck på **ta bort** knappen.</span><span class="sxs-lookup"><span data-stu-id="cf44b-118">toodelete a streaming endpoint, press **Delete** button.</span></span>      
3. <span data-ttu-id="cf44b-119">Klicka på hello **starta** knappen toostart hello strömmande slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="cf44b-119">Click hello **Start** button toostart hello streaming endpoint.</span></span>
   
    ![Strömmande slutpunkt](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints2.png)


## <span data-ttu-id="cf44b-121"><a id="configure_streaming_endpoints"></a>Konfigurera hello Streaming slutpunkt</span><span class="sxs-lookup"><span data-stu-id="cf44b-121"><a id="configure_streaming_endpoints"></a>Configuring hello Streaming Endpoint</span></span>
<span data-ttu-id="cf44b-122">Strömmande slutpunkten kan du tooconfigure hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="cf44b-122">Streaming Endpoint enables you tooconfigure hello following properties:</span></span>

* <span data-ttu-id="cf44b-123">Åtkomstkontroll</span><span class="sxs-lookup"><span data-stu-id="cf44b-123">Access control</span></span>
* <span data-ttu-id="cf44b-124">Cache-kontroll</span><span class="sxs-lookup"><span data-stu-id="cf44b-124">Cache control</span></span>
* <span data-ttu-id="cf44b-125">Mellan åtkomstprinciper för platsen</span><span class="sxs-lookup"><span data-stu-id="cf44b-125">Cross site access policies</span></span>

<span data-ttu-id="cf44b-126">Detaljerad information om de här egenskaperna finns [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint).</span><span class="sxs-lookup"><span data-stu-id="cf44b-126">For detailed information about these properties, see [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint).</span></span>

<span data-ttu-id="cf44b-127">Du kan konfigurera strömmande slutpunkt hello följande:</span><span class="sxs-lookup"><span data-stu-id="cf44b-127">You can configure streaming endpoint by doing hello following:</span></span>

1. <span data-ttu-id="cf44b-128">Välj hello strömningsslutpunkt som du vill tooconfigure.</span><span class="sxs-lookup"><span data-stu-id="cf44b-128">Select hello streaming endpoint you want tooconfigure.</span></span>
2. <span data-ttu-id="cf44b-129">Klicka på **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="cf44b-129">Click **Settings**.</span></span>

<span data-ttu-id="cf44b-130">En kort beskrivning av hello fält följer.</span><span class="sxs-lookup"><span data-stu-id="cf44b-130">A brief description of hello fields follows.</span></span>

![Strömmande slutpunkt](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints4.png)

1. <span data-ttu-id="cf44b-132">Maximal cachestorlek princip: använda tooconfigure cache-livstid för tillgångar hanteras via den här strömmande slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="cf44b-132">Maximum cache policy: used tooconfigure cache lifetime for assets served through this streaming endpoint.</span></span> <span data-ttu-id="cf44b-133">Om inget värde anges används standard-hello.</span><span class="sxs-lookup"><span data-stu-id="cf44b-133">If no value is set, hello default is used.</span></span> <span data-ttu-id="cf44b-134">hello standardvärdena kan också definieras direkt i Azure-lagring.</span><span class="sxs-lookup"><span data-stu-id="cf44b-134">hello default values can also be defined directly in Azure storage.</span></span> <span data-ttu-id="cf44b-135">Om Azure CDN har aktiverats för hello strömmande slutpunkten, bör du inte ange hello cache princip värdet tooless än 600 sekunder.</span><span class="sxs-lookup"><span data-stu-id="cf44b-135">If Azure CDN is enabled for hello streaming endpoint, you should not set hello cache policy value tooless than 600 seconds.</span></span>  
2. <span data-ttu-id="cf44b-136">Tillåtna IP-adresser: används toospecify IP-adresser som skulle vara tillåten tooconnect toohello publicerade strömmande slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="cf44b-136">Allowed IP addresses: used toospecify IP addresses that would be allowed tooconnect toohello published streaming endpoint.</span></span> <span data-ttu-id="cf44b-137">Om inga angivna IP-adresser är IP-adresser kan tooconnect.</span><span class="sxs-lookup"><span data-stu-id="cf44b-137">If no IP addresses specified, any IP address would be able tooconnect.</span></span> <span data-ttu-id="cf44b-138">IP-adresser kan anges som en IP-adress (t.ex, 10.0.0.1), en IP-intervall med IP-adress och nätmask CIDR (till exempel 10.0.0.1/22) eller en IP-intervall med IP-adress och en prickad decimal nätmask (till exempel ”10.0.0.1 () 255.255.255.0)').</span><span class="sxs-lookup"><span data-stu-id="cf44b-138">IP addresses can be specified as either a single IP address (for example, '10.0.0.1'), an IP range using an IP address and a CIDR subnet mask (for example, '10.0.0.1/22'), or an IP range using IP address and a dotted decimal subnet mask (for example, '10.0.0.1(255.255.255.0)').</span></span>
3. <span data-ttu-id="cf44b-139">Konfiguration för autentisering med Akamai signatur-huvud: används toospecify hur signatur sidhuvud autentiseringsbegäran från Akamai servrar har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="cf44b-139">Configuration for Akamai signature header authentication: used toospecify how signature header authentication request from Akamai servers is configured.</span></span> <span data-ttu-id="cf44b-140">Förfallodatum är i UTC.</span><span class="sxs-lookup"><span data-stu-id="cf44b-140">Expiration is in UTC.</span></span>

## <a name="scale-your-premium-streaming-endpoint"></a><span data-ttu-id="cf44b-141">Skala din Premium strömmande slutpunkten</span><span class="sxs-lookup"><span data-stu-id="cf44b-141">Scale your Premium streaming endpoint</span></span>

<span data-ttu-id="cf44b-142">Mer information finns i [detta](media-services-portal-scale-streaming-endpoints.md) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="cf44b-142">For more information, see [this](media-services-portal-scale-streaming-endpoints.md) topic.</span></span>

## <span data-ttu-id="cf44b-143"><a id="enable_cdn"></a>Aktivera Azure CDN-integrering</span><span class="sxs-lookup"><span data-stu-id="cf44b-143"><a id="enable_cdn"></a>Enable Azure CDN integration</span></span>

<span data-ttu-id="cf44b-144">När du skapar ett nytt konto är standard strömmande slutpunkten Azure CDN-integrering aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="cf44b-144">When you create a new account, default Streaming Endpoint Azure CDN integration is enabled by default.</span></span>

<span data-ttu-id="cf44b-145">Om du senare vill toodisable/aktivera hello CDN strömmande slutpunkten måste vara i hello **stoppats** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="cf44b-145">If you later want toodisable/enable hello CDN, your streaming endpoint must be in hello **stopped** state.</span></span> <span data-ttu-id="cf44b-146">Det kan ta upp too2 timmar för hello Azure CDN integration tooget aktiverad och hello ändringar toobe active över alla hello CDN POP.</span><span class="sxs-lookup"><span data-stu-id="cf44b-146">It could take up too2 hours for hello Azure CDN integration tooget enabled and for hello changes toobe active across all hello CDN POPs.</span></span> <span data-ttu-id="cf44b-147">Dock kan starta strömmande slutpunkt och dataströmmen utan avbrott från hello strömmande slutpunkten och när hello integrationen är slutförd, hello dataströmmen levereras från hello CDN.</span><span class="sxs-lookup"><span data-stu-id="cf44b-147">However, your can start your streaming endpoint and stream without interruptions from hello streaming endpoint and once hello integration is complete, hello stream will be delivered from hello CDN.</span></span> <span data-ttu-id="cf44b-148">Under hello etablering period inte din strömmande slutpunkt i **startar** tillstånd och du kan se degredad prestanda.</span><span class="sxs-lookup"><span data-stu-id="cf44b-148">During hello provisioning period your streaming endpoint will be in **starting** state and you might observe degredad performance.</span></span>

<span data-ttu-id="cf44b-149">CDN-integrering är aktiverat i alla hello Azure data Datacenter execpt Kina och Federal Government regioner.</span><span class="sxs-lookup"><span data-stu-id="cf44b-149">CDN integration is enabled in all hello Azure data centers execpt China and Federal Goverment regions.</span></span>

<span data-ttu-id="cf44b-150">När den har aktiverats hello **åtkomstkontroll**, **anpassad hostname** och **autentisering med Akamai signatur** konfigurationen hämtar inaktiverad.</span><span class="sxs-lookup"><span data-stu-id="cf44b-150">Once it is enabled, hello **Access Control**, **Custom hostname** and **Akamai Signature authentication** configuration gets disabled.</span></span>
 
> [!IMPORTANT]
> <span data-ttu-id="cf44b-151">Azure Media Services-integrering med Azure CDN implementeras på **Azure CDN från Verizon** standard strömningsslutpunkter.</span><span class="sxs-lookup"><span data-stu-id="cf44b-151">Azure Media Services integration with Azure CDN is implemented on **Azure CDN from Verizon** for standard streaming endpoints.</span></span> <span data-ttu-id="cf44b-152">Premium strömningsslutpunkter kan konfigureras med hjälp av alla **Azure CDN priser nivåer och providers**.</span><span class="sxs-lookup"><span data-stu-id="cf44b-152">Premium streaming endpoints can be configured using all **Azure CDN pricing tiers and providers**.</span></span> <span data-ttu-id="cf44b-153">Mer information om Azure CDN funktioner finns hello [CDN-översikt](../cdn/cdn-overview.md).</span><span class="sxs-lookup"><span data-stu-id="cf44b-153">For more information about Azure CDN features, see hello [CDN overview](../cdn/cdn-overview.md).</span></span>
 
### <a name="additional-considerations"></a><span data-ttu-id="cf44b-154">Annat som är bra att tänka på</span><span class="sxs-lookup"><span data-stu-id="cf44b-154">Additional considerations</span></span>

* <span data-ttu-id="cf44b-155">När CDN är aktiverad för en strömmande slutpunkten, det går inte att klienter begär innehåll direkt från hello ursprung.</span><span class="sxs-lookup"><span data-stu-id="cf44b-155">When CDN is enabled for a streaming endpoint, clients cannot request content directly from hello origin.</span></span> <span data-ttu-id="cf44b-156">Om du behöver hello möjlighet tootest ditt innehåll med eller utan CDN kan skapa du en annan strömningsslutpunkt som inte är aktiverat CDN.</span><span class="sxs-lookup"><span data-stu-id="cf44b-156">If you need hello ability tootest your content with or without CDN, you can create another streaming endpoint that isn't CDN enabled.</span></span>
* <span data-ttu-id="cf44b-157">Din strömmande slutpunkten hostname förblir hello samma när du har aktiverat CDN.</span><span class="sxs-lookup"><span data-stu-id="cf44b-157">Your streaming endpoint hostname remains hello same after enabling CDN.</span></span> <span data-ttu-id="cf44b-158">Du behöver inte toomake eventuella ändringar tooyour media services-arbetsflödet efter CDN har aktiverats.</span><span class="sxs-lookup"><span data-stu-id="cf44b-158">You don’t need toomake any changes tooyour media services workflow after CDN is enabled.</span></span> <span data-ttu-id="cf44b-159">Om din strömmande slutpunktens värdnamn är strasbourg.streaming.mediaservices.windows.net, efter att aktivera CDN, används till exempel hello exakt samma värdnamn.</span><span class="sxs-lookup"><span data-stu-id="cf44b-159">For example, if your streaming endpoint hostname is strasbourg.streaming.mediaservices.windows.net, after enabling CDN, hello exact same hostname is used.</span></span>
* <span data-ttu-id="cf44b-160">För nya strömningsslutpunkter kan du aktivera CDN genom att skapa en ny slutpunkt; Du måste toofirst stoppa hello slutpunkt och sedan aktivera/inaktivera hello CDN för befintliga strömningsslutpunkter.</span><span class="sxs-lookup"><span data-stu-id="cf44b-160">For new streaming endpoints, you can enable CDN simply by creating a new endpoint; for existing streaming endpoints, you need toofirst stop hello endpoint and then enable/disable hello CDN.</span></span>
* <span data-ttu-id="cf44b-161">Standard strömmande slutpunkten kan bara konfigureras med hjälp av **Verizon Standard CDN-providern** med hjälp av Azure-hanteringsportalen.</span><span class="sxs-lookup"><span data-stu-id="cf44b-161">Standard streaming endpoint can only be configured using **Verizon Standard CDN provider** using Azure management portal.</span></span> <span data-ttu-id="cf44b-162">Du kan dock aktivera andra leverantörer av Azure CDN med hjälp av REST API: er.</span><span class="sxs-lookup"><span data-stu-id="cf44b-162">However, you can enable other Azure CDN providers using REST APIs.</span></span>

## <a name="configure-cdn-profile"></a><span data-ttu-id="cf44b-163">Konfigurera CDN-profilen</span><span class="sxs-lookup"><span data-stu-id="cf44b-163">Configure CDN profile</span></span>

<span data-ttu-id="cf44b-164">Du kan konfigurera hello CDN-profilen genom att välja hello **hantera CDN** knappen hello uppifrån.</span><span class="sxs-lookup"><span data-stu-id="cf44b-164">You can configure hello CDN profile by selecting hello **Manage CDN** button from hello top.</span></span>

![Strömmande slutpunkt](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints6.png)

## <a name="next-steps"></a><span data-ttu-id="cf44b-166">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cf44b-166">Next steps</span></span>
<span data-ttu-id="cf44b-167">Granska sökvägarna för Media Services-utbildning.</span><span class="sxs-lookup"><span data-stu-id="cf44b-167">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="cf44b-168">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="cf44b-168">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

