---
title: "Ansluter till Media Services-konto med hjälp av REST API | Microsoft Docs"
description: "Det här avsnittet visar hur du ansluter till Media Services med REST API."
services: media-services
documentationcenter: 
author: Juliako
manager: erikre
editor: 
ms.assetid: 79dc64f1-15d8-4a81-b9d9-3d3c44d2e1e8
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 09/26/2016
ms.author: juliako
ms.openlocfilehash: 4feb0eb81823835e8e0b701463d85b27f5598019
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="connecting-to-media-services-account-using-media-services-rest-api"></a><span data-ttu-id="3d11d-103">Ansluter till Media Services-konto med hjälp av Media Services REST API</span><span class="sxs-lookup"><span data-stu-id="3d11d-103">Connecting to Media Services Account using Media Services REST API</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3d11d-104">.NET</span><span class="sxs-lookup"><span data-stu-id="3d11d-104">.NET</span></span>](media-services-dotnet-connect-programmatically.md)
> * [<span data-ttu-id="3d11d-105">REST</span><span class="sxs-lookup"><span data-stu-id="3d11d-105">REST</span></span>](media-services-rest-connect-programmatically.md)
> 
> 

<span data-ttu-id="3d11d-106">Det här avsnittet beskriver hur du skaffar en programmässiga anslutning till Microsoft Azure Media Services vid programmering med Media Services REST API.</span><span class="sxs-lookup"><span data-stu-id="3d11d-106">This topic describes how to obtain a programmatic connection to Microsoft Azure Media Services when you are programming with the Media Services REST API.</span></span>

<span data-ttu-id="3d11d-107">Det krävs två saker att få åtkomst till Microsoft Azure Media Services: en åtkomst-token som tillhandahålls av Azure Access Control-tjänster (ACS) och URI Media Services sig själv.</span><span class="sxs-lookup"><span data-stu-id="3d11d-107">Two things are required when accessing Microsoft Azure Media Services: An access token provided by Azure Access Control Services (ACS), and the URI of Media Services itself.</span></span> <span data-ttu-id="3d11d-108">Du kan använda andra metoder som du vill använda när du skapar dessa begäranden som du anger rätt huvudvärden och korrekt skicka i åtkomsttoken vid anrop till Media Services.</span><span class="sxs-lookup"><span data-stu-id="3d11d-108">You can use any means you want when creating these requests as long as you specify the correct header values and pass in the access token correctly when calling into Media Services.</span></span>

<span data-ttu-id="3d11d-109">Följande steg beskriver de vanligaste arbetsflödet när du använder Media Services REST API för att ansluta till Media Services:</span><span class="sxs-lookup"><span data-stu-id="3d11d-109">The following steps describe the most common workflow when using the Media Services REST API to connect to Media Services:</span></span>

1. <span data-ttu-id="3d11d-110">Hämta en åtkomst-token</span><span class="sxs-lookup"><span data-stu-id="3d11d-110">Getting an access token</span></span> 
2. <span data-ttu-id="3d11d-111">Ansluter till Media Services URI</span><span class="sxs-lookup"><span data-stu-id="3d11d-111">Connecting to the Media Services URI</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="3d11d-112">När du har anslutit till https://media.windows.net, får du en 301 omdirigering att ange en annan Media Services-URI.</span><span class="sxs-lookup"><span data-stu-id="3d11d-112">After successfully connecting to https://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="3d11d-113">Du måste göra följande anrop till en ny URI.</span><span class="sxs-lookup"><span data-stu-id="3d11d-113">You must make subsequent calls to the new URI.</span></span>
   > <span data-ttu-id="3d11d-114">Du kan också få HTTP/1.1 200 svar som innehåller beskrivningen för ODATA-API-metadata.</span><span class="sxs-lookup"><span data-stu-id="3d11d-114">You may also receive a HTTP/1.1 200 response that contains the ODATA API metadata description.</span></span>
   > 
   > 
