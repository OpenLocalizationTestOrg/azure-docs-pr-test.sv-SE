---
title: "Konfigurera principerna för tillgångsleverans med hjälp av Media Services REST API | Microsoft Docs"
description: "Det här avsnittet visar hur du konfigurerar olika principerna för tillgångsleverans med hjälp av Media Services REST API."
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
ms.openlocfilehash: 7ffbde11b943961dd3a3b5edebd0cfd52429e845
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="configuring-asset-delivery-policies"></a><span data-ttu-id="10ee2-103">Konfigurera principerna för tillgångsleverans</span><span class="sxs-lookup"><span data-stu-id="10ee2-103">Configuring asset delivery policies</span></span>
[!INCLUDE [media-services-selector-asset-delivery-policy](../../includes/media-services-selector-asset-delivery-policy.md)]

<span data-ttu-id="10ee2-104">Om du planerar att leverera dynamiskt krypterade tillgångar ett av stegen i Media Services innehållsleverans arbetsflödet konfigurera leveransprinciperna för tillgångar.</span><span class="sxs-lookup"><span data-stu-id="10ee2-104">If you plan to deliver dynamically encrypted assets, one of the steps in the Media Services content delivery workflow is configuring delivery policies for assets.</span></span> <span data-ttu-id="10ee2-105">Anger tillgångsleveransprincip Media Services hur du vill använda för din tillgång som ska levereras: till vilka streaming-protokollet bör din tillgång dynamiskt paketeras (till exempel MPEG DASH, HLS, Smooth Streaming eller alla), oavsett om du vill kryptera dynamiskt din tillgång och hur (envelope eller vanliga kryptering).</span><span class="sxs-lookup"><span data-stu-id="10ee2-105">The asset delivery policy tells Media Services how you want for your asset to be delivered: into which streaming protocol should your asset be dynamically packaged (for example, MPEG DASH, HLS, Smooth Streaming, or all), whether or not you want to dynamically encrypt your asset and how (envelope or common encryption).</span></span>

<span data-ttu-id="10ee2-106">Det här avsnittet beskriver varför och hur du skapar och konfigurerar principerna för tillgångsleverans.</span><span class="sxs-lookup"><span data-stu-id="10ee2-106">This topic discusses why and how to create and configure asset delivery policies.</span></span>

>[!NOTE]
><span data-ttu-id="10ee2-107">När ditt AMS-konto skapas läggs en **standard**-slutpunkt för direktuppspelning till på ditt konto med tillståndet **Stoppad**.</span><span class="sxs-lookup"><span data-stu-id="10ee2-107">When your AMS account is created a **default** streaming endpoint is added to your account in the **Stopped** state.</span></span> <span data-ttu-id="10ee2-108">Om du vill starta direktuppspelning av innehåll och dra nytta av dynamisk paketering och dynamisk kryptering måste slutpunkten för direktuppspelning som du vill spela upp innehåll från ha tillståndet **Körs**.</span><span class="sxs-lookup"><span data-stu-id="10ee2-108">To start streaming your content and take advantage of dynamic packaging and dynamic encryption, the streaming endpoint from which you want to stream content has to be in the **Running** state.</span></span> 
>
><span data-ttu-id="10ee2-109">Dessutom måste din tillgång för att kunna använda dynamisk paketering och dynamisk kryptering innehåller en uppsättning MP4s med anpassningsbar bithastighet eller Smooth Streaming-filer.</span><span class="sxs-lookup"><span data-stu-id="10ee2-109">Also, to be able to use dynamic packaging and dynamic encryption your asset must contain a set of adaptive bitrate MP4s or adaptive bitrate Smooth Streaming files.</span></span>

