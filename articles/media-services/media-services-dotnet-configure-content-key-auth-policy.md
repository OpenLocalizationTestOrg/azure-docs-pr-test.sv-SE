---
title: "Konfigurera innehållsnyckelns auktoriseringsprincip med Media Services .NET SDK | Microsoft Docs"
description: "Lär dig hur du konfigurerar en auktoriseringsprincip för en innehållsnyckel med Media Services .NET SDK."
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
ms.openlocfilehash: 75dd9107dca215a0b31db3d44bada69210fe9ac6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="dynamic-encryption-configure-content-key-authorization-policy"></a><span data-ttu-id="b6125-103">Dynamisk kryptering: Konfigurera innehållsnyckelns auktoriseringsprincip</span><span class="sxs-lookup"><span data-stu-id="b6125-103">Dynamic encryption: configure content key authorization policy</span></span>
[!INCLUDE [media-services-selector-content-key-auth-policy](../../includes/media-services-selector-content-key-auth-policy.md)]

## <a name="overview"></a><span data-ttu-id="b6125-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="b6125-104">Overview</span></span>
<span data-ttu-id="b6125-105">Microsoft Azure Media Services kan du leverera MPEG DASH, Smooth Streaming och HTTP Live Streaming (HLS) dataströmmar som skyddas med Standard AES (Advanced Encryption) (med 128-bitars krypteringsnycklar) eller [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/).</span><span class="sxs-lookup"><span data-stu-id="b6125-105">Microsoft Azure Media Services enables you to deliver MPEG-DASH, Smooth Streaming, and HTTP-Live-Streaming (HLS) streams protected with Advanced Encryption Standard (AES) (using 128-bit encryption keys) or [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/).</span></span> <span data-ttu-id="b6125-106">AMS kan du leverera DASH krypterat med Widevine DRM-dataströmmar.</span><span class="sxs-lookup"><span data-stu-id="b6125-106">AMS also enables you to deliver DASH streams encrypted with Widevine DRM.</span></span> <span data-ttu-id="b6125-107">Både PlayReady och Widevine krypteras enligt den gemensamma  krypteringsspecifikationen (ISO/IEC 23001 7 CENC).</span><span class="sxs-lookup"><span data-stu-id="b6125-107">Both PlayReady and Widevine are encrypted per the Common Encryption (ISO/IEC 23001-7 CENC) specification.</span></span>

<span data-ttu-id="b6125-108">Media Services tillhandahåller också en **nyckel-/ Licensleveranstjänst** från vilka klienterna kan hämta AES-nycklar eller PlayReady/Widevine-licenser för att spela upp det krypterade innehållet.</span><span class="sxs-lookup"><span data-stu-id="b6125-108">Media Services also provides a **Key/License Delivery Service** from which clients can obtain AES keys or PlayReady/Widevine licenses to play the encrypted content.</span></span>

<span data-ttu-id="b6125-109">Om du vill använda för Media Services att kryptera en tillgång, måste du associera en krypteringsnyckel (**CommonEncryption** eller **EnvelopeEncryption**) med tillgången (enligt [här](media-services-dotnet-create-contentkey.md)) och även konfigurera auktoriseringsprinciper för nyckeln (som beskrivs i den här artikeln).</span><span class="sxs-lookup"><span data-stu-id="b6125-109">If you want for Media Services to encrypt an asset, you need to associate an encryption key (**CommonEncryption** or **EnvelopeEncryption**) with the asset (as described [here](media-services-dotnet-create-contentkey.md)) and also configure authorization policies for the key (as described in this article).</span></span>

