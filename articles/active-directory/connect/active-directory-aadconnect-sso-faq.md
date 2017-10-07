---
title: "Azure AD Connect: Sömlös Single Sign-On - vanliga frågor och svar | Microsoft Docs"
description: "Svaren toofrequently frågor och svar om Azure Active Directory sömlös enkel inloggning."
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
ms.date: 08/02/2017
ms.author: billmath
ms.openlocfilehash: e91e7950670633c08c180ece873f7451fa19eef8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-seamless-single-sign-on-frequently-asked-questions"></a>Azure Active Directory sömlös enkel inloggning: vanliga frågor och svar

Vi adressera vanliga frågor och svar om Azure Active Directory sömlös enkel inloggning (sömlös SSO) i den här artikeln. Kontrollera för nytt innehåll.

>[!IMPORTANT]
>hello sömlös SSO-funktionen är för närvarande under förhandsgranskning.

## <a name="what-sign-in-methods-do-seamless-sso-work-with"></a>Vilka metoder för inloggning fungerar sömlös SSO med?

Sömlös SSO kan kombineras med antingen hello [synkronisering av lösenords-hash-](active-directory-aadconnectsync-implement-password-synchronization.md) eller [direkt autentisering](active-directory-aadconnect-pass-through-authentication.md) inloggning metoder. Men den här funktionen kan inte användas med Active Directory Federation Services (ADFS).

## <a name="is-seamless-sso-a-free-feature"></a>Är sömlös SSO en kostnadsfri funktion?

Sömlös SSO är en kostnadsfri funktion och du inte behöver några betald utgåvor av Azure AD toouse den. Den återstående ledigt när hello funktionen når allmän tillgänglighet.

## <a name="what-applications-take-advantage-of-domainhint-or-loginhint-parameter-capability-of-seamless-sso"></a>Vilka program utnyttja `domain_hint` eller `login_hint` parametern möjligheterna för sömlös SSO?

Vi kan i hello processen för att sammanställa hello lista över program som skickar dessa parametrar och hello som inte. Om du har program som är intresserade berätta för oss under hello kommentarer.

## <a name="does-seamless-sso-support-alternate-id-as-hello-username-instead-of-userprincipalname"></a>Stöder sömlös SSO `Alternate ID` som hello username, i stället för `userPrincipalName`?

Ja. Sömlös SSO stöder `Alternate ID` som hello användarnamn konfigurerades i Azure AD Connect enligt [här](active-directory-aadconnect-get-started-custom.md). Inte alla Office 365-program stöder `Alternate ID`. Referera toohello specifika program dokumentationen för hello support-instruktionen.

## <a name="i-want-tooregister-non-windows-10-devices-with-azure-ad-without-using-ad-fs-can-i-use-seamless-sso-instead"></a>Jag vill tooregister Windows 10 - enheter med Azure AD, utan att använda AD FS. Kan jag använda sömlös SSO i stället?

