---
title: "aaaManage din första API i Azure API Management | Microsoft Docs"
description: "Lär dig hur toocreate API: er, lägga till åtgärder och kom igång med API-hantering."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 51b7df8b-1c43-43c6-90c9-0aa24f48206b
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 7d43f33aa359c4d1e605e9fb41e43d323ca6a777
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-your-first-api-in-azure-api-management"></a>Hantera ditt första API i Azure API Management
## <a name="overview"> </a>Översikt
Den här guiden visar hur tooquickly Kom igång med Azure API Management och göra din första API-anrop.

## <a name="concepts"> </a>Vad är Azure API Management?
Du kan använda Azure API Management tootake alla serverdelar och starta en komplett API-programmet baseras på den.

Här följer exempel på några vanliga scenarier:

* **Skydda den mobila infrastrukturen** genom att hantera åtkomsten med API-nycklar, förhindra DOS-attacker med hjälp av begränsningar och använda avancerade säkerhetsprinciper som JWT-tokenvalidering.
* **Aktivera ISV partner ekosystem** genom att erbjuda snabb partner onboarding via hello developer portal och bygga ett API facade toodecouple från interna implementeringar som inte är mogna för partner förbrukning.
* **Ett internt API-program körs** genom att erbjuda en central plats för hello organisation toocommunicate om hello tillgänglighet och senaste ändras tooAPIs, gating åtkomst baserat på organisationskonton, alla baserat på en skyddad kanal mellan hello API-gateway och hello backend.

hello system består av hello följande komponenter:

* Hej **API gateway** är hello slutpunkt som:
  
  * Accepterar API-anrop och skickar dem tooyour serverdelar.
  * Verifierar API-nycklar, JWT-token, certifikat och andra autentiseringsuppgifter.
  * Tillämpar användningskvoter och hastighetsbegränsningar.
  * Transformerar ditt API på hello direkt utan kod ändringar.
  * Cachelagrar backend-svar om detta konfigurerats.
  * Loggar metadata i anrop för analysändamål.
* Hej **publisher portal** är hello administrationsgränssnittet där du ställa in API-program. Använd portalen om du vill:
  
  * Definiera eller importera API-schemat.
  * Paketera API:er till produkter.
  * Konfigurera principer som kvoter eller omformningar på hello API: er.
  * Få insikter från analyser.
  * Hantera användare.
* Hej **utvecklarportalen** fungerar som hello huvudsakliga webben för utvecklare, där de kan:
  
  * Få tillgång till API-dokumentation.
  * Prova att använda en API via interaktiva hello-konsolen.
  * Skapa ett konto och prenumerera tooget API-nycklar.
  * Komma åt analyser om deras egen användning.

## <a name="create-service-instance"> </a>Skapa en API Management-instans
> [!NOTE]
> toocomplete den här självstudiekursen kommer du behöver ett Azure-konto. Om du inte har något konto kan du skapa ett kostnadsfritt konto på bara några minuter. Mer information finns i [kostnadsfri utvärderingsversion av Azure][Azure Free Trial].
> 
> 

hello första steget i att arbeta med API-hantering är toocreate en instans av tjänsten. Logga in toohello [Azure Portal] [ Azure Portal] och på **ny**, **webb + mobilt**, **API Management**.

![Ny API Management-instans][api-management-create-instance-menu]

För **namn**, ange en unik underordnade domän namnet toouse för hello tjänst-URL.

Välj önskad hello **prenumeration**, **resursgruppen** och **plats** för service-instans.

Ange **Contoso AB** för hello **organisationsnamn**, och ange din e-postadress i hello **administratör e-post** fältet.

> [!NOTE]
> E-postadressen används för meddelanden från hello API Management-systemet. Mer information finns i [hur tooconfigure meddelanden och e-mallar i Azure API Management][How tooconfigure notifications and email templates in Azure API Management].
> 
> 

![Ny API Management-tjänst][api-management-create-instance-step1]

