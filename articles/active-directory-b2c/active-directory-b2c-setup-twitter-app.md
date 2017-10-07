---
title: 'Azure Active Directory B2C: Twitter-konfiguration | Microsoft Docs'
description: "Ange tooconsumers för registrering och logga in med Twitter-konton i dina program som skyddas av Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 579a6841-9329-45b8-a351-da4315a6634e
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 4/06/2017
ms.author: parakhj
ms.openlocfilehash: 275c5c73fd5e8e5075e77fee942cbc1b5e1586cf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-twitter-accounts"></a>Azure Active Directory B2C: Ange tooconsumers för registrering och logga in med Twitter-konton

> [!NOTE]
> Den här funktionen är i förhandsgranskningen.
> 

## <a name="create-a-twitter-application"></a>Skapa ett Twitter-program
toouse Twitter som en identitetsleverantör i Azure Active Directory (AD Azure) B2C du behöver toocreate ett Twitter-program och lämna hello rätt parametrar. Du behöver en Twitter developer konto toodo detta. Om du inte har något du kan hämta den på [https://dev.twitter.com/](https://dev.twitter.com/).

1. Gå toohello [Twitter-utvecklare webbplats](https://dev.twitter.com/) och logga in med dina autentiseringsuppgifter.
2. Klicka på **Mina appar** under **verktyg och Support** och klicka sedan på **Skapa ny App**. 
3. Ange ett värde i hello form för hello **namn**, **beskrivning**, och **webbplats**.
4. För hello **motringning URL**, ange `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp`. Se till att tooreplace **{klient}** med din klient namn (till exempel contosob2c.onmicrosoft.com).
5. Kontrollera hello rutan tooagree toohello **Developer avtal** och på **skapa programmet Twitter**.
6. När hello app har skapats klickar du på **nycklar och åtkomst-token**.
7. Kopiera hello värdet för **konsumenten nyckeln** och **konsumenthemlighet**. Du måste båda tooconfigure Twitter som en identitetsleverantör i din klient.

## <a name="configure-twitter-as-an-identity-provider-in-your-tenant"></a>Konfigurera Twitter som en identitetsleverantör i din klientorganisation
1. Följ dessa steg för[navigera toohello B2C-funktionsbladet](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) på hello Azure-portalen.
2. Klicka på hello B2C-funktionsbladet **identitetsleverantörer**.
3. Klicka på **+ Lägg till** hello överst i hello-bladet.
4. Ange ett eget **namn** för hello identitet Providerkonfiguration. Ange till exempel ”Twitter”.
5. Klicka på **identitet providertyp**väljer **Twitter**, och klicka på **OK**.
6. Klicka på **ställa in den här identitetsleverantör** och ange hello Twitter **konsumenten nyckeln** för hello **klient-id** och hello Twitter **konsumenthemlighet**för hello **klienthemlighet**.
7. Klicka på **OK**, och klicka sedan på **skapa** toosave Twitter-konfiguration.

