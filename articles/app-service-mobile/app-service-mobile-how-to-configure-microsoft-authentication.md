---
title: "aaaHow tooconfigure Account autentisering för tillämpningsprogrammet Apptjänster"
description: "Lär dig hur tooconfigure Account autentisering för din App Services-tjänstprogrammet."
author: mattchenderson
services: app-service
documentationcenter: 
manager: syntaxc4
editor: 
ms.assetid: ffbc6064-edf6-474d-971c-695598fd08bf
ms.service: app-service
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: d86d8dab26a189f4454082fc18e44e3fb6e0a01d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-your-app-service-application-toouse-microsoft-account-login"></a>Hur tooconfigure Apptjänst programmet toouse Account inloggningen
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Det här avsnittet beskrivs hur du tooconfigure Azure App Service toouse Account som en autentiseringsprovider. 

## <a name="register-microsoft-account"></a>Registrera din app med Microsoft-konto
1. Logga in toohello [Azure-portalen], och navigera tooyour program. Kopiera ditt **URL**, vilket du använda senare tooconfigure din app med Account.
2. Navigera toohello [Mina program] i hello Microsoft Account Developer Center och logga in med ditt Microsoft-konto om det behövs.
3. Klicka på **Lägg till en app**, ange ett programnamn och på **skapa program**.
4. Anteckna hello **program-ID**, som du behöver senare. 
5. Klicka på under ”plattformar”, **lägga till plattformen** och välj ”Web”.
6. Under ”omdirigerings-URI: er” ange hello slutpunkten för ditt program, sedan klickar du på **spara**. 
   
   > [!NOTE]
   > Din omdirigering URI är hello URL för ditt program läggas till med hello sökvägen */.auth/login/microsoftaccount/callback*. Till exempel `https://contoso.azurewebsites.net/.auth/login/microsoftaccount/callback`.   
   > Kontrollera att du använder hello HTTPS-schema.
   
7. Klicka på under ”programmet hemligheter”, **generera nya lösenord**. Anteckna hello-värdet som visas. När du lämnar sidan hello, kommer den inte att visas igen.

    > [!IMPORTANT]
    > hello lösenordet är en viktig säkerhetsuppgift för autentisering. Dela hello lösenord med någon eller inte distribuera inom ett klientprogram.

## <a name="secrets"></a>Lägg till Microsoft-konto information tooyour App-tjänstprogram
1. Tillbaka i hello [Azure-portalen]navigerar tooyour programmet, klickar du på **inställningar** > **autentisering / auktorisering**.
2. Om hello autentisering / auktorisering är inte aktiverad, växlar den **på**.
3. Klicka på **Microsoft-konto**. Klistra in i hello program-ID och lösenord värden som du hämtade tidigare och välja att aktivera alla scope som krävs för ditt program. Klicka sedan på **OK**.
   
    ![][1]
   
    Som standard Apptjänst ger autentisering, men begränsar inte auktoriserad åtkomst tooyour plats innehåll och API: er. Du måste auktorisera användare i din Appkod.
4. (Valfritt) toorestrict-tooyour plats tooonly användare autentiseras av Microsoft-konto, ange **åtgärd tootake när begäran inte har autentiserats** för**Account**. Detta kräver att alla förfrågningar autentiseras och alla oautentiserade begäranden omdirigeras tooMicrosoft kontot för autentisering.
5. Klicka på **Spara**.

Nu är du redo toouse Account för autentisering i appen.

## <a name="related-content"></a>Relaterat innehåll
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/app-service-microsoftaccount-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/mobile-app-microsoftaccount-settings.png

<!-- URLs. -->

[Mina program]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Azure-portalen]: https://portal.azure.com/
