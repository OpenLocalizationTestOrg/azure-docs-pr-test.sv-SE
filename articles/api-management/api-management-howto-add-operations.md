---
title: aaaHow tooadd operations tooan API i Azure API Management | Microsoft Docs
description: "Lär dig hur tooadd operations tooan API i Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 1158a023-1913-4e9c-93de-9164b672f9b3
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: d57fa59a2b0ceb392cde23150a0cbb326e52d27d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooadd-operations-tooan-api-in-azure-api-management"></a>Hur tooadd operations tooan API i Azure API Management
Innan en API i API Management kan användas, måste du lägga till åtgärder. Detta hjälper visar hur tooadd och konfigurera olika typer av åtgärder tooan API i API-hantering.

## <a name="add-operation"></a>Lägga till en åtgärd
Åtgärder har lagts till och konfigurerats tooan API i hello publisher portal. tooaccess hello publisher klickar du på **Publisher portal** i hello Azure-portalen för API Management-tjänsten.

![Utgivarportalen][api-management-management-console]

> Om du inte har skapat en instans för API Management-tjänsten finns [skapa en instans för API Management-tjänsten] [ Create an API Management service instance] i hello [Kom igång med Azure API Management] [ Get started with Azure API Management] kursen.
> 
> 

Välj hello önskade API i hello publisher portal och välj sedan hello **Operations** fliken. 

![Åtgärder][api-management-operations]

Klicka på **lägga till åtgärden** tooadd en ny operation. Hej **nya åtgärden** visas och hello **signatur** fliken väljs som standard.

![Lägg till åtgärd][api-management-add-operation]

Ange hello **HTTP-verbet** genom att välja från hello nedrullningsbara listan.

![HTTP-metoden][api-management-http-method]

<a name="url-template"></a>

Definiera hello URL: en mall genom att skriva in ett URL-fragment som består av en eller flera URL-sökvägssegment och noll eller flera sträng Frågeparametrar. hello URL: en mall, tillagda toohello bas-URL för hello API, identifierar en enskild HTTP-åtgärd. Det kan innehålla en eller flera namngivna variabeln delar som identifieras av klammerparenteser. Dessa rörliga delar kallas mallparametrar och tilldelas dynamiskt värden som extraheras från hello URL: en när hello begäran behandlas av hello API Management-plattformen.

> hello URL: en mall kan innehålla mönster med jokertecken. Ange till exempel `/*` vidarebefordra alla begäranden om att HTTP-metoden toohello tillbaka avslutas tjänsten.

![URL: en mall][api-management-url-template]

<a name="rewrite-url-template"></a>

Om du vill ange hello **skriva om URL: en mall**. Detta gör att du toouse hello URL: en standardmall för bearbetning av inkommande begäranden på hello frontend, när du anropar hello serverdel via en konverterade URL enligt toohello omskrivning av mallen. Mallparametrar från hello URL mallen ska användas i hello omarbetning mallen. hello visas följande exempel hur content-type kodad som sökvägssegmentet i hello webbtjänst från hello föregående exempel kan anges som en frågeparameter i hello API som publicerats via hello API-plattform med hjälp av mallar för hello-URL.

![URL: en mall för omarbetning][api-management-url-template-rewrite]

Anropare toohello åtgärden använder hello formatet `/customers?customerid=ALFKI` och detta kommer att mappas för`/Customers('ALFKI')` när hello backend-tjänst har anropats.

**Visa** namn och **beskrivning** ger en beskrivning av åtgärden hello och är används tooprovide dokumentationen toohello utvecklare som använder detta API i hello developer-portalen.

![Beskrivning][api-management-description]

hello åtgärden beskrivning kan anges som oformaterad text eller HTML i hello **beskrivning** textruta.

## <a name="operation-caching"></a>Åtgärden cachelagring
Svaret cachelagring minskar svarstider uppfattas av hello API konsumenter, driftmiljön bandbreddsanvändning och minskar hello belastningen på hello HTTP web service implementera hello API. 

tooeasily och snabbt aktivera cachelagring för hello åtgärden väljer hello **cachelagring** och kontrollerar hello **aktivera** kryssrutan.

![Cachelagring][api-management-caching-tab]

**Varaktighet** anger hello tidsperiod under vilken hello åtgärden svar finns kvar i hello cache. hello standardvärdet är 3 600 sekunder eller 1 timme.

Cache-nycklar är används toodifferentiate mellan svar så att hello svaret motsvarande tooeach olika cachenyckel får sitt eget separat cachelagrade värde. Du kan också ange särskilda frågeparametrar för sträng och/eller HTTP-huvuden toobe använts vid beräkning av cache nyckelvärdena i hello **variera inom strängen frågeparametrar** och **variera av huvuden** textrutor respektive. När ingen är angivna, fullständig URL: en och hello följande värden i HTTP-huvudet som används i cache-nyckelgenerering: **acceptera** och **Accept-Charset**.

