---
title: "aaaConfigure innehållsnyckelns auktoriseringsprincip med Media Services .NET SDK | Microsoft Docs"
description: "Lär dig hur tooconfigure en auktoriseringsprincip för en innehållsnyckel med Media Services .NET SDK."
services: media-services
documentationcenter: 
author: Mingfeiy
manager: cfowler
editor: 
ms.assetid: 1a0aedda-5b87-4436-8193-09fc2f14310c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako;mingfeiy
ms.openlocfilehash: cfcbc5da9819bcec8b163fef183988a8beff9ed2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="dynamic-encryption-configure-content-key-authorization-policy"></a><span data-ttu-id="fb364-103">Dynamisk kryptering: Konfigurera innehållsnyckelns auktoriseringsprincip</span><span class="sxs-lookup"><span data-stu-id="fb364-103">Dynamic encryption: configure content key authorization policy</span></span>
[!INCLUDE [media-services-selector-content-key-auth-policy](../../includes/media-services-selector-content-key-auth-policy.md)]

## <a name="overview"></a><span data-ttu-id="fb364-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="fb364-104">Overview</span></span>
<span data-ttu-id="fb364-105">Microsoft Azure Media Services kan du toodeliver MPEG DASH, Smooth Streaming och HTTP Live Streaming (HLS)-dataströmmar som skyddas med Standard AES (Advanced Encryption) (med 128-bitars krypteringsnycklar) eller [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/).</span><span class="sxs-lookup"><span data-stu-id="fb364-105">Microsoft Azure Media Services enables you toodeliver MPEG-DASH, Smooth Streaming, and HTTP-Live-Streaming (HLS) streams protected with Advanced Encryption Standard (AES) (using 128-bit encryption keys) or [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/).</span></span> <span data-ttu-id="fb364-106">AMS gör också att du toodeliver DASH-dataströmmar krypterat med Widevine DRM.</span><span class="sxs-lookup"><span data-stu-id="fb364-106">AMS also enables you toodeliver DASH streams encrypted with Widevine DRM.</span></span> <span data-ttu-id="fb364-107">Både PlayReady och Widevine krypteras enligt hello Common Encryption (ISO/IEC 23001 7 CENC)-specifikationen.</span><span class="sxs-lookup"><span data-stu-id="fb364-107">Both PlayReady and Widevine are encrypted per hello Common Encryption (ISO/IEC 23001-7 CENC) specification.</span></span>

<span data-ttu-id="fb364-108">Media Services tillhandahåller också en **nyckel-/ Licensleveranstjänst** från vilka klienterna kan hämta AES-nycklar eller PlayReady/Widevine-licenser tooplay hello krypterat innehåll.</span><span class="sxs-lookup"><span data-stu-id="fb364-108">Media Services also provides a **Key/License Delivery Service** from which clients can obtain AES keys or PlayReady/Widevine licenses tooplay hello encrypted content.</span></span>

<span data-ttu-id="fb364-109">Om du vill använda för Media Services tooencrypt en tillgång, behöver du tooassociate en krypteringsnyckel (**CommonEncryption** eller **EnvelopeEncryption**) med hello tillgången (enligt [här](media-services-dotnet-create-contentkey.md)) och även konfigurera auktoriseringsprinciper för hello nyckel (som beskrivs i den här artikeln).</span><span class="sxs-lookup"><span data-stu-id="fb364-109">If you want for Media Services tooencrypt an asset, you need tooassociate an encryption key (**CommonEncryption** or **EnvelopeEncryption**) with hello asset (as described [here](media-services-dotnet-create-contentkey.md)) and also configure authorization policies for hello key (as described in this article).</span></span>

<span data-ttu-id="fb364-110">När en dataströmmen har begärts av en spelare, använder Media Services hello angetts viktiga toodynamically kryptera ditt innehåll med hjälp av AES eller DRM-kryptering.</span><span class="sxs-lookup"><span data-stu-id="fb364-110">When a stream is requested by a player, Media Services uses hello specified key toodynamically encrypt your content using AES or DRM encryption.</span></span> <span data-ttu-id="fb364-111">toodecrypt hello dataströmmen hello player begär hello nyckeln från hello viktiga leverans av tjänsten.</span><span class="sxs-lookup"><span data-stu-id="fb364-111">toodecrypt hello stream, hello player will request hello key from hello key delivery service.</span></span> <span data-ttu-id="fb364-112">toodecide om huruvida användaren hello är godkänd tooget hello nyckel, hello tjänsten utvärderar hello auktoriseringsprinciper som du angav för hello nyckeln.</span><span class="sxs-lookup"><span data-stu-id="fb364-112">toodecide whether or not hello user is authorized tooget hello key, hello service evaluates hello authorization policies that you specified for hello key.</span></span>

