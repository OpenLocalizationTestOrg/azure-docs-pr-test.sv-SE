---
title: "aaaAzure MFA-Server-Mobilappwebbtjänsten | Microsoft Docs"
description: "hello Microsoft Authenticator-appen finns ett alternativ för ytterligare out-of-band-autentisering.  Det gör hello MFA server toouse push-meddelanden toousers."
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: 6c8d6fcc-70f4-4da4-9610-c76d66635b8b
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/23/2017
ms.author: joflore
ms.reviewer: alexwe
ms.custom: it-pro
ms.openlocfilehash: 4175b68fcbf85ec3fd53d8edf4e07306c75a4c71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-mobile-app-authentication-with-azure-multi-factor-authentication-server"></a>Aktivera mobilappautentisering och Azure Multi-Factor Authentication Server

hello Microsoft Authenticator-appen finns ett alternativ för ytterligare out-of-band-verifiering. I stället för att placera en automatiserad telefonsamtal eller SMS toohello användare under inloggningen skickar Azure Multi-Factor Authentication en avisering toohello Microsoft Authenticator-appen på hello användarens smartphone eller surfplatta. hello användaren klickar helt enkelt **Kontrollera** (eller anger en PIN-kod och klickar på ”verifiera”) i hello app toocomplete sina inloggning.

Att använda en mobilapp för tvåstegsverifiering rekommenderas när mobilmottagningen är opålitlig. Om du använder hello appen som en OATH-token generator behöver inte alla nätverk eller internet-anslutning.

Beroende på din miljö kan du vill toodeploy hello mobilappwebbtjänsten på hello samma server som Azure Multi-Factor Authentication-servern eller på en annan mot internet-server.

## <a name="requirements"></a>Krav

toouse hello Microsoft Authenticator-appen hello följande krävs så att hello app kan kommunicera med Mobilappwebbtjänsten:

