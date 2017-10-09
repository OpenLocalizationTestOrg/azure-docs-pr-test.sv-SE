---
title: "principerna för tillgångsleverans aaaConfigure med .NET SDK | Microsoft Docs"
description: "Det här avsnittet visar hur tooconfigure olika principerna för tillgångsleverans med Azure Media Services .NET SDK."
services: media-services
documentationcenter: 
author: Mingfeiy
manager: cfowler
editor: 
ms.assetid: 3ec46f58-6cbb-4d49-bac6-1fd01a5a456b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/13/2017
ms.author: juliako;mingfeiy
ms.openlocfilehash: a6f2644d639cd36d4cdc269b6f01fd4acdf7160b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-asset-delivery-policies-with-net-sdk"></a><span data-ttu-id="05335-103">Konfigurera principerna för tillgångsleverans med .NET SDK</span><span class="sxs-lookup"><span data-stu-id="05335-103">Configure asset delivery policies with .NET SDK</span></span>
[!INCLUDE [media-services-selector-asset-delivery-policy](../../includes/media-services-selector-asset-delivery-policy.md)]

## <a name="overview"></a><span data-ttu-id="05335-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="05335-104">Overview</span></span>
<span data-ttu-id="05335-105">Om du planerar toodelivery krypterade tillgångar, steg ett av hello i hello Media Services content delivery arbetsflödet konfigurera leveransprinciperna för tillgångar.</span><span class="sxs-lookup"><span data-stu-id="05335-105">If you plan toodelivery encrypted assets, one of hello steps in hello Media Services content delivery workflow is configuring delivery policies for assets.</span></span> <span data-ttu-id="05335-106">Hej tillgångsleveransprincip talar om för Media Services hur du vill använda för din tillgång toobe levereras: i vilket strömningsprotokoll bör din tillgång dynamiskt paketeras (till exempel MPEG DASH, HLS, Smooth Streaming eller alla), oavsett om du vill toodynamically kryptera din tillgång och hur (envelope eller vanliga kryptering).</span><span class="sxs-lookup"><span data-stu-id="05335-106">hello asset delivery policy tells Media Services how you want for your asset toobe delivered: into which streaming protocol should your asset be dynamically packaged (for example, MPEG DASH, HLS, Smooth Streaming, or all), whether or not you want toodynamically encrypt your asset and how (envelope or common encryption).</span></span>

<span data-ttu-id="05335-107">Det här avsnittet beskrivs hur och varför toocreate och konfigurera leveransprinciperna för tillgången.</span><span class="sxs-lookup"><span data-stu-id="05335-107">This topic discusses why and how toocreate and configure asset delivery policies.</span></span>

>[!NOTE]
><span data-ttu-id="05335-108">När AMS-kontot skapas en **standard** strömningsslutpunkt har lagts till tooyour konto i hello **stoppad** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="05335-108">When your AMS account is created a **default** streaming endpoint is added tooyour account in hello **Stopped** state.</span></span> <span data-ttu-id="05335-109">toostart strömning ditt innehåll och dra nytta av dynamisk paketering och dynamisk kryptering hello strömningsslutpunkt som du vill toostream innehåll har toobe i hello **kör** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="05335-109">toostart streaming your content and take advantage of dynamic packaging and dynamic encryption, hello streaming endpoint from which you want toostream content has toobe in hello **Running** state.</span></span> 
>
><span data-ttu-id="05335-110">Dessutom toobe kan toouse dynamisk paketering och dynamisk kryptering din tillgång måste innehålla en uppsättning MP4s med anpassningsbar bithastighet eller Smooth Streaming-filer.</span><span class="sxs-lookup"><span data-stu-id="05335-110">Also, toobe able toouse dynamic packaging and dynamic encryption your asset must contain a set of adaptive bitrate MP4s or adaptive bitrate Smooth Streaming files.</span></span>


