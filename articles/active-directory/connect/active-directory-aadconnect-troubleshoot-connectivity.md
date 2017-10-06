---
title: "Azure AD Connect: Felsöka anslutningsproblem | Microsoft Docs"
description: "Förklarar hur tootroubleshoot anslutning problem med Azure AD Connect."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 3aa41bb5-6fcb-49da-9747-e7a3bd780e64
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 60d6b7c4ad8a3ab907c20e598ec9443f115df287
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-connectivity-issues-with-azure-ad-connect"></a>Felsökning av anslutningsproblem med Azure AD Connect
Den här artikeln förklarar hur anslutning mellan Azure AD Connect och AD Azure fungerar och hur tootroubleshoot anslutning problem. Dessa problem är sannolikt toobe som visas i en miljö med en proxyserver.

## <a name="troubleshoot-connectivity-issues-in-hello-installation-wizard"></a>Felsökning av anslutningsproblem i installationsguiden för hello
Azure AD Connect använder Modern autentisering (med hello ADAL-biblioteket) för autentisering. hello installationsguiden och hello Synkroniseringsmotorn rätt kräver machine.config toobe rätt konfigurerade, eftersom dessa två .NET-program.

I den här artikeln visar vi hur Fabrikam ansluter tooAzure AD via dess proxy. hello proxyserver heter fabrikamproxy och använder port 8080.

