---
title: "Azure Data Source-guiden för Azure Machine Learning | Microsoft Docs"
description: "Beskriver guiden Datakälla AML arbetsstationen"
services: machine-learning
author: cforbe
ms.author: cforbe
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/07/2017
ms.openlocfilehash: ff0159facd693b83230c731eb7e76f0a9495fdf2
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/18/2017
---
# <a name="data-source-wizard"></a>Guiden datakälla #

Guiden datakälla är ett snabbt och enkelt sätt att sätta en datamängd i Azure ML-arbetsstationen utan kod. Det är där du kan också välja en exempel-strategi för datauppsättningen. 

## <a name="step-1-trigger-the-data-source-wizard"></a>Steg 1: Starta guiden datakälla ## 

Hämta data till ett projekt med hjälp av guiden datakälla. Välj den  **+**  knappen bredvid sökrutan i datavyn och välj Lägg till datakälla. 

![Lägga till en datakälla](media/data-source-wizard/add-data-source.png)

## <a name="step-2-select-where-data-is-stored"></a>Steg 2: Välj var data lagras ##
Ange först hur dina data är för närvarande i. Det kan lagras i en flat-fil eller en katalog, en parkettgolv-fil, en Excel-fil eller en databas. Se [stöds datakällor](data-prep-appendix2-supported-data-sources.md) för mer information.

![Steg 1](media/data-source-wizard/step1.png)

## <a name="step-3-select-data-file"></a>Steg 3: Välj datafilen ##
Ange sökvägen till filen för en fil/katalog. Välj plats för data i listrutan – det kan vara en lokal filsökväg eller Azure Blob Storage. 

Ange sökvägen genom att skriva in den eller klicka på den **Bläddra...** knappen för att söka efter den. Du kan bläddra efter en katalog eller en eller flera filer.

Klicka på **Slutför** Godkänn standardinställningarna för resten av stegen eller nästa för att gå vidare till nästa steg.


![Steg 4](media/data-source-wizard/step2.png)

## <a name="step-4-choose-file-parameters"></a>Steg 4: Välja parametrar ##

Guiden datakälla kan automatiskt identifiera filen typ, avgränsare och kodning. Den visas också ett exempel på hur data ser ut. Du kan också ändra någon av dessa parametrar manuellt. 

![Steg 5](media/data-source-wizard/step3.png)

## <a name="step-5-set-data-types-for-columns"></a>Steg 5: Ange datatyper för kolumner ##

Guiden Datakälla identifierar automatiskt datatyperna för kolumnerna i dataset. Om den missar någon eller om du vill framtvinga en datatyp, kan du manuellt ändra datatypen. Den **utdata EXEMPELDATA** kolumnen visar exempel på hur data ser ut.

![Steg 6](media/data-source-wizard/step4.png)

## <a name="step-6-choose-sampling-strategy-for-data"></a>Steg 6: Välj provtagning för data ##

Du kan ange en eller flera provtagning strategier för datauppsättningen och välj som aktiv strategin. Standardvärdet är att ladda upp 10000 rader. Du kan redigera det här exemplet genom att klicka på den **redigera** i verktygsfältet eller lägga till en ny strategi genom att klicka på + ny. De strategier som vi stöder för närvarande är

-     Högsta antal rader
-     Slumpmässigt antal rader
-     Slumpmässiga procentandelen rader
-     Fullständig

Du kan ange så många provtagning strategier som du vill, men det finns bara ett som kan ställas in som aktiv när du förbereder dina data. Du kan ange en strategi för att vara aktiv genom att välja en strategi för och klicka på Ange som aktiv i verktygsfältet.

Beroende på var data kommer ifrån, stöds några exempel strategier inte. Mer information om sampling avsnittet provtagning i [det här dokumentet](data-prep-user-guide.md) 

![Steg 6](media/data-source-wizard/step5.png)

## <a name="step-7-path-column-handling"></a>Steg 7: Sökväg kolumnen hantering ##

Om sökvägen innehåller viktiga data, kan du inkludera den som den första kolumnen i datauppsättningen. Detta är användbart om du tar med i flera filer. Annars kan du inte inkludera den.

![Steg 7](media/data-source-wizard/step6.png)

När du klickar på Slutför läggs en ny datakälla i projektet. Du hittar den under gruppen datakällor i datavyn eller som en .dsource-fil i den **Filvyn**.