* Azure Multi-Factor Authentication Server version 6.0 eller senare
* Installera mobilappwebbtjänsten på en Internetuppkopplad webbserver som kör Microsoft® [Internet Information Services (IIS) IIS 7.x eller senare](http://www.iis.net/)
* ASP.NET v4.0.30319 är installerat, registrerats och ange tooAllowed
* Rolltjänsterna ASP.NET och IIS 6 Metabase-kompatibilitet krävs.
* Mobilappwebbtjänsten kan nås via en offentlig URL
* Mobilappwebbtjänsten skyddas av ett SSL-certifikat.
* Installera hello Azure Multi-Factor Authentication webbtjänst-SDK på IIS 7.x eller senare på hello **samma server som hello Azure Multi-Factor Authentication-servern**
* hello Azure Multi-Factor Authentication webbtjänst-SDK är skyddad med ett SSL-certifikat.
* Mobilappwebbtjänsten kan ansluta toohello Azure Multi-Factor Authentication webbtjänst-SDK via SSL
* Mobilappwebbtjänsten kan autentisera toohello Azure MFA webbtjänst-SDK med hjälp av hello autentiseringsuppgifterna för ett konto som är medlem i säkerhetsgruppen för hello ”PhoneFactor Admins”. Det här kontot och gruppen finns i Active Directory om hello Azure Multi-Factor Authentication-servern finns på en domänansluten server. Det här kontot och gruppen finns lokalt på hello Azure Multi-Factor Authentication-servern om den inte är domänansluten tooa domän.

## <a name="install-hello-mobile-app-web-service"></a>Installera hello mobilappwebbtjänsten

Innan du installerar hello mobilappwebbtjänsten bör du vara medveten om hello följande information:

* Du behöver ett tjänstkonto som är en del av gruppen "PhoneFactor Admins". Det här kontot kan vara samma som hello en användas för installationen av hello Användarportalen hello.
* Det är bra tooopen en webbläsare på webbservern för hello mot Internet och navigera toohello URL för hello webbtjänst-SDK som angavs i hello web.config-filen. Om hello webbläsare kan få toohello webbtjänsten, fråga den du efter autentiseringsuppgifter. Ange hello användarnamn och lösenord som har infogats i hello web.config-filen exakt som det visas i hello-filen. Se till att inga certifikatvarningar eller fel visas.
* Om en omvänd proxy eller brandvägg är placerad framför hello Mobilappwebbtjänsten webbservern och utför SSL-avlastning, kan du redigera hello Mobilappwebbtjänsten web.config-filen så att hello Mobilappwebbtjänsten kan använda http i stället för https. SSL krävs fortfarande från hello Mobilapp toohello brandvägg/omvänd proxy. Lägg till följande nyckel toohello hello \<appSettings\> avsnitt:

        <add key="SSL_REQUIRED" value="false"/>

### <a name="install-hello-web-service-sdk"></a>Installera webbtjänsten för hello SDK

I båda fallen om hello är Azure Multi-Factor Authentication webbtjänst-SDK **inte** redan installerat på hello Azure Multi-Factor Authentication (MFA)-Server är klar hello steg som följer.

1. Öppna konsolen för hello Multi-Factor Authentication-servern.
2. Gå toohello **webbtjänst-SDK** och välj **installera webbtjänst-SDK**.
3. Fullständig hello installera hello standardvärden om du inte behöver toochange dem av någon anledning.
4. Binda ett SSL-certifikat toohello webbplats i IIS.

Om du har frågor om hur du konfigurerar ett SSL-certifikat på en IIS-server finns i artikeln hello [hur tooSet in SSL på IIS](https://docs.microsoft.com/en-us/iis/manage/configuring-security/how-to-set-up-ssl-on-iis).

hello webbtjänst-SDK måste skyddas med ett SSL-certifikat. Ett självsignerat certifikat är lämpligt för detta ändamål. Importera hello certifikat till hello ”betrodda rotcertifikatutfärdare” Arkiv för hello lokalt datorkonto på hello Användarportalen webbserver så att den litar på certifikatet när du initierar hello SSL-anslutning.

![MFA-serverkonfiguration för webbtjänst-SDK](./media/multi-factor-authentication-get-started-server-webservice/sdk.png)

### <a name="install-hello-service"></a>Installera hello-tjänsten

1. **På hello MFA-Server**, bläddra toohello installationssökväg.
2. Navigera toohello mappen där hello Azure MFA-Server är installerat hello standard är **Files\Azure C:\Program Multifaktorautentisering**.
3. Hitta installationsfilen hello **MultiFactorAuthenticationMobileAppWebServiceSetup64**. Om servern hello **inte** mot Internet, kopiera hello installation toohello mot Internet filserver.
4. Om hello MFA-servern är **inte** mot internet-växeln toohello **internet-riktade servern**.
5. Kör hello **MultiFactorAuthenticationMobileAppWebServiceSetup64** installera filen som en administratör, ändra hello plats om du vill och ändra hello virtuella katalogen tooa kort namn om du vill ha.
6. Efter färdigställa hello installation bläddrar för**C:\inetpub\wwwroot\MultiFactorAuthMobileAppWebService** (eller lämplig directory baserat på hello katalognamnet) och redigera hello Web.Config-filen.

   * Hitta hello nyckel **”WEB_SERVICE_SDK_AUTHENTICATION_USERNAME”** och ändra **värde = ””** för**värde = ”domän\användare”** där domän\användare är ett konto som är en del av Gruppen ”PhoneFactor Admins”.
   * Hitta hello nyckel **”WEB_SERVICE_SDK_AUTHENTICATION_PASSWORD”** och ändra **värde = ””** för**värde = ”Password”** där lösenordet är hello lösenord för hello Service Kontot som angetts i hello föregående rad.
   * Hitta hello **pfMobile App Web Service_pfwssdk_PfWsSdk** inställningen och ändra hello-värde från **http://localhost:4898/PfWsSdk.asmx** toohello webbtjänst-SDK URL (exempel: https://mfa.contoso.com/ MultiFactorAuthWebServiceSdk/PfWsSdk.asmx).
   * Spara hello Web.Config-filen och stäng Anteckningar.

   > [!NOTE]
   > Eftersom SSL används för den här anslutningen måste du referera hello webbtjänst-SDK med **fullständigt kvalificerade domännamnet (FQDN)** och **inte IP-adress**. hello SSL-certifikatet skulle ha utfärdats hello FQDN och hello URL som används måste matcha hello namnet på hello certifikatet.

7. Om hello-webbplats som Mobilappwebbtjänsten installerades under inte har redan bundits med ett offentligt signerat certifikat, installera hello certifikat på hello servern, öppna IIS-hanteraren och binda hello certifikat toohello webbplats.
8. Öppna en webbläsare från valfri dator och navigera toohello URL där Mobilappwebbtjänsten har installerats (exempel: https://mfa.contoso.com/MultiFactorAuthMobileAppWebService). Se till att inga certifikatvarningar eller fel visas.

## <a name="configure-hello-mobile-app-settings-in-hello-azure-multi-factor-authentication-server"></a>Konfigurera inställningarna för hello mobilappar i hello Azure Multi-Factor Authentication-servern

Nu när hello mobilappwebbtjänsten installeras, måste tooconfigure hello Azure Multi-Factor Authentication-servern toowork med hello-portalen.

1. Hello Multi-Factor Authentication-konsolen, klicka på ikonen för hello Användarportalen. Om användare tillåts toocontrol sina autentiseringsmetoder, kontrollera **Mobilapp** på fliken Inställningar hello under **användarna tooselect metoden**. Utan den här funktionen är aktiverad slutanvändarna är obligatoriska toocontact supportavdelningen toocomplete aktiveringen för hello Mobilapp.
2. Kontrollera hello **Tillåt användare tooactivate Mobilapp** rutan.
3. Kontrollera hello **Tillåt användarregistrering** rutan.
4. Klicka på hello **Mobilapp** ikon.
5. Ange hello-URL som används med hello virtuell katalog som skapades när du installerar MultiFactorAuthenticationMobileAppWebServiceSetup64 (exempel: https://mfa.contoso.com/MultiFactorAuthMobileAppWebService/) i fältet hello  **Mobila URL för Mobilappwebbtjänsten:**.
6. Fylla hello **kontonamn** med toodisplay för hello-företaget eller organisationen namn i hello mobila program för det här kontot.
   ![MFA-serverkonfiguration för mobilappinställningar](./media/multi-factor-authentication-get-started-server-webservice/mobile.png)

## <a name="next-steps"></a>Nästa steg

- [Avancerade scenarier med Azure Multi-Factor Authentication och virtuella privata nätverk från tredje part](multi-factor-authentication-advanced-vpn-configurations.md).