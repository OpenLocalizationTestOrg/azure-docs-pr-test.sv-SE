---
title: "aaaCustomize hello API Management developer-portalen med hjälp av mallar-Azure | Microsoft Docs"
description: "Lär dig hur toocustomize hello Azure API Management developer-portalen med hjälp av mallar."
services: api-management
documentationcenter: 
author: steved0x
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
ms.openlocfilehash: b00d5f1534e9466f30ff3920e7aae048feb8b8c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocustomize-hello-azure-api-management-developer-portal-using-templates"></a>Hur toocustomize hello Azure API Management developer-portalen med hjälp av mallar

Det finns tre huvudsakliga sätt toocustomize hello developer-portalen i Azure API Management:

* [Redigera hello innehållet i statiska sidor och layout sidelement][modify-content-layout]
* [Uppdatera hello formatmallar som används för sidelement över hello developer-portalen][customize-styles]
* [Ändra hello-mallar som används för sidor som genereras av hello portal] [ portal-templates] (beskrivs i den här guiden)

Mallar är används toocustomize hello innehållet i systemgenererade developer portalens sidor (t.ex. API-dokumentation, produkter, autentisering av användare, etc.). Med hjälp av [DotLiquid](http://dotliquidmarkup.org/) syntax och en angiven uppsättning lokaliserad strängresurser, ikoner och kontroller, du har stor flexibilitet tooconfigure hello innehåll hello sidor som du vill.

## <a name="developer-portal-templates-overview"></a>Översikt över Developer-portalen mallar
Redigera mallar görs från hello **utvecklarportalen** när du är inloggad som administratör. tooget det först öppna hello Azure-portalen och klicka på **Publisher portal** hello service verktygsfältet för API Management-instansen.

![Utgivarportalen][api-management-management-console]

Klicka på **utvecklarportalen** på hello längst upp till höger. 

![Developer portal meny][api-management-developer-portal-menu]

tooaccess Hej developer portal mallar, klicka på hello anpassa ikonen på hello vänstra toodisplay hello anpassning-menyn och klicka på **mallar**.

![Developer portal mallar][api-management-customize-menu]

lista över mallar hello visas flera kategorier av mallar som omfattar hello olika sidor i hello developer-portalen. Varje mall skiljer sig, men hello steg tooedit dem och publicera ändringar i hello är hello samma. tooedit en mall klickar du på hello hello mallens namn.

![Developer portal mallar][api-management-templates-menu]

Klicka på en mall går du toohello developer Portalsida som kan anpassas av mallen. I det här exemplet hello **produktlista** mallen visas. Hej **produktlista** mall kontroller hello område i hello-skärmen visas med röd hello rektangel. 

![Produkter mall][api-management-developer-portal-templates-overview]

Vissa mallar som hello **användarprofil** mallar, anpassa olika delar av hello samma sida. 

![Användaren profilmallar][api-management-user-profile-templates]

hello Redigeraren för varje developer portal mall har två avsnitt visas på hello hello sidans nederkant. hello vänster visar hello Redigera fönstret för hello mall och hello höger visar hello datamodellen för hello mallen. 

hello mallredigeringsläge fönstret innehåller hello markup som styr hello utseendet och beteendet för hello motsvarande sida i hello developer-portalen. hello markeringar i hello mallen använder hello [DotLiquid](http://dotliquidmarkup.org/) syntax. Är en populär Redigeraren för DotLiquid [DotLiquid för Designers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers). Alla ändringar som gjorts toohello mallen under redigering visas i realtid i hello webbläsare, men är inte synligt tooyour kunder förrän du [spara](#to-save-a-template) och [publicera](#to-publish-a-template) hello mallen.

![Mallen markering][api-management-template]

Hej **malldata** innehåller en guide toohello data modellen för hello entiteter som är tillgängliga för användning i en viss mall. Det ger den här guiden genom att visa hello realtidsdata som visas för närvarande i hello developer-portalen. Du kan expandera hello mallen fönster genom att klicka på hello rektangel i hello övre högra hörnet av hello **malldata** fönstret.

![Mall-datamodell][api-management-template-data]

I hello föregående exempel finns två produkter som visas i hello developer-portalen och som har hämtats från hello data som visas i hello **malldata** fönstret som visas i följande exempel hello.

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
            "Description": "Subscribers will be able toorun 5 calls/minute up tooa maximum of 100 calls/week.",
            "Terms": "",
            "ProductState": 1,
            "AllowMultipleSubscriptions": false,
            "MultipleSubscriptionsCount": 1
        },
        {
            "Id": "56ec64c380ed850042060002",
            "Title": "Unlimited",
            "Description": "Subscribers have completely unlimited access toohello API. Administrator approval is required.",
            "Terms": null,
            "ProductState": 1,
            "AllowMultipleSubscriptions": false,
            "MultipleSubscriptionsCount": 1
        }
    ]
}
```

hello markeringar i hello **produktlista** Mallprocesser hello tooprovide hello önskad utdata genom att gå igenom hello samling produkter toodisplay information och en länk tooeach enskilda produkt. Obs hello `<search-control>` och `<page-control>` element i hello markering. Dessa avgör hello hello sökning och växling kontroller på hello sida. `ProductsStrings|PageTitleProducts`är en referens för lokaliserad sträng som innehåller hello `h2` rubriktexten för hello-sidan. En lista över strängresurser kontroller och ikoner som är tillgängliga för användning i developer portal mallar, se [API Management portal mallar för utvecklare](api-management-developer-portal-templates-reference.md).

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

## <a name="toosave-a-template"></a>toosave en mall
toosave en mall, klicka på Spara hello mallen editor.

![Spara mallen][api-management-save-template]

Sparade ändringar är inte live i hello developer-portalen förrän de har publicerats.

## <a name="toopublish-a-template"></a>toopublish en mall
Sparade mallar kan publiceras antingen individuellt eller alla tillsammans. toopublish en mall för enskilda, klicka på Publicera i hello mall-redigeraren.

![Publicera mall][api-management-publish-template]

Klicka på **Ja** tooconfirm och göra hello mallen live på hello developer-portalen.

![Bekräfta publicera][api-management-publish-template-confirm]

toopublish alla opublicerade mallversioner klickar du på **publicera** i hello lista över mallar. Avpublicera mallar betecknas med en asterisk efter hello mallnamn. I det här exemplet hello **produktlista** och **produkten** mallar publiceras.

![Publicera mallar][api-management-publish-templates]

Klicka på **publicera anpassningar** tooconfirm.

![Bekräfta publicera][api-management-publish-customizations]

Nyligen publicerade mallarna träder i kraft omedelbart i hello developer-portalen.

## <a name="toorevert-a-template-toohello-previous-version"></a>toorevert en mall toohello tidigare version
toorevert en toohello tidigare publicerade versionen av principmallen, klickar du på återgå i hello mall-redigeraren.

![Återställa mall][api-management-revert-template]

Klicka på **Ja** tooconfirm.

![Bekräfta][api-management-revert-template-confirm]

hello tidigare publicerade versionen av en mall finns i hello developer-portalen när hello återställning har slutförts.

## <a name="toorestore-a-template-toohello-default-version"></a>toorestore en mall toohello standardversion
Återställa mallar tootheir standardversionen är en tvåstegsprocess. Första hello mallar måste återställas och sedan hello återställts versioner måste publiceras.

toorestore en standardversion för samma mall toohello Klicka på Återställ i hello mall-redigeraren.

![Återställa mall][api-management-reset-template]

Klicka på **Ja** tooconfirm.

![Bekräfta][api-management-reset-template-confirm]

toorestore alla mallar tootheir standardversioner, klicka på **återställa standardmallarna** hello mall-listan.

![Återställ mallar][api-management-restore-templates]

hello återställda mallar måste sedan publiceras individuellt eller på en gång genom att följa stegen hello i [toopublish en mall](#to-publish-a-template).

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







