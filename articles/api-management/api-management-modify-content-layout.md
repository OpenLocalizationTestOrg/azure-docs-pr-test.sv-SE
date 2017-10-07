---
title: "aaaModify sidinnehållet i hello developer-portalen i Azure API Management | Microsoft Docs"
description: "Lär dig hur tooedit sidinnehåll på hello developer-portalen i Azure API Management."
services: api-management
documentationcenter: 
author: antonba
manager: vlvinogr
editor: 
ms.assetid: 186128fe-41c0-4efb-9efe-2478ad4d103f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/09/2017
ms.author: antonba
ms.openlocfilehash: fd5a854e900d9512518643e593b1b59a0952621f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="modify-hello-content-and-layout-of-pages-on-hello-developer-portal-in-azure-api-management"></a>Ändra hello innehåll och layout av sidorna på hello developer-portalen i Azure API Management
Det finns tre huvudsakliga sätt toocustomize hello developer-portalen i Azure API Management:

* [Redigera hello innehållet i statiska sidor och layout sidelement] [ modify-content-layout] (beskrivs i den här guiden)
* [Uppdatera hello formatmallar som används för sidelement över hello developer-portalen][customize-styles]
* [Ändra hello-mallar som används för sidor som genereras av hello portal] [ portal-templates] (t.ex. API-dokumentation, produkter, autentisering av användare, etc.)

## <a name="page-structure"> </a>Sidstruktur för utvecklarportalen

hello developer-portalen är baserad på ett system för innehållshantering. hello layout på varje sida bygger baserat på mängd av små sidelement kallas widgetar:

![Sidstruktur för utvecklarportalen][api-management-customization-widget-structure]

Alla widgetar kan redigeras. 
* hello core specifika tooeach enskilda innehållssidan finnas i hello ”innehållet” widget. Redigera en sida innebär redigering hello innehållet i den här widget.
* Alla element på layout finns med hello återstående widgetar. Ändringar som gjorts toothese widgetar gäller tooall sidor. De kommer att anges tooas ”layout widgetar”.

Dagliga sidan ändras Redigera en vanligtvis endast hello innehåll widget som har olika innehåll för varje enskild sida.

## <a name="modify-layout-widget"></a>Ändra hello innehållet i en layout widget

Innehållet i hello developer-portalen ändras via hello publisher portal som kan nås från hello Azure-portalen. tooreach, klickar du på **Publisher portal** hello service verktygsfältet för API Management-instansen.

![Utgivarportalen][api-management-management-console]

tooedit hello innehållet i den widgeten, klickar du på **widgetar** från hello **Utvecklarportalen** menyn hello vänster. För det här exemplet kan du ändra hello innehållet i hello sidhuvud widget. Välj hello **huvud** widget hello-listan.

![Widgeten Sidhuvud][api-management-widgets-header]

hello innehållet i hello sidhuvud redigeras från hello **brödtext** fältet. Ändra hello text efter behov och klicka sedan på **spara** på hello hello sidans nederkant.

Nu bör du kunna toosee hello nytt huvud på varje sida i hello developer-portalen.

> tooopen hello developer-portalen i hello publisher-portalen klickar du på **utvecklarportalen** i hello översta raden.
> 
> 

## <a name="edit-page-contents"></a>Redigera hello innehållet på en sida

toosee hello lista över alla befintliga innehållssidor, klicka på **innehåll** från hello **utvecklarportalen** -menyn i hello publisher portal.

![Hantera innehåll][api-management-customization-manage-content]

Klicka på hello **Välkommen** sidan tooedit vad som visas på hello startsida hello developer-portalen. Ändra hello, förhandsgranska dem om det behövs och klicka sedan på **publicera nu** toomake dem synliga tooeveryone.

> startsidan för hello använder en särskild layout som tillåter toodisplay en banderoll överst hello. Det här sidhuvudet kan inte ändras från hello **innehåll** avsnitt. tooedit detta banderoll, klicka på **widgetar** från hello **utvecklarportalen** väljer du **startsidan** från hello **aktuella lagret** listrutan listan, och sedan öppna hello **banderoll** objekt under hello **aktuella avsnittet**. hello innehållet i den här widget kan redigeras precis som andra sidan.
> 
> 

## <a name="next-steps"> </a>Nästa steg
* [Uppdatera hello formatmallar som används för sidelement över hello developer-portalen][customize-styles]
* [Ändra hello-mallar som används för sidor som genereras av hello portal] [ portal-templates] (t.ex. API-dokumentation, produkter, autentisering av användare, etc.)

[Structure of developer portal pages]: #page-structure
[Modifying hello contents of a layout widget]: #modify-layout-widget
[Edit hello contents of a page]: #edit-page-contents
[Next steps]: #next-steps

[modify-content-layout]: api-management-modify-content-layout.md
[customize-styles]: api-management-customize-styles.md
[portal-templates]: api-management-developer-portal-templates.md

[api-management-customization-widget-structure]: ./media/api-management-modify-content-layout/portal-widget-structure.png
[api-management-management-console]: ./media/api-management-modify-content-layout/api-management-management-console.png
[api-management-widgets-header]: ./media/api-management-modify-content-layout/api-management-widgets-header.png
[api-management-customization-manage-content]: ./media/api-management-modify-content-layout/api-management-customization-manage-content.png