Ja, det här scenariot måste version 2.1 eller senare av hello [till arbetsplatsen klienten](https://www.microsoft.com/download/details.aspx?id=53554).

## <a name="how-can-i-roll-over-hello-kerberos-decryption-key-of-hello-azureadssoacct-computer-account"></a>Hur kan jag få över hello Kerberos krypteringsnyckel för hello `AZUREADSSOACCT` datorkontot?

Det är viktigt toofrequently rulla över hello Kerberos krypteringsnyckel för hello `AZUREADSSOACCT` datorkontot (som representerar Azure AD) skapas i din lokala AD-skog.

>[!IMPORTANT]
>Vi rekommenderar starkt att du lanserar över hello Kerberos dekrypteringsnyckeln minst var 30 dagar.

Följ dessa steg på hello lokal server där du kör Azure AD Connect:

### <a name="step-1-get-list-of-ad-forests-where-seamless-sso-has-been-enabled"></a>Steg 1. Hämta lista över AD-skogar där sömlös SSO har aktiverats

1. Först ladda ned och installera hello [Microsoft Online Services-inloggningsassistent](http://go.microsoft.com/fwlink/?LinkID=286152).
2. Hämta och installera hello [64-bitars Azure Active Directory-modulen för Windows PowerShell](http://go.microsoft.com/fwlink/p/?linkid=236297).
3. Navigera toohello `%programfiles%\Microsoft Azure Active Directory Connect` mapp.
4. Importera hello sömlös SSO-PowerShell-modulen med det här kommandot: `Import-Module .\AzureADSSO.psd1`.
5. Kör PowerShell som administratör. I PowerShell anropa `New-AzureADSSOAuthenticationContext`. Det här kommandot bör du få ett popup-tooenter globala autentiseringsuppgifterna för din klient.
6. Anropa `Get-AzureADSSOStatus`. Det här kommandot ger du hello lista över AD-skogar (titt på domäner ”hello” lista) på som den här funktionen har aktiverats.

### <a name="step-2-update-hello-kerberos-decryption-key-on-each-ad-forest-that-it-was-set-it-up-on"></a>Steg 2. Uppdatera hello Kerberos dekrypteringsnyckel för varje AD-skog som har angetts den in på

1. Anropa `$creds = Get-Credential`. När du uppmanas ange hello domänadministratörsbehörighet för hello avsedd AD-skog.
2. Anropa `Update-AzureADSSOForest -OnPremCredentials $creds`. Det här kommandot uppdateringar hello Kerberos dekrypteringsnyckel för hello `AZUREADSSOACCT` datorkonto i den här specifika AD-skogen och uppdateras i Azure AD.
3. Upprepa föregående steg för varje AD-skog som du har konfigurerat hello-funktionen på hello.

>[!IMPORTANT]
>Se till att du _inte_ kör hello `Update-AzureADSSOForest` kommandot mer än en gång. Annars slutar hello funktionen fungera tills hello användarnas Kerberos-biljetter upphör och återutfärdas av din lokala Active Directory.

## <a name="how-can-i-disable-seamless-sso"></a>Hur kan jag inaktivera sömlös SSO?

Sömlös SSO kan inaktiveras med Azure AD Connect.

Kör Azure AD Connect, Välj ”Ändra användare inloggningssidan” och klicka på ”Nästa”. Sedan avmarkera alternativet för hello ”aktivera enkel inloggning”. Fortsätt hello guiden. När du har slutfört guiden hello är sömlös SSO inaktiverad på din klient.

Dock visas ett meddelande på skärmen som lyder som följer:

”Enkel inloggning har inaktiverats, men det finns ytterligare manuella steg tooperform i ordning toocomplete rensning. Lär dig mer ”

toocomplete Hej process, Följ dessa instruktioner på hello lokal server där du kör Azure AD Connect:

### <a name="step-1-get-list-of-ad-forests-where-seamless-sso-has-been-enabled"></a>Steg 1. Hämta lista över AD-skogar där sömlös SSO har aktiverats

1. Först ladda ned och installera hello [Microsoft Online Services-inloggningsassistent](http://go.microsoft.com/fwlink/?LinkID=286152).
2. Hämta och installera hello [64-bitars Azure Active Directory-modulen för Windows PowerShell](http://go.microsoft.com/fwlink/p/?linkid=236297).
3. Navigera toohello `%programfiles%\Microsoft Azure Active Directory Connect` mapp.
4. Importera hello sömlös SSO-PowerShell-modulen med det här kommandot: `Import-Module .\AzureADSSO.psd1`.
5. Kör PowerShell som administratör. I PowerShell anropa `New-AzureADSSOAuthenticationContext`. Det här kommandot bör du få ett popup-tooenter globala autentiseringsuppgifterna för din klient.
6. Anropa `Get-AzureADSSOStatus`. Det här kommandot ger du hello lista över AD-skogar (titt på domäner ”hello” lista) på som den här funktionen har aktiverats.

### <a name="step-2-manually-delete-hello-azureadssoacct-computer-account-from-each-ad-forest-that-you-see-listed"></a>Steg 2. Ta manuellt bort hello `AZUREADSSOACCT` datorkontot från varje AD-skog i listan.

## <a name="next-steps"></a>Nästa steg

- [**Snabbstart** ](active-directory-aadconnect-sso-quick-start.md) – få och kör Azure AD sömlös SSO.
- [**Tekniska ingående** ](active-directory-aadconnect-sso-how-it-works.md) -förstå hur funktionen fungerar.
- [**Felsöka** ](active-directory-aadconnect-troubleshoot-sso.md) – Lär dig hur tooresolve vanliga problem med hello-funktionen.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – för arkivering av nya funktioner som efterfrågas.
