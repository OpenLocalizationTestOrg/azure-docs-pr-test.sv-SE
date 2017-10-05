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
ms.openlocfilehash: 8f5703d15766f221517cd89352d41685652d32d6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-active-directory-b2c-manage-sso-and-token-customization-with-custom-policies"></a><span data-ttu-id="ebcbf-102">Azure Active Directory B2C: Hantera enkel inloggning och token anpassning med anpassade principer</span><span class="sxs-lookup"><span data-stu-id="ebcbf-102">Azure Active Directory B2C: Manage SSO and token customization with custom policies</span></span>
<span data-ttu-id="ebcbf-103">Anpassade principer ger dig samma kontroll över din token, session och enkel inloggning (SSO) konfigurationer som via inbyggda principer.</span><span class="sxs-lookup"><span data-stu-id="ebcbf-103">Using custom policies provides you the same control over your token, session and single sign-on (SSO) configurations as through built-in policies.</span></span>  <span data-ttu-id="ebcbf-104">Om du vill veta vad varje inställning gör finns i dokumentationen för [här](#active-directory-b2c-token-session-sso).</span><span class="sxs-lookup"><span data-stu-id="ebcbf-104">To learn what each setting does, please see the documentation [here](#active-directory-b2c-token-session-sso).</span></span>

## <a name="token-lifetimes-and-claims-configuration"></a><span data-ttu-id="ebcbf-105">Konfiguration av token livslängd och anspråk</span><span class="sxs-lookup"><span data-stu-id="ebcbf-105">Token lifetimes and claims configuration</span></span>
<span data-ttu-id="ebcbf-106">Om du vill ändra inställningarna för din token livslängd, måste du lägga till en `<ClaimsProviders>` element i filen förlitande part för den princip du vill påverka.</span><span class="sxs-lookup"><span data-stu-id="ebcbf-106">To change the settings on your token lifetimes, you need to add a `<ClaimsProviders>` element in the relying party file of the policy you want to impact.</span></span>  <span data-ttu-id="ebcbf-107">Den `<ClaimsProviders>` element är underordnad den `<TrustFrameworkPolicy>`.</span><span class="sxs-lookup"><span data-stu-id="ebcbf-107">The `<ClaimsProviders>` element is a child of the `<TrustFrameworkPolicy>`.</span></span>  <span data-ttu-id="ebcbf-108">Inuti måste du placera den information som påverkar din token livslängd.</span><span class="sxs-lookup"><span data-stu-id="ebcbf-108">Inside you'll need to put the information that affects your token lifetimes.</span></span>  <span data-ttu-id="ebcbf-109">XML-filen ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="ebcbf-109">The XML looks like this:</span></span>

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

<span data-ttu-id="ebcbf-110">**Åtkomst-token livslängd** livslängd för åtkomst-token kan ändras genom att ändra värdet i den `<Item>` med nyckel = ”token_lifetime_secs” i sekunder.</span><span class="sxs-lookup"><span data-stu-id="ebcbf-110">**Access token lifetimes** The access token lifetime can be changed by modifying the value inside the `<Item>` with the Key="token_lifetime_secs" in seconds.</span></span>  <span data-ttu-id="ebcbf-111">Standardvärdet i inbyggda är 3 600 sekunder (60 minuter).</span><span class="sxs-lookup"><span data-stu-id="ebcbf-111">The default value in built-in is 3600 seconds (60 minutes).</span></span>

<span data-ttu-id="ebcbf-112">**Livslängd för token ID** ID livslängd för token kan ändras genom att ändra värdet i den `<Item>` med nyckel = ”id_token_lifetime_secs” i sekunder.</span><span class="sxs-lookup"><span data-stu-id="ebcbf-112">**ID token lifetime** The ID token lifetime can be changed by modifying the value inside the `<Item>` with the Key="id_token_lifetime_secs" in seconds.</span></span>  <span data-ttu-id="ebcbf-113">Standardvärdet i inbyggda är 3 600 sekunder (60 minuter).</span><span class="sxs-lookup"><span data-stu-id="ebcbf-113">The default value in built-in is 3600 seconds (60 minutes).</span></span>

<span data-ttu-id="ebcbf-114">**Uppdatera livslängd för token** livslängd för token uppdatering kan ändras genom att ändra värdet i den `<Item>` med nyckel = ”refresh_token_lifetime_secs” i sekunder.</span><span class="sxs-lookup"><span data-stu-id="ebcbf-114">**Refresh token lifetime** The refresh token lifetime can be changed by modifying the value inside the `<Item>` with the Key="refresh_token_lifetime_secs" in seconds.</span></span>  <span data-ttu-id="ebcbf-115">Standardvärdet i inbyggda är 1209600 sekunder (14 dagar).</span><span class="sxs-lookup"><span data-stu-id="ebcbf-115">The default value in built-in is 1209600 seconds (14 days).</span></span>

<span data-ttu-id="ebcbf-116">**Uppdatera livslängd för token glidande fönstret** om du vill ange en glidande fönstret livslängd till din uppdateringstoken, ändra värdet i `<Item>` med nyckel = ”rolling_refresh_token_lifetime_secs” i sekunder.</span><span class="sxs-lookup"><span data-stu-id="ebcbf-116">**Refresh token sliding window lifetime** If you would like to set a sliding window lifetime to your refresh token, modify the value inside `<Item>` with the Key="rolling_refresh_token_lifetime_secs" in seconds.</span></span>  <span data-ttu-id="ebcbf-117">Standardvärdet i inbyggda är 7776000 (90 dagar).</span><span class="sxs-lookup"><span data-stu-id="ebcbf-117">The default value in built-in is 7776000 (90 days).</span></span>  <span data-ttu-id="ebcbf-118">Om du inte vill enfore ett glidande fönstret livslängd, ersätter den här raden med:</span><span class="sxs-lookup"><span data-stu-id="ebcbf-118">If you don't want to enfore a sliding window lifetime, replace this line with:</span></span>
```XML
<Item Key="allow_infinite_rolling_refresh_token">True</Item>
```

<span data-ttu-id="ebcbf-119">**Utfärdaren (iss) anspråk** om du vill ändra utfärdaren (iss)-anspråk, ändra värdet i den `<Item>` med nyckel = ”IssuanceClaimPattern”.</span><span class="sxs-lookup"><span data-stu-id="ebcbf-119">**Issuer (iss) claim** If you want to change the Issuer (iss) claim, modify the value inside the `<Item>` with the Key="IssuanceClaimPattern".</span></span>  <span data-ttu-id="ebcbf-120">Lämpliga värden är `AuthorityAndTenantGuid` och `AuthorityWithTfp`.</span><span class="sxs-lookup"><span data-stu-id="ebcbf-120">The applicable values are `AuthorityAndTenantGuid` and `AuthorityWithTfp`.</span></span>

<span data-ttu-id="ebcbf-121">**Inställningen anspråk som representerar princip-ID** alternativen för det här värdet är TFP (förtroendeprincipen för framework) och ACR (authentication kontexten referens).</span><span class="sxs-lookup"><span data-stu-id="ebcbf-121">**Setting claim representing policy ID** The options for setting this value are TFP (trust framework policy) and ACR (authentication context reference).</span></span>  
<span data-ttu-id="ebcbf-122">Vi rekommenderar att du TFP, för att göra detta, se till att den `<Item>` med nyckel = ”AuthenticationContextReferenceClaimPattern” finns och värdet är `None`.</span><span class="sxs-lookup"><span data-stu-id="ebcbf-122">We recommend setting this to TFP, to do this, ensure the `<Item>` with the Key="AuthenticationContextReferenceClaimPattern" exists and the value is `None`.</span></span>
<span data-ttu-id="ebcbf-123">I din `<OutputClaims>` objektet, lägga till det här elementet:</span><span class="sxs-lookup"><span data-stu-id="ebcbf-123">In your `<OutputClaims>` item, add this element:</span></span>
```XML
<OutputClaim ClaimTypeReferenceId="trustFrameworkPolicy" Required="true" DefaultValue="{policy}" />
```
<span data-ttu-id="ebcbf-124">För ACR, ta bort den `<Item>` med nyckel = ”AuthenticationContextReferenceClaimPattern”.</span><span class="sxs-lookup"><span data-stu-id="ebcbf-124">For ACR, remove the `<Item>` with the Key="AuthenticationContextReferenceClaimPattern".</span></span>

<span data-ttu-id="ebcbf-125">**Ämne (under) anspråk** det här alternativet är som standard objekt-ID om du vill växla detta `Not Supported`, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="ebcbf-125">**Subject (sub) claim** This option is defaulted to ObjectID, if you would like to switch this to `Not Supported`, do the following:</span></span>

<span data-ttu-id="ebcbf-126">Ersätt den här raden</span><span class="sxs-lookup"><span data-stu-id="ebcbf-126">Replace this line</span></span> 
```XML
<OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub" />
```
<span data-ttu-id="ebcbf-127">med den här raden:</span><span class="sxs-lookup"><span data-stu-id="ebcbf-127">with this line:</span></span>
```XML
<OutputClaim ClaimTypeReferenceId="sub" />
```

## <a name="session-behavior-and-sso"></a><span data-ttu-id="ebcbf-128">Sessionen fungerar och enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ebcbf-128">Session behavior and SSO</span></span>
<span data-ttu-id="ebcbf-129">Om du vill ändra din session beteende och konfigurationer för enkel inloggning måste du lägga till en `<UserJourneyBehaviors>` element inuti den `<RelyingParty>` element.</span><span class="sxs-lookup"><span data-stu-id="ebcbf-129">To change your session behavior and SSO configurations, you need to add a `<UserJourneyBehaviors>` element inside of the `<RelyingParty>` element.</span></span>  <span data-ttu-id="ebcbf-130">Den `<UserJourneyBehaviors>` element måste omedelbart efter den `<DefaultUserJourney>`.</span><span class="sxs-lookup"><span data-stu-id="ebcbf-130">The `<UserJourneyBehaviors>` element must immediately follow the `<DefaultUserJourney>`.</span></span>  <span data-ttu-id="ebcbf-131">För din `<UserJourneyBehavors>` element ska se ut så här:</span><span class="sxs-lookup"><span data-stu-id="ebcbf-131">The inside of your `<UserJourneyBehavors>` element should look like this:</span></span>

```XML
<UserJourneyBehaviors>
   <SingleSignOn Scope="Application" />
   <SessionExpiryType>Absolute</SessionExpiryType>
   <SessionExpiryInSeconds>86400</SessionExpiryInSeconds>
</UserJourneyBehaviors>
```
<span data-ttu-id="ebcbf-132">**Enkel inloggning (SSO) konfiguration** om du vill ändra konfigurationen för enkel inloggning måste du ändra värdet för `<SingleSignOn>`.</span><span class="sxs-lookup"><span data-stu-id="ebcbf-132">**Single sign-on (SSO) configuration** To change the single sign-on configuration, you need to modify the value of `<SingleSignOn>`.</span></span>  <span data-ttu-id="ebcbf-133">Lämpliga värden är `Tenant`, `Application`, `Policy` och `Disabled`.</span><span class="sxs-lookup"><span data-stu-id="ebcbf-133">The applicable values are `Tenant`, `Application`, `Policy` and `Disabled`.</span></span> 

<span data-ttu-id="ebcbf-134">**Webbprogrammet session-livstid (minuter)** att ändra den webbappen sessioners livstid, måste du ändra värdet för den `<SessionExpiryInSeconds>` element.</span><span class="sxs-lookup"><span data-stu-id="ebcbf-134">**Web app session lifetime (minutes)** To change the the web app session lifetime, you need to modify value of the `<SessionExpiryInSeconds>` element.</span></span>  <span data-ttu-id="ebcbf-135">Standardvärdet i inbyggda principerna är 86400 sekunder (1 440 minuter).</span><span class="sxs-lookup"><span data-stu-id="ebcbf-135">The default value in built-in policies is 86400 seconds (1440 minutes).</span></span>

<span data-ttu-id="ebcbf-136">**Tidsgräns för Web app** om du vill ändra tidsgränsen för sessionen web app måste du ändra värdet för `<SessionExpiryType>`.</span><span class="sxs-lookup"><span data-stu-id="ebcbf-136">**Web app session timeout** To change the web app session timeout, you need to modify the value of `<SessionExpiryType>`.</span></span>  <span data-ttu-id="ebcbf-137">Lämpliga värden är `Absolute` och `Rolling`.</span><span class="sxs-lookup"><span data-stu-id="ebcbf-137">The applicable values are `Absolute` and `Rolling`.</span></span>
