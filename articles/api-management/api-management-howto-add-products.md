---
title: aaaHow toocreate och publicera en produkt i Azure API Management
description: "Lär dig hur toocreate och publicera produkter i Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 31de55cb-9384-490b-a2f2-6dfcf83da764
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: f0a37f08b4e29ca68be9caec4c7604e3b4b6aaa6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-and-publish-a-product-in-azure-api-management"></a>Hur toocreate och publicera en produkt i Azure API Management
En produkt innehåller en eller flera API: er samt användning kvoten och hello användningsvillkor i Azure API Management. När en produkt har publicerats kan utvecklare prenumerera toohello produkt och börja toouse hello produkten API: er. hello avsnittet innehåller en guide toocreating en produkt, lägga till en API och publicera för utvecklare.

## <a name="create-product"></a>Skapar en produkt
Åtgärder har lagts till och konfigurerats tooan API i hello publisher portal. tooaccess hello publisher klickar du på **Publisher portal** i hello Azure-portalen för API Management-tjänsten.

![Utgivarportalen][api-management-management-console]

> Om du inte har skapat en instans för API Management-tjänsten finns [skapa en instans för API Management-tjänsten] [ Create an API Management service instance] i hello [Kom igång med Azure API Management] [ Get started with Azure API Management] kursen.
> 
> 

Klicka på **produkter** hello menyn hello vänstra toodisplay hello **produkter** och klicka på **lägga till produkten**.

![Produkter][api-management-products]

![Ny produkt][api-management-add-new-product]

Ange ett beskrivande namn för hello produkt i hello **namn** fältet och en beskrivning av hello produkt i hello **beskrivning** fältet.

Produkter i API-hantering kan vara **öppna** eller **skyddade**. Skyddade produkter måste vara prenumererade toobefore som de kan användas, öppen produkter kan användas utan en prenumeration. Kontrollera **kräver prenumeration** toocreate en skyddad produkt som kräver en prenumeration. Det här är standardinställningen för hello.

Kontrollera **kräver godkännande för prenumerationen** om du vill att en administratör tooreview och godkänna eller avvisa prenumeration försöker toothis produkten. Om hello rutan är avmarkerad att prenumeration försök automatiskt godkänd. Mer information om prenumerationer finns [visa prenumeranter tooa produkt][View subscribers tooa product].

tooallow developer konton toosubscribe flera gånger toohello produkt, kontrollera hello **tillåter flera prenumerationer** kryssrutan. Om inte den här kryssrutan är markerad, kan varje utvecklarkonto prenumerera på bara en gång toohello produkt.

![Obegränsade flera prenumerationer][api-management-unlimited-multiple-subscriptions]

toolimit hello antal flera samtidiga prenumerationer, kontrollera hello **begränsa antalet samtidiga prenumerationer** kryssrutan och ange hello gränsen. I följande exempel hello, är samtidiga prenumerationer begränsad toofour per utvecklarkonto.

![Fyra flera prenumerationer][api-management-four-multiple-subscriptions]

När alla nya produktalternativ för har konfigurerats, klickar du på **spara** toocreate hello ny produkt.

![Produkter][api-management-products-page]

> Som standard nya produkter är opublicerad och är synliga endast toohello **administratörer** grupp.
> 
> 

tooconfigure en produkt, klicka på hello Produktnamn i hello **produkter** fliken.

## <a name="add-apis"></a>Lägg till API: er tooa produkten
Hej **produkter** sidan innehåller fyra länkar för konfiguration: **sammanfattning**, **inställningar**, **synlighet**, och  **Prenumeranter**. Hej **sammanfattning** är där du kan lägga till API: er och publicera eller avpublicera en produkt.

![Sammanfattning][api-management-new-product-summary]

Innan du publicerar produkten måste tooadd en eller flera API: er. toodo, genom att välja **lägga till API tooproduct**.

![Lägg till API: er][api-management-add-apis-to-product]

Välj hello önskad API: er och klicka på **spara**.

## <a name="add-description"></a>Lägg till beskrivande information tooa produkten
Hej **inställningar** fliken kan du tooprovide detaljerad information om hello produkten som sitt syfte, hello API: er som den ger åtkomst till och annan användbar information. hello innehåll är riktad mot hello utvecklare som kommer att ringa upp hello API och kan skrivas i oformaterad text eller HTML-kod.

![Produktinställningar för][api-management-product-settings]

Kontrollera **kräver prenumeration** toocreate en skyddad produkt som kräver en prenumeration toobe används, eller avmarkera hello kryssrutan toocreate en öppen produkt som kan anropas utan en prenumeration.

