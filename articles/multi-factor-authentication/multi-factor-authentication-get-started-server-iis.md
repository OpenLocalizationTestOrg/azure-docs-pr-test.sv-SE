---
title: aaaIIS autentisering och Azure MFA-Server | Microsoft Docs
description: "Det här är hello Azure Multi-Factor authentication sida som hjälper distribuerar IIS-autentisering och Azure Multi-Factor Authentication-servern."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: d1bf1c8a-2c10-4ae6-9f4b-75f0c3df43eb
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/16/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: H1Hack27Feb2017,it-pro
ms.openlocfilehash: 74bd39c2644e2bca0880baea3824cad4c9215111
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-azure-multi-factor-authentication-server-for-iis-web-apps"></a>Konfigurera Azure Multi-Factor Authentication Server för IIS-webbappar

Använd hello IIS-autentisering avsnitt i hello Azure Multi-Factor Authentication (MFA) Server tooenable och konfigurera IIS-autentisering för integrering med Microsoft IIS-webbprogram. hello Azure MFA-servern installerar ett plugin-program som kan filtrera begäranden som gjorts toohello IIS web server tooadd Azure Multi-Factor Authentication. hello IIS plugin-program har stöd för formulärbaserad autentisering och integrerad Windows-HTTP-autentisering. Betrodda IP-adresser kan också vara konfigurerade tooexempt interna IP-adresser från tvåfaktorsautentisering.

![IIS-autentisering](./media/multi-factor-authentication-get-started-server-iis/iis.png)

## <a name="using-form-based-iis-authentication-with-azure-multi-factor-authentication-server"></a>Använda formulärbaserad IIS-autentisering med Azure Multi-Factor Authentication Server
toosecure en IIS-webbprogram som använder formulärbaserad autentisering, installera hello Azure Multi-Factor Authentication-servern på hello IIS-webbserver och konfigurera hello Server per hello nedan:

