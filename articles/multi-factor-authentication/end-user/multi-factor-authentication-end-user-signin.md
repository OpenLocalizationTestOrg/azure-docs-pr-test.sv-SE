---
title: "aaaAzure MFA-inloggning med tvåstegsverifiering | Microsoft Docs"
description: "Den här sidan ger du riktlinjer för där toogo toosee hello olika inloggning metoder med Azure MFA."
keywords: "autentisering av användare, inloggning, logga in med mobiltelefon, logga in med Arbetstelefon"
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: b310b762-471b-4b26-887a-a321c9e81d46
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2017
ms.author: kgremban
ms.reviewer: librown
ms.custom: end-user
ms.openlocfilehash: fcd5eb5e8426eda537db9e099bf247bde29c195b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="hello-sign-in-experience-with-azure-multi-factor-authentication"></a>hello inloggning med Azure Multi-Factor Authentication
> [!NOTE]
> hello syftet med den här artikeln är toowalk via en typisk inloggningen. Hjälp med att logga in eller tootroubleshoot problem finns i [har problem med Azure Multi-Factor Authentication](multi-factor-authentication-end-user-troubleshoot.md).

## <a name="what-will-your-sign-in-experience-be"></a>Vad är din inloggning?
Din inloggning skiljer sig åt beroende på vad du väljer toouse som din andra faktor: ett telefonsamtal eller ett authentication-appen texter. Välj hello-alternativ som bäst beskriver vad du vill göra:

| Hur loggar du in? | 
| --- |
| [Med ett telefonsamtal toomy mobile- eller office telefon](#signing-in-with-a-phone-call) |
| [Med en text toomy mobiltelefon](#signing-in-with-a-text-message)
| [Med meddelanden från hello Microsoft Authenticator-appen](#signing-in-with-the-microsoft-authenticator-app-using-notification) |
| [Med verifieringskoder från hello Microsoft Authenticator-appen](#signing-in-with-the-microsoft-authenticator-app-using-verification-code) |
| [Med en alternativ metod, eftersom det inte kan använda min verifieringsmetod just nu](#signing-in-with-an-alternate-method) |

## <a name="signing-in-with-a-phone-call"></a>Logga in med ett telefonsamtal
hello följande information beskriver hello tvåstegsverifiering verifiering erfarenhet av ett anrop tooyour mobile eller Arbetstelefon.

1. Logga in tooan program eller tjänst, till exempel Office 365 med ditt användarnamn och lösenord.  
2. Microsoft anropar du.  
3. Besvara hello telefon och tryck hello #-knappen.  

## <a name="signing-in-with-a-text-message"></a>Logga in med ett textmeddelande
hello följande information beskriver hello tvåstegsverifiering verifiering erfarenhet av text meddelandet tooyour mobiltelefon:

1. Logga in tooan program eller tjänst, till exempel Office 365 med ditt användarnamn och lösenord. 
2. Microsoft skickar ett textmeddelande som innehåller ett antal kod. 
3. Ange hello kod i hello rutan på hello-inloggningssida. 

## <a name="signing-in-with-hello-microsoft-authenticator-app"></a>Logga in med hello Microsoft Authenticator-appen 
hello beskriver följande information hello upplevelse av hello Microsoft Authenticator-appen för tvåstegsverifiering kontroller. Det finns två olika sätt toouse hello app. Du kan ta emot push-meddelanden på enheten eller så kan du öppna hello app tooget en Verifieringskod.

### <a name="toosign-in-with-a-notification-from-hello-microsoft-authenticator-app"></a>toosign in med ett meddelande från hello Microsoft Authenticator-appen
1. Logga in tooan program eller tjänst, till exempel Office 365 med ditt användarnamn och lösenord.
2. Microsoft skickar ett meddelande toohello Microsoft Authenticator-appen på enheten.

  ![Microsoft skickar meddelanden](./media/multi-factor-authentication-end-user-signin/notify.png)

3. Öppna hello-meddelande på din telefon och välj hello **Kontrollera** nyckel. Om ditt företag kräver en PIN-kod kan du ange det här.
4. Du bör nu vara inloggad.

### <a name="toosign-in-using-a-verification-code-with-hello-microsoft-authenticator-app"></a>toosign in med en Verifieringskod har hello Microsoft Authenticator-appen

Om du använder hello Microsoft Authenticator app tooget verifieringskoder sedan visas när du öppnar hello app ett tal under namnet på ditt konto. Det här talet ändras med 30 sekunders mellanrum så att du inte använder hello samma tal två gånger. När du uppmanas för en Verifieringskod öppna hello app och använda tal som visas för närvarande. 

1. Logga in tooan program eller tjänst, till exempel Office 365 med ditt användarnamn och lösenord.
2. Microsoft efterfrågar en Verifieringskod.

  ![Ange verifieringskoden](./media/multi-factor-authentication-end-user-signin/verify3.png)

3. Öppna hello Microsoft Authenticator-appen på din telefon och ange koden hello hello i rutan där du loggar in.

## <a name="signing-in-with-an-alternate-method"></a>Logga in med en alternativ metod
Du saknar ibland hello telefon eller enhet som du ställer in som din primära verifieringsmetod. Den här situationen är därför rekommenderar vi att du konfigurerar metoder för säkerhetskopiering för ditt konto. hello följande avsnitt visas hur toosign in med en alternativ metod när den primära metoden är inte tillgängliga.

1. Logga in tooan program eller tjänst, till exempel Office 365 med ditt användarnamn och lösenord.
2. Välj **använder ett annat verifieringsalternativ**. Du kan se olika verifieringsalternativ baserat på hur många du har konfigurerat.
3. Välj en alternativ metod och logga in.

  ![Använd alternativ metod](./media/multi-factor-authentication-end-user-signin/alt.png)

## <a name="next-steps"></a>Nästa steg

Om du har problem med att logga in med tvåstegsverifiering kan få mer information på [har problem med Azure Multi-Factor Authentication](multi-factor-authentication-end-user-troubleshoot.md).

Lär dig hur för[hantera dina inställningar för tvåstegsverifiering](multi-factor-authentication-end-user-manage-settings.md).

Ta reda på hur för[Kom igång med hello Microsoft Authenticator-appen](microsoft-authenticator-app-how-to.md) så att du kan använda meddelanden toosign i, i stället för texter och telefonsamtal. 
