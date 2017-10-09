---
title: "aaaHow tooconfigure Azure Active Directory-autentisering för tillämpningsprogrammet Apptjänster"
description: "Lär dig hur tooconfigure Azure Active Directory-autentisering för ditt program med App-tjänster."
author: mattchenderson
services: app-service
documentationcenter: 
manager: syntaxc4
editor: 
ms.assetid: 6ec6a46c-bce4-47aa-b8a3-e133baef22eb
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: 5b1d73f8fdf5831a266d900cd07f4e3b0917a7f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-your-app-service-application-toouse-azure-active-directory-login"></a>Hur tooconfigure din Apptjänst programmet toouse Azure Active Directory-inloggning
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Det här avsnittet beskrivs hur du tooconfigure Azure App Service toouse Azure Active Directory som en autentiseringsprovider.

## <a name="express"></a>Konfigurera Azure Active Directory med standardinställningar
1. I hello [Azure-portalen], navigera tooyour program. Klicka på **inställningar**, och sedan **autentisering/auktorisering**.
2. Om hello autentisering / auktoriseringsfunktionen är inte aktiverad, aktivera hello växeln för**på**.
3. Klicka på **Azure Active Directory**, och klicka sedan på **Express** under **hanteringsläge**.
4. Klicka på **OK** tooregister hello program i Azure Active Directory. Detta skapar en ny registrering. Om du vill toochoose en befintlig registrering i stället **väljer en befintlig app** och sök sedan efter hello namnet på en tidigare skapad registrering i din klient.
   Klicka på hello registrering tooselect den och på **OK**. Klicka på **OK** på inställningsbladet för hello Azure Active Directory.
   
   ![][0]
   
   Som standard Apptjänst ger autentisering, men begränsar inte auktoriserad åtkomst tooyour plats innehåll och API: er. Du måste auktorisera användare i din Appkod.
5. (Valfritt) toorestrict-tooyour plats tooonly användare autentiseras av Azure Active Directory, ange **åtgärd tootake när begäran inte har autentiserats** för**logga in med Azure Active Directory**. Detta kräver att alla förfrågningar autentiseras, och alla begäranden som inte har autentiserats är omdirigerade tooAzure Active Directory för autentisering.
6. Klicka på **Spara**.

Nu är du redo toouse Azure Active Directory för autentisering i appen.

## <a name="advanced"></a>(Alternativ metod) manuellt konfigurera Azure Active Directory med avancerade inställningar
Du kan också välja tooprovide konfigurationsinställningar manuellt. Detta är hello önskad lösning om hello AAD-klient som du vill toouse skiljer sig från hello-klient som du loggar in i Azure. toocomplete hello konfigurationen måste du först skapa en registrering i Azure Active Directory och du måste ange några hello registrering information tooApp Service.

### <a name="register"></a>Registrera ditt program med Azure Active Directory
1. Logga in toohello [Azure-portalen], och navigera tooyour program. Kopiera ditt **URL**. Du använder den här tooconfigure appen Azure Active Directory.
2. Logga in toohello [klassiska Azure-portalen] och navigera för**Active Directory**.
   
    ![][2]
3. Välj din katalog och välj sedan hello **program** fliken hello överst. Klicka på **lägga till** på hello nedre toocreate en ny appregistrering.
4. Klicka på **Lägg till ett program som min organisation utvecklar**.
5. I guiden Lägg till programmet hello, ange en **namn** för ditt program och klicka på hello **Web Application och/eller webb-API** typen. Klicka på toocontinue.
6. I hello **SIGN-ON-URL** klistra in hello programmets URL som du kopierade tidigare. Ange samma URL: en i hello **App-ID URI** rutan. Klicka på toocontinue.
7. När programmet hello har lagts till, klickar du på hello **konfigurera** fliken. Redigera hello **Reply URL** under **enkel inloggning** toobe hello URL för ditt program läggas till med hello sökvägen */.auth/login/aad/callback*. Till exempel `https://contoso.azurewebsites.net/.auth/login/aad/callback`. Kontrollera att du använder hello HTTPS-schema.
   
    ![][3]