<span data-ttu-id="fb364-113">Media Services stöder flera olika sätt att auktorisera användare som begär nycklar.</span><span class="sxs-lookup"><span data-stu-id="fb364-113">Media Services supports multiple ways of authenticating users who make key requests.</span></span> <span data-ttu-id="fb364-114">Hej innehållsnyckelns auktoriseringsprincip kan ha en eller flera auktoriseringsbegränsningar: **öppna** eller **token** begränsning.</span><span class="sxs-lookup"><span data-stu-id="fb364-114">hello content key authorization policy could have one or more authorization restrictions: **open** or **token** restriction.</span></span> <span data-ttu-id="fb364-115">Hej tokenbegränsade principen måste åtföljas av en token som utfärdas av en säker säkerhetstokentjänst (STS).</span><span class="sxs-lookup"><span data-stu-id="fb364-115">hello token restricted policy must be accompanied by a token issued by a Secure Token Service (STS).</span></span> <span data-ttu-id="fb364-116">Media Services stöder token i hello **Simple Web Tokens** ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) format och **JSON Web Token** ([JWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3)) format.</span><span class="sxs-lookup"><span data-stu-id="fb364-116">Media Services supports tokens in hello **Simple Web Tokens** ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) format and **JSON Web Token** ([JWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3)) format.</span></span>

<span data-ttu-id="fb364-117">Media Services tillhandahåller inte Secure Token tjänster.</span><span class="sxs-lookup"><span data-stu-id="fb364-117">Media Services does not provide Secure Token Services.</span></span> <span data-ttu-id="fb364-118">Du kan skapa en anpassad STS eller använda Microsoft Azure ACS tooissue token.</span><span class="sxs-lookup"><span data-stu-id="fb364-118">You can create a custom STS or leverage Microsoft Azure ACS tooissue tokens.</span></span> <span data-ttu-id="fb364-119">hello STS måste vara konfigurerade toocreate en token som signerats med hello angiven nyckel och utfärda anspråk som du angav i hello tokenbegränsningar konfiguration (som beskrivs i den här artikeln).</span><span class="sxs-lookup"><span data-stu-id="fb364-119">hello STS must be configured toocreate a token signed with hello specified key and issue claims that you specified in hello token restriction configuration (as described in this article).</span></span> <span data-ttu-id="fb364-120">hello returnerar Media Services-tjänsten för leverans av nyckeln hello encryption key toohello klienten om hello token är giltig och hello anspråk i hello token som matchar de som konfigurerats för hello innehållsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="fb364-120">hello Media Services key delivery service will return hello encryption key toohello client if hello token is valid and hello claims in hello token match those configured for hello content key.</span></span>

<span data-ttu-id="fb364-121">Mer information finns i</span><span class="sxs-lookup"><span data-stu-id="fb364-121">For more information, see</span></span>

[<span data-ttu-id="fb364-122">JWT-token autentisering</span><span class="sxs-lookup"><span data-stu-id="fb364-122">JWT token authentication</span></span>](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)

<span data-ttu-id="fb364-123">[Integrera Azure Media Services OWIN MVC baserat app med Azure Active Directory och begränsa viktiga innehållsleverans baserat på JWT anspråk](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).</span><span class="sxs-lookup"><span data-stu-id="fb364-123">[Integrate Azure Media Services OWIN MVC based app with Azure Active Directory and restrict content key delivery based on JWT claims](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).</span></span>

