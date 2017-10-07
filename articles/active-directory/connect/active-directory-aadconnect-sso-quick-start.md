---
title: "Azure AD Connect: Sömlös Single Sign-On - Snabbstart | Microsoft Docs"
description: "Den här artikeln beskriver hur tooget igång med Azure Active Directory sömlös enkel inloggning."
services: active-directory
keywords: "Vad är Azure AD Connect, installera Active Directory, nödvändiga komponenter för Azure AD, SSO, Single Sign-on"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: billmath
ms.openlocfilehash: 97d40ed41b3bfad9ff7e11ddbdb8de594ee85de1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-seamless-single-sign-on-quick-start"></a>Azure Active Directory sömlös enkel inloggning: Snabbstart

## <a name="how-toodeploy-seamless-sso"></a>Hur toodeploy sömlös SSO

Azure Active Directory sömlös enkel inloggning (Azure AD sömlös SSO) automatiskt loggar användarna när de är på företagets datorer anslutna tooyour företagets nätverk. Det ger dina användare enkel åtkomst tooyour molnbaserade program utan att behöva ytterligare lokala komponenter.

>[!IMPORTANT]
>hello sömlös SSO-funktionen är för närvarande under förhandsgranskning.

toodeploy sömlös SSO, behöver du toofollow de här stegen:

## <a name="step-1-check-prerequisites"></a>Steg 1: Kontrollera krav

Kontrollera att hello följande förutsättningar är uppfyllda:

