---
title: 'Azure Active Directory B2C: LinkedIn-konfiguration | Microsoft Docs'
description: "Ange tooconsumers för registrering och inloggning med LinkedIn-konton i dina program som skyddas av Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: fa51a16b-9ce9-4e27-9eff-0869b4c4f0ef
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: 6c5233ef48b24968fd6383f470b5d8a969a78ecf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-linkedin-accounts"></a>Azure Active Directory B2C: Ge registrering och inloggning tooconsumers LinkedIn-konton
## <a name="create-a-linkedin-application"></a>Skapa ett LinkedIn-program
toouse LinkedIn som en identitetsleverantör i Azure Active Directory (AD Azure) B2C du behöver toocreate ett LinkedIn-program och lämna hello rätt parametrar. Du behöver en LinkedIn konto toodo detta. Om du inte har något du kan hämta den på [https://www.linkedin.com/](https://www.linkedin.com/).

1. Gå toohello [LinkedIn utvecklare webbplats](https://www.developer.linkedin.com/) och logga in med autentiseringsuppgifterna för ditt LinkedIn.
2. Klicka på **Mina appar** i hello översta menyraden och klicka sedan på **skapa program**.
   
    ![LinkedIn - ny app](./media/active-directory-b2c-setup-li-app/linkedin-new-app.png)
3. I hello **skapa ett nytt program** formuläret, Fyll i relevanta hello (**företagsnamn**, **namn**, **beskrivning**, **Programmets logotypen URL**, **användningen av**, **Webbadress**, **företags-e-** och **telefon**).
4. Godkänner toohello **LinkedIn API användningsvillkoren** och på **skicka**.
   
    ![LinkedIn - registrera appen](./media/active-directory-b2c-setup-li-app/linkedin-register-app.png)
5. Kopiera hello värdena för **klient-ID** och **Klienthemlighet**. (Du hittar dem under **autentiseringsnycklar**.) Du måste båda tooconfigure LinkedIn som en identitetsleverantör i din klient.
   
   > [!NOTE]
   > **Klienthemlighet** är en viktig säkerhetsuppgift för autentisering.
   > 
   > 
6. Ange `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` i hello **behörighet omdirigerings-URL: er** fält (under **OAuth 2.0**). Ersätt **{klient}** med din klient namn (till exempel contoso.onmicrosoft.com). Klicka på **Lägg till**, och klicka sedan på **uppdatering**. Hej **{klient}** värdet är skiftlägeskänsligt.
   
    ![LinkedIn - installationsprogrammet för app](./media/active-directory-b2c-setup-li-app/linkedin-setup.png)

## <a name="configure-linkedin-as-an-identity-provider-in-your-tenant"></a>Konfigurera LinkedIn som en identitetsleverantör i din klientorganisation
1. Följ dessa steg för[navigera toohello B2C-funktionsbladet](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) på hello Azure-portalen.
2. Klicka på hello B2C-funktionsbladet **identitetsleverantörer**.
3. Klicka på **+ Lägg till** hello överst i hello-bladet.
4. Ange ett eget **namn** för hello identitet Providerkonfiguration. Ange till exempel ”LI”.
5. Klicka på **identitet providertyp**väljer **LinkedIn**, och klicka på **OK**.
6. Klicka på **ställa in den här identitetsleverantör** och ange hello klient-ID och klienten hemligheten för hello LinkedIn-program som du skapade tidigare.
7. Klicka på **OK** och klicka sedan på **skapa** toosave LinkedIn-konfiguration.

