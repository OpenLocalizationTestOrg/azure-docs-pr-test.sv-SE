1. Ett nytt fönster och logga in toohello [Azure-portalen](https://portal.azure.com/).
2. Hello vänstra menyn klickar du på **ny**, klickar du på **databaser**, och sedan under **Azure Cosmos DB**, klickar du på **skapa**.
   
   ![Skärmbild som visar hello Azure-portalen, markera fler tjänster och Azure Cosmos DB](./media/cosmos-db-create-dbaccount-mongodb/create-nosql-db-databases-json-tutorial-1.png)

3. I hello **nytt konto** bladet anger hello önskad konfiguration för hello Azure DB som Cosmos-konto. 

    Med Azure Cosmos DB kan du välja någon av fyra programmeringsmodeller: Gremlin (graf), MongoDB, SQL (DocumentDB) och Tabell (nyckelvärde). 
       
    I den här snabbstartsguide vi kommer att programmera mot hello MongoDB API så att du ska välja **MongoDB** som du anger i hello formulär. Men om du har grafdata för en app för sociala medier, dokumentdata från en katalogapp eller nyckelvärdedata (tabell) ska du tänka på att Azure Cosmos DB kan tillhandahålla en mycket tillgänglig, globalt distribuerad databastjänstplattform för alla dina verksamhetskritiska program.

    Fyll i hello **nytt konto** blad med hjälp av hello information i hello tabellen som en vägledning.
 
    ![Skärmbild som visar hello nya Azure Cosmos-DB-bladet](./media/cosmos-db-create-dbaccount-mongodb/create-nosql-db-databases-json-tutorial-2.png)
   
    Inställning|Föreslaget värde|Beskrivning
    ---|---|---
    ID|*Unikt värde*|Ett unikt namn du tooidentify hello Azure DB som Cosmos-konto. *Documents.Azure.com* är tillagda toohello-ID du ange toocreate din URI, så Använd ett unikt men identifierbar-ID. hello-ID: T kan innehålla endast små bokstäver, siffror och hello '-' tecken och måste vara mellan 3 och 50 tecken.
    API|MongoDB|Vi kommer att programmera mot hello [MongoDB API](../articles/documentdb/documentdb-protocol-mongodb.md) senare i den här artikeln.|
    Prenumeration|*Din prenumeration*|hello Azure-prenumeration som du vill toouse för hello Azure DB som Cosmos-kontot. 
    Resursgrupp|*hello samma värde som ID*|hello namn på ny resursgrupp för ditt konto. För enkelhetens skull kan du använda hello samma namn som ditt ID. 
    Plats|*hello region närmaste tooyour användare*|Hej geografisk plats i vilka toohost Azure DB som Cosmos-konto. Välj hello plats närmaste tooyour användare toogive dem hello snabbast åtkomst toohello data.

4. Klicka på **skapa** toocreate hello-konto.
5. På verktygsfältet hello **meddelanden** toomonitor hello distributionsprocessen.

    ![Meddelande om att distributionen har påbörjats](./media/cosmos-db-create-dbaccount-mongodb/azure-documentdb-nosql-notification.png)

6.  När hello distributionen är klar, öppna hello nytt konto från hello alla resurser panelen. 

    ![DocumentDB-konto på hello panelen alla resurser](./media/cosmos-db-create-dbaccount-mongodb/azure-documentdb-all-resources.png)
