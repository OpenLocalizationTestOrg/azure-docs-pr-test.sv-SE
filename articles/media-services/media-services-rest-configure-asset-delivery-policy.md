---
title: "aaaConfiguring principerna för tillgångsleverans med hjälp av Media Services REST API | Microsoft Docs"
description: "Det här avsnittet visar hur tooconfigure olika principerna för tillgångsleverans med hjälp av Media Services REST API."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 5cb9d32a-e68b-4585-aa82-58dded0691d0
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: juliako
ms.openlocfilehash: 8203230d570935e17382c598820dbfe42f83f8d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-asset-delivery-policies"></a><span data-ttu-id="52b52-103">Konfigurera principerna för tillgångsleverans</span><span class="sxs-lookup"><span data-stu-id="52b52-103">Configuring asset delivery policies</span></span>
[!INCLUDE [media-services-selector-asset-delivery-policy](../../includes/media-services-selector-asset-delivery-policy.md)]

<span data-ttu-id="52b52-104">Om du planerar toodeliver dynamiskt krypterad tillgångar, steg ett av hello i hello Media Services content delivery arbetsflödet konfigurera leveransprinciperna för tillgångar.</span><span class="sxs-lookup"><span data-stu-id="52b52-104">If you plan toodeliver dynamically encrypted assets, one of hello steps in hello Media Services content delivery workflow is configuring delivery policies for assets.</span></span> <span data-ttu-id="52b52-105">Hej tillgångsleveransprincip talar om för Media Services hur du vill använda för din tillgång toobe levereras: i vilket strömningsprotokoll bör din tillgång dynamiskt paketeras (till exempel MPEG DASH, HLS, Smooth Streaming eller alla), oavsett om du vill toodynamically kryptera din tillgång och hur (envelope eller vanliga kryptering).</span><span class="sxs-lookup"><span data-stu-id="52b52-105">hello asset delivery policy tells Media Services how you want for your asset toobe delivered: into which streaming protocol should your asset be dynamically packaged (for example, MPEG DASH, HLS, Smooth Streaming, or all), whether or not you want toodynamically encrypt your asset and how (envelope or common encryption).</span></span>

<span data-ttu-id="52b52-106">Det här avsnittet beskrivs hur och varför toocreate och konfigurera leveransprinciperna för tillgången.</span><span class="sxs-lookup"><span data-stu-id="52b52-106">This topic discusses why and how toocreate and configure asset delivery policies.</span></span>

>[!NOTE]
><span data-ttu-id="52b52-107">När AMS-kontot skapas en **standard** strömningsslutpunkt har lagts till tooyour konto i hello **stoppad** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="52b52-107">When your AMS account is created a **default** streaming endpoint is added tooyour account in hello **Stopped** state.</span></span> <span data-ttu-id="52b52-108">toostart strömning ditt innehåll och dra nytta av dynamisk paketering och dynamisk kryptering hello strömningsslutpunkt som du vill toostream innehåll har toobe i hello **kör** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="52b52-108">toostart streaming your content and take advantage of dynamic packaging and dynamic encryption, hello streaming endpoint from which you want toostream content has toobe in hello **Running** state.</span></span> 
>
><span data-ttu-id="52b52-109">Dessutom toobe kan toouse dynamisk paketering och dynamisk kryptering din tillgång måste innehålla en uppsättning MP4s med anpassningsbar bithastighet eller Smooth Streaming-filer.</span><span class="sxs-lookup"><span data-stu-id="52b52-109">Also, toobe able toouse dynamic packaging and dynamic encryption your asset must contain a set of adaptive bitrate MP4s or adaptive bitrate Smooth Streaming files.</span></span>

<span data-ttu-id="52b52-110">Du kan använda olika principer toohello samma tillgång.</span><span class="sxs-lookup"><span data-stu-id="52b52-110">You could apply different policies toohello same asset.</span></span> <span data-ttu-id="52b52-111">Du kan till exempel använda PlayReady-kryptering tooSmooth Streaming och AES Envelope kryptering tooMPEG DASH och HLS.</span><span class="sxs-lookup"><span data-stu-id="52b52-111">For example, you could apply PlayReady encryption tooSmooth Streaming and AES Envelope encryption tooMPEG DASH and HLS.</span></span> <span data-ttu-id="52b52-112">Alla protokoll som inte har definierats i en leveransprincip (exempelvis du lägga till en enda princip som endast anger HLS som hello protokoll) kommer att blockeras från strömning.</span><span class="sxs-lookup"><span data-stu-id="52b52-112">Any protocols that are not defined in a delivery policy (for example, you add a single policy that only specifies HLS as hello protocol) will be blocked from streaming.</span></span> <span data-ttu-id="52b52-113">hello undantag toothis är om du har någon tillgångsleveransprincip alls definierats.</span><span class="sxs-lookup"><span data-stu-id="52b52-113">hello exception toothis is if you have no asset delivery policy defined at all.</span></span> <span data-ttu-id="52b52-114">Sedan tillåts alla protokoll i hello Rensa.</span><span class="sxs-lookup"><span data-stu-id="52b52-114">Then, all protocols will be allowed in hello clear.</span></span>

