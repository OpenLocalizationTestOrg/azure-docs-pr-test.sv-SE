---
title: "Vanliga frågor (FAQ) - Azure AD B2C | Microsoft Docs"
description: "Vanliga frågor och svar om Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: saeeda
manager: krassk
editor: bryanla
ms.assetid: ed33c2ca-76d0-442a-abb1-8b7b7bb92d6a
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2017
ms.author: saeeda
ms.openlocfilehash: f7857299bc3cb9d5fbe58e047818ec56741e0740
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-frequently-asked-questions-faq"></a>Azure AD B2C: Vanliga frågor (FAQ) 
Den här sidan svar på vanliga frågor om hello Azure Active Directory (AD Azure) B2C. Hålla kontroll för uppdateringar.

### <a name="can-i-use-azure-ad-b2c-features-in-my-existing-employee-based-azure-ad-tenant"></a>Kan jag använda Azure AD B2C-funktioner i Mina befintliga, medarbetare-baserad Azure AD-klient?
Azure AD och Azure AD B2C separat produkterbjudanden och kan inte finnas i hello samma klient.  En Azure AD-klient representerar en organisation.  En Azure AD B2C-klient representerar en samling av identiteter toobe används med förlitande parters program.  Azure AD B2C kan federera tooAzure AD möjliggör autentisering av anställda i en organisation med anpassade principer (i förhandsversion).

### <a name="can-i-use-azure-ad-b2c-tooprovide-social-login-facebook-and-google-into-office-365"></a>Kan jag använda Azure AD B2C tooprovide inloggning via sociala medier (Facebook och Google +) i Office 365?
Azure AD B2C måste använda tooauthenticate användare för Microsoft Office 365.  Azure AD är Microsofts lösning för hantering av medarbetare åtkomst tooSaaS appar och funktioner som är utformade för detta ändamål, till exempel licensiering och villkorlig åtkomst.  Azure AD B2C innehåller en identitets- och plattform för att skapa webb- och mobilprogram.  När Azure AD B2C är konfigurerade toofederate tooan Azure AD-klient, hanterar hello Azure AD-klient medarbetare åtkomst tooapplications som förlitar sig på Azure AD B2C.

### <a name="what-are-local-accounts-in-azure-ad-b2c-how-are-they-different-from-work-or-school-accounts-in-azure-ad"></a>Vad är lokala konton i Azure AD B2C? Vad är de från arbets-eller skolkonton i Azure AD?
I en Azure AD-klient användare som tillhör toohello klient logga in med en e-postadress i formatet hello `<xyz>@<tenant domain>`.  Hej `<tenant domain>` verifieras en hello domäner i hello klient eller hello inledande `<...>.onmicrosoft.com` domän. Den här typen av konto är ett konto för arbetet eller skolan.

I en Azure AD B2C-klient vill de flesta appar hello användaren toosign i med en godtycklig e-postadress (till exempel joe@comcast.net, bob@gmail.com, sarah@contoso.com, eller jim@live.com). Den här typen av konto är ett lokalt konto.  Vi stöder också godtycklig användarnamn som lokala konton (till exempel joe bob, Sara eller jim). Du kan välja något av dessa två typer av lokalt konto genom att konfigurera Azure AD B2C hello Azure-portalen.

### <a name="which-social-identity-providers-do-you-support-now-which-ones-do-you-plan-toosupport-in-hello-future"></a>Vilka sociala identitetsleverantörer du stöder nu? Vilka som vill du planera toosupport i hello framtida?
Vi stöder för närvarande Facebook, Google +, LinkedIn, Amazon, Twitter (förhandsgranskning), WeChat (förhandsgranskning), Weibo (förhandsversion) och QT (förhandsversion). Vi lägger till stöd för andra populära sociala identitetsleverantörer baserat på kundernas behov.

