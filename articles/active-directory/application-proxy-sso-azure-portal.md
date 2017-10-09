---
title: aaaSingle inloggning tooapps med Azure AD Application Proxy | Microsoft Docs
description: "Aktivera enkel inloggning för din publicerade lokala program med Azure AD Application Proxy i hello Azure-portalen."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: d94ac3f4-cd33-4c51-9d19-544a528637d4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 5ff288d36163b74215677d9e34c93c985ac33d54
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="password-vaulting-for-single-sign-on-with-application-proxy"></a>Lösenord vaulting för enkel inloggning med Application Proxy

Azure Active Directory Application Proxy hjälper dig att öka produktiviteten genom att publicera lokala program så att fjärranslutna anställda kan på ett säkert sätt komma åt dem för. I hello Azure-portalen, kan du också konfigurera enkel inloggning (SSO) toothese appar. Användarna behöver bara tooauthenticate med Azure AD och de kan komma åt enterprise-programmet utan att behöva toosign igen.

Application Proxy stöder flera [enkel inloggning lägen](application-proxy-sso-overview.md). Lösenordsbaserade inloggning är avsedd för program som använder en kombination av användarnamn/lösenord för autentisering. Användarna har toosign i toohello lokalt program en gång när du konfigurerar lösenordsbaserade inloggning för ditt program. Efter det Azure Active Directory lagrar hello inloggningsinformation och ger automatiskt den toohello programmet när användarna få fjärråtkomst till den. 

Du bör redan har publicerats och testat din app med Application Proxy. Om inte, gör hello i [publicera program med Azure AD Application Proxy](application-proxy-publish-azure-portal.md) sedan återvända hit. 

## <a name="set-up-password-vaulting-for-your-application"></a>Konfigurera lösenord vaulting för ditt program

1. Logga in toohello [Azure-portalen](https://portal.azure.com) som administratör.
2. Välj **Azure Active Directory** > **företagsprogram** > **alla program**.
3. Välj hello listan hello app som du vill tooset upp med enkel inloggning.  
4. Välj **enkel inloggning**.

   ![Välja enkel inloggning](./media/application-proxy-sso-azure-portal/select-sso.png)

5. Hello SSO-läge, Välj **lösenordsbaserade inloggning**.
6. För hello inloggnings-URL, anger du hello URL för hello sida där användare kan ange sina användarnamn och lösenord toosign tooyour app utanför hello företagsnätverket. Detta kan vara hello extern URL som du skapade när du har publicerat hello appen via Application Proxy. 

   ![Välj lösenordsbaserade inloggning och ange URL: en](./media/application-proxy-sso-azure-portal/password-sso.png)

7. Välj **Spara**.

<!-- Need toorepro?
7. hello page should tell you that a sign-in form was successfully detected at hello provided URL. If it doesn't, select **Configure [your app name] Password Single Sign-on Settings** and choose **Manually detect sign-in fields**. Follow hello instructions toopoint out where hello sign-in credentials go. 
-->

## <a name="test-your-app"></a>Testa din app

Gå tooexternal URL som du konfigurerade för fjärråtkomst tooyour program. Logga in med dina autentiseringsuppgifter för appen (eller hello autentiseringsuppgifter för ett testkonto som du skapat med åtkomst). När du loggar in korrekt, bör du vara kan tooleave hello app och gå tillbaka utan att ange dina autentiseringsuppgifter igen. 

## <a name="next-steps"></a>Nästa steg

- Läs mer om andra sätt tooimplement [enkel inloggning med Application Proxy](application-proxy-sso-overview.md)
- Lär dig mer om [säkerhetsaspekter för att komma åt appar med Azure AD Application Proxy](application-proxy-security-considerations.md)