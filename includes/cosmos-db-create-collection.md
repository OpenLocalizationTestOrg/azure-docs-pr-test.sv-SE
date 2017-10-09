Du kan nu använda hello Data Explorer verktyg i hello Azure portal toocreate en databas och samling. 

1. Klicka på hello Azure-portalen i hello vänstra navigeringsmenyn **Data Explorer (förhandsgranskning)**. 

2. På hello **Data Explorer (förhandsgranskning)** bladet, klickar du på **ny samling**, och sedan ange hello följande information:

    ![hello Azure portal Data Explorer-bladet](./media/cosmos-db-create-collection/azure-cosmosdb-data-explorer.png)

    Inställning|Föreslaget värde|Beskrivning
    ---|---|---
    Databas-id|Uppgifter|hello namn för den nya databasen. Databasnamn måste innehålla mellan 1 och 255 tecken och får inte innehålla /, \\, #, ? eller avslutande blanksteg.
    Samlings-id|Objekt|hello namn för den nya samlingen. Samlingsnamn på har hello samma tecken krav databas-ID: N.
    Lagringskapacitet| Fast (10 GB)|Använd hello standardvärdet. Det här värdet är hello lagringskapaciteten för hello-databasen.
    Dataflöde|400 RU|Använd hello standardvärdet. Om du vill tooreduce svarstid, kan du skala upp hello genomströmning senare.
    Partitionsnyckeln|/category|En partitionsnyckel som distribuerar data jämnt tooeach partition. Du väljer hello rätt Partitionsnyckeln är viktigt att skapa en performant-samling. Det finns fler toolearn [utformning för partitionering](../articles/cosmos-db/partition-data.md#designing-for-partitioning).    
3. När du har fyllt i formuläret hello, klickar du på **OK**.

Data Explorer visar hello ny databas och samling. 
