---
title: 'Azure Active Directory B2C: Programregistrering | Microsoft Docs'
description: Hur tooregister ditt program med Azure Active Directory B2C
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: PatAltimore
ms.assetid: 20e92275-b25d-45dd-9090-181a60c99f69
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 6/13/2017
ms.author: parakhj
ms.openlocfilehash: bd58e123751db387d6c8f16bd010291ba698b1a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-register-your-application"></a>Azure Active Directory B2C: Registrera ditt program

Med hjälp av den här snabbstarten registrerar du ett program i en Microsoft Azure Active Directory (Azure AD) B2C-klientorganisation på några få minuter. När du är klar registreras ditt program för användning i hello Azure B2C-klient.

## <a name="prerequisites"></a>Krav

toobuild ett program som accepterar konsumenten registrering och inloggning, måste du först tooregister hello program med en Azure Active Directory B2C-klient. Skaffa en egen klient genom att använda hello steg som beskrivs i [skapa en Azure AD B2C-klient](active-directory-b2c-get-started.md).

Program som har skapats från hello Azure AD B2C-bladet i hello Azure-portalen måste hanteras från hello samma plats. Om du redigerar hello B2C-program med hjälp av PowerShell eller en annan portal, blir stöds inte och fungerar inte med Azure AD B2C. Mer information finns i hello [fel appar](#faulted-apps) avsnitt. 

## <a name="navigate-toob2c-settings"></a>Navigera tooB2C inställningar

Logga in toohello [Azure-portalen](https://portal.azure.com/) som hello Global administratör för hello B2C-klienten. 

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](../../includes/active-directory-b2c-portal-navigate-b2c-service.md)]

Välj nästa steg baserat på hello programtyp som du registrerar:

* [Registrera ett webbprogram](#register-a-web-app)
* [Registrera ett webb-API](#register-a-web-api)
* [Registrera ett mobilt eller inbyggt program](#register-a-mobile-or-native-app)
 
## <a name="register-a-web-app"></a>Registrera en webbapp

[!INCLUDE [active-directory-b2c-register-web-app](../../includes/active-directory-b2c-register-web-app.md)]

Gör så här om webbprogrammet anropar ett webb-API som skyddas av Azure AD B2C:
   1. Skapa en programhemlighet genom att gå toohello **nycklar** bladet och klicka på hello **Generera nyckel** knappen. Anteckna hello **appkey** värde. Du kan använda hello-värde som hello programhemlighet i din programkod.
   2. Klicka på **API-åtkomst**, **Lägg till** och välj webb-API och områden (behörigheter).

> [!NOTE]
> En **programhemlighet** är en viktig autentiseringsuppgift och bör skyddas på lämpligt sätt.
> 

[Hoppa för**nästa steg**](#next-steps)

## <a name="register-a-web-api"></a>Registrera ett webb-API

[!INCLUDE [active-directory-b2c-register-web-api](../../includes/active-directory-b2c-register-web-api.md)]

Klicka på **publicerade scope** tooadd mer scope som krävs. Som standard definieras hello ”user_impersonation” omfång. Hej user_impersonation omfång ger andra program hello möjlighet tooaccess detta api hello inloggade användarens räkning. Om du vill kan hello user_impersonation omfång tas bort.

[Hoppa för**nästa steg**](#next-steps)

## <a name="register-a-mobile-or-native-app"></a>Registrera en mobil eller inbyggd app

[!INCLUDE [active-directory-b2c-register-mobile-native-app](../../includes/active-directory-b2c-register-mobile-native-app.md)]

[Hoppa för**nästa steg**](#next-steps)

## <a name="limitations"></a>Begränsningar

### <a name="choosing-a-web-app-or-api-reply-url"></a>Om du väljer en webbapp eller en api-svarswebbadress

Appar som har registrerats med Azure AD B2C finns för närvarande begränsad tooa begränsad uppsättning reply URL-värden. Hej reply URL: en för webbprogram och tjänster måste börja med hello schema `https`, och alla svara URL-värden måste dela en enda DNS-domän. Exempelvis kan du registrera ett webbprogram som har en av dessa svars-URL: er:

`https://login-east.contoso.com`

`https://login-west.contoso.com`

hello registreringssystem jämför hello hela DNS-namnet på hello befintliga reply URL toohello DNS-namnet på hello reply-URL som du lägger till. hello begäran tooadd hello DNS-namnet misslyckas om någon av hello följande villkor föreligger:

* hello hela DNS-namnet på hello nya reply URL matchar inte hello DNS-namnet på hello befintliga reply-URL.
* hello hela DNS-namnet på hello nya reply-URL är inte en underdomän till hello befintliga reply-URL.

Om hello har appen reply-URL:

`https://login.contoso.com`

Du kan lägga till tooit så här:

`https://login.contoso.com/new`

I det här fallet matchar hello DNS-namnet exakt. Du kan också göra detta:

`https://new.login.contoso.com`

I så fall måste refererar du tooa DNS-underdomänen av login.contoso.com. Om du vill toohave en app som har inloggningen east.contoso.com och inloggnings-west.contoso.com som svara URL: er, måste du lägga till dessa svars-URL: er i den här ordningen:

`https://contoso.com`

`https://login-east.contoso.com`

`https://login-west.contoso.com`

Du kan lägga till hello två senare eftersom de är underdomäner hello första reply URL contoso.com.

### <a name="choosing-a-native-app-redirect-uri"></a>Om du väljer en omdirigerings-URI för en inbyggd app

Det finns två viktiga överväganden när du väljer en omdirigerings-URI för mobila/interna program:

* **Unik**: hello ordning med hello omdirigerings-URI måste vara unikt för varje program. I vårt exempel (com.onmicrosoft.contoso.appname://redirect/path) använder vi com.onmicrosoft.contoso.appname som hello schema. Vi rekommenderar att detta mönster följs. Om två program delar hello samma schemat, hello användaren ser en dialogruta med ett ”Välj app”. Om hello användare gör ett felaktigt val, misslyckas hello inloggningen.
* **Fullständig**: Omdirigerings-URI måste ha ett schema och en sökväg. hello sökväg måste innehålla minst ett snedstreck hello domän (till exempel //contoso/ fungerar och //contoso misslyckas).

Se till att det inte finns några specialtecken som understreck i hello omdirigerings-uri.

### <a name="faulted-apps"></a>Felaktig appar

B2C program bör INTE redigeras:

* På andra programhanteringsportaler, som den [klassiska Azure-portalen](https://manage.windowsazure.com/) & den [Programregistreringsportalen](https://apps.dev.microsoft.com/).
* Med Graph API eller PowerShell

Om du redigerar hello B2C programmet enligt ovan och försök tooedit den igen i hello Azure AD B2C-funktionsbladet på hello Azure-portalen, blir den en felaktig app och programmet inte längre kan användas med Azure AD B2C. Du har toodelete hello programmet och skapa den igen.

toodelete hello app, gå toohello [Programregistreringsportalen](https://apps.dev.microsoft.com/) och ta bort det hello-programmet. För hello programmet toobe synliga måste toobe hello ägare av programmet hello (och inte bara en administratör av hello innehavaren).

## <a name="next-steps"></a>Nästa steg

Nu när du har registrerat ett program med Azure AD B2C kan du slutföra en av [våra snabbstartsguider](active-directory-b2c-overview.md#get-started) tooget igång.

> [!div class="nextstepaction"]
> [Skapa en ASP.NET-webbapp med registrering, inloggning och lösenordsåterställning](active-directory-b2c-devquickstarts-web-dotnet-susi.md)