<span data-ttu-id="fb364-124">[Använd Azure ACS tooissue token](http://mingfeiy.com/acs-with-key-services).</span><span class="sxs-lookup"><span data-stu-id="fb364-124">[Use Azure ACS tooissue tokens](http://mingfeiy.com/acs-with-key-services).</span></span>

### <a name="some-considerations-apply"></a><span data-ttu-id="fb364-125">Vissa förutsättningar gäller:</span><span class="sxs-lookup"><span data-stu-id="fb364-125">Some considerations apply:</span></span>
* <span data-ttu-id="fb364-126">När AMS-kontot skapas en **standard** strömningsslutpunkt har lagts till tooyour konto i hello **stoppad** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="fb364-126">When your AMS account is created a **default** streaming endpoint is added  tooyour account in hello **Stopped** state.</span></span> <span data-ttu-id="fb364-127">toostart strömning ditt innehåll och dra nytta av dynamisk paketering och dynamisk kryptering, strömmande slutpunkten har toobe i hello **kör** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="fb364-127">toostart streaming your content and take advantage of dynamic packaging and dynamic encryption, your streaming endpoint has toobe in hello **Running** state.</span></span> 
* <span data-ttu-id="fb364-128">Din tillgång måste innehålla en uppsättning MP4s med anpassningsbar bithastighet eller Smooth Streaming-filer.</span><span class="sxs-lookup"><span data-stu-id="fb364-128">Your asset must contain a set of adaptive bitrate MP4s or  adaptive bitrate Smooth Streaming files.</span></span> <span data-ttu-id="fb364-129">Mer information finns i [koda en tillgång](media-services-encode-asset.md).</span><span class="sxs-lookup"><span data-stu-id="fb364-129">For more information, see [Encode an asset](media-services-encode-asset.md).</span></span>
* <span data-ttu-id="fb364-130">Ladda upp och koda dina tillgångar med **AssetCreationOptions.StorageEncrypted** alternativet.</span><span class="sxs-lookup"><span data-stu-id="fb364-130">Upload and encode your assets using **AssetCreationOptions.StorageEncrypted** option.</span></span>
* <span data-ttu-id="fb364-131">Om du planerar toohave nycklar för multiinnehåll som kräver hello samma principkonfigurationer, det är starkt rekommenderat toocreate en enda auktoriseringsprincip och återanvända med nycklar för multiinnehåll.</span><span class="sxs-lookup"><span data-stu-id="fb364-131">If you plan toohave multiple content keys that require hello same policy configuration, it is strongly recommended toocreate a single authorization policy and reuse it with multiple content keys.</span></span>
* <span data-ttu-id="fb364-132">hello nyckeln Delivery service cachelagrar ContentKeyAuthorizationPolicy och dess relaterade objekt (alternativ och begränsningar) i 15 minuter.</span><span class="sxs-lookup"><span data-stu-id="fb364-132">hello Key Delivery service caches ContentKeyAuthorizationPolicy and its related objects (policy options and restrictions) for 15 minutes.</span></span>  <span data-ttu-id="fb364-133">Om du skapar en ContentKeyAuthorizationPolicy och ange toouse ”Token” begränsning, och sedan testa den och sedan uppdatera hello-principen för ”öppna” begränsning, det tar ungefär 15 minuter innan hello princip växlar toohello ”öppen” version av hello-principen.</span><span class="sxs-lookup"><span data-stu-id="fb364-133">If you create a ContentKeyAuthorizationPolicy and specify toouse a “Token” restriction, then test it, and then update hello policy too“Open” restriction, it will take roughly 15 minutes before hello policy switches toohello “Open” version of hello policy.</span></span>
* <span data-ttu-id="fb364-134">Om du lägger till eller uppdaterar din tillgångs leveransprincip måste du ta bort en befintlig lokaliserare (om sådan finns) och skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="fb364-134">If you add or update your asset’s delivery policy, you must delete an existing locator (if any) and create a new locator.</span></span>
* <span data-ttu-id="fb364-135">Det går för närvarande kryptera progressiv hämtning.</span><span class="sxs-lookup"><span data-stu-id="fb364-135">Currently, you cannot encrypt progressive downloads.</span></span>

## <a name="aes-128-dynamic-encryption"></a><span data-ttu-id="fb364-136">AES-128 dynamisk kryptering</span><span class="sxs-lookup"><span data-stu-id="fb364-136">AES-128 Dynamic Encryption</span></span>
### <a name="open-restriction"></a><span data-ttu-id="fb364-137">Öppna begränsning</span><span class="sxs-lookup"><span data-stu-id="fb364-137">Open Restriction</span></span>
<span data-ttu-id="fb364-138">Öppna begränsning innebär hello system levererar hello viktiga tooanyone som begär nycklar.</span><span class="sxs-lookup"><span data-stu-id="fb364-138">Open restriction means hello system will deliver hello key tooanyone who makes a key request.</span></span> <span data-ttu-id="fb364-139">Den här begränsningen kan vara användbart för testning.</span><span class="sxs-lookup"><span data-stu-id="fb364-139">This restriction might be useful for testing purposes.</span></span>

<span data-ttu-id="fb364-140">hello följande exempel skapar en öppen auktoriseringsprincip och lägger till den toohello innehållsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="fb364-140">hello following example creates an open authorization policy and adds it toohello content key.</span></span>

    static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
    {
        // Create ContentKeyAuthorizationPolicy with Open restrictions
        // and create authorization policy
        IContentKeyAuthorizationPolicy policy = _context.
        ContentKeyAuthorizationPolicies.
        CreateAsync("Open Authorization Policy").Result;
        
        List<ContentKeyAuthorizationPolicyRestriction> restrictions =
            new List<ContentKeyAuthorizationPolicyRestriction>();

        ContentKeyAuthorizationPolicyRestriction restriction =
            new ContentKeyAuthorizationPolicyRestriction
            {
                Name = "HLS Open Authorization Policy",
                KeyRestrictionType = (int)ContentKeyRestrictionType.Open,
                Requirements = null // no requirements needed for HLS
            };

        restrictions.Add(restriction);

        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create(
            "policy", 
            ContentKeyDeliveryType.BaselineHttp, 
            restrictions, 
            "");

        policy.Options.Add(policyOption);

        // Add ContentKeyAutorizationPolicy tooContentKey
        contentKey.AuthorizationPolicyId = policy.Id;
        IContentKey updatedKey = contentKey.UpdateAsync().Result;
        Console.WriteLine("Adding Key tooAsset: Key ID is " + updatedKey.Id);
    }


### <a name="token-restriction"></a><span data-ttu-id="fb364-141">Tokenbegränsningar</span><span class="sxs-lookup"><span data-stu-id="fb364-141">Token Restriction</span></span>
<span data-ttu-id="fb364-142">Det här avsnittet beskrivs hur toocreate en innehåll nyckeln auktoriseringsprincip och koppla den till hello innehållsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="fb364-142">This section describes how toocreate a content key authorization policy and associate it with hello content key.</span></span> <span data-ttu-id="fb364-143">hello auktoriseringsprincip beskriver vilka behörighetskraven måste vara uppfyllda toodetermine hello användare som är auktoriserade tooreceive hello nyckeln (till exempel innehåller hello ”Verifieringsnyckeln” listan hello nyckel hello säkerhetstoken som har signerats med).</span><span class="sxs-lookup"><span data-stu-id="fb364-143">hello authorization policy describes what authorization requirements must be met toodetermine if hello user is authorized tooreceive hello key (for example, does hello “verification key” list contain hello key that hello token was signed with).</span></span>

<span data-ttu-id="fb364-144">tooconfigure hello tokenbegränsningar alternativet behöver du toouse XML behörighetskraven toodescribe hello token.</span><span class="sxs-lookup"><span data-stu-id="fb364-144">tooconfigure hello token restriction option, you need toouse an XML toodescribe hello token’s authorization requirements.</span></span> <span data-ttu-id="fb364-145">Hej tokenbegränsningar konfigurations-XML måste följa toohello följande XML-schema.</span><span class="sxs-lookup"><span data-stu-id="fb364-145">hello token restriction configuration XML must conform toohello following XML schema.</span></span>

#### <span data-ttu-id="fb364-146"><a id="schema"></a>Tokenbegränsningar schema</span><span class="sxs-lookup"><span data-stu-id="fb364-146"><a id="schema"></a>Token restriction schema</span></span>
    <?xml version="1.0" encoding="utf-8"?>
    <xs:schema xmlns:tns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1" elementFormDefault="qualified" targetNamespace="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1" xmlns:xs="http://www.w3.org/2001/XMLSchema">
      <xs:complexType name="TokenClaim">
        <xs:sequence>
          <xs:element name="ClaimType" nillable="true" type="xs:string" />
          <xs:element minOccurs="0" name="ClaimValue" nillable="true" type="xs:string" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="TokenClaim" nillable="true" type="tns:TokenClaim" />
      <xs:complexType name="TokenRestrictionTemplate">
        <xs:sequence>
          <xs:element minOccurs="0" name="AlternateVerificationKeys" nillable="true" type="tns:ArrayOfTokenVerificationKey" />
          <xs:element name="Audience" nillable="true" type="xs:anyURI" />
          <xs:element name="Issuer" nillable="true" type="xs:anyURI" />
          <xs:element name="PrimaryVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
          <xs:element minOccurs="0" name="RequiredClaims" nillable="true" type="tns:ArrayOfTokenClaim" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="TokenRestrictionTemplate" nillable="true" type="tns:TokenRestrictionTemplate" />
      <xs:complexType name="ArrayOfTokenVerificationKey">
        <xs:sequence>
          <xs:element minOccurs="0" maxOccurs="unbounded" name="TokenVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ArrayOfTokenVerificationKey" nillable="true" type="tns:ArrayOfTokenVerificationKey" />
      <xs:complexType name="TokenVerificationKey">
        <xs:sequence />
      </xs:complexType>
      <xs:element name="TokenVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
      <xs:complexType name="ArrayOfTokenClaim">
        <xs:sequence>
          <xs:element minOccurs="0" maxOccurs="unbounded" name="TokenClaim" nillable="true" type="tns:TokenClaim" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ArrayOfTokenClaim" nillable="true" type="tns:ArrayOfTokenClaim" />
      <xs:complexType name="SymmetricVerificationKey">
        <xs:complexContent mixed="false">
          <xs:extension base="tns:TokenVerificationKey">
            <xs:sequence>
              <xs:element name="KeyValue" nillable="true" type="xs:base64Binary" />
            </xs:sequence>
          </xs:extension>
        </xs:complexContent>
      </xs:complexType>
      <xs:element name="SymmetricVerificationKey" nillable="true" type="tns:SymmetricVerificationKey" />
    </xs:schema>

<span data-ttu-id="fb364-147">När du konfigurerar hello **token** begränsad princip, måste du ange hello primära ** verifiering nyckeln **, **utfärdaren** och **målgruppen** parametrar.</span><span class="sxs-lookup"><span data-stu-id="fb364-147">When configuring hello **token** restricted policy, you must specify hello primary** verification key**, **issuer** and **audience** parameters.</span></span> <span data-ttu-id="fb364-148">hello ** primära Verifieringsnyckeln ** innehåller hello-nyckel som hello token har signerats med, **utfärdaren** är hello säker tokentjänst problem hello säkerhetstoken.</span><span class="sxs-lookup"><span data-stu-id="fb364-148">hello **primary verification key **contains hello key that hello token was signed with, **issuer** is hello secure token service that issues hello token.</span></span> <span data-ttu-id="fb364-149">Hej **målgruppen** (kallas ibland **omfång**) beskriver hello avsikt hello-token eller hello resursen hello token auktoriserar åtkomst till.</span><span class="sxs-lookup"><span data-stu-id="fb364-149">hello **audience** (sometimes called **scope**) describes hello intent of hello token or hello resource hello token authorizes access to.</span></span> <span data-ttu-id="fb364-150">hello Media Services viktiga delivery service verifierar att dessa värden i hello token matchar hello värden i hello mallen.</span><span class="sxs-lookup"><span data-stu-id="fb364-150">hello Media Services key delivery service validates that these values in hello token match hello values in hello template.</span></span> 

<span data-ttu-id="fb364-151">När du använder **Media Services SDK för .NET**, du kan använda hello **TokenRestrictionTemplate** toogenerate hello begränsning klasstoken.</span><span class="sxs-lookup"><span data-stu-id="fb364-151">When using **Media Services SDK for .NET**, you can use hello **TokenRestrictionTemplate** class toogenerate hello restriction token.</span></span>
<span data-ttu-id="fb364-152">hello följande exempel skapar en auktoriseringsprincip med en token begränsning.</span><span class="sxs-lookup"><span data-stu-id="fb364-152">hello following example creates an authorization policy with a token restriction.</span></span> <span data-ttu-id="fb364-153">I det här exemplet hello klienten skulle ha toopresent en token som innehåller: signering nyckel (VerificationKey), en token utfärdare och nödvändiga anspråk.</span><span class="sxs-lookup"><span data-stu-id="fb364-153">In this example, hello client would have toopresent a token that contains: signing key (VerificationKey), a token issuer, and required claims.</span></span>

    public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
    {
        string tokenTemplateString = GenerateTokenRequirements();

        IContentKeyAuthorizationPolicy policy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("HLS token restricted authorization policy").Result;

        List<ContentKeyAuthorizationPolicyRestriction> restrictions =
                new List<ContentKeyAuthorizationPolicyRestriction>();

        ContentKeyAuthorizationPolicyRestriction restriction =
                new ContentKeyAuthorizationPolicyRestriction
                {
                    Name = "Token Authorization Policy",
                    KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                    Requirements = tokenTemplateString
                };

        restrictions.Add(restriction);

        //You could have multiple options 
        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create(
                "Token option for HLS",
                ContentKeyDeliveryType.BaselineHttp,
                restrictions,
                null  // no key delivery data is needed for HLS
                );

        policy.Options.Add(policyOption);

        // Add ContentKeyAutorizationPolicy tooContentKey
        contentKey.AuthorizationPolicyId = policy.Id;
        IContentKey updatedKey = contentKey.UpdateAsync().Result;
        Console.WriteLine("Adding Key tooAsset: Key ID is " + updatedKey.Id);

        return tokenTemplateString;
    }

    static private string GenerateTokenRequirements()
    {
        TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);

        template.PrimaryVerificationKey = new SymmetricVerificationKey();
        template.AlternateVerificationKeys.Add(new SymmetricVerificationKey());
            template.Audience = _sampleAudience.ToString();
            template.Issuer = _sampleIssuer.ToString();

        template.RequiredClaims.Add(TokenClaim.ContentKeyIdentifierClaim);

        return TokenRestrictionTemplateSerializer.Serialize(template);
    }

