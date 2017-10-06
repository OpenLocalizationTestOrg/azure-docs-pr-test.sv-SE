---
title: "Azure Active Directory B2C: Token, session och konfiguration för enkel inloggning | Microsoft Docs"
description: Token, session och enkel inloggning-konfiguration i Azure Active Directory B2C
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: e78e6344-0089-49bf-8c7b-5f634326f58c
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2017
ms.author: swkrish
ms.openlocfilehash: 63325ed97a7363723c97ee3a992046ebb5592662
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-token-session-and-single-sign-on-configuration"></a>Azure Active Directory B2C: Token, session och konfiguration för enkel inloggning
Den här funktionen ger dig finmaskig kontroll på en [per princip bas](active-directory-b2c-reference-policies.md), av:

1. Livslängd för säkerhetstoken som sänds av Azure Active Directory (AD Azure) B2C.
2. Livslängd för web application-sessioner som hanteras av Azure AD B2C.
3. Format för viktiga anspråk i hello säkerhetstoken som sänds av Azure AD B2C.
4. Enkel inloggning (SSO) beteendet över flera appar och principer i din B2C-klient.

Du kan använda den här funktionen i din B2C-klient på följande sätt:

1. Följ dessa steg för[navigera toohello B2C-funktionsbladet](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) på hello Azure-portalen.
2. Klicka på **inloggning principer**. *Obs: Du kan använda den här funktionen på alla principtypen inte bara på **inloggning principer***.
3. Öppna en princip genom att klicka på den. Klicka till exempel på **B2C_1_SiIn**.
4. Klicka på **redigera** hello överst i hello-bladet.
5. Klicka på **Token, session och enkel inloggning config**.
6. Gör ändringarna du. Läs mer om tillgängliga egenskaper i nedanstående avsnitt.
7. Klicka på **OK**.
8. Klicka på **spara** hello längst upp på bladet hello.

## <a name="token-lifetimes-configuration"></a>Livslängd för token-konfiguration
Azure AD B2C stöder hello [OAuth 2.0-protokollet för auktorisering](active-directory-b2c-reference-protocols.md) för att aktivera säker åtkomst tooprotected resurser. tooimplement detta stöd, Azure AD B2C avger olika [säkerhetstoken](active-directory-b2c-reference-tokens.md). Dessa är hello egenskaper som du kan använda toomanage livslängd av säkerhetstoken som sänds av Azure AD B2C:

* **Få åtkomst till & ID token livstid (minuter)**: hello livstid hello OAuth 2.0 ägar-token används toogain åtkomst tooa skyddad resurs. Azure AD B2C utfärdar ID-token just nu. Det här värdet används tooaccess token, när vi lägger till stöd för dessa.
  * Standard = 60 minuter.
  * Minsta (sammanlagt) = 5 minuter.
  * Maximalt (sammanlagt) = 1 440 minuter.
* **Uppdatera livslängd för token (dagar)**: hello maximala tidsperiod som en uppdateringstoken kan vara används tooacquire en ny åtkomst eller ID-token (och eventuellt en ny uppdateringstoken, om ditt program har beviljats hello `offline_access` omfattning).
  * Standard = 14 dagar.
  * Minsta (sammanlagt) = 1 dag.
  * Maximalt (sammanlagt) = 90 dagar.
* **Uppdatera livslängd för token glidande fönstret (dagar)**: när den här perioden är slut hello användare är framtvingad toore-autentisera oavsett hello giltighetsperioden för hello senaste uppdateringstoken som skapats av programmet hello. Det kan endast anges om hello växeln har angetts för**Bounded**. Måste toobe som är större än eller lika med toohello **uppdatering livslängd för token (dagar)** värde. Om hello växeln har angetts för**Unbounded**, du kan inte ange ett specifikt värde.
  * Standard = 90 dagar.
  * Minsta (sammanlagt) = 1 dag.
  * Maximalt (sammanlagt) = 365 dagar.

Det här är några användningsområden som kan aktiveras med hjälp av egenskaperna:

* Tillåt en användare toostay inloggad på obestämd tid ett mobilt program som han eller hon är kontinuerligt aktiv på hello program. Du kan göra detta genom att ange hello **uppdatering glidande fönstret livslängd för token (dagar)** växla för**Unbounded** i din inloggningsprincip.
* Uppfyller kraven för din bransch säkerhet och efterlevnad genom att ange hello lämplig åtkomst-token livslängd.

    > [!NOTE]
    > De här inställningarna är inte tillgängliga för principer för återställning av lösenord.
    > 
    > 

## <a name="token-compatibility-settings"></a>Token kompatibilitetsinställningar
Vi har gjort formatering ändringar tooimportant anspråk i säkerhetstoken som sänds av Azure AD B2C. Detta var klar tooimprove vår standardprotokoll support och för bättre samverkan med tredjeparts-identity-bibliotek. Dock tooavoid senaste befintliga appar, vi skapade hello följande egenskaper tooallow kunder tooopt-modulen efter behov:

