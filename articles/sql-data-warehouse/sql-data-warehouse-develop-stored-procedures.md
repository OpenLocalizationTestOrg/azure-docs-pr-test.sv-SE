---
title: aaaStored procedurer i SQL Data Warehouse | Microsoft Docs
description: "Tips för att använda lagrade procedurer i Azure SQL Data Warehouse för utveckling av lösningar."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: 9b238789-6efe-4820-bf77-5a5da2afa0e8
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: 416252dd3dea95c66aa5e886860b933b22578002
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="stored-procedures-in-sql-data-warehouse"></a>Lagrade procedurer i SQL Data Warehouse
SQL Data Warehouse stöder många av hello Transact-SQL-funktioner som finns i SQL Server. Det finns viktigare skala ut specifika funktioner som vi tooleverage toomaximize hello prestandan för din lösning.

Toomaintain hello skalning och prestanda för SQL Data Warehouse det är dock också vissa egenskaper och funktioner som har beteendebaserade skillnader och andra som inte stöds.

Den här artikeln förklarar hur tooimplement lagrade procedurer i SQL Data Warehouse.

## <a name="introducing-stored-procedures"></a>Introduktion till lagrade procedurer
En lagrad procedur är ett bra sätt för att kapsla in din SQL-kod. lagra det Stäng tooyour data i hello-datalagret. Genom att kapsla in hello koden i hanterbara enheter hjälper lagrade procedurer utvecklare modularize sina lösningar. underlätta större primärförpackningar i koden. Alla lagrade procedurer kan också ta emot parametrar toomake dem ännu mer flexibel.

SQL Data Warehouse ger en förenklad och effektiv lagrade proceduren implementering. hello största skillnaden jämfört med tooSQL servern som hello lagrade proceduren är inte före kompilerad kod. Vi är vanligtvis mindre berörda med hello Kompileringstid i datalager. Det är viktigare att hello lagrade proceduren koden är korrekt optimerad när mot stora datamängder. hello målet är toosave timmar, minuter och sekunder inte millisekunder. Därför är det mer praktiskt toothink lagrade procedurer som behållare för SQL-logiken.     

När SQL Data Warehouse kör är lagrad procedur hello SQL-uttryck parsas, översättas och optimerad vid körning. Under den här processen konvertera varje instruktion i distribuerade frågor. hello SQL-kod som faktiskt körs mot hello data är olika toohello fråga skickas.

## <a name="nesting-stored-procedures"></a>Kapsling lagrade procedurer
När lagrade procedurer anropa andra lagrade procedurer eller köra dynamisk sql hello inre lagrad procedur eller kod anrop sägs toobe kapslade.

SQL Data Warehouse stöder maximalt 8 kapslingsnivåer. Detta är något annorlunda tooSQL Server. hello kapsla nivå i SQL Server är 32.

hello lagrat proceduranrop med översta nivån är lika toonest nivå 1

```sql
EXEC prc_nesting
```
Om hello lagrade proceduren är också en annan EXEC anropa sedan detta ökar hello kapsla nivå too2

```sql
CREATE PROCEDURE prc_nesting
AS
EXEC prc_nesting_2  -- This call is nest level 2
GO
EXEC prc_nesting
```
Om hello andra proceduren utför vissa dynamisk sql sedan ökar detta hello kapsla nivå too3

```sql
CREATE PROCEDURE prc_nesting_2
AS
EXEC sp_executesql 'SELECT 'another nest level'  -- This call is nest level 2
GO
EXEC prc_nesting
```

Obs SQL Data Warehouse stöder för närvarande inte@NESTLEVEL. Du behöver tookeep reda på nest-nivå själv. Det är inte troligt träffar du hello 8 kapsla begränsningen men om du vill du kommer behöver toore fungerar koden och ”förenkla” den så att den passar i den här gränsen.

## <a name="insertexecute"></a>INSERT... KÖRA
SQL Data Warehouse tillåter inte att tooconsume hello resultatet av en lagrad procedur med en INSERT-instruktion. Det finns en annan metod som du kan använda.

Se följande artikel toohello [temporära tabeller] ett exempel på hur toodo detta.

## <a name="limitations"></a>Begränsningar
Det finns vissa aspekter av Transact-SQL-lagrade procedurer som inte har implementerats i SQL Data Warehouse.

De är:

* tillfälliga lagrade procedurer
* numrerade lagrade procedurer
* utökade lagrade procedurer
* CLR lagrade procedurer
* Krypteringsalternativ
* replikeringsalternativet
* tabellvärdeparametrar
* skrivskyddad parametrar
* standardparametrar
* körningen kontexter
* returnera instruktionen

## <a name="next-steps"></a>Nästa steg
För fler utvecklingstips, se [utvecklingsöversikt][development overview].

<!--Image references-->

<!--Article references-->
[temporära tabeller]: ./sql-data-warehouse-tables-temporary.md#modularizing-code
[development overview]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[nest level]: https://msdn.microsoft.com/library/ms187371.aspx

<!--Other Web references-->