<span data-ttu-id="52b52-115">Om du vill toodeliver en krypterad tillgång för lagring, måste du konfigurera hello tillgångsleveransprincip.</span><span class="sxs-lookup"><span data-stu-id="52b52-115">If you want toodeliver a storage encrypted asset, you must configure hello asset’s delivery policy.</span></span> <span data-ttu-id="52b52-116">Innan din tillgång kan strömmas angetts hello streaming server tar bort hello lagringskryptering och dataströmmar innehåll med hello principen för tillgångsleverans.</span><span class="sxs-lookup"><span data-stu-id="52b52-116">Before your asset can be streamed, hello streaming server removes hello storage encryption and streams your content using hello specified delivery policy.</span></span> <span data-ttu-id="52b52-117">Till exempel toodeliver din tillgång krypteras med Advanced Encryption Standard (AES) kuvert krypteringsnyckeln genom att ange hello principtypen för**DynamicEnvelopeEncryption**.</span><span class="sxs-lookup"><span data-stu-id="52b52-117">For example, toodeliver your asset encrypted with Advanced Encryption Standard (AES) envelope encryption key, set hello policy type too**DynamicEnvelopeEncryption**.</span></span> <span data-ttu-id="52b52-118">tooremove lagringskryptering och dataströmmen hello tillgångar i hello rensa, ange hello princip för**NoDynamicEncryption**.</span><span class="sxs-lookup"><span data-stu-id="52b52-118">tooremove storage encryption and stream hello asset in hello clear, set hello policy type too**NoDynamicEncryption**.</span></span> <span data-ttu-id="52b52-119">Exempel som visar hur tooconfigure dessa principtyper följer.</span><span class="sxs-lookup"><span data-stu-id="52b52-119">Examples that show how tooconfigure these policy types follow.</span></span>

<span data-ttu-id="52b52-120">Beroende på hur du konfigurerar hello tillgångsleveransprincip du skulle kunna toodynamically paketet, dynamiskt kryptera och strömma hello följande strömningsprotokoll: Smooth Streaming, HLS, MPEG DASH-dataströmmar.</span><span class="sxs-lookup"><span data-stu-id="52b52-120">Depending on how you configure hello asset delivery policy you would be able toodynamically package, dynamically encrypt, and stream hello following streaming protocols: Smooth Streaming, HLS, MPEG DASH streams.</span></span>

<span data-ttu-id="52b52-121">följande lista visar hello hello formaterar du använder toostream Smooth, HLS, DASH.</span><span class="sxs-lookup"><span data-stu-id="52b52-121">hello following list shows hello formats that you use toostream Smooth, HLS, DASH.</span></span>

<span data-ttu-id="52b52-122">Smooth Streaming:</span><span class="sxs-lookup"><span data-stu-id="52b52-122">Smooth Streaming:</span></span>

<span data-ttu-id="52b52-123">{namn på slutpunkt för direktuppspelning-namn på mediaservicekonto}.streaming.mediaservices.windows.net/lokalisator-ID}/{filnamn}.ism/Manifest</span><span class="sxs-lookup"><span data-stu-id="52b52-123">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest</span></span>

<span data-ttu-id="52b52-124">HLS:</span><span class="sxs-lookup"><span data-stu-id="52b52-124">HLS:</span></span>

<span data-ttu-id="52b52-125">{strömmande slutpunkten namn media services-konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)</span><span class="sxs-lookup"><span data-stu-id="52b52-125">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)</span></span>

<span data-ttu-id="52b52-126">MPEG DASH</span><span class="sxs-lookup"><span data-stu-id="52b52-126">MPEG DASH</span></span>

<span data-ttu-id="52b52-127">{strömmande slutpunkten namn media services-konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)</span><span class="sxs-lookup"><span data-stu-id="52b52-127">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)</span></span>


<span data-ttu-id="52b52-128">Anvisningar för hur toopublish en tillgång och skapa en strömnings-URL, se [skapa en strömnings-URL](media-services-deliver-streaming-content.md).</span><span class="sxs-lookup"><span data-stu-id="52b52-128">For instructions on how toopublish an asset and build a streaming URL, see [Build a streaming URL](media-services-deliver-streaming-content.md).</span></span>

