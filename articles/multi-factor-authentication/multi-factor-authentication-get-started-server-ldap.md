---
title: aaaLDAP autentisering och Azure MFA-Server | Microsoft Docs
description: "Detta är hello Azure Multi-Factor authentication sida som är till hjälp vid distribution av LDAP-autentisering och Azure Multi-Factor Authentication-servern."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: e1a68568-53d1-4365-9e41-50925ad00869
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2017
ms.author: kgremban
ms.openlocfilehash: 17a26b57dbf6afa2fcfdb3d19c5b5ba2987a9f79
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="ldap-authentication-and-azure-multi-factor-authentication-server"></a>LDAP-autentisering och Azure Multi-Factor Authentication-servern
Som standard hello Azure Multi-Factor Authentication-servern är konfigurerad tooimport eller synkronisera användare från Active Directory. Det kan dock vara konfigurerade toobind toodifferent LDAP-kataloger, till exempel en ADAM-katalog eller en specifik Active Directory-domänkontrollant. När anslutna tooa directory via LDAP, hello Azure Multi-Factor Authentication-Server kan fungera som en LDAP-proxy tooperform autentiseringar. Dessutom kan hello användningen av LDAP-bindning som ett RADIUS-mål för autentisering av användare med IIS-autentisering eller för primär autentisering i användarportalen för hello Azure MFA.

toouse Azure Multi-Factor Authentication som en LDAP-proxy Infoga hello Azure Multi-Factor Authentication-servern mellan hello LDAP-klienten (t.ex, VPN-enhet, program) och hello LDAP-katalogserver. hello Azure Multi-Factor Authentication-servern måste vara konfigurerade toocommunicate med både klientservrar hello och hello LDAP-katalogen. I den här konfigurationen hello Azure Multi-Factor Authentication-servern accepterar LDAP-förfrågningar från klientservrar och program och vidarebefordrar dem toohello mål LDAP-katalogen toovalidate hello primära autentiseringsuppgifter. Om hello LDAP-katalogen validerar hello primära autentiseringsuppgifter, utför en andra identitetsverifiering Azure Multi-Factor Authentication och skickar en svar tillbaka toohello LDAP-klient. hello hela autentiseringen lyckas bara om både hello LDAP-server-autentisering och hello andra steg verifiering lyckas.

## <a name="configure-ldap-authentication"></a>Konfigurera LDAP-autentisering
tooconfigure LDAP-autentisering, installera hello Azure Multi-Factor Authentication-servern på en Windows server. Använd hello nedan:

### <a name="add-an-ldap-client"></a>Lägg till en LDAP-klient

1. Välj hello LDAP-autentisering ikonen hello vänstra menyn i hello Azure Multi-Factor Authentication-servern.
2. Kontrollera hello **Aktivera LDAP-autentisering** kryssrutan.

   ![LDAP-autentisering](./media/multi-factor-authentication-get-started-server-ldap/ldap2.png)

3. Ändra hello TCP-port och SSL-port hello klienter på fliken om hello Azure Multi-Factor Authentication LDAP-tjänsten ska bindas toonon standard portar toolisten för LDAP-förfrågningar.
4. Om du planerar toouse LDAPS från hello klienten toohello Azure Multi-Factor Authentication-servern ett SSL-certifikat måste installeras på hello samma server som MFA-servern. Klicka på **Bläddra** nästa toohello SSL certifikat kryssrutan och välj ett certifikat toouse för hello säker anslutning.
5. Klicka på **Lägg till**.
6. Hello Lägg till LDAP-klient i dialogrutan Ange hello IP-adressen för hello installation, server eller program som autentiserar toohello Server och ett programnamn (valfritt). hello programnamn i Azure Multi-Factor Authentication-rapporter och kan visas i autentiseringsmeddelanden SMS eller Mobilapp.
7. Kontrollera hello **Azure Multi-Factor Authentication-användarmatchning** om alla användare har eller importeras till hello Server och ämne tootwo steg kontroll. Om ett stort antal användare inte har ännu importerats till hello Server och/eller undantas från tvåstegsverifiering, lämnar du hello kryssrutan avmarkerad. Se hello MFA-servern hjälpfilen för mer information om den här funktionen.

Upprepa dessa steg tooadd ytterligare LDAP-klienter.