<span data-ttu-id="05335-111">Du kan använda olika principer toohello samma tillgång.</span><span class="sxs-lookup"><span data-stu-id="05335-111">You could apply different policies toohello same asset.</span></span> <span data-ttu-id="05335-112">Du kan till exempel använda PlayReady-kryptering tooSmooth Streaming och AES Envelope kryptering tooMPEG DASH och HLS.</span><span class="sxs-lookup"><span data-stu-id="05335-112">For example, you could apply PlayReady encryption tooSmooth Streaming and AES Envelope encryption tooMPEG DASH and HLS.</span></span> <span data-ttu-id="05335-113">Alla protokoll som inte har definierats i en leveransprincip (exempelvis du lägga till en enda princip som endast anger HLS som hello protokoll) kommer att blockeras från strömning.</span><span class="sxs-lookup"><span data-stu-id="05335-113">Any protocols that are not defined in a delivery policy (for example, you add a single policy that only specifies HLS as hello protocol) will be blocked from streaming.</span></span> <span data-ttu-id="05335-114">hello undantag toothis är om du har någon tillgångsleveransprincip alls definierats.</span><span class="sxs-lookup"><span data-stu-id="05335-114">hello exception toothis is if you have no asset delivery policy defined at all.</span></span> <span data-ttu-id="05335-115">Sedan tillåts alla protokoll i hello Rensa.</span><span class="sxs-lookup"><span data-stu-id="05335-115">Then, all protocols will be allowed in hello clear.</span></span>

<span data-ttu-id="05335-116">Om du vill toodeliver en krypterad tillgång för lagring, måste du konfigurera hello tillgångsleveransprincip.</span><span class="sxs-lookup"><span data-stu-id="05335-116">If you want toodeliver a storage encrypted asset, you must configure hello asset’s delivery policy.</span></span> <span data-ttu-id="05335-117">Innan din tillgång kan strömmas angetts hello streaming server tar bort hello lagringskryptering och dataströmmar innehåll med hello principen för tillgångsleverans.</span><span class="sxs-lookup"><span data-stu-id="05335-117">Before your asset can be streamed, hello streaming server removes hello storage encryption and streams your content using hello specified delivery policy.</span></span> <span data-ttu-id="05335-118">Till exempel toodeliver din tillgång krypteras med Advanced Encryption Standard (AES) kuvert krypteringsnyckeln genom att ange hello principtypen för**DynamicEnvelopeEncryption**.</span><span class="sxs-lookup"><span data-stu-id="05335-118">For example, toodeliver your asset encrypted with Advanced Encryption Standard (AES) envelope encryption key, set hello policy type too**DynamicEnvelopeEncryption**.</span></span> <span data-ttu-id="05335-119">tooremove lagringskryptering och dataströmmen hello tillgångar i hello rensa, ange hello princip för**NoDynamicEncryption**.</span><span class="sxs-lookup"><span data-stu-id="05335-119">tooremove storage encryption and stream hello asset in hello clear, set hello policy type too**NoDynamicEncryption**.</span></span> <span data-ttu-id="05335-120">Exempel som visar hur tooconfigure dessa principtyper följer.</span><span class="sxs-lookup"><span data-stu-id="05335-120">Examples that show how tooconfigure these policy types follow.</span></span>

<span data-ttu-id="05335-121">Beroende på hur du konfigurerar hello tillgångsleveransprincip du skulle kunna toodynamically paketet, dynamiskt kryptera och strömma hello följande strömningsprotokoll: Smooth Streaming, HLS och MPEG DASH-dataströmmar.</span><span class="sxs-lookup"><span data-stu-id="05335-121">Depending on how you configure hello asset delivery policy you would be able toodynamically package, dynamically encrypt, and stream hello following streaming protocols: Smooth Streaming, HLS, and MPEG DASH streams.</span></span>

<span data-ttu-id="05335-122">hello följande lista visar hello format att du använder toostream Smooth, HLS och STRECK.</span><span class="sxs-lookup"><span data-stu-id="05335-122">hello following list shows hello formats that you use toostream Smooth, HLS, and DASH.</span></span>

<span data-ttu-id="05335-123">Smooth Streaming:</span><span class="sxs-lookup"><span data-stu-id="05335-123">Smooth Streaming:</span></span>

<span data-ttu-id="05335-124">{namn på slutpunkt för direktuppspelning-namn på mediaservicekonto}.streaming.mediaservices.windows.net/lokalisator-ID}/{filnamn}.ism/Manifest</span><span class="sxs-lookup"><span data-stu-id="05335-124">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest</span></span>

<span data-ttu-id="05335-125">HLS:</span><span class="sxs-lookup"><span data-stu-id="05335-125">HLS:</span></span>

<span data-ttu-id="05335-126">{strömmande slutpunkten namn media services-konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)</span><span class="sxs-lookup"><span data-stu-id="05335-126">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)</span></span>

<span data-ttu-id="05335-127">MPEG DASH</span><span class="sxs-lookup"><span data-stu-id="05335-127">MPEG DASH</span></span>

