---
title: 'Azure Active Directory B2C: Facebook-konfiguration | Microsoft Docs'
description: "Ange tooconsumers för registrering och inloggning med Facebook-konton i dina program som skyddas av Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: sromeroz
manager: krassk
editor: sromeroz
ms.assetid: b875f235-a1d2-4abb-b9f0-b89beac38a32
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/7/2017
ms.author: sromeroz
ms.openlocfilehash: 4c3b79c7248bd1e789bf33fc467abb27d0170edd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-facebook-accounts"></a>Azure Active Directory B2C: Ange tooconsumers för registrering och inloggning med Facebook-konton
## <a name="create-a-facebook-application"></a>Skapa ett Facebook-program
toouse Facebook som en identitetsleverantör i Azure Active Directory (AD Azure) B2C du behöver toocreate Facebook-programmet och lämna hello rätt parametrar. Du behöver en Facebook konto toodo detta. Om du inte har något du kan hämta den på [https://www.facebook.com/](https://www.facebook.com/).

1. Gå toohello [Facebook för utvecklare](https://developers.facebook.com/) webbplatsen och logga in med dina Facebook kontoautentiseringsuppgifter.
2. Om du inte redan har gjort det måste tooregister som utvecklare Facebook. toodo, genom att välja **registrera** (på hello övre högra hörnet på sidan hello) godkänner Facebooks principer och utföra hello registrering steg.
3. Klicka på **Mina appar** och klicka sedan på **lägga till en ny App**. 
4. Hello formuläret och ange en **visningsnamn** och en giltig **kontakta e-post**.
5. Klicka på **skapa App-ID**. Detta kan kräva tooaccept Facebook-plattformen principer och slutföra en kontroll av online-säkerhet.
6. Klicka på hello vänstra kolumnen **inställningar** och välj sedan **grundläggande** om du inte redan har markerats.
7. Välj en **kategori**. 
8. Klicka på **+ Lägg till plattformen** och välj **webbplats**.
   
    ![Facebook - inställningar](./media/active-directory-b2c-setup-fb-app/fb-settings.png)
   
    ![Facebook - inställningar - webbplats](./media/active-directory-b2c-setup-fb-app/fb-website.png)
9. Ange `https://login.microsoftonline.com/` i hello **Webbadress** fältet och klickar sedan på **spara ändringar** på hello hello sidans nederkant.
   
    ![Facebook - Webbadress](./media/active-directory-b2c-setup-fb-app/fb-site-url.png)

10. Kopiera hello värdet för **App-ID**. Klicka på **visa** och kopiera hello värdet för **App hemlighet**. Du måste båda tooconfigure Facebook som en identitetsleverantör i din klient. **Appen hemlighet** är en viktig säkerhetsuppgift för autentisering.
   
    ![Facebook - App-ID & App hemlighet](./media/active-directory-b2c-setup-fb-app/fb-app-id-app-secret.png)
11. Klicka på **+ Lägg till produkten** hello vänstra navigeringsfönstret och sedan hello **konfigurera** knappen för **Facebook inloggningen**.
   
    ![Facebook - Facebook-inloggning](./media/active-directory-b2c-setup-fb-app/fb-login.png)
12. Klicka på **inställningar** på hello rätt nav under **Facebook-inloggning**

    ![Facebook - Facebook inloggningsinställningar](./media/active-directory-b2c-setup-fb-app/fb-login-settings.png)
13. Ange `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` i hello **omdirigerings-URI: er för giltig OAuth** i hello **OAuth klientinställningar** avsnitt. Ersätt **{klient}** med din klient namn (till exempel contosob2c.onmicrosoft.com). Klicka på **spara ändringar** på hello hello sidans nederkant.
    
    ![Facebook - OAuth omdirigerings-URI](./media/active-directory-b2c-setup-fb-app/fb-oauth-redirect-uri.png)
14. toomake Facebook-programmet kan användas av Azure AD B2C måste toomake den allmänt tillgängliga. Du kan göra detta genom att klicka på **App granska** på hello vänstra navigeringsrutan och genom att stänga hello växla hello överst på hello sidan för**Ja** och klicka på **Bekräfta**.
    
    ![Facebook - App offentliga](./media/active-directory-b2c-setup-fb-app/fb-app-public.png)

## <a name="configure-facebook-as-an-identity-provider-in-your-tenant"></a>Konfigurera Facebook som en identitetsleverantör i din klientorganisation
1. Följ dessa steg för[navigera toohello B2C-funktionsbladet](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) på hello Azure-portalen.
2. Klicka på hello B2C-funktionsbladet **identitetsleverantörer**.
3. Klicka på **+ Lägg till** hello överst i hello-bladet.
4. Ange ett eget **namn** för hello identitet Providerkonfiguration. Till exempel ange ”Facebook”.
5. Klicka på **identitet providertyp**väljer **Facebook**, och klicka på **OK**.
6. Klicka på **ställa in den här identitetsleverantör** och ange hello app-ID och app hemligheten (för hello Facebook-programmet som du skapade tidigare) i hello **klient-ID** och **klienthemlighet**respektive fält.
7. Klicka på **OK**, och klicka sedan på **skapa** toosave Facebook-konfiguration.

> [!NOTE]
> Lägga till en **identitetsleverantör** tooyour klient inte ändrar de befintliga principerna. Kom ihåg tooupdate dina principer genom att inkludera hello identitetsleverantör som du nyss skapade.
>