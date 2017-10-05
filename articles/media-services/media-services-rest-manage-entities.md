---
title: Hantera Media Services entiteter med REST | Microsoft Docs
description: "Lär dig hur du hanterar Media Services entiteter med REST API."
author: juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: 95262a32-0f2a-4286-b9e2-1a1ca6399b5b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: juliako
ms.openlocfilehash: a336907b605da962f835b8057ac6071f480cd85e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="managing-media-services-entities-with-rest"></a><span data-ttu-id="0489d-103">Hantera Media Services entiteter med REST</span><span class="sxs-lookup"><span data-stu-id="0489d-103">Managing Media Services entities with REST</span></span> 
> [!div class="op_single_selector"]
> * [<span data-ttu-id="0489d-104">REST</span><span class="sxs-lookup"><span data-stu-id="0489d-104">REST</span></span>](media-services-rest-manage-entities.md)
> * [<span data-ttu-id="0489d-105">.NET</span><span class="sxs-lookup"><span data-stu-id="0489d-105">.NET</span></span>](media-services-dotnet-manage-entities.md)
> 
> 

<span data-ttu-id="0489d-106">Microsoft Azure Media Services är en REST-baserad tjänst som bygger på OData v3.</span><span class="sxs-lookup"><span data-stu-id="0489d-106">Microsoft Azure Media Services is a REST-based service built on OData v3.</span></span> <span data-ttu-id="0489d-107">Du kan lägga till, fråga, uppdatera och ta bort enheter på ungefär samma sätt som på andra OData-tjänst.</span><span class="sxs-lookup"><span data-stu-id="0489d-107">You can add, query, update, and delete entities in much the same way as you can on any other OData service.</span></span> <span data-ttu-id="0489d-108">Undantag kallas när så är tillämpligt.</span><span class="sxs-lookup"><span data-stu-id="0489d-108">Exceptions will be called out when applicable.</span></span> <span data-ttu-id="0489d-109">Mer information om OData finns [dokumentation Open Data Protocol](http://www.odata.org/documentation/).</span><span class="sxs-lookup"><span data-stu-id="0489d-109">For more information on OData, see [Open Data Protocol documentation](http://www.odata.org/documentation/).</span></span>

<span data-ttu-id="0489d-110">Det här avsnittet visar hur du hanterar Azure Media Services entiteter med REST.</span><span class="sxs-lookup"><span data-stu-id="0489d-110">This topic shows you how to manage Azure Media Services entities with REST.</span></span>

>[!NOTE]
> <span data-ttu-id="0489d-111">Från och med 1 april 2017 raderas alla jobbposter i ditt konto som är äldre än 90 dagar automatiskt, tillsammans med deras associerade uppgiftsposter, även om det totala antalet poster är lägre än den maximala kvoten.</span><span class="sxs-lookup"><span data-stu-id="0489d-111">Starting April 1, 2017, any Job record in your account older than 90 days will be automatically deleted, along with its associated Task records, even if the total number of records is below the maximum quota.</span></span> <span data-ttu-id="0489d-112">Till exempel på 1 April 2017 tas alla jobb poster i ditt konto som är äldre än den 31 December 2016 automatiskt bort.</span><span class="sxs-lookup"><span data-stu-id="0489d-112">For example, on April 1, 2017, any Job record in your account older than December 31, 2016, will be automatically deleted.</span></span> <span data-ttu-id="0489d-113">Du kan använda koden som beskrivs i det här avsnittet om du behöver Arkivera jobb/aktivitetsinformationen.</span><span class="sxs-lookup"><span data-stu-id="0489d-113">If you need to archive the job/task information, you can use the code described in this topic.</span></span>

## <a name="considerations"></a><span data-ttu-id="0489d-114">Överväganden</span><span class="sxs-lookup"><span data-stu-id="0489d-114">Considerations</span></span>  

<span data-ttu-id="0489d-115">Vid åtkomst till entiteter i Media Services måste du ange specifika namn på huvudfält och värden i HTTP-begäranden.</span><span class="sxs-lookup"><span data-stu-id="0489d-115">When accessing entities in Media Services, you must set specific header fields and values in your HTTP requests.</span></span> <span data-ttu-id="0489d-116">Mer information finns i [installationsprogrammet för Media Services REST API-utveckling](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="0489d-116">For more information, see [Setup for Media Services REST API Development](media-services-rest-how-to-use.md).</span></span>

## <a name="connect-to-media-services"></a><span data-ttu-id="0489d-117">Ansluta till Media Services</span><span class="sxs-lookup"><span data-stu-id="0489d-117">Connect to Media Services</span></span>

<span data-ttu-id="0489d-118">Information om hur du ansluter till AMS API: et finns [åtkomst till Azure Media Services-API med Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="0489d-118">For information on how to connect to the AMS API, see [Access the Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

>[!NOTE]
><span data-ttu-id="0489d-119">När du har anslutit till https://media.windows.net, får du en 301 omdirigering att ange en annan Media Services-URI.</span><span class="sxs-lookup"><span data-stu-id="0489d-119">After successfully connecting to https://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="0489d-120">Du måste göra följande anrop till en ny URI.</span><span class="sxs-lookup"><span data-stu-id="0489d-120">You must make subsequent calls to the new URI.</span></span>

## <a name="adding-entities"></a><span data-ttu-id="0489d-121">Lägger till enheter</span><span class="sxs-lookup"><span data-stu-id="0489d-121">Adding entities</span></span>
<span data-ttu-id="0489d-122">Varje entitet i Media Services har lagts till i en entitetsuppsättning, till exempel tillgångar, via en HTTP POST-begäran.</span><span class="sxs-lookup"><span data-stu-id="0489d-122">Every entity in Media Services is added to an entity set, such as Assets, through a POST HTTP request.</span></span>

<span data-ttu-id="0489d-123">I följande exempel visas hur du skapar en AccessPolicy.</span><span class="sxs-lookup"><span data-stu-id="0489d-123">The following example shows how to create an AccessPolicy.</span></span>

    POST https://media.windows.net/API/AccessPolicies HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: media.windows.net
    Content-Length: 74
    Expect: 100-continue

    {"Name": "DownloadPolicy", "DurationInMinutes" : "300", "Permissions" : 1}

## <a name="querying-entities"></a><span data-ttu-id="0489d-124">Fråga entiteter</span><span class="sxs-lookup"><span data-stu-id="0489d-124">Querying entities</span></span>
<span data-ttu-id="0489d-125">Frågar och visar en lista över enheter är enkla och endast innebär en hämta HTTP-begäran och valfria OData-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="0489d-125">Querying and listing entities is straightforward and only involves a GET HTTP request and optional OData operations.</span></span>
<span data-ttu-id="0489d-126">I följande exempel hämtar en lista över alla MediaProcessor entiteter.</span><span class="sxs-lookup"><span data-stu-id="0489d-126">The following example retrieves a list of all MediaProcessor entities.</span></span>

    GET https://media.windows.net/API/MediaProcessors HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337078831&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=suFkxhvPWxQVMjOYelOJfYEWkyTWJCBc02pF0N7NghI%3d
    Host: media.windows.net

<span data-ttu-id="0489d-127">Du kan också hämta en viss enhet eller alla entitetsuppsättningar som är associerade med en specifik enhet, som i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="0489d-127">You can also retrieve a specific entity or all entity sets associated with a specific entity, such as in the following examples:</span></span>

    GET https://media.windows.net/API/JobTemplates('nb:jtid:UUID:e81192f5-576f-b247-b781-70a790c20e7c') HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1336907474&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=OpuY0CeTylqFFcFaP4pKUVGesT4PGx4CP55zDf2zXnc%3d
    Host: media.windows.net

    GET https://media.windows.net/API/JobTemplates('nb:jtid:UUID:e81192f5-576f-b247-b781-70a790c20e7c')/TaskTemplates HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1336907474&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=OpuY0CeTylqFFcFaP4pKUVGesT4PGx4CP55zDf2zXnc%3d
    Host: media.windows.net

<span data-ttu-id="0489d-128">I följande exempel returneras endast egenskapen State för alla jobb.</span><span class="sxs-lookup"><span data-stu-id="0489d-128">The following example returns only the State property of all Jobs.</span></span>

    GET https://media.windows.net/API/Jobs?$select=State HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337078831&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=suFkxhvPWxQVMjOYelOJfYEWkyTWJCBc02pF0N7NghI%3d
    Host: media.windows.net

<span data-ttu-id="0489d-129">I följande exempel returneras alla JobTemplates med namnet ”SampleTemplate”.</span><span class="sxs-lookup"><span data-stu-id="0489d-129">The following example returns all JobTemplates with the name "SampleTemplate."</span></span>

    GET https://media.windows.net/API/JobTemplates?$filter=startswith(Name,%20'SampleTemplate') HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337078831&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=suFkxhvPWxQVMjOYelOJfYEWkyTWJCBc02pF0N7NghI%3d
    Host: media.windows.net

> [!NOTE]
> <span data-ttu-id="0489d-130">$Expand åtgärden stöds inte i Media Services samt stöds inte LINQ metoderna som beskrivs i LINQ överväganden (WCF Data Services).</span><span class="sxs-lookup"><span data-stu-id="0489d-130">The $expand operation is not supported in Media Services as well as the unsupported LINQ methods described in LINQ Considerations (WCF Data Services).</span></span>
> 
> 

## <a name="enumerating-through-large-collections-of-entities"></a><span data-ttu-id="0489d-131">Uppräkning av stora mängder av entiteter</span><span class="sxs-lookup"><span data-stu-id="0489d-131">Enumerating through large collections of entities</span></span>
<span data-ttu-id="0489d-132">När du frågar entiteter, finns det en gräns på 1000 entiteter som returneras i taget eftersom offentlig REST-v2 begränsar frågeresultaten till 1000 resultat.</span><span class="sxs-lookup"><span data-stu-id="0489d-132">When querying entities, there is a limit of 1000 entities returned at one time because public REST v2 limits query results to 1000 results.</span></span> <span data-ttu-id="0489d-133">Använd **hoppa över** och **upp** att räkna upp genom stor samling med entiteter.</span><span class="sxs-lookup"><span data-stu-id="0489d-133">Use **skip** and **top** to enumerate through the large collection of entities.</span></span> 

<span data-ttu-id="0489d-134">I följande exempel visas hur du använder **hoppa över** och **upp** hoppa över först 2000 jobb och hämta nästa 1000 jobb.</span><span class="sxs-lookup"><span data-stu-id="0489d-134">The following example shows how to use **skip** and **top** to skip the first 2000 jobs and get the next 1000 jobs.</span></span>  

    GET https://media.windows.net/api/Jobs()?$skip=2000&$top=1000 HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337078831&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=suFkxhvPWxQVMjOYelOJfYEWkyTWJCBc02pF0N7NghI%3d
    Host: media.windows.net

## <a name="updating-entities"></a><span data-ttu-id="0489d-135">Uppdatera entiteter</span><span class="sxs-lookup"><span data-stu-id="0489d-135">Updating entities</span></span>
<span data-ttu-id="0489d-136">Beroende på enhetstypen och att den är i tillståndet kan du uppdatera egenskaper för enheten via en korrigering PUT eller sammanfoga HTTP-begäranden.</span><span class="sxs-lookup"><span data-stu-id="0489d-136">Depending on the entity type and the state that it is in, you can update properties on that entity through a PATCH, PUT, or MERGE HTTP requests.</span></span> <span data-ttu-id="0489d-137">Mer information om dessa åtgärder finns [MERGE-korrigering/PUT](https://msdn.microsoft.com/library/dd541276.aspx).</span><span class="sxs-lookup"><span data-stu-id="0489d-137">For more information about these operations, see [PATCH/PUT/MERGE](https://msdn.microsoft.com/library/dd541276.aspx).</span></span>

<span data-ttu-id="0489d-138">Följande kodexempel visar hur du uppdaterar egenskapen Name för en tillgång entitet.</span><span class="sxs-lookup"><span data-stu-id="0489d-138">The following code example shows how to update the Name property on an Asset entity.</span></span>

    MERGE https://media.windows.net/API/Assets('nb:cid:UUID:80782407-3f87-4e60-a43e-5e4454232f60') HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337083279&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=DMLQXWah4jO0icpfwyws5k%2b1aCDfz9KDGIGao20xk6g%3d
    Host: media.windows.net
    Content-Length: 21
    Expect: 100-continue

    {"Name" : "NewName" }

## <a name="deleting-entities"></a><span data-ttu-id="0489d-139">Ta bort enheter</span><span class="sxs-lookup"><span data-stu-id="0489d-139">Deleting entities</span></span>
<span data-ttu-id="0489d-140">Enheter kan tas bort i Media Services med hjälp av en ta bort HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="0489d-140">Entities can be deleted in Media Services by using a DELETE HTTP request.</span></span> <span data-ttu-id="0489d-141">Den ordning som du kan ta bort enheter kan vara viktiga beroende på enheten.</span><span class="sxs-lookup"><span data-stu-id="0489d-141">Depending on the entity, the order in which you delete entities may be important.</span></span> <span data-ttu-id="0489d-142">Till exempel entiteter, till exempel tillgångar kräver att du återkalla (eller ta bort) alla lokaliserare som refererar till viss tillgången innan du tar bort tillgången.</span><span class="sxs-lookup"><span data-stu-id="0489d-142">For example, entities such as Assets require that you revoke (or delete) all Locators that reference that particular Asset before deleting the Asset.</span></span>

<span data-ttu-id="0489d-143">I följande exempel visas hur du tar bort en positionerare som används för att överföra en fil i blob-lagring.</span><span class="sxs-lookup"><span data-stu-id="0489d-143">The following example shows how to delete a Locator that was used to upload a file into blob storage.</span></span>

    DELETE https://media.windows.net/API/Locators('nb:lid:UUID:76dcc8e8-4230-463d-97b0-ce25c41b5c8d') HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: media.windows.net
    Content-Length: 0

## <a name="media-services-learning-paths"></a><span data-ttu-id="0489d-144">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="0489d-144">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="0489d-145">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="0489d-145">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

