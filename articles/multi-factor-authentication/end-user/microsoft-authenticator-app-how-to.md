---
title: "aaaMicrosoft Authenticator-appen för mobila enheter | Microsoft Docs"
description: "Lär dig hur tooupgrade toohello senaste versionen av Azure Authenticator."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 3065a1ee-f253-41f0-a68d-2bd84af5ffba
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: kgremban
ms.reviewer: librown
ms.custom: H1Hack27Feb2017, end-user
ms.openlocfilehash: d895d92d89613cbafd9fc09d4ff4810cbf25652e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-microsoft-authenticator-app"></a>Kom igång med hello Microsoft Authenticator-appen
hello Microsoft Authenticator-appen ger en extra nivå av säkerhet i arbets-eller skolkonto (t.ex, bsimon@contoso.com) eller Microsoft-konto (till exempel bsimon@outlook.com).

hello appen fungerar på ett av två sätt:

* **Meddelande**. hello app kan du förhindra obehörig åtkomst tooaccounts och stoppa olagliga transaktioner genom att skicka en avisering tooyour smartphone eller surfplatta. Visa bara hello-meddelande och om det är tillförlitligt, Välj **Kontrollera**. Välj annars **neka**. 
* **Verifieringskoden**. hello app kan användas som en programuppdatering token toogenerate en OAuth Verifieringskod. När du har angett ditt användarnamn och lösenord anger hello koden som tillhandahålls av hello app till hello på inloggningsskärmen. hello Verifieringskod innehåller andra formen av autentisering.

hello Microsoft Authenticator-appen ersätter hello Azure Authenticator-appen. hello Azure Authenticator-appen fungerar fortfarande, men om du väljer toomove toohello nya Microsoft Authenticator-appen, den här artikeln kan hjälpa dig.  

## <a name="opt-in-for-two-step-verification"></a>Välja tvåstegsverifiering

hello Microsoft Authenticator-appen fungerar inte med sig själv. Konfigurera var och en av dina konton tooprompt du för en andra verifieringsmetod när du loggar in med ditt användarnamn och lösenord. 

För ett arbets- eller skolkonto konto får du inte vanligtvis toochoose funktionen själv. I stället måste en administratör väljer i den för din räkning och meddelar dig tooregister verifieringsmetoderna för ditt konto. Om det här scenariot gäller tooyou, Läs mer i [vad Azure Multi-Factor Authentication innebär för mig](multi-factor-authentication-end-user.md).

För ett personligt konto behöver du tooset tvåstegsverifiering själv. Om du har ett Microsoft-konto kan de här stegen finns i [om tvåstegsverifiering](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification). 

Du kan också använda hello Microsoft Authenticator med icke-Microsoft konton. De kan anropa hello funktionen något annat än tvåstegsverifiering, men du bör vara kan toofind det under inställningar för säkerhets- eller logga in. 

