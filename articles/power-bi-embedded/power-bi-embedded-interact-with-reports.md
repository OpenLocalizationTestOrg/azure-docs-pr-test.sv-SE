---
title: aaaInteract med rapporter med hello JavaScript API | Microsoft Docs
description: Power BI Embedded, interagera med rapporter med hello JavaScript API
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: bdd885d3-1b00-4dcf-bdff-531eb1f97bfb
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 01/06/2017
ms.author: asaxton
ms.openlocfilehash: 657e4d5cee031bdda173ab3f451cc19b93ddb17b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="interact-with-power-bi-reports-using-hello-javascript-api"></a>Interagera med Power BI-rapporter med hello JavaScript API
hello Power BI JavaScript API som ger du tooeasily bädda in Power BI-rapporter i dina program. Med hello API interagera dina program med annan rapportelement som sidor och filter. Den här interaktiviteten gör Power BI-rapporter till en mer integrerad del av ditt program.

Du bädda in en Power BI-rapport i ditt program med en iframe som finns som en del av programmet hello. hello iframe fungerar som en gräns mellan program och hello rapport som du ser i följande bild hello. 

![Power BI inbäddad iframe utan Javascript API](media/powerbi-embedded-interact-with-reports/powerbi-embedded-interact-report-1.png)

hello iframe gör hello bädda in processen mycket enklare, men utan hello JavaScript API hello rapporten och programmet samverka inte med varandra. Den här bristen på interaktionen kan det vara känner hello rapporten inte är verkligen en del av programmet hello. hello rapporten och program du verkligen behöver toocommunicate fram och tillbaka som hello följande bild.

![Power BI inbäddad iframe med Javascript API](media/powerbi-embedded-interact-with-reports/powerbi-embedded-interact-report-2.png)

hello Power BI JavaScript API kan du toowrite kod som på ett säkert sätt kan passera hello iframe gräns. Det här kan ditt program tooprogrammatically utföra en åtgärd i en rapport och toolisten händelser från åtgärder som användare gör inom hello rapport.

## <a name="what-can-you-do-with-hello-power-bi-javascript-api"></a>Vad kan du göra med hello Power BI JavaScript API?
Med hello JavaScript API kan du hantera rapporter, navigera toopages i en rapport, filtrera en rapport och hantera bädda in händelser. hello visar följande diagram hello strukturen för hello API.

![Power BI JavaScript API-diagram](media/powerbi-embedded-interact-with-reports/powerbi-embedded-interact-report-3.png)

### <a name="manage-reports"></a>Hantera rapporter
hello Javascript API kan du toomanage beteende på hello rapport- och:

* Bädda in en specifik Power BI-rapport på ett säkert sätt i ditt program - försök hello [bädda in demo-program](http://azure-samples.github.io/powerbi-angular-client/#/scenario1)
  * Ange åtkomst-token
* Konfigurera hello-rapport
  * Aktivera och inaktivera hello filterfönstret och navigeringsfönstret - försök hello [uppdatera inställningar demo-program](http://azure-samples.github.io/powerbi-angular-client/#/scenario6)
  * Ange standardinställningar för sidor och filter - försök hello [uppsättning standarder demo](http://azure-samples.github.io/powerbi-angular-client/#/scenario5)
* Gå in och ur fullskärmsläge

[Läs mer om att bädda in en rapport](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embedding-Basics)

### <a name="navigate-toopages-in-a-report"></a>Navigera toopages i en rapport
hello JavaScript API enbales du toodiscover alla sidor i en rapport och tooset hello sidan. Försök hello [navigering demo programmet](http://azure-samples.github.io/powerbi-angular-client/#/scenario3).

[Läs mer om sidnavigering](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Page-Navigation)

### <a name="filter-a-report"></a>Filtrera en rapport
hello JavaScript API ger grundläggande och avancerade filtreringsfunktioner för inbäddade rapporter och rapportsidor. Försök hello [filtrering demo programmet](http://azure-samples.github.io/powerbi-angular-client/#/scenario4), och granska vissa inledande koden här.  

#### <a name="basic-filters"></a>Grundläggande filter
Ett grundläggande filter placeras på en kolumn eller hierarki nivå och innehåller en lista över värden tooinclude eller utelämna.

```
const basicFilter: pbi.models.IBasicFilter = {
  $schema: "http://powerbi.com/product/schema#basic",
  target: {
    table: "Store",
    column: "Count"
  },
  operator: "In",
  values: [1,2,3,4]
}
```


#### <a name="advanced-filters"></a>Avancerade filter
Avancerade filter använder hello logisk operator och eller eller och acceptera en eller två villkor, var och en med sin egen operator och ett värde. Villkor som stöds är:

* Ingen
* LessThan
* LessThanOrEqual
* GreaterThan
* GreaterThanOrEqual
* Contains
* DoesNotContain
* StartsWith
* DoesNotStartWith
* Is
* IsNot
* IsBlank
* IsNotBlank

```
const advancedFilter: pbi.models.IAdvancedFilter = {
  $schema: "http://powerbi.com/product/schema#advanced",
  target: {
    table: "Store",
    column: "Name"
  },
  logicalOperator: "Or",
  conditions: [
    {
      operator: "Contains",
      value: "Wash"
    },
    {
      operator: "Contains",
      value: "Park"
    }
  ]
}
```
[Läs mer om filtrering](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Filters)

### <a name="handling-events"></a>Hantera händelser
Dessutom toosending information till hello iframe ditt program kan också ta emot information om hello följande händelser som kommer från hello iframe:

* Bädda in
  * inläst
  * fel
* Rapporter
  * pageChanged
  * dataSelected (kommer snart)

[Läs mer om att hantera händelser](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Handling-Events)

## <a name="next-steps"></a>Nästa steg
Mer information om hello Power BI JavaScript API för utcheckning hello följande länkar:

* [JavaScript API Wiki](https://github.com/Microsoft/PowerBI-JavaScript/wiki)
* [Referens för objektmodell](https://microsoft.github.io/powerbi-models/modules/_models_.html)
* Exempel
  * [Angular](http://azure-samples.github.io/powerbi-angular-client)
  * [Ember](https://github.com/Microsoft/powerbi-ember)
* [Live-demo](https://microsoft.github.io/PowerBI-JavaScript/demo/)

