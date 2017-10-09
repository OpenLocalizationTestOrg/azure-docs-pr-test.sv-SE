---
title: "aaaIn minne OLTP förbättrar prestanda för SQL-txn | Microsoft Docs"
description: Anta Minnesintern OLTP tooimprove prestanda i en befintlig SQL-databas.
services: sql-database
documentationcenter: 
author: jodebrui
manager: jhubbard
editor: MightyPen
ms.assetid: c2bf14a0-905b-47b4-afb6-efe9a61147d5
ms.service: sql-database
ms.custom: develop databases
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/22/2016
ms.author: jodebrui
ms.openlocfilehash: 1bed796069565eb6741953837cf0477c2ddd8519
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-in-memory-oltp-tooimprove-your-application-performance-in-sql-database"></a>Använd Minnesintern OLTP tooimprove ditt programprestanda i SQL-databas
[Minnesintern OLTP](sql-database-in-memory.md) kan ha använt tooimprove hello prestanda för transaktionsbearbetning, datapåfyllning och tillfälligt datascenarier [Premium](sql-database-service-tiers.md) Azure SQL-databaser utan att öka hello prisnivå. 

> [!NOTE] 
> Lär dig hur [kvorum fördubblar viktiga databasen arbetsbelastning och sänka DTU med 70% med SQL-databas](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database)


Följ dessa steg tooadopt Minnesintern OLTP i den befintliga databasen.

## <a name="step-1-ensure-you-are-using-a-premium-database"></a>Steg 1: Kontrollera att du använder en Premium-databas
Minnesintern OLTP stöds endast i Premium-databaser. I minnet stöds om hello returnerade resultatet är 1 (inte 0):

```
SELECT DatabasePropertyEx(Db_Name(), 'IsXTPSupported');
```

*XTP* står för *extrema transaktionsbearbetning*



## <a name="step-2-identify-objects-toomigrate-tooin-memory-oltp"></a>Steg 2: Identifiera objekt toomigrate tooIn minnet OLTP
SSMS innehåller en **översikt över transaktion prestanda Analysis** rapport som du kan köra mot en databas med en aktiv arbetsbelastning. hello rapporten identifieras tabeller och lagrade procedurer som lämpar sig för migrering tooIn minne OLTP.

I SSMS toogenerate hello rapporten:

* I hello **Object Explorer**, högerklicka på databasnoden för din.
* Klicka på **rapporter** > **standardrapporter** > **översikt över transaktion prestanda Analysis**.