1. Klicka på hello IIS-autentisering ikonen hello vänstra menyn i hello Azure Multi-Factor Authentication-servern.
2. Klicka på hello **formulärbaserad** fliken.
3. Klicka på **Lägg till**.
4. toodetect användarnamn, lösenord och domän variabler automatiskt, ange hello inloggnings-URL (t.ex. https://localhost/contoso/auth/login.aspx) i dialogrutan för hello Konfigurera automatiskt formulärbaserad webbplats och klicka på **OK**.
5. Kontrollera hello **Multi-Factor Authentication-användarmatchning** om alla användare har eller importeras till hello Server och ämne toomulti-factor-autentisering. Om ett stort antal användare inte har ännu importerats till hello Server och/eller undantas från Multi-Factor authentication, lämnar du hello kryssrutan avmarkerad.
6. Om hello sidvariablerna inte kan identifieras automatiskt, klickar du på **ange manuellt** hello Konfigurera automatiskt formulärbaserad webbplats i dialogrutan.
7. Hello Lägg till formulärbaserad webbplats i dialogrutan Ange hello URL toohello inloggningssidan i hello överförings-URL för fältet och ange ett programnamn (valfritt). hello programnamn i Azure Multi-Factor Authentication-rapporter och kan visas i autentiseringsmeddelanden SMS eller Mobilapp.
8. Välj hello rätt format för förfrågan. Detta är inställt för**POST eller GET** för de flesta webbprogram.
9. Ange hello variabel för användarnamn, lösenordsvariabeln och domän variabel (om det visas på inloggningssidan för hello). toofind hello namnen på hello indata rutor, navigera toohello inloggningssidan i en webbläsare, högerklicka på hello sida och välj **Visa källa**.
10. Kontrollera hello **Azure Multi-Factor Authentication-användarmatchning** om alla användare har eller importeras till hello Server och ämne toomulti-factor-autentisering. Om ett stort antal användare inte har ännu importerats till hello Server och/eller undantas från Multi-Factor authentication, lämnar du hello kryssrutan avmarkerad.
11. Klicka på **Avancerat** tooreview avancerade inställningar, inklusive:

  - Välj en anpassad nekandeväxlingsfil
  - Cache lyckade autentiseringar toohello webbplats för en viss tidsperiod med hjälp av cookies
  - Välj om tooauthenticate hello primära autentiseringsuppgifter mot en Windows-domän, LDAP-katalogen. eller RADIUS-servern.

12. Klicka på **OK** tooreturn toohello i dialogrutan Lägg till formulärbaserad webbplats.
13. Klicka på **OK**.
14. En gång hello URL och sidvariabler har identifierats eller angett, hello webbplats data som visas i hello formulärbaserad panelen.

## <a name="using-integrated-windows-authentication-with-azure-multi-factor-authentication-server"></a>Använda integrerad Windows-autentisering med Azure Multi-Factor Authentication Server
toosecure en IIS-webbprogram som använder integrerad Windows-HTTP-autentisering, installera hello Azure MFA-Server på hello IIS-webbserver och sedan konfigurera hello Server med hello följande steg:

1. Klicka på hello IIS-autentisering ikonen hello vänstra menyn i hello Azure Multi-Factor Authentication-servern.
2. Klicka på hello **HTTP** fliken.
3. Klicka på **Lägg till**.
4. Ange hello URL för hello webbplats där HTTP-autentisering utförs (till exempel http://localhost/owa) i hello dialogruta för Lägg till grundläggande Webbadress, och ange ett programnamn (valfritt). hello programnamn i Azure Multi-Factor Authentication-rapporter och kan visas i autentiseringsmeddelanden SMS eller Mobilapp.
5. Justera hello timeout för inaktivitet och maximalt tidsgränsen om hello standard inte är tillräcklig.
6. Kontrollera hello **Multi-Factor Authentication-användarmatchning** om alla användare har eller importeras till hello Server och ämne toomulti-factor-autentisering. Om ett stort antal användare inte har ännu importerats till hello Server och/eller undantas från Multi-Factor authentication, lämnar du hello kryssrutan avmarkerad.
7. Kontrollera hello **Cookie cache** rutan om du vill.
8. Klicka på **OK**.

## <a name="enable-iis-plug-ins-for-azure-multi-factor-authentication-server"></a>Aktivera IIS plugin-program för Azure Multi-Factor Authentication Server
När du har konfigurerat hello formulärbaserad eller URL: er för HTTP-autentisering och inställningar, väljer hello platser där hello Azure Multi-Factor Authentication IIS plugin-program ska läsas in och aktiverat i IIS. Använd hello nedan:

1. Om körs på IIS 6, klickar du på hello **ISAPI** fliken. Välj hello-webbplats som hello webbprogram körs under (t.ex. standardwebbplats) tooenable hello Azure Multi-Factor Authentication ISAPI-filtret plugin-program för platsen.
2. Om körs på IIS 7 eller senare, klickar du på hello **ursprunglig modul** fliken. Välj hello server, webbplatser eller program tooenable hello IIS plugin-programmet på hello önskad nivåer.
3. Klicka på hello **aktivera IIS-autentisering** rutan hello överst i hello-skärmen. Azure Multi-Factor Authentication är nu att säkra hello valt IIS-program. Se till att användarna har importerats till hello Server.

## <a name="trusted-ips"></a>Tillförlitliga IP-adresser
hello kan tillförlitliga IP-adresser användare toobypass Azure Multi-Factor Authentication för webbplatsförfrågningar som kommer från särskilda IP-adresser eller undernät. Exempelvis kanske tooexempt användare från Azure Multi-Factor Authentication under inloggningen från hello office. För det här kan du ange hello office undernät som en betrodd IP-adresser. tooconfigure tillförlitliga IP-adresser, Använd hello nedan:

1. I hello IIS-autentisering-avsnittet, klickar du på hello **tillförlitliga IP-adresser** fliken.
2. Klicka på **Lägg till**.
3. När dialogrutan Lägg till tillförlitliga IP-adresser för hello visas väljer du hello **enda IP**, **IP-intervall**, eller **undernät** knappen.
4. Ange hello IP-adressen, IP-adressintervall eller undernät som ska vara godkända. Om att ange ett undernät, Välj hello lämpliga nätmask och klicka på **OK**. hello godkända har nu lagts till.
