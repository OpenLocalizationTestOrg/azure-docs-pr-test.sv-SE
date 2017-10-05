---
title: "Hantera strömningsslutpunkter med Azure-portalen | Microsoft Docs"
description: "Det här avsnittet visar hur du hanterar strömningsslutpunkter med Azure-portalen."
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
ms.openlocfilehash: 797dced6c3e2525730afa29987259cb9b435ba66
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="manage-streaming-endpoints-with-the-azure-portal"></a><span data-ttu-id="efc62-103">Hantera strömningsslutpunkter med Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="efc62-103">Manage streaming endpoints with the Azure portal</span></span>

<span data-ttu-id="efc62-104">Det här avsnittet visar hur du använder Azure-portalen för att hantera strömningsslutpunkter.</span><span class="sxs-lookup"><span data-stu-id="efc62-104">This topic shows  how to use the Azure portal to manage streaming endpoints.</span></span> 

>[!NOTE]
><span data-ttu-id="efc62-105">Se till att granska den [översikt](media-services-streaming-endpoints-overview.md) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="efc62-105">Make sure to review the [overview](media-services-streaming-endpoints-overview.md) topic.</span></span> 

<span data-ttu-id="efc62-106">Information om hur du skalar den strömmande slutpunkten finns [detta](media-services-portal-scale-streaming-endpoints.md) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="efc62-106">For information about how to scale the streaming endpoint, see [this](media-services-portal-scale-streaming-endpoints.md) topic.</span></span>

## <a name="start-managing-streaming-endpoints"></a><span data-ttu-id="efc62-107">Börja hantera strömningsslutpunkter</span><span class="sxs-lookup"><span data-stu-id="efc62-107">Start managing streaming endpoints</span></span> 

<span data-ttu-id="efc62-108">Gör följande om du vill börja hantera strömningsslutpunkter för ditt konto.</span><span class="sxs-lookup"><span data-stu-id="efc62-108">To start managing streaming endpoints for your account, do the following.</span></span>

