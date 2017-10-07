---
title: aaaAdd cachelagring tooimprove prestanda i Azure API Management | Microsoft Docs
description: "Lär dig hur tooimprove hello svarstid, bandbreddsanvändning och webbtjänsten ladda för hantering av API-anrop."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 740f6a27-8323-474d-ade2-828ae0c75e7a
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 056ab7cf788218327e30bd5c028b76e3b1977fb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-caching-tooimprove-performance-in-azure-api-management"></a>Lägg till cachelagring tooimprove prestanda i Azure API Management
Du kan konfigurera åtgärder i API Management för cachelagring av svar. Cachelagring av svar kan avsevärt minska API:ets svarstider, bandbreddsanvändning och webbtjänstbelastning när data inte ändras så ofta.

Den här guiden visar hur tooadd svar cachelagring för din API och konfigurera principer för hello exempel Echo-API: et. Sedan kan du anropa hello åtgärden från hello developer portal tooverify cachelagring i åtgärden.

> [!NOTE]
> Mer information om hur du cachelagrar objekt med nycklar med hjälp av principuttryck finns i [Anpassad cachelagring i Azure API Management](api-management-sample-cache-by-key.md).
> 
> 

## <a name="prerequisites"></a>Krav
Innan följande hello stegen i den här guiden, måste du ha en API Management service-instans med API och en produkt som har konfigurerats. Om du inte har skapat en instans för API Management-tjänsten finns [skapa en instans för API Management-tjänsten] [ Create an API Management service instance] i hello [Kom igång med Azure API Management] [ Get started with Azure API Management] kursen.

## <a name="configure-caching"> </a>Konfigurera en åtgärd för cachelagring
I det här steget ska du granska hello cacheinställningar av hello **hämta resurs (Zeroed)** åtgärden hello exemplet Echo-API.

> [!NOTE]
> Varje instans för API Management-tjänsten kommer förkonfigurerade med en Echo-API som kan använda tooexperiment med och lär dig mer om API-hantering. Mer information finns i [Komma igång med Azure API Management][Get started with Azure API Management].
> 
> 

tooget har startats klickar du på **Publisher portal** i hello Azure-portalen för API Management-tjänsten. Då kommer du toohello API Management publisher portal.

![Utgivarportalen][api-management-management-console]

Klicka på **API: er** från hello **API Management** menyn på hello vänster och klicka sedan på **Echo API**.

![Echo API][api-management-echo-api]

Klicka på hello **Operations** fliken och klicka sedan på hello **hämta resurs (Zeroed)** åtgärden från hello **Operations** lista.

![Echo API-åtgärder][api-management-echo-api-operations]

Klicka på hello **cachelagring** fliken tooview hello cache-inställningar för den här åtgärden.

![Fliken Cachelagring][api-management-caching-tab]

tooenable cachelagring för en åtgärd, Välj hello **aktivera** kryssrutan. I det här exemplet är cachelagring aktiverat.

Anpassade varje åtgärden svar, baserat på hello värdena i hello **variera inom strängen frågeparametrar** och **variera av huvuden** fält. Om du vill toocache som flera svar baserat på frågan string-parametrar eller huvuden kan konfigurera du dem i dessa två fält.

**Varaktighet** anger hello giltighetstid tidsintervall hello cachelagrat svar. I det här exemplet hello är **3600** sekunder, vilket är likvärdiga tooone timme.

Med hello cachelagring konfigurationen i det här exemplet hello första begäran toohello **hämta resurs (Zeroed)** åtgärd returnerar ett svar från hello backend-tjänsten. Svaret ska cachelagras Nyckelbaserade av hello angetts sidhuvuden och fråga strängparametrar. Efterföljande anrop toohello åtgärden med matchande parametrar, har hello cachelagrade svar som returneras till hello cache varaktighet intervall har gått ut.

## <a name="caching-policies"></a>Granska hello cachelagring principer
I det här steget kan du granska hello cacheinställningar för hello **hämta resurs (Zeroed)** åtgärden hello exemplet Echo-API.

När inställningar för cachelagring har konfigurerats för en åtgärd på hello **cachelagring** fliken Cachelagring principer har lagts till för hello igen. Dessa principer kan visas och ändras i hello redigeraren.

Klicka på **principer** från hello **API Management** menyn på hello vänster och välj sedan **Echo API / hämta resurs (Zeroed)** från hello **åtgärden**listrutan.

![Åtgärden Principomfattning][api-management-operation-dropdown]

