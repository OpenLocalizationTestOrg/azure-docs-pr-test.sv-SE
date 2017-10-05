---
title: Azure Media Services-felkoder | Microsoft Docs
description: "Avsnittet ger en översikt över Azure Media Services-felkoder."
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
ms.openlocfilehash: 39886a955124429302609dd9d5a7c20ae7f498d9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="azure-media-services-error-codes"></a><span data-ttu-id="51b1a-103">Azure Media Services-felkoder</span><span class="sxs-lookup"><span data-stu-id="51b1a-103">Azure Media Services error codes</span></span>
<span data-ttu-id="51b1a-104">När du använder Microsoft Azure Media Services kan du få HTTP felkoder från beroende på problem som till exempel autentiseringstoken ut till åtgärder som inte stöds i Media Services-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="51b1a-104">When using Microsoft Azure Media Services, you may receive HTTP error codes from the service depending on issues such as authentication tokens expiring to actions that are not supported in Media Services.</span></span> <span data-ttu-id="51b1a-105">Följande är en lista över **HTTP felkoder** som kan returneras av Media Services och möjliga orsaker för dessa.</span><span class="sxs-lookup"><span data-stu-id="51b1a-105">The following is a list of **HTTP error codes** that may be returned by Media Services and the possible causes for them.</span></span>  

## <a name="400-bad-request"></a><span data-ttu-id="51b1a-106">400 Felaktig förfrågan</span><span class="sxs-lookup"><span data-stu-id="51b1a-106">400 Bad Request</span></span>
<span data-ttu-id="51b1a-107">Begäran innehåller ogiltig information och avvisas på grund av följande orsaker:</span><span class="sxs-lookup"><span data-stu-id="51b1a-107">The request contains invalid information and is rejected due to one of the following reasons:</span></span>