<span data-ttu-id="05335-128">{strömmande slutpunkten namn media services-konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)</span><span class="sxs-lookup"><span data-stu-id="05335-128">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)</span></span>


## <a name="considerations"></a><span data-ttu-id="05335-129">Överväganden</span><span class="sxs-lookup"><span data-stu-id="05335-129">Considerations</span></span>
* <span data-ttu-id="05335-130">Du kan inte ta bort en AssetDeliveryPolicy som är associerade med en tillgång, medan det finns en (streaming) positionerare för tillgången.</span><span class="sxs-lookup"><span data-stu-id="05335-130">You cannot delete an AssetDeliveryPolicy associated with an asset while an OnDemand (streaming) locator exists for that asset.</span></span> <span data-ttu-id="05335-131">hello rekommendation är tooremove hello principen från hello tillgångsinformation innan du tar bort hello princip.</span><span class="sxs-lookup"><span data-stu-id="05335-131">hello recommendation is tooremove hello policy from hello asset before deleting hello policy.</span></span>
* <span data-ttu-id="05335-132">En strömningslokaliserare kan inte skapas på en krypterad tillgång lagring när någon tillgångsleveransprincip anges.</span><span class="sxs-lookup"><span data-stu-id="05335-132">A streaming locator cannot be created on a storage encrypted asset when no asset delivery policy is set.</span></span>  <span data-ttu-id="05335-133">Om inte hello tillgången är lagringskrypterad, kan hello system du skapa en positionerare och dataströmmen hello tillgång i hello Rensa utan en tillgångsleveransprincip.</span><span class="sxs-lookup"><span data-stu-id="05335-133">If hello Asset isn’t storage encrypted, hello system will let you create a locator and stream hello asset in hello clear without an asset delivery policy.</span></span>
* <span data-ttu-id="05335-134">Du kan ha flera principerna för tillgångsleverans som är associerade med en enda resurs, men du kan bara ange ett sätt toohandle angivna AssetDeliveryProtocol.</span><span class="sxs-lookup"><span data-stu-id="05335-134">You can have multiple asset delivery policies associated with a single asset but you can only specify one way toohandle a given AssetDeliveryProtocol.</span></span>  <span data-ttu-id="05335-135">Vilket innebär att om du toolink två leveransprinciperna som anger hello AssetDeliveryProtocol.SmoothStreaming protokoll som resulterar i ett fel eftersom hello system inte vet vilken vill du tooapply när en klient gör en Smooth Streaming-begäran.</span><span class="sxs-lookup"><span data-stu-id="05335-135">Meaning if you try toolink two delivery policies that specify hello AssetDeliveryProtocol.SmoothStreaming protocol that will result in an error because hello system does not know which one you want it tooapply when a client makes a Smooth Streaming request.</span></span>
* <span data-ttu-id="05335-136">Om du har en tillgång med en befintlig strömmande lokaliserare, kan inte du länka en ny princip toohello tillgång (du kan antingen Avlänka en befintlig princip från hello tillgång eller uppdatera en leveransprincip som är associerade med hello tillgången).</span><span class="sxs-lookup"><span data-stu-id="05335-136">If you have an asset with an existing streaming locator, you cannot link a new policy toohello asset (you can either unlink an existing policy from hello asset, or update a delivery policy associated with hello asset).</span></span>  <span data-ttu-id="05335-137">Du först har tooremove hello strömningslokaliserare, justera hello principer och återskapa hello strömning lokaliserare.</span><span class="sxs-lookup"><span data-stu-id="05335-137">You first have tooremove hello streaming locator, adjust hello policies, and then re-create hello streaming locator.</span></span>  <span data-ttu-id="05335-138">Du kan använda samma locatorId när du skapar nytt hello strömning lokaliserare, men du bör kontrollera hello som inte orsakar problem för klienter eftersom innehållet kan cachelagras av hello ursprung eller en underordnad CDN.</span><span class="sxs-lookup"><span data-stu-id="05335-138">You can use hello same locatorId when you recreate hello streaming locator but you should ensure that won’t cause issues for clients since content can be cached by hello origin or a downstream CDN.</span></span>

## <a name="clear-asset-delivery-policy"></a><span data-ttu-id="05335-139">Rensa tillgångsleveransprincip</span><span class="sxs-lookup"><span data-stu-id="05335-139">Clear asset delivery policy</span></span>

