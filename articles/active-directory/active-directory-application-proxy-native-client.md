---
title: "aaaPublish native client-appar – Azure AD | Microsoft Docs"
description: "Beskriver hur tooenable native client appar toocommunicate med Azure AD Application Proxy Connector tooprovide säker fjärråtkomst tooyour lokala appar."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: f0cae145-e346-4126-948f-3f699747b96e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 0ed2be217bf992f034d8321d5e66569b4cace24f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooenable-native-client-apps-toointeract-with-proxy-applications"></a>Hur tooenable native client appar toointeract med proxy program

Azure Active Directory Application Proxy kan också vara används toopublish native client appar i tillägg tooweb program. Native client-appar skiljer sig från webbprogram eftersom de är installerade på en enhet, medan webbappar kan nås via en webbläsare. 

Application Proxy stöder interna klientprogram av accepterar Azure AD som utfärdade token som skickas i standard godkänner HTTP-huvuden.

![Förhållandet mellan användare och Azure Active Directory publicerade program](./media/active-directory-application-proxy-native-client/richclientflow.png)

Använd hello Azure AD Authentication Library, som tar hand om autentisering och stöder många klienten miljöer, toopublish interna program. Programproxy passar in i hello [programspecifika tooWeb API scenariot](develop/active-directory-authentication-scenarios.md#native-application-to-web-api). Den här artikeln vägleder dig genom hello fyra steg toopublish programspecifika med Application Proxy och hello Azure AD-Autentiseringsbiblioteket. 

## <a name="step-1-publish-your-application"></a>Steg 1: Publicera programmet
Publicera programmet proxy precis som alla andra program och tilldela användare tooaccess ditt program. Mer information finns i [publicera program med programproxy](active-directory-application-proxy-publish.md).

## <a name="step-2-configure-your-application"></a>Steg 2: Konfigurera ditt program
Konfigurera ditt interna program på följande sätt:

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Navigera för**Azure Active Directory** > **App registreringar**.
3. Välj **nya appregistrering**.
4. Ange ett namn för ditt program, Välj **interna** som hello programtyp, och ange hello omdirigerings-URI för ditt program. 

   ![Skapa en ny appregistrering](./media/active-directory-application-proxy-native-client/create.png)
5. Välj **Skapa**.

Mer detaljerad information om hur du skapar en ny appregistrering finns [integrera program med Azure Active Directory](.//develop/active-directory-integrating-applications.md).


## <a name="step-3-grant-access-tooother-applications"></a>Steg 3: Ge åtkomst tooother program
Aktivera det ursprungliga programmet hello toobe exponeras tooother program i din katalog:

1. Fortfarande i **App registreringar**, Välj hello nytt interna program som du nyss skapade.
2. Välj **nödvändiga behörigheter**.
3. Välj **Lägg till**.
4. Öppna hello första steg bör **väljer en API**.
5. Använda hello Sök-fältet toofind hello Application Proxy app som du har publicerat i hello första avsnittet. Välj appen och klicka sedan på **Välj**. 

   ![Sök efter hello proxy app](./media/active-directory-application-proxy-native-client/select_api.png)
6. Öppna hello andra steget **Välj behörigheter**.
7. Använda hello kryssrutan toogrant programspecifika åtkomst tooyour proxy programmet och klicka sedan på **Välj**.

   ![Bevilja åtkomst tooproxy app](./media/active-directory-application-proxy-native-client/select_perms.png)
8. Välj **klar**.


## <a name="step-4-edit-hello-active-directory-authentication-library"></a>Steg 4: Redigera hello Active Directory Authentication Library
Redigera hello interna programkod hello autentisering gäller hello Active Directory Authentication Library (ADAL) tooinclude hello följande text:

```
// Acquire Access Token from AAD for Proxy Application
AuthenticationContext authContext = new AuthenticationContext("https://login.microsoftonline.com/<Tenant ID>");
AuthenticationResult result = authContext.AcquireToken("< External Url of Proxy App >",
        "<App ID of hello Native app>",
        new Uri("<Redirect Uri of hello Native App>"),
        PromptBehavior.Never);

//Use hello Access Token tooaccess hello Proxy Application
HttpClient httpClient = new HttpClient();
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
HttpResponseMessage response = await httpClient.GetAsync("< Proxy App API Url >");
```

hello variabler i hello exempelkod ska ersättas med följande:

* **Klient-ID** hittar du i hello Azure-portalen. Navigera för**Azure Active Directory** > **egenskaper** och kopiera hello Directory-ID. 
* **Externa URL: en** är hello frontend-URL som du angav i hello Proxy-programmet. toofind detta värde, navigera toohello **programproxy** avsnittet hello proxy-appen.
* **App-ID** av hello inbyggda appen kan hittas på hello **egenskaper** sidan hello det ursprungliga programmet.
* **Omdirigerings-URI för hello inbyggda appen** kan hittas på hello **omdirigerings-URI: er** sidan hello det ursprungliga programmet.


## <a name="see-also"></a>Se även

Läs mer om hello programspecifika flödet [programspecifika tooweb API](develop/active-directory-authentication-scenarios.md#native-application-to-web-api)

Hello senaste nyheterna och uppdateringarna finns på hello [bloggen om Application Proxy](http://blogs.technet.com/b/applicationproxyblog/)