* <span data-ttu-id="51b1a-108">En API-version som inte stöds har angetts.</span><span class="sxs-lookup"><span data-stu-id="51b1a-108">An unsupported API version is specified.</span></span> <span data-ttu-id="51b1a-109">Den senaste versionen finns [installationsprogrammet för Media Services REST API-utveckling](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="51b1a-109">For the most current version, see [Setup for Media Services REST API Development](media-services-rest-how-to-use.md).</span></span>
* <span data-ttu-id="51b1a-110">API-versionen av Media Services har inte angetts.</span><span class="sxs-lookup"><span data-stu-id="51b1a-110">The API version of Media Services is not specified.</span></span> <span data-ttu-id="51b1a-111">Information om hur du anger den API-versionen finns i [Media Services Operations REST API-referens](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference).</span><span class="sxs-lookup"><span data-stu-id="51b1a-111">For information on how to specify the API version, see [Media Services Operations REST API Reference](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference).</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="51b1a-112">Om du använder .NET eller Java SDK för att ansluta till Media Services har API-versionen angetts för dig när du försöker och utföra vissa åtgärder mot Media Services.</span><span class="sxs-lookup"><span data-stu-id="51b1a-112">If you are using the .NET or Java SDKs to connect to Media Services, the API version is specified for you whenever you try and perform some action against Media Services.</span></span>
  > 
  > 
* <span data-ttu-id="51b1a-113">En odefinierad egenskap har angetts.</span><span class="sxs-lookup"><span data-stu-id="51b1a-113">An undefined property has been specified.</span></span> <span data-ttu-id="51b1a-114">Egenskapsnamnet är i felmeddelandet.</span><span class="sxs-lookup"><span data-stu-id="51b1a-114">The property name is in the error message.</span></span> <span data-ttu-id="51b1a-115">Endast de egenskaper som är medlemmar i en given entitet kan anges.</span><span class="sxs-lookup"><span data-stu-id="51b1a-115">Only those properties that are members of a given entity can be specified.</span></span> <span data-ttu-id="51b1a-116">Se [Azure Media Services REST API-referens](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference) för en lista över enheter och deras egenskaper.</span><span class="sxs-lookup"><span data-stu-id="51b1a-116">See [Azure Media Services REST API Reference](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference) for a list of entities and their properties.</span></span>
* <span data-ttu-id="51b1a-117">Ett ogiltigt egenskapsvärde har angetts.</span><span class="sxs-lookup"><span data-stu-id="51b1a-117">An invalid property value has been specified.</span></span> <span data-ttu-id="51b1a-118">Egenskapsnamnet är i felmeddelandet.</span><span class="sxs-lookup"><span data-stu-id="51b1a-118">The property name is in the error message.</span></span> <span data-ttu-id="51b1a-119">Se föregående länk för giltig egenskapstyperna och deras värden.</span><span class="sxs-lookup"><span data-stu-id="51b1a-119">See the previous link for valid property types and their values.</span></span>
* <span data-ttu-id="51b1a-120">Ett värde för egenskapen saknas och måste anges.</span><span class="sxs-lookup"><span data-stu-id="51b1a-120">A property value is missing and is required.</span></span>
* <span data-ttu-id="51b1a-121">En del av Webbadressen som angetts innehåller ett felaktigt värde.</span><span class="sxs-lookup"><span data-stu-id="51b1a-121">Part of the URL specified contains a bad value.</span></span>
* <span data-ttu-id="51b1a-122">Ett försök gjordes att uppdatera en WriteOnce-egenskap.</span><span class="sxs-lookup"><span data-stu-id="51b1a-122">An attempt was made to update a WriteOnce property.</span></span>
* <span data-ttu-id="51b1a-123">Ett försök gjordes att skapa ett jobb som har en inkommande tillgång med en primär AssetFile som angavs inte eller kunde inte fastställas.</span><span class="sxs-lookup"><span data-stu-id="51b1a-123">An attempt was made to create a Job that has an input Asset with a primary AssetFile that was not specified or could not be determined.</span></span>
* <span data-ttu-id="51b1a-124">Ett försök gjordes att uppdatera en SAS-lokaliserare.</span><span class="sxs-lookup"><span data-stu-id="51b1a-124">An attempt was made to update a SAS Locator.</span></span> <span data-ttu-id="51b1a-125">SAS-positionerare kan endast skapas eller tas bort.</span><span class="sxs-lookup"><span data-stu-id="51b1a-125">SAS locators can only be created or deleted.</span></span> <span data-ttu-id="51b1a-126">Strömning positionerare kan uppdateras.</span><span class="sxs-lookup"><span data-stu-id="51b1a-126">Streaming locators can be updated.</span></span> <span data-ttu-id="51b1a-127">Mer information finns i [lokaliserare](https://docs.microsoft.com/rest/api/media/operations/locator).</span><span class="sxs-lookup"><span data-stu-id="51b1a-127">For more information, see [Locators](https://docs.microsoft.com/rest/api/media/operations/locator).</span></span>
* <span data-ttu-id="51b1a-128">En åtgärd som inte stöds eller en fråga skickades.</span><span class="sxs-lookup"><span data-stu-id="51b1a-128">An unsupported operation or query was submitted.</span></span>

## <a name="401-unauthorized"></a><span data-ttu-id="51b1a-129">401 obehörig</span><span class="sxs-lookup"><span data-stu-id="51b1a-129">401 Unauthorized</span></span>
<span data-ttu-id="51b1a-130">Begäran kunde inte autentiseras (innan den kan godkännas) på grund av följande orsaker:</span><span class="sxs-lookup"><span data-stu-id="51b1a-130">The request could not be authenticated (before it can be authorized) due to one of the following reasons:</span></span>

* <span data-ttu-id="51b1a-131">Autentiseringshuvud saknas.</span><span class="sxs-lookup"><span data-stu-id="51b1a-131">Missing authentication header.</span></span>
* <span data-ttu-id="51b1a-132">Felaktig autentisering huvudvärde.</span><span class="sxs-lookup"><span data-stu-id="51b1a-132">Bad authentication header value.</span></span>
  * <span data-ttu-id="51b1a-133">Token har upphört att gälla.</span><span class="sxs-lookup"><span data-stu-id="51b1a-133">The token has expired.</span></span> 
  * <span data-ttu-id="51b1a-134">Token som innehåller en ogiltig signatur.</span><span class="sxs-lookup"><span data-stu-id="51b1a-134">The token contains an invalid signature.</span></span>

## <a name="403-forbidden"></a><span data-ttu-id="51b1a-135">403 Nekad</span><span class="sxs-lookup"><span data-stu-id="51b1a-135">403 Forbidden</span></span>
<span data-ttu-id="51b1a-136">Begäran tillåts inte på grund av följande orsaker:</span><span class="sxs-lookup"><span data-stu-id="51b1a-136">The request is not allowed due to one of the following reasons:</span></span>

* <span data-ttu-id="51b1a-137">Media Services-konto kan inte hittas eller har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="51b1a-137">The Media Services account cannot be found or has been deleted.</span></span>
* <span data-ttu-id="51b1a-138">Media Services-kontot är inaktiverat och typ av begäran är inte HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="51b1a-138">The Media Services account is disabled and the request type is not HTTP GET.</span></span> <span data-ttu-id="51b1a-139">Tjänståtgärder returneras ett 403 svar.</span><span class="sxs-lookup"><span data-stu-id="51b1a-139">Service operations will return a 403 response as well.</span></span>
* <span data-ttu-id="51b1a-140">Autentiseringstoken innehåller inte information om användarens autentiseringsuppgifter: AccountName och/eller prenumerations-ID.</span><span class="sxs-lookup"><span data-stu-id="51b1a-140">The authentication token does not contain the user’s credential information: AccountName and/or SubscriptionId.</span></span> <span data-ttu-id="51b1a-141">Du hittar den här informationen i Användargränssnittet för Media Services-tillägget för ditt Media Services-konto i Azure-hanteringsportalen.</span><span class="sxs-lookup"><span data-stu-id="51b1a-141">You can find this information in the Media Services UI extension for your Media Services account in the Azure Management Portal.</span></span>
* <span data-ttu-id="51b1a-142">Resursen kan inte nås.</span><span class="sxs-lookup"><span data-stu-id="51b1a-142">The resource cannot be accessed.</span></span>
  
  * <span data-ttu-id="51b1a-143">Ett försök gjordes att använda en MediaProcessor som inte är tillgängligt för Media Services-kontot.</span><span class="sxs-lookup"><span data-stu-id="51b1a-143">An attempt was made to use a MediaProcessor that is not available for your Media Services account.</span></span>
  * <span data-ttu-id="51b1a-144">Ett försök gjordes att uppdatera en jobbmall som definierats av Media Services.</span><span class="sxs-lookup"><span data-stu-id="51b1a-144">An attempt was made to update a JobTemplate defined by Media Services.</span></span>
  * <span data-ttu-id="51b1a-145">Ett försök gjordes att skriva över vissa andra Media Services kontots lokaliserare.</span><span class="sxs-lookup"><span data-stu-id="51b1a-145">An attempt was made to overwrite some other Media Services account's Locator.</span></span>
  * <span data-ttu-id="51b1a-146">Ett försök gjordes att skriva över vissa andra Media Services kontots ContentKey.</span><span class="sxs-lookup"><span data-stu-id="51b1a-146">An attempt was made to overwrite some other Media Services account's ContentKey.</span></span>
* <span data-ttu-id="51b1a-147">Det gick inte att skapa resursen på grund av en tjänst-kvot har uppnåtts för Media Services-kontot.</span><span class="sxs-lookup"><span data-stu-id="51b1a-147">The resource could not be created due to a service quota that was reached for the Media Services account.</span></span> <span data-ttu-id="51b1a-148">Mer information om tjänsten kvoter finns [kvoter och begränsningar](media-services-quotas-and-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="51b1a-148">For more information on the service quotas, see [Quotas and Limitations](media-services-quotas-and-limitations.md).</span></span>

## <a name="404-not-found"></a><span data-ttu-id="51b1a-149">404 Hittades inte</span><span class="sxs-lookup"><span data-stu-id="51b1a-149">404 Not Found</span></span>
<span data-ttu-id="51b1a-150">Begäran är inte tillåten på en resurs på grund av följande orsaker:</span><span class="sxs-lookup"><span data-stu-id="51b1a-150">The request is not allowed on a resource due to one of the following reasons:</span></span>

* <span data-ttu-id="51b1a-151">Ett försök gjordes att uppdatera en entitet som inte finns.</span><span class="sxs-lookup"><span data-stu-id="51b1a-151">An attempt was made to update an entity that does not exist.</span></span>
* <span data-ttu-id="51b1a-152">Ett försök gjordes att ta bort en entitet som inte finns.</span><span class="sxs-lookup"><span data-stu-id="51b1a-152">An attempt was made to delete an entity that does not exist.</span></span>
* <span data-ttu-id="51b1a-153">Ett försök gjordes att skapa en entitet som länkar till en entitet som inte finns.</span><span class="sxs-lookup"><span data-stu-id="51b1a-153">An attempt was made to create an entity that links to an entity that does not exist.</span></span>
* <span data-ttu-id="51b1a-154">Ett försök gjordes att hämta en entitet som inte finns.</span><span class="sxs-lookup"><span data-stu-id="51b1a-154">An attempt was made to GET an entity that does not exist.</span></span>
* <span data-ttu-id="51b1a-155">Ett försök gjordes att ange ett lagringskonto som inte är associerad med Media Services-kontot.</span><span class="sxs-lookup"><span data-stu-id="51b1a-155">An attempt was made to specify a storage account that is not associated with the Media Services account.</span></span>  

## <a name="409-conflict"></a><span data-ttu-id="51b1a-156">409 konflikt</span><span class="sxs-lookup"><span data-stu-id="51b1a-156">409 Conflict</span></span>
<span data-ttu-id="51b1a-157">Begäran tillåts inte på grund av följande orsaker:</span><span class="sxs-lookup"><span data-stu-id="51b1a-157">The request is not allowed due to one of the following reasons:</span></span>

* <span data-ttu-id="51b1a-158">Mer än en AssetFile har det angivna namnet i tillgången.</span><span class="sxs-lookup"><span data-stu-id="51b1a-158">More than one AssetFile has the specified name within the Asset.</span></span>
* <span data-ttu-id="51b1a-159">Ett försök gjordes att skapa en andra primära AssetFile i tillgången.</span><span class="sxs-lookup"><span data-stu-id="51b1a-159">An attempt was made to create a second primary AssetFile within the Asset.</span></span>
* <span data-ttu-id="51b1a-160">Ett försök gjordes att skapa en ContentKey med angivet Id används redan.</span><span class="sxs-lookup"><span data-stu-id="51b1a-160">An attempt was made to create a ContentKey with the specified Id already used.</span></span>
* <span data-ttu-id="51b1a-161">Ett försök gjordes att skapa en lokaliserare med angivet Id används redan.</span><span class="sxs-lookup"><span data-stu-id="51b1a-161">An attempt was made to create a Locator with the specified Id already used.</span></span>
* <span data-ttu-id="51b1a-162">Mer än en IngestManifestFile har det angivna namnet i IngestManifest.</span><span class="sxs-lookup"><span data-stu-id="51b1a-162">More than one IngestManifestFile has the specified name within the IngestManifest.</span></span>
* <span data-ttu-id="51b1a-163">Ett försök gjordes att länka en andra lagringskryptering ContentKey till tillgången lagring krypteras.</span><span class="sxs-lookup"><span data-stu-id="51b1a-163">An attempt was made to link a second storage encryption ContentKey to the storage-encrypted Asset.</span></span>
* <span data-ttu-id="51b1a-164">Ett försök gjordes att länka samma ContentKey till tillgången.</span><span class="sxs-lookup"><span data-stu-id="51b1a-164">An attempt was made to link the same ContentKey to the Asset.</span></span>
* <span data-ttu-id="51b1a-165">Ett försök gjordes att skapa en positionerare till en tillgång vars lagringsbehållaren saknas eller är inte längre kopplade till tillgången.</span><span class="sxs-lookup"><span data-stu-id="51b1a-165">An attempt was made to create a locator to an Asset whose storage container is missing or is no longer associated with the Asset.</span></span>
* <span data-ttu-id="51b1a-166">Ett försök gjordes att skapa en positionerare till en tillgång som redan har 5 lokaliserare används.</span><span class="sxs-lookup"><span data-stu-id="51b1a-166">An attempt was made to create a locator to an Asset which already has 5 locators in use.</span></span> <span data-ttu-id="51b1a-167">(Azure Storage tillämpar högst fem principer för delad åtkomst på en lagringsbehållare.)</span><span class="sxs-lookup"><span data-stu-id="51b1a-167">(Azure Storage enforces the limit of five shared access policies on one storage container.)</span></span>
* <span data-ttu-id="51b1a-168">Länka storage-konto för en tillgång till en IngestManifestAsset är inte samma som det lagringskontot som används av överordnat IngestManifest.</span><span class="sxs-lookup"><span data-stu-id="51b1a-168">Linking storage account of an Asset to an IngestManifestAsset is not the same as the storage account used by the parent IngestManifest.</span></span>  

## <a name="500-internal-server-error"></a><span data-ttu-id="51b1a-169">500 Internt serverfel</span><span class="sxs-lookup"><span data-stu-id="51b1a-169">500 Internal Server Error</span></span>
<span data-ttu-id="51b1a-170">Media Services påträffar ett fel som förhindrar bearbetningen fortsätter under bearbetning av begäran.</span><span class="sxs-lookup"><span data-stu-id="51b1a-170">During the processing of the request, Media Services encounters some error that prevents the processing from continuing.</span></span> <span data-ttu-id="51b1a-171">Detta kan bero på något av följande:</span><span class="sxs-lookup"><span data-stu-id="51b1a-171">This could be due to one of the following reasons:</span></span>

* <span data-ttu-id="51b1a-172">Skapa en tillgång eller jobb misslyckas eftersom Media Services-kontot service-kvotinformation är otillgänglig.</span><span class="sxs-lookup"><span data-stu-id="51b1a-172">Creating an Asset or Job fails because the Media Services account's service quota information is temporarily unavailable.</span></span>
* <span data-ttu-id="51b1a-173">Skapa en tillgång eller IngestManifest blobblagringsbehållare misslyckas eftersom kontots information om lagringskontot är inte tillgänglig för tillfället.</span><span class="sxs-lookup"><span data-stu-id="51b1a-173">Creating an Asset or IngestManifest blob storage container fails because the account's storage account information is temporarily unavailable.</span></span>
* <span data-ttu-id="51b1a-174">Ett oväntat fel.</span><span class="sxs-lookup"><span data-stu-id="51b1a-174">Other unexpected error.</span></span>

## <a name="503-service-unavailable"></a><span data-ttu-id="51b1a-175">503 inte tillgänglig</span><span class="sxs-lookup"><span data-stu-id="51b1a-175">503 Service Unavailable</span></span>
<span data-ttu-id="51b1a-176">Servern är för närvarande inte att ta emot begäranden.</span><span class="sxs-lookup"><span data-stu-id="51b1a-176">The server is currently unable to receive requests.</span></span> <span data-ttu-id="51b1a-177">Det här felet kan orsakas av långa förfrågningar till tjänsten.</span><span class="sxs-lookup"><span data-stu-id="51b1a-177">This error may be caused by excessive requests to the service.</span></span> <span data-ttu-id="51b1a-178">Media Services begränsning mekanism begränsar resursanvändningen för program som gör överdriven begäran för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="51b1a-178">Media Services throttling mechanism restricts the resource usage for applications that make excessive request to the service.</span></span>

> [!NOTE]
> <span data-ttu-id="51b1a-179">Kontrollera felmeddelandet och felsträng kod för att få mer detaljerad information om orsaken till att du har fått 503-felet.</span><span class="sxs-lookup"><span data-stu-id="51b1a-179">Check the error message and error code string to get more detailed information about the reason you got the 503 error.</span></span> <span data-ttu-id="51b1a-180">Det här felet innebär alltid inte begränsning.</span><span class="sxs-lookup"><span data-stu-id="51b1a-180">This error does not always mean throttling.</span></span>
> 
> 

<span data-ttu-id="51b1a-181">Möjliga status är:</span><span class="sxs-lookup"><span data-stu-id="51b1a-181">Possible status descriptions are:</span></span>

* <span data-ttu-id="51b1a-182">”Servern är upptagen.</span><span class="sxs-lookup"><span data-stu-id="51b1a-182">"Server is busy.</span></span> <span data-ttu-id="51b1a-183">Tidigare körs av denna typ av begäran tog mer än {0} sekunder ”.</span><span class="sxs-lookup"><span data-stu-id="51b1a-183">Previous runs of this type of request took more than {0} seconds."</span></span>
* <span data-ttu-id="51b1a-184">”Servern är upptagen.</span><span class="sxs-lookup"><span data-stu-id="51b1a-184">"Server is busy.</span></span> <span data-ttu-id="51b1a-185">Mer än {0}-begäranden per sekund kan vara begränsas ”.</span><span class="sxs-lookup"><span data-stu-id="51b1a-185">More than {0} requests per second can be throttled."</span></span>
* <span data-ttu-id="51b1a-186">”Servern är upptagen.</span><span class="sxs-lookup"><span data-stu-id="51b1a-186">"Server is busy.</span></span> <span data-ttu-id="51b1a-187">Fler än {0}-förfrågningar inom {1} sekunder kan begränsas ”.</span><span class="sxs-lookup"><span data-stu-id="51b1a-187">More than {0} requests within {1} seconds can be throttled."</span></span>

<span data-ttu-id="51b1a-188">Om du vill hantera det här felet, bör du använda exponentiell inte logik.</span><span class="sxs-lookup"><span data-stu-id="51b1a-188">To handle this error, we recommend using exponential back-off retry logic.</span></span> <span data-ttu-id="51b1a-189">Som innebär att progressivt längre väntar mellan två på varandra följande felsvar försök.</span><span class="sxs-lookup"><span data-stu-id="51b1a-189">That means using progressively longer waits between retries for consecutive error responses.</span></span>  <span data-ttu-id="51b1a-190">Mer information finns i [tillfälliga fel hanterar programmet Block](https://msdn.microsoft.com/library/hh680905.aspx).</span><span class="sxs-lookup"><span data-stu-id="51b1a-190">For more information, see [Transient Fault Handling Application Block](https://msdn.microsoft.com/library/hh680905.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="51b1a-191">Om du använder [Azure Media Services SDK för .net](https://github.com/Azure/azure-sdk-for-media-services/tree/master), logik för omprövning för 503-fel har implementerats av SDK.</span><span class="sxs-lookup"><span data-stu-id="51b1a-191">If you are using [Azure Media Services SDK for .Net](https://github.com/Azure/azure-sdk-for-media-services/tree/master), the retry logic for the 503 error has been implemented by the SDK.</span></span>  
> 
> 

## <a name="see-also"></a><span data-ttu-id="51b1a-192">Se även</span><span class="sxs-lookup"><span data-stu-id="51b1a-192">See Also</span></span>
[<span data-ttu-id="51b1a-193">Media Services Management felkoder</span><span class="sxs-lookup"><span data-stu-id="51b1a-193">Media Services Management Error Codes</span></span>](http://msdn.microsoft.com/library/windowsazure/dn167016.aspx)

## <a name="next-steps"></a><span data-ttu-id="51b1a-194">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="51b1a-194">Next steps</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="51b1a-195">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="51b1a-195">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