<span data-ttu-id="05335-140">hello följande **ConfigureClearAssetDeliveryPolicy** metoden anger toonot använda dynamisk kryptering och toodeliver hello dataström i någon av hello följande protokoll: MPEG DASH, HLS och Smooth Streaming-protokoll.</span><span class="sxs-lookup"><span data-stu-id="05335-140">hello following **ConfigureClearAssetDeliveryPolicy** method specifies toonot apply dynamic encryption and toodeliver hello stream in any of hello following protocols:  MPEG DASH, HLS, and Smooth Streaming protocols.</span></span> <span data-ttu-id="05335-141">Du kanske vill tooapply den här principen tooyour lagringskrypterad tillgångar.</span><span class="sxs-lookup"><span data-stu-id="05335-141">You might want tooapply this policy tooyour storage encrypted assets.</span></span>

<span data-ttu-id="05335-142">Information om vilka värden som du anger när du skapar en AssetDeliveryPolicy finns hello [typer som används när du definierar AssetDeliveryPolicy](#types) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="05335-142">For information on what values you can specify when creating an AssetDeliveryPolicy, see hello [Types used when defining AssetDeliveryPolicy](#types) section.</span></span>

    static public void ConfigureClearAssetDeliveryPolicy(IAsset asset)
    {
        IAssetDeliveryPolicy policy =
        _context.AssetDeliveryPolicies.Create("Clear Policy",
        AssetDeliveryPolicyType.NoDynamicEncryption,
        AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.Dash, null);
        
        asset.DeliveryPolicies.Add(policy);
    }

## <a name="dynamiccommonencryption-asset-delivery-policy"></a><span data-ttu-id="05335-143">DynamicCommonEncryption tillgångsleveransprincip</span><span class="sxs-lookup"><span data-stu-id="05335-143">DynamicCommonEncryption asset delivery policy</span></span>

<span data-ttu-id="05335-144">hello följande **CreateAssetDeliveryPolicy** metoden skapar hello **AssetDeliveryPolicy** som är konfigurerade tooapply dynamisk vanliga kryptering (**DynamicCommonEncryption**) tooa smooth streaming protocol (andra protokoll kommer att blockeras från strömning).</span><span class="sxs-lookup"><span data-stu-id="05335-144">hello following **CreateAssetDeliveryPolicy** method creates hello **AssetDeliveryPolicy** that is configured tooapply dynamic common encryption (**DynamicCommonEncryption**) tooa smooth streaming protocol (other protocols will be blocked from streaming).</span></span> <span data-ttu-id="05335-145">hello-metoden har två parametrar: **tillgången** (hello tillgången toowhich som du vill tooapply hello leveransprincipen) och **IContentKey** (hello innehållsnyckeln av hello **CommonEncryption**typ för mer information, se: [att skapa en innehållsnyckel](media-services-dotnet-create-contentkey.md#common_contentkey)).</span><span class="sxs-lookup"><span data-stu-id="05335-145">hello method takes two parameters : **Asset** (hello asset toowhich you want tooapply hello delivery policy) and **IContentKey** (hello content key of hello **CommonEncryption** type, for more information, see: [Creating a content key](media-services-dotnet-create-contentkey.md#common_contentkey)).</span></span>

<span data-ttu-id="05335-146">Information om vilka värden som du anger när du skapar en AssetDeliveryPolicy finns hello [typer som används när du definierar AssetDeliveryPolicy](#types) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="05335-146">For information on what values you can specify when creating an AssetDeliveryPolicy, see hello [Types used when defining AssetDeliveryPolicy](#types) section.</span></span>

    static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
    {
        Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);
        
        Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
                new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
            {
                {AssetDeliveryPolicyConfigurationKey.PlayReadyLicenseAcquisitionUrl, acquisitionUrl.ToString()},
            };
    
            var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                    "AssetDeliveryPolicy",
                AssetDeliveryPolicyType.DynamicCommonEncryption,
                AssetDeliveryProtocol.SmoothStreaming,
                assetDeliveryPolicyConfiguration);
    
            // Add AssetDelivery Policy toohello asset
            asset.DeliveryPolicies.Add(assetDeliveryPolicy);
    
            Console.WriteLine();
            Console.WriteLine("Adding Asset Delivery Policy: " +
                assetDeliveryPolicy.AssetDeliveryPolicyType);
     }

<span data-ttu-id="05335-147">Azure Media Services kan du också tooadd Widevine kryptering.</span><span class="sxs-lookup"><span data-stu-id="05335-147">Azure Media Services also enables you tooadd Widevine encryption.</span></span> <span data-ttu-id="05335-148">hello visar exemplet nedan både PlayReady och Widevine som läggs till toohello tillgångsleveransprincip.</span><span class="sxs-lookup"><span data-stu-id="05335-148">hello following example demonstrates both PlayReady and Widevine being added toohello asset delivery policy.</span></span>

    static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
    {
        // Get hello PlayReady license service URL.
        Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);


        // GetKeyDeliveryUrl for Widevine attaches hello KID toohello URL.
        // For example: https://amsaccount1.keydelivery.mediaservices.windows.net/Widevine/?KID=268a6dcb-18c8-4648-8c95-f46429e4927c.  
        // hello WidevineBaseLicenseAcquisitionUrl (used below) also tells Dynamaic Encryption 
        // tooappend /? KID =< keyId > toohello end of hello url when creating hello manifest.
        // As a result Widevine license acquisition URL will have KID appended twice, 
        // so we need tooremove hello KID that in hello URL when we call GetKeyDeliveryUrl.

        Uri widevineUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.Widevine);
        UriBuilder uriBuilder = new UriBuilder(widevineUrl);
        uriBuilder.Query = String.Empty;
        widevineUrl = uriBuilder.Uri;

        Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
        {
            {AssetDeliveryPolicyConfigurationKey.PlayReadyLicenseAcquisitionUrl, acquisitionUrl.ToString()},
            {AssetDeliveryPolicyConfigurationKey.WidevineLicenseAcquisitionUrl, widevineUrl.ToString()}

        };

        var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
            AssetDeliveryPolicyType.DynamicCommonEncryption,
            AssetDeliveryProtocol.Dash,
            assetDeliveryPolicyConfiguration);


        // Add AssetDelivery Policy toohello asset
        asset.DeliveryPolicies.Add(assetDeliveryPolicy);

    }

