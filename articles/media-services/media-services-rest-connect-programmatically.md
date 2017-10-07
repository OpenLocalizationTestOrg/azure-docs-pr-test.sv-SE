---
title: "aaaConnecting tooMedia Services-konto med hjälp av REST API | Microsoft Docs"
description: "Det här avsnittet visar hur tooconnect tooMedia tjänster med REST API."
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
ms.openlocfilehash: 1d5064a3612dc96f5c5ad910d183d84fb70a3b6a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connecting-toomedia-services-account-using-media-services-rest-api"></a><span data-ttu-id="b29aa-103">Ansluta tooMedia Services-konto med Media Services REST API</span><span class="sxs-lookup"><span data-stu-id="b29aa-103">Connecting tooMedia Services Account using Media Services REST API</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b29aa-104">.NET</span><span class="sxs-lookup"><span data-stu-id="b29aa-104">.NET</span></span>](media-services-dotnet-connect-programmatically.md)
> * [<span data-ttu-id="b29aa-105">REST</span><span class="sxs-lookup"><span data-stu-id="b29aa-105">REST</span></span>](media-services-rest-connect-programmatically.md)
> 
> 

<span data-ttu-id="b29aa-106">Det här avsnittet beskrivs hur tooobtain programmässiga anslutning-tooMicrosoft Azure Media Services vid programmering med hello Media Services REST API.</span><span class="sxs-lookup"><span data-stu-id="b29aa-106">This topic describes how tooobtain a programmatic connection tooMicrosoft Azure Media Services when you are programming with hello Media Services REST API.</span></span>

<span data-ttu-id="b29aa-107">Det krävs två saker att få åtkomst till Microsoft Azure Media Services: en åtkomst-token som tillhandahålls av Azure Access Control-tjänster (ACS) och hello URI för Media Services sig själv.</span><span class="sxs-lookup"><span data-stu-id="b29aa-107">Two things are required when accessing Microsoft Azure Media Services: An access token provided by Azure Access Control Services (ACS), and hello URI of Media Services itself.</span></span> <span data-ttu-id="b29aa-108">Du kan använda andra metoder som du vill använda när du skapar dessa begäranden som du anger hello rätt huvudvärden och korrekt skicka i hello åtkomsttoken vid anrop till Media Services.</span><span class="sxs-lookup"><span data-stu-id="b29aa-108">You can use any means you want when creating these requests as long as you specify hello correct header values and pass in hello access token correctly when calling into Media Services.</span></span>

<span data-ttu-id="b29aa-109">hello följande steg beskriver hello vanligaste arbetsflödet när med hello Media Services REST API tooconnect tooMedia Services:</span><span class="sxs-lookup"><span data-stu-id="b29aa-109">hello following steps describe hello most common workflow when using hello Media Services REST API tooconnect tooMedia Services:</span></span>

1. <span data-ttu-id="b29aa-110">Hämta en åtkomst-token</span><span class="sxs-lookup"><span data-stu-id="b29aa-110">Getting an access token</span></span> 
2. <span data-ttu-id="b29aa-111">Ansluta toohello Media Services-URI</span><span class="sxs-lookup"><span data-stu-id="b29aa-111">Connecting toohello Media Services URI</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="b29aa-112">När du har anslutit toohttps://media.windows.net, får du en 301 omdirigering att ange en annan Media Services-URI.</span><span class="sxs-lookup"><span data-stu-id="b29aa-112">After successfully connecting toohttps://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="b29aa-113">Du måste göra följande anrop toohello ny URI.</span><span class="sxs-lookup"><span data-stu-id="b29aa-113">You must make subsequent calls toohello new URI.</span></span>
   > <span data-ttu-id="b29aa-114">Du kan också få HTTP/1.1 200 svar som innehåller hello beskrivning för ODATA-API-metadata.</span><span class="sxs-lookup"><span data-stu-id="b29aa-114">You may also receive a HTTP/1.1 200 response that contains hello ODATA API metadata description.</span></span>
   > 
   > 
