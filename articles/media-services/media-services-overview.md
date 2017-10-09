---
title: "aaaAzure Media Services översikt | Microsoft Docs"
description: "Det här avsnittet ger en översikt över Azure Media Services"
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 7a5e9723-c379-446b-b4d6-d0e41bd7d31f
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/04/2017
ms.author: juliako;anilmur
ms.openlocfilehash: 81f9f4d9ff75effea30c10fd09449e9d2025f377
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-media-services-overview"></a><span data-ttu-id="c85e4-103">Översikt över Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="c85e4-103">Azure Media Services overview</span></span> 

<span data-ttu-id="c85e4-104">Microsoft Azure Media Services är en utökningsbar molnbaserad plattform som gör att utvecklare toobuild skalbara media hanterings- och program.</span><span class="sxs-lookup"><span data-stu-id="c85e4-104">Microsoft Azure Media Services is an extensible cloud-based platform that enables developers toobuild scalable media management and delivery applications.</span></span> <span data-ttu-id="c85e4-105">Media Services baseras på REST API: er som gör att du toosecurely överföra, lagra, koda och paketera video-eller ljudinnehåll för både på begäran och live strömmande leverans toovarious klienter (till exempel TV, datorer och mobila enheter).</span><span class="sxs-lookup"><span data-stu-id="c85e4-105">Media Services is based on REST APIs that enable you toosecurely upload, store, encode, and package video or audio content for both on-demand and live streaming delivery toovarious clients (for example, TV, PC, and mobile devices).</span></span>

<span data-ttu-id="c85e4-106">Du kan bygga arbetsflöden för slutpunkt till slutpunkt bara med Media Services.</span><span class="sxs-lookup"><span data-stu-id="c85e4-106">You can build end-to-end workflows using entirely Media Services.</span></span> <span data-ttu-id="c85e4-107">Du kan också välja toouse komponenter från tredje part för vissa delar av arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="c85e4-107">You can also choose toouse third-party components for some parts of your workflow.</span></span> <span data-ttu-id="c85e4-108">Koda till exempel med hjälp av en kodare från tredje part.</span><span class="sxs-lookup"><span data-stu-id="c85e4-108">For example, encode using a third-party encoder.</span></span> <span data-ttu-id="c85e4-109">Därefter kan du överföra, skydda, paketera och leverera med Media Services.</span><span class="sxs-lookup"><span data-stu-id="c85e4-109">Then, upload, protect, package, deliver using Media Services.</span></span>

<span data-ttu-id="c85e4-110">Du kan välja toostream innehållet live eller leverera innehåll på begäran.</span><span class="sxs-lookup"><span data-stu-id="c85e4-110">You can choose toostream your content live or deliver content on-demand.</span></span> <span data-ttu-id="c85e4-111">hello innehåller också länkar tooother relevanta avsnitt.</span><span class="sxs-lookup"><span data-stu-id="c85e4-111">hello topic also links tooother relevant topics.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="c85e4-112">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="c85e4-112">Media Services learning paths</span></span>
<span data-ttu-id="c85e4-113">Du kan visa sökvägar för AMS-utbildning här:</span><span class="sxs-lookup"><span data-stu-id="c85e4-113">You can view AMS learning paths here:</span></span>

* [<span data-ttu-id="c85e4-114">Arbetsflöde för AMS-liveuppspelning</span><span class="sxs-lookup"><span data-stu-id="c85e4-114">AMS Live Streaming Workflow</span></span>](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-live/)
* [<span data-ttu-id="c85e4-115">Arbetsflöde för AMS-strömning på begäran</span><span class="sxs-lookup"><span data-stu-id="c85e4-115">AMS on-Demand Streaming Workflow</span></span>](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-on-demand/)

## <a name="prerequisites"></a><span data-ttu-id="c85e4-116">Krav</span><span class="sxs-lookup"><span data-stu-id="c85e4-116">Prerequisites</span></span>

