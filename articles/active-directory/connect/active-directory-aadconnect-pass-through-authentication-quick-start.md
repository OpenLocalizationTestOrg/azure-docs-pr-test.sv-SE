---
title: Azure AD-direkt-autentisering - Snabbstart | Microsoft Docs
description: "Den här artikeln beskriver hur tooget igång med Azure Active Directory (AD Azure) direkt-autentisering."
services: active-directory
keywords: "Azure AD Connect direkt-autentisering, installera Active Directory, nödvändiga komponenter för Azure AD, SSO, Single Sign-on"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: billmath
ms.openlocfilehash: d6d0f85fe144cf36cc94676f6592d37988b20647
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-pass-through-authentication-quick-start"></a>Azure Active Directory direkt-autentisering: Snabbstart

## <a name="how-toodeploy-azure-ad-pass-through-authentication"></a>Hur toodeploy direkt i Azure AD-autentisering

Azure Active Directory (AD Azure) direkt-autentisering kan dina användare toosign i tooboth lokala och molnbaserade program med hjälp av hello samma lösenord. Den loggar användarna in genom att verifiera sina lösenord direkt mot din lokala Active Directory.

>[!IMPORTANT]
>Azure AD direkt-autentisering är för närvarande under förhandsgranskning. Om du har använt funktionen via Förhandsgranska, bör du kontrollera att du uppgraderar förhandsversioner av hello autentisering agenter med hjälp av anvisningarna hello [här](./active-directory-aadconnect-pass-through-authentication-upgrade-preview-authentication-agents.md).

Du behöver toofollow dessa instruktioner toodeploy direkt-autentisering:

## <a name="step-1-check-prerequisites"></a>Steg 1: Kontrollera krav

Kontrollera att hello följande förutsättningar är uppfyllda:

### <a name="on-hello-azure-active-directory-admin-center"></a>På hello Azure Active Directory Administrationscenter

1. Skapa ett globalt administratörskonto endast molnbaserad på Azure AD-klienten. Det här sättet kan du hantera hello konfigurationen för din klient ska lokala tjänster misslyckas eller vara tillgänglig. Lär dig mer om [att lägga till ett globalt administratörskonto endast molnbaserad](../active-directory-users-create-azure-portal.md). Gör det här steget är kritiska tooensure som du inte blir utelåst för din klient.
2. Lägg till en eller flera [domänen namn](../active-directory-add-domain.md) tooyour Azure AD-klient. Dina användare logga in med ett av dessa domännamn.

### <a name="in-your-on-premises-environment"></a>I din lokala miljö

