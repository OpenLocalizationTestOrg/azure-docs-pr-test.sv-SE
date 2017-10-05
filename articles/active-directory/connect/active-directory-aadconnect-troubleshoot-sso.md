---
title: "Azure AD Connect: Felsöka sömlös enkel inloggning | Microsoft Docs"
description: "Det här avsnittet beskriver hur du felsöker Azure Active Directory sömlös enkel inloggning (Azure AD sömlös SSO)."
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
ms.openlocfilehash: bc4ff9125553c8918df3a1f84041560a5b7d4cd8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="troubleshoot-azure-active-directory-seamless-single-sign-on"></a>Felsöka Azure Active Directory sömlös enkel inloggning

Den här artikeln får du hittar felsökningsinformation om vanliga problem om Azure AD sömlös enkel inloggning.

## <a name="known-issues"></a>Kända problem

- Om du vill synkronisera 30 eller flera AD-skogar, kan du inte aktivera sömlös SSO med Azure AD Connect. Som en tillfällig lösning kan du [manuellt aktivera](#manual-reset-of-azure-ad-seamless-sso) funktionen på din klient.
- Att lägga till URL: er till Azure AD (https://autologon.microsoftazuread-sso.com, https://aadg.windows.net.nsatc.net) ”” tillförlitliga platser i stället för ”lokalt intranät” **hindrar användare från att logga in**.
- Sömlös SSO fungerar inte i privat webbläsarens läge på Firefox och kant. Och även på Internet Explorer när läget för utökat skydd är aktiverat.

>[!IMPORTANT]
>Vi nyligen återställde stöd för kant för att undersöka problem som rapporteras av kunden.

## <a name="check-status-of-the-feature"></a>Kontrollera statusen för funktionen

Kontrollera att funktionen sömlös SSO är fortfarande **aktiverad** på din klient. Du kan kontrollera statusen genom att gå till den **Azure AD Connect** blad i den [Azure Active Directory Administrationscenter](https://aad.portal.azure.com/).

![Azure Active Directory Administrationscenter - bladet i Azure AD Connect](./media/active-directory-aadconnect-sso/sso10.png)

## <a name="sign-in-failure-reasons-on-the-azure-active-directory-admin-center"></a>Inloggningsfel orsaker på Azure Active Directory Administrationscenter

En bra att starta felsökning användare logga in med sömlös SSO är att titta på den [inloggningsaktivitet rapporten](../active-directory-reporting-activity-sign-ins.md) på den [Azure Active Directory Administrationscenter](https://aad.portal.azure.com/).

![Azure Active Directory Administrationscenter - inloggningar rapport](./media/active-directory-aadconnect-sso/sso9.png)

Gå till **Azure Active Directory** -> **inloggningar** på den [Azure Active Directory Administrationscenter](https://aad.portal.azure.com/) och klicka på en viss användare inloggningsaktivitet. Leta efter den **LOGGA IN FELKODEN** fältet. Mappa värdet för fältet till en orsaken till felet och en lösning med hjälp av följande tabell:

|Felkod för inloggning|Logga in felorsak|Lösning
| --- | --- | ---
| 81001 | Användarens Kerberos-biljett är för stor. | Minska användarens gruppmedlemskap och försök igen.
| 81002 | Det gick inte att verifiera användarens Kerberos-biljett. | Se [felsökning checklista](#troubleshooting-checklist).
| 81003 | Det gick inte att verifiera användarens Kerberos-biljett. | Se [felsökning checklista](#troubleshooting-checklist).
| 81004 | Kerberos-autentiseringsförsök misslyckades. | Se [felsökning checklista](#troubleshooting-checklist).
| 81008 | Det gick inte att verifiera användarens Kerberos-biljett. | Se [felsökning checklista](#troubleshooting-checklist).
| 81009 | ”Det gick inte att verifiera användarens Kerberos-biljett. | Se [felsökning checklista](#troubleshooting-checklist).
| 81010 | Sömlös SSO misslyckades eftersom användarens Kerberos-biljett har upphört att gälla eller är ogiltig. | Användaren måste logga in från en domänansluten enhet i företagsnätverket.
| 81011 | Det gick inte att hitta användarobjektet baserat på informationen i användarens Kerberos-biljett. | Använda Azure AD Connect för att synkronisera användarinformation i Azure AD.
| 81012 | Användaren som försöker logga in på Azure AD skiljer sig från användaren som har loggat in på enheten. | Logga in från en annan enhet.
| 81013 | Det gick inte att hitta användarobjektet baserat på informationen i användarens Kerberos-biljett. |Använda Azure AD Connect för att synkronisera användarinformation i Azure AD. 

## <a name="troubleshooting-checklist"></a>Checklista för felsökning

Använd följande checklista för att felsöka problem med sömlös SSO:

- Kontrollera om sömlös SSO-funktionen är aktiverad i Azure AD Connect. Om du inte kan aktivera funktionen (t.ex, på grund av en blockerad-port), se till att du har alla de [förutsättningar](active-directory-aadconnect-sso-quick-start.md#step-1-check-prerequisites) på plats.
- Kontrollera om båda dessa Azure AD-URL: er (https://autologon.microsoftazuread-sso.com och https://aadg.windows.net.nsatc.net) är en del av användarens intranätsinställningar för zonen.
- Se till att företagsenhet som är ansluten till den AD-domänen.
- Se till att användaren är inloggad på den enhet som använder ett AD-domänkonto.
- Kontrollera att användarkontot har från en AD-skog där sömlös SSO har ställts in.
- Se till att enheten är ansluten i företagsnätverket.
- Kontrollera att enhetens tid är synkroniserad med Active Directory och-domänkontrollanter och inom fem minuter från varandra.
- Visa en lista med befintliga Kerberos-biljetter på enheten med den **klist** kommandot från en kommandotolk. Kontrollera om biljetter som utfärdats för den `AZUREADSSOACCT` datorkonto finns. Användarnas Kerberos-biljetter gäller vanligtvis 12 timmar. Du kan ha olika inställningar i Active Directory.
- Rensa befintliga Kerberos-biljetter från enheten med den **klist Rensa** kommandot och försök igen.
- Granska loggarna för konsolen i webbläsaren (under ”Developer Tools”) för att avgöra om det är JavaScript-relaterade problem.
- Granska de [domänkontrollanten loggar](#domain-controller-logs) samt.

### <a name="domain-controller-logs"></a>Domänkontrollanten loggar

Om lyckad granskning är aktiverat på en domänkontrollant och sedan varje gång en användare loggar in med sömlös SSO registreras en säkerhet post i händelseloggen. Du hittar dessa säkerhetshändelser med följande fråga (Sök efter händelse **4769** som är associerade med datorkontot **AzureADSSOAcc$**):

```
    <QueryList>
      <Query Id="0" Path="Security">
    <Select Path="Security">*[EventData[Data[@Name='ServiceName'] and (Data='AZUREADSSOACC$')]]</Select>
      </Query>
    </QueryList>
```

## <a name="manual-reset-of-the-feature"></a>Manuell återställning av funktionen

Om felsökningen inte hjälper kan återställa du manuellt funktionen på din klient. Följ dessa steg på den lokala servern där du kör Azure AD Connect:

### <a name="step-1-import-the-seamless-sso-powershell-module"></a>Steg 1: Importera sömlös SSO-PowerShell-modulen

1. Först ladda ned och installera den [Microsoft Online Services-inloggningsassistent](http://go.microsoft.com/fwlink/?LinkID=286152).
2. Hämta och installera den [64-bitars Azure Active Directory-modulen för Windows PowerShell](http://go.microsoft.com/fwlink/p/?linkid=236297).
3. Navigera till den `%programfiles%\Microsoft Azure Active Directory Connect` mapp.
4. Importera sömlös SSO-PowerShell-modulen med det här kommandot: `Import-Module .\AzureADSSO.psd1`.

### <a name="step-2-get-the-list-of-ad-forests-on-which-seamless-sso-has-been-enabled"></a>Steg 2: Hämta listan över AD-skogar där sömlös SSO har aktiverats

1. Kör PowerShell som administratör. I PowerShell anropa `New-AzureADSSOAuthenticationContext`. När du uppmanas, anger du autentiseringsuppgifter för Global administratör för din klient.
2. Anropa `Get-AzureADSSOStatus`. Det här kommandot innehåller en lista över AD-skogar (titt på listan ”domäner”) på som den här funktionen har aktiverats.

### <a name="step-3-disable-seamless-sso-for-each-ad-forest-that-it-was-set-it-up-on"></a>Steg 3: Inaktivera sömlös SSO för varje AD-skog som har angetts den in på

1. Anropa `$creds = Get-Credential`. När du uppmanas, anger du inloggningsuppgifterna för domänadministratören för den avsedda AD-skogen.
2. Anropa `Disable-AzureADSSOForest -OnPremCredentials $creds`. Detta kommando tar bort den `AZUREADSSOACCT` datorkontot från den lokala domänkontrollanten för den här specifika AD-skogen.
3. Upprepa föregående steg för varje AD-skog som du har ställt in funktionen på.

### <a name="step-4-enable-seamless-sso-for-each-ad-forest"></a>Steg 4: Aktivera sömlös SSO för varje AD-skog

1. Anropa `Enable-AzureADSSOForest`. När du uppmanas, anger du inloggningsuppgifterna för domänadministratören för den avsedda AD-skogen.
2. Upprepa föregående steg för varje AD-skog som du vill konfigurera funktionen på.

### <a name="step-5-enable-the-feature-on-your-tenant"></a>Steg 5. Aktivera funktionen på din klient

Anropa `Enable-AzureADSSO` och anger ”true” på den `Enable: ` prompten för att aktivera funktionen i din klient.