<span data-ttu-id="10ee2-110">Du kan tillämpa olika principer för samma tillgång.</span><span class="sxs-lookup"><span data-stu-id="10ee2-110">You could apply different policies to the same asset.</span></span> <span data-ttu-id="10ee2-111">Du kan till exempel tillämpa PlayReady-kryptering till kryptering för Smooth Streaming- och AES Envelope MPEG DASH och HLS.</span><span class="sxs-lookup"><span data-stu-id="10ee2-111">For example, you could apply PlayReady encryption to Smooth Streaming and AES Envelope encryption to MPEG DASH and HLS.</span></span> <span data-ttu-id="10ee2-112">Alla protokoll som inte har definierats i en leveransprincip (exempelvis kan du lägga till en enskild princip som endast anger HLS som protokoll) kommer att blockeras från strömning.</span><span class="sxs-lookup"><span data-stu-id="10ee2-112">Any protocols that are not defined in a delivery policy (for example, you add a single policy that only specifies HLS as the protocol) will be blocked from streaming.</span></span> <span data-ttu-id="10ee2-113">Ett undantag till detta är om du inte har definierat någon tillgångsleveransprincip alls.</span><span class="sxs-lookup"><span data-stu-id="10ee2-113">The exception to this is if you have no asset delivery policy defined at all.</span></span> <span data-ttu-id="10ee2-114">Därefter tillåts alla protokoll fritt.</span><span class="sxs-lookup"><span data-stu-id="10ee2-114">Then, all protocols will be allowed in the clear.</span></span>

<span data-ttu-id="10ee2-115">Om du vill leverera en krypterad tillgång lagring måste du konfigurera den tillgångsleveransprincip.</span><span class="sxs-lookup"><span data-stu-id="10ee2-115">If you want to deliver a storage encrypted asset, you must configure the asset’s delivery policy.</span></span> <span data-ttu-id="10ee2-116">Innan din tillgång kan strömmas strömmande server tar du bort krypteringen lagring och strömmar ditt innehåll med hjälp av angivna leveransprincipen.</span><span class="sxs-lookup"><span data-stu-id="10ee2-116">Before your asset can be streamed, the streaming server removes the storage encryption and streams your content using the specified delivery policy.</span></span> <span data-ttu-id="10ee2-117">Till exempel för att leverera din tillgång som krypterats med Advanced Encryption Standard (AES) kuvert krypteringsnyckeln anger du principtypen **DynamicEnvelopeEncryption**.</span><span class="sxs-lookup"><span data-stu-id="10ee2-117">For example, to deliver your asset encrypted with Advanced Encryption Standard (AES) envelope encryption key, set the policy type to **DynamicEnvelopeEncryption**.</span></span> <span data-ttu-id="10ee2-118">Om du vill ta bort lagringskryptering och strömma tillgången i klartext, anger du principtypen **NoDynamicEncryption**.</span><span class="sxs-lookup"><span data-stu-id="10ee2-118">To remove storage encryption and stream the asset in the clear, set the policy type to **NoDynamicEncryption**.</span></span> <span data-ttu-id="10ee2-119">Exempel som visar hur du konfigurerar dessa principtyper följer.</span><span class="sxs-lookup"><span data-stu-id="10ee2-119">Examples that show how to configure these policy types follow.</span></span>

<span data-ttu-id="10ee2-120">Beroende på hur du konfigurerar tillgångsleveransprincip du skulle kunna dynamiskt paketera dynamiskt kryptera och strömma följande protokoll för dataströmmar: Smooth Streaming, HLS, MPEG DASH-dataströmmar.</span><span class="sxs-lookup"><span data-stu-id="10ee2-120">Depending on how you configure the asset delivery policy you would be able to dynamically package, dynamically encrypt, and stream the following streaming protocols: Smooth Streaming, HLS, MPEG DASH streams.</span></span>

<span data-ttu-id="10ee2-121">I följande lista visas format för att du använder att dataströmmen Smooth, HLS, DASH.</span><span class="sxs-lookup"><span data-stu-id="10ee2-121">The following list shows the formats that you use to stream Smooth, HLS, DASH.</span></span>

<span data-ttu-id="10ee2-122">Smooth Streaming:</span><span class="sxs-lookup"><span data-stu-id="10ee2-122">Smooth Streaming:</span></span>

<span data-ttu-id="10ee2-123">{namn på slutpunkt för direktuppspelning-namn på mediaservicekonto}.streaming.mediaservices.windows.net/lokalisator-ID}/{filnamn}.ism/Manifest</span><span class="sxs-lookup"><span data-stu-id="10ee2-123">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest</span></span>

