---
title: aaaRADIUS autentisering och Azure MFA-Server | Microsoft Docs
description: "Det här är hello Azure Multi-Factor authentication sida som hjälper distribuera RADIUS-autentisering och Azure Multi-Factor Authentication-servern."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: f4ba0fb2-2be9-477e-9bea-04c7340c8bce
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/26/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: H1Hack27Feb2017, it-pro
ms.openlocfilehash: dac061b83f2657c67192a7aa9c5de63ffeffaaa8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-radius-authentication-with-azure-multi-factor-authentication-server"></a>Integrera RADIUS-autentisering och Azure Multi-Factor Authentication Server
Använd hello RADIUS-autentisering avsnitt i Azure MFA-Server tooenable och konfigurera RADIUS-autentisering. RADIUS är en standard för protokollet tooaccept autentiseringsbegäranden och tooprocess dessa förfrågningar. hello Azure Multi-Factor Authentication-servern fungerar som en RADIUS-server. Infoga det mellan RADIUS-klienten (VPN-installation) och autentiseringsmål-, som kan vara Active Directory (AD), en LDAP-katalog eller en annan RADIUS-server tooadd Azure Multi-Factor Authentication. Azure Multi-Factor Authentication (MFA) toofunction måste du konfigurera hello Azure MFA-servern så att den kan kommunicera med både klientservrar hello och hello-autentiseringsmål. hello Azure MFA-servern accepterar begäranden från RADIUS-klient, verifierar autentiseringsuppgifter mot hello-autentiseringsmål, lägger till Azure Multi-Factor Authentication och skickar en svar tillbaka toohello RADIUS-klient. hello autentiseringsbegäran lyckas bara om både hello primär autentisering och hello Azure Multi-Factor Authentication lyckas.

> [!NOTE]
> Hej MFA-Server endast stöder PAP (lösenord authentication protocol) och MSCHAPv2 (Microsoft Challenge Handshake Authentication Protocol) RADIUS protokoll när fungerar som en RADIUS-server.  Andra protokoll, som EAP (extensible authentication protocol), som kan användas när hello MFA-servern fungerar som en RADIUS-proxy tooanother RADIUS-server som har stöd för protokollet.
>
> I den här konfigurationen fungerar inte enkelriktad SMS och OATH-token eftersom hello MFA-servern inte kan initiera lyckade RADIUS-svar med hjälp av alternativa protokoll.

![RADIUS-autentisering](./media/multi-factor-authentication-get-started-server-rdg/radius.png)

## <a name="add-a-radius-client"></a>Lägga till en RADIUS-klient
tooconfigure RADIUS-autentisering, installera hello Azure Multi-Factor Authentication-servern på en Windows server. Om du har en Active Directory-miljö ska hello server vara domänansluten toohello i hello nätverk. Använd hello följa proceduren tooconfigure hello Azure Multi-Factor Authentication-servern:

1. Klicka på hello RADIUS-autentisering ikonen hello vänstra menyn i hello Azure Multi-Factor Authentication-servern.
2. Kontrollera hello **aktivera RADIUS-autentisering** kryssrutan.
3. Ändra hello autentisering- och redovisningsportar hello klienter på fliken om hello Azure MFA RADIUS tjänsten behöver toolisten för RADIUS-begäranden på portar som inte är standard.
4. Klicka på **Lägg till**.
5. Ange hello IP-adressen hello enheten/servern som autentiserar toohello Azure Multi-Factor Authentication-servern och ett programnamn (valfritt) en delad hemlighet.

  hello programnamn i Azure Multi-Factor Authentication-rapporter och kan visas i autentiseringsmeddelanden SMS eller Mobilapp.

  hello delade hemliga behov toobe hello samma på båda hello Azure Multi-Factor Authentication-servern och enheten/servern.

6. Kontrollera hello **Multi-Factor Authentication-användarmatchning** om alla användare har eller importeras till hello Server och ämne toomulti-factor-autentisering. Om ett stort antal användare inte har ännu importerats till hello Server och/eller ska undantas från tvåstegsverifiering, lämnar du hello kryssrutan avmarkerad.
7. Kontrollera hello **aktivera återställningsplats OATH-token** rutan om du vill toouse OATH-lösenord från mobil verifiering appar som en reserv toohello out-of-band telefonsamtal, SMS, eller push-meddelande.
8. Klicka på **OK**.

Upprepa steg 4 till 8 tooadd så många ytterligare RADIUS-klienter som du behöver.

## <a name="configure-your-radius-client"></a>Konfigurera RADIUS-klienten

1. Klicka på hello **mål** fliken.
2. Om hello Azure MFA-Server är installerad på en domänansluten server i en Active Directory-miljö, väljer du Windows-domän.
3. Om användarna ska autentiseras mot en LDAP-katalog väljer du **LDAP-bindning**.

  toouse LDAP-bindning, klickar du på ikonen för hello katalogintegrering och redigera hello LDAP-konfiguration på hello-inställningar så att hello servern kan binda tooyour directory. Instruktioner för att konfigurera LDAP hittar du i hello [LDAP-Proxy konfigurationsguiden](multi-factor-authentication-get-started-server-ldap.md).

4. Om användarna ska verifieras mot en annan RADIUS-server väljer du RADIUS-server.
5. Klicka på **Lägg till** tooconfigure hello server toowhich hello Azure MFA-Server kommer proxy hello RADIUS-begäranden.
6. Ange hello IP-adressen för hello RADIUS-server och en delad hemlighet hello Lägg till RADIUS-Server i dialogrutan.

  hello delade hemliga behov toobe hello samma på båda hello Azure Multi-Factor Authentication-servern och RADIUS-servern. Ändra hello autentiseringsport och redovisningsporten om olika portar som används av hello RADIUS-server.

7. Klicka på **OK**.
8. Lägg till hello Azure MFA-Server som en RADIUS-klient i hello annan RADIUS-server så att den kan bearbeta förfrågningar skickas tooit från hello Azure MFA-Server. Använd hello samma delade hemlighet som konfigurerats i hello Azure Multi-Factor Authentication-servern.

Upprepa dessa steg tooadd flera RADIUS-servrar och konfigurera hello ordning i vilken hello Azure MFA-servern ska ringa upp dem med hello **Flytta upp** och **Flytta ned** knappar.

Detta avslutar hello Azure Multi-Factor Authentication-konfigurationen. hello servern lyssnar nu på hello konfigurerade portar för RADIUS-förfrågningar från klienter hello konfigurerats.   

## <a name="radius-client-configuration"></a>RADIUS-klientkonfiguration
tooconfigure hello RADIUS-klient, Använd hello riktlinjer:

* Konfigurera din enhet/server tooauthenticate via RADIUS toohello Azure Multi-Factor Authentication-serverns IP-adress som ska fungera som hello RADIUS-server.
* Använd hello samma delade hemlighet som har konfigurerats tidigare.
* Konfigurera hello RADIUS timeout too30 60 sekunder så att det är tid toovalidate hello användarens autentiseringsuppgifter, utföra tvåstegsverifiering, deras svar och svara toohello RADIUS-åtkomstbegäran.
