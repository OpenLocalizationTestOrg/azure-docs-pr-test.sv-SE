1. Ett nytt fönster och logga in toohello [Azure-portalen](https://portal.azure.com/).
2. Hello vänster klickar du på **ny**, klickar du på **databaser**, och sedan under **Azure Cosmos DB**, klickar du på **skapa**.
   
   ![Azure Portal databasfönstret](./media/cosmos-db-create-dbaccount-graph/create-nosql-db-databases-json-tutorial-1.png)

3. I hello **nytt konto** bladet anger hello-konfiguration som du vill använda för det här kontot i Azure Cosmos DB. 

    Med Azure Cosmos DB kan du välja någon av fyra programmeringsmodeller: Gremlin (graf), MongoDB, SQL (DocumentDB) och Tabell (nyckelvärde), som var och en kräver ett separat konto.
       
    I den här artikeln Snabbkurs, vi programmerar mot hello Graph API, så Välj **Gremlin (diagram)** som du anger i hello formulär. Om du har dokumentdata från en katalogapp, nyckelvärdedata (tabell) eller data som har migrerats från en MongoDB-app ska du tänka på att Azure Cosmos DB kan tillhandahålla en mycket tillgänglig, globalt distribuerad databastjänstplattform för alla dina verksamhetskritiska program.

    Hello fält på hello **nytt konto** bladet med hello information i hello följande skärmbild som guide - värdena kan skilja sig hello värden i hello skärmbild.
 
    ![hello nytt kontoblad för Azure Cosmos DB](./media/cosmos-db-create-dbaccount-graph/create-nosql-db-databases-json-tutorial-2.png)

    Inställning|Föreslaget värde|Beskrivning
    ---|---|---
    ID|*Unikt värde*|Ett unikt namn du väljer för att identifiera Azure Cosmos DB-kontot. Eftersom *documents.azure.com* är tillagda toohello ID som du anger toocreate URI: N, Använd en unik men identifierbar-ID. hello-ID måste innehålla endast små bokstäver, siffror och hello bindestreck (-) och det måste innehålla från 3 too50 tecken.
    API|Gremlin (graf)|Vi programmerar mot hello [Graph API](../articles/cosmos-db/graph-introduction.md) senare i den här artikeln.|
    Prenumeration|*Din prenumeration*|hello Azure-prenumeration som du vill toouse för det här kontot i Azure Cosmos DB. 
    Resursgrupp|*hello samma värde som ID*|hello namn på ny resursgrupp för ditt konto. För enkelhetens skull kan du använda hello samma namn som ditt ID. 
    Plats|*hello region närmaste tooyour användare*|Hej geografisk plats i vilka toohost Azure DB som Cosmos-konto. Välj hello plats närmaste tooyour användare toogive dem hello snabbast åtkomst toohello data.

4. Klicka på **skapa** toocreate hello-konto.
5. Hello på hello översta verktygsfältet **meddelanden** ikonen ![hello meddelandeikonen](./media/cosmos-db-create-dbaccount-graph/notification-icon.png) toomonitor hello distributionsprocessen.

    ![hello Azure portal meddelanden fönstret](./media/cosmos-db-create-dbaccount-graph/notification.png)

6.  När hello meddelanden visar hello distribution har slutförts, Stäng hello meddelandefönstret och öppna hello nytt konto från hello **alla resurser** panelen på hello instrumentpanelen. 

    ![DocumentDB-konto på hello panelen alla resurser](./media/cosmos-db-create-dbaccount-graph/azure-documentdb-all-resources.png)
