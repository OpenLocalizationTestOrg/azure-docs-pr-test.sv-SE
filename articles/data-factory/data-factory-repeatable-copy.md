---
title: aaaRepeatable kopian i Azure Data Factory | Microsoft Docs
description: "Lär dig hur tooavoid duplicerar även om en sektor som kopierar data körs mer än en gång."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jingwang
ms.openlocfilehash: 79a3fde2b700bf0a0e167479d6a86c5bee1bf7ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="repeatable-copy-in-azure-data-factory"></a>Repeterbara kopian i Azure Data Factory

## <a name="repeatable-read-from-relational-sources"></a>Upprepbar läsning från relationella källor
När du kopierar data från relationella datalager, Kom ihåg tooavoid repeterbarhet oönskade resultat. I Azure Data Factory, kan du köra en sektor manuellt. Du kan också konfigurera i principen för en dataset så att ett segment som körs när ett fel uppstår. När ett segment körs på antingen sätt måste toomake att som hello samma data läses oavsett hur många gånger ett segment körs.  
 
> [!NOTE]
> hello följande exempel är för Azure SQL men tillämpliga tooany datalager som har stöd för rektangulär datauppsättningar. Du kan ha tooadjust hello **typen** för källan och hello **frågan** egenskapen (till exempel: frågan i stället sqlReaderQuery) hello data lagras.   

Vanligtvis vid läsning från relationella lagrar vill du att tooread endast hello data motsvarande toothat sektorn. Ett sätt toodo så är med hjälp av hello WindowStart och WindowEnd systemvariabler tillgängliga i Azure Data Factory. Läs mer om hello variabler och funktioner i Azure Data Factory här i hello [Azure Data Factory - funktioner och systemvariabler](data-factory-functions-variables.md) artikel. Exempel: 

```json
"source": {
    "type": "SqlSource",
    "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm\\'', WindowStart, WindowEnd)"
},
```
Den här frågan läser data som finns i hello sektorn varaktighet intervallet (WindowStart -> WindowEnd) från hello tabellen mytable prefix. Kör i det här segmentet skulle alltid se till att hello samma data läses. 

I annat fall vilja tooread hello hela tabellen och kan definiera hello sqlReaderQuery enligt följande:

```json
"source": 
{            
    "type": "SqlSource",
    "sqlReaderQuery": "select * from MyTable"
},
```

## <a name="repeatable-write-toosqlsink"></a>Repeterbara skrivåtgärder tooSqlSink
När du kopierar data för**Azure SQL/SQL Server** från andra datalager måste tookeep repeterbarhet i åtanke tooavoid oönskade resultat. 

När du kopierar data tooAzure SQL/SQL Server-databasen läggs hello kopieringsaktiviteten datatabell toohello mottagare som standard. Säg, kopierar du data från en CSV-fil (fil med kommaavgränsade värden)-fil som innehåller två poster toohello i den följande tabellen i en Azure SQL/SQL Server-databas. När ett segment körs är hello två posterna kopierade toohello SQL-tabell. 

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
```

Du kanske fel har påträffats på källfilen och uppdatera hello antal ned röret från 2 too4. Om du manuellt köra hello datasektorn för denna period hittar du två nya poster läggs tooAzure SQL/SQL Server-databas. Det här exemplet förutsätter att ingen av hello kolumner i tabellen hello har hello primärnyckelns begränsning.

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

tooavoid problemet, du behöver toospecify UPSERT semantik med någon av följande två metoder hello:

### <a name="mechanism-1-using-sqlwritercleanupscript"></a>Metod 1: använder sqlWriterCleanupScript
Du kan använda hello **sqlWriterCleanupScript** egenskapen tooclean in data från hello sink tabell innan du infogar hello data när du kör ett segment. 

```json
"sink":  
{ 
  "type": "SqlSink", 
  "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM table WHERE ModifiedDate >= \\'{0:yyyy-MM-dd HH:mm}\\' AND ModifiedDate < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
}
```

När ett segment körs körs hello Rensa skript första toodelete data som motsvarar toohello segment från hello SQL-tabell. Hej kopieringsaktiviteten infogar data i hello SQL-tabell. Om hello sektorn körs hello kvantitet uppdateras enligt önskemål.

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

Anta att hello Flat bricka post tas bort från hello ursprungliga csv. Köra hello sektorn skulle sedan skapa hello följande resultat: 

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
7     Down Tube    4            2015-05-01 00:00:00
```

hello kopieringsaktiviteten kördes hello Rensa skriptet toodelete hello motsvarande data för den sektorn. Sedan den läsa hello indata från hello csv (som sedan finns bara en post) och infogas i hello tabell. 

### <a name="mechanism-2-using-sliceidentifiercolumnname"></a>Metod 2: använda sliceIdentifierColumnName
> [!IMPORTANT]
> SliceIdentifierColumnName stöds för närvarande inte för Azure SQL Data Warehouse. 

hello andra mekanism tooachieve repeterbarhet är genom att ha en dedikerad kolumn (sliceIdentifierColumnName) i hello mål tabell. Den här kolumnen kan användas av Azure Data Factory tooensure hello käll- och fortsätter att synkroniseras. Den här metoden fungerar när det finns flexibilitet för att ändra eller definiera hello mål SQL tabellens schema. 

Den här kolumnen används av Azure Data Factory för repeterbarhet och pågående hello Azure Data Factory inte göra några schemat ändras toohello tabell. Sätt toouse den här metoden:

1. Definiera en kolumn av typen **binary (32)** i hello målet SQL-tabell. Det bör finnas några begränsningar på den här kolumnen. Vi namn på den här kolumnen som AdfSliceIdentifier för det här exemplet.


    Källtabellen:

    ```sql
    CREATE TABLE [dbo].[Student](
       [Id] [varchar](32) NOT NULL,
       [Name] [nvarchar](256) NOT NULL
    )
    ```

    Måltabellen: 

    ```sql
    CREATE TABLE [dbo].[Student](
       [Id] [varchar](32) NOT NULL,
       [Name] [nvarchar](256) NOT NULL,
       [AdfSliceIdentifier] [binary](32) NULL
    )
    ```

2. Använd den i hello kopieringsaktiviteten enligt följande:
   
    ```json
    "sink":  
    { 
   
        "type": "SqlSink", 
        "sliceIdentifierColumnName": "AdfSliceIdentifier"
    }
    ```

Azure Data Factory fylls den här kolumnen enligt dess måste tooensure hello käll- och hålls synkroniserade. hello värdena i den här kolumnen ska inte användas utanför den här kontexten. 

Liknande toomechanism 1, Kopieringsaktiviteten rensas automatiskt hello data för hello angivna segment från hello målet SQL-tabell. Därefter infogas data från källan i toohello måltabellen. 

## <a name="next-steps"></a>Nästa steg
Granska hello efter anslutningen artiklar som slutförts JSON-exempel: 

- [Azure SQL Database](data-factory-azure-sql-connector.md)
- [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md)
- [SQL Server](data-factory-sqlserver-connector.md)