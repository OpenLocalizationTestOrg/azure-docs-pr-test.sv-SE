---
title: aaaCopy data enkelt med guiden Kopiera - Azure | Microsoft Docs
description: "Läs mer om hur toouse hello guiden för Data Factory kopiera toocopy data från källor toosinks för data som stöds."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: f904972f-cd33-48db-9755-2b3196ae4168
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 99437ec16facf3b94c8be18487ec89e9f13acc9b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="copy-or-move-data-easily-with-azure-data-factory-copy-wizard"></a>Kopiera eller flytta data med guiden för Azure Data Factory kopiera
hello Azure Data Factory kopiera guiden handlar tooease hello vill föra in data, som vanligtvis är ett första steg i ett scenario för integrering av data för slutpunkt till slutpunkt. När du går igenom hello Azure Data Factory kopiera guiden behöver inte toounderstand en JSON-rolldefinitioner för länkade tjänster, datauppsättningar och rörledningar. Men när du har slutfört alla hello stegen i guiden hello hello guiden skapar automatiskt en pipeline toocopy data från hello valda datakällan toohello valda målet. Dessutom hello kopiera guiden hjälper dig att toovalidate hello data som inhämtas när hello redigering, vilket sparar mycket tid, särskilt när du mata in data för hello första gången från hello-datakälla. toostart hello guiden Kopiera, klicka på hello **kopiera data** panelen på hello startsida i din data factory.

![Guiden Kopiera](./media/data-factory-copy-wizard/copy-data-wizard.png)

## <a name="an-intuitive-wizard-for-copying-data"></a>En intuitiv guiden för att kopiera data
Den här guiden kan du tooeasily flytta data från en mängd olika källor toodestinations i minuter. Efter att gå igenom guiden hello skapas automatiskt en pipeline med en kopia-aktivitet du tillsammans med beroende Data Factory-enheter (länkade tjänster och datauppsättningar). Inga ytterligare steg är nödvändiga toocreate hello pipeline.   

![Välj datakälla](./media/data-factory-copy-wizard/select-data-source-page.png)

> [!NOTE]
> Se [guiden Kopiera kursen](data-factory-copy-data-wizard-tutorial.md) artikel för stegvisa instruktioner toocreate en pipeline toocopy exempeldata från ett Azure blob tooan Azure SQL Database-tabellen. 
> 
> 

hello-guiden är utformad med stordata i åtanke från hello start. Det är enkelt och effektivt tooauthor Data Factory pipelines som flyttar hundratals mappar, filer eller tabeller med hjälp av guiden för hello kopieringsdata. hello guiden stöder hello följande tre funktioner: automatisk förhandsgranskning, schemat avbilda mappning och att filtrera data. 

## <a name="automatic-data-preview"></a>Automatisk förhandsgranskning
hello kopiera guiden kan du tooreview del av hello data från hello markerad datakälla du toovalidate om hello data är det hello högerklickar du vill använda toocopy. Dessutom om hello källdata finns i en textfil, Parsar hello kopiera guiden hello text filen toolearn rad och kolumn avgränsare och schemat automatiskt. 

![Inställningar för format](./media/data-factory-copy-wizard/file-format-settings.png)

## <a name="schema-capture-and-mapping"></a>Schemat avbildning och mappning
hello schemat för indata är inte matchar hello schemat för utdata i vissa fall. I det här scenariot måste toomap kolumner från hello källa schemat toocolumns från hello målschema. 

guiden för hello kopiera mappar automatiskt kolumner i hello källa schemat toocolumns i hello mål schemat. Du kan åsidosätta hello mappningar genom att använda listrutorna hello (eller) om en kolumn måste toobe hoppas över medan hello data kopieras.   

![Schemamappning](./media/data-factory-copy-wizard/schema-mapping.png)

