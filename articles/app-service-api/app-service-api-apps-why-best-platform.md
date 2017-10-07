---
title: aaaAPI appar introduktion | Microsoft Docs
description: "Lär dig hur Azure Apptjänst hjälper dig att utveckla, hantera och använda RESTful-API:er."
services: app-service\api
documentationcenter: .net
author: alexkarcher-msft
manager: erikre
editor: 
ms.assetid: 60049a16-8159-47aa-a34b-110be0d8dab6
ms.service: app-service-api
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/23/2016
ms.author: alkarche
ms.openlocfilehash: f066e96db667d4685480001bc43c2b1bff4ea352
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="api-apps-overview"></a>Översikt över API Apps
API apps i Azure App Service har funktioner som gör det enklare toodevelop värd och använda API: er i hello molnet och lokalt. Med API Apps får du säkerhet i företagsklass, enkel åtkomstkontroll, hybridanslutning, automatisk SDK-generering och smidig integrering med [Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md).

[Azure Apptjänst](../app-service/app-service-value-prop-what-is.md) är en helt hanterad plattform för webb-, mobil- och integreringsscenarier. API Apps är en av fyra apptyper som erbjuds av [Azure Apptjänst](../app-service/app-service-value-prop-what-is.md).

![Apptyper i Azure Apptjänst](./media/app-service-api-apps-why-best-platform/appservicesuite.png)

## <a name="why-use-api-apps"></a>Varför använda API Apps?
Här följer några viktiga funktioner i API Apps:

* **Ta din befintliga API som-är** - du inte har någon hello code toochange i din befintliga API: er tootake nytta av API Apps--distribuera bara din kod tooan API-app. Din API kan använda alla språk eller ramverk som stöds av Apptjänst, bland annat ASP.NET och C#, Java, PHP, Node.js och Python.
* **Enkel användning** – inbyggt stöd för [Swagger API-metadata](http://swagger.io/) gör dina API:er lätta att använda för många olika klienter.  Generera automatiskt klientkod för dina API:er på flera olika språk, bland annat C#, Java och Javascript. Konfigurera enkelt [CORS](app-service-api-cors-consume-javascript.md) utan att ändra din kod. Mer information finns i [Metadata om API Apps i Apptjänst för API-identifiering och kodgenerering](app-service-api-metadata.md) och [Använda en API-app från JavaScript med CORS](app-service-api-cors-consume-javascript.md). 
* **Enkel åtkomstkontroll** -skydda en API-app från obehörig åtkomst utan ändringar tooyour kod. Inbyggda autentiseringstjänster skyddar API:erna mot åtkomst från andra tjänster eller från klienter som representerar användare. Identitetsleverantörer som stöds är bland annat Azure Active Directory, Facebook, Twitter, Google och Microsoft Account. Klienter kan använda Active Directory Authentication Library (ADAL) eller hello Mobile Apps-SDK. Mer information finns i [Autentisering och auktorisering för API Apps i Azure Apptjänst](app-service-api-authentication.md).
* **Visual Studio-integrationen** – dedikerade verktyg i Visual Studio effektiviserar hello arbetet med att skapa, distribuera, använda, felsökning och hantera API apps. Mer information finns i [Announcing hello Azure SDK 2.8.1 för .NET](https://azure.microsoft.com/blog/announcing-azure-sdk-2-8-1-for-net/).
* **Integrering med Logic Apps** – API Apps som du skapar kan användas av [Logic Apps i Apptjänst](../logic-apps/logic-apps-what-are-logic-apps.md).  Mer information finns i [Använda anpassat API som finns på Apptjänst med Logic Apps](../logic-apps/logic-apps-custom-hosted-api.md) och [Ny schemaversion 2015-08-01 –  förhandsgranskning](../logic-apps/logic-apps-schema-2015-08-01.md).

Dessutom kan en API-app dra nytta av funktioner som erbjuds av [webbappar](../app-service-web/app-service-web-overview.md) och [Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md). hello omvänd gäller även: Om du använder en webbapp eller mobilapp toohost en API som den kan dra nytta av API Apps funktioner, till exempel Swagger-metadata för klientkodsgenerering och CORS för åtkomst mellan domänwebbläsare. hello endast skillnaden mellan hello tre apptyper (API, webb, mobil) är hello namnet och ikonen som används för dem i hello Azure-portalen.

## <a name="whats-hello-difference-between-api-apps-and-azure-api-management"></a>Vad är hello skillnaden mellan API Apps och Azure API Management?
API Apps och [Azure API Management](../api-management/api-management-key-concepts.md) är kompletterande tjänster:

* API Management handlar om att hantera API:er. Du placera en API Management-klientdel på en API-toomonitor och begränsa användningen, ändra indata och utdata, konsolidera flera API: er till en slutpunkt och så vidare. hello API: er som hanteras kan finnas var som helst.
* API Apps handlar om att vara värd för API:er. hello-tjänsten innehåller funktioner som underlättar utveckling och användning API: er, men den gör inte hello typer av övervakning, begränsning, manipulera eller konsolidering som API Management gör. Om du inte behöver API Management-funktioner kan du vara värd för API:er i API Apps utan att använda API Management.

Här är ett diagram som visar hur API Management används för API:er som finns i API Apps och på andra platser.

![Azure API Management och API Apps](./media/app-service-api-apps-why-best-platform/apia-apim.png)

Vissa funktioner i API Management och API Apps liknar varandra.  Exempelvis kan båda automatisera CORS-support. När du använder hello två tjänsterna tillsammans använder du API Management för CORS eftersom den fungerar som hello klientdelen tooyour API apps. 

## <a name="getting-started"></a>Komma igång
tooget igång med API Apps genom att distribuera exempel kod tooone finns hello självstudierna för det ramverk du föredrar:

* [ASP.NET](app-service-api-dotnet-get-started.md) 
* [Node.js](app-service-api-nodejs-api-app.md) 
* [Java](app-service-api-java-api-app.md) 

tooask frågor om API apps, starta en tråd hello [forumet för API Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureAPIApps). 

