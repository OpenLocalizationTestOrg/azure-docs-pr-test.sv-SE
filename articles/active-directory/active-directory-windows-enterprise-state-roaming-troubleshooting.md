---
title: "Felsökning av Enterprise tillstånd centrala inställningar i Azure Active Directory | Microsoft Docs"
description: "Ger svar toosome frågor IT-administratörer kan ha om inställningar och data appsynkronisering."
services: active-directory
keywords: "Enterprise tillstånd centrala inställningar för windows-moln, vanliga frågor och svar för enterprise tillstånd centrala"
documentationcenter: 
author: tanning
manager: swadhwa
editor: 
ms.assetid: f45d0515-99f7-42ad-94d8-307bc0d07be5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: markvi
ms.openlocfilehash: 4636d016983426e30d696459f54167b8ad2babcd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
#<a name="troubleshooting-enterprise-state-roaming-settings-in-azure-active-directory"></a>Felsökning av Enterprise tillstånd centrala inställningar i Azure Active Directory

Det här avsnittet innehåller information om hur tootroubleshoot och diagnostisera problem med Enterprise tillstånd centrala och visar en lista över kända problem.

## <a name="preliminary-steps-for-troubleshooting"></a>Förberedande steg för felsökning 
Kontrollera att hello användare och enhet har konfigurerats korrekt och att alla krav för hello för företag tillstånd nätverksväxling uppfylls av hello enhets- och hello innan du påbörjar felsökningen. 

1. Windows 10 med hello senaste uppdateringar, och en lägsta Version 1511 (OS-version 10586 eller senare) har installerats på hello enhet. 
2. hello-enheten är Azure AD ansluten, eller domänansluten och registrerad med Azure AD.
3. I Azure Active Directory-portalen hello **Enterprise tillstånd centrala** har aktiverats för hello-katalogen under **konfigurera** > **enheter**  >  **Användarna kan synkronisera inställningar och företagets AppData**. Alla användare har valts, eller hello användaren är aktiverad för att synkronisera via hello som valts och ingår i hello säkerhetsgrupp.
4. hello användare har en tilldelad toothem Azure Active Directory Premium-prenumeration.  
5. hello enheten har startats och hello användaren har loggat in när Enterprise tillstånd nätverksväxling har aktiverats.

## <a name="information-tooinclude-when-you-need-help"></a>Information tooinclude när du behöver hjälp
Om du inte kan lösa problemet med hello vägledning nedan kan du kontakta vår supportpersonal. När du kontaktar dem bör tooinclude hello följande information:

- **Allmän beskrivning av hello fel** – finns det felmeddelanden som visas av hello användare? Om det fanns inget felmeddelande, beskriver hello oväntade resultat som du tänkt i detalj. Vilka funktioner är aktiverade för synkronisering och vad är hello användare förväntas toosync? Flera funktioner inte synkroniserar eller är it isolerade tooone?
- **Användare som påverkas** – är synkronisering fungerar/misslyckas för en användare eller flera användare? Hur många enheter ingår per användare? Dem inte synkroniserar alla eller några av dem synkroniserar och vissa inte synkroniserar?
- **Information om hello användare** – vilka identiteten är hello användare med hjälp av toolog i toohello enhet? Hur är hello loggning av användaråtkomst i toohello enhet? De är en del av en vald säkerhetsgrupp tillåtna toosync? 
- **Information om hello enhet** – är den här enheten anslutits med Azure AD eller ansluten till domänen? Vilken version är hello enhet på? Vad är hello senaste uppdateringar?
- **Datum / tid / tidszonen** – vad var hello exakt datum och tid som du såg hello-fel (inklusive hello tidszon)?
- Inklusive den här informationen hjälper oss att lösa problemet så snabbt som möjligt.

## <a name="troubleshooting-and-diagnosing-issues"></a>Felsökning och diagnos av problem
Det här avsnittet innehåller förslag på hur tootroubleshoot och diagnostisera problem relaterade tooEnterprise tillstånd nätverksväxling.

## <a name="verify-sync-and-hello-sync-your-settings-settings-page"></a>Kontrollerar synkronisering och hello ”synkronisera dina inställningar” inställningssidan 

