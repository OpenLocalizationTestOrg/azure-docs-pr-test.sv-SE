---
title: "aaaUser portal för Azure MFA-Server | Microsoft Docs"
description: "Det här är hello Azure Multi-Factor authentication sida som beskriver hur tooget igång med Azure MFA och hello användarportalen."
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: 06b419fa-3507-4980-96a4-d2e3960e1772
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/23/2017
ms.author: joflore
ms.reviewer: alexwe
ms.custom: it-pro
ms.openlocfilehash: 0e36644c3d780249fb98d5da654e9d267c63561a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="user-portal-for-hello-azure-multi-factor-authentication-server"></a>Användarportalen för hello Azure Multi-Factor Authentication-servern

Hej användarportalen är en IIS-webbplats som tillåter användare tooenroll i Azure Multi-Factor Authentication (MFA) och underhålla sina konton. En användare kan ändra sitt telefonnummer, ändra sina PIN-kod eller välj toobypass tvåstegsverifiering under deras nästa inloggning.

Användare loggar in toohello användarportalen med sitt vanliga användarnamn och lösenord, sedan slutföra ett verifieringssamtal i två steg eller besvara säkerhet frågor toocomplete sin autentisering. Om användarregistrering tillåts konfigurera användare sina telefonnummer och PIN-kod hello första gången de loggar i toohello användarportalen.

Användarportalens administratörer kan ställa in och beviljats behörighet tooadd nya användare och uppdatera befintliga användare.

Beroende på din miljö kan du vill toodeploy hello användarportalen på hello samma server som Azure Multi-Factor Authentication-servern eller på en annan mot internet-server.

![MFA-användarportal](./media/multi-factor-authentication-get-started-portal/portal.png)

> [!NOTE]
> Hej användarportalen är bara tillgänglig med Multi-Factor Authentication-servern. Om du använder multi-Factor Authentication i molnet hello finns dina användare toohello [konfigurera kontot för tvåstegsverifiering](./end-user/multi-factor-authentication-end-user-first-time.md) eller [hantera inställningar för tvåstegsverifiering](./end-user/multi-factor-authentication-end-user-manage-settings.md).

## <a name="install-hello-web-service-sdk"></a>Installera webbtjänsten för hello SDK

I båda fallen om hello är Azure Multi-Factor Authentication webbtjänst-SDK **inte** redan installerat på hello Azure Multi-Factor Authentication (MFA)-Server är klar hello steg som följer.

1. Öppna konsolen för hello Multi-Factor Authentication-servern.
2. Gå toohello **webbtjänst-SDK** och välj **installera webbtjänst-SDK**.
3. Fullständig hello installera hello standardvärden om du inte behöver toochange dem av någon anledning.
4. Binda ett SSL-certifikat toohello webbplats i IIS.

