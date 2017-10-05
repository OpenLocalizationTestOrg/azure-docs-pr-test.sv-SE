---
title: "Autentisering och auktorisering för API Apps i Azure App Service | Microsoft Docs"
description: "Mer information om autentisering och auktorisering tjänster som Azure App Service tillhandahåller för API Apps."
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
ms.openlocfilehash: f9fd533dfbd54517232f9dae5000ed4779baebd4
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="authentication-and-authorization-for-api-apps-in-azure-app-service"></a>Autentisering och auktorisering för API Apps i Azure App Service
## <a name="overview"></a>Översikt
> [!NOTE]
> Det här avsnittet kommer att migreras till en konsoliderad [App Service autentisering / auktorisering](../app-service/app-service-authentication-overview.md) avsnittet, som omfattar webb-, mobil- och API Apps.
> 
> 

Azure App Service erbjuder inbyggda autentisering och auktorisering services att implementera [OAuth 2.0](#oauth) och [OpenID Connect](#oauth). Den här artikeln beskrivs de alternativ som är tillgängliga för API Apps i Azure App Service och tjänster.

Följande diagram illustrerar vissa kännetecknar Apptjänst autentisering:

* Den preprocesses inkommande API-begäranden, vilket innebär att den fungerar med alla språk eller ramverk som stöds av App Service.
* Får du flera alternativ för hur mycket autentisering fungerar du vill göra i din kod.
* Det fungerar för både slutanvändare och autentiseringen av tjänsten. 
* Den stöder fem identitetsleverantörer: Azure Active Directory, Facebook, Google, Twitter och Account.
* Den fungerar på samma för API Apps, Webbappar och Mobilappar.

![](./media/app-service-api-authentication/api-apps-overview.png)

## <a name="language-agnostic"></a>Oberoende av språk
Bearbetning av App Service-autentisering sker innan begäran når din API-app, vilket innebär att funktionerna för autentisering fungerar för API-appar som skrivits i valfritt språk eller ramverk.  Din API kan baseras på ASP.NET, Java, Node.js och alla ramverk som har stöd för Apptjänst.

Apptjänst skickar på JSON web token (JWT) i auktoriseringshuvudet för en HTTP-begäran och kod som skrivs i valfritt språk eller ramverk kan hämta den information som behövs från token. Dessutom gör Apptjänst att du lättare åtkomst till de mest använda anspråk genom att ange vissa särskilda sidhuvud, till exempel följande:

* X-MS-CLIENT--HUVUDNAMN
* X-MS-CLIENT-SÄKERHETSOBJEKT-ID
* X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN
* X-MS-TOKEN-FACEBOOK-EXPIRES-ON

I en .NET-API kan du använda den `Authorize` attributet och för detaljerade auktorisering kan du enkelt skriva kod baserat på anspråk eftersom anspråk information fylls automatiskt i .NET-klasser.

## <a name="multiple-protection-options"></a>Flera skyddsalternativ
Apptjänst kan förhindra anonyma HTTP-begäranden från att nå din API-app, den kan vidarebefordra alla begäranden och validera token för begäranden som innehåller dem och kan via alla förfrågningar utan att någon åtgärd på dem:

1. Tillåt endast autentiserade begäranden att nå din API-app.
   
    Om en anonym begäran tas emot från en webbläsare, App-tjänsten omdirigerar till en inloggningssida för autentiseringsprovidern (Azure AD, Google, Twitter, etc.) som du väljer. 
   
    Med det här alternativet behöver du inte skriva någon Autentiseringskod på alla i din app och auktoriseringskod är förenklad eftersom de viktigaste anspråk finns i HTTP-huvuden.
2. Tillåt alla begäranden om att nå din API-app, men Validera autentiserade begäranden och skickar vidare autentiseringsinformation i HTTP-rubriker.
   
    Det här alternativet ger mer flexibilitet för hantering av anonyma begäranden, men du måste skriva kod om du vill förhindra anonyma användare från att använda din API. Eftersom de mest populära anspråk skickas i sidhuvuden för HTTP-begäranden, är auktoriseringskod relativt enkla.
3. Tillåt alla begäranden om att nå din API, inte vidtar någon åtgärd för autentisering på begäran.
   
    Det här alternativet lämnar uppgifterna för autentisering och auktorisering hållet programkoden.

I den [Azure-portalen](https://portal.azure.com/), du väljer alternativet som du vill använda den **autentisering / auktorisering** bladet.

![](./media/app-service-api-authentication/authblade.png)

För alternativ 1 och 2 aktiverar **App autentiseringen av tjänsten**, och i den **åtgärd att vidta när begäran inte har autentiserats** listrutan Välj **logga in** eller **Tillåt begäran (ingen åtgärd)**.  Om du väljer **logga in**, du måste välja en autentiseringsprovider och konfigurera providern.

![](./media/app-service-api-authentication/actiontotake.png)

Detaljerad information om hur du konfigurerar autentisering finns [hur du konfigurerar din App tjänstprogram att använda Azure Active Directory-inloggningen](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). Den länkar till andra artiklar för andra autentiseringsproviders artikeln gäller API apps samt mobila appar.

## <a id="internal"></a>Autentiseringen av tjänsten
Apptjänst autentisering fungerar för interna scenarier som för anropas från en API-app till en annan API-app. I det här scenariot hämta en token med hjälp av autentiseringsuppgifter för ett tjänstkonto i stället för autentiseringsuppgifter för användaren. Ett tjänstkonto kallas även en *tjänstens huvudnamn* i Azure Active Directory och autentisering med sådant konto kallas även ett scenario med tjänst-till-tjänst. 

Skydda kallas API-appen med hjälp av Azure Active Directory i scenarier med tjänst-till-tjänst och tillhandahålla en AAD service principal autentiseringstoken när du anropar API-app. Du kan hämta en token genom att ange klientens ID och klientens hemliga från AAD-program. Ingen speciell endast Azure-kod krävs, som används för att vara sanna för att hantera Mobile Services Zumo-token. Ett exempel på det här scenariot med ASP.NET API-appar som omfattas av kursen [Service principal autentisering för API Apps](app-service-api-dotnet-service-principal-auth.md).

Du kan använda klientcertifikat eller grundläggande autentisering om du vill hantera ett scenario med tjänst-till-tjänst utan att använda autentisering i Apptjänst. Information om klientcertifikat i Azure finns [hur att konfigurera TLS ömsesidig autentisering för Web Apps](../app-service-web/app-service-web-configure-tls-mutual-auth.md). Information om grundläggande autentisering i ASP.NET finns [autentisering filter i ASP.NET Web API 2](http://www.asp.net/web-api/overview/security/authentication-filters).

Autentiseringen av tjänsten konto från en Apptjänst logic app till en API-app är ett specialfall som beskrivs i [använda anpassat API som finns på Apptjänst med Logic apps](../logic-apps/logic-apps-custom-hosted-api.md).

## <a name="mobile-client-authentication"></a>Mobila klientautentisering
Information om hur du hanterar autentisering från mobila klienter finns i [dokumentation om autentisering för mobila appar](../app-service-mobile/app-service-mobile-ios-get-started-users.md). Apptjänst autentisering fungerar på samma sätt för mobilappar och API apps.

## <a name="more-information"></a>Mer information
Mer information om autentisering och auktorisering i Azure App Service finns i följande resurser:

* [Expanderande Apptjänst autentisering / auktorisering](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/)
* [Så här konfigurerar du din App tjänstprogram att använda Azure Active Directory-inloggningen](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md) (omfattar länkar för andra autentiseringsproviders överst på sidan.) 

Mer information om OAuth 2.0, OpenID Connect och token JWT (JSON Web) finns i följande resurser.

* [Komma igång med OAuth 2.0](http://shop.oreilly.com/product/0636920021810.do "komma igång med OAuth 2.0") 
* [Introduktion till OAuth2, OpenID Connect och JSON Web token (JWT) - Pluralsight kursen](http://www.pluralsight.com/courses/oauth2-json-web-tokens-openid-connect-introduction) 
* [Bygga och skydda en RESTful-API för flera klienter i ASP.NET - Pluralsight kursen](http://www.pluralsight.com/courses/building-securing-restful-api-aspdotnet)

Mer information om Azure Active Directory finns i följande resurser.

* [Azure AD-scenarier](http://aka.ms/aadscenarios)
* [Azure AD-utvecklarhandboken](http://aka.ms/aaddev)
* [Azure AD-exempel](http://aka.ms/aadsamples)

## <a name="next-steps"></a>Nästa steg
Den här artikeln har beskrivs funktioner för autentisering och auktorisering i Apptjänst som du kan använda för API apps. I nästa kurs i komma igång-serien visar hur du implementerar [användarautentisering i API Apps i Apptjänst](app-service-api-dotnet-user-principal-auth.md).