#### <span data-ttu-id="fb364-154"><a id="test"></a>Test-token</span><span class="sxs-lookup"><span data-stu-id="fb364-154"><a id="test"></a>Test token</span></span>
<span data-ttu-id="fb364-155">tooget en test-token baserat på hello tokenbegränsningar som användes för nyckelauktoriseringsprincipen för hello, hello följande.</span><span class="sxs-lookup"><span data-stu-id="fb364-155">tooget a test token based on hello token restriction that was used for hello key authorization policy, do hello following.</span></span>

    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate =
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);

    // Generate a test token based on hello hello data in hello given TokenRestrictionTemplate.
    // Note, you need toopass hello key id Guid because we specified 
    // TokenClaim.ContentKeyIdentifierClaim in during hello creation of TokenRestrictionTemplate.
    Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);

    //hello GenerateTestToken method returns hello token without hello word “Bearer” in front
    //so you have tooadd it in front of hello token string. 
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey);
    Console.WriteLine("hello authorization token is:\nBearer {0}", testToken);
    Console.WriteLine();


## <a name="playready-dynamic-encryption"></a><span data-ttu-id="fb364-156">PlayReady-dynamisk kryptering</span><span class="sxs-lookup"><span data-stu-id="fb364-156">PlayReady Dynamic Encryption</span></span>
<span data-ttu-id="fb364-157">Media Services kan du tooconfigure hello behörigheter och begränsningar som du vill för hello PlayReady DRM runtime tooenforce när en användare försöker tooplay tillbaka skyddat innehåll.</span><span class="sxs-lookup"><span data-stu-id="fb364-157">Media Services enables you tooconfigure hello rights and restrictions that you want for hello PlayReady DRM runtime tooenforce when a user is trying tooplay back protected content.</span></span> 