Vi måste först toomake till [ **machine.config** ](active-directory-aadconnect-prerequisites.md#connectivity) är korrekt konfigurerad.  
![machineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/machineconfig.png)

> [!NOTE]
> I vissa icke-Microsoft-bloggar dokumenteras det att ändringar ska göras toomiiserver.exe.config i stället. Den här filen är över på varje uppgradering även om det fungerar under inledande installationen hello system slutar att fungera på första uppgraderingen. Därför är hello rekommendation tooupdate machine.config i stället.
>
>

hello proxyserver måste också ha hello krävs webbadresser öppnas. hello officiella listan dokumenteras i [Office 365-URL: er och IP-adressintervall](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

Av dessa webbadresser är hello i den följande tabellen hello absolut utan minsta toobe kan tooconnect tooAzure AD. Den här listan innehåller inte några valfria funktioner, till exempel tillbakaskrivning av lösenord eller Azure AD Connect Health. Det är dokumenterade här toohelp vid felsökning för hello inledande konfiguration.

| URL: EN | Port | Beskrivning |
| --- | --- | --- |
| mscrl.microsoft.com |HTTP/80 |Använda toodownload CRL visas. |
| \*. verisign.com |HTTP/80 |Använda toodownload CRL visas. |
| \*. entrust.com |HTTP/80 |Använda toodownload CRL listar för MFA. |
| \*.windows.net |HTTPS/443 |Använda toosign i tooAzure AD. |
| Secure.aadcdn.microsoftonline p.com |HTTPS/443 |Används för MFA. |
| \*.microsoftonline.com |HTTPS/443 |Använda tooconfigure dina Azure AD-katalog och importera och exportera data. |

## <a name="errors-in-hello-wizard"></a>Fel i hello-guiden
hello installationsguiden använder två olika kontexter. På sidan hello **ansluta tooAzure AD**, den använder hello inloggad användare. På sidan hello **konfigurera**, ändring av toohello [konto som kör hello-tjänsten för hello Synkroniseringsmotorn](active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-account). Om det finns ett problem, visas det förmodligen redan vid hello **ansluta tooAzure AD** sida i guiden för hello eftersom hello proxykonfiguration är globala.

hello efter problem är hello de vanligaste felen som kan uppstå i hello installationsguiden.

### <a name="hello-installation-wizard-has-not-been-correctly-configured"></a>hello installationsguiden har inte konfigurerats korrekt
Det här felet uppstår när själva hello guiden inte kan nå hello proxy.  
![nomachineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/nomachineconfig.png)

* Om du ser felet Kontrollera hello [machine.config](active-directory-aadconnect-prerequisites.md#connectivity) har konfigurerats korrekt.
* Som verkar vara korrekta, gör om hello i [proxy verifiera](#verify-proxy-connectivity) toosee om hello problemet finns utanför samt hello-guiden.

### <a name="a-microsoft-account-is-used"></a>Ett Microsoft-konto används
Om du använder en **Microsoft-konto** snarare än en **Skol- eller organisation** konto, visas ett allmänt fel.  
![Ett Account används](./media/active-directory-aadconnect-troubleshoot-connectivity/unknownerror.png)

### <a name="hello-mfa-endpoint-cannot-be-reached"></a>att det går inte nå slutpunkten för hello MFA
Det här felet uppstår om hello endpoint **https://secure.aadcdn.microsoftonline-p.com** går inte att nå och den globala administratören har MFA är aktiverat.  
![nomachineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/nomicrosoftonlinep.png)

* Om du ser det här felet kan verifiera den hello slutpunkten **secure.aadcdn.microsoftonline p.com** har lagts till toohello proxy.

### <a name="hello-password-cannot-be-verified"></a>Det går inte att verifiera hello lösenord
Att om guiden för installation av hello lyckas ansluta tooAzure AD, men själva hello lösenordet går inte verifiera det här felet visas:  
![badpassword](./media/active-directory-aadconnect-troubleshoot-connectivity/badpassword.png)

* Är ett tillfälligt lösenord för hello lösenord och måste ändras? Är det verkligen hello rätt lösenord? Försök toosign i toohttps://login.microsoftonline.com (på en annan dator än hello Azure AD Connect-server) och kontrollera hello konto kan användas.

### <a name="verify-proxy-connectivity"></a>Kontrollera proxy-anslutningen
tooverify om hello Azure AD Connect-servern har Faktisk anslutningsbarhet med hello Proxy och Internet, använder du vissa PowerShell toosee om hello proxy tillåter webbegäranden eller inte. I PowerShell-Kommandotolken kör `Invoke-WebRequest -Uri https://adminwebservice.microsoftonline.com/ProvisioningService.svc`. (Tekniskt hello första anropet är toohttps://login.microsoftonline.com och den här URI fungerar också, men hello andra URI snabbare toorespond.)

PowerShell använder hello konfiguration i machine.config toocontact hello-proxy. hello inställningar i winhttp/netsh bör inte påverkar dessa cmdlets.

Om hello proxy är korrekt konfigurerad, du får en lyckad status: ![proxy200](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest200.png)

Om du får **tooconnect toohello fjärrservern**, PowerShell försöker toomake direkta samtal utan att använda hello proxy eller DNS är inte korrekt konfigurerad. Se till att hello **machine.config** filen är korrekt konfigurerad.
![unabletoconnect](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequestunable.png)

Om hello proxy inte är korrekt konfigurerad, du får ett felmeddelande: ![proxy200](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest403.png)
![proxy407](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest407.png)

| Fel | Feltext | Kommentar |
| --- | --- | --- |
| 403 |Tillåts inte |hello-proxy har inte öppnats för hello begärda URL: en. Besöker hello proxykonfiguration och kontrollerar att hello [URL: er](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) har öppnats. |
| 407 |Det krävs proxyautentisering |hello proxyserver krävs en inloggning och inget har tillhandahållits. Om proxyservern kräver autentisering, se till att toohave den här inställningen konfigurerad i hello machine.config. Kontrollera också att du använder domänkonton för hello-användaren som kör guiden hello och hello-tjänstkontot. |

## <a name="hello-communication-pattern-between-azure-ad-connect-and-azure-ad"></a>hello mönster för kommunikation mellan Azure AD Connect och Azure AD
Om du har följt de föregående stegen och fortfarande inte kan ansluta, kan du nu starta tittar på nätverket loggar. Det här avsnittet är dokumentera ett normalt och lyckad anslutning mönster. Det också en lista med vanliga red herrings som kan ignoreras när du har läst hello loggar för nätverket.

* Det finns anrop toohttps://dc.services.visualstudio.com. Det är inte obligatoriska toohave denna URL som är öppna i hello proxy för hello installation toosucceed och dessa anrop kan ignoreras.
* Du ser att dns-matchning visar hello faktiska värdar toobe i hello DNS-namnet utrymme nsatc.net och andra namnområden inte under microsoftonline.com. Men det finns inte någon webbtjänstbegäranden på hello faktiska servernamn och du har inte någon tooadd dessa URL: er toohello proxy.
* hello slutpunkter adminwebservice och provisioningapi är identifiering slutpunkter och används toofind hello faktisk slutpunkt toouse. De här slutpunkterna är olika beroende på region.

### <a name="reference-proxy-logs"></a>Referens för proxy-loggar
Här är en dump från en verklig proxy loggen och hello installationen guidesidan varifrån den togs (dubblettposter toohello samma slutpunkt har tagits bort). Det här avsnittet kan användas som referens för proxy- och loggarna. hello faktiska slutpunkter kan skilja sig i din miljö (särskilt dessa URL: er i *kursiv*).

**Ansluta tooAzure AD**

| Tid | URL: EN |
| --- | --- |
| 1/11/2016 8:31 |Connect://login.microsoftonline.com:443 |
| 1/11/2016 8:31 |Connect://adminwebservice.microsoftonline.com:443 |
| 1/11/2016 8:32 |ansluta: / /*bba800 fästpunkt*. microsoftonline.com:443 |
| 1/11/2016 8:32 |Connect://login.microsoftonline.com:443 |
| 1/11/2016 8:33 |Connect://provisioningapi.microsoftonline.com:443 |
| 1/11/2016 8:33 |ansluta: / /*bwsc02 relay*. microsoftonline.com:443 |

**Konfigurera**

| Tid | URL: EN |
| --- | --- |
| 1/11/2016 8:43 |Connect://login.microsoftonline.com:443 |
| 1/11/2016 8:43 |ansluta: / /*bba800 fästpunkt*. microsoftonline.com:443 |
| 1/11/2016 8:43 |Connect://login.microsoftonline.com:443 |
| 1/11/2016 8:44 |Connect://adminwebservice.microsoftonline.com:443 |
| 1/11/2016 8:44 |ansluta: / /*bba900 fästpunkt*. microsoftonline.com:443 |
| 1/11/2016 8:44 |Connect://login.microsoftonline.com:443 |
| 1/11/2016 8:44 |Connect://adminwebservice.microsoftonline.com:443 |
| 1/11/2016 8:44 |ansluta: / /*bba800 fästpunkt*. microsoftonline.com:443 |
| 1/11/2016 8:44 |Connect://login.microsoftonline.com:443 |
| 1/11/2016 8:46 |Connect://provisioningapi.microsoftonline.com:443 |
| 1/11/2016 8:46 |ansluta: / /*bwsc02 relay*. microsoftonline.com:443 |

**Inledande synkronisering**

| Tid | URL: EN |
| --- | --- |
| 1/11/2016 8:48 |Connect://login.Windows.NET:443 |
| 1/11/2016 8:49 |Connect://adminwebservice.microsoftonline.com:443 |
| 1/11/2016 8:49 |ansluta: / /*bba900 fästpunkt*. microsoftonline.com:443 |
| 1/11/2016 8:49 |ansluta: / /*bba800 fästpunkt*. microsoftonline.com:443 |

## <a name="authentication-errors"></a>Autentiseringsfel
Det här avsnittet beskriver fel som kan returneras från ADAL (hello-autentiseringsbibliotek som används av Azure AD Connect) och PowerShell. hello fel förklaras hjälper dig i Förstå nästa steg.

### <a name="invalid-grant"></a>Ogiltig bevilja
Ogiltigt användarnamn eller lösenord. Mer information finns i [hello lösenordet går inte att verifiera](#the-password-cannot-be-verified).

### <a name="unknown-user-type"></a>Okänd typ
Azure AD-katalogen kan inte hitta eller matcha. Kanske försök toologin med ett användarnamn i en overifierade domän?

### <a name="user-realm-discovery-failed"></a>Identifiering av startsfär användare misslyckades
Nätverks- och proxyinställningar konfigurationsproblem. hello-nätverket kan inte nås. Se [felsökning av anslutningsproblem i installationsguiden för hello](#troubleshoot-connectivity-issues-in-the-installation-wizard).

### <a name="user-password-expired"></a>Användarens lösenord har upphört att gälla
Dina autentiseringsuppgifter har upphört att gälla. Ändra ditt lösenord.

### <a name="authorizationfailure"></a>AuthorizationFailure
Okänt problem.

### <a name="authentication-cancelled"></a>Autentisering avbröts
Hej multifaktorautentisering (MFA) challenge avbröts.

### <a name="connecttomsonline"></a>ConnectToMSOnline
Autentiseringen lyckades, men Azure AD PowerShell har problem med autentisering.

### <a name="azurerolemissing"></a>AzureRoleMissing
Autentiseringen lyckades. Du är inte en global administratör.

### <a name="privilegedidentitymanagement"></a>PrivilegedIdentityManagement
Autentiseringen lyckades. Privileged identity management har aktiverats och du för närvarande är inte en global administratör. Mer information finns i [Privileged Identity Management](../active-directory-privileged-identity-management-getting-started.md).

### <a name="companyinfounavailable"></a>CompanyInfoUnavailable
Autentiseringen lyckades. Det gick inte att hämta företagets information från Azure AD.

### <a name="retrievedomains"></a>RetrieveDomains
Autentiseringen lyckades. Det gick inte att hämta domäninformation från Azure AD.

### <a name="unexpected-exception"></a>Ett oväntat undantag
Visas som ett oväntat fel i hello installationsguiden. Kan inträffa om du försöker toouse en **Account** snarare än en **Skol- eller organisation konto**.

## <a name="troubleshooting-steps-for-previous-releases"></a>Felsökningssteg för tidigare versioner.
Med versioner som börjar med build-nummer 1.1.105.0 (utgiven februari 2016) hello-inloggningsassistent har dragits tillbaka. Det här avsnittet och hello configuration bör inte längre behövs, men behålls som referens.

För hello enkel inloggning i assistenten toowork konfigureras winhttp. Den här konfigurationen kan göras med [ **netsh**](active-directory-aadconnect-prerequisites.md#connectivity).  
![Netsh](./media/active-directory-aadconnect-troubleshoot-connectivity/netsh.png)

### <a name="hello-sign-in-assistant-has-not-been-correctly-configured"></a>hello-inloggningsassistent har inte konfigurerats korrekt
Det här felet visas när hello inloggningsassistenten kan inte nå hello proxy eller hello proxy tillåter inte hello-begäran.
![nonetsh](./media/active-directory-aadconnect-troubleshoot-connectivity/nonetsh.png)

* Om du ser det här felet kan titta på hello proxykonfiguration i [netsh](active-directory-aadconnect-prerequisites.md#connectivity) och den är korrekt.
  ![netshshow](./media/active-directory-aadconnect-troubleshoot-connectivity/netshshow.png)
* Som verkar vara korrekta, gör om hello i [proxy verifiera](#verify-proxy-connectivity) toosee om hello problemet finns utanför samt hello-guiden.

## <a name="next-steps"></a>Nästa steg
Läs mer om hur du [integrerar dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).