1. Identifiera en server som kör Windows Server 2012 R2 eller senare på vilka toorun Azure AD Connect. Lägg till hello server toohello samma AD-skog som hello användare vars lösenord måste toobe verifieras.
2. Installera hello [senaste versionen av Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594) på hello server identifierade i föregående steg. Om du redan har Azure AD Connect körs, kontrollera att hello-versionen är 1.1.557.0 eller senare.
3. Identifiera en ytterligare server som kör Windows Server 2012 R2 eller senare på vilka toorun en fristående autentiseringsagent. hello autentiseringsagent version måste toobe 1.5.193.0 eller senare. Den här servern är nödvändiga tooensure hög tillgänglighet för inloggningsförfrågningar. Lägg till hello server toohello samma AD-skog som hello användare vars lösenord måste toobe verifieras.
4. Om det finns en brandvägg mellan servrarna och Azure AD, behöver du tooconfigure hello följande objekt:
   - Se till att autentisering agenter kan göra **utgående** begäranden tooAzure AD över hello följande portar:
   
   | Portnummer | Hur den används |
   | --- | --- |
   | **80** | Hämta certifikatåterkallning listor över återkallade under validering av hello SSL-certifikat |
   | **443** | All utgående kommunikation med vår tjänst |
   
   Om brandväggen tillämpar regler enligt toooriginating användare, kan du öppna dessa portar för trafik från Windows-tjänster som körs som Nätverkstjänst.
   - Om din brandvägg eller proxyserver tillåter DNS, vitlistning av tillåtna anslutningar för**\*. msappproxy.net** och  **\*. servicebus.windows.net**. Om inte, Tillåt åtkomst för[Azure DataCenter-IP-intervall](https://www.microsoft.com/download/details.aspx?id=41653), som uppdateras varje vecka.
   - Autentisering agenterna behöver åtkomst för**login.windows.net** och **login.microsoftonline.com** för inledande registrering, så att öppna brandväggen för dessa URL: er samt.
   - Valideringen av servercertifikatet avblockera i hello följande URL: er: **mscrl.microsoft.com:80**, **crl.microsoft.com:80**, **ocsp.msocsp.com:80** och  **www.microsoft.com:80**. Dessa URL: er används för certifikatsverifiering med andra Microsoftprodukter, så du kanske redan har webbadresserna avblockerad.

## <a name="step-2-enable-exchange-activesync-support-optional"></a>Steg 2: Aktivera stöd för Exchange ActiveSync (valfritt)

Följ dessa instruktioner tooenable stöd för Exchange ActiveSync:

1. Använd [Exchange PowerShell](https://technet.microsoft.com/library/mt587043(v=exchg.150).aspx) toorun hello följande kommando:
```
Get-OrganizationConfig | fl per*
```

2. Kontrollera hello värdet av hello `PerTenantSwitchToESTSEnabled` inställningen. Om värdet för hello är **SANT**, konfigurerats korrekt för din klient – detta är vanligtvis hello fallet för de flesta kunder. Om värdet för hello är **FALSKT**kör hello följande kommando:
```
Set-OrganizationConfig -PerTenantSwitchToESTSEnabled:$true
```

3. Kontrollera att namnvärdet hello av hello `PerTenantSwitchToESTSEnabled` är nu inställningen för**SANT**. Vänta en timme innan glidande toohello nästa steg.

Om du står inför eventuella problem i det här steget kontrollerar vår [felsökningsguide för](active-directory-aadconnect-troubleshoot-pass-through-authentication.md#exchange-activesync-configuration-issues) för mer information.

## <a name="step-3-enable-hello-feature"></a>Steg 3: Aktivera hello-funktionen

Direkt-autentisering kan aktiveras med [Azure AD Connect](active-directory-aadconnect.md).

>[!IMPORTANT]
>Direkt-autentisering kan aktiveras på hello Azure AD Connect primär eller fristående server. Vi rekommenderar att du aktiverar den från hello primära servern.

Om du installerar Azure AD Connect för hello första gången, Välj hello [anpassade installationssökvägen](active-directory-aadconnect-get-started-custom.md). Vid hello **användarinloggning** väljer **direkt autentisering** som hello logga metod. Åtgärden lyckades, en direkt autentisering agent har installerats på hello samma server som Azure AD Connect. Dessutom är hello direkt-autentisering aktiverad på din klient.

![Azure AD Connect - användarinloggning](./media/active-directory-aadconnect-sso/sso3.png)

Om du redan har installerat Azure AD Connect (med hjälp av hello [Snabbinstallation](active-directory-aadconnect-get-started-express.md) eller hello [anpassad installation](active-directory-aadconnect-get-started-custom.md) sökväg), Välj **ändra användare inloggningssidan** på Azure AD Ansluta och klicka på **nästa**. Välj sedan **direkt autentisering** som hello logga metod. Åtgärden lyckades, en direkt autentisering agent har installerats på hello samma server som Azure AD Connect och hello-funktionen är aktiverad på din klient.

![Azure AD Connect - ändra användarens inloggning](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

>[!IMPORTANT]
>Direkt-autentisering är en funktion för klient-nivå. Aktivera den påverkar inloggning för användare i _alla_ hello hanterade domäner i din klient. Om du byter från AD FS tooPass via autentisering, rekommenderar vi att du väntar minst 12 timmar innan du stänger av AD FS-infrastrukturen - väntetiden är tooensure som användare kan hålla inloggning tooExchange ActiveSync under övergång.

## <a name="step-4-test-hello-feature"></a>Steg 4: Testa hello-funktion

Följ dessa instruktioner tooverify att du har aktiverat direkt-autentisering korrekt:

1. Logga in toohello [Azure Active Directory Administrationscenter](https://aad.portal.azure.com) med hello globala administratörsbehörigheter för din klient.
2. Välj **Azure Active Directory** på hello vänstra navigeringsfönstret.
3. Välj **Azure AD Connect**.
4. Kontrollera att hello **direkt autentisering** funktionen visar som **aktiverad**.
5. Välj **direkt autentisering**. Det här bladet visar hello servrar där autentisering-agenter är installerade.

![Azure Active Directory Administrationscenter - bladet i Azure AD Connect](./media/active-directory-aadconnect-pass-through-authentication/pta7.png)

![Azure Active Directory Administrationscenter - bladet direkt-autentisering](./media/active-directory-aadconnect-pass-through-authentication/pta8.png)

I det här skedet kan användare från alla hanterade domäner i din klient logga in med hjälp av direkt-autentisering. Dock fortsätta användare från externa domäner toosign i med hjälp av Active Directory Federation Services (AD FS) eller en annan federation-provider som du tidigare har konfigurerat. Om du konverterar en domän från federerade toomanaged starta alla användare från domänen automatiskt logga in med hjälp av direkt-autentisering. Endast molnbaserad användare påverkas inte av hello direkt autentisering funktionen.

## <a name="step-5-ensure-high-availability"></a>Steg 5: Garantera hög tillgänglighet

Om du planerar toodeploy direkt autentisering i en produktionsmiljö, bör du installera en fristående autentiseringsagent. Installera Agent på den här andra autentisering på en server _andra_ än hello en körs Azure AD Connect och hello första autentiseringsagent. Den här inställningen ger du hög tillgänglighet för inloggningsförfrågningar. Följ dessa instruktioner toodeploy fristående autentiseringsagent:

1. **Hämta hello senaste versionen av hello autentiseringsagent (versioner 1.5.193.0 eller senare)**: Logga in toohello [Azure Active Directory Administrationscenter](https://aad.portal.azure.com) med globala administratörsbehörigheter för din klient.
2. Välj **Azure Active Directory** på hello vänstra navigeringsfönstret.
3. Välj **Azure AD Connect** och sedan **direkt autentisering**. Och välj **Download agent**.
4. Klicka på hello **accepterar villkoren och ladda ned** knappen.
5. **Installera hello senaste versionen av hello autentiseringsagent**: kör hello körbara hämtade i hello föregående steg. Ange din klient Global administratör autentiseringsuppgifter när du tillfrågas.

![Azure Active Directory Administrationscenter - knappen ladda ned Agent för autentisering](./media/active-directory-aadconnect-pass-through-authentication/pta9.png)

![Azure Active Directory Administrationscenter – hämta Agent-bladet](./media/active-directory-aadconnect-pass-through-authentication/pta10.png)

>[!NOTE]
>Du kan också hämta hello autentiseringsagent från [här](https://aka.ms/getauthagent). Se till att du granskar och acceptera hello Authentication Agent [användarvillkoren](https://aka.ms/authagenteula) _innan_ installera den.

## <a name="next-steps"></a>Nästa steg
- [**Aktuella begränsningar** ](active-directory-aadconnect-pass-through-authentication-current-limitations.md) -funktionen är för närvarande under förhandsgranskning. Läs om vilka scenarier som stöds och vilka som inte är.
- [**Tekniska ingående** ](active-directory-aadconnect-pass-through-authentication-how-it-works.md) -förstå hur funktionen fungerar.
- [**Vanliga frågor och svar** ](active-directory-aadconnect-pass-through-authentication-faq.md) -toofrequently frågor och svar.
- [**Felsöka** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) – Lär dig hur tooresolve vanliga problem med hello-funktionen.
- [**Azure AD sömlös SSO** ](active-directory-aadconnect-sso.md) -mer information om den här funktionen.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – för arkivering av nya funktioner som efterfrågas.
