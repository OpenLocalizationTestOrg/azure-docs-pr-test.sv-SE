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
# <a name="configure-asset-delivery-policies-with-net-sdk"></a>Konfigurera principerna för tillgångsleverans med .NET SDK
[!INCLUDE [media-services-selector-asset-delivery-policy](../../includes/media-services-selector-asset-delivery-policy.md)]

## <a name="overview"></a>Översikt
Om du planerar toodelivery krypterade tillgångar, steg ett av hello i hello Media Services content delivery arbetsflödet konfigurera leveransprinciperna för tillgångar. Hej tillgångsleveransprincip talar om för Media Services hur du vill använda för din tillgång toobe levereras: i vilket strömningsprotokoll bör din tillgång dynamiskt paketeras (till exempel MPEG DASH, HLS, Smooth Streaming eller alla), oavsett om du vill toodynamically kryptera din tillgång och hur (envelope eller vanliga kryptering).

Det här avsnittet beskrivs hur och varför toocreate och konfigurera leveransprinciperna för tillgången.

>[!NOTE]
>När AMS-kontot skapas en **standard** strömningsslutpunkt har lagts till tooyour konto i hello **stoppad** tillstånd. toostart strömning ditt innehåll och dra nytta av dynamisk paketering och dynamisk kryptering hello strömningsslutpunkt som du vill toostream innehåll har toobe i hello **kör** tillstånd. 
>
>Dessutom toobe kan toouse dynamisk paketering och dynamisk kryptering din tillgång måste innehålla en uppsättning MP4s med anpassningsbar bithastighet eller Smooth Streaming-filer.


Du kan använda olika principer toohello samma tillgång. Du kan till exempel använda PlayReady-kryptering tooSmooth Streaming och AES Envelope kryptering tooMPEG DASH och HLS. Alla protokoll som inte har definierats i en leveransprincip (exempelvis du lägga till en enda princip som endast anger HLS som hello protokoll) kommer att blockeras från strömning. hello undantag toothis är om du har någon tillgångsleveransprincip alls definierats. Sedan tillåts alla protokoll i hello Rensa.

Om du vill toodeliver en krypterad tillgång för lagring, måste du konfigurera hello tillgångsleveransprincip. Innan din tillgång kan strömmas angetts hello streaming server tar bort hello lagringskryptering och dataströmmar innehåll med hello principen för tillgångsleverans. Till exempel toodeliver din tillgång krypteras med Advanced Encryption Standard (AES) kuvert krypteringsnyckeln genom att ange hello principtypen för**DynamicEnvelopeEncryption**. tooremove lagringskryptering och dataströmmen hello tillgångar i hello rensa, ange hello princip för**NoDynamicEncryption**. Exempel som visar hur tooconfigure dessa principtyper följer.

Beroende på hur du konfigurerar hello tillgångsleveransprincip du skulle kunna toodynamically paketet, dynamiskt kryptera och strömma hello följande strömningsprotokoll: Smooth Streaming, HLS och MPEG DASH-dataströmmar.

hello följande lista visar hello format att du använder toostream Smooth, HLS och STRECK.

Smooth Streaming:

{namn på slutpunkt för direktuppspelning-namn på mediaservicekonto}.streaming.mediaservices.windows.net/lokalisator-ID}/{filnamn}.ism/Manifest

HLS:

