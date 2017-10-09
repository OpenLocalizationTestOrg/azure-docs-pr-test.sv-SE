Du kan nu använda hello Data Explorer verktyget i hello Azure portal toocreate en graph-databas. 

1. Klicka på hello Azure-portalen i hello vänstra navigeringsmenyn **Data Explorer (förhandsgranskning)**. 
2. I hello **Data Explorer (förhandsgranskning)** bladet, klickar du på **nytt diagram**, fyll sedan i hello sida med hello följande information.

    ![Data Explorer i hello Azure-portalen](./media/cosmos-db-create-graph/azure-cosmosdb-data-explorer.png)

    Inställning|Föreslaget värde|Beskrivning
    ---|---|---
    Databas-id|sample-database|hello-ID för den nya databasen. Databasnamn måste innehålla mellan 1 och 255 tecken och får inte innehålla `/ \ # ?` eller avslutande blanksteg.
    Graf-id|sample-graph|hello-ID för nya diagrammet. Diagrammet namn har hello samma tecken krav som databas-ID: n.
    Lagringskapacitet| 10 GB|Lämna hello standardvärdet. Detta är hello lagringskapaciteten för hello-databasen.
    Dataflöde|400 RU:er|Lämna hello standardvärdet. Du kan skala upp hello genomströmning senare om du vill tooreduce svarstid.
    Partitionsnyckeln|/userid|En partitionsnyckel som ska distribuera data jämnt tooeach partition. Att välja rätt Partitionsnyckeln är viktigt att skapa en performant hello kurva Läs mer om den i [utformning för partitionering](../articles/cosmos-db/partition-data.md#designing-for-partitioning).

3. När hello formuläret, klickar du på **OK**.
