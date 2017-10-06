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
# <a name="azure-active-directory-b2c-manage-sso-and-token-customization-with-custom-policies"></a>Azure Active Directory B2C: Hantera enkel inloggning och token anpassning med anpassade principer
Anpassade principer ger du hello samma kontroll över din token, session och enkel inloggning (SSO) konfigurationer som via inbyggda principer.  toolearn vad varje inställning har finns hello dokumentationen [här](#active-directory-b2c-token-session-sso).

## <a name="token-lifetimes-and-claims-configuration"></a>Konfiguration av token livslängd och anspråk
toochange hello inställningar på din token livslängd, behöver du tooadd en `<ClaimsProviders>` element i hello förlitande part-filen för hello princip du vill tooimpact.  Hej `<ClaimsProviders>` element är underordnad hello `<TrustFrameworkPolicy>`.  Du behöver inuti tooput hello information som påverkar din token livslängd.  hello XML ser ut så här:

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

**Åtkomst-token livslängd** hello åtkomst livslängd för token kan ändras genom att ändra hello värdet i hello `<Item>` med hello Key = ”token_lifetime_secs” i sekunder.  hello standardvärdet i inbyggda är 3 600 sekunder (60 minuter).

**Livslängd för token ID** livslängd för token hello-ID kan ändras genom att ändra hello värdet i hello `<Item>` med hello Key = ”id_token_lifetime_secs” i sekunder.  hello standardvärdet i inbyggda är 3 600 sekunder (60 minuter).

**Uppdatera livslängd för token** livslängd för token hello uppdatering kan ändras genom att ändra hello värdet i hello `<Item>` med hello Key = ”refresh_token_lifetime_secs” i sekunder.  hello standardvärdet i inbyggda är 1209600 sekunder (14 dagar).

**Uppdatera livslängd för token glidande fönstret** om du vill tooset ett glidande fönstret tooyour uppdatering livslängdstoken ändrar hello värdet i `<Item>` med hello Key = ”rolling_refresh_token_lifetime_secs” i sekunder.  hello standardvärdet i inbyggda är 7776000 (90 dagar).  Om du inte vill tooenfore ett glidande fönstret livslängd, ersätter den här raden med:
```XML
<Item Key="allow_infinite_rolling_refresh_token">True</Item>
```

**Utfärdaren (iss) anspråk** toochange hello utfärdaren (iss) anspråk kan ändra hello värdet i hello `<Item>` med hello Key = ”IssuanceClaimPattern”.  hello lämpliga värden är `AuthorityAndTenantGuid` och `AuthorityWithTfp`.

**Inställningen anspråk som representerar princip-ID** hello alternativ för det här värdet är TFP (förtroendeprincipen för framework) och ACR (authentication kontexten referens).  
Vi rekommenderar den här tooTFP toodo, kontrollera hello `<Item>` med hello Key = ”AuthenticationContextReferenceClaimPattern” finns och hello värde är `None`.
I din `<OutputClaims>` objektet, lägga till det här elementet:
```XML
<OutputClaim ClaimTypeReferenceId="trustFrameworkPolicy" Required="true" DefaultValue="{policy}" />
```
Ta bort hello för ACR, `<Item>` med hello Key = ”AuthenticationContextReferenceClaimPattern”.

**Ämne (under) anspråk** det här alternativet är som standard tooObjectID, om du vill tooswitch detta för`Not Supported`, hello följande:

Ersätt den här raden 
```XML
<OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub" />
```
med den här raden:
```XML
<OutputClaim ClaimTypeReferenceId="sub" />
```

## <a name="session-behavior-and-sso"></a>Sessionen fungerar och enkel inloggning
toochange din session beteende och konfigurationer för enkel inloggning måste tooadd en `<UserJourneyBehaviors>` -element inuti hello `<RelyingParty>` element.  Hej `<UserJourneyBehaviors>` element måste följa omedelbart efter hello `<DefaultUserJourney>`.  Hej inuti din `<UserJourneyBehavors>` element ska se ut så här:

```XML
<UserJourneyBehaviors>
   <SingleSignOn Scope="Application" />
   <SessionExpiryType>Absolute</SessionExpiryType>
   <SessionExpiryInSeconds>86400</SessionExpiryInSeconds>
</UserJourneyBehaviors>
```
**Enkel inloggning (SSO) konfiguration** toochange hello enkel inloggning konfiguration, behöver du toomodify hello värdet för `<SingleSignOn>`.  hello lämpliga värden är `Tenant`, `Application`, `Policy` och `Disabled`. 

**Webbprogrammet session-livstid (minuter)** toochange hello hello webbprogrammet sessioners livstid måste toomodify värdet för hello `<SessionExpiryInSeconds>` element.  hello standardvärdet i inbyggda principer är 86400 sekunder (1 440 minuter).

**Tidsgräns för Web app** toochange Sessionstidsgräns hello web app, behöver du toomodify hello värdet för `<SessionExpiryType>`.  hello lämpliga värden är `Absolute` och `Rolling`.