API Management-tjänstinstanser är tillgängliga på tre nivåer: Developer, Standard och Premium.

> [!NOTE]
> hello Developer nivå är för utveckling, testning och API pilotprogram där hög tillgänglighet inte är ett problem. I hello Standard och Premium-nivåer, kan du skala din enhet antal toohandle mer trafik. hello Standard och Premium nivåer ge din API Management-tjänsten med hello de flesta processorkraft och prestanda. Du kan genomföra den här självstudiekursen med valfri nivå. Mer information om API Management-nivåer finns i avsnittet om [API Management-priser][API Management pricing].
> 
> 

Klicka på **skapa** toostart etablering service-instans.

![Ny API Management-tjänst][api-management-instance-created]

När du har skapat hello tjänstinstans hello nästa steg är toocreate eller importerar en API.

## <a name="create-api"> </a>Importera ett API
Ett API består av en uppsättning åtgärder som kan anropas från ett klientprogram. API: et är via proxy tooexisting webbtjänster.

API:er kan skapas (och åtgärder kan läggas till) manuellt eller importeras. I den här självstudiekursen kommer vi importera hello API för en exempel Kalkylatorn-webbtjänst som tillhandahålls av Microsoft och finns på Azure.

> [!NOTE]
> Anvisningar för att skapa en API och manuellt lägga till operations finns [hur toocreate API: er](api-management-howto-create-apis.md) och [hur tooadd operations tooan API](api-management-howto-add-operations.md).
> 
> 

API: er konfigureras från hello publisher-portalen. tooreach, klickar du på **Publisher portal** hello service verktygsfältet.

![Utgivarportalen][api-management-management-console]

tooimport hello Kalkylatorn API, klickar du på **API: er** från hello **API Management** menyn på hello vänster och klicka sedan på **Import API**.

![Knappen Importera API][api-management-import-api]

Utför hello följande steg tooconfigure hello Kalkylatorn API:

1. Klicka på **från URL: en**, ange **http://calcapi.cloudapp.net/calcapi.json** till hello **specifikation dokumentet URL** text och på hello **Swagger**  knappen.
2. Typen **beräkna** till hello **URL för Web API-suffix** textruta.
3. Klicka i hello **produkter (valfritt)** och väljer **Starter**.
4. Klicka på **spara** tooimport hello API.

![Lägga till ett nytt API][api-management-import-new-api]

