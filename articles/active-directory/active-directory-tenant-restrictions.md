---
title: "aaaManage åtkomst toocloud appar genom att begränsa klienter – Azure | Microsoft Docs"
description: "Hur toouse klient begränsningar toomanage vilka användare kan öppna programmen baserat på sina Azure AD-klient."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: kgremban
ms.openlocfilehash: 6470fa217738b29104353ae17a2f53216f825c19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-tenant-restrictions-toomanage-access-toosaas-cloud-applications"></a>Använda begränsningar för klient toomanage åtkomst tooSaaS molnprogram

Stora organisationer som betona säkerhet vill toomove toocloud tjänster som Office 365, men behöver tooknow som användarna bara kan komma åt godkända resurser. Företag begränsa traditionellt domännamn eller IP-adresser när de vill toomanage åtkomst. Den här metoden misslyckas i en värld där SaaS-appar finns i ett offentligt moln som körs på delade domännamn som outlook.office.com och login.microsoftonline.com. Blockerar adresserna skulle förhindra att användare ansluter till Outlook på hello web helt, i stället för att begränsa dem bara tooapproved identiteter och resurser.

Azure Active Directory-lösningen toothis utmaningen är en funktion som kallas klient begränsningar. Klient begränsningar kan organisationer toocontrol åtkomst tooSaaS molnprogram, baserat på hello Azure AD-klient hello program används för enkel inloggning. Exempelvis kanske tooallow åtkomst tooyour organisations Office 365-program, samtidigt som du förhindrar åtkomst tooother organisationers instanser av samma programmen.  

Klient begränsningar ger organisationer hello möjlighet toospecify hello listan över klienter som användarna tillåts tooaccess. Azure AD sedan ger endast åtkomst toothese tillåts klienter.

