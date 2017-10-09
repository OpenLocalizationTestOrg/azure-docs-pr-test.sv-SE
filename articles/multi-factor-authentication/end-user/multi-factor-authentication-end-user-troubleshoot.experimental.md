---
title: "aaaTroubleshoot tvåstegsverifiering | Microsoft Docs"
description: "Det här dokumentet ger användare information om vilka toodo om de stöter på ett problem med Azure Multi-Factor Authentication."
services: multi-factor-authentication
keywords: flerfunktionsautentisering klienten, problem med autentisering, Korrelations-ID
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 8f3aef42-7f66-4656-a7cd-d25a971cb9eb
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: end-user
ms.openlocfilehash: 65c25fb70bebeb5eff15ffe63ce11a65f5cdb1f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-help-with-two-step-verification"></a>Få hjälp med tvåstegsverifiering
Den här artikeln besvarar hello de vanligaste frågorna om tvåstegsverifiering. 

## <a name="why-do-i-have-tooperform-two-step-verification-can-i-turn-it-off"></a>Varför har tooperform tvåstegsverifiering? Kan jag inaktivera den?

Tvåstegsverifiering är en säkerhetsfunktion som din organisation valde toouse tooprotect dina konton. Det är säkrare än bara ett lösenord, eftersom den är beroende av två typer av autentisering: något du känner till och något du har med dig. hello är något användaren vet ditt lösenord. hello är något du har med dig en telefon eller en enhet som du ofta har med dig. När ditt konto skyddas med tvåstegsverifiering logga hackare inte in dig även om de komma åt ditt lösenord. De kan inte logga in eftersom de inte har åtkomst till tooyour phone. 

Microsoft erbjuder tvåstegsverifiering men organisationen väljer toouse hello-funktionen. Du kan välja om din IT-avdelning kräver av du, precis som du inte kan välja bort med hjälp av ett lösenord tooprotect ditt konto. 

Om du har aktiverat för ditt personliga Microsoft-konto för tvåstegsverifiering och vill att inställningarna för toochange läsa [om tvåstegsverifiering](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification) i stället. 

## <a name="i-dont-have-my-phone-with-me-today"></a>Jag har inte min telefon med mig idag

Vissa dagar du lämnar din telefon hemma, men fortfarande måste toosign i på arbetet. hello första försök logga in med en annan verifieringsmetod. När du registrerat för tvåstegsverifiering du ställa in mer än ett telefonnummer? logga in med en annan metod tootry så här:

1. Logga in som vanligt.
2. När hello tvåstegsverifiering verifiering sidan öppnas väljer **använder ett annat verifieringsalternativ**.

   ![Olika verifiering](./media/multi-factor-authentication-end-user-troubleshoot/diff_option.png)

3. Välj hello verifieringsalternativ som du vill toouse. 
  - Om du inte har åtkomst tooyour alternativa metoder antingen, kontakta din IT-avdelningen tooget hjälp logga in tooyour konto.
  - Om du har åtkomst tooyour alternativa metoder kan fortsätta med tvåstegsverifiering.

Om du inte ser hello **använder ett annat verifieringsalternativ** länkar, och det innebär att du inte konfigurera alternativa metoder när du först registrerade för tvåstegsverifiering. Kontakta din IT-avdelningen tooget hjälp logga in tooyour konto. När du har loggat in, kontrollera för[hantera dina inställningar](multi-factor-authentication-end-user-manage-settings.md) tooadd ytterligare verifieringsmetoder för nästa gång. 

## <a name="i-lost-my-phone-or-got-a-new-number"></a>Jag tappar bort min telefon eller har fått ett nytt nummer
Det finns två sätt tooget tillbaka i tooyour konto. hello är först toosign in med ditt alternativa autentisering telefonnummer om du har skapat en. hello andra är tooask din IT-avdelningen tooclear dina inställningar.

Om telefonen har tappas bort eller blir stulen, rekommenderar vi också att du tala om för din IT-avdelning. De måste tooreset dina applösenord och ta bort alla lagrade enheter. 

