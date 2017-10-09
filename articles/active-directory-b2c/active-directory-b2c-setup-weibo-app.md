---
title: 'Azure Active Directory B2C: Weibo konfiguration | Microsoft Docs'
description: Ge registrering och inloggning tooconsumers Weibo konton i dina program som skyddas av Azure Active Directory B2C.
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 1860de34-94cb-4ceb-851e-102f930f7230
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/26/2017
ms.author: parakhj
ms.openlocfilehash: c0648620f318046c1d9d24feb92a0c5f19c6a91a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-weibo-accounts"></a>Azure Active Directory B2C: Ge registrering och inloggning tooconsumers Weibo konton

> [!NOTE]
> Den här funktionen är i förhandsgranskningen. Använd inte den här identitetsleverantör i produktionsmiljön.
> 

## <a name="create-a-weibo-application"></a>Skapa ett Weibo program

toouse Weibo som en identitetsleverantör i Azure Active Directory (AD Azure) B2C du behöver toocreate ett Weibo program och lämna hello rätt parametrar. Du behöver en Weibo konto toodo detta. Om du inte har någon kan du skaffa ett på [http://weibo.com/signup/signup.php?lang=en-us](http://weibo.com/signup/signup.php?lang=en-us).

### <a name="register-for-hello-weibo-developer-program"></a>Registrera dig för hello Weibo utvecklarprogram

1. Gå toohello [Weibo utvecklarportalen](http://open.weibo.com/) och logga in med autentiseringsuppgifterna för ditt Weibo.
2. När du har loggat in, klicka på ditt namn i hello övre högra hörnet.
3. Välj i listrutan hello**编辑开发者信息**(redigera information för utvecklare).
4. Ange hello krävs information till hello format och på**提交**(skicka).
5. Slutföra verifieringsprocessen för hello e-post.
6. Gå toohello [identitet bekräftelsesidan](http://open.weibo.com/developers/identity/edit).
7. Ange hello krävs information till hello format och på**提交**(skicka).

### <a name="register-a-weibo-application"></a>Registrera en Weibo-program

1. Gå toohello [nya Weibo app registreringssidan](http://open.weibo.com/apps/new).
2. Ange information om programmet hello.
3. Klicka på**创建**(skapa).
4. Kopiera hello värdena för **Appkey** och **App hemlighet**. Du behöver det senare.
5. Överför hello krävs foton och ange hello nödvändig information.
6. Klicka på**保存以上信息**(spara).
7. Klicka på**高级信息**(Avancerat information).
8. Klicka på**编辑**(redigera) nästa toohello fält för OAuth2.0**授权设置**(omdirigera URL).
9. Ange `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp` för OAuth2.0**授权设置**(omdirigera URL). Till exempel om din `tenant_name` är contoso.onmicrosoft.com set hello URL toobe `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.
10. Klicka på**提交**(skicka).  

## <a name="configure-weibo-as-an-identity-provider-in-your-tenant"></a>Konfigurera Weibo som en identitetsleverantör i din klientorganisation
1. Följ dessa steg för[navigera toohello B2C-funktionsbladet](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) på hello Azure-portalen.
2. Klicka på hello B2C-funktionsbladet **identitetsleverantörer**.
3. Klicka på **+ Lägg till** hello överst i hello-bladet.
4. Ange ett eget **namn** för hello identitet Providerkonfiguration. Till exempel ange ”Weibo”.
5. Klicka på **identitet providertyp**väljer **Weibo**, och klicka på **OK**.
6. Klicka på **ställa in den här identitetsleverantören.**
7. Ange hello **Appkey** som du kopierade tidigare som hello **klient-ID**.
8. Ange hello **App hemlighet** som du kopierade tidigare som hello **Klienthemlighet**.
9. Klicka på **OK** och klicka sedan på **skapa** toosave Weibo konfigurationen.