<span data-ttu-id="b6125-110">När en dataströmmen har begärts av en spelare, använder Media Services den angivna nyckeln för att kryptera dynamiskt innehåll med AES eller DRM-kryptering.</span><span class="sxs-lookup"><span data-stu-id="b6125-110">When a stream is requested by a player, Media Services uses the specified key to dynamically encrypt your content using AES or DRM encryption.</span></span> <span data-ttu-id="b6125-111">Om du vill dekryptera dataströmmen begär spelaren nyckeln från tjänsten nyckel.</span><span class="sxs-lookup"><span data-stu-id="b6125-111">To decrypt the stream, the player will request the key from the key delivery service.</span></span> <span data-ttu-id="b6125-112">Om du vill avgöra om användaren har behörighet att hämta nyckel för utvärderar tjänsten auktoriseringsprinciper som du angav för nyckeln.</span><span class="sxs-lookup"><span data-stu-id="b6125-112">To decide whether or not the user is authorized to get the key, the service evaluates the authorization policies that you specified for the key.</span></span>

<span data-ttu-id="b6125-113">Media Services stöder flera olika sätt att auktorisera användare som begär nycklar.</span><span class="sxs-lookup"><span data-stu-id="b6125-113">Media Services supports multiple ways of authenticating users who make key requests.</span></span> <span data-ttu-id="b6125-114">Principen för auktorisering av innehållsnyckel kan ha en eller flera auktoriseringsbegränsningar: **öppna** eller **token** begränsning.</span><span class="sxs-lookup"><span data-stu-id="b6125-114">The content key authorization policy could have one or more authorization restrictions: **open** or **token** restriction.</span></span> <span data-ttu-id="b6125-115">Den tokenbegränsade principen måste åtföljas av en token utfärdad av en säker tokentjänst (Secure Token Service – STS).</span><span class="sxs-lookup"><span data-stu-id="b6125-115">The token restricted policy must be accompanied by a token issued by a Secure Token Service (STS).</span></span> <span data-ttu-id="b6125-116">Media Services stöder token i den **Simple Web Tokens** ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) format och **JSON Web Token** ([JWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3)) format.</span><span class="sxs-lookup"><span data-stu-id="b6125-116">Media Services supports tokens in the **Simple Web Tokens** ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) format and **JSON Web Token** ([JWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3)) format.</span></span>

<span data-ttu-id="b6125-117">Media Services tillhandahåller inte Secure Token tjänster.</span><span class="sxs-lookup"><span data-stu-id="b6125-117">Media Services does not provide Secure Token Services.</span></span> <span data-ttu-id="b6125-118">Du kan skapa en anpassad STS eller använda Microsoft Azure ACS problemet tokens.</span><span class="sxs-lookup"><span data-stu-id="b6125-118">You can create a custom STS or leverage Microsoft Azure ACS to issue tokens.</span></span> <span data-ttu-id="b6125-119">STS måste konfigureras för att skapa en token som signerats med angiven nyckel och utfärda anspråk som du angav i tokenbegränsningar-konfiguration (som beskrivs i den här artikeln).</span><span class="sxs-lookup"><span data-stu-id="b6125-119">The STS must be configured to create a token signed with the specified key and issue claims that you specified in the token restriction configuration (as described in this article).</span></span> <span data-ttu-id="b6125-120">Media Services viktiga tjänsten returneras krypteringsnyckeln till klienten om token är giltig och anspråk i token som matchar de som konfigurerats för innehållsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="b6125-120">The Media Services key delivery service will return the encryption key to the client if the token is valid and the claims in the token match those configured for the content key.</span></span>

<span data-ttu-id="b6125-121">Mer information finns i</span><span class="sxs-lookup"><span data-stu-id="b6125-121">For more information, see</span></span>

[<span data-ttu-id="b6125-122">JWT-token autentisering</span><span class="sxs-lookup"><span data-stu-id="b6125-122">JWT token authentication</span></span>](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)

<span data-ttu-id="b6125-123">[Integrera Azure Media Services OWIN MVC baserat app med Azure Active Directory och begränsa viktiga innehållsleverans baserat på JWT anspråk](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).</span><span class="sxs-lookup"><span data-stu-id="b6125-123">[Integrate Azure Media Services OWIN MVC based app with Azure Active Directory and restrict content key delivery based on JWT claims](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).</span></span>

