---
title: "aaaHow toodiscover datakällor i Azure Data Catalog | Microsoft Docs"
description: "Den här artikeln visar hur toodiscover registrerade datatillgångar med Azure Data Catalog, inklusive sökning och filtrering och använder hello träffar färgmarkera funktionerna i hello Azure Data Catalog-portalen."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: f72ae3a3-6573-4710-89a7-f13555e1968c
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 624834b8895dd50c8931c9d3e6f8dc217927c617
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodiscover-data-sources-in-azure-data-catalog"></a>Hur toodiscover datakällor i Azure Data Catalog
## <a name="introduction"></a>Introduktion
Azure Data Catalog är en helt hanterad molntjänst som fungerar som ett system för registrering och identifiering för företagets datakällor. Med andra ord Data Catalog hjälper användare att identifiera, förstå och använda datakällor och det hjälper organisationer som ger mer värde ur befintliga data. När en datakälla har registrerats med Data Catalog kan indexeras dess metadata av hello-tjänsten så att du lätt kan söka toodiscover hello data som du behöver.

## <a name="searching-and-filtering"></a>Sökning och filtrering
Identifieringen i Data Catalog använder två primära mekanismer: sökning och filtrering.

Sökning är utformad toobe både intuitiva och kraftfull. Som standard matchas söktermer mot någon egenskap i hello-katalogen, inklusive som användaren tillhandahållit anteckningar.

Filtrering är utformad toocomplement sökning. Du kan välja specifika egenskaper som till exempel experter, typ av datakälla, objekttyp och taggar. Du kan visa endast matchande datatillgångar och begränsa sökningen resultat toomatching tillgångar.

Genom att använda en kombination av sökning och filtrering kan navigera du snabbt hello datakällor som har registrerats med Data Catalog toodiscover hello datakällor måste.

## <a name="search-syntax"></a>Söksyntax
Men hello fritext standardsökningen är enkel och intuitiv, kan du också använda söksyntax för Data Catalog för större kontroll över hello sökresultat. Data Catalog Sök stöder hello följande metoder:

| Teknik | Användning | Exempel |
| --- | --- | --- |
| Enkel sökning |Enkel sökning som använder en eller flera sökvillkor. Resultatet är alla objekt som matchar en egenskap med ett eller flera av hello villkoren som angetts. |`sales data` |
| Egenskapsomfång |Returnerar endast datakällor där söktermen hello matchas med hello angivna egenskapen. |`name:finance` |
| Booleska operatorer |Utöka eller begränsa sökningen med booleska åtgärder. |`finance NOT corporate` |
| Gruppera med parenteser |Använd parenteser toogroup delar av hello frågan tooachieve logisk isolering, särskilt i kombination med booleska operatorer. |`name:finance AND (tags:Q1 OR tags:Q2)` |
| Jämförelseoperatorer |Använda andra jämförelser än lika för egenskaper som innehåller datum och numeriska datatyper. |`modifiedTime > "11/05/2014"` |

Mer information om Data Catalog search finns hello [Azure Data Catalog](https://msdn.microsoft.com/library/azure/mt267594.aspx) artikel.

## <a name="hit-highlighting"></a>Träffmarkering
När du visar sökresultat något visas egenskaper som matchar angivna hello sökvillkoren (till exempel hello data Tillgångsnamn, beskrivning och taggar) är markerade toomake den enklare tooidentify varför en viss datatillgång returnerades av en viss sökning.

> [!NOTE]
> tooturn av träffar markering, Använd hello **Markera** växla i hello Data Catalog-portalen.
>
>

När du visar sökresultat, kanske den inte alltid är uppenbara varför en datatillgång ingår, även om träffar markering aktiverad. Eftersom alla egenskaper söks som standard kan en datatillgång returneras på grund av en matchning på en egenskap på kolumnnivå. Och eftersom flera användare kan kommentera registrerade datatillgångar med egna etiketter och beskrivningar, inte alla metadata kan inte visas i hello lista över sökresultat.

I hello standard panelen visas varje bildruta som visas i sökresultaten hello innehåller en **visa sökterm matchar** ikonen, så att du snabbt visa hello antalet matchningar och deras plats och toojump toothem om du vill.

 ![Träffar syntaxmarkering och sökning matchar hello Azure Data Catalog-portalen](./media/data-catalog-how-to-discover/search-matches.png)

## <a name="summary"></a>Sammanfattning
Eftersom registrera en datakälla med Data Catalog kopior strukturella och beskrivande metadata från hello data source toohello katalogtjänsten, hello datakällan blir enklare toodiscover och förstå. När du har registrerat en datakälla kan du identifiera med hjälp av filtrering och söka från hello Data Catalog-portalen.

## <a name="next-steps"></a>Nästa steg
* Steg för steg-information om hur toodiscover datakällor, se [Kom igång med Azure Data Catalog](data-catalog-get-started.md).
