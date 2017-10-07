---
title: aaaTransactions i SQL Data Warehouse | Microsoft Docs
description: "Tips för transaktioner i Azure SQL Data Warehouse för utveckling av lösningar."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: ae621788-e575-41f5-8bfe-fa04dc4b0b53
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: 7c541648553238443b407666612561918096eb61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="transactions-in-sql-data-warehouse"></a>Transaktioner i SQL Data Warehouse
Som förväntat, stöder transaktioner som en del av hello arbetsbelastningen för informationslager i SQL Data Warehouse. Dock underhålls tooensure hello prestanda för SQL Data Warehouse i större skala vissa funktioner är begränsade när jämfört med tooSQL Server. Den här artikeln visar hello skillnader och visar hello andra. 

## <a name="transaction-isolation-levels"></a>Transaktionsisoleringsnivåer
SQL Data Warehouse implementerar ACID-transaktioner. Hello isolering av hello transaktionellt stöd är dock begränsad för`READ UNCOMMITTED` och detta kan inte ändras. Du kan implementera ett antal kodning metoder tooprevent dirty läser data om det här är ett problem för dig. hello populäraste metoder utnyttja CTAS och tabellen partition växling (ofta kallas hello glidande fönstret mönster) tooprevent användare från datafrågor som förbereds fortfarande. Vyer som före filtrera hello data är också en populär metod.  

## <a name="transaction-size"></a>Transaktionsstorlek
En enda data ändras transaktion har begränsad storlek. hello gränsen tillämpas idag ”per distribution”. Därför kan hello totala fördelningen beräknas genom att multiplicera hello gränsen med hello distribution count. tooapproximate hello maximalt antal rader i hello transaktion delar hello distribution cap med hello totala storleken på varje rad. Överväg att tar en genomsnittlig kolumnlängden i stället hello maxstorleken för kolumner med variabel längd.

Följande antaganden har gjorts i hello tabellen nedan hello:

* Det uppstod en jämn fördelning av data 
* hello genomsnittlig radlängden är 250 byte

| [DWU][DWU] | CAP per distribution (GiB) | Antal distributioner | Maxstorlek för transaktionen (GiB) | # Rader per distribution | Maximalt antal rader per transaktion |
| --- | --- | --- | --- | --- | --- |
| DW100 |1 |60 |60 |4,000,000 |240,000,000 |
| DW200 |1.5 |60 |90 |6,000,000 |360,000,000 |
| DW300 |2.25 |60 |135 |9,000,000 |540,000,000 |
| DW400 |3 |60 |180 |12,000,000 |720,000,000 |
| DW500 |3.75 |60 |225 |15,000,000 |900,000,000 |
| DW600 |4.5 |60 |270 |18,000,000 |1,080,000,000 |
| DW1000 |7.5 |60 |450 |30,000,000 |1,800,000,000 |
| DW1200 |9 |60 |540 |36,000,000 |2,160,000,000 |
| DW1500 |11.25 |60 |675 |45,000,000 |2,700,000,000 |
| DW2000 |15 |60 |900 |60,000,000 |3,600,000,000 |
| DW3000 |22.5 |60 |1,350 |90,000,000 |5,400,000,000 |
| DW6000 |45 |60 |2,700 |180,000,000 |10,800,000,000 |

storleksgräns för hello transaktion tillämpas per transaktion eller åtgärden. Det har inte tillämpats över alla samtidiga transaktioner. Varje transaktion är därför tillåts toowrite den här mängden data toohello loggen. 

toooptimize och minimera hello mängden data som skrivs toohello loggen finns toohello [transaktioner metodtips] [ Transactions best practices] artikel.

> [!WARNING]
> hello maximala transaktionsstorlek kan bara genomföras för HASH eller ROUND_ROBIN distribuerade tabeller där hello sprida sig på hello data är jämnt. Om hello transaktion skriver data i ett skeva sätt toohello distributioner är hello gränsen sannolikt toobe nått tidigare toohello transaktion maximala storlek.
> <!--REPLICATED_TABLE-->
> 
> 

## <a name="transaction-state"></a>Transaktionstillstånd
SQL Data Warehouse använder hello XACT_STATE() funktionen tooreport en misslyckad transaktion med värdet hello -2. Detta innebär att hello transaktionen misslyckades och har markerats för återställning endast

> [!NOTE]
> hello använda 2 genom hello XACT_STATE funktionen toodenote en misslyckade transaktionen representerar olika beteenden tooSQL Server. SQL Server använder hello värdet -1 toorepresent en som transaktion. SQL Server kan tolerera fel inuti en transaktion utan att den har markerats som som toobe. Till exempel `SELECT 1/0` skulle orsaka fel men inte att tvinga fram en transaktion till ett som tillstånd. SQL Server tillåter också läsningar i hello som transaktion. Dock kan SQL Data Warehouse du inte göra detta. Om ett fel uppstår i en transaktion i SQL Data Warehouse hello -2 tillstånd anges automatiskt och du kommer inte att kunna toomake Välj någon ytterligare instruktioner förrän hello-instruktionen har återställts. Det är därför viktigt toocheck att ditt program kod toosee om XACT_STATE() används som du kanske måste toomake kod ändringar.
> 
> 