* **Utfärdaren (iss) anspråk**: identifierar hello Azure AD B2C-klient som utfärdade hello-token.
  * `https://login.microsoftonline.com/{B2C tenant GUID}/v2.0/`: Det här är hello standardvärdet.
  * `https://login.microsoftonline.com/tfp/{B2C tenant GUID}/{Policy ID}/v2.0/`: Det här värdet omfattar ID: N för både hello B2C-klient och hello policy som används i begäran om hello-token. Om din app eller biblioteket måste Azure AD B2C toobe följer hello [OpenID Connect identifiering 1.0-specifikationen](http://openid.net/specs/openid-connect-discovery-1_0.html), Använd det här värdet.
* **Ämne (under) anspråk**: identifierar hello entitetstyper, d.v.s. hello användare för vilka hello Assert token information.
  * **ObjectID**: Detta är hello standardvärde. Hello objekt-ID för hello användare i hello directory fylls i hello `sub` anspråk i hello-token.
  * **Stöds inte**: det finns bara för bakåtkompatibilitet och vi rekommenderar att du växla för**ObjectID** så snart du har.
* **Anspråk som representerar princip-ID**: identifierar hello Anspråkstyp till vilka hello princip-ID som används i hello tokenbegäran fylls.
  * **tfp**: Detta är hello standardvärde.
  * **ACR**: det finns bara för bakåtkompatibilitet och vi rekommenderar att du växla för`tfp` som du kan.

## <a name="session-behavior"></a>Sessionen fungerar
Azure AD B2C stöder hello [autentiseringsprotokollet OpenID Connect](active-directory-b2c-reference-oidc.md) för att aktivera säker inloggning tooweb program. Hello-egenskaper som du kan använda toomanage web application sessioner är följande:

* **Webbprogrammet session-livstid (minuter)**: hello livslängden för Azure AD B2C sessions-cookie som lagras på hello användarens webbläsare vid autentiseringen.
  * Standard = 1 440 minuter.
  * Minsta (sammanlagt) = 15 minuter.
  * Maximalt (sammanlagt) = 1 440 minuter.
* **Tidsgräns för Web app**: om den här växeln anges för**absolut**, hello användaren är framtvingad toore-autentisera efter hello tidsperiod som anges av **webbprogrammet session-livstid (minuter)** långa. Om den här växeln anges för**rullande** (hello standardinställningen), hello användaren förblir inloggad så länge hello användaren är aktiv kontinuerligt i webbprogrammet.

Det här är några användningsområden som kan aktiveras med hjälp av egenskaperna:

* Uppfyller kraven för din bransch säkerhet och efterlevnad genom att ange hello lämpliga program webbsessionen livslängd.
* Tvinga autentiseras igen efter en angiven tidsperiod när användaren interagerar med en hög säkerhet som en del av ditt webbprogram. 

    > [!NOTE]
    > De här inställningarna är inte tillgängliga för principer för återställning av lösenord.
    > 
    > 

## <a name="single-sign-on-sso-configuration"></a>Enkel inloggning (SSO) konfiguration
Om du har flera program och principer i din B2C-klient kan du hantera användarinteraktioner mellan dem med hjälp av hello **konfiguration för enkel inloggning** egenskapen. Du kan ange hello egenskapen tooone av hello följande inställningar:

* **Klient**: det här är standardinställningen för hello. Med hjälp av den här inställningen kan flera program och principer i din B2C-klient tooshare hello samma användarsession. Till exempel när en användare loggar in på ett program, Contoso Shopping kan han eller hon kan också sömlöst logga in på en annan en, Contoso-apotek, vid åtkomst till den.
* **Programmet**: Detta kan du toomaintain en användarsession endast för ett program, oberoende av andra program. Till exempel om du vill att hello användaren toosign i tooContoso apotek (med hello samma autentiseringsuppgifter), även om han eller hon är redan inloggad Contoso Shopping ett annat program på hello samma B2C-klient. 
* **Principen**: Detta kan du toomaintain en användarsession endast för en princip, oberoende av hello-program som använder den. Till exempel om hello användaren har redan loggat in och slutfört steg en Multi-Factor authentication (MFA), kan han eller hon få åtkomst toohigher säkerhet delar av flera program så länge hello session knutna toohello principen inte upphör.
* **Inaktiverade**: Detta tvingar hello användaren toorun via hello hela användaren resa på varje körning av hello princip. Till exempel gör det att flera användare toosign in tooyour program (i en delad skrivbord scenariot), även medan en enskild användare är inloggad under hello hela tiden.

    > [!NOTE]
    > De här inställningarna är inte tillgängliga för principer för återställning av lösenord.
    > 
    > 

