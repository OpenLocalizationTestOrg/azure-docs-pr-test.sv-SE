---
title: "Azure Active Directory B2C: Konfigurationen för Microsoft-kontot | Microsoft Docs"
description: "Ange tooconsumers för registrering och inloggning med Microsoft-konton i dina program som skyddas av Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: 06407322-142c-4cb3-9106-a8d752c4c853
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: bec4777f003c459030f68c35b24f0e4bcddf84ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-microsoft-accounts"></a>Azure Active Directory B2C: Ange tooconsumers för registrering och inloggning med Microsoft-konton
## <a name="create-a-microsoft-account-application"></a>Skapa ett program för Microsoft-konto
toouse Microsoft-kontot som en identitetsleverantör i Azure Active Directory (AD Azure) B2C kan du behöver toocreate ett program för Microsoft-konto och lämna hello rätt parametrar. Du behöver ett Microsoft-konto toodo detta. Om du inte har något du kan hämta den på [https://www.live.com/](https://www.live.com/).

1. Gå toohello [Microsoft Programregistreringsportalen](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) och logga in med ditt Microsoft-kontouppgifter.
2. Klicka på **Lägg till en app**.
   
    ![Microsoft-konto – Lägg till en ny app](./media/active-directory-b2c-setup-msa-app/msa-add-new-app.png)
3. Ange en **namn** för dina program och klickar på **skapa program**.
   
    ![Microsoft-konto - programnamn](./media/active-directory-b2c-setup-msa-app/msa-app-name.png)
4. Kopiera hello värdet för **program-Id**. Du behöver den tooconfigure Microsoft-konto som en identitetsleverantör i din klient.
   
    ![Microsoft-konto - program-Id](./media/active-directory-b2c-setup-msa-app/msa-app-id.png)
5. Klicka på **Lägg till plattformen** och välj **Web**.
   
    ![Microsoft-konto – Lägg till plattformen](./media/active-directory-b2c-setup-msa-app/msa-add-platform.png)
   
    ![Microsoft-konto - webbtjänst](./media/active-directory-b2c-setup-msa-app/msa-web.png)
6. Ange `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` i hello **omdirigerings-URI: er** fältet. Ersätt **{klient}** med din klient namn (till exempel contosob2c.onmicrosoft.com).
   
    ![Microsoft-konto - omdirigerings-URL](./media/active-directory-b2c-setup-msa-app/msa-redirect-url.png)
7. Klicka på **generera nya lösenord** under hello **programmet hemligheter** avsnitt. Kopiera hello nytt lösenord visas på skärmen. Du behöver den tooconfigure Microsoft-konto som en identitetsleverantör i din klient. Lösenordet är en viktig säkerhetsuppgift för autentisering.
   
    ![Microsoft-konto – Skapa nytt lösenord](./media/active-directory-b2c-setup-msa-app/msa-generate-new-password.png)
   
    ![Microsoft-konto - nytt lösenord](./media/active-directory-b2c-setup-msa-app/msa-new-password.png)
8. Hello kryssrutan som anger att kontrollsumman **Live SDK stöd** under hello **avancerade alternativ** avsnitt. Klicka på **Spara**.
   
    ![Microsoft-konto - stöd för Live SDK](./media/active-directory-b2c-setup-msa-app/msa-live-sdk-support.png)

## <a name="configure-microsoft-account-as-an-identity-provider-in-your-tenant"></a>Konfigurera Microsoft-konto som en identitetsleverantör i din klientorganisation
1. Följ dessa steg för[navigera toohello B2C-funktionsbladet](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) på hello Azure-portalen.
2. Klicka på hello B2C-funktionsbladet **identitetsleverantörer**.
3. Klicka på **+ Lägg till** hello överst i hello-bladet.
4. Ange ett eget **namn** för hello identitet Providerkonfiguration. Till exempel ange ”MSA”.
5. Klicka på **identitet providertyp**väljer **Microsoft-konto**, och klicka på **OK**.
6. Klicka på **ställa in den här identitetsleverantör** och ange hello program-Id och lösenord för hello Microsoft-konto-program som du skapade tidigare.
7. Klicka på **OK** och klicka sedan på **skapa** toosave ditt Microsoft-konto konfiguration.