1. <span data-ttu-id="efc62-109">Välj ditt Azure Media Services-konto i [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="efc62-109">In the [Azure portal](https://portal.azure.com/), select your Azure Media Services account.</span></span>
2. <span data-ttu-id="efc62-110">I den **inställningar** bladet väljer **Strömningsslutpunkter**.</span><span class="sxs-lookup"><span data-stu-id="efc62-110">In the **Settings** blade, select **Streaming endpoints**.</span></span>
   
    ![Strömmande slutpunkt](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints1.png)

> [!NOTE]
> <span data-ttu-id="efc62-112">Du debiteras endast när din Strömningsslutpunkt är i körningstillstånd.</span><span class="sxs-lookup"><span data-stu-id="efc62-112">You are only billed when your Streaming Endpoint is in running state.</span></span>

## <a name="adddelete-a-streaming-endpoint"></a><span data-ttu-id="efc62-113">Lägg till/ta bort en strömmande slutpunkt</span><span class="sxs-lookup"><span data-stu-id="efc62-113">Add/delete a streaming endpoint</span></span>

>[!NOTE]
><span data-ttu-id="efc62-114">Standard strömmande slutpunkten kan inte tas bort.</span><span class="sxs-lookup"><span data-stu-id="efc62-114">The default streaming endpoint cannot be deleted.</span></span>

<span data-ttu-id="efc62-115">Om du vill lägga till/ta bort strömmande slutpunkt med hjälp av Azure portal, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="efc62-115">To add/delete streaming endpoint using the Azure portal, do the following:</span></span>

1. <span data-ttu-id="efc62-116">Lägg till en strömningsslutpunkt, klicka på den **+ Endpoint** överst på sidan.</span><span class="sxs-lookup"><span data-stu-id="efc62-116">To add a streaming endpoint, click the **+ Endpoint** at the top of the page.</span></span> 

    <span data-ttu-id="efc62-117">Om du planerar att ha olika CDN-nät eller en CDN och direktåtkomst kanske du vill flera Strömningsslutpunkter.</span><span class="sxs-lookup"><span data-stu-id="efc62-117">You might want multiple Streaming Endpoints if you plan to have different CDNs or a CDN and direct access.</span></span>

2. <span data-ttu-id="efc62-118">Ta bort en strömmande slutpunkt, trycka på **ta bort** knappen.</span><span class="sxs-lookup"><span data-stu-id="efc62-118">To delete a streaming endpoint, press **Delete** button.</span></span>      
3. <span data-ttu-id="efc62-119">Klicka på den **starta** för att starta den strömmande slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="efc62-119">Click the **Start** button to start the streaming endpoint.</span></span>
   
    ![Strömmande slutpunkt](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints2.png)


## <span data-ttu-id="efc62-121"><a id="configure_streaming_endpoints"></a>Konfigurera strömmande slutpunkt</span><span class="sxs-lookup"><span data-stu-id="efc62-121"><a id="configure_streaming_endpoints"></a>Configuring the Streaming Endpoint</span></span>
<span data-ttu-id="efc62-122">Strömmande slutpunkten kan du konfigurera följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="efc62-122">Streaming Endpoint enables you to configure the following properties:</span></span>

* <span data-ttu-id="efc62-123">Åtkomstkontroll</span><span class="sxs-lookup"><span data-stu-id="efc62-123">Access control</span></span>
* <span data-ttu-id="efc62-124">Cache-kontroll</span><span class="sxs-lookup"><span data-stu-id="efc62-124">Cache control</span></span>
* <span data-ttu-id="efc62-125">Mellan åtkomstprinciper för platsen</span><span class="sxs-lookup"><span data-stu-id="efc62-125">Cross site access policies</span></span>

<span data-ttu-id="efc62-126">Detaljerad information om de här egenskaperna finns [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint).</span><span class="sxs-lookup"><span data-stu-id="efc62-126">For detailed information about these properties, see [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint).</span></span>

<span data-ttu-id="efc62-127">Du kan konfigurera strömningsslutpunkt genom att göra följande:</span><span class="sxs-lookup"><span data-stu-id="efc62-127">You can configure streaming endpoint by doing the following:</span></span>

1. <span data-ttu-id="efc62-128">Välj den strömningsslutpunkt som du vill konfigurera.</span><span class="sxs-lookup"><span data-stu-id="efc62-128">Select the streaming endpoint you want to configure.</span></span>
2. <span data-ttu-id="efc62-129">Klicka på **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="efc62-129">Click **Settings**.</span></span>

<span data-ttu-id="efc62-130">En kort beskrivning av fälten följer.</span><span class="sxs-lookup"><span data-stu-id="efc62-130">A brief description of the fields follows.</span></span>

![Strömmande slutpunkt](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints4.png)

1. <span data-ttu-id="efc62-132">Maximal cachestorlek princip: används för att konfigurera cache-livstid för tillgångar som hanteras via den här strömmande slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="efc62-132">Maximum cache policy: used to configure cache lifetime for assets served through this streaming endpoint.</span></span> <span data-ttu-id="efc62-133">Om inget värde har angetts används standardvärdet.</span><span class="sxs-lookup"><span data-stu-id="efc62-133">If no value is set, the default is used.</span></span> <span data-ttu-id="efc62-134">Standardvärdena kan också definieras direkt i Azure-lagring.</span><span class="sxs-lookup"><span data-stu-id="efc62-134">The default values can also be defined directly in Azure storage.</span></span> <span data-ttu-id="efc62-135">Om Azure CDN aktiveras för den strömmande slutpunkten kan ställas inte in principvärdet cache på mindre än 600 sekunder.</span><span class="sxs-lookup"><span data-stu-id="efc62-135">If Azure CDN is enabled for the streaming endpoint, you should not set the cache policy value to less than 600 seconds.</span></span>  
2. <span data-ttu-id="efc62-136">Tillåtna IP-adresser: används för att ange IP-adresser som skulle kunna ansluta till den publicerade strömningsslutpunkt.</span><span class="sxs-lookup"><span data-stu-id="efc62-136">Allowed IP addresses: used to specify IP addresses that would be allowed to connect to the published streaming endpoint.</span></span> <span data-ttu-id="efc62-137">Om inga angivna IP-adresser, skulle IP-adresser kunna ansluta.</span><span class="sxs-lookup"><span data-stu-id="efc62-137">If no IP addresses specified, any IP address would be able to connect.</span></span> <span data-ttu-id="efc62-138">IP-adresser kan anges som en IP-adress (t.ex, 10.0.0.1), en IP-intervall med IP-adress och nätmask CIDR (till exempel 10.0.0.1/22) eller en IP-intervall med IP-adress och en prickad decimal nätmask (till exempel ”10.0.0.1 () 255.255.255.0)').</span><span class="sxs-lookup"><span data-stu-id="efc62-138">IP addresses can be specified as either a single IP address (for example, '10.0.0.1'), an IP range using an IP address and a CIDR subnet mask (for example, '10.0.0.1/22'), or an IP range using IP address and a dotted decimal subnet mask (for example, '10.0.0.1(255.255.255.0)').</span></span>
3. <span data-ttu-id="efc62-139">Konfiguration för autentisering med Akamai signatur-huvud: används för att ange hur signatur sidhuvud autentiseringsbegäran från Akamai servrar har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="efc62-139">Configuration for Akamai signature header authentication: used to specify how signature header authentication request from Akamai servers is configured.</span></span> <span data-ttu-id="efc62-140">Förfallodatum är i UTC.</span><span class="sxs-lookup"><span data-stu-id="efc62-140">Expiration is in UTC.</span></span>

## <a name="scale-your-premium-streaming-endpoint"></a><span data-ttu-id="efc62-141">Skala din Premium strömmande slutpunkten</span><span class="sxs-lookup"><span data-stu-id="efc62-141">Scale your Premium streaming endpoint</span></span>

<span data-ttu-id="efc62-142">Mer information finns i [detta](media-services-portal-scale-streaming-endpoints.md) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="efc62-142">For more information, see [this](media-services-portal-scale-streaming-endpoints.md) topic.</span></span>

## <span data-ttu-id="efc62-143"><a id="enable_cdn"></a>Aktivera Azure CDN-integrering</span><span class="sxs-lookup"><span data-stu-id="efc62-143"><a id="enable_cdn"></a>Enable Azure CDN integration</span></span>

<span data-ttu-id="efc62-144">När du skapar ett nytt konto är standard strömmande slutpunkten Azure CDN-integrering aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="efc62-144">When you create a new account, default Streaming Endpoint Azure CDN integration is enabled by default.</span></span>

<span data-ttu-id="efc62-145">Om du senare vill Aktiverar/inaktiverar CDN strömmande slutpunkten måste vara i den **stoppats** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="efc62-145">If you later want to disable/enable the CDN, your streaming endpoint must be in the **stopped** state.</span></span> <span data-ttu-id="efc62-146">Det kan ta upp till två timmar för Azure CDN-integrering för att hämta aktiverad och för att ändringarna ska vara aktiva för alla de CDN POP.</span><span class="sxs-lookup"><span data-stu-id="efc62-146">It could take up to 2 hours for the Azure CDN integration to get enabled and for the changes to be active across all the CDN POPs.</span></span> <span data-ttu-id="efc62-147">Dock kan starta strömmande slutpunkt och dataströmmen utan avbrott från strömningsslutpunkt och när integrationen är slutförd, dataströmmen levereras från CDN.</span><span class="sxs-lookup"><span data-stu-id="efc62-147">However, your can start your streaming endpoint and stream without interruptions from the streaming endpoint and once the integration is complete, the stream will be delivered from the CDN.</span></span> <span data-ttu-id="efc62-148">Under etablering strömmande slutpunkten kommer att ha **startar** tillstånd och du kan se degredad prestanda.</span><span class="sxs-lookup"><span data-stu-id="efc62-148">During the provisioning period your streaming endpoint will be in **starting** state and you might observe degredad performance.</span></span>

<span data-ttu-id="efc62-149">CDN-integrering har aktiverats i Azure data Datacenter execpt Kina och Federal Government regioner.</span><span class="sxs-lookup"><span data-stu-id="efc62-149">CDN integration is enabled in all the Azure data centers execpt China and Federal Goverment regions.</span></span>

<span data-ttu-id="efc62-150">När den har aktiverats på **åtkomstkontroll**, **anpassad hostname** och **autentisering med Akamai signatur** konfigurationen hämtar inaktiverad.</span><span class="sxs-lookup"><span data-stu-id="efc62-150">Once it is enabled, the **Access Control**, **Custom hostname** and **Akamai Signature authentication** configuration gets disabled.</span></span>
 
> [!IMPORTANT]
> <span data-ttu-id="efc62-151">Azure Media Services-integrering med Azure CDN implementeras på **Azure CDN från Verizon** standard strömningsslutpunkter.</span><span class="sxs-lookup"><span data-stu-id="efc62-151">Azure Media Services integration with Azure CDN is implemented on **Azure CDN from Verizon** for standard streaming endpoints.</span></span> <span data-ttu-id="efc62-152">Premium strömningsslutpunkter kan konfigureras med hjälp av alla **Azure CDN priser nivåer och providers**.</span><span class="sxs-lookup"><span data-stu-id="efc62-152">Premium streaming endpoints can be configured using all **Azure CDN pricing tiers and providers**.</span></span> <span data-ttu-id="efc62-153">Mer information om Azure CDN-funktioner finns i [CDN-översikt](../cdn/cdn-overview.md).</span><span class="sxs-lookup"><span data-stu-id="efc62-153">For more information about Azure CDN features, see the [CDN overview](../cdn/cdn-overview.md).</span></span>
 
### <a name="additional-considerations"></a><span data-ttu-id="efc62-154">Annat som är bra att tänka på</span><span class="sxs-lookup"><span data-stu-id="efc62-154">Additional considerations</span></span>

* <span data-ttu-id="efc62-155">När CDN är aktiverad för en strömmande slutpunkten, det går inte att klienter begär innehåll direkt från ursprunget.</span><span class="sxs-lookup"><span data-stu-id="efc62-155">When CDN is enabled for a streaming endpoint, clients cannot request content directly from the origin.</span></span> <span data-ttu-id="efc62-156">Om du behöver kunna testa ditt innehåll med eller utan CDN kan skapa du en annan strömningsslutpunkt som inte är aktiverat CDN.</span><span class="sxs-lookup"><span data-stu-id="efc62-156">If you need the ability to test your content with or without CDN, you can create another streaming endpoint that isn't CDN enabled.</span></span>
* <span data-ttu-id="efc62-157">Din strömmande slutpunktens värdnamn är densamma när du har aktiverat CDN.</span><span class="sxs-lookup"><span data-stu-id="efc62-157">Your streaming endpoint hostname remains the same after enabling CDN.</span></span> <span data-ttu-id="efc62-158">Du behöver inte göra ändringar i media services arbetsflödet efter CDN har aktiverats.</span><span class="sxs-lookup"><span data-stu-id="efc62-158">You don’t need to make any changes to your media services workflow after CDN is enabled.</span></span> <span data-ttu-id="efc62-159">Om din strömmande slutpunktens värdnamn är strasbourg.streaming.mediaservices.windows.net, efter att aktivera CDN, använda exakt samma värdnamn.</span><span class="sxs-lookup"><span data-stu-id="efc62-159">For example, if your streaming endpoint hostname is strasbourg.streaming.mediaservices.windows.net, after enabling CDN, the exact same hostname is used.</span></span>
* <span data-ttu-id="efc62-160">För nya strömningsslutpunkter kan du aktivera CDN genom att skapa en ny slutpunkt; för befintliga strömningsslutpunkter behöver du först stoppa slutpunkten och sedan aktivera/inaktivera CDN.</span><span class="sxs-lookup"><span data-stu-id="efc62-160">For new streaming endpoints, you can enable CDN simply by creating a new endpoint; for existing streaming endpoints, you need to first stop the endpoint and then enable/disable the CDN.</span></span>
* <span data-ttu-id="efc62-161">Standard strömmande slutpunkten kan bara konfigureras med hjälp av **Verizon Standard CDN-providern** med hjälp av Azure-hanteringsportalen.</span><span class="sxs-lookup"><span data-stu-id="efc62-161">Standard streaming endpoint can only be configured using **Verizon Standard CDN provider** using Azure management portal.</span></span> <span data-ttu-id="efc62-162">Du kan dock aktivera andra leverantörer av Azure CDN med hjälp av REST API: er.</span><span class="sxs-lookup"><span data-stu-id="efc62-162">However, you can enable other Azure CDN providers using REST APIs.</span></span>

## <a name="configure-cdn-profile"></a><span data-ttu-id="efc62-163">Konfigurera CDN-profilen</span><span class="sxs-lookup"><span data-stu-id="efc62-163">Configure CDN profile</span></span>

<span data-ttu-id="efc62-164">Du kan konfigurera CDN-profilen genom att välja den **hantera CDN** knappen från överkanten.</span><span class="sxs-lookup"><span data-stu-id="efc62-164">You can configure the CDN profile by selecting the **Manage CDN** button from the top.</span></span>

![Strömmande slutpunkt](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints6.png)

## <a name="next-steps"></a><span data-ttu-id="efc62-166">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="efc62-166">Next steps</span></span>
<span data-ttu-id="efc62-167">Granska sökvägarna för Media Services-utbildning.</span><span class="sxs-lookup"><span data-stu-id="efc62-167">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="efc62-168">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="efc62-168">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

