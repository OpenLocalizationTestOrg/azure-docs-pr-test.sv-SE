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
# <a name="connecting-to-media-services-account-using-media-services-rest-api"></a>Ansluter till Media Services-konto med hjälp av Media Services REST API
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-connect-programmatically.md)
> * [REST](media-services-rest-connect-programmatically.md)
> 
> 

Det här avsnittet beskriver hur du skaffar en programmässiga anslutning till Microsoft Azure Media Services vid programmering med Media Services REST API.

Det krävs två saker att få åtkomst till Microsoft Azure Media Services: en åtkomst-token som tillhandahålls av Azure Access Control-tjänster (ACS) och URI Media Services sig själv. Du kan använda andra metoder som du vill använda när du skapar dessa begäranden som du anger rätt huvudvärden och korrekt skicka i åtkomsttoken vid anrop till Media Services.

Följande steg beskriver de vanligaste arbetsflödet när du använder Media Services REST API för att ansluta till Media Services:

1. Hämta en åtkomst-token 
2. Ansluter till Media Services URI 
   
   > [!NOTE]
   > När du har anslutit till https://media.windows.net, får du en 301 omdirigering att ange en annan Media Services-URI. Du måste göra följande anrop till en ny URI.
   > Du kan också få HTTP/1.1 200 svar som innehåller beskrivningen för ODATA-API-metadata.
   > 
   > 
3. Publicera din efterföljande API-anrop till den nya URL. 
   
    Till exempel om när du försöker ansluta har du följande:
   
        HTTP/1.1 301 Moved Permanently
        Location: https://wamsbayclus001rest-hs.cloudapp.net/api/
   
    Du bör publicera dina efterföljande API-anrop till https://wamsbayclus001rest-hs.cloudapp.net/api/.

    >[!NOTE]
    >Det finns en gräns på 1 000 000 principer för olika AMS-principer (till exempel för positionerarprincipen eller ContentKeyAuthorizationPolicy). Du bör använda samma princip-ID om du alltid använder samma dagar/åtkomstbehörigheter, till exempel principer för positionerare som är avsedda att vara på plats under en längre tid (icke-överföringsprinciper). Mer information finns i [detta](media-services-dotnet-manage-entities.md#limit-access-policies) avsnitt.

## <a name="access-control-address"></a>-Adressen
Media Services-adressen är https://wamsprodglobal001acs.accesscontrol.windows.net förutom norra Kina region, där det är https://wamsprodglobal001acs.accesscontrol.chinacloudapi.cn.

## <a name="getting-an-access-token"></a>Hämta en åtkomst-token
Hämta en åtkomst-token från ACS och använder den under varje HTTP-begäran som du gör i tjänsten för att komma åt Media Services direkt via REST API. Denna token liknar andra token som tillhandahålls av ACS baserat på åtkomst anspråk som angetts i rubriken för en HTTP-begäran och använder protokollet OAuth v2. Du behöver inte eventuella andra nödvändiga komponenter innan du ansluter direkt till Media Services.

I följande exempel visar HTTP-begärans rubrik och text som används för att hämta en token.

**Huvudet**:

    POST https://wamsprodglobal001acs.accesscontrol.windows.net/v2/OAuth2-13 HTTP/1.1
    Content-Type: application/x-www-form-urlencoded
    Host: wamsprodglobal001acs.accesscontrol.windows.net
    Content-Length: 120
    Expect: 100-continue
    Connection: Keep-Alive
    Accept: application/json


**Brödtext**:

Du behöver bevisa värdena client_id och client_secret i brödtexten i begäran. client_id och client_secret motsvarar värdena AccountName eller AccountKey. Dessa värden som du av Media Services när du konfigurerar ditt konto. 

Observera att AccountKey för Media Services-kontot måste vara URL-kodade (se [procent-Encoding](http://tools.ietf.org/html/rfc3986#section-2.1) när du använder client_secret värdet i din token åtkomstbegäran.

    grant_type=client_credentials&client_id=ams_account_name&client_secret=URL_encoded_ams_account_key&scope=urn%3aWindowsAzureMediaServices


Exempel: 

    grant_type=client_credentials&client_id=amstestaccount001&client_secret=wUNbKhNj07oqjqU3Ah9R9f4kqTJ9avPpfe6Pk3YZ7ng%3d&scope=urn%3aWindowsAzureMediaServices


I följande exempel visas HTTP-svar som innehåller åtkomsten-token i brödtext för svar.

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
> Det rekommenderas att cachelagra värdena ”access_token” och ”expires_in” till en extern lagring. Token data kan senare hämtas från lagringen och återanvändas i Media Services REST API-anrop. Detta är särskilt användbart för scenarier där token kan delas på ett säkert sätt mellan flera processer eller datorer.
> 
> 

Se till att övervaka ”expires_in”-värdet för den åtkomst-token och uppdatera REST API-anrop med nya token efter behov.

### <a name="connecting-to-the-media-services-uri"></a>Ansluter till Media Services URI
Rot-URI för Media Services är https://media.windows.net/. Du ansluta först till den här URI och om du får en 301 omdirigering tillbaka i svaret, bör du se efterföljande anrop till ny URI. Dessutom inte använda några Följ-auto-omdirigering logik i din begäran. HTTP-verb och begäran organ vidarebefordras inte till en ny URI.

Observera att roten URI för filöverföring tillgångsfiler https://yourstorageaccount.blob.core.windows.net/ där lagringskontonamnet är samma du använde under din kontokonfiguration för Media Services.

Exemplet nedan visar HTTP-begäran till Media Services roten URI (https://media.windows.net/). Begäran hämtar 301 omdirigering tillbaka i svaret. Efterföljande begäran använder nya URI: N (https://wamsbayclus001rest-hs.cloudapp.net/api/).     

**HTTP-begäran**:

    GET https://media.windows.net/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
    x-ms-version: 2.11
    Accept: application/json
    Host: media.windows.net


**HTTP-svar**:

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


**HTTP-begäran** (med ny URI):

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
    x-ms-version: 2.11
    Accept: application/json
    Host: wamsbayclus001rest-hs.cloudapp.net


**HTTP-svar**:

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
> När du har hämtat den nya URI som är den URI som ska användas för att kommunicera med Media Services. 
> 
> 

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

