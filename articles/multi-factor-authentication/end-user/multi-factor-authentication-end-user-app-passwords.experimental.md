---
title: "Hur du använder Applösenord i Azure MFA? | Microsoft Docs"
description: "Den här sidan hjälper användarna att förstå vad applösenord är och vad de används med hänsyn till Azure MFA."
services: multi-factor-authentication
documentationcenter: 
author: barlanmsft
manager: mtillman
ms.reviewer: richagi
ms.assetid: 345b757b-5a2b-48eb-953f-d363313be9e5
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2018
ms.author: barlan
ms.custom: end-user
ms.openlocfilehash: 0812719ddee8c0ff0c2fa9256c2819611692dfe5
ms.sourcegitcommit: 7d4b3cf1fc9883c945a63270d3af1f86e3bfb22a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/08/2018
---
# <a name="what-are-app-passwords-in-azure-multi-factor-authentication"></a>Vad är Applösenord i Azure Multi-Factor Authentication?
Vissa icke-webbläsarappar, till exempel Apple interna e-klienten som använder Exchange Active Sync stöder för tillfället inte multifaktorautentisering. Multifaktorautentisering aktiveras per användare.  Detta innebär att en användare inte kan använda multifaktorautentisering om:

- Användaren har aktiverats för multifaktorautentisering
- Användaren försöker använda en icke-Webbläsarprogrammet.

Ett applösenord gör att användaren kan använda appen.

När du har ett applösenord kan använda du den i stället för det ursprungliga lösenordet med dessa icke-webbläsarbaserade appar. När du registrerar dig för tvåstegsverifiering du talar om Microsoft inte att vem som helst logga in med ditt lösenord om de inte kan också utföra andra verifieringen. Apple interna e-postklienten på telefonen logga inte in dig eftersom den inte kan fråga efter tvåstegsverifiering. Lösning på problemet är att skapa en säkrare applösenord som du inte använder dagliga. Applösenord gäller endast de appar som inte stöder tvåstegsverifiering. Använd applösenordet så att appar kan hoppa över multifaktorautentisering och fortsätta att fungera.


> [!NOTE]
> Office 2013-klienter (inklusive Outlook) stöder nya autentiseringsprotokoll och kan användas med tvåstegsverifiering. Applösenord krävs inte för användning med Office 2013-klienter.  Mer information finns i [Office 2013 modern autentisering offentlig förhandsgranskning av](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).


## <a name="how-to-use-app-passwords"></a>Använda applösenord
Här följer några saker du behöver veta om applösenord:

* Du skapa inte egna applösenord. De genereras automatiskt.
* Det finns för närvarande en gräns på 40 lösenord per användare.
* Om du försöker skapa ett applösenord när gränsen har uppnåtts, har du ta bort en av dina befintliga applösenord innan du skapar en ny.
* Använda ett applösenord per enhet, inte per program. Du kan till exempel skapa ett applösenord för din bärbara dator och använda det applösenordet för alla program på den bärbara datorn. Skapa sedan en andra lösenord ska användas för alla dina appar på ditt skrivbord.
* Du får ett applösenord första gången du registrerar dig för tvåstegsverifiering.  Om du behöver ytterligare mallar kan skapa du dem.



## <a name="creating-and-deleting-app-passwords"></a>Skapa och ta bort applösenord
Vid första inloggningen får du ett applösenord som du kan använda.  Du kan också skapa och ta bort applösenord vid ett senare tillfälle. Hur du tar bort applösenord beror på hur du använder multi-Factor authentication. Besvara följande frågor för att avgöra var du ska gå för att hantera applösenord:

