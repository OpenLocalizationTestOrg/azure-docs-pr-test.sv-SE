---
title: "Azure AD Connect: Sömlös Single Sign-On - Snabbstart | Microsoft Docs"
description: "Den här artikeln beskriver hur du kommer igång med Azure Active Directory sömlös enkel inloggning."
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
ms.openlocfilehash: 977108687734a5eb7f7a30419de2a6bdef184d0e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-seamless-single-sign-on-quick-start"></a>Azure Active Directory sömlös enkel inloggning: Snabbstart

## <a name="how-to-deploy-seamless-sso"></a>Så här distribuerar du sömlös SSO

Azure Active Directory sömlös enkel inloggning (Azure AD sömlös SSO) automatiskt loggar användarna när de är på sina företagets datorer som är anslutna till företagsnätverket. Det ger dina användare enkel åtkomst till dina molnbaserade program utan att behöva ytterligare lokala komponenter.

>[!IMPORTANT]
>Sömlös SSO-funktionen är för närvarande under förhandsgranskning.

Om du vill distribuera sömlös SSO, måste du följa dessa steg:

## <a name="step-1-check-prerequisites"></a>Steg 1: Kontrollera krav

Se till att följande krav är uppfyllda:

1. Konfigurera Azure AD Connect-servern: Om du använder [direkt autentisering](active-directory-aadconnect-pass-through-authentication.md) som din inloggningsmetod, ingen ytterligare åtgärd krävs. Om du använder [synkronisering av lösenords-Hash](active-directory-aadconnectsync-implement-password-synchronization.md) som din inloggningsmetod, och om det finns en brandvägg mellan Azure AD Connect och Azure AD, kontrollera att:
- Du använder versioner 1.1.484.0 eller senare av Azure AD Connect.
- Azure AD Connect kan kommunicera med `*.msappproxy.net` URL: er och över port 443. Det här kravet gäller bara när du aktiverar funktionen, inte för faktiska användarinloggningar.
- Azure AD Connect kan göra direkta IP-anslutningar till den [Azure Datacenter IP-adressintervall](https://www.microsoft.com/download/details.aspx?id=41653). Det här kravet gäller igen, bara när du aktiverar funktionen.
2. Du behöver inloggningsuppgifterna för domänadministratören för varje AD-skog som du synkroniserar till Azure AD (med Azure AD Connect) och vars användare du vill aktivera sömlös SSO.

## <a name="step-2-enable-the-feature"></a>Steg 2: Aktivera funktionen

Sömlös SSO kan aktiveras med [Azure AD Connect](active-directory-aadconnect.md).

Om du gör en ren installation av Azure AD Connect, väljer du den [anpassade installationssökvägen](active-directory-aadconnect-get-started-custom.md). Markera alternativet ”Aktivera enkel inloggning på” på sidan ”användarens inloggning”.

![Azure AD Connect - användarinloggning](./media/active-directory-aadconnect-sso/sso8.png)

Om du redan har en installation av Azure AD Connect, Välj ”Ändra användare inloggningssidan” på Azure AD Connect och klicka på ”Nästa”. Kontrollera sedan alternativet ”Aktivera enkel inloggning på”.

![Azure AD Connect - ändra användarens inloggning](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

Fortsätt genom guiden tills du kommer till sidan ”Aktivera enkel inloggning på”. Ange autentiseringsuppgifter för domänadministratör för varje AD-skog som du synkroniserar till Azure AD (med Azure AD Connect) och vars användare du vill aktivera sömlös SSO. 

När guiden har slutförts, är sömlös SSO aktiverat på din klient.

>[!NOTE]
> Domänadministratören autentiseringsuppgifterna lagras inte i Azure AD Connect eller i Azure AD, men endast används för att aktivera funktionen.

Följ instruktionerna för att kontrollera att du har aktiverat sömlös SSO korrekt:

1. Logga in på den [Azure Active Directory Administrationscenter](https://aad.portal.azure.com) med Global administratörsbehörighet för din klient.
2. Välj **Azure Active Directory** i navigeringen till vänster.
3. Välj **Azure AD Connect**.
4. Kontrollera att den **sömlös enkel inloggning** funktionen visar som **aktiverad**.

![Azure portal – Azure AD Connect-bladet](./media/active-directory-aadconnect-sso/sso10.png)

## <a name="step-3-roll-out-the-feature"></a>Steg 3: Lansera funktionen

För att lansera funktionen till användarna, måste du lägga till två Azure AD-URL: er (https://autologon.microsoftazuread-sso.com och https://aadg.windows.net.nsatc.net) till användarnas intranät Zoninställningar via en Grupprincip i Active Directory.

>[!NOTE]
> Följande instruktioner fungerar endast för Internet Explorer och Google Chrome på Windows (om de har samma uppsättning URL: er för betrodda webbplatser i Internet Explorer). Läs avsnittet om hur du ställer in Mozilla Firefox och Google Chrome på Mac.

### <a name="why-do-you-need-to-modify-users-intranet-zone-settings"></a>Varför behöver du ändra användarnas zonen intranätsinställningar?

Som standard beräknas webbläsaren automatiskt rätt zonen (Internet eller intranätet) från en URL. Till exempel mappas http://contoso/ till zonen Intranät, medan http://intranet.contoso.com/ mappas till zonen Internet (eftersom den innehåller en punkt). Webbläsare Skicka inte Kerberos-biljetter till en molnslutpunkt, till exempel två Azure AD URL: er – om URL: en explicit har lagts till i webbläsarens intranätzonen.

### <a name="detailed-steps"></a>Detaljerade steg

1. Öppna verktyget hantering av Grupprincip.
2. Redigera en grupprincip som tillämpas på vissa eller alla användare. I det här exemplet använder vi den **Standarddomänprincip**.
3. Gå till **användaren Datorkonfiguration\Administrativa mallar\Windows-komponenter\Internet Explorer\Internet Panel\Security kontrollsida** och välj **plats till zon Tilldelningslista**.
![Enkel inloggning](./media/active-directory-aadconnect-sso/sso6.png)  
4. Aktivera principen och ange följande värden (Azure AD-URL: er som Kerberos-biljetter vidarebefordras) och data (*1* anger zonen intranät) i dialogrutan.

        Value: https://autologon.microsoftazuread-sso.com
        Data: 1
        Value: https://aadg.windows.net.nsatc.net
        Data: 1
>[!NOTE]
> Om du vill hindra vissa användare från att använda sömlös SSO - exempelvis om dessa användare loggar in på delade kiosker - värdet föregående värden *4*. Den här åtgärden lägger till URL: er för Azure AD för zonen Ej tillförlitliga och misslyckas sömlös SSO hela tiden.

5. Klicka på **OK** och **OK** igen.

![Enkel inloggning](./media/active-directory-aadconnect-sso/sso7.png)

### <a name="browser-considerations"></a>Överväganden för webbläsare

#### <a name="mozilla-firefox"></a>Mozilla Firefox

Mozilla Firefox inte automatiskt Kerberos-autentisering. Varje användare har manuellt lägga till URL: er för Azure AD i sina Firefox-inställningar med hjälp av följande steg:
1. Kör Firefox och ange `about:config` i adressfältet. Ignorera alla meddelanden som visas.
2. Sök efter den **network.negotiate-auth.trusted-URI: er** inställningar. Den här inställningen visar Firefoxs betrodda platser för Kerberos-autentisering.
3. Högerklicka och välj ”Ändra”.
4. Ange ”https://autologon.microsoftazuread-sso.com, https://aadg.windows.net.nsatc.net” i fältet.
5. Klicka på ”OK” och öppna webbläsaren.

#### <a name="safari-on-mac-os"></a>Safari på Mac OS

Se till att datorn som kör Mac OS är ansluten till AD. Instruktioner om hur du gör finns [här](http://training.apple.com/pdf/Best_Practices_for_Integrating_OS_X_with_Active_Directory.pdf).

#### <a name="google-chrome-on-mac-os"></a>Google Chrome på Mac OS

Google Chrome på Mac OS x och andra icke-Windows-plattformar finns i [i den här artikeln](https://dev.chromium.org/administrators/policy-list-3#AuthServerWhitelist) information om hur du godkända Azure AD-URL: er för integrerad autentisering.

Med hjälp av Grupprincip i Active Directory-tredjepartstillägg lansera URL: er för Azure AD Firefox och Google Chrome på Mac-användare ligger utanför omfånget för den här artikeln.

#### <a name="known-limitations"></a>Kända begränsningar

Sömlös SSO fungerar inte i privat webbläsarens läge i Firefox och Edge-webbläsare. Den också fungerar inte på Internet Explorer om webbläsaren körs i läget för utökat skydd.

>[!IMPORTANT]
>Vi nyligen återställde stöd för kant för att undersöka problem som rapporteras av kunden.

## <a name="step-4-test-the-feature"></a>Steg 4: Testa funktionen

Om du vill testa funktionen för en viss användare, se till att _alla_ följande villkor är uppfyllda:
  - Användaren loggar in på företagets enheter.
  - Enheten har tidigare har anslutit till Active Directory (AD)-domän.
  - Enheten har en direkt anslutning till din domänkontrollanten (DC), antingen på företagets kabelanslutna eller trådlösa nätverk eller via en fjärranslutning, till exempel en VPN-anslutning.
  - Du har [distribuerat funktionen](##step-3-roll-out-the-feature) till den här användaren med hjälp av en Grupprincip.

Att testa scenariot där användaren anger endast användarnamnet men inte lösenordet:
- Logga in på *https://myapps.microsoft.com/* i en ny privat webbläsarsession.

Att testa scenariot där användaren inte behöver ange användarnamnet eller lösenordet: 
- Logga in på *https://myapps.microsoft.com/contoso.onmicrosoft.com* i en ny privat webbläsarsession. Ersätt ”*contoso*” med namnet på din klient.
- Eller logga in på *https://myapps.microsoft.com/contoso.com* i en ny privat webbläsarsession. Ersätt ”*contoso.com*” med en verifierad domän (inte en federerad domän) i din klient.

## <a name="step-5-roll-over-keys"></a>Steg 5: Återställ över nycklar

I steg 2 skapar Azure AD Connect datorkonton (som representerar Azure AD) i alla AD-skogar som du har aktiverat sömlös SSO. Läs mer i detalj [här](active-directory-aadconnect-sso-how-it-works.md). För förbättrad säkerhet rekommenderas det att du ofta slås via Kerberos dekrypteringsnycklar för dessa konton.

>[!IMPORTANT]
>Du behöver inte göra det här steget _omedelbart_ när du har aktiverat funktionen. Rulla över dekrypteringsnycklar Kerberos minst var 30 dagar.

## <a name="next-steps"></a>Nästa steg

- [**Tekniska ingående** ](active-directory-aadconnect-sso-how-it-works.md) -förstå hur funktionen fungerar.
- [**Vanliga frågor och svar** ](active-directory-aadconnect-sso-faq.md) -svar på vanliga frågor och svar.
- [**Felsöka** ](active-directory-aadconnect-troubleshoot-sso.md) – Lär dig att lösa vanliga problem med funktionen.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – för arkivering av nya funktioner som efterfrågas.
