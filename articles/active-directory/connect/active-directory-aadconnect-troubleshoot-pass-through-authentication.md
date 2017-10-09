---
title: "Azure AD Connect: Felsöka direkt-autentisering | Microsoft Docs"
description: "Den här artikeln beskriver hur tootroubleshoot Azure Active Directory (AD Azure) direkt-autentisering."
services: active-directory
keywords: "Felsöka Azure AD Connect direkt-autentisering, installera Active Directory, komponenter som krävs för Azure AD, SSO, Single Sign-on"
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
ms.openlocfilehash: 87130952f660762f91b0a34b05287603b565639f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-active-directory-pass-through-authentication"></a>Felsöka Azure Active Directory direkt-autentisering

Den här artikeln får du hittar felsökningsinformation om vanliga frågor om Azure AD direkt-autentisering.

>[!IMPORTANT]
>Om du inför användaren inloggningsproblem med direkt-autentisering inte inaktivera funktionen hello eller avinstallera agenter för direkt-autentisering utan att behöva en endast molnbaserad Global administratör konto toofall igen. Lär dig mer om [att lägga till ett globalt administratörskonto endast molnbaserad](../active-directory-users-create-azure-portal.md). Gör det här steget är mycket viktigt och garanterar att du inte blir utelåst från din klient.

## <a name="general-issues"></a>Allmänna frågor

### <a name="check-status-of-hello-feature-and-authentication-agents"></a>Kontrollera status för hello-funktionen och agenter för autentisering

