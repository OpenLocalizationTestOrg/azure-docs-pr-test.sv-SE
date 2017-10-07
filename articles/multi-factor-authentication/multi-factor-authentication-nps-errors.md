---
title: "aaaTroubleshoot felkoder för hello Azure MFA NPS-tillägg | Microsoft Docs"
description: "Få hjälp med att lösa problem med hello NPS-tillägg för Azure Multi-Factor Authentication med specifika lösningar på vanliga felmeddelanden"
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: 8b602954364c6e9f801d86edca6432bd8446c58b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="resolve-error-messages-from-hello-nps-extension-for-azure-multi-factor-authentication"></a>Lösa felmeddelanden från hello NPS-tillägg för Azure Multi-Factor Authentication

Om det uppstår fel med hello NPS-tillägg för Azure Multi-Factor Authentication Använd den här artikeln tooreach upplösningen snabbare. 

## <a name="troubleshooting-steps-for-common-errors"></a>Felsökning av vanliga fel

| Felkod | Felsökningssteg |
| ---------- | --------------------- |
| **CONTACT_SUPPORT** | [Kontakta supporten](#contact-microsoft-support), och nämnt hello lista över steg för att samla in loggar. Ange så mycket information som du kan om vad som hänt innan hello fel, inklusive klient-id och användarens huvudnamn (UPN). |
| **CLIENT_CERT_INSTALL_ERROR** | Det kan finnas ett problem med hur hello klientcertifikat installerades eller som är associerade med din klient. Följ anvisningarna hello i [felsökning hello MFA NPS tillägget](multi-factor-authentication-nps-extension.md#troubleshooting) tooinvestigate cert klientproblem. |
| **ESTS_TOKEN_ERROR** | Följ instruktionerna för hello i [felsökning hello MFA NPS tillägget](multi-factor-authentication-nps-extension.md#troubleshooting) tooinvestigate klienten cert och ADAL token problem. |
| **HTTPS_COMMUNICATION_ERROR** | hello NPS-servern är tooreceive svar från Azure MFA. Kontrollera att dina brandväggar är öppet åt två håll för trafik tooand från https://adnotifications.windowsazure.com |
| **HTTP_CONNECT_ERROR** | Kontrollera att du kan nå https://adnotifications.windowsazure.com och https://login.microsoftonline.com/ på hello-server som kör NPS-tillägget för hello. Om dessa platser inte ladda, felsöka anslutning på den servern. |
| **REGISTRY_CONFIG_ERROR** | En nyckel saknas i hello registret för programmet hello, vilket kan bero på att hello [PowerShell-skript](multi-factor-authentication-nps-extension.md#install-the-nps-extension) inte kör efter installationen. hello felmeddelande bör innehålla hello nyckel som saknas. Kontrollera att du har hello nyckel under HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\AzureMfa. |
| **REQUEST_FORMAT_ERROR** <br> RADIUS-begäran saknar det obligatoriska attributet för Radius-userName\Identifier. Kontrollera att NPS får RADIUS-begäranden | Det här felet visar vanligtvis ett installationsproblem. hello NPS tillägget måste vara installerad i NPS-servrar som kan ta emot RADIUS-begäranden. NPS-servrar som är installerade som beroenden för tjänster som Fjärrskrivbordsgateway och RRAS inte ta emot radius-begäranden. NPS-tillägg fungerar inte när du installerar över sådana installationer och fel eftersom det går inte att läsa hello information från hello autentiseringsbegäran. |
| **REQUEST_MISSING_CODE** | Kontrollera att hello lösenord krypteringsprotokollet mellan hello NPS- och NAS-servrar stöder hello sekundära autentiseringsmetoden som du använder. **PAP** stöder alla hello-autentiseringsmetoder för Azure MFA i molnet hello: telefonsamtal, enkelriktade SMS, mobilappavisering och Verifieringskod för mobila appar. **CHAPv2** och **EAP** stöd för telefonsamtal och mobilappavisering. |
| **USERNAME_CANONICALIZATION_ERROR** | Kontrollera att hello användare finns i din lokala Active Directory-instans och det hello NPS-tjänsten har behörigheter tooaccess hello katalog. Om du använder förtroenden mellan skogar, [supporten](#contact-microsoft-support) för ytterligare hjälp. |


   

### <a name="alternate-login-id-errors"></a>Alternativt inloggnings-ID-fel

| Felkod | Felmeddelande | Felsökningssteg |
| ---------- | ------------- | --------------------- |
| **ALTERNATE_LOGIN_ID_ERROR** | Fel: userObjectSid sökning misslyckades | Kontrollera att användaren hello finns i din lokala Active Directory-instans. Om du använder förtroenden mellan skogar, [supporten](#contact-microsoft-support) för ytterligare hjälp. |
| **ALTERNATE_LOGIN_ID_ERROR** | Fel: Det gick inte att alternativa LoginId sökning | Kontrollera att LDAP_ALTERNATE_LOGINID_ATTRIBUTE är tooa [giltigt active directory-attributet](https://msdn.microsoft.com/library/ms675090(v=vs.85).aspx). <br><br> Om LDAP_FORCE_GLOBAL_CATALOG anges tooTrue eller LDAP_LOOKUP_FORESTS har konfigurerats med ett tomt värde, kontrollera att du har konfigurerat en Global katalog och hello AlternateLoginId attributet läggs tooit. <br><br> Om LDAP_LOOKUP_FORESTS är konfigurerad med ett tomt värde, kan du kontrollera att hello-värdet är korrekt. Om det finns fler än en skogsnamnet, måste hello namn vara avgränsade med semikolon, inte blanksteg. <br><br> Om stegen inte löser problemet hello [supporten](#contact-microsoft-support) mer hjälp. |
| **ALTERNATE_LOGIN_ID_ERROR** | Fel: Alternativ LoginId värdet är tomt | Kontrollera hello AlternateLoginId attributet har konfigurerats för hello användare. |


## <a name="errors-your-users-may-encounter"></a>Fel kan uppstå när dina användare

| Felkod | Felmeddelande | Felsökningssteg |
| ---------- | ------------- | --------------------- |
| **AccessDenied** | Anroparen klienten har inte behörighet toodo autentisering för hello användare | Kontrollera om hello klientdomänen och hello domän för hello UPN (user Principal name) är hello samma. Till exempel se till att user@contoso.com försöker tooauthenticate toohello Contoso-klienten. hello UPN representerar en giltig användare för hello klient i Azure. |
| **AuthenticationMethodNotConfigured** | hello anges vilken autentiseringsmetod du valde inte har konfigurerats för hello användare | Hello användare lägga till eller verifiera sina verifieringsmetoderna enligt toohello instruktionerna i [hantera inställningar för tvåstegsverifiering](./end-user/multi-factor-authentication-end-user-manage-settings.md). |
| **AuthenticationMethodNotSupported** | Angiven autentiseringsmetod stöds inte. | Samla in alla loggfiler som innehåller det här felet och [supporten](#contact-microsoft-support). När du kontaktar supporten ange hello användarnamn och hello sekundära verifieringsmetod som utlöste hello-fel. |
| **BecAccessDenied** | MSODS Bec-anropet returnerade åtkomst nekad, förmodligen hello användarnamnet har inte definierats i hello-klient | hello användare finns i Active Directory lokalt men har synkroniserats inte till Azure AD med AD Connect. Eller hello användaren saknas för hello-klient. Lägg till hello användaren tooAzure AD och be dem lägga till sina verifieringsmetoderna enligt toohello instruktionerna i [hantera inställningar för tvåstegsverifiering](./end-user/multi-factor-authentication-end-user-manage-settings.md). |
| **InvalidFormat** eller **StrongAuthenticationServiceInvalidParameter** | hello telefonnumret är i ett okänt format | Låt hello användare korrigera deras telefonnummer för verifiering. |
| **InvalidSession** | hello angivna sessionen är ogiltig eller har gått ut | hello-sessionen har trätt i mer än tre minuter toocomplete. Kontrollera hello användaren att ange hello Verifieringskod eller svarar toohello appmeddelande inom tre minuter för inledande hello autentiseringsbegäran. Om detta inte löser problemet hello måste du kontrollera att det inte finns några nätverksfördröjningar mellan klienten, NAS-Server, NPS-servern och hello Azure MFA endpoint.  |
| **NoDefaultAuthenticationMethodIsConfigured** | Ingen standardautentiseringsmetod har konfigurerats för hello användare | Hello användare lägga till eller verifiera sina verifieringsmetoderna enligt toohello instruktionerna i [hantera inställningar för tvåstegsverifiering](./end-user/multi-factor-authentication-end-user-manage-settings.md). Kontrollera hello användaren har valt en standardmetoden för autentisering och konfigurerats som metod för sitt konto. |
| **OathCodePinIncorrect** | Fel kod och angiven PIN-kod. | Det här felet förväntas inte i hello NPS-tillägget. Om användaren påträffar, [supporten](#contact-microsoft-support) för hjälp med felsökning. |
| **ProofDataNotFound** | Bevis data har inte konfigurerats för hello angetts autentiseringsmetod. | Hello användare prova en annan verifieringsmetod eller Lägg till en ny verifieringsmetoderna enligt toohello instruktionerna i [hantera inställningar för tvåstegsverifiering](./end-user/multi-factor-authentication-end-user-manage-settings.md). Om hello användaren fortfarande toosee felet när du har bekräftat att deras verifieringsmetod är korrekt konfigurerade, [supporten](#contact-microsoft-support). |
| **SMSAuthFailedWrongCodePinEntered** | Fel kod och angiven PIN-kod. (OneWaySMS) | Det här felet förväntas inte i hello NPS-tillägget. Om användaren påträffar, [supporten](#contact-microsoft-support) för hjälp med felsökning. |
| **TenantIsBlocked** | Klienten är blockerad | [Kontakta supporten](#contact-microsoft-support) med katalog-ID från hello Azure AD-egenskapssidan i hello Azure-portalen. |
| **UserNotFound** | hello användaren hittades inte | hello-klienten inte längre visas som aktiv i Azure AD. Kontrollera att din prenumeration är aktiv och att du har hello obligatoriska appar från första part. Kontrollera också att hello klient i hello certifikatets ämne är som förväntat och hello cert fortfarande är giltig och registrerad under hello tjänstens huvudnamn. |

## <a name="messages-your-users-may-encounter-that-arent-errors"></a>Meddelandena som användarna kan stöta på som inte fel

Ibland kan kan användarna få meddelanden från Multi-Factor Authentication eftersom deras autentiseringsbegäran misslyckades. Dessa är inte fel i hello produkten av konfigurationen, men är avsiktlig varningar som förklarar varför begäran om autentisering nekades.

| Felkod | Felmeddelande | Rekommenderade åtgärder | 
| ---------- | ------------- | ----------------- |
| **OathCodeIncorrect** | Fel kod entered\OATH kod felaktig | Användaren har angett fel kod inte ett fel. | hello användaren angav fel hello-kod. Låt användaren prova igen genom att begära en ny kod eller logga in igen. | 
| **SMSAuthFailedMaxAllowedCodeRetryReached** | Högsta tillåtna kod försök nå | hello användaren kunde inte hello verifiering utmaning för många gånger. Beroende på dina inställningar, kanske de måste toobe avblockerad nu av en administratör.  |
| **SMSAuthFailedWrongCodeEntered** | Fel kod anges/Text meddelandet OTP felaktigt | hello användaren angav fel hello-kod. Låt användaren prova igen genom att begära en ny kod eller logga in igen. |

## <a name="errors-that-require-support"></a>Fel som kräver stöd

Om det uppstår något av dessa fel, rekommenderar vi att du [supporten](#contact-microsoft-support) diagnostiska hjälp. Det finns ingen standarduppsättning steg som kan åtgärda dessa fel. När du kontaktar supporten, vara säker på att tooinclude så mycket information som möjligt om hello steg som ledde tooan fel och klient-information.

| Felkod | Felmeddelande |
| ---------- | ------------- |
| **InvalidParameter** | Begäran får inte vara null |
| **InvalidParameter** | ObjectId får inte vara null eller tomt för ReplicationScope: {0} |
| **InvalidParameter** | Hej längden på CompanyName \{0} \ är längre än hello högsta tillåtna längden {1} |
| **InvalidParameter** | UserPrincipalName får inte vara null eller tomt |
| **InvalidParameter** | hello angivna klient-ID inte har rätt format |
| **InvalidParameter** | Sessions-ID får inte vara null eller tomt |
| **InvalidParameter** | Det gick inte att matcha alla ProofData från begäran eller Msods. Hej ProofData är okänd |
| **InternalError** |  |
| **OathCodePinIncorrect** |  |
| **VersionNotSupported** |  |
| **MFAPinNotSetup** |  |

## <a name="next-steps"></a>Nästa steg

### <a name="troubleshoot-user-accounts"></a>Felsöka användarkonton

Om användarna är [har problem med tvåstegsverifiering](./end-user/multi-factor-authentication-end-user-troubleshoot.md), hjälpa dem själv diagnosticera problem. 

### <a name="contact-microsoft-support"></a>Kontakta Microsoft support

Om du behöver ytterligare hjälp kan du kontakta supportpersonalen via [stöd för Azure Multi-Factor Authentication-servern](https://support.microsoft.com/oas/default.aspx?prid=14947). När du kontaktar oss är det bra om du kan ange så mycket information om problemet som möjligt. Information som du kan ange innehåller hello sida där du såg hello fel, hello felkod kod, hello specifika sessions-ID, hello-ID för hello användare som såg hello fel, loggar och felsökningsloggar.

toocollect felsökningsloggar för support diagnostics, Använd hello följande steg: 

1. Öppna en administratörskommandotolk och kör följande kommandon:

   ```
   Mkdir c:\NPS
   Cd NPS
   netsh trace start Scenario=NetConnection capture=yes tracefile=c:\NPS\nettrace.etl
   logman create trace "NPSExtension" -ow -o c:\NPS\NPSExtension.etl -p {7237ED00-E119-430B-AB0F-C63360C8EE81} 0xffffffffffffffff 0xff -nb 16 16 -bs 1024 -mode Circular -f bincirc -max 4096 -ets
   logman update trace "NPSExtension" -p {EC2E6D3A-C958-4C76-8EA4-0262520886FF} 0xffffffffffffffff 0xff -ets
   ```

2. Återskapa problemet hello

3. Stoppa hello spårning med följande kommandon:

   ```
   logman stop "NPSExtension" -ets
   netsh trace stop
   wevtutil epl AuthNOptCh C:\NPS\%computername%_AuthNOptCh.evtx
   wevtutil epl AuthZOptCh C:\NPS\%computername%_AuthZOptCh.evtx
   wevtutil epl AuthZAdminCh C:\NPS\%computername%_AuthZAdminCh.evtx
   Start .
   ```

4. ZIP-hello innehållet i hello C:\NPS mapp och koppla hello zippade filen toohello supportärende.


