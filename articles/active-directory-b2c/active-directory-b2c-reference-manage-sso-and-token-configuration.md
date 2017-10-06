---
title: 'Azure Active Directory B2C: Hantera enkel inloggning och token anpassning med anpassade principer | Microsoft Docs'
description: 
services: active-directory-b2c
documentationcenter: 
author: sama
ms.assetid: eec4d418-453f-4755-8b30-5ed997841b56
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 05/02/2017
ms.author: sama
ms.openlocfilehash: b65271a22c77ea41eeec2126e4a3ad24364edd17
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-manage-sso-and-token-customization-with-custom-policies"></a><span data-ttu-id="c94f6-102">Azure Active Directory B2C: Hantera enkel inloggning och token anpassning med anpassade principer</span><span class="sxs-lookup"><span data-stu-id="c94f6-102">Azure Active Directory B2C: Manage SSO and token customization with custom policies</span></span>
<span data-ttu-id="c94f6-103">Anpassade principer ger du hello samma kontroll över din token, session och enkel inloggning (SSO) konfigurationer som via inbyggda principer.</span><span class="sxs-lookup"><span data-stu-id="c94f6-103">Using custom policies provides you hello same control over your token, session and single sign-on (SSO) configurations as through built-in policies.</span></span>  <span data-ttu-id="c94f6-104">toolearn vad varje inställning har finns hello dokumentationen [här](#active-directory-b2c-token-session-sso).</span><span class="sxs-lookup"><span data-stu-id="c94f6-104">toolearn what each setting does, please see hello documentation [here](#active-directory-b2c-token-session-sso).</span></span>

## <a name="token-lifetimes-and-claims-configuration"></a><span data-ttu-id="c94f6-105">Konfiguration av token livslängd och anspråk</span><span class="sxs-lookup"><span data-stu-id="c94f6-105">Token lifetimes and claims configuration</span></span>
<span data-ttu-id="c94f6-106">toochange hello inställningar på din token livslängd, behöver du tooadd en `<ClaimsProviders>` element i hello förlitande part-filen för hello princip du vill tooimpact.</span><span class="sxs-lookup"><span data-stu-id="c94f6-106">toochange hello settings on your token lifetimes, you need tooadd a `<ClaimsProviders>` element in hello relying party file of hello policy you want tooimpact.</span></span>  <span data-ttu-id="c94f6-107">Hej `<ClaimsProviders>` element är underordnad hello `<TrustFrameworkPolicy>`.</span><span class="sxs-lookup"><span data-stu-id="c94f6-107">hello `<ClaimsProviders>` element is a child of hello `<TrustFrameworkPolicy>`.</span></span>  <span data-ttu-id="c94f6-108">Du behöver inuti tooput hello information som påverkar din token livslängd.</span><span class="sxs-lookup"><span data-stu-id="c94f6-108">Inside you'll need tooput hello information that affects your token lifetimes.</span></span>  <span data-ttu-id="c94f6-109">hello XML ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="c94f6-109">hello XML looks like this:</span></span>

```XML
<ClaimsProviders>
   <ClaimsProvider>
      <DisplayName>Token Issuer</DisplayName>
      <TechnicalProfiles>
         <TechnicalProfile Id="JwtIssuer">
            <Metadata>
               <Item Key="token_lifetime_secs">3600</Item>
               <Item Key="id_token_lifetime_secs">3600</Item>
               <Item Key="refresh_token_lifetime_secs">1209600</Item>
               <Item Key="rolling_refresh_token_lifetime_secs">7776000</Item>
               <Item Key="IssuanceClaimPattern">AuthorityAndTenantGuid</Item>
               <Item Key="AuthenticationContextReferenceClaimPattern">None</Item>
            </Metadata>
         </TechnicalProfile>
      </TechnicalProfiles>
   </ClaimsProvider>
</ClaimsProviders>
```

<span data-ttu-id="c94f6-110">**Åtkomst-token livslängd** hello åtkomst livslängd för token kan ändras genom att ändra hello värdet i hello `<Item>` med hello Key = ”token_lifetime_secs” i sekunder.</span><span class="sxs-lookup"><span data-stu-id="c94f6-110">**Access token lifetimes** hello access token lifetime can be changed by modifying hello value inside hello `<Item>` with hello Key="token_lifetime_secs" in seconds.</span></span>  <span data-ttu-id="c94f6-111">hello standardvärdet i inbyggda är 3 600 sekunder (60 minuter).</span><span class="sxs-lookup"><span data-stu-id="c94f6-111">hello default value in built-in is 3600 seconds (60 minutes).</span></span>

<span data-ttu-id="c94f6-112">**Livslängd för token ID** livslängd för token hello-ID kan ändras genom att ändra hello värdet i hello `<Item>` med hello Key = ”id_token_lifetime_secs” i sekunder.</span><span class="sxs-lookup"><span data-stu-id="c94f6-112">**ID token lifetime** hello ID token lifetime can be changed by modifying hello value inside hello `<Item>` with hello Key="id_token_lifetime_secs" in seconds.</span></span>  <span data-ttu-id="c94f6-113">hello standardvärdet i inbyggda är 3 600 sekunder (60 minuter).</span><span class="sxs-lookup"><span data-stu-id="c94f6-113">hello default value in built-in is 3600 seconds (60 minutes).</span></span>

<span data-ttu-id="c94f6-114">**Uppdatera livslängd för token** livslängd för token hello uppdatering kan ändras genom att ändra hello värdet i hello `<Item>` med hello Key = ”refresh_token_lifetime_secs” i sekunder.</span><span class="sxs-lookup"><span data-stu-id="c94f6-114">**Refresh token lifetime** hello refresh token lifetime can be changed by modifying hello value inside hello `<Item>` with hello Key="refresh_token_lifetime_secs" in seconds.</span></span>  <span data-ttu-id="c94f6-115">hello standardvärdet i inbyggda är 1209600 sekunder (14 dagar).</span><span class="sxs-lookup"><span data-stu-id="c94f6-115">hello default value in built-in is 1209600 seconds (14 days).</span></span>

<span data-ttu-id="c94f6-116">**Uppdatera livslängd för token glidande fönstret** om du vill tooset ett glidande fönstret tooyour uppdatering livslängdstoken ändrar hello värdet i `<Item>` med hello Key = ”rolling_refresh_token_lifetime_secs” i sekunder.</span><span class="sxs-lookup"><span data-stu-id="c94f6-116">**Refresh token sliding window lifetime** If you would like tooset a sliding window lifetime tooyour refresh token, modify hello value inside `<Item>` with hello Key="rolling_refresh_token_lifetime_secs" in seconds.</span></span>  <span data-ttu-id="c94f6-117">hello standardvärdet i inbyggda är 7776000 (90 dagar).</span><span class="sxs-lookup"><span data-stu-id="c94f6-117">hello default value in built-in is 7776000 (90 days).</span></span>  <span data-ttu-id="c94f6-118">Om du inte vill tooenfore ett glidande fönstret livslängd, ersätter den här raden med:</span><span class="sxs-lookup"><span data-stu-id="c94f6-118">If you don't want tooenfore a sliding window lifetime, replace this line with:</span></span>
```XML
<Item Key="allow_infinite_rolling_refresh_token">True</Item>
```

<span data-ttu-id="c94f6-119">**Utfärdaren (iss) anspråk** toochange hello utfärdaren (iss) anspråk kan ändra hello värdet i hello `<Item>` med hello Key = ”IssuanceClaimPattern”.</span><span class="sxs-lookup"><span data-stu-id="c94f6-119">**Issuer (iss) claim** If you want toochange hello Issuer (iss) claim, modify hello value inside hello `<Item>` with hello Key="IssuanceClaimPattern".</span></span>  <span data-ttu-id="c94f6-120">hello lämpliga värden är `AuthorityAndTenantGuid` och `AuthorityWithTfp`.</span><span class="sxs-lookup"><span data-stu-id="c94f6-120">hello applicable values are `AuthorityAndTenantGuid` and `AuthorityWithTfp`.</span></span>

<span data-ttu-id="c94f6-121">**Inställningen anspråk som representerar princip-ID** hello alternativ för det här värdet är TFP (förtroendeprincipen för framework) och ACR (authentication kontexten referens).</span><span class="sxs-lookup"><span data-stu-id="c94f6-121">**Setting claim representing policy ID** hello options for setting this value are TFP (trust framework policy) and ACR (authentication context reference).</span></span>  
<span data-ttu-id="c94f6-122">Vi rekommenderar den här tooTFP toodo, kontrollera hello `<Item>` med hello Key = ”AuthenticationContextReferenceClaimPattern” finns och hello värde är `None`.</span><span class="sxs-lookup"><span data-stu-id="c94f6-122">We recommend setting this tooTFP, toodo this, ensure hello `<Item>` with hello Key="AuthenticationContextReferenceClaimPattern" exists and hello value is `None`.</span></span>
<span data-ttu-id="c94f6-123">I din `<OutputClaims>` objektet, lägga till det här elementet:</span><span class="sxs-lookup"><span data-stu-id="c94f6-123">In your `<OutputClaims>` item, add this element:</span></span>
```XML
<OutputClaim ClaimTypeReferenceId="trustFrameworkPolicy" Required="true" DefaultValue="{policy}" />
```
<span data-ttu-id="c94f6-124">Ta bort hello för ACR, `<Item>` med hello Key = ”AuthenticationContextReferenceClaimPattern”.</span><span class="sxs-lookup"><span data-stu-id="c94f6-124">For ACR, remove hello `<Item>` with hello Key="AuthenticationContextReferenceClaimPattern".</span></span>

<span data-ttu-id="c94f6-125">**Ämne (under) anspråk** det här alternativet är som standard tooObjectID, om du vill tooswitch detta för`Not Supported`, hello följande:</span><span class="sxs-lookup"><span data-stu-id="c94f6-125">**Subject (sub) claim** This option is defaulted tooObjectID, if you would like tooswitch this too`Not Supported`, do hello following:</span></span>

<span data-ttu-id="c94f6-126">Ersätt den här raden</span><span class="sxs-lookup"><span data-stu-id="c94f6-126">Replace this line</span></span> 
```XML
<OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub" />
```
<span data-ttu-id="c94f6-127">med den här raden:</span><span class="sxs-lookup"><span data-stu-id="c94f6-127">with this line:</span></span>
```XML
<OutputClaim ClaimTypeReferenceId="sub" />
```

## <a name="session-behavior-and-sso"></a><span data-ttu-id="c94f6-128">Sessionen fungerar och enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c94f6-128">Session behavior and SSO</span></span>
<span data-ttu-id="c94f6-129">toochange din session beteende och konfigurationer för enkel inloggning måste tooadd en `<UserJourneyBehaviors>` -element inuti hello `<RelyingParty>` element.</span><span class="sxs-lookup"><span data-stu-id="c94f6-129">toochange your session behavior and SSO configurations, you need tooadd a `<UserJourneyBehaviors>` element inside of hello `<RelyingParty>` element.</span></span>  <span data-ttu-id="c94f6-130">Hej `<UserJourneyBehaviors>` element måste följa omedelbart efter hello `<DefaultUserJourney>`.</span><span class="sxs-lookup"><span data-stu-id="c94f6-130">hello `<UserJourneyBehaviors>` element must immediately follow hello `<DefaultUserJourney>`.</span></span>  <span data-ttu-id="c94f6-131">Hej inuti din `<UserJourneyBehavors>` element ska se ut så här:</span><span class="sxs-lookup"><span data-stu-id="c94f6-131">hello inside of your `<UserJourneyBehavors>` element should look like this:</span></span>

```XML
<UserJourneyBehaviors>
   <SingleSignOn Scope="Application" />
   <SessionExpiryType>Absolute</SessionExpiryType>
   <SessionExpiryInSeconds>86400</SessionExpiryInSeconds>
</UserJourneyBehaviors>
```
<span data-ttu-id="c94f6-132">**Enkel inloggning (SSO) konfiguration** toochange hello enkel inloggning konfiguration, behöver du toomodify hello värdet för `<SingleSignOn>`.</span><span class="sxs-lookup"><span data-stu-id="c94f6-132">**Single sign-on (SSO) configuration** toochange hello single sign-on configuration, you need toomodify hello value of `<SingleSignOn>`.</span></span>  <span data-ttu-id="c94f6-133">hello lämpliga värden är `Tenant`, `Application`, `Policy` och `Disabled`.</span><span class="sxs-lookup"><span data-stu-id="c94f6-133">hello applicable values are `Tenant`, `Application`, `Policy` and `Disabled`.</span></span> 

<span data-ttu-id="c94f6-134">**Webbprogrammet session-livstid (minuter)** toochange hello hello webbprogrammet sessioners livstid måste toomodify värdet för hello `<SessionExpiryInSeconds>` element.</span><span class="sxs-lookup"><span data-stu-id="c94f6-134">**Web app session lifetime (minutes)** toochange hello hello web app session lifetime, you need toomodify value of hello `<SessionExpiryInSeconds>` element.</span></span>  <span data-ttu-id="c94f6-135">hello standardvärdet i inbyggda principer är 86400 sekunder (1 440 minuter).</span><span class="sxs-lookup"><span data-stu-id="c94f6-135">hello default value in built-in policies is 86400 seconds (1440 minutes).</span></span>

<span data-ttu-id="c94f6-136">**Tidsgräns för Web app** toochange Sessionstidsgräns hello web app, behöver du toomodify hello värdet för `<SessionExpiryType>`.</span><span class="sxs-lookup"><span data-stu-id="c94f6-136">**Web app session timeout** toochange hello web app session timeout, you need toomodify hello value of `<SessionExpiryType>`.</span></span>  <span data-ttu-id="c94f6-137">hello lämpliga värden är `Absolute` och `Rolling`.</span><span class="sxs-lookup"><span data-stu-id="c94f6-137">hello applicable values are `Absolute` and `Rolling`.</span></span>