Se till att funktionen hello direkt-autentisering är fortfarande **aktiverad** på din klient och hello statusen för autentisering agenter visas **Active**, och inte **inaktiv**. Du kan kontrollera statusen genom att gå toohello **Azure AD Connect** bladet på hello [Azure Active Directory Administrationscenter](https://aad.portal.azure.com/).

![Azure Active Directory Administrationscenter - bladet i Azure AD Connect](./media/active-directory-aadconnect-pass-through-authentication/pta7.png)

![Azure Active Directory Administrationscenter - bladet direkt-autentisering](./media/active-directory-aadconnect-pass-through-authentication/pta11.png)

### <a name="user-facing-sign-in-error-messages"></a>Användarinriktad inloggning felmeddelanden

Om användaren hello toosign till med hjälp av direkt-autentisering, kan de finns i hello följande användarinriktad fel på inloggningssidan för hello Azure AD: 

|Fel|Beskrivning|Lösning
| --- | --- | ---
|AADSTS80001|Det går inte tooconnect tooActive Directory|Se till att agenten servrarna är medlemmar i hello samma AD-skog som hello användare vars lösenord måste toobe verifieras och de kan tooconnect tooActive Directory.  
|AADSTS8002|En timeout uppstod anslutande tooActive Directory|Kontrollera tooensure att Active Directory är tillgänglig och svarar toorequests från hello agenter.
|AADSTS80004|hello användarnamnet som skickades toohello agenten var inte giltig|Kontrollera hello användaren försöker toosign in med hello rätt användarnamn.
|AADSTS80005|Verifieringen påträffade oväntade WebException|Ett tillfälligt fel. Försök igen med hello-begäran. Kontakta Microsoft support om det fortfarande toofail.
|AADSTS80007|Ett fel inträffade under kommunikation med Active Directory|Kontrollera hello agenten loggar för mer information och att Active Directory fungerar som förväntat.

### <a name="sign-in-failure-reasons-on-hello-azure-active-directory-admin-center"></a>Inloggningsfel orsaker på hello Azure Active Directory Administrationscenter

Starta felsökning användare logga in genom att titta på hello [inloggningsaktivitet rapporten](../active-directory-reporting-activity-sign-ins.md) på hello [Azure Active Directory Administrationscenter](https://aad.portal.azure.com/).

![Azure Active Directory Administrationscenter - inloggningar rapport](./media/active-directory-aadconnect-pass-through-authentication/pta4.png)

Navigera för**Azure Active Directory** -> **inloggningar** på hello [Azure Active Directory Administrationscenter](https://aad.portal.azure.com/) och klicka på en viss användare inloggningsaktivitet. Leta efter hello **LOGGA IN FELKODEN** fältet. Mappa hello-värde som fältet tooa orsaken till felet och lösning med hjälp av hello följande tabell:

|Felkod för inloggning|Logga in felorsak|Lösning
| --- | --- | ---
| 50144 | Användarens Active Directory-lösenord har upphört att gälla. | Återställa hello användarens lösenord i din lokala Active Directory.
| 80001 | Ingen autentiseringsagent är tillgänglig. | Installera och registrera en Agent för autentisering.
| 80002 | Den tillåtna tiden för autentiseringsagentens lösenordsvalidering överskreds. | Kontrollera om din Active Directory kan nås från hello autentiseringsagent.
| 80003 | Ogiltigt svar har tagits emot av autentiseringsagenten. | Om hello problemet konsekvent reproduceras mellan flera användare, kontrollera konfigurationen av Active Directory.
| 80004 | Felaktig UPN (User Principal Name) används i begäran om inloggning. | Be hello användaren toosign in med hello rätt användarnamn.
| 80005 | Autentiseringsagent: Fel uppstod. | Tillfälligt fel. Försök igen senare.
| 80007 | Autentisering Agent tooconnect tooActive Directory. | Kontrollera om din Active Directory kan nås från hello autentiseringsagent.
| 80010 | Autentisering Agent toodecrypt lösenord. | Om hello problem är konsekvent reproduceras, installera och registrera en ny Agent för autentisering. Och avinstallera hello aktuella. 
| 80011 | Krypteringsnyckel för autentisering Agent tooretrieve. | Om hello problem är konsekvent reproduceras, installera och registrera en ny Agent för autentisering. Och avinstallera hello aktuella.

## <a name="authentication-agent-installation-issues"></a>Problem med installationen av autentisering

### <a name="an-unexpected-error-occurred"></a>Ett oväntat fel uppstod

[Samla in agenten loggar](#collecting-pass-through-authentication-agent-logs) från hello server och kontakta Microsoft Support med ditt problem.

## <a name="authentication-agent-registration-issues"></a>Problem med autentisering Agent registrering

### <a name="registration-of-hello-authentication-agent-failed-due-tooblocked-ports"></a>Registrering av hello autentiseringsagent misslyckades på grund av tooblocked portar

Kontrollera att hello-server på vilken hello autentiseringsagent har installerats kan kommunicera med vår tjänst-URL: er och portar [här](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-1-check-prerequisites).

### <a name="registration-of-hello-authentication-agent-failed-due-tootoken-or-account-authorization-errors"></a>Registrering av hello autentiseringsagent misslyckades på grund av fel tootoken eller konto auktorisering

Kontrollera att du använder ett globalt administratörskonto endast molnbaserad för alla Azure AD Connect eller fristående autentiseringsagent installation och registrering åtgärder. Det finns ett känt problem med MFA-aktiverade globala administratörskonton; inaktivera MFA tillfälligt (endast toocomplete hello operations) som en lösning.

### <a name="an-unexpected-error-occurred"></a>Ett oväntat fel uppstod

[Samla in agenten loggar](#collecting-pass-through-authentication-agent-logs) från hello server och kontakta Microsoft Support med ditt problem.

## <a name="authentication-agent-uninstallation-issues"></a>Problem med autentisering Agent avinstallation

### <a name="warning-message-when-uninstalling-azure-ad-connect"></a>Varning vid avinstallation av Azure AD Connect

Om du har direkt-autentisering aktiverad på din klient och försök toouninstall Azure AD Connect, den visar du hello följande varningsmeddelande: ”användare kommer inte att kunna toosign i tooAzure AD såvida du inte har andra direkt autentisering agenter installeras på andra servrar ”.

Se till att inställningarna är [hög tillgänglighet](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-5-ensure-high-availability) innan du avinstallerar Azure AD Connect tooavoid bryter användarinloggning.

## <a name="issues-with-enabling-hello-feature"></a>Problem med att aktivera funktionen hello

### <a name="enabling-hello-feature-failed-because-there-were-no-authentication-agents-available"></a>Aktivera hello funktionen misslyckades eftersom det fanns inga agenter för autentisering

Du behöver toohave minst en aktiv autentiseringsagent tooenable direkt-autentisering på din klient. Du kan installera en Agent för autentisering genom att installera Azure AD Connect eller en fristående autentiseringsagent.

### <a name="enabling-hello-feature-failed-due-tooblocked-ports"></a>Aktivera hello funktionen misslyckades på grund av tooblocked portar

Kontrollera hello servern där Azure AD Connect är installerat kan kommunicera med vår tjänst-URL: er och portar [här](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-1-check-prerequisites).

### <a name="enabling-hello-feature-failed-due-tootoken-or-account-authorization-errors"></a>Aktivera hello funktionen misslyckades på grund av tootoken eller konto auktorisering fel

Kontrollera att du använder ett globalt administratörskonto endast molnbaserad när du aktiverar hello-funktionen. Det är ett känt problem med multifaktorautentisering (MFA)-aktiverad globala administratörskonton; inaktivera MFA tillfälligt (endast toocomplete hello åtgärden) som en lösning.

## <a name="exchange-activesync-configuration-issues"></a>Problem med Exchange ActiveSync-konfiguration

Dessa är hello vanliga problem när du konfigurerar Exchange ActiveSync-stöd för direkt-autentisering.

### <a name="exchange-powershell-issue"></a>Exchange PowerShell problemet

Om du ser hello ”**går inte att hitta en parameter som matchar parameternamnet 'PerTenantSwitchToESTSEnabled'\.**” fel när du kör hello `Set-OrganizationConfig` Exchange PowerShell kommandot, kontakta Microsoft Support.

### <a name="exchange-activesync-not-working"></a>Exchange ActiveSync fungerar inte

hello konfigurationen att gälla vissa tid tootake - hello tidsperiod beror på din miljö. Kontakta Microsoft Support om hello situationen kvarstår under lång tid.

## <a name="collecting-pass-through-authentication-agent-logs"></a>Autentiseringsagent för att samla in direkt loggar

Beroende på hello typ av problem som du kan ha, behöver du toolook på olika platser för vidarekoppling autentiseringsagent loggar.

### <a name="authentication-agent-event-logs"></a>Autentisering Agent händelseloggar

För fel relaterade toohello autentiseringsagent, öppna upp hello Loggboken program på hello-servern och kontrollera **program och tjänsten Logs\Microsoft\AzureAdConnect\AuthenticationAgent\Admin**.

Aktivera hello ”” sessionsloggen för detaljerad analys. Kör inte hello autentiseringsagent med den här loggen aktiveras under normal drift; Använd endast för felsökning. Hej logginnehållet visas bara när hello loggen är inaktiverad igen.

### <a name="detailed-trace-logs"></a>Detaljerad spårningsloggar

tootroubleshoot användare logga in fel, leta efter spårningsloggar på **%programdata%\Microsoft\Azure AD ansluta autentisering Agent\Trace\\**. Loggarna finns orsakerna till en specifik användare logga in att inte funktionen för hello direkt-autentisering. Dessa fel är också mappade toohello inloggningsfel orsaker som visas i föregående hello [tabellen](#sign-in-failure-reasons-on-the-Azure-portal). Följande är ett exempel på post i loggen:

```
    AzureADConnectAuthenticationAgentService.exe Error: 0 : Passthrough Authentication request failed. RequestId: 'df63f4a4-68b9-44ae-8d81-6ad2d844d84e'. Reason: '1328'.
        ThreadId=5
        DateTime=xxxx-xx-xxTxx:xx:xx.xxxxxxZ
```

Du kan visa beskrivande information för hello-fel ('1328' i föregående exempel hello) genom att öppna hello-kommandotolk och kör hello följande kommando (Obs: Ersätt '1328' med hello faktiska felnummer som visas i loggarna):

`Net helpmsg 1328`

![Direktautentisering](./media/active-directory-aadconnect-pass-through-authentication/pta3.png)

### <a name="domain-controller-logs"></a>Domänkontrollanten loggar

Om granskningsloggning är aktiverad, kan ytterligare information finns i hello säkerhetsloggar domänkontrollanter. Ett enkelt sätt tooquery inloggning förfrågningar som skickas av direkt autentisering agenter är följande:

```
    <QueryList>
    <Query Id="0" Path="Security">
    <Select Path="Security">*[EventData[Data[@Name='ProcessName'] and (Data='C:\Program Files\Microsoft Azure AD Connect Authentication Agent\AzureADConnectAuthenticationAgentService.exe')]]</Select>
    </Query>
    </QueryList>
```

### <a name="performance-monitor-counters"></a>Prestandaräknare

Ett annat sätt toomonitor autentisering agenter är tootrack specifika prestandaräknare på varje server där hello Authentication Agent är installerad. Använd hello följande globala räknare (**# Tereftalsyra autentiseringar**, **#PTA misslyckades autentiseringar** och **#PTA lyckade autentiseringar**) och fel räknare (**# Tereftalsyra autentiseringsfel**):

![Direkt-autentisering prestandaräknare](./media/active-directory-aadconnect-pass-through-authentication/pta12.png)

>[!IMPORTANT]
>Direkt-autentisering ger hög tillgänglighet med hjälp av flera autentisering agenter och _inte_ belastningsutjämning. Beroende på din konfiguration _inte_ alla autentisering agenter får ungefär _lika_ antal begäranden. Det är möjligt att en specifik autentiseringsagent får ingen trafik alls.