### <a name="configure-hello-ldap-directory-connection"></a>Konfigurera hello LDAP-anslutning

När hello Azure Multi-Factor Authentication är konfigurerade tooreceive LDAP-autentiseringar, måste proxy dessa autentiseringar toohello LDAP-katalogen. Därför hello målflik visas en enda bara nedtonad alternativet toouse ett LDAP-mål.

1. tooconfigure hello LDAP-anslutning, klicka på hello **katalogintegrering** ikon.
2. På fliken Inställningar hello väljer hello **Använd specifik LDAP-konfiguration** knappen.
3. Välj **Redigera...**
4. Hello dialogrutan Redigera LDAP-konfiguration, fylla hello fält med hello information som krävs för tooconnect toohello LDAP-katalogen. Beskrivningar av hello fält ingår i hjälpfilen för hello Azure Multi-Factor Authentication-servern.

    ![Katalogintegrering](./media/multi-factor-authentication-get-started-server-ldap/ldap.png)

5. Testa hello LDAP-anslutning genom att klicka på hello **Test** knappen.
6. Om hello LDAP Anslutningstestet lyckades, klickar du på hello **OK** knappen.
7. Klicka på hello **filter** fliken hello Server är förkonfigurerade tooload behållare, säkerhetsgrupper och användare från Active Directory. Om bindning tooa olika LDAP-katalogen, måste du förmodligen tooedit hello filter visas. Klicka på hello **hjälp** länk för mer information om filter.
8. Klicka på hello **attribut** fliken hello Server är förkonfigurerade toomap attribut från Active Directory.
9. Om du bindning tooa olika LDAP-katalog eller toochange hello förkonfigurerade attributmappning, klickar du på **redigera...**
10. Hello Redigera attribut i dialogrutan Ändra hello LDAP-mappningar för din katalog. Attributnamn kan skrev i eller genom att klicka hello **...** knappen Nästa tooeach fält. Klicka på hello **hjälp** länk för mer information om attribut.
11. Klicka på hello **OK** knappen.
12. Klicka på hello **Företagsinställningar** ikon och väljer hello **användarnamn** fliken.
13. Om du ansluter tooActive Directory från en domänansluten server, lämnar hello **med Windows säkerhetsidentifierare (SID) för att matcha användarnamn** alternativknappen är markerad. Annars, Välj hello **unikt identifierarattribut för LDAP för att matcha användarnamn** knappen. 

När hello **unikt identifierarattribut för LDAP för att matcha användarnamn** alternativknappen är markerad, hello Azure Multi-Factor Authentication-servern försöker tooresolve varje användarnamn tooa Unik identifierare i hello LDAP-katalogen. En LDAP-sökning utförs på hello Username-attribut som definieras i hello katalogintegrering > fliken attribut. När en användare autentiseras är hello användarnamnet löst toohello Unik identifierare i hello LDAP-katalogen. Unik identifierare för hello används för matchande hello användaren i hello Azure Multi-Factor Authentication-datafilen. Detta möjliggör skiftlägeskänsliga jämförelser och långa och korta användarnamnformat.

När du har slutfört de här stegen konfigurerats hello MFA-servern lyssnar på hello konfigurerade portar för LDAP-förfrågningar från hello klienter och fungerar som proxy för de begär toohello LDAP-katalogen för autentisering.

## <a name="configure-ldap-client"></a>Konfigurera LDAP-klient
tooconfigure hello LDAP-klient, Använd hello riktlinjer:

* Konfigurera din enhet, server eller program tooauthenticate via LDAP toohello Azure Multi-Factor Authentication-servern som om det vore LDAP-katalogen. Använd hello samma inställningar som du använder normalt tooconnect direkt tooyour LDAP-katalogen, förutom hello servernamn eller IP-adress som ska vara som hello Azure Multi-Factor Authentication-servern.
* Konfigurera hello LDAP-timeout too30 60 sekunder så att det är tid toovalidate hello användarens autentiseringsuppgifter med hello LDAP-katalogen, utför hello andra steg kontrollen, deras svar och svara toohello LDAP-åtkomst-begäran.
* Om du använder LDAPS måste hello installation eller server hello LDAP-frågor lita hello SSL-certifikat installeras på hello Azure Multi-Factor Authentication-servern.