Redigeraren för hello visas hello principer för den här åtgärden.

![API Management-principredigeraren][api-management-policy-editor]

hello principdefinitionen för den här åtgärden innehåller hello principer som definierar hello cachelagring konfiguration som kontrollerades med hello **cachelagring** fliken i hello föregående steg.

```xml
<policies>
    <inbound>
        <base />
        <cache-lookup vary-by-developer="false" vary-by-developer-groups="false">
            <vary-by-header>Accept</vary-by-header>
            <vary-by-header>Accept-Charset</vary-by-header>
        </cache-lookup>
        <rewrite-uri template="/resource" />
    </inbound>
    <outbound>
        <base />
        <cache-store caching-mode="cache-on" duration="3600" />
    </outbound>
</policies>
```

> [!NOTE]
> Ändringar som gjorts toohello cachelagring principer i Redigeraren för hello används på hello **cachelagring** fliken för en åtgärd och vice versa.
> 
> 

## <a name="test-operation"></a>Anropa en åtgärd och testa hello cachelagring
toosee hello cachelagring i praktiken vi kallar hello åtgärden från hello developer-portalen. Klicka på **utvecklarportalen** i hello övre högra menyn.

![Utvecklarportalen][api-management-developer-portal-menu]

Klicka på **API: er** i hello översta menyn och välj sedan **Echo API**.

![Echo API][api-management-apis-echo-api]

> Om du har bara en API som konfigurerats eller synliga tooyour konto, klicka på API: er kommer du direkt toohello åtgärder för att API.
> 
> 

Välj hello **hämta resurs (Zeroed)** igen och klicka sedan på **öppna konsolen**.

![Öppna konsolen][api-management-open-console]

hello-konsolen kan du tooinvoke operations direkt från hello developer-portalen.

![Konsolen][api-management-console]

Behåll hello standardvärden för **param1** och **param2**.

Välj hello önskade nyckeln från hello **prenumeration nyckeln** listrutan. Om ditt konto bara har en prenumeration är den redan vald.

Ange **sampleheader:value1** i hello **begärandehuvuden** textruta.

Klicka på **HTTP Get** och anteckna hello-svarshuvuden.

Ange **sampleheader:value2** i hello **Begäransrubriker** textrutan och klicka sedan på **HTTP Get**.

Observera att hello-värdet för **sampleheader** fortfarande **value1** hello svar. Prova några olika värden och Observera att hello cachelagrade svar från hello första anropet returneras.

Ange **25** till hello **param2** fältet och klickar sedan på **HTTP Get**.

Observera att hello-värdet för **sampleheader** hello svar finns i **value2**. Eftersom hello Åtgärdsresultat baseras frågesträng, returnerades hello föregående cachelagrade svar inte.

## <a name="next-steps"> </a>Nästa steg
* Mer information om cachelagring principer finns [cachelagring principer] [ Caching policies] i hello [principreferens för API Management][API Management policy reference].
* Mer information om hur du cachelagrar objekt med nycklar med hjälp av principuttryck finns i [Anpassad cachelagring i Azure API Management](api-management-sample-cache-by-key.md).

[api-management-management-console]: ./media/api-management-howto-cache/api-management-management-console.png
[api-management-echo-api]: ./media/api-management-howto-cache/api-management-echo-api.png
[api-management-echo-api-operations]: ./media/api-management-howto-cache/api-management-echo-api-operations.png
[api-management-caching-tab]: ./media/api-management-howto-cache/api-management-caching-tab.png
[api-management-operation-dropdown]: ./media/api-management-howto-cache/api-management-operation-dropdown.png
[api-management-policy-editor]: ./media/api-management-howto-cache/api-management-policy-editor.png
[api-management-developer-portal-menu]: ./media/api-management-howto-cache/api-management-developer-portal-menu.png
[api-management-apis-echo-api]: ./media/api-management-howto-cache/api-management-apis-echo-api.png
[api-management-open-console]: ./media/api-management-howto-cache/api-management-open-console.png
[api-management-console]: ./media/api-management-howto-cache/api-management-console.png


[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How tooadd and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs tooa product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: api-management-get-started.md

[API Management policy reference]: https://msdn.microsoft.com/library/azure/dn894081.aspx
[Caching policies]: https://msdn.microsoft.com/library/azure/dn894086.aspx

[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[Configure an operation for caching]: #configure-caching
[Review hello caching policies]: #caching-policies
[Call an operation and test hello caching]: #test-operation
[Next steps]: #next-steps