Välj **kräver godkännande för prenumerationen** om du vill toomanually godkänna alla produkten prenumerationsbegäranden. Som standard beviljas alla produktprenumeration automatiskt.

tooallow developer konton toosubscribe flera gånger toohello produkt, kontrollera hello **tillåter flera prenumerationer** kryssrutan och om du vill ange en gräns. Om inte den här kryssrutan är markerad, kan varje utvecklarkonto prenumerera på bara en gång toohello produkt.

Om du vill fylla i hello **användningsvillkoren** fält som beskriver hello villkor för användning av hello produkt som prenumeranter måste acceptera i ordning toouse hello produkten.

## <a name="publish-product"></a>Publicera en produkt
Hello produkten måste publiceras innan hello API: er i en produkt kan anropas. På hello **sammanfattning** för hello produkten klickar du på **publicera**, och klicka sedan på **Ja, publicera den** tooconfirm. toomake en tidigare publicerade produkten privat klickar du på **Upphäv publicering**.

![Publicera produkten][api-management-publish-product]

## <a name="make-visible"></a>Gör en synlig toodevelopers produkten
Hej **synlighet** fliken kan du toochoose vilka roller är kan toosee hello produkten på hello developer-portalen och prenumerera toohello produkten.

![Produkten synlighet][api-management-product-visiblity]

tooenable eller inaktivera synligheten för en produkt för hello utvecklare i en grupp Markera eller avmarkera hello kryssrutan bredvid hello gruppen och klicka sedan på **spara**.

> Mer information finns i [hur toocreate och Använd grupper toomanage developer konton i Azure API Management][How toocreate and use groups toomanage developer accounts in Azure API Management].
> 
> 

## <a name="view-subscribers"></a>Visa prenumeranter tooa produkt
Hej **prenumeranter** fliken listar hello utvecklare som prenumererar toohello produkten. hello kan information och inställningar för varje utvecklare visas genom att klicka på hello developer namn. I det här exemplet har inga utvecklare ännu prenumererat toohello produkten.

![Utvecklare][api-management-developer-list]

## <a name="next-steps"> </a>Nästa steg
När önskade hello API: er och hello produkten publicerade utvecklare kan prenumerera toohello produkt och börja toocall hello API: er. En genomgång som visar dessa objekt samt avancerade produktkonfigurationen finns [hur skapar och konfigurerar produktinställningar för avancerad i Azure API Management][How create and configure advanced product settings in Azure API Management].

Mer information om hur du arbetar med produkter finns i följande video hello.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Using-Products/player]
> 
> 

[Create a product]: #create-product
[Add APIs tooa product]: #add-apis
[Add descriptive information tooa product]: #add-description
[Publish a product]: #publish-product
[Make a product visible toodevelopers]: #make-visible
[View subscribers tooa product]: #view-subscribers
[Next steps]: #next-steps

[api-management-management-console]: ./media/api-management-howto-add-products/api-management-management-console.png
[api-management-add-product]: ./media/api-management-howto-add-products/api-management-add-product.png
[api-management-add-new-product]: ./media/api-management-howto-add-products/api-management-add-new-product.png
[api-management-unlimited-multiple-subscriptions]: ./media/api-management-howto-add-products/api-management-unlimited-multiple-subscriptions.png
[api-management-four-multiple-subscriptions]: ./media/api-management-howto-add-products/api-management-four-multiple-subscriptions.png
[api-management-products-page]: ./media/api-management-howto-add-products/api-management-products-page.png
[api-management-new-product-summary]: ./media/api-management-howto-add-products/api-management-new-product-summary.png
[api-management-add-apis-to-product]: ./media/api-management-howto-add-products/api-management-add-apis-to-product.png
[api-management-product-settings]: ./media/api-management-howto-add-products/api-management-product-settings.png
[api-management-publish-product]: ./media/api-management-howto-add-products/api-management-publish-product.png
[api-management-product-visiblity]: ./media/api-management-howto-add-products/api-management-product-visibility.png
[api-management-developer-list]: ./media/api-management-howto-add-products/api-management-developer-list.png



[api-management-products]: ./media/api-management-howto-add-products/api-management-products.png
[api-management-]: ./media/api-management-howto-add-products/
[api-management-]: ./media/api-management-howto-add-products/


[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How toocreate and publish a product]: api-management-howto-add-products.md
[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Next steps]: #next-steps
[How toocreate and use groups toomanage developer accounts in Azure API Management]: api-management-howto-create-groups.md
[How create and configure advanced product settings in Azure API Management]: api-management-howto-product-with-rules.md 