## <a name="install-hello-app"></a>Installera hello app
hello Microsoft Authenticator-appen är tillgänglig för [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), och [iOS](http://go.microsoft.com/fwlink/?Linkid=825073).

## <a name="add-accounts-toohello-app"></a>Lägg till konton toohello app
För varje konto som du vill tooadd toohello Microsoft Authenticator-appen, använder du något av följande procedurer hello:

### <a name="add-a-personal-microsoft-account-toohello-app"></a>Lägg till en personlig Microsoft-konto toohello app

För ett personligt microsoftkonto (en att du använder toosign i tooOutlook.com, Xbox, Skype, osv), du har toodo är att logga in tooyour konto i hello Microsoft Authenticator-appen.

### <a name="add-a-work-or-school-account-toohello-app-using-hello-qr-code-scanner"></a>Lägga till en arbets- eller skolkonto konto toohello app hello QR-kod skanner
1. Gå toohello skärmen inställningar för säkerhet verifiering.  Mer information om hur tooget toothis skärmen finns [ändra säkerhetsinställningarna](multi-factor-authentication-end-user-manage-settings.md#where-to-find-the-settings-page).
2. Kryssrutan hello bredvid för**autentiseringsapp** Välj **konfigurera**.

    ![hello Konfigureringsknappen hello skärmen inställningar för säkerhet verifiering](./media/authenticator-app-how-to/azureauthe.png)

    Nu visas en skärm med en QR-kod på den.

    ![Skärmen som ger hello QR-kod](./media/authenticator-app-how-to/barcode2.png)
3. Öppna hello Microsoft Authenticator-appen. På hello **konton** väljer  **+** , och sedan ange att du vill tooadd ett arbets- eller skolkonto konto.
4. Använda hello kamera tooscan hello QR-kod och välj sedan **klar** tooclose hello QR-kod skärmen.

    Om kameran inte fungerar kan du [ange hello QR-koden och Webbadressen manuellt](#add-an-account-to-the-app-manually).

5. När hello app visas namnet på ditt konto med en sexsiffrig kod under den är du klar. 

    ![Konton skärmen](./media/authenticator-app-how-to/accounts.png)

### <a name="add-an-account-toohello-app-manually"></a>Lägg till ett konto toohello app manuellt
1. Gå toohello skärmen inställningar för säkerhet verifiering.  Mer information om hur tooget toothis skärmen finns [ändra säkerhetsinställningarna](multi-factor-authentication-end-user-manage-settings.md).
2. Välj **konfigurera**.

    ![hello Konfigureringsknappen hello skärmen inställningar för säkerhet verifiering](./media/authenticator-app-how-to/azureauthe.png)

    Nu visas en skärm med en QR-kod på den.  Observera hello koden och Webbadressen.

    ![Skärmen som ger hello QR-koden och Webbadressen](./media/authenticator-app-how-to/barcode2.png)
3. Öppna hello Microsoft Authenticator-appen. På hello **konton** väljer  **+** , och sedan ange att du vill tooadd ett arbets- eller skolkonto konto.

4. Markera i hello skanner **ange koden manuellt**.

    ![Skärmen för att skanna QR-kod](./media/multi-factor-authentication-end-user-first-time/scan2.png)
5. Ange hello kod och hello URL i hello rutorna i hello app och välj sedan **Slutför**.

    ![Skärmen för att ange koden och Webbadressen](./media/authenticator-app-how-to/manual.png)

6. När hello app visas namnet på ditt konto med en sexsiffrig kod under den är du klar.

    ![Konton skärmen](./media/authenticator-app-how-to/accounts.png)

### <a name="add-an-account-toohello-app-using-touch-id"></a>Lägg till ett konto toohello app som använder Touch ID
hello Microsoft Authenticator-appen på iOS stöder Touch ID.  Azure Multi-Factor Authentication kan organisationer toorequire PIN-koden för enheter. Med Touch ID behöver iOS-användare tooenter en PIN-kod. De kan i stället söka igenom sitt fingeravtryck och välj **Godkänn**.

Det är enkelt att ställa in Touch ID med Microsoft Authenticator. Du har slutfört en normal verifiering utmaning med en PIN-kod. Om enheten har stöd för Touch ID, konfigurerar Microsoft Authenticator den automatiskt för det kontot.

![Verifiering av installationsprogrammet för Touch ID](./media/authenticator-app-how-to/touchid1.png)

Peka framåt, som när du är krävs tooverify din inloggning, du väljer hello emot push-meddelanden och söka igenom ditt fingeravtryck istället för att ange din PIN-kod.

![Push-meddelanden](./media/authenticator-app-how-to/touchid2.png)

## <a name="use-hello-app-when-you-sign-in"></a>Använda hello app när du loggar in

När ditt konto har lagts till toohello app, kanske ange toodo ett test verifiering toomake att allt är korrekt konfigurerad. När är du klar! Du behöver inte toodo något annat tills hello nästa gång du loggar in.

Om du väljer toouse verifieringskoder i hello app måste du starta toosee dem på hello startsidan. De kan ändra var 30: e sekund så att du alltid har en ny kod när du behöver en. Men du behöver inte toodo något med dem förrän du logga in och ange tooenter en Verifieringskod.  
