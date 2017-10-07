---
title: "aaaPortals för att skapa och redigera loggen frågor i Azure Log Analytics | Microsoft Docs"
description: "Den här artikeln beskriver hello portaler att du kan använda i Azure Log Analytics toocreate och redigera loggen sökningar."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: bwren
ms.openlocfilehash: 7a2657574a132b2c4298511bb31259e68113ac91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="portals-for-creating-and-editing-log-queries-in-azure-log-analytics"></a>Portaler för att skapa och redigera loggen frågor i Azure Log Analytics

> [!NOTE]
> Den här artikeln beskriver portaler i Azure Log Analytics med hjälp av hello nya frågespråk.  Du lär dig mer om hello nytt språk och få hello proceduren tooupgrade ditt arbetsområde på [uppgradera sökningen Azure logganalys arbetsytan toonew loggen](log-analytics-log-search-upgrade.md).  
>
> Om ditt arbetsområde inte har varit uppgraderade toohello nya frågespråk, bör du läsa för[söka efter data med hjälp av loggen sökningar i logganalys](log-analytics-log-searches.md) information om hello nuvarande version av hello loggen Sök-portalen.

Du kan använda loggen sökningar på olika ställen i logganalys tooretrieve data från hello arbetsyta.  Dessutom tooworking interaktivt med returnerade data om för att skapa och redigera frågor, har du två alternativ som beskrivs nedan.  

## <a name="log-search-portal"></a>Loggen sökportal
hello loggen Sök portal är åtkomlig från hello Azure-portalen eller hello OMS-portalen.  Det är lämpligt för att skapa grundläggande frågor som kan skapas på en enda rad.  hello loggen Sök portal kan användas utan att starta en extern portal och du kan använda den tooperform en mängd funktioner med loggen söker bland annat skapa Varningsregler, skapar datorgrupper och exportera hello resultaten av hello-frågan.  

hello loggen Sök portal innehåller flera funktioner för redigerar hello fråga utan att ha fullständiga kunskaper om hello frågespråk.  Du kan få en översikt över funktionerna i [skapa loggen söker i Azure Log Analytics med hjälp av hello loggen Sök portal](log-analytics-log-search-log-search-portal.md).


![Logga sökportal](media/log-analytics-log-search-portals/log-search-portal.png)

## <a name="advanced-analytics-portal"></a>Avancerade Analytics-portalen
hello avancerade analyser portal är en dedikerad portal som ger avancerade funktioner som är inte tillgänglig i hello loggen Sök-portalen.  Funktioner omfattar hello möjlighet tooedit en fråga på flera rader, köra selektivt kod, sammanhangsberoende Intellisense och smarta Analytics.  hello Advanced Analytics-portalen är mest lämpliga för att utforma komplexa frågor som antingen sparas som en logg sökning eller kopieras och klistras in i andra logganalys-element.  Du kan starta hello Advanced Analytics-portalen från en länk i hello loggen Sök portal.

![Avancerade Analytics-portalen](media/log-analytics-log-search-portals/advanced-analytics-portal.png)


På grund av dess avancerade funktioner använder vanligtvis du hello avancerade analyser portal som din primära verktyget för att skapa och redigera frågor.  När du har bestämt att hello frågan fungerar som förväntat och sedan du kopierar och klistrar in den någon annanstans, till exempel hello loggen Sök portalen eller View Designer.  Eftersom hello Advanced Analytics portal stöder dock frågor med flera rader, måste tootake hello följande i åtanke när du kopierar en fråga från den här portalen.

- Kommentarer måste tas bort från hello fråga innan den har kopieras och klistras in i en annan plats.  Du kan kommentera en rad genom att låta det med två snedstreck (/ /).  När du klistrar in en flera rad-fråga till en enda rad tas radbrytningar bort.  Om kommentarer ingår betraktas alla tecken efter hello första kommentaren som en del av hello-kommentaren.


## <a name="next-steps"></a>Nästa steg

- Gå igenom en självstudiekurs om hur du använder hello [loggen Sök portal](log-analytics-log-search-log-search-portal.md) eller hello [Advanced Analytics portal](https://go.microsoft.com/fwlink/?linkid=856587) toocreate frågor.
- Checka ut en [självstudier om hur du skriver frågor](https://go.microsoft.com/fwlink/?linkid=856078) med hello nya frågespråk.