<span data-ttu-id="b6125-124">[Använd Azure ACS problemet tokens](http://mingfeiy.com/acs-with-key-services).</span><span class="sxs-lookup"><span data-stu-id="b6125-124">[Use Azure ACS to issue tokens](http://mingfeiy.com/acs-with-key-services).</span></span>

### <a name="some-considerations-apply"></a><span data-ttu-id="b6125-125">Vissa förutsättningar gäller:</span><span class="sxs-lookup"><span data-stu-id="b6125-125">Some considerations apply:</span></span>
* <span data-ttu-id="b6125-126">När AMS-kontot skapas en **standard** strömningsslutpunkt har lagts till i ditt konto i den **stoppad** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="b6125-126">When your AMS account is created a **default** streaming endpoint is added  to your account in the **Stopped** state.</span></span> <span data-ttu-id="b6125-127">Om du vill starta strömning ditt innehåll och dra nytta av dynamisk paketering och dynamisk kryptering strömmande slutpunkten måste vara i den **kör** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="b6125-127">To start streaming your content and take advantage of dynamic packaging and dynamic encryption, your streaming endpoint has to be in the **Running** state.</span></span> 
* <span data-ttu-id="b6125-128">Din tillgång måste innehålla en uppsättning MP4s med anpassningsbar bithastighet eller Smooth Streaming-filer.</span><span class="sxs-lookup"><span data-stu-id="b6125-128">Your asset must contain a set of adaptive bitrate MP4s or  adaptive bitrate Smooth Streaming files.</span></span> <span data-ttu-id="b6125-129">Mer information finns i [koda en tillgång](media-services-encode-asset.md).</span><span class="sxs-lookup"><span data-stu-id="b6125-129">For more information, see [Encode an asset](media-services-encode-asset.md).</span></span>
* <span data-ttu-id="b6125-130">Ladda upp och koda dina tillgångar med **AssetCreationOptions.StorageEncrypted** alternativet.</span><span class="sxs-lookup"><span data-stu-id="b6125-130">Upload and encode your assets using **AssetCreationOptions.StorageEncrypted** option.</span></span>
* <span data-ttu-id="b6125-131">Om du planerar att ha flera nycklar för innehåll som kräver i samma konfiguration, rekommenderas att skapa en enda auktoriseringsprincip och återanvända med nycklar för multiinnehåll.</span><span class="sxs-lookup"><span data-stu-id="b6125-131">If you plan to have multiple content keys that require the same policy configuration, it is strongly recommended to create a single authorization policy and reuse it with multiple content keys.</span></span>
* <span data-ttu-id="b6125-132">Tjänsten nyckeln cachelagrar ContentKeyAuthorizationPolicy och dess relaterade objekt (alternativ och begränsningar) i 15 minuter.</span><span class="sxs-lookup"><span data-stu-id="b6125-132">The Key Delivery service caches ContentKeyAuthorizationPolicy and its related objects (policy options and restrictions) for 15 minutes.</span></span>  <span data-ttu-id="b6125-133">Om du skapar en ContentKeyAuthorizationPolicy och ange om du vill använda en ”Token” begränsning, testa, och uppdatera principen till ”öppen” begränsningen, det tar ungefär 15 minuter innan principen växlar till ”öppen” versionen av principen.</span><span class="sxs-lookup"><span data-stu-id="b6125-133">If you create a ContentKeyAuthorizationPolicy and specify to use a “Token” restriction, then test it, and then update the policy to “Open” restriction, it will take roughly 15 minutes before the policy switches to the “Open” version of the policy.</span></span>
* <span data-ttu-id="b6125-134">Om du lägger till eller uppdaterar din tillgångs leveransprincip måste du ta bort en befintlig lokaliserare (om sådan finns) och skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="b6125-134">If you add or update your asset’s delivery policy, you must delete an existing locator (if any) and create a new locator.</span></span>
* <span data-ttu-id="b6125-135">Det går för närvarande kryptera progressiv hämtning.</span><span class="sxs-lookup"><span data-stu-id="b6125-135">Currently, you cannot encrypt progressive downloads.</span></span>

## <a name="aes-128-dynamic-encryption"></a><span data-ttu-id="b6125-136">AES-128 dynamisk kryptering</span><span class="sxs-lookup"><span data-stu-id="b6125-136">AES-128 Dynamic Encryption</span></span>
### <a name="open-restriction"></a><span data-ttu-id="b6125-137">Öppna begränsning</span><span class="sxs-lookup"><span data-stu-id="b6125-137">Open Restriction</span></span>
<span data-ttu-id="b6125-138">Öppna begränsning innebär systemet ger nyckeln till alla som begär nycklar.</span><span class="sxs-lookup"><span data-stu-id="b6125-138">Open restriction means the system will deliver the key to anyone who makes a key request.</span></span> <span data-ttu-id="b6125-139">Den här begränsningen kan vara användbart för testning.</span><span class="sxs-lookup"><span data-stu-id="b6125-139">This restriction might be useful for testing purposes.</span></span>

<span data-ttu-id="b6125-140">I följande exempel skapar en öppen auktoriseringsprincip och lägger till den innehållsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="b6125-140">The following example creates an open authorization policy and adds it to the content key.</span></span>

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

        // Add ContentKeyAutorizationPolicy to ContentKey
        contentKey.AuthorizationPolicyId = policy.Id;
        IContentKey updatedKey = contentKey.UpdateAsync().Result;
        Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);
    }


