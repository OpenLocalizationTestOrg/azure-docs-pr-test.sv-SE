---
title: "Azure AD Connect: Felsöka sömlös enkel inloggning | Microsoft Docs"
description: "Det här avsnittet beskrivs hur tootroubleshoot Azure Active Directory sömlös enkel inloggning (Azure AD sömlös SSO)."
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
ms.openlocfilehash: f1c1c11522f22d5bc742c126fff483c5b06e1f06
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-active-directory-seamless-single-sign-on"></a>Felsöka Azure Active Directory sömlös enkel inloggning

Den här artikeln får du hittar felsökningsinformation om vanliga problem om Azure AD sömlös enkel inloggning.

## <a name="known-issues"></a>Kända problem

- Om du vill synkronisera 30 eller flera AD-skogar, kan du inte aktivera sömlös SSO med Azure AD Connect. Som en tillfällig lösning kan du [manuellt aktivera](#manual-reset-of-azure-ad-seamless-sso) hello-funktionen på din klient.
- Att lägga till Azure AD-tjänsten URL: er (https://autologon.microsoftazuread-sso.com, https://aadg.windows.net.nsatc.net) toohello ”” i zonen Tillförlitliga platser i stället för hello ”lokalt intranät-zonen **hindrar användare från att logga in**.
- Sömlös SSO fungerar inte i privat webbläsarens läge på Firefox och kant. Och även på Internet Explorer när läget för utökat skydd är aktiverat.

>[!IMPORTANT]
>Vi nyligen återställde stöd för Edge tooinvestigate kunden rapporterade problem.

## <a name="check-status-of-hello-feature"></a>Kontrollera status för hello-funktion

Se till att hello sömlös SSO-funktionen är fortfarande **aktiverad** på din klient. Du kan kontrollera statusen genom att gå toohello **Azure AD Connect** bladet på hello [Azure Active Directory Administrationscenter](https://aad.portal.azure.com/).

![Azure Active Directory Administrationscenter - bladet i Azure AD Connect](./media/active-directory-aadconnect-sso/sso10.png)

## <a name="sign-in-failure-reasons-on-hello-azure-active-directory-admin-center"></a>Inloggningsfel orsaker på hello Azure Active Directory Administrationscenter

En bra toostart felsökning användare logga in med sömlös SSO är toolook på hello [inloggningsaktivitet rapporten](../active-directory-reporting-activity-sign-ins.md) på hello [Azure Active Directory Administrationscenter](https://aad.portal.azure.com/).

![Azure Active Directory Administrationscenter - inloggningar rapport](./media/active-directory-aadconnect-sso/sso9.png)

Navigera för**Azure Active Directory** -> **inloggningar** på hello [Azure Active Directory Administrationscenter](https://aad.portal.azure.com/) och klicka på en viss användare inloggningsaktivitet. Leta efter hello **LOGGA IN FELKODEN** fältet. Mappa hello-värde som fältet tooa orsaken till felet och lösning med hjälp av hello följande tabell:

|Felkod för inloggning|Logga in felorsak|Lösning
| --- | --- | ---
| 81001 | Användarens Kerberos-biljett är för stor. | Minska användarens gruppmedlemskap och försök igen.
| 81002 | Det går inte toovalidate användarens Kerberos-biljett. | Se [felsökning checklista](#troubleshooting-checklist).
| 81003 | Det går inte toovalidate användarens Kerberos-biljett. | Se [felsökning checklista](#troubleshooting-checklist).
| 81004 | Kerberos-autentiseringsförsök misslyckades. | Se [felsökning checklista](#troubleshooting-checklist).
| 81008 | Det går inte toovalidate användarens Kerberos-biljett. | Se [felsökning checklista](#troubleshooting-checklist).
| 81009 | ”Det gick inte toovalidate användarens Kerberos-biljett. | Se [felsökning checklista](#troubleshooting-checklist).
| 81010 | Sömlös SSO misslyckades eftersom hello användarens Kerberos-biljetten har upphört att gälla eller är ogiltig. | Användaren måste toosign i från en domänansluten enhet i företagsnätverket.
| 81011 | Det går inte toofind användarobjektet baserat på informationen i hello användarens Kerberos-biljett. | Använda Azure AD Connect toosynchronize användarinformation i Azure AD.
| 81012 | hello användaren försöker toosign i tooAzure AD skiljer sig från hello användaren loggat in hello enhet. | Logga in från en annan enhet.
| 81013 | Det går inte toofind användarobjektet baserat på informationen i hello användarens Kerberos-biljett. |Använda Azure AD Connect toosynchronize användarinformation i Azure AD. 

## <a name="troubleshooting-checklist"></a>Checklista för felsökning

Använd följande checklista tootroubleshoot sömlös SSO problem hello:

- Kontrollera om hello sömlös SSO-funktionen är aktiverad i Azure AD Connect. Om du inte aktiverar hello-funktionen (t.ex, på grund av tooa blockeras port), kontrollera att du har alla hello [förutsättningar](active-directory-aadconnect-sso-quick-start.md#step-1-check-prerequisites) på plats.
- Kontrollera om båda dessa Azure AD-URL: er (https://autologon.microsoftazuread-sso.com och https://aadg.windows.net.nsatc.net) är en del av hello användarens intranätsinställningar för zonen.
- Kontrollera hello företagets enheter är domänanslutna toohello AD-domän.
- Kontrollera hello användaren är inloggad på toohello enhet med hjälp av en AD-domänkonto.
- Se till att hello användarkonto har från en AD-skog där sömlös SSO har ställts in.
- Kontrollera hello enhet är ansluten i hello företagsnätverk.
- Se till att hello enheten synkroniseras med hello Active Directory och hello-domänkontrollanter och inom fem minuter efter varandra.
- Visa en lista med befintliga Kerberos-biljetter på hello-enhet med hjälp av hello **klist** kommandot från en kommandotolk. Kontrollera om biljetter som utfärdats för hello `AZUREADSSOACCT` datorkonto finns. Användarnas Kerberos-biljetter gäller vanligtvis 12 timmar. Du kan ha olika inställningar i Active Directory.
- Rensa befintliga Kerberos-biljetter från hello-enhet med hjälp av hello **klist Rensa** kommandot och försök igen.
- toodetermine om det är JavaScript-relaterade problem loggarna hello-konsolen i webbläsaren hello (under ”Developer Tools”).
- Granska hello [domänkontrollanten loggar](#domain-controller-logs) samt.

### <a name="domain-controller-logs"></a>Domänkontrollanten loggar

Om lyckad granskning är aktiverat på domänkontrollanten, sedan varje gång en användare loggar in med sömlös SSO en säkerhet post sparas i hello-händelseloggen. Du hittar dessa säkerhetshändelser med följande fråga hello (Sök efter händelse **4769** som är associerade med hello datorkonto **AzureADSSOAcc$**):

```
    <QueryList>
      <Query Id="0" Path="Security">
    <Select Path="Security">*[EventData[Data[@Name='ServiceName'] and (Data='AZUREADSSOACC$')]]</Select>
      </Query>
    </QueryList>
```

## <a name="manual-reset-of-hello-feature"></a>Manuell återställning av hello-funktionen

Om felsökningen inte hjälper kan återställa du manuellt hello-funktionen på din klient. Följ dessa steg på hello lokal server där du kör Azure AD Connect:

### <a name="step-1-import-hello-seamless-sso-powershell-module"></a>Steg 1: Importera hello sömlös SSO-PowerShell-modulen

1. Först ladda ned och installera hello [Microsoft Online Services-inloggningsassistent](http://go.microsoft.com/fwlink/?LinkID=286152).
2. Hämta och installera hello [64-bitars Azure Active Directory-modulen för Windows PowerShell](http://go.microsoft.com/fwlink/p/?linkid=236297).
3. Navigera toohello `%programfiles%\Microsoft Azure Active Directory Connect` mapp.
4. Importera hello sömlös SSO-PowerShell-modulen med det här kommandot: `Import-Module .\AzureADSSO.psd1`.

### <a name="step-2-get-hello-list-of-ad-forests-on-which-seamless-sso-has-been-enabled"></a>Steg 2: Hämta hello lista över AD-skogar där sömlös SSO har aktiverats

1. Kör PowerShell som administratör. I PowerShell anropa `New-AzureADSSOAuthenticationContext`. När du uppmanas, anger du autentiseringsuppgifter för Global administratör för din klient.
2. Anropa `Get-AzureADSSOStatus`. Det här kommandot ger du hello lista över AD-skogar (titt på domäner ”hello” lista) på som den här funktionen har aktiverats.

### <a name="step-3-disable-seamless-sso-for-each-ad-forest-that-it-was-set-it-up-on"></a>Steg 3: Inaktivera sömlös SSO för varje AD-skog som har angetts den in på

1. Anropa `$creds = Get-Credential`. När du uppmanas ange hello domänadministratörsbehörighet för hello avsedd AD-skog.
2. Anropa `Disable-AzureADSSOForest -OnPremCredentials $creds`. Detta kommando tar bort hello `AZUREADSSOACCT` datorkontot från hello lokala domänkontrollanten för den här specifika AD-skogen.
3. Upprepa föregående steg för varje AD-skog som du har konfigurerat hello-funktionen på hello.

### <a name="step-4-enable-seamless-sso-for-each-ad-forest"></a>Steg 4: Aktivera sömlös SSO för varje AD-skog

1. Anropa `Enable-AzureADSSOForest`. När du uppmanas ange hello domänadministratörsbehörighet för hello avsedd AD-skog.
2. Upprepa hello föregående steg för varje AD-skog som du vill ha tooset in hello-funktionen.

### <a name="step-5-enable-hello-feature-on-your-tenant"></a>Steg 5. Aktivera hello-funktionen på din klient

Anropa `Enable-AzureADSSO` och anger hello ”true” `Enable: ` prompt tooturn på hello-funktionen i din klient.
