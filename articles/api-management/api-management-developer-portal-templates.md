---
title: "Anpassa API Management developer-portalen med hjälp av mallar-Azure | Microsoft Docs"
description: "Lär dig hur du anpassar Azure API Management developer-portalen med hjälp av mallar."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: a195675b-f7d0-4fc9-90bf-860e6f17ccf7
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 8a2211e76150a90e4e10d79fd527decd3cbcc220
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/16/2017
---
# <a name="how-to-customize-the-azure-api-management-developer-portal-using-templates"></a>Hur du anpassar Azure API Management developer-portalen med hjälp av mallar

Det finns tre grundläggande metoder för att anpassa utvecklarportalen i Azure API Management:

* [Redigera innehållet på statiska sidor och sidlayoutelement][modify-content-layout]
* [Uppdatera formaten som används för sidelement i utvecklingsportalen][customize-styles]
* [Ändra mallarna som används för sidor som genereras av portalen] [ portal-templates] (beskrivs i den här guiden)

Mallar för att anpassa innehållet i systemgenererade developer portalens sidor (t.ex. API-dokumentation, produkter, autentisering av användare, etc.). Med hjälp av [DotLiquid](http://dotliquidmarkup.org/) syntax och en angiven uppsättning lokaliserad strängresurser, ikoner och kontroller, du har stor flexibilitet för att konfigurera innehåll för sidorna som du vill.

## <a name="developer-portal-templates-overview"></a>Översikt över Developer-portalen mallar
Redigera mallar görs från den **utvecklarportalen** när du är inloggad som administratör. Att hämta det först öppna Azure Portal och klicka på **Publisher portal** från verktygsfältet service för din API Management-instans.

![Utgivarportalen][api-management-management-console]

Klicka sedan på **Utgivarportal** överst till höger. 

![Developer portal meny][api-management-developer-portal-menu]

Klicka på ikonen Anpassa till vänster för att visa menyn anpassning och klicka på för att komma åt developer portal mallar **mallar**.

![Developer portal mallar][api-management-customize-menu]

Lista över mallar visas flera kategorier av mallar som omfattar olika sidor i developer-portalen. Varje mall skiljer sig, men stegen för att redigera dem och publicera ändringarna är samma. Redigera en mall, klicka på namnet på mallen.

![Developer portal mallar][api-management-templates-menu]

Klicka på en mall går du till utvecklare portalsidan som kan anpassas av mallen. I det här exemplet i **produktlista** mallen visas. Den **produktlista** mallen styr området på skärmen visas med röd rektangel. 

![Produkter mall][api-management-developer-portal-templates-overview]

Vissa mallar, som den **användarprofil** mallar, anpassa olika delar av samma sida. 

![Användaren profilmallar][api-management-user-profile-templates]

Redigeraren för varje developer portal mall har två avsnitt visas längst ned på sidan. Den vänstra sidan visar redigeringsfönstret för mallen och till höger visar datamodellen för mallen. 

Mallredigeringsläge fönstret innehåller koden som styr utseendet och beteendet för respektive sida i developer-portalen. Koden i mallen använder den [DotLiquid](http://dotliquidmarkup.org/) syntax. Är en populär Redigeraren för DotLiquid [DotLiquid för Designers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers). Alla ändringar av mallen under redigering visas i realtid i webbläsaren, men är inte synlig för kunderna förrän du [spara](#to-save-a-template) och [publicera](#to-publish-a-template) mallen.

![Mallen markering][api-management-template]

Den **malldata** innehåller en guide till datamodellen för de enheter som är tillgängliga för användning i en viss mall. Det ger den här guiden genom att visa den aktuella data som visas för närvarande i developer-portalen. Du kan expandera mallen fönster genom att klicka på rektangel i det övre högra hörnet på de **malldata** fönstret.

![Mall-datamodell][api-management-template-data]

I föregående exempel finns två produkter som visas i developer-portalen och som har hämtats från data som visas i den **malldata** fönstret som visas i följande exempel.

```json
{
    "Paging": {
        "Page": 1,
        "PageSize": 10,
        "TotalItemCount": 2,
        "ShowAll": false,
        "PageCount": 1
    },
    "Filtering": {
        "Pattern": null,
        "Placeholder": "Search products"
    },
    "Products": [
        {
            "Id": "56ec64c380ed850042060001",
            "Title": "Starter",
            "Description": "Subscribers will be able to run 5 calls/minute up to a maximum of 100 calls/week.",
            "Terms": "",
            "ProductState": 1,
            "AllowMultipleSubscriptions": false,
            "MultipleSubscriptionsCount": 1
        },
        {
            "Id": "56ec64c380ed850042060002",
            "Title": "Unlimited",
            "Description": "Subscribers have completely unlimited access to the API. Administrator approval is required.",
            "Terms": null,
            "ProductState": 1,
            "AllowMultipleSubscriptions": false,
            "MultipleSubscriptionsCount": 1
        }
    ]
}
```

Koden i den **produktlista** mall bearbetar data för att tillhandahålla önskade utdata genom att gå igenom insamling av produkter som ska visa information och en länk till varje enskild produkt. Observera den `<search-control>` och `<page-control>` element i koden. Dessa styra visningen av söka och växling kontroller på sidan. `ProductsStrings|PageTitleProducts`är en referens för lokaliserad sträng som innehåller den `h2` rubriktexten på sidan. En lista över strängresurser kontroller och ikoner som är tillgängliga för användning i developer portal mallar, se [API Management portal mallar för utvecklare](api-management-developer-portal-templates-reference.md).

```html
<search-control></search-control>
<div class="row">
    <div class="col-md-9">
        <h2>{% localized "ProductsStrings|PageTitleProducts" %}</h2>
    </div>
</div>
<div class="row">
    <div class="col-md-12">
    {% if products.size > 0 %}
    <ul class="list-unstyled">
    {% for product in products %}
        <li>
            <h3><a href="/products/{{product.id}}">{{product.title}}</a></h3>
            {{product.description}}
        </li>    
    {% endfor %}
    </ul>
    <paging-control></paging-control>
    {% else %}
    {% localized "CommonResources|NoItemsToDisplay" %}
    {% endif %}
    </div>
</div>
```

## <a name="to-save-a-template"></a>Spara en mall
Om du vill spara en mall, klicka på Spara i mallen-redigeraren.

![Spara mallen][api-management-save-template]

Sparade ändringar är inte live i developer-portalen förrän de har publicerats.

## <a name="to-publish-a-template"></a>Så här publicerar du en mall
Sparade mallar kan publiceras antingen individuellt eller alla tillsammans. Om du vill publicera en mall för enskilda, klicka på Publicera i Redigeraren för mallen.

![Publicera mall][api-management-publish-template]

Klicka på **Ja** att bekräfta och göra mallen live på developer-portalen.

![Bekräfta publicera][api-management-publish-template-confirm]

Om du vill publicera alla för tillfället opublicerade mallversioner **publicera** i listan över mallar. Avpublicera mallar betecknas med en asterisk efter mallnamnet. I det här exemplet i **produktlista** och **produkten** mallar publiceras.

![Publicera mallar][api-management-publish-templates]

Klicka på **publicera anpassningar** att bekräfta.

![Bekräfta publicera][api-management-publish-customizations]

Nyligen publicerade mallarna träder i kraft omedelbart i developer-portalen.

## <a name="to-revert-a-template-to-the-previous-version"></a>Återställa en mall till föregående version
Om du vill återställa en mall till den tidigare publicerade versionen klickar du på Återställ i Redigeraren för mallen.

![Återställa mall][api-management-revert-template]

Bekräfta genom att klicka på **Ja**.

![Bekräfta][api-management-revert-template-confirm]

Den tidigare publicerade versionen av en mall är live i developer-portalen när återställningen är klar.

## <a name="to-restore-a-template-to-the-default-version"></a>Återställa en mall till standardversion
Återställer mallar till deras standardversionen är en tvåstegsprocess. Först mallarna måste återställas och sedan de återställda versionerna måste publiceras.

Om du vill återställa en mall till standardversionen klickar du på Återställ i Redigeraren för mallen.

![Återställa mall][api-management-reset-template]

Bekräfta genom att klicka på **Ja**.

![Bekräfta][api-management-reset-template-confirm]

Om du vill återställa alla mallar till standardversioner **återställa standardmallarna** mall-listan.

![Återställ mallar][api-management-restore-templates]

Återställda mallarna måste sedan publiceras individuellt eller på en gång genom att följa stegen i [att publicera en mall](#to-publish-a-template).

## <a name="next-steps"></a>Nästa steg
Referensinformation för developer portal mallar, strängresurser, ikoner och kontroller, se [API Management portal mallar för utvecklare](api-management-developer-portal-templates-reference.md).

[modify-content-layout]: api-management-modify-content-layout.md
[customize-styles]: api-management-customize-styles.md
[portal-templates]: api-management-developer-portal-templates.md

[api-management-customize-menu]: ./media/api-management-developer-portal-templates/api-management-customize-menu.png
[api-management-templates-menu]: ./media/api-management-developer-portal-templates/api-management-templates-menu.png
[api-management-developer-portal-templates-overview]: ./media/api-management-developer-portal-templates/api-management-developer-portal-templates-overview.png
[api-management-template]: ./media/api-management-developer-portal-templates/api-management-template.png
[api-management-template-data]: ./media/api-management-developer-portal-templates/api-management-template-data.png
[api-management-developer-portal-menu]: ./media/api-management-developer-portal-templates/api-management-developer-portal-menu.png
[api-management-management-console]: ./media/api-management-developer-portal-templates/api-management-management-console.png
[api-management-browse]: ./media/api-management-developer-portal-templates/api-management-browse.png
[api-management-user-profile-templates]: ./media/api-management-developer-portal-templates/api-management-user-profile-templates.png
[api-management-save-template]: ./media/api-management-developer-portal-templates/api-management-save-template.png
[api-management-publish-template]: ./media/api-management-developer-portal-templates/api-management-publish-template.png
[api-management-publish-template-confirm]: ./media/api-management-developer-portal-templates/api-management-publish-template-confirm.png
[api-management-publish-templates]: ./media/api-management-developer-portal-templates/api-management-publish-templates.png
[api-management-publish-customizations]: ./media/api-management-developer-portal-templates/api-management-publish-customizations.png
[api-management-revert-template]: ./media/api-management-developer-portal-templates/api-management-revert-template.png
[api-management-revert-template-confirm]: ./media/api-management-developer-portal-templates/api-management-revert-template-confirm.png
[api-management-reset-template]: ./media/api-management-developer-portal-templates/api-management-reset-template.png
[api-management-reset-template-confirm]: ./media/api-management-developer-portal-templates/api-management-reset-template-confirm.png
[api-management-restore-templates]: ./media/api-management-developer-portal-templates/api-management-restore-templates.png







