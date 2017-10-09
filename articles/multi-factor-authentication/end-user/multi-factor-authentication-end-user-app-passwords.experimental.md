---
title: "aaaHow toouse Applösenord i Azure MFA? | Microsoft Docs"
description: "Den här sidan hjälper användarna att förstå vad applösenord finns och hur de kan användas för med beaktande tooAzure MFA."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 345b757b-5a2b-48eb-953f-d363313be9e5
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: kgremban
ms.custom: end-user
ms.openlocfilehash: 3afa2003d8e87576f035bf9440a1dba67bd85f5b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-are-app-passwords-in-azure-multi-factor-authentication"></a>Vad är Applösenord i Azure Multi-Factor Authentication?
Vissa icke-webbläsarappar, till exempel hello Apple interna e-klient som använder Exchange Active Sync stöder för tillfället inte multifaktorautentisering. Multifaktorautentisering aktiveras per användare.  Detta innebär att en användare inte kan använda multifaktorautentisering om:

- hello användaren har aktiverats för multifaktorautentisering
- hello användaren försöker toouse en icke-Webbläsarprogrammet.

Ett applösenord kan hello användaren toouse hello app.

När du har ett applösenord kan använda du den i stället för det ursprungliga lösenordet med dessa icke-webbläsarbaserade appar. När du registrerar dig för tvåstegsverifiering du talar om Microsoft inte toolet någon logga in med ditt lösenord om de inte kan också utföra andra hello-verifiering. hello Apple interna e-postklienten på telefonen logga inte in dig eftersom den inte kan fråga efter tvåstegsverifiering. hello lösning toothis problemet är toocreate ett säkrare applösenord som du inte använder dagliga. Applösenord gäller endast de appar som inte stöder tvåstegsverifiering. Använd hello lösenord så att appar kan hoppa över multifaktorautentisering och fortsätta toowork.


> [!NOTE]
> Office 2013-klienter (inklusive Outlook) stöder nya autentiseringsprotokoll och kan användas med tvåstegsverifiering. Applösenord krävs inte för användning med Office 2013-klienter.  Mer information finns i [Office 2013 modern autentisering offentlig förhandsgranskning av](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).


## <a name="how-toouse-app-passwords"></a>Hur toouse applösenord
Här följer några saker tooknow om applösenord:

* Du skapa inte egna applösenord. De genereras automatiskt.
* Det finns för närvarande en gräns på 40 lösenord per användare. 
* Om du försöker toocreate ett applösenord när hello gränsen har nåtts, har du toodelete en av dina befintliga applösenord innan du skapar en ny.
* Använda ett applösenord per enhet, inte per program. Du kan till exempel skapa ett applösenord för din bärbara dator och använda det applösenordet för alla program på den bärbara datorn. Skapa sedan en andra app lösenord toouse för alla dina appar på ditt skrivbord. 
* Du får en app lösenord hello första gången du registrerar dig för tvåstegsverifiering.  Om du behöver ytterligare mallar kan skapa du dem.



## <a name="creating-and-deleting-app-passwords"></a>Skapa och ta bort applösenord
Vid första inloggningen får du ett applösenord som du kan använda.  Du kan också skapa och ta bort applösenord vid ett senare tillfälle. Hur du tar bort applösenord beror på hur du använder multi-Factor authentication. Svaret hello följande frågor toodetermine där du ska gå toomanage applösenord: 

