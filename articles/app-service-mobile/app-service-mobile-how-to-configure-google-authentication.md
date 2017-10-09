---
title: "aaaHow tooconfigure Google-autentisering för tillämpningsprogrammet Apptjänster"
description: "Lär dig hur tooconfigure Google-autentisering för din App Services-tjänstprogrammet."
services: app-service
documentationcenter: 
author: mattchenderson
manager: syntaxc4
editor: 
ms.assetid: 2b2f9abf-9120-4aac-ac5b-4a268d9b6e2b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: 9175c40b78c859e9e191504c41cd0bb9a3380ccd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-your-app-service-application-toouse-google-login"></a>Hur tooconfigure din Apptjänst programmet toouse Google-inloggning
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Det här avsnittet beskrivs hur du tooconfigure Azure App Service toouse Google som en autentiseringsprovider.

Du måste ha ett Google-konto som har en verifierad e-postadress toocomplete hello proceduren i det här avsnittet. Gå toocreate ett nytt Google-konto för[accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).

## <a name="register"></a>Registrera programmet med Google
1. Logga in toohello [Azure-portalen], och navigera tooyour program. Kopiera ditt **URL**, som du använder senare tooconfigure Google-app.
2. Navigera toohello [Google API: er](http://go.microsoft.com/fwlink/p/?LinkId=268303) logga in med autentiseringsuppgifterna för ditt Google-konto, klickar du på **skapa projekt**, ange en **projektnamn**, klicka på  **Skapa**.
3. Under **sociala API: er** klickar du på **Google + API** och sedan **aktivera**.
4. I vänster navigering hello **autentiseringsuppgifter** > **OAuth-medgivande skärmen**och välj din **e-postadress**, ange en **produktnamn**, och klicka på **spara**.
5. I hello **autentiseringsuppgifter** klickar du på **skapa autentiseringsuppgifter** > **OAuth-klient-ID**och välj **webbprogrammet**.
6. Klistra in hello Apptjänst **URL** du kopierade tidigare i **behörighet JavaScript ursprung**, klistra sedan in din omdirigering URI i **behörighet omdirigerings-URI**. Hej omdirigerings-URI: N är hello URL för ditt program läggas till med hello sökvägen */.auth/login/google/callback*. Till exempel `https://contoso.azurewebsites.net/.auth/login/google/callback`. Kontrollera att du använder hello HTTPS-schema. Klicka sedan på **Skapa**.
7. På nästa skärm hello anteckna hello värdena för hello klient-ID och klienten hemlighet.

    > [!IMPORTANT]
    > Hej klienthemlighet är en viktig säkerhetsuppgift för autentisering. Dela den här hemligheten med någon eller inte distribuera inom ett klientprogram.


## <a name="secrets"></a>Lägga till Google information tooyour program
1. Tillbaka i hello [Azure-portalen], navigera tooyour program. Klicka på **inställningar**, och sedan **autentisering / auktorisering**.
2. Om hello autentisering / auktoriseringsfunktionen är inte aktiverad, aktivera hello växeln för**på**.
3. Klicka på **Google**. Klistra in i hello App-ID och App hemlighet värden som du hämtade tidigare och välja att aktivera alla scope som krävs för ditt program. Klicka sedan på **OK**.
   
   ![][1]
   
   Som standard Apptjänst ger autentisering, men begränsar inte auktoriserad åtkomst tooyour plats innehåll och API: er. Du måste auktorisera användare i din Appkod.
4. (Valfritt) toorestrict-tooyour plats tooonly användare autentiseras av Google, ange **åtgärd tootake när begäran inte har autentiserats** för**Google**. Detta kräver att alla förfrågningar autentiseras, och alla begäranden som inte har autentiserats är omdirigerade tooGoogle för autentisering.
5. Klicka på **Spara**.

Nu är du redo toouse Google för autentisering i appen.

## <a name="related-content"></a>Relaterat innehåll
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Anchors. -->

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-settings.png

<!-- URLs. -->

[Google apis]: http://go.microsoft.com/fwlink/p/?LinkId=268303

[Azure-portalen]: https://portal.azure.com/