> Mer information om cachelagring och cachelagring principer finns [hur toocache åtgärden resulterar i Azure API Management][How toocache operation results in Azure API Management].
> 
> 

## <a name="request-parameters"></a>Parametrar för begäran
Åtgärdsparametrar hanteras på fliken för hello-parametrar. Parametrar som anges i hello **URL mallen** på hello **signatur** läggs till automatiskt och kan ändras endast genom att redigera mallen för hello-URL. Ytterligare parametrar kan anges manuellt.

tooadd en ny frågeparameter klickar du på **lägga till frågeparameter** och ange hello följande information:

* **Namnet** -parameternamnet.
* **Beskrivning** -en kort beskrivning av hello-parametern (valfritt).
* **Typen** -parametertypen som valts i hello listrutan.
* **Värden** -värden som kan tilldelas toothis-parametern. Ett av hello värden kan anges som standard (valfritt).
* **Krävs** -göra hello parametern obligatorisk genom att kontrollera hello kryssrutan. 

![Parametrar för begäran][api-management-request-parameters]

## <a name="request-body"></a>Begärandetext
Om hello åtgärden tillåter (t.ex. PUT, POST) och kräver en text som du kan ge ett exempel på det till alla hello stöds representation format (t.ex. json, XML). 

> Hej begärandetexten används endast för dokumentation och har inte verifierats.
> 
> 

tooenter en begärantext växla toohello **brödtext** fliken.

Klicka på **lägga till Representation**börjar skriva önskad content-type-namn (t.ex. program/json), markerar du den i hello listrutan och klistra in hello önskad begäran brödtext exempel hello valda format i hello textruta. 

![Begärandetexten][api-management-request-body]

I ytterligare toorepresentations kan du också ange en valfri beskrivning i hello **beskrivning** textruta.

## <a name="responses"></a>Svar
Det är en bra idé tooprovide exempel på svar för alla statuskoder som kan generera hello igen. Varje statuskod kan ha fler än ett svar brödtext exempel, en för varje hello stöds innehållstyper. 

tooadd ett svar, klickar du på **Lägg till** och börja skriva hello önskad statuskod. I det här exemplet hello status koden är **200 OK**. När hello kod visas i hello listrutan, markerar du det och hello svarskoden är skapas och tillagda tooyour igen.

![Svarskod][api-management-response-code]

Klicka på **lägga till Representation**, börja skriva hello önskade content-type-namn (t.ex. program/json) och välj sedan den i hello listrutan.

![Brödtext innehållstyp][api-management-response-body-content-type]

Klistra in hello svar brödtext exempel hello valda format i hello textruta. 

![Svarstexten][api-management-response-body]

Om du vill lägga till en valfri beskrivning i hello **beskrivning** textruta.

När hello åtgärd har konfigurerats klickar du på **spara**.

## <a name="next-steps"> </a>Nästa steg
När hello operations läggs tooan API, hello nästa steg tooassociate hello API med en produkt och publicera den så att utvecklare kan anropa åtgärderna.

* [Hur toocreate och publicera en produkt][How toocreate and publish a product]

[api-management-management-console]: ./media/api-management-howto-add-operations/api-management-management-console.png
[api-management-operations]: ./media/api-management-howto-add-operations/api-management-operations.png
[api-management-add-operation]: ./media/api-management-howto-add-operations/api-management-add-operation.png
[api-management-http-method]: ./media/api-management-howto-add-operations/api-management-http-method.png
[api-management-url-template]: ./media/api-management-howto-add-operations/api-management-url-template.png
[api-management-url-template-rewrite]: ./media/api-management-howto-add-operations/api-management-url-template-rewrite.png
[api-management-description]: ./media/api-management-howto-add-operations/api-management-description.png
[api-management-caching-tab]: ./media/api-management-howto-add-operations/api-management-caching-tab.png
[api-management-request-parameters]: ./media/api-management-howto-add-operations/api-management-request-parameters.png
[api-management-request-body]: ./media/api-management-howto-add-operations/api-management-request-body.png
[api-management-response-code]: ./media/api-management-howto-add-operations/api-management-response-code.png
[api-management-response-body-content-type]: ./media/api-management-howto-add-operations/api-management-response-body-content-type.png
[api-management-response-body]: ./media/api-management-howto-add-operations/api-management-response-body.png


[api-management-contoso-api]: ./media/api-management-howto-add-operations/api-management-contoso-api.png

[api-management-add-new-api]: ./media/api-management-howto-add-operations/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-add-operations/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-add-operations/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-add-operations/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-add-operations/api-management-echo-operations.png

[Add an operation]: #add-operation
[Operation caching]: #operation-caching
[Request parameters]: #request-parameters
[Request body]: #request-body
[Responses]: #responses
[Next steps]: #next-steps

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How toocreate and publish a product]: api-management-howto-add-products.md
[How toocache operation results in Azure API Management]: api-management-howto-cache.md
