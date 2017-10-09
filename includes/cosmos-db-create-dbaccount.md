1. Ett nytt fönster och logga in toohello [Azure-portalen](https://portal.azure.com/).
2. Hello vänster klickar du på **ny**, klickar du på **databaser**, och sedan under **Azure Cosmos DB**, klickar du på **skapa**.
   
   ![hello Azure portal databaser fönstret](./media/cosmos-db-create-dbaccount/create-nosql-db-databases-json-tutorial-1.png)

3. På hello **nytt konto** bladet anger hello-konfiguration som du vill använda för det här kontot i Azure Cosmos DB. 

    Med Azure Cosmos DB kan du välja någon av fyra programmeringsmodeller: Gremlin (graf), MongoDB, SQL (DocumentDB) och Tabell (nyckelvärde), som var och en kräver ett separat konto.
    
    I den här Snabbkurs artikeln vi programmerar mot hello DocumentDB API, så Välj **SQL (DocumentDB)** som du anger i hello formulär. Om du har grafdata för en app för sociala medier, nyckel/värde-data (tabell) eller data som har migrerats från en MongoDB-app, ska du tänka på att Azure Cosmos DB kan vara en mycket tillgänglig och globalt distribuerad plattform för databastjänster i alla dina verksamhetskritiska program.

    Hello fält på hello **nytt konto** blad, med hjälp av hello informationen i följande skärmbild som guide - hello värdena kan skilja sig hello värden i hello skärmbild.
 
    ![hello nytt kontoblad för Azure Cosmos DB](./media/cosmos-db-create-dbaccount/create-nosql-db-databases-json-tutorial-2.png)

    Inställning|Föreslaget värde|Beskrivning
    ---|---|---
    ID|*Unikt värde*|Ett unikt namn du väljer för att identifiera Azure Cosmos DB-kontot. Eftersom *documents.azure.com* är tillagda toohello ID som du anger toocreate URI: N, Använd en unik men identifierbar-ID. hello-ID: T kan innehålla endast små bokstäver, siffror och hello bindestreck (-) och får innehålla 3 too50 tecken.
    API|SQL (DocumentDB)|Vi programmerar mot hello [DocumentDB API](../articles/documentdb/documentdb-introduction.md) senare i den här artikeln.|
    Prenumeration|*Din prenumeration*|hello Azure-prenumeration som du vill toouse för det här kontot i Azure Cosmos DB. 
    Resursgrupp|*hello samma värde som ID*|hello ny resursgrupp namn för ditt konto. För enkelhetens skull kan du använda hello samma namn som ditt ID. 
    Plats|*hello region närmaste tooyour användare*|Hej geografisk plats i vilka toohost Azure DB som Cosmos-konto. Välj hello-plats som är närmast tooyour användare toogive dem hello snabbast åtkomst toohello data.
4. Klicka på **skapa** toocreate hello-konto.
5. Hello på hello översta verktygsfältet **meddelanden** ikonen ![hello meddelandeikonen](./media/cosmos-db-create-dbaccount/notification-icon.png) toomonitor hello distributionsprocessen.

    ![hello Azure portal meddelanden fönstret](./media/cosmos-db-create-dbaccount-graph/azure-documentdb-nosql-notification.png)

6.  När hello meddelanden visar hello distribution har slutförts, Stäng hello meddelandefönstret och öppna hello nytt konto från hello **alla resurser** panelen på hello instrumentpanelen. 

    ![Hej DocumentDB-konto på hello panelen alla resurser](./media/cosmos-db-create-dbaccount/all-resources.png)
 
