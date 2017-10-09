---
title: "Azure Active Directory B2C: Förstå anpassade principer för hello startpaket | Microsoft Docs"
description: "Ett ämne på Azure Active Directory B2C anpassade principer"
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: rojasja
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/25/2017
ms.author: joroja
ms.openlocfilehash: 3484e8cc6fa6a9d57c2aa14c0cc9616065892d10
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="understanding-hello-custom-policies-of-hello-azure-ad-b2c-custom-policy-starter-pack"></a>Förstå hello anpassade principer för startpaket för hello Azure AD B2C-anpassad princip

Det här avsnittet innehåller alla hello grundelementen i hello B2C_1A_base princip som medföljer hello **startpaket** och som utnyttjas för att skapa dina egna principer via hello arv av hello *B2C_1A_base_ princip för tillägg*.

Därför det mer fokuserar särskilt på hello som redan har definierats anspråk typer, anspråksomvandlingar, definitioner av innehåll, Anspråksproviders med deras tekniska eller profilerna och hello core användaren resor.

> [!IMPORTANT]
> Microsoft lämnar inga garantier, uttryckliga eller underförstådda, avseende toohello information tillhandahålls nedan. Ändringar kan införas när som helst före GA tiden GA tidpunkt eller efter.

Både dina egna principer och hello B2C_1A_base_extensions princip kan åsidosätta dessa definitioner och utöka överordnade principen genom att tillhandahålla ytterligare dem efter behov.

Hej grundelementen i hello *B2C_1A_base princip* är anspråkstyper och anspråksomvandlingar innehåll definitioner. De här elementen kan mottagliga toobe som refereras i dina egna principer samt som hello *B2C_1A_base_extensions princip*.

## <a name="claims-schemas"></a>Anspråk scheman

Detta anspråk scheman är indelat i tre delar:

1.  Ett första avsnitt som visar hello minsta anspråk som krävs för hello användaren resor toowork korrekt.
2.  Ett andra avsnitt som visar hello anspråk som krävs för frågan string-parametrar och andra toobe särskilda parametrar skickades tooother Anspråksproviders, särskilt login.microsoftonline.com för autentisering. **Ändra inte de här anspråken**.
3.  Och slutligen ett tredje avsnitt som visar en lista över ytterligare och valfria anspråk som kan samlas in från hello användare lagras i hello directory skickas i token under inloggningen. Nya anspråk typen toobe samlas in från hello användare och/eller skickas i hello token kan läggas till i det här avsnittet.

> [!IMPORTANT]
> hello anspråk schemat innehåller begränsningar för vissa anspråk som lösenord och användarnamn. hello förtroende Framework (TF) princip behandlar Azure AD som andra anspråksprovider och dess begränsningar utformas i hello premium princip. En princip kunde ändrade tooadd fler begränsningar, eller Använd en annan anspråksprovider för lagring av autentiseringsuppgifter som har sin egen begränsningar.

hello tillgängliga anspråkstyper visas nedan.

### <a name="claims-that-are-required-for-hello-user-journeys"></a>Anspråk som krävs för hello användaren resor

Det krävs hello följande anspråk för användaren resor toowork korrekt:

| Typ av anspråk | Beskrivning |
|-------------|-------------|
| *Användar-ID* | Användarnamn |
| *signInName* | Logga in namn |
| *klient-ID* | Klient ID-Numret för hello användarobjekt i Azure AD B2C Premium |
| *objekt-ID* | Objekt-ID (ID) hello användarobjektet i Azure AD B2C Premium |
| *lösenord* | Lösenord |
| *nytt lösenord* | |
| *reenterPassword* | |
| *passwordPolicies* | Lösenordsprinciper som används av Azure AD B2C Premium toodetermine lösenordssäkerhet, upphör att gälla, osv. |
| *Sub* | |
| *alternativeSecurityId* | |
| *identityProvider* | |
| *Visningsnamn* | |
| *strongAuthenticationPhoneNumber* | Användarens telefonnummer |
| *Verified.strongAuthenticationPhoneNumber* | |
| *e-post* | E-postadress som kan vara används toocontact hello användare |
| *signInNamesInfo.emailAddress* | E-postadress som hello användare kan använda toosign i |
| *otherMails* | E-postadresser som kan vara används toocontact hello användare |
| *userPrincipalName* | Användarnamnet som lagras i hello Azure AD B2C Premium |
| *upnUserName* | Användarnamn för att skapa användarens huvudnamn |
| *mailNickName* | Användarens e-smeknamn som lagras i hello Azure AD B2C Premium |
| *ny användare* | |
| *köra SelfAsserted-inmatning* | Anspråk som anger om attribut samlades in från hello användare |
| *köra PhoneFactor-inmatning* | Anspråk som anger om ett nytt telefonnummer samlats in från hello användare |
| *authenticationSource* | Anger om hello användaren autentiserades på sociala identitetsleverantör, login.microsoftonline.com eller lokalt konto |