{strömmande slutpunkten namn media services-konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

MPEG DASH

{strömmande slutpunkten namn media services-konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


## <a name="considerations"></a>Överväganden
* Du kan inte ta bort en AssetDeliveryPolicy som är associerade med en tillgång, medan det finns en (streaming) positionerare för tillgången. hello rekommendation är tooremove hello principen från hello tillgångsinformation innan du tar bort hello princip.
* En strömningslokaliserare kan inte skapas på en krypterad tillgång lagring när någon tillgångsleveransprincip anges.  Om inte hello tillgången är lagringskrypterad, kan hello system du skapa en positionerare och dataströmmen hello tillgång i hello Rensa utan en tillgångsleveransprincip.
* Du kan ha flera principerna för tillgångsleverans som är associerade med en enda resurs, men du kan bara ange ett sätt toohandle angivna AssetDeliveryProtocol.  Vilket innebär att om du toolink två leveransprinciperna som anger hello AssetDeliveryProtocol.SmoothStreaming protokoll som resulterar i ett fel eftersom hello system inte vet vilken vill du tooapply när en klient gör en Smooth Streaming-begäran.
* Om du har en tillgång med en befintlig strömmande lokaliserare, kan inte du länka en ny princip toohello tillgång (du kan antingen Avlänka en befintlig princip från hello tillgång eller uppdatera en leveransprincip som är associerade med hello tillgången).  Du först har tooremove hello strömningslokaliserare, justera hello principer och återskapa hello strömning lokaliserare.  Du kan använda samma locatorId när du skapar nytt hello strömning lokaliserare, men du bör kontrollera hello som inte orsakar problem för klienter eftersom innehållet kan cachelagras av hello ursprung eller en underordnad CDN.

## <a name="clear-asset-delivery-policy"></a>Rensa tillgångsleveransprincip

hello följande **ConfigureClearAssetDeliveryPolicy** metoden anger toonot använda dynamisk kryptering och toodeliver hello dataström i någon av hello följande protokoll: MPEG DASH, HLS och Smooth Streaming-protokoll. Du kanske vill tooapply den här principen tooyour lagringskrypterad tillgångar.

Information om vilka värden som du anger när du skapar en AssetDeliveryPolicy finns hello [typer som används när du definierar AssetDeliveryPolicy](#types) avsnitt.

    static public void ConfigureClearAssetDeliveryPolicy(IAsset asset)
    {
        IAssetDeliveryPolicy policy =
        _context.AssetDeliveryPolicies.Create("Clear Policy",
        AssetDeliveryPolicyType.NoDynamicEncryption,
        AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.Dash, null);
        
        asset.DeliveryPolicies.Add(policy);
    }

## <a name="dynamiccommonencryption-asset-delivery-policy"></a>DynamicCommonEncryption tillgångsleveransprincip

hello följande **CreateAssetDeliveryPolicy** metoden skapar hello **AssetDeliveryPolicy** som är konfigurerade tooapply dynamisk vanliga kryptering (**DynamicCommonEncryption**) tooa smooth streaming protocol (andra protokoll kommer att blockeras från strömning). hello-metoden har två parametrar: **tillgången** (hello tillgången toowhich som du vill tooapply hello leveransprincipen) och **IContentKey** (hello innehållsnyckeln av hello **CommonEncryption**typ för mer information, se: [att skapa en innehållsnyckel](media-services-dotnet-create-contentkey.md#common_contentkey)).

Information om vilka värden som du anger när du skapar en AssetDeliveryPolicy finns hello [typer som används när du definierar AssetDeliveryPolicy](#types) avsnitt.

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

Azure Media Services kan du också tooadd Widevine kryptering. hello visar exemplet nedan både PlayReady och Widevine som läggs till toohello tillgångsleveransprincip.

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
> Vid kryptering med Widevine, skulle du bara att kunna toodeliver med bindestreck. Kontrollera att toospecify STRECK i hello protokollet för tillgångsleverans.
> 
> 

## <a name="dynamicenvelopeencryption-asset-delivery-policy"></a>DynamicEnvelopeEncryption tillgångsleveransprincip
hello följande **CreateAssetDeliveryPolicy** metoden skapar hello **AssetDeliveryPolicy** som är konfigurerade tooapply dynamiska kryptering (**DynamicEnvelopeEncryption** ) tooSmooth Streaming, HLS och DASH-protokoll (om du väljer toonot ange vissa protokoll, kommer att blockeras från strömning). hello-metoden har två parametrar: **tillgången** (hello tillgången toowhich som du vill tooapply hello leveransprincipen) och **IContentKey** (hello innehållsnyckeln av hello **EnvelopeEncryption**typ för mer information, se: [att skapa en innehållsnyckel](media-services-dotnet-create-contentkey.md#envelope_contentkey)).

Information om vilka värden som du anger när du skapar en AssetDeliveryPolicy finns hello [typer som används när du definierar AssetDeliveryPolicy](#types) avsnitt.   

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


## <a id="types"></a>Typer som används när du definierar AssetDeliveryPolicy

### <a id="AssetDeliveryProtocol"></a>AssetDeliveryProtocol

hello beskrivs följande enum värden som du kan ange för hello protokollet för tillgångsleverans.

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

### <a id="AssetDeliveryPolicyType"></a>AssetDeliveryPolicyType

hello beskriver följande enum värden som du kan ange för hello tillgångstyp leverans princip.  

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

### <a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType

hello beskriver följande enum värden som du kan använda tooconfigure hello leveransmetod av hello innehåll viktiga toohello-klienten.
    
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

### <a id="AssetDeliveryPolicyConfigurationKey"></a>AssetDeliveryPolicyConfigurationKey

hello följande enum beskrivs värden som du kan ange tooconfigure nycklar som används tooget specifik konfiguration för en tillgångsleveransprincip.

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

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