## <a name="considerations"></a><span data-ttu-id="52b52-129">Överväganden</span><span class="sxs-lookup"><span data-stu-id="52b52-129">Considerations</span></span>
* <span data-ttu-id="52b52-130">Du kan inte ta bort en AssetDeliveryPolicy som är associerade med en tillgång, medan det finns en (streaming) positionerare för tillgången.</span><span class="sxs-lookup"><span data-stu-id="52b52-130">You cannot delete an AssetDeliveryPolicy associated with an asset while an OnDemand (streaming) locator exists for that asset.</span></span> <span data-ttu-id="52b52-131">hello rekommendation är tooremove hello principen från hello tillgångsinformation innan du tar bort hello princip.</span><span class="sxs-lookup"><span data-stu-id="52b52-131">hello recommendation is tooremove hello policy from hello asset before deleting hello policy.</span></span>
* <span data-ttu-id="52b52-132">En strömningslokaliserare kan inte skapas på en krypterad tillgång lagring när någon tillgångsleveransprincip anges.</span><span class="sxs-lookup"><span data-stu-id="52b52-132">A streaming locator cannot be created on a storage encrypted asset when no asset delivery policy is set.</span></span>  <span data-ttu-id="52b52-133">Om inte hello tillgången är lagringskrypterad, kan hello system du skapa en positionerare och dataströmmen hello tillgång i hello Rensa utan en tillgångsleveransprincip.</span><span class="sxs-lookup"><span data-stu-id="52b52-133">If hello Asset isn’t storage encrypted, hello system will let you create a locator and stream hello asset in hello clear without an asset delivery policy.</span></span>
* <span data-ttu-id="52b52-134">Du kan ha flera principerna för tillgångsleverans som är associerade med en enda resurs, men du kan bara ange ett sätt toohandle angivna AssetDeliveryProtocol.</span><span class="sxs-lookup"><span data-stu-id="52b52-134">You can have multiple asset delivery policies associated with a single asset but you can only specify one way toohandle a given AssetDeliveryProtocol.</span></span>  <span data-ttu-id="52b52-135">Vilket innebär att om du toolink två leveransprinciperna som anger hello AssetDeliveryProtocol.SmoothStreaming protokoll som resulterar i ett fel eftersom hello system inte vet vilken vill du tooapply när en klient gör en Smooth Streaming-begäran.</span><span class="sxs-lookup"><span data-stu-id="52b52-135">Meaning if you try toolink two delivery policies that specify hello AssetDeliveryProtocol.SmoothStreaming protocol that will result in an error because hello system does not know which one you want it tooapply when a client makes a Smooth Streaming request.</span></span>
* <span data-ttu-id="52b52-136">Om du har en tillgång med en befintlig strömningslokaliserare kan inte länka en ny princip toohello tillgång, Avlänka en befintlig princip från hello tillgång eller uppdatera en leveransprincip som är associerade med hello tillgången.</span><span class="sxs-lookup"><span data-stu-id="52b52-136">If you have an asset with an existing streaming locator, you cannot link a new policy toohello asset, unlink an existing policy from hello asset, or update a delivery policy associated with hello asset.</span></span>  <span data-ttu-id="52b52-137">Du först har tooremove hello strömningslokaliserare, justera hello principer och återskapa hello strömning lokaliserare.</span><span class="sxs-lookup"><span data-stu-id="52b52-137">You first have tooremove hello streaming locator, adjust hello policies, and then re-create hello streaming locator.</span></span>  <span data-ttu-id="52b52-138">Du kan använda samma locatorId när du skapar nytt hello strömning lokaliserare, men du bör kontrollera hello som inte orsakar problem för klienter eftersom innehållet kan cachelagras av hello ursprung eller en underordnad CDN.</span><span class="sxs-lookup"><span data-stu-id="52b52-138">You can use hello same locatorId when you recreate hello streaming locator but you should ensure that won’t cause issues for clients since content can be cached by hello origin or a downstream CDN.</span></span>

>[!NOTE]

