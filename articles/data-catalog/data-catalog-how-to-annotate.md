---
title: "datakällor för aaaHow tooannotate | Microsoft Docs"
description: "Hur tooarticle syntaxmarkering hur tooannotate datatillgångar i Azure Data Catalog, inklusive egna namn, taggar, beskrivningar och experter."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 5a7e6bb2-863c-4eca-b614-1c814920d9ed
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 1d1ef34e3f1ef73cdc65129209d938abe1e36c01
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooannotate-data-sources"></a>Hur tooannotate datakällor
## <a name="introduction"></a>Introduktion
**Microsoft Azure Data Catalog** är en helt hanterad molntjänst som fungerar som ett system för registrering och identifieringssystem för företagets datakällor. Data Catalog är med andra ord om hjälper människor identifiera, förstå och använda datakällor och hjälpa organisationer tooget mer värde från deras befintliga data. När en datakälla har registrerats med Data Catalog kan dess metadata kopieras och indexerade av hello-tjänsten, men hello artikeln det sluta inte. Data Catalog kan användare tooprovide sina egna beskrivande metadata – till exempel beskrivningar och taggar – toosupplement hello metadata som extraherats från datakällan hello och toomake hello datakällan fler förstå toomore personer.

## <a name="annotation-and-crowdsourcing"></a>Anteckningen och gemensamt skapade
Alla har en åsikt. Detta är en bra.
Data Catalog märker att olika användare har olika perspektiv på företagets datakällor och var och en av dessa perspektiv kan vara användbart. Tänk hello följande scenario:

* hello systemadministratören vet hello servicenivåavtalet för hello servrar eller tjänster som värd hello-datakällan.
* hello databasadministratören vet hello säkerhetskopiering schema för varje databas och tillåtna ETL-bearbetningen windows hello.
* hello systemets huvudägare vet hello process för användare toorequest access toohello-datakälla.
* hello data steward vet hur hello tillgångar och attribut i hello datakällan mappa toohello enterprise-datamodell.
* hello analytiker vet hur hello data används i hello kontexten hello affärsprocesser som han har stöd för.

Var och en av dessa perspektiv är värdefullt och Data Catalog använder ett gemensamt skapade metoden toometadata som gör att varje en toobe fångas och används tooprovide en komplett bild av registrerade datakällor. Med hello Data Catalog-portalen kan varje användare lägga till och redigera egna anteckningar, samtidigt som kan tooview anteckningar som tillhandahålls av andra användare.

## <a name="different-types-of-annotations"></a>Olika typer av anteckningar
Data Catalog stöder hello följande typer av kommentarer:

| Anteckningen | Anteckningar |
| --- | --- |
| Eget namn |Eget namn kan anges på hello data tillgången nivå lättare förstås toomake hello datatillgångar. Eget namn är mest användbara när hello underliggande objektnamnet är kryptiskt, förkortat eller på annat sätt inte meningsfulla toousers. |
| Beskrivning |Beskrivningar som kan anges på hello datatillgång och attributet / kolumn nivåer. Beskrivningar är Friform kort textanteckningar som beskriver hello användare på hello datatillgång eller dess användning. |
| Taggar (användare taggar) |Taggar kan anges på hello datatillgång och attributet / kolumn nivåer. Användaren taggar är användardefinierade etiketter som kan vara används toocategorize datatillgångar eller attribut. |
| Taggar (ordlista taggar) |Taggar kan anges på hello datatillgång och attributet / kolumn nivåer. Ordlista taggar är centralt definierade ordlista som kan vara används toocategorize datatillgångar eller attribut med hjälp av en gemensam taxonomi för företag. Mer information finns i [hur tooset in hello Företagsordlista för hanterade taggar](data-catalog-how-to-business-glossary.md) |
| Experter |Experter kan anges på hello data tillgången nivå. Experter identifiera användare eller grupper med expertkunskaper om hello data och kan fungera som kontaktpunkter för användare som identifierar hello registrerade datakällor och har frågor som inte besvaras av hello befintliga kommentarer. |
| Begär åtkomst |Information om åtkomstbegäran åtkomst kan anges på hello data tillgången nivå. Den här informationen är avsedd för användare som identifierar en datakälla som att de inte ännu har behörigheter tooaccess. Användare kan ange hello e-postadress hello användare eller grupp som beviljar åtkomst, hello URL för hello processen verktyget som användare behöver toogain åtkomst eller kan ange hello-processen som text. |
| Dokumentation |Dokumentationen kan anges på hello data tillgången nivå. Tillgångsinformation dokumentation är RTF-information som kan innehålla länkar och bilder och som kan ge information som överförs inte via beskrivningar och taggar. |

## <a name="annotating-multiple-assets"></a>Kommentera flera tillgångar
När du väljer flera datatillgångar i hello Data Catalog-portalen kan användarna anteckna alla valda tillgångar i en enda åtgärd. Anteckningar kommer tillämpa tooall valda tillgångar, vilket gör det enkelt tooselect och ger en konsekvent beskrivning och uppsättningar med taggar och experter för relaterade datatillgångar.

> [!NOTE]
> Taggar och experter kan också anges när registrera datatillgångar med hello Data Catalog data source registreringsverktyget.
>
>

När du väljer flera tabeller och vyer, endast kolumner alla markerade data som är gemensamma för tillgångar visas i hello Data Catalog-portalen. Detta ger användarna tooprovide taggar och beskrivningar för alla kolumner med samma namn hello alla valda tillgångar.

## <a name="annotations-and-discovery"></a>Anteckningar och identifiering
Precis som hello metadata som extraherats från hello datakällan under registreringen läggs toohello Data Catalog sökindex, användardefinierat metadata indexeras också. Detta innebär att inte bara göra anteckningar gör det enklare för användarna toounderstand hello data de identifiera anteckningar gör det också lättare för användare toodiscover hello kommenterats datatillgångar genom att söka med hello villkor som gör meningsfullt toothem.

## <a name="summary"></a>Sammanfattning
Registrera en datakälla med Data Catalog gör att data kan identifieras genom att kopiera strukturella och beskrivande metadata från hello datakälla till hello katalogtjänsten. När en datakälla har registrerats, kan användare ange anteckningar toomake enklare toodiscover och förstå från inom hello Data Catalog-portalen.

## <a name="see-also"></a>Se även
* [Kom igång med Azure Data Catalog](data-catalog-get-started.md) självstudiekursens steg för steg-information om hur tooannotate datakällor.