Mer information finns i [fastställa om en tabell eller en lagrad procedur ska flyttas tooIn minne OLTP](http://msdn.microsoft.com/library/dn205133.aspx).

## <a name="step-3-create-a-comparable-test-database"></a>Steg 3: Skapa en motsvarande testdatabas
Anta att hello rapporten visar att databasen har en tabell som har nytta som konverteras tooa minnesoptimerade tabeller. Vi rekommenderar att du först testar tooconfirm hello uppgift genom att testa.

Du behöver en test kopia av produktionsdatabasen. hello testdatabasen ska finnas på hello tjänsten samma nivå som produktionsdatabasen.

tooease testning, justera testa databasen på följande sätt:

1. Anslut toohello Testa databas med SSMS.
2. tooavoid behöver hello alternativet WITH (SNAPSHOT) i frågor genom att ange hello databasalternativet enligt hello följande T-SQL-instruktion:
   
   ```
   ALTER DATABASE CURRENT
    SET
        MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT = ON;
   ```

## <a name="step-4-migrate-tables"></a>Steg 4: Migrera tabeller
Du måste skapa och fylla i en minnesoptimerad kopia hello tabell som du vill tootest. Du kan skapa den med hjälp av antingen:

* hello praktiska minne optimering guiden i SSMS.
* Manuell T-SQL.

#### <a name="memory-optimization-wizard-in-ssms"></a>Guiden för optimering av minne i SSMS
toouse det här alternativet för migrering:

1. Ansluta toohello Testa databas med SSMS.
2. I hello **Object Explorer**, högerklicka på hello tabell och klicka sedan på **minne optimering Advisor**.
   
   * Hej **tabell minne optimering Advisor** guiden visas.
3. Klicka på hello guiden **migrering validering** (eller hello **nästa** knappen) toosee om hello tabell har någon stöds inte funktioner som inte stöds i minnesoptimerade tabeller. Mer information finns i:
   
   * Hej *minne optimering checklista* i [minne optimering Advisor](http://msdn.microsoft.com/library/dn284308.aspx).
   * [Transact-SQL-konstruktioner som inte stöds av Minnesintern OLTP](http://msdn.microsoft.com/library/dn246937.aspx).
   * [Migrera tooIn minne OLTP](http://msdn.microsoft.com/library/dn247639.aspx).
4. Om hello tabellen har inga funktioner som inte stöds, utföra hello advisor hello faktiska schemat och datamigrering åt dig.

#### <a name="manual-t-sql"></a>Manuell T-SQL
toouse det här alternativet för migrering:

1. Anslut tooyour Testa databas med SSMS (eller en liknande funktion).
2. Hämta hello fullständig T-SQL-skript för tabellen och dess index.
   
   * Högerklicka på noden tabell i SSMS.
   * Klicka på **skript tabell som** > **skapa till** > **nytt frågefönster**.
3. Lägga till WITH hello skriptfönstret (MEMORY_OPTIMIZED = ON) toohello CREATE TABLE-instruktion.
4. Om det finns ett KLUSTRADE index måste du ändra den tooNONCLUSTERED.
5. Byt namn på hello befintlig tabell med hjälp av SP_RENAME.
6. Skapa hello ny minnesoptimerade kopia av hello tabell genom att köra skriptet redigerade CREATE TABLE.
7. Kopiera hello tooyour minnesoptimerade tabellen med hjälp av INSERT... VÄLJ * TILL:

```
INSERT INTO <new_memory_optimized_table>
        SELECT * FROM <old_disk_based_table>;
```


## <a name="step-5-optional-migrate-stored-procedures"></a>Steg 5 (valfritt): migrera lagrade procedurer
hello InMemory-funktionen kan också ändra en lagrad procedur för bättre prestanda.

### <a name="considerations-with-natively-compiled-stored-procedures"></a>Överväganden med internt kompilerade lagrade procedurer
En internt kompilerad lagrad procedur måste ha följande alternativ på dess T-SQL WITH-satsen hello:

* NATIVE_COMPILATION
* SCHEMABINDING: betydelse tabeller som hello lagrade proceduren kan inte ha sina kolumndefinitionerna ändras på något sätt som påverkar hello lagrad procedur om du släppa hello lagrade proceduren.

En inbyggd modul måste använda en stor [ATOMIC-block](http://msdn.microsoft.com/library/dn452281.aspx) för hantering av transaktioner. Det finns ingen roll för en explicit BEGIN TRANSACTION eller ROLLBACK TRANSACTION. Om din kod upptäcker ett brott mot en affärsregel, det kan säga upp hello atomic-block med en [UTLÖSA](http://msdn.microsoft.com/library/ee677615.aspx) instruktionen.

### <a name="typical-create-procedure-for-natively-compiled"></a>Vanliga CREATE PROCEDURE för internt kompilerade
Hej T-SQL toocreate en internt kompilerad lagrad procedur är vanligtvis liknande toohello följande mall:

```
CREATE PROCEDURE schemaname.procedurename
    @param1 type1, …
    WITH NATIVE_COMPILATION, SCHEMABINDING
    AS
        BEGIN ATOMIC WITH
            (TRANSACTION ISOLATION LEVEL = SNAPSHOT,
            LANGUAGE = N'your_language__see_sys.languages'
            )
        …
        END;
```

* Hej TRANSACTION_ISOLATION_LEVEL är ÖGONBLICKSBILD hello vanligaste värde för hello internt kompilerade lagrade proceduren. Men en delmängd av hello andra värden stöds:
  
  * UPPREPBAR LÄSNING
  * SERIALISERBARA
* hello värdet LANGUAGE måste finnas i hello sys.languages vy.

### <a name="how-toomigrate-a-stored-procedure"></a>Hur toomigrate en lagrad procedur
hello migreringssteg är:

1. Hämta hello CREATE PROCEDURE skriptet toohello reguljära tolkad lagrad procedur.
2. Skriv om dess huvud toomatch hello tidigare mallen.
3. Fastställa om hello lagrade proceduren T-SQL-kod använder alla funktioner som inte stöds för internt kompilerade lagrade procedurer. Implementera lösningar om det behövs.
   
   * Mer information finns i [migreringsproblem för internt kompilerade lagrade procedurer](http://msdn.microsoft.com/library/dn296678.aspx).
4. Byt namn på hello gamla lagrade proceduren med hjälp av SP_RENAME. Eller helt enkelt.
5. Kör skriptet redigerade skapa proceduren T-SQL.

## <a name="step-6-run-your-workload-in-test"></a>Steg 6: Kör din arbetsbelastning i test
Köra en arbetsbelastning i testdatabasen är liknande toohello arbetsbelastning som körs i produktionsdatabasen. Detta bör visa hello prestandafördelar uppnås genom din användning av hello InMemory-funktionen för tabeller och lagrade procedurer.

Viktiga attribut för hello arbetsbelastning är:

* Antalet samtidiga anslutningar.
* Förhållandet mellan läsning och skrivning.

tootailor och kör hello test arbetsbelastning, Överväg att använda hello praktiska ostress.exe tool, som illustreras i [här](sql-database-in-memory.md).

toominimize Nätverksfördröjningen, kör testet i hello samma Azure geografiska region där hello databasen finns.

## <a name="step-7-post-implementation-monitoring"></a>Steg 7: Efter implementering övervakning
Överväg att övervaka hello prestanda effekterna av din InMemory-implementeringar i produktion:

* [Övervaka InMemory-lagringen](sql-database-in-memory-oltp-monitoring.md).
* [Övervaka Azure SQL Database med dynamiska hanteringsvyer](sql-database-monitoring-with-dmvs.md)

## <a name="related-links"></a>Relaterade länkar
* [OLTP (i minnet optimering) i minnet](http://msdn.microsoft.com/library/dn133186.aspx)
* [Introduktion tooNatively kompilerade lagrade procedurer](http://msdn.microsoft.com/library/dn133184.aspx)
* [Klassificering av minne optimering](http://msdn.microsoft.com/library/dn284308.aspx)

