---
title: aaaMigrate ditt schema tooSQL Data Warehouse | Microsoft Docs
description: "Tips för att migrera din schemat tooAzure SQL Data Warehouse för utveckling av lösningar."
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: 538b60c9-a07f-49bf-9ea3-1082ed6699fb
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: migrate
ms.date: 10/31/2016
ms.author: joeyong;barbkess
ms.openlocfilehash: 1309b743b78564575695038a4856d9d25a2b18d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-your-schemas-toosql-data-warehouse"></a>Migrera din scheman tooSQL Data Warehouse
Riktlinjer för att migrera dina SQL-scheman tooSQL Data Warehouse. 

## <a name="plan-your-schema-migration"></a>Planera migrering av schemat

När du planerar en migrering finns hello [tabell översikt] [ table overview] toobecome bekant med tabellen designöverväganden, till exempel statistik, distribution, partitionering och indexering.  Den listar också några [stöds inte i tabellen funktioner] [ unsupported table features] och sina lösningar.

## <a name="use-user-defined-schemas-tooconsolidate-databases"></a>Använda anpassade scheman tooconsolidate databaser

Din befintliga arbetsbelastning har antagligen mer än en databas. Ett informationslager i SQL Server kan exempelvis innehålla en fristående databas, en databas för datalager och vissa data mart-databaser. I den här topologin körs varje databas som en separat arbetsbelastning med separata säkerhetsprinciper.

Däremot kör SQL Data Warehouse hello hela arbetsbelastningen för informationslager inom en databas. Mellan databasen tillåts inte kopplingar. SQL Data Warehouse förväntar därför alla tabeller som används av hello data warehouse toobe lagras i en hello-databas.

Vi rekommenderar att du använder användardefinierade scheman tooconsolidate din befintliga arbetsbelastning i en databas. Exempel finns [användardefinierade scheman](sql-data-warehouse-develop-user-defined-schemas.md)

## <a name="use-compatible-data-types"></a>Använd kompatibla datatyper
Ändra dina data typer toobe kompatibel med SQL Data Warehouse. En lista över datatyper som stöds respektive inte stöds, se [datatyper][data types]. Avsnittet ger lösningar för typer hello stöds inte. Det ger också en fråga tooidentify befintliga typer som inte stöds i SQL Data Warehouse.

## <a name="minimize-row-size"></a>Minimera Radstorleken
Minimera hello radlängden tabeller för bästa prestanda. Eftersom kortare raden längder leda toobetter prestanda, kan du använda hello minsta datatyper som fungerar för dina data. 

Tabellens rad bredd har PolyBase en gräns på 1 MB.  Om du planerar tooload data till SQL Data Warehouse med PolyBase uppdatera dina tabeller toohave största raden bredden på mindre än 1 MB. 

<!--
- For example, this table uses variable length data but hello largest possible size of hello row is still less than 1 MB. PolyBase will load data into this table.

- This table uses variable length data and hello defined row width is less than one MB. When loading rows, PolyBase allocates hello full length of hello variable-length data. hello full length of this row is greater than one MB.  PolyBase will not load data into this table.  

-->

## <a name="specify-hello-distribution-option"></a>Ange hello distributionsalternativ
SQL Data Warehouse är en distribuerad databas. Varje tabell distribueras eller replikeras över hello Compute-noder. Det finns ett Tabellalternativ som du kan ange hur toodistribute hello data. hello alternativ är resursallokering, replikeras, eller hash distribueras. Varje har- och nackdelar. Om du inte anger hello distributionsalternativ använder SQL Data Warehouse resursallokering som hello standard.

- Resursallokering är hello som standard. Är det enklaste hello-toouse och läser in hello data så snabbt som möjligt, men kopplingar kräver dataflyttning som minskar frågeprestanda.
- Replikerade lagrar en kopia av hello tabellen på varje beräkningsnod. Replikerade tabeller är performant eftersom de inte kräver dataflyttning för kopplingar och aggregeringar. De kräver extra lagring och därför fungerar bäst för mindre tabeller.
- Hash distribuerade distribuerar hello rader för alla hello noder via en hash-funktionen. Distribuerade hash-tabeller är hello hjärtat i SQL Data Warehouse eftersom de är utformad tooprovide hög frågeprestanda för stora tabeller. Det här alternativet kräver vissa planering tooselect hello bästa kolumn på vilka toodistribute hello data. Men om du inte väljer hello bästa kolumnen hello första gången, kan du enkelt nytt distribuera hello data på en annan kolumn. 

toochoose hello bästa distributionsalternativ för varje tabell finns [distribuerade tabeller](sql-data-warehouse-tables-distribute.md).


## <a name="next-steps"></a>Nästa steg
När du har migrerat dina databasen schemat tooSQL datalagret fortsätta tooone av hello följande artiklar:

* [Migrera dina data][Migrate your data]
* [Migrera koden][Migrate your code]

Mer information om metodtips för SQL Data Warehouse finns hello [metodtips] [ best practices] artikel.

<!--Image references-->

<!--Article references-->
[Migrate your code]: ./sql-data-warehouse-migrate-code.md
[Migrate your data]: ./sql-data-warehouse-migrate-data.md
[best practices]: ./sql-data-warehouse-best-practices.md
[table overview]: ./sql-data-warehouse-tables-overview.md
[unsupported table features]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[data types]: ./sql-data-warehouse-tables-data-types.md
[unsupported data types]: ./sql-data-warehouse-tables-data-types.md#unsupported-data-types

<!--MSDN references-->


<!--Other Web references-->