<span data-ttu-id="fb364-158">När du skyddar ditt innehåll med PlayReady hello saker du behöver toospecify i principen för auktorisering är en XML-sträng som definierar hello [PlayReady-licensmall](media-services-playready-license-template-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fb364-158">When protecting your content with PlayReady, one of hello things you need toospecify in your authorization policy is an XML string that defines hello [PlayReady license template](media-services-playready-license-template-overview.md).</span></span> <span data-ttu-id="fb364-159">I Media Services SDK för .NET hello **PlayReadyLicenseResponseTemplate** och **PlayReadyLicenseTemplate** klasser hjälper dig att definiera hello PlayReady License-mall.</span><span class="sxs-lookup"><span data-stu-id="fb364-159">In Media Services SDK for .NET, hello **PlayReadyLicenseResponseTemplate** and **PlayReadyLicenseTemplate** classes will help you define hello PlayReady License Template.</span></span>

<span data-ttu-id="fb364-160">[Det här avsnittet](media-services-protect-with-drm.md) visar hur tooencrypt ditt innehåll med **PlayReady** och **Widevine**.</span><span class="sxs-lookup"><span data-stu-id="fb364-160">[This topic](media-services-protect-with-drm.md) shows how tooencrypt your content with **PlayReady** and **Widevine**.</span></span>

### <a name="open-restriction"></a><span data-ttu-id="fb364-161">Öppna begränsning</span><span class="sxs-lookup"><span data-stu-id="fb364-161">Open Restriction</span></span>
<span data-ttu-id="fb364-162">Öppna begränsning innebär hello system levererar hello viktiga tooanyone som begär nycklar.</span><span class="sxs-lookup"><span data-stu-id="fb364-162">Open restriction means hello system will deliver hello key tooanyone who makes a key request.</span></span> <span data-ttu-id="fb364-163">Den här begränsningen kan vara användbart för testning.</span><span class="sxs-lookup"><span data-stu-id="fb364-163">This restriction might be useful for testing purposes.</span></span>

<span data-ttu-id="fb364-164">hello följande exempel skapar en öppen auktoriseringsprincip och lägger till den toohello innehållsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="fb364-164">hello following example creates an open authorization policy and adds it toohello content key.</span></span>

    static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
    {

        // Create ContentKeyAuthorizationPolicy with Open restrictions 
        // and create authorization policy          

        List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
        {
            new ContentKeyAuthorizationPolicyRestriction 
            { 
                Name = "Open", 
                KeyRestrictionType = (int)ContentKeyRestrictionType.Open, 
                Requirements = null
            }
        };

        // Configure PlayReady license template.
        string newLicenseTemplate = ConfigurePlayReadyLicenseTemplate();

        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create("",
                ContentKeyDeliveryType.PlayReadyLicense,
                    restrictions, newLicenseTemplate);

        IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                    ContentKeyAuthorizationPolicies.
                    CreateAsync("Deliver Common Content Key with no restrictions").
                    Result;


        contentKeyAuthorizationPolicy.Options.Add(policyOption);

        // Associate hello content key authorization policy with hello content key.
        contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
        contentKey = contentKey.UpdateAsync().Result;
    }

### <a name="token-restriction"></a><span data-ttu-id="fb364-165">Tokenbegränsningar</span><span class="sxs-lookup"><span data-stu-id="fb364-165">Token Restriction</span></span>
<span data-ttu-id="fb364-166">tooconfigure hello tokenbegränsningar alternativet behöver du toouse XML behörighetskraven toodescribe hello token.</span><span class="sxs-lookup"><span data-stu-id="fb364-166">tooconfigure hello token restriction option, you need toouse an XML toodescribe hello token’s authorization requirements.</span></span> <span data-ttu-id="fb364-167">Hej tokenbegränsningar konfigurations-XML måste uppfylla toohello XML-schemat visas i [detta](#schema) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="fb364-167">hello token restriction configuration XML must conform toohello XML schema shown in [this](#schema) section.</span></span>

    public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
    {
        string tokenTemplateString = GenerateTokenRequirements();

        IContentKeyAuthorizationPolicy policy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("HLS token restricted authorization policy").Result;

        List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
        {
            new ContentKeyAuthorizationPolicyRestriction 
            { 
                Name = "Token Authorization Policy", 
                KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                Requirements = tokenTemplateString, 
            }
        };

        // Configure PlayReady license template.
        string newLicenseTemplate = ConfigurePlayReadyLicenseTemplate();

        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                ContentKeyDeliveryType.PlayReadyLicense,
                    restrictions, newLicenseTemplate);

        IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                    ContentKeyAuthorizationPolicies.
                    CreateAsync("Deliver Common Content Key with no restrictions").
                    Result;

        policy.Options.Add(policyOption);

        // Add ContentKeyAutorizationPolicy tooContentKey
        contentKeyAuthorizationPolicy.Options.Add(policyOption);

        // Associate hello content key authorization policy with hello content key
        contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
        contentKey = contentKey.UpdateAsync().Result;

        return tokenTemplateString;
    }

    static private string GenerateTokenRequirements()
    {

        TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);

        template.PrimaryVerificationKey = new SymmetricVerificationKey();
        template.AlternateVerificationKeys.Add(new SymmetricVerificationKey());
            template.Audience = _sampleAudience.ToString();
            template.Issuer = _sampleIssuer.ToString();


        template.RequiredClaims.Add(TokenClaim.ContentKeyIdentifierClaim);

        return TokenRestrictionTemplateSerializer.Serialize(template);
    } 

    static private string ConfigurePlayReadyLicenseTemplate()
    {
        // hello following code configures PlayReady License Template using .NET classes
        // and returns hello XML string.

        //hello PlayReadyLicenseResponseTemplate class represents hello template for hello response sent back toohello end user. 
        //It contains a field for a custom data string between hello license server and hello application 
        //(may be useful for custom app logic) as well as a list of one or more license templates.
        PlayReadyLicenseResponseTemplate responseTemplate = new PlayReadyLicenseResponseTemplate();

        // hello PlayReadyLicenseTemplate class represents a license template for creating PlayReady licenses
        // toobe returned toohello end users. 
        //It contains hello data on hello content key in hello license and any rights or restrictions toobe 
        //enforced by hello PlayReady DRM runtime when using hello content key.
        PlayReadyLicenseTemplate licenseTemplate = new PlayReadyLicenseTemplate();
        //Configure whether hello license is persistent (saved in persistent storage on hello client) 
        //or non-persistent (only held in memory while hello player is using hello license).  
        licenseTemplate.LicenseType = PlayReadyLicenseType.Nonpersistent;

        // AllowTestDevices controls whether test devices can use hello license or not.  
        // If true, hello MinimumSecurityLevel property of hello license
        // is set too150.  If false (hello default), hello MinimumSecurityLevel property of hello license is set too2000.
        licenseTemplate.AllowTestDevices = true;


        // You can also configure hello Play Right in hello PlayReady license by using hello PlayReadyPlayRight class. 
        // It grants hello user hello ability tooplayback hello content subject toohello zero or more restrictions 
        // configured in hello license and on hello PlayRight itself (for playback specific policy). 
        // Much of hello policy on hello PlayRight has toodo with output restrictions 
        // which control hello types of outputs that hello content can be played over and 
        // any restrictions that must be put in place when using a given output.
        // For example, if hello DigitalVideoOnlyContentRestriction is enabled, 
        //then hello DRM runtime will only allow hello video toobe displayed over digital outputs 
        //(analog video outputs won’t be allowed toopass hello content).

        //IMPORTANT: These types of restrictions can be very powerful but can also affect hello consumer experience. 
        // If hello output protections are configured too restrictive, 
        // hello content might be unplayable on some clients. For more information, see hello PlayReady Compliance Rules document.

        // For example:
        //licenseTemplate.PlayRight.AgcAndColorStripeRestriction = new AgcAndColorStripeRestriction(1);

        responseTemplate.LicenseTemplates.Add(licenseTemplate);

        return MediaServicesLicenseTemplateSerializer.Serialize(responseTemplate);
    }