><span data-ttu-id="52b52-139">Vid åtkomst till entiteter i Media Services måste du ange specifika namn på huvudfält och värden i HTTP-begäranden.</span><span class="sxs-lookup"><span data-stu-id="52b52-139">When accessing entities in Media Services, you must set specific header fields and values in your HTTP requests.</span></span> <span data-ttu-id="52b52-140">Mer information finns i [installationsprogrammet för Media Services REST API-utveckling](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="52b52-140">For more information, see [Setup for Media Services REST API Development](media-services-rest-how-to-use.md).</span></span>

## <a name="connect-toomedia-services"></a><span data-ttu-id="52b52-141">Ansluta tooMedia tjänster</span><span class="sxs-lookup"><span data-stu-id="52b52-141">Connect tooMedia Services</span></span>

<span data-ttu-id="52b52-142">Mer information om hur tooconnect toohello AMS API, se [åtkomst hello Azure Media Services API med Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="52b52-142">For information on how tooconnect toohello AMS API, see [Access hello Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

>[!NOTE]
><span data-ttu-id="52b52-143">När du har anslutit toohttps://media.windows.net, får du en 301 omdirigering att ange en annan Media Services-URI.</span><span class="sxs-lookup"><span data-stu-id="52b52-143">After successfully connecting toohttps://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="52b52-144">Du måste göra följande anrop toohello ny URI.</span><span class="sxs-lookup"><span data-stu-id="52b52-144">You must make subsequent calls toohello new URI.</span></span>

## <a name="clear-asset-delivery-policy"></a><span data-ttu-id="52b52-145">Rensa tillgångsleveransprincip</span><span class="sxs-lookup"><span data-stu-id="52b52-145">Clear asset delivery policy</span></span>
### <span data-ttu-id="52b52-146"><a id="create_asset_delivery_policy"></a>Skapa tillgångsleveransprincip</span><span class="sxs-lookup"><span data-stu-id="52b52-146"><a id="create_asset_delivery_policy"></a>Create asset delivery policy</span></span>
<span data-ttu-id="52b52-147">hello följande HTTP-begäran skapar en tillgångsleveransprincip som anger toonot använda dynamisk kryptering och toodeliver hello dataström i någon av hello följande protokoll: MPEG DASH, HLS och Smooth Streaming-protokoll.</span><span class="sxs-lookup"><span data-stu-id="52b52-147">hello following HTTP request creates an asset delivery policy that specifies toonot apply dynamic encryption and toodeliver hello stream in any of hello following protocols:  MPEG DASH, HLS, and Smooth Streaming protocols.</span></span> 

<span data-ttu-id="52b52-148">Information om vilka värden som du anger när du skapar en AssetDeliveryPolicy finns hello [typer som används när du definierar AssetDeliveryPolicy](#types) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="52b52-148">For information on what values you can specify when creating an AssetDeliveryPolicy, see hello [Types used when defining AssetDeliveryPolicy](#types) section.</span></span>   

<span data-ttu-id="52b52-149">Begäran:</span><span class="sxs-lookup"><span data-stu-id="52b52-149">Request:</span></span>

    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423397827&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=Szo6lbJAvL3dyecAeVmyAnzv3mGzfUNClR5shk9Ivbk%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 4651882c-d7ad-4d5e-86ab-f07f47dcb41e
    Host: media.windows.net

    {"Name":"Clear Policy",
    "AssetDeliveryProtocol":7,
    "AssetDeliveryPolicyType":2,
    "AssetDeliveryConfiguration":null}

<span data-ttu-id="52b52-150">Svar:</span><span class="sxs-lookup"><span data-stu-id="52b52-150">Response:</span></span>

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 363
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3A92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 4651882c-d7ad-4d5e-86ab-f07f47dcb41e
    request-id: 6aedbf93-4bc2-4586-8845-fd45590136af
    x-ms-request-id: 6aedbf93-4bc2-4586-8845-fd45590136af
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 08 Feb 2015 06:21:27 GMT

    {"odata.metadata":"https://media.windows.net/api/$metadata#AssetDeliveryPolicies/@Element",
    "Id":"nb:adpid:UUID:92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd",
    "Name":"Clear Policy",
    "AssetDeliveryProtocol":7,
    "AssetDeliveryPolicyType":2,
    "AssetDeliveryConfiguration":null,
    "Created":"2015-02-08T06:21:27.6908329Z",
    "LastModified":"2015-02-08T06:21:27.6908329Z"}

### <span data-ttu-id="52b52-151"><a id="link_asset_with_asset_delivery_policy"></a>Länken tillgång med tillgångsleveransprincip</span><span class="sxs-lookup"><span data-stu-id="52b52-151"><a id="link_asset_with_asset_delivery_policy"></a>Link asset with asset delivery policy</span></span>
<span data-ttu-id="52b52-152">hello efter HTTP-begäran länkar hello angetts toohello tillgången tillgångsleveransprincip till.</span><span class="sxs-lookup"><span data-stu-id="52b52-152">hello following HTTP request links hello specified asset toohello asset delivery policy to.</span></span>

<span data-ttu-id="52b52-153">Begäran:</span><span class="sxs-lookup"><span data-stu-id="52b52-153">Request:</span></span>

    POST https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A86933344-9539-4d0c-be7d-f842458693e0')/$links/DeliveryPolicies HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Content-Type: application/json
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-e769-3344-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423397827&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=Szo6lbJAvL3dyecAeVmyAnzv3mGzfUNClR5shk9Ivbk%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 56d2763f-6e72-419d-ba3c-685f6db97e81
    Host: media.windows.net

    {"uri":"https://media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3A92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd')"}

<span data-ttu-id="52b52-154">Svar:</span><span class="sxs-lookup"><span data-stu-id="52b52-154">Response:</span></span>

    HTTP/1.1 204 No Content


## <a name="dynamicenvelopeencryption-asset-delivery-policy"></a><span data-ttu-id="52b52-155">DynamicEnvelopeEncryption tillgångsleveransprincip</span><span class="sxs-lookup"><span data-stu-id="52b52-155">DynamicEnvelopeEncryption asset delivery policy</span></span>
### <a name="create-content-key-of-hello-envelopeencryption-type-and-link-it-toohello-asset"></a><span data-ttu-id="52b52-156">Skapa innehållsnyckeln av hello EnvelopeEncryption typ och länka det toohello tillgångsinformation</span><span class="sxs-lookup"><span data-stu-id="52b52-156">Create content key of hello EnvelopeEncryption type and link it toohello asset</span></span>
<span data-ttu-id="52b52-157">När du anger DynamicEnvelopeEncryption leveransprincip måste toomake att toolink din tillgång tooa innehållsnyckeln av hello EnvelopeEncryption typen.</span><span class="sxs-lookup"><span data-stu-id="52b52-157">When specifying DynamicEnvelopeEncryption delivery policy, you need toomake sure toolink your asset tooa content key of hello EnvelopeEncryption type.</span></span> <span data-ttu-id="52b52-158">Mer information finns: [att skapa en innehållsnyckel](media-services-rest-create-contentkey.md)).</span><span class="sxs-lookup"><span data-stu-id="52b52-158">For more information, see: [Creating a content key](media-services-rest-create-contentkey.md)).</span></span>

### <span data-ttu-id="52b52-159"><a id="get_delivery_url"></a>Hämta URL för leverans</span><span class="sxs-lookup"><span data-stu-id="52b52-159"><a id="get_delivery_url"></a>Get delivery URL</span></span>
<span data-ttu-id="52b52-160">Get hello leverans URL för hello angetts leveransmetod av hello innehållsnyckeln som skapats i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="52b52-160">Get hello delivery URL for hello specified delivery method of hello content key created in hello previous step.</span></span> <span data-ttu-id="52b52-161">En klient använder hello returnerade URL toorequest AES-nyckel eller en PlayReady-licens i ordning tooplayback hello skyddat innehåll.</span><span class="sxs-lookup"><span data-stu-id="52b52-161">A client uses hello returned URL toorequest an AES key or a PlayReady license in order tooplayback hello protected content.</span></span>

<span data-ttu-id="52b52-162">Ange hello hello URL tooget i hello brödtext hello HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="52b52-162">Specify hello type of hello URL tooget in hello body of hello HTTP request.</span></span> <span data-ttu-id="52b52-163">Om du skyddar ditt innehåll med PlayReady Media Services PlayReady licens förvärv URL-begäran med 1 för hello keyDeliveryType: {”keyDeliveryType”: 1}.</span><span class="sxs-lookup"><span data-stu-id="52b52-163">If you are protecting your content with PlayReady, request a Media Services PlayReady license acquisition URL, using 1 for hello keyDeliveryType: {"keyDeliveryType":1}.</span></span> <span data-ttu-id="52b52-164">Om du skyddar ditt innehåll med hello kuvert kryptering URL-begäran en nyckel förvärv genom att ange 2 för keyDeliveryType: {”keyDeliveryType”: 2}.</span><span class="sxs-lookup"><span data-stu-id="52b52-164">If you are protecting your content with hello envelope encryption, request a key acquisition URL by specifying 2 for keyDeliveryType: {"keyDeliveryType":2}.</span></span>

<span data-ttu-id="52b52-165">Begäran:</span><span class="sxs-lookup"><span data-stu-id="52b52-165">Request:</span></span>

    POST https://media.windows.net/api/ContentKeys('nb:kid:UUID:dc88f996-2859-4cf7-a279-c52a9d6b2f04')/GetKeyDeliveryUrl HTTP/1.1
    Content-Type: application/json
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423452029&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=IEXV06e3drSIN5naFRBdhJZCbfEqQbFZsGSIGmawhEo%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 569d4b7c-a446-4edc-b77c-9fb686083dd8
    Host: media.windows.net
    Content-Length: 21

    {"keyDeliveryType":2}

<span data-ttu-id="52b52-166">Svar:</span><span class="sxs-lookup"><span data-stu-id="52b52-166">Response:</span></span>

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 198
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 569d4b7c-a446-4edc-b77c-9fb686083dd8
    request-id: d26f65d2-fe65-4136-8fcf-31545be68377
    x-ms-request-id: d26f65d2-fe65-4136-8fcf-31545be68377
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 08 Feb 2015 21:42:30 GMT

    {"odata.metadata":"media.windows.net/api/$metadata#Edm.String","value":"https://amsaccount1.keydelivery.mediaservices.windows.net/?KID=dc88f996-2859-4cf7-a279-c52a9d6b2f04"}


### <a name="create-asset-delivery-policy"></a><span data-ttu-id="52b52-167">Skapa tillgångsleveransprincip</span><span class="sxs-lookup"><span data-stu-id="52b52-167">Create asset delivery policy</span></span>
<span data-ttu-id="52b52-168">hello följande HTTP-begäran skapar hello **AssetDeliveryPolicy** som är konfigurerade tooapply dynamiska kryptering (**DynamicEnvelopeEncryption**) toohello **HLS**protocol (i det här exemplet andra protokoll kommer att blockeras från strömning).</span><span class="sxs-lookup"><span data-stu-id="52b52-168">hello following HTTP request creates hello **AssetDeliveryPolicy** that is configured tooapply dynamic envelope encryption (**DynamicEnvelopeEncryption**) toohello **HLS** protocol (in this example, other protocols will be blocked from streaming).</span></span> 

<span data-ttu-id="52b52-169">Information om vilka värden som du anger när du skapar en AssetDeliveryPolicy finns hello [typer som används när du definierar AssetDeliveryPolicy](#types) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="52b52-169">For information on what values you can specify when creating an AssetDeliveryPolicy, see hello [Types used when defining AssetDeliveryPolicy](#types) section.</span></span>   

<span data-ttu-id="52b52-170">Begäran:</span><span class="sxs-lookup"><span data-stu-id="52b52-170">Request:</span></span>

    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423480651&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=T2FG3tIV0e2ETzxQ6RDWxWAsAzuy3ez2ruXPhrBe62Y%3d
    x-ms-version: 2.11
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    Host: media.windows.net

    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":4,"AssetDeliveryPolicyType":3,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\\/\"}]"}


<span data-ttu-id="52b52-171">Svar:</span><span class="sxs-lookup"><span data-stu-id="52b52-171">Response:</span></span>

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 460
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3Aec9b994e-672c-4a5b-8490-a464eeb7964b')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    request-id: c2a1ac0e-9644-474f-b38f-b9541c3a7c5f
    x-ms-request-id: c2a1ac0e-9644-474f-b38f-b9541c3a7c5f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 09 Feb 2015 05:24:38 GMT

    {"odata.metadata":"media.windows.net/api/$metadata#AssetDeliveryPolicies/@Element","Id":"nb:adpid:UUID:ec9b994e-672c-4a5b-8490-a464eeb7964b","Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":4,"AssetDeliveryPolicyType":3,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\\/\"}]","Created":"2015-02-09T05:24:38.9167436Z","LastModified":"2015-02-09T05:24:38.9167436Z"}


### <a name="link-asset-with-asset-delivery-policy"></a><span data-ttu-id="52b52-172">Länken tillgång med tillgångsleveransprincip</span><span class="sxs-lookup"><span data-stu-id="52b52-172">Link asset with asset delivery policy</span></span>
<span data-ttu-id="52b52-173">Se [länk tillgång med tillgångsleveransprincip](#link_asset_with_asset_delivery_policy)</span><span class="sxs-lookup"><span data-stu-id="52b52-173">See [Link asset with asset delivery policy](#link_asset_with_asset_delivery_policy)</span></span>

## <a name="dynamiccommonencryption-asset-delivery-policy"></a><span data-ttu-id="52b52-174">DynamicCommonEncryption tillgångsleveransprincip</span><span class="sxs-lookup"><span data-stu-id="52b52-174">DynamicCommonEncryption asset delivery policy</span></span>
### <a name="create-content-key-of-hello-commonencryption-type-and-link-it-toohello-asset"></a><span data-ttu-id="52b52-175">Skapa innehållsnyckeln av hello CommonEncryption typ och länka det toohello tillgångsinformation</span><span class="sxs-lookup"><span data-stu-id="52b52-175">Create content key of hello CommonEncryption type and link it toohello asset</span></span>
<span data-ttu-id="52b52-176">När du anger DynamicCommonEncryption leveransprincip måste toomake att toolink din tillgång tooa innehållsnyckeln av hello CommonEncryption typen.</span><span class="sxs-lookup"><span data-stu-id="52b52-176">When specifying DynamicCommonEncryption delivery policy, you need toomake sure toolink your asset tooa content key of hello CommonEncryption type.</span></span> <span data-ttu-id="52b52-177">Mer information finns: [att skapa en innehållsnyckel](media-services-rest-create-contentkey.md)).</span><span class="sxs-lookup"><span data-stu-id="52b52-177">For more information, see: [Creating a content key](media-services-rest-create-contentkey.md)).</span></span>

### <a name="get-delivery-url"></a><span data-ttu-id="52b52-178">Hämta URL för leverans</span><span class="sxs-lookup"><span data-stu-id="52b52-178">Get Delivery URL</span></span>
<span data-ttu-id="52b52-179">Hämta hello leverans URL för hello PlayReady leveransmetod av hello innehållsnyckeln som skapats i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="52b52-179">Get hello delivery URL for hello PlayReady delivery method of hello content key created in hello previous step.</span></span> <span data-ttu-id="52b52-180">En klient använder hello returnerade URL toorequest en PlayReady-licens i ordning tooplayback hello skyddat innehåll.</span><span class="sxs-lookup"><span data-stu-id="52b52-180">A client uses hello returned URL toorequest a PlayReady license in order tooplayback hello protected content.</span></span> <span data-ttu-id="52b52-181">Mer information finns i [Hämta leverans URL](#get_delivery_url).</span><span class="sxs-lookup"><span data-stu-id="52b52-181">For more information, see [Get Delivery URL](#get_delivery_url).</span></span>

### <a name="create-asset-delivery-policy"></a><span data-ttu-id="52b52-182">Skapa tillgångsleveransprincip</span><span class="sxs-lookup"><span data-stu-id="52b52-182">Create asset delivery policy</span></span>
<span data-ttu-id="52b52-183">hello följande HTTP-begäran skapar hello **AssetDeliveryPolicy** som är konfigurerade tooapply dynamisk vanliga kryptering (**DynamicCommonEncryption**) toohello **Smooth Streaming**  protocol (i det här exemplet andra protokoll kommer att blockeras från strömning).</span><span class="sxs-lookup"><span data-stu-id="52b52-183">hello following HTTP request creates hello **AssetDeliveryPolicy** that is configured tooapply dynamic common encryption (**DynamicCommonEncryption**) toohello **Smooth Streaming** protocol (in this example, other protocols will be blocked from streaming).</span></span> 

<span data-ttu-id="52b52-184">Information om vilka värden som du anger när du skapar en AssetDeliveryPolicy finns hello [typer som används när du definierar AssetDeliveryPolicy](#types) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="52b52-184">For information on what values you can specify when creating an AssetDeliveryPolicy, see hello [Types used when defining AssetDeliveryPolicy](#types) section.</span></span>   

<span data-ttu-id="52b52-185">Begäran:</span><span class="sxs-lookup"><span data-stu-id="52b52-185">Request:</span></span>

    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423480651&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=T2FG3tIV0e2ETzxQ6RDWxWAsAzuy3ez2ruXPhrBe62Y%3d
    x-ms-version: 2.11
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    Host: media.windows.net

    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":1,"AssetDeliveryPolicyType":4,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\/PlayReady\/"}]"}


<span data-ttu-id="52b52-186">Om du vill tooprotect innehåll med Widevine DRM, uppdatera hello AssetDeliveryConfiguration värden toouse WidevineLicenseAcquisitionUrl (som har hello värdet 7) och ange hello-URL för en licensleveranstjänst.</span><span class="sxs-lookup"><span data-stu-id="52b52-186">If you want tooprotect your content using Widevine DRM, update hello AssetDeliveryConfiguration values toouse WidevineLicenseAcquisitionUrl (which has hello value of 7) and specify hello URL of a license delivery service.</span></span> <span data-ttu-id="52b52-187">Du kan använda följande AMS-partner toohelp du leverera Widevine-licenser hello: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).</span><span class="sxs-lookup"><span data-stu-id="52b52-187">You can use hello following AMS partners toohelp you deliver Widevine licenses: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).</span></span>