1. Efter anslutning av din Windows 10-datorer tooa konfigurerad domänen som är tooallow Enterprise tillstånd Roaming, logga in med ditt arbetskonto. Gå för**inställningar** > **konton** > **din synkroniseringsinställningar** och bekräfta att enskilda inställningar för synkronisering och hello finns på, och den hello överkant hello inställningssidan anger att du synkroniserar med ditt arbetskonto. Bekräfta hello samma konto används också som ditt inloggningskonto i **inställningar** > **konton** > **din Info**. 
2. Kontrollera att synkronisering fungerar på flera datorer genom att göra några ändringar på hello ursprungliga dator, till exempel flytta hello Aktivitetsfältet toohello högra eller översta sida hello-skärmen. Titta på hello ändra sprida toohello andra dator inom fem minuter. 
 - Låsning och upplåsning hello-skärmen (Win + L) kan hjälpa till att utlösa en synkronisering.
 - Du måste använda hello samma inloggningskonto på båda datorerna för synkronisering toowork – som Enterprise tillstånd nätverksväxling är bundet toohello användarkontot och inte hello datorkontot.

**Potentiella problem**: hello inställningssidan har hello växlar nedtonad och i stället för ett konto, du visas hello text ”vissa Windows-funktioner är endast tillgängliga om du använder ett Microsoft-konto eller arbetskonto”. Det här problemet kan uppstå för enheter som har ställts in toobe domänansluten och registrerad tooAzure AD, men hello enheten har inte autentiserats tooAzure AD. En möjlig orsak är att hello enhetsprincip måste användas, men det här programmet sker asynkront, och kan vara fördröjd med några timmar. tooverify problemet, följ hello stegen i Kontrollera hello enheten registreringen status toocheck hello fall.

### <a name="verify-hello-device-registration-status"></a>Kontrollera registreringsstatus för hello-enhet
Enterprise tillstånd centrala kräver hello enheten toobe registrerad med Azure AD. Även om de inte specifikt tooEnterprise tillstånd centrala hello instruktionerna nedan hjälper dig att bekräfta att hello Windows 10-klient är registrerad och bekräfta tumavtrycket URL för Azure AD-inställningar, ngc, Rapportera status och annan information.

1.  Öppna hello kommandotolk upphöjt. toodo detta i Windows, öppna hello kör programstart (Win + R) och Skriv ”cmd” tooopen.
2.  När hello kommandotolk är öppen skriver ”*dsregcmd.exe/status*”.
3.  Förväntad utdata hello **AzureAdJoined** fältvärdet ska vara ”Ja” **WamDefaultSet** fältvärdet ska vara ”Ja” och hello **WamDefaultGUID** fältvärdet ska vara en GUID med ”(AzureAd)” hello slutet.

**Potentiella problem**: **WamDefaultSet** och **AzureAdJoined** både har ”ingen” i hello fältvärde hello enheten var ansluten till domänen och registrerad med Azure AD och hello enheten synkroniserar inte. Om det visas detta hello enheten behöver toowait för principen toobe tillämpas eller hello-autentisering för hello enheten misslyckades vid anslutningen tooAzure AD. hello användare ha toowait några timmar innan hello princip toobe tillämpas. Andra felsökningssteg kan omfatta du försöker automatisk registrering av signering ut och in igen eller starta hello uppgift i Schemaläggaren. I vissa fall, kör ”*dsregcmd.exe /leave*” i ett Kommandotolk-fönster startas om och försök registrera igen får hjälp med problemet.


**Potentiella problem**: hello fält för **AzureAdSettingsUrl** är tomt och hello enheten fungerar inte synkronisering. hello användaren kan ha senast loggade in toohello enheten innan Enterprise tillstånd nätverksväxling har aktiverats i hello Azure Active Directory-portalen. Starta om hello enhet och har hello användarinloggning. Försök eventuellt med hello IT-administratören inaktivera och återaktivera användare kan synkroniseringsinställningar och företagsdata för appen i hello-portalen. När aktiveras omstart hello enhet och har hello användarinloggning. Om detta inte löser problemet hello **AzureAdSettingsUrl** kan vara tom i hello fallet med ett felaktigt certifikat. I det här fallet kör ”*dsregcmd.exe /leave*” i ett Kommandotolk-fönster startas om och försök registrera igen får hjälp med problemet.