Azure AD B2C också har lagt till stöd för [anpassade principer](https://docs.microsoft.com/en-us/azure/active-directory-b2c/active-directory-b2c-overview-custom).  Dessa [anpassade principer](https://docs.microsoft.com/en-us/azure/active-directory-b2c/active-directory-b2c-overview-custom) kan en utvecklare toocreate sina egna princip att med eventuella identitetsprovider som stöder [OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html) eller SAML. 

Kom igång med anpassade principer genom att checka ut våra [anpassad princip startpaket](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack).

### <a name="can-i-configure-scopes-toogather-more-information-about-consumers-from-various-social-identity-providers"></a>Kan jag konfigurera scope toogather mer information om användare från olika sociala identitetsleverantörer?
Nej, men den här funktionen finns på vår översikt. hello standard omfattningar som används för våra uppsättning sociala identitetsleverantörer som stöds är:

* Facebook: e-post
* Google +: e-post
* Microsoft-konto: openid e-postprofil
* Amazon: profil
* LinkedIn: r_emailaddress, r_basicprofile

### <a name="does-my-application-have-toobe-run-on-azure-for-it-work-with-azure-ad-b2c"></a>Har mitt program toobe som körs på Azure för den fungerar med Azure AD B2C?
Nej, kan du värd för ditt program var som helst (i hello molnet eller lokalt). Allt som behövs för toointeract med Azure AD B2C är hello möjlighet toosend och ta emot HTTP-förfrågningar på offentligt tillgänglig slutpunkter.

### <a name="i-have-multiple-azure-ad-b2c-tenants-how-can-i-manage-them-on-hello-azure-portal"></a>Jag har flera innehavare av Azure AD B2C. Hur kan jag hantera dem på hello Azure-portalen?
Innan du öppnar 'Azure AD B2C' hello vänster-menyn i hello Azure-portalen, måste du växla till hello directory du vill toomanage.  Växla kataloger genom att klicka på din identitet i hello övre högra hörnet på hello Azure-portalen och sedan välja en katalog i hello listruta som visas.  En stegvis med bilder, finns i [navigerar tooAzure AD B2C inställningar](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).

### <a name="how-do-i-customize-verification-emails-hello-content-and-hello-from-field-sent-by-azure-ad-b2c"></a>Hur anpassa bekräftelsemeddelanden (hello innehåll och hello ”från”: fältet) skickas av Azure AD B2C?
Du kan använda hello [funktionen för företagsanpassning](../active-directory/active-directory-add-company-branding.md) toocustomize hello innehållet i e-post för verifiering. Mer specifikt kan du anpassa dessa två element av hello e-post:

* **Banderoll logotypen**: visas vid hello längst ned till höger.
* **Bakgrundsfärg**: visas överst hello.

    ![Skärmbild av en anpassad e-postmeddelandet](./media/active-directory-b2c-faqs/company-branded-verification-email.png)

hello e-signaturen innehåller hello B2C innehavarens namn som du angav när du först skapade hello B2C-klient. Du kan ändra hello namn med hjälp av dessa anvisningar:

1. Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com/) som Hej administratör för prenumeration.
1. Navigera tooyour B2C-klient.
1. Klicka på hello **konfigurera** fliken.
1. Ändra hello **namn** fältet under hello **katalogegenskaper** avsnitt.
1. Klicka på **spara** på hello hello sidans nederkant.

Det finns för närvarande inget sätt toochange hello ”från”: på hello e-post. Rösta på [feedback.azure.com](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/15334335-fully-customizable-verification-emails) du är intresserad av att anpassa hello brödtext hello e-postmeddelandet.

### <a name="how-can-i-migrate-my-existing-user-names-passwords-and-profiles-from-my-database-tooazure-ad-b2c"></a>Hur kan jag migrera Mina befintliga användarnamn, lösenord och profiler från min databasen tooAzure AD B2C?
Du kan använda hello Azure AD Graph API toowrite Migreringsverktyget för. Se hello [Graph API-exemplet](active-directory-b2c-devquickstarts-graph-dotnet.md) mer information.