### <a name="claims-required-for-query-string-parameters-and-other-special-parameters"></a>Anspråk som krävs för frågan string-parametrar och andra särskilda parametrar

hello är följande anspråk obligatoriska toopass på särskilda parametrar (inklusive vissa frågeparametrar sträng) tooother Anspråksproviders:

| Typ av anspråk | Beskrivning |
|-------------|-------------|
| *nux* | Särskilda parameter som skickades för lokalt konto autentisering toologin.microsoftonline.com |
| *NCA* | Särskilda parameter som skickades för lokalt konto autentisering toologin.microsoftonline.com |
| *kommandotolk* | Särskilda parameter som skickades för lokalt konto autentisering toologin.microsoftonline.com |
| *mkt* | Särskilda parameter som skickades för lokalt konto autentisering toologin.microsoftonline.com |
| *LC* | Särskilda parameter som skickades för lokalt konto autentisering toologin.microsoftonline.com |
| *grant_type* | Särskilda parameter som skickades för lokalt konto autentisering toologin.microsoftonline.com |
| *omfång* | Särskilda parameter som skickades för lokalt konto autentisering toologin.microsoftonline.com |
| *client_id* | Särskilda parameter som skickades för lokalt konto autentisering toologin.microsoftonline.com |
| *objectIdFromSession* | Parametern som angetts av hello standard session management provider tooindicate som hello objekt-id har hämtats från en SSO-session |
| *isActiveMFASession* | Parametern som tillhandahålls av hello MFA session management tooindicate hello användaren har en aktiv session MFA |

### <a name="additional-optional-claims-that-can-be-collected"></a>Ytterligare (valfritt) anspråk som kan samlas in

hello följande anspråk ytterligare anspråk som kan samlas in från hello användare lagras i hello directory, och skickas i hello-token. Enligt innan kan ytterligare anspråk läggas till toothis lista.

| Typ av anspråk | Beskrivning |
|-------------|-------------|
| *givenName* | Användarens förnamn (även kallat Förnamn) |
| *Efternamn* | Användarens efternamn (även kallat namn eller efternamn) |
| *Extension_picture* | Användarens bild från sociala |

## <a name="claim-transformations"></a>Anspråksomvandlingar

hello tillgängliga anspråksomvandlingar visas nedan.

| Anspråksomvandling av | Beskrivning |
|----------------------|-------------|
| *CreateOtherMailsFromEmail* | |
| *CreateRandomUPNUserName* | |
| *CreateUserPrincipalName* | |
| *CreateSubjectClaimFromObjectID* | |
| *CreateSubjectClaimFromAlternativeSecurityId* | |
| *CreateAlternativeSecurityId* | |

## <a name="content-definitions"></a>Definitioner för innehåll

Det här avsnittet beskrivs hello innehåll definitioner har redan deklarerats i hello *B2C_1A_base* princip. Dessa definitioner av innehåll är mottagliga toobe refererar till, åsidosätts och/eller utökad som behövs i dina egna principer samt som hello *B2C_1A_base_extensions* princip.

| Anspråksleverantör | Beskrivning |
|-----------------|-------------|
| *Facebook* | |
| *Logga in lokalt konto* | |
| *PhoneFactor* | |
| *Azure Active Directory* | |
| *Self vars* | |
| *Lokalt konto* | |
| *Sessionshantering* | |
| *Trustframework principmodulen* | |
| *TechnicalProfiles* | |
| *Token utfärdare* | |

## <a name="technical-profiles"></a>Tekniska profiler

