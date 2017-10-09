---
title: aaaUse Azure MFA-Server med AD FS 2.0 | Microsoft Docs
description: "Det här är hello Azure Multi-Factor authentication sida som beskriver hur tooget igång med Azure MFA och AD FS 2.0."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 96168849-241a-4499-a224-d829913caa7e
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/14/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: H1Hack27Feb2017, it-pro
ms.openlocfilehash: 15011a94ca69dc6e51aa3edf74f5db6cd44d697a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-azure-multi-factor-authentication-server-toowork-with-ad-fs-20"></a>Konfigurera Azure-flerfunktionsautentisering toowork med AD FS 2.0
Den här artikeln är för organisationer som federerade med Azure Active Directory och vill toosecure resurser som är lokalt eller i hello molnet. Skydda dina resurser med hjälp av hello Azure Multi-Factor Authentication-servern och konfigurera den toowork med AD FS så att tvåstegsverifiering utlöses för högt värde slutpunkter.

Den här dokumentationen visar hello Azure Multi-Factor Authentication-Server med AD FS 2.0. Mer information om AD FS finns i [Skydda molnresurser och lokala resurser med Azure Multi-Factor Authentication Server med Windows Server 2012 R2 AD FS](multi-factor-authentication-get-started-adfs-w2k12.md).

## <a name="secure-ad-fs-20-with-a-proxy"></a>Skydda AD FS 2.0 med en proxy
toosecure AD FS 2.0 med en proxy, installera hello Azure Multi-Factor Authentication-servern på hello AD FS-proxyservern.

### <a name="configure-iis-authentication"></a>Konfigurera IIS-autentisering
1. Hello Azure Multi-Factor Authentication-servern, klicka på hello **IIS-autentisering** ikonen hello vänstra menyn.
2. Klicka på hello **formulärbaserad** fliken.
3. Klicka på **Lägg till**.

   <center>![Installation](./media/multi-factor-authentication-get-started-adfs-adfs2/setup1.png)</center>

