---
title: 'Azure Active Directory B2C: WeChat konfiguration | Microsoft Docs'
description: Ge registrering och inloggning tooconsumers WeChat konton i dina program som skyddas av Azure Active Directory B2C.
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: d2424c66-ba68-4d82-847e-d137683527b0
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/26/2017
ms.author: parakhj
ms.openlocfilehash: 92cc3579d818d2379a503ccc695138b33a34466d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-wechat-accounts"></a>Azure Active Directory B2C: Ge registrering och inloggning tooconsumers WeChat konton

> [!NOTE]
> Den här funktionen är i förhandsgranskningen.
> 

## <a name="create-a-wechat-application"></a>Skapa ett WeChat program

toouse WeChat som en identitetsleverantör i Azure Active Directory (AD Azure) B2C du behöver toocreate ett WeChat program och lämna hello rätt parametrar. Du behöver en WeChat konto toodo detta. Om du inte har någon kan skaffa du en genom att registrera dig genom en av sina mobila appar eller genom att använda QT-nummer. Efter det att få ditt konto som har registrerats med hello WeChat developer-programmet. Du hittar mer information [här](http://kf.qq.com/faq/161220Brem2Q161220uUjERB.html).

### <a name="register-a-wechat-application"></a>Registrera en WeChat-program

1. Gå för[https://open.weixin.qq.com/](https://open.weixin.qq.com/) och logga in.
2. Klicka på**管理中心**(management center).
3. Följ hello nödvändiga åtgärder tooregister ett nytt program.
4. För**授权回调域**(återanrop URL), ange `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp`. Till exempel om din `tenant_name` är contoso.onmicrosoft.com set hello URL toobe `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.
5. Hitta och kopiera hello **APP-ID** och **APPKEY**. Du behöver dessa senare.

## <a name="configure-wechat-as-an-identity-provider-in-your-tenant"></a>Konfigurera WeChat som en identitetsleverantör i din klientorganisation
1. Följ dessa steg för[navigera toohello B2C-funktionsbladet](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) på hello Azure-portalen.
2. Klicka på hello B2C-funktionsbladet **identitetsleverantörer**.
3. Klicka på **+ Lägg till** hello överst i hello-bladet.
4. Ange ett eget **namn** för hello identitet Providerkonfiguration. Till exempel ange ”WeChat”.
5. Klicka på **identitet providertyp**väljer **WeChat**, och klicka på **OK**.
6. Klicka på **ställa in den här identitetsleverantören.**
7. Ange hello **Appkey** som du kopierade tidigare som hello **klient-ID**.
8. Ange hello **App hemlighet** som du kopierade tidigare som hello **Klienthemlighet**.
9. Klicka på **OK** och klicka sedan på **skapa** toosave WeChat konfigurationen.