1. Konfigurera Azure AD Connect-servern: Om du använder [direkt autentisering](active-directory-aadconnect-pass-through-authentication.md) som din inloggningsmetod, ingen ytterligare åtgärd krävs. Om du använder [synkronisering av lösenords-Hash](active-directory-aadconnectsync-implement-password-synchronization.md) som din inloggningsmetod, och om det finns en brandvägg mellan Azure AD Connect och Azure AD, kontrollera att:
- Du använder versioner 1.1.484.0 eller senare av Azure AD Connect.
- Azure AD Connect kan kommunicera med `*.msappproxy.net` URL: er och över port 443. Det här kravet gäller bara när du aktiverar hello-funktionen, inte för faktiska användarinloggningar.
- Azure AD Connect kan göra direkt IP-anslutningar toohello [Azure Datacenter IP-adressintervall](https://www.microsoft.com/download/details.aspx?id=41653). Det här kravet gäller igen, bara när du aktiverar hello-funktionen.
2. Du behöver inloggningsuppgifterna för domänadministratören för varje AD-skog som du synkroniserar tooAzure AD (med Azure AD Connect) och vars användare du vill ha tooenable sömlös SSO.

## <a name="step-2-enable-hello-feature"></a>Steg 2: Aktivera hello-funktionen

Sömlös SSO kan aktiveras med [Azure AD Connect](active-directory-aadconnect.md).

Om du gör en ren installation av Azure AD Connect väljer hello [anpassade installationssökvägen](active-directory-aadconnect-get-started-custom.md). Vid hello ”användare” inloggningssidan, kontrollera hello ”aktivera enkel inloggning” alternativet.

![Azure AD Connect - användarinloggning](./media/active-directory-aadconnect-sso/sso8.png)

Om du redan har en installation av Azure AD Connect, Välj ”Ändra användare inloggningssidan” på Azure AD Connect och klicka på ”Nästa”. Kontrollera sedan hello ”aktivera enkel inloggning” alternativet.

![Azure AD Connect - ändra användarens inloggning](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

Fortsätt hello guiden tills du kommer toohello ”aktivera enkel inloggning” sidan. Ange autentiseringsuppgifter för domänadministratör för varje AD-skog att du synkroniserar tooAzure AD (med Azure AD Connect) och vars användare du vill ha tooenable sömlös SSO. 

När du har slutfört guiden hello är sömlös SSO aktiverat på din klient.

>[!NOTE]
> hello domänadministratörsbehörighet lagras inte i Azure AD Connect eller i Azure AD, men det är bara använda tooenable hello-funktionen.

Följ dessa instruktioner tooverify att du har aktiverat sömlös SSO korrekt:

1. Logga in toohello [Azure Active Directory Administrationscenter](https://aad.portal.azure.com) med hello globala administratörsbehörigheter för din klient.
2. Välj **Azure Active Directory** på hello vänstra navigeringsfönstret.
3. Välj **Azure AD Connect**.
4. Kontrollera att hello **sömlös enkel inloggning** funktionen visar som **aktiverad**.

![Azure portal – Azure AD Connect-bladet](./media/active-directory-aadconnect-sso/sso10.png)

## <a name="step-3-roll-out-hello-feature"></a>Steg 3: Lansera hello-funktion

tooroll ut hello funktionen tooyour användare måste tooadd två Azure AD-URL: er (https://autologon.microsoftazuread-sso.com och https://aadg.windows.net.nsatc.net) toousers' intranät Zoninställningar via en Grupprincip i Active Directory.

>[!NOTE]
> hello följa instruktionerna bara arbetar för Internet Explorer och Google Chrome på Windows (om de har samma uppsättning URL: er för betrodda webbplatser i Internet Explorer). Läsa hello nästa avsnitt för instruktioner tooset Mozilla Firefox och Google Chrome på Mac.

### <a name="why-do-you-need-toomodify-users-intranet-zone-settings"></a>Varför behöver du toomodify användarnas zonen intranätsinställningar?

Som standard beräknas hello webbläsaren automatiskt hello rätt zon (Internet eller intranätet) från en URL. Http://contoso/ är till exempel mappade toohello intranätzonen http://intranet.contoso.com/ är mappade toohello zonen för Internet (eftersom hello URL innehåller en punkt). Webbläsare Skicka inte molnslutpunkt för Kerberos-biljetter tooa - som hello två Azure AD - om dess URL läggs explicit toohello webbläsarens intranätzonen.

### <a name="detailed-steps"></a>Detaljerade steg

1. Öppna hello verktyg för hantering av Grupprincip.
2. Redigera hello Grupprincip som är kopplade toosome eller alla användare. I det här exemplet använder vi hello **Standarddomänprincip**.
3. Navigera för**användaren Datorkonfiguration\Administrativa mallar\Windows-komponenter\Internet Explorer\Internet Panel\Security kontrollsida** och välj **plats tooZone Tilldelningslista**.
![Enkel inloggning](./media/active-directory-aadconnect-sso/sso6.png)  
4. Aktivera princip för hello och ange hello följande värden (Azure AD-URL: er som Kerberos-biljetter vidarebefordras) och data (*1* anger zonen intranät) hello i dialogrutan.

        Value: https://autologon.microsoftazuread-sso.com
        Data: 1
        Value: https://aadg.windows.net.nsatc.net
        Data: 1
>[!NOTE]
> Om du vill toodisallow vissa användare från att använda sömlös SSO - exempelvis om dessa användare loggar in på delade kiosker - ange hello föregående värden för*4*. Den här åtgärden lägger till hello Azure AD-URL: er toohello begränsad zon och misslyckas sömlös SSO alla hello tid.

5. Klicka på **OK** och **OK** igen.

![Enkel inloggning](./media/active-directory-aadconnect-sso/sso7.png)

### <a name="browser-considerations"></a>Överväganden för webbläsare

#### <a name="mozilla-firefox"></a>Mozilla Firefox

Mozilla Firefox inte automatiskt Kerberos-autentisering. Varje användare har toomanually lägga till hello Azure AD-URL: er tootheir Firefox inställningar med hjälp av hello följande steg:
1. Kör Firefox och ange `about:config` i hello adressfältet. Ignorera alla meddelanden som visas.
2. Sök efter hello **network.negotiate-auth.trusted-URI: er** inställningar. Den här inställningen visar Firefoxs betrodda platser för Kerberos-autentisering.
3. Högerklicka och välj ”Ändra”.
4. Ange ”https://autologon.microsoftazuread-sso.com, https://aadg.windows.net.nsatc.net” hello-fältet.
5. Klicka på ”OK” och öppna hello webbläsare.

#### <a name="safari-on-mac-os"></a>Safari på Mac OS

Se till att hello-dator som kör Mac OS är domänansluten tooAD. Se anvisningar för hur toodo som [här](http://training.apple.com/pdf/Best_Practices_for_Integrating_OS_X_with_Active_Directory.pdf).

#### <a name="google-chrome-on-mac-os"></a>Google Chrome på Mac OS

Google Chrome på Mac OS x och andra icke-Windows-plattformar finns för[i den här artikeln](https://dev.chromium.org/administrators/policy-list-3#AuthServerWhitelist) information om hur toowhitelist hello Azure AD-URL: er för integrerad autentisering.

Med hjälp av tredjeparts-Grupprincip i Active Directory tillägg tooroll hello Azure AD-URL: er tooFirefox och Google Chrome på Mac-användare som ligger utanför omfånget för den här artikeln.

#### <a name="known-limitations"></a>Kända begränsningar

Sömlös SSO fungerar inte i privat webbläsarens läge i Firefox och Edge-webbläsare. Den också fungerar inte på Internet Explorer om hello webbläsaren körs i läget för utökat skydd.

>[!IMPORTANT]
>Vi nyligen återställde stöd för Edge tooinvestigate kunden rapporterade problem.

## <a name="step-4-test-hello-feature"></a>Steg 4: Testa hello-funktion

tootest hello funktionen för en viss användare, se till att _alla_ hello följande villkor är uppfyllda:
  - hello användaren loggar in på företagets enheter.
  - hello enhet har varit tidigare kopplade tooyour Active Directory (AD)-domän.
  - hello-enhet har en direktanslutning tooyour domänkontrollanten (DC), antingen på hello företagets kabelanslutna eller trådlösa nätverk eller via en fjärranslutning, till exempel en VPN-anslutning.
  - Du har [distribuerat hello funktionen](##step-3-roll-out-the-feature) toothis användare med hjälp av en Grupprincip.

tootest hello scenario där hello användaren anger endast hello användarnamn, men inte hello lösenord:
- Logga in på *https://myapps.microsoft.com/* i en ny privat webbläsarsession.

tootest hello scenario där hello användaren inte har tooenter hello användarnamn eller hello lösenord: 
- Logga in på *https://myapps.microsoft.com/contoso.onmicrosoft.com* i en ny privat webbläsarsession. Ersätt ”*contoso*” med namnet på din klient.
- Eller logga in på *https://myapps.microsoft.com/contoso.com* i en ny privat webbläsarsession. Ersätt ”*contoso.com*” med en verifierad domän (inte en federerad domän) i din klient.

## <a name="step-5-roll-over-keys"></a>Steg 5: Återställ över nycklar

I steg 2 skapar Azure AD Connect datorkonton (som representerar Azure AD) i alla hello AD-skogar som du har aktiverat sömlös SSO. Läs mer i detalj [här](active-directory-aadconnect-sso-how-it-works.md). För förbättrad säkerhet bör du lanserar ofta över hello Kerberos dekrypteringsnycklar för dessa konton.

>[!IMPORTANT]
>Du behöver inte toodo det här steget _omedelbart_ när du har aktiverat hello-funktionen. Rulla över hello Kerberos dekrypteringsnycklar minst var 30 dagar.

## <a name="next-steps"></a>Nästa steg

- [**Tekniska ingående** ](active-directory-aadconnect-sso-how-it-works.md) -förstå hur funktionen fungerar.
- [**Vanliga frågor och svar** ](active-directory-aadconnect-sso-faq.md) -toofrequently frågor och svar.
- [**Felsöka** ](active-directory-aadconnect-troubleshoot-sso.md) – Lär dig hur tooresolve vanliga problem med hello-funktionen.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – för arkivering av nya funktioner som efterfrågas.