<span data-ttu-id="10ee2-124">HLS:</span><span class="sxs-lookup"><span data-stu-id="10ee2-124">HLS:</span></span>

<span data-ttu-id="10ee2-125">{strömmande slutpunkten namn media services-konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)</span><span class="sxs-lookup"><span data-stu-id="10ee2-125">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)</span></span>

<span data-ttu-id="10ee2-126">MPEG DASH</span><span class="sxs-lookup"><span data-stu-id="10ee2-126">MPEG DASH</span></span>

<span data-ttu-id="10ee2-127">{strömmande slutpunkten namn media services-konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)</span><span class="sxs-lookup"><span data-stu-id="10ee2-127">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)</span></span>


<span data-ttu-id="10ee2-128">Anvisningar för hur du publicerar en tillgång och skapar en strömnings-URL finns i [Skapa en strömnings-URL](media-services-deliver-streaming-content.md).</span><span class="sxs-lookup"><span data-stu-id="10ee2-128">For instructions on how to publish an asset and build a streaming URL, see [Build a streaming URL](media-services-deliver-streaming-content.md).</span></span>

## <a name="considerations"></a><span data-ttu-id="10ee2-129">Överväganden</span><span class="sxs-lookup"><span data-stu-id="10ee2-129">Considerations</span></span>
* <span data-ttu-id="10ee2-130">Du kan inte ta bort en AssetDeliveryPolicy som är associerade med en tillgång, medan det finns en (streaming) positionerare för tillgången.</span><span class="sxs-lookup"><span data-stu-id="10ee2-130">You cannot delete an AssetDeliveryPolicy associated with an asset while an OnDemand (streaming) locator exists for that asset.</span></span> <span data-ttu-id="10ee2-131">Det rekommenderas att ta bort principen från tillgången innan du tar bort principen.</span><span class="sxs-lookup"><span data-stu-id="10ee2-131">The recommendation is to remove the policy from the asset before deleting the policy.</span></span>
* <span data-ttu-id="10ee2-132">En strömningslokaliserare kan inte skapas på en krypterad tillgång lagring när någon tillgångsleveransprincip anges.</span><span class="sxs-lookup"><span data-stu-id="10ee2-132">A streaming locator cannot be created on a storage encrypted asset when no asset delivery policy is set.</span></span>  <span data-ttu-id="10ee2-133">Om tillgången är lagringskrypterad, kan systemet du skapa en positionerare och strömma tillgången i klartext utan en tillgångsleveransprincip.</span><span class="sxs-lookup"><span data-stu-id="10ee2-133">If the Asset isn’t storage encrypted, the system will let you create a locator and stream the asset in the clear without an asset delivery policy.</span></span>
* <span data-ttu-id="10ee2-134">Du kan ha flera principerna för tillgångsleverans som är associerade med en enda resurs, men du kan bara ange ett sätt att hantera en viss AssetDeliveryProtocol.</span><span class="sxs-lookup"><span data-stu-id="10ee2-134">You can have multiple asset delivery policies associated with a single asset but you can only specify one way to handle a given AssetDeliveryProtocol.</span></span>  <span data-ttu-id="10ee2-135">Vilket innebär att om du försöker länka två leveransprinciperna som anger protokollet AssetDeliveryProtocol.SmoothStreaming som leder till ett fel eftersom systemet inte vet vilken du vill att det ska tillämpas när en klient gör en begäran om Smooth Streaming.</span><span class="sxs-lookup"><span data-stu-id="10ee2-135">Meaning if you try to link two delivery policies that specify the AssetDeliveryProtocol.SmoothStreaming protocol that will result in an error because the system does not know which one you want it to apply when a client makes a Smooth Streaming request.</span></span>
* <span data-ttu-id="10ee2-136">Om du har en tillgång med en befintlig strömningslokaliserare kan inte länka en ny princip till tillgången, Avlänka en befintlig princip från tillgången eller uppdatera en leveransprincip som är kopplade till tillgången.</span><span class="sxs-lookup"><span data-stu-id="10ee2-136">If you have an asset with an existing streaming locator, you cannot link a new policy to the asset, unlink an existing policy from the asset, or update a delivery policy associated with the asset.</span></span>  <span data-ttu-id="10ee2-137">Du måste först ta bort strömningspositioneraren, justera principerna och sedan återskapa strömningspositioneraren.</span><span class="sxs-lookup"><span data-stu-id="10ee2-137">You first have to remove the streaming locator, adjust the policies, and then re-create the streaming locator.</span></span>  <span data-ttu-id="10ee2-138">Du kan använda samma locatorId när du återskapa strömningslokaliseraren men du bör se till att den orsakar problem för klienter eftersom innehållet kan cachelagras av ursprung eller en underordnad CDN.</span><span class="sxs-lookup"><span data-stu-id="10ee2-138">You can use the same locatorId when you recreate the streaming locator but you should ensure that won’t cause issues for clients since content can be cached by the origin or a downstream CDN.</span></span>

