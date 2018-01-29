Nu kan du använda datautforskarverktyget i Azure Portal för att skapa en databas och samling. 

1. Klicka på **Datautforskaren** > **Ny samling**. 
    
    Området **Lägg till samling** visas längst till höger, du kan behöva bläddra åt höger för att se det.

    ![Datautforskaren i Azure-portalen, bladet Lägg till samling](./media/cosmos-db-create-collection/azure-cosmosdb-data-explorer.png)

2. På sidan **Lägg till samling** anger du inställningarna för den nya samlingen.

    Inställning|Föreslaget värde|Beskrivning
    ---|---|---
    Databas-id|Uppgifter|Ange *Uppgifter* som namn på den nya databasen. Databasnamn måste innehålla mellan 1 och 255 tecken och får inte innehålla /, \\, #, ? eller avslutande blanksteg.
    Samlings-id|Objekt|Ange *Objekt* som namnet på din nya samling. Samma teckenkrav gäller för samlings-ID:n som databasnamn.
    Lagringskapacitet| Fast (10 GB)|Ändra värdet till **Fast (10 GB)**. Det här värdet är databasens lagringskapacitet.
    Dataflöde|400 RU|Ändra genomflödet till 400 begäransenheter per sekund (RU/s). Lagringskapaciteten måste anges till **Fast (10 GB)** för att kunna ställa in dataflöde på 400 RU/s. Du kan skala upp dataflödet senare om du vill minska svarstiden. 
    
    Klicka på **OK**.

    Datautforskaren visar den nya databasen och samlingen.

    ![Datautforskaren i Azure-portalen visar den nya databasen och samlingen](./media/cosmos-db-create-collection/azure-cosmos-db-new-collection.png)