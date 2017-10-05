---
title: "Portaler för att skapa och redigera loggen frågor i Azure Log Analytics | Microsoft Docs"
description: "Den här artikeln beskriver portalerna som du kan använda i Azure Log Analytics för att skapa och redigera logga sökningar."
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
ms.openlocfilehash: 29dfa31d38f85574f84ed351bc5c26224b1a7e16
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="portals-for-creating-and-editing-log-queries-in-azure-log-analytics"></a>Portaler för att skapa och redigera loggen frågor i Azure Log Analytics

> [!NOTE]
> Den här artikeln beskriver portaler i Azure Log Analytics med hjälp av det nya språket i fråga.  Du lär dig mer om det nya språket och få proceduren för att uppgradera din arbetsyta på [uppgradera Azure logganalys-arbetsytan till ny logg sökning](log-analytics-log-search-upgrade.md).  
>
> Om ditt arbetsområde inte har uppgraderats till det nya språket i fråga, bör du gå till [söka efter data med hjälp av loggen sökningar i logganalys](log-analytics-log-searches.md) information om den aktuella versionen av loggen Sök-portalen.

Du kan använda loggen sökningar på olika ställen i Log Analytics för att hämta data från arbetsytan.  Faktiskt skapar och redigerar frågor förutom att arbeta interaktivt med data som returneras om, har du två alternativ som beskrivs nedan.  

## <a name="log-search-portal"></a>Loggen sökportal
Loggen Sök-portalen är åtkomlig från Azure-portalen eller OMS-portalen.  Det är lämpligt för att skapa grundläggande frågor som kan skapas på en enda rad.  Loggen Sök-portalen kan användas utan att starta en extern portal och du kan använda för att utföra en mängd funktioner med loggen sökningar, inklusive att skapa Varningsregler, skapar datorgrupper och exportera resultatet av frågan.  

Loggen Sök portal innehåller flera funktioner för att redigera frågan utan att ha fullständiga kunskaper om frågespråket.  Du kan få en översikt över funktionerna i [skapa loggen söker i Azure Log Analytics med hjälp av loggen Sök portal](log-analytics-log-search-log-search-portal.md).


![Logga sökportal](media/log-analytics-log-search-portals/log-search-portal.png)

## <a name="advanced-analytics-portal"></a>Avancerade Analytics-portalen
Avancerade analyser portal är en dedikerad portal som ger avancerade funktioner som inte är tillgängliga i loggen Sök-portalen.  Innefattar möjligheten att redigera en fråga på flera rader, selektivt köra kod, sammanhangsberoende Intellisense och smarta Analytics.  Advanced Analytics-portalen är mest lämpliga för att utforma komplexa frågor som antingen sparas som en logg sökning eller kopieras och klistras in i andra logganalys-element.  Du kan starta Advanced Analytics-portalen från en länk i loggen Sök-portalen.

![Avancerade Analytics-portalen](media/log-analytics-log-search-portals/advanced-analytics-portal.png)


På grund av dess avancerade funktioner, ska du vanligtvis använda avancerade analyser portal som din primära verktyget för att skapa och redigera frågor.  När du har bestämt att frågan fungerar som förväntat och sedan du kopierar och klistrar in den någon annanstans, till exempel loggen Sök portalen eller View Designer.  Eftersom Advanced Analytics-portalen stöder frågor med flera rader om du behöver ta hänsyn till följande när du kopierar en fråga från den här portalen.

- Kommentarer måste tas bort från frågan innan den har kopieras och klistras in i en annan plats.  Du kan kommentera en rad genom att låta det med två snedstreck (/ /).  När du klistrar in en flera rad-fråga till en enda rad tas radbrytningar bort.  Om kommentarer ingår betraktas alla tecken efter den första kommentaren som en del av kommentaren.


## <a name="next-steps"></a>Nästa steg

- Gå igenom en självstudiekurs om hur du använder den [loggen Sök portal](log-analytics-log-search-log-search-portal.md) eller [Advanced Analytics portal](https://go.microsoft.com/fwlink/?linkid=856587) att skapa frågor.
- Checka ut en [självstudier om hur du skriver frågor](https://go.microsoft.com/fwlink/?linkid=856078) med det nya språket i fråga.
