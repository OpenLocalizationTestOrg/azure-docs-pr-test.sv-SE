---
title: 'Azure Active Directory B2C: Amazon-konfiguration | Microsoft Docs'
description: "Ange tooconsumers för registrering och inloggning med Amazon-konton i dina program som skyddas av Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: 77c099bb-a005-4d75-87f9-f61e3de48725
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: 60d7c4b76d9d3e86ed535765329abed07f1e5996
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-amazon-accounts"></a>Azure Active Directory B2C: Ange tooconsumers för registrering och inloggning med Amazon-konton
## <a name="create-an-amazon-application"></a>Skapa ett Amazon-program
toouse Amazon som en identitetsleverantör i Azure Active Directory (AD Azure) B2C du behöver toocreate ett Amazon-program och lämna hello rätt parametrar. Du behöver en Amazon-konto toodo detta. Om du inte har något du kan hämta den på [http://www.amazon.com/](http://www.amazon.com/).

1. Gå toohello [Amazon Developer Center](https://login.amazon.com/) och logga in med autentiseringsuppgifterna för ditt Amazon-konto.
2. Om du inte redan har gjort det, klickar du på **registrera dig**gör hello developer registrering och acceptera hello princip.
3. Klicka på **registrera nya program**.
   
    ![Registrera ett nytt program på hello Amazon-webbplatsen](./media/active-directory-b2c-setup-amzn-app/amzn-new-app.png)
4. Ange programinformationen (**namn**, **beskrivning**, och **meddelande Sekretesswebbadress**) och klicka på **spara**.
   
    ![Tillhandahåller information om programmet för att registrera en ny App på Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-register-app.png)
5. I hello **webbinställningar** avsnittet Kopiera hello värdena för **klient-ID** och **Klienthemlighet**. (Du behöver tooclick hello **visa hemlighet** knappen toosee detta.) Du måste båda tooconfigure Amazon som en identitetsleverantör i din klient. Klicka på **redigera** längst hello hello-avsnittet. **Klienthemlighet** är en viktig säkerhetsuppgift för autentisering.
   
    ![Med klient-ID och Klienthemlighet för det nya programmet på Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-client-secret.png)
6. Ange `https://login.microsoftonline.com` i hello **tillåtna JavaScript ursprung** fält och `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` i hello **tillåtna returnerar URL: er** fältet. Ersätt **{klient}** med din klient namn (till exempel contoso.onmicrosoft.com). Klicka på **Spara**. Hej **{klient}** värdet är skiftlägeskänsligt.
   
    ![Med JavaScript ursprung och returnera URL: er för det nya programmet på Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-urls.png)

## <a name="configure-amazon-as-an-identity-provider-in-your-tenant"></a>Konfigurera Amazon som en identitetsleverantör i din klientorganisation
1. Följ dessa steg för[navigera toohello B2C-funktionsbladet](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) på hello Azure-portalen.
2. Klicka på hello B2C-funktionsbladet **identitetsleverantörer**.
3. Klicka på **+ Lägg till** hello överst i hello-bladet.
4. Ange ett eget **namn** för hello identitet Providerkonfiguration. Till exempel ange ”Amzn”.
5. Klicka på **identitet providertyp**väljer **Amazon**, och klicka på **OK**.
6. Klicka på **ställa in den här identitetsleverantör** och ange hello klient-ID och klienten hemligheten för hello Amazon-program som du skapade tidigare.
7. Klicka på **OK** och klicka sedan på **skapa** toosave Amazon-konfiguration.

