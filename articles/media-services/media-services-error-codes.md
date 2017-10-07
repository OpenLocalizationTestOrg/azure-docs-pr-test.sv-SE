---
title: "felkoder för aaaAzure Media Services | Microsoft Docs"
description: "hello avsnittet ger en översikt över Azure Media Services-felkoder."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: d3a62a64-7608-4b17-8667-479b26ba0d6c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: juliako
ms.openlocfilehash: de1ffd6dee8901a3051eb5032536c3669482d6b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-media-services-error-codes"></a><span data-ttu-id="5799d-103">Azure Media Services-felkoder</span><span class="sxs-lookup"><span data-stu-id="5799d-103">Azure Media Services error codes</span></span>
<span data-ttu-id="5799d-104">När du använder Microsoft Azure Media Services kan du få HTTP felkoder från hello tjänsten beroende på problem som till exempel autentiseringstoken ut tooactions som inte stöds i Media Services.</span><span class="sxs-lookup"><span data-stu-id="5799d-104">When using Microsoft Azure Media Services, you may receive HTTP error codes from hello service depending on issues such as authentication tokens expiring tooactions that are not supported in Media Services.</span></span> <span data-ttu-id="5799d-105">hello följer en lista över **HTTP felkoder** som kan returneras av Media Services och hello möjliga orsakerna till dem.</span><span class="sxs-lookup"><span data-stu-id="5799d-105">hello following is a list of **HTTP error codes** that may be returned by Media Services and hello possible causes for them.</span></span>  

## <a name="400-bad-request"></a><span data-ttu-id="5799d-106">400 Felaktig förfrågan</span><span class="sxs-lookup"><span data-stu-id="5799d-106">400 Bad Request</span></span>
<span data-ttu-id="5799d-107">hello-begäran innehåller ogiltig information och nekas på grund av tooone av hello följande orsaker:</span><span class="sxs-lookup"><span data-stu-id="5799d-107">hello request contains invalid information and is rejected due tooone of hello following reasons:</span></span>

