## <a name="repeatability-during-copy"></a>Repeterbarhet vid kopiering
När kopieringen data tooAzure SQL/SQL Server från andra data lagras en behov tookeep repeterbarhet i åtanke tooavoid oönskade resultat. 

När du kopierar data tooAzure SQL/SQL Server-databas kommer kopieringsaktiviteten av APPEND hello datauppsättning toohello sink Standardtabell som standard. När du kopierar data från en CSV-fil (fil med kommaavgränsade värden) filen datakälla som innehåller två registrerar tooAzure SQL/SQL Server-databas, är det till exempel vilka hello tabellen ser ut som:

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
```

Anta att du fel har påträffats på källfilen och uppdaterade hello mängd ned röret från 2 too4 i hello källfil. Om du kör nytt hello datasektorn för denna period, hittar du två nya poster läggs tooAzure SQL/SQL Server-databas. hello nedan förutsätter att ingen av hello kolumner i tabellen hello har hello primärnyckelns begränsning.

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

tooavoid detta, behöver du toospecify UPSERT semantik genom att använda en av hello nedan 2 mekanismer som anges nedan.

> [!NOTE]
> En sektor går att köra automatiskt i Azure Data Factory enligt hello återförsökspolicyn som anges.
> 
> 

### <a name="mechanism-1"></a>Mekanism 1
Du kan utnyttja **sqlWriterCleanupScript** egenskapen toofirst utföra rensning av åtgärden när du kör ett segment. 

```json
"sink":  
{ 
  "type": "SqlSink", 
  "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM table WHERE ModifiedDate >= \\'{0:yyyy-MM-dd HH:mm}\\' AND ModifiedDate < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
}
```

hello Rensa skriptet ska köras första vid kopiering av för ett visst segment som skulle ta bort hello data från hello SQL-tabell motsvarande toothat sektorn. hello aktivitet kommer därefter infoga hello data i hello SQL-tabell. 

Om hello sektorn är nu kör igen och du hittar hello kvantitet uppdateras som önskade.

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

Anta att hello Flat bricka post tas bort från hello ursprungliga csv. Nytt körs hello sektorn skulle sedan skapa hello följande resultat: 

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
7     Down Tube    4            2015-05-01 00:00:00
```
Inget nytt hade toobe klar. hello kopieringsaktiviteten kördes hello Rensa skriptet toodelete hello motsvarande data för den sektorn. Sedan den läsa hello indata från hello csv (som sedan finns endast 1 post) och infogas i hello tabell. 

### <a name="mechanism-2"></a>Metod 2
> [!IMPORTANT]
> sliceIdentifierColumnName stöds inte för Azure SQL Data Warehouse just nu. 

En annan mekanism tooachieve repeterbarhet av är att ha en dedikerad kolumn (**sliceIdentifierColumnName**) på hello tabellen som mål. Den här kolumnen kan användas av Azure Data Factory tooensure hello käll- och fortsätter att synkroniseras. Den här metoden fungerar när det finns flexibilitet för att ändra eller definiera hello mål SQL tabellens schema. 

Den här kolumnen som ska användas av Azure Data Factory för repeterbarhet och pågående hello Azure Data Factory kommer inte att genomföra alla scheman ändras toohello tabell. Sätt toouse den här metoden:

1. Definiera en kolumn av typen binary (32) i hello målet SQL-tabell. Det bör finnas några begränsningar på den här kolumnen. Vi namn i den här kolumnen som 'ColumnForADFuseOnly' för det här exemplet.
2. Använd den i hello kopieringsaktiviteten enligt följande:
   
    ```json
    "sink":  
    { 
   
        "type": "SqlSink", 
        "sliceIdentifierColumnName": "ColumnForADFuseOnly"
    }
    ```

Azure Data Factory kommer att fylla i den här kolumnen enligt dess måste tooensure hello käll- och hålls synkroniserade. hello värdena i den här kolumnen får inte användas utanför den här kontexten av hello användare. 

Liknande toomechanism 1, Kopieringsaktiviteten kommer automatiskt första Rensa hello data för hello angivna segment från hello målet SQL-tabellen och kör sedan hello kopieringsaktiviteten normalt tooinsert hello data från källan toodestination för den sektorn. 

