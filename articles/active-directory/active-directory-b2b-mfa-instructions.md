---
title: "aaaConditional åtkomst för användare för Azure Active Directory B2B-samarbete | Microsoft Docs"
description: "Azure Active Directory B2B-samarbete stöder multifaktorautentisering (MFA) för selektiv åtkomst tooyour företagsprogram"
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 05/24/2017
ms.author: sasubram
ms.openlocfilehash: 3a05be4393f74ff8e87f32432a222a5fbac9af62
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="conditional-access-for-b2b-collaboration-users"></a>Villkorlig åtkomst för användare för B2B-samarbete

## <a name="multi-factor-authentication-for-b2b-users"></a>Multi-Factor authentication för B2B-användare
Organisationer kan tillämpa principer för multifaktorautentisering (MFA) för B2B-användare med Azure AD B2B-samarbete. Dessa principer kan tillämpas på hello klienten, app eller enskilda användarnivå hello samma sätt som de är aktiverade för heltidsanställda och medlemmar i hello organisation. MFA-principer tillämpas på hello resursorganisationen.

Exempel:
1. Administratören eller information worker i företag A uppmanar användare från företaget B tooan program *Foo* i företag A.
2. Programmet *Foo* i företag A är konfigurerade toorequire MFA på åtkomst.
3. När hello användare från företaget B försöker tooaccess app *Foo* hello företag en klientorganisation, de är och toocomplete MFA-kontrollen.
4. hello användaren kan ställa in sina MFA med företagets A och väljer alternativet sina MFA.
5. Det här scenariot fungerar för alla identitet (Azure AD eller MSA om användare i företaget B autentisera med sociala ID exempelvis)
6. Företag A måste ha tillräckligt många licenser för Premium Azure AD som stöder MFA. hello användare från företaget B använder denna licens från företag A.

hello bjuda in innehavare är alltid ansvarig för MFA för användare från partnerorganisationen hello, även om hello partnerorganisation har funktioner för MFA.

### <a name="setting-up-mfa-for-b2b-collaboration-users"></a>Konfigurera MFA för B2B-samarbete användare
toodiscover hur lätt det är tooset in MFA för B2B-samarbete användare kan se hur i hello följande video:

>[!VIDEO https://channel9.msdn.com/Blogs/Azure/b2b-conditional-access-setup/Player]

### <a name="b2b-users-mfa-experience-for-offer-redemption"></a>B2B användare MFA upplevelse i erbjuder inlösning
Kolla hello efter animering toosee hello inlösning upplevelse:

>[!VIDEO https://channel9.msdn.com/Blogs/Azure/MFA-redemption/Player]

### <a name="mfa-reset-for-b2b-collaboration-users"></a>MFA återställa för B2B-samarbete användare
För närvarande Hej administratör kan kräva B2B-samarbete användare tooproof in igen med hello följande PowerShell-cmdlets:

1. Ansluta tooAzure AD

  ```
  $cred = Get-Credential
  Connect-MsolService -Credential $cred
  ```
2. Hämta alla användare med bevis metoder

  ```
  Get-MsolUser | where { $_.StrongAuthenticationMethods} | select UserPrincipalName, @{n="Methods";e={($_.StrongAuthenticationMethods).MethodType}}
  ```
  Här är ett exempel:

  ```
  PS C:\Users\tjwasserGet-MsolUser | where { $_.StrongAuthenticationMethods} | select UserPrincipalName, @{n="Methods";e={($_.StrongAuthenticationMethods).MethodType}}
  ```

3. Återställa hello MFA-metoden för en specifik användare toorequire hello B2B-samarbete användaren tooset bevis upp metoder igen. Exempel:

  ```
  Reset-MsolStrongAuthenticationMethodByUpn -UserPrincipalName gsamoogle_gmail.com#EXT#@ WoodGroveAzureAD.onmicrosoft.com
  ```

### <a name="why-do-we-perform-mfa-at-hello-resource-tenancy"></a>Varför gör vi MFA vid hello resurs innehavare?

I hello aktuella versionen är MFA alltid i hello resurs innehavare av förutsägbarhet. Anta att till exempel en Contoso-användare (obegränsad) är inbjudna tooFabrikam och Fabrikam har aktiverat MFA för B2B-användare.

Om Contoso har aktiverats för App1 men inte App2 MFA-principen, kan sedan om vi tittar på hello Contoso MFA anspråk i hello token vi se hello följande problem:

* Dag 1: En användare har MFA i Contoso och har åtkomst till App1 och inga ytterligare MFA uppmaning visas i Fabrikam.

* Dag 2: hello användare har åtkomst till appen 2 i Contoso, nu att få åtkomst till Fabrikam, de måste registrera sig för MFA det.

Den här processen kan vara förvirrande och leda toodrop i inloggning slutföranden.

Dessutom även om Contoso MFA funktionen, är det inte alltid hello case hello Fabrikam skulle förtroende hello Contoso MFA-principen.

Slutligen fungerar resurs klient MFA även för MSA: er och sociala-ID: N och partner organisationer som behöver inte konfigurera MFA.

Därför är hello rekommendation för MFA för användare B2B tooalways kräva MFA i hello bjuda in klient. Det här kravet medför toodouble MFA i vissa fall, men när åtkomst till hello bjuda in klient hello slutanvändare upplevelse är förutsägbar: Lisa måste registrera sig för MFA med hello bjuda in innehavaren.

### <a name="device-based-location-based-and-risk-based-conditional-access-for-b2b-users"></a>Enhetsbaserad platsbaserad och risker-baserad villkorlig åtkomst för B2B-användare

När Contoso aktiverar enhetsbaserad villkorlig åtkomstvillkorspolicy till företagets data, förhindrade åtkomst från enheter som inte hanteras av Contoso och inte är kompatibla med principer för hello Contoso-enheter.

Om hello B2B användarens enhet inte hanteras av Contoso, blockeras åtkomsten av B2B användare från hello partnerorganisationer i oavsett kontexten principerna tillämpas. Contoso kan dock skapa undantag listor som innehåller viss partner användare tooexclude dem från hello enhetsbaserad villkorlig åtkomstprincip.

#### <a name="location-based-conditional-access-for-b2b"></a>Plats-baserad villkorlig åtkomst för B2B

Principer för plats-baserad villkorlig åtkomst tillämpas för B2B användare om hello bjuda in organisation kan toocreate ett tillförlitliga IP-adressintervall som definierar deras partnerorganisationer.

#### <a name="risk-based-conditional-access-for-b2b"></a>Risk-baserad villkorlig åtkomst för B2B

Risk-baserade inloggning principer kan för närvarande inte tillämpade tooB2B användare eftersom hello riskbedömning utförs på hello B2B användarens hemorganisation.

## <a name="next-steps"></a>Nästa steg

Läs andra artiklar om Azure AD B2B-samarbete:

* [Vad är Azure AD B2B-samarbete?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Hur lägger Azure Active Directory-administratörer till B2B-samarbete användare?](active-directory-b2b-admin-add-users.md)
* [Hur lägger informationsarbetare till B2B-samarbete användare?](active-directory-b2b-iw-add-users.md)
* [hello-element i hello e-postinbjudan för B2B-samarbete](active-directory-b2b-invitation-email.md)
* [B2B-samarbete inbjudan inlösning](active-directory-b2b-redemption-experience.md)
* [Azure AD B2B-samarbete och licensiering](active-directory-b2b-licensing.md)
* [Felsökning av Azure Active Directory B2B-samarbete](active-directory-b2b-troubleshooting.md)
* [Vanliga och frågor svar om Azure Active Directory B2B-samarbete](active-directory-b2b-faq.md)
* [Azure Active Directory B2B-samarbete API och anpassning](active-directory-b2b-api.md)
* [Lägg till B2B-samarbete användare utan inbjudan](active-directory-b2b-add-user-without-invite.md)
* [Artikelindex för programhantering i Azure Active Directory](active-directory-apps-index.md)