> [!NOTE]
> <span data-ttu-id="05335-149">Vid kryptering med Widevine, skulle du bara att kunna toodeliver med bindestreck.</span><span class="sxs-lookup"><span data-stu-id="05335-149">When encrypting with Widevine, you would only be able toodeliver using DASH.</span></span> <span data-ttu-id="05335-150">Kontrollera att toospecify STRECK i hello protokollet för tillgångsleverans.</span><span class="sxs-lookup"><span data-stu-id="05335-150">Make sure toospecify DASH in hello asset delivery protocol.</span></span>
> 
> 

## <a name="dynamicenvelopeencryption-asset-delivery-policy"></a><span data-ttu-id="05335-151">DynamicEnvelopeEncryption tillgångsleveransprincip</span><span class="sxs-lookup"><span data-stu-id="05335-151">DynamicEnvelopeEncryption asset delivery policy</span></span>
<span data-ttu-id="05335-152">hello följande **CreateAssetDeliveryPolicy** metoden skapar hello **AssetDeliveryPolicy** som är konfigurerade tooapply dynamiska kryptering (**DynamicEnvelopeEncryption** ) tooSmooth Streaming, HLS och DASH-protokoll (om du väljer toonot ange vissa protokoll, kommer att blockeras från strömning).</span><span class="sxs-lookup"><span data-stu-id="05335-152">hello following **CreateAssetDeliveryPolicy** method creates hello **AssetDeliveryPolicy** that is configured tooapply dynamic envelope encryption (**DynamicEnvelopeEncryption**) tooSmooth Streaming, HLS, and DASH protocols (if you decide toonot specify some protocols, they will be blocked from streaming).</span></span> <span data-ttu-id="05335-153">hello-metoden har två parametrar: **tillgången** (hello tillgången toowhich som du vill tooapply hello leveransprincipen) och **IContentKey** (hello innehållsnyckeln av hello **EnvelopeEncryption**typ för mer information, se: [att skapa en innehållsnyckel](media-services-dotnet-create-contentkey.md#envelope_contentkey)).</span><span class="sxs-lookup"><span data-stu-id="05335-153">hello method takes two parameters : **Asset** (hello asset toowhich you want tooapply hello delivery policy) and **IContentKey** (hello content key of hello **EnvelopeEncryption** type, for more information, see: [Creating a content key](media-services-dotnet-create-contentkey.md#envelope_contentkey)).</span></span>

<span data-ttu-id="05335-154">Information om vilka värden som du anger när du skapar en AssetDeliveryPolicy finns hello [typer som används när du definierar AssetDeliveryPolicy](#types) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="05335-154">For information on what values you can specify when creating an AssetDeliveryPolicy, see hello [Types used when defining AssetDeliveryPolicy](#types) section.</span></span>   

    private static void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
    {

        //  Get hello Key Delivery Base Url by removing hello Query parameter.  hello Dynamic Encryption service will
        //  automatically add hello correct key identifier toohello url when it generates hello Envelope encrypted content
        //  manifest.  Omitting hello IV will also cause hello Dynamice Encryption service toogenerate a deterministic
        //  IV for hello content automatically.  By using hello EnvelopeBaseKeyAcquisitionUrl and omitting hello IV, this
        //  allows hello AssetDelivery policy toobe reused by more than one asset.
        //
        Uri keyAcquisitionUri = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.BaselineHttp);
        UriBuilder uriBuilder = new UriBuilder(keyAcquisitionUri);
        uriBuilder.Query = String.Empty;
        keyAcquisitionUri = uriBuilder.Uri;

        // hello following policy configuration specifies: 
        //   key url that will have KID=<Guid> appended toohello envelope and
        //   hello Initialization Vector (IV) toouse for hello envelope encryption.
        Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string> 
        {
            {AssetDeliveryPolicyConfigurationKey.EnvelopeBaseKeyAcquisitionUrl, keyAcquisitionUri.ToString()},
        };

        IAssetDeliveryPolicy assetDeliveryPolicy =
            _context.AssetDeliveryPolicies.Create(
                        "AssetDeliveryPolicy",
                        AssetDeliveryPolicyType.DynamicEnvelopeEncryption,
                        AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.Dash,
                        assetDeliveryPolicyConfiguration);

        // Add AssetDelivery Policy toohello asset
        asset.DeliveryPolicies.Add(assetDeliveryPolicy);

        Console.WriteLine();
        Console.WriteLine("Adding Asset Delivery Policy: " + assetDeliveryPolicy.AssetDeliveryPolicyType);
    }


## <span data-ttu-id="05335-155"><a id="types"></a>Typer som används när du definierar AssetDeliveryPolicy</span><span class="sxs-lookup"><span data-stu-id="05335-155"><a id="types"></a>Types used when defining AssetDeliveryPolicy</span></span>

### <span data-ttu-id="05335-156"><a id="AssetDeliveryProtocol"></a>AssetDeliveryProtocol</span><span class="sxs-lookup"><span data-stu-id="05335-156"><a id="AssetDeliveryProtocol"></a>AssetDeliveryProtocol</span></span>

<span data-ttu-id="05335-157">hello beskrivs följande enum värden som du kan ange för hello protokollet för tillgångsleverans.</span><span class="sxs-lookup"><span data-stu-id="05335-157">hello following enum describes values you can set for hello asset delivery protocol.</span></span>

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

### <span data-ttu-id="05335-158"><a id="AssetDeliveryPolicyType"></a>AssetDeliveryPolicyType</span><span class="sxs-lookup"><span data-stu-id="05335-158"><a id="AssetDeliveryPolicyType"></a>AssetDeliveryPolicyType</span></span>

<span data-ttu-id="05335-159">hello beskriver följande enum värden som du kan ange för hello tillgångstyp leverans princip.</span><span class="sxs-lookup"><span data-stu-id="05335-159">hello following enum describes values you can set for hello asset delivery policy type.</span></span>  

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

### <span data-ttu-id="05335-160"><a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType</span><span class="sxs-lookup"><span data-stu-id="05335-160"><a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType</span></span>

<span data-ttu-id="05335-161">hello beskriver följande enum värden som du kan använda tooconfigure hello leveransmetod av hello innehåll viktiga toohello-klienten.</span><span class="sxs-lookup"><span data-stu-id="05335-161">hello following enum describes values you can use tooconfigure hello delivery method of hello content key toohello client.</span></span>
    
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

### <span data-ttu-id="05335-162"><a id="AssetDeliveryPolicyConfigurationKey"></a>AssetDeliveryPolicyConfigurationKey</span><span class="sxs-lookup"><span data-stu-id="05335-162"><a id="AssetDeliveryPolicyConfigurationKey"></a>AssetDeliveryPolicyConfigurationKey</span></span>

<span data-ttu-id="05335-163">hello följande enum beskrivs värden som du kan ange tooconfigure nycklar som används tooget specifik konfiguration för en tillgångsleveransprincip.</span><span class="sxs-lookup"><span data-stu-id="05335-163">hello following enum describes values you can set tooconfigure keys used tooget specific configuration for an asset delivery policy.</span></span>

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

## <a name="media-services-learning-paths"></a><span data-ttu-id="05335-164">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="05335-164">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="05335-165">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="05335-165">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

