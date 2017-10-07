---
title: "aaaHow tooconfigure Twitter-autentisering för tillämpningsprogrammet Apptjänster"
description: "Lär dig hur tooconfigure Twitter-autentisering för din App Services-tjänstprogrammet."
services: app-service
documentationcenter: 
author: mattchenderson
manager: syntaxc4
editor: 
ms.assetid: c6dc91d7-30f6-448c-9f2d-8e91104cde73
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: 0d16ac44d7b54e3567b793a904059d31ab1d8911
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-your-app-service-application-toouse-twitter-login"></a>Hur tooconfigure din Apptjänst programmet toouse Twitter-inloggning
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Det här avsnittet beskrivs hur du tooconfigure Azure App Service toouse Twitter som en autentiseringsprovider.

Du måste ha ett Twitter-konto som har ett verifierad e-postadressen och telefonnumret toocomplete hello proceduren i det här avsnittet. Gå toocreate ett nytt Twitter-konto för<a href="http://go.microsoft.com/fwlink/p/?LinkID=268287" target="_blank">twitter.com</a>.

## <a name="register"></a>Registrera programmet med Twitter
1. Logga in toohello [Azure-portalen], och navigera tooyour program. Kopiera ditt **URL**. Du använder den här tooconfigure Twitter-app.
2. Navigera toohello [Twitter utvecklare] webbplats, logga in med dina autentiseringsuppgifter för Twitter-konto och klicka på **Skapa ny App**.
3. Typen i hello **namn** och en **beskrivning** för din nya app. Klistra in i ditt program **URL** för hello **webbplats** värde. Sedan för hello **motringning URL**, klistra in hello **motringning URL** du kopierade tidigare. Här hittar du Mobilapp läggas till med hello sökvägen */.auth/login/twitter/callback*. Till exempel `https://contoso.azurewebsites.net/.auth/login/twitter/callback`. Kontrollera att du använder hello HTTPS-schema.
4. Läs och acceptera hello hello nedre hello sida. Klicka på **skapa programmet Twitter**. Den här registren hello appen visar hello programinformation.
5. Klicka på hello **inställningar** markerar **Tillåt det här programmet används toobe toosign in med Twitter**, klicka på **uppdateringsinställningar**.
6. Välj hello **nycklar och åtkomst-token** fliken. Anteckna hello värdena för **konsumentnyckel (API-nyckel)** och **konsumenten hemligheten (API hemliga)**.
   
   > [!NOTE]
   > hello konsumenthemlighet är en viktig säkerhetsuppgift för autentisering. Dela den här hemligheten med någon eller inte distribuera med din app.
   > 
   > 

## <a name="secrets"></a>Lägg till Twitter information tooyour program
1. Tillbaka i hello [Azure-portalen], navigera tooyour program. Klicka på **inställningar**, och sedan **autentisering / auktorisering**.
2. Om hello autentisering / auktoriseringsfunktionen är inte aktiverad, aktivera hello växeln för**på**.
3. Klicka på **Twitter**. Klistra in i hello App-ID och App hemlighet värden som du hämtade tidigare. Klicka sedan på **OK**.
   
   ![][1]
   
   Som standard Apptjänst ger autentisering, men begränsar inte auktoriserad åtkomst tooyour plats innehåll och API: er. Du måste auktorisera användare i din Appkod.
4. (Valfritt) toorestrict-tooyour plats tooonly användare autentiseras av Twitter, ange **åtgärd tootake när begäran inte har autentiserats** för**Twitter**. Detta kräver att alla förfrågningar autentiseras, och alla begäranden som inte har autentiserats är omdirigerade tooTwitter för autentisering.
5. Klicka på **Spara**.

Nu är du redo toouse Twitter för autentisering i appen.

## <a name="related-content"></a>Relaterat innehåll
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-twitter-authentication/app-service-twitter-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-twitter-authentication/mobile-app-twitter-settings.png

<!-- URLs. -->

[Twitter utvecklare]: http://go.microsoft.com/fwlink/p/?LinkId=268300
[Azure-portalen]: https://portal.azure.com/
[xamarin]: ../app-services-mobile-app-xamarin-ios-get-started-users.md
