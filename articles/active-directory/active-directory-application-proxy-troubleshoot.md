---
title: aaaTroubleshoot Application Proxy | Microsoft Docs
description: Omfattar hur tootroubleshoot fel i Azure AD Application Proxy.
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 970caafb-40b8-483c-bb46-c8b032a4fb74
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: H1Hack27Feb2017; it-pro
ms.openlocfilehash: d426708a2ee32da6674987b5794602ed984b06b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-application-proxy-problems-and-error-messages"></a>Felsökning av problem med Application Proxy och felmeddelanden
Om fel uppstår i att komma åt ett publicerat program eller publicera program, kontrollera hello följande alternativ toosee om Microsoft Azure AD Application Proxy fungerar korrekt:

* Öppna hello Windows Services konsolen och kontrollera att hello **Microsoft AAD Application Proxy Connector** tjänsten är aktiverad och körs. Du kan också toolook på hello Application Proxy service egenskapssida som visas i följande bild hello:  
  ![Egenskaper för Microsoft AAD Application Proxy Connector skärmbild](./media/active-directory-application-proxy-troubleshoot/connectorproperties.png)
* Öppna Loggboken och leta efter Application Proxy connector-händelser i **program- och tjänstloggar** > **Microsoft** > **AadApplicationProxy** > **Connector** > **Admin**.
* Om det behövs mer detaljerade loggar är tillgängliga som [aktivera hello Application Proxy connector-sessionsloggar](application-proxy-understand-connectors.md#under-the-hood).

Läs mer om hello Azure AD-felsökningsverktyget [felsökning verktyget toovalidate connector nätverk krav](https://blogs.technet.microsoft.com/applicationproxyblog/2015/09/03/troubleshooting-tool-to-validate-connector-networking-prerequisites).

## <a name="hello-page-is-not-rendered-correctly"></a>hello sidan renderas inte korrekt
Du kan ha problem med programmet återgivning eller felaktigt fungerar utan att få felmeddelanden. Detta kan inträffa om du har publicerat hello artikel sökväg, men hello programmet kräver innehåll som finns utanför sökvägen.

Till exempel om du publicerar hello sökvägen https://yourapp/app men hello programmet anropar bilder i https://yourapp/media, återges de inte. Se till att publicera hello program med hjälp av hello högsta nivå sökvägen tooinclude måste alla relevant innehåll. Det skulle vara http://yourapp/ i det här exemplet.

Om du ändrar sökvägen tooinclude refererar till innehåll, men måste fortfarande användare tooland på en djupare länk i hello sökväg, finns i blogginlägget hello [inställningen hello rätt länk för Application Proxy-program i hello Azure AD-åtkomstpanelen och Office 365-app Starta](https://blogs.technet.microsoft.com/applicationproxyblog/2016/04/06/setting-the-right-link-for-application-proxy-applications-in-the-azure-ad-access-panel-and-office-365-app-launcher/).

## <a name="connector-errors"></a>Connector-fel

Använd hello [Azure AD Application Proxy Connector portar Test Tool](https://aadap-portcheck.connectorporttest.msappproxy.net/) tooverify att din connector kan nå hello Application Proxy-tjänsten. Se till att hello centrala USA region och hello region närmaste tooyou har alla gröna bockmarkeringarna minimum. Utöver det kan innebär mer gröna bockmarkeringarna större flexibilitet. 

Om registreringen misslyckas under hello installation av guiden, finns det två sätt tooview hello varför hello misslyckades. Antingen finns i händelseloggen för hello under **program och tjänster Logs\Microsoft\AadApplicationProxy\Connector\Admin**, eller kör hello följande Windows PowerShell-kommando:

    Get-EventLog application –source “Microsoft AAD Application Proxy Connector” –EntryType “Error” –Newest 1

När du har hittat hello Connector-fel från hello händelseloggen kan du använda den här tabellen för vanliga fel tooresolve hello problem:

| Fel | Rekommenderade åtgärder |
| ----- | ----------------- |
| Registreringen av anslutningsverktyget misslyckades: Kontrollera att du har aktiverat Application Proxy i hello Azure-hanteringsportalen och att du angett din Active Directory-användarnamn och lösenord korrekt. Fel: 'ett eller flera fel uppstod ”. | Om du har stängt hello registrering fönster utan att logga in tooAzure AD kör hello guiden igen och registrera hello Connector. <br><br> Om hello registrering fönstret öppnar och stänger sedan omedelbart utan att tillåta toolog i, får du förmodligen det här felet. Det här felet uppstår vid ett nätverksfel på datorn. Kontrollera att det är möjligt tooconnect från en webbläsare tooa offentliga webbplats och att hello portar är öppna som anges i [krav för Application Proxy](active-directory-application-proxy-enable.md). |
| Rensa fel visas i hello registrering fönster. Det går inte att fortsätta | Om du ser detta fel och sedan hello fönstret stängs, du har angett hello felaktigt användarnamn eller lösenord. Försök igen. |
| Registreringen av anslutningsverktyget misslyckades: Kontrollera att du har aktiverat Application Proxy i hello Azure-hanteringsportalen och att du angett din Active Directory-användarnamn och lösenord korrekt. Fel: ' AADSTS50059: ingen klient identifierande information finns i antingen hello-begäran eller underförstås av alla angivna autentiseringsuppgifter och Sök efter service principal URI har misslyckats. | Du försöker toosign in med ett Account och inte en domän som är en del av hello organisations-ID hello directory försöker tooaccess. Kontrollera att Hej administratör är en del av hello samma domännamn som hello klient domän, till exempel om hello Azure AD-domänen är contoso.com, Hej administratör ska vara admin@contoso.com. |
| Det gick inte tooretrieve hello aktuella körningsprincipen för att köra PowerShell-skript. | Om hello kopplingsinstallationen misslyckas, kontrollera toomake du att PowerShell-körningsprincipen inte är inaktiverad. <br><br>1. Öppna hello Redigeraren för Grupprincip.<br>2. Gå för**Datorkonfiguration** > **Administrationsmallar** > **Windows-komponenter**  >   **Windows PowerShell** och dubbelklicka på **aktivera skriptkörningen**.<br>3. hello körningsprincipen kan ställas in tooeither **inte konfigurerad** eller **aktiverad**. Om anges för**aktiverad**, se till att under Alternativ, hello körningsprincipen anges tooeither **Tillåt lokala skript och fjärranslutna signerade skript** eller för**alla skript**. |
| Connector kunde inte toodownload hello konfiguration. | hello Connector klientcertifikat som används för autentisering, upphört att gälla. Detta kan också inträffa om du har hello Connector bakom en proxyserver. I det här fallet hello Connector kan inte komma åt hello Internet och kommer inte att kunna tooprovide program tooremote användare. Förnya manuellt med hjälp av hello `Register-AppProxyConnector` cmdlet i Windows PowerShell. Om din koppling är bakom en proxyserver, är det nödvändigt toogrant Internet toohello Connector åtkomstkonton ”nätverkstjänster” och ”lokalt system”. Detta kan åstadkommas genom att ge dem åtkomst till toohello Proxy eller genom att ange dem toobypass hello proxy. |
| Registreringen av anslutningsverktyget misslyckades: Kontrollera att du är Global administratör för din Active Directory tooregister hello Connector. Fel: ' hello registreringsbegäran nekades ”. | hello-alias som du försöker toolog med är inte en administratör på den här domänen. Din Connector installeras alltid för hello-katalog som äger hello användarens domän. Se till att hello-administratörskonto som du försöker toosign med har globala behörigheter toohello Azure AD-klient. |

## <a name="kerberos-errors"></a>Kerberos-fel

Den här tabellen innehåller hello vanliga fel som kommer från Kerberos-installation och konfiguration, samt förslag för matchning.

| Fel | Rekommenderade åtgärder |
| ----- | ----------------- |
| Det gick inte tooretrieve hello aktuella körningsprincipen för att köra PowerShell-skript. | Om hello kopplingsinstallationen misslyckas, kontrollera toomake du att PowerShell-körningsprincipen inte är inaktiverad.<br><br>1. Öppna hello Redigeraren för Grupprincip.<br>2. Gå för**Datorkonfiguration** > **Administrationsmallar** > **Windows-komponenter**  >   **Windows PowerShell** och dubbelklicka på **aktivera skriptkörningen**.<br>3. hello körningsprincipen kan ställas in tooeither **inte konfigurerad** eller **aktiverad**. Om anges för**aktiverad**, se till att under Alternativ, hello körningsprincipen anges tooeither **Tillåt lokala skript och fjärranslutna signerade skript** eller för**alla skript**. |
| 12008 - azure AD överskred hello maximalt antal tillåtna Kerberos-autentisering försöker toohello backend-servern. | Det här felet kan indikera felaktig konfiguration mellan Azure AD och hello backend-programserver eller ett problem i tid och datum konfiguration på båda datorerna. hello backend-servern nekade hello Kerberos-biljetten som skapats av Azure AD. Kontrollera att Azure AD och hello backend-Programserver är korrekt konfigurerade. Kontrollera att hello tid och datum konfiguration på hello Azure AD och hello backend-Programserver är synkroniserade. |
| 13016 - azure AD kan inte hämta en Kerberos-biljett hello användarens räkning eftersom det finns inga UPN i hello kant-token eller i hello åtkomst cookie. | Det finns ett problem med hello STS-konfigurationen. Åtgärda hello UPN-anspråk konfigurationen i hello STS. |
| 13019 - azure AD kan inte hämta en Kerberos-biljett för hello användares räkning på grund av hello följande allmänna API-fel. | Den här händelsen kan tyda på felaktig konfiguration mellan Azure AD och hello domänkontrollantserver eller ett problem i konfigurationen för datum och tid på båda datorerna. hello domänkontrollant nekade hello Kerberos-biljetten som skapats av Azure AD. Kontrollera att Azure AD och hello backend-Programserver är korrekt konfigurerade, särskilt hello SPN konfiguration. Kontrollera att hello Azure AD är domänansluten toohello samma domän som hello domain controller tooensure som hello domänkontrollant upprättar förtroende med Azure AD. Kontrollera att hello tid och datum konfigurationen på hello Azure AD och hello domänkontrollant är synkroniserade. |
| 13020 - azure AD kan inte hämta en Kerberos-biljett hello användarens räkning eftersom hello backend server SPN inte har definierats. | Den här händelsen kan tyda på felaktig konfiguration mellan Azure AD och hello domänkontrollantserver eller ett problem i konfigurationen för datum och tid på båda datorerna. hello domänkontrollant nekade hello Kerberos-biljetten som skapats av Azure AD. Kontrollera att Azure AD och hello backend-Programserver är korrekt konfigurerade, särskilt hello SPN konfiguration. Kontrollera att hello Azure AD är domänansluten toohello samma domän som hello domain controller tooensure som hello domänkontrollant upprättar förtroende med Azure AD. Kontrollera att hello tid och datum konfigurationen på hello Azure AD och hello domänkontrollant är synkroniserade. |
| 13022 - azure AD kan inte autentisera hello användaren eftersom hello backend-servern svarar tooKerberos autentiseringsförsök med ett HTTP 401-fel. | Den här händelsen kan tyda på felaktig konfiguration mellan Azure AD och hello backend-programserver eller ett problem i tid och datum konfiguration på båda datorerna. hello backend-servern nekade hello Kerberos-biljetten som skapats av Azure AD. Kontrollera att Azure AD och hello backend-Programserver är korrekt konfigurerade. Kontrollera att hello tid och datum konfiguration på hello Azure AD och hello backend-Programserver är synkroniserade. |

## <a name="end-user-errors"></a>Slutanvändarens fel

Den här listan innehåller fel som slutanvändarna kan uppstå när de försöker tooaccess hello app och misslyckas. 

| Fel | Rekommenderade åtgärder |
| ----- | ----------------- |
| hello webbplatsen kan inte visa hello-sidan. | Användaren kan få detta fel när de försöker tooaccess hello app som du har publicerat om programmet hello är ett IWA. hello definierats SPN för det här programmet kan vara felaktigt. Kontrollera att hello SPN som konfigurerats för tillämpningsprogrammet är korrekt för IWA appar. |
| hello webbplatsen kan inte visa hello-sidan. | Användaren kan få detta fel när de försöker tooaccess hello app som du har publicerat om programmet hello är ett OWA. Detta kan orsakas av något av följande hello:<br><li>hello definierats SPN för det här programmet är felaktig. Kontrollera att hello SPN som konfigurerats för tillämpningsprogrammet är korrekt.</li><li>hello-användare som försökt tooaccess hello programmet använder ett Microsoft-konto i stället för hello rätt företagskonto toosign i eller hello användare är en gästanvändare. Se till att hello användaren loggar in med hjälp sitt företagskonto som matchar hello domänen för hello publicerade program. Account användare och gäst kan inte komma åt IWA program.</li><li>hello-användaren som försökte tooaccess hello programmet har inte definierats korrekt för det här programmet hello lokalt på klientsidan. Kontrollera att den här användaren har rätt behörighet för hello som definierats för det här backend-programmet på hello lokal dator. |
| Den här företagets appen kan inte nås. Du är inte behörig tooaccess det här programmet. Det gick inte att auktorisera. Se till att tooassign hello användare med åtkomst toothis program. | Användarna kan få detta fel när de försöker tooaccess hello app som du har publicerat om de använder Microsoft-konton i stället för deras företagskonto toosign i. Gästanvändare kan också få det här felet. Account användare och gäster kan inte komma åt IWA program. Se till att hello användaren loggar in med hjälp sitt företagskonto som matchar hello domänen för hello publicerade program.<br><br>Du har inte tilldelat hello användare för det här programmet. Gå toohello **programmet** fliken och under **användare och grupper**, tilldela den här användaren eller gruppen toothis-användarprogram. |
| Den här företagets appen är inte tillgänglig just nu. Försök igen senare... hello connector tidsgränsen. | Användarna kan få detta fel när de försöker tooaccess hello app som du har publicerat om de korrekt inte har definierats för det här programmet hello lokalt på klientsidan. Se till att användarna har rätt behörighet för hello som definierats för det här backend-programmet på hello lokal dator. |
| Den här företagets appen kan inte nås. Du är inte behörig tooaccess det här programmet. Det gick inte att auktorisera. Kontrollera att användaren hello har en licens för Azure Active Directory Premium eller Basic. | Användarna kan få detta fel när de försöker tooaccess hello app som du har publicerat om de inte uttryckligen tilldelade med en licens för Premium/enkel av hello prenumeranten administratör. Gå toohello prenumeranten Active Directory **licenser** fliken och se till att den här användaren eller användargruppen är tilldelad en Premium eller Basic-licens. |

## <a name="my-error-wasnt-listed-here"></a>Mina fel listas inte här

Om det uppstår ett fel eller problem med Azure AD Application Proxy som inte finns i den här felsökningsguiden vill vi toohear om den. Skicka ett e-tooour [feedback team](mailto:aadapfeedback@microsoft.com) hello uppgifter om hello fel du.

## <a name="see-also"></a>Se även
* [Aktivera Application Proxy för Azure Active Directory](active-directory-application-proxy-enable.md)
* [Publicera program med Application Proxy](active-directory-application-proxy-publish.md)
* [Aktivera enkel inloggning](active-directory-application-proxy-sso-using-kcd.md)
* [Aktivera villkorlig åtkomst](active-directory-application-proxy-conditional-access.md)


<!--Image references-->
[1]: ./media/active-directory-application-proxy-troubleshoot/connectorproperties.png
[2]: ./media/active-directory-application-proxy-troubleshoot/sessionlog.png
