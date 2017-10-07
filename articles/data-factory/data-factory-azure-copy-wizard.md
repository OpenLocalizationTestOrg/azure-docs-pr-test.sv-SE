---
title: aaaData Factory Azure kopiera guiden | Microsoft Docs
description: "Läs mer om hur toouse hello guiden för Data Factory Azure kopiera toocopy data från källor toosinks för data som stöds."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 0974eb40-db98-4149-a50d-48db46817076
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: spelluru
ms.openlocfilehash: 188b3ae15f937b84a58aec1b979347ac8090abf9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-factory-copy-wizard"></a>Guiden för Azure Data Factory kopiera
hello Azure Data Factory kopiera guiden förenklar hello processen att vill föra in data, som vanligtvis är ett första steg i ett scenario för integrering av data för slutpunkt till slutpunkt. När du går igenom hello Azure Data Factory kopiera guiden behöver inte toounderstand en JSON-rolldefinitioner för länkade tjänster, datauppsättningar och rörledningar. hello guiden skapar automatiskt en pipeline toocopy data från hello valda datakällan toohello valda målet. Dessutom hjälper hello guiden Kopiera dig att toovalidate hello data som inhämtas hello tidpunkt för redigering. Detta sparar tid, särskilt när du mata in data för hello första gången från hello-datakälla. toostart hello guiden Kopiera, klicka på hello **kopiera data** panelen på hello startsida i din data factory.

![Guiden Kopiera](./media/data-factory-copy-wizard/copy-data-wizard.png)

## <a name="designed-for-big-data"></a>Utformad för stora data
Den här guiden kan du tooeasily flytta data från en mängd olika källor toodestinations i minuter. När du går igenom guiden hello skapas automatiskt en pipeline med en kopieringsaktiviteten, tillsammans med beroende Data Factory-enheter (länkade tjänster och datauppsättningar). Inga ytterligare steg är nödvändiga toocreate hello pipeline.   

![Välj datakälla](./media/data-factory-copy-wizard/select-data-source-page.png)

> [!NOTE]
> Stegvisa instruktioner toocreate en pipeline toocopy exempeldata från ett Azure blob tooan Azure SQL Database-tabellen finns hello [guiden Kopiera kursen](data-factory-copy-data-wizard-tutorial.md).
>
>

hello-guiden är utformad med stordata i åtanke från hello start med stöd för olika data och objekt av typen. Du kan skapa Data Factory pipelines som flyttar hundratals mappar, filer eller tabeller. hello guiden stöder automatisk förhandsgranskning, schemat avbildning och mappning och filtrering av data.

## <a name="automatic-data-preview"></a>Automatisk förhandsgranskning
Du kan förhandsgranska en del av hello data från hello valda datakälla i ordning toovalidate om hello data är vad du vill toocopy. Dessutom, om hello källdata finns i en textfil, Parsar hello guiden Kopiera hello text filen toolearn hello rad och kolumn avgränsare och schemat automatiskt.

![Inställningar för format](./media/data-factory-copy-wizard/file-format-settings.png)

## <a name="schema-capture-and-mapping"></a>Schemat avbildning och mappning
hello schemat för indata är inte matchar hello schemat för utdata i vissa fall. I det här scenariot måste toomap kolumner från hello källa schemat toocolumns från hello målschema.

> [!TIP]
> När stöd för kopiering av data från SQL Server eller Azure SQL-databas till Azure SQL Data Warehouse, om hello tabellen inte finns i hello målarkivet, Data Factory automatiskt skapande av tabell med datakällans schema. Mer information från [flytta data tooand från Azure SQL Data Warehouse med Azure Data Factory](./data-factory-azure-sql-data-warehouse-connector.md).
>

Använd listrutan-tooselect en kolumn från hello schemat toomap tooa källkolumnen i hello mål schemat. hello guiden Kopiera försöker toounderstand mönstret för kolumnmappningen. Det gäller hello samma mönster toohello resten av hello kolumner så att du inte behöver tooselect hello kolumner individuellt toocomplete hello schemamappning. Om du vill kan åsidosätta du dessa mappningar genom att använda hello listrutorna toomap hello kolumner i taget. hello mönster blir mer exakt du mappa fler kolumner. hello guiden Kopiera uppdaterar ofta hello mönster och slutligen når hello rätt mönster för hello kolumnen mappning av du vill tooachieve.     