<span data-ttu-id="c85e4-117">toostart med Azure Media Services bör du ha hello följande:</span><span class="sxs-lookup"><span data-stu-id="c85e4-117">toostart using Azure Media Services, you should have hello following:</span></span>

* <span data-ttu-id="c85e4-118">Ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="c85e4-118">An Azure account.</span></span> <span data-ttu-id="c85e4-119">Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="c85e4-119">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="c85e4-120">Mer information finns i [kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="c85e4-120">For details, see [Azure Free Trial](https://azure.microsoft.com).</span></span>
* <span data-ttu-id="c85e4-121">Ett Azure Media Services-konto.</span><span class="sxs-lookup"><span data-stu-id="c85e4-121">An Azure Media Services account.</span></span> <span data-ttu-id="c85e4-122">Mer information finns i [Skapa konto](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="c85e4-122">For more information, see [Create Account](media-services-portal-create-account.md).</span></span>
* <span data-ttu-id="c85e4-123">(Valfritt) Konfigurera utvecklingsmiljön.</span><span class="sxs-lookup"><span data-stu-id="c85e4-123">(Optional) Set up development environment.</span></span> <span data-ttu-id="c85e4-124">Välj .NET eller REST API för din utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="c85e4-124">Choose .NET or REST API for your development environment.</span></span> <span data-ttu-id="c85e4-125">Mer information finns i [Ställa in miljön](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="c85e4-125">For more information, see [Set up environment](media-services-dotnet-how-to-use.md).</span></span>

    <span data-ttu-id="c85e4-126">Lär dig också hur för[ansluta programmässigt tooAMS API](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="c85e4-126">Also, learn how too[connect  programmatically tooAMS API](media-services-use-aad-auth-to-access-ams-api.md).</span></span>
* <span data-ttu-id="c85e4-127">En standard- eller premiumslutpunkt för direktuppspelning med tillståndet Startad.</span><span class="sxs-lookup"><span data-stu-id="c85e4-127">A standard or premium streaming endpoint in started state.</span></span>  <span data-ttu-id="c85e4-128">Mer information finns i [Hantera strömningsslutpunkter](media-services-portal-manage-streaming-endpoints.md).</span><span class="sxs-lookup"><span data-stu-id="c85e4-128">For more information, see [Managing streaming endpoints](media-services-portal-manage-streaming-endpoints.md)</span></span>

## <a name="sdks-and-tools"></a><span data-ttu-id="c85e4-129">SDK:er och verktyg</span><span class="sxs-lookup"><span data-stu-id="c85e4-129">SDKs and tools</span></span>

<span data-ttu-id="c85e4-130">toobuild Media Services-lösningar som du kan använda:</span><span class="sxs-lookup"><span data-stu-id="c85e4-130">toobuild Media Services solutions, you can use:</span></span>

* [<span data-ttu-id="c85e4-131">Media Services REST API</span><span class="sxs-lookup"><span data-stu-id="c85e4-131">Media Services REST API</span></span>](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference)
* <span data-ttu-id="c85e4-132">En av hello tillgängliga klient-SDK:</span><span class="sxs-lookup"><span data-stu-id="c85e4-132">One of hello available client SDKs:</span></span>
    * <span data-ttu-id="c85e4-133">[Azure Media Services SDK för .NET](https://github.com/Azure/azure-sdk-for-media-services),</span><span class="sxs-lookup"><span data-stu-id="c85e4-133">[Azure Media Services SDK for .NET](https://github.com/Azure/azure-sdk-for-media-services),</span></span>
    * <span data-ttu-id="c85e4-134">[Azure SDK för Java](https://github.com/Azure/azure-sdk-for-java),</span><span class="sxs-lookup"><span data-stu-id="c85e4-134">[Azure SDK for Java](https://github.com/Azure/azure-sdk-for-java),</span></span>
    * <span data-ttu-id="c85e4-135">[Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php),</span><span class="sxs-lookup"><span data-stu-id="c85e4-135">[Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php),</span></span>
    * <span data-ttu-id="c85e4-136">[Azure Media Services för Node.js](https://github.com/michelle-becker/node-ams-sdk/blob/master/lib/request.js) (detta är en icke-Microsoft-version av en Node.js SDK.</span><span class="sxs-lookup"><span data-stu-id="c85e4-136">[Azure Media Services for Node.js](https://github.com/michelle-becker/node-ams-sdk/blob/master/lib/request.js) (This is a non-Microsoft version of a Node.js SDK.</span></span> <span data-ttu-id="c85e4-137">Den underhålls av en grupp och är inte en 100% täckning för hello AMS APIs).</span><span class="sxs-lookup"><span data-stu-id="c85e4-137">It is maintained by a community and currently does not have a 100% coverage of hello AMS APIs).</span></span>
* <span data-ttu-id="c85e4-138">Befintliga verktyg:</span><span class="sxs-lookup"><span data-stu-id="c85e4-138">Existing tools:</span></span>
    * [<span data-ttu-id="c85e4-139">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c85e4-139">Azure portal</span></span>](https://portal.azure.com/)
    * <span data-ttu-id="c85e4-140">[Azure-Media Services-Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (Azure Media Services Explorer (AMSE) är ett Winforms/C#-program för Windows)</span><span class="sxs-lookup"><span data-stu-id="c85e4-140">[Azure-Media-Services-Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (Azure Media Services Explorer (AMSE) is a Winforms/C# application for Windows)</span></span>

## <a name="concepts-and-overview"></a><span data-ttu-id="c85e4-141">Koncept och översikt</span><span class="sxs-lookup"><span data-stu-id="c85e4-141">Concepts and overview</span></span>
<span data-ttu-id="c85e4-142">Azure Media Services-koncepten finns i [Koncept](media-services-concepts.md).</span><span class="sxs-lookup"><span data-stu-id="c85e4-142">For Azure Media Services concepts, see [Concepts](media-services-concepts.md).</span></span>

<span data-ttu-id="c85e4-143">En hur-tooseries som introducerar tooall hello huvudkomponenterna i Azure Media Services, se [Azure Media Services stegvisa självstudier](https://docs.com/fukushima-shigeyuki/3439/english-azure-media-services-step-by-step-series).</span><span class="sxs-lookup"><span data-stu-id="c85e4-143">For a how-tooseries that introduces you tooall hello main components of Azure Media Services, see [Azure Media Services Step-by-Step tutorials](https://docs.com/fukushima-shigeyuki/3439/english-azure-media-services-step-by-step-series).</span></span> <span data-ttu-id="c85e4-144">Den här serien ger en bra översikt över koncepten och använder hello AMSE-verktyget toodemonstrate AMS uppgifter.</span><span class="sxs-lookup"><span data-stu-id="c85e4-144">This series has a great overview of concepts and it uses hello AMSE tool toodemonstrate AMS tasks.</span></span> <span data-ttu-id="c85e4-145">AMSE-verktyget är ett verktyg i Windows.</span><span class="sxs-lookup"><span data-stu-id="c85e4-145">AMSE tool is a Windows tool.</span></span> <span data-ttu-id="c85e4-146">Det här verktyget stöder de flesta av hello uppgifter som du kan göra programmässigt med [AMS SDK för .NET](https://github.com/Azure/azure-sdk-for-media-services), [Azure SDK för Java](https://github.com/Azure/azure-sdk-for-java), eller [Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php).</span><span class="sxs-lookup"><span data-stu-id="c85e4-146">This tool supports most of hello tasks you can achieve programmatically with [AMS SDK for .NET](https://github.com/Azure/azure-sdk-for-media-services), [Azure SDK for Java](https://github.com/Azure/azure-sdk-for-java), or  [Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php).</span></span>

## <a name="supported-scenarios-and-availability-of-media-services-across-data-centers"></a><span data-ttu-id="c85e4-147">Scenarier som stöds och tillgänglighet för Media Services över datacenter</span><span class="sxs-lookup"><span data-stu-id="c85e4-147">Supported scenarios and availability of Media Services across data centers</span></span>

<span data-ttu-id="c85e4-148">Detaljerad information finns i [AMS-scenarier och tillgänglighet för funktioner och tjänster över datacenter](scenarios-and-availability.md).</span><span class="sxs-lookup"><span data-stu-id="c85e4-148">For detailed information, see [AMS scenarios and availability of features and services across data centers](scenarios-and-availability.md).</span></span>

## <a name="service-level-agreement-sla"></a><span data-ttu-id="c85e4-149">Serviceavtal (SLA)</span><span class="sxs-lookup"><span data-stu-id="c85e4-149">Service Level Agreement (SLA)</span></span>

* <span data-ttu-id="c85e4-150">Vi garanterar 99,9 % tillgänglighet för REST API-transaktioner för Media Services Encoding.</span><span class="sxs-lookup"><span data-stu-id="c85e4-150">For Media Services Encoding, we guarantee 99.9% availability of REST API transactions.</span></span>
* <span data-ttu-id="c85e4-151">För Streaming svarar vi på serviceförfrågningar med 99,9 % tillgänglighet för befintligt medieinnehåll när en Standard- eller Premium-slutpunkt för direktuppspelning har köpts.</span><span class="sxs-lookup"><span data-stu-id="c85e4-151">For Streaming, we will successfully service requests with a 99.9% availability guarantee for existing media content when a standard or premium streaming endpoint is purchased.</span></span>
* <span data-ttu-id="c85e4-152">För Live-kanaler garanterar vi att köra kanaler ska ha extern anslutning minst 99,9% av hello tid.</span><span class="sxs-lookup"><span data-stu-id="c85e4-152">For Live Channels, we guarantee that running Channels will have external connectivity at least 99.9% of hello time.</span></span>
* <span data-ttu-id="c85e4-153">Content Protection garanterar vi att vi ska uppfyller viktiga förfrågningar minst 99,9% av hello tid.</span><span class="sxs-lookup"><span data-stu-id="c85e4-153">For Content Protection, we guarantee that we will successfully fulfill key requests at least 99.9% of hello time.</span></span>
* <span data-ttu-id="c85e4-154">Indexer ska vi utföra service för indexeringsuppgifter som bearbetas med en Encoding-reserverad enhet 99,9% av hello tid.</span><span class="sxs-lookup"><span data-stu-id="c85e4-154">For Indexer, we will successfully service Indexer Task requests processed with an Encoding Reserved Unit 99.9% of hello time.</span></span>

<span data-ttu-id="c85e4-155">Mer information finns i [Microsoft Azure SLA](https://azure.microsoft.com/support/legal/sla/).</span><span class="sxs-lookup"><span data-stu-id="c85e4-155">For more information, see [Microsoft Azure SLA](https://azure.microsoft.com/support/legal/sla/).</span></span>

<span data-ttu-id="c85e4-156">Information om tillgänglighet i datacenter finns hello [Avaiability](scenarios-and-availability.md#availability) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="c85e4-156">For information about availability in datacenters, see hello [Avaiability](scenarios-and-availability.md#availability) section.</span></span>

## <a name="support"></a><span data-ttu-id="c85e4-157">Support</span><span class="sxs-lookup"><span data-stu-id="c85e4-157">Support</span></span>

<span data-ttu-id="c85e4-158">[Azure-supporten](https://azure.microsoft.com/support/options/) tillhandahåller support för Azure, inklusive Media Services.</span><span class="sxs-lookup"><span data-stu-id="c85e4-158">[Azure Support](https://azure.microsoft.com/support/options/) provides support options for Azure, including Media Services.</span></span>

## <a name="provide-feedback"></a><span data-ttu-id="c85e4-159">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="c85e4-159">Provide feedback</span></span>

[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