4. toodetect användarnamn, lösenord och domän variabler automatiskt, ange hello inloggnings-URL (t.ex. https://sso.contoso.com/adfs/ls) i dialogrutan för hello Konfigurera automatiskt formulärbaserad webbplats och klicka på **OK**.
5. Kontrollera hello **Azure Multi-Factor Authentication-användarmatchning** om alla användare har eller importeras till hello Server och ämne tootwo steg kontroll. Om ett stort antal användare inte har ännu importerats till hello Server och/eller ska undantas från tvåstegsverifiering, lämnar du hello kryssrutan avmarkerad.
6. Om hello sidvariablerna inte kan identifieras automatiskt, klickar du på hello **ange manuellt...** knappen hello Konfigurera automatiskt formulärbaserad webbplats i dialogrutan.
7. Hello Lägg till formulärbaserad webbplats i dialogrutan Ange hello URL toohello AD FS-inloggningssidan i hello överförings-URL-fältet (till exempel https://sso.contoso.com/adfs/ls) och ange ett programnamn (valfritt). hello programnamn i Azure Multi-Factor Authentication-rapporter och kan visas i autentiseringsmeddelanden SMS eller Mobilapp.
8. Ange hello begäran format för**POST eller GET**.
9. Ange hello användarnamn (ctl00$ ContentPlaceHolder1$ UsernameTextBox) och lösenord variabel (ctl00$ ContentPlaceHolder1$ PasswordTextBox). Om din formulärbaserad inloggningssidan visas i en textruta i domänen, ange hello domän-och variabel. toofind hello namnen på hello inkommande rutor på inloggningssidan för hello gå toohello inloggningssidan i en webbläsare, högerklicka på hello sida och välj **Visa källa**.
10. Kontrollera hello **Azure Multi-Factor Authentication-användarmatchning** om alla användare har eller importeras till hello Server och ämne tootwo steg kontroll. Om ett stort antal användare inte har ännu importerats till hello Server och/eller ska undantas från tvåstegsverifiering, lämnar du hello kryssrutan avmarkerad.
    <center>![Installation](./media/multi-factor-authentication-get-started-adfs-adfs2/manual.png)</center>
11. Klicka på **Avancerat...** tooreview avancerade inställningar. Bland de inställningar som du kan konfigurera finns följande:

    - Välj en anpassad nekandeväxlingsfil
    - Cache lyckade autentiseringar toohello webbplats som använder cookies
    - Välj hur tooauthenticate hello primära autentiseringsuppgifter

12. Eftersom hello AD FS-proxyservern inte troligtvis toobe anslutit toohello domän kan du använda LDAP tooconnect tooyour domänkontrollant för att importera användare och förautentisering. I dialogrutan för hello formulärbaserad webbplats klickar du på hello **primär autentisering** fliken och markera **LDAP-bindning** för hello förautentisering autentiseringstyp.
13. När du är klar klickar du på **OK** tooreturn toohello i dialogrutan Lägg till formulärbaserad webbplats.
14. Klicka på **OK** tooclose hello dialogrutan.
15. En gång hello URL och sidvariabler har identifierats eller angett, hello webbplats data som visas i hello formulärbaserad panelen.
16. Klicka på hello **ursprunglig modul** fliken och markera hello server, hello-webbplats som hello AD FS-proxy körs under (som ”Default Web Site”), eller hello AD FS-proxy programmet (till exempel ”ls” under ”adfs”) tooenable hello IIS plugin-programmet på hello önskad nivå.
17. Klicka på hello **aktivera IIS-autentisering** rutan hello överst i hello-skärmen.

hello IIS-autentisering har nu aktiverats.

### <a name="configure-directory-integration"></a>Konfigurera katalogintegrering
Du har aktiverat IIS-autentisering, men tooperform hello förautentisering tooyour Active Directory (AD) via LDAP måste du konfigurera hello LDAP-anslutningen toohello-domänkontrollant.

1. Klicka på hello **katalogintegrering** ikon.
2. På fliken Inställningar hello väljer hello **Använd specifik LDAP-konfiguration** knappen.

   <center>![Installation](./media/multi-factor-authentication-get-started-adfs-adfs2/ldap1.png)</center>

3. Klicka på **Redigera**.
4. Hello dialogrutan Redigera LDAP-konfiguration, fylla hello fält med hello information krävs tooconnect toohello AD-domänkontrollant. Beskrivningar av hello fält ingår i hjälpfilen för hello Azure Multi-Factor Authentication-servern.
5. Testa hello LDAP-anslutning genom att klicka på hello **Test** knappen.

   <center>![Installation](./media/multi-factor-authentication-get-started-adfs-adfs2/ldap2.png)</center>

6. Om hello LDAP Anslutningstestet lyckades, klickar du på **OK**.

### <a name="configure-company-settings"></a>Konfigurera företagsinställningar
1. Klicka sedan på hello **Företagsinställningar** ikon och väljer hello **användarnamn** fliken.
2. Välj hello **unikt identifierarattribut för LDAP för att matcha användarnamn** knappen.
3. Om användarna anger sina användarnamn i formatet ”domän\användarnamn”, måste hello Server toobe kan toostrip hello domän ut hello användarnamn när den skapar hello LDAP-fråga. Detta kan göras via en registerinställning.
4. Öppna hello Registereditorn och gå tooHKEY_LOCAL_MACHINE SOFTWARE/Wow6432Node/positiva nätverk/PhoneFactor på en 64-bitars server. Om en 32-bitars server vidta hello ”Wow6432Node” utanför hello sökväg. Skapa en DWORD registernyckeln kallas ”UsernameCxz_stripPrefixDomain” och ange hello värdet too1. Azure Multi-Factor Authentication är nu att säkra hello AD FS-proxy.

Se till att användarna har importerats från Active Directory till hello Server. Se hello [tillförlitliga IP-adresser avsnittet](#trusted-ips) om du vill att toowhitelist interna IP-adresser så att tvåstegsverifiering inte krävs när du loggar in toohello webbplatsen från dessa platser.

<center>![Installation](./media/multi-factor-authentication-get-started-adfs-adfs2/reg.png)</center>

## <a name="ad-fs-20-direct-without-a-proxy"></a>AD FS 2.0 direkt utan någon proxy
Du kan skydda AD FS när hello AD FS-proxy inte används. Installera hello Azure Multi-Factor Authentication-servern på hello AD FS-servern och konfigurera hello Server per hello följande steg:

1. Inom hello Azure Multi-Factor Authentication-servern, klickar du på hello **IIS-autentisering** ikonen hello vänstra menyn.
2. Klicka på hello **HTTP** fliken.
3. Klicka på **Lägg till**.
4. Ange hello URL för hello AD FS-webbplats där HTTP-autentisering utförs (till exempel https://sso.domain.com/adfs/ls/auth/integrated) till hello bas-URL-fältet i hello dialogruta för Lägg till grundläggande Webbadress. Ange sedan ett programnamn (valfritt). hello programnamn i Azure Multi-Factor Authentication-rapporter och kan visas i autentiseringsmeddelanden SMS eller Mobilapp.
5. Om du vill justera hello timeout för inaktivitet och maximalt tidsgränsen.
6. Kontrollera hello **Azure Multi-Factor Authentication-användarmatchning** om alla användare har eller importeras till hello Server och ämne tootwo steg kontroll. Om ett stort antal användare inte har ännu importerats till hello Server och/eller ska undantas från tvåstegsverifiering, lämnar du hello kryssrutan avmarkerad.
7. Kryssrutan hello cookie cachen om du vill.

   <center>![Installation](./media/multi-factor-authentication-get-started-adfs-adfs2/noproxy.png)</center>

8. Klicka på **OK**.
9. Klicka på hello **ursprunglig modul** fliken och väljer hello server, hello webbplats (som ”Default Web Site”) eller hello AD FS programmet (till exempel ”ls” under ”adfs”) tooenable hello IIS plugin-programmet på hello önskad nivå.
10. Klicka på hello **aktivera IIS-autentisering** rutan hello överst i hello-skärmen.

Nu skyddas AD FS av Azure Multi-Factor Authentication.

Se till att användarna har importerats från Active Directory till hello Server. Se hello tillförlitliga IP-adresser avsnittet om du vill att toowhitelist interna IP-adresser så att tvåstegsverifiering inte krävs när du loggar in toohello webbplatsen från dessa platser.

## <a name="trusted-ips"></a>Tillförlitliga IP-adresser
Tillförlitliga IP-adresser kan användare toobypass Azure Multi-Factor Authentication för webbplatsförfrågningar som kommer från särskilda IP-adresser eller undernät. Du kan exempelvis vilja tooexempt användare från tvåstegsverifiering när de loggar in från hello office. För det här kan du ange hello office undernät som en betrodd IP-adresser.

### <a name="tooconfigure-trusted-ips"></a>tooconfigure tillförlitliga IP-adresser
1. I hello IIS-autentisering-avsnittet, klickar du på hello **tillförlitliga IP-adresser** fliken.
2. Klicka på hello **Lägg till...** till.
3. När dialogrutan Lägg till tillförlitliga IP-adresser för hello visas väljer du något av hello **enda IP**, **IP-intervall**, eller **undernät** alternativknappar.
4. Ange hello IP-adress, intervall av IP-adresser och undernät som ska vara godkända. Om du registrerar ett undernät, Välj hello lämplig nätmask och klicka på hello **OK** knappen. hello betrodda IP har nu lagts till.

<center>![Installation](./media/multi-factor-authentication-get-started-adfs-adfs2/trusted.png)</center>