<span data-ttu-id="52b52-188">Exempel:</span><span class="sxs-lookup"><span data-stu-id="52b52-188">For example:</span></span> 

    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":2,"AssetDeliveryPolicyType":4,"AssetDeliveryConfiguration":"[{\"Key\":7,\"Value\":\"https:\\/\\/example.net\/WidevineLicenseAcquisition\/"}]"}

> [!NOTE]
> <span data-ttu-id="52b52-189">Vid kryptering med Widevine, skulle du bara att kunna toodeliver med bindestreck.</span><span class="sxs-lookup"><span data-stu-id="52b52-189">When encrypting with Widevine, you would only be able toodeliver using DASH.</span></span> <span data-ttu-id="52b52-190">Se till att toospecify STRECK (2) i hello protokollet för tillgångsleverans.</span><span class="sxs-lookup"><span data-stu-id="52b52-190">Make sure toospecify DASH (2) in hello asset delivery protocol.</span></span>
> 
> 

### <a name="link-asset-with-asset-delivery-policy"></a><span data-ttu-id="52b52-191">Länken tillgång med tillgångsleveransprincip</span><span class="sxs-lookup"><span data-stu-id="52b52-191">Link asset with asset delivery policy</span></span>
<span data-ttu-id="52b52-192">Se [länk tillgång med tillgångsleveransprincip](#link_asset_with_asset_delivery_policy)</span><span class="sxs-lookup"><span data-stu-id="52b52-192">See [Link asset with asset delivery policy](#link_asset_with_asset_delivery_policy)</span></span>

## <span data-ttu-id="52b52-193"><a id="types"></a>Typer som används när du definierar AssetDeliveryPolicy</span><span class="sxs-lookup"><span data-stu-id="52b52-193"><a id="types"></a>Types used when defining AssetDeliveryPolicy</span></span>

### <a name="assetdeliveryprotocol"></a><span data-ttu-id="52b52-194">AssetDeliveryProtocol</span><span class="sxs-lookup"><span data-stu-id="52b52-194">AssetDeliveryProtocol</span></span>

<span data-ttu-id="52b52-195">hello beskrivs följande enum värden som du kan ange för hello protokollet för tillgångsleverans.</span><span class="sxs-lookup"><span data-stu-id="52b52-195">hello following enum describes values you can set for hello asset delivery protocol.</span></span>

    [Flags]
    public enum AssetDeliveryProtocol
    {
        /// <summary>
        /// No protocols.
        /// </summary>
        None = 0x0,

        /// <summary>
        /// Smooth streaming protocol.
        /// </summary>
        SmoothStreaming = 0x1,

        /// <summary>
        /// MPEG Dynamic Adaptive Streaming over HTTP (DASH)
        /// </summary>
        Dash = 0x2,

        /// <summary>
        /// Apple HTTP Live Streaming protocol.
        /// </summary>
        HLS = 0x4,

        ProgressiveDownload = 0x10, 
 
        /// <summary>
        /// Include all protocols.
        /// </summary>
        All = 0xFFFF
    }

### <a name="assetdeliverypolicytype"></a><span data-ttu-id="52b52-196">AssetDeliveryPolicyType</span><span class="sxs-lookup"><span data-stu-id="52b52-196">AssetDeliveryPolicyType</span></span>

<span data-ttu-id="52b52-197">hello beskriver följande enum värden som du kan ange för hello tillgångstyp leverans princip.</span><span class="sxs-lookup"><span data-stu-id="52b52-197">hello following enum describes values you can set for hello asset delivery policy type.</span></span>  

    public enum AssetDeliveryPolicyType
    {
        /// <summary>
        /// Delivery Policy Type not set.  An invalid value.
        /// </summary>
        None,

        /// <summary>
        /// hello Asset should not be delivered via this AssetDeliveryProtocol. 
        /// </summary>
        Blocked, 

        /// <summary>
        /// Do not apply dynamic encryption toohello asset.
        /// </summary>
        /// 
        NoDynamicEncryption,  

        /// <summary>
        /// Apply Dynamic Envelope encryption.
        /// </summary>
        DynamicEnvelopeEncryption,

        /// <summary>
        /// Apply Dynamic Common encryption.
        /// </summary>
        DynamicCommonEncryption
        }

### <a name="contentkeydeliverytype"></a><span data-ttu-id="52b52-198">ContentKeyDeliveryType</span><span class="sxs-lookup"><span data-stu-id="52b52-198">ContentKeyDeliveryType</span></span>

<span data-ttu-id="52b52-199">hello beskriver följande enum värden som du kan använda tooconfigure hello leveransmetod av hello innehåll viktiga toohello-klienten.</span><span class="sxs-lookup"><span data-stu-id="52b52-199">hello following enum describes values you can use tooconfigure hello delivery method of hello content key toohello client.</span></span>
    
    public enum ContentKeyDeliveryType
    {
        /// <summary>
        /// None.
        ///
        </summary>
        None = 0,

        /// <summary>
        /// Use PlayReady License acquistion protocol
        ///
        </summary>
        PlayReadyLicense = 1,

        /// <summary>
        /// Use MPEG Baseline HTTP key protocol.
        ///
        </summary>
        BaselineHttp = 2,

        /// <summary>
        /// Use Widevine License acquistion protocol
        ///
        </summary>
        Widevine = 3

    }


### <a name="assetdeliverypolicyconfigurationkey"></a><span data-ttu-id="52b52-200">AssetDeliveryPolicyConfigurationKey</span><span class="sxs-lookup"><span data-stu-id="52b52-200">AssetDeliveryPolicyConfigurationKey</span></span>

<span data-ttu-id="52b52-201">hello följande enum beskrivs värden som du kan ange tooconfigure nycklar som används tooget specifik konfiguration för en tillgångsleveransprincip.</span><span class="sxs-lookup"><span data-stu-id="52b52-201">hello following enum describes values you can set tooconfigure keys used tooget specific configuration for an asset delivery policy.</span></span>

    public enum AssetDeliveryPolicyConfigurationKey
    {
        /// <summary>
        /// No policies.
        /// </summary>
        None,

        /// <summary>
        /// Exact Envelope key URL.
        /// </summary>
        EnvelopeKeyAcquisitionUrl,

        /// <summary>
        /// Base key url that will have KID=<Guid> appended for Envelope.
        /// </summary>
        EnvelopeBaseKeyAcquisitionUrl,

        /// <summary>
        /// hello initialization vector toouse for envelope encryption in Base64 format.
        /// </summary>
        EnvelopeEncryptionIVAsBase64,

        /// <summary>
        /// hello PlayReady License Acquisition Url toouse for common encryption.
        /// </summary>
        PlayReadyLicenseAcquisitionUrl,

        /// <summary>
        /// hello PlayReady Custom Attributes tooadd toohello PlayReady Content Header
        /// </summary>
        PlayReadyCustomAttributes,

        /// <summary>
        /// hello initialization vector toouse for envelope encryption.
        /// </summary>
        EnvelopeEncryptionIV,

        /// <summary>
        /// Widevine DRM acquisition url
        /// </summary>
        WidevineLicenseAcquisitionUrl
    }

## <a name="media-services-learning-paths"></a><span data-ttu-id="52b52-202">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="52b52-202">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="52b52-203">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="52b52-203">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