> [!NOTE]
> **API Management** stöder för närvarande både 1.2- och 2.0-versionen av Swagger-dokument för importändamål. Egenskaperna `host`, `basePath` och `schemes` är valfria enligt [Swagger 2.0-specifikationen](http://swagger.io/specification), men ditt Swagger 2.0-dokument **MÅSTE** innehålla dessa egenskaper för att importeras. 
> 
> 

När du har importerat hello API visas hello sammanfattningssidan hello-API: t i hello publisher-portalen.

![API-sammanfattning][api-management-imported-api-summary]

hello API-avsnitt har flera flikar. Hej **sammanfattning** fliken visas information om hello API och grundläggande mått. Hej [inställningar](api-management-howto-create-apis.md#configure-api-settings) fliken har konfigurerats att använda tooview och redigera hello API. Hej [Operations](api-management-howto-add-operations.md) är operations används toomanage hello API: er. Hej **säkerhet** flik kan vara används tooconfigure gateway-autentisering för hello backend-servern med hjälp av grundläggande autentisering eller [ömsesidig autentisering](api-management-howto-mutual-certificates.md), och tooconfigure [ Auktoriseringen av användaren med hjälp av OAuth 2.0](api-management-howto-oauth2.md).  Hej **problem** är används tooview problem som rapporteras av hello utvecklare som använder dina API: er. Hej **produkter** är används tooconfigure hello produkter som innehåller detta API.

Som standard medföljer två exempelprodukter varje API Management-instans:

* **Starter**
* **Obegränsat**

I den här självstudiekursen lades hello grundläggande Kalkylatorn API toohello Starter produkten när hello API importerades.

Utvecklare måste först prenumerera tooa produkt som ger dem åtkomst tooit i ordning toomake anrop tooan API. Utvecklare kan prenumerera på tooproducts i hello developer-portalen eller administratörer kan prenumerera på utvecklare tooproducts hello publisher-portalen. Du är administratör sedan du skapade hello API Management-instans i hello föregående steg i självstudiekursen hello så att du redan prenumererar på tooevery produkten som standard.

## <a name="call-operation"></a>Anropa en åtgärd från hello developer-portalen
Åtgärder kan anropas direkt från hello developer-portalen, vilket ger ett bekvämt sätt tooview och testa hello driften av ett API. I den här självstudiekursen steg ska du anropa hello grundläggande Kalkylatorn API: er **lägga till två heltal** igen. Klicka på **Developer-portalen** hello-menyn på hello övre högra hello publisher portalen.

![Utvecklarportalen][api-management-developer-portal-menu]

Klicka på **API: er** hello översta menyn och klicka sedan på **grundläggande Kalkylatorn** toosee hello tillgängliga åtgärder.

![Utvecklarportalen][api-management-developer-portal-calc-api]

Observera hello exempel beskrivningar och parametrarna som har importerats tillsammans med hello API och åtgärder som att tillhandahålla dokumentation för hello utvecklare som använder den här åtgärden. Dessa beskrivningar kan även läggas till om åtgärder läggs till manuellt.

toocall hello **lägga till två heltal** åtgärden, klickar du på **prova**.

![Prova][api-management-developer-portal-calc-api-console]

Du kan ange vissa värden för hello parametrar eller hålla hello standardvärden och klicka sedan på **skicka**.

![HTTP Get][api-management-invoke-get]

När en åtgärd har anropats hello developer-portalen visar hello **svarsstatusen**, hello **svarshuvuden**, och en **svar innehåll**.

![Svar][api-management-invoke-get-response]

## <a name="view-analytics"> </a>Visa analys
tooview analytics för grundläggande Kalkylatorn växla tillbaka toohello publisher portal genom att välja **hantera** hello-menyn på hello uppifrån höger om hello developer-portalen.

![Hantera][api-management-manage-menu]

hello standardvyn för hello publisher portal är hello **instrumentpanelen**, vilket ger en översikt över API Management-instans.

![Instrumentpanel][api-management-dashboard]

Hovra hello muspekaren över hello diagram för **grundläggande Kalkylatorn** toosee hello specifika mått för hello användning av hello API för en viss tidsperiod.

> [!NOTE]
> Om du inte ser några rader i diagrammet, växla tillbaka toohello developer-portalen och vissa anrop till hello API, vänta en stund och sedan gå tillbaka toohello instrumentpanelen.
> 
> 

Klicka på **visa information** tooview hello sammanfattningssidan för hello API, inklusive en större version av hello visas mått.

![Analys][api-management-mouse-over]

![Sammanfattning][api-management-api-summary-metrics]

För detaljerad mätvärden och rapporter, klickar du på **Analytics** från hello **API Management** menyn hello vänster.

![Översikt][api-management-analytics-overview]

Hej **Analytics** avsnitt har hello följande fyra flikar:

* **En överblick över** ger totala användningen och hälsotillstånd mått samt hello översta utvecklare, produkter, övre API: er och översta åtgärder.
* **Användning** innehåller detaljerad information om API-anrop och bandbredd, inklusive en geografisk representation.
* **Hälsa** fokuserar på statuskoder, lyckade cachelagringsåtgärder, svarstider och API- och tjänstsvarstider.
* **Aktiviteten** innehåller rapporter som detaljnivån om hello specifik aktivitet efter utvecklare, produkt, API och åtgärden.

## <a name="next-steps"> </a>Nästa steg
* Lär dig hur för[skydda din API med hastighetsbegränsningar](api-management-howto-product-with-rules.md).

[Azure Free Trial]: http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=api_management_hero_a

[Create an API Management instance]: #create-service-instance
[Create an API]: #create-api
[Add an operation]: #add-operation
[Add hello new API tooa product]: #add-api-to-product
[Subscribe toohello product that contains hello API]: #subscribe
[Call an operation from hello Developer Portal]: #call-operation
[View analytics]: #view-analytics
[Next steps]: #next-steps


[How toomanage developer accounts in Azure API Management]: api-management-howto-create-or-invite-developers.md
[Configure API settings]: api-management-howto-create-apis.md#configure-api-settings
[How tooconfigure notifications and email templates in Azure API Management]: api-management-howto-configure-notifications.md
[Responses]: api-management-howto-add-operations.md#responses
[How create and publish a product]: api-management-howto-add-products.md
[API Management pricing]: http://azure.microsoft.com/pricing/details/api-management/

[Azure Portal]: https://portal.azure.com/

[api-management-management-console]: ./media/api-management-get-started/api-management-management-console.png
[api-management-create-instance-menu]: ./media/api-management-get-started/api-management-create-instance-menu.png
[api-management-create-instance-step1]: ./media/api-management-get-started/api-management-create-instance-step1.png
[api-management-create-instance-step2]: ./media/api-management-get-started/api-management-create-instance-step2.png
[api-management-instance-created]: ./media/api-management-get-started/api-management-instance-created.png
[api-management-import-api]: ./media/api-management-get-started/api-management-import-api.png
[api-management-import-new-api]: ./media/api-management-get-started/api-management-import-new-api.png
[api-management-imported-api-summary]: ./media/api-management-get-started/api-management-imported-api-summary.png
[api-management-calc-operations]: ./media/api-management-get-started/api-management-calc-operations.png
[api-management-list-products]: ./media/api-management-get-started/api-management-list-products.png
[api-management-add-api-to-product]: ./media/api-management-get-started/api-management-add-api-to-product.png
[api-management-add-myechoapi-to-product]: ./media/api-management-get-started/api-management-add-myechoapi-to-product.png
[api-management-api-added-to-product]: ./media/api-management-get-started/api-management-api-added-to-product.png
[api-management-developers]: ./media/api-management-get-started/api-management-developers.png
[api-management-add-subscription]: ./media/api-management-get-started/api-management-add-subscription.png
[api-management-add-subscription-window]: ./media/api-management-get-started/api-management-add-subscription-window.png
[api-management-subscription-added]: ./media/api-management-get-started/api-management-subscription-added.png
[api-management-developer-portal-menu]: ./media/api-management-get-started/api-management-developer-portal-menu.png
[api-management-developer-portal-calc-api]: ./media/api-management-get-started/api-management-developer-portal-calc-api.png
[api-management-developer-portal-calc-api-console]: ./media/api-management-get-started/api-management-developer-portal-calc-api-console.png
[api-management-invoke-get]: ./media/api-management-get-started/api-management-invoke-get.png
[api-management-invoke-get-response]: ./media/api-management-get-started/api-management-invoke-get-response.png
[api-management-manage-menu]: ./media/api-management-get-started/api-management-manage-menu.png
[api-management-dashboard]: ./media/api-management-get-started/api-management-dashboard.png

[api-management-add-response]: ./media/api-management-get-started/api-management-add-response.png
[api-management-add-response-window]: ./media/api-management-get-started/api-management-add-response-window.png
[api-management-developer-key]: ./media/api-management-get-started/api-management-developer-key.png
[api-management-mouse-over]: ./media/api-management-get-started/api-management-mouse-over.png
[api-management-api-summary-metrics]: ./media/api-management-get-started/api-management-api-summary-metrics.png
[api-management-analytics-overview]: ./media/api-management-get-started/api-management-analytics-overview.png
[api-management-analytics-usage]: ./media/api-management-get-started/api-management-analytics-usage.png
[api-management-]: ./media/api-management-get-started/api-management-.png
[api-management-]: ./media/api-management-get-started/api-management-.png