### <a name="token-restriction"></a><span data-ttu-id="b6125-141">Tokenbegränsningar</span><span class="sxs-lookup"><span data-stu-id="b6125-141">Token Restriction</span></span>
<span data-ttu-id="b6125-142">Det här avsnittet beskrivs hur du skapar en princip för auktorisering av innehållsnyckel och associera det med innehållsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="b6125-142">This section describes how to create a content key authorization policy and associate it with the content key.</span></span> <span data-ttu-id="b6125-143">Auktoriseringsprincipen beskriver vilka auktorisering krav måste uppfyllas för att avgöra om användaren har behörighet att ta emot nyckeln (till exempel innehåller listan ”Verifieringsnyckeln” innehålla den nyckel som token som signerats med).</span><span class="sxs-lookup"><span data-stu-id="b6125-143">The authorization policy describes what authorization requirements must be met to determine if the user is authorized to receive the key (for example, does the “verification key” list contain the key that the token was signed with).</span></span>

<span data-ttu-id="b6125-144">Om du vill konfigurera alternativet tokenbegränsningar som du behöver använda en XML för att beskriva behörighetskraven för token.</span><span class="sxs-lookup"><span data-stu-id="b6125-144">To configure the token restriction option, you need to use an XML to describe the token’s authorization requirements.</span></span> <span data-ttu-id="b6125-145">Tokenbegränsningar konfigurations-XML måste uppfylla följande XML-schema.</span><span class="sxs-lookup"><span data-stu-id="b6125-145">The token restriction configuration XML must conform to the following XML schema.</span></span>

#### <span data-ttu-id="b6125-146"><a id="schema"></a>Tokenbegränsningar schema</span><span class="sxs-lookup"><span data-stu-id="b6125-146"><a id="schema"></a>Token restriction schema</span></span>
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

