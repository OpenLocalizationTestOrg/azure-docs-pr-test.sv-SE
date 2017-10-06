---
title: "aaaHow tooconfigure Facebook-autentisering för tillämpningsprogrammet Apptjänster"
description: "Lär dig hur tooconfigure Facebook-autentisering för din App Services-tjänstprogrammet."
services: app-service
documentationcenter: 
author: mattchenderson
manager: syntaxc4
editor: 
ms.assetid: b6b4f062-fcb4-47b3-b75a-ec4cb51a62fd
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: 53d03445a2ad17de1d2f69f5e770d14385b48ad4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-your-app-service-application-toouse-facebook-login"></a>Hur tooconfigure din Apptjänst programmet toouse Facebook-inloggning
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Det här avsnittet beskrivs hur du tooconfigure Azure App Service toouse Facebook som en autentiseringsprovider.

Du måste ha en Facebook-konto som har en verifierad e-postadress och ett mobiltelefonnummer toocomplete hello proceduren i det här avsnittet. Gå toocreate ett nytt Facebook-konto för[facebook.com].

## <a name="register"></a>Registrera programmet med Facebook
1. Logga in toohello [Azure-portalen], och navigera tooyour program. Kopiera ditt **URL**. Du använder den här tooconfigure Facebook-app.
2. Navigera i ett nytt webbläsarfönster toohello [Facebook utvecklare] webbplatsen och logga in med dina Facebook kontoautentiseringsuppgifter.
3. (Valfritt) Om du inte redan har registrerat, klickar du på **appar** > **registrera som en utvecklare**, acceptera hello princip och gör hello-registrering.
4. Klicka på **Mina appar** > **lägga till en ny App** > **webbplats** > **hoppa över och skapa App-ID**. 
5. I **visningsnamn**, ange ett unikt namn för din app typen din **kontakta e-post**, Välj en **kategori** för din app, klicka sedan på **skapa App-ID**och slutföra hello säkerhetskontrollen. Då kommer du instrumentpanelen för utvecklare av toohello för din nya Facebook-app.
6. Klicka på under ”Facebook inloggning”, **Kom igång**. Lägg till programmets **omdirigerings-URI** för**omdirigerings-URI: er för giltig OAuth**, klicka på **spara ändringar**. 
   
   > [!NOTE]
   > Din omdirigering URI är hello URL för ditt program läggas till med hello sökvägen */.auth/login/facebook/callback*. Till exempel `https://contoso.azurewebsites.net/.auth/login/facebook/callback`. Kontrollera att du använder hello HTTPS-schema.
   > 
   > 
7. I hello vänstra navigeringsfönstret klickar du på **inställningar**. På hello **App hemlighet** klickar **visa**, ange ditt lösenord om det begärs och anteckna hello värdena för **App-ID** och **App hemlighet**. Använd dessa senare tooconfigure ditt program i Azure.
   
   > [!IMPORTANT]
   > hello app hemlighet är en viktig säkerhetsuppgift för autentisering. Dela den här hemligheten med någon eller inte distribuera inom ett klientprogram.
   > 
   > 
8. hello Facebook-konto som har använt tooregister hello program är administratör för hello app. Endast administratörer kan nu logga in på det här programmet. tooauthenticate andra Facebook-konton klickar du på **App granska** och aktivera **Se < appens-namn > offentliga** tooenable allmän allmän åtkomst med hjälp av Facebook-autentisering.

## <a name="secrets"></a>Lägg till Facebook information tooyour program
1. Tillbaka i hello [Azure-portalen], navigera tooyour program. Klicka på **inställningar** > **autentisering / auktorisering**, och se till att **App autentiseringen av tjänsten** är **på**.
2. Klicka på **Facebook**, klistra in i hello App-ID och App hemlighet värden som du hämtade tidigare, om du vill aktivera alla scope som behövs i programmet och sedan klickar du på **OK**.
   
    ![][0]
   
    Som standard Apptjänst ger autentisering, men begränsar inte auktoriserad åtkomst tooyour plats innehåll och API: er. Du måste auktorisera användare i din Appkod.
3. (Valfritt) toorestrict-tooyour plats tooonly användare autentiseras av Facebook, ange **åtgärd tootake när begäran inte har autentiserats** för**Facebook**. Detta kräver att alla förfrågningar autentiseras, och alla begäranden som inte har autentiserats är omdirigerade tooFacebook för autentisering.
4. När konfigurera autentisering klickar du på **spara**.

Nu är du redo toouse Facebook för autentisering i appen.

## <a name="related-content"></a>Relaterat innehåll
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->
[0]: ./media/app-service-mobile-how-to-configure-facebook-authentication/mobile-app-facebook-settings.png

<!-- URLs. -->
[Facebook utvecklare]: http://go.microsoft.com/fwlink/p/?LinkId=268286
[facebook.com]: http://go.microsoft.com/fwlink/p/?LinkId=268285
[Get started with authentication]: /en-us/develop/mobile/tutorials/get-started-with-users-dotnet/
[Azure-portalen]: https://portal.azure.com/