3. <span data-ttu-id="3d11d-115">Publicera din efterföljande API-anrop till den nya URL.</span><span class="sxs-lookup"><span data-stu-id="3d11d-115">Post your subsequent API calls to the new URL.</span></span> 
   
    <span data-ttu-id="3d11d-116">Till exempel om när du försöker ansluta har du följande:</span><span class="sxs-lookup"><span data-stu-id="3d11d-116">For example, if after trying to connect, you got the following:</span></span>
   
        HTTP/1.1 301 Moved Permanently
        Location: https://wamsbayclus001rest-hs.cloudapp.net/api/
   
    <span data-ttu-id="3d11d-117">Du bör publicera dina efterföljande API-anrop till https://wamsbayclus001rest-hs.cloudapp.net/api/.</span><span class="sxs-lookup"><span data-stu-id="3d11d-117">You should post your subsequent API calls to https://wamsbayclus001rest-hs.cloudapp.net/api/.</span></span>

    >[!NOTE]
    ><span data-ttu-id="3d11d-118">Det finns en gräns på 1 000 000 principer för olika AMS-principer (till exempel för positionerarprincipen eller ContentKeyAuthorizationPolicy).</span><span class="sxs-lookup"><span data-stu-id="3d11d-118">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="3d11d-119">Du bör använda samma princip-ID om du alltid använder samma dagar/åtkomstbehörigheter, till exempel principer för positionerare som är avsedda att vara på plats under en längre tid (icke-överföringsprinciper).</span><span class="sxs-lookup"><span data-stu-id="3d11d-119">You should use the same policy ID if you are always using the same days / access permissions, for example, policies for locators that are intended to remain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="3d11d-120">Mer information finns i [detta](media-services-dotnet-manage-entities.md#limit-access-policies) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="3d11d-120">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>

## <a name="access-control-address"></a><span data-ttu-id="3d11d-121">-Adressen</span><span class="sxs-lookup"><span data-stu-id="3d11d-121">Access control address</span></span>
<span data-ttu-id="3d11d-122">Media Services-adressen är https://wamsprodglobal001acs.accesscontrol.windows.net förutom norra Kina region, där det är https://wamsprodglobal001acs.accesscontrol.chinacloudapi.cn.</span><span class="sxs-lookup"><span data-stu-id="3d11d-122">Media Services access control address is https://wamsprodglobal001acs.accesscontrol.windows.net, except for North China region, where it is https://wamsprodglobal001acs.accesscontrol.chinacloudapi.cn.</span></span>

## <a name="getting-an-access-token"></a><span data-ttu-id="3d11d-123">Hämta en åtkomst-token</span><span class="sxs-lookup"><span data-stu-id="3d11d-123">Getting an access token</span></span>
<span data-ttu-id="3d11d-124">Hämta en åtkomst-token från ACS och använder den under varje HTTP-begäran som du gör i tjänsten för att komma åt Media Services direkt via REST API.</span><span class="sxs-lookup"><span data-stu-id="3d11d-124">To access Media Services directly through the REST API, retrieve an access token from ACS and use it during every HTTP request you make into the service.</span></span> <span data-ttu-id="3d11d-125">Denna token liknar andra token som tillhandahålls av ACS baserat på åtkomst anspråk som angetts i rubriken för en HTTP-begäran och använder protokollet OAuth v2.</span><span class="sxs-lookup"><span data-stu-id="3d11d-125">This token is similar to other tokens provided by ACS based on access claims provided in the header of an HTTP request and using the OAuth v2 protocol.</span></span> <span data-ttu-id="3d11d-126">Du behöver inte eventuella andra nödvändiga komponenter innan du ansluter direkt till Media Services.</span><span class="sxs-lookup"><span data-stu-id="3d11d-126">You do not need any other prerequisites before directly connecting to Media Services.</span></span>

<span data-ttu-id="3d11d-127">I följande exempel visar HTTP-begärans rubrik och text som används för att hämta en token.</span><span class="sxs-lookup"><span data-stu-id="3d11d-127">The following example shows the HTTP request header and body used to retrieve a token.</span></span>

<span data-ttu-id="3d11d-128">**Huvudet**:</span><span class="sxs-lookup"><span data-stu-id="3d11d-128">**Header**:</span></span>

    POST https://wamsprodglobal001acs.accesscontrol.windows.net/v2/OAuth2-13 HTTP/1.1
    Content-Type: application/x-www-form-urlencoded
    Host: wamsprodglobal001acs.accesscontrol.windows.net
    Content-Length: 120
    Expect: 100-continue
    Connection: Keep-Alive
    Accept: application/json


<span data-ttu-id="3d11d-129">**Brödtext**:</span><span class="sxs-lookup"><span data-stu-id="3d11d-129">**Body**:</span></span>

<span data-ttu-id="3d11d-130">Du behöver bevisa värdena client_id och client_secret i brödtexten i begäran. client_id och client_secret motsvarar värdena AccountName eller AccountKey.</span><span class="sxs-lookup"><span data-stu-id="3d11d-130">You need to prove the client_id and client_secret values in the body of this request; client_id and client_secret correspond to the AccountName and AccountKey values, respectively.</span></span> <span data-ttu-id="3d11d-131">Dessa värden som du av Media Services när du konfigurerar ditt konto.</span><span class="sxs-lookup"><span data-stu-id="3d11d-131">These values are provided to you by Media Services when you set up your account.</span></span> 

<span data-ttu-id="3d11d-132">Observera att AccountKey för Media Services-kontot måste vara URL-kodade (se [procent-Encoding](http://tools.ietf.org/html/rfc3986#section-2.1) när du använder client_secret värdet i din token åtkomstbegäran.</span><span class="sxs-lookup"><span data-stu-id="3d11d-132">Note that the AccountKey for your Media Services account must be URL-encoded (see [Percent-Encoding](http://tools.ietf.org/html/rfc3986#section-2.1) when using it as the client_secret value in your access token request.</span></span>

    grant_type=client_credentials&client_id=ams_account_name&client_secret=URL_encoded_ams_account_key&scope=urn%3aWindowsAzureMediaServices


<span data-ttu-id="3d11d-133">Exempel:</span><span class="sxs-lookup"><span data-stu-id="3d11d-133">For example:</span></span> 

    grant_type=client_credentials&client_id=amstestaccount001&client_secret=wUNbKhNj07oqjqU3Ah9R9f4kqTJ9avPpfe6Pk3YZ7ng%3d&scope=urn%3aWindowsAzureMediaServices


<span data-ttu-id="3d11d-134">I följande exempel visas HTTP-svar som innehåller åtkomsten-token i brödtext för svar.</span><span class="sxs-lookup"><span data-stu-id="3d11d-134">The following example shows the HTTP response that contains the access token in the response body.</span></span>

    HTTP/1.1 200 OK
    Cache-Control: no-cache, no-store
    Pragma: no-cache
    Content-Type: application/json; charset=utf-8
    Expires: -1
    request-id: a65150f5-2784-4a01-a4b7-33456187ad83
    X-Content-Type-Options: nosniff
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Thu, 15 Jan 2015 08:07:20 GMT
    Content-Length: 670

    {  
       "token_type":"http://schemas.xmlsoap.org/ws/2009/11/swt-token-profile-1.0",
       "access_token":"http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421330840&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=uf69n82KlqZmkJDNxhJkOxpyIpA2HDyeGUTtSnq1vlE%3d",
       "expires_in":"21600",
       "scope":"urn:WindowsAzureMediaServices"
    }


> [!NOTE]
> <span data-ttu-id="3d11d-135">Det rekommenderas att cachelagra värdena ”access_token” och ”expires_in” till en extern lagring.</span><span class="sxs-lookup"><span data-stu-id="3d11d-135">It is recommended to cache the "access_token " and "expires_in " values to an external storage.</span></span> <span data-ttu-id="3d11d-136">Token data kan senare hämtas från lagringen och återanvändas i Media Services REST API-anrop.</span><span class="sxs-lookup"><span data-stu-id="3d11d-136">The token data could later be retrieved from the storage and re-used in your Media Services REST API calls.</span></span> <span data-ttu-id="3d11d-137">Detta är särskilt användbart för scenarier där token kan delas på ett säkert sätt mellan flera processer eller datorer.</span><span class="sxs-lookup"><span data-stu-id="3d11d-137">This is especially useful for scenarios where the token can be securely shared among multiple processes or computers.</span></span>
> 
> 

<span data-ttu-id="3d11d-138">Se till att övervaka ”expires_in”-värdet för den åtkomst-token och uppdatera REST API-anrop med nya token efter behov.</span><span class="sxs-lookup"><span data-stu-id="3d11d-138">Make sure to monitor the "expires_in" value of the access token and update your REST API calls with new tokens as needed.</span></span>

### <a name="connecting-to-the-media-services-uri"></a><span data-ttu-id="3d11d-139">Ansluter till Media Services URI</span><span class="sxs-lookup"><span data-stu-id="3d11d-139">Connecting to the Media Services URI</span></span>
<span data-ttu-id="3d11d-140">Rot-URI för Media Services är https://media.windows.net/.</span><span class="sxs-lookup"><span data-stu-id="3d11d-140">The root URI for Media Services is https://media.windows.net/.</span></span> <span data-ttu-id="3d11d-141">Du ansluta först till den här URI och om du får en 301 omdirigering tillbaka i svaret, bör du se efterföljande anrop till ny URI.</span><span class="sxs-lookup"><span data-stu-id="3d11d-141">You should initially connect to this URI, and if you get a 301 redirect back in response, you should make subsequent calls to the new URI.</span></span> <span data-ttu-id="3d11d-142">Dessutom inte använda några Följ-auto-omdirigering logik i din begäran.</span><span class="sxs-lookup"><span data-stu-id="3d11d-142">In addition, do not use any auto-redirect/follow logic in your requests.</span></span> <span data-ttu-id="3d11d-143">HTTP-verb och begäran organ vidarebefordras inte till en ny URI.</span><span class="sxs-lookup"><span data-stu-id="3d11d-143">HTTP verbs and request bodies will not be forwarded to the new URI.</span></span>

<span data-ttu-id="3d11d-144">Observera att roten URI för filöverföring tillgångsfiler https://yourstorageaccount.blob.core.windows.net/ där lagringskontonamnet är samma du använde under din kontokonfiguration för Media Services.</span><span class="sxs-lookup"><span data-stu-id="3d11d-144">Note that the root URI for uploading and downloading Asset files is https://yourstorageaccount.blob.core.windows.net/ where the storage account name is the same one you used during your Media Services account setup.</span></span>

<span data-ttu-id="3d11d-145">Exemplet nedan visar HTTP-begäran till Media Services roten URI (https://media.windows.net/).</span><span class="sxs-lookup"><span data-stu-id="3d11d-145">The following example demonstrates HTTP request to the Media Services root URI (https://media.windows.net/).</span></span> <span data-ttu-id="3d11d-146">Begäran hämtar 301 omdirigering tillbaka i svaret.</span><span class="sxs-lookup"><span data-stu-id="3d11d-146">The request gets a 301 redirect back in response.</span></span> <span data-ttu-id="3d11d-147">Efterföljande begäran använder nya URI: N (https://wamsbayclus001rest-hs.cloudapp.net/api/).</span><span class="sxs-lookup"><span data-stu-id="3d11d-147">The subsequent request is using the new URI (https://wamsbayclus001rest-hs.cloudapp.net/api/).</span></span>     

<span data-ttu-id="3d11d-148">**HTTP-begäran**:</span><span class="sxs-lookup"><span data-stu-id="3d11d-148">**HTTP Request**:</span></span>

    GET https://media.windows.net/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
    x-ms-version: 2.11
    Accept: application/json
    Host: media.windows.net


<span data-ttu-id="3d11d-149">**HTTP-svar**:</span><span class="sxs-lookup"><span data-stu-id="3d11d-149">**HTTP Response**:</span></span>

    HTTP/1.1 301 Moved Permanently
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/
    Server: Microsoft-IIS/8.5
    request-id: 987d7652-497a-44e5-b815-f492e02aef97
    x-ms-request-id: 987d7652-497a-44e5-b815-f492e02aef97
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sat, 17 Jan 2015 07:44:53 GMT
    Content-Length: 164

    <html><head><title>Object moved</title></head><body>
    <h2>Object moved to <a href="https://wamsbayclus001rest-hs.cloudapp.net/api/">here</a>.</h2>
    </body></html>


<span data-ttu-id="3d11d-150">**HTTP-begäran** (med ny URI):</span><span class="sxs-lookup"><span data-stu-id="3d11d-150">**HTTP Request** (using the new URI):</span></span>

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
    x-ms-version: 2.11
    Accept: application/json
    Host: wamsbayclus001rest-hs.cloudapp.net


<span data-ttu-id="3d11d-151">**HTTP-svar**:</span><span class="sxs-lookup"><span data-stu-id="3d11d-151">**HTTP Response**:</span></span>

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 1250
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 5f52ea9d-177e-48fe-9709-24953d19f84a
    x-ms-request-id: 5f52ea9d-177e-48fe-9709-24953d19f84a
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sat, 17 Jan 2015 07:44:52 GMT

    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata","value":[{"name":"AccessPolicies","url":"AccessPolicies"},{"name":"Locators","url":"Locators"},{"name":"ContentKeys","url":"ContentKeys"},{"name":"ContentKeyAuthorizationPolicyOptions","url":"ContentKeyAuthorizationPolicyOptions"},{"name":"ContentKeyAuthorizationPolicies","url":"ContentKeyAuthorizationPolicies"},{"name":"Files","url":"Files"},{"name":"Assets","url":"Assets"},{"name":"AssetDeliveryPolicies","url":"AssetDeliveryPolicies"},{"name":"IngestManifestFiles","url":"IngestManifestFiles"},{"name":"IngestManifestAssets","url":"IngestManifestAssets"},{"name":"IngestManifests","url":"IngestManifests"},{"name":"StorageAccounts","url":"StorageAccounts"},{"name":"Tasks","url":"Tasks"},{"name":"NotificationEndPoints","url":"NotificationEndPoints"},{"name":"Jobs","url":"Jobs"},{"name":"TaskTemplates","url":"TaskTemplates"},{"name":"JobTemplates","url":"JobTemplates"},{"name":"MediaProcessors","url":"MediaProcessors"},{"name":"EncodingReservedUnitTypes","url":"EncodingReservedUnitTypes"},{"name":"Operations","url":"Operations"},{"name":"StreamingEndpoints","url":"StreamingEndpoints"},{"name":"Channels","url":"Channels"},{"name":"Programs","url":"Programs"}]}



> [!NOTE]
> <span data-ttu-id="3d11d-152">När du har hämtat den nya URI som är den URI som ska användas för att kommunicera med Media Services.</span><span class="sxs-lookup"><span data-stu-id="3d11d-152">Once you get the new URI, that is the URI that should be used to communicate with Media Services.</span></span> 
> 
> 

## <a name="media-services-learning-paths"></a><span data-ttu-id="3d11d-153">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="3d11d-153">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="3d11d-154">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="3d11d-154">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

