---
title: 'Azure Active Directory B2C: Google + configuration | Microsoft Docs'
description: "Ange tooconsumers för registrering och inloggning med Google + konton i dina program som skyddas av Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: 4dcca66f-29e4-4b4d-8840-50baad736bd7
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: 6ef66eb17777acd95b5f4745ed6097c77e37663b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-google-accounts"></a>Azure Active Directory B2C: Ange tooconsumers för registrering och inloggning med Google + konton
## <a name="create-a-google-application"></a>Skapa ett Google +-program
toouse Google + som en identitetsleverantör i Azure Active Directory (AD Azure) B2C du behöver toocreate Google +-programmet och lämna hello rätt parametrar. Du behöver ett Google + konto toodo detta. Om du inte har något du kan hämta den på [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp).

1. Gå toohello [Google utvecklare konsolen](https://console.developers.google.com/) och logga in med Google + autentiseringsuppgifterna för ditt konto.
2. Klicka på **skapa projekt**, ange en **projektnamn**, och klicka sedan på **skapa**.
   
    ![Google + - komma igång](./media/active-directory-b2c-setup-goog-app/google-get-started.png)
   
    ![Google + - nytt projekt](./media/active-directory-b2c-setup-goog-app/google-new-project.png)
3. Klicka på **API-hanterare** och klicka sedan på **autentiseringsuppgifter** i hello vänstra navigeringsrutan.
4. Klicka på hello **OAuth-medgivande skärmen** fliken hello överst.
   
    ![Google + - autentiseringsuppgifter](./media/active-directory-b2c-setup-goog-app/google-add-cred.png)
5. Välj eller ange en giltig **e-postadress**, ange en **produktnamn**, och klicka på **spara**.
   
    ![Google + - skärmen för OAuth-medgivande](./media/active-directory-b2c-setup-goog-app/google-consent-screen.png)
6. Klicka på **nya autentiseringsuppgifter** och välj sedan **OAuth-klient-ID**.
   
    ![Google + - skärmen för OAuth-medgivande](./media/active-directory-b2c-setup-goog-app/google-add-oauth2-client-id.png)
7. Under **programtyp**väljer **webbprogrammet**.
   
    ![Google + - skärmen för OAuth-medgivande](./media/active-directory-b2c-setup-goog-app/google-web-app.png)
8. Ange en **namn** för ditt program ange `https://login.microsoftonline.com` i hello **behörighet JavaScript ursprung** fältet och `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` i hello **auktoriserad omdirigerings-URI: er**fält. Ersätt **{klient}** med din klient namn (till exempel contosob2c.onmicrosoft.com). Hej **{klient}** värdet är skiftlägeskänsligt. Klicka på **Skapa**.
   
    ![Google + - skapar klient-ID](./media/active-directory-b2c-setup-goog-app/google-create-client-id.png)
9. Kopiera hello värdena för **klient-ID** och **klienthemlighet**. Du måste båda tooconfigure Google + som en identitetsleverantör i din klient. **Klienthemlighet** är en viktig säkerhetsuppgift för autentisering.
   
    ![Google + - klienthemlighet](./media/active-directory-b2c-setup-goog-app/google-client-secret.png)

## <a name="configure-google-as-an-identity-provider-in-your-tenant"></a>Konfigurera Google + som en identitetsleverantör i din klientorganisation
1. Följ dessa steg för[navigera toohello B2C-funktionsbladet](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) på hello Azure-portalen.
2. Klicka på hello B2C-funktionsbladet **identitetsleverantörer**.
3. Klicka på **+ Lägg till** hello överst i hello-bladet.
4. Ange ett eget **namn** för hello identitet Providerkonfiguration. Till exempel ange ”G +”.
5. Klicka på **identitet providertyp**väljer **Google**, och klicka på **OK**.
6. Klicka på **ställa in den här identitetsleverantör** och ange hello klient-ID och klienten hemligheten för hello Google +-programmet som du skapade tidigare.
7. Klicka på **OK** och klicka sedan på **skapa** toosave Google +-konfiguration.