Till exempel i SQL Server kan du se en transaktion som ser ut så här:

```sql
SET NOCOUNT ON;
DECLARE @xact_state smallint = 0;

BEGIN TRAN
    BEGIN TRY
        DECLARE @i INT;
        SET     @i = CONVERT(INT,'ABC');
    END TRY
    BEGIN CATCH
        SET @xact_state = XACT_STATE();

        SELECT  ERROR_NUMBER()    AS ErrNumber
        ,       ERROR_SEVERITY()  AS ErrSeverity
        ,       ERROR_STATE()     AS ErrState
        ,       ERROR_PROCEDURE() AS ErrProcedure
        ,       ERROR_MESSAGE()   AS ErrMessage
        ;

        IF @@TRANCOUNT > 0
        BEGIN
            PRINT 'ROLLBACK';
            ROLLBACK TRAN;
        END

    END CATCH;

IF @@TRANCOUNT >0
BEGIN
    PRINT 'COMMIT';
    COMMIT TRAN;
END

SELECT @xact_state AS TransactionState;
```

Om du lämnar din kod som ovan kommer du få hello följande felmeddelande:

Meddelande 111233, nivå 16, tillstånd 1, rad 1 111233; hello aktuella transaktionen har avbrutits och eventuella väntande ändringar har återställts. Orsak: En transaktion endast återställning har inte uttryckligen återställts innan DDL-, DML- eller SELECT-instruktion.

Du får också inte hello utdata från hello ERROR_ * funktioner.

I SQL Data Warehouse måste hello toobe något ändras:

```sql
SET NOCOUNT ON;
DECLARE @xact_state smallint = 0;

BEGIN TRAN
    BEGIN TRY
        DECLARE @i INT;
        SET     @i = CONVERT(INT,'ABC');
    END TRY
    BEGIN CATCH
        SET @xact_state = XACT_STATE();

        IF @@TRANCOUNT > 0
        BEGIN
            PRINT 'ROLLBACK';
            ROLLBACK TRAN;
        END

        SELECT  ERROR_NUMBER()    AS ErrNumber
        ,       ERROR_SEVERITY()  AS ErrSeverity
        ,       ERROR_STATE()     AS ErrState
        ,       ERROR_PROCEDURE() AS ErrProcedure
        ,       ERROR_MESSAGE()   AS ErrMessage
        ;
    END CATCH;

IF @@TRANCOUNT >0
BEGIN
    PRINT 'COMMIT';
    COMMIT TRAN;
END

SELECT @xact_state AS TransactionState;
```

hello förväntat beteende observeras nu. hello-fel i hello transaktion hanteras och hello ERROR_ * funktioner ange värden som förväntat.

Alla som har ändrats är den hello `ROLLBACK` av hello transaktionen hade toohappen innan hello läser av hello felinformation i hello `CATCH` block.

## <a name="errorline-function"></a>Error_Line() funktion
Det är också värt att nämna att SQL Data Warehouse inte implementera eller stöder hello ERROR_LINE() funktion. Om du har detta i din kod måste tooremove den toobe som är kompatibla med SQL Data Warehouse. Använd fråga etiketter i koden i stället tooimplement motsvarande funktioner. Se toohello [etikett] [ LABEL] mer information om den här funktionen.

## <a name="using-throw-and-raiserror"></a>Med hjälp av THROW och RAISERROR
THROW är hello mer modern implementering för att öka undantag i SQL Data Warehouse men stöds även RAISERROR. Det finns några skillnader som är värt uppmärksamhet toohowever.

* Användardefinierade felmeddelanden siffror inte får finnas i hello 150 100 000 000 intervallet för THROW
* RAISERROR felmeddelanden korrigeras på 50 000
* Användning av sys.messages stöds inte

## <a name="limitiations"></a>Limitiations
SQL Data Warehouse har några andra begränsningar som handlar om tootransactions.

De är följande:

* Inga distribuerade transaktioner
* Inga kapslade transaktioner tillåtna
* Spara punkter tillåts inte
* Inga namngivna transaktioner
* Inga markerade transaktioner
* Inget stöd för DDL som `CREATE TABLE` definieras inuti en transaktion

## <a name="next-steps"></a>Nästa steg
toolearn mer information om hur du optimerar transaktioner, se [transaktioner metodtips][Transactions best practices].  toolearn om andra Metodtips för SQL Data Warehouse, se [SQL Data Warehouse metodtips][SQL Data Warehouse best practices].

<!--Image references-->

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md
[development overview]: ./sql-data-warehouse-overview-develop.md
[Transactions best practices]: ./sql-data-warehouse-develop-best-practices-transactions.md
[SQL Data Warehouse best practices]: ./sql-data-warehouse-best-practices.md
[LABEL]: ./sql-data-warehouse-develop-label.md

<!--MSDN references-->

<!--Other Web references-->
