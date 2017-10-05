---
title: Publicera native client - appar i Azure AD | Microsoft Docs
description: "Beskriver hur du aktiverar inbyggd klientprogram att kommunicera med Azure AD Application Proxy Connector att tillhandahålla säker fjärråtkomst till lokala appar."
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
ms.openlocfilehash: bdaa5af6ff5331bc310499586615b48a864c3c5e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-enable-native-client-apps-to-interact-with-proxy-applications"></a>Så här aktiverar du native client appar kan interagera med proxy program

Förutom webbprogram, kan Azure Active Directory Application Proxy också användas att publicera appar-klienten. Native client-appar skiljer sig från webbprogram eftersom de är installerade på en enhet, medan webbappar kan nås via en webbläsare. 

Application Proxy stöder interna klientprogram av accepterar Azure AD som utfärdade token som skickas i standard godkänner HTTP-huvuden.

![Förhållandet mellan användare och Azure Active Directory publicerade program](./media/active-directory-application-proxy-native-client/richclientflow.png)

Använda Azure AD Authentication Library, som tar hand om autentisering och stöder många klienten miljöer, publicering av interna program. Programproxy passar in i den [programspecifika Web API-scenariot](develop/active-directory-authentication-scenarios.md#native-application-to-web-api). Den här artikeln vägleder dig genom fyra stegen för att publicera en interna program med Application Proxy och Azure AD-Autentiseringsbiblioteket. 

## <a name="step-1-publish-your-application"></a>Steg 1: Publicera programmet
Publicera programmet proxy precis som alla andra program och tilldela användare åtkomst till ditt program. Mer information finns i [publicera program med programproxy](active-directory-application-proxy-publish.md).

## <a name="step-2-configure-your-application"></a>Steg 2: Konfigurera ditt program
Konfigurera ditt interna program på följande sätt:

1. Logga in på [Azure Portal](https://portal.azure.com).
2. Gå till **Azure Active Directory** > **App registreringar**.
3. Välj **nya appregistrering**.
4. Ange ett namn för ditt program, Välj **interna** som programtyp, och ange omdirigerings-URI för ditt program. 

   ![Skapa en ny appregistrering](./media/active-directory-application-proxy-native-client/create.png)
5. Välj **Skapa**.

Mer detaljerad information om hur du skapar en ny appregistrering finns [integrera program med Azure Active Directory](.//develop/active-directory-integrating-applications.md).


## <a name="step-3-grant-access-to-other-applications"></a>Steg 3: Ge tillgång till andra program
Aktivera det ursprungliga programmet kan exponeras för andra program i din katalog:

1. Fortfarande i **App registreringar**, Välj ny interna program som du nyss skapade.
2. Välj **nödvändiga behörigheter**.
3. Välj **Lägg till**.
4. Öppna det första steget **väljer en API**.
5. Använd sökfältet för att hitta Application Proxy-appen som du har publicerat i det första avsnittet. Välj appen och klicka sedan på **Välj**. 

   ![Sök efter den proxy-app](./media/active-directory-application-proxy-native-client/select_api.png)
6. Öppna det andra steget **Välj behörigheter**.
7. Använd kryssrutan för att ge din ursprungliga programmet åtkomst till ditt program för proxy och klicka sedan på **Välj**.

   ![Bevilja åtkomst till proxy-app](./media/active-directory-application-proxy-native-client/select_perms.png)
8. Välj **klar**.


## <a name="step-4-edit-the-active-directory-authentication-library"></a>Steg 4: Redigera Active Directory Authentication Library
Redigera det ursprungliga programmet koden i kontexten för autentisering av den Active Directory Authentication Library (ADAL) att inkludera följande text:

```
// Acquire Access Token from AAD for Proxy Application
AuthenticationContext authContext = new AuthenticationContext("https://login.microsoftonline.com/<Tenant ID>");
AuthenticationResult result = authContext.AcquireToken("< External Url of Proxy App >",
        "<App ID of the Native app>",
        new Uri("<Redirect Uri of the Native App>"),
        PromptBehavior.Never);

//Use the Access Token to access the Proxy Application
HttpClient httpClient = new HttpClient();
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
HttpResponseMessage response = await httpClient.GetAsync("< Proxy App API Url >");
```

Variabler i exempelkoden ska ersättas med följande:

* **Klient-ID** kan hittas i Azure-portalen. Gå till **Azure Active Directory** > **egenskaper** och kopiera Directory-ID. 
* **Externa URL: en** är frontend-URL du angav i programmets Proxy. Du hittar det här värdet genom att navigera till den **programproxy** avsnitt av proxy-app.
* **App-ID** av den inbyggda appen kan hittas på den **egenskaper** sidan i det ursprungliga programmet.
* **Omdirigerings-URI för den inbyggda appen** kan hittas på den **omdirigerings-URI: er** sidan i det ursprungliga programmet.


## <a name="see-also"></a>Se även

Mer information om det ursprungliga programmet flödet finns [programspecifika i webb-API](develop/active-directory-authentication-scenarios.md#native-application-to-web-api)

Läs mer om de senaste nyheterna och uppdateringarna i [bloggen om Application Proxy](http://blogs.technet.com/b/applicationproxyblog/)