<span data-ttu-id="fb364-168">tooget en test-token baserat på hello tokenbegränsningar som användes för hello auktorisering av innehållsnyckel princip Se [detta](#test) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="fb364-168">tooget a test token based on hello token restriction that was used for hello key authorization policy see [this](#test) section.</span></span> 

## <span data-ttu-id="fb364-169"><a id="types"></a>Typer som används när du definierar ContentKeyAuthorizationPolicy</span><span class="sxs-lookup"><span data-stu-id="fb364-169"><a id="types"></a>Types used when defining ContentKeyAuthorizationPolicy</span></span>
### <span data-ttu-id="fb364-170"><a id="ContentKeyRestrictionType"></a>ContentKeyRestrictionType</span><span class="sxs-lookup"><span data-stu-id="fb364-170"><a id="ContentKeyRestrictionType"></a>ContentKeyRestrictionType</span></span>
    public enum ContentKeyRestrictionType
    {
        Open = 0,
        TokenRestricted = 1,
        IPRestricted = 2,
    }

### <span data-ttu-id="fb364-171"><a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType</span><span class="sxs-lookup"><span data-stu-id="fb364-171"><a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType</span></span>
    public enum ContentKeyDeliveryType
    {
      None = 0,
      PlayReadyLicense = 1,
      BaselineHttp = 2,
      Widevine = 3
    }

### <span data-ttu-id="fb364-172"><a id="TokenType"></a>TokenType</span><span class="sxs-lookup"><span data-stu-id="fb364-172"><a id="TokenType"></a>TokenType</span></span>
    public enum TokenType
    {
        Undefined = 0,
        SWT = 1,
        JWT = 2,
    }



## <a name="media-services-learning-paths"></a><span data-ttu-id="fb364-173">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="fb364-173">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="fb364-174">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="fb364-174">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-step"></a><span data-ttu-id="fb364-175">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fb364-175">Next step</span></span>
<span data-ttu-id="fb364-176">Nu när du har konfigurerat innehållsnyckelns auktoriseringsprincip gå toohello [hur tooconfigure tillgångsleveransprincip](media-services-dotnet-configure-asset-delivery-policy.md) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="fb364-176">Now that you have configured content key's authorization policy, go toohello [How tooconfigure asset delivery policy](media-services-dotnet-configure-asset-delivery-policy.md) topic.</span></span>