<span data-ttu-id="b6125-147">När du konfigurerar den **token** begränsad princip, måste du ange den primära ** verifiering nyckeln **, **utfärdaren** och **målgruppen** parametrar.</span><span class="sxs-lookup"><span data-stu-id="b6125-147">When configuring the **token** restricted policy, you must specify the primary** verification key**, **issuer** and **audience** parameters.</span></span> <span data-ttu-id="b6125-148">Den ** primära Verifieringsnyckeln ** innehåller den nyckel som token som signerats med, **utfärdaren** är den säkra tokentjänst som utfärdar token.</span><span class="sxs-lookup"><span data-stu-id="b6125-148">The **primary verification key **contains the key that the token was signed with, **issuer** is the secure token service that issues the token.</span></span> <span data-ttu-id="b6125-149">Den **målgruppen** (kallas ibland **omfång**) beskriver syftet med token eller token auktoriserar åtkomst till resursen.</span><span class="sxs-lookup"><span data-stu-id="b6125-149">The **audience** (sometimes called **scope**) describes the intent of the token or the resource the token authorizes access to.</span></span> <span data-ttu-id="b6125-150">Media Services viktiga tjänsten verifierar att dessa värden i token matchar värdena i mallen.</span><span class="sxs-lookup"><span data-stu-id="b6125-150">The Media Services key delivery service validates that these values in the token match the values in the template.</span></span> 

<span data-ttu-id="b6125-151">När du använder **Media Services SDK för .NET**, du kan använda den **TokenRestrictionTemplate** klassen för att skapa begränsning-token.</span><span class="sxs-lookup"><span data-stu-id="b6125-151">When using **Media Services SDK for .NET**, you can use the **TokenRestrictionTemplate** class to generate the restriction token.</span></span>
<span data-ttu-id="b6125-152">I följande exempel skapas en auktoriseringsprincip med en token begränsning.</span><span class="sxs-lookup"><span data-stu-id="b6125-152">The following example creates an authorization policy with a token restriction.</span></span> <span data-ttu-id="b6125-153">I det här exemplet klienten skulle ha presentera en token som innehåller: signering nyckel (VerificationKey), en token utfärdare och nödvändiga anspråk.</span><span class="sxs-lookup"><span data-stu-id="b6125-153">In this example, the client would have to present a token that contains: signing key (VerificationKey), a token issuer, and required claims.</span></span>

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

        // Add ContentKeyAutorizationPolicy to ContentKey
        contentKey.AuthorizationPolicyId = policy.Id;
        IContentKey updatedKey = contentKey.UpdateAsync().Result;
        Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);

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

#### <span data-ttu-id="b6125-154"><a id="test"></a>Test-token</span><span class="sxs-lookup"><span data-stu-id="b6125-154"><a id="test"></a>Test token</span></span>
<span data-ttu-id="b6125-155">Gör följande för att få en test-token baserat på de tokenbegränsningar som användes för nyckelauktoriseringsprincipen.</span><span class="sxs-lookup"><span data-stu-id="b6125-155">To get a test token based on the token restriction that was used for the key authorization policy, do the following.</span></span>

    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate =
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);

    // Generate a test token based on the the data in the given TokenRestrictionTemplate.
    // Note, you need to pass the key id Guid because we specified 
    // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
    Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);

    //The GenerateTestToken method returns the token without the word “Bearer” in front
    //so you have to add it in front of the token string. 
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey);
    Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
    Console.WriteLine();


## <a name="playready-dynamic-encryption"></a><span data-ttu-id="b6125-156">PlayReady-dynamisk kryptering</span><span class="sxs-lookup"><span data-stu-id="b6125-156">PlayReady Dynamic Encryption</span></span>
<span data-ttu-id="b6125-157">Media Services kan du konfigurera behörigheter och begränsningar som du vill använda för PlayReady DRM-körningsmiljön att tvinga när en användare försöker att spela upp skyddat innehåll.</span><span class="sxs-lookup"><span data-stu-id="b6125-157">Media Services enables you to configure the rights and restrictions that you want for the PlayReady DRM runtime to enforce when a user is trying to play back protected content.</span></span> 

