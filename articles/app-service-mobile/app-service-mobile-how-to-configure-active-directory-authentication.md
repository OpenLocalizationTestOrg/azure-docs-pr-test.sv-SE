---
title: "Hur du konfigurerar Azure Active Directory-autentisering för tillämpningsprogrammet Apptjänster"
description: "Lär dig hur du konfigurerar Azure Active Directory-autentisering för ditt program med App-tjänster."
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
ms.openlocfilehash: fe007fa8640d5641f29dc88f8f3a8ca52a1ca8ae
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-configure-your-app-service-application-to-use-azure-active-directory-login"></a>Så här konfigurerar du App Service-programmet för att använda Azure Active Directory-inloggning
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Det här avsnittet visar hur du konfigurerar Azure App Services att använda Azure Active Directory som en autentiseringsprovider.

## <a name="express"></a>Konfigurera Azure Active Directory med standardinställningar
1. I den [Azure-portalen], navigera till programmet. Klicka på **inställningar**, och sedan **autentisering/auktorisering**.
2. Om autentisering / auktorisering är inte aktiverad, aktivera växeln **på**.
3. Klicka på **Azure Active Directory**, och klicka sedan på **Express** under **hanteringsläge**.
4. Klicka på **OK** registrera programmet i Azure Active Directory. Detta skapar en ny registrering. Om du vill välja en befintlig registrering i stället, klickar du på **väljer en befintlig app** och sök sedan efter namnet på en tidigare skapad registrering i din klient.
   Klicka på registrering för att markera den och klicka på **OK**. Klicka på **OK** på inställningsbladet för Azure Active Directory.
   
   ![][0]
   
   Som standard Apptjänst ger autentisering, men begränsar inte auktoriserad åtkomst till webbplatsens innehåll och API: er. Du måste auktorisera användare i din Appkod.
5. (Valfritt) Om du vill begränsa åtkomsten till din webbplats till enbart användare som autentiseras av Azure Active Directory, ange **åtgärd att vidta när begäran inte har autentiserats** till **logga in med Azure Active Directory**. Detta kräver att alla förfrågningar autentiseras och alla oautentiserade begäranden omdirigeras till Azure Active Directory för autentisering.
6. Klicka på **Spara**.

Du är nu redo att använda Azure Active Directory för autentisering i appen.

## <a name="advanced"></a>(Alternativ metod) manuellt konfigurera Azure Active Directory med avancerade inställningar
Du kan också välja att ange konfigurationsinställningar manuellt. Detta är den bästa lösningen om AAD-klient som du vill använda skiljer sig från den klient som du loggar in i Azure. Om du vill slutföra konfigurationen måste du först skapa en registrering i Azure Active Directory och du måste ange några av registreringsinformation till App Service.

### <a name="register"></a>Registrera ditt program med Azure Active Directory
1. Logga in på den [Azure-portalen], och navigera till programmet. Kopiera ditt **URL**. Du använder detta för att konfigurera din app i Azure Active Directory.
2. Logga in på den [klassiska Azure-portalen] och gå till **Active Directory**.
   
    ![][2]
3. Välj din katalog och välj sedan den **program** högst upp. Klicka på **lägga till** längst ned för att skapa en ny appregistrering.
4. Klicka på **Lägg till ett program som min organisation utvecklar**.
5. I guiden Lägg till program och ange en **namn** för ditt program och klicka på den **Web Application och/eller webb-API** typen. Klicka om du vill fortsätta.
6. I den **SIGN-ON-URL** klistra in URL: en för programmet som du kopierade tidigare. Anger att samma URL i den **App-ID URI** rutan. Klicka om du vill fortsätta.
7. När programmet har lagts till, klickar du på den **konfigurera** fliken. Redigera den **Reply URL** under **enkel inloggning** till URL: en för programmet läggas till med sökvägen */.auth/login/aad/callback*. Till exempel `https://contoso.azurewebsites.net/.auth/login/aad/callback`. Kontrollera att du använder HTTPS-schema.
   
    ![][3]