Det här avsnittet visar hello tekniska profiler har redan deklarerats per anspråksleverantör i hello *B2C_1A_base* princip. Dessa tekniska profiler är mottagliga toobe ytterligare refererar till, åsidosätts och/eller utökad som behövs i dina egna principer samt som hello *B2C_1A_base_extensions* princip.

### <a name="technical-profiles-for-facebook"></a>Tekniska profiler för Facebook

| Tekniska profil | Beskrivning |
|-------------------|-------------|
| *Facebook-OAUTH* | |

### <a name="technical-profiles-for-local-account-signin"></a>Tekniska profiler för lokal inloggning för kontot

| Tekniska profil | Beskrivning |
|-------------------|-------------|
| *Logga in utan interaktivitet* | |

### <a name="technical-profiles-for-phone-factor"></a>Tekniska profiler för Phonefactor

| Tekniska profil | Beskrivning |
|-------------------|-------------|
| *PhoneFactor-indata* | |
| *PhoneFactor-InputOrVerify* | |
| *PhoneFactor-verifiera* | |

### <a name="technical-profiles-for-azure-active-directory"></a>Tekniska profiler för Azure Active Directory

| Tekniska profil | Beskrivning |
|-------------------|-------------|
| *AAD-gemensamma* | Tekniska profil inkluderas genom hello andra tekniska AAD-xxx-profiler |
| *AAD-UserWriteUsingAlternativeSecurityId* | Tekniska profil för sociala inloggningar |
| *AAD-UserReadUsingAlternativeSecurityId* | Tekniska profil för sociala inloggningar |
| *AAD-UserReadUsingAlternativeSecurityId-NoError* | Tekniska profil för sociala inloggningar |
| *AAD-UserWritePasswordUsingLogonEmail* | Tekniska profil för lokala konton |
| *AAD-UserReadUsingEmailAddress* | Tekniska profil för lokala konton |
| *AAD-UserWriteProfileUsingObjectId* | Tekniska profil för att uppdatera användarpost med objekt-ID |
| *AAD-UserWritePhoneNumberUsingObjectId* | Tekniska profil för att uppdatera användarpost med objekt-ID |
| *AAD-UserWritePasswordUsingObjectId* | Tekniska profil för att uppdatera användarpost med objekt-ID |
| *AAD-UserReadUsingObjectId* | Tekniska profil är används tooread data när användaren autentiseras |

### <a name="technical-profiles-for-self-asserted"></a>Tekniska profiler för Self vars

| Tekniska profil | Beskrivning |
|-------------------|-------------|
| *SelfAsserted Social* | |
| *SelfAsserted ProfileUpdate* | |

### <a name="technical-profiles-for-local-account"></a>Tekniska profiler för lokala konton

| Tekniska profil | Beskrivning |
|-------------------|-------------|
| *LocalAccountSignUpWithLogonEmail* | |

### <a name="technical-profiles-for-session-management"></a>Tekniska profiler för sessionshantering

| Tekniska profil | Beskrivning |
|-------------------|-------------|
| *SM-Noop* | |
| *SM-AAD* | |
| *SM-SocialSignup* | Profilnamnet som används toodisambiguate AAD session mellan logga in och logga in |
| *SM-SocialLogin* | |
| *SM-MFA* | |

### <a name="technical-profiles-for-trustframework-policy-engine-technicalprofiles"></a>Tekniska profiler för Trustframework princip motorn TechnicalProfiles

För närvarande inga tekniska profiler definieras för hello **Trustframework princip motorn TechnicalProfiles** anspråksleverantör.

### <a name="technical-profiles-for-token-issuer"></a>Tekniska profiler för tokenutfärdare

| Tekniska profil | Beskrivning |
|-------------------|-------------|
| *JwtIssuer* | |

## <a name="user-journeys"></a>Användaren resor

Det här avsnittet visar hello användaren resor redan deklarerats i hello *B2C_1A_base* princip. Dessa användare resor är mottagliga toobe ytterligare refererar till, åsidosätts och/eller utökad som behövs i dina egna principer samt som hello *B2C_1A_base_extensions* princip.

| Användaren resa | Beskrivning |
|--------------|-------------|
| *Registreringen* | |
| *Logga in* | |
| *SignUpOrSignIn* | |
| *EditProfile* | |
| *PasswordReset* | |