1. Använder du tvåstegsverifiering för ditt personliga Microsoft-konto? Om Ja, du måste referera till den [applösenord och tvåstegsverifiering](https://support.microsoft.com/help/12409/microsoft-account-app-passwords-two-step-verification) artikeln för hjälp. Om Nej, fortsätta att fråga två.

2. OK så att du kan använda tvåstegsverifiering för ditt arbets- eller skolkonto konto. Du använder den för att logga in på Office 365-appar? Om Ja, bör du gå till [skapa ett applösenord för Office 365](https://support.office.com/article/Create-an-app-password-for-Office-365-3e7c860f-bda4-4441-a618-b53953ee1183) för hjälp. Om Nej, Fortsätt till fråga tre.

3. Använder du tvåstegsverifiering med Microsoft Azure? Om Ja, fortsätter du till den [hantera lösenord i Azure portal](#manage-app-passwords-in-the-Azure-portal) i den här artikeln. Om Nej, fortsätta att fråga fyra.

4. Osäker på om du använder tvåstegsverifiering? Fortsätta att den [hantera applösenord med portalen MyApps](#manage-app-passwords-with-the-myapps-portal) i den här artikeln.


## <a name="manage-app-passwords-in-the-azure-portal"></a>Hantera lösenord i Azure-portalen
Om du använder tvåstegsverifiering med Azure som du vill skapa applösenord via Azure-portalen.

### <a name="to-create-app-passwords-in-the-azure-portal"></a>Skapa applösenord i Azure-portalen
1. Logga in på Azure Portal.
2. Klicka på ditt användarnamn längst upp och markera **ändra lösenord**.
3. På sidan proofup överst och välj **applösenord**.
4. Välj **Skapa**.
5. Ange ett namn för applösenordet och välj **nästa**.
6. Kopiera applösenordet till Urklipp och klistrar in den i din app.

   ![Molnet](./media/multi-factor-authentication-end-user-app-passwords/app2.png)


## <a name="manage-app-passwords-with-the-myapps-portal"></a>Hantera applösenord med MyApps-portalen.
Om du inte är säker på hur du använder multi-Factor authentication kan sedan du alltid skapa och ta bort applösenord myapps-portalen.

### <a name="to-create-an-app-password-using-the-myapps-portal"></a>Skapa ett applösenord med hjälp av MyApps-portalen
1. Logga in på [https://myapps.microsoft.com](https://myapps.microsoft.com)
2. Klicka på ditt namn längst upp till höger och välj **profil**.
3. Välj **ytterligare säkerhetsverifiering**.
   ![Välj ytterligare säkerhetskontroll – skärmbild](./media/multi-factor-authentication-end-user-manage/myapps1.png)

4. Välj **applösenord**.
   ![Välj applösenord – skärmbild](./media/multi-factor-authentication-end-user-app-passwords/apppass2.png)

5. Klicka på **Skapa**.
6. Ange ett namn för applösenordet och klicka på **nästa**.
7. Kopiera applösenordet till Urklipp och klistrar in den i din app.
   ![Skapa ett applösenord](./media/multi-factor-authentication-end-user-app-passwords/create2.png)

### <a name="to-delete-an-app-password-using-the-myapps-portal"></a>Ta bort ett applösenord med hjälp av MyApps-portalen
1. Logga in på [https://myapps.microsoft.com](https://myapps.microsoft.com)
2. Välj profil överst.
3. Välj **ytterligare säkerhetsverifiering**.

   ![Välj ytterligare säkerhetskontroll – skärmbild](./media/multi-factor-authentication-end-user-manage/myapps1.png)

4. Välj **applösenord**.

   ![Välj applösenord – skärmbild](./media/multi-factor-authentication-end-user-app-passwords/apppass2.png)

5. Bredvid applösenord som du vill ta bort, klickar du på **ta bort**.

   ![Ta bort ett applösenord](./media/multi-factor-authentication-end-user-app-passwords/delete1.png)

6. Bekräfta att du vill ta bort lösenordet genom att klicka på **Ja**.
7. När applösenordet har tagits bort, kan du klicka på **Stäng**.

## <a name="next-steps"></a>Nästa steg

- [Hantera dina inställningar för tvåstegsverifiering](multi-factor-authentication-end-user-manage-settings.md)

- Testa den [Microsoft Authenticator-appen](microsoft-authenticator-app-how-to.md) att verifiera din inloggningar med app-meddelanden i stället för att ta emot texter eller samtal.