Om du har frågor om hur du konfigurerar ett SSL-certifikat på en IIS-server finns i artikeln hello [hur tooSet in SSL på IIS](https://docs.microsoft.com/en-us/iis/manage/configuring-security/how-to-set-up-ssl-on-iis).

hello webbtjänst-SDK måste skyddas med ett SSL-certifikat. Ett självsignerat certifikat är lämpligt för detta ändamål. Importera hello certifikat till hello ”betrodda rotcertifikatutfärdare” Arkiv för hello lokalt datorkonto på hello Användarportalen webbserver så att den litar på certifikatet när du initierar hello SSL-anslutning.

![MFA-serverkonfiguration för webbtjänst-SDK](./media/multi-factor-authentication-get-started-portal/sdk.png)

## <a name="deploy-hello-user-portal-on-hello-same-server-as-hello-azure-multi-factor-authentication-server"></a>Distribuera hello användarportalen på hello samma server som hello Azure Multi-Factor Authentication-servern

hello följande förutsättningar är obligatoriska tooinstall hello användarportalen på hello **samma server** som hello Azure Multi-Factor Authentication-servern:

* IIS, inklusive ASP.NET och IIS 6 metabase-kompatibilitet (för IIS 7 eller senare)
* Ett konto med administratörsrättigheter för hello dator och domän om tillämpligt. hello-kontot måste behörigheter toocreate Active Directory-säkerhetsgrupper.
* Skydda hello användarportalen med ett SSL-certifikat.
* Skydda hello Azure Multi-Factor Authentication webbtjänst-SDK med ett SSL-certifikat.

toodeploy hello användaren portal, Följ dessa steg:

1. Öppna hello Azure Multi-Factor Authentication-konsolen, klicka på hello **Användarportalen** ikon i hello vänster menyn, klicka på **installera Användarportal**.
2. Fullständig hello installera hello standardvärden om du inte behöver toochange dem av någon anledning.
3. Binda ett SSL-certifikat toohello webbplats i IIS

   > [!NOTE]
   > SSL-certifikatet är vanligtvis ett offentligt signerat SSL-certifikat.

4. Öppna en webbläsare från valfri dator och navigera toohello URL där hello användarportalen installerades (exempel: https://mfa.contoso.com/MultiFactorAuth). Se till att inga certifikatvarningar eller fel visas.

![Installation av MFA-serveranvändarportal](./media/multi-factor-authentication-get-started-portal/install.png)

Om du har frågor om hur du konfigurerar ett SSL-certifikat på en IIS-server finns i artikeln hello [hur tooSet in SSL på IIS](https://docs.microsoft.com/en-us/iis/manage/configuring-security/how-to-set-up-ssl-on-iis).

## <a name="deploy-hello-user-portal-on-a-separate-server"></a>Distribuera hello användarportalen på en separat server

Om hello servern där Azure Multi-Factor Authentication-servern körs inte är mot internet, installerar du användarportalen hello på en **separat, internet-riktade servern**.

Om din organisation använder hello Microsoft Authenticator-appen som en hello verifieringsmetoderna och vill toodeploy hello användarportalen på en egen server, utför hello följande krav:

* Använd v6.0 eller högre för hello Azure Multi-Factor Authentication-servern.
* Installera användarportalen hello på en internet-riktade webbserver som kör Microsoft internet Information Services (IIS) 6.x eller högre.
* När du använder IIS 6.x, se till att ASP.NET v2.0.50727 är installerad, registrerats och ange för**tillåtna**.
* När du använder IIS 7.x eller högre, IIS, inklusive grundläggande autentisering, ASP.NET och IIS 6 metabase-kompatibilitet.
* Skydda hello användarportalen med ett SSL-certifikat.
* Skydda hello Azure Multi-Factor Authentication webbtjänst-SDK med ett SSL-certifikat.
* Se till att hello användarportalen kan ansluta toohello Azure Multi-Factor Authentication webbtjänst-SDK via SSL.
* Se till att hello användarportalen kan autentisera toohello Azure Multi-Factor Authentication webbtjänst-SDK med hello autentiseringsuppgifter för ett tjänstkonto i säkerhetsgruppen för hello ”PhoneFactor Admins”. Det här kontot och gruppen ska finnas i Active Directory om hello Azure Multi-Factor Authentication-servern körs på en domänansluten server. Det här kontot och gruppen finns lokalt på hello Azure Multi-Factor Authentication-servern om den inte är domänansluten tooa domän.

Installera hello användarportalen på en annan server än hello Azure Multi-Factor Authentication-servern kräver hello följande steg:

1. **På hello MFA-Server**, bläddra toohello installationssökvägen (exempel: C:\Program program\multi-Factor Authentication-Server), och kopiera hello fil **MultiFactorAuthenticationUserPortalSetup64** tooa plats tillgänglig toohello mot internet-server där du installerar den.
2. **På hello webbserver för internet-riktade**, köra hello MultiFactorAuthenticationUserPortalSetup64 filen för installation som administratör, ändra hello plats om du vill och ändra hello virtuella katalogen tooa kort namn om du vill ha.
3. Binda ett SSL-certifikat toohello webbplats i IIS.

   > [!NOTE]
   > SSL-certifikatet är vanligtvis ett offentligt signerat SSL-certifikat.

4. Bläddra för**C:\inetpub\wwwroot\MultiFactorAuth**
5. Redigera hello Web.config i anteckningar

    * Hitta hello nyckel **”USE_WEB_SERVICE_SDK”** och ändra **värde = ”false”** för**värde = ”true”**
    * Hitta hello nyckel **”WEB_SERVICE_SDK_AUTHENTICATION_USERNAME”** och ändra **värde = ””** för**värde = ”domän\användare”** där domän\användare är ett konto som är en del av Gruppen ”PhoneFactor Admins”.
    * Hitta hello nyckel **”WEB_SERVICE_SDK_AUTHENTICATION_PASSWORD”** och ändra **värde = ””** för**värde = ”Password”** där lösenordet är hello lösenord för hello Service Kontot som angetts i hello föregående rad.
    * Hitta värdet för hello **https://www.contoso.com/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx** och ändra den här platshållare URL toohello SDK URL för webbtjänsten vi installerade i steg 2.
    * Spara hello Web.Config-filen och stäng Anteckningar.

6. Öppna en webbläsare från valfri dator och navigera toohello URL där hello användarportalen installerades (exempel: https://mfa.contoso.com/MultiFactorAuth). Se till att inga certifikatvarningar eller fel visas.

Om du har frågor om hur du konfigurerar ett SSL-certifikat på en IIS-server finns i artikeln hello [hur tooSet in SSL på IIS](https://docs.microsoft.com/en-us/iis/manage/configuring-security/how-to-set-up-ssl-on-iis).

## <a name="configure-user-portal-settings-in-hello-azure-multi-factor-authentication-server"></a>Konfigurera inställningar för användarportal i hello Azure Multi-Factor Authentication-servern

Nu att hello användarportalen installeras måste tooconfigure hello Azure Multi-Factor Authentication-servern toowork med hello-portalen.

1. Hello Azure Multi-Factor Authentication-konsolen, klicka på hello **Användarportalen** ikon. Ange hello URL toohello användarportalen på fliken Inställningar hello i hello **Användarportalens URL** textruta. Om e-postfunktioner har aktiverats, med den här URL: en i hello e-postmeddelanden som skickas toousers när de importeras till hello Azure Multi-Factor Authentication-servern.
2. Välj hello inställningar som du vill toouse i hello Användarportalen. Om användare tillåts toochoose sina autentiseringsmetoder, till exempel som **användarna tooselect metoden** kontrolleras tillsammans med hello-metoder som de kan välja från.
3. Definiera vem som ska vara administratörer på hello **administratörer** fliken. Du kan skapa detaljerade administrativa behörigheter med hjälp av kryssrutorna hello och nedrullningsbara listorna i hello Lägg till/redigera rutor.

Valfri konfiguration:
- **Säkerhetsfrågor** -definiera godkända säkerhetsfrågor för din miljö och hello språk som de visas i.
- **Slutförda sessioner** – Konfigurera användarportalintegrering med en formulärbaserad webbplats som använder MFA.
- **Betrodda IP-adresser** -Tillåt användare tooskip MFA när autentisering från en lista över betrodda IP-adresser eller intervall.

![Konfiguration av MFA-serveranvändarportal](./media/multi-factor-authentication-get-started-portal/config.png)

Azure Multi-Factor Authentication-servern erbjuder flera alternativ för hello användarportalen. hello innehåller följande tabell en lista över de här alternativen och en beskrivning av vad de används.

| Inställningar för användarportalen | Beskrivning |
|:--- |:--- |
| URL till användarportalen | Ange hello Webbadressen för där hello portal finns. |
| Primär autentisering | Ange hello typ av autentisering toouse när du loggar in toohello portal. Du kan välja mellan Windows-, RADIUS- eller LDAP-autentisering. |
| Tillåt användare toolog i | Tillåt användare tooenter användarnamn och lösenord på hello-inloggningssida för hello användarportalen. Om inte det här alternativet väljs är hello rutor nedtonade. |
| Tillåt användarregistrering | Tillåt tooenroll en användare i Multi-Factor Authentication genom att göra dem tooa installationsskärmen som uppmanar dem att ytterligare information, till exempel telefonnummer. Fråga efter reservtelefonnummer kan användare toospecify ett sekundärt telefonnummer. Fråga efter utomstående OATH-token kan användare toospecify en utomstående OATH-token. |
| Tillåt användare tooinitiate Engångsförbikoppling | Tillåt användare tooinitiate en engångsförbikoppling. Om en användare har upprättat det här alternativet, tar hello gälla nästa gång hello användaren loggar in. Fråga efter kringgå sekunder ger hello användare med en ruta så att de kan ändra hello standard 300 sekunder. Annars gäller hello engångsförbikoppling bara 300 sekunder. |
| Tillåt att användare tooselect metod | Tillåt användare toospecify sina Primär kontaktmetod. Den här metoden kan vara ett telefonsamtal, ett textmeddelande, en mobilapp eller en OATH-token. |
| Tillåt att användare tooselect språk | Tillåt användare toochange hello språk som används för hello telefonsamtal, textmeddelande, mobilapp eller OATH-token. |
| Tillåt att användare tooactivate mobila appen | Tillåt användare toogenerate kod toocomplete hello mobilappen aktivering aktiveringsprocessen som används med hello-server.  Du kan också ange hello antalet enheter som de kan aktivera hello appen på mellan 1 och 10. |
| Använd säkerhetsfrågor som reserv | Tillåt säkerhetsfrågor om tvåstegsverifiering misslyckas. Du kan ange hello antal säkerhetsfrågor som måste besvaras korrekt. |
| Tillåt användare tooassociate utomstående OATH-token | Tillåt användare toospecify en utomstående OATH-token. |
| Använd OATH-token som reserv | Tillåt för hello användning av en OATH-token om det inte går att genomföra tvåstegsverifiering. Du kan också ange hello tidsgräns i minuter. |
| Aktivera loggning | Aktivera loggning på hello användarportalen. hello loggfilerna finns på: C:\Program program\multi-Factor Authentication Server\Logs. |

Dessa inställningar blir synliga toohello användaren i hello portal när de är aktiverade och de har loggat in toohello användarportalen.

![Inställningar för användarportalen](./media/multi-factor-authentication-get-started-portal/portalsettings.png)

### <a name="self-service-user-enrollment"></a>Användarregistrering via självbetjäning

Om du vill använda dina användare toosign och registrera måste du välja hello **Tillåt användare toolog i** och **Tillåt användarregistrering** alternativ under hello på fliken Inställningar. Kom ihåg att hello-inställningar som du väljer påverkar hello inloggning användarupplevelsen.

Till exempel när en användare loggar in toohello användarportalen för hello första gången, hämtas sedan de toohello Azure Multi-Factor Authentication användarinställningar för sidan. Beroende på hur du har konfigurerat Azure Multi-Factor Authentication hello användare kan vara kan tooselect sina autentiseringsmetod.

Om de väljer hello röst anropa verifieringsmetod eller har redan konfigurerats toouse metoden uppmanas hello sidan hello användaren tooenter sina primära telefonnummer och tillägg i tillämpliga fall. De kan också vara tillåtna tooenter ett sekundärt telefonnummer.

![Registrera primära och sekundära telefonnummer](./media/multi-factor-authentication-get-started-portal/backupphone.png)

Om hello användaren krävs toouse en PIN-kod när de autentiserar, uppmanar hello sidan dem toocreate en PIN-kod. När du har angett sina phone nummer och PIN-kod (om tillämpligt) hello användaren klickar på hello **Ring mig nu tooAuthenticate** knappen. Azure Multi-Factor Authentication utför ett telefonsamtal verifiering toohello användarens primära telefonnummer. hello användare måste besvara hello telefonsamtal och anger sina PIN-kod (om tillämpligt) och tryck på # toomove på toohello nästa steg i processen för hello självregistrering.

Om hello användaren väljer hello textmeddelande verifieringsmetod eller har redan konfigurerats toouse metoden, frågar hello sidan hello användaren efter deras mobiltelefonnummer. Om hello användaren krävs toouse en PIN-kod när de autentiserar, hello sidan också uppmanas dem tooenter en PIN-kod.  När du har angett sitt telefonnummer och PIN-kod (om tillämpligt) hello användaren klickar på hello **Text mig nu tooAuthenticate** knappen. Azure Multi-Factor Authentication utför ett SMS verifiering toohello användarens mobiltelefon. hello användaren får hello SMS med en-gång-lösenord (OTP) och sedan svar toohello meddelande med den OTP samt deras PIN-kod (om tillämpligt).

![SMS på användarportalen](./media/multi-factor-authentication-get-started-portal/text.png)

Om hello användaren väljer hello Mobilapp verifieringsmetod, hello sidan uppmanar hello användaren tooinstall hello Microsoft Authenticator-appen på sin enhet och generera en aktiveringskod. När du har installerat hello app hello användaren klickar hello Generera en aktiveringskod.

> [!NOTE]
> toouse hello Microsoft Authenticator-appen hello användaren måste aktivera push-meddelanden för enheten.

hello sidan visar sedan en aktiveringskod och en Webbadressen tillsammans med en streckkod bild. Om hello användaren krävs toouse en PIN-kod när de autentiserar, hello sidan uppmanas dessutom dem tooenter en PIN-kod. hello användaren anger hello aktiveringskoden och URL: en i hello Microsoft Authenticator-appen eller använder hello streckkod skanner tooscan hello streckkod bild och klickar på hello aktivera knappen.

När hello aktiveringen är klar hello användaren klickar på hello **autentisera mig nu** knappen. Azure Multi-Factor Authentication utför en verifiering toohello användarens mobilapp. hello användaren måste ange sina PIN-kod (om tillämpligt) och tryck hello autentisera på sina mobila appar toomove på toohello nästa steg i processen för hello självregistrering.

Om hello administratörer har konfigurerat hello Azure Multi-Factor Authentication-servern toocollect säkerhetsfrågor och svar, tas hello användaren därefter toohello säkerhetsfrågor sidan. hello användaren måste Välj fyra säkerhetsfrågor och ange svaren tootheir valt frågor.

![Säkerhetsfrågor för användarportalen](./media/multi-factor-authentication-get-started-portal/secq.png)

självregistrering av användare hello är slutförd och hello användare är inloggad toohello användarportalen. Användare kan logga in igen toohello användarportalen när som helst i hello framtida toochange deras telefonnummer, PIN-koder, autentiseringsmetoder och säkerhetsfrågor om är tillåtet att ändra deras metoder via deras administratörer.

## <a name="next-steps"></a>Nästa steg

- [Distribuera hello Azure Multi-Factor Authentication Server Mobilappwebbtjänsten](multi-factor-authentication-get-started-server-webservice.md)