![Schemamappning](./media/data-factory-copy-wizard/schema-mapping.png)

## <a name="filtering-data"></a>Filtrera data
Du kan filtrera data tooselect endast hello källdata som behöver toobe kopieras toohello sink-datalagret. Filtrering minskar hello mängden hello data toobe kopierade toohello sink data lagras och därför förbättrar hello genomflödet i hello kopieringen. Den innehåller ett flexibelt sätt toofilter data i en relationsdatabas med hello SQL-frågespråket eller filer i en Azure blob-mapp med hjälp av [Data Factory-funktioner och variabler](data-factory-functions-variables.md).   

### <a name="filtering-of-data-in-a-database"></a>Filtrering av data i en databas
hello följande skärmbild visar en SQL-fråga med hello `Text.Format` funktion och `WindowStart` variabeln.

![Validera uttryck](./media/data-factory-copy-wizard/validate-expressions.png)

### <a name="filtering-of-data-in-an-azure-blob-folder"></a>Filtrering av data i Azure blob-mappen
Du kan använda variabler i mappen hello sökvägen toocopy data från en mapp som ska fastställas vid körning baserat på [systemvariabler](data-factory-functions-variables.md#data-factory-system-variables). hello stöds variabler är: **{year}**, **{month}**, **{day}**, **{timme}**, **{minut}**, och **{anpassade}**. Till exempel: inputfolder / {year} / {month} / {day}.

Anta att du har definierat mappar i hello följande format:

    2016/03/01/01
    2016/03/01/02
    2016/03/01/03
    ...

Klicka på hello **Bläddra** knappen för **filen eller mappen**, bläddra tooone av dessa mappar (till exempel 2016 -> 03 -> 02-01 >), och klicka på **Välj**. Du bör se `2016/03/01/02` i hello textruta. Ersätt nu **2016** med **{year}**, **03** med **{month}**, **01** med **{day}** , och **02** med **{timme}**, och tryck på hello **fliken** nyckel. Du bör se listrutorna tooselect hello format för dessa fyra variabler:

![Med hjälp av systemvariabler](./media/data-factory-copy-wizard/blob-standard-variables-in-folder-path.png)   

I följande skärmbild hello visas kan du också använda en **anpassade** variabeln och eventuella [stöds formatsträngar](https://msdn.microsoft.com/library/8kb3ddd4.aspx). tooselect en mapp med denna struktur, Använd hello **Bläddra** knappen först. Ersätt ett värde med **{anpassade}**, och tryck på hello **fliken** viktiga toosee hello textruta där du kan ange hello formatsträng.     

![Med anpassad variabel](./media/data-factory-copy-wizard/blob-custom-variables-in-folder-path.png)

## <a name="scheduling-options"></a>Alternativ för schemaläggning
Du kan köra hello kopieringsåtgärden när eller enligt ett schema (varje timme, varje dag, och så vidare). Båda dessa alternativ kan användas för hello breda hello kopplingar mellan miljöer, inklusive lokalt, molnet och lokala skrivbord kopian.

Flytt av data från en källa tooa mål kan bara en gång i en enstaka kopieringsåtgärd. Det gäller toodata av valfri storlek och ett format som stöds. hello schemalagda kopia kan toocopy data på en föreskrivna upprepning. Du kan använda omfattande inställningar (t.ex. Försök igen, tidsgräns och varningar) tooconfigure hello schemalagda kopia.

![Egenskaper för schemaläggning](./media/data-factory-copy-wizard/scheduling-properties.png)

## <a name="next-steps"></a>Nästa steg
En snabb genomgång av med Kopieringsaktiviteten hello Data Factory kopiera guiden toocreate en pipeline finns [Självstudier: skapa en pipeline som använder hello guiden Kopiera](data-factory-copy-data-wizard-tutorial.md).
