---
title: 'Azure Active Directory B2C: QT konfiguration | Microsoft Docs'
description: Ge registrering och inloggning tooconsumers QT konton i dina program som skyddas av Azure Active Directory B2C.
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 18c2cf94-8004-4de1-81c2-e45be65ce12d
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/26/2017
ms.author: parakhj
ms.openlocfilehash: 896d6221e01d15de1652a5717cf1f65619101e0c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-qq-accounts"></a>Azure Active Directory B2C: Ge registrering och inloggning tooconsumers QT konton

> [!NOTE]
> Den här funktionen är i förhandsgranskningen.
> 

## <a name="create-a-qq-application"></a>Skapa ett QT-program

toouse QT som en identitetsleverantör i Azure Active Directory (AD Azure) B2C du behöver toocreate ett QT program och lämna hello rätt parametrar. Du behöver en QT konto toodo detta. Om du inte har någon kan du skaffa ett på [https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033](https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033).

### <a name="register-for-hello-qq-developer-program"></a>Registrera dig för hello QT utvecklarprogram

1. Gå toohello [QT developer-portalen](http://open.qq.com) och logga in med autentiseringsuppgifterna för ditt QT.
2. Efter inloggningen gå för[http://open.qq.com/reg](http://open.qq.com/reg) tooregister själv som utvecklare.
3. Välj menyn hello**个人**(enskilda developer).
4. Ange hello krävs information till hello format och på**下一步**(nästa steg).
5. Slutföra verifieringsprocessen för hello e-post.

> [!NOTE]
> Du behöver toowait några dagar toobe godkända efter registrering som utvecklare. 

### <a name="register-a-qq-application"></a>Registrera en QT-program

1. Gå för[https://connect.qq.com/index.html](https://connect.qq.com/index.html).
2. Klicka på**应用管理**(hantering).
3. Klicka på**创建应用**(skapa app).
4. Ange information om hello nödvändiga app.
5. Klicka på**创建应用**(skapa app).
6. Ange information om hello krävs.
7. För hello**授权回调域**(återanrop URL), ange `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp`. Till exempel om din `tenant_name` är contoso.onmicrosoft.com set hello URL toobe `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.
8. Klicka på**创建应用**(skapa app).
9. På sidan Bekräfta hello klickar du på**应用管理**sidan för hantering (hantering) tooreturn toohello app.
10. Klicka på**查看**(Visa) nästa toohello app som du nyss skapade.
11. Klicka på**修改**(redigera).
12. Kopiera från hello överst på sidan för hello, hello **APP-ID** och **APPKEY**.

## <a name="configure-qq-as-an-identity-provider-in-your-tenant"></a>Konfigurera QT som en identitetsleverantör i din klientorganisation
1. Följ dessa steg för[navigera toohello B2C-funktionsbladet](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) på hello Azure-portalen.
2. Klicka på hello B2C-funktionsbladet **identitetsleverantörer**.
3. Klicka på **+ Lägg till** hello överst i hello-bladet.
4. Ange ett eget **namn** för hello identitet Providerkonfiguration. Till exempel ange ”QT”.
5. Klicka på **identitet providertyp**väljer **QT**, och klicka på **OK**.
6. Klicka på **ställa in den här identitetsleverantören.**
7. Ange hello **Appkey** som du kopierade tidigare som hello **klient-ID**.
8. Ange hello **App hemlighet** som du kopierade tidigare som hello **Klienthemlighet**.
9. Klicka på **OK** och klicka sedan på **skapa** toosave QT konfigurationen.

