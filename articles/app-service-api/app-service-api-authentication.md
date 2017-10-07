---
title: "aaaAuthentication och auktorisering för API Apps i Azure App Service | Microsoft Docs"
description: "Läs mer om hello autentisering och auktorisering tjänster som Azure App Service tillhandahåller för API Apps."
services: app-service\api
documentationcenter: .net
author: alexkarcher-msft
manager: erikre
editor: 
ms.assetid: d620b53a-5a6f-41c9-84c7-f7ef5ff02ae7
ms.service: app-service-api
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2016
ms.author: alkarche
ms.openlocfilehash: 6d26754b8b606ec232a74768787922ea80577c95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="authentication-and-authorization-for-api-apps-in-azure-app-service"></a>Autentisering och auktorisering för API Apps i Azure App Service
## <a name="overview"></a>Översikt
> [!NOTE]
> Det här avsnittet kommer att migrerade tooa konsoliderade [App Service autentisering / auktorisering](../app-service/app-service-authentication-overview.md) avsnittet, som omfattar webb-, mobil- och API Apps.
> 
> 

Azure App Service erbjuder inbyggda autentisering och auktorisering services att implementera [OAuth 2.0](#oauth) och [OpenID Connect](#oauth). Den här artikeln beskriver hello tjänster och alternativ som är tillgängliga för API Apps i Azure App Service.

hello följande diagram illustrerar vissa kännetecknar Apptjänst autentisering:

* Den preprocesses inkommande API-begäranden, vilket innebär att den fungerar med alla språk eller ramverk som stöds av App Service.
* Får du flera alternativ för hur mycket autentisering fungerar du vill toodo i din kod.
* Det fungerar för både slutanvändare och autentiseringen av tjänsten. 
* Den stöder fem identitetsleverantörer: Azure Active Directory, Facebook, Google, Twitter och Account.
* Det fungerar hello samma för API Apps, Webbappar och Mobilappar.

![](./media/app-service-api-authentication/api-apps-overview.png)

## <a name="language-agnostic"></a>Oberoende av språk
Bearbetning av App Service-autentisering sker innan begäran når din API-app, vilket innebär att hello autentiseringsfunktioner fungerar för API-appar som skrivits i valfritt språk eller ramverk.  Din API kan baseras på ASP.NET, Java, Node.js och alla ramverk som har stöd för Apptjänst.

Apptjänst skickar på hello JSON web token (JWT) i hello Authorization-huvud för en HTTP-begäran och kod som skrivs i valfritt språk eller ramverk kan få hello information som behövs från hello token. Dessutom ger Apptjänst enklare åtkomst toohello vanligaste anspråk genom att ange vissa särskilda sidhuvud, till exempel hello följande:

* X-MS-CLIENT--HUVUDNAMN
* X-MS-CLIENT-SÄKERHETSOBJEKT-ID
* X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN
* X-MS-TOKEN-FACEBOOK-EXPIRES-ON

I en .NET-API kan du använda hello `Authorize` attributet och för detaljerade auktorisering kan du enkelt skriva kod baserat på anspråk eftersom anspråk information fylls automatiskt i .NET-klasser.

## <a name="multiple-protection-options"></a>Flera skyddsalternativ
Apptjänst kan förhindra anonyma HTTP-begäranden från att nå din API-app, den kan vidarebefordra alla begäranden och validera token för begäranden som innehåller dem och kan via alla förfrågningar utan att någon åtgärd på dem:

1. Tillåt endast autentiserade begäranden tooreach API-appen.
   
    Om en anonym begäran tas emot från en webbläsare, App-tjänsten omdirigerar tooa inloggningssidan för hello autentiseringsprovidern (Azure AD, Google, Twitter, etc.) som du väljer. 
   
    Med det här alternativet behöver du inte toowrite alla Autentiseringskod alls i din app och auktoriseringskod är förenklad eftersom hello viktigaste anspråk finns i hello HTTP-huvuden.
2. Tillåt alla begäranden tooreach API-appen, men Validera autentiserade begäranden och skickar vidare autentiseringsinformation i hello HTTP-huvuden.
   
    Det här alternativet ger mer flexibilitet för hantering av anonyma begäranden, men du har toowrite kod om du vill tooprevent anonyma användare från att använda din API. Auktoriseringskoden är relativt enkel eftersom hello populäraste anspråk skickas i hello sidhuvuden för HTTP-begäranden.
3. Tillåt alla begäranden tooreach ditt API, inte vidtar någon åtgärd för autentisering på hello begäranden.
   
    Det här alternativet lämnar hello uppgifterna för autentisering och auktorisering helt upp tooyour programkod.

I hello [Azure-portalen](https://portal.azure.com/), du väljer hello alternativ på hello **autentisering / auktorisering** bladet.

![](./media/app-service-api-authentication/authblade.png)

För alternativ 1 och 2 aktiverar **App autentiseringen av tjänsten**, och i hello **åtgärd tootake när begäran inte har autentiserats** listrutan Välj **logga in** eller **Tillåt begäran (ingen åtgärd)**.  Om du väljer **logga in**, du har toochoose en autentiseringsprovider och konfigurera den providern.

![](./media/app-service-api-authentication/actiontotake.png)

Detaljerad information om hur tooconfigure autentisering, se [hur tooconfigure Apptjänst programmet toouse Azure Active Directory inloggningen](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). den länkar tooother artiklar för hello andra autentiseringsproviders hello artikeln gäller tooAPI appar samt mobila appar.

## <a id="internal"></a>Autentiseringen av tjänsten
Apptjänst autentisering fungerar för interna scenarier som för anropas från en API-app tooanother API-app. I det här scenariot hämta en token med hjälp av autentiseringsuppgifter för ett tjänstkonto i stället för autentiseringsuppgifter för användaren. Ett tjänstkonto kallas även en *tjänstens huvudnamn* i Azure Active Directory och autentisering med sådant konto kallas även ett scenario med tjänst-till-tjänst. 

Skydda hello kallas API-app med Azure Active Directory för scenarier med tjänst-till-tjänst och tillhandahålla en AAD service principal autentiseringstoken när du anropar hello API-app. Du kan hämta en token genom att ange hello klient-ID och klientens hemliga från hello AAD-program. Ingen speciell endast Azure-kod är krävs, t.ex använda toobe sant för att hantera hello Mobile Services Zumo token. Ett exempel på det här scenariot med ASP.NET API-appar som omfattas av hello kursen [Service principal autentisering för API Apps](app-service-api-dotnet-service-principal-auth.md).

Om du vill toohandle ett scenario med tjänst-till-tjänst utan att använda Apptjänst autentisering kan använda du klientcertifikat eller grundläggande autentisering. Information om klientcertifikat i Azure finns [hur tooConfigure TLS ömsesidig autentisering för Web Apps](../app-service-web/app-service-web-configure-tls-mutual-auth.md). Information om grundläggande autentisering i ASP.NET finns [autentisering filter i ASP.NET Web API 2](http://www.asp.net/web-api/overview/security/authentication-filters).

Autentiseringen av tjänsten konto från en Apptjänst logic app tooan API-app är ett specialfall som beskrivs i [använda anpassat API som finns på Apptjänst med Logic apps](../logic-apps/logic-apps-custom-hosted-api.md).

## <a name="mobile-client-authentication"></a>Mobila klientautentisering
Information om hur toohandle autentisering från mobila klienter, se hello [dokumentation om autentisering för mobila appar](../app-service-mobile/app-service-mobile-ios-get-started-users.md). Apptjänst autentisering fungerar hello samma sätt för mobilappar och API apps.

## <a name="more-information"></a>Mer information
Mer information om autentisering och auktorisering i Azure App Service finns hello följande resurser:

* [Expanderande Apptjänst autentisering / auktorisering](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/)
* [Hur tooconfigure Apptjänst programmet toouse Azure Active Directory inloggningen](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md) (omfattar länkar för andra autentiseringsproviders hello överst på hello sidan.) 

Mer information om OAuth 2.0, OpenID Connect och token JWT (JSON Web) finns i hello följande resurser.

* [Komma igång med OAuth 2.0](http://shop.oreilly.com/product/0636920021810.do "komma igång med OAuth 2.0") 
* [Introduktion tooOAuth2 OpenID Connect och JSON Web token (JWT) - Pluralsight kursen](http://www.pluralsight.com/courses/oauth2-json-web-tokens-openid-connect-introduction) 
* [Bygga och skydda en RESTful-API för flera klienter i ASP.NET - Pluralsight kursen](http://www.pluralsight.com/courses/building-securing-restful-api-aspdotnet)

Mer information om Azure Active Directory finns i hello följande resurser.

* [Azure AD-scenarier](http://aka.ms/aadscenarios)
* [Azure AD-utvecklarhandboken](http://aka.ms/aaddev)
* [Azure AD-exempel](http://aka.ms/aadsamples)

## <a name="next-steps"></a>Nästa steg
Den här artikeln har beskrivs funktioner för autentisering och auktorisering i Apptjänst som du kan använda för API apps. hello nästa kurs i hello komma igång serien visar hur tooimplement [användarautentisering i API Apps i Apptjänst](app-service-api-dotnet-user-principal-auth.md).

