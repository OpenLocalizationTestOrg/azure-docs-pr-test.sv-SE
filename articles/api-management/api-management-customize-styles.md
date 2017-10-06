---
title: aaaCustomize format i hello developer-portalen i Azure API Management | Microsoft Docs
description: "Lär dig hur toomodify hello format används för en sida i hello developer-portalen i Azure API Management."
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
ms.openlocfilehash: aaaa86527992ba43e64eab5fd35c7f57b573c812
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="customize-hello-styling-of-hello-developer-portal-in-azure-api-management"></a>Anpassa hello stil för hello developer-portalen i Azure API Management
Det finns tre huvudsakliga sätt toocustomize hello developer-portalen i Azure API Management:

* [Redigera hello innehållet i statiska sidor och layout sidelement][modify-content-layout]
* [Uppdatera hello formatmallar som används för sidelement över hello developer-portalen] [ customize-styles] (beskrivs i den här guiden)
* [Ändra hello-mallar som används för sidor som genereras av hello portal] [ portal-templates] (t.ex. API-dokumentation, produkter, autentisering av användare, etc.)

## <a name="change-headers-styling"></a>Ändra hello formatmalls sidelement

hello definieras färger, teckensnitt, storlek, avståndet mellan och andra style-relaterade element på en sida på portalen hello av style-regler. 

Redigera hello formatmallsregler görs från hello **utvecklarportalen** när du är inloggad som administratör. tooget det först öppna hello Azure-portalen och klicka på **Publisher portal** hello service verktygsfältet för API Management-instansen.

![Utgivarportalen][api-management-management-console]

Klicka på **utvecklarportalen** på hello längst upp till höger. 

![Developer portal länk på hello publisher portal][api-management-pp-dp-link]

tooopen hello anpassning verktygsfältet håller muspekaren över ikonen för anpassning av hello (eller markera den) och sedan klicka på ”Format” hello verktygsfältet.

![Knapp i verktygsfältet för anpassning][api-management-customization-toolbar-button]

Det finns två huvudsakliga sätt att redigera formatmalls-regler – du kan titta igenom hello lista över alla regler för hello format används var som helst som visas som standard och ändrar du en formatmall eller du kan välja **Markera ett element på sidan hello** och sedan Klicka någonstans på hello sidan toosee endast hello format för elementet.

![Verktygsfält för anpassning][api-management-customization-toolbar]

Klicka på hello **Markera ett element på sidan hello** alternativ för det här exemplet.  Element markeras nu när du hovrar över dem med hello musen toosignify vilka element som du vill börja redigera om du klickade på. Flytta hello Håll muspekaren över hello text i hello huvud (vanligtvis du har hello företagsnamn här) och klicka sedan på den. En uppsättning regler för namngivna och kategoriserade Design visas inom hello formatmalls-redigeraren. Varje regel representerar en formatmalls-egenskap för hello valda elementet. Till exempel för hello rubriktexten ovan valda, hello textstorleken hello är i @font-size-h1 medan hello namnet på hello teckensnitt med alternativ @headings-font-family.

> Om du är bekant med [bootstrap][bootstrap], dessa regler är i själva verket [mindre variabler] [ LESS variables] inom hello bootstrap-tema som används av hello Developer-portalen.
> 
> 

Nu ska vi ändra hello färg hello rubrik. Välj hello posten i hello  **@headings-color**  fält och ange **#000000**. Detta är hello hex-kod för hello färg svart. När du gör detta kan se du att en kvadratisk färg indikatorn visas hello slutet av hello textruta. Om du klickar på denna indikator kan en toochoose en färg.

![Färgväljare][api-management-customization-toolbar-color-picker]

Ändringar förhandsgranskas i realtid som du gör dem, men visas bara tooadministrators. toomake dessa ändringar visas tooeveryone klickar du på hello **publicera** i hello formatmalls-redigeraren och bekräfta hello ändringar.

![Menyn Publish (Publicera)][api-management-customization-toolbar-publish-form]

> toochange hello formatmallen regler som gäller tooany andra element på sidan hello, följ hello samma procedur som du gjorde för hello-huvud. Klicka på **Markera ett element på sidan hello** från hello formatmalls-redigeraren, väljer hello-element som du är intresserad av och starta ändra hello värdena för hello formatmallsregler som visas på hello-skärmen.
> 
> 


## <a name="next-steps"> </a>Nästa steg
* Lär dig hur toocustomize hello innehållet i developer-portalen sidor med [Developer portal mallar](api-management-developer-portal-templates.md).

[Change hello styling of hello headers]: #change-headers-styling
[Next steps]: #next-steps

[Azure Classic Portal]: https://manage.windowsazure.com/

[api-management-management-console]: ./media/api-management-customize-styles/api-management-management-console.png
[api-management-pp-dp-link]: ./media/api-management-customize-styles/api-management-pp-dp-link.png
[api-management-customization-toolbar-button]: ./media/api-management-customize-styles/api-management-customization-toolbar-button.png
[api-management-customization-toolbar]: ./media/api-management-customize-styles/api-management-customization-toolbar.png
[api-management-customization-toolbar-color-picker]: ./media/api-management-customize-styles/api-management-customization-toolbar-color-picker.png
[api-management-customization-toolbar-publish-form]: ./media/api-management-customize-styles/api-management-customization-toolbar-publish-form.png

[modify-content-layout]: api-management-modify-content-layout.md
[customize-styles]: api-management-customize-styles.md
[portal-templates]: api-management-developer-portal-templates.md

[bootstrap]: http://getbootstrap.com/
[LESS variables]: http://getbootstrap.com/css/