### <a name="use-an-alternate-phone-number"></a>Använda ett alternativt telefonnummer
Om du konfigurerar flera alternativ för verifiering, t.ex. ett sekundärt telefonnummer eller en autentiseringsapp på en annan enhet med någon av dem toosign i.

toosign med hello alternativt telefonnummer, Följ dessa steg:

1. Logga in som vanligt.
2. När du tillfrågas toofurther verifiera ditt konto, välja **använder ett annat verifieringsalternativ**.
   
   ![Olika verifiering](./media/multi-factor-authentication-end-user-troubleshoot/diff_option.png)

3. Välj hello telefonnummer eller enhet som du har åtkomst till.
4. När du är tillbaka i ditt konto, [hantera dina inställningar](multi-factor-authentication-end-user-manage-settings.md) toochange autentiseringen telefonnummer.

### <a name="clear-your-settings"></a>Rensa dina inställningar
Om du inte har konfigurerat ett telefonnummer för sekundära autentiseringen har toocontact IT-avdelningen för hjälp. Ha dem Rensa dina inställningar hello så nästa tid du loggar in uppmanas du att för[registrera dig för tvåstegsverifiering](multi-factor-authentication-end-user-first-time.md) igen.

## <a name="i-am-not-receiving-a-text-or-call-on-my-phone"></a>Jag kan inte ta emot en text eller anropa på min telefon
Det finns flera skäl till varför du kan försöka toosign i, men inte ta emot hello text eller telefonsamtal. Om du fått har text eller telefonsamtal tooyour phone hello tidigare, sedan det här problemet är förmodligen ett problem med hello phone provider, inte ditt konto. Kontrollera att du har en bra cell signal. Och om du försöker tooreceive ett textmeddelande kontrollerar du att du är kan tooreceive textmeddelanden. Be en vän toocall du eller text du som ett test. 

Om du har väntat flera minuter innan ett textmeddelande eller samtal är hello snabbaste sättet tooget till ditt konto tootry ett annat alternativ.

1. Välj **använder ett annat verifieringsalternativ** på hello sidor som väntar på att verifieringen.
   
    ![Olika verifiering](./media/multi-factor-authentication-end-user-troubleshoot/diff_option.png)
2. Markera hello phone tal eller leverans metod toouse.
   
    Använd hello senaste en om du har fått flera verifieringskoder.

Om du inte har någon annan metod som har konfigurerats, kontakta IT-avdelningen och be dem tooclear dina inställningar. hello nästa gång du loggar in, uppmanas du att för[konfigurera multifaktorautentisering](multi-factor-authentication-end-user-first-time.md) igen.

Om du har ofta fördröjningar på grund av toobad cell signal, rekommenderar vi du använder hello [Microsoft Authenticator-appen](microsoft-authenticator-app-how-to.md) på din smartphone. hello app kan generera slumpmässigt säkerhetskoder som du använder toosign i och dessa koder kräver inte någon cell signal eller internet-anslutning.

## <a name="app-passwords-are-not-working"></a>Applösenord fungerar inte
Kontrollera först att du har angett rätt hello applösenord. hello genereras applösenord ersätter lösenordet normal, men endast för äldre program som inte stöder tvåstegsverifiering. Om det fortfarande inte fungerar, försök logga in och [skapa ett nytt applösenord](multi-factor-authentication-end-user-app-passwords.md).  Om det fortfarande inte fungerar kontaktar du IT-avdelningen och ha dem [ta bort dina befintliga applösenord](../multi-factor-authentication-manage-users-and-devices.md) och du kan sedan skapa en ny.

## <a name="i-didnt-find-an-answer-toomy-problem"></a>Jag hittade ett svar toomy problem.
Om du har gjort dessa felsökningssteg men är fortfarande körs problem, kan du kontakta IT-avdelningen. De ska kunna tooassist du.

## <a name="related-topics"></a>Relaterade ämnen
* [Hantera inställningar för tvåstegsverifiering](multi-factor-authentication-end-user-manage-settings.md)  
* [Microsoft Authenticator program vanliga frågor och svar](microsoft-authenticator-app-faq.md)