8. Klicka på **Spara**. Och sedan kopiera hello **klient-ID** för hello app. Du vill konfigurera ditt program toouse detta senare.
9. I hello kommandofältet längst ned, klickar du på **visa slutpunkter**, och sedan kopiera hello **Federation Metadata dokumentet** URL och hämta som dokument eller navigera tooit i en webbläsare.
10. Inom hello rot **EntityDescriptor** element, bör det finnas en **ID för entiteterna** attributet hello formatet `https://sts.windows.net/` följt av en klient-GUID specifika tooyour (kallas en ”klient-ID”). Kopiera det här värdet – det ska fungera som din **utfärdar-URL**. Du vill konfigurera ditt program toouse detta senare.

### <a name="secrets"></a>Lägg till Azure Active Directory information tooyour program
1. Tillbaka i hello [Azure-portalen], navigera tooyour program. Klicka på **inställningar**, och sedan **autentisering/auktorisering**.
2. Om hello autentisering/auktorisering i funktionen inte är aktiverad, aktivera hello växeln för**på**.
3. Klicka på **Azure Active Directory**, och klicka sedan på **Avancerat** under **hanteringsläge**. Klistra in hello klient-ID och utfärdar-URL-värde som du fick tidigare. Klicka sedan på **OK**.
   
   ![][1]
   
   Som standard Apptjänst ger autentisering, men begränsar inte auktoriserad åtkomst tooyour plats innehåll och API: er. Du måste auktorisera användare i din Appkod.
4. (Valfritt) toorestrict-tooyour plats tooonly användare autentiseras av Azure Active Directory, ange **åtgärd tootake när begäran inte har autentiserats** för**logga in med Azure Active Directory**. Detta kräver att alla förfrågningar autentiseras, och alla begäranden som inte har autentiserats är omdirigerade tooAzure Active Directory för autentisering.
5. Klicka på **Spara**.

Nu är du redo toouse Azure Active Directory för autentisering i appen.

## <a name="optional-configure-a-native-client-application"></a>(Valfritt) Konfigurera en native client-program
Azure Active Directory kan du också tooregister interna klienter, vilket ger dig större kontroll över behörigheter mappning. Du behöver den här om du inte vill tooperform inloggningar som använder ett bibliotek som hello **Active Directory Authentication Library**.

1. Navigera för**Active Directory** i hello [klassiska Azure-portalen].
2. Välj din katalog och välj sedan hello **program** fliken hello överst. Klicka på **lägga till** på hello nedre toocreate en ny appregistrering.
3. Klicka på **Lägg till ett program som min organisation utvecklar**.
4. I guiden Lägg till programmet hello, ange en **namn** för ditt program och klicka på hello **internt klientprogram** typen. Klicka på toocontinue.
5. I hello **omdirigerings-URI** Ange webbplatsens */.auth/login/done* slutpunkten, med hjälp av hello HTTPS-schema. Det här värdet bör vara densamma för*https://contoso.azurewebsites.net/.auth/login/done*. Om du skapar ett Windows-program, i stället använda hello [paket-SID](app-service-mobile-dotnet-how-to-use-client-library.md#package-sid) som hello URI.
6. När hello det ursprungliga programmet har lagts till, klickar du på hello **konfigurera** fliken. Hitta hello **klient-ID** och anteckna värdet.
7. Rulla hello PGDN toohello **behörigheter tooother program** avsnittet och klicka på **lägga till program**.
8. Söka efter hello webbprogram som du tidigare har registrerat och klicka på hello plus ikon. Klicka på hello Kontrollera tooclose hello dialogrutan. Om hello webb-applikationen inte kan hitta, navigera tooits registrering och lägga till en ny reply URL (t.ex. hello http-versionen av din aktuella URL), klicka på Spara och sedan upprepa stegen - hello programmet ska visas i listan hello.
9. Öppna hello på hello ny post du just lagt till, **delegerade behörigheter** listrutan och välj **åtkomst (appName)**. Klicka sedan på **Spara**.

Du har nu konfigurerat native client-program som har åtkomst till din App-tjänstprogrammet.

## <a name="related-content"></a>Relaterat innehåll
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/mobile-app-aad-express-settings.png
[1]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/mobile-app-aad-advanced-settings.png
[2]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-navigate-aad.png
[3]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-configure.png

<!-- URLs. -->

[Azure-portalen]: https://portal.azure.com/
[klassiska Azure-portalen]: https://manage.windowsazure.com/
[alternative method]:#advanced