3. <span data-ttu-id="b29aa-115">Bokför efterföljande API-anrop toohello nya URL: en.</span><span class="sxs-lookup"><span data-stu-id="b29aa-115">Post your subsequent API calls toohello new URL.</span></span> 
   
    <span data-ttu-id="b29aa-116">Till exempel om när du har försökt tooconnect, du har fått hello följande:</span><span class="sxs-lookup"><span data-stu-id="b29aa-116">For example, if after trying tooconnect, you got hello following:</span></span>
   
        HTTP/1.1 301 Moved Permanently
        Location: https://wamsbayclus001rest-hs.cloudapp.net/api/
   
    <span data-ttu-id="b29aa-117">Du bör publicera dina efterföljande API-anrop toohttps://wamsbayclus001rest-hs.cloudapp.net/api/.</span><span class="sxs-lookup"><span data-stu-id="b29aa-117">You should post your subsequent API calls toohttps://wamsbayclus001rest-hs.cloudapp.net/api/.</span></span>

    >[!NOTE]
    ><span data-ttu-id="b29aa-118">Det finns en gräns på 1 000 000 principer för olika AMS-principer (till exempel för positionerarprincipen eller ContentKeyAuthorizationPolicy).</span><span class="sxs-lookup"><span data-stu-id="b29aa-118">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="b29aa-119">Du bör använda hello samma princip-ID om du alltid använder hello samma dagar / åtkomstbehörigheter, till exempel principer för lokaliserare som är avsedda tooremain på plats för lång tid (icke-överföringen principer).</span><span class="sxs-lookup"><span data-stu-id="b29aa-119">You should use hello same policy ID if you are always using hello same days / access permissions, for example, policies for locators that are intended tooremain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="b29aa-120">Mer information finns i [detta](media-services-dotnet-manage-entities.md#limit-access-policies) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="b29aa-120">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>

## <a name="access-control-address"></a><span data-ttu-id="b29aa-121">-Adressen</span><span class="sxs-lookup"><span data-stu-id="b29aa-121">Access control address</span></span>
<span data-ttu-id="b29aa-122">Media Services-adressen är https://wamsprodglobal001acs.accesscontrol.windows.net förutom norra Kina region, där det är https://wamsprodglobal001acs.accesscontrol.chinacloudapi.cn.</span><span class="sxs-lookup"><span data-stu-id="b29aa-122">Media Services access control address is https://wamsprodglobal001acs.accesscontrol.windows.net, except for North China region, where it is https://wamsprodglobal001acs.accesscontrol.chinacloudapi.cn.</span></span>

## <a name="getting-an-access-token"></a><span data-ttu-id="b29aa-123">Hämta en åtkomst-token</span><span class="sxs-lookup"><span data-stu-id="b29aa-123">Getting an access token</span></span>
<span data-ttu-id="b29aa-124">tooaccess Media Services direkt via hello REST-API, hämta en åtkomst-token från ACS och använder den under varje HTTP-begäran som du gör i hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="b29aa-124">tooaccess Media Services directly through hello REST API, retrieve an access token from ACS and use it during every HTTP request you make into hello service.</span></span> <span data-ttu-id="b29aa-125">Denna token är liknande tooother token som tillhandahålls av ACS baserat på åtkomst anspråk levereras i hello-rubriken för en HTTP-begäran och med hello OAuth v2-protokollet.</span><span class="sxs-lookup"><span data-stu-id="b29aa-125">This token is similar tooother tokens provided by ACS based on access claims provided in hello header of an HTTP request and using hello OAuth v2 protocol.</span></span> <span data-ttu-id="b29aa-126">Du behöver inte eventuella andra nödvändiga komponenter innan du ansluter direkt tooMedia tjänster.</span><span class="sxs-lookup"><span data-stu-id="b29aa-126">You do not need any other prerequisites before directly connecting tooMedia Services.</span></span>

<span data-ttu-id="b29aa-127">hello följande exempel visar hello HTTP-begärans rubrik och text används tooretrieve en token.</span><span class="sxs-lookup"><span data-stu-id="b29aa-127">hello following example shows hello HTTP request header and body used tooretrieve a token.</span></span>

<span data-ttu-id="b29aa-128">**Huvudet**:</span><span class="sxs-lookup"><span data-stu-id="b29aa-128">**Header**:</span></span>

    POST https://wamsprodglobal001acs.accesscontrol.windows.net/v2/OAuth2-13 HTTP/1.1
    Content-Type: application/x-www-form-urlencoded
    Host: wamsprodglobal001acs.accesscontrol.windows.net
    Content-Length: 120
    Expect: 100-continue
    Connection: Keep-Alive
    Accept: application/json


<span data-ttu-id="b29aa-129">**Brödtext**:</span><span class="sxs-lookup"><span data-stu-id="b29aa-129">**Body**:</span></span>

<span data-ttu-id="b29aa-130">Du behöver tooprove hello client_id och client_secret värden i hello brödtexten i begäran. client_id och client_secret motsvaras toohello AccountName och AccountKey värden.</span><span class="sxs-lookup"><span data-stu-id="b29aa-130">You need tooprove hello client_id and client_secret values in hello body of this request; client_id and client_secret correspond toohello AccountName and AccountKey values, respectively.</span></span> <span data-ttu-id="b29aa-131">Dessa värden tillhandahålls tooyou av Media Services när du har lagt upp ditt konto.</span><span class="sxs-lookup"><span data-stu-id="b29aa-131">These values are provided tooyou by Media Services when you set up your account.</span></span> 

<span data-ttu-id="b29aa-132">Observera att hello AccountKey för Media Services-kontot måste vara URL-kodade (se [procent-Encoding](http://tools.ietf.org/html/rfc3986#section-2.1) när du använder som hello client_secret värdet i din token åtkomstbegäran.</span><span class="sxs-lookup"><span data-stu-id="b29aa-132">Note that hello AccountKey for your Media Services account must be URL-encoded (see [Percent-Encoding](http://tools.ietf.org/html/rfc3986#section-2.1) when using it as hello client_secret value in your access token request.</span></span>

    grant_type=client_credentials&client_id=ams_account_name&client_secret=URL_encoded_ams_account_key&scope=urn%3aWindowsAzureMediaServices


<span data-ttu-id="b29aa-133">Exempel:</span><span class="sxs-lookup"><span data-stu-id="b29aa-133">For example:</span></span> 

    grant_type=client_credentials&client_id=amstestaccount001&client_secret=wUNbKhNj07oqjqU3Ah9R9f4kqTJ9avPpfe6Pk3YZ7ng%3d&scope=urn%3aWindowsAzureMediaServices


<span data-ttu-id="b29aa-134">hello följande exempel visar hello HTTP-svar som innehåller hello åtkomst-token i hello svarstexten.</span><span class="sxs-lookup"><span data-stu-id="b29aa-134">hello following example shows hello HTTP response that contains hello access token in hello response body.</span></span>

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
> <span data-ttu-id="b29aa-135">Det rekommenderas toocache hello ”access_token” och ”expires_in” värden tooan extern lagring.</span><span class="sxs-lookup"><span data-stu-id="b29aa-135">It is recommended toocache hello "access_token " and "expires_in " values tooan external storage.</span></span> <span data-ttu-id="b29aa-136">hello token data kan senare hämtas från hello lagring och återanvändas i Media Services REST API-anrop.</span><span class="sxs-lookup"><span data-stu-id="b29aa-136">hello token data could later be retrieved from hello storage and re-used in your Media Services REST API calls.</span></span> <span data-ttu-id="b29aa-137">Detta är särskilt användbart för scenarier där hello token kan delas på ett säkert sätt mellan flera processer eller datorer.</span><span class="sxs-lookup"><span data-stu-id="b29aa-137">This is especially useful for scenarios where hello token can be securely shared among multiple processes or computers.</span></span>
> 
> 

<span data-ttu-id="b29aa-138">Kontrollera att toomonitor hello ”expires_in” värdet av hello åtkomst-token och uppdaterar REST API-anrop med nya token efter behov.</span><span class="sxs-lookup"><span data-stu-id="b29aa-138">Make sure toomonitor hello "expires_in" value of hello access token and update your REST API calls with new tokens as needed.</span></span>

### <a name="connecting-toohello-media-services-uri"></a><span data-ttu-id="b29aa-139">Ansluta toohello Media Services-URI</span><span class="sxs-lookup"><span data-stu-id="b29aa-139">Connecting toohello Media Services URI</span></span>
<span data-ttu-id="b29aa-140">hello rot-URI för Media Services är https://media.windows.net/.</span><span class="sxs-lookup"><span data-stu-id="b29aa-140">hello root URI for Media Services is https://media.windows.net/.</span></span> <span data-ttu-id="b29aa-141">Du bör först ansluta toothis URI och om du får en 301 omdirigering tillbaka i svaret, bör du se följande anrop toohello ny URI.</span><span class="sxs-lookup"><span data-stu-id="b29aa-141">You should initially connect toothis URI, and if you get a 301 redirect back in response, you should make subsequent calls toohello new URI.</span></span> <span data-ttu-id="b29aa-142">Dessutom inte använda några Följ-auto-omdirigering logik i din begäran.</span><span class="sxs-lookup"><span data-stu-id="b29aa-142">In addition, do not use any auto-redirect/follow logic in your requests.</span></span> <span data-ttu-id="b29aa-143">HTTP-verb och begäran organ inte vidarebefordras toohello ny URI.</span><span class="sxs-lookup"><span data-stu-id="b29aa-143">HTTP verbs and request bodies will not be forwarded toohello new URI.</span></span>

<span data-ttu-id="b29aa-144">Observera att hello rot URI för filöverföring tillgångsfiler är https://yourstorageaccount.blob.core.windows.net/ där hello lagringskontonamnet hello samma som du använde under din kontokonfiguration för Media Services.</span><span class="sxs-lookup"><span data-stu-id="b29aa-144">Note that hello root URI for uploading and downloading Asset files is https://yourstorageaccount.blob.core.windows.net/ where hello storage account name is hello same one you used during your Media Services account setup.</span></span>

<span data-ttu-id="b29aa-145">hello som följande exempel visar http-begäran toohello Media Services rot URI (https://media.windows.net/).</span><span class="sxs-lookup"><span data-stu-id="b29aa-145">hello following example demonstrates HTTP request toohello Media Services root URI (https://media.windows.net/).</span></span> <span data-ttu-id="b29aa-146">hello begäran hämtar 301 omdirigering tillbaka i svaret.</span><span class="sxs-lookup"><span data-stu-id="b29aa-146">hello request gets a 301 redirect back in response.</span></span> <span data-ttu-id="b29aa-147">hello efterföljande begäran använder hello ny URI (https://wamsbayclus001rest-hs.cloudapp.net/api/).</span><span class="sxs-lookup"><span data-stu-id="b29aa-147">hello subsequent request is using hello new URI (https://wamsbayclus001rest-hs.cloudapp.net/api/).</span></span>     

<span data-ttu-id="b29aa-148">**HTTP-begäran**:</span><span class="sxs-lookup"><span data-stu-id="b29aa-148">**HTTP Request**:</span></span>

    GET https://media.windows.net/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
    x-ms-version: 2.11
    Accept: application/json
    Host: media.windows.net


<span data-ttu-id="b29aa-149">**HTTP-svar**:</span><span class="sxs-lookup"><span data-stu-id="b29aa-149">**HTTP Response**:</span></span>

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
    <h2>Object moved too<a href="https://wamsbayclus001rest-hs.cloudapp.net/api/">here</a>.</h2>
    </body></html>


<span data-ttu-id="b29aa-150">**HTTP-begäran** (med hjälp av hello ny URI):</span><span class="sxs-lookup"><span data-stu-id="b29aa-150">**HTTP Request** (using hello new URI):</span></span>

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
    x-ms-version: 2.11
    Accept: application/json
    Host: wamsbayclus001rest-hs.cloudapp.net


<span data-ttu-id="b29aa-151">**HTTP-svar**:</span><span class="sxs-lookup"><span data-stu-id="b29aa-151">**HTTP Response**:</span></span>

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
> <span data-ttu-id="b29aa-152">När du har hämtat hello nya hello URI som är URI: N som ska använda toocommunicate med Media Services.</span><span class="sxs-lookup"><span data-stu-id="b29aa-152">Once you get hello new URI, that is hello URI that should be used toocommunicate with Media Services.</span></span> 
> 
> 

## <a name="media-services-learning-paths"></a><span data-ttu-id="b29aa-153">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="b29aa-153">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="b29aa-154">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="b29aa-154">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