## <a name="enterprise-state-roaming-and-multi-factor-authentication"></a>Enterprise tillstånd centrala och Multifaktorautentisering 
Under vissa förhållanden kan Enterprise tillstånd centrala misslyckas toosync data om Azure Multi-Factor Authentication har konfigurerats. Ytterligare information om problemen finns hello stöddokument [KB3193683](https://support.microsoft.com/kb/3193683). 

**Potentiella problem**: om enheten är konfigurerade toorequire Multi-Factor Authentication på hello Azure Active Directory-portalen, du kan inte toosync inställningar när du loggar in tooa Windows 10-enhet med ett lösenord. Den här typen av Multi-Factor Authentication-konfigurationen är avsedda tooprotect administratörskonto Azure. Administratörer kan fortfarande vara kan toosync genom att logga in tootheir Windows 10-enheter med sina Microsoft Passport för arbetsplats PIN-kod eller genom att fylla i Multi-Factor Authentication vid åtkomst till andra Azure-tjänster som Office 365.

**Potentiella problem**: synkronisering kan misslyckas om Hej administratör konfigurerar principen för villkorlig åtkomst hello Active Directory Federation Services Multifaktorautentisering och hello åtkomst-token på hello enhet upphör att gälla. Se till att du logga in och logga ut med hello Microsoft Passport för arbetsplats PIN-kod eller slutföra Multifaktorautentisering vid åtkomst till andra Azure-tjänster som Office 365.

###<a name="event-viewer"></a>Loggboken
Vid avancerad felsökning kan vara används toofind felen. Dessa dokumenteras i hello nedan. hello-händelser finns under Loggboken > program- och tjänstloggar > **Microsoft** > **Windows** > **SettingSync** och identity-relaterade problem med synkronisering av **Microsoft** > **Windows** > **Azure AD**.


## <a name="known-issues"></a>Kända problem

### <a name="sync-does-not-work-on-devices-that-have-apps-side-loaded-using-mdm-software"></a>Synkronisering fungerar inte på enheter som har appar sidoladdad via MDM-programvara

Påverkar enheter som kör hello Windows 10 årsdagar Update (Version 1607). I Loggboken under hello SettingSync Azure loggar visas hello händelse-ID 6013 med fel 80070259 ofta.

**Rekommenderad åtgärd**  
Kontrollera att hello Windows 10 v1607 klienten har hello 23 augusti 2016 kumulativa uppdateringen ([KB3176934](https://support.microsoft.com/kb/3176934) OS skapa 14393.82). 

---

### <a name="internet-explorer-favorites-do-not-sync"></a>Favoriter i Internet Explorer är inte synkroniserade

Påverkar enheter som kör hello Windows 10 November Update (Version 1511).

**Rekommenderad åtgärd**  
Kontrollera att hello Windows 10 v1511 klienten har hello juli 2016 kumulativa uppdateringen ([KB3172985](https://support.microsoft.com/kb/3172985) OS skapa 10586.494).

---

### <a name="theme-is-not-syncing-as-well-as-data-protected-with-windows-information-protection"></a>Temat synkroniserar inte, samt data som skyddas av Windows informationsskydd 

tooprevent data sprids, data som skyddas med [Windows informationsskydd](https://technet.microsoft.com/itpro/windows/keep-secure/protect-enterprise-data-using-wip) kommer inte att synkronisera via Enterprise tillstånd centrala för enheter med hjälp av hello Windows 10 årsdagar Update.



**Rekommenderad åtgärd**  
Ingen. Framtida uppdateringar tooWindows kanske kan lösa problemet.

---

### <a name="date-time-and-region-settings-do-not-sync-on-domain-joined-device"></a>Inställningar för datum, tid och regionen är inte synkroniserade på domänansluten enhet 
  
Enheter som är anslutna till en domän får inte synkronisering för inställning av datum, tid och Region hello: automatisk tid. Använda automatisk tid kan åsidosättning hello andra inställningar för datum, tid och Region och orsaka dessa inställningar inte toosync. 

**Rekommenderad åtgärd**  
Ingen. 

---

### <a name="uac-prompts-when-syncing-passwords"></a>UAC uppmanar när synkroniserar lösenord

Påverkar enheter som kör hello konfigurerad Windows 10 November Update (Version 1511) med ett trådlöst nätverkskort som är toosync lösenord.

**Rekommenderad åtgärd**  
Kontrollera hello Windows 10 v1511 klienten har hello kumulativa uppdateringen ([KB3140743](https://support.microsoft.com/kb/3140743) OS skapa 10586.494).

---

### <a name="sync-does-not-work-on-devices-that-use-smart-card-for-login"></a>Synkronisering fungerar inte på enheter som använder smartkort för inloggning
Om du försöker toosign i tooyour Windows-enhet med ett smartkort eller virtuellt smartkort, upphöra synkronisera inställningar att fungera.     

**Rekommenderad åtgärd**  
Ingen. Framtida uppdateringar tooWindows kanske kan lösa problemet.

---

### <a name="domain-joined-device-is-not-syncing-after-leaving-corporate-network"></a>Domänansluten enhet synkroniserar inte efter lämnar företagets nätverk     
Domänanslutna enheter registrerade tooAzure AD kan uppstå synkroniseringfel vid hello enhet är extern för längre tid och domänautentisering kan inte slutföras.

**Rekommenderad åtgärd**  
Ansluta enheten hello tooa företagsnätverket så att synkroniseringen kan återupptas.

---

 ### <a name="azure-ad-joined-device-is-not-syncing-and-hello-user-has-a-mixed-case-user-principal-name"></a>Azure AD-ansluten enhet synkroniserar inte och hello användare har en blandad case UPN-namnet.
 Om hello användaren har ett UPN (t.ex. användarnamn i stället för användarnamn) med blandat skiftläge och hello användaren finns på en Azure AD-ansluten enhet som har uppgraderats från Windows 10 Build 10586 too14393, misslyckas hello användarens enhet toosync. 

**Rekommenderad åtgärd**  
hello användare behöver toounjoin och återansluta hello enhet toohello moln. toodo, inloggningen som hello lokal administratör och frånkoppling från hello enhet genom att gå för**inställningar** > **System** > **om** och välj ” Hantera eller koppla från arbetet eller skolan ”. Rensa hello filerna nedan och Azure AD Join hello enhet igen i **inställningar** > **System** > **om** och välja ”Anslut tooWork eller skola ”. Fortsätt toojoin hello enheten tooAzure Active Directory och fullständig hello flödet.

I hello rensning steget Rensa hello följande filer:
- Settings.dat i`C:\Users\<Username>\AppData\Local\Packages\Microsoft.AAD.BrokerPlugin_cw5n1h2txyewy\Settings\`
- Alla filer i hello hello i mappen`C:\Users\<Username>\AppData\Local\Packages\Microsoft.AAD.BrokerPlugin_cw5n1h2txyewy\AC\TokenBroker\Account`

---

### <a name="event-id-6065-80070533-this-user-cant-sign-in-because-this-account-is-currently-disabled"></a>Händelse-ID 6065:80070533 den här användaren inte logga in eftersom det här kontot är inaktiverat  
I Loggboken under hello SettingSync/Debug-loggar, kan du se det här felet när hello användarens autentiseringsuppgifter har upphört att gälla. Dessutom kan det inträffa när hello klient inte automatiskt har etablerats AzureRMS. 

**Rekommenderad åtgärd**  
Hello första fallet ha hello användare uppdaterar sina autentiseringsuppgifter och inloggningen toohello enheten med hello nya autentiseringsuppgifter. toosolve hello AzureRMS problemet fortsätta med hello stegen i [KB3193791](https://support.microsoft.com/kb/3193791). 

---

### <a name="event-id-1098-error-0xcaa5001c-token-broker-operation-failed"></a>Händelse-ID 1098: Fel: 0xCAA5001C Token broker åtgärden misslyckades  
I Loggboken under hello AAD/Operational loggar felet kan visas med händelsen 1104: AAD moln AP plugin-anropet Get-token returnerade fel: 0xC000005F. Det här problemet uppstår om det saknar behörighet eller ägarskap attribut.    

**Rekommenderad åtgärd**  
Fortsätt med steg hello [KB3196528](https://support.microsoft.com/kb/3196528).  



## <a name="next-steps"></a>Nästa steg

- Använd hello [User Voice forum](https://feedback.azure.com/forums/169401-azure-active-directory/category/158658-enterprise-state-roaming) tooprovide feedback och kontrollera förslag på hur tooimprove Enterprise tillstånd centrala.

- Mer information finns i hello [företagsroaming](active-directory-windows-enterprise-state-roaming-overview.md). 

## <a name="related-topics"></a>Relaterade ämnen
* [Övergripande översikt över Enterprise tillstånd](active-directory-windows-enterprise-state-roaming-overview.md)
* [Aktivera företagsroaming i Azure Active Directory](active-directory-windows-enterprise-state-roaming-enable.md)
* [Inställningar och dataroaming vanliga frågor och svar](active-directory-windows-enterprise-state-roaming-faqs.md)
* [Gruppen principer och MDM-inställningar för synkronisering av inställningar](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
* [Centrala referens för Windows 10](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