<span data-ttu-id="b6125-158">När du skyddar ditt innehåll med PlayReady, en av de saker som du måste ange i principen för auktorisering är en XML-sträng som definierar den [PlayReady-licensmall](media-services-playready-license-template-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b6125-158">When protecting your content with PlayReady, one of the things you need to specify in your authorization policy is an XML string that defines the [PlayReady license template](media-services-playready-license-template-overview.md).</span></span> <span data-ttu-id="b6125-159">I Media Services SDK för .NET, den **PlayReadyLicenseResponseTemplate** och **PlayReadyLicenseTemplate** klasser hjälper dig att definiera PlayReady License-mall.</span><span class="sxs-lookup"><span data-stu-id="b6125-159">In Media Services SDK for .NET, the **PlayReadyLicenseResponseTemplate** and **PlayReadyLicenseTemplate** classes will help you define the PlayReady License Template.</span></span>

<span data-ttu-id="b6125-160">[Det här avsnittet](media-services-protect-with-drm.md) visar hur du krypterar ditt innehåll med **PlayReady** och **Widevine**.</span><span class="sxs-lookup"><span data-stu-id="b6125-160">[This topic](media-services-protect-with-drm.md) shows how to encrypt your content with **PlayReady** and **Widevine**.</span></span>

### <a name="open-restriction"></a><span data-ttu-id="b6125-161">Öppna begränsning</span><span class="sxs-lookup"><span data-stu-id="b6125-161">Open Restriction</span></span>
<span data-ttu-id="b6125-162">Öppna begränsning innebär systemet ger nyckeln till alla som begär nycklar.</span><span class="sxs-lookup"><span data-stu-id="b6125-162">Open restriction means the system will deliver the key to anyone who makes a key request.</span></span> <span data-ttu-id="b6125-163">Den här begränsningen kan vara användbart för testning.</span><span class="sxs-lookup"><span data-stu-id="b6125-163">This restriction might be useful for testing purposes.</span></span>

<span data-ttu-id="b6125-164">I följande exempel skapar en öppen auktoriseringsprincip och lägger till den innehållsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="b6125-164">The following example creates an open authorization policy and adds it to the content key.</span></span>

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

        // Associate the content key authorization policy with the content key.
        contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
        contentKey = contentKey.UpdateAsync().Result;
    }

### <a name="token-restriction"></a><span data-ttu-id="b6125-165">Tokenbegränsningar</span><span class="sxs-lookup"><span data-stu-id="b6125-165">Token Restriction</span></span>
<span data-ttu-id="b6125-166">Om du vill konfigurera alternativet tokenbegränsningar som du behöver använda en XML för att beskriva behörighetskraven för token.</span><span class="sxs-lookup"><span data-stu-id="b6125-166">To configure the token restriction option, you need to use an XML to describe the token’s authorization requirements.</span></span> <span data-ttu-id="b6125-167">Konfigurationen av tokenbegränsningar XML måste motsvara det XML-schemat visas i [detta](#schema) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="b6125-167">The token restriction configuration XML must conform to the XML schema shown in [this](#schema) section.</span></span>

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

        // Add ContentKeyAutorizationPolicy to ContentKey
        contentKeyAuthorizationPolicy.Options.Add(policyOption);

        // Associate the content key authorization policy with the content key
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
        // The following code configures PlayReady License Template using .NET classes
        // and returns the XML string.

        //The PlayReadyLicenseResponseTemplate class represents the template for the response sent back to the end user. 
        //It contains a field for a custom data string between the license server and the application 
        //(may be useful for custom app logic) as well as a list of one or more license templates.
        PlayReadyLicenseResponseTemplate responseTemplate = new PlayReadyLicenseResponseTemplate();

        // The PlayReadyLicenseTemplate class represents a license template for creating PlayReady licenses
        // to be returned to the end users. 
        //It contains the data on the content key in the license and any rights or restrictions to be 
        //enforced by the PlayReady DRM runtime when using the content key.
        PlayReadyLicenseTemplate licenseTemplate = new PlayReadyLicenseTemplate();
        //Configure whether the license is persistent (saved in persistent storage on the client) 
        //or non-persistent (only held in memory while the player is using the license).  
        licenseTemplate.LicenseType = PlayReadyLicenseType.Nonpersistent;

        // AllowTestDevices controls whether test devices can use the license or not.  
        // If true, the MinimumSecurityLevel property of the license
        // is set to 150.  If false (the default), the MinimumSecurityLevel property of the license is set to 2000.
        licenseTemplate.AllowTestDevices = true;


        // You can also configure the Play Right in the PlayReady license by using the PlayReadyPlayRight class. 
        // It grants the user the ability to playback the content subject to the zero or more restrictions 
        // configured in the license and on the PlayRight itself (for playback specific policy). 
        // Much of the policy on the PlayRight has to do with output restrictions 
        // which control the types of outputs that the content can be played over and 
        // any restrictions that must be put in place when using a given output.
        // For example, if the DigitalVideoOnlyContentRestriction is enabled, 
        //then the DRM runtime will only allow the video to be displayed over digital outputs 
        //(analog video outputs won’t be allowed to pass the content).

        //IMPORTANT: These types of restrictions can be very powerful but can also affect the consumer experience. 
        // If the output protections are configured too restrictive, 
        // the content might be unplayable on some clients. For more information, see the PlayReady Compliance Rules document.

        // For example:
        //licenseTemplate.PlayRight.AgcAndColorStripeRestriction = new AgcAndColorStripeRestriction(1);

        responseTemplate.LicenseTemplates.Add(licenseTemplate);

        return MediaServicesLicenseTemplateSerializer.Serialize(responseTemplate);
    }