Den här artikeln fokuserar på klient begränsningar för Office 365, men hello-funktionen ska fungera med alla SaaS-molnappar som använder modern autentiseringsprotokoll med Azure AD för enkel inloggning. Om du använder SaaS-appar med en annan Azure AD-klient från hello-klienten som används av Office 365, se till att alla nödvändiga klienter tillåts. Mer information om SaaS molnappar finns hello [Active Directory Marketplace](https://azure.microsoft.com/en-us/marketplace/active-directory/).

## <a name="how-it-works"></a>Hur det fungerar

hello övergripande består lösningen hello följande komponenter: 

1. **Azure AD** – om hello `Restrict-Access-To-Tenants: <permitted tenant list>` är finns, Azure AD endast hello problem säkerhetstoken för klienter. 

2. **Lokal infrastruktur för server** – en proxy-enhet kan SSL-kontroll, konfigurerade tooinsert hello-huvudet som innehåller hello lista över tillåtna hyresgäster i trafiken till Azure AD. 

3. **Klientprogrammet** – toosupport klient begränsningar klientprogrammet måste begära token direkt från Azure AD så att trafik kan avlyssnas av hello infrastruktur. Begränsningar för klienten stöds för närvarande genom webbläsarbaserad Office 365-program och Office-klienter när du använder modern autentisering (till exempel OAuth 2.0). 

4. **Modern autentisering** – molntjänster måste använda modern autentisering toouse klient begränsningar och blockera åtkomst till tooall ej tillåten innehavare. Office 365 molntjänster måste vara konfigurerade toouse moderna autentiseringsprotokoll som standard. Hello senaste informationen på Office 365-stöd för modern autentisering kan läsa [uppdateras Office 365 modern autentisering](https://blogs.office.com/2015/11/19/updated-office-365-modern-authentication-public-preview/).

hello följande diagram illustrerar hello övergripande trafikflöde. SSL-kontroll krävs bara på trafik tooAzure AD, inte toohello Office 365 molntjänster. Skillnaden är viktig eftersom hello trafikvolym för autentisering tooAzure AD är vanligtvis mycket lägre än trafik tooSaaS program som Exchange Online och SharePoint Online.

![Klient begränsningar trafikflöde – diagram](./media/active-directory-tenant-restrictions/traffic-flow.png)

## <a name="set-up-tenant-restrictions"></a>Konfigurera begränsningar för klient

Det finns två steg tooget igång med klient-begränsningar. hello första steget är toomake till att klienterna kan ansluta toohello rätt adresser. hello andra är tooconfigure din infrastruktur.

### <a name="urls-and-ip-addresses"></a>URL: er och IP-adresser

toouse klient begränsningar klienterna måste vara kan tooconnect toohello följande URL: er i Azure AD tooauthenticate: login.microsoftonline.com, login.microsoft.com och login.windows.net. Dessutom tooaccess Office 365 klienterna måste också vara kan tooconnect toohello FQDN/URL: er och IP-adresser som definierats i [Office 365-URL: er och IP-adressintervall](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2). 

### <a name="proxy-configuration-and-requirements"></a>Proxykonfigurationen och krav

hello följande konfiguration är obligatoriska tooenable begränsningar för klient via proxy-infrastruktur. Den här vägledningen är generisk, så bör du läsa tooyour proxy leverantörens dokumentation för specifika stegen.

#### <a name="prerequisites"></a>Krav

- hello proxy måste vara kan tooperform SSL avlyssning, HTTP-huvudet infogning och filter mål med hjälp av FQDN/URL: er. 

- Klienter måste lita hello certifikatkedja som presenteras av hello proxy för SSL-kommunikation. Om certifikat från en intern PKI som används till exempel vara hello interna utfärdande certifikat certifikat för rotutfärdare betrodda.

- Den här funktionen ingår i Office 365-prenumerationer, men om du vill toouse klient begränsningar toocontrol åtkomst tooother SaaS-appar sedan Azure AD Premium 1 licenser krävs.

#### <a name="configuration"></a>Konfiguration

För varje inkommande begäran toologin.microsoftonline.com login.microsoft.com och login.windows.net, Infoga två HTTP-huvuden: *begränsa åtkomst till klienter* och *begränsa åtkomst kontext*.

hello huvuden bör innehålla hello följande element: 
- För *begränsa åtkomst till klienter*, värdet \<tillåts klienten listan\>, som är en kommaavgränsad lista för klienter som du vill tooallow användare tooaccess. Alla domäner som har registrerats med en klient kan vara används tooidentify hello klient i den här listan. Toopermit komma åt tooboth Contoso och Fabrikam klienter, hello namn/värde-par ser ut som:`Restrict-Access-To-Tenants: contoso.onmicrosoft.com,fabrikam.onmicrosoft.com` 
- För *begränsa åtkomst kontext*, ett värde av en enskild katalog-ID, deklarerar vilken anger hello klient begränsningar. Till exempel toodeclare Contoso som hello-klient som angetts hello klient begränsningar princip hello namn/värde-par ser ut som:`Restrict-Access-Context: 456ff232-35l2-5h23-b3b3-3236w0826f3d`  

> [!TIP]
> Du hittar din katalog-ID i hello [Azure-portalen](https://portal.azure.com). Logga in som administratör, Välj **Azure Active Directory**och välj **egenskaper**.

tooprevent användare från att lägga till egna HTTP-huvud med icke-godkända innehavare hello proxy måste tooreplace hello begränsa åtkomst till klienter huvudet om det finns redan i hello inkommande begäran. 

Klienter måste vara framtvingad toouse hello proxy för alla begäranden toologin.microsoftonline.com login.microsoft.com och login.windows.net. Till exempel om PAC-filer används toodirect klienter toouse hello proxy, slutanvändare inte får kan tooedit eller inaktivera hello PAC-filer.

## <a name="hello-user-experience"></a>hello användarupplevelse

Detta avsnitt visar hello upplevelsen för både användare och administratörer.

### <a name="end-user-experience"></a>Slutanvändarens upplevelse

Ett exempel på användare på hello Contoso-nätverket, men försöker tooaccess hello Fabrikam instans av en delad SaaS-program som Outlook online. Om en klient som inte tillåts för instansen är Contoso ser hello användaren hello följande sida:

![Åtkomst nekades för användare i ej tillåten klienter](./media/active-directory-tenant-restrictions/end-user-denied.png)

### <a name="admin-experience"></a>Administratörsupplevelse

När konfigurationen av klient begränsningar görs på hello företagets infrastruktur, Administratörer kan komma åt hello klient begränsningar rapporter i hello Azure-portalen direkt. tooview hello rapporterar gå toohello Azure Active Directory översiktssidan sedan tittar du under andra funktioner.

Hej administratör för hello klienten som angetts som hello begränsad åtkomst kontext innehavare kan använda den här rapporten toosee alla inloggningar som blockerats på grund av hello klient begränsningar princip, inklusive hello-identitet som används och målkatalogen hello-ID.

![Använd hello Azure portal tooview begränsade inloggningsförsök](./media/active-directory-tenant-restrictions/portal-report.png)

Du kan använda filter toospecify hello omfånget för rapporten som andra rapporter i hello Azure-portalen. Du kan filtrera efter en viss användare, program, klienter eller tidsintervall.

## <a name="office-365-support"></a>Stöd för Office 365

Office 365-program måste uppfylla två kriterier toofully stöd klient begränsningar:

1. hello klienten använde stöder modern autentisering
2. Modern autentisering är aktiverat som hello standardprotokollet för autentisering för hello-Molntjänsten.

Se för[uppdateras Office 365 modern autentisering](https://blogs.office.com/2015/11/19/updated-office-365-modern-authentication-public-preview/) hello senaste informationen om vilka Office klienter för närvarande stöder modern autentisering. Sidan innehåller också länkar tooinstructions för att aktivera modern autentisering på specifika Exchange Online och Skype för företag – Online-innehavare. Modern autentisering är redan aktiverat som standard i SharePoint Online.

Klient-begränsningar för närvarande stöds av Office 365-webbläsarbaserade program (hello Office-portalen Yammer, SharePoint platser Outlook på hello Web osv.). För tjock klienter (Outlook, Skype för företag, Word, Excel, PowerPoint, etc.) Klient begränsningar kan bara tillämpas när modern autentisering används.  

Outlook och Skype för företag-klienter som stöder modern autentisering är fortfarande toouse äldre protokoll mot klienter där modern autentisering inte har aktiverats, effektivt kringgå klient begränsningar. För Outlook på Windows kan kunderna välja tooimplement begränsningar som hindrar användare från att lägga till icke-godkända e-postkonton tootheir profiler. Se exempelvis hello [förhindra att lägga till konton som inte är standard Exchange](http://gpsearch.azurewebsites.net/default.aspx?ref=1) grupprincipinställning. Fullständigt stöd för klienten är inte tillgängliga för Outlook på Windows-plattformar och för Skype för företag på alla plattformar.

## <a name="testing"></a>Testning

Om du vill tootry ut klient begränsningar innan du implementerar för hela organisationen, det finns två alternativ: en värdbaserad metod med ett verktyg som Fiddler eller en stegvis distribution av proxy-inställningar.

### <a name="fiddler-for-a-host-based-approach"></a>Fiddler för en värdbaserad metod

Fiddler är en gratis webbplatser felsökning proxy som kan använda toocapture och ändra HTTP/HTTPS-trafik, inklusive lägga till HTTP-huvuden. tooconfigure Fiddler tootest klient begränsningar utför hello följande steg:

1.  [Hämta och installera Fiddler](http://www.telerik.com/fiddler).
2.  Konfigurera Fiddler toodecrypt HTTPS-trafik [Fiddlers hjälpdokumentationen](http://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS).
3.  Konfigurera Fiddler tooinsert hello *begränsa åtkomst till klienter* och *begränsa åtkomst kontext* huvuden med hjälp av anpassade regler:
  1. Välj hello i hello Fiddler Web felsökningsverktyget, **regler** -menyn och välj **anpassa regler...** tooopen hello CustomRules fil.
  2. Lägg till följande rader hello början av hello hello *OnBeforeRequest* funktion. Ersätt \<klientdomänen\> för en domän registrerats med din klient, till exempel contoso.onmicrosoft.com. Ersätt \<katalog-ID\> med din klients Azure AD-GUID-identifierare.

  ```
  if (oSession.HostnameIs("login.microsoftonline.com") || oSession.HostnameIs("login.microsoft.com") || oSession.HostnameIs("login.windows.net")){      oSession.oRequest["Restrict-Access-To-Tenants"] = "<tenant domain>";      oSession.oRequest["Restrict-Access-Context"] = "<directory ID>";}
  ```

  Om du behöver tooallow flera klienter kan du använda ett kommatecken tooseparate hello klient namn. Exempel:

  ```
  oSession.oRequest["Restrict-Access-To-Tenants"] = "contoso.onmicrosoft.com,fabrikam.onmicrosoft.com";
  ```

4. Spara och Stäng hello CustomRules fil.

När du har konfigurerat Fiddler kan du fånga in trafik genom att gå toohello **filen** menyn och välja **fånga in trafik**.

### <a name="staged-rollout-of-proxy-settings"></a>Stegvis distribution av proxy-inställningar

Beroende på hello funktionerna i din infrastruktur du kan toostage hello distributionen av inställningar tooyour användare. Här följer några avancerade alternativ hänsyn till:

1.  Använd PAC filer toopoint test användare tooa test infrastruktur, medan normal användare fortsätter toouse hello produktionsinfrastruktur.
2.  Vissa proxyservrar kanske stöder olika konfigurationer med hjälp av grupper.

Specifik information finns i dokumentationen för tooyour proxy-server.

## <a name="next-steps"></a>Nästa steg

- Läs mer om [uppdateras Office 365 modern autentisering](https://blogs.office.com/2015/11/19/updated-office-365-modern-authentication-public-preview/)

- Granska hello [Office 365-URL: er och IP-adressintervall](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)