>[!NOTE]

><span data-ttu-id="10ee2-139">Vid åtkomst till entiteter i Media Services måste du ange specifika namn på huvudfält och värden i HTTP-begäranden.</span><span class="sxs-lookup"><span data-stu-id="10ee2-139">When accessing entities in Media Services, you must set specific header fields and values in your HTTP requests.</span></span> <span data-ttu-id="10ee2-140">Mer information finns i [installationsprogrammet för Media Services REST API-utveckling](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="10ee2-140">For more information, see [Setup for Media Services REST API Development](media-services-rest-how-to-use.md).</span></span>

## <a name="connect-to-media-services"></a><span data-ttu-id="10ee2-141">Ansluta till Media Services</span><span class="sxs-lookup"><span data-stu-id="10ee2-141">Connect to Media Services</span></span>

<span data-ttu-id="10ee2-142">Information om hur du ansluter till AMS API: et finns [åtkomst till Azure Media Services-API med Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="10ee2-142">For information on how to connect to the AMS API, see [Access the Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

>[!NOTE]
><span data-ttu-id="10ee2-143">När du har anslutit till https://media.windows.net, får du en 301 omdirigering att ange en annan Media Services-URI.</span><span class="sxs-lookup"><span data-stu-id="10ee2-143">After successfully connecting to https://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="10ee2-144">Du måste göra följande anrop till en ny URI.</span><span class="sxs-lookup"><span data-stu-id="10ee2-144">You must make subsequent calls to the new URI.</span></span>

## <a name="clear-asset-delivery-policy"></a><span data-ttu-id="10ee2-145">Rensa tillgångsleveransprincip</span><span class="sxs-lookup"><span data-stu-id="10ee2-145">Clear asset delivery policy</span></span>
### <span data-ttu-id="10ee2-146"><a id="create_asset_delivery_policy"></a>Skapa tillgångsleveransprincip</span><span class="sxs-lookup"><span data-stu-id="10ee2-146"><a id="create_asset_delivery_policy"></a>Create asset delivery policy</span></span>
<span data-ttu-id="10ee2-147">Följande HTTP-begäran skapar en tillgångsleveransprincip som anger att inte använda dynamisk kryptering och för att leverera dataströmmen i något av följande protokoll: MPEG DASH, HLS och Smooth Streaming-protokoll.</span><span class="sxs-lookup"><span data-stu-id="10ee2-147">The following HTTP request creates an asset delivery policy that specifies to not apply dynamic encryption and to deliver the stream in any of the following protocols:  MPEG DASH, HLS, and Smooth Streaming protocols.</span></span> 

<span data-ttu-id="10ee2-148">Information om vilka värden du kan ange när du skapar en AssetDeliveryPolicy finns i [typer som används när du definierar AssetDeliveryPolicy](#types) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="10ee2-148">For information on what values you can specify when creating an AssetDeliveryPolicy, see the [Types used when defining AssetDeliveryPolicy](#types) section.</span></span>   

<span data-ttu-id="10ee2-149">Begäran:</span><span class="sxs-lookup"><span data-stu-id="10ee2-149">Request:</span></span>

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

<span data-ttu-id="10ee2-150">Svar:</span><span class="sxs-lookup"><span data-stu-id="10ee2-150">Response:</span></span>

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

### <span data-ttu-id="10ee2-151"><a id="link_asset_with_asset_delivery_policy"></a>Länken tillgång med tillgångsleveransprincip</span><span class="sxs-lookup"><span data-stu-id="10ee2-151"><a id="link_asset_with_asset_delivery_policy"></a>Link asset with asset delivery policy</span></span>
<span data-ttu-id="10ee2-152">Följande HTTP-begäran länkar den angivna tillgången till tillgångsleveransprincip till.</span><span class="sxs-lookup"><span data-stu-id="10ee2-152">The following HTTP request links the specified asset to the asset delivery policy to.</span></span>

<span data-ttu-id="10ee2-153">Begäran:</span><span class="sxs-lookup"><span data-stu-id="10ee2-153">Request:</span></span>

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

<span data-ttu-id="10ee2-154">Svar:</span><span class="sxs-lookup"><span data-stu-id="10ee2-154">Response:</span></span>

    HTTP/1.1 204 No Content


## <a name="dynamicenvelopeencryption-asset-delivery-policy"></a><span data-ttu-id="10ee2-155">DynamicEnvelopeEncryption tillgångsleveransprincip</span><span class="sxs-lookup"><span data-stu-id="10ee2-155">DynamicEnvelopeEncryption asset delivery policy</span></span>
### <a name="create-content-key-of-the-envelopeencryption-type-and-link-it-to-the-asset"></a><span data-ttu-id="10ee2-156">Skapa innehållsnyckeln av typen EnvelopeEncryption och länka det till tillgången</span><span class="sxs-lookup"><span data-stu-id="10ee2-156">Create content key of the EnvelopeEncryption type and link it to the asset</span></span>
<span data-ttu-id="10ee2-157">När du anger DynamicEnvelopeEncryption leveransprincip måste se till att länka din tillgång till en innehållsnyckel av typen EnvelopeEncryption.</span><span class="sxs-lookup"><span data-stu-id="10ee2-157">When specifying DynamicEnvelopeEncryption delivery policy, you need to make sure to link your asset to a content key of the EnvelopeEncryption type.</span></span> <span data-ttu-id="10ee2-158">Mer information finns: [att skapa en innehållsnyckel](media-services-rest-create-contentkey.md)).</span><span class="sxs-lookup"><span data-stu-id="10ee2-158">For more information, see: [Creating a content key](media-services-rest-create-contentkey.md)).</span></span>

### <span data-ttu-id="10ee2-159"><a id="get_delivery_url"></a>Hämta URL för leverans</span><span class="sxs-lookup"><span data-stu-id="10ee2-159"><a id="get_delivery_url"></a>Get delivery URL</span></span>
<span data-ttu-id="10ee2-160">Hämta leverans URL för den angivna leveransmetoden för innehållsnyckeln skapade i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="10ee2-160">Get the delivery URL for the specified delivery method of the content key created in the previous step.</span></span> <span data-ttu-id="10ee2-161">En klient använder returnerade URL: en för att begära en AES-nyckel eller en PlayReady-licenser för att spela upp det skyddade innehållet.</span><span class="sxs-lookup"><span data-stu-id="10ee2-161">A client uses the returned URL to request an AES key or a PlayReady license in order to playback the protected content.</span></span>

<span data-ttu-id="10ee2-162">Ange vilken typ av URL: en för att hämta i brödtexten för HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="10ee2-162">Specify the type of the URL to get in the body of the HTTP request.</span></span> <span data-ttu-id="10ee2-163">Om du skyddar ditt innehåll med PlayReady Media Services PlayReady licens förvärv URL-begäran med 1 för keyDeliveryType: {”keyDeliveryType”: 1}.</span><span class="sxs-lookup"><span data-stu-id="10ee2-163">If you are protecting your content with PlayReady, request a Media Services PlayReady license acquisition URL, using 1 for the keyDeliveryType: {"keyDeliveryType":1}.</span></span> <span data-ttu-id="10ee2-164">Om du skyddar ditt innehåll med kuvert kryptering URL-begäran en nyckel förvärv genom att ange 2 för keyDeliveryType: {”keyDeliveryType”: 2}.</span><span class="sxs-lookup"><span data-stu-id="10ee2-164">If you are protecting your content with the envelope encryption, request a key acquisition URL by specifying 2 for keyDeliveryType: {"keyDeliveryType":2}.</span></span>

<span data-ttu-id="10ee2-165">Begäran:</span><span class="sxs-lookup"><span data-stu-id="10ee2-165">Request:</span></span>

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

<span data-ttu-id="10ee2-166">Svar:</span><span class="sxs-lookup"><span data-stu-id="10ee2-166">Response:</span></span>

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


### <a name="create-asset-delivery-policy"></a><span data-ttu-id="10ee2-167">Skapa tillgångsleveransprincip</span><span class="sxs-lookup"><span data-stu-id="10ee2-167">Create asset delivery policy</span></span>
<span data-ttu-id="10ee2-168">Följande HTTP-begäran skapas i **AssetDeliveryPolicy** som är konfigurerad för att använda dynamiska kryptering (**DynamicEnvelopeEncryption**) till den **HLS** protokollet (i det här exemplet andra protokoll kommer att blockeras från strömning).</span><span class="sxs-lookup"><span data-stu-id="10ee2-168">The following HTTP request creates the **AssetDeliveryPolicy** that is configured to apply dynamic envelope encryption (**DynamicEnvelopeEncryption**) to the **HLS** protocol (in this example, other protocols will be blocked from streaming).</span></span> 

<span data-ttu-id="10ee2-169">Information om vilka värden du kan ange när du skapar en AssetDeliveryPolicy finns i [typer som används när du definierar AssetDeliveryPolicy](#types) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="10ee2-169">For information on what values you can specify when creating an AssetDeliveryPolicy, see the [Types used when defining AssetDeliveryPolicy](#types) section.</span></span>   

<span data-ttu-id="10ee2-170">Begäran:</span><span class="sxs-lookup"><span data-stu-id="10ee2-170">Request:</span></span>

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


<span data-ttu-id="10ee2-171">Svar:</span><span class="sxs-lookup"><span data-stu-id="10ee2-171">Response:</span></span>

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


### <a name="link-asset-with-asset-delivery-policy"></a><span data-ttu-id="10ee2-172">Länken tillgång med tillgångsleveransprincip</span><span class="sxs-lookup"><span data-stu-id="10ee2-172">Link asset with asset delivery policy</span></span>
<span data-ttu-id="10ee2-173">Se [länk tillgång med tillgångsleveransprincip](#link_asset_with_asset_delivery_policy)</span><span class="sxs-lookup"><span data-stu-id="10ee2-173">See [Link asset with asset delivery policy](#link_asset_with_asset_delivery_policy)</span></span>

## <a name="dynamiccommonencryption-asset-delivery-policy"></a><span data-ttu-id="10ee2-174">DynamicCommonEncryption tillgångsleveransprincip</span><span class="sxs-lookup"><span data-stu-id="10ee2-174">DynamicCommonEncryption asset delivery policy</span></span>
### <a name="create-content-key-of-the-commonencryption-type-and-link-it-to-the-asset"></a><span data-ttu-id="10ee2-175">Skapa innehållsnyckeln av typen CommonEncryption och länka det till tillgången</span><span class="sxs-lookup"><span data-stu-id="10ee2-175">Create content key of the CommonEncryption type and link it to the asset</span></span>
<span data-ttu-id="10ee2-176">När du anger DynamicCommonEncryption leveransprincip måste se till att länka din tillgång till en innehållsnyckel av typen CommonEncryption.</span><span class="sxs-lookup"><span data-stu-id="10ee2-176">When specifying DynamicCommonEncryption delivery policy, you need to make sure to link your asset to a content key of the CommonEncryption type.</span></span> <span data-ttu-id="10ee2-177">Mer information finns: [att skapa en innehållsnyckel](media-services-rest-create-contentkey.md)).</span><span class="sxs-lookup"><span data-stu-id="10ee2-177">For more information, see: [Creating a content key](media-services-rest-create-contentkey.md)).</span></span>

### <a name="get-delivery-url"></a><span data-ttu-id="10ee2-178">Hämta URL för leverans</span><span class="sxs-lookup"><span data-stu-id="10ee2-178">Get Delivery URL</span></span>
<span data-ttu-id="10ee2-179">Hämta leverans URL för PlayReady leveransmetod för innehållsnyckeln skapade i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="10ee2-179">Get the delivery URL for the PlayReady delivery method of the content key created in the previous step.</span></span> <span data-ttu-id="10ee2-180">En klient använder returnerade URL: en för att begära en PlayReady-licens för att spela upp skyddat innehåll.</span><span class="sxs-lookup"><span data-stu-id="10ee2-180">A client uses the returned URL to request a PlayReady license in order to playback the protected content.</span></span> <span data-ttu-id="10ee2-181">Mer information finns i [Hämta leverans URL](#get_delivery_url).</span><span class="sxs-lookup"><span data-stu-id="10ee2-181">For more information, see [Get Delivery URL](#get_delivery_url).</span></span>

### <a name="create-asset-delivery-policy"></a><span data-ttu-id="10ee2-182">Skapa tillgångsleveransprincip</span><span class="sxs-lookup"><span data-stu-id="10ee2-182">Create asset delivery policy</span></span>
<span data-ttu-id="10ee2-183">Följande HTTP-begäran skapas i **AssetDeliveryPolicy** som är konfigurerad för att använda dynamisk vanliga kryptering (**DynamicCommonEncryption**) till den **Smooth Streaming**protocol (i det här exemplet andra protokoll kommer att blockeras från strömning).</span><span class="sxs-lookup"><span data-stu-id="10ee2-183">The following HTTP request creates the **AssetDeliveryPolicy** that is configured to apply dynamic common encryption (**DynamicCommonEncryption**) to the **Smooth Streaming** protocol (in this example, other protocols will be blocked from streaming).</span></span> 

<span data-ttu-id="10ee2-184">Information om vilka värden du kan ange när du skapar en AssetDeliveryPolicy finns i [typer som används när du definierar AssetDeliveryPolicy](#types) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="10ee2-184">For information on what values you can specify when creating an AssetDeliveryPolicy, see the [Types used when defining AssetDeliveryPolicy](#types) section.</span></span>   

<span data-ttu-id="10ee2-185">Begäran:</span><span class="sxs-lookup"><span data-stu-id="10ee2-185">Request:</span></span>

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


<span data-ttu-id="10ee2-186">Om du vill skydda innehåll med Widevine DRM uppdatera AssetDeliveryConfiguration värden om du vill använda WidevineLicenseAcquisitionUrl (som har värdet 7) och ange Webbadressen till en licensleveranstjänst.</span><span class="sxs-lookup"><span data-stu-id="10ee2-186">If you want to protect your content using Widevine DRM, update the AssetDeliveryConfiguration values to use WidevineLicenseAcquisitionUrl (which has the value of 7) and specify the URL of a license delivery service.</span></span> <span data-ttu-id="10ee2-187">Du kan använda följande AMS-partner som hjälper dig att leverera Widevine-licenser: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).</span><span class="sxs-lookup"><span data-stu-id="10ee2-187">You can use the following AMS partners to help you deliver Widevine licenses: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).</span></span>

<span data-ttu-id="10ee2-188">Exempel:</span><span class="sxs-lookup"><span data-stu-id="10ee2-188">For example:</span></span> 

    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":2,"AssetDeliveryPolicyType":4,"AssetDeliveryConfiguration":"[{\"Key\":7,\"Value\":\"https:\\/\\/example.net\/WidevineLicenseAcquisition\/"}]"}

> [!NOTE]
> <span data-ttu-id="10ee2-189">Vid kryptering med Widevine, skulle du bara att kunna leverera med bindestreck.</span><span class="sxs-lookup"><span data-stu-id="10ee2-189">When encrypting with Widevine, you would only be able to deliver using DASH.</span></span> <span data-ttu-id="10ee2-190">Se till att ange STRECK (2) i protokollet för tillgångsleverans.</span><span class="sxs-lookup"><span data-stu-id="10ee2-190">Make sure to specify DASH (2) in the asset delivery protocol.</span></span>
> 
> 

### <a name="link-asset-with-asset-delivery-policy"></a><span data-ttu-id="10ee2-191">Länken tillgång med tillgångsleveransprincip</span><span class="sxs-lookup"><span data-stu-id="10ee2-191">Link asset with asset delivery policy</span></span>
<span data-ttu-id="10ee2-192">Se [länk tillgång med tillgångsleveransprincip](#link_asset_with_asset_delivery_policy)</span><span class="sxs-lookup"><span data-stu-id="10ee2-192">See [Link asset with asset delivery policy](#link_asset_with_asset_delivery_policy)</span></span>

## <span data-ttu-id="10ee2-193"><a id="types"></a>Typer som används när du definierar AssetDeliveryPolicy</span><span class="sxs-lookup"><span data-stu-id="10ee2-193"><a id="types"></a>Types used when defining AssetDeliveryPolicy</span></span>

### <a name="assetdeliveryprotocol"></a><span data-ttu-id="10ee2-194">AssetDeliveryProtocol</span><span class="sxs-lookup"><span data-stu-id="10ee2-194">AssetDeliveryProtocol</span></span>

<span data-ttu-id="10ee2-195">Följande enum beskrivs värden som du kan ange för protokollet för tillgångsleverans.</span><span class="sxs-lookup"><span data-stu-id="10ee2-195">The following enum describes values you can set for the asset delivery protocol.</span></span>

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

### <a name="assetdeliverypolicytype"></a><span data-ttu-id="10ee2-196">AssetDeliveryPolicyType</span><span class="sxs-lookup"><span data-stu-id="10ee2-196">AssetDeliveryPolicyType</span></span>

<span data-ttu-id="10ee2-197">Följande enum beskriver värden som du kan ange för tillgångstyp leverans princip.</span><span class="sxs-lookup"><span data-stu-id="10ee2-197">The following enum describes values you can set for the asset delivery policy type.</span></span>  

    public enum AssetDeliveryPolicyType
    {
        /// <summary>
        /// Delivery Policy Type not set.  An invalid value.
        /// </summary>
        None,

        /// <summary>
        /// The Asset should not be delivered via this AssetDeliveryProtocol. 
        /// </summary>
        Blocked, 

        /// <summary>
        /// Do not apply dynamic encryption to the asset.
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

### <a name="contentkeydeliverytype"></a><span data-ttu-id="10ee2-198">ContentKeyDeliveryType</span><span class="sxs-lookup"><span data-stu-id="10ee2-198">ContentKeyDeliveryType</span></span>

<span data-ttu-id="10ee2-199">Följande enum beskrivs värden som du kan använda för att konfigurera leveransmetod för innehållsnyckeln till klienten.</span><span class="sxs-lookup"><span data-stu-id="10ee2-199">The following enum describes values you can use to configure the delivery method of the content key to the client.</span></span>
    
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


### <a name="assetdeliverypolicyconfigurationkey"></a><span data-ttu-id="10ee2-200">AssetDeliveryPolicyConfigurationKey</span><span class="sxs-lookup"><span data-stu-id="10ee2-200">AssetDeliveryPolicyConfigurationKey</span></span>

<span data-ttu-id="10ee2-201">Följande enum beskrivs värden som du kan ange för att konfigurera nycklar som används för att få specifik konfiguration för en tillgångsleveransprincip.</span><span class="sxs-lookup"><span data-stu-id="10ee2-201">The following enum describes values you can set to configure keys used to get specific configuration for an asset delivery policy.</span></span>

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
        /// The initialization vector to use for envelope encryption in Base64 format.
        /// </summary>
        EnvelopeEncryptionIVAsBase64,

        /// <summary>
        /// The PlayReady License Acquisition Url to use for common encryption.
        /// </summary>
        PlayReadyLicenseAcquisitionUrl,

        /// <summary>
        /// The PlayReady Custom Attributes to add to the PlayReady Content Header
        /// </summary>
        PlayReadyCustomAttributes,

        /// <summary>
        /// The initialization vector to use for envelope encryption.
        /// </summary>
        EnvelopeEncryptionIV,

        /// <summary>
        /// Widevine DRM acquisition url
        /// </summary>
        WidevineLicenseAcquisitionUrl
    }

## <a name="media-services-learning-paths"></a><span data-ttu-id="10ee2-202">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="10ee2-202">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="10ee2-203">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="10ee2-203">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