<span data-ttu-id="b6125-168">Att hämta en token för test baserat på de tokenbegränsningar som användes för auktorisering av innehållsnyckel princip Se [detta](#test) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="b6125-168">To get a test token based on the token restriction that was used for the key authorization policy see [this](#test) section.</span></span> 

## <span data-ttu-id="b6125-169"><a id="types"></a>Typer som används när du definierar ContentKeyAuthorizationPolicy</span><span class="sxs-lookup"><span data-stu-id="b6125-169"><a id="types"></a>Types used when defining ContentKeyAuthorizationPolicy</span></span>
### <span data-ttu-id="b6125-170"><a id="ContentKeyRestrictionType"></a>ContentKeyRestrictionType</span><span class="sxs-lookup"><span data-stu-id="b6125-170"><a id="ContentKeyRestrictionType"></a>ContentKeyRestrictionType</span></span>
    public enum ContentKeyRestrictionType
    {
        Open = 0,
        TokenRestricted = 1,
        IPRestricted = 2,
    }

### <span data-ttu-id="b6125-171"><a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType</span><span class="sxs-lookup"><span data-stu-id="b6125-171"><a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType</span></span>
    public enum ContentKeyDeliveryType
    {
      None = 0,
      PlayReadyLicense = 1,
      BaselineHttp = 2,
      Widevine = 3
    }

### <span data-ttu-id="b6125-172"><a id="TokenType"></a>TokenType</span><span class="sxs-lookup"><span data-stu-id="b6125-172"><a id="TokenType"></a>TokenType</span></span>
    public enum TokenType
    {
        Undefined = 0,
        SWT = 1,
        JWT = 2,
    }



## <a name="media-services-learning-paths"></a><span data-ttu-id="b6125-173">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="b6125-173">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="b6125-174">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="b6125-174">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-step"></a><span data-ttu-id="b6125-175">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b6125-175">Next step</span></span>
<span data-ttu-id="b6125-176">Nu när du har konfigurerat innehållsnyckelns auktoriseringsprincip, gå till den [konfigurera tillgångsleveransprincip](media-services-dotnet-configure-asset-delivery-policy.md) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="b6125-176">Now that you have configured content key's authorization policy, go to the [How to configure asset delivery policy](media-services-dotnet-configure-asset-delivery-policy.md) topic.</span></span>