8. Klicka på **Spara**. Kopiera den **klient-ID** för appen. Du ska konfigurera programmet så att det senare.
9. Klicka i kommandofältet längst ned **visa slutpunkter**, och sedan kopiera den **Federation Metadata dokumentet** URL och hämta som dokument eller navigera till den i en webbläsare.
10. I roten **EntityDescriptor** element, bör det finnas en **ID för entiteterna** attribut med formatet `https://sts.windows.net/` följt av ett GUID som är specifika för din klient (kallas en ”klient-ID”). Kopiera det här värdet – det ska fungera som din **utfärdar-URL**. Du ska konfigurera programmet så att det senare.

### <a name="secrets"></a>Lägg till Azure Active Directory-information för ditt program
1. I den [Azure-portalen], navigera till programmet. Klicka på **inställningar**, och sedan **autentisering/auktorisering**.
2. Om funktionen autentisering/auktorisering inte är aktiverad, aktivera växeln **på**.
3. Klicka på **Azure Active Directory**, och klicka sedan på **Avancerat** under **hanteringsläge**. Klistra in i klient-ID och utfärdar-URL-värdet som du hämtade tidigare. Klicka sedan på **OK**.
   
   ![][1]
   
   Som standard Apptjänst ger autentisering, men begränsar inte auktoriserad åtkomst till webbplatsens innehåll och API: er. Du måste auktorisera användare i din Appkod.
4. (Valfritt) Om du vill begränsa åtkomsten till din webbplats till enbart användare som autentiseras av Azure Active Directory, ange **åtgärd att vidta när begäran inte har autentiserats** till **logga in med Azure Active Directory**. Detta kräver att alla förfrågningar autentiseras och alla oautentiserade begäranden omdirigeras till Azure Active Directory för autentisering.
5. Klicka på **Spara**.

Du är nu redo att använda Azure Active Directory för autentisering i appen.

## <a name="optional-configure-a-native-client-application"></a>(Valfritt) Konfigurera en native client-program
Azure Active Directory kan du också registrera interna klienter, vilket ger dig större kontroll över behörigheter mappning. Du behöver den här om du vill utföra inloggningar som använder ett bibliotek som den **Active Directory Authentication Library**.

1. Gå till **Active Directory** i den [klassiska Azure-portalen].
2. Välj din katalog och välj sedan den **program** högst upp. Klicka på **lägga till** längst ned för att skapa en ny appregistrering.
3. Klicka på **Lägg till ett program som min organisation utvecklar**.
4. I guiden Lägg till program och ange en **namn** för ditt program och klicka på den **internt klientprogram** typen. Klicka om du vill fortsätta.
5. I den **omdirigerings-URI** Ange webbplatsens */.auth/login/done* slutpunkten, med hjälp av HTTPS-schema. Det här värdet ska vara liknar *https://contoso.azurewebsites.net/.auth/login/done*. Om du skapar ett Windows-program i stället använda den [paket-SID](app-service-mobile-dotnet-how-to-use-client-library.md#package-sid) som URI: N.
6. När det ursprungliga programmet har lagts till, klickar du på den **konfigurera** fliken. Hitta de **klient-ID** och anteckna värdet.
7. Rulla ned till sidan den **behörigheter för andra program** avsnittet och klicka på **lägga till program**.
8. Sök efter webbprogrammet som du tidigare har registrerat och klicka på plusikonen. Sedan klicka på Sök om du vill stänga dialogrutan. Om webbprogrammet inte kan hitta, navigera till dess registrering och lägga till en ny reply URL (t.ex. den HTTP-versionen av din aktuella URL), klicka på Spara och sedan upprepa stegen - programmet ska visas i listan.
9. Öppna på den nya posten som du just lagt till den **delegerade behörigheter** listrutan och välj **åtkomst (appName)**. Klicka sedan på **Spara**.

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