1. Använder du tvåstegsverifiering för ditt personliga Microsoft-konto? Om Ja, bör du läsa toohello [applösenord och tvåstegsverifiering](https://support.microsoft.com/help/12409/microsoft-account-app-passwords-two-step-verification) artikeln för hjälp. Om Nej, Fortsätt tooquestion två.

2. OK så att du kan använda tvåstegsverifiering för ditt arbets- eller skolkonto konto. Du använder den toosign i tooOffice 365 appar? Om Ja, bör du läsa för[skapa ett applösenord för Office 365](https://support.office.com/article/Create-an-app-password-for-Office-365-3e7c860f-bda4-4441-a618-b53953ee1183) för hjälp. Om Nej, Fortsätt tooquestion tre. 

3. Använder du tvåstegsverifiering med Microsoft Azure? Om Ja, Fortsätt toohello [hantera applösenord i hello Azure-portalen](#manage-app-passwords-in-the-Azure-portal) i den här artikeln. Om Nej, Fortsätt tooquestion fyra.

4. Osäker på om du använder tvåstegsverifiering? Fortsätta toohello [hantera applösenord med hello MyApps portal](#manage-app-passwords-with-the-myapps-portal) i den här artikeln. 


## <a name="manage-app-passwords-in-hello-azure-portal"></a>Hantera lösenord i hello Azure-portalen
Om du använder tvåstegsverifiering med Azure kan du toocreate applösenord via hello Azure-portalen.

### <a name="toocreate-app-passwords-in-hello-azure-portal"></a>toocreate applösenord i hello Azure-portalen
1. Logga in toohello klassiska Azure-portalen.
2. Högerklicka på ditt användarnamn hello överst och välj ytterligare säkerhetskontroll.
3. Välj applösenord på hello proofup sidan hello överst
4. Klicka på **Skapa**.
5. Ange ett namn för hello lösenord och klicka på **nästa**
6. Kopiera hello app lösenord toohello Urklipp och klistrar in den i din app.
   
   ![Molnet](./media/multi-factor-authentication-end-user-app-passwords/app2.png)


### <a name="toodelete-app-passwords-in-hello-azure-portal"></a>toodelete applösenord i hello Azure-portalen
1. Logga in toohello klassiska Azure-portalen.
2. Högerklicka på ditt användarnamn hello överst och välj ytterligare säkerhetskontroll.
3. Överst hello, nästa tooadditional säkerhetskontroll Välj **applösenord.**
4. Nästa toohello applösenord som du vill toodelete, Välj **ta bort**.
5. Bekräfta borttagning av hello genom att klicka på **Ja**.
6. När hello applösenord har tagits bort, kan du klicka på **Stäng**.


## <a name="manage-app-passwords-with-hello-myapps-portal"></a>Hantera applösenord med hello MyApps portal.
Om du inte är säker på hur du använder multi-Factor authentication kan sedan du alltid skapa och ta bort applösenord hello myapps-portalen.

### <a name="toocreate-an-app-password-using-hello-myapps-portal"></a>en app lösenord med hjälp av toocreate hello Myapps portal
1. Logga in för[https://myapps.microsoft.com](https://myapps.microsoft.com)
2. Klicka på ditt namn på hello längst upp till höger och välj **profil**.
3. Välj **ytterligare säkerhetsverifiering**.
   ![Välj ytterligare säkerhetskontroll – skärmbild](./media/multi-factor-authentication-end-user-manage/myapps1.png)

4. Välj **applösenord**.
   ![Välj applösenord – skärmbild](./media/multi-factor-authentication-end-user-app-passwords/apppass2.png)

5. Klicka på **Skapa**.
6. Ange ett namn för hello lösenord och klicka på **nästa**.
7. Kopiera hello app lösenord toohello Urklipp och klistrar in den i din app.
   ![Skapa ett applösenord](./media/multi-factor-authentication-end-user-app-passwords/create2.png)

### <a name="toodelete-an-app-password-using-hello-myapps-portal"></a>en app lösenord med hjälp av toodelete hello Myapps portal
1. Logga in för[https://myapps.microsoft.com](https://myapps.microsoft.com)
2. Välj profil hello överst.
3. Välj **ytterligare säkerhetsverifiering**.

   ![Välj ytterligare säkerhetskontroll – skärmbild](./media/multi-factor-authentication-end-user-manage/myapps1.png)

4. Välj **applösenord**.

   ![Välj applösenord – skärmbild](./media/multi-factor-authentication-end-user-app-passwords/apppass2.png)

5. Nästa toohello applösenord som du vill toodelete, klickar du på **ta bort**.

   ![Ta bort ett applösenord](./media/multi-factor-authentication-end-user-app-passwords/delete1.png)

6. Bekräfta att du vill toodelete lösenordet genom att klicka på **Ja**.
7. När hello applösenord har tagits bort, kan du klicka på **Stäng**.

## <a name="next-steps"></a>Nästa steg

- [Hantera dina inställningar för tvåstegsverifiering](multi-factor-authentication-end-user-manage-settings.md)

- Testa hello [Microsoft Authenticator-appen](microsoft-authenticator-app-how-to.md) tooverify din inloggningar med app-meddelanden i stället för att ta emot texter eller samtal. 