### <a name="what-password-policy-is-used-for-local-accounts-in-azure-ad-b2c"></a>Vilka lösenordsprincip används för lokala konton i Azure AD B2C?
hello Azure AD B2C lösenordsprincip för lokala konton är baserad på hello princip för Azure AD. Azure AD B2C s registrering, registrering eller inloggning och lösenord Återställ principer använder hello ”starka” lösenordssäkerhet och inte ut eventuella lösenord. Läs hello [Azure AD-lösenordsprincip](https://msdn.microsoft.com/library/azure/jj943764.aspx) för mer information.

### <a name="can-i-use-azure-ad-connect-toomigrate-consumer-identities-that-are-stored-on-my-on-premises-active-directory-tooazure-ad-b2c"></a>Kan jag använda Azure AD Connect toomigrate konsumenten identiteter som lagras på min lokala Active Directory tooAzure AD B2C?
Nej, Azure AD Connect är inte utformad toowork med Azure AD B2C. Överväg att använda hello [Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md) för användarmigrering.

### <a name="can-my-app-open-up-azure-ad-b2c-pages-within-an-iframe"></a>Öppnar min app in Azure AD B2C sidor i en iFrame?
Nej, av säkerhetsskäl, Azure AD B2C-sidor kan inte öppnas en iFrame.  Vår tjänst kommunicerar med hello webbläsare tooprohibit iFrames.  Hej säkerhetsgrupp i allmänhet och hello OAUTH2-specifikationen, rekommenderar mot med iFrames för identitetsupplevelser på grund av toohello risken för Klicka fästpunkter.

### <a name="does-azure-ad-b2c-work-with-crm-systems-such-as-microsoft-dynamics"></a>Fungerar Azure AD B2C med till exempel Microsoft Dynamics CRM-system?
Integrering med Microsoft Dynamics 365-portalen är tillgänglig.  Se [konfigurerar Dynamics 365-portalen toouse Azure AD B2C för autentisering](https://docs.microsoft.com/en-us/dynamics365/customer-engagement/portals/azure-ad-b2c).

### <a name="does-azure-ad-b2c-work-with-sharepoint-on-premises-2016-or-earlier"></a>Har Azure AD B2C fungerar med SharePoint lokalt 2016 eller tidigare?
Azure AD B2C är inte avsedd för hello SharePoint extern delning av partner scenariot; Se [Azure AD B2B](http://blogs.technet.com/b/ad/archive/2015/09/15/learn-all-about-the-azure-ad-b2b-collaboration-preview.aspx) i stället.

### <a name="should-i-use-azure-ad-b2c-or-b2b-toomanage-external-identities"></a>Bör jag använda Azure AD B2C eller B2B toomanage externa identiteter?
Den här artikeln om [externa identiteter](../active-directory/active-directory-b2b-compare-external-identities.md) toolearn information hur du använder hello lämpliga funktioner tooyour externa identitet scenarier.

### <a name="what-reporting-and-auditing-features-does-azure-ad-b2c-provide-are-they-hello-same-as-in-azure-ad-premium"></a>Vilka rapportering och granskning funktioner Azure AD B2C ger? Är de hello samma som i Azure AD Premium?
Nej, stöder inte Azure AD B2C hello samma uppsättning rapporter som Azure AD Premium. Det finns emellertid många gemensamma utformning gör:

* hello inloggning rapporter tillhandahåller en post för varje inloggning med nedsatt information.
* Granskningsrapporter är tillgängliga i hello Azure-portalen, Azure Active Directory > aktivitet granskningsloggar > Välj B2C och tillämpa filter som du vill. Både administratörsaktivitet samt programaktivitet omfattas. 
* En användningsrapport som omfattar antalet användare, antal inloggningar och volymen av MFA är tillgänglig på [användning Reporting API](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-usage-reporting-api)

### <a name="can-i-localize-hello-ui-of-pages-served-by-azure-ad-b2c-what-languages-are-supported"></a>Kan jag lokalisera hello UI sidor som hanteras av Azure AD B2C? Vilka språk som stöds?
Visst!  Läs mer om [språk anpassning](active-directory-b2c-reference-language-customization.md), som är tillgänglig som förhandsversion.  Vi ger översättningar för 36 språk och du kan åsidosätta eventuella sträng toosuit dina behov.

### <a name="can-i-use-my-own-urls-on-my-sign-up-and-sign-in-pages-that-are-served-by-azure-ad-b2c-for-instance-can-i-change-hello-url-from-loginmicrosoftonlinecom-toologincontosocom"></a>Kan jag använda min egen URL: er på sidorna registrering och inloggning som hanteras av Azure AD B2C? Till exempel ändrar hello URL från login.microsoftonline.com toologin.contoso.com?
För närvarande inte. Den här funktionen finns på vår översikt. Verifiera din domän i hello **domäner** fliken på hello klassiska Azure-portalen inte utför det här målet.

### <a name="how-do-i-delete-my-azure-ad-b2c-tenant"></a>Hur tar jag bort min Azure AD B2C-klient?
Följ dessa steg toodelete din Azure AD B2C-klient:

1. Följ dessa steg för[navigerar tooAzure AD B2C inställningar](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) på hello Azure-portalen.
1. Navigera toohello **program**, **identitetsleverantörer**, och **alla principer** och ta bort alla hello poster i var och en av dem.
1. Nu logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com/) som Hej administratör för prenumeration. (Använd hello samma arbets- eller skolkonto eller hello samma Microsoft-konto som du använde toosign för Azure.)
1. Navigera toohello Active Directory-tillägget hello vänster och klicka på din B2C-klient.
1. Klicka på hello **användare** fliken.
1. Markera varje användare i sin tur (utelämna Hej administratör för prenumeration användare för närvarande är inloggad som). Klicka på **ta bort** längst hello hello sidan och klicka på **Ja** när du tillfrågas.
1. Klicka på hello **program** fliken.
1. Välj **program som företaget äger** i hello **visa** listrutan och klicka på hello är markerat.
1. Ett program kallas **b2c-tillägg-app**. Klicka på **ta bort** längst hello hello sidan och klicka på **Ja** när du tillfrågas.
1. Navigera toohello Active Directory-tillägget igen och välj din B2C-klient.
1. Klicka på **ta bort** på hello hello sidans nederkant. toocomplete hello processen hello anvisningar på hello-skärmen.

### <a name="can-i-get-azure-ad-b2c-as-part-of-enterprise-mobility-suite"></a>Kan jag få Azure AD B2C som en del av Enterprise Mobility Suite?
Nej, Azure AD B2C är en betala per användning i Azure-tjänsten och är inte en del av Enterprise Mobility Suite.

### <a name="how-do-i-report-issues-with-azure-ad-b2c"></a>Hur rapporterar problem med Azure AD B2C?
Se [filbegäranden stöd för Azure Active Directory B2C](active-directory-b2c-support.md).