* <span data-ttu-id="5799d-108">En API-version som inte stöds har angetts.</span><span class="sxs-lookup"><span data-stu-id="5799d-108">An unsupported API version is specified.</span></span> <span data-ttu-id="5799d-109">Senaste version hello finns [installationsprogrammet för Media Services REST API-utveckling](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="5799d-109">For hello most current version, see [Setup for Media Services REST API Development](media-services-rest-how-to-use.md).</span></span>
* <span data-ttu-id="5799d-110">hello API-version av Media Services har inte angetts.</span><span class="sxs-lookup"><span data-stu-id="5799d-110">hello API version of Media Services is not specified.</span></span> <span data-ttu-id="5799d-111">Mer information om hur toospecify hello API-versionen finns [Media Services Operations REST API-referens](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference).</span><span class="sxs-lookup"><span data-stu-id="5799d-111">For information on how toospecify hello API version, see [Media Services Operations REST API Reference](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference).</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="5799d-112">Om du använder hello .NET eller Java SDK tooconnect tooMedia tjänster, hello API-version har angetts för dig när du försöker och utföra vissa åtgärder mot Media Services.</span><span class="sxs-lookup"><span data-stu-id="5799d-112">If you are using hello .NET or Java SDKs tooconnect tooMedia Services, hello API version is specified for you whenever you try and perform some action against Media Services.</span></span>
  > 
  > 
* <span data-ttu-id="5799d-113">En odefinierad egenskap har angetts.</span><span class="sxs-lookup"><span data-stu-id="5799d-113">An undefined property has been specified.</span></span> <span data-ttu-id="5799d-114">hello egenskapsnamnet är i hello felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="5799d-114">hello property name is in hello error message.</span></span> <span data-ttu-id="5799d-115">Endast de egenskaper som är medlemmar i en given entitet kan anges.</span><span class="sxs-lookup"><span data-stu-id="5799d-115">Only those properties that are members of a given entity can be specified.</span></span> <span data-ttu-id="5799d-116">Se [Azure Media Services REST API-referens](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference) för en lista över enheter och deras egenskaper.</span><span class="sxs-lookup"><span data-stu-id="5799d-116">See [Azure Media Services REST API Reference](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference) for a list of entities and their properties.</span></span>
* <span data-ttu-id="5799d-117">Ett ogiltigt egenskapsvärde har angetts.</span><span class="sxs-lookup"><span data-stu-id="5799d-117">An invalid property value has been specified.</span></span> <span data-ttu-id="5799d-118">hello egenskapsnamnet är i hello felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="5799d-118">hello property name is in hello error message.</span></span> <span data-ttu-id="5799d-119">Se hello föregående länk för giltig egenskapstyper och deras värden.</span><span class="sxs-lookup"><span data-stu-id="5799d-119">See hello previous link for valid property types and their values.</span></span>
* <span data-ttu-id="5799d-120">Ett värde för egenskapen saknas och måste anges.</span><span class="sxs-lookup"><span data-stu-id="5799d-120">A property value is missing and is required.</span></span>
* <span data-ttu-id="5799d-121">En del av hello angivna URL: en innehåller ett felaktigt värde.</span><span class="sxs-lookup"><span data-stu-id="5799d-121">Part of hello URL specified contains a bad value.</span></span>
* <span data-ttu-id="5799d-122">Ett försök har gjorts tooupdate en WriteOnce-egenskap.</span><span class="sxs-lookup"><span data-stu-id="5799d-122">An attempt was made tooupdate a WriteOnce property.</span></span>
* <span data-ttu-id="5799d-123">Ett försök har gjorts toocreate ett jobb som har en inkommande tillgång med en primär AssetFile som angavs inte eller kunde inte fastställas.</span><span class="sxs-lookup"><span data-stu-id="5799d-123">An attempt was made toocreate a Job that has an input Asset with a primary AssetFile that was not specified or could not be determined.</span></span>
* <span data-ttu-id="5799d-124">Ett försök har gjorts tooupdate en SAS-lokaliserare.</span><span class="sxs-lookup"><span data-stu-id="5799d-124">An attempt was made tooupdate a SAS Locator.</span></span> <span data-ttu-id="5799d-125">SAS-positionerare kan endast skapas eller tas bort.</span><span class="sxs-lookup"><span data-stu-id="5799d-125">SAS locators can only be created or deleted.</span></span> <span data-ttu-id="5799d-126">Strömning positionerare kan uppdateras.</span><span class="sxs-lookup"><span data-stu-id="5799d-126">Streaming locators can be updated.</span></span> <span data-ttu-id="5799d-127">Mer information finns i [lokaliserare](https://docs.microsoft.com/rest/api/media/operations/locator).</span><span class="sxs-lookup"><span data-stu-id="5799d-127">For more information, see [Locators](https://docs.microsoft.com/rest/api/media/operations/locator).</span></span>
* <span data-ttu-id="5799d-128">En åtgärd som inte stöds eller en fråga skickades.</span><span class="sxs-lookup"><span data-stu-id="5799d-128">An unsupported operation or query was submitted.</span></span>

## <a name="401-unauthorized"></a><span data-ttu-id="5799d-129">401 obehörig</span><span class="sxs-lookup"><span data-stu-id="5799d-129">401 Unauthorized</span></span>
<span data-ttu-id="5799d-130">hello begäran kunde inte autentiseras (innan den kan godkännas) på grund av tooone av hello följande orsaker:</span><span class="sxs-lookup"><span data-stu-id="5799d-130">hello request could not be authenticated (before it can be authorized) due tooone of hello following reasons:</span></span>

* <span data-ttu-id="5799d-131">Autentiseringshuvud saknas.</span><span class="sxs-lookup"><span data-stu-id="5799d-131">Missing authentication header.</span></span>
* <span data-ttu-id="5799d-132">Felaktig autentisering huvudvärde.</span><span class="sxs-lookup"><span data-stu-id="5799d-132">Bad authentication header value.</span></span>
  * <span data-ttu-id="5799d-133">hello-token har upphört att gälla.</span><span class="sxs-lookup"><span data-stu-id="5799d-133">hello token has expired.</span></span> 
  * <span data-ttu-id="5799d-134">hello token innehåller en ogiltig signatur.</span><span class="sxs-lookup"><span data-stu-id="5799d-134">hello token contains an invalid signature.</span></span>

## <a name="403-forbidden"></a><span data-ttu-id="5799d-135">403 Nekad</span><span class="sxs-lookup"><span data-stu-id="5799d-135">403 Forbidden</span></span>
<span data-ttu-id="5799d-136">hello-begäran är inte tillåtet på grund av tooone av hello följande orsaker:</span><span class="sxs-lookup"><span data-stu-id="5799d-136">hello request is not allowed due tooone of hello following reasons:</span></span>

* <span data-ttu-id="5799d-137">hello Media Services-konto kan inte hittas eller har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="5799d-137">hello Media Services account cannot be found or has been deleted.</span></span>
* <span data-ttu-id="5799d-138">hello Media Services-konto har inaktiverats och hello Begärandetypen är inte HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="5799d-138">hello Media Services account is disabled and hello request type is not HTTP GET.</span></span> <span data-ttu-id="5799d-139">Tjänståtgärder returneras ett 403 svar.</span><span class="sxs-lookup"><span data-stu-id="5799d-139">Service operations will return a 403 response as well.</span></span>
* <span data-ttu-id="5799d-140">hello autentiseringstoken innehåller inte information om hello användarens autentiseringsuppgifter: AccountName och/eller prenumerations-ID.</span><span class="sxs-lookup"><span data-stu-id="5799d-140">hello authentication token does not contain hello user’s credential information: AccountName and/or SubscriptionId.</span></span> <span data-ttu-id="5799d-141">Du hittar den här informationen i hello Media Services UI-tillägg för ditt Media Services-konto i hello Azure-hanteringsportalen.</span><span class="sxs-lookup"><span data-stu-id="5799d-141">You can find this information in hello Media Services UI extension for your Media Services account in hello Azure Management Portal.</span></span>
* <span data-ttu-id="5799d-142">hello resursen kan inte nås.</span><span class="sxs-lookup"><span data-stu-id="5799d-142">hello resource cannot be accessed.</span></span>
  
  * <span data-ttu-id="5799d-143">Ett försök har gjorts toouse en MediaProcessor som inte är tillgängligt för Media Services-kontot.</span><span class="sxs-lookup"><span data-stu-id="5799d-143">An attempt was made toouse a MediaProcessor that is not available for your Media Services account.</span></span>
  * <span data-ttu-id="5799d-144">Ett försök gjordes tooupdate en jobbmall definieras av Media Services.</span><span class="sxs-lookup"><span data-stu-id="5799d-144">An attempt was made tooupdate a JobTemplate defined by Media Services.</span></span>
  * <span data-ttu-id="5799d-145">Ett försök har gjorts toooverwrite vissa andra Media Services kontots lokaliserare.</span><span class="sxs-lookup"><span data-stu-id="5799d-145">An attempt was made toooverwrite some other Media Services account's Locator.</span></span>
  * <span data-ttu-id="5799d-146">Ett försök har gjorts toooverwrite vissa andra Media Services kontots ContentKey.</span><span class="sxs-lookup"><span data-stu-id="5799d-146">An attempt was made toooverwrite some other Media Services account's ContentKey.</span></span>
* <span data-ttu-id="5799d-147">hello resurs kunde inte skapas på grund av tooa service kvot som har uppnåtts för hello Media Services-konto.</span><span class="sxs-lookup"><span data-stu-id="5799d-147">hello resource could not be created due tooa service quota that was reached for hello Media Services account.</span></span> <span data-ttu-id="5799d-148">Mer information om hello service kvoter finns [kvoter och begränsningar](media-services-quotas-and-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="5799d-148">For more information on hello service quotas, see [Quotas and Limitations](media-services-quotas-and-limitations.md).</span></span>

## <a name="404-not-found"></a><span data-ttu-id="5799d-149">404 Hittades inte</span><span class="sxs-lookup"><span data-stu-id="5799d-149">404 Not Found</span></span>
<span data-ttu-id="5799d-150">hello-begäran är inte tillåten på en resurs på grund av tooone av hello följande orsaker:</span><span class="sxs-lookup"><span data-stu-id="5799d-150">hello request is not allowed on a resource due tooone of hello following reasons:</span></span>

* <span data-ttu-id="5799d-151">Ett försök har gjorts tooupdate en entitet som inte finns.</span><span class="sxs-lookup"><span data-stu-id="5799d-151">An attempt was made tooupdate an entity that does not exist.</span></span>
* <span data-ttu-id="5799d-152">Ett försök har gjorts toodelete en entitet som inte finns.</span><span class="sxs-lookup"><span data-stu-id="5799d-152">An attempt was made toodelete an entity that does not exist.</span></span>
* <span data-ttu-id="5799d-153">Ett försök har gjorts toocreate en entitet som länkar tooan entitet som inte finns.</span><span class="sxs-lookup"><span data-stu-id="5799d-153">An attempt was made toocreate an entity that links tooan entity that does not exist.</span></span>
* <span data-ttu-id="5799d-154">Ett försök har gjorts tooGET en entitet som inte finns.</span><span class="sxs-lookup"><span data-stu-id="5799d-154">An attempt was made tooGET an entity that does not exist.</span></span>
* <span data-ttu-id="5799d-155">Ett försök har gjorts toospecify ett lagringskonto som inte är associerad med hello Media Services-konto.</span><span class="sxs-lookup"><span data-stu-id="5799d-155">An attempt was made toospecify a storage account that is not associated with hello Media Services account.</span></span>  

## <a name="409-conflict"></a><span data-ttu-id="5799d-156">409 konflikt</span><span class="sxs-lookup"><span data-stu-id="5799d-156">409 Conflict</span></span>
<span data-ttu-id="5799d-157">hello-begäran är inte tillåtet på grund av tooone av hello följande orsaker:</span><span class="sxs-lookup"><span data-stu-id="5799d-157">hello request is not allowed due tooone of hello following reasons:</span></span>

* <span data-ttu-id="5799d-158">Mer än en AssetFile har hello angivet namn inom hello tillgången.</span><span class="sxs-lookup"><span data-stu-id="5799d-158">More than one AssetFile has hello specified name within hello Asset.</span></span>
* <span data-ttu-id="5799d-159">Ett försök gjordes toocreate en andra primära AssetFile inom hello tillgången.</span><span class="sxs-lookup"><span data-stu-id="5799d-159">An attempt was made toocreate a second primary AssetFile within hello Asset.</span></span>
* <span data-ttu-id="5799d-160">Ett försök gjordes toocreate en ContentKey med hello angivna ID: T används redan.</span><span class="sxs-lookup"><span data-stu-id="5799d-160">An attempt was made toocreate a ContentKey with hello specified Id already used.</span></span>
* <span data-ttu-id="5799d-161">Ett försök gjordes toocreate en lokaliserare med hello angivna ID: T används redan.</span><span class="sxs-lookup"><span data-stu-id="5799d-161">An attempt was made toocreate a Locator with hello specified Id already used.</span></span>
* <span data-ttu-id="5799d-162">Mer än en IngestManifestFile har hello angivet namn inom hello IngestManifest.</span><span class="sxs-lookup"><span data-stu-id="5799d-162">More than one IngestManifestFile has hello specified name within hello IngestManifest.</span></span>
* <span data-ttu-id="5799d-163">Ett försök har gjorts toolink en andra storage kryptering ContentKey toohello lagring krypteras tillgången.</span><span class="sxs-lookup"><span data-stu-id="5799d-163">An attempt was made toolink a second storage encryption ContentKey toohello storage-encrypted Asset.</span></span>
* <span data-ttu-id="5799d-164">Ett försök gjordes toolink hello samma ContentKey toohello tillgången.</span><span class="sxs-lookup"><span data-stu-id="5799d-164">An attempt was made toolink hello same ContentKey toohello Asset.</span></span>
* <span data-ttu-id="5799d-165">Ett försök gjordes toocreate en positionerare tooan tillgången vars lagringsbehållaren saknas eller är inte längre som är associerade med hello tillgången.</span><span class="sxs-lookup"><span data-stu-id="5799d-165">An attempt was made toocreate a locator tooan Asset whose storage container is missing or is no longer associated with hello Asset.</span></span>
* <span data-ttu-id="5799d-166">Ett försök har gjorts toocreate en positionerare tooan tillgång som redan har 5 lokaliserare används.</span><span class="sxs-lookup"><span data-stu-id="5799d-166">An attempt was made toocreate a locator tooan Asset which already has 5 locators in use.</span></span> <span data-ttu-id="5799d-167">(Azure Storage tillämpar hello gräns på fem principer för delad åtkomst på en lagringsbehållare.)</span><span class="sxs-lookup"><span data-stu-id="5799d-167">(Azure Storage enforces hello limit of five shared access policies on one storage container.)</span></span>
* <span data-ttu-id="5799d-168">Länka lagringskontot för en tillgång tooan IngestManifestAsset är inte hello samma som hello storage-konto används av hello överordnade IngestManifest.</span><span class="sxs-lookup"><span data-stu-id="5799d-168">Linking storage account of an Asset tooan IngestManifestAsset is not hello same as hello storage account used by hello parent IngestManifest.</span></span>  

## <a name="500-internal-server-error"></a><span data-ttu-id="5799d-169">500 Internt serverfel</span><span class="sxs-lookup"><span data-stu-id="5799d-169">500 Internal Server Error</span></span>
<span data-ttu-id="5799d-170">Under hello bearbetningen av hello begäran påträffar Media Services ett fel som förhindrar hello bearbetningen fortsätter.</span><span class="sxs-lookup"><span data-stu-id="5799d-170">During hello processing of hello request, Media Services encounters some error that prevents hello processing from continuing.</span></span> <span data-ttu-id="5799d-171">Detta kan bero på grund av tooone av hello följande orsaker:</span><span class="sxs-lookup"><span data-stu-id="5799d-171">This could be due tooone of hello following reasons:</span></span>

* <span data-ttu-id="5799d-172">Skapa en tillgång eller jobb misslyckas eftersom hello Media Services konto service-kvotinformation är otillgänglig.</span><span class="sxs-lookup"><span data-stu-id="5799d-172">Creating an Asset or Job fails because hello Media Services account's service quota information is temporarily unavailable.</span></span>
* <span data-ttu-id="5799d-173">Skapa en tillgång eller IngestManifest blobblagringsbehållare misslyckas eftersom hello konto information om lagringskontot är inte tillgänglig för tillfället.</span><span class="sxs-lookup"><span data-stu-id="5799d-173">Creating an Asset or IngestManifest blob storage container fails because hello account's storage account information is temporarily unavailable.</span></span>
* <span data-ttu-id="5799d-174">Ett oväntat fel.</span><span class="sxs-lookup"><span data-stu-id="5799d-174">Other unexpected error.</span></span>

## <a name="503-service-unavailable"></a><span data-ttu-id="5799d-175">503 inte tillgänglig</span><span class="sxs-lookup"><span data-stu-id="5799d-175">503 Service Unavailable</span></span>
<span data-ttu-id="5799d-176">hello-servern är för närvarande inte tooreceive begäranden.</span><span class="sxs-lookup"><span data-stu-id="5799d-176">hello server is currently unable tooreceive requests.</span></span> <span data-ttu-id="5799d-177">Det här felet kan orsakas av hög begäranden toohello service.</span><span class="sxs-lookup"><span data-stu-id="5799d-177">This error may be caused by excessive requests toohello service.</span></span> <span data-ttu-id="5799d-178">Media Services begränsning mekanism begränsar hello resursanvändningen för program som gör överdriven begäran toohello service.</span><span class="sxs-lookup"><span data-stu-id="5799d-178">Media Services throttling mechanism restricts hello resource usage for applications that make excessive request toohello service.</span></span>

> [!NOTE]
> <span data-ttu-id="5799d-179">Kontrollera hello fel meddelande och fel kod sträng tooget mer detaljerad information om hello orsak som du fick hello 503-fel.</span><span class="sxs-lookup"><span data-stu-id="5799d-179">Check hello error message and error code string tooget more detailed information about hello reason you got hello 503 error.</span></span> <span data-ttu-id="5799d-180">Det här felet innebär alltid inte begränsning.</span><span class="sxs-lookup"><span data-stu-id="5799d-180">This error does not always mean throttling.</span></span>
> 
> 

<span data-ttu-id="5799d-181">Möjliga status är:</span><span class="sxs-lookup"><span data-stu-id="5799d-181">Possible status descriptions are:</span></span>

* <span data-ttu-id="5799d-182">”Servern är upptagen.</span><span class="sxs-lookup"><span data-stu-id="5799d-182">"Server is busy.</span></span> <span data-ttu-id="5799d-183">Tidigare körs av denna typ av begäran tog mer än {0} sekunder ”.</span><span class="sxs-lookup"><span data-stu-id="5799d-183">Previous runs of this type of request took more than {0} seconds."</span></span>
* <span data-ttu-id="5799d-184">”Servern är upptagen.</span><span class="sxs-lookup"><span data-stu-id="5799d-184">"Server is busy.</span></span> <span data-ttu-id="5799d-185">Mer än {0}-begäranden per sekund kan vara begränsas ”.</span><span class="sxs-lookup"><span data-stu-id="5799d-185">More than {0} requests per second can be throttled."</span></span>
* <span data-ttu-id="5799d-186">”Servern är upptagen.</span><span class="sxs-lookup"><span data-stu-id="5799d-186">"Server is busy.</span></span> <span data-ttu-id="5799d-187">Fler än {0}-förfrågningar inom {1} sekunder kan begränsas ”.</span><span class="sxs-lookup"><span data-stu-id="5799d-187">More than {0} requests within {1} seconds can be throttled."</span></span>

<span data-ttu-id="5799d-188">toohandle felet, bör du använda exponentiell inte logik.</span><span class="sxs-lookup"><span data-stu-id="5799d-188">toohandle this error, we recommend using exponential back-off retry logic.</span></span> <span data-ttu-id="5799d-189">Som innebär att progressivt längre väntar mellan två på varandra följande felsvar försök.</span><span class="sxs-lookup"><span data-stu-id="5799d-189">That means using progressively longer waits between retries for consecutive error responses.</span></span>  <span data-ttu-id="5799d-190">Mer information finns i [tillfälliga fel hanterar programmet Block](https://msdn.microsoft.com/library/hh680905.aspx).</span><span class="sxs-lookup"><span data-stu-id="5799d-190">For more information, see [Transient Fault Handling Application Block](https://msdn.microsoft.com/library/hh680905.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="5799d-191">Om du använder [Azure Media Services SDK för .net](https://github.com/Azure/azure-sdk-for-media-services/tree/master), hello logik för hello 503-fel har implementerats av hello SDK.</span><span class="sxs-lookup"><span data-stu-id="5799d-191">If you are using [Azure Media Services SDK for .Net](https://github.com/Azure/azure-sdk-for-media-services/tree/master), hello retry logic for hello 503 error has been implemented by hello SDK.</span></span>  
> 
> 

## <a name="see-also"></a><span data-ttu-id="5799d-192">Se även</span><span class="sxs-lookup"><span data-stu-id="5799d-192">See Also</span></span>
[<span data-ttu-id="5799d-193">Media Services Management felkoder</span><span class="sxs-lookup"><span data-stu-id="5799d-193">Media Services Management Error Codes</span></span>](http://msdn.microsoft.com/library/windowsazure/dn167016.aspx)

## <a name="next-steps"></a><span data-ttu-id="5799d-194">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5799d-194">Next steps</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="5799d-195">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="5799d-195">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