## <a name="filtering-data"></a>Filtrera data
hello-guiden kan du toofilter data tooselect endast hello källdata som behöver toobe kopieras toohello mål-sink-datalagret. Filtrering minskar hello mängden hello data toobe kopierade toohello sink data lagras och därför förbättrar hello genomflödet i hello kopieringen. Det ger ett flexibelt sätt toofilter data i en relationsdatabas med hjälp av SQL query language (eller) filer i en mapp i Azure blob [Data Factory-funktioner och variabler](data-factory-functions-variables.md).   

### <a name="filtering-of-data-in-a-database"></a>Filtrering av data i en databas
I exemplet hello hello SQL-frågan använder hello `Text.Format` funktion och `WindowStart` variabeln. 

![Validera uttryck](./media/data-factory-copy-wizard/validate-expressions.png)

### <a name="filtering-of-data-in-an-azure-blob-folder"></a>Filtrering av data i Azure blob-mappen
Du kan använda variabler i mappen hello sökvägen toocopy data från en mapp som ska fastställas vid körning baserat på [systemvariabler](data-factory-functions-variables.md#data-factory-system-variables). hello stöds variabler är: **{year}**, **{month}**, **{day}**, **{timme}**, **{minut}**, och **{anpassade}**. Exempel: inputfolder / {year} / {month} / {day}.

Anta att du har definierat mappar i hello följande format:

    2016/03/01/01
    2016/03/01/02
    2016/03/01/03
    ...

Klicka på hello **Bläddra** knappen för **filen eller mappen**, bläddra tooone av dessa mappar (till exempel 2016 -> 03 -> 02-01 >), och klicka på **Välj**. Du bör se `2016/03/01/02` i hello textruta. Ersätt nu **2016** med **{year}**, **03** med **{month}**, **01** med **{day}** , och **02** med **{timme}**, och tryck på Tab. Du bör se listrutorna tooselect hello format för dessa fyra variabler:

![Med hjälp av systemvariabler](./media/data-factory-copy-wizard/blob-standard-variables-in-folder-path.png)   

I följande skärmbild hello visas kan du också använda en **anpassade** variabeln och eventuella [stöds formatsträngar](https://msdn.microsoft.com/library/8kb3ddd4.aspx). tooselect en mapp med denna struktur, Använd hello **Bläddra** knappen först. Ersätt ett värde med **{anpassade}**, och tryck på fliken toosee hello textruta där du kan ange hello formatsträng.     

![Med anpassad variabel](./media/data-factory-copy-wizard/blob-custom-variables-in-folder-path.png)

## <a name="support-for-diverse-data-and-object-types"></a>Stöd för olika data och objekt av typen
Genom att använda hello guiden Kopiera kan flytta du effektivt hundratals mappar, filer eller tabeller.

![Välj tabeller från vilka toocopy data](./media/data-factory-copy-wizard/select-tables-to-copy-data.png)

## <a name="scheduling-options"></a>Alternativ för schemaläggning
Du kan köra hello kopieringsåtgärden när eller enligt ett schema (varje timme, varje dag, och så vidare). Båda dessa alternativ kan användas för hello breda hello kopplingar mellan lokala molnet och lokala skrivbord kopian.

Flytt av data från en källa tooa mål kan bara en gång i en enstaka kopieringsåtgärd. Det gäller toodata av valfri storlek och ett format som stöds. hello schemalagda kopia kan toocopy data på en föreskrivna upprepning. Du kan använda omfattande inställningar (t.ex. Försök igen, tidsgräns och varningar) tooconfigure hello schemalagda kopia.

![Egenskaper för schemaläggning](./media/data-factory-copy-wizard/scheduling-properties.png)

## <a name="next-steps"></a>Nästa steg
En snabb genomgång av med Kopieringsaktiviteten hello Data Factory kopiera guiden toocreate en pipeline finns [Självstudier: skapa en pipeline som använder hello guiden Kopiera](data-factory-copy-data-wizard-tutorial.md).